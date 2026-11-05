# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 18:39** · publicado em **05/11/2026 18:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 18:39 · sessao c49dc6236336 · - · - · origem=Ronald (tmux)

Referências confirmadas nesta cópia, sem uso de banco ou logs:

**Leis e diferença do censo anterior**

- O aval vigente está localmente em `app/docs/PROMPTS.md:1875`: abertura de `batida_ausente` + cinco atos; a enumeração literal e a retirada dos vivos legítimos antes do RED estão em `app/docs/PROMPTS.md:1882`; a autorização de diff textual do pré-código está em `app/docs/PROMPTS.md:1886`.
- C11 operacional é o nome da saída da exceção do disparador em `app/docs/CORTES.md:305`. `_C11` do registro vivo responde outra pergunta — silêncio por ausência — em `app/core/juizes.py:117`, autoridade em `app/core/juizes.py:271`. Não declarar equivalência técnica.
- Nenhum juiz novo é necessário para saber se uma pergunta morreu: C6 está registrado em `app/core/juizes.py:266`, com cálculo em `app/chamados/juizes.py:1135`; C15 responde `any` da coleção em `app/chamados/juizes.py:1233`, registro `app/core/juizes.py:275`; C17 responde alcance, registro `app/core/juizes.py:277`.
- Regra do evento e lâmpada: `CLAUDE.md:166`–`170`; um juiz/escritor e idempotência: `CLAUDE.md:175`; censo completo antes de filtro: `CLAUDE.md:193`; casos pela regra antes do código: L-110, `app/docs/LEIS.md:136`; sítio redundante sai no mesmo ato: L-111, `app/docs/LEIS.md:137`.
- Corte estrutural manda separar chamado vivo de cobrança viva e veda fallback/juiz novo: `app/docs/CORTES.md:253`. Chamado nasce da célula, morre por lastro, nunca sem dono: `app/docs/ESTRUTURAL.md:46`.

**Casos A fundamentáveis pela regra**

| Cadastro + estado + ato | Resposta exigida e fonte |
|---|---|
| Ativo, vínculo do dia válido, ata com marco vencido faltante, competência aberta; abrir `batida_ausente`. | Pergunta correspondente nasce no evento, lendo a dívida da célula; `app/docs/PROMPTS.md:1875`, `CLAUDE.md:168`. |
| Mesmo fato; duas aberturas concorrentes. | Mesmo estado, uma trilha e zero duplicata; `CLAUDE.md:175`. |
| Chamado encerrado, dívida persiste, nenhuma pergunta ressuscitável; `reabrir`. | Vivo sozinho não quita o contrato: persiste pergunta viva ou ação administrativa nomeada; `app/docs/CORTES.md:253`, `app/docs/PROMPTS.md:1882`. |
| A perde sua última resolvedora por `retratar_duplicata` ou por `reapontar` para B. | Preservar um fio alcançável para o fato, sem exigir uma nova pergunta por chamado duplicado; `CLAUDE.md:175`, `app/docs/CORTES.md:253`, `app/docs/CORTES.md:404`. |
| Pergunta respondida discordante, sem decisão humana; retratar como duplicata. | Preservar resposta para decisão humana; lei HX-ADMIN-DECIDE-O-DIA expressa em `app/chamados/juizes.py:1151`, exceção de concordância em `app/chamados/juizes.py:1159`. |
| Saída e intervalo faltantes; validar/materializar só saída. | Marco resolvido não é perguntado de novo; intervalo devido conserva fio/ação; regra por marco em `app/chamados/juizes.py:1173`, lâmpada/fio em `app/docs/CORTES.md:253`. |
| Validação encontra recusa do plantio, fato continua devido. | Recusa não produz quitação fictícia nem retira a decisão humana; corte `app/docs/CORTES.md:253`; porta documenta devolução em `app/chamados/services/validacao.py:413`. |
| Célula/batida/tranca muda sem `save` de PerguntaDisputa; pergunta antes viva passa a morta. | C6 responde vida; não inventar juiz nem escrita em função leitora. Evento precisa cobrir o escritor da dependência que mudou; `app/core/juizes.py:266`, `CLAUDE.md:168`, `app/docs/PROMPTS.md:1882`. |
| Corte cadastrado 21; fatos 21/09, 20/10, 21/10. | Primeiros dois são competência 10; terceiro é 11. Tranca da 10 não cala 21/10; L-090 `app/docs/LEIS.md:123`, autoridade `app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`. |
| Turno 22h–06h do dia anterior; observar 00h01 e 06h10. | Meia-noite não encerra fato; saída ainda não venceu com margem de 30 minutos. `CLAUDE.md:185`, `app/ponto/services/cartorio.py:828`, encadeamento `app/ponto/services/cartorio.py:845`, lei explicitada `app/chamados/services/fabrica_da_celula.py:18`. |
| Marco às 18h, margem 30 min; 18h29m59 e 18h30. | Antes não cobra; no instante de vencimento pode cobrar o marco faltante; `CLAUDE.md:185`, `app/ponto/services/cartorio.py:846`. |
| Novo vínculo ativo hoje; célula de ontem nomeia antigo ainda dentro da vigência. | Antigo responde pelo dia; fora da vigência retorna CADASTRO/None, sem substituição silenciosa; `app/escala/alimentacao.py:231`, `app/escala/alimentacao.py:272`. |
| Escala vigente, célula sem ata; tentar emissão de marco. | Abstém e acende falta de ata; não fabricar marco. `app/chamados/services/disputa_emissao.py:819`, `app/chamados/services/disputa_emissao.py:833`. |
| `trabalha=None`, sem ata que diga outro tipo de dia. | Não converter desconhecido em folga; limbo cobra conforme autoridade, mas pergunta de marco ainda exige ata; `app/docs/CORTES.md:416`, `app/core/juizes.py:137`, `app/chamados/services/disputa_emissao.py:819`. |
| Parcial aprovada sem período; dívida restante. | Lista administrativa CADASTRO; não julga dentro/fora, não fabrica falta/pergunta; `app/docs/CORTES.md:319`, `app/chamados/services/disputa_emissao.py:847`. |
| `registrado` vivo sem ação; ausência aberta suspensiva; parcial na lista administrativa. | Retirar os legítimos do universo dos 494 antes de RED, sem “curar” criando pergunta artificial; `app/docs/PROMPTS.md:1882`, `app/chamados/catalogo/motor.py:24`, `app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/disputa_emissao.py:856`. |
| Competência trancada, depois destrancada ou tocada pelo admin. | Tranca cala por leitura, sem quitação do fato; destranca/toque humano devolve vida conforme L-051; `app/chamados/juizes.py:919`, `app/chamados/juizes.py:951`. |
| Competência exportada, célula `trabalha=True`; outro controle limbo `trabalha=None`. | Não aplicar guarda universal “exportada não pergunta”: trabalho continua perguntando pelo O44; limbo exportado segue Pauta DP. `app/chamados/services/fabrica_da_celula.py:167`. |
| Passado sem fato novo; introduzir a obra. | Não backfill de todo estoque automaticamente: horizonte para frente e passado FOTO até fato novo; L-124 `app/docs/LEIS.md:145`. |

**Contraexemplos relevantes para C/D**

- `post_save` de pergunta sozinho não cobre morte por leitura: tranca é leitura de `periodo_trancado`, sem carimbo/estado novo, e destranca devolve vida sozinha; `app/chamados/juizes.py:919`. Célula muda vida em `app/chamados/juizes.py:1181`; resposta concordante com luz pode morrer sem novo carimbo em `app/chamados/juizes.py:1159`.
- Adicionar escrita dentro de C6 torna leitor escritor e permite side effects em app/folha/vigia; C6 é explicitamente função de leitura na política de exceção `app/chamados/juizes.py:1210` e usado por `perguntas_do_app` em `app/chamados/services/pergunta_no_app.py:42`.
- Um diff que sempre crie pergunta após perda pode recriar saída já resolvida quando o dia ainda acusa intervalo: marco vem antes do dia, `app/chamados/juizes.py:1173`.
- Um diff que use qualquer pergunta morta como prova de quitação erra: célula acusadora pode manter vida apesar de carimbo, `app/chamados/juizes.py:1181`; validação/tranca/identidade têm precedências próprias em `app/chamados/juizes.py:1135`.
- Guarda universal de exportado erra o caso O44: `app/chamados/services/fabrica_da_celula.py:167`; portanto o antigo pd não pode ser copiado nesse ponto.
- O denominador 494 não veio com registros; distribuição por ato, legítimos e incidências concretas são **[não verificado]**. O aval exige retirá-los antes do RED em `app/docs/PROMPTS.md:1882`; não é possível inventar quantos sobram.
- Mudanças de empresa/corte durante vigia e reabertura durante request afetam frescor da tranca; memo global da rodada não pode congelar empresa/período. Motivo e correção documentados em `app/chamados/juizes.py:982`, guarda só SIM do toque em `app/chamados/juizes.py:1033`.

**Leitores adicionais que precisam aparecer no censo**

- App: `app/chamados/services/pergunta_no_app.py:42`.
- Retenção/classificação de causa: `app/chamados/services/pergunta_no_app.py:65`; medição usa COBRANCA e ativos em `app/chamados/services/pergunta_no_app.py:119`.
- Fábrica: `app/chamados/services/fabrica_da_celula.py:74`, exclusão por identidade histórica em `app/chamados/services/fabrica_da_celula.py:118`.
- Emissão por evento/célula: `app/chamados/services/fabrica_da_celula.py:145`.
- Alcance depende de pergunta no app e âncora na fila, não só C15: `app/core/juizes.py:132`; corte manda folha/raio-X/vigia lerem C17 em `app/docs/CORTES.md:404`.
- Tranca depende dos três toques humanos: Cobrar, Resolver dia, desarquivar; resposta do colaborador e cron não contam. `app/chamados/juizes.py:957`.

## 05/11 18:36 · sessao 7cce7722cbad · PCCMP-FIO-MUDO · censo · origem=Code

HASH LIDO = 9c0fd9d3e90f166cba697ec840422b5585a8ab09

Uso **pc1** e **pc2** para identificar os anexos transcritos, com suas seções e casos. Numeração física das linhas dos anexos: **[não verificado]**. Os **494** são o agregado declarado no aval; sua composição individual e a partição entre defeitos e vivos legítimos permanecem **[não verificado]**. (`app/docs/PROMPTS.md:1875`; `app/docs/PROMPTS.md:1882`)

**1 COINCIDE**

**Casos equivalentes**

| pc1 | pc2 | Coincidência conferida na cópia |
|---|---|---|
| A1/A2 | A01/A02 | Nascimento por evento e idempotência como requisito: mesmo estado, uma trilha, nenhuma duplicata. (`CLAUDE.md:168`; `CLAUDE.md:175`) |
| A3 | A03 | Reabrir o chamado não demonstra recuperação do fio: grava estado e ressuscita perguntas existentes; a ressuscitação pode retornar zero. (`app/chamados/models.py:1382`; `app/chamados/models.py:1391`; `app/chamados/models.py:1482`) |
| A4 | A05 | Duplicata não exige uma pergunta nova por chamado; importa conservar a cobrança alcançável do fato. (`app/chamados/models.py:2388`; `app/chamados/juizes.py:1344`) |
| A5 | A06 | Carimbo de identidade não encerra necessariamente resposta discordante pendente: a resposta é examinada antes da via de identidade. (`app/chamados/models.py:2617`; `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1171`) |
| A7/A8 | A07/A08 | Resolver um marco não resolve outro; a devolução de validação recusada preserva a resposta. (`app/chamados/juizes.py:1173`; `app/chamados/services/validacao.py:171`) |
| A9/A10 | A10/A11 | Reapontamento precisa preservar alcance; a costura dispõe de guarda contra destino terminal e trava sua linha durante a operação transacional. (`app/chamados/juizes.py:1344`; `app/chamados/services/fio.py:752`; `app/chamados/services/fio.py:837`) |
| A11 | A12 | C6 já julga morte por leitura: consulta tranca, estado da célula, ata e janela. (`app/core/juizes.py:266`; `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1195`; `app/chamados/juizes.py:1197`) |
| A13/A14 | A15/A14 | Vencimento por instante, incluindo igualdade e turno que cruza meia-noite. O comparador é `<=`; a saída das 06h ainda não venceu às 06h10 com margem de 30 minutos. (`app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:846`; `app/chamados/services/fabrica_da_celula.py:18`) |
| A15 | A13 | Janela de competência depende do corte cadastrado da empresa. (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:54`) |
| A18 | A16 | Vínculo do dia considera a geradora e sua vigência; não se substitui automaticamente pelo vínculo ativo atual. (`app/escala/alimentacao.py:231`; `app/escala/alimentacao.py:269`; `app/escala/alimentacao.py:272`) |
| A19/A20 | A17/A19 | Sem célula/ata possível para escala vigente, o questionário espera; parcial administrativa pode permanecer sem pergunta. (`app/chamados/services/disputa_emissao.py:833`; `app/chamados/services/disputa_emissao.py:842`; `app/chamados/services/disputa_emissao.py:856`; `app/chamados/services/fabrica_da_celula.py:102`) |
| A21 | A09/A20/A21 | Registro, decisão disponível, ausência aberta e dono efetivamente na lista precisam ser separados do defeito. Estado `em_analise` sozinho não constitui ação nomeada. (`app/chamados/services/painel.py:174`; `app/chamados/services/vigia_fio.py:283`; `app/chamados/services/vigia_fio.py:287`; `app/chamados/services/vigia_fio.py:292`) |
| A22/A23, parte histórica | A23 | Hoje fica fora do contador alarmado; passado sem fato novo não autoriza backfill geral. Isso não impede emissão de marco já vencido por instante. (`app/chamados/services/vigia_fio.py:420`; `app/chamados/services/fabrica_da_celula.py:8`; `app/docs/LEIS.md:145`) |

**Sítios e autoridades**

| Item presente nos dois | Conferência |
|---|---|
| C6, C15 e C17; distinção do C11 | O registro aponta C6 para vitalidade, C15 para coleção, C17 para alcance e C11 para ausência. (`app/core/juizes.py:266`; `app/core/juizes.py:271`; `app/core/juizes.py:275`; `app/core/juizes.py:277`) |
| `ChamadoColaborador.abrir` | Trava colaborador, pode devolver existente/renascido e só chega ao evento no nascimento real. `MODULOS_DE_TURNO` não contém `batida_ausente`. (`app/chamados/models.py:866`; `app/chamados/models.py:885`; `app/chamados/models.py:933`; `app/chamados/models.py:939`; `app/chamados/reconciliador.py:336`) |
| `ChamadoColaborador.criar` | O corpo termina em criação, trilha e retorno, sem emissão explícita equivalente. (`app/chamados/models.py:1048`; `app/chamados/models.py:1049`; `app/chamados/models.py:1050`) |
| Detector, grade e reemissão C2 | Detector tenta recuperar fio e absorve falha; grade abre e retorna; C2 abre e reaponta. (`app/ponto/management/commands/detectar_ausencias.py:790`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/reconciliar_grade.py:531`; `app/chamados/reconciliador.py:879`; `app/chamados/reconciliador.py:922`) |
| Sinal indireto | Julgamento está sob `engolir` e pode retornar durante julgamento já em curso. (`app/chamados/signals.py:172`; `app/chamados/signals.py:174`) |
| Duplicata e transferência | Retratação chama `carimbar_identidade`; reapontamento escreve destino, lavra e salva. Esses corpos não reconciliam explicitamente a origem que perdeu a resolvedora. (`app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:895`; `app/chamados/services/fio.py:845`; `app/chamados/services/fio.py:849`) |
| Validação | Assinatura, materialização individual e devolução ficam no `atomic`; materialização da disputa inteira vem depois. (`app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:424`; `app/chamados/services/validacao.py:425`; `app/chamados/services/validacao.py:430`) |
| Identidade histórica na fábrica | A seleção exclui duplicatas da consulta histórica, reúne os motivos já existentes e os retira dos motivos novos, sem consultar C6 nessa seleção. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:119`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`) |
| Reapontamento dentro de `PerguntaDisputa.abrir` | Está nos dois: pc1 na matriz de campos; pc2 como sítio irmão. A identidade existente pode receber célula e destino. (`app/chamados/models.py:2424`; `app/chamados/models.py:2427`; `app/chamados/models.py:2439`) |
| Cartório e consumidor | Cartório chama fábrica para datas julgadas naquela passada. Consumidor atual retrata chamados e encerra órfãs, sem chamada à fábrica nesse corpo. (`app/ponto/services/cartorio.py:996`; `app/ponto/services/cartorio.py:997`; `app/chamados/reconciliador.py:1326`; `app/chamados/reconciliador.py:1329`) |
| Varredura ainda escritora | Comando chama `perguntar` sob `--apply`; agenda usa `--apply`; registro mantém o ramo FIO MUDO como julgamento por varredura. (`app/chamados/management/commands/disparar_perguntas_competencia.py:280`; `app/chamados/management/commands/disparar_perguntas_competencia.py:287`; `app/config/crons.py:1495`; `app/config/crons.py:2007`) |

**Trecho de diff comum**

Ambos propõem a mesma substituição da condição de nascimento; a condição antiga está exatamente em `app/chamados/models.py:939`:

```diff
- if (_mo or '') in _MT and ch.celula_id is not None:
+ if ((_mo or '') in _MT or _mo == 'batida_ausente') and ch.celula_id is not None:
```

O trecho conserva a emissão pela porta existente e fica dentro do `atomic` de `abrir`. (`app/chamados/models.py:863`; `app/chamados/models.py:940`; `app/ponto/portas/celula.py:664`)

**Contraexemplos comuns**

- Existente reutilizado, reabertura sem candidata e nascimento por `criar` escapam do hunk condicionado a `criado`. (`app/chamados/models.py:885`; `app/chamados/models.py:933`; `app/chamados/models.py:1048`; `app/chamados/models.py:1482`)
- O hunk não instrumenta duplicata, validação nem origem de transferência. (`app/chamados/services/fio.py:891`; `app/chamados/services/validacao.py:425`; `app/chamados/services/fio.py:845`)
- Chamar a fábrica não demonstra recuperação da identidade histórica. (`app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/models.py:2420`)
- Morte por leitura não depende exclusivamente de escrita da pergunta; lavra de materialização e C6 respondem perguntas distintas. (`app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1197`; `app/chamados/veredito_lavrado.py:219`)
- Acrescentar efeito ao getter mistura leitura memoizada com entrega; o memo não comprova processamento transacional da obrigação. (`app/chamados/juizes.py:822`; `app/chamados/juizes.py:835`; `app/chamados/juizes.py:847`)
- Nenhum candidato demonstra a cobertura exigida para aposentar a varredura: o mandato inclui abertura e cinco atos; o comando continua escritor. (`app/docs/PROMPTS.md:1882`; `app/chamados/management/commands/disparar_perguntas_competencia.py:280`)

Os dois também nomeiam leitores de app/web/API, relatórios, calendário, fechamento, folha e vigias. Esses consumos aparecem efetivamente na cópia. (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/views.py:1238`; `app/api/views.py:2627`; `app/chamados/relatorios_disputa.py:18`; `app/colaboradores/services/calendario.py:468`; `app/chamados/models.py:1783`; `app/folha/fora_da_folha.py:67`; `app/chamados/services/vigia_fio.py:635`)

**2 DIVERGE**

**D1 — Consumidor passa a acionar perguntas: DESENHO.**

- **pc1 C:** acrescenta `perguntar_pela_celula` para cada célula encontrada pelos `celula_id` do lote, depois do encerramento de órfãs.
- **pc2 C:** propõe apenas o evento de nascimento e declara que o consumidor atual não entrega a pergunta.
- **Código aberto:** em `app/chamados/reconciliador.py:1326` há `retratar_chamados_absolvidos_por_celula`; em `app/chamados/reconciliador.py:1329`, `encerrar_orfas_pela_lampada`; em `app/chamados/reconciliador.py:1332`, o cálculo de adiamento; depois vem o tratamento de exceção, em `app/chamados/reconciliador.py:1333`.

A diferença muda **quem aciona a escrita de perguntas e seu universo**: o consumidor lê eventos pendentes e os agrupa por colaborador, sem recorte por nascimento de `batida_ausente`. Ambos reprovam seus candidatos; não divergem afirmando que um deles satisfaz a obra. (`app/chamados/reconciliador.py:1309`; `app/chamados/reconciliador.py:1315`; `app/chamados/services/fabrica_da_celula.py:175`)

**D2 — Competência exportada: DESENHO na formulação da regra.**

- **pc1 A23:** manda conservar o “gravado exportado” e encaminhar a diferença, citando L-092. Essa linha realmente contém “depois do export, diferença não se aplica”. (`app/docs/LEIS.md:125`)
- **pc2 A24:** explicita que L-113 revogou a recusa geral de recalcular e a Pauta DP como pré-condição, preservando TXT anterior e trilha. (`app/docs/LEIS.md:149`)
- **Código aberto:** declara “COMPETENCIA EXPORTADA NAO BARRA MAIS” e conserva versão anterior do TXT e trilha. (`app/ponto/services/fechamento.py:476`; `app/ponto/services/fechamento.py:484`)

Se “gravado” em pc1 significa congelar o cálculo, a formulação contraria a alteração registrada e o comportamento documentado na função. Se significa somente preservar o TXT anterior, coincide com pc2; essa delimitação não está explícita em pc1. O tratamento particular do limbo exportado continua na fábrica e não equivale à antiga proibição geral. (`app/docs/LEIS.md:149`; `app/ponto/services/fechamento.py:479`; `app/chamados/services/fabrica_da_celula.py:167`)

**D3 — Premissa do nascimento: detalhe de especificação.**

Pc1 A1 diz “sem pergunta”; pc2 A01 exige “sem identidade histórica”. A segunda formulação é mais restrita: ausência de pergunta viva não significa ausência de identidade. Entretanto, pc1 D também reconhece essa barreira histórica; não há incompatibilidade global demonstrada entre os anexos nesse ponto. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/models.py:2388`)

**D4 — Separação dos vivos legítimos: detalhe de exposição.**

Pc1 reúne condições em A21; pc2 distribui entre A09/A20/A21. “Viva sem porta”, destacado em pc2 A22, também está conceitualmente em pc1 B/D. A cópia distingue vitalidade, alcance e dono fora do fio; não foi demonstrada uma escolha incompatível de universo entre essas apresentações. (`app/chamados/juizes.py:1344`; `app/chamados/juizes.py:1367`; `app/chamados/services/vigia_fio.py:274`)

As divergências por **omissão** estão discriminadas abaixo, com sua classificação e o sítio conferido.

**3 SÓ NUMA**

“Só numa” abaixo significa ausência daquele item explícito na outra resposta; não presume que a outra defenda a regra contrária.

| Origem | Item omitido na outra | Código aberto e classificação |
|---|---|---|
| **pc1 A6** | Caso separado de validação integral, sem dívida residual | O fechamento pergunta a premissa; marco atendido pode encerrar sua cobrança. **Detalhe de cobertura de casos.** (`app/chamados/models.py:681`; `app/chamados/juizes.py:1173`) |
| **pc1 A12** | Caso separado de resposta concordante com a luz | C6 mantém pendência quando a comparação não demonstra concordância. **Detalhe de cobertura de casos.** (`app/chamados/juizes.py:1096`; `app/chamados/juizes.py:1162`) |
| **pc1 A17** | Toque humano posterior à tranca como cenário próprio | Há consulta à trilha para atos humanos nomeados, excluindo usuário de sistema. **Detalhe de cobertura de casos.** (`app/chamados/juizes.py:1054`; `app/chamados/juizes.py:1071`) |
| **pc2 A04** | Caso separado de reabertura recusada | Guardas devolvem `admin_sem_fato_novo` e `premissa_regularizada`. **Detalhe de cobertura de casos.** (`app/chamados/models.py:1292`; `app/chamados/models.py:1297`; `app/chamados/models.py:1300`) |
| **pc2 A18** | Passado sem escala/marcos como cenário próprio | A fábrica preserva esse ramo e pode consultar classificação pelas batidas. **Detalhe de cobertura de casos.** (`app/chamados/services/fabrica_da_celula.py:21`; `app/chamados/services/fabrica_da_celula.py:52`; `app/chamados/services/fabrica_da_celula.py:55`) |
| **pc1 B** | Inventário explícito das quatro portas de contexto e duas de silêncio | `anotar`, `renovar`, `retirar`, `fundir`, `silenciar` e `levantar`. **DESENHO: cobertura dos escritores.** (`app/chamados/services/contexto.py:62`; `app/chamados/services/contexto.py:81`; `app/chamados/services/contexto.py:106`; `app/chamados/services/contexto.py:128`; `app/chamados/services/arquivo.py:141`; `app/chamados/services/arquivo.py:176`) |
| **pc1 B** | Mapa nominal C1/C2/C3/C16/C18 e precedência do registro vivo sobre lookup | As autoridades estão registradas; `JUIZES.md` declara seu papel de lookup. **Detalhe do mapa de autoridades existentes.** (`app/core/juizes.py:261`; `app/core/juizes.py:263`; `app/core/juizes.py:276`; `app/core/juizes.py:278`; `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`) |
| **pc2 B** | C12/C13 nominalmente identificados | Registro aponta exclusão da fábrica e retenção. **Detalhe do mapa de autoridades existentes.** (`app/core/juizes.py:272`; `app/core/juizes.py:273`) |
| **pc1 B/D** | Fonte específica da tranca e comando/agenda stale | Há serviço de tranca e execução própria de lavra vencida. **DESENHO: fontes de acionamento e distinção de julgamento.** (`app/chamados/services/competencia_trancada.py:178`; `app/chamados/services/competencia_trancada.py:276`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:219`; `app/config/crons.py:390`) |
| **pc1 A/B** | Advertência expressa de que C17 exclui “Resolver dia”, aceito separadamente pelo vigia | A exclusão está escrita no juiz; o vigia consulta a decisão oferecida. **DESENHO: universos distintos de alcance e dono.** (`app/chamados/juizes.py:1367`; `app/chamados/services/vigia_fio.py:283`) |
| **pc2 B** | Critério de escolha de `canonica_do_grupo` | Prioriza resposta; depois, menor identificador. **Detalhe do dedup existente.** (`app/chamados/services/fio.py:870`; `app/chamados/services/fio.py:873`; `app/chamados/services/fio.py:875`) |
| **pc2 B** | `relavrar` individualizado como escritor de via | A função escreve via e salva junto com a lavra. **DESENHO: cobertura dos escritores.** (`app/chamados/services/relavra_via.py:104`; `app/chamados/services/relavra_via.py:123`; `app/chamados/services/relavra_via.py:125`) |
| **pc1 D** | Agrupamento decidido depois de abrir | Detector decide agrupamento sob limbo após receber o chamado. **DESENHO: ordem entre abertura, agrupamento e emissão.** (`app/ponto/management/commands/detectar_ausencias.py:745`; `app/ponto/management/commands/detectar_ausencias.py:758`; `app/ponto/management/commands/detectar_ausencias.py:761`) |
| **pc1 C/D** | Laço consumidor→fábrica e alerta sobre eventos de outras origens | O lote não distingue esse nascimento dos demais eventos. **DESENHO: acionador e universo**, conforme D1. (`app/chamados/reconciliador.py:1284`; `app/chamados/reconciliador.py:1309`) |
| **pc1 D** | Célula ausente como bloqueio explícito do candidato | A condição exige `ch.celula_id is not None`. **DESENHO: universo coberto pelo evento.** (`app/chamados/models.py:939`) |
| **pc1 D** | Exceção permissiva de leitura da ata explicitamente localizada | A exceção retorna `True, None`; a fábrica pode entrar na classificação pelas batidas quando `vencidos=None`. **DESENHO: autoridade usada após falha.** (`app/chamados/services/disputa_emissao.py:828`; `app/chamados/services/disputa_emissao.py:832`; `app/chamados/services/fabrica_da_celula.py:52`) |
| **pc1 D** | Ausência de ata e relógio na chave do memo explicitamente demonstrada | A chave não os contém, mas o cálculo lê ata e janela; chave igual permite devolver resultado guardado. **DESENHO: frescor da leitura.** Ocorrência nos 494: **[não verificado]**. (`app/chamados/juizes.py:811`; `app/chamados/juizes.py:837`; `app/chamados/juizes.py:1195`; `app/chamados/juizes.py:1197`) |
| **pc2 D10** | Risco de inversão ao emitir sincronamente dentro de `abrir` | Ordem declarada: Disputa→Colaborador; `abrir` já trava colaborador; fábrica chama agregação de disputa. **DESENHO: ordem de trava.** (`app/chamados/models.py:843`; `app/chamados/models.py:866`; `app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/services/disputa_emissao.py:598`) |
| **pc2 D12** | “Forçar julgamento” pode não chegar à fábrica | `julgar_celula` retorna sem célula ou com turno atual ainda em curso; fábrica depende das datas julgadas. **DESENHO: cobertura do acionamento.** (`app/ponto/services/cartorio.py:1976`; `app/ponto/services/cartorio.py:1982`; `app/ponto/services/cartorio.py:1986`; `app/ponto/services/cartorio.py:996`) |
| **pc2 D, complemento** | Duas pausas e pergunta de conversa sem identidade de fato | Duas pausas podem tornar comparação indeterminada; conversa segue ramo sem dedup do fato. **Detalhe de cobertura de casos.** (`app/chamados/juizes.py:1108`; `app/chamados/models.py:2392`; `app/chamados/models.py:2413`; `app/chamados/models.py:2417`) |
| **pc2 D, complemento** | Evento cuja célula foi regenerada | `celula_id` é inteiro sem FK para o evento sobreviver à regeneração. **DESENHO: identidade e universo do consumidor.** (`app/escala/models.py:1853`; `app/escala/models.py:1854`) |
| **pc2 B/D** | Falha da lista administrativa explicitamente tratada | Falha de leitura da lista não autoriza excluir o dia do alarme. **DESENHO: universo de exclusão.** (`app/chamados/services/vigia_fio.py:219`) |
| **pc2 D, complemento** | Resposta nova concorrente com validação como aceite separado | O aval nomeia essa intercalação; resposta nova pode limpar assinatura. **DESENHO: concorrência entre escritores.** (`app/docs/PROMPTS.md:1882`; `app/chamados/services/disputa_emissao.py:1295`) |
| **pc2 B/D** | Exceção formal do selo e condição de saída | A exceção exige medida do fio mudo e pouso do C11 operacional. **Detalhe de verificação da aposentadoria.** (`bin/tests/test_papel_prazo_nao_deriva.sh:303`

[truncado em 20000 de 23754 caracteres]

## 05/11 18:33 · sessao d8df42718d53 · - · - · origem=Ronald (tmux)

Conferência de B: não identifiquei incompatibilidade entre as duas descrições dos sítios centrais; ambas descrevem as mesmas portas e limitações. O acréscimo efetivo no consumidor existe apenas no candidato de pc1, matéria de desenho, pois o consumidor atual termina a reconciliação sem chamar a fábrica (`app/chamados/reconciliador.py:1326`, `app/chamados/reconciliador.py:1329`, `app/chamados/reconciliador.py:1332`).

Coincidências conferidas:

- Registro: C6=`pergunta_viva`, C15=`fio_vivo`, C17=`porta_alcancavel_do_dia`; C11 é ausência, não FIO MUDO (`app/core/juizes.py:266`, `app/core/juizes.py:271`, `app/core/juizes.py:275`, `app/core/juizes.py:277`).
- `abrir` trava colaborador, pode retornar existente/renascido e só emite evento no nascimento real, condicionado a `MODULOS_DE_TURNO`; essa tupla não contém `batida_ausente` (`app/chamados/models.py:866`, `app/chamados/models.py:882`, `app/chamados/models.py:885`, `app/chamados/models.py:933`, `app/chamados/models.py:939`, `app/chamados/reconciliador.py:336`).
- `criar` termina com criação, trilha e retorno, sem emissão explícita nesse corpo (`app/chamados/models.py:1048`, `app/chamados/models.py:1049`, `app/chamados/models.py:1050`).
- Detector tenta fio em `try/except`; grade termina sem emissão direta; C2 usa `abrir` e reapontamento (`app/ponto/management/commands/detectar_ausencias.py:790`, `app/ponto/management/commands/detectar_ausencias.py:801`, `app/ponto/management/commands/detectar_ausencias.py:802`, `app/ponto/management/commands/reconciliar_grade.py:507`, `app/ponto/management/commands/reconciliar_grade.py:531`, `app/chamados/reconciliador.py:879`, `app/chamados/reconciliador.py:922`).
- Sinal do chamado pode retornar durante julgamento e está sob `engolir` (`app/chamados/signals.py:172`, `app/chamados/signals.py:174`).
- Reabertura grava vivo e ressuscita existentes; filtro exige pergunta histórica materializada e pode resultar em zero (`app/chamados/models.py:1382`, `app/chamados/models.py:1391`, `app/chamados/models.py:1423`, `app/chamados/models.py:1482`).
- Duplicata usa `carimbar_identidade` e retorna; reapontamento escreve destino/lavra/save e retorna sem tratar origem nesse corpo (`app/chamados/services/fio.py:891`, `app/chamados/services/fio.py:895`, `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:846`, `app/chamados/services/fio.py:849`).
- Validação individual e devolução estão em `atomic`; materialização da disputa ocorre depois (`app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:422`, `app/chamados/services/validacao.py:425`, `app/chamados/services/validacao.py:430`).
- Fábrica exclui identidade histórica não duplicata, independentemente de C6 (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`).
- Cartório chama fábrica somente para datas presentes no conjunto julgado naquela passada (`app/ponto/services/cartorio.py:986`, `app/ponto/services/cartorio.py:995`, `app/ponto/services/cartorio.py:996`, `app/ponto/services/cartorio.py:997`).
- Cron está configurado com `--apply`, permanece na lista de julgamento por varredura; instrumento já possui `--alarme` sem escrita (`app/config/crons.py:1495`, `app/config/crons.py:2007`, `app/config/crons.py:2011`, `app/chamados/management/commands/disparar_perguntas_competencia.py:251`).

Exclusivos globais relevantes de pc1:

- Matriz formal de campos com escritores/leitores, incluindo quatro portas de contexto e duas de silêncio, ausentes como inventário em pc2 (`app/chamados/services/contexto.py:62`, `app/chamados/services/contexto.py:81`, `app/chamados/services/contexto.py:106`, `app/chamados/services/contexto.py:128`, `app/chamados/services/arquivo.py:141`, `app/chamados/services/arquivo.py:176`).
- Registro explícito de C1/C2/C3/C16/C18 e regra do lookup versus registro vivo (`app/core/juizes.py:261`, `app/core/juizes.py:262`, `app/core/juizes.py:263`, `app/core/juizes.py:276`, `app/core/juizes.py:278`, `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`).
- Tratamento específico da tranca por serviço e gancho no ato (`app/chamados/services/competencia_trancada.py:178`, `app/chamados/services/competencia_trancada.py:184`, `app/chamados/services/competencia_trancada.py:276`, `app/chamados/services/competencia_trancada.py:282`).
- Comando e agenda da relavra stale, distinguindo seu juiz de materialização de C6 (`app/chamados/management/commands/lavrar_perguntas_stale.py:58`, `app/chamados/management/commands/lavrar_perguntas_stale.py:87`, `app/chamados/veredito_lavrado.py:219`, `app/config/crons.py:390`).
- Exclusão explícita de “Resolver dia” de C17, embora o vigia aceite essa ação separadamente (`app/chamados/juizes.py:1367`, `app/chamados/juizes.py:1369`, `app/chamados/services/vigia_fio.py:283`).

Exclusivos globais relevantes de pc2:

- C12/C13 nomeados e registrados como autoridades de alcance da fábrica/retenção (`app/core/juizes.py:119`, `app/core/juizes.py:120`, `app/core/juizes.py:272`, `app/core/juizes.py:273`).
- `canonica_do_grupo` prioriza resposta, depois menor identificador (`app/chamados/services/fio.py:867`, `app/chamados/services/fio.py:873`, `app/chamados/services/fio.py:874`, `app/chamados/services/fio.py:875`).
- `relavrar` como escritor adicional de via; a definição está em 104 e escrita real em 123–126 (`app/chamados/services/relavra_via.py:104`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:124`, `app/chamados/services/relavra_via.py:125`).
- Chamadores irmãos individualizados: dedup, migração, declaração textual, limbo, reata, foto/API, ações e classe A (`app/chamados/management/commands/dedup_perguntas_fato.py:88`, `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:947`, `app/chamados/services/declaracao_texto.py:320`, `app/chamados/services/limbo_folgas.py:243`, `app/chamados/services/limbo_folgas.py:276`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:122`, `app/api/foto_recebida.py:51`, `app/chamados/services/acoes_disputa.py:375`, `app/chamados/management/commands/validar_classe_a.py:100`).
- Retorno antecipado de `julgar_celula` por ausência de célula ou turno atual ainda em curso (`app/ponto/services/cartorio.py:1976`, `app/ponto/services/cartorio.py:1977`, `app/ponto/services/cartorio.py:1982`, `app/ponto/services/cartorio.py:1986`).
- Falha da lista administrativa não exclui dia e contador textual tem consumidor no placar (`app/chamados/services/vigia_fio.py:195`, `app/chamados/services/vigia_fio.py:219`, `app/chamados/services/vigia_fio.py:225`, `app/chamados/management/commands/disparar_perguntas_competencia.py:235`, `bin/placar_code.sh:381`).
- Exceção formal do selo e condição de saída para cron (`bin/tests/test_papel_prazo_nao_deriva.sh:303`, `bin/tests/test_papel_prazo_nao_deriva.sh:306`, `bin/tests/test_papel_prazo_nao_deriva.sh:307`, `app/docs/CORTES.md:305`).

Cuidados para consolidação:

- Reapontamento irmão **não é SO NUMA global**: pc1 já o inclui na matriz de campos e pc2 o individualiza como sítio; o corpo realmente escreve célula e destino da identidade existente (`app/chamados/models.py:2424`, `app/chamados/models.py:2427`, `app/chamados/models.py:2439`, `app/chamados/models.py:2463`).
- Risco de ordem de trava de pc2 é de desenho; a ordem declarada é Disputa→Colaborador e `abrir` já segura colaborador (`app/chamados/models.py:843`, `app/chamados/models.py:845`, `app/chamados/models.py:866`).
- Universo do ramo é qualquer módulo vivo com célula na janela, sem filtro de `batida_ausente`; pc2 o explicita em D13, pc1 cita a consulta no censo B, portanto diferenciar explicitação de omissão completa (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`).

Citações falsas de B: nenhuma materialmente falsa identificada nos sítios conferidos; conferência literal de todas as referências periféricas de leitores **[não verificado]**. Referências a início de função não devem ser rotuladas falsas quando o corpo sustenta a afirmação: pc1 cita `materializacao.py:728`, cuja reconciliação está em 738 e aplicação a destino/clones em 740–748; pc2 cita `relavra_via.py:104`, cuja escrita está em 123–126 (`app/chamados/services/materializacao.py:728`, `app/chamados/services/materializacao.py:738`, `app/chamados/services/materializacao.py:740`, `app/chamados/services/materializacao.py:748`, `app/chamados/services/relavra_via.py:104`, `app/chamados/services/relavra_via.py:123`).

## 05/11 18:32 · sessao f4be53cc4ea6 · - · - · origem=Ronald (tmux)

Casos equivalentes conferidos:

| pc1 | pc2 | Sustentação na cópia |
|---|---|---|
| A1/A2 | A01/A02 | Evento e idempotência: `CLAUDE.md:168`, `CLAUDE.md:175`. |
| A3 | A03 | Reabertura não equivale a ação nomeada: `app/chamados/services/vigia_fio.py:283`. |
| A4/A5 | A05/A06 | Duplicata preserva identidade; resposta discordante pode continuar viva: `app/chamados/models.py:2617`, `app/chamados/juizes.py:1150`. |
| A7/A8 | A07/A08 | Marco atendido não quita outro; devolução preserva resposta: `app/chamados/juizes.py:1173`, `app/chamados/services/validacao.py:171`. |
| A9/A10 | A10/A11 | Alcance exige porta: `app/chamados/juizes.py:1344`. |
| A11 | A12 | Morte por leitura existente em C6: `app/chamados/juizes.py:1135`, `app/chamados/juizes.py:1140`, `app/chamados/juizes.py:1195`. |
| A13/A14 | A15/A14 | Igualdade vence (`<=`); turno noturno: `app/ponto/services/cartorio.py:846`, `app/chamados/services/fabrica_da_celula.py:18`. |
| A15/A18 | A13/A16 | Janela por empresa; geradora dentro da vigência: `app/ponto/janelas.py:54`, `app/escala/alimentacao.py:272`. |
| A19/A20 | A17/A19 | Ausência de ata; parcial administrativa: `app/chamados/services/disputa_emissao.py:842`, `app/chamados/services/fabrica_da_celula.py:101`. |
| A21 | A09/A20/A21 | Registro não pede ação; ausência/decisão/lista são baldes legítimos: `app/chamados/services/painel.py:174`, `app/chamados/services/vigia_fio.py:283`. |
| A22/A23 histórico | A23 | Hoje fora do alarme; passado até fato novo: `app/chamados/services/vigia_fio.py:420`, `app/docs/LEIS.md:145`. |

Divergência de **DESENHO**: pc1 A23 cita congelamento do gravado exportado (`app/docs/LEIS.md:125`); pc2 A24 cita revogação dessa recusa geral, preservando TXT anterior e trilha (`app/docs/LEIS.md:149`). A cópia declara a queda da recusa geral em `app/ponto/services/fechamento.py:477`, conserva TXT/trilha em `app/ponto/services/fechamento.py:484` e registra exportada por aviso em `app/ponto/services/fechamento.py:532`; ainda há guarda específica em `app/ponto/services/fechamento.py:552`. A fábrica mantém tratamento particular do limbo exportado e permite perguntar em dia de trabalho exportado (`app/chamados/services/fabrica_da_celula.py:167`).

Somente como casos explicitamente discriminados:

- **pc1:** validação integral A6 (`app/chamados/models.py:681`); resposta concordante com a luz A12 (`app/chamados/juizes.py:1090`, `app/chamados/juizes.py:1159`); toque humano posterior à tranca A17 (`app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1071`).
- **pc2:** recusa de reabertura A04, implementada por decisão humana sem fato novo e premissa regularizada (`app/chamados/models.py:1292`, `app/chamados/models.py:1298`); ramo passado sem escala A18 (`app/chamados/services/fabrica_da_celula.py:21`, `app/chamados/services/fabrica_da_celula.py:52`).

O caso pc2 A22 “viva sem porta” também aparece conceitualmente em pc1 B/D; C17 distingue vitalidade e alcance e exclui expressamente “Resolver dia” (`app/chamados/juizes.py:1344`, `app/chamados/juizes.py:1367`). Ausência aberta, registro e decisão legítima estão agrupados em pc1 A21 e separados em pc2; isso é detalhe de exposição (`app/chamados/services/vigia_fio.py:283`, `app/chamados/services/vigia_fio.py:287`, `app/chamados/services/painel.py:174`).

Não classificar `app/docs/LEIS.md:125` como citação falsa: a linha sustenta literalmente o congelamento, mas foi alterada pela regra de `app/docs/LEIS.md:149`. A divergência é normativa e material; falsidade da localização não foi demonstrada. As outras referências da seção A examinadas sustentam seus núcleos, com a ressalva de que `app/docs/LEIS.md:65` trata de escritor/trilha e não basta, isoladamente, para provar preservação da decisão humana (`app/docs/LEIS.md:65`, `app/chamados/models.py:1292`).

## 05/11 18:32 · sessao 775a4b15b644 · - · - · origem=Ronald (tmux)

**Conferência C/D, somente leitura:**

**Coincidem**

- Mesmo hunk de nascimento em `ChamadoColaborador.abrir`: acrescentar `batida_ausente` ao conjunto autorizado a emitir evento; o código atual condiciona o evento a `criado` e a `MODULOS_DE_TURNO`. (`app/chamados/models.py:933`; `app/chamados/models.py:938`; `app/chamados/models.py:939`)
- Os dois reprovam seus candidatos: reabertura pode retornar antes do hunk; o dedup devolve o existente em `return existente, False`; a outra porta `PerguntaDisputa.abrir` reaponta identidade existente e retorna sem nascimento. (`app/chamados/models.py:885`; `app/chamados/models.py:2420`; `app/chamados/models.py:2439`; `app/chamados/models.py:2464`)
- Duplicata e reapontamento exigem considerar a origem: `retratar_duplicata` apenas chama `carimbar_identidade` e retorna; `reapontar_resolvedora` muda destino, lavra e salva, sem tratar explicitamente o chamado anterior. (`app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:895`; `app/chamados/services/fio.py:845`; `app/chamados/services/fio.py:849`)
- Chamar a fábrica não demonstra cura de identidade histórica: seleciona motivos históricos não duplicados e retira-os dos motivos novos. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`)
- Morte por leitura e lavra são perguntas diferentes: C6 lê tranca, ata e janela; `lavrar` usa `_pergunta_materializada_de_fato`, não C6. (`app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1195`; `app/chamados/juizes.py:1197`; `app/chamados/veredito_lavrado.py:219`)
- Não aposentar a varredura com esses candidatos: o comando ainda reúne todo módulo vivo com célula na janela e escreve por `perguntar` sob `--apply`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/management/commands/disparar_perguntas_competencia.py:280`; `app/chamados/management/commands/disparar_perguntas_competencia.py:287`)

**Divergência de desenho**

- **pc1 C** acrescenta consumidor → fábrica, para todos os `celula_id` do lote; **pc2 C** altera só a emissão do evento e deixa explícito que isso não entrega pergunta. O consumidor atual agrupa eventos pendentes por colaborador, chama retratação e encerramento de órfãs, depois marca processamento; não chama a fábrica nesse corpo. Assim os candidatos mudam quem aciona a escrita de pergunta e seu universo, embora ambos sejam expressamente reprovados. (`app/chamados/reconciliador.py:1309`; `app/chamados/reconciliador.py:1315`; `app/chamados/reconciliador.py:1326`; `app/chamados/reconciliador.py:1329`; `app/chamados/reconciliador.py:1346`)

**Só pc1 D**

- Agrupamento decidido depois da abertura: detector só examina defeito de folga e agrupa sob pai depois de receber o chamado. (`app/ponto/management/commands/detectar_ausencias.py:745`; `app/ponto/management/commands/detectar_ausencias.py:758`; `app/ponto/management/commands/detectar_ausencias.py:761`)
- Alargamento do consumidor para eventos de outras origens: o lote atual contém eventos pendentes sem recorte por nascimento de `batida_ausente`. (`app/chamados/reconciliador.py:1284`; `app/chamados/reconciliador.py:1309`)
- Célula ausente impede o novo evento de nascimento. (`app/chamados/models.py:939`)
- Leitura da ata tem exceção permissiva; com ausência de faltantes e `vencidos=None`, a fábrica recorre à classificação pelas batidas. (`app/chamados/services/disputa_emissao.py:828`; `app/chamados/services/disputa_emissao.py:832`; `app/chamados/services/fabrica_da_celula.py:52`; `app/chamados/services/fabrica_da_celula.py:55`)
- Chave memo de C6 não contém ata nem relógio, embora o cálculo os leia; reutilização da instância pode devolver memo se chave não mudou. (`app/chamados/juizes.py:811`; `app/chamados/juizes.py:837`; `app/chamados/juizes.py:1195`; `app/chamados/juizes.py:1197`)

**Só pc2 D**

- Emissão síncrona dentro de `abrir` exige examinar inversão de trava: `abrir` trava colaborador; a ordem declarada é disputa → colaborador; a fábrica chama agregação de disputa. (`app/chamados/models.py:843`; `app/chamados/models.py:866`; `app/chamados/services/fabrica_da_celula.py:139`)
- “Forçar julgamento” não garante passagem: hoje sem turno encerrado pode retornar antes de `julgar_colab`; a fábrica só roda para datas julgadas naquele ato. (`app/ponto/services/cartorio.py:1982`; `app/ponto/services/cartorio.py:1986`; `app/ponto/services/cartorio.py:996`)
- Duas pausas: comparação resposta/luz devolve indeterminação quando há mais de um par comparável. (`app/chamados/juizes.py:1108`)
- Pergunta de conversa sem identidade de fato: o gate distingue a entrada sem data e não executa o dedup do fato nesse ramo. (`app/chamados/models.py:2392`; `app/chamados/models.py:2413`; `app/chamados/models.py:2417`)
- Evento sobrevive à célula regenerada: `celula_id` é inteiro sem FK. (`app/escala/models.py:1853`; `app/escala/models.py:1854`)
- Falha da lista administrativa não exclui ninguém do alarme. (`app/chamados/services/vigia_fio.py:219`)
- Candidato de nascimento restrito a `batida_ausente` não demonstra cobertura dos demais módulos vivos do universo consultado. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`)

**Citação sem sustentação específica**

- **pc2 D05**, frase “acrescentar apenas evento no primeiro carimbo ainda permitiria observar intenção antes do desfecho”: as referências não demonstram essa consequência para outbox criado dentro da transação. A assinatura, materialização individual e devolução estão no mesmo `atomic`; o comentário declara que leitores `on_commit` veem o desfecho. A materialização da disputa inteira ocorre depois. Portanto existe insuficiência de cobertura, mas leitura prematura depende do tipo/posição do gancho e permanece **[não verificado]**. (`app/chamados/services/validacao.py:392`; `app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:396`; `app/chamados/services/validacao.py:424`; `app/chamados/services/validacao.py:425`; `app/chamados/services/validacao.py:430`)

## 05/11 18:29 · sessao 476cec01b106 · PC2-FIO-MUDO · censo · origem=Code

HASH LIDO = 9c0fd9d3e90f166cba697ec840422b5585a8ab09

## A CASOS PELA REGRA

O hash identifica a cópia informada; sua correspondência com um commit Git é **[não verificado]**. Os **494** são o agregado declarado no aval; cadastro individual, distribuição pelas cinco perdas e quantidade de exclusões legítimas são **[não verificado]**. O mandato exige abertura de `batida_ausente` mais **reabrir, retratar_duplicata, validação, morte por leitura e reapontar**, retirando os vivos legítimos antes do RED. (`app/docs/PROMPTS.md:1875`, `app/docs/PROMPTS.md:1882`)

Os casos abaixo são cenários normativos, não registros observados dos 494. A referência é a regra aplicada ao cadastro e às batidas; executar ou comparar com o código anterior não substitui essa referência. (`app/docs/LEIS.md:136`)

| Caso | Cadastro + estado + ato | Resposta pela regra |
|---|---|---|
| A01 — nascimento | Ativo, vínculo do dia válido, ata acusa marco vencido, sem identidade histórica; abrir `batida_ausente`. | Entregar a pergunta pelo evento de abertura, lendo a célula/ata. A varredura deve apenas detectar o que o evento deixou. (`app/docs/PROMPTS.md:1875`, `CLAUDE.md:167`, `CLAUDE.md:169`) |
| A02 — duas aberturas | Mesmo cadastro e fato de A01; duas transações abrem simultaneamente. | Mesmo estado final, uma trilha do fato e nenhuma pergunta duplicada. (`CLAUDE.md:175`) |
| A03 — reabrir sem candidata | Dia ainda devido, chamado encerrado, nenhuma pergunta restaurável; reabrir. | Reabrir o chamado não basta: a dívida conserva pergunta viva ou ação administrativa nomeada. (`app/docs/CORTES.md:253`) |
| A04 — reabertura recusada | Dia regularizado ou decisão humana ainda válida, sem fato novo; tentar reabrir automaticamente. | Preservar a decisão e recusar a reabertura; não produzir pergunta para preencher contador. (`app/chamados/models.py:1269`, `app/chamados/models.py:1327`, `app/docs/LEIS.md:65`) |
| A05 — retratar duplicata | A perde sua última resolvedora; a canônica do mesmo fato está em B. | Preservar o fio do fato pela canônica e sua porta alcançável. Não exigir uma pergunta para cada chamado duplicado. (`app/docs/CORTES.md:253`, `CLAUDE.md:175`, `app/chamados/juizes.py:1344`) |
| A06 — duplicata respondida | Duplicata tem resposta discordante, sem decisão humana; retratá-la. | Preservar a conversa para decisão. A via de identidade não autoriza apagar a resposta pendente. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`, `app/docs/LEIS.md:65`) |
| A07 — validação com resto | Saída e intervalo devidos; validar/materializar somente a saída. | Não perguntar novamente a saída atendida; conservar fio ou ação nomeada para o intervalo residual. (`app/chamados/juizes.py:1173`, `app/docs/CORTES.md:253`) |
| A08 — validação recusada | Resposta de hora; a materialização recusa sem resolver o fato. | A recusa não pode produzir quitação fictícia. A resposta e o motivo continuam disponíveis ao admin pela porta de devolução. (`app/chamados/services/validacao.py:155`, `app/chamados/services/validacao.py:171`, `app/docs/LEIS.md:65`) |
| A09 — decisão legítima | Pergunta do mesmo marco já decidida; chamado continua vivo sem pergunta. | Separar a ação administrativa efetivamente disponível da necessidade de perguntar. Não criar irmã só porque C6 devolve morta; excluir do universo indevido somente com dono/ação demonstrados. (`app/docs/PROMPTS.md:1882`, `app/chamados/services/vigia_fio.py:283`, `app/chamados/management/commands/disparar_perguntas_competencia.py:201`) |
| A10 — reapontar | A perde a última resolvedora para B; o dia continua devido. | Preservar a alcançabilidade do fato e tratar a origem que perdeu o fio; transferência não constitui dívida nova. (`app/docs/CORTES.md:253`, `app/chamados/juizes.py:1344`, `CLAUDE.md:175`) |
| A11 — costura concorrente | Reapontamento disputa com encerramento do destino. | A intercalação não pode deixar pergunta viva inacessível sob âncora terminal; preservar alcance ou recusar com motivo. (`app/chamados/services/fio.py:743`, `app/chamados/juizes.py:1344`, `CLAUDE.md:175`) |
| A12 — morte por leitura | Sem alteração do carimbo, uma leitura encontra tranca, marco atendido ou célula regularizada. | Consultar **C6 existente**. Morte da pergunta não significa automaticamente quitação do dia nem autorização para reemitir. (`app/core/juizes.py:266`, `app/chamados/juizes.py:1135`, `app/docs/CORTES.md:253`) |
| A13 — borda 21→20 | Empresa com corte 21; fatos de 20/10 e 21/10; trancar competência 10. | Competência 10 abrange 21/09–20/10; 21/10 pertence à 11. Ler o corte da empresa e a competência do fato, sem fronteira civil ou 21 universal. (`CLAUDE.md:81`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:36`) |
| A14 — cross-meia-noite | Turno 22h–06h ancorado ontem; observar 00h01 e 06h10. | Meia-noite não encerra a jornada. Com margem de 30 minutos, a saída das 06h ainda não venceu às 06h10. (`CLAUDE.md:185`, `app/chamados/services/fabrica_da_celula.py:18`) |
| A15 — instante exato | Marco 18h, margem vigente de 30 minutos; observar 18h29min59s e 18h30. | Não antecipar a cobrança; comparar o instante encadeado acrescido da margem. (`CLAUDE.md:185`, `app/ponto/services/cartorio.py:845`) |
| A16 — vínculo trocado | Célula gerada pelo vínculo antigo, dentro da vigência; outro vínculo está ativo hoje. | Vale a geradora do dia. Fora de sua vigência, o problema é CADASTRO; não substituir silenciosamente pelo vínculo atual. (`app/escala/alimentacao.py:231`, `app/escala/alimentacao.py:272`, `app/docs/LEIS.md:122`) |
| A17 — célula sem ata | Há escala vigente, mas falta ata; tentar perguntar por marco. | Não afirmar marco faltante sem a ata. Nomear a ausência de ata e aguardar a autoridade; não converter desconhecimento em folga. (`app/chamados/services/disputa_emissao.py:818`, `app/chamados/services/disputa_emissao.py:842`, `app/docs/LEIS.md:95`) |
| A18 — sem escala | Dia passado sem escala e sem marcos comparáveis; existem batidas. | Usar o ramo existente para esse cadastro. Ele não autoriza transformar falha de leitura da ata em fallback. (`app/chamados/services/fabrica_da_celula.py:34`, `app/chamados/services/disputa_emissao.py:833`, `app/docs/LEIS.md:136`) |
| A19 — parcial administrativa | Ausência parcial sem período, encaminhada à lista administrativa. | Conservar o dia na lista, sem pergunta indevida; retirar essa condição legítima antes do RED. (`app/chamados/services/fabrica_da_celula.py:101`, `app/chamados/services/pergunta_no_app.py:26`, `app/docs/PROMPTS.md:1882`) |
| A20 — ausência aberta | Ausência aberta cobre o dia; cobrança suspensa. | Preservar a decisão pendente de ausência; não fabricar pergunta para zerar agregado. (`app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/vigia_fio.py:287`, `app/docs/PROMPTS.md:1882`) |
| A21 — registro/silêncio | Chamado `registrado` ou com silêncio vigente, sem pergunta. | Existência viva não prova ação devida. Separar essas condições e verificar a premissa antes de classificá-las como defeito. (`app/chamados/services/painel.py:174`, `app/chamados/services/painel.py:186`, `app/docs/PROMPTS.md:1882`) |
| A22 — viva sem porta | C6 verdadeiro, mas pergunta fora do app ou sob âncora fora da fila. | Não declarar alcance apenas pela vitalidade. Aplicar C17: chamado da célula na fila ou pergunta viva no app com âncora na fila. (`app/chamados/juizes.py:1344`) |
| A23 — hoje/passado | Marco de hoje ainda não venceu; ou dia antigo sem fato novo. | Não antecipar cobrança/alarmar o ciclo legítimo. Obra nova respeita o horizonte; passado permanece foto até fato novo, sem backfill implícito. (`CLAUDE.md:185`, `app/chamados/services/vigia_fio.py:420`, `app/docs/LEIS.md:145`) |
| A24 — exportada | Fato novo em competência já exportada. | Preservar TXT anterior e trilha; não aplicar a antiga proibição absoluta de recalcular, alterada pela L-113. Isso não autoriza reparo indiscriminado dos 494. (`app/docs/LEIS.md:149`, `app/docs/LEIS.md:145`) |

“Falta de batida” não deve ser transformada automaticamente em “falta decidida”; nem esta obra autoriza fabricar batida. (`app/docs/LEIS.md:87`, `CLAUDE.md:140`)

## B SITIOS

**Juízes existentes:** morte por leitura está coberta por **C6**, coleção por **C15**, alcance por **C17**, exclusões/causas da fábrica por **C12/C13**. O **C11 registrado é ausência**, distinto do nome operacional “C11=(i)” do aval; não há fundamento para renomeá-lo como juiz de FIO MUDO. (`app/core/juizes.py:117`, `app/core/juizes.py:266`, `app/core/juizes.py:271`, `app/core/juizes.py:272`, `app/core/juizes.py:273`, `app/core/juizes.py:275`, `app/core/juizes.py:277`)

| Sítio/família | Escritor, leitor ou dependência que a obra precisa resolver |
|---|---|
| Nascimento comum | `ChamadoColaborador.abrir`: trava colaborador, deduplica, pode renascer e retornar antes do trecho de evento; nascimento real grava evento somente para `MODULOS_DE_TURNO`. (`app/chamados/models.py:863`, `app/chamados/models.py:882`, `app/chamados/models.py:932`, `app/chamados/models.py:938`) |
| Nascimento irmão | `ChamadoColaborador.criar`: cria e registra nascimento, sem evento explícito nesse corpo. Uma solução restrita a `abrir` precisa declarar essa irmã. (`app/chamados/models.py:1015`, `app/chamados/models.py:1048`) |
| Três emissores de `batida_ausente` | Detector, grade e reemissão C2 convergem em `abrir`; detector ainda tenta fio depois, grade retorna e C2 reaponta. (`app/ponto/management/commands/detectar_ausencias.py:693`, `app/ponto/management/commands/detectar_ausencias.py:787`, `app/ponto/management/commands/reconciliar_grade.py:507`, `app/ponto/management/commands/reconciliar_grade.py:531`, `app/chamados/reconciliador.py:879`, `app/chamados/reconciliador.py:922`) |
| Sinal indireto | `post_save` do chamado chama julgamento sob `engolir` e sai durante `em_julgamento`; não constitui entrega garantida da pergunta. (`app/chamados/signals.py:165`, `app/chamados/signals.py:172`, `app/chamados/signals.py:174`, `app/chamados/signals.py:180`) |
| Estado do chamado | `_gravar_estado` escreve `status_local`, `resolvido_em`, `via_encerramento` e campos associados; terminal tem guarda de perguntas vivas. Reabertura e fechamento devem continuar usando essa porta. (`app/chamados/models.py:195`, `app/chamados/models.py:215`, `app/chamados/models.py:222`, `app/chamados/models.py:274`) |
| Reabrir | `reabrir` grava vivo e ressuscita existentes; universo exige candidatas históricas e pode resultar em zero. Não cria pergunta inexistente. (`app/chamados/models.py:1312`, `app/chamados/models.py:1382`, `app/chamados/models.py:1391`, `app/chamados/models.py:1423`, `app/chamados/models.py:1482`) |
| Duplicata | `canonica_do_grupo` prioriza resposta; `retratar_duplicata` chama a porta de identidade e termina sem reconciliar o chamado que perdeu a pergunta. (`app/chamados/services/fio.py:867`, `app/chamados/services/fio.py:878`, `app/chamados/services/fio.py:891`) |
| Validação | `_carimbar_validacao` escreve assinatura; `devolver_validacao_recusada` desfaz assinatura preservando resposta. Validação e tentativa individual de plantio estão em `atomic`; materialização da disputa inteira ocorre depois. (`app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:155`, `app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:430`) |
| Outros escritores da assinatura | Resposta nova desfaz validação; correção em lote também limpa; ressuscitação passa por `descarimbar_em_lote`; `carimbar(resposta='validar')` pode preencher assinatura. Não basta instrumentar `validar_pergunta`. (`app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1493`, `app/chamados/models.py:2580`) |
| Carimbo da pergunta | `_escrever_carimbo`, `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote` controlam materialização/via; `encerrar_por_fato_no_espelho` troca via isoladamente; `relavrar` também troca via pela porta própria. (`app/chamados/models.py:2545`, `app/chamados/models.py:2551`, `app/chamados/models.py:2614`, `app/chamados/models.py:2635`, `app/chamados/models.py:2644`, `app/chamados/models.py:2655`, `app/chamados/services/relavra_via.py:104`) |
| Reapontar | `reapontar_resolvedora` escreve destino/data e lavra no mesmo ato; valida o dia e trava o destino. O corpo não trata a origem que perdeu a última resolvedora. (`app/chamados/services/fio.py:784`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`) |
| Reapontamento irmão | `PerguntaDisputa.abrir` também altera destino/célula da identidade histórica. Um gatilho apenas em `reapontar_resolvedora` deixa esse caminho descoberto. (`app/chamados/models.py:2420`, `app/chamados/models.py:2424`, `app/chamados/models.py:2427`, `app/chamados/models.py:2463`) |
| Morte por leitura | C6 lê assinatura, tranca, resposta, identidade, marco, célula e janela; a porta é memoizada por instância. C6 não deve ser confundido com carimbo ou com alcance. (`app/chamados/juizes.py:822`, `app/chamados/juizes.py:1135`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1197`) |
| Lavra/relógio | `lavrar` e persistência em lote controlam `veredito_lavrado`, `julgado_em`, `janela_expira_em`; `esta_stale` lê expiração e marcos. O módulo declara mudança por relógio sem escrita. (`app/chamados/veredito_lavrado.py:23`, `app/chamados/veredito_lavrado.py:173`, `app/chamados/veredito_lavrado.py:197`, `app/chamados/veredito_lavrado.py:211`) |
| Lâmpada e outbox | `salvar_e_emitir` junta célula e evento em transação; `ata`/`veredito` acordam o consumidor; `emitir_evento_celula` é o escritor do evento. (`app/ponto/portas/celula.py:661`, `app/ponto/portas/celula.py:664`, `app/ponto/portas/celula.py:672`) |
| Consumidor existente | Consome eventos, retrata chamados absolvidos e encerra órfãs; falha mantém pendente, teto pode adiar, sucesso marca processado. Não chama a fábrica nesse corpo. (`app/chamados/reconciliador.py:1283`, `app/chamados/reconciliador.py:1326`, `app/chamados/reconciliador.py:1329`, `app/chamados/reconciliador.py:1339`, `app/chamados/reconciliador.py:1346`) |
| Fonte da pergunta | `motivos_a_perguntar` seleciona pela lâmpada e exclui identidades históricas; `perguntar` usa as portas existentes; `perguntar_pela_celula` orquestra. Não abrir segundo escritor em detector, sinal ou cron. (`app/chamados/services/fabrica_da_celula.py:74`, `app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:129`, `app/chamados/services/fabrica_da_celula.py:145`, `CLAUDE.md:175`) |
| Julgamento | Cartório chama fábrica somente para células julgadas naquela passada; `julgar_celula` também pode retornar por célula ausente ou turno de hoje ainda em curso. (`app/ponto/services/cartorio.py:982`, `app/ponto/services/cartorio.py:995`, `app/ponto/services/cartorio.py:1976`, `app/ponto/services/cartorio.py:1982`) |
| Vivos legítimos | C12/C13 nomeiam retenções; `dono_fora_do_fio` lê decisão disponível, ausência aberta e lista administrativa. Exclusão exige a fonte responder; falha da lista não absolve o dia. (`app/core/juizes.py:272`, `app/core/juizes.py:273`, `app/chamados/services/vigia_fio.py:195`, `app/chamados/services/vigia_fio.py:274`) |
| Cron e instrumento | Ramo FIO MUDO ainda escreve; execução configurada usa `--apply`; `--alarme` já oferece caminho sem escrita. Exceção e registro de varredura só saem com a condição documentada satisfeita. (`app/chamados/management/commands/disparar_perguntas_competencia.py:251`, `app/chamados/management/commands/disparar_perguntas_competencia.py:280`, `app/config/crons.py:1495`, `app/config/crons.py:2007`, `bin/tests/test_papel_prazo_nao_deriva.sh:303`, `app/docs/CORTES.md:305`) |

Os acionadores irmãos incluem dedup por comando, migração de resolvedoras, declaração textual, limbo, reata do beco e validação por foto/API, ações de disputa e comando de classe A. (`app/chamados/management/commands/dedup_perguntas_fato.py:88`, `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:947`, `app/chamados/services/declaracao_texto.py:320`, `app/chamados/services/limbo_folgas.py:243`, `app/chamados/services/limbo_folgas.py:276`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:122`, `app/api/foto_recebida.py:51`, `app/chamados/services/acoes_disputa.py:375`, `app/chamados/management/commands/validar_classe_a.py:100`)

Os leitores que devem permanecer coerentes com esses campos e portas são:

| Leitores | Referências |
|---|---|
| App, resposta web/API, propriedades `estado/viva`, relatórios, aviso de pendência, calendário e detalhe do dia | `app/chamados/services/pergunta_no_app.py:53`; `app/chamados/views.py:1238`; `app/api/views.py:2627`; `app/chamados/models.py:2491`; `app/chamados/models.py:2762`; `app/chamados/relatorios_disputa.py:18`; `app/chamados/services/aviso_pendencia.py:13`; `app/colaboradores/services/calendario.py:468`; `app/ponto/services/dia_do_colab.py:70` |
| Fechamento da disputa, dedup de emissão, limbo, prévia, grade, supra e emissão retroativa | `app/chamados/models.py:1783`; `app/chamados/services/disputa_emissao.py:1244`; `app/chamados/services/limbo_folgas.py:201`; `app/folha/previa.py:144`; `app/ponto/management/commands/reconciliar_grade.py:460`; `app/ponto/management/commands/supra_juiz.py:215`; `app/ponto/management/commands/emitir_furo_retroativo.py:98` |
| Fio administrativo, guarda de encerramento, carimbo, desfazimento de carimbo e vigias | `app/chamados/services/fio.py:234`; `app/chamados/models.py:351`; `app/chamados/models.py:2585`; `app/chamados/services/relavra_via.py:227`; `app/chamados/services/vigia_fio.py:417`; `app/chamados/services/vigia_fio.py:813` |
| Alcance, folha e lista administrativa | `app/chamados/juizes.py:1341`; `app/folha/fora_da_folha.py:67`; `app/chamados/services/vigia_fio.py:635`; `app/escala/services/cadastro_realidade.py:421` |
| Contador textual raspado pelo placar | `app/chamados/management/commands/disparar_perguntas_competencia.py:235`; `bin/placar_code.sh:381` |

A exaustividade de chamadas dinâmicas e o alcance individual nos 494 permanecem **[não verificado]**. O censo de escritores deve fechar antes de um filtro novo; transferir chamadores sem aposentar autoridade abandonada também não encerra a obrigação. (`CLAUDE.md:193`, `app/docs/LEIS.md:137`)

## C DIFF PROPOSTO EM TEXTO

**Não fecho um diff integral que realize o contrato.** A morte por leitura já tem juiz — C6 —, portanto não há motivo demonstrado para “PAREI por juiz novo”; falta fechar a cobertura dos produtores, das identidades históricas e da perda de alcance. A seção C solicitada fica **incompleta**, sem alegação de solução pronta. (`app/core/juizes.py:266`, `app/docs/PROMPTS.md:1882`, `app/chamados/models.py:2427`, `app/chamados/veredito_lavrado.py:23`)

Este é o menor candidato local para incluir o **nascimento real** de `batida_ausente` no evento existente. Ele fica **REPROVADO como diff da obra**, pelos contraexemplos da seção D; não deve ser confundido com implementação dos seis gatilhos. (`app/chamados/models.py:932`, `app/chamados/models.py:939`, `app/docs/PROMPTS.md:1875`)

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/models.py
@@ -938,4 +938,4 @@
                 from chamados.reconciliador import MODULOS_DE_TURNO as _MT
-                if (_mo or '') in _MT and ch.celula_id is not None:
+                if ((_mo or '') in _MT or _mo == 'batida_ausente') and ch.celula_id is not None:
                     from ponto.portas.celula import emitir_evento_celula
                     emitir_evento_celula(ch.celula)
```

O candidato conserva a porta escritora de evento e sua transação existente. Ele **não entrega a pergunta**: o consumidor atual retrata/encerra, sem chamar a fábrica. Acrescentar uma chamada à fábrica também não demonstra cobertura das perdas históricas. (`app/chamados/models.py:863`, `app/ponto/portas/celula.py:664`, `app/chamados/reconciliador.py:1326`, `app/chamados/reconciliador.py:1329`, `app/

[truncado em 20000 de 25078 caracteres]

## 05/11 18:28 · sessao bf8af0890a16 · C20-O169 · verificador · origem=Code

1. **Fechado.** A L-125 e o ADR agora ressalvam que `dia_sem_trabalho` prevalece sobre a falta de insumos, exatamente como o leitor executa: consulta o juiz antes de validar `orfas` e `lampadas` (`app/docs/LEIS.md:146`; `app/docs/PLANO-PADROES.md:74`; `app/ponto/services/cluster_espurio.py:25-30`).

2. **A descrição nova corresponde à ordem implementada nos cinco casos**, considerando os chamados vivos selecionados pelo coletor e com dia identificado pelo juiz (`app/ponto/services/cluster_espurio.py:66-74`; `app/docs/LEIS.md:146`; `app/docs/PLANO-PADROES.md:74`).

   | Caso | Ordem e resultado no código |
   |---|---|
   | **Célula ausente** | O coletor **não chama o leitor**: atribui `None`, preserva esse valor como fato e o catálogo conserva a premissa; não entra em `a_fechar` (`app/ponto/services/cluster_espurio.py:74-77`; `app/chamados/catalogo/premissa.py:127-130,144-150`). |
   | **Ata sem insumos, dia de trabalho** | O juiz retorna falso; depois, a falta de `orfas` ou `lampadas` como listas produz `None`. O coletor conserva a premissa (`app/chamados/juizes.py:422-428`; `app/ponto/services/cluster_espurio.py:25-30,74-77`; `app/chamados/catalogo/premissa.py:127-130`). |
   | **Ata sem insumos, dia sem trabalho** | O juiz retorna verdadeiro e o leitor devolve `()` **antes da validação dos insumos**. O coletor transforma isso em `False`; a premissa morre e o chamado entra em `a_fechar`. Isso inclui `trabalha=False, ata={}` (`app/chamados/juizes.py:422-428`; `app/ponto/services/cluster_espurio.py:25-30,74-77`; `app/chamados/catalogo/premissa.py:132-134,144-150`). |
   | **Ata medida sem cluster** | Se o juiz não declarar dia sem trabalho, o leitor valida as listas e avalia a assinatura; sem ela, retorna `()`. Se declarar, retorna `()` antes. Nos dois caminhos, fica elegível ao fechamento (`app/ponto/services/cluster_espurio.py:25-37,74-77`). |
   | **Ata com cluster** | **Se o juiz não declarar dia sem trabalho**, a assinatura produz `(orfas, n_missing)`, convertida em `True`, conservando a premissa. **Se declarar**, prevalece o retorno antecipado `()`, mesmo havendo insumos com assinatura (`app/ponto/services/cluster_espurio.py:25-36,74-77`; `app/chamados/catalogo/premissa.py:132-134`). |

   “Elegível” está correto: o fechamento efetivo ainda depende do recorte de colaborador/escala e das guardas de reconciliação (`app/ponto/services/cluster_espurio.py:117-125`; `app/chamados/models.py:637-655`).

3. **Não identifiquei outra divergência de comportamento entre esses textos e o coletor.** Ele consulta as datas dos próprios chamados, sem janela de idade, e distingue `None`, ausência e assinatura, compatível com a reconciliação descrita (`app/docs/LEIS.md:146`; `app/docs/PLANO-PADROES.md:74-75`; `app/ponto/services/cluster_espurio.py:66-77`).

   Permanecem duas ressalvas documentais:

   - “Fechamento por idade com outro nome” é uma **analogia**, não uma condição executada: converter `None` em `False` também atingiria dias recentes sem medida (`app/docs/PLANO-PADROES.md:76`; `app/ponto/services/cluster_espurio.py:74-77`; `app/chamados/catalogo/premissa.py:127-134`).
   - A frase “entra aqui no commit que mexe no código” continua na L-125; cumprimento histórico dessa exigência: **[nao verificado]** (`app/docs/LEIS.md:146`; exigência em `app/docs/BACKLOG.md:421-423`).

   O teste específico `trabalha=False, ata={}` continua declarado como fila; o caso existente usa listas preenchidas como vazias, portanto não prova essa precedência específica (`app/docs/LEIS.md:146`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:108-111`).

**APROVA — bloqueantes desta volta: nenhum; a generalização apontada na volta 2 foi corrigida nos dois textos (`app/docs/LEIS.md:146`; `app/docs/PLANO-PADROES.md:74`; `app/ponto/services/cluster_espurio.py:25-30`).**

## 05/11 18:28 · sessao f24449a2fddb · PCCMP-PEP · censo · origem=Code

HASH LIDO = 9c0fd9d3e90f166cba697ec840422b5585a8ab09

**1 COINCIDE**

As duas adotam a candidata 1: transação e releitura travada dentro de `carimbar`, seguida de C6, com conferência do memo e medição das travas antes do verde. Esse aval está registrado em `app/docs/PROMPTS.md:1882`.

**Casos comuns**

| pc1 ↔ pc2 | Item coincidente | Conferência na cópia |
|---|---|---|
| A1 ↔ A5 | Pergunta viva deve recusar carimbo automático. | O candidato passa por C6; resultado vivo desfaz o candidato e retorna `False`. `app/chamados/models.py:2585`; `app/chamados/models.py:2589`. |
| A2 ↔ A6 | Resolver o marco perguntado não exige resolver todos os demais marcos. | A precedência do marco perguntado está explícita em `app/chamados/juizes.py:1173`; condição em `app/chamados/juizes.py:1177`. |
| A3/A4 ↔ A14 | Resposta concordante pode encerrar; discordante ou incomparável permanece para decisão. | Critério em `app/chamados/juizes.py:1090`; incomparabilidade em `app/chamados/juizes.py:1096`; consumo em `app/chamados/juizes.py:1162`. |
| A5 ↔ A1 | Intenção antiga não deve substituir decisão humana posterior. | O inverso documenta essa corrida e relê a linha travada. `app/chamados/services/relavra_via.py:214`; `app/chamados/services/relavra_via.py:221`. |
| A6/A7 ↔ A2/A3 | Resposta nova não herda a assinatura da anterior, nas duas ordens de intercalação. | O escritor limpa a validação conforme a instância recebida e salva campos selecionados. `app/chamados/services/disputa_emissao.py:1286`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1301`. |
| A8 ↔ A4 | Repetição deve preservar estado e trilha. | Regra em `CLAUDE.md:175`; renovação atual do instante em `app/chamados/models.py:2570`; guarda administrativa em `app/chamados/models.py:2502`. |
| A9 ↔ A7 | Corte 21→20 e competência pertencente ao fato. | Cálculo em `app/ponto/janelas.py:10`; passagem à competência seguinte em `app/ponto/janelas.py:27`; corte cadastrado em `app/ponto/janelas.py:60`. |
| A10 ↔ A8 | Meia-noite integra a jornada iniciada anteriormente. | Encadeamento dos marcos em `app/escala/servico_jornada.py:80`; avanço para D+1 em `app/escala/servico_jornada.py:82`. |
| A11 ↔ A9 | Provisório sem prova não absolve; chegada da batida permite reconhecer o marco. | Corte em `app/docs/CORTES.md:413`; tratamento em `app/chamados/juizes.py:1190`; condição em `app/chamados/juizes.py:1195`. |
| A12 ↔ A10 | Respeitar a geradora histórica, com a diferença de fronteira indicada na seção 2. | Autoridade da geradora em `app/escala/alimentacao.py:231`; limite de vigência em `app/escala/alimentacao.py:272`. |
| A15 ↔ A13 | Ausência parcial cobra o restante descoberto. | Predicado em `app/chamados/juizes.py:403`; implementação em `app/chamados/juizes.py:410`; regra em `app/docs/JUIZES.md:58`. |
| A17 ↔ A16 | Falha do julgamento, lavra ou salvamento deve restaurar a memória local. | Restaurações em `app/chamados/models.py:2586` e `app/chamados/models.py:2599`; testes em `app/chamados/tests/test_o261e_f2_fila.py:179`, `app/chamados/tests/test_o261e_f2_fila.py:190` e `app/chamados/tests/test_o261e_f2_fila.py:200`. |
| A18 ↔ parte de A17 | Antiguidade não substitui fato resolutivo. | Corte específico em `app/docs/PROMPTS.md:1317`. |

**Sítios e escritores comuns**

| Item presente nas duas | Sítios conferidos |
|---|---|
| Fonte da alteração e julgamento existente | `carimbar`, atribuição da tripla, C6 e salvamento: `app/chamados/models.py:2551`; `app/chamados/models.py:2545`; `app/chamados/models.py:2585`; `app/chamados/models.py:2598`. |
| Autoridades C6/C7/C15/C17/C18 e competência | `app/core/juizes.py:266`; `app/core/juizes.py:267`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/core/juizes.py:278`; `app/core/juizes.py:283`. |
| Vínculo histórico e marcos após meia-noite | Registro do vínculo em `app/core/juizes.py:1285`; endereço dos marcos em `app/docs/JUIZES.md:24`. |
| Tripla atribuída por carimbo, identidade, restauração e descarimbo; limpeza em lote | `app/chamados/models.py:2575`; `app/chamados/models.py:2626`; `app/chamados/models.py:2608`; `app/chamados/models.py:2637`; `app/chamados/models.py:2652`. |
| Escritores adicionais da via isolada | `app/chamados/models.py:2662`; `app/chamados/services/relavra_via.py:123`. |
| Escritores de resposta e instante de resposta | `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1445`; limpeza em `app/chamados/services/disputa_emissao.py:1393`. |
| Validação também escrita fora da porta de validação | `app/chamados/models.py:2581`; `app/chamados/services/validacao.py:131`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/models.py:1493`. |
| Lavra, restauração e persistência em lote | `app/chamados/veredito_lavrado.py:211`; `app/chamados/veredito_lavrado.py:240`; `app/chamados/veredito_lavrado.py:197`. |
| Memo produzido por C6, reutilizado por chave e removido no desfazimento | `app/chamados/juizes.py:847`; `app/chamados/juizes.py:837`; `app/chamados/models.py:2612`; `app/chamados/models.py:2638`. |

Os **oito pontos diretos** enumerados coincidem:

| Chamador | Sítio |
|---|---|
| `encerrar_fantasma` | `app/chamados/models.py:2674` |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:405` |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:620` |
| Resolução por chamado, dois ramos | `app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/disputa_emissao.py:1675` |
| Absorção por ausência | `app/chamados/services/materializacao.py:546` |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1199` |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1346` |

Também coincidem os seguintes grupos de leitores; referências diferentes dentro da mesma função não foram consideradas omissões, como definição e retorno de `perguntas_do_app`. `app/chamados/services/pergunta_no_app.py:42`; `app/chamados/services/pergunta_no_app.py:53`.

| Grupo comum | Sítios conferidos |
|---|---|
| Estado, seleção, vitalidade, lastro e raio-X | `app/chamados/models.py:2481`; `app/chamados/models.py:2713`; `app/chamados/juizes.py:632`; `app/chamados/services/raiox.py:133`. |
| Reconciliação, materialização, validação e fábrica | `app/chamados/reconciliador.py:583`; `app/chamados/services/materializacao.py:433`; `app/chamados/services/materializacao.py:529`; `app/chamados/services/materializacao.py:1293`; `app/chamados/services/validacao.py:363`; `app/chamados/services/fabrica_da_celula.py:119`. |
| Fio, ações, declaração, avisos e becos | `app/chamados/services/fio.py:336`; `app/chamados/services/fio.py:656`; `app/chamados/services/acoes_disputa.py:130`; `app/chamados/services/acoes_disputa.py:341`; `app/chamados/services/declaracao_texto.py:211`; `app/chamados/services/declaracao_texto.py:366`; `app/chamados/services/aviso_pendencia.py:13`; `app/chamados/services/becos.py:30`. |
| Histórico, painel, verbos, explicação, canal e relatório | `app/chamados/services/historico.py:99`; `app/chamados/services/painel.py:67`; `app/chamados/services/verbos.py:240`; `app/chamados/services/verbos.py:311`; `app/chamados/services/explicador.py:79`; `app/chamados/canal.py:65`; `app/chamados/relatorios_disputa.py:21`. |
| App, API, mensageria e telas | `app/chamados/views.py:1257`; `app/chamados/views_cobrar.py:169`; `app/api/views.py:1173`; `app/api/views.py:2580`; `app/api/views.py:2636`; `app/api/views_mensageria.py:567`. |
| Calendário, ficha, folha, dia, cartório e procedência | `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/ponto/services/dia_do_colab.py:69`; `app/ponto/services/cartorio.py:1590`; `app/ponto/services/cartorio.py:1599`; `app/ponto/services/procedencia_batida.py:72`. |
| Vigias, supra e passivo | `app/chamados/services/vigia_fio.py:675`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/furo_sem_canal.py:81`; `app/ponto/supra_juiz.py:545`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:140`. |
| Comandos e auditoria | `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`; `app/chamados/management/commands/dedup_perguntas_fato.py:71`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:33`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`; `app/chamados/management/commands/relavrar_via_i6.py:58`; `app/ponto/management/commands/reconciliar_fantasmas.py:212`; `app/chamados/management/commands/auditar_invariantes_chamados.py:538`; `app/chamados/management/commands/auditar_invariantes_chamados.py:905`; `app/chamados/management/commands/auditar_invariantes_chamados.py:955`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1073`. |
| Templates, eventos e selos | `app/templates/chamados/partials/_card_atendimento.html:94`; `app/templates/chamados/partials/_card_atendimento.html:186`; `app/templates/chamados/partials/_card_atendimento.html:384`; `app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/modal_fio.html:212`; `app/chamados/signals.py:75`; `app/chamados/signals.py:125`; `app/chamados/signals.py:202`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:374`. |

**Trechos comuns dos diffs**

- Ambos propõem `refresh_from_db(from_queryset=...)` com `select_for_update(of=('self', 'disputa')).select_related('disputa', 'celula')`. O padrão existente está em `app/chamados/services/relavra_via.py:221`; o corpo atual de `carimbar` começa a mutação sem essa releitura em `app/chamados/models.py:2572`.
- Ambos fotografam o mesmo conjunto de 13 campos, rejeitam mudança desde a coleta e executam `self.__dict__.pop('_pv_memo', None)` antes do candidato. A chave atual e sua reutilização estão em `app/chamados/juizes.py:811` e `app/chamados/juizes.py:837`; ata e DNA são lidos em `app/chamados/juizes.py:1105` e `app/chamados/juizes.py:1111`.
- Ambos conservam `_escrever_carimbo`, C6 e a autoridade da lavra, sem apresentar outro escritor persistente nesses trechos. `app/chamados/models.py:2545`; `app/chamados/models.py:2585`; `app/chamados/veredito_lavrado.py:211`.

**Contraexemplos comuns**

| pc1 ↔ pc2 | Lacuna comum conferida |
|---|---|
| D1 ↔ D1 | Resposta posterior pode usar `update_fields` formado antes da validação concorrente. `app/chamados/services/disputa_emissao.py:1285`; `app/chamados/services/disputa_emissao.py:1297`; `app/chamados/services/disputa_emissao.py:1301`. |
| D2 ↔ D2 | Validação busca e examina a pergunta antes do `atomic`, sem releitura travada naquele caminho. `app/chamados/services/validacao.py:359`; `app/chamados/services/validacao.py:374`; `app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:396`. |
| D3 ↔ D6/D8 | A trava proposta não inclui todos os insumos de C6; copiar o `of` não mede ordem global. `app/chamados/services/relavra_via.py:221`; `app/chamados/juizes.py:1178`; `app/chamados/juizes.py:1201`; `app/docs/PROMPTS.md:1882`. |
| D4 ↔ D10 | Rollback da transação externa após retorno positivo ultrapassa a restauração local da porta. `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1199`; `app/chamados/models.py:2603`. |
| D5 ↔ D5 | A guarda de obsolescência não impede repetir uma linha já recarregada e renovar seu instante. `app/chamados/models.py:2570`; `app/chamados/models.py:2598`; contraste em `app/chamados/models.py:2502`. |
| D6 ↔ D7 | Limpar o memo nessa chamada não corrige sua chave para os demais leitores. `app/chamados/juizes.py:811`; `app/chamados/juizes.py:837`. |
| D7 ↔ D3 | Ata vazia recebe tratamentos diferentes no juiz de furo e em C6. `app/chamados/juizes.py:477`; `app/chamados/juizes.py:1195`. |
| D8 ↔ D4 | Tranca pode mudar entre o filtro do consumidor e a porta; C6 morto pela tranca não prova resolução. `app/chamados/reconciliador.py:616`; `app/chamados/services/disputa_emissao.py:1589`; `app/chamados/juizes.py:1140`. |
| D9 ↔ fecho de D | A prova precisa observar consumidores da escrita e `on_commit`. `app/chamados/services/disputa_emissao.py:1680`; `app/ponto/services/procedencia_batida.py:72`; `app/chamados/services/passivo_validado.py:140`; `app/chamados/signals.py:202`. |

**2 DIVERGE**

| Divergência | O que pc1 diz | O que pc2 diz | O que a cópia mostra e classificação |
|---|---|---|---|
| Ato obsoleto | C acrescenta `return False` após comparar a fotografia. Sítio alterado: `app/chamados/models.py:2570`. | C acrescenta `raise ValueError(...)` na mesma condição. Sítio alterado: `app/chamados/models.py:2570`. | Hoje a recusa booleana corresponde a C6 viva. Ausência registra essa interpretação; materialização transforma-a em reprovação. **DESENHO: protocolo de conflito e tratamento pelos consumidores.** `app/chamados/models.py:2589`; `app/chamados/services/materializacao.py:546`; `app/chamados/services/materializacao.py:549`; `app/chamados/services/materializacao.py:1201`; `app/chamados/services/materializacao.py:1207`. |
| Identidade preexistente | C não acrescenta guarda para identidade já presente quando a fotografia continua igual. O candidato continua no escritor atual. `app/chamados/models.py:2575`. | C acrescenta `False` se a linha já estiver materializada por identidade. Referência invocada: `app/chamados/services/relavra_via.py:161`. | A porta atual rejeita identidade na **via recebida**, mas não testa a identidade anterior antes de sobrescrevê-la. **DESENHO: quais estados o escritor pode substituir.** `app/chamados/models.py:2566`; `app/chamados/models.py:2575`; `app/chamados/models.py:2615`. |
| Limite da restauração transacional | C usa `@transaction.atomic`, preservando os tratamentos internos atuais. `app/chamados/models.py:2584`; `app/chamados/models.py:2594`. | C põe `with transaction.atomic()` dentro de `try`, incluindo a saída da transação local no tratamento. Autoridade de restauração: `app/chamados/veredito_lavrado.py:240`. | Os tratamentos atuais cobrem exceções de C6 e de lavra/save; terminam antes do retorno. **DESENHO: alcance da restauração.** Nenhuma proposta cobre automaticamente rollback futuro do consumidor. `app/chamados/models.py:2586`; `app/chamados/models.py:2599`; `app/chamados/models.py:2603`; `app/chamados/services/materializacao.py:1142`. |
| Ata vazia contraditória | A14 deixa o resultado exato de `trabalha=True`, veredito não acusador e `ata={}` como `[nao verificado]`. Referências: `app/docs/LEIS.md:95`; `app/docs/LEIS.md:136`; `app/docs/CORTES.md:416`. | A11 exige desconhecimento e rejeita encerramento. Referências: `app/docs/LEIS.md:95`; `app/chamados/juizes.py:473`; `app/docs/LEIS.md:136`. | `_celula_furada` retorna `None` sem lâmpadas; C6 pode retornar `False` pelo veredito não acusador e pela ausência de pendências. **DESENHO: resultado exigido e autoridade aplicável.** O código atual não resolve a divergência normativa. `app/chamados/juizes.py:477`; `app/chamados/juizes.py:1195`; `app/ponto/services/lastro.py:148`; `app/docs/LEIS.md:136`. |
| Limite da geradora histórica | A12 manda respeitar geradora e DNA, sem explicitar geradora fora da vigência do fato. `app/docs/JUIZES.md:98`; `app/escala/alimentacao.py:231`. | A10 acrescenta geradora fora da vigência → desconhecimento/CADASTRO. `app/escala/alimentacao.py:272`. | A função devolve `None` fora da vigência, sem escolher silenciosamente outro vínculo. **DESENHO por omissão de fronteira**; os textos são compatíveis dentro da vigência. `app/escala/alimentacao.py:279`; `app/escala/alimentacao.py:282`. |
| Contrato documental da memória | C atualiza a descrição para desfazer sobre a linha relida. Sítio: `app/chamados/models.py:2606`. | C mantém a descrição atual, embora também releia a instância. Sítio: `app/chamados/models.py:2606`. | O texto atual promete voltar “EXATAMENTE como chegou”. **Detalhe documental com efeito sobre o contrato descrito.** `app/chamados/models.py:2606`; `app/chamados/models.py:2607`. |
| Organização e ordem dos campos | A3/A4 separados; A8 reúne concorrência e repetição; C ordena os 13 campos de uma forma. `app/chamados/juizes.py:1090`; `CLAUDE.md:175`. | A14 agrupa concordância/discordância; A4 enfatiza repetição; C reordena os mesmos campos. `app/chamados/juizes.py:1090`; `CLAUDE.md:175`. | **Detalhe:** não muda o juiz nem o conjunto comparado; os campos centrais continuam em `app/chamados/models.py:2572`, `app/chamados/models.py:2576` e `app/chamados/models.py:2580`. |

As diferenças do censo na seção 3 são divergências de **universo da prova por omissão**, sem demonstrarem comportamentos incompatíveis dos consumidores. A conferência de data é particularmente relevante: altera a liberação da materialização e não aparece nos campos comparados de nenhum dos dois diffs. `app/chamados/services/validacao.py:617`; `app/chamados/services/materializacao.py:1116`.

**3 SO NUMA**

**Só pc1 — casos e sítios adicionais**

| Item | Conferência |
|---|---|
| A13: variante explícita `trabalha=None` versus `False` | Corte em `app/docs/CORTES.md:416`; predicado em `app/chamados/juizes.py:418`. |
| A16: continente encerrado não equivale à decisão humana do dia | Corte em `app/docs/CORTES.md:342`; C6 procura a decisão na célula em `app/chamados/juizes.py:1148`. |
| Instrumentos fora de `app` | `bin/censo_folha.py:28`; `bin/censo_folha.py:29`; `bin/sombra_regras_pessoais.py:85`. |
| Cobrança e flip | `app/chamados/services/cobranca.py:229`; `app/ponto/services/flip_auto.py:129`. |
| Comandos adicionais | `app/ponto/management/commands/reconciliar_grade.py:461`; `app/chamados/management/commands/validar_classe_a.py:80`; `app/chamados/management/commands/auditar_disputas.py:71`; `app/chamados/management/commands/auditar_disputas.py:150`. |
| Preparação dos campos da lavra no backfill | `app/chamados/management/commands/backfill_veredito_lavrado.py:138`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`; `app/chamados/management/commands/backfill_veredito_lavrado.py:140`. |
| Seletores e consumidores adicionais em arquivos comuns | `app/chamados/reconciliador.py:1017`; `app/chamados/reconciliador.py:1433`; `app/chamados/services/materializacao.py:604`; `app/chamados/services/disputa_emissao.py:909`; `app/chamados/services/disputa_emissao.py:1152`; `app/chamados/services/fabrica_da_celula.py:138`; `app/chamados/services/acoes_disputa.py:95`; `app/chamados/services/declaracao_texto.py:635`; `app/chamados/services/painel.py:260`; `app/chamados/services/explicador.py:32`; `app/chamados/services/explicador.py:95`. |
| Propriedade e redução adicionais | `app/chamados/models.py:2807`; `app/chamados/juizes.py:1225`; `app/chamados/juizes.py:1257`. |
| App/API/foto adicionais | `app/chamados/views.py:1280`; `app/chamados/views.py:1416`; `app/chamados/views_cobrar.py:176`; `app/api/views.py:1009`; `app/api/views.py:1187`; `app/api/foto_recebida.py:48`. |
| Templates adicionais | `app/templates/chamados/partials/modal_fio.html:184`; `app/templates/chamados/partials/modal_fio.html:223`; `app/templates/chamados/partials/_card_atendimento.html:100`; `app/templates/chamados/partials/_card_atendimento.html:137`; `app/templates/chamados/partials/_card_atendimento.html:155`; `app/templates/chamados/partials/_pergunta_card.html:42`. |
| I20: resposta preenchida sem instante de resposta | O sítio existe, mas recebeu rótulo inadequado no anexo, conforme seção 4. `app/chamados/management/commands/auditar_invariantes_chamados.py:1102`. |

**Só pc2 — casos, guardas e sítios adicionais**

| Item | Conferência |
|---|---|
| A15: identidade com resposta ainda pendente | A resposta pode preceder a identidade em C6. `app/chamados/models.py:2617`; `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1171`. |
| A10: geradora fora da vigência do fato | Fronteira explícita em `app/escala/alimentacao.py:272`; retorno em `app/escala/alimentacao.py:282`. |
| Guarda de identidade preexistente

[truncado em 20000 de 25961 caracteres]

## 05/11 18:27 · sessao e719ad4dc562 · PC3-OTIMIZA-3 · censo · origem=Code

HASH LIDO = d1354f77cf4c7214728709ad948145994a0c0366

## A CASOS PELA REGRA

O hash acima identifica a cópia solicitada; não consegui confirmá-lo por Git porque esta cópia não contém `.git`. [nao verificado]

A referência é o contrato, pela L-110. A unidade é **faceta**: arquivo e cada símbolo mudado têm cobertura própria. Faceta coberta devolve N; cobertura desconhecida devolve **“nao varrido”**, com eventuais achados avulsos. Um zero não pode fundamentar L-111 quando o mecanismo do chamador não foi coberto. (`app/docs/LEIS.md:136`, `app/docs/LEIS.md:137`, `bin/quem_chama.py:8`)

O aval fornecido determina **não varrer Python em `.sh`** e limita o instrumento a duas voltas opus; as reprovações #3/#4 também integram o contrato desta análise. Esses anexos não têm localização conferida na cópia. [nao verificado]

Nos casos sintéticos abaixo, N pressupõe somente os leitores expressamente descritos; não representa contagem de produção. (`bin/quem_chama.py:11`, `CLAUDE.md:35`)

| Alvo + estado + ato | Resposta exigida |
|---|---|
| `datas_cravadas_prod` mudado; chamada existente em `placar_code.sh` por `shell --command` | Declarar **não varridas** as facetas Python afetadas; conservar a citação encontrada como avulsa. Nunca apresentar zero conclusivo para o símbolo. (`bin/placar_code.sh:384`, `bin/quem_chama.py:12`) |
| Módulo/símbolo mudado; único leitor em heredoc, `python3 -c` ou `shell --command` | Mesma abstenção, independentemente do invólucro shell. A solução não pode depender apenas da forma do caso de `placar_code`. (`bin/quem_chama.py:14`, `bin/esteira.sh:16`, `bin/fabricante.sh:41`, `bin/placar_code.sh:384`; aval específico [nao verificado]) |
| `core.esteira_vigia` mudado; leitor usa `from core import esteira_vigia` | Não exigir a presença textual de `core.esteira_vigia` para reconhecer cobertura incompleta. Declarar não varrido. (`bin/esteira.sh:19`, `bin/fabricante.sh:43`, `bin/quem_chama.py:12`) |
| Módulo `bin/foo.py`; leitor shell cita `foo`, inclusive execução `-mfoo` | A ausência de ponto no nome não autoriza zero. Declarar a cobertura incompleta. (`bin/quem_chama.py:125`, `bin/quem_chama.py:14`; caso `-mfoo` [nao verificado]) |
| `.py` comum; somente `from m.lib import mudada`, absoluto ou relativo | Arquivo: **1**; símbolo `mudada`: **1**. O import precisa aparecer na faceta correta. (`bin/quem_chama.py:39`, `bin/quem_chama.py:51`, `bin/tests/test_quem_chama.sh:6`) |
| `mudada` alterada; único leitor importa `parada`, inclusive por relativo | Arquivo: **1**; símbolo `mudada`: **0**, se não houver outro leitor nem mecanismo excluído. A linha “importa de” pertence somente ao arquivo. (`bin/tests/test_quem_chama.sh:158`, `bin/tests/test_quem_chama.sh:166`) |
| Pacote sem `__init__.py`; símbolo sem uso local; único leitor relativo externo | Símbolo: **1**. Uso próprio não pode mascarar uma falha na resolução do relativo. (`bin/quem_chama.py:39`, `bin/tests/test_quem_chama.sh:28`, `bin/tests/test_quem_chama.sh:55`) |
| Símbolo apagado/renomeado ou módulo apagado; leitor permanece na ponta | Manter o alvo antigo e listar o leitor residual da ponta; arquivo apagado deve ser identificado. (`bin/quem_chama.py:17`, `bin/tests/test_quem_chama.sh:7`) |
| CSS/JS/imagem sob `static/`; única referência literal suportada | Faceta arquivo: **1**. A extensão isoladamente não o torna não varrido. (`bin/quem_chama.py:49`, `bin/tests/test_quem_chama.sh:213`) |
| Template; único `include`/`extends` literal ou referência de renderização | Faceta arquivo: **1**. (`bin/quem_chama.py:47`, `bin/tests/test_quem_chama.sh:182`) |
| Filtro registrado; template carrega a biblioteca e usa `\|nome` | Biblioteca e símbolo recebem seus respectivos achados; filtro com um uso: **1**. (`bin/quem_chama.py:53`, `bin/tests/test_quem_chama.sh:217`) |
| Filtro registrado; único uso em `{% filter nome %}` | O uso deve alcançar a faceta do filtro; não pode resultar em zero falso. A fila da reprovação cita essa borda. (`bin/quem_chama.py:53`; anexo da reprovação [nao verificado]) |
| Aparente uso somente em `{# … #}` ou `{% comment %}…{% endcomment %}`, multilinha | Não contar o comentário. Havendo apenas o carregamento da biblioteca: arquivo **1**, filtro **0**. (`bin/quem_chama.py:21`, `bin/quem_chama.py:520`) |
| Método por instância/herança; setting; autodiscovery; decorador opaco; tipo desconhecido | **Não varrido**, mesmo quando nenhum achado avulso exista. (`bin/quem_chama.py:14`, `bin/quem_chama.py:299`, `bin/quem_chama.py:315`) |
| Nome dinâmico, reexport ou crontab do host | Exclusão declarada; a contagem da parte coberta não prova ausência nesses mecanismos. (`bin/quem_chama.py:65`) |
| Mesmo range imutável, disco alterado ou duas leituras concorrentes sem saída compartilhada | Ler a ponta identificada pelo range; alterações do disco não entram. Mesmos insumos devem produzir o mesmo texto. (`bin/quem_chama.py:17`, `bin/tests/test_quem_chama.sh:184`, `bin/tests/test_quem_chama.sh:257`) |
| Mudança 21→20, cross-meia-noite, vínculo trocado ou célula sem ata, sem alterar os insumos Git | Não introduzir julgamento de ponto nesta ferramenta. Essas bordas pertencem ao domínio; aqui os insumos são revisões e conteúdo Git. (`CLAUDE.md:81`, `CLAUDE.md:185`, `bin/quem_chama.py:17`) |

## B SITIOS

O instrumento declara **zero autoridade nova**. `core/juizes.py` registra autoridades de domínio e `JUIZES.md` é lookup; não identifiquei juiz registrado que substitua o contrato de cobertura deste censo. (`bin/quem_chama.py:6`, `app/core/juizes.py:3`, `app/docs/JUIZES.md:7`)

| Sítio/campo | Escritor e leitores que precisam acompanhar a obra |
|---|---|
| **Contrato e exclusões — tocar** | Docstring e `FORA`; `render` publica as exclusões. (`bin/quem_chama.py:8`, `bin/quem_chama.py:65`, `bin/quem_chama.py:706`) |
| **Tipo da faceta arquivo — tocar na fonte** | `tipo_do_arquivo` decide; `quem_chama` grava `tipo`; `render/_faceta` leem. A abstenção deve nascer aqui, sem remendo no texto final. (`bin/quem_chama.py:284`, `bin/quem_chama.py:637`, `bin/quem_chama.py:658`, `bin/quem_chama.py:713`) |
| **Tipo da faceta símbolo** | `_tipo_simbolo` herda tipos não cobertos; `quem_chama` grava `info['tipo']`; `render` lê cada símbolo. (`bin/quem_chama.py:311`, `bin/quem_chama.py:655`, `bin/quem_chama.py:715`) |
| **Código shell — tocar a alimentação do classificador** | `tipo_chamador` identifica shell, inclusive script sem extensão; `_codigo_sh` retira comentários; `varrer_linhas` e `artefatos_sh` já leem essas linhas. (`bin/quem_chama.py:599`, `bin/quem_chama.py:479`, `bin/quem_chama.py:510`, `bin/quem_chama.py:576`, `bin/quem_chama.py:674`) |
| **Identidade da biblioteca — preservar ao tocar** | `lib` nasce em `quem_chama`; `Alvos.libs` indexa; `varrer_template` consulta. Transformar cobertura em desconhecida não pode apagar essa identidade. (`bin/quem_chama.py:657`, `bin/quem_chama.py:373`, `bin/quem_chama.py:549`) |
| **Filtro em bloco — tocar na fonte** | `varrer_template` produz o achado do símbolo; é chamado pelo ramo template do coletor. Limpeza de comentários já existe. (`bin/quem_chama.py:526`, `bin/quem_chama.py:560`, `bin/quem_chama.py:676`, `bin/quem_chama.py:520`) |
| Insumos Git e conjunto de alvos | `lados`, `tocados_do_diff`, `linhas_mudadas`, `_git/_git_bytes` e `_ler_rev` alimentam `quem_chama`. Base fornece os apagados; ponta fornece leitores. (`bin/quem_chama.py:76`, `bin/quem_chama.py:88`, `bin/quem_chama.py:109`, `bin/quem_chama.py:142`, `bin/quem_chama.py:157`, `bin/quem_chama.py:619`) |
| Metadados `nomes/modulo/template/estatico/artefatos` | `quem_chama` constrói; `Alvos` indexa; leitores são `varrer_py`, `varrer_linhas` e `varrer_template`. Artefatos shell vêm de `artefatos_sh`. (`bin/quem_chama.py:633`, `bin/quem_chama.py:658`, `bin/quem_chama.py:363`, `bin/quem_chama.py:390`, `bin/quem_chama.py:510`, `bin/quem_chama.py:573`) |
| Metadados dos símbolos | `simbolos_mudados` usa `_parse`, `_libs_de_template`, `_registro` e `_nome_decorador`; `_sim`, `varrer_py`, `varrer_proprio`, `_tipo_simbolo` e `render` consomem os resultados. (`bin/quem_chama.py:195`, `bin/quem_chama.py:212`, `bin/quem_chama.py:233`, `bin/quem_chama.py:243`, `bin/quem_chama.py:385`, `bin/quem_chama.py:442`, `bin/quem_chama.py:649`) |
| Imports e referências Python | `_resolve`, `_cadeia`, `_prosa` e `Alvos.citacoes` são lidos por `varrer_py`; não devem ser reutilizados para interpretar payload shell contra o aval. (`bin/quem_chama.py:329`, `bin/quem_chama.py:336`, `bin/quem_chama.py:351`, `bin/quem_chama.py:378`, `bin/quem_chama.py:390`; aval [nao verificado]) |
| Achados e contadores | `quem_chama` agrega `por`; shell atualmente escreve somente `q=None`. `_faceta` conta varridas/zeros/desconhecidas; `render` fecha o resumo; `main` lê `conta['nao']`. (`bin/quem_chama.py:510`, `bin/quem_chama.py:662`, `bin/quem_chama.py:681`, `bin/quem_chama.py:690`, `bin/quem_chama.py:718`, `bin/quem_chama.py:747`) |
| **Selo — tocar** | Fixture, asserções de parado/relativo, isolamento por arquivo e invariante de cobertura. As verificações atuais não prendem a faceta. (`bin/tests/test_quem_chama.sh:28`, `bin/tests/test_quem_chama.sh:159`, `bin/tests/test_quem_chama.sh:167`, `bin/tests/test_quem_chama.sh:210`, `bin/tests/test_quem_chama.sh:239`) |
| Consumidores externos encontrados | O selo seleciona a ferramenta por `QUEM_CHAMA_PY`, executa fixture, determinismo e históricos; a régua chama todos os selos shell. (`bin/tests/test_quem_chama.sh:25`, `bin/tests/test_quem_chama.sh:148`, `bin/tests/test_quem_chama.sh:257`, `bin/tests/test_quem_chama.sh:266`, `bin/regua.sh:138`) |
| Saída persistida | `main` é quem cria diretórios e grava `--saida/--obra`. Esse caminho não foi necessário para a proposta e não deve ganhar outro escritor. (`bin/quem_chama.py:738`, `bin/quem_chama.py:741`) |

## C DIFF PROPOSTO EM TEXTO

O candidato abaixo cura a classe shell por **abstenção conservadora no classificador**, preserva a biblioteca de template e corrige o filtro em bloco. Não interpreta Python em shell nem acrescenta juiz. O selo passa a conferir arquivo **e faceta**. Permanecem os limites impeditivos descritos em D. (`bin/quem_chama.py:284`, `bin/quem_chama.py:311`, `bin/quem_chama.py:560`, `bin/quem_chama.py:657`, `bin/tests/test_quem_chama.sh:159`)

```diff
--- a/bin/quem_chama.py
+++ b/bin/quem_chama.py
@@ -12,7 +12,8 @@
 faceta de qualquer outro tipo imprime 'nao varrido (<tipo>)', NUNCA 0, e lista o que achou como AVULSO (achado
 avulso prova que ha chamador; a falta dele nao prova nada). O RESUMO do fim conta as nao varridas por tipo.
 Desconhecido e desconhecido: o classificador (`tipo_do_arquivo`, `_tipo_simbolo`) cai em tipo FORA da lista para
-tudo que nao reconhece.
+tudo que nao reconhece. Python em shell nao passa por AST: ocorrencia textual do ultimo componente do
+modulo em codigo shell torna suas facetas nao varridas; os achados continuam avulsos.
 
 O que se le: a arvore e o conteudo da PONTA do range, por git, nunca o disco; o lado VELHO (a base) so entra para
 colher o que o diff apagou ou renomeou (simbolo e modulo apagados seguem com os chamadores que sobraram na ponta).
@@ -66,7 +67,8 @@
         'com variavel); reexport por __init__ (o __init__ entra, quem importa dele nao); crontab do host (vale '
         'config/crons.py); artefato escrito por .py; no proprio arquivo, nome local que sombreia o de topo conta '
         'como uso, e nome ambiguo/texto visivel de template conta como citacao (falso positivo, nunca falso 0); '
-        'comentario de .sh dentro de string de varias linhas.')
+        'comentario de .sh dentro de string de varias linhas; Python em shell nao e varrido '
+        '(modulo citado: facetas nao varridas, achados avulsos).')
 LOGS = os.path.join(os.path.expanduser('~'), 'saas-hasner', 'logs')
 RAIZES_DE_MODULO = ('app/', 'bin/')     # sys.path do Django (app/) e dos scripts de host (bin/)
 _CONFIG_EXT = ('.json', '.yml', '.yaml', '.toml', '.ini', '.cfg', '.conf', '.service', '.timer', '.path',
@@ -281,7 +283,7 @@
 _COMANDO = re.compile(r'(?:^|/)management/commands/(\w+)\.py$')
 
 
-def tipo_do_arquivo(rel):
+def tipo_do_arquivo(rel, codigo_sh=''):
     """Tipo da faceta ARQUIVO. Tipo fora de VARRIDOS = nao varrido. -> str"""
     base = rel.rsplit('/', 1)[-1]
     ext = base.rsplit('.', 1)[-1].lower() if '.' in base.strip('.') else ''
@@ -292,6 +294,8 @@
     if ext == 'py':
         if modulo_de(rel) is None:
             return 'py fora de app/ e bin/'
+        if modulo_de(rel).rsplit('.', 1)[-1] in codigo_sh:
+            return 'py citado em shell: payload nao varrido'
         if re.search(r'(^|/)tests?/', rel) or re.match(r'(test_.*|.*_test|conftest|tests)\.py$', base):
             return 'py-teste' [nao verificado]
         if '/migrations/' in rel:
@@ -557,7 +561,7 @@
                 if not reg or not reg[1] or (nomes is not None and reg[1] not in nomes):
                     continue
                 papel, nome = reg
-                rx = re.compile(r'\|\s*' + re.escape(nome) + r'(?![\w])' if papel == 'filtro'
+                rx = re.compile(r'(?:\|\s*|{%\s*filter\s+)' + re.escape(nome) + r'(?![\w])' if papel == 'filtro'
                                 else r'{%\s*' + re.escape(nome) + r'(?![\w])')
                 for n, ln in enumerate(linhas, start=1):
                     if rx.search(ln):
@@ -626,6 +630,8 @@
     por_base = {}
     for r in arvore:
         por_base[r.rsplit('/', 1)[-1]] = por_base.get(r.rsplit('/', 1)[-1], 0) + 1
+    codigo_sh = '\n'.join(cod for r, f in na_ponta.items() if tipo_chamador(r, f) == 'sh'
+                          for cod in _codigo_sh(f).values())
     alvos = []
     for rel in tocados:
         fonte, antiga = na_ponta.get(rel), na_base.get(rel)
@@ -634,7 +640,7 @@
         if base != rel:
             n = por_base.get(base, 0)
             nomes.append((base, 'cita' if n <= 1 else 'cita (nome ambiguo, %d arquivos)' % n))
-        tipo = tipo_do_arquivo(rel)
+        tipo = tipo_do_arquivo(rel, codigo_sh)
         mod = modulo_de(rel)
         cmd = _COMANDO.search(rel)
         if cmd:
@@ -654,7 +660,7 @@
                         simb[q]['registro'] = info['registro']
         for info in simb.values():
             info['tipo'] = _tipo_simbolo(rel, tipo, info)
-        lib = base[:-3] if tipo == 'py-templatetag' else None
+        lib = base[:-3] if tipo_do_arquivo(rel) == 'py-templatetag' else None
         alvos.append({'rel': rel, 'existe': fonte is not None, 'modulo': mod, 'simbolos': simb, 'tipo': tipo,
                       'template': _template_de(rel), 'estatico': _estatico_de(rel), 'lib': lib,
                       'artefatos': artef, 'nomes': nomes})
--- a/bin/tests/test_quem_chama.sh
+++ b/bin/tests/test_quem_chama.sh
@@ -115,6 +115,33 @@
 : > app/m/vazio.py
 printf '{"hooks": {"Stop": "bash bin/alvo.sh"}}\n' > .claude/settings.json
 printf '[Service]\nExecStart=/bin/bash bin/alvo.sh\n' > bin/hasner-x.service
+# Casos independentes: nenhum uso local nem __init__.py mascara o import relativo.
+printf 'def unico():\n    return 1\n' > app/m/isolada.py
+printf 'from .isolada import unico\n' > app/m/rel_isolada.py
+for _nome in shell_heredoc shell_c shell_command shell_import shell_sem_ext; do
+  printf 'def unico():\n    return 1\n' > "app/m/$_nome.py"
+done
+printf 'def unico():\n    return 1\n' > bin/shell_raiz.py
+cat > bin/emb.sh <<'EOT'
+python3 - <<'PY'
+from m.shell_heredoc import unico
+PY
+python3 -c "from m.shell_c import unico"
+python manage.py shell --command "from m.shell_command import unico"
+python3 -c "from m import shell_import"
+python3 -mshell_raiz
+EOT
+printf '#!/bin/sh\npython3 -c "from m import shell_sem_ext"\n' > bin/emb
+cat > app/m/templates/m/bloco.html <<'EOT'
+{% load m_tags %}
+{% filter rot %}x{% endfilter %}
+{#
+{{ x|rot }}
+#}
+{% comment %}
+{{ x|rot }}
+{% endcomment %}
+EOT
 git add -A && git commit -qm base [nao verificado]
 # --- muda: mudada muda, sumiu some, velho_nome vira novo_nome, velho.py some, cmd_x/alvo.html/ação.sh mudam
 printf '#!/usr/bin/env bash\necho b > logs/alvo.stamp\n' > bin/alvo.sh
@@ -137,6 +164,10 @@
 printf 'from x import receiver\n\n\n@receiver("s")\ndef r():\n    return 2\n' > app/m/sinais.py
 printf 'caf\351 2\n' > app/m/dados.txt
 git rm -q app/m/vazio.py
+for _nome in isolada shell_heredoc shell_c shell_command shell_import shell_sem_ext; do
+  printf 'def unico():\n    return 2\n' > "app/m/$_nome.py"
+done
+printf 'def unico():\n    return 2\n' > bin/shell_raiz.py
 git commit -qam muda [nao verificado]
 # (f) disco sujo depois da ponta: um chamador NAO commitado nao entra; um chamador apagado so no disco segue [nao verificado]
 printf 'from m.lib import mudada\n' > app/m/sujo_sai.py && git add app/m/sujo_sai.py && git commit -qm sujo
@@ -148,6 +179,18 @@
 python3 "$Q" ponta~2..ponta --raiz "$F" --saida "$T/f.txt" >/dev/null 2>"$T/err" || _falha "fixture rc!=0: $(head -c 300 "$T/err")"
 _tem()   { grep -qF -- "$2" "$T/f.txt" || _falha "$1: devia listar [$2]"; }
 _naotem(){ grep -qF -- "$2" "$T/f.txt" && _falha "$1: MORDE -- [$2] entrou"; }
+# Isola arquivo E faceta; "simbolos" seleciona todas as facetas de simbolo.
+_fac() {
+  awk -v a="== $2 (" -v f="$3" '
+    /^== / {alvo=index($0,a)==1; faceta=0}
+    /^RESUMO/ {alvo=0}
+    alvo && /^   (arquivo|simbolo )/ {
+      faceta=(f=="simbolos" && index($0,"   simbolo ")==1) ||
+             index($0,"   " f " [")==1 || index($0,"   " f ":")==1
+    }
+    alvo && faceta {print}
+  ' "$1"
+}
 # (a) codigo x prosa
 _tem   base   "  bin/codigo.sh:2:"
 _tem   base   "  app/m/usa.py:1:"
@@ -156,16 +199,16 @@
 _naotem prosa "  bin/prosa.sh:"
 _naotem prosa "  app/m/doc.py:"
 # volta 4: quem importa um nome PARADO carrega o ARQUIVO (faceta arquivo), mas nao chama o simbolo mudado
-grep -F "  app/m/usa_outra.py:" "$T/f.txt" | grep -qv "^  app/m/usa_outra.py:1: importa de m.lib$" && _falha "simbolo-parado: MORDE -- usa_outra entrou fora da faceta arquivo"
-_tem   simbolo-parado "  app/m/usa_outra.py:1: importa de m.lib"
+_fac "$T/f.txt" app/m/lib.py arquivo | grep -qxF "  app/m/usa_outra.py:1: importa de m.lib" || _falha "simbolo-parado: falta faceta arquivo"
+_fac "$T/f.txt" app/m/lib.py simbolos | grep -qF "  app/m/usa_outra.py:" && _falha "simbolo-parado: chamador na faceta de simbolo"
 _naotem fim-de-linha "  bin/fim_coment.sh:"
 _tem   fim-de-linha "  bin/codigo_coment.sh:2:"
 # (b) relativo e apelido
 _tem   relativo1 "  app/m/rel1.py:1: importa mudada"
 _tem   relativo2 "  app/m/sub/rel2.py:1: importa mudada"
 # volta 4: quem importa um nome PARADO carrega o ARQUIVO (faceta arquivo), mas nao chama o simbolo mudado
-grep -F "  app/m/rel_outra.py:" "$T/f.txt" | grep -qv "^  app/m/rel_outra.py:1: importa de m.lib$" && _falha "relativo-parado: MORDE -- rel_outra entrou fora da faceta arquivo"
-_tem   relativo-parado "  app/m/rel_outra.py:1: importa de m.lib"
+_fac "$T/f.txt" app/m/lib.py arquivo | grep -qxF "  app/m/rel_outra.py:1: importa de m.lib" || _falha "relativo-parado: falta faceta arquivo"
+_fac "$T/f.txt" app/m/lib.py simbolos | grep -qF "  app/m/rel_outra.py:" && _falha "relativo-parado: chamador na faceta de simbolo"
 _tem   apelido1 "  app/m/apelido1.py:2: usa L.mudada"
 _tem   apelido2 "  app/m/apelido2.py:2: usa L.mudada"
 # (c) lado velho
@@ -249,6 +292,17 @@
     && _falha "r4-invariante $2: faceta que nao e contagem nem nao varrido"
 }
 _invariante "$T/f.txt" fixture
+# O relativo e o unico leitor: a faceta precisa mostrar exatamente um.
+[ ! -e "$F/app/m/__init__.py" ] || _falha "fixture ganhou __init__.py"
+_fac "$T/f.txt" app/m/isolada.py 'simbolo unico' | grep -qF ']: 1 chamador(es)' || _falha "relativo isolado: contagem"
+_fac "$T/f.txt" app/m/isolada.py 'simbolo unico' | grep -qxF '  app/m/rel_isolada.py:1: importa unico (mudado no diff)' || _falha "relativo isolado: leitor"
+for _alvo in app/m/shell_{heredoc,c,command,import,sem_ext}.py bin/shell_raiz.py; do
+  for _faceta in arquivo 'simbolo unico'; do
+    _fac "$T/f.txt" "$_alvo" "$_faceta

[truncado em 20000 de 23817 caracteres]

## 05/11 18:26 · sessao 9ef08422633f · C20-O169 · verificador · origem=Code

1. **A ausência da lei foi corrigida; o cumprimento temporal não foi demonstrado.** A L-125 reproduz literalmente o corte de `app/docs/BACKLOG.md:424-426`, protege as funções existentes `_leitura_da_celula` e `_vivos_que_a_premissa_matou`, e cita uma classe de teste existente com sete casos (`app/docs/LEIS.md:146`; `app/ponto/services/cluster_espurio.py:15,53`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:39,74-111`).

   O adendo exige registro **no commit que mexer no código** (`app/docs/BACKLOG.md:421-423`). Os anexos apresentados mostram cura e registro em commits separados; conferência no histórico local: [nao verificado]. Portanto, fecha a falta atual de registro, mas não comprova a frase “entra aqui no commit que mexe no código” (`app/docs/LEIS.md:146`). O corte pertinente em CORTES determina consultar `dia_sem_trabalho`, e essa consulta permanece no código (`app/docs/CORTES.md:416`; `app/ponto/services/cluster_espurio.py:25-26`). Existência do literal O169 também em CORTES: [nao verificado].

2. **Há uma generalização documental falsa sobre ata sem insumos.** O confronto das afirmações é:

   | Afirmação da L-125/ADR | Código da cópia |
   |---|---|
   | Não fecha por idade; consulta o próprio dia, a qualquer idade. | Correto para esse coletor: seleciona chamados vivos, obtém o dia pelo juiz e consulta essas datas sem janela (`app/ponto/services/cluster_espurio.py:66-76`). |
   | Assinatura presente conserva; medida sem assinatura permite fechar. | Correto: assinatura retorna tupla; ausência retorna `()`; o coletor converte para booleano e consulta a premissa (`app/ponto/services/cluster_espurio.py:31-37,74-77`; `app/chamados/catalogo/premissa.py:132-150`). |
   | Dia sem trabalho permite fechar. | Correto: o juiz é consultado primeiro e retorna `()` antes da validação dos insumos (`app/ponto/services/cluster_espurio.py:25-30`; `app/chamados/juizes.py:426-428`). |
   | “Célula ausente […] o chamado vive.” | Correto nesse coletor: falta de célula produz `None`, conservado pelo catálogo (`app/ponto/services/cluster_espurio.py:74-76`; `app/chamados/catalogo/premissa.py:127-130`). |
   | “Ata sem insumos = não medido, e o chamado vive.” | **Falso sem ressalva.** Com `trabalha=False, ata={}`, o juiz responde verdadeiro e o leitor retorna `()` antes de chegar ao ramo `None`; o fato fica `False` e a premissa morre (`app/docs/LEIS.md:146`; `app/docs/PLANO-PADROES.md:74`; `app/chamados/juizes.py:428`; `app/ponto/services/cluster_espurio.py:25-30,75-77`). |
   | Reconciliação *level-triggered*. | Compatível: compara o chamado vivo com o estado da célula, sem usar idade na decisão (`app/docs/PLANO-PADROES.md:75`; `app/ponto/services/cluster_espurio.py:66-77`). |
   | Trocar `None` por `False` fecha por falta de medida. | Correto quanto ao efeito; chamar isso de fechamento “por idade” é uma analogia: também afetaria dia recente sem medida (`app/docs/PLANO-PADROES.md:76`; `app/chamados/catalogo/premissa.py:127-134`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:86-94`). |

   A correção documental mínima é: **“Se o juiz não declarar dia sem trabalho, ata sem insumos conserva; dia sem trabalho prevalece.”** Essa é a ordem já implementada (`app/ponto/services/cluster_espurio.py:25-30`). “Fecha” deve ser entendido como elegibilidade no coletor: o ato ainda passa pelas guardas de reconciliação (`app/ponto/services/cluster_espurio.py:123-125`; `app/chamados/models.py:637-655`).

3. **O formato ADR está completo.** Lei, Contexto, Alternativas, Consequências, Padrão e Armadilha aparecem na seção L-125, conforme o formato exigido pela L-122 (`app/docs/PLANO-PADROES.md:36-42,70-76`; `app/docs/LEIS.md:143`). O padrão já existia no glossário; não nasceu desse ADR (`app/docs/GLOSSARIO.md:13`). Completude formal não resolve a imprecisão das Consequências apontada acima (`app/docs/PLANO-PADROES.md:74`).

4. **Não identifiquei proteção nominal duplicada das duas funções.** A entrada que as protege é a L-125; as demais células PROTEGE preenchidas nomeiam outros sítios (`app/docs/LEIS.md:117,129-130,140-146,148-151`). A consulta ao juiz de dia sem trabalho é complementar e continua respeitada (`app/docs/CORTES.md:416`; `app/ponto/services/cluster_espurio.py:25-26`). A regra do índice exige proteção expressa, sem deduzi-la de leis gerais (`app/docs/LEIS.md:43-48`).

5. **Os três itens permanecem FILA; nenhum demonstra defeito atual da cura.**

   - **Assinatura duplicada/15 literal:** existe duplicação do limiar, atualmente igual; o classificador agrupa órfãs, enquanto o serviço decide a assinatura do chamado (`app/escala/orfa_classe.py:28,40-48,74-77`; `app/ponto/services/cluster_espurio.py:31-36`).
   - **Caso 7 sem mordida específica:** procede. As listas vazias permitem retornar `()` mesmo removendo a consulta ao juiz (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:109`; `app/ponto/services/cluster_espurio.py:37`). Contudo, essa remoção global seria detectada pelos testes existentes de emissão com assinatura válida em folga/ausência (`app/chamados/tests/test_f1c4_3_emissores.py:113-128`). Permanece descoberto o mutante que verifica insumos **antes** do juiz: ele alteraria justamente `trabalha=False, ata={}`, hoje tratado corretamente (`app/ponto/services/cluster_espurio.py:25-30`). Isso não torna vacuosos os testes centrais de idade e conservação (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:74-100`).
   - **Contador de `None`:** falta observabilidade desse universo; o coletor retorna somente os candidatos a fechamento, e o vigia contabiliza abertura/fechamento (`app/ponto/services/cluster_espurio.py:74-78`; `app/ponto/management/commands/detectar_cluster_espurio.py:53-60`).

6. **Quem escreve e quem lê:**

   - **Autoria documental:** o ADR identifica a redação como do Code; os selos abaixo leem o índice, sem escrevê-lo (`app/docs/PLANO-PADROES.md:77`; `bin/tests/test_lei_protege_sitio.sh:49`; `bin/tests/test_leis_indice.sh:29`). Autoria histórica do commit: [nao verificado].
   - **Proteção de sítio:** lê PROTEGE, encontra funções por AST e exige L-125 se o diff tocar uma delas. Aceita citação em **qualquer commit do range**, sem exigir o mesmo commit da cura; não valida a semântica da lei nem o RED (`bin/tests/test_lei_protege_sitio.sh:77-87,160-177,179-204`). A L-125 não acrescenta proteção a outro arquivo (`app/docs/LEIS.md:146`).
   - **Índice:** verifica IDs de CORTES.json, colunas obrigatórias e IDs de lei duplicados; não confronta frases com código nem valida ADR/testes (`bin/tests/test_leis_indice.sh:28-35,49,63`). A cobertura é JSON→LEIS; o mapeamento novo não cria exigência inversa sobre CORTES (`app/docs/LEIS.md:387`; `bin/tests/test_leis_indice.sh:35`).
   - **Akita:** lê GLOSSARIO/PLANO, não a linha L-125. O ADR acrescenta o termo isolado *level-triggered* ao vocabulário capturado por itálico, podendo mudar a aceitação de `padrao=level-triggered` em outro commit; o termo completo já existia (`bin/tests/test_lei_akita.sh:95-108,138-140`; `app/docs/PLANO-PADROES.md:75`; `app/docs/GLOSSARIO.md:13`).
   - **Outros leitores:** o verificador lê LEIS; a publicação raspa seu conteúdo; o disjuntor reconhece o nome do anexo, sem julgar L-125 (`.claude/agents/verificador.md:8,15`; `bin/relato.sh:103-108`; `bin/raspar_publico.py:28-40`; `bin/disjuntor.py:1232,1363-1366`). Vereditos de push reais: [nao verificado].

**REPROVA. Bloqueante: corrigir a generalização “ata sem insumos vive” na L-125 e no ADR, explicitando a precedência de `dia_sem_trabalho`; o código já realiza essa precedência (`app/docs/LEIS.md:146`; `app/docs/PLANO-PADROES.md:74`; `app/ponto/services/cluster_espurio.py:25-30`).**

## 05/11 18:26 · sessao f4d60ab09e1e · PCCMP-OTIMIZA-3 · censo · origem=Code

HASH LIDO = d1354f77cf4c7214728709ad948145994a0c0366

PC1 e PC2 identificam os anexos transcritos na pergunta; a numeração dos arquivos originais desses anexos é **[nao verificado]**. As referências abaixo apontam para os sítios conferidos nesta cópia.

**1 COINCIDE**

| Item presente nas duas | Conferência no código |
|---|---|
| **Regra:** contar por faceta; cobertura desconhecida recebe “nao varrido”, preservando avulsos. | O contrato distingue arquivo e símbolo; `_faceta` publica contagem somente para tipos presentes em `VARRIDOS`. (`bin/quem_chama.py:10`, `bin/quem_chama.py:691`, `bin/quem_chama.py:697`) |
| **Caso:** importar função alterada produz achado de arquivo e de símbolo; importar somente função parada pertence ao arquivo. | O achado de arquivo é incondicional quando o módulo coincide; o de símbolo depende de `_sim` encontrar o nome entre os alterados. (`bin/quem_chama.py:408`, `bin/quem_chama.py:409`, `bin/quem_chama.py:411`, `bin/quem_chama.py:414`) |
| **Caso:** chamada Python embutida em shell impede concluir ausência pela falta de achado de símbolo. | Há chamada concreta em shell; o despacho shell atual produz apenas referências textuais da faceta arquivo. (`bin/placar_code.sh:384`, `bin/quem_chama.py:512`, `bin/quem_chama.py:674`) |
| **Casos:** heredoc, `-c`, comando Django, importação pelo pacote e shell sem extensão precisam integrar a discussão de cobertura. | Existem importações pelo pacote em comando inline e heredoc; o detector reconhece `.sh` e arquivo sem extensão com shebang. (`bin/placar_code.sh:484`, `bin/gate_cobranca.sh:47`, `bin/gate_cobranca.sh:50`, `bin/quem_chama.py:608`, `bin/quem_chama.py:612`) |
| **Casos:** biblioteca carregada e uso de filtro/tag são facetas distintas; comentários não contam. | O carregamento gera achado de arquivo; o uso registrado gera achado de símbolo; comentários são removidos preservando quebras de linha. (`bin/quem_chama.py:550`, `bin/quem_chama.py:565`, `bin/quem_chama.py:521`, `bin/quem_chama.py:523`) |
| **Casos:** métodos, settings, convenções, testes e decoradores não transparentes não recebem automaticamente cobertura completa. | Há classificações específicas fora da lista positiva. (`bin/quem_chama.py:295`, `bin/quem_chama.py:315`, `bin/quem_chama.py:317`, `bin/quem_chama.py:319`, `bin/quem_chama.py:323`) |
| **Caso:** símbolos apagados/renomeados continuam confrontados com leitores da ponta. | A montagem reúne símbolos da base e da ponta; o selo verifica leitores sobreviventes. (`bin/quem_chama.py:650`, `bin/tests/test_quem_chama.sh:172`, `bin/tests/test_quem_chama.sh:173`, `bin/tests/test_quem_chama.sh:175`) |
| **Caso:** mesmas revisões resolvidas determinam o conteúdo consultado, independentemente do disco sujo ou da hora. | As revisões são resolvidas para commits e os conteúdos vêm de `_ler_rev`. (`bin/quem_chama.py:120`, `bin/quem_chama.py:624`, `bin/quem_chama.py:625`) |
| **Sítios:** classificação → alvos/índices → scanners → conjuntos de achados → `_faceta` → apresentação/saída. | Essa cadeia está materializada na montagem dos tipos, índices, conjuntos e chamadas de apresentação. (`bin/quem_chama.py:637`, `bin/quem_chama.py:656`, `bin/quem_chama.py:661`, `bin/quem_chama.py:662`, `bin/quem_chama.py:713`, `bin/quem_chama.py:737`) |
| **Diff comum:** acrescentar reconhecimento de `{% filter nome %}`, preservar a biblioteca e corrigir negativas que não prendem a faceta. | A regex atual reconhece somente `\|`; `lib` alimenta o índice de bibliotecas; as negativas atuais examinam texto sem acompanhar o cabeçalho da faceta. (`bin/quem_chama.py:560`, `bin/quem_chama.py:657`, `bin/quem_chama.py:375`, `bin/tests/test_quem_chama.sh:159`, `bin/tests/test_quem_chama.sh:167`) |
| **Integração:** manter `_faceta` como contador, o selo existente e sua execução pela régua; não acrescentar juiz. | `_faceta` atualiza os contadores; o selo seleciona a ferramenta; a régua executa o glob de selos; o contrato declara “fato, não juiz”. (`bin/quem_chama.py:694`, `bin/quem_chama.py:698`, `bin/tests/test_quem_chama.sh:25`, `bin/regua.sh:138`, `bin/regua.sh:140`, `bin/quem_chama.py:6`) |
| **Contraexemplos comuns:** falha de AST pode desaparecer como ausência de achados; saída concorrente usa escrita direta; zero do instrumento não significa zero de produção. | `_parse` retorna `None`, o scanner retorna lista vazia, a saída abre em `w`, e qualquer `.py` pode ser chamador, inclusive teste. A L-111 exige chamador de produção. (`bin/quem_chama.py:191`, `bin/quem_chama.py:393`, `bin/quem_chama.py:742`, `bin/quem_chama.py:606`, `app/docs/LEIS.md:137`) |

**2 DIVERGE**

**D1 — Universo rebaixado: DESENHO.**

- **PC1/C:** propõe `ha_shell` na leitura da ponta e `_tipo_cobertura` aplicado às facetas Python antes cobertas; qualquer shell basta. Âncoras do diff: `bin/quem_chama.py:625`, `bin/quem_chama.py:656`, `bin/quem_chama.py:658`.
- **PC2/C:** propõe concatenar código shell e procurar qualquer componente literal de `mod.split('.')`; somente os alvos com coincidência lexical são rebaixados. Âncoras do diff: `bin/quem_chama.py:623`, `bin/quem_chama.py:652`, `bin/quem_chama.py:655`.
- **A cópia:** atribui diretamente `tipo_do_arquivo(rel)` e `_tipo_simbolo(...)`, sem propagação de exposição shell à cobertura. Depois, `if tipo in VARRIDOS` decide publicar N. Portanto as propostas divergem sobre **quais facetas podem afirmar zero**. (`bin/quem_chama.py:637`, `bin/quem_chama.py:656`, `bin/quem_chama.py:691`)
- Um shell contendo apenas `true` já acionaria PC1; PC2 dependeria de coincidência lexical. Um componente comum também pode rebaixar módulos diferentes em PC2, pois `_padrao_nome` verifica limites lexicais, não identidade de módulo. (`bin/quem_chama.py:347`, `bin/quem_chama.py:348`; predicados propostos nos anexos C)

**D2 — Ordem da classificação e motivos publicados: detalhe de diagnóstico.** [nao verificado]

- **PC1/C:** classifica o símbolo e depois rebaixa somente os tipos cobertos; preserva motivos já desconhecidos. Sítio substituído: `bin/quem_chama.py:656`.
- **PC2/C:** altera primeiro o tipo do arquivo e depois chama `_tipo_simbolo`. Sítios substituídos: `bin/quem_chama.py:655`, `bin/quem_chama.py:657`.
- **A cópia:** `_tipo_simbolo` devolve imediatamente tipos de arquivo fora dos três tipos Python admitidos. Em PC2, esse retorno substituiria os motivos específicos de método, setting, convenção, registro dinâmico e decorador pelo motivo shell. Essas facetas continuariam desconhecidas; muda a explicação e o agrupamento do resumo. (`bin/quem_chama.py:313`, `bin/quem_chama.py:314`, `bin/quem_chama.py:315`, `bin/quem_chama.py:319`, `bin/quem_chama.py:322`, `bin/quem_chama.py:720`)

**D3 — Expectativas do selo: DESENHO decorrente de D1.**

- **PC1/C:** troca expectativas de símbolo coberto por “nao varrido” e acrescenta uma consulta sem shells para recuperar expectativas de zero. Os sítios alterados são `bin/tests/test_quem_chama.sh:218`, `bin/tests/test_quem_chama.sh:223`, `bin/tests/test_quem_chama.sh:224`, `bin/tests/test_quem_chama.sh:231`, `bin/tests/test_quem_chama.sh:279`.
- **PC2/C:** mantém essas expectativas e acrescenta verificações dos módulos lexicalmente expostos. Os pontos de inserção são `bin/tests/test_quem_chama.sh:44` e `bin/tests/test_quem_chama.sh:210`.
- **A cópia:** exige atualmente filtro e símbolo de topo cobertos, inclusive zero para filtro não utilizado. Não são expectativas intercambiáveis entre os dois desenhos. (`bin/tests/test_quem_chama.sh:218`, `bin/tests/test_quem_chama.sh:223`, `bin/tests/test_quem_chama.sh:231`)

**D4 — Negativa de faceta: detalhe com acoplamento ao desenho.**

- **PC1/C:** propõe `awk` aceitando cabeçalho de arquivo contado ou não varrido. **PC2/C:** propõe Python exigindo `faceta.startswith('arquivo [')`, portanto apenas arquivo contado. Ambos substituem as negativas em `bin/tests/test_quem_chama.sh:159` e `bin/tests/test_quem_chama.sh:167`.
- **A cópia:** os formatos são distintos: arquivo contado contém colchetes; arquivo desconhecido começa com `arquivo: nao varrido`. O teste de PC2 rejeitaria uma saída desconhecida mesmo que o achado estivesse corretamente sob arquivo. (`bin/quem_chama.py:692`, `bin/quem_chama.py:697`)

**D5 — Comentário multilinha e posição posterior: detalhe da prova.**

- **PC1/C:** acrescenta uso válido depois do comentário e exige sua linha original. **PC2/C:** exige uso anterior ao comentário e ausência dentro dele; não prende a numeração posterior. Pontos de inserção/substituição: `bin/tests/test_quem_chama.sh:106`, `bin/tests/test_quem_chama.sh:107`.
- **A cópia:** preserva as quebras de linha dos comentários, mas a fixture existente fecha cada comentário na mesma linha. A prova proposta por PC1 é mais específica para a preservação posterior. (`bin/quem_chama.py:521`, `bin/quem_chama.py:523`, `bin/tests/test_quem_chama.sh:61`)

**D6 — Isolamento das formas shell: detalhe da prova, sem independência demonstrada.**

- **PC1/C:** põe as três formas sobre um único módulo; **PC2/C:** usa três módulos, todos sob o mesmo pacote. São acréscimos junto à montagem dos leitores em `bin/tests/test_quem_chama.sh:45`.
- No predicado de PC2, o componente compartilhado `ilha` permite que uma única forma rebaixe os três módulos; portanto três asserções positivas não demonstram separadamente as três formas. A identidade do módulo vem do caminho, e o predicado lexical usa os limites de `_padrao_nome`. (`bin/quem_chama.py:130`, `bin/quem_chama.py:133`, `bin/quem_chama.py:347`, `bin/quem_chama.py:638`)

As divergências por **omissão** estão discriminadas em 3; os dois diffs preservam o contador e o escritor de relatório existentes. (`bin/quem_chama.py:690`, `bin/quem_chama.py:737`, `bin/quem_chama.py:742`; propostas C dos anexos)

**3 SO NUMA**

**Só PC1:**

- **Contraexemplo de função sob condicional — DESENHO/universo:** o inventário recursa em classes, mas não em corpos de condicionais; uma definição alterada pode faltar como faceta. (`bin/quem_chama.py:257`, `bin/quem_chama.py:264`, `bin/quem_chama.py:266`, `bin/quem_chama.py:277`)
- **Contraexemplo de Python em configuração — DESENHO/universo:** a cura shell proposta não cobre configuração que recebe apenas busca textual. (`bin/quem_chama.py:610`, `bin/quem_chama.py:678`)
- **Contraexemplo de caminho com quebra de linha — DESENHO/universo:** `_ler_rev` exclui esses caminhos e a varredura ignora conteúdo ausente. (`bin/quem_chama.py:90`, `bin/quem_chama.py:668`)
- **Caso explícito de binário, vazio e alteração somente de modo — detalhe do inventário:** a lista de alvos vem de `--name-status`; não depende de haver linhas textuais alteradas. (`bin/quem_chama.py:143`, `bin/quem_chama.py:144`, `bin/quem_chama.py:621`)
- **Mudança explícita de `FORA` e controle sem shell no diff — respectivamente detalhe de documentação e prova do desenho global.** Os sítios atuais são a declaração apresentada no relatório e a transição para o smoke. (`bin/quem_chama.py:65`, `bin/quem_chama.py:706`, `bin/tests/test_quem_chama.sh:260`)

**Só PC2:**

- **Quantidades históricas específicas:** 39 alvos documentais, 12 achados de JSON, CSS N=3, biblioteca N=13 e filtro N=15. A confirmação dessas quantidades é **[nao verificado]**; classificação documental, carregamento e registro são conferíveis nos sítios citados. (`bin/quem_chama.py:308`, `app/templates/base.html:17`, `app/chamados/templatetags/chamados_tags.py:53`)
- **Caso de módulo simples de `bin/` — detalhe de abrangência:** `modulo_de` admite nome sem ponto, enquanto a lista textual só acrescenta o módulo quando contém ponto. (`bin/quem_chama.py:125`, `bin/quem_chama.py:133`, `bin/quem_chama.py:642`)
- **Caso explícito de JSON como chamador sem ser alvo coberto — detalhe de classificação:** JSON entra como configuração chamadora, mas seu tipo de alvo não está na lista positiva. (`bin/quem_chama.py:72`, `bin/quem_chama.py:308`, `bin/quem_chama.py:610`, `bin/quem_chama.py:691`)
- **Caso explícito de range/revisão/raiz ilegível — detalhe do contrato de erro:** falha Git gera `RuntimeError`, capturado como rc=2. (`bin/quem_chama.py:78`, `bin/quem_chama.py:119`, `bin/quem_chama.py:734`, `bin/quem_chama.py:736`)
- **Contraexemplo de sintaxe de template repartida em linhas — DESENHO/universo:** as buscas de carregamento, inclusão, estático e uso de filtro operam por linha. (`bin/quem_chama.py:528`, `bin/quem_chama.py:531`, `bin/quem_chama.py:541`, `bin/quem_chama.py:562`)
- **Contraexemplo explícito de nomes calculados e reexport — DESENHO/universo declarado:** são exclusões expressas que o diff não elimina. (`bin/quem_chama.py:65`, `bin/quem_chama.py:66`)

**4 CITACAO FALSA**

**Não identifiquei contradição demonstrada nos sítios funcionais citados. Há citações que não sustentam a parte quantitativa atribuída a elas:**

| Atribuição de PC2/A | O que a citação efetivamente sustenta |
|---|---|
| 39 alvos documentais e 12 achados do JSON. | Classificação e apresentação de cobertura; essas linhas não enumeram os alvos nem os achados históricos. Quantidades: **[nao verificado]**. (`bin/quem_chama.py:284`, `bin/quem_chama.py:308`, `bin/quem_chama.py:697`) |
| CSS N=3. | Um carregamento concreto e a regra do scanner; não o total histórico de três leitores. N=3: **[nao verificado]**. (`app/templates/base.html:17`, `bin/quem_chama.py:537`) |
| Biblioteca N=13; filtro N=15. | Declaração de cobertura e registro do filtro; não os totais históricos. N=13/N=15: **[nao verificado]**. (`bin/quem_chama.py:43`, `bin/quem_chama.py:53`, `app/chamados/templatetags/chamados_tags.py:53`) |

As referências originais a `avais.txt`, `otimiza3_reprovacoes.txt` e `pd_otimiza3.r`, assim como as verificações em memória alegadas pelos anexos, permanecem **[nao verificado]**. Ausência dessa evidência não demonstra falsidade dos resultados alegados.

A citação de PC2 a `bin/tests/test_quem_chama.sh:51` **não é falsa quanto a N=1 por faceta**: embora a fixture também contenha `mudada()`, o scanner externo não registra esse `Name` simples; registra a importação. (`bin/tests/test_quem_chama.sh:51`, `bin/quem_chama.py:414`, `bin/quem_chama.py:420`, `bin/quem_chama.py:429`)

## 05/11 18:25 · sessao e7c93d089ad0 · - · - · origem=Ronald (tmux)

Auditoria B consolidada, com os sítios abertos:

**Coincide**

- Os oito pontos literais de chamada de `carimbar` coincidem: `app/chamados/models.py:2674`; `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:620`; `app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/disputa_emissao.py:1675`; `app/chamados/services/materializacao.py:546`; `app/chamados/services/materializacao.py:1199`; `app/chamados/services/materializacao.py:1346`.
- O registro sustenta C6/C7/C15/C17/C18 e competência: `app/core/juizes.py:266`; `app/core/juizes.py:267`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/core/juizes.py:278`; `app/core/juizes.py:283`.
- `app/core/juizes.py:1285` realmente registra `vinculo_do_dia`: não é citação falsa. O lookup distingue endereço de autoridade e registro vivo em `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:11`.
- A tripla é atribuída em `_escrever_carimbo`; carimbo, identidade, restauração e descarimbo o chamam: `app/chamados/models.py:2545`; `app/chamados/models.py:2575`; `app/chamados/models.py:2608`; `app/chamados/models.py:2626`; `app/chamados/models.py:2628`; `app/chamados/models.py:2637`.
- Há escritores da via isolada: `app/chamados/models.py:2662`; `app/chamados/services/relavra_via.py:123`. Há atribuições de validação fora do serviço de validação: `app/chamados/models.py:2581`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/models.py:1493`.
- O memo omite ata/DNA na chave e reutiliza resultado por igualdade da chave: `app/chamados/juizes.py:811`; `app/chamados/juizes.py:837`. O salvamento agenda beco em `on_commit`: `app/chamados/signals.py:191`; `app/chamados/signals.py:202`.

**Só pc1 — sítios/grupos adicionais do censo**

- Instrumentos externos ao `app`: `bin/censo_folha.py:28`; `bin/censo_folha.py:29`; `bin/sombra_regras_pessoais.py:85`.
- Cobrança e flip: `app/chamados/services/cobranca.py:229`; `app/ponto/services/flip_auto.py:129`.
- Comandos: `app/ponto/management/commands/reconciliar_grade.py:461`; `app/chamados/management/commands/validar_classe_a.py:80`; `app/chamados/management/commands/auditar_disputas.py:71`; `app/chamados/management/commands/auditar_disputas.py:150`.
- Preparação direta dos campos da lavra no backfill, omitida do inventário de escritores de pc2: `app/chamados/management/commands/backfill_veredito_lavrado.py:138`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`; `app/chamados/management/commands/backfill_veredito_lavrado.py:140`.
- Outros seletores/consumidores: `app/chamados/reconciliador.py:1017`; `app/chamados/reconciliador.py:1433`; `app/chamados/services/materializacao.py:604`; `app/chamados/services/disputa_emissao.py:909`; `app/chamados/services/disputa_emissao.py:1152`; `app/chamados/services/fabrica_da_celula.py:138`; `app/chamados/services/acoes_disputa.py:95`; `app/chamados/services/declaracao_texto.py:635`; `app/chamados/services/painel.py:260`; `app/chamados/services/explicador.py:32`; `app/chamados/services/explicador.py:95`.
- Propriedade/reduções adicionais: `app/chamados/models.py:2807`; `app/chamados/juizes.py:1225`; `app/chamados/juizes.py:1257`.
- App/API/foto: `app/chamados/views.py:1280`; `app/chamados/views.py:1416`; `app/chamados/views_cobrar.py:176`; `app/api/views.py:1009`; `app/api/views.py:1187`; `app/api/foto_recebida.py:48`.
- Templates adicionais: `app/templates/chamados/partials/modal_fio.html:184`; `app/templates/chamados/partials/modal_fio.html:223`; `app/templates/chamados/partials/_card_atendimento.html:100`; `app/templates/chamados/partials/_card_atendimento.html:137`; `app/templates/chamados/partials/_card_atendimento.html:155`; `app/templates/chamados/partials/_pergunta_card.html:42`.
- I20, resposta sem instante: `app/chamados/management/commands/auditar_invariantes_chamados.py:1102`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1103`.

**Só pc2 — sítios/grupos adicionais do censo**

- Conferência humana escreve `data_conferida_em/por`, sem escrever `data_evento`, e libera o gate de materialização: `app/chamados/services/validacao.py:617`; `app/chamados/services/validacao.py:619`; `app/chamados/services/materializacao.py:1115`; `app/chamados/services/materializacao.py:1116`.
- Efeitos de exclusão `SET_NULL`: célula em `app/chamados/models.py:2198`; `app/chamados/models.py:2199`; destino em `app/chamados/models.py:2772`; `app/chamados/models.py:2774`.
- Emissores que alimentam `tipo_resposta`: `app/chamados/services/disputa_emissao.py:393`; `app/chamados/services/disputa_emissao.py:774`; `app/chamados/services/acoes_disputa.py:262`; `app/chamados/services/declaracao_texto.py:325`; `app/chamados/services/declaracao_texto.py:614`.
- Leitores adicionais: `app/chamados/templatetags/chamados_tags.py:128`; `app/templates/chamados/partials/_lote_preview.html:24`; `app/chamados/services/passivo_validado.py:515`; `app/chamados/views_cobrar.py:291`; `app/chamados/services/fio.py:882`; `app/chamados/services/historico.py:103`.
- Ampliação explícita dos chamadores de C6: `app/chamados/models.py:351`; `app/chamados/models.py:1447`; `app/chamados/models.py:1728`; `app/chamados/reconciliador.py:727`; `app/chamados/reconciliador.py:1054`; `app/chamados/services/competencia_trancada.py:122`; `app/chamados/services/pergunta_no_app.py:111`; `app/chamados/services/fio.py:775`; `app/chamados/services/vigia_fio.py:755`.
- Slot e fato do marco explicitados no inventário: `app/chamados/juizes.py:326`; `app/chamados/juizes.py:875`.

As diferenças de inventário acima ampliam o universo da prova apresentado por cada anexo; não demonstram implementações incompatíveis nesses consumidores. O caso da conferência de data acrescenta um insumo real ausente dos snapshots propostos: `app/chamados/services/validacao.py:617`; `app/chamados/services/materializacao.py:1116`.

**Citações sem sustentação semântica encontradas em B**

1. **pc1, leitores de `resposta_colab`: `app/chamados/juizes.py:277`.** O sítio pertence a `_retratada_com_trilha`, que consulta `materializada_em`, `pk` e auditoria; não lê a resposta nem deriva seu slot. O corpo completo confirma isso em `app/chamados/juizes.py:271`; `app/chamados/juizes.py:277`; `app/chamados/juizes.py:280`. O slot que lê a resposta está em `app/chamados/juizes.py:326`; `app/chamados/juizes.py:334`.

2. **pc1, grupo de auditorias: `app/chamados/management/commands/auditar_invariantes_chamados.py:1103`.** Não sustenta os quatro rótulos apresentados naquele grupo (“carimbo sem testemunha, materializada viva, via ausente e validação sem plantio”). É I20, texto de resposta preenchido com instante de resposta nulo, declarado em `app/chamados/management/commands/auditar_invariantes_chamados.py:1102`; filtro em `app/chamados/management/commands/auditar_invariantes_chamados.py:1103`. Trata-se de associação imprecisa dessa referência; outras referências do grupo sustentam seus próprios rótulos, por exemplo via ausente em `app/chamados/management/commands/auditar_invariantes_chamados.py:954`; validação sem materialização/recusa em `app/chamados/management/commands/auditar_invariantes_chamados.py:1072`.

Definições e linhas próximas que permanecem no mesmo sítio semântico não foram tratadas como falsas, como `_carimbar_validacao` em `app/chamados/services/validacao.py:127` e sua atribuição em `app/chamados/services/validacao.py:131`, ou `abrir` em `app/chamados/models.py:2387` e criação em `app/chamados/models.py:2465`.

## 05/11 18:25 · sessao 42d8407c5a99 · - · - · origem=Ronald (tmux)

Os três itens de FILA não demonstram bug atual na cura; há uma lacuna de teste na precedência entre “dia sem trabalho” e “ata sem insumos”. O código atual resolve corretamente essa precedência: pergunta ao juiz antes de validar os insumos. (`app/ponto/services/cluster_espurio.py:25`, `app/ponto/services/cluster_espurio.py:29`)

1. **Assinatura duplicada / 15 literal — FILA.** O classificador usa `JANELA_CLUSTER_MIN = 15` para agrupar rajadas; o serviço compara consecutivas com `15`. Ambos preservam o mesmo limiar, mas têm saídas distintas: classificação de órfãs versus assinatura que sustenta o chamado. (`app/escala/orfa_classe.py:28`, `app/escala/orfa_classe.py:44`, `app/escala/orfa_classe.py:75`, `app/ponto/services/cluster_espurio.py:35`)

2. **Caso 7 — lacuna real, sem invalidar todos os REDs.** Ele fornece listas vazias; retirar o juiz ainda produz `()` pelo retorno final, portanto esse caso isolado não verifica a consulta ao juiz. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:109`, `app/ponto/services/cluster_espurio.py:37`) Porém a remoção global da consulta seria detectada pelos testes existentes de emissão: folga e ausência possuem órfãs e lâmpada apagada, mas esperam nenhum achado. (`app/chamados/tests/test_f1c4_3_emissores.py:115`, `app/chamados/tests/test_f1c4_3_emissores.py:121`, `app/chamados/tests/test_f1c4_3_emissores.py:127`, `app/ponto/services/cluster_espurio.py:47`)

   Um mutante mais específico — validar os insumos antes do juiz — preservaria esses testes com listas e o caso 7, mas transformaria `trabalha=False, ata={}` em `None`, conservando indevidamente o chamado. Isso decorre da ordem atual dos dois ramos e das fixtures citadas; execução desse mutante: [nao verificado]. (`app/ponto/services/cluster_espurio.py:25`, `app/ponto/services/cluster_espurio.py:29`, `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:109`, `app/chamados/tests/test_f1c4_3_emissores.py:66`)

3. **Contador de fato `None` — FILA.** O coletor conserva `None`, mas retorna apenas a lista dos chamados cuja premissa morreu; o vigia conta pendências de abertura e fechamento, sem expor o universo não medido. (`app/ponto/services/cluster_espurio.py:74`, `app/ponto/services/cluster_espurio.py:78`, `app/ponto/management/commands/detectar_cluster_espurio.py:53`, `app/ponto/management/commands/detectar_cluster_espurio.py:60`)

**Bloqueante documental recomendado:** a generalização “ata sem insumos = não medido e o chamado vive” é falsa sem ressalvar a prioridade do juiz, principalmente no ADR, que não explicita essa exceção. (`app/docs/LEIS.md:146`, `app/docs/PLANO-PADROES.md:74`) Para `trabalha=False, ata={}`, o juiz responde verdadeiro, o leitor devolve `()`, o fato vira `False` e a premissa morre. (`app/chamados/juizes.py:428`, `app/ponto/services/cluster_espurio.py:25`, `app/ponto/services/cluster_espurio.py:75`, `app/chamados/catalogo/premissa.py:134`, `app/chamados/catalogo/premissa.py:150`)

A redação fiel precisa limitar a conservação por falta de insumos aos dias que o juiz **não** declarou sem trabalho; o código já realiza essa ordem. (`app/ponto/services/cluster_espurio.py:25`, `app/ponto/services/cluster_espurio.py:29`)

## 05/11 18:25 · sessao 0c77e9cba804 · PC3-PEP · censo · origem=Code

HASH LIDO = 9c0fd9d3e90f166cba697ec840422b5585a8ab09

## A CASOS PELA REGRA

O aval escolheu **candidata 1: dentro de `carimbar`, `atomic`, releitura travada e C6 rejulgando**; exige conferir `_pv_memo`, RED das intercalações e medir a ordem de trava antes do verde. `app/docs/PROMPTS.md:1882`.

Os cadastros abaixo são **casos de ensaio**, não ocorrências medidas em produção. Correspondência da cópia com objeto Git, execução dos casos e impacto produtivo: **[nao verificado]**. O gabarito deve nascer da regra e do cadastro, antes da implementação. `CLAUDE.md:45`; `app/docs/LEIS.md:136`.

| Caso | Cadastro + estado | Ato | Resposta exigida pela regra |
|---|---|---|---|
| A1 — decisão humana anterior à trava | Pergunta coletada sem resolução; admin decide o mesmo fato antes da tentativa automática. | Carimbar usando a instância antiga. | Preservar decisão, autoria e instante humanos; a intenção antiga não pode sobrescrevê-los. O caminho inverso já registra essa exigência. `app/chamados/services/relavra_via.py:214`; `app/chamados/models.py:2502`; `CLAUDE.md:32`. |
| A2 — resposta nova anterior à trava | Ato foi calculado sobre R1; colaborador grava R2 antes do carimbo. | Aplicar `resposta='validar'` com a intenção de R1. | Recusar a intenção obsoleta: validar R1 não autoriza validar R2. A resposta nova desfaz a validação anterior e conserva trilha. `app/chamados/services/disputa_emissao.py:1287`; `app/chamados/services/disputa_emissao.py:1303`. |
| A3 — resposta começa antes, grava depois | Escritor de resposta leu pergunta sem validação; outro ato valida/carimba enquanto o primeiro aguarda escrever. | Gravar a nova resposta depois do carimbo. | A nova resposta não pode herdar a validação da anterior; a propriedade deve valer também nessa intercalação. `app/chamados/services/disputa_emissao.py:1287`; `app/docs/CORTES.md:333`. |
| A4 — duas tentativas automáticas | Dois consumidores coletam a mesma pergunta sem carimbo. | Ambos tentam concluir o mesmo ato. | Um estado final coerente, sem substituição indevida de autoria/instante nem duplicata; a segunda tentativa precisa reconhecer a mudança persistida. `CLAUDE.md:175`; `app/docs/PROMPTS.md:1882`. |
| A5 — lâmpada ainda apagada | Célula de trabalho acusa o marco perguntado; nenhuma decisão humana ou prova daquele marco. | Tentar carimbo automático. | Não fabricar resolução: consumidor lê célula/ata, e carimbo não substitui o fato. `CLAUDE.md:166`; `app/docs/LEIS.md:95`; `app/docs/PROMPTS.md:1184`. |
| A6 — marco provado, outro furo | Lâmpada desta pergunta acesa; outro marco mantém o dia acusador. | Encerrar a pergunta do marco provado. | Encerrar esta pergunta e preservar a cobrança do outro marco. `app/chamados/models.py:2257`; `app/chamados/juizes.py:1173`. |
| A7 — antes e depois do teto | Célula provisoriamente não acusadora; marco perguntado pendente, sem batida. | Tentar concluir; depois chegar a batida e rejulgar. | Antes, não absolver pelo veredito provisório; depois da prova, reconhecer o fato na passada seguinte. `app/chamados/juizes.py:1185`; `app/docs/CORTES.md:413`. |
| A8 — competência 21→20 | Empresa com corte cadastrado 21; fatos em 20/10 e 21/10. | Determinar a competência usada na tranca. | 20/10 pertence à competência 10; 21/10 pertence à 11. Outro corte cadastrado exige outra janela, sem 21 cravado no consumidor. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/ponto/janelas.py:60`. |
| A9 — trancar/destrancar | Competência trancada, sem resolução independente do fato; depois reaberta. | Ler a pergunta e tentar encerramento automático. | Tranca silencia por leitura, sem fabricar batida ou quitação permanente; destrancar devolve a pergunta conforme o fato. `app/chamados/juizes.py:920`; `app/chamados/models.py:2514`. |
| A10 — meia-noite | Vínculo 12x36 19–07, intervalo 23–00, turno iniciado em D. | Consumir resposta de volta 00:00. | A volta ocorre em D+1; sua data sai da cadeia de marcos do vínculo do dia, preservando o turno iniciado em D. `app/chamados/juizes.py:285`. |
| A11 — vínculo trocado | Célula histórica gerada por A, vigente em D; hoje B está ativo. | Julgar/carimbar o fato de D. | A geradora A manda dentro da vigência. Fora dela, declarar desconhecimento/CADASTRO, sem substituir silenciosamente por B. `app/escala/alimentacao.py:228`; `app/escala/alimentacao.py:272`. |
| A12 — célula sem ata | Célula de trabalho sem lâmpadas lavradas; separar ausência de veredito de veredito não acusador. | Concluir porque “não há lâmpada apagada”. | Ausência de prova não prova resolução. A regra local declara ata nunca lavrada como “não sei”; a divergência do ramo atual do C6 consta em D. `app/chamados/juizes.py:473`; `app/chamados/juizes.py:878`. |
| A13 — identidade com voz pendente | Resposta não validada, discordante da luz; pergunta recebe identidade de duplicata. | Ler a vitalidade. | Identidade não apaga a voz ainda sujeita à decisão administrativa, ressalvada decisão humana explícita sobre o mesmo dia. `app/chamados/models.py:2615`; `app/chamados/juizes.py:1150`. |
| A14 — ausência parcial | Ata de ausência parcial; marco fora do período coberto. | Absorver perguntas do dia pela ausência. | Cobrar o resto descoberto; ausência parcial não dispensa o dia inteiro. `app/docs/JUIZES.md:58`. |
| A15 — erro após julgamento | Candidato aceito pelo C6; lavra ou salvamento falha. | Devolver erro ao chamador. | Memória não pode declarar carimbo/lavra que não foram gravados. `app/chamados/models.py:2561`; `app/chamados/models.py:2599`; `app/chamados/veredito_lavrado.py:240`. |
| A16 — antiguidade | Fato antigo, lâmpada ainda acusadora, sem resolução humana ou testemunha do marco. | Encerrar apenas pela idade ou pelo carimbo. | Idade não apaga a dívida do fato. `app/docs/PROMPTS.md:1184`; `CLAUDE.md:170`. |

## B SITIOS

**Fonte da intervenção:** `PerguntaDisputa.carimbar`; o atribuidor da tripla continua sendo `_escrever_carimbo`. O corpo atual julga e grava a instância recebida, sem releitura travada. `app/chamados/models.py:2545`; `app/chamados/models.py:2551`; `app/chamados/models.py:2585`; `app/chamados/models.py:2598`.

**Não mover o corpo para outra função escritora:** a catraca autoriza nominalmente `carimbar` a chamar `_escrever_carimbo` e `_desfazer_em_memoria`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:373`.

| Pergunta | Autoridade existente |
|---|---|
| A pergunta está viva? | C6, `pergunta_viva`. `app/core/juizes.py:266`. |
| A coleção tem fio vivo? | C15, `fio_vivo`. `app/core/juizes.py:275`. |
| O dia tem porta alcançável? | C17, `porta_alcancavel_do_dia`. `app/core/juizes.py:277`. |
| O dia dispensa cobrança? | C18, `dia_sem_trabalho`. `app/core/juizes.py:278`. |
| Qual competência contém o fato? | `janela_atual`, lendo o corte empresarial. `app/core/juizes.py:283`; `app/ponto/janelas.py:60`. |
| Qual vínculo responde pelo dia? | `vinculo_do_dia`, respeitando geradora e vigência. `app/escala/alimentacao.py:228`. |
| Quem valida a resposta? | `validar_pergunta`, conforme lookup; o censo precisa registrar também o preenchimento condicional existente em `carimbar`. `app/docs/JUIZES.md:38`; `app/chamados/models.py:2580`. |
| Há testemunha da materialização? | Auxiliar existente `_pergunta_materializada_de_fato`, também consumido pela lavra; não confundir com vitalidade. `app/chamados/juizes.py:632`; `app/chamados/veredito_lavrado.py:219`. |

JUIZES.md é lookup; o registro vivo prevalece. Não há necessidade de juiz novo para a pergunta desta obra. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`; `app/core/juizes.py:266`; `app/docs/PROMPTS.md:1882`.

**Escritores e leitores dos campos centrais:**

| Campo | Escritores/portas | Leitores |
|---|---|---|
| `materializada_em/por`, `via_resolucao` | Atribuidor: `app/chamados/models.py:2545`; carimbo comum: `:2551`; identidade: `:2614`; inversos: `:2635`, `:2652`. Via isolada: `app/chamados/models.py:2662`; `app/chamados/services/relavra_via.py:123`. | C6: `app/chamados/juizes.py:1171`, `:1177`, `:1207`; estado/SQL: `app/chamados/models.py:2488`, `:2735`; procedência: `app/ponto/services/procedencia_batida.py:71`; materialização: `app/chamados/services/materializacao.py:431`, `:529`, `:1341`. |
| `resposta_colab`, `respondida_em` | Humana: `app/chamados/services/disputa_emissao.py:1282`; automática: `:1428`; recuperação: `:1445`; correção: `:1393`; ressuscitação: `app/chamados/models.py:1493`; preenchimento do instante pelo carimbo: `app/chamados/models.py:2576`. | C6/concordância: `app/chamados/juizes.py:1113`, `:1150`; validação: `app/chamados/services/validacao.py:363`; plantio: `app/chamados/services/materializacao.py:1183`; API: `app/api/views.py:2580`, `:2636`; calendário/ficha/folha: `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`. |
| `validada_em/por` | Validação/devolução: `app/chamados/services/validacao.py:131`, `:188`; carimbo comum: `app/chamados/models.py:2581`; resposta nova/correção: `app/chamados/services/disputa_emissao.py:1295`, `:1393`; ressuscitação: `app/chamados/models.py:1493`; restauração: `app/chamados/models.py:2609`. | C6: `app/chamados/juizes.py:1136`; estado: `app/chamados/models.py:2488`; materializador: `app/chamados/services/materializacao.py:1292`; procedência: `app/ponto/services/procedencia_batida.py:72`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | Lavra/restauração/lote: `app/chamados/veredito_lavrado.py:227`, `:243`, `:207`; backfill em memória: `app/chamados/management/commands/backfill_veredito_lavrado.py:138`. | Frescor: `app/chamados/veredito_lavrado.py:173`; raio-X: `app/chamados/services/raiox.py:173`; passivo: `app/chamados/services/passivo_validado.py:64`. |
| `_pv_memo` | Gravação: `app/chamados/juizes.py:847`; descarte: `app/chamados/models.py:2612`, `:2638`. | Chave e reutilização: `app/chamados/juizes.py:811`, `:836`. |

**Insumos cuja identidade a proposta compara antes de executar a intenção:**

| Insumo | Fontes/escritores | Leitores relevantes |
|---|---|---|
| `motivo`, `tipo_resposta` | Catálogo/emissão/nascimento: `app/chamados/catalogo/perguntas.py:36`; `app/chamados/services/disputa_emissao.py:391`; `app/chamados/models.py:2465`. | Marco/plantio/validação: `app/chamados/juizes.py:886`; `app/chamados/services/materializacao.py:305`, `:1143`; `app/chamados/services/disputa_emissao.py:1257`. |
| `data_evento` | Nascimento: `app/chamados/models.py:2465`; preenchimento de data ausente: `app/chamados/services/fio.py:832`. Conferência humana grava autorização, não outra data: `app/chamados/services/validacao.py:617`. | Meia-noite/janela/plantio: `app/chamados/juizes.py:285`, `:1197`; `app/chamados/services/materializacao.py:345`. |
| `celula_id` | Nascimento/reaproveitamento: `app/chamados/models.py:2419`, `:2425`, `:2463`; FK com `SET_NULL`: `:2198`. | C6 e validação da alimentação: `app/chamados/juizes.py:1147`, `:621`. |
| `disputa_id` | Nascimento/FK obrigatória: `app/chamados/models.py:2465`, `:2200`; outra reatribuição persistida **[nao verificado]**. | Pessoa/âncora/fila: `app/chamados/juizes.py:1200`; `app/chamados/services/fio.py:116`; `app/chamados/services/disputa_emissao.py:1575`. |
| `chamado_destino_id` | Nascimento/dedup: `app/chamados/models.py:2466`, `:2439`; reapontamento: `app/chamados/services/fio.py:845`. | Listagem/plantio/via: `app/chamados/services/fio.py:117`; `app/chamados/services/materializacao.py:761`, `:1185`. |

**Todos os oito pontos de chamada do carimbo comum encontrados:**

| Chamador | Sítio |
|---|---|
| `encerrar_fantasma` | `app/chamados/models.py:2674`. |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:405`. |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:620`. |
| `resolver_perguntas_por_chamado_resolvido`, dois ramos | `app/chamados/services/disputa_emissao.py:1603`; `:1675`. |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:546`. |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1199`. |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1346`. |

As portas irmãs de identidade têm quatro chamadores: decisão administrativa, duplicata, paridade e colisão. Os inversos abrangem ressuscitação, reabertura, resposta nova e desfazimento sem lastro. `app/chamados/models.py:2511`; `app/chamados/services/fio.py:891`; `app/chamados/services/materializacao.py:891`; `:1066`; `app/chamados/models.py:1444`; `:1493`; `:2685`; `:2702`; `:2709`; `app/chamados/services/disputa_emissao.py:1412`; `app/chamados/services/relavra_via.py:231`.

**Demais leitores diretos encontrados**, além dos já associados aos campos:

| Grupo | Sítios |
|---|---|
| Fila, apresentação e ações | `app/chamados/canal.py:65`; `app/chamados/relatorios_disputa.py:21`; `app/chamados/views.py:1257`; `app/chamados/views_cobrar.py:169`; `app/chamados/templatetags/chamados_tags.py:128`; `app/chamados/services/acoes_disputa.py:95`; `:130`; `:341`; `app/chamados/services/verbos.py:240`; `:311`. |
| Emissão e cobrança | `app/chamados/services/pergunta_no_app.py:53`; `:111`; `app/chamados/services/aviso_pendencia.py:13`; `app/chamados/services/becos.py:30`; `app/chamados/services/cobranca.py:229`; `app/chamados/services/competencia_trancada.py:251`; `app/chamados/services/fabrica_da_celula.py:119`; `:138`; `app/chamados/services/furo_sem_canal.py:81`; `app/chamados/services/disputa_emissao.py:695`; `:909`; `:929`; `:1152`; `:1481`; `:1596`; `:1657`. |
| Explicação, histórico e painel | `app/chamados/services/declaracao_texto.py:211`; `:366`; `:635`; `app/chamados/services/explicador.py:32`; `:79`; `app/chamados/services/historico.py:99`; `:103`; `app/chamados/services/painel.py:67`; `:260`; `app/chamados/services/raiox.py:31`; `:133`. |
| Fio, passivo e vigilância | `app/chamados/services/fio.py:336`; `:656`; `:847`; `app/chamados/services/passivo_validado.py:140`; `:515`; `app/chamados/services/regularizacao_ext.py:198`; `:219`; `app/chamados/services/relavra_via.py:116`; `:153`; `:179`; `:229`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/vigia_fio.py:675`; `:783`. |
| Outros ramos de materialização e validação | `app/chamados/services/materializacao.py:278`; `:305`; `:356`; `:400`; `:467`; `:604`; `:938`; `:962`; `:1023`; `app/chamados/services/validacao.py:139`; `:180`; `:315`; `:451`; `:534`; `:541`; `:685`; `:706`; `:806`; `:877`. |
| API e outros domínios | `app/api/views.py:1009`; `:1173`; `:1187`; `app/api/views_mensageria.py:567`; `app/ponto/services/cartorio.py:1590`; `:1599`; `app/ponto/services/dia_do_colab.py:69`; `app/ponto/services/flip_auto.py:129`; `app/ponto/supra_juiz.py:545`. |
| Auditorias | `app/chamados/management/commands/auditar_disputas.py:71`; `app/chamados/management/commands/auditar_invariantes_chamados.py:177`; `:433`; `:538`; `:588`; `:905`; `:955`; `:1073`; `:1103`; `:1154`; `:1170`; `:1569`; `:1599`; `:1652`; `:1707`; `:1763`. |
| Comandos consumidores | `app/chamados/management/commands/backfill_veredito_lavrado.py:120`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`; `app/chamados/management/commands/dedup_perguntas_fato.py:71`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`; `:66`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`; `:129`; `app/chamados/management/commands/relavrar_via_i6.py:58`; `app/chamados/management/commands/validar_classe_a.py:80`; `app/ponto/management/commands/reconciliar_fantasmas.py:212`; `app/ponto/management/commands/reconciliar_grade.py:461`. |
| Templates | `app/templates/chamados/partials/_pergunta_card.html:20`; `:29`; `:42`; `app/templates/chamados/partials/_card_atendimento.html:94`; `:100`; `:108`; `:135`; `:142`; `:148`; `:155`; `:176`; `:186`; `:384`; `app/templates/chamados/partials/_lote_preview.html:24`; `app/templates/chamados/modal_fio.html:184`; `:212`; `:223`. |

O alcance dinâmico além dessas ocorrências — SQL construído, reflexão ou consumidores externos à cópia — é **[nao verificado]**.

## C DIFF PROPOSTO EM TEXTO

A proposta mantém o corpo escritor em `carimbar`, acrescenta transação/releitura e recusa intenção cujo estado mudou. A comparação é necessária porque **C6 morto não significa autorização para sobrescrever uma decisão anterior**: validação encerra no primeiro degrau, e a via candidata é aplicada antes do julgamento. `app/chamados/juizes.py:1135`; `app/chamados/models.py:2575`; `app/chamados/services/relavra_via.py:214`.

Mantêm-se `_escrever_carimbo`, a lavra existente e suas restaurações; não nasce outra porta ou juiz. O padrão é **Single Writer Principle**. Esta proposta ainda tem os impedimentos de fechamento descritos em D. `app/chamados/models.py:2545`; `app/chamados/veredito_lavrado.py:235`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:373`; `app/docs/GLOSSARIO.md:12`.

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/models.py
@@ -2571,33 +2571,45 @@
         autor_id = getattr(autor, 'pk', None)
         antes = (self.materializada_em, self.materializada_por_id, self.via_resolucao,
                  self.respondida_em, self.validada_em, self.validada_por_id)
-        campos = ['materializada_em', 'materializada_por', 'via_resolucao']
-        self._escrever_carimbo(quando, autor_id, via)
-        if resposta and not texto_vazio(self.resposta_colab):
-            if self.respondida_em is None:
-                self.respondida_em = quando
-                campos.append('respondida_em')
-            if resposta == 'validar' and self.validada_em is None:
-                self.validada_em = quando
-                self.validada_por = autor if autor_id is not None else None
-                campos += ['validada_em', 'validada_por']
-        try:
-            viva = pergunta_viva(self)
-        except Exception:
-            self._desfazer_em_memoria(antes)
-            raise
-        if viva:
-            self._desfazer_em_memoria(antes)
-            return False
+        campos_do_ato = (
+            'materializada_em', 'materializada_por_id', 'via_resolucao',
+            'respondida_em', 'validada_em', 'validada_por_id', 'resposta_colab',
+            'motivo', 'data_evento', 'tipo_resposta', 'disputa_id',
+            'celula_id', 'chamado_destino_id',
+        )
         from chamados import veredito_lavrado as _vl
         lavra_antes = _vl.foto_da_lavra(self)
+        self.__dict__.pop('_pv_memo', None)
         try:
-            if lavrar_veredito:
-                _vl.lavrar(self, salvar=False)
-                campos += ['veredito_lavrado', 'julgado_em', 'janela_expira_em']
-            self.save(update_fields=campos)
+            with transaction.atomic():
+                atual = (PerguntaDisputa.objects
+                         .select_for_update(of=('self', 'disputa'))
+                         .select_related('disputa', 'celula').get(pk=self.pk))
+                if any(getattr(atual, c) != getattr(self, c) for c in campos_do_ato):
+                    return False
+                if (atual.materializada_em is not None
+                        and atual.via_resolucao in self.vias_que_encerram_por_identidade()):
+                    return False
+                self.refresh_from_db()
+                campos = ['materializada_em', 'materializada_por', 'via_resolucao']
+                self._escrever_carimbo(quando, autor_id, via)
+                if resposta and not texto_vazio(self.resposta_colab):
+                    if self.respondida_em is None:
+                        self.respondida_em = quando
+                        campos.append('respondida_em')
+                    if resposta == 'validar' and self.validada_em is None:
+                        self.validada_em = quando
+                        self.validada_por = autor if autor_id is not None else None
+                        campos += ['validada_em', 'validada_por']
+                if pergunta_viva(self):
+                    self._desfazer_em_memoria(antes)
+         

[truncado em 20000 de 26583 caracteres]

## 05/11 18:25 · sessao a2f4c7a37e99 · - · - · origem=Ronald (tmux)

Q6 — leitores e efeito:

- `test_lei_protege_sitio.sh` lê LEIS do disco, extrai apenas PROTEGE e encontra funções por AST; L-125 acrescenta os dois sítios de `cluster_espurio.py`, sem proteger outro arquivo. Se um hunk tocar essas funções, exige citação L-125 em algum commit do range, não necessariamente no commit da cura (`bin/tests/test_lei_protege_sitio.sh:49`, `:77`, `:83`, `:160`, `:175`, `:180`, `:200`).
- O selo não valida conteúdo da lei, ADR, existência do teste ou autenticidade do RED; seu juiz compara leis tocadas e citadas (`bin/tests/test_lei_protege_sitio.sh:188`, `:200`). Sem base Git, ainda verifica existência dos sítios, mas pula o julgamento de toque (`bin/tests/test_lei_protege_sitio.sh:164`, `:168`).
- `test_leis_indice.sh` confere cobertura dos IDs de CORTES.json, preenchimento das colunas e unicidade dos L-NNN (`bin/tests/test_leis_indice.sh:28`, `:35`, `:49`, `:63`). O novo mapeamento é `CLUSTER-NAO-FECHA-POR-IDADE → L-125` (`app/docs/LEIS.md:387`), mas esse identificador não consta como ID em CORTES.json [nao verificado por referência positiva; busca direta da cópia]. A conferência só vai JSON→LEIS, portanto não exige que esse novo mapeamento corresponda a corte registrado nem que a lei indicada exista (`bin/tests/test_leis_indice.sh:35`).
- `test_lei_akita.sh` lê GLOSSARIO e PLANO, não a linha L-125: extrai termos e julga `padrao=` dos commits (`bin/tests/test_lei_akita.sh:89`, `:95`, `:102`, `:124`). O termo completo já existia (`app/docs/GLOSSARIO.md:13`); o ADR acrescenta `level-triggered` isolado ao vocabulário pelo itálico (`app/docs/PLANO-PADROES.md:75`; `bin/tests/test_lei_akita.sh:105`). Isso pode habilitar esse termo isolado em outro commit; não impõe regra a outro arquivo (`bin/tests/test_lei_akita.sh:138`, `:185`).
- A régua executa os selos de host; pre-push chama proteção e Akita diretamente (`bin/regua.sh:138`; `bin/pre-push.sh:16`, `:38`). Instalação e execução reais no host [nao verificado].
- O verificador recebe instrução para ler LEIS e confrontar os sítios (`.claude/agents/verificador.md:8`, `:15`). O disjuntor reconhece apenas nome de anexo e volta (`bin/disjuntor.py:1232`, `:1363`).
- `relato.sh` lê LEIS para publicar uma cópia raspada; o raspador escreve o destino, não a fonte (`bin/relato.sh:103`, `:108`; `bin/raspar_publico.py:28`, `:40`). Autoria humana da L-125: somente pelo anexo fornecido [nao verificado no histórico local].

Ressalva temporal: proteção aceita citação em qualquer commit do range; não prova a exigência de registro no mesmo commit da cura (`bin/tests/test_lei_protege_sitio.sh:180`, `:200`; `app/docs/LEIS.md:146`).

## 05/11 18:24 · sessao 425e665f8129 · - · - · origem=Ronald (tmux)

1. **O registro pedido existe e cita o literal correto.** L-125 reproduz o corte de `BACKLOG.md:424-425`, protege as duas funções reais e aponta para a classe de teste existente: `app/docs/LEIS.md:146`; `app/ponto/services/cluster_espurio.py:15,53`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:39`. Os sete casos estão em `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:74-111`. O prazo “no commit que mexer no código” está em `app/docs/BACKLOG.md:421-423`; cumprimento temporal fora dos anexos: [nao verificado].

2. **CORTES não contradiz esse literal.** O literal O169 foi encontrado no BACKLOG; não foi encontrado em `CORTES.md`/`CORTES.json` [nao verificado por arquivo:linha para ausência]. O corte pertinente ao juiz já usado determina que cluster consulte `dia_sem_trabalho`: `app/docs/CORTES.md:416`; o código consulta esse juiz antes de examinar insumos: `app/ponto/services/cluster_espurio.py:21-30`.

3. **ADR completo e padrão preexistente.** A exigência está em `app/docs/LEIS.md:143` e `app/docs/PLANO-PADROES.md:36-42`. Os seis campos — Lei, Contexto, Alternativas, Consequências, Padrão e Armadilha — aparecem em `app/docs/PLANO-PADROES.md:70-76`. “Reconciliação level-triggered” já está no glossário: `app/docs/GLOSSARIO.md:13`.

4. **Não há outra proteção nominal dessas funções na tabela.** A única entrada PROTEGE de `cluster_espurio.py` é L-125: `app/docs/LEIS.md:146`. As outras entradas preenchidas protegem funções diferentes: `app/docs/LEIS.md:117,129,130,140,141,145,148-151`. A regra distingue proteção expressa de inferência e diz que célula vazia não protege código: `app/docs/LEIS.md:43-48`. Portanto, não identifiquei lei duplicada ou conflitante nas mesmas funções: `app/docs/LEIS.md:146`; `app/docs/CORTES.md:416`.

**Bloqueantes nas perguntas 1/3/4: nenhum quanto ao conteúdo e à estrutura presentes.** A alegação histórica de entrada no mesmo commit não é comprovável pela árvore sem Git; os anexos mostram commits separados [nao verificado além do dado fornecido]. [nao verificado]

## 05/11 18:24 · sessao 822a7923d62d · - · - · origem=Ronald (tmux)

Censo da abertura/evento, somente pela cópia:

- `ChamadoColaborador.abrir` é a fonte comum dos três emissores de `batida_ausente`: detector em `app/ponto/management/commands/detectar_ausencias.py:693`, grade em `app/ponto/management/commands/reconciliar_grade.py:507`, C2 em `app/chamados/reconciliador.py:879`; os três emissores também estão nomeados no corte em `app/docs/CORTES.md:360`.
- Abertura serializa dedup+nascimento pela linha do colaborador, dentro de `atomic`; o retorno de existente/renascido acontece antes do trecho de evento, portanto acrescentar `batida_ausente` somente no filtro atual cobriria nascimento real, não reabertura nem dedup existente (`app/chamados/models.py:863`, `app/chamados/models.py:882`, `app/chamados/models.py:885`, `app/chamados/models.py:932`, `app/chamados/models.py:938`).
- O filtro do evento atual é `MODULOS_DE_TURNO`, que contém apenas `turno_aberto`, `turno_aberto_24h`, `entrada_bloqueada_turno_aberto`; não contém `batida_ausente` (`app/chamados/reconciliador.py:336`). A porta `criar` não grava evento e não tem `atomic` envolvendo nascimento+evento (`app/chamados/models.py:1015`, `app/chamados/models.py:1048`).
- Sinal de nascimento/mudança de estado é insuficiente: roda sob `engolir`, sai em `em_julgamento` e chama julgamento, não entrega diretamente pergunta (`app/chamados/signals.py:165`, `app/chamados/signals.py:172`, `app/chamados/signals.py:174`, `app/chamados/signals.py:180`).
- Detector tem escritor adicional por agrupador de órfãos depois da abertura, em tentativa que absorve falha; grade retorna sem emitir pergunta; C2 reaponta pela porta (`app/ponto/management/commands/detectar_ausencias.py:787`, `app/ponto/management/commands/detectar_ausencias.py:801`, `app/ponto/management/commands/detectar_ausencias.py:802`, `app/ponto/management/commands/reconciliar_grade.py:531`, `app/chamados/reconciliador.py:922`).

O caminho de evento existente:

- `emitir_evento_celula` é o escritor de `EventoCelula`, e `salvar_e_emitir` junta gravação da célula e evento no mesmo `atomic`; só `ata`/`veredito` acordam o consumidor (`app/ponto/portas/celula.py:661`, `app/ponto/portas/celula.py:664`, `app/ponto/portas/celula.py:669`, `app/ponto/portas/celula.py:672`, `app/ponto/portas/celula.py:677`).
- Consumidor atual não emite perguntas: chama retratação de chamados absolvidos e encerramento de órfãs; falha mantém pendente, teto pode adiar, sucesso marca processado (`app/chamados/reconciliador.py:1326`, `app/chamados/reconciliador.py:1329`, `app/chamados/reconciliador.py:1339`, `app/chamados/reconciliador.py:1341`, `app/chamados/reconciliador.py:1346`). Portanto, **somente ampliar o filtro de nascimento para `batida_ausente` não resolve** (`app/chamados/models.py:939`, `app/chamados/reconciliador.py:1326`).
- Não colocar fábrica síncrona sob a trava do colaborador em `abrir`: a ordem declarada da casa é Disputa → Colaborador; fábrica chama abertura/agregação de disputa, e consumidor deliberadamente evita manter essas travas externamente (`app/chamados/models.py:843`, `app/chamados/services/disputa_emissao.py:598`, `app/chamados/reconciliador.py:1290`).

Por que “chamar `julgar_celula` nos seis atos” ainda não entrega o contrato:

1. Hoje pode retornar antes do fim do turno sem `teto_de_hoje`; célula inexistente também retorna (`app/ponto/services/cartorio.py:1976`, `app/ponto/services/cartorio.py:1982`, `app/ponto/services/cartorio.py:1986`).
2. Fábrica só é chamada para datas efetivamente julgadas na passada; impressão igual pode impedir sua passagem (`app/ponto/services/cartorio.py:982`, `app/ponto/services/cartorio.py:995`).
3. `motivos_a_perguntar` exclui qualquer identidade histórica não duplicata, independentemente de C6 vivo/morto; despertar a mesma fábrica não é prova de cura das cinco perdas (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`).
4. Forçar julgamento amplia o alcance de relavra, inclusive nas competências anteriores; não é um mero wakeup (`app/ponto/services/cartorio.py:199`, `app/ponto/services/cartorio.py:204`, `app/ponto/services/cartorio.py:215`).

Autoridades para a seção C:

- Morte por leitura **já tem juiz**, C6=`pergunta_viva`; C15=`fio_vivo` apenas reduz a coleção; C17 responde alcance (`app/core/juizes.py:266`, `app/core/juizes.py:275`, `app/core/juizes.py:277`, `app/chamados/juizes.py:1233`, `app/chamados/juizes.py:1242`).
- C6 cobre validação, tranca, identidade, marco próprio satisfeito, veredito da célula e demais degraus; não usar “materializada != NULL” como equivalente (`app/chamados/juizes.py:1135`, `app/chamados/juizes.py:1140`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1177`, `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1207`).
- “C11=(i)” do aval não é o C11 técnico do registro: este responde ausência/silêncio e aponta `silencio_ausencia.reconciliar_por_ausencia` (`app/docs/PROMPTS.md:1875`, `app/core/juizes.py:117`, `app/core/juizes.py:271`).
- Não há impedimento normativo demonstrado para usar os juízes existentes; o impedimento de um diff minúsculo é implementação/cobertura, não necessidade demonstrada de juiz novo (`app/core/juizes.py:260`, `app/docs/PROMPTS.md:1882`).

Sítios de cron/instrumento que a obra precisa reconciliar:

- Fonte executora continua `--comp-corrente --apply` (`app/config/crons.py:1495`).
- Ramo FIO MUDO lê qualquer módulo vivo com célula na janela, não só `batida_ausente`; filtra vida, parcial, motivo vencido, ativo e identidade histórica (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/chamados/management/commands/disparar_perguntas_competencia.py:148`, `app/chamados/management/commands/disparar_perguntas_competencia.py:154`, `app/chamados/management/commands/disparar_perguntas_competencia.py:158`, `app/chamados/management/commands/disparar_perguntas_competencia.py:169`, `app/chamados/management/commands/disparar_perguntas_competencia.py:211`).
- Escritor atual é `fabrica_da_celula.perguntar`, chamado pelo comando em `app/chamados/management/commands/disparar_perguntas_competencia.py:280`; fábrica por célula usa o mesmo escritor em `app/chamados/services/fabrica_da_celula.py:176`.
- `--alarme` já é caminho sem escrita; o cron deve passar a ele após implementação/cobertura, com exceção e lista de juiz por varredura coerentes (`app/chamados/management/commands/disparar_perguntas_competencia.py:251`, `app/config/crons.py:2007`, `app/config/crons.py:2011`, `bin/tests/test_papel_prazo_nao_deriva.sh:303`, `bin/tests/test_papel_prazo_nao_deriva.sh:307`).
- A condição registrada para sair é medida+pouso do FIO MUDO, não mera existência de diff (`app/docs/CORTES.md:305`).
- Leitor externo do texto do comando é `bin/placar_code.sh:381`; preservar ou migrar a linha de contador por rodada quando mudar saída (`app/chamados/management/commands/disparar_perguntas_competencia.py:235`).

Contraexemplos úteis:

- Pergunta morta legitimamente por validação não autoriza nova irmã do mesmo fato; o próprio ramo distingue “FIO DECIDIDO”, falta ato administrativo (`app/chamados/management/commands/disparar_perguntas_competencia.py:201`, `app/chamados/management/commands/disparar_perguntas_competencia.py:211`).
- Duplicata respondida pode continuar viva: resposta antecede identidade no C6 (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`).
- Último marco satisfeito não resolve outro marco do dia; regra do marco próprio vem antes do veredito do dia (`app/chamados/juizes.py:1173`).
- O universo do comando é maior que abertura de `batida_ausente`; não alegar que simples hook nesse módulo cobre todo o agregado 494 (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/docs/PROMPTS.md:1875`). Distribuição produtiva por módulo/ato: [não verificado].
- Sem ata e com escala, não emitir âncora nem irmãs; sem escala há ramo distinto, que não pode virar fallback para erro de leitura (`app/chamados/services/disputa_emissao.py:818`, `app/chamados/services/disputa_emissao.py:833`, `app/chamados/services/disputa_emissao.py:842`).

## 05/11 18:24 · sessao 1afd20528d0c · - · - · origem=Ronald (tmux)

Comparação C/D, com sítios abertos:

| Item | Coincidência ou divergência | Código conferido |
|---|---|---|
| Fonte e juiz | Ambas mantêm `carimbar` como escritor, `_escrever_carimbo` como atribuidor da tripla e C6 como juiz; acrescentam releitura travada antes do candidato. | `app/chamados/models.py:2545`, `:2551`, `:2575`, `:2585`, `:2598`. |
| Snapshot | Os conjuntos de 13 campos são iguais; somente a ordem dos nomes muda. Diferença de **detalhe**, sem universo diferente no snapshot. | Campos consumidos/mudados na porta atual: `app/chamados/models.py:2572`, `:2576`, `:2580`; C6: `app/chamados/juizes.py:1135`, `:1150`, `:1171`. |
| Trava | Ambas copiam `select_for_update(of=('self','disputa')).select_related('disputa','celula')`; a célula está carregada, mas fora do `of`. | Padrão existente: `app/chamados/services/relavra_via.py:221`–`:224`. |
| Memo | Ambas descartam `_pv_memo` após a releitura; a chave omite ata/DNA, embora a concordância os leia. | `app/chamados/juizes.py:811`, `:816`, `:837`, `:1105`, `:1111`. |
| Obsolescência | **DESENHO:** pc1 retorna `False`; pc2 lança `ValueError`. Muda o protocolo de recusa/conflito dos chamadores. Hoje `False` expressa C6 viva; a ausência registra essa interpretação e o plantio levanta reprovação com a mesma descrição. | `app/chamados/models.py:2552`, `:2589`–`:2591`; `app/chamados/services/materializacao.py:546`–`:550`, `:1199`–`:1208`. |
| Identidade preexistente | **DESENHO:** somente pc2 recusa uma linha já materializada por identidade, mesmo quando o snapshot não mudou. pc1 somente detecta alteração desde a coleta. A porta atual proíbe identidade na **via de entrada**, mas depois sobrescreve a via recebida na instância; não possui essa guarda de estado anterior. | `app/chamados/models.py:2566`, `:2575`; identidade e precedência: `:2615`–`:2620`; preservação pelo inverso: `app/chamados/services/relavra_via.py:161`–`:175`. |
| Limite da transação/restauração | **DESENHO:** pc1 usa decorator, mantendo os catches atuais dentro do corpo; pc2 envolve `with atomic()` com `try`, incluindo a saída da transação local no tratamento. O código atual restaura falha do juiz e da lavra/save, sem `atomic` nessa função. | `app/chamados/models.py:2551`, `:2584`–`:2603`; autoridade da restauração da lavra: `app/chamados/veredito_lavrado.py:235`–`:245`. |
| Contrato de memória | **Detalhe documental:** pc1 altera a docstring de `_desfazer_em_memoria` para indicar a linha relida; pc2 deixa a declaração atual “EXATAMENTE como chegou”, embora também releia a instância. | `app/chamados/models.py:2605`–`:2612`. |
| Garantias pendentes | Ambas reconhecem que o PEP local não fecha a concorrência e que copiar o `of` não mede ordem global nem prova ausência de deadlock. | Exigência: `app/docs/PROMPTS.md:1882`; disputa primeiro: `app/chamados/services/disputa_emissao.py:467`, `:1156`; chamado antes de pergunta: `app/chamados/models.py:263`–`:266`, `app/chamados/signals.py:91`–`:92`. |

Contraexemplos presentes nas duas:

- **Resposta nova depois do carimbo:** pc1 D1 = pc2 D1. A limpeza da validação depende da instância antiga e seus `update_fields`; a atualização da resposta pode não incluir a validação concorrente. `app/chamados/services/disputa_emissao.py:1285`–`:1301`.
- **Validação fora da releitura:** pc1 D2 = pc2 D2. A pergunta é buscada/examinada antes do `atomic`; a assinatura é gravada depois, sem releitura travada nesse caminho. `app/chamados/services/validacao.py:359`, `:374`, `:395`–`:396`, `:127`–`:133`.
- **Insumos externos:** pc1 D3 = pc2 D6/D8, com pc2 separando ordem de travas. C6 lê célula, fato do marco e classificação da falta. `app/chamados/juizes.py:1147`, `:1177`–`:1178`, `:1201`–`:1202`; conjunto travado no padrão: `app/chamados/services/relavra_via.py:221`.
- **Rollback externo posterior:** pc1 D4 = pc2 D10. Há transação do materializador envolvendo plantio e carimbo; tratamento da memória da porta termina quando ela retorna. `app/chamados/services/materializacao.py:1142`, `:1199`–`:1208`; `app/chamados/models.py:2599`–`:2603`.
- **Idempotência:** pc1 D5 = pc2 D5. A porta comum renova o instante e salva sem guarda equivalente à administrativa. `app/chamados/models.py:2570`, `:2575`, `:2598`; guarda administrativa: `:2502`–`:2510`.
- **Memo fora da porta:** pc1 D6 = pc2 D7; pc2 amplia os insumos omitidos e identifica `_tranca_memo` no request/colaborador. `app/chamados/juizes.py:811`–`:819`, `:940`–`:947`, `:966`–`:979`.
- **Ata vazia:** pc1 D7 = pc2 D3. C6 aceita veredito não acusador se não há marcos pendentes, enquanto `_celula_furada` devolve desconhecimento quando não há `lampadas`. `app/chamados/juizes.py:1195`–`:1196`, `:473`–`:478`.
- **Tranca entre precheck e porta:** pc1 D8 = pc2 D4. C6 morre pela leitura da tranca antes dos demais fatos; os prechecks ficam nos chamadores. `app/chamados/juizes.py:1140`–`:1141`; `app/chamados/reconciliador.py:616`–`:620`; `app/chamados/services/disputa_emissao.py:1589`–`:1603`.
- **Observadores:** pc1 D9 e fecho de pc2 D destacam signals e `on_commit`; pc1 também enumera procedência/passivo/consumo da mesma instância. `app/chamados/signals.py:75`, `:125`, `:191`–`:202`; `app/chamados/services/disputa_emissao.py:1680`–`:1684`; `app/ponto/services/procedencia_batida.py:72`–`:75`; `app/chamados/services/passivo_validado.py:140`–`:145`.

Só explícitos na seção D de pc2:

- **D9, plantio anterior ao PEP:** o plantio ocorre dentro do `atomic` externo antes de `carimbar`; uma recusa precisa reverter esse ato. pc1 B já descreve essa composição, portanto não é ausência no documento inteiro. `app/chamados/services/materializacao.py:1142`–`:1148`, `:1199`–`:1208`.
- **D11, significado de conflito/retorno:** também incide no pc1, pois o `False` por obsolescência tampouco significa C6 viva. `app/chamados/services/materializacao.py:546`–`:550`, `:1201`–`:1203`.
- **D12, contexto legítimo em memória:** existe cópia de julgamento cuja célula é substituída pela passada em curso; vínculo desse contexto com um dos oito chamadores de `carimbar`: **[nao verificado]**. `app/chamados/models.py:1432`–`:1447`.
- **Exclusão por CASCADE:** relação com disputa usa `CASCADE`. Intercalação executada: **[nao verificado]**. `app/chamados/models.py:2200`–`:2203`.
- **Conferência humana de data fora do snapshot:** escreve `data_conferida_em/por`; a materialização lê essa conferência. `app/chamados/services/validacao.py:617`–`:619`; `app/chamados/services/materializacao.py:1115`–`:1116`.
- **Duas pausas:** o juiz exige prova de todas as pausas correspondentes; execução concorrente específica: **[nao verificado]**. `app/chamados/juizes.py:894`–`:913`.
- **Reapontamento concorrente:** o escritor muda destino e eventualmente data; intercalação executada: **[nao verificado]**. `app/chamados/services/fio.py:832`–`:847`.
- **Teste transacional real:** o histórico registra que `TestCase` ocultava ausência de atomicidade e determina `TransactionTestCase`. `app/docs/PROMPTS.md:1325`–`:1327`.

Citação falsa/parcial identificável em C/D:

- **pc2 D6, expressão “não [...] fechamento”: parcialmente excessiva se significar fechamento da própria disputa.** A proposta trava também a linha de `disputa`; o fechamento escreve `fechada_em/por/resolucao/motivo` nessa mesma linha. Portanto não é correto apresentar o fechamento dessa linha como livre durante a trava proposta. Para fechamento do período/tranca ou do chamado destino, a crítica continua pertinente; o texto precisa distinguir os objetos. `app/chamados/services/relavra_via.py:221`–`:224`; `app/chamados/models.py:1850`–`:1854`; leitura externa da tranca: `app/chamados/juizes.py:935`–`:947`.

Demais citações C/D abertas sustentam os sítios descritos; execuções de corrida, rollback e deadlock permanecem **[nao verificado]**.

## 05/11 18:24 · sessao 8930f357251c · - · - · origem=Ronald (tmux)

Os cinco atos estão localizados, mas um diff que apenas chame a fábrica após cada ato não realiza toda a regra: a fábrica exclui qualquer identidade histórica não duplicata, e a porta de nascimento devolve essa identidade sem ressuscitá-la. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/models.py:2420`; `app/chamados/models.py:2464`)

| Ato | Fonte e efeitos atuais | Ponto que o desenho precisa responder |
|---|---|---|
| Reabrir | `reabrir` relê o estado, grava pelo núcleo e ressuscita perguntas existentes. (`app/chamados/models.py:1322`; `app/chamados/models.py:1382`; `app/chamados/models.py:1391`) | Sem candidata histórica materializada e sem texto, ressuscitação devolve zero; não nasce a pergunta inexistente. (`app/chamados/models.py:1423`; `app/chamados/models.py:1482`) |
| Retratar duplicata | Canônica prioriza resposta textual; duplicata passa pela porta de identidade. (`app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:891`) | O corpo termina sem reconciliar o chamado que perdeu a resolvedora; a canônica pode preservar o fio sob outro chamado. (`app/chamados/services/fio.py:878`; `app/chamados/services/fio.py:895`; `app/chamados/services/pergunta_no_app.py:92`) |
| Validação | `_carimbar_validacao` escreve a assinatura; validação e tentativa individual de materialização HORA ficam no mesmo `atomic`; recusa pode desfazer a assinatura. (`app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:422`) | Materialização da disputa inteira ocorre depois daquele bloco; reconciliação do destino/clones pode ser recusada pela premissa residual. (`app/chamados/services/validacao.py:430`; `app/chamados/services/materializacao.py:728`; `app/chamados/models.py:701`; `app/chamados/models.py:730`) |
| Morte por leitura | O juiz é **C6 `pergunta_viva`**, registrado; não é necessário inventar juiz de vitalidade. (`app/core/juizes.py:266`; `app/chamados/juizes.py:822`) | C6 distingue validação, tranca, identidade, marco atendido, célula e outras provas; `False` sozinho não autoriza nova pergunta. (`app/chamados/juizes.py:1135`; `app/chamados/juizes.py:1170`; `app/chamados/juizes.py:1195`) |
| Reapontar | Porta valida dia, trava destino terminal e grava destino junto da lavra. (`app/chamados/services/fio.py:784`; `app/chamados/services/fio.py:761`; `app/chamados/services/fio.py:837`; `app/chamados/services/fio.py:845`) | O corpo não reconcilia a origem; `PerguntaDisputa.abrir` também reaponta identidade existente, portanto é sítio irmão obrigatório. (`app/chamados/services/fio.py:849`; `app/chamados/models.py:2427`; `app/chamados/models.py:2439`) |

A morte por leitura também pode envolver relógio sem qualquer escrita: o módulo da lavra declara essa condição e fornece expiração e relavra preguiçosa. Um `post_save` sozinho não cobre esse mecanismo. (`app/chamados/veredito_lavrado.py:23`; `app/chamados/veredito_lavrado.py:80`; `app/chamados/veredito_lavrado.py:173`)

C6 tem memo por instância; inserir escrita ou evento disparado em cada `False` afetaria seus leitores de app, fila de validação e alcance, além de poder repetir o efeito em instâncias distintas. (`app/chamados/juizes.py:822`; `app/chamados/juizes.py:835`; `app/chamados/services/pergunta_no_app.py:53`; `app/chamados/juizes.py:1403`; `app/chamados/juizes.py:1378`)

Para excluir vivos legítimos antes do RED, já existem autoridades nomeadas para fábrica e retenção, **C12/C13**, além de **C17** para alcance; não confundir C11 operacional do aval com C11 do registro, que é ausência. (`app/core/juizes.py:271`; `app/core/juizes.py:272`; `app/core/juizes.py:273`; `app/core/juizes.py:277`)

Os casos legítimos já nomeados incluem parcial na lista administrativa, pergunta viva em outra âncora e marco previamente decidido que requer ato administrativo; ausência de ata é espera pela célula, não autorização para fabricar pergunta. (`app/chamados/services/pergunta_no_app.py:23`; `app/chamados/services/pergunta_no_app.py:26`; `app/chamados/services/pergunta_no_app.py:28`; `app/chamados/services/pergunta_no_app.py:29`; `app/chamados/services/pergunta_no_app.py:80`)

Chamadores adicionais dos atos: dedup por comando; migração de resolvedoras; reconciliador; declaração textual; limbo; portas administrativa e de competência; validação por API/foto, ações de disputa, declaração e comando de classe A. (`app/chamados/management/commands/dedup_perguntas_fato.py:88`; `app/chamados/services/fio.py:943`; `app/chamados/services/fio.py:947`; `app/chamados/reconciliador.py:922`; `app/chamados/reconciliador.py:1570`; `app/chamados/reconciliador.py:1582`; `app/chamados/services/declaracao_texto.py:320`; `app/chamados/services/limbo_folgas.py:243`; `app/chamados/views_arquivar.py:70`; `app/chamados/services/competencia_trancada.py:265`; `app/api/foto_recebida.py:51`; `app/chamados/services/acoes_disputa.py:375`; `app/chamados/services/declaracao_texto.py:644`; `app/chamados/management/commands/validar_classe_a.py:100`)

Contraexemplos necessários:

- **Validada do mesmo marco:** reemitir só porque C6 morreu repetiria pergunta decidida; o disparador já separa “FIO DECIDIDO” e história do fato. (`app/chamados/management/commands/disparar_perguntas_competencia.py:201`; `app/chamados/management/commands/disparar_perguntas_competencia.py:211`)
- **Reapontamento concorrente com encerramento:** o destino precisa permanecer travado até gravar; a porta `reapontar` faz isso, mas o reapontamento histórico em `abrir` não abre `atomic` local. (`app/chamados/services/fio.py:752`; `app/chamados/services/fio.py:837`; `app/chamados/models.py:2427`)
- **Reabertura concorrente:** a releitura atual do status não equivale à trava da unidade inteira; `reabrir` não contém `atomic` em torno da consulta, transição e ressuscitação. (`app/chamados/models.py:1322`; `app/chamados/models.py:1382`; `app/chamados/models.py:1391`)
- **Duplicata respondida:** resposta com texto precede a via de identidade no C6 e pode manter a pergunta viva para decisão; “carimbou duplicata, morreu” não é uma inferência válida. (`app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1171`)
- **Dois marcos:** resolver saída não resolve intervalo; o próprio C6 aplica prova do marco antes do veredito do dia. (`app/chamados/juizes.py:1173`; `app/chamados/juizes.py:1183`)
- **C6 verdadeiro sem porta:** alcance exige app e âncora na fila, ou chamado da célula na fila; fio vivo sozinho não prova acesso. (`app/chamados/juizes.py:1344`; `app/chamados/juizes.py:1380`)

A incidência de cada classe nos 494, a quantidade de exclusões legítimas e o resultado produtivo de qualquer diff permanecem **[não verificado]**.

## 05/11 18:23 · sessao ddf0debdf637 · - · - · origem=Ronald (tmux)

Autoridades e casos pela regra, com as linhas da cópia atual:

| Caso: cadastro + estado + ato | Resposta normativa |
|---|---|
| Ativo, vínculo válido, ata acusa marco vencido, chamado `batida_ausente` sem pergunta; abrir chamado. | A pergunta nasce pelo evento, lendo a lâmpada; varredura fica vigia, sem decisão paralela. (`app/docs/PROMPTS.md:1875`; `CLAUDE.md:167`; `CLAUDE.md:168`; `CLAUDE.md:169`; `CLAUDE.md:170`) |
| Mesmo fato em duas aberturas concorrentes. | Mesmo estado final, uma linha de trilha, nenhuma duplicata. (`CLAUDE.md:175`) |
| Dívida persiste; reabrir sem pergunta restaurável. | Deve haver pergunta viva ou ação administrativa nomeada; simplesmente colocar o chamado em vivo não satisfaz a regra. (`app/docs/CORTES.md:253`) |
| Mesmo marco com duplicata; retratar última resolvedora de A enquanto canônica está em B. | O fato mantém fio alcançável; identidade não justifica cobrança duplicada nem perda de dono. (`CLAUDE.md:175`; `app/docs/CORTES.md:253`; `app/chamados/juizes.py:1344`) |
| Resposta discordante, ainda sem decisão administrativa; retratar duplicata. | A resposta continua para decisão humana; carimbo identitário não apaga a conversa. (`app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1158`; `app/chamados/juizes.py:1163`) |
| Saída e intervalo devidos; validar/materializar saída. | O marco atendido deixa de ser cobrado; o intervalo residual conserva fio ou ação nomeada. (`app/chamados/juizes.py:1173`; `app/docs/CORTES.md:253`) |
| Validação seguida de recusa de materialização, sem resolução do fato. | Não pode produzir quitação fictícia ou retirar a decisão humana de quem ainda deve. (`app/docs/CORTES.md:253`; `app/docs/LEIS.md:120`) |
| Última resolvedora de A transferida para B; reapontar. | Transferência preserva alcance do dia; não implica uma pergunta nova para cada chamado duplicado. (`app/docs/CORTES.md:253`; `app/chamados/juizes.py:1344`; `CLAUDE.md:175`) |
| Costura concorre com encerramento da âncora. | Estado final deve manter alcance e idempotência; pergunta viva sob âncora terminal não satisfaz alcance. (`CLAUDE.md:175`; `app/chamados/juizes.py:1344`; `app/chamados/juizes.py:1380`) |
| Empresa corte 21; fatos em 20/10 e 21/10; trancar competência 10. | Competência 10 é 21/09–20/10; 21/10 pertence à 11. Usar corte da empresa, nunca 21 cravado. (`app/docs/LEIS.md:77`; `app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/ponto/janelas.py:36`) |
| Turno 22h–06h ancorado ontem, observação 00h01 ou 06h10. | Virada civil não encerra fato; saída ainda não vencida às 06h10 se a margem é 30 minutos. (`CLAUDE.md:185`; `app/chamados/services/fabrica_da_celula.py:18`; `app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:845`) |
| Marco 18h, margem 30min; observar 18h29min59s e 18h30. | Não antecipar cobrança; no limite comparar instante encadeado mais margem. (`CLAUDE.md:185`; `app/ponto/services/cartorio.py:846`) |
| Vínculo antigo gerou célula do dia; novo está ativo hoje. | Vale geradora dentro da vigência, inclusive inativa; fora da vigência ou geradora não alimentada, não substituir por vínculo atual: `None`/CADASTRO. (`app/escala/alimentacao.py:231`; `app/escala/alimentacao.py:264`; `app/escala/alimentacao.py:272`) |
| Escala vigente, célula sem ata; emitir pergunta de marco. | Não afirmar marco faltante sem ata; acender falta de ata e aguardar lavra. Sem escala vigente há ramo distinto pelas batidas, que não autoriza generalizar “sem ata sempre cala”. (`app/chamados/services/disputa_emissao.py:818`; `app/chamados/services/disputa_emissao.py:833`; `app/chamados/services/fabrica_da_celula.py:34`) |
| `trabalha=None`, sem ata de outro tipo. | Não converter em folga: dia sem trabalho tem juiz próprio; ausência parcial pode cobrar resto. (`app/docs/JUIZES.md:55`; `app/docs/JUIZES.md:58`; `app/core/juizes.py:278`) |
| Parcial sem período, pendência administrativa. | Vai à lista do admin, sem pergunta/pai/push; permanece vivo legitimamente e sai da seleção “mudo indevido” antes do RED. (`app/chamados/services/disputa_emissao.py:847`; `app/chamados/services/disputa_emissao.py:892`; `app/docs/PROMPTS.md:1882`) |
| Ausência aberta cobre dia; cobrança suspensa. | Não forçar pergunta para zerar universo; ausência tem decisão própria, e C11 existente suspende por porta de arquivo. (`app/chamados/services/silencio_ausencia.py:77`; `app/chamados/services/silencio_ausencia.py:86`; `app/docs/PROMPTS.md:1882`) |
| Chamado `registrado` ou silenciado vigente, sem pergunta. | Vivo não equivale a pendência: fila exclui ambos; devem ser separados antes do RED dos 494. (`app/chamados/services/painel.py:174`; `app/chamados/services/painel.py:186`; `app/docs/PROMPTS.md:1882`) |
| Marco atendido, célula regularizada, chamado vivo sem pergunta. | Cobrança sem premissa fecha por lastro; não fabricar pergunta. (`app/docs/CORTES.md:253`; `app/core/juizes.py:263`) |
| Pergunta morre por leitura da tranca, fato do marco ou célula regularizada. | Juiz existente é C6 `pergunta_viva`, não novo juiz; morte individual não prova que o dia perdeu toda ação. C15 reduz coleção; C17 verifica alcance. (`app/core/juizes.py:266`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/chamados/juizes.py:1135`; `app/chamados/juizes.py:1233`; `app/chamados/juizes.py:1344`) |

Pontos que devem constar da consolidação:

- O aval local **nomeia literalmente os cinco atos** e manda remover vivos legítimos antes do RED: `app/docs/PROMPTS.md:1882`. O escopo 494 e padrão evento estão em `app/docs/PROMPTS.md:1875` e `app/docs/BACKLOG.md:1759`. Registros individuais/distribuição dos 494: **[não verificado]**.
- “C11” operacional do aval não equivale ao identificador `_C11` do registro: o registrado ainda responde ausência, via `reconciliar_por_ausencia`; não renomear nem clonar essa autoridade. (`app/core/juizes.py:117`; `app/core/juizes.py:271`)
- C6 é leitura memoizada por instância; chamar emissão de dentro dele transformaria a porta de leitura em escritora e afetaria todos seus consumidores. O corpo atual declara memo por request e só grava atributo em memória. (`app/chamados/juizes.py:822`; `app/chamados/juizes.py:835`; `app/chamados/juizes.py:846`)
- Ausência de prova não resolve marco: `fato_do_marco_no_espelho` devolve `None` para falta de célula/ata/DNA e isso nunca vira fato provado. (`app/chamados/juizes.py:875`)
- Exceção do cron só sai quando fio mudo medido e C11 pousado; leitura de código sozinha não satisfaz condição. (`app/docs/CORTES.md:305`; `app/docs/LEIS.md:139`)
- L-124 limita obra nova ao horizonte e preserva passado como foto até fato novo; backfill único é separado. L-113 altera a antiga proibição absoluta em exportada: preserva TXT anterior e trilha, admite fato novo pela porta; portanto não escrever “exportada nunca pode receber evento” como regra geral. (`app/docs/LEIS.md:145`; `app/docs/LEIS.md:149`)
- Referência normativa é regra aplicada ao cadastro/batidas; código velho só mede impacto. Sem resposta normativa, pergunta de lei antes do código. (`app/docs/LEIS.md:136`)
- Censo completo dos escritores precede filtro novo; autoridade abandonada continua existindo até aposentadoria com lápide/testes/pendente no mesmo marco. (`CLAUDE.md:193`; `app/docs/LEIS.md:137`)
- O PD antigo tem números de linhas desatualizados: cálculo C6 agora começa em `app/chamados/juizes.py:1135`; `fio_vivo` em `app/chamados/juizes.py:1233`; fábrica permanece em `app/chamados/services/fabrica_da_celula.py:145`.

_1953 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
