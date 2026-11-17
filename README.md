# ZIS QA Docs

QA documents for the ZAMCELCO Information System (ZIS): test plans, the files they are based on, and the test cases, results and bug reports made from them. This repo is the backup and the shared copy, so anyone on the team can clone it and see what was tested and how.

Code lives in its own repos. Nothing in this repo is deployed.

| System | Repos | Status |
| --- | --- | --- |
| ZIS (legacy, Yii2) | `zamcelco_information_system` | In production |
| ZIS 2.0 | `zis-2.0` (React frontend), `zis` (Spring Boot backend) | In development, planned for 2027 |

## Folder layout

One folder per system, then one folder per module inside it. Each module keeps its inputs and outputs side by side. Folder names use no spaces, so scripts and tools on Windows work with them.

```text
docs/
├── README.md
├── style-guides/                   How QA documents are written
│   ├── QA_Workflow_Instructions.md
│   └── Test_Plan_Style_Guide.md
├── zis-yii2/                       ZIS (legacy, Yii2) modules
│   └── <module>/                   For example billing/
│       ├── qa-input/
│       └── qa-output/
└── zis-2.0/                        ZIS 2.0 modules
    └── <module>/                   For example warehouse-inventory/
        ├── qa-input/               What the test cases are made from
        │   ├── <Module>_Test_Plan.md
        │   └── SRS and user guide
        └── qa-output/              What QA produces
            ├── <Module>_Test_Cases.xlsx
            └── Test results and bug reports
```

## Modules

| Module | Code | System | Category | Folder | Status |
| --- | --- | --- | --- | --- | --- |
| Warehouse Inventory | WH | ZIS 2.0 | A (new) | `zis-2.0/warehouse-inventory/` | Test plan done; test cases next |

Category A is a new module or feature. Category B is one that is already in production. See `style-guides/QA_Workflow_Instructions.md` for the steps of each.

## IDs

IDs never change once used. A removed case is marked "Retired", not deleted or renumbered.

- **Test case:** `<MODULE>-<TYPE>-<###>`, for example `WH-FN-012`
- **Bug:** `<MODULE>-DEF-<###>`, for example `WH-DEF-003`
- **Module codes:** CON Consumer, BIL Billing, COL Collection, HR HRIS, WF Workflow, MR Meter Reading, WH Warehouse Inventory, RPT Reportorial. Add a new module's code here and in the style guide first.
- **Type codes:** SM Smoke, FN Functional, NG Negative, DB Database, SC Security, CJ Console Job, E2E End-to-End, UAT User Acceptance, RG Regression.

## Working with this repo

Clone it next to the code repos, so Claude Code can read all of them at once:

```text
C:\Dev\ZAMCELCO\NEW_ZIS\
├── zis\
├── zis-2.0\
└── docs\
```

For a ZIS Yii2 module, open Claude Code in a folder that holds `docs` and `zamcelco_information_system` side by side.

### Adding a module

1. Create `<module>/qa-input/` and `<module>/qa-output/` under `zis-yii2/` or `zis-2.0/`, whichever system the module belongs to.
2. Add the module to the Modules table above.
3. Write the test plan in Google Docs, following `style-guides/Test_Plan_Style_Guide.md`.
4. Export the plan as Markdown (File > Download > Markdown) into `qa-input/`, together with the SRS and the user guide.

### Generating test cases

1. Open Claude Code in `NEW_ZIS`, in plan mode.
2. Paste the prompt in Section 8 of the style guide, pointing it at `docs/<system>/<module>/qa-input/`, for example `docs/zis-2.0/warehouse-inventory/qa-input/`.
3. Review the suites and gaps it proposes, then approve.
4. Claude Code saves `<Module>_Test_Cases.xlsx` in `docs/<system>/<module>/qa-output/`. Import it into Google Sheets to run it.

### Google files and the repo

Google Docs and Google Sheets are where documents are edited. This repo holds snapshots of them.

| What | Export as | How |
| --- | --- | --- |
| Google Doc (test plan, test result write-ups) | `.md` | File > Download > Markdown |
| Google Sheet (test cases, results, bugs) | `.xlsx` | File > Download > Microsoft Excel. It keeps every tab; CSV saves only one. |
| Signed-off version (optional) | `.pdf` | File > Download > PDF. Add it next to the `.md`, not instead of it. |

Don't use `.docx`: GitHub can't show it, and a PDF covers the same need.

Going the other way:

- **Test cases from Claude Code (`.xlsx`):** upload to Google Drive and open with Google Sheets. From then on, the Google Sheet is the master copy.
- **A `.md` file back into Google Docs:** turn on Tools > Preferences > Enable Markdown, then use Edit > Paste from Markdown. This is rarely needed.

Three rules keep the copies in step:

1. **Never edit the repo copy.** Change the Google file, then export it again.
2. **Export at milestones:** plan approved, test cases generated, test run finished, bugs closed.
3. **Keep the same file name every time**, such as `Warehouse_Test_Plan.md`, not `_v2`. Git keeps the old versions, and the commit message says what changed.

## Rules

- **Keep the repo private.** SRS documents and user guides are internal.
- **No secrets.** Never commit `.env` files, passwords, tokens or test account logins. Use account aliases (SA, MGR, SUP, EMP-IN, EMP-OUT), never real names.
- **Write clear commit messages**, since spreadsheets don't show changes in GitHub. Start with the module code, for example `WH: add 12 cases for lot and serial entry`.
- **Test code stays in the code repos.** Playwright specs go in the `e2e/` folder of the zis-2.0 code repo, and JUnit tests in `src/test/` of the zis code repo. Only documents go here.

## Contact

QA Analyst, ICT Department.
