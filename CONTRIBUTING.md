# Contributing — Creytix fleet (default)

This is the fleet-default contributing guide (from `moshbenav-commits/.github`).
A repo with its own `CONTRIBUTING.md` overrides it. Everything here follows
the **Creytix Repo Standard** (`REPO_STANDARD.md` in the `.github` repo).

## The short version

1. **One repo = one product.** Sites live in their own repo (the standalone
   repo is the SSOT); small/sub-scale projects live in the Expedia-Solutions
   incubator until they graduate (standard §6/§8). Don't put code in the
   wrong home — if you're not sure where it goes, that's the thing to settle
   first.
2. **Shared code is a Creytix tool, not a copy.** If a second repo needs it,
   it gets promoted, owned once, and versioned (standard §7). Don't paste a
   second copy to drift.
3. **Never commit secrets or build output.** The standard `.gitignore`
   baseline (standard §5) is in every repo for a reason. A leaked secret is
   rotated first, discussed second.
4. **Commit after every work session; push before ending the day.** Keep the
   repo's `PLAN.md` (where one exists) current.

## Pull requests

- Default branches are protected: PR + 1 approval is the standard path.
- Say what changed, why, and how you verified it. For anything user-facing,
  verification means the real end-to-end scenario works, not just that the
  code runs its own demo.
- Batch/destructive changes land as approve → apply, and the apply output
  includes the undo.

## Ownership

Every repo has a `CODEOWNERS` (standard §2). PRs touching owned paths request
that owner automatically. Ownership is a team/service, never a person — on
this account the platform handle is `@moshbenav-commits` until an org exists.
