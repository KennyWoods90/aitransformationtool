# ARIA — AI Readiness & Intelligence Agent

> A multi-mode AI agent for enterprise and federal AI transformation strategy. Combines conversational advising, organizational diagnostics, behavioral science-informed coaching, and roadmap generation in a single deployable tool.

---

## Overview

ARIA is built on a single differentiating premise: AI adoption fails because organizations treat it as a technology problem. It is a behavior change problem.

ARIA applies frameworks from Applied Behavior Analysis (ABA), organizational psychology, and AI governance (including NIST AI RMF) to help leaders understand where they are vulnerable, what sequence of interventions will have the most impact, and what happens if they do nothing.

Designed to work in live client conversations, university settings, conference presentations, and independent use.

---

## Modes

### Strategy Advisor
Conversational AI for open-ended AI transformation strategy questions. Responds with a grounded, behavioral science-informed, anti-hype posture. Covers organizational readiness, governance architecture, leadership alignment, and workforce adoption dynamics.

### Org Diagnostic
Structured intake form across six readiness dimensions. Generates a full AI-powered report including:
- Scored dimension cards (Healthy / At Risk / Critical)
- Vulnerability analysis with interdependency diagnosis
- Sequenced interventions with rationale
- "What happens if you do nothing" projection
- First-90-days action recommendations

### Adoption Coach
Behavioral science-informed coaching for workforce AI adoption challenges. Applies ABA principles — motivation operations, competing behaviors, extinction bursts, differential reinforcement — to help practitioners design adoption as an intervention, not a training event.

### Transformation Plan
Generates a phased 12-month AI transformation roadmap from plain-language organizational input. Each plan includes phase names and timeframes, sequenced interventions, governance checkpoints, risk flags, and success metrics.

---

## Architecture

```
aria-deploy/
├── netlify.toml                  # Build config and function directory
├── public/
│   └── index.html                # Frontend application (single file)
└── netlify/
    └── functions/
        └── chat.mts              # Secure Anthropic API proxy
```

**Frontend:** Vanilla HTML / CSS / JavaScript. No framework, no build step. Single self-contained file.

**Backend:** Netlify serverless function (TypeScript). Acts as a secure proxy — the Anthropic API key is stored as a Netlify environment variable and never exposed to the browser.

**AI model:** `claude-sonnet-4-20250514` via Anthropic Messages API.

**Request flow:**
1. User submits a message in any mode
2. Frontend sends `POST /api/chat`
3. Serverless function injects `ANTHROPIC_API_KEY` from environment
4. Request forwarded to Anthropic API
5. Response returned and rendered

---

## Deployment

### Prerequisites
- Netlify account
- Anthropic API key ([console.anthropic.com](https://console.anthropic.com))

### Steps

1. Unzip `aria-deploy.zip`

2. Deploy to Netlify — drag the unzipped folder onto the Netlify deploy drop zone at [app.netlify.com](https://app.netlify.com), or use the CLI:
   ```bash
   netlify deploy --prod --dir=.
   ```

3. Add your API key in Netlify:
   - Go to **Site configuration → Environment variables**
   - Add variable: `ANTHROPIC_API_KEY` = your key
   - Mark as secret
   - Trigger a redeploy

4. Share the live URL — no login required for visitors

Your site will be live at `aria-transformation-agent.netlify.app` (or your custom domain).

---

## Security

- API key stored as a Netlify environment variable — never in client-side code
- `/api/chat` endpoint only accepts `POST` requests
- No user data stored — all conversations are stateless and session-only
- No authentication required by default — add [Netlify Access Control](https://docs.netlify.com/security/secure-access-to-sites/site-protection/) to restrict access if needed

---

## Customization

All customization lives in `public/index.html` — no rebuild needed:

| Element | Where to edit |
|---|---|
| System prompts per mode | `SYSTEM_PROMPTS` object in the `<script>` section |
| Starter chips (suggested prompts) | `starter-chips` divs in each mode panel |
| Diagnostic dimensions | Slider rows in the Org Diagnostic panel |
| Mode names and descriptions | Hero copy and sidebar nav in the HTML |
| Color scheme | CSS variables in `:root` block |
| Brand name | `.brand-name` element in the sidebar |

---

## System Prompt Design

Each mode encodes the following design principles:

- **Behavior change frame:** adoption treated as a behavior change problem, not a communications or training problem
- **Anti-hype posture:** no generic advice, no filler, no recommendations that apply to every organization
- **Advisor voice:** direct, confident, intellectually serious — not an assistant voice
- **Format discipline:** prose over bullet lists; structured outputs for diagnostic reports; under 250 words unless depth is requested

---

## Local Development

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Run locally with function emulation
netlify dev
```

The local dev server emulates the serverless function at `http://localhost:8888/api/chat`. Set `ANTHROPIC_API_KEY` in a `.env` file at the project root.

---

## Roadmap

| Layer | Description | Status |
|---|---|---|
| 1 | Core four-mode agent | Complete |
| 2 | Scenario comparison + benchmark export | Planned |
| 3 | Integration with AI Transformation Dashboard simulation | Planned |
| 4 | Persistent sessions via Netlify Blobs | Planned |
| 5 | Custom org profiles with shareable links | Planned |

---

## License

Private use. Not licensed for redistribution.

---

*ARIA — built on Human-Centered AI Systems Design principles.*
