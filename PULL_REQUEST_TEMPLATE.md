## What changed

<!-- One or two sentences. Link the issue/plan if there is one. -->

## Why

<!-- The reason this change exists. "Why" outlives "what" in history. -->

## How it was verified

<!-- The real end-to-end scenario you ran — not just "tests pass".
     For deploys: the live check. For destructive/batch changes: the undo. -->

## Fleet standard checklist

- [ ] No secrets, `.env` values, keys, or build output committed
- [ ] Shared code consumed from its SSOT / Creytix tool — no new copy to drift
- [ ] `CODEOWNERS` still accurate for the paths touched
- [ ] PLAN.md / README updated if this changes what the repo is or how it runs
