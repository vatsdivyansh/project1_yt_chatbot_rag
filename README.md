# project1_yt_chatbot_rag

A RAG-based YouTube chatbot built with LangChain, Groq (LLaMA 3.3), HuggingFace Embeddings, and FAISS.

## How It Works
```
YouTube URL → Transcript → Chunks → Embeddings → FAISS → Retriever → LLM → Answer
```

## Tech Stack

| Component | Tool |
|---|---|
| Transcript Extraction | `youtube-transcript-api` |
| Text Splitting | `RecursiveCharacterTextSplitter` |
| Embeddings | `HuggingFace all-MiniLM-L6-v2` |
| Vector Store | `FAISS` |
| LLM | `LLaMA 3.3 70B via Groq API` |
| Orchestration | `LangChain LCEL` |

## Setup
```bash
pip install youtube-transcript-api langchain-community langchain-groq \
            langchain-huggingface langchain-text-splitters faiss-cpu \
            sentence-transformers tiktoken python-dotenv
```

Set your API key:
```python
from google.colab import userdata
os.environ["GROQ_API_KEY"] = userdata.get("GROQ_API_KEY")
```

## Usage
```python
# 1. Fetch transcript
ytt_api    = YouTubeTranscriptApi()
fetched    = ytt_api.fetch(video_id)
transcript = " ".join(snippet.text for snippet in fetched)

# 2. Split → Embed → Store
chunks       = text_splitter.split_documents([Document(page_content=transcript)])
vector_store = FAISS.from_documents(chunks, HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2"))

# 3. Query
chain.invoke("your question here")
```

## Notes

- Works only on YouTube videos with English transcripts enabled
- Uses free open-source models — no OpenAI API key required
