# Contributing to the EGAT network data

Thank you for helping map the Thai transmission grid. This repository holds
one file, `data/networks/egat.json`, in the
[THGrid](https://github.com/sfalpha/thgrid) network format. All changes are
made with the THGrid editor and submitted as pull requests on that file.

## Before you start

* Only contribute information you are allowed to share: public records, your
  own observations and measurements, satellite imagery and sources whose
  licence permits reuse. Do not copy restricted or confidential material.
* Work from the latest `main`; the file is edited by several people and a
  stale copy produces large, conflicting diffs.
* One topic per pull request (one area, one line corridor, one correction) so
  it can be reviewed and merged quickly.

## Workflow

1. Fork and clone this repository, and clone
   [thgrid](https://github.com/sfalpha/thgrid) next to it.
2. In `thgrid`: `npm install && npm run seed && npm run dev`, then open
   http://localhost:5173.
3. Copy `data/networks/egat.json` from your clone into `thgrid/data/networks/`
   (or use *Networks → Import JSON* in the UI).
4. Open the **EGAT** network and edit on the **Map** tab (sites, junctions,
   paths, circuits, lines) or the **Substations** tab (buses, bays,
   equipment, take-offs of one yard).
5. Fix every issue reported in the *Issues* panel, then **Save version**.
6. Export the network (*Export JSON*) or copy
   `thgrid/data/networks/egat.json` back into your clone of this repository.
7. Commit, push and open a pull request. Describe what you added or changed
   and where the information comes from.

Do not commit files from `thgrid/data/networks/history/` or `journal/`; only
`data/networks/egat.json` belongs here.

## Data conventions

These follow the THGrid
[model specification](https://github.com/sfalpha/thgrid/blob/master/docs/models.md)
and the naming already used in the file.

### Ids and names

* Ids are stable, human readable and unique in the document. Leave the id
  empty when creating a record and the editor derives it from the model
  naming rules; rename with the *id…* button so every reference is updated.
* Sites use the EGAT substation code as `groupName` (`CTI`, `KLA`, `RY2`) and
  the full name in `name` (`Chanthaburi`, `Klaeng`). Power plants set
  `generation: true` with their `generationTypes`
  (`HYDRO`, `SOLAR`, `WIND`, `BESS`, `GAS_TURBINE`,
  `COMBINED_CYCLE_GAS_TURBINE`, `THERMAL`, `GEOTHERMAL`).
* Yards: `SSY_<site>_<voltage>` (`SSY_CTI_115KV`), one per voltage level of
  a site.
* Take-offs are named after the line they feed, as labelled in the yard:
  `230KV_CTI_L1`.
* Lines: `TLN_<voltage>_<from>_<to>_L<n>` (`TLN_230KV_BKI_CNO_L1`); paths:
  `TPA_<from>_<to>` (`TPA_RY4_RY3_JT1`); junctions `TJT_<name>`; a site's
  own junction takes the site code (`TJT_NCO`).
* Circuits are named after their line and path: `TCC_<line name>_<path name>`
  (`TCC_230KV_CNO_CT_L1_CNO-CNO_JT1`); a circuit that belongs to no line is
  `TCC_<voltage>_<path name>_S<n>` (`TCC_115KV_RY1-KLA_JT1_S1`), `n`
  numbering the unlined circuits of that path. The editor's *Regenerate
  from model rules* box reapplies these rules.

### Voltages

Use the `TransmissionVoltage` values: `AC500KV`, `AC230KV`, `AC115KV`,
`AC69KV`, `AC22KV`, …. A circuit has a rated `voltage` (design) and an
`energizedVoltage` (operated at). The editor sets the energized voltage from
the yard or junction node it is attached to; change it only when a line is
really operated below its design voltage (for example a 230 kV line run at
115 kV). A circuit that is built but not in service (unused, or a planned
line that is partly constructed) is energized at `0V` (De-Energized) and
drawn black.

### Paths, circuits, lines and connections

* Every circuit runs on exactly one **transmission path**. Parallel circuits
  on the same towers share one path; draw the route once and add the
  circuits to it.
* A circuit leaves a yard through a **take-off** and ends at the yard or
  junction where its path ends. Use a **junction** for tees and splices.
* Group circuit sections from yard to yard into a **line** (the circuit
  panel offers this when a chain is complete).
* Waypoints follow the real tower route; add enough points that the line
  does not cut through terrain or built-up areas it avoids, but no more.
  Keep coordinates to six decimals.

### Circuits and conductors

* Create circuits from the **EGAT standard** templates (`npm run seed`
  installs them): 477, 765 and 1272 MCM ACSR conductors in single, double
  and quad bundles at 115, 230 and 500 kV. The template values are copied
  into the circuit so the file stays self-describing.
* Fill in `lengthKm` when known from a source; otherwise leave it for the
  editor's route length.
* If a standard is missing, propose the new template in the
  [thgrid](https://github.com/sfalpha/thgrid) repository rather than
  inventing values here.

## Reviewing a pull request

Maintainers check that:

* the file still loads in THGrid without validation issues
  (`validateNetworkDocument` runs on import),
* ids and names follow the conventions above,
* the change is plausible against public sources or imagery,
* the diff touches only the records described in the pull request.

## Reporting problems

If you spot an error but cannot fix it yourself, open an issue with the
record id (or site name) and what you believe is correct, with a source
where possible.

## Licence

By contributing you agree that your contribution is released under the
project's [MIT License](LICENSE).
