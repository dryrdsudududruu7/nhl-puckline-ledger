# PUCKLINE play ledger

A public, timestamped record of every play the PUCKLINE NHL prediction lab logs, published automatically by its cloud
refresh. Each file in `ledger/` lists one game day's plays. Git's commit history shows when each play was published, so
anyone can check it was logged before the game started.

The files are append-only: a row once published never disappears and never changes, except its `result`. A play is
published only after the private database holding its fingerprint's nonce has been saved.

Paper tracking only: no money is bet, and nothing here is advice to bet.

## Columns

| Column | Meaning |
|---|---|
| `logged_at_utc` | When the play was logged (UTC) |
| `game_day` | The game's date (US Eastern) |
| `game` | Away team at home team |
| `pick` | The play |
| `label` | "Experimental (unproven)" for the tested win model's plays; "Research only" for plays the site does not show |
| `model_chance` | The model's own chance for the play |
| `commitment` | A SHA-256 fingerprint of the play's price (see below) |
| `result` | won, lost, push or void once graded; blank until then; "duplicate (not counted)" when the same bet was logged earlier (the earliest one counts) |

## Why there are no prices

The odds providers' terms do not allow their prices to be republished. Instead, each play carries a fingerprint:
SHA-256 of `pick_key|logged_at|price (4 decimals)|sportsbook|nonce`, where the nonce is a private random value kept for
that play. Revealing one play's price, sportsbook and nonce later lets anyone recompute the fingerprint and confirm the
price was recorded when the play was logged, without publishing every price.
