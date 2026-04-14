# AashrAI - Post-Death Paperwork Agent

> *"The worst day of your life shouldn't also be the most bureaucratic."*

AashrAI is a single-file, browser-based AI agent that helps Indian families navigate post-death paperwork. Enter the deceased's details and assets - five specialized AI agents generate a complete, prioritized action plan with state-specific procedures, interactive checklists, and ready-to-submit letters in the family's preferred language.

---

## Demo

Open `index.html` in any modern browser. No server, no install, no data leaves your device (except the API call to Gemini).

---

## How It Works

AashrAI runs a **5-agent pipeline** on every case:

```
User Input (form)
      ↓
[Agent 1] Core Brain        - Parses the case, generates 8–12 categorized tasks
      ↓
[Agent 2] Bank Agent        - Enriches bank/account tasks with steps & documents
[Agent 3] Govt Agent        - Enriches death cert, Aadhaar, pension tasks
[Agent 4] Insurance Agent   - Enriches LIC / private insurance claim tasks
[Agent 5] Property Agent    - Enriches mutation, succession, vehicle transfer tasks
      ↓
Merge → Dashboard with tasks, checklists, letters
      ↓
[On-demand] Letter Agent    - Writes a formal, ready-to-submit letter per task
```

All 5 agents call **Gemini 2.5 Flash** sequentially — each with a specialized system prompt focused on its domain.

---

## Features

### Core
- **5-agent AI pipeline** — Core Brain plans the task tree; four domain specialists enrich each task with steps, documents, and tips
- **8–12 prioritized tasks** per case — classified as Urgent (≤15 days), This Month (≤45 days), When Ready (≤90 days)
- **4 task categories** — Bank, Govt, Insurance, Property — with color-coded badges

### State-Specific Intelligence
Portals and authorities are auto-injected into every prompt based on the selected state. Supported states:

| State | Death Cert Portal | Property Portal |
|---|---|---|
| Maharashtra | aaplesarkar.mahaonline.gov.in | mahabhulekh.maharashtra.gov.in |
| Delhi | edistrict.delhigovt.nic.in | doris.delhigovt.nic.in |
| Karnataka | nadakacheri.karnataka.gov.in | landrecords.karnataka.gov.in |
| Tamil Nadu | tnreginet.gov.in | eservices.tn.gov.in |
| Telangana | ts.meeseva.telangana.gov.in | dharani.telangana.gov.in |
| Andhra Pradesh | meeseva.ap.gov.in | registration.ap.gov.in |
| Uttar Pradesh | edistrict.up.gov.in | bhulekh.up.nic.in |
| Gujarat | digitalgujarat.gov.in | anyror.gujarat.gov.in |
| Rajasthan | emitra.rajasthan.gov.in | apna.rajasthan.gov.in |
| West Bengal | edistrict.wb.gov.in | banglarbhumi.gov.in |
| Kerala | cr.lsgkerala.gov.in | erekha.kerala.gov.in |
| Madhya Pradesh | mpedistrict.gov.in | mpbhulekh.gov.in |
| Punjab | serviceindia.punjab.gov.in | plrs.org.in |
| Haryana | saralharyana.gov.in | jamabandi.nic.in |
| Bihar | serviceonline.bihar.gov.in | biharbhumi.bihar.gov.in |

### Task Dashboard
- **Priority filter tabs** — All / Urgent / This Month / When Ready
- **Category filter tabs** — All / Bank / Govt / Insurance / Property
- **Per-task info** — deadline, agency, form name, portal link, documents needed, action steps
- **Mark done** — toggle task completion, stats update live

### Interactive Progress Checklist
Every task has an AI-generated checklist of 3–5 physical actions the claimant must take. Checking off items fills a green progress bar. When all items are checked, the task auto-completes.

### Letter Generator
Click **"Generate letter"** on any task — AashrAI writes a complete, formal, ready-to-submit letter including:
- Date and recipient address block
- Subject line
- Formal body with case-specific details
- Signature block
- `[PLACEHOLDER]` markers for unknown specifics (account numbers, policy numbers)

Letters can be **copied to clipboard** or **downloaded as PDF** (A4 format via jsPDF).

### Language Support
Output language can be switched at any time on the dashboard:
- English, Hindi, Marathi, Tamil, Telugu, Kannada

### Session Persistence
The current case (tasks, form data, checklist state) is saved to `localStorage` automatically. On page refresh, a restore banner appears — click **Restore** to continue where you left off. Cases expire after 72 hours.

---

## Getting Started

### 1. Get a Gemini API Key
Go to [aistudio.google.com/apikey](https://aistudio.google.com/apikey) and create a free key.

> The free tier supports limited requests per day. For heavy usage, enable billing in Google AI Studio — Gemini 2.5 Flash is very affordable.

### 2. Open the App
Open `index.html` directly in Chrome, Firefox, Edge, or Safari. No local server needed.

### 3. Enter Your API Key
Paste your key in the banner at the top of the page and click **Save key**. The key is stored in `localStorage` — it never leaves your browser except in API calls to Google.

### 4. Fill the Form
| Field | Required | Notes |
|---|---|---|
| Full name of deceased | Yes | |
| Date of death | Yes | |
| State of residence | Yes | Determines which portals are used |
| Your relationship | Yes | Son/Daughter, Spouse, etc. |
| Your name | Yes | Used in letters |
| Age at death | No | Helps with pension/PF tasks |
| Religion | No | Relevant for succession laws |
| Other survivors | No | e.g. "Spouse, 2 children" |
| Preferred language | No | Default: English |
| Assets | No | Add each asset as a tag - e.g. "SBI savings account", "LIC policy", "Flat in Pune" |
| Additional context | No | e.g. "Government employee", "Had a will" |

### 5. Generate
Click **"Generate action plan →"** - the 5-agent pipeline runs (takes 30–90 seconds) and the dashboard appears.

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI | Vanilla HTML + CSS + JS — zero frameworks, zero build step |
| AI | Gemini 2.5 Flash via Google AI Studio REST API |
| PDF | jsPDF 2.5.1 (CDN) |
| Fonts | Cormorant Garamond, Outfit, JetBrains Mono (Google Fonts) |
| Storage | Browser `localStorage` only |

Everything runs in a single `index.html` file — ~1000 lines total.

---

## Project Structure

```
index.html          - entire app (HTML + CSS + JS in one file)
README.md           - this file
```

---

## Configuration

At the top of the `<script>` block:

```js
const GEMINI_MODEL = 'gemini-2.5-flash';   // change model here
const GEMINI_URL   = `https://generativelanguage.googleapis.com/v1beta/models/${GEMINI_MODEL}:generateContent`;
```

To switch to a different Gemini model (e.g. `gemini-2.0-flash` for lower cost), change `GEMINI_MODEL`.

---

## Limitations

- **Sequential agents** - the 5 agents run one after another (not in parallel) to avoid API rate limits. Total generation time is 30–90 seconds depending on Gemini response speed.
- **Sub-agent failures are non-fatal** - if Bank/Govt/Insurance/Property agents fail (e.g. rate limit), the core tasks from Agent 1 are still shown without enrichment.
- **No backend** - all state is in the browser. Clearing `localStorage` loses the current case.
- **15 states covered** for state-specific intelligence. Other states fall back to generic national procedures.
- **Legal disclaimer** - AashrAI provides procedural guidance only. For complex succession disputes, legal heir issues, or court matters, consult a lawyer.

---

## Roadmap

- [ ] WhatsApp share button for generated letters
- [ ] Multi-case support (multiple family members)
- [ ] Timeline / calendar view of task deadlines
- [ ] Remaining states (Odisha, Jharkhand, Chhattisgarh, etc.)
- [ ] Vernacular voice input (speak the form in Hindi/Marathi)
- [ ] Lawyer/CA referral for flagged complex tasks
- [ ] Direct portal deep-links per task
- [ ] Pre-Death Planning Mode
- [ ] Verified Legal Network
---


*Built with care for Indian families navigating one of life's hardest moments.*
