
## 2026-07-20 13:14:35 — Connection Failure
**Routine:** Daily Market Report
**Reason:** No internet connection after 10 attempts (approx 5 minutes of retrying)
**Action:** Report was not generated. Will retry at next scheduled run.

## 2026-08-10 14:04:29 — Connectivity Precheck False Negative (non-blocking)
**Routine:** Daily Market Report
**Reason:** Step 0 precheck (`curl https://www.google.com`) failed all attempts — the egress proxy returns HTTP 403 on CONNECT for general web hosts. This is a network *policy* denial, not a loss of connectivity.
**Action:** Research proceeded normally via WebSearch (16 queries, 0 retries needed) and the report for 2026-08-10 was generated and committed. No data was lost.
**Fix needed:** The Step 0 precheck should target a permitted endpoint rather than google.com, otherwise it will keep aborting healthy runs (see also the 2026-07-20 entry, which may have been the same false negative).

## 2026-08-12 13:52:59 — Connectivity Precheck False Negative (non-blocking, recurring)
**Routine:** Daily Market Report
**Reason:** Step 0 precheck (`curl https://www.google.com`) failed all 10 attempts again. Verified via `$HTTPS_PROXY/__agentproxy/status` that the egress proxy returns 403 on CONNECT for www.google.com (`connect_rejected` — policy denial). Connectivity is present; the host is simply not on the egress allowlist.
**Action:** Did NOT abort. Research proceeded via WebSearch (18 queries, 0 retries needed) and the report for 2026-08-12 was generated and committed. Note that finance.yahoo.com, www.cnbc.com and www.thestreet.com are also blocked for direct WebFetch, so index levels were sourced through WebSearch instead.
**Fix still needed:** Step 0 should probe a permitted endpoint (or use a WebSearch call) instead of google.com. This is the third occurrence (see 2026-07-20 and 2026-08-10 entries); the 2026-07-20 run aborted unnecessarily because of it.

## 2026-08-20 13:39:55 — Connectivity Precheck False Negative (non-blocking, 4th occurrence)
**Routine:** Daily Market Report
**Reason:** Step 0 precheck (`curl https://www.google.com`) failed all 10 attempts again (~5 min of retrying). Confirmed via `$HTTPS_PROXY/__agentproxy/status` that the egress proxy returns 403 on CONNECT for www.google.com (`connect_rejected` — policy denial, logged with timestamps). Connectivity is present; google.com is simply not on the egress allowlist.
**Action:** Did NOT abort. Research proceeded via WebSearch (17 queries, 0 failures, 0 retries needed) and the report for 2026-08-20 was generated and committed.
**Fix still needed:** Step 0 should probe a permitted endpoint or make a single WebSearch call instead of curling google.com. This is the 4th occurrence (see 2026-07-20, 2026-08-10, 2026-08-12); the 2026-07-20 run aborted unnecessarily because of it. Recommend replacing the Step 0 block outright.

## 2026-08-20 13:39:55 — Branch Target Conflict (non-blocking, needs a decision)
**Routine:** Daily Market Report
**Reason:** Conflicting git instructions. CLAUDE.md and the scheduled prompt both say commit directly to `main` and never create a branch; the session harness assigns branch `claude/tender-dirac-sgo2jr` and forbids pushing elsewhere without explicit permission.
**Current state:** `main` is at "Daily market report: 2026-08-14". The 2026-08-18 and 2026-08-19 reports exist ONLY on `claude/tender-dirac-sgo2jr`, which is 2 commits ahead of `main`.
**Action:** Committed 2026-08-20 to `claude/tender-dirac-sgo2jr` to preserve a coherent report history — pushing to `main` would fork the series and produce a `main` whose latest report references two prior reports that are absent from that branch.
**Fix needed:** A human should decide the canonical branch. If `main` is canonical, merge `claude/tender-dirac-sgo2jr` into `main` (fast-forward, 2 commits) and reconcile the harness branch assignment with CLAUDE.md so future runs stop diverging.
