# Clocked In — shared floor setup (≈5 minutes, free)

`clocked-in.html` works out of the box as a solo tracker. To get the **shared
floor** — everyone with the link on one leaderboard, live, from any browser,
no accounts — it needs one thing: a free Firebase Realtime Database URL.

## 1. Create the database (one person does this, once)

1. Go to https://console.firebase.google.com and sign in with any Google account.
2. **Create a project** — name it anything (e.g. `clocked-in`). You can turn off
   Google Analytics when it asks; it's not needed.
3. In the left sidebar: **Build → Realtime Database → Create Database**.
   Pick the location closest to you, and choose **Start in test mode**.
4. Copy the database URL shown at the top of the Data tab. It looks like
   `https://clocked-in-xxxxx-default-rtdb.firebaseio.com` (or ends in
   `firebasedatabase.app`).
5. One more step — test mode expires after 30 days. In the **Rules** tab,
   replace the rules with:

   ```json
   {
     "rules": {
       "floors": {
         ".read": true,
         ".write": true
       }
     }
   }
   ```

   and click **Publish**. (Yes, this means anyone who has your database URL can
   read/write the floors in it — treat the URL like a group-chat invite link.
   For a private toy among friends this is the whole security model, and it's
   the same one BeReal links, Google Doc links, and Zoom links use.)

## 2. Give the app the URL (pick either)

**Option A — edit the file (clean links):** open `clocked-in.html`, find the
`SHARED FLOOR CONFIG` block near the top, and change

```js
window.CLOCKEDIN_DB = null;
```

to

```js
window.CLOCKEDIN_DB = "https://your-project-default-rtdb.firebaseio.com";
```

**Option B — no editing at all:** open the app with the database in the URL
once: `clocked-in.html#db=https://your-project-default-rtdb.firebaseio.com` —
the app remembers it on that device, and the invite links it generates carry
the database along for your friends automatically.

## 3. Put it on the web

Any static host works. The zero-effort route with this repo:

1. On GitHub: **Settings → Pages → Source: Deploy from a branch**, pick your
   branch (`main` after merging, or the feature branch directly) and `/ (root)`,
   then Save.
2. Your app is at `https://<username>.github.io/<repo>/clocked-in.html`.

(A custom domain, Netlify, Vercel, or literally any web server also works —
it's one static file. HTTPS is required for the camera.)

## 4. Start the floor

Open the app → **Start a floor** → go to the **Desk** tab → **🔗 Invite the
floor** → paste the link in the group chat. Everyone who opens it takes a desk
with one tap, and the Margin Call fires for all of you at the same random
minute each day.

## Notes

- Photos self-shred after 7 days (the app cleans them up automatically) so the
  database stays tiny — the free tier is far more than enough.
- If the database is unreachable, the app keeps your clock-ins on-device and
  retries; nothing is lost.
- Multiple friend groups can share one database — every floor code is its own
  world.
