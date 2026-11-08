# PLANO-PADROES — registro e ordem

_Aval Ronald `PLANO-PADROES`, 27/10 13:3x. Registro e ordem; **nao reordena a fila ja mandada**
(IA-RBAC, ATA-PAUSA, FECHAR2/3 e PERGUNTA-SEM-SELO seguem como estao). Proibido em todo o plano:
event sourcing completo, bitemporal fora de vinculo/escala/regra, juiz novo._

| prio | obra | o que e | dono | entra quando | estado |
|---|---|---|---|---|---|
| P0 | `PERGUNTA-SEM-SELO` | censo do chat (classes 1, 3-7); o SELO e uma *fitness function* pelo SIGNIFICADO da classe (dia da batida, competencia, vinculo do dia, retratada), nunca pela forma, com allowlist com motivo por sitio. **Absorve** o bitemporal passo 1 (= classe [5], vinculo do dia) e a batida imutavel (= classe [7], universo por `batidas_apuraveis`) | Code (raias em `wt-semselo5-*`; ordem no BACKLOG) | em curso (aval PERGUNTA-SEM-SELO-AGORA 27/10 13:0x) | raia (1) construindo; classe (1) medida |
| P1 | `O-OUTBOX` | a transacao da batida ou do veredito grava **so o evento** numa tabela de saida; **UM** consumidor idempotente pela chave do fato processa **fora da pilha da batida**; cada cron de `JUIZES_POR_VARREDURA` vira **consumidor** ou **VIGIA** (pendentes, idade do mais velho). Destrava o pouso 2.1 do CONTRATO-20 | Code; **CONTRATO DE ENTRADA vai a corte do Ronald antes do codigo** | logo apos o pouso das raias ATA-PAUSA, IA-RBAC e FECHAR | registrado |
| P1 | `O-PROJECOES` | celula = ESCRITA; espelho, DiaPago, fechamento, retrato e pilulas = PROJECOES, cada uma com **fonte**, **versao do juiz** (carimbo de regra) e **comando de reconstrucao** | Code | apos o `REJULGAR-COMP10-RESTO` (pergunta de lei aberta no topo do RELATO) | registrado |
| P2 | `ADR-NAS-LEIS` | lei nova no `LEIS.md` ganha contexto, alternativas descartadas, consequencias, padrao conhecido e armadilha (corte (ii) abaixo) | Code, a cada lei nova por corte | vale ja para as leis novas; retroativo nao pedido | vigente |
| P3 | `O-BITEMPORAL-2` | versoes de vinculo, escala e regra com `vigente_de`/`vigente_ate` e `registrado_em` | Code | apos `contratos_estruturais` = 20/20 | registrado |

## CONTRATO DE ENTRADA DO O-OUTBOX (a escrever e levar a corte ANTES do codigo)

Cinco perguntas, cada uma com numero medido, nenhuma com opiniao:
1. **tabela** — campos, indice, retencao;
2. **chave de idempotencia** — qual fato, e por que dois eventos do mesmo fato nao processam duas vezes;
3. **quais crons de `JUIZES_POR_VARREDURA` viram CONSUMIDOR e quais viram VIGIA** (lista nominal lida de `config/crons.py`);
4. **custo na pilha da batida medido ANTES e DEPOIS** (pelo caminho real `registrar_batida`, selo do chokepoint, L-030);
5. **escala de 5.000 colabs** — vazao do consumidor e idade maxima do pendente nessa escala.

## CORTES DESTE AVAL (lei nova por corte literal; L-117 permite)

_A linha **Lei** e o corte literal dele; contexto, alternativas, consequencias, padrao e armadilha sao REDACAO do Code a partir do aval e do RELATO (a L-122 manda o formato, nao o conteudo) -- revise._

### L-121 — POUSO DE CURA DE JUIZ RECONSTROI AS PROJECOES DA COMPETENCIA ABERTA
- **Lei:** o pouso de cura de juiz inclui reconstruir pela PORTA as projecoes da competencia NAO exportada, pela 7b (DIFF antes, reversao); a competencia exportada segue a L-092/L-113.
- **Contexto:** a cura de juiz que pousa sem rejulgar deixa a celula/projecao carimbada pela regra velha; o REJULGAR-COMP10 (27/10) mostrou 15 colabs com ata movida por curas JA no ar e 10 celulas que pioram ou emitem.
- **Alternativas descartadas:** esperar o ciclo natural (a projecao mente ate la); rejulgar a exportada junto (fere a L-092).
- **Consequencias:** todo pouso de juiz passa a ter um passo de reconstrucao com DIFF e reversao; o custo de pouso sobe; a projecao passa a carregar a versao do juiz (O-PROJECOES).
- **Padrao conhecido:** *projection rebuild* / *read model replay* (CQRS).
- **Armadilha:** reconstruir sem a versao do juiz carimbada nao diz QUAL regra a projecao reflete; reconstruir por recalculo nunca e cirurgico (AVAL-DE-CRITERIO) — medir contra o GRAVADO.

### L-122 — TODA LEI NOVA NO FORMATO ADR
- **Lei:** toda lei nova entra no `LEIS.md` com contexto, alternativas descartadas, consequencias, padrao conhecido e armadilha.
- **Contexto:** as LAPIDES guardam a historia, mas nao as ALTERNATIVAS descartadas; quem chega relitiga a escolha.
- **Alternativas descartadas:** ADR em arquivo por lei (fragmenta o indice unico); so ponteiro para a LAPIDE (nao responde "por que nao a outra").
- **Consequencias:** a linha da lei no LEIS.md aponta para a secao ADR (aqui, enquanto o indice nao tem coluna propria).
- **Padrao conhecido:** *Architecture Decision Record* (Nygard).
- **Armadilha:** ADR escrito depois da lei vira justificativa, nao decisao; escreve-se no ATO do corte.

## NORTE DO SISTEMA (corte Ronald `PADRAO-NA-CURA`, 27/10 17:0x -- L-123; texto LITERAL, nao padrao novo)

"o norte do sistema e o desenho de engenharia do GLOSSARIO e do PLANO-PADROES": **single source of truth / um juiz por
pergunta**, **single writer / escritor unico**, **evento em vez de varredura**, **log imutavel / outbox**, **bitemporal**,
**projecao reconstruivel / CQRS**, **fitness function / selo**, **ADR**. Os termos acima e os da tabela do GLOSSARIO
(coluna termo tecnico e coluna alias da casa, ex.: "juiz unico") valem no campo `padrao=` da linha LEI-AKITA; padrao
novo so por corte dele (o Code propoe no topo do RELATO com o caso).

### L-123 — PADRAO-NA-CURA (regra sagrada)
- **Lei:** nenhuma cura entra sem dizer qual padrao ela REALIZA; cura e a CLASSE, nunca o caso; `padrao=` obrigatorio na linha LEI-AKITA (termo do GLOSSARIO ou deste arquivo); o verificador confere o padrao contra o diff; padrao novo so por corte.
- **Contexto:** em 27/10 as curas do dia (BO-ADMIN, laudos) chegavam como CASO (um colab, um dia); a mesma classe reaparecia em outro colab no dia seguinte.
- **Alternativas descartadas:** padrao so na prosa do commit (nao se mede); padrao livre (vira rotulo vazio).
- **Consequencias:** commit de cura sem padrao conhecido fica VERMELHO no pre-push; o GLOSSARIO vira vocabulario controlado.
- **Padrao conhecido:** *fitness function* sobre a propria conduta; vocabulario controlado (DDD *ubiquitous language*).
- **Armadilha:** rotular a cura com o padrao certo sem que o diff o realize -- por isso a clausula (3) e do verificador, nao do selo.
_(contexto/alternativas/armadilha: redacao do Code, L-122 manda o formato; revise.)_

### L-124 — HORIZONTE-PARA-FRENTE (corte literal Ronald 02/11 13:38)
- **Lei:** "horizonte para frente, backfill unico no fim": obra nova alcanca do horizonte para frente, no default do COBPARC-HORIZONTE-10; o passado fica FOTO ate fato novo; o alcance do corte 07/10 acontece UMA vez, no BACKFILL UNICO no fim. L-113 e L-092 intactas.
- **Contexto:** o pouso E3-ISENTO-INSUMOS (c9f16a60) rejulgava 1.482 celulas de isentos em 08/09; o encerramento pelo consumidor deu 13 > teto 5 e a CAUSA-13 achou o vazamento do `out['cods']` do lote (CODS-DO-ATO). Cada pouso com alcance retroativo repetia o mesmo risco sobre comp ja fechada.
- **Alternativas descartadas:** rejulgar o passado a cada pouso (o (A) de 10:4x: repete o risco e o teto a cada obra); bump de versao global v5 (mudaria a impressao de toda celula de comp 10+ e rejulgaria a frota).
- **Consequencias:** ate o backfill, comp antiga fica em regra antiga e a tela dela pode diferir da regra nova; o BACKFILL-UNICO fica maior e vira obra propria, que separa fato novo x so regra e sai por lista com dono, por uma porta so.
- **Padrao conhecido:** *Backfill / Reprocessing* com corte de vigencia (versionamento de regra por horizonte, `versao_para`).
- **Armadilha:** regra nova lida SEM o horizonte em algum leitor (tela, D5) faz a comp antiga parecer julgada pela regra nova; e fato novo em comp antiga rejulga pela regra DAQUELA comp, nao pela nova.
_(contexto/alternativas/armadilha: redacao do Code a partir do aval HORIZONTE-PARA-FRENTE + ISENTO-H10; revise.)_

### L-125 — CLUSTER NAO FECHA POR IDADE (corte literal Ronald 11/10 11:4x, CORTES-DOS-CONTRATOS (1))
- **Lei:** "chamado de cluster NAO fecha por idade; so morre por lastro, e o fechamento por janela e sobra a remover."
- **Contexto:** o fecho de `cluster_espurio.py::_vivos_que_a_premissa_matou` conservava so os chamados cujo dia caia na janela de 14 dias e fechava os mais velhos sem ler a celula do dia deles (o dia fora da janela caia como "sem assinatura"): a idade decidia, nao o fato (O169, pergunta com opcoes (a)/(b)).
- **Alternativas descartadas:** (a) manter a janela como regra de prescricao (idade vira juiz paralelo do fato); estender a janela (so muda o numero da idade).
- **Consequencias:** chamado antigo so fecha quando a celula do proprio dia mostra a assinatura ausente; celula ausente = nao medido e o chamado vive; ata sem insumos tambem vive, MENOS quando `dia_sem_trabalho` declara o dia sem trabalho, que e perguntado ANTES dos insumos (`_leitura_da_celula`) e prevalece -- o chamado fica elegivel a fechar, ainda sujeito as guardas da reconciliacao; universo do nao medido sem contador ainda (fila).
- **Padrao conhecido:** reconciliacao *level-triggered* (o estado se compara ao fato atual, nao ao evento nem ao relogio).
- **Armadilha:** trocar o None (nao medido) por False (ausente) fecha por falta de medida, que e o fechamento por idade com outro nome.
_(contexto/alternativas/armadilha: redacao do Code a partir do corte e da O169; revise.)_
