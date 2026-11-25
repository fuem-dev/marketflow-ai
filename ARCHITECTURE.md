# ARCHITECTURE - MarketFlow AI

## Overview

MarketFlow AI is an intelligent agent system that automates answers to customer questions on marketplaces, with mandatory human review.

**Key principle:** AI suggests, you validate, then it gets published.

---

## Flow Diagram

```
┌─────────────────────────────────────────────────────────┐
│         SALES CHANNELS (Marketplaces)                   │
├─────────────────────────────────────────────────────────┤
│  • Mercado Libre Colombia (MVP)                         │
│  • Mercado Libre [other countries] (Phase 2+)           │
│  • eBay (Phase 2+)                                      │
│  • Shopify (Phase 3+)                                   │
└──────────────────┬──────────────────────────────────────┘
                   │ Webhooks
                   ▼
        ┌──────────────────────┐
        │  n8n (Orchestrator)  │
        │ ├─ Receives questions│
        │ ├─ Prepares data     │
        │ ├─ Calls Claude API  │
        │ └─ Stores answers    │
        └──────┬───────────────┘
               │
       ┌───────┴──────────┐
       ▼                  ▼
   ┌────────────┐  ┌─────────────────┐
   │ Claude API │  │    Supabase     │
   │ (Haiku)    │  │   (Database)    │
   │ Generates  │  │  ├─ productos   │
   │ suggested  │  │  ├─ preguntas   │
   │ answers    │  │  └─ respuestas  │
   └────────────┘  └─────────────────┘
       ▲                   ▲
       └───────┬───────────┘
               │
               ▼
    ┌──────────────────────────────┐
    │  Dashboard (Next.js/React)   │
    │  ├─ See pending questions    │
    │  ├─ Review answers           │
    │  ├─ Edit if needed           │
    │  └─ Accept / reject          │
    └──────────────┬───────────────┘
                   │ (you approve)
                   ▼
        ┌──────────────────────┐
        │  Publishes answer    │
        │  on the marketplace  │
        └──────────────────────┘
```

---

## Question Flow (step by step)

1. **Customer asks** on Mercado Libre: "How much is shipping to Bogotá?"
2. **Webhook notifies n8n** with: product_id, question, customer location
3. **n8n runs:**
   - Queries the Supabase DB: product info
   - Queries shipping costs by city
   - Prepares the prompt for Claude
4. **Claude API responds** with a professional suggestion
5. **Saved in Supabase** with status `pendiente_revision` (pending review)
6. **Dashboard notifies** (you see the question)
7. **You review:**
   - Is the answer correct?
   - Does it need editing?
   - Reject it?
8. **You send or reject** from the dashboard
9. **n8n publishes** on Mercado Libre (if you accepted)
10. **History is stored** to improve prompts in the future

---

## Tech Stack

| Layer | Tool | Role | Cost |
|------|-------------|---------|-------|
| **Orchestration** | n8n | Automated workflows | Free (cloud) |
| **AI** | Claude API (Haiku) | Generate answers | ~$0.005/question |
| **DB** | Supabase (PostgreSQL) | Data + REST API | Free tier |
| **Dashboard** | Next.js + React | Review interface | Free (code) |
| **Dashboard hosting** | Vercel | Hosting | Free |
| **n8n hosting** | Railway / VPS | Orchestrator hosting | $5-10/month |
| **Marketplace APIs** | ML / eBay | Receive/send data | Free |

**Total MVP cost (month 1):** ~$0-15

---

## Design Principles

✅ **Mandatory human control:** You always review before publishing
✅ **Low cost:** Pay only for what you use
✅ **Scalable:** Same architecture for multiple countries
✅ **Flexible:** Easy to add new marketplaces
✅ **Safe:** Nothing is published without your validation
✅ **Learns:** Improves with real data
✅ **MVP first:** Working before perfect

---

## Initial Constraints (MVP)

🇨🇴 **Country:** Colombia only
📍 **Geolocation:** By Colombian city
🏪 **Marketplace:** Mercado Libre Colombia
❓ **Question types:** Pre-sale (availability, price, shipping, warranty, features)
⏱️ **Review:** 100% manual (you validate everything)

---

## Planned Evolution

**Phase 1 (MVP):** Colombia, ML, 100% manual review
**Phase 2:** All of Colombia + eBay + more cities
**Phase 3:** Multiple Latin American countries
**Phase 4:** Semi-automatic (simple answers without validation)
**Phase 5:** Almost fully automatic (95%+)

---

## Related Documents

- `DATA_STRUCTURE.md` - Database design
- `PHASE_PLAN.md` - Detailed project phases
- `CLAUDE.md` - Master project instructions
