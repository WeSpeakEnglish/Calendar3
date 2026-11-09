# Year Calendar

A year-at-a-glance calendar in the classic wall-calendar layout. Each month shows weekdays as rows and weeks as columns, and Sundays and public holidays are in red. You can add your own events. Events and public holidays are kept in two separate JSON files that the calendar connects to, the same way the Change board does.

## Files

| File | What it is | Changed by the calendar? |
|---|---|---|
| `index.html` | The calendar itself. Open it in Edge or Chrome. | – |
| `calendar.json` | Your events | Yes, every change is saved to it |
| `holidays.json` | Public holidays: Czech Republic, Russia, United States | No, it is only read |
| `translations.json` | Interface texts: English, Czech, Russian | No, it is only read |
| `theme.json` | Colour palettes: Cosy, Classic, Forest, Lavender | No, it is only read |

Keep all five in one folder, for example `C:\PRJ\Calendar\`.

## First-time setup

1. Open `index.html` in **Edge or Chrome**.
2. Click **Save**. A Save-as window opens with `calendar.json` suggested. Keep the name or type your own, choose the folder, and save. Allow editing when the browser asks.
3. Click **Data files…**. Under **Public holidays file**, click **Connect file…** and choose `holidays.json`.
4. Under **Translations file**, click **Connect file…** and choose `translations.json`.
5. Under **Colours file**, click **Connect file…** and choose `theme.json`.

The calendar remembers the file you last saved to and loads it the next time you open the calendar. It also remembers the holidays, translations and colours files. If the browser needs permission again, the button turns amber and reads **Reconnect files**. One click reconnects all of them.

To use an events file you already have instead, click **Data files…** → **Open an events file…**.

Without this setup the calendar still works. Events are kept in the browser, the built-in Czech holidays are used, the interface is in English only, and only the Cosy colours are available.

When the calendar is served from a web server, `holidays.json`, `translations.json` and `theme.json` next to `index.html` are loaded automatically, so steps 3–5 aren't needed. When `index.html` is opened directly from disk, browsers don't let a page read neighbouring files on its own, so connect them once.

## Everyday use

- **Add an event:** click a day, fill in the title (and optionally a note and type), tick **Every year** for a yearly event, then click **Add to this day**.
- **Delete an event:** click a day, then **Delete** next to the event, then **Delete?** to confirm. The message that appears has **Undo**, which brings the event back. Holidays show a lock instead and can't be deleted here; edit `holidays.json` instead.
- **Save** (Ctrl+S): every change is already written to your events file automatically; **Save** writes immediately. The first time, it asks for a name and location (`calendar.json` is suggested).
- **Save as…** (Ctrl+Shift+S): save your events under another name or in another folder. From then on, that file is the one that's remembered, saved to and loaded on the next visit. The previous file stays as it was.
- **Year and month view:** the calendar opens in the year view. Switch with **Year | Month** next to **Today**, or click a month's name in the year view to open it. The month view shows each day as a box with its events. Click a faded day to jump to the neighbouring month.
- **Moving around:** the arrows step one year in the year view and one month in the month view. **Today** jumps back to today.
- **Language:** the language menu switches all interface texts, month and weekday names.
- **Coming up** lists the next eight events and holidays. Click one to jump to it.

## Importing and exporting events

- **Import events…** reads a JSON file. With the checkbox ticked, the file's events are added and duplicates are skipped. Untick the checkbox to replace your events.
- **Drag and drop** one or more `.json` files onto the page. Their events are always added, never replaced.
- **Export backup** downloads a dated copy of your events, `calendar-YYYY-MM-DD.json`, to your Downloads folder. Unlike **Save** and **Save as…**, it doesn't change which file the calendar uses.
- **View / copy JSON** shows your events as text you can copy. Use this inside the Claude viewer, where downloads don't work.

None of these touch the holidays.

## Holidays and translations options (Data files…)

The **Public holidays file**, **Translations file** and **Colours file** rows have the same buttons:

- **Connect file…** links a JSON file on disk; it is read on every visit.
- **Reload from file** re-reads it after you edit it.
- **Disconnect** stops reading the file but keeps the last loaded copy.
- **Load JSON…** loads a file once and keeps a copy in the browser. This works in every browser.
- **Use built-in** returns to the default: Czech holidays, English-only texts, and the Cosy colours. On a server, the files next to `index.html` are used instead.

## Public holidays

Pick the country in the holidays menu next to the language and colours menus: **Czech Republic**, **Russia**, **United States**, or **No holidays**. Your choice is remembered in the browser.

All sets live in `holidays.json` under `"sets"`. To add a country, add a block with a short key, a `name` and its `events`:

```json
"sets": {
  "de": {
    "name": { "en": "Germany", "cs": "Německo", "ru": "Германия" },
    "events": [
      { "id": "de-1003", "date": "10-03", "title": "Tag der Deutschen Einheit", "note": "German Unity Day", "type": "holiday" }
    ]
  }
}
```

- `name` can be one text or one text per language code.
- `"default"` at the top picks the set new users see first.
- Besides `"MM-DD"`, `"YYYY-MM-DD"` and `"easter"`, holidays can use **"nth weekday of a month"**: `"month": 11, "weekday": "Thu", "nth": 4` is Thanksgiving. `"nth": -1` is the last one, e.g. Memorial Day is `"month": 5, "weekday": "Mon", "nth": -1`. Weekdays are `Mon` … `Sun`.
- `"observed": true` (for `"MM-DD"` dates) also marks the day off when the date falls on a weekend: Saturday → the Friday before, Sunday → the Monday after. The US set uses it.
- Russian weekend transfers are decided by government decree each year, so they aren't in the file; add them as `"YYYY-MM-DD"` events if you need them.
- An older file with just `"events"` (like the earlier `czech-holidays.json`) still works as a single set.

## Translations

`translations.json` holds every interface text: buttons, labels, dialogs, messages, month and weekday names. Languages in the file appear in the language menu automatically.

```json
{
  "name": "Calendar translations",
  "languages": {
    "de": {
      "name": "Deutsch",
      "locale": "de-DE",
      "months": ["Januar", "Februar", "…", "Dezember"],
      "weekdays": ["Mo", "Di", "Mi", "Do", "Fr", "Sa", "So"],
      "ui": { "today": "Heute", "save": "Speichern", "…": "…" }
    }
  }
}
```

- To add a language, copy the `"en"` block, give it a new code (e.g. `"de"`) and translate the texts.
- `months` needs 12 names and `weekdays` needs 7, starting with Monday. `locale` controls how dates are written in the day panel and the **Coming up** list.
- Any `ui` text missing from a language falls back to English.
- Texts can contain placeholders such as `{name}` or `{n}`. The calendar fills them in, so keep them as they are.
- After editing, use **Reload from file** in **Data files…**, or reload the page if the calendar is served from a server.

## Colours

Pick a palette in the **Colours** menu in the header (Cosy, Classic, Forest, Lavender). Your choice is remembered in the browser.

All colours live in `theme.json`. Each palette has `light` colours and `dark` colours; the dark ones are used when the computer is in dark mode.

```json
{
  "default": "cosy",
  "palettes": {
    "mine": {
      "name": "My colours",
      "light": { "bg": "#f3ede4", "surface": "#fffaf4", "accent": "#3f6b57", "heading": "#9b2f45" },
      "dark":  { "bg": "#1c1916", "surface": "#26221e", "accent": "#8fc2a6", "heading": "#eb93a3" }
    }
  }
}
```

| Name | What it colours |
|---|---|
| `bg` | Page background |
| `surface` | Month cards, side panel, buttons |
| `surface2` | Weekends in the month view, hover, event boxes |
| `ink` | Main text |
| `muted` | Labels and secondary text |
| `line` | Borders |
| `heading` | Month names and the year |
| `sun` | Sundays and public holidays |
| `accent` | Today, selected day, main buttons |
| `accentSoft` | Soft accent (current month outline, Undo link) |
| `onAccent` | Text on accent-coloured backgrounds |
| `holiday`, `personal`, `work`, `birthday`, `other` | Event type colours |

- **To change a colour,** edit its value. You can use any CSS colour, e.g. `#3f6b57`, `rgb(63 107 87)` or `teal`.
- **To make your own palette,** copy a palette block under a new key, give it a `name` and change the values. It appears in the **Colours** menu.
- **To pick the first palette for new users,** set `default` to that palette's key.
- **Missing values** fall back to the Cosy colours. If a palette has no `dark` block, its light colours are used in dark mode too.
- **After editing,** use **Reload from file** in **Data files…**, or reload the page if the calendar is served from a server.

## JSON format

Your events file and each holiday set use the same event format:

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
| `date` | one of `date` / `easter` / `month`+`weekday`+`nth` | `"YYYY-MM-DD"` for a single day, or `"MM-DD"` to repeat every year |
| `easter` | one of `date` / `easter` / `month`+`weekday`+`nth` | Days from Easter Sunday, e.g. `-2` is Good Friday and `1` is Easter Monday. Easter is calculated for every year. |
| `type` | no | `holiday`, `personal`, `work`, `birthday` or `other`. Holidays are shown in red, other types as coloured dots. |
| `note` | no | Extra text shown under the title |
| `id` | no | Unique id. It is created automatically if missing. |

Entries that are missing a title or a valid date are skipped, and the calendar tells you how many were skipped.

## Where things are stored

| Opened as | Events | Holidays, translations, colours |
|---|---|---|
| `index.html` from disk in Edge/Chrome | Linked `calendar.json`, plus a copy in the browser | Linked files, or the built-in copies |
| From a web server (`https://` or `localhost`) | Same as above | The files next to `index.html` on the server are used by default |
| Firefox | In the browser only; **Save** downloads a backup | Load JSON, or the built-in copies |
| Inside the Claude viewer | Claude's own storage | Load JSON, or the built-in copies |

Browser storage belongs to the address used. Opening the calendar from disk and from a server gives two separate sets of events. A linked `calendar.json` keeps one shared copy.

To serve it locally, open a terminal in the folder and run `python -m http.server 8000`, then open `http://localhost:8000/`.

## Troubleshooting

- **The Data files… dialog says linking isn't available.** Open `index.html` directly in Edge or Chrome, or from an `https://` or `localhost` address. Plain `http://` intranet addresses and embedded viewers can't link files.
- **"Couldn't save to calendar.json – is it open elsewhere?"** Close the file in other programs, then press **Save**.
- **The calendar and the file disagree.** The calendar asks which version to keep before overwriting anything.
- **A holiday is missing or wrong.** Edit `holidays.json`, then use **Reload from file**.
- **Only one palette, no Colours menu.** `theme.json` isn't connected. Connect it under **Colours file** in **Data files…**.
- **Only English is in the language menu.** `translations.json` isn't connected. Open **Data files…** and connect it under **Translations file**.
