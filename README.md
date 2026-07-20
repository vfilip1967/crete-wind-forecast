# milatos-wind-forecast

Daily wind forecast for **Milatos beach (Crete, Greece)**, delivered to Telegram every morning.

A small bash script that fetches tomorrow's hourly wind speed from the free
[Open-Meteo API](https://open-meteo.com/) and sends a color-flagged report
to a Telegram chat via the Bot API:

- 🟢 ≤ 10 km/h — calm (beach weather)
- 🟡 11–24 km/h — moderate breeze
- 🔴 > 24 km/h — windy

Example message:

```
🌬️ Πρόγνωση ανέμου παραλίας Μιλάτου — Αύριο (2026-07-21)
🟢 ≤10 km/h | 🟡 11–24 km/h | 🔴 >24 km/h

🟢 00:00 — 10.2 km/h
🟡 01:00 — 10.6 km/h
...
🔴 13:00 — 14.6 km/h
```

## Requirements

- `bash`, `curl`, `jq`, `bc`
- A Telegram bot token (create one via [@BotFather](https://t.me/BotFather))
- The chat ID to send the report to

## Setup

1. Copy the script somewhere on your PATH:

   ```bash
   sudo install -m 755 check-milatos-wind /usr/local/bin/check-milatos-wind
   ```

2. Create the config file with your credentials (or export them as env vars):

   ```bash
   sudo install -m 600 config.example /etc/default/check-milatos-wind
   sudoedit /etc/default/check-milatos-wind   # fill in BOT_TOKEN and CHAT_ID
   ```

3. Add a cron entry to run it every morning at 08:45:

   ```
   45 8 * * * /usr/local/bin/check-milatos-wind
   ```

## Customization

- **Location**: edit `LAT`/`LON` in the script (default: Milatos beach, 35.3197, 25.5673).
- **Thresholds**: edit `GREEN_MAX` / `YELLOW_MAX`.
- **Config path**: set `WIND_CONFIG` to override the default `/etc/default/check-milatos-wind`.

## License

MIT
