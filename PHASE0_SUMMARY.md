# SUMMARY: PHASE 0 - Preparation

## What is MarketFlow AI

MarketFlow AI is a personal intelligent agent system that automates answers to customer questions about products on online sales channels.

**Key concept:** AI suggests answers → you review and validate them → they get published on the channel.

---

## MarketFlow AI's Goal

Reduce customer service time with an agent that:
- Reads frequent customer questions
- Looks up product information
- Suggests professional answers
- Allows human review before publishing

**Benefit:** Less manual work, faster answers, full control over what gets published.

---

## MVP - Initial Constraints

- **Geography:** One region (to keep it simple)
- **Channels:** One sales channel (to start)
- **Question types:** Pre-sale (availability, price, shipping, warranty, features)
- **Review:** 100% manual (you validate everything before publishing)

---

## Selected Tech Stack

| Component | Tool | Cost | Role |
|---|---|---|---|
| Orchestration | n8n | Free (cloud) | Automate workflows |
| AI | Claude API (Haiku) | ~$0.005/question | Generate answers |
| Database | Supabase | Free tier | Store products, questions, answers |
| Dashboard | Next.js + React | Free (code) | Interface to review answers |
| Deploy | Vercel + Railway | Free–$10/month | Host applications |

**Total cost, month 1:** ~$0-15

---

## PHASE 0: Preparation - What did we complete?

### ✅ Step 1: Documentation and Architecture
**Files created:**
- `ARCHITECTURE.md` - Diagram and overview
- `DATA_STRUCTURE.md` - Database design
- `PHASE_PLAN.md` - Project phases with estimates
- `CLAUDE.md` - Master instructions (updated)

**Why it matters**
Clear documentation helps to:
- Understand what is being built
- Have a reference for future improvements
- Communicate the project to other developers

---

### ✅ Step 2: Database
**Completed in Supabase:**
- `productos` table - Info for each product
- `envios_x_zona` table - Shipping costs and times by region
- `preguntas` table - Stores customer questions
- `respuestas` table - Stores suggested and final answers

**Test data:**
- 1 sample product
- 5 regions with shipping costs

**Why it matters**
Without a DB there's nowhere to store data. Supabase has a free tier and scales.

---

### ✅ Step 3: AI Credentials
**Completed in the Anthropic Console:**
- Claude API key obtained
- Ready to use once credits are added

**Why it matters**
Without an API key, n8n can't call Claude to generate answers.

---

### ⏳ Step 4: Understand the Mercado Libre API (PENDING)
**Why it will matter in Phase 1:**
- OAuth: How to connect securely
- Webhooks: How to receive new-question notifications
- Endpoints: How to read/write data on the channel

**Decision:** To be studied during the real integration in Phase 1 (more practical with context).

---

### ⏳ Step 5: Learn Basic n8n (PENDING)
**Why it matters:**
- n8n is the "orchestrator" that connects everything
- Understand how workflows work
- The concepts of "nodes" and "connections"

---

## Complete Flow (What we'll build)

```
1. Customer asks on the sales channel
   ↓
2. Webhook notifies n8n
   ↓
3. n8n queries Supabase (product info)
   ↓
4. n8n calls the Claude API
   ↓
5. Claude responds (suggestion)
   ↓
6. Saved in Supabase as "pendiente_revision" (pending review)
   ↓
7. Dashboard shows you the answer
   ↓
8. YOU review: Accept? Edit? Reject?
   ↓
9. If you accept, n8n publishes on the channel
   ↓
10. Answer stored with feedback for improvement
```

---

## Next Steps

### Phase 0 (Preparation) - IN PROGRESS
- [x] Architecture and documentation
- [x] Database in Supabase
- [x] Claude API key
- [ ] Understand ML API concepts
- [ ] Learn basic n8n

### Phase 1 (Minimum MVP) - NEXT
- [ ] Build a basic n8n workflow (no real integration)
- [ ] Manual question tests
- [ ] Verify everything communicates correctly

### Phase 2 (Dashboard)
- [ ] Interface to review answers
- [ ] Buttons: Accept/Edit/Reject

### Phase 3 (Real Integration)
- [ ] Connect to the real sales channel
- [ ] Receive real questions
- [ ] Publish real answers

---

## Confirmed Architectural Decisions

✅ **Scalable DB:** Designed for multiple regions and channels
✅ **Mandatory human control:** 100% manual review in the MVP
✅ **Low initial cost:** Affordable, scalable stack
✅ **Official technology:** Mainstream tools, not experiments
✅ **Documentation first:** Document before coding

---

## Important Notes

- **Claude credits:** To be added at the first real test (~$5-10)
- **Sensitive data:** Store credentials in a local file, NOT on GitHub
- **Scalability:** The architecture allows adding regions and channels without a redesign

---

## Reference Documents

- `ARCHITECTURE.md` - Full system diagram
- `DATA_STRUCTURE.md` - Detailed SQL table design
- `PHASE_PLAN.md` - Phases with time estimates
- `CLAUDE.md` - Master project instructions
- `PHASE0_SUMMARY.md` - This file (executive summary)

---

## Status

**Date:** May 2026
**Phase:** 0 (Preparation) - 60% complete
**Next:** Learn basic n8n + start Phase 1
