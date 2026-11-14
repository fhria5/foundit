# FoundIt — Requirements

Spec-driven requirements. No app code exists yet; this file is the contract the build must satisfy.

## REQ-1 — Report lost / found

- User can report a lost item or a found item with: photo (optional, ≤ 2 MB), description (≤ 2.000 chars), category (electronics, wallet, keys, bag, documents, clothing, pet, other), location (pin or text), date + time.
- Report is stored locally first, then synced when online.

## REQ-2 — Matching engine

- Every lost–found pair gets a score 0–100: category 40, text (fuzzy) 30, distance 20, time 10.
- Only pairs ≥ 50 are shown as matches, ranked highest first, with the reason for the score visible.
- Matching runs on the Worker; the client never trusts its own score.

## REQ-3 — Claim workflow (state machine)

- States: reported → matched → claimed → verifying → returned / rejected → archived.
- Every transition records actor + timestamp in the audit log.
- A claim requires answering 3 proof-of-ownership questions set by the reporter.

## REQ-4 — Proof verification

- The finder approves or rejects a claim with one tap; rejection requires a reason.
- Approved claims move to handover; handover is recorded and the item is archived.

## REQ-5 — Moderation

- Any user can report a false post; reports lower the poster's trust score.
- Posts below the trust threshold are held automatically pending review.
- Moderation actions are audit-logged.

## REQ-6 — Multi-device sync

- Workers KV sync, debounce 1,5 s, last-write-wins, cap 64 KB / 200 items (same pattern as FreshTrack).
- Photos stay on-device and are stripped from sync payloads.
- Offline: reading the list works; writes queue and sync on reconnect.

## REQ-7 — PWA + accessibility

- Installable PWA; contrast AA; tap targets ≥ 44 px; works at 320 px width.

## REQ-8 — Honesty

- Scores are shown as computed, never as certainty ("likely match", not "this is your item").
- No chat, no payments, no real-time GPS tracking, no official ID verification.
