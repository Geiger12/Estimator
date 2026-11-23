# GrayMatter Estimator

A simple fixed-price estimating app for SCADA and HMI work. It runs entirely in the browser from one file, `index.html`, with no server, build step or add-ins.

## What it does

- **Open Excel** reads the GMS estimator workbook (.xlsm) and pulls the tasks, engineer levels, rates and settings from an estimate tab such as SCADA. It also opens workbooks saved from this app.
- **Tasks** is where you enter phase, task, engineer level, quantity and hours.
- **Dashboard** shows the fixed price, a margin slider, the price build-up, and hours by engineer level and phase.
- **Save as Excel** writes an .xlsx with Summary, Tasks and Rates tabs and working formulas, so you can keep editing in Excel and reopen the file here.

## How pricing works

- Total hours = ROUNDUP(quantity × hours each)
- PM hours = ROUNDUP(PM % × task hours)
- Contingency hours = ROUNDDOWN(contingency % × (task hours + PM hours))
- Contingency cost = contingency % × (task cost + PM cost)
- Price = cost × (1 + escalation) ÷ (1 − margin)

On the SCADA and Onsite Visits tabs of the estimator template, the PM formula (`SUM(N8:N20)`) stops one row short of the last task line. This app counts every task, so its total can be slightly higher. The dashboard shows and explains any difference.

## Data handling

- This repository holds code only. It contains no rates, margins or client data.
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
