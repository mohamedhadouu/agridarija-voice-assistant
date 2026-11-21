# AgriDarija: Voice Assistant in Moroccan Darija for Agriculture 🌾

A **voice and text assistant that understands and answers in Moroccan Darija** (Latin or Arabic script), French and Arabic, built on a **RAG (Retrieval-Augmented Generation)** pipeline over agricultural documents. The goal is to make agricultural information easier to access for Moroccan farmers, including people who read little, thanks to speech input and spoken answers.

![Python](https://img.shields.io/badge/Python-3.10-blue) ![Flask](https://img.shields.io/badge/Backend-Flask-black) ![OpenAI](https://img.shields.io/badge/LLM-GPT--4o-412991) ![FAISS](https://img.shields.io/badge/Vector%20search-FAISS-orange) ![MongoDB](https://img.shields.io/badge/DB-MongoDB-47A248)

## Features

- 🗣️ **Two-way voice interaction**: speech-to-text with Whisper, spoken answers with OpenAI TTS
- 🌐 **Multilingual**: Darija (Latin and Arabic script), French and Arabic, with automatic language detection
- 🔍 **Document search (RAG)** over technical agricultural documents
- 📄 **Document upload**: PDF, Word, PowerPoint and images (OCR with Tesseract), indexed automatically
- 💬 **Conversation memory** to keep the context of the last exchanges
- 👋 **Greeting detection** for fast, natural replies

## Architecture

```
User (voice or text)
        ↓
Web interface (HTML / CSS / JS)
        ↓
Flask backend
        ↓
Whisper speech-to-text (if audio)
        ↓
Language detection + greeting filter ──▶ direct greeting reply
        ↓
Query translation / reformulation to French (GPT)
        ↓
Embedding (text-embedding-3-large)
        ↓
Semantic search (FAISS index + MongoDB metadata)
        ↓
Answer generation with GPT-4o (multilingual prompt)
        ↓
Answer rewritten in the user's language + OpenAI TTS
        ↓
Spoken answer + text displayed in the interface
```

## Project structure

| File | Role |
|---|---|
| `app.py` | Flask web app: chat, document upload, audio upload / recording, text-to-speech, health check |
| `Main.py` | Orchestrates the system: embedding, retrieval, query processing, re-indexing |
| `Embedding.py` | Text extraction (PDF, Word, PowerPoint, images with OCR), chunking, embeddings, FAISS index and MongoDB storage |
| `Retriever_s.py` | Query embedding and semantic search in the FAISS index |
| `LLM.py` | Language detection, translation, Darija prompts, answer generation and conversation context |
| `Documents.py` | Detects changes in the source documents folder to trigger re-indexing |

## Tech stack

Python · Flask · OpenAI (GPT-4o, Whisper, TTS, text-embedding-3-large) · FAISS · MongoDB · Tesseract OCR · HTML / CSS / JavaScript

## How to run

1. **Install the dependencies**

```bash
git clone https://github.com/mohamedhadouu/agridarija-voice-assistant.git
cd agridarija-voice-assistant
pip install -r requirements.txt
```

2. **Configure your API key**: create a `.env` file

```bash
OPENAI_API_KEY=your_openai_api_key
```

3. **Start MongoDB locally** (default: `mongodb://localhost:27017/`) and put your documents in the `Source Files` folder

4. **Launch the app**

```bash
python app.py
```

The interface is available at `https://localhost:5000`. The app generates a self-signed certificate on first launch (HTTPS is needed for microphone access in the browser), so accept the browser security warning.

## Next steps

- Replace OpenAI with open-source models (Gemma, Mistral, open-source Whisper) to run fully locally
- Fine-tune speech recognition on Darija audio
- Add an evaluation set of real farmer questions
- Deploy a public demo

## Author

**Mohamed Hadou**, AI / ML Engineer · [LinkedIn](https://www.linkedin.com/in/mohamed-hadou/) · [ORCID](https://orcid.org/0009-0002-2913-0442) · [GitHub](https://github.com/mohamedhadouu)

This project is a proof of concept. Using it requires your own OpenAI API key.
