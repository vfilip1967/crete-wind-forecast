# Project memory

## Purpose

This repository sends a daily Telegram forecast for the following day:

- Hourly wind at Milatos beach, Crete.
- A separate rain alert for Fourni, Lasithi, only when at least 0.1 mm is forecast in an hour.

## Forecast presentation

Wind strength uses traffic-light symbols because they communicate severity clearly:

- 🟢 up to and including 10 km/h
- 🟡 above 10 and up to and including 24 km/h
- 🔴 above 24 km/h

Rain uses 🌦️ for 0.1–2.5 mm, 🌧️ for above 2.5–7.5 mm, and ⛈️ for above 7.5 mm.
Keep the wind and rain alerts active together. Dry hours are omitted from the rain alert, and no
rain message is sent when the entire forecast day is dry.

## Runtime and deployment

- Source script: `check-milatos-wind`
- Live script: `/usr/local/bin/check-milatos-wind`
- Credentials: `/etc/default/check-milatos-wind` (`BOT_TOKEN` and `CHAT_ID`); never commit or print them.
- Cron: `45 8 * * * /usr/local/bin/check-milatos-wind`
- API and forecast timezone: `Europe/Athens`

Editing the repository does not update production. After a validated change, deploy explicitly:

```bash
sudo install -m 755 check-milatos-wind /usr/local/bin/check-milatos-wind
cmp -s check-milatos-wind /usr/local/bin/check-milatos-wind
```

Do not run the live script merely as a test: doing so sends real Telegram messages. Use
`bash -n check-milatos-wind` for syntax validation. A manual live run is appropriate only when
the user requests delivery or a missed forecast must be sent.

## Current state

As of 2026-09-29, the traffic-light wind symbols and conditional Fourni rain alert are deployed.
The corresponding implementation was pushed to `main` in commit `c0a6e29`.

