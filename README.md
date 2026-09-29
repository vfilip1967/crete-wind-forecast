# Crete wind and rain forecast

Tomorrow's wind forecast for **Milatos beach** and rain alerts for **Fourni,
Lasithi (Crete, Greece)**, delivered to Telegram every morning.

A small bash script that fetches tomorrow's hourly wind speed from the free
[Open-Meteo API](https://open-meteo.com/) and sends a color-flagged report
to a Telegram chat via the Bot API:

- 🟢 ≤ 10 km/h — calm (beach weather)
- 🟡 11–24 km/h — moderate breeze
- 🔴 > 24 km/h — windy

It also fetches tomorrow's hourly precipitation for Fourni. A separate rain
alert is sent when at least one hour has 0.1 mm or more forecast or at least
a 50% probability of precipitation:

- ☁️ 50–69% probability with less than 0.1 mm forecast
- 🌧️ ≥70% probability with less than 0.1 mm forecast
- 🌦️ 0.1–2.5 mm with less than 70% probability — possible light rain
- ☔ 0.1–2.5 mm with ≥70% probability, or >2.5–7.5 mm — likely/moderate rain
- ⛈️ >7.5 mm — heavy rain

Hours below both thresholds are left out. Each alerted hour shows the forecast
precipitation amount and probability.

Example message:

```
🌬️ Πρόγνωση ανέμου παραλίας Μιλάτου — Αύριο (2026-07-21)
🟢 ≤10 km/h | 🟡 11–24 km/h | 🔴 >24 km/h

🟢 00:00 — 9.2 km/h
🟡 01:00 — 10.6 km/h
...
🔴 13:00 — 25.6 km/h
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
- **Rain location**: edit `RAIN_LAT`/`RAIN_LON` (default: Fourni, 35.258297, 25.662362).
- **Rain thresholds**: edit `RAIN_MIN`, `RAIN_PROBABILITY_MIN`, `RAIN_LIKELY_MIN`,
  `RAIN_LIGHT_MAX`, and `RAIN_MODERATE_MAX`.
- **Config path**: set `WIND_CONFIG` to override the default `/etc/default/check-milatos-wind`.

## License

MIT
