# URPN Service Flow — Netlify version

Two screens, one app:

| Screen | Address | Who |
|---|---|---|
| Control room | `https://YOUR-SITE.netlify.app/` | Operator (PIN protected) |
| Stage display | `https://YOUR-SITE.netlify.app/display` | Projector / stage screen (press F11) |

## Project layout

```
public/index.html              control room
public/display.html            stage display
netlify/functions/api.mjs      the "server": timer state, schedule, messages, Bible verses, logo
netlify.toml, package.json     Netlify settings
```

State (current session, schedule, message/verse, logo) is stored in **Netlify Blobs**,
which is built into Netlify, so there is no database to set up.

## Deploy (recommended: GitHub)

1. Create a new GitHub repository and upload everything in this folder.
2. In Netlify: **Add new site > Import an existing project**, pick the repo.
   Leave the build command empty; the settings in `netlify.toml` are picked up automatically.
3. Click **Deploy**.
4. **Set your PIN** (important): Site configuration > Environment variables >
   add `CONTROL_PIN` with a number/word of your choice, then **Deploys > Trigger deploy**.
   Without it, anyone who finds the control address can control the screen.

## Deploy with the Netlify CLI instead

```
npm install
npm install -g netlify-cli
netlify login
netlify deploy --prod
```
Then set `CONTROL_PIN` as in step 4 and redeploy.

## First run

- The schedule is seeded from your Google Sheet (7 sessions). After that, add and remove
  sessions inside the control room. The Sheet is no longer used.
- Upload the logo once from the control room; it becomes the stage background.
- If a Bible version shows an error, open `https://YOUR-SITE.netlify.app/api/bible-versions`
  to see the exact version codes, and fix the `BIBLE_VERSIONS` list in `api.mjs`.
- To reset the schedule to the defaults in `api.mjs`: Netlify dashboard > Blobs > store `urpn` > delete `schedule`.

## Notes

- Both screens ask the server for updates every 2 seconds and tick smoothly in between.
  Change `POLL_MS` in either page to adjust.
- Run locally with `netlify dev` (needs `npm install` first).
