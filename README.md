# FoundIt — Global Lost & Found with Matching Engine

Lost something? Found something? FoundIt matches them.

Report a lost or found item (photo, description, place, time). A matching engine scores every lost–found pair and shows why they match. Claim with proof of ownership, verify, hand over, archived.

**Category:** Frankenstein (Kiroween 2026) — a lost & found board, a fuzzy matching engine, proof-of-ownership verification, and community moderation stitched into one app.

**Status:** Spec stage (14 Nov 2026). No app code yet. The build starts only after our first Kiroween app, FreshTrack, is declared perfect on a real phone.

## How it will work

1. Report lost or found — photo, description, category, location, time.
2. Matching engine scores pairs 0–100 (text, category, distance, time) and explains each score.
3. Claim — answer 3 proof-of-ownership questions; the finder approves or rejects.
4. Hand over — recorded and archived. False posts get reported and held by moderation.

## Tech (planned)

- PWA (vanilla HTML/CSS/JS) on GitHub Pages.
- Cloudflare Worker + Workers KV for sync and the matching API.
- Built with Kiro: spec-driven development, steering, hooks, MCP server, property-based tests.

## Links

- Spec (Indonesian): [`SPEK.md`](SPEK.md)
- Phase plan: [`rencana-per-phase.md`](rencana-per-phase.md)
- Kiro spec: `.kiro/specs/foundit/requirements.md`
