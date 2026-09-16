# RELATO — esteira saas-hasner

_Atualizado 16/09 09:08. Publico: so ids e contagens, nunca nome/CPF, nenhum codigo._

## PLACAR (15/09 — sem mudanca)

| contador | valor | esperado |
|---|---|---|
| parados_esperando_corte | 3 | fila do Ronald |
| fatias_esperando_smoke | 15 | fila do Ronald |
| contratos_estruturais | 8/22 | 22/22 |

## FATIAS DE 16/09

**P7.1-MARCO-SEM-HORA** (turno) — EM CURSO
- Bug da S3 fatia 4: marco de intermitente sem hora (so rotulo) derrubava a derivacao do tipo da batida; o auditor diario de invariantes caiu em prod 07:18 e o ensaio (sombra) de 16/09 saiu FALHOU.
- 7 perguntas afetadas (3385, 3673, 5509, 6149, 6415, 6911, 8736), todas ja validadas: nenhum chamado mudou de estado.
- RED 3 erros → GREEN parcial 588 → suite 6.793 verde. Sombra refeita com a cura: OK (erro 0). Seguem DIFF da folha, regua, push e deploy; so sobe antes das 16:00.

## TRAVADAS ATRAS DA P7.1

- **SLA-PELA-FILA** — suite verde (6.798); relanca sozinha depois da P7.1.
- **Medida da re-lavra 16/09** — roda sozinha com a sombra OK.
- **ADESAO sitio 2** — teste corrigido: RED 3 falhas, GREEN verde; entra depois do SLA.

## PARADOS

- T4-MARCO-QUE-A-BATIDA-OCUPA — DIFF TXT=9 RETIDOS=47 → Pauta DP 83/84/85.
- TURNO-F1-S3 / P71-ANTECIPACAO-12X36 — corte Ronald.

## TRANCA DAS COMPETENCIAS — ESPERA DECISAO

Nenhuma trancaria pelo botao hoje (ha colaboradores de fora em todas). DRY do encerramento que a tranca dispara:

| emp | comp | marco | de fora do botao | encerramento projetado |
|---|---|---|---|---|
| 2 | 07/2026 | aprovada, nao exportada | 458 vazios, 1 turno aberto | 7 acoes |
| 2 | 08/2026 | paga sem trancada | 41 vazios, 152 turnos abertos | 1.807 acoes |
| 3 | 07/2026 | exportada | 23 vazios, 47 turnos abertos | 65 acoes |
| 3 | 08/2026 | paga sem trancada | 17 vazios, 44 turnos abertos | 326 acoes |
| 4 | 08/2026 | paga sem trancada | 4 vazios, 12 turnos abertos | 114 acoes |

emp 4 07/2026 ja trancada (passivo aplicado 15/09).

## ONDE O DP TRANCA A COMPETENCIA (proximos meses)

- Tela **Fechamento**, escolher mes/ano/empresa, botao **Aprovar** do lote. Nao existe botao so de "trancar".
- O botao aprova os abertos e so TRANCA quando ninguem fica de fora. Ficam de fora (e a tranca nao acontece): conta vazia (0 h), turno aberto, 12x36 sem ancora. A mensagem avisa "Periodo NAO bloqueado" e lista quem ficou de fora.
- Ao trancar, o sistema encerra sozinho os chamados e perguntas dos dias da competencia.
- Ordem certa: aprovar → exportar TXT → TRANCAR → publicar holerite.

## ESPERANDO RONALD

- Decisao da tranca (tabela acima).
- Auditoria de invariantes de 16/09: aval dado; re-roda em prod depois do deploy da P7.1.
- AMOSTRA-QUINTA-17-09 e caso do admin (adicional noturno): esperam os nomes.
- Pauta DP do passivo do SLA, depois do deploy da SLA-PELA-FILA.
- CONGELAMENTO de dinheiro: qua 16/09 18:00 → qui 17/09 14:00.
