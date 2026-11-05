# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 18:07** · publicado em **05/11/2026 18:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 18:07 · sessao 2d1e22c04bb5 · - · - · origem=Ronald (tmux)

Regras verificadas para consolidar:

- **C11 do registro vivo é ausência**, não “FIO MUDO”: pergunta “a ausência do dia suspende, fecha ou levanta a cobrança viva?” e aponta a `silencio_ausencia.reconciliar_por_ausencia`. O rótulo operacional do aval não autoriza substituir essa autoridade. (`app/core/juizes.py:117`; `app/core/juizes.py:271`)
- **Morte por leitura já tem juiz: C6 `pergunta_viva`**. C15 `fio_vivo` reduz a coleção; C17 `porta_alcancavel_do_dia` responde alcance. Não precisam nascer juiz novo. (`app/core/juizes.py:266`; `app/core/juizes.py:275`; `app/core/juizes.py:277`)
- C6 é leitura com memo por instância, cuja regra mora em `_pergunta_viva_calc`; introduzir escrita na consulta faria o consumidor virar produtor. (`app/chamados/juizes.py:822`; `app/chamados/juizes.py:839`; `CLAUDE.md:168`; `CLAUDE.md:170`)
- A regra estrutural exige fio vivo ou ação administrativa nomeada enquanto persistir dívida da lâmpada; a morte não pode ser decidida por carimbo isolado, e o cron é vigia esperado zero. (`app/docs/CORTES.md:253`; `CLAUDE.md:167`; `CLAUDE.md:169`)
- O aceite deve nascer de cadastro + batidas + regra, antes de desenho/código; o caminho velho só mede impacto. (`app/docs/LEIS.md:136`)
- Idempotência exigida: repetir dá mesmo estado, uma linha de trilha, zero duplicata; censo de escritores deve fechar antes de alterar filtro/regra. (`CLAUDE.md:175`; `CLAUDE.md:193`)

Casos para A, com resposta pela regra:

| Cadastro/estado/ato | Resposta exigida |
|---|---|
| Ativo, vínculo válido, marco vencido faltante na ata, chamado aberto por `batida_ausente`. | A emissão nasce no evento da célula/abertura contemplada pelo aval; pergunta é do marco devido, não do simples status vivo. (`CLAUDE.md:168`; `app/docs/CORTES.md:253`; `app/chamados/services/fabrica_da_celula.py:8`) |
| Duas aberturas ou perdas simultâneas do mesmo fato. | Mesma dívida/fio final sem duplicação e com trilha idempotente. (`CLAUDE.md:175`) |
| Reabrir com dívida persistente, sem candidata a ressuscitar. | Estado vivo sozinho não satisfaz a regra: deve haver pergunta viva ou ação administrativa nomeada. (`app/docs/CORTES.md:253`) |
| Retratar duplicata cuja canônica cobre o mesmo fato. | Preservar unidade do fato; não criar irmã só porque um chamado ficou sem pergunta. Resposta discordante pendente continua para decisão humana. (`CLAUDE.md:175`; `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1163`) |
| Validar uma saída, permanecendo intervalo devido. | Marco quitado não volta a ser cobrado; outro marco continua com fio/ação. Pergunta cobra marco, célula julga dia. (`app/chamados/juizes.py:1173`; `app/docs/CORTES.md:253`) |
| Reapontar a última resolvedora de A para B. | Dívida conserva porta alcançável; não existe obrigação de uma pergunta por chamado duplicado. (`app/docs/CORTES.md:253`; `app/docs/CORTES.md:404`; `CLAUDE.md:175`) |
| Morte por leitura com competência trancada. | Pergunta sai da fila, resposta fica na trilha e decisão segue ao DP; isso não planta batida nem absolve o dia. (`app/chamados/juizes.py:1138`; `app/chamados/juizes.py:1142`) |
| Morte por leitura de marco atendido, ainda havendo outro furo. | Não recriar pergunta do marco atendido; conservar cobertura do resto pela lâmpada. (`app/chamados/juizes.py:1173`; `app/docs/CORTES.md:253`) |
| Resposta discordante ou sem hora comparável, sem decisão explícita do admin. | Continua viva para decisão humana; não transformar discordância em quitação. (`app/chamados/juizes.py:1096`; `app/chamados/juizes.py:1158`) |
| Empresa com corte 21, competência 10. | Janela é **21/09→20/10**, não mês civil; 21/10 pertence à seguinte. Usar corte da empresa, nunca 21 fixo. (`app/docs/LEIS.md:77`; `app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/ponto/janelas.py:54`) |
| Turno 22h→06h ancorado ontem; observar 00h01 ou 06h10. | Meia-noite não encerra fato; saída ainda não vence antes de 06h30, usando margem vigente 30 min e marcos encadeados. (`CLAUDE.md:185`; `app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:834`; `app/ponto/services/cartorio.py:846`; `app/chamados/services/fabrica_da_celula.py:18`) |
| Vínculo antigo gerou célula e cobre a data, embora inativo hoje. | Geradora responde; vínculo ativo atual não substitui. Fora da vigência, resposta é `None` e dono CADASTRO. (`app/escala/alimentacao.py:231`; `app/escala/alimentacao.py:272`) |
| Escala vigente, célula sem ata, tentar perguntar marco. | Esperar ata e acender indicação de falta de ata; não inventar marco/furo. `trabalha=None` não equivale a folga. (`app/chamados/services/disputa_emissao.py:818`; `app/chamados/services/disputa_emissao.py:839`; `app/docs/JUIZES.md:56`) |
| Ausência parcial sem período, caso mandado à lista admin. | Sem pergunta, sem pai, sem push; ação fica no cadastro/lista. (`app/chamados/services/fabrica_da_celula.py:77`; `app/chamados/services/fabrica_da_celula.py:101`; `app/chamados/services/disputa_emissao.py:847`) |
| `registrado`, sem ação devida. | Vivo legítimo sem cobrança; excluir do universo de defeitos antes de RED. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:25`) |
| Ausência em decisão ou silêncio manual vigente. | Suspensão legítima; não criar pergunta para zerar bruto. Ausência aprovada só fecha se a premissa morreu. (`app/chamados/services/silencio_ausencia.py:77`; `app/chamados/services/silencio_ausencia.py:80`; `app/chamados/services/silencio_ausencia.py:93`) |
| Dia regularizado sem dívida, chamado ainda vivo. | Cobrança sem premissa morre por lastro, não recebe pergunta artificial. (`app/docs/PROMPTS.md:1519`) |
| Histórico fora do horizonte, sem fato novo. | Passado é foto; esta obra não autoriza backfill recorrente. Exportada conserva gravado e diferença tem porta própria. (`app/docs/LEIS.md:145`; `app/docs/LEIS.md:125`) |

Contraexemplos críticos para D:

- **“C6 morreu ⇒ emitir pergunta” é errado**: validação, tranca, identidade, marco atendido e célula conclusiva regular são mortes legítimas; dívida residual exige outro marco/ação. (`app/chamados/juizes.py:1136`; `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1171`; `app/chamados/juizes.py:1178`; `app/chamados/juizes.py:1195`)
- **C15 verdadeiro não prova alcance**: C17 exige chamado da célula na fila ou pergunta viva no app com âncora na fila. (`app/chamados/juizes.py:1233`; `app/chamados/juizes.py:1341`; `app/chamados/juizes.py:1378`)
- **“Excluir todo `trabalha=None`” cala LIMBO devido**: só `trabalha=False` ou ata válida de outro tipo cala; parcial tem exceções próprias. (`app/docs/JUIZES.md:55`; `app/docs/JUIZES.md:58`)
- **“Sem ata: fabricar pela classificação” viola autoridade quando há escala**: a exceção de ramo só batidas é para ausência de escala; célula com marcos sem ata espera. (`app/chamados/services/disputa_emissao.py:833`; `app/chamados/services/fabrica_da_celula.py:39`; `app/chamados/services/fabrica_da_celula.py:52`)
- **Evento pós-save genérico não demonstra entrega garantida**: o chamado usa `engolir` e pode sair por julgamento em curso; a cura precisa fonte e cobertura das cinco perdas. (`app/chamados/signals.py:172`; `app/chamados/signals.py:174`)
- **Não retirar cron só por proposta**: baixa de pendente exige frota real não vazia, condição literal medida, contador no mesmo commit e reversão declarada. Sem banco/log, cumprimento dessas condições é [nao verificado]. (`app/docs/LEIS.md:139`)

A individualização dos 494 e quantos são vivos legítimos, cada via e eventual redução do universo são [nao verificado]; o anexo só fornece total declarado, sem registros. (`avais.txt`, anexo:1)

## 05/11 17:57 · sessao aa351fdaafbd · - · - · origem=Ronald (tmux)

Critério: classificar pelo conteúdo alegado, sem validar procedência.

- `divergencia-texto-codigo`: comparação explícita de comentário, help, docstring, cláusula ou promessa com o código.
- `defeito`: comportamento funcional ou ausência funcional alegados, inclusive riscos de segurança e carga sem categoria própria.
- `lacuna-de-teste-ou-selo`: teste, selo, saída ou comprovação operacional insuficientes. [nao verificado]
- `documentacao`: relato histórico ou configuração produtiva sem insumos para comprovação.
- `risco-concorrencia` e `risco-idempotencia`: somente quando o mecanismo correspondente aparece na afirmação.
- `instrumentacao`: limitações de censos, auditores, contadores e alarmes.
- `apresentacao`: informação exposta em tela, PDF, mensagem ou payload.

Exceções LA ao predominante `divergencia-texto-codigo`:

| Classe | IDs |
|---|---|
| defeito | LA-21, LA-32, LA-49, LA-50, LA-55 |
| risco-idempotencia | LA-15 |
| apresentacao | LA-34 |
| documentacao | LA-16, LA-27, LA-35, LA-53, LA-56, LA-60 |
| lacuna-de-teste-ou-selo | LA-06, LA-70, LA-76, LA-83 | [nao verificado]

LC: `lacuna` funcional deve virar `defeito`; exceções são LC-69 → `risco-idempotencia` e LC-116 → `instrumentacao`. Entre `risco`, LC-48 → `risco-idempotencia`; LC-05 e LC-98–102 → `documentacao`; restantes → `defeito`.

Exceções entre LC originalmente `divergência`:

| Classe | IDs |
|---|---|
| divergencia-texto-codigo | LC-35 |
| documentacao | LC-45 |
| apresentacao | LC-02, LC-36, LC-66, LC-71, LC-74, LC-76, LC-79 |
| instrumentacao | LC-93 |
| defeito | LC-06, LC-10, LC-12, LC-13, LC-58, LC-59, LC-70, LC-78, LC-82, LC-91, LC-92, LC-103 |

Contagem anterior às fusões nessa proposta:

| Classe | LA | LC |
|---|---:|---:|
| defeito | 5 | 97 |
| divergencia-texto-codigo | 69 | 1 |
| documentacao | 6 | 7 |
| lacuna-de-teste-ou-selo | 4 | 0 | [nao verificado]
| instrumentacao | 0 | 2 |
| risco-idempotencia | 1 | 2 |
| apresentacao | 1 | 7 |
| Total | 86 | 116 |

Não fundir apenas por compartilharem referência:

- LA-17 e LC-84: help de 720 versus derivação automática, e materializador que fornece 720, respectivamente (`ponto/models.py:875`; `app/chamados/services/materializacao.py:519`).
- LA-01 e LC-107: contradição documental da retenção versus derivação de furo somente de pausa (`docs/LEIS.md:81`; `app/folha/export.py:1124`).
- LA-70 e LC-46: comprovação histórica de alcance versus custo de consultas anteriores ao skip (`ponto/services/cartorio.py:1362`; `app/ponto/services/cartorio.py:1362`).
- LB-25 e LC-92: fallback de paridade sem foto versus escopo de vínculo diferente na leitura de folga (`app/escala/models.py:1280`).

## 05/11 17:56 · sessao eb873f1af426 · - · - · origem=Ronald (tmux)

Seis famílias recomendadas para fusão:

| IDs | Causa comum e referências |
|---|---|
| LA-21; LC-11 | Piso de pausa de quinze minutos entre quatro e seis horas não atravessa a indenização; `ponto/motor_calculo_v2.py:3226`; `ponto/motor_calculo_v2.py:3232`; `escala/models.py:19`; `app/escala/models.py:17`; `app/ponto/calculador/periodo.py:334`; `app/ponto/calculador/regras.py:764`. |
| LA-26; LB-31; LC-12 | Switch/default da régua altera a prorrogação noturna; `docs/LEIS.md:83`; `core/regua_cct.py:453`; `ponto/motor_calculo_v2.py:758`; `app/core/regua_cct.py:449`; `app/ponto/motor_calculo_v2.py:758`. |
| LB-40; LC-13 | DSR comercial e extras são recompostos/agrupados pela data civil; `app/ponto/motor_calculo_v2.py:3313`; `app/ponto/motor_calculo_v2.py:3441`; `app/ponto/motor_calculo_v2.py:3466`; `app/ponto/motor_calculo_v2.py:2578`; `app/ponto/motor_calculo_v2.py:2639`. |
| LA-72; LB-77; LC-57 | Corte 1 substituído por 2; `colaboradores/models.py:46`; `colaboradores/views.py:499`; `ponto/janelas.py:51`; `app/templates/colaboradores/empresa_form.html:54`; `app/colaboradores/views.py:499`; `app/ponto/janelas.py:51`; `app/ponto/janelas.py:15`; `app/ponto/janelas.py:26`. |
| LA-42; LB-28; LC-38; LC-39 | Recusas de marcação por férias/situação, GPS e proximidade, abrangidas conjuntamente por LB-28; `api/views.py:815`; `chamados/tests/test_invariantes_disputa.py:193`; `docs/LEIS.md:78`; `api/views.py:423`; `api/views.py:466`; `app/ponto/registro_batida.py:145`; `app/api/views_core.py:402`; `app/api/views_core.py:435`; `app/api/views_core.py:454`; `app/ponto/registro_batida.py:21`; `app/ponto/registro_batida.py:138`; `app/ponto/services/triagem_batida.py:668`; `app/api/views.py:415`; `app/api/views.py:449`; `app/api/views.py:462`. |
| LA-28; LB-41; LC-109 | Cobertura autoritativa insuficiente deixa o tipo gravado decidir no fallback do pareador; `docs/LEIS.md:119`; `ponto/motor_calculo_v2.py:573`; `ponto/turnos.py:1380`; `app/ponto/motor_calculo_v2.py:559`; `app/ponto/motor_calculo_v2.py:573`; `app/ponto/turnos.py:1380`. |

Manter separados:

- **LC-107 / LA-01:** derivação de furo pelos papéis versus divergência documental da retenção; `app/folha/export.py:1124`; `app/folha/export.py:1127`; `docs/LEIS.md:81`; `folha/export.py:1147`; `folha/export.py:1235`.
- **LC-84 / LA-17:** materializador fornece 720 minutos versus help e regra de derivação/preservação dos minutos informados; `app/chamados/services/materializacao.py:519`; `app/ponto/models.py:875`; `ponto/models.py:757`; `ponto/models.py:875`; `ponto/models.py:883`.
- **LC-61 / LB-03:** pré-requisito cadastral do export versus alcance insuficiente dos comparadores; `app/folha/export.py:29`; `app/folha/export.py:846`; `app/folha/fora_da_folha.py:128`; `app/folha/export.py:764`; `app/folha/porta_export.py:469`; `app/folha/porta_export.py:718`.
- **LC-91 / LA-44:** desacordo entre juízes versus comentário antigo do materializador; `app/chamados/services/materializacao.py:78`; `app/chamados/juizes.py:446`; `app/ponto/turnos.py:2632`; `chamados/services/materializacao.py:61`; `chamados/services/materializacao.py:78`; `ponto/turnos.py:2555`.
- **LC-34 / LB-127:** ausência de preservação do tipo original versus corrida de anotações; `app/ponto/services/flip_batida.py:25`; `app/ponto/registro_batida.py:254`; `app/ponto/registro_batida.py:274`.

## 05/11 17:56 · sessao c231c0062af1 · - · - · origem=Ronald (tmux)

Fusões propostas entre A e B, sem julgamento de procedência:

| IDs a fundir | Causa/afirmação comum e referências das origens |
|---|---|
| LA-13; LB-103 | Upload avulso substitui/apaga holerite anterior sem ciência, inclusive na repetição. `holerite/views.py:1191`; `holerite/views.py:1193`; `app/holerite/views.py:1174`; `app/holerite/views.py:1198` |
| LA-24; LB-29 | Indenização de intervalo suprimido sem consultar `intervalo_indenizavel`. `ponto/motor_calculo_v2.py:822`; `app/ponto/motor_calculo_v2.py:822` |
| LA-25; LB-30 | Desconto somente do intervalo reconhecido/real, em divergência da subtração universal do intervalo cadastrado. `ponto/motor_calculo_v2.py:2773`; `app/ponto/motor_calculo_v2.py:2774` |
| LA-26; LB-31 | Configuração sem CCT aplicada permite regra noturna derivada do ciclo para empresa não CLT. `core/regua_cct.py:453`; `app/core/regua_cct.py:449`; `ponto/motor_calculo_v2.py:758` |
| LA-28; LB-41 | Alimentação autoritativa indisponível/incompleta permite fallback pelo tipo gravado da batida. `ponto/motor_calculo_v2.py:573`; `ponto/turnos.py:1380`; `app/ponto/motor_calculo_v2.py:559` |
| LA-29; LB-44 | Seleção de motor ainda usa rótulo e limita a correção geométrica a determinados ramos. `ponto/motor_calculo_v2.py:3635`; `ponto/motor_calculo_v2.py:3668`; `app/ponto/motor_calculo_v2.py:3635`; `app/ponto/motor_calculo_v2.py:3668` |
| LA-30; LB-45 | Relavra pela chave `versao` apaga/recria linhas anteriores, sem conservar revisões; LB acrescenta ausência de DIFF por rubrica. `ponto/services/dia_pago.py:357`; `app/ponto/services/dia_pago.py:333`; `app/ponto/services/dia_pago.py:357` |
| LA-33; LB-42 | Falta aprovada em folga entra na contagem aquisitiva de férias; preservar também a alegação sobre overlay contida em LA-33. `ferias/models.py:144`; `ferias/services.py:246`; `app/ferias/models.py:144` (movida) |
| LA-34; LB-32 | PDF conserva faltas do motor quando não ocorre sobrescrita pelos totais da folha. `relatorios/pdf_espelho.py:461`; `app/relatorios/pdf_espelho.py:461`; `app/relatorios/pdf_espelho.py:471` |
| LA-42; LB-28 | Promessa de batida nunca barrada diverge das recusas efetivas; conservar férias, situação, GPS e colisão próxima. `api/views.py:423`; `api/views.py:466`; `api/views.py:815`; `app/ponto/registro_batida.py:145`; `app/api/views_core.py:402`; `app/api/views_core.py:435`; `app/api/views_core.py:454` |
| LA-49; LB-24 | Regeneração fora do fim/vigência do vínculo altera DNA/trabalho sem atualizar os campos de denúncia da regeneração. `ponto/portas/celula.py:233`; `ponto/portas/celula.py:259`; `app/ponto/portas/celula.py:225`; `app/ponto/portas/celula.py:233` |
| LA-57; LA-58; LB-35 | Diagnóstico documental antigo de L-050 afirma que exportação cala emissão e que tranca recusa cobrança; LB-35 abrange as duas afirmações da mesma linha documental. `docs/LEIS.md:189`; `app/docs/LEIS.md:91`; `app/docs/LEIS.md:189` |
| LA-72; LB-77 | Corte de competência 1 oferecido/documentado é convertido em 2 pela view e pelo juiz. `colaboradores/views.py:499`; `ponto/janelas.py:51`; `app/colaboradores/views.py:499`; `app/ponto/janelas.py:51` |
| LA-80; LB-36 | Varredura mantém `disparar_perguntas_competencia` como escritor; LB amplia o item para cobrança com lastro e reconciliação. `config/crons.py:1483`; `app/config/crons.py:1999`; `app/config/crons.py:2004`; `app/config/crons.py:2018` |

Cuidados de conservação:

- LA-34/LB-32 deve registrar `dinheiro=sim`, preservando a divergência original `LA-34: nao; LB-32: sim`. `relatorios/pdf_espelho.py:461`; `app/relatorios/pdf_espelho.py:461`
- LA-72/LB-77 conserva `FERI-11 — B:2099`, embora LB-77 registre nenhuma. `colaboradores/views.py:499`; `app/colaboradores/views.py:499`
- LA-29/LB-44 conserva `O136 — B:1346`, embora LB-44 registre nenhuma. `ponto/motor_calculo_v2.py:3635`; `app/ponto/motor_calculo_v2.py:3635`
- LA-80/LB-36 conserva todas as coberturas das duas origens. `config/crons.py:1483`; `app/config/crons.py:1999`

Não fundir apenas por proximidade temática:

- LA-09/LB-161: docstring incompatível com emissor versus selo incapaz de detectar neutralização; afirmações distintas. `folha/tests/test_export_59a.py:1`; `app/folha/tests/test_export_59a.py:63`
- LA-11/LB-125/LB-126: escrita fora da guarda versus duas corridas de atualização; causas distintas. `ponto/services/fechamento.py:1144`; `app/ponto/services/fechamento.py:1128`; `app/ponto/services/fechamento.py:1640`
- LA-40/LB-28: promessa de bloqueio por ausência versus recusa de batida por situação/GPS/colisão; causas distintas. `ponto/models.py:586`; `app/ponto/registro_batida.py:145`
- LA-81/LB-36: inventário documental antigo de papéis versus escritores residuais atuais; afirmações distintas. `docs/LEIS.md:191`; `app/config/crons.py:1999`

O mapeamento contém 14 grupos, absorve 29 IDs A/B e reduz a contagem em 15 itens, calculado dos grupos acima [nao verificado em arquivo externo].

## 05/11 17:53 · sessao 93fbeae1d62f · FUNIL-28 · censo · origem=Code

Consolidação documental, sem novo julgamento de procedência. Ocorrência e impacto real em produção: **[nao verificado]**.

`B:NN` significa `app/docs/BACKLOG.md:NN`. As origens numéricas correspondem aos nomes completos na última tabela. Competências identificam exemplos ou relatos dos anexos; `n/a` indica ausência de recorte. “Parcial” indica cobertura incompleta no BACKLOG.

| id | classe | arquivo:linha | afirmacao (uma frase) | dinheiro | competencia | obra ou linha do BACKLOG que ja cobre | resposta(s) de origem |
|---|---|---|---|---|---|---|---|
| LC-01 | defeito | `app/folha/export.py:348`; `app/folha/export.py:409`; `app/folha/export.py:411` | A regra do fim da competência pode zerar feriado apurado em fatia anterior. | sim | n/a | FERI-06 — B:2094 | 11 |
| LC-02 | divergência | `app/ponto/services/fechamento.py:910`; `app/folha/export.py:42` | O total de HE arredondado para a tela pode diferir da soma das parcelas do TXT. | sim | n/a | nenhuma | 11 |
| LC-03 | defeito | `app/folha/export.py:269`; `app/folha/services/vigia_txt.py:91` | Remapear falta para código diferente de 8792 perde conferência e atribuição das datas. | sim | n/a | nenhuma | 11 |
| LC-04 | lacuna | `app/core/regua_cct.py:72`; `app/core/regua_cct.py:458`; `app/ponto/motor_calculo_v2.py:2006` | Sete parâmetros CCT cadastráveis não chegam ao cálculo, que conserva tolerâncias e mínimos próprios. | sim | n/a | PERCENTUAL-DO-NOTURNO-SEM-LEITOR — B:1602; TOLERANCIA-SEM-LEITOR — B:1605; INTERVALO-MINIMO-SEM-LEITOR — B:1606; parcial | 11, 12, 16 |
| LC-05 | risco | `app/ponto/motor_calculo_v2.py:3141`; `app/ponto/calculador/regras.py:800`; `app/ponto/calculador/periodo.py:166` | A divisão HE50/HE100 usa degrau fixo de 120 minutos sem lastro jurídico demonstrado pelo anexo. | sim | n/a | nenhuma | 11, 12 |
| LC-06 | divergência | `app/core/regua_cct.py:378`; `app/ponto/motor_calculo_v2.py:3122`; `app/ponto/calculador/regras.py:771` | A política CCT global `legais` limita a referência do turno partido a 480 minutos e diverge do calculador paralelo. | sim | n/a | EXCEDENTE-IGNORA-O-SINDICATO — B:1604 | 11, 12 |
| LC-07 | defeito | `app/folha/export.py:716`; `app/folha/export.py:981` | Evento com valor e sem vínculo de rubrica é omitido pela montagem e homologação sem retenção correspondente. | sim | n/a | nenhuma | 11, 16 |
| LC-08 | lacuna | `app/folha/export.py:190`; `app/folha/export.py:711` | O helper de dias abonados para 8932 não é consumido pelo emissor do TXT. | sim | n/a | O137-8932-ENTERRAR — B:1491 | 11 |
| LC-09 | lacuna | `app/core/models.py:40`; `app/core/regua_cct.py:374`; `app/core/regua_cct.py:458` | Cláusulas escritas somente em `Sindicato.obs` não configuram os argumentos do motor. | sim | n/a | nenhuma | 12 |
| LC-10 | divergência | `app/ponto/motor_calculo_v2.py:2767`; `app/ponto/motor_calculo_v2.py:3232`; `app/ponto/calculador/regras.py:764` | O portão de intrajornada usa duração bruta no 12x36 e líquida no comercial e calculador. | sim | n/a | O281 — B:1523; parcial | 12 |
| LC-11 | lacuna | `app/escala/models.py:17`; `app/ponto/calculador/periodo.py:334`; `app/ponto/calculador/regras.py:764` | O piso de pausa de 15 minutos entre quatro e seis horas não atravessa os portões de indenização. | sim | n/a | O281 — B:1523; parcial | 12 |
| LC-12 | divergência | `app/core/regua_cct.py:449`; `app/ponto/motor_calculo_v2.py:758` | O switch altera a prorrogação noturna da mesma empresa CCT 12x36 sem aplicação vigente. | sim | n/a | nenhuma | 12 |
| LC-13 | divergência | `app/ponto/motor_calculo_v2.py:2578`; `app/ponto/motor_calculo_v2.py:3441`; `app/ponto/motor_calculo_v2.py:2639` | O DSR comercial recompõe presença pela data civil e o reflexo semanal também agrupa extras por data civil. | sim | 09/2026, exemplo | O76 — B:1320; O281 — B:1523 | 12 |
| LC-14 | lacuna | `app/ponto/utils.py:152`; `app/ponto/utils.py:191` | A seleção de cobertura não valida o intervalo de 11 horas entre jornadas. | nao | n/a | nenhuma | 12 |
| LC-15 | lacuna | `app/config/crons.py:17`; `app/config/crons.py:462`; `app/config/crons.py:1826` | A agenda fixa Juliani e as empresas 2, 3 e 4, deixando outros tenants e empresas fora da expansão. | sim | n/a | nenhuma | 12, 16 |
| LC-16 | defeito | `app/colaboradores/queries.py:86` | O filtro de empresas visíveis exclui empresas pelo prefixo literal de CNPJ `11222333`. | nao | n/a | nenhuma | 12 |
| LC-17 | risco | `app/core/views_usuarios.py:331`; `app/core/views_usuarios.py:341`; `app/core/credenciais.py:185` | Gerir usuários permite redefinir a senha de superusuário e receber sua senha temporária. | nao | n/a | RBAC-ESCRITA-POR-PAPEL — B:1725 | 13 |
| LC-18 | defeito | `app/colaboradores/views.py:2271`; `app/colaboradores/views.py:2303` | A escrita de agenda usa o ID do colaborador sem conferir sua empresa contra o alcance do ator. | sim | n/a | RBAC-ESCRITA-POR-PAPEL — B:1725 | 13 |
| LC-19 | defeito | `app/ponto/views.py:3032`; `app/ponto/services/cobertura.py:70`; `app/ponto/services/cobertura.py:100` | A cobertura aceita IDs sem conferir alcance empresarial ou vínculo obrigatório do chamado de origem. | sim | n/a | RBAC-ESCRITA-POR-PAPEL — B:1725 | 13 |
| LC-20 | defeito | `app/ponto/views.py:436`; `app/ponto/views.py:456`; `app/ponto/views.py:477` | Visualizar painel permite aprovar ou apagar batida pendente sem conferir alcance empresarial. | sim | n/a | RBAC-ESCRITA-POR-PAPEL — B:1725 | 13 |
| LC-21 | defeito | `app/colaboradores/views.py:2153`; `app/colaboradores/services/beneficios.py:47` | A permissão de leitura de benefícios autoriza gravar a configuração monetária de VA. | sim | 11/2026, exemplo | RBAC-ESCRITA-POR-PAPEL — B:1725 | 13 |
| LC-22 | risco | `app/core/views.py:974`; `app/core/views.py:978`; `app/core/views.py:1007` | O detalhe de batida expõe identificação, GPS, justificativa, histórico e URL de foto sem escopo empresarial. | nao | n/a | nenhuma | 13 |
| LC-23 | risco | `app/ponto/views.py:1938` **(movida)**; `app/ponto/views.py:3024` | Escritas por sessão dispensam CSRF na recusa de notificações e criação de cobertura. | sim | n/a | nenhuma | 13 |
| LC-24 | risco | `app/api/views_core.py:851`; `app/api/foto_recebida.py:61`; `app/api/foto_recebida.py:79` | O upload persiste conteúdo sem validar imagem e pode resolver a pendência `foto_ausente`. | sim | n/a | nenhuma | 13 |
| LC-25 | risco | `app/api/views_core.py:19`; `app/api/credencial.py:186`; `app/config/settings/saas.py:10` | JWT sem vínculo explícito ao tenant pode atravessar schemas nas condições de credenciais e chave descritas. | nao | n/a | nenhuma | 13 |
| LC-26 | risco | `app/api/credencial.py:124`; `app/api/credencial.py:138`; `app/api/credencial.py:150` | A revogação aceita JWT legado sem `iat` ou diante de erro na consulta ao estado. | nao | n/a | nenhuma | 13 |
| LC-27 | risco | `app/config/settings/base.py:6`; `mensageria/config/settings.py:3` | A falta das variáveis de configuração seleciona segredos literais previsíveis. | nao | n/a | nenhuma | 13 |
| LC-28 | risco | `app/core/ia_acoes.py:63`; `app/core/views_ia.py:908`; `mensageria/nucleo/ferramentas.py:394` | A entrada da IA une permissões e não transmite autorização específica por domínio às ferramentas. | nao | n/a | IA-DOMINIO-POR-FERRAMENTA — B:736 | 13 |
| LC-29 | risco | `mensageria/nucleo/views.py:26`; `mensageria/nucleo/views.py:55`; `mensageria/nucleo/views.py:83` | Console, chat e tratamento de alertas internos não exigem identidade autenticada. | nao | n/a | nenhuma | 13 |
| LC-30 | risco | `app/sse_server.py:31`; `app/sse_server.py:41`; `app/sse_server.py:73`; `app/sse_server.py:93` | SSE toma a existência de sessão como autorização para receber o canal agregado. | nao | n/a | nenhuma | 13 |
| LC-31 | risco | `app/api/views.py:2658`; `app/api/views.py:2674`; `app/api/views.py:2677` | A telemetria anônima aceita texto livre e seu emissor inclui CPF quando há usuário autenticado. | nao | n/a | nenhuma | 13 |
| LC-32 | defeito | `app/chamados/services/regularizacao_ext.py:131`; `app/chamados/services/regularizacao_ext.py:149` | A regularização externa pode apagar a batida original ou sobrescrever seu horário e tipo. | sim | n/a | PERGUNTA-SEM-SELO — B:728; parcial | 14 |
| LC-33 | defeito | `app/ponto/views.py:477`; `app/ponto/registro_batida.py:291` | Rejeitar batida offline pendente executa exclusão física. | sim | n/a | nenhuma | 14 |
| LC-34 | defeito | `app/ponto/services/flip_batida.py:25`; `app/ponto/registro_batida.py:254` | Flip troca E/S na própria linha sem preservar nela o tipo original. | sim | n/a | nenhuma | 14 |
| LC-35 | divergência | `app/ponto/models.py:64`; `app/ponto/services/espelho.py:1022`; `app/ponto/turnos.py:2903`; `app/templates/ponto/espelho.html:76` | Espelhos omitem retratadas apesar da promessa de asterisco e o feed cru não sinaliza retratação. | nao | n/a | nenhuma | 14 |
| LC-36 | divergência | `app/templates/ponto/espelho.html:237`; `app/api/views.py:1471`; `app/relatorios/pdf_espelho.py:881` | A tela pode mostrar o tipo do marco enquanto API e PDF mostram o tipo registrado. | nao | n/a | nenhuma | 14 |
| LC-37 | defeito | `app/api/views.py:557`; `app/api/views_core.py:551`; `app/static/pwa.js:158` | A deduplicação pode devolver batida retratada como sucesso e suprimir a nova tentativa. | sim | n/a | nenhuma | 14, 17 |
| LC-38 | risco | `app/ponto/registro_batida.py:21`; `app/ponto/registro_batida.py:138`; `app/ponto/services/triagem_batida.py:668` | A porta comum pode recusar uma marcação oposta distinta por proximidade temporal. | sim | n/a | nenhuma | 14 |
| LC-39 | risco | `app/api/views.py:415`; `app/api/views.py:449`; `app/api/views.py:462` | GPS ausente ou situação cadastral podem impedir a gravação de uma tentativa de ponto. | sim | n/a | nenhuma | 14 |
| LC-40 | defeito | `app/api/hora_do_aparelho.py:63`; `app/api/hora_do_aparelho.py:148`; `app/static/pwa.js:158` | Batida offline acima de 48 horas recebe timestamp efetivo da chegada e o PWA elimina a pendência após 2xx. | sim | n/a | nenhuma | 14, 17 |
| LC-41 | lacuna | `app/templates/core/glossario.html:155`; `app/ponto/views.py:57`; `app/ponto/views.py:70` | AFD é declarado pendente e não foi localizado gerador AEJ, cuja rota encontrada importa e responde 410. | nao | n/a | nenhuma | 14 |
| LC-42 | lacuna | `app/ponto/models.py:25`; `app/ponto/turnos.py:2365`; `app/ponto/turnos.py:2373` | A sequência reinicia por jornada e não demonstra NSR global sequencial. | nao | n/a | nenhuma | 14 |
| LC-43 | lacuna | `app/api/views.py:823`; `app/api/views.py:834` | O retorno de sucesso auditado não fornece hash, NSR ou comprovante assinado. | nao | n/a | nenhuma | 14 |
| LC-44 | risco | `app/ponto/registro_batida.py:254`; `app/ponto/registro_batida.py:263`; `app/core/auditoria.py:17` | Algumas alterações precedem auditoria cujas falhas são capturadas, permitindo perder a trilha. | sim | n/a | nenhuma | 14 |
| LC-45 | divergência | `app/templates/core/glossario.html:144`; `app/templates/core/glossario.html:145` | O glossário chama registrador por programa de REP-C e não sustenta a classificação REP-P solicitada. | nao | n/a | nenhuma | 14 |
| LC-46 | risco | `app/ponto/management/commands/processar_cartorio.py:61`; `app/ponto/services/cartorio.py:1306`; `app/ponto/services/cartorio.py:1362` | O cartório percorre histórico sem limite inferior e consulta ausências e batidas antes de pular impressão inalterada. | nao | n/a | nenhuma | 15 |
| LC-47 | risco | `app/ponto/portas/celula.py:598`; `app/escala/models.py:1286`; `app/escala/models.py:1310` | O gerador consulta folga por vínculo/dia antes do skip e grava células individualmente. | nao | n/a | nenhuma | 15 |
| LC-48 | risco | `app/ponto/portas/celula.py:872`; `app/ponto/portas/celula.py:875` | A apuração de furos atualiza cada dia e renova a avaliação mesmo sem mudança funcional. | nao | n/a | nenhuma | 15 |
| LC-49 | risco | `app/ponto/services/he_pendente_lavrado.py:94`; `app/ponto/services/he_pendente_lavrado.py:132` | O lavrador de HE monta espelho por ativo antes de descartar dias sem HE pendente. | nao | n/a | LAVRAR-HE-PENDENTE-ESCALA — B:750; parcial | 15 |
| LC-50 | risco | `app/escala/views_plano_folgas.py:59`; `app/escala/views_plano_folgas.py:76`; `app/escala/models.py:1092` | O GET do Plano de Folgas consulta relação por vínculo e célula por dia sem carga prévia. | nao | n/a | nenhuma | 15 |
| LC-51 | risco | `app/escala/views.py:407`; `app/escala/views.py:411`; `app/escala/views.py:416`; `app/escala/folgas.py:192` | Aplicar calendário ao tipo repete regenerações dos vínculos numa transação coletiva síncrona. | nao | n/a | nenhuma | 15 |
| LC-52 | risco | `app/core/views.py:177`; `app/core/views.py:376`; `app/core/views.py:779`; `app/core/views.py:1181`; `app/core/views.py:798` | Quatro painéis consultam turno individualmente e a TV filtra empresa depois da apuração. | nao | n/a | nenhuma | 15 |
| LC-53 | risco | `app/ponto/views.py:376`; `app/ponto/views.py:392`; `app/ponto/views.py:405` | A tela de turnos abertos percorre todos os ativos e consulta chamados antes do filtro de nível. | nao | n/a | nenhuma | 15 |
| LC-54 | risco | `app/folha/export.py:942`; `app/folha/export.py:1188`; `app/folha/export.py:1194` | O classificador da folha carrega células e vínculos separadamente por fechamento. | nao | n/a | nenhuma | 15 |
| LC-55 | risco | `app/folha/views.py:723`; `app/folha/views.py:764`; `app/folha/previa.py:58`; `app/folha/previa.py:170` | A prévia processa toda a população antes da paginação e repete retenção para propostas. | nao | n/a | nenhuma | 15 |
| LC-56 | risco | `app/relatorios/furos.py:124`; `app/relatorios/furos.py:160`; `app/colaboradores/services/calendario.py:221` | A auditoria de espelhos faz carga coletiva e depois recria calendários com cargas individuais por card. | nao | n/a | FUROS-EMPRESA-TIMEOUT — B:1775 | 15 |
| LC-57 | defeito | `app/colaboradores/views.py:499`; `app/ponto/janelas.py:15`; `app/ponto/janelas.py:26` | O corte 1 é convertido em 2 e impede representar competência civil de 1 ao último dia. | sim | 10–11/2026, exemplos | FERI-11 — B:2099 | 16 |
| LC-58 | divergência | `app/tenants/views.py:48`; `Caddyfile:25`; `app/config/settings/saas.py:115` | O domínio provisionado e coberto pelo wildcard não é liberado pela lista SaaS de hosts. | nao | n/a | nenhuma | 16 |
| LC-59 | divergência | `app/core/models.py:85`; `app/escala/models.py:17`; `app/escala/models.py:597` | O sindicato aceita mínimo de 30 minutos que a validação fixa da escala recusa acima de seis horas. | sim | n/a | nenhuma | 16 |
| LC-60 | lacuna | `app/tenants/bootstrap.py:27`; `app/core/regua_cct.py:444`; `app/core/regua_cct.py:366` | O bootstrap não completa a configuração trabalhista e a CCT depende de switch e aplicação vigente sem empate. | sim | n/a | nenhuma | 16 |
| LC-61 | lacuna | `app/folha/export.py:29`; `app/folha/export.py:846` | Falta de integração ativa ou código Domínio impede exportar pelos pré-requisitos cadastrais explícitos. | sim | n/a | nenhuma | 16 |
| LC-62 | risco | `app/folha/export.py:996`; `app/folha/export.py:1037`; `app/folha/export.py:1042` | A memória da janela ignora tenant na chave e pode reutilizar a janela de outro schema. | sim | 10/2026, exemplo | nenhuma | 16 |
| LC-63 | risco | `app/pautas/context_processors.py:23`; `app/pautas/context_processors.py:29`; `app/pautas/context_processors.py:33` | O cache de pautas usa somente ID do usuário e pode compartilhar número e remetentes entre tenants. | nao | n/a | nenhuma | 16 |
| LC-64 | defeito | `app/core/views_ia.py:23`; `app/core/views_ia.py:909`; `mensageria/nucleo/views.py:136` | O assistente envia o cliente global configurado em vez do tenant da requisição. | nao | n/a | nenhuma | 16 |
| LC-65 | lacuna | `app/core/constants.py:5`; `app/ponto/services/fechamento.py:204`; `app/escala/alimentacao.py:390` | O piso global de Juliani exclui fatias e previsto anteriores a 21/05/2026 de qualquer cliente. | sim | 05/2026, exemplo | nenhuma | 16 |
| LC-66 | divergência | `app/folha/porta_export.py:809`; `app/folha/porta_export.py:812` | A orientação para destravar o export manda medir no schema Juliani mesmo para outro tenant. | sim | n/a | nenhuma | 16 |
| LC-67 | defeito | `app/static/pwa.js:50`; `app/static/pwa.js:113`; `app/api/views_core.py:389` | A fila offline não guarda titular e pode enviar batida de A como pertencente à sessão de B. | sim | n/a | PWA-OFFLINE-APP-SHELL — B:1745 | 17 |
| LC-68 | defeito | `app/static/pwa.js:109`; `app/static/pwa.js:147`; `app/api/hora_do_aparelho.py:141` | A fila guarda horário corrigido pelo gap, mas envia somente o horário bruto do aparelho. | sim | n/a | nenhuma | 17 |
| LC-69 | lacuna | `app/static/pwa.js:143`; `app/ponto/models.py:106`; `app/api/hora_do_aparelho.py:148` | O reenvio offline não leva chave estável e pode duplicar evento antigo cujo timestamp vira novo agora. | sim | n/a | nenhuma | 17 |
| LC-70 | divergência | `app/static/pwa.js:143`; `app/ponto/services/geofence.py:117`; `app/ponto/services/geofence.py:121` | O reenvio offline perde a precisão GPS e pode mudar o julgamento da mesma localização. | nao | n/a | nenhuma | 17 |
| LC-71 | divergência | `app/templates/ponto/bater_ponto_v2.html:340`; `app/api/views_core.py:592` | A tela anuncia E/S local sem ler o tipo efetivamente corrigido pelo servidor. | nao | n/a | nenhuma | 17 |
| LC-72 | defeito | `app/static/sw.js:46`; `app/static/sw.js:67` | A navegação retorna antes de preencher o cache HTML e a visita online não prepara a reabertura offline. | nao | n/a | PWA-OFFLINE-APP-SHELL — B:1745 | 17 |
| LC-73 | lacuna | `app/static/sw.js:116`; `app/static/sw.js:122` | Background Sync sem página aberta apenas notifica para abrir o aplicativo e não envia a fila. | sim | n/a | nenhuma | 17 |
| LC-74 | divergência | `app/templates/ponto/bater_ponto_v2.html:163`; `app/templates/ponto/bater_ponto_v2.html:168`; `app/core/views.py:14` | O relógio passa da hora do servidor ao fuso local do aparelho quando entra no fallback. | nao | n/a | nenhuma | 17 |
| LC-75 | defeito | `app/api/views.py:1071`; `app/ponto/models.py:701`; `app/api/views.py:3101` | O teste do estado aposentado `pendente` oculta anexo faltante em `aguardando_documento`. | nao | n/a | nenhuma | 17 |
| LC-76 | divergência | `app/api/views.py:939`; `app/api/views_core.py:248`; `app/escala/models.py:1124` | As APIs anunciam horários do template mesmo quando a autoridade lê horários congelados na célula. | nao | n/a | nenhuma | 17 |
| LC-77 | lacuna | `app/api/views_core.py:1298`; `app/chamados/models.py:3214` | O payload de regularização omite operação, alvo e intervalo, tornando operações diferentes indistinguíveis. | sim | n/a | nenhuma | 17 |
| LC-78 | divergência | `app/api/views_core.py:1332`; `app/chamados/services/regularizacao_ext.py:58` | A confirmação com falha de materialização devolve HTTP 200 embora o corpo sinalize falha. | sim | n/a | nenhuma | 17 |
| LC-79 | divergência | `app/api/views.py:1172`; `app/api/views.py:2553`; `app/chamados/services/disputa_emissao.py:1267` | Um GET oferece `OUTRO_MOTIVO` para pergunta de hora enquanto outro oculta e o POST recusa essa opção. | nao | n/a | nenhuma | 17 |
| LC-80 | defeito | `app/static/pwa.js:246`; `app/static/pwa.js:335`; `app/static/pwa.js:367` | A inscrição push anuncia sucesso sem conferir recusa HTTP e a assinatura local impede renovação automática. | nao | n/a | nenhuma | 17 |
| LC-81 | risco | `app/static/pwa.js:335`; `app/ponto/views.py:1989`; `app/chamados/push_utils.py:24` | A associação WebPush pode permanecer no titular anterior após troca de conta no navegador. | nao | n/a | nenhuma | 17 |
| LC-82 | divergência | `app/chamados/push_utils.py:28`; `app/static/sw.js:97`; `app/static/sw.js:102` | O SW ignora URL e `requireInteraction` do emissor e pode abrir a raiz sem a persistência solicitada. | nao | n/a | nenhuma | 17 |
| LC-83 | defeito | `app/folha/export.py:834`; `app/folha/export.py:840` | Qualquer demissão não nula exclui do TXT mensal, inclusive quando posterior à janela examinada. | sim | 10/2

[truncado em 20000 de 30595 caracteres]

## 05/11 17:53 · sessao 881869f1c6db · FUNIL-28 · censo · origem=Code

`B:linha` significa `app/docs/BACKLOG.md:linha`. As origens aparecem pelo número e pelo nome completo no quadro final. A tabela conserva as alegações dos anexos; ocorrência e prejuízo em produção permanecem [nao verificado].

| id | classe | arquivo:linha | afirmacao (uma frase) | dinheiro | competencia | obra ou linha do BACKLOG que ja cobre | resposta(s) de origem |
|---|---|---|---|---|---|---|---|
| LB-01 | instrumentacao | `bin/gerar_estado.py:295`; `bin/gerar_estado.py:373` | O gerador consultado não demonstra N/15 nem sequência persistente de rodadas limpas reiniciada após violação. | sim | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-02 | instrumentacao | `app/chamados/services/vigia_fio.py:333` | O censo legado começa pelos chamados vivos e pode omitir célula acusadora sem chamado. | sim | n/a | PORTAO-FATURAMENTO-A1 — B:1493 | 01 |
| LB-03 | instrumentacao | `app/folha/fora_da_folha.py:128`; `app/folha/export.py:764`; `app/folha/porta_export.py:469`; `app/folha/porta_export.py:718` | O alcance e os comparadores limitados ao universo exportável podem omitir pessoas sem fechamento ou código Domínio e publicar zero apesar das divergências excluídas. | sim | n/a | PORTAO-FATURAMENTO-A1 — B:1493, quanto ao canal; demais comparadores: nenhuma | 01; 08 |
| LB-04 | instrumentacao | `app/chamados/management/commands/vigiar_fio_morto.py:93` (movida); `app/chamados/management/commands/vigiar_fio_morto.py:179` (movida) | O alcance positivo é carimbado, mas não participa de `gritou` no alarme do comando. | sim | n/a | PORTAO-FATURAMENTO-A1 — B:1493 | 01 |
| LB-05 | teste | `app/chamados/tests/test_o261e_f5_alcance.py:72` (movida) | O selo substitui o Resolver real por mock e não demonstra a integração completa da tela. | sim | n/a | PORTAO-FATURAMENTO-A1 — B:1493 | 01 |
| LB-06 | instrumentacao | `app/chamados/management/commands/fechar_becos_disputa.py:42`; `app/chamados/management/commands/fechar_becos_disputa.py:67` | Beco barrado não aciona o alarme reservado aos fecháveis. | nao | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-07 | instrumentacao | `app/chamados/services/fila.py:92`; `app/chamados/services/fila.py:122` | As faixas etárias da fila excluem chamados vivos silenciados e não medem a idade de todos os vivos. | nao | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-08 | instrumentacao | `app/folha/porta_export.py:239`; `app/folha/porta_export.py:451` | C1 aceita diferenças pela tolerância e pula campos quando falta o fechamento. | sim | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-09 | instrumentacao | `app/folha/porta_export.py:687`; `app/folha/porta_export.py:696` | A divergência entre fechamento e soma dos dias pagos fica fora das falhas que decidem o selo verde. | sim | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-10 | instrumentacao | `app/folha/porta_export.py:546`; `app/folha/porta_export.py:562` | C2 examina somente HE contra intrajornada e admite tolerância de um minuto. | sim | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-11 | instrumentacao | `app/ponto/tests/test_noturno_desconta_intervalo.py:50`; `app/ponto/tests/test_o280_bateria_da_lei.py:2440` | Os testes noturnos citados não demonstram censo C3 do adicional gravado contra a janela noturna. | sim | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-12 | teste | `app/ponto/tests/test_feriado_sumula146.py:64` | O selo com feriados injetados não certifica a alimentação pela praça do vínculo real. | sim | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-13 | instrumentacao | `app/folha/export.py:110`; `app/folha/export.py:129`; `app/ponto/services/fechamento.py:864` | A concordância dos agregados de falta não verifica C5 e pode coexistir com falta superior ao previsto. | sim | n/a | C5-FALTA-MAIOR-QUE-PREVISTO — B:1681; FALTA-ACIMA-DO-PREVISTO-[nome] — B:1496 | 01 |
| LB-14 | instrumentacao | `bin/r5_idempotencia_frota.py:26`; `bin/r5_idempotencia_frota.py:65`; `bin/r5_idempotencia_frota.py:72` | A fotografia do R5 omite rubricas e restringe o universo à competência e empresas fixadas. | sim | 10/2026 | PORTAO-FATURAMENTO-RESTO — B:1494; PLACAR-ESTRUTURAL — B:1135 | 01 |
| LB-15 | instrumentacao | `bin/r5_idempotencia_frota.py:108`; `bin/r5_idempotencia_frota.py:132` | Falhas absorvidas nas duas rodadas podem deixar fotografias iguais e produzir OK no R5. | sim | 10/2026 | PORTAO-FATURAMENTO-RESTO — B:1494; PLACAR-ESTRUTURAL — B:1135 | 01 |
| LB-16 | instrumentacao | `bin/placar_code.sh:186`; `bin/placar_code.sh:549`; `app/core/placar_estrutural.py:701` | Os verdes agregados cobrem menos que L-120 porque o TOTAL omite o loop e a cor estrutural mede a cadência do termômetro. | sim | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-17 | documentacao | `app/docs/LEIS.md:141`; `app/folha/porta_export.py:451` | A ficha “0 de 15” não descreve toda a instrumentação parcial encontrada. | nao | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-18 | documentacao | `app/docs/LEIS.md:141`; `app/docs/BACKLOG.md:1494` | A2–A5 e B1–B4 aparecem agrupadas sem cláusulas individuais e associação individual a contadores demonstradas. | nao | n/a | PORTAO-FATURAMENTO-RESTO — B:1494 | 01 |
| LB-19 | instrumentacao | `app/folha/porta_export.py:433` | Falha na consulta de lavratura sem furo apurado deixa somente aviso, sem divergência ou estado de não medição. | sim | n/a | nenhuma | 01; 08 |
| LB-20 | instrumentacao | `app/folha/porta_export.py:652` | Falha ao consultar DiaPago reduz o universo ao gravado e deixa somente aviso no resultado correspondente. | sim | n/a | nenhuma | 01 |
| LB-21 | implementacao | `app/ponto/services/cartorio.py:1407`; `app/chamados/juizes.py:413` (movida) | `_protesta` conserva decisão própria por `trabalha` e pode protestar em dia declarado sem trabalho pelo juiz canônico. | sim | n/a | nenhuma | 02 — L-002; L-061 |
| LB-22 | implementacao | `app/colaboradores/services/vinculo.py:615`; `app/colaboradores/services/desligamento.py:73` | Desligamento sem request pode fechar vínculos sem a trilha por PK condicionada à presença de autor. | nao | n/a | nenhuma | 02 |
| LB-23 | teste | `bin/tests/test_juiz_novo_tem_corte.sh:28`; `bin/tests/test_juiz_novo_tem_corte.sh:57` | O extrator ignora caminhos com dígitos e aspas duplas, enquanto o caso adversarial não atravessa essa extração. | nao | n/a | nenhuma | 02 |
| LB-24 | implementacao | `app/ponto/portas/celula.py:225`; `app/ponto/portas/celula.py:233` | A regeneração além do fim do vínculo grava `trabalha/dna` sem os campos de denúncia da regeneração. | sim | n/a | nenhuma | 02; 03 |
| LB-25 | implementacao | `app/escala/models.py:1280`; `app/escala/models.py:1289`; `app/escala/models.py:1351` | Mês sem foto pode retornar a paridade da âncora antes de herdar a foto de folga de outro mês. | sim | n/a | nenhuma | 02 |
| LB-26 | implementacao | `app/ponto/motor_calculo_v2.py:1254`; `app/ponto/motor_calculo_v2.py:1273`; `app/ponto/motor_calculo_v2.py:3503` (movida) | A indisponibilidade do previsto canônico permite fallback para a jornada cadastrada e para 480 minutos no Plantão. | sim | n/a | nenhuma | 02 |
| LB-27 | implementacao | `app/escala/services/cadastro_realidade.py:199` | `_jornada_que_mente` usa mês civil e corte literal 21 em vez da competência corrente da empresa. | nao | n/a | LISTA-POR-COMPETENCIA — B:1490 | 02; 05 |
| LB-28 | implementacao | `app/ponto/registro_batida.py:145`; `app/api/views_core.py:402`; `app/api/views_core.py:435`; `app/api/views_core.py:454` | Situação, GPS e colisão próxima podem recusar batidas apesar da formulação literal “nunca barrada”. | sim | n/a | nenhuma | 02 |
| LB-29 | implementacao | `app/ponto/juiz_batida.py:310`; `app/ponto/motor_calculo_v2.py:822`; `app/ponto/motor_calculo_v2.py:2773` | O motor indeniza intervalo suprimido sem consultar `intervalo_indenizavel`, pendência declarada no código. | sim | n/a | nenhuma | 02 |
| LB-30 | implementacao | `app/ponto/juiz_batida.py:294`; `app/ponto/motor_calculo_v2.py:2774` | O ramo desconta somente intervalo real e não realiza a cláusula literal de subtrair o intervalo cadastrado. | sim | n/a | nenhuma | 02 |
| LB-31 | implementacao | `app/core/regua_cct.py:449`; `app/ponto/motor_calculo_v2.py:758` | Com o switch CCT desligado, parâmetro indefinido pode aplicar à empresa não CLT uma regra noturna derivada do ciclo. | sim | n/a | nenhuma | 02 |
| LB-32 | implementacao | `app/relatorios/pdf_espelho.py:461`; `app/relatorios/pdf_espelho.py:471`; `app/relatorios/cartao_pela_celula.py:321` | O PDF pode preservar faltas do motor quando período livre ou fonte indisponível impede a sobrescrita pelos totais da folha. | sim | n/a | nenhuma | 02 |
| LB-33 | implementacao | `app/escala/services/cadastro_realidade.py:555`; `app/escala/services/cadastro_realidade.py:635`; `app/ponto/services/esmeril_espelho.py:159` | Cadastro x Realidade não demonstra ingestão integral dos dias acusados pelo motor e pode omitir ocorrência abaixo dos limiares de repetição. | nao | n/a | CXR-TELA-LE-O-MOTOR — B:775 | 02 |
| LB-34 | implementacao | `app/escala/services/cadastro_realidade.py:250`; `app/escala/services/cadastro_realidade.py:449`; `app/escala/services/propositor.py:382` | C1/VINCULO-FIM oferece texto e link genérico sem decisão do propositor canônico para essa classe. | nao | n/a | nenhuma | 02 |
| LB-35 | documentacao | `app/docs/LEIS.md:91`; `app/docs/LEIS.md:189` | A seção de dívidas da L-050 contradiz a reauditoria principal e os caminhos já sem a antiga fronteira de exportação. | nao | n/a | nenhuma | 02 |
| LB-36 | implementacao | `app/config/crons.py:1999`; `app/config/crons.py:2004`; `app/config/crons.py:2018` | Três escritores residuais de perguntas, cobrança com lastro e reconciliação continuam como juízes por varredura. | nao | n/a | CHAMADO-EM-RAIA — B:1351; E4-INCLUI-CONTRATO-20 — B:792 | 02 |
| LB-37 | implementacao | `app/chamados/services/cobrar_dia.py:26` | A cobrança manual recusa todo dia corrente pela data civil, inclusive turno já encerrado naquele dia. | nao | n/a | nenhuma | 02 |
| LB-38 | instrumentacao | `bin/regua.sh:165`; `bin/isolamento.sh:34`; `bin/regua_relogio.sh:67`; `bin/regua_calendario.sh:43` | Leitores da suíte ainda decidem pelo texto do log e o calendário executa testes sem a trava comum. | nao | n/a | O240 — B:1168, quanto ao log; trava: nenhuma | 02 |
| LB-39 | implementacao | `bin/db_teste.sh:55`; `bin/tests/test_segredo_fora_do_argv.sh:19` | A criação do banco passa `POSTGRES_PASSWORD` no argv e escapa ao selo limitado a `DB_PASSWORD`. | nao | n/a | nenhuma | 02 |
| LB-40 | implementacao | `app/ponto/motor_calculo_v2.py:3313`; `app/ponto/motor_calculo_v2.py:3441`; `app/ponto/motor_calculo_v2.py:3466` | O segundo cálculo comercial de DSR recria trabalho e extras pela data civil da entrada em vez do dia da jornada. | sim | n/a | nenhuma | 02 |
| LB-41 | implementacao | `app/ponto/motor_calculo_v2.py:559`; `app/ponto/motor_calculo_v2.py:573`; `app/ponto/turnos.py:1380` | Turno partido, célula ausente e ata aberta ou desalinhada ainda deixam o tipo gravado decidir no fallback do pareador. | sim | n/a | nenhuma | 02 |
| LB-42 | implementacao | `app/ferias/models.py:144` (movida); `app/ferias/models.py:54` | O contador aquisitivo inclui falta aprovada em folga e pode reduzir o direito de férias por linha fora do universo de trabalho. | sim | n/a | nenhuma | 02 |
| LB-43 | implementacao | `app/inteligencia/management/commands/calcular_scores.py:57`; `app/inteligencia/management/commands/calcular_scores.py:93`; `app/relatorios/views.py:195` | Assiduidade e absenteísmo recortam mês civil em vez da competência. | nao | n/a | TELA-112 — B:1907, quanto ao absenteísmo; assiduidade: nenhuma | 02 |
| LB-44 | implementacao | `app/ponto/motor_calculo_v2.py:3635`; `app/ponto/motor_calculo_v2.py:3668` | A fábrica corrige pelo desenho somente alguns templates rotulados como turno partido e seleciona os demais motores pelo rótulo. | sim | n/a | nenhuma | 02 |
| LB-45 | implementacao | `app/ponto/services/dia_pago.py:333`; `app/ponto/services/dia_pago.py:357` | A relavra apaga e recria linhas pela chave `versao`, sem preservar revisões anteriores e DIFF por rubrica. | sim | n/a | nenhuma | 02 |
| LB-46 | implementacao | `app/escala/services/cadastro_tipo.py:348`; `app/escala/services/cadastro_tipo.py:357` | Salvar template regenera células e invalida fechamentos sem concluir o recálculo no processamento em lote. | sim | n/a | E2-D3-TRABALHA-FORA-DA-IMPRESSAO — B:955 | 02 |
| LB-47 | instrumentacao | `app/core/placar_estrutural.py:634`; `app/core/placar_estrutural.py:664`; `app/core/placar_estrutural.py:701` | O predicado de cadência não recebe o último pouso e pode continuar verde após pouso sem nova medição. | sim | n/a | nenhuma | 02 |
| LB-48 | teste | `CLAUDE.md:431`; `app/docs/LEIS.md:146`; `bin/relato_afirma_com_prova.py:36` | O checker encontrado exige prova do ato, mas não verifica teto de linhas nem novidade da seção ancorada em commit. | nao | n/a | RELATO-CURTO-SELO — B:1431 | 02 |
| LB-49 | teste | `app/core/tests/test_contract_escritores_estado.py:13` (movida); `app/ponto/tests/test_chokepoint_celula_gate.py:29` | P13 procura campos em seis apps sem resolver entidade, enquanto o gate descarta escritas cuja entidade não resolve. | sim | n/a | ESMERIL-MECANICO — B:1101 | 03 |
| LB-50 | implementacao | `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`; `app/escala/tests/test_d3_guarda_da_linha_celula.py:72` | A edição humana prometida da célula não tem porta nem escritor de produção para `editada_em/editada_por`. | sim | n/a | E2-D3-EDICAO-HUMANA-SEM-PORTA — B:962 | 03 |
| LB-51 | teste | `app/escala/tests/test_d3_guarda_da_linha_celula.py:70`; `app/ponto/tests/test_d3_impressao_cobre_insumos.py:171` | `trabalha` é exceção da impressão de insumos e mudar somente trabalho para folga pode não mudar o hash. | sim | n/a | E2-D3-TRABALHA-FORA-DA-IMPRESSAO — B:955 | 03 |
| LB-52 | implementacao | `app/ponto/services/triagem_batida.py:734` (movida); `app/api/views.py:723` (movida); `app/api/views_core.py:720` (movida) | Três cascas escrevem `OrdemSubstituicao.status='em_curso'` fora da porta declarada de cobertura. | sim | n/a | ESMERIL-MECANICO — B:1101 | 03 |
| LB-53 | documentacao | `app/chamados/services/validacao.py:131`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/models.py:2581` | A descrição de validação como escritor único de `validada_em/por` não abrange os demais escritores autorizados. | nao | n/a | O261-E-F5C-FILA-DO-OPUS — B:862 | 03 |
| LB-54 | concorrencia | `app/ponto/views.py:1875`; `app/ponto/services/justificativa.py:262` | A decisão em lote fora da porta pode sobrescrever aprovação individual concorrente por ler pendência sem lock. | sim | n/a | ESMERIL-MECANICO — B:1101, quanto aos escritores; corrida: nenhuma | 03 |
| LB-55 | implementacao | `app/ponto/services/ausencia.py:1267`; `app/ponto/models.py:870` | A rejeição de férias por update em lote evita o preenchimento de `aprovada_em` feito pelo save do model. | sim | n/a | nenhuma | 03 |
| LB-56 | implementacao | `app/ponto/services/fechamento.py:1316`; `app/ponto/services/fechamento.py:1330`; `app/ponto/services/fechamento.py:1338` | Aprovar ou reabrir fechamento pode mudar o estado sem atualizar `atualizado_em`. | sim | n/a | CARIMBO-POR-ORIGEM — B:1560 | 03 |
| LB-57 | documentacao | `app/ponto/services/fechamento.py:1354`; `app/ponto/services/fechamento.py:1364` | A restauração aceita alterar status apesar da promessa textual de não tocá-lo. | sim | n/a | nenhuma | 03 |
| LB-58 | implementacao | `app/core/credenciais.py:165`; `app/colaboradores/models.py:420` | O save pode forçar onboarding completo depois de a porta solicitar seu reset para falso. | nao | n/a | nenhuma | 03 |
| LB-59 | concorrencia | `app/ferias/services.py:499`; `app/ferias/services.py:503` | Confirmar e recusar férias concorrentemente pode deixar confirmação recusada com instante de confirmação preenchido. | nao | n/a | nenhuma | 03 |
| LB-60 | implementacao | `app/holerite/servicos.py:113`; `app/holerite/views.py:1821` | O sucesso do aviso pelo service limpa a falha anterior, enquanto o aviso em lote a preserva. | nao | n/a | nenhuma | 03 |
| LB-61 | implementacao | `app/chamados/management/commands/backfill_veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:227` | O backfill pula pergunta com veredito igual mesmo quando a janela difere, ao contrário da lavra normal. | nao | n/a | nenhuma | 03 |
| LB-62 | documentacao | `app/chamados/services/disputa_emissao.py:1340`; `app/chamados/services/validacao.py:545`; `app/chamados/services/fio.py:892` | `recusa_motivo` recebe fatos de naturezas diferentes e não identifica exclusivamente a última recusa do materializador. | nao | n/a | nenhuma | 03 |
| LB-63 | implementacao | `app/chamados/services/regularizacao_ext.py:264`; `app/chamados/services/regularizacao_ext.py:266` | A recusa direta de regularização materializada muda o status sem limpar o carimbo de materialização. | sim | n/a | nenhuma | 03 |
| LB-64 | implementacao | `app/ponto/turnos.py:1229`; `app/ponto/turnos.py:1237` | O pareador escolhe marcos pela data civil da batida e pode usar o DNA do dia seguinte durante turno noturno. | sim | n/a | DIA-DA-BATIDA-JUIZ-UNICO — B:1683 | 04 |
| LB-65 | implementacao | `app/ponto/services/fechamento.py:245`; `app/ponto/services/fechamento.py:252` | A apuração por fatia filtra batidas pela data UTC e pode excluir batidas pertencentes ao turno julgado. | sim | n/a | PERGUNTA-SEM-SELO, item 4 — B:728 | 04 |
| LB-66 | implementacao | `app/escala/servico_jornada.py:203`; `app/escala/utils.py:963` | As duas funções de saída esperada podem avançar um dia por ancorarem a entrada tardia na data civil após meia-noite. | sim | n/a | DIA-DA-BATIDA-JUIZ-UNICO — B:1683 | 04 |
| LB-67 | implementacao | `app/api/views.py:745`; `app/api/views_core.py:745` | O emissor de desvio leve grava o dia civil da saída apesar de o detector resolver outro dia do turno. | sim | n/a | DIA-DA-BATIDA-JUIZ-UNICO — B:1683 | 04 |
| LB-68 | implementacao | `app/chamados/detector_anomalias.py:144`; `app/chamados/detector_anomalias.py:164` | O detector escolhe o vínculo pela data civil da batida e o conserva depois de resolver o dia do turno. | sim | n/a | DIA-DA-BATIDA-JUIZ-UNICO — B:1683 | 04 |
| LB-69 | implementacao | `app/ponto/services/espelho.py:84`; `app/ponto/services/espelho.py:85` | O resumo por competência pode excluir períodos após meia-noite pertencentes à jornada do último dia. | sim | n/a | DIA-DA-BATIDA-JUIZ-UNICO — B:1683 | 04 |
| LB-70 | implementacao | `app/escala/detector_proposta.py:18`; `app/escala/detector_proposta.py:58` | O propositor divide noites por dias civis e pode produzir quantidade de dias, saída e padrão incorretos. | nao | n/a | DIA-DA-BATIDA-JUIZ-UNICO — B:1683 | 04 |
| LB-71 | implementacao | `app/folha/export.py:767`; `app/ponto/services/fechamento.py:612`; `app/ponto/models.py:485` | A empresa atual recebe o fechamento inteiro e redefine sua janela histórica após transferência, com risco de sobreposição de exportações. | sim | n/a | nenhuma | 05 |
| LB-72 | implementacao | `app/core/regua_cct.py:329`; `app/core/regua_cct.py:352`; `app/core/regua_cct.py:366` | A régua encontra o vínculo histórico, mas escolhe regime e convenção pela empresa atual. | sim | n/a | nenhuma | 05 |
| LB-73 | implementacao | `app/ponto/janelas.py:85`; `app/ponto/services/flip_batida.py:22` | A trava de batida histórica pode consultar outra competência após transferência entre empresas com cortes diferentes. | sim | n/a | nenhuma | 05 |
| LB-74 | implementacao | `app/ponto/services/fechamento.py:232`; `app/ponto/services/fechamento.py:236`; `app/ponto/services/fechamento.py:250` | A partição pode entregar as mesmas batidas a dois vínculos sem células ou apurar previsto em dias atribuídos ao outro vínculo. | sim | n/a | nenhuma | 05 |
| LB-75 | implementacao | `app/colaboradores/views.py:1385`; `app/colaboradores/views.py:1392` | Transferir o posto altera 

[truncado em 20000 de 44955 caracteres]

## 05/11 17:50 · sessao 274e22dbbb6d · FUNIL-28 · censo · origem=Code

Referências relativas a `app/`, salvo `bin/`. **B:n** = `app/docs/BACKLOG.md:n`. Nas fusões, a classe a–e é a da primeira ocorrência; todas as origens estão citadas. Ocorrência e impacto efetivo em produção: **[nao verificado]**.

| id | classe | arquivo:linha | afirmacao (uma frase) | dinheiro | competencia | obra ou linha do BACKLOG que ja cobre | resposta(s) de origem |
|---|---|---|---|---|---|---|---|
| LA-01 | b | docs/LEIS.md:81; folha/export.py:1147; folha/export.py:1235 | A coluna antiga descreve retenção universal de furo, enquanto o retentor isenta furo exclusivamente de pausa. | sim | n/a | O236 — B:1164 | lote1.r:01; lote2.r:28; lote6.r:01 |
| LA-02 | b | docs/LEIS.md:93; ponto/portas/celula.py:196; ponto/portas/regen_exportada.py:174 | A proibição absoluta de corrigir exportada diverge da porta de retificação com motivo e abertura da guarda. | sim | n/a | O121 — B:1189 | lote1.r:03; lote4.r:11; lote6.r:04 |
| LA-03 | b | docs/LEIS.md:113; folha/views.py:208; ponto/services/fechamento.py:277 | A geração do TXT alcança o motor por recálculo em leitura, apesar da promessa de ler somente a lavratura. | sim | n/a | O-DIA-PAGO S4 — B:1095 | lote1.r:04 |
| LA-04 | b | docs/LEIS.md:115; colaboradores/models.py:115; folha/porta_export.py:230; folha/porta_export.py:690 | A trava de HE considera apenas dias acima do limite sem decisão e depende da configuração da empresa, contrariando as formulações universais. | sim | n/a | O214, item 3 — B:1134 | lote1.r:05; lote7.r:10 |
| LA-05 | b | docs/LEIS.md:149; ponto/portas/celula.py:198; ponto/portas/celula.py:238 | A regeneração ordinária continua bloqueando competências exportadas sem `apesar_da_lavra`, apesar da revogação da trava. | sim | n/a | O121-A1-VINCULO-CORRIGE-O-PASSADO — B:1008 | lote1.r:07; lote4.r:12; lote6.r:05 |
| LA-06 | b | docs/LEIS.md:149; folha/management/commands/regerar_txt_dominio.py:118 | Os artefatos do regerador não demonstram publicação do DIFF antes do ato de frota. | sim | n/a | nenhuma | lote1.r:08 |
| LA-07 | d | folha/models.py:97; folha/models.py:134; folha/models.py:137 | `invalidada_em` nulo não garante vigência, porque ela também depende de ser a geração mais recente. | sim | n/a | nenhuma | lote1.r:09; lote2.r:19 |
| LA-08 | a | folha/views.py:213; folha/views.py:247; folha/views.py:268 | Entrar no TXT não garante aprovação, porque o carimbo exclui itens com determinadas travas. | sim | n/a | FECH-05 — B:2069 | lote1.r:10; lote7.r:03 |
| LA-09 | e | folha/tests/test_export_59a.py:1; folha/export.py:411 | A docstring proíbe universalmente parcela de feriado em 12x36, mas o emissor permite a parcela com cláusula de dobra. | sim | n/a | nenhuma | lote1.r:11 |
| LA-10 | a | ponto/services/fechamento.py:476; ponto/views.py:1248; ponto/services/fechamento.py:552 | As promessas de recalcular qualquer exportada omitem a exclusão específica de setembro de 2026. | sim | 09/2026 | nenhuma | lote1.r:18; lote7.r:02 |
| LA-11 | a | ponto/services/fechamento.py:1441; folha/porta_export.py:417; docs/LEIS.md:125; ponto/services/fechamento.py:1144 | As promessas de congelamento de competência trancada, paga ou exportada divergem da escrita que pode atualizar valores fora da guarda de setembro. | sim | 10/2026; guarda de 09/2026 | QUEM-ESCREVE-NO-GRAVADO-DA-EXPORTADA — B:1091 | lote1.r:19; lote2.r:20; lote5.r:B1 |
| LA-12 | a | ponto/services/fechamento.py:1182; colaboradores/services/calendario.py:290; ponto/services/dia_pago.py:852 | O comentário de que ninguém lê `DiaPago` ficou antigo diante dos leitores existentes. | nao | n/a | O-DIA-PAGO S3 — B:1090 | lote1.r:24 |
| LA-13 | a | holerite/models.py:138; holerite/views.py:1191; holerite/views.py:1193 | O upload avulso apaga holerite anterior sem ciência apesar da promessa de preservar versões. | nao | n/a | nenhuma | lote1.r:25 |
| LA-14 | d | holerite/models.py:123; holerite/views.py:1202 | O upload avulso grava a empresa atual do colaborador apesar do help definir a empresa impressa no documento. | nao | n/a | nenhuma | lote1.r:26 |
| LA-15 | a | holerite/models.py:143; holerite/motor.py:448; holerite/motor.py:480 | A primeira publicação conjunta deixa `anexos_sha` vazio, permitindo anexar novamente a mesma carta na republicação. | nao | n/a | nenhuma | lote1.r:29 |
| LA-16 | a | holerite/motor.py:180; holerite/motor.py:186 | As afirmações históricas de recibos sem CPF e correspondência 29/29 carecem dos documentos e do cadastro usados na comparação. | nao | n/a | nenhuma | lote1.r:32 |
| LA-17 | d | ponto/models.py:757; ponto/models.py:875; ponto/models.py:883 | O help fixa falta inteira em 720 minutos, mas o preenchimento automático soma o previsto diário e preserva minutos positivos informados. | sim | n/a | nenhuma | lote1.r:33; lote2.r:18; lote3.r:15; lote7.r:05 |
| LA-18 | a | ponto/motor_calculo_v2.py:53; ponto/motor_calculo_v2.py:2845; ponto/motor_calculo_v2.py:2851 | A promessa de vinte minutos diários sem HE diverge do ramo que elimina excedentes somente até dez minutos. | sim | n/a | nenhuma | lote2.r:01 |
| LA-19 | a | ponto/motor_calculo_v2.py:2104; ponto/motor_calculo_v2.py:2122; ponto/motor_calculo_v2.py:2148 | O comentário exclui genericamente atraso após três horas, mas o código restringe essa exclusão ao dia sem marcos de intervalo. | sim | n/a | nenhuma | lote2.r:02 |
| LA-20 | a | ponto/motor_calculo_v2.py:2807; ponto/motor_calculo_v2.py:934; docs/LEIS.md:118 | O comentário e a cláusula original excluem dobra em feriado previsto, enquanto o helper decide pela política do ciclo sem exigir dia não previsto. | sim | n/a | nenhuma | lote2.r:03,31 |
| LA-21 | c | ponto/motor_calculo_v2.py:3226; ponto/motor_calculo_v2.py:3232; escala/models.py:19 | O ramo comercial não indeniza pausa faltante acima de quatro e até seis horas apesar do piso de quinze minutos da tabela consultada. | sim | n/a | O270 — B:1511 | lote2.r:04 |
| LA-22 | a | ponto/calculador/alimentacao.py:333; ponto/calculador/nucleo.py:38; ponto/calculador/nucleo.py:186 | A narrativa atribui deduplicação de dez minutos ao núcleo que registra sua retirada e percorre todas as batidas. | sim | n/a | nenhuma | lote2.r:11 |
| LA-23 | e | ponto/tests/test_calculador_portoes_de_marco.py:8; ponto/calculador/regras.py:251; escala/regua_defesa.py:472 | A docstring descreve perdão da entrada por proximidade da volta, mas o código consulta o marco ocupado e sua conformidade. | sim | n/a | nenhuma | lote2.r:16 |
| LA-24 | b | docs/LEIS.md:79; ponto/motor_calculo_v2.py:822; ponto/motor_calculo_v2.py:2784 | A cláusula original atribui a indenização a `intervalo_indenizavel`, enquanto o motor declara não ler a flag e mantém a indenização do suprimido. | sim | n/a | nenhuma | lote2.r:26 |
| LA-25 | b | docs/LEIS.md:80; ponto/motor_calculo_v2.py:2773; ponto/motor_calculo_v2.py:2784 | A afirmação de que o intervalo cadastrado sempre sai da jornada diverge da preservação do trabalho sem pausa reconhecida com indenização separada. | sim | n/a | nenhuma | lote2.r:27 |
| LA-26 | b | docs/LEIS.md:83; core/regua_cct.py:453; ponto/motor_calculo_v2.py:758 | O construtor pode excluir prorrogação pós-05h em 12x36 sem CLT declarada, contrariando o “somente” documental. | sim | n/a | nenhuma | lote2.r:29 |
| LA-27 | b | docs/LEIS.md:84; core/regua_cct.py:150 | O valor falso da constante nesta cópia não demonstra sua configuração efetiva na implantação de produção. | sim | n/a | nenhuma | lote2.r:30 |
| LA-28 | b | docs/LEIS.md:119; ponto/motor_calculo_v2.py:573; ponto/turnos.py:1380 | Sem alimentação útil da ata, o pareador pode usar o tipo gravado, contrariando sua exclusão universal. | sim | n/a | nenhuma | lote2.r:32 |
| LA-29 | b | docs/LEIS.md:124; escala/models.py:174; ponto/motor_calculo_v2.py:3635; ponto/motor_calculo_v2.py:3668 | As descrições da seleção do motor divergem do despacho que ainda usa `tipo_base` e aplica correção geométrica somente a determinados ramos. | sim | n/a | O136 — B:1346 | lote2.r:33; lote4.r:03,13 |
| LA-30 | b | docs/LEIS.md:113; ponto/services/fechamento.py:1211; ponto/services/dia_pago.py:357 | Lavraturas sucessivas em `versao='motor'` apagam e recriam linhas anteriores apesar da promessa de preservar o passado em nova versão. | sim | n/a | nenhuma | lote2.r:34 |
| LA-31 | b | docs/LEIS.md:170; ponto/services/fechamento.py:1214; ponto/services/dia_pago.py:345 | A nota de que ninguém preenche `versao_motor` diverge do fechamento que fornece o carimbo e do escritor que o utiliza. | nao | n/a | nenhuma | lote2.r:35 |
| LA-32 | b | docs/LEIS.md:111; ponto/motor_calculo_v2.py:1194; ponto/motor_calculo_v2.py:1196 | O arredondamento separado de atraso e antecipada pode produzir soma ligeiramente superior ao teto aritmético. | sim | n/a | nenhuma | lote2.r:36 |
| LA-33 | b | docs/LEIS.md:121; escala/utils.py:1231; ferias/models.py:144; ferias/services.py:246 | A promessa de ausência sem efeito nem consumo em folga diverge do overlay e da contagem de faltas aquisitivas sem filtro de jornada. | sim | n/a | nenhuma | lote3.r:01 |
| LA-34 | b | docs/LEIS.md:87; relatorios/pdf_espelho.py:461; relatorios/cartao_pela_celula.py:322 | O PDF pode apresentar como “Faltas” dias sem decisão quando conserva o total produzido pelo motor. | nao | n/a | nenhuma | lote3.r:02 |
| LA-35 | a | ponto/precedencia.py:519; ponto/precedencia.py:520; ponto/precedencia.py:536 | O código atual não comprova o censo histórico de dez faltas aprovadas fora da jornada em setembro e zero em outubro. | sim | 09/2026 e 10/2026 | O198 — B:1409 | lote3.r:06 |
| LA-36 | e | ferias/test_services_leitura.py:2; ferias/services.py:118; ferias/models.py:257 | Serviço e property de vencimento usam status gravado e derivado diferentes, contrariando a concordância “SEMPRE”. | nao | n/a | nenhuma | lote3.r:07 |
| LA-37 | d | ferias/models.py:122; ferias/services.py:473 | O limite anunciado como doze meses após o fim é calculado somando 365 dias. | nao | n/a | nenhuma | lote3.r:08 |
| LA-38 | d | ferias/models.py:307; ferias/services.py:314; ferias/services.py:323 | `aviso_emitido` é calculado pela antecedência do agendamento sem informação de emissão participar da decisão. | nao | n/a | nenhuma | lote3.r:09 |
| LA-39 | d | ponto/models.py:584; ponto/services/ausencia.py:333; ponto/services/ausencia.py:779 | A exigência documental anunciada como recusa permite criar ausência aguardando documento. | sim | n/a | nenhuma | lote3.r:12; lote7.r:06 |
| LA-40 | d | ponto/models.py:586; api/views.py:683; api/views_core.py:680; ponto/services/aviso_da_batida.py:79 | `bloqueia_ponto` anuncia impedimento, mas o fluxo pode registrar a batida e depois avisar sobre a ausência. | sim | n/a | nenhuma | lote3.r:13; lote7.r:07 |
| LA-41 | d | ponto/models.py:588; ponto/catalogo/ausencias.py:358; ponto/turnos.py:2598 | A flag parcial permite lançamento por horas, enquanto a cobertura parcial depende também de um único dia e minutos estritamente entre zero e 480. | sim | n/a | nenhuma | lote3.r:14 |
| LA-42 | a | api/views.py:815; chamados/tests/test_invariantes_disputa.py:193; docs/LEIS.md:78; api/views.py:423; api/views.py:466 | As promessas de batida nunca recusada ou apenas avisada divergem das recusas por férias e GPS ausente. | sim | n/a | nenhuma | lote3.r:16; lote5.r:E1; lote7.r:08 |
| LA-43 | e | ponto/tests/test_atestados_lote.py:11; ponto/services/atestados_lote.py:60; ponto/services/atestados_lote.py:77 | A promessa de que documento faltante nunca some por filtro diverge da filtragem anterior por tipo e status. | nao | n/a | nenhuma | lote3.r:19 |
| LA-44 | a | chamados/services/materializacao.py:61; chamados/services/materializacao.py:78; ponto/turnos.py:2555 | O comentário conserva o critério antigo de intervalo aberto por bloqueio de ponto, enquanto a consulta usa o conjunto atual de tipos indeterminados. | sim | n/a | nenhuma | lote3.r:25 |
| LA-45 | a | ponto/precedencia.py:13; ponto/precedencia.py:376; escala/models.py:1392 | A exceção textual de feriado exige `trabalha_em_feriado=True`, mas a implementação também considera o ciclo. | sim | n/a | nenhuma | lote3.r:26 |
| LA-46 | a | chamados/juizes.py:414 (movida); chamados/juizes.py:426 (movida) | A descrição de dia sem trabalho omite a exceção da cobertura parcial que preserva a cobrança do restante. | sim | n/a | nenhuma | lote3.r:27 |
| LA-47 | d | escala/models.py:191; core/regua_cct.py:458; ponto/motor_calculo_v2.py:3671 | O campo de perfil promete regras e padrão, embora o caminho atual resolva parâmetros pela régua sem consultar essa FK. | sim | n/a | nenhuma | lote4.r:04 |
| LA-48 | d | escala/models.py:1699; ponto/management/commands/gerar_celulas.py:16; ponto/management/commands/gerar_celulas.py:96 | O help anuncia DNA versão 1, enquanto o produtor grava versão 2. | nao | n/a | nenhuma | lote4.r:05; lote6.r:07 |
| LA-49 | b | docs/LEIS.md:71; ponto/portas/celula.py:233; ponto/portas/celula.py:259 | A correção fora da vigência reescreve DNA sem atualizar DNA anterior, instante e contador de regeneração. | sim | n/a | nenhuma | lote4.r:10; lote6.r:03 |
| LA-50 | a | ponto/portas/celula.py:349; ponto/portas/celula.py:375; ponto/portas/celula.py:397 | A falha capturada no rejulgamento da ata não impede que o fluxo seguinte recalcule o dinheiro. | sim | n/a | nenhuma | lote4.r:14 |
| LA-51 | e | escala/tests/test_f4_template_sem_fase.py:4; escala/models.py:851; escala/tests/test_f4_template_sem_fase.py:43 | A docstring promete `None` para 6x1 sem folga, mas a implementação e a expectativa atual retornam trabalho. | sim | n/a | nenhuma | lote4.r:19 |
| LA-52 | e | escala/tests/test_furos_diarios.py:62; escala/services/furos_diarios.py:54; ponto/portas/celula.py:872 | A promessa de nunca gerar `FuroDiario` para quem nunca bateu diverge da apuração de marcos faltantes sem exigir histórico de batidas. | sim | n/a | nenhuma | lote4.r:20 |
| LA-53 | a | escala/servico_jornada.py:129; escala/servico_jornada.py:131; escala/servico_jornada.py:144 | O helper atual não comprova os totais históricos de 37 templates e 761 dias citados na docstring. | sim | 09/2026 | nenhuma | lote4.r:30 |
| LA-54 | a | chamados/services/validacao.py:720; chamados/services/validacao.py:730; chamados/services/validacao.py:873 | O comentário anuncia quatro classes, mas a fila separa cinco grupos. | nao | n/a | nenhuma | lote5.r:A2 |
| LA-55 | a | chamados/juizes.py:1151 (movida); chamados/juizes.py:1162 (movida) | A resposta pode deixar de manter a pergunta viva sem decisão administrativa quando concorda com a luz e o dia não acusa. | sim | n/a | PERGUNTA-RESPONDIDA-E-MARCO-ACESO — B:1634 | lote5.r:A3 |
| LA-56 | a | chamados/catalogo/motor.py:84; chamados/services/arquivo.py:118 | O prazo de silêncio de trinta dias não comprova a afirmação de que a folha já fechou. | sim | n/a | nenhuma | lote5.r:A8 |
| LA-57 | b | docs/LEIS.md:189; chamados/services/fabrica_da_celula.py:126; chamados/services/fabrica_da_celula.py:185 | O diagnóstico de recusa de emissão em exportada ficou antigo, e o desvio para pauta está restrito ao limbo. | sim | n/a | O44 — B:1286 | lote5.r:B2 |
| LA-58 | b | docs/LEIS.md:189; chamados/services/cobrar_dia.py:39; chamados/juizes.py:954 (movida) | O diagnóstico de que a tranca recusa cobrança ficou antigo, pois o toque humano pode retirar o silêncio daquele dia. | sim | n/a | O44 — B:1286 | lote5.r:B3 |
| LA-59 | b | docs/LEIS.md:92; chamados/services/competencia_trancada.py:125; chamados/services/competencia_trancada.py:181 | A coluna de implementação ainda atribui a `encerrar` escrita da via nas perguntas, embora o fluxo atual só as conte por leitura. | nao | n/a | O44 — B:1286 | lote5.r:B4 |
| LA-60 | b | docs/LEIS.md:92; chamados/juizes.py:1145 (movida) | O leitor da via histórica não comprova a quantidade de 1.994 perguntas nem sua conservação. | nao | n/a | O44 — B:1286 | lote5.r:B7 |
| LA-61 | c | chamados/services/desvio_escala.py:12; chamados/services/desvio_escala.py:74; chamados/services/desvio_escala.py:84 | O pareamento de dia completo usa posição sem aplicar o teto de 240 minutos anunciado pelo comentário. | sim | n/a | JUIZ-DO-PAR-GRUPO-1 — B:778 | lote5.r:C4 |
| LA-62 | d | chamados/models.py:2782; chamados/models.py:2628; chamados/services/materializacao.py:519 | `materializada_em` também pode ser preenchido por identidade ou ausência sem garantir batida criada e chamado fechado. | sim | n/a | nenhuma | lote5.r:D3 |
| LA-63 | d | chamados/models.py:2797; chamados/services/materializacao.py:729; chamados/services/materializacao.py:748 | Os clones anunciados como fechados junto podem permanecer vivos por resposta irmã sem consumo ou pela reconciliação. | sim | n/a | nenhuma | lote5.r:D4 |
| LA-64 | e | chamados/tests/test_sem_resposta_nao_planta_batida.py:14; chamados/services/materializacao.py:796; chamados/services/materializacao.py:808 | A docstring aceita carimbo de resposta como anuência, mas a materialização exige texto não vazio. | sim | n/a | nenhuma | lote5.r:E2 |
| LA-65 | e | pautas/tests/test_pauta_texto.py:80; pautas/semanal.py:180; pautas/services.py:185 | A linha prometida inteira pode ser truncada silenciosamente em 500 caracteres pela persistência. | nao | n/a | ESCREVER-CORTA-PAUTA-EM-SILENCIO — B:1592 | lote5.r:E3 |
| LA-66 | a | ponto/supra_juiz.py:330; ponto/supra_juiz.py:348; ponto/supra_juiz.py:372 | A justificativa remove `FURO_COM_REGUA_DEGENERADA` apesar da promessa de preservar acusações de cadastro. | sim | n/a | nenhuma | lote6.r:02 |
| LA-67 | b | docs/LEIS.md:167; ponto/services/cartorio.py:1105; ponto/services/cartorio.py:1423; ponto/supra_juiz.py:278 | O diagnóstico de proteção temporal limitada a hoje ficou antigo diante da consulta de encerramento do turno. | sim | n/a | nenhuma | lote6.r:06 |
| LA-68 | a | ponto/supra_juiz.py:264; ponto/supra_juiz.py:530 | O comentário restringe a parcial ao portão do furo, mas ela também impede a classificação de cobrança em dia coberto. | sim | n/a | nenhuma | lote6.r:08 |
| LA-69 | e | ponto/tests/test_carimbo_isencao_lampada.py:29; ponto/management/commands/processar_cartorio.py:27; ponto/management/commands/processar_cartorio.py:65 | A docstring diz que `--forcar` é somente sombra, embora possa ser combinado com escrita. | sim | n/a | nenhuma | lote6.r:09 |
| LA-70 | e | ponto/tests/test_carimbo_isencao_lampada.py:23; ponto/services/cartorio.py:1362 | O mecanismo de pulo não demonstra o alcance histórico ZERO de 1.202 lâmpadas anunciado no teste. | sim | n/a | nenhuma | lote6.r:10 |
| LA-71 | a | escala/services/leitor_celula.py:204; escala/utils.py:850; escala/utils.py:858 | A isenção modifica julgamento de batida real e alcança folga e célula sem marco, contrariando a docstring do leitor. | nao | n/a | nenhuma | lote6.r:11 |
| LA-72 | d | colaboradores/models.py:46; colaboradores/views.py:499; ponto/janelas.py:51 | O cadastro anuncia corte entre 1 e 28, mas view e motor elevam 1 para 2. | sim | n/a | FERI-11 — B:2099 | lote7.r:01 |
| LA-73 | a | api/views.py:1662; relatorios/cartao_pela_celula.py:340; relatorios/cartao_pela_celula.py:344 | O comentário descreve duas fontes de totais, mas o fluxo também fornece `lavratura`. | nao | n/a | nenhuma | lote7.r:04 |
| LA-74 | e | api/tests/test_api_bater_ponto.py:533; api/views.py:600; api/views.py:608; api/tests/test_api_bater_ponto.py:569 | A docstring espera segunda entrada distante como E e disputa, mas o fluxo ao vivo corrige o tipo e o teste espera E/S. | sim | n/a | nenhuma | lote7.r:09 |
| LA-75 | b | docs/LEIS.md:174; api/views_mensageria.py:723; api/views_mensageria.py:883 | O mês civil também é escolhido com zero cortes, contrariando a descrição de uso somente quando eles divergem. | nao | n/a | nenhuma | lote7.r:15 |
| LA-76 | b | docs/LEIS.md:99; core/tests/test_smoke_chromium.py:35 | O teste automatizado não comprova o smoke humano nas duas cascas antes do push exigido pelo texto. | nao | n/a | nenhuma | lote7.r:29 |
| LA-77 | a | config/crons.py:1614; config/crons.py:1669; config/crons.py:1701; config/crons.py:1802; bin/encadeado.sh:102 | Os comentários dizem que `log` não te

[truncado em 20000 de 23537 caracteres]

## 05/11 17:48 · sessao 9f8ec08303ab · - · - · origem=Ronald (tmux)

Conferi os trechos usados abaixo na cópia. `B:NN` significa `app/docs/BACKLOG.md:NN`; as origens `01`, `02`, `03` correspondem aos nomes integrais dos respectivos anexos. Todas as competências são `n/a`, exceto o instrumento R5, fixado em 10/2026 (`bin/r5_idempotencia_frota.py:26`).

São **63 itens consolidados** neste bloco: **20** do anexo 01, **28** do anexo 02 após fundir L-002/L-061, **16** do anexo 03, com **uma fusão entre 02 e 03** (regeneração fora da vigência). O anexo 02 traz 29 alegações em entradas antes dessa fusão interna. Estes números são contagem da tabela abaixo.

| id | classe | arquivo:linha | afirmação | dinheiro | competência | cobertura BACKLOG | origens |
|---|---|---|---|---|---|---|---|
| A01 | instrumentacao | `bin/gerar_estado.py:295`; `bin/gerar_estado.py:373`; `app/docs/LEIS.md:141` | O gerador consultado não demonstra a composição N/15 nem a sequência persistente de rodadas limpas com reinício após violação. | sim | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A02 | instrumentacao | `app/chamados/services/vigia_fio.py:333` | O censo legado começa por chamados vivos e pode omitir célula acusadora sem chamado. | sim | n/a | PORTAO-FATURAMENTO-A1, B:1493 | 01 |
| A03 | instrumentacao | `app/chamados/services/vigia_fio.py:584`; `app/folha/fora_da_folha.py:128`; `app/folha/export.py:764`; `app/folha/export.py:846` | O alcance herda o universo da folha e pode excluir célula sem fechamento ou colaborador sem código Domínio. | sim | n/a | PORTAO-FATURAMENTO-A1, B:1493 | 01 |
| A04 | instrumentacao | `app/chamados/management/commands/vigiar_fio_morto.py:93` (movida); `app/chamados/management/commands/vigiar_fio_morto.py:179` (movida) | O alcance positivo é publicado no carimbo, mas não participa de `gritou` no alarme do comando. | sim | n/a | PORTAO-FATURAMENTO-A1, B:1493 | 01 |
| A05 | teste | `app/chamados/tests/test_o261e_f5_alcance.py:72` (movida) | O selo substitui o Resolver real por mock e não demonstra a integração completa da tela. | sim | n/a | PORTAO-FATURAMENTO-A1, B:1493 | 01 |
| A06 | instrumentacao | `app/chamados/management/commands/fechar_becos_disputa.py:42`; `app/chamados/management/commands/fechar_becos_disputa.py:67` | Beco classificado como barrado não aciona o alarme reservado aos fecháveis. | nao | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A07 | instrumentacao | `app/chamados/services/fila.py:92`; `app/chamados/services/fila.py:122` | As faixas etárias da fila excluem chamados vivos silenciados e não medem a idade de todos os vivos. | nao | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A08 | instrumentacao | `app/folha/porta_export.py:239`; `app/folha/porta_export.py:451` | C1 aceita diferenças abaixo da tolerância e pula campos quando o fechamento está ausente. | sim | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A09 | instrumentacao | `app/folha/porta_export.py:687`; `app/folha/porta_export.py:696` | A divergência entre fechamento e soma dos dias pagos é publicada fora das falhas que decidem o selo verde. | sim | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A10 | instrumentacao | `app/folha/porta_export.py:546`; `app/folha/porta_export.py:562` | C2 examina apenas HE contra intrajornada e ainda admite a tolerância de um minuto. | sim | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A11 | instrumentacao | `app/ponto/tests/test_noturno_desconta_intervalo.py:50`; `app/ponto/tests/test_o280_bateria_da_lei.py:2440`; `app/docs/BACKLOG.md:1494` | Os testes de cenário noturno não demonstram um censo C3 que confronte o adicional gravado com a janela noturna. | sim | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A12 | teste | `app/ponto/tests/test_feriado_sumula146.py:64` | O selo com feriados injetados não certifica a alimentação pela praça do vínculo real. | sim | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A13 | instrumentacao | `app/folha/export.py:110`; `app/folha/export.py:129`; `app/ponto/services/fechamento.py:864` | A concordância dos agregados de falta não verifica o teto C5 e pode coexistir com falta superior ao previsto. | sim | n/a | C5-FALTA-MAIOR-QUE-PREVISTO, B:1681; FALTA-ACIMA-DO-PREVISTO-[nome], B:1496 | 01 |
| A14 | instrumentacao | `bin/r5_idempotencia_frota.py:26`; `bin/r5_idempotencia_frota.py:65`; `bin/r5_idempotencia_frota.py:72` | A fotografia do R5 omite rubricas e restringe o universo à competência e empresas fixadas. | sim | 10/2026 | PORTAO-FATURAMENTO-RESTO, B:1494; PLACAR-ESTRUTURAL, B:1135 | 01 |
| A15 | instrumentacao | `bin/r5_idempotencia_frota.py:108`; `bin/r5_idempotencia_frota.py:132` | Falhas absorvidas nas duas rodadas podem manter fotografias iguais e produzir veredito OK no R5. | sim | 10/2026 | PORTAO-FATURAMENTO-RESTO, B:1494; PLACAR-ESTRUTURAL, B:1135 | 01 |
| A16 | instrumentacao | `bin/placar_code.sh:186`; `bin/placar_code.sh:549`; `app/core/placar_estrutural.py:693`; `app/core/placar_estrutural.py:701` | Os verdes agregados têm escopo menor que L-120, pois o TOTAL omite o loop e a cor estrutural mede a cadência do termômetro. | sim | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A17 | documentacao | `app/docs/LEIS.md:141`; `app/chamados/services/vigia_fio.py:541`; `app/folha/porta_export.py:451` | A ficha documental “0 de 15” não descreve toda a instrumentação parcial existente. | nao | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A18 | documentacao | `app/docs/LEIS.md:141`; `app/docs/BACKLOG.md:1494` | A2–A5 e B1–B4 aparecem como grupos, sem cláusulas individuais e associação individual a contadores demonstradas. | nao | n/a | PORTAO-FATURAMENTO-RESTO, B:1494 | 01 |
| A19 | instrumentacao | `app/folha/porta_export.py:433` | Falha na consulta de lavratura sem furo apurado apenas registra aviso e não acrescenta estado de não medição ao resultado. | sim | n/a | nenhuma | 01 |
| A20 | instrumentacao | `app/folha/porta_export.py:652` | Falha ao consultar DiaPago reduz o universo ao gravado e deixa apenas aviso sem estado de falha no resultado correspondente. | sim | n/a | nenhuma | 01 |
| A21 | implementacao | `app/ponto/services/cartorio.py:1407`; `app/ponto/services/cartorio.py:1408`; `app/chamados/juizes.py:413` (movida) | `_protesta` conserva decisão própria por `trabalha` e pode protestar em dia declarado sem trabalho pelo juiz canônico. | sim | n/a | nenhuma | 02 — L-002 e L-061 |
| A22 | implementacao | `app/colaboradores/services/vinculo.py:615`; `app/colaboradores/services/vinculo.py:616`; `app/colaboradores/services/desligamento.py:73` | Desligamento sem request pode fechar vínculos sem a trilha por PK condicionada à presença de autor. | nao | n/a | nenhuma | 02 |
| A23 | teste | `bin/tests/test_juiz_novo_tem_corte.sh:28`; `bin/tests/test_juiz_novo_tem_corte.sh:57` | O extrator da trava de juiz novo ignora caminhos com dígitos e aspas duplas, enquanto o caso adversarial não atravessa essa extração. | nao | n/a | nenhuma | 02 |
| A24 | implementacao | `app/ponto/portas/celula.py:225`; `app/ponto/portas/celula.py:233` | A regeneração além do fim do vínculo grava `trabalha/dna` sem os campos de denúncia da regeneração. | sim | n/a | nenhuma | 02; 03 |
| A25 | implementacao | `app/escala/models.py:1280`; `app/escala/models.py:1289`; `app/escala/models.py:1351` | O mês sem foto pode retornar a paridade da âncora antes de herdar a foto de folga de outro mês. | sim | n/a | nenhuma | 02 |
| A26 | implementacao | `app/ponto/motor_calculo_v2.py:1254`; `app/ponto/motor_calculo_v2.py:1273`; `app/ponto/motor_calculo_v2.py:3503` (movida) | A indisponibilidade do previsto canônico permite fallback para jornada cadastrada e para 480 minutos no Plantão. | sim | n/a | nenhuma | 02 |
| A27 | implementacao | `app/escala/services/cadastro_realidade.py:199` | `_jornada_que_mente` escolhe o mês civil e o corte literal 21 em vez da janela corrente da empresa. | nao | n/a | LISTA-POR-COMPETENCIA, B:1490 | 02 |
| A28 | implementacao | `app/ponto/registro_batida.py:145`; `app/api/views_core.py:402`; `app/api/views_core.py:435`; `app/api/views_core.py:454` | Os portões de situação, GPS e colisão próxima recusam batidas apesar da formulação literal “nunca barrada”. | sim | n/a | nenhuma | 02 |
| A29 | implementacao | `app/ponto/juiz_batida.py:310`; `app/ponto/motor_calculo_v2.py:822`; `app/ponto/motor_calculo_v2.py:2773` | O motor indeniza o intervalo suprimido sem consultar a flag `intervalo_indenizavel`, pendência declarada no próprio código. | sim | n/a | nenhuma | 02 |
| A30 | implementacao | `app/ponto/juiz_batida.py:294`; `app/ponto/motor_calculo_v2.py:2773`; `app/ponto/motor_calculo_v2.py:2774` | O ramo desconta somente intervalo real e não realiza a cláusula literal de subtrair o intervalo cadastrado. | sim | n/a | nenhuma | 02 |
| A31 | implementacao | `app/core/regua_cct.py:449`; `app/ponto/motor_calculo_v2.py:758` | Com o switch CCT desligado, parâmetro indefinido pode fazer empresa não CLT receber a regra noturna derivada do ciclo. | sim | n/a | nenhuma | 02 |
| A32 | implementacao | `app/relatorios/pdf_espelho.py:461`; `app/relatorios/pdf_espelho.py:471`; `app/relatorios/cartao_pela_celula.py:203`; `app/relatorios/cartao_pela_celula.py:321` | O PDF pode preservar faltas calculadas pelo motor quando o período livre ou a falta de fonte impede a sobrescrita pelos totais da folha. | sim | n/a | nenhuma | 02 |
| A33 | implementacao | `app/escala/services/cadastro_realidade.py:555`; `app/escala/services/cadastro_realidade.py:635`; `app/ponto/services/esmeril_espelho.py:159` | A lista Cadastro x Realidade não demonstra ingestão integral dos dias acusados pelo motor e pode omitir ocorrência abaixo dos limiares de repetição. | nao | n/a | CXR-TELA-LE-O-MOTOR, B:775 | 02 |
| A34 | implementacao | `app/escala/services/cadastro_realidade.py:250`; `app/escala/services/cadastro_realidade.py:449`; `app/escala/services/propositor.py:382` | C1/VINCULO-FIM oferece texto e link genérico sem decisão do propositor canônico para essa classe. | nao | n/a | nenhuma | 02 |
| A35 | documentacao | `app/docs/LEIS.md:91`; `app/docs/LEIS.md:189` | A seção de dívidas da L-050 contradiz a reauditoria principal e os caminhos já sem a antiga fronteira de exportação. | nao | n/a | nenhuma | 02 |
| A36 | implementacao | `app/config/crons.py:1999`; `app/config/crons.py:2004`; `app/config/crons.py:2018` | Três escritores residuais de perguntas, cobrança com lastro e reconciliação continuam cadastrados como juízes por varredura. | nao | n/a | CHAMADO-EM-RAIA, B:1351; E4-INCLUI-CONTRATO-20, B:792 | 02 |
| A37 | implementacao | `app/chamados/services/cobrar_dia.py:26` | A cobrança manual recusa todo dia corrente pela data civil, inclusive turno já encerrado naquele dia. | nao | n/a | nenhuma | 02 |
| A38 | instrumentacao | `bin/regua.sh:165`; `bin/regua.sh:179`; `bin/isolamento.sh:34`; `bin/regua_calendario.sh:43` | Leitores da suíte ainda decidem por prosa do log e o calendário executa testes sem a trava comum. | nao | n/a | O240, B:1168 — cobre o veredito por prosa; ausência de trava sem obra específica | 02 |
| A39 | implementacao | `bin/db_teste.sh:55`; `bin/tests/test_segredo_fora_do_argv.sh:19`; `bin/tests/test_segredo_fora_do_argv.sh:24` | A criação do banco passa `POSTGRES_PASSWORD` no argv e escapa ao selo limitado à forma `DB_PASSWORD`. | nao | n/a | nenhuma | 02 |
| A40 | implementacao | `app/ponto/motor_calculo_v2.py:3313`; `app/ponto/motor_calculo_v2.py:3441`; `app/ponto/motor_calculo_v2.py:3466` | O segundo cálculo comercial de DSR recria trabalho e extras pela data civil da entrada em vez do dia da jornada já mapeado. | sim | n/a | nenhuma | 02 |
| A41 | implementacao | `app/ponto/motor_calculo_v2.py:559`; `app/ponto/motor_calculo_v2.py:573`; `app/ponto/motor_calculo_v2.py:632`; `app/ponto/turnos.py:1380` | Turno partido, ausência de célula e ata aberta ou desalinhada ainda deixam o tipo gravado decidir no fallback do pareador. | sim | n/a | nenhuma | 02 |
| A42 | implementacao | `app/ferias/models.py:144` (movida); `app/ferias/models.py:54` | O contador aquisitivo de férias inclui falta aprovada em folga e pode reduzir o direito por linha fora do universo de trabalho. | sim | n/a | nenhuma | 02 |
| A43 | implementacao | `app/inteligencia/management/commands/calcular_scores.py:57`; `app/inteligencia/management/commands/calcular_scores.py:93`; `app/relatorios/views.py:195` | Assiduidade e absenteísmo continuam recortando mês civil em vez da competência. | nao | n/a | TELA-112, B:1907 — absenteísmo; assiduidade sem obra específica | 02 |
| A44 | implementacao | `app/ponto/motor_calculo_v2.py:3635`; `app/ponto/motor_calculo_v2.py:3668` | A fábrica corrige pelo desenho apenas alguns templates rotulados como turno partido e ainda seleciona os demais motores pelo rótulo. | sim | n/a | nenhuma | 02 |
| A45 | implementacao | `app/ponto/services/dia_pago.py:333`; `app/ponto/services/dia_pago.py:357` | A relavra apaga e recria linhas pela chave `versao`, sem preservar revisões anteriores e DIFF por rubrica. | sim | n/a | nenhuma | 02 |
| A46 | implementacao | `app/escala/services/cadastro_tipo.py:348`; `app/escala/services/cadastro_tipo.py:350`; `app/escala/services/cadastro_tipo.py:357` | Salvar template regenera células e invalida fechamentos sem concluir o recálculo nesse processamento em lote. | sim | n/a | E2-D3-TRABALHA-FORA-DA-IMPRESSAO, B:955 — inclui `cadastro_tipo` com `recalculo=False` | 02 |
| A47 | instrumentacao | `app/core/placar_estrutural.py:634`; `app/core/placar_estrutural.py:664`; `app/core/placar_estrutural.py:701` | O predicado de cadência não recebe a identidade ou data do último pouso e pode continuar verde após pouso sem nova medição. | sim | n/a | nenhuma | 02 |
| A48 | teste | `CLAUDE.md:431`; `app/docs/LEIS.md:146`; `bin/relato_afirma_com_prova.py:36` | O checker encontrado exige prova do ato, mas não verifica o teto de linhas nem a novidade da seção ancorada em commit. | nao | n/a | RELATO-CURTO-SELO, B:1431 | 02 |
| A49 | teste | `app/core/tests/test_contract_escritores_estado.py:13` (movida); `app/core/tests/test_contract_escritores_estado.py:177`; `app/ponto/tests/test_chokepoint_celula_gate.py:29` | P13 varre nomes de campos em seis apps sem resolver entidade, enquanto o gate descarta escritas de entidade não resolvida. | sim | n/a | ESMERIL-MECANICO, B:1101 — contrato do censo de escritores | 03 |
| A50 | implementacao | `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`; `app/escala/tests/test_d3_guarda_da_linha_celula.py:72` | A edição humana prometida da célula não tem porta nem escritor de produção para `editada_em/editada_por`. | sim | n/a | E2-D3-EDICAO-HUMANA-SEM-PORTA, B:962 | 03 |
| A51 | teste | `app/escala/tests/test_d3_guarda_da_linha_celula.py:70`; `app/ponto/tests/test_d3_impressao_cobre_insumos.py:171` | `trabalha` é exceção explícita da impressão de insumos e alterar somente trabalho para folga pode não mudar o hash. | sim | n/a | E2-D3-TRABALHA-FORA-DA-IMPRESSAO, B:955 | 03 |
| A52 | implementacao | `app/ponto/services/triagem_batida.py:734` (movida); `app/api/views.py:723` (movida); `app/api/views_core.py:720` (movida) | Três cascas escrevem `OrdemSubstituicao.status='em_curso'` fora da porta declarada de cobertura. | sim | n/a | ESMERIL-MECANICO, B:1101 | 03 |
| A53 | documentacao | `app/chamados/services/validacao.py:131`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/models.py:2581`; `app/core/tests/test_contract_escritores_estado.py:133` | A descrição de `validacao.py` como escritor único de `validada_em/por` não abrange os outros escritores explicitamente autorizados. | nao | n/a | O261-E-F5C-FILA-DO-OPUS, B:862 — divergência dos escritores em JUIZES | 03 |
| A54 | concorrencia | `app/ponto/views.py:1875`; `app/ponto/services/justificativa.py:262`; `app/ponto/views.py:1801` | A decisão em lote fora da porta pode sobrescrever uma aprovação individual concorrente por ler pendência sem lock. | sim | n/a | ESMERIL-MECANICO, B:1101 — múltiplos escritores; corrida sem linha específica | 03 |
| A55 | implementacao | `app/ponto/services/ausencia.py:1267`; `app/ponto/models.py:870` | A rejeição de férias por update em lote evita o preenchimento de `aprovada_em` realizado pelo save do model. | sim | n/a | nenhuma | 03 |
| A56 | implementacao | `app/ponto/services/fechamento.py:1316`; `app/ponto/services/fechamento.py:1330`; `app/ponto/services/fechamento.py:1338` | Aprovar ou reabrir fechamento pode mudar o estado sem atualizar `atualizado_em`. | sim | n/a | CARIMBO-POR-ORIGEM, B:1560 | 03 |
| A57 | documentacao | `app/ponto/services/fechamento.py:1354`; `app/ponto/services/fechamento.py:1364` | A restauração aceita alterar status apesar da promessa textual de não tocá-lo. | sim | n/a | nenhuma | 03 |
| A58 | implementacao | `app/core/credenciais.py:165`; `app/colaboradores/models.py:420` | O save do colaborador pode forçar onboarding completo após a porta solicitar seu reset para falso. | nao | n/a | nenhuma | 03 |
| A59 | concorrencia | `app/ferias/services.py:499`; `app/ferias/services.py:503` | Confirmar e recusar férias concorrentemente pode deixar confirmação recusada com instante de confirmação preenchido. | nao | n/a | nenhuma | 03 |
| A60 | implementacao | `app/holerite/servicos.py:113`; `app/holerite/views.py:1821` | Sucesso de aviso pelo service limpa a falha anterior, enquanto o aviso em lote a preserva. | nao | n/a | nenhuma | 03 |
| A61 | implementacao | `app/chamados/management/commands/backfill_veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:227` | O backfill pula a pergunta com veredito igual mesmo quando a janela difere, ao contrário da lavra normal. | nao | n/a | nenhuma | 03 |
| A62 | documentacao | `app/chamados/services/disputa_emissao.py:1340`; `app/chamados/services/validacao.py:545`; `app/chamados/services/fio.py:892` | `recusa_motivo` recebe fatos de naturezas diferentes e não identifica exclusivamente a última recusa do materializador. | nao | n/a | nenhuma | 03 |
| A63 | implementacao | `app/chamados/services/regularizacao_ext.py:264`; `app/chamados/services/regularizacao_ext.py:266` | A recusa direta de regularização materializada muda o status sem limpar o carimbo de materialização. | sim | n/a | nenhuma | 03 |

**Fusões externas necessárias:** A27 com 05/C2; A19 com 08/lavratura sem furo só no log; A03 com 08/zeros restritos ao TXT por universo de classificação, preservando os dois mecanismos de exclusão. A16 não se funde necessariamente com 08/TOTAL indisponível: omissão de parcela e indisponibilidade convertida em zero são causas diferentes.

**Sem achado contados à parte:**

- **01: 0** unidades expressamente declaradas conformes/sem achado; os controles positivos de proteção são contexto, não veredictos autônomos.
- **02: 7** entradas: L-033, L-051, L-052, L-092, L-101, L-112, L-113; L-050 permanece somente como divergência documental.
- **03: 3** unidades de exclusão expressas: (i) campos de veredito/status/julgada/impressão da célula não acrescentam módulos escritores (`app/ponto/portas/celula.py:713`, `:777`, `:808`; `app/escala/models.py:1828`); (ii) resolução/reabertura não comprovam escritores independentes (`app/chamados/models.py:238`, `:242`, `:2136`); (iii) exportação/retrato não apresentam duas portas explícitas (`app/folha/models.py:158`, `:186`; `app/folha/management/commands/lavrar_retrato_folha.py:74`).

Todos os locais selecionados existem; não identifiquei trecho ausente no HEAD neste bloco. A existência de mecanismos complementares não foi usada para descartar os achados das origens.

## 05/11 17:47 · sessao 4f381c691de0 · - · - · origem=Ronald (tmux)

Conferi os trechos abaixo nesta cópia: não identifiquei mudança de linha nem trecho ausente nas referências selecionadas. As competências dos exemplos sintéticos não restringem o defeito; por isso ficam `n/a`, exceto o apply específico da 09/2026. Produção e execução dos contraexemplos permanecem [não verificado].

Origens: `04` = `04-relogio.md`; `05` = `05-borda-competencia.md`; `06` = `06-idempotencia.md`; `07` = `07-corrida-trava.md`.

| id | classe | arquivo:linha | afirmacao | dinheiro | competencia | cobertura no BACKLOG | origem |
|---|---|---|---|---|---|---|---|
| B01 | implementacao | app/ponto/turnos.py:1229; app/ponto/turnos.py:1237 | O pareador escolhe os marcos pela data civil da batida e pode usar o DNA do dia seguinte durante um turno noturno. | sim | n/a | DIA-DA-BATIDA-JUIZ-UNICO — app/docs/BACKLOG.md:1683 | 04 |
| B02 | implementacao | app/ponto/services/fechamento.py:245; app/ponto/services/fechamento.py:252 | A apuração por fatia filtra batidas por data UTC e pode excluir batidas pertencentes ao turno julgado. | sim | n/a | PERGUNTA-SEM-SELO, item 4 — app/docs/BACKLOG.md:728 | 04 |
| B03 | implementacao | app/escala/servico_jornada.py:203; app/escala/utils.py:963 | As duas funções de saída esperada ancoram na data civil da entrada e podem avançar um dia para entradas tardias após meia-noite. | sim | n/a | DIA-DA-BATIDA-JUIZ-UNICO — app/docs/BACKLOG.md:1683 | 04 |
| B04 | implementacao | app/api/views.py:745; app/api/views_core.py:745 | O emissor de desvio leve grava o dia civil da saída como dia do chamado apesar de o detector resolver outro dia do turno. | sim | n/a | DIA-DA-BATIDA-JUIZ-UNICO — app/docs/BACKLOG.md:1683 | 04 |
| B05 | implementacao | app/chamados/detector_anomalias.py:144; app/chamados/detector_anomalias.py:164 | O detector escolhe o vínculo pela data civil da batida e conserva esse vínculo depois de resolver o dia do turno. | sim | n/a | DIA-DA-BATIDA-JUIZ-UNICO — app/docs/BACKLOG.md:1683 | 04 |
| B06 | implementacao | app/ponto/services/espelho.py:84; app/ponto/services/espelho.py:85 | O resumo recortado pela competência pode excluir períodos após meia-noite pertencentes à jornada do último dia. | sim | n/a | DIA-DA-BATIDA-JUIZ-UNICO — app/docs/BACKLOG.md:1683 | 04 |
| B07 | implementacao | app/escala/detector_proposta.py:18; app/escala/detector_proposta.py:58 | O propositor agrupa jornadas noturnas por dias civis e pode produzir quantidade de dias, saída e padrão incorretos. | nao | n/a | DIA-DA-BATIDA-JUIZ-UNICO — app/docs/BACKLOG.md:1683 | 04 |
| B08 | implementacao | app/folha/export.py:767; app/ponto/services/fechamento.py:612; app/ponto/models.py:485 | A empresa atual recebe o fechamento inteiro e define sua janela histórica após uma transferência, com risco alegado de sobreposição entre exportações por empresa. | sim | n/a | nenhuma | 05 |
| B09 | implementacao | app/core/regua_cct.py:329; app/core/regua_cct.py:352; app/core/regua_cct.py:366 | A régua encontra o vínculo histórico mas escolhe regime e convenção pela empresa atual do colaborador. | sim | n/a | nenhuma | 05 |
| B10 | implementacao | app/ponto/janelas.py:85; app/ponto/services/flip_batida.py:22 | A trava de alteração de batida histórica pode consultar outra competência depois de uma transferência entre empresas com cortes diferentes. | sim | n/a | nenhuma | 05 |
| B11 | implementacao | app/ponto/services/fechamento.py:232; app/ponto/services/fechamento.py:236; app/ponto/services/fechamento.py:250 | A partição por vínculos sobrepostos pode entregar as mesmas batidas a dois motores sem células ou visitar no previsto dias atribuídos a outro vínculo. | sim | n/a | nenhuma | 05 |
| B12 | implementacao | app/colaboradores/views.py:1385; app/colaboradores/views.py:1392 | Transferir o posto altera o cadastro sem estabelecer a vigência do novo posto no vínculo usado pelos feriados. | sim | n/a | TROCA-DE-POSTO-SEM-VINCULO — app/docs/BACKLOG.md:748 | 05 |
| B13 | implementacao | app/chamados/juizes.py:563; app/chamados/juizes.py:582; app/chamados/veredito_lavrado.py:114 | A janela viva e a expiração de perguntas omitem empresa e aplicam implicitamente o corte 21. | sim | n/a | nenhuma | 05 |
| B14 | implementacao | app/escala/services/cadastro_realidade.py:199 | O diagnóstico C2 usa o mês civil e corte 21 literal, podendo ignorar dias da competência corrente da empresa. | nao | n/a | nenhuma | 05 |
| B15 | apresentacao | app/colaboradores/views.py:499; app/templates/colaboradores/empresa_form.html:54; app/ponto/janelas.py:51 | A tela oferece início de competência 1, mas o POST e o juiz o substituem silenciosamente por 2. | sim | n/a | nenhuma | 05 |
| B16 | idempotencia | app/ponto/management/commands/aplicar_09_he_intra.py:146 | O apply financeiro soma novamente o mesmo delta ao valor já alterado quando executado outra vez. | sim | 09/2026 | nenhuma | 06 |
| B17 | idempotencia | app/escala/regua_defesa.py:757; app/escala/regua_defesa.py:767 | O termômetro avança confiança e dias de faixa baixa por chamada, sem guarda diária, acrescentando trilha a cada mudança. | sim | n/a | TERMOMETRO-GRAVA-SEM-TRILHA — app/docs/BACKLOG.md:745 | 06 |
| B18 | idempotencia | app/chamados/services/disputa_emissao.py:484; app/chamados/services/disputa_emissao.py:501; app/ponto/management/commands/marcar_foto_ausente_retro.py:95 | O cron de foto ausente incrementa novamente a quantidade da disputa para a mesma batida. | nao | n/a | nenhuma | 06 |
| B19 | idempotencia | app/ferias/services.py:330; app/ferias/services.py:333; app/ferias/views.py:324 | O cancelamento de férias pelo DP devolve novamente os dias quando repetido para um agendamento já cancelado. | sim | n/a | nenhuma | 06 |
| B20 | idempotencia | app/ferias/services.py:510; app/ferias/services.py:512; app/ferias/views.py:913 | A venda de dias de férias soma novamente o mesmo pedido enquanto saldo e teto permitirem. | sim | n/a | nenhuma | 06 |
| B21 | idempotencia | app/core/views_usuarios.py:82; app/core/views_usuarios.py:347; app/core/views_usuarios.py:375; app/core/credenciais.py:84; app/core/credenciais.py:196; app/colaboradores/views.py:1583 | POSTs de alternância de superusuário, acesso, usuário ativo e setor ativo desfazem o primeiro efeito quando repetidos. | nao | n/a | nenhuma | 06 |
| B22 | idempotencia | app/colaboradores/views.py:1793; app/colaboradores/services/beneficios.py:11 | O lançamento individual repetido de benefício cria dois benefícios ativos iguais. | sim | n/a | nenhuma | 06 |
| B23 | idempotencia | app/colaboradores/views.py:1975; app/colaboradores/services/beneficios.py:22 | A solicitação repetida de benefício cria outra solicitação e outro chamado. | nao | n/a | nenhuma | 06 |
| B24 | idempotencia | app/colaboradores/views.py:2114; app/colaboradores/services/beneficios.py:38 | Adicionar novamente a mesma linha de transporte cria outra linha ativa igual. | sim | n/a | nenhuma | 06 |
| B25 | idempotencia | app/escala/services/agenda.py:21; app/colaboradores/views.py:2294 | Adicionar novamente o mesmo bloco de agenda cria outro horário igual no dia reutilizado. | sim | n/a | nenhuma | 06 |
| B26 | idempotencia | app/colaboradores/services/estrutura.py:32; app/core/views_config.py:323; app/comunicados/views.py:136 | Formulários repetidos de criação de praça, sindicato ou comunicado criam outra entidade. | nao | n/a | nenhuma | 06 |
| B27 | idempotencia | app/pautas/services.py:180; app/pautas/services.py:200 | Escrever ou responder novamente a mesma pauta cria outra pauta ou outra filha. | nao | n/a | nenhuma | 06 |
| B28 | idempotencia | app/escala/services/cadastro_tipo.py:174; app/escala/services/cadastro_tipo.py:369; app/escala/views_wizard.py:102 | Criar ou copiar novamente o mesmo template de escala produz outro registro e código. | sim | n/a | nenhuma | 06 |
| B29 | idempotencia | app/api/views.py:2185; app/api/views.py:2198 | Repetir a abertura de chamado pelo app cria outro chamado e outra nota de abertura. | nao | n/a | nenhuma | 06 |
| B30 | idempotencia | app/chamados/services/nota.py:95; app/chamados/services/nota.py:103; app/chamados/views.py:547; app/chamados/views.py:674; app/chamados/views.py:954; app/chamados/views.py:986; app/chamados/views.py:1104; app/api/views.py:990 | Respostas e mensagens livres repetidas chegam sem chave de evento e duplicam notas no fio. | nao | n/a | nenhuma | 06 |
| B31 | idempotencia | app/folha/models.py:178; app/folha/models.py:182; app/folha/views.py:289 | Gerar novamente um TXT inalterado cria outro registro de exportação e outra auditoria. | sim | n/a | O100 — app/docs/BACKLOG.md:1212 | 06 |
| B32 | idempotencia | app/ponto/views.py:2976; app/api/views_core.py:967; app/api/views.py:2429 | Repetir uma amostra geográfica cria outro ping e pode satisfazer o gatilho de dois pings fora. | nao | n/a | nenhuma | 06 |
| B33 | idempotencia | app/ponto/services/ausencia.py:840; app/ponto/services/ausencia.py:858; app/ponto/views.py:2152 | Anexar novamente o mesmo documento de ausência repete a escrita e a trilha. | nao | n/a | nenhuma | 06 |
| B34 | idempotencia | app/ponto/views.py:2798 | Editar ausência com os mesmos valores acrescenta outra auditoria sem mudança. | nao | n/a | nenhuma | 06 |
| B35 | idempotencia | app/core/views_config.py:251; app/core/views_config.py:340 | Editar sindicato sem mudança recria seus vínculos com praças e repete a trilha. | nao | n/a | nenhuma | 06 |
| B36 | idempotencia | app/core/views_config.py:68; app/core/views_usuarios.py:203; app/core/views_usuarios.py:334; app/core/views_usuarios.py:367; app/core/views_quadro.py:40 | Edições absolutas repetidas de feriado, usuário, vínculo de setor, nome de setor ou permissões acrescentam auditoria sem mudança. | nao | n/a | nenhuma | 06 |
| B37 | idempotencia | app/core/credenciais.py:134; app/core/credenciais.py:163; app/colaboradores/views.py:1466; app/colaboradores/views.py:1502 | Alterar perfil já atribuído ou resetar onboarding já limpo repete a trilha. | nao | n/a | nenhuma | 06 |
| B38 | idempotencia | app/core/credenciais.py:54; app/core/credenciais.py:185; app/colaboradores/management/commands/regerar_senhas_iniciais.py:70; app/core/views_usuarios.py:341 | Resets repetidos de senha refazem a operação e a auditoria, podendo sortear outra senha temporária. | nao | n/a | nenhuma | 06 |
| B39 | idempotencia | app/chamados/views_cobrar.py:381; app/chamados/views_cobrar.py:383 | Validar novamente o mesmo lote pela tela acrescenta auditoria mesmo quando nenhuma pergunta é novamente validada. | nao | n/a | nenhuma | 06 |
| B40 | idempotencia | app/chamados/views_regularizacao.py:160; app/chamados/services/regularizacao_ext.py:339 | Enviar novamente uma regularização aguardando repete as chamadas de comunicação. | nao | n/a | nenhuma | 06 |
| B41 | idempotencia | app/holerite/views.py:1174; app/holerite/views.py:1198 | O upload avulso repetido do mesmo holerite sem ciência substitui novamente o documento e cria outra PK. | nao | n/a | nenhuma | 06 |
| B42 | idempotencia | app/holerite/views.py:1764; app/holerite/views.py:1783 | O upload repetido do mesmo lote de holerites cria outro lote e job. | nao | n/a | nenhuma | 06 |
| B43 | idempotencia | app/relatorios/views.py:1247; app/relatorios/views.py:1254 | Pedir novamente o mesmo espelho em lote acrescenta trilha apesar de reutilizar o job vivo. | nao | n/a | nenhuma | 06 |
| B44 | idempotencia | app/api/views.py:3039; app/api/views.py:3048 | Cada POST repetido de token WebView cria outra credencial para o mesmo usuário e destino. | nao | n/a | nenhuma | 06 |
| B45 | idempotencia | app/ponto/management/commands/alertar_disputas_pendentes.py:72; app/ponto/management/commands/alertar_disputas_pendentes.py:75 | O cron de disputas pendentes repete a trilha `disputa_velha` apesar da guarda diária do push individual. | nao | n/a | nenhuma | 06 |
| B46 | idempotencia | app/ferias/management/commands/alertar_retornos.py:45; app/ferias/management/commands/alertar_ferias_vencendo.py:85; app/ponto/management/commands/alertar_justificativas_sla.py:18; app/ponto/management/commands/tripwire_tipo_batida.py:66 | Os quatro crons de retornos, férias vencendo, justificativas fora do SLA e tipo de batida repetem chamadas de push para os mesmos fatos. | nao | n/a | nenhuma | 06 |
| B47 | idempotencia | app/chamados/services/sla.py:14; app/chamados/management/commands/alertar_chamados_sla.py:30 | O teto de vinte avisos por passada impede o cron de SLA de alcançar o estado final em uma execução quando há mais elegíveis. | nao | n/a | nenhuma | 06 |
| B48 | idempotencia | app/ponto/management/commands/aplicar_janela_he_total.py:119; app/colaboradores/management/commands/marcar_feriado_do_posto.py:128; app/colaboradores/management/commands/corrigir_escala_retroativa.py:206; app/chamados/management/commands/backfill_veredito_lavrado.py:157 | Os quatro commands de apply citados repetem auditoria mesmo quando os valores já estavam aplicados. | nao | n/a | nenhuma | 06 |
| B49 | idempotencia | app/holerite/management/commands/lembrar_holerites.py:69; app/holerite/management/commands/lembrar_holerites.py:94; app/holerite/management/commands/lembrar_holerites.py:108 | O lembrete de holerites repete trilha e, com `--dias 0`, pode repetir envio e incremento do contador. | nao | n/a | nenhuma | 06 |
| B50 | idempotencia | app/core/views.py:1077; app/core/views.py:1104; app/core/views.py:1126 | POSTs repetidos de onboarding renovam datas de concessão, aceite e confirmação com o mesmo payload. | nao | n/a | nenhuma | 06 |
| B51 | idempotencia | app/ponto/services/fechamento.py:1127; app/ponto/services/fechamento.py:1173 | Recalcular o fechamento com os mesmos insumos renova os carimbos de derivação e atualização. | nao | n/a | PORTAO-FATURAMENTO-RESTO, C6 — app/docs/BACKLOG.md:1494 | 06 |
| B52 | idempotencia | app/core/management/commands/lavrar_badge_navbar.py:91; app/core/management/commands/lavrar_placar_situacional.py:105; app/pautas/services.py:354; app/folha/management/commands/lavrar_retrato_folha.py:74; app/inteligencia/models.py:21 | Lavras repetidas de navbar, placar, pautas, retrato e scores renovam carimbos com conteúdo igual. | nao | n/a | nenhuma | 06 |
| B53 | idempotencia | app/inteligencia/metricas.py:47; app/inteligencia/models.py:96 | Snapshots repetidos de métricas renovam `calculado_em` apesar de manter entidade, escopo, data e valores. | nao | n/a | nenhuma | 06 |
| B54 | idempotencia | bin/cron_run.sh:13; bin/cron_run.sh:25 | Se o contrato de trilha incluir log operacional, duas execuções envelopadas acrescentam dois registros e renovam o arquivo de término. | nao | n/a | nenhuma | 06 |
| B55 | concorrencia | app/ponto/registro_batida.py:113; app/ponto/registro_batida.py:157; app/ponto/models.py:106 | O dedup de batida sem trava permite inserções próximas concorrentes e não devolve a vencedora da corrida por timestamp idêntico. | sim | n/a | nenhuma | 07 |
| B56 | concorrencia | app/ponto/nucleo.py:45; app/ponto/nucleo.py:60; app/ponto/models.py:1213 | Reconstruções concorrentes do núcleo calculadas antes da transação podem duplicar turnos ou repor geometria antiga. | sim | n/a | nenhuma | 07 |
| B57 | concorrencia | app/ponto/services/cartorio.py:1123; app/ponto/services/cartorio.py:1822; app/ponto/portas/celula.py:745; app/ponto/portas/celula.py:783 | O cartório pode salvar insumos antigos depois de um julgamento novo ou de uma decisão administrativa. | sim | n/a | nenhuma | 07 |
| B58 | concorrencia | app/chamados/services/disputa_emissao.py:467; app/chamados/services/disputa_emissao.py:523 | Duas primeiras aberturas concorrentes podem criar duas disputas porque a trava sobre disputa existente não protege o resultado vazio. | nao | n/a | nenhuma | 07 |
| B59 | concorrencia | app/chamados/models.py:2420; app/chamados/models.py:2465; app/chamados/models.py:2800 | Perguntas do mesmo fato podem nascer duplicadas porque consulta e criação não possuem trava nem unicidade. | nao | n/a | nenhuma | 07 |
| B60 | concorrencia | app/chamados/services/disputa_emissao.py:1286; app/chamados/services/disputa_emissao.py:1301; app/chamados/services/validacao.py:396 | Uma correção de resposta baseada em instância antiga pode conservar validação e materialização da resposta anterior. | sim | n/a | nenhuma | 07 |
| B61 | concorrencia | app/chamados/models.py:763; app/chamados/models.py:769; app/chamados/models.py:279 | `analisar()` pode ressuscitar um chamado cancelado ao gravar uma instância antiga ainda aberta. | nao | n/a | ANALISAR-RELE-O-BANCO — app/docs/BACKLOG.md:979 | 07 |
| B62 | concorrencia | app/chamados/services/contexto.py:70; app/chamados/services/contexto.py:77; app/chamados/services/contexto.py:96; app/chamados/services/contexto.py:102 | Atualizações concorrentes de chaves distintas do contexto JSON podem se apagar por salvarem o campo inteiro a partir de instâncias antigas. | nao | n/a | nenhuma | 07 |
| B63 | concorrencia | app/ponto/services/fechamento.py:1128; app/ponto/services/fechamento.py:1316; app/ponto/models.py:498 | Um recálculo carregado com fechamento aberto pode salvar depois da aprovação e desfazê-la. | sim | n/a | nenhuma | 07 |
| B64 | concorrencia | app/ponto/services/fechamento.py:651; app/ponto/services/fechamento.py:1127; app/ponto/services/fechamento.py:1640; app/ponto/services/fechamento.py:1680 | Um recálculo antigo pode sobrescrever atualização ou invalidação posterior e receber um carimbo novo que o faz parecer atual. | sim | n/a | nenhuma | 07 |
| B65 | concorrencia | app/ponto/services/flip_batida.py:25; app/ponto/registro_batida.py:254; app/ponto/registro_batida.py:274 | Anotações concorrentes de batida podem perder inversões e repetir retratações com substituição do primeiro carimbo. | sim | n/a | nenhuma | 07 |

Atenções para a consolidação do pai:

- **B14 funde com 02/L-026**, mesmo `_jornada_que_mente` e mesma chamada `periodo_apuracao(..., 21)` em `app/escala/services/cadastro_realidade.py:199`.
- **B51 não deve fundir com 01/C6**: B51 afirma renovação literal do estado, enquanto 01/C6 denuncia a cobertura incompleta da fotografia e do universo do instrumento.
- **B60 não deve fundir com 03/multiplicidade de `validada_em`**: aqui é conservação indevida de validação em uma corrida; lá é o contrato de escritor único literal.
- **B54 está fora da tabela final da origem**, mas é uma alegação explícita, condicional, de descumprimento do contrato de trilha.
- A cobertura **DIA-DA-BATIDA-JUIZ-UNICO** é a obra geral que manda censar e migrar os julgamentos por datas civis; o BACKLOG não enumera individualmente todos esses sete sítios (`app/docs/BACKLOG.md:1683`).
- Não atribuí **GRUPO-1-DATA-SAIDA-ESPERADA** ao B03: sua linha denuncia seleção do vínculo atual, enquanto B03 denuncia ancoragem no dia civil da entrada (`app/docs/BACKLOG.md:1027`).
- Não atribuí **V-FASE-AS-OF-TRANSFERIDO** aos B08–B10: essa linha nomeia outro juiz e outro efeito (`app/docs/BACKLOG.md:865`).
- Não atribuí os `PORT-*` de smoke genérico aos defeitos de dedup de suas rotas, pois essas linhas não nomeiam a lacuna de idempotência (`app/docs/BACKLOG.md:1971`; `app/docs/BACKLOG.md:2015`; `app/docs/BACKLOG.md:2017`).

Contagens deste recorte: **65 itens; implementacao 14, apresentacao 1, idempotencia 39, concorrencia 11; dinheiro=sim 29; sem cobertura 53; nao verificavel 0**. Recontagem humana dos 29 e 53 é aconselhável antes de fechar a consolidação [não verificado].

| anexo | achados distintos trazidos | unidades sem achado contadas à parte |
|---|---:|---:|
| 04-relogio.md | 7 | 1 |
| 05-borda-competencia.md | 8 | 4 |
| 06-idempotencia.md | 39 | 4 |
| 07-corrida-trava.md | 11 | 4 |

Unidades sem achado usadas para essa contagem:

- **04: 1** — `date.today()` isolado não foi classificado como defeito de fuso, porque o fuso do processo é [não verificado].
- **05: 4** — limites inclusivos centrais 20/21; admissão/demissão sem erro demonstrado com cadastro coerente; vigência inclusiva e troca normal D−1/desligamento; busca ampliada noturna sem demonstração de dupla contagem (`app/ponto/janelas.p

[truncado em 20000 de 20944 caracteres]

## 05/11 17:46 · sessao 27c504fff50b · - · - · origem=Ronald (tmux)

Conferência somente leitura concluída para lote3.r–lote5.r; todas as referências abaixo existem, e não encontrei trecho ausente dentre os achados selecionados. Alterações de linha estão marcadas. A ocorrência em produção permanece [nao verificado].

Contagem estrita por veredito, contando cada item numerado uma vez, sem repetir a tabela-resumo:

| anexo | achados antes de fusão | afirmações sem achado |
|---|---:|---:|
| lote3.r | 15 | 15 |
| lote4.r | 11 | 19 |
| lote5.r | 14 | 19 |
| total | 40 | 53 |

“Classe” preserva os tipos a–e da origem. “Dinheiro” abaixo usa efeito potencial em horas, rubrica, TXT ou fechamento; campos de apresentação/documentação sem tal efeito demonstrado receberam não. Todos têm arquivo:linha; nenhum é “nao verificavel”.

| origem | classe | referências conferidas | afirmação consolidável | dinheiro | competência | cobertura específica conferida |
|---|---|---|---|---|---|---|
| lote3.r:1 | b | app/docs/LEIS.md:121; app/escala/utils.py:1231; app/escala/utils.py:1241; app/escala/utils.py:1272; app/ferias/models.py:144; app/ferias/services.py:246 | A promessa de ausência sem efeito nem consumo em folga diverge do overlay e da contagem de faltas do período aquisitivo sem filtro de jornada. | sim | n/a | nenhuma |
| lote3.r:2 | b | app/docs/LEIS.md:87; app/relatorios/pdf_espelho.py:461; app/relatorios/cartao_pela_celula.py:322; app/relatorios/pdf_espelho.py:787 | O PDF pode apresentar como “Faltas” dias sem decisão quando mantém o total produzido pelo motor. | nao | n/a | nenhuma |
| lote3.r:6 | a | app/ponto/precedencia.py:519; app/ponto/precedencia.py:520; app/ponto/precedencia.py:536 | A cópia não comprova o censo histórico de dez faltas aprovadas fora da jornada. | sim | 09/2026 e 10/2026 | O198 — app/docs/BACKLOG.md:1409 |
| lote3.r:7 | e | app/ferias/test_services_leitura.py:2; app/ferias/services.py:118; app/ferias/models.py:257 | Serviço e property de vencimento usam status gravado e derivado diferentes, contrariando a concordância “SEMPRE”. | nao | n/a | nenhuma |
| lote3.r:8 | d | app/ferias/models.py:122; app/ferias/services.py:472; app/ferias/services.py:473 | O limite anunciado como doze meses após o fim é calculado somando 365 dias. | nao | n/a | nenhuma |
| lote3.r:9 | d | app/ferias/models.py:307; app/ferias/services.py:314; app/ferias/services.py:323 | `aviso_emitido` é calculado pela antecedência do agendamento, sem informação de emissão participar da decisão. | nao | n/a | nenhuma |
| lote3.r:12 | d | app/ponto/models.py:584; app/ponto/services/ausencia.py:333; app/ponto/services/ausencia.py:418; app/ponto/services/ausencia.py:779 | A exigência documental anunciada como recusa permite criar ausência aguardando documento. | sim | n/a | nenhuma |
| lote3.r:13 | d | app/ponto/models.py:586; app/api/views.py:415; app/api/views.py:683; app/ponto/services/aviso_da_batida.py:79; app/ponto/services/aviso_da_batida.py:88 | `bloqueia_ponto` anuncia impedimento, mas o fluxo pode registrar a batida e depois avisar sobre a ausência. | sim | n/a | nenhuma |
| lote3.r:14 | d | app/ponto/models.py:588; app/ponto/catalogo/ausencias.py:358; app/ponto/turnos.py:2598; app/ponto/turnos.py:2600 | A flag parcial permite horas, enquanto a classificação de cobertura parcial depende também de um dia e minutos estritamente entre zero e 480. | sim | n/a | nenhuma |
| lote3.r:15 | d | app/ponto/models.py:757; app/ponto/models.py:875; app/ponto/models.py:883; app/ponto/models.py:885 | O help de falta inteira fixa 720, embora o preenchimento automático derive o previsto e preserve minutos positivos informados. | sim | n/a | nenhuma |
| lote3.r:16 | a | app/api/views.py:815; app/api/views.py:817; app/api/views.py:415; app/api/views.py:418; app/api/views.py:423 | O comentário promete apenas aviso em férias, mas o endpoint recusa essa situação antes do registro. | sim | n/a | nenhuma |
| lote3.r:19 | e | app/ponto/tests/test_atestados_lote.py:11; app/ponto/services/atestados_lote.py:60; app/ponto/services/atestados_lote.py:62; app/ponto/services/atestados_lote.py:77 | A promessa de que documento faltante nunca some por filtro diverge do filtro anterior por tipo e status. | nao | n/a | nenhuma |
| lote3.r:25 | a | app/chamados/services/materializacao.py:61; app/chamados/services/materializacao.py:62; app/chamados/services/materializacao.py:78; app/ponto/turnos.py:2555 | O comentário conserva o critério antigo de range aberto por bloqueio de ponto, enquanto a consulta usa tipos indeterminados pela autoridade atual. | sim | n/a | nenhuma |
| lote3.r:26 | a | app/ponto/precedencia.py:13; app/ponto/precedencia.py:14; app/ponto/precedencia.py:376; app/escala/models.py:1392 | A exceção textual de feriado exige `trabalha_em_feriado=True`, mas a implementação também considera o ciclo. | sim | n/a | nenhuma |
| lote3.r:27 | a | app/chamados/juizes.py:414 (movida de 412); app/chamados/juizes.py:426 (movida de 424); app/chamados/juizes.py:427 | A descrição de dia sem trabalho omite a exceção da cobertura parcial que preserva cobrança do restante. | sim | n/a | nenhuma |
| lote4.r:3 | d | app/escala/models.py:174; app/ponto/motor_calculo_v2.py:3635; app/ponto/motor_calculo_v2.py:3663; app/ponto/motor_calculo_v2.py:3666 | O help atribui a escolha do motor à base, mas a fábrica também usa a geometria em determinados ramos. | sim | n/a | O136 — app/docs/BACKLOG.md:1346, se fundido ao problema do dispatch parcial |
| lote4.r:4 | d | app/escala/models.py:191; app/core/regua_cct.py:458; app/ponto/motor_calculo_v2.py:3671; app/ponto/views.py:2903 | O campo de perfil promete regras e padrão, embora o caminho atual resolva parâmetros pela régua sem consultar essa FK. | sim | n/a | nenhuma |
| lote4.r:5 | d | app/escala/models.py:1699; app/ponto/management/commands/gerar_celulas.py:16; app/ponto/management/commands/gerar_celulas.py:96 | O help anuncia DNA versão 1, enquanto o produtor grava a versão 2. | nao | n/a | nenhuma |
| lote4.r:10 | b | app/docs/LEIS.md:71; app/ponto/portas/celula.py:225; app/ponto/portas/celula.py:231; app/ponto/portas/celula.py:233; app/ponto/portas/celula.py:259 | A correção de célula fora da vigência altera DNA sem atualizar os campos de denúncia usados na regeneração regular. | sim | n/a | nenhuma |
| lote4.r:11 | b | app/docs/LEIS.md:93; app/ponto/portas/celula.py:196 | A proibição absoluta de tocar exportada foi parcialmente superada pelo caminho com `apesar_da_lavra`. | sim | n/a | O121 — app/docs/BACKLOG.md:1189 |
| lote4.r:12 | b | app/docs/LEIS.md:149; app/ponto/portas/celula.py:199; app/ponto/portas/celula.py:238 | A regeneração ordinária continua bloqueando competências exportadas, apesar da revogação da trava pela L-113. | sim | n/a | O121-A1-VINCULO-CORRIGE-O-PASSADO — app/docs/BACKLOG.md:1008; O121 — :1189 |
| lote4.r:13 | b | app/docs/LEIS.md:124; app/ponto/motor_calculo_v2.py:3633; app/ponto/motor_calculo_v2.py:3668 | A fábrica ainda seleciona motores pelo rótulo da base, contrariando a exclusão geral desse rótulo pela L-091. | sim | n/a | O136 — app/docs/BACKLOG.md:1346 |
| lote4.r:14 | a | app/ponto/portas/celula.py:349; app/ponto/portas/celula.py:372; app/ponto/portas/celula.py:375; app/ponto/portas/celula.py:397 | A falha capturada no rejulgamento da ata não impede que o fluxo seguinte recalcule o dinheiro. | sim | n/a | nenhuma |
| lote4.r:19 | e | app/escala/tests/test_f4_template_sem_fase.py:4; app/escala/models.py:835; app/escala/models.py:851; app/escala/tests/test_f4_template_sem_fase.py:43 | A docstring promete `None` para 6x1 sem folga, mas a implementação e a expectativa do teste retornam trabalho. | sim | n/a | nenhuma |
| lote4.r:20 | e | app/escala/tests/test_furos_diarios.py:62; app/escala/services/furos_diarios.py:49; app/escala/services/furos_diarios.py:54; app/escala/services/furos_diarios.py:108; app/ponto/portas/celula.py:872 | A promessa de nunca gerar `FuroDiario` para quem nunca bateu diverge da apuração de marcos faltantes da grade sem exigir histórico. | sim | n/a | nenhuma |
| lote4.r:30 | a | app/escala/servico_jornada.py:129; app/escala/servico_jornada.py:131; app/escala/servico_jornada.py:144 | O helper atual não comprova os totais históricos de 37 templates e 761 dias citados na docstring. | sim | 09/2026 | nenhuma |
| lote5.r:A2 | a | app/chamados/services/validacao.py:720; app/chamados/services/validacao.py:730; app/chamados/services/validacao.py:821; app/chamados/services/validacao.py:873 | O comentário anuncia quatro classes, mas a fila separa cinco grupos. | nao | n/a | nenhuma |
| lote5.r:A3 | a | app/chamados/juizes.py:1151 (movida de 1093); app/chamados/juizes.py:1158; app/chamados/juizes.py:1162 (movida de 1104) | A resposta pode deixar de manter a pergunta viva sem decisão administrativa quando concorda com a luz e o dia não acusa. | sim | n/a | PERGUNTA-RESPONDIDA-E-MARCO-ACESO — app/docs/BACKLOG.md:1634 |
| lote5.r:A8 | a | app/chamados/catalogo/motor.py:84; app/chamados/services/arquivo.py:118 | O prazo de silêncio de trinta dias não comprova a afirmação de que a folha já fechou. | sim | n/a | nenhuma |
| lote5.r:B1 | b | app/docs/LEIS.md:125; app/ponto/services/fechamento.py:37; app/ponto/services/fechamento.py:551; app/ponto/services/fechamento.py:1144 | O caminho de escrita pode atualizar fechamento exportado fora da guarda específica de setembro, contrariando a preservação geral do gravado. | sim | 10/2026, com exceção 09/2026 | QUEM-ESCREVE-NO-GRAVADO-DA-EXPORTADA — app/docs/BACKLOG.md:1091; descrição histórica da mesma escrita em lote |
| lote5.r:B2 | b | app/docs/LEIS.md:189; app/chamados/services/fabrica_da_celula.py:91; app/chamados/services/fabrica_da_celula.py:126; app/chamados/services/fabrica_da_celula.py:185 | O diagnóstico documental de recusa de emissão em exportada ficou antigo, e o desvio para pauta está restrito ao limbo. | sim | n/a | O44 — app/docs/BACKLOG.md:1286; QUARENTENA-DO-COLAB — :761 |
| lote5.r:B3 | b | app/docs/LEIS.md:189; app/chamados/services/cobrar_dia.py:37; app/chamados/services/cobrar_dia.py:39; app/chamados/juizes.py:954 (movida de 949) | O diagnóstico de que a tranca recusa cobrança ficou antigo, pois o toque humano pode retirar o silêncio daquele dia. | sim | n/a | O44 — app/docs/BACKLOG.md:1286 |
| lote5.r:B4 | b | app/docs/LEIS.md:92; app/chamados/services/competencia_trancada.py:77; app/chamados/services/competencia_trancada.py:125; app/chamados/services/competencia_trancada.py:181; app/chamados/services/competencia_trancada.py:196 | A coluna de implementação ainda atribui a `encerrar` escrita da via nas perguntas, embora o fluxo atual só as conte por leitura. | nao | n/a | O44 — app/docs/BACKLOG.md:1286 |
| lote5.r:B7 | b | app/docs/LEIS.md:92; app/chamados/juizes.py:1140 (movida da região 1082–1087) | O código leitor da via histórica não comprova a quantidade de 1.994 perguntas nem sua conservação. | nao | n/a | O44 — app/docs/BACKLOG.md:1286; O40 — :1282 |
| lote5.r:C4 | c | app/chamados/services/desvio_escala.py:12; app/chamados/services/desvio_escala.py:74; app/chamados/services/desvio_escala.py:77; app/chamados/services/desvio_escala.py:84 | O pareamento de dia completo usa posição sem aplicar o teto de 240 minutos anunciado pelo comentário. | sim | n/a | JUIZ-DO-PAR-GRUPO-1 — app/docs/BACKLOG.md:778 |
| lote5.r:D3 | d | app/chamados/models.py:2782; app/chamados/models.py:2547; app/chamados/models.py:2628; app/chamados/models.py:2633; app/chamados/services/fio.py:891 | `materializada_em` também pode ser preenchido por identidade ou ausência, sem garantir batida criada e chamado fechado. | sim | n/a | nenhuma específica; O261-E-CELULA-20-UM-DONO :859 trata a porta do campo, mas não declara este help |
| lote5.r:D4 | d | app/chamados/models.py:2797; app/chamados/services/materializacao.py:729; app/chamados/services/materializacao.py:737; app/chamados/services/materializacao.py:748 | Os clones anunciados como fechados junto podem permanecer vivos por resposta irmã sem consumo ou pela reconciliação. | sim | n/a | nenhuma |
| lote5.r:E1 | e | app/chamados/tests/test_invariantes_disputa.py:193; app/api/views.py:449; app/api/views.py:466; app/chamados/tests/test_invariantes_disputa.py:288 | O teste anuncia batida nunca bloqueada para ativo com tipo válido, mas a API recusa ausência de GPS. | sim | n/a | nenhuma |
| lote5.r:E2 | e | app/chamados/tests/test_sem_resposta_nao_planta_batida.py:14; app/chamados/services/materializacao.py:796; app/chamados/services/materializacao.py:808 | A docstring aceita carimbo de resposta como anuência, mas a materialização exige texto não vazio. | sim | n/a | nenhuma específica; RESPONDIDA-SEM-TEXTO-DUAS-COPIAS :953 trata duplicação do predicado, não esta divergência |
| lote5.r:E3 | e | app/pautas/tests/test_pauta_texto.py:80; app/pautas/semanal.py:168; app/pautas/semanal.py:180; app/pautas/services.py:185 | A linha prometida inteira pode ser truncada silenciosamente em 500 caracteres pela persistência. | nao | n/a | ESCREVER-CORTA-PAUTA-EM-SILENCIO — app/docs/BACKLOG.md:1592 |

Pontos para a fusão geral:

- Lote3:15 duplica o defeito “falta inteira universalmente 720” de lote1:33, lote2:18 e lote7:5.
- Lote3:12/13 duplicam lote7:6/7, respectivamente.
- Lote4:5 duplica lote6:7; lote4:10 duplica lote6:3; lote4:11 duplica lote1:3/lote6:4; lote4:12 duplica lote1:7/lote6:5; lote4:13 duplica lote2:33.
- Lote4:3 e lote4:13 podem ser fundidos pelo mesmo dispatch parcialmente geométrico; preservam afirmações documentais opostas sobre o mesmo sítio.
- Lote3:16, lote5:E1 e lote7:8 compartilham a promessa absoluta de batida nunca recusada, mas seus sítios de recusa são férias e GPS; a origem lote7:8 reúne ambos.
- Lote5:B1 e lote1:19 têm a mesma escrita `FechamentoMensal.update()` fora da guarda de setembro; a primeira fala de exportação e a segunda de tranca, exigindo decisão de fusão por mesma causa.

## 05/11 17:46 · sessao f35d43343e10 · - · - · origem=Ronald (tmux)

Referências das 46 linhas abaixo conferidas no HEAD: nenhuma movida/ausente; nenhuma sem referência. Os achados mantêm as alegações das origens, sem veredito de procedência. Origem abreviada: 08=`08-falso-zero.md`, 09=`09-selo-nao-morde.md`, 10=`10-textos.md`.

| id | classe | arquivo:linha | afirmacao | dinheiro | competencia | cobertura BACKLOG | origem |
|---|---|---|---|---|---|---|---|
| C01 | instrumentacao | app/core/censo_pergunta2.py:81 | O censo não detecta filtros de vigência equivalentes escritos por `**dict` ou repartidos em instruções. | nao | n/a | nenhuma | 08 |
| C02 | instrumentacao | app/core/censo_pergunta2.py:67 | Um erro de sintaxe faz o censo devolver conjunto vazio para o arquivo inteiro. | nao | n/a | nenhuma | 08 |
| C03 | instrumentacao | app/core/censo_pergunta2.py:115 | A exclusão de funções inteiras pode esconder uma nova consulta de vigência dentro de função já dispensada. | nao | n/a | nenhuma | 08 |
| C04 | instrumentacao | app/relatorios/management/commands/pdf_x_espelho_divergentes.py:59 | Falha na classificação de uma empresa a exclui silenciosamente do comparador PDF × espelho. | sim | n/a | nenhuma | 08 |
| C05 | instrumentacao | app/relatorios/management/commands/pdf_x_espelho_divergentes.py:104 | Um dia presente no espelho e ausente no PDF é ignorado em vez de contado como divergência. | sim | n/a | nenhuma | 08 |
| C06 | instrumentacao | app/relatorios/management/commands/diff_pdf_x_espelho.py:82; app/relatorios/management/commands/diff_pdf_x_espelho.py:138 | O DIFF declara igualdade quando não encontra colaboradores ou ambos os resumos estão vazios. | sim | n/a | nenhuma | 08 |
| C07 | instrumentacao | app/relatorios/management/commands/diff_pdf_x_espelho.py:93; app/relatorios/management/commands/diff_pdf_x_espelho.py:138 | Resumos com chaves disjuntas podem receber “DIFF ZERO” porque somente a interseção é comparada. | sim | n/a | nenhuma | 08 |
| C08 | instrumentacao | app/relatorios/management/commands/cartao_x_txt_divergentes.py:54 | `ValueError` na classificação exclui a empresa do comparador cartão × TXT sem publicar não medição. | sim | n/a | nenhuma | 08 |
| C09 | instrumentacao | app/folha/porta_export.py:469; app/folha/porta_export.py:718 | Os comparadores restritos a quem entra no TXT podem publicar zero enquanto colaboradores excluídos conservam divergências. | sim | n/a | nenhuma | 08 |
| C10 | instrumentacao | app/folha/porta_export.py:433 | A falha de consulta de `furo_apurado` produz somente aviso e deixa vazio o contador correspondente. | sim | n/a | nenhuma | 08 |
| C11 | instrumentacao | app/folha/services/certificacao.py:85; app/folha/management/commands/colabs_nao_certificados.py:22 | Uma certificação JSON sem colaboradores é não certificada, mas publica zero não certificados. | sim | n/a | nenhuma | 08 |
| C12 | instrumentacao | app/folha/management/commands/vigiar_txt_dominio.py:57; app/folha/management/commands/vigiar_txt_dominio.py:119 | Filtrar o vigia TXT por empresa inexistente esvazia o acervo e permite concluir zero divergência e cegueira. | sim | n/a | nenhuma | 08 |
| C13 | instrumentacao | app/chamados/management/commands/auditar_invariantes_chamados.py:650; app/chamados/management/commands/auditar_invariantes_chamados.py:662 | I5b, I5c e I5d descartam exceções por chamado e podem publicar `OK 0` sem conferir a cobertura. | sim | n/a | nenhuma | 08 |
| C14 | instrumentacao | app/chamados/management/commands/auditar_invariantes_chamados.py:1168; app/chamados/management/commands/auditar_invariantes_chamados.py:1174 | S6 ignora falha do juiz, mas conserva a pergunta na quantidade anunciada da amostra. | sim | n/a | nenhuma | 08 |
| C15 | instrumentacao | app/chamados/management/commands/auditar_invariantes_chamados.py:1157 | O zero amostral de S6 não certifica as perguntas que ficaram fora das 300 selecionadas. | sim | n/a | nenhuma | 08 |
| C16 | instrumentacao | app/chamados/management/commands/auditar_invariantes_chamados.py:996 | I13 recebe zero literal e não conta portas laterais existentes ou novas. | nao | n/a | nenhuma | 08 |
| C17 | instrumentacao | app/ponto/management/commands/vigia_b5_ausencia.py:54; app/ponto/management/commands/vigia_b5_ausencia.py:107 | B5 anuncia “lavra íntegra” mesmo quando nenhuma célula foi conferida. | sim | n/a | nenhuma | 08 |
| C18 | instrumentacao | app/ponto/management/commands/vigia_plantio_overnight.py:43; app/ponto/management/commands/vigia_plantio_overnight.py:47 | O vigia de plantio pula falta de vínculo e converte erro de dia trabalhável em `None`, podendo manter zero achados. | sim | n/a | nenhuma | 08 |
| C19 | instrumentacao | app/ponto/services/vigia_pareador_celula.py:102; app/ponto/services/vigia_pareador_celula.py:113 | O vigia do pareador pode publicar cegueira zero quando parte dos turnos foi descartada por falta de posicionamento. | sim | n/a | nenhuma | 08 |
| C20 | instrumentacao | app/chamados/services/vigia_loop.py:114; app/chamados/services/vigia_loop.py:121 | `trilha_x_banco` não acusa chamado sem trilha nem última trilha sem `status_local`. | nao | n/a | nenhuma | 08 |
| C21 | instrumentacao | app/chamados/services/vigia_loop.py:30; app/chamados/services/vigia_loop.py:44 | O corte temporal do contador de loops pode separar fechamento e reabertura e esconder o par. | nao | n/a | REABRIR-PERGUNTA-A-PREMISSA — app/docs/BACKLOG.md:1642 | 08 |
| C22 | instrumentacao | app/chamados/services/vigia_loop.py:64; app/chamados/services/vigia_loop.py:87 | O contador de escritas por cron não cobre alterações sem auditoria ou com ator diferente do nome exato esperado. | nao | n/a | CONTADOR-DE-ESCRITA-POR-CRON — app/docs/BACKLOG.md:1646 | 08 |
| C23 | instrumentacao | app/core/services/crons_quebrados.py:31; app/core/services/crons_quebrados.py:37; app/core/services/crons_quebrados.py:46 | O leitor de crons quebrados transforma fonte indisponível e registros descartados ou sem exit em ausência de falhas. | nao | n/a | nenhuma | 08 |
| C24 | instrumentacao | bin/placar_code.sh:37; bin/crons.sh:27 | Falha ao renderizar crons pode virar zero instalados divergentes porque o coletor ignora o retorno e conta apenas linhas de diff. | nao | n/a | nenhuma | 08 |
| C25 | instrumentacao | bin/placar_code.sh:137; bin/placar_code.sh:144 | A falta do marcador do teste de credenciais é substituída por zero credenciais sem trilha. | nao | n/a | nenhuma | 08 |
| C26 | instrumentacao | bin/tickets_placar.sh:54; bin/placar_code.sh:51 | Falha de leitura do histórico Git ou ausência do marcador pode publicar zero citações indevidas de itens parados. | nao | n/a | nenhuma | 08 |
| C27 | instrumentacao | bin/placar_code.sh:146 | Erro do contrato de deploy sem linhas no formato esperado pode publicar zero violações. | nao | n/a | nenhuma | 08 |
| C28 | instrumentacao | bin/placar_code.sh:396; bin/placar_code.sh:528 | Fontes indisponíveis de prompts repetidos ou testes em quarentena podem ser publicadas como zero. | nao | n/a | nenhuma | 08 |
| C29 | instrumentacao | bin/selo_paginas_500.sh:16 | O contador de páginas 5xx aceita zero após falha de coleta e exclui respostas 510–599 pelo padrão usado. | nao | n/a | nenhuma | 08 |
| C30 | instrumentacao | app/config/esteira.py:32; app/config/esteira.py:34; app/config/esteira.py:47 | Eventos ausentes ou inválidos podem reduzir a zero a medição de horas da esteira parada. | nao | n/a | nenhuma | 08 |
| C31 | instrumentacao | bin/placar_code.sh:473; bin/placar_code.sh:549 | A indisponibilidade do contador de crons quebrados contribui zero ao TOTAL e pode permitir o agregado `OK`. | nao | n/a | nenhuma | 08 |
| C32 | instrumentacao | app/relatorios/management/commands/pdf_x_espelho_divergentes.py:55; app/relatorios/management/commands/cartao_x_txt_divergentes.py:44 | Os comparadores PDF × espelho e cartão × TXT aceitam tenant válido vazio e publicam zero sem universo medido. | sim | n/a | nenhuma | 08 |
| C33 | teste | app/ponto/tests/test_motor_dna.py:63 | O teste de atraso sem vínculo aceita `periodos=[]` por usar somente `all(...)`. | sim | n/a | nenhuma | 09 |
| C34 | teste | app/ponto/tests/test_motor_comercial_dia_do_turno.py:78 | O teste de dobra de feriado aceita o desaparecimento do trabalho por comparar duas somas que podem ser zero. | sim | n/a | nenhuma | 09 |
| C35 | teste | app/folha/tests/test_haiku_fora_da_folha.py:308; app/folha/tests/test_haiku_fora_da_folha.py:318 | O selo do universo usa fixture sem fechamento e aceita a omissão integral dos itens. | sim | n/a | nenhuma | 09 |
| C36 | teste | app/folha/tests/test_export_59a.py:63 | A contagem textual da cláusula de feriado não detecta argumento neutralizado por `and False`. | sim | n/a | nenhuma | 09 |
| C37 | teste | app/folha/tests/test_export_59a.py:71; app/folha/tests/test_export_59a.py:73 | O mock constante e a asserção de tipo não distinguem a data correta da competência de outra data. | sim | n/a | nenhuma | 09 |
| C38 | teste | app/folha/tests/test_l113_a0_forma_do_txt.py:110; app/folha/tests/test_l113_a0_forma_do_txt.py:115 | O selo local do hash compara a função consigo mesma e não detecta o mutante que ignora alterações numéricas. | sim | n/a | nenhuma | 09 |
| C39 | apresentacao | app/ponto/views.py:2639; app/ponto/views.py:2643 | A aprovação de ausência anuncia dias abonados mesmo quando o tipo aprovado desconta. | nao | n/a | nenhuma | 10 |
| C40 | apresentacao | app/templates/ponto/espelho.html:38; app/relatorios/pdf_espelho.py:766; app/relatorios/cartao_pela_celula.py:340 | Tela e PDF apresentam totais sem ressalvar a ausência de apuração, e a tela os chama de pagos. | nao | n/a | nenhuma | 10 |
| C41 | apresentacao | app/folha/retidos_pdf.py:90 | A coluna “Dias” do PDF de retidos pode contar motivos distintos em vez de dias. | nao | n/a | nenhuma | 10 |
| C42 | apresentacao | app/relatorios/pdf_espelho.py:307; app/relatorios/pdf_espelho.py:907 | O horário do vínculo do último dia pode ser impresso nas linhas referentes ao vínculo anterior. | nao | n/a | nenhuma | 10 |
| C43 | apresentacao | app/templates/chamados/meu_atendimento.html:117; app/templates/chamados/meu_atendimento.html:120 | A mensagem de nenhuma pendência pode aparecer enquanto há férias aguardando confirmação. | nao | n/a | nenhuma | 10 |
| C44 | apresentacao | app/chamados/services/acoes_chamado.py:313; app/chamados/services/acoes_chamado.py:323 | Alterar somente o status pode responder “Mensagem enviada” sem conteúdo enviado. | nao | n/a | nenhuma | 10 |
| C45 | apresentacao | app/chamados/services/acoes_disputa.py:284; app/chamados/services/acoes_disputa.py:295 | A reabertura afirma que o colaborador foi notificado sem conferir o resultado do FCM. | nao | n/a | nenhuma | 10 |
| C46 | apresentacao | app/templates/ponto/espelho.html:87; app/templates/ponto/espelho.html:286 | O espelho descreve como não registrado um intervalo parcialmente registrado cuja diferença foi indenizada. | nao | n/a | nenhuma | 10 |

Contagem local: **46 itens**, **32 instrumentacao**, **6 teste**, **8 apresentacao**; **22 dinheiro=sim**; **44 nenhuma**; **0 nao verificavel**. Contagem de achados por origem: **08=32**, **09=6**, **10=8**. A manifestação B5 do tenant vazio da última linha do anexo 08 já está em C17; C32 preserva os dois outros comparadores. [nao verificado]

Fusões cruzadas a considerar pelo pai:

- C09 e o achado 01/A1 de universo incompleto da folha compartilham `classificar_export`/seleção de universo, mas os comparadores são distintos; se fundir, manter a lista dos seis comparadores de 08 e a exclusão por falta de fechamento/código de 01.
- C31 e o verde agregado de 01 compartilham o TOTAL, mas possuem causas distintas: indisponibilidade substituída por zero versus contador de loop excluído da soma. [nao verificado]
- C40 e 02/L039 compartilham fallback do motor sem folha, mas são alegações distintas: rotulagem “pagas” versus faltas do motor em totais do PDF.

Afirmativas positivas explicitamente destacadas fora dos achados, caso o critério de “sem achado” inclua controles positivos:

- **08: 4** — lastro indisponível soma 1; vigia de pausa publica cego; retido calado publica cego; falha externa cartão × TXT propaga `None`.
- **09: 5** — outro selo detecta o mutante do hash; fechamento preparado no teste do export não é defeito; controle numérico de DSR; controle numérico de HE noturna; skip Chromium é apresentação e não foi tratado como cálculo monetário. [nao verificado]
- **10: 0**.

Se o censo de “sem achado” exigir uma unidade principal explicitamente absolvida pelo veredito formal, e excluir controles positivos incidentais, os três anexos têm **0** dessas unidades; o pai deve declarar um critério único para o lote.

## 05/11 17:46 · sessao a9e58e35a93d · - · - · origem=Ronald (tmux)

Referências conferidas na cópia: as citadas abaixo permanecem nas linhas originais; não encontrei caso “movida” ou “ausente no HEAD” nesta fatia. A verificação foi estática e somente leitura.

Para evitar sobreposição, o item 11.5 foi repartido entre: tolerâncias fixas incorporadas aos parâmetros sem consumo; degrau HE120; política CCT480. O item 14.6 foi repartido entre dedup de retratada e supressão de batidas opostas próximas.

| chave | classe | arquivo:linha confirmado | afirmação consolidada | dinheiro | competência | BACKLOG conferido | origens |
|---|---|---|---|---|---|---|---|
| A01 | defeito | app/folha/export.py:348; app/folha/export.py:359; app/folha/export.py:409; app/folha/export.py:411 | A regra do último dia da competência pode suprimir feriado apurado numa fatia anterior. | sim | n/a | FERI-06 — app/docs/BACKLOG.md:2094 | 11 |
| A02 | divergência | app/ponto/services/fechamento.py:910; app/ponto/services/fechamento.py:911; app/ponto/services/fechamento.py:913; app/folha/export.py:42 | O arredondamento separado das parcelas permite diferença entre HE total da tela e soma das rubricas TXT. | sim | n/a | nenhuma | 11 |
| A03 | defeito | app/folha/export.py:269; app/folha/services/vigia_txt.py:91 | Validador e vigia reconhecem datas de falta pelo literal 8792, embora o emissor permita remapear a rubrica. | sim | n/a | nenhuma; BUG-8792:1186 trata outro defeito | 11 |
| A04 | lacuna | app/core/regua_cct.py:72; app/core/regua_cct.py:458; app/ponto/motor_calculo_v2.py:51; app/ponto/motor_calculo_v2.py:60; app/ponto/motor_calculo_v2.py:69; app/ponto/motor_calculo_v2.py:2006 | Sete parâmetros CCT não chegam ao motor, que mantém tolerâncias e pisos próprios. | sim | n/a | cobertura parcial: PERCENTUAL-DO-NOTURNO-SEM-LEITOR:1602; TOLERANCIA-SEM-LEITOR:1605; INTERVALO-MINIMO-SEM-LEITOR:1606 | 11,12; também16 |
| A05 | risco | app/ponto/motor_calculo_v2.py:3141; app/ponto/calculador/regras.py:800; app/ponto/calculador/periodo.py:166 | O degrau após 120 minutos divide HE50/HE100 por constante, sem lastro escrito demonstrado na origem. | sim | n/a | nenhuma específica | 11,12 |
| A06 | divergência | app/core/regua_cct.py:378; app/ponto/motor_calculo_v2.py:3119; app/ponto/motor_calculo_v2.py:3131; app/ponto/calculador/regras.py:771 | A política CCT global “legais” limita o turno partido a 480 minutos e diverge do calculador paralelo. | sim | n/a | EXCEDENTE-IGNORA-O-SINDICATO:1604; O281:1523 (escopo relacionado) | 11,12 |
| A07 | defeito | app/folha/export.py:716; app/folha/export.py:981 | Eventos com valor e sem vínculo de rubrica são omitidos tanto na montagem quanto na homologação. | sim | n/a | nenhuma | 11; também16 |
| A08 | lacuna | app/folha/export.py:190; app/folha/export.py:428; app/folha/export.py:711 | O helper de dias abonados 8932 não é consumido pelo emissor de eventos. | sim | n/a | O137-8932-ENTERRAR — app/docs/BACKLOG.md:1491 | 11 |
| A09 | lacuna | app/core/models.py:40; app/core/regua_cct.py:374; app/core/regua_cct.py:458 | Cláusulas escritas apenas em Sindicato.obs não configuram os argumentos do motor. | sim | n/a | nenhuma | 12 |
| A10 | divergência | app/ponto/motor_calculo_v2.py:2767; app/ponto/motor_calculo_v2.py:3232; app/ponto/calculador/regras.py:764 | O portão de intrajornada usa duração bruta no 12x36 e líquida nos outros dois cálculos comparados. | sim | n/a | O281 — app/docs/BACKLOG.md:1523 (quatro gates de intra) | 12 |
| A11 | defeito | app/escala/models.py:17; app/ponto/calculador/regras.py:764; app/ponto/calculador/periodo.py:334 | O piso de pausa de 15 minutos para quatro a seis horas não alcança os portões de indenização. | sim | n/a | O281 — app/docs/BACKLOG.md:1523 (quatro gates de intra; cobertura ampla) | 12 |
| A12 | divergência | app/core/regua_cct.py:449; app/ponto/motor_calculo_v2.py:758 | Sem aplicação CCT vigente, o switch altera a prorrogação noturna da mesma jornada 12x36. | sim | n/a | nenhuma específica | 12 |
| A13 | divergência | app/ponto/motor_calculo_v2.py:2578; app/ponto/motor_calculo_v2.py:2639; app/ponto/motor_calculo_v2.py:3441; app/ponto/motor_calculo_v2.py:3454 | DSR comercial e agrupamento de extras pela data civil podem divergir do dia atribuído à jornada. | sim | 09/2026 sintética | O76:1320; O281:1523 | 12 |
| A14 | lacuna | app/ponto/utils.py:152; app/ponto/utils.py:191; app/ponto/utils.py:211 | A seleção de disponíveis para cobertura não valida o descanso interjornada de 11 horas. | não | n/a | nenhuma | 12 |
| A15 | lacuna | app/config/crons.py:17; app/config/crons.py:1821 | A agenda de crons fixa Juliani e o universo empresarial 2,3,4. | sim | n/a | nenhuma específica | 12; também16 |
| A16 | defeito | app/colaboradores/queries.py:85; app/colaboradores/queries.py:86 | O filtro de empresas visíveis exclui empresas ativas pelo prefixo literal de CNPJ 11222333. | não | n/a | nenhuma | 12 |
| A17 | risco | app/core/views_usuarios.py:331; app/core/views_usuarios.py:341; app/core/credenciais.py:185 | Gerir usuários permite redefinir a senha de um superusuário e receber a senha temporária. | não | n/a | RBAC-ESCRITA-POR-PAPEL:1725 | 13 |
| A18 | defeito | app/colaboradores/views.py:2271; app/colaboradores/views.py:2274; app/colaboradores/views.py:2303 | A escrita de agenda busca colaborador pelo ID sem conferir sua empresa contra o alcance do ator. | sim | n/a | RBAC-ESCRITA-POR-PAPEL:1725 | 13 |
| A19 | defeito | app/ponto/views.py:3032; app/ponto/services/cobertura.py:70; app/ponto/services/cobertura.py:92; app/ponto/services/cobertura.py:100 | A designação de cobertura aceita IDs sem conferir alcance empresarial nem vínculo obrigatório do chamado de origem. | sim | n/a | RBAC-ESCRITA-POR-PAPEL:1725 | 13 |
| A20 | defeito | app/ponto/views.py:436; app/ponto/views.py:456; app/ponto/views.py:471; app/ponto/views.py:477 | A permissão de visualizar painel permite aprovar ou rejeitar batida pendente buscada sem escopo empresarial. | sim | n/a | RBAC-ESCRITA-POR-PAPEL:1725 | 13 |
| A21 | defeito | app/colaboradores/views.py:2153; app/colaboradores/views.py:2165; app/colaboradores/services/beneficios.py:47 | A permissão de ler benefícios autoriza a gravação da configuração monetária de VA. | sim | 11/2026 sintética | RBAC-ESCRITA-POR-PAPEL:1725 | 13 |
| A22 | risco | app/core/views.py:974; app/core/views.py:978; app/core/views.py:1007 | O detalhe de batida devolve identificação, GPS, justificativa, histórico e foto sem escopo de empresa. | não | n/a | nenhuma | 13 |
| A23 | risco | app/ponto/views.py:1937; app/ponto/views.py:3024 | Escritas autenticadas por sessão dispensam CSRF nos fluxos de notificação negada e cobertura. | sim | n/a | nenhuma específica; RBAC não cobre explicitamente CSRF | 13 |
| A24 | risco | app/api/views_core.py:851; app/ponto/registro_batida.py:296; app/api/foto_recebida.py:61; app/api/foto_recebida.py:79 | O upload persiste conteúdo sem validar imagem e pode resolver a pendência foto_ausente. | sim | n/a | nenhuma | 13 |
| A25 | risco | app/api/views_core.py:19; app/api/credencial.py:186; app/config/settings/base.py:47; app/config/settings/saas.py:10 | JWT não carrega vínculo explícito ao tenant e pode atravessar schemas nas condições de credencial e chave descritas. | não | n/a | nenhuma | 13 |
| A26 | risco | app/api/credencial.py:124; app/api/credencial.py:138; app/api/credencial.py:150 | JWT legado é aceito sem iat ou quando falha a consulta ao estado de revogação. | não | n/a | nenhuma | 13 |
| A27 | risco | app/config/settings/base.py:6; mensageria/config/settings.py:3 | A ausência das variáveis de ambiente seleciona segredos literais previsíveis. | não | n/a | nenhuma | 13 |
| A28 | risco | app/core/ia_acoes.py:63; app/core/views_ia.py:864; app/core/views_ia.py:908; mensageria/nucleo/ferramentas.py:394 | A entrada da IA une permissões, mas não transmite autorização específica de domínio às ferramentas. | não | n/a | IA-DOMINIO-POR-FERRAMENTA:736 | 13 |
| A29 | risco | mensageria/nucleo/views.py:26; mensageria/nucleo/views.py:55; mensageria/nucleo/views.py:83 | Console, chat e tratamento de alertas internos não exigem identidade autenticada. | não | n/a | nenhuma | 13 |
| A30 | risco | app/sse_server.py:31; app/sse_server.py:41; app/sse_server.py:73; app/sse_server.py:93 | SSE toma existência de sessão como autorização para receber o canal agregado. | não | n/a | nenhuma | 13 |
| A31 | risco | app/api/views.py:2658; app/api/views.py:2674; app/api/views.py:2677 | Telemetria anônima admite texto livre e o emissor inclui CPF quando o pedido está autenticado. | não | n/a | nenhuma | 13 |
| A32 | defeito | app/chamados/services/regularizacao_ext.py:131; app/chamados/services/regularizacao_ext.py:136; app/chamados/services/regularizacao_ext.py:149; app/chamados/services/regularizacao_ext.py:153 | A regularização externa pode apagar a original ou sobrescrever seu horário e tipo. | sim | n/a | PERGUNTA-SEM-SELO:728 (classe 9 explicitamente); LAUDO-[nome]-COL94-1610:742 (censo de escritores) | 14 |
| A33 | defeito | app/ponto/views.py:477; app/ponto/registro_batida.py:285; app/ponto/registro_batida.py:291 | Rejeitar batida offline pendente executa exclusão física. | sim | n/a | nenhuma específica | 14 |
| A34 | defeito | app/ponto/services/flip_batida.py:25; app/ponto/registro_batida.py:254 | Flip substitui o tipo E/S da própria linha sem preservar seu tipo original nela. | sim | n/a | nenhuma específica | 14 |
| A35 | divergência | app/ponto/models.py:64; app/ponto/services/espelho.py:1022; app/ponto/turnos.py:2903; app/templates/ponto/espelho.html:76 | Espelhos de período omitem retratadas apesar da promessa de exibição com asterisco, e o feed cru não as sinaliza. | não | n/a | nenhuma específica | 14 |
| A36 | divergência | app/escala/utils.py:1123; app/templates/ponto/espelho.html:237; app/api/views.py:1471; app/relatorios/pdf_espelho.py:881 | A área Batidas pode mostrar o tipo do marco enquanto API e PDF mostram o tipo registrado. | não | n/a | nenhuma específica | 14 |
| A37 | defeito | app/api/views.py:557; app/ponto/registro_batida.py:138 | A deduplicação da API pode devolver como sucesso uma batida retratada e suprimir nova tentativa. | sim | n/a | nenhuma específica | 14; também17 |
| A38 | risco | app/ponto/registro_batida.py:21; app/ponto/registro_batida.py:138; app/ponto/services/triagem_batida.py:668 | A porta comum pode recusar uma marcação oposta distinta por proximidade temporal. | sim | n/a | nenhuma específica | 14 |
| A39 | risco | app/api/views.py:415; app/api/views.py:449; app/api/views.py:462 | GPS ausente e situação cadastral podem impedir a gravação de uma tentativa de ponto. | sim | n/a | nenhuma | 14 |
| A40 | defeito | app/api/hora_do_aparelho.py:63; app/api/hora_do_aparelho.py:141; app/api/hora_do_aparelho.py:148 | Uma batida offline com mais de 48 horas recebe como timestamp o horário de chegada. | sim | n/a | nenhuma | 14; também17 |
| A41 | lacuna | app/templates/core/glossario.html:155; app/ponto/views.py:57; app/ponto/views.py:70 | AFD permanece declarado pendente e não foi localizado gerador AEJ, cuja rota encontrada é importação aposentada. | não | n/a | nenhuma | 14 |
| A42 | lacuna | app/ponto/models.py:25; app/ponto/turnos.py:2365; app/ponto/turnos.py:2373 | Sequência reinicia por jornada e não demonstra NSR global sequencial. | não | n/a | nenhuma | 14 |
| A43 | lacuna | app/api/views.py:823; app/api/views.py:834 | A resposta de sucesso auditada não fornece hash, NSR ou comprovante assinado. | não | n/a | nenhuma | 14 |
| A44 | risco | app/ponto/registro_batida.py:254; app/ponto/registro_batida.py:263; app/ponto/registro_batida.py:291; app/core/auditoria.py:17 | Algumas alterações precedem auditoria cujas falhas são capturadas, permitindo perder a trilha correspondente. | sim | n/a | nenhuma específica | 14 |
| A45 | divergência | app/templates/core/glossario.html:144; app/templates/core/glossario.html:145; app/templates/core/glossario.html:149 | O glossário identifica o registrador por programa como REP-C e não sustenta a classificação REP-P solicitada. | não | n/a | nenhuma | 14 |

**Nota de referência:** para CSRF, `app/ponto/views.py:1937` continua sendo o mesmo sítio citado, mas o decorator exato está em **1938**, imediatamente abaixo; se a tabela final exigir a linha exata do decorator, use `1938 (movida)` ou preserve a outra referência original `3024`, que contém exatamente `@csrf_exempt`.

**Contagem desta fatia:** 45 itens na tabela, sendo 8 de origem11; 11 de origem12; 15 de origem13; 14 de origem14; três causas repetidas entre11/12. Portanto `8+11+15+14−3=45`.

**Grupos sem achado, critério sugerido para enumeração reproduzível:**

- **11: 1** — o formulário já avisa que os sete parâmetros não têm efeito (`app/templates/core/config/sindicato_form.html:74`). A descrição neutra de DSR fora do TXT não foi contada.
- **12: 5** — aviso CCT presente (`sindicato_form.html:74`); parâmetro consumido de afastamento da hora reduzida (`regua_cct.py:470`); versões oráculo separadas não demonstram dois pagamentos (`fechamento.py:1249`, `:1278`, `porta_export.py:284`); antigo desconto noturno por intervalo já compartilhado (`motor_calculo_v2.py:983`, `calculador/regras.py:63`, `:85`); seletor monetário por nome/identidade não demonstrado (`regua_cct.py:253`, `:352`).
- **13: 5** — `/me` e bater derivam o titular autenticado (`views_core.py:275`, `:388`); foto exige dono (`:846`); regularização filtra dono/estado (`:1319`); CSRF global ativo (`settings/base.py:58`); SQL injection explorável não demonstrada (`sse_server.py:23`, `:36`, `:77`).
- **14: 1** — retratação comum preserva a linha e `anotar` não permite timestamp (`registro_batida.py:203`, `:267`).

As negativas de exploração e as proteções acima são grupos declarados pela origem, não nova conclusão sobre produção.

## 05/11 17:45 · sessao 7d371a460cd4 · F3-BECO-1 · verificador · origem=Code

A construção fora do `with` está correta: `ListaDoAdmin.__init__` apenas guarda parâmetros e inicializa campos; a carga ocorre na primeira consulta à lista. O comando faz essas consultas pelos censos, dentro do lote. (`app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:205`, `app/chamados/management/commands/vigiar_fio_morto.py:171`)

1. **Quando lê C6, tranca e toque**

   `_carregar()` chama `cadastro_realidade.lista(hoje=..., empresas=..., agora=...)` uma vez, quando algum método da lista exige os dados; o construtor não consulta C6, tranca, toque nem `_memo_da_tranca`. (`app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:208`, `app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:224`)

   No caminho F3/C11, `beco_em_lote()` chama `alimentar_portas()`, que consulta `pergunta_viva()`; o cálculo de vitalidade consulta a tranca e, havendo tranca, o toque humano posterior a `pf.fechado_em`. (`app/escala/services/cadastro_realidade.py:417`, `app/chamados/juizes.py:1302`, `app/chamados/juizes.py:1140`, `app/chamados/juizes.py:954`)

   **Construir dentro ou fora do lote não muda a leitura**, desde que a primeira consulta aconteça no mesmo contexto, como neste comando; o objeto não captura o memo no construtor. (`app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:216`, `app/chamados/juizes.py:972`, `app/chamados/management/commands/vigiar_fio_morto.py:167`)

   Executar a carga com lote muda o compartilhamento do memo: o SIM do toque e o usuário de sistema passam a ser compartilhados pela rodada; empresa e tranca continuam no memo do avulso, por `lote=False`. Para estado estável, isso preserva os predicados consultados. (`app/chamados/juizes.py:940`, `app/chamados/juizes.py:966`, `app/chamados/juizes.py:1043`, `app/chamados/juizes.py:1068`)

2. **O destino de `agora` e os limites do congelamento**

   O comando lê `agora`, deriva `hoje` desse instante e entrega ambos à lista; `ListaDoAdmin` guarda e repassa `agora` ao carregador. (`app/chamados/management/commands/vigiar_fio_morto.py:163`, `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/services/vigia_fio.py:201`, `app/chamados/services/vigia_fio.py:216`)

   `cadastro_realidade.lista()` usa o instante recebido, encaminha-o ao C11 e à conciliação dos donos; o C11 o entrega a `InsumosDoDono`, cujo eventual caminho para o espelho também recebe esse instante. (`app/escala/services/cadastro_realidade.py:717`, `app/escala/services/cadastro_realidade.py:787`, `app/escala/services/cadastro_realidade.py:801`, `app/escala/services/cadastro_realidade.py:454`, `app/ponto/management/commands/e6_oraculo.py:159`)

   **Isso não estabelece um instante único para todo o grafo:** a lista também chama C2, que lê `date.today()`; o calendário chamado pelos censos lê `timezone.now()`; o C17 usado pelo C11 chega à partição de verbos, que também lê seu relógio. O repasse de F3 preservado pela resolução não elimina essas leituras. (`app/escala/services/cadastro_realidade.py:728`, `app/escala/services/cadastro_realidade.py:207`, `app/chamados/services/vigia_fio.py:271`, `app/colaboradores/services/calendario.py:163`, `app/chamados/juizes.py:1287`, `app/chamados/services/verbos.py:35`)

   O lote não congela `agora`, células, chamados, donos ou o resultado completo da lista: seus valores compartilhados são o usuário de sistema e os SIMs do toque; a chave do toque inclui colaborador, dia e `desde`. Uma tranca com outro `fechado_em` produz outra chave. (`app/chamados/juizes.py:999`, `app/chamados/juizes.py:1044`, `app/chamados/juizes.py:1049`, `app/chamados/juizes.py:1070`, `app/chamados/juizes.py:954`)

   A equivalência não deve ser declarada universal diante de concorrência: o toque também depende dos vínculos **atuais** célula/chamado, além da trilha. Alterações concorrentes desses vínculos ou da identidade do usuário de sistema em produção: **[não verificado]**. (`app/chamados/juizes.py:1071`, `app/chamados/juizes.py:1076`, `app/chamados/juizes.py:1081`)

3. **Escrita seguida de releitura pelo memo**

   Não identifiquei, nos caminhos F3 chamados pelo bloco, escrita persistente de célula, chamado ou trilha seguida de releitura que envelheça o SIM. O C11 consulta portas, âncoras, pais e cadastro e monta sua saída; `um_dono_por_dia()` altera o dicionário da lista em memória. (`app/escala/services/cadastro_realidade.py:417`, `app/escala/services/cadastro_realidade.py:426`, `app/escala/services/cadastro_realidade.py:444`, `app/escala/services/cadastro_realidade.py:458`, `app/escala/services/cadastro_realidade.py:934`)

   A leitura dos pais usa `pais_limbo_vivos()`; o escritor `reatar_perguntas_ao_limbo()` é outra função, que chama `reapontar_resolvedora()`, e não é chamado pelo carregador C11. (`app/escala/services/cadastro_realidade.py:428`, `app/chamados/services/limbo_folgas.py:212`, `app/chamados/services/limbo_folgas.py:224`, `app/chamados/services/limbo_folgas.py:243`)

   As escritas F3 de regeneração/rejulgamento permanecem na porta de célula, fora desse caminho de leitura; a projeção de célula para julgamento apenas cria uma cópia em memória. (`app/ponto/portas/celula.py:253`, `app/ponto/portas/celula.py:270`, `app/ponto/portas/celula.py:289`, `app/ponto/portas/celula.py:301`, `app/ponto/portas/celula.py:705`)

   **Ressalva ao “zero escrita” absoluto:** consultar o usuário de sistema chama `get_user_sistema()`, que executa `User.objects.get_or_create()`. Pode haver criação de usuário, embora isso não seja escrita F3 de célula/chamado/trilha nem o ciclo de invalidação perguntado; existência desse usuário em produção: **[não verificado]**. (`app/chamados/juizes.py:1070`, `app/core/utils.py:18`)

4. **Quem escreve e quem lê**

   | Estado | Quem escreve | Quem lê |
   |---|---|---|
   | `lista.sem_resposta` | A propriedade não tem escritor: retorna o tamanho de `_sem_resposta`; `dia_do_admin()` e `papel_do_dia()` acrescentam pares distintos ao conjunto quando falta resposta. (`app/chamados/services/vigia_fio.py:226`, `app/chamados/services/vigia_fio.py:244`, `app/chamados/services/vigia_fio.py:255`) | `censo()` e, depois dos dois censos, o comando. (`app/chamados/services/vigia_fio.py:489`, `app/chamados/management/commands/vigiar_fio_morto.py:177`) |
   | `d['fio_do_admin_sem_resposta']` | `censo()` inicializa e preenche; o comando sobrescreve com o total final da lista compartilhada, incluindo o alcance. (`app/chamados/services/vigia_fio.py:366`, `app/chamados/services/vigia_fio.py:489`, `app/chamados/management/commands/vigiar_fio_morto.py:177`) | `linhas()` imprime; o comando inclui o dicionário no carimbo. (`app/chamados/services/vigia_fio.py:884`, `app/chamados/management/commands/vigiar_fio_morto.py:221`) |
   | Dict do lote | `tranca_em_lote()` instala `{}`; `_admin_tocou_o_dia_depois()` grava somente SIM; `_toque_humano_no_dia()` grava o PK de sistema. (`app/chamados/juizes.py:999`, `app/chamados/juizes.py:1049`, `app/chamados/juizes.py:1070`) | `_memo_da_tranca()` entrega o dict; o toque consulta o SIM e exclui o usuário de sistema das testemunhas humanas. O contexto restaura o anterior ao sair, inclusive por exceção. (`app/chamados/juizes.py:972`, `app/chamados/juizes.py:1045`, `app/chamados/juizes.py:1072`, `app/chamados/juizes.py:1003`) |

5. **Classes de reprovação, limitadas à resolução**

   - **Segundo juiz / derivação paralela:** não encontrados na costura; ela preserva as chamadas existentes, e o C11 consulta C6/C17 e o juiz do dono. (`app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/management/commands/vigiar_fio_morto.py:172`, `app/escala/services/cadastro_realidade.py:417`, `app/escala/services/cadastro_realidade.py:459`)
   - **Segundo escritor fora da porta:** não encontrado nos caminhos F3 executados pelo lote; os escritores de reata e regeneração são caminhos separados dos leitores acima. (`app/chamados/services/limbo_folgas.py:212`, `app/chamados/services/limbo_folgas.py:224`, `app/ponto/portas/celula.py:253`)
   - **Relógio fora da borda:** o repasse solicitado pelo F3 foi preservado; persistem os relógios internos explicitados no item 2, sem nova leitura de relógio introduzida pelo lote. (`app/chamados/management/commands/vigiar_fio_morto.py:163`, `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/juizes.py:994`)
   - **Ordem de trava:** o contexto instala um `ContextVar`; não abre transação nem adquire trava de banco, portanto não acrescenta ordem de aquisição de locks. (`app/chamados/juizes.py:994`)
   - **Pouso antes do pré-requisito:** a ordem local está satisfeita: lote antes das cinco medições; contador final depois dos dois censos. Execução de testes, pré-requisitos operacionais e situação de produção: **[não verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:171`, `app/chamados/management/commands/vigiar_fio_morto.py:177`)

Há testes escritos para paridade lote/avulso, frescor do NÃO, reabertura/tranca, instalação do lote e repasse de `agora`; resultado de execução nesta auditoria: **[não verificado]**. (`app/chamados/tests/test_snc2_toque_em_lote.py:113`, `app/chamados/tests/test_snc2_toque_em_lote.py:137`, `app/chamados/tests/test_snc2_toque_em_lote.py:171`, `app/chamados/tests/test_snc2_toque_em_lote.py:200`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:208`)

**APROVA a resolução de merge. Bloqueantes identificados no hunk: nenhum.** (`app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:216`, `app/chamados/juizes.py:940`, `app/chamados/juizes.py:1043`, `app/chamados/management/commands/vigiar_fio_morto.py:167`)

## 05/11 17:45 · sessao 8192d62cfd23 · - · - · origem=Ronald (tmux)

Contagem bruta conferida: **lote6.r: 11 achados / 21 sem achado; lote7.r: 12 / 17; lote8.r: 13 / 16**. São 36 ocorrências antes das fusões; todas têm referências verificáveis na origem. Referências abaixo continuam no HEAD, sem trechos ausentes; linhas mais precisas dentro do mesmo bloco não representam movimentação.

| origem/item | classe | arquivo:linha conferido | afirmação consolidável | dinheiro sugerido | competência | cobertura específica |
|---|---|---|---|---|---|---|
| lote6.r/01 | b | app/docs/LEIS.md:81; app/folha/export.py:1235 | A coluna antiga da L-033 ainda afirma retenção universal, embora o retentor isente furo exclusivamente de pausa. | sim | n/a | nenhuma |
| lote6.r/02 | a | app/ponto/supra_juiz.py:330; app/ponto/supra_juiz.py:348; app/ponto/supra_juiz.py:372 | A justificativa remove `FURO_COM_REGUA_DEGENERADA`, apesar da promessa de preservar acusações de cadastro. | sim | n/a | nenhuma |
| lote6.r/03 | b | app/docs/LEIS.md:71; app/ponto/portas/celula.py:233; app/ponto/portas/celula.py:259 | A correção fora da vigência reescreve DNA sem atualizar DNA anterior, instante e contador de regeneração. | sim | n/a | nenhuma |
| lote6.r/04 | b | app/docs/LEIS.md:93; app/ponto/portas/celula.py:196 | A proibição absoluta de corrigir exportada ficou superada pela exceção `apesar_da_lavra`. | sim | n/a | nenhuma |
| lote6.r/05 | b | app/docs/LEIS.md:149; app/ponto/portas/celula.py:198; app/ponto/portas/celula.py:238 | A regeneração ordinária continua barrada por exportação, apesar da revogação dessa trava pela L-113. | sim | n/a | O121-A1-VINCULO-CORRIGE-O-PASSADO — app/docs/BACKLOG.md:1008 |
| lote6.r/06 | b | app/docs/LEIS.md:100; app/docs/LEIS.md:167; app/ponto/services/cartorio.py:1105; app/ponto/services/cartorio.py:1423; app/ponto/supra_juiz.py:278 | O diagnóstico documental de proteção temporal limitada a hoje ficou antigo diante da consulta de encerramento do turno. | sim | n/a | nenhuma para a divergência documental |
| lote6.r/07 | d | app/escala/models.py:1699; app/ponto/management/commands/gerar_celulas.py:16; app/ponto/management/commands/gerar_celulas.py:96 | O help text anuncia DNA versão 1, mas o produtor grava versão 2. | nao | n/a | nenhuma |
| lote6.r/08 | a | app/ponto/supra_juiz.py:264; app/ponto/supra_juiz.py:530 | O comentário restringe a parcial ao portão do furo, mas ela também impede a classificação de cobrança em dia coberto. | sim | n/a | nenhuma para a divergência documental |
| lote6.r/09 | e | app/ponto/tests/test_carimbo_isencao_lampada.py:29; app/ponto/management/commands/processar_cartorio.py:27; app/ponto/management/commands/processar_cartorio.py:65; app/ponto/services/cartorio.py:900 | A docstring diz que `--forcar` é somente sombra, embora possa ser combinado com escrita. | sim | n/a | nenhuma |
| lote6.r/10 | e | app/ponto/tests/test_carimbo_isencao_lampada.py:23; app/ponto/services/cartorio.py:1362 | O alcance histórico ZERO de 1.202 lâmpadas não é demonstrado pelo dado fornecido. [nao verificado] | sim | n/a | nenhuma |
| lote6.r/11 | a | app/escala/services/leitor_celula.py:204; app/escala/services/leitor_celula.py:213; app/escala/utils.py:850; app/escala/utils.py:858 | A isenção modifica julgamento de batida real e alcança folga e célula sem marco, contrariando a docstring do leitor. | nao | n/a | nenhuma |
| lote7.r/01 | d | app/colaboradores/models.py:46; app/colaboradores/views.py:499; app/ponto/janelas.py:51 | O cadastro anuncia corte entre 1 e 28, mas view e motor elevam 1 para 2. | sim | n/a | FERI-11 — app/docs/BACKLOG.md:2099 |
| lote7.r/02 | a | app/ponto/views.py:1248; app/ponto/services/fechamento.py:37; app/ponto/services/fechamento.py:552 | O comentário promete recálculo irrestrito de exportadas, mas a guarda exclui empresas exportadas de setembro/2026. | sim | 09/2026 | nenhuma para esse comentário |
| lote7.r/03 | a | app/folha/views.py:213; app/folha/views.py:247 | Entrar no TXT não garante aprovação, porque o carimbo exclui itens com determinadas travas. | sim | n/a | nenhuma específica |
| lote7.r/04 | a | app/api/views.py:1662; app/relatorios/cartao_pela_celula.py:340; app/relatorios/cartao_pela_celula.py:344 | O comentário descreve duas fontes de totais, mas o fluxo também fornece `lavratura`. | nao | n/a | nenhuma |
| lote7.r/05 | d | app/ponto/models.py:757; app/ponto/models.py:875; app/ponto/models.py:883 | O help text fixa falta inteira em 720, mas a derivação automática usa o previsto diário e preserva minutos positivos informados. | sim | n/a | nenhuma para o help text |
| lote7.r/06 | d | app/ponto/models.py:584; app/ponto/services/ausencia.py:333; app/ponto/services/ausencia.py:779 | O help text promete recusa sem comprovante, mas a ausência pode nascer aguardando documento. | sim | n/a | nenhuma |
| lote7.r/07 | d | app/ponto/models.py:586; app/api/views_core.py:402; app/api/views_core.py:680 | `bloqueia_ponto` promete impedir registro, mas colaborador afastado pode registrar batida e receber aviso. | sim | n/a | nenhuma |
| lote7.r/08 | b | app/docs/LEIS.md:78; app/docs/LEIS.md:161; app/api/views_core.py:402; app/api/views_core.py:440; app/api/views_core.py:458 | A promessa absoluta de nunca barrar batida conflita com recusas por férias e GPS ausente. | sim | n/a | nenhuma |
| lote7.r/09 | e | app/api/tests/test_api_bater_ponto.py:533; app/api/views.py:600; app/api/views.py:608; app/api/tests/test_api_bater_ponto.py:569 | A docstring espera segunda entrada distante como E e disputa, mas o fluxo ao vivo corrige o tipo e o teste espera E/S. | sim | n/a | nenhuma |
| lote7.r/10 | d | app/colaboradores/models.py:115; app/folha/porta_export.py:230; app/folha/porta_export.py:691 | O help text omite que a trava de HE considera somente dias acima do limite e sem decisão. | sim | n/a | O214, item 3 — app/docs/BACKLOG.md:1134 |
| lote7.r/15 | b | app/docs/LEIS.md:174; app/api/views_mensageria.py:723; app/api/views_mensageria.py:883 | O mês civil também é escolhido com zero cortes, contrariando a descrição de uso somente quando eles divergem. | nao | n/a | nenhuma específica |
| lote7.r/29 | b | app/docs/LEIS.md:99; app/core/tests/test_smoke_chromium.py:35 | O dado fornecido não comprova o smoke humano nas duas cascas antes do push exigido pela L-072. [nao verificado] | nao | n/a | nenhuma |
| lote8.r/01 | a | app/config/crons.py:1614; app/config/crons.py:1669; app/config/crons.py:1701; app/config/crons.py:1802; bin/encadeado.sh:102 | Os comentários dizem que `log` não tem leitor nos encadeados, mas renderização e executor o utilizam. | nao | n/a | nenhuma específica |
| lote8.r/02 | a | app/config/crons.py:2119; app/folha/services/vigia_txt.py:231; app/folha/services/vigia_txt.py:358; app/folha/services/vigia_txt.py:367 | O cadastro diz que o vigia TXT consulta duas autoridades, mas o censo retirou a classificação do dono. | nao | n/a | nenhuma |
| lote8.r/03 | a | app/config/crons.py:1533; app/config/crons.py:1618 | O vigia TXT é descrito como único elo sem escrita, embora também exista o vigia de pausa. | nao | n/a | nenhuma |
| lote8.r/09 | b | app/docs/LEIS.md:94; app/config/crons.py:1483; app/config/crons.py:1995; app/chamados/management/commands/disparar_perguntas_competencia.py:280 | A varredura agenda emissão com `--apply`, contrariando a cláusula de cron que somente alarma. | sim | n/a | DISPARAR-PERGUNTAS-NO-CONTRATO-20 — app/docs/BACKLOG.md:780 |
| lote8.r/12 | b | app/docs/LEIS.md:127; app/config/crons.py:1955; app/config/crons.py:2114 | O diagnóstico de sete papéis sem `prazo` e 27 juízes ficou antigo diante do cadastro atual. | nao | n/a | nenhuma para a prosa velha |
| lote8.r/13 | b | app/docs/LEIS.md:191; app/config/crons.py:2062; app/config/crons.py:2069; app/config/crons.py:2071; app/config/crons.py:2084 | A lista documental ainda trata como juízes de varredura comandos que já têm papel `prazo`. | nao | n/a | nenhuma para a prosa velha |
| lote8.r/14 | b | app/docs/LEIS.md:194; app/config/crons.py:1955; app/config/crons.py:2114; bin/tests/test_papel_prazo_nao_deriva.sh:247; bin/tests/test_papel_prazo_nao_deriva.sh:295 | O índice declara pendentes o papel `prazo` e sua guarda, embora ambos existam. | nao | n/a | nenhuma para a prosa velha |
| lote8.r/18 | a | app/config/crons.py:1922; app/config/crons.py:2069 | O comentário ainda mantém o par relâmpago em `juiz`, mas o cadastro já o atribui a `prazo`. | nao | n/a | nenhuma para esse comentário |
| lote8.r/19 | a | app/ponto/management/commands/censo_fase_12x36.py:270; app/ponto/management/commands/censo_fase_12x36.py:299; app/ponto/management/commands/censo_fase_12x36.py:306; app/ponto/management/commands/censo_fase_12x36.py:314 | A lavra do censo é descrita como somente contador, mas também contém itens, cobertura e critério. | nao | n/a | nenhuma para a divergência documental |
| lote8.r/20 | a | app/config/crons.py:453; app/ponto/management/commands/tripwire_situacao_afastado.py:35; app/ponto/management/commands/tripwire_situacao_afastado.py:39; app/ponto/management/commands/tripwire_situacao_afastado.py:51 | Falta saída datada que demonstre o zero exigido para autorizar os leitores de `situacao`. [nao verificado] | sim | n/a | nenhuma |
| lote8.r/22 | d | app/colaboradores/models.py:370; app/colaboradores/queries.py:53; app/colaboradores/queries.py:65; app/ponto/management/commands/detectar_ausencias.py:98 | O help text exige flag individual de operação, mas a consulta usa rollout da empresa e vínculo vigente. | sim | n/a | nenhuma |
| lote8.r/23 | d | app/colaboradores/models.py:228; app/ponto/management/commands/detectar_ausencias.py:470; app/ponto/management/commands/detectar_ausencias.py:473; app/core/configuracao_efeito.py:123 | `tolerante_offline` promete adiar falta, mas perdeu seu leitor e o gate usa vencimento de marcos e encerramento do dia. | sim | n/a | nenhuma |
| lote8.r/26 | e | app/ponto/tests/test_vigia_reconciliar_vinculo.py:1; app/ponto/tests/test_vigia_reconciliar_vinculo.py:25; app/config/crons.py:2138 | A docstring mantém `reconciliar_vinculo` como juiz, mas o cadastro e a asserção atual o classificam como vigia. | nao | n/a | nenhuma para a divergência documental |

Fusões com outros lotes: lote6/01 = lote1/01 = lote2/28; lote6/03 = lote4/10; lote6/04 = lote1/03 = lote4/11; lote6/05 = lote1/07 = lote4/12; lote6/07 = lote4/05; lote7/02 = lote1/18; lote7/03 = lote1/10; lote7/05 = lote1/33 = lote2/18 = lote3/15; lote7/06 = lote3/12; lote7/07 = lote3/13; lote7/10 compartilha causa com lote1/05.

**Atenção à fusão de lote7/08:** ela agrega a recusa de férias de lote3/16 e a recusa de GPS de lote5/E1; esses dois são causas distintas no mesmo endpoint, enquanto a divergência da L-030 é uma formulação documental mais ampla.

**Coberturas que não considerei específicas:** `LOG-DO-CRON-ORFAS-SUMIU` em BACKLOG:732 trata da falta de log, não dos comentários antigos; `PAPEL-PRAZO-CABECALHO` em BACKLOG:1668 trata de outro arquivo; `FERI-10` em BACKLOG:2098 trata de outro trecho da consulta de competência.

## 05/11 17:45 · sessao 8c6126039d63 · - · - · origem=Ronald (tmux)

Todos os trechos abaixo existem na linha indicada no HEAD e sustentam o mecanismo inventariado; não precisei marcar referência como movida ou ausente.

`B:NN` significa `app/docs/BACKLOG.md:NN`. Não usei correspondência temática ampla como cobertura.

| origem/item | classe | referência conferida | afirmação consolidável | dinheiro | competência | cobertura BACKLOG |
|---|---|---|---|---|---|---|
| 15/1 | risco | app/ponto/management/commands/processar_cartorio.py:61; app/ponto/services/cartorio.py:1306; app/ponto/services/cartorio.py:1307; app/ponto/services/cartorio.py:1362 | O cartório percorre histórico sem limite inferior, consulta ausência e varre batidas por célula antes da guarda de impressão inalterada. | nao | n/a | nenhuma |
| 15/2 | risco | app/ponto/portas/celula.py:598; app/escala/models.py:1286; app/escala/models.py:1310 | O gerador consulta folgas por vínculo/dia antes de pular células e grava criações e alterações individualmente. | nao | n/a | nenhuma |
| 15/3 | risco | app/ponto/portas/celula.py:872; app/ponto/portas/celula.py:875 | A apuração de furos executa atualização individual por dia e renova a avaliação mesmo sem mudança funcional. | nao | n/a | nenhuma |
| 15/4 | risco | app/ponto/services/he_pendente_lavrado.py:92; app/ponto/services/he_pendente_lavrado.py:94; app/ponto/services/he_pendente_lavrado.py:132 | O lavrador de HE monta espelho por ativo antes de descartar dias sem HE pendente. | nao | n/a | LAVRAR-HE-PENDENTE-ESCALA, B:750, **cobertura parcial**, pois a linha registra segunda apuração e piso visual, mas não promete eliminar espelho por ativo |
| 15/5 | risco | app/escala/views_plano_folgas.py:59; app/escala/views_plano_folgas.py:66; app/escala/views_plano_folgas.py:76; app/escala/models.py:1092 | O GET do Plano de Folgas consulta relação por vínculo e célula por dia sem carga prévia. | nao | n/a | nenhuma |
| 15/6 | risco | app/escala/views.py:407; app/escala/views.py:411; app/escala/views.py:412; app/escala/views.py:416; app/escala/folgas.py:192 | Aplicar calendário ao tipo repete a regeneração dos vínculos numa transação coletiva síncrona. | nao | n/a | nenhuma |
| 15/7 | risco | app/core/views.py:177; app/core/views.py:376; app/core/views.py:779; app/core/views.py:1181; app/core/views.py:798 | Quatro painéis consultam turno individualmente, e o filtro de empresa da TV acontece depois dessa apuração. | nao | n/a | nenhuma; O113 é painel situacional, expressamente separado pela origem |
| 15/8 | risco | app/ponto/views.py:376; app/ponto/views.py:392; app/ponto/views.py:405 | A tela de turnos abertos percorre todos os ativos e consulta chamados antes de aplicar o filtro de nível. | nao | n/a | nenhuma |
| 15/9 | risco | app/folha/export.py:942; app/folha/export.py:1188; app/folha/export.py:1194 | O classificador da folha carrega células e vínculos individualmente por fechamento. | nao | n/a | nenhuma |
| 15/10 | risco | app/folha/views.py:723; app/folha/views.py:764; app/folha/previa.py:58; app/folha/previa.py:170 | A prévia classifica e consulta a população inteira antes da paginação e repete retenção para propostas. | nao | n/a | nenhuma |
| 15/11 | risco | app/relatorios/furos.py:124; app/relatorios/furos.py:160; app/colaboradores/services/calendario.py:221 | A auditoria de espelhos realiza carga coletiva e depois recria calendários com cargas individuais por card. | nao | n/a | FUROS-EMPRESA-TIMEOUT, B:1775 |
| 16/1 | defeito | app/colaboradores/views.py:499; app/ponto/janelas.py:15; app/ponto/janelas.py:26 | O corte 1 é convertido em 2 e impede representar competência civil 1–31. | sim | 10/2026 e 11/2026, exemplos sintéticos | FERI-11, B:2099 |
| 16/2 | divergência | app/tenants/views.py:48; Caddyfile:25; app/config/settings/saas.py:115 | O provisionamento e o wildcard aceitam novo domínio que a lista SaaS de hosts não permite. | nao | n/a | nenhuma |
| 16/3 | lacuna | app/config/crons.py:17; app/config/crons.py:18; app/config/crons.py:462; app/config/crons.py:1826 | A agenda fixa tenant Juliani e empresas 2, 3 e 4, deixando novo tenant e novas empresas fora da expansão. | sim | n/a | nenhuma |
| 16/4 | lacuna | app/core/regua_cct.py:72; app/ponto/motor_calculo_v2.py:2006 | Sete parâmetros CCT cadastráveis não são consumidos pelo cálculo. | sim | n/a | PERCENTUAL-DO-NOTURNO-SEM-LEITOR B:1602; TOLERANCIA-SEM-LEITOR B:1605; INTERVALO-MINIMO-SEM-LEITOR B:1606, **cobertura parcial dos sete** |
| 16/5 | divergência | app/core/models.py:85; app/escala/models.py:17; app/escala/models.py:352; app/escala/models.py:597 | O sindicato aceita intervalo mínimo de 30 minutos que a validação fixa da escala recusa acima de seis horas. | sim | n/a | INTERVALO-MINIMO-SEM-LEITOR B:1606 é relacionado, porém não cobre explicitamente a recusa da escala; recomendação conservadora: nenhuma |
| 16/6 | lacuna | app/tenants/bootstrap.py:27; app/tenants/bootstrap.py:34; app/colaboradores/models.py:46; app/colaboradores/models.py:151; app/core/regua_cct.py:444; app/core/regua_cct.py:449; app/core/regua_cct.py:366 | O bootstrap não configura os requisitos trabalhistas e a CCT depende de switch e aplicação vigente sem empate. | sim | n/a | nenhuma; O309 trata aviso do empate, não bootstrap incompleto |
| 16/7 | lacuna | app/folha/export.py:29; app/folha/export.py:846 | Empresa sem integração e pessoa sem código Domínio encontram pré-requisitos cadastrais que impedem exportar. | sim | n/a | nenhuma |
| 16/8 | defeito | app/folha/export.py:716; app/folha/export.py:717; app/folha/export.py:981; app/folha/export.py:982 | Evento com valor sem rubrica vinculada desaparece da montagem e da homologação sem retenção por essa ausência. | sim | n/a | nenhuma |
| 16/9a | risco | app/folha/export.py:996; app/folha/export.py:1037; app/folha/export.py:1042 | A memória da janela usa empresa e competência sem tenant e pode reutilizar janela de outro schema. | sim | 10/2026, exemplo sintético | nenhuma |
| 16/9b | risco | app/pautas/context_processors.py:23; app/pautas/context_processors.py:29; app/pautas/context_processors.py:33 | O cache de pautas usa somente ID do usuário e pode compartilhar número e remetentes entre tenants. | nao | n/a | nenhuma |
| 16/10 | defeito | app/core/views_ia.py:23; app/core/views_ia.py:909; mensageria/nucleo/views.py:136 | O assistente envia o cliente global configurado em vez do tenant da requisição. | nao | n/a | nenhuma; IA-DOMINIO-POR-FERRAMENTA B:736 é autorização por ferramenta, causa distinta |
| 16/11 | lacuna | app/core/constants.py:5; app/ponto/services/fechamento.py:204; app/escala/alimentacao.py:390 | O piso global de operação de Juliani exclui fatias e previsto anteriores a 21/05/2026 de qualquer cliente. | sim | 05/2026, exemplo sintético | nenhuma |
| 16/12 | divergência | app/folha/porta_export.py:809; app/folha/porta_export.py:810; app/folha/porta_export.py:812 | A mensagem de recuperação do export manda executar a medição no schema Juliani mesmo para outro tenant. | sim | n/a | nenhuma |
| 17/1 | defeito | app/static/pwa.js:50; app/static/pwa.js:113; app/static/pwa.js:128; app/api/views_core.py:389 | A fila offline não guarda titular e pode enviar a batida de A como pertencente à sessão de B. | sim | n/a | PWA-OFFLINE-APP-SHELL, B:1745 |
| 17/2a | defeito | app/static/pwa.js:109; app/static/pwa.js:114; app/static/pwa.js:147; app/api/hora_do_aparelho.py:141 | A fila guarda horário corrigido pelo gap, mas envia somente o horário bruto do aparelho. | sim | n/a | nenhuma |
| 17/2b, prosa | defeito | app/api/hora_do_aparelho.py:63; app/api/hora_do_aparelho.py:148; app/static/pwa.js:158 | Batida offline com mais de 48 horas é gravada no horário da chegada e a pendência é apagada após sucesso HTTP. | sim | n/a | nenhuma; **fundir 14/8** |
| 17/3 | lacuna | app/static/pwa.js:143; app/ponto/models.py:106; app/api/hora_do_aparelho.py:148 | O reenvio offline não leva chave estável do evento e pode duplicar tentativa antiga cujo horário vira novo agora. | sim | n/a | nenhuma |
| 17/4 | defeito | app/api/views_core.py:551; app/api/views_core.py:558; app/static/pwa.js:158 | A deduplicação pode devolver batida retratada como sucesso e apagar a nova pendência sem entrada ativa. | sim | n/a | nenhuma; **fundir parcela retratada de 14/6** |
| 17/5 | divergência | app/static/pwa.js:117; app/static/pwa.js:143; app/ponto/services/geofence.py:117; app/ponto/services/geofence.py:121 | O reenvio offline perde a precisão GPS e muda a classificação da mesma localização perante o envio online. | nao | n/a | nenhuma |
| 17/6 | divergência | app/templates/ponto/bater_ponto_v2.html:340; app/api/views_core.py:592; app/api/views_core.py:600 | A tela anuncia sucesso com E/S local sem ler o tipo efetivamente corrigido pelo servidor. | nao | n/a | nenhuma |
| 17/7a | defeito | app/static/sw.js:46; app/static/sw.js:50; app/static/sw.js:67; app/static/sw.js:74 | A navegação retorna antes da gravação do cache HTML e a visita online não prepara a tela para reabertura offline. | nao | n/a | PWA-OFFLINE-APP-SHELL, B:1745 |
| 17/7b, prosa | lacuna | app/static/sw.js:116; app/static/sw.js:121; app/static/sw.js:122 | O Background Sync sem página aberta somente notifica para abrir o aplicativo e não envia a fila. | sim | n/a | nenhuma |
| 17/8 | divergência | app/config/settings/base.py:93; app/core/views.py:14; app/templates/ponto/bater_ponto_v2.html:163; app/templates/ponto/bater_ponto_v2.html:168 | O relógio passa da hora do servidor ao fuso local do aparelho quando entra no fallback. | nao | n/a | nenhuma |
| 17/9 | defeito | app/api/views.py:1071; app/api/views.py:1156; app/ponto/models.py:701; app/api/views.py:3101 | O estado antigo pendente faz o contrato de chamados ocultar anexo faltante em aguardando_documento. | nao | n/a | nenhuma; API-NAO-TEM-PORTA-DE-ANEXAR... B:1052 refere-se à porta, não a esse estado morto |
| 17/10 | divergência | app/api/views_core.py:249; app/api/views_core.py:250; app/api/views.py:939; app/escala/models.py:1124 | As APIs anunciam horários do template mesmo quando a autoridade do vínculo lê horários congelados na célula. | nao | n/a | nenhuma |
| 17/11 | lacuna | app/api/views_core.py:1298; app/api/views_core.py:1302; app/chamados/models.py:3214 | O payload de regularização omite operação, alvo e intervalo e torna operações diferentes indistinguíveis. | sim | n/a | nenhuma |
| 17/12 | divergência | app/api/views_core.py:1332; app/chamados/services/regularizacao_ext.py:58; app/chamados/services/regularizacao_ext.py:352 | A confirmação que falha em materializar batidas devolve HTTP 200 embora o corpo sinalize falha. | sim | n/a | nenhuma |
| 17/13 | divergência | app/api/views.py:1172; app/api/views.py:2553; app/chamados/services/disputa_emissao.py:1267 | Um GET oferece OUTRO_MOTIVO para pergunta de hora enquanto outro remove a opção e o POST a recusa. | nao | n/a | nenhuma; API-CHAMADOS-V2-SERVE-PERGUNTA-SEM-A-LEI B:1577 trata pergunta viva/morta, não filtragem das opções |
| 17/14 | defeito | app/static/pwa.js:246; app/static/pwa.js:251; app/static/pwa.js:367; app/ponto/views.py:2000 | A inscrição push anuncia sucesso sem conferir a recusa HTTP do servidor e a assinatura local impede renovação automática. | nao | n/a | nenhuma |
| 17/15 | risco | app/static/pwa.js:335; app/ponto/views.py:1989; app/chamados/push_utils.py:24 | A associação WebPush permanece no titular anterior após troca da conta no navegador. | nao | n/a | nenhuma |
| 17/16 | divergência | app/chamados/push_utils.py:28; app/static/sw.js:97; app/static/sw.js:107 | O SW ignora URL e requireInteraction enviados pelo emissor e abre a raiz sem persistência solicitada. | nao | n/a | nenhuma |

Observações para a consolidação:

- 16/4 deve fundir com 11/4 e 12/1; seu exemplo de tolerância também encosta em 11/5.
- 16/8 deve fundir com 11/6.
- 16/3 deve fundir com 12/universo fixo dos crons.
- 17/2b deve fundir com 14/8.
- 17/4 deve fundir com a parcela de deduplicação de retratadas de 14/6; a recusa de opostas próximas do mesmo item 14 tem causa adicional.
- Não marquei 16/7 como conforme: a origem a incluiu expressamente na tabela de achados como lacuna CADASTRO, embora delimite que os bloqueios não demonstram defeito estrutural.
- A referência `app/core/constants.py:3` da prosa16 é comentário; a tabela de origem também fornece a referência correta `:5`, portanto usei `:5` sem “movida”.
- 15/7 usa chamadas `:177/:376/:779/:1181`, imediatamente depois dos laços citados pela origem; não houve movimento de trecho, apenas âncora mais específica.
- 15/11 tem a carga coletiva na linha124, continuação da linha123 citada pela origem; não houve movimento de trecho.

Contagem de achados de origem antes das fusões entre anexos: **15=11; 16=13; 17=18**, contando separadamente em17 a transformação >48h e a limitação do Background Sync, ambas afirmadas na prosa.

Contagem sugerida de afirmações expressamente sem achado, por proposição delimitadora: **15=3** (situacional plural; retrato materializado; caderno em processo próprio); **16=6** (motores além do12x36; trava de âncora específica; semente sistema; semente tipos de ausência; semente rubrica HE100; permissões por signal); **17=3** (divergência de fuso não comprova ISO incorreto; FCM distribuído não demonstrado divergente; mudança histórica sem versão não comprovada). Esses números dependem de o consolidado contar controles demonstrados como afirmações sem achado; se o critério for somente rótulo literal de veredito, os três anexos não têm tabelas de “conforme/sem achado”.

## 05/11 17:45 · sessao 683897038ae3 · - · - · origem=Ronald (tmux)

Conferência somente leitura: referências abaixo continuam nas linhas citadas, sem deslocamento ou ausência nos trechos conferidos. Contagem bruta: lote1.r = **17 achados + 17 sem achado**; lote2.r = **20 achados + 16 sem achado**. As contagens derivam dos anexos, não de arquivo local. [nao verificado]

Todos os caminhos abaixo são relativos a `app/`. “Nenhuma” significa que não identifiquei cobertura específica após consulta ao BACKLOG; cobertura não explicitada permanece [nao verificado].

| chave de consolidação | classe original | referências conferidas | afirmação consolidável | dinheiro | competência | cobertura específica conferida | origem |
|---|---|---|---|---|---|---|---|
| Retenção universal de furo | b | docs/LEIS.md:81; folha/export.py:1147; folha/export.py:1235 | A coluna antiga de L-033 ainda descreve retenção universal, enquanto o retentor isenta furo exclusivamente de pausa. | sim | n/a | O236, docs/BACKLOG.md:1164 cobre a implementação da isenção, mas não explicita correção da coluna antiga | lote1.r:01; lote2.r:28 |
| Exportada intocável | b | docs/LEIS.md:93; ponto/portas/regen_exportada.py:106; ponto/portas/regen_exportada.py:174 | A proibição absoluta de corrigir folha emitida diverge da porta de retificação com autor e motivo. | sim | n/a | O121, docs/BACKLOG.md:1189 cobre regeneração em exportada; O121-A1-VINCULO-CORRIGE-O-PASSADO, docs/BACKLOG.md:1008 | lote1.r:03 |
| TXT ainda chama motor | b | docs/LEIS.md:113; folha/views.py:208; ponto/services/fechamento.py:277 | A geração do TXT recalcula em leitura e alcança o motor apesar da promessa de somente ler a lavratura. | sim | n/a | O-DIA-PAGO S4, docs/BACKLOG.md:1095 | lote1.r:04 |
| Trava HE não universal | b | docs/LEIS.md:115; folha/porta_export.py:230; folha/porta_export.py:690 | A trava de HE considera somente dias acima do limite sem decisão e depende do campo da empresa, contrariando a formulação universal. | sim | n/a | O214, docs/BACKLOG.md:1134, item 3 explicitamente cobre limite e configuração | lote1.r:05 |
| Regeneração ordinária ainda bloqueada | b | docs/LEIS.md:149; ponto/portas/celula.py:196; ponto/portas/celula.py:199 | A regeneração ordinária continua submetida à guarda de exportação quando não recebe `apesar_da_lavra`. | sim | n/a | O121-A1-VINCULO-CORRIGE-O-PASSADO, docs/BACKLOG.md:1008; O121, docs/BACKLOG.md:1189 | lote1.r:07 |
| DIFF prévio não demonstrado | b | docs/LEIS.md:149; folha/management/commands/regerar_txt_dominio.py:99; folha/management/commands/regerar_txt_dominio.py:118 | Os artefatos do regerador não demonstram publicação do DIFF antes do ato de frota. | sim | n/a | nenhuma | lote1.r:08 |
| NULL não basta para vigência | d | folha/models.py:97; folha/models.py:134; folha/models.py:137 | `invalidada_em` nulo não garante vigência porque somente a geração mais recente é vigente. | sim | n/a | nenhuma | lote1.r:09; lote2.r:19 |
| TXT não garante aprovação | a | folha/views.py:213; folha/views.py:247; folha/views.py:268 | Um colaborador classificado como `entra` pode permanecer no TXT sem receber aprovação quando tem trava considerada pelo carimbo. | sim | n/a | FECH-05, docs/BACKLOG.md:2069 cobre o sítio `_apv` e a pergunta de aprovação | lote1.r:10 |
| Parcela feriado 12x36 | e | folha/tests/test_export_59a.py:1; folha/tests/test_export_59a.py:48; folha/export.py:411 | A docstring de teste proíbe universalmente parcela de feriado em 12x36, mas a emissão ocorre com cláusula de dobra ligada. | sim | n/a | nenhuma | lote1.r:11 |
| Recalcular qualquer exportada | a | ponto/services/fechamento.py:476; ponto/services/fechamento.py:551; ponto/services/fechamento.py:553 | A generalização de que exportação não barra recálculo omite a guarda específica de setembro de 2026. | sim | 09/2026 | nenhuma específica para essa divergência de comentário | lote1.r:18 |
| Congelamento não universal | a | ponto/services/fechamento.py:1441; ponto/services/fechamento.py:1144; folha/porta_export.py:417 | O comentário de imutabilidade de competência trancada ou paga diverge do escritor que pode atualizar valores fora da guarda de setembro. | sim | 09/2026 é a guarda; 10/2026 é o contraexemplo | nenhuma específica; O80 em docs/BACKLOG.md:1324 é histórico da antiga recusa | lote1.r:19; lote2.r:20 |
| Ninguém lê DiaPago | a | ponto/services/fechamento.py:1182; colaboradores/services/calendario.py:290; ponto/services/dia_pago.py:852 | O comentário de que ninguém lê as linhas de `DiaPago` está desatualizado diante dos leitores existentes. | nao | n/a | O-DIA-PAGO S3, docs/BACKLOG.md:1090 cobre os leitores; não explicita a correção desse comentário | lote1.r:24 |
| Avulso apaga anterior | a | holerite/models.py:138; holerite/views.py:1191; holerite/views.py:1193 | O upload avulso apaga holerite anterior sem ciência apesar da promessa de preservar versões. | nao | n/a | nenhuma; PORT-70 em docs/BACKLOG.md:1987 só cobre porta/smoke genericamente | lote1.r:25 |
| Empresa folha do avulso | d | holerite/models.py:123; holerite/views.py:1199; holerite/views.py:1202 | O upload avulso grava a empresa atual do colaborador apesar do help definir a empresa impressa no documento. | nao | n/a | nenhuma | lote1.r:26 |
| Carta duplicada na republicação | a | holerite/models.py:143; holerite/motor.py:448; holerite/motor.py:475; holerite/motor.py:480 | A primeira publicação conjunta não preenche `anexos_sha`, permitindo anexar novamente a mesma carta na republicação. | nao | n/a | nenhuma | lote1.r:29 |
| CPF e 29/29 históricos | a | holerite/motor.py:180; holerite/motor.py:186 | A docstring afirma ausência de CPF e resultado histórico 29/29 sem documentos ou cadastro verificáveis nos anexos. | nao | n/a | nenhuma | lote1.r:32 |
| Falta inteira 720 | d | ponto/models.py:757; ponto/models.py:875; ponto/models.py:883 | O help fixa falta inteira em 720 minutos, mas a derivação automática soma o previsto diário e preserva valores positivos informados. | sim | n/a | nenhuma para esse help; H06-FALTA-PELA-GRADE em docs/BACKLOG.md:1742 cobre outro caminho, materialização com 720 fixo | lote1.r:33; lote2.r:18 |
| Tolerância 20 versus 10 HE | a | ponto/motor_calculo_v2.py:53; ponto/motor_calculo_v2.py:2845; ponto/motor_calculo_v2.py:2851 | A promessa de tolerar vinte minutos diários sem HE diverge do ramo que elimina excedentes de até dez minutos. | sim | n/a | nenhuma; TOLERANCIA-SEM-LEITOR em docs/BACKLOG.md:1605 trata campos de sindicato, outro defeito | lote2.r:01 |
| Entrada três horas depois | a | ponto/motor_calculo_v2.py:2104; ponto/motor_calculo_v2.py:2122; ponto/motor_calculo_v2.py:2148 | O comentário exclui genericamente atraso após três horas, mas o código restringe essa exclusão ao dia sem marcos de intervalo. | sim | n/a | nenhuma | lote2.r:02 |
| Feriado previsto pode dobrar | a/b | ponto/motor_calculo_v2.py:2807; ponto/motor_calculo_v2.py:934; docs/LEIS.md:118 | O comentário e a cláusula original excluem dobra em feriado previsto, enquanto o helper decide por política de dobra e feriado. | sim | n/a | nenhuma específica para a divergência documental | lote2.r:03; lote2.r:31 |
| Intra 4h–6h | c | ponto/motor_calculo_v2.py:3226; ponto/motor_calculo_v2.py:3232; escala/models.py:19 | O ramo comercial não indeniza pausa faltante em duração acima de quatro e até seis horas apesar do piso consultado pelo motor. | sim | n/a | O270, docs/BACKLOG.md:1511 | lote2.r:04 |
| Dedup núcleo retirado | a | ponto/calculador/alimentacao.py:333; ponto/calculador/nucleo.py:38; ponto/calculador/nucleo.py:186 | A narrativa ainda atribui deduplicação de dez minutos ao núcleo que já registra a retirada e percorre todas as batidas. | sim | n/a | nenhuma | lote2.r:11 |
| Perdão entrada proximidade | e | ponto/tests/test_calculador_portoes_de_marco.py:8; ponto/calculador/regras.py:251; escala/regua_defesa.py:472 | A docstring descreve perdão da entrada por proximidade da volta, mas o código consulta marco ocupado e conformidade. | sim | n/a | nenhuma específica; MOTOR-PASSA-O-TURNO-A-VOLTA em docs/BACKLOG.md:779 trata resíduo do alinhamento por período | lote2.r:16 |
| Flag intra não operacional | b | docs/LEIS.md:79; ponto/motor_calculo_v2.py:822; ponto/motor_calculo_v2.py:2784 | A cláusula original atribui indenização a `intervalo_indenizavel`, enquanto o motor declara não ler essa flag e mantém indenização do suprimido. | sim | n/a | nenhuma | lote2.r:26 |
| Intervalo nem sempre subtraído | b | docs/LEIS.md:80; ponto/motor_calculo_v2.py:2773; ponto/motor_calculo_v2.py:2784 | A afirmação de que intervalo cadastrado sempre sai da jornada diverge do tratamento que preserva trabalho sem pausa reconhecida e indeniza o suprimido separadamente. | sim | n/a | nenhuma específica; BUG-HE-INTRA-DOBRADA em docs/BACKLOG.md:1081 é correção distinta do limite de HE | lote2.r:27 |
| Pós05 sem CLT declarada | b | docs/LEIS.md:83; core/regua_cct.py:453; ponto/motor_calculo_v2.py:758 | O fallback do construtor pode excluir prorrogação pós-05h em 12x36 sem CLT declarada, contrariando o “somente” documental. | sim | n/a | nenhuma | lote2.r:29 |
| Flag em produção sem prova | b | docs/LEIS.md:84; core/regua_cct.py:150 | A constante é falsa na cópia, mas a afirmação sobre seu valor efetivo em produção não está demonstrada. | sim | n/a | nenhuma | lote2.r:30 |
| Tipo gravado decide fallback | b | docs/LEIS.md:119; ponto/motor_calculo_v2.py:573; ponto/turnos.py:1380 | Sem alimentação útil da ata, o pareador pode usar `Batida.tipo`, contrariando a exclusão universal desse campo. | sim | n/a | nenhuma específica; COL369-TRES-RESTANTES em docs/BACKLOG.md:1118 é um caso associado, sem equivalência causal demonstrada | lote2.r:32 |
| Rótulo manda no dispatch | b | docs/LEIS.md:124; ponto/motor_calculo_v2.py:3635; ponto/motor_calculo_v2.py:3668 | O dispatch continua usando `tipo_base` e só aplica correção geométrica a certos rótulos de partido. | sim | n/a | O136, docs/BACKLOG.md:1346 | lote2.r:33 |
| DiaPago substitui passado | b | docs/LEIS.md:113; ponto/services/fechamento.py:1211; ponto/services/dia_pago.py:357; ponto/services/dia_pago.py:358 | Lavraturas sucessivas em `versao='motor'` apagam e recriam as linhas anteriores apesar da promessa de preservar passado em nova versão. | sim | n/a | nenhuma específica; O196 em docs/BACKLOG.md:1407 reconhece delete/recreate para reversão, mas não cobre preservação de versões históricas | lote2.r:34 |
| versao_motor já preenchida | b | docs/LEIS.md:170; ponto/services/fechamento.py:1214; ponto/services/dia_pago.py:345 | A nota de que ninguém preenche `versao_motor` diverge do fechamento que passa o carimbo e do escritor que o grava. | nao | n/a | nenhuma específica; GRAVADO-NAO-CARIMBA-A-REGUA em docs/BACKLOG.md:1586 trata FechamentoMensal sem carimbo, outro sítio | lote2.r:35 |
| Resíduo arredondamento teto | b | docs/LEIS.md:111; ponto/motor_calculo_v2.py:1194; ponto/motor_calculo_v2.py:1196 | O arredondamento separado de atraso e antecipada pode produzir soma ligeiramente superior ao teto aritmético. | sim | n/a | nenhuma | lote2.r:36 |

Duplicatas adicionais com outros lotes, para consolidação central: retenção universal → lote6.r:01; exportada intocável → lote4.r:11/lote6.r:04; regeneração bloqueada → lote4.r:12/lote6.r:05; TXT não garante aprovação → lote7.r:03; recálculo versus guarda09 → lote7.r:02; congelamento não universal → lote5.r:B1; falta inteira720 → lote3.r:15/lote7.r:05; rótulo manda no dispatch → lote4.r:13. Estas equivalências são derivadas dos anexos. [nao verificado]

## 05/11 17:45 · sessao 218d96043d83 · - · - · origem=Ronald (tmux)

Conferi em leitura os anexos 18–20. Todas as referências abaixo existem; a referência documental de `O261-b` mudou de `BACKLOG.md:2098` para `:2115` **(movida)**. Nenhum trecho ficou ausente no HEAD.

`B:NN` abaixo significa `app/docs/BACKLOG.md:NN`.

| origem | classe | arquivo:linha conferido | afirmação consolidável | dinheiro | competência | cobertura conferida |
|---|---|---|---|---|---|---|
| 18 | defeito | `app/folha/export.py:840` | Qualquer demissão não nula exclui do TXT mensal, inclusive quando posterior à janela examinada. | sim | 10/2026 no exemplo | H12-DEMISSAO-NA-JANELA — B:1741 |
| 18 | defeito | `app/chamados/services/materializacao.py:519`; `app/ponto/models.py:875` | A materialização de `FALTA_DIA` informa 720 minutos, impedindo a derivação pela jornada quando os minutos já vieram preenchidos. | sim | n/a | H06-FALTA-PELA-GRADE — B:1742 |
| 18 | defeito | `app/ponto/services/dia_pago.py:190`; `:328` | A rubrica do calculador pode sobrescrever o atraso já reduzido pelo crédito diário com o atraso bruto. | sim | n/a | V3-LAVRAR-ATRASO-CREDITO — B:858 |
| 18 | defeito | `app/ponto/motor_calculo_v2.py:440`; `:110` | O recorte isolado da HE pode perder a prorrogação noturna reconhecida para o turno original. | sim | n/a | V3-HE-NOTURNA-POR-PECA — B:933 |
| 18 | defeito | `app/ponto/motor_calculo_v2.py:94`; `:110`; `:116` | A função noturna pode sobrepor faixas em recortes longos e retorna zero acima de 26 horas. | sim | n/a | V3-NOTURNO-RECORTE-LONGO — B:939 |
| 18 | defeito | `app/ponto/motor_calculo_v2.py:3095`; `:3323`; `:3337`; `:3352` | Os ramos Comercial e TurnoPartido podem atribuir HE100 de feriado sem recompor seu componente noturno antes do `continue`. | sim | n/a; BACKLOG documenta 10/2026 | V3-MOTOR-FERIADO-HE100-NOTURNA — B:937 |
| 18 | defeito | `app/ponto/calculador/alimentacao.py:390`; `:404`; `:528` | O calculador resolve flags CCT na data inicial da janela e reutiliza-as nos dias seguintes. | sim | 11/2026 no exemplo | V3-RETOMA-FILA, item 3 — B:943 |
| 18 | lacuna | `app/ponto/models.py:399`; `app/folha/export.py:668`; `:698` | O fechamento não possui estado `encerrado`, cuja introdução isolada poderia divergir entre classificação e montagem do TXT. | sim | n/a | STATUS-ENCERRADO-DA-COMPETENCIA-NAO-EXISTE — B:2130 |
| 18 | divergência | `app/chamados/services/materializacao.py:78`; `app/chamados/juizes.py:446`; `app/ponto/turnos.py:2632` | Uma ausência parcial sem período pode calar o materializador enquanto o juiz da célula mantém o furo. | sim | n/a | COBPARC-AUSENCIA-CALA-DOIS-JUIZES — B:769 |
| 18 | divergência | `app/escala/models.py:1280`; `:1309`; `app/ponto/precedencia.py:237` | O gerador consulta folgas do próprio vínculo enquanto o juiz aceita folga manual da pessoa vinculada a contrato encerrado. | sim | n/a | GRADE-FOLGA-GERADOR — B:936 |
| 18 | divergência | `app/chamados/management/commands/auditar_invariantes_chamados.py:1531`; `:1541`; `app/ponto/precedencia.py:237` | O auditor C5 pode acusar trabalho correto ao considerar folga de calendário de vínculo encerrado que o juiz rejeita. | não | n/a | GRADE-FOLGA-C5 — B:935 |
| 18 | defeito | `app/escala/services/fase_conflitante.py:71`; `:80`; `app/escala/alimentacao.py:228` | O medidor de fase aplica o vínculo ativo a todos os dias da janela sem escolher o vínculo vigente em cada data. | não | 10–11/2026 no exemplo | FASE-CONFLITANTE-VINCULO-DO-DIA — B:812 |
| 18 | lacuna | `app/ponto/admin.py:18`; `app/ponto/services/justificativa.py:199`; `:214`; `app/ponto/signals.py:161` | A aprovação de justificativa pelo Admin contorna a porta de encerramento da origem, invalidação do fechamento e log de domínio, embora permaneça o signal do dia. | sim | n/a | JUSTIFICATIVA-ADMIN-FORA-DA-PORTA — B:781 |
| 18 | risco | `app/core/constants.py:13`; `app/ponto/services/cartorio.py:1140`; `:1229`; `:1419` | Leituras diretas da flag de isenção coexistem com o juiz declarado, criando risco de divergência caso apenas a função seja alterada. | sim | n/a | ISENTO-PONTO-JUIZ-REGISTRADO — B:920 |
| 18 | lacuna | `app/colaboradores/models.py:294`; `app/core/constants.py:18`; `app/ponto/services/cartorio.py:520` | A isenção não possui vigência e a flag atual pode alterar o insumo de rejulgamento de dias anteriores sob a guarda de versão existente. | sim | 10/2026 no exemplo | ISENTO-FLAG-ATEMPORAL — B:921 |
| 18 | risco | `app/docs/BACKLOG.md:744` | O relato documental registra julgamento diferente da mesma célula entre sombra e produção, sem insumos fornecidos para determinar causa ou estado atual. | sim | 10/2026 | SOMBRA-X-PROD-COL988-124639 — B:744 |
| 18 | risco | `app/docs/BACKLOG.md:764` | O relato de atas defasadas contém medidas contestadas e não permite confirmar quantidade afetada ou causa atual. | sim | 09/2026 | ATA-DEFASADA-COMP09 — B:764 |
| 18 | risco | `app/docs/BACKLOG.md:1618` | O relato O327 registra cobertura incompleta do oráculo por campos sem dono, sem evidência fornecida para confirmar o residual atual. | sim | 10/2026 | O327 — B:1618 |
| 18 | risco | `app/docs/BACKLOG.md:1573` | O relato registra carimbo `celula_absolveu` com célula atualmente acusando, sem comparação temporal que determine passivo e dono. | não | n/a | CARIMBO-QUE-A-CELULA-DESMENTE — B:1573 |
| 18 | risco | `app/docs/BACKLOG.md:2115` **(movida)** | O lote documental de perguntas absolvidas por idade não tem execução, reavaliação e residual atual demonstrados pelos dados fornecidos. | não | 08–09/2026 no relato | O261-b — B:2115 |
| 19 | divergência | `app/escala/utils.py:325`; `app/ponto/services/cartorio.py:677`; `app/escala/services/leitor_celula.py:121`; `app/ponto/juiz_batida.py:86`; `app/ponto/motor_calculo_v2.py:596`; `app/ponto/turnos.py:1653` | A representação `HH:MM` descarta precisão e identidade, levando leitor e juiz a reconstruir instantes e motor e pareador a resolver colisões por minuto de formas distintas. | sim | n/a | nenhuma cobertura específica localizada; O118 em B:1194 documenta colisão textual semelhante, mas não esta obra |
| 19 | risco | `app/ponto/services/espelho.py:267`; `:614`; `:783` | O espelho continua derivando realizado, turno ausente e estado aberto, derivações que o mapa por marco proposto não substituiria sozinho. | sim | n/a | O-PROJECOES — B:733; PROGRAMA-CELULA-E-O-DIA — B:787 |
| 19 | risco | `app/colaboradores/services/calendario.py:718`; `app/ponto/services/dia_decidido.py:179` | O calendário combina minutos da ata com ausência de turno para derivar `ata_sem_turno`. | sim | n/a | O-PROJECOES — B:733; PROGRAMA-CELULA-E-O-DIA — B:787 |
| 19 | risco | `app/ponto/services/espelho.py:762`; `app/colaboradores/services/calendario.py:378`; `app/relatorios/dia_da_frota.py:20` | Espelho, calendário e relatório da frota extraem separadamente os marcos faltantes. | sim | n/a | PROGRAMA-CELULA-E-O-DIA — B:787 |
| 19 | risco | `app/folha/export.py:1124`; `:1127` | A exportação continua derivando retenção por furo somente de pausa dos papéis devolvidos pelo juiz. | sim | n/a | nenhuma específica; não confundir com O254 em B:1442, que registra outra alteração da retenção |
| 19 | risco | `app/ponto/services/cartorio.py:570` | A montagem documenta dependência da ata da passada anterior para obter o papel da batida. | sim | n/a | nenhuma específica localizada |
| 19 | lacuna | `app/ponto/motor_calculo_v2.py:559` | A alimentação autoritativa dos períodos declara cobertura incompleta para turno partido. | sim | n/a | nenhuma específica localizada |
| 20 | risco | `app/core/tests/marca_cron15.py:24`; `app/core/management/commands/crons.py:97`; `app/config/crons_duracao.json:78` | Mantidos prazo e duração da cópia, a exceção de cron termina após 13/11 e o render de instalação volta a propagar `CronNaoCabe`. | não | n/a; data 14/11/2026 | CRON-NAO-CABE-SOMBRA-87-MIN — B:747; F3-PORTA-E-CRON-PRAZO — B:1739 |
| 20 | defeito | `app/ferias/services.py:399`; `app/ponto/services/ausencia.py:1165`; `:1195` | A remarcação comum limpa somente o intervalo novo e pode conservar ausências de férias do intervalo anterior. | sim | 11–12/2026 no exemplo | nenhuma |
| 20 | defeito | `app/holerite/motor.py:323`; `:344`; `:467` | Um recibo publicado pode impedir a importação posterior de outro tipo para o mesmo colaborador e competência. | não | 11/2026 no exemplo | nenhuma |
| 20 | risco | `app/folha/management/commands/pre_fechamento.py:89`; `:91`; `app/pautas/models.py:57` | Após a virada da competência, o pré-fechamento deixa de atualizar a pauta anterior ainda viva. | não | 11–12/2026 no exemplo | nenhuma |
| 20 | defeito | `app/config/crons.py:644`; `app/ponto/management/commands/mapa_divergencia.py:93`; `app/ponto/janelas.py:27` | O cron do mapa envia competência civil e deixa de examinar os primeiros dias da competência seguinte após o corte. | não | 11–12/2026 no exemplo | nenhuma |
| 20 | lacuna | `app/core/feriados.py:27`; `app/ponto/precedencia.py:386` | Sem a linha de feriado cadastrada, a tabela de importação não impede que o juiz conserve o dia como trabalho. | sim | 11/2026 no exemplo | nenhuma |
| 20 | lacuna | `app/core/datas_cravadas.py:25`; `:86`; `app/core/management/commands/crons.py:92` | O censo de datas exclui testes embora um comando de produção importe deles o prazo CRON-15. | não | n/a | nenhuma |

Contagens para consolidar:

- **18:** 20 achados; **22 afirmações resolvidas/conformes** fora da lista: P5; FUROS-LEITOR-PARCIAL; FASE-DONO-AS-OF; FECH-01 a FECH-19. São 15 “AINDA VALE” mais cinco riscos documentais na tabela de origem.
- **19:** 7 itens consolidados, provenientes de 10 manifestações de achado: primeiros quatro da tabela fundidos em um, quatro derivações mantidas como riscos e duas lacunas/riscos da prosa. **Quatro blocos sem achado:** campos já existentes; chamadas à mesma autoridade não equivalem a cópias da lei; limite de cadastro; limite de batida. Se a contagem “por anexo” for anterior à deduplicação, registrar **10**, explicando a fusão.
- **20:** 7 achados, seis numerados mais lacuna do censo na prosa. **Dois blocos sem achado claro:** literal da vigência CCT é comentário e não vence no recorte; funções puras tratam a virada dezembro/janeiro. A nota de férias coletivas não afirma defeito nem conformidade jurídica; apenas ausência de evidência.

Total desta fatia após fusão: **34 itens**, sendo **26 dinheiro=sim**, **0 não verificáveis** e **12 sem cobertura**, usando as coberturas amplas explicitamente indicadas acima. Distribuição: **11 defeitos, 7 lacunas, 4 divergências, 12 riscos**. Confira a classificação/contagem contra as linhas antes de usar: a tabela é a fonte prevalente.

## 05/11 17:45 · sessao 1295076d2e5b · - · - · origem=Ronald (tmux)

No grafo F3 examinado, não encontrei bloqueante introduzido pela resolução: `beco_em_lote` consulta C6/C17 e monta a assinatura; o escritor de reata é função separada, chamada apenas quando explicitamente invocada (`app/escala/services/cadastro_realidade.py:417`, `:421`, `:459`; `app/chamados/services/limbo_folgas.py:224`, `:243`).

- `agora` é armazenado no construtor sem leitura de estado e encaminhado na carga preguiçosa (`app/chamados/services/vigia_fio.py:199`, `:201`, `:216`). A lista o passa a C11 e à conciliação dos donos (`app/escala/services/cadastro_realidade.py:717`, `:787`, `:801`); C11 o entrega a `InsumosDoDono` (`:454`). O fallback do espelho recebe o mesmo instante (`app/ponto/management/commands/e6_oraculo.py:159`).
- C11 exige `folga_nao_definida` (`app/escala/services/cadastro_realidade.py:451`); com esse insumo, o juiz devolve CADASTRO (`app/ponto/management/commands/e6_oraculo.py:98`). `InsumosDoDono.dono` retorna antes de solicitar espelho nessa condição (`:175`, `:180`, `:182`).
- `alimentar_portas` consulta fila, verbos e perguntas, montando dicts (`app/chamados/juizes.py:1287`, `:1290`, `:1300`, `:1304`); `_no_app` chama `perguntas_do_app` (`:1319`), que apenas consulta e filtra por C6 (`app/chamados/services/pergunta_no_app.py:46`, `:53`).
- O pai LIMBO é consultado por `pais_limbo_vivos`, sem reatar perguntas (`app/chamados/services/limbo_folgas.py:212`, `:217`, `:221`). A reata usa outra função e a porta `reapontar_resolvedora` (`:224`, `:243`).
- Os escritores F3 de célula e trilha pertencem a `regenerar_celulas_vinculo`: criação e alteração da célula (`app/ponto/portas/celula.py:253`, `:270`), trilha do rejulgamento do mês (`:301`) e rejulgamento pela porta do cartório (`:402`). Esse caminho não é chamado pela composição de C11 apresentada (`app/escala/services/cadastro_realidade.py:396`, `:469`).
- O SIM consulta vínculos atuais de célula e chamado, portanto sua monotonicidade não deve ser justificada apenas por “a trilha cresce” (`app/chamados/juizes.py:1076`, `:1081`, `:1083`). Não identifiquei escritor F3 desses vínculos invocado pelo lote; afirmar imutabilidade geral desses vínculos seria [não verificado].

Ressalva obrigatória sobre relógio: passar `agora` não uniformiza todos os leitores transitivos do censo. A jornada C2 lê `date.today()` (`app/escala/services/cadastro_realidade.py:207`, `:728`); o calendário chamado pelo censo lê seu próprio `timezone.now()` (`app/chamados/services/vigia_fio.py:270`; `app/colaboradores/services/calendario.py:132`, `:163`, `:250`); C17 consulta os relógios da fila e da partição de verbos (`app/chamados/juizes.py:1287`, `:1290`; `app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:35`). São leituras independentes do memo da tranca; classificá-las como regressão introduzida pelo hunk exigiria histórico não disponível na cópia [não verificado].

APROVA quanto à interação F3 × lote examinada; bloqueantes identificados: nenhum (`app/chamados/management/commands/vigiar_fio_morto.py:167`, `:171`, `:177`).

## 05/11 17:44 · sessao 482d7ed4e7b6 · - · - · origem=Ronald (tmux)

- `ListaDoAdmin.__init__` só armazena argumentos, inicializa caches e um `set`; nenhuma leitura C6/tranca/toque ocorre ali. A carga acontece em `_carregar`, chamada pelos predicados; ela passa `agora` a `cadastro_realidade.lista`. Portanto construir antes do `with` não antecipa a leitura. (`app/chamados/services/vigia_fio.py:199-218`, `:222-251`)
- O lote é um `ContextVar` com `set({})`, `yield` e `reset`, reentrante; não adquire trava de banco nem abre transação. Não cria ordem nova de locks. (`app/chamados/juizes.py:990-1003`)
- `_memo_da_tranca` devolve o dict do lote quando habilitado; fora dele usa request ou instância do colaborador. Empresa e período fechado usam explicitamente `lote=False`, logo não são compartilhados entre colaboradores pelo novo lote. (`app/chamados/juizes.py:966-979`, `:937-948`)
- Quem escreve o dict do lote: `tranca_em_lote` cria o dict; `_toque_humano_no_dia` escreve `('sistema',): pk`; `_admin_tocou_o_dia_depois` escreve somente `True` na chave `('toque', colab.pk, data, desde)`. Quem lê: `_memo_da_tranca`, o teste `memo.get(k)` e o filtro que exclui o usuário de sistema. O `False` nunca é armazenado. (`app/chamados/juizes.py:999`, `:1043-1049`, `:1068-1072`)
- A chave SIM inclui `desde`, recebido de `pf.fechado_em`; tranca com outro timestamp não reutiliza esse SIM. (`app/chamados/juizes.py:954`, `:1044`)
- A leitura do toque compõe trilha e vínculos atuais: cobrar pelo colaborador/data; resolver pelas células atuais; desarquivar pelos chamados atualmente ligados às células. Assim, “trilha só cresce” não prova sozinho paridade universal diante de alterações concorrentes desses vínculos. A ocorrência dessas alterações em produção está [não verificado]. (`app/chamados/juizes.py:1071-1084`)
- Há uma escrita condicional indireta: `get_user_sistema()` usa `User.objects.get_or_create`, embora sua docstring diga que o usuário deve existir no deploy. Isso pode criar **User** se ausente, e não constitui escrita F3 de célula/chamado/trilha; existência real do usuário está [não verificado]. (`app/chamados/juizes.py:1068-1070`, `app/core/utils.py:12-18`)
- Os leitores adicionais do `with` consultam e contam: `viva_em_disputa_fechada` chama C6; `respondida_sem_resposta` lê texto; `materializada_viva` conta listas produzidas por C6 e pelos juízes da competência. (`app/chamados/services/vigia_fio.py:746-759`, `:782-786`, `:803-819`, `:847-851`)
- O C11 F3 usa `alimentar_portas` e `porta_alcancavel_do_dia`; não reimplementa C6/C17 nesse caminho. (`app/escala/services/cadastro_realidade.py:417-423`, `app/chamados/juizes.py:1300-1306`, `:1375-1387`)
- A projeção de célula é cópia rasa em memória, sem `save`, e devolve a instância original sem insumos de julgamento; não é segundo escritor persistente. (`app/chamados/juizes.py:383-397`, `app/ponto/portas/celula.py:705-721`)
- Existem testes de paridade lote/avulso, NAO→SIM, mudança de vínculo, reabertura/tranca e instalação/limpeza do lote pelo comando. Resultado da execução nesta auditoria: [não verificado]. (`app/chamados/tests/test_snc2_toque_em_lote.py:113-164`, `:171-213`)

**Conclusão do subescopo: APROVA, sem bloqueante concreto de interação F3 × memo no hunk.** Construção lazy e instalação do lote estão combinadas no comando. (`app/chamados/services/vigia_fio.py:199-218`, `app/chamados/management/commands/vigiar_fio_morto.py:167-177`)

## 05/11 17:44 · sessao 76317a1e3b44 · - · - · origem=Ronald (tmux)

Evidência estática dos testes, sem execução: [nao verificado]

- Equivalência lote/avulso: matriz de perguntas vivas e mortas, incluindo toque humano/sistema, toque anterior e posterior, célula e chamado de outro colaborador — `app/chamados/tests/test_snc2_toque_em_lote.py:113-135`.
- Frescor do NÃO: toque escrito após primeira consulta é visto na próxima; chamado desarquivado que ganha célula também é relido — `app/chamados/tests/test_snc2_toque_em_lote.py:137-164`.
- Empresa/tranca fora do lote: testes cobrem reabrir e trancar entre colaboradores diferentes — `app/chamados/tests/test_snc2_toque_em_lote.py:171-198`.
- Costura do comando: teste verifica `censo` dentro do contexto e contexto limpo ao sair — `app/chamados/tests/test_snc2_toque_em_lote.py:200-213`. Esse teste intercepta somente `censo`, não cada uma das cinco chamadas.
- Relógio F3: teste força relógio da máquina em outra competência e passa `agora`; outro verifica repasse por `ListaDoAdmin`; o teste AST exige keyword `agora` no comando — `app/escala/tests/test_f3_beco1_folga_na_lista.py:196-230`.
- Uma lista compartilhada e falha distinta por dia: `app/chamados/tests/test_o261e_f5_alcance.py:227-248`.
- Leitura final da falha, inclusive dia presente somente em alcance: `app/chamados/tests/test_o261e_f5_alcance.py:263-293`.

Pontos para a consolidação:

- `ListaDoAdmin.__init__` só armazena argumentos e inicializa campos; a chamada real está em `_carregar`, acionada pelos métodos de consulta — `app/chamados/services/vigia_fio.py:199-218`, `:222-255`.
- Não afirmar zero escrita universal: o memo chama `get_user_sistema()` e essa função usa `get_or_create`, portanto pode inserir `auth_user` se ausente — `app/chamados/juizes.py:1068-1070`, `app/core/utils.py:18`. Existência desse usuário em produção: `[não verificado]`.
- Falha do carregador chama observabilidade que altera contadores em memória e envia logging; esse corpo não grava `LogAuditoria` — `app/chamados/services/vigia_fio.py:219-220`, `app/core/observ.py:73-82`.
- O helper `tranca_em_lote` só instala/reseta `ContextVar`; não abre transação nem adquire trava de banco — `app/chamados/juizes.py:993-1003`.
- As autoridades F3 usadas no C11 estão compostas por chamadas ao juiz existente: `alimentar_portas`/`porta_alcancavel_do_dia`, `chamado_que_lista`, `cadastro_do_dia`, `InsumosDoDono` — `app/escala/services/cadastro_realidade.py:396-405`, `:417-459`. Nenhuma alteração de autoridade aparece no hunk em `app/chamados/management/commands/vigiar_fio_morto.py:167-177`.

Nenhum bloqueante identificado por esta revisão dos testes e da costura estática; execução dos testes e resultado de produção: `[não verificado]`. [nao verificado]

_1901 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
