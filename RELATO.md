# RELATO — esteira saas-hasner

_Atualizado 16/09 11:44. Publico: so ids e contagens, nunca nome/CPF, nenhum codigo._

## PLACAR

| contador | valor | esperado |
|---|---|---|
| parados_esperando_corte | 3 | fila do Ronald |
| fatias_esperando_smoke | 15 | fila do Ronald |
| contratos_estruturais | 8/22 | 22/22 |
| competencias_pagas_sem_tranca | 0 | 0 (dono DP) |
| sla_vencido_sem_aviso | 260 | 0 (dono supervisao/DP) |
| furos_vetados_por_regua | 4 em 3 vinculos | 0 (dono DP/cadastro) |
| chamados_em_competencia_trancada | 0 | 0 (dono sistema) |

## FATIAS NO AR EM 16/09

**P7.1-MARCO-SEM-HORA** (turno) — 09:1x
- Marco de intermitente sem hora derrubava a derivacao do tipo e o auditor diario (7 perguntas, nenhuma mudou de estado).
- Suite 6.793 verde, sombra refeita com a cura OK, DIFF folha 0. Auditoria de invariantes re-rodada em prod: rc=2 (alarme de negocio, 13 violadas), nao mais erro.

**SLA-PELA-FILA** (chamado) — 09:34
- O alerta de prazo ve a fila inteira; um juiz so para tela e cron.
- Sem rajada: marco de corte gravado 09:35; passivo de 1.203 vencidos (260 sem aviso) virou Pauta DP 89/90/91, nunca push.

**COMPETENCIAS-PAGAS-SEM-TRANCA** (fechamento) — 09:5x
- Contador no placar: mes pago sem tranca. Hoje 0.

**VETO-QUE-CAI-REJULGA** (turno, bug achado) — 10:27
- Furo vetado pela regua ficava preso para sempre depois que o veto caia; agora a celula e rejulgada.
- Porta nova para desfazer a confirmacao de "cadastro errado" (motivo obrigatorio, trilha, rejulga na hora). Tela da porta: fila de TELA.

**HAIKU-BUSCA-PESSOA** (copiloto, pedido Ronald) — NO AR 11:12 (commit 25675b48)
- A busca de pessoa acha por nome parcial, sem acento e sem ordem. Vale para o copiloto e para as telas.
- 1 resultado = responde; varios = lista para escolher; zero = "nao achei ninguem com esse nome", nunca "em dia".
- Golden +3 (um, varios, zero). Conferido em prod: "simoes samyra" = 1, "Silva" = 150, nome inexistente = 0.

**TRANCA-SEM-CADASTRO** (chamado, bug achado) — NO AR 10:55; chamados_em_competencia_trancada = 0
- A tranca de 09:12 encerrou 73 avisos de cadastro (o dia deles e o inicio do defeito, que segue vivo); o emissor recriou 50 em 2 min e reabriu 1.
- Passivo com "!": 72 avisos reabertos, 48 gemeos superados no original.
- Cura: a tranca deixa de fora o aviso de cadastro e a revisao de desligamento (corte Ronald 16/09: ato do DP com dinheiro, fica ate o DP resolver); o emissor nao reabre chamado de dia trancado.
- Passivo da revisao de desligamento (aval Ronald 10:59): 38 reabertos (emp 2 27, emp 3 7, emp 4 4; 34 de colaboradores ja desligados), trilha "reaberto: tranca nao encerra cadastro/desligamento". Vivos 1.873 -> 1.911; fila 1.312 -> 1.318 (a maioria continua no arquivo). Segunda passada 0.

**ADESAO-AGRUPA-SO-VIVO** (chamado, sitio 2) — NO AR 11:35 (commit 419eb885)
- O agrupamento da adesao nao reagrupa chamado encerrado (quem esta vivo e pergunta do motor).

## EM CURSO

**CALENDARIO-UM-JUIZ** (tela, bug achado, corte Ronald) — suite e DIFF rodando desde 11:36; sobe sozinha se verde e antes das 16:00
- Tela, rodape e porta curta do calendario leem UM juiz; o tipo do dia vem da lei de precedencia (ausencia e feriado vencem a grade).
- "Extra" deixou de ser status: vira selo HE e so acende quando a hora extra conta (acima de 10 min no dia). Dia de folga com batida = "Trabalhou na folga".
- Medido na sombra (14.326 dias): 2.535 mudam; 1.574 eram "extra" indevido; 126 folgas com batida pintavam ok verde.

**BALAO-DO-ADMIN** (chamado, corte Ronald) — construida; sobe depois do calendario
- O balao do icone de chamados mostrava 836 = "toques" (outra derivacao, sem pilula correspondente).
- Passa a mostrar o que exige ato do ADMIN, lido das mesmas pilulas: Validar + Decidir (738 as 11:43). Cobrar e do colaborador e nao entra.
- Contraprova no selo: balao = soma das duas pilulas. RED 3 falhas + 1 erro; GREEN 1.245 verdes.

## TRANCA DAS COMPETENCIAS (16/09, aval Ronald)

**REGRA**: mes PAGO (ou exportado) tranca pela porta; os "de fora" (conta vazia, turno aberto, 12x36 sem ancora) nao barram e ficam listados na trilha.

| emp | comp | marco | de fora (na trilha) | encerramento aplicado |
|---|---|---|---|---|
| 2 | 08/2026 | paga | 193 | 1.795 |
| 3 | 07/2026 | exportada | 70 | 65 |
| 3 | 08/2026 | paga | 61 | 323 |
| 4 | 08/2026 | paga | 16 | 113 |

Pilula (fila de trabalho): 1.621 -> 1.345. Dos 562 chamados encerrados: 230 estavam em cobrar, 51 em decidir, 170 arquivados, 108 registrados.
Ficam com o admin (resposta do colaborador sem veredito): emp 2/08 18 chamados; emp 3/07 4; emp 3/08 11; emp 4/08 2.

**emp 2 07/2026 e 06/2026 — NAO trancadas**: anteriores ao rollout da emp 2 (folha fora do sistema). Espera o DP confirmar.

## CONCILIACAO DO 837 (pedido Ronald 16/09)

Nenhum recorte da fila da exatamente 837, nem agora nem antes da tranca (fonte unica: pilulas por verbo e painel, por empresa). O mais proximo e a pilula **Cobrar** (todas as empresas): 830 as 04:00 (antes da tranca) e **574 agora**. Dos 562 encerrados pela tranca, 230 estavam em Cobrar.

A fila de trabalho agora (1.321) = Validar 151 + Cobrar 574 + Decidir 586 + historico 4 + sem verbo 6. Arquivados (171 na pilula) e registros (429) ficam fora da fila.

| todas as empresas | antes (04:00) | agora |
|---|---|---|
| fila (aberto + em analise) | 1.621 | 1.321 |
| Validar | 156 | 151 |
| Cobrar | 830 | 574 |
| Decidir | 630 | 586 |
| arquivados (pilula) | 281 | 171 |

Por empresa, fila antes -> agora: emp 2 1.182 -> 988; emp 3 377 -> 294; emp 4 62 -> 39. Falta saber qual tela mostrou 837 para fechar a conta.

## ONDE O DP TRANCA A COMPETENCIA (proximos meses)

- Tela **Fechamento**, escolher mes/ano/empresa, botao **Aprovar** do lote. Nao existe botao so de "trancar".
- O botao aprova os abertos e so TRANCA quando ninguem fica de fora (conta vazia, turno aberto, 12x36 sem ancora).
- Mes ja PAGO com gente de fora: o botao nao tranca; pedir a tranca pela porta (regra acima).
- Ao trancar, o sistema encerra sozinho os chamados e perguntas dos dias da competencia (avisos de cadastro ficam).
- Ordem certa: aprovar → exportar TXT → TRANCAR → publicar holerite.

## CASOS

- **colab 49**: 23/08 e 12/09 sao furos vetados (cadastro confirmado como errado em 08/09). Corte: a sequencia 24/08-04/09 foi cobertura. Pergunta a supervisao na Pauta 92; depois: declarar a cobertura, desfazer a confirmacao, resolver o aviso 17652, rejulgar.
- **colab 901, 15/09**: dia cumprido (720/720 min). A tela pinta "extra" por 10 segundos de hora extra (bug de tela na fila). O 14/09 segue cobrado: a colaboradora contestou ("era folga") e espera validacao do admin — ou o DP antecipa a nova escala para 14/09.
- **Medida da re-lavra 16/09**: fechou. Os 2 casos batem com a simulacao; das 265 diferencas sem causa, 253 sao o dia a mais de batidas; 12 ficam para autopsia.

## PARADOS

- T4-MARCO-QUE-A-BATIDA-OCUPA — DIFF TXT=9 RETIDOS=47 → Pauta DP 83/84/85.
- TURNO-F1-S3 / P71-ANTECIPACAO-12X36 — corte Ronald.
- **HE 12x36 sem tolerancia de 10 min/dia** (bug achado) — cura so DEPOIS do export de 09. Pauta DP 93/94/95: 08/2026 paga com 36,3 h (emp 2), 21,5 h (emp 3), 1,0 h (emp 4) de HE que a CLT manda ignorar.

## FILA

- Tela do calendario: status "extra" com rotulo errado e dois juizes para o mesmo dia.
- Copiloto: legenda do calendario; recusa honesta sem porta por colaborador; dia do colaborador na ficha (que mora no core).
- UI do fio do colaborador (cabecalho + card da proposta de escala, evidencia de 28 dias).

## ESPERANDO RONALD / DP

- DP: Pautas 89/90/91 (passivo do SLA), 93/94/95 (HE 12x36), 83/84/85 (T4).
- DP: emp 2 06 e 07/2026 — a folha saiu fora do sistema?
- Supervisao: Pauta 92 (colab 49 foi cobertura?).
- Admin: validar a contestacao de 14/09 do colab 901.
- CONGELAMENTO de dinheiro: qua 16/09 18:00 → qui 17/09 14:00.
