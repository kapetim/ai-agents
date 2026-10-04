# .github

Default community health files and shared automations for the **kapetim** account — the honeypot that every repo inherits from.

## Inherited by every repo

| File | Effect |
| --- | --- |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | shown when opening an issue or pull request |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | community standards |
| [`SECURITY.md`](SECURITY.md) | private vulnerability reporting |
| [`SUPPORT.md`](SUPPORT.md) | where to get help |
| [`.github/FUNDING.yml`](.github/FUNDING.yml) | sponsor button |
| [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md) | default PR body |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE) | default issue forms + chooser config |

A repo **overrides** a default by providing its own copy. Note: a repo-local `.github/ISSUE_TEMPLATE/` (even just `config.yml`) disables **all** inherited issue templates for that repo.

Not inherited: `LICENSE`, `CITATION.cff`, `CODEOWNERS`, `dependabot.yml`, `GOVERNANCE.md`, and workflow files.

## Issue hierarchy — `epic → feature → task`

1. **epic** — a folder/domain parent; **never closed**.
2. **feature** — belongs to one epic; holds tasks.
3. **task** — a single narrow item under one feature.

Guidance: [`guidance/epic.md`](guidance/epic.md) · [`guidance/feature.md`](guidance/feature.md) · [`guidance/task.md`](guidance/task.md).

## Layout

```text
.github/ISSUE_TEMPLATE/   account default forms (devops, scripts, docs) + config
.github/actions/          composite actions used by the workflows
.github/workflows/        reusable stages + manual jobs (see below)
guidance/                 hierarchy docs (root, inert)
scripts/                  shell logic (one script per concern)
templates/                canonical forms copied by the scaffold script
docker/                   Dockerfiles for workflows that need a toolchain
```

## Workflows

All manual (`workflow_dispatch`) or reusable (`workflow_call`) — nothing auto-runs. Every write lands as a **PR, issue, or comment**; no workflow pushes to `main`.

| Workflow | Kind | Purpose |
| --- | --- | --- |
| `profile-refresh.yml` | manual | refresh the profile repo-status table — opens a PR in `kapetim/kapetim` |
| `issue-*.yml` | reusable + manual | issue lifecycle stages (find gaps, create epic, review, pick task) |
| `pr-*.yml` | reusable + manual | PR lifecycle stages (create, fix, review, janitor) |
| `ensure-epics.yml` | reusable + manual | recreate a missing per-folder epic |
| `sync-labels.yml` | reusable + manual | ensure `epic`/`feature`/`task`/`default` exist |
| `release.yml` | manual | bump `VERSION` on a release branch and open a PR |
| `release-tag.yml` | manual | tag merged `main` and publish a release — no `main` commit |
| `test.yml` | manual | this repo's own markdownlint |

## Scaffolding a repo

```sh
./scripts/scaffold-issue-templates.sh /path/to/repo --dry-run
./scripts/scaffold-issue-templates.sh /path/to/repo
```

Writes the default trio + `feature`/`task`, **one epic per `src/*` folder**, a PR template, and the manual `ensure-epics` / `sync-labels` callers.

## Repo numbering

The account keeps **one source per domain** and a hard cap of **10 repos** (`0–9`). The order lives in [`scripts/profile/repos.txt`](scripts/profile/repos.txt); exceeding 10 fails the profile-refresh workflow. If something doesn't fit, append it to an existing repo instead of adding an 11th.
