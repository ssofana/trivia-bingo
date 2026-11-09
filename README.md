# Trivia Bingo

A family web app that replaces the question cards from the *Trivia Bingo* board game. Players open the site on their phones, take turns reading questions, and play on paper boards or on digital 5×5 cards. Everything syncs live between phones.

No app store and no accounts: open the link in Safari or Chrome and play.

---

## Starting a game

1. **Host:** open the site and tap **Start a new game**. You get a 6-character **game code** (e.g. `K7QF2M`) and become the game's **host**.
2. **Invite:** tap **Invite** to text the link, or read out the code. Others tap the link, or type the code under **Join with a game code**.
3. **Join:** everyone adds their name. The order people join is the **reading order**.

Each phone remembers its last game and goes straight back to it. **Switch game** returns to the start screen.

## How to play

1. **Draw.** The current reader taps **Draw a question**.
   - The reader sees the full card: question, number, and **or more** / **or less**.
   - Everyone else sees only the question. The number stays hidden as "?".
2. **Answer.**
   - **Digital cards:** tap the number on your card you think is the answer. It turns purple-grey, and you can change it until the reveal. The reader doesn't pick.
   - **Paper cards:** call out answers as usual.
   - If the **timer** is on, a countdown runs on every phone and picks lock at zero.
3. **Reveal.** The reader taps **Reveal answer to everyone**.
   - **Digital:** a correct pick turns **green** and stays. A wrong pick turns **red**.
   - **Paper:** if you got it right and marked your board, tap **Number pressed**.
4. **Pass.** The reader taps **Pass to next reader**.
5. **Bingo.** Tap **BINGO!** when you win. Confetti plays on every phone when the win is confirmed.

### "Or more" and "or less"
Every card has a direction. For **19 or less**, any number from 1 to 19 is correct. For **12 or more**, any number from 12 to 31. Picking the exact answer is always safe. Picking near it works only if you guess the direction right.

---

## Host settings (bottom of the page, host only)

| Setting | Options | What it does |
|---|---|---|
| **Timer** | Off / 15s / 30s / 60s | Countdown from when a card is drawn. Beeps at zero and locks picks. |
| **Cards** | Paper / Digital | **Paper** works with the physical boards. **Digital** gives each player a 5×5 card on their phone. |
| **New game** | – | Reshuffles the question deck, resets turns, and deals everyone a new digital card. |
| **Remove** | – | Removes a player from the reading order. |

Other players see who the host is and can only **Leave** themselves.

### Digital cards
- 25 random numbers from 1–31 per card, with no free space. **No two boards in a game are the same.**
- **Green** squares (correct) stay for the whole game.
- **Red** squares (wrong) stay until it's **your turn to read**, then clear.
- You need **2 complete lines** (any rows, columns or diagonals). Completed lines get a gold ring. The BINGO button appears once you have two, and the win is confirmed automatically.
- The reading order shows **Picked** before the reveal, then **Correct** or **Missed** for each player.

### Paper cards
- When someone taps **BINGO!**, play pauses so everyone can check their card.
- **Card checks out** confirms the win (with confetti). **False alarm** returns to the game.

---

## Add it to your home screen

- **iPhone:** open the link in **Safari** → **Share** → **Add to Home Screen**. It opens full screen with its own icon, like an app.
- **Android / Chrome:** **⋮** menu → **Add to Home screen** (or **Install** on a computer).

---

## Question deck

There are 93 questions, three for every number from 1 to 31, across **Math, Geography, History, Movies, Pop culture**, plus a few **Books** and **Sports**. A card won't repeat until the whole deck has been used.

### Edit or add questions
In `index.html`, find `const BANK = [`. Each line is one card:

```js
[12, "The number of people who have walked on the Moon", "more", "History"],
```

| Part | Meaning |
|---|---|
| `12` | The answer (1–31) |
| `"The number of…"` | The question as shown on the card |
| `"more"` or `"less"` | The direction for marking |
| `"History"` | Category label shown above the question |

Keep the commas between lines, then upload the file again.

---

## Security

The site is public, but each game is locked down by the Firebase database rules in `database.rules.json`:

- **Private game codes.** Games live under random 6-character codes, and the database refuses to list them. Without the code there's nothing to find or join.
- **Invisible sign-in.** Each phone gets an anonymous Firebase ID automatically, with no accounts or passwords. The rules use it to check who is writing.
- **Ownership:**
  - Players can change only their own name, card and picks.
  - Only the host can change settings, start a new game or remove other players.
  - Only people who've joined a game can run its turns.
  - No one can take over as host.
- **Validation.** Every field is checked: names up to 24 characters, card numbers 1–31, only the 25 card squares, timer values from the menu. Unknown fields and anything outside a game are rejected.

The Firebase settings in `index.html` are meant to be public. The rules are what control access.

---

## How it's built

| File | Purpose |
|---|---|
| `index.html` | The whole app: layout, question deck and game logic |
| `manifest.json` | App name, colours and icon for "Add to Home Screen" |
| `icon-180.png`, `icon-512.png` | Home-screen icons |
| `database.rules.json` | Firebase Realtime Database security rules (paste into Firebase; not used by GitHub) |

- **Hosting:** GitHub Pages (free) at `https://<username>.github.io/trivia-bingo/`.
- **Live sync:** Firebase Realtime Database (free plan). Every phone listens for changes, so updates appear instantly.
- **Sign-in:** Firebase Anonymous Authentication.

### Data layout

```
rooms/<CODE>/
  host        uid of the person who created the game
  settings    timerSecs, digital, round           (host only)
  state       turn, card, revealed, used, bingo   (players in the game)
  players/<uid>  name, at, card, marked, misses   (that player, or the host)
  picks/<uid>    the player's pick for the current question  (that player)
```

---

## Updating the site

1. Open the `trivia-bingo` repository on github.com.
2. **Add file** → **Upload files** → choose the new `index.html` (it replaces the old one).
3. Tap **Commit changes**. The site updates in 1–2 minutes.

If you change `database.rules.json`, paste it into Firebase → Realtime Database → **Rules** and tap **Publish**. Uploading it to GitHub alone does nothing.

---

## Good to know

- **New phone:** joining from a new phone adds you as a new player. The host can remove your old entry.
- **Needs internet:** every phone syncs through the database, so the app doesn't work offline.
- **Costs:** the Firebase free plan has no billing attached, so it can't charge you. If it ever hit its daily limit, it would pause until the next day.

### Troubleshooting

| What you see | Fix |
|---|---|
| *Couldn't sign in. Turn on Anonymous sign-in…* | Firebase → Authentication → Sign-in method → **Anonymous** → Enable. |
| *Couldn't start a game…* / *Can't read the game…* | Paste `database.rules.json` into Firebase → Realtime Database → Rules → **Publish**, and check Anonymous sign-in is on. |
| *No game found with code …* | Check the code. Codes never use 0, O, 1 or I. |
| *Can't reach the game yet* | Check Wi-Fi or data, then reload. |
| Old version still showing | Wait 2 minutes after uploading, then reload. On iPhone, close the home-screen app fully and reopen it. |
