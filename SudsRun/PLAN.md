# Suds Run: Game Plan

This is the plan we can come back to. It covers what the game is, how it plays, and how it gets one message across: **putting soap in the fountain isn't as funny as it looks.**

Live URL once pushed: `https://sydneyfr.github.io/SudsRun/`

---

## 1. The pitch

A UNL student dumps soap into Broyhill Fountain, swims around in the foam, and then sprints down Greek Row with UNL Police right behind. It's an endless runner, so you just try to get as far as you can.

The fountain part is meant to feel fun. The run is the consequence. The screens between rounds show what actually happens after the foam settles.

## 2. Goals (in order)

1. **Fun first.** If students don't want to play a second round, nobody reads the message. Short rounds, quick restart, unlockable colors.
2. **Get the message across without lecturing.** The message lives in the game's structure and the between-round screens, not in walls of text.
3. **Accuracy.** This is a journalism project. Anything presented as a fact has to be true and sourced (see section 6).
4. **Respectful tone.** Police are the chasers, not villains. No mocking real people, chapters or businesses.

## 3. Game loop

```
Title screen ──► Fountain (free play, no timer)
                    │  pour soap, swim, watch the foam spread
                    ▼  "Make a run for it"
                 Siren alert (2 s)
                    ▼
                 Greek Row run (endless)
                    ▼  caught
                 "Caught!" screen
                   - distance, bubbles, best
                   - "Meanwhile, at the fountain" cleanup checklist
                   - one "Real talk" fact
                   - pick a bubble color (unlocked by best distance)
                    ▼
                 Back to the fountain ──► ...
```

### Fountain phase
- Top-down view of the fountain basin and plaza.
- Move: arrow keys / WASD, or touch and drag toward where you want to swim.
- Pour: hold **Space** or the **Pour** button.
- Foam spreads on its own and keeps growing after you stop pouring. Past about 70%, it spills over the rim onto the plaza.
- **There is no reward for pouring more soap.** It doesn't give extra points or a head start. The only thing that changes is how bad the cleanup checklist gets. That's on purpose: the "fun" part has nothing to show for it except a mess.
- A small "No soap in the fountain" sign sits on the rim as background detail.
- Text pops up as foam grows ("The pump is starting to gurgle…").

### Run phase
- Three lanes down Greek Row: houses with columns and Greek letters, trees, sidewalks. Houses are the housed IFC fraternities and PHA sororities listed on fsl.unl.edu (Nov. 2026), shown by their Greek letters (Acacia, FarmHouse and Triangle spelled out) across the top of a white two-story portico on the street side, so you can read them as you run past. Each side of the street switches between three house styles every 2–3 houses: Colonial/Georgian Revival (red brick, two-story white columned portico, shutters, dormers, chimney), Tudor (brick ground floor, half-timbered upper floor, steep slate roof, arched-door vestibule) and Craftsman (painted siding, full-width front porch with piers, name on the porch fascia). Styles and colors are in `STYLE_COLS`. The list is `HOUSE_NAMES` at the top of the run code.
- Controls: ← → (or A/D) switch lanes, ↑ / W / Space jump, ↓ / S slide. On phones: swipe, or tap to jump.
- Speed goes up the farther you run.

| Obstacle | Move | What happens if you hit it |
|---|---|---|
| Rental e-scooter parked across the lane (white frame, green stem, black deck — no brand logo) | **Jump** | Stumble. The officer catches up. A second stumble within 6 s means you're caught. |
| Lincoln city bus (navy back, green sides with darker green triangles, "LINCOLN" + route number) | **Switch lanes** | Caught |
| Giant inflatable Nebraska farmer standing with his legs apart (one lane, or three side by side) | **Slide** between his legs | Caught |

**The slide obstacle:** an original giant inflatable farmer, the kind you'd see at a tailgate. He's white, with blonde hair, a red ball cap with a white Nebraska-state patch, a plain white shirt, blue denim overalls with a red Nebraska-state patch on the bib pocket, and he waves an ear of corn. We decided not to use Herbie Husker or a lookalike, so the farmer has his own look. He's drawn in `drawMascot()`. Collision doesn't depend on the art: anything taller than 1.05 m hits, so you always have to slide.

- Bubbles float along the lanes as pickups, in the color you picked. The runner also leaves a trail of bubbles behind them, since they're still soapy.

### Colors (unlocked by best distance)

| Color | Distance needed |
|---|---|
| Classic Suds (white) | 0 m |
| Husker Red | 200 m |
| Pool Blue | 450 m |
| Scooter Green | 750 m |
| Purple Haze | 1,100 m |
| Gold | 1,600 m |
| Rainbow | 2,300 m |

The color changes both the fountain foam and the bubbles in the run. Best distance and color are saved in the browser (localStorage).

## 4. How the message gets across

Most of the message comes from how the game is built, not from text on screen.

1. **Cause → effect structure.** Every round starts with the prank and ends with getting caught. You can't play the fun part without the consequence.
2. **No reward for the prank.** More foam doesn't help your score. It just makes the cleanup list longer.
3. **"Meanwhile, at the fountain."** After each run, a checklist shows the cleanup work your foam level would cause (shut off, skim, drain, rinse, check pumps, refill). It's built from your actual coverage %, so it feels personal.
4. **One fact per round.** A short, rotating "Real talk" line. One sentence, not a paragraph, so it doesn't feel like homework.
5. **A "Why this game?" screen** for anyone who wants the full explanation.
6. **Tone:** jokes about the chase, never about the damage. The game laughs *with* the player while running, and is straight with them about the fountain.

### What we avoid
- Making the prank look consequence-free or cool.
- Made-up statistics. There are **no invented dollar amounts or hour counts** in the game right now. The cleanup checklist describes the kind of work involved, not a cost.
- Preachy pop-ups that interrupt gameplay.

## 5. Things to test with students

- Do they play more than 2 rounds? (Fun check)
- After playing, can they name one consequence of soaping the fountain? (Message check)
- Do they skip the "Caught!" screen right away? If yes, shorten the text.
- Does it work on phones? Most students will open it from a link on their phone.

## 6. Reporting to-do (before promoting the game)

These would make the message stronger, and we need sources before putting them in:

- [x] Confirmed: Broyhill Fountain outside the Nebraska Union was soaped around 2 a.m. Sept. 5, 2026. Per the Daily Nebraskan (Paige Nickless, Sept. 15, 2026), it caused thousands of dollars in damage, the fountain was drained and refilled, and the filter was replaced. Interim UNLPD Chief Marty Fehringer was quoted on the cost. Summarized and linked on the "Why this game?" screen.
- [ ] Ask UNL Facilities for an exact dollar figure for the Sept. 5 cleanup. If they give one, add a "real cost" line to the Caught screen **with attribution**.
- [ ] Ask UNLPD what charge usually applies (likely criminal mischief, Neb. Rev. Stat. 28-519, where the penalty depends on the damage amount) and whether anyone has been cited for it.
- [ ] Ask Student Conduct whether this leads to a conduct referral.
- [x] Linked the Daily Nebraskan crime-log story from the "Why" screen. Could still check the archives for earlier fountain-soap incidents.
- [ ] Follow up with UNLPD on whether anyone was identified or cited for the Sept. 5 incident.
- [x] Swapped the Group Therapy pedal bus for a Lincoln city bus, and the scooters now match the white/green rental scooters on campus (no logo). Text labels are constants at the top of the script (`BUS_LABEL`, `FOUNTAIN_NAME`).

## 7. Future ideas

- Sound toggle is in. Could add a short music loop.
- Power-up: "Hoodie up" (one free stumble).
- More obstacles: a tour group crossing the street, a sprinkler, a construction cone line.
- Daily challenge seed so friends can compare scores on the same course.
- Shareable score card: "I made it 840 m down Greek Row. The fountain is still closed."

## 8. Tech notes

- One file: `SudsRun/index.html`. Plain canvas + JavaScript, no libraries, no build step. GitHub Pages serves it as is.
- Canvas is 960×540 and scales to fit the window (16:9).
- Phones: menu screens (title, Why, Caught, Pause) cover the whole screen with fixed, readable text sizes, and tighter spacing on short sideways screens. Gameplay is landscape-only: holding a phone upright during play shows a "Turn your phone sideways" screen and pauses a run in progress.
- Saved data uses localStorage keys starting with `sudsrun.`.
- Editable text (facts, colors, labels) is near the top of the script.
