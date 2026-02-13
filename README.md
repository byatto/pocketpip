# littlemoments

A fast, minimal web app for capturing moments of Malcolm's life as they happen. Built for parents who want to remember the funny quotes, the firsts, and the everyday magic — without the overhead of a full journaling app.

## Philosophy

Moments happen fast. By the time you open an app, unlock it, find the right section, and start typing — the moment's lost. **littlemoments** is designed around one principle: **capture first, organise later**.

- **Capture** — text box, optional category tap, done. Under 5 seconds.
- **Review** — batch-categorise uncategorised moments when you have a quiet minute.
- **Timeline** — browse, search, filter, and revisit everything.

## Features

- **Instant capture** with quick-tap categories: Quote, First, Funny, Note
- **Date override** — log something that happened yesterday or last week
- **Freeform tags** for extra context
- **Review inbox** — categorise moments you captured in a hurry
- **Timeline view** — chronological browsing grouped by date, with search and filters
- **Pin** favourite moments
- **Cloud sync** via Google Apps Script — both parents can contribute
- **Dark mode**
- **Keyboard shortcuts** for power users
- **Export/Import** JSON backups
- **Offline-first** — works without internet, syncs when connected
- **Soft delete** with 30-day trash and undo

## Getting Started

1. Open `index.html` in any modern browser
2. Start capturing moments

That's it. No build step, no dependencies, no server required.

### Cloud Sync (Optional)

To sync between devices (e.g. both parents' phones):

1. See [`docs/sync-setup.md`](docs/sync-setup.md) for full instructions
2. Click **Sync Settings** in the app footer
3. Paste your Google Apps Script URL
4. Both devices will now stay in sync

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `/` | Focus capture input from anywhere |
| `1` `2` `3` | Switch between Capture / Review / Timeline |
| `j` / `k` | Navigate review items (down / up) |
| `q` `f` `u` `n` | Categorise focused item (Quote / First / Funny / Note) |
| `d` | Delete focused review item |
| `Ctrl+Enter` | Submit capture |
| `Esc` | Close modal / dismiss toast |

## Data

All data is stored in `localStorage` under the key `littlemoments_data`. The data structure:

```json
{
  "items": [
    {
      "id": "unique-id",
      "text": "Malcolm said 'daddy, the moon is following us'",
      "category": "quote",
      "tags": ["bedtime", "3yo"],
      "createdAt": "2026-02-13T20:30:00.000Z",
      "updatedAt": "2026-02-13T20:31:00.000Z",
      "pinned": false
    }
  ],
  "tags": ["bedtime", "3yo", "park", "grandma"]
}
```

### Categories

| Category | Use for |
|----------|---------|
| **Quote** | Things Malcolm said |
| **First** | Milestones and first-time events |
| **Funny** | Hilarious moments |
| **Note** | General observations, daily snapshots |

## Tech Stack

- Single `index.html` file — HTML, CSS, JavaScript
- No framework, no build tools, no dependencies
- Fonts: DM Serif Display, DM Sans, JetBrains Mono (Google Fonts)
- Cloud sync: Google Apps Script (optional)

## Roadmap

See [Issues](../../issues) for planned features. Key ideas for future versions:

- [ ] "On This Day" — resurface moments from the same date in previous years
- [ ] Photo attachments (v2)
- [ ] Age calculator — auto-tag Malcolm's age at time of capture
- [ ] Monthly digest — email summary of the month's moments
- [ ] PWA support for home screen install
- [ ] Multiple children support

## License

MIT — see [LICENSE](LICENSE)
