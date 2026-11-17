# **Warehouse Inventory Test Plan**

## ZAMCELCO Information System (ZIS)

| Item | Value |
| :---- | :---- |
| Module | Warehouse Inventory (WH) |
| System / Repo | ZIS 2.0: zis-2.0 (frontend) \+ zis (backend) |
| Category | A (new) |
| Sprint / Branch | November 15, 2026 to November 20, 2026 / main |
| Version / Date | 1.0 / November 13, 2026 |
| Test Execution Document | Warehouse\_Test\_Cases |
| Prepared by | QA Analyst |

**TABLE OF CONTENTS**

[**1\. INTRODUCTION	3**](#1.-introduction)

[1.1. Objectives	3](#1.1.-objectives)

[1.2. Team Members	3](#1.2.-team-members)

[**2\. SCOPE	3**](#2.-scope)

[**3\. ASSUMPTIONS & RISKS	4**](#3.-assumptions-&-risks)

[3.1. Assumptions	4](#3.1.-assumptions)

[3.2. Risks	4](#3.2.-risks)

[**4\. TEST APPROACH	5**](#4.-test-approach)

[4.1. Manual Testing	5](#4.1.-manual-testing)

[4.2. Automation Testing	6](#4.2.-automation-testing)

[4.3. Entry & Exit Criteria	6](#4.3.-entry-&-exit-criteria)

[**5\. TEST ENVIRONMENT	6**](#5.-test-environment)

[**6\. MILESTONES & DELIVERABLES	7**](#6.-milestones-&-deliverables)

[6.1. Schedules	7](#6.1.-schedules)

[6.2. Deliverables	7](#6.2.-deliverables)

# **1\. INTRODUCTION** {#1.-introduction}

## **1.1. Objectives** {#1.1.-objectives}

* The Warehouse Inventory module follows every item from the purchase request to its issue, return or salvage. Each step is a numbered document that leads to the next one.  
* Stock goes up only when a Receiving Report is approved or a returned item is verified as usable, and goes down only when a Memorandum Receipt (MR) is issued. Each posting uses the moving-average cost per location and bin, never makes a balance negative, and happens only once per document.  
* Each document allows only its listed status moves and refuses the rest, for example editing a submitted PR or an issued PO, or closing a partly issued RIV.  
* Each step needs its permission, and no user can approve their own work: an award, a return inspection, or audit papers.  
* The Process Tracker shows the right stage and who each process is waiting for, and labels and the scan check work on deliveries and MRs.

## **1.2. Team Members** {#1.2.-team-members}

Aliases are used instead of names (see 3.1).

| Name | Role | Responsibility |
| :---- | :---- | :---- |
| QA | IT QA Analyst | Test plan, test cases, execution, bug reports |
| DEV-WH | Developer | Fixes, confirms user stories, test accounts and data |
| BO-WH | Business Owner (Warehouse) | UAT sign-off |

# **2\. SCOPE** {#2.-scope}

**In scope (Warehouse Inventory menu):**

* Process Tracker: Purchases, Issuances and Material returns, with stages, Waiting for, and Timeline.  
* Catalogue \> Items (stock status cards, CSV export, Print labels) and Catalogue \> Categories.  
* Purchasing \> Purchase Requests (create, update and delete drafts), Canvassing & Bidding (canvass, quotations, abstract, public bidding, award recommendation and approval), Purchase Orders, Suppliers, Budget Codes.  
* Receiving \> Deliveries (with inspection and Scan item), Receiving Reports (prepare, check, approve and post), Supplier Returns (acknowledge, redelivery, close), Withdrawals.  
* Issuance \> RIVs (submit, stock check, issue MR, refer to purchasing, close) and Issuance \> Memorandum Receipts (read-only register).  
* Returns \> Material Returns (ticket, receive, inspect, verify), Salvage Records, Audit Papers (prepare, review, approve).  
* Stock \> Stock on Hand and Stock \> Serial Numbers (read-only).  
* Lot and serial entry on Receiving Reports, Memorandum Receipts and Material Returns: lot number, expiry date and one serial number per unit, as the item's tracking flags require.  
* Setup \> Departments, Locations, Bins (with bin labels), Number Sequences, Procurement Types.  
* Mobile & Tablet: Look up, Receive and Issue with a scanner or camera.  
* Stock posting and valuation behind every RR, MR and verified return, and role-based access with segregation of duties.

**Out of scope:**

* Submitting, budget-checking, approving and closing a PR on screen: the buttons are not in the portal yet (User Guide 5, step 2). Covered through the API only if the developer agrees (3.1).  
* Automatic budget check (BR-001), attachments (NFR-005) and the Reports Dashboard (FR-039): not built yet (SRS v1.2, 14.1).  
* Stock adjustments: planned for later in the backend docs.  
* Accounting, payables and payment (SRS 1.3); performance, backup and recovery; legacy data migration: no target or build this sprint.  
* zamcelco\_information\_system (Yii2): the module is built only in ZIS 2.0.

# **3\. ASSUMPTIONS & RISKS** {#3.-assumptions-&-risks}

## **3.1. Assumptions** {#3.1.-assumptions}

* The build under test consists of zis (commit 332289d) and zis-2.0 (commit c222bbc) as of November 13, 2026\. A result counts only on a noted commit.  
* The developer will give one test account per role alias with the matching warehouse permissions; today only SA can open every page.  
* PR submit, budget check, approve and reject exist only as API endpoints. Whether QA may call them directly to move a PR to For PO is not confirmed.  
* Stock on Hand, Serial Numbers, and lot and serial tracking are in the code and the backend docs but not in SRS v1.2 or User Guide 1.0. The stock pages are tested read-only. Lot and serial entry on Receiving Reports, Memorandum Receipts and Material Returns is tested against the checks in the backend code until the developer confirms the requirement.  
* No bug tracker is chosen yet; bugs are logged in Warehouse\_Test\_Cases until one is.

## **3.2. Risks** {#3.2.-risks}

| Risk | Level | Mitigation |
| :---- | :---- | :---- |
| Stock quantity or value is wrong after an RR, MR or verified return (moving-average cost, salvage kept out of stock) | High | E2E flows per posting type; read-only DB checks of balances and transactions in SQLyog after each posting; review of the existing JUnit posting tests |
| A document posts twice, or a quantity above what is due, available, accepted or issued is accepted | High | Negative tests with boundary values and double clicks on the Local copy; DB check of one transaction per document |
| A user does a step without permission, or approves their own award, inspection or audit papers | High | Role and permission matrix with one account per alias; segregation-of-duty cases automated in Playwright |
| A refused status move is allowed (edit a submitted PR or issued PO, close a partly issued RIV) | High | State transition testing on PR, PO, RR, RIV, ticket and audit paper statuses |
| Lot or serial rules fail: a missing lot or expiry date is accepted, a serial number is duplicated, or stock is issued from an expired or on-hold lot | High | Negative cases on Receiving Report, MR and Material Return lines; DB check of lots and serials after posting |
| One QA for a large module in a 6-day sprint | High | Run High-priority cases first; automate smoke and the three E2E flows early; defer Low cases |
| Wrong procurement type from the amount ranges, or a gap in the ranges | Medium | Boundary values on Setup \> Procurement Types; PR route checked via API if allowed |
| Environment not ready: no per-role accounts, module not on the QA Server, no backend in CI | Medium | Raise on day 1; run on Local until ready (3.1) |
| Code ahead of the SRS (stock pages, lots and serials) | Medium | Confirm with the developer; mark unconfirmed cases "Unconfirmed: ask developer" |
| Faults in labels, scan check or the Mobile & Tablet app | Low | Manual checks with an A4 label sheet and a scanner |

# **4\. TEST APPROACH** {#4.-test-approach}

## **4.1. Manual Testing** {#4.1.-manual-testing}

* Exploratory sessions on each new build, one per process (Purchases, Issuances, Material returns), following User Guide 1.0 and using only QA- records.  
* Role and segregation-of-duty checks with each alias account: award approval, RR check and approval, return verification, audit paper review and approval.  
* Read-only DB checks in SQLyog after each posting type: inventory balances, transactions and average cost must match the screen.  
* Label printing on an A4 label sheet (21 labels, 1 to 50 copies) and the scan check on Deliveries, MRs and the Mobile & Tablet app: these need a printer, a scanner or a phone.  
* Usability of list pages, greyed-out buttons with their reasons, and validation messages, with warehouse staff.  
* UAT with warehouse and purchasing users on the QA Server.

**Test design techniques:**

| Technique | Used on |
| :---- | :---- |
| Boundary values and equivalence partitions | Quantities, costs, label copies (1 to 50), and the procurement type amount ranges, plus lot numbers, expiry dates and serial numbers |
| State transitions | PR, PO, RR, RIV, ticket and audit paper statuses |
| Decision tables | RIV stock check (available, partial, not available) and delivery inspection results |
| Role-permission matrix | Every step that needs a permission, including segregation of duties |
| Calculation checks | Moving-average cost after each posting |

## 

## **4.2. Automation Testing** {#4.2.-automation-testing}

| What | Tool | Runs Where / When |
| :---- | :---- | :---- |
| Smoke: every Warehouse Inventory page opens for SA | Playwright (zis-2.0 e2e/) | Local first; CI once a backend runs in CI |
| E2E: PR to RR posting, RIV to MR, material return to verification; segregation-of-duty checks | Playwright (zis-2.0 e2e/) | Local on every new build |
| Posting, cost and status rules (existing developer tests) | JUnit (zis) | Local; CI only builds the jar today |
| Components and API routes (existing developer tests) | Vitest (zis-2.0) | CI on every pull request |

The zis-2.0 CI already runs Playwright on every pull request, but with no backend or database, so warehouse specs run only on Local until a test backend is added to CI (step A.14).

## **4.3. Entry & Exit Criteria** {#4.3.-entry-&-exit-criteria}

**Entry:**

* zis and zis-2.0 main at a noted commit run on Local and on the QA Server.  
* User stories confirmed by the developer.  
* One account per alias with its warehouse permissions; base data ready: QA- items, three active suppliers, an active budget code, a location with two bins, a holding location, and procurement type ranges with no gaps.

**Exit:**

* All High-priority WH cases run and passed.  
* No open Critical or High WH-DEF bugs.  
* DB checks match the screen for every posting type (RR, MR, usable return, salvage).  
* Sanity and selective regression passed after fixes.  
* Playwright smoke and E2E specs pushed to zis-2.0 e2e/, and UAT signed off by BO-WH.

# **5\. TEST ENVIRONMENT** {#5.-test-environment}

| Environment | URL | Database | Used For |
| :---- | :---- | :---- | :---- |
| Local | http\://localhost:3000 (frontend), http\://localhost:8081 (backend) | Local MySQL copy | Automation runs, early checks, destructive tests |
| QA Server | http\://192.168.200.210/ (LAN only) | QA DB (read-only in SQLyog) | Manual testing, UAT |

Browsers: Chrome only. 

Devices: one phone or tablet with a barcode scanner or camera, and A4 label sheets.

# **6\. MILESTONES & DELIVERABLES** {#6.-milestones-&-deliverables}

## **6.1. Schedules** {#6.1.-schedules}

| Activity | Start | End |
| :---- | :---- | :---- |
| Test plan and test cases | November 15, 2026 | November 17, 2026 |
| Automation scripts | November 16, 2026 | November 18, 2026 |
| Manual and automated execution | November 17, 2026 | November 19, 2026 |
| Bug fixing, sanity, selective regression | November 18, 2026 | November 20, 2026 |
| UAT and sign-off | November 19, 2026 | November 20, 2026 |

## **6.2. Deliverables** {#6.2.-deliverables}

| Deliverable | Workflow Step | Location |
| :---- | :---- | :---- |
| Test Plan | A.5 | Google Docs |
| Test Execution Document (suites and cases) | A.6 | Warehouse\_Test\_Cases |
| Automation scripts | A.7 | zis-2.0 e2e/ |
| Test Result | A.10 | Warehouse\_Test\_Cases, Summary sheet |
| Bug Reports | A.11 | Warehouse\_Test\_Cases (see 3.1) |

