# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 23:12** · publicado em **16/11/2026 23:11** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 23:12 · sessao ecdfda28a78b · - · - · origem=Ronald (tmux)

Censo feito por `rg` em todo `app/` e AST de todos os `.py`, incluindo filtros ORM, strings de campos, `getattr`, aliases/imports e templates; sem execução de Django, suite ou banco. Nenhuma ocorrência dos campos/funções F2 apareceu em arquivo `.js`; cobertura de nomes construídos dinamicamente não demonstrável: [nao verificado]. [nao verificado]

Convenção abaixo: caminhos relativos a `app/`; **NOVO** significa ausente da enumeração do brief/matriz fornecidos, não necessariamente introduzido entre os hashes. Linhas são desta cópia.

**Escritores da pergunta, com persistência e escritores apenas em memória**

| Campo | Escritor real e forma |
|---|---|
| R/T no nascimento | `chamados/models.py:2949` valida `campos.get('respondida_em')`/`resposta_colab`; `_nascer` recebe `**campos`, instancia e salva em `chamados/models.py:3124`, `chamados/models.py:3127`; conserva atos em `chamados/models.py:3129`. **NOVO no censo**. |
| R/T resposta humana | `responder_pergunta_disputa_core`: atribuições `chamados/services/disputa_emissao.py:1481`, `:1482`; `save` em `:1483`; DP RESPOSTA em `:1525`. |
| R/T automática | `responder_automaticamente`: `chamados/services/disputa_emissao.py:1674`, `:1675`; `save` `:1677`; DP RESPOSTA `:1678`; retorna `False` sem sobrescrever texto em `:1663`, `True` em `:1680`. |
| R só | `carimbar_resposta_pela_trilha`: `chamados/services/disputa_emissao.py:1696`, save `:1698`, DP RESPOSTA `:1699`; guarda idempotência `:1692`. **NOVO**. |
| R/V/Vpor pelo carimbo | `PerguntaDisputa.carimbar`: `chamados/models.py:3242`, `:3245`, `:3246`; somente com resposta não vazia `:3240`; salva `:3262`; recusa viva em `:3253`. |
| V/Vpor validação | `_carimbar_validacao`: `chamados/services/validacao.py:168`, `:169`; save `:171`; DP VALIDACAO `:173`. |
| V/Vpor resposta automática classe A | `responder_pergunta_disputa_core`: `chamados/services/disputa_emissao.py:1494`, `:1495`; save `:1496`. |
| V/Vpor devolução | `devolver_validacao_recusada`: `chamados/services/validacao.py:273`, `:274`; save `:276`; DP DEVOLUCAO `:279`. **NOVO**. |
| V/Vpor devolução BUG-PAR | `devolver_paridade_bug_par_103`: descarimba em `chamados/services/validacao.py:346`; limpa V/Vpor em `:349`, `:350`; save `:351`; DP DEVOLUCAO com motivo BUG-PAR em `:354`. **NOVO**. |
| M/Mpor/via individual | `_escrever_carimbo`: atribui trio em `chamados/models.py:3209`, `:3210`, `:3211`; persistidores são `carimbar` `:3262`, `carimbar_identidade` `:3299`, `descarimbar` `:3322`. |
| M/Mpor/via em lote | `descarimbar_em_lote`: `update` direto `chamados/models.py:3391`, `:3393`; exclui terminais `:3392`; devolve inteiro da operação ORM. |
| Lote zera também R/T/V/Vpor | `ressuscitar_perguntas`: chama lote em `chamados/models.py:1629`, `:1630`; DP legado anterior em `:1613`; não escreve REABERTURA, conforme `:1622` e execução `:1629`. `reabrir_para_correcao`: campos `chamados/services/disputa_emissao.py:1609`; DP RECUSA/REABERTURA/foto em `:1631`; chama lote em `:1648`. |
| Só via | `encerrar_por_fato_no_espelho`: `chamados/models.py:3402`, save `:3403`; `relavra_via.relavrar`: `chamados/services/relavra_via.py:125`, save `:127`. |
| Só memória | Simulação da ressuscitação `chamados/models.py:1554`, `:1555`, `:1556`; rollback `_desfazer_em_memoria` `:3272`, `:3273`, `:3274`, `:3275`. |

`chamados/services/regularizacao_ext.py:199`, `:200`, `:201` escreve **RegularizacaoExterna**, não PerguntaDisputa; excluir esse falso positivo. A outra declaração `materializada_em/materializada_por` de `chamados/models.py:4152`, `:4153` também é de RegularizacaoExterna.

**DP: escritores, leitores e pré-requisito**

- Escritor físico: `DecisaoPergunta.registrar`, `chamados/models.py:4110`, `bulk_create` em `:4120`; retorno de `linhas_da_projecao_sem_linha` é lista de dicts em `:4095`, `:4106`; atos disponíveis em `:4024`, `:4025`; proibições de atualização/apagamento em `:3997`, `:4000`, `:4003`, `:4072`, `:4076`.
- RESPOSTA: `chamados/services/disputa_emissao.py:1525`, `:1678`, `:1699`; VALIDACAO `chamados/services/validacao.py:173`; DEVOLUCAO `:279`, `:354`; ABSORCAO `chamados/services/materializacao.py:1045`; RECUSA/REABERTURA `chamados/services/disputa_emissao.py:1631`; SILENCIO `chamados/models.py:2005`, `ponto/services/veredito_celula.py:195`, `chamados/management/commands/backfill_decisoes_pergunta.py:324`; RETRATACAO `chamados/models.py:2137`.
- Consumidores completos de `linhas_da_projecao_sem_linha`: `chamados/models.py:1613`, `:3129`; `chamados/services/disputa_emissao.py:1480`, `:1673`; `chamados/services/validacao.py:167`, `:272`, `:345`; `chamados/services/tarefa_do_dia.py:430`; comandos `backfill_decisoes_pergunta.py:67`, **NOVO** `backfill_fio_morto_e4b.py:141`.
- Leitores DP adicionais **NOVO**: `PerguntaDisputa.respostas_anteriores`, `chamados/models.py:3675`; motivo BUG-PAR na L2, `chamados/services/materializacao.py:275`, `:276`; prefetch `chamados/services/fio.py:263`; carga da tarefa `chamados/services/tarefa_do_dia.py:421`; fotografia `chamados/services/foto_e4b.py:280`; comando backfill `chamados/management/commands/backfill_decisoes_pergunta.py:99`, `:315`.
- Helpers c3 `_leitura_dos_atos`, `_q_validada_pelo_ato`, `_leitura_conservada`: nenhuma definição localizada nesta cópia; ausência de hash Git local: [nao verificado]. Não confundir `respostas_anteriores` existente em `chamados/models.py:3668` com helper c3.

**Todos os consumidores das funções F2, enumerados concretamente**

| Função | Consumidores produtivos |
|---|---|
| `PerguntaDisputa.estado` | `api/views.py:2550`; `chamados/services/explicador.py:95`; `chamados/services/fio.py:660`; `chamados/services/vigia_fio.py:259`; `templates/chamados/partials/_card_atendimento.html:189`. Os três primeiros já constavam; referências antigas de vigia caíram de linha/forma. |
| `respondida` | Property em `chamados/models.py:3664`, lê somente R `:3665`. Nenhum acesso produtivo `.respondida` localizado; a menção `chamados/juizes.py:1551` é docstring, não consumidor. Cobertura dinâmica [nao verificado]. |
| `resposta_display` | Já listado: `api/views.py:2562`, `templates/chamados/partials/_pergunta_card.html:31`; **NOVO** `api/views.py:1164`, `chamados/services/disputa_emissao.py:1560`, `_card_atendimento.html:106`, `:143`, `:183`, `:391`. |
| `sem_veredito_entre` | `chamados/models.py:1906`; selo7 `chamados/juizes.py:2105`; `ponto/supra_juiz.py:573`, `:574`. |
| `respostas_sem_veredito` | `chamados/models.py:1934`, `:1967`; `chamados/reconciliador.py:1164`; `chamados/management/commands/validar_classe_a.py:54`; `chamados/services/competencia_trancada.py:126`; `chamados/services/fila_por_causa.py:226`; `colaboradores/services/desligamento.py:165`; `colaboradores/services/ficha.py:169`. |
| `pode_fechar` | `chamados/relatorios_disputa.py:24`; `api/foto_recebida.py:83`; `chamados/services/explicador.py:177`; `becos.py:33`, `:56`; `materializacao.py:1067`, `:1989`; `disputa_emissao.py:1292`, `:2113`; `competencia_trancada.py:178`, `:227`; `chamados/reconciliador.py:412`, `:612`, `:1171`; `ponto/services/entrada_sincronizada.py:59`. Fora os explicitamente enumerados em M8, **NOVO**. |
| `q_vivas` | `chamados/juizes.py:1751`; `chamados/services/fio.py:256`; `pergunta_no_app.py:53`; `vigia_fio.py:711`; **NOVO** `janela_colab.py:63`, `cobranca.py:237`, `raiox.py:74`, `ponto/services/veredito_celula.py:181`. `_ancoradas` não chama `q_vivas` nesta base (`chamados/models.py:344`). |
| `_ancoradas` | `chamados/models.py:223`, `:334`, `:381`; **NOVO** `chamados/services/disputa_emissao.py:1821`, `:1962`. |
| `_gravar_estado` | `chamados/models.py:450`, `:655`, `:835`, `:853`, `:1489`, `:1683`, `:1703`. |
| `DisputaSupervisao.fechar` | `chamados/models.py:2031`, `:2153`; `colaboradores/services/desligamento.py:167`; `chamados/services/materializacao.py:1068`, `:2006`; `disputa_emissao.py:1298`, `:2114`; `competencia_trancada.py:229`; `becos.py:57`; `chamados/management/commands/encerrar_perguntas_com_fato.py:88`; `chamados/reconciliador.py:413`, `:613`, `:1172`; `api/foto_recebida.py:84`; `ponto/services/entrada_sincronizada.py:60`. `ponto/management/commands/fechar_chamados_meio_periodo.py:110` chama **ChamadoColaborador.fechar**, excluir homônimo. |
| `remover_motivo` | `colaboradores/signals.py:77`; `escala/signals.py:174`. |
| `ressuscitar_perguntas` | `chamados/models.py:1498`; `chamados/views_arquivar.py:86`; `ponto/services/cartorio.py:1673`. |
| `_universo_da_ressuscitacao` | `chamados/models.py:1507`, `:1512`, `:1593`. |
| `projecao_temporal_da_pergunta` | `chamados/juizes.py:351`, `:1404`; `escala/services/cadastro_realidade.py:502`. |
| `irmas_respondidas_sem_consumo` | `chamados/reconciliador.py:448`; `chamados/services/materializacao.py:1244`; alias `_irmas_hx`, `chamados/services/disputa_emissao.py:2024`, chamada `:2032`; alias `_irmas12` importado em `chamados/management/commands/auditar_invariantes_chamados.py:975` (conferir sua chamada no bloco desse invariante). **NOVO em relação à enumeração inicial M8**, já incorporado ao selo7 pelo complemento. |

**Leitores de campos: inventário produtivo completo por arquivo**

R=`respondida_em`; T=`resposta_colab`; V=`validada_em`; VP=`validada_por` e `_id`; M=`materializada_em`; MP=`materializada_por` e `_id`; via=`via_resolucao`. Entram leituras por atributos, `getattr`, filtros, `only`, `values`, `update_fields`, fotos e parâmetros de portas; os escritores foram separados acima. Cada referência abaixo é concreta; arquivos não enumerados no brief são **NOVO**.

- `api/views.py`: R `:1165`, `:2563`, `:2577`, `:2620`, `:2637`; T `:1163`, `:2561`.
- **NOVO** `api/views_mensageria.py`: R `:569`, `:570`; T `:567`.
- **NOVO** `chamados/canal.py`: T `:65`.
- `chamados/juizes.py`: M `:292`, `:683`, `:711`, `:858`, `:1390`, `:1502`; R `:352`, `:856`, `:1389`, `:1423`, `:1534`, `:1541`, `:1552`, `:1572`, `:1580`, `:1744`, `:1753`; T `:364`, `:857`, `:1360`, `:1389`, `:1423`, `:1534`, `:1541`, `:1552`, `:1572`, `:1580`, `:1779`, `:1926`, `:2030`, `:2035`, `:2052`; V `:856`, `:1389`; via `:683`, `:695`, `:857`, `:1390`.
- `chamados/models.py`: `_gravar_estado` V `:310`; `_ancoradas` V `:344`; `_universo_da_ressuscitacao` M `:1531`, T `:1533`, via `:1535`, simulação V/R/T `:1554`–`:1556`; ressuscitar foto `:1614`, parâmetros V/VP/R/T `:1629`, `:1630`; lista V/M `:1923`, R `:1924`, T `:1925`; fechar via `:2001`, M `:2006`, T `:2007`, via `:2010`, `:2011`; remover via `:2136`, T `:2139`, via `:2141`; identidade via `:2829`, `:2860`; nascimento R/T `:2949`, `:2950`; estado V/M/R `:3151`, `:3153`, `:3155`, `:3156`, `:3158`; fechar_admin via/M `:3165`, `:3166`; portas carimbo foto trio/R/V/VP `:3236`, `:3237`, lista trio `:3238`, T `:3240`, R `:3241`, V `:3244`; identidade M/MP `:3292`, `:3293`, lista trio `:3296`; fora_fato via `:3311`; descarimbo trio `:3320`; CAMPOS_QUE_ACORDAM `:3331`; lote trio `:3365`, par `:3370`, `:3375`, `:3376`, `:3379`, `:3380`, `:3382`, `:3383`, via `:3392`; encerrar fato via `:3400`; fantasma via/M `:3411`; reabrir fantasma via/M `:3423`; reabrir resposta via/M `:3440`; reabrir competência via `:3447`; q_vivas via/M `:3483`, `:3485`, V `:3496`; guarda par `:3568`, `:3584`, R/T `:3585`, `:3586`; via `:3652`, `:3658`; respondida R `:3665`; display T `:3688`, `:3693`, `:3697`, `:3701`, `:3705`, `:3708`, `:3709`, `:3710`; legado R/V `:4091`, R `:4098`, `:4099`, `:4100`, T `:4101`, V `:4102`, `:4103`, `:4104`, VP/T `:4105`.
- `chamados/reconciliador.py`: M `:367`, `:630`, `:1050`, `:1145`, `:1155`, `:1505`; MP `:1146`, `:1156`; T `:365`, `:1083`; V `:631`, `:1050`; via `:1144`, `:1155`.
- **NOVO** `chamados/services/acoes_disputa.py`: R `:97`, `:132`; T `:355`; V `:347`.
- **NOVO** `chamados/services/becos.py`: R `:30`.
- `chamados/services/competencia_trancada.py`: via `:276`, `:282`.
- **NOVO** `chamados/services/declaracao_texto.py`: R `:417`, `:419`, `:687`; T `:417`; V `:227`.
- `chamados/services/disputa_emissao.py`: M `:228`, `:776`, `:1084`, `:1104`, `:1340`, `:1637`, `:1930`, `:2062`, `:2073`, `:2090`; R `:1483`, `:1609`, `:1635`, `:1677`, `:1692`, `:1698`, `:1703`, `:1735`, `:1843`, `:1861`, `:2074`, `:2091`; T `:1483`, `:1609`, `:1635`, `:1663`, `:1677`, `:1692`, `:1700`, `:1861`, `:2079`; V `:1484`, `:1485`, `:1496`, `:1537`, `:1538`, `:1609`, `:1636`, `:2062`; VP `:1484`, `:1496`, `:1537`, `:1538`, `:1609`, `:1636`; via `:337`, `:409`, `:428`, `:446`, `:1637`, `:2072`, `:2073`, `:2087`, `:2090`.
- `chamados/services/explicador.py`: T `:55`, `:102`, `:103`; via `:118`.
- **NOVO** `chamados/services/fabrica_da_celula.py`: M `:171`; via `:149`.
- `chamados/services/fio.py`: R `:187`, `:706`, `:1613`; T `:377`, `:1279`, `:1291`, `:1613`; via `:1147`, `:1214`, `:1290`, `:1397`, `:1480`, `:1921`, `:1957`, `:1979`, `:2024`.
- **NOVO** `chamados/services/foto_e4b.py`: M/R/V `:180`.
- **NOVO** `chamados/services/historico.py`: R `:99`, V `:101`, M `:103`, T `:109`.
- `chamados/services/materializacao.py`: M `:148`, `:261`, `:262`, `:289`, `:326`, `:862`, `:871`, `:1015`, `:1107`, `:1109`, `:1114`, `:1153`, `:1652`, `:1656`, `:2066`, `:2079`, `:2127`; MP `:1154`; R `:1151`, `:2064`; T `:546`, `:574`, `:625`, `:671`, `:678`, `:718`, `:733`, `:770`, `:869`, `:950`, `:1047`, `:1109`, `:1150`, `:1311`, `:1352`, `:1534`, `:1558`, `:1567`, `:1619`; V `:199`, `:201`, `:1152`, `:2065`, `:2082`; VP `:1152`; via `:261`, `:292`, `:862`, `:868`, `:1108`, `:1150`, `:1652`.
- **NOVO** `chamados/services/painel.py`: M/R/V `:67`, V `:270`, `:271`, `:272`, `:273`; VP `:262`.
- **NOVO** `chamados/services/passivo_validado.py`: M/MP/via `:64`, R/V/VP `:66`, via `:140`, `:141`, `:142`, `:530`, M `:534`, trio `:535`, M `:536`.
- `chamados/services/pergunta_no_app.py`: R `:81`.
- **NOVO** `chamados/services/raiox.py`: M `:135`, `:182`; R/T `:138`, `:147`.
- `chamados/services/relavra_via.py`: via `:118`, `:121`, `:123`, `:127`, `:134`, `:136`, `:155`, `:181`, `:245`, `:252`; M `:181`, `:244`, `:252`.
- `chamados/services/tarefa_do_dia.py`: via `:256`, `:257`.
- `chamados/services/validacao.py`: M `:262`, `:264`, `:341`, `:363`, `:556`, `:681`, `:701`, `:717`; MP `:342`, `:363`; R `:594`, `:813`, `:824`, `:848`, `:1167`; T `:175`, `:263`, `:281`, `:356`, `:380`, `:823`, `:843`, `:848`, `:1094`, `:1096`, `:1098`, `:1103`, `:1104`, `:1105`, `:1106`, `:1109`; V `:171`, `:173`, `:181`, `:262`, `:269`, `:276`, `:300`, `:302`, `:343`, `:351`, `:364`, `:489`, `:553`, `:597`, `:701`, `:810`, `:975`, `:996`; VP `:171`, `:181`, `:262`, `:276`, `:300`, `:302`, `:343`, `:351`, `:364`, `:489`; via `:341`, `:363`.
- **NOVO** `chamados/services/verbos.py`: R `:335`, `:336`, `:342`, `:345`; V `:264`, `:270`; VP `:264`, `:265`, `:266`, `:268`.
- **NOVO** `chamados/services/vigia_carimbo.py`: V `:98`.
- `chamados/services/vigia_fio.py`: R `:636`, `:746`; T `:749`.
- **NOVO** `chamados/templatetags/chamados_tags.py`: T `:139`, `:166`.
- **NOVO** `chamados/veredito_lavrado.py`: M `:203`, `:204`; R `:207`, `:208`.
- **NOVO** `chamados/views.py`: M `:1257`; R `:1280`, `:1416`.
- **NOVO** `chamados/views_cobrar.py`: R `:169`, `:176`; T `:291`.
- **NOVO** `colaboradores/services/calendario.py`: T `:504`.
- `colaboradores/services/ficha.py`: T `:171`.
- **NOVO** `folha/previa.py`: T `:142`.
- **NOVO** `ponto/services/cartorio.py`: T `:1641`; via `:1650`.
- **NOVO** `ponto/services/dia_do_colab.py`: T `:70`.
- **NOVO** `ponto/services/flip_auto.py`: V `:130`; via `:129`.
- **NOVO** `ponto/services/procedencia_batida.py`: MP/VP `:71`, `:72`; M/V `:74`.
- **NOVO** `ponto/services/veredito_celula.py`: M `:196`; T `:197`.

**Comandos adicionais, também entram no censo**

Todos sob `app/chamados/management/commands/`, salvo indicado.

- **NOVO** `auditar_disputas.py`: M `:71`, `:150`.
- **NOVO** `auditar_invariantes_chamados.py`: M `:470`, `:538`, `:588`, `:905`, `:955`, `:1075`, `:1156`, `:1577`, `:1604`, `:1769`; R `:177`, `:592`, `:1105`, `:1605`; T `:1106`, `:1657`; V `:205`, `:1075`, `:1576`, `:1604`, `:1768`, `:1780`; VP `:1076`; via `:202`, `:206`, `:433`, `:542`, `:955`, `:1712`.
- **NOVO** `backfill_decisoes_pergunta.py`: M `:101`, `:325`; MP `:326`; R `:56`, `:60`; T `:61`, `:326`, `:504`; V `:56`, `:60`, `:102`; VP `:60`; via `:101`, `:314`, `:330`, `:335`, `:336`, `:398`, `:504`.
- **NOVO** `bug_par_103.py`: M/via `:66`; MP `:67`; V/VP `:68`.
- **NOVO** `curar_perguntas_em_limbo.py`: R `:46`, `:69`; T `:32`, `:46`.
- **NOVO** `dedup_perguntas_fato.py`: T `:75`.
- **NOVO** `encerrar_perguntas_com_fato.py`: M/V `:32`; via `:33`, `:42`, `:49`, `:66`, `:75`, `:76`.
- **NOVO** `migrar_data_evento_dia_do_turno.py`: M `:147`; via `:70`, `:71`, `:72`.
- **NOVO** `reabrir_orfas_enterradas.py`: M `:67`, `:88`, `:91`, `:129`, `:142`, `:160`; MP `:130`, `:143`, `:160`; V `:68`; via `:67`, `:129`, `:142`, `:160`.
- **NOVO** `relavrar_via_i6.py`: via `:58`, `:59`, `:62`, `:66`, `:68`, `:88`.
- `validar_classe_a.py`: T `:80`.
- **NOVO** `ponto/management/commands/reconciliar_fantasmas.py:213`: M.
- **NOVO** `ponto/management/commands/reconciliar_grade.py:461`: R.

**Templates, incluindo uso em formulário**

- `_pergunta_card.html`: V `:20`, `:23`, `:42`; R `:24`, `:29`, `:32`; display `:31`; novos estados BUG-PAR `:44`, `:47`.
- `modal_fio.html`: R `:185`, `:217`, `:220`; T `:219`; V `:228`; novos estados BUG-PAR `:230`, `:252`, `:274`; `pode_fechar` já transformado pelo explicador em `:462`, `:473`.
- `_card_atendimento.html`: R `:106`, `:143`, `:391`; T `:106`, `:114`, `:141`, `:143`, `:148`, `:154`, `:182`, `:183`, `:391`; display `:106`, `:143`, `:183`, `:391`; estado `:189`; via `:192`. A seleção de formulário usa o valor atual T, não apenas display; entra no selo se F2 mudar seu valor.
- **NOVO** `_lote_preview.html:24`: T.

**C2 temporal e BUG-PAR**

- C2 estrito: três chamadores produtivos da projeção temporal: `_slot_esperado`, `chamados/juizes.py:351` (usa R em `:352`); `_pergunta_viva_calc`, `:1404` (usa M para vitalidade); `beco_em_lote`, `escala/services/cadastro_realidade.py:502` (somente índice `[1]`, R). `sem_veredito_entre` não recebe `agora` `chamados/models.py:1910`; `estado` chama C6 sem ele `:3154`; supra chama lista sem ele `ponto/supra_juiz.py:573`, `:574`. Nenhum deles reconstrói instante apagado; a projeção começa nos campos atuais `chamados/juizes.py:1389`, `:1390`. O **CENSO=0 do predicado estrito** confere por leitura desses caminhos; produção [nao verificado].
- **NOVO leitor temporal fora do C2 estrito:** `chamados/veredito_lavrado.py:163`, `:166`, `:203`, `:204` filtra `materializada_em <= ate` do campo atual; não confundir com reconstrução após descarimbo.
- BUG-PAR L2 lê a trilha, não recalcula: `chamados/services/materializacao.py:267`, `:275`, `:276`; escritor `chamados/services/validacao.py:354`, `:355`, `:357`; display atual via `chamados/services/materializacao.py:339`/testemunha.
- `k6_consulta_a_pergunta`: `chamados/services/materializacao.py:792`, predicado `:795`; `_k6_que_barra` chama em `:806`; `validacao.entrega_que_barra_a_validacao` chama em `chamados/services/validacao.py:224`. A otimização dispensa buscar irmã quando a própria pergunta é consultada; justificativa do mesmo fato/mesma disputa em `:219`, `:222`.
- `bloqueada_sem_dia_do_fato`: constante `chamados/services/materializacao.py:706`; `_k6_que_barra` retorna em `:810`; porta traduz em `chamados/services/validacao.py:233`; testemunha retorna em `chamados/services/materializacao.py:888`; telas leem `_pergunta_card.html:44`, `:47`, `modal_fio.html:230`, `:252`, `:274`. Não equiparar a `bloqueada_competencia_entregue`, cujo ramo distinto está em `chamados/services/materializacao.py:811`, `:812`.
- Censo/testes existentes inclui a classe de guarda sem dia: `chamados/tests/test_bug_par_volta.py:306`, asserções `:312`, `:317`, `:318`, `:338`; extrator sem banco carrega helper real em `chamados/tests/test_bug_par_sem_banco.py:140`. Execução [nao verificado].

## 16/11 23:10 · sessao 74a6e4ce50a2 · - · - · origem=Ronald (tmux)

Censo realizado por AST de todos os `.py` em `app/` (excluídos `tests/`, `migrations/`, `__pycache__` e `tests.py`) e grep de todos os templates; referências por nome são teto, com homônimos identificados abaixo. O script reproduzível está ao fim. Não executei banco nem testes e não criei arquivos. [nao verificado] [nao verificado]

**Achados que derrubam o brief**

- **CAIU “três chamadores”**: `_ts_tipo_do_plantio` tem **cinco sítios de chamada**, em quatro funções: juiz `juizes.py:376`; **NOVO** `par_do_intervalo` em `materializacao.py:214,222`; testemunha `materializacao.py:895`; materializador `materializacao.py:1328`. O RED de paridade precisa alcançar também as duas metades do par. (`app/chamados/juizes.py:376`; `app/chamados/services/materializacao.py:214`; `app/chamados/services/materializacao.py:222`; `app/chamados/services/materializacao.py:895`; `app/chamados/services/materializacao.py:1328`)
- **NOVO** `data_conferida_em` tem leitor em `_data_em_disputa`, além de `_materializar_uma`; A20 não se resume ao sítio do brief. (`app/chamados/services/materializacao.py:169`; `app/chamados/services/materializacao.py:1851`)
- **NOVOS leitores R/V/C BUG-PAR**: `par_do_intervalo` lê V da irmã; `_declarado_da_metade`, `divergencia_declarado_x_gravado`, `espera_na_escrita` leem R; `_situacao_da_irma`, `paridade_absolvida_bug_par_103`, contador e nova testemunha leem C/VIA. (`app/chamados/services/materializacao.py:199`; `app/chamados/services/materializacao.py:201`; `app/chamados/services/materializacao.py:718`; `app/chamados/services/materializacao.py:733`; `app/chamados/services/materializacao.py:770`; `app/chamados/services/materializacao.py:261`; `app/chamados/services/materializacao.py:289`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:862`)
- **NOVO escritor de V**: `devolver_paridade_bug_par_103` limpa V/autor, conserva R, registra DEVOLUCAO, sem limpar `recusa_motivo`. (`app/chamados/services/validacao.py:349`; `app/chamados/services/validacao.py:350`; `app/chamados/services/validacao.py:354`; `app/chamados/services/validacao.py:364`)
- **NOVOS chamadores `_materializar_uma`**: `_gravar_irma_do_par`, além de `_validar_o_par` e `validar_pergunta`; o passivo agora chama em 537 e a fila em 2084. (`app/chamados/services/materializacao.py:1707`; `app/chamados/services/validacao.py:555`; `app/chamados/services/validacao.py:687`; `app/chamados/services/passivo_validado.py:537`; `app/chamados/services/materializacao.py:2084`)
- `estado_aplicacao_pergunta` tem leitores **NOVOS** no contador, na espera de declarado×gravado e no templatetag; o explicador recebe a mesma função por import no início do arquivo. (`app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:757`; `app/chamados/templatetags/chamados_tags.py:134`; `app/chamados/templatetags/chamados_tags.py:136`; `app/chamados/services/explicador.py:124`)

**Escritores por estado/entidade**

| Campo | Escritores persistentes/portas; simulações separadas |
|---|---|
| `resposta_colab` | Respondedor `disputa_emissao.py:1481`; automático `:1674`; lote de reabertura `:1609`→`:1648`; ressuscitação `models.py:1629`→`:1630`; nascimento por `**campos` `models.py:3124`→`:3127`. `models.py:1556` é simulação em cópia para julgamento. |
| `respondida_em` | Respondedor `disputa_emissao.py:1482`; automático `:1675`; reparo da trilha `:1696`; lote `:1609`→`:1648`; ressuscitação `models.py:1629`; `carimbar(resposta=)` `models.py:3242`; nascimento `models.py:3124`. Simulação `:1555`, reversão em memória `:3273`. |
| `validada_em/por` | `validacao.py:168`→`:169`; devolução normal `:273`→`:274`; **NOVO** devolução BUG-PAR `:349`→`:350`; nova resposta limpa `disputa_emissao.py:1494`→`:1495`; lote `:1609`; ressuscitação `models.py:1629`; `carimbar(resposta='validar')` `models.py:3245`→`:3246`; nascimento `:3124`. Simulação `:1554`, reversão em memória `:3274`→`:3275`. |
| `materializada_em` | Atribuição canônica `_escrever_carimbo` `models.py:3209`, acionada por portas; lote `models.py:3393`. `regularizacao_ext.py:199` escreve **outro modelo**, `reg`, não PerguntaDisputa. |
| `via_resolucao` | `_escrever_carimbo` `models.py:3211`; lote `:3393`; porta específica `encerrar_por_fato_no_espelho` `:3402`; relavra `relavra_via.py:125`. |
| `recusa_motivo` | Resposta escape `disputa_emissao.py:1511`; pendência técnica `materializacao.py:1332`; recusa `materializacao.py:1665` ou extra da porta de identidade `models.py:3298`; recusa administrativa `validacao.py:827`. |
| `data_conferida_em` | `validacao.py:907`; dois leitores do materializador `materializacao.py:169,1851`. |
| `fechada_em` **Disputa** | `models.py:2013` (`fechar`), `:2230` (`reabrir_admin`), `:2254` (`reabrir_sistema`). `models.py:190` e outros nomes de atributos genéricos são teto por nome, sem atribuição automática a Disputa. |

**Furo de escritor continua**: `carimbar(resposta=)` atribui R/V e salva sem registrar atos RESPOSTA/VALIDACAO; absorção chama com `resposta='validar'` e registra só ABSORCAO; resolução de chamado chama com `resposta='responder'`. (`app/chamados/models.py:3240`; `app/chamados/models.py:3242`; `app/chamados/models.py:3245`; `app/chamados/models.py:3262`; `app/chamados/services/materializacao.py:1043`; `app/chamados/services/materializacao.py:1045`; `app/chamados/services/disputa_emissao.py:2082`)

**Todos os sítios AST por nome — produção**

Legenda: `R=resposta_colab`, `r=respondida_em`, `V=validada_em`, `v=validada_por[/_id]`, `C=materializada_em`, `M=recusa_motivo`, `I=via_resolucao`, `D=data_conferida_em`, `F=fechada_em`. A lista inclui escritores, leitores, filtros, listas de campos e fotos; classificação de escrita acima. Cada `arquivo:linha` abaixo é diretamente da BASE.

```text
app/api/foto_recebida.py F:66,78
app/api/views.py F:1018,1034,2678 R:1163,2561 r:1165,2563,2577,2620,2637
app/api/views_mensageria.py R:567 r:569,570
app/chamados/canal.py R:65
app/chamados/juizes.py C:292,683,711,858,1390,1502 F:2466 I:683,695,857,1390 R:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052 V:856,1389 r:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753
app/chamados/management/commands/auditar_chamados_orfaos.py F:41
app/chamados/management/commands/auditar_disputas.py C:71,150 F:87,136,167
app/chamados/management/commands/auditar_invariantes_chamados.py C:470,538,588,905,955,1075,1156,1577,1604,1769 F:592,806 I:202,206,433,542,955,1712 M:536,1075,1112 R:1106,1657 V:205,1075,1576,1604,1768,1780 r:177,592,1105,1605 v:1076
app/chamados/management/commands/backfill_decisoes_pergunta.py C:101,325 I:101,314,330,335,336,398,504 R:61,326,504 V:56,60,102 r:56,60 v:60
app/chamados/management/commands/bug_par_103.py C:66 I:66 M:69 V:68 v:68
app/chamados/management/commands/curar_perguntas_em_limbo.py R:32,46 r:46,69
app/chamados/management/commands/dedup_perguntas_fato.py R:75
app/chamados/management/commands/encerrar_perguntas_com_fato.py C:32 F:82 I:33,42,49,66,75,76 V:32
app/chamados/management/commands/fechar_becos_disputa.py F:32
app/chamados/management/commands/migrar_data_evento_dia_do_turno.py C:147 I:70,71,72
app/chamados/management/commands/reabrir_orfas_enterradas.py C:67,88,91,129,142,160 F:96,131,164,165 I:67,129,142,160 V:68
app/chamados/management/commands/relavrar_via_i6.py I:58,59,62,66,68,88
app/chamados/management/commands/validar_classe_a.py R:80
app/chamados/models.py C:1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485 F:190,1638,1870,1888,1960,1964,2013,2017,2020,2112,2114,2219,2230,2235,2248,2254,2258,2305,2343,2778,2781 I:1535,2001,2010,2011,2136,2141,2829,2860,3165,3211,3236,3238,3296,3311,3320,3331,3365,3392,3393,3400,3402,3403,3411,3423,3440,3447,3483,3485,3652,3658 M:3298,3299 R:1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105 V:310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104 r:1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100 v:1614,1629,3237,3246,3247,3275,4105
app/chamados/reconciliador.py C:367,630,1050,1145,1155,1505 F:412,600,611,612,1161,1162 I:1144,1155 R:365,1083 V:631,1050
app/chamados/relatorios_disputa.py F:46
app/chamados/services/acoes_chamado.py F:346
app/chamados/services/acoes_disputa.py F:257,349 M:83 R:355 V:347 r:97,132
app/chamados/services/becos.py F:27,56,59 r:30
app/chamados/services/competencia_trancada.py F:176,221 I:276,282
app/chamados/services/declaracao_texto.py R:417 V:227 r:417,419,687
app/chamados/services/disputa_emissao.py C:228,776,1084,1104,1340,1637,1930,2062,2073,2090 F:41,541,992,1111,1347,1536,2026,2050,2052,2111,2113 I:337,409,428,446,1637,2072,2073,2087,2090 M:1511,1513 R:1481,1483,1609,1635,1663,1674,1677,1692,1700,1861,2079 V:1484,1485,1494,1496,1537,1538,1609,1636,2062 r:1482,1483,1609,1635,1675,1677,1692,1696,1698,1703,1735,1843,1861,2074,2091 v:1484,1495,1496,1537,1538,1609,1636
app/chamados/services/explicador.py I:118 M:113 R:55,102,103
app/chamados/services/fabrica_da_celula.py C:171 I:149
app/chamados/services/fila_por_causa.py F:225,259,262
app/chamados/services/fio.py F:131,646 I:1147,1214,1290,1397,1480,1921,1957,1979,2024 M:564,1407 R:377,1279,1291,1613 r:187,706,1613
app/chamados/services/foto_e4b.py C:180 F:182 V:180 r:180
app/chamados/services/historico.py C:103 R:109 V:101 r:99
app/chamados/services/materializacao.py C:148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127 D:169,1851 F:1067,1989 I:261,292,862,868,1108,1150,1652 M:1332,1333,1652,1655,1663,1665,1666 R:546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619 V:199,201,1152,2065,2082 r:1151,2064 v:1152
app/chamados/services/painel.py C:67 V:67,270,271,272,273 r:67 v:262
app/chamados/services/passivo_validado.py C:64,534,535,536 I:64,140,141,142,530,535 M:66 V:66 r:66 v:66
app/chamados/services/pergunta_no_app.py F:81 r:81
app/chamados/services/raiox.py C:135,182 R:138,147 r:138,147
app/chamados/services/regularizacao_ext.py C:199,201,220 F:60
app/chamados/services/relavra_via.py C:181,244,252 F:260 I:118,121,123,125,127,134,136,155,181,245,252
app/chamados/services/tarefa_do_dia.py I:256,257
app/chamados/services/validacao.py C:262,264,341,363,556,681,701,717 D:905,907,909 F:182,288,291,293 I:341,363 M:181,262,266,282,299,301,302,344,364,825,827,833,847 R:175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109 V:168,171,173,181,262,269,273,276,300,302,343,349,351,364,489,553,597,701,810,975,996 r:594,813,824,848,1167 v:169,171,181,262,274,276,300,302,343,350,351,364,489
app/chamados/services/verbos.py V:264,270 r:335,336,342,345 v:264,265,266,268
app/chamados/services/vigia_carimbo.py V:98
app/chamados/services/vigia_fio.py R:749 r:636,746
app/chamados/signals.py F:29,131,155
app/chamados/templatetags/chamados_tags.py R:139,166
app/chamados/veredito_lavrado.py C:203,204 r:207,208
app/chamados/views.py C:1257 F:471,911,1329 r:1280,1416
app/chamados/views_cobrar.py F:161,164 R:291 r:169,176
app/colaboradores/painel_cell.py F:32
app/colaboradores/services/calendario.py R:504
app/colaboradores/services/desligamento.py F:164
app/colaboradores/services/drawer.py F:72
app/colaboradores/services/ficha.py R:171
app/colaboradores/services/situacional.py F:117
app/colaboradores/signals.py F:73
app/escala/signals.py F:170
app/folha/previa.py R:142
app/ponto/management/commands/alertar_disputas_pendentes.py F:34
app/ponto/management/commands/escalonar_chamados_supervisao.py F:58,149
app/ponto/management/commands/reconciliar_fantasmas.py C:213
app/ponto/management/commands/reconciliar_grade.py r:461
app/ponto/services/cartorio.py I:1650 R:1641
app/ponto/services/dia_do_colab.py R:70
app/ponto/services/entrada_sincronizada.py F:57
app/ponto/services/flip_auto.py I:129 M:132 V:130
app/ponto/services/procedencia_batida.py C:74 V:74 v:71,72
app/ponto/services/veredito_celula.py C:196 R:197
```

Definições dos campos não entram na listagem de referências AST acima: `models.py:1821` (Disputa F); `:2552`/`:2553`/`:2554` (R/r/V); `:2563` (D); `:2665`/`:2666` (M/I); `:3514` (v); `:3530` (C). `models.py:4152` é outro modelo. (`app/chamados/models.py:1821`; `app/chamados/models.py:2552`; `app/chamados/models.py:2563`; `app/chamados/models.py:2665`; `app/chamados/models.py:3514`; `app/chamados/models.py:3530`; `app/chamados/models.py:4152`)

**Leitores de templates — grep completo dos nomes**

- `_pergunta_card.html`: V `20,23,42`; r `24,29,32`; estados de aplicação **NOVOS BUG-PAR** `44,47`. (`app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/_pergunta_card.html:32`; `app/templates/chamados/partials/_pergunta_card.html:42`; `app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/_pergunta_card.html:47`)
- `modal_fio.html`: D `170,171`; r `185,217,220`; R `219`; M `226`; V `228`; estados **NOVOS** `230,232,234,236,238,240,242,244,246,248,252,254,256,258,260,262,264,266,268,270,274`; F `406`. Inputs M são `61,322`, não leitores do valor. (`app/templates/chamados/partials/modal_fio.html:170`; `app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217`; `app/templates/chamados/partials/modal_fio.html:226`; `app/templates/chamados/partials/modal_fio.html:228`; `app/templates/chamados/partials/modal_fio.html:230`; `app/templates/chamados/partials/modal_fio.html:252`; `app/templates/chamados/partials/modal_fio.html:274`; `app/templates/chamados/partials/modal_fio.html:406`)
- `_lote_preview.html`: R `24`. (`app/templates/chamados/partials/_lote_preview.html:24`)
- `_card_atendimento.html`: R/r `106`; R `114,141,143,148,154,182,183`; VIA/M `192`; R/r `391`. Linhas `95,134,188,190` são comentários. (`app/templates/chamados/partials/_card_atendimento.html:106`; `app/templates/chamados/partials/_card_atendimento.html:114`; `app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:143`; `app/templates/chamados/partials/_card_atendimento.html:148`; `app/templates/chamados/partials/_card_atendimento.html:154`; `app/templates/chamados/partials/_card_atendimento.html:182`; `app/templates/chamados/partials/_card_atendimento.html:192`; `app/templates/chamados/partials/_card_atendimento.html:391`)

**Funções e autoridades: chamadores com identidade conferida**

| Função | Todos os sítios diretos de produção encontrados |
|---|---|
| `_ts_tipo_do_plantio` | `juizes.py:376`; `materializacao.py:214,222,895,1328` |
| `_tipo_pelo_marco` | `materializacao.py:656` |
| `_materializar_uma` | `materializacao.py:1707,2084`; `passivo_validado.py:537`; `validacao.py:555,687` |
| `_materializar_resposta_hora/data_hora/escolha` | `materializacao.py:1889/1891/1893` |
| `_materializar_ausencia_de_resposta` | `disputa_emissao.py:1555` |
| `materializar_perguntas_validadas_da_disputa` | `backfill_fio_morto_e4b.py:147`; **NOVO vs lista do brief** `acoes_disputa.py:141`, `fio.py:716`, `validacao.py:698` |
| `materializar_pendentes_aprovadas` | `signals.py:165` |
| `estado_aplicacao_pergunta` | `fio.py:376`; `materializacao.py:339,757`; `explicador.py:124`; `templatetags/chamados_tags.py:136` |
| `par_do_intervalo` **NOVO** | `materializacao.py:907,1695`; `validacao.py:545` |
| `metade_irma_do_intervalo` **NOVO** | `materializacao.py:194,2019`; `validacao.py:206` |
| `competencia_do_fato` **NOVO, desta materialização** | `materializacao.py:330,808`; `validacao.py:338`; `bug_par_103.py:58,74,116` (import `:41`). Os usos nos comandos backfill/migração e `ponto` são homônimos de `ponto.services.fechamento`, **excluídos**. |
| `k6_consulta_a_pergunta` **NOVO** | `materializacao.py:806`; `validacao.py:224` |
| `_k6_que_barra` **NOVO** | `materializacao.py:825`; `validacao.py:225` |
| `_competencia_que_barra` **NOVO** | `materializacao.py:909,933,1450,1830` |
| `espera_declarado_x_gravado` **NOVO** | `explicador.py:132`; `validacao.py:611`; `templatetags/chamados_tags.py:128` |
| `espera_na_escrita` **NOVO** | `materializacao.py:1877` |
| `divergencia_declarado_x_gravado` **NOVO** | `materializacao.py:741` |
| `_declarado_da_metade` **NOVO** | `materializacao.py:728` |
| `_espera_do_aval_163` **NOVO** | `materializacao.py:922,936,1451` |
| `respostas_intervalo_recusadas_paridade` **NOVO** | **um leitor direto** `bug_par_103.py:79` |
| `paridade_absolvida_bug_par_103` **NOVO** | `materializacao.py:327`; `validacao.py:334` |
| `rotulo_bug_par_103` **NOVO** | `materializacao.py:880` |
| `perguntas_absorvidas_por` | `materializacao.py:1147` |
| `devolver_absorvidas_da_ausencia_rejeitada` | `ponto/signals.py:262` |
| `_carimbar_recusa` | `materializacao.py:1318,1430,1871,1966`; `passivo_validado.py:566`; `validacao.py:562` |
| `responder_pergunta_disputa_core` | `api/views.py:2627`; `acoes_disputa.py:373`; `declaracao_texto.py:419,693`; `views.py:1283` |
| `responder_automaticamente` | `api/foto_recebida.py:48` |
| `carimbar_resposta_pela_trilha` | `curar_perguntas_em_limbo.py:67` |
| `reabrir_para_correcao` | `curar_perguntas_em_limbo.py:69`; `models.py:2318` (alias import `:2300`); `acoes_disputa.py:105`; `materializacao.py:1164`; `validacao.py:836`; `views.py:1436` |
| `recobrar_questionario` | `views.py:1425` |
| `reabrir_credor_de_pergunta_viva` | `backfill_fio_morto_e4b.py:90,135,175`; `disputa_emissao.py:1969` |
| `resolver_perguntas_por_chamado_resolvido` | `reconciliador.py:537,1649`; `becos.py:54`; `signals.py:117`; `ponto/management/commands/reconciliar_fantasmas.py:281` |
| `_resolver_perguntas_por_chamado_resolvido` | `disputa_emissao.py:1897,1903` |
| `_fechar_disputa_se_quitada` | `materializacao.py:2086`; `passivo_validado.py:413` |
| `relavrar` / `via_de_hoje` | `relavrar_via_i6.py:102` e `relavra_via.py:157` / `relavrar_via_i6.py:64` e `relavra_via.py:120` |
| `q_carimbo_desfazivel` | `migrar_data_evento_dia_do_turno.py:446`; `fabrica_da_celula.py:150`; `relavra_via.py:238`; `vigia_fio.py:769` |
| `desfazer_carimbo_sem_lastro` | `fabrica_da_celula.py:169,209` |
| `reparar` / `_plantar_o_dia` | `materializar_passivo_validado.py:104,113` / `passivo_validado.py:443` |
| `motivos_a_perguntar` | `disparar_perguntas_competencia.py:117`; `fabrica_da_celula.py:207,242` |
| `perguntar` | `perguntar_chamados_sem_pergunta.py:29`; `fabrica_da_celula.py:219` |

**Contador e U163**

Há um leitor direto de produção do contador (`bug_par_103.py:79`), que imprime `pendentes_aval`/`congelados_no_recorte`; a montagem da Pauta é outro percurso, não leitor dos baldes do contador. (`app/chamados/management/commands/bug_par_103.py:53`; `app/chamados/management/commands/bug_par_103.py:71`; `app/chamados/management/commands/bug_par_103.py:79`; `app/chamados/management/commands/bug_par_103.py:81`; `app/chamados/management/commands/bug_par_103.py:85`)

Retirar U163 do pulo K6 altera tanto `_k6_que_barra` quanto a decisão de procurar irmã em `entrega_que_barra_a_validacao`; é o mesmo predicado, sem cópia. (`app/chamados/services/materializacao.py:806`; `app/chamados/services/validacao.py:224`)

O novo estado L1/L5 de U163 não carimbada seria `outras`, logo continuaria em `universo` e `pendentes_aval`; os baldes especiais atuais são só do ramo carimbado elegível U103. Destino futuro é pendência do Ronald, não decisão deste censo. (`app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:331`; `app/chamados/services/materializacao.py:334`; `app/chamados/services/materializacao.py:347`; `app/chamados/services/materializacao.py:351`; `app/chamados/services/materializacao.py:356`; anexo `revisao.r`, item 3 [nao verificado em arquivo local])

**Script AST 

[truncado em 20000 de 22785 caracteres]

## 16/11 23:10 · sessao 08b2e3bb47c8 · - · - · origem=Ronald (tmux)

Revalidação estritamente por leitura; nenhuma suíte, banco ou arquivo alterado. [nao verificado]

**Delta certificado:** comparei os bytes da cópia com `git show 520d7b557:<arquivo>`: `models.py`, `juizes.py`, `tarefa_do_dia.py`, `explicador.py`, `_pergunta_card.html`, `modal_fio.html`, `CORTES.md` coincidem. A cópia não tem `.git`; o histórico foi lido somente por objetos no repositório `/home/ronald/saas-hasner`. Evidência persistida em arquivo: [nao verificado].

**Linhas do anexo matriz.md**, contadas do conteúdo fornecido incluindo vazias:

- Premissa F1=be2b8d733: **14**.
- Premissa helper/leitura conservada: **27–29**.
- Título DEPENDE-F1: **69**.
- Itens 1–6: **70–71; 72–73; 74; 75; 76; 77**, respectivamente.
- Re-medida: **112–117**; resumo DEPENDE-F1: **116**.
- Essa contagem concorda com o próprio pedido, que localiza o parágrafo residual em `matriz.md:112–115`.

| Premissa do anexo | Julgamento na BASE | Evidência BASE |
|---|---|---|
| `matriz.md:14`, `:27–29`: F1 vigente é be2b8d733, helpers e instante conservado existem | **CAIU** | Projeção lê os campos atuais e devolve `materializada`, `app/chamados/juizes.py:1389`, `:1390`, `:1400`; descarimbo apaga o instante, `app/chamados/models.py:3318`. Os identificadores `_leitura_conservada`, `_leitura_dos_atos`, `_q_validada_pelo_ato`, `materializada_conservada_em` não foram encontrados nos módulos atuais; localização positiva deles: [nao verificado]. |
| DEP 1 `:70–71`: `[3]` é instante conservado, inclusive depois de descarimbo | **CAIU** | Campo atual em `app/chamados/juizes.py:1390`; descarimbo em `app/chamados/models.py:3318`. A conservação vigente C3 conserva antes/depois da F2; não recupera instante apagado. |
| DEP 2 `:72–73`: assinatura conservada reaplicada no caso passivo | **CAIU** como premissa da BASE | A projeção apenas recorta campos futuros; não reaplica assinatura, `app/chamados/juizes.py:1391–1400`. Execução do antigo T148: [nao verificado]. |
| DEP 3 `:74`: relógio via E desaparece de `[3]` | **CONFERE** para o resultado, **CAIU** a explicação “pela F1 be2” | `[3]` é campo atual, sem testemunha E, `app/chamados/juizes.py:1390`, `:1400`. |
| DEP 4 `:75`: F1 sozinha já lê ato e rompe continência de q_vivas | **CAIU** | C6 usa projeção dos campos, `app/chamados/juizes.py:1404`; `q_vivas` usa validação projetada, `app/chamados/models.py:3496`; continência continua o contrato declarado, `:3467`. |
| DEP 5 `:76`: guarda PK nula do helper muda resultado das instâncias não salvas | **CAIU** para os leitores atuais | `estado` lê campos em `app/chamados/models.py:3151`; `respondida` em `:3665`; display em `:3688`. Não há chamada àquele helper nesses corpos. |
| DEP 6 `:77`: NameError do teste de memória F1c1 herdado | **CAIU** para o defeito identificável por leitura | O diff `40052af46..520d7b557` adiciona `SimpleNamespace=NS`, `texto_vazio` e carrega `pergunta_respondida` real no harness: `app/chamados/tests/test_e4b_classe_e_memoria.py:308–312`; teste temporal existente `::AlcanceTest.test_RED_harness_carrega_projecao_real_sem_alterar_pergunta`, `:374–391`. Resultado por execução: [nao verificado]. |

**BUG-PAR: premissas que precisam entrar na parte 1**

| Premissa | Julgamento e prova |
|---|---|
| “Entregue e sem dia do fato têm o mesmo estado” | **CAIU**: constantes distintas `bloqueada_competencia_entregue` L1 e `bloqueada_sem_dia_do_fato` L5 em `app/chamados/services/materializacao.py:705`, `:706`; retornos distintos em `:809–814`. |
| Sem `data_evento`, K6 não é consultado | **CONFERE**: `competencia_do_fato` retorna antes da consulta, `app/chamados/services/materializacao.py:784–789`; teste `app/chamados/tests/test_bug_par_volta.py::BugParVoltaTest.test_matriz_linha5_fora163_sem_dia_do_fato_estado_proprio` — conferir nome da classe — assertions `:312–321`, incluindo K6 `assert_not_called` em `:319`. |
| L2 F6 fechado/aprovado não entregue conserva toque Validar e não vira paridade | **CONFERE**: testemunha `bloqueada_competencia`, `app/chamados/services/materializacao.py:909–911`, `:933–935`; motivo verdadeiro preservado depois do rollback em `:1469–1477`; formatador canônico `:1789–1795`. |
| L2 deve produzir `COMPETENCIA_APROVADA` também no laço e passivo | **CONFERE**, provas existentes: `app/chamados/tests/test_bug_par_volta.py::test_matriz_linha2_fechada_nao_entregue_conserva_o_toque_e_o_motivo`, `:237–273`; `::test_matriz_linha2_laco_conserva_o_motivo`, `:378–389`; `::test_matriz_linha2_passivo_conserva_o_motivo`, `:391–403`. Asserções explicitam VALIDACAO+DEVOLUCAO, nenhuma batida e motivo COMPETENCIA_APROVADA. |
| `k6_consulta_a_pergunta` pode ser substituído por “fora U163” genérico | **CAIU**: predicado exige pergunta não nula **e metade de intervalo** fora U163, `app/chamados/services/materializacao.py:792–795`. |
| c4c sempre procura a irmã para K6 | **CAIU**: `app/chamados/services/validacao.py:224` pula a busca quando a guarda consulta a própria pergunta; senão usa `irma_da_validacao`. O universo da irmã é mesma disputa/data, `app/chamados/services/materializacao.py:139–149`. |
| Só entregue tira Validar | **CAIU**: sem dia também tira, assim como espera e indisponibilidade; `app/templates/chamados/partials/_pergunta_card.html:44–50`; `app/templates/chamados/partials/modal_fio.html:276–279`. F6 `bloqueada_competencia` conserva toque nesses mesmos sítios. |
| Precedência K6 → F6 → espera declarado | **CONFERE**: K6 em `app/chamados/services/materializacao.py:885–888`; F6 do par em `:909–911` antes de espera `:922–924`; metade isolada F6 em `:933–935` antes da espera `:936–938`; criação tardia conserva ordem em `:1447–1453`. Prova `app/chamados/tests/test_bug_par_volta.py::test_matriz_precedencia_k6_antes_de_f6_antes_de_declarado`, `:323–345`. |

**Diff `71e9576bf..520d7b557` nos sítios citados**

- **Sem alteração de conteúdo nem linha:** `app/chamados/models.py`, `app/chamados/juizes.py`, `app/chamados/services/tarefa_do_dia.py`, `app/chamados/services/pergunta_no_app.py`, `app/api/views.py`, `app/ponto/supra_juiz.py`, `_card_atendimento.html`. Portanto todas as referências de estados.md a esses arquivos ainda casam; a divergência “leitor por campos versus c3 por atos” continua evidenciada por `models.py:3151`, `:3665`, `:3688`, `:1923`, `:3496`.
- **`explicador.py`**: referências antigas 72→95; 73→96; 74→97; 77→100; 85→108; 93→116; 139→177. Antiga 102 **mudou de conteúdo**: agora `_sem_gravar(...,'sistema')` em `app/chamados/services/explicador.py:142`. Entraram tabela `SEM_GRAVAR_POR_DONO` `:33`, helper `_sem_gravar` `:43`, recusa com `gravou=False` `:124–126`, espera declarado `:131–136`.
- **`fio.py`**: antiga187=187; 357→359; 652→660. Prefetch `colaborador__empresa` em `app/chamados/services/fio.py:217`; testemunha ausente/exceção agora produz indisponível em `:378–385`.
- **`_pergunta_card.html`**: antigas23/25/29/31 continuam exatamente nessas linhas; bloco novo da aplicação em `app/templates/chamados/partials/_pergunta_card.html:43–51`.
- **`modal_fio.html`**: antiga436→463; 440→467. Antiga442 **mudou**: `app/templates/chamados/partials/modal_fio.html:469` agora lê `it.quem_resolve`, sem tradução própria TI/DP por dono. Novos estados e supressão de Validar em `:230–279`.
- **UI-VINCULO/EDITOR-CAL:** diff também alcança painel_vinculo e folga_calendario_editar; não altera os corpos F2 acima. Ligação causal além do diff: [nao verificado].
- **Consumidores novos relevantes:** `app/chamados/services/desenlace.py:65–82` lê `gravou=False` e estado dos itens; `app/chamados/services/acoes_disputa.py:63–66` passa a devolver a mensagem da porta, inclusive par.

**Leis já respondem às perguntas antigas**

- Desenho 1, 4, 5, 6, 7 e 9, mais F2 L1: **DECIDIDO PELA LEI CORTES-F2-F9-1611 + RESPOSTAS-F2-ESTADOS/C1–C3 | contestavel**. Corte BASE em `app/docs/CORTES.md:487`; leitores conservados em `app/chamados/models.py:3151`, `:1923`, `:3665`, `:3688`, `:3496`; materialização atual em `app/chamados/juizes.py:1390`.
- Desenho 2 e 3, significado de respondida e ilegalidade do par: **DECIDIDO PELA LEI L-130 | contestavel**, `app/docs/LEIS.md:151`; juiz real `app/chamados/juizes.py:1552`. C1/C2 preservam aguarda_admin e tarefa, e mandam o ramo RESPOSTA vazia para B2.
- Desenho 8, **resultado exigido para esta F2**: conservação da B1, decidido pelo aval fornecido; implementação `app/chamados/models.py:1622–1630`; SILENCIO permanece sem ações `app/chamados/services/tarefa_do_dia.py:176–177`. **Lei geral futura “ressuscitar devolve ao colaborador?” continua registrada não decidida** em `app/docs/LEIS.md:151`, `app/chamados/models.py:1625–1626`; não é bloqueio F2.
- F2 L2: **DECIDIDO PELA LEI CORTES-F2-F9-1611 item 2 | contestavel**, `app/docs/CORTES.md:487`: face SQL, zero juiz novo, selo integral por pergunta.
- Dono futuro do ramo último ato RESPOSTA vazia continua pergunta explicitamente aberta no C2 fornecido; situação atual responde colaborador em `app/chamados/services/tarefa_do_dia.py:159–161`. Frota desse ramo: [nao verificado].

## 16/11 23:09 · sessao 5545f6e324a9 · - · - · origem=Ronald (tmux)

Censo estático executado com `python3`/AST e `rg`, sem importar Django nem executar testes. Critério AST: atributos, keywords e strings de campo/lookup; fixtures/testes/migrations separados. Não trate a lista como prova de ausência de chamadas dinâmicas: isso é [nao verificado]. [nao verificado]

**Escritores da projeção R/I/V/autor — os dois do brief não bastam**

| Escritor/porta | O que escreve | NOVO em relação à lista do brief |
|---|---|---|
| `app/chamados/services/disputa_emissao.py:1439` `responder_pergunta_disputa_core` | R/I em `:1481-1483`; limpa V/autor em `:1494-1496`; RESPOSTA em `:1525` | **NOVO** |
| `app/chamados/services/disputa_emissao.py:1576` `reabrir_para_correcao` | quatro campos limpos em `:1609`, snapshot/decisão em `:1631-1646`, escreve pela porta de lote `:1648` | já estava |
| `app/chamados/services/disputa_emissao.py:1654` `responder_automaticamente` | R/I `:1674-1677`, conserva passivo `:1673`, RESPOSTA `:1678` | **NOVO** |
| `app/chamados/services/disputa_emissao.py:1683` `carimbar_resposta_pela_trilha` | I `:1696-1698`, RESPOSTA `:1699-1700` | **NOVO** |
| `app/chamados/services/validacao.py:161` `_carimbar_validacao` | V/autor `:168-171`, VALIDACAO `:173-175` | **NOVO** |
| `app/chamados/services/validacao.py:237` `devolver_validacao_recusada` | limpa V/autor `:273-276`, DEVOLUCAO `:279-282` | já estava, linhas mudaram |
| `app/chamados/services/validacao.py:308` `devolver_paridade_bug_par_103` | limpa V/autor `:349-351`, DEVOLUCAO `:354-358`; K6 antes da escrita `:338-340` | **NOVO** |
| `app/chamados/models.py:1560` `ressuscitar_perguntas` | conserva R/V sem linha `:1613-1614`, limpa par inteiro e V/autor pela porta `:1629-1630` | **NOVO** |
| `app/chamados/models.py:3213` `carimbar` | I condicional `:3242-3243`, V/autor condicionais `:3245-3247`, `save` `:3262` | **NOVO** |
| `app/chamados/models.py:3362` `descarimbar_em_lote` | `update(**outros)` `:3391-3393`, valida composição do par `:3370-3383` | **NOVO**, infraestrutura |
| `app/chamados/models.py:3124` nascimento `_nascer` | recebe campos no construtor, salva `:3127`, conserva linhas R/V `:3129` | **NOVO**, infraestrutura |

Atribuições só em memória devem ser separadas: universo da ressuscitação usa cópia `app/chamados/models.py:1541-1557`; desfazimento do `carimbar` restaura memória `app/chamados/models.py:3269-3276`. A porta `carimbar` escreve R/V no corpo `:3242-3262` sem registrar `DecisaoPergunta` nesse método; portanto a frase do brief “todos mantêm concordância / divergência só no LEGADO” **CAIU como descrição universal do código** (`app/chamados/models.py:3213-3267`). Não há dado para afirmar ocorrência em produção: [nao verificado].

**Chamadores desses escritores/portas — inclui aliases resolvidos pelo AST**

- `responder_pergunta_disputa_core`: `app/api/views.py:2627`; `app/chamados/views.py:1283`; `app/chamados/services/declaracao_texto.py:419,693`; `app/chamados/services/acoes_disputa.py:373`.
- `responder_automaticamente`: `app/api/foto_recebida.py:48`.
- `carimbar_resposta_pela_trilha`: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.
- `reabrir_para_correcao`: `app/chamados/models.py:2318`; `app/chamados/views.py:1436`; `app/chamados/services/acoes_disputa.py:105`; `app/chamados/services/materializacao.py:1164`; `app/chamados/services/validacao.py:836`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`.
- `_carimbar_validacao`: `app/chamados/services/validacao.py:432,464,554,655`.
- `devolver_validacao_recusada`: `app/chamados/services/validacao.py:693`.
- `devolver_paridade_bug_par_103`: `app/chamados/management/commands/bug_par_103.py:106`.
- `descarimbar_em_lote`: `app/chamados/models.py:1629`; `app/chamados/services/disputa_emissao.py:1648`.
- `carimbar` da pergunta: `app/chamados/reconciliador.py:406,596`; `app/chamados/models.py:3414`; `app/ponto/services/cartorio.py:1314,1873`; `app/chamados/services/materializacao.py:1043,1944,2139`; `app/chamados/services/disputa_emissao.py:2008,2082`. O AST também acha `app/api/views.py:24` e `app/api/views_core.py:23`, mas esses `carimbar` nominais precisam desambiguação [nao verificado].

**Todos os arquivos de produção encontrados pelo AST com leituras/seleções de R/I/V/autor**

Legenda: referências abaixo incluem strings de auditoria/seleção de campos, além de leituras operativas. Arquivos ausentes da lista fechada do brief são **NOVO**; mesmo nos arquivos conhecidos, o brief não listava todas essas funções. Não confundir com o censo dos quatro templates, que o outro explorer está fazendo.

| Arquivo BASE | Linhas de acesso/lookup/seleção |
|---|---|
| `app/chamados/models.py` | `310,344,1533,1614,1923-1925,2007,2139,2949-2950,3151-3158,3237-3247,3331,3370-3383,3496,3568,3584-3586,3665,3688-3710,4091-4105` |
| `app/chamados/juizes.py` | `352,364,856-857,1360,1389,1423,1534,1541,1552,1572,1580,1744,1753,1779,1926,2030,2035,2052` |
| `app/chamados/views.py` | `1280,1416` |
| `app/chamados/views_cobrar.py` | `169,176,291` |
| `app/chamados/templatetags/chamados_tags.py` | `139,166` |
| `app/chamados/services/fio.py` | `187,377,706,1279,1291,1613` |
| **NOVO** `app/chamados/services/validacao.py` | `173,175,181,262-263,269,281,343,356,380,489,553,594,597,701,810,813,823-824,843,848,975,996,1094-1109,1167` |
| **NOVO** `app/chamados/services/materializacao.py` | `199,201,546,574,625,671,678,718,733,770,869,950,1047,1109,1150-1152,1311,1352,1534,1558,1567,1619,2064-2065,2082` |
| **NOVO** `app/chamados/services/disputa_emissao.py` | `1484-1485,1537-1538,1635-1636,1663,1692,1700,1703,1735,1843,1861,2062,2074,2079,2091` |
| **NOVO** `app/chamados/services/acoes_disputa.py` | `97,132,347,355` |
| **NOVO** `app/chamados/services/becos.py` | `30` |
| **NOVO** `app/chamados/services/declaracao_texto.py` | `227,417,419,687` |
| **NOVO** `app/chamados/services/explicador.py` | `55,102-103` |
| **NOVO** `app/chamados/services/foto_e4b.py` | `180` |
| **NOVO** `app/chamados/services/historico.py` | `99,101,109` |
| **NOVO** `app/chamados/services/painel.py` | `67,262,270-273` |
| **NOVO** `app/chamados/services/passivo_validado.py` | `66` |
| **NOVO** `app/chamados/services/pergunta_no_app.py` | `81` |
| **NOVO** `app/chamados/services/raiox.py` | `138,147` |
| **NOVO** `app/chamados/services/verbos.py` | `264-270,335-345` |
| **NOVO** `app/chamados/services/vigia_carimbo.py` | `98` |
| **NOVO** `app/chamados/services/vigia_fio.py` | `636,746,749` |
| **NOVO** `app/chamados/reconciliador.py` | `365,631,1050,1083` |
| **NOVO** `app/chamados/canal.py` | `65` |
| **NOVO** `app/chamados/veredito_lavrado.py` | `207-208` |
| **NOVO** `app/api/views.py` | `1163-1165,2561-2563,2577,2620,2637` |
| **NOVO** `app/api/views_mensageria.py` | `567,569-570` |
| **NOVO** `app/colaboradores/services/calendario.py` | `504` |
| **NOVO** `app/colaboradores/services/ficha.py` | `171` |
| **NOVO** `app/folha/previa.py` | `142` |
| **NOVO** `app/ponto/services/cartorio.py` | `1641` |
| **NOVO** `app/ponto/services/dia_do_colab.py` | `70` |
| **NOVO** `app/ponto/services/flip_auto.py` | `130` |
| **NOVO** `app/ponto/services/procedencia_batida.py` | `71-74` |
| **NOVO** `app/ponto/services/veredito_celula.py` | `197` |
| **NOVO** `app/ponto/management/commands/reconciliar_grade.py` | `461` |
| **NOVO** `app/chamados/management/commands/auditar_invariantes_chamados.py` | `177,205,592,1075-1076,1105-1106,1576,1604-1605,1657,1768,1780` |
| **NOVO** `app/chamados/management/commands/backfill_decisoes_pergunta.py` | `56,60-61,102,326,504` |
| **NOVO** `app/chamados/management/commands/bug_par_103.py` | `68` |
| **NOVO** `app/chamados/management/commands/curar_perguntas_em_limbo.py` | `32,46,69` |
| **NOVO** `app/chamados/management/commands/dedup_perguntas_fato.py` | `75` |
| **NOVO** `app/chamados/management/commands/encerrar_perguntas_com_fato.py` | `32` |
| **NOVO** `app/chamados/management/commands/reabrir_orfas_enterradas.py` | `68` |
| **NOVO** `app/chamados/management/commands/validar_classe_a.py` | `80` |
| **NOVO** `bin/censo_folha.py` | `28-29` |
| **NOVO** `bin/sombra_regras_pessoais.py` | `85` |

`config/crons.py` só apareceu em comentários para os campos crus, mas os crons podem consumir as funções reais indiretamente; por isso não deve ser inventado escritor ali a partir do grep (`app/config/crons.py:1753-1759`).

**Derivados/portas que o censo precisa acompanhar**

- `estado` segue cru: V tem precedência `app/chamados/models.py:3151-3153`, I classifica pendente/respondida `:3155,:3158`; `respondida` segue apenas I `:3663-3665`; `resposta_display` continua a resolver o texto de R `:3686-3710`. O contrato de F2/antiga F1 não pousou nesses corpos (`app/chamados/models.py:3151,:3665,:3688`).
- `viva` delega a `pergunta_viva` (`app/chamados/models.py:3512`). `q_vivas` lê V cru em `app/chamados/models.py:3496`.
- `resposta_enviada` tem um escritor: `app/chamados/services/fio.py:359`; chama `espera_o_texto_do_colaborador`, que lê I cru e `respondida_sem_texto` (`app/chamados/services/fio.py:177-187`). Outros leitores dessa função: `app/chamados/services/fio.py:174` e `app/chamados/services/cobranca.py:241`.
- `estado_aplicacao` tem escritor de apresentação `app/chamados/services/fio.py:376`; os fallbacks estão em `:377-384`. **NOVO** filtro `aplicacao_da_resposta` faz a mesma consulta à testemunha `app/chamados/templatetags/chamados_tags.py:132-141`; nenhum dos dois é juiz independente.
- `estado_aplicacao_pergunta`: consumidores diretos `app/chamados/services/materializacao.py:339,757`; `app/chamados/services/explicador.py:124`; `app/chamados/templatetags/chamados_tags.py:136`; alias de `models` em `app/chamados/services/fio.py:376`.
- **NOVO** `espera_declarado_x_gravado` filtro `app/chamados/templatetags/chamados_tags.py:121-128` delega à função real; outros consumidores diretos da função: `app/chamados/services/validacao.py:611` e `app/chamados/services/explicador.py:132`.
- `leitura_da_resposta` mudou de `:121` no brief para `app/chamados/templatetags/chamados_tags.py:145`; lê R via `getattr` em `:166` e chama classificação real `:171`. O irmão `frase_resposta` também chama classificação real `:198` (**NOVO no censo**).
- `respostas_anteriores` lê sequência `app/chamados/models.py:3667-3683`; consumidor Python `app/chamados/services/disputa_emissao.py:2123`.
- `todas_respondidas`: escritor `app/chamados/services/fio.py:386-391`.
- Exceção A20: campos declarados `app/chamados/models.py:2563-2564`; único escritor encontrado `app/chamados/services/validacao.py:907-909`; leitores Python `app/chamados/services/validacao.py:905`, `app/chamados/services/materializacao.py:169,1851`.

**Conservação e sequência**

Chamadas de `linhas_da_projecao_sem_linha`, além das duas citadas no brief: `app/chamados/models.py:1613,3129`; `app/chamados/services/validacao.py:167,272,345`; `app/chamados/services/disputa_emissao.py:1480,1673`; **leitor NOVO** `app/chamados/services/tarefa_do_dia.py:430`; commands `app/chamados/management/commands/backfill_decisoes_pergunta.py:67` e `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`. Implementação atual compara `(pergunta, ato, instante)` em `app/chamados/models.py:4091-4105`.

Escritor material das decisões é `DecisaoPergunta.registrar` → `bulk_create`, `app/chamados/models.py:4120`. Outros chamadores além dos acima: `app/chamados/models.py:2005,2137`; `app/chamados/services/materializacao.py:1045`; `app/ponto/services/veredito_celula.py:195`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`. Leitura adicional de sequência **NOVO** em `app/chamados/services/tarefa_do_dia.py:421`, foto/exportação em `app/chamados/services/foto_e4b.py:280`, backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:99`.

**R2 e REDs — cuidado com fixture**

- R2 ainda pode morder o template porque prefill lê R em `app/templates/chamados/partials/_card_atendimento.html:114,148,154`; mas o estado `R='velho', I=None` agora não pode nascer por `save` normal: `app/chamados/models.py:3574-3590`. Legado intocado é exceção explícita `:3587-3588`. Para RED futuro usar legado caracterizado/objeto em memória, sem burlar a trava: decisão de construção é do responsável; aqui só registro.
- A reabertura real limpa o par em `app/chamados/services/disputa_emissao.py:1609`; a ressuscitação também limpa o par e conserva anteriores sem gerar REABERTURA em `app/chamados/models.py:1615-1630`. Logo “todo legado divergente nasce hoje pela reabertura” seria falso.
- Provas existentes para esse corte: `app/chamados/tests/test_f1nova_b1_juiz_respondida.py::RecusaTextoSemCarimboTest::test_RED_create_com_texto_sem_instante_e_recusado` (`:103`), `::test_MORDE_legado_texto_sem_instante_intocado_segue_e_sair_dele_e_legal` (`:177`), `::test_RED_lote_que_zera_so_o_instante_de_quem_tem_texto_e_recusado` (`:206`); ressuscitação sem REABERTURA `::RessuscitacaoZeraAResposta::test_RED_ressuscitar_zera_o_par_com_o_anterior_na_trilha_sem_REABERTURA` (`:270`).
- BUG-PAR já tem provas de interface novas: `app/chamados/tests/test_bug_par_projetado.py::BugPar163XDeclaradoTest::test_RED_testemunha_que_falha_no_modal_nao_oferece_validar` (`:1155`), `::test_RED_modal_mostra_o_rotulo_e_nao_o_botao_de_validar` (`:1287`), `::test_RED_card_da_pagina_mostra_o_rotulo_e_nao_o_botao_de_validar` (`:1299`). Esses não provam migração de R/I/V para ato.

**Interseção B2 para a F7 e seu grafo ampliado**

Diretos: `app/templates/chamados/partials/_pergunta_card.html:42`, `app/templates/chamados/partials/modal_fio.html:228`, `app/chamados/models.py:3144`, `app/chamados/juizes.py:1389`, `app/chamados/services/fio.py:190`, `app/chamados/views.py:397`, `app/chamados/views_cobrar.py:291`.

Autoridades/escritores do censo ampliado também listados pelo anexo B2: `app/api/views.py:2627`, `app/chamados/canal.py:65`, `app/chamados/reconciliador.py:631`, `app/chamados/services/acoes_disputa.py:105`, `app/chamados/services/becos.py:30`, `app/chamados/services/declaracao_texto.py:419`, `app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/materializacao.py:852`, `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/services/validacao.py:161`, `app/chamados/services/verbos.py:264`, `app/chamados/services/vigia_fio.py:636`, commands de auditoria `:1075`, backfill `:67`, dedup `:75`, `app/colaboradores/services/calendario.py:504`, `app/colaboradores/services/ficha.py:171`, `app/folha/previa.py:142`, `app/ponto/services/cartorio.py:1641`, `app/ponto/management/commands/reconciliar_grade.py:461`. O fato de estarem em voo vem exclusivamente do anexo, não há árvore B2 disponível: [nao verificado].

## 16/11 23:08 · sessao 0642cf92186e · - · - · origem=Ronald (tmux)

Leitura concluída sem alteração. O censo anexo foi reproduzido em memória com `python3`; nenhum acesso a banco/teste Django foi feito. O vínculo desta cópia ao hash é o fornecido pelo usuário; não existe `.git` para confirmar o HEAD [nao verificado]. [nao verificado]

**Resultado dos quatro templates:** o regex do anexo produz 126 ocorrências: `_card_atendimento.html` 35, `_lote_preview.html` 1, `modal_fio.html` 75, `_pergunta_card.html` 15. No anexo eram respectivamente 35, 1, 33 e 8: +49 ocorrências, sem saída das 22 linhas R/V antigas. Os novos pontos estão nos estados de aplicação, não em migração das leituras R/V. (`app/templates/chamados/partials/_card_atendimento.html:106`; `_lote_preview.html:24`; `modal_fio.html:230`; `_pergunta_card.html:43`.)

### Censo exato reproduzido

Abreviações: `I=respondida_em`, `V=validada_em`, `R=resposta_colab`; cada linha abaixo pertence ao arquivo indicado.

**`app/templates/chamados/partials/_card_atendimento.html` —35, igual ao anexo:**

- `77 p.viva IF`
- `100 p.resposta_enviada IF`
- `106 p.resposta_display SHOW; p.resposta_colab SHOW; p.respondida_em SHOW`
- `114 p.resposta_colab IF`
- `141 p.resposta_colab IF`
- `143 p.resposta_display SHOW; p.resposta_colab SHOW; p.resposta_enviada IF; p.respondida_em SHOW`
- `148 p.resposta_colab SHOW`
- `154 p.resposta_colab IF`
- `161 p.resposta_enviada IF`
- `182 p.resposta_colab IF`
- `183 p.resposta_display SHOW; p.resposta_colab SHOW`
- `189 p.pendencia_admin IF; p.estado IF`
- `192 p.via_resolucao IF ×10; p.recusa_motivo IF; p.recusa_motivo SHOW`
- `390 pj.resposta_enviada IF`
- `391 pj.resposta_display SHOW; pj.resposta_colab SHOW; pj.respondida_em SHOW`

**`app/templates/chamados/partials/_lote_preview.html` —1, igual ao anexo:** `24 pergunta.resposta_colab SHOW`; a expressão real é `x.pergunta.resposta_colab`, cuja raiz `x` o regex omite (`app/templates/chamados/partials/_lote_preview.html:24`).

**`app/templates/chamados/partials/_pergunta_card.html` —15:**

- `20 perg.validada_em IF`
- `23 perg.validada_em IF; perg.validada_em SHOW`
- `24 perg.respondida_em IF`
- `29 perg.respondida_em IF`
- `31 perg.resposta_display SHOW`
- `32 perg.respondida_em SHOW`
- `42 perg.validada_em IF`
- **NOVO:** `44 ap.estado IF ×3; 45 ap.estado IF; 47 ap.estado IF ×2; 49 ap.estado IF`

**`app/templates/chamados/partials/modal_fio.html` —75:**

- `119 desenlace.estado SHOW`
- `170 perg.data_conferida_em IF`
- `171 perg.data_conferida_em SHOW; perg.data_conferida_por IF; perg.data_conferida_por SHOW ×2`
- `185 perg.respondida_em IF`
- `217 perg.respondida_em IF`
- `219 perg.resposta_colab SHOW`
- `220 perg.respondida_em SHOW`
- `222 perg.pendencia_admin IF`
- `226 perg.recusa_motivo IF; perg.recusa_motivo SHOW`
- `228 perg.validada_em IF`
- `230 perg.estado_aplicacao IF ×10`
- `232 perg.estado_aplicacao SHOW`
- `234 perg.estado_aplicacao IF ×2`
- `236 perg.estado_aplicacao SHOW`
- `238 perg.estado_aplicacao IF ×2`
- `240 perg.estado_aplicacao SHOW`
- `242 perg.estado_aplicacao IF ×2`
- `244 perg.estado_aplicacao SHOW`
- `246 perg.estado_aplicacao IF ×2`
- `248 perg.estado_aplicacao SHOW`
- `252 perg.estado_aplicacao IF ×10`
- `254 perg.estado_aplicacao SHOW`
- `256 perg.estado_aplicacao IF ×2`
- `258 perg.estado_aplicacao SHOW`
- `260 perg.estado_aplicacao IF ×2`
- `262 perg.estado_aplicacao SHOW`
- `264 perg.estado_aplicacao IF ×2`
- `266 perg.estado_aplicacao SHOW`
- `268 perg.estado_aplicacao IF ×2`
- `270 perg.estado_aplicacao SHOW`
- `274 perg.estado_aplicacao IF ×8`
- `361 chamado.todas_respondidas IF`
- `406 disputa_obj.fechada_em IF; disputa_obj.fechada_em SHOW`
- `464 it.estado IF; 467 it.estado IF; 468 it.estado IF; 469 it.estado IF`

No modal, as leituras antigas R/V continuam em 185/217/219/220/228. O bloco `estado_aplicacao` foi ampliado em 230–274; os leitores posteriores deslocaram +27: 334→361,379→406,437→464,440→467,441→468,442→469. (`app/templates/chamados/partials/modal_fio.html:185`; `:217`; `:228`; `:230`; `:274`; `:361`; `:406`; `:464`.)

### BUG-PAR e Validar

- O card mantém o gate exterior cru `{% if not perg.validada_em and pode_agir %}` na linha42 e o gate cru `respondida_em` na29. O filtro novo `aplicacao_da_resposta` em43 chama a função real `estado_aplicacao_pergunta(perg.disputa,perg)` e, se não obtiver estado para resposta hora não vazia, devolve indisponível. Ele próprio lê `resposta_colab` em139. (`app/templates/chamados/partials/_pergunta_card.html:29`; `:42`; `:43`; `app/chamados/templatetags/chamados_tags.py:132`; `:136`; `:139`.)
- Card44 exibe detalhe para `bloqueada_competencia`, `bloqueada_competencia_entregue`, `bloqueada_sem_dia_do_fato`; card45/47/49 retiram Validar para indisponível, entregue, sem dia e espera declarado x gravado. `bloqueada_competencia` conserva Validar. Botões de validação estão81 e105; Rejeitar125 permanece fora desse condicional de aplicação, mas dentro dos gates I/V. (`app/templates/chamados/partials/_pergunta_card.html:44`; `:45`; `:47`; `:49`; `:81`; `:105`; `:125`.)
- Modal mantém “✓ Validada” condicionado a V228. Os estados entregue e sem dia aparecem no ramo validada230 e no não validada252. O gate274 retira Validar para espera, indisponível, entregue e sem dia; conserva o toque para competência apenas fechada. Validar implausível296, normal311. Rejeitar325 e Corrigir330 dependem de `not perg.em_disputa_fechada`316. (`app/templates/chamados/partials/modal_fio.html:228`; `:230`; `:252`; `:274`; `:296`; `:311`; `:316`; `:325`; `:330`.)

### Provas existentes novas e antigas

- `app/chamados/tests/test_bug_par_volta.py::test_f6_sem_k6_conserva_validar_k6_esconde_nos_dois_htmls` (125): os dois HTMLs mantêm Validar sob F6 e o retiram sob K6; asserção143.
- `...::test_matriz_linha1_entregue_fora163_recusa_estado_proprio_esconde_validar` (222): estado entregue, detalhe e ausência do botão nos dois HTMLs231; porta retorna entregue233, não altera foto persistida235.
- `...::test_matriz_linha2_fechada_nao_entregue_conserva_o_toque_e_o_motivo` (237): contrato F6 conserva toque, validação+devolução, motivo competência aprovada240.
- `...::test_matriz_linha5_fora163_sem_dia_do_fato_estado_proprio` (304): detalhe sem dia, botão ausente nos dois HTMLs314, porta concorda318, K6 não chamado319, foto persistida igual321.
- `app/chamados/tests/test_bug_par_projetado.py::test_RED_modal_mostra_o_rotulo_e_nao_o_botao_de_validar` (1287): ausência de Validar para as duas metades1296–1297.
- `...::test_RED_card_da_pagina_mostra_o_rotulo_e_nao_o_botao_de_validar` (1299): frase, botão ausente1304–1305; resposta igual volta a exibir1308.
- `...::test_RED_testemunha_que_falha_no_modal_nao_oferece_validar` (1155): asserções1166–1167.
- `app/chamados/tests/test_ui_resposta_diz_o_que_e.py::test_o_cartao_da_PAGINA_RENDERIZA_a_frase_e_o_rotulo` (229): render do fragmento226, afirma frase/data de referência238–242; não prova texto/data da leitura dos atos.
- `app/chamados/tests/test_selo_teto_modal.py::test_TETO_do_modal_prod_like` (88): GET real85 e teto93, ainda existente.

Esses testes foram lidos, não executados [nao verificado]. [nao verificado]

### Grafo/contexto reconfirmado

| Parcial | Raiz/contexto atual | Prova |
|---|---|---|
| card | `meu_atendimento`: `montar_chamados_fio(colaborador,so_janela=True)`; includes em127/144; casca dinâmica | `app/chamados/views.py:569`; `app/templates/chamados/meu_atendimento.html:1`; `:127`; `:144`; `app/core/context_processors.py:210` |
| pergunta | detalhe inclui310; lista ORM sem prefetch352 e filtrada `pergunta_viva`358; casca base fixa1 | `app/templates/chamados/detalhe_local.html:1`; `:310`; `app/chamados/services/acoes_chamado.py:352`; `:358` |
| pergunta htmx | ORM só `select_related('disputa__colaborador')`; contexto só perg/pode_agir | `app/chamados/views.py:399`; `:404`; `:405` |
| modal | Prefetch decisões já existe; `estado_aplicacao` anexado com função real e fallback indisponível; contexto parcial monta com explicação | `app/chamados/services/fio.py:263`; `:376`; `:377`; `:384`; `:483`; `:492`; `:542`; `app/chamados/views.py:927`; `:1399` |
| lote | função real fila; usa por_classe LOTE_A | `app/chamados/views_cobrar.py:361`; `:363` |

**NÃO há include `_pergunta_resposta_lida` nos quatro arquivos BASE.** Só o modal possui includes: cabeçalho13 e relatório geo525; os demais três não possuem include. O cabeçalho lê ficha.cabecalho, sem projeção de pergunta (`app/templates/colaboradores/partials/_cabecalho_ficha.html:7`; `:8`; `:9`); o relatório atual não contém os campos R/V pesquisados [nao verificado para leituras transitivas Python].

### Limites do censo

- Perde filtros `perg|leitura_da_resposta` card43/modal274 e `perg|aplicacao_da_resposta` card43: não há `.campo` no chamador, mas os filtros leem a projeção em tags166 e139 e chamam juízes reais171/136. (`app/templates/chamados/partials/_pergunta_card.html:43`; `app/templates/chamados/partials/modal_fio.html:274`; `app/chamados/templatetags/chamados_tags.py:139`; `:166`; `:171`; `:136`.)
- Perde `perg.respostas_anteriores` modal205 e leituras `v.ato_texto`, `v.ato_em`, originais e autores em210/212: nomes ausentes de CAMPOS do anexo. (`app/templates/chamados/partials/modal_fio.html:205`; `:210`; `:212`.)
- Não segue includes, nem resolve alias; em lote24 reporta `pergunta.resposta_colab` e perde a raiz `x`; identifica prefixo `perg.estado_aplicacao`, sem registrar separadamente `.estado`/`.detalhe`. (`app/templates/chamados/partials/_lote_preview.html:24`; `app/templates/chamados/partials/modal_fio.html:230`; `:232`.)
- Classifica qualquer `{% %}` como IF, inclusive atribuição `{% with %}`; o parser Django é necessário para a classificação semântica. (`app/templates/chamados/partials/modal_fio.html:274`.)
- Mascara comentários HTML, embora Django processe tags dentro de HTML comments; **nos quatro arquivos atuais nenhum bloco `<!-- -->` contém `{{` ou `{%`**, conforme varredura em memória. Portanto há risco no algoritmo, mas nenhum leitor vivo atual perdido dessa maneira [nao verificado por parser Django].

### Todos os filtros encontrados

- Card: `date`5,6,50,61,90,106,143,170,391,436; `rotulo`17; `default`17,50,106,143,148,183,391; `truncatechars`17,39; `length`33; `stringformat`154; `lower`192; `slice`241×3,306; `make_list`306; `yesno`412. (`app/templates/chamados/partials/_card_atendimento.html`, linhas indicadas.)
- Lote: `length`7,9; `default`18,24; `date`24; `truncatechars`24. (`app/templates/chamados/partials/_lote_preview.html`, linhas indicadas.)
- Pergunta: `date`23,32; `join`38; `leitura_da_resposta`43; `aplicacao_da_resposta`43. (`app/templates/chamados/partials/_pergunta_card.html`, linhas indicadas.)
- Modal: `pluralize`16×2,18,19×2,46×2,399; `default`44,83,171,210,212,495,514×2,515×2,556; `date`86,130,167,168,171,183,210,212×2,220,406,482,498,516; `espera`102; `upper`119; `get_dict`123,527,574,590,622,633,635,663,679,705; `data_do_fato`162; `data_do_rotulo_antigo`162; `data_do_fato_diverge`163; `pergunta_cega`185; `lower`212; `leitura_da_resposta`274; `respostas_que_o_reabrir_apaga`394; `truncatechars`466,482,483,495,497; `par_tem_gps`508; `unlocalize`512×2,513×2,514×2,532,543,554,556. (`app/templates/chamados/partials/modal_fio.html`, linhas indicadas.)

`pergunta_cega` lê apenas motivo/pergunta (`app/chamados/templatetags/chamados_extras.py:8`; `:11`). Filtros data delegam à função do fio (`:15`; `:30`; `:37`; `:50`). `respostas_que_o_reabrir_apaga` delega a `perguntas_corrigiveis` (`app/chamados/templatetags/chamados_tags.py:222`; `:226`).

## 16/11 23:08 · sessao eca3963189f1 · - · - · origem=Ronald (tmux)

Leitura concluída sem banco, sem testes executados e sem escrita. As evidências abaixo são da cópia atribuída à BASE; a cópia não tem `.git`, portanto SHA efetivo não é verificável localmente. [nao verificado] [nao verificado]

**BUG-PAR / U163 / pontos que ficam com Ronald**

| Estado | Juiz | Operação | Persistido | Retorno | Prova existente |
|---|---|---|---|---|---|
| Fora163, dia do fato entregue | `competencia_do_fato` → K6; composição anterior às demais guardas (`materializacao.py:776-815`; `validacao.py:652-655`) | Validar, laço ou passivo | Nada novo em perguntas, DP ou Batida | `bloqueada_competencia_entregue` | `app/chamados/tests/test_bug_par_volta.py::test_fora163_entregue_k6_tres_caminhos_intactos` (162-188); `::test_matriz_linha1_entregue_fora163_recusa_estado_proprio_esconde_validar` (222-235) |
| Fora163 sem `data_evento` | `_k6_que_barra`, sem chamar K6 (`materializacao.py:808-810`) | Validar/tela | Nada novo | `bloqueada_sem_dia_do_fato`; Validar oculto | `test_bug_par_volta.py::test_matriz_linha5_fora163_sem_dia_do_fato_estado_proprio` (304-321; `assert_not_called`:319) |
| K6 aberto, FM aprovado (F6) | `competencia_fechada`, instante local do plantio (`ponto/janelas.py:75-89`) | Toque de validação é permitido, plantio recusado | VALIDACAO + DEVOLUCAO, validação limpa, motivo/nota COMPETENCIA_APROVADA, irmã intocada, zero Batida | Tela `bloqueada_competencia`, mantém Validar; porta `nao_gravou`, `devolvida=True` | `test_bug_par_volta.py::test_matriz_linha2_fechada_nao_entregue_conserva_o_toque_e_o_motivo` (237-273); laço `::test_matriz_linha2_laco_conserva_o_motivo` (378-389); passivo `::test_matriz_linha2_passivo_conserva_o_motivo` (391-403) |
| U163 declarado diferente do gravado, F6 aberto | `_espera_do_aval_163` via derivador (`materializacao.py:922-924,936-938`) | Validar/tela | Nada novo | `espera_declarado_x_gravado`; sem Validar | `test_bug_par_volta.py::test_matriz_linha3_163_declarado_x_gravado_mostra_a_espera` (275-287) |
| Nada barra, fora163 | Derivador/porta compartilhados (`materializacao.py:925-927`; `validacao.py:681-698`) | Validar | Par, C/VIA, uma VALIDACAO por metade | `validada`, `par=<irmã>` | `test_bug_par_volta.py::test_matriz_linha4_nada_barra_aplicavel_validar_grava` (289-302) |
| U163 com irmã fora163 entregue | Pergunta U163 é pulada; composição consulta irmã (`materializacao.py:795`; `validacao.py:219-225`) | Validar/tela | Zero Batida | **Já bloqueia hoje** com L1 | `test_bug_par_volta.py::test_k6_da_irma_fora163_testemunha_le_a_mesma_composicao_da_porta` (145-160) |
| U163 entregue, irmã ausente ou também U163, FM aberto, demais guardas satisfeitas | Hoje K6 pula ambas; CORTE-U163-NO-K6 manda consultar a pergunta | Complemento futuro F4 | Pelo corte: nada novo; hoje pode seguir até plantio (`materializacao.py:795,1830,1887-1893`; `validacao.py:652-655,681-687`) | Futuro L1; hoje sucesso depende das condições abaixo | **NENHUM** teste do novo corte localizado. Condição irmã tem prova adversária no teste 145-160 | [nao verificado]
| U163 sem dia, sem irmã consultável pelo K6 | Hoje exclusão `not no_universo_163`; corte estende L5 (`materializacao.py:795,808-810`) | Complemento futuro F4 | Pelo corte, nada novo | Futuro L5 | **NENHUM** para U163; teste atual L5 é fora163 (304-321) | [nao verificado]
| U60 entregue/sem dia precisa aparecer em “PAUTA DP” | Comando tem laço U103 (53-63) e outro **excluindo U163** (71-77) | Apenas saída textual | Não cria `Pauta` (`bug_par_103.py:85-89`) | Retirar exclusão K6 não muda essa lista | **NENHUM** do complemento. **Destino/abrangência permanecem com Ronald**, conforme revisão fornecida |
| U163 não materializada ganha L1/L5 | Contador chama testemunha; estados novos caem em `outras` (`materializacao.py:338-347`) | Contar | Nada | Soma em `universo` e `pendentes_aval` (`351-356`) | **NENHUM** do destino futuro. **Balde permanece com Ronald**; conservação atual explicitada `316,351-356` |

`F6 status exportado` não está provado pelo teste F6 de 217-220, que monta somente `aprovado`; existe fila expressa para isso em `app/docs/BACKLOG.md:824`.

**RED U163 corretamente condicionado**

Cadastro deve montar escala/vínculo, pergunta hora de intervalo dentro U163, resposta e RESPOSTA correspondentes, data do fato coerente com contexto, texto igual ao marco efetivamente plantado, nenhuma validação anterior, K6 real entregue, FM aberto no instante local do plantio e **irmã ausente ou também U163**. Deve também assegurar ausência de bloqueio de data, implausível sem confirmação, colisão/paridade, defeito de fato e rejeição C6. A porta exige resposta em `validacao.py:594`, ausência de V em `597`, passa declarado×gravado em `609-614`, implausível em `615-618`, K6 em `652-655`, depois plantio em `681-687`; `_materializar_uma` acrescenta data em `1850-1872`, espera em `1877-1881`, juiz de fato em `1899-1910`, C6 em `1944-1948`.

Pela regra do corte fornecido: L1, nada gravado; controle U163 em competência aberta continua validando. **MORDE-HOJE: sim, condicional à fixture acima**: exclusão está em `materializacao.py:795`; simplesmente “U163 + entregue + FM aberto” **não demonstra** falha atual por causa da irmã (`validacao.py:224`; teste 145-160). Resultado de produção atravessando todas as guardas = [nao verificado]. [nao verificado]

RED lista: U60 entregue/sem dia deve estar na saída textual decidida pelo corte; **morde hoje** porque a primeira lista só inclui U103 e a segunda exclui U163 (`bug_par_103.py:53,71-77`). O escopo exato do ajuste e eventual duplicidade U103 ficam com Ronald, conforme revisão fornecida.

RED contador: incluir caso U163 não materializado L1/L5 e cobrar conservação; **balde esperado ainda não decidido**, logo não inventar asserção de destino. Hoje ambos seriam `outras` e continuariam em `pendentes_aval` (`materializacao.py:347,351-356`).

**FILA-OPUS-C4-B: teste com autoridade real** [nao verificado]

O teste `app/chamados/tests/test_bug_par_volta.py::BugParF4RecorteTest.test_c4_f4_pendentes_aval_zera_pela_validacao_do_admin` (457-472) cobra exatamente pendentes 1→0, resolvidas 0→1, congelados=1 e conservação; porém herda o mock fixo K6=False de `_Col960Fixture` (`test_bug_par_projetado.py:45-53`). Sua comparação “K6 inalterado” não prova a autoridade real (`test_bug_par_volta.py:461,471-472`).

Montagem real precisa:

- Declarar U60/U163 por PK sem instalar patch em `folha.export.competencia_entregue`; hoje universo e patch K6 estão acoplados no mesmo `if` (`test_bug_par_projetado.py:45-53`).
- Cenário resolvível com escala, batidas de chão E/S e respostas nos marcos (`test_bug_par_projetado.py:59-85,103-110`; `test_bug_par_volta.py:459-460`).
- K6 **real** falso: ausência de `ExportacaoDominio` vigente válido ou vigente revogado sem substituta; usar a função real, cuja autoridade é `folha/export.py:588-594`. Caso sem TXT é provado em `app/folha/tests/test_k6_competencia_entregue.py::JuizTest.test_sem_txt_nao_entregue` (60-63); revogado em `::test_MORDE_vigente_revogado_sem_substituta_nao_e_entregue` (72-76).
- Cobrar os mesmos 1→0/0→1/conservação e que empresa/competência/exportações permaneçam iguais antes/depois; controls de TXT vigente/substituição podem reutilizar a estrutura dos testes K6 (65-91). [nao verificado]
- Para o controle entregue, criar `ExportacaoDominio` vigente da empresa/competência, com os campos do modelo `folha/models.py:75-86`; o juiz aceita vigente não invalidado (`folha/export.py:588-594`). Após complemento U163, a porta deve bloquear L1 e não zerar pendentes por “validação”; destino de contador com Ronald.

**Nenhum leitor de produção do contador muda só para acrescentar esse teste.** Busca global do nome e chaves encontrou consumo de produção em `app/chamados/management/commands/bug_par_103.py:79-83`; o próprio produtor e docstrings estão em `materializacao.py:295-356`. Templates consomem estado da testemunha, não esse contador (`materializacao.py:852-873`; teste tela `test_bug_par_volta.py:199-210`). Escopo de ampliação de contador pelo corte U163 é assunto distinto, pendente de balde.

**Provas úteis dos demais estados F4**

| Estado | Comportamento/prova |
|---|---|
| Resposta normal | R + instante + trilha, `(True,None)` (`disputa_emissao.py:1481-1482,1522-1526`); `test_porta_resposta_pergunta.py::test_responder_grava_a_resposta_e_UMA_trilha` (174-182) |
| Texto vazio | Nada escrito, `(False,erro)` (`disputa_emissao.py:1446-1448`); `test_porta_resposta_pergunta.py::test_MORDE_resposta_vazia_nao_escreve_nada` (184-194) |
| Resposta nova sobre validada | Limpa V/autor (`disputa_emissao.py:1484-1496`); `test_porta_resposta_pergunta.py::test_MORDE_resposta_NOVA_invalida_a_validacao_anterior` (225-237) |
| Reabertura | Foto DP, R/V limpos, C/VIA pela porta lote, n (`disputa_emissao.py:1609,1631-1648`); `test_o261_e1_ausencia_rejeitada_devolve.py::test_RED_rejeitada_devolve_as_absorvidas_a_pendente` (88-103) |
| Auto com voz já existente | False sem query (`disputa_emissao.py:1663-1664`); `test_e3_10_cura_no_escritor.py::test_voz_do_colaborador_continua_no_op_mesmo_com_texto_vazio` (80-85) |
| Auto texto válido | R/RESPOSTA, True (`disputa_emissao.py:1672-1680`); `test_e3_10_cura_no_escritor.py::test_MORDE_texto_valido_segue_escrevendo_o_par` (71-78) |
| Auto vazio | Exceção antes de escrever (`disputa_emissao.py:1672`); `test_e3_10_cura_no_escritor.py::test_RED_texto_vazio_ou_so_espacos_e_recusado_sem_escrever` (55-69) |
| Limbo reparado | Instante da trilha, RESPOSTA retroativa, True (`disputa_emissao.py:1692-1704`); `test_curar_perguntas_em_limbo.py::test_MORDE_carimba_com_o_instante_da_trilha` (29-42); fora meio estado `::test_nao_age_fora_do_meio_estado` (44-52) |
| Recobrar | Conta `respondida_em NULL`, não apaga fala (`disputa_emissao.py:1734-1735`); `test_recobrar_questionario.py::test_MORDE_recobrar_nao_apaga_resposta_de_ninguem` (122-141) |
| Falta/atestado | Ausência, sem Batida, carimbo C (`materializacao.py:944-978`); `test_declara_falta.py::test_falta_dia_inteiro_materializa_ausencia` (37-59), `::test_atestado_materializa_ausencia_atestado` (61-73) |
| Absorção e rejeição | Carimbo atual identifica absorvidas; volta com foto, não duplica (`materializacao.py:1085-1173`); `test_o261_e1_ausencia_rejeitada_devolve.py::test_RED_rejeitada_devolve_as_absorvidas_a_pendente` (88-103), `::test_idempotente_rejeitar_2x_e_1_devolucao` (106-117) |
| Hora vazia materializador | Recusa falante, sem carimbo C (`materializacao.py:1311-1323`); `test_a27_recusa_com_dono.py::test_hora_vazia_fica_pendente_com_motivo_e_followup` (38-50) |
| Colisão oposta | VIA_FATO_OCUPADO terminal (`materializacao.py:1646-1670`); `test_a27_recusa_com_dono.py::test_colisao_tipo_oposto_e_terminal_fato_ocupado` (52-62) |
| Juiz reprova Batida recém plantada | Rollback Batida e C (`materializacao.py:1899-1910`); `test_hx_materializa_depois_julga.py::test_reprovacao_do_juiz_desfaz_a_batida` (30-43) |
| Hora inválida/escape/texto do ato divergente | Hoje lê projeção (`materializacao.py:625,678,1311,1328`; fila `2063-2067`); **NENHUM** teste novo F4 de troca texto D01 encontrado nesta busca | [nao verificado]
| Paridade do ts nos três chamadores | `_ts_tipo_do_plantio` recebe só `carga` como kwarg (`materializacao.py:599`); não existe contrato `decisoes/agora` nesse helper na BASE; **NENHUM** RED novo F4 encontrado nesta busca |

**Sonda U163 só leitura, funções reais**

A etiqueta “aplicável” abaixo é a testemunha real, não prova de futura escrita: `estado_aplicacao_pergunta` é leitura (`materializacao.py:852-873`), enquanto a porta tem outras condições (`validacao.py:594-618`). Não se deve chamar `validar_pergunta`, nem “em rollback”: isso executaria escritores e efeitos. Para a unidade “validáveis hoje”, o script reporta a fila real e separa testemunha/implausível.

```python
# Django shell. Somente leitura. Não imprime nome, CPF nem resposta.
from collections import Counter
from django_tenants.utils import schema_context
from chamados.models import PerguntaDisputa
from chamados.juizes import perguntas_respondidas_a_validar, motivo_implausivel_validacao
from chamados.services.universo_bug_par import _carregar_universo_bug_par
from chamados.services.materializacao import (
    competencia_do_fato, estado_aplicacao_pergunta,
    respostas_intervalo_recusadas_paridade,
)
from chamados.services.validacao import entrega_que_barra_a_validacao

with schema_context('juliani'):
    universo = _carregar_universo_bug_par()
    fila_ids = {p.pk for p in perguntas_respondidas_a_validar()}
    numeros = Counter()
    ps = (PerguntaDisputa.objects.filter(pk__in=universo.U163)
          .select_related('disputa__colaborador__empresa', 'chamado_destino')
          .order_by('pk'))
    for p in ps.iterator():
        numeros['u163_presentes'] += 1
        k6 = competencia_do_fato(p, p.disputa.colaborador.empresa)
        if k6 is None:
            numeros['u163_sem_dia_do_fato'] += 1
            continue
        if not k6[2]:
            continue
        numeros['u163_competencia_entregue'] += 1
        estado = estado_aplicacao_pergunta(p.disputa, p)
        estado_nome = estado['estado'] if estado else 'fora_de_escopo'
        numeros['u163_entregue_estado_' + estado_nome] += 1
        if p.pk in fila_ids:
            numeros['u163_entregue_na_fila_real_de_validacao'] += 1
        if p.pk in fila_ids and estado_nome == 'aplicavel':
            numeros['u163_entregue_fila_testemunha_aplicavel'] += 1
            imp = motivo_implausivel_validacao(p)
            numeros['u163_entregue_aplicavel_' + (
                'exige_confirmacao_implausivel' if imp else 'sem_implausivel'
            )] += 1
            barra = entrega_que_barra_a_validacao(p)
            numeros['u163_entregue_aplicavel_' + (
                'com_bloqueio_real_validacao' if barra else 'sem_bloqueio_real_validacao'
            )] += 1
            print('CASO p%s col%s %02d/%s estado=%s implausivel=%s' % (
                p.pk, p.disputa.colaborador_id, k6[0], k6[1],
                estado_nome, bool(imp)))
    for rotulo in (
        'u163_presentes', 'u163_sem_dia_do_fato',
        'u163_competencia_entregue',
        'u163_entregue_na_fila_real_de_validacao',
        'u163_entregue_fila_testemunha_aplicavel',
        'u163_entregue_aplicavel_exige_confirmacao_implausivel',
        'u163_entregue_aplicavel_sem_implausivel',
        'u163_entregue_aplicavel_com_bloqueio_real_validacao',
        'u163_entregue_aplicavel_sem_bloqueio_real_validacao',
    ):
        print('NUMERO %s: %s' % (rotulo, numeros[rotulo]))
    for rotulo in sorted(k for k in numeros if k.startswith('u163_entregue_estado_')):
        print('NUMERO %s: %s' % (rotulo, numeros[rotulo]))
    c = respostas_intervalo_recusadas_paridade()
    for chave in ('pendentes_aval', 'resolvidas', 'congelados_no_recorte'):
        print('NUMERO contador_%s: %s' % (chave, c[chave]))
    for chave in ('aplicaveis', 'legitimas', 'outras', 'espera_declarado',
                  'bug_par_103', 'bug_par_103_pauta_dp', 'bug_par_103_sem_dia'):
        print('NUMERO contador_%s: %s' % (chave, len(c[chave])))
    soma = (c['pendentes_aval'] + c['resolvidas']
            + len(c['bug_par_103_pauta_dp']) + len(c['bug_par_103_sem_dia']))
    print('NUMERO contador_delta_conservacao: %s' % (soma - c['congelados_no_recorte']))
```

`perguntas_respondidas_a_validar` é fila real em `app/chamados/juizes.py:1749-1754`; `motivo_implausivel_validacao` é real em `2043-2063`; fonte congelada verificada em `universo_bug_par.py:12-30`. Números atuais do script = [nao verificado].

## 16/11 23:08 · sessao 7e1e748d3c13 · - · - · origem=Ronald (tmux)

Todas as referências de `models.py` dos M1a–M20 de `estados.md` continuam nos mesmos números e com os mesmos corpos projetivos na cópia; não há `_leitura_dos_atos` ou migração c3 nos leitores (`app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/models.py:1923`; `app/chamados/models.py:3496`). Suíte não executada; resultado de execução: [nao verificado].

| M | BASE: operação/retorno | Prova existente lida |
|---|---|---|
| M1a | Validação sem materialização retorna `validada`, antes de C6 (`app/chamados/models.py:3151`). | `app/chamados/tests/test_pergunta_estado.py::EstadoDaPerguntaTest.test_validada_nao_e_mais_viva`, asserções :41–42. |
| M1b | Validação + materialização retorna `resolvida`, sem testemunha nesse ramo (`app/chamados/models.py:3153`). | `app/chamados/tests/test_pergunta_estado.py::EstadoDaPerguntaTest.test_materializada_e_resolvida`, :44–48. |
| M2 sem instante | Viva sem instante → `pendente` (`app/chamados/models.py:3155`). | `app/chamados/tests/test_pergunta_estado.py::EstadoDaPerguntaTest.test_sem_carimbo_e_pendente_e_viva`, :28–31. |
| M2 com instante | Viva com instante → `respondida`, sem exigir texto nesse leitor (`app/chamados/models.py:3155`). | `app/chamados/tests/test_pergunta_estado.py::EstadoDaPerguntaTest.test_respondida_sem_validar_ainda_e_viva`, :33–37; caso instante com texto vazio neste leitor: NENHUM confirmado. |
| M3 | Morta + qualquer materialização → `resolvida` (`app/chamados/models.py:3156`). | `app/chamados/tests/test_pergunta_estado.py::EstadoDaPerguntaTest.test_materializada_sem_resposta_tambem_e_resolvida`, :50–54. |
| M4 | Morta sem validação/materialização ainda cai no instante (`app/chamados/models.py:3158`). | NENHUM específico confirmado para o estado de pergunta morta sem carimbo. |
| M3/M10 sem carimbo | C6 retorna viva antes da testemunha quando `materializada is None` (`app/chamados/juizes.py:1482`). | NENHUM específico confirmado para testemunha factual verdadeira sem carimbo. |
| M5 | `respondida` continua só `respondida_em is not None` (`app/chamados/models.py:3665`). | NENHUM direto confirmado para property contra último ato divergente. |
| M6 | Display lê texto projetado, não texto do ato (`app/chamados/models.py:3688`; `app/chamados/models.py:3710`). | NENHUM direto confirmado para divergência ato/projeção. |
| M7 | Posto/escala/opções continuam por projeção (`app/chamados/models.py:3693`; `app/chamados/models.py:3701`; `app/chamados/models.py:3708`). | `app/chamados/tests/test_valor_rotulo.py::RespostaDisplayValorTest.test_escolha_com_valor_resolve_rotulo`, :12; `test_escolha_string_pura_back_compat`, :16. Posto/escala: NENHUM confirmado. |
| M8 | Validação exclui da lista (`app/chamados/models.py:1923`). | `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py::PaiComConversaNoGateTest.test_RED_retratacao_pergunta_o_MESMO_juiz_do_veredito`, :289–290; `app/ponto/tests/test_supra_juiz_executor.py::ExecutorCsfTest.test_resposta_com_veredito_nao_barra`, :83–85. |
| M9 instante OU texto | Inclusão por instante ou texto mantém OR vigente (`app/chamados/models.py:1924`). | `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py::PaiComConversaNoGateTest.test_RED_retratacao_pergunta_o_MESMO_juiz_do_veredito`, :283–288 testa os DOIS pares ilegais. |
| M9 sem ambos | Lista exclui sem instante e sem texto; fechamento ainda consulta vida (`app/chamados/models.py:1924`; `app/chamados/models.py:1934`). | `app/ponto/tests/test_supra_juiz_executor.py::ExecutorCsfTest.test_sem_conversa_executa`, :68–69; `app/chamados/tests/test_c7_red_minimos.py::C7_01_PerguntaVivaRecusaTest.test_RED_fechar_com_pergunta_MUDA_viva_RECUSA`, :132–140. |
| M10 | Qualquer materialização exclui, mesmo se C6 viva (`app/chamados/models.py:1923`). | `app/ponto/tests/test_supra_juiz_executor.py::ExecutorCsfTest.test_resposta_com_veredito_nao_barra`, :83–85 cobre exclusão por materialização; combinação materializada E C6 viva: NENHUM confirmado. |
| M11 | Tranca mata C6 e lista normal exclui; CSF/retratação ignoram tranca (`app/chamados/juizes.py:1409`; `app/chamados/models.py:1926`). | `app/chamados/tests/test_c7_red_minimos.py::C7_08_CompetenciaTrancadaTest.test_RED_o_C6_le_a_tranca_sem_escrever`, :361–367; `test_RED_resposta_de_dia_trancado_nao_segura_a_disputa`, :377–382; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py::PaiComConversaNoGateTest.test_RED_dia_trancado_CSF_e_retratacao_dao_a_MESMA_resposta`, :303–311. |
| M12 | Q continua campo de validação e face SQL de resposta, contrato de continência (`app/chamados/models.py:3467`; `app/chamados/models.py:3481`; `app/chamados/models.py:3496`). | `app/chamados/tests/test_contract_pergunta_viva.py::DerivacaoNaoBifurcaTest.test_property_e_o_juiz_e_queryset_contem_nas_cinco_combinacoes`, :55–60. Degraus também em `test_viva_delega_ao_juiz.py::_Bateria._confere`, :42–46. |
| M13 | `_ancoradas` sem PK → []; com PK exige validação nula e destino/âncora; sem q_vivas (`app/chamados/models.py:341`; `app/chamados/models.py:344`). | PARCIAL: `app/chamados/tests/test_e4b_classe_e_memoria.py::[classe do teste a confirmar].test_RED_D3_carga_le_ancoradas_mesmo_morta_pelo_c6`, :898–910 confirma consumidor/carga; corpo ORM/PK nula: NENHUM confirmado. |
| M14 | Guarda antes do save; relavra destinos não validados depois do save (`app/chamados/models.py:221`; `app/chamados/models.py:296`; `app/chamados/models.py:310`). | PARCIAL: `app/ponto/tests/test_selo_chokepoint_escrita.py` declara casos `_gravar_estado` :124–125; prova específica da troca do filtro para ato: NENHUM. |
| M15 normal | Resposta sem veredito recusa antes de vivas (`app/chamados/models.py:1967`; `app/chamados/models.py:1978`). | `app/chamados/tests/test_c7_red_minimos.py::C7_03_RespostaSemVereditoRecusaTest.test_resposta_sem_veredito_RECUSA`, :160–166; `C7_01_PerguntaVivaRecusaTest.test_RED_fechar_com_pergunta_MUDA_viva_RECUSA`, :132–140. |
| M15 admin/desligamento | Carimba e escreve SILENCIO com texto projetado (`app/chamados/models.py:1999`; `app/chamados/models.py:2007`). | `app/chamados/tests/test_c7_red_minimos.py::C7_06_AdminEncerraTest.test_RED_com_motivo_fecha_e_carimba_as_mudas_com_trilha`, :261–279; `C7_05_DesligamentoPelaPortaTest.test_resposta_sem_veredito_SEGURA_a_disputa_mesmo_no_desligamento`, :234–242. Foto do texto SILENCIO divergente do ato: NENHUM confirmado. |
| M15 competência | Exceção fecha disputa sem carimbar pergunta (`app/chamados/models.py:1999`; `app/chamados/models.py:2013`). | `app/chamados/tests/test_c7_red_minimos.py::C7_08_CompetenciaTrancadaTest.test_RED_trancada_fecha_pela_porta_com_trilha_e_nao_carimba`, :329–350. |
| M16 | RETRATACAO fotografa projeção; conserva carimbo quando existente (`app/chamados/models.py:2137`; `app/chamados/models.py:2142`). | `app/chamados/tests/test_e1_entradas.py::RemoverMotivoRetrataTest.test_MORDE_a_retratada_morre_e_tem_linha`, :142–154 conserva texto e sequência. Foto divergente do ato: NENHUM confirmado. |
| M17 | Ressuscitar conserva linhas faltantes, zera instante E texto; NÃO escreve REABERTURA (`app/chamados/models.py:1613`; `app/chamados/models.py:1622`; `app/chamados/models.py:1629`). | `app/chamados/tests/test_f1nova_b1_juiz_respondida.py::RessuscitacaoZeraAResposta.test_RED_ressuscitar_com_SILENCIO_nao_devolve_ao_colaborador`, :334–335, helper compara tarefa antes/depois :319–329. `test_RED_ressuscitar_sem_decisao_nao_muda_a_tarefa_do_dia`, :331–332. |
| M18 | Simula descarimbo e C6 em cópia; marco aceso exclui (`app/chamados/models.py:1550`; `app/chamados/models.py:1553`; `app/chamados/models.py:1557`). | `app/chamados/tests/test_o261_ressuscita_respeita_o_marco.py::RessuscitaRespeitaOMarcoTest.test_RED_marco_ACESO_fica_morto_e_o_APAGADO_volta`, :108–128; `test_MORDE_sem_celula_o_juiz_diz_None_e_a_pergunta_VOLTA`, :151–159; `test_MORDE_pulou_TODAS_nao_muda_estado_e_nao_escreve_trilha`, :161–169. |
| M19 | Helper c3 ausente; properties continuam lendo projeção inclusive instância não salva (`app/chamados/models.py:3665`; `app/chamados/models.py:3688`). | `test_valor_rotulo.py` :6–12 confirma instância não salva. Custos existentes: `app/chamados/tests/test_o168_custo_da_guarda_constante.py::CustoDaGuardaTest.test_custo_da_guarda_E2_e_constante_com_1_e_3_disputas`, :64–68; `test_custo_constante_com_N_PENDENTES`, :70–76. Custo da hipotética migração: [nao verificado]. |
| M20 | Selo c3 ausente; teste AST atual exige materialização nula no corpo (`app/chamados/tests/test_contract_dedup_status.py:143`). | `app/chamados/tests/test_contract_dedup_status.py::ContratoDedupDistingueStatusTest.test_porta_humana_de_disputa_presta_contas`, :133–146. Selo F2 c3: NENHUM na cópia. |

**Referências de apresentação que CAÍRAM ou ganharam comportamento:**

- `_pergunta_card.html:23`, `:25`, `:29`, `:31` continuam correspondendo ao cabeçalho por projeção e ao display; o corpo da ação ganhou `bloqueada_sem_dia_do_fato` em :44/:47 e espera DECLARADO-X-GRAVADO em :49–51 (`app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/_pergunta_card.html:49`).
- Explicador antigo `:73/:77/:85/:93/:102/:139` não casa mais: estado agora :95; pendente :96; respondida :100; pendência admin :108; testemunha :116; sem gravação :134–142; pode_fechar :177 (`app/chamados/services/explicador.py:95`; `app/chamados/services/explicador.py:177`).
- Há mudança material adicional no explicador: ao testemunhar efeito, consulta `estado_aplicacao_pergunta` e `ESTADOS_DE_RECUSA` e marca `gravou=False`; sem efeito consulta `espera_declarado_x_gravado` antes da explicação de cadastro/sistema (`app/chamados/services/explicador.py:122`; `app/chamados/services/explicador.py:125`; `app/chamados/services/explicador.py:131`).
- Modal antigo `:436/:440/:442` não casa: bloco explicação agora :462; “Aguardando COLABORADOR” :467; aguardando validação :468; validada sem gravar :469 (`app/templates/chamados/partials/modal_fio.html:462`; `app/templates/chamados/partials/modal_fio.html:467`; `app/templates/chamados/partials/modal_fio.html:469`).
- `pergunta_no_app.py:52/:61/:81` preservados: SQL → C6 → partição por instante/disputa aberta (`app/chamados/services/pergunta_no_app.py:52`; `app/chamados/services/pergunta_no_app.py:61`; `app/chamados/services/pergunta_no_app.py:81`).

**Complementos conferidos no código:**

- Descarimbo sozinho não garante inclusão: validação remanescente, falta simultânea de instante/texto ou tranca continuam excluindo (`app/chamados/models.py:1923`; `app/chamados/models.py:1924`; `app/chamados/models.py:1926`).
- Ressuscitação zera ambos, logo serve como controle que NÃO volta à lista por resposta (`app/chamados/models.py:1629`; `app/chamados/models.py:1630`; `app/chamados/models.py:1924`).
- Tarefa usa último ato: passivo antes; sem decisão; RESPOSTA vazia; RECUSA/REABERTURA; DEVOLUCAO; VALIDACAO; SILENCIO; ABSORCAO (`app/chamados/services/tarefa_do_dia.py:154`; `app/chamados/services/tarefa_do_dia.py:156`; `app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:160`; `app/chamados/services/tarefa_do_dia.py:163`; `app/chamados/services/tarefa_do_dia.py:165`; `app/chamados/services/tarefa_do_dia.py:167`; `app/chamados/services/tarefa_do_dia.py:176`; `app/chamados/services/tarefa_do_dia.py:178`).

## 16/11 23:08 · sessao 791cd10eb62e · - · - · origem=Ronald (tmux)

Achado central: o contrato antigo de `leitura`/fold não pousou nesta BASE; pousou o juiz `pergunta_respondida`, que lê o par projetado instante+texto. A lei determina a migração dos leitores B/C/D na B2, e o código ainda conserva `estado`, `respondida` e `resposta_display` lendo a projeção. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`; `app/chamados/models.py:3151`; `app/chamados/models.py:3664`; `app/chamados/models.py:3688`.)

Premissas relevantes para consolidar:

| Premissa | Julgamento | Evidência BASE |
|---|---|---|
| F1 disponibilizou `LeituraDosAtos`, texto e instantes extraídos de fold | **CAIU**: o contrato pousado é `pergunta_respondida`; disponibilidade da API antiga = [nao verificado] | `app/chamados/juizes.py:1544`; `app/docs/LEIS.md:151` |
| D0/C1/P0/M0 leem V crua antes da vitalidade | **CONFERE** | `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1404` |
| U2 com V não nula é considerada morta e filtrada pelo juiz atual | **CONFERE** enquanto a projeção permanece assim | `app/chamados/juizes.py:1405` |
| `respondida`/`estado`/`resposta_display` já leriam o ato depois da F2 | **CAIU na BASE** | `app/chamados/models.py:3151`; `app/chamados/models.py:3664`; `app/chamados/models.py:3688` |
| A data e o texto antigos são zerados por reabertura real | **CONFERE**, agora também por L-130 | `app/chamados/services/disputa_emissao.py:1609`; `app/docs/LEIS.md:151` |
| Devolução real deixa V legada não nula | **CAIU**: a porta limpa V/autoria e registra DEVOLUÇÃO no mesmo atomic | `app/chamados/services/validacao.py:269`; `app/chamados/services/validacao.py:275` |
| `linhas_da_projecao_sem_linha` conserva por correspondência `(ato, instante)` | **CONFERE**, porém isso não comprova um leitor conservado de fold disponível | `app/chamados/models.py:4081`; `app/chamados/models.py:4094`; `app/chamados/models.py:4102` |
| BUG-PAR acrescenta somente espera DECLARADO-X-GRAVADO ao gate antigo | **CAIU**: também há `indisponivel`, entrega K6 e ausência do dia do fato | `app/chamados/services/materializacao.py:699`; `app/chamados/services/materializacao.py:702`; `app/chamados/services/materializacao.py:705`; `app/chamados/services/materializacao.py:706` |
| F6 e K6 retiram igualmente Validar | **CAIU**: F6 conserva o toque; só os estados próprios K6/sem-dia o retiram pela guarda | `app/chamados/services/validacao.py:59`; `app/chamados/services/validacao.py:209`; `app/chamados/services/materializacao.py:881` |

Tabela da porta já respaldada por testes: [nao verificado]

| Estado | Juiz/porta | Operação | Persistido | Retorno | Prova |
|---|---|---|---|---|---|
| Fora163 entregue | `entrega_que_barra_a_validacao` → `_k6_que_barra` | esconde Validar; recusa antes do carimbo | nada muda | `bloqueada_competencia_entregue` | `app/chamados/tests/test_bug_par_volta.py::test_matriz_linha1_entregue_fora163_recusa_estado_proprio_esconde_validar`, :222 |
| F6 fechada/aprovada, não entregue | testemunha + chokepoint | conserva Validar; tentativa recusada | VALIDACAO+DEVOLUCAO, V volta a nulo, irmã intacta | tela `bloqueada_competencia`; porta `nao_gravou`, `devolvida=True` | `app/chamados/tests/test_bug_par_volta.py::test_matriz_linha2_fechada_nao_entregue_conserva_o_toque_e_o_motivo`, :237 |
| U163 divergente | `_espera_do_aval_163` | mostra espera; esconde Validar | nada muda | `espera_declarado_x_gravado` | `app/chamados/tests/test_bug_par_volta.py::test_matriz_linha3_163_declarado_x_gravado_mostra_a_espera`, :275 |
| Nenhum impedimento | `estado_aplicacao_pergunta`/`validar_pergunta` | conserva Validar; grava par inteiro | uma VALIDACAO nova por metade + par | `aplicavel` → `validada`, `par=pk` | `app/chamados/tests/test_bug_par_volta.py::test_matriz_linha4_nada_barra_aplicavel_validar_grava`, :289 |
| Fora163 sem dia do fato | `_k6_que_barra` | não consulta K6; esconde Validar | nada muda | `bloqueada_sem_dia_do_fato` | `app/chamados/tests/test_bug_par_volta.py::test_matriz_linha5_fora163_sem_dia_do_fato_estado_proprio`, :304 |
| Testemunha falha/ausente | `fio`/filtro | esconde Validar | render não persiste | `indisponivel` | `app/chamados/tests/test_bug_par_volta.py::test_testemunha_falha_ou_ausente_nos_dois_htmls`, :103 |

A implementação das guardas está em `app/chamados/services/materializacao.py:798`, `app/chamados/services/materializacao.py:876`, e a porta retorna o mesmo nome de estado em `app/chamados/services/validacao.py:228`. O `validar_pergunta` ainda recusa ausência de instante por leitura crua, em `app/chamados/services/validacao.py:594`; a migração L-130 dessa finalidade pertence à B2, conforme `app/chamados/juizes.py:1550`.

RED pela regra, sem elevar o brief a lei:

- **R2 por fluxo real não morde hoje**: reabrir deve zerar o par (L-130), e a porta o faz; prova `app/chamados/tests/test_reabrir_nao_destroi.py::test_MORDE_a_pergunta_volta_a_pendente_como_antes`, :54–58; escritor `app/chamados/services/disputa_emissao.py:1609`.
- **R2 sintético morde a leitura crua**, mas sua entrada `(I=None,T='velho')` é legado ilegal, não estado novo permitido: o legado intocado permanece tolerado e a transição nova é recusada (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:177`; `app/chamados/juizes.py:1537`; `app/chamados/juizes.py:1544`).
- **R1 por fluxo real não morde hoje**: devolver limpa V e registra DEVOLUÇÃO (`app/chamados/services/validacao.py:269`; `app/chamados/services/validacao.py:279`).
- **R1 sintético U2 comprova leitura crua, mas não comprova sozinho o retorno exigido pelo fold antigo**: o juiz atual mata por V (`app/chamados/juizes.py:1405`) e D05 conserva fases (`app/docs/BACKLOG.md:2088`). Dizer que o template necessariamente deve sobrepor V por um fold inexistente seria [nao verificado].
- A tabela verdade L-130 está provada em `app/chamados/tests/test_f1nova_b1_juiz_respondida.py::test_RED_tabela_verdade_do_corte`, :28: instante+texto = True; instante sem texto e texto sem instante = False.

Decisões de lei existentes:

- **DECIDIDO PELA LEI L-130 | contestavel**: significado de respondida, par ilegal, reabertura zerando instante e texto (`app/docs/LEIS.md:151`).
- **DECIDIDO PELA LEI E4C-D04-D05/L-127 | contestavel**: atos humanos e conservação das fases públicas; exigibilidade aditiva (`app/docs/BACKLOG.md:2088`).
- **DECIDIDO PELA LEI LU-08 | contestavel**: conferência só vale para o par conferido (`app/docs/CORTES.md:465`).
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 1 | contestavel**: efeito = ato de materialização, valor conservado; não presumir que qualquer carimbo atual prova esse efeito (`app/docs/CORTES.md:487`; distinção das portas em `app/chamados/models.py:3183`).
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 8 | contestavel**: resposta sintética de backfill posterior não revoga validação humana legada (`app/docs/CORTES.md:487`).
- **DECIDIDO PELA LEI CORTE-BUG-PAR-FORA163 | contestavel**: fora163 entregue, os três caminhos não plantam nem desvalidam (`app/docs/BACKLOG.md:2082`).
- **DECIDIDO PELA LEI BUG-PAR-163-X-DECLARADO | contestavel**: U163 divergente espera sem Validar (`app/docs/BACKLOG.md:2083`).

O literal **CORTE-U163-NO-K6** não apareceu nos registros inspecionados; o anexo só o nomeia. Sua redação e abrangência são [nao verificado]. O código atual pula U163 no K6 (`app/chamados/services/materializacao.py:792`); a própria fila ainda menciona “lei U163 em entregue” (`app/docs/BACKLOG.md:2081`). Isso deve ser registrado como dependência de reconferência da F4, sem inventar questão nova própria da F7.

Para sondas:

- U1 pode medir a **forma física** “materializada sem via de identidade” chamando `PerguntaDisputa.vias_de_identidade()` (`app/chamados/models.py:3197`), sem chamar isso automaticamente de erro.
- U2 pode ser contado como **forma física** “última linha DEVOLUCAO + V não nula”, pela ordenação real `(ato_em,pk)`, sem implementar fold; função de domínio própria para esse censo não foi encontrada [nao verificado].
- O estoque de projeção sem linha tem função de domínio real pronta: `linhas_a_criar()` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:44`), que chama `linhas_da_projecao_sem_linha` em :67 e devolve grupos por empresa/competência/ato em :82. Para escopo irrestrito além do horizonte desse comando, chamar diretamente o método de model em lotes (`app/chamados/models.py:4081`).
- Uma sonda que importe a antiga `leitura` para comparar pura/conservada não é executável confirmada nesta BASE [nao verificado]; não incluir implementação substituta.

## 16/11 23:07 · sessao e04e0be3606c · PREPARA-F2-F4-F7 · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 16/11 23:07 · sessao fdc5a77d78e7 · - · - · origem=Ronald (tmux)

- **Premissa F2: confere só quanto à F1 diferente.** O registro diz expressamente “premissa F1=be2b8d733 da matriz CAIU”; não atribui a queda à BUG-PAR (`app/docs/RELATO.md:142`). O código atual projeta os campos vigentes (`app/chamados/juizes.py:1389`) e descarimbar realmente apaga o instante (`app/chamados/models.py:3391`). “F2 caiu por F1 **e** BUG-PAR diferentes” amplia a evidência; a parte BUG-PAR é [nao verificado].

- **ESPINHA-NAO-FICA item 1/PREPARA B2 existem documentalmente.** Item 1 registrado como abertura condicional, inicialmente sem brief (`app/docs/PROMPTS.md:2427`); PREPARA rodou como leitura (`app/docs/RELATO.md:39`); depois fechou censo, lei 0, tabela, mas encontrou 8 arquivos cruzados e condicionou a abertura ao pouso BUG-PAR (`app/docs/RELATO.md:37`; `app/docs/BACKLOG.md:821`). Isso prova o precedente de preparo; a cópia B2 em voo e sua lista atual permanecem [nao verificado].

- **Complementos F2 1/2/3 existem como registros, não como anexos legíveis aqui.** C1 substitui respostas 2 e 9 (`app/docs/PROMPTS.md:2433`); C2 especifica medida por último ato e efeito como carimbo vigente, com histórico remetido à E4d (`app/docs/PROMPTS.md:2438`); C3 responde ao 9a devolvido (`app/docs/PROMPTS.md:2442`). RELATO confirma revisão C3 “1,2,3 CONFERE”, registro e L1 resolvido, incluindo selo dos sete leitores (`app/docs/RELATO.md:8`). Conteúdo integral dos complementos/brief e aplicação no arquivo não disponível: [nao verificado].

- **Quatro condições específicas de abertura F2: [nao verificado].** A enumeração localizada é a do PREPARA BUG-PAR: lei 0, revisão 3x NÃO PROCEDE, cruzamento 0, vaga livre (`app/docs/RELATO.md:136`). Não autoriza transplantá-la implicitamente à F2. Para B2, o registro expõe censo, tabela, cruzamento e lei 0, mas não apresenta um literal chamado “as quatro condições da F2” (`app/docs/RELATO.md:37`; `app/docs/RELATO.md:39`). PROCEDE no pedido de tornar essa remissão explícita.

- **Lei aberta não impede preparar; impede construir a fatia pertinente.** O precedente F2 exige as nove respostas antes da construção (`app/docs/RELATO.md:36`); C2/C3 registram fechamento dos pontos que tinham sido devolvidos (`app/docs/PROMPTS.md:2438`; `app/docs/RELATO.md:8`). “Nenhuma fatia abre com pergunta aberta” é compatível com preparar agora e consolidar perguntas depois; não prova lei 0 das três entregas futuras [nao verificado].

- **RED em texto é possível; “morde” executado não.** Há precedente explícito de leitura com “RED só em TEXTO” (`app/docs/PROMPTS.md:2321`). O item 9 antigo é outra atividade: construir testes em cópia e declarar mutante/resultado (`app/docs/CORTES.md:487`). No PREPARA atual, “morde” pode significar avaliação por leitura; execução permanece [nao verificado]. Convém rotular “caso/resultado esperado; mordida por leitura”, sem declarar RED executado.

- **Envelope:** Codex não roda a sonda de prod diretamente; o Code executa `sonda_leitura.sh`, raspa a saída e a anexa antes de chamar Codex (`bin/codex_pergunta.sh:15`; `bin/codex_pergunta.sh:17`; `bin/codex_pergunta.sh:20`). “Tudo ao Codex (… sonda)” precisa dessa explicitação; os números novos pedidos não vieram no raspado [nao verificado].

## 16/11 23:06 · sessao c745ac5b3b03 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 16/11 23:06 · sessao 8d3f205a31df · - · - · origem=Ronald (tmux)

Consolidação da pergunta 2 e premissa B2:

- **Existe ESPINHA-NAO-FICA item 1 e PREPARA B2**, com registro em `app/docs/PROMPTS.md:2427`, despacho em `app/docs/RELATO.md:38` e resultado em `app/docs/RELATO.md:37`: censo fechado, lei 0, tabela pronta, mas cruzamento de **8 arquivos** com BUG-PAR; B2 abre depois do pouso da BUG-PAR c4. Os logs originais não estão na cópia; seu conteúdo além desses registros é **[nao verificado]**.
- **“B2 já abriu/em voo” e worktree `wt-e4c-b2-215705`: [nao verificado]**. Os registros disponíveis ainda mostram abertura condicionada à BUG-PAR (`app/docs/BACKLOG.md:821`, `app/docs/RELATO.md:37`); a BUG-PAR consta pousada às 21:41 (`app/docs/RELATO.md:5`). Isso permite a abertura posterior, mas não a comprova.
- **CLASSE-D continua sendo predecessor da construção F2**: `app/docs/PROMPTS.md:2331`, `app/docs/BACKLOG.md:1987`, `app/docs/RELATO.md:346`. O encaixe técnico específico de R1 sozinho entre F1 e F2 aparece em `app/docs/RELATO.md:394`. Preparação por leitura não elimina essa precedência; dizer que F2 abre apenas após B2 e “4 condições” deixa esse predecessor sem explicitação.
- **ORDEM VIVA** permanece identificada como ORDEM-VIVA-1511-NOITE (`app/docs/BACKLOG.md:8`, `app/docs/BACKLOG.md:1872`, `app/docs/PROMPTS.md:2364`). A proibição literal de nova obra de instrumento/lateral por iniciativa aparece no comentário da ordem anterior (`app/docs/BACKLOG.md:9`), e sua aplicação à ORDEM-VIVA-1511-NOITE aparece em `app/docs/RELATO.md:258`. Este aval é uma autorização expressa de preparação de fatias existentes; não há colisão demonstrada com essa proibição enquanto ficar nessa preparação.
- **PISTA**: regra registrada em `app/docs/CORTES.md:489`; complemento mais recente em `app/docs/RELATO.md:75`: suites de não pouso de lateral/tela/instrumento vão à pista; VM para espinha, pouso e push. Preparação sem suite/trava é compatível com esse recorte.
- **CORTE-U163-NO-K6 e devolução em três itens: [nao verificado]**. Não aparecem na cópia consultada. O que aparece é a sonda anterior: **1** competência do fato entregue com FechamentoMensal aberto (`app/docs/RELATO.md:19`) e a pergunta a subir depois da c4; item pendente do opus em `app/docs/RELATO.md:5`. Não cabe tratá-lo como complemento já decidido.
- **As “4 condições de sempre” da F2: [nao verificado]**. Existe um conjunto explícito para **BUG-PAR**, em `app/docs/RELATO.md:136`: lei 0; revisão sem procedência; cruzamento 0; vaga livre. Não prova que seja o conjunto F2. A preparação B2 enumera censo/tabela/arquivos em `app/docs/RELATO.md:39`, sem fornecer quatro condições literais completas.
- **Lei aberta não deve impedir a preparação**, mas impede decidir sua construção: a regra manda pergunta de lei ao topo com números e seguir obra independente (`CLAUDE.md:269`); desenho para a própria obra (`CLAUDE.md:274`). Esperar necessariamente as três entregas para publicar lei residual pode atrasar esse registro, conforme o literal `CLAUDE.md:269`.
- **Exportada/!**: tocar gravado de competência exportada, dado de escala/vínculo ou regra fora do pedido exige `!` (`CLAUDE.md:265`). Preparação por leitura não autoriza esses atos; garantir a sonda concreta como somente leitura depende de verificar seu envelope **[nao verificado]**.

## 16/11 23:01 · sessao 64ed77f64ee2 · REVISAO-AVAL-2300 · verificador · origem=Code

**CORTE-U163-NO-K6: PROCEDE nos itens lista/contador e pré-condições do RED.** Retirar a exclusão no K6 não inclui automaticamente U163 na lista, e uma U163 com irmã fora163 entregue já pode bloquear hoje. (`app/chamados/services/materializacao.py:795`; `app/chamados/management/commands/bug_par_103.py:72`; `app/chamados/tests/test_bug_par_volta.py:145`)

**PUSH-DOS-TRES-MARCOS: PROCEDE no item 1 quanto à garantia de ordem; item 2 depende de apuração.** A trava oferece exclusão mútua, sem prioridade; a causa da demora e a ausência de tentativas posteriores são [nao verificado]. (`bin/trava_teste.sh:25`; `bin/trava_teste.sh:35`)

**CORTE-U163-NO-K6**

1. **Premissas: conferem quanto ao pulo e às autoridades.**

   `k6_consulta_a_pergunta` retorna verdadeiro somente para pergunta existente, metade de intervalo e **fora U163**; `_k6_que_barra` pula quando recebe falso. (`app/chamados/services/materializacao.py:792`; `app/chamados/services/materializacao.py:806`)

   O pulo está documentado na docstring do predicado (“as 163 nao passam pelo K6”), na docstring da guarda (“o ramo que pula as 163 nao muda”), na composição K6/F6, na docstring do escritor e no comentário de sua guarda inicial. Todos precisam acompanhar a mudança. (`app/chamados/services/materializacao.py:793`; `app/chamados/services/materializacao.py:799`; `app/chamados/services/materializacao.py:819`; `app/chamados/services/materializacao.py:1818`; `app/chamados/services/materializacao.py:1829`)

   Há **um predicado de entregue nessa composição**: `competencia_do_fato` deriva a competência pelo dia do fato e chama `competencia_entregue`; esse juiz verifica exportação vigente não invalidada e está registrado como K6. F6 responde outra pergunta. (`app/chamados/services/materializacao.py:776`; `app/chamados/services/materializacao.py:786`; `app/folha/export.py:556`; `app/folha/export.py:588`; `app/core/juizes.py:343`)

   Hoje, F6 também alcança U163: consulta a competência do **instante local do plantio**, barrando FM `aprovado` ou `exportado`; FM aberto passa por essa guarda. O materializador chama a porta com `fato_de_chao=False`, que aplica F6. (`app/ponto/janelas.py:75`; `app/ponto/janelas.py:84`; `app/ponto/registro_batida.py:154`; `app/chamados/services/materializacao.py:1455`)

2. **Colisões: extensão autorizada, mas exige atualizar a exceção e a matriz.**

   O corte anterior exige K6 nos três caminhos **fora163**; o COMPLEMENTO-4 registra explicitamente U163 entregue sob F6, com aberto seguindo. A lacuna da matriz original — L1 fora163 e L2 não entregue — já consta no RELATO. O novo corte pode suceder essa exceção, mas deve ampliar L1/L5 e registrar essa sucessão. (`app/docs/BACKLOG.md:2082`; `app/docs/RELATO.md:22`; `app/docs/RELATO.md:30`)

   A dependência da B2 está registrada: oito arquivos compartilhados, abertura por cima da BUG-PAR c4. Construir U163 depois da B2 é compatível com essa dependência; B2 efetivamente “em voo” no horário do anexo é [nao verificado]. (`app/docs/RELATO.md:37`; `app/docs/BACKLOG.md:821`)

   CORTES mantém uma condição adicional para a futura F4: pousar atrás da F1/F2 ou deixar a costura J1 sob contador esperado zero. O encaixe precisa respeitá-la. (`app/docs/CORTES.md:487`) Os avais anteriores têm remissões em PROMPTS; seu registro literal em CORTES.md fica [nao verificado]. (`app/docs/PROMPTS.md:2382`; `app/docs/PROMPTS.md:2422`)

3. **Literal: há três pendências.**

   - **PROCEDE — “vai na lista da Pauta DP”.** A lista existe como **saída textual do comando** `bug_par_103`: inclui U103 entregue/sem dia e, em outro laço, metades respondidas fora U163; depois imprime `PAUTA DP`. Não há nessa construção criação de uma `Pauta`. (`app/chamados/management/commands/bug_par_103.py:53`; `app/chamados/management/commands/bug_par_103.py:63`; `app/chamados/management/commands/bug_par_103.py:70`; `app/chamados/management/commands/bug_par_103.py:85`)

     A guarda apenas devolve estado e texto “preservada para Pauta DP”. Retirar o `not no_universo_163` não altera o laço que exclui U163; portanto não garante incluir U60 nessa lista. O ajuste do comando precisa entrar no escopo futuro. (`app/chamados/services/materializacao.py:812`; `app/chamados/management/commands/bug_par_103.py:72`)

   - **PROCEDE — classificação do contador.** Entregue/sem dia têm baldes separados no ramo das perguntas carimbadas elegíveis à devolução U103. Uma U163 não materializada que passe a receber L1/L5 cai hoje em `outras` e continua somando `pendentes_aval`. Registrar a matriz F4 precisa explicitar o destino dessas perguntas e a conservação das contagens. (`app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:347`; `app/chamados/services/materializacao.py:351`)

   - **PROCEDE — RED insuficientemente condicionado.** “U163 + entregue + FM aberto” não basta para garantir “hoje valida e grava”: quando pula a pergunta, a composição procura a irmã; uma irmã fora163 entregue já bloqueia. Existe teste desse cenário. (`app/chamados/services/validacao.py:219`; `app/chamados/services/validacao.py:224`; `app/chamados/tests/test_bug_par_volta.py:145`)

     O caminho que pode gravar é `validar_pergunta` → carimbo → `_validar_o_par`/`_materializar_uma` → `registrar_batida`. Exige resposta, ausência de validação anterior, passagem pelo declarado × gravado, confirmação de implausível quando necessária e demais guardas do plantio. (`app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:609`; `app/chamados/services/validacao.py:615`; `app/chamados/services/validacao.py:652`; `app/chamados/services/validacao.py:681`; `app/chamados/services/materializacao.py:1455`)

     O número **1** está declarado no registro da sonda; ele não demonstra, sozinho, que essa pergunta atravessa todas essas condições e cria batida. Essa conclusão e a nova medição permanecem [nao verificado]. (`app/docs/RELATO.md:19`)

4. **Exportada, dinheiro e `!`: não há autorização adicional implícita.**

   O bloqueio proposto atua antes do carimbo na validação e antes de recusa/pendência no escritor, compatível com preservar o gravado. Seu efeito declarado é impedir plantio futuro em competência entregue. (`app/chamados/services/validacao.py:652`; `app/chamados/services/materializacao.py:1829`)

   Há recalculo por evento no caminho atual de validação, já apontado como herdado no RELATO. Isso não autoriza apply, retificação ou alteração de FM exportado; essa escrita continua sob `!`. Impacto monetário efetivo é [nao verificado]. (`app/chamados/services/validacao.py:664`; `app/docs/RELATO.md:30`; `CLAUDE.md:411`)

**PUSH-DOS-TRES-MARCOS** [nao verificado]

1. **Premissas: a regra existe; push disputa a trava.** [nao verificado]

   “PUSH: um por MARCO” está literal. `push.sh` chama `git push`, preserva seu retorno e chama `pos_push.sh` após sucesso; o pós-push atualiza e commita o derivado TICKETS. (`CLAUDE.md:336`; `bin/push.sh:6`; `bin/pos_push.sh:18`; `bin/pos_push.sh:29`)

   O pre-push adquire `/tmp/regua.lock`, com espera ou herança; cada `_teste_irmao` disputa separadamente a trava do banco de teste. O arsenal pode ser dispensado por certificação da mesma árvore; o control-plane permanece chamado. (`bin/pre-push.sh:80`; `bin/pre-push.sh:86`; `bin/pre-push.sh:142`; `bin/pre-push.sh:66`; `bin/pre-push.sh:198`)

   Os três marcos têm registros documentais, mas SHA remoto, distância de 34 commits, hook instalado e ausência efetiva de push são [nao verificado]. (`app/docs/RELATO.md:5`; `app/docs/RELATO.md:6`; `app/docs/RELATO.md:13`; `app/docs/RELATO.md:15`)

2. **Colisões: a finalidade confere; a prioridade precisa ser despachada.**

   CORTES reserva a VM para espinha, pouso e push; lateral usa a pista e só volta à VM, atrás da espinha, se a pista não responder. O novo aval não deve ser interpretado como trazer lateral para disputar normalmente a VM. (`app/docs/CORTES.md:489`)

   A continuidade da B2 é compatível com a dependência registrada; sua execução e eventual interferência operacional são [nao verificado]. (`app/docs/RELATO.md:37`) O COMPLEMENTO-2 fornecido exclui push de seu alcance; conferência do texto integral em arquivo local é [nao verificado].

3. **Literal: item 1 precisa de mecanismo operacional; item 2 não tem causa provada.**

   - **PROCEDE, item 1:** `flock -w` oferece exclusão mútua, sem ticket, posição ou prioridade. Apenas lançar o push em espera não garante “logo depois da atual, antes da lateral”. Arsenal e control-plane ainda fazem aquisições separadas, permitindo intercalação. A ordem precisa ser assegurada pelo despacho dos chamadores. (`bin/trava_teste.sh:25`; `bin/trava_teste.sh:35`; `bin/pre-push.sh:142`; `bin/pre-push.sh:186`; `bin/pre-push.sh:198`)
   - **Item 2:** há registro de push em pre-push às 17:1x. A causa da demora e a ausência de tentativas posteriores são [nao verificado]; a linha pedida não pode inventá-las. (`app/docs/RELATO.md:44`)
   - **Item 3:** pedir push no fechamento acompanha a regra vigente; os scripts examinados não implementam esse agendamento automático. (`CLAUDE.md:332`; `CLAUDE.md:336`; `bin/push.sh:6`; `bin/pre-push.sh:142`)
   - **Item 4:** as duas partials existem. L5 mostra o detalhe “sem dia do fato” e oculta Validar; L2 conserva Validar e preserva `COMPETENCIA_APROVADA` na recusa, devolução e nota. O roteiro pode cobrar esses resultados; disponibilidade de casos reais e execução do smoke são [nao verificado]. (`app/chamados/tests/test_bug_par_volta.py:200`; `app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/modal_fio.html:274`; `app/chamados/tests/test_bug_par_volta.py:247`; `app/chamados/tests/test_bug_par_volta.py:258`; `app/chamados/tests/test_bug_par_volta.py:267`; `app/chamados/tests/test_bug_par_volta.py:313`)

4. **Exportada, dinheiro e `!`: nenhum toque direto aparece nesses caminhos.**

   O wrapper executa push e pós-push documental; os testes usam árvore do commit e configurações de teste. Não há chamada explícita de deploy/apply nesses trechos. Efeitos efetivos do ambiente e do hook instalado são [nao verificado]. (`bin/push.sh:6`; `bin/pos_push.sh:19`; `bin/pos_push.sh:29`; `bin/pre-push.sh:144`; `bin/pre-push.sh:186`; `bin/pre-push.sh:198`)

**Quem escreve e quem lê**

| Valor | Quem produz/escreve | Leitores diretos de produção |
|---|---|---|
| `no_universo_163(p)` | É função, sem campo persistido: produz booleano consultando a PK no universo congelado. O carregador lê o JSON e forma conjuntos imutáveis. (`app/chamados/services/universo_bug_par.py:7`; `app/chamados/services/universo_bug_par.py:12`; `app/chamados/services/universo_bug_par.py:25`; `app/chamados/services/universo_bug_par.py:33`) | `_espera_do_aval_163`, para aplicar declarado × gravado; `k6_consulta_a_pergunta`, para excluir U163. (`app/chamados/services/materializacao.py:742`; `app/chamados/services/materializacao.py:795`) |
| Retorno de `k6_consulta_a_pergunta(p)` | A própria função produz **booleano de consulta**, não veredito de entrega. (`app/chamados/services/materializacao.py:792`) | `_k6_que_barra`, decidindo pular a pergunta; `entrega_que_barra_a_validacao`, decidindo procurar a irmã quando a pergunta é pulada. (`app/chamados/services/materializacao.py:806`; `app/chamados/services/validacao.py:224`) |

Os leitores diretos de teste de pertencimento estão em `test_universo_bug_par`, `test_bug_par_volta`; há substituições do predicado nos testes sem banco e mutantes de pertencimento. A função K6 também é carregada no ambiente do teste sem banco. Isso é evidência de código, sem comprovação de execução nesta revisão. (`app/chamados/tests/test_universo_bug_par.py:65`; `app/chamados/tests/test_bug_par_volta.py:33`; `app/chamados/tests/test_bug_par_volta.py:100`; `app/chamados/tests/test_bug_par_sem_banco.py:88`; `app/chamados/tests/test_bug_par_sem_banco.py:134`; `app/chamados/tests/bug_par_mutantes.py:55`; `app/chamados/tests/test_bug_par_sem_banco.py:140`)

## 16/11 22:58 · sessao 3a1b9f7f1505 · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **Premissas do pulo conferem.** `k6_consulta_a_pergunta` devolve `False` para U163 (`app/chamados/services/materializacao.py:793-795`); `_k6_que_barra` obedece esse retorno (`:805-808`). Documentação a atualizar no ato: docstrings `:793-794`, `:799-803`, `:819`; comentário sobre procurar a irmã somente quando a pergunta é pulada em `app/chamados/services/validacao.py:220-224`.

2. **Um predicado de entregue nessa composição.** `competencia_do_fato` compõe competência pela data do fato com `competencia_entregue` (`app/chamados/services/materializacao.py:776-789`); registro do juiz em `app/core/juizes.py:343`; implementação do predicado único em `app/folha/export.py:556-594`. F6 consulta outra pergunta: `FechamentoMensal` aprovado/exportado para a competência dos instantes (`app/ponto/janelas.py:75-89`, `app/chamados/services/materializacao.py:818-835`).

3. **Mudança autorizada sucede a exceção vigente; não é colisão impeditiva por si.** O corte anterior registrado no BACKLOG exige K6 nos três caminhos **fora163** (`app/docs/BACKLOG.md:2082`). COMPLEMENTO-4 registra que U163 entregue atualmente cai em F6: aprovado/exportado bloqueia; aberto segue (`app/docs/RELATO.md:22`). A matriz original deixou lacuna para U163 entregue, registrada em `app/docs/RELATO.md:30`. É necessário ampliar L1/L5 e registrar a sucessão dessa exceção; atualizar só F4 deixa o texto anterior coexistindo (`app/docs/RELATO.md:22,30`; matriz anexada pelo usuário [nao verificado quanto a arquivo local]).

4. **PROCEDE — condição literal “vai na lista da Pauta DP”.** A lista existe como saída do comando: U103 entra pelo laço de `app/chamados/management/commands/bug_par_103.py:53-63`; fora163 entra pelo laço `:71-77`; a saída é impressa em `:85`. Esse segundo laço exclui U163 inteira (`:72`), portanto retirar a exclusão da guarda K6 não põe U60 nessa lista. O contador também coloca bloqueadas por entregue/sem dia em `outras` e conserva a soma em `pendentes_aval` (`app/chamados/services/materializacao.py:339-356`); só o ramo das já materializadas separa pauta/sem dia (`:326-336`). Registrar uma matriz não implementa esse encaminhamento [nao verificado quanto à implementação futura].

5. **PROCEDE — RED insuficientemente condicionado.** “Uma das 163 + entregue + FM aberto” não garante que hoje valida e grava: ao pular a pergunta U163, a porta procura a irmã e consulta seu K6 (`app/chamados/services/validacao.py:220-225`); há teste de U163 com irmã fora163 entregue já bloqueando hoje (`app/chamados/tests/test_bug_par_volta.py:145-160`). Também há portão declarado × gravado (`app/chamados/services/materializacao.py:922-937`). O RED deve explicitar irmã igualmente pulada/ausente e declaração compatível, além das demais condições de plantio (`app/chamados/services/validacao.py:220-225`; `app/chamados/services/materializacao.py:895-937`). O caso real da sonda efetivamente gravar permanece **[nao verificado]**.

6. **B2.** O registro disponível diz que cruza oito arquivos com BUG-PAR e abre por cima da c4 (`app/docs/RELATO.md:37`; `app/docs/BACKLOG.md:821`). “B2 em voo” na hora do aval não está demonstrado pelo dado anexado **[nao verificado]**. Respeitar sua continuidade e encaixar U163 depois dela é compatível com a dependência registrada (`app/docs/RELATO.md:37`).

7. **CORTES.md.** Não localizei registro literal de `CORTE-BUG-PAR-FORA163`, `COMPLEMENTO-2 ESPINHA-ABRE-A-VOLTA-BUG-PAR` ou `CORTE-U163-NO-K6` no arquivo **[nao verificado quanto a registro ausente]**. Há referências aos dois primeiros em `app/docs/PROMPTS.md:2382,2422`; o corte fora163 é reproduzido em `app/docs/BACKLOG.md:2082`.

**Veredito recomendado U163: PROCEDE nos itens literais de encaminhamento à lista/contador e pré-condições do RED**, pelas referências dos itens 4–5; a extensão do K6 em si confere com a estrutura existente (`app/chamados/services/materializacao.py:792-835`).

## 16/11 22:58 · sessao d0594a09bc3a · - · - · origem=Ronald (tmux)

1. **Pulo atual confirmado.** `k6_consulta_a_pergunta` retorna bool e exclui `no_universo_163`; a docstring diz literalmente “as 163 nao passam pelo K6” (`app/chamados/services/materializacao.py:792-795`). `_k6_que_barra` documenta “o ramo que pula as 163 nao muda” e consome aquele bool (`:798-807`). Outros textos a atualizar no mesmo ato: composição “metades fora163” (`:818-824`), docstring de `_materializar_uma` (`:1813-1819`) e comentário da guarda inicial (`:1829-1833`).

2. **Um juiz de entregue neste caminho.** `competencia_do_fato` compõe janela do dia do fato com `folha.export.competencia_entregue`, devolvendo `None` sem dia ou `(mes, ano, entregue)` (`app/chamados/services/materializacao.py:776-789`). O juiz verifica Exportação vigente não invalidada; não usa `FechamentoMensal` (`app/folha/export.py:556-594`). A F6 é outra pergunta: por instante local, busca FM e barra somente status `aprovado`/`exportado` (`app/ponto/janelas.py:75-89`); `registrar_batida` a aplica para `fato_de_chao=False` (`app/ponto/registro_batida.py:147-161`), exatamente a flag passada pelo materializador (`app/chamados/services/materializacao.py:1454-1462`). FM aberto portanto não basta para barrar U163 entregue por esse caminho (`app/ponto/janelas.py:86-89`).

3. **RED possível, porém existência real não demonstrada.** Porta exige resposta, ausência de validação anterior (`app/chamados/services/validacao.py:594-599`), passa pelo portão declarado x gravado e pela confirmação de implausível (`:605-618`), consulta K6 de pergunta+irmã e só depois carimba (`:638-655`); segue por `_validar_o_par` ou `_materializar_uma` (`:681-687`), e o escritor chama `registrar_batida` (`app/chamados/services/materializacao.py:1454-1462`). Uma U163 com irmã fora163 entregue **já é bloqueada hoje**: a composição consulta também a irmã quando pula a pergunta (`app/chamados/services/validacao.py:219-225`), cenário explicitamente coberto no código do teste (`app/chamados/tests/test_bug_par_volta.py:145-160`; execução [nao verificado]). Portanto o RED precisa uma U163 realmente gravável, sem irmã fora163 que já a barre e sem L3/paridade/data/reuso impeditivos (`app/chamados/services/materializacao.py:889-940`). O “1” da sonda e qual pergunta satisfaz todas essas condições: [nao verificado].

4. **PROCEDE — literal “a pergunta vai na lista da Pauta DP” / escopo MUDA incompleto.** A lista existe como saída textual do comando `bug_par_103`, não como escrita de `Pauta`: constrói `pauta=[]`, inclui U103 entregue/sem dia, acrescenta metades respondidas hora **fora U163** e imprime stdout (`app/chamados/management/commands/bug_par_103.py:54-85`). `_k6_que_barra` apenas retorna estado e texto “preservada para Pauta DP” (`app/chamados/services/materializacao.py:808-815`). Retirar a exclusão no bool não faz o comando incluir U60/U163 não materializadas, pois o filtro atual continua excluindo U163 (`app/chamados/management/commands/bug_par_103.py:70-77`). Deve declarar ajuste desse leitor ou deixar explícito que o encaminhamento ainda não estará implementado.

5. **PROCEDE — F4 exige completar classificação.** Contador só separa entregues/sem dia no ramo das U103 carimbadas (`app/chamados/services/materializacao.py:325-337`). Pergunta U163 não materializada que ganhar L1/L5 cairá hoje em `outras` (`:339-347`), continuará em `universo` e `pendentes_aval` (`:351-356`); não irá automaticamente aos baldes `bug_par_103_pauta_dp`/`bug_par_103_sem_dia`. Isso precisa entrar na matriz F4 que o aval manda registrar (`app/chamados/services/materializacao.py:311-316`).

6. **Escopo e preservação.** Bloqueio K6 precede qualquer carimbo na validação (`app/chamados/services/validacao.py:652-655`) e precede recusa/pendência/materialização no escritor (`app/chamados/services/materializacao.py:1829-1833`), compatível com “nada gravado”. O delta muda a possibilidade futura de plantar batidas em competência entregue, cuja criação passa pelo escritor indicado (`app/chamados/services/materializacao.py:1454-1462`); impacto monetário efetivo e dados exportados de produção [nao verificado]. As condições de L3 continuam antes da escrita (`app/chamados/services/validacao.py:605-613`), e L1/L5 permanecem estados próprios consumidos pela tela (`app/templates/chamados/partials/_pergunta_card.html:44-47`, `app/templates/chamados/partials/modal_fio.html:274`).

**Veredito do recorte: PROCEDE nos itens lista Pauta DP e classificação F4; RED e contagem precisam ser explicitamente tratados como cenário a demonstrar, sem afirmar prova de produção [nao verificado].**

## 16/11 22:58 · sessao e0112d968bb3 · - · - · origem=Ronald (tmux)

Veredito PUSH: **PROCEDE, item 1** — a posição na fila não é garantida pelos scripts; os demais itens conferem como instruções de conduta/documentação, com limites abaixo. (`bin/trava_teste.sh:35-42`; `CLAUDE.md:332-336`)

1. **Premissas.** “PUSH: um por MARCO” existe literalmente. (`CLAUDE.md:336`) `push.sh` chama `git push`, preserva seu rc e, se passar, chama `pos_push.sh`. (`bin/push.sh:6-9`) O pós-push atualiza e commita somente o derivado do TICKETS. (`bin/pos_push.sh:18-30`) O pre-push possui trava da régua, espera até 30 minutos ou herda a trava do chamador; cada execução de testes disputa também a trava do banco de teste. (`bin/pre-push.sh:80-89`, `bin/pre-push.sh:142-145`) O arsenal pode ser dispensado pelo carimbo da mesma árvore; o control-plane continua chamado. (`bin/pre-push.sh:65-70`, `bin/pre-push.sh:159`, `bin/pre-push.sh:190-199`) Existe bypass `SKIP_TESTS=1`; o aval não o autoriza. (`bin/pre-push.sh:5`; [nao verificado] quanto ao ambiente efetivo)

2. **Colisão.** A orientação de push por marco está alinhada com CLAUDE. (`CLAUDE.md:332-336`) A B2 tinha dependência expressa de pousar BUG-PAR antes de abrir, por oito arquivos em comum; isso é registro da cópia, não prova de seu estado atual em execução. (`app/docs/RELATO.md:37`) O COMPLEMENTO-2 separa push do próprio alcance no dado fornecido; interferência operacional real na B2/suite em curso fica [nao verificado]. Os cortes de dinheiro preservam `!` para alterar gravado exportado. (`CLAUDE.md:411-417`)

3. **Literal que não fecha.**
   - **Item 1, PROCEDE:** a trava implementa somente `flock -w`, execução e liberação; não tem posição, ticket, prioridade nem reserva para “push depois da atual e antes da lateral”. (`bin/trava_teste.sh:21-42`) Além disso, arsenal e control-plane são duas aquisições separadas da trava: outra execução pode intercalar. (`bin/pre-push.sh:142-145`, `bin/pre-push.sh:186-199`) Precisa declarar o despacho dos chamadores e impedir que a lateral seja lançada antes do push; atribuir essa ordem à trava seria falso. (`bin/trava_teste.sh:25-42`)
   - **Item 2:** não há prova da causa da demora nem da ausência de tentativa posterior; [nao verificado]. Há registro de push em pre-push às 17:1x. (`app/docs/RELATO.md:44`) Logo a linha deve relatar uma causa apurada, sem inventar histórico; [nao verificado].
   - **Item 3:** pedir o push no fechamento está alinhado à regra existente, mas não há agendador automático nos dois scripts. (`CLAUDE.md:332-336`; `bin/push.sh:1-9`; `bin/pre-push.sh:80-89`)
   - **Item 4:** as duas partials existem, identificadas também no teste de matriz. (`app/chamados/tests/test_bug_par_volta.py:200-202`) L2 conserva Validar e cobra motivo/nota `COMPETENCIA_APROVADA`; L5 mostra “sem dia do fato” e oculta Validar. (`app/chamados/tests/test_bug_par_volta.py:247-267`, `app/chamados/tests/test_bug_par_volta.py:311-318`) O RELATO apenas remete o smoke ao responsável, sem roteiro ali. (`app/docs/RELATO.md:5`) Disponibilidade de casos reais para executar esses cliques: [nao verificado].
   - SHA remoto `71e9576bf`, 34 commits e ausência de push dos três marcos não puderam ser confirmados por git: cópia sem repositório utilizável; [nao verificado]. Há registros dos pousos e do uso de `343565cf6` como base, que não demonstram estado remoto. (`app/docs/RELATO.md:6`, `app/docs/RELATO.md:13-16`)

4. **Exportada, dinheiro, `!`.** O wrapper apresentado faz push e atualização documental do TICKETS; o pre-push executa testes com configurações de teste e árvore do commit. (`bin/push.sh:6-8`; `bin/pos_push.sh:18-30`; `bin/pre-push.sh:142-145`, `bin/pre-push.sh:186-199`) Nesses caminhos não há chamada explícita de apply/deploy/retificação de produção. (`bin/push.sh:1-9`; `bin/pos_push.sh:18-30`; `bin/pre-push.sh:186-199`) A escrita real em produção, efeitos das credenciais e estado do hook instalado são [nao verificado]; o wrapper esperado do hook está documentado no instalador. (`bin/hooks.sh:60-65`)

## 16/11 22:30 · sessao c014cdc3a87b · E4C-F1-B2 · verificador · origem=Code

**Não aprovaria B2 como “todo leitor lê o juiz” nem como garantia de ausência de efeitos nas competências entregues.** Restam leitores operacionais próprios e existem caminhos de escrita alcançados pela mudança da retenção. `app/chamados/services/verbos.py:335`, `app/chamados/canal.py:65`, `app/chamados/models.py:1925`, `app/ponto/management/commands/supra_juiz.py:286`.

A cópia não contém `.git`: identidade do HEAD e comparação integral `520d7b557..HEAD` **[nao verificado]**. Testes, PostgreSQL, estado efetivo das duas perguntas e efeitos nas 141 entregues **[nao verificado]**.

**ACHADOS**

1. **Grave — a lista fechada do saneamento não isola os efeitos dos leitores nas entregues.** A retenção deixa de segurar ambos os pares ilegais; isso libera o CSF a tentar retratação. O gate posterior de pergunta viva exclui expressamente competência entregue, portanto não substitui essa proteção. Há caminho para resolver chamado, rejeitar justificativa e executar o resolver de perguntas. `app/chamados/models.py:1923`, `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:588`, `app/chamados/models.py:364`, `app/chamados/reconciliador.py:525`, `app/chamados/reconciliador.py:531`, `app/chamados/reconciliador.py:538`.  
   Isso comprova **alcance possível**, não ocorrência nas 141 perguntas: faltam suas datas, âncoras, lâmpadas e trilhas **[nao verificado]**.

2. **Médio — a migração dos leitores ficou incompleta.** O placar continua chamando de “respondidos” os registros selecionados somente por instante; o canal partido continua definindo “sem resposta” por texto exatamente `''`; a ressuscitação mantém seleção por texto NULL/`''`; cobrança/cartão ainda recebem uma recomposição local de `not J`. São resíduos preexistentes que impedem certificar a conclusão “todo leitor”. `app/chamados/services/verbos.py:325`, `app/chamados/services/verbos.py:335`, `app/chamados/canal.py:53`, `app/chamados/canal.py:65`, `app/chamados/models.py:1494`, `app/chamados/models.py:1533`, `app/chamados/services/fio.py:187`.

3. **Médio — a lista fechada não é uma restrição da porta.** `ESTOQUE_L130_ALVOS` existe, mas `impedimento_estoque_l130` e a porta aceitam qualquer pk elegível. Somente o comando compara o conjunto com a lista, antes das travas; depois aplica uma pergunta por transação. Assim, a igualdade é garantida na fotografia inicial, não como condição atômica do lote inteiro. Pode haver aplicação parcial por perda de elegibilidade ou falha de trilha da outra pergunta. `app/chamados/services/validacao.py:378`, `app/chamados/services/validacao.py:381`, `app/chamados/services/validacao.py:432`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:53`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:70`.

4. **Médio — novo caminho de julgamento sem relógio recebido da borda.** O predicado novo chama `pergunta_viva(perg)` sem `agora`/`hoje`; esse caminho pode ler `date.today()` e, adiante, `timezone.now()`. A leitura do comando acontece depois da seleção e serve apenas ao arquivo de reversão. Contraria a consequência documentada de Clock Injection. `app/chamados/services/validacao.py:398`, `app/chamados/juizes.py:596`, `app/chamados/juizes.py:1475`, `app/escala/servico_jornada.py:387`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:61`, `app/docs/PLANO-PADROES.md:101`.

5. **Médio — o teste da property não prova delegação.** Comparar resultados com o juiz permite passar uma property que copie integralmente sua expressão. Os casos distinguem as regras antigas B/C/D, mas não demonstram a mordida de um mutante que substitua a delegação por um segundo juiz equivalente. `app/chamados/tests/test_f1nova_b2_leitores.py:61`, `app/chamados/tests/test_f1nova_b2_leitores.py:75`, `app/chamados/tests/test_f1nova_b2_leitores.py:142`. Mordidas executadas **[nao verificado]**.

6. **Menor — documentação permanece na situação anterior.** O lookup ainda diz que B/C/D migrarão no B2, e a LEIS declara B2 “por construir”; o teste O168 conserva docstring dizendo que a retenção usa I OU T e “nunca solta”, embora seu corpo agora teste o contrário para os pares ilegais. `app/docs/JUIZES.md:105`, `app/docs/LEIS.md:151`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:278`, `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:290`.

**Conferido e conforme, com as ressalvas acima**

**1. Escritores, leitores e significado dos campos**

O contrato correto é J = instante presente E texto restante após trim; os carimbos são `DateTimeField`, não o dia do fato. `app/chamados/juizes.py:1552`, `app/chamados/models.py:2553`, `app/chamados/models.py:2554`, `app/chamados/models.py:3531`.

| Campo | Quem escreve em produção |
|---|---|
| `respondida_em` | Nascimento com campos recebidos; resposta humana; resposta automática; recuperação pela trilha; preenchimento de simetria em `carimbar`; limpeza por reabertura/ressuscitação e pela nova porta, via lote. `app/chamados/models.py:3124`, `app/chamados/services/disputa_emissao.py:1482`, `app/chamados/services/disputa_emissao.py:1675`, `app/chamados/services/disputa_emissao.py:1696`, `app/chamados/models.py:3243`, `app/chamados/services/disputa_emissao.py:1648`, `app/chamados/models.py:1629`, `app/chamados/services/validacao.py:440`. |
| `resposta_colab` | Nascimento; resposta humana/automática; limpeza integral na reabertura, ressuscitação e saneamento L-130. `app/chamados/models.py:3124`, `app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/services/disputa_emissao.py:1609`, `app/chamados/models.py:1630`, `app/chamados/services/validacao.py:440`. |
| `validada_em` | Validação administrativa; preenchimento de simetria em `carimbar(resposta='validar')`; limpeza por nova resposta, devolução de validação, BUG-PAR-103, reabertura e ressuscitação. `app/chamados/services/validacao.py:168`, `app/chamados/models.py:3246`, `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/services/validacao.py:273`, `app/chamados/services/validacao.py:349`, `app/chamados/services/disputa_emissao.py:1609`, `app/chamados/models.py:1629`. |
| `materializada_em` | Atribuição central em `_escrever_carimbo`, usada por `carimbar`, `carimbar_identidade` e `descarimbar`; limpeza de lote em `descarimbar_em_lote`. `app/chamados/models.py:3210`, `app/chamados/models.py:3240`, `app/chamados/models.py:3293`, `app/chamados/models.py:3319`, `app/chamados/models.py:3394`. |
| `via_resolucao` | Mesmas portas de carimbo; adicionalmente, encerramento por fato no espelho e relavra da via derivada. `app/chamados/models.py:3212`, `app/chamados/models.py:3394`, `app/chamados/models.py:3403`, `app/chamados/services/relavra_via.py:125`. |
| `DecisaoPergunta` | Escritor físico `registrar`. Recebe atos de nascimento, resposta, reabertura/recusa, validação/devolução, silêncio, retratação, absorção e reconstrução da projeção. `app/chamados/models.py:4124`, `app/chamados/models.py:3129`, `app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1631`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:354`, `app/chamados/models.py:2005`, `app/chamados/models.py:2137`, `app/chamados/services/materializacao.py:1048`, `app/chamados/services/validacao.py:439`. |
| Evento novo | A porta chama `evento('l130_instante_sem_texto_devolvida')`; o escritor de `LogAuditoria` é `core.observ.evento`. `app/chamados/services/validacao.py:449`, `app/core/observ.py:134`. |

Os leitores ficam distribuídos assim:

| Campo | Quem lê |
|---|---|
| Par instante/texto para decidir resposta | Juiz Python, faces SQL, vitalidade temporal, property, estado, retenção, corrigíveis e fila a validar. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1423`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1588`, `app/chamados/models.py:3669`, `app/chamados/models.py:3156`, `app/chamados/models.py:1925`, `app/chamados/juizes.py:1753`, `app/chamados/juizes.py:1760`. |
| Par instante/texto nos consumidores migrados | API/web, regularização/reabertura, declaração de hora, partição do app, validação/recusa, becos, canônica/mérito, materialização, reconciliador, cartório, ficha, calendário, prévia e auditoria. `app/api/views.py:2577`, `app/api/views.py:2620`, `app/chamados/views.py:1280`, `app/chamados/views.py:1417`, `app/chamados/services/acoes_disputa.py:98`, `app/chamados/services/acoes_disputa.py:133`, `app/chamados/services/fio.py:706`, `app/chamados/services/declaracao_texto.py:417`, `app/chamados/services/declaracao_texto.py:687`, `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/services/validacao.py:681`, `app/chamados/services/validacao.py:900`, `app/chamados/services/becos.py:30`, `app/chamados/services/fio.py:1281`, `app/chamados/services/fio.py:1294`, `app/chamados/services/materializacao.py:872`, `app/chamados/services/materializacao.py:1315`, `app/chamados/services/materializacao.py:2069`, `app/chamados/reconciliador.py:366`, `app/chamados/reconciliador.py:1085`, `app/ponto/services/cartorio.py:1642`, `app/colaboradores/services/ficha.py:171`, `app/colaboradores/services/calendario.py:505`, `app/folha/previa.py:142`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1658`. |
| `respondida_em` como tempo bruto | Reconstrução da projeção, marcos de invalidação, histórico, idade da fila e serialização API; também o placar divergente. `app/chamados/models.py:4095`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/historico.py:99`, `app/chamados/services/validacao.py:1254`, `app/api/views.py:2563`, `app/chamados/services/verbos.py:335`. |
| `resposta_colab` como conteúdo | Display, parsing do horário/data, opções/escapes, classificação da resposta e testemunha declarado × gravado. `app/chamados/models.py:3692`, `app/chamados/juizes.py:364`, `app/chamados/juizes.py:1788`, `app/chamados/juizes.py:1935`, `app/chamados/juizes.py:2039`, `app/chamados/services/materializacao.py:625`, `app/chamados/services/materializacao.py:718`, `app/chamados/services/materializacao.py:953`, `app/chamados/services/materializacao.py:1562`, `app/chamados/services/validacao.py:1181`. |
| `validada_em` | Gates de chamado e retenção, estado/vitalidade/SQL, validação/devolução/recusa/lote, materialização/irmã, declaração, fila de flip, vigias, placares, histórico e auditorias. `app/chamados/models.py:310`, `app/chamados/models.py:344`, `app/chamados/models.py:1924`, `app/chamados/models.py:3151`, `app/chamados/models.py:3497`, `app/chamados/juizes.py:1389`, `app/chamados/services/validacao.py:269`, `app/chamados/services/validacao.py:684`, `app/chamados/services/validacao.py:897`, `app/chamados/services/validacao.py:1062`, `app/chamados/services/materializacao.py:199`, `app/chamados/services/materializacao.py:2070`, `app/chamados/services/declaracao_texto.py:227`, `app/ponto/services/flip_auto.py:130`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/services/verbos.py:264`, `app/chamados/services/painel.py:270`, `app/chamados/services/historico.py:101`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`. |
| `materializada_em` | Ressuscitação, retenção, estado, portas de carimbo, vitalidade/lastro, reconciliador, materialização/passivo, validação, API/web, emissão, procedência, marcos temporais, histórico e comandos de estoque/auditoria. `app/chamados/models.py:1531`, `app/chamados/models.py:1924`, `app/chamados/models.py:3153`, `app/chamados/models.py:3293`, `app/chamados/juizes.py:683`, `app/chamados/juizes.py:1502`, `app/chamados/reconciliador.py:368`, `app/chamados/reconciliador.py:631`, `app/chamados/services/materializacao.py:862`, `app/chamados/services/materializacao.py:2071`, `app/chamados/services/passivo_validado.py:534`, `app/chamados/services/validacao.py:264`, `app/chamados/views.py:1257`, `app/chamados/services/disputa_emissao.py:776`, `app/ponto/services/procedencia_batida.py:74`, `app/chamados/veredito_lavrado.py:203`, `app/chamados/services/historico.py:103`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`. |
| `via_resolucao` | Identidade/terminalidade, vitalidade e projeção temporal, ressuscitação, estado/carimbo, resolver, canônica/mérito/dedup, materialização/recusas/absorção, tarefa, relavra, Pauta/tranca, passivo, explicador e templates. `app/chamados/models.py:3312`, `app/chamados/models.py:3484`, `app/chamados/juizes.py:683`, `app/chamados/juizes.py:1390`, `app/chamados/models.py:1535`, `app/chamados/models.py:3166`, `app/chamados/services/disputa_emissao.py:2072`, `app/chamados/services/fio.py:1293`, `app/chamados/services/fio.py:1400`, `app/chamados/services/materializacao.py:868`, `app/chamados/services/materializacao.py:1111`, `app/chamados/services/tarefa_do_dia.py:256`, `app/chamados/services/relavra_via.py:118`, `app/chamados/services/competencia_trancada.py:276`, `app/chamados/services/passivo_validado.py:141`, `app/chamados/services/explicador.py:118`, `app/templates/chamados/partials/_card_atendimento.html:192`. |
| Decisões/evento | Decisões alimentam projeção temporal, tarefa, respostas anteriores e reconstrução da projeção; o evento fica disponível na consulta genérica da auditoria. `app/chamados/juizes.py:1389`, `app/chamados/services/tarefa_do_dia.py:158`, `app/chamados/services/tarefa_do_dia.py:421`, `app/chamados/models.py:3679`, `app/chamados/models.py:4098`, `app/core/views.py:705`. |

**Não há significado uniforme em todos os leitores.** Os resíduos do achado 2 ainda confundem presença de carimbo ou de string com resposta. Além disso, `validada_em` tem assinatura administrativa e preenchimento de simetria; `materializada_em` também registra encerramentos sem plantio de batida. Esses significados já existem nas portas, não foram criados pelo descarimbo B2. `app/chamados/services/verbos.py:335`, `app/chamados/canal.py:65`, `app/chamados/models.py:3245`, `app/chamados/models.py:3175`, `app/chamados/models.py:3415`.

O instante da resposta não é o dia do fato: a recuperação usa o instante testemunhado pela nota; a resposta automática usa instante recebido; K6 usa o dia do fato. Não encontrei conversão nova desses carimbos em “dia de competência”. `app/chamados/services/disputa_emissao.py:1687`, `app/chamados/services/disputa_emissao.py:1675`, `app/chamados/services/materializacao.py:784`. Ausência integral de tal conversão fora dos caminhos examinados **[nao verificado]**.

**2. Leitores residuais: migrar ou preservar**

A classificação formal E/F depende do censo citado, que não veio no dado nem está disponível na cópia **[nao verificado]**. A classificação abaixo é pela finalidade observável.

| Residual | Avaliação |
|---|---|
| `fio.espera_o_texto_do_colaborador` | **Deveria delegar.** Recompõe `I ausente OU instante sem texto`, equivalente hoje a `not J`; alimenta cobrança e cartão. `app/chamados/services/fio.py:187`, `app/chamados/services/cobranca.py:227`, `app/chamados/services/fio.py:359`. |
| `canal.perguntas_mudas_com_resposta` | **Deveria migrar**, se “sem resposta” mantém o sentido declarado: exige texto exatamente vazio e perde brancos/NBSP/texto sem instante. Consumido pelo cobrar. `app/chamados/canal.py:53`, `app/chamados/canal.py:65`, `app/chamados/views_cobrar.py:181`. |
| `verbos.placar_aceite` | **Deveria migrar.** “Respondidos” depende somente de I após cobrança; chega ao painel e à Folha. `app/chamados/services/verbos.py:325`, `app/chamados/services/verbos.py:335`, `app/chamados/services/painel.py:298`, `app/folha/views_retrato.py:515`. |
| `_universo_da_ressuscitacao` | **Regra operacional própria**, não exibição: declara “SEM resposta”, mas seleciona NULL/`''`. Brancos/NBSP ficam fora; T sem I conserva proteção pelo texto. `app/chamados/models.py:1494`, `app/chamados/models.py:1530`. |
| `bin/censo_folha.py` | **Também residual**, fora de `app/`: publica `PERG_VIVAS`/`AGUARDA_ADMIN` por I nulo/não nulo. É instrumento de diagnóstico; uso efetivo em produção **[nao verificado]**. `bin/censo_folha.py:28`, `bin/censo_folha.py:29`, `bin/censo_folha.py:37`. |

Preservações legítimas por finalidade:

- **Trilha, projeção e cache temporal:** leem o carimbo bruto, inclusive ilegal, para não apagar evidência. `app/chamados/models.py:4095`, `app/chamados/models.py:4102`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `app/chamados/services/historico.py:98`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/painel.py:67`.
- **Integridade/estoque específico:** detectam meio-par, não julgam resposta operacional. I20 e curar limbo ainda usam `!= ''`, portanto seu diagnóstico também não acompanha o trim. `app/chamados/services/vigia_fio.py:737`, `app/chamados/services/disputa_emissao.py:1692`, `app/chamados/services/disputa_emissao.py:1861`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1105`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`.
- **Preservação de voz e reparo do par:** impedir sobrescrita de texto bruto na resposta automática; preencher carimbo faltante na porta de simetria. `app/chamados/services/disputa_emissao.py:1663`, `app/chamados/models.py:3241`.
- **Parse e apresentação de conteúdo/testemunha:** não são, por si, outro julgamento de J. `app/chamados/services/materializacao.py:625`, `app/chamados/services/materializacao.py:718`, `app/chamados/services/materializacao.py:770`, `app/chamados/templatetags/chamados_tags.py:139`, `app/chamados/templatetags/chamados_tags.py:166`, `app/chamados/services/fio.py:377`.
- **API, ponto e templates:** serialização, seleção de opção, preenchimento e exibição do cru. As condições operacionais dos dois templates alterados usam `.respondida`. `app/api/views.py:1163`, `app/api/views.py:2563`, `app/api/views_mensageria.py:567`, `app/ponto/services/dia_do_colab.py:70`, `app/templates/chamados/partials/_card_atendimento.html:114`, `app/templates/chamados/partials/_card_atendimento.html:141`, `app/templates/chamados/partials/_card_atendimento.html:148`, `app/templates/chamados/partials/_card_atendimento.html:154`, `app/templates/chamados/partials/_card_atendimento.html:182`, `app/templates/chamados/partials/_lote_preview.html:24`, `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/modal_fio.html:217`.

Não apareceu negação executável `~q_respondida_com_texto()` nos leitores examinados; a nova face escreve os ramos explicitamente. `app/chamados/juizes.py:1588`. Ausência por execução **[nao verificado]**.

**3. Complemento SQL e whitespace**

**Conforme por inspeção.** O conjunto de brancos vem de `str.isspace()`, e não de `\s` do PostgreSQL; Python usa `(t or '').strip()`. `app/chamados/juizes.py:1526`, `app/chamados/juizes.py:1560`.

| Valores | Positiva | Negada |
|---|---:|---:|
| Sem I, qualquer texto | Falso | Verdadeiro |
| Com I, texto NULL, `''`, somente espaços, NBSP, tabs ou LF | Falso | Verdadeiro |
| Com I e algum caractere não branco | Verdadeiro | Falso |

A tabela decorre dos ramos NULL/vazio, regex positiva e negação escrita. `app/chamados/juizes.py:1572`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1588`.

O teste B2 contém seis pares e não testa NULL/tab/LF; a bateria anterior cobre tab/LF, NBSP e outros brancos. NULL SQL não é exercitado porque o campo é NOT NULL; `None` aparece apenas no teste Python. `app/chamados/tests/test_f1nova_b2_leitores.py:127`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:11`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:29`, `app/chamados/tests/test_e3_forma_sql_do_juiz.py:118`. Concordância executada no banco desta cópia **[nao verificado]**.

**4. Pergunta viva com I e sem T**

| Leitor | Resultado atual |
|---|---|
| App/consulta | Pendente se a disputa estiver aberta; em disputa fechada, histórico. `app/chamados/services/pergunta_no_app.py:81`. |
| Fila a validar | Não entra. `app/chamados/juizes.py:1760`. |

[truncado em 20000 de 33679 caracteres]

## 16/11 22:23 · sessao 833c837a69a0 · - · - · origem=Ronald (tmux)

Achados:

- **Médio — relógio fora da borda por novo caminho.** `impedimento_estoque_l130` não recebe `agora` e chama `pergunta_viva(perg)` sem relógio (`app/chamados/services/validacao.py:381`, `:398`). O juiz pode cair em `date.today()` (`app/chamados/juizes.py:1471`, `:596`) e passar `agora=None` a `classificar_falta` (`:1475`), que lê `_tz.now()` (`app/escala/servico_jornada.py:387`). O comando lê `timezone.localtime()` somente depois da seleção, para o arquivo de reversão, e não o transmite (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:61`). A consequência documentada exige um relógio na borda até o julgamento (`app/docs/PLANO-PADROES.md:99`–`:103`).

- **Médio — lista fechada não pertence à porta.** A porta aceita qualquer pk que satisfaça o predicado; nem o predicado nem a porta consultam `ESTOQUE_L130_ALVOS` (`app/chamados/services/validacao.py:378`, `:381`, `:408`, `:432`). O comando compara a lista antes das travas (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:53`), depois chama a porta separadamente por pergunta (`:70`–`:76`). Assim, garante igualdade na fotografia inicial, não igualdade do conjunto durante as escritas; também permite aplicação parcial se outra pergunta perder elegibilidade ou falhar. A porta individual continua segura contra perda de elegibilidade daquela pergunta porque relê o predicado travado (`app/chamados/services/validacao.py:431`–`:433`).

- **Médio — teste não prova delegação da property.** O teste compara resultados nos casos listados; uma property que copie integralmente a expressão do juiz passa por ele (`app/chamados/tests/test_f1nova_b2_leitores.py:75`–`:85`). Há casos que, por inspeção, distinguem os mutantes antigos de B, C e D (`:61`–`:69`, `:75`–`:91`, `:142`–`:147`), porém o arquivo não contém injeção de mutantes, patch do juiz ou teste que force a delegação. Mordida executada: [não verificado].

Conforme:

- Ordem C → Disputa → Chamado → Pergunta pela porta canônica: chamada em `app/chamados/services/validacao.py:429`; aquisições em `app/chamados/models.py:2771`, `:2780`, `:2793`, `:2797`.
- Releitura sob trava e uma escrita pelo descarimbo canônico: `app/chamados/services/validacao.py:431`–`:440`; único `update` do descarimbo em `app/chamados/models.py:3392`–`:3394`. Não nasce segundo escritor de descarimbo nesse trecho.
- Linha RESPOSTA registrada antes do `update`, no mesmo `atomic` (`app/chamados/services/validacao.py:428`, `:439`, `:440`). O evento vem depois, mas `None` levanta exceção dentro do mesmo `atomic`, revertendo pergunta e sequência (`:449`–`:457`). `evento` retorna registro ou `None` (`app/core/observ.py:133`–`:139`); `DecisaoPergunta.registrar` não engole falhas (`app/chamados/models.py:4123`–`:4127`).
- Idempotência individual decorre de retirar `respondida_em`: segunda chamada encontra `nao_e_instante_sem_texto` (`app/chamados/services/validacao.py:390`, `:440`). A projeção evita duplicar RESPOSTA pelo mesmo instante (`app/chamados/models.py:4098`–`:4105`). O teste verifica segunda execução do comando, não duas chamadas diretas nem rollback por falha do evento (`app/chamados/tests/test_f1nova_b2_leitores.py:238`–`:244`).
- Complemento SQL correto por inspeção para NULL, vazio, espaços, NBSP, tab e LF: Python usa `(t or '').strip()` (`app/chamados/juizes.py:1526`–`:1528`), regex deriva os caracteres de `isspace()` (`:1560`–`:1562`), vazio SQL inclui NULL (`:1572`), positivo exige I e algum não branco (`:1580`), negativo é ausência de I ou I com vazio (`:1588`). Execução no PostgreSQL desta cópia: [não verificado].
- O teste B2 da face negada tem seis pares, sem NULL/tab/LF (`app/chamados/tests/test_f1nova_b2_leitores.py:127`–`:129`). A bateria anterior cobre tab/LF, NBSP e outros brancos (`app/chamados/tests/test_e3_forma_sql_do_juiz.py:29`–`:38`, `:89`–`:120`). NULL SQL não é exercitado porque o campo é NOT NULL; há teste de `None` somente em Python (`:11`–`:15`, `:118`–`:120`).
- `test_o168` preserva o sujeito e adiciona controle positivo legal antes das duas metades ilegais; a inversão corresponde à L-130 (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:287`–`:297`). Restou docstring histórica que afirma “nunca solta”, agora contrariada pelo próprio teste (`:276`–`:280`): **menor**, documentação.
- `test_e3_10` inverte exatamente a retenção de instante sem texto, conserva controle positivo e mantém `pode_fechar=False`; não enfraquece essa proteção (`app/chamados/tests/test_e3_10_cura_no_escritor.py:127`–`:148`).
- `test_estado_aplicacao` completa o objeto sintético com instante fixo, adequando a fixture respondida ao novo contrato; as asserções permanecem específicas (`app/chamados/tests/test_estado_aplicacao.py:37`–`:44`, `:49`–`:77`).
- K6 usa dia do fato, sem relógio implícito: `app/chamados/services/materializacao.py:784`–`:789` → `app/chamados/services/passivo_validado.py:86`–`:95`.

Nenhum teste foi executado, nenhum banco acessado e nenhum arquivo escrito. Execução e resultados de produção: [não verificado]. [nao verificado]

## 16/11 22:23 · sessao ba4d04fd8d7d · - · - · origem=Ronald (tmux)

Q2 — resíduos que deveriam ler o juiz:

- `espera_o_texto_do_colaborador` ainda recompõe `not J`: `I ausente OU respondida_sem_texto`. Hoje é equivalente, mas é derivação paralela; alimenta cobrança e cartão. `app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:187`, `app/chamados/services/cobranca.py:227`, `app/chamados/services/fio.py:359`.
- Canal partido chama seu universo de “Perguntas sem resposta” e exige `resposta_colab=''`: perde espaços, NBSP e texto sem instante. O cobrar consome esse detector. `app/chamados/canal.py:53`, `app/chamados/canal.py:65`, `app/chamados/views_cobrar.py:181`.
- Placar de aceite conta “respondidos” por `respondida_em__gte`, sem texto. É consumido pelo painel e pela Folha. `app/chamados/services/verbos.py:277`, `app/chamados/services/verbos.py:325`, `app/chamados/services/verbos.py:335`, `app/chamados/views.py:1489`, `app/chamados/services/painel.py:298`, `app/folha/views_retrato.py:515`.
- Ressuscitação seleciona materializadas com texto NULL/`''`, embora descreva seu universo como perguntas “SEM resposta”. Espaços/NBSP ficam fora; texto sem instante recebe a proteção reservada à resposta. É decisão operacional, não exibição. `app/chamados/models.py:1494`, `app/chamados/models.py:1530`, `app/chamados/models.py:1533`. Sua classificação formal E/F no censo é [não verificado].

E/F legítimos por sua função:

- Trilha/projeção: `app/chamados/models.py:4095`, `app/chamados/models.py:4102`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `app/chamados/services/historico.py:98`.
- Marcos temporais/cache: `app/chamados/veredito_lavrado.py:202`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/services/painel.py:64`.
- Integridade/estoque específico: `app/chamados/services/vigia_fio.py:737`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1104`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/services/disputa_emissao.py:1692`, `app/chamados/services/disputa_emissao.py:1861`. Auditor I20 e curar limbo usam `!= ''`, portanto não espelham trim, mas julgam integridade, não estado operacional. `app/chamados/management/commands/auditar_invariantes_chamados.py:1105`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`.
- Preservar texto bruto/escrever metades: `app/chamados/services/disputa_emissao.py:1663`, `app/chamados/models.py:3241`.
- Parse/classificação de conteúdo: `app/chamados/services/validacao.py:467`, `app/chamados/services/validacao.py:1181`, `app/chamados/services/materializacao.py:546`, `app/chamados/services/materializacao.py:625`, `app/chamados/services/materializacao.py:718`, `app/chamados/services/materializacao.py:770`, `app/chamados/services/materializacao.py:953`, `app/chamados/services/materializacao.py:1562`.
- Fallback da testemunha por conteúdo bruto: `app/chamados/templatetags/chamados_tags.py:139`, `app/chamados/templatetags/chamados_tags.py:166`, `app/chamados/services/fio.py:377`.
- Exibição API/ponto: `app/api/views.py:1163`, `app/api/views.py:2561`, `app/api/views.py:2637`, `app/api/views_mensageria.py:567`, `app/chamados/views_cobrar.py:292`, `app/ponto/services/dia_do_colab.py:70`, `app/ponto/services/veredito_celula.py:197`. A API de validação recebe seu universo do juiz. `app/api/views_mensageria.py:539`.
- Templates: seleção/exibição do cru em `_card_atendimento.html:114`, `:141`, `:148`, `:154`, `:182`; datas sob `resposta_enviada` em `:106` e `:391`; preview cru em `_lote_preview.html:24`; os dois templates migrados guardam a resposta com `.respondida`. `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/modal_fio.html:217`.

Q4 — pergunta viva I sem T:

| Leitor | Resultado |
|---|---|
| App | Pendente se disputa aberta. `app/chamados/services/pergunta_no_app.py:81` |
| Fila validar | Não entra. `app/chamados/juizes.py:1760` |
| Estado | Pendente. `app/chamados/models.py:3156` |
| Retenção | Não integra respostas sem veredito. `app/chamados/models.py:1923` |
| Pode fechar | Não fecha enquanto fio vivo. `app/chamados/models.py:1934` |
| CSF | Essa pergunta não segura pelo predicado de conversa. `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:588` |
| Órfã | Ausência de resposta permite continuar; ainda exige fato=True, C6 e teto. `app/chamados/reconciliador.py:1085`, `app/chamados/reconciliador.py:1087`, `app/chamados/reconciliador.py:1091` |
| Pauta DP | Não entra por resposta sem veredito; só mede trancada, não mera exportada. `app/chamados/services/competencia_trancada.py:95`, `app/chamados/services/competencia_trancada.py:126`, `app/chamados/services/competencia_trancada.py:137` |
| Cobrar | Conta como esperando colaborador, sujeito ao C6/canal/prazo. `app/chamados/services/cobranca.py:241`, `app/chamados/services/cobranca.py:252`, `app/chamados/services/cobranca.py:255` |
| Vigia | Pode classificar causa b, depois de precedências anteriores. `app/chamados/services/vigia_fio.py:628`, `app/chamados/services/vigia_fio.py:634` |

Tarefa/dono não decorre apenas de I/T: última linha RESPOSTA vazia exige responder/colaborador; projeção sem linha gera desconhecimento; linhas posteriores podem mudar o próximo ato. `app/chamados/services/tarefa_do_dia.py:154`, `app/chamados/services/tarefa_do_dia.py:159`, `app/chamados/services/tarefa_do_dia.py:163`. O dono colab exige também presença entre pendentes do app e ação responder. `app/chamados/services/tarefa_do_dia.py:651`.

Existe oposição condicional: placar diz “respondido” e app diz “pendente” para I sem T cujo instante esteja na janela pós-cobrança. `app/chamados/services/verbos.py:335`, `app/chamados/services/verbos.py:342`, `app/chamados/services/pergunta_no_app.py:81`. A ocorrência dessa janela nas pk reais é [não verificado].

## 16/11 22:23 · sessao 00869dd13935 · - · - · origem=Ronald (tmux)

Q6 — há caminhos de escrita que alcançam competência entregue; o comando novo não protege os demais consumidores. A ocorrência nas 141 perguntas do dado não pode ser afirmada sem conhecer suas datas, âncoras e lâmpadas [não verificado].

| Consumidor | Mudança e escrita possível | Entregue |
|---|---|---|
| **CSF / supra_juiz** | `sem_veredito_entre` deixa de reter I sem T/T sem I (`app/chamados/models.py:1923`). A lista compartilhada libera CSF (`app/ponto/supra_juiz.py:573`, `:588`); o executor chama `retratar` (`app/ponto/management/commands/supra_juiz.py:261`, `:286`). `_retratar` fecha chamado, pode rejeitar justificativa e chama resolver (`app/chamados/reconciliador.py:520`, `:525`, `:531`, `:538`). | **Sim, potencial.** O gate de pergunta viva exclui entregue explicitamente (`app/chamados/models.py:364`); portanto não substitui a retenção removida. O cron `--executar` está configurado (`app/config/crons.py:599`), mas sua janela padrão é atual (`app/ponto/management/commands/supra_juiz.py:90`). Alcance nas 141 [não verificado]. |
| **Pauta após fechamento** | S143 passa I sem T para `reabrir_credor` (`app/chamados/services/disputa_emissao.py:1843`, `:1969`), que recusa entregue por K6 (`:1857`, `:1867`) e escreve nota de recusa (`:1879`). O resolver coleta (`:1973`); `transicionar_com_pauta` escreve Pauta no mesmo ato (`app/chamados/services/fecho_runtime.py:161`, `:168`, `:139`). | **Sim, potencial**, após fechamento agora liberado pela retenção B; nenhum descarimbo é necessário para nascer essa Pauta. |
| **Cartório** | A isenção D passa de T para J (`app/ponto/services/cartorio.py:1641`); T sem I deixa de isentar o chamado e pode liberar dedup/fecho (`:1652`, `:1667`) ou tentativa de ressuscitação (`:1674`). | **Não há veto K6 nesse bloco.** A ressuscitação continua selecionando somente texto NULL/`''` (`app/chamados/models.py:1530`), então T sem I isolado não é descarimbado; irmãs vazias no mesmo chamado podem ser atingidas (`:1600`, `:1629`). O comentário reconhece alcance exportado histórico (`app/ponto/services/cartorio.py:1626`). |
| **Reconciliador — lavrar_sem_turno_aberto** | D passa a J (`app/chamados/reconciliador.py:365`); T sem I pode chegar ao carimbo `sem_turno_aberto` e ao fechamento da disputa (`:407`, `:413`). | **Não existe K6 neste caminho**; depende de ausência de turno e C6 morto (`:373`, `:399`). O carimbo escreve materialização/via (`app/chamados/models.py:3239`, `:3263`), sem tocar o par quando `resposta=None` (`:3241`). |
| **Reconciliador — veredito_orfa** | D passa a J (`app/chamados/reconciliador.py:1084`); T sem I pode produzir `ENCERRA` após lâmpada, C6 e teto (`:1087`, `:1093`). O consumidor chama `encerrar_fantasma` e evento (`:1138`, `:1150`, `:1152`), podendo fechar disputa (`:1173`). | **Sem veto K6 neste caminho**. `encerrar_fantasma` grava pela porta de carimbo (`app/chamados/models.py:3415`). Para I sem T a condição D já era falsa antes: a troca D não cria delta próprio nesse caso (`app/chamados/reconciliador.py:1085`). |
| **Becos** | O leitor C usa property (`app/chamados/services/becos.py:29`); I sem T torna-se visível quando destino não terminal, reduzindo candidatos. Havendo destino terminal, `pode_fechar` ainda decide (`:33`). O ato resolve perguntas e fecha disputa (`:54`, `:56`). | A retirada da retenção B pode liberar disputa cuja pergunta esteja morta pelo C6, inclusive trancada (`app/chamados/models.py:1934`). **O cron instalado usa `--alarme`, sem escrita** (`app/config/crons.py:663`); a porta por evento continua escrevendo (`app/chamados/services/becos.py:74`, `:78`). |
| **Materialização** | A seleção C agora exclui I sem T (`app/chamados/services/materializacao.py:2067`); reduz plantios. D `_materializar_resposta_hora` agora recusa T sem I com escrita de recusa (`:1314`, `:1322`). | A guarda K6 é específica para metades de intervalo fora163 (`:792`, `:805`), não geral. A fila pública não traz mais esses pares (`:2069`), mas a unidade `_materializar_uma` também tem consumidor no passivo (`app/chamados/services/passivo_validado.py:39`). Não afirmar novo plantio: a migração endurece anuência. |
| **Vigia fio** | Usa J no subtipo c, mantendo antes o subtipo b para I sem T (`app/chamados/services/vigia_fio.py:634`, `:636`). | Esse trecho somente conta/lista e retorna (`:656`, `:660`). Não é escritor. |
| **views_cobrar** | I sem T sai do lote validar e entra em aguardando colaborador (`app/chamados/views_cobrar.py:169`, `:175`, `:197`). | **Sem nova escrita para o par ilegal**; validação só é chamada para `pendentes` J (`:202`, `:209`), em POST humano (`:151`). |
| **Dedup / backfill decisões** | A escolha canônica e mérito mudam T para J (`app/chamados/services/fio.py:1281`, `:1294`). O apply do dedup delega à porta do backfill (`app/chamados/management/commands/dedup_perguntas_fato.py:105`). | **O apply preserva grupos exportados**, com lista e saída PAREI (`:112`, `:132`); o contrato do veto aparece no próprio chamador (`:102`). |

Pauta da tranca: `medir` só trabalha se `periodo_trancado` existir (`app/chamados/services/competencia_trancada.py:95`); pede `respostas_sem_veredito(ignorar_tranca=True)` (`:126`). I sem T sai de `decidir_pelo_dp` (`:137`), podendo liberar `pode_fechar` se C6 também disser morto (`:178`); `encerrar` escreve chamado/disputa (`:209`, `:229`) e Pauta somente se sobrar lista (`:232`). **Exportada e trancada são estados distintos no próprio contrato** (`:5`); o dado não prova quantas das 141 são trancadas [não verificado].

Push: a migração de `recobrar_questionario` altera a contagem de pendentes (`app/chamados/services/disputa_emissao.py:1735`); o mesmo serviço envia push e grava nota (`:1748`, `:1758`), sem veto K6 nesse caminho. É ação de recobrança, não consequência demonstrada de cron [não verificado].

_6209 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
