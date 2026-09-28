# {{WORKING_GROUP_NAME}}

## Project Overview
{{ONE-PARAGRAPH DESCRIPTION OF THE WORKING GROUP: its mission, what it builds or studies, and its
audience.}}

This repository holds the working group's public website (a Jekyll site in `docs/`, deployed to
GitHub Pages by `.github/workflows/pages.yml`), along with its software (`src/`), its hardware
design files (`hardware/`), and its AI governance scaffolding.

**Lead**: {{LEAD_NAME_CALLSIGN_AND_AFFILIATION}}
**Collaborators**: {{COLLABORATORS: names, callsigns, and roles}}
**HamSCI Working Group**: {{WORKING_GROUP_NAME}}
**Institution**: {{INSTITUTION, or "none: this is a volunteer project"}}
**Funder**: {{FUNDER_AND_GRANT_NUMBER, or "unfunded"}}
**Project period**: {{PROJECT_PERIOD}}

## Project Goal
{{PROJECT_GOAL, 1 to 3 sentences.}}

## Standing Rules

These two are binding on every HamSCI project and are imported here so they load into context
automatically:

@.claude/rules/ai-governance.md
@.claude/rules/hamsci-data.md

Two further rule files are optional, and are scoped by their own `paths:` frontmatter to the
file types they govern:

- `.claude/rules/latex-writing.md` applies to `.tex`, `.bib`, `.cls`, and `.sty` files
- `.claude/rules/python-code.md` applies to `.py`, `pyproject.toml`, and `requirements*.txt`

Delete whichever the project does not use. To have one of them load unconditionally instead, add
an `@` import line for it above.

## Repository Structure

This repository starts from the HamSCI `hamsci-wg` template, which incorporates the HamSCI
`ai_project_template` governance scaffold. Add or remove top-level directories to match the
working group. The scaffold provides:

```
{{REPO_NAME}}/
├── CLAUDE.md                     ← this file; project instructions for Claude
├── README.md                     ← working-group description and setup checklist
├── LICENSE
├── CITATION.cff                  ← make the working group's outputs citable
├── Gemfile
├── .gitignore
├── .gitmodules                   ← present only if you add submodules
├── .claude/
│   ├── settings.json
│   ├── commands/commit.md        ← /commit workflow
│   └── rules/
│       ├── ai-governance.md      ← required
│       ├── hamsci-data.md        ← required
│       ├── latex-writing.md      ← delete if no LaTeX
│       └── python-code.md        ← delete if no Python
├── .github/
│   ├── ISSUE_TEMPLATE/
│   └── workflows/pages.yml       ← builds and deploys the website
├── ai/
│   ├── ai_usage_log.md           ← mandatory AI session log
│   └── GETTING_STARTED.md        ← delete once the working group is running
├── docs/                         ← Jekyll website source; every page here is published
│   ├── _config.yml               ← site title, description, baseurl
│   ├── 1_about.md … 6_results.md ← site pages, shown in filename order
│   ├── _bibliography/            ← publications.bib, datasets.bib (jekyll-scholar)
│   └── assets/
├── src/                          ← software, or a link to the software repository
├── hardware/                     ← editable hardware design sources
└── {{PROJECT-SPECIFIC FOLDERS}}  ← e.g. notes/, posters/, data/
```

**`docs/` is the published website.** Every Markdown file placed there becomes a public page,
and Jekyll treats double-brace expressions as Liquid template tags. Keep session notes, drafts,
and developer documentation out of `docs/`. Build the site locally with
`cd docs && bundle exec jekyll serve`.

## Working Conventions

**Session notes.** Keep one dated notes file per working session in `notes/`, named
`YYYY-MM-DD_<topic>.md`, recording what was decided, why, what it depends on, and what is still
open. Write them for a reader with no context, which in practice means a future Claude session
and a future you.

**Commits.** Use the `/commit` command. It logs the AI session, commits submodules first, then
commits the main repo. Prefix AI-assisted commits with `[AI-assisted]`. Reference tracking
issues (`refs #N`, or `closes #N` only when completion is yours to declare).

**This repository is public.** It publishes the website, so everything committed to it is visible,
including files outside `docs/`. Unpublished manuscripts, pre-acceptance figures, and unreleased
data belong in a separate private repository.

**Never push without explicit instruction.** Never force-push or hard-reset. Fetch and verify
remote state before any push.

**Project boards and issue status are human-curated.** Read them freely; propose changes and
name the exact command rather than running it.

## Submodules (optional)

If the project includes submodules (an Overleaf manuscript, a separate code repository, a
hardware design repo), the commit and push order is fixed:

1. Commit **inside** the submodule
2. Commit the updated submodule pointer in this repo
3. Push the submodule
4. Push this repo

Never the reverse at either stage. A parent pushed ahead of its submodule looks correct on the
machine that did it and breaks for every clone, because the recorded pointer names a commit no
remote has. Verify before pushing a parent:

```bash
git -C <submodule> branch -r --contains HEAD   # empty output = local only; push the submodule first
```

Add submodules with:

```bash
git submodule add https://git.overleaf.com/<id> overleaf      # Overleaf manuscript
git submodule add git@github.com:HamSCI/<repo>.git <path>     # HamSCI code repo
```

The `/commit` workflow auto-detects submodules via `git submodule status`.

## AI Governance

Every substantive AI session is logged in `ai/ai_usage_log.md` **before** the work is committed.
Use `/commit`, which enforces the ordering. The full policy stack is in
`.claude/rules/ai-governance.md`, which is imported above.
