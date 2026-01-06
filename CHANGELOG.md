# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Planned
- Bulk document upload functionality
- User authentication system
- Document deletion API
- DOCX and TXT file support
- Admin dashboard
- Audit logging

## [1.0.0] - 2026-01-07

### Initial Release - Enhanced Agentic RAG MVP

#### Added

**Core Workflows**
- `document-upload-portal.json` - Web-based PDF upload interface
- `ingestion-pipeline.json` - Automated document vectorization
- `get-document-metadata.json` - Collection statistics API
- `query-agent.json` - AI-powered document query interface

**Document Upload Portal**
- Beautiful drag-and-drop web interface
- PDF validation (max 50MB)
- Real-time upload progress
- Error handling with user-friendly messages
- Responsive design for mobile/desktop

**Ingestion Pipeline**
- File system monitoring (`/files/` directory)
- PDF-only validation
- Text extraction from PDFs
- Intelligent chunking (2,500 chars, 250 overlap)
- Local embeddings via Ollama (nomic-embed-text)
- Qdrant vector storage
- Automatic file archiving to `/files/processed/`
- Comprehensive metrics tracking:
  - Processing time
  - Chunks created
  - Success/failure rates
  - File size statistics

**Metadata Service**
- RESTful API endpoint for collection statistics
- Total chunks and vectors count
- Unique documents list
- JSON response format

**Query Agent (Agentic RAG)**
- Multi-tool AI agent with intelligent routing
- Three specialized tools:
  1. Company_Knowledge_Base (vector search)
  2. Document_Metadata_Tool (statistics)
  3. Live_Web_Search (internet fallback)
- Query optimization (filler word removal)
- Conversation memory
- Source citation in responses
- Groq LLM integration (Llama 3.3 70B)
- Security layer with prompt injection detection

**Security Features**
- AI-powered prompt injection detection
- System prompt protection
- Role manipulation blocking
- File type validation
- File size limits (50MB)
- Sanitized filenames
- Comprehensive error handling

**Privacy Features**
- 80% local processing (embeddings, storage, orchestration)
- Self-hosted Qdrant vector database
- Local Ollama embeddings
- Optional fully local operation (with Ollama LLM)

**Documentation**
- Comprehensive README with architecture diagram
- ARCHITECTURE.md - Detailed system design
- PRIVACY.md - Data sovereignty guide
- SECURITY.md - Security best practices
- CONTRIBUTING.md - Contribution guidelines
- Complete setup instructions
- API documentation
- Troubleshooting guide

**Configuration**
- Environment-based credential management
- Configurable chunk sizes
- Adjustable embedding models
- Flexible LLM provider selection

### Technical Details

**Stack**
- n8n v1.0.0+ for workflow orchestration
- Qdrant v1.7.0+ for vector storage
- Ollama for local embeddings
- Groq API for LLM inference
- SerpAPI for web search (optional)

**Performance**
- Processes PDFs in 5-10 seconds
- Vector search latency < 500ms
- Handles documents up to 50MB
- Scales to 1000+ documents tested

**Compatibility**
- n8n Cloud and self-hosted
- Docker deployment ready
- Linux/macOS/Windows support
- Works with standard Qdrant setup

## Upgrade Guide

### From Future Versions
(Will be added as new versions are released)

## Known Issues

### Version 1.0.0

**Minor Issues**
- Conversation memory is session-based (not persistent)
- No built-in authentication (requires manual setup)
- Single file upload only (bulk upload not yet implemented)
- Limited to PDF files (DOCX/TXT planned)

**Workarounds**
- Use external authentication proxy for webhooks
- Upload multiple files sequentially
- Convert DOCX to PDF before upload

## Security Updates

### Version 1.0.0
- Implemented prompt injection detection
- Added file validation and sanitization
- Included security best practices documentation

## Performance Improvements

### Version 1.0.0
- Optimized chunking strategy (2500/250)
- Efficient vector search with top-k=8
- Local embeddings for fast processing
- Metrics tracking for monitoring

## Roadmap

### v1.1.0 (Q1 2026)
- [ ] Bulk document upload
- [ ] Document deletion API
- [ ] User authentication
- [ ] Audit logging

### v1.2.0 (Q2 2026)
- [ ] DOCX/TXT support
- [ ] Advanced search filters
- [ ] Admin dashboard
- [ ] Prometheus metrics

### v2.0.0 (Q3 2026)
- [ ] Multi-tenancy support
- [ ] Document versioning
- [ ] Collaborative annotations
- [ ] Mobile app

---

**Note**: This is version 1.0.0 - the initial public release of the Enhanced Agentic RAG Document Analyst system.
