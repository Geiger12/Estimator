# GrayMatter Estimator

A simple fixed-price estimating app for SCADA and HMI work. It runs entirely in the browser from one file, `index.html`, with no server, build step or add-ins.

## What it does

- **Open Excel** reads three kinds of workbook:
  - The GMS estimator (.xlsm): every estimating tab listed on the Financial tab (with its Include Yes/No), tasks, engineer levels, rates, expenses, the expense rate list, the DOA approval matrix and the HMI Breakdown skill mix.
  - The Organized workbook: graphics, popups and dashboards scored by complexity, loaded into the Complexity matrix on a worksheet you choose. Research and calibration tabs are listed but left out by default.
  - Workbooks saved from this app.
- **Dashboard** shows the final fixed price across included worksheets, a labor-margin slider, a target-price solver, the price build-up, a Financial-style worksheet roll-up, hours by engineer level and phase, and checks that reconcile against the workbook's Financial tab (T35).
- **Price breakdown** is where you enter and price work, one worksheet at a time. Tasks are editable in place and grouped by phase, with rate, cost, margin and price beside each one. Below them come the complexity matrix groups, PM, contingency, escalation, expenses and the worksheet total, then the final total and labor rates by engineer level.
- **Complexity matrix** holds hours per item by type (Graphic, Popup, Dashboard or your own) at complexity 0 to 6, a skill mix across engineer levels, and the line list (area, title, type, complexity, quantity, worksheet, level). Lines can be added, copied, deleted and filtered; they feed the Price breakdown grouped by area and type.
- **Expenses** holds travel and living, one worksheet at a time.
- A status line on every tab shows price, cost, margin, hours and the required approver, like the workbook's status bar.
- **Save as Excel** writes an .xlsx with Summary, Worksheets, Tasks, Complexity, Expenses, Rates and Approval tabs, all with working formulas, so you can keep editing in Excel and reopen the file here.

## How pricing works

Per worksheet, matching an estimating tab:

- Total hours = ROUNDUP(quantity × hours each)
- PM hours = ROUNDUP(PM % × PM base hours)
- Contingency hours = ROUNDDOWN(contingency % × (task hours + PM hours))
- Contingency cost = contingency % × (task cost + PM cost)
- Labor price = cost × (1 + escalation) ÷ (1 − margin). An engineer level can carry its own margin (the workbook prices iOPS at 75%).
- Expense price = cost × (1 + expense escalation) ÷ (1 − expense margin)
- Complexity line hours = quantity × matrix hours. For a per-complexity type, a blank matrix cell means hours per point × complexity (the Organized workbook uses 6 for graphics and 1.5 for popups); a typed cell overrides it. Fixed types such as dashboards use the sum of their stage hours.
- Lines on the skill mix are costed at the blended rate (share × cost per hour across levels) and priced level by level with each level's margin. They count toward the PM base on their worksheet.

Across worksheets, matching the Financial tab: final price, cost and hours are the sums of included worksheets, and the final margin is (price − cost) ÷ price, the workbook's `Final_Total_Margin` (Financial T35). The required approver follows the DOA tab's tests on price, labor margin, expense margin and the overall margin floor.

**PM base.** On some estimator tabs the PM formula (for example `SUM(N8:N20)`) stops one row short of the last task line. On import the app follows the workbook rule so totals match T35 exactly, and marks the excluded lines. Switch PM base to All tasks in Rates and settings to count every task.

**Not yet carried:** materials, subcontracts, commissioned software, time-and-materials pricing, overtime multipliers and the extended payment-terms adjustment. The dashboard warns when an included worksheet has any of these in the workbook.

## Data handling

- This repository holds code only. It contains no rates, margins, productivity norms (hours per complexity point, stage hours) or client data; those load from the workbooks you open.
- Workbooks are read inside the browser and never uploaded. The browser keeps a working copy in local storage.
- Never commit estimator workbooks or saved estimates. `.gitignore` blocks the common file types.
- Store workbooks in approved GrayMatter locations.
- GitHub Pages sites are reachable by anyone with the link, even from a private repository, unless the organization uses private Pages on GitHub Enterprise Cloud.

## Publish with GitHub Pages

1. Create a repository. Private is recommended.
2. Add `index.html`, `README.md`, `.gitignore` and `.nojekyll` at the root.
3. Go to Settings > Pages, choose Deploy from a branch, select `main` and `/ (root)`, then save.
4. After a minute or two the site is live at `https://<owner>.github.io/<repository>/`.

To update the app, replace `index.html` and commit.

## Requirements

A current version of Edge, Chrome, Firefox or Safari. The app uses the browser's built-in decompression to read Excel files. Fonts load from Google Fonts and fall back to system fonts offline.

## Review

Estimates are drafts. A qualified person reviews hours, rates and margin before anything is sent.
