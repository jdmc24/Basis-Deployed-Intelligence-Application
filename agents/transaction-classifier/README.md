# Transaction Classifier Agent

An AI-powered agent that automatically categorizes bank transactions into accounting categories. Built as a demonstration of accounting workflow automation.

## How It Works

The agent uses a **two-tier classification approach**:

### Tier 1: Rule-Based Classification
For predictable transactions, the agent uses keyword matching and amount direction rules:

```
"GUSTO PAYROLL" + negative amount → Payroll
"STRIPE TRANSFER" + positive amount → Revenue
"THOMSON REUTERS" + negative amount → Software - Accounting
```

**Why rules first?**
- Fast (no API calls needed)
- Free (no per-transaction costs)
- Explainable (easy to audit why a transaction was categorized)
- Handles 90%+ of typical transactions

### Tier 2: LLM Fallback (Optional)
For ambiguous transactions that don't match any rules, the agent can use OpenAI's GPT to classify:

- Only triggers for unmatched transactions
- Requires user to provide their own API key
- Shows "AI" badge on LLM-classified transactions

## Categories

The agent classifies into 20+ standard accounting categories:

| Category Type | Examples |
|---------------|----------|
| **Revenue** | Stripe transfers, client payments, wire transfers |
| **Payroll** | Gusto, ADP, Paychex (separates base pay from tax deposits) |
| **Software** | Productivity (Slack, Zoom), Accounting (QuickBooks, CCH), Development (GitHub, AWS) |
| **Travel** | Transportation (flights, Uber), Lodging (hotels, Airbnb) |
| **Operations** | Office supplies, shipping, rent, utilities |
| **Professional** | CPE courses, professional dues, conferences |
| **Banking** | Fees, interest income, internal transfers |

## Confidence Scoring

Each classification includes a confidence level:

| Confidence | Meaning |
|------------|---------|
| **High** (green) | Strong keyword match, unambiguous |
| **Medium** (yellow) | Partial match or shorter keyword |
| **Low** (red) | Weak match or LLM-classified, recommend human review |

## Sample Data

Two sample datasets are included to demonstrate the agent:

1. **Small Business Transactions** - Generic SMB: Stripe revenue, Gusto payroll, typical SaaS stack
2. **Accounting Firm Transactions** - CPA firm-specific: client retainers, tax software (Lacerte, CCH), CPE, E&O insurance

## Usage

1. Open `index.html` in a browser
2. Upload a CSV with columns: `date`, `description`, `amount`, `account`
3. Or click a sample data button to test
4. (Optional) Enter OpenAI API key to classify ambiguous transactions
5. Click "Classify X with AI" to process unmatched items
6. Export results as CSV

## CSV Format

Input CSV should have these columns:

```csv
date,description,amount,reference,account
2024-01-02,STRIPE TRANSFER,4250.00,,Business Checking
2024-01-03,GUSTO PAYROLL,-3200.00,PAY-001,Business Checking
```

- **amount**: Positive for income, negative for expenses
- **reference**: Optional (check numbers, invoice IDs)
- **account**: Bank account name

## Technical Details

- Pure HTML/CSS/JavaScript (no build step)
- Runs entirely in browser
- OpenAI API calls made client-side (key never sent to any server except OpenAI)
- Matches the dark theme of the main Basis Target Map dashboard

## Limitations

- Rule-based classification depends on recognizable vendor names in bank descriptions
- LLM fallback requires OpenAI API key and incurs usage costs
- Not intended for production use - demonstration only

---

*Part of the [Basis Target Intelligence Map](../../) project*
