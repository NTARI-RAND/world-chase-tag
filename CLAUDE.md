# World Chase Tag — Project Intelligence Document (`claude.md`)
 
## Project Overview
 
**World Chase Tag** is a cross-platform mobile app (iOS + Android) that facilitates real-time, :20-second tag matches between users who enter 33-foot proximity within designated safe zones. It operates in two modes: **casual** (drop-in social play) and **competitive** (opt-in ranked matches with leaderboards). A third-party camera operator role records match footage delivered to players post-game. Tags are confirmed **contactlessly** via BLE signal threshold — no physical touch required — making gameplay safer and more accessible. The app blends physical sport, social media, and real-time coordination into a single experience.
 
**License:** Released under **AGPL-3.0** in alignment with NTARI's open-source infrastructure policy. All derivative works, hosted deployments, and modifications must be disclosed under the same license.
 
**Target Market:** Parks and recreation officials — municipal departments, recreation program coordinators, and public space administrators who manage designated outdoor areas and organized physical activity programs. The app is designed to be deployable within existing park infrastructure as a managed, operator-configured experience.
 
---
 
## Core Concepts
 
### The Match
- A match is exactly **20 seconds** long.
- It begins when **two registered users enter a 33-foot (~10-meter) proximity radius** inside a designated safe zone.
- **Roles are randomly assigned** at match trigger: one player becomes the **Chaser**, one becomes the **Runner**.
- A **third nearby user** (the Observer) uses their phone camera to record the chase. The video is **captured server-side** for the duration of the match and held in temporary storage.
- After the match ends, both the Chaser and Runner are notified that footage is available. Each may **choose to save the video** to their device or dismiss it. The option expires after **24 hours**, after which the server copy is permanently deleted regardless of player action.
- The Observer **cannot save, download, or retain the video** — their role is documentation on behalf of the players, not content creation for themselves. The app provides no save or share control on the Observer's post-match screen.
- A successful **tag is contactless** — confirmed entirely by BLE signal strength crossing a close-range threshold. No physical touch is required or expected. This is a core safety design principle.
- If no Observer is present, BLE-only confirmation is the authoritative fallback.
- If either player **exits the safe zone boundary during a match**, that player **automatically forfeits**. The match ends immediately, the opposing player is awarded the win, and both players are notified. This applies equally to the Chaser and Runner.
### Safe Zones
- Safe zones are **pre-defined geographic areas** (parks, event venues, campuses) registered in the app.
- Matches can only trigger inside active safe zones.
- Safe zones have a **host/admin** who can activate, deactivate, or configure the zone.
### Post-Match Flow
- Result is **logged** to both players' match history.
- Result is **posted to a public activity feed** (opt-out available in settings).
- If both players have **opted into ranked mode**, rated points are calculated and leaderboard standings are updated.
- Players may **rematch immediately** or dismiss.
---
 
## User Roles
 
| Role | Description |
|---|---|
| **Runner** | Randomly assigned at match trigger. Avoids being tagged for 20 seconds. |
| **Chaser** | Randomly assigned at match trigger. Attempts to tag the Runner. |
| **Observer** | A third nearby user who joins the match as the camera operator. Streams live video to both players and confirms tags via UI. |
| **Zone Admin** | Manages a designated safe zone. Can configure zone parameters and moderate local activity. |
 
---
 
## Feature Specifications
 
### 1. Proximity Detection
- **Bluetooth Low Energy (BLE)** is the sole proximity technology for both match triggering and tag confirmation. GPS handles safe zone boundary enforcement only.
- A match trigger fires when two users are within **~33 feet (~10 meters)** inside an active safe zone, as determined by BLE RSSI (Received Signal Strength Indicator).
- A tag fires when the Chaser's BLE signal to the Runner crosses a **close-range threshold** — calibrated to approximately 3–5 feet (~1–1.5 meters) — representing the Chaser entering the Runner's immediate space without requiring physical contact.
- BLE RSSI thresholds should be **device-calibrated** at first launch and periodically during zone setup to account for hardware variance across Android and iOS devices.
- Proximity is continuously monitored while the app is foregrounded inside a safe zone.
- **No NFC is used.** Contactless BLE-based tagging is a deliberate safety and accessibility choice — players do not need to touch each other or their devices together.
### 2. Match Initiation
- On proximity trigger, both phones receive a **match prompt** simultaneously.
- Roles (Chaser / Runner) are randomly assigned by the server at this moment.
- The Observer role is offered to any third user within proximity range of the safe zone.
- A **3-second countdown** precedes the start of the 20-second match clock.
### 3. Observer Video Capture
- The Observer's camera records the match and streams the feed **to the server** for the duration of the 20-second match. The server holds the video in **temporary storage** tied to the match ID.
- Video is **not streamed to the players during the match** — the Chaser and Runner are physically active and cannot watch a screen. Post-match delivery is the sole intended use.
- After the match concludes, both players receive an in-app notification: *"Your match was recorded. Save your footage before it expires in 24 hours."* Each player's save action is independent — one saving does not affect the other's copy.
- The Observer's post-match screen shows only a confirmation that footage was delivered to the players. **No save, download, share, or replay control is available to the Observer.** The Observer's device retains no local copy. This restriction establishes the correct default: the video exists for the Chaser and Runner's safety record and social sharing. OS-level screen capture is a known bypass and is considered acceptable — enforcing the intent of the design is sufficient.
- Server-side video is **permanently deleted after 24 hours**, regardless of whether players have saved it.
- If the Observer disconnects mid-match, partial footage up to the disconnection point is retained and delivered to players with a notice that the recording is incomplete.
- Observer's UI during the match overlays a single **"TAG!"** button against the live camera viewfinder. The viewfinder is display-only — it confirms the Observer is pointed at the action but is not transmitted to players in real time.
### 4. Safe Zone Boundary Enforcement During Matches
- GPS monitors both players' positions continuously throughout the 20-second match.
- If either player crosses the safe zone boundary, a **forfeit is triggered immediately** — the match clock stops and the exiting player is recorded as the loser.
- Both players receive a notification: *"[Player] left the safe zone. Match ended — [Opponent] wins by forfeit."*
- A brief **grace period of 1 second** is applied before forfeit triggers, to absorb minor GPS drift at zone edges. Sustained boundary exit beyond 1 second is treated as intentional.
- The Observer is also notified of the forfeit and their recording is finalized at that moment.
- Boundary forfeit is recorded identically to a standard loss in match history. In ranked mode, Elo adjustments apply normally.
- If GPS signal is lost for both players simultaneously (e.g., heavy tree cover), the match continues on BLE alone until GPS recovers. If GPS is lost for only one player, that player's last known position is used for up to 3 seconds before a GPS-loss warning is displayed. Match is not forfeited due to signal loss alone.
### 5. Tag Confirmation Logic
 
**Primary (Observer present):**
- Tag is confirmed when **both conditions are met**:
  1. BLE RSSI indicates Chaser is within close-range threshold of Runner (~3–5 feet)
  2. Observer taps the "TAG!" button within a ±1 second window of the BLE signal
- Dual confirmation prevents false positives from RSSI noise and accidental taps.
**Fallback (no Observer):**
- Tag is confirmed by **BLE signal strength alone** when the Chaser sustains close-range RSSI for a minimum of **0.5 seconds** — preventing instantaneous signal spikes from triggering a false tag.
- This mode is explicitly labeled on the match screen so players know the confirmation method.
**No physical contact is ever required.** The Chaser entering the Runner's close-range BLE zone constitutes a valid tag in all modes. This is a safety design principle, not a compromise.
 
If the 20 seconds expire without a confirmed tag, the **Runner wins** by survival.
 
### 6. Match Timer
- A shared synchronized **20-second countdown** is displayed on all three players' screens.
- Timer is server-authoritative to prevent drift.
- Visual and haptic pulses escalate in intensity in the final 5 seconds.
### 7. Casual Mode
- No account required beyond a display name and device ID.
- Results are stored locally and optionally posted to the public feed.
- No ranked points are issued.
- Casual matches use **raw uncalibrated BLE connection data** for proximity and tag confirmation. No calibration profile is applied or required.
- Every casual match screen displays a persistent **"UNRANKED MATCH"** notice — a non-dismissible label in the match UI that makes clear the result carries no rated weight and that signal accuracy is not calibration-verified.
- Ideal for spontaneous, walk-up play.
### 8. Competitive / Ranked Mode (Opt-In)
- Requires a registered account with verified identity.
- Requires **completed manual BLE calibration** — uncalibrated players cannot trigger ranked matches.
- Uses an **Elo-style rating system** — rating adjustments are weighted by player rating differential.
- Ranked matches require both players to have opted in before the match triggers.
- A **cooldown period** (configurable, default 60 seconds) applies between ranked matches with the same opponent to prevent farming.
- Season-based leaderboards per safe zone and globally.
**Calibration access and ranked gate:**
- The **Calibration Wizard** is accessible at any time from the player profile screen, the in-zone lobby, and the ranked opt-in settings page. It is never buried.
- When a player attempts to enable ranked mode without calibration, they receive a blocking notice: *"Ranked matches require signal calibration. Calibration takes about 90 seconds and needs one other player nearby. Casual matches are available now."* The notice contains a direct **"Calibrate Now"** action.
- When a ranked match cannot trigger because one player is uncalibrated, the initiating player sees: *"Your match partner hasn't calibrated their signal yet. This match will be unranked."* They may proceed as a casual match or wait.
- Calibration status is visible on a player's public profile as a small indicator badge — giving ranked players social context about who they are matching with.
### 9. Public Activity Feed
- Each completed match generates a feed card: player handles, result, timestamp, safe zone name.
- Players may attach a short reaction (emoji only) post-match.
- Feed is public by default; users may set their profile to private.
- No video or images are ever posted — feed is text/metadata only.
### 10. Safe Zone Management
- Zone Admins register safe zones with a name, polygon boundary (drawn on map), and operating hours.
- Admins can approve or reject Observer applications for their zone.
- NTARI or platform operators can verify zones as **Certified Safe Zones** (moderated, recurring events).
### 11. BLE Calibration Flow
 
BLE RSSI values are not universal — they vary by device model, OS version, phone case thickness, body orientation, and even how a phone is held or pocketed. Calibration is required to ensure that "33 feet" and "3–5 feet" mean the same thing across a Samsung Galaxy and an iPhone 15. There are two calibration contexts: **device onboarding** (one-time, per device) and **zone calibration** (per safe zone, performed by the Zone Admin or a trusted pair of users).
 
**Calibration and ranked mode:** Manual calibration is **required to participate in ranked matches**. A player without a manual calibration profile may play casual matches freely — those matches run on raw uncalibrated BLE data and are clearly labeled as unranked. The Calibration Wizard is always accessible from the profile screen and in-zone lobby, and is surfaced prominently whenever a player attempts to access ranked mode.
 
#### 11a. Device Onboarding Calibration (First Launch)
Triggered on first app launch or after a device OS update. Requires a second user with the app. Takes approximately 90 seconds.
 
**Tethered Distance UX:**
Distance is guided by a **live tethered indicator** — a visual rope displayed on both screens that stretches in real time as the two devices move apart, driven by live BLE RSSI. The rope transitions from red → yellow → green as the user approaches the target distance, and locks with a haptic pulse and an audio tone when the correct range is reached. Users do not estimate distance — they walk until the app tells them to stop.
 
- At the **33-foot stage**: rope stretches to full extension, turns green, and both phones pulse simultaneously. Users hold position for the scan.
- At the **3-foot stage**: users walk toward each other. The rope compresses. A distinct tone and haptic fire when tag range is reached.
The tethered indicator renders on a minimal full-screen canvas — no other UI elements compete for attention during the walk.
 
**Flow:**
1. User is prompted: *"To calibrate, you'll need one other person with the app. Stand together, then walk apart when instructed."*
2. Both users tap **"Start Calibration"** to sync the session via server handshake.
3. An animated cue instructs both users to **walk apart slowly**. The tethered distance indicator stretches in real time as RSSI weakens.
4. When BLE RSSI reaches the target trigger range, both phones lock with a haptic pulse and tone. Users hold position for a **10-second BLE scan** — the match trigger baseline is recorded.
5. Users are prompted to **walk toward each other**. The rope compresses toward the center.
6. When RSSI reaches tag range, both phones lock again. A second **10-second scan** records the tag confirmation baseline.
7. The app stores both RSSI baselines locally as a **calibration profile**, timestamped and tied to device hardware ID, and syncs to the server.
8. If a second user is unavailable, the user may skip and the app applies a **platform default profile** (pre-tuned per device model from a maintained lookup table). Skip is flagged in the player's profile. **Ranked matches are unavailable until manual calibration is completed.**
#### 11b. Zone Calibration (Zone Admin)
Performed by the Zone Admin when a safe zone is first created or its physical environment changes (new structures, seasonal foliage, etc.). Establishes a **zone-level RSSI adjustment** that is broadcast to all devices entering the zone.
 
**Flow:**
1. Zone Admin opens the safe zone settings and selects **"Calibrate Zone Signal"**.
2. Two users (Admin + one volunteer) perform the same two-distance BLE scan as onboarding calibration.
3. The zone-level delta between the scan results and the platform default is stored server-side as a **zone offset**.
4. When any device enters the zone, it receives the zone offset and applies it on top of its device calibration profile.
5. Zone calibration expires after **30 days** or when the Admin manually resets it.
#### 11c. Calibration Data Model
```
DeviceCalibrationProfile {
  device_id:           string
  platform:            'ios' | 'android'
  model_identifier:    string        // e.g. "iPhone15,2"
  trigger_rssi:        number        // RSSI at ~33ft (match trigger)
  tag_rssi:            number        // RSSI at ~3ft (tag confirmation)
  calibrated_at:       timestamp
  method:              'manual' | 'default_profile'
}
 
ZoneCalibrationOffset {
  zone_id:             string
  rssi_offset:         number        // delta applied to all device profiles in zone
  calibrated_at:       timestamp
  expires_at:          timestamp     // 30 days after calibration
  calibrated_by:       user_id
}
```
 
#### 11d. Runtime RSSI Resolution
When the app evaluates a proximity event during a match, it resolves the effective threshold as:
 
```
effective_trigger_rssi = device.trigger_rssi + zone.rssi_offset
effective_tag_rssi     = device.tag_rssi + zone.rssi_offset
```
 
The **average of both players' effective thresholds** is used as the match threshold, so neither device's calibration alone determines the outcome.
 
#### 11e. Calibration Edge Cases
| Scenario | Handling |
|---|---|
| One player has manual calibration, other has default profile | Casual: match proceeds on raw uncalibrated data; "UNRANKED MATCH" notice displayed. Ranked: blocked — uncalibrated player receives a calibration prompt; initiating player is notified and offered a casual match instead. |
| Zone offset expired | Fall back to device-only profiles; Zone Admin notified to recalibrate |
| RSSI readings are anomalous during calibration (high variance) | App prompts retry; after 2 failed attempts, defaults to platform profile |
| Device model not in lookup table | Apply conservative (wider) default thresholds; flag for manual calibration |

### 12. Harm Track — the covenant layer

WCT is a contactless proximity sport: in play, the only ways to break the rules are **physical contact** and **leaving the designated arena**. Boundary exit is already game logic (forfeit, §4). The harm track exists for what game logic cannot settle.

**What is a harm.** Not every touch. A **harm claim** asserts one of two things:
1. **Injury caused by another player's actions** — a collision from reckless play, a shove, a trip, play forced into unsafe ground; or
2. **Assaultive contact** — a touch that would otherwise be considered assault, regardless of injury.

Incidental brush-contact with no injury and no assaultive character is a game-rule matter (the tag is voided or stands; rematch if agreed) — it never becomes a reputation event.

**Mechanics (the covenant, applied minimally):**
- **No general rating system.** Players never rate each other after matches. The covenant here is at its purest: a member is in good standing, or a claim is open or adjudicated against them. Adjudicated harms are never counted into any score.
- **Filing.** A match participant or the match Observer may file a harm claim against a specific player, tied to the **match id**, with a mandatory justifying description (≤500 words). Intake is one tap from match history and is never obstructed.
- **The claim holds the record open.** The game result stands for play purposes, but the match record carries the claim lifecycle (filed → adjudicated → resolved) as annotations. Nothing is ever erased; a dismissal is an annotation with reasons, and the claimant sees it.
- **Footage hold.** A claim filed within the 24-hour footage window suspends TTL deletion for that match: the footage moves to access-controlled hold storage — hashed at ingest, chain-of-custody preserved — for the duration of the claim and any lawful process. This is the record layer doing its documentary job for the person harmed; it is never a general archive, and unheld footage still deletes on schedule.
- **Adjudication is a function, never a verdict on a person.** First-line adjudication sits with the Zone Admin — present, local, and accountable for the zone — with escalation to the operator's review. The adjudicator measures the event; outcomes attach to the claim. Zone Admin adjudication conduct is itself readable structure (open-claim dwell, dismissal patterns), inherited from the architecture's adjudication-conduct discipline.
- **Local law keeps players safe; the platform keeps the record.** Any claim describing assault or serious injury triggers the safety track: the claimant receives clear guidance to contact local law enforcement or emergency services, and the operator preserves — and can export under lawful process — the match record and held footage. Recording a claim never discharges a legal duty, and the platform never adjudicates crimes.
- **Safety precedence.** A credible claim of assaultive contact may suspend the implicated account pending review — protection now; due process governs the durability of consequences.
- **Consequences gate whether, never how much.** An adjudicated harm can suspend matchability — platform-wide or per zone — with restoration terms set at disposition. It **never adjusts Elo**: skill rating and covenant standing do not mix, and no volume of clean matches dissolves an adjudicated harm, which remains visible as an annotation on the player's record.
- **Interim honesty.** Until claims can file at an independent witness, intake and record are operator-attested, and dispositions say so (architecture open problem 2).
 
---
 
## Technical Architecture
 
### Stack
- **Frontend**: React Native (cross-platform iOS + Android)
- **Backend**: Node.js / Express API — stateless, horizontally scalable
- **Real-time**: WebSockets for match state sync and timer; RTMP or WebRTC ingest for Observer-to-server video capture
- **Video storage**: Temporary server-side storage per match (e.g., S3-compatible object storage with 24-hour TTL); video is never stored on the Observer's device and is inaccessible to the Observer post-match
- **Proximity**: BLE via `react-native-ble-plx` (match trigger + tag confirmation); GPS via `react-native-geolocation-service` (safe zone boundary only). No NFC.
- **Database**: PostgreSQL (users, zones, match history, rankings); Redis (active match state, cooldowns)
- **Auth**: OAuth 2.0 (Google / Apple Sign-In) for ranked accounts; anonymous device token for casual
### Data Privacy
- **Observer video is held temporarily and deleted.** Match footage is stored server-side for a maximum of 24 hours and deleted automatically — unless a harm claim filed within the window places it on access-controlled hold (hashed, chain-of-custody) for the claim's duration and any lawful process (§12). Players (Chaser and Runner) may save their copy within that window. The Observer has no access to the video at any point after the match ends — no save, share, or replay capability is exposed to them.
- Match metadata (players, result, timestamp, zone) is stored but not linked to device location history.
- Proximity data is processed in real time and discarded — it is never logged to a database.
- GDPR and CCPA compliant by design: users may export or delete their account data at any time.
### Key Flows
 
```
MATCH TRIGGER FLOW
──────────────────
1. Both users enter safe zone boundary (GPS check)
2. BLE scan detects mutual proximity < 33 feet (match trigger threshold)
3. Server assigns roles randomly (Chaser / Runner)
4. Observer prompt sent to nearby third user (optional)
5. 3-second countdown begins
6. Match clock starts (server-authoritative 20s)
7. Observer begins recording to server (if Observer present); viewfinder active on Observer screen
8a. WITH OBSERVER: Tag confirmed by BLE close-range threshold + Observer "TAG!" button
8b. WITHOUT OBSERVER: Tag confirmed by BLE close-range threshold sustained ≥ 0.5s
    OR timer expires → Runner wins by survival
9. Match ends → server finalizes video file → push notification sent to Chaser and Runner
10. Players have 24 hours to save footage; Observer receives no access; server deletes file at TTL
 
CALIBRATION FLOW (DEVICE ONBOARDING)
──────────────────────────────────────
1. First launch detected → calibration prompt displayed
2. Two users stand ~33 feet apart → 10s BLE scan → trigger_rssi recorded
3. Two users stand ~3 feet apart → 10s BLE scan → tag_rssi recorded
4. Calibration profile stored locally + synced to server
5. If solo: platform default profile applied, flagged as uncalibrated
 
CALIBRATION FLOW (ZONE)
─────────────────────────
1. Zone Admin initiates zone calibration in settings
2. Same two-distance scan performed within zone boundary
3. Zone offset (delta from platform default) stored server-side
4. Offset broadcast to all devices entering zone
5. Offset expires after 30 days
 
 
───────────────
1. Result logged to match history
2. Feed card generated and posted (opt-out respected)
3. If both players in ranked mode: Elo update calculated and applied
4. Rematch prompt displayed (5-second window)
```
 
---
 
## UX & Design Direction
 
### Target Operator Context
World Chase Tag is designed for deployment by **parks and recreation officials** — the primary administrators and zone owners. UX and onboarding should reflect this:
- Zone setup flows should feel familiar to program coordinators accustomed to permit-based event management.
- Safe zone registration should map naturally onto existing park boundaries (fields, courts, plazas) with simple polygon drawing tools.
- The app should be presentable in a municipal procurement context: clear data privacy documentation, AGPL-3 license transparency, and no dark patterns.
- Casual players (park visitors) need zero onboarding friction — they should be able to join a match without an account.
- Recreation officials may want aggregate usage data (matches played, active zones, participation rates) for program reporting. An operator dashboard is a candidate for v2.
### Aesthetic
- **High-contrast sport utility**: dark backgrounds, bold typography, neon accent colors (electric yellow or signal orange).
- Match screens are full-bleed, immersive — no chrome or nav bars during active play.
- Countdown and timer are the dominant visual element during a match.
- Observer view is a live camera feed with a single oversized confirmation button — zero cognitive load.
### Key UX Principles
- **Zero friction for casual play**: proximity trigger → roles assigned → go. Minimal taps.
- **Clarity under physical stress**: UI must be readable while running. Large type, high contrast, haptic feedback.
- **Ephemeral by default**: nothing is saved unless the user explicitly opts in to ranked mode or feed posting.
### Screens (Minimum Viable)
1. **Home / Safe Zone Map** — nearby active safe zones, user status (casual / ranked), recent feed
2. **In-Zone Lobby** — active users in the current zone, Observer availability indicator
3. **Match Trigger Alert** — role assignment reveal, countdown
4. **Active Match** — timer, role label, Observer stream (Chaser/Runner) OR camera + tag button (Observer)
5. **Result Screen** — winner, match duration, post-to-feed toggle, rematch prompt
6. **Profile** — match history, ranked rating, safe zone affiliations, calibration status
7. **Leaderboard** — per-zone and global, filtered by casual / ranked
8. **BLE Calibration Wizard** — two-step guided distance calibration (onboarding + re-calibration trigger)
---
 
## Open Questions & Design Decisions

> **Canonical home:** these now live in `OPEN-QUESTIONS.md` — the living open-questions deliverable required by Janus-Facing Architecture — with the constraints each resolution inherits (including two this table doesn't carry: the Elo/covenant boundary and the missing harm track). Update there; this table is a summary.

| # | Question | Status |
|---|---|---|
| 1 | What happens if no Observer is available? Does the match proceed on proximity-only tag confirmation? | **Resolved: Yes — BLE-only is the accepted fallback. Match screen labels the active confirmation mode.** |
| 2 | Should zone admins be able to disable ranked mode within their zone? | Open |
| 3 | What is the minimum number of users required in a safe zone before matches can trigger? | Open |
| 4 | Should the Observer role be mandatory or optional at match trigger? | Open |
| 5 | How should ties be handled (tag confirmed exactly at 0:00)? | Open |
| 6 | Is there an anti-cheat mechanism if the Chaser and Runner are the same physical person using two phones? | Open |
 
---
 
## Licensing & Compliance
 
### Accessibility — WCAG 2.1 Level AA (Required)
 
World Chase Tag's target market is parks and recreation officials at state and local government agencies. Under the DOJ's final rule updating ADA Title II, any mobile app deployed by or on behalf of a public entity must conform to **WCAG 2.1 Level AA**. Municipalities cannot delegate this liability to vendors — if a parks department deploys this app, they are responsible for its compliance. WCAG 2.1 Level AA conformance is therefore a **v1 release requirement**, not a post-launch consideration.
 
Compliance deadlines: municipalities with populations of 50,000+ were required to comply beginning **April 24, 2026**. Smaller jurisdictions follow on **April 26, 2027**.
 
#### What WCAG 2.1 Level AA Requires
 
WCAG 2.1 is organized around four principles — content must be **Perceivable, Operable, Understandable, and Robust** (POUR). Level AA includes all Level A criteria plus the following most relevant to a mobile app like World Chase Tag:
 
| Criterion | Requirement | World Chase Tag Implication |
|---|---|---|
| 1.1.1 Non-text Content | All non-text UI elements have text alternatives | BLE status icons, match role indicators, and timer must have accessible labels |
| 1.3.1 Info and Relationships | Structure conveyed via markup, not appearance alone | Role assignment (Chaser/Runner) must not rely on color alone |
| 1.4.1 Use of Color | Color is not the sole means of conveying information | Tethered calibration rope must use shape/label in addition to red/yellow/green |
| 1.4.3 Contrast (Minimum) | Text contrast ratio ≥ 4.5:1 (normal), 3:1 (large text) | High-contrast sport aesthetic is aligned with this — must be verified |
| 1.4.4 Resize Text | Text resizable up to 200% without loss of content | Match timer and role labels must scale without truncation |
| 1.4.11 Non-text Contrast | UI component contrast ratio ≥ 3:1 | Buttons (TAG!, Start Calibration, etc.) must meet contrast against their backgrounds |
| 2.1.1 Keyboard / Switch Access | All functionality operable without touch gestures | Match trigger and confirmation accessible via switch control (iOS) and TalkBack (Android) |
| 2.3.1 Three Flashes | No content flashes more than 3 times per second | Match countdown pulse effects must stay below flash threshold |
| 2.4.3 Focus Order | Focus order preserves meaning and operability | Modal screens (match trigger alert, result screen) must manage focus correctly |
| 3.3.1 Error Identification | Errors identified and described in text | Calibration failures, GPS loss warnings must use descriptive text, not icon-only |
| 4.1.2 Name, Role, Value | All UI components have accessible name, role, and state | All interactive elements must be correctly labeled for VoiceOver / TalkBack |
| 4.1.3 Status Messages | Status messages conveyed without focus change | "Match ended," "Tag confirmed," "Player left zone" must be announced to screen readers |
 
#### Active Match Screen — Accessibility Special Case
The active match screen is a physically demanding, real-time context. Several standard accessibility accommodations require specific treatment:
 
- **Screen reader announcements** must cover match state changes (match start, tag confirmed, forfeit, time warnings) without requiring the user to look at the screen — haptic and audio feedback serve as the primary channel during active play.
- **Motor accessibility**: the Observer's TAG! button must be large enough to meet touch target minimums (44x44pt per Apple HIG / 48x48dp per Material Design) and operable via switch access.
- **Cognitive load**: match UI must use plain language. "YOU ARE THE CHASER" not iconography alone.
#### Calibration Wizard — Accessibility
- The tethered distance indicator must not rely solely on color (red → yellow → green). Shape change, label text, and haptic/audio feedback must redundantly communicate distance stage.
- All spoken/displayed instructions must be available to screen readers.
- The two-player sync step must announce waiting state via status message.
#### Testing Requirements
Before v1 release, the app must be tested with:
- **VoiceOver** (iOS) and **TalkBack** (Android) enabled across all primary screens
- **Dynamic Type** at maximum size setting (iOS) and largest font scale (Android)
- **Switch Control** (iOS) for the Observer TAG! button and core match actions
- Color contrast verified via automated tooling (e.g., axe, Deque) on all screens
#### Known Limitations & Exemptions
WCAG 2.1 allows an exemption where compliance would cause a "fundamental alteration" to the nature of a service. The physical gameplay itself (running, chasing) is not a digital service and is not subject to WCAG. However, all app screens — including active match, calibration, and results — are digital interfaces and must comply. Parks departments should document this distinction in their accessibility conformance records.
 
---
 
### AGPL-3.0
World Chase Tag is released under the **GNU Affero General Public License v3.0 (AGPL-3.0)**. Key implications for development:
- All source code must be made publicly available, including any server-side modifications.
- Any organization deploying a modified version of the app (including as a hosted service) must release their modifications under AGPL-3.0.
- Third-party dependencies must be license-compatible with AGPL-3.0. Dependencies under MIT, Apache 2.0, and GPL are generally compatible; proprietary SDKs require review.
- The AGPL-3.0 license text must be included in the app's legal/about screen and in the repository root.
### Data & Privacy Compliance
- GDPR and CCPA compliant by design (see Data Privacy section).
- Municipal deployments may have additional public records or data retention requirements — Zone Admins in public agency contexts should be advised to consult their legal teams.
---
 
## Out of Scope (v1)
 
- In-app voice or text chat between players
- Spectator mode beyond the single Observer
- Video replay or highlight clips
- Tournament bracket management
- Payment / monetization layer
- Web client (mobile only in v1)
---
 
## Glossary
 
| Term | Definition |
|---|---|
| **Safe Zone** | A registered geographic area where matches can occur |
| **Match** | A 20-second competitive tag event between one Chaser and one Runner |
| **Observer** | A third user who streams live video and confirms tags |
| **Tag** | A contactless event confirmed by BLE close-range signal, with optional Observer corroboration |
| **Casual Mode** | Unranked, minimal-account play |
| **Ranked Mode** | Opt-in rated play with Elo scoring and leaderboards |
| **Proximity Trigger** | The BLE/GPS event that initiates a match when two users enter 33-foot range |
| **Tag Threshold** | The BLE RSSI level (~3–5 feet) that constitutes a valid contactless tag |
| **Contactless Tag** | A tag confirmed by BLE signal proximity alone — no physical contact required |
| **Match Footage** | Video recorded by the Observer and held server-side for 24 hours; accessible only to the Chaser and Runner for optional download; permanently deleted at TTL unless placed on hold by a harm claim (§12) |
| **Harm Claim** | A covenant-layer claim tied to a match id: injury caused by another player's actions, or a touch that would otherwise be considered assault. Adjudicated as an event, annotated never erased, gates matchability and never Elo (§12) |