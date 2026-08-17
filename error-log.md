
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

## 2026-08-17 13:47:28 — Connectivity Precheck False Negative (non-blocking, recurring)
**Routine:** Daily Market Report
**Reason:** Step 0 precheck (`curl https://www.google.com`) failed again. Confirmed via `$HTTPS_PROXY/__agentproxy/status` that the egress gateway returns 403 on CONNECT for www.google.com (`connect_rejected` — policy denial). Verified reachability separately: api.github.com returns 200 and WebSearch worked on every call, so connectivity was fully present.
**Action:** Did NOT abort. Research proceeded via WebSearch (14 queries, 0 retries needed) and the report for 2026-08-17 was generated and committed. finance.yahoo.com, www.cnbc.com, www.reuters.com and www.thestreet.com remain blocked for direct fetch, so all figures were sourced through WebSearch.
**Fix still needed:** Step 0 should probe a permitted endpoint (api.github.com) or simply rely on a WebSearch call instead of google.com. This is the fourth occurrence (see 2026-07-20, 2026-08-10, 2026-08-12); the 2026-07-20 run aborted unnecessarily because of it.
**Second item — branch/instruction conflict:** CLAUDE.md directs commits to `main`, but the session harness designates branch `claude/tender-dirac-w14j58` and forbids pushing elsewhere. Committed to the designated branch, consistent with the last four reports (08-10 through 08-14), which also live there. Note that `origin/main` is now stale at 2026-08-07 — these five reports need merging into main.
