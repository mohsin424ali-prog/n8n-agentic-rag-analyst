# 🤖 Agentic RAG Document Analyst

An **Enhanced Agentic RAG** (Retrieval-Augmented Generation) system built with n8n, featuring intelligent document analysis, multi-tool agent orchestration, and privacy-focused local embeddings.

> **Perfect for**: Compliance teams, HR departments, and organizations needing secure, authoritative knowledge retrieval from internal documents with full source attribution.

## 🌟 System Overview

This is **not** a basic RAG implementation - it's a production-ready **Agentic RAG MVP+** with advanced features:

### Core RAG Features ✅
- Automated document ingestion pipeline
- Vector embeddings (local via Ollama)
- Semantic search with Qdrant
- LLM-powered intelligent responses
- Comprehensive source citations

### Enhanced Agentic Features ✅
- **Multi-tool agent** with intelligent tool selection
- **Hybrid search**: Internal documents + live web search fallback
- **Metadata service** for collection insights
- **Query optimization** preprocessing
- **Conversation memory** for context retention
- **Security hardening** against prompt injection attacks

### Production-Ready Additions ✅
- User-friendly drag-and-drop upload interface
- Comprehensive error handling & logging
- **Performance metrics tracking**
- Automatic file organization
- Professional UI/UX design

## 🏗️ System Architecture

```
┌─────────────────┐
│  User uploads   │
│      PDF        │
└────────┬────────┘
         │
         ▼
┌─────────────────────┐
│ Upload Portal       │ ◄── Workflow 1: Entry Point
│ (Web Interface)     │
└────────┬────────────┘
         │ Saves to /files/
         ▼
┌─────────────────────┐
│ Ingestion Pipeline  │ ◄── Workflow 2: Background Processing
│ • File Monitor      │
│ • PDF → Text        │
│ • Text → Chunks     │
│ • Ollama Embeddings │
│ • Qdrant Storage    │
└────────┬────────────┘
         │
         ▼
    ┌───────────┐
    │  Qdrant   │ ◄── Vector Database
    │  Vectors  │
    └─────┬─────┘
          │
          ▼
┌──────────────────────┐
│ Query Agent          │ ◄── Workflow 4: AI Interface
│ ┌──────────────────┐ │
│ │ Tool 1: Vector   │ │
│ │ Search (Qdrant)  │ │
│ └──────────────────┘ │
│ ┌──────────────────┐ │
│ │ Tool 2: Metadata │ │ ◄── Workflow 3: Stats Service
│ │ Service          │ │
│ └──────────────────┘ │
│ ┌──────────────────┐ │
│ │ Tool 3: Web      │ │
│ │ Search (SerpAPI) │ │
│ └──────────────────┘ │
│                      │
│ + Security Layer     │
│ + Memory             │
│ + LLM (Groq)         │
└──────────────────────┘
         │
         ▼
    User Answer
    with Citations
```

## 🔗 Workflow Interconnections

### 1. **Document Upload Portal** (Entry Point)
- Beautiful drag-and-drop web interface
- PDF validation (max 50MB)
- Saves to `/files/` directory
- Real-time upload feedback

**Output**: Triggers automatic processing

### 2. **Ingestion Pipeline** (Automated Background Processing)
- Monitors `/files/` folder via Local File Trigger
- Extracts text from PDFs
- Chunks documents (2,500 chars, 250 overlap)
- Generates embeddings using **Ollama nomic-embed-text** (local)
- Stores vectors in **Qdrant**
- Tracks metrics: processing time, chunks, success rates
- Moves processed files to `/files/processed/`

**Output**: Vectorized document chunks in Qdrant

### 3. **Get Document Metadata** (Supporting Service)
- Queries Qdrant collection statistics
- Lists all unique documents
- Returns inventory data
- Used by Query Agent's metadata tool

**Output**: JSON with document stats

### 4. **Query Agent** (AI-Powered Chat Interface)
**Query Optimization**: Removes filler words, extracts keywords

**Multi-Tool Agent** with 3 specialized tools:
1. **Company_Knowledge_Base**: Searches document content (Qdrant)
2. **Document_Metadata_Tool**: Fetches collection stats
3. **Live_Web_Search**: Internet fallback (SerpAPI)

**Security Layer**: Detects/blocks prompt injection

**Memory**: Maintains conversation context

**LLM**: Groq Llama 3.3 70B

**Output**: Intelligent answers with proper citations

## 🔒 Privacy & Data Sovereignty

### ✅ What's Local (Privacy-Focused)
- **Embeddings**: Ollama (runs locally)
- **File Storage**: Local filesystem
- **Vector DB**: Qdrant (self-hosted)
- **Orchestration**: n8n (self-hosted)

**Privacy Level**: 80% local

### ⚠️ External Dependencies
- **LLM**: Groq API (Llama 3.3 70B) - cloud-based
- **Web Search**: SerpAPI - cloud service (optional)

### 🔐 Achieve 100% Privacy
To make this **fully private**:
1. Replace Groq → **Ollama** (`llama3.3:70b` locally)
2. Remove SerpAPI or use self-hosted search

**Result**: 95-100% local operation

## 📦 Installation

### Prerequisites
- **n8n** v1.0.0+ (self-hosted)
- **Qdrant** v1.7.0+ (vector database)
- **Ollama** with `nomic-embed-text` model
- **Groq API** key (or local Ollama LLM)
- **SerpAPI** key (optional, for web search)
- 4GB+ RAM recommended

### Quick Start

1. **Clone Repository**
```bash
git clone https://github.com/YOUR_USERNAME/n8n-agentic-rag-analyst.git
cd n8n-agentic-rag-analyst
```

2. **Import Workflows** (in order)
   - `document-upload-portal.json`
   - `ingestion-pipeline.json`
   - `get-document-metadata.json`
   - `query-agent.json`

3. **Configure Credentials**
   - Qdrant API connection
   - Ollama endpoint
   - Groq API key
   - SerpAPI key (optional)

4. **Create Directories**
```bash
mkdir -p /files /files/processed
```

5. **Activate All Workflows**

## 🎯 Use Cases

### Perfect For:
- ✅ **Compliance Teams**: Authoritative policy retrieval
- ✅ **HR Departments**: Internal knowledge base queries
- ✅ **Legal Teams**: Contract and regulation analysis
- ✅ **Research Teams**: Multi-document reasoning
- ✅ **Customer Support**: Internal documentation search

### Key Benefits:
- 🔒 **Secure**: Prompt injection protection, local embeddings
- 📚 **Authoritative**: Direct citations from source documents
- 🧠 **Intelligent**: Multi-document reasoning via RAG
- 🔐 **Privacy-Focused**: Mostly local processing
- 📊 **Transparent**: Document inventory tracking

## 🚀 API Endpoints

Once activated:

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/webhook/upload-documents` | GET | Document upload UI |
| `/webhook/upload-documents-process` | POST | Process file upload |
| `/webhook/get-document-metadata` | GET | Collection statistics |
| `/chat/YOUR_WEBHOOK_ID` | WebSocket | AI chat interface |

## 💬 Example Queries

The Query Agent intelligently handles:

```
"How many documents are available?"
→ Uses Document_Metadata_Tool

"What does the employee handbook say about vacation policy?"
→ Uses Company_Knowledge_Base (Qdrant vector search)

"What are the latest labor laws in California?"
→ Uses Live_Web_Search (external fallback)

"Find all mentions of 'data retention' across our compliance docs"
→ Multi-document semantic search with citations
```

## 📊 Performance Metrics

The system automatically tracks:
- ⏱️ Processing time per document
- 📦 Chunks created per document
- ✅ Success/failure rates
- 📈 Query performance
- 🚨 Error categorization

View metrics in n8n execution logs.

## 🛡️ Security Features

### Built-in Protection
- **Prompt Injection Detection**: AI-powered malicious query blocking
- **PDF-only Validation**: File type enforcement
- **Size Limits**: 50MB maximum
- **Secure Credential Storage**: n8n encrypted credentials
- **Error Sanitization**: No sensitive data leakage

### Security Response Example
```
🚨 SECURITY ALERT

Your query has been identified as an attempt to access 
system configurations or manipulate operational parameters.

Please reformulate your request to focus on legitimate 
document-related questions.
```

## 🔧 Configuration

### Qdrant Settings
```yaml
Collection: documents
Dimension: 768 (nomic-embed-text)
Distance: Cosine
```

### Embedding Model
```yaml
Model: nomic-embed-text:latest
Provider: Ollama (local)
Dimensions: 768
```

### LLM Configuration
```yaml
Model: openai/gpt-oss-120b (Llama 3.3 70B)
Provider: Groq
Temperature: 0 (consistent responses)
```

### Document Processing
```yaml
Chunk Size: 2,500 characters
Chunk Overlap: 250 characters
Top K Results: 8 chunks per query
```

## 🚀 Advanced Usage

### Switch to Fully Local LLM
Replace Groq with Ollama in Query Agent:

1. Delete "Groq Chat Model" node
2. Add "Ollama Chat Model" node
3. Configure: `model: llama3.3:70b`
4. Reconnect to AI Agent

**Result**: 100% local, zero data leaves your infrastructure

### Add Authentication
Protect workflows with n8n's built-in auth:
- Basic Auth
- Header Auth
- OAuth2

### Custom Metadata Fields
Modify ingestion pipeline to add:
- Department tags
- Document categories
- Version control
- Author information

## 📈 System Maturity

**Classification**: Enhanced Agentic RAG MVP+

| Feature | Status |
|---------|--------|
| Core RAG | ✅ Production-ready |
| Agent Architecture | ✅ Multi-tool, intelligent routing |
| Privacy Focus | ✅ 80% local (95% with Ollama LLM) |
| Security Hardening | ✅ Prompt injection protection |
| Production Features | ✅ Metrics, error handling, UI |
| Scalability | ⚠️ Single-instance (can be scaled) |

## 🤝 Contributing

Contributions welcome! Areas for enhancement:
- [ ] Multi-user authentication
- [ ] Document versioning
- [ ] Bulk upload
- [ ] DOCX/TXT support
- [ ] Advanced search filters
- [ ] Audit logging for compliance

## 📝 License

MIT License - see LICENSE file

## 👤 Author

**Mohsin Ali**
- Email: alilaptop393@gmail.com
- System Type: Enhanced Agentic RAG Document Analyst

## 🔗 Resources

- [n8n Documentation](https://docs.n8n.io)
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [Ollama Models](https://ollama.ai/library)
- [Groq API](https://groq.com/docs)

## 🎯 What Makes This "Agentic"?

Unlike traditional RAG systems that just retrieve and generate:

1. **Tool Selection**: Agent intelligently chooses between 3 tools
2. **Adaptive Routing**: Internal docs → metadata → web search fallback
3. **Context Awareness**: Memory maintains conversation flow
4. **Security Reasoning**: Detects and blocks manipulation attempts
5. **Query Optimization**: Preprocesses queries for better retrieval
6. **Multi-step Reasoning**: Can combine metadata + content searches

This is a **reasoning agent** with RAG capabilities, not just a RAG system.

---

**Tags**: `agentic-rag` `document-analysis` `n8n-workflows` `vector-search` `ollama` `qdrant` `privacy-focused` `compliance-tools` `knowledge-base`
