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

### Design rules

`design-rules/` holds KiCad custom DRC rules, one file per board maker and class.
KiCad only reads `<board>.kicad_dru` next to the `.kicad_pro`, so a board keeps a
copy: `just rules` in the board repo copies the file its justfile names, and
`just check` fails when the copy differs. Change the rules here, never in KiCad's
Board Setup.

## Layout

| Path | What is in it |
| --- | --- |
| `symbols/solar_cooker.kicad_sym` | All symbols, one library |
| `footprints/solar_cooker.pretty/` | All footprints, one `.kicad_mod` per part |
| `3dmodels/` | 3D models, `<part>.step` in lowercase, referenced by the footprints as `${KIPRJMOD}/kicad-common/3dmodels/<part>.step` |
| `design-rules/` | Custom DRC rules per board maker and class, `eurocircuits-proto-6c.kicad_dru` for Eurocircuits PCB proto |

## Register

Every part, where it came from and when it was checked against its datasheet.

| Part | Symbol | Footprint | 3D model | Source | Datasheet checked | Used in |
| --- | --- | --- | --- | --- | --- | --- |
| solar_cooker_logo | | ✓ | | Unknown | n/a, a logo | ds3231, bme680, max31865, microsd |
| DS3231M | ✓ | | | Own symbol from kicad-adafruit-ds3231, pins 5–12 named GND. Checked against the [Analog Devices DS3231M datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/DS3231M.pdf) (19-5312 rev 7, 16 SO) | 2026-09-27 | ds3231, testing-station |
| MIC5225-3.3YM5 | ✓ | | | [Microchip datasheet DS20006683](https://ww1.microchip.com/downloads/aemDocuments/documents/APID/ProductDocuments/DataSheets/MIC5225-Ultra-Low-Quiescent-Current-150mA-MicroCap-Low-Dropout-Regulator-DS20006683.pdf) | 2026-09-27 | max31865 |
| MEM2061-01-188-00-A | | ✓ | ✓ | SnapEDA (GCT's CAD partner), checked against [GCT drawing MEM2061](https://gct.co/connector/mem2061) of 2015-06-15 | 2026-09-27 | microsd, testing-station |
| 393570004 | | | ✓ | Molex STEP (2015), for KiCad's `TerminalBlock_4Ucon_1x04_P3.50mm_Horizontal` with offset 5.25 0.25 0, rotation -90 0 -180. Holes checked against [Molex SD-39357-001](https://tools.molex.com/pdm_docs/sd/393570004_sd.pdf) | 2026-09-27 | max31865, testing-station |
