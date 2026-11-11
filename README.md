# ⚡ QuizVerse — Online Multiplayer Quiz Bee

A static HTML/CSS/JavaScript quiz game for GitHub Pages. Uses **PeerJS 1.5.5** over WebRTC (loaded from unpkg) for genuine browser-to-browser multiplayer. No custom backend or shared database. **A public PeerJS signaling service is still required** for peer discovery, and connectivity may fail on restrictive NATs/firewalls without TURN. It is not guaranteed to work on every network.

## Deploy to GitHub Pages
1. Sign in to GitHub and create a new public repository named `quizverse`.
2. Upload `index.html`, `css/`, and `js/` (preserve folders). You can also upload this `README.md`.
3. Open the repository's **Settings → Pages**.
4. Under **Build and deployment**, select **Deploy from a branch**, then `main` and `/ (root)`. Save.
5. Open `https://YOUR-USERNAME.github.io/quizverse/` once publishing completes.
6. Host: enter a nickname → **Create Game** → choose quiz and settings → **Create Room** → copy room link/code.
7. Guest: open invite link or site → enter a different nickname → **Join Game** → enter code if necessary → **I'm Ready**.
8. Host clicks **Start Game** once two or more participants are connected and ready. Keep the host's browser tab open.

**Local testing:** ES modules require HTTP(S), not `file://`. Use GitHub Pages or a simple static development server. No server is needed for *deployment*.

## Custom questions
Use **Create Your Own Quiz** on the homepage. Set the quiz title and write questions using multiple-choice, true/false or identification. Identification allows alternate accepted answers separated by `|`; comparison is case-insensitive with extra whitespace normalized. Save to this browser's `localStorage`, or export JSON to share/backup. Import JSON on another device. Only the host needs to load the quiz: visitors receive questions round-by-round without receiving the whole answer key.

## Modes and scoring
- **Classic:** Easy 100, Medium 200, Hard 300 points (or per-question override).
- **Speed:** 1st correct answer 1000, 2nd 800, 3rd 600, subsequent 400, using receipt order/time on the host. Tight races may be affected by network latency.
- **Elimination:** 3 lives; wrong/missing answer costs one. Game ends when one or zero active players have lives remaining, or questions are exhausted. Standings sort by remaining lives, then points, correct answers and nickname. Simultaneous elimination ties may require a human-decided rematch.
- Standard ties sort by score, correct answers, average answer time, then nickname.

## Limitations
- Live match state exists **only in the host tab's memory**. Closing/reloading the host ends the room. There is no host migration or recovery.
- Refreshing a guest's browser disconnects them; for fairness, join is only allowed before the game starts. Reconnect during an active match is not supported in this release.
- PeerJS signaling, WebRTC NAT traversal, and CDN dependencies require Internet access. Public signaling is not a managed SLA. Networks with restrictive firewalls may need TURN or a hosted signaling service, which would violate an absolute no-service requirement.
- The host is authoritative for answers and scoring, but browser code is inspectable and modifiable. This is **not cheat-proof** and should not be used for high-stakes competitions.
- Quizzes are not shared/persisted across browsers unless JSON is explicitly exported/imported. `localStorage` may be cleared by the browser.
- 2–10 players supported in the lobby. Actual practical capacity depends on the host network and device.
- No framework, accounts, analytics, cloud database or paid service.

## Testing checklist
These tests require live devices/browsers and **have not been executed** merely by generating the code:
- [ ] Two players on different networks can join using a room code
- [ ] 2–10 distinct player nicknames appear in one lobby
- [ ] Duplicate nickname and full-room join are rejected
- [ ] All participants see the same question and countdown
- [ ] The host receives and validates each player's answer once
- [ ] Correct answer is not sent to guests before reveal
- [ ] All leaderboards synchronize after each question
- [ ] Answer timer expiry and skipped questions score correctly
- [ ] Classic, Speed and Elimination each finish and rank correctly
- [ ] Custom quiz create/save/edit/import/export works
- [ ] Guest disconnect is visible; host disconnect ends room
- [ ] Copy invite link and join via URL query string
- [ ] Mobile screen and keyboard usability
- [ ] GitHub Pages loads JS modules and PeerJS CDN
- [ ] Signaling outage and restrictive network show errors

## Troubleshooting
- **PeerJS could not load:** Check Internet, ad blockers, or availability of `unpkg.com`.
- **Room unavailable:** Check the 6-character room code, keep the host online, try another network. PeerJS public service can be unavailable.
- **Can't join game in progress:** Joining is limited to lobby; start a new room.
- **Custom quizzes gone:** This browser's storage was cleared; restore the exported JSON.
- **Nothing happens opening index.html directly:** Use the published GitHub Pages link (ES module loading often fails with `file://`).
