# Bank Reconciliation Agent - Interview Walkthrough

Use this document to guide your discussion of the Bank Reconciliation Agent during your Basis interview.

---

## The Problem

**What accountants deal with today:**
- Bank reconciliation is done monthly (or more frequently) for every client
- Accountants manually compare bank statements to recorded transactions in accounting software
- They hunt for discrepancies: missing transactions, duplicate entries, amount differences
- This is tedious, time-consuming, and easy to make mistakes
- When the books don't balance, finding the error can take hours

**Why this matters to Basis:**
- Bank reconciliation is a core CAS workflow
- It's the "moment of truth" - when errors in bookkeeping are discovered
- Automating this saves significant time AND reduces errors
- This agent demonstrates understanding of the reconciliation workflow

---

## The Solution

**A Bank Reconciliation Agent that:**
1. Accepts two CSV uploads (bank statement + book entries)
2. Uses **smart matching** to pair transactions across sources
3. Detects four types of discrepancies:
   - Missing from bank (recorded in books but not on statement)
   - Missing from books (on statement but not recorded)
   - Amount mismatches (same transaction, different amounts)
   - Duplicate entries (same transaction recorded multiple times)
4. Provides **fix suggestions** for each discrepancy
5. Optionally uses LLM for smarter, context-aware suggestions

---

## Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Input method | Two separate file uploads | Clear, explicit - accountants know exactly what's being compared |
| Matching approach | Smart multi-signal scoring | More accurate than single-field matching; handles real-world messiness |
| Discrepancy types | 4 core types | Covers 95%+ of real reconciliation issues |
| Output | Summary cards + drill-down list | Quick overview, then detail on demand |
| AI component | Suggestions only | Matching should be deterministic; AI helps explain fixes |

---

## Technical Implementation

### Smart Matching Algorithm

The agent scores potential matches using multiple signals:

```javascript
function calculateMatchScore(bankTxn, bookTxn) {
    let score = 0;

    // Amount match (most important) - up to 50 points
    if (amounts match exactly) score += 50;

    // Reference number match - up to 30 points
    if (references match) score += 30;

    // Date proximity - up to 15 points
    if (same day) score += 15;
    else if (within 3 days) score += 10;

    // Description similarity - up to 10 points
    score += commonWords * 2;

    return score; // Match if score >= 50
}
```

**Why this approach:**
- Amount alone isn't enough (multiple transactions for same amount)
- Reference numbers are most reliable but not always present
- Date flexibility handles timing differences (checks clearing, pending transactions)
- Description matching catches vendor name variations

### Discrepancy Detection

**Missing from Books:**
- Bank transaction has no match scoring ≥50
- Suggests recording the transaction

**Missing from Bank:**
- Book entry has no match scoring ≥50
- Suggests verifying if transaction cleared or was voided

**Amount Mismatch:**
- Match found but amounts differ
- Shows both amounts and the difference

**Duplicates:**
- Same reference + amount + date appears multiple times in books
- Suggests removing extra occurrences

---

## Sample Data Strategy

**Created two scenarios:**

### Clean Scenario (~25 transactions, 4 issues)
- Mostly matching transactions
- 1 missing from bank (Adobe subscription not cleared)
- 1 missing from books (USPS not recorded)
- 1 amount mismatch (Insurance: $450 vs $425)
- 1 duplicate (Client payment entered twice)

**Purpose:** Shows the agent works accurately on well-organized data

### Messy Scenario (~35 transactions, 12+ issues)
- Accounting firm transactions (relevant to Basis customers)
- Multiple missing transactions in both directions
- Transposed amounts (data entry errors)
- Multiple duplicates
- Varying descriptions for same vendor

**Purpose:** Shows robustness with real-world messy data

---

## Key Metrics

When demoing, highlight:

| Metric | Clean Scenario | Messy Scenario |
|--------|----------------|----------------|
| Bank transactions | 24 | 37 |
| Book entries | 25 | 42 |
| Successfully matched | ~20 | ~25 |
| Discrepancies found | 4 | 12+ |
| Reconciled % | ~83% | ~68% |

---

## What This Demonstrates About Me

**Technical skills:**
- Can implement non-trivial matching algorithms
- Understand scoring/ranking approaches
- Handle edge cases (duplicates, missing data)

**Domain knowledge:**
- Understand bank reconciliation workflow
- Know what discrepancies look like in practice
- Grasp the accountant's mental model

**Product thinking:**
- Summary + drill-down pattern (overview → detail)
- Actionable suggestions, not just flags
- Two scenarios demonstrate different use cases

---

## Comparison to Transaction Classifier

| Aspect | Transaction Classifier | Bank Reconciliation |
|--------|------------------------|---------------------|
| Input | Single CSV | Two CSVs (comparison) |
| Core logic | Classification (categorization) | Matching (pairing) |
| AI role | Classifies ambiguous items | Suggests fixes |
| Output | Categorized list | Discrepancy report |
| User action | Review low-confidence | Resolve discrepancies |

**Together they show:**
- Different types of accounting automation problems
- Both rule-based and AI approaches
- Understanding of multiple CAS workflows

---

## Questions to Ask / Discuss

**To show deeper thinking:**

1. "How does Basis handle reconciliation across multiple bank accounts or entities?"

2. "What's the most common cause of reconciliation failures you see - missing transactions or amount discrepancies?"

3. "Do accountants prefer to resolve discrepancies one-by-one or batch them?"

4. "How does Basis handle bank feeds vs. manual statement uploads?"

---

## Demo Script (3-4 minutes)

1. **Open the Bank Reconciliation Agent page**

2. **Click "Clean Scenario"**
   - Show both upload areas turn green
   - Point out the transaction counts

3. **Click "Reconcile Transactions"**
   - Walk through summary cards: "24 bank transactions, 25 book entries, 20 matched, 4 discrepancies"
   - Highlight the 83% reconciled rate

4. **Click on a discrepancy to expand**
   - Show the details (date, reference, amount)
   - Read the suggested fix

5. **Click "Messy Scenario"**
   - "Now let's try more realistic, messy data"
   - Click Reconcile
   - Show more discrepancies, lower reconcile rate
   - "This is where the agent really saves time"

6. **Optionally show AI suggestions**
   - Paste API key
   - Expand a discrepancy
   - Show the AI-generated suggestion

**Time: ~4 minutes**

---

## Potential Follow-up Discussion

If the interviewer asks "what would you add?":

1. **Auto-categorization of discrepancies** - Group by type, severity
2. **Export reconciliation report** - PDF for client files
3. **Historical comparison** - "This discrepancy happened last month too"
4. **Direct integration** - Connect to QuickBooks/Xero instead of CSV upload

---

## Files Reference

```
basis-target-map/
├── agents/
│   └── bank-reconciliation/
│       ├── index.html                 # Main interface + all logic
│       ├── sample-clean-bank.csv      # Clean scenario - bank data
│       ├── sample-clean-books.csv     # Clean scenario - book data
│       ├── sample-messy-bank.csv      # Messy scenario - bank data
│       └── sample-messy-books.csv     # Messy scenario - book data
└── BANK-RECONCILIATION-WALKTHROUGH.md # This file (private)
```

---

*Keep this document private - it's your interview prep, not public documentation.*
