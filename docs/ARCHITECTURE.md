## ✅ **Goals**
- User submits a YouTube podcast URL.
- System fetches metadata (title, duration, channel, etc.)
- Predicts average watch-time based on podcast details.
- Displays metadata and predicted watch-time in the frontend.
- Scalable, fault-tolerant, and maintainable design.

---

## 🧱 **Updated High-Level Architecture**

```
┌────────────────────────────────────────────┐
│                Frontend (SPA)              │
│          (Next.js + TanStack Query)        │
└────────────────────────────────────────────┘
                  │
      REST API    ▼
┌────────────────────────────────────────────┐
│          Backend API Gateway (REST)        │
│      Hono.js / NestJS + Auth Layer         │
│ (Handles input, orchestration, persistence)│
└────────────────────────────────────────────┘
        │              ▲
        ▼              │
Fetch from      Send result to frontend
YouTube API            │
        ▼              │
┌────────────────────────────────────────────┐
│       Video Metadata Processor (Node)      │
│     (Fetch YouTube details, clean data)    │
└────────────────────────────────────────────┘
        │
        ▼
┌────────────────────────────────────────────┐
│    gRPC or tRPC Client (Node.js Layer)     │
│        - Converts metadata to request      │
│        - Connects to Python FastAPI gRPC   │
└────────────────────────────────────────────┘
        │
  gRPC/tRPC Call
        ▼
┌────────────────────────────────────────────┐
│        ML Prediction Service (Python)      │
│   FastAPI + gRPC/tRPC + Trained Model API  │
│  (Receives structured metadata, predicts)  │
└────────────────────────────────────────────┘
        │
        ▼
      Prediction Result
        ▼
┌────────────────────────────────────────────┐
│           PostgreSQL (Metadata, Logs)      │
└────────────────────────────────────────────┘

```

---

## ✨ **Frontend**
- **Framework**: Next.js (App Router) + TypeScript
- **State/Data**: TanStack Query
- **Flow**:
  1. User inputs YouTube link
  2. Submit via form
  3. Render metadata + predicted watch-time

---

## 🛠️ **Backend**
### **API Gateway / Server**
- **Framework**: NestJS
- **Responsibility**:
  - Validate input (YouTube URL)
  - Call metadata fetcher
  - Trigger or call prediction service
  - Return combined result

---

## 🎬 **Video Metadata Fetching**
- Use [YouTube Data API v3] to fetch:
  - Title
  - Description
  - Duration
  - View count
  - Channel title
  - Published date


---

## 🧠 **ML Watch-Time Predictor**
- Language: FastAPI micro-service
- Input: Video metadata (duration, description, etc.)
- Output: Predicted watch time (minutes or %)

---

## 🛡️ **Security & DevOps**
- **Input Sanitization**: Validate and escape user inputs
- **Rate Limiting**: Prevent abuse on prediction endpoint
- **CI/CD**: GitHub Actions + Docker builds
- **Containerization**: All components Dockerized

---