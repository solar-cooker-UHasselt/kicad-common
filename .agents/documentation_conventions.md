# Documentation conventions

This repo's documentation rules. The shared conventions are the `check-docs` skill,
which reads this file first and follows it where the two differ.

## Doc map

Each document and the one type it is:

| File                                   | Type      |
| -------------------------------------- | --------- |
| `README.md`                            | how-to    |
| `AGENTS.md`                            | reference |
| `.agents/commit_conventions.md`        | reference |
| `.agents/documentation_conventions.md` | reference |

The README's register of parts is reference inside the how-to, as the shared
conventions allow for a list of what ships.

## Where things go

- **`docs/`**: none yet.
- **`.agents/`**: `commit_conventions.md` (types, scopes, the library check, one part
  per commit), `documentation_conventions.md` (this file).

## Exceptions

- **Part names.** A footprint is named after its part as printed, with its capitals and
  hyphens: `MEM2061-01-188-00-A.kicad_mod`. Its 3D model is the same name in lowercase,
  `mem2061-01-188-00-a.step`, as the README's layout says.
- **`solar_cooker.pretty/`.** A footprint library is a directory KiCad names
  `<library>.pretty`, after the library name `solar_cooker` the boards use.
- **Design rules.** `<maker>-<class>.kicad_dru`, with hyphens, e.g.
  `eurocircuits-proto-6c.kicad_dru`. Boards name it in their justfile (`rules`), so a
  rename is a change in every board.

## Check

Every relative link in the repo's own Markdown resolves: the fallback link check in the
`check-docs` skill.
