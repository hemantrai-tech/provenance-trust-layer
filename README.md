# Provenance: A Trust Layer for Research Agents

**Theme:** AI Agents & Intelligent Systems  
**Problem statement:** Tavily Research Agent

Provenance is a proposed trust layer between web retrieval and answer generation. It is designed to help research agents assess whether retrieved claims are independent, current, and safe to use.

> **Submission note:** The supplied project currently contains a visual showcase. The backend pipeline, Tavily integration, trust scoring, and evaluation are not implemented yet. Do not describe the prototype animation or placeholder metrics as measured system behavior.

## Project details

- **Team:** Team information was not provided.
- **Members and roles:** Not provided.
- **Demo video:** No link was provided.
- **Live site:** No link was provided.
- **License:** No license file or license choice was provided.

## Problem statement

The supplied materials identify the hackathon problem as **Tavily Research Agent**, under **AI Agents & Intelligent Systems**. They do not include the portal's exact official problem-statement text. Insert that text verbatim from the hackathon portal before submission.

Our interpretation is that a research agent needs to do more than retrieve pages: it should assess the reliability and provenance of claims before using them in a response.

## Problem

Counting retrieved pages can overstate evidence and miss risks:

1. **Fake consensus:** many articles may repeat one original claim, creating the appearance of independent confirmation.
2. **Indirect prompt injection:** retrieved pages may contain text aimed at controlling the model rather than informing the reader.
3. **Stale information:** older facts can be presented as current, especially on time-sensitive topics.

## Proposed solution

Provenance is designed as a layer between retrieval and generation. Its planned modules are:

| Module | Intended responsibility |
|---|---|
| Injection Shield | Identify hidden or instruction-like page content and quarantine it from answer generation. |
| Origin Tracer | Group copied claims by likely underlying origin so repeated copies do not count as independent evidence. |
| Freshness Judge | Assess dates against the question's time sensitivity and flag stale or contradicted claims. |
| Trust Score | Provide a transparent claim-level score and a plain-language explanation. |

The intended report includes claims, source URLs, likely origins, dates, trust explanations, blocked content, and unresolved uncertainty.

## Current implementation status

| Component | Status |
|---|---|
| Interactive showcase website (`provenance.html` in the supplied materials) | Implemented as a static, single-file prototype with six screens and transitions. |
| Scripted attack demonstration | Animation only; it is not connected to a research backend. |
| Results screen | Placeholder layout; no evaluation results are available. |
| Backend API and Tavily pipeline | Not implemented in the supplied files. |
| Injection Shield, Origin Tracer, Freshness Judge, Trust Score | Design only; not implemented in the supplied files. |
| Evaluation set and baseline comparison | Planned; no results are available. |

## Prototype workflow

The intended end-to-end workflow is:

1. Plan a question into sub-questions and estimate time sensitivity.
2. Retrieve pages using Tavily Search.
3. Extract page content.
4. Scan and quarantine suspicious instructions.
5. Extract atomic claims from cleaned content.
6. Trace claims to likely underlying origins.
7. Assess freshness and contradictions.
8. Score claims with visible reasons.
9. Search again when important claims remain weak or conflicting, with a bounded number of rounds.
10. Return an answer with citations, trust indicators, blocked-threat details, and uncertainty.

This is the planned workflow, not a description of a functioning backend in the supplied prototype.

## Planned architecture

```text
Web showcase / future client
            │ HTTP or SSE
            ▼
      FastAPI orchestrator
       ├─ Planner (LLM)
       ├─ Tavily Search / Extract / Crawl client
       ├─ Injection Shield
       ├─ Claim extractor (LLM)
       ├─ Origin Tracer
       ├─ Freshness Judge
       └─ Trust Scorer
            │
            └─ Tavily API and LLM provider
```

The proposed Python package API is `analyze(question)`, returning a structured research report. This API is not present in the supplied files.

## Proposed trust factors

Initial design weights (heuristics, not calibrated or validated):

| Factor | Initial weight |
|---|---:|
| Independent origins supporting a claim | 0.35 |
| Source authority | 0.25 |
| Fit between claim age and question sensitivity | 0.25 |
| Cross-source agreement or conflict | 0.15 |

Injection flags may cap or remove a claim from use. These factors and weights require implementation and evaluation before they can be treated as system behavior.

## Evaluation plan

The planned evaluation compares Provenance with a naive search-and-summary baseline on 20 cases:

| Category | Cases | Measure |
|---|---:|---|
| Harmless planted injection markers on pages controlled by the team | 7 | Whether injected instructions are followed and whether the shield flags them. |
| Echo chains | 7 | Whether copied claims are grouped under one origin. |
| Stale facts | 6 | Whether newer contradictory information is recognized. |

Cases should be written before tuning; failures should be reported; and results should be reported as counts with the sample size. **No evaluation results currently exist.** Do not fill in success rates until reproducible runs have been completed.

## Running the supplied showcase

Place `provenance.html` in a directory and serve that directory locally:

```bash
python -m http.server 8000
```

Then open <http://localhost:8000/provenance.html>. The supplied HTML is self-contained apart from its external Google Fonts import. No backend is required for the showcase.

## Planned stack

- Python 3.11+, FastAPI, asyncio
- `tavily-python`, `networkx`, `beautifulsoup4`
- An LLM provider that supports structured output
- Vanilla HTML, CSS, and JavaScript for the current showcase

Confirm current Tavily endpoint details and parameters against Tavily's official documentation when implementing the backend.

## Security and limitations

- Injection detection is best-effort and cannot guarantee that all attacks are detected.
- Origin tracing is heuristic; inaccessible or unlinked origins may remain unknown.
- Web publication dates may be absent or incorrect; unknown dates must remain unknown.
- Trust scores are aids to judgment, not proof that a claim is true.
- A 20-case evaluation would be a small sample and would not establish general performance.
- Results depend on the quality of retrieved sources and the chosen LLM.
- Never commit API keys. Keep `.env` out of version control and provide a `.env.example` containing variable names only.

## Planned repository layout

```text
provenance-trust-layer/
├── provenance.html
├── provenance/
│   ├── pipeline.py
│   ├── shield.py
│   ├── origin.py
│   ├── freshness.py
│   ├── scoring.py
│   └── tavily_client.py
├── api/main.py
├── eval/
│   ├── cases.json
│   ├── test_pages/
│   └── run_eval.py
├── .env.example
├── .gitignore
└── README.md
```

This is a target layout, not the current supplied repository structure.

## Team

| Name | Role |
|---|---|
| Team details | Names and roles were not provided. |

## License

No license has been selected or included.

## Acknowledgements

Tavily for its search, extraction, and crawl services; OWASP for prompt-injection security guidance; and the open-source libraries selected for the eventual implementation.


