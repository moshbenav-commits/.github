# Creytix fleet — default community health files

This repository holds the **org-default** community health files for every repo
in the `moshbenav-commits` fleet, per the Creytix Repo Standard
([`REPO_STANDARD.md`](REPO_STANDARD.md), §3).

GitHub applies these defaults to any fleet repo that doesn't carry its own
copy of the file:

- [`SECURITY.md`](SECURITY.md) — how to report a vulnerability
- [`CONTRIBUTING.md`](CONTRIBUTING.md) — how work lands in fleet repos
- [`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md)
- [`ISSUE_TEMPLATE/`](ISSUE_TEMPLATE/) — bug report / feature request forms

Change a default here, in one place — never fork a default into a product repo
to tweak it. A repo overrides a default only deliberately, with its own file.
