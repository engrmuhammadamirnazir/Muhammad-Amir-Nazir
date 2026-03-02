---
type: resource
tags: [project, oenoteca, odoo, accounting, reconciliation]
created: 2026-02-25
updated: 2026-03-02
---

# Oenoteca Fine Wines - Bookkeeping & Bank Reconciliation Instructions

**Database:** `oenoteca-fine-wines` (Odoo 19 Enterprise)
**Company:** Oenoteca Fine Wines (A&N Fine Wines LLC dba Oenoteca)
**Data Period:** October 8, 2025 - February 24, 2026
**Prepared:** February 25, 2026

---

## SYSTEM SNAPSHOT

| Item                                               | Value                                |
| -------------------------------------------------- | ------------------------------------ |
| Posted journal entries                             | 630                                  |
| Draft entries (need action)                        | 5                                    |
| Cancelled entries                                  | 6                                    |
| Accounts Receivable balance                        | $340,061.74 (196 unreconciled lines) |
| Accounts Payable balance                           | $22,418.09 (9 unreconciled lines)    |
| Unreconciled bank lines (Bank/BNK1)                | 378                                  |
| Unreconciled bank lines (OENOTECA EXCELLENCE/BNK2) | 33                                   |
| OENOTECA THE LIBRARY (BNK3)                        | Empty - 0 transactions               |
| Registered payments                                | 0                                    |
| Bank sync                                          | Disconnected                         |

---

## PHASE 1: HOUSEKEEPING (Do First - 15 minutes)

### Step 1.1: Process 5 Draft Entries

Go to **Accounting > Journal Entries** > Filter: Status = Draft

| #   | Date       | Journal   | Type             | Amount    | Ref                                              | Action                                                                                                     |
| --- | ---------- | --------- | ---------------- | --------- | ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| 1   | 2026-01-29 | Purchases | Vendor Bill      | -$862.00  | (none)                                           | Review: negative amount is unusual - confirm if this is a credit note from JOANNE USA, then Post or Delete |
| 2   | 2026-01-29 | Purchases | Vendor Bill      | $666.00   | (none)                                           | Review: confirm vendor and details, then Post or Delete                                                    |
| 3   | 2026-02-10 | Sales     | Credit Note      | $333.50   | Reversal of INV/2026/00124 (GARAGE, Item is OOS) | Post if the out-of-stock refund is approved                                                                |
| 4   | 2026-02-20 | Purchases | Vendor Bill      | $0.00     | (none)                                           | Delete - zero amount, likely created in error                                                              |
| 5   | 2026-02-21 | Sales     | Customer Invoice | $3,336.00 | S1633                                            | Review and Post if correct                                                                                 |

### Step 1.2: Review 4 "In Payment" Invoices

Go to **Accounting > Customers > Invoices** > Filter: Payment Status = In Payment

These invoices are marked "in payment" but no actual payment is registered. Investigate:

| Invoice | Date | Customer | Amount | Action |
|---------|------|----------|--------|--------|
| INV/2026/00037 | 2026-01-15 | ENOTECA ROSSA | $120.00 | Check if payment was received. If yes, register payment. If no, reset to "Not Paid" |
| INV/2026/00038 | 2026-01-15 | LA GRIGLIA | $1,284.00 | Same as above |
| INV/2026/00026 | 2026-01-08 | MILTON'S | $396.00 | Same as above |
| INV/2026/00036 | 2026-01-15 | Rosie Cannonball | $174.00 | Same as above |

**Total "in payment" amount: $1,974.00**

---

## PHASE 2: SET UP RECONCILIATION MODELS (30 minutes - saves hours later)

Go to **Accounting > Configuration > Reconciliation Models**

Existing models (keep as-is):
- Internal Transfers (manual trigger)
- Bank Fees (matches "Bank Fees")

### Create These New Models:

#### Model 1: Fintech Customer Payments (biggest category - 89 lines)
- **Name:** Fintech Customer Payments
- **Type:** Match existing entries (find matching invoices)
- **Match Label:** Contains `A&N Fine Wines`
- **Match Amount:** Amount received (tolerance 0%)
- **Auto-reconcile:** No (requires manual invoice matching)

#### Model 2: Zelle Customer Payments
- **Name:** Zelle Customer Payments
- **Type:** Match existing entries
- **Match Label:** Contains `Zelle payment from`
- **Auto-reconcile:** No

#### Model 3: Square POS Payments
- **Name:** Square POS Payments
- **Type:** Match existing entries
- **Match Label:** Contains `Square Inc`
- **Match Nature:** Amount received (positive only)
- **Auto-reconcile:** No

#### Model 4: Amex Card Payments
- **Name:** Amex Card Payments
- **Type:** Write-off (create a journal entry)
- **Match Label:** Contains `AMERICAN EXPRESS`
- **Account:** Create or use a "Credit Card Payable - Amex" liability account
- **Auto-reconcile:** Yes

#### Model 5: WISE International Wires (Enohance Srl)
- **Name:** WISE International Wire
- **Type:** Write-off
- **Match Label:** Contains `WIRE TRANSFER` AND `WISE`
- **Account:** Accounts Payable (211000)
- **Partner:** Enohance Srl
- **Auto-reconcile:** No (amounts need manual matching to bills)

#### Model 6: AT&T Phone Bill
- **Name:** AT&T Bill
- **Type:** Write-off
- **Match Label:** Contains `ATT`
- **Account:** Miscellaneous Expenses (690000) or create "Telecom Expense"
- **Partner:** AT&T (partner ID 3789, already exists)
- **Auto-reconcile:** Yes

#### Model 7: Insurance (Progressive)
- **Name:** Insurance Premium
- **Type:** Write-off
- **Match Label:** Contains `PROG COUNTY`
- **Account:** Miscellaneous Expenses (690000) or create "Insurance Expense"
- **Auto-reconcile:** Yes

#### Model 8: Rent (Westdale)
- **Name:** Office Rent
- **Type:** Write-off
- **Match Label:** Contains `WESTDALE`
- **Account:** Miscellaneous Expenses (690000) or create "Rent Expense"
- **Auto-reconcile:** Yes

#### Model 9: Vehicle Lease (Ford Credit)
- **Name:** Vehicle Lease
- **Type:** Write-off
- **Match Label:** Contains `LINCOLN AFS` OR `FORDCREDIT`
- **Account:** Miscellaneous Expenses (690000) or create "Vehicle Expense"
- **Auto-reconcile:** Yes

#### Model 10: Chase Credit Card Payment
- **Name:** Chase CC Payment
- **Type:** Write-off
- **Match Label:** Contains `Payment to Chase`
- **Account:** Create "Credit Card Payable - Chase" liability account
- **Auto-reconcile:** Yes

#### Model 11: Odoo Subscription
- **Name:** Odoo Subscription
- **Type:** Write-off
- **Match Label:** Contains `ODOO INC`
- **Account:** Miscellaneous Expenses (690000) or "Software Subscriptions"
- **Auto-reconcile:** Yes

---

## PHASE 3: BANK RECONCILIATION - BANK (BNK1) - 378 Lines

Go to **Accounting > Bank > Bank**

Work through in this exact order for maximum speed:

### Step 3.1: Internal Transfers (24 outgoing + 4 incoming = ~28 lines)

**What to look for:** Lines containing "Online Transfer to/from CHK"

These are transfers between bank accounts. Match outgoing BNK1 transfers to incoming BNK2 transfers and vice versa. Use the existing "Internal Transfers" reconciliation model.

| Pattern | Direction | Count | Total |
|---------|-----------|-------|-------|
| `Online Transfer to CHK` | Outgoing | 24 | -$40,195.55 |
| `Online Transfer from CHK` | Incoming | 4 | +$22,358.73 |

**Action:** For each, select "Internal Transfers" model. Match to the corresponding entry on the other bank journal.

### Step 3.2: Recurring Auto-Match Expenses (~65 lines)

These should auto-match once reconciliation models are set up:

| Category                | Count | Total       | Model to Use                      |
| ----------------------- | ----- | ----------- | --------------------------------- |
| Amex Card Payments      | 16    | -$84,623.44 | Amex Card Payments                |
| Chase CC Payments       | 6     | -$7,176.04  | Chase CC Payment                  |
| AT&T Bills              | 5     | -$234.35    | AT&T Bill                         |
| Insurance (Progressive) | 3     | -$2,248.77  | Insurance Premium                 |
| Rent (Westdale)         | 4     | -$5,369.85  | Office Rent                       |
| Vehicle Lease (Ford)    | 3     | -$1,490.49  | Vehicle Lease                     |
| Bank Service Charges    | 3     | -$150.00    | Bank Fees (existing)              |
| Shell Gas               | 1     | -$46.32     | Write-off to Fuel/Vehicle Expense |
| Odoo Subscription       | 2     | -$130.26    | Odoo Subscription                 |

**Action:** Open each line, the reconciliation model should auto-propose. Click Validate.

### Step 3.3: Fintech Customer Payments - INFLOWS (89 lines, +$421,500)

**These are the bulk of income.** Fintech.com (registered as "A&N Fine Wines LLC") sweeps accumulated customer payments into the bank account.

**Key fact:** Each Fintech deposit is a BATCH of multiple customer invoice payments. The deposit amount will NOT match a single invoice. You need to:

1. Open the bank line (shows "A&N Fine Wines" in description)
2. In the reconciliation widget, search for customer invoices by amount
3. Select MULTIPLE invoices that sum up to (or near) the deposit amount
4. If there's a small difference, it may be a Fintech processing fee - write off to "Bank Fees" or "Fintech Processing Fees"

**Tip:** Sort by date. Cross-reference the Fintech report file (`FINTECH REPORT 100825 TO 13126.xlsx` in Accounting Working folder) which shows which customer invoices each deposit covers.

**Monthly breakdown:**

| Month | Lines | Approx Total |
|-------|-------|-------------|
| Oct 2025 | 21 | +$115,000 |
| Nov 2025 | 18 | +$81,000 |
| Dec 2025 | 17 | +$72,000 |
| Jan 2026 | 17 | +$85,000 |
| Feb 2026 | 16 | +$68,500 |

### Step 3.4: Republic National Debits (6 lines, -$2,321.28)

**What these are:** Fintech platform deduction fees by Republic National Distributors.

**Action:** Write off each to "Bank Fees" or create a specific "Fintech/Distribution Fees" expense account.

### Step 3.5: Fintech.net Fees (5 lines, -$401.78)

**What these are:** Monthly Fintech.net platform fees.

**Action:** Write off to "Bank Fees" or "Fintech Processing Fees" expense account.

### Step 3.6: Fintech Verification Debits (4 lines, -$3.15 total)

Tiny amounts ($1.00, $0.15) for Fintech account verification.

**Action:** Write off to "Bank Fees."

### Step 3.7: WISE Payments - Small/Invoice-Specific (26 lines, -$69,702.95)

**What these are:** Payments via WISE to European suppliers/individuals. The WISE reference often includes the invoice number:

| Reference Pattern | Meaning | Likely Payee |
|-------------------|---------|-------------|
| `Invoice 169/202`, `Invoice 176/202`, `Invoice 179/202`, etc. | Enohance Srl invoice payments | Enohance Srl |
| `WAGE NOVEMBER`, `Wages October` | Andrea Graneris salary (Italy partner) | Partner draw or Wage expense |
| `VISA FEE` | Visa processing fee | Miscellaneous Expense |
| `Truffle for per` | Business expense | Miscellaneous Expense |
| `Invoice 1 11/4/`, `Invoice 2 11/13` | Specific supplier invoices | Match to Enohance bills |
| `Invoice 22/2025`, `Invoice 200`, `invoices 187 an` | Batch Enohance payments | Match to Enohance bills |

**Action for Enohance invoices:** Match to the corresponding vendor bill in Accounts Payable. The WISE reference has the Enohance invoice number.

**Action for wages/expenses:** Write off to appropriate expense accounts:
- Wages → "Salary Expense" or "Partner Draws"
- VISA FEE → "Bank Fees"
- Other → "Miscellaneous Expenses (690000)"

### Step 3.8: WISE International Wire Transfers (12 lines, -$154,304.06)

**What these are:** Large wire transfers to WISE US, Inc. for payments to Enohance Srl (Italian supplier).

**Action:** Match to Enohance Srl vendor bills (BILL/2026/02/0005: $11,118.80, BILL/2026/02/0006: $4,009.38, etc.). These are large batches - a single wire may cover multiple Enohance invoices.

### Step 3.9: Vendor ACH Payments (32 lines, -$130,019)

| Vendor | Lines | Total | Action |
|--------|-------|-------|--------|
| GRW Wine | 9 | -$36,479.43 | Match to GRW WINE vendor bills |
| Joanne USA | 4 | -$16,402.00 | Match to JOANNE USA vendor bills |
| Walter Hansel | 1 | -$7,632.00 | Match to vendor bill (create if missing) |
| Flickinger/Ventoux Wines | 1 | -$1,044.00 | Match to Ventoux Fine Wine bill (BILL/2026/02/0009: $8.00 exists, but ACH is $1,044 - likely need new bill) |
| Other vendors | 17 | -$60,697.61 | Review individually - these are ACH payments without named vendor |

**For the 17 "Other" ACH vendor payments:** The bank reference may say "Online ACH Payment" with just an account number. You need to:
1. Check the amount against open vendor bills
2. If no matching bill exists, ask Niccolo which vendor was paid
3. Create a vendor bill if needed, then match

### Step 3.10: Zelle Customer Payments - INFLOWS (19 lines, +$24,331.81)

Zelle payments from customers. The reference includes the sender's name:

| Bank Line Name | Likely Customer Match | Amount |
|----------------|----------------------|--------|
| `Zelle payment from LUCAS A GARIBALDI` | Lucas Garibaldi | $378.00 |
| `Zelle payment from JEFFREY CORBETT` | Jeff Corbett (INV/2026/00145: $467.64) | $467.64 |
| Various others | Match by name/amount to open invoices | Various |

**Action:** For each Zelle inflow, match the sender name to a customer in the system, then select the matching open invoice(s).

### Step 3.11: Square POS Payments - INFLOWS (12 lines, +$13,502.28)

Square payments are batch deposits from point-of-sale transactions. Like Fintech, one deposit may cover multiple sales.

**Action:** Match to customer invoices by date and amount. Check if the Square payment corresponds to walk-in/retail sales vs. specific customer invoices.

### Step 3.12: Remote Check Deposits - INFLOWS (12 lines, +$13,793)

Lines show "REMOTE ONLINE DEPOSIT # 1" - these are check deposits.

**Action:** Match to customer invoices. The check amount should match an open invoice amount. Work through by matching amounts.

### Step 3.13: Zelle Payments OUT (22 lines, -$6,348.98)

Outgoing Zelle payments to individuals:

| Recipient | Count | Total | Likely Category |
|-----------|-------|-------|----------------|
| JUSTIN E SANDERS | 5 | ~-$685 | Contractor/Service |
| JASMINE | 3 | ~-$282 | Contractor/Service |
| Jonathan Geffrard (JEG ADVISOR) | 3 | ~-$3,223 | Professional advisor |
| DAVIS | 1 | -$1,000 | Ask Niccolo |
| Logan Wynn | 1 | -$483.75 | Contractor/Service |
| Jeffrey Gold | 1 | -$48.90 | Ask Niccolo |
| Eric Hastings | 1 | -$18.60 | Ask Niccolo |
| Others | Various | Various | Ask Niccolo |

**Action:** Write off each to "Contractor Expense" or "Professional Services" or ask Niccolo for categorization.

### Step 3.14: Cash Withdrawals (12 lines, -$5,660)

All show "WITHDRAWAL" - cash taken from account.

**Action:** Write off to "Owner Draws" or "Petty Cash" depending on business policy. Confirm with Niccolo.

### Step 3.15: Check Payments (5 lines, -$4,800.50)

Lines show "CHECK # XXX" - outgoing checks.

**Action:** Match to vendor bills or ask Niccolo for check register details.

### Step 3.16: Other Vendor Payments via Fintech (11 lines, -$6,085.19)

| Vendor | Lines | Total | Action |
|--------|-------|-------|--------|
| Johnson Brothers | 4 | -$4,356.00 | Write off to COGS or match to bills if available |
| Rootstock LLC | 6 | -$1,729.19 | Write off to COGS or match to bills |
| Favorite Brands | 1 | -$606.00 | Write off to COGS or match to bills |

### Step 3.17: Other Operating Expenses (~15 lines)

| Item | Lines | Total | Account |
|------|-------|-------|---------|
| QuickBooks/Intuit | 3 | -$367.77 | Software Subscriptions expense |
| Square Fees (debits) | 4 | -$958.20 | Bank Fees / Payment Processing Fees |
| Merchant Settlement (EPX) | 4 | -$318.80 | Payment Processing Fees |
| Creative Supply | 2 | -$4,973.93 | Supplies or COGS - ask Niccolo |
| IRS Tax Payment | 2 | -$893.00 | Tax Expense (Note: paid for Andrea Graneris) |
| Payroll | 2 | -$6,500.00 | Salary/Wages Expense |
| State Farm | 1 | -$232.08 | Insurance Expense |
| Vencru Inc. | 1 | -$135.00 | Software/SaaS (plus $135 reversal + $135 re-charge = net $135) |
| GRW RealTime Payment | 1 | -$2,259.05 | Match to GRW Wine vendor bill |
| Flickinger Wines | 1 | -$1,044.00 | Match to vendor bill or create one |

### Step 3.18: Other Inflows (~16 lines, +$40,752.11)

| Source | Amount | Action |
|--------|--------|--------|
| AFTERMATH WINE (Oct 17) | +$7,098.00 | Customer payment - match to invoice |
| WINE CELLARS LTD Fedwire (Nov 18) | +$7,263.00 | Customer payment - match to Wine Cellars Ltd invoices |
| WINE CELLARS LTD Fedwire (Nov 26) | +$5,673.00 | Customer payment - match to Wine Cellars Ltd invoices |
| BOTTEGA SIRVINO LLC Fedwire (Jan 7) | +$13,874.00 | Customer payment - match to invoices (large amount) |
| MAISON GIGI LLC (Oct 16, Oct 17, Jan 7) | +$4,952.00 | Customer payments - possibly related to Enoteca Rossa or another client |
| TOSLA D.O.O. Slovenia (Dec 22, Jan 5) | +$500.00 | Incoming payment from Slovenia - ask Niccolo |
| Gemstone Vineyards (Oct 16) | +$273.49 | Customer payment or vendor refund |
| Johnson Brothers inflow (Jan 1) | +$720.00 | Refund or credit from Johnson Brothers |
| Cash Redemptions (3x) | +$128.62 | Coin/cash machine redemptions - write off to "Other Income" |
| Vencru reversal (Jan 6) | +$135.00 | Matches the Vencru charge reversal |

---

## PHASE 4: BANK RECONCILIATION - OENOTECA EXCELLENCE (BNK2) - 33 Lines

Go to **Accounting > Bank > OENOTECA EXCELLENCE**

### BNK2 Transaction Categories:

| Category | Count | Total | Action |
|----------|-------|-------|--------|
| Remote Check Deposits | 19 | +$6,741.00 | Match to customer invoices by amount |
| Transfers from BNK1 (Online Transfer from CHK ...5820) | 5 | +$12,501.00 | Internal transfer - match to BNK1 outflows |
| Transfers to other accounts (Online Transfer to CHK) | 1 | -$120.00 | Internal transfer |
| Chase CC Payments (Payment to Chase card 7189/8982) | 4 | -$3,056.91 | Write off to Chase CC liability |
| ACH to Walter Hansel | 1 | -$8,808.00 | Match to vendor bill |
| Creative Supply debits | 2 | -$2,866.45 | Match to vendor bills or expense |
| REMOTE ONLINE DEPOSIT (customer payments) | Various | Various | Match to invoices |

### BNK2 Specific Lines to Reconcile:

**Inflows (deposits into OENOTECA EXCELLENCE):**

Most are "REMOTE ONLINE DEPOSIT # 1" - check deposits. Amounts range from $87 to $833. Match each to a customer invoice by amount:
- $768, $174, $120, $87, $833, $120, $648, $198, $561, $216, $261, $198, $198, $570, $375, $198, $198, $174, $576, $198

**Outflows:**
- Chase card ending 8982: -$797.57 (Oct 24)
- Chase card ending 7189: -$1,117.79 (Nov 20), -$500.00 (Dec 16), -$641.55 (Jan 31)
- Walter Hansel ACH: -$8,808.00 (Dec 1) - match to vendor bill
- Creative Supply: -$735.00 (Jan 10), -$2,131.45 (Feb 11) - expense or vendor bill
- Transfer to CHK ...6913: -$120.00 (Nov 3) - internal transfer

---

## PHASE 5: VENDOR BILLS - MISSING BILLS (1 hour)

### Currently Posted Vendor Bills (9 unpaid, $20,301.73):

| Bill | Date | Vendor | Amount | Action |
|------|------|--------|--------|--------|
| BILL/2026/02/0005 | 2026-02-16 | Enohance Srl | $11,118.80 | Match to WISE wire |
| BILL/2026/02/0006 | 2026-02-16 | Enohance Srl | $4,009.38 | Match to WISE wire |
| BILL/2026/01/0002 | 2026-01-29 | JOANNE USA | $2,484.00 | Match to Joanne ACH |
| BILL/2026/02/0001 | 2026-02-10 | GRW WINE | $1,403.55 | Match to GRW ACH |
| BILL/2026/01/0001 | 2026-01-29 | JOANNE USA | $862.00 | Match to Joanne ACH |
| BILL/2026/02/0003 | 2026-02-16 | Enohance Srl | $300.00 | Match to WISE wire |
| BILL/2026/02/0004 | 2026-02-16 | Enohance Srl | $62.00 | Match to WISE wire |
| BILL/2026/02/0007 | 2026-02-19 | JOANNE USA | $54.00 | Match to Joanne ACH |
| BILL/2026/02/0009 | 2026-02-20 | Ventoux Fine Wine | $8.00 | Match to Flickinger ACH |

### Bills That Need to Be CREATED:

The following bank payments have NO matching vendor bill. Create bills first, then reconcile:

| Bank Payment | Amount | Vendor | Notes |
|-------------|--------|--------|-------|
| ACH to GRW (9 payments) | $36,479.43 total | GRW WINE | Only 1 bill exists ($1,403.55). Need ~8 more bills. Check Customer Invoices folder for GRW invoices |
| ACH to Joanne USA (4 payments) | $16,402.00 total | JOANNE USA | Only 2 bills exist ($3,346). Need more bills |
| ACH to Walter Hansel (2 payments) | $16,440.00 total | Create "WALTERHANSEL" vendor | No bills exist. Need purchase records |
| WISE wires to Enohance (38 payments) | ~$224,007 total | Enohance Srl | Only 4 bills exist ($15,490.18). Need many more bills. Check Vendor Bills folder |
| Johnson Brothers (4 payments) | $4,356.00 total | Create "JOHNSON BROTHERS" vendor | No bills exist |
| Rootstock LLC (6 payments) | $1,729.19 total | Create "ROOTSTOCK LLC" vendor | No bills exist |
| Favorite Brands (1 payment) | $606.00 | Create "FAVORITE BRANDS" vendor | No bill exists |
| Flickinger Wines (1 payment) | $1,044.00 | Ventoux Fine Wine (Flickinger) | Only $8 bill exists |

**Vendor bills source:** Check:
- `Vendor Bills/` folder (3 subfolders with PDF invoices)
- `Oenoteca_Complete_Vendor_Bills_Import (1).xlsx`
- `Customer Invoices/` folder (GRW Wine Collection Invoice #58961.pdf)

---

## PHASE 6: MISSING EXPENSE ACCOUNTS (15 minutes)

The chart of accounts is minimal. Create these accounts for proper expense categorization:

Go to **Accounting > Configuration > Chart of Accounts** > Create:

| Code | Name | Type | Suggested |
|------|------|------|-----------|
| 620000 | Rent Expense | Expense | For Westdale payments ($1,342/mo) |
| 621000 | Insurance Expense | Expense | For Progressive + State Farm (~$830/mo) |
| 622000 | Vehicle Expense | Expense | For Ford lease (~$497/mo) |
| 623000 | Telecom Expense | Expense | For AT&T (~$47/mo) |
| 624000 | Software & SaaS | Expense | For Odoo, QuickBooks, Vencru |
| 625000 | Payment Processing Fees | Expense | For Fintech fees, Square fees, EPX merchant settlement |
| 626000 | Professional Services | Expense | For JEG Advisor, contractors (Zelle payments) |
| 627000 | Salary & Wages | Expense | For payroll + Andrea Graneris wages via WISE |
| 628000 | Tax Expense | Expense | For IRS payments |
| 212000 | Credit Card Payable - Amex | Current Liability | For Amex card balance |
| 213000 | Credit Card Payable - Chase | Current Liability | For Chase card balance |

---

## PHASE 7: VERIFICATION CHECKLIST (After Reconciliation)

### 7.1: Bank Balance Check

After reconciling all lines, verify:

| Journal | Expected GL Balance | Compare Against |
|---------|-------------------|-----------------|
| Bank (BNK1) | Current: $10,354.37 + reconciled adjustments | Actual bank statement balance as of Feb 20, 2026 |
| OENOTECA EXCELLENCE (BNK2) | Current: $4,446.64 + reconciled adjustments | Actual bank statement balance |

**The GL balance after reconciliation MUST match the actual bank statement balance.**

### 7.2: Accounts Receivable Verification

After matching customer payments to invoices:
1. Go to **Accounting > Reporting > Aged Receivable**
2. The remaining unpaid invoices should only be genuinely unpaid ones
3. Current total: $340,061.74 across 66 customers
4. After matching Fintech deposits ($421K) + Zelle + Square + deposits, this should drop significantly
5. If AR remains high, some invoices may have been paid before the data period (pre-Oct 2025) - check with Niccolo

### 7.3: Accounts Payable Verification

After matching vendor payments to bills:
1. Go to **Accounting > Reporting > Aged Payable**
2. Remaining should only be truly unpaid bills
3. Current: $22,418.09 across 4 vendors (Enohance, Joanne, GRW, Ventoux)

### 7.4: Suspense Account Check

- **Bank Suspense Account (101402)** currently shows -$12,062.01
- This balance should be $0.00 after full reconciliation
- If not zero, there are unmatched bank transactions

### 7.5: Run Reports

After completing all reconciliation:

1. **Balance Sheet** (Accounting > Reporting > Balance Sheet) - verify assets = liabilities + equity
2. **Profit & Loss** (Accounting > Reporting > Profit and Loss) - verify revenue and expenses make sense for the period
3. **Trial Balance** (Accounting > Reporting > Trial Balance) - total debits must equal total credits
4. **Bank Reconciliation Report** (Accounting > Reporting > Bank Reconciliation) - verify each bank account balances match statements

---

## PHASE 8: ONGOING RECONCILIATION SETUP

### 8.1: Reconnect Bank Sync

Go to **Accounting > Configuration > Online Synchronization**
- Current status: Disconnected
- Reconnect to auto-import new bank statements going forward

### 8.2: Set Up Monthly Close Process

At the end of each month:
1. Import/sync all bank statements
2. Run bank reconciliation for each journal
3. Create any missing vendor bills
4. Post all draft entries
5. Run Aged Receivable and follow up on overdue invoices
6. Run Aged Payable and schedule vendor payments
7. Generate P&L and Balance Sheet
8. Generate TABC compliance report

---

## TOP 66 CUSTOMERS BY OUTSTANDING BALANCE

| Customer | Invoices | Total Due | Oldest Invoice |
|----------|----------|-----------|---------------|
| HOUSTON OAKS COUNTRY CLUB BG | 16 | $104,749.00 | Jan 8, 2026 |
| TOCA MADERA | 8 | $17,447.00 | Jan 19, 2026 |
| MARCH | 2 | $15,802.00 | Jan 13, 2026 |
| LA GRIGLIA | 12 | $11,637.00 | Jan 15, 2026 |
| PAPPAS BRO STEAKHOUSE | 3 | $11,128.00 | Jan 17, 2026 |
| MARIGOLD CLUB | 4 | $10,905.00 | Jan 17, 2026 |
| HOTEL CHILDRESS | 3 | $10,830.00 | Feb 21, 2026 |
| NUMBER 13 | 7 | $9,919.00 | Jan 19, 2026 |
| INTERNATIONAL SPIRITS & WHOLESALE | 7 | $8,688.00 | Jan 12, 2026 |
| COWBOY PRIME | 3 | $8,482.00 | Feb 21, 2026 |
| Mr Sameer Sethna | 2 | $7,829.00 | Feb 9, 2026 |
| CREDENCE | 6 | $7,239.00 | Jan 8, 2026 |
| MILTON'S | 11 | $6,987.00 | Jan 8, 2026 |
| WINE CELLARS LTD | 2 | $6,122.00 | Jan 20, 2026 |
| Cole Mason | 1 | $5,872.00 | Jan 13, 2026 |
| SANTA FE STEAKHOUSE | 2 | $5,703.00 | Jan 19, 2026 |
| DALLAS FINE WINE | 2 | $5,395.00 | Feb 9, 2026 |
| LA ESTANCIA | 4 | $5,356.00 | Jan 23, 2026 |
| ZANTI RIVER OAKS | 5 | $5,079.00 | Jan 8, 2026 |
| PAPPAS BRO DOWNTOWN | 1 | $5,007.00 | Feb 9, 2026 |

**Note:** Many of these are likely already paid via Fintech/Zelle/Square deposits sitting unreconciled in the bank. After Phase 3, this list will shrink dramatically.

---

## KEY CONTACTS

| Role | Name | Email |
|------|------|-------|
| CEO / Wine Buyer | Niccolo Saltarelli | nick@winehuntersconsulting.com |
| COO (Italy) | Andrea Graneris | (contact via Niccolo) |
| Accounting Support | Muhammad Amir | contact@muhammadamir.com |

---

## ESTIMATED TIME

| Phase | Time |
|-------|------|
| Phase 1: Housekeeping | 15 min |
| Phase 2: Reconciliation Models | 30 min |
| Phase 3: Bank (BNK1) 378 lines | 3-4 hours |
| Phase 4: OENOTECA EXCELLENCE 33 lines | 30 min |
| Phase 5: Missing Vendor Bills | 1-2 hours |
| Phase 6: Chart of Accounts | 15 min |
| Phase 7: Verification | 30 min |
| **TOTAL** | **6-8 hours** |

**Fastest approach:** Do Phases 1, 2, and 6 first (setup), then work through Phase 3 by category (auto-match expenses first, then Fintech batches, then individual payments). Phase 5 (vendor bills) can be done in parallel if another person is available.
