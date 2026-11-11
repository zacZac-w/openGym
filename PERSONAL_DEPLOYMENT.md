# Personal Windows deployment

This non-commercial fork starts from DuarteSantos8/openGym. Changes added on
2026-11-12: a pinned v1.4.4 deployment configuration, these deployment notes,
and exclusion of the optional Coach credential cache from Git.
The code remains under the upstream GNU AGPL v3.0; retain LICENSE and NOTICE.md.
Exercise media have separate terms in NOTICE.md. Keep the unmodified 180 px
media in this non-commercial OpenGym fork; do not use them as AI inputs.

## Run the initial release

Open PowerShell in this repository after starting Docker Desktop:

```powershell
docker compose -f docker-compose.yml -f compose.release.yaml pull
docker compose -f docker-compose.yml -f compose.release.yaml up -d --no-build
```

Open http://localhost:8787 and create your personal profile. The initial local
configuration makes the first profile an administrator and disables guest mode.
The web port is bound to 127.0.0.1; the server is initially accessible only on
this Windows PC. These local choices are in .env, which must stay out of Git.

To stop the application without deleting its records:

```powershell
docker compose -f docker-compose.yml -f compose.release.yaml stop
```

Back up the entire data folder, preferably with the services stopped. It contains
your account, workouts, body records, uploads and session key. Do not upload it,
.env or coach-auth to GitHub. The app also has JSON and CSV export options.

## Connect the iPhone

The Windows browser and phone must use the same server profile. For private
remote access, set up Tailscale on both devices and Tailscale Serve on Windows
for http://127.0.0.1:8787. Then set RP_ID to the resulting bare HTTPS hostname
and ORIGIN to its full https:// origin in .env, and run the up command above.
Prefer setting the final hostname before creating a passkey, because passkeys
are bound to their hostname. See docs/SELF_HOSTING.md for the complete procedure.

Use the iPhone app's Connect to my server option and the pairing code from the
signed-in browser's Settings > Account > Pair the mobile app. Alternatively use
the HTTPS address in Safari and add it to the Home Screen. Standalone phone mode
and browser guest mode do not sync to this server. The PC must be awake and
Docker running for synchronization; the phone can record offline and sync later.

## Build our modified version later

The pinned deployment above runs the upstream release, not local source edits.
After developing and checking our changes, build the source explicitly:

```powershell
docker compose up -d --build
```

This uses the same data directory. Back it up before switching versions, and do
not downgrade after a future data-format change without checking compatibility.

When publishing modifications, retain notices, identify changed work and dates,
and license the combined code under AGPL v3. Offer corresponding source to users
of a modified hosted version as required by section 13. Public source code does
not require public workout records or credentials. See LICENSE for full terms.
