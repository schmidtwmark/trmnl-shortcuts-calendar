# TRMNL Shortcuts Calendar

A TRMNL plugin that displays unified calendar events from all your iOS/iPadOS calendars, sent via Apple Shortcuts.

## Overview

This plugin allows you to display calendar events from multiple calendars on your TRMNL device. Events are exported from your iPhone or iPad using an Apple Shortcut and sent to TRMNL via webhook. The plugin automatically groups events into Today, Tomorrow, and Upcoming.

## Features

- Displays events from all your iOS calendars in one unified view
- Automatically groups events by: Today, Tomorrow, and Upcoming
- Supports all-day events with special labeling
- Configurable display options (times, calendar names, locations, notes)
- Multiple layout options (full, half-horizontal, half-vertical, quadrant)
- Localized date and time formatting

## Setup Instructions

### 1. Create a Private Plugin in TRMNL

1. Go to your TRMNL dashboard
2. Navigate to **Plugins** > **Private Plugins**
3. Click **Create New Plugin**
4. Give it a name (e.g., "Shortcuts Calendar")
5. Copy the **Plugin UUID** - you'll need this for the Shortcut

### 2. Upload the Plugin Files

Upload the following files to your TRMNL private plugin:

- `form-fields.yaml` - Plugin configuration fields
- `layouts/full.html` - Full-screen layout (3 columns)
- `layouts/half-horizontal.html` - Half-screen horizontal layout
- `layouts/half-vertical.html` - Half-screen vertical layout
- `layouts/quadrant.html` - Quarter-screen compact layout

### 3. Create the iOS Shortcut

Create a new Shortcut on your iPhone/iPad. The Shortcut needs to:

1. **Get today's and tomorrow's dates** as strings in `YYYY-MM-DD` format
2. **Find Calendar Events** for your desired date range (e.g., next 14 days)
3. **Build an events array** with each event containing the required fields
4. **POST to TRMNL** with the data

#### Example Shortcut Flow:

```
1. Find Calendar Events (Start: today, End: +14 days)
2. Repeat with Each event:
   - Create dictionary with keys: t, s, e, a, c, l, n
   - Add to events list
3. Create final dictionary with: events
4. Get Contents of URL (POST to TRMNL webhook)
```

### 4. Set Up Automations

Create automations to run the Shortcut:

- **Time-based**: Run every hour or at specific times
- **App-based**: Run when Calendar app is closed
- **Manual**: Add to Home Screen for on-demand refresh

## Data Format

The Shortcut should send data in this format:

```json
{
  "events": [
    {
      "t": "Team Meeting",
      "s": "2025-03-25T09:00:00-04:00",
      "e": "2025-03-25T10:00:00-04:00",
      "a": false,
      "c": "Work",
      "l": "Conference Room A",
      "n": "Weekly sync"
    },
    {
      "t": "Dentist Appointment",
      "s": "2025-03-26T14:00:00-04:00",
      "e": "2025-03-26T15:00:00-04:00",
      "a": false,
      "c": "Personal",
      "l": "123 Main St",
      "n": ""
    }
  ]
}
```

### Field Reference

| Key | Description | Type | Required |
|-----|-------------|------|----------|
| `t` | Event title/summary | String | Yes |
| `s` | Start time (ISO 8601) | String | Yes |
| `e` | End time (ISO 8601) | String | Yes |
| `a` | All-day event | Boolean | Yes |
| `c` | Calendar name | String | No |
| `l` | Location | String | No |
| `n` | Notes/description | String | No |

## How Grouping Works

The plugin computes today's and tomorrow's dates automatically, then groups events based on each event's start time:
- Events starting today → **Today** column
- Events starting tomorrow → **Tomorrow** column
- All other events → **Upcoming** column

## Configuration Options

The plugin supports the following configuration options:

- **Title Lines**: Max lines for event titles (1-3)
- **Show Times**: Display start/end times
- **Calendar Names**: Show which calendar events belong to
- **Location**: Display event locations
- **Notes**: Show event descriptions
- **Date Format**: Choose from multiple date formats
- **Time Format**: 12-hour or 24-hour time
- **Right Label**: Attribution, your name, or blank

## Layouts

| Layout | Description |
|--------|-------------|
| `full.html` | 3-column layout: Today, Tomorrow, Upcoming |
| `half-horizontal.html` | 3-column compact layout |
| `half-vertical.html` | Single-column stacked layout |
| `quadrant.html` | Compact single-column for small displays |

## Credits

- Calendar icons from [Icons8](https://icons8.com)
- Inspired by [trmnl-apple-reminders](https://github.com/sdjmchattie/trmnl-apple-reminders)

## License

MIT License
