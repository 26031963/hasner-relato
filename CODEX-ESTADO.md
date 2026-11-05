# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 15:13** · publicado em **05/11/2026 15:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 15:13 · sessao 799d23195448 · - · - · origem=Ronald (tmux)

1. **Item 4 — selo que não morde:** `test_MORDE_duas_competencias_duas_contas` cria uma pergunta em cada uma de duas competências e exige duas consultas à tranca; não repete competência alguma. Com `perguntas_do_app` anterior do patch, cada pergunta recebe sua própria instância e consulta sua competência uma vez: total igualmente dois. É demonstração estática; execução com reversão [nao verificado]. A contagem observa chamadas a `periodo_trancado`, e o memo usa empresa/mês/ano. **Toca dinheiro: nao**, diretamente. (`app/chamados/tests/test_n1_perguntas_do_app.py:40`, `:73`; `app/chamados/juizes.py:935`.)

   Isso não invalida a suíte inteira: o teste imediatamente anterior cria três perguntas da mesma competência e exige apenas uma consulta, exercitando a reutilização que aquele selo não exercita. (`app/chamados/tests/test_n1_perguntas_do_app.py:58`.)

2. **Item 1 — escritores persistidos alcançados pelos laços alterados:**

   | Campos/valores | Escritor | Leitor/consumidor |
   |---|---|---|
   | `materializada_em`, `materializada_por_id`, `via_resolucao` | `_escrever_carimbo` (`app/chamados/models.py:2532`) | C6 (`app/chamados/juizes.py:1087`, `:1116`, `:1122`) |
   | `respondida_em`, `validada_em`, `validada_por` opcionais | `carimbar`, quando recebe resposta apropriada (`app/chamados/models.py:2561`) | C6 (`app/chamados/juizes.py:1078`, `:1092`); resposta sem veredito (`app/chamados/models.py:1772`) |
   | `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `carimbar(..., lavrar_veredito=True)` (`app/chamados/models.py:2577`) | O fluxo de resolução pede essa lavra (`app/chamados/services/disputa_emissao.py:1603`) |
   | `chamado_destino`, `data_evento` se anteriormente nula, campos da lavra | `reapontar_resolvedora` (`app/chamados/services/fio.py:831`, `:845`) | Migração compara motivo/data e reaponta (`app/chamados/services/fio.py:937`) |
   | Via `duplicata`, carimbo e `recusa_motivo` | `retratar_duplicata` → `carimbar_identidade` (`app/chamados/services/fio.py:891`; `app/chamados/models.py:2604`) | C6 lê via/carimbo (`app/chamados/juizes.py:1111`) |
   | `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | `DisputaSupervisao.fechar` (`app/chamados/models.py:1850`) | A própria porta verifica fechamento/identidade do ato (`app/chamados/models.py:1809`); reconciliador verifica antes de fechar (`app/chamados/reconciliador.py:634`) |
   | Limpeza de carimbo, validação e resposta temporal | `ressuscitar_perguntas` → `descarimbar_em_lote` (`app/chamados/models.py:1493`, `:2631`) | `_universo_da_ressuscitacao` julga a cópia sem esses carimbos (`app/chamados/models.py:1444`) |

   Os consumidores do diff chamam essas portas, mas N1 não altera os corpos dessas escritas: retratação chama `carimbar`; resolução chama `carimbar` com validação e lavra; migração chama dedup e reapontamento. (`app/chamados/reconciliador.py:620`; `app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/fio.py:943`, `:947`.)

3. **Item 3 — não demonstrado por esta subanálise:** o helper atribui colaborador do mapa pela mesma chave de colaborador, e `perguntas_do_app` continua lendo C6. Ausência do nome `N1-IDENTITY-MAP` nas leis anexadas não demonstra, por si, alteração de regra de negócio. (`app/chamados/juizes.py:985`; `app/chamados/services/pergunta_no_app.py:53`.)

   A descrição de auditoria antiga na L-051 diz que `encerrar` ainda carimba perguntas; o código atual mede perguntas mortas por leitura e encerra chamados/disputas sem essa escrita. Não usar aquela descrição como prova de regressão N1. (`app/docs/LEIS.md:92`; `app/chamados/services/competencia_trancada.py:125`, `:178`.)

   O corte anexado autoriza a leitura da tranca com exceção de toque humano posterior, e o juiz implementa essa sequência. (`app/docs/CORTES.md:272`; `app/chamados/juizes.py:917`, `:946`.)

## 05/11 15:12 · sessao f0fbebfbe781 · H12-ITEM-C · leitura · origem=Code

**Os 18 valores de horas são aritmeticamente compatíveis com o FM informado; as três linhas 8792 não podem ser validadas pelo FM.** A conversão de horas está em `app/folha/export.py:42`; a 8792 usa datas de ausência, em `app/folha/export.py:721`. Os valores dos anexos são os transcritos no enunciado, sem verificação independente da execução **[nao verificado]**.

**1. Campos, mapa e conversão.** O exportador resolve rubricas ativas globais e depois aplica os vínculos específicos da empresa, que prevalecem: [app/folha/export.py:958](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/export.py:958). Portanto, o mapa efetivamente cadastrado em emp2/emp3 é **[nao verificado]**; a fixture explicita os vínculos abaixo em `app/folha/tests/test_sm1_monster_export.py:73`.

| Rubrica | Origem pela regra do exportador | Conversão / referência |
|---|---|---|
| 0025 | `horas_noturnas` → `adicional_noturno` | `app/folha/export.py:436` |
| 0243 | `horas_intra_indenizada` → `intrajornada_diurna` | `app/folha/export.py:437` |
| 8069 | `horas_atraso + horas_saida_antecipada` → `atraso_diurno` | `app/folha/export.py:388`, `app/folha/export.py:440` |
| 0200 | HE100 normal, parcela de feriado ou folga trabalhada | `app/folha/export.py:409`, `app/folha/export.py:431`, `app/folha/export.py:435` |
| 8792 | Quantidade de datas retornadas por `datas_falta`; nenhum campo FM | `app/folha/export.py:721`, `app/folha/export.py:207` |

Para rubrica de horas, o inteiro é `int(round(float(valor) * 100))`; para dias, `int(round(float(valor)))`: [app/folha/export.py:34](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/export.py:34). Assim, `0,09 h → 9`, sem conversão para HH:MM (`app/folha/export.py:42`).

Em **col457/0200**, a parcela normal é `14,67 − 14,67 = 0`; os `1467` correspondem numericamente ao feriado informado **[nao verificado]**. Essa emissão depende de o feriado não ser suprimido por ciclo 12x36 sem dobra; ciclo e cláusula do colaborador não vieram no dado **[nao verificado]** (`app/folha/export.py:409–412`). Em **col951/0200**, `7,34 → 734` corresponde à folga trabalhada informada **[nao verificado]**, campo lido em `app/folha/export.py:435`.

**Contraexemplos:** `0200 = horas_extras` não descreve col951, pois há evento próprio de folga (`app/folha/export.py:435`). O mapa também não é fixo: o teste demonstra intrajornada empresarial em 270 prevalecendo sobre global 243 (`app/folha/tests/test_r1_caracterizacao_rubricas_map.py:31`). **Certeza:** alta sobre fórmulas; condicionada à configuração viva do mapa e, em col457, à regra de feriado **[nao verificado]**.

**2. Os sete entregues versus os quatro ausentes.** O código local não contém uma regra que reproduza essa separação histórica: ele exclui **qualquer `data_demissao` preenchida**, sem compará-la à competência ou à data de geração: [app/folha/export.py:840](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/export.py:840). Esse trecho ainda apresenta o bloqueio geral; a implementação da cura H12 executada no anexo não está comprovada nesta cópia **[nao verificado]**.

Os sete entregues têm FM `aprovado` e os quatro ausentes têm `aberto`, conforme a transcrição **[nao verificado]**. Isso é associação observada, não o critério de exclusão: ambos os status são aceitos (`app/folha/export.py:668`, `app/folha/export.py:698`). Além disso, gerar o TXT pode produzir o carimbo de aprovação (`app/folha/views.py:246–252`).

**Contraexemplo:** um FM `aberto` que passe pela classificação pode emitir linhas (`app/folha/export.py:697–708`). Logo, “os quatro não constam porque estavam abertos” não está demonstrado. A data de geração e o estado cadastral naquele instante não constam dos anexos **[nao verificado]**. **Certeza:** alta sobre o bloqueio atual; motivo histórico da divisão 7/4 **[nao verificado]**.

**3. Por que a sombra encontra `no_txt_entregue=[]`.** Há um mecanismo explícito que mascara o **conteúdo inteiro do TXT**: `ExportacaoDominio.conteudo` pertence ao conjunto de textos mascarados (`bin/sombra_regras_pessoais.py:105`), e `texto()` substitui esse conteúdo por `… <hash>` (`bin/sombra_regras_pessoais.py:29–33`). A máscara é aplicada aos objetos em `bin/sombra_mascara_pessoal.py:44–61` e chamada após a restauração em [bin/sombra.sh:272](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/bin/sombra.sh:272).

A máscara inicial altera CPF, nome, email e token (`bin/sombra_mascara.py:66–68`); a matrícula do exportador usa `codigo_dominio` (`app/folha/export.py:709`). O leitor oficial exige registro 10 com 43 caracteres; um conteúdo reduzido a hash não fornece registros reconhecíveis (`app/folha/export.py:236–244`).

**Contraexemplo:** mascarar apenas nome/CPF preservaria a matrícula usada na busca (`app/folha/export.py:709`); mascarar `conteudo` elimina as linhas pesquisáveis (`bin/sombra_regras_pessoais.py:33`). **Certeza:** alta sobre esse mecanismo; que ele foi exatamente o caminho executado pela sonda e pelo objeto consultado é **[nao verificado]**. Portanto, `[]` na sombra não comprova ausência no TXT original.

**4. col954, 8792=2 e `minutos_abonados=1440`.** A 8792 está vinculada a `falta_diurna`, em dias, no mapa da fixture (`app/folha/tests/test_sm1_monster_export.py:78`). O exportador busca ausências aprovadas que intersectam a janela, dos tipos que o catálogo considera descontáveis (`app/folha/export.py:76–81`, `app/folha/export.py:169`). Depois emite a quantidade de datas e os registros 11 correspondentes (`app/folha/export.py:204–209`).

Logo, `8792=2` exige que essa consulta retorne duas entradas de data; `1440` minutos abonados não participa dessa conta (`app/folha/export.py:721–725`). As ausências e os registros 11 de col954 não foram fornecidos **[nao verificado]**.

**Contraexemplo:** distribuir o mesmo total de minutos entre uma ou duas datas pode mudar a 8792, porque o numerador é `len(datas)` (`app/folha/export.py:207`). **Certeza:** alta sobre a regra; o valor 2 estar correto é **[nao verificado]**. A mesma limitação vale para 576/8792=1 e 643/8792=3 **[nao verificado]**.

**5. col639 readmitido.** No caminho que lê o FM gravado, `dem=None` passa pelo bloqueio de rescisão (`app/folha/export.py:840`). A admissão posterior ajusta o início da janela de retenção; quando o início ultrapassa o fim, a função retorna **nenhum motivo de retenção**, sem excluir o colaborador: [app/folha/export.py:1180](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/folha/export.py:1180).

Assim, passando pelos demais filtros, col639 **pode entrar na 09 com o FM gravado**, apesar da admissão em novembro (`app/folha/export.py:764–769`, `app/folha/export.py:942–954`, `app/folha/export.py:698–720`). A data de admissão, sozinha, não explica `hoje=[]`.

**Contraexemplo:** FM09 aprovado e positivo, demissão nula, admissão posterior e demais filtros satisfeitos permanece elegível nesses trechos. A porta HTTP fornece objetos recalculados em leitura (`app/folha/views.py:207–212`), enquanto a montagem sem injeção lê FM gravado (`app/folha/export.py:698–703`). Qual caminho a sonda usou é **[nao verificado]**. Também há estados diferentes entre anexos: PROD informa `dem=None`; sombra informa `dem=2026-10-29` **[nao verificado]**. **Certeza:** alta sobre o comportamento local; causa concreta de `hoje=[]` **[nao verificado]**.

Na tabela, **S** significa compatibilidade aritmética com o FM transcrito; **N\*** significa que o FM não permite comprovar, sem indicar divergência. “Alta” refere-se à aritmética; valores e execução permanecem os fornecidos **[nao verificado]**. `—` indica ausência no entregue informado **[nao verificado]**.

| col | rubrica | entregue | copia | campo FM | confere(S/N) | certeza |
|---|---|---:|---:|---|:---:|---|
| 400 | 0243 | — | 1300 | intra 13,00 ×100; `app/folha/export.py:437` | S | Alta |
| 457 | 0025 | — | 8224 | noturnas 82,24 ×100; `app/folha/export.py:436` | S | Alta |
| 457 | 0200 | — | 1467 | HE100 feriado 14,67 ×100; `app/folha/export.py:409` | S | Condicionada à não supressão; [nao verificado] |
| 457 | 0243 | — | 392 | intra 3,92 ×100; `app/folha/export.py:437` | S | Alta |
| 576 | 0025 | 14400 | 14400 | noturnas 144,00 ×100; `app/folha/export.py:436` | S | Alta |
| 576 | 0243 | 1400 | 1400 | intra 14,00 ×100; `app/folha/export.py:437` | S | Alta |
| 576 | 8792 | 1 | 1 | Nenhum; datas de ausência; `app/folha/export.py:721` | N* | [nao verificado] |
| 907 | 0025 | — | 11198 | noturnas 111,98 ×100; `app/folha/export.py:436` | S | Alta |
| 907 | 0243 | — | 124 | intra 1,24 ×100; `app/folha/export.py:437` | S | Alta |
| 907 | 8069 | — | 9 | atraso 0,09 + antecipada zero no recorte; `app/folha/export.py:440` | S | Alta |
| 951 | 0200 | 734 | 734 | folga trabalhada 7,34 ×100; `app/folha/export.py:435` | S | Alta |
| 951 | 0243 | 17 | 17 | intra 0,17 ×100; `app/folha/export.py:437` | S | Alta |
| 954 | 8792 | — | 2 | Nenhum; datas de ausência; `app/folha/export.py:721` | N* | [nao verificado] |
| 639 | 0025 | 12000 | 12000 | noturnas 120,00 ×100; `app/folha/export.py:436` | S | Alta |
| 639 | 0243 | 1500 | 1500 | intra 15,00 ×100; `app/folha/export.py:437` | S | Alta |
| 643 | 0243 | 1100 | 1100 | intra 11,00 ×100; `app/folha/export.py:437` | S | Alta |
| 643 | 8792 | 3 | 3 | Nenhum; datas de ausência; `app/folha/export.py:721` | N* | [nao verificado] |
| 752 | 0243 | 1500 | 1500 | intra 15,00 ×100; `app/folha/export.py:437` | S | Alta |
| 755 | 0025 | 12800 | 12800 | noturnas 128,00 ×100; `app/folha/export.py:436` | S | Alta |
| 755 | 0243 | 1600 | 1600 | intra 16,00 ×100; `app/folha/export.py:437` | S | Alta |
| 761 | 0243 | 1600 | 1600 | intra 16,00 ×100; `app/folha/export.py:437` | S | Alta |

## 05/11 15:11 · sessao d6de2868c599 · SOMBRA-NAO-CABE-2 · verificador · origem=Code

**VEREDITO: REPROVA.** O lote pode dar pergunta por morta depois de um toque que o caminho avulso reconheceria: ele conserva também a ausência do toque até terminar a rodada. (app/chamados/juizes.py:1041–1052)

Identidade do commit contra Git, execução dos testes e ocorrência desses cenários em produção: [não verificado]. [nao verificado]

1. **Quem escreve e quem lê.**

   | Estado ou função | Escritores | Leitores |
   |---|---|---|
   | Memo da tranca | `_memo_da_tranca` cria `_tranca_memo`; `dia_da_pergunta_trancado` preenche empresa e período; `_admin_tocou_o_dia_depois` preenche toque/conjunto. (app/chamados/juizes.py:975, app/chamados/juizes.py:939–945, app/chamados/juizes.py:1042–1051) | As mesmas funções consultam essas chaves; dentro do lote, o dicionário do lote prevalece sobre o da requisição e o do colaborador. (app/chamados/juizes.py:946, app/chamados/juizes.py:968–975, app/chamados/juizes.py:1045–1047) |
   | `LOTE_DA_TRANCA` | Declarado com `None`; `tranca_em_lote` instala `{}` e restaura o token em `finally`. O comando abre o contexto. (app/chamados/juizes.py:985, app/chamados/juizes.py:994–998, app/chamados/management/commands/vigiar_fio_morto.py:171) | `_memo_da_tranca`, o próprio context manager e `_admin_tocou_o_dia_depois`. (app/chamados/juizes.py:968, app/chamados/juizes.py:991, app/chamados/juizes.py:1041) |
   | Contexto da requisição | O middleware instala a requisição e restaura seu token. (app/core/middleware.py:78–82) | O memo lê o dono; `evento` lê usuário/IP. O lote é outro ContextVar. (app/chamados/juizes.py:971–975, app/core/observ.py:108–115, app/chamados/juizes.py:985) |
   | Cache do usuário sistema | `_toques_humanos_depois` grava `memo[('sistema',)]`. A busca subjacente usa `get_or_create`; o comando garantidor também cria/normaliza esse usuário. (app/chamados/juizes.py:1074–1076, app/core/utils.py:18, app/core/management/commands/garantir_user_sistema.py:13–41) | O filtro de humanos exclui o PK guardado. (app/chamados/juizes.py:1077–1078) |
   | `_toques_humanos_depois` | Produz o `frozenset`; não grava a trilha. Sua dependência `get_user_sistema` pode criar usuário ausente — comportamento já existente no caminho anterior fornecido. (app/chamados/juizes.py:1076, app/chamados/juizes.py:1111, app/core/utils.py:18) | `_admin_tocou_o_dia_depois` chama a versão plural ou avulsa. (app/chamados/juizes.py:1044, app/chamados/juizes.py:1049) |
   | Campos de `LogAuditoria` usados pelo helper | Cobrar escreve ação/modelo/objeto/descrição; Resolver aponta para a célula; Desarquivar aponta para o chamado. Todos chegam a `evento`, que cria a linha; `criado_em` é automático. (app/chamados/services/cobrar_dia.py:37–38, app/ponto/services/veredito_celula.py:334–336, app/chamados/services/arquivo.py:137–138, app/core/observ.py:134–137, app/core/models.py:307) | O helper lê tempo, usuário, ação, modelo, objeto e descrição nos dois modos. (app/chamados/juizes.py:1077–1108) |
   | Identidades de célula/chamado usadas na reconstrução | A porta cria célula com colaborador/data; `ChamadoColaborador.abrir` grava o chamado; `apontar_celula` pode preencher sua célula ausente. (app/ponto/portas/celula.py:247–248, app/chamados/models.py:932, app/chamados/services/vinculos.py:92–99) | O avulso procura os vínculos atuais; o plural reconstrói os pares pelos vínculos atuais. (app/chamados/juizes.py:1085–1094, app/chamados/juizes.py:1104–1110) |
   | Tranca consultada pelo memo | `trancar_periodo` cria/retranca; `reabrir_periodo` grava a reabertura. (app/ponto/services/fechamento.py:1392–1403, app/ponto/services/fechamento.py:1421–1425) | `periodo_trancado` seleciona a linha sem reabertura; C6 chega nela por `dia_da_pergunta_trancado`. (app/ponto/services/fechamento.py:1448–1454, app/chamados/juizes.py:945, app/chamados/juizes.py:1167) |

   No vigia, os consumidores chegam ao C6 pelo fio, pela alimentação das portas, pelo app e pelos contadores adicionais. A função tocada `perguntas_do_app` também atende a requisição real da API. (app/chamados/services/vigia_fio.py:416, app/chamados/juizes.py:1269, app/chamados/services/vigia_fio.py:603, app/chamados/juizes.py:1329, app/chamados/juizes.py:1346, app/chamados/services/vigia_fio.py:754, app/chamados/services/vigia_fio.py:812, app/api/views.py:2552)

   Fora do vigia, portas de escrita também consomem C6: a ressuscitação seleciona pelo juiz antes de limpar carimbos, e `carimbar` consulta o juiz antes de salvar. (app/chamados/models.py:1447, app/chamados/models.py:1481–1493, app/chamados/models.py:2585–2598)

2. **Contraexemplo durante o lote; alcance do vazamento.**

   Cenário construído com as identidades do próprio selo: colaborador `self.colab`, período julho/2026 trancado, P1 em **10/07/2026** e P2 em **11/07/2026**, ambas sem validação nem materialização. Esses estados são produzidos pelas fixtures existentes. (app/chamados/tests/test_snc2_toque_em_lote.py:29–44, app/chamados/tests/test_c7_red_minimos.py:110–118)

   P1 aquece o lote quando ainda não existe toque. Durante a rodada, outra execução confirma um toque humano para P2: `cobrar_dia_manual`, modelo `Colaborador`, objeto `self.colab.pk`, descrição contendo `dia=2026-07-11`, com `criado_em > pf.fechado_em`. Esse é o formato escrito pela porta e representado pela fixture `_cobrar`. (app/chamados/juizes.py:1042–1044, app/chamados/services/cobrar_dia.py:37–38, app/chamados/tests/test_snc2_toque_em_lote.py:46–54)

   Ao avaliar **P2 ainda não julgada**, o lote devolve “não tocou”, usando o conjunto anterior, e C6 retorna **False** pela tranca. Uma instância fresca de P2 no caminho avulso encontra o toque; sem os demais impedimentos, chega ao retorno **True** de pergunta não materializada. Portanto: **lote=morta; avulso=viva**. (app/chamados/juizes.py:1045–1052, app/chamados/juizes.py:1163–1173, app/chamados/juizes.py:1234–1235)

   Essa intercalação não exige que o vigia escreva: o contexto não estabelece transação de leitura nem imutabilidade da fonte. Ocorrência real dessa intercalação em produção: [não verificado]. (app/chamados/juizes.py:989–998)

   Quanto a requisição/escrita: a API permite abrir lote dentro de requisição e então prioriza seu memo, mas não há cadeia de produção demonstrada fazendo isso. O `finally/reset` restaura o contexto; vazamento para uma requisição posterior: [não verificado]. (app/chamados/juizes.py:968–975, app/chamados/juizes.py:989–998)

3. **Regra de negócio alterada.**

   A regra documentada é “tranca menos o dia que o ADMIN tocou depois dela”. O novo ramo passa a significar “tocou antes da primeira coleta desse `desde` no lote”; um toque posterior fica invisível até outra rodada. Essa alteração ocorre na dependência do juiz, mesmo preservando seus corpos. (app/chamados/juizes.py:949–952, app/chamados/juizes.py:1034–1045)

   C6 continua registrado como `pergunta_viva`, que delega a `_pergunta_viva_calc`; esta continua consultando a tranca antes dos demais degraus. Logo, manter o endereço do juiz preserva sua identidade, mas não assegura o mesmo veredito quando seu insumo fica congelado. (app/core/juizes.py:266, app/chamados/juizes.py:840, app/chamados/juizes.py:1167–1168)

   Aval adicional autorizando essa diferença temporal: [não verificado].

4. **Selo que não morde.**

   `test_lote_nao_vaza_e_request_segue_guardando_so_o_SIM` nunca abre lote e nunca instala requisição: verifica contexto inicialmente vazio, escreve toque e chama diretamente `dia_da_pergunta_trancado`. Por inspeção, esse caso continuaria válido com a cura revertida. (app/chamados/tests/test_snc2_toque_em_lote.py:116–124)

   Ele também não detectaria uma regressão que cacheasse o NÃO **somente no memo de requisição**, porque o contexto de requisição é instalado pelo middleware, ausente nesse teste. Ele exercita o memo da instância. (app/core/middleware.py:78, app/chamados/juizes.py:972–975, app/chamados/tests/test_snc2_toque_em_lote.py:118–124)

   A paridade usa `nullcontext` se o lote não existir: com reversão, pode comparar dois caminhos avulsos. Todos os toques desse teste precedem as avaliações; nenhum chega durante o lote. (app/chamados/tests/test_snc2_toque_em_lote.py:22–24, app/chamados/tests/test_snc2_toque_em_lote.py:96–114)

   O teste de desempenho tem mordida própria sobre o helper. Porém, remover apenas a abertura do lote **do comando** deixa os três testes novos exercitando os mesmos caminhos: nenhum chama o comando. Resultados dessas mutações por execução: [não verificado]. (app/chamados/tests/test_snc2_toque_em_lote.py:74–90, app/chamados/tests/test_snc2_toque_em_lote.py:116–124, app/chamados/management/commands/vigiar_fio_morto.py:171)

5. **Cadastro de hoje decidindo passado.**

   Há esse comportamento, herdado: o dia histórico da pergunta é classificado usando `colab.empresa` e o corte atual dessa empresa. A edição/transferência pode alterar a empresa do colaborador; o formulário também permite alterar o corte. (app/chamados/juizes.py:932–945, app/ponto/janelas.py:50–63, app/colaboradores/views.py:1313, app/colaboradores/views.py:1386–1388, app/colaboradores/views.py:496–523)

   Exemplo construído: **10/07/2026**, julho trancado, agosto aberto, sem toque. Alterar hoje o corte de **21 para 10** muda a competência desse dia de julho para agosto; o degrau da tranca deixa de matar a pergunta. Ocorrência real: [não verificado]. (app/ponto/janelas.py:24–33, app/chamados/juizes.py:942–948)

   O helper também atribui trilhas antigas de Resolver/Desarquivar pelos relacionamentos atuais de célula e chamado. O diff não introduz essa leitura, mas o lote conserva seu resultado por mais tempo. (app/chamados/juizes.py:1085–1094, app/chamados/juizes.py:1104–1110, app/chamados/juizes.py:1042–1045)

6. **Classes que já reprovaram obra.**

   **Segundo juiz / derivação paralela:** não identifico novo C6; o registro permanece único. Existem dois ramos de reconhecimento do toque — `contains`/consultas direcionadas e regex/reconstrução plural — dentro do helper; a divergência demonstrada é a validade temporal do resultado. (app/core/juizes.py:266, app/chamados/juizes.py:1081–1110, app/chamados/juizes.py:1041–1052)

   **Segundo escritor fora da porta:** a trilha continua chegando a `evento`. Há escrita condicional de usuário em `get_user_sistema`, anterior à mudança, que impede tratar toda essa cadeia como incondicionalmente só leitura. (app/core/auditoria.py:22–24, app/chamados/services/arquivo.py:137–138, app/core/observ.py:134–137, app/core/utils.py:18)

   **Relógio fora da borda:** permanece uma leitura interna preexistente: o comando captura `agora/hoje`, mas C6 pode alcançar `_dentro_da_janela_viva`, que chama `date.today()`. (app/chamados/management/commands/vigiar_fio_morto.py:159–164, app/chamados/juizes.py:1224, app/chamados/juizes.py:563)

   **Ordem de trava / pouso antes do pré-requisito:** o contexto novo apenas instala/restaura memo; as dependências declaradas do cron continuam sendo cartório e reconciliação. Cumprimento operacional, suite verde, revisão final e pouso: [não verificado]. (app/chamados/juizes.py:989–998, app/config/crons.py:1680–1682, CLAUDE.md:370–373)

## 05/11 15:11 · sessao 6847d9683659 · LASTRO-PARTE-2 · sonda · origem=Code

A sonda comprova duas recusas por pergunta viva e uma sequência de fechamento/reabertura. Ela não identifica o chamador externo dessas operações nem mostra a nota “a batida EXISTE”; essas atribuições ficam **[nao verificado]**. Fontes: sonda, linhas `LOG 888606`, `LOG 895771`, `LOG 792643` a `LOG 792831` e os três blocos `ocorrencias/notas:`; escritores em [models.py:275](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:275) e [lastro.py:345](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/lastro.py:345).

Uso abaixo **S35623**, **S33126** e **S36045** para identificar os respectivos blocos da sonda, citando suas linhas literais.

**#35623.** O escritor persistente do estado atual foi `_gravar_estado()`: atribui `status_local`, registra `estado_chamado` e salva. Na cópia, a assinatura `resolvido → em_analise`, com `via=''`, corresponde ao caminho `reabrir() → _gravar_estado()`. Sustentação: **S35623**, `LOG 792831 ... resolvido ... → ... em_analise`; [models.py:222](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:222), [models.py:275](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:275), [models.py:1380](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:1380). Quem pediu essa reabertura e por qual evento concreto: **[nao verificado]**.

Os fechamentos anteriores, `LOG 792643` e `LOG 792671`, com `via='sistema'`, correspondem a `reconciliar() → _gravar_estado()`. Entre eles, `LOG 792648` registra outra reabertura. Não há tentativa recusada documentada nesse chamado: **S35623**, `trilha fechamento_recusado n= 0 []`. Hoje, o recorte mostra premissa permitindo, motor permitindo e duas perguntas reatáveis para #35794; o código admite fechar após reapontar todas as vivas. Isso é um plano favorável, **não uma medição completa do fechamento**: a própria sonda diz `reconciliaria NAO MEDIDO ... InternalError`. Fontes: [models.py:617](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:617), [motor.py:95](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/catalogo/motor.py:95), [models.py:357](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:357), [models.py:415](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:415).

**Os marcos cobrados não acenderam:** 11:00/S e 12:00/E estão apagados. As luzes 07:00/E e 19:00/S não satisfazem essa cobrança; `marcos_acesos()` exige todos os pares hora/tipo cobrados. Portanto, nesse retrato, não havia fecho devido por marco aceso. Sustentação: **S35623**, `contexto marcos [('11:00', 'S'), ('12:00', 'E')]`, `lampadas ... False ... False ...` e `marcos_acesos(ch,cel) False`; [lastro.py:170](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/lastro.py:170).

As perguntas 51561/53192 permanecem vivas porque não têm carimbo e a célula acusa. A 51720 está morta pelo ramo anterior “carimbo + próprio marco aceso”, embora `_pergunta_materializada_de_fato=False`: essa última função não chega a decidir o resultado de `pergunta_viva()` nesse caso. Sustentação: **S35623**, linhas das três perguntas; [juizes.py:1119](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/juizes.py:1119), [juizes.py:1123](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/juizes.py:1123).

**#33126.** O estado atual foi escrito pelo caminho `reconciliar() → _gravar_estado()`, sustentado por **S33126**, `LOG 890440 ... aberto → ... resolvido`, `via='sistema'`; [models.py:620](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:620). O chamador externo — cartório, consumidor, varredura ou outro — é **[nao verificado]**.

A tentativa anterior foi recusada por `_gate_pergunta_viva()`, que registrou `pergunta_viva_na_competencia_aberta` para 46857/46858/46859. Esse ramo recusa quando não consegue reatar todas as vivas a uma cobrança receptora válida. Sustentação: **S33126**, `LOG 888606 fechamento_recusado ... perguntas_vivas: [46857, 46858, 46859]`; [models.py:369](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:369), [models.py:405](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:405). Quem iniciou a tentativa: **[nao verificado]**.

**O marco E07:00 está aceso.** O caminho previsto no cartório é reconhecer o marco satisfeito e passar por `reconciliar()`; havendo fio, a ordem implementada é **lavrar → E2 → reconciliar**. A persistência de ata/veredito também emite `EventoCelula`; seu consumidor tenta retratar chamados absolvidos pela célula, caminho compatível com o `concorde` atual. Sustentação: **S33126**, `marcos_acesos(ch,cel) True`, `veredito concorde`, `marcos_pendentes []`; [cartorio.py:1482](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/cartorio.py:1482), [cartorio.py:953](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/cartorio.py:953), [celula.py:642](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/portas/celula.py:642), [reconciliador.py:1326](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/reconciliador.py:1326).

Hoje, as quatro perguntas estão mortas, com via `celula_absolveu`. **Não é possível explicar a recusa histórica apenas por falta de carimbo:** no código atual, `concorde` sem pendentes já mata a pergunta sem exigir `materializada_em`. O veredito/ata existentes no instante de `LOG 888606` são **[nao verificado]**. Sustentação: **S33126**, perguntas 46856–46859; [juizes.py:1136](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/juizes.py:1136). O `decidir False None` atual é ausência de transição `RECONCILIAR` para quem já está `resolvido`, não nova recusa por pergunta viva; [motor.py:111](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/catalogo/motor.py:111), [motor.py:161](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/catalogo/motor.py:161).

**#36045.** O estado atual também veio de `reconciliar() → _gravar_estado()`: **S36045**, `LOG 895879 ... aberto → ... resolvido`, `via='sistema'`; [models.py:620](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:620). Antes, `_gate_pergunta_viva()` recusou a saída por causa da pergunta 52102, sem reata possível: **S36045**, `LOG 895771 ... pergunta_viva_na_competencia_aberta ... [52102]`; [models.py:405](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:405). Chamadores externos das duas operações: **[nao verificado]**.

**O marco S16:00 está aceso**, mas `fato_em_ausencia` continua acusatório. Por isso, a absolvição do dia inteiro não o alcança; o caminho pertinente é o fechamento por marco. O consumidor de `EventoCelula` tenta absolver chamados antes de carimbar órfãs, e não contém uma etapa geral de reconciliação por marco depois desse carimbo. Fontes: **S36045**, `marcos_acesos ... True`, `veredito acusa? True`; [reconciliador.py:995](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/reconciliador.py:995), [cartorio.py:1488](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/cartorio.py:1488), [reconciliador.py:1326](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/reconciliador.py:1326).

A recusa ocorreu às **11:29:35 UTC**; o carimbo `fantasma` aparece às **11:30:10 UTC**, portanto depois. Com carimbo e marco aceso, o C6 mata a pergunta antes de considerar o veredito acusatório. Sem carimbo, esse veredito pode mantê-la viva. Isso explica o mecanismo compatível com a sequência, mas a ata exata na recusa e o evento que executou cada passo são **[nao verificado]**. Sustentação: **S36045**, `trilha fechamento_recusado` e `materializada_em ... via fantasma`; [juizes.py:1119](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/juizes.py:1119). `fantasma` não planta Batida nem altera resposta; [models.py:2666](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/models.py:2666).

Sobre **a nota e a existência de Batida**, nos três casos:

- **35623:** nenhuma nota foi exibida; anexação histórica **[nao verificado]**. As Batidas mostradas são E06:53 e S11:33; a ata mantém apagados os dois marcos cobrados. Sustentação: **S35623**, linhas `batida 140656`, `batida 140657` e `lampadas`; [lastro.py:174](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/lastro.py:174).
- **33126:** nenhuma nota foi exibida; anexação histórica **[nao verificado]**. As Batidas listadas são de 20/10 e 22/10; nenhuma de E07:00 em 21/10 foi mostrada. A lâmpada está acesa com `luz=None`. Fonte: **S33126**, linhas `lampadas`, `batida 132522` e `batida 134123`.
- **36045:** nenhuma nota foi exibida; anexação histórica **[nao verificado]**. Há S13:40:53 em 30/10, mas nenhuma S16:00 desse dia foi mostrada; a lâmpada final está acesa com `luz=None`. Se S13:40 foi associada ao marco final: **[nao verificado]**. Fonte: **S36045**, linhas `lampadas` e `batida 141783`.

O código não autoriza converter automaticamente “acesa” em “Batida existente”: `lastro.julgar()` aceita a lâmpada e substitui `luz` vazia pela hora prevista; `lastro.fechar()` anexa a frase após `reconciliar()` retornar verdadeiro. Ausência aprovada/justificativa também pode acender lâmpada sem hora real. **Abono, edição ou troca de escala como causa concreta desses três retratos: [nao verificado].** Fontes: [lastro.py:289](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/lastro.py:289), [lastro.py:345](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/lastro.py:345), [cartorio.py:766](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/services/cartorio.py:766).

Os escritores e leitores decisivos são os mesmos nos três chamados:

| Campo | Quem escreve | Quem lê para decidir o fecho |
|---|---|---|
| `status_local` | `_gravar_estado()`, `app/chamados/models.py:222,275-279`; portas identificadas acima pelos LOGs | `_decisao_do_fechamento()`, `models.py:638-655`; motor, `app/chamados/catalogo/motor.py:111` |
| `ata.lampadas` | `lampadas_do_dia()`/`ata_do_dia()`, `app/ponto/services/cartorio.py:647,677,704`; dispensa, `:774-776`; persistência em `app/ponto/portas/celula.py:775` | `marcos_acesos()`, `app/ponto/services/lastro.py:170-181`; premissa, `app/chamados/models.py:1236-1239`; C6, `app/chamados/juizes.py:1119-1138` |
| `validada_em` | Ato de validar: `app/chamados/services/validacao.py:127-133`; simetria com resposta textual: `app/chamados/models.py:2576-2583`; ressuscitação limpa em `:1493` | Seleção das ancoradas, `models.py:327`; C6, `app/chamados/juizes.py:1078-1079` |
| `materializada_em` | `_escrever_carimbo()` e `carimbar()`, `app/chamados/models.py:2545-2549,2598`; `celula_absolveu`, `app/chamados/reconciliador.py:620`; `fantasma`, `:1115`; limpeza, `models.py:2652-2653` | C6, `app/chamados/juizes.py:1119-1126`; teste de materialização efetiva, `:645-669` |

`validada_em=None` aparece em todas as perguntas fornecidas: a resolução mostrada não comprova validação humana. O próprio código distingue escritor único **do ato de validar** de escritor único **do campo**; [validacao.py:19](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/chamados/services/validacao.py:19). Na sonda, #33126 usa `celula_absolveu`; 51720/#35623 e 52102/#36045 usam `fantasma`.

Quanto a **julgar passado pelo cadastro de hoje**, a seleção do vínculo é para a data do fato, e os marcos esperados podem vir do DNA congelado. Contudo, `classificar_falta()` contém leituras do template vivo: `_turno_noturno_completo()` lê `hora_inicio/hora_fim`, e `fim_turno(ec.tipo_escala, data_evento)` é chamado sem `ec`, recaindo no template. Fontes: [servico_jornada.py:55](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/escala/servico_jornada.py:55), [models.py:1124](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/escala/models.py:1124), [servico_jornada.py:237](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/escala/servico_jornada.py:237), [servico_jornada.py:375](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/escala/servico_jornada.py:375), [servico_jornada.py:68](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/escala/servico_jornada.py:68). **Essas leituras terem determinado algum dos três estados históricos, ou uma regra atual ter substituído a histórica: [nao verificado].** As linhas `classificar_falta (...)` da sonda não informam os ramos percorridos.

Também não há insumos suficientes para escolher rigorosamente o **dono L-099**: o juiz considera falta de vínculo/folga, batida fora de turno, paridade, fase e cadastro versus realidade. A sonda não entrega esse conjunto; atribuir ESTRUTURA, CADASTRO ou BATIDA apenas pelo status seria extrapolar. Fontes: [CLAUDE.md:176](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/CLAUDE.md:176), [e6_oraculo.py:98](/home/ronald/codex_ws/a026943099b8d88468240fd4b18491141b6810af/app/ponto/management/commands/e6_oraculo.py:98).

| chamado | caminho | marco aceso? | nota batida (sim/nao, procede?) | dono L-099 (ESTRUTURA/CADASTRO/BATIDA) |
|---|---|---|---|---|
| 35623 | Reconciliado e reaberto; atual por `reabrir → _gravar_estado` — S35623, LOG 792831; `models.py:1382` | Não: 11S/12E apagados — S35623, `marcos_acesos False`; `lastro.py:174-180` | Não exibida; sim/não e procedência histórica **[nao verificado]** | **[nao verificado]** |
| 33126 | Recusa por perguntas vivas, depois `reconciliar → _gravar_estado` — S33126, LOG 888606/890440; `models.py:405,620` | Sim: E07 — S33126, `marcos_acesos True`; `lastro.py:181` | Não exibida; anexação **[nao verificado]**; Batida no marco não demonstrada | **[nao verificado]** |
| 36045 | Recusa por 52102; carimbo fantasma posterior; resolvido por `reconciliar → _gravar_estado` — S36045, LOG 895771/895879; `models.py:620,2674` | Sim: S16 — S36045, `marcos_acesos True`; `lastro.py:181` | Não exibida; anexação **[nao verificado]**; Batida associada ao marco não demonstrada | **[nao verificado]** |

## 05/11 15:11 · sessao 5decd52066cb · - · - · origem=Ronald (tmux)

Achados só de leitura; os testes abaixo foram lidos, não executados. [não verificado] [nao verificado]

**Regra de porta e causas.**

- `porta_alcancavel_do_dia` aceita chamado **da célula** na fila, ou pergunta viva da célula, no app, com âncora na fila e sem resposta vazia carimbada; retorna `None` se nenhuma dessas vias existir. `app/chamados/juizes.py:1314-1329`.
- A fila exige `status_local ∈ COBRANCA` e exclui `silenciado_ate > agora`; `registrado` está fora dela. `app/chamados/services/painel.py:174-188`.
- A vitalidade da pergunta **não consulta o estado do chamado**: consulta validação, tranca, resposta, via/carimbo, veredito/ata, marco e lei do dia. Por isso pergunta viva e chamado terminal podem coexistir; isso não torna a âncora uma porta. `app/chamados/juizes.py:1077-1160`, `app/chamados/juizes.py:1321-1325`.
- Causas são **diagnósticos da escada de leitura**, não prova do escritor histórico: `a` véu vivo; depois `b` resposta vazia; depois `c` respondida sob terminal; depois `d` sem pergunta, dia fora e pessoa na lista; depois detalhes `e`. `app/chamados/services/vigia_fio.py:660-698`.

| Classe | Condição comprovável / porta esperada | Contraexemplo de código |
|---|---|---|
| a | Chamado vivo fora da fila; no anexo detalhe é `registrado`. Porta esperada é chamado acionável da célula na fila, ou pergunta sob outra âncora na fila; mero vivo não basta. `vigia_fio.py:663-669`; `painel.py:174-188`; `juizes.py:1318-1323` | Aberto sem silêncio da célula vira `porta_fila`; registrado vira `a`. `app/chamados/tests/test_o261e_f5_alcance.py:134-159` |
| b | Pergunta viva com `respondida_em` e texto vazio é recusada explicitamente. A via seria pergunta corrigida no app, sob âncora na fila; não presumir resposta. `juizes.py:1191-1199`, `juizes.py:1322` | Texto vazio dá `b`; pergunta sem resposta sob aberto na fila é porta. `test_o261e_f5_alcance.py:141-147`, `:161-167` |
| c | Pergunta respondida com âncora terminal perde a fila; via esperada de validação, com âncora acionável. A classe não prova qual ato terminal ocorreu. `vigia_fio.py:674-675`; `juizes.py:1321-1325` | Respondida sob resolvido dá `c`; aberto ancorando pergunta dá porta. `test_o261e_f5_alcance.py:141-147`, `:169-174` |
| e/terminal | Pergunta viva, âncora terminal, sem porta anterior ou balde legal; via seria questionário sob âncora na fila. `vigia_fio.py:651-659`, `:686-687`; `juizes.py:1321-1325` | Sem resposta sob resolvido dá `e/terminal`; sob aberto dá porta. `test_o261e_f5_alcance.py:141-147`, `:176-182` |
| d ou e/sem pergunta | O nome não identifica o emissor que falhou ou por que não nasceu pergunta/chamado; isso requer insumos não presentes no dado. Porta possível é fila da célula; lista só exclui com o **dia** e dono BATIDA/CADASTRO. `vigia_fio.py:676-685`, `:221-227`; `juizes.py:1318-1325` | Dia na lista BATIDA sai para fio do admin; ESTRUTURA permanece e nem é classe d. `test_o261e_f5_alcance.py:194-203` |

**Campos: escritores e leitores diretos.** Prefixos abaixo são `app/`.

| Campo/insumo | Quem escreve | Quem lê para porta |
|---|---|---|
| Chamado `status_local` | Outro agente cobre o gate/terminais. | `chamados/services/painel.py:155`, `:186-188`; causas em `chamados/services/vigia_fio.py:606-617`, `:661-687` |
| Chamado `silenciado_ate` | `arquivo.silenciar`: atribui e salva; `arquivo.levantar`: limpa e salva. `chamados/services/arquivo.py:160-171`, `:198-208` | `chamados/services/painel.py:186-188`; verbo também muda por relógio em `chamados/services/verbos.py:44-52` |
| Chamado `celula_id` | Gate de nascimento usa célula em `chamados/models.py:1073`, `:1102`. | Lote indexa a fila por célula em `chamados/juizes.py:1232-1237`; porta consulta em `:1318-1319` |
| Pergunta `celula_id`, `data_evento`, `motivo`, `disputa_id`, `chamado_destino_id` | `PerguntaDisputa.abrir`: procura célula, reaponta existente e cria nova em `chamados/models.py:2387-2466`; outro reapontamento em `chamados/services/fio.py:773`, `:845`. | Perguntas da célula em `chamados/juizes.py:1242-1245`; âncora é destino ou chamado da disputa em `chamados/services/fio.py:116-117`; app consulta pessoa/disputa em `chamados/services/pergunta_no_app.py:42-53` |
| `resposta_colab`, `respondida_em` | Resposta humana valida não vazio e grava o par em `chamados/services/disputa_emissao.py:1254-1256`, `:1282-1301`; automática em `:1417-1430`; reparo de carimbo com texto em `:1434-1446`; reabertura limpa par e validação em `:1393-1413`. `PerguntaDisputa.carimbar` só põe carimbo de resposta com texto em `chamados/models.py:2576-2583`. | Vitalidade em `chamados/juizes.py:1092-1104`; resposta vazia em `:1196-1199`; veto de porta em `:1322`; app usa pré-filtro `q_vivas`, `chamados/models.py:2735-2748` |
| `validada_em` | Validação canônica em `chamados/services/validacao.py:127-133`; `carimbar(resposta='validar')` em `chamados/models.py:2576-2583`; resposta nova desfaz em `disputa_emissao.py:1295-1297`; reabertura em `:1393-1413`. | Mata a pergunta no primeiro degrau: `chamados/juizes.py:1077-1079`; pré-filtro app `chamados/models.py:2746` |
| `materializada_em`, `via_resolucao` | Escritor canônico `_escrever_carimbo`, chamado por `carimbar`, `carimbar_identidade` e `descarimbar`: `chamados/models.py:2545-2549`, `:2575`, `:2625-2637`; lote em `:2644-2653`. | Tranca e vias de identidade, marco, existência de carimbo e lastro em `chamados/juizes.py:1087-1088`, `:1112-1126`, `:1149-1156` |
| Célula `veredito`, `veredito_via`, `ata` | Porta da célula escreve em `ponto/portas/celula.py:735-783`; cartório fornece ata/lavra em `ponto/services/cartorio.py:1800-1807`; admin chama via admin em `ponto/services/veredito_celula.py:330`. | Vitalidade lê decisão admin e acusação em `chamados/juizes.py:1089-1091`, `:1123-1138`; marco lê ata/lâmpadas em `:908-914` |
| Célula `dna.marcos` | Geração/regeneração em `ponto/portas/celula.py:251-261`, `:611-622`. | Juiz do marco em `chamados/juizes.py:891-905`; confronto resposta/luz em `:1047-1054` |
| Tranca `PeriodoFechado.reaberto_em`, `fechado_em`, empresa/competência | `trancar_periodo` cria/retranca e limpa reabertura em `ponto/services/fechamento.py:1391-1403`; `reabrir_periodo` grava reabertura em `:1420-1425`. | `periodo_trancado` usa **reaberto_em**, não flag `ativo`, em `:1448-1454`; pergunta consulta janela e toque admin em `chamados/juizes.py:929-949` |
| Toque humano posterior à tranca | Atos são cobrar dia, resolver dia e desarquivar; ações nomeadas em `chamados/juizes.py:952-958`. Escritores específicos da trilha não rastreados nesta exploração. [não verificado] | `dia_da_pergunta_trancado` chama `_admin_tocou_o_dia_depois`, `chamados/juizes.py:949`, `:996` |

**Balde Resolver dia não é porta do corte.** Ele exige último recurso, passado e status em falta/irregular/aberto/sem_certidão; último recurso exige data anterior à competência atual e ausência de pergunta viva. `app/colaboradores/services/calendario.py:94-104`, `:738-739`. O calendário monta o campo e o censo o lê em lote; o balde é aplicado antes das causas. `app/colaboradores/services/calendario.py:132-136`, `:766-769`; `app/chamados/services/vigia_fio.py:634-659`. A permissão `regularizar_batida` é do usuário, fica fora do predicado do dia e controla a tela. `app/colaboradores/services/calendario.py:100-102`; `app/colaboradores/services/drawer.py:130`, `:154`; `app/colaboradores/views.py:2957`.

## 05/11 15:10 · sessao 3809f74c0814 · - · - · origem=Ronald (tmux)

Censo da minha partição, somente por leitura estática. Os contraexemplos abaixo são construídos; ocorrência e quantidade em produção são [nao verificado].

**Achados históricos comprovados no código**

Os exemplos podem ser reutilizados por grupo:

- **V**: D=01/10, vínculo A 07h–19h vigente em D; hoje A encerrado e vínculo B 19h–07h ativo desde 01/11. Perguntar A/B pelo flag atual devolve B para outubro. Juízes existentes: `vinculo_do_dia` e marcos congelados da célula; registro em `app/docs/JUIZES.md:22` e `app/docs/JUIZES.md:98`.
- **S**: D=01/10, pessoa ativa, célula com furo ou HE; hoje desligada em 01/11. Filtrar `situacao='ativo'` apaga sua participação no relatório de outubro. Escrita real desse cenário em `app/colaboradores/services/desligamento.py:104`.
- **E**: D=01/10, pessoa na empresa A, cujo corte é 21; hoje transferida para B, corte 1. Empresa de hoje pode trocar a competência, dono e tranca do fato passado. Transferência efetivamente altera `Colaborador.empresa` em `app/colaboradores/views.py:1386`; cortes mutáveis em `app/colaboradores/models.py:46`.
- **I**: D=01/10, pessoa sujeita a ponto, sem batidas/furo; isenção ligada em 01/11. A flag atual cala o furo de outubro. A troca da flag existe em `app/colaboradores/views.py:176`; juiz atual não recebe data em `app/core/constants.py:13`. Juiz histórico de isenção registrado em JUIZES: [nao verificado]; a lista não apresenta essa autoridade (`app/docs/JUIZES.md:20`).

| Sítio | Campo e decisão passada | Classificação e contraexemplo | Juiz existente |
|---|---|---|---|
| `app/chamados/juizes.py:1470` | `EscalaColaborador.ativa`; escolhe vínculo para `perg.data_evento` (`:1465`) | BUG V; `:1474` ainda chama template, ignorando célula | `vinculo_do_dia` + `EscalaColaborador.marcos_do_dia` |
| `app/chamados/services/desvio_escala.py:44` | vínculo ativo atual; escolhe um conjunto de marcos dos últimos 14 dias (`:50`) e o aplica às batidas de 84 dias (`:58`, `:64`) | BUG V: 70 dias de escala diurna anterior podem ser denunciados como desvio do novo template noturno; diagnóstico resultante `:88` | vínculo e marcos de cada dia |
| `app/colaboradores/views.py:2203` | vínculo ativo; navegação semanal passada por `?semana=` (`:2188`, `:2210`) | BUG V, sobretudo sem célula: `eh_dia_trabalho(d)` em `:2223` e horários em `:2233` do vínculo B para outubro | `vinculo_do_dia`, `eh_dia_trabalho`, marcos da célula |
| `app/relatorios/views.py:455` | situação atual; universo do extrato de mês/ano pedido (`:447`) | BUG S: pessoa some e totais caem | juiz histórico de situação [nao verificado] |
| `app/relatorios/views.py:459` | empresa atual; universo do mesmo extrato | BUG E: trabalho de outubro muda de empresa | juiz histórico de empresa [nao verificado] |
| `app/relatorios/views.py:461` | posto/praca atuais; universo do extrato | BUG E, adaptado a transferência de praça | `vinculo_do_dia` para posto da escala; pertencimento cadastral histórico [nao verificado] |
| `app/relatorios/views.py:492` | vínculo ativo e template; pareamento das batidas do mês em `:511`, conta turnos abertos em `:514` | BUG V: a rubrica vem de DiaPago, mas a geometria ainda pode mudar com template atual | `turnos_do_colab`/juiz dos turnos + vínculo do dia |
| `app/relatorios/dia_da_frota.py:34` | situação atual; “quem faltou no dia X” (`:1`) | BUG S: furo lavrado desaparece quando pessoa sai | juiz histórico de situação [nao verificado] |
| `app/relatorios/dia_da_frota.py:37` | empresa atual; filtro de empresa no dia X | BUG E: furo de A aparece em B | juiz histórico de empresa [nao verificado] |
| `app/relatorios/dia_da_frota.py:64` | situação/empresa atuais (`:66`); “sem célula” no dia X (`:67`) | BUG S/E; pessoa admitida só em novembro pode aparecer “sem célula” em outubro; desligado deixa denominador | juiz histórico de situação/empresa [nao verificado] |
| `app/relatorios/services.py:105` | situação/empresa atuais; seleciona caderno de espelhos da competência | BUG S/E quando `colabs=None`: `gerar_pdf_espelho_lote_bytes` usa esse universo em `:217`, competência em `:214` | juiz histórico do universo [nao verificado] |
| `app/relatorios/cartao_pela_celula.py:418` | isenção atual; elimina furos das `datas_em_aberto` de todo período | BUG I | juiz de isenção por D [nao verificado]; atual `isento_ponto` não resolve data |
| `app/relatorios/cartao_pela_celula.py:24` | empresa atual; janela da competência | BUG E | `janela_fechamento` existe, mas empresa vigente em D [nao verificado] |
| `app/relatorios/cartao_pela_celula.py:32` | empresa atual; decide se intervalo corresponde a competência histórica | BUG E | `janela_atual` com insumo histórico; juiz da empresa histórica [nao verificado] |
| `app/relatorios/cartao_pela_celula.py:92` | empresa atual; `datas_falta` do fechamento pedido | BUG E | juiz da empresa histórica [nao verificado] |
| `app/relatorios/cartao_pela_celula.py:167` | empresa atual; outra leitura de `datas_falta` | BUG E | idem |
| `app/ponto/services/he_pendente_lavrado.py:92` | situação/empresa atuais; universo do retrato mes/ano (`:71`, `:89`) | BUG S/E: HE pendente histórica some após desligamento/transferência | juiz histórico do universo [nao verificado] |
| `app/ponto/management/commands/reconciliar_grade.py:106` | situação/empresa atuais; janela pode ser passada via `--ref` (`:78`) | BUG S/E: deixa fora grade histórica da pessoa desligada | juiz histórico de situação/empresa [nao verificado] |
| `app/ponto/management/commands/reconciliar_grade.py:85` | empresa atual atravessando colaborador de batida histórica | BUG E; universo `batted` pertence à empresa de hoje | juiz histórico de empresa [nao verificado] |
| `app/ponto/management/commands/reconciliar_grade.py:97` | empresa atual atravessando chamado | BUG E; universo `com_chamado` segue CNPJ atual | idem |
| `app/ponto/management/commands/emitir_furo_retroativo.py:52` | situação/empresa atuais; --ini/--fim (`:40`) | BUG S/E | juiz histórico do universo [nao verificado] |
| `app/ponto/management/commands/emitir_furo_retroativo.py:106` | isento atual; `classificar_dia(data=data)` (`:101`) | BUG I | juiz histórico isenção [nao verificado] |
| `app/ponto/management/commands/supra_juiz.py:110` | situação/empresa atuais; janela explicitável ou atual (`:95`) | BUG S/E para janela passada | juiz histórico do universo [nao verificado] |
| `app/ponto/management/commands/supra_juiz.py:228` | isento atual; classifica dias da grade | BUG I | juiz histórico isenção [nao verificado] |
| `app/ponto/management/commands/mapa_divergencia.py:65` | isento atual; exclui antes de comparar competência (`:61`) | BUG I para auditoria histórica; pode produzir concordância removendo quem mudou | juiz histórico isenção [nao verificado] |
| `app/ponto/management/commands/detectar_ausencias.py:104` | isento atual; cala antes do laço hoje+ontem (`:112`) | BUG I no dia de ontem anterior à mudança da flag | juiz histórico isenção [nao verificado] |
| `app/ponto/management/commands/processar_alertas_turno.py:85` | isento atual; cala turnos passados materializados (`:78`) | BUG I | juiz histórico isenção [nao verificado] |
| `app/ponto/management/commands/processar_alertas_avancados.py:32` | isento atual; todo laço julga ontem (`:34`, `:37`) | BUG I | juiz histórico isenção [nao verificado] |
| `app/ponto/services/vigia_de_hora.py:117` | situação/isento atuais; candidatos incluem ontem (`:115`) | BUG S/I na madrugada após mudança cadastral | juiz histórico situação/isencao [nao verificado] |
| `app/chamados/juizes.py:938` | empresa atual; resolve competência de `data_evento` (`:929`, `:939`) e tranca (`:942`) | BUG E: A trancada/B aberta; pergunta de A revive após transferência B | `janela_atual`, `periodo_trancado`; empresa vigente em D [nao verificado] |
| `app/chamados/reconciliador.py:713` | empresa atual; cerca de competência entregue do dia passado | BUG E: A entregue/B não entregue muda alcance da reconciliação | `competencia_do_dia_entregue`; empresa histórica [nao verificado] |
| `app/chamados/reconciliador.py:809` | empresa atual; horizonte de emissão do dia | BUG E se cortes/competências diferentes | `emissao_no_horizonte`; empresa histórica [nao verificado] |
| `app/chamados/services/fabrica_da_celula.py:188` | empresa atual; limbo de dia passado vai a pauta ou pergunta | BUG E: A entregue/B aberta muda destino | `competencia_do_dia_entregue`; empresa histórica [nao verificado] |
| `app/chamados/services/passivo_validado.py:342` | empresa atual; agrupa plantio histórico por competência | BUG E | `janela_atual`; empresa histórica [nao verificado] |
| `app/chamados/services/passivo_validado.py:355` | empresa atual; identifica exportada e ramo do passivo | BUG E | `competencia_entregue`; empresa histórica [nao verificado] |
| `app/chamados/services/vigia_fio.py:815` | empresa atual; classifica pergunta passada como aberta/exportada | BUG E | `competencia_do_dia_entregue`; empresa histórica [nao verificado] |
| `app/chamados/models.py:347` | empresa atual; pergunta histórica entregue ou não | BUG E | mesma composição e mesma lacuna |
| `app/chamados/services/fio.py:769` | empresa atual para questão da pergunta | **Inspecionar fluxo antes de afirmar efeito específico** [nao verificado] | — |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:86` | empresa atual; competência de `p.data_evento` | BUG E; consulta exportada em `:95` | janela + fechamento; empresa histórica [nao verificado] |

**Leituras que exigem distinguir pergunta histórica de ato atual**

- `app/ponto/management/commands/reconciliar_fantasmas.py:173` lê vínculo atual, mas hoje só escreve o diagnóstico. O comentário em `:198` declara isso e o fechamento real pergunta ao núcleo em `:244`–`:247`. Não relatar como BUG de fechamento; no máximo diagnóstico histórico errado V (`:204`).
- `app/relatorios/management/commands/diff_extrato_parcial.py:105` usa vínculo atual no lado **ANTES**, reproduzido expressamente para medir o defeito antigo (`:2`–`:12`). INTENCIONAL como instrumento de impacto, conforme L-110 (`app/docs/LEIS.md:136`), não é resposta de produção ao “como era D”. Seu universo atual `:86` deve ser descrito como recorte da medição, e não evidência do histórico inteiro.
- `app/ponto/management/commands/fechar_chamados_meio_periodo.py:60` e `:76`–`:79` leem template **hoje** para limpar passivo causado por cadastro corrigido. A seção CRITÉRIO explicitamente exige “template HOJE já declara” em `:12`–`:20`. É intenção de **correção do passado errado**, não de reconstrução do cadastro anterior; lei geral da exceção L-020 (`app/docs/LEIS.md:71`). Porém escolher **vínculo ativo** de outra escala para encerrar chamado do vínculo antigo continua sujeito ao contraexemplo V; a correção de B não prova erro de A.
- `app/ponto/services/furos_vetados.py:62` exclui desligado de **cobrança atual**, com corte e assimetria deliberada registrados na seção `:48`–`:54`. `situacao` ali é INTENCIONAL para quem cobrar agora; empresa atual em `:61` pode continuar atribuindo histórico ao CNPJ errado.
- `app/ponto/services/furos_sem_cobranca.py:62` seleciona ativos, mas o contrato declara universo da emissão atual em `:3`–`:7` e a suspensão atual por afastamento em `:17`–`:26`. Não confundir esse vigia de cobrança atual com contagem histórica de faltas.
- `app/chamados/services/silencio_isentos.py:24` silencia **acervo vivo atual** pelo mandato de 06/08 transcrito em `:3`–`:6`; porém os cortes posteriores por período substituem esse entendimento (`app/docs/CORTES.md:348`, `:352`). Portanto não usar o docstring antigo como autorização vigente incontestável: a obra está registrada/em construção nos próprios cortes.
- `app/ponto/services/vinculo_na_lista.py:70` exclui desligado de diagnóstico de **cadastro atual**, e seção `:67`–`:69` diz explicitamente que situação vale hoje; `colabs_em_revisao` compara lavra anterior com cadastro atual de propósito para reconhecer correção (`:139`–`:153`).
- `app/colaboradores/services/calendario.py:258` aparece ABERTO no selo, mas atualmente o vínculo ativo só alimenta `folgas_foto` quando cobre **as duas pontas** por `vinculo_cobre_a_data` em `:505`–`:509`; as geradoras são passadas à autoridade em `:510`; status vem do D4 (`:41`–`:47`). Não é correto afirmar que o calendário inteiro ainda julga todos os dias pelo ativo.
- `app/ponto/services/abriu_nao_bateu.py:194` usa ativa, mas o chamador real passa `hoje` (`:140`); por default é detector do agora. Tratar como sítio histórico apenas porque função aceita data de teste seria extrapolação.
- `app/ponto/utils.py:170`, `:172`, `:180` selecionam gente para **cobertura** futura/ato presente, conforme contrato `:141`–`:160`; sem prova de chamador para passado não declarar BUG histórico.

**Escritores dos campos**

| Campo | Onde é definido e quem decide/escreve |
|---|---|
| `Colaborador.isento_ponto` | Campo `app/colaboradores/models.py:294`; UI `app/colaboradores/views.py:176`, trilha `:183`–`:186`; porta física `app/colaboradores/services/colaborador.py:55`–`:61` |
| `Colaborador.situacao` | Campo `app/colaboradores/models.py:323`; criação `app/colaboradores/views.py:1205`; desligamento `app/colaboradores/services/desligamento.py:104`; reativação `:210`; afastamento `app/ponto/views.py:2425`; reversão `app/ponto/management/commands/reverter_situacao_afastado.py:73` |
| `Colaborador.data_demissao` | Campo `app/colaboradores/models.py:325`; agendamento `app/colaboradores/services/desligamento.py:86`; desligamento `:105`; reativação apaga em `:211`. Logo sobrescrita/perda dessa data é suportada por código, não hipótese sem escritor |
| `Colaborador.empresa` | Campo `app/colaboradores/models.py:321`; edição direta `app/colaboradores/views.py:1313`, salva em `:1342`; transferência `:1386`, salva `:1388`; porta operação posto declarada em `app/colaboradores/services/colaborador.py:33` |
| `Colaborador.posto` | Campo `app/colaboradores/models.py:322`; transferência `app/colaboradores/views.py:1385`, salva `:1388` |
| Campos de Empresa | Modelo corte `app/colaboradores/models.py:46`; janela HE `:58`, `:69`, `:101`; escritor `app/colaboradores/services/estrutura.py:17`; form do corte `app/colaboradores/views.py:496`, `:523`, `:538` |

A porta `salvar_inteiro` apenas confere a operação, sem validar nomes de campos, depois chama `save()` completo (`app/colaboradores/services/colaborador.py:73`–`:76`). Assim, não concluir “empresa só pode ser escrita na operação posto”: a edição cadastral já a escreve por inteiro (`app/colaboradores/views.py:1313`, `:1342`).

Os leitores históricos aparecem na tabela; leitores de hoje incluem `colaboradores_comuns` (`app/colaboradores/queries.py:25`–`:28`), operação corrente (`:62`–`:65`), Passe livre (`app/colaboradores/views.py:204`), juiz da flag (`app/core/constants.py:18`), eventos que silenciam acervo (`app/chamados/signals.py:215`, `:223`) e cadastro corrente (`app/colaboradores/views.py:33`).

**Selos e buracos**

- `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:216` mantém ABERTO com dívida nominal. A doc define que só sítio novo fora das listas é vermelho (`:12`–`:15`); logo não impede os bugs já permitidos. Só escaneia `EscalaColaborador.ativa`, não `Colaborador.situacao/empresa/isento` (`:5`, `:17`–`:23`).
- Mesmo selo: HOJE permite gates de universo em `:170`–`:175` e diagnóstico corrente em `:54`–`:61`; ABERTO envelheceu para `calendario` e `reconciliar_fantasmas`, como descrito acima. Lista não equivale a classificação jurídica.
- `app/ponto/tests/test_c3_crons_perguntam_a_juiza.py:17` lista só sete arquivos; seu detector procura keyword `ativa=True` no modelo (`:44`–`:59`), e não isenção ou situação no topo desses crons. Assim migração do vínculo fica verde enquanto isento atual ainda cala ontem (`app/ponto/management/commands/processar_alertas_avancados.py:32`).
- `app/relatorios/tests/test_dia_da_frota.py:30` cria todos ativos; os testes de contagem `:46`–`:63` não mudam situação/empresa depois de lavrar D. A classe seguinte cobre geradora fora da vigência (`:78`), outra dimensão.
- `app/colaboradores/tests/test_passe_livre_trilha.py:64` e `:96` provam gravação e ida/volta da flag; não perguntam o resultado de um D anterior após a troca. A própria explicação histórica distingue chamados predatando flag (`:11`–`:12`).
- `app/chamados/tests/test_silenciar_isentos.py:7` só inspeciona fonte; exige nomes `isento_ponto`, `--apply`, `silenciado_ate` e ausência de atribuição crua de status (`:10`–`:13`). Não prova vigência por dia nem efeito em cálculos.
- Paridade entre leitores pode permanecer verde quando todos recebem a mesma flag atual; exemplo explícito de leitores simultaneamente calados já consta em `app/colaboradores/services/calendario.py:540`–`:544`.

## 05/11 15:10 · sessao 77a7c1337052 · - · - · origem=Ronald (tmux)

Item 3: **o código explica ESTRUTURA como resíduo; o anexo não prova erro de leitura do juiz nem a correção semântica dos 185 julgamentos.** O juiz não consulta a lista CxR da tela: `InsumosDoDono.cxr()` lê exclusivamente os dias de `resultado.dias_cadastro_x_realidade` do motor; `dono()` agrega batidas, fase lavrada e cadastro do dia. (`app/ponto/management/commands/e6_oraculo.py:150-178`)

- Precedência: folga indefinida ou ausência de vínculo ⇒ CADASTRO; batida fora do turno ou contagem ímpar ⇒ BATIDA; fase deslocada ⇒ CADASTRO; dia presente na lista do motor ⇒ CADASTRO; restante ⇒ ESTRUTURA. Zero batidas não significa BATIDA nessa função. (`app/ponto/management/commands/e6_oraculo.py:98-110`, `:72-75`)
- A classe d é decidida **sem consultar esse juiz**: nenhuma pergunta viva, colaborador presente na lista da tela e dia ausente de `dias_na_lista`. (`app/chamados/services/vigia_fio.py:660-677`)
- A lista da tela é mais ampla: agrega assinaturas A e C por colaborador, incluindo assinaturas do vínculo inteiro; `dias_na_lista` só admite assinaturas com `data`. Portanto “colaborador na CxR” não implica “este dia pertence ao cadastro”. (`app/escala/services/cadastro_realidade.py:555-685`, `:736-756`)
- Contraexemplo de código: `dono_da_divergencia([], False)` ⇒ ESTRUTURA e `dono_da_divergencia([], True)` ⇒ CADASTRO. Há testes explícitos dos dois resultados; não foram executados nesta exploração. (`app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:143-147`)
- Contraexemplo no dado fornecido: a própria classe d contém 185 ESTRUTURA, 9 CADASTRO e 3 BATIDA; esses números são do anexo textual, sem arquivo local conferido. [não verificado]
- **Certeza alta sobre o mecanismo; julgamento individual dos 185 não verificado**: o anexo não traz `horas`, lista do motor, flags cadastrais, lavra de fase nem assinatura CxR de cada dia. [não verificado]
- A L-099 não autoriza converter todo colaborador listado em CADASTRO; distingue estrutura e dados e aponta esse juiz. Mas a concordância do resultado com os insumos reais dos 185 dias não foi demonstrada pelo anexo. (`CLAUDE.md:176`) [não verificado]

Para d e e/sem_pergunta_viva, **a classe não identifica o escritor histórico**. d significa ausência de perguntas vivas, não ausência de histórico; e/sem_pergunta_viva é o resíduo sem vivas que não satisfez d. (`app/chamados/services/vigia_fio.py:676-685`) Portanto não é possível afirmar “nunca emitiu” ou “matou a pergunta” nesses registros apenas com o anexo. [não verificado]

A porta exigida pelo censo é: chamado da célula na fila, ou pergunta viva no app sob âncora na fila. Acesso manual ao chamado/modal não conta como porta; Resolver dia possui balde próprio. (`app/chamados/juizes.py:1283-1311`, `:1317-1329`) A tela de correção cadastral depende da assinatura: vínculo, wizard, ausência, adesão ou fio; como a assinatura não veio no anexo, escolher uma delas para cada d seria especulação. (`app/escala/services/cadastro_realidade.py:449-510`) [não verificado]

A lista possui rota por dono para C9/C10: BATIDA ⇒ regularizar; CADASTRO ⇒ porta da assinatura; ESTRUTURA ⇒ “com o sistema”, sem URL. (`app/escala/services/cadastro_realidade.py:513-527`) Isso não demonstra que os 185 d sejam C9/C10; d, por definição, está fora da lista por dia. (`app/chamados/services/vigia_fio.py:676-677`)

Por fim, **dono retornado não é autoria do defeito**: a função foi documentada para classificar divergências R1, com ESTRUTURA como resíduo quando os números ainda discordam. (`app/ponto/management/commands/e6_oraculo.py:55-70`) Usá-la como atribuição causal da falta de porta, sem examinar o escritor, excederia o que ela demonstra. [não verificado]

## 05/11 15:10 · sessao ab1f8a299db5 · - · - · origem=Ronald (tmux)

Censo estático; nenhum banco, log ou teste executado. Ocorrência e quantidade em produção: [nao verificado]. [nao verificado]

**Achados fortes de BUG, com contraexemplos hipotéticos concretos**

| Sítio | Campo / decisão histórica | Contraexemplo concreto | Juiz existente |
|---|---|---|---|
| `app/folha/export.py:407` | `Colaborador.isento_ponto` atual zera eventos do fechamento de qualquer competência | Setembro: não isento, HE50=8h; flag ativada em novembro; exportar setembro retorna `[]`. Código chama o snapshot antigo de “era pre-isencao” em `:402-404`. | `core.constants.isento_ponto` é juiz existente, porém apenas lê bool atual (`app/core/constants.py:13-18`); juiz histórico da isenção em JUIZES: [nao verificado]. |
| `app/folha/export.py:1011` | Mesmo flag torna prontidão histórica `cego/isento`, previsto zero | Setembro: controle normal, 160h previstas/120h feitas; novembro flag=True; setembro perde denominador. | `prontidao` está registrado (`app/docs/JUIZES.md:74-76`), mas ele próprio usa hoje. |
| `app/folha/export.py:1174` | Mesmo flag elimina retenções de qualquer `[ini,fim]` antigo | D=10/09, célula `furo/FURO_PARCIAL`, trabalhador não isento; flag novembro=True; retorna nenhuma retenção. | Não há juiz histórico de isenção verificado; `core.constants.isento_ponto` não recebe D (`app/core/constants.py:13-18`). |
| `app/folha/previa.py:74` | Mesmo flag exclui colaborador inteiro da prévia histórica | D=10/09, marco apagado normal; flag novembro=True; dia desaparece antes do laço de células. | Mesmo limite. |
| `app/ponto/services/fechamento.py:357` | Mesmo flag descarta `datas_falta` históricas antes da lavra | D=10/09, falta sem isenção; flag novembro=True; função retorna `[]`. | Isenção histórica: [nao verificado]. Juiz de encerramento temporal existe e é usado depois (`:356-368`). |
| `app/ponto/services/espelho.py:600` e `:669` | Flag única do cadastro limpa inconsistências de todos os dias montados | D=10/09, turno incompleto quando não isento; flag novembro=True; `inconsistente=False`. | Isenção histórica: [nao verificado]. |
| `app/folha/export.py:840` | Qualquer `data_demissao` preenchida exclui qualquer competência mensal | Competência setembro trabalhada integralmente; demissão 30/10; tentar setembro hoje recebe `rescisao_modulo_proprio`. | `classificar_export` é juiz registrado (`app/docs/JUIZES.md:74`), mas é a origem do erro. O próprio corte transcrito diz demissão **na janela**, não em qualquer futuro (`app/folha/export.py:834-839`). |
| `app/folha/export.py:788` | Vínculo `ativa=True`, ciclo atual e âncora atual travam aprovação de competência antiga | Setembro tinha 6x1 correto; vínculo novembro é 12x36 sem âncora; setembro ganha trava `sem_ancora_12x36` (`:813-814`). | `vinculo_do_dia`, registrado em `app/docs/JUIZES.md:98-99`. |
| `app/ponto/turnos.py:1556` e `:1591` | `_escalas_da_ata` escolhe maior `(ativa,data_inicio)` sem D para ler atas antigas | D=10/09 pertence ao vínculo A, pausa 60min; atual B pausa 120min; leitura de cadastro da ata usa TE de B. | `vinculo_do_dia`. Dívida explicitamente reconhecida em `:1539-1548`. |
| `app/ponto/juiz_batida.py:125` | `intervalo_cadastrado` pergunta `pausas_do_dia(date.today())`, depois lê horários/duração atuais (`:135-145`) | D era dia com duas pausas de 30min; hoje override diferente com total 90min; dia antigo recebe 90. | Marcos congelados: EC.marcos_do_dia (`app/docs/JUIZES.md:22-23`); juiz histórico do campo duração: [nao verificado]. |
| `app/ponto/juiz_batida.py:270` e `:299` | Usa pausa atual do template para descontar pausa pré-assinalada de D com batida incompleta | D tinha pausa declarada 60min e falta volta; template hoje passa para 90; desconto de D passa a 90. | Marcos congelados/`intervalo_declarado_do_dia` já existem; este último é usado no calculador (`app/ponto/calculador/periodo.py:518-523`), mas não listado nominalmente em JUIZES. |
| `app/ponto/juiz_batida.py:310` | `TipoEscala.intervalo_indenizavel` atual sai na interpretação da célula histórica | D flag=False; hoje=True; resposta histórica muda para indenizável. | Juiz histórico dessa flag: [nao verificado]. Não alegar efeito financeiro automático: o motor documenta que **não lê** essa flag por aval (`app/ponto/motor_calculo_v2.py:822-827`). |
| `app/ponto/motor_calculo_v2.py:832` e `:845` | Intervalo atual vira mínimo cadastrado do motor para todos os dias calculados | D duração=60; hoje=90, mesmas batidas; requisito de intervalo de D aumenta 30min. | Mesmo limite do leitor `intervalo_cadastrado`; piso por dia já usa juiz do previsto (`:841-844`). |
| `app/ponto/calculador/alimentacao.py:407` | Mesmo cadastro atual alimenta `_cad_min` da janela passada | Mesmo exemplo 60→90. | `intervalo_declarado_do_dia` e previsto já são lidos no módulo (`:377-378`); duração histórica: [nao verificado]. |
| `app/ponto/calculador/periodo.py:520` | `regua_do_dia(colaborador,dia)` lê mínimo do template sem passar `dia` | Mesmo exemplo 60→90. | Mesmo limite; o corpo já lê dois insumos por D em `:521-523`. |
| `app/ponto/turnos.py:788` | `_intervalo_livre()` atual decide realizado histórico, embora pausa em `:792` venha da célula | D fixo com pausa sem batida; hoje TE livre=True; realizado deixa de descontar a janela fixa (`:758-760`). | Marcos/intervalo do dia; histórico da modalidade livre: [nao verificado]. |
| `app/ponto/services/espelho.py:290` | Mesmo `_intervalo_livre()` atual fornecido ao realizado de D | Mesmo exemplo. | Idem; o vínculo do dia já é escolhido corretamente em `:286`. |
| `app/ponto/nucleo.py:27` e `:37` | `dt_fim_previsto_de` usa TE atual, sem célula/geradora congelada | Célula D fim=17h; template hoje fim=18h; materialização grava fim previsto 18h (`:55`). | `vinculo_do_dia` + EC.marcos_do_dia/`marcos_turno`, registrados em `app/docs/JUIZES.md:22-24` e `:98-99`. `janela_turno_de` aceita EC e documenta que sem EC usa fonte viva (`app/ponto/selecao_periodo.py:43-53`). |
| `app/core/regua_cct.py:352` | Empresa atual do colaborador e regime atual decidem regra na `competencia` passada | D colaborador em A/CCT; hoje transferido para B/CLT; D recebe piso CLT. | `regua_para` registrado (`app/docs/JUIZES.md:30-34`), mas ele próprio usa hoje; juiz histórico da empresa/regime: [nao verificado]. |
| `app/core/regua_cct.py:254` | `AplicacaoConvencao.ativo=True` atual filtra aplicação da CCT a D | Em D linha ativa e CCT vigente; desativada hoje; consulta de D cai em `sem_aplicacao`. | `_aplicacao_vigente` é o juiz registrado, sem histórico de `ativo`. |
| `app/core/regua_cct.py:261` e `:374` | Limites e parâmetros atuais de Sindicato decidem regra antiga | Em D CCT concedia dobra=True; hoje mesmo registro alterado False; D perde dobra. | `regua_para/_aplicacao_vigente`; versão histórica dos parâmetros: [nao verificado]. |
| `app/core/regua_cct.py:444` e `:453` | Switch atual `regua_cct_ativa` e regime atual decidem motor para competência passada | D switch=1/CCT com regra própria; hoje switch=0; recalcular D escolhe motor sem aquela regra. | `get_motor_cct` existe (`app/docs/JUIZES.md:29`), não é bitemporal. |
| `app/ponto/janelas.py:50` | Corte atual da empresa define janelas de qualquer competência | Setembro corte=21; hoje corte=26; setembro passa de 21/08–20/09 a 26/08–25/09. | `janela_fechamento/corte_da_empresa` registrados (`app/docs/JUIZES.md:35-36`, `:71`), mas usam cadastro atual. |
| `app/folha/services/ranking_he.py:122`, `:127`, `:131-135` | Posto/empresa atuais e ciclo do vínculo ativo filtram HE de competência encerrada | Setembro trabalhador A/12x36; hoje B/6x1; ranking setembro filtrado por A/12x36 o omite. | Para vínculo/ciclo: `vinculo_do_dia`; empresa histórica: [nao verificado]. |
| `app/ponto/services/espelho.py:43` | Início do vínculo ativo atual define piso histórico do espelho/drawer | Setembro vínculo antigo existente; atual começa 01/11; piso hoje passa a novembro; consumidor cru perde setembro. | Vínculo por D existe. O próprio comentário registra que drawer usa piso cru, em `:25-30`. |
| `app/holerite/matriz.py:87-104` | Empresa atual + último par admissão/demissão define universo de meses passados | Setembro empregado A; transferência outubro para B; universo setembro/A deixa de incluir a pessoa, embora fosse vínculo daquele mês. | Histórico empresa/vínculo empregatício: [nao verificado]. O fato documental pode recolocar quem já tem holerite (`app/holerite/views.py:267-268`), sem proteger quem deveria ter documento e ainda não tem. |
| `app/ponto/services/fechamento.py:430-435` | Empresa/situação/demissão atuais definem universo de recalcular competência passada | Setembro pessoa na A; hoje empresa=B; recalcular setembro/A não a inclui (`:435`). Reativação após demissão apaga o último desligamento e universo não representa dois contratos antigos. | Juiz histórico da empresa/contrato: [nao verificado]. |

**Leituras que merecem distinção**

- `app/folha/export.py:791`: a cláusula **situação atual ativo** na trava da âncora é INTENCIONAL, com corte transcrito “31/07 Ronald: desligado nao trava por ancora”; selo confirma literalmente essa política em `app/folha/tests/test_contrato_export_um_juiz.py:159-160`. Isso não justifica escolher vínculo atual em `:788`.
- `app/folha/export.py:1061-1062`, `:1180-1183`, `:1211`: comparação com a data efetiva admissão/demissão é recorte intencional do espelho, documentado em `:1056-1060` e `:1156`; não confundir com o veto global `:840`.
- `app/ponto/services/espelho.py:202`, `app/holerite/views.py:282` e `app/folha/views_retrato.py:107` leem escala atual para cabeçalho/enriquecimento/filtros de gestão. A interpretação como declaração de “escala que valia em D” não está provada; há descrição explícita de cadastro atual em `app/folha/views_retrato.py:89-94`. Não promover automaticamente a BUG.
- `app/ponto/turnos.py:2462` e `:2481` são próximas batidas **agora**; chamador de produção passando instante histórico: [nao verificado].
- PerfilApuração `parametros.intrajornada_minutos` em `app/ponto/motor_calculo_v2.py:859-865` tem ramo morto documentado em `app/core/configuracao_efeito.py:57`; não contar como sítio de produção sem chamador.

**Escritores e leitores**

- `Colaborador.isento_ponto`: tela Passe Livre chama escrita em `app/colaboradores/views.py:176`, com antes/depois em `:185-186`; porta `gravar` atribui/salva em `app/colaboradores/services/colaborador.py:55-61`. Leitor central `app/core/constants.py:13-18`; consumidores históricos acima.
- `Colaborador.empresa`: porta permite operação `posto` em `app/colaboradores/services/colaborador.py:33` e salva em `:55-61`; leitores históricos `regua_cct:352`, fechamento `:435`, holerite/matriz `:87`, export `:767`/`:326`/`:1055`/`:1248`.
- `data_demissao/situacao`: operação própria em `app/colaboradores/services/colaborador.py:32`; desligamento escreve em `app/colaboradores/services/desligamento.py:105` e reativação limpa em `:211`. Leitores acima.
- `Empresa.dia_inicio_competencia/regime_trabalhista`: formulário monta campos em `app/colaboradores/views.py:496-523`, chama atualização em `:538`; porta atribui e salva `app/colaboradores/services/estrutura.py:17-20`. Leitores `janelas:50`, `regua_cct:353`/`:453`.
- `AplicacaoConvencao.ativo`: `cadastrar` cria em `app/core/services/aplicacao_convencao.py:120-122`, altera em `:130-132`; `desativar` em `:151-155`. Leitor decisório `app/core/regua_cct.py:254`.
- Parâmetros e vigência de `Sindicato`: `app/core/views_config.py:307-320` escreve no mesmo registro; `:325` cria. Leitor `app/core/regua_cct.py:261-262`/`:374`.
- TipoEscala/vínculo: escritores detalhados ficam com agente escala; leitores adicionais acima, especialmente `juiz_batida`, motor, realizado e materialização.

**Selos que não mordem esta classe**

- `app/folha/tests/test_isento_cargo_export.py:58-68` chega a **exigir** apagar eventos do snapshot pré-isencão; fixtures têm apenas flag final (`:30-41`), sem duas vigências. Isso prova comportamento cristalizado, não lei autorizando apagar competência anterior.
- `app/core/tests/test_semselo5_regua_pelo_vinculo_do_dia.py:47-75` testa troca de vínculo/praca/ciclo mantendo a **mesma empresa**, sem edição temporal do regime/aplicação/parâmetros.
- `app/folha/tests/test_contrato_export_um_juiz.py:154-160` testa trava de âncora e desligado, sem vínculo posterior cuja escala contamine competência anterior; o selo sela unidade da autoridade atual.
- `app/folha/tests/test_txt_le_a_celula_e_o_juiz.py:244-249` cobra que TXT obedeça à trava do juiz; não valida que a trava foi escolhida pelo vínculo histórico.
- `app/ponto/tests/test_fechamento_escopo_competencia.py:27-59` cobre desligado na janela, afastado atual e desligado antigo; não cobre mudança de empresa/readmissão.
- `app/folha/tests/test_borda_celula.py:40-62` cobre célula/ata, data passada/futura, miolo e véspera; isenção vigente em D versus flag atual não é caso dessa bateria.
- `app/ponto/turnos.py:1539-1548` declara que o selo `test_vinculo_do_dia_pela_celula` ainda admite a dívida `_escalas_da_ata`; não é defesa bitemporal completa.
- `app/docs/CORTES.md:288` adia versões `vigente_de/ate + registrado_em` de vínculo/escala/regra para P3 após 20/20. É dívida/ordem de obra; não é corte que torne correto ler hoje para responder D.

Cautela jurídica: os cortes recentes de quarentena exigem janela desde a flag (`app/docs/CORTES.md:348`), mas explicitam que quarentena não altera espelho/folha/TXT. Não usar esse juiz de cobrança como substituto comprovado do juiz histórico de cálculo.

## 05/11 15:10 · sessao 7ccf44e239a5 · - · - · origem=Ronald (tmux)

**Rubricas e conversões**

- O mapa usado é dinâmico: rubricas ativas globais, substituídas pelas da empresa para o mesmo evento (`app/folha/export.py:958-965`). A configuração viva de emp2/emp3 não foi fornecida: **[nao verificado]**.
- A fixture explicita o mapa esperado: adicional noturno→25, intrajornada→243, atraso→8069, falta→8792 dias e HE feriado→200 (`app/folha/tests/test_sm1_monster_export.py:72-83`). Contra-exemplo de mapa fixo: override empresarial de intrajornada para 270 (`app/folha/tests/test_r1_caracterizacao_rubricas_map.py:31-36`).
- Horas saem como `int(round(float(valor)*100))`; dias como `int(round(float(valor)))` (`app/folha/export.py:34-45`). Não há conversão para HH:MM.
- `0025`: `horas_noturnas`→`adicional_noturno` (`app/folha/export.py:436`).
- `0243`: `horas_intra_indenizada`→`intrajornada_diurna` (`app/folha/export.py:437`).
- `8069`: `horas_atraso + horas_saida_antecipada` (`app/folha/export.py:388`, `app/folha/export.py:440-441`).
- `0200`: pode receber HE100 normal (`horas_extras_100 − horas_extras_100_feriado`), parcela feriado ou folga trabalhada; feriado é suprimido em 12x36 sem cláusula de dobra (`app/folha/export.py:409-435`). As migrações configuram globalmente folga e HE100 normal para 200 (`app/folha/migrations/0008_folga_trabalhada_rubrica_200.py:6-11`, `app/folha/migrations/0009_evento_he100_rubrica_200.py:4-10`).
- Zero e evento sem vínculo de rubrica não geram linha (`app/folha/export.py:714-720`).

**8792 é o ponto decisivo**

Ela **não deriva de campo do FM**. O exportador consulta `datas_falta` e emite `len(datas)` mais os registros 11 das datas (`app/folha/export.py:721-725`, `app/folha/export.py:204-209`). As datas vêm das ausências aprovadas que intersectam a janela e cujos tipos o catálogo vivo considera descontáveis (`app/folha/export.py:60-81`, `app/folha/export.py:158-174`).

Logo, col954 ter `minutos_abonados=1440` e nenhuma hora não permite validar nem invalidar 8792=2: faltam as ausências/datas elegíveis. **Valor 2 correto: [nao verificado]**. O mesmo limite se aplica a 576/8792=1 e 643/8792=3.

Contra-exemplo à conversão de horas/minutos em 8792: dois registros com igual total de minutos, distribuídos em uma ou duas datas elegíveis, podem produzir contagens diferentes, pois o código usa datas (`app/folha/export.py:149-151`, `app/folha/export.py:207`).

**Conferência aritmética dos anexos**

São 21 linhas fornecidas: 18 linhas de horas batem os valores dos campos pela conversão; três linhas 8792 não são conferíveis pelo FM. A origem dos valores é o dado colado, sem arquivo local: **[nao verificado]**; as fórmulas são as citadas acima.

| col | rubrica | cópia | campo FM / cálculo | resultado |
|---|---|---:|---|---|
|400|0243|1300|13,00×100|S|
|457|0025|8224|82,24×100|S|
|457|0200|1467|feriado 14,67×100; normal=14,67−14,67=0|S, condicionado à não supressão|
|457|0243|392|3,92×100|S|
|576|0025|14400|144,00×100|S|
|576|0243|1400|14,00×100|S|
|576|8792|1|não usa campo FM|[nao verificado]|
|907|0025|11198|111,98×100|S|
|907|0243|124|1,24×100|S|
|907|8069|9|(0,09+0)×100|S|
|951|0200|734|folga trabalhada 7,34×100|S|
|951|0243|17|0,17×100|S|
|954|8792|2|não usa campo FM|[nao verificado]|
|639|0025|12000|120,00×100|S|
|639|0243|1500|15,00×100|S|
|643|0243|1100|11,00×100|S|
|643|8792|3|não usa campo FM|[nao verificado]|
|752|0243|1500|15,00×100|S|
|755|0025|12800|128,00×100|S|
|755|0243|1600|16,00×100|S|
|761|0243|1600|16,00×100|S|

Contra-exemplo à ideia de `0200 = horas_extras`: col951 tem a parcela de folga trabalhada; o exportador lê esse campo separadamente (`app/folha/export.py:435`). Col457 não permite somar `horas_extras`, `horas_extras_100` e `horas_extras_100_feriado`: o feriado é subtraído para formar a parcela normal (`app/folha/export.py:409-410`).

## 05/11 15:10 · sessao 8e7ebb6a4c0f · - · - · origem=Ronald (tmux)

Item 4 — conclusões com referências:

- **Escritor atual do estado:** `ChamadoColaborador._gravar_estado` atribui `status_local`, carimba `resolvido_em`/`via_encerramento` e salva; registra também a transição em `evento('estado_chamado', ...)`. [`app/chamados/models.py:195`, `:222`, `:227`, `:274`]
- **Entradas que chegam ao escritor:** humano `transicionar` → `_aplicar` → `_gravar_estado`; sistema `reconciliar`; descarte `cancelar`; cascata `encerrar`; competência trancada `encerrar_por_competencia_trancada`. [`app/chamados/models.py:193`, `:430`, `:607`, `:736`, `:1524`, `:1541`]
- **Exemplos de chamadores concretos:** fechamento da disputa dispara Defesa 1 → `ch.reconciliar`; cartório chama `reconciliar`; desligamento chama `encerrar`; serviço de tranca chama `encerrar_por_competencia_trancada`. [`app/chamados/signals.py:42`; `app/ponto/services/cartorio.py:1488`; `app/colaboradores/services/desligamento.py:155`; `app/chamados/services/competencia_trancada.py:184`]
- **Quem escreveu os terminais específicos dos 170 dias:** **[não verificado]**. O anexo não traz ID da pergunta/âncora, `via_encerramento`, data de transição nem trilha. Não permite escolher um desses chamadores nem provar que a pergunta já estava viva quando fecharam. O código guarda via e trilha que poderiam distinguir os caminhos, mas seus valores não estão no dado entregue. [`app/chamados/models.py:239`, `:275`]

**F1 existe e pretende impedir exatamente esse novo fechamento.** A âncora é destino, ou pai da disputa quando destino nulo; busca perguntas não validadas e usa C6 no pós-estado. Só inclui competência **entregue is False**; entregue ou indeterminável fica fora. [`app/chamados/models.py:302`, `:327`, `:332`, `:347`, `:351`]

No sistema, F1 reata todas as vivas à cobrança sobrevivente legítima do mesmo colaborador-dia; se não consegue, recusa fechamento. No humano, pergunta viva ancorada causa `ValueError`. O núcleo tem tripwire adicional vivo→terminal. [`app/chamados/models.py:159`, `:357`, `:373`, `:393`, `:405`, `:415`, `:215`]

**Contraexemplos essenciais à inferência “há terminal com viva, logo F1 deixou fechar com viva”:**

1. **Pergunta nova pode nascer sob chamado já terminal:** o ramo novo de `PerguntaDisputa.abrir` faz `objects.create(... chamado_destino=...)` sem `_costura_nao_mata_o_fio`; a guarda aparece só no ramo de reapontamento da existente. O próprio comentário declara nascimento fora do escopo F1. Isso é caminho concreto da cópia, mas atribuí-lo aos 170 é **[não verificado]**. [`app/chamados/models.py:2427`, `:2436`, `:2465`; `app/chamados/services/fio.py:750`]
2. **Pergunta pode voltar viva por leitura depois:** destrancar devolve vitalidade; célula passando a acusar faz C6 retornar viva. Portanto um terminal pode preceder a vitalidade observada. Se ocorreu nesses 170, **[não verificado]**. [`app/chamados/juizes.py:917`, `:923`, `:1123`]
3. **Terminal→terminal não é vivo→terminal:** F1 retorna imediatamente fora de vivo e o tripwire só cerca passagem cujo estado anterior em memória ou banco é vivo. Existe teste explícito terminal→terminal com viva aceitando `resolvido→fechado`. [`app/chamados/models.py:400`, `:213`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:211`]
4. **Competência entregue fica fora de F1:** teste explicita viva de competência exportada não impedir fechamento; isso não demonstra que algum dos 170 esteja nesse caso. [`app/chamados/models.py:347`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:113`]

**Certeza:** alta sobre escritor, guardas e lacunas da cópia; indeterminada sobre cronologia e chamador dos 170. Os testes citados foram **lidos**, não executados. [`app/chamados/tests/test_o261e_f1_porta_encerramento.py:81`, `:127`, `:192`; execução: não verificado]

**Como vira ausência de porta:** `TERMINAIS = resolvido, fechado, cancelado, superado`; a fila só admite `COBRANCA = aberto, em_analise` e remove silêncio vigente; porta por pergunta exige âncora na fila, pergunta viva, resposta não vazia se respondida e presença no app. Uma viva sob âncora terminal falha nessa pertença à fila. [`app/chamados/catalogo/motor.py:24`; `app/chamados/services/painel.py:186`; `app/chamados/juizes.py:1318`]

**Classes a/b/c/e: predicados, não atribuição histórica automática.**

- a: vivo da célula ou âncora viva fora da fila; no anexo os dois são `registrado`, que `qs_na_fila` exclui por exigir COBRANCA. Porta esperada: chamado cobrando na fila ou pergunta sob âncora na fila. Não implica que trocar todo `registrado` por aberto seja correto: catálogo diz que registro deliberadamente não exige ação. [`app/chamados/services/vigia_fio.py:662`; `app/chamados/services/painel.py:186`; `app/chamados/catalogo/motor.py:25`]
- b: C6 viva com `respondida_em != None` e texto vazio; porta a rejeita por `respondida_sem_texto`. Escritor histórico desse estado específico **[não verificado]**. [`app/chamados/juizes.py:1196`, `:1322`; `app/chamados/services/vigia_fio.py:672`]
- c: pergunta viva respondida com âncora terminal; caminho esperado de validação precisa de âncora na fila. Não equivale a toda resposta com terminal: validada morre logo no primeiro degrau do C6, e resposta comprovadamente concordante pode morrer. [`app/chamados/services/vigia_fio.py:674`; `app/chamados/juizes.py:1077`, `:1100`, `:1322`]
- e/terminal: algum C6 vivo com âncora terminal após precedências a/b/c/d; isso não significa que todas as perguntas daquela célula estejam sob terminal. [`app/chamados/services/vigia_fio.py:668`, `:674`, `:686`]

Referências extras para a matriz escritor/leitor: âncora `chamado_que_lista` em `app/chamados/services/fio.py:97`; escritor de reapontamento `reapontar_resolvedora`, guardado e atômico em `:836–848`; nascimento e reapontamento por identidade também escrevem em `app/chamados/models.py:2439` e `:2465`. O comentário “escritor único” do serviço não elimina esses dois caminhos explícitos do modelo.

## 05/11 15:10 · sessao 5f2d08d820fe · - · - · origem=Ronald (tmux)

Leitura de `app/escala/` e do cartório real, sem execução com banco e sem escrita. O caminho informado `app/escala/cartorio.py` não existe nesta cópia; o conteúdo correspondente está em `app/ponto/services/cartorio.py:1697` e `:1762`.

Os contraexemplos abaixo são hipóteses concretas para provar o ramo estático; ocorrência em produção: [nao verificado].

1. **Cartório: os dois pontos iniciais são INTENCIONAIS.** `c.situacao != 'desligado'` controla emissão presente sobre um furo passado, preservando o julgamento do dia. O corte HX-CARTORIO-UNIVERSO/T4.2 diz “julga todo mundo, EMITE só pra quem responde”, em `app/ponto/services/cartorio.py:1651`. Os gates estão em `:1697` e `:1762`; o selo testa desligado julgado sem chamado em `app/ponto/tests/test_hx_cartorio_universo.py:34`. Afastado também é explicitamente **agora**, pelo corte 19/10 item 10 registrado em `app/ponto/services/cartorio.py:1281`, principalmente `:1286`; leitor `:1298`, usos `:1711` e `:1763`.

2. **Isenção atual reescreve interpretação histórica.** `app/ponto/services/cartorio.py:1141` lê uma flag por colaborador e a transporta para todo dia em `:1229`, `:1246`, `:1419`, `:1806`, além de `:890`. `app/escala/utils.py:904` usa a mesma flag para todo `dias_out` em `:909`, com chamadas de produção em `:808` e `:1871`. `app/escala/services/leitor_celula.py:349` reaplica a flag por cima da ATA em `:412`. Exemplo: D=26/10, não isento, quatro marcos missing; hoje isento por mudança de função em 03/11; o dia antigo ganha cumprimento e não acusa, embora a isenção não existisse em D. Corte Art.62 citado em `app/escala/utils.py:894` manda completar o isento, mas não estabelece retroatividade da flag. L-124 só recorta a impressão/horizonte, não historiciza isenção (`app/ponto/services/cartorio.py:509`, `:514`, `:520`; `app/docs/LEIS.md:145`). **BUG temporal; juiz histórico de isenção não registrado em JUIZES** (`app/docs/JUIZES.md:20` a `:102`); `core.constants.isento_ponto` é juiz atual, sem data.

3. **Saída esperada duplicada e histórica.** `app/escala/utils.py:959` escolhe vínculo ativo aberto hoje para `ts_entrada`, e só depois extrai o dia em `:963`. Chamador concreto: entrada pendente antiga em `app/chamados/detector_anomalias.py:42`/`:45`, gravando data do evento em `:49`. Exemplo: D tinha vínculo noturno; hoje não há vínculo ativo aberto; retorna `None` em `app/escala/utils.py:961`, embora D tenha vínculo/célula com saída D+1. O selo coloca a função em HOJE (`app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:134`) apesar desse parâmetro histórico. **BUG; `vinculo_do_dia`, depois `EC.marcos_do_dia`.**

4. **Vínculo de D correto, mas marcos do template vivo.**
   - `app/escala/servico_jornada.py:208`: `data_saida_esperada` chama `fim_turno(te,d)` sem `ec`.
   - `app/escala/servico_jornada.py:238`: `_turno_noturno_completo` decide cruzamento pelos escalares vivos `hora_inicio/hora_fim`.
   - `app/escala/servico_jornada.py:376`: `classificar_falta` verifica teto com `fim_turno(ec.tipo_escala,D)` sem `ec`.
   A falta do argumento desvia para template em `app/escala/servico_jornada.py:71`; a porta já permite DNA pelo argumento em `:69`. Exemplo: D DNA 19–07; hoje template 07–19. A data de saída passa de D+1 para D; guarda noturna muda. **BUG; `EC.marcos_do_dia`/`marcos_turno(ec=)`**, registrados em `app/docs/JUIZES.md:22`.

5. **Régua fixa atual faz o DNA histórico perder marcos no builder normativo.** `app/escala/utils.py:1390` monta colunas pelo template atual. O dia usa DNA em `:1628`, mas `:1729` alinha somente às colunas atuais. Exemplo: D DNA 08–17, sem pausa, sem batidas; template hoje 09–18. Match cria missing E08/S17; alinhamento vira `[None,None]`, sem missing. Batidas antigas casadas fora das colunas viram órfãs em `:1740`. A origem é leitura de horas atuais em `:1374`–`:1393`, não a obtenção do DNA. **BUG; célula/EC.marcos_do_dia.** Também há leitura viva de intervalo livre em `:1666` e `:2069`, e número atual de intervalos do intermitente em `:1383`/`:1385` que limita o sweep em `:1562`/`:1565`; vigência histórica desses parâmetros não encontrada [nao verificado].

6. **Apuração histórica de furos filtrada pelo cadastro atual.** `app/escala/services/furos_diarios.py:33` retorna vazio para isento atual; `:35`/`:36` retorna vazio pela régua do vínculo ativo atual antes de ler a ATA. A escrita posterior apaga furos do período em `:108`. Exemplo: D vínculo A confiável, furo lavrado; hoje vínculo B suspeito. Reapuração exclui e apaga furo legítimo de A. **BUG; `vinculo_do_dia` para régua de D; isenção histórica sem juiz registrado.**

7. **Ranking histórico pelo vínculo/empresa de hoje.** `app/escala/services/furos_diarios.py:133` filtra empresa atual; `:158` agrupa furos passados pelo template ativo atual. Exemplo: D furo sob A/empresa1; hoje B/empresa2; o histórico some de empresa1 e aparece sob B. **BUG se a pergunta for atribuição histórica**, em contraste com destinatário atual de cobrança [nao verificado quanto ao contrato de UI]. O selo reconhece a dívida em `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:235`.

8. **Universo de cobrança atual versus classe do período.** `app/escala/services/furos_diarios.py:248` registra corte 17/08 para universo atual, e `:259` implementa empresa/situação atuais: **INTENCIONAL para universo**. Porém `:301` escolhe vínculo atual para dizer “sem dia previsto no período”, percorrendo D em `:316`; comentário declara pergunta histórica em `:290`. Exemplo: D havia A com trabalho previsto e hoje B sem fase; período vira LIMBO por B. **BUG na classificação histórica; `vinculo_do_dia`.** Isento atual em `:298` também decide a classe de todo período.

9. **Diagnósticos históricos com vínculo atual.**
   - `app/escala/services/meio_periodo.py:152` e `:174`: sábado de D comparado com template atual. Exemplo D tinha 4h sem pausa, hoje 8h com pausa; duas batidas de 4h viram evidência de cadastro incorreto.
   - `app/escala/services/cadastro_realidade.py:202`/`:209`/`:212`: template atual alimenta juiz de períodos de células passadas; `:223` compara sua jornada atual à média histórica. Exemplo D estava corretamente 4h; hoje 8h; histórico acusa C2.
   - `app/escala/management/commands/celulas_contra_o_template.py:28`/`:34`/`:36`: célula de D comparada ao template do vínculo ativo hoje. Exemplo D sob 12x36 com trabalho sábado, hoje 5x2; célula correta é acusada.
   **BUG; vínculo do dia + DNA.** Os três estão ABERTO no selo: `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:229`, `:231`, `:241`.

10. **Impacto “antes” da troca.** `app/escala/services/impacto_troca.py:34` escolhe ativo atual para todo trecho passado em `:39`. Exemplo sem célula em D: A previa trabalho; B atual não cobre D e sua fase diz folga; “furos antes” zero. Com célula existente, `eh_dia_trabalho` já lê célula da pessoa (`app/escala/models.py:1090`), então não alegar que sempre diverge. **BUG no ramo sem célula; `vinculo_do_dia`.** O “depois” é simulação intencional (`app/escala/services/impacto_troca.py:8`), não um erro temporal.

11. **Fase conflitante.** `app/escala/services/fase_conflitante.py:71` escolhe só vínculo ativo e `:80` compara seus próprios calendário/âncora para todos os 60 dias, sem recortar vigência. Exemplo B iniciou hoje e conflita com sua foto; conta conflitos nos 59 dias de A. **BUG; `vinculo_do_dia` e `folga_lancada_do_dia`**, dívida já explícita em `app/docs/JUIZES.md:94`/`:96`. Selo HOJE chama apenas “cadastro” em `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:128`.

12. **Régua/empresa atuais mesmo com vínculo de D.** `app/escala/services/escala_certa.py:32` usa `ec.regua_classe` atual para julgar confiabilidade de D e liberação de folga100, finalidade em `:3`. `:38` usa empresa atual para competência de D. Exemplo D confiável/empresa corte21, hoje régua LIMBO ou empresa corte1: muda veto ou janela de evidência. **BUG temporal da régua/empresa; juiz histórico desses campos não encontrado em JUIZES**; `vinculo_do_dia` só resolve o vínculo, não versões internas do seu cadastro. A régua degenerada também lê template vivo para D em `app/escala/regua_defesa.py:26`, sendo consumida pelo veto em `:46` e por `app/ponto/services/furos_vetados.py:40`; distinguir veto de cobrança agora, possivelmente intencional, de marcos históricos. Lastro explícito para “usar hoje” nesse último veto: [nao verificado].

**Escritores relevantes e leitores encontrados**

- Isenção: edição em `app/colaboradores/views.py:176`, trilha em `:185`; porta autoriza campo em `app/colaboradores/services/colaborador.py:34`. Leitores históricos listados nos itens 2, 6, 8; diagnósticos de régua também usam flag atual em `app/escala/regua_defesa.py:136`, `:355`, `:513`; propositor em `app/escala/services/propositor.py:456`.
- EC tipo/posto/ativa/fim/âncora: `app/colaboradores/services/vinculo.py:265`–`:274`; criação `:278`; encerramento `:797`; correção retroativa `:922`; `fechar_vigencia` nasce em `:561`, `encerrar_vinculo` em `:780`. Correção retroativa é operação explicitamente declarada, não um BUG por si só (`:250`–`:257`).
- Horas/pausas/intervalo do TipoEscala: `app/escala/services/cadastro_tipo.py:224`, `:226`, `:229`, `:234`, `:245`; propagação retroativa pela porta em `:334`/`:348`. Leitores: itens 4, 5, 9 e ramo fallback de `app/escala/models.py:1133`.
- Régua: `app/escala/regua_defesa.py:766` grava classe/escore/dias/data pela porta `app/colaboradores/services/vinculo.py:969`; corroboração em `app/escala/regua_defesa.py:667`, desfazimento `:722`; leitores `rotulo_regua_para_espelho` em `:628`, `deve_vetar_regua` em `:644`, e itens 6,8,12.
- Empresa: transferência de posto em `app/colaboradores/views.py:1386`; porta autoriza posto/empresa em `app/colaboradores/services/colaborador.py:33`. Leitores históricos: itens 7,12 e cartório `app/ponto/services/cartorio.py:1145`/`:1676`.

**Selos que deixam a classe passar**

- `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:11`/`:12`: aceita HOJE ou ABERTO; a dívida não fica vermelha. Furos/meio-período/cadastro/impacto estão explicitamente ABERTO em `:229`–`:242`. `data_saida_esperada` e fase conflitante estão HOJE em `:128`/`:134`, sem provar o contrato temporal do chamador. Limites AST expressos em `:21`.
- `app/escala/tests/test_contract_juiz_escala.py:124`: caça tomada de `escala_geradora`; não equivale a caça de template vivo ou atributo atual. O próprio selo explica ausência de padrões proibidos da família em `:28`–`:32`.
- `app/escala/tests/test_selo_bypass_marcos_dna.py:65` e `:83`: testa duas alterações de horário/intervalo para classificar falta, mas não exercita inversão noturno/diurno da guarda nem resolução da data da saída; fixtures em `:33`–`:63`.
- `app/escala/tests/test_grade_isencao.py:19` e `:41`: compara flag fixa True/False, sem data de início da isenção.
- `app/escala/tests/test_fase_conflitante.py:49`: todas fixtures têm um único vínculo iniciado em janeiro; casos `:60` e `:72` provam contrafase, não troca de vínculo.
- L-020 exige DNA e permite correção pela porta (`app/docs/LEIS.md:71`); fallback sem célula/DNA é contrato explícito de `app/escala/models.py:1098`–`:1103`. Não classificar todo fallback como bypass sem declarar a condição de ausência.

## 05/11 15:10 · sessao 96e54032dc5e · - · - · origem=Ronald (tmux)

O código local **não exclui automaticamente col639 pela admissão em novembro**:

- `classificar_export` seleciona FM pela empresa/mês/ano, sem filtro de admissão: `app/folha/export.py:764-769`.
- `dem=None` passa pelo bloqueio de rescisão: `app/folha/export.py:840-845`.
- Com `horas_trabalhadas=180.28`, não executa o ramo de cadastro zero: `app/folha/export.py:876`.
- A retenção ajusta início para a admissão e devolve **nenhum motivo** quando a janela colapsa: `app/folha/export.py:1180-1187`. Logo admissão posterior, nesse leitor, libera da retenção; não marca `fora`.
- Passando código, férias e homologação, o item permanece `entra`: `app/folha/export.py:846-854`, `app/folha/export.py:942-954`. A montagem lê o FM aprovado e seus eventos: `app/folha/export.py:668`, `app/folha/export.py:697-720`.

**Contraexemplo:** FM09 aprovado, horas positivas, demissão nula, admissão novembro, código válido, sem férias com batida e rubricas homologadas: o fluxo local pode emitir valores antigos de setembro. Portanto “readmitido fica fora por admissão posterior” é falso para esse código. **Certeza alta sobre o fluxo; causa de `col639 hoje=[]` [nao verificado].**

A porta HTTP atual fornece objetos recalculados em leitura, não necessariamente o FM gravado: `app/folha/views.py:207-212`, `app/folha/views.py:268`. Já `montar_linhas_export()` sem injeção lê o FM: `app/folha/export.py:698-703`. Sem código de `sonda_c.py`, não é possível determinar qual autoridade produziu `hoje=[]` [nao verificado]. O universo do recalculo também não exclui por admissão: `app/ponto/services/fechamento.py:430-436`.

Quanto ao H12, o código local ainda traz o bloqueio genérico `data_demissao is not None`: `app/folha/export.py:840`. A cura consta como intenção/fila em `app/docs/PROMPTS.md:1860` e `app/docs/BACKLOG.md:1741`; o código da cura executada na sombra não está comprovado neste hash [nao verificado].

## 05/11 15:09 · sessao 90f18801beba · - · - · origem=Ronald (tmux)

Censo H06, somente leitura; nenhum teste executado [nao verificado]. [nao verificado]

- **Escritores de `Ausencia.minutos`:** `criar_ausencia` recebe default 0 em `app/ponto/services/ausencia.py:130`, monta o campo em `:343` e grava em `:418`; `editar_ausencia_registro` escreve zero/valor humano em `:507-508`; `repor_minutos_de_reversao` escreve diretamente o inteiro recebido em `:531-532`; `Ausencia.save` deriva somente falta aprovada com número falso em `app/ponto/models.py:877-887`. A transição de estado inclui a derivação nos `update_fields` em `app/ponto/services/ausencia.py:838-839`.
- **Chamadores:** os dois sítios curados omitem minutos em `app/chamados/services/materializacao.py:484-492` e `:520-528`; admin humano recebe em `app/ponto/views.py:2386` e envia em `:2413-2419`; PWA envia em `:2534-2539`; edição envia em `:2792-2800`; API recebe e envia em `app/api/views.py:1815-1826`; regularização envia 0 em `app/ponto/services/regularizacao.py:119`; abandono envia 0 e chamado de origem em `app/ponto/management/commands/marcar_abandono_provisorio.py:76-84`; férias envia 0 em `app/ponto/services/ausencia.py:1206-1211`.
- **Porta registrada:** `repor_minutos_de_reversao` está dentro do arquivo que o gate considera porta única, `app/ponto/tests/test_chokepoint_ausencia_gate.py:65`; portanto “escritor fora do arquivo-porta” seria descrição incorreta. A função constitui exceção que não valida origem do arquivo, tipo, status, competência nem valor esperado atual antes de gravar; apenas exige motivo em `app/ponto/services/ausencia.py:527-535`.
- **Mapa:** escreve tipos em `app/chamados/models.py:3007-3010`, único leitor operacional encontrado em `app/chamados/services/materializacao.py:468`; o import está em `:49`.
- **Help text:** escritores `app/ponto/models.py:759` e `app/ponto/migrations/0077_ausencia_minutos_help_text_grade.py:15`. `AusenciaForm` é `forms.Form`, com campos manuais sem minutos/help_text em `app/ponto/forms.py:41-53`; admin é somente leitura e exibe minutos em `app/ponto/admin.py:38-44`; drawer usa texto manual em `app/templates/ponto/partials/painel_ausencia.html:147-150`. Consumidor explícito que renderize o novo help_text nas telas da aplicação: [nao verificado].

Leitores e consequências:

1. **Continua existindo limiar 480:** `MINUTOS_DIA_INTEIRO = 480`, `app/ponto/turnos.py:2580`, usado em `:2598-2600`; o comentário reconhece que é número cravado com aval antigo e que trocá-lo pela grade exige decisão em `:2575-2579`. A UI nova lê esse juiz em `app/ponto/views.py:2237-2241` e exibe “1 dia” quando falso em `app/templates/ponto/ausencias.html:163-164`. Leitores alcançados desse mesmo juiz: grade `app/escala/utils.py:1209-1212`; serviço de jornada `app/escala/servico_jornada.py:341-345`; decisão de período `app/ponto/services/ausencia.py:886-892`; painel `app/ponto/views.py:2085`; ausência de dia inteiro `app/ponto/turnos.py:2675`.

2. **Outros cálculos ainda leem minutos como verdade de parcial:** `app/ponto/services/fechamento.py:665-669` soma qualquer pago com minutos, e `:685-686` pula abono integral; `app/ponto/services/efeito_ausencia.py:44-52` reproduz o critério; crédito por dia idem `app/ponto/services/credito_parcial.py:24-25`. Isso não é automaticamente defeito da migração H06, mas constitui resposta diferente à pergunta “é parcial?” daquela de `cobre_parte_do_dia`, `app/ponto/turnos.py:2598-2600`.

3. **Contraexemplo parcial concreto:** cadastro sintético com 12x36, âncora 07/10/2026, jornada 07–19 com pausa12–13, atestado de 600 minutos em 07/10/2026: grade prevê660, conforme geometria de `app/escala/utils.py:2290-2295` e `:2326-2342`; juiz limiar480 responde dia inteiro (`app/ponto/turnos.py:2580`, `:2600`), UI mostra “1 dia” (`app/templates/ponto/ausencias.html:164`), mas fechamento trata600 como parcial (`app/ponto/services/fechamento.py:665-669`, `:685-686`). Período explícito para esse atestado é recusado por “Só ausência PARCIAL tem período” (`app/ponto/services/ausencia.py:891-892`). Não é registro de produção [nao verificado].

4. **Contraexemplo de range pela grade:** mesmo cadastro sintético, falta07..09/10/2026: 12x36 âncora07 responde trabalho/folga/trabalho (`app/escala/models.py:1289-1292`), previsto660/0/660; `Ausencia.save` soma1320 (`app/ponto/models.py:882-887`). Export rateia uniformemente1320/3, resultando440/440/440, inclusive folga (`app/folha/export.py:103-111`, `:129-131`). O novo teste range exercita três dias iguais420/420/420, que não distingue essa divergência (`app/chamados/tests/test_h06_falta_pela_grade.py:128-134`). Não é registro de produção [nao verificado].

5. **Grade0:** novo fluxo grava falta aprovada com0 quando não há vínculo/template/certeza de trabalho (`app/escala/utils.py:2277-2292`; derivação `app/ponto/models.py:877-887`); o passivo, ao contrário, mantém720 quando grade0 (`app/ponto/management/commands/rederivar_falta_declarada.py:59-64`), comportamento explicitamente cristalizado no teste `app/ponto/tests/test_h06_rederivar_falta_declarada.py:122-132`. Para cadastro sintético sem vínculo e falta em07/10/2026, novo0 versus legado720. A intenção adicional de excluir grade0 do ato não consta do resumo de aval fornecido [nao verificado].

6. **Atestado muda numericamente e muda dinheiro de leitores não migrados:** os sítios passam de720 para0 (`app/chamados/services/materializacao.py:484-492`, `:520-528`); atestado não entra na derivação exclusiva de falta (`app/ponto/models.py:877`). Assim, depois de aprovado, os leitores antigos deixam de lhe dar crédito720 e passam a aboná-lo como dia inteiro (`app/ponto/services/fechamento.py:665-690`; `app/ponto/services/efeito_ausencia.py:44-56`). Essa mudança existe além da simples correção do tamanho da falta; autorização específica além do resumo dos dois avais [nao verificado].

7. **Leitores visuais/API adicionais de minutos:** horas `app/ponto/models.py:866`; rótulos `app/ponto/services/dia_decidido.py:219-235`, `:484-488`; cartão/histórico `:512`; API `app/api/views.py:1878`; admin `app/ponto/admin.py:44`; painel `app/templates/ponto/partials/painel_ausencia.html:125`, `:149`, `:299`, `:313`; trilha criação `app/ponto/services/ausencia.py:446`; relatório atestados `app/ponto/services/atestados_lote.py:128`; rateio/desconto folha `app/folha/export.py:110`, `:129-131`.

8. **Selo/RED:** os testes dos dois sítios mordem a reversão para720 por expectativas420/660/240/0 e nascimento0 (`app/chamados/tests/test_h06_falta_pela_grade.py:86-122`). O teste range passa se apenas a cura dos dois sítios for revertida porque chama a porta diretamente (`:129-134`), portanto é teste do chokepoint preexistente. O selo AST pega a reversão literal exata de ambos os sítios e do mapa (`app/chamados/tests/test_h06_selo_minutos_sem_literal.py:36-47`, `:71-74`), mas aceita variável livre sem rastrear sua origem (`:77`), portanto `minutos=int(720)` ou variável constante escapam ao predicado literal de `:38-39`. Não houve execução/mutação de testes [nao verificado].

9. **Cadastros atuais decidindo passado, no alcance:** vínculo é selecionado pela data com `vinculo_do_dia` (`app/escala/utils.py:2197-2215`), mas sem DNA de marcos o histórico cai no `TipoEscala` vivo (`app/escala/models.py:1124-1133`); sem célula, dia de trabalho cai na aritmética/template vivo (`:1094-1096`, `:1271-1292`). Isenção atual vem da flag atual (`app/core/constants.py:13-18`) e muda todos os dias de período passado em `app/ponto/services/espelho.py:599-600`, `:669-670`; fechamento elimina furos do isento atual em `app/ponto/services/fechamento.py:355-358`; grade aplica isenção atual ao período em `app/escala/utils.py:903-910`. Empresa atual determina janela em `app/ponto/services/fechamento.py:612`; situação/data_demissão atuais determinam universo em `:423-435`. Regime/posto além do censo H12 do outro agente: [nao verificado].

10. **Relógio fora da borda no alcance:** `Ausencia.save` lê `timezone.now` em `app/ponto/models.py:871-873`; criação lê `localdate` para prazo de atestado em `app/ponto/services/ausencia.py:338-339` e `now` para aprovação em `:349`; materialização lê `now` para carimbar irmãs em `app/chamados/services/materializacao.py:529`, `:549`. Esses usos preexistem; o diff H06 passa a depender deles com minutos derivado, sem introduzir leitura nova nesses trechos.

11. **Reenvio não rederiva legado:** materialização pede `retornar_mesmo_tipo` (`app/chamados/services/materializacao.py:490`, `:526`), que devolve registro existente sem alteração (`app/ponto/services/ausencia.py:294-301`); H06 não migra atestado legado nem falta ainda aberta com720 por esse caminho, e o comando filtra somente falta aprovada (`app/ponto/management/commands/rederivar_falta_declarada.py:50-52`). Isso é limite concreto do alcance, não prova de incidência em produção [nao verificado].

## 05/11 15:09 · sessao 100dd96cd839 · - · - · origem=Ronald (tmux)

A máscara incide no **conteúdo do TXT**, conforme `('folha', 'ExportacaoDominio'): ('conteudo', 'invalidada_motivo')` em `bin/sombra_regras_pessoais.py:105`. O transformador converte texto longo em `… <sha1[:10]>`, preservando apenas valores especiais definidos na regex (`bin/sombra_regras_pessoais.py:21`, `bin/sombra_regras_pessoais.py:29`, `bin/sombra_regras_pessoais.py:33`). A aplicação aos objetos e a gravação ocorrem em `bin/sombra_mascara_pessoal.py:44`, `bin/sombra_mascara_pessoal.py:52`, `bin/sombra_mascara_pessoal.py:61`.

O fluxo completo restaura os dumps e chama essa máscara (`bin/sombra.sh:257`, `bin/sombra.sh:259`, `bin/sombra.sh:272`); o incremental também a chama (`bin/sombra.sh:204`).

Não há alteração de `codigo_dominio` nas regras de máscara examinadas; a máscara inicial altera CPF, nome, email pessoal e token (`bin/sombra_mascara.py:66`). A matrícula utilizada no TXT provém de `codigo_dominio`, com `strip().zfill(10)` (`app/folha/export.py:709`).

**Contraexemplo:** nome/CPF mascarados não mudariam a matrícula usada pelo exportador (`app/folha/export.py:709`); substituir o conteúdo inteiro por um hash elimina todos os registros reconhecíveis. O leitor oficial exige prefixo `10` e 43 caracteres; caso contrário, devolve `None` (`app/folha/export.py:236`, `app/folha/export.py:244`).

**Certeza:** alta para o mecanismo existente no código. O objeto de exportação efetivamente lido e o parser da sonda concreta são **[nao verificado]**; os scripts dos anexos não foram encontrados na árvore **[nao verificado]**.

## 05/11 15:09 · sessao da4a438b21a9 · - · - · origem=Ronald (tmux)

Achado principal: o lote altera a validade temporal do “não tocou”. `_admin_tocou_o_dia_depois` guarda o conjunto inteiro por `desde` e devolve sua ausência sem reler; avulso relê enquanto não houver SIM. Isso independe de `pergunta_viva` e `_pergunta_viva_calc` permanecerem textualmente intactos. (`app/chamados/juizes.py:1041-1052`, `app/chamados/juizes.py:1167`)

Contraexemplo construído, ocorrência em produção [não verificado]: PF julho fechada em t0; pergunta P1 de col001/10-07 aquece lote sem toque; durante a rodada admin cobra col001/11-07, gravando `cobrar_dia_manual`, `modelo=Colaborador`, descrição `dia=2026-07-11`, criado_em > t0; pergunta P2 fresca desse dia tem célula acusando, sem validação. Lote devolve morta por ausência no conjunto antigo; avulso encontra o toque e chega à célula acusadora, devolvendo viva. (`app/chamados/juizes.py:1041-1052`, `app/chamados/services/cobrar_dia.py:37-38`, `app/chamados/juizes.py:1163-1168`, `app/chamados/juizes.py:1208-1211`)

Mapa:

- Memo: dono definido em `_memo_da_tranca`, lote prevalece sobre request e colaborador; criação `_tranca_memo` em `975`; chaves empresa/tranca escritas e lidas em `939-947`; toque avulso em `1046-1051`; conjunto lote em `1042-1045`; sistema em `1074-1078`. (`app/chamados/juizes.py:938-975`, `app/chamados/juizes.py:1042-1078`)
- ContextVar: declarado `985`; `tranca_em_lote` consulta `991`, escreve `994`, restaura em finally `998`; `_memo` consulta `968`; `_admin` consulta `1041`; chamador produtivo é comando `vigiar_fio_morto:171`. (`app/chamados/juizes.py:968`, `app/chamados/juizes.py:985-998`, `app/chamados/juizes.py:1041`, `app/chamados/management/commands/vigiar_fio_morto.py:171`)
- Contexto request separado: declarado em `core/observ.py:19`, escrito/restaurado pelo middleware `78,82`, lido pelo memo e evento; o lote não impõe guarda contra request/escrita, mas a cópia não demonstra chamador request abrindo lote. (`app/core/middleware.py:78`, `app/core/middleware.py:82`, `app/core/observ.py:108-115`, `app/chamados/juizes.py:968-975`, `app/chamados/juizes.py:989-998`)
- Sistema: `core/utils.py:18` usa `get_or_create(username='sistema')`; leitura pode criar usuário ausente, comportamento herdado. Command garantidor escreve defaults em `13-23`, senha em `25-26`, lê e normaliza atributos em `33-41`. (`app/core/utils.py:18`, `app/core/management/commands/garantir_user_sistema.py:13-41`)
- Trilhas: Cobrar `services/cobrar_dia.py:37-38`; Resolver `ponto/services/veredito_celula.py:334-336`; Desarquivar `views_arquivar.py:77-79` → `services/arquivo.py:208-209` → `_trilha:137-138`. Todos chegam a `core.observ.evento`, escritor em `133-137`; definição dos campos em `core/models.py:295-307`. (`app/core/auditoria.py:22-24`, `app/core/observ.py:133-137`)
- Chamado.colaborador/celula: definição `models.py:32,44-45`; nascimento `abrir:913-932` ou `criar:1026-1048`, célula resolvida por `_celula_de:1174-1190`. Pós-nascimento `services/vinculos.py:92-99` só preenche NULL. Backfill `rejuncao.py:59-80` anexa célula ausente pelo mesmo colaborador/dia; isso não demonstra mutação de identidade histórica da célula. Símbolo `ancorar_chamado` [não verificado].
- CelulaDia.colaborador/data: definição `escala/models.py:1681-1685`; nascimento pela porta `ponto/portas/celula.py:247-248` e `601-604`. Helper avulso consulta identidade atual em `juizes.py:1085-1094`; lote reconstrói-a em `1104-1110`. Escritor produtivo trocando esses dois campos após nascimento [não verificado].
- Portas de escrita que leem C6: universo da ressuscitação `models.py:1421-1447`; escritor `ressuscitar_perguntas:1481-1493`; `PerguntaDisputa.carimbar` aplica estado em memória `2575`, consulta C6 `2585`, recusa ou salva `2589-2598`. Desarquivar chama reabrir antes do toque e repete ressuscitação depois dele. (`app/chamados/views_arquivar.py:70`, `app/chamados/views_arquivar.py:77-86`)

Cadastro de hoje decidindo passado é herdado: C6 usa `colab.empresa` atual e corte atual da empresa para classificar `p.data_evento`; transferência altera empresa em `views.py:1384-1388`, edição direta em `1313`, corte é alterável em `495-523` pela porta `services/estrutura.py:17-20`. Exemplo construído: dia 10/07/2026, julho trancado/agosto aberto, corte 21→10 hoje: o mesmo passado passa de julho para agosto. (`app/chamados/juizes.py:932-945`, `app/ponto/janelas.py:24-33`, `app/ponto/janelas.py:50-63`) Ocorrência real [não verificado].

Teste de vazamento não abre lote nem instala request: começa direto com pergunta avulsa, verifica ContextVar vazio e escreve toque. Portanto não prova entrada/saída/finally/reentrância nem vazamento após um lote; equivalência estática também não intercala toque durante lote. (`app/chamados/tests/test_snc2_toque_em_lote.py:92-124`)

## 05/11 15:09 · sessao 667df4f15e1b · - · - · origem=Ronald (tmux)

Achados do comando e da reversão, por inspeção estática:

1. **Reversão escreve em competência entregue.** `handle` desvia diretamente para `_reverter` antes de carregar empresa/mês e antes da guarda `competencia_entregue`; `_reverter` busca somente pelo PK e chama a reposição. A reposição exige motivo e salva minutos, sem verificar competência. Exemplo sintético: falta de 22/10/2026 rederivada na comp. 11 aberta; depois da entrega da 11, o mesmo arquivo ainda restaura 720. `app/ponto/management/commands/rederivar_falta_declarada.py:93`, `:114`, `:158`, `:161`; `app/ponto/services/ausencia.py:528`, `:531`.

2. **A segunda aplicação pode destruir a reversão da primeira.** O nome usa resolução de um segundo e `open(..., 'w')`; após primeira aplicação transformar 720 em 420, segunda aplicação no mesmo segundo encontra zero alvos e sobrescreve o mesmo arquivo com `[]`. O teste confirma apenas minutos/idempotência numérica, sem conferir que o arquivo original sobrevive à segunda chamada. `app/ponto/management/commands/rederivar_falta_declarada.py:51`, `:122`, `:124`, `:125`; `app/ponto/tests/test_h06_rederivar_falta_declarada.py:89`, `:93`, `:95`.

3. **Não há proteção contra entrega concorrente nem mudança concorrente da ausência.** O comando obtém objetos no censo, verifica entregue depois e salva os objetos antigos sem transação abrangente, lock ou nova leitura antes de editar; a porta executa `ausencia.save()` integral. Uma entrega entre a verificação e o save permite a escrita posterior; uma edição humana posterior ao censo pode ser sobrescrita. É caminho possível pelo código, ocorrência em produção [nao verificado]. `app/ponto/management/commands/rederivar_falta_declarada.py:101`, `:114`, `:130`, `:132`; `app/ponto/services/ausencia.py:496`, `:507`, `:508`.

4. **A guarda é da competência pedida, mas a ausência pode atravessar outras.** A seleção limita apenas `data`, depois soma até `data_fim`; a edição regrava a ausência inteira. Exemplo sintético: falta 720 entre 20 e 22/10/2026; `--mes 10 --aplicar` verifica somente a 10, mas muda um registro que também cobre a 11 eventualmente entregue. Inversamente, esse registro não entra no `--mes 11` porque iniciou antes de 21/10. `app/ponto/management/commands/rederivar_falta_declarada.py:49`, `:52`, `:56`, `:58`, `:114`, `:132`; `app/ponto/services/ausencia.py:477`, `:499`.

5. **O comando permite apply na 10.** O argumento mês é livre; a única restrição temporal de aplicação é entregue. O teste inclusive manda aplicar a 10. Isso permite ação além da instrução dada no pedido: apply 11; 10/09 Pauta DP [instrução do usuário, sem arquivo:linha]. `app/ponto/management/commands/rederivar_falta_declarada.py:74`, `:114`; `app/ponto/tests/test_h06_rederivar_falta_declarada.py:84`.

6. **A porta de reversão não comprova que está revertendo o ato.** O JSON grava PK, colaborador, data, minutos e grade; na leitura só usa PK/minutos. Não compara colaborador/data/grade, valor atual, tipo/status ou relação com aplicação anterior. A função pública aceita diretamente qualquer inteiro e só valida motivo. Arquivo antigo pode repor 720 sobre uma correção humana posterior. `app/ponto/management/commands/rederivar_falta_declarada.py:125`, `:158`, `:161`; `app/ponto/services/ausencia.py:518`, `:528`, `:531`.

7. **Reversão gravada antes: sim, no caminho serial normal do apply; garantia durável: não.** O arquivo fecha antes da primeira edição, mas pode ser sobrescrito conforme achado 2. `_reverter` escreve sem gravar reversão do estado que está prestes a substituir. `app/ponto/management/commands/rederivar_falta_declarada.py:124`, `:127`, `:132`, `:158`, `:161`.

8. **Foto não prova identidade de Ausencia/TXT e a falha é posterior às escritas.** `foto` lê campos de `FechamentoMensal`; não fotografa Ausencia ou conteúdo/hash do TXT. As fotos ficam em memória; só hashes resumidos são impressos. Se diferirem, o comando lança erro depois do loop, sem desfazer as mudanças anteriores. `app/ponto/management/commands/carimbo_gravado.py:34`, `:39`, `:53`; `app/ponto/management/commands/rederivar_falta_declarada.py:127`, `:132`, `:140`, `:145`.

9. **A trilha não é condição da escrita.** Apply e reversão salvam antes de `evento`; `evento` engole falhas e retorna `None`. Há caminho de mudança sem `LogAuditoria` gravado, embora a aplicação tenha arquivo de reversão normal. `app/ponto/management/commands/rederivar_falta_declarada.py:132`, `:135`; `app/ponto/services/ausencia.py:532`, `:533`; `app/core/observ.py:94`, `:105`, `:133`, `:139`.

10. **Grade zero recebe tratamento diferente no passivo.** O comando preserva 720 e apenas imprime `CADASTRO`, enquanto a derivação normal de falta aprovada soma grade zero e grava zero. Exemplo sintético: sem vínculo em 07/10/2026, falta antiga aprovada720/com chamado permanece720 no apply; falta nova aprovada sem minutos fica0. Esse contraste é explicitamente selado pelo teste do comando. `app/ponto/management/commands/rederivar_falta_declarada.py:60`, `:63`, `:103`; `app/ponto/models.py:877`, `:885`, `:887`; `app/ponto/tests/test_h06_rederivar_falta_declarada.py:122`, `:130`.

11. **Selo de minutos não protege a reversão nem todo escritor.** O AST verifica somente chamadas cujo nome termina em `criar_ausencia` com literal e `_DECLARA_FALTA_MAPA` contendo inteiro. Atribuição direta a `.minutos`/save, `editar_ausencia_registro` e `repor_minutos_de_reversao` passam pela varredura. O teste de “mordida” cobre as formas específicas antigas, não essas saídas. `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:33`, `:36`, `:46`, `:70`; `app/ponto/services/ausencia.py:531`.

12. **Não classificar a mera existência de `repor_minutos_de_reversao` como escritor “fora da porta”.** Está dentro do serviço como porta de exceção, e essa forma é prevista pela regra local; o defeito concreto é não limitar/verificar a exceção, conforme achado 6. `app/ponto/services/ausencia.py:518`; `CLAUDE.md:32`.

13. **Relógios no alcance:** o comando lê relógio na borda para nome de arquivo; a gravação chama `Ausencia.save`, que lê `now` para carimbo de decisão quando ausente, e sinais calculam `hoje` internamente. `app/ponto/management/commands/rederivar_falta_declarada.py:122`; `app/ponto/models.py:872`; `app/ponto/signals.py:115`, `:122`, `:143`.

14. **Cadastro atual no comando/foto:** alvo escolhe empresa via `colaborador__empresa`, foto também filtra fechamento pela empresa atual do colaborador; transferência de empresa muda o universo de uma competência antiga. `app/ponto/management/commands/rederivar_falta_declarada.py:50`; `app/ponto/management/commands/carimbo_gravado.py:53`.

Não executei testes nem escrevi arquivos; execução real, sombra 09/10/11, hashes e aplicação efetiva [nao verificado]. [nao verificado]

## 05/11 15:09 · sessao 7a5540af951a · - · - · origem=Ronald (tmux)

Escritores e leitores relevantes:

| Campo | Escritor/porta | Leitor decisivo |
|---|---|---|
| `status_local` | `_gravar_estado`, `app/chamados/models.py:222`; registra `estado_chamado` antes de salvar, `:275-279` | `_decisao_do_fechamento`, `:638-655`; motor `app/chamados/catalogo/motor.py:111`; `_gate_pergunta_viva`, `models.py:400-414` |
| `validada_em` | Validação humana: `_carimbar_validacao`, `app/chamados/services/validacao.py:127-133`; simetria de `carimbar(resposta='validar')`, `models.py:2576-2583`; limpeza por ressuscitação, `:1493` | Filtro das ancoradas, `models.py:327`; C6 mata se preenchida, `app/chamados/juizes.py:1078-1079` |
| `materializada_em` | `_escrever_carimbo`, `models.py:2545-2549`, persistido por `carimbar`, `:2598`; limpeza em lote, `:2652-2653` | C6 exige carimbo + marco aceso antes do veredito acusatório, `juizes.py:1119-1126`; absolvição por veredito não acusatório sem pendentes, `:1136-1138` |
| Via `celula_absolveu` | `retratar_perguntas_absolvidas_por_celula`, `app/chamados/reconciliador.py:549`; consulta C6 `:610`, tranca `:616`, chama `carimbar` `:620` | Mesmo C6; sem necessidade de validação humana, `juizes.py:1136-1138` |
| Via `fantasma` | `encerrar_orfas_pela_lampada`, `reconciliador.py:1080`; `encerrar_fantasma`, `:1115` → `models.py:2666-2674` → `carimbar` | `veredito_orfa` exige marco aceso, pergunta viva, dia passado e encerrado, `reconciliador.py:1050-1060` |

`reabrir()` chama `ressuscitar_perguntas()` em `models.py:1391`; a porta limpa `materializada_em`, autor/via, `validada_em`, autor e `respondida_em` em `:1493`/`:2652-2653`. Ela poupa marcos acesos (`:1441-1443`) e só devolve perguntas que o C6 daria por vivas sem carimbos (`:1444-1447`). A limpeza efetivamente ocorrida na produção não está individualizada no recorte [nao verificado].

**#35623**

- LOG 792831 (`resolvido → em_analise`, via vazia) corresponde, na cópia, a `reabrir()` → `_gravar_estado`, `models.py:1380-1382`. `analisar()` não explica essa origem, pois só aceita aberto/registrado, `motor.py:112`. Chamador externo concreto da reabertura [nao verificado].
- LOGs 792643 e 792671 (`via=sistema`, resolvido) correspondem a `reconciliar()` → `_gravar_estado`, `models.py:617-620`, `motor.py:95`.
- Nenhuma recusa documentada: sonda `trilha fechamento_recusado n=0`. Hoje há duas vivas reatáveis para #35794; o plano permitiria reata em `models.py:369-377` e o gate a executaria em `:415-430`. A sonda não mediu `reconciliaria`, pois terminou em `InternalError`; isso não prova recusa do juiz.
- PG51720: `_pergunta_materializada_de_fato=False` não implica viva. A sonda mostra `materializada_em` preenchida + `fato_do_marco_no_espelho=True`; C6 já retorna morta em `juizes.py:1119-1121`, antes de chegar ao teste final `:1155-1156`. [nao verificado]
- PG51561/53192: sem carimbo, marco apagado e veredito `furo` acusatório; C6 retorna viva em `juizes.py:1123-1126`.

**#33126**

- Escritor atual: `reconciliar()` → `_gravar_estado`, `models.py:617-620`/`:222`, sustentado por LOG 890440 (`aberto → resolvido`, `via=sistema`).
- Tentativa recusada: gate `_gate_pergunta_viva`, `models.py:400-414`, registrador `_registrar_recusa`, `:381-391`, sustentados por LOG 888606: `pergunta_viva_na_competencia_aberta`, PG46857/46858/46859. O plano recusa quando não acha receptor válido para todas as vivas, `:369-376`. Chamador externo da tentativa [nao verificado].
- Estado atual das quatro perguntas: `celula_absolveu`, sem validação ou resposta, sonda PG46856–46859. Porta correspondente: `reconciliador.py:549-620`; C6 hoje as absolve por `concorde`, sem pendentes, `juizes.py:1136-1138`.
- Não se pode atribuir a recusa histórica a carimbo ausente: o C6 atual já mata pergunta sem carimbo por veredito não acusatório sem pendentes. Estado da ata/veredito no instante do LOG888606 [nao verificado].

**#36045**

- Escritor atual: `reconciliar()` → `_gravar_estado`, `models.py:617-620`/`:222`, sustentado por LOG895879 (`aberto → resolvido`, `via=sistema`).
- Tentativa recusada: `_gate_pergunta_viva` → `_registrar_recusa`, `models.py:400-414`/`:381-391`, sustentada por LOG895771, PG52102 viva.
- Recusa em 04/11 11:29:35 UTC; materialização `fantasma` em 04/11 11:30:10 UTC. São tempos explicitamente dados pela sonda. Depois do carimbo, marco `hf` aceso permite C6 retornar morta em `juizes.py:1119-1121`, apesar de `fato_em_ausencia` acusar (`:1123-1126`). O estado exato da ata no instante da recusa [nao verificado].
- Via fantasma não planta Batida nem toca resposta, `models.py:2666-2674`; consumidor `reconciliador.py:1080-1123`.
- O `decidir=False None` atual de #33126/#36045 é idempotência: `RECONCILIAR` não tem entrada para `resolvido`, `motor.py:111`/`:161-163`; não é recusa por furo ou pergunta.

## 05/11 15:08 · sessao 025ff3c39b6e · - · - · origem=Ronald (tmux)

1. O command abre o lote ao redor de `censo`, `censo_alcance`, `viva_em_disputa_fechada`, `respondida_sem_resposta` e `materializada_viva`; impressão e carimbo ficam depois da saída do contexto (`app/chamados/management/commands/vigiar_fio_morto.py:171`, `:181`, `:210`). `respondida_sem_resposta` não consulta C6: lê texto e carimbo diretamente (`app/chamados/services/vigia_fio.py:781`).

2. Cadeias de leitura:
   - `censo → fio_do_dia → fio_vivo → pergunta_viva`: `app/chamados/services/vigia_fio.py:397`, `:416`, `:165`; `app/chamados/juizes.py:1269`.
   - `censo_alcance → alimentar_portas → pergunta_viva`: `app/chamados/services/vigia_fio.py:603`; `app/chamados/juizes.py:1327`.
   - `porta_alcancavel_do_dia → _no_app → perguntas_do_app → pergunta_viva`: `app/chamados/juizes.py:1407`, `:1346`; `app/chamados/services/pergunta_no_app.py:53`.
   - Contadores adicionais consultam C6 em `app/chamados/services/vigia_fio.py:754` e `:812`; `materializada_viva` delega ao segundo em `:849`.
   - A função tocada também serve request real: `api_disputa_perguntas → perguntas_do_app` (`app/api/views.py:2534`, `:2551`). Esse caminho não abre lote (`app/api/views.py:2552`; `app/chamados/services/pergunta_no_app.py:42`).

3. **Selo sem mordida demonstrável por inspeção:** `test_lote_nao_vaza_e_request_segue_guardando_so_o_SIM` nunca abre lote nem cria request; verifica apenas o contexto inicialmente vazio e duas leituras fora dele (`app/chamados/tests/test_snc2_toque_em_lote.py:116`). Passaria com a cura revertida, pela lógica avulsa prévia fornecida no diff [não verificado por execução]. Também não detectaria uma mutação que guardasse o NÃO somente quando `REQUISICAO_DA_TRILHA` tivesse request: esse contexto só é instalado pelo middleware, ausente no teste (`app/core/middleware.py:76`; `app/chamados/tests/test_snc2_toque_em_lote.py:118`).

4. O teste de paridade também aceita reversão: `_lote()` usa `nullcontext` quando a cura inexiste, de modo que compara dois caminhos avulsos (`app/chamados/tests/test_snc2_toque_em_lote.py:22`, `:74`, `:112`). **O teste de desempenho tem mordida distinta** e deve falhar sem o lote, porque compara contagens de uma e cinco perguntas (`app/chamados/tests/test_snc2_toque_em_lote.py:86`) [não verificado por execução]. Nenhum dos três testes novos chama o command; remover somente sua abertura do lote não muda o código exercitado por eles (`app/chamados/tests/test_snc2_toque_em_lote.py:74`, `:79`, `:116`; `app/chamados/management/commands/vigiar_fio_morto.py:171`).

5. **Vazamento:** a restauração usa `finally/reset`, inclusive após exceção; não há evidência de vazamento para chamadas posteriores no mesmo contexto (`app/chamados/juizes.py:994`). Há exposição condicional dentro do contexto: `_memo_da_tranca` prioriza lote sobre request; `tranca_em_lote` não verifica request nem capacidade de escrita; qualquer request/porta chamado dentro dele receberia o cache negativo (`app/chamados/juizes.py:968`, `:989`, `:1041`). Um chamador real de produção fazendo isso não foi encontrado [não verificado].

6. **Classes anteriores:** C6 continua registrado como `pergunta_viva` (`app/core/juizes.py:112`, `:266`); seu corpo e `_pergunta_viva_calc` não aparecem nos hunks fornecidos [não verificado contra Git]. Há relógio interno preexistente: C6 chega a `_dentro_da_janela_viva`, que lê `date.today()`, apesar da borda única do command (`app/chamados/juizes.py:1224`, `:563`; `app/chamados/management/commands/vigiar_fio_morto.py:159`). O calendário chamado pelo vigia também lê seu próprio relógio (`app/chamados/services/vigia_fio.py:269`; `app/colaboradores/services/calendario.py:132`, `:163`). O cron conserva dependências declaradas de `processar_cartorio` e `reconciliar_perguntas_orfas`; nenhuma mudança de ordem de trava ou pouso consta no diff dado (`app/config/crons.py:1680`). Execução de pré-requisitos e pouso efetivo [não verificado].

## 05/11 15:08 · sessao 212818460d73 · - · - · origem=Ronald (tmux)

H12 usa o juiz da janela: `app/folha/export.py:796` → `:47-51` → `app/ponto/janelas.py:54-57`. O gate novo compara `data_demissao <= fim`, inclui o último dia e exclui a demissão posterior do motivo de rescisão; não encontrei segundo juiz novo nesse trecho. `app/folha/export.py:847-852`.

- **Escritor do motivo:** `classificar_export`, `app/folha/export.py:848`; inicialização do item em `:818-830`. O TXT lê `status='entra'` em `:695-707` e monta linhas em `:719-725`.
- **Leitores específicos do motivo:** `app/folha/views.py:22`, `:594`, `:598`; `app/folha/retrato_folha.py:33`, `:93`, `:364`, `:378`, `:414`; `app/ponto/services/fechamento_lista.py:25-37`, `:70`, `:97-99`; `app/relatorios/views.py:1658-1669`.
- **Outros consumidores da classificação:** `app/api/views_mensageria.py:283`; `app/folha/previa.py:43`; `app/folha/fora_da_folha.py:119`, `:215`; `app/folha/validacao_pdf.py:65`; `app/folha/services/vigia_txt.py:231`; `app/folha/porta_export.py:469`; `app/inteligencia/calculadores.py:210`, `:271`; `app/relatorios/views_pend.py:58`; `app/ponto/services/dias_em_aberto.py:104`; `app/folha/views.py:212`, `:331`, `:858`.
- **Travas:** são calculadas antes da saída por rescisão, preservadas no item; ordem `conta_vazia`, `turno_aberto`, `sem_ancora_12x36`. `app/folha/export.py:811-829`. Consumidores da aprovação usam essas travas, não o motivo: `app/ponto/views.py:1297-1313`, `:1507-1516`.
- **Escritores de `data_demissao`:** agendamento `app/colaboradores/services/desligamento.py:84-86`; desligamento efetivo `:104-108`; reativação apaga a data `:210-214`. Porta de persistência: `app/colaboradores/services/colaborador.py:32`, `:55-61`, `:73-76`.
- **Leitores históricos adicionais de demissão:** `app/folha/export.py:1049`, `:1068-1069`, `:1189-1190`, `:1218`; `app/folha/views.py:612-613`; universo de fechamento em `app/ponto/services/fechamento.py:423-435`.

Achados no alcance:

1. **Relógio fora da borda:** `classificar_export` chama retenção sem injetar `hoje`; o caminho termina em `localdate()` e usa o resultado para julgar célula ausente. `app/folha/export.py:949`, `:1255-1256`, `:1186`, `:1220`, `:1244`. É preexistente no alcance.
2. **Cadastro atual decide passado:** empresa atual escolhe o universo (`app/folha/export.py:767`); situação atual e escala ativa decidem trava de âncora (`:787-793`); demissão atual decide rescisão histórica (`:847`); corte atual determina janela (`app/ponto/janelas.py:50-57`).
3. **Isenção atual apaga eventos históricos:** `app/folha/export.py:406-408`, lendo o booleano atual em `app/core/constants.py:18`; também elimina retenção histórica em `app/folha/export.py:1180-1186`.
4. **Exemplo histórico concreto, por construção:** pessoa demitida em **10/07/2026**, reativada em **05/11/2026**, tem a demissão apagada pela porta; ao classificar novamente a competência **07/2026**, deixa de cair em rescisão e, se os demais gates estiverem limpos, pode voltar ao TXT. `app/colaboradores/services/desligamento.py:210-214`; `app/folha/export.py:847`, `:695-725`. É dívida preexistente no alcance, não mudança adicional comprovada do H12.
5. **Último dia não é contraexemplo:** empresa corte21, competência07, demissão **20/07/2026** → rescisão; corte16, demissão **15/07/2026** → rescisão. Código em `app/folha/export.py:847`; casos em `app/folha/tests/test_rescisao_modulo_proprio.py:51-76`, `:79-83`.
6. **Demissão futura posterior à janela não contraria o aval:** cadastro sintético com demissão **06/07/2027**, competência **11/2026**, corte21, não recebe motivo de rescisão; isso não garante inclusão no TXT, pois os demais gates continuam existindo. `app/folha/export.py:847`, `:853-960`.
7. **Mordida dos testes:** antes/dentro/último continuam passando com o gate antigo; o caso depois falha com a cura revertida. `app/folha/tests/test_rescisao_modulo_proprio.py:66-76`. Portanto, a coleção H12 tem mordida estática demonstrável; execução não realizada.
8. **Provas operacionais:** aval local exige sombra09/10/11, hash09 igual e tabela de parada; o prompt não fornece resultados dessas provas. Execução e hash: **[nao verificado]**. Exigência documentada em `app/docs/PROMPTS.md:1860`.

## 05/11 15:08 · sessao 5263bd9aeab5 · - · - · origem=Ronald (tmux)

Achados com referências:

- **Escritor da ata:** `cartorio.lampadas_do_dia()` constrói as lâmpadas; no ramo principal, `acesa = isinstance(x, dict) and not x.get('missing')`, sem exigir hora real, e `luz = x.get('hora')`. Portanto `acesa=True, luz=None` é admitido pelo código. `ata_do_dia()` põe a lista em `ata['lampadas']`. Fontes: `app/ponto/services/cartorio.py:647`, `:677`, `:701`, `:704`.
- **Persistência:** `lavrar_veredito()` escreve `cel.ata` em `app/ponto/portas/celula.py:775`; `salvar_e_emitir()` salva e emite `EventoCelula` no mesmo atomic quando os campos incluem ata/veredito, em `:630`, `:642`, `:648`, `:650`.
- **Outra fonte para lâmpada acesa:** ausência aprovada de dia inteiro ou justificativa pode produzir `acesa=True`, `luz=None`, `dispensada=motivo` em `app/ponto/services/cartorio.py:766` e `:772`. Parcial pode chegar já dispensada da grade (`app/escala/utils.py:1267`); isenção preenche o previsto sem fabricar batida (`app/escala/utils.py:842`, `:858`). **Qual fonte concreta atuou nos três casos: [nao verificado]**, pois a sonda não imprime os marcadores `dispensada/isencao`, ausência, edições ou histórico da escala.

**Fechamento esperado por evento:**

- Cartório pergunta pelo marco nomeado, não pelo dia inteiro: `app/ponto/services/cartorio.py:1461`; quem não protesta por ter todos os marcos acesos entra no `reconciliar()` em `:1482` e `:1488`. Com fio, a ordem é lavrar → E2 → reconciliar, código em `:953`, `:958`, `:963`.
- Consumidor do evento tenta a absolvição do chamado em `app/chamados/reconciliador.py:1326`, depois encerra perguntas órfãs pela lâmpada em `:1329`.
- A absolvição inteira exige veredito fora de `VEREDITOS_ACUSAM` e ausência de marcos pendentes (`app/chamados/reconciliador.py:994`). Assim #33126 (`concorde`) cabe nesse caminho; #36045 (`fato_em_ausencia`, sonda `veredito acusa? True`) depende do caminho por marco, não dessa absolvição do dia. Executor concreto do fechamento: [nao verificado].
- #36045: a sequência recusa às 11:29:35 → pergunta `fantasma` às 11:30:10 → estado resolvido é compatível com os gates e com o consumidor, mas a sonda não identifica o evento/call stack executor: [nao verificado].
- #35623: os marcos cobrados 11:00/S e 12:00/E estão apagados; `marcos_acesos=False`. O código exige todos (`app/ponto/services/lastro.py:174`); não existe obrigação de fechar por essas lâmpadas no retrato dado.

**Nota “a batida ... EXISTE”:**

- `lastro.julgar()` aceita lâmpada acesa sem consultar Batida e usa o horário previsto como texto quando `luz` é vazio: `app/ponto/services/lastro.py:289`, `:292`. Logo a prova “lâmpada acendeu” não prova “existe Batida”.
- `lastro.fechar()` relê status, exige vivo, chama `reconciliar()`, e **só se retornar True** anexa a nota de tipo `lastro_encontrado`: `app/ponto/services/lastro.py:338`, `:345`, `:347`.
- Os três blocos fornecidos em `ocorrencias/notas:` contêm apenas LOG; nenhuma ocorrência/nota foi mostrada. Anexação da nota nos três casos: **[nao verificado]**. `estado_chamado via=sistema` não identifica `lastro.fechar`, porque vários chamadores usam o mesmo reconciliador/escritor (`app/chamados/reconciliador.py:523`; `app/ponto/services/cartorio.py:1488`).
- #33126: batidas fornecidas são 20/10 e 22/10; nenhuma de 21/10 no marco E07:00 foi mostrada. #36045: S13:40:53 existe em 30/10; nenhuma S16:00 de 30/10 foi mostrada, embora o marco esteja aceso sem luz. A fonte das luzes sem hora real e eventual atribuição de S13:40 ao marco final permanecem **[nao verificado]**.

_1554 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
