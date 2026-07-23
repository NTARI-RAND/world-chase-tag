# World Chase Tag — Open Questions

*The living open-questions deliverable required by Janus-Facing Architecture (standard check 7). Reviewed and re-shipped with each release; staleness is nonconformance. An entry leaves only by resolution or by an explicit values call made by the people who live with the outcome. Invariant IDs cite the registry in `JFA/jfa-conformance-suite.py`. This document is the canonical home of the spec's open-questions table.*

## 1. Elo is a score — where is the covenant boundary?
**Problem.** Ranked mode uses an Elo-style composite rating and leaderboards. The covenant forbids averaging reputation into a score (COV-1).
**Constraints inherited.** The defensible line: Elo is a *game-skill* rating, not trust — conformant only while it (a) never gates anything except ranked matchmaking and display, (b) is never presented as reputation-of-conduct, and (c) never feeds moderation, credit, or governance standing (COV-4: reputation gates whether, never how much — and skill isn't reputation at all). That boundary must be encoded in code and copy, not assumed.
**Status.** Open — boundary declared here; enforce at build.

## 2. The harm track — design direction set (spec §12); details open
**Problem.** Direction recorded 2026-07-22 and specced at §12: in a contactless proximity sport the only rule breaks are contact and boundary exit; a *harm* is narrower — injury caused by another player's actions, or a touch that would otherwise be considered assault. WCT runs a claims-only covenant (no general ratings); adjudication is a function measuring the event; local law keeps players safe while the record layer provides the documentation (including the footage hold). What remains is the machinery around that design.
**Constraints inherited.** Harm surfaced, never averaged, never mixed into Elo (COV-1/COV-2 and the declared boundary); claims answerable, dismissals annotated (COV-3, REC-2) — answerability is what keeps the covenant measuring the solution rather than judging the person; safety precedence with due process governing durability; operator-attested intake named as such until independent-witness filing exists (problem 2).
**Sub-questions still open.** Zone Admin escalation path and the adjudication-conduct read on admins themselves (dwell, dismissal patterns); footage-hold duration, custody, and access policy; suspension and restoration terms; minors playing in public parks (guardian consent, municipal policy); whether the Observer owes any duty to report what they filmed.
**Status.** Direction set; build committed with public play; sub-questions open.

## 3. Anti-cheat and the two-phones Sybil (spec table #6)
**Problem.** One person running Chaser and Runner on two phones farms ranked points.
**Constraints inherited.** MSP posture — make gaming cost more than it pays rather than pretend to prevent it; the cooldown is one dial. Collusive match-farming is the wash-trade / too-clean-signature analog: detectable as structure (same pairs, same zones, improbable cadence), flagged for contest, never auto-punished (flag-never-finding).
**Status.** Open.

## 4. Server-attested results
**Problem.** Match results, rankings, and history live in one operator's Postgres; the server is authoritative for the clock, the roles, and the outcome. Every ranked standing is self-attested by the platform (the architecture's problem 2, in miniature).
**Constraints inherited.** For a game this may be an acceptable values call — but it must be called, and labeled, not assumed; if ranked integrity ever matters competitively (sponsored seasons), witnessing or at least an exportable, hash-chained match log is the shape.
**Status.** Open — currently a named stand-in.

## 5. Remaining spec-table decisions
**Problem.** Zone admins disabling ranked mode in their zone (#2); minimum users before matches trigger (#3); Observer mandatory vs. optional (#4); ties at exactly 0:00 (#5).
**Constraints inherited.** Product calls; none bind architecturally. #2 touches operator sovereignty over their own zone — the federating instinct says yes.
**Status.** Open.
