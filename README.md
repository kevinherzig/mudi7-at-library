# mudi7-at-library — community AT command library for MudiModem

JSON snippets of AT commands for the Quectel RG650V-NA (and other modems), consumed by the
**MudiModem** add-on's AT console. Contribute a command by adding an entry to `at-library/<vendor>.json`.

## How it reaches a router
CI validates every PR (`tools/lib-validate.py`). On merge to `main`, CI rebuilds and commits the
merged, validated `dist/at-library.json` + `dist/version.json`. A MudiModem router fetches these on a
**manual** refresh; nothing is pushed to any device automatically.

## ⚠️ Trust boundary
These commands go straight to a cellular modem. **This repo's branch protection + PR review is the
only gate** before a command can appear on someone's router. Nothing auto-runs — the UI fills the
prompt, badges the risk, and a human still confirms — but treat every entry as if it will be run.

## Schema
Each entry: `id, cat, title, risk (read|set|nv), vendor, verified[], summary, source, by`, and either
`cmd` (single) **or** `steps[]` (a sequence), optional `params[]`, optional `decode` (single `cmd`
only). `set`/`nv` entries need a `warn`. `tools/lib-validate.py` is the authority; run it before a PR.
