# 🧱 TileSupport AI — Customer Support Admin Console

A full-stack AI-powered customer support operations console for a tiles company.  
Built with **FastAPI** (backend), **Streamlit** (admin frontend), **Ollama** (local LLM), **MongoDB** (data), **ChromaDB** (RAG vector store), and **LiveKit + Deepgram** (real-time voice calls).

---

## 📦 Project Structure

```
customer-support-admin/
│
├── backend/                          # FastAPI Backend
│   ├── main.py                       # FastAPI app entry point
│   ├── requirements.txt              # Python dependencies
│   ├── .env                          # Backend environment variables
│   │
│   ├── app/
│   │   ├── api/
│   │   │   ├── __init__.py
│   │   │   └── routes.py             # All API endpoint definitions
│   │   │
│   │   ├── core/
│   │   │   ├── __init__.py
│   │   │   └── config.py             # BackendSettings (pydantic-settings)
│   │   │
│   │   ├── database/
│   │   │   ├── __init__.py
│   │   │   ├── mongo.py              # MongoDB client helpers (singleton)
│   │   │   └── seed_demo_data.py     # Seed demo customers, orders, etc.
│   │   │
│   │   ├── schemas/
│   │   │   ├── chat.py               # ChatMessageRequest/Response
│   │   │   └── support.py            # Various response models
│   │   │
│   │   ├── services/
│   │   │   ├── ai_service.py         # Direct LLM query testing
│   │   │   ├── chat_agent_service.py # Natural language chat agent (Ollama)
│   │   │   ├── data_service.py       # MongoDB CRUD for orders, customers, etc.
│   │   │   ├── livekit_service.py    # LiveKit room/token generation
│   │   │   ├── notification_service.py  # AI notification generation
│   │   │   └── speech_service.py     # STT (Whisper) + TTS (edge-tts) pipeline
│   │   │
│   │   ├── rag/
│   │   │   ├── __init__.py
│   │   │   ├── embedding_service.py  # Ollama embedding client
│   │   │   ├── ingest.py             # Knowledge base -> ChromaDB ingestion
│   │   │   ├── retriever.py          # Query -> RAG context retrieval
│   │   │   └── vector_store.py       # ChromaDB client and search
│   │   │
│   │   ├── ai_agents/
│   │   │   ├── __init__.py
│   │   │   └── router_agent.py       # (Planned) Intent routing agent
│   │   │
│   │   └── voice_agent/
│   │       └── livekit_worker.py     # LiveKit AI voice agent (Deepgram + Ollama)
│   │
│   ├── knowledge_base/               # Company policy documents (txt files)
│   │   ├── company_profile.txt
│   │   ├── damage_policy.txt
│   │   ├── delivery_policy.txt
│   │   ├── order_policy.txt
│   │   ├── product_faq.txt
│   │   └── return_policy.txt
│   │
│   ├── demo_data/                    # Seed data JSON files
│   │   ├── conversations.json
│   │   ├── customers.json
│   │   └── orders.json
│   │
│   └── storage/                      # Persistent data
│       ├── chroma/                   # ChromaDB vector store
│       └── voice/                    # Voice chat audio files
│
├── streamlit_app/                    # Streamlit Admin Frontend
│   ├── app.py                        # Main app with sidebar navigation
│   ├── config.py                     # AppConfig (dataclass)
│   ├── requirements.txt              # Frontend dependencies
│   │
│   ├── core/
│   │   ├── api_client.py             # Reusable HTTP client for backend
│   │   ├── auth.py                   # (Planned) Authentication
│   │   ├── constants.py              # Constants and enums
│   │   └── session.py                # Streamlit session state helpers
│   │
│   ├── services/
│   │   ├── admin_service.py          # Admin control operations
│   │   ├── ai_service.py             # AI testing service
│   │   ├── analytics_service.py      # Analytics data service
│   │   ├── chat_service.py           # Chat management service
│   │   ├── customer_service.py       # Customer data service
│   │   ├── email_service.py          # Email operations
│   │   ├── knowledge_service.py      # Knowledge base operations
│   │   ├── order_service.py          # Order management service
│   │   └── voice_service.py          # Voice chat & LiveKit service
│   │
│   ├── views/                        # Page render functions
│   │   ├── admin_controls.py         # System admin controls
│   │   ├── ai_testing.py             # AI query testing playground
│   │   ├── analytics.py              # Analytics dashboard
│   │   ├── chat_history.py           # Chat session history viewer
│   │   ├── customer_chat.py          # Live customer chat interface
│   │   ├── customer_history.py       # Customer conversation history
│   │   ├── customer_profile.py       # Customer profile viewer
│   │   ├── dashboard.py              # Main admin dashboard
│   │   ├── inbox_review.py           # Pending requests & complaints inbox
│   │   ├── knowledge_base.py         # RAG knowledge base management
│   │   ├── notification_center.py    # AI notification generator
│   │   ├── order_management.py       # Order CRUD management
│   │   ├── retry_center.py           # Failed operations retry
│   │   └── voice_chat.py             # Voice chat & LiveKit call UI
│   │
│   ├── components/                   # Reusable UI components
│   │   ├── empty_states.py
│   │   ├── json_viewer.py
│   │   ├── layout.py
│   │   ├── metric_cards.py
│   │   └── status_badges.py
│   │
│   ├── styles/
│   │   └── custom.css                # Custom Streamlit styling
│   │
│   └── assests/                      # Static assets (images, icons)
│
├── .env                              # Root environment variables
└── README.md                         # This file
```

---

## 🚀 Features

### 🤖 AI Chat Agent (Text)
- Natural language customer support via Ollama (Qwen 2.5)
- Intent detection: new orders, order tracking, complaints, general questions
- Automatic data extraction (email, order IDs, phone numbers, tile types, etc.)
- RAG-augmented answers from company policy knowledge base
- Session management with MongoDB

### 🎙️ Voice Agent (Real-Time)
- **LiveKit**-powered real-time voice calls
- **Deepgram** for Speech-to-Text and Text-to-Speech
- **Ollama** LLM for natural conversation
- **Silero VAD** for voice activity detection
- AI tools: order lookup, customer lookup, complaint registration, booking requests
- Automatic order ID normalization for voice transcription quirks

### 📊 Admin Dashboard (Streamlit)
- **Dashboard**: KPIs, priority metrics, recent orders/complaints/chats
- **Customer Chat**: Text chat interface with the AI agent
- **Voice Chat**: Voice recording + LiveKit real-time call integration
- **AI Testing**: Direct LLM playground for testing queries
- **Inbox Review**: Pending order requests & open complaints
- **Chat History**: Browse all past chat sessions
- **Customer History**: View customer conversation history
- **Customer Profile**: Look up customer details by email
- **Order Management**: View, update, and manage orders
- **Notification Center**: Generate AI-powered notifications (WhatsApp/Email/SMS)
- **Knowledge Base**: View and manage RAG knowledge base status
- **Admin Controls**: System configuration and demo data seeding
- **Analytics**: Charts and metrics on support operations
- **Retry Center**: Retry failed operations

### 🧠 RAG System
- **ChromaDB** vector store for company knowledge
- **Ollama embeddings** (`nomic-embed-text`) for document indexing
- Support policies indexed: company profile, delivery, returns, damages, orders, product FAQ
- Context-aware AI responses

---

## 🛠️ Tech Stack

| Component       | Technology                                      |
|----------------|-------------------------------------------------|
| Backend        | Python 3.11+, FastAPI, Uvicorn                  |
| Frontend       | Streamlit, streamlit-option-menu, plotly        |
| LLM            | Ollama (Qwen 2.5 14B)                           |
| Embeddings     | Ollama (nomic-embed-text)                       |
| Vector Store   | ChromaDB                                        |
| Database       | MongoDB (PyMongo)                               |
| Voice STT      | Deepgram Nova-3 / faster-whisper                |
| Voice TTS      | Deepgram Aura-2 / edge-tts                      |
| Voice Calls    | LiveKit, LiveKit Agents, Silero VAD             |
| HTTP Client    | requests (backend), APIClient (frontend)        |
| Validation     | Pydantic v2, pydantic-settings                  |

---

## ⚙️ Prerequisites

1. **Python 3.11+**
2. **MongoDB** (local or remote)
3. **Ollama** with models:
   - `qwen2.5:14b-instruct` (LLM)
   - `nomic-embed-text` (embeddings)
4. **LiveKit Server** (for voice calls)
5. **Deepgram API Key** (for voice calls)
6. **ChromaDB** (installed automatically via pip)

---

## 🔧 Installation & Setup

### 1. Clone the repository

```bash
git clone <repo-url>
cd customer-support-admin
```

### 2. Backend setup

```bash
cd backend

# Create virtual environment
python -m venv venv
# or: python3 -m venv venv

# Activate (Windows)
venv\Scripts\activate
# Activate (Linux/Mac)
# source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 3. Configure environment variables

Copy the example `.env` file:

```bash
# Backend .env (backend/.env)
# Copy from the existing file or create your own
```

**Key backend environment variables:**

| Variable                  | Default                          | Description                    |
|--------------------------|----------------------------------|--------------------------------|
| `OLLAMA_BASE_URL`        | `http://localhost:11434`         | Ollama server URL             |
| `LLM_MODEL`              | `qwen2.5:14b-instruct`           | Ollama chat model             |
| `OLLAMA_EMBEDDING_MODEL` | `nomic-embed-text`               | Ollama embedding model        |
| `MONGO_URI`              | `mongodb://localhost:27017`       | MongoDB connection URI        |
| `MONGO_DB_NAME`          | `tiles_support_demo`             | MongoDB database name         |
| `LIVEKIT_URL`            | `ws://localhost:7880`             | LiveKit WebSocket URL         |
| `LIVEKIT_API_KEY`        | `devkey`                         | LiveKit API key               |
| `LIVEKIT_API_SECRET`     | `secret`                         | LiveKit API secret            |
| `DEEPGRAM_API_KEY`       | *(empty)*                        | Deepgram API key (voice)      |
| `ADMIN_API_KEY`          | `dev_admin_key`                  | Admin API key for auth        |

### 4. Frontend setup

```bash
cd streamlit_app

# Install dependencies
pip install -r requirements.txt
```

### 5. Seed demo data & knowledge base

```bash
cd backend

# Seed MongoDB with demo customers, orders, conversations
python -c "from backend.app.database.seed_demo_data import seed_all; seed_all()"

# Ingest knowledge base into ChromaDB
python -c "from backend.app.rag.ingest import ingest_knowledge_base; ingest_knowledge_base()"
```

---

## 🏃 Running the Application

### Start the Backend (FastAPI)

```bash
cd backend
venv\Scripts\activate      # Windows
# source venv/bin/activate  # Linux/Mac

uvicorn backend.main:app --reload --host 0.0.0.0 --port 8000
```

API Docs: http://localhost:8000/docs  
Health Check: http://localhost:8000/health

### Start the Frontend (Streamlit)

In a new terminal:

```bash
cd streamlit_app
streamlit run app.py
```

Open http://localhost:8501 in your browser.

### Start the Voice Agent Worker

```bash
cd backend
venv\Scripts\activate      # Windows
python -m backend.app.voice_agent.livekit_worker
```

---

## 📡 API Endpoints

### Health & System
| Method | Endpoint            | Description                    |
|--------|---------------------|--------------------------------|
| GET    | `/`                 | Root welcome message          |
| GET    | `/health`           | Health check                  |
| GET    | `/db/status`        | MongoDB connection & counts   |
| GET    | `/rag/status`       | RAG/ChromaDB status           |

### Dashboard & Analytics
| Method | Endpoint               | Description                |
|--------|------------------------|----------------------------|
| GET    | `/dashboard/summary`   | Full dashboard data        |
| GET    | `/analytics/summary`   | Analytics summary          |

### Chat
| Method | Endpoint            | Description                            |
|--------|---------------------|----------------------------------------|
| POST   | `/chat/message`     | Send chat message to AI agent          |
| GET    | `/chat-sessions`    | List recent chat sessions              |
| GET    | `/chat-sessions/{id}` | Get chat session by ID                |

### Voice
| Method | Endpoint                  | Description                          |
|--------|---------------------------|--------------------------------------|
| POST   | `/voice/chat`             | Send audio message (STT + TTS)       |
| POST   | `/voice/livekit/call`     | Create LiveKit call room & token     |

### Orders
| Method | Endpoint                       | Description                      |
|--------|--------------------------------|----------------------------------|
| GET    | `/orders`                      | List all orders                 |
| GET    | `/orders/{order_id}`           | Get order by ID                 |
| POST   | `/orders/{order_id}/status`    | Update order status             |

### Order Requests
| Method | Endpoint                                  | Description                        |
|--------|-------------------------------------------|------------------------------------|
| GET    | `/order-requests`                         | List order requests               |
| GET    | `/order-requests/{id}`                    | Get order request by ID           |
| POST   | `/order-requests/{id}/approve`            | Approve & convert to order        |
| POST   | `/order-requests/{id}/reject`             | Reject order request              |
| POST   | `/order-requests/{id}/contacted`          | Mark as contacted                 |

### Complaints
| Method | Endpoint                          | Description                    |
|--------|-----------------------------------|--------------------------------|
| GET    | `/complaints`                     | List all complaints           |
| GET    | `/complaints/{id}`                | Get complaint by ID           |
| POST   | `/complaints/{id}/status`         | Update complaint status       |

### Customers
| Method | Endpoint                         | Description                        |
|--------|----------------------------------|------------------------------------|
| GET    | `/customers/{email}`             | Lookup customer by email          |
| GET    | `/customers/{email}/history`     | Get customer conversation history  |

### Notifications
| Method | Endpoint                                   | Description                        |
|--------|--------------------------------------------|------------------------------------|
| POST   | `/notifications/generate`                  | Generate AI notification message   |
| GET    | `/notifications/logs`                      | List notification logs             |
| GET    | `/notifications/logs/{id}`                 | Get notification log by ID         |
| POST   | `/notifications/logs/{id}/sent`            | Mark notification as sent          |

### AI & RAG
| Method | Endpoint              | Description                     |
|--------|-----------------------|---------------------------------|
| POST   | `/ai/test-query`      | Test direct LLM query          |
| POST   | `/rag/search`         | Search RAG knowledge base      |

---

## 🤖 Voice Agent Tools

The LiveKit voice agent (`TileSupportVoiceAgent`) provides these function tools:

| Tool                          | Description                                      |
|-------------------------------|--------------------------------------------------|
| `save_customer_email`         | Save customer email, fetch profile & latest order|
| `lookup_order`                | Look up order by ID (handles STT quirks)         |
| `lookup_customer_profile`     | Fetch customer profile by email                  |
| `get_latest_order_for_saved_customer` | Get latest order for known customer      |
| `search_company_rules`        | RAG search company policies                      |
| `save_complaint`              | Register a complaint for admin review            |
| `save_booking_request`        | Save new booking/order request for admin review  |
| `save_call_note`              | Save a voice call note to chat history           |

---

## 🗄️ MongoDB Collections

| Collection         | Purpose                                        |
|--------------------|------------------------------------------------|
| `customers`        | Customer profiles (email, name, phone, address)|
| `orders`           | Orders (status, items, amounts, delivery dates)|
| `conversations`    | Historical customer conversations               |
| `chat_sessions`    | Active/text chat sessions                       |
| `order_requests`   | New order requests pending admin review         |
| `complaints`       | Customer complaints                             |
| `notification_logs`| Generated notification history                  |

---

## 📚 Knowledge Base Documents

The RAG system indexes these policy files:

| File                   | Content                                           |
|------------------------|---------------------------------------------------|
| `company_profile.txt`  | Company overview, mission, values, contact info   |
| `order_policy.txt`     | Ordering process, minimum quantities, lead times  |
| `delivery_policy.txt`  | Delivery areas, timelines, shipping costs          |
| `return_policy.txt`    | Return eligibility, conditions, process            |
| `damage_policy.txt`    | Damage reporting, inspection, replacement process  |
| `product_faq.txt`      | Frequently asked questions about tile products    |

---

## 🧪 Development

### Run tests
```bash
cd streamlit_app
pytest                  # Streamlit tests
```

### Code formatting
```bash
cd streamlit_app
black .
ruff check .
```

### Seed demo data manually
```python
from backend.app.database.seed_demo_data import seed_all
seed_all()
```

### Re-ingest knowledge base
```python
from backend.app.rag.ingest import ingest_knowledge_base
ingest_knowledge_base()
```

---

## 🐳 Docker (Planned)

Docker support is planned for future releases to simplify deployment with all services (MongoDB, Ollama, ChromaDB, LiveKit) running in containers.

---

## 🔐 Security

- Admin API key authentication via `X-Admin-API-Key` header
- CORS restricted to `localhost:8501`
- LiveKit tokens with granular room permissions
- No customer PII exposure via API responses
- Voice agent never exposes internal tool names to customers

---

## 📄 License

This project is for demonstration and internal use.  
All rights reserved.

---

## 👥 Contributors

- **Essa Afridi** — Initial development & architecture

---

## 🆘 Support

For issues, questions, or feature requests, please open an issue in the project repository.