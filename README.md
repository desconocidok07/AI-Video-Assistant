# 🎬 AI Video Assistant

**Turn any meeting recording or YouTube video into a searchable, summarized, chat-ready transcript.**

AI Video Assistant is a Streamlit app that takes a YouTube URL or a local audio/video file, transcribes it locally with Whisper, generates a summary + title, pulls out action items, key decisions, and open questions, and spins up a RAG (Retrieval-Augmented Generation) engine so you can chat with your meeting afterward.

---

## ✨ Features

- 🎥 **Flexible input** — analyze a YouTube URL or a local audio/video file
- 🗣️ **Local transcription** — powered by OpenAI Whisper (runs on your machine, no audio leaves your system)
- 🌐 **Hinglish support** — optional Hindi → English translation via `deep-translator`
- 🏷️ **Auto-generated title** for each session
- 📋 **Summary generation** of the full transcript
- ✅ **Action item extraction**
- 🔑 **Key decision extraction**
- ❓ **Open question extraction**
- 💬 **Chat with your meeting** — ask follow-up questions against the transcript using a RAG pipeline (ChromaDB + HuggingFace embeddings + Mistral LLM)
- 🖥️ **Two ways to run it** — a polished dark-themed Streamlit UI (`app.py`) or a plain CLI (`main.py`)

---

## 🧠 How It Works

```
Input (YouTube URL / local file)
        │
        ▼
 utils/audio_processor.py   →  extracts & chunks audio
        │
        ▼
 core/transcriber.py        →  Whisper transcription (+ optional Hinglish translation)
        │
        ▼
 core/summarizer.py         →  title + summary generation (Mistral via LangChain)
        │
        ▼
 core/extractor.py          →  action items / key decisions / open questions
        │
        ▼
 core/rag_engine.py         →  builds a RAG chain over the transcript
        │
        ▼
   Chat interface (ask questions about the transcript)
```

---

## 📁 Project Structure

```
AI-Video-Assistant/
├── app.py                  # Streamlit web app (main UI)
├── main.py                 # CLI entry point
├── core/
│   ├── transcriber.py       # Whisper-based transcription
│   ├── summarizer.py        # Title + summary generation
│   ├── extractor.py         # Action items / decisions / questions
│   └── rag_engine.py        # RAG chain builder + Q&A
├── utils/
│   └── audio_processor.py   # Audio download/extraction & chunking
├── Requirements.txt
└── .gitignore
```

---

## 🛠️ Tech Stack

| Layer | Tools |
|---|---|
| UI | Streamlit |
| Speech-to-Text | OpenAI Whisper (local, via PyTorch) |
| Audio/Video acquisition | yt-dlp, pydub, ffmpeg-python |
| Translation | deep-translator (Hindi → English) |
| LLM orchestration | LangChain (LCEL) + Mistral AI |
| Vector store / RAG | ChromaDB, sentence-transformers, LangChain-HuggingFace |
| Export | reportlab, fpdf2 |
| Config | python-dotenv |

---

## ⚙️ Setup

### 1. Prerequisites

- Python 3.10+
- [FFmpeg](https://ffmpeg.org/download.html) installed and available on your system `PATH` (required by `ffmpeg-python` / Whisper)
- A [Mistral AI API key](https://console.mistral.ai/)

### 2. Clone the repo

```bash
git clone https://github.com/desconocidok07/AI-Video-Assistant.git
cd AI-Video-Assistant
```

### 3. Create a virtual environment (recommended)

```bash
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
```

### 4. Install dependencies

```bash
pip install -r Requirements.txt
```

### 5. Configure environment variables

Create a `.env` file in the project root:

```env
MISTRAL_API_KEY=your_mistral_api_key_here
```

---

## 🚀 Usage

### Option A — Streamlit App (recommended)

```bash
streamlit run app.py
```

Then, in the sidebar:
1. Paste a **YouTube URL** or a **local file path**
2. Choose the language (`english` or `hinglish`)
3. Click **⚡ Analyse**
4. View the title, summary, action items, key decisions, and open questions
5. Use the **💬 Chat with your Meeting** panel to ask follow-up questions

### Option B — Command Line

```bash
python main.py
```

You'll be prompted for:
- A YouTube URL or local file path
- A language (`english` / `hinglish`)

The script prints the title, summary, action items, decisions, and open questions, then drops you into an interactive chat loop — type `exit`, `quit`, or `q` to leave.

---

## 📝 Example (CLI)

```
Enter YouTube URL or local file path: https://youtube.com/watch?v=xxxxxxxx
Language (english/hinglish): english

============================================================
📌 Title: Q3 Roadmap Planning Sync
📋 Summary: ...
✅ Action Items: ...
🔑 Key Decisions: ...
❓ Open Questions: ...
============================================================

💬 Chat with your meeting (type 'exit' to quit)

You: What did we decide about the launch date?
🤖 Assistant: ...
```

---

## 🗺️ Roadmap Ideas

- [ ] Export summary/transcript to PDF (dependencies already included: `reportlab`, `fpdf2`)
- [ ] Multi-speaker diarization
- [ ] Support for additional languages beyond Hinglish
- [ ] Dockerized setup

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome. Feel free to open an issue or submit a pull request.

## 📄 License

No license specified yet — add a `LICENSE` file to define how others can use this project.
