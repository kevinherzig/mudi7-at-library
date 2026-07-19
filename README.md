# mudi7-at-library — community AT command library for MudiModem

JSON snippets of AT commands for the Quectel RG650V-NA (and other modems), consumed by the
**MudiModem** add-on's AT console.

## Contributing
1. Add or edit an entry in `at-library/<vendor>.json`.
2. **Run `python3 tools/lib-validate.py`** — it validates the schema AND rebuilds `dist/` (the merged
   `at-library.json` + tiny `version.json`, stamped with a content-derived `revision`).
3. **Commit the updated `dist/` along with your source change** and open a PR. CI re-runs the same
   build and **fails the PR if `dist/` is stale** — so a PR always carries a matching, validated `dist/`.

## How it reaches a router
`main` always holds a validated `dist/at-library.json` + `dist/version.json` (enforced by CI + the
"dist must be fresh" check + branch protection). A MudiModem router fetches these on a **manual**
refresh; nothing is pushed to any device automatically.

## ⚠️ Trust boundary
These commands go straight to a cellular modem. **This repo's branch protection + PR review is the
only gate** before a command can appear on someone's router. Nothing auto-runs — the UI fills the
prompt, badges the risk, and a human still confirms — but treat every entry as if it will be run.

## Schema
Each entry: `id, cat, title, risk (read|set|nv), vendor, verified[], summary, source, by`, and either
`cmd` (single) **or** `steps[]` (a sequence), optional `params[]`, optional `decode` (single `cmd`
only). `set`/`nv` entries need a `warn`. `tools/lib-validate.py` is the authority; run it before a PR.
