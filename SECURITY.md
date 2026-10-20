# Security Policy — Creytix fleet (default)

This is the fleet-default security policy (from `moshbenav-commits/.github`).
It applies to every repo in the fleet that doesn't carry its own `SECURITY.md`.

## Reporting a vulnerability

Do **not** open a public issue for a security problem.

1. Use GitHub's **private vulnerability reporting** on the affected repo
   (Security tab → "Report a vulnerability"), if enabled there; or
2. Contact the repo owner (`@moshbenav-commits`) directly through GitHub.

Include: the affected repo/commit, what the issue is, and the impact you
believe it has. You should get an acknowledgement before any public discussion.

## Fleet security baseline

The fleet standard (`REPO_STANDARD.md`, §5) requires, for every repo:

- No secrets in git. `.env`/key material is gitignored fleet-wide; a secret
  that lands in history is treated as compromised — **rotate it first**,
  deleting the line does not remove it from history.
- Branch protection on the default branch: force pushes and branch deletion
  blocked, pull-request review required.
- Secret scanning + push protection are enabled on every repo where the
  account plan supports them (all public repos today); the compensating
  control elsewhere is the no-secrets rule above plus Gitleaks in CI.

## Supported versions

Fleet repos are maintained at `HEAD` of their default branch. Security fixes
land there; there is no backport branch unless a repo's own `SECURITY.md`
says otherwise.
