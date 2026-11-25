# MarketFlow AI

> 📚 **Educational project.** A hands-on exercise to learn how to design and build AI agents, n8n automation and solution architecture. It is not an operating business.

MarketFlow AI is a project to build an intelligent agent system for sales, customer service and commercial automation on marketplaces.

The initial goal is an MVP for Mercado Libre Colombia, where AI suggests answers to customers' pre-sale questions and the user reviews them before they are published.

## MVP Goal

Build an initial flow that can:

1. Register products.
2. Register shipping zones.
3. Receive or simulate customer questions.
4. Generate suggested answers with AI.
5. Store the answers.
6. Allow human review before publishing.
7. Prepare the architecture to scale to eBay, Shopify and Latin America.

## Current Status

- **Phase:** 1 (MVP), in progress.
- **Working:** Webhook (n8n) → product lookup (Supabase).
- **In progress:** Claude API call to generate the suggested answer.
- **Next:** store answers and build the review dashboard in Next.js.

## Flow

```
Customer question → n8n webhook → Supabase (product) → Claude (suggested answer)
→ Saved as "pending" → Human review → Publish
```

## Sample Catalog

Refurbished Apple products **with no cellular connectivity** (avoids import restrictions in Colombia):
Apple Watch GPS, iPad Wi-Fi, AirPods and MacBook.

## Initial Scope

- Initial country: Colombia.
- Initial marketplace: Mercado Libre.
- Initial automation: suggested, not automatic.
- Human review: mandatory.
- Database: Supabase.
- Automation: n8n.
- AI: Claude API or an equivalent provider.
- Future dashboard: Next.js, React, TypeScript and Tailwind CSS.

## Project Principle

First, validate the flow in a controlled way.
Then evolve toward partial automation.
Finally, if the system proves reliable, move toward near-full or full automation with clear safety rules.

## Documentation

| File | Content |
|---|---|
| `ARCHITECTURE.md` | System overview and diagram |
| `DATA_STRUCTURE.md` | Current Supabase tables |
| `SUPABASE_SCHEMA.md` | Target normalized data model |
| `PHASE_PLAN.md` | Project roadmap (10 phases) |
| `CONFIRMED_DECISIONS.md` | Decision log and history |
| `PROJECT_STATUS.md` | Phase 0 status snapshot |
| `PHASE0_SUMMARY.md` | Phase 0 summary |
| `MERCADO_LIBRE_API.md` | Mercado Libre integration basics |
| `MARKETPLACE_CONNECTION_GUIDE.md` | Accounts and credentials for eBay and ML |
| `PROMPTS.md` | Initial agent prompts |
| `AUTOMATION_RULES.md` | Safety and automation rules |
| `TEST_PRODUCTS.md` | Test product data |
| `PROJECT_INSTRUCTIONS.md` / `CLAUDE.md` | Instructions for the AI assistant |
| `sessions/` | Work session logs |
| `.env.example` | Environment variable template |

> Database identifiers (`productos`, `preguntas`, `respuestas`, `envios_x_zona`) are in Spanish because they match the live Supabase schema.

## Setup

1. Copy `.env.example` to `.env` and fill in your credentials.
2. Never commit `.env` or n8n exports that contain credentials.

## Author

Fabio Hernández, Senior Frontend Developer.
