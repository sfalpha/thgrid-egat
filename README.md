# THGrid – EGAT transmission network data

Crowd-sourced mapping data of the **EGAT** (Electricity Generating Authority of
Thailand) high-voltage transmission grid, in the network format of the
[THGrid](https://github.com/sfalpha/thgrid) project. THGrid provides the data
models, a local backend and a browser based map / single-line editor; this
repository holds the EGAT network file that those tools read and write.

Licensed under the [MIT License](LICENSE).

## What is here

```
data/networks/egat.json    the EGAT network, one THGrid network document
```

The file is a `thgrid.network` document (format version 1). It places sites
across the whole country; lines and circuits are mapped so far around Bangkok
and in the eastern region. It contains:

| Collection | Count | Meaning |
|---|---|---|
| `substationGroups` | 332 | sites drawn on the map (substations and power plants, with coordinates) |
| `substations` | 402 | voltage yards of a site (69 kV to 500 kV, and the 300 kV HVDC yards) |
| `takeOffs` | 396 | points where a line leaves a yard |
| `junctions` | 38 | tee / splice points on the map, and site junctions (splices at a site without entering a yard) |
| `paths` | 120 | physical routes with waypoints, shared by parallel circuits |
| `circuits` | 303 | one circuit on a path: rated and energized voltage, bundle, conductor, length, tower position |
| `lines` | 198 | named lines, each a chain of circuit sections between two yards |
| `connections` | 526 | electrical nodes joining take-offs and circuit ends |
| `buses`, `bays`, `equipments` | 2, 7, 25 | in-yard single-line detail, started for a few yards |

Ids are stable and human readable (`SSY_CTI_115KV`,
`TCC_230KV_CNO_CT_L1_CNO-CNO_JT1`, `TLN_230KV_BKI_CNO_L1`), so diffs stay
reviewable. The full record definitions are in the THGrid
[model specification](https://github.com/sfalpha/thgrid/blob/master/docs/models.md),
section 6.

## Using the data

### View and edit in THGrid

Requires Node.js 20 or newer and a checkout of
[thgrid](https://github.com/sfalpha/thgrid) next to this repository.

```bash
git clone https://github.com/sfalpha/thgrid.git
git clone https://github.com/sfalpha/thgrid-egat.git
cd thgrid
npm install
npm run seed                                   # EGAT standard templates (conductors, circuits, transformers)
cp ../thgrid-egat/data/networks/egat.json data/networks/
npm run dev                                    # UI on http://localhost:5173
```

Open **Networks → EGAT**. The **Map** tab shows sites, junctions and circuits
coloured by energized voltage (500 kV purple, 230 kV red, 115 kV green); the
**Substations** tab edits the single-line diagram of a yard.

Instead of copying, the UI can also import the file directly (*Networks →
Import JSON*), and the `THGRID_DATA_DIR` environment variable can point the
backend at another data root.

### Use the JSON directly

The document is plain, pretty-printed JSON. Every record carries an `id`;
records reference each other by id only, so it can be loaded without the
THGrid code. Coordinates are WGS84 `{ latitude, longitude }`. Voltages use
THGrid's `TransmissionVoltage` names (`AC500KV`, `AC230KV`, `AC115KV`, …).

## Accuracy and sources

This is a community map, not an official EGAT dataset. Positions, routes,
conductor sizes and lengths are collected from public records, satellite
imagery and field observation and may be incomplete or wrong. Do not rely on
it for operational, safety or engineering decisions. Corrections are welcome,
see below.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to add or correct sites,
lines and circuits, the naming conventions, and how to submit a pull request.
Please only contribute information you are allowed to share.

## License

MIT License, see [LICENSE](LICENSE). Map tiles used in the editor come from
[OpenStreetMap](https://www.openstreetmap.org/copyright).
