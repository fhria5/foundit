# FoundIt — Product steering

## Product

FoundIt is a global lost & found board with a matching engine. One flow: report → match → claim with proof → hand over → archive.

## Audience

Anyone, anywhere, who lost or found something. UI in English.

## Non-negotiables

- No AI slop: disciplined palette, no stacked effects, honest copy. Scores are "likely matches", never certainty.
- Privacy first: no chat, no payments, no real-time GPS, no official ID checks. Photos stay on-device.
- Spec-driven: no feature without a requirement in `.kiro/specs/foundit/requirements.md`.
- Claim safety is five layers, never one: proof questions, photo proof, finder decision, 24-hour contest window, handover photo + audit log. No official ID checks by design; the residual risk is stated honestly in the Devpost write-up.
- Tests before claims: every phase ends with a green regression run + a real-phone check by the user.
- Honest write-up: the Devpost write-up must say plainly which parts Kiro built and which were finished by hand (lesson from FreshTrack).

## Tech

- Frontend: vanilla HTML/CSS/JS PWA on GitHub Pages.
- Backend: Cloudflare Worker (route on grokingstore.my.id) + Workers KV. Never workers.dev for a new Worker (known 404).
- Kiro: spec-driven development, steering, hooks (check-js-on-save), MCP server (read-only), property-based tests.
