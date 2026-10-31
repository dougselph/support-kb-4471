# support-kb-4471 — what #4471 really holds

Ticket **#4471** on the support desk merged **412 reports** about wrong dates into one issue and
marked it **known**: one bug, one fix queued (the parser reads dates month-first). The reports were
merged by [subject line alone](https://hq.mega.dev) — [decision 0001](src/routes/kb/decisions) —
so the bodies were never read after the merge.

Reading them, and running the desk's own `causes.ts` over the data, #4471 is **three distinct
problems**, not one. The queued parser fix only addresses the first.

## The three problems (one ticket each)

| # | Problem | Reports | Root cause | Fix |
|---|---------|--------:|-----------|-----|
| 1 | [Parser reads dates month-first](../../issues/1) | **374** | shared `sched-prod` parser reads stored dates as `MM/DD`; day-first tenants get day↔month swapped | the queued parser fix |
| 2 | [booking-v3 prints the wrong date hint](../../issues/2) | **33** | the booking-v3 form prints `MM/DD/YYYY`; day-first staff follow it and the form stores it day-first, swapping at input | fix the form's date hint |
| 3 | [One-day / time-zone shift](../../issues/3) | **5** | dates land one day off (DST / UTC offset / cross-zone), not swapped | a day-boundary / time-zone fix |

374 + 33 + 5 = **412**. Each is filed as an issue in this repo (see Issues).

## Evidence

- 427 rows in `reports-4471.json`; 15 are duplicate export pages → **412 unique** reports, all merged into #4471.
- Grouping by `causeOf` (from the kb's `causes.ts`): `parser` = 379, `hint` = 33.
- `desk-notes.json` → `oneDayMove` flags 5 of the `parser` reports as a one-day move, not a swap; `CauseTable` already footnotes them. Treated as their own problem, that leaves `parser` swaps = 374.

## The kb page

- Source: [`src/routes/kb/incidents/4471/breakdown/+page.svx`](src/routes/kb/incidents/4471/breakdown/+page.svx) — drops into the kb repo at that path; signed with the challenge ticket in its `signature:` frontmatter and ends with `<RenderStamp {signature} />`.
- As the desk will see it (rendered): [`rendered/4471-breakdown.html`](rendered/4471-breakdown.html) — desk ref **DR-7J3R-5VNJ**.
