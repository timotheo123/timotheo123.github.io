# Fieldmark — browser-only attendance prototype

Plain HTML, CSS, and JavaScript. No framework, package installation, build, API key, or backend.

## Files

- `index.html` — application entry point
- `styles.css` — responsive desktop/mobile layout
- `app.js` — fixed accounts, scheduling, capture, review, IndexedDB storage
- `.nojekyll` — lets GitHub Pages serve the files as-is

## Deploy

Upload the extracted files (not the ZIP) to the root of the repository/folder you publish with GitHub Pages. Keep index.html, styles.css and app.js together. Enable GitHub Pages for that branch/folder if not already configured. Open the deployed HTTPS address directly in Safari/Chrome.

No absolute asset paths are used, so repository subpaths work. Do not open the ZIP itself. Opening index.html via file:// is not recommended: camera, geolocation, and storage behavior differs by browser. For local development, serve the folder on localhost, for example `python3 -m http.server 8000`, then open `http://localhost:8000`.

## Demo accounts

| Role | Display name | Username | Password |
| --- | --- | --- | --- |
| Admin | Demo Admin | admin | admin123 |
| Standard | Alex Rivera | user1 | user123 |
| Standard | Jamie Santos | user2 | user123 |

The login page has buttons that fill the public demo credentials. Click Sign in afterward.

## Demo walkthrough

1. Sign in as admin. Weekly schedule initially contains two visits per weekday for each user in the current week.
2. Add, edit, or delete an assignment. Choose a user, date, site name/address, start/end times, and optional instructions. Multiple visits per day are supported. Overlaps for the same person are rejected. Unassigned days are off days.
3. Sign out, then sign in as user1 **in the same browser and on the same device**.
4. My day shows today's assignments only. If testing on a weekend, first add an assignment for today as admin.
5. Check in: Open camera, grant permission, Capture selfie, then allow location. Include both your face and the site surroundings. Preview the captured photo, add an optional note, and save.
6. If GPS is denied/unavailable, retry or enter an explanation. The admin will see that GPS is missing. GPS is captured near the selfie time, with its own sample timestamp and accuracy.
7. Check out with a new selfie. Completed visits become Pending. Check-out is also available for an ongoing visit from a previous day.
8. Switch to admin. Review the two photos, device timestamps, actual duration versus planned duration, and coordinates. Open Google Maps if desired. Approve, or reject with a required explanation.
9. Sign in as the user again to see the status and review note.

## Included behavior

- Admin overview, weekly schedule navigation, user filters, attendance history and decisions.
- Standard users see only their own schedules and attendance through the interface.
- Live camera capture only (no file upload represented as a fresh selfie).
- Check-in and check-out photo, exact ISO capture time, GPS coordinates, accuracy, sample time, and notes.
- GPS permission/timeout handling with a required explanation if missing.
- One active visit per user and one attendance pair per assignment. Selfies older than 5 minutes must be retaken before saving.
- Check-out can occur after midnight. Schedule assignments themselves cannot cross midnight.
- Assignments with attendance are locked. Completed decisions are final in this demo. Rejected visits remain visible; no resubmission/editing of proof.
- IndexedDB transactions recheck duplicate attendance and review status before saving, including across tabs.
- Per-tab sign-in via sessionStorage, with records shared across the same browser origin.
- CSV export respects attendance filters and excludes images. Spreadsheet-formula-leading text is escaped.
- Admin JSON backup export includes all local records and photos. This is an archival export; restore/import is not included.
- Admin reset with confirmation removes all local attendance and recreates sample schedules.
- Responsive layouts and labeled forms; mobile camera uses the front-facing camera where available.

## Important limits

This is a demonstration, not a secure attendance service. Account credentials and authorization logic are client-side; a person with developer tools can inspect or alter all local data. Device clocks and GPS can be changed/spoofed. Selfies are supporting evidence, not verified presence or facial recognition. There is no geofence enforcement or server-verified time.

Data does not sync across phones, computers, browser profiles, or origins. Admin cannot see another device's attendance. To demonstrate the approval cycle, switch accounts in the same browser. For multiple-device use later, add a backend with real authentication and server-controlled access.

All photos and GPS remain in IndexedDB; the app does not send them to a server. Google Maps is opened only when someone clicks its link, which sends the selected coordinates to Google. Downloaded backups contain photos and location data and must be kept private. Clearing site data or leaving private browsing can erase records. Quota failures are surfaced and do not intentionally discard existing records.

Date/time display uses the device's timezone. Schedules are local calendar dates and local wall-clock times. Keep the same timezone during demos. Actual durations use timestamps. Visit date means the scheduled/check-in date; a next-day checkout is labeled separately in history.

Test on your intended Android/iOS device over HTTPS. Camera/GPS may be unavailable in embedded browsers, HTTP previews, or if permission is blocked. The interface disables submission until a selfie has been captured. No placeholder image is saved as attendance.

## Verification performed

- JavaScript syntax check.
- Browser startup, admin/demo-user sign-in, schedule creation and overlap rejection, persistence after reload, and user-specific itinerary display.
- Selfie dialog and HTTP camera-unavailable error path.
- Core scheduling/visibility/time calculation assertions.

Real camera capture, GPS permission outcomes, and the full physical-device check-in/review cycle require testing on an HTTPS deployment with a camera and location hardware. The local browser preview used for this build was HTTP, so it could not verify those hardware integrations.
