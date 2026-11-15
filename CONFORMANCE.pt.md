> Tradução comunitária (rascunho) — Política NTARI P2-002, Transmissão Multilíngue Global (Global Multilingual Broadcast). Fonte: CONFORMANCE.md (original em inglês, snapshot de 2026-10-05). Rascunho comunitário assistido por máquina, pendente de revisão por mantenedor regional conforme P2-002 §3.1. As especificações técnicas centrais permanecem em inglês conforme §2.2.
>
> Notou algum erro nesta tradução? Correções de tradução são contribuições
> valiosas e muito bem-vindas: faça um fork do repositório e abra um pull
> request em https://github.com/NTARI-RAND/world-chase-tag.

> O original em inglês é normativo. Esta tradução é oferecida para leitura, não para interpretação: onde ela divergir de `CONFORMANCE.md`, prevalece o inglês. As palavras-chave normativas (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY), os nomes de campos, os rótulos e os identificadores são mantidos em inglês porque constituem o contrato de conformidade.

# World Chase Tag — Autodescrição de conformidade

*Autoatestada; pré-construção. IDs do registro: `JFA/jfa-conformance-suite.py`.*

**Postura:** o WCT é uma **instância da JFA cuja forma de economia-educação se flexiona** — um aplicativo de coordenação de esporte físico, não um mercado de crédito. A forma pode se flexionar; os pisos não: o WCT está sob o pacto desde o nascimento porque coloca membros em jogo físico com desconhecidos, e sua trilha de danos ausente é uma **solução provisória nomeada em um caminho com compromisso assumido**, não um recurso opcional (OPEN-QUESTIONS #2). Ele carrega um limite declarado do pacto:

**O limite do Elo (declarado):** a classificação ranqueada é uma pontuação de habilidade no jogo, nunca confiança. Ela controla apenas o pareamento ranqueado; nunca aparece como reputação de conduta e nunca alimenta a moderação, o crédito ou a posição na governança. Se esse limite se romper, o COV-1 se rompe junto (OPEN-QUESTIONS #1).

| ID / disciplina | Status |
|---|---|
| GOV-9 documento vivo de questões em aberto | **Atendido** — `OPEN-QUESTIONS.md` acompanha esta semente |
| GOV-10 commons copyleft | **Atendido** — AGPL-3.0 declarada, regra de compatibilidade de dependências incluída |
| Limite COV-1/COV-4 | **Declarado** — Elo ≠ reputação; aplicar na construção |
| Camada de pacto (danos) | **Projetado** (especificação §12) — pacto somente de alegações: dano = lesão por ação ou toque agressivo; a adjudicação mede o evento; as consequências controlam a elegibilidade para partidas, nunca o Elo; construção assumida como compromisso junto com o jogo público (OQ-2) |
| Disciplina REC | **Parcial** — o histórico de partidas é atestado pelo servidor (OQ-4); o vídeo efêmero com TTL de 24 h e a não retenção pelo Observer são um design deliberado de privacidade em primeiro lugar, com a retenção de gravações em caso de alegação de dano (com hash, cadeia de custódia) como a exceção documental (§12) |
| Disciplina de PII | **Atendido por design** — dados de proximidade processados e descartados, metadados desvinculados do histórico de localização, exportação e exclusão conforme GDPR/CCPA |
| Legibilidade | **Parcial** — o modo de confirmação rotulado na tela ("UNRANKED MATCH", avisos somente BLE) tem a forma de um recibo de decisão; WCAG 2.1 AA exigido para a v1 |

**Linha de honestidade:** tudo aqui é texto de especificação; nada está construído nem testemunhado. As lacunas conhecidas são a trilha de danos e o registro ranqueado autoatestado.
