# WinAdda

A points-based rewards front-end: login/signup, a home screen for games, a daily
free spin wheel, a Store for fixed-price cosmetic items (board skins), and a
Wallet where players can buy points or add money at a fixed rate.

**Important — read before adding real payments:**
India's *Promotion and Regulation of Online Gaming Act, 2025* bans real-money
games where you pay for a *chance* at winning something (skill or luck based).
To stay on the right side of that law, this app keeps the two flows separate:

- The daily **spin is free** — nobody pays to spin, so it isn't real-money
  gaming, even though the points amount is random.
- **Buying points or Store items has a fixed price** — pay ₹100, get exactly
  100 points, every time. Pay ₹49, get exactly the Golden Board, every time.
  No randomness is involved in anything you pay for.

Do **not** change this so that paid points can be spent on the spin wheel, or
so that spinning costs money — that would turn it back into real-money
gambling and break the law. If you want to monetize further, add more fixed-
price Store items instead.

## Files

Everything (HTML + CSS + JS) is bundled into the single `index.html` file, so
that's the only file you need to upload — it also means you can just
double-click it locally to preview, no server needed.

## How it works

- Pure HTML/CSS/JS, no build step, no server. Works straight on GitHub Pages.
- Accounts and points are stored in the browser's `localStorage`. That means:
  - No real password security — don't reuse a real password here.
  - Data is per-browser/device. Logging in on a different phone or clearing
    site data will not see the same account.
  - This is fine for a demo / personal project. If you later want real
    accounts that sync across devices, you'd need a backend (e.g. Firebase
    Auth + Firestore, or Supabase) — happy to help wire that up when you're
    ready.
- One free spin per calendar day (tracked per account). Spin values: 5, 10,
  15, 20, 25, 30, 50, 100 points (edit `WHEEL_VALUES` in `script.js`).
- **Store**: fixed-price cosmetic items (board/skin placeholders for the
  future Ludo/Chess/Adda Runner games). Each item can be bought with points,
  with wallet money, or both — edit `STORE_ITEMS` in `script.js`.
- **Wallet**: two ways to top up —
  - *Buy Points*: pay ₹X, get X points (1:1), with 100/200/500/1000 presets
    or a custom amount (minimum 100).
  - *Add Wallet Money*: pay ₹X, get ₹X in the wallet, to spend on Store items
    later.
- **Payments are manual**, since this is a static site with no backend or
  payment gateway:
  1. Put your own UPI QR code image in the project folder named
     `upi-qr-placeholder.png` (same folder as `index.html`).
  2. Set `OWNER_WHATSAPP` in `script.js` to your WhatsApp number (country
     code, digits only, e.g. `919876543210`) so the "Send payment proof on
     WhatsApp" button opens a chat with you.
  3. When someone pays, verify it yourself, then give them one of the codes
     from `REDEEM_CODES` in `script.js` that matches what they paid.
  4. They enter that code on the Wallet screen to get their points/money.
  Each code works once per browser — add more codes and redeploy whenever
  you run low. This isn't fully automatic, but it's the realistic option for
  a free static site; a real payment gateway (Razorpay/Cashfree) would need a
  small backend, which is a natural next step if this takes off.
- The three game cards (Ludo, Chess, Adda Runner) are placeholders marked
  "Coming soon" — tapping them just shows a toast. Build the real games later
  as separate pages/scripts and swap the placeholder cards for links.

## Deploying to GitHub Pages

1. Create a new GitHub repo (e.g. `winadda`).
2. Add `index.html` to the repo root.
3. Commit and push.
4. In the repo, go to **Settings → Pages**.
5. Under "Build and deployment", set **Source** to `Deploy from a branch`,
   branch `main`, folder `/ (root)`. Save.
6. Wait a minute, then your site will be live at
   `https://<your-username>.github.io/winadda/`.

## Where to extend later

- `script.js` has clearly separated sections for auth, navigation, spin, and
  withdraw — add a `screen-ludo` etc. section in `index.html` and a matching
  nav/game-card link when a game is ready.
- Point balance and withdrawn balance are just numbers on the user object in
  `localStorage`, so any new game can read/write `user.points` the same way
  the spin wheel does.
