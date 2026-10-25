# AGENTS.md

Guidance for AI coding agents working in this repository. `CLAUDE.md` is a symlink to
this file.

Make sure to also read [`README.md`](README.md) for the human-facing docs: how a board
mounts this library, the layout, and the register of every part.

## Project overview

The shared KiCad 10 library of the solar-cooker-UHasselt boards: symbols, footprints
and 3D models KiCad's own library does not have, each checked against its datasheet,
and the custom DRC rules per board maker. No code. Board repos mount it as a git
submodule at `kicad-common/` and pin a commit, so a change reaches a board only when
that board updates its submodule.

Before adding a part, check whether KiCad's own library has it: a board uses KiCad's
part where one fits, and only what is missing lives here.

## Repository map

```
symbols/solar_cooker.kicad_sym      # all symbols, one library
footprints/solar_cooker.pretty/     # all footprints, one .kicad_mod per part
3dmodels/                           # <part>.step in lowercase, named by the footprints
design-rules/                       # custom DRC rules, <maker>-<class>.kicad_dru
README.md                           # use in a board, layout, the register of parts
.github/CODEOWNERS                  # who approves pull requests
.agents/                            # commit and documentation conventions
```

## Where things live

| To change…                       | Edit                                                                           |
| -------------------------------- | ------------------------------------------------------------------------------ |
| A symbol                         | `symbols/solar_cooker.kicad_sym`, in KiCad's symbol editor                     |
| A footprint                      | `footprints/solar_cooker.pretty/<part>.kicad_mod`, in KiCad's footprint editor |
| A 3D model                       | `3dmodels/<part>.step`, and the footprint that names it                        |
| A board maker's limits           | `design-rules/<maker>-<class>.kicad_dru`                                       |
| Where a part came from, its date | its row in the README's register                                               |

The maintainer edits symbols and footprints in KiCad. Agents read the files, check
parts against datasheets, and change text files (the register, the design rules, docs).

## Setup

Nothing to install for the files. The check needs KiCad 10 (`kicad-cli`).

## Checks

The libraries stay in KiCad 10 format and the register matches them: the check in
[.agents/commit_conventions.md](.agents/commit_conventions.md), run on a copy. Each 3D
model a footprint names exists in `3dmodels/`. A change is then tested in a board: its
submodule pointed at the change, `just check` there.

Before finishing, re-read the diff and check it follows existing patterns, is minimal
(nothing unrelated touched), and passes the check. (Our convention.)

## Tools

- **kicad-happy.** Its `datasheets` and `kicad` skills help check a part against its
  datasheet. House rules it does not know are in this file and `.agents/`.

## Security

- No secrets, no CI. Datasheet PDFs stay in `tmp/`, never committed: a register row
  links the datasheet instead.
- Never commit `tmp/`, backups or lock files. `.gitignore` covers them.

## Commit & PR guidelines

**Never commit. Propose the message only**, and the maintainer reviews and commits
manually. Do not run `git commit` / `git push` / `git add` unless explicitly told to.
(Our convention.)

Use the `propose-commit` skill: Conventional Commits, one part per commit with its
register row, the message handed over in `tmp/`. This repo's types, scopes and rules:
[.agents/commit_conventions.md](.agents/commit_conventions.md). **Never use em dashes or
semicolons** in commit messages or any written output. (Our convention.)

Pull requests follow the org's
[CONTRIBUTING.md](https://github.com/solar-cooker-UHasselt/.github/blob/main/.github/CONTRIBUTING.md),
enforced by the org's
[rulesets](https://github.com/solar-cooker-UHasselt/.github/tree/main/rulesets).

## Documentation

Use the `check-docs` skill when writing or reshaping a README or an agent file: one
Diátaxis type per file, the README shape, naming. This repo's doc map, exceptions and
link check: [.agents/documentation_conventions.md](.agents/documentation_conventions.md).
(Our convention.)

## Deployment

- None. Boards pick up a change by updating their submodule and committing the new
  pointer. Renovate does it in ds3231.

## References

- agents.md spec: <https://agents.md/>
- KiCad library conventions: <https://klc.kicad.org/>
- KiCad file formats: <https://dev-docs.kicad.org/en/file-formats/>
