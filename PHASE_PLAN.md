# PHASE PLAN - MarketFlow AI

## Overall Goal

Build an intelligent agent that suggests answers to customer questions on marketplaces, with mandatory human review in all early phases.

**Future vision:** Scale from Colombia to all of Latin America, and from Mercado Libre to multiple channels.

---

## PHASE 0: Preparation (1-2 days)

**Status:** IN PROGRESS

### Tasks
- [x] Design the overall architecture
- [x] Define the DB structure
- [ ] Document technical decisions
- [ ] Prepare credentials and accounts

### Deliverables
- `ARCHITECTURE.md` ✅
- `DATA_STRUCTURE.md` ✅
- `PHASE_PLAN.md` ✅
- Credentials configured (next)

### Next
Create Supabase and configure the DB

---

## PHASE 1: Minimum Viable Product (5-7 days)

**Goal:** Prove the concept works end-to-end, without polished interfaces.

### Components to Build

#### 1.1 Database (Supabase)
- [ ] Create the Supabase project
- [ ] Create 4 tables (productos, envios_x_zona, preguntas, respuestas)
- [ ] Insert test data (5 products, 5 Colombian cities)
- [ ] Use the auto-generated REST API
- [ ] Configure authentication (if needed)

**Estimate:** 1-2 days

#### 1.2 Claude API + Prompt
- [ ] Create an Anthropic Console account
- [ ] Generate an API key
- [ ] Write the base agent prompt (v1.0)
- [ ] Test answers manually

**Estimate:** 1 day

#### 1.3 n8n - Basic Workflow
- [ ] Set up n8n (cloud or self-hosted)
- [ ] Build the basic workflow:
  - Webhook trigger
  - Query Supabase (product + shipping)
  - Call Claude API
  - Save the answer in Supabase
- [ ] Test with manual requests

**Estimate:** 2-3 days

#### 1.4 Manual Testing
- [ ] Simulate a customer question
- [ ] Verify it reaches Supabase
- [ ] Verify the suggested answer
- [ ] Verify the history

**Estimate:** 1 day

### How you'll see results (no polished UI)
- n8n logs
- Direct Supabase queries
- JSON responses

### Deliverables
- Working Supabase DB with data
- Working basic n8n workflow
- API endpoint documentation
- Documented prompt v1.0

### Success criteria
One test question → suggested answer stored in the DB in <5 seconds

---

## PHASE 2: Dashboard and UI (4-5 days)

**Goal:** A visual interface to review, edit and accept answers.

### Tasks
- [ ] Create the Next.js project
- [ ] Review panel:
  - List of pending questions
  - Suggested answer
  - Product information
  - Buttons: Accept / Edit / Reject
- [ ] Supabase integration
- [ ] New-question notifications
- [ ] Answer history

**Estimate:** 4-5 days (familiar ground with React)

### Deliverables
- Working dashboard on Vercel
- Complete flow: question → you review → accept/reject

---

## PHASE 3: Real Mercado Libre Integration (3-4 days)

**Goal:** Connect to ML for real, not manual requests.

### Tasks
- [ ] Study ML API OAuth
- [ ] Configure webhooks in ML
- [ ] Implement OAuth authentication
- [ ] n8n receives REAL questions from ML
- [ ] n8n publishes answers on ML (after validation)
- [ ] Test with real customers

**Estimate:** 3-4 days

### Deliverables
- Live connection with ML Colombia
- Answers published on ML after your validation

---

## PHASE 4: Prompt Improvement (2-3 days)

**Goal:** Use real data to improve answer quality.

### Tasks
- [ ] Analyze 50+ accepted/rejected answers
- [ ] Identify improvement patterns
- [ ] Create prompt v2.0 with the learnings
- [ ] Test on 20+ new questions
- [ ] Measure acceptance rate (target: >90%)

**Estimate:** 2-3 days

---

## PHASE 5: Geographic Coverage (3-4 days)

**Goal:** Scale to all Colombian cities.

### Tasks
- [ ] Add 20+ Colombian cities to the DB
- [ ] Update shipping costs per city
- [ ] Test questions from different cities
- [ ] Dashboard shows question distribution by city

**Estimate:** 3-4 days

---

## PHASE 6: Semi-Automation (4-5 days)

**Goal:** Simple answers are published automatically. Complex ones go to review.

### Tasks
- [ ] Classify questions by complexity
- [ ] Configure auto-publish rules:
  - "Simple availability" → auto-publish
  - "Shipping" → auto-publish
  - "Complaint" → requires review
- [ ] Dashboard shows what was auto-published and what needs review
- [ ] Error monitoring

**Estimate:** 4-5 days

---

## PHASE 7: Multiple Countries (5-7 days)

**Goal:** Scale to Argentina, Mexico, Peru, etc.

### Tasks
- [ ] Add shipping tables for new countries
- [ ] Configure ML in multiple countries
- [ ] Adapt prompts by country/region
- [ ] Dashboard filters by country
- [ ] Test on 100+ questions

**Estimate:** 5-7 days

---

## PHASE 8: eBay and Shopify (4-5 days)

**Goal:** Same agent, multiple marketplaces.

### Tasks
- [ ] Connect the eBay API
- [ ] Connect Shopify
- [ ] Unify history
- [ ] Dashboard shows each question's channel

**Estimate:** 4-5 days

---

## PHASE 9: Analytics and Learning (3-4 days)

**Goal:** Data to understand what works.

### Tasks
- [ ] Metrics dashboard:
  - Acceptance rate
  - Response time
  - Questions by type
  - Trending topics
- [ ] Identify frequent questions
- [ ] Suggest prompt improvements automatically

**Estimate:** 3-4 days

---

## PHASE 10: Almost Fully Automatic (variable)

**Goal:** 95%+ of answers are published without intervention (optional).

### Consider
- Risks of full automation
- Cases that can NEVER be automated
- Constant monitoring
- Plan B if something fails

---

## Timeline Summary

| Phase | Duration | Estimated End | Status |
|------|----------|--------------|--------|
| Phase 0 | 1-2 days | May 15 | IN PROGRESS |
| Phase 1 | 5-7 days | May 22 | Next |
| Phase 2 | 4-5 days | May 27 | Upcoming |
| Phase 3 | 3-4 days | June 1 | Upcoming |
| Phase 4 | 2-3 days | June 4 | Upcoming |
| MVP total (Phases 0-4) | ~3-4 weeks | June 4 | - |

---

## Key Confirmed Decisions

✅ **Country:** Colombia (MVP) → Latin America (future)
✅ **Marketplace:** Mercado Libre (MVP) → eBay, Shopify (future)
✅ **Review:** 100% manual (MVP) → Semi-automatic (Phase 6)
✅ **Scalability:** DB designed for multiple countries from the start
✅ **Technology:** n8n + Claude + Supabase + Next.js

---

## Identified Risks

| Risk | Impact | Mitigation |
|--------|--------|-----------|
| ML API doesn't respond | High | Automatic retry in n8n |
| Claude API is slow | Medium | Queue in n8n, show "processing..." |
| Out-of-sync data | Medium | Automatic hourly sync |
| Bad AI answer | High | Mandatory review + feedback |

---

## Related Documents

- `ARCHITECTURE.md` - Vision and diagram
- `DATA_STRUCTURE.md` - DB design
- `CLAUDE.md` - Master instructions
