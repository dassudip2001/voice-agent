# LiveKit Voice Agent + HR Policy AI Assistant

A full-stack **AI-powered HR support assistant** that combines:

* 🎤 **Real-time Voice AI Agent** using LiveKit
* 👤 **Interactive AI Avatar** using Beyond Presence
* 🤖 **RAG-based HR Policy Chatbot** using LangChain + ChromaDB
* 🌐 **Modern Web Client** using Next.js

The system allows employees to **talk with an AI avatar**, ask **HR policy questions**, troubleshoot issues, and receive **automated email support summaries**.

---

# 🚀 Features

## 🎤 Voice AI Assistant

* Real-time voice conversations
* Speech-to-text and text-to-speech
* Voice activity detection (VAD)
* Noise cancellation
* Thinking sounds while processing

## 👤 AI Avatar

* Interactive avatar powered by **Beyond Presence**
* Human-like visual engagement
* Live avatar lip-sync with speech

## 🤖 HR Policy RAG Chatbot

* Retrieval-Augmented Generation
* Answers questions based on HR policy documents
* Semantic vector search using **ChromaDB**
* Context-aware responses using **LangChain**

## 🛠️ HR Support Tools

The voice agent can:

* Unblock employee accounts
* Send support emails
* Document support tickets
* Assist with login issues
* Guide troubleshooting using screen sharing
* Query HR policy knowledge base via API

## 📧 Email Integration

* Gmail SMTP integration
* Sends ticket summaries to users
* Supports CC recipients

## 🔌 External API Integration

The Voice Agent connects to the **HR RAG backend** to answer HR policy questions.

---

# 🏗 System Architecture

```
                        +----------------------+
                        |   Next.js Frontend   |
                        |   HR Chat Client     |
                        +----------+-----------+
                                   |
                                   |
                        REST API /api/v1/ask
                                   |
                                   v
                    +-----------------------------+
                    |  Flask RAG Backend API      |
                    |  LangChain + ChromaDB       |
                    +--------------+--------------+
                                   |
                                   |
                            HR Policy Docs
                                   |
                                   v
                          Vector Embeddings


        +------------------------------------------------+
        |             LiveKit Voice Agent                |
        |                                                |
        |  STT (AssemblyAI)                              |
        |  LLM (OpenAI GPT-4.1-mini)                     |
        |  TTS (Cartesia Sonic 3)                        |
        |  Avatar (Beyond Presence)                      |
        |                                                |
        |  Tools:                                        |
        |  • unblock_user()                              |
        |  • send_email()                                |
        |  • HR policy API query                         |
        +------------------------------------------------+
```

---

# 📁 Project Structure

```
project-root/
│
├── livekit-voice-agent/           # Voice AI assistant
│   ├── agent.py                   # Main voice agent
│   ├── main.py                    # Alternative agent
│   ├── prompts.py                 # Agent instructions
│   ├── tools.py                   # Agent tools
│   ├── api_call.py                # HR API integration
│   ├── pyproject.toml
│   └── .env.local
│
├── chat-bot-langchain-latest/     # HR RAG backend
│   ├── app/
│   │   ├── config/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── routes/
│   │   └── services/
│   ├── chroma_db/
│   ├── main.py
│   └── pyproject.toml
│
└── chat-client/                   # Next.js frontend
    ├── app/
    ├── public/
    ├── package.json
    └── tsconfig.json
```

---

# 🛠 Tech Stack

## AI & Backend

* Python 3.13+
* Flask
* LangChain
* ChromaDB
* OpenAI GPT-4.1-mini
* OpenAI text-embedding-3-large

## Voice AI

* LiveKit Agents
* AssemblyAI (STT)
* Cartesia Sonic 3 (TTS)
* Silero VAD
* Beyond Presence Avatar

## Frontend

* Next.js 16
* React 19
* TypeScript
* Tailwind CSS
* Bun

## Infrastructure

* Docker
* ChromaDB
* Gmail SMTP

---

# 📋 Prerequisites

* Python **3.13+**
* Node.js **20+**
* Bun (recommended)
* OpenAI API Key
* LiveKit Account
* Beyond Presence Avatar ID
* Gmail App Password
* Git

---

# 🔧 Installation

# 1️⃣ Clone Repository

```bash
git clone <repository-url>
cd project-root
```

---

# 2️⃣ Setup Voice Agent

```
cd livekit-voice-agent
```

Install dependencies:

```bash
uv sync
```

Create `.env.local`

```env
LIVEKIT_URL=
LIVEKIT_API_KEY=
LIVEKIT_API_SECRET=

BEY_AVATAR_ID=

OPENAI_API_KEY=

GMAIL_USER=
GMAIL_APP_PASSWORD=

EXTERNAL_API_URL=http://localhost:5000/api/v1/ask
```

---

# 3️⃣ Setup HR RAG Backend

```
cd chat-bot-langchain-latest
```

Install dependencies:

```bash
uv pip install -e .
```

Create `.env`

```env
OPENAI_API_KEY=
PORT=5000
FLASK_ENV=development
```

Run backend:

```bash
python main.py
```

Backend runs at:

```
http://localhost:5000
```

---

# 4️⃣ Setup Frontend Client

```
cd chat-client
```

Install dependencies:

```bash
bun install
```

Start client:

```bash
bun dev
```

Frontend runs at:

```
http://localhost:3000
```

---

# 🚀 Running the Full System

### Step 1 — Start HR Backend

```
cd chat-bot-langchain-latest
python main.py
```

### Step 2 — Start Voice Agent

```
cd livekit-voice-agent
uv run python agent.py
```

### Step 3 — Start Frontend

```
cd chat-client
bun dev
```

---

# 📡 API Endpoints

## Ask HR Policy

```
POST /api/v1/ask
```

Request

```json
{
  "question": "What is the leave policy?"
}
```

Response

```json
{
  "answer": "Employees are entitled to...",
  "status": "success"
}
```

---

## Health Check

```
GET /api/v1/health
```

Response

```json
{
  "status": "healthy",
  "message": "Service running"
}
```

---

# 🔄 How the RAG System Works

1️⃣ HR documents are split into chunks
2️⃣ Chunks converted into embeddings
3️⃣ Stored in **ChromaDB vector database**
4️⃣ User query converted to embedding
5️⃣ Top relevant documents retrieved
6️⃣ GPT-4.1-mini generates contextual answer

---

# 🛠 Voice Agent Tools

### unblock_user(username)

Removes block file and unlocks account.

### send_email(to_email, subject, message)

Sends support ticket summary via Gmail SMTP.

### ask_hr_policy(question)

Calls backend API to answer HR policy questions.

---

# 🐳 Docker Support

Run ChromaDB:

```
docker-compose -f chroma-docker-compose.yml up -d
```

Run backend:

```
docker-compose up -d
```

---

# 🧪 Testing

```
cd chat-bot-langchain-latest
python testing/testing.py
```

---

# 🐛 Troubleshooting

### Avatar not appearing

Check `BEY_AVATAR_ID`.

### Email not sending

Use **Gmail App Password**, not regular password.

### Backend connection error

Ensure backend is running on port **5000**.

### ChromaDB error

Check permissions of `chroma_db/`.

---

# 📄 License

MIT License

---

# 👤 Author

**Sudip Das**

GitHub
https://github.com/dassudip2001

---

# 🤝 Contributing

Pull requests are welcome.
For major changes, please open an issue first.

---

# 📞 Support

If you encounter issues, please open a GitHub issue.

---

**Project created for AI-powered HR Support and Voice Assistant Systems.**
