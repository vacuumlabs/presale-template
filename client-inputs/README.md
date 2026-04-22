# client-inputs/

Raw material **from the customer**: meeting transcripts, RFPs, emails, annexes, decks they sent, forms they filled in, screenshots they shared.

## Edit rule — immutable

Nobody edits files under `client-inputs/`. Not humans, not Claude. If the client sends an updated version, commit it as a *new* date-prefixed file — the old one stays as the historical record.

This is different from `team-inputs/`, where the author can iterate on their file. See the [root README](../README.md).

## Binary companion rule

Any non-markdown file (PDF, deck, spreadsheet, image) gets a matching `.md` next to it summarising its contents — e.g. `annex-3.xlsx` → `annex-3.md`. Without this, Claude can't answer questions about the binary.

## Index of client inputs

<!-- Claude maintains this on every /ingest and /lint pass -->

_(empty)_
