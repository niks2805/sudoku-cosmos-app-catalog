# App and announcement entry folders

Create one folder per owned or authorized property:

```text
apps/<app-slug>/
├── app.json
├── card.jpg
└── gallery/                 # Optional; reserved for later carousel variants
```

`catalog.json` is the only runtime index currently read by Sudoku Cosmos. Each `apps/<app-slug>/app.json` is the durable source manifest that should mirror its matching top-level index entry.

## Link-optional publishing contract

Links are intentionally optional. This makes the catalog useful before an app is listed: an upcoming product can communicate its name, visual identity, and honest one-sentence promise without sending players to an empty store listing.

| Field | Required behavior |
| --- | --- |
| `enabled` and `visibility` | Use `true` and `published` only for material ready to be displayed. Use `false` or `hidden` to remove it. |
| `releaseStage` | `planned` renders an **Upcoming** chip. `live` renders a **Live** chip. |
| `links.android`, `links.ios`, `links.web` | Each can be `null`. Any non-null value must be a verified HTTPS destination. No valid destination means a card is informative, not clickable. |
| `communication.settingsSurface.enabled` | Shows the card in the Settings carousel. Defaults to true when omitted. |
| `communication.discoveryPrompt.enabled` | Requires a valid destination and must be reserved for genuine launch news. It is disabled for linkless cards and sample content. |
| `communication.gameplayBanner` | Controls an in-game banner independently of the carousel. The CTA always opens the Settings carousel inside Sudoku Cosmos; it never opens a store. |
| `assets.cardPath` | A public relative path such as `apps/<app-slug>/card.jpg`; never use `..` or a remote host. |

## In-game banner schedule

Use light, positive frequencies: do not make this a banner ad. The client checks the player’s total successful boards; if an enabled card is eligible at that milestone, it is displayed in the standard-board communication slot. It is suppressed in Cipher, daily race, 16×16 boards, and when a more useful gameplay recovery action is active.

```json
"gameplayBanner": {
  "enabled": true,
  "minSuccessfulGames": 8,
  "everySuccessfulGames": 15
}
```

`minSuccessfulGames` and `everySuccessfulGames` accept whole numbers from 1 to 1,000. The current recommended starting range is **8–20 successful boards**, with an interval of **15 or more**. The client rotates among simultaneously eligible items in priority order.

## Artwork and copy

Provide an original or authorized 1:1 card. Target **720 × 720 px**, normally under **150 KB** in an optimized JPEG or WebP. Keep the primary visual obvious at card size, avoid fine print, and use native app copy for title and description rather than placing text in the image.

Keep descriptions short, factual, and present-tense. Do not promise availability, pre-registration, a release date, rewards, or discounts until those claims are true and a suitable destination exists.

## Safe launch change

1. Verify Android/iOS or web links in a private browser session.
2. Change only the relevant non-null `links` value(s).
3. Change `releaseStage` to `live` when available.
4. Optionally enable `discoveryPrompt` for a confirmed launch.
5. Update `catalog.json` and the matching app manifest in the same commit.
6. Verify raw JSON and every referenced image URL after publishing.

The mobile client refreshes metadata at most every 24 hours, keeps the last valid cache for offline play, and hides the entire carousel when no valid Settings cards survive validation.
