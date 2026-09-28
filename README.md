# n8n AI Automation Workflows

Two production-style n8n workflows that put an LLM in the middle of a real inbox and a real job search. Both are exported as importable JSON (credentials and IDs removed).

| Workflow | What it does | Nodes (in order) |
|---|---|---|
| [Gmail AI Triage Agent](workflows/gmail-ai-triage-agent.json) | Reads every incoming email, classifies it with Claude, labels/marks it in Gmail and sends an alert to Telegram | Gmail Trigger → Claude (classify) → Switch (route by category) → Gmail label / mark-read nodes → Telegram |
| [AI Job Match Bot](workflows/ai-job-match-bot.json) | Pulls fresh job listings, scores each against a candidate profile with Claude, keeps only strong matches and pushes them to Telegram | Schedule → HTTP Request (JSearch API) → Claude (fit score + reason) → Code (JavaScript: parse/rank) → Filter → Telegram |

## Design notes
- **LLM as a router, not a chatbot.** The model returns a category/score that a `Switch`/`Filter` node acts on, so the rest of the flow stays deterministic and easy to debug.
- **Human in the loop by default.** Results land in Telegram; nothing is sent or applied automatically.
- **Cheap to run.** Small Claude model, one call per email / per listing.

## Run it
1. Import a JSON from `workflows/` into n8n (self-hosted or cloud).
2. Create credentials: Anthropic API key, Gmail OAuth2, Telegram bot (and a RapidAPI key for JSearch).
3. Replace `YOUR_TELEGRAM_CHAT_ID` and, for the job bot, set your own candidate profile in the Claude prompt.

## Related
- [unknownbhaarath](https://github.com/bodavulasiddardha-png/unknownbhaarath) — a fully autonomous, self-healing content agent on GitHub Actions.

Built by [Siddardha Bodavula](https://github.com/bodavulasiddardha-png) · [Portfolio](https://siddu-portfolio-omega.vercel.app)
