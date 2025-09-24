# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Course Materials RAG (Retrieval-Augmented Generation) System - a full-stack web application that enables users to query course materials and receive intelligent, context-aware responses using ChromaDB for vector storage and Anthropic's Claude for AI generation.

## Architecture

The system follows a modular Python backend + HTML/JS frontend architecture:

### Backend Components (`/backend/`)
- `app.py` - FastAPI application with CORS, serves frontend and provides API endpoints
- `rag_system.py` - Main orchestrator that coordinates all components
- `document_processor.py` - Processes course documents into chunks
- `vector_store.py` - ChromaDB interface for semantic search
- `ai_generator.py` - Anthropic Claude integration with tool support
- `session_manager.py` - Manages conversation history
- `search_tools.py` - Tool-based search system for Claude
- `models.py` - Pydantic models for courses, lessons, and chunks
- `config.py` - Configuration management with environment variables

### Frontend Components (`/frontend/`)
- Static HTML/CSS/JS served by FastAPI
- Communicates with backend via `/api/query` and `/api/courses` endpoints

### Key Features
- Tool-based search: Claude uses defined search tools rather than direct vector similarity
- Session management: Maintains conversation context with configurable history
- Document processing: Automatically chunks course materials with overlap
- Vector storage: Uses ChromaDB with sentence-transformers embeddings
- Course analytics: Tracks course statistics and titles

## Development Commands

### Running the Application
```bash
# Quick start (preferred)
chmod +x run.sh
./run.sh

# Manual start
cd backend && uv run uvicorn app:app --reload --port 8000
```

### Package Management
```bash
# Install dependencies
uv sync

# Add new dependencies
uv add package_name
```

### Environment Setup
Create `.env` file in root:
```
ANTHROPIC_API_KEY=your_api_key_here
```

## Configuration

Key settings in `backend/config.py`:
- `CHUNK_SIZE: 800` - Text chunk size for vector storage
- `CHUNK_OVERLAP: 100` - Character overlap between chunks
- `MAX_RESULTS: 5` - Maximum search results returned
- `MAX_HISTORY: 2` - Conversation messages to remember
- `EMBEDDING_MODEL: "all-MiniLM-L6-v2"` - Sentence transformer model
- `ANTHROPIC_MODEL: "claude-sonnet-4-20250514"` - Claude model version

## Data Flow

1. Course documents in `/docs/` are processed on startup
2. Documents are chunked and stored in ChromaDB (`./chroma_db/`)
3. User queries trigger tool-based search via Claude
4. Claude uses `CourseSearchTool` to find relevant content
5. Response generated with sources and conversation context maintained

## Important Notes

- The system uses tool-based search rather than direct similarity search
- Course documents are automatically deduplicated by title
- Frontend served at root path, API at `/api/*`
- ChromaDB storage persists in `./chroma_db/` directory
- Requires Python 3.13+ and Anthropic API key
- Always use uv to run the server do not use pip directly
- make sure that uv manage all dependencies
- use uv to run python files