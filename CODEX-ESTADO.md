# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 09:59** · publicado em **15/11/2026 10:04** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 09:59 · sessao 656d7497dde9 · - · - · origem=Ronald (tmux)

Há duas lacunas semânticas concretas:

1. **Escolha:** VALIDACAO não testemunha materialização. T distingue `validada_sem_efeito` (`chamados/services/tarefa_do_dia.py:169`). O materializador da escolha devolve sucesso sem Batida (`chamados/services/materializacao.py:1069`), depois carimba e lavra (`:1283`), sem registrar DP de materialização. SILENCIO testemunha fechamento humano (`chamados/models.py:1988`); ABSORCAO testemunha ausência vinculada (`chamados/services/materializacao.py:569`). Nenhum deles cobre toda escolha.

   Pergunta: **qual testemunha factual substitui C na materialização de escolha, preservando a distinção entre validação e efeito?** Pausar esse ramo da unidade E.

2. **Retratação auditada antiga:** `_retratada_com_trilha` exige C mais auditoria `retratar_pergunta_obsoleta` (`chamados/juizes.py:291`). RETRATACAO atual significa remoção do motivo, seguida de identidade (`chamados/models.py:2110`, `:2120`, `:2125`). Nenhum escritor produtivo atual desse nome de auditoria apareceu; acervo real [nao verificado].

   Pergunta: **qual fato conserva o fechamento obsoleto auditado e o distingue da remoção do motivo?** A06 pode conservar identidade aplicável; substituir universalmente por RETRATACAO + log não tem equivalência demonstrada. Pausar essa unidade fora de A06.

## 15/11 09:59 · sessao 656d7497dde9 · - · - · origem=Ronald (tmux)

Censo somente leitura; nenhum arquivo editado.

As 16 unidades no HEAD informado:

| Unidade | Linha antiga preparo → atual | Leitura atual relevante |
|---|---:|---|
| `_retratada_com_trilha` | 285? → 285 | C:291; auditoria:294-296 |
| `_slot_esperado` | 341? → 344 | R:351,363; delega ao plantio:374-376 |
| `_pergunta_materializada_de_fato` | 662 → 665 | C/VIA:682,694,710 |
| `_chave_memo_pv` | 821 → 824 | R/V/C/VIA:855-857 |
| `_resposta_concorda_com_a_luz` | 1330? → 1333 | R:1359 |
| `projecao_temporal_da_pergunta` | 1378 → 1381 | R/V/C/VIA:1388-1398 |
| `pergunta_sem_lastro_ou_viva` | 1492? → 1495 | C:1499 |
| `respondida_sem_texto` | 1525? → 1528 | R:1531 |
| `q_respondida_sem_texto` | 1548 → 1544 | R SQL:1551 |
| `q_respondida_com_texto` | 1556 → 1554 | R SQL:1559 |
| `perguntas_corrigiveis` | 1712? → 1716 | R SQL:1718 |
| `perguntas_respondidas_a_validar` | 1721? → 1723 | R ordenação:1727 |
| `declarou_hora` | 1747? → 1738 | R:1753 |
| `delta_da_resposta` | 1894? → 1890 | R:1900 |
| `classificar_resposta_validacao` | 1998? → 1996 | R:2004,2009 |
| `motivo_implausivel_validacao` | 2020? → 2017 | R:2026 |

As interrogações indicam que o preparo forneceu linha do acesso, sem linha de declaração; não apresentar essas antigas como medidas.

**Escritores/fatos, compartilhados pelas unidades D01/D03/D06:**

- R humano: `chamados/services/disputa_emissao.py:1481`, `:1482`; mesma testemunha `_agora` em DP RESPOSTA `:1525`; nova resposta limpa V `:1494`.
- R automático: `chamados/services/disputa_emissao.py:1673`, `:1674`; DP mesmo instante `:1677`.
- R reparado por trilha: `chamados/services/disputa_emissao.py:1695`; DP instante da testemunha `:1698`.
- Nascimento com R/V recebido: `chamados/models.py:3102` chama conservação e `registrar`.
- V: `chamados/services/validacao.py:159`, `:160`; DP mesmo instante e autor `:164`.
- DEVOLUÇÃO: limpa V `chamados/services/validacao.py:224`, `:225`; DP `:230`.
- REABERTURA/RECUSA: `chamados/services/disputa_emissao.py:1631` registra foto e instante `:1632-1637`; porta do lote limpa projeção `:1648`. RECUSA humana entra nessa porta pelo fluxo `chamados/services/validacao.py:626`.
- C e VIA têm atribuição única em memória: `chamados/models.py:3180-3184`; identidade usa a porta `:3265-3268`.
- SILÊNCIO: DP com instante do C em `chamados/models.py:1988-1990`.
- RETRATAÇÃO: DP em `chamados/models.py:2120-2124`, identidade em `:2125`.
- ABSORÇÃO: DP com mesmo `agora` do carimbo `chamados/services/materializacao.py:567-572`.
- Escrita física DP: `chamados/models.py:4086`, `:4096`; ordenação canônica `:4032`; conservação `:4057-4083`.
- Efeito Batida: registro `chamados/services/materializacao.py:933-939`; vínculo, inclusive reuso, `:952-956`; `timestamp`, `retratada_em`, `criado_em` são campos distintos em `ponto/models.py:23`, `:67`, `:79`.

**Quem lê os valores migrados, por unidade:**

- `_retratada_com_trilha`: E em `chamados/juizes.py:800`, `:818`, `:821`; lote em `chamados/services/carga_pergunta_viva.py:194`.
- `_slot_esperado`: E `chamados/juizes.py:755`; lote `chamados/services/carga_pergunta_viva.py:119`; explicador `chamados/services/explicador.py:29`; vigia `chamados/services/vigia_carimbo.py:110`; classe do lote `chamados/services/validacao.py:907`.
- E: P `chamados/juizes.py:1486`; lampada/lavra `chamados/veredito_lavrado.py:263`; materialização `chamados/services/materializacao.py:1239`, `:1433`; raiox `chamados/services/raiox.py:191`; explicador `chamados/services/explicador.py:93`; backfill `chamados/management/commands/backfill_veredito_lavrado.py:105`.
- Memo: `chamados/juizes.py:893`, `:905`.
- Concordância: P `chamados/juizes.py:1432`.
- Projeção temporal: P `chamados/juizes.py:1403`; slot `:350`.
- Sem lastro: corrigíveis `chamados/juizes.py:1719`; leitor de declaração `chamados/services/declaracao_texto.py:240`.
- Respondida sem texto: porta e vigia `chamados/services/fio.py:187`, `:1602`; `chamados/services/vigia_fio.py:629`; admin `chamados/juizes.py:1706`.
- Face SQL com texto: `chamados/models.py:3453-3454`; fila `chamados/juizes.py:1725`. Outros consumidores diretos da face sem texto fora da declaração: [nao verificado].
- Corrigíveis: `chamados/models.py:2282`; `chamados/services/acoes_disputa.py:96`; `chamados/views.py:1419`; template tag `chamados/templatetags/chamados_tags.py:202`.
- Fila a validar: `chamados/services/validacao.py:956`; `api/views_mensageria.py:540`.
- Declarou hora: `chamados/services/relavra_via.py:97`; `chamados/services/validacao.py:185`; `chamados/services/materializacao.py:555`.
- Delta: classificação `chamados/juizes.py:2010`; lote `chamados/services/validacao.py:899`.
- Classificação: tags `chamados/templatetags/chamados_tags.py:147`, `:174`; mensageria `api/views_mensageria.py:544`; comando `chamados/management/commands/validar_classe_a.py:63`.
- Implausível: validação `chamados/services/validacao.py:429`; tag `chamados/templatetags/chamados_tags.py:22`.
- Reexportação histórica das autoridades: `chamados/models.py:4226`.

**Esmeril semântico:**

- `ato_em` é instante do ato (`chamados/models.py:3984-3992`, `:4016`), ordenado com `pk` (`:4032`). R/V humanos usam o mesmo instante da projeção (`disputa_emissao.py:1474-1482`, `:1525`; `validacao.py:159-166`).
- Ambiente aplica `USE_TZ=True`, fuso `America/Sao_Paulo` (`config/settings/base.py:93`, `:95`). “Todo datetime está materializado em UTC em memória” não é garantido pelo campo; [nao verificado]. Não confundir representação/fuso com equivalência de instante.
- Instante do ato não é hora declarada do trabalho. Texto `data_hora` é convertido em horário local consciente (`chamados/juizes.py:363-369`). Para `hora`, a data sai da cadeia de marcos do turno (`:299-334`) e da função de plantio (`:374-376`).
- `data_evento` é dia do turno; conversa guarda dia civil e é excluída dessa interpretação (`chamados/juizes.py:337-341`). Preservar essa separação.
- C não equivale a efeito: registro, vínculo, timestamp do trabalho e instante de criação da Batida são distintos (`materializacao.py:933-956`; `ponto/models.py:23`, `:40`, `:79`). Marco D06 deve captar efeito/retratação e lavra; só o último timestamp do trabalho não basta.
- T atual está em `chamados/services/tarefa_do_dia.py:140-185`, não nas antigas linhas do preparo. Passivo em `:152-153`; devolução `:163-164`; validação sem efeito `:169-170`.

**Dois riscos concretos para a construção:**

1. `_slot_esperado` delega à F4 ainda leitora de projeção (`chamados/juizes.py:374-376`); precisa fornecer os valores do ato ao delegado sem alterar F4.
2. Guarda de escritor chama `respondida_sem_texto` com `SimpleNamespace` sem relação DP (`chamados/services/fio.py:1598-1605`). Além disso, A05 aplica R/V/C em memória antes de chamar P (`chamados/models.py:3212-3222`). Preservar essas exceções expressamente; uma leitura irrestrita de `p.decisoes` quebraria esses caminhos.

## 15/11 09:58 · sessao 9ff5783795fe · ESPINHA-NA-FRENTE · sonda · origem=Code

**NOVO: 248 dias sem porta; ANTIGO: 7 dias de fio morto.** A saída fornecida mede 248, enquanto o aval anexo menciona 247. Todos os números abaixo vêm exclusivamente dessa saída, sem arquivo numerado para conferência. **[nao verificado]**

O NOVO lista também os dias com porta ou responsável: apenas `sem_porta` incrementa `fio_morto_alcance`. [vigia_fio.py:658](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:658), [vigia_fio.py:714](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:714).

**Nenhum plano apresentado permite atribuir zeragem integral aos recortes atuais.** E4c migra leitores; E4d interrompe a projeção; E4B declara cobertura geral das classes a–e e deixa os itens 18–19 para o fim. O anexo E4c enumera arquivos por F1–F9, sem associar essas causas à zeragem. `e4c_reconfere_v2.md:7–15`; [BACKLOG.md:1968](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1968), [BACKLOG.md:1977](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1977).

E3-RESTO-R3 remete ao lote LU; RAIZ trata a reação conjunta de perguntas e disputas. Essas linhas também não identificam quais dias desta saída zerariam. [BACKLOG.md:768](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:768), [BACKLOG.md:1988](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1988).

Para atribuir a cura, faltam:

- **a:** identificar a ação alcançável que faltou; o mapa registra que o PRONTO não transforma `registrado` em ação. [RELATO.md:8823](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/RELATO.md:8823).
- **c:** ligar a resposta sob terminal ao ato reparador; `reabrir_credor` retorna sem ato para pergunta respondida. [disputa_emissao.py:1842](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/disputa_emissao.py:1842).
- **d:** diagnosticar a retenção e o ato correspondente; a definição da classe exige medida antes de cura. [BACKLOG.md:859](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:859).
- **e/terminal:** conferir a carga do credor e o resultado do reparo E4B; outra pergunta exportada, limbo ou recusa podem impedir a reabertura. [disputa_emissao.py:1855](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/disputa_emissao.py:1855).
- **e/sem viva e ANTIGO:** vincular cada dia ao ato executável e medir o resultado; o backfill distingue `tarefa_nada` e dias que continuam sem porta. [backfill_fio_morto_e4b.py:117](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:117), [backfill_fio_morto_e4b.py:368](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:368).

As contas fecham pelo dado fornecido. **[nao verificado]**

- NOVO sem porta: **212 + 11 + 10 + 7 + 2 + 4 + 2 = 248**.
- NOVO completo: **1.560 + 248 + 104 + 65 + 19 + 5 + 248 = 2.249**.
- ANTIGO morto: **6 + 1 = 7**. Universo completo de lâmpadas: **2.381 + 386 + 2 + 0 + 8 + 7 + 0 + 130 = 2.914**.
- Interseção dos **7 antigos com os 2.249 novos: 6**, todos classificados `porta_fila` no NOVO. Interseção entre os **dois contadores de mortos: 0**.
- **“Sem obra” com zeragem demonstrada: 248 NOVOS + 7 ANTIGOS = 255 dias distintos entre os alarmes.** A sonda não identifica individualmente quais antigos compõem cada causa.

Na tabela, **E/A = `exportada=True` / `exportada=False`**; todas essas contagens são do dado fornecido. **[nao verificado]** “Sem obra” significa sem zeragem atribuível demonstrada pelos planos acima.

| Universo · rota/causa/detalhe | E/A | Código classificador | Obra que zera |
|---|---:|---|---|
| NOVO · `porta_app/-/-` | 0/1.560 | [tarefa:634](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/tarefa_do_dia.py:634) | Fora do contador morto |
| NOVO · `porta_fila/-/-` | 0/248 | [tarefa:632](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/tarefa_do_dia.py:632) | Fora do contador morto |
| NOVO · `porta_pergunta/-/-` | 0/104 | [tarefa:632](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/tarefa_do_dia.py:632) | Fora do contador morto |
| NOVO · `decisao_do_dia_pendente/-/-` | 0/65 | [vigia:306](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:306) | Fora do contador morto |
| NOVO · `fio_do_admin/-/-` | 0/19 | [vigia:313](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:313) | Fora do contador morto |
| NOVO · `aguarda_decisao_de_ausencia/-/-` | 0/5 | [vigia:308](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:308) | Fora do contador morto |
| NOVO · `sem_porta/d/-` | 0/212 | [vigia:696](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:696) | **sem obra** |
| NOVO · `sem_porta/c/-` | 0/11 | [vigia:694](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:694) | **sem obra** |
| NOVO · `sem_porta/a/em_analise` | 0/10 | [vigia:683–689](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:683) | **sem obra** |
| NOVO · `sem_porta/a/aberto` | 0/7 | [vigia:683–689](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:683) | **sem obra** |
| NOVO · `sem_porta/a/registrado` | 0/2 | [vigia:683–689](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:683) | **sem obra** |
| NOVO · `sem_porta/e/pergunta_viva_sob_chamado_terminal` | 0/4 | [vigia:706](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:706) | **sem obra**; candidata E4B |
| NOVO · `sem_porta/e/sem_pergunta_viva` | 0/2 | [vigia:704](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:704) | **sem obra** |
| ANTIGO · `pergunta_morreu` | 0/6 | [vigia:449–450](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:449) | **sem obra** |
| ANTIGO · `sem_pergunta_nenhuma` | 0/1 | [vigia:449–450](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:449) | **sem obra** |

## 15/11 09:57 · sessao ccf03c37b60d · - · - · origem=Ronald (tmux)

Confirmo a leitura conservadora: **não há plano que demonstre zeragem dos recortes atuais por causa**; há obras candidatas e cobertura parcial documentada. `app/docs/RELATO.md:8820`, `app/docs/RELATO.md:8823`, `app/docs/RELATO.md:8826`, `app/docs/RELATO.md:8827`.

- **a/registrado:** o mapa anterior declara “PRONTO nao transforma registrado em acao”. `app/docs/RELATO.md:8823`.
- **c/respondida sob terminal:** o reparo de credor exclui respondidas; o código retorna sem ato quando `respondida_em` existe. `app/docs/RELATO.md:8827`; `app/chamados/services/disputa_emissao.py:1842`.
- **d:** o mapa registra a lacuna “folha retem, tarefa nada”; não demonstra cura pela E3 ou E4B. `app/docs/RELATO.md:8820`; `app/docs/RELATO.md:610`.
- **e/terminal:** E4B é candidata, mas pode recusar por limbo, outra pergunta exportada na carga ou recusa da reabertura; `exportada=False` do dia não basta. `app/chamados/services/disputa_emissao.py:1855`, `app/chamados/services/disputa_emissao.py:1860`, `app/chamados/services/disputa_emissao.py:1869`.
- **e/sem viva e ANTIGO:** falta diagnóstico que vincule cada dia ao ato reparador; o comando determina atos por tarefa e mede depois se o dia permaneceu sem porta. `app/chamados/management/commands/backfill_fio_morto_e4b.py:255`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:368`.

Recomendo escrever **“sem obra com zeragem demonstrada”**, evitando afirmar ausência de obra registrada: E4B cobre genericamente classes a–e, E4c migra leitores e E4d interrompe projeção. `app/docs/BACKLOG.md:1977`; `app/docs/BACKLOG.md:1968`.

Subtotal conservador: **248 NOVO + 7 ANTIGO = 255 incidências sem zeragem atribuível demonstrada** pelo material disponível. Os números atuais vêm exclusivamente do enunciado. `[nao verificado]`

## 15/11 09:56 · sessao 3cff7206f1ea · - · - · origem=Ronald (tmux)

- **E4c/E4d não têm zeragem por causa demonstrada nos anexos.** BACKLOG define E4c como migração dos leitores DIVERGE e selo; E4d como parar a projeção. O inventário F1–F9 do anexo identifica arquivos, não um reparo que zere cada classe. `app/docs/BACKLOG.md:1968`; anexo `e4c_reconfere_v2.md`, itens F1–F9.
- **E4B é a obra candidata ao passivo a–e**, explicitamente; porém a mesma linha informa apenas que itens 18–19 ficam para o fim do programa. Não demonstra que estes 248 dias desaparecerão. `app/docs/BACKLOG.md:1977`.
- **O próprio comando exige classificação adicional:** deriva atos da tarefa; sem ato executável retorna `tarefa_nada`/`nao_sei`; retira essas linhas da execução; depois mede quais continuam sem porta. A saída fornecida não contém esses atos/resultados por dia. `app/chamados/management/commands/backfill_fio_morto_e4b.py:109`; `:117`; `:262`; `:368`.
- **Classe d tem lacuna documentada:** a definição do contador não equivale ao “sair de d” do aval; exige medida antes de cura. RELATO também registra ausência de decisão na E3 para o estado retido cuja tarefa não exige nada. `app/docs/BACKLOG.md:859`; `app/docs/RELATO.md:610`.
- **E3-RESTO-R3 remete ao lote LU**, mas sua linha não relaciona LU-01..07/LU3 às causas a/c/d/e nem promete zerá-las. `app/docs/BACKLOG.md:768`.
- **RAIZ-DA-DISPUTA trata reação conjunta de raiz e filhos**, destino e órfãos, mas não contém associação dos 248/7 dias atuais a uma fatia que os zere. `app/docs/BACKLOG.md:1988`; `app/docs/GRADE-MESTRA.md:108`.

Classificação conservadora: **“sem obra com zeragem comprovada”** para os 248 NOVOS e os 7 ANTIGOS; E4B pode ser indicada como candidata, sem subtrair dias do subtotal “sem obra”. Para atribuição verificável falta plano por ramo/ato e resultado do DRY sobre os dias fornecidos. `app/docs/BACKLOG.md:1977`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:255`; `:368`.

## 15/11 09:56 · sessao 95f73f751961 · - · - · origem=Ronald (tmux)

- **NOVO:** `alcance_universo` reúne dias retidos pela Folha, por colaborador/data, nas competências enumeradas; não é o contador de mortos. `app/chamados/services/vigia_fio.py:584–600`. Apenas `sem_porta` incrementa `fio_morto_alcance`. `app/chamados/services/vigia_fio.py:714–718`.
- **Classificações NOVO:** `a` = presença de chamado vivo do dia ou âncora viva fora da fila, detalhe com status; `c` = pergunta viva respondida com destino terminal; `d` = nenhuma pergunta viva, dia fora da lista e colaborador na lista; `e/sem_pergunta_viva` e `e/pergunta_viva_sob_chamado_terminal` são residuais posteriores. `app/chamados/services/vigia_fio.py:679–707`.
- **Rotas publicadas:** `porta_fila`/`porta_pergunta` vêm da tarefa em `app/chamados/services/tarefa_do_dia.py:628–632`; `porta_app`, em `app/chamados/services/tarefa_do_dia.py:634`; o vigia apenas as lê e conta. `app/chamados/services/vigia_fio.py:658–665`.
- **Baldes com ação nomeada:** decisão pendente, ausência aberta e fio do admin são classificados respectivamente em `app/chamados/services/vigia_fio.py:306–307`, `:308–310`, `:313–314`; entram antes das causas de `sem_porta`. `app/chamados/services/vigia_fio.py:670–678`.
- **ANTIGO:** parte de chamados vivos com célula, restringe o vocabulário e exige lâmpada acesa; primeiro separa fio vivo, hoje/futuro e os baldes com dono. `app/chamados/services/vigia_fio.py:334–379`, `:412–448`. No residual, `ps` não vazio gera `pergunta_morreu`; vazio gera `sem_pergunta_nenhuma`. `app/chamados/services/vigia_fio.py:449–450`.
- A sonda fornecida permite **6 `pergunta_morreu` + 1 `sem_pergunta_nenhuma` = 7**, todos abertos; não imprime qual célula pertence a qual dessas duas causas. Atribuição individual permanece **[nao verificado]**.
- As **6 células antigas presentes no NOVO estão todas em `porta_fila` no dado fornecido**. Logo, a interseção dos 7 antigos com o universo NOVO de 2249 é **6**; com os **248 `sem_porta` é 0**, conforme as linhas fornecidas **[nao verificado]**.
- A partição antiga `com_alvo=0`, `sem_alvo=5`, `sem_pergunta_na_porta=2` também soma 7, mas é classificação distinta e não equivale às causas a–e. `app/chamados/services/vigia_fio.py:467–480`.
- Para eventual conferência do universo completo antigo: **2381 + 386 + 2 + 0 + 8 + 7 + 0 + 130 = 2914**, conforme agregados fornecidos **[nao verificado]**; o código declara a identidade em `app/chamados/services/vigia_fio.py:104–107`.
- Exportação do ANTIGO compõe a janela do dia com `competencia_entregue`; não presume o mês civil. `app/chamados/services/vigia_fio.py:843–865`.

## 15/11 09:53 · sessao 698c79408f3e · ESPINHA-NA-FRENTE · leitura · origem=Code

**“Nada” abaixo significa nenhum pedido adicional demonstrado pelo recorte descrito.** O anexo fornece alvos da E4c; o BACKLOG descreve CONTRACT e fatos da E4d, mas o diff final de cada pouso é **[nao verificado]**. `e4c_reconfere_v2.md:7–15` (anexo); `app/docs/BACKLOG.md:1968–1975`.

Cláusulas usadas: **SMOKE** = BUG 73, duas cascas; PRODUCAO-MAXIMA 5 permite smoke posterior somente quando resta TELA, com reversão pronta. **BATIDA** = kernel espera Ronald. **DINHEIRO** = aberta preaprovada com reversão anterior, hash das exportadas intacto e prova de frota posterior; gravado exportado exige `!` e REGEN-EM-EXPORTADA. **PARADA 2 / §4** = dados ESCALA/VÍNCULO, motor e apagar/voltar arquivo usado em prod exigem `!`. **CADASTRO** = regra variável precisa de cadastro pela UI. `CLAUDE.md:202–203`, `CLAUDE.md:143–144`, `CLAUDE.md:265–268`, `CLAUDE.md:406–417`, `CLAUDE.md:43–44`; `producao_maxima.txt:1` (anexo).

**Dinheiro não pode receber “exportada intacta” por presunção:** validação/materialização alcançam o fechamento da competência do fato; a cópia persiste também outros status e tem guarda específica para 09/2026. Competências e valores efetivamente atingidos por cada futura fatia: **[nao verificado]**. `app/chamados/services/validacao.py:470–472`; `app/ponto/services/fechamento.py:1809–1810`, `app/ponto/services/fechamento.py:1137–1162`, `app/ponto/services/fechamento.py:36–43`.

| fatia | pede | cláusula | arquivo:linha |
|---|---|---|---|
| **E4c F1 — juízes** | **Smoke: nada direto**, sem template/JS/view no alvo. **Batida/dinheiro:** migra predicados e fila de validação; efeito efetivo sobre esses caminhos **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE; BATIDA/DINHEIRO se alterar o caminho executor; PARADA 2; CADASTRO | `e4c_reconfere_v2.md:7` (anexo); `app/chamados/juizes.py:1528–1559`, `app/chamados/juizes.py:1722–1724` |
| **E4c F2 — modelos** | **Smoke: nada direto. Batida: conferir com Ronald o recorte de `_nascer`**, pertencente ao caminho de emissão alcançado pela batida; estado/seleção/apresentação isolados não demonstram esse efeito. **Dinheiro: [nao verificado]. Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; §4; PARADA 2; CADASTRO | `e4c_reconfere_v2.md:8` (anexo); `app/chamados/models.py:2922`, `app/chamados/models.py:3124–3131`; `app/chamados/services/fio.py:1161–1167` |
| **E4c F3 — validação/verbos/painel/ações** | **Smoke: nada direto. Batida: Ronald se mudar validação → materialização. Dinheiro:** validação agenda recálculo da competência **do fato**, aberta ou exportada conforme o caso; exportada gravada pede `!`. **Escala: `!` se atingir aceite de folga**, que chama `declarar_dia` → `lancar_folga`. **Motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; DINHEIRO 1–2; PARADA 2 / §4; CADASTRO | `app/chamados/services/validacao.py:346–347`, `app/chamados/services/validacao.py:460–487`; `app/ponto/services/declarar_dia.py:179–193`; `app/chamados/services/materializacao.py:932–940` |
| **E4c F4 — emissão/materialização/passivo** | **Smoke: nada direto. Batida: Ronald no recorte que alterar plantio. Dinheiro:** materialização → `registrar_batida` → fechamento por evento; passivo exportado tem caminho explícito **plantio → REGEN → Pauta DP**. **`!` para executar retificação exportada e regeneração de DNA/células.** Competências atingidas **[nao verificado]**. **Motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; DINHEIRO 1–2; PARADA 2 / §4; CADASTRO | `app/chamados/services/materializacao.py:932–940`, `app/chamados/services/materializacao.py:1012–1024`; `app/chamados/services/passivo_validado.py:433–449`; `app/ponto/portas/regen_exportada.py:87–104` |
| **E4c F5 — fio/reconciliador e leitores** | **Smoke: nada direto. Batida: Ronald para o recorte do reconciliador/contrato no caminho da batida. Dinheiro:** POST do fio pode chamar materialização, alcançando competência do fato; tarefa/vigia/apresentação isolados não demonstram movimento. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito; impacto real [nao verificado].** | BATIDA; DINHEIRO 1–2; PARADA 2; CADASTRO | `app/ponto/signals.py:206–215`; `app/chamados/reconciliador.py:1429–1442`; `app/chamados/services/fio.py:698–708`, `app/chamados/services/fio.py:1165` |
| **E4c F6 — views/API/canal/tags** | **Smoke:** Meu Atendimento, detalhe local, modal do fio e prévia/resultado do lote; nas duas cascas onde compartilhados. **API:** preservar fases públicas e adicionar exigibilidade; isso não demonstra alteração da API de bater ponto. **Batida/dinheiro:** seleção do lote alcança validação → materialização/recálculo; Ronald se esse caminho mudar, `!` se mover gravado exportado. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/chamados/views.py:550`, `app/chamados/views.py:632–633`, `app/chamados/views.py:927`; `app/chamados/views_cobrar.py:169–208`, `app/chamados/views_cobrar.py:353–386`; `app/api/urls.py:67–73`; `app/docs/BACKLOG.md:1999` |
| **E4c F7 — quatro partials** | **Smoke de Ronald:** Meu Atendimento/card, detalhe/pergunta, modal do fio e drawer do lote; **duas cascas** conforme o grafo. Pode ficar para depois **se** o diff conservar apenas apresentação/leitura, com reversão pronta. **Kernel direto: nada. Dinheiro:** nenhuma escrita demonstrada pelo recorte de apresentação; os formulários acionam portas existentes. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE + PRODUCAO-MAXIMA 5; PARADA 2; CADASTRO | `app/templates/chamados/meu_atendimento.html:1`, `app/templates/chamados/meu_atendimento.html:127`; `app/templates/chamados/detalhe_local.html:310`; `app/templates/chamados/partials/modal_fio.html:163–177`; `app/templates/chamados/partials/_lote_preview.html:21–26`; `app/core/context_processors.py:210` |
| **E4c F8 — ponto/calendário/ficha/prévia** | **Smoke:** calendário do perfil, cabeçalho do fio e Prévia da Folha se a mudança alcançar essas saídas; template/JS futuro **[nao verificado]**. **Batida: Ronald no recorte `flip_auto`**: leitura alimenta cron com `--apply` → `flip_tipo` → `registro_batida.anotar`. **Dinheiro:** prévia é leitura; alteração monetária decorrente do flip **[nao verificado]**. **`!` se houver apply que regrave célula/ata; motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; SMOKE se front; DINHEIRO se houver efeito; PARADA 2 / §4; CADASTRO | `app/colaboradores/views.py:1948–1965`; `app/chamados/views.py:918–930`; `app/folha/previa.py:28–31`; `app/ponto/services/flip_auto.py:128–140`; `app/config/crons.py:707`; `app/ponto/services/flip_batida.py:12–39`; `app/ponto/services/cartorio.py:951–956` |
| **E4c F9 — comandos** | **Smoke: nada. Migrar leitores não implica executar apply.** Se o pouso incluir `validar_classe_a --apply`, chama validação real: **Ronald para batida**, DINHEIRO na competência do fato e **`!` se alcançar gravado exportado**. Outros comandos escrevem atos/carimbos; universo/competências dos applies **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/chamados/management/commands/validar_classe_a.py:85–100`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:323–331`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:53–68`; `app/chamados/management/commands/relavrar_via_i6.py:96–102` |
| **E4d — CONTRACT** | **Arquivos do futuro corte: [nao verificado].** Conferência funcional identificável: fio e atendimento ao parar projeção. **Smoke obrigatório** depende de tocar front. **Kernel/dinheiro/escala/vínculo:** conservação dos efeitos após desligar projeção **[nao verificado]**; não cabe declarar “nada”. **Motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA/DINHEIRO conforme alcance; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1968`; `app/chamados/models.py:2525–2527`, `app/chamados/models.py:3982–3992`; `app/templates/chamados/partials/modal_fio.html:217–229`; `app/templates/chamados/partials/_card_atendimento.html:189–192` |
| **E4d — FATO-A03, reabertura** | **Smoke candidato:** fio, “Reabertura administrativa”; obrigatório se editar esse front. Porta atual escreve disputa/chamado/pergunta. **Batida/dinheiro diretos: nada nessa porta; alcance futuro [nao verificado]. Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1970`; `app/chamados/services/acoes_disputa.py:235–274`; `app/templates/chamados/partials/modal_fio.html:377–390` |
| **E4d — FATO-A05, assinatura** | **Tela/casca, kernel, dinheiro, competência e arquivos do pouso: [nao verificado].** O registro apenas nomeia “ramo de assinatura”; não sustenta escolher uma assinatura de escala como alvo. **Motor, apagar arquivo e cadastro novo: [nao verificado].** | SMOKE/BATIDA/DINHEIRO/PARADA 2/CADASTRO: enquadramento depende do sítio ainda não identificado | `app/docs/BACKLOG.md:1971`; [nao verificado] |
| **E4d — FATO-A06, identidade/duplicata** | **Smoke candidato:** atendimento mostrando repetida/via; obrigatório se editar front. Porta atual grava identidade/carimbo/motivo. **Batida/dinheiro diretos: nada nessa porta; alcance futuro [nao verificado]. Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1972`; `app/chamados/models.py:3251–3273`; `app/chamados/services/fio.py:1384–1401`; `app/templates/chamados/partials/_card_atendimento.html:192` |
| **E4d — FATO-A17, pendência técnica** | **Smoke candidato:** pendência no fio/card. **Batida/dinheiro:** fato está dentro do materializador; se a troca alterar prosseguimento para plantio, espera Ronald e pode alcançar fechamento da competência do fato, inclusive exportada → `!`. Delta real **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1973`; `app/chamados/services/materializacao.py:851–857`, `app/chamados/services/materializacao.py:932–940`; `app/templates/chamados/partials/modal_fio.html:222–226`; `app/templates/chamados/partials/_card_atendimento.html:189` |
| **E4d — FATO-A18, recusa técnica** | **Smoke candidato:** recusa/motivo no fio/card. **Batida/dinheiro:** mesma fronteira materializadora; mudança na recusa pode mudar plantio e recálculo da competência do fato. Ronald para kernel; **`!` se mover gravado exportado**; impacto **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/docs/BACKLOG.md:1974`; `app/chamados/services/materializacao.py:1089–1113`, `app/chamados/services/materializacao.py:932–940`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/_card_atendimento.html:192` |
| **E4d — FATO-A20, conferência de data** | **Smoke candidato:** fio, “Datas em disputa / Conferir data”. **Batida/dinheiro:** conferência libera gate materializador → plantio → fechamento da competência do fato; Ronald para kernel, **`!` se alcançar gravado exportado**. A regra “só o par conferido” já está decidida; implementação/impacto **[nao verificado]**. **Escala/vínculo, motor, apagar arquivo, cadastro: nada descrito.** | SMOKE se front; BATIDA; DINHEIRO; PARADA 2; CADASTRO | `app/chamados/services/validacao.py:660–709`; `app/chamados/views.py:1292–1314`; `app/templates/chamados/partials/modal_fio.html:163–177`; `app/chamados/services/materializacao.py:1199–1220`; `app/docs/CORTES.md:440` |
| **Retirada física posterior — campos legados** | Está registrada **depois da série**, separada de parar projeção. **Smoke/kernel/dinheiro e arquivos da migration: [nao verificado].** Remover campo não demonstra apagar arquivo de prod; **se apagar/voltar arquivo usado, `!`**. **Cadastro novo: nada descrito.** | PARADA 2; demais cláusulas dependem do diff | `app/docs/BACKLOG.md:1969`; `CLAUDE.md:265–268` |

## 15/11 09:52 · sessao 59206908181f · - · - · origem=Ronald (tmux)

F6–F9: evidências para a lista única:

| Fatia | Pedido antecipado identificado | Cláusula / prova |
|---|---|---|
| E4c F6 | Smoke de **Meu Atendimento**, **detalhe do chamado**, **modal do fio**, **validar inline/lote** e contrato dos endpoints de chamados/disputa. `views.py:632–633`, `:550`, `:927`; `views_cobrar.py:169`, `:208`, `:353–386`; `api/views.py:1159–1165`, `:2549–2563`, `:2611–2627`. A presença de `api/views.py` **não prova edição da API de batida**: `/ponto/bater/` é outra função/rota (`api/urls.py:67`), distinta de chamados/disputa (`:73`, `:92–93`). Há caminho transitivo **seleção → validar → materializar/recalcular**, portanto dinheiro e batida não podem ser descartados sem o diff: `views_cobrar.py:169–208` → `validacao.py:461–471`, `:487–494`. Competências/delta reais [nao verificado]. Novo cadastro, alteração de escala/vínculo, edição do motor ou exclusão de arquivo não estão demonstrados no recorte [nao verificado]. | BUG 73 `CLAUDE.md:202`; PARADA 2 `:265–268`; DINHEIRO `:406–417`; PROD-MÁXIMA item 5, anexo. |
| E4c F7 | Smoke de **Meu Atendimento nas duas cascas**, **pergunta no detalhe DP**, **modal do fio**, **prévia/resultado de validar lote**: `_card_atendimento.html:106`, `:114`, `:141–154`; `modal_fio.html:185`, `:217–228`; `_pergunta_card.html:20–42`; `_lote_preview.html:21–26`. Casca colaborador: `meu_atendimento.html:1`, `:127`, `:144` + `core/context_processors.py:210`; detalhe DP: `detalhe_local.html:1`, `:310`; modal servido por `chamados/views.py:927`; lote servido por `views_cobrar.py:353–386`. Os quatro alvos são templates; não demonstram edição do kernel, motor, cadastro ou dados de escala/vínculo. O clique de validação já chama a porta que materializa/recalcula (`views_cobrar.py:379–381`; `validacao.py:461–494`), portanto o smoke de escrita não deve ser confundido com simples inspeção da tela (`CLAUDE.md:205`). Delta monetário/competências [nao verificado]. | BUG 73 `CLAUDE.md:202–203`; smoke de POST `:205`; PROD-MÁXIMA item 5 admite pouso com smoke depois **somente quando for só tela**, com reversão pronta, anexo. |
| E4c F8 | Smoke das superfícies **Calendário do perfil**, **procedência no espelho/PDF**, **cabeçalho/ficha do modal do fio**, **Prévia de proposta da folha**: `colaboradores/views.py:1938`, `:1948–1965`; `ponto/services/espelho.py:755`; `relatorios/pdf_espelho.py:950`; `chamados/views.py:918`, `:930`; `modal_fio.html:13`; `folha/views.py:701–723`. **Não é fatia só de apresentação**: `flip_auto.py:128–140` lê projeção para montar fila → `:268` → cron `config/crons.py:707` (`--competencia --apply`) → `flip_automatico.py:25–31`, `:68` → `flip_batida.py:12`, `:30–39` → **`registro_batida.anotar` e recompute**. Este ramo afeta kernel/escrita de Batida e pede Ronald antes pelo item 5. Não edita `registro_batida.py` diretamente no conjunto-alvo do anexo. Janela do cron é a aberta (`flip_automatico.py:25–31`); delta/competências reais [nao verificado]. Prévia em si é leitura (`folha/previa.py:29–31`, `:139–145`). Cartório escreve campos de `CelulaDia`, mas não DNA/vínculo (`cartorio.py:951–956`); se o pouso incluir apply de relavratura desses dados de ESCALA, pede **`!`**. Edição de `motor_calculo_v2.py`, exclusão de arquivo e cadastro novo não estão demonstrados [nao verificado]. | Seção 4 `CLAUDE.md:142–153`; PARADA 2 `:265–268`; DINHEIRO `:406–417`; PROD-MÁXIMA item 5, anexo. |
| E4c F9 | **Tela direta: nada**, pois os alvos do anexo são comandos. Smoke indireto/execução de apply não decorre automaticamente de trocar os leitores; operações efetivamente incluídas no pouso [nao verificado]. **Não declarar “nada de batida/dinheiro”** se executar `validar_classe_a --apply`: `validar_classe_a.py:85–100` chama `validar_pergunta`, que materializa e recalcula (`validacao.py:461–494`). Outros applies escrevem atos/perguntas: `backfill_decisoes_pergunta.py:323–331`; `encerrar_perguntas_com_fato.py:53–68`; `relavrar_via_i6.py:96–102` → `services/relavra_via.py:106–128`; `migrar_data_evento_dia_do_turno.py:393–400`; `reconciliar_fantasmas.py:269–281`; `reconciliar_grade.py:305–316`. Backfill/migração já excluem exportadas (`backfill_decisoes_pergunta.py:302–308`; `migrar_data_evento_dia_do_turno.py:384–398`). Alterar estado de pergunta em exportada não equivale, por si, a mexer no `FechamentoMensal`; apply monetário real/competências [nao verificado]. Novo cadastro, mudança de escala/vínculo, edição do motor e exclusão de arquivo prod [nao verificado]. | DINHEIRO `CLAUDE.md:406–417`; PARADA 2 `:265–268`; kernel pelo caminho de validação e PROD-MÁXIMA item 5, anexo. |

Observações para a consolidação:

- **F8 é o caso que mais facilmente receberia “nada” indevido**: a fila de recusas abastece cron escritor de Batida pela porta `registro_batida.anotar`. `flip_auto.py:128–140`; `config/crons.py:707`; `flip_automatico.py:68`; `flip_batida.py:12`, `:30–39`.
- `cartorio.py:951–956` distingue a relavratura de veredito/ata da regeneração de DNA. O leitor R identificado pela E4c está em `cartorio.py:1637–1641`; esse ramo documenta reativação de perguntas sem tocar FechamentoMensal/batida/TXT (`:1627–1628`), mas não prova ausência de efeito de todo o arquivo ou do diff futuro.
- O cadastro exigido por “TUDO TEM CADASTRO” só nasce se a mudança introduzir regra que varie por cliente/praça/empresa (`CLAUDE.md:43–44`). Não há essa regra nova demonstrada no inventário F6–F9 do anexo; afirmar necessidade de cadastro novo seria [nao verificado].
- Excluir um arquivo só por achá-lo órfão é diferente de trocar leitores: exclusão de arquivo usado por prod pede `!` (`CLAUDE.md:265–267`). O inventário não demonstra exclusões futuras [nao verificado].

## 15/11 09:52 · sessao 479eee87c4e0 · ESPINHA-NA-FRENTE · leitura · origem=Code

**Veredito: depende do alcance efetivo dos itens 18–19. O zero atual não basta para dispensar a autorização.** A regra exige `!` para tocar o GRAVADO de competência exportada; a correspondência dos dois itens com funções e campos concretos permanece **[não verificado]**. ([CLAUDE.md:265](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/CLAUDE.md:265))

**De onde nasce a exigência**

- Regra geral, literal: “**tocar o GRAVADO de competencia EXPORTADA**” está entre os atos que exigem `!`, nunca previamente aprovados. ([CLAUDE.md:265](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/CLAUDE.md:265))
- Regra de dinheiro, literal: “**o GRAVADO (`FechamentoMensal`) nao muda por caminho generico; e `!`**”, com DIFF anterior, reversão e prova posterior. ([CLAUDE.md:411](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/CLAUDE.md:411))
- Na obra, o BACKLOG manda deixar exportadas “em lista para o `!`”; a GRADE registra explicitamente “itens 18–19; autorização das exportadas”. ([BACKLOG.md:1977](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1977), [GRADE-MESTRA.md:717](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/GRADE-MESTRA.md:717))

**O número sustenta somente a fotografia fornecida**

São **581 dias = 581 não exportados + 0 exportados + 0 indeterminados**; existem **7 competências exportadas**. Para A18/A19, a sonda informa “sem alcance chamável”, com `linhas=null` e `mudariam_campo_lido_gravado_TXT=null`: esses valores não demonstram delta zero nem ausência de caminho futuro. (*m5_sonda.out*, anexo fornecido:10, :12 e :3–9; linhas contadas desde `sonda_leitura`.)

O BACKLOG chama os itens 18–19 de “FIM DO PROGRAMA”, e o registro do aval confirma uma lista de 26 itens, mas essas referências não descrevem seus campos ou funções. Equiparar essa numeração a A18/A19 seria **[não verificado]**. ([BACKLOG.md:1977](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1977), [PROMPTS.md:2141](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/PROMPTS.md:2141))

**O alcance comprovável do código**

| Caminho | O que o código demonstra |
|---|---|
| Backfill E4B existente | `--aplicar` exige competência corrente; o ato consulta novamente se o dia está entregue e o recusa antes da execução. Portanto, sua proteção não depende apenas do censo de hoje. ([backfill_fio_morto_e4b.py:223](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:223), [:304](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:304)) |
| Cascatas desse backfill | A cerca recusa linha alterada em exportada, fora da competência ou de alcance indeterminado; a tentativa sofre rollback. A fotografia inclui `FechamentoMensal` e `DiaPago`; DML em tabela fora dela também causa recusa. ([foto_e4b.py:18](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/foto_e4b.py:18), [:145](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/foto_e4b.py:145), [backfill_fio_morto_e4b.py:324](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:324)) |
| Comando executado depois | `recalcular_fechamento --apply` chama `recalcular_fechamento_mes` pela competência solicitada. O serviço tem guarda nominal para **09/2026**, não uma recusa geral de exportadas; grava FM e chama a lavra de DiaPago. Há, portanto, caminho de escrita em outras competências exportadas, independentemente dos 581 dias atuais. ([recalcular_fechamento.py:118](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/recalcular_fechamento.py:118), [fechamento.py:36](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:36), [:560](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:560), [:1160](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:1160), [:1191](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:1191)) |

Ligar uma alteração dos itens 18–19 a esse comando posterior, ou provar que eles ficam integralmente contidos na cerca do backfill, permanece **[não verificado]**.

**Quem escreve e quem lê**

Para **cada campo que os itens 18 e 19 gravariam**, campo, escritor e leitores estão **[não verificado]**: falta a definição dos itens e a sonda devolveu `null`, não uma relação de campos. (*m5_sonda.out*, anexo:3–9.)

Os destinos financeiros comprováveis são estes; sua atribuição aos itens permanece **[não verificado]**:

| Dados | Quem escreve | Quem lê |
|---|---|---|
| Campos calculados de `FechamentoMensal` | `recalcular_fechamento_mes`, incluindo horas, faltas, DSR, minutos e causas. ([fechamento.py:1160](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/fechamento.py:1160)) | `montar_linhas_export` consulta o FM gravado e entrega seus valores a `eventos_do_fechamento`. ([export.py:719](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/folha/export.py:719)) |
| Linhas de `DiaPago` | `dia_pago.lavrar`, que apaga a versão do escopo e recria as linhas. ([dia_pago.py:359](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/dia_pago.py:359)) | Leitores por campo e influência dos itens 18–19: **[não verificado]**. |
| `ExportacaoDominio.conteudo`, `hash_sha256` e metadados | `ExportacaoDominio.registrar`. ([models.py:174](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/folha/models.py:174)) | `competencia_entregue` lê vigência; `txt_vigente_x_gravado` lê o hash. ([export.py:588](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/folha/export.py:588), [:673](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/folha/export.py:673)) |

**Depende de X:** se os itens preservarem uma exclusão efetiva de qualquer escrita em exportada, essa autorização específica não é acionada; se alcançarem o GRAVADO exportado, inclusive por execução posterior, ela continua necessária. **Com a evidência disponível, não está demonstrada a dispensa para os itens 18–19.** ([CLAUDE.md:265](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/CLAUDE.md:265); correspondência item → campo → caminho: **[não verificado]**.)

## 15/11 09:51 · sessao fe287b9b65c1 · - · - · origem=Ronald (tmux)

Não foi localizado mapa confiável dos itens **18–19 do PRONTO** para funções/campos: `app/docs/BACKLOG.md:1977` apenas os chama de “FIM DO PROGRAMA”; `app/docs/PROMPTS.md:2141` informa N=26 e repete isso. Não confundir com A18/A19 da matriz de projeções, que representam outro inventário (`app/docs/PROMPTS.md:2034`; `app/docs/BACKLOG.md:1974`).

**O backfill existente tem cerca própria, independente do número de hoje:**

- Apply só na competência corrente: `app/chamados/management/commands/backfill_fio_morto_e4b.py:223`.
- Plano retira entregue/indeterminada dos executáveis: mesmo arquivo `:256`–`:262`.
- Revalidação de entregue dentro do ato: `:303`–`:308`.
- Delta de todos os campos concretos fotografados e rollback quando cruza competência/exportada: `:319`–`:334`; `app/chamados/services/foto_e4b.py:145`.
- Foto inclui FechamentoMensal e DiaPago (`foto_e4b.py:289`–`:290`); ExportacaoDominio não está na lista de tabelas permitidas (`:18`–`:27`), portanto DML nele cairia na recusa de tabela fora da foto (`backfill_fio_morto_e4b.py:325`–`:334`).
- Callbacks desconhecidos ou capazes de escapar do recorte recusam o ato: `foto_e4b.py:30`–`:84`.

**Mas o serviço compartilhado permite alcançar gravado de exportada:**

`recalcular_fechamento --apply` → `app/ponto/management/commands/recalcular_fechamento.py:118` → `recalcular_fechamento_mes` → persistência FM `app/ponto/services/fechamento.py:1144` / `:1160`, e DiaPago `:1191`–`:1201` → `app/ponto/services/dia_pago.py:359`–`:360`.

A guarda é nominal para **09/2026**, não geral: `fechamento.py:36`–`:43`, aplicada em `:560`–`:564`. O serviço declara que exportada deixou de ser trava (`:485`–`:498`) e o código só registra trilha (`:539`–`:547`). Portanto, uma execução futura noutra competência exportada pode escrever FM/DiaPago; influência dos itens 18–19 nesse caminho é **[não verificado]**. [nao verificado]

Caminho por evento também existe: `registro_batida.py:158`–`:180` → `fechamento.py:1809`–`:1824`. Porém materialização chama a porta com `fato_de_chao=False` (`materializacao.py:932`–`:939`), que consulta `competencia_fechada` (`registro_batida.py:132`–`:137`), barrando FM aprovado/exportado (`janelas.py:75`–`:89`). Essa guarda lê status do FM, não K6/ExportacaoDominio (`janelas.py:83`–`:88`).

**Exemplo de campos/escritores/leitores comprovado, somente se o item corresponder ao registro do passivo:**

`backfill_fio_morto_e4b.py:138`–`:141` chama `DecisaoPergunta.registrar(linhas_da_projecao_sem_linha(ps))`.

| Campos gravados | Quem escreve | Quem lê |
|---|---|---|
| `pergunta_id`, `ato`, `ato_em`, `ato_texto`, `ato_motivo`; na validação também `ato_por_id` | Valores montados em `app/chamados/models.py:4073`–`:4082`; persistência única `:4086`–`:4098` | `PerguntaDisputa.respostas_anteriores` lê a sequência e `ato` em `:3643`–`:3659`; tarefa lê último `ato`/`ato_texto` em `app/chamados/services/tarefa_do_dia.py:147`–`:161`; foto lê todos os campos concretos em `foto_e4b.py:247`–`:252`, `:280` |

O registro também acorda EventoCelula (`models.py:4097`–`:4098`; `:3314`–`:3331`), portanto nem esse ato deve ser tratado como inserção sem consumidores.

**Leitores do dinheiro:** `folha/export.py:782`–`:788` carrega FM gravado para classificação; rubricas leem `horas_extras_50`, extras100/feriado, folga trabalhada, noturnas e intrajornada em `:420`–`:448`. Isso comprova consumidores do gravado, mas não identifica quais campos os itens 18–19 mudariam **[não verificado]**.

Recomendação de veredito: **depende de X = definição exata dos itens 18–19 e sua vinculação a escritores/campos**. Zero atual sustenta ausência medida na fotografia fornecida; não sustenta ausência estrutural de caminho futuro.

## 15/11 09:51 · sessao 8a636440cc56 · - · - · origem=Ronald (tmux)

F1 | **nada diretamente pelo escopo leitor**; nenhum template/JS/view alvo no anexo, portanto BUG 73 não cria smoke obrigatório só por trocar o predicado. Filas de validação são alcançadas; eventual alteração de batida/dinheiro pelo diff futuro **[nao verificado]**; cadastro novo e remoção de arquivo **[nao verificado]** | BUG 73; PARADA 2 | `app/chamados/juizes.py:1528`, `app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1554`, `app/chamados/juizes.py:1723`; `CLAUDE.md:202`, `CLAUDE.md:265`.

F2 | **nada diretamente para estado/seleção/apresentação**; a guarda de `_nascer` está em caminho quente de emissão, portanto conferir impacto de kernel; não há tela alvo direta nem cadastro variável novo demonstrado. Dinheiro, remoção de arquivo e diff final **[nao verificado]** | BUG 73; §4; TUDO TEM CADASTRO; PARADA 2 | `app/chamados/models.py:2903`, `app/chamados/models.py:2922`, `app/chamados/models.py:3117`, `app/chamados/models.py:3454`, `app/chamados/models.py:3640`, `app/chamados/models.py:3662`; `app/chamados/services/fio.py:1165`; `CLAUDE.md:202`, `CLAUDE.md:265`.

F3 | **esperar Ronald se o recorte mudar efeito de validação/batida ou dinheiro pago; `!` se mudar o ramo que grava folga**. Nenhum front alvo direto; validar/recusar aparece no detalhe/modal, mas smoke de clique obrigatório por BUG 73 somente por esse alcance **não está demonstrado**. Caminho real: validação → materialização → `registrar_batida`; validação também agenda fechamento na competência do fato, inclusive exportada. Cadastro novo/remoção de arquivo **[nao verificado]** | PRODUCAO-MAXIMA 5; PARADA 2; DINHEIRO 1–2; §4 | `app/chamados/services/validacao.py:346`, `app/chamados/services/validacao.py:460`, `app/chamados/services/validacao.py:470`; `app/ponto/services/declarar_dia.py:179`, `app/ponto/services/declarar_dia.py:191`; `app/chamados/services/materializacao.py:932`, `app/chamados/services/materializacao.py:1012`; `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1160`; `app/chamados/views.py:404`, `app/chamados/views.py:550`.

F4 | **esperar Ronald no alcance de plantio/retificação; `!` para executar o passivo em exportada com regeneração de DNA/gravado**. Nenhum front alvo direto. Caminho real: materialização planta batida; passivo planta → `regenerar_em_exportada` → Pauta DP quando mudou; alcança competência do dia do plantio. Quantas abertas/exportadas o pouso atingirá **[nao verificado]**. Cadastro novo/remoção de arquivo **[nao verificado]** | PRODUCAO-MAXIMA 5; PARADA 2; DINHEIRO 1–2; §4 | `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:932`, `app/chamados/services/materializacao.py:1012`; `app/chamados/services/passivo_validado.py:344`, `app/chamados/services/passivo_validado.py:433`, `app/chamados/services/passivo_validado.py:435`, `app/chamados/services/passivo_validado.py:437`, `app/chamados/services/passivo_validado.py:442`, `app/chamados/services/passivo_validado.py:448`.

F5 | **esperar Ronald para o recorte de kernel**: reconciliador roda por batida e fio tem guarda no caminho do chokepoint; nenhum template/JS/view alvo direto, então não há smoke obrigatório direto demonstrado por BUG 73. Dinheiro só pelo alcance comprovado de `processar_post_fio` → materialização; tarefa/Folha lê porta e não demonstra mover dinheiro por si. Cadastro novo, dado ESCALA/VÍNCULO, remoção de arquivo e impacto efetivo **[nao verificado]** | PRODUCAO-MAXIMA 5; §4; PARADA 2; DINHEIRO | `app/ponto/signals.py:206`, `app/ponto/signals.py:214`; `app/chamados/reconciliador.py:1429`, `app/chamados/reconciliador.py:1433`; `app/chamados/services/fio.py:1165`, `app/chamados/services/fio.py:701`, `app/chamados/services/fio.py:708`; `app/folha/fora_da_folha.py:63`, `app/folha/fora_da_folha.py:69`.

Cuidados para consolidar:

- **Não declarar exportada intacta em F3/F4:** o código real chama `recalcular_fechamento_mes` por evento; ele persiste aberto via `save` e demais status via `update`. Há apenas guarda específica de competência, sem veto genérico de exportada. `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1160`, `app/ponto/services/fechamento.py:558`.
- Telas de alcance, caso o recorte exija smoke: Central de Atendimento/lote, detalhe local, modal do fio, Meu Atendimento. A casca de Meu Atendimento é dinâmica; contexto escolhe `base.html` ou `base_app.html`. Isso é alcance funcional, não prova de que F1–F5 editam front. `app/templates/chamados/painel_gestao.html:14`, `app/chamados/views.py:550`, `app/chamados/views.py:927`, `app/chamados/views.py:632`, `app/templates/chamados/meu_atendimento.html:1`, `app/core/context_processors.py:210`.
- O anexo lista arquivos futuros, sem diff construído. Assim não há fundamento para prometer que não apagará arquivo, não tocará ramo monetário ou não exigirá cadastro; use **[nao verificado]** nesses negativos se a lista precisar ser categórica. [nao verificado]

## 15/11 09:51 · sessao 55637bbee891 · - · - · origem=Ronald (tmux)

E4d está descrita como CONTRACT para parar a projeção; os seis fatos próprios continuam registrados separadamente, “depois da E4d”, com projeção preservada até existirem. O aval fornecido pede E4d “com os fatos”; partição final e arquivos futuros não estão demonstrados. `app/docs/BACKLOG.md:1968`, `app/docs/BACKLOG.md:1970–1975`; [nao verificado].

| Fatia | Evidência para a linha consolidada |
|---|---|
| E4d CONTRACT | Parar escrita de projeção de resposta/validação exige conferir leitores do fio e atendimento: campos atuais em `app/chamados/models.py:2525–2527`, sequência em `app/chamados/models.py:3982–3992`, fio em `app/templates/chamados/partials/modal_fio.html:217–229`, atendimento em `app/templates/chamados/partials/_card_atendimento.html:189–192`. Smoke das telas é identificável; tocar template/base/JS, kernel, dinheiro, escala/vínculo, apagar arquivo e cadastro novo no diff futuro: [nao verificado]. |
| E4d FATO-A03 | Reabertura administrativa: `app/chamados/models.py:2190–2225`; casca executa a porta e cria pergunta de conversa: `app/chamados/services/acoes_disputa.py:235–274`; smoke do fio, “Reabertura administrativa”: `app/templates/chamados/partials/modal_fio.html:377–390`. Escritas demonstradas são disputa/chamado/pergunta, sem cadastro novo ou gravação direta de dinheiro/batida nessa porta. `app/chamados/services/acoes_disputa.py:259–274`. Alcance futuro [nao verificado]. |
| E4d FATO-A05 | BACKLOG só identifica “ramo de assinatura”; sítio inequívoco e casca específica não determinados: `app/docs/BACKLOG.md:1971`; [nao verificado]. Não confundir com `assinatura_proposta` do fio, que pertence à proposta de escala: `app/chamados/services/proposta_no_fio.py:96–113`. |
| E4d FATO-A06 | Identidade/duplicata escreve carimbo/via/motivo: `app/chamados/models.py:3251–3273`; dedup chama essa porta: `app/chamados/services/fio.py:1384–1401`. Smoke do atendimento exibindo “pergunta repetida”/via: `app/templates/chamados/partials/_card_atendimento.html:192`. Kernel e dinheiro não são escritos diretamente nessa porta; alcance futuro [nao verificado]. |
| E4d FATO-A17 | Pendência técnica escreve `pendencia_admin` + motivo dentro do materializador: `app/chamados/services/materializacao.py:851–857`; smoke fio e atendimento: `app/templates/chamados/partials/modal_fio.html:222–226`, `app/templates/chamados/partials/_card_atendimento.html:189`. Caminho é vizinho do plantio pelo chokepoint: `app/chamados/services/materializacao.py:932–940`; mudança efetiva no plantio/dinheiro depende do futuro diff [nao verificado]. |
| E4d FATO-A18 | Recusa técnica: `app/chamados/services/materializacao.py:1089–1113`; escreve motivo e, em colisão, identidade; anexa nota no fio. Smoke fio/atendimento: `app/templates/chamados/partials/modal_fio.html:226`, `app/templates/chamados/partials/_card_atendimento.html:192`. Integra caminho materializador; efeito futuro no plantio/dinheiro [nao verificado]. |
| E4d FATO-A20 | Conferência de data: `app/chamados/services/validacao.py:660–709`; view `app/chamados/views.py:1292–1314`; smoke “Datas em disputa / Conferir data”: `app/templates/chamados/partials/modal_fio.html:163–177`. Destrava gate materializador: `app/chamados/services/materializacao.py:1199–1220`; plantio subsequente alcança chokepoint em `app/chamados/services/materializacao.py:932–940`. Regra do par já decidida: `app/docs/CORTES.md:440`; implementação dessa decisão não demonstrada. |

**Dinheiro: cuidado material, sem inventar universo afetado.** Materializador chama `registrar_batida`; batida gravada chama fechamento por evento; fechamento recalcula a competência do fato em `on_commit`. `app/chamados/services/materializacao.py:932–940`, `app/ponto/registro_batida.py:158–180`, `app/ponto/services/fechamento.py:1798–1824`. A chamada real não filtra exportada, e o comentário explicita recalcular exportada também. `app/ponto/services/fechamento.py:1790–1793`, `app/ponto/services/fechamento.py:1809–1810`. Portanto, A17/A18/A20 e CONTRACT não podem receber “dinheiro=nada” sem provar que a migração conserva o caminho; competências, valores e frota atingidos são [nao verificado].

**Cláusulas para consolidar:** aberta pode aplicar com reversão anterior, hash das exportadas intacto e prova de frota posterior; exportada gravada exige `!` e porta REGEN. `CLAUDE.md:406–417`. Kernel de batida aguarda Ronald segundo PRODUCAO-MAXIMA item 5 fornecido; regra de chokepoint está em `CLAUDE.md:143–144`. Escala/vínculo, motor ou apagar/voltar arquivo usado por produção exigem `!`; nenhum desses atos consta do escopo resumido dos seis fatos, mas diff futuro permanece [nao verificado]. `CLAUDE.md:265–268`, `app/docs/BACKLOG.md:1970–1975`. Novo cadastro de regra variável depende de UI; nenhuma variação por cliente/praça/empresa está descrita nesses fatos. `CLAUDE.md:43–44`, `app/docs/BACKLOG.md:1970–1975`.

## 15/11 09:49 · sessao 5ac9eee1605f · E3-RESTO-R3 · construtor · origem=Code

Os 11 testes foram reescritos pela lei (b), com antes/depois e provas conservadas na [entrega](/home/ronald/wt-codex-e3-resto-r3-093851/app/chamados/tests/e3r3_c9_entrega.txt:27). [nao verificado]

Os 11 pares mutante/teste e controles GREEN→RED→GREEN estão no [script](/home/ronald/wt-codex-e3-resto-r3-093851/app/chamados/tests/e3r3_c9_mutantes.py:26). [nao verificado]

[Ruff e py_compile passaram](/home/ronald/wt-codex-e3-resto-r3-093851/app/chamados/tests/e3r3_c9_entrega.txt:221). Suíte e mutantes não foram executados; resultados funcionais: [nao verificado]. [nao verificado]

## 15/11 09:49 · sessao d533b23fcb35 · JUSTIFICATIVA-PAGA-O-PERIODO · censo · origem=Code

**A guarda atual não protege a 10/2026 por pagamento externo.** Ela exige simultaneamente a chave `(9, 2026)` e empresa exportada; o teste da 10 exige que o evento regrave o fechamento. O marco chamado `paga` consulta holerite de lote publicado, sem consultar declaração de pagamento manual. (`app/ponto/services/fechamento.py:37`, `app/ponto/services/fechamento.py:560`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:200`, `app/ponto/services/fechamento.py:1552`)

**1. Leitores da guarda e do contador**

| Leitor | Significado da chave e leitura por empresa |
|---|---|
| `recalcular_fechamento_mes` | Usa `(mes, ano)` da **competência de apuração**, com janela pelo corte da empresa. Consulta a guarda e exclui somente colaboradores das empresas exportadas. A chave sozinha não bloqueia empresa sem entrega. (`app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:560`, `app/ponto/services/fechamento.py:562`, `app/ponto/services/fechamento.py:620`) |
| `dia_pago.retratar_exportada` | Usa a competência das linhas de `FechamentoMensal`/`DiaPago`. Exige chave guardada e empresa exportada, encontrada por `colaborador_ids=[colab.pk]`; devolve `barrado_pela_guarda=True`. (`app/ponto/services/dia_pago.py:459`, `app/ponto/services/dia_pago.py:468`, `app/ponto/services/dia_pago.py:469`) |
| `barrados_pela_guarda` | Lê `ACAO_BARRADO_GUARDA` e conta registros de `LogAuditoria` cujo texto começa com `MM/AAAA `. Não calcula janela nem filtra empresa. (`app/ponto/services/fechamento.py:47`, `app/ponto/services/fechamento.py:50`) |
| Command `recalcular_fechamento` | Depois do apply, testa a chave e imprime contador acumulado e `saida`. O recálculo pode receber empresa; o contador impresso recebe somente mês/ano. (`app/ponto/management/commands/recalcular_fechamento.py:118`, `app/ponto/management/commands/recalcular_fechamento.py:123`, `app/ponto/management/commands/recalcular_fechamento.py:126`) |
| `test_bob_b2_abono_feriado_guarda` | Cenário com corte 21 e exportação 09: exige 09 preservada, contador 1; 10 regravada, contador 0; leitura pura da 09 permitida; retrato da 09 barrado. (`app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:79`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:175`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:192`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:200`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:206`, `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:211`) |
| `test_reverter_regen_recalcula_dinheiro` | Lê o contador antes/depois da reversão de célula em 09 exportada; exige célula revertida, dinheiro preservado e contador aumentado. O contador continua sem empresa. (`app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:197`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:205`, `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:211`) |
| `test_justificativa_c9_sem_banco` | Injeta guarda vazia e ação fictícia para testar retrato sem ata; não caracteriza competência nem entrega real. (`app/ponto/tests/test_justificativa_c9_sem_banco.py:138`, `app/ponto/tests/test_justificativa_c9_sem_banco.py:150`) |

`ACAO_BARRADO_GUARDA='recalculo_barrado_guarda'` é escrita pelo recálculo barrado, com modelo `FechamentoMensal`, e pelo retrato barrado, com modelo `DiaPago`; seu leitor funcional é o contador acima. (`app/ponto/services/fechamento.py:44`, `app/ponto/services/fechamento.py:50`, `app/ponto/services/fechamento.py:569`, `app/ponto/services/dia_pago.py:463`)

O contador mede **registros de bloqueio**, sem somar pessoas: uma chamada pode excluir vários colaboradores e produzir uma linha, com quantidade no texto e IDs no JSON. Ele também inclui o bloqueio do retrato. (`app/ponto/services/fechamento.py:569`, `app/ponto/services/fechamento.py:570`, `app/ponto/services/fechamento.py:573`, `app/ponto/services/dia_pago.py:463`)

A competência termina no mês da chave: com corte 21, `10/2026` significa `21/09–20/10`; o corte vem de `Empresa.dia_inicio_competencia`, com default 21. O corte efetivo cadastrado de Juliani em produção é [nao verificado]. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`)

**2. Candidatas a responder exportada, entregue, paga, fechada ou trancada**

O registro vivo declara separadamente F6, K4, K6, K7 e K9; ele distingue expressamente fechamento por colaborador/status e tranca por empresa. (`app/core/juizes.py:76`, `app/core/juizes.py:309`, `app/core/juizes.py:326`, `app/core/juizes.py:337`, `app/core/juizes.py:338`, `app/core/juizes.py:347`)

| Candidata | Predicado, universo e fonte |
|---|---|
| `competencia_entregue` — K6 | **Empresa + mês + ano**: seleciona o `ExportacaoDominio` mais recente por `(gerado_em, pk)` e exige `invalidada_em is None`. Retorna bool ou registro. Responde entrega por TXT. (`app/folha/export.py:559`, `app/folha/export.py:591`, `app/folha/export.py:594`, `app/folha/models.py:203`) |
| `empresas_exportadas_no_escopo` | Projeta a pergunta anterior: empresa indicada; senão empresas dos colaboradores indicados; senão empresas ativas. Retorna IDs cujo marco `exportada` é verdadeiro. Lista vazia de colaboradores produz escopo vazio. (`app/ponto/services/fechamento.py:1509`, `app/ponto/services/fechamento.py:1521`, `app/ponto/services/fechamento.py:1528`) |
| `marcos_da_competencia()['aprovada']` | **Empresa + competência**: existe ao menos um `FechamentoMensal` de colaborador da empresa com status `aprovado` ou `exportado`. (`app/ponto/services/fechamento.py:1546`) |
| `marcos_da_competencia()['exportada']` | Chama K6; mesma pergunta de entrega por TXT. (`app/ponto/services/fechamento.py:1550`) |
| `marcos_da_competencia()['trancada']` | Chama `competencia_trancada`; mesma pergunta da tranca viva. (`app/ponto/services/fechamento.py:1551`) |
| `marcos_da_competencia()['paga']` | **Empresa + competência**: existe `Holerite` com lote publicado, atribuído pela `empresa_folha` ou, se ela for nula, pela empresa atual do colaborador. Um único documento satisfaz o `exists()`; não exige todos os colaboradores nem prova bancária. (`app/ponto/services/fechamento.py:1552`) |
| `marco_da_competencia` | Retorna o verdadeiro mais adiantado na ordem `aprovada → exportada → trancada → paga`, ou `aberta`. É projeção dos marcos. (`app/ponto/services/fechamento.py:1474`, `app/ponto/services/fechamento.py:1555`, `app/ponto/services/fechamento.py:1566`) |
| `competencia_trancada` — K4 | **Empresa + competência**: existe `PeriodoFechado` com `reaberto_em IS NULL`. Retorna bool; sem empresa retorna falso. (`app/ponto/services/fechamento.py:1486`) |
| `periodo_trancado` — K7 | Exatamente o mesmo filtro do K4, devolvendo linha ou `None`. `ativo` não participa da decisão. (`app/ponto/services/fechamento.py:1477`, `app/ponto/services/fechamento.py:1483`) |
| `janelas.competencia_fechada` — F6, “mês fechado” | **Colaborador + instante**: converte para data local, encontra competência pelo corte da empresa e procura FM `aprovado`/`exportado`. Retorna mensagem ou `''`. Não consulta TXT, holerite ou tranca. (`app/ponto/janelas.py:75`, `app/ponto/janelas.py:84`) |
| `competencia_fechada_para_celula` — K9 | **Empresa + competência + colaborador opcional**: K6 **OU** holerite do próprio colaborador com lote publicado. Sem colaborador, somente TXT conta; `fontes=True` informa as metades verdadeiras. (`app/folha/export.py:600`, `app/folha/export.py:620`) |
| `_competencias_exportadas`, `fonte_da_lavra`, `autoridade_da_lavra` | Enumeram candidatos de TXT/holerite, consultam K9 e convertem em janelas pelo corte da empresa; apresentam qual fonte intercepta o recorte. (`app/ponto/management/commands/gerar_celulas.py:135`, `app/ponto/management/commands/gerar_celulas.py:170`, `app/ponto/management/commands/gerar_celulas.py:182`, `app/ponto/management/commands/gerar_celulas.py:192`, `app/ponto/management/commands/gerar_celulas.py:217`) |
| `dia_em_competencia_trancada` | **Empresa + dia**: enumera `PeriodoFechado` sem reabertura e testa pertencimento à janela. Sem empresa/dia retorna falso; trata falhas com abertura. (`app/chamados/juizes.py:2041`, `app/chamados/juizes.py:2051`) |
| `dia_da_pergunta_trancado` / `dias_trancados` — C20 | **Colaborador + dia de turno**: tranca viva **e ausência de toque administrativo posterior naquele colaborador/dia**. Lê K7 e trilha de cobrar/resolver/desarquivar. (`app/chamados/juizes.py:974`, `app/chamados/juizes.py:1004`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:1076`, `app/chamados/juizes.py:1083`) |
| `ChamadoColaborador._dia_em_competencia_trancada` | Excetua módulos fora da tranca, obtém a competência do chamado e consulta K4. Decide escopo do chamado. (`app/chamados/models.py:1080`) |
| `competencia_do_dia_entregue` | **Empresa + dia**: compõe `janela_atual` e K6. Sem dia retorna `None`; memo por empresa/competência. (`app/chamados/services/vigia_fio.py:827`, `app/chamados/services/vigia_fio.py:839`) |
| `autorizacao_he_periodo._exportada` | Testa se a empresa do colaborador pertence a `empresas_exportadas_no_escopo`; mesma entrega TXT. (`app/ponto/services/autorizacao_he_periodo.py:113`) |
| `tripwire_celulas._exportada` | Testa se a data cai nas janelas de `_competencias_exportadas(empresa)`. Sem colaborador, alcança somente a metade TXT do K9. (`app/ponto/management/commands/tripwire_celulas.py:318`, `app/folha/export.py:623`) |
| `ExportacaoDominio.e_vigente`, `anotar_vigencia`, `so_vigentes` | Respondem **qual geração é a mais recente** por empresa/competência; não excluem o registro por `invalidada_em`. (`app/folha/models.py:120`, `app/folha/models.py:134`, `app/folha/models.py:192`, `app/folha/models.py:203`) |
| `exportacoes_da_competencia` / `passivo_validado._esta_exportada` | O primeiro devolve o mais recente por `e_vigente`; o segundo testa `vigente is not None`. Um último TXT invalidado pode dar verdadeiro aqui e falso no K6. (`app/folha/haiku_export.py:59`, `app/folha/haiku_export.py:70`, `app/chamados/services/passivo_validado.py:116`, `app/chamados/services/passivo_validado.py:127`, `app/folha/export.py:594`) |
| `ExportacaoDominio.registrar → ja_existia` | Testa existência de **qualquer geração anterior** da tripla. Marca reexportação; não exige entrega vigente nem pagamento. (`app/folha/models.py:174`, `app/folha/models.py:178`, `app/folha/models.py:186`) |

Os chamadores das candidatas ficam assim:

- **K6 direto:** marcos, preview, comparador de TXT e K9; ranking HE; Pautas pago/devido e folga exportada; Esmeril; foto E4b; vigia do fio; tarefa do dia; competências a lavrar; contagem de dias abonados; backfill de decisões; lavratura de status. (`app/ponto/services/fechamento.py:1550`, `app/folha/views.py:354`, `app/folha/export.py:621`, `app/folha/export.py:658`, `app/folha/services/ranking_he.py:89`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:93`, `app/ponto/services/esmeril_espelho.py:58`, `app/chamados/services/foto_e4b.py:245`, `app/chamados/services/vigia_fio.py:846`, `app/chamados/services/tarefa_do_dia.py:442`, `app/ponto/services/he_pendente_lavrado.py:223`, `app/ponto/services/dia_pago.py:717`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:471`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:498`, `app/ponto/management/commands/lavrar_status_competencia.py:43`)
- **Marcos:** projeções `empresas_exportadas_no_escopo` e `marco_da_competencia`, pré-fechamento e reabertura de órfãs enterradas. (`app/ponto/services/fechamento.py:1528`, `app/ponto/services/fechamento.py:1568`, `app/folha/management/commands/pre_fechamento.py:74`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:95`)
- **K4/K7:** marcos, chamado, painel/tela/reabertura de fechamento, juiz das perguntas e serviço de encerramento por competência trancada. (`app/ponto/services/fechamento.py:1551`, `app/chamados/models.py:1091`, `app/ponto/views.py:1030`, `app/ponto/views.py:1129`, `app/ponto/views.py:1543`, `app/chamados/juizes.py:1066`, `app/chamados/services/competencia_trancada.py:95`)
- **F6:** registro de batida, correção de tipo e lavratura de foto de folgas. (`app/ponto/registro_batida.py:135`, `app/ponto/services/flip_batida.py:22`, `app/escala/management/commands/lavrar_foto_folgas.py:95`)
- **K9 e suas janelas:** gerador de células, apply específico, regeneração/edição/reversão de células, correção retroativa de escala, vigência impossível, tripwire e auditor de invariantes. (`app/ponto/management/commands/gerar_celulas.py:182`, `app/ponto/management/commands/gerar_celulas.py:212`, `app/ponto/management/commands/aplicar_col900.py:64`, `app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:607`, `app/ponto/portas/celula.py:821`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:82`, `app/escala/services/vigencia_impossivel.py:42`, `app/ponto/management/commands/tripwire_celulas.py:319`, `app/chamados/management/commands/auditar_invariantes_chamados.py:372`)
- **C20:** C6, consulta em lote, respostas sem veredito, reconciliador, tarefa do dia e emissão de disputa. O juiz simples de dia trancado é chamado por `furos_sem_cobranca`; o método do chamado também é lido por `cluster_espurio`. (`app/chamados/juizes.py:1405`, `app/chamados/juizes.py:1149`, `app/chamados/models.py:1902`, `app/chamados/reconciliador.py:668`, `app/chamados/reconciliador.py:748`, `app/chamados/services/tarefa_do_dia.py:422`, `app/chamados/services/disputa_emissao.py:1970`, `app/ponto/services/furos_sem_cobranca.py:85`, `app/ponto/services/cluster_espurio.py:254`)
- **Entrega por dia:** modelos, reconciliador, fio, foto E4b, emissão de disputa, fábrica da célula, backfill E4b e rederivação H06. O passivo validado chama sua própria `_esta_exportada`. (`app/chamados/models.py:364`, `app/chamados/reconciliador.py:745`, `app/chamados/services/fio.py:777`, `app/chamados/services/fio.py:1317`, `app/chamados/services/foto_e4b.py:238`, `app/chamados/services/disputa_emissao.py:1837`, `app/chamados/services/fabrica_da_celula.py:231`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:173`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:115`, `app/chamados/services/passivo_validado.py:358`)

**As equivalências são limitadas:** K6, marco `exportada`, empresas exportadas e entrega por dia compartilham a pergunta de TXT; K4/K7 compartilham tranca. F6 consulta status individual; C20 acrescenta toque administrativo; K9 acrescenta documento individual; `paga` é existência de holerite publicado. Nenhum desses predicados consulta pagamento manual declarado. (`app/folha/export.py:591`, `app/ponto/services/fechamento.py:1528`, `app/ponto/services/fechamento.py:1483`, `app/ponto/services/fechamento.py:1493`, `app/ponto/janelas.py:86`, `app/chamados/juizes.py:1078`, `app/folha/export.py:625`, `app/ponto/services/fechamento.py:1552`)

Também há perguntas temporais: `janela_anterior` responde competência encerrada no calendário; `versao_para` e `sem_adesao_no_horizonte` respondem horizontes de regra. Não consultam pagamento. O comentário do horizonte registra a 10 paga fora, mas o corpo compara competência com `'2026-11'`. (`app/ponto/janelas.py:66`, `app/ponto/services/cartorio.py:258`, `app/ponto/services/cartorio.py:280`, `app/ponto/services/cartorio.py:291`)

**3. Cadastro de competência paga fora do sistema**

Não localizei cadastro específico para declarar **empresa + competência paga externamente**. Inexistência absoluta fora da cópia pesquisada: [nao verificado]. Nos modelos pertinentes encontrados:

| Modelo | O que registra e quem escreve |
|---|---|
| `PeriodoFechado` — mais próximo estruturalmente | Empresa, competência, autor/data da tranca e reabertura; não contém declaração de pagamento. Escrito por `trancar_periodo` e `reabrir_periodo`. A aprovação da tela pode trancar; a reabertura chama a porta e registra auditoria. (`app/ponto/models.py:920`, `app/ponto/services/fechamento.py:1413`, `app/ponto/services/fechamento.py:1445`, `app/ponto/views.py:1369`, `app/ponto/views.py:1546`, `app/ponto/views.py:1548`) |
| `ExportacaoDominio` | Geração do TXT, autor, nome, hash e conteúdo. `registrar` cria o histórico da geração. (`app/folha/models.py:75`, `app/folha/models.py:173`) |
| `FechamentoMensal` | Por colaborador: estados aberto/aprovado/exportado, aprovação e observação; não há estado `pago`. (`app/ponto/models.py:398`, `app/ponto/models.py:471`) |
| `Holerite` / `LoteHolerite` | Documento e publicação do lote; essa publicação alimenta o marco `paga`. O command de processamento carimba `publicado_em`. (`app/holerite/models.py:6`, `app/holerite/models.py:103`, `app/holerite/management/commands/processar_lote_holerite.py:123`, `app/ponto/services/fechamento.py:1552`) |

O modelo próximo, `PeriodoFechado`, não está registrado no admin de ponto mostrado nessa cópia. (`app/ponto/admin.py:1`)

Há uma diferença temporal adicional: a matriz de holerites usa mês civil, enquanto a apuração de ponto usa o corte da empresa. O registro vivo documenta essa distinção. (`app/holerite/matriz.py:51`, `app/ponto/janelas.py:10`, `app/core/juizes.py:615`)

**4. O que impede regravar a 10 e quais escritores passam pela guarda**

**Não há bloqueio da 10 por “competência paga” no recálculo atual.** A guarda contém somente 09; status aprovado também não impede esse escritor: aberto recebe `save()`, demais estados recebem `queryset.update()` dos campos calculados. O bloqueio aprovado→aprovado do model só atua no `save()`. O contrato existente exige que recálculo não consulte tranca. (`app/ponto/services/fechamento.py:37`, `app/ponto/services/fechamento.py:1150`, `app/ponto/services/fechamento.py:1173`, `app/ponto/models.py:492`, `app/ponto/tests/test_contract_juiz_fechamento.py:117`)

Isso descreve a proteção por competência; a execução concreta para Juliani, seu universo elegível e eventuais falhas de cálculo são [nao verificado]. O serviço filtra o universo e captura falhas por colaborador. (`app/ponto/services/fechamento.py:554`, `app/ponto/services/fechamento.py:1316`)

| Escritor | Escrita e passagem pela guarda |
|---|---|
| `recalcular_fechamento_mes` | Cria/salva/atualiza FM e chama lavratura de DP `motor` e `oraculo`. **Passa:** colaboradores barrados saem antes do laço. (`app/ponto/services/fechamento.py:564`, `app/ponto/services/fechamento.py:903`, `app/ponto/services/fechamento.py:1157`, `app/ponto/services/fechamento.py:1173`, `app/ponto/services/fechamento.py:1214`, `app/ponto/services/fechamento.py:1290`) |
| `dia_pago.lavrar` | Apaga e recria as linhas do colaborador/competência/versão. **Sem guarda própria**; suas chamadas funcionais estão dentro do recálculo guardado. (`app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1214`, `app/ponto/services/fechamento.py:1290`) |
| `retratar_exportada` | Atualiza/cria campos de grade, ajuste DSR/banco e carimbos. **Tem guarda própria.** (`app/ponto/services/dia_pago.py:459`, `app/ponto/services/dia_pago.py:495`, `app/ponto/services/dia_pago.py:518`, `app/ponto/services/dia_pago.py:552`, `app/ponto/services/dia_pago.py:559`) |
| `apurar_furo_da_exportada` | Atualiza apenas `furo_apurado` e `lavrado_em` nas linhas ainda não apuradas. **Não consulta a guarda.** (`app/ponto/services/dia_pago.py:664`, `app/ponto/services/dia_pago.py:685`, `app/ponto/services/dia_pago.py:687`) |
| `aprovar_lote` / `aprovar` | Status e carimbos de aprovação, eventualmente observação. **Não consultam a guarda.** (`app/ponto/services/fechamento.py:1336`, `app/ponto/services/fechamento.py:1348`) |
| `reabrir_lote` / `reabrir` | Status aberto e limpeza da aprovação. **Não consultam a guarda.** (`app/ponto/services/fechamento.py:1357`, `app/ponto/services/fechamento.py:1362`) |
| `restaurar_fechamento` | Atualiza campos fornecidos e invalida `derivado_em` por padrão; exige motivo. **Não consulta a guarda.** (`app/ponto/services/fecham

[truncado em 20000 de 29035 caracteres]

## 15/11 09:45 · sessao 0dbb05660749 · - · - · origem=Ronald (tmux)

Censo das perguntas 2 e 3, somente código.

| Candidata | Predicado exato, universo e fonte | Quem chama |
|---|---|---|
| `competencia_entregue` (_K6) | Por `(empresa, mes, ano)`: seleciona o `ExportacaoDominio` mais recente, desempate `(gerado_em,pk)`, e exige `invalidada_em is None`; retorna bool ou o registro. É entrega por TXT, não pagamento externo. `app/folha/export.py:559,583-597`; `app/folha/models.py:203-225`. | `folha/views.py:354`; `folha/export.py:621,658`; `folha/services/ranking_he.py:89`; `ponto/services/fechamento.py:1550`; `pautas/management/commands/pauta_pago_x_devido_exportada.py:124`; `pauta_folga_nao_definida_exportada.py:93`; `ponto/services/esmeril_espelho.py:58`; `chamados/services/foto_e4b.py:245`; `vigia_fio.py:846,848`; `tarefa_do_dia.py:442`; `ponto/services/he_pendente_lavrado.py:223`; `dia_pago.py:717`; `chamados/management/commands/backfill_decisoes_pergunta.py:471,498`; `ponto/management/commands/lavrar_status_competencia.py:43`. Todos caminhos com prefixo `app/`. |
| `empresas_exportadas_no_escopo` | Projeção do mesmo K6: empresa indicada; senão empresas dos colaboradores indicados; senão empresas ativas. Retorna IDs cujo `marcos_da_competencia()['exportada']` é verdadeiro; `[]` de colaboradores é vazio. `app/ponto/services/fechamento.py:1509-1528`. | O próprio recalculador/carimbo L-113 `app/ponto/services/fechamento.py:539`; demais consumidores ficam com o outro explorador. |
| `marcos_da_competencia` | Por empresa+competência. `aprovada`: existe FM de colaborador da empresa com status aprovado/exportado. `exportada`: K6. `trancada`: K4. `paga`: existe `Holerite` na competência com **lote publicado**, atribuído pela `empresa_folha` ou, se nula, cadastro atual do colaborador. Um único holerite satisfaz o `exists()`, não exige todos pagos. `app/ponto/services/fechamento.py:1531-1557`. | Wrapper exportadas `:1528`, `marco_da_competencia :1568`; `app/folha/management/commands/pre_fechamento.py:74`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:95`. |
| `marco_da_competencia` | Retorna o marco verdadeiro mais adiantado na ordem aprovada/exportada/trancada/paga; caso contrário aberta. Não é outro predicado de pagamento. `app/ponto/services/fechamento.py:1474,1555-1557,1566-1568`. | Leitores de tela/placar: conferir censo do agente de consumidores. |
| `competencia_trancada` (_K4) / `periodo_trancado` (_K7) | Mesmíssimo filtro `PeriodoFechado(empresa_id,mes,ano,reaberto_em IS NULL)`. K4 retorna bool; K7 retorna linha/None. `ativo` não decide. Sem empresa, falso/None. `app/ponto/services/fechamento.py:1477-1493`. | K4: marcos `:1551`, `app/chamados/models.py:1091`. K7: `app/ponto/views.py:1030,1129,1543`; `app/chamados/juizes.py:1066`; `app/chamados/services/competencia_trancada.py:95`. |
| `ponto.janelas.competencia_fechada` (_F6, “mês fechado”) | Por colaborador+dia: timestamp convertido a data local; `janela_atual` com corte da empresa; procura FM daquela competência; aprovado/exportado devolve mensagem de recusa, demais devolvem `''`. Não lê TXT, tranca ou pagamento. `app/ponto/janelas.py:75-89`. | `app/ponto/registro_batida.py:135`; `app/ponto/services/flip_batida.py:22`; `app/escala/management/commands/lavrar_foto_folgas.py:95`. |
| `competencia_fechada_para_celula` (_K9) | K6 **OU** holerite do próprio colaborador na competência com lote publicado; sem colaborador só TXT conta. `fontes=True` informa exportação/holerite. `app/folha/export.py:600-631`. | `app/ponto/management/commands/gerar_celulas.py:182,212`; `app/ponto/management/commands/aplicar_col900.py:64`. |
| `_competencias_exportadas` / `fonte_da_lavra` / `autoridade_da_lavra` | Enumera pares de ED e holerites publicados, pergunta ao K9, converte à janela da empresa. As outras funções apresentam a fonte e a janela que intercepta o recorte. Não julgam pagamento. `app/ponto/management/commands/gerar_celulas.py:135-189,192-229`. | `app/ponto/portas/celula.py:221,607,613,821`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:82`; `app/escala/services/vigencia_impossivel.py:42`; `app/chamados/management/commands/auditar_invariantes_chamados.py:372,396,422,1549`; `app/ponto/management/commands/tripwire_celulas.py:319`. |
| `dia_em_competencia_trancada` | Por empresa+dia: enumera `PeriodoFechado` vivos e verifica se dia pertence à `janela_fechamento`; sem empresa/dia falso, tratamento de exceção fail-open. `app/chamados/juizes.py:2041-2059`. | `app/ponto/services/furos_sem_cobranca.py:85`. O cartório registra a retirada desse leitor em `app/ponto/services/cartorio.py:1713`. |
| `dia_da_pergunta_trancado` / `dias_trancados` (_C20) | Por colaborador+dia de turno: tranca viva da competência **e nenhum toque administrativo posterior nesse colab/dia**. Consulta `PeriodoFechado` via K7 e trilha `LogAuditoria` de cobrar/resolver/desarquivar; conversa não tem competência de fato. `app/chamados/juizes.py:974-992,1004-1008,1062-1093`. | C6 `app/chamados/juizes.py:1405`; lote `:1149`; `app/chamados/models.py:1902`; reconciliador `:668,748`; `services/tarefa_do_dia.py:422`; `services/disputa_emissao.py:1970`. |
| `ChamadoColaborador._dia_em_competencia_trancada` | Excetua módulos fora da tranca, obtém competência do chamado e pergunta K4. É escopo de renascimento de chamado. `app/chamados/models.py:1080-1091`. | `app/chamados/models.py:1056`; `app/ponto/services/cluster_espurio.py:254`. |
| `competencia_do_dia_entregue` | Composição data→`janela_atual`→K6; `None` sem dia, memo por empresa+competência. Mesma entrega TXT, com entrada por data. `app/chamados/services/vigia_fio.py:827-849`. | `app/chamados/models.py:364`; `reconciliador.py:745`; `services/fio.py:777,1317`; `foto_e4b.py:238`; `disputa_emissao.py:1837`; `vigia_fio.py:821`; `fabrica_da_celula.py:231`; `management/commands/backfill_fio_morto_e4b.py:173,255,303`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:115`. |
| `autorizacao_he_periodo._exportada` | Pergunta se empresa do colaborador pertence ao resultado de `empresas_exportadas_no_escopo`. Mesma entrega TXT. `app/ponto/services/autorizacao_he_periodo.py:113-118`. | `app/ponto/services/autorizacao_he_periodo.py:288`. |
| `tripwire_celulas._exportada` | Verifica data dentro das janelas retornadas por `_competencias_exportadas(empresa)`; sem colaborador, portanto só a metade TXT de K9. `app/ponto/management/commands/tripwire_celulas.py:318-322`. | Mesmo command `:346`. |
| `ExportacaoDominio.e_vigente` / `anotar_vigencia` / `so_vigentes` | “Qual registro é mais recente?” por empresa+competência. Não excluem `invalidada_em`. `app/folha/models.py:120-141,192-225`. | K6 `app/folha/export.py:591-594`; memória `app/folha/haiku_export.py:67-75`; listas/histórico e command de regeração são outros consumidores. |
| `exportacoes_da_competencia` / `passivo_validado._esta_exportada` | O primeiro devolve mais recente por `e_vigente`; o segundo testa `'vigente' is not None`. **Não são estritamente equivalentes a K6**: um último TXT invalidado sem substituto ainda aparece aqui, enquanto K6 responde falso. `app/folha/haiku_export.py:59-75`; `app/chamados/services/passivo_validado.py:116-128`; comparação K6 `app/folha/export.py:594-597`. | `_esta_exportada` chamado em `app/chamados/services/passivo_validado.py:358`. |

Registro vivo confirma F6 em `app/core/juizes.py:309` e K4/K6/K7/K9 em `app/core/juizes.py:326-347`; o próprio registro distingue “fechada por colaborador/status” de “trancada por empresa” em `app/core/juizes.py:76-85`.

Não encontrei modelo/campo específico para declarar “paga fora do sistema”; inexistência absoluta além da cópia pesquisada [nao verificado]. O modelo estruturalmente mais próximo por **empresa+competência com autor/data** é `PeriodoFechado`, que registra **tranca**, com reabertura, e não pagamento: `app/ponto/models.py:920-949`. Escrevem nele `trancar_periodo` e `reabrir_periodo`, em `app/ponto/services/fechamento.py:1413-1465`; a tela aprovar chama trancar quando há empresa e ninguém foi pulado, `app/ponto/views.py:1365-1370`; reabrir chama a porta e registra log em `app/ponto/views.py:1543-1548`. Não está registrado no admin de ponto mostrado em `app/ponto/admin.py:1-48`.

`ExportacaoDominio` guarda nome/hash/conteúdo do TXT, autor e geração, não declaração externa: `app/folha/models.py:75-86`; seu escritor `registrar` cria histórico de gerações em `app/folha/models.py:173-188`. `FechamentoMensal` guarda por colaborador status aberto/aprovado/exportado, aprovação e observação, sem status pago: `app/ponto/models.py:398-410,471-485`. `Holerite`/`LoteHolerite` permitem representar documento publicado, não pagamento manual declarado: `app/holerite/models.py:6-30,103-123`; quem carimba publicação do lote é `app/holerite/management/commands/processar_lote_holerite.py:123-130`.

A própria cópia documenta o desencontro entre pagamento externo e o marco “paga”: julho/agosto pagos à mão em `app/ponto/services/fechamento.py:500-504`, e julho com `marcos['paga']=False` apesar de paga fora em `app/ponto/management/commands/passivo_disputa_retro.py:37-38`. Para outubro há comentário “Comp <= 10 conserva [...] inclusive a 10 paga fora do sistema”, em `app/ponto/services/cartorio.py:282`; isso é corte temporal em código, não cadastro. Estado real dessas competências em produção [nao verificado].

## 15/11 09:45 · sessao cf072147f76f · - · - · origem=Ronald (tmux)

Q1 — leitores diretos encontrados:

| Leitor | Leitura e sentido de `(mes, ano)` | Empresa? |
|---|---|---|
| `recalcular_fechamento_mes` | `GUARDA_COMPETENCIA.get((mes, ano))`; usa a competência da folha, janela por corte da empresa; exclui do universo somente os colaboradores das empresas exportadas. `app/ponto/services/fechamento.py:539`, `:554`, `:560`, `:562`, `:564`; janela em `app/ponto/janelas.py:10`, `:54`. | Sim; consulta `empresas_exportadas_no_escopo`; a competência listada sozinha não barra empresa sem entrega. `app/ponto/services/fechamento.py:561`. |
| `dia_pago.retratar_exportada` | Testa presença da competência na guarda e entrega da empresa do colaborador; retorna `barrado_pela_guarda=True`. `app/ponto/services/dia_pago.py:457`, `:459`, `:467`. | Sim, via `colaborador_ids=[colab.pk]`. `app/ponto/services/dia_pago.py:460`. |
| `barrados_pela_guarda` | Lê `ACAO_BARRADO_GUARDA`; conta **linhas de LogAuditoria** cujo `descricao` começa por `MM/AAAA `. Não calcula janela, não soma colaboradores e não filtra empresa. `app/ponto/services/fechamento.py:47`, `:50`. | Não. `app/ponto/services/fechamento.py:50`. |
| command `recalcular_fechamento` | Importa guarda e contador; depois do apply imprime o acumulado da competência e `['saida']`. Mesmo com `--empresa`, passa só mês/ano ao contador. `app/ponto/management/commands/recalcular_fechamento.py:101`, `:118`, `:123`, `:126`. | O recalculo/foto podem ser por empresa; o contador impresso não é. `app/ponto/management/commands/recalcular_fechamento.py:60`, `:61`, `:126`. |
| `test_bob_b2_abono_feriado_guarda` | Usa empresa com corte 21; cria entrega 09 e fechamentos 09/10. Exige evento 09 preservado e contador 1; evento 10 regravado e contador 0; leitura 09 permitida; retrato 09 barrado. `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:79`, `:169`, `:175`, `:192`, `:200`, `:206`, `:211`. | Sim no cenário montado; contador segue global. `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:175`, `:198`. |
| `test_reverter_regen_recalcula_dinheiro` | Lê contador antes/depois da reversão de célula em 09 exportada; exige célula revertida, dinheiro preservado e contador aumentado. `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:197`, `:203`, `:205`, `:211`, `:212`. | Cenário de um colaborador/empresa; contador não recebe empresa. `app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:205`. |
| `test_justificativa_c9_sem_banco` | Injeta módulo falso com `GUARDA_COMPETENCIA=set()` e ação `'guarda'` para testar retrato sem ata; não caracteriza calendário nem produção. `app/ponto/tests/test_justificativa_c9_sem_banco.py:138`, `:149`, `:153`. | Não exercita entrega real; fornecedor falso retorna conjunto vazio. `app/ponto/tests/test_justificativa_c9_sem_banco.py:151`. |

`ACAO_BARRADO_GUARDA` é usada para escrever o evento do recalculo barrado (`modelo='FechamentoMensal'`) e do retrato barrado (`modelo='DiaPago'`); seu leitor funcional é `barrados_pela_guarda`. `app/ponto/services/fechamento.py:44`, `:50`, `:569`; `app/ponto/services/dia_pago.py:463`.

O contador mede chamadas/eventos, não quantidade de pessoas: uma chamada barrando vários colaboradores gera uma linha, com quantidade no texto e IDs no JSON; o retrato gera outra linha por chamada/colaborador. `app/ponto/services/fechamento.py:569`, `:570`, `:573`; `app/ponto/services/dia_pago.py:463`.

As menções documentais ao nome estão em `app/docs/RELATO.md:1372`, `:1374`, `:1695`, `:2567`, `:4754`, `:4758`; não são leitores executáveis da variável. As ocorrências de `barrado_pela_guarda` em `eval/juiz.py:23`, `:60`, `:227` pertencem a outro veredito, descrito como motivo `numero_fora_dos_dados`. `eval/juiz.py:60`.

Q4 — 10/2026 e inventário dos escritores:

**A guarda desta cópia não impede regravação da 10:** só contém `(9, 2026)`; a aplicação exige simultaneamente chave guardada e empresa exportada. `app/ponto/services/fechamento.py:36`, `:37`, `:560`, `:561`. O teste existente exige explicitamente que evento na 10 regrave. `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:200`.

`status='aprovado'` não impede o escritor de recalculo: aberto recebe `save()`; demais estados recebem `queryset.update()` dos números calculados, mantendo os carimbos de aprovação. `app/ponto/services/fechamento.py:1150`, `:1157`, `:1159`, `:1173`. A proteção do model é apenas no `save()` de linha originalmente aprovada que continua aprovada; não alcança o `update()` do recalculo. `app/ponto/models.py:492`, `:498`, `:503`; `app/ponto/services/fechamento.py:1173`. O contrato AST exige que `recalcular_fechamento_mes` não consulte tranca. `app/ponto/tests/test_contract_juiz_fechamento.py:117`, `:123`, `:130`.

| Escritor funcional | O que grava | Passa na guarda? |
|---|---|---|
| `recalcular_fechamento_mes` | Cria FM, salva/atualiza campos calculados; chama lavratura DP `motor`, e DP `oraculo` desde 10/2026. `app/ponto/services/fechamento.py:903`, `:1157`, `:1173`, `:1214`, `:1278`, `:1290`. | Sim, exclui colaboradores antes do laço. `app/ponto/services/fechamento.py:560`, `:564`. |
| `dia_pago.lavrar` | Apaga e recria todas as linhas do colaborador/competência/versão. `app/ponto/services/dia_pago.py:135`, `:359`, `:360`. | Sem guarda própria; as duas chamadas funcionais ficam no recalculo guardado. `app/ponto/services/fechamento.py:1214`, `:1290`. |
| `dia_pago.retratar_exportada` | Atualiza/cria três campos de grade, ajuste DSR/banco e carimbos de retrato. `app/ponto/services/dia_pago.py:486`, `:495`, `:518`, `:552`, `:556`, `:559`. | Sim, guarda própria antes de ler FM. `app/ponto/services/dia_pago.py:459`, `:469`. |
| `dia_pago.apurar_furo_da_exportada` | Somente `furo_apurado` e `lavrado_em` em linhas já existentes com furo NULL. `app/ponto/services/dia_pago.py:659`, `:664`, `:685`, `:687`. | Não consulta guarda; colhe motor em leitura e escreve por porta própria. `app/ponto/management/commands/apurar_furo_da_exportada.py:111`, `:131`. |
| `aprovar_lote` / `aprovar` | Status, usuário, data e eventualmente observação. `app/ponto/services/fechamento.py:1336`, `:1342`, `:1345`, `:1348`, `:1353`. | Não. `app/ponto/services/fechamento.py:1336`. |
| `reabrir_lote` / `reabrir` | Status aberto e limpeza da aprovação. `app/ponto/services/fechamento.py:1357`, `:1359`, `:1362`, `:1367`. | Não. `app/ponto/services/fechamento.py:1357`. |
| `restaurar_fechamento` | Atualiza campos fornecidos, exige motivo, invalida `derivado_em` por padrão. `app/ponto/services/fechamento.py:1371`, `:1391`, `:1404`, `:1406`. | Não. `app/ponto/services/fechamento.py:1386`. |
| `reabrir_periodo` | Reabre PeriodoFechado e devolve FM aprovado para aberto. `app/ponto/services/fechamento.py:1445`, `:1450`, `:1462`, `:1465`. | Não. `app/ponto/services/fechamento.py:1445`. |
| `invalidar_previsto` | Zera `previsto_em` e atualiza carimbo. `app/ponto/services/fechamento.py:1620`, `:1624`, `:1626`. | Não. `app/ponto/services/fechamento.py:1620`. |
| `invalidar_fechamento` | Zera `derivado_em` e atualiza carimbo nas competências do fato. `app/ponto/services/fechamento.py:1662`, `:1665`, `:1669`. | Não. `app/ponto/services/fechamento.py:1629`. |
| `apagar_em_ensaio` | Delete de FM, exigindo banco de ensaio. `app/ponto/services/fechamento.py:1777`, `:1779`, `:1781`. | Não; tem guarda de banco de ensaio. `app/ponto/services/fechamento.py:1780`. |
| `FechamentoMensal.save` | Escrita ORM, com proteção aprovado→aprovado. `app/ponto/models.py:492`, `:498`, `:503`. | Não consulta guarda de competência. `app/ponto/models.py:492`. |
| `bin/restore_relavratura_10_2026.py` | Salva FM por `save(update_fields)`, apaga FM excedentes, apaga/recria DP motor. `bin/restore_relavratura_10_2026.py:74`, `:81`, `:83`, `:85`, `:86`. | Não passa pelo serviço guardado. `bin/restore_relavratura_10_2026.py:69`. |
| `bin/restore_o263_comp10.py` | Mesmos atos, limitados ao alvo. `bin/restore_o263_comp10.py:43`, `:50`, `:52`, `:54`, `:56`. | Não passa pelo serviço guardado. `bin/restore_o263_comp10.py:40`. |
| `bin/restore_badge_linha_09_10.py` | Apaga DP das competências fotografadas e recria snapshot. `bin/restore_badge_linha_09_10.py:60`, `:61`, `:63`. | Não passa pelo serviço guardado. `bin/restore_badge_linha_09_10.py:59`. |

Chamadores de escrita que chegam ao recalculo, portanto à guarda: botão `app/ponto/views.py:1255`; evento `app/ponto/services/fechamento.py:1823`; command normal `app/ponto/management/commands/recalcular_fechamento.py:118`; REGEN `app/ponto/portas/regen_exportada.py:184`; `app/folha/management/commands/simular_folha.py:123`; commands `aplicar_09_corte_b.py:212`, `plano_b_no_dinheiro.py:110`, `folga_que_sumiu.py:70`, `desvio_o68b.py:94`, `celula_veredito_velho.py:149`, `diff_reclassificar_partido.py:161` e `:206`, todos sob `app/ponto/management/commands/`. Também chegam os scripts `bin/aplicar_o263_comp10.py:51`, `bin/relavrar_o118_col221.py:53`, `bin/relavrar_o119_col221.py:52`, `bin/relavrar_o120_col221_09.py:74`.

`lavrar_dias_pagos` roteia empresa não exportada para `somente_leitura=True, lavrar_dias_pagos=True` e exportada para `retratar_exportada`; ambos caminhos passam por guarda de escrita. `app/ponto/management/commands/lavrar_dias_pagos.py:104`, `:106`, `:123`. O modo leitura puro não consulta exportadas para bloquear e não persiste FM/DP. `app/ponto/services/fechamento.py:539`, `:540`, `:1141`, `:1204`.

**REGEN não contorna a guarda 09 nesta cópia:** chama o recalculo e depois o retrato, e as duas funções guardam 09 exportada; não há parâmetro de bypass nessas chamadas. `app/ponto/portas/regen_exportada.py:184`, `:193`; `app/ponto/services/fechamento.py:560`; `app/ponto/services/dia_pago.py:459`.

`restaurar_fechamento` é chamado diretamente pelos commands de reversão/aplicação sem passar pela guarda do recalculo: `app/ponto/management/commands/aplicar_09_corte_b.py:220`, `:266`; `app/ponto/management/commands/aplicar_09_he_intra.py:149`; `app/ponto/management/commands/desvio_o68b.py:197`; `app/ponto/management/commands/diff_reclassificar_partido.py:224`.

A data da competência não é mês civil: com corte 21, 25/09 pertence a 10; há comentário explícito no restore que conserva `mes/ano` da linha justamente por esse motivo. `app/ponto/janelas.py:11`, `:13`; `bin/restore_badge_linha_09_10.py:44`. O corte real cadastrado de Juliani em produção permanece [nao verificado].

## 15/11 09:45 · sessao c053ba9ea9dc · - · - · origem=Ronald (tmux)

Censo da pergunta 5, somente código; sem medição de produção.

**A distinção que os consumidores fazem hoje:** `empresas_exportadas_no_escopo` devolve empresas cujo marco `exportada` é verdadeiro; este marco chama `competencia_entregue`. O predicado de entrega é existir TXT vigente da tripla empresa/mês/ano, com vigente não invalidado. O marco `paga` consulta holerite em lote publicado; são fontes e perguntas diferentes. `app/ponto/services/fechamento.py:1528`, `app/ponto/services/fechamento.py:1550`, `app/ponto/services/fechamento.py:1552`, `app/folha/export.py:591`.

**Consumidores diretos de `empresas_exportadas_no_escopo`:**

| Consumidor | Uso da resposta |
|---|---|
| Recalculo | Consulta apenas quando pode escrever `FechamentoMensal` ou `DiaPago`; carimba L-113 quando exportada; combina com `GUARDA_COMPETENCIA` para excluir colaboradores das empresas exportadas. `app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:541`, `app/ponto/services/fechamento.py:560` |
| `lavrar_dias_pagos` | Não exportada: motor em leitura com lavratura; exportada: `retratar_exportada` usando gravado. `app/ponto/management/commands/lavrar_dias_pagos.py:104`, `app/ponto/management/commands/lavrar_dias_pagos.py:106`, `app/ponto/management/commands/lavrar_dias_pagos.py:123` |
| `lavrar_do_motor` | Combina presença da competência na guarda com empresa exportada para recusar lavratura. `app/ponto/services/dia_pago.py:458` |
| Porta do export | Separa divergências de dinheiro congelado, grade e linha versus badge; exclui exportada do contador de lavratura sem furo apurado. Na falha da consulta assume aberta. `app/folha/porta_export.py:343`, `app/folha/porta_export.py:346`, `app/folha/porta_export.py:424`, `app/folha/porta_export.py:430`, `app/folha/porta_export.py:452`, `app/folha/porta_export.py:461` |
| Cobrança da folha | `muda_a_folha`: dia sem data retorna falso; competência pelo corte da empresa; exportada retorna falso e conta motivo. A tela de cobrar filtra com essa resposta. `app/chamados/services/cobranca_folha.py:54`, `app/chamados/services/cobranca_folha.py:63`, `app/chamados/services/cobranca_folha.py:68`, `app/chamados/views_cobrar.py:95` |
| Autorização HE por período | `_exportada` pergunta pela empresa; ato com autorização efetiva e exportada produz Pauta DP. `app/ponto/services/autorizacao_he_periodo.py:113`, `app/ponto/services/autorizacao_he_periodo.py:288`, `app/ponto/services/autorizacao_he_periodo.py:299` |

**Telas e Pauta DP que leem entregue/exportada por outros caminhos:**

- Preview mostra “Já exportado em … por …” usando o registro devolvido por `competencia_entregue(..., registro=True)`. A emissão do TXT também mostra aviso de reexportação pelo `ja_existia` retornado por `ExportacaoDominio.registrar`; é outro caminho, histórico de geração. `app/folha/views.py:354`, `app/folha/views.py:361`, `app/folha/views.py:297`, `app/folha/views.py:306`.
- Ranking HE: sem linhas=`nao_apurado`; com linhas e todas as empresas entregues=`lavrado`; demais=`parcial`. `app/folha/services/ranking_he.py:85`.
- Pauta pago versus devido aceita somente empresa cuja competência o K6 considera entregue; “pago” vem do `FechamentoMensal` gravado e “devido” do recalculo em leitura; escreve cabeçalho e filhas com os dois valores. Competência sem TXT vigente é classificada `nao_entregue`. `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:175`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`.
- Pauta folga não definida exportada percorre empresas ativas, exige entregue e usa janela da empresa; a não entregue permanece atribuída à lista do admin quando coberta por `competencias_a_lavrar`. `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:92`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:96`, `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:104`.
- Limbo (`trabalha=None`) com motivos em competência entregue vai para Pauta DP em lugar de perguntar ao colaborador; dia de trabalho permanece em outro ramo. `app/chamados/services/fabrica_da_celula.py:214`, `app/chamados/services/fabrica_da_celula.py:223`, `app/chamados/services/fabrica_da_celula.py:268`.
- Órfãs enterradas: competência exportada entra no balde Pauta DP e sai antes da reabertura. `app/chamados/management/commands/reabrir_orfas_enterradas.py:95`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:99`.
- Pré-fechamento relata marco e violações da competência anterior; não cria trava própria por exportada. `app/folha/management/commands/pre_fechamento.py:69`.

**REGEN-EM-EXPORTADA e carimbo L-113:**

- A porta exige colaborador individual, staff, motivo de dez caracteres e intervalo válido; regenera células com `apesar_da_lavra=motivo`; depois chama o recalculo comum e `retratar_exportada`. `app/ponto/portas/regen_exportada.py:109`, `app/ponto/portas/regen_exportada.py:111`, `app/ponto/portas/regen_exportada.py:114`, `app/ponto/portas/regen_exportada.py:179`, `app/ponto/portas/regen_exportada.py:184`, `app/ponto/portas/regen_exportada.py:193`.
- Essa chamada de recalculo **não tem bypass da guarda de fechamento**: passa apenas competência, colaborador e motivo; portanto encontra o mesmo filtro de `GUARDA_COMPETENCIA`. O comentário da porta dizendo “não há mais guarda para destravar” diverge desse código atual. `app/ponto/portas/regen_exportada.py:181`, `app/ponto/portas/regen_exportada.py:184`, `app/ponto/services/fechamento.py:560`.
- A própria porta registra hashes, campos, réguas antes/depois e retorna o resultado; o escritor de Pauta é o chamador `passivo_validado`, que chama a porta quando o par é exportado e teve plantio, e escreve Pauta quando mudou ou Pauta de saída do DIFF quando não mudou. `app/ponto/portas/regen_exportada.py:204`, `app/ponto/portas/regen_exportada.py:211`, `app/ponto/portas/regen_exportada.py:221`, `app/chamados/services/passivo_validado.py:435`, `app/chamados/services/passivo_validado.py:444`, `app/chamados/services/passivo_validado.py:448`, `app/chamados/services/passivo_validado.py:456`.
- O carimbo L-113 usa `_exportadas`, não o marco `paga`, e escreve aviso antes do filtro da guarda; uma chamada barrada pode ter também o aviso de competência exportada. Leitura pura não carimba; leitura com lavratura carimba. `app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:541`, `app/ponto/services/fechamento.py:560`.

**Demais consumidores de entrega:**

- `dias_com_abono` conserva a contagem das linhas positivas lavradas quando entregue; fora disso consulta ata e períodos justificados. `app/ponto/services/dia_pago.py:715`, `app/ponto/services/dia_pago.py:717`, `app/ponto/services/dia_pago.py:720`.
- `competencias_a_lavrar` mantém atual e anterior enquanto anterior não entregue. A janela da lista do Esmeril recua até primeira competência entregue ou até acabar o histórico de células. `app/ponto/services/he_pendente_lavrado.py:220`, `app/ponto/services/he_pendente_lavrado.py:223`, `app/ponto/services/esmeril_espelho.py:55`.
- `competencia_do_dia_entregue` compõe janela da empresa com K6; leitores: classificação de carimbos por dono, reconciliador, modelos, fio, emissão de disputa, fábrica da célula e backfill E4b. `app/chamados/services/vigia_fio.py:827`, `app/chamados/services/vigia_fio.py:821`, `app/chamados/reconciliador.py:745`, `app/chamados/models.py:364`, `app/chamados/services/fio.py:777`, `app/chamados/services/disputa_emissao.py:1837`, `app/chamados/services/fabrica_da_celula.py:231`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:173`.
- `veto_de_competencia` veta acervo da E1 quando algum dia é entregue; é usado pelo backfill de decisões e migração de data do turno. `app/chamados/services/fio.py:1307`.
- Tarefa do dia carrega entrega em lote, remove essas candidatas da consulta C6 e transmite o atributo `exportada` para a tarefa. `app/chamados/services/tarefa_do_dia.py:430`, `app/chamados/services/tarefa_do_dia.py:483`, `app/chamados/services/tarefa_do_dia.py:526`.
- Rederivação de minutos H06 recusa dia em competência entregue; lavratura de status apenas informa entrega, sem usá-la como guarda. `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:115`, `app/ponto/management/commands/lavrar_status_competencia.py:43`.
- Vigia do TXT e regerador comparam hash do TXT vigente com o gravado atual por `txt_vigente_x_gravado`; o vigia distingue vigente revogado sem substituta como incomparável e alarma movimentação de valores. `app/folha/export.py:658`, `app/folha/services/vigia_txt.py:220`, `app/folha/services/vigia_txt.py:244`, `app/folha/services/vigia_txt.py:265`, `app/folha/management/commands/regerar_txt_dominio.py:106`.

**Guarda de células é outra pergunta:** `competencia_fechada_para_celula` compõe entrega por empresa com holerite publicado do próprio colaborador; `_competencias_exportadas` enumera candidatos e consulta esse juiz. Regenerar, editar e reverter célula usam esse conjunto, com exceção textual `apesar_da_lavra`. Seus consumidores indiretos incluem corrigir escala retroativa, vigência impossível, tripwire e auditoria de invariantes. `app/folha/export.py:600`, `app/folha/export.py:621`, `app/ponto/management/commands/gerar_celulas.py:170`, `app/ponto/management/commands/gerar_celulas.py:182`, `app/ponto/portas/celula.py:218`, `app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:820`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:82`, `app/escala/services/vigencia_impossivel.py:42`, `app/ponto/management/commands/tripwire_celulas.py:319`, `app/chamados/management/commands/auditar_invariantes_chamados.py:372`.

**Testes que explicitam contratos desses consumidores:** K6 entrega e integração em marcos/preview/hash/carimbo/gerador; fechamento de célula por TXT ou holerite; contrato de competência lavrada; Pauta pago/devido e folga exportada; autorização HE; cobrança folha; janela da lista; REGEN e passivo. `app/folha/tests/test_k6_competencia_entregue.py:61`, `app/folha/tests/test_k6_competencia_entregue.py:96`, `app/folha/tests/test_k6_competencia_entregue.py:112`, `app/folha/tests/test_k6_competencia_entregue.py:147`, `app/folha/tests/test_k6_competencia_entregue.py:165`, `app/folha/tests/test_k6_competencia_entregue.py:232`, `app/folha/tests/test_k6_fechada_para_celula.py:103`, `app/ponto/tests/test_contract_competencia_lavrada.py:63`, `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:407`, `app/pautas/tests/test_f3_beco1_pauta_exportada.py:43`, `app/ponto/tests/test_o214_item2_autorizar_periodo.py:405`, `app/chamados/tests/test_cobrar_so_o_que_muda_a_folha.py:157`, `app/ponto/tests/test_janela_da_lista_le_o_k6.py:46`, `app/ponto/tests/test_regen_em_exportada.py:54`.

**Limite factual da mudança hipotética:** trocar somente a pergunta da guarda não troca automaticamente os consumidores de entrega listados; hoje eles chamam seus respectivos predicados diretamente. Se a alteração pretendida também modificar o significado de `empresas_exportadas_no_escopo` ou do K6, os consumidores acima deixam de receber a mesma pergunta histórica. O desenho concreto dessa alteração ainda é [nao verificado]. `app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:560`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124`, `app/folha/views.py:354`.

## 15/11 09:41 · sessao c3740e289d15 · - · - · origem=Ronald (tmux)

As quatro linhas registradas de `chamados/juizes.py` coincidem com a cópia; nenhum deslocamento observado:

| Sítio | Registro | Fonte atual |
|---|---|---|
| `_dia_da_batida` | `core/tests/test_medida_turno_do_par.py:258` → `[53]` | `chamados/juizes.py:53` |
| `_dia_do_contexto_fraco` | `core/tests/test_medida_turno_do_par.py:259` → `[169]` | `chamados/juizes.py:169` |
| `_plantio_na_hora_do_slot` | `core/tests/test_medida_turno_do_par.py:260` → `[274]` | `chamados/juizes.py:274` |
| `dias_do_fato` | `core/tests/test_medida_turno_do_par.py:261` → `[203]` | `chamados/juizes.py:203` |

Para `padrao=`, o termo literal é `single source of truth / um juiz por pergunta` (`docs/PLANO-PADROES.md:46–50`).

Censo textual de leitores de produção, conferido por leitura:

- `_portas_da_tarefa`: `chamados/services/tarefa_do_dia.py:591`, `:616`.
- `porta_da_tarefa`: `escala/services/cadastro_realidade.py:472`; `chamados/services/vigia_fio.py:354`, `:589`; `chamados/services/furo_sem_canal.py:97`; `folha/fora_da_folha.py:72`, `:261`; alias `candidata` em `chamados/management/commands/diff_e3_sombra.py:43`, usado em `:146`.
- `porta_alcancavel_do_dia`: único chamador de produção restante é o lado “antes” de `chamados/management/commands/diff_e3_sombra.py:144`.

Censo real e suite: [nao verificado]. Nenhum arquivo editado. [nao verificado]

## 15/11 08:04 · sessao 74371de003cb · DISPUTA-ZUMBI-1110 · censo · origem=Code

**O HEAD fornecido mantém carimbadores sem consulta geral de fechamento após a escrita. Porém, `lavrar_sem_turno_aberto` consulta C7 e chama `fechar()` depois do carimbo; a descrição “sem leitor depois” não corresponde a esse fluxo atual.** (`app/chamados/reconciliador.py:406-414`; `app/chamados/services/fio.py:1398-1402`; `app/chamados/services/disputa_emissao.py:1681-1689`.)

Os números e as causas individuais das disputas dos anexos não foram reconferidos em produção; faltam linhas individualizadas, instantes dos carimbos e resultados das tentativas de fechamento. **[não verificado]**

Para compactar as referências: caminhos sem prefixo abaixo são relativos a **`app/chamados/`**; caminhos iniciados por `app/` ou `bin/` são relativos à raiz do archive. As referências são desta cópia; correspondência com o hash Git informado e histórico contra `eac3f25dd`: **[não verificado]**.

**1. Carimbadores e fechamento no mesmo fluxo**

Na tabela, **B** significa `PerguntaDisputa.save → post_save → on_commit → fechar_beco_da_disputa`. Esse caminho somente consulta C7 se a disputa estiver aberta, o pai for terminal e não houver pergunta sem `respondida_em` cujo destino não seja terminal; também há proteção contra reentrada. Portanto, **B não é uma consulta universal de C7 após qualquer carimbo**. (`signals.py:229-240`; `services/becos.py:25-33,66-80`.)

| Carimbador / sítio de escrita | Campos ou ato | Consulta C7 / chama `fechar()`? Condição adicional |
|---|---|---|
| `carimbar` | Materialização, autor, via; opcionalmente resposta/validação | A porta consulta **C6**, desfaz em memória se a pergunta continuar viva e salva somente se aceita. Não consulta C7 diretamente; o fechamento depende do chamador e de B. (`models.py:3186-3239`.) |
| `carimbar_identidade` | Materialização, autor, via; motivo opcional | Grava identidade sem consulta C6/C7 própria; fechamento depende do chamador e de B. Recusa devolver linha fora do fato por outra via. (`models.py:3251-3274`.) |
| `lavrar_sem_turno_aberto` | Via `sem_turno_aberto` | **Sim:** carimbo aceito → disputa aberta + C7 → `fechar`. Também B. (`reconciliador.py:406-414`.) |
| `retratar_perguntas_absolvidas_por_celula` | Via `celula_absolveu` | **Sim, após o lote:** relê fechamento, exige aberta + C7; captura falhas do fechamento. Também B. (`reconciliador.py:593-617`.) |
| `encerrar_orfas_pela_lampada` → `encerrar_fantasma` | Via `fantasma` | **Sim, após o lote:** relê disputa, exige aberta, nenhuma resposta sem veredito e C7. Também B. (`reconciliador.py:1148-1177`; `models.py:3378-3386`.) |
| `_materializar_ausencia_de_resposta` | Carimbo das irmãs; pode completar resposta/validação | **Sim:** aberta + C7 → `fechar`. Ausência ainda aberta não encerra o destino. Também B. (`services/materializacao.py:555-592`.) |
| `_materializar_uma` | Materialização/via; pode completar resposta/validação | **Sim pela cauda do lote:** `materializar_perguntas_validadas_da_disputa → _fechar_disputa_se_quitada`, aberta + C7. Também B. (`services/materializacao.py:1283-1284,1322-1339,1381-1385`.) |
| Recusa por paridade, dentro da materialização | Identidade `paridade_recusada` | Mesma cauda C7 do materializador; também B. (`services/materializacao.py:930,1381-1385`.) |
| `_carimbar_recusa`, colisão | Identidade `fato_ocupado` | Mesma cauda C7 do materializador; exige ausência de carimbo e recusa `COLISAO`. Também B. (`services/materializacao.py:1099-1106,1303,1381-1385`.) |
| `materializar_pendentes_aprovadas` | Via `juiz_aprovou` | Sem C7 próprio: o signal o chama quando a disputa **já está fechada**. Exige lastro aprovado e pula pergunta em voo. Também B. (`signals.py:155-167`; `services/materializacao.py:1431-1438`.) |
| S143: perguntas ligadas ao chamado resolvido | Via pelo fato; resposta/validação quando cabíveis | **Sim na cauda:** relê disputa sob nova consulta de tranca; aberta + C7. **Pula a disputa se alguma pergunta estiver em voo.** Também B. (`services/disputa_emissao.py:1973-1994,2088-2099`.) |
| S143: perguntas sem credor vivo | Via pelo fato; pode completar `respondida_em` | Mesma cauda C7; exige pai/destinos fora de `VIVOS`, pergunta fora de voo e C6 morta. Também B. (`services/disputa_emissao.py:2039-2067,2088-2099`.) |
| `responder_pergunta_disputa_core` | Texto + instante; limpa validação anterior; pode desfazer carimbo sem lastro | Sem C7 geral próprio. Escape que vira ausência chama o materializador com C7; demais respostas têm B. (`services/disputa_emissao.py:1467-1485,1503-1512,1538-1541`.) |
| `responder_automaticamente` | Texto + instante | Sem C7 próprio; o chamador da foto valida pelo dono e pode fechar posteriormente. Também B. (`services/disputa_emissao.py:1659-1664`; `app/api/foto_recebida.py:48-51,71-80`.) |
| `carimbar_resposta_pela_trilha` | `respondida_em`, usando instante da nota | **Somente B.** O comando `curar_perguntas_em_limbo --apply` não acrescenta consulta C7 depois. (`services/disputa_emissao.py:1677-1689`; `management/commands/curar_perguntas_em_limbo.py:64-70`.) |
| `_carimbar_validacao` no fluxo normal de `validar_pergunta` | Validação + autor | **Sim pela materialização integral**, depois do tratamento da recusa; também B. (`services/validacao.py:159-166,460,488-494`; `services/materializacao.py:1322-1339`.) |
| Aceite de folga | Validação e identidade `admin_decidiu` | Sem C7 próprio nesse ramo; fecha o fio do dia, que pode reconciliar chamados e acionar S143. Também B. (`services/validacao.py:313-321,345-354`; `app/ponto/services/veredito_celula.py:190-203`.) |
| `devolver_validacao_recusada` | Limpa validação + autor | Sem C7 próprio; pode **reabrir** disputa fechada quando `reabrir=True`. Também B. (`services/validacao.py:224-243`.) |
| `recusar_pergunta` | Devolve questionário para correção | Delega à reabertura em lote; sem fechamento C7 acrescentado nessa porta. (`services/validacao.py:588,628-640`; `services/disputa_emissao.py:1634-1636`.) |
| `relavrar` | Troca via derivada e atualiza cache da lavra | **Somente B.** O comando `relavrar_via_i6 --apply` também não acrescenta C7. (`services/relavra_via.py:118-139`; `management/commands/relavrar_via_i6.py:96-104`.) |
| `retratar_para_canonica → retratar_duplicata` | Identidade `duplicata`; preserva instante existente | **Somente B.** A porta de retratação exige as travas e aplica os vetos antes do carimbo. (`services/fio.py:1373-1402`.) |
| `remover_motivo` | Retratação na sequência + identidade `motivo_retratado` | **Com motivos restantes:** retorna, apenas B. **Sem motivos restantes:** chama `fechar()` diretamente, sem consulta prévia literal a C7. (`models.py:2120-2136`.) |
| `fechar_por_admin` | Identidade `admin_decidiu` | Sem C7 próprio. É usado dentro das exceções de `Disputa.fechar`, no aceite de folga e no fechamento do fio do dia. Também B. (`models.py:1982-1985,3147`; `services/validacao.py:314,354`; `app/ponto/services/veredito_celula.py:190`.) |
| `descarimbar` | Limpa materialização, autor e via | Sem C7 próprio; B somente quando `salvar=True`. (`models.py:3286-3296`.) |
| `descarimbar_em_lote` | `QuerySet.update`: limpa carimbo/via e campos extras | **Sem C7 e sem post_save/B.** Exige colaboradores travados e exclui linhas fora do fato. (`models.py:3335-3365`.) |
| `reabrir_para_correcao` | Limpa texto, resposta, validação, autores, materialização e via | **Sem C7/B:** usa a porta do lote e emite eventos de célula explicitamente. (`services/disputa_emissao.py:1595,1634-1636`.) |
| `ressuscitar_perguntas` | Lote: limpa carimbo, resposta e validação | **Sem C7/B:** emite eventos e reabre disputas fechadas afetadas. (`models.py:1613-1622`.) |
| `desfazer_carimbo_sem_lastro` | Descarimba pergunta que C6 considera viva | Sem C7 próprio; B. Pode reabrir disputa fechada, conforme `reabrir_fechada`. (`services/relavra_via.py:238-261`.) |
| `encerrar_por_fato_no_espelho` | Troca somente via para `fato_materializado` | A porta tem B. Seu comando chamador, com `--apply`, depois exige disputa aberta e `fio_vivo=False`, chama `fechar()` e captura `ValueError`; não consulta C7 literalmente antes. (`models.py:3367-3376`; `management/commands/encerrar_perguntas_com_fato.py:53-55,68,81-93`.) |
| `reabrir_fantasma` | Descarimba somente a via fantasma materializada | Sem C7 próprio; B. O comando pode reabrir disputa e pai. (`models.py:3395-3398`; `management/commands/reabrir_orfas_enterradas.py:157-172`.) |
| `reabrir_por_resposta` | Descarimba em memória as vias sem lastro | Sem C7 próprio; o escritor da resposta salva depois, acionando B. (`models.py:3412-3414`; `services/disputa_emissao.py:1485,1508`.) |
| `reabrir_por_competencia` | Descarimba a antiga via `competencia_trancada` | Sem C7 próprio; B. (`models.py:3419-3422`; `services/competencia_trancada.py:279`.) |
| `_nascer` | Insere pergunta com campos recebidos | Sem C7 próprio; nascimento também passa pelo signal B. Preserva os atos projetados na sequência de decisões. (`models.py:3097-3102`; `signals.py:229-240`.) |

O escritor básico do trio materialização/autor/via é `_escrever_carimbo`; `encerrar_por_fato_no_espelho` e `relavrar` também têm escritas específicas de **via**, e o lote tem a escrita SQL própria já indicada. (`models.py:3180-3184,3363-3375`; `services/relavra_via.py:125-128`.)

`gravar_lavras_em_lote` **continua sem C7/post_save**, mas grava somente `veredito_lavrado`, `julgado_em` e `janela_expira_em`. Não resolve diretamente pelos campos enumerados na pergunta; C6 lê a projeção dos carimbos e consulta o lastro, não esse cache. Essa classificação da triagem precisa ser corrigida. (`veredito_lavrado.py:237-250`; `juizes.py:1399-1400,1476-1485`.)

Além dos carimbos, existem alterações de âncora/dia/célula que afetam os insumos de C6: reuso da pergunta, reapontamento da resolvedora e rechaveamento de data. Esses sítios não acrescentam C7 próprio; seus saves passam por B. (`models.py:3086-3090`; `services/fio.py:959-973,1076-1084`; `models.py:3303-3305`.)

O evento de célula é outro mecanismo: saves dos campos relevantes emitem outbox; a escrita por lote precisa emitir explicitamente. **Emitir evento não equivale a consultar C7 imediatamente nesse ato.** (`signals.py:243-255`; `services/disputa_emissao.py:1609-1611,1634-1635`; `models.py:3314-3331`.)

**2. Escritores do fechamento e reabridores**

Há **um escritor positivo de `DisputaSupervisao.fechada_em` encontrado**: `DisputaSupervisao.fechar`, que grava instante, autor, resolução e motivo juntos. Não há coluna `status/estado` nessa entidade; `aberta` deriva de `fechada_em is None`. (`models.py:1804,1869-1871,1996-2000`.)

Os sítios diretos de chamada da porta são:

| Chamador | Condição anterior à porta |
|---|---|
| `admin_encerra` | Motivo escrito obrigatório; usa exceção nomeada. (`models.py:2011-2014`.) |
| `remover_motivo` | Nenhum motivo restante. (`models.py:2130-2136`.) |
| Desligamento | Disputa aberta e nenhuma resposta sem veredito; exceção `desligamento`. O corpo atual não faz o antigo bulk de fechamento. (`app/colaboradores/services/desligamento.py:157-167`.) |
| Comando de fato no espelho | `--apply`, aberta, sem fio vivo; captura `ValueError`. (`management/commands/encerrar_perguntas_com_fato.py:53-55,81-93`.) |
| Reconciliador, sem turno | Carimbo aceito, aberta e C7. (`reconciliador.py:406-414`.) |
| Reconciliador, absolvição pela célula | Após lote, refresh, aberta e C7. (`reconciliador.py:609-617`.) |
| Reconciliador, fantasma pela lâmpada | Refresh, aberta, sem resposta sem veredito e C7. (`reconciliador.py:1159-1177`.) |
| Foto recebida | Aberta, batida coincide, nenhum motivo restante; valida antes. (`app/api/foto_recebida.py:61-80`.) |
| Beco | Resolve destinos terminais, refresh, aberta e C7; entrada por evento exige o recorte de beco. (`services/becos.py:25-33,42-57,74-78`.) |
| Competência trancada | Aplicação habilitada; medição exclui pai vivo fora do conjunto encerrável; consulta C7 novamente no ato. (`services/competencia_trancada.py:178-183,192,221-230`.) |
| Emissão/atualização da disputa | Após tentar perguntar, C7; **não fecha se houve abstenção por ata ausente**. (`services/disputa_emissao.py:1270-1287`.) |
| S143 | Refresh, nenhuma pergunta em voo, aberta e C7. (`services/disputa_emissao.py:2088-2099`.) |
| Materialização de ausência | Aberta e C7. (`services/materializacao.py:587-592`.) |
| Cauda da materialização granular | Aberta e C7. (`services/materializacao.py:1322-1339`.) |
| Entrada sincronizada | Pai manual vivo que agrupa somente aquele órfão; disputa aberta e C7. (`app/ponto/services/entrada_sincronizada.py:48-62`.) |

As reaberturas efetivas da disputa são:

| Porta | Escrita e restrições | Chamadores encontrados |
|---|---|---|
| `reabrir_admin` | Limpa fechamento/autor/resolução/motivo; exige fechada, usuário e menos de duas reaberturas administrativas. (`models.py:2202-2225`.) | Ação administrativa que busca a fechada mais recente. (`services/acoes_disputa.py:247-259`.) |
| `reabrir_sistema` | Limpa os mesmos campos; idempotente se já aberta; sem teto administrativo; ocorrência no pai. (`models.py:2231-2244`.) | Ressuscitação (`models.py:1620-1622`); desfazer carimbo sem lastro (`services/relavra_via.py:260-261`); devolução de validação, condicionada à flag (`services/validacao.py:242-243`); ausência rejeitada (`services/materializacao.py:681-696`); comando de órfãs enterradas (`management/commands/reabrir_orfas_enterradas.py:165-172`). |

`reabrir_por_colab` reabre **o questionário de disputa que permanece aberta**; não é terceiro escritor da reabertura da disputa. (`models.py:2257-2266,2288-2304`.)

**3. Cron/vigia e contador geral**

O agendamento comprovado é **`fechar_becos_disputa --alarme`**, dependente de `reconciliar_perguntas_orfas`. A configuração não pede aplicação de fechamento. (`app/config/crons.py:663`.)

O comando percorre abertas, classifica pelo recorte de beco, conta fecháveis/barradas e, em `--alarme`, termina com código 2 se houver candidatas. Só aplica com `--apply` e sem `--alarme`. (`management/commands/fechar_becos_disputa.py:30-70`.)

Não localizei um vigia agendado cujo universo seja exatamente **todas as disputas abertas com C7 verdadeiro**; a existência por invocação dinâmica ou configuração externa permanece **[não verificado]**. Os mecanismos próximos têm universos diferentes:

- Relatório sob demanda: usa C7, mas separa `SEM_PERGUNTA` **antes** da consulta; `PRONTA_FECHAR` não inclui as vazias. (`relatorios_disputa.py:12-25,45-58`; `app/config/crons.py:1463`.)
- Auditor C2: conta **perguntas** por carimbos e veredito da célula; não consulta C7 e nem filtra disputa aberta, apesar do rótulo. (`management/commands/auditar_invariantes_chamados.py:1601-1605`.)
- `bin/censo_folha.py` chama uma variável de `pviva`, mas conta carimbo de resposta nulo em disputa aberta; não é C6/C7. (`bin/censo_folha.py:28-29`.)

**4. Leitores de C7 e homônimos**

As chamadas diretas encontradas ao juiz são: relatório (`relatorios_disputa.py:24`); reconciliador (`reconciliador.py:412,612,1171`); materialização (`services/materializacao.py:591,1322`); beco (`services/becos.py:33,56`); competência (`services/competencia_trancada.py:178,227`); explicador (`services/explicador.py:139`); emissão/S143 (`services/disputa_emissao.py:1278,2098`); entrada sincronizada (`app/ponto/services/entrada_sincronizada.py:59`). O registro em `core/juizes.py` é endereço declarativo, não chamada. (`app/core/juizes.py:287`.)

**A divergência do explicador apontada na triagem foi removida:** ele consulta C7 e publica esse resultado na chave `pode_fechar`. (`services/explicador.py:134-165`.)

Os consumidores dessa chave são:

| Consumidor | Significado empregado |
|---|---|
| `desenlace` | Traduz verdadeiro como **“Tudo validado e gravado”**. Isso afirma mais que C7: o predicado também aceita universo vazio e não exige que toda pergunta tenha carimbo de validação/materialização. (`services/desenlace.py:63-65`; `models.py:1905-1917`; `juizes.py:1403-1406`.) |
| Modal | Usa a chave para cor e aviso “Nada pendente… pode fechar/regularizar”. (`app/templates/chamados/partials/modal_fio.html:435,446`.) |

Outro significado próximo, porém sem a chave homônima: `disputa_viva_do_chamado` considera **sem perguntas OU fio vivo**; uma disputa vazia pode, portanto, travar esse leitor enquanto C7 aceita o universo vazio. (`juizes.py:2437-2444`; `models.py:1916-1917`.)

**5. Quando `fechar()` recusa ou falha apesar de C7 verdadeiro**

Para **o mesmo universo e estado**, não encontrei diferença no predicado normal de negócio: C7 exige nenhuma resposta sem veredito e nenhum fio vivo; `fechar()` consulta a mesma lista e `pergunta_viva`. Ele não chama `pode_fechar()` literalmente. (`models.py:1915-1917,1950-1967`; `juizes.py:1504-1513`.)

| Situação adicional | Resultado |
|---|---|
| Motivo explícito fora das portas permitidas | `ValueError`. (`models.py:1940-1941`.) |
| Disputa já fechada | Mesma entrada retorna `False`; entrada diferente levanta `ValueError`. C7 não testa se já está fechada. (`models.py:1915-1917,1943-1947`.) |
| Usuário ausente/falso | `ValueError`. (`models.py:1948-1949`.) |
| C7 recebeu `perguntas=` incompleto ou antigo | A porta relê a relação inteira e pode encontrar resposta/viva que a consulta anterior não recebeu. Possibilidade estática; ocorrência nas disputas anexas **[não verificado]**. (`models.py:1889-1890,1916,1950,1959`.) |
| Estado mudou entre consulta e fechamento | As leituras dos predicados antecedem as travas internas e não são repetidas depois delas. Ocorrência concreta **[não verificado]**. (`models.py:1950-1976`.) |
| Exceção na trava, carimbos das exceções, save ou trilha | Pode impedir a conclusão do ato; qual operação teria falhado nos casos anexos **[não verificado]**. (`models.py:1974-2004`.) |

A tranca **já participa do predicado**, tanto na lista de respostas quanto em C6; não é uma recusa adicional posterior de `fechar()`. (`models.py:1903-1909`; `juizes.py:1403-1406`.)

**O168 pode reter o pai mesmo após a disputa fechar.** O signal pede a reconciliação do pai, aceita recusa e captura exceções. O juiz do CSF usa o mesmo corpo de resposta sem veredito, mas com `ignorar_tranca=True` e universo de origem mais destino; C7 conserva a isenção da tranca. Pai aberto, isoladamente, não prova falha do fechamento da disputa. (`signals.py:29-47`; `models.py:699-719`; `app/ponto/supra_juiz.py:561-574`.)

**6. Campos: significado, escritores e leitores**

| Campo solicitado | Significado e escritor |
|---|---|
| `validada_em` | Instante da validação; escritor normal `_carimbar_validacao`; a porta `carimbar(... resposta='validar')` também pode preenchê-lo. Resposta nova, devolução e reabertura podem limpá-lo. (`models.py:2527,3217-3219`; `services/validacao.py:159,224`; `services/disputa_emissao.py:1480,1595`.) |
| `materializada_em` | Instante do carimbo de resolução; a via declara o tipo de resolução, não necessariamente plantio de batida. Portas de carimbo/descarimbo e lote estão na tabela anterior. (`models.py:2542-2545,3180-3184,3291,3363-3365,3502`.) |
| `respondida_em` | Instante da resposta; pode ser o instante atual ou o testemunhado pela nota. Não é dia do fato. (`models.py:2526`; `services/disputa_emissao.py:1462-1468,1660,1672-1681`.) |
| `resposta` | O campo real é `resposta_colab`, texto cujo formato depende de `tipo_resposta`: escolha, hora, data/hora etc. Escritores: resposta normal, automática e limpeza da reabertura. (`models.py:2520-2526`; `services/disputa_emissao.py:1435-1459,1467,1595,1659`.) |
| `status/estado` da pergunta | `estado` é propriedade calculada; não há escritor de coluna de estado. (`models.py:3116-3131`.) |
| Retratação da pergunta | Via `duplicata`/`motivo_retratado` e ato na sequência de decisões; não é `Batida.retratada_em`. (`models.py:2573,2577-2580,2120-2125,4001-4009`; `services/fio.py:1398-1401`.) |
| `via` | O campo é `via_resolucao`: código textual da resolução; não é instante nem dia. (`models.py:2639-2641`.) |
| Fechamento da disputa | `fechada_em` é instante; `aberta` é 

[truncado em 20000 de 31970 caracteres]

## 15/11 08:02 · sessao 5d38228c71a2 · DISPUTA-ZUMBI-1110 · sonda · origem=Code

```python
# Django shell; executar no envelope de leitura, tenant juliani ja ativo.
# OBRA: DISPUTA-ZUMBI-1110
# HASH informado: 880331a8c8d394724b78217edc84bede94b99cb7.
# Conferido na copia; correspondencia ao HEAD Git: [nao verificado].
# A copia disponibilizada nao contem .git.
# Execucao, resultados e atualidade do dump: [nao verificado].
# Causa historica de nao terem fechado: [nao verificado].
#
# U = fechada_em IS NULL, sem filtro no chamado-pai:
# app/chamados/models.py:1804, :1870-1871.
# Empresa = empresa do colaborador DA DISPUTA:
# app/chamados/models.py:1783-1786; app/colaboradores/models.py:321.
# C7 real: DisputaSupervisao.pode_fechar(perguntas=...):
# app/chamados/models.py:1911-1917.
# C6 real: fio_vivo(perguntas), que chama pergunta_viva:
# app/chamados/juizes.py:1504-1513, :862.
#
# Carimbo usado para "resolvida": materializada_em:
# app/chamados/models.py:3117-3130, :3502-3505.
# Via gravada: via_resolucao:
# app/chamados/models.py:2639-2641; atribuicao conjunta em :3180-3184.
# Pergunta pode morrer por leitura sem carimbo:
# app/chamados/juizes.py:1400-1406.
#
# fechar() NAO sera chamado. Suas recusas de estado sao medidas pelos
# leitores reais:
# respostas_sem_veredito: app/chamados/models.py:1873-1909, :1950-1958;
# pergunta viva/C6: app/chamados/models.py:1959-1966.
# Tranca e lida pelo juiz e retira perguntas dessas recusas:
# app/chamados/models.py:1903-1909; app/chamados/juizes.py:974-992.
# fechar exige user: app/chamados/models.py:1948-1949.
# Usuario de eventual tentativa historica: [nao verificado].
# A etapa de lock ADQUIRE as travas, sem consulta separada de disponibilidade:
# app/chamados/models.py:1974-1976, :2707-2727, :2744-2771.
# Sua disponibilidade: "nao mensuravel sem escrever" neste envelope.
#
# Convencoes da sonda:
# Ultimo carimbo = MAX(materializada_em) entre as perguntas atuais da disputa.
# Empates preservam todas as vias; a combinacao conta uma vez no histograma.
# Cortes usam data local: ANTES < corte; EM_OU_DEPOIS >= corte.
# Sem carimbo fica em balde proprio, fora de ambas as faixas dos cortes.
# data_evento pertence a pergunta, nao a disputa:
# app/chamados/models.py:2494-2503.
# A amostra preserva todas as datas distintas, inclusive null.
# Via gravada nao prova o emissor/call site historico: [nao verificado].
# Qualquer ERRO invalida a conclusao; contagens podem ser parciais.
# Sem only/defer, troca de tenant, atomic, SET ou chamada de lock.

erros = 0
logging_mod = sys_mod = None
nivel_anterior = bytecode_anterior = None


def emitir(chave, valor):
    print("{} {}".format(chave, valor), flush=True)


class FalhaInterna(BaseException):
    def __init__(self, original):
        self.original = original


class ConsultaRecusada(BaseException):
    pass


def erro(exc):
    global erros
    if isinstance(exc, FalhaInterna):
        exc = exc.original
    erros += 1
    emitir("ERRO", "{} {}".format(erros, type(exc).__name__))


try:
    import sys

    sys_mod = sys
    bytecode_anterior = sys.dont_write_bytecode
    sys.dont_write_bytecode = True

    import logging

    logging_mod = logging
    nivel_anterior = logging.root.manager.disable
    logging.disable(10**9)

    import json
    import re
    from collections import Counter
    from contextlib import ExitStack
    from datetime import date
    from unittest.mock import patch

    from django.conf import settings
    from django.db import connection, connections
    from django.db.models import Prefetch
    from django.utils import timezone

    alias = connection.alias

    def somente_select(execute, sql, params, many, context):
        if (
            many
            or context["connection"].alias != alias
            or not isinstance(sql, str)
            or not re.match(r"\s*SELECT\b", sql, re.I)
            or ";" in sql
            or re.search(
                r"\bFOR\s+(?:(?:NO\s+KEY|KEY)\s+)?(?:UPDATE|SHARE)\b"
                r"|\bINTO\b|\b(?:INSERT|UPDATE|DELETE|MERGE|CALL|COPY)\b"
                r"|\b(?:set_config|nextval|setval|pg_advisory\w*)\s*\(",
                sql,
                re.I,
            )
        ):
            raise ConsultaRecusada()
        return execute(sql, params, many, context)

    # Instrumentacao apenas de falha interna, em memoria.
    # O C6 registra excecoes em app/chamados/juizes.py:1473-1488;
    # o registrador escreve em logging: app/core/observ.py:73-82.
    # Propaga em BaseException para nao medir um fallback apos erro interno.
    # Nenhum predicado de negocio e substituido.
    def falha_interna(logger_nome, operacao, exc, **contexto):
        raise FalhaInterna(exc)

    def emitir_json(chave, valor):
        emitir(chave, json.dumps(valor, ensure_ascii=True, sort_keys=True))

    with ExitStack() as pilha:
        for nome_alias in connections:
            pilha.enter_context(
                connections[nome_alias].execute_wrapper(somente_select)
            )

        # Classes: app/chamados/models.py:27, :1757, :2484.
        from chamados.models import (
            DisputaSupervisao,
            PerguntaDisputa,
            ChamadoColaborador,
        )
        from colaboradores.models import Empresa  # app/colaboradores/models.py:40
        # app/chamados/catalogo/motor.py:30-31.
        from chamados.catalogo.motor import TERMINAIS
        # app/chamados/juizes.py:1504, :974.
        from chamados.juizes import fio_vivo, dia_da_pergunta_trancado
        from core import observ  # app/core/observ.py:73

        pilha.enter_context(
            patch.object(observ, "registrar_engolido", new=falha_interna)
        )
        # Fuso configurado: app/config/settings/base.py:93, :95.
        pilha.enter_context(timezone.override(timezone.get_default_timezone()))
        if not settings.USE_TZ:
            raise RuntimeError("use_tz_incompativel")

        # Banco lateral e tenant: app/config/settings/sombra.py:7-14, :29.
        # Apenas confere; nao troca banco/schema.
        with connection.cursor() as cursor:
            cursor.execute("SELECT current_database(), current_schema()")
            banco, schema = cursor.fetchone()
        if banco != "sombra" or schema != "juliani":
            raise RuntimeError("envelope_incorreto")

        emitir("BANCO", banco)
        emitir("TENANT", schema)
        emitir("INSTANTE_LOCAL", timezone.localtime(timezone.now()).isoformat())
        emitir("FUSO_LOCAL", str(timezone.get_current_timezone()))
        emitir("DUMP_DE_HOJE", "[nao verificado]")
        emitir("CAUSA_HISTORICA_NAO_FECHOU", "[nao verificado]")
        emitir("M2_LOCK", "nao mensuravel sem escrever")
        emitir("M2_USER", "nao informado; fechar exige user")
        emitir("M2_CORTE_REGRA", "ANTES < corte; EM_OU_DEPOIS >= corte")
        emitir("M2_CARIMBO_CAMPO", "materializada_em")
        emitir("M2_VIA_CAMPO", "via_resolucao")

        cortes = (date(2026, 10, 22), date(2026, 11, 1))
        fixas = [
            "M1_U",
            "M1_MEDIDOS",
            "M1_PODE_FECHAR_TRUE",
            "M1_COM_PERGUNTA_VIVA_C6",
            "M1_SEM_PERGUNTA_VIVA_C6",
            "M1_SEM_PERGUNTA_NENHUMA",
            "M2_MEDIDOS",
            "M2_SEM_CARIMBO",
            "M2_PAI_TERMINAL",
            "M2_PAI_NAO_TERMINAL",
            "M2_SEM_PAI",
            "M2_RECUSA_RESPOSTAS_SEM_VEREDITO",
            "M2_RECUSA_PERGUNTA_VIVA",
            "M2_PERGUNTAS_TRANCADAS",
            "M2_COM_PERGUNTA_TRANCADA",
        ]
        for corte in cortes:
            for lado in ("ANTES", "EM_OU_DEPOIS"):
                fixas.append(
                    "M2_ULTIMO_CARIMBO_{}_{}".format(lado, corte.isoformat())
                )

        contagens = {
            pk: Counter()
            for pk in Empresa.objects.using(alias).values_list("pk", flat=True)
        }
        # Vias catalogadas: app/chamados/models.py:2603-2621.
        vias_catalogadas = {v for v, _ in PerguntaDisputa.VIAS}
        # STATUS_CHOICES / status_local: app/chamados/models.py:28-36, :52.
        status_catalogados = {s for s, _ in ChamadoColaborador.STATUS_CHOICES}

        # Nunca imprime texto livre, nome, CPF, objetos ou mensagens de excecao.
        def via_segura(valor):
            if not valor:
                return "vazia"
            return valor if valor in vias_catalogadas else "fora_do_catalogo"

        # Relacoes: app/chamados/models.py:1783, :1796, :2492-2497, :3494-3500;
        # empresa do colaborador: app/colaboradores/models.py:321.
        universo = (
            DisputaSupervisao.objects.using(alias)
            .filter(fechada_em__isnull=True)
            .select_related("colaborador__empresa", "chamado")
            .prefetch_related(
                Prefetch(
                    "perguntas",
                    queryset=PerguntaDisputa.objects.using(alias)
                    .select_related("celula", "chamado_destino")
                    .order_by("pk"),
                )
            )
            .order_by("pk")
        )

        ultimo_pk = None
        amostras = 0
        while True:
            pagina_qs = universo
            if ultimo_pk is not None:
                pagina_qs = pagina_qs.filter(pk__gt=ultimo_pk)
            pagina = list(pagina_qs[:100])
            if not pagina:
                break

            for disputa in pagina:
                try:
                    empresa_pk = disputa.colaborador.empresa_id
                    cont = contagens.setdefault(empresa_pk, Counter())
                    cont["M1_U"] += 1
                    perguntas = list(disputa.perguntas.all())

                    # Juizes REAIS:
                    # app/chamados/models.py:1911;
                    # app/chamados/juizes.py:1504.
                    pode = disputa.pode_fechar(perguntas=perguntas)
                    viva = fio_vivo(perguntas)

                    cont["M1_MEDIDOS"] += 1
                    cont["M1_PODE_FECHAR_TRUE"] += int(pode)
                    cont["M1_COM_PERGUNTA_VIVA_C6"] += int(viva)
                    cont["M1_SEM_PERGUNTA_VIVA_C6"] += int(not viva)
                    cont["M1_SEM_PERGUNTA_NENHUMA"] += int(not perguntas)
                    if not pode:
                        continue

                    # Mesmo leitor chamado por fechar():
                    # app/chamados/models.py:1950.
                    pendentes = disputa.respostas_sem_veredito(
                        perguntas=perguntas
                    )
                    # Tranca real, por leitura: app/chamados/juizes.py:974-992.
                    trancadas = sum(
                        int(dia_da_pergunta_trancado(p)) for p in perguntas
                    )

                    carimbadas = [
                        p for p in perguntas if p.materializada_em is not None
                    ]
                    ultimo = max(
                        (p.materializada_em for p in carimbadas), default=None
                    )
                    local = timezone.localtime(ultimo) if ultimo else None
                    empatadas = [
                        p for p in carimbadas if p.materializada_em == ultimo
                    ]
                    vias = sorted(
                        {via_segura(p.via_resolucao) for p in empatadas}
                    )
                    via_grupo = "__".join(vias) if vias else "sem_carimbo"

                    pai = disputa.chamado
                    # Predicado terminal: status_local in TERMINAIS,
                    # app/chamados/catalogo/motor.py:30-31.
                    terminal = (
                        pai.status_local in TERMINAIS if pai is not None else None
                    )
                    pai_status = (
                        pai.status_local
                        if pai is not None
                        and pai.status_local in status_catalogados
                        else ("sem_pai" if pai is None else "fora_do_catalogo")
                    )

                    delta = Counter({
                        "M2_MEDIDOS": 1,
                        "M2_SEM_CARIMBO": int(local is None),
                        "M2_PAI_TERMINAL": int(terminal is True),
                        "M2_PAI_NAO_TERMINAL": int(terminal is False),
                        "M2_SEM_PAI": int(pai is None),
                        "M2_RECUSA_RESPOSTAS_SEM_VEREDITO": int(bool(pendentes)),
                        "M2_RECUSA_PERGUNTA_VIVA": int(viva),
                        "M2_PERGUNTAS_TRANCADAS": trancadas,
                        "M2_COM_PERGUNTA_TRANCADA": int(trancadas > 0),
                        "M2_PAI_STATUS_" + pai_status: 1,
                        "M2_VIAS_ULTIMO_CARIMBO_" + via_grupo: 1,
                        "M2_ULTIMO_CARIMBO_DIA_" + (
                            local.date().isoformat()
                            if local else "sem_carimbo"
                        ): 1,
                    })
                    if local is not None:
                        for corte in cortes:
                            lado = (
                                "ANTES" if local.date() < corte
                                else "EM_OU_DEPOIS"
                            )
                            delta["M2_ULTIMO_CARIMBO_{}_{}".format(
                                lado, corte.isoformat()
                            )] += 1
                    for chave, quantidade in delta.items():
                        cont[chave] += quantidade

                    # Uma linha para CADA disputa pode_fechar=True medida.
                    detalhe = {
                        "disputa_pk": disputa.pk,
                        "empresa_pk": empresa_pk,
                        "pai_status": pai_status,
                        "pai_terminal": terminal,
                        "ultimo_carimbo_local": (
                            local.isoformat() if local else None
                        ),
                        "via": vias,
                        "perguntas_ultimo_carimbo": [p.pk for p in empatadas],
                        "respostas_sem_veredito": len(pendentes),
                        "recusa_por_vivas": bool(viva),
                        "perguntas_trancadas": trancadas,
                    }
                    emitir_json("M2_DISPUTA", detalhe)

                    if amostras < 25:
                        # app/chamados/models.py:2501-2503.
                        dias = sorted({
                            p.data_evento.isoformat()
                            for p in perguntas if p.data_evento
                        })
                        if any(p.data_evento is None for p in perguntas):
                            dias.append(None)
                        amostra = dict(detalhe)
                        amostra["colaborador_pk"] = disputa.colaborador_id
                        amostra["data_evento"] = dias
                        emitir_json("M3_AMOSTRA", amostra)
                        amostras += 1

                except BaseException as exc:
                    erro(exc)

            ultimo_pk = pagina[-1].pk

        # Todas as empresas, inclusive zero; total = soma das empresas.
        chaves = sorted(
            set(fixas).union(*(set(c) for c in contagens.values()))
        )
        totais = Counter()
        for empresa_pk in sorted(contagens):
            cont = contagens[empresa_pk]
            for chave in chaves:
                emitir("{}_EMPRESA_{}".format(chave, empresa_pk), cont[chave])
                totais[chave] += cont[chave]
        for chave in chaves:
            emitir(chave + "_TOTAL", totais[chave])
        emitir("M3_AMOSTRAS_TOTAL", amostras)

except BaseException as exc:
    erro(exc)
finally:
    try:
        if logging_mod is not None and nivel_anterior is not None:
            logging_mod.disable(nivel_anterior)
        if sys_mod is not None and bytecode_anterior is not None:
            sys_mod.dont_write_bytecode = bytecode_anterior
    except BaseException as exc:
        erro(exc)
    emitir("ERROS_TOTAL", erros)
    emitir("FIM", "ok={}".format(int(erros == 0)))
```

_5557 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
