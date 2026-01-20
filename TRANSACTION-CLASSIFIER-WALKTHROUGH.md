# Transaction Classifier Agent - Interview Walkthrough

Use this document to guide your discussion of the Transaction Classifier Agent during your Basis interview.

---

## The Problem

**What accountants deal with today:**
- CAS (Client Accounting Services) teams manually categorize hundreds or thousands of bank transactions monthly
- Bank descriptions are cryptic (e.g., "AMZN MKTP US*2K4X7" instead of "Amazon - Office Supplies")
- Each transaction requires human judgment to assign to the correct account in the chart of accounts
- This is time-consuming, repetitive, and error-prone
- Senior accountants' time is spent on data entry instead of advisory work

**Why this matters to Basis:**
- Transaction classification is a core workflow that Basis automates
- This agent demonstrates understanding of the exact problem Basis solves
- Shows I can think from the accountant's perspective (Basis's end user)

---

## The Solution

**A Transaction Classifier Agent that:**
1. Accepts CSV uploads of bank transactions
2. Automatically categorizes each transaction into accounting categories
3. Uses a two-tier approach:
   - **Rule-based classification** for common, predictable patterns (fast, free, explainable)
   - **LLM fallback** for ambiguous transactions (handles edge cases)
4. Provides confidence scores so accountants know which classifications to review
5. Exports classified data for import into accounting software

**Key design decisions:**

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Primary method | Rule-based | Practical - most transactions follow patterns. Rules are fast, free, and explainable |
| Fallback method | LLM (OpenAI) | Handles edge cases without breaking. Shows understanding of when AI adds value |
| Interface | Web-based | No installation required. Easy to demo. Accessible to non-technical users |
| Sample data | Two scenarios | Shows understanding of both generic SMB and accounting firm-specific workflows |

---

## Technical Implementation

### Classification Engine

**How rule-based classification works:**

```javascript
// Each category has keywords and amount direction rules
'Payroll': {
    keywords: ['GUSTO', 'ADP PAYROLL', 'PAYCHEX'],
    excludeKeywords: ['TAX'],  // Separates payroll from payroll tax
    amountDirection: 'negative'  // Must be expense, not income
}
```

**Matching logic:**
1. Check if transaction description contains any keyword
2. Check exclude keywords (prevents false positives)
3. Verify amount direction (positive = income, negative = expense)
4. Score by keyword length (longer = more specific = higher confidence)

**Why this approach:**
- Mirrors how accountants actually think ("I see GUSTO, that's payroll")
- Transparent and auditable (vs. black box ML)
- Fast - no API calls needed for 90%+ of transactions
- Free - no per-transaction costs

### LLM Fallback

**When it triggers:**
- Only for transactions that don't match any rules
- Only if user provides an OpenAI API key

**Why not LLM-first:**
- Cost: API calls add up at scale
- Speed: Network latency for every transaction
- Overkill: Most transactions are predictable
- Explainability: Rules are easier to audit than LLM decisions

**Interview talking point:** "Basis probably uses a similar hybrid approach - AI where it adds value, not everywhere just because they can."

---

## Categories Covered

The classifier handles 20+ categories relevant to CAS workflows:

| Category Type | Examples |
|---------------|----------|
| Revenue | Stripe transfers, client payments, wire transfers |
| Payroll | Gusto, ADP, Paychex (separates base from tax) |
| Software | Productivity (Slack, Zoom), Accounting (QuickBooks, CCH), Dev (GitHub, AWS) |
| Travel | Transportation (flights, Uber), Lodging (hotels, Airbnb), Meals |
| Operations | Office supplies, shipping, rent, utilities |
| Professional | CPE courses, AICPA dues, conferences |
| Banking | Fees, interest income, transfers |

**Accounting firm-specific awareness:**
- E&O insurance (CAMICO)
- Tax software (Lacerte, ProConnect)
- Practice management (Karbon, SafeSend)
- Trust account transactions

---

## Sample Data Strategy

**Created two datasets to demonstrate range:**

### 1. Generic Small Business (50 transactions)
- Stripe revenue, Gusto payroll
- SaaS subscriptions (typical tech stack)
- Travel & meals
- Shows broad applicability

### 2. Accounting Firm (50 transactions)
- Client retainers and invoice payments
- ADP payroll (larger staff)
- Industry-specific software (Thomson Reuters, CCH, Lacerte)
- CPE and professional dues
- Trust account activity
- **Shows I understand Basis's customer**

**Intentionally included "unknown" transactions:**
- "UNKNOWN MERCHANT 847293"
- "UNKNOWN VENDOR 9X7Y2"
- These don't match any rules, demonstrating the LLM fallback value

---

## Key Metrics / Results

When demoing, highlight:

| Metric | Small Business | Accounting Firm |
|--------|----------------|-----------------|
| Total transactions | 50 | 50 |
| High confidence | ~75% | ~75% |
| Needs review | 2-3 | 2-3 |
| Categories used | 15+ | 18+ |

---

## What This Demonstrates About Me

**Technical skills:**
- Can build functional prototypes quickly
- Understand when to use AI vs. simpler solutions
- Write clean, documented code

**Domain knowledge:**
- Understand CAS workflows and pain points
- Know accounting firm-specific tools and terminology
- Grasp the accountant-as-user perspective

**Product thinking:**
- Built for practical use, not just technical demonstration
- Included confidence scores (accountants need to trust but verify)
- Designed exportable output (fits into real workflows)

---

## Questions to Ask / Discuss

**To show deeper thinking:**

1. "How does Basis handle the rule-based vs. LLM balance? I assumed rules-first, but curious about your approach."

2. "What's the hardest transaction type to classify? I imagine things like 'CHECK 1234' with no description are tricky."

3. "Do you find accountants want to see why a transaction was classified a certain way, or just trust the output?"

4. "How do you handle multi-entity scenarios where the same vendor might be categorized differently for different clients?"

---

## Potential Follow-up Projects

If the interviewer asks "what would you build next?":

1. **Bank Reconciliation Agent** - Match bank transactions to recorded entries, flag discrepancies
2. **Journal Entry Generator** - Take business event description, output proper debit/credit entry
3. **Anomaly Detector** - Flag unusual transactions (potential fraud, duplicate payments, etc.)

---

## Demo Script

1. **Open the Transaction Classifier page**
2. **Click "Small Business Transactions"** - show it classifying 50 transactions instantly
3. **Point out the confidence indicators** - green (high), yellow (medium), red (needs review)
4. **Find an "Uncategorized" transaction** - explain this is where LLM would help
5. **Click "Accounting Firm Transactions"** - show industry-specific categories
6. **Click "Export Classified CSV"** - show it outputs data ready for QuickBooks import
7. **Optionally paste an OpenAI key** - show LLM classifying the ambiguous ones

**Time: ~3 minutes**

---

## Files Reference

```
basis-target-map/
├── agents/
│   └── transaction-classifier/
│       ├── index.html                 # Main interface + all logic
│       ├── sample-small-business.csv  # Generic SMB data
│       └── sample-accounting-firm.csv # CPA firm-specific data
└── TRANSACTION-CLASSIFIER-WALKTHROUGH.md  # This file (private)
```

---

*Keep this document private - it's your interview prep, not public documentation.*
