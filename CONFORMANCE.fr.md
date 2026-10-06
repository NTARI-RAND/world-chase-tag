> Traduction communautaire (brouillon) — politique NTARI P2-002, Diffusion multilingue mondiale. Source : CONFORMANCE.md (original anglais, instantané du 2026-10-05). Brouillon communautaire assisté par machine, en attente de relecture par le mainteneur régional conformément à P2-002 §3.1. Les spécifications techniques essentielles restent en anglais conformément au §2.2.
>
> Vous avez remarqué une erreur de traduction ? N'hésitez pas à la corriger
> vous-même : forkez le dépôt https://github.com/NTARI-RAND/world-chase-tag et
> ouvrez une pull request. Les corrections de traduction sont des
> contributions précieuses, tout autant que le code.

> L'original anglais est normatif. Cette traduction est proposée pour la lecture, non pour l'interprétation : là où elle diverge de `CONFORMANCE.md`, l'anglais prévaut. Les mots-clés normatifs (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY), les noms de champs, les libellés et les identifiants sont conservés en anglais, car ils constituent le contrat de conformité.

# World Chase Tag — Autodescription de conformité

*Auto-attestée ; antérieure à la construction. Identifiants du registre : `JFA/jfa-conformance-suite.py`.*

**Positionnement :** WCT est une **instance JFA dont la forme économie-éducation s'infléchit** — une application de coordination de sport physique, non une place de marché du crédit. La forme peut s'infléchir ; les planchers, non : WCT est soumise au pacte dès sa naissance, parce qu'elle met des membres en situation de jeu physique avec des inconnus, et sa piste préjudice manquante est une **solution provisoire nommée sur une trajectoire engagée**, non une fonctionnalité facultative (OPEN-QUESTIONS #2). Elle porte une frontière du pacte déclarée :

**La frontière Elo (déclarée) :** le classement compétitif est un score d'habileté au jeu, jamais de confiance. Il conditionne uniquement l'appariement classé ; il n'apparaît jamais comme réputation de conduite et n'alimente jamais la modération, le crédit ni la position en matière de gouvernance. Si cette frontière cède, COV-1 cède avec elle (OPEN-QUESTIONS #1).

| Identifiant / discipline | Statut |
|---|---|
| GOV-9 document vivant de questions ouvertes | **Met** — `OPEN-QUESTIONS.md` est livré avec ce document fondateur |
| GOV-10 communs copyleft | **Met** — AGPL-3.0 déclarée, règle de compatibilité des dépendances incluse |
| Frontière COV-1/COV-4 | **Declared** — Elo ≠ réputation ; à faire respecter lors de la construction |
| Couche Pacte (préjudice) | **Designed** (spécification §12) — pacte fondé uniquement sur les réclamations : préjudice = blessure causée par une action, ou contact agressif ; l'arbitrage mesure l'événement ; les conséquences conditionnent la possibilité d'être apparié, jamais l'Elo ; construction engagée en même temps que le jeu public (OQ-2) |
| Discipline REC | **Partial** — l'historique des matchs est attesté par le serveur (OQ-4) ; la vidéo éphémère avec un TTL de 24 h et la non-conservation par l'Observer relèvent d'une conception délibérée privilégiant la vie privée, avec une conservation des images en cas de réclamation pour préjudice (hachées, avec chaîne de traçabilité) comme exception documentaire (§12) |
| Discipline PII | **Met by design** — données de proximité traitées puis supprimées, métadonnées dissociées de l'historique de localisation, export et suppression conformes au GDPR/CCPA |
| Lisibilité | **Partial** — le mode de confirmation signalé à l'écran ("UNRANKED MATCH", avis BLE uniquement) a la forme d'un reçu de décision ; WCAG 2.1 AA est exigé pour la v1 |

**Ligne d'honnêteté :** tout ce qui figure ici est du texte de spécification ; rien n'est construit ni attesté par témoin. Les lacunes connues sont la piste préjudice et le registre classé auto-attesté.
