# TRMNL Shortcuts Calendar

A TRMNL plugin that displays unified calendar events from all your iOS/iPadOS calendars, sent via Apple Shortcuts.

## Overview

This plugin allows you to display calendar events from multiple calendars on your TRMNL device. Events are exported from your iPhone or iPad using an Apple Shortcut and sent to TRMNL via webhook.

## Features

- Displays events from all your iOS calendars in one unified view
- Shows events organized by: Today, Tomorrow, and Upcoming
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

Create a new Shortcut on your iPhone/iPad with the following actions:

#### Shortcut Steps:

1. **Find Calendar Events**
   - Start Date: Current Date
   - End Date: Current Date + 14 days (or your preferred range)

2. **Repeat with Each** (calendar event)
   - Create a dictionary for each event with these keys:
     - `t` - Event title (Summary)
     - `s` - Start date (ISO 8601 format)
     - `e` - End date (ISO 8601 format)
     - `a` - Is All Day (boolean)
     - `c` - Calendar name
     - `l` - Location (if available)
     - `n` - Notes (if available)
   - Add to a list

3. **Filter Events by Date**
   - Create three arrays:
     - `today` - Events starting today
     - `tomorrow` - Events starting tomorrow
     - `upcoming` - Events starting after tomorrow

4. **Get Contents of URL** (POST request)
   - URL: `https://usetrmnl.com/api/custom_plugins/YOUR_PLUGIN_UUID`
   - Method: POST
   - Headers:
     - `Content-Type`: `application/json`
   - Request Body (JSON):
     ```json
     {
       "merge_variables": {
         "today": [...],
         "tomorrow": [...],
         "upcoming": [...]
       }
     }
     ```

### 4. Set Up Automations

Create automations to run the Shortcut:

- **Time-based**: Run every hour or at specific times
- **App-based**: Run when Calendar app is closed
- **Manual**: Add to Home Screen for on-demand refresh

## Data Format

Events should be sent as arrays with the following structure:

```json
{
  "merge_variables": {
    "today": [
      {
        "t": "Team Meeting",
        "s": "2025-03-25T09:00:00-04:00",
        "e": "2025-03-25T10:00:00-04:00",
        "a": false,
        "c": "Work",
        "l": "Conference Room A",
        "n": "Weekly sync"
      }
    ],
    "tomorrow": [...],
    "upcoming": [...]
  }
}
```

### Field Reference

| Key | Description | Type |
|-----|-------------|------|
| `t` | Event title/summary | String |
| `s` | Start time (ISO 8601) | String |
| `e` | End time (ISO 8601) | String |
| `a` | All-day event | Boolean |
| `c` | Calendar name | String |
| `l` | Location | String (optional) |
| `n` | Notes/description | String (optional) |

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
