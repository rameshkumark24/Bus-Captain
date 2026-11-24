# Bus Captain — Research: risks, legal & compliance, store rejection

> Researched **2026-11-23** against [IDEA.md](IDEA.md). L-12 decided and updated 2026-11-24. Competitors are deliberately excluded; they're the next step.
> **Not legal advice.** Neither the owner nor the author is a lawyer. §2.16 separates "get advice" items from "standard practice" and says **when** each is needed.

## How to read this

- **Tiers:**
  - **T1:** the rule itself (law or regulator text, a store's current policy page, official provider docs).
  - **T2:** a reputable summary (law firm, the vendor's own blog or maintainers).
  - **T3:** what people report.
  - **T4:** the author's memory ("from memory, not checked").
- **Findings that change the design** carry T1 or T2. Where only T3/T4 exists, the finding is marked **UNVERIFIED**, with what would confirm it.
- **Source IDs** (S01…) point to the table in §4, which gives each link, its date, its tier and how it was checked. "Search excerpt" means the page was read through search-result extracts rather than opened in full.
- **Labels:** [A] = assumption, [Q] = open question, as in IDEA.md.

## Facts this research rests on

Where the owner hasn't said, these are the author's most likely case:

| Fact | Value used | Status |
|---|---|---|
| Country sold in first | India | [A] Owner said "Play Store"; country not stated |
| Who sells | An individual developer resident in India, no company | [A] |
| Play account | Personal [F]; created after 13 Nov 2023 | Date [A], implied by the closed-test plan |
| v1 | No ads, no purchases, no analytics, no accounts, no network calls | IDEA.md §8 |
| v1.1 / v1.2 | Ads (AdMob) + Firebase Analytics / in-app purchases via Play Billing | IDEA.md §8 and Pending change P-5 |
| Distribution countries | India only at launch | [F] Approved 2026-11-23 (P-1) |

## 0. What this research changes (summary)

**Nothing found stops the product.** No risk is classed STOPS IT. The changes it forces are P-1…P-14 at the end of IDEA.md, **approved by the owner on 2026-11-23**. That does not mean "no legal work": the advice items in §2.16 remain, before ads (v1.1) and before purchases (v1.2). The five that matter most:

1. **Ads go only to players who say they are 18+**, on a neutral age screen shown before any ad or analytics code starts. Under-18s play ad-free. Reason: India's DPDP law treats under-18s as children, and Play tells developers to follow local child-age laws. (L-05)
2. **Money comes in two steps: ads first, purchases later.** Selling anything through Play makes your full legal address public on the store page; ads alone don't. The owner accepted this on 2026-11-24, so purchases (v1.2) will show the home address. (L-12)
3. **Never sell coins, and never let anything bought be risked for a chance at more.** India's 2025 gaming law counts purchased virtual coins as "stakes"; offering a money game is a crime. (L-10)
4. **Have a complaints route from v1.** India's 2026 gaming rules already require one for social games, and the e-commerce rules require one once you sell. It is one Settings row plus a support email. (L-10, L-11)
5. **Launch in India only.** This avoids GDPR, UK GDPR, COPPA and every other country's rules until revenue justifies checking them. (L-02, L-03)

---

## 1. Risk register

**Scoring:** Likelihood (L) and Damage (D), each 1–5; ranked by L×D.
**Class:**
- **STOPS IT:** no legal or practical way through.
- **COSTS:** a known way through, and it says what it takes.
- **ROUTINE:** every app of this kind lives with it.

**Design?** = does it change what we build.

| # | ID | Risk — and why it applies here | Type | L | D | L×D | Class | Design? | What it forces | Sources |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | R-01 | **Nobody finds it.** No marketing budget; "bus tycoon" keywords are crowded by studio games | Adoption | 5 | 5 | 25 | COSTS | Yes | Share card in v1 carries name + tagged Play link; Tamil-language listing; seed through testers and Tamil Nadu bus-fan communities. Competitor step will size it | T4 — UNVERIFIED; confirmed by success check 2 and the competitor step |
| 2 | R-02 | **Tamil renders wrong.** Tamil needs a shaping engine; libGDX's font path doesn't shape | Technical | 5 | 5 | 25 | COSTS | Yes | Draw Tamil with Android's own text engine into cached textures (platform interface); Java2D on desktop. M0 spike decides. Fallback: Matn (HarfBuzz for libGDX, Apache-2.0, very new) | S36 T1 (shaping needed); S37 T3 (libGDX gap) — libGDX part UNVERIFIED until the M0 spike |
| 3 | R-03 | **Under-18 players vs ads.** Tamil Nadu bus fans skew young; DPDP children's rules apply from ~May 2027; Play's local-law clause | Dependency (law) | 5 | 5 | 25 | COSTS | Yes | P-5: age screen before any SDK starts; ads and analytics only for 18+ | S25 T1, S27 T2, S02 T1, S03 T1 |
| 4 | R-04 | **Players quit on day 1.** Idle-genre retention is low; 75% of games keep under 3% of players by day 28 | Adoption | 4 | 5 | 20 | COSTS | Yes | 10-second first payout; simulator targets; prestige in v1 | S46 T2 |
| 5 | R-05 | **Closed-test gate fails.** 12 testers must be opted in continuously for 14 days, and engagement is judged; the owner has 14 | Dependency | 4 | 4 | 16 | COSTS | Yes | P-13: recruit ~20; daily reasons to open the game; budget ~3 weeks | S01 T1 |
| 6 | R-06 | **Ad revenue lower than hoped.** India's ad rates are low, and ads go to 18+ only | Dependency | 4 | 4 | 16 | COSTS | Yes | Ads-first sequencing; cosmetics as the second stream; decide from real data | T4 — UNVERIFIED; confirm with v1.1 AdMob numbers |
| 7 | R-07 | **The share card isn't shared** | Adoption | 4 | 4 | 16 | COSTS | No | Already in v1; measured by success check 2 | T4 |
| 8 | R-08 | **Bus art not layered from M1**, forcing a redraw of every bus at M3 | Technical | 4 | 4 | 16 | COSTS | Yes | Build buses from greyscale masks from M1; 15 models = variations of 3–4 shapes | T4 — UNVERIFIED; confirmed when M3 needs no redraw |
| 9 | R-09 | **Home address published once you sell anything** (personal account) | Dependency | 5 | 3 | 15 | COSTS | Yes | P-5: ads before purchases. **Decided 2026-11-24: owner accepts the public address from v1.2** (L-12) | S10 T1 |
| 10 | R-10 | **Save corrupted** when a low-RAM phone kills the app mid-write | Technical | 3 | 5 | 15 | COSTS | Yes | Write to a temp file then rename; 2–3 rotating backups; checksum to detect damage | T4 — UNVERIFIED; confirm with a kill-during-save test in M1 |
| 11 | R-11 | **NaN or overflow poisons the economy forever** | Technical | 3 | 5 | 15 | COSTS | Yes | Saturating maths, NaN guards on every write, unit tests | T4; confirmed by unit tests |
| 12 | R-12 | **Android 16 behaviour.** Edge-to-edge can't be opted out; back key not delivered unless migrated | Technical | 5 | 3 | 15 | COSTS | Yes | Handle insets; back via `OnBackInvokedCallback` (or the temporary opt-out); use libGDX ≥ 1.14.1 | S18 T1, S34 T1 |
| 13 | R-13 | **AdMob account disabled for invalid traffic** (testers tapping real ads) | Dependency | 3 | 5 | 15 | COSTS | Yes | Test ad units / registered test devices only; never ask anyone to click ads | S23 T1 |
| 14 | R-14 | **Play's yearly requirement changes** (target API, billing library, forms) | Dependency | 5 | 3 | 15 | ROUTINE | No | A maintenance window each July–August | S16 T1 |
| 15 | R-15 | **Offline earnings ≠ what live play would pay** | Technical | 3 | 4 | 12 | COSTS | Yes | One shared pure-Java economy module; test that live and offline agree within ~5% | T4 — UNVERIFIED; confirmed by the M1 test |
| 16 | R-16 | **Support load:** lost progress, pending UPI purchases, refunds; plus 48-hour acknowledgement duty | Operational | 4 | 3 | 12 | COSTS | Yes | P-2/P-3 complaints route; clear "pending" state; Auto Backup; Help answers | S20 T1, S30 T1 |
| 17 | R-17 | **Content runs out** in v1's 3 cities | Adoption | 3 | 4 | 12 | COSTS | Yes | Prestige in v1 (already planned) | T4 |
| 18 | R-18 | **Tamil typing** (keyboard composition) breaks in a game text field | Technical | 3 | 4 | 12 | COSTS | Yes | libGDX 1.14.1 added a native input option (`NativeOnscreenKeyboard`) — use it, then render via R-02's path | S34 T1 |
| 19 | R-19 | **Slow or large on 2–3 GB phones** | Technical | 3 | 4 | 12 | COSTS | No | Test on a cheap phone [Q: owner has one?]; render-on-demand; texture atlas | T4 |
| 20 | R-20 | **Upload blocked:** native libraries not 16 KB-aligned | Technical | 2 | 5 | 10 | ROUTINE | No | Current libGDX (≥ 1.13.5; latest 1.14.2), fresh `android/libs`, Android Studio ≥ 2025.1.1; check with APK Analyzer / `zipalign -P 16` | S17 T1, S35 T2 |
| 21 | R-21 | **Coins become "stakes".** Under India's 2025 gaming law, purchased coins count as stakes; any wager-like mechanic risks the money-game ban | Dependency (law) | 2 | 5 | 10 | COSTS | Yes | P-6: never sell coins; no paid or random wagering | S28 T1 |
| 22 | R-22 | **Clock cheating** for offline earnings and daily rewards | Technical | 5 | 2 | 10 | ROUTINE | No | Boot-time checks, the 4 h cap, ≥20 h + new date for dailies | T4 |
| 23 | R-23 | **Complaint-handling duties** (gaming Rule 20, e-commerce Rule 6, DPDP) | Operational | 5 | 2 | 10 | ROUTINE | Yes | P-2 Settings row + written response process | S29 T1, S30 T1, S25 T1 |
| 24 | R-24 | **Tax and accounting once money arrives** (GST is yours to assess; Google withholds tax; PAN) | Operational | 5 | 2 | 10 | COSTS | No | Add PAN to the payments profile; see a CA when revenue starts | S09 T1, S47 T1 |
| 25 | R-25 | **Cached Tamil textures lost** when Android drops the graphics context on resume | Technical | 3 | 3 | 9 | COSTS | No | Re-render on resume | T4 — UNVERIFIED; confirm with an M0 background/resume test |
| 26 | R-26 | **Tamil copy reads machine-translated** | Adoption | 3 | 3 | 9 | COSTS | No | A native speaker writes or reviews [Q: who?] | T4 |
| 27 | R-27 | **Backlash when ads arrive** | Adoption | 3 | 3 | 9 | ROUTINE | No | Rewarded first; interstitial rules from M5 | T4 |
| 28 | R-28 | **"Bus Captain" clashes** with an existing app or trademark. Play India has no game by that name; "Bus Captain-Yolobus" is a bus-staff app (1K+); trademark not searched | Dependency (law) | 2 | 4 | 8 | COSTS | Yes | P-8: neutral package name (done); **IP India search before the first upload** (owner) | S48 (seen); trademark part UNVERIFIED |
| 29 | R-29 | **Notifications don't fire** on Indian OEM phones | Technical | 4 | 2 | 8 | ROUTINE | No | Not in v1; never rely on them | T3/T4 — UNVERIFIED |
| 30 | R-30 | **Losing the upload/signing key** | Operational | 2 | 4 | 8 | ROUTINE | No | Play App Signing; back up the upload keystore offline | T4 |
| 31 | R-31 | **Offensive name boards** circulating with the game's name on them | Operational | 3 | 2 | 6 | ROUTINE | Minor | Small local Tamil/English/Tanglish word filter | T4 |
| 32 | R-32 | **"Captain" reads as Vijayakanth/DMDK** to some Tamil Nadu players. Owner's meaning is "captain = driver", as in Rapido Captain (S50) | Adoption | 2 | 2 | 4 | ROUTINE | No | P-9 marketing rule | S44 T2, S50 |
| 33 | R-33 | **Government later requires registration** for our category of social games | Dependency | 2 | 3 | 6 | ROUTINE | No | Check MeitY/Authority notices each quarter | S29 T1 |
| 34 | R-34 | **Bus fans want real operators and film-tune horns** | Adoption | 3 | 2 | 6 | ROUTINE | No | Legal necessity; win on ease and originality | T4 |
| 35 | R-35 | **libGDX stops being maintained** | Dependency | 1 | 5 | 5 | ROUTINE | No | Releases 1.14.0–1.14.2 in Oct 2025–Jun 2026; keep updated | S34 T1 |
| 36 | R-36 | **Pirated cosmetics** via modded APKs | Operational | 4 | 1 | 4 | ROUTINE | No | Accept; nothing important is pay-only | T4 |
| 37 | R-37 | **GitHub Pages goes away or changes terms**, breaking the privacy-policy URL | Dependency | 1 | 4 | 4 | ROUTINE | No | Static policy is easy to rehost; check the link each release | S43 T1 |
| 38 | R-38 | **Cost at scale** | Operational | 1 | 2 | 2 | ROUTINE | No | No server. GitHub Pages has a 100 GB/month soft limit; events JSON is tiny | S43 T1 |

**STOPS IT: none.**
- The two candidates were "libGDX can't ship to Play" (16 KB pages) and "ads can't be shown in India". Both turned out to have a known way through: R-20 and R-03.

---

## 2. Legal and compliance picture

Every finding uses the same format: **What** · **Why here** · **Forces** · **Sources** · **Advice?**

### L-01 India — personal data law (DPDP Act 2023 + DPDP Rules 2025)
- **What:**
  - The Rules were notified on 13/14 Nov 2025 (sources differ; the government's own backgrounder says 14 Nov). They give **18 months for phased compliance**, so most duties bite around **13–14 May 2027** (T2 dates; news reports say the government may shorten this — T3, UNVERIFIED).
  - Duties include:
    - a separate, clear consent notice;
    - consent that is clear and can be withdrawn;
    - visible contact details for data questions;
    - answers to access, correction and erasure requests **within 90 days**;
    - breach notification;
    - verifiable parental consent for children.
  - Penalties go up to **₹250 crore** (security failures), **₹200 crore** (children's obligations, breach notification) and **₹50 crore** (anything else).
- **Why here:**
  - **v1** sends no personal data anywhere. Company name, saves and stats stay on the phone; Auto Backup goes end-to-end encrypted to the player's own Google account (L-06). [A] There is nothing for the developer to "process", so exposure is minimal.
  - **v1.1** sends IP address, device IDs and interactions to Google through AdMob and Firebase, for purposes the developer chooses. [A] That makes the developer a Data Fiduciary for India-based players.
- **Forces:**
  - v1: nothing beyond an honest privacy policy (L-15).
  - v1.1: design to DPDP from day one, without waiting for May 2027:
    - a plain-language notice listing data, purposes and Google as the recipient, plus how to withdraw and how to complain;
    - consent by a clear tap, withdrawable in Settings as easily as it was given;
    - a published contact for data questions;
    - a 90-day response process;
    - the children's rules in L-05.
- **Until May 2027:** the older IT Act s.43A / SPDI Rules 2011 regime covers "sensitive personal data" (passwords, financial or health data). We collect none, so this changes nothing (T4, from memory, not checked).
- **Sources:** S25 T1 (PIB backgrounder, 17 Nov 2025), S26 T1 (Act text; automated reading blocked), S27 T2.
- **Advice?** **Yes — before v1.1 ships ads in India.** Confirm the Data Fiduciary status and the notice/consent wording. Not a v1 cost.

### L-02 EU, UK and Switzerland (GDPR / UK GDPR)
- **What:**
  - GDPR reaches a non-EU developer who **offers** goods or services to people in the EU or monitors their behaviour (Art. 3(2)). The EDPB reads "offering" as needing an intention to target EU people.
  - A non-EU controller caught this way generally needs an **EU representative** (Art. 27), with a narrow exception for occasional, low-risk processing (T4 for the exception's scope — UNVERIFIED).
  - Google requires a **Google-certified consent tool** (e.g., its UMP SDK) for ads to EEA/UK users since 16 Jan 2024 and Swiss users since 31 Jul 2024.
- **Why here:** Play lists an app in every country by default. An English UI plus ads in an EU or UK listing plausibly counts as offering and monitoring.
- **Forces:** **P-1 — launch in India only.** That means no GDPR work and no UMP in v1.1. If EEA/UK is added later: UMP, a GDPR-grade privacy policy, and possibly a paid EU/UK representative service (needs the owner's yes and a price).
- **Sources:** S32 T1, S22 T1.
- **Advice?** Only before distributing in EEA/UK/Switzerland. Not a launch cost.

### L-03 United States and other Tamil-diaspora countries
- **What:**
  - **COPPA** covers services directed to under-13s, or with actual knowledge of them. The amended rule took effect 23 Jun 2025, with compliance due by 22 Apr 2026.
  - A cartoon bus game could be judged child-directed under the FTC's factors (subject matter, visuals, animated characters) (T4 for the factors).
  - **CCPA** applies only above revenue or volume thresholds we won't reach (T4).
  - **Sri Lanka, Malaysia, Singapore, the Gulf, Canada and Australia** were not researched (gap).
- **Why here:** diaspora Tamils are a natural second market.
- **Forces:** P-1. Add countries one at a time, each after a check.
- **Sources:** S33 T1; T4 as marked.
- **Advice?** Before adding the US (COPPA). Not a launch cost.

### L-04 Consent — what is asked, when, and how it is recorded
- **v1: nothing to consent to.**
  - No data leaves the device, there is no sign-up, there is no marketing, and the company-name box stays on the phone.
  - The **closed-test feedback form** (Google Forms) is a data collection by the owner. Make it anonymous (don't collect emails) and say at the top who reads the answers and why.
- **v1.1 (ads + analytics):**
  1. First launch: language picker, then the **neutral age screen** (L-05).
  2. 18+: one short notice ("Bus Captain shows ads and measures play using Google services. They receive your device's advertising ID, IP address and in-game taps to show ads and count usage.").
  3. Two equal buttons, e.g. "Personalised ads" / "Basic ads only". Nothing is pre-selected.
  4. Record on the device: consent version, choice, timestamp, app version.
  5. A Settings row ("Privacy choices") changes it in one tap, as easily as it was given.
  6. Under-18 or no answer: no ad SDK, no analytics.
- **Marketing:**
  - None is planned.
  - Later notifications are an OS opt-in, game-only, and never pre-enabled.
  - Purchases use Play's own confirmation sheet. That meets the Indian e-commerce rule against recording purchase consent through pre-ticked boxes (Rule 4(9)).
- **Sources:** S25 T1 (consent clear, withdrawable); S22 T1 (withdrawal as easy as giving); S30 T1 (Rule 4(9)).
- **Advice?** The v1.1 notice wording and whether "basic" (non-personalised) ads still need consent under DPDP. **Before v1.1.**

### L-05 Age gating and children's data
- **What:**
  - **India:** a "child" is anyone under **18**. Processing a child's personal data needs **verifiable parental consent**. **Tracking, behavioural monitoring and targeted advertising directed at children are banned.** Exemptions cover only listed classes such as healthcare, education and real-time safety; games aren't among them.
  - **Google Play:**
    - Ages 13–15 and 16–17 "may be considered to include children in some locales".
    - "If your app targets any users who are under the age of 21, you must consider if they would be considered children under their local laws."
    - If the audience includes children, the **Families policy** applies. Only Families self-certified ad SDKs; no interest-based ads; no advertising ID from children or users of unknown age.
    - A mixed audience needs a **neutral age screen**, e.g. "a system that asks users to freely enter their month, day, and year of birth". A preset age is "an incorrect setup".
    - Marketing that suggests a children's app can get it rejected.
- **Why here:** a cartoon bus game set in Tamil Nadu will have 13–17-year-old players in India. The 13+ setting [F] does not make them adults under Indian law.
- **Forces (P-5):**
  - **v1:** nothing to gate, because no data is processed. Declare target audience 13–15, 16–17, 18+.
  - **v1.1:** the neutral age screen runs **before** any SDK initialises.
    - 18+: the consent choice in L-04, then ads and analytics.
    - Under-18 or unknown: AdMob and Firebase are never started, so the game plays ad-free with nothing leaving the device.
    - Later purchases by minors go through Google Play's own payment and parental controls (T4).
  - **Listing:** a business-tycoon tone; no kid-styled marketing.
- **Sources:** S25 T1, S27 T2 (Act s.9 via law-firm summaries; India Code blocks automated reading), S02 T1, S03 T1.
- **Exact rule text:** the wording of DPDP Rule 10 (how parental consent is verified) was not read. UNVERIFIED in detail; reading Rule 10 would confirm.
- **Advice?** **Yes — before v1.1:**
  1. Is a self-declared 18+ enough to treat a player as an adult?
  2. Can no-ID contextual ads be shown to under-18s without parental consent?
  - If yes to (2), under-18s could get basic ads later. Until then, no.

### L-06 SDKs and tracking — what each collects without being asked

| Component | When | What it collects or sends automatically | Links data across other companies' apps for ads? | What we do |
|---|---|---|---|---|
| libGDX | v1 | Nothing | No | — |
| Android Auto Backup (the OS) | v1 | The save file, to a private folder in the player's own Google Drive. ≤25 MB, roughly nightly, only the latest copy kept. **End-to-end encrypted on Android 9+ when the phone has a screen lock** (S19 T1) | No | **P-4:** back up only with client-side encryption (`requireFlags="clientSideEncryption"`). Data unreadable by the developer because of end-to-end encryption "does not need to be disclosed" (S04 T1), so Data safety can say **"No data collected"** |
| Google Mobile Ads SDK (AdMob) | v1.1 | IP address (approximate location); product interactions (launches, taps, video views); diagnostics; device and account IDs (advertising ID, app set ID, signed-in account IDs). For ads, analytics and fraud prevention; encrypted in transit (S21 T1, SDK 25.5.0) | **Yes, when ads are personalised** (the advertising ID is used across apps) | Declare in Data safety; AD_ID permission and Play Console advertising-ID declaration (S11 T1); 18+ only (P-5); offer "basic ads only" |
| UMP SDK | Only if EEA/UK is added | Consent state; a geography check | No | Not needed with P-1 |
| Firebase Analytics | v1.1 | App-instance ID, advertising ID (can be disabled — we will), coarse location from masked IP, events (S24 T1) | [A] No, with the advertising ID disabled and no Ads linking | 18+ only; disable ad-ID collection; shortest retention |
| Play Billing Library | v1.2 | Purchase tokens and order data on the device; Google handles payment | No | Tokens stay on the device. [Q] Confirm Data safety treatment of purchase history before v1.2 |
| Play Games Services | Later | Player ID, achievements (T4) | No | Read Google's Play Games data-collection page before adding |

- **Android advertising-ID rules** (S11 T1):
  - The AD_ID permission is merged in automatically by the ads SDK.
  - For ads, use the advertising ID instead of other device identifiers.
  - Respect the user's reset or opt-out; the ID is zeroed when they opt out.
  - Apps for children must not transmit it.
  - P-5 handles this by never starting AdMob for under-18s.
- **iOS App Tracking Transparency:** not applicable, because there's no iOS. If iOS ever ships with personalised AdMob ads, the ATT prompt must come first (T4).
- **Data safety form** (S04 T1):
  - Even apps that collect nothing must complete it and link a privacy policy.
  - Collection by third-party SDKs must be declared.
  - Apps active only on internal testing tracks are exempt.
- **Advice?** Standard practice. No advice needed.

### L-07 Data retention and deletion
- **v1:**
  - Everything lives on the phone. Uninstalling removes it, and Settings has "Reset progress".
  - Auto Backup keeps only the latest copy, in the player's own account.
  - **Play's account-deletion requirement does not apply:** it covers apps that "allow users to create an account". But **all developers must still answer the Data safety deletion questions**. Answer: no accounts; data stays on the device; uninstall or Reset deletes it.
  - Apple's in-app account deletion rule: not applicable (no iOS, no accounts).
- **v1.1:**
  - Set Firebase to its shortest retention (T4: GA4 offers 2 or 14 months).
  - AdMob data follows Google's retention.
  - Under DPDP, erase data when its purpose ends or consent is withdrawn, and answer requests within 90 days (S25 T1).
  - In Settings: "Stop analytics & ad personalisation", plus a one-line pointer to Android's "Delete advertising ID".
- **Changes nothing:**
  - The DPDP Rules' retention limits for large "online gaming intermediaries" (T2; not read) don't apply. We have no registered users and aren't an intermediary.
  - The 2026 gaming rules' data-retention rule (Rule 17) applies only once the government issues directions (S29 T1).
- **Sources:** S05 T1, S19 T1, S25 T1, S29 T1.
- **Advice?** Standard practice.

### L-08 User-generated content
- **What:** Play's user-generated-content rules cover content users contribute "that is visible to or accessible by at least a subset of the app's users". Such apps need reporting, blocking, moderation and accepted terms. Apple's four requirements (filter, report, block, contact) are the iOS equivalent.
- **Why here:**
  - Company names and liveries **never reach other players inside the app**. Sharing goes through the Android share sheet, initiated by the player.
  - So the UGC rules don't apply in v1 or v1.1. Apple's don't apply either (no iOS).
- **Forces:**
  - Keep it that way: no in-app gallery or feed (already a non-goal).
  - **P-10:** the LATER "livery codes" feature must carry colours and pattern IDs only. Free text imported into another player's game would arguably be user content.
  - A small local word filter for name boards (standard practice).
- **Sources:** S12 T1.
- **Advice?** No.

### L-09 Money — payment rules, refunds, disclosures, fair offers
- **Play Billing is mandatory** for in-app digital goods ("must use Google Play's billing system…"). No Stripe, no UPI links.
- **Fee:** 15% on the first $1M a year once enrolled in the 15% tier, outside AU/EEA/JP/UK/US (S08 T1). Enrol before v1.2 (free).
- **Pending purchases:** UPI and other slow methods can arrive *pending*.
  - Grant only when the purchase becomes PURCHASED.
  - **Acknowledge within 3 days, or Google refunds and revokes it** (S20 T1).
  - Acknowledging on the device is supported; a server is recommended but not required.
  - Grants are idempotent per purchase token, recorded in the save, so a restore never double-grants.
- **Refunds:**
  - Buyers can request one through Google within ~48 h. After that, developers refund "according to their policies and applicable laws" (S15 T1).
  - Indian law requires a seller to refund services that are **deficient or not as advertised**, e.g. Remove Ads that doesn't remove ads (E-Commerce Rules r.6(3), S30 T1).
- **Subscriptions and auto-renew disclosures:** not applicable; none are planned.
- **Loot boxes:** randomised paid items require disclosed odds (S07 T1). We have none, by design (an owner rule).
- **India's gaming law:**
  - "Other stakes" include coins "real or virtual… purchased by paying money" (Act s.2(1)(j)).
  - An online money game is one played "by paying fees, depositing money or other stakes in expectation of winning" (s.2(1)(g)).
  - Offering one carries up to **3 years / ₹1 crore** (s.9(1)) (S28 T1).
  - **P-6:** never sell coins. The starter pack gives cosmetics and a permanent time boost. Nothing bought can be risked for a chance at more.
- **Dark patterns** (Central Consumer Protection Authority, 30 Nov 2023): 13 listed patterns, including false urgency, confirm shaming, forced action, nagging, interface interference, drip pricing and disguised advertisement (S31 T2).
  - **P-7:** no fake timers on offers; no shaming "No thanks"; no repeated prompts; "No thanks" as visible as "Yes"; ads clearly labelled.
- **Advice?** If the owner wants to sell coins after all, get advice first. Otherwise standard practice. **Before v1.2.**

### L-10 Sector rules — India's online gaming law (Act 2025 + Rules 2026)
- **What:**
  - **Act No. 32 of 2025** (assented 22 Aug 2025) bans online money games and their advertising.
  - It defines an **"online social game"** as one with no staking for monetary gain. It may charge a subscription or one-time fee "not in the nature of a stake", and must be offered solely for entertainment, recreation or skill development.
  - The **Rules 2026** (G.S.R. 303(E), notified 22 Apr 2026, **in force 1 May 2026**):
    - A social game needs no determination or registration unless the government notifies its category (Rules 8 and 12).
    - **Every provider of an online social game "shall establish and maintain a functional grievance redressal mechanism"** (Rule 20). Dissatisfied users may go to the Online Gaming Authority within 30 days.
    - Contact-point and data-retention duties (Rules 16–17) apply only once directions are issued.
- **Why here:** Bus Captain is a social game if it counts as an "online game" at all ("operated as a software through the internet"; arguable for an offline game, clearer once ads and purchases arrive).
  - **Advertising money games** is an offence in itself: up to 2 years / ₹50 lakh (s.6, s.9(2)). An ad network could serve such ads inside our app.
- **Forces:**
  - **P-2:** a complaints route from v1 (Settings row + support email + written process).
  - **P-6:** keeps us clearly a social game.
  - **P-14:** at v1.1, block gambling and real-money-game ad categories in AdMob (the AdMob control itself is T4; confirm in the AdMob console).
  - Check for category notifications quarterly (R-33).
- **Health, finance, education, dating:** none apply.
- **Sources:** S28 T1 (Act text), S29 T1 (Rules text) + T2 (Medianama summary).
- **Advice?** Not for v1 (standard practice). Get advice only if competitions with prizes are ever added, since that is e-sports territory.

### L-11 Selling in India — seller identity, complaints route, invoices, consumer rules
- **Who is named as seller:** the developer's legal name on the Play listing. For accounts selling paid apps or in-app purchases, also the **full address** (S10 T1; see L-12).
- **E-Commerce Rules 2020** (G.S.R. 462(E), 23 Jul 2020) cover "all goods and services bought or sold over digital or electronic network including digital products". They exclude only a natural person's activity that isn't "professional or commercial… on a regular or systematic basis" (r.2).
  - [A] Most likely classification: **a seller on a marketplace (Google Play).** The rules exclude marketplace sellers from "e-commerce entity" (r.3(1)(b)), and r.4(1)(a) expects e-commerce entities to be companies.
- **Seller duties (r.6)** (S30 T1):
  - No unfair trade practices.
  - Refund or withdraw services that are deficient or not as advertised (6(3)).
  - Appoint a **grievance officer** who **acknowledges within 48 hours and resolves within one month** (6(4)(b)).
  - Ads consistent with actual features (6(4)(c)).
  - Give the marketplace your legal name, principal address, website, email, customer-care contacts, and GSTIN/PAN where applicable (6(4)(d)).
  - Have the grievance officer's name, contact and designation, the refund terms, and the total price including tax displayed (6(5)).
- **If instead the app itself counts as the "e-commerce entity"** (r.4): name, address, website, customer-care and grievance contacts must be shown **inside the app**. That's another reason to put them in Settings.
- **Invoices and tax:**
  - Google issues the buyer's purchase receipt (T4).
  - **India-based developers must work out their own GST**; Google won't advise (S47 T1).
  - Google withholds tax from payouts: **0.1% of the purchase price (excluding GST) with PAN, 5% without**, with quarterly Form 131 certificates (S09 T1). Add your PAN to the payments profile.
  - GST registration is generally needed above ₹20 lakh a year of turnover for services in most states, after which proper tax invoices are required (T4, from memory, not checked).
- **Consumer rules that apply:**
  - Consumer Protection Act 2019;
  - E-Commerce Rules 2020;
  - Dark Patterns Guidelines 2023;
  - the gaming Rules' grievance duty;
  - DPDP's contact and 90-day duties (from v1.1).
- **Forces:**
  - **P-2/P-3 now:** one Settings row and a dedicated support email. Write down a response routine: acknowledge in 48 h, resolve in 30 days.
  - **At v1.2:** Terms + refund statement; grievance details on the store listing, website and in the app.
- **Advice?**
  - **Yes — before v1.2:** which category applies and what must be displayed; Terms and refund wording.
  - **A chartered accountant once revenue starts:** GST and income tax. That isn't a launch cost.

### L-12 Your address goes public once you sell anything
- **What:** "merchant accounts (developer accounts with apps that monetize via paid apps or in-app purchases) must show their full address on Google Play. The address used is the one taken from the Google payments profile" (S10 T1, read in full 2026-11-24). Google's stated reason: "to comply with consumer protection laws".
- **The Indian rule behind it:** marketplaces must display each seller's "name of their business, whether registered or not, their geographic address, customer care number" (E-Commerce Rules 2020, r.5(3)(a); S30 T1). So every route shows *an* address; the routes differ only in whose.
- **Why here:** on a personal account [A] that is your home address, permanently scrapeable.
- **What else a personal account makes public** (S10 T1):
  - The developer name and the store-listing email. Both are public from v1; the email is the dedicated support address (P-3).
  - A public phone number is required only from Korea-based developers. The store-listing phone is optional. [A] Leave it blank unless the v1.2 advice says r.5(3)(a)'s "customer care number" needs one.
- **Forces (P-5):**
  - v1.1 is ads-only. [A] An ads-only app has no merchant account, so the address isn't shown, based on S10's "paid apps or in-app purchases" wording.
    - Confirm on Play Console's **Play developer profile** page (Manage account › Account details). Google says it shows all the information that may be public.
    - Check before enabling ads, and again before the first in-app product.
  - Before v1.2, choose:
    - (a) accept;
    - (b) an organisation account, which needs a registered business + D-U-N-S number and an app transfer. That's effort and possibly cost, so it needs your yes;
    - (c) stay ads-only.
- **DECIDED 2026-11-24 [F]: (a) accept.**
  - The owner first asked to keep the address private. After seeing the cost of (b), they chose to show it ("okay to give address").
  - It appears from v1.2 only; v1 and v1.1 show none.
- **Fallback (b), if the owner changes their mind** (checked 2026-11-24):
  1. **A sole proprietorship at a non-home address**, either:
     - a virtual office in Coimbatore: ₹599–₹1,400 a month, or about ₹16,000–₹20,000 a year (S54); or
     - a relative's or friend's business address, with their written permission. That's free, but their address becomes public instead.
  2. **A D-U-N-S number in exactly that name and address.**
     - It's free from Dun & Bradstreet and takes up to 30 days. Avoid paid third-party services (S51, S53 T1).
     - It must match the payments profile exactly.
     - UNVERIFIED whether D&B and Google accept a virtual-office address. Ask both before paying.
  3. **A new organisation developer account:** US$25 (≈ ₹2,340). It publicly shows the organisation name, D-U-N-S number, address, developer email and developer phone (S10 T1).
  4. **Transfer the app.** Users, download stats, ratings and reviews move; test groups don't (S52 T1).
  - **Cost:** about ₹9,500–₹22,000 in the first year, or ≈ ₹2,340 with a family address. Each paid item needs the owner's yes.
  - It cannot un-publish an address already shown: copies scraped earlier stay out there.
- **Advice?** None needed; the decision is made.

### L-13 Licences — code, fonts, art, sound, maps

| Item | Licence | What it requires | Source |
|---|---|---|---|
| libGDX | Apache-2.0 | Include the licence and any NOTICE contents in the distribution (§4(d)) — goes on the Credits & licences screen | S38 T1, S34 T1 |
| gdx-freetype (FreeType), if used for Latin text | Choose **FTL** (not GPLv2) | Credit line in documentation: "Portions of this software are copyright © <year> The FreeType Project (https://freetype.org). All rights reserved." | S39 T1 |
| Noto Sans Tamil | SIL OFL 1.1, **no Reserved Font Name** in its header | Bundle with the OFL text; **subsetting is allowed without renaming**; don't sell the font on its own | S40 T1 |
| Latin UI font | [A] Noto Sans or another OFL font | Same OFL rules | T4 |
| Matn (HarfBuzz for libGDX), fallback only | Apache-2.0 | Notice; note it has 0 stars and one maintainer (last push 7 Oct 2026), a dependency risk | S37 T1 (licence field) |
| Kenney packs, if used | CC0 | No credit needed; keep each pack's License.txt | T4 — confirm per pack |
| Tamil Nadu outline (map) | **Natural Earth: public domain** | Nothing (credit optional) — **P-11** | S41 T1 |
| OpenStreetMap / Google Maps | ODbL (attribution; share-alike for databases) / Google terms | **Don't trace from them** | S42 T1; Google terms T4 |
| Sounds made with jsfxr / BeepBox | Generated output | [A] Output is yours; confirm each tool's terms | T4 |
| Horn tunes | Original compositions | No film melodies (copyright) — owner rule | T4 |
| Google SDKs (Ads, UMP, Firebase, Billing, Play Games) | Google's terms (not open source) | Follow AdMob and Play policies | T4 |
| **GPL / AGPL anything** | — | **Banned:** would force releasing the game's source. Check every new dependency's licence (Apache, MIT, BSD, OFL, CC0 are fine) | T4, standard practice |

**Forces:** P-11, P-12. CREDITS.md and the in-app Credits & licences screen list everything above. **Advice?** Standard practice.

### L-14 Names, marks, likeness and content rules
- **"Bus Captain" clash** (checked 2026-11-23):
  - **Google Play India search:** no *game* titled "Bus Captain" (S48, seen). Hits were bus-staff and driver apps:
    - "Bus Captain-Yolobus" (yolobus.in, 1K+ downloads, Travel & Local);
    - "Captain Bus Service" (500+);
    - "Captain by Cityflo" (10K+);
    - "Rapido Captain: Drive & Earn" (50M+);
    - "Daily Shuttles Captain" (100+);
    - one small game, "Road Captain: Truck & Bus Life" (100+).
  - **"Captain" is a generic driver title:** Indian mobility firms use it for drivers (Rapido, Cityflo, YoloBus), and "Bus Captain" is SBS Transit Singapore's official bus-driver job title (S49). [A] That makes the phrase descriptive in transport, so a strong exclusive claim is unlikely.
  - **Store search will mix the game with driver apps.** A distinctive title suffix helps, e.g. "Bus Captain: Bus Tycoon".
  - **Not yet searched:** the **IP India trademark registry** (free public search, captcha — owner task). Search the word marks "BUS CAPTAIN" and "CAPTAIN" in classes 9 (software), 28 (games) and 41 (entertainment) [A: likely classes] **before the first upload**.
  - Forces P-8 (package name done; trademark search pending).
- **"Captain" in Tamil Nadu:**
  - It is the widely used title of the late actor-politician **Vijayakanth**, from his 100th film *Captain Prabhakaran* (1991). He founded the DMDK party in 2005 and died 28 Dec 2023 (S44 T2).
  - [A] Legal risk from the common word alone is low (T4). The association is the risk.
  - Forces **P-9:** no likeness, voice, film titles, slogans, or DMDK colours or symbols, in the game or its marketing.
- **Real operators, manufacturers, logos, film songs, religious imagery:** banned by the owner [F]. This also keeps clear of trademark, passing-off and copyright claims and Play's IP/impersonation policy (T4).
- **Government emblems and the Tamil Nadu state seal:** never use them (Emblems and Names Act 1950; State Emblem Act 2005 — T4, from memory, not checked).
- **Map:** Tamil Nadu only, never an outline of India, which avoids India's sensitivity about border depictions (T4).
- **Fictional company and manufacturer names:** quick web and Play search for each before shipping (standard practice).
- **Advice?** A trademark attorney only if a conflict appears, or later if you file your own trademark (costs money, needs your yes).

### L-15 The pages the stores and the law expect

| Page | Needed? | Where | Must contain |
|---|---|---|---|
| **Privacy policy** (public URL, not a PDF; linked in Play Console **and inside the app**) | **Now — before the first tester** (Play requires it even for apps that collect nothing: S04 T1, S06 T1) | GitHub Pages; Settings → link opens the browser | **v1:** developer name, app name; "collects no personal data; game data stays on your phone; Android may back it up encrypted to your own Google account"; sharing happens only when you tap Share; children; contact; effective date; how changes are announced. **v1.1 adds:** Google AdMob and Firebase, data types, purposes, the 18+ rule, consent and how to withdraw, retention, your rights and the 90-day response, grievance contact, complaints to India's Data Protection Board |
| **Help & complaints** (grievance) | **Now** (gaming Rules r.20; later e-commerce r.6 and DPDP) | Settings row + same text on the website | Support email, grievance contact name and designation, response times (48 h acknowledgement / 30 days), escalation route (Online Gaming Authority) |
| Terms of use / EULA | Not legally required for v1 (T4); **needed at v1.2** | Website + Settings link | Licence to play; coins and virtual items have no cash value, can't be transferred and can't be wagered; purchases via Google Play; refunds; grievance contact; governing law India |
| Refund terms | **At v1.2** | Inside the Terms | Google handles requests within ~48 h; after that contact us; we refund any purchase that doesn't work as described (r.6(3)) |
| Cookie policy and consent | **Not needed:** the GitHub Pages site sets no cookies without analytics (T4), and the app has no in-app web view (the policy opens in the browser) | — | Revisit if analytics is ever added to the website |
| Data safety form | **Now** | Play Console | v1: "No data collected / shared" (requires P-4) plus the deletion answers. v1.1: AdMob + Firebase items |
| Content rating (IARC questionnaire) | Now | Play Console | Honest answers: no violence, no gambling, no user interaction |
| Target audience & content | Now | Play Console | 13–15, 16–17, 18+; store art that doesn't suggest a kids' app |
| Ads declaration | v1: "No"; **v1.1: "Yes"** ("Contains ads" label; includes third-party SDK ads — S14 T1) | Play Console | — |
| Advertising-ID declaration | v1.1 | Play Console | Used for advertising (and analytics if enabled) |
| app-ads.txt | v1.1 (recommended by AdMob) | Root of the developer website | [A] Needs a user site `rameshkumark24.github.io` — T3/T4, UNVERIFIED |

### L-16 Advice register — what needs a professional, and when

| Item | Advice or standard practice | When | Launch cost? |
|---|---|---|---|
| Privacy policy v1, Data safety, licence notices, complaints row | Standard practice | Before the first tester | No — free, written in-house |
| Name clearance ("Bus Captain") | Standard practice (free self-searches); attorney only if a conflict appears | Before the first upload | No |
| DPDP: Data Fiduciary status, notice/consent wording, age screen and under-18 ads | **Get advice** | **Before v1.1** (and in any case before May 2027) | No (v1 doesn't need it) |
| E-commerce classification, in-app disclosures, Terms and refund wording | **Get advice** | **Before v1.2** (in-app purchases) | No |
| Selling coins under the gaming law | **Advice only if you choose to sell coins**; otherwise P-6 is standard practice | Before v1.2 | No |
| GST, income tax, invoices | **Get advice (CA)** | Once revenue starts / approaches the GST threshold | No — paid from revenue |
| GDPR, UK, US or any other country | **Get advice** | Only before distributing there | No |
| Organisation account vs public home address | Your decision — **made 2026-11-24: show the home address** (L-12) | Before v1.2 | No |

---

## 3. What would get this rejected or pulled — ranked by likelihood

| # | ID | Likelihood | What triggers it here | Prevention | Sources |
|---|---|---|---|---|---|
| 1 | X-01 | **High** | **Production access not granted.** Fewer than 12 testers stayed opted in for 14 continuous days, or "insufficient tester engagement". The owner has 14 testers, a margin of 2 | P-13: recruit ~20; give testers a daily reason to open the game (Stats screen goals); answer the application's three sections (closed test, app, readiness) concretely. If not approved, the closed test continues | S01 T1 |
| 2 | X-02 | Medium–High | **Privacy policy / Data safety mismatch:** no in-app link, a broken URL, or "no data" declared while an SDK collects | v1 ships no SDKs; P-4 backup config; update the form *before* uploading v1.1 | S04 T1, S06 T1 |
| 3 | X-03 | Medium | **Metadata policy.** Title over 30 characters; emoji; ALL CAPS; "#1/best/top"; keyword lists in the description (the planned keywords must read as sentences, not a list) | Title ≤30 characters, e.g. "Bus Captain: Bus Tycoon" (23); keywords woven into natural sentences in English and Tamil | S13 T1 |
| 4 | X-04 | Medium | **Target audience vs appeal to children:** cartoon buses + declared 13+ | Business-tycoon tone in icon, screenshots and text; no kid-styled characters in marketing | S02 T1 |
| 5 | X-05 | Medium | **Upload blocked:** target API below 36; non-16 KB-aligned native libraries from a stale `android/libs` | Build checklist (R-20); APK Analyzer check before every upload | S16 T1, S17 T1, S35 T2 |
| 6 | X-06 | Medium (v1.1+) | **Ads:** an interstitial at a surprising moment; a reward granted before the ad completes; **AdMob invalid traffic** from testers | M5 interstitial rules; reward only on the completion callback; test ads in every non-production build | S23 T1 |
| 7 | X-07 | Low–Medium | **IP / impersonation complaints:** name clash; lookalike real liveries in *our* presets or marketing; horn tunes resembling film songs | P-8 searches; original presets; original tunes | S45 T3; T4 |
| 8 | X-08 | Low–Medium | **Broken on reviewer devices:** crash at launch, black screen on a notched phone | Test on several screen shapes; keep crash/ANR under 1.09% / 0.47% (Play's bad-behaviour thresholds, which hurt visibility rather than reject — T4) | T4 |
| 9 | X-09 | Low | **Payments policy:** selling digital items outside Play Billing; randomised paid items without odds | Play Billing only; no randomised paid items (P-6) | S07 T1 |
| 10 | X-10 | Low (with P-5) | **Families policy:** ads to children or unknown-age users from a non-certified SDK, or using the advertising ID | P-5: no SDK starts for under-18s or unknown ages | S03 T1 |
| 11 | X-11 | Low | **Legal complaint in India:** money-game features, ads for money games, dark patterns | P-6, P-7, P-14 | S28 T1, S31 T2 |
| 12 | X-12 | Low | **Account-level:** payments-profile/identity mismatch when monetising | Keep the legal name and address identical across Play Console and the payments profile | S10 T1 |

---

## 4. Sources

Dates are the source's own date where shown. "Undated" = the page shows no date. All were checked on **2026-11-23** (S10 and S16 on 2026-11-22).

| ID | Source | Date | Tier | How checked |
|---|---|---|---|---|
| S01 | Google Play — App testing requirements for new personal developer accounts — https://support.google.com/googleplay/android-developer/answer/14151465 | Undated | T1 | Read in full |
| S02 | Google Play — Target audience and app content — https://support.google.com/googleplay/android-developer/answer/9867159 | Undated | T1 | Read in full |
| S03 | Google Play — Families policy — https://support.google.com/googleplay/android-developer/answer/9893335 | Undated | T1 | Read in full |
| S04 | Google Play — Data safety section — https://support.google.com/googleplay/android-developer/answer/10787469 | Undated | T1 | Read in full (last part via summary) |
| S05 | Google Play — Account deletion requirements — https://support.google.com/googleplay/android-developer/answer/13327111 | Undated | T1 | Read in full |
| S06 | Google Play — User Data policy (privacy policy in Console and in app) — https://support.google.com/googleplay/android-developer/answer/10144311 | Undated | T1 | Search excerpt |
| S07 | Google Play — Payments policy — https://support.google.com/googleplay/android-developer/answer/9858738 | © 2026 | T1 | Read in full |
| S08 | Google Play — Service fees — https://support.google.com/googleplay/android-developer/answer/112622 | Undated | T1 | Read in full |
| S09 | Google Play — Withholding tax (India PAN 0.1% / 5%, Form 131) — https://support.google.com/googleplay/android-developer/answer/9384608 | Undated | T1 | Read in full |
| S10 | Google Play — Developer information shown on Google Play (full address for merchant accounts) — https://support.google.com/googleplay/android-developer/answer/13634081 | Undated | T1 | Read in full (2026-11-24) |
| S11 | Google Play — Advertising ID — https://support.google.com/googleplay/android-developer/answer/6048248 | Undated | T1 | Search excerpt |
| S12 | Google Play — User generated content — https://support.google.com/googleplay/android-developer/answer/9876937 | Undated | T1 | Search excerpt |
| S13 | Google Play — Metadata policy — https://support.google.com/googleplay/android-developer/answer/9898842 | Undated (title limit since 2021) | T1 | Search excerpt |
| S14 | Google Play — Ads declaration ("Contains ads") — https://support.google.com/googleplay/android-developer/answer/9859455 | Undated | T1 | Search excerpt |
| S15 | Google Play — Refund policies (buyers) — https://support.google.com/googleplay/answer/15574908 | Undated | T1 | Search excerpt |
| S16 | Android — Target API level requirements (API 36 from 31 Aug 2026; extension to 1 Nov 2026) — https://developer.android.com/google/play/requirements/target-sdk | Undated | T1 | Search excerpt (2026-11-22) |
| S17 | Android — Support 16 KB page sizes (new apps since 1 Nov 2025; updates by 1 Feb 2027) — https://developer.android.com/guide/practices/page-sizes | Undated | T1 | Search excerpt |
| S18 | Android 16 behaviour changes for apps targeting API 36 — https://developer.android.com/about/versions/16/behavior-changes-16 | Undated | T1 | Search excerpt |
| S19 | Android — Back up user data with Auto Backup — https://developer.android.com/identity/data/autobackup | Undated | T1 | Read in full |
| S20 | Android — Integrate the Play Billing Library (pending; acknowledge in 3 days) — https://developer.android.com/google/play/billing/integrate | Undated (examples use PBL 9.1.0) | T1 | Read (parts via summary) |
| S21 | AdMob — Google Play data disclosure (Mobile Ads SDK 25.5.0) — https://developers.google.com/admob/android/privacy/play-data-disclosure | Undated | T1 | Read in full |
| S22 | Google — consent management requirements for EEA/UK/CH (certified CMP from 16 Jan 2024; CH 31 Jul 2024) — https://support.google.com/admob/answer/13554116 | 2024 | T1 | Search excerpt |
| S23 | AdMob — Invalid activity — https://support.google.com/admob/answer/3342099 ; Test ads — https://developers.google.com/admob/android/test-ads | Undated | T1 | Search excerpt |
| S24 | Google Analytics for Firebase — Play data disclosure — https://support.google.com/analytics/answer/11582702 (also https://firebase.google.com/docs/android/play-data-disclosure) | Undated | T1 | Search excerpt |
| S25 | Government of India (PIB) — "DPDP Rules, 2025 Notified" backgrounder — https://static.pib.gov.in/WriteReadData/specificdocs/documents/2025/nov/doc20251117695301.pdf | 17 Nov 2025 | T1 | Read in full (text extracted) |
| S26 | Digital Personal Data Protection Act, 2023 (India Code) — https://www.indiacode.nic.in/bitstream/123456789/22037/2/a2023-22.pdf | Act 11 Aug 2023; copy current as on 19 Nov 2025 | T1 | **Not readable** (automated access refused); contents via S27 |
| S27 | DPDP commentary — Bar & Bench (Nov 2025) https://www.barandbench.com/news/centre-notifies-digital-personal-data-protection-rules-2025-how-different-are-they-from-the-draft-rules ; SCC Online (26 Dec 2025) https://www.scconline.com/blog/post/2025/12/26/digital-personal-data-protection-rules-2025-key-highlights/ ; Legal500 18-month countdown https://www.legal500.com/firms/237811-saga-legal/c-india/news-and-developments/the-privacy-timer-starts-now-analysing-the-dpdp-rules-and-its-18-month-countdown ; Cyril Amarchand Mangaldas on children (Aug 2023) https://corporate.cyrilamarchandblogs.com/2023/08/children-and-consent-under-the-data-protection-act-a-study-in-evolution/ | 2023–2025 | T2 | Search excerpts |
| S28 | Promotion and Regulation of Online Gaming Act, 2025 (Act 32 of 2025; MeitY copy) — https://www.meity.gov.in/static/uploads/2025/10/8a7f103cefc68ed8aaa2ebc9a2ed7c13.pdf | Assented 22 Aug 2025; corrigenda 28 Aug 2025 | T1 | Read (text extracted): s.2(1)(f),(g),(i),(j); s.4–7; s.9 |
| S29 | Promotion and Regulation of Online Gaming Rules, 2026 (G.S.R. 303(E)) — Gazette copy: https://www.medianama.com/wp-content/uploads/2026/04/Promotion-and-Regulation-of-Online-Gaming-Rules-2026-APR232026.pdf ; summary: https://www.medianama.com/2026/04/223-online-gaming-rules-notified-may-1-major-changes/ | Notified 22 Apr 2026; in force 1 May 2026 | T1 (gazette text) + T2 (summary) | Read (text extracted): r.2, 16–20 |
| S30 | Consumer Protection (E-Commerce) Rules, 2020 (G.S.R. 462(E)) — Gazette copy: https://taxguru.in/wp-content/uploads/2020/07/Consumer-Protection-E-Commerce-Rules-2020.pdf | 23 Jul 2020 | T1 (gazette text, third-party host) | Read (text extracted): r.2–4, 6 |
| S31 | Guidelines for Prevention and Regulation of Dark Patterns, 2023 (Central Consumer Protection Authority) — Deloitte alert https://www2.deloitte.com/in/en/pages/tax/articles/tax-alert-the-guidelines-for-prevention-and-regulation-of-dark-patterns-2023.html ; JSA https://jsalaw.com/wp-content/uploads/2023/12/JSA-Prism-Consumer-Protection-Guidelines-for-Prevention-and-Regulation-of-Dark-Patterns-20233-Final.pdf | 30 Nov 2023 | T2 | Search excerpts (official text not read) |
| S32 | EDPB Guidelines 3/2018 on territorial scope — https://edpb.europa.eu/sites/edpb/files/files/file1/edpb_guidelines_3_2018_territorial_scope_en.pdf | Final 12 Nov 2019 | T1 | Search excerpt |
| S33 | FTC — Amended COPPA Rule, Federal Register — https://www.govinfo.gov/content/pkg/FR-2025-04-22/html/2025-05904.htm | 22 Apr 2025; effective 23 Jun 2025; comply by 22 Apr 2026 | T1 | Search excerpt |
| S34 | libGDX CHANGES file and releases — https://github.com/libgdx/libgdx/blob/master/CHANGES ; https://github.com/libgdx/libgdx/releases | 1.14.2 released 5 Jun 2026 | T1 | Read |
| S35 | libGDX issues on 16 KB pages — https://github.com/libgdx/libgdx/issues/7695 (closed 11 Sep 2025), /7785 (closed 19 Apr 2026: "16K should already be supported"), /7855 (closed 24 Aug 2026: stale `android/libs`) | 2025–2026 | T2 (maintainers) | Read |
| S36 | HarfBuzz manual — "Why do I need a shaping engine?" — https://harfbuzz.github.io/why-do-i-need-a-shaping-engine.html | Undated | T1 | Search excerpt (via manual mirrors) |
| S37 | libGDX HarfBuzz shaping discussion — https://lightrun.com/answers/libgdx-libgdx-discussion-harfbuzz-shaping-extension-design ; Matn — https://github.com/0880880/matn | Matn last push 7 Oct 2026 | T3 (discussion) / T1 (Matn licence field) | Search excerpt; GitHub API |
| S38 | Apache License 2.0 — https://www.apache.org/licenses/LICENSE-2.0 | 2004 | T1 | Read §4(d) |
| S39 | FreeType licence (FTL, credit clause; dual FTL/GPLv2) — https://gitlab.freedesktop.org/freetype/freetype/-/raw/master/docs/FTL.TXT | Current master | T1 | Read |
| S40 | Noto Sans Tamil OFL.txt — https://github.com/notofonts/tamil/blob/main/OFL.txt | © 2022 | T1 | Read header |
| S41 | Natural Earth — Terms of use (public domain) — https://www.naturalearthdata.com/about/terms-of-use/ | Undated | T1 | Search excerpt |
| S42 | OSM Foundation — Licence and Legal FAQ (ODbL) — https://osmfoundation.org/wiki/Licence_and_Legal_FAQ | Undated | T1 | Search excerpt |
| S43 | GitHub Docs — GitHub Pages limits (not for commercial transactions; 100 GB/month soft limit) — https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits | Undated | T1 | Search excerpt |
| S44 | Vijayakanth, "Captain" — The Week (28 Dec 2023) https://www.theweek.in/news/india/2023/12/28/end-of-an-era-life-and-legacy-of-captain-vijayakanth.amp.html ; Business Standard (28 Dec 2023) https://www.business-standard.com/politics/vijayakanth-an-action-hero-who-took-on-tn-s-political-heavyweights-123122800353_1.html | 28 Dec 2023 | T2 | Search excerpt |
| S45 | Web search for "Bus Captain" game app Android | 2026-11-23 | T3 | Search only |
| S48 | Google Play India search "bus captain" — https://play.google.com/store/search?q=bus%20captain&c=apps&hl=en_IN&gl=IN — plus each hit's store page (e.g., https://play.google.com/store/apps/details?id=com.yolo.bus.captain) | 2026-11-23 | T1 (store pages) | Opened: titles, downloads, categories, developers |
| S49 | SBS Transit (Singapore) bus recruitment brochure — "Bus Captain" job grades — https://cmsweb.sbstransit.com.sg/Uploads/Join_Us/Grow%20with%20us/Grow%20With%20Us/E-Brochure_Bus%20(15%20Oct%2025)%20(1).pdf ; careers page https://discover.nyc.gov.sg/Jobs/Bus-Captain | Oct 2025 | T1 | Search excerpt |
| S50 | Google Play — "Rapido Captain: Drive & Earn" (com.rapido.rider, 50M+) — https://play.google.com/store/apps/details?id=com.rapido.rider | 2026-11-23 | T1 | Opened (title, downloads, developer) |
| S46 | GameAnalytics mobile benchmarks 2025 (75% of projects have day-28 retention under 3%) — https://gamedevreports.substack.com/p/gameanalytics-mobile-gaming-benchmarks | 2025 | T2 | Search excerpt |
| S47 | Google Payments — Tax information for India (India-based developers determine their own GST) — https://support.google.com/paymentscenter/answer/7421525 | Undated | T1 | Search excerpt |
| S51 | Google Play — Required information to create a developer account (organisations need a D-U-N-S number; free from D&B, up to 30 days) — https://support.google.com/googleplay/android-developer/answer/13628312 | Undated | T1 | Read (2026-11-24) |
| S52 | Google Play — Transfer apps to a different developer account (users, stats, ratings and reviews move; test groups don't) — https://support.google.com/googleplay/android-developer/answer/6230247 | Undated | T1 | Read (2026-11-24) |
| S53 | Google Payments — Add a D-U-N-S number to your payments profile (name and address must match D&B exactly) — https://support.google.com/paymentscenter/answer/13992651 | Undated | T1 | Read (2026-11-24) |
| S54 | Virtual-office price pages, Coimbatore ("from" prices: RegisterKaro ₹599/month; IncorpX ₹999/month + 18% GST; Regus ₹1,400/month; Cofynd ₹16,000–₹19,999/year) — https://www.registerkaro.in/virtual-office-in-coimbatore · https://www.incorpx.io/virtual-office-in-coimbatore · https://www.regus.com/en-gb/india/coimbatore/virtual-offices · https://cofynd.com/virtual-office/coimbatore | 2026-11-24 | T1 (vendors' own prices) | Search excerpts |

**Known gaps** (would strengthen this document):
- DPDP Act s.9 and Rule 10 in their own words (India Code refuses automated reading). Read them from a browser.
- The Dark Patterns Guidelines' official text.
- The IP India trademark search for "Bus Captain" (owner task; the Play Store search is done).
- Laws in the Tamil-diaspora countries.
- Play Billing's Data safety treatment.
- Whether an ads-only account shows the address. Check Play Console's Play developer profile page before v1.1 (L-12).
