# Bus Captain — Product Definition (IDEA.md)

> **Status:** agreed with the owner on 2026-11-23. This file is the source of truth for every later step — read it, not the chat history.
> **Research:** risks, legal and store findings are in [RESEARCH.md](RESEARCH.md); competitors and the USP are in [COMPETITORS.md](COMPETITORS.md). The research rules P-1…P-14 (approved 2026-11-23) and the USP rules U-1…U-6 (approved 2026-11-24) are listed at the end of this file under "Rules adopted from research and the USP".
> **Legal status:** nothing found blocks launch. Professional advice is still needed **before ads (v1.1)** and **before in-app purchases (v1.2)** — see RESEARCH.md L-16. v1 needs none.

**Labels:** **[F]** FACT — the owner said or decided it · **[A]** ASSUMPTION — inferred; correct it if wrong · **[Q]** OPEN QUESTION — nobody knows yet.

| | |
|---|---|
| Name | **Bus Captain** [F] — renamed from "Bus Mudhalali" on 2026-11-23. "Captain" = the driver, as in Indian mobility apps (Rapido Captain) [F]. Tamil display name பஸ் கேப்டன் [A] |
| Name check | 2026-11-23, Google Play India search: no *game* titled "Bus Captain". One bus-staff app is titled "Bus Captain-Yolobus" (1K+). "Captain" is the standard word for drivers in Indian mobility apps (Rapido Captain 50M+, Captain by Cityflo). IP India trademark search not yet done [Q — owner] (RESEARCH.md L-14) |
| Platform | Android phones, Google Play only [F]. No iOS [A] |
| Engine | libGDX (Java) [F] |
| Repo | https://github.com/rameshkumark24/Bus-Captain [F] — public since 2026-11-23 |
| Package name | **`io.github.rameshkumark24.busgame`** [F] (approved 2026-11-23). Neutral, so a rename costs nothing. Permanent from the first Play Console upload |
| Distribution | **India only at launch** [F] (P-1). Other countries only after a legal check of each |

---

## 0. Reality check (read before planning money)

- **Money** [A]: Indian idle-game players earn roughly ₹0.4–0.9 per daily player per day (ads + small purchases). ₹10,000/month ≈ 400–800 daily players ≈ 2,000–4,500 *new* installs **every month**, with no marketing budget. ₹1 lakh/month is ten times that. Replace these guesses with real AdMob numbers after v1.1.
- **Scope** [A]: the full plan (M0–M6 below) is roughly 4–6 months of part-time work. The v1 cut in §8 takes about 6–8 weeks of building plus the 14-day closed test.
- **Nothing found that stops the product** (see RESEARCH.md). The biggest risks are discovery, Tamil text rendering, and the Indian under-18 rules for ads.

---

## Original brief (owner's spec, condensed) [F]

**Pitch:** start with one old town bus in Coimbatore. Run routes, buy buses, hire drivers and conductors, paint your own bus colours, grow into the biggest bus company in Tamil Nadu. Money keeps flowing while you're offline.
**Core feeling:** watching buses fill up, depart, return and pay out, then reinvesting and seeing everything get faster and bigger.

**Originality and legal rules:**
- Fictional bus companies and manufacturers only. No real names, logos or badges (no TNSTC, SETC, KSRTC, Ashok Leyland, Tata, Volvo, BharatBenz…).
- City and town names are fine.
- No real celebrities, film songs, film melodies or religious imagery on buses.
- In-game cash uses a **coin icon, never ₹**. No real-money mechanics, no loot boxes.

**Hard constraints:**
- Zero cost, no server, no paid assets or services, fully playable offline.
- Assets: CC0 (e.g., Kenney) or shapes drawn in code; OFL fonts (Noto Sans Tamil); sounds from jsfxr/CC0; horn tunes are original synths; keep CREDITS.md.
- Smooth on 2–3 GB RAM phones; 30 fps on idle screens; AAB under ~40 MB.
- UI in English and Tamil, with a language switch.

**Quality bar:**
- First bus departs and pays out within **10 seconds**; one-line hints only.
- Numbers never overflow or go negative.
- Offline earnings stay correct across time-zone and clock changes; backward clock jumps are rejected.
- Unit tests cover economy formulas, offline earnings, prestige maths, save/load, and save migration between versions.

**Working style:** milestone by milestone. After each one, stop, say exactly what to test on the phone, list known issues, and wait for "go".

**Milestones as originally specified** (the build order to v1 is in §8):
- **M0 — setup and risk checks.**
  - Reuse proven parts of the old idle repo `rameshkumark24/App` without modifying it. The repo is not accessible, so we build fresh unless the owner provides it [Q].
  - Tamil text spike, tested on கொ, கோ, கௌ, க்ஷ, ஸ்ரீ and "பேருந்து நிலையம்", with screenshots.
- **M1 — core loop, one depot in Coimbatore.**
  - Bays, a passenger queue, boarding count-up, departure, an animated route strip, return, coin burst and sound.
  - 3 town routes, each with fare, trip time, time-of-day demand and capacity. Revenue = min(waiting, capacity) × fare.
  - Upgrades: seats, engine, boarding (conductor), depot bays, maintenance. A breakdown is a short delay, never lost progress.
  - Offline earnings capped at 4 h, extendable.
  - All economy numbers in JSON, plus a headless desktop balance simulator that prints a timeline.
  - Targets: 2nd bus ~2 min · 1st new route ~5 min · next city ~30–60 min · first prestige ~day 3.
- **M2 — Tamil Nadu expansion.**
  - A map of 12 cities (Coimbatore, Tiruppur, Erode, Salem, Madurai, Trichy, Thanjavur, Tirunelveli, Chennai, Kanyakumari, Vellore, Ooty) with ~30 routes.
  - Route tiers: town → mofussil (intercity) → express → night sleeper/AC. Ooty ghat route: slower, with a bonus fare.
  - 15 fictional bus models; managers automate dispatch per city.
- **M3 — livery painter (the signature feature).**
  - Buses built from layered masks (body, stripes, roof, window frame, bumper), tinted at runtime.
  - Palettes; stripe, wave and kolam-style patterns; company name board (Tamil or English); fictional number plate; destination board; horn picker.
  - Liveries saved as small JSON, applied per bus or company-wide.
  - "Share my bus" card via the Android share sheet.
- **M4 — reasons to come back.**
  - Events: Pongal and Deepavali rush, monsoon week, temple-festival crowds.
  - Daily login reward, 3 daily tasks, a Rush Hour boost.
  - Prestige "Expand to a new region" for Reputation Stars (a permanent multiplier).
  - Play Games achievements; checksummed local save; optional cloud save.
- **M5 — money.**
  - AdMob rewarded ads, always optional: 2× offline earnings, Rush Hour, free upgrade.
  - ~~Interstitials at most once per 10 minutes of active play, never in the first session~~ — **superseded 2026-11-24 by U-3: no interstitials at all, rewarded ads only.** No banner on the main screen.
  - UMP consent for the EEA and UK (not needed while P-1 keeps distribution India-only).
  - IAP: Remove Ads (₹99 / $1.99), livery pattern packs (₹49), starter pack (₹79), restore purchases. No pay-to-win walls. [A] With no forced ads (U-3), "Remove Ads" becomes a ₹99 "skip the ad" pass: rewarded boosts without watching. Owner to confirm before v1.2.
  - TEST ad IDs until the owner says release.
- **M6 — release prep.**
  - Privacy policy on GitHub Pages and Data safety answers; target audience 13+; content rating.
  - English and Tamil store listing. Keywords: "bus tycoon", "bus game", "idle bus", "Tamil bus game", "bus company".
  - Icon concept, a 6-screenshot plan (a painted bus + the map), and a 20-second trailer script.
  - Closed test: 12 testers × 14 days; record where each tester stopped and collect their best livery.
  - README with build and release steps.

---

## 1. What it is

[F] An offline-first idle tycoon for Android set in Tamil Nadu. You start with one old town bus in Coimbatore, watch it fill, depart, return and pay out, then reinvest in upgrades, buses, routes and cities. Managers automate it, and prestige gives a permanent multiplier. A livery painter makes the company yours, and you share your bus on WhatsApp or Instagram. English and Tamil, fictional brands, coin currency, optional ads, cosmetic purchases, zero running cost, smooth on cheap phones.

**Product type — DECIDED [F]: A. Idle tycoon first, with the livery as its signature.** Rejected alternative: B, a livery creator with a tycoon frame. The livery and share card still ship in v1, so we learn whether players' energy goes there.

## 2. Complete feature set (the destination, not v1)

**CORE** — without these it isn't the product
- [F] Depot cycle: arrive → board (count up) → depart → route strip → return → coin burst and sound
- [F] Routes with fare, trip time, time-of-day demand and capacity; revenue = min(waiting, capacity) × fare
- [F] Upgrades: seats, engine, boarding, bays, maintenance; a breakdown is a delay, never a loss
- [A] Buy buses and assign them to routes. Tap to dispatch at first, then managers automate
- [F] Offline earnings, capped at 4 h, extendable
- [F] 12-city map, ~30 routes, 4 tiers, Ooty ghat route, 15 fictional buses, managers
- [F] Prestige for Reputation Stars. [A] Liveries, company name and purchases survive prestige
- [F] Save with checksum and version migration. [A] Plus crash-safe writes and 2–3 rotating backups
- [F] English and Tamil with correctly shaped Tamil
- [F] Numbers never overflow or go negative. [A] Plus a NaN guard on every write
- [F] Economy numbers in JSON, a headless balance simulator, unit tests

**EXPECTED** — players assume these exist
- [A] Buy ×1 / ×10 / Max
- [A] "Welcome back" summary of offline earnings
- [A] Settings: language, sound/music/vibration, number format, battery saver, privacy policy, credits and licences, version, reset progress
- [A] Number format: Indian (L, Cr), international (K, M, B) or scientific. Indian switches to letter suffixes past ~1,000 Cr
- [A] Progress survives a phone change: Android Auto Backup first (free, no SDK), Play Games cloud save later
- [A] Opt-in local notifications: depot full, event started, reward ready
- [F] Daily reward, 3 daily tasks, achievements, Rush Hour boost
- [F] Optional rewarded ads, Remove Ads, restore purchases
- [A] Rating prompt (Google's in-app review) after a happy moment
- [A] Stats screen: playtime, trips, cities, prestiges
- [A] Vibration feedback; notched, tall and tablet screens; working back gesture; big tap targets; no red/green-only signals
- [A] Never blocks on the network ("no ad available", "store unavailable")

**DIFFERENTIATING** — why someone picks this over the generic bus idle games
- [F] Tamil Nadu itself: real cities, mofussil culture, the ghat bends to Ooty, Pongal/Deepavali rush, monsoon week
- [A] Real local place names on routes (bus stands, localities), so players find their own town
- [F] Livery painter: palettes, stripe/wave/kolam patterns, Tamil or English name board, fictional plate, destination board
- [F] Share card to WhatsApp/Instagram. [A] It's the only marketing channel, so the card carries the game's name and the caption carries a tagged Play link
- [F] Original horn tunes. [A] Plus recorded conductor calls ("Gandhipuram! Gandhipuram!")
- [A] Tamil done properly: correctly shaped text and natural spoken Tamil written by a native speaker
- [F] Small, offline, smooth on 2–3 GB phones. Fair: no loot boxes, no pay walls

**LATER** — deliberately deferred
- [A] Livery codes: share a design as a short text code friends paste in
- [A] Play Games leaderboards; a home-screen income widget
- [A] New regions after Tamil Nadu (cities in Kerala, Karnataka) as prestige content
- [A] Livery contests on an Instagram hashtag
- [A] iOS (needs its own Tamil text path and $99/yr); more Indian languages

## 3. The one core loop [F]

**Watch a bus pay out → spend on whatever makes the next payout bigger → watch it pay out bigger.** It repeats every few seconds while playing, on every return for offline earnings, and every few days at prestige.

The differentiator lives *inside* the loop: the buses you watch fill up are the ones you painted, each new model is a new canvas, and payouts look and sound local.

**Design rule** [A]: the first few minutes use tap-the-whistle dispatch, which also delivers the 10-second first payout. The Coimbatore manager unlocks within the first session, so the first return visit earns offline.

## 4. Every screen (✓ = in v1)

| # | Screen | Purpose | v1 |
|---|---|---|---|
| 1 | Splash / loading | Logo while art loads, under 3 s on a cheap phone | ✓ |
| 2 | Language picker | First launch: two big buttons, English / தமிழ் | ✓ |
| 3 | Ad consent (Google UMP form) | EEA/UK only; ads wait for it | |
| 4 | Notification priming | "Ping you when the depot is full?" before Android's one-time prompt | |
| 5 | Depot (home) | Bays, queue, buses, route strip, coins, income/sec, one-line hint | ✓ |
| 6 | Routes panel | Fare, trip time, demand by hour; assign buses, unlock routes, hire the city's manager | ✓ |
| 7 | Upgrades panel | Seats, engine, boarding, bays, maintenance; ×1/×10/Max | ✓ |
| 8 | Garage | Your buses (model, route, livery) and new models to buy, by tier | ✓ |
| 9 | Managers | Hire and assign across many cities (v1 hires from #6) | |
| 10 | Tamil Nadu map | Cities, locked or unlocked, unlock cost, income; celebration on unlock | ✓ |
| 11 | Welcome back | Time away, coins earned, whether the cap was hit, optional 2× | ✓ |
| 12 | Livery editor | Layers, palettes, patterns, name board, plate, destination board, horn | ✓ |
| 13 | Name entry | Android's own text box, so Tamil keyboards work | ✓ |
| 14 | Livery library | Saved designs; apply to one bus or the whole company | |
| 15 | Share card preview | Square or story-shaped image, then the share sheet | ✓ |
| 16 | Prestige | Stars you'll get, what resets, what stays; confirm | ✓ |
| 17 | Events | Current and upcoming festival events, rules, timer | |
| 18 | Daily reward | 7-day calendar | |
| 19 | Daily tasks | 3 tasks and their rewards | |
| 20 | Rush Hour | Start the boost; timer | |
| 21 | Achievements | Google Play Games' own achievements screen | |
| 22 | Shop | Remove Ads, pattern packs, starter pack; owned state; restore | |
| 23 | Rewarded-ad offer | "Watch a short ad for 2×?", with a no-ad-available fallback | |
| 24 | Purchase pending | UPI and slow payments: "waiting for Google to confirm" | |
| 25 | Settings | Language, sound, vibration, number format, battery saver, Play Games, privacy options, help and complaints, version, reset | ✓ |
| 26 | Privacy policy | Link to the GitHub Pages policy (required inside the app, even when collecting nothing) | link ✓ |
| 27 | Credits & licences | Asset credits and open-source licence notices | ✓ |
| 28 | Reset progress | Double confirmation; purchases are kept | in #25 |
| 29 | Stats | Playtime, trips, earnings, cities, prestiges; testers screenshot it | ✓ |
| 30 | Help | Five one-line answers: offline cap, moving to a new phone, purchases | |
| 31 | Save restored | "Your save was damaged; restored from 10:42" | |
| 32 | Save from a newer version | "Update the game to load this save" | |
| 33 | Cloud save conflict | Keep this phone's progress or the cloud's? | |
| 34 | Update available | Google Play's in-app update prompt | |
| 35 | Event ended | Tapped an old event notification: "It ended; next is Deepavali in 9 days" | |
| 36 | Rate prompt | Google's in-app review card | |

**Where the standard list of easily-forgotten screens applies:**
- Permission priming: only for notifications (#4).
- Offline: the game *is* offline; each online feature gets its own fallback.
- Force update: Google Play's in-app update prompt (#34) plus a check for saves from newer versions (#32).
- "No longer exists": only for event notifications (#35).
- Session expired: not applicable, because there are no accounts.
- Empty states: livery library, events, achievements, garage filtered by city.
- Account deletion: not applicable; reset progress (#28) plus cloud-save deletion cover it.
- Restore purchases: in #22 and #25.
- Privacy policy inside the app: #26.
- App version: in #25.

## 5. Harder than it looks (details in RESEARCH.md)

1. **Tamil has to go in, not just out.** Typing needs Android's own input box (libGDX 1.14.1+ has a native input option). Shaped rendering needs Android's text engine. Length limits count letters, not code points. Cached text must be redrawn when the game resumes and Android has dropped its graphics memory.
2. **Offline earnings must equal what playing would have paid.** One pure-Java economy module serves the game, the simulator, the offline calculation and the tests, with a test that live and offline earnings agree within ~5%.
3. **Clock cheats go both ways.** Check elapsed time since boot within a single boot, keep the 4 h cap, and require a new local date *and* ≥20 h for daily rewards. Never punish honest cases.
4. **Saves get corrupted on cheap phones.** Write to a temp file then rename it, keep backups, and guard against NaN. Auto Backup restores the save on a new phone for free.
5. **Payments in India arrive late.** UPI purchases can be *pending*; acknowledge within 3 days or Google refunds them. Grants are idempotent per purchase token.
6. **Under-18s and ads** are restricted in India (DPDP, ~May 2027). See RESEARCH.md L-05.
7. **Notifications** need permission priming, and Indian OEM phones kill background work. Never rely on them.
8. **Events without app updates** need a built-in multi-year festival calendar plus an optional JSON file on GitHub Pages.
9. **Art decides the schedule.** Buses are layered masks from M1, and the 15 models are variations of 3–4 basic shapes.
10. **Platform rules:**
    - Target API 36.
    - 16 KB-aligned native libraries: current libGDX, fresh `android/libs`, verified by APK Analyzer.
    - Edge-to-edge display and predictive back gesture.
    - Play Games needs the app-signing key's fingerprint registered.
11. **Name boards on share cards:** keep sharing outside the app (no in-app gallery) and use a small local word filter.
12. **Release mechanics:**
    - 12 testers opted in *continuously* for 14 days; testers who drop out don't count.
    - Production review usually takes ≤7 days.
    - The package name is permanent from the first upload.

## 6. Decisions — DECIDED 2026-11-23 [F]

- **D1. Rush-hour clock → b. A sped-up game day.** One in-game day ≈ 20 real minutes, so every session sees a rush. Offline earnings use the day-average demand. The real calendar is used only for festivals and daily rewards.
- **D2. Look → a. Side view (cut-away diorama).** All art, the depot layout and the livery masks are built for this camera.
- **D3. Measurement → a. No analytics SDK in v1.**
  - Play Console's free statistics: installs, uninstalls, daily and 28-day active users, returning users, peer comparison, ratings, crash/ANR.
  - The in-game Stats screen and a 3-question tester form.
  - Firebase Analytics (free) arrives in the same update as ads, so the consent and privacy work happens once.

## 7. Success and non-goals

**Success — checked one month after public launch, all free in Play Console** [F: owner said "keep"]:
1. **They come back:** daily ÷ 28-day active users ≥ 15%, and at or above similar games in Play Console's peer comparison.
2. **They share it:** ≥ 10% of store-listing visitors arrive from share-card links tagged `utm_source=share`. This is the softest check, because Play fills the UTM field only for some traffic sources [Q].
3. **They like it and it's stable:** ≥ 4.3★ from ≥ 30 ratings; crash and ANR rates under Play's bad-behaviour thresholds (1.09% / 0.47%).

**"Worth it" gate** [A]: if all three pass, the owner invests more time (v1.1 money, then the rest of M2–M5). If they fail, fix the weakest one or stop. No money is spent either way.

**Non-goals** [A — proposed, not objected to]:
- Not a driving simulator.
- Not online: no accounts, chat, multiplayer or in-app gallery, so no server and no moderation.
- Not a copy of reality: no real operators, manufacturers, logos, film songs, deities or politics.
- Not pay-to-win or gambling: no loot boxes, spin wheels, or real-money mechanics.
- Not a transit-information app: real place names, never real timetables or fares.
- No map of India: Tamil Nadu only.
- Not a children's game: 13+, and never marketed to kids.
- Not on iOS for now.

## 8. The first version (v1) [F — accepted "proceed with recommended"]

**Goal:** learn whether Tamil players **come back** and **share their bus**.

**In** [A specifics]:
- Coimbatore depot with the full cycle: 10-second first payout, tap-to-dispatch, then a manager.
- **3 cities, 3 routes each:** Coimbatore → Tiruppur (mofussil) → Ooty (ghat bends). The other 9 cities are shown locked.
- 2 tiers (town, mofussil), 4 bus models, all 5 upgrades, 4 h offline earnings.
- **Prestige** — without it, content ends in a few days and the 7- and 28-day numbers mean nothing.
- **Livery v1:** palettes, 6 patterns (2 kolam-style), Tamil or English name board, fictional plate, apply to one bus or all; one original horn.
- Share card with the game's name and a tagged Play link.
- **U-1:** a "paint your bus" moment in the first minute (6 colours + company name on the board), and a "Share your bus" prompt after the first city unlock.
- English and Tamil; save system with backups; Auto Backup; Stats screen.
- Behind the scenes: Tamil text spike, economy module, simulator, unit tests.

**Out:** ads, purchases, Play Games, cloud save, notifications, events, daily rewards and tasks, livery library, 9 cities, express/sleeper tiers, horn picker.

**15 screens:** splash, language picker, depot, routes panel, upgrades panel, garage, map, welcome back, livery editor, name entry, share preview, prestige, settings (with reset confirmation), stats, credits and licences. Plus the privacy-policy link.

**Build order to v1** [A]: M0 → M1 → M2-lite (3 cities, 2 tiers, 4 models) → M3-lite (livery v1 + share card) → prestige from M4 → M6-lite (privacy policy, Data safety, listing) → closed test → production.
**After v1** [F, P-5]:
- **v1.1 = ads only**: **rewarded ads only, no interstitials (U-3)**. Shown only to players who declare 18+ on a neutral age screen, which runs before any ad or analytics code starts. Under-18s play ad-free with no analytics.
- **v1.2 = in-app purchases**, after legal advice. From then your home address shows on the store page; you accepted that on 2026-11-24 (RESEARCH.md L-12).
- Then the rest of M2–M5.

**Timing** [A]: about 6–8 weeks of building, then 14 days of closed test, then ~1 week of production review. Owner's time is not a constraint [F]. Tamil New Year (mid-April 2027) is a realistic launch moment.

## 9. The owner's bar (answers from 2026-11-23)

- **What I want back:** [F] "need money and users", both.
  - [Q] How much money, by when? How many users?
  - [A] Order: users first (v1 proves people return and share), then money (v1.1 soon after v1 passes the gate).
- **What I can put in:**
  - [F] "Ready to put time, if it's worth it"; "time no issue" (2026-11-23).
  - [F] **No more money spent without returns.**
  - [F] **Home address public from v1.2** ("okay to give address", 2026-11-24). It's the free way to sell in-app purchases: no organisation account needed (RESEARCH.md L-12).
  - [A] "Worth it" is defined by the §7 gate.
- **What I already have:**
  - [F] A **personal** Google Play developer account. [Q] Creation date; [A] after 13 Nov 2023, so the 12-tester rule applies.
  - [F] **14 testers.** Recruit ~20 (P-13), because anyone who drops out mid-test doesn't count.
  - [F] A **2–3 GB RAM test phone**.
  - [F] Public GitHub repo `rameshkumark24/Bus-Captain`.
  - [Q] Skills: Java, drawing, who writes and reviews the Tamil copy.
  - [Q] The old `App` repo for reuse; [A] build fresh unless it's provided.
- **Where I sell first:** [F] the Google Play Store, **India only at launch** (P-1). Prices in ₹.
- **What it may cost:**
  - [F] Only free tools and free tiers.
  - [F] The Play account already exists, so the $25 is paid.
  - [A] No iOS, so no $99/yr.
  - Any paid item needs the owner's yes, with its price and the free alternative.

---

## Rules adopted from research and the USP — APPROVED by the owner [F]

These are product rules now. P-items (approved 2026-11-23) point to findings in RESEARCH.md; U-items (approved 2026-11-24) point to the USP section of COMPETITORS.md. Later steps must follow them.

| ID | Rule (USP, approved 2026-11-24) | Why (COMPETITORS.md "USP") |
|---|---|---|
| U-1 | A "paint your bus" moment in the first minute (6 colours + company name on the board); a "Share your bus" prompt after the first city unlock | Makes the USP visible within 60 seconds |
| U-2 | Store page: icon and screenshots 1–2 show a player-painted bus at a Tamil-signed bus stand; first line "Run your own Tamil Nadu bus company" (Tamil and English); never name other apps | The USP has to sell before install |
| U-3 | No interstitial ads, ever: rewarded ads only, opt-in, never blocking progress | Turns competitors' #1 complaint into our promise |
| U-4 | Answer every Play review within 48 hours | Big competitors answer almost none |
| U-5 | A native-speaker check of every Tamil string and sign is a release gate | One "Hindi signs in Coimbatore" moment kills the USP |
| U-6 | The closed test asks every tester: "How would you describe this game to a friend?" | Proof the USP is noticed |

| ID | Rule | Why (RESEARCH.md) |
|---|---|---|
| P-1 | Distribute in **India only** at launch (v1 and v1.1); add any other country only after checking its laws | L-02, L-03 |
| P-2 | Settings gets a **"Help & complaints"** row: support email, named grievance contact, "reply within 48 h, resolve within 30 days". Still 15 screens | L-10, L-11 |
| P-3 | Use a **dedicated free support email**, not a personal one | L-11 |
| P-4 | Auto Backup only with client-side encryption (`requireFlags="clientSideEncryption"`), so the Data safety form can truthfully say "no data collected" | L-06 |
| P-5 | **Money in two steps.** v1.1 = ads only (rewarded first; interstitial rules as in M5), shown only to players who declare 18+ on a neutral age screen *before* any SDK starts; under-18s play ad-free with no analytics. v1.2 = in-app purchases, after advice. They make the owner's home address public, which the owner accepted on 2026-11-24 | L-05, L-12, L-16 |
| P-6 | **Never sell coins.** The starter pack gives cosmetics and a permanent time boost, not coins. Nothing bought can be risked for a chance at more: no spins, bets, mystery boxes or double-or-nothing | L-09, L-10 |
| P-7 | Offer and ad UI rules: no fake countdown timers, no guilt-trip "No thanks" text, no repeated nagging; "No thanks" is as visible as "Yes" | L-09 |
| P-8 | Package name `io.github.rameshkumark24.busgame` (neutral). Play Store search done 2026-11-23 (no game by that name). **IP India trademark search still to do before the first upload** | L-14 |
| P-9 | Marketing rule: no references to Vijayakanth ("Captain"), his films, slogans or DMDK colours or symbols | L-14 |
| P-10 | LATER livery codes carry colours and pattern IDs only, never free text, to stay outside Play's user-generated-content rules | L-08 |
| P-11 | Draw the Tamil Nadu outline from Natural Earth (public domain), not OpenStreetMap or Google Maps | L-13 |
| P-12 | Credits & licences screen carries the libGDX Apache-2.0 licence and NOTICE, the FreeType credit line (if gdx-freetype is used) and the OFL text. No GPL/AGPL dependencies, ever | L-13 |
| P-13 | Recruit ~20 testers (6 more than the 14 you have) and plan how to keep them playing for the full 14 days | X-01 |
| P-14 | v1.1: block gambling and real-money-game ad categories in AdMob | L-10 |

---

## Business verdict

> Worked out 2026-11-24 from this file, [RESEARCH.md](RESEARCH.md) and [COMPETITORS.md](COMPETITORS.md), including its USP section.
>
> **Labels:**
> - **SEEN** — a price, fee or number I opened, linked, with the date;
> - **DERIVED** — worked out from seen numbers, with the formula shown;
> - **ASSUMED** — a guess, with what would confirm it.
>
> Money is in ₹. **$1 ≈ ₹93.5** [ASSUMED: mid-market rate of 18 Nov 2026 per [therighttrader](https://therighttrader.com/convert/1-usd-to-inr); confirm on xe.com]. **No price is put on your hours**; your bar gives none.

**Verdict (it rests on ASSUMED reach numbers): DO IT — the first version is the test.**
- The likely case earns **about ₹3,900 a month at month 12**, which is 39% of your ₹10,000 bar.
- Reaching the bar needs **about 260 organic installs a day**. That's RARE for a zero-budget idle game; the likely case gets about 100.
- But the first version costs **₹0 in cash** and **about 12 weeks** at your 10 hours a week, and it's the only thing that can measure that number. Build it, with the stop rule in §8.

### The bar and the first version
- **What I want back** [F, 2026-11-24]: **₹10,000 a month by month 12 after public launch.** Money decides; there's no separate players target.
- **What I can put in** [F]: up to **10 hours a week** of review and testing. No new money without returns.
- **What I already have** [F]: a personal Play account (fee already paid), 14 testers, a 2–3 GB test phone, a public repo.
- **Where I sell first** [F]: Google Play, **India only** (P-1).
- **The first version:** §8 of this file (15 screens; Coimbatore → Tiruppur → Ooty; livery v1 + share card; prestige; no ads, no purchases). **Money only starts with v1.1** (rewarded ads, players 18+ only) **and v1.2** (in-app purchases).

### 1. Who pays

| Payer | Pays for | What they pay today | A price they'd accept |
|---|---|---|---|
| **Advertisers, through AdMob** | Opt-in rewarded ad views by players aged 18+ (P-5, U-3) | India rewarded-video rates of **$0.70–$4.00 per 1,000 views** are reported by ad vendors ([vmobify](https://vmobify.com/blog/casual-game-marketing-strategy), [getkoro](https://getkoro.app/blog/how-much-do-rewarded-video-ads-pay-for-indian-d2c-brands-e-commerce-sellers-2026), [cas.ai](https://cas.ai/?p=11066)). Used here: **$1.50** [ASSUMED — confirm with your AdMob Benchmark report once the account exists: **owner task**] | — |
| **Players** (any age; Google Play handles payment and parental controls) | Cosmetics: pattern packs **₹49**, starter pack **₹79** (no coins, P-6), "skip the ad" pass **₹99** | Per-item price ranges on Play India [SEEN 2026-11-24]: Bus Sim Kerala ₹20–₹80 · City Transportation Tycoon ₹20–₹500 · Bus Masters: India ₹99–₹1,699 · Terminal Master ₹200–₹2,000 · Bus Livery Tamilnadu ₹9–₹16,700. Terminal Master's remove-ads, quoted in a review at "$9.9…" ≈ **₹925** [DERIVED: $9.90 × 93.5] | **₹49–₹99 per item**: the low end of what Indian bus-game players pay today, inside Bus Sim Kerala's ₹20–₹80 band [DERIVED]. That they'd accept it is ASSUMED; v1.2's conversion confirms it |

Under-18 players see no ads, so any money from them comes through purchases, often on a parent's payment method [ASSUMED].

### 2. The reachable market — bottom-up, year one (India, Google Play only)

| Channel | Year-one installs | Basis |
|---|---|---|
| Testers' own circles at launch | ~200 | ASSUMED: 20 testers (P-13) × ~10 installs each. Confirm at launch |
| **Play Store search for Tamil bus terms** | **~36,500 (≈100 a day)** | ASSUMED, anchored on the pull of the most popular Tamil bus-livery app (Tamil Bus Mods & Liverys) — see below |
| Share cards (U-1) | +11% | ASSUMED: each install brings 0.1 more. Success check 2 measures it |
| **Likely year-one total** | **≈ 40,700 installs** | DERIVED: (36,500 + 200) × 1.11 |
| Worst / best | **~2,000 / ~260,000** | Worst: 5 a day from search, 0.03 shares per install. That's like the idle bus tycoons nobody found: Idle Bus Station Tycoon has **181** lifetime installs and Idle Transit **38** [SEEN 2026-11-23, Play page data]. Best: 500 a day, 0.3 shares per install — a quarter of Bus Sim Kerala's lifetime average of ≈2,120 a day [DERIVED: 3,949,411 installs ÷ 1,862 days since its first review in Oct 2021] |

**How the "100 a day" anchor was built:**
- Tamil Bus Mods & Liverys has **567,940** installs [SEEN 2026-11-24]. That's **≈245 a day** over its life [DERIVED: ÷ 2,316 days since its first review, 22 Jul 2020].
- Its *current* pull is uncertain, somewhere between:
  - **≈40 a day**: only 8 of its 386 reviews are dated 2026 [DERIVED: 567,940 × 8/386 ÷ 328 days];
  - **≈1,000 a day**: its install number rose by 1,089 between my 23 and 24 Nov readings [SEEN both]. Google's refresh interval isn't known, so this may cover several days.
- We take ~40% of the middle (245 a day), because those searchers want sim content and we're a tycoon [ASSUMED].

Not counted: Instagram and YouTube fan pages (sizes NOT VERIFIED) and Play featuring.

### 3. The cost of the first version

**To build:** 18 working slices × 2 sessions each (I build, then you review and test) = **36 sessions** [DERIVED]. The slices:
1. Project setup
2. Tamil text spike
3. Economy core + unit tests
4. Balance simulator
5. Depot scene (masked bus art)
6. Bus cycle + payout feel
7. Routes, upgrades, manager
8. Save, offline earnings, clock checks
9. Garage (4 models)
10. Map + 3 cities (incl. Ooty ghat)
11. Prestige
12. Livery editor + Tamil name entry
13. First-minute paint (U-1) + share card
14. English/Tamil strings + native check (U-5)
15. Settings, help & complaints, credits, stats
16. Sound, conductor calls, low-end phone pass
17. Release prep (policy page, Data safety, listing with U-2, signing)
18. Closed-test fixes

Time:
- Your time is about 2.5 hours per session [ASSUMED: testing on the phone plus written feedback; confirm after M0]. 36 × 2.5 h = 90 h, so **~9 weeks at 10 h a week** [DERIVED].
- Waits nobody can shorten: **14 days of continuous closed testing**, then a production review that "usually takes seven days or less" [SEEN 2026-11-23, [Play testing requirements](https://support.google.com/googleplay/android-developer/answer/14151465)]. That adds **~3 weeks**.
- **About 12 weeks to public launch** — roughly **late Feb 2027** if work starts on 1 Dec 2026 [DERIVED; start date ASSUMED].

**Cash before the first user: ₹0.** Each item:

| Item | Needed to launch? | Cost |
|---|---|---|
| Google Play developer account | Yes | Already paid [F] → **₹0** |
| A privacy policy at a public URL, linked in the app | Yes — required even for apps that collect nothing ([Play User Data policy](https://support.google.com/googleplay/android-developer/answer/10144311)) | GitHub Pages is free ([limits](https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits)) → **₹0** |
| Server, APIs, domain | No — offline game, no backend | **₹0** |
| Art, fonts, sounds | Yes | CC0 / OFL / made in-house → **₹0** |
| Test phone, testers | Yes | Owner has both [F] → **₹0** |
| Lawyer | **No.** No rule makes v1 need one: v1 collects no data and sells nothing (RESEARCH.md L-16) | **₹0** |
| Trademark search | Yes, before the first upload (P-8) | IP India's public search is free → **₹0** |

**Later, if it grows:**
- Legal advice before v1.1 (DPDP, age gating, ads) and before v1.2 (e-commerce terms). Price NOT VERIFIED; ASSUMED ₹5,000–₹15,000 per consultation — get two or three quotes.
- A chartered accountant once money arrives: ASSUMED ₹2,000–₹5,000 a year.
- A trademark filing, optional: fee NOT VERIFIED here.
- An organisation account: **not needed**. You accepted showing your home address from v1.2 (2026-11-24). If you change your mind, it costs about ₹9,500–₹22,000 in the first year (RESEARCH.md L-12).

**To run each month:**
- **₹0 fixed**: no hosting, no paid APIs.
- On purchases, Google keeps **15%** of the first $1M a year once you've enrolled in the 15% tier. There's **no separate billing fee for India** (that "+5% billing fee" appears only in the AU/EEA/JP/UK/US table) [SEEN 2026-11-24, [service fees](https://support.google.com/googleplay/android-developer/answer/112622)]. Google may change this rate for India later; no date is given.
- Google also withholds **0.1%** of each purchase price (excluding GST) when your PAN is on file, or 5% without it [SEEN 2026-11-23, [withholding tax](https://support.google.com/googleplay/android-developer/answer/9384608)].
- Ad income is reported after Google's share [ASSUMED].

**To win one paying user:**
- **₹0 in cash** through organic search and share cards.
- Paid ads lose money:
  - An install costs about **₹7.50–₹19** [ASSUMED from reported India casual-game install costs of $0.08–$0.20: [Playio](https://blog.playio.co/mobile-game-cpi-benchmarks-2026), [vmobify](https://vmobify.com/blog/app-install-cost-india)].
  - An install earns about **₹1.16** in the likely case [DERIVED: ₹0.29 a day × 4 active days], a loss of ₹6–₹18 per install.
  - Bought *paying* users would cost **₹1,500–₹3,700 each** [DERIVED: install cost ÷ an ASSUMED 0.5% of installs who ever pay], against a value of about ₹84–₹168 each.
  - **No paid ads.**

### 4. The ROI math — at month 12 after public launch, against the ₹10,000/month bar

| | Worst | **Likely** | Best | Label |
|---|---|---|---|---|
| Installs a month (steady state, including shares) | 165 | **3,330** | 21,450 | DERIVED from §2 ÷ 12 |
| Active days per install over its life | 2.5 | **4** | 8 | ASSUMED, anchored on GameAnalytics 2026: median day-30 retention ≈0.7%, top 10% ≈4% ([summary](https://investgame.net/wp-content/uploads/2026/01/2026-01-27-2026-mobile-pc-benchmarks_compressed.pdf)). "Likely" implies about 1.5% at day 30, above the median, on the bet that idle games retain better. v1's day-30 data confirms it |
| **Daily players at month 12** | 14 | **444** | 5,720 | DERIVED: installs a month × active days ÷ 30 |
| Share of players aged 18+ (who may see ads) | 35% | 50% | 60% | ASSUMED; v1.1's age screen measures it |
| Rewarded views per adult player per day | 1 | 2 | 3 | ASSUMED; confirmed by AdMob data |
| Ad rate per 1,000 views | $0.80 | **$1.50** | $3.00 | ASSUMED, from the reported $0.70–$4.00 (§1) |
| Ad income per daily player per day | ₹0.03 | **₹0.14** | ₹0.50 | DERIVED: share × views × rate ÷ 1,000 × 93.5 |
| Players who buy in a month (of monthly players) · daily ÷ monthly players | 0.3% · 10% | **0.8% · 15%** | 1.5% · 20% | ASSUMED; v1.2 measures it. 15% is our own success check |
| Purchase income per daily player per day | ₹0.08 | **₹0.15** | ₹0.21 | DERIVED: (1 ÷ daily/monthly) × buyer share × ₹84 net (₹99 − 15%) ÷ 30 |
| **Income per daily player per day** | ₹0.11 | **₹0.29** | ₹0.71 | DERIVED |
| **Income a month at month 12** | **≈ ₹50** | **≈ ₹3,900** | **≈ ₹1,22,000** | DERIVED: daily players × 30 × income per player per day |
| Against the ₹10,000 bar | misses | **misses (39%)** | beats (12×) | DERIVED |

- **Paying users needed to cover running costs: 0**, because running costs are ₹0.
- **To reach the bar:** about **119 paying users a month** if purchases alone had to do it [DERIVED: ₹10,000 ÷ ₹84], or **≈1,150 daily players** with the likely ads-plus-purchases mix [DERIVED: ₹10,000 ÷ (30 × ₹0.29)].
- **Months to break even:**
  - **Month 0 in cash**, because nothing is spent.
  - If you later pay ~₹10,000 for legal advice before v1.1 [ASSUMED price], that's earned back in about **3 months** of ads in the likely case, under a month in the best, and **never** in the worst. **So buy advice only after the stop rule has passed.**
- **Months to reach the bar:**
  - best: about **month 3–4** after public launch (ads start around month 2);
  - likely: **not within 12 months** — month 12 earns ₹3,900, 39% of the bar;
  - worst: never [DERIVED].

### 5. The one number that decides it

**Organic installs per day.**
- It swings the result most: from 5 to 500 a day (×100). Compare income per player (₹0.11–₹0.71, ×6.5) and active days per install (2.5–8, ×3.2).
- **Value at which the likely case reaches the bar: ≈260 a day** [DERIVED: 1,150 daily players × 30 ÷ 4 active days = 8,620 installs a month, ÷ 1.11 for shares ÷ 30 days]. The likely case assumes 100.

**RARE** for this kind of app:
- Zero-budget idle bus tycoons we can see sit at **181** and **38** installs in their whole lives [SEEN 2026-11-23].
- 260 a day is about what **the most popular Tamil bus-livery app has averaged for six years** (≈245 a day [DERIVED]).

It would be *ordinary* only for an app that already has Tamil Nadu bus fans' attention. That's exactly what v1 tests.

### 6. What kills it — none of these is fatal on its own

1. **Nobody finds it** (RESEARCH.md R-01; COMPETITORS.md §6).
   - No budget, and paid installs lose money (§3).
   - Not fatal: share cards and the testers' Tamil Nadu bus-fan circles are the way through. It's a cost every small competitor lives with.
2. **Wrong format for the fans** (COMPETITORS.md §2, §8).
   - Indian bus fans search "bus simulator" (61) far more than "idle game" or "tycoon game" (1–2).
   - Requests for Tamil Nadu content are about 1% of review text.
   - Not fatal: v1 tests it, and the one-week check in §7 gives early warning.
3. **Thin money by design.**
   - Rewarded ads only (U-3), adults only (P-5), and India's ad rates leave ≈₹0.29 per daily player per day.
   - Even a loved game needs ≈1,150 daily players for ₹10,000.
   - Not fatal: the levers are purchases and, later, more countries (only after a legal check, P-1). **Relaxing U-3 or P-5 is not a lever.**

RESEARCH.md found nothing that stops it outright.

### 7. The cheapest test

**The first version is the test** of the number in §5. It costs ₹0 in cash, and nothing cheaper measures installs per day from Google Play.

A **one-week early warning**, run alongside M0 so it doesn't delay the build:
1. Once M0 produces the first side-view bus, make three static "my bus" cards: one bus in three player-made liveries, saying "Bus Captain — run your own Tamil Nadu bus company". The 20 testers post them in Tamil Nadu bus-fan WhatsApp groups and Instagram stories. Count people *outside the testers' own circles* who ask to join the test.
2. Hold **ten conversations** with Tamil Nadu bus fans: "Would you play a game where you *run* the bus company instead of driving?"

**Go:** at least **30 outsiders** ask to join within 7 days, and at least **6 of 10** fans say yes and name what they'd want. **If both fail**, rethink the format before M3; don't stop the build on this alone.

### 8. The verdict

**DO IT — it rests on ASSUMED reach numbers; the first version is the test.**

Why:
- **It's not DROP IT:** there's no fatal finding, and the best case beats the bar 12×.
- **It's not a plain DO IT:** the likely case reaches only 39% of the bar, and getting there needs a RARE ~260 organic installs a day. By the rules that's TEST FIRST.
- **The only test that measures that number is the first version itself.** It fits what you can put in (₹0 cash, about 12 weeks at 10 h a week). So the verdict is DO IT, with this stop rule.

**STOP RULE:** on **day 90 after public launch** (about late May 2027 if launch is late Feb 2027), look at Play Console. **If new installs average under 100 a day over the last 30 days, stop:**
- no paid legal advice;
- no v1.1 or v1.2 work;
- the game stays up at ₹0 a month.

Otherwise continue to ads (v1.1), and check the bar itself at **month 12** (about Feb 2028).

**What would move the likely case to the bar:** any one of —
- installs ×2.6 (share cards working, or a fan-page creator picking it up);
- retention ×2.6;
- income per player ×2.6 (more purchases — not more ads).

SCORE — first version: ~12 weeks (9 build + 3 store wait) · likely at month 12: ~₹3,900/month (bar ₹10,000) · needs: organic installs ≈260/day (likely ~100) · DO IT — v1 is the test; stop if under 100 installs/day on day 90
