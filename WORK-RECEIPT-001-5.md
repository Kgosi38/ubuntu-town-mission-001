# UBUNTU TOWN WORK RECEIPT

Mission: UT-TECH-001.5
Builder: Kgosi
Title: The Agentic Handshake

## Estate before mission
Mission 001 was already complete: Android/Termux environment, Git repository connected to GitHub (Kgosi38/ubuntu-town-mission-001), and a live static site deployed via Cloudflare. No Supabase project, no agent tooling, and no branch/preview workflow existed yet.

## Hermes
Version: v0.21.2 (2026.9.11), upstream 5eb99eb2
Doctor result: Healthy — Python 3.13.13, OpenAI SDK 2.24.0, install via git at ~/.hermes/hermes-agent
Provider configured: [name only - NO KEY]

## Git / deployment
Feature branch: mission/001-5-agentic-handshake
Pull request: https://github.com/Kgosi38/ubuntu-town-mission-001/pull/1
Preview URL: https://mission-001-5-agentic-handshake-ubuntu-town-mission-001.mogogolek832.workers.dev
Production URL: not yet deployed — still on feature branch, PR not merged
Final production commit: N/A — pending human approval and merge

## Supabase
Development project: ut-apprentice-lab (https://aqwgduxatheikayigcym.supabase.co)
Migration: supabase/migrations/20260917_create_missions.sql — creates public.missions table
RLS policy: "public can read mission log" — for select, to anon, using (true)
Data exposed to client: mission_code, title, status, proof_url, started_at, completed_at (no secrets; publishable/anon key only)

## Agent commission
Prompt file: prompts/HH-001.md — HH-001, Mission Log Integration
Files agent inspected: index.html, supabase/migrations/20260917_create_missions.sql, wrangler.jsonc, WORK-RECEIPT.md, reports/HH-001-REPORT.md, AGENTS.md
Files agent changed: index.html (added "My Mission Log" section — CSS, HTML, Supabase client JS); created reports/HH-001-REPORT.md

## Human review
What I accepted: The Mission Log section structure, styling, loading/empty/error states, and the fetch logic using the Supabase JS client with the anon/publishable key.
What I rejected or changed: Found and fixed a bug the agent's own report did not catch — the SUPABASE_URL constant included a trailing "/rest/v1/" path, which broke window.supabase.createClient() at runtime ("Invalid path specified in request URL"). Corrected it to the bare project URL.
Security checks: Confirmed via git diff and grep that only the client-safe anon/publishable key appears in tracked files; no service-role/secret key was requested or used at any point.
Runtime checks: Verified the live Cloudflare preview twice — first showing the runtime error, then confirming both mission records (UT-TECH-001 COMPLETED, UT-TECH-001.5 IN_PROGRESS) render correctly with proper status badges after the fix.

## Failure / debugging log
1. First Cloudflare Workers Build failed with "Missing entry-point to Worker script or to assets directory" — the project had no wrangler.jsonc. Fixed by adding a wrangler.jsonc pointing wrangler at the repo root as a static assets directory. Re-deployed successfully.
2. After Hermes's commit was pushed, the live preview showed "Error loading mission log: Invalid path specified in request URL." Traced this to the SUPABASE_URL constant in index.html containing a "/rest/v1/" suffix that the Supabase JS client does not expect. Fixed with a one-line sed edit, committed, and pushed. Preview then rendered correctly.

## What the agent did well
Hermes was appropriately cautious: it inspected the repo thoroughly before writing any code, correctly identified that it needed client-safe config (project URL + anon key) rather than guessing or requesting a secret key, verified the Supabase JS SDK CDN was reachable before using it, tested a live data fetch with real credentials before declaring success, and stopped cleanly for human approval with a clear, itemized report instead of merging or deploying on its own.

## Where the agent was wrong or incomplete
The agent used an incorrect format for the Supabase project URL (included the REST API path suffix), which broke the client at runtime. Its own report claimed the acceptance criteria were verified with live data, but this was only true after my manual review caught and fixed the bug — the agent's self-reported "live data confirmed" was based on a curl test against the REST endpoint directly, not an actual test of the JS client integration it built.

## What I can now do without help
I can now commission an AI agent with a scoped, constrained task; review its file diff line by line before trusting it; independently debug a live deployment issue (tracing a runtime error back to a specific line of generated code); and verify agent-reported success against actual runtime evidence rather than taking its word for it.

## One thing I would change in the next agent commission
I would explicitly require the agent to test the exact client code path it writes (not just a raw curl equivalent) before reporting the acceptance criteria as verified, so integration bugs like the URL format issue are caught by the agent itself rather than by human review afterward.
