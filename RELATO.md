# RELATO — esteira saas-hasner

_Estado de 16/09 13:56. Publico: so ids e contagens, nunca nome/CPF, nenhum codigo._

## PLACAR

| contador | valor | esperado | dono |
|---|---|---|---|
| parados_esperando_corte | 3 | fila do Ronald | Ronald |
| fatias_esperando_smoke | 15 | fila do Ronald | Ronald |
| contratos_estruturais | 8/22 | 22/22 | Code |
| balao de chamados (Validar + Decidir) | 711 | — | admin |
| fila de trabalho (aberto + em analise) | 1.321 | — | admin/colab |
| competencias_pagas_sem_tranca | 0 | 0 | DP |
| chamados_em_competencia_trancada | 0 | 0 | sistema |
| sla_vencido_sem_aviso | 260 | 0 | supervisao/DP |
| furos_vetados_por_regua | 4 (3 vinculos) | 0 | DP/cadastro |

## NO AR HOJE (16/09) — 12 fatias

| hora | fatia | o que mudou |
|---|---|---|
| 09:1x | P7.1-MARCO-SEM-HORA (bug) | marco de intermitente sem hora derrubava o auditor diario; 7 perguntas, nenhuma mudou de estado. Auditoria re-rodada em prod: alarme de negocio (13 invariantes), nao mais erro |
| 09:34 | SLA-PELA-FILA | o alerta de prazo ve a fila inteira. Sem rajada: passivo de 1.203 vencidos virou Pauta DP 89/90/91 |
| 09:5x | COMPETENCIAS-PAGAS-SEM-TRANCA | contador de mes pago sem tranca (hoje 0) |
| 10:27 | VETO-QUE-CAI-REJULGA (bug) | furo vetado pela regua deixa de ficar preso quando o veto cai; porta para desfazer a confirmacao de "cadastro errado" |
| 10:55 | TRANCA-SEM-CADASTRO (bug) | a tranca nao encerra aviso de cadastro nem revisao de desligamento; o emissor nao reabre dia trancado |
| 11:12 | HAIKU-BUSCA-PESSOA | busca de pessoa por nome parcial, sem acento e sem ordem; zero resultado = "nao achei", nunca "em dia" |
| 11:35 | ADESAO-AGRUPA-SO-VIVO | o agrupamento da adesao nao reagrupa chamado encerrado |
| 11:56 | CALENDARIO-UM-JUIZ (bug) | tela e rodape leem um juiz; "extra" virou selo HE (so acima de 10 min); dia de folga com batida = "Trabalhou na folga" |
| 12:14 | BALAO-DO-ADMIN | o balao mostra Validar + Decidir (739); antes mostrava 836 "toques" |
| ~12:30 | C1-EMISSORES-VIVOS | aceite de escala e pedido de autorizacao perguntam ao motor quem esta vivo; registro da familia chamado 54 -> 52 |
| 13:0x | FILA-ROTEIA-CATALOGO (bug) | chamado sem dia por natureza sai da gaveta "carimbar o dia": revisao de vinculo e aviso de cadastro vao para "revisao de cadastro" (supervisao, um toque por colaborador); modulo sem dia pela lei tem gaveta propria |
| 13:3x | VALIDACAO-RESPEITA-TRANCA (bug) | validar nao planta batida em competencia trancada; a disputa que nao fecha nao derruba quem validou; o lote classe A nao lista dia trancado |

Todas com suite verde, regua e deploy OK; as de dinheiro com DIFF de folha 0.

**Em curso**: FICHA-DO-COLAB-NO-CORE (copiloto/tela). A 1a suite parou num contrato: a ficha julgava "resposta sem veredito" lendo o carimbo direto. Agora pergunta ao juiz da disputa; suite rodando de novo desde 14:1x. Uma porta para o copiloto, o fio e a ficha completa: vinculo em uma linha, lotacao, app, horas (do espelho), pendencias (das pilulas), proposta de escala e acoes, das que mais resolvem para as que menos. Telefone so na tela.

## ATOS EM PROD HOJE (com aval)

- **Tranca pela porta** (emp 2/08, 3/07, 3/08, 4/08), com a lista dos "de fora" na trilha. Encerramento aplicado: 1.795 + 65 + 323 + 113. Regra: mes pago ou exportado tranca pela porta; quem ficou de fora nao barra.
- **Reparo da tranca**: 72 avisos de cadastro e 38 revisoes de desligamento reabertos; 48 gemeos superados no original. Segunda passada 0.
- **SLA**: marco de corte gravado; Pautas DP 89/90/91.
- **Pautas escritas**: 92 (supervisao, colab 49), 93/94/95 (DP, HE 12x36), **39 Pautas de posto** (supervisao, colaboradores sem notificacao do app).

## E0 BALDES — os 739 do balao

571 causas raiz; as 20 maiores cobrem so 119 (16%) — a cauda e por colaborador.

| porta que fecha | chamados | acoes |
|---|---|---|
| pergunta ao colab / declarar dia | 161 | 154 |
| validar um a um | 122 | 100 |
| fila de suporte (solicitacoes) | 120 | 84 |
| "sem dia" (ver abaixo) | 92 | 56 |
| corrigir cadastro da escala | 67 | 61 |
| medir (sem causa) | 29 | 28 |
| validar lote classe A | 28 | **1** |
| proposta de escala — wizard / Aplicar | 28 / 26 | 10 / 12 |
| conferir adesao | 26 | 26 |
| lastro (feriado do posto) | 15 | 14 |
| plano de folgas | 14 | 14 |
| pergunta ao colab (conversa) | 11 | 11 |

- **Lote classe A — FEITO** (aval Ronald 16/09, assinado pelo sistema, trilha "classe A, criterio declarado, aval Ronald 16/09"):
  - 12:57: 16 validadas; o lote parou em dois defeitos (validacao em mes trancado e disputa que nao fecha). As 5 batidas plantadas em mes trancado foram RETRATADAS; cura no ar 13:3x.
  - 13:34 (retomada): 22 validadas, 4 recusadas pela guarda de paridade (batida vizinha do mesmo tipo); 38 perguntas de mes trancado ficaram de fora; 0 batida em mes trancado; trilha completada em 9 que estavam sem ela.
  - Antes (12:57) x depois (13:34): respostas pendentes 174 -> 128; classe A 46 -> 0 (4 ficam, todas em mes trancado); chamados vivos 1.921 -> 1.890; balao 742 -> 711; na retomada 22 chamados passaram a resolvido.
  - Pauta de conferencia para o admin: 5 escritas (DP, por empresa). Retratacao possivel pela porta de retratar batida.
- **Criterio revisado da classe A** (casa qualquer marco do mesmo tipo +-10 min, sem contradicao no dia), sobre os 124 "um a um": 57 perguntas entrariam, so 3 chamados fechariam inteiros. Nada aplicado.
- **Os 92 "sem dia"**: roteados pelo catalogo (fatia no ar 13:0x).

**BALDE 0 — com o colaborador** (competencia corrente): 365 colaboradores, 1.965 perguntas sem resposta, 1.457 furos sem justificativa, 8.470 h em jogo (teto bruto). Idade mediana 21 dias. 63 sem notificacao do app, em 39 postos — Pautas de posto escritas. R$ nao medivel (sistema sem salario). Pauta PREVIA-DO-HOLERITE (em horas) na fila F7.

## ONDE O DP TRANCA A COMPETENCIA (proximos meses)

- Tela **Fechamento** → mes/ano/empresa → botao **Aprovar** do lote. Nao existe botao so de "trancar".
- O botao so tranca quando ninguem fica de fora (conta vazia, turno aberto, 12x36 sem ancora). Mes ja pago com gente de fora: pedir a tranca pela porta.
- Ao trancar, o sistema encerra os chamados dos dias da competencia; avisos de cadastro e revisao de desligamento ficam.
- Ordem: aprovar → exportar TXT → trancar → publicar holerite.

## CASOS

- **colab 49**: furos 23/08 e 12/09 vetados (cadastro confirmado errado em 08/09). Corte: 24/08-04/09 foi cobertura. Espera a supervisao (Pauta 92).
- **colab 901, 15/09**: dia cumprido, calendario corrigido (ok, sem selo). 14/09 segue cobrado: a colaboradora contestou ("era folga") e espera validacao do admin.
- **colab 709, espelho app x admin** (so leitura, 14:0x): mesmos numeros. Competencia 21/08-20/09: 112,5 h trabalhadas, 3,8 h extras, 4 turnos abertos e 11 dias inconsistentes nos dois lados, que leem a mesma funcao. Nao voltou o BUG 139. Ela nao tem aparelho cadastrado e usa o app pela web, que abre a mesma tela do admin. Diferencas so de apresentacao: (a) no app nativo, a lista de dias vai de 01 a 30/09, mas os totais sao da competencia; (b) o app pinta "alerta" em qualquer atraso, e o admin so marca dia inconsistente; (c) no app nativo, o dia de hoje sem batida ja aparece como "falta" no meio do dia (teto temporal; nao atinge ela; vai para a fila).
- **Feriado abrindo chamado** (so leitura, 14:0x): 76 chamados vivos em dia de feriado (07/09 e 08/09), sendo 26 em 5x2, 24 em 6x1, 24 em 12x36 e 2 em personalizado. Em todos o vinculo esta marcado "trabalha em feriado"; a celula, a escala e a precedencia dizem "dia de trabalho", entao o emissor cobra pela regra. Nos 5x2 e 6x1 vigentes, 195 de 200 vinculos estao marcados assim (o padrao do sistema). Se eles folgam no feriado, e cadastro: **Pautas DP 140-149** escritas (lista por posto; aval Ronald). Achados: (1) colab 143 esta sem posto, entao nao enxerga feriado municipal (cadastro); (2) **bug**: a precedencia nao enxerga o vinculo ja encerrado por troca de escala (colab 152, 07/09: a celula diz trabalho e a precedencia diz feriado sem previsao). Na competencia sao 47 vinculos, 42 colabs e 435 dias, 271 deles de trabalho. Cura so na precedencia (aval Ronald): fatia PRECEDENCIA-VINCULO-ENCERRADO na esteira desde 14:13, com DIFF de folha; se nao subir ate 16:00, espera o fim do congelamento.
- **Re-lavra 16/09**: medida fechada; 12 diferencas ficam para autopsia.

## PARADOS

- T4-MARCO-QUE-A-BATIDA-OCUPA — DIFF TXT=9 RETIDOS=47 → Pauta DP 83/84/85.
- TURNO-F1-S3 / P71-ANTECIPACAO-12X36 — corte Ronald.
- **HE 12x36 sem a tolerancia de 10 min/dia** (bug) — cura so depois do export de 09. Pauta DP 93/94/95 (08/2026 paga: 36,3 h emp 2, 21,5 h emp 3, 1,0 h emp 4).

## FILA

- Copiloto: legenda do calendario; dia do colaborador na ficha (a ficha mora no core).
- UI do fio do colaborador (cabecalho + proposta de escala com 28 dias de evidencia).
- PREVIA-DO-HOLERITE (app, em horas) + cobranca escalonada + metrica resposta_48h.
- Familia chamado: 52 sitios no registro. Achado: "tem canal de push?" tem dois juizes.

## ESPERANDO RONALD / DP / SUPERVISAO

- DP: Pautas 140-149 (feriado em 5x2/6x1; colab 143 sem posto), 89/90/91 (SLA), 93/94/95 (HE 12x36), 83/84/85 (T4); emp 2 06 e 07/2026 — folha fora do sistema?
- Supervisao: Pauta 92 (colab 49) e as 39 Pautas de posto (notificacao do app).
- Admin: validar a contestacao de 14/09 do colab 901.
- CONGELAMENTO de dinheiro: hoje 18:00 → qui 17/09 14:00.
