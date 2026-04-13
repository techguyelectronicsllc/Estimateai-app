# EstimateAI — Voice-First Estimating Platform

A full-stack business tool for Techguy Electronics LLC built to handle estimating, invoicing, procurement, field team management, and visual blueprint/rack planning — all hands-free via voice.

---

## 🚀 Live App

**Frontend:** https://techguyelectronicsllc.github.io/estimateai-app  
**Backend API:** https://estimateai-backend-production.up.railway.app/health

---

## 📋 What It Does

### Phase 1 — AI Voice Estimator
- Record a job description by voice on any device
- Claude AI parses speech into structured line items with quantities and prices
- Live staging area populates in real time as you speak
- Voice corrections — say "change labor to $700" and it updates instantly
- Noise monitoring with decibel alerts for job site environments

### Phase 2 — Payments & Invoicing
- Set payment milestones (deposits, progress payments, final balance)
- Fixed dollar or percentage-based milestone schedules
- Digital signature capture — draw on screen or type name
- Estimate auto-converts to invoice on signature
- Credit card fee pass-through toggle (3%)
- Refund / negative line items styled in red

### Phase 3 — Procurement
- Distributor vault — securely store supplier websites and credentials
- Live pricing — track model numbers and detect price increases
- Automated Purchase Orders grouped by distributor
- One-click email POs directly to vendors
- Voice inventory counts — speak stock levels, AI updates quantities

### Phase 4 — Field Team
- GPS clock-in with geofence verification
- Two-tier access: Admin view (full costs) vs Field view (parts only, no pricing)
- Project journal — site photos, next-tech notes, issue flags
- Live crew status dashboard with on-site indicators
- Hours log with GPS verified badges

### Phase 5 — Visual Tools
- Blueprint takeoff — upload a floor plan, drag and drop equipment
- Rack builder — 1U to 42U drag-and-drop equipment layout
- Per-drawing billable toggle — link placements to estimate or reference only
- Live wattage and amperage tracking for rack builds
- Auto-push placed items to linked estimate

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, JavaScript (single file) |
| Backend | Node.js + Express |
| Database | PostgreSQL via Supabase |
| AI / Parsing | Anthropic Claude claude-sonnet-4-20250514 |
| Speech-to-Text | Browser SpeechRecognition / Deepgram |
| Email | SendGrid (SMTP) |
| Hosting — Frontend | GitHub Pages |
| Hosting — Backend | Railway |

---

## 📁 Repository Structure

### Frontend (this repo)
```
estimateai-app/
  index.html          # Complete unified app — all 5 phases
```

### Backend (separate repo)
```
estimateai-backend/
  server.js           # Express entry point
  routes/
    auth.js           # Register, login, invite techs
    estimates.js      # Full CRUD + sign → invoice
    ai.js             # Claude proxy + Deepgram STT
    field.js          # GPS clock-in, journal, crew status
    email.js          # SendGrid estimate/invoice/PO emails
    other.js          # Clients, invoices, procurement
  middleware/
    auth.js           # JWT verification + role checks
    errorHandler.js   # Global error handling
  db/
    supabase.js       # Supabase client
    schema.sql        # Full database schema (14 tables)
```

---

## 🗄️ Database Schema

14 tables in PostgreSQL via Supabase:

- `companies` — business account
- `users` — employees with role-based access (admin / tech / viewer)
- `clients` — customer records
- `estimates` — estimate documents with signature and voice transcript
- `line_items` — individual items per estimate
- `milestones` — payment schedule per estimate
- `invoices` — auto-generated on signature
- `distributors` — supplier vault with encrypted credentials
- `inventory` — stock levels with low-stock alerts
- `purchase_orders` — POs grouped by distributor
- `po_items` — line items per PO
- `time_entries` — GPS clock-in/out records
- `journal_entries` — field notes and site photos
- `rack_layouts` — saved rack configurations

---

## 🔑 API Endpoints

```
POST   /api/auth/register         Create company + admin account
POST   /api/auth/login            Get JWT token
GET    /api/auth/me               Current user + company
POST   /api/auth/invite           Add a technician

GET    /api/estimates             List all estimates
POST   /api/estimates             Create estimate from voice
POST   /api/estimates/:id/sign    Sign → auto-create invoice

POST   /api/ai/parse-estimate     Transcript → structured estimate
POST   /api/ai/voice-correction   Apply spoken corrections
POST   /api/ai/parse-inventory    Voice stock count → updates
POST   /api/ai/transcribe         Audio → transcript (Deepgram)

POST   /api/field/clock-in        GPS clock-in
POST   /api/field/clock-out       Clock out + log hours
GET    /api/field/crew-status     Live status of all techs
POST   /api/field/journal         Add field note or photo

POST   /api/email/estimate        Send estimate to client
POST   /api/email/invoice         Send invoice to client
POST   /api/email/po              Email PO to distributor
```

---

## ⚙️ Environment Variables

Set these in Railway for the backend:

```
SUPABASE_URL=
SUPABASE_ANON_KEY=
SUPABASE_SERVICE_KEY=
JWT_SECRET=
JWT_EXPIRES_IN=7d
ANTHROPIC_API_KEY=
DEEPGRAM_API_KEY=
SENDGRID_API_KEY=
EMAIL_FROM=
EMAIL_FROM_NAME=
COMPANY_NAME=
COMPANY_EMAIL=
```

---

## 📱 Usage

**As admin:**
1. Open the app URL
2. Go to AI Estimator → tap mic → describe the job
3. Hit Build Estimate → review and edit line items
4. Set payment milestones → get client signature
5. Invoice auto-generates and can be emailed instantly

**As a field tech:**
1. Open the app → toggle to Field View
2. Clock in — GPS verifies you're at the job site
3. See your parts checklist — no costs visible
4. Leave next-tech notes for the crew

---

## 🏢 About

Built for **Techguy Electronics LLC** — Lafayette, Louisiana  
Electronics installation, security systems, home theater, and network infrastructure.

---

*Built with EstimateAI — voice-first estimating for the trades*
