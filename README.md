# Family Command Center v0.5.2 — Google Calendar support

Admin → Integrations → Calendar integration: select Google Calendar, paste the Secret address in iCal format from Google Calendar settings, enable, assign family members, save and Sync now. Keep the calendar private. Existing iCloud links remain supported. One read-only feed at a time; switching feeds replaces cached imported events, not local events. No new dependencies. Live Google sync requires testing on your installation.

# Family Command Center v0.5.1 — Header weather

Header weather is enabled by default when weather data is available. Admin → Display controls header weather independently of the Weather widget. Forecast now requests six days including today; cached older data may show four future days until refreshed.

# Family Command Center v0.5.0 — Integrations

## Setup
- Upload all update files, including Dockerfile and integrations.js. GitHub Actions installs the pinned ical.js dependency during the image build.
- Admin → Integrations → iCloud: on an Apple device open Calendar, select the calendar information button, enable Public Calendar, and copy its sharing link. Paste the link, select family members, enable and save. Default refresh: five minutes. This is read-only and anyone with the link can access the calendar. If Public Calendar is unavailable for the shared Family calendar, use a separate calendar you own; private CalDAV is not implemented.
- Admin → Integrations → Weather: enter a town label and approximate town-center latitude/longitude, enable and save. Then enable Weather in Display and save. Open-Meteo provides Fahrenheit current conditions and five-day forecast. Default refresh: 30 minutes.
- Imported events are shown alongside local events, with a read-only label and selected profile colors. Import window: previous 31 days through next 180 days. Events are displayed on their start date only; multi-day spans are not expanded. A single feed is supported. Failed refreshes retain cached data. Sync now triggers a background refresh.
- Backup files contain the private sharing URL; store them securely. Public TV state does not include the URL. The TV and its state endpoint are accessible to devices on the LAN; do not expose the service publicly.
- Automatic pre-v0.5.0 backup: state.json.pre-v050. To roll back, restore that backup with the app stopped (changes after upgrade will be lost).

## Validation limitations
Syntax and local API tests only. Network access prevented npm installation and live provider tests in the development environment; verify GitHub build and live sync before treating this as stable. No new UI screenshots were captured.

# v0.4.2 — Calendar synchronization fix

Separates the saved TV default from the synchronized admin view. Disabling synchronization restores the TV default.

# v0.4.1 — Dashboard improvements

Profile colors, family filters, TV day/week/month views, optional admin view sync, and household list widgets. TV always displays the current period. Display settings select lists and completed item visibility. Existing data retained.

# Family Command Center — v0.4.0

A self-hosted TV family dashboard and phone-based parent administration app. No touchscreen required.

## Features in v0.4.0
- Cozy Home TV display and responsive parent admin navigation.
- Family profiles, assigned recurring chores, editable rewards, gaming timer and weekly challenges.
- Local calendar with day, week and month views, all-day or timed events, family assignments, editing and deletion.
- Shared grocery and custom lists with item completion and removal.
- Weekly meal overview, favorite meals and family announcements on the TV.
- Authenticated JSON backup download; automatic pre-v0.4 migration backup.

## Not yet included
- iCloud/CalDAV sync, weather, individual gaming banks, stars, one-time chores, undo/excused chores, PWA and restore UI.
- Calendar events are local only; month view is a date-card grid, not a traditional aligned month grid.
- UI screenshots pending browser access. The display and its state API are readable on the trusted LAN.

## Install on TrueNAS / Docker
1. Upload this entire project to a private Git repository (do not upload `.env` or `data/`).
2. On the Docker host, clone the repository and `cd family-command-center`.
3. Copy `.env.example` to `.env`.
4. Replace `ADMIN_PASSWORD` with a strong unique password of at least 12 characters.
5. Replace `SESSION_SECRET` with at least 32 random characters. Example generation: `openssl rand -hex 32`.
6. Run `docker compose up -d --build`.
7. Visit `http://SERVER-IP:3000/admin` from your phone and `http://SERVER-IP:3000/` from the TV.

For TrueNAS SCALE, use a custom Docker Compose app if your version supports it. Bind-mount a persistent host dataset instead of `./data` if preferred; ensure the container's `node` user (UID 1000) can write to the folder.

## TV kiosk
A Raspberry Pi / mini PC can run Chromium in kiosk mode at `http://SERVER-IP:3000/display.html`. For automatic audio on an unattended kiosk, configure Chromium to permit autoplay on this trusted local display (e.g. `--autoplay-policy=no-user-gesture-required`), with HDMI audio output enabled. **Test the alarm on the actual TV:** ordinary browsers can block sound until the page has been interacted with.

## Security
- Keep this application on a trusted LAN or behind a VPN (e.g. Tailscale). **Do not forward port 3000 to the public internet.**
- HTTP on the LAN does not encrypt passwords in transit. Use HTTPS through a trusted local reverse proxy or a VPN for stronger security, especially on shared networks.
- Admin API operations require an HttpOnly SameSite=Strict session cookie; display data is intentionally readable by anyone who can reach the app.
- One shared admin password in this first release; use a unique password not used for other accounts.
- Back up the persistent `data/` directory regularly. Do not commit it to Git.
- The server uses the `TZ` environment variable for the local date; set it to your actual timezone.

## Notes
- Each chore can be completed only once per scheduled date; completions award minutes immediately.
- A streak bonus is issued once per week after all required completion dates are recorded.
- The timer deducts the reserved minutes at session start; early stop refunds only full unused minutes.
- A TV dashboard polls the server every five seconds; the countdown runs locally between polls. The timer remains authoritative on the server.
- TV alarm sounds only if the dashboard is open and the browser permits audio; no sound is guaranteed if the TV or display device is off.
- In this first release, a parent can adjust the bank manually with an audit reason; corrections to completed chores are not yet supported.

## v0.2.0 — Cozy Home
Warm dashboard and parent interface, calendar-first layout, chore progress, streak celebration, saved family name, three display themes, widget visibility, preset ordering and layout, scheduled visual dimming. Calendar integration remains pending. Existing state is retained; display preferences are optional settings.

## v0.3.0 family profiles update
Migrates existing tasks and gaming bank to Bentley, adds Dad and Mom as non-gaming profiles, allows additional non-gaming profiles, task editing, custom days, soft deletion, and challenge editing/disabling. Existing ledger and completion history retained. A pre-migration state.json.pre-v030 backup is created. Gaming remains a single bank for Bentley. Calendar sync, weather, lists, additional reward banks, undo and other roadmap features are not included in this build.

## v0.3.1
Responsive parent navigation, overview, separate family/chore/gaming/rewards/meals/display/settings pages. Existing single gaming bank and database schema retained. Calendar integration, lists, separate reward banks and full PWA remain pending.

## v0.6.0 preview
Rewards page includes household tools for dated chores, excused days, completion undo, individual gaming balances, emoji avatars, star rewards, recipes and routines. Only one gaming timer and one calendar feed are supported. Recipes and routines are text records. Legacy rewarded completions cannot be automatically undone; use manual adjustments. Back up before upgrading; migration creates state.json.pre-v060.
