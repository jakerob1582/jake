# App Build Prompt: "Clocked In" — BeReal for Finance Hours

Copy everything below this line into your app builder of choice. (This prompt has since been implemented — see `clocked-in.html`.)

---

Build a mobile-first app called **Clocked In** (working titles also acceptable: *GrindReal*, *The Daily Margin Call*, *ClockedTF-In*). It's BeReal meets a time clock, built for a small group of finance friends who want to compete over who works the most unhinged hours — and prove it with photos.

## Core concept

A shared workspace where a group of friends tracks their work hours with one-tap clock in / clock out, gets hit with a random daily BeReal-style photo moment, and battles for the top of a leaderboard that crowns the longest-working person. Tone: dry finance humor throughout. The app should feel like it was designed by someone who has personally suffered through a pitch deck at 2am.

## 1. Login = Clock In. That's it.

- No passwords, no email verification, no OAuth maze. First launch asks for a display name and an emoji. Done forever.
- The home screen is dominated by one giant button: **CLOCK IN**. When clocked in, it becomes **CLOCK OUT**.
- Clocking in shows a confirmation like: *"Position opened. Good luck out there."*
- Clocking out shows a session summary: *"Position closed. Time in market: 11h 24m. Unrealized life losses: incalculable."*
- Device remembers you. Opening the app and tapping one button is the entire authentication story.

## 2. Shared workspace ("The Floor")

- Friends join a group via a 6-character invite code (make the code generator finance-flavored: codes like `EBITDA`, `LBO4EVA`, `BPSBOI`).
- The Floor shows everyone live: who's clocked in right now, how long they've been in, their current streak, and their title (see leaderboard).
- Someone clocking in past 10pm triggers a group notification: *"🌙 After-hours trading detected: [name] is back on the desk."*
- Someone crossing 12 hours in a single session gets flagged on The Floor with a small flashing **HALTED — VOLATILITY** banner.

## 3. The Daily Margin Call (the BeReal mechanic)

- Once per day, at a random time, everyone in the workspace gets a simultaneous push notification: **"⚠️ MARGIN CALL. You have 2 minutes to post."**
- Tapping it opens the camera and captures front + back photos at once — your face and whatever your screen/desk/sad desk salad looks like at that exact moment.
- Photos post to a shared daily feed, stamped with the time and your current clocked-in status and hours today.
- Post late and your photo gets a red **LATE FILING** stamp. Miss it entirely and your leaderboard entry shows **FAILED TO REPORT — SEC INQUIRY PENDING** until tomorrow.
- You can't see anyone else's Margin Call photos until you've posted yours (classic BeReal rule).
- Reactions are finance-only emoji: 📈 📉 💀 🖨️ (money printer) and a "circle back" 🔄 that just means nothing, like in real life.

## 4. Leaderboard ("League Tables")

- Weekly and monthly rankings by total hours worked.
- #1 holds the title **Managing Director of Suffering** and gets a crown on their avatar until dethroned.
- Everyone gets a title based on their weekly hours, and the ladder is the joke:
  - < 20 hrs — **Tourist**
  - 20–40 — **Intern**
  - 40–55 — **Analyst**
  - 55–70 — **Associate**
  - 70–85 — **VP (Vice President of Being Tired)**
  - 85–100 — **Executive Director**
  - 100+ — **Patrick Bateman** (title renders in Copperplate, obviously)
- Last place each week gets the title **Work-Life Balance Champion**, displayed sincerely, which somehow stings more.
- Stats page per person: hours per week chart, longest session, current streak, personal records, and a cursed metric: *"If you billed at $850/hr, you generated $X this week. You were paid approximately none of that."*

## 5. Copy & tone (very important)

All microcopy stays in character. Examples to build from:

- Empty feed: *"No filings yet today. The market is quiet. Too quiet."*
- Notification when a friend clocks in before 6am: *"[name] is up before the futures market. Concerning."*
- Weekly summary email/notification: *"Your week in review: 74 hours. Your plants miss you."*
- Clock-out button held down for 3 seconds: *"Are you sure? Nobody actually leaves."*

## 6. Easter eggs (hide these, don't document them in-app)

- **9:41 club** — clock in at exactly 9:41am and earn the hidden "Keynote" badge (Apple demo time).
- **The Konami code** on the leaderboard screen turns everyone's avatar into Warren Buffett for 60 seconds.
- Typing **HODL** in any text field triggers a confetti burst of tiny green candlestick charts.
- Log a **100-hour week** and get the "Goldman First-Year" achievement, plus the app quietly changes your clock-in button to say **TOUCH GRASS** for one day (it still clocks you in — it just judges you).
- Clock out within 5 minutes of clocking in → **"Quiet Quitter"** badge, visible to the whole Floor.
- Still clocked in on Friday at exactly 5:00pm → a very faint, tasteful sad trombone.
- Pull-to-refresh the feed 10 times in a row: *"The market is closed. Go home."*
- On your birthday, the Margin Call notification reads: **"MARGIN CALL 🎂 (yes, even today)."**
- If two friends clock in within the same second, both get the **"Synergy"** badge and the app briefly forces them to see each other's Margin Call photo first.
- Set your display name to "Bloomberg Terminal" and the whole app UI switches to orange-on-black terminal styling until you rename yourself.

## 7. Technical notes for the builder

- Mobile-first responsive web app (or React Native if the builder supports it).
- Camera access for dual front/back capture; graceful fallback to sequential capture if simultaneous isn't supported.
- Push notifications for the daily Margin Call (randomized server-side per workspace, same instant for all members), clock-in events, and weekly league table results.
- Lightweight backend: workspaces, members (device-identity + invite code, no passwords), sessions (clock in/out timestamps), daily photo posts, reactions.
- Timezone-aware: hours count toward the day they occurred in each member's local time; the Margin Call fires at one shared moment per workspace.
- All photos are private to the workspace. No public feed, no discovery, no strangers.

Ship it with the energy of someone who has 14 unread Outlook reminders about a model that was due yesterday.
