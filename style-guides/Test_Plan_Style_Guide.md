# ZIS Test Plan: Style Guide

This guide defines the one format every ZIS test plan uses (workflow step A.5 / B.6,
Special Category A). The plan is lean and straight to the point: it says what will
be tested, how, where, and when. It does **not** hold test cases, steps, SQL, or
results. Those live in the Test Execution Document (step A.6 / B.7) and the Test
Result (step A.10 / B.11).

**Where the plan is written:** in Google Docs. For each module, I will attach
the link to the Google Doc where the test plan will be made. Write the plan
directly in that document, following this guide. Do not create a new document
or a separate file unless I ask for one.

**What happens next:** once the plan is done, I will upload it to Claude Code so
Claude Code can help generate the test cases for the Test Execution Document
(step A.6 / B.7). Write the plan so Claude Code can use it as its source: name
every in-scope feature and screen in 2. SCOPE, list the risks in 3.2, and say
what is manual and what is automated in 4.1 and 4.2. Use the module and test
case ID codes in Section 5 of this guide (Shared IDs) so the generated cases
get the right IDs. Use the prompt in Section 8.

**Target length:** 3 to 5 pages in Google Docs. If a plan grows past that, cut
text or move detail to the Test Execution Document.

---

## 1. Required Structure

Every plan has these sections, in this order, with these exact headings. If a
sub-section does not apply, write one line saying why (for example, "No
automation this sprint: the module has no stable UI yet.").

```text
TITLE PAGE
TABLE OF CONTENTS
1. INTRODUCTION
   1.1. Objectives
   1.2. Team Members
2. SCOPE
3. ASSUMPTIONS & RISKS
   3.1. Assumptions
   3.2. Risks
4. TEST APPROACH
   4.1. Manual Testing
   4.2. Automation Testing
   4.3. Entry & Exit Criteria
5. TEST ENVIRONMENT
6. MILESTONES & DELIVERABLES
   6.1. Schedules
   6.2. Deliverables
```

### Title Page
Page 1 holds only the title, the subtitle, and the info table. Then a page break.

- **Title** (Title style): `<Module Name> Test Plan`
- **Subtitle** (Subtitle style): `ZAMCELCO Information System (ZIS)`
- **Info table:**

| Item | Value |
| --- | --- |
| Module | `<Module name>` (`<MODULE CODE>`) |
| System / Repo | `<zamcelco_information_system / zis-2.0 + zis>` |
| Category | A (new) or B (existing, in production) |
| Sprint / Branch | `<sprint>` / `<branch>` |
| Version / Date | `<x.y>` / `YYYY-MM-DD` |
| Test Execution Document | `<Module>_Test_Cases` |
| Prepared by | QA Analyst |

### Table of Contents
Page 2, then a page break before Section 1.

- Insert it with Insert > Table of contents > **with page numbers**. Do not type it by hand.
- Show Heading 1 and Heading 2 only (sections 1 to 6 and their sub-sections).
- Refresh it before every share or PDF export (click inside it, then the refresh icon).

---

## 2. What Goes in Each Section

Each section is a few bullets or one small table. Never copy test cases into the plan.

### 1. INTRODUCTION
- **1.1. Objectives:** 3 to 5 bullets. What the module does in one line, then
  what this testing must prove (for example, "Stock issued through an RIV is
  deducted correctly from inventory").
- **1.2. Team Members:** one table.

  | Name | Role | Responsibility |
  | --- | --- | --- |
  | `<name>` | QA Analyst | Test plan, test cases, execution, bug reports |
  | `<name>` | Developer | Fixes, confirms user stories |
  | `<name>` | Business Owner | UAT sign-off |

### 2. SCOPE
Two short bullet lists: **In scope** (features or screens, by ZIS menu label) and
**Out of scope** (with a few words on why). For Category B, also name the
existing modules that could be affected by the change.

### 3. ASSUMPTIONS & RISKS
- **3.1. Assumptions:** bullets. Things you are taking as true (for example, "The
  QA server has the latest `staging` build", "The developer confirmed the user
  stories on `<date>`").
- **3.2. Risks:** one table, highest risk first. Risks that drive more testing go
  here (wrong amounts, lost data, wrong access, deadline pressure).

  | Risk | Level (High / Medium / Low) | Mitigation |
  | --- | --- | --- |

### 4. TEST APPROACH
- **4.1. Manual Testing:** bullets. What is tested by hand and why: exploratory
  sessions, UAT with business users, usability, one-off DB checks in SQLyog
  (read-only). Name the test design techniques used, in one line (for example,
  "Boundary values and equivalence partitions on the RIV form; state
  transitions on RIV statuses").
- **4.2. Automation Testing:** one table. What is automated, with which tool, and
  where it runs.

  | What | Tool | Runs Where / When |
  | --- | --- | --- |
  | Smoke and E2E flows | Playwright | Local first; then CI on every push to `staging` |
  | Unit / functional (Yii2) | Codeception | CI |
  | Unit / API (Spring Boot) | JUnit | CI |

  For Category B, add one line on the existing developer-written tests reviewed
  in step B.5 and whether they are kept, fixed, or extended.
- **4.3. Entry & Exit Criteria:** two short bullet lists.
  - **Entry:** build deployed to the QA server, user stories confirmed, test
    data and accounts ready.
  - **Exit:** all High-priority cases run and passed, no open Critical or High
    bugs, sanity and selective regression passed, automation added to CI.

### 5. TEST ENVIRONMENT
One table.

| Environment | URL | Database | Used For |
| --- | --- | --- | --- |
| Local | `http://localhost:<port>` | Local Docker DB | Automation runs, early checks |
| QA Server | `http://192.168.200.210/` (LAN only) | QA DB (read-only in SQLyog) | Manual testing, UAT |

Add one line for browsers or devices only if the module needs them.

### 6. MILESTONES & DELIVERABLES
- **6.1. Schedules:** one table that follows the sprint.

  | Activity | Start | End |
  | --- | --- | --- |
  | Test plan and test cases | | |
  | Automation scripts | | |
  | Manual and automated execution | | |
  | Bug fixing, sanity, selective regression | | |
  | UAT and sign-off | | |

- **6.2. Deliverables:** one table, mapped to the workflow steps.

  | Deliverable | Workflow Step | Location |
  | --- | --- | --- |
  | Test Plan | A.5 / B.6 | Google Docs |
  | Test Execution Document (suites and cases) | A.6 / B.7 | `<Module>_Test_Cases` |
  | Automation scripts | A.7 / B.8 | Repo, `e2e/` or `tests/` |
  | Test Result | A.10 / B.11 | `<Module>_Test_Cases`, Summary sheet |
  | Bug Reports | A.11 / B.12 | `<bug tracker>` |

---

## 3. Formatting (Google Docs)

Keep it simple. Default page setup (Letter, portrait, 1 in margins), plus:

| Element | Style |
| --- | --- |
| Title | Title style, `<Module> Test Plan` |
| Main sections (1. to 6.) | Heading 1, Arial 14, bold |
| Sub-sections (1.1. etc.) | Heading 2, Arial 12, bold |
| Body and tables | Arial 11 (tables 10), header row bold |
| Footer | `Page <n> of <total>` |

Rules:
- Use real Heading styles so the outline works. Type the numbers yourself.
- No page break before each section; sections flow one after another.
- Tables: at most 4 columns, no merged cells, `-` for an empty cell.

---

## 4. Writing Rules

- Plain words, short sentences, active voice. Write for a new QA who has never
  seen the module.
- Use the ZIS side-menu label for screens: `Warehouse > RIV`.
- Dates as `YYYY-MM-DD` in tables. Amounts as "PHP 240.00".
- No real consumer or employee names; use account aliases (`SA`, `MGR`, `SUP`,
  `EMP-IN`, `EMP-OUT`).
- No credentials, passwords, or `.env` values.
- Do not state behavior as fact unless it was confirmed from the code or by a
  developer. List unconfirmed items under 3.1 Assumptions.

---

## 5. Shared IDs (Plan and Test Execution Document)

The plan refers to cases by ID; the Test Execution Document holds them. IDs stay
stable so they can move into a future Test Case Management module.

- **Test case ID:** `<MODULE>-<TYPE>-<###>`, e.g. `WH-FN-012`. Numbers never change;
  a removed case is marked "Retired".
- **Module codes:** CON Consumer, BIL Billing, COL Collection, HR HRIS, WF Workflow,
  MR Meter Reading, WH Warehouse Inventory, RPT Reportorial. A new module gets a
  2 to 4 letter code; add it here first.
- **Type codes:** SM Smoke, FN Functional, NG Negative, DB Database, SC Security,
  CJ Console Job, E2E End-to-End, UAT User Acceptance, RG Regression.
- **Bug ID:** `<MODULE>-DEF-<###>`.

---

## 6. Pre-Share Checklist

- [ ] Title page and table of contents are present; the TOC is refreshed.
- [ ] All 6 sections and their sub-sections are present, with the exact headings.
- [ ] 3 to 5 pages; no test cases, steps, or SQL copied into the plan.
- [ ] Every High risk in 3.2 is covered by the approach in 4.1 or 4.2.
- [ ] Entry and exit criteria are specific to this module.
- [ ] No real names, credentials, or leftover `< >` placeholders.

---

## 7. Prompt for Claude (Writing the Test Plan)

```text
Write a ZIS test plan for <module> that follows the attached
"ZIS Test Plan: Style Guide" exactly, directly in the Google Doc linked below.

Inputs:
- Google Doc link: [link to the Google Doc where the test plan will be made]
- Category: [A (new) / B (existing)]
- Module and code: [e.g. Warehouse Inventory, WH]
- Sources: [SRS, user manual, repo/branch, confirmed user stories]
- Sprint dates: [start and end]

Requirements:
1. Start with the title page and table of contents, then use only the 6 sections and sub-sections in Section 1.
2. Fill each section as described in Section 2: bullets or one small table.
3. Keep it to 3 to 5 pages. No test cases, steps, or SQL.
4. Do not invent behavior; put anything unconfirmed under 3.1 Assumptions.
5. Finish with the Pre-Share Checklist and list any item not met.
```

---

## 8. Prompt for Claude Code (Generating Test Cases)

Use this after the plan is done (step A.6 / B.7). Upload or attach the finished
test plan (exported from the Google Doc as PDF or Markdown) to Claude Code, open
it in the module's repository, and paste the prompt below.

- **Start in plan mode** (type `/plan`, or press Shift+Tab until it shows plan
  mode). Claude Code reads the plan and the code and shows you the suites,
  case count, and gaps first, without writing anything. Review it, then approve
  so it writes the file.
- **Output:** `qa-output/<Module>_Test_Cases.xlsx` in the repo folder on your
  computer. Import it into Google Sheets to make it the Test Execution
  Document. It is not committed to the repo.

```text
Generate the test cases for the <module> Test Execution Document from the
attached test plan. The plan follows the "ZIS Test Plan: Style Guide".

Inputs:
- Test plan: [attach the exported test plan]
- Category: [A (new) / B (existing)]
- Module and code: [e.g. Warehouse Inventory, WH]
- Repository and branch: [e.g. zamcelco_information_system, feature/warehouse]
- Other sources: [SRS, user manual, confirmed user stories]
- For Category B: [existing CI tests reviewed in step B.5]

Requirements:
1. Cover every in-scope feature in 2. SCOPE. Use nothing listed as out of scope.
2. Group the cases into test suites, one suite per feature or screen.
3. Give every case an ID in the format <MODULE>-<TYPE>-<###> from Section 5 of
   the style guide (for example WH-FN-001). Never reuse or renumber an ID.
4. Set each case's priority from the risk it covers in 3.2. Every High risk
   must have at least one case.
5. Mark each case Manual or Automated, following 4.1 and 4.2 of the plan. For
   an Automated case, name the tool (Playwright, Codeception, or JUnit).
6. Keep it lean: one case per objective, about 2 to 4 cases per feature. Put
   boundary and invalid values inside the case for that form, not as separate
   cases.
7. Read the code on the branch to confirm fields, rules, messages, and roles.
   Do not invent behavior. Mark anything you could not confirm "Unconfirmed:
   ask developer".
8. Use account aliases (SA, MGR, SUP, EMP-IN, EMP-OUT), never real names or
   credentials. DB checks are SELECT-only.
9. Save the test cases as <Module>_Test_Cases.xlsx in a qa-output/ folder at
   the repo root, one row per case, with these columns: Test Case ID, Test
   Suite, Type, Priority, Role / Account, Preconditions, Test Data, Steps,
   Expected Result, Manual / Automated, Tool, Status (Not Run). Do not commit
   this file or the qa-output/ folder.
10. End with a short list of gaps: risks or features with no case, and open
    questions for the developer.
```
