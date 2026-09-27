# kicad-common

Shared KiCad library for the board repos of
[solar-cooker-UHasselt](https://github.com/solar-cooker-UHasselt): the symbols,
footprints and 3D models KiCad's own library does not have. Each part is here once,
checked against its datasheet, instead of copied between boards.

KiCad 10.

## Use it in a board repo

Mount it as a git submodule, always at `kicad-common/` in the board's root, so every
path below is the same in all boards:

```bash
git submodule add https://github.com/solar-cooker-UHasselt/kicad-common.git kicad-common
```

Clone a board with its library: `git clone --recurse-submodules <board>`, or in a
clone made without it, `git submodule update --init`.

In the board's `sym-lib-table` and `fp-lib-table`, one library each, named
`solar_cooker`:

```
(lib (name "solar_cooker")(type "KiCad")(uri "${KIPRJMOD}/kicad-common/symbols/solar_cooker.kicad_sym")(options "")(descr ""))
(lib (name "solar_cooker")(type "KiCad")(uri "${KIPRJMOD}/kicad-common/footprints/solar_cooker.pretty")(options "")(descr ""))
```

A board stays on the kicad-common commit its submodule points to. A change here
reaches a board when that board updates its submodule and commits the new pointer.

## Layout

| Path | What is in it |
| --- | --- |
| `symbols/solar_cooker.kicad_sym` | All symbols, one library |
| `footprints/solar_cooker.pretty/` | All footprints, one `.kicad_mod` per part |
| `3dmodels/` | 3D models, `<part>.step` in lowercase, referenced by the footprints as `${KIPRJMOD}/kicad-common/3dmodels/<part>.step` |

## Register

Every part, where it came from and when it was checked against its datasheet.

| Part | Symbol | Footprint | 3D model | Source | Datasheet checked | Used in |
| --- | --- | --- | --- | --- | --- | --- |
| solar_cooker_logo | | ✓ | | Unknown | n/a, a logo | ds3231, bme680, max31865, microsd |
