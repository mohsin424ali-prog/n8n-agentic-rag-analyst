# Contributing to Agentic RAG Document Analyst

Thank you for your interest in contributing! This document provides guidelines for contributing to this project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Development Workflow](#development-workflow)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)

## Code of Conduct

### Our Standards

**Positive behavior includes**:
- Using welcoming and inclusive language
- Being respectful of differing viewpoints
- Gracefully accepting constructive criticism
- Focusing on what is best for the community

**Unacceptable behavior includes**:
- Trolling, insulting/derogatory comments
- Public or private harassment
- Publishing others' private information

## Getting Started

### Prerequisites

- Git
- Docker and Docker Compose
- Basic understanding of n8n workflows
- Familiarity with JavaScript
- Knowledge of RAG systems (helpful)

### Setting Up Development Environment

1. **Fork the Repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/n8n-agentic-rag-analyst.git
   cd n8n-agentic-rag-analyst
   ```

2. **Set Up Development Stack**
   ```bash
   docker-compose up -d
   docker ps
   ```

3. **Import Workflows**
   - Access n8n at `http://localhost:5678`
   - Import all workflows from `workflows/` directory

## How to Contribute

### Types of Contributions

1. **Bug Reports** 🐛
   - Use GitHub Issues
   - Include steps to reproduce
   - Provide workflow execution logs

2. **Feature Requests** 💡
   - Describe the use case
   - Explain expected behavior
   - Consider privacy/security implications

3. **Code Contributions** 💻
   - Bug fixes
   - New features
   - Performance improvements
   - Documentation updates

## Development Workflow

### Branch Naming Convention

```
feature/your-feature-name
bugfix/issue-number-description
docs/documentation-update
security/security-fix
```

### Testing Changes

1. **Functional Testing**
   - Upload test documents
   - Run queries through agent
   - Verify security features

2. **Performance Testing**
   - Measure ingestion time
   - Check query latency

## Pull Request Process

### PR Title Format

```
[Type] Short description

Types: feat, fix, docs, security, perf
```

Examples:
- `[feat] Add bulk document upload`
- `[fix] Resolve Qdrant timeout`
- `[docs] Update privacy guide`

### PR Description Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update

## Testing
How was this tested?

## Checklist
- [ ] Tested in development
- [ ] Documentation updated
- [ ] No sensitive data
```

## Coding Standards

### Workflow JSON Standards

```json
{
  "name": "Descriptive Workflow Name",
  "nodes": [
    {
      "id": "descriptive-node-id",
      "name": "Descriptive Node Name"
    }
  ]
}
```

### JavaScript Code Standards

```javascript
// ✅ Good: Clear names, error handling
const input = $input.first().json;

try {
  // Process data
  return { json: result };
} catch (error) {
  throw new Error(`Failed: ${error.message}`);
}
```

## Areas Needing Contribution

### High Priority

1. **Authentication System**
2. **Bulk Upload**
3. **Document Management**
4. **Multi-format Support**

### Medium Priority

5. **Monitoring Dashboard**
6. **Advanced Search**
7. **Audit Logging**

## Questions?

- 📧 Email: alilaptop393@gmail.com
- 💬 GitHub Discussions
- 🐛 GitHub Issues

Thank you for contributing! 🙏
