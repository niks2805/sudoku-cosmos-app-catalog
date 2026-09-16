# Future app entry folders

Create one folder per owned or authorized property:

`apps/<app-slug>/`
- `app.json`: app identity, launch state, copy, store links, compatibility, and display priority.
- `card.jpg`: square 720px JPEG or WebP, usually below 150 KB.
- Optional `gallery/`: additional 720px cards for future carousel variants.

The mobile client reads `catalog.json` as the safe public index. Add only published entries to its `items` array.

## Visibility rules

| State | `enabled` | `visibility` | In app |
|---|---:|---|---|
| Draft | false | hidden | Never loaded |
| Planned | true | published | May show with a Planned chip when a live store or web destination exists |
| Live | true | published | Appears in Settings and may join the skippable discovery prompt |
| Retired | false | hidden | Falls out of the next refresh, cache safely expires |

Keep all links HTTPS, all card paths relative to the repository root, and retain the `sample: true` marker only for testing examples. The client refreshes catalog metadata at most every 24 hours, caches valid cards to disk, and hides the entire shelf when no eligible entries survive validation.
