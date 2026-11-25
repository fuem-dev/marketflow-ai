# Guide: Connecting eBay and Mercado Libre to MarketFlow AI

**Purpose:** Document which accounts and credentials you need for MarketFlow AI to work.

**Audience:** A user starting with no seller accounts.

---

## QUICK SUMMARY

For MarketFlow AI to work, you need:

```
1. An eBay Seller account
2. A Mercado Libre Seller account
3. Credentials (API keys) for both
4. n8n connected to both
5. Supabase with your products
```

---

## PART 1: eBay SELLER ACCOUNT

### What is it?
An eBay seller account lets you:
- List products for sale
- Receive money from buyers
- Access the eBay API (for automation)

### Requirements

| Requirement | Detail |
|-----------|---------|
| **Age** | 18 or older |
| **Country** | Residence in a supported country |
| **Identity** | Identity verification (ID document) |
| **Payment method** | Credit card or bank account |
| **Tax info** | Tax ID or equivalent (varies by country) |
| **Phone** | Phone verification |

### Steps to create an eBay Seller account

1. Go to **https://pages.ebay.com/seller-center/**
2. Click **"Register as a Seller"**
3. Choose the account type:
   - **Individual** (if you sell occasionally)
   - **Business** (if you sell regularly) ← **Better fit**
4. Fill in personal details
5. Add a payment method
6. Verify identity (document + phone)
7. Wait for approval (may take 24-48 hours)

### Cost

| Plan | Monthly cost | Listing limit |
|------|---------------|-------------------|
| Individual | Free | 40 listings/month |
| Business | $27.95/month | Unlimited |

**Recommendation:** Start with Individual, move to Business when you scale.

### Credentials you'll need

Once you have an account:

```
EBAY_USER_ID=your_ebay_username
EBAY_API_KEY=sk_live_xxxx (generate later)
EBAY_API_SECRET=sk_secret_xxxx (generate later)
```

**How to get eBay API keys?**

1. Go to **https://developer.ebay.com/**
2. Log in with your account
3. Click **"Create a Keyset"**
4. Choose "Production"
5. Generate:
   - App ID
   - Cert ID (Certificate)
   - Dev ID

---

## PART 2: MERCADO LIBRE SELLER ACCOUNT

### What is it?
A Mercado Libre seller account lets you:
- List products for sale on ML
- Receive money from buyers
- Access the ML API (for automation)

### Requirements

| Requirement | Detail |
|-----------|---------|
| **Email** | Valid email |
| **Phone** | SMS verification |
| **Identity** | ID document |
| **Country** | Colombia (for ML Colombia) |
| **Payment method** | Bank account or wallet |

### Steps to create an ML Seller account

1. Go to **https://www.mercadolibre.com.co/**
2. Click your profile (top corner)
3. Select **"Vender"** (Sell)
4. Click **"Quiero vender"** (I want to sell)
5. Fill in:
   - Name / business name
   - ID document
   - Phone (SMS verification)
   - Bank details (to receive money)
6. Accept the terms
7. Instant verification or within 24 hours

### Cost

| Item | Cost |
|----------|-------|
| Create an account | Free |
| Publish a listing | Free (up to 20 active listings) |
| Fee per sale | 5-12% (by category) |

### Credentials you'll need

Once you have an account:

```
MELI_USER_ID=your_ml_user
MELI_SELLER_ID=your_seller_id
MELI_API_KEY=APP_USR-xxxx (generate later)
MELI_API_SECRET=APP_USR-secret-xxxx (generate later)
```

**How to get ML credentials?**

1. Go to **https://developers.mercadolibre.com.ar/**
2. Log in with your ML account
3. Click **"Mis aplicaciones"** (My applications)
4. Create a new application:
   - Name: "MarketFlow AI"
   - Redirect to: `https://hook.n8n.cloud/oauth2/callback`
5. Generate:
   - Client ID
   - Client Secret

---

## PART 3: HOW THE PLATFORMS CONNECT

### The complete flow

```
eBay (Warehouse)
    │
    ├─ Product A (stock: 10, price: $50)
    ├─ Product B (stock: 5, price: $30)
    └─ ...
    
    ↓ (MarketFlow AI queries)
    
Mercado Libre (Sales)
    │
    ├─ Lists Product A ($65 = $50 + margin)
    ├─ Lists Product B ($40 = $30 + margin)
    └─ ...
    
    ↓ (Customer asks on ML)
    
MarketFlow AI answers
    │
    ├─ Checks eBay: "Is it in stock?"
    ├─ Generates an answer with AI
    ├─ Saves to Supabase
    └─ You review and publish
```

### Required connections

| From | To | Method | Credential |
|----|---|--------|-----------|
| n8n | eBay API | OAuth 2.0 + API Keys | EBAY_API_KEY |
| n8n | Mercado Libre API | OAuth 2.0 + API Keys | MELI_API_KEY |
| n8n | Supabase | REST API | SUPABASE_URL + ANON_KEY |
| n8n | Claude API | API Key | ANTHROPIC_API_KEY |

---

## PART 4: GETTING STARTED CHECKLIST

### Step 1: Create accounts (3-5 days)
- [ ] Create an Individual eBay account
  - Link: https://pages.ebay.com/seller-center/
  - Time: 1 day
  - Cost: Free

- [ ] Create a Seller account on ML Colombia
  - Link: https://www.mercadolibre.com.co/
  - Time: 1-2 days
  - Cost: Free

### Step 2: Get credentials (1-2 days)
- [ ] Generate eBay API keys
  - Link: https://developer.ebay.com/
  - Requires: verified ID document
  
- [ ] Generate ML API keys
  - Link: https://developers.mercadolibre.com.ar/
  - Requires: verified seller account

### Step 3: Store credentials (1 hour)
- [ ] Create a `.env.example` file in your project
- [ ] Store ALL credentials securely
- [ ] NEVER push `.env` to GitHub

### Step 4: Configure n8n (1-2 days)
- [ ] Create an n8n account
- [ ] Create the initial workflow
- [ ] Connect the eBay API
- [ ] Connect the Mercado Libre API
- [ ] Connect Supabase
- [ ] Initial test

---

## PART 5: .env FILE (Template)

Create a `.env` file in your project (DO NOT push to GitHub):

```bash
# eBay
EBAY_USER_ID=your_ebay_user
EBAY_API_KEY=your_ebay_app_id
EBAY_API_SECRET=your_ebay_cert_id
EBAY_DEV_ID=your_ebay_dev_id

# Mercado Libre
MELI_USER_ID=your_ml_user
MELI_SELLER_ID=your_ml_seller_id
MELI_CLIENT_ID=your_ml_client_id
MELI_CLIENT_SECRET=your_ml_client_secret
MELI_REDIRECT_URI=https://hook.n8n.cloud/oauth2/callback

# Supabase
SUPABASE_URL=https://xxxxx.supabase.co
SUPABASE_ANON_KEY=your_anon_key
SUPABASE_SERVICE_KEY=your_service_key

# Claude API
ANTHROPIC_API_KEY=sk-ant-xxxxx

# n8n
N8N_WEBHOOK_URL=https://hook.n8n.cloud/webhook/xxxxx
```

Also create `.env.example` (DO push to GitHub) without the real values:

```bash
EBAY_USER_ID=
EBAY_API_KEY=
# ... etc
```

---

## PART 6: SECURITY

### NEVER do this:

❌ Store credentials in `.md` files
❌ Push `.env` to GitHub
❌ Share API keys over WhatsApp/email
❌ Store credentials in Slack
❌ Put credentials in visible code

### DO this:

✅ Store them in a local `.env` file
✅ Add `.env` to `.gitignore`
✅ Use environment variables in n8n
✅ Use secrets in Vercel/Railway if you deploy to the cloud
✅ Rotate API keys every 3-6 months

---

## PART 7: ESTIMATED COSTS

### Initial cost (to get started)

| Item | Cost |
|------|-------|
| eBay account (Individual) | Free |
| Mercado Libre account | Free |
| Supabase (free tier) | Free |
| Claude API (first tests) | $5-10 |
| n8n Cloud (free plan) | Free |
| **INITIAL TOTAL** | **~$5-10** |

### Monthly cost (later)

| Item | Cost |
|------|-------|
| eBay (Business if you scale) | $27.95/month |
| Mercado Libre (5-12% fee) | % per sale |
| Supabase | $25/month (after free tier) |
| Claude API | $0.005/question (~$50/1000) |
| n8n | $30-50/month (if you scale) |
| **ESTIMATED TOTAL** | **$80-150/month + fees** |

---

## PART 8: NEXT STEPS

### Recommended order:

1. **This week:** Create eBay and ML accounts
2. **Week 2:** Get API keys
3. **Week 3:** Build the n8n workflow
4. **Week 4:** Phase 1 test

---

## FAQ

### Can I use a personal eBay account?
**Not recommended.** Seller accounts have different limits and protections.

### Do I need starting capital?
**Not to get started,** but you'll need funds to:
- Buy products on eBay (to then sell on ML)
- Pay fees as you grow

### How long does verification take?
- **eBay:** 24-48 hours
- **Mercado Libre:** Instant or up to 24 hours
- **API keys:** Immediate after generating

### What if I'm rejected?
Check:
- Valid (non-expired) document
- Complete and correct information
- Correct country selected
- Valid payment method

Contact the platform's support.

---

## RESOURCES

- **eBay Developers:** https://developer.ebay.com/
- **ML Developers:** https://developers.mercadolibre.com.ar/
- **eBay API Docs:** https://developer.ebay.com/api-docs
- **ML API Docs:** https://developers.mercadolibre.com.ar/es_AR/api-docs
- **n8n Docs:** https://docs.n8n.io/

---

## STATUS

- Document: In use
- Last review: May 25, 2026
- Next review: When the accounts are created
