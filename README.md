# Year Calendar

A year-at-a-glance calendar in the classic wall-calendar layout. Each month shows weekdays as rows and weeks as columns, and Sundays and public holidays are in red. You can add your own events. Events and public holidays are kept in two separate JSON files that the calendar connects to, the same way the Change board does.

## Files

| File | What it is | Changed by the calendar? |
|---|---|---|
| `index.html` | The calendar itself. Open it in Edge or Chrome. | – |
| `calendar.json` | Your events | Yes, every change is saved to it |
| `czech-holidays.json` | Czech public holidays (13 entries) | No, it is only read |

Keep all three in one folder, for example `C:\PRJ\Calendar\`.

## First-time setup

1. Open `index.html` in **Edge or Chrome**.
2. Click **Save**. A Save-as window opens with `calendar.json` suggested. Keep the name or type your own, choose the folder, and save. Allow editing when the browser asks.
3. Click **Data files…**. Under **Public holidays file**, click **Connect holidays file…** and choose `czech-holidays.json`.

The calendar remembers the file you last saved to and loads it the next time you open the calendar. It also remembers the holidays file. If the browser needs permission again, the button turns amber and reads **Reconnect files**. One click reconnects both files.

To use an events file you already have instead, click **Data files…** → **Open an events file…**.

Without this setup the calendar still works. Events are kept in the browser and the built-in Czech holidays are used.

## Everyday use

- **Add an event:** click a day, fill in the title (and optionally a note and type), tick **Every year** for a yearly event, then click **Add to this day**.
- **Remove an event:** click **Remove** next to it, then **Confirm**. Holidays can't be removed here; edit `czech-holidays.json` instead.
- **Save** (Ctrl+S): every change is already written to your events file automatically; **Save** writes immediately. The first time, it asks for a name and location (`calendar.json` is suggested).
- **Save as…** (Ctrl+Shift+S): save your events under another name or in another folder. From then on, that file is the one that's remembered, saved to and loaded on the next visit. The previous file stays as it was.
- **Year and language:** use the arrows next to the year and **Today** to move around. The language menu switches month and weekday names between English, Czech and Russian.
- **Coming up** lists the next eight events and holidays. Click one to jump to it.

## Importing and exporting events

- **Import events…** reads a JSON file. With the checkbox ticked, the file's events are added and duplicates are skipped. Untick the checkbox to replace your events.
- **Drag and drop** one or more `.json` files onto the page. Their events are always added, never replaced.
- **Export backup** downloads a dated copy of your events, `calendar-YYYY-MM-DD.json`, to your Downloads folder. Unlike **Save** and **Save as…**, it doesn't change which file the calendar uses.
- **View / copy JSON** shows your events as text you can copy. Use this inside the Claude viewer, where downloads don't work.

None of these touch the holidays.

## Holidays options (Data files… → Public holidays file)

- **Connect holidays file…** links a holidays JSON on disk; it is read on every visit.
- **Reload from file** re-reads it after you edit it.
- **Disconnect** stops reading the file but keeps the last loaded holidays.
- **Load holidays JSON…** loads a holidays file once and keeps a copy in the browser. This works in every browser.
- **Use built-in Czech holidays** returns to the default.

To use another country's holidays, make a file in the same format and connect it instead.

## JSON format

Both files use the same format:

```json
{
  "name": "My events",
  "events": [
    { "id": "ev-1", "date": "2026-12-03", "title": "Design review", "type": "work", "note": "Room 2" },
    { "id": "ev-2", "date": "03-14", "title": "Anna's birthday", "type": "birthday" },
    { "id": "cz-good-friday", "easter": -2, "title": "Velký pátek", "note": "Good Friday", "type": "holiday" }
  ]
}
```

| Field | Required | Meaning |
|---|---|---|
| `title` | yes | Event name |
| `date` | one of `date` / `easter` | `"YYYY-MM-DD"` for a single day, or `"MM-DD"` to repeat every year |
| `easter` | one of `date` / `easter` | Days from Easter Sunday, e.g. `-2` is Good Friday and `1` is Easter Monday. Easter is calculated for every year. |
| `type` | no | `holiday`, `personal`, `work`, `birthday` or `other`. Holidays are shown in red, other types as coloured dots. |
| `note` | no | Extra text shown under the title |
| `id` | no | Unique id. It is created automatically if missing. |

Entries that are missing a title or a valid date are skipped, and the calendar tells you how many were skipped.

## Where things are stored

| Opened as | Events | Holidays |
|---|---|---|
| `index.html` from disk in Edge/Chrome | Linked `calendar.json`, plus a copy in the browser | Linked `czech-holidays.json`, or the built-in copy |
| From a web server (`https://` or `localhost`) | Same as above | `czech-holidays.json` next to `index.html` on the server is used by default |
| Firefox | In the browser only; **Save** downloads a backup | Load holidays JSON, or the built-in copy |
| Inside the Claude viewer | Claude's own storage | Load holidays JSON, or the built-in copy |

Browser storage belongs to the address used. Opening the calendar from disk and from a server gives two separate sets of events. A linked `calendar.json` keeps one shared copy.

To serve it locally, open a terminal in the folder and run `python -m http.server 8000`, then open `http://localhost:8000/`.

## Troubleshooting

- **The Data files… dialog says linking isn't available.** Open `index.html` directly in Edge or Chrome, or from an `https://` or `localhost` address. Plain `http://` intranet addresses and embedded viewers can't link files.
- **"Couldn't save to calendar.json – is it open elsewhere?"** Close the file in other programs, then press **Save**.
- **The calendar and the file disagree.** The calendar asks which version to keep before overwriting anything.
- **A holiday is missing or wrong.** Edit `czech-holidays.json`, then use **Reload from file**.
