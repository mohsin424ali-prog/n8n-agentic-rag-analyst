# System Architecture

## Overview

This is an **Enhanced Agentic RAG** (Retrieval-Augmented Generation) system built on n8n with four interconnected workflows forming a complete document analysis pipeline.

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER LAYER                              │
└─────────────────────────────────────────────────────────────────┘
         │                                      │
         │ Upload PDFs                          │ Ask Questions
         ▼                                      ▼
┌──────────────────┐                  ┌──────────────────┐
│  Workflow 1:     │                  │  Workflow 4:     │
│  Upload Portal   │                  │  Query Agent     │
│  (Web UI)        │                  │  (Chat UI)       │
└────────┬─────────┘                  └────────┬─────────┘
         │                                      │
         │ Saves to                             │ Orchestrates
         │ /files/                              │ 3 Tools
         │                                      │
         ▼                                      ▼
┌──────────────────┐            ┌────────────────────────┐
│  Workflow 2:     │            │   Tool Selection       │
│  Ingestion       │            │   (Intelligent Router) │
│  Pipeline        │            └───┬────────┬───────┬───┘
│  (Background)    │                │        │       │
└────────┬─────────┘                │        │       │
         │                           ▼        ▼       ▼
         │                      ┌────────┐ ┌───┐ ┌──────┐
         │                      │Vector  │ │WF3│ │ Web  │
         │                      │Search  │ │   │ │Search│
         │                      └────────┘ └───┘ └──────┘
         │
         ▼
┌──────────────────────────────────────────────────────────┐
│              PROCESSING LAYER                            │
│                                                          │
│  PDF → Text → Chunks → Embeddings → Qdrant             │
│   │      │       │         │            │               │
│   │      │       │         │            └─ Vector Store │
│   │      │       │         └─ Ollama (Local)           │
│   │      │       └─ Text Splitter (2500/250)           │
│   │      └─ Extract from File                           │
│   └─ File Trigger                                       │
└──────────────────────────────────────────────────────────┘
         │
         ▼
┌──────────────────────────────────────────────────────────┐
│              STORAGE LAYER                               │
│                                                          │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────────┐  │
│  │  /files/    │  │  /processed/ │  │  Qdrant DB    │  │
│  │  (Staging)  │  │  (Archive)   │  │  (Vectors)    │  │
│  └─────────────┘  └──────────────┘  └───────────────┘  │
└──────────────────────────────────────────────────────────┘
```

## Workflow Details

### Workflow 1: Document Upload Portal

**Type**: Webhook-triggered web interface  
**Trigger**: HTTP GET request  
**Function**: User-facing document submission

**Flow**:
1. User accesses `/webhook/upload-documents`
2. Beautiful HTML form rendered with drag-and-drop
3. File validation (PDF only, max 50MB)
4. POST to `/webhook/upload-documents-process`
5. File saved to `/files/` directory
6. Success/error response returned

**Output**: PDF file in `/files/` directory

### Workflow 2: Ingestion Pipeline

**Type**: File system trigger (automated)  
**Trigger**: New file detected in `/files/`  
**Function**: Automated document vectorization

**Flow**:
1. Local File Trigger monitors `/files/`
2. Start Timer (performance tracking)
3. IF (PDF Only) validation
4. Read Files from Disk
5. Extract from File (PDF → Text)
6. Text Splitter (2500 chars, 250 overlap)
7. Ollama Embeddings (nomic-embed-text)
8. Qdrant Vector Store (insert)
9. Move to Processed folder
10. Success/Error logging

**Metrics Tracked**:
- Processing time (ms)
- File size (bytes)
- Chunks created
- Success/failure status

**Output**: Vectorized chunks in Qdrant

### Workflow 3: Get Document Metadata

**Type**: HTTP API endpoint  
**Trigger**: HTTP GET request  
**Function**: Collection statistics service

**Response Schema**:
```json
{
  "success": true,
  "totalChunks": 1250,
  "totalVectors": 1250,
  "uniqueDocuments": 15,
  "documents": ["file1.pdf", "file2.pdf"]
}
```

### Workflow 4: Query Agent

**Type**: Chat interface with AI agent  
**Trigger**: WebSocket chat message  
**Function**: Intelligent document querying

**Components**:

1. **Query Optimizer** - Removes filler words
2. **AI Agent** - Multi-tool orchestration
3. **Tool 1: Company_Knowledge_Base** - Vector search
4. **Tool 2: Document_Metadata_Tool** - Statistics
5. **Tool 3: Live_Web_Search** - Internet fallback
6. **Memory** - Conversation context
7. **LLM** - Groq Llama 3.3 70B

**Security Layer**: Detects prompt injection attempts

## Data Flow

### Upload → Vectorization Flow

```
User uploads PDF
     ↓
[Upload Portal] Saves to /files/document.pdf
     ↓
[File Trigger] Detects new file
     ↓
[Ingestion] Reads PDF → "This is document text..."
     ↓
[Text Splitter] Creates chunks (2500 chars each)
     ↓
[Ollama] Generates embeddings (768 dimensions)
     ↓
[Qdrant] Stores vectors with metadata
     ↓
[Archive] Moves to /files/processed/
```

### Query → Response Flow

```
User asks: "What is the vacation policy?"
     ↓
[Query Optimizer] Extracts: "vacation policy"
     ↓
[AI Agent] Analyzes intent → "document content query"
     ↓
[Tool Selection] Chooses: Company_Knowledge_Base
     ↓
[Vector Search] Returns top 8 relevant chunks
     ↓
[LLM Synthesis] Generates answer with citations
     ↓
User receives answer
```

## Technology Stack

- **Orchestration**: n8n (self-hosted)
- **Vector Database**: Qdrant (self-hosted)
- **Embedding Model**: Ollama nomic-embed-text (local)
- **LLM**: Groq Llama 3.3 70B (API)
- **Web Search**: SerpAPI (optional)

## Scalability

### Current Limitations
- Single-instance n8n server
- File-based triggers
- Sequential processing

### Scaling Options
- Multiple n8n instances with shared Qdrant
- Load balancer for webhooks
- Distributed file storage (S3, MinIO)
- Batch processing optimization

## Security Architecture

### Defense Layers

1. **Input Validation**: File type, size limits
2. **AI Security**: Prompt injection detection
3. **Network Security**: Internal Qdrant communication
4. **Data Privacy**: Local embeddings, self-hosted storage

## Performance

- Processes PDFs in 5-10 seconds
- Vector search latency < 500ms
- Handles documents up to 50MB
- Scales to 1000+ documents

---

**Last Updated**: January 2026  
**Version**: 1.0.0
