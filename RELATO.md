# RELATO — esteira saas-hasner

_Atualizado 16/09 09:37. Publico: so ids e contagens, nunca nome/CPF, nenhum codigo._

## PLACAR (15/09 — sem mudanca)

| contador | valor | esperado |
|---|---|---|
| parados_esperando_corte | 3 | fila do Ronald |
| fatias_esperando_smoke | 15 | fila do Ronald |
| contratos_estruturais | 8/22 | 22/22 |

## FATIAS DE 16/09

**P7.1-MARCO-SEM-HORA** (turno) — NO AR 09:1x (commit 9177d8dd)
- Bug da S3 fatia 4: marco de intermitente sem hora (so rotulo) derrubava a derivacao do tipo da batida; o auditor diario de invariantes caiu em prod 07:18 e o ensaio (sombra) de 16/09 saiu FALHOU.
- 7 perguntas afetadas (3385, 3673, 5509, 6149, 6415, 6911, 8736), todas ja validadas: nenhum chamado mudou de estado.
- RED 3 erros → suite 6.793 verde → sombra refeita com a cura OK (erro 0) → DIFF folha 0 (TXT=0 RETIDOS=0) → deploy OK.
- Auditoria de invariantes de 16/09 re-rodada em prod 09:11 (aval): rodou ate o fim, rc=2 = alarme de negocio (13 invariantes violadas, 60 OK), nao mais erro.

**SLA-PELA-FILA** (chamado) — NO AR 09:34 (commit 14d00c65)
- O alerta de prazo passa a ver a fila inteira (aberto + em analise); o prazo tem um juiz so, lido pela tela e pelo cron.
- Passivo sem rajada: marco de corte gravado 09:35; 1.203 vencidos (260 sem aviso) viraram Pauta DP 89/90/91 (emp 2: 898, emp 3: 270, emp 4: 35), nunca push. Push so para vencimento novo, ate 20 por passada.
- Contador sla_vencido_sem_aviso = 260 (dono supervisao/DP).
- 1a cadeia caiu na regua (faltava um teste na lista da cadeia); corrigida e relancada.

**UI-FIO-DO-COLAB** — pauta registrada (evidencia de 28 dias do propositor; a ficha do copiloto mora no core).

## EM SEGUIDA

- **COMPETENCIAS-PAGAS-SEM-TRANCA** — contador; testes rodando.
- **Medida da re-lavra 16/09** — roda sozinha com a sombra OK.
- **ADESAO sitio 2** — teste corrigido: RED 3 falhas, GREEN verde.

## PARADOS

- T4-MARCO-QUE-A-BATIDA-OCUPA — DIFF TXT=9 RETIDOS=47 → Pauta DP 83/84/85.
- TURNO-F1-S3 / P71-ANTECIPACAO-12X36 — corte Ronald.

## TRANCA DAS COMPETENCIAS (16/09, aval Ronald)

**REGRA**: mes PAGO (ou exportado) tranca pela porta; os "de fora" (conta vazia, turno aberto, 12x36 sem ancora) NAO barram e ficam listados na trilha. Contador **competencias_pagas_sem_tranca** (esperado 0, dono DP): hoje **0**.

| emp | comp | marco | de fora (na trilha) | encerramento aplicado |
|---|---|---|---|---|
| 2 | 08/2026 | paga | 193 (41 vazias, 152 turno aberto) | 1.795 (407 chamados, 1.366 perguntas, 22 disputas) |
| 3 | 07/2026 | exportada | 70 (23, 47) | 65 (31, 34) |
| 3 | 08/2026 | paga | 61 (17, 44) | 323 (92, 229, 2) |
| 4 | 08/2026 | paga | 16 (4, 12) | 113 (32, 81) |

Ficam com o admin (resposta do colaborador sem veredito, nada foi apagado): emp 2/08 18 chamados e 158 perguntas; disputa 3421 segue aberta ate a pergunta 21042 ter veredito.

**emp 2 07/2026 — NAO trancada, espera DP**: competencia anterior ao rollout da emp 2 (136 batidas de 37 colaboradores contra 20.188 de 375 em 08; nenhuma exportacao nem holerite no sistema). O "aprovada" vem de UMA aprovacao de conta com 0 h em 31/07 (fechamento 2781). A folha de 07 saiu fora do sistema. Proposta: DP confirma e ela tranca pela porta com trilha "anterior ao rollout" (7 acoes); a 06/2026 esta no mesmo caso.

emp 4 07/2026 ja trancada (15/09).

## ONDE O DP TRANCA A COMPETENCIA (proximos meses)

- Tela **Fechamento**, escolher mes/ano/empresa, botao **Aprovar** do lote. Nao existe botao so de "trancar".
- O botao aprova os abertos e so TRANCA quando ninguem fica de fora. Ficam de fora (e a tranca nao acontece): conta vazia (0 h), turno aberto, 12x36 sem ancora. A mensagem avisa "Periodo NAO bloqueado" e lista quem ficou de fora.
- Ao trancar, o sistema encerra sozinho os chamados e perguntas dos dias da competencia.
- Mes ja PAGO com gente de fora: o botao nao tranca; pedir a tranca pela porta (regra acima).
- Ordem certa: aprovar → exportar TXT → TRANCAR → publicar holerite.

## ESPERANDO RONALD

- emp 2 07/2026 (e 06/2026): confirmar com o DP que a folha saiu fora do sistema.
- AMOSTRA-QUINTA-17-09 e caso do admin (adicional noturno): esperam os nomes.
- Pauta DP 89/90/91: o DP decide responder em lote ou arquivar o passivo do SLA.
- CONGELAMENTO de dinheiro: qua 16/09 18:00 → qui 17/09 14:00.
