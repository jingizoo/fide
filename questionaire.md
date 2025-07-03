Below are revised tables for each configuration area:

* **Answer —** only facts explicitly provided in your screenshots/conversation.
* **Suggestion —** where something is still blank, I’ve added a concise prompt or possible approach you can confirm (or delete).

---

### 1 Core Foundation – ChartFields, SetIDs, Business Units

| # | Question                                   | **Answer (confirmed)**                                                                                                                                                     | **Suggestion (if still blank)**                                                                          |
| - | ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| 1 | Final BU codes & base currencies           | RealNetworks Ltd (GBR01) – GBP <br>Muzicall Ltd (MUS01) – GBP <br>RealNetworks GmbH (AUS01) – EUR <br>RN Portugal branch (PRT01) – EUR <br>RN Croatia d.o.o. (HRV01) – EUR | Add any US/Brazil or other entities now, to avoid re-opening BU tables later.                            |
|   | Does any BU use a non-Jan–Dec fiscal year? |                                                                                                                                                                            | Default to Jan–Dec unless local law (e.g., UK Apr–Mar) demands change.                                   |
| 2 | ChartFields to enable                      | Company, Division, Department, Account, Product Line, Project, Territory Seg 1-3, Order Channel, Revenue Type                                                              | Confirm if **Affiliate** or **Market** fields are needed for inter-company & reporting.                  |
|   | Owner for each CF list                     |                                                                                                                                                                            | Finance Ops typically owns Dept & Account; Sales Ops owns Product.                                       |
| 3 | Trees/Roll-ups required                    |                                                                                                                                                                            | Minimum: NAT\_ACCT, Dept, Division, Product for nVision.                                                 |
|   | Security trees?                            |                                                                                                                                                                            | If AP users must not see HRV01 data, add BU security tree.                                               |
| 4 | SetID strategy                             |                                                                                                                                                                            | Single global **SHARE** is simplest; add BU-specific SetIDs only if local vendor/customer tables differ. |

---

### 2 General Ledger

| # | Question                            | **Answer** | **Suggestion**                                                                   |
| - | ----------------------------------- | ---------- | -------------------------------------------------------------------------------- |
| 1 | Need MultiBook (Local GAAP + IFRS)? |            | Enable now if any entity reports IFRS + local GAAP; turning on later is painful. |
|   | Statistical ledger?                 |            | Use STAT if subscriber counts must post into GL.                                 |
| 2 | Rate-type publisher & frequency     |            | Kyriba daily upload (AVG, CRRNT, HIST) is common.                                |
|   | Revaluation schedule                |            | Monthly Day +2 aligns with 5-day close goal.                                     |
| 3 | Journal approval threshold          |            | Typical: > £/\$ 250 k routes to Controller via AWE.                              |
|   | Journal ID prefix for uploads       |            | “NSJ-{YYMMDD}-{SEQ}” keeps NetSuite history clear.                               |

---

### 3 Accounts Payable & Purchasing

| # | Question                     | **Answer** | **Suggestion**                                                        |
| - | ---------------------------- | ---------- | --------------------------------------------------------------------- |
| 1 | Supplier master scope        |            | Single global list reduces duplicates; mark inactive suppliers.       |
|   | Bank-account masking rule    |            | Store IBAN but mask all but last 4 when displayed.                    |
| 2 | Match rules & tolerances     |            | Goods = 3-Way, Services = 2-Way with ±2 % price tolerance is typical. |
| 3 | VAT/GST codes & rates        |            | UK 20 %, AT 20 %, PT 23 % (confirm).                                  |
|   | Reverse-charge handling      |            | Needed for intra-EU services.                                         |
| 4 | Payment methods per currency |            | GBP → BACS; EUR → SEPA XML; USD → ACH.                                |
|   | Batch approval limits        |            | AP Mgr ≤ 25 k; CFO above.                                             |

---

### 4 Accounts Receivable & Billing

| # | Question                        | **Answer**                                                                                | **Suggestion**                                                      |
| - | ------------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| 1 | Invoice upload template columns | BU, Invoice ID/Date, Customer ID, Line Amt, Currency, VAT, Account, Product, Dept, Region | Add **Order Channel** & **Territory Segs** if needed for reporting. |
|   | File naming / max rows          |                                                                                           | `INV_{BU}_{YYYYMMDD}_{SEQ}.csv`, ≤ 10 000 rows.                     |
| 2 | Invoice numbering scheme        |                                                                                           | `{BU}-{FY}-{#####}` keeps per-entity sequence.                      |
|   | Delivery channels mix           | 5 invoices per month via SFTP upload; 12 via e-mail PDF                                   | Verify counts; link e-mail addresses & SFTP paths.                  |
| 3 | Revenue-recognition types       | Milestone, Usage, Fixed (all flagged “applicable”)                                        | Map to **CLASS\_FLD** for downstream reporting.                     |
| 4 | Dunning approach                |                                                                                           | Manual wave in Phase 1; HighRadius Phase 2.                         |

---

### 5 Asset Management

| # | Question                  | **Answer**                                  | **Suggestion**                                 |
| - | ------------------------- | ------------------------------------------- | ---------------------------------------------- |
| 1 | Books per BU              |                                             | Typical: CORP (GAAP) & STAT (IFRS).            |
| 2 | Capitalisation thresholds |                                             | Hardware £/\$ 5 k; Software £/\$ 20 k—confirm. |
|   | CIP linkage               | Project Costing will feed CIP if activated. |                                                |
| 3 | ARO / impairment tracking |                                             | Only if leased data-centre liabilities exist.  |

---

### 6 Project Costing

| # | Question                  | **Answer**         | **Suggestion**                                                        |
| - | ------------------------- | ------------------ | --------------------------------------------------------------------- |
| 1 | Usage (rate-based, capex) |                    | Rate-based billing + costs to CIP common for prof-serv organisations. |
| 2 | Project statuses          | PLN, ACT, CLS, CAN | Confirm if “HLD” (Hold) needed.                                       |
| 3 | Rate sets & currencies    |                    | Define COST→BILL per labour grade.                                    |

---

### 7 Banking & Treasury

| # | Question                   | **Answer**                                                                                                                                               | **Suggestion**                                                             |
| - | -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| 1 | External bank list         | BofA x9025 (EUR) & x9017 (GBP) – RN Ltd <br>BofA x7018 (GBP) – Muzicall <br>BofA x6018 (EUR) – RN AT <br>Bankinter x7776 (EUR) – PT <br>mojaRBA (€) – HR |                                                                            |
|   | Statement formats per bank |                                                                                                                                                          | MT940 for BofA/Bankinter; CAMT.053 for mojaRBA, unless banks supply MT940. |
| 2 | Payment file types         |                                                                                                                                                          | BACS, SEPA, ACH; verify bank acceptance specs.                             |
| 3 | Auto-rec match tolerances  |                                                                                                                                                          | ± €1 or ± GBP 1; reference vs amount.                                      |
|   | Unmatched line owner       |                                                                                                                                                          | Treasury Analyst to clear daily.                                           |

---

### 8 Integrations

| Interface                  | **Answer (known)**                     | **Suggestion**                        |
| -------------------------- | -------------------------------------- | ------------------------------------- |
| NetSuite → PS Journals     | Daily SFTP CSV to JRNL\_LOAD           | Confirm column map & archive path.    |
| Coupa → Vouchers           | 4-hourly CSV to Voucher Build          | Specify error-handling file share.    |
| Invoice Spreadsheet Upload | ExcelToCI → BI\_INTFC\_STG → BI Loader | Lock template owner & change-control. |
| Bank Statements Loader     | EBS Loader 05:00 daily                 | Collect sample MT940/CAMT files now.  |
| PS → Bank Payments         | BACS XML, SEPA XML, ACH files via SFTP | PGP encrypt Y/N; bank ack files?      |
| PS → Tableau               | Nightly ODBC extract                   | Confirm dashboard refresh time.       |

---

### 9 Data Conversion

| Question                 | **Answer**      | **Suggestion**                                            |
| ------------------------ | --------------- | --------------------------------------------------------- |
| Historical GL start date |                 | Earliest audited year (e.g., 2022) keeps load reasonable. |
| AP/AR scope              | Open items only | Include *Open PO Receipts* if 3-Way match on go-live.     |
| Extract owners           |                 | Finance Ops for GL; AP/AR Leads; FA Lead.                 |
| Reconciliation rule      |                 | Target TB variance < 0.5 %.                               |

---

### 10 Security & Workflow

| Question              | **Answer** | **Suggestion**                |
| --------------------- | ---------- | ----------------------------- |
| Roles / SoD matrix    |            | Draft early to avoid re-work. |
| Journal AWE threshold |            | Typical > \$250 k.            |
| Voucher AWE threshold |            | AP Mgr ≤ 25 k; CFO above.     |
| Delegation rules      |            | 30-day expiry recommended.    |

---

### 11 Reporting & Analytics

| Question           | **Answer**                                  | **Suggestion**                                     |
| ------------------ | ------------------------------------------- | -------------------------------------------------- |
| Close-pack reports |                                             | nVision TB / P\&L / BS + BI Publisher VAT returns. |
| KPI dashboard      | Days-to-Close, Auto-rec %, Invoice accuracy | Load via Tableau nightly extract.                  |

---

### 12 Testing, Cut-over & Support

| Question               | **Answer** | **Suggestion**                                                    |
| ---------------------- | ---------- | ----------------------------------------------------------------- |
| Test cycles planned    |            | Two SIT, one UAT, Parallel Close – confirm business availability. |
| Cut-over freeze window |            | 72 h is standard.                                                 |
| Hyper-care duration    |            | 30 days with daily triage call.                                   |

---

### 13 Change Management & Training

| Question        | **Answer** | **Suggestion**                             |
| --------------- | ---------- | ------------------------------------------ |
| Training format |            | Role-based 2-hr live + Loom videos.        |
| Comms cadence   |            | Weekly “Bonafide Buzz” e-mail & Slack FAQ. |

---

**Action:** fill blanks with your SMEs; review suggestions; strike any you don’t need. Once every row is complete, you’ll have the definitive input set to drive the playbook’s configuration tasks.
