# Family Command Center — v0.2.0

A self-hosted TV family dashboard and phone-based parent administration app. No touchscreen required.

## Included in v0.1
- Read-only TV dashboard at `/display.html` (also `/`)
- Parent admin at `/admin` with password authentication
- Custom recurring chores, completion approvals and gaming-minute ledger
- Homework Mon–Thu streak: automatically awards 60 minutes when all four are complete
- Custom weekly streak challenges
- Gaming bank, start/pause/resume/extend/stop timer, return whole unused minutes on early stop
- TV audio alarm at timer expiration (15 seconds by default, configurable)
- Meal planner for breakfast, lunch, dinner and snacks
- JSON database persisted in `./data` across Docker restarts

## Not yet included
- **iCloud Family calendar sync** (requires secure CalDAV credential integration; placeholder only)
- Multiple separate parent accounts (v0.1 uses one shared admin password)
- Separate child profiles, edit/delete tasks, custom task day selection, full grocery list
- Mobile push notifications, remote access setup, automatic console shutdown

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
