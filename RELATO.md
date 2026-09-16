# RELATO — esteira saas-hasner

_Estado de 16/09 14:25. Publico: so ids e contagens, nunca nome/CPF, nenhum codigo._

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

## NO AR HOJE (16/09) — 20 fatias

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
| 14:18 | FICHA-DO-COLAB-NO-CORE | uma porta para o copiloto, o fio e a ficha completa: vinculo em uma linha, lotacao, app, horas (do espelho), pendencias (das pilulas), proposta de escala, acoes pelo que mais resolve; telefone so na tela |
| 14:37 | PRECEDENCIA-VINCULO-ENCERRADO (bug) | a precedencia do dia enxerga o vinculo encerrado pela troca de escala (colab 152, 07/09: agora "trabalho", previsto); 47 vinculos, 435 dias na competencia; DIFF de folha 0 |
| 14:52 | FERIADO-PADRAO-POR-ESCALA (corte Ronald) | vinculo novo nasce pelo tipo de escala: 5x2 e 6x1 comercial folgam no feriado, 12x36 e escala corrida trabalham; o posto que declarou "opera em feriado" vence. Os existentes nao mudaram (Pautas DP 140-149). Contador vinculos_5x2_6x1_trabalha_feriado = 183 hoje, 0 declarados (nenhum posto marcou "opera em feriado" ainda; quem zera sao as Pautas 140-149); DIFF de folha 0 |
| 15:23 | FURO-PARCIAL-SEM-COBRANCA (bug) | o furo parcial vira cobranca tambem no julgamento na hora (a chave do cron saiu; crontab reinstalado e igual ao codigo); o chamado do dia e o espelho da celula (um por colab-dia, reabre nomeando o marco); retratacao e emissao leem a ata do julgamento corrente. Marco nao vencido, dia trancado e veto nao cobram. DIFF de folha 0 |
| 15:3x | APP-FALTA-NO-TETO | no app nativo, o dia de hoje sem batida so vira "falta" depois do fim do turno previsto (antes, as 14h ja era falta); so leitor |
| 15:47 | CANAL-DE-PUSH | "tem canal de push?" tem um juiz so, em core (cobranca e holerite importam dele); contador juizes_discordam_push = 0 |
| 16:18 | FUROS-SEM-COBRANCA-PORTAS | o contador do furo sem cobranca so conta o que o emissor pode cobrar; **furos_sem_cobranca_viva = 0** (portas fora do total: veto da celula 16, coberto por quem superou 1, decidido pelo admin 3, veto da regua 3) |
| 17:12 | CONTRATO-TROCAR-SENHA (pedido Fernando) | contrato da troca de senha escrito do codigo e travado por teste; /api/me passa a dizer se a troca e obrigatoria; contador colabs_com_troca_de_senha_pendente = 38 (emp 2: 36, emp 3: 2); copiloto responde "quantos colabs estao com senha provisoria?" |

Todas com suite verde, regua e deploy OK; as de dinheiro com DIFF de folha 0.

**Em curso** (fila da cadeia):
- **GEOFENCE-VALIDAR-E-RECUSAR** (BO Ronald): **PRONTA 16:17** -- teste vermelho na arvore anterior (7 falhas + 2 erros), suite verde (6.930), DIFF de folha 0 (TXT 0, retidos 0), esmeril limpo. **Deploy armado para qui 17/09 14:00** (dinheiro: a recusa retrata batida); o DRY dos 231 registrados sai no deploy e o --apply espera o "!". Antes do deploy, os testes rodam de novo (o crons.py mudou depois do ensaio).
- **FURO-COBRANCA-MORTA-REABRE** (corte Ronald: 117): a regra do chamado do dia vale para o dia inteiro com cobranca encerrada. **PRONTA 16:26** (vermelho 3; suite 6.928; DIFF de folha 0). Deploy tambem qui 14:00; o DRY do passivo sai no deploy.
  - Achado no ensaio: pela lei E1 (03/09), chamado do dia FECHADO pelo admin tambem renasce quando o furo segue. Mantido; o contador foi alinhado a isso.


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

## PASSIVO DO FURO SEM COBRANCA — APLICADO (aval Ronald 16/09, 15:41-15:44)

Pela porta do cron, com trilha "passivo furo sem cobranca, aval Ronald 16/09" em cada celula. O push de supervisao foi suprimido (dias retroativos); a pergunta ao colab segue o fluxo normal.

| | celulas | colabs |
|---|---|---|
| antes | 478 | 151 |
| cobradas | 457 | |
| 2o DRY | 20 | 5 |

- As 20 que sobraram o emissor, pela lei, nao cobra:
  - 16: celula que nao e dia de trabalho (vinculo intermitente);
  - 1: dia coberto por pedido de ausencia em analise;
  - 3: chamado do dia fechado pelo admin (decisao humana).
- FUROS-SEM-COBRANCA-PORTAS no ar 16:18: `furos_sem_cobranca_viva` = 0 no placar.
- Achado (fila): celula de intermitente (nao e dia de trabalho) lavrada como furo parcial.
- Colab 709: 14/09 e os outros 5 dias entraram em cobranca.

## BO GEOFENCE (medido 15:4x, so leitura)

1. **Emissao nao caiu.** Furo de geofence: 2 a 5 por dia nos ultimos 14 dias (48 chamados novos; ocorrencias novas vao para o chamado vivo do colab: 176 chamados com nota nova em 14 dias). O ultimo foi hoje as 13:12.
2. **Onde estao**: 231 vivos em "registrado", **sem verbo, fora da fila do admin**. A causa e o corte A3.1b (e201b0c2, 23/08: modulo de auditoria nasce "registrado"). A FILA-ROTEIA-CATALOGO de hoje so mexeu na revisao de vinculo (69 em Decidir, gaveta "revisao de cadastro"), nao no furo de geofence -- nao e bug da fatia de hoje. Autorizacoes do admin por semana: 55 (03/08), 22, 15, 5 (24/08), 0, 3.
3. **Porta do admin**: o fio tem "Aceitar" e "Negar e advertir", e o segundo TAMBEM autoriza a batida (so grava advertencia). Nao existe recusa. E em chamado "registrado" os dois botoes falham: a transicao manual registrado -> resolvido nao e permitida.
- **Corte (Claude)**: fatia GEOFENCE-VALIDAR-E-RECUSAR. O furo de geofence volta a ser cobranca do admin (nasce aberto, verbo Validar); porta de RECUSA com motivo obrigatorio (retrata a batida pela porta unica, trilha "fora do posto"); aceitar/advertir/recusar funcionam tambem no que esta "registrado". A batida continua nunca barrada na hora. A recusa tira batida da folha, entao e dinheiro: sobe quinta 14:00. Os 231 registrados passam para "em analise" por DRY + "!".

## CONTRATOS DO APP

_Para o Fernando (app iOS). A mesma coisa está em app/docs/HANDOFF.md, seção "Plataforma do app"._

### POST `/api/auth/trocar-senha/` (pedido Fernando, 16/09)

Escrito a partir do código e travado por um teste de contrato que confere cada ramo abaixo.

**1. Autenticação.** Header `Authorization: Bearer <access>` (o `access` do login). Sem header, ou token inválido: `401`
do framework, corpo `{"detail": "..."}` (token inválido traz também `"code": "token_not_valid"`) -- **não** traz
`erro_codigo`. O `X-Hasner-Client` não muda nada nesta rota.

**2. Corpo (JSON).** Os três campos são obrigatórios, texto:

| campo            | regra |
|------------------|-------|
| `senha_atual`    | a senha de agora (na senha provisória, é o CPF só com números) |
| `senha_nova`     | pelo menos 6 caracteres (é a única regra de complexidade) |
| `senha_confirma` | igual a `senha_nova` |

**3. Sucesso.** `200`:

```json
{"sucesso": true, "mensagem": "Senha alterada com sucesso.", "tokens": {"refresh": "...", "access": "..."}}
```

- **Guarde os tokens novos na hora.** A troca derruba **todos** os tokens anteriores da pessoa, inclusive o que fez
  esta chamada: access antigo → `401` com `"code": "credencial_trocada"` ("senha alterada; entre de novo"); refresh
  antigo → `401` `{"detail": "senha alterada; entre de novo"}`. Isso vale para os outros aparelhos dela também.
- A flag de troca obrigatória é zerada (`precisa_trocar_senha` passa a `false` no `/api/me/`).

**4. Erros.** Todos com `{"erro_codigo", "erro_msg", "erro_dica"}`. Conferidos **nesta ordem** (o primeiro que falha
responde):

| ordem | status | `erro_codigo`           | `erro_msg`                                        | `erro_dica` |
|-------|--------|-------------------------|---------------------------------------------------|-------------|
| 1     | 400    | `campos_vazios`         | Preencha todos os campos.                         | Senha atual, nova e confirmacao sao obrigatorias. |
| 2     | **401**| `senha_atual_incorreta` | Senha atual incorreta.                            | na senha provisória: "No primeiro acesso, a senha e o seu CPF (somente numeros)."; fora dela: "Verifique a senha atual e tente novamente." |
| 3     | 400    | `senha_curta`           | A nova senha deve ter pelo menos 6 caracteres.    | Use uma senha mais longa. |
| 4     | 400    | `senhas_nao_conferem`   | As senhas nao coincidem.                          | Digite a mesma senha nos dois campos. |
| 5     | 400    | `senha_igual_atual`     | A nova senha deve ser diferente da atual.         | Escolha uma senha diferente. |

**Atenção ao 401 do ramo 2:** é o mesmo status de token vencido. Diferencie pelo corpo: com `erro_codigo` é senha
atual errada (mostre a dica, **não** deslogue); sem `erro_codigo` (só `detail`) é token -- aí sim, renove ou volte ao
login. Os textos vêm sem acento, como estão no código.

**5. Efeitos colaterais.**
- Trilha: uma linha `senha_trocada_pelo_dono` no LogAuditoria (sem a senha).
- Sessões web da pessoa caem; tokens do app (todos os aparelhos) deixam de valer -- os outros aparelhos voltam ao login.
- Nenhum push é enviado.

**Como o app sabe que precisa trocar.**
- `POST /api/auth/login/` (em qualquer plataforma, `X-Hasner-Client: ios` inclusive): `"precisa_trocar_senha": true`
  quando a troca é obrigatória. **Quando não é, a chave não vem** (convenção do login desde S113; ausente = `false`).
  O login **não** é barrado: o token sai válido.
- `GET /api/me/` (**novo, 16/09**): `"precisa_trocar_senha": true|false`, sempre presente. Use ao reabrir o app com o
  token guardado, sem novo login.
- **O bloqueio é do app:** enquanto `precisa_trocar_senha` for `true`, o app mostra só a tela de troca.

## ONDE O DP TRANCA A COMPETENCIA (proximos meses)

- Tela **Fechamento** → mes/ano/empresa → botao **Aprovar** do lote. Nao existe botao so de "trancar".
- O botao so tranca quando ninguem fica de fora (conta vazia, turno aberto, 12x36 sem ancora). Mes ja pago com gente de fora: pedir a tranca pela porta.
- Ao trancar, o sistema encerra os chamados dos dias da competencia; avisos de cadastro e revisao de desligamento ficam.
- Ordem: aprovar → exportar TXT → trancar → publicar holerite.

## CASOS

- **colab 49**: furos 23/08 e 12/09 vetados (cadastro confirmado errado em 08/09). Corte: 24/08-04/09 foi cobertura. Espera a supervisao (Pauta 92).
- **colab 901, 15/09**: dia cumprido, calendario corrigido (ok, sem selo). 14/09 segue cobrado: a colaboradora contestou ("era folga") e espera validacao do admin.
- **colab 709, espelho app x admin** (so leitura, 14:0x): mesmos numeros. Competencia 21/08-20/09: 112,5 h trabalhadas, 3,8 h extras, 4 turnos abertos e 11 dias inconsistentes nos dois lados, que leem a mesma funcao. Nao voltou o BUG 139. Ela nao tem aparelho cadastrado e usa o app pela web, que abre a mesma tela do admin. Diferencas so de apresentacao: (a) no app nativo, a lista de dias vai de 01 a 30/09, mas os totais sao da competencia; (b) o app pinta "alerta" em qualquer atraso, e o admin so marca dia inconsistente; (c) no app nativo, o dia de hoje sem batida ja aparece como "falta" no meio do dia (teto temporal; nao atinge ela; vai para a fila).
- **colab 709, por dia** (so leitura, 14:2x): 11 dias inconsistentes e 4 turnos abertos, com **0 chamado vivo**. Ela recebe pela web: 28 respostas dela, a ultima hoje as 13:55; nao tem aparelho, entao nao ha push. Nas perguntas o canal funciona; o buraco e o emissor.
  - **bug (espera corte)**: o furo parcial (volta do intervalo ou saida sem batida, com o resto batido) nao vira cobranca em dois casos:
    - (a) o julgamento na hora (quando chega batida ou resposta) nao usa a chave `--furo-parcial` do cron, grava a impressao, e o cron das 06:28 pula a celula (colab 709, 14/09);
    - (b) o cartorio nao emite quando o dia ja tem QUALQUER chamado, inclusive resolvido. O chamado da entrada atrasada fecha as 07:2x e cala o furo da tarde (03, 04, 08, 09 e 15/09).
  - Frota, competencia ate 15/09: 839 celulas com furo; 332 sem chamado nenhum e 348 so com chamado encerrado, ou seja, 680 celulas sem cobranca viva em 188 colabs. No log do cron, o furo parcial emitiu 2 vezes em 15 rodadas.
- **Feriado abrindo chamado** (so leitura, 14:0x): 76 chamados vivos em dia de feriado (07/09 e 08/09), sendo 26 em 5x2, 24 em 6x1, 24 em 12x36 e 2 em personalizado. Em todos o vinculo esta marcado "trabalha em feriado"; a celula, a escala e a precedencia dizem "dia de trabalho", entao o emissor cobra pela regra. Nos 5x2 e 6x1 vigentes, 195 de 200 vinculos estao marcados assim (o padrao do sistema). Se eles folgam no feriado, e cadastro: **Pautas DP 140-149** escritas (lista por posto; aval Ronald). Achados: (1) colab 143 esta sem posto, entao nao enxerga feriado municipal (cadastro); (2) **bug**: a precedencia nao enxerga o vinculo ja encerrado por troca de escala (colab 152, 07/09: a celula diz trabalho e a precedencia diz feriado sem previsao). Na competencia sao 47 vinculos, 42 colabs e 435 dias, 271 deles de trabalho. Cura so na precedencia (aval Ronald): **no ar** (PRECEDENCIA-VINCULO-ENCERRADO).
- **Re-lavra 16/09**: medida fechada; 12 diferencas ficam para autopsia.

## PARADOS

- T4-MARCO-QUE-A-BATIDA-OCUPA — DIFF TXT=9 RETIDOS=47 → Pauta DP 83/84/85.
- TURNO-F1-S3 / P71-ANTECIPACAO-12X36 — corte Ronald.
- **HE 12x36 sem a tolerancia de 10 min/dia** (bug) — cura so depois do export de 09. Pauta DP 93/94/95 (08/2026 paga: 36,3 h emp 2, 21,5 h emp 3, 1,0 h emp 4).

## FILA

- Copiloto: ferramenta dia_da_frota (data + filtros; celula soberana por veredito; mesma funcao do Raio-X; golden +1 da pergunta de 07/09, escala != 12x36) -- F7, pedido Ronald 16/09.

- Copiloto: legenda do calendario; dia do colaborador na ficha (a ficha mora no core).
- UI do fio do colaborador (cabecalho + proposta de escala com 28 dias de evidencia).
- PREVIA-DO-HOLERITE (app, em horas) + cobranca escalonada + metrica resposta_48h.
- Familia chamado: 52 sitios no registro. Achado: "tem canal de push?" tem dois juizes.

## ESPERANDO RONALD / DP / SUPERVISAO

- DP: Pautas 140-149 (feriado em 5x2/6x1; colab 143 sem posto), 89/90/91 (SLA), 93/94/95 (HE 12x36), 83/84/85 (T4); emp 2 06 e 07/2026 — folha fora do sistema?
- Supervisao: Pauta 92 (colab 49) e as 39 Pautas de posto (notificacao do app).
- Admin: validar a contestacao de 14/09 do colab 901.
- Ronald: "!" dos 231 geofence registrados (DRY no deploy de quinta); "!" do passivo da cobranca morta (DRY no deploy de quinta).
- CONGELAMENTO de dinheiro: hoje 18:00 → qui 17/09 14:00.
