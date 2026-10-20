# Creytix Repo Standard

**Adopted:** 2026-10-20 (Ricardo, on the large-org repo-organization research —
`~/workspace/research_notes/large-org-repo-organization-20261020-1436/report.md`, §8/§9)
**Owner:** Creytix platform (account `moshbenav-commits`)
**Applies to:** every repo in the fleet — sites, backends, tools, docs, the incubator.
**Canonical copy:** this file. Mirrored in the fleet `.github` repo
(`moshbenav-commits/.github`) and at `~/workspace/REPO_STANDARD.md`.

The fleet model the standard serves (fleet rule, 2026-10-19/20):

- **Every real site/product gets its own standalone, self-contained repo** — the
  standalone repo is the SSOT for that site.
- **The Expedia-Solutions monorepo is the incubator** — projects without the
  dimension/scope to be their own site live there until they graduate (§6, §8).
- **Genuinely shared code is a Creytix tool** — owned once by Creytix, versioned,
  and shared *with* each repo; it is not copied per repo to drift (§7).

---

## 1. Naming

- **New repos: lowercase kebab-case** (`house-of-bid`, `creytix-auth`). URL-safe,
  shell-safe, safe on case-insensitive filesystems.
- **The name is the product or service** — what is deployed/owned here. Never a
  technology, a person, or a date. No `test2`, `new-app`, `johns-thing`.
- Suffixes only where they encode a real class the fleet filters by:
  `<site>-front`, `<site>-back`.
- **Legacy names are grandfathered.** Much of the current fleet predates this
  rule (`IroncladAutoParts-Front`, `Expedia-Solutions`, default branch `Main` in
  PraxiumLaw). Renaming a live repo breaks clone URLs, deploy configs and
  bookmarks, so renames are a separate migration decision with a plan — never a
  drive-by. The rule binds at creation.

## 2. Ownership

- **Every repo carries a `CODEOWNERS` file at its root.** Format (one line is
  enough for a single-owner repo):

  ```
  # Creytix fleet standard — REPO_STANDARD.md §2
  * @moshbenav-commits
  ```

  Monorepos and multi-area repos add per-path lines below the catch-all;
  the last matching pattern wins, and the catch-all stays owned by the platform.
- **The owner is a team/service, never an individual** (GitHub's own rule:
  "for any repository critical enough to outlast a single person, ownership
  should be a team or a service"). Fleet reality today: the fleet lives on a
  personal account, so the platform team *is* the account owner and
  `@moshbenav-commits` is the only owner handle that exists. **On any migration
  to a GitHub organization, every `CODEOWNERS` handle is replaced with the
  platform team handle (`@<org>/platform`) in the same migration commit.**
- Ownership is also recorded per repo in the fleet index
  (`~/workspace/PROJECT_INDEX.md`) — the account is a personal account, so
  org custom properties (`ownership-type`/`ownership-name`) are **not
  available**; the index + CODEOWNERS are the fallback until an org exists.
- A repo or incubator project with no owner gets one warning cycle at the
  quarterly sweep, then is archived (§6). No owner = no repo.

## 3. Org-default community files — one `.github` repo

- The fleet keeps ONE defaults repo: **`moshbenav-commits/.github`** (public —
  required for defaults to apply on a personal account). It holds:
  `SECURITY.md`, `CONTRIBUTING.md`, `PULL_REQUEST_TEMPLATE.md`,
  `ISSUE_TEMPLATE/`, and this standard (`REPO_STANDARD.md`).
- These defaults apply automatically to every repo that doesn't carry its own
  copy. A repo overrides a default only deliberately, by adding its own file —
  never by accident.
- Change a default in the `.github` repo only; never fork a default into a
  product repo to tweak it.

## 4. New sites start from the site template (golden path)

- New sites are created from **`moshbenav-commits/site-template`** (marked as a
  GitHub template repo). Being born compliant is the point: the template ships
  the §5 `.gitignore` baseline, `CODEOWNERS`, a README skeleton, `SECURITY.md`,
  and the fleet's validated standalone build pattern for Next/OpenNext sites.
- The template **references** shared tooling; it does not copy it. Build scripts
  shared by all sites are a Creytix tool (§7). Until that tool ships as a
  versioned package, a new site vendors the two build scripts from their SSOT
  (monorepo `scripts/` / `nr-design-scripts`) and records the SSOT version in
  its README — vendoring is the documented interim mechanism, and the drift it
  causes is tracked as a known debt (the flatten script already exists in 5
  versions across 14 repos — that is what §7 exists to end).
- The traffic beacon is wired per the template's pattern (component in the root
  layout, `NEXT_PUBLIC_TNOTIFY_API` at build time). The beacon server and
  `tnotify.js` SSOT live in `align-feedback`; the template points there instead
  of carrying another copy.

## 5. Hygiene baseline (every repo)

### Required files

| File | Rule |
|---|---|
| `README.md` | Exists in every repo. Minimum: what it is, owner, how to build/run/deploy. A stub README is marked as a stub — never fabricated content. |
| `CODEOWNERS` | §2. |
| `SECURITY.md` | Satisfied by the `.github` default unless the repo carries its own. |
| `.gitignore` | Contains the baseline block below, merged with repo-specific rules. |

### .gitignore baseline (verbatim block)

Every repo's `.gitignore` contains this block (append-only merge; a repo's own
rules are never removed by the standard — blanket patterns that swallow real
source are a hazard class handled by the fleet audit, e.g. Ironclad's `reports/`
rule that hid `src/components/admin/reports/`):

```
# --- Creytix repo standard: .gitignore baseline v1 (2026-10-20) ---
# Dependencies
node_modules/
vendor/
.venv/
venv/
__pycache__/
*.pyc
# Build output
dist/
build/
target/
bin/
obj/
.next/
.open-next/
.wrangler/
out/
# Caches, logs, coverage
.cache/
*.log
coverage/
.nyc_output/
# OS / editor
.DS_Store
.idea/
.vs/
.vscode/*
!.vscode/extensions.json
# Local env & secrets (never commit real values)
.env
.env*.local
*.pem
*.key
# --- end Creytix baseline ---
```

Notes: `.gitignore` only affects untracked files — an artifact committed before
the rule exists must be untracked explicitly (`git rm --cached`), it does not
untrack itself. Commit the manifest + lockfile (`package.json` +
`package-lock.json`), never the installed/generated output. **No build
artifacts are tracked** — output must be reproducible from source + lockfile.

### Branch protection (as enforced 2026-10-20)

Classic branch protection on every repo's default branch:

- Force pushes: **blocked**. Branch deletion: **blocked**.
- Pull request reviews: **required, 1 approval, stale approvals dismissed** —
  with admin bypass left ON (`enforce_admins` off). Fleet reality: this is a
  solo-operator + agent fleet whose pushes run as the account owner; the
  protection binds collaborators, apps and future team members today, and
  becomes a hard gate the day a second human or an org arrives. Target state
  (org + rulesets + admins included) is recorded here so nobody mistakes the
  current setting for the finished one.
- **Org rulesets are not available** — the fleet is on a personal account, and
  per-repo rulesets would recreate exactly the per-repo config sprawl the
  research warns against. One org ruleset replaces this section when an org
  exists.

### Secret scanning

- **Public repos: secret scanning + push protection ON** (free on GitHub).
- **Private repos: NOT AVAILABLE on the current account plan** — verified
  2026-10-20 (API: "Secret scanning is not available for this repository").
  Compensating layers, mandatory until the plan changes:
  1. Never commit secrets — `.env`/key patterns are in the baseline above.
  2. **Gitleaks** as the pre-commit/CI check on any repo handling credentials.
  3. A secret that does land is treated as compromised: **rotate first** —
     deleting the line does not remove it from git history.
- Enabling Advanced Security / Secret Protection (or moving to an org plan
  that includes it) is the recorded fix; when it lands, scanning + push
  protection go on fleet-wide in one pass.

## 6. Lifecycle — incubator, graduation, archive

- **Incubator projects carry an owner and a review date.** Every project in
  Expedia-Solutions is listed in the monorepo's `INCUBATOR.md` index with:
  project path, owner, status, and a **review date**. Unknown review dates
  default to **adoption date + 90 days** and are marked `defaulted`.
- Incubator project header format (in `INCUBATOR.md`, one row per project):

  ```
  | Project | Path | Owner | Status | Review date | Notes |
  |---|---|---|---|---|---|
  | <name> | <dir>/ | @moshbenav-commits | experiment / active / hibernating | YYYY-MM-DD (defaulted?) | one line |
  ```

- **Quarterly sweep** (aligned to the review dates): every incubator project
  and every repo is graduated, renewed with a new review date, or archived.
- **Archive, never delete.** Archiving is reversible and non-destructive
  (read-only, Actions stop); deletion is not a lifecycle tool in this fleet.
  Delete candidates (empty repos, README-only shells) are listed for Ricardo
  and removed only on his explicit call.
- A repo/project with no owner and no activity past its review date is
  archived after one warning cycle — fail toward archiving, because it is
  reversible (GitHub's Evergreen/ownership cleanups are the model).

## 7. Shared code — Creytix tools (paved road)

- Shared code is promoted to a **Creytix tool only at the SECOND consumer**
  (the Nx/InnerSource rule: code in "shared" has every consumer as a
  stakeholder; premature sharing creates a god-library and a bottleneck).
  Until a second consumer exists, the code lives inside the first consumer.
- A Creytix tool is **owned once** (platform), **SemVer-versioned**, has a
  `CHANGELOG`, and is **consumed, not copied**: versioned package or reusable
  workflow, consumers pin a version range + lockfile, updates arrive as
  automated PRs (Dependabot/Renovate) when that layer lands.
- **Current state (honest):** the fleet's shared packages live as SSOT in the
  monorepo `packages/` (~88 packages) and reach site repos as *vendored
  copies* — the interim mechanism, with known drift (see §4). The standard's
  direction is versioned consumption; each vendored copy names its SSOT and
  version so drift stays measurable until then.
- The paved road is **not mandatory**: a site may step off it, but forking the
  shared tool instead of upgrading is treated as a platform bug report first —
  fix the road before blaming the driver (Netflix/Spotify model).

## 8. Graduation checklist — incubator → own repo

A project graduates out of Expedia-Solutions when it meets the owner test
plus **any two** of the trigger tests (synthesis from the research; no large
org publishes this list — this fleet does):

**Owner test (required):**
- [ ] A named owner (§2) accepts running it as a standalone repo.

**Trigger tests (any two):**
- [ ] **Independent deploy** — it ships on its own cadence or to its own
      production URL/domain, separate from the monorepo's other projects.
- [ ] **Real users** — someone other than the builder depends on it being up.
- [ ] **Distinct cadence or access needs** — its release rhythm, permissions,
      or sensitivity no longer matches its incubator neighbours.
- [ ] **Needed as a dependency** — another project consumes it (at which point
      it needs the stable, versioned interface of §7 anyway).
- [ ] **Degrading the incubator** — its size/build/test weight slows the
      monorepo for everyone else.
- [ ] **No longer an experiment** — it survived its review date with a renewal
      decision of "this is a product now."

**How a graduation runs:** create the repo from §4's template (or §1–§5
applied directly), move code with history where practical (`git filter-repo`
extraction; a clean import + a pointer back to the monorepo is acceptable when
history is worthless), leave a redirect note in `INCUBATOR.md`, and update the
fleet index the same day. Graduation is a move, not a copy — afterwards the
standalone repo is the SSOT and the incubator copy is removed or archived.

---

*Adoption sweep 2026-10-20: CODEOWNERS + .gitignore baseline + README presence
applied per repo (see the sweep report); account features that the current
plan doesn't support are recorded in §5 rather than silently skipped.*
