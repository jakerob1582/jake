# Clocked In — shared group setup (≈5 minutes, free)

`clocked-in.html` works out of the box as a personal hours tracker. To get the
**shared group** — everyone with the link on one leaderboard, live, from any
browser, no accounts — it needs one thing: a free Firebase Realtime Database
URL. One person sets it up once; everyone else just taps the invite link.

## 1. Create the database (one person, once)

1. Go to https://console.firebase.google.com and sign in with any Google account.
2. **Create a project** — name it anything (e.g. `clocked-in`). Google Analytics
   can be turned off; it's not needed.
3. In the left sidebar: **Build → Realtime Database → Create Database**.
   Pick the location closest to you and choose **Start in test mode**.
4. Copy the database URL shown at the top of the Data tab. It looks like
   `https://clocked-in-xxxxx-default-rtdb.firebaseio.com` (or ends in
   `firebasedatabase.app`).
5. Test mode expires after 30 days, so in the **Rules** tab replace the rules
   with:

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

   and click **Publish**. (This means anyone who has the database URL can read
   and write the groups in it — treat invite links like group-chat invite
   links. For a private app among friends, that's the whole security model.)

## 2. Connect the app (no file editing needed)

Open the app → **Profile → Group → Set up** → paste the database URL. Done.
The app remembers it, and every invite link it generates carries the database
along automatically, so friends never see this step.

(Alternative for clean links: open `clocked-in.html` and set
`window.CLOCKEDIN_DB = "https://…"` in the marked config block near the top.)

## 3. Put it on the web

Any static host works. The zero-effort route with this repo:

1. On GitHub: **Settings → Pages → Source: Deploy from a branch**, pick your
   branch and `/ (root)`, then Save.
2. The app is at `https://<username>.github.io/<repo>/clocked-in.html`.

(Netlify, Vercel, or any web server also works — it's one static file. HTTPS
is required for the camera.)

## 4. Start the group

Open the app → **Start a group** → **Profile → Invite friends → Share link** →
send it to the chat. Everyone who opens it joins with just their name. The
daily photo drops for all of you at the same random minute, and the
leaderboard crowns a weekly winner every Monday.

## Notes

- Photos expire after 7 days (the app cleans them up automatically), so the
  database stays tiny — the free tier is far more than enough.
- If the database is unreachable, clock-ins queue on the device — surviving
  even a page reload — and sync automatically when it's back.
- Multiple friend groups can share one database; every group code is its own
  world.
