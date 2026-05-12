# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

This is the **GitHub profile README** repository for the user `AuroraMaster` (the special `username/username` repo that renders on the GitHub profile page). It is not a software project — it contains a single Markdown profile page plus GitHub Actions that regenerate SVG visualizations on a schedule.

There is no build, no test suite, no package manager, and no application code. Edits are either:
- changes to `README.md` (the profile content), or
- changes to `.github/workflows/*.yml` (the automation that produces SVGs).

## Generated vs. hand-edited files

Most files in this repo are written **by bots**, not humans. Do not hand-edit them — the next workflow run will overwrite the changes:

- `github-metrics/*.svg` and `github-metrics.svg` — regenerated daily by `.github/workflows/metrics.yml` (lowlighter/metrics).
- `dist/github-contribution-grid-snake*.svg` — regenerated every 12 hours by `.github/workflows/snake.yml` (Platane/snk).
- `profile-snake-contrib/*.svg`, `profile-3d-contrib/*.svg` — generated SVGs served via jsDelivr in the README.
- The block between `<!--START_SECTION:waka-->` and `<!--END_SECTION:waka-->` inside `README.md` — rewritten daily by `.github/workflows/waka.yml` (athul/waka-readme). Edit text *outside* those markers; leave the markers in place.

Hand-edit only: `README.md` (outside the waka markers), `beautiful.css`, `assets/`, and the workflow YAML files.

## Workflows and required secrets

Three scheduled workflows live in `.github/workflows/`. All three require repo secrets that must be set in GitHub repo settings — they cannot be tested locally without those tokens:

| Workflow | Schedule | Secrets used |
|---|---|---|
| `metrics.yml` | daily `0 0 * * *` | `METRICS_TOKEN` (GitHub PAT with `repo`, `read:user` etc. for lowlighter/metrics), `GH_TOKEN` (committer), `WAKATIME_API_KEY` |
| `snake.yml` | every 12h `0 */12 * * *` | `GH_TOKEN` (used both as `GITHUB_TOKEN` for the action and to push the commit) |
| `waka.yml` | daily `0 0 * * *` | `GH_TOKEN`, `WAKATIME_API_KEY` |

Each workflow can also be triggered manually via `workflow_dispatch` from the Actions tab.

The metrics workflow runs `lowlighter/metrics@latest` ~14 times in sequence, each producing one SVG into `github-metrics/`. When adding or removing a plugin, mirror the pattern of the existing steps (set `filename:`, `base: ""`, the plugin flags).

## Conventions when editing

- Commit messages in this repo are short Chinese sentences with an emoji prefix (`📊 更新…`, `🐍 更新贪吃蛇动画`, `🔧 修复…`). Match that style.
- The username `AuroraMaster` appears in many places (workflow `user:` fields, jsDelivr URLs in README, the git remote, the `Platane/snk` `github_user_name`). If the username ever changes, all of these must be updated together.
- The README uses `<picture>` + `prefers-color-scheme` to swap dark/light SVG variants; preserve both `<source>` tags when editing image blocks.
- `config_timezone: Asia/Shanghai` is set in metrics plugins — keep it consistent if adding new metric steps that care about local time.
