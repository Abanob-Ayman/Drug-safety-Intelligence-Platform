# SafeSight Drug Safety Intelligence

A responsive React and Vite frontend for exploring reported drug event data. The app currently runs with a clearly labeled illustrative dataset; it is not connected to the project's backend or an AI service.

## Run locally

Requirements: Node.js 18 or newer and npm (or pnpm).

```bash
npm install
npm run dev
```

Open the local address printed by Vite. Create a production bundle with `npm run build`, or preview it with `npm run preview`.

## Demo mode

With no API base URL configured, the app uses `src/data/mockData.js`. Every count and report row in this dataset is synthetic and illustrative. Drug names and RxCUIs are example catalog entries; associated counts and reactions are not official FDA statistics or clinical claims. The assistant clearly reports that it cannot perform grounded analysis while its backend is unavailable.

## Backend integration

Copy `.env.example` to `.env` and set `VITE_API_BASE_URL` to the backend API root. `src/services/api.js` maps the frontend to these expected endpoints:

| Function | Expected endpoint |
| --- | --- |
| `getDashboardMetrics()` | `GET /dashboard/metrics` |
| `searchDrugs(query)` | `GET /drugs?search=...` |
| `getDrugDetails(id)` | `GET /drugs/:id` |
| `getDrugReports(id)` | `GET /drugs/:id/reports` |
| `getAnalytics(filters)` | `GET /analytics?...` |
| `sendChatMessage(message, history)` | `POST /assistant/chat` |
| `getMLResults(filters)` | `GET /analytics/ml?...` |

These routes are an integration placeholder and should be adapted to the backend team's actual API contract. Keep database credentials and AI provider keys on the server; the browser should only call the project's backend. Ensure the backend permits requests from the frontend origin.

## Data sources and interpretation

The planned upstream sources are [openFDA Drug Event API](https://api.fda.gov/drug/event.json) and [RxNorm RxNav REST API](https://rxnav.nlm.nih.gov/REST/). The expected pipeline includes collection, cleaning and name standardization, PySpark processing, PostgreSQL/data warehouse storage, optional machine learning, and optional retrieval-grounded assistant responses.

Spontaneous adverse event reports have limitations and do not independently prove causation or estimate the probability that a patient will experience an event. This platform is informational and does not provide diagnoses or prescribing recommendations.

## Project structure

- `src/pages` are represented as routed page components in `src/App.jsx`.
- `src/components/three/HeroCanvas.jsx` is lazy loaded and renders a lightweight capsule scene with a CSS fallback.
- `src/data/mockData.js` holds synthetic data separately from `src/services/api.js`.
- Tailwind is configured for utility styling; the current interface uses a small custom design system in `src/index.css`.
