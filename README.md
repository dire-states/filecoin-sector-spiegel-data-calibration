# sector-spiegel data: calibnet

Daily snapshot of every Filecoin **calibration network** miner that still has live sectors, for the
[sector-spiegel](https://github.com/dire-states/filecoin-sector-spiegel) site, served at
`https://dire-states.github.io/filecoin-sector-spiegel-data-calibration/`.

GitHub holds only the latest snapshot, as a single commit that each publish replaces. The full
history (one commit per snapshot) is kept locally on the machine that runs the fetch.

## Files

- `dataset.json`: marks this repo as the `calibrationnet` dataset. The fetch script refuses to
  write data from another network into it.
- `miners.json`: the overview index, loaded first by the site.
  - Snapshot height, genesis timestamp and fetch time.
  - Per miner: live, active, faulty and recovering counts, end date and max-lifetime end date, plus a
    30-day lifetime strip.
  - Network-wide daily expiration histograms, current and if every sector were extended to its
    maximum lifetime.
- `history.json`: changes between consecutive snapshots, appended by each fetch before it
  overwrites the previous snapshot. Per miner: sectors extended (and days added), expired,
  terminated early and added. The site reads it for the extensions list.
- `miners/<id>.json`: one file per miner with its live sectors, loaded when the miner is selected.
  - Sector numbers, faulty and recovering sectors as run-length bitfields.
  - Activation, expiration and early-expiration epochs, delta-coded.

The exact shapes are `MinerIndex` and `MinerData` in `app/src/lib/types.ts`. Per-sector details such
as CIDs and pledge are not stored here; the site fetches them live from Glif.

## Updating

From the app repo:

```sh
scripts/publish.sh calibnet
```
