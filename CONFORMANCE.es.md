> Traducción comunitaria (borrador) — Política P2-002 de NTARI, Difusión Multilingüe Global. Fuente: CONFORMANCE.md (original en inglés, instantánea del 2026-10-05). Borrador comunitario asistido por máquina, pendiente de revisión por mantenedores regionales conforme a P2-002 §3.1. Las especificaciones técnicas centrales permanecen en inglés conforme a §2.2.
>
> ¿Encontraste un error en esta traducción? Tu corrección es una contribución
> bienvenida y valorada: haz un fork del repositorio y abre un pull request en
> https://github.com/NTARI-RAND/world-chase-tag.

> El original en inglés es normativo. Esta traducción se ofrece para lectura, no para interpretación: donde difiera de `CONFORMANCE.md`, prevalece el inglés. Las palabras clave normativas (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY), los nombres de campos, las etiquetas y los identificadores se mantienen en inglés porque constituyen el contrato de conformidad.

# World Chase Tag — Autodescripción de conformidad

*Autodeclarada; previa a la construcción. IDs de registro: `JFA/jfa-conformance-suite.py`.*

**Postura:** WCT es una **instancia de JFA cuya forma de economía-educación se flexiona** — una aplicación de coordinación de deporte físico, no un mercado de crédito. La forma puede flexionarse; los pisos no: WCT está bajo el pacto desde su nacimiento porque pone a los miembros en juego físico con desconocidos, y su vía de daños faltante es una **medida provisional nombrada en un camino comprometido**, no una función opcional (OPEN-QUESTIONS #2). Lleva un límite del pacto declarado:

**El límite de Elo (declarado):** la calificación clasificatoria es una puntuación de habilidad de juego, nunca de confianza. Condiciona únicamente el acceso al emparejamiento clasificatorio; nunca aparece como reputación de conducta y nunca alimenta la moderación, el crédito ni la posición en la gobernanza. Si ese límite se rompe, COV-1 se rompe con él (OPEN-QUESTIONS #1).

| ID / disciplina | Estado |
|---|---|
| GOV-9 documento vivo de preguntas abiertas | **Met** — `OPEN-QUESTIONS.md` se entrega con esta semilla |
| GOV-10 bien común copyleft | **Met** — AGPL-3.0 declarada, regla de compatibilidad de dependencias incluida |
| Límite COV-1/COV-4 | **Declared** — Elo ≠ reputación; aplicar en la construcción |
| Capa de pacto (daño) | **Designed** (especificación §12) — pacto solo de reclamaciones: daño = lesión por acción o contacto constitutivo de agresión; la adjudicación mide el evento; las consecuencias condicionan la posibilidad de emparejamiento, nunca el Elo; construcción comprometida junto con el juego público (OQ-2) |
| Disciplina REC | **Partial** — el historial de partidas está atestiguado por el servidor (OQ-4); el video efímero con TTL de 24 h y la no retención por parte del Observer son un diseño deliberado que prioriza la privacidad, con una retención de grabaciones ante reclamaciones de daño (con hash, con cadena de custodia) como excepción documental (§12) |
| Disciplina de PII | **Met by design** — los datos de proximidad se procesan y se descartan, los metadatos se desvinculan del historial de ubicación, exportación y eliminación conforme a GDPR/CCPA |
| Legibilidad | **Partial** — el modo de confirmación etiquetado en pantalla ("UNRANKED MATCH", avisos de solo BLE) tiene forma de recibo de decisión; WCAG 2.1 AA es obligatorio para v1 |

**Línea de honestidad:** todo lo que aquí figura es texto de especificación; nada está construido ni presenciado por testigos. Las brechas conocidas son la vía de daños y el libro mayor clasificatorio autodeclarado.
