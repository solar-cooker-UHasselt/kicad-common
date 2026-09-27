# Commit conventions

This repo's commit rules. The shared procedure is the `propose-commit` skill, which
reads this file first and follows it where the two differ.

## Check

Every commit leaves the libraries in KiCad 10 format, loadable, and the register in
`README.md` matching what is in them. For a changed library, KiCad writes it back
unchanged:

```bash
kicad-cli sym upgrade --force <copy of symbols/solar_cooker.kicad_sym>
kicad-cli fp upgrade --force <copy of footprints/solar_cooker.pretty>
```

Run on a copy and compare with the original: no difference means the files are
current. Each 3D model a footprint names exists in `3dmodels/`.

## Types

Choose by the change's *nature*, not by copying past messages.

| Type    | When to use                                                   |
| ------- | ------------------------------------------------------------- |
| `feat`  | A new part (symbol, footprint, model) and its register row    |
| `fix`   | Corrects a part: a pin, a pad, a dimension, a wrong model     |
| `build` | KiCad file format upgrades of the libraries                   |
| `docs`  | README and the register, without a part change                |
| `chore` | `.gitignore`, agent files, other maintenance                  |

## Scope

Optional. The part name, for example `feat(MAX31865ATP+): add symbol, footprint and
3D model`. Leave the scope out when a commit spans several parts.

## Rules

- One part per commit, with its register row in the same commit.
- A part is only added after checking it against its datasheet, and its register row
  says when.
- A fix to a part reaches the boards only when they update their submodule, so its
  body says which boards use the part.
- Breaking changes: renaming or removing a part, or changing its pins or pads, breaks
  the boards that use it once they update. Mark it with `!` and a `BREAKING CHANGE:`
  footer naming the part and the boards.

## Well-formed examples

```
feat(MEM2061-01-188-00-A): add footprint and 3D model
fix(CR1220): correct the pad size to the datasheet
docs: add the register
chore: add commit conventions and ignore tmp/
```
