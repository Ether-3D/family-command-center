# Family Command Center

Version 0.7.0 — display and personalization (preview; v0.6.1 is the last user-tested release).

Self-hosted family dashboard with a read-only display at `/` and parent administration at `/admin`.

## Features
- Day/week/month calendar, local events, one read-only Google or iCloud feed, family colors and optional admin/display synchronization.
- Open-Meteo current weather and five-day header forecast.
- Profiles, avatars, recurring and one-time chores, excused days, completion undo, gaming balances and timer, stars and reward store.
- Meal planning, favorites, grocery/custom lists, basic recipes and routines.
- Display visibility, custom colors, background photos, widget ordering and widths, photo/video screensaver and scheduled dark/blank display.

## Install or update
Build the supplied Dockerfile or use `ghcr.io/ether-3d/family-command-center:latest` after GitHub Actions builds the committed files. Map container port 3000 to your chosen host port (e.g. 3010). Mount persistent storage at `/data`, writable by UID/GID 1000. Set `ADMIN_PASSWORD` (12+ characters), `SESSION_SECRET` (32+ characters), and `TZ` (e.g. America/New_York). Do not commit secrets.

For TrueNAS updates, back up the dataset, upload files to GitHub preserving paths, wait for the build, pull the image, stop the app, edit/save and restart. Hard-refresh both interfaces. Keep the previous image for rollback.

## Integrations
In Admin > Integrations select Google or iCloud, enter the secret Google iCal URL or public iCloud sharing URL, select profiles, save and sync. Never publish the secret URL. Weather uses town-center coordinates and needs no API key.

## Display personalization
Admin > Display contains existing display preferences and a separate personalization form. Drag widgets or use arrows, choose widths, select custom colors, and save personalization. Upload JPEG, PNG, WebP, MP4 or WebM files (20 MB each, 50 files maximum), then choose a background or screensaver mode. Idle is measured on the display itself; moving the mouse or pressing a key dismisses the screensaver. A running gaming timer or visible alarm takes priority. Scheduled dark/blank mode uses the existing night start/end times. Blank mode does not power off the monitor. Video playback is muted and depends on browser codec support.

## Privacy and backups
Use a trusted LAN only: the display state and uploaded media are accessible without authentication. Do not expose this service directly to the internet. Calendar URLs are excluded from the display API. Download JSON backups under Settings; also back up the entire `/data` dataset including `media/` because JSON does not include media bytes. Restore by stopping the app and restoring a matching dataset snapshot. No in-app restore yet.

## Limitations
One calendar feed only; multiple feeds deferred. No physical monitor power control, no remote screensaver dismissal, no video transcoding. Actual UI screenshots and browser/device testing pending. See CHANGELOG.md and ROADMAP.md.
