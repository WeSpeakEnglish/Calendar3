# Calendar3 - Easy-to-use Year Calendar

A year-at-a-glance calendar designed in a classic wall-calendar layout. Each month displays weekdays as rows and weeks as columns, with Sundays and public holidays highlighted in red. You can easily add, manage, and sync your personal events using lightweight JSON files.

▶ Live app: [Open](https://wespeakenglish.github.io/Calendar3/) to use the calendar right now!

## Files

| File | What it is | Changed by the calendar? |
|---|---|---|
| `index.html` | The calendar application. Open it in Edge or Chrome. | – |
| `calendar.json` | Your personal events | Yes, every change is automatically saved to it |
| `holidays.json` | Public holiday sets: Czech Republic, Russia, United States | No, it is only read |
| `translations.json` | Interface translations: English, Czech, Russian | No, it is only read |
| `theme.json` | Colour palettes: Cosy, Classic, Forest, Lavender | No, it is only read |

Keep all files together in one working directory, for example `C:\PRJ\Calendar\`.

## First-Time Setup

1. Open `index.html` in **Edge or Chrome**.
2. Click **Save**. A Save-as dialog opens with `calendar.json` suggested. Keep the default name or choose your preferred location and save. Allow file access when prompted by your browser.
3. Click **Data files…**. Under **Public holidays file**, click **Connect file…** and select `holidays.json`.
4. Under **Translations file**, click **Connect file…** and select `translations.json`.
5. Under **Colours file**, click **Connect file…** and select `theme.json`.

The calendar remembers your connected files and automatically reloads them on your next visit. If browser permissions expire, the main data button turns amber and reads **Reconnect files**—a single click reconnects everything.

To open an existing events file instead, click **Data files…** → **Open an events file…**.

Without local file linking, the calendar remains fully functional: events are stored safely in browser storage, built-in Czech holidays are used, the interface defaults to English, and the Cosy palette is applied. When served from a web server (`https://` or `localhost`), `holidays.json`, `translations.json`, and `theme.json` located next to `index.html` load automatically without manual connection.

## Everyday Use

- **Add an event:** Click any day, enter a title (plus optional note and type), tick **Every year** for recurring events, and click **Add to this day**.
- **Delete an event:** Click a day, then click **Delete** next to the event, followed by **Delete?** to confirm. The confirmation notification includes an **Undo** button to instantly restore the item. Public holidays display a lock icon and cannot be edited or deleted here; modify `holidays.json` instead.
- **Save** (Ctrl+S): Changes are written automatically; clicking **Save** forces an immediate write.
- **Save as…** (Ctrl+Shift+S): Save your events under a new name or location. From then on, that file becomes your active linked calendar file.
- **Year and Month views:** Switch instantly between views using the segment control next to **Today**, or click any month header in the year view to zoom into it. Click any faded day in the month view to jump across months.
- **Navigation:** Use the arrow buttons to step through years or months, and **Today** to jump right back to the current date.
- **Language & Styling:** Use the headers dropdowns to instantly switch interface languages, public holiday sets, and colour palettes.

## Importing and Exporting Events

- **Import events…** reads a JSON file. With the merge checkbox ticked, events are added and duplicates skipped; untick it to replace your current events entirely.
- **Drag and drop** one or more `.json` files anywhere onto the page to add their events instantly.
- **Export backup** downloads a timestamped backup (`calendar-YYYY-MM-DD.json`) to your Downloads folder without altering your active file connection.
- **View / copy JSON** displays your events in a copy-pasteable format (ideal for environments where direct downloads are restricted).

## Customizing Holidays and Translations

### Public Holidays
Choose your region from the holiday dropdown menu (**Czech Republic**, **Russia**, **United States**, or **No holidays**). Your preference is saved in the browser.

All holiday definitions reside in `holidays.json` under `"sets"`. Add a custom region by appending a block:
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
- Dates support `"MM-DD"` (repeating yearly), `"YYYY-MM-DD"` (single day), `"easter": N` (days offset from Easter Sunday), or nth weekday rules (e.g. `"month": 11, "weekday": "Thu", "nth": 4`).
- `"observed": true` automatically shifts weekend holidays to the nearest workday.

### Translations
`translations.json` contains all interface labels, buttons, dialogs, and month/weekday names.
- To add a language, copy the `"en"` dictionary block, assign a new language code key (e.g., `"de"`), and translate the strings.
- Missing keys automatically fall back to English. Placeholders like `{name}` or `{n}` are dynamically populated by the app.

### Colours
All palettes in `theme.json` support separate `light` and `dark` mode configurations. Edit existing values or define custom palettes under new keys to tailor the app's appearance.

## JSON Format Reference

Events and holiday entries follow this structure:
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
Valid event types are `holiday`, `personal`, `work`, `birthday`, and `other`.