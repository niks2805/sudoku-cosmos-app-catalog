# Sudoku Cosmos App Catalog

This public repository is the **content control plane** for the optional “From our orbit” shelf inside Sudoku Cosmos. The game fetches `catalog.json`, validates it defensively, caches the last valid response, and changes its presentation without a Sudoku Cosmos content release.

## What the catalog controls

Every published item decides its own title, copy, image, release state, display priority, optional store destination, Settings visibility, discovery-prompt eligibility, and gameplay-banner cadence. A card remains a safe, non-clickable announcement when every entry in `links` is `null`.

| Communication goal | GitHub configuration | Player experience |
| --- | --- | --- |
| Show an upcoming app | `releaseStage: "planned"`; all `links` values `null` | Read-only carousel card; **In the works** replaces the action link. |
| Launch an app or website | Add a valid HTTPS platform or web destination | Card becomes a player-initiated link only. |
| Offer a post-clear discovery prompt | `discoveryPrompt.enabled: true`, plus a valid destination | Optional, skippable prompt; never shown for linkless or sample cards. |
| Add a board message | `gameplayBanner.enabled: true`, tune milestones | Black banner with white text and golden border; CTA opens the in-app Settings shelf, never an external store. |
| Pause any item | `enabled: false` or `visibility: "hidden"` | The item disappears after the next refresh. |

> **Publication rule:** all metadata and images are public. Never store secrets, player information, draft claims, or unverified destinations in this repository.

## Structure

```text
catalog.json                         # Runtime index read by Sudoku Cosmos
apps/
  <app-slug>/
    app.json                         # Human-maintained per-app source manifest
    card.jpg                         # 720 × 720 original/authorized visual
    gallery/                         # Optional future variants, not yet consumed
templates/app.json                   # Copy-and-adapt manifest template
```

See [`apps/README.md`](apps/README.md) for the full publishing contract.
