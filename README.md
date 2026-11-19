# Engineering reference tables, as data

Ten CSV datasets covering the tables engineers actually look up: drill sizes, sheet
metal gauge, AWG wire, pipe schedules, and exact unit-conversion factors.

The reason this exists: **two correct sources will print the same dimension as two
different numbers.** 3/32 inch is 2.381 mm if you convert the exact fraction, and
2.383 mm if you convert a 4-decimal display value that had already been rounded.
Both tables are right. Neither tells you which rounding chain it used. Every dataset
here names its convention in the column, and `manifest.json` records it per file.

## Datasets

| File | Rows | What it is |
|---|---:|---|
| `data/unit-conversion-factors.csv` | 98 | Exact multiplicative factors to the SI base unit of each category. factor_fraction is the exact rational; factor_decimal is its decimal expansion. |
| `data/non-multiplicative-conversions.csv` | 8 | Conversions that are not a single multiplication. Applying a factor to these gives a wrong answer, which is the most common unit-conversion error. |
| `data/sheet-metal-gauge.csv` | 114 | Gauge numbers are not a universal thickness. Each material has its own scale; the same gauge is a different thickness in each row. |
| `data/drill-bits-fractional.csv` | 64 | Fractional sizes are exact n/64 in. mm_exact_basis is computed from the exact fraction; mm_from_rounded_inch is computed from the 4-decimal display value and is the source of the two different mm values found on other sites. |
| `data/drill-bits-number.csv` | 80 | ANSI/ASME B94.11M number sizes, #1 (largest) to #80 (smallest). |
| `data/drill-bits-letter.csv` | 26 | ANSI/ASME B94.11M letter sizes, A (smallest) to Z (largest). |
| `data/drill-bits-metric.csv` | 121 | ISO 235 nominal metric sizes: 1.0 to 10.0 mm in 0.1 mm steps, then 10.5 to 25.0 mm in 0.5 mm steps. |
| `data/awg-wire-gauge.csv` | 44 | Derived from the defining formula d = 0.005 x 92^((36-n)/39) in, not copied from a printed table. area_mm2 and circular_mils are second-order quantities. |
| `data/pipe-schedule-40-80.csv` | 23 | ASME B36.10M. Outside diameter and wall thickness are the defining values; bore and weight are derived. NPS 22 is omitted because no source publishes its Schedule 40/80 wall thickness. |
| `data/pipe-wall-classes.csv` | 7 | STD and XS are wall-thickness classes, not schedules. From NPS 12 STD stops thickening while Schedule 40 keeps going, so the two diverge. |

`manifest.json` carries row counts, column lists, SHA-256 per file, and the source
page for each dataset.

## Conventions

- `*_mm_exact_basis` is computed from the defining value: the exact fraction, the
  defining formula, or the standard table entry.
- `*_mm_from_rounded_inch` is computed from the rounded inch value. It is included
  only where the difference is real, so you can see where the other number on the
  web comes from.
- `factor_fraction` is authoritative. `factor_decimal` is its expansion, at 15
  significant digits when it does not terminate.
- Inch values are given to the precision the source defines: 4 decimals for gauge
  and drill, 3 for pipe wall thickness.
- All CSVs are UTF-8 without BOM, LF line endings, comma separated.

## Provenance

| Dataset group | Defining source |
|---|---|
| Unit conversion factors | NIST SP 811; BIPM SI Brochure (9th ed.); 1959 International Yard and Pound Agreement; IAU 2012 for au and ly; statutory US definitions |
| Sheet metal gauge | MSG nominal weights, 41.82 lb/ft2 per inch (CSSBI Facts 10-12); galvanized adds 2.5 oz/ft2 of zinc; stainless uses the 1893 n/64 ladder; aluminium, brass and copper use Brown and Sharpe (= AWG geometry); copper-ounce uses 555 lb/ft3 |
| Drill bits | ANSI/ASME B94.11M for number and letter sizes; exact n/64 for fractional; ISO 235 for metric |
| AWG | the defining formula d = 0.005 x 92^((36-n)/39) inch. The ASTM B258 nominal table is this formula rounded to 4 decimals |
| Pipe | ASME B36.10M. Outside diameter and wall thickness are defining values; bore and weight are derived at 7850 kg/m3 |

Derived values are checked against published tables from at least three independent
sources. Where sources disagree, the disagreement is resolved and documented rather
than averaged away. Derivation notes: https://enginstack.com/methodology

**On pipe weight.** lb/ft here uses 10.69 x t(D-t), from 7850 kg/m3. Some published
tables use 10.68 (7841 kg/m3) and differ in the last digit by about 0.1 percent. That
is a density convention, not an error.

## Known omissions

- NPS 22 is absent. No consulted source publishes its Schedule 40 or 80 wall
  thickness; it is not interpolated.
- Temperature and fuel economy live in `non-multiplicative-conversions.csv`, because
  they are affine and inverse relationships. Applying a single factor to them gives a
  wrong answer, which is the most common unit-conversion mistake there is.

## Cite

    EnginStack. Engineering reference tables (GitHub).
    https://github.com/rkingers776-boop/engineering-reference-tables
    accessed YYYY-MM-DD.

Each dataset maps back to a page with the full derivation and the source list, see
`source_page` in `manifest.json`.

## License

Data: CC BY 4.0. Reuse it, including commercially, with attribution.

Not affiliated with ASME, ASTM, ISO, NIST, BIPM or any standards body. This is a
derived reference for checking work, not a substitute for the standards themselves.
