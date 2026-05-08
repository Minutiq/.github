# 🎙️ Minutiq — Meeting Intelligence, Searchable by Meaning

*AI-powered transcription, analysis, and RAG-based Q&A for every meeting.*

![Java](https://img.shields.io/badge/Backend-Java%20Spring%20Boot-6DB33F?logo=springboot&logoColor=white)
![OpenAI Whisper](https://img.shields.io/badge/Transcription-OpenAI%20Whisper-412991?logo=openai&logoColor=white)
![Anthropic Claude](https://img.shields.io/badge/Analysis-Anthropic%20Claude-191919)
![OpenAI Embeddings](https://img.shields.io/badge/Embeddings-text--embedding--3--small-412991?logo=openai&logoColor=white)
![ChromaDB](https://img.shields.io/badge/Vector%20Store-ChromaDB-5A4BDA)
![WebSocket](https://img.shields.io/badge/Streaming-Spring%20WebSocket-6DB33F?logo=spring&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Container-Docker-2496ED?logo=docker&logoColor=white)

## What is Minutiq?

Minutiq is an AI-powered meeting intelligence platform that transcribes, analyzes, and stores meeting data for semantic retrieval. It combines Whisper transcription, Claude-based meeting analysis, and ChromaDB vector search to support a Retrieval-Augmented Generation (RAG) workflow. Teams can ask natural language questions about past meetings and get context-aware answers grounded in transcript evidence.

## Features

- 🎧 **Audio upload transcription** for common formats (MP3, WAV, M4A, and more)
- 🎙️ **Live microphone transcription** via Spring WebSocket streaming
- 📄 **Transcript ingestion** for pre-existing text meeting notes/transcripts
- 🧠 **AI meeting analysis**: summaries, action items, key topics, and decisions
- 🧩 **ChromaDB vector storage** with semantic chunking + embeddings
- 🔎 **RAG query interface** to ask natural language questions across meetings

## System Architecture Overview

```text
            +------------------------+
            |   Client / Frontend    |
            | upload | live stream   |
            +-----------+------------+
                        |
                        v
          +-----------------------------+
          |     Spring Boot Backend     |
          | REST API + WebSocket        |
          +------+----------------------+
                 |                     |
                 v                     v
      +-------------------+   +-------------------+
      | OpenAI Whisper API|   | Claude API        |
      | transcription     |   | analysis outputs  |
      +---------+---------+   +---------+---------+
                |                       |
                +-----------+-----------+
                            v
                +------------------------+
                | Chunk + Embed Pipeline |
                | text-embedding-3-small |
                +-----------+------------+
                            |
                            v
                +------------------------+
                | ChromaDB Vector Store  |
                +-----------+------------+
                            |
                            v
                +------------------------+
                | RAG Retrieval + LLM Q&A|
                +------------------------+
```

## Getting Started (Quickstart)

1. **Clone the repository**
   ```bash
   git clone https://github.com/Minutiq/minutiq.git
   cd minutiq
   ```
2. **Configure environment variables**
   ```bash
   cp .env.example .env
   ```
3. **Start infrastructure**
   ```bash
   docker compose up -d
   ```
4. **Run the backend**
   ```bash
   ./mvnw spring-boot:run
   ```
5. **Open API/docs**
   - App/API: `http://localhost:8080`

## Environment Variables

| Variable | Required | Description |
|---|---|---|
| `OPENAI_API_KEY` | Yes | API key for Whisper transcription + embeddings |
| `ANTHROPIC_API_KEY` | Yes | API key for Claude analysis / generation |
| `CHROMA_HOST` | Yes | ChromaDB host (e.g. `localhost`) |
| `CHROMA_PORT` | Yes | ChromaDB port (e.g. `8000`) |
| `SPRING_DATASOURCE_URL` | Yes | JDBC URL for H2 (dev) or PostgreSQL (prod) |
| `SPRING_DATASOURCE_USERNAME` | Yes | Database username |
| `SPRING_DATASOURCE_PASSWORD` | Yes | Database password |
| `SERVER_PORT` | No | Spring Boot server port (default: `8080`) |

## API Endpoints Overview

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/api/transcriptions/upload` | Upload and transcribe audio files |
| `WS` | `/ws/transcriptions/live` | Live microphone streaming transcription |
| `POST` | `/api/transcripts/ingest` | Ingest pre-existing text transcripts |
| `POST` | `/api/analysis/{meetingId}` | Generate summary, action items, topics, decisions |
| `POST` | `/api/rag/query` | Ask natural language questions over meeting corpus |
| `GET` | `/api/meetings/{meetingId}` | Fetch meeting metadata and processed artifacts |

## RAG Pipeline

Minutiq transforms transcript text into semantically meaningful chunks, generates embeddings using `text-embedding-3-small`, and stores vectors in ChromaDB. During query time, relevant chunks are retrieved by semantic similarity and supplied as context to the LLM. This produces answers that are grounded in actual meeting content rather than generic model memory.

## Contributing

Contributions are welcome. Please open an issue to discuss substantial changes before submitting a pull request. Keep PRs focused, include tests where applicable, and document behavior changes.

## License

This project is open source. See the `LICENSE` file for details.
