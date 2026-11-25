# CONFIRMED DECISIONS - MarketFlow AI

**Last updated:** May 26, 2026  
**Validated by:** Fabio Hernández  
**Status:** FINAL DEFINITIONS FOR PHASE 1

---

## 🎯 MAIN DECISION: BUSINESS MODEL

### ✅ CHOSEN: MODEL B (Self-Fulfillment)

**For:** MVP and Phase 1  
**Switch to A when:** You have validated winning products and want to scale  
**Never use C for:** Automation (opportunity analysis only)

---

## 📋 FINAL DECISION MATRIX

| Decision | Chosen Option | Reason | Change when |
|----------|----------------|-------|---|
| **Delivery model** | B (Self-fulfillment) | Learn the process, validate products, understand real costs | You have 5+ winning products |
| **Product sourcing** | Manual | Validate the concept first, without automation | You know what to search for automatically |
| **Pricing** | Fixed (constant margin) | Initial simplicity, clear data | You have real demand data |
| **Product range** | Refurbished Apple only | Clear niche, better margin | Viability is proven |
| **MVP timeline** | 2-3 weeks | Fast to validate | N/A |
| **Answer review** | 100% manual | Full control, learn from the AI | Phase 6 (semi-automatic) |

---

## 🔄 MODEL B FLOW (WHAT WE'LL BUILD)

```
1. You list 5 Apple products on Mercado Libre (manually, once)
   ↓
2. Customer asks on ML: "Is that iPhone original?"
   ↓
3. n8n receives the question (webhook)
   ↓
4. n8n queries the DB (product info, shipping)
   ↓
5. Claude generates a suggested answer
   ↓
6. You review in the dashboard: Accept / Edit / Reject?
   ↓
7. If accepted: it's published on ML
   ↓
8. Customer buys → You buy on eBay → You ship to the customer
   ↓
9. AI learns from accepted/rejected answers
```

---

## ⚠️ PREVIOUS MISTAKES (DON'T REPEAT)

❌ **Mistake:** Recommending Model A for the MVP  
✅ **Correction:** Model B is better to validate and learn  

❌ **Mistake:** Assuming you have your own warehouse  
✅ **Correction:** Your model is eBay as warehouse → ML as sales channel  

❌ **Mistake:** Automating product sourcing without validation  
✅ **Correction:** Manual first, automatic later  

---

## 💡 KEY PRINCIPLES

1. **MVP first, scale later** - Don't automate without validating
2. **Mandatory human control** - You validate before publishing
3. **Learn from the real process** - Self-fulfillment shows you what's missing
4. **Low initial cost** - ~$5-15/month, no surprises
5. **Reversible decisions** - Switching to Model A is simple when ready

---

## 📍 IMMEDIATE NEXT STEPS

**Phase 1 (2-3 weeks):**
1. [ ] Create an n8n account
2. [ ] Design the basic workflow
3. [ ] Prepare 5 test products
4. [ ] Manual testing with questions
5. [ ] Basic dashboard (Next.js)

**Phase 2 (After validating):**
1. [ ] Real Mercado Libre integration
2. [ ] Start processing real orders
3. [ ] Collect feedback

**Phase 6 (When there's data):**
1. [ ] Move to semi-automatic
2. [ ] Consider switching to Model A if needed

---

## 🔐 HOW TO USE THIS FILE

✅ **Check at the start of every session** to stay aligned  
✅ **Review before changing decisions** to avoid contradictions  
✅ **Update when new decisions are confirmed**  
❌ **Don't change without an explicit reason**  

---

## OWNER

- **User:** Fabio Hernández
- **Project:** MarketFlow AI
- **Last decision:** May 26, 2026
- **Status:** READY FOR PHASE 1

---

## 🔄 UPDATE - October 30, 2026

### ✅ Product range: REFURBISHED APPLE WATCH ONLY
- **Replaces:** "Refurbished Apple only" (general)
- **Reason:** Cell phones (iPhone) have import restrictions in Colombia (Decree 2025 of 2015): max 1 per courier shipment, and regular resale requires a formal import + IMEI verification/registration.
- **Preference:** **GPS (non-cellular)** models to avoid IMEI issues. GPS + Cellular models have an IMEI → similar risk to phones.
- **Formal legal advice:** not available for now. Validated with free sources (see below).

### Derived to-dos
- [ ] Margin calculator (eBay cost + shipping + taxes + ML fee + customer shipping + 5-10% failure reserve)
- [ ] Written warranty policy for listings (Colombian Law 1480 of 2011)
- [ ] Identify a local Apple Watch repair service
- [ ] Free validation with DIAN / Bogotá Chamber of Commerce

---

## 🔄 UPDATE - November 24, 2026

### Focus: EDUCATIONAL PROJECT
- The goal is to learn to build the system (agents, n8n, Supabase, dashboard), not to validate the business.
- Even so, sample products are chosen **with no special DIAN (customs) restrictions**.

### Allowed catalog (refurbished Apple, no cellular)
| Product | Condition |
|---|---|
| Apple Watch | **GPS** models only (no Cellular) |
| iPad | **Wi-Fi** models only (no Wi-Fi + Cellular) |
| AirPods | All |
| MacBook | All (built-in battery) |

### Excluded
- iPhone (Decree 2025 of 2015)
- Any model with **Cellular / IMEI**
- Loose batteries or uncertified power banks
