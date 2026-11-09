# Trivia Bingo

A family web app that replaces the question cards from the *Trivia Bingo* board game. Players open the site on their phones, take turns reading questions, and play on paper boards or on digital 5×5 cards. Everything syncs live between phones.

No app store, no accounts: open the link in Safari or Chrome and play.

---

## How to play

1. **Join.** Everyone opens the site and adds their name. The order people join is the **reading order**.
2. **Draw.** The current reader taps **Draw a question**.
   - The reader sees the full card: question, number, and **or more** / **or less**.
   - Everyone else sees only the question. The number stays hidden as "?".
3. **Answer.** Players call out answers. If the **timer** is on, a countdown runs on every phone.
4. **Reveal.** The reader taps **Reveal answer to everyone**.
   - **Paper cards:** if you get it right and mark your board, tap **Number pressed**.
   - **Digital cards:** squares you're allowed to mark glow green. If you got it right, tap one.
5. **Pass.** The reader taps **Pass to next reader** and the turn moves on.
6. **Bingo.** Tap **BINGO!** when you win. Confetti plays on every phone when the win is confirmed.

### "Or more" and "or less"
Every card has a direction. For **19 or less**, a correct answer lets you mark **any number from 1 to 19**. For **12 or more**, **any number from 12 to 31**.

---

## Settings (bottom of the page)

| Setting | Options | What it does |
|---|---|---|
| **Timer** | Off / 15s / 30s / 60s | Countdown that starts when a card is drawn. Beeps at zero. |
| **Cards** | Paper / Digital | **Paper** works with the physical boards. **Digital** gives each player a 5×5 card on their phone. |
| **New game** | – | Reshuffles the question deck, resets turns, and deals new digital cards. Asks you to confirm first. |

Settings apply to everyone in the game.

### Digital cards
- 25 random numbers from 1–31 per card, with no free space.
- **No two boards in a game are the same.**
- You need **2 complete lines** to win (any rows, columns or diagonals). The BINGO button only appears once you have them, and the win is confirmed automatically.
- Tap a marked square again to undo it, or tap another green square to move your mark.

### Paper cards
- When someone taps **BINGO!**, play pauses so everyone can check their card.
- **Card checks out** confirms the win (with confetti). **False alarm** returns to the game.

---

## Add it to your home screen

- **iPhone:** open the link in **Safari** → **Share** → **Add to Home Screen**. It opens full screen with its own icon, like an app.
- **Android / Chrome:** **⋮** menu → **Add to Home screen** (or **Install** on a computer).

A Safari bookmark also works, but it opens as a normal webpage instead of full screen.

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

Keep the commas between lines, then upload the file again (see **Updating the site**).

---

## How it's built

| File | Purpose |
|---|---|
| `index.html` | The whole app: layout, question deck and game logic |
| `manifest.json` | App name, colours and icon for "Add to Home Screen" |
| `icon-180.png`, `icon-512.png` | Home-screen icons |

- **Hosting:** GitHub Pages (free) serves the files at `https://<username>.github.io/trivia-bingo/`.
- **Live sync:** a free Firebase Realtime Database stores the shared game: players, turns, the current card, cards and marks. Every phone listens for changes, so updates appear instantly.
- The Firebase settings in `index.html` are meant to be public. The database rules decide what anyone can do.

### Firebase database rules
Under Firebase console → Realtime Database → **Rules**:

```json
{ "rules": { "rooms": { "$room": { ".read": true, ".write": true } } } }
```

These let the app read and write only the game data under `rooms/` and keep everything else locked.

---

## Updating the site

1. Open the `trivia-bingo` repository on github.com.
2. **Add file** → **Upload files** → choose the new `index.html` (it replaces the old one).
3. Tap **Commit changes**.
4. The site updates in 1–2 minutes. Refresh the page (or close and reopen the home-screen app) to get the new version.

---

## Good to know

- **Anyone with the link can join.** Share it with family only.
- **Separate game spaces:** change `const ROOM = "family";` in `index.html` to another word (e.g. `"friends"`) for a separate game that doesn't share players or turns.
- **Same person, new phone:** type the same name to reconnect to your spot.
- **Removing players:** tap **Remove** next to a name, or **Leave** next to your own.
- **Needs internet:** every phone syncs through the database, so the app doesn't work offline.
- **Costs:** Firebase's free plan and GitHub Pages are far more than enough for a family game.

### Troubleshooting

| Message at the bottom of the page | Fix |
|---|---|
| *Can't read the game. Check the database rules in Firebase.* | Paste the rules above into Firebase and tap **Publish**. |
| *Can't reach the game yet. Check your connection.* | Check Wi-Fi or data, then reload. |
| *Reconnecting…* | Temporary. It reconnects on its own. |
| Old version still showing | Wait 2 minutes after uploading, then reload. On iPhone, close the home-screen app fully and reopen it. |
