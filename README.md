![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Gemini](https://img.shields.io/badge/Google_Gemini-4285F4?style=for-the-badge&logo=googlegemini&logoColor=white)
![License](https://img.shields.io/badge/License-All%20Rights%20Reserved-red?style=for-the-badge)

<h1 align="center">🗣️ Aapki Awaaz – Every Voice Counts, Every Complaint Ranked</h1>

<p align="center">

  <b>A multilingual, voice-first civic grievance platform powered by Google Gemini.</b><br/>

  Citizens speak. AI understands. Leaders see what matters most.

</p>

---

## 💡 About the Project

**Aapki Awaaz** (Hindi for *"Your Voice"*) is a Civic GenAI Multilingual Prioritization Platform that closes the gap between citizens and the people who serve them.

One app, two very different users:

- 🧑‍🌾 **A villager who can't read or type** gets a home screen that is almost entirely icons, colour, and voice. Tap the big mic, speak in Hindi or English, tap again, and the complaint is sent. No forms, no menus.

- 🏛️ **An MP's staffer or analyst** gets a separate, data-dense **Control Room**: a dark dashboard with adjustable weighting sliders, a live priority ranking, and hotspot tracking.

Behind the scenes, **Gemini 2.5 Flash** reads each complaint (text, dictated speech, or a photo), translates it, categorises it, scores its urgency, and filters out spam. A transparent, weight-based engine then ranks which civic sector deserves attention first.

---

## 🚀 What Aapki Awaaz Can Do

- 🎙️ Voice-first complaint capture in Hindi and English (Web Speech API)

- 🌐 Multilingual understanding: regional languages and Hinglish are translated to English summaries

- 📸 Optional photo attachments, analysed together with the text by Gemini

- 🏷️ Automatic categorisation: Water, Roads, Sanitation, Education, Health, Electricity

- 🚨 AI urgency scoring from 1 (low) to 10 (critical)

- 🛡️ Spam and fake complaint detection before anything reaches the dashboard

- 📍 Hotspot aggregation by category, with report counts and critical incident counts

- ⚖️ Adjustable priority engine: balance citizen demand, demographic deficit, and infrastructure gap

- 🚧 Master Plan conflict flags that hold tasks clashing with local development plans

- 🔊 Read-aloud support (speech synthesis) for non-readers

- 📴 Offline demo fallback, so the UI stays usable when the backend is unreachable

---

## 🛠️ Tech Stack

### Backend

| Tool             | Purpose                                      |

|------------------|----------------------------------------------|

| Python           | Backend language                             |

| FastAPI          | API framework                                |

| Uvicorn          | ASGI server                                  |

| Pydantic         | Request validation and Gemini output schema  |

| google-genai     | Gemini 2.5 Flash integration                 |

| python-multipart | Multipart form and image upload parsing      |

### Frontend

| Tool          | Purpose                                         |

|---------------|-------------------------------------------------|

| Next.js 14    | React framework (App Router)                    |

| React 18      | UI library                                      |

| TypeScript    | Type-safe frontend code                         |

| Tailwind CSS  | Styling                                         |

| Framer Motion | Animations                                      |

| Lucide React  | Icon set                                        |

| Web Speech API| Voice input and read-aloud, built into browsers |

---

## 🧠 How It Works

```

Citizen (voice / text / photo)

        │

        ▼

Next.js frontend ──► POST /api/submissions

                           │

                           ▼

              Gemini 2.5 Flash (structured JSON)

     category · summary · urgency · spam check · landmarks

                           │

                  spam? ───┴─── valid

                   │             │

               rejected     stored + ranked

                                 │

                                 ▼

              GET /api/hotspots   POST /api/prioritize

                                 │

                                 ▼

                    Control Room dashboard

```

### ⚖️ Priority Scoring

Each category receives a composite score out of 100:

```

score = (demand × wDemand) + (demographicDeficit × wDemographic) + (infrastructureGap × wGap)

```

- **Demand** is normalised from the volume of complaints, weighted by severity (urgency ≥ 8 counts 2.5×, 5–7 counts 1.5×, below 5 counts 1×).

- **Demographic deficit** and **infrastructure gap** come from the regional metrics dataset.

- The three weights are set by the user and **must sum to exactly 100**.

- Categories that conflict with the Master Development Plan are flagged `HOLD TASK ORDER`; all others are flagged `EXECUTE PROPOSAL RUNWAY`.

---

### 📁 Project Structure

```bash

Aapki-Awaaz/

│

├── backend/                         # FastAPI service

│   ├── app.py                       # API routes, Gemini pipeline, priority engine

│   └── requirements.txt             # Python dependencies

│

├── frontend/

│   └── civic-connect2/              # Next.js 14 app

│       ├── app/

│       │   ├── (simple)/            # Citizen-facing pages (light theme + bottom nav)

│       │   │   ├── report/          # Record / photo / type → review → send

│       │   │   ├── track/           # "My Reports" as colour-coded cards

│       │   │   ├── layout.tsx

│       │   │   └── page.tsx         # Home: big mic + category tiles

│       │   ├── power/               # MP Control Room (dark, data-dense)

│       │   │   └── page.tsx

│       │   ├── globals.css

│       │   └── layout.tsx

│       │

│       ├── components/

│       │   ├── power/               # Dashboard components

│       │   │   ├── HotspotList.tsx

│       │   │   ├── PriorityTable.tsx

│       │   │   └── WeightSliders.tsx

│       │   ├── BottomNav.tsx

│       │   ├── CategoryTile.tsx

│       │   ├── LanguageToggle.tsx

│       │   ├── MicButton.tsx

│       │   └── PillButton.tsx

│       │

│       ├── lib/

│       │   ├── i18n/                # Hindi / English strings + language context

│       │   │   ├── LanguageContext.tsx

│       │   │   └── translations.ts

│       │   ├── api.ts               # Backend client with offline demo fallback

│       │   ├── categoryMeta.ts      # Colour + icon per civic category

│       │   └── useVoiceCapture.ts   # Web Speech API hook

│       │

│       ├── .env.local.example       # Frontend environment template

│       ├── next.config.mjs

│       ├── package.json

│       ├── postcss.config.js

│       ├── tailwind.config.ts

│       └── tsconfig.json

│

└── README.md

```

---

## ⚙️ Getting Started

To run Aapki Awaaz locally, you need **Python 3.10+**, **Node.js 18+**, and a **Gemini API key** from [Google AI Studio](https://aistudio.google.com/).

### 1. Clone the repository

```bash

git clone https://github.com/Preetesh1/Aapki-Awaaz.git

cd Aapki-Awaaz

```

### 2. Start the backend

```bash

cd backend

python3 -m venv venv

source venv/bin/activate        # Windows: venv\Scripts\activate

pip install -r requirements.txt

```

Set your Gemini API key (the `google-genai` client reads it from the environment):

```bash

export GEMINI_API_KEY=your_gemini_api_key

```

Run the server:

```bash

uvicorn app:app --reload --port 8000

```

**The API will start on `http://localhost:8000`**, with interactive docs at `http://localhost:8000/docs`.

### 3. Start the frontend

In a new terminal:

```bash

cd frontend/civic-connect2

npm install

cp .env.local.example .env.local

```

Point `.env.local` at your backend:

```bash

NEXT_PUBLIC_API_BASE_URL=http://localhost:8000

```

Then run:

```bash

npm run dev

```

**The app will start on `http://localhost:3000`**

> 💡 If the backend is unreachable, the frontend falls back to built-in demo data, so the whole UI stays explorable.

---

### 📱 App Screens

| Route      | Audience | Description                                                  |

|------------|----------|--------------------------------------------------------------|

| `/`        | Citizen  | Home screen with the big mic button and category tiles       |

| `/report`  | Citizen  | Record, photograph, or type a complaint, review it, and send |

| `/track`   | Citizen  | "My Reports" as large colour-coded cards                     |

| `/power`   | MP staff | Control Room with weight sliders, priority table, hotspots   |

---

### 📡 API Endpoints Overview

#### 📬 Submissions (`/api/submissions`)

| Method | Endpoint           | Description                                                                 |

|--------|--------------------|-----------------------------------------------------------------------------|

| GET    | `/api/submissions` | List all active submissions                                                 |

| POST   | `/api/submissions` | Submit a complaint (multipart form), analysed by Gemini                     |

**POST fields:** `text_prompt` (required), `mode` (`Voice`, `Text`, `Photo`, or `WhatsApp`), and an optional `image` file.

Spam or unrelated submissions return `{ "status": "rejected" }` and are not stored.

#### 🗺️ Hotspots (`/api/hotspots`)

| Method | Endpoint        | Description                                                              |

|--------|-----------------|--------------------------------------------------------------------------|

| GET    | `/api/hotspots` | Per-category report count, critical incident count, and centre coordinates |

#### ⚖️ Prioritization (`/api/prioritize`)

| Method | Endpoint          | Description                                                       |

|--------|-------------------|-------------------------------------------------------------------|

| POST   | `/api/prioritize` | Rank all categories using custom weights (must sum to 100)        |

**Example request body:**

```json

{

  "citizenDemand": 40,

  "demographicDeficit": 30,

  "infrastructureGap": 30

}

```

---

## 🎨 Design Notes

- **Solid palette, no gradients:** marigold, deep indigo, peacock, clay, asphalt, and cobalt on a warm paper background. Each civic category owns one colour everywhere, so a non-reader can learn "the green one is water" in a single use.

- **Typography:** Baloo 2 for citizen-facing text (rounded, high legibility, supports Devanagari), Inter for body copy and the dashboard, JetBrains Mono for scores.

- **Signature element:** the oversized mic button with a hard offset shadow and pulsing rings while listening.

- **Two visual registers on purpose:** light and friendly for citizens, dark and dense for analysts.

---

## ⚠️ Known Limitations

- Submissions are stored **in memory**, so they reset whenever the backend restarts. A database such as MongoDB or PostgreSQL is the natural next step.

- Complaint locations are approximated (random coordinates around the region); only landmarks extracted by Gemini are real.

- Regional metrics (demographic deficit, infrastructure gap, Master Plan conflicts) are a static sample dataset.

- Voice is transcribed in the browser with the Web Speech API (best on Chrome, Edge, and Android WebView).

- Hotspots are shown as a ranked list rather than a literal map.

---

## 🔮 Roadmap

- 🗄️ Persistent database storage

- 🗺️ Interactive map with Leaflet or Mapbox

- 🎧 Raw audio upload straight to Gemini

- 🔐 Authentication for the Control Room

- 🌏 More regional languages

---

> 🚫 **Important Notice**

>

> This project is publicly visible **only for demonstration and resume purposes**.

> All rights are reserved by the author. **Unauthorized use, reproduction, or distribution of this code is strictly prohibited.**

> Please do not copy, fork, or reuse this code for commercial or educational purposes without explicit permission.

> 🛡️ Project by Preetesh Sharma | All Rights Reserved
