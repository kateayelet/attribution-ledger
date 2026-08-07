# attribution-ledger

Off-site tamper-evidence anchor for the aftrveil / Boaz attribution ledger.

This repository holds published **Merkle roots** of the ledger — a Merkle root
is a single hash that fingerprints the entire ledger, so if any past entry were
altered, the root would no longer match. Publishing the root here (in a separate,
append-only public repo) means the ledger cannot be silently rewritten after the
fact: anyone can compare a claimed ledger against the root committed here.

## Layout

- `anchors/latest.json` — the current root (overwritten each anchor).
- `anchors/<root>.json` — an immutable per-root snapshot, keyed by the root hash
  itself, so re-anchoring the same root is idempotent and distinct roots never
  collide.

Each anchor records: `merkle_root`, `entry_count`, `source` (the Factor ledger
is the source of truth), and `anchored_at`.

## How it's written

The main aftrveil repo computes the root in Factor (`aftrveil.ledger`), exposes
it through the bridge (`ledger.get_stats`), and publishes it here via
`core/ledger_mirror.py` (`anchor_factor_root_now`). The anchor is dormant until
`AFTRVEIL_GITHUB_MIRROR_TOKEN` is configured.

B'ezrat Hashem
