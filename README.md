# UX Research Intelligence Tool

A RAG-based pipeline that processes UX interview transcripts and automatically generates structured research outputs — themes, insights, recommendations, research gaps, and contradictions — using LLMs.

Built as a personal project to reduce the manual analysis burden in UX research studies.

---

## What it does

Feed it interview transcripts. Get back a structured research report.

The pipeline runs in 6 stages:
1. **Parse** — handles VTT timestamped transcripts and plain-label formats
2. **Extract insights** — per-participant insight extraction with quotes and context
3. **Synthesise themes** — cross-participant pattern detection with confidence scoring
4. **Generate recommendations** — prioritised, evidence-linked design recommendations
5. **Detect gaps & contradictions** — flags unanswered research questions and conflicting participant views
6. **Executive summary** — concise synthesis for stakeholder sharing

Outputs a structured JSON report and displays results inline in the notebook.

---

## Tech stack

| Component | Tool |
|---|---|
| LLM | Groq API — Llama 3.3 70B (free tier) |
| Embeddings | `all-MiniLM-L6-v2` via `sentence-transformers` (local, no API key needed) |
| Vector DB | Qdrant (local disk persistence) |
| Environment | Jupyter Notebook (Python 3) |
| Credentials | `python-dotenv` |

---

## Setup

### 1. Clone the repo
```bash
git clone https://github.com/grmanasarao/ux-research-intelligence.git
cd ux-research-intelligence
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Get a free Groq API key
Sign up at [console.groq.com](https://console.groq.com) — no credit card needed.

### 4. Create a `.env` file
```
GROQ_API_KEY=your_key_here
```

### 5. Open the notebook
```bash
jupyter notebook UXSearch_Transformer.ipynb
```

Run cells top to bottom.

---

## Input formats supported

- **VTT / timestamped:** `[00:01:23] Speaker: text`
- **Plain label:** `Speaker: text`
- **Inline (Cell 10):** paste raw transcript text directly in the notebook

Participants can be tagged with metadata (role, company size, experience level, etc.) which gets carried through into the analysis.

---

## Output

A `StudyReport` object containing:
- `executive_summary` — plain-English summary
- `themes` — named themes with confidence, participant count, and supporting quotes
- `recommendations` — prioritised actions linked to themes
- `gaps` — unanswered questions with suggested follow-ups
- `contradictions` — conflicting views across participants with quotes
- `all_insights` — full per-participant insight log

Reports are saved as JSON to `./reports/`.

---

## Feedback loop

Cell 8 includes a `submit_feedback()` function for rating and editing generated outputs. Designed as the foundation for a future prompt refinement / DPO loop.

---

## Repo structure

```
ux-research-intelligence/
├── UXSearch_Transformer.ipynb   # Main notebook — run this
├── requirements.txt             # Python dependencies
├── .gitignore                   # Excludes .env, qdrant data, reports
└── README.md
```

---

## Roadmap

- [ ] Fix Jupyter export (replace Colab `files.download` with local save)
- [ ] Add `requirements.txt`
- [ ] Map-Reduce theme synthesis for large transcript sets
- [ ] Chunked handling for long transcripts
- [ ] Provider switching (OpenAI / Anthropic fallback)
- [ ] RLHF-style feedback loop using collected ratings

---

## Notes

- Groq's free tier (Llama 3.3 70B) works well for studies up to ~10 participants
- For larger studies, GPT-4o-mini is the recommended paid alternative
- The `.env` file and `data/qdrant/` folder are excluded from the repo — never commit your API keys
