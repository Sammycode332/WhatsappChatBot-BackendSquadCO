# WhatsApp AI Marketplace Chatbot

A WhatsApp-powered AI marketplace assistant for Nigerian traders and job seekers. The system onboards users via WhatsApp, extracts their profession, location, and goals using OpenAI GPT-4o-mini, and stores profiles in a PostgreSQL database via Prisma. The project contains two sub-applications:

| Sub-project | Description |
|---|---|
| `whatsapp-ai-marketplace` | WhatsApp webhook bot — handles messages, onboarding, AI understanding, and voice notes |
| `backend-ai` | REST API backend — authentication, user profiles, scores, payments (Squad), and dashboards |

---

## 📁 Project Structure

```
whatsapp chatbot/
├── whatsapp-ai-marketplace/     # WhatsApp bot (TypeScript)
│   ├── src/
│   │   ├── server.ts            # Express entry point & webhook registration
│   │   ├── webhooks/
│   │   │   ├── whatsapp.ts      # Incoming WhatsApp message router
│   │   │   └── whatsappClient.ts# WhatsApp API client (send messages, buttons)
│   │   ├── bot/
│   │   │   ├── onboarding.ts    # Multi-stage onboarding conversation flow
│   │   │   ├── aiUnderstanding.ts # GPT-4o-mini profile extraction
│   │   │   ├── profileExtractor.ts# Regex-based fallback profile extractor
│   │   │   ├── memoryStore.ts   # In-memory session/state store
│   │   │   └── types.ts         # Shared TypeScript types
│   │   ├── audio/
│   │   │   ├── transcription.ts # Voice note → text (Whisper API)
│   │   │   └── textToSpeech.ts  # Text → audio response
│   │   └── config/
│   │       └── env.ts           # Environment variable validation
│   └── prisma/
│       └── schema.prisma        # PostgreSQL schema (User, Message)
│
└── backend-ai/                  # REST API backend (JavaScript / Express)
    ├── src/
    │   ├── server.js            # HTTP server entry point
    │   └── app.js               # Express app with all routes mounted
    ├── modules/
    │   ├── auth/                # Register, login, JWT auth
    │   ├── users/               # User CRUD
    │   ├── profiles/            # Trader / job-seeker profiles
    │   ├── scores/              # Economic intelligence scoring
    │   ├── payments/            # Squad payment gateway integration
    │   ├── dashboard/           # Aggregated dashboard data
    │   └── webhooks/            # Incoming payment/event webhooks
    ├── middleware/
    │   ├── auth.middleware.js   # JWT verification middleware
    │   ├── error.middleware.js  # Global error handler
    │   └── validate.middleware.js # Zod request validation
    ├── config/
    │   ├── database.js          # Prisma client singleton
    │   └── env.js               # Environment variable loader
    └── prisma/
        └── schema.prisma        # PostgreSQL schema
```

---

## ✨ Features

### WhatsApp Bot (`whatsapp-ai-marketplace`)
- 🤖 **AI-Powered Onboarding** — GPT-4o-mini extracts profession, location, and intent from free-text messages
- 🌍 **Multi-Language Support** — English, Yoruba, Hausa, Igbo, and Nigerian Pidgin
- 🎙️ **Voice Note Support** — incoming voice notes are transcribed using OpenAI Whisper
- 💬 **Interactive Buttons** — language selection and goal prompts use WhatsApp reply buttons
- 🔄 **Multi-Stage Conversation** — `NEW → LANGUAGE_SELECTED → ASKING_TRADE → ASKING_LOCATION → ASKING_GOAL → COMPLETE`
- 📸 **Photo Handling** — users can send photos of their goods/work to attach to their profile
- 🧠 **Regex Fallback** — profile extraction works without OpenAI if the API key is not set
- 🗄️ **PostgreSQL Storage** — all user profiles and message history stored with Prisma

### REST API (`backend-ai`)
- 🔐 **JWT Authentication** — register, login, and protected routes
- 👤 **User & Profile Management** — full CRUD for users and their marketplace profiles
- 📊 **Scores / Economic Intelligence** — scoring system for traders and job seekers
- 💰 **Squad Payment Integration** — payment initiation and verification via Squad API
- 📈 **Dashboard** — aggregated stats and insights
- 🔔 **Webhook Receiver** — handles incoming payment and event notifications
- ✅ **Zod Validation** — all request bodies validated with Zod schemas

---

## 🛠️ Tech Stack

### WhatsApp Bot
| Technology | Purpose |
|---|---|
| Node.js + TypeScript | Runtime & language |
| Express | HTTP server & webhook endpoint |
| OpenAI API (GPT-4o-mini) | AI profile understanding from text |
| OpenAI Whisper API | Voice note transcription |
| Prisma | ORM for PostgreSQL |
| PostgreSQL | Database |
| `ts-node-dev` | TypeScript development runner |

### REST API Backend
| Technology | Purpose |
|---|---|
| Node.js + Express 5 | REST API framework |
| Prisma | ORM for PostgreSQL |
| PostgreSQL | Database |
| bcryptjs | Password hashing |
| jsonwebtoken | JWT auth tokens |
| Zod | Schema validation |
| Axios | HTTP requests to Squad API |
| dotenv | Environment variable management |
| nodemon | Auto-restart in development |

---

## 🚀 Getting Started

### Prerequisites
- Node.js >= 18
- PostgreSQL database
- A [WhatsApp Business API](https://developers.facebook.com/docs/whatsapp/) account (or Meta Cloud API)
- An [OpenAI](https://platform.openai.com/) API key

---

### WhatsApp Bot Setup

```bash
cd whatsapp-ai-marketplace
npm install
```

Create a `.env` file:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/whatsapp_marketplace
OPENAI_API_KEY=your_openai_api_key
WHATSAPP_VERIFY_TOKEN=your_webhook_verify_token
WHATSAPP_ACCESS_TOKEN=your_meta_whatsapp_access_token
WHATSAPP_PHONE_NUMBER_ID=your_phone_number_id
PORT=3000
```

Run database migrations:

```bash
npx prisma migrate dev
```

Start the development server:

```bash
npm run dev
```

---

### REST API Backend Setup

```bash
cd backend-ai
npm install
```

Create a `.env` file:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/backend_ai
JWT_SECRET=your_jwt_secret
SQUAD_SECRET_KEY=your_squad_secret_key
SQUAD_BASE_URL=https://sandbox-api-d.squadco.com
PORT=5000
```

Run database migrations:

```bash
npx prisma migrate dev
```

Start the development server:

```bash
npm run dev
```

---

## 📜 Available Scripts

### WhatsApp Bot
| Command | Description |
|---|---|
| `npm run dev` | Start bot with ts-node-dev (hot reload) |
| `npm run build` | Compile TypeScript to `dist/` |
| `npm run start` | Run compiled production build |
| `npm run prisma:generate` | Generate Prisma client |
| `npm run prisma:migrate` | Run database migrations |

### REST API Backend
| Command | Description |
|---|---|
| `npm run dev` | Start with nodemon (hot reload) |
| `npm run start` | Start production server |

---

## 🗂️ Database Schema

### WhatsApp Bot (`whatsapp-ai-marketplace`)

**User**
| Field | Type | Description |
|---|---|---|
| `id` | String (cuid) | Primary key |
| `phone` | String (unique) | WhatsApp phone number |
| `displayName` | String? | Name from WhatsApp profile |
| `language` | String? | Selected language |
| `trade` | String? | Profession / trade |
| `location` | String? | City / area |
| `intent` | Enum | `TRADER`, `JOB_SEEKER`, `BUSINESS_GROWTH`, `UNKNOWN` |
| `onboardingStage` | Enum | Current onboarding step |
| `wantsJobs` | Boolean | Wants job opportunities |
| `wantsBusinessHelp` | Boolean | Wants business growth help |

**Message** — full message history with direction, type, body, and media URL

---

## 🌐 API Endpoints (`backend-ai`)

| Method | Endpoint | Description |
|---|---|---|
| GET | `/health` | Health check |
| POST | `/api/auth/register` | Register a new user |
| POST | `/api/auth/login` | Login and receive JWT |
| GET | `/api/users` | List all users (protected) |
| GET/PUT | `/api/profiles/:id` | Get / update a profile |
| GET | `/api/scores` | Get economic scores |
| POST | `/api/payments/initiate` | Initiate a Squad payment |
| GET | `/api/dashboard` | Get dashboard stats |
| POST | `/api/webhooks` | Receive external event webhooks |

---

## 🌐 Deployment

- **WhatsApp Bot**: Deploy to [Railway](https://railway.app), [Render](https://render.com), or any VPS. Expose a public HTTPS URL and register it as the Meta webhook endpoint.
- **REST API**: Deploy on the same platform or separately. Set all environment variables in the platform dashboard.

---

## 📄 License

This project is for educational and portfolio purposes.
