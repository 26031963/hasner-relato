# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 18:47** · publicado em **14/11/2026 18:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 18:47 · sessao eb0432a81e21 · - · - · origem=Ronald (tmux)

Os cinco casos têm cobertura literal em **BO2910-ESTRUTURA**, cuja coluna `resolve` enumera “casos 01, 02, 05, 06, 08”; balde LATERAL-ESTRUTURA, estado REGISTRADA, grade `.+.......`. Portanto, **cen não; lei sim; LATERAL NÃO PRONTA**. (app/docs/GRADE-MESTRA.md:170; legenda app/docs/GRADE-MESTRA.md:7)

```tsv
caso_fonte	caso_id	obra_id	estado_obra	prova_arquivo:linha	lateral_cen	lateral_lei	toca_congelado
BO2910-ESTRUTURA	01	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	02	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	05	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	06	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	08	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
```

Provas específicas, todas em **app/docs/BACKLOG.md:1102**:

- **01:** `vincular_posto` engole exceção e troca posto sem empresa; sítios `colaboradores/views.py:333-351` e `transferir_posto :1366`.
- **02:** chamado de texto livre sem leitor da célula; sítio `reconciliador.py:336`, `MODULOS_DE_TURNO`.
- **05:** parcial aprovada sem período pela tela; período obrigatório na aprovação/edição.
- **06:** desligamento grava `data_fim` sem conferir vigência; reativação não reabre vínculo.
- **08:** veredito `admin` sobrevive à regeneração da célula com marcos novos; sítio `veredito_celula.py:222-224`.

**Resolve por obra:** BO2910-ESTRUTURA → BO2910-ESTRUTURA/01, /02, /05, /06, /08. (app/docs/GRADE-MESTRA.md:170; app/docs/BACKLOG.md:1102)

**Subtotal:** 5 = NO AR 0 + EM VOO 0 + ESPINHA A FAZER 0 + LATERAL PRONTA 0 + LATERAL NÃO PRONTA 5 + SEM OBRA 0. (app/docs/GRADE-MESTRA.md:170; app/docs/GRADE-MESTRA.md:7)

A referência `BACKLOG:1059` impressa na grade está desatualizada: a linha efetiva da obra nesta cópia é **app/docs/BACKLOG.md:1102**. Nenhum dos seis congelados é citado nessa linha; alteração efetiva futura desses arquivos permanece **[nao verificado]**.

## 14/11 18:47 · sessao 32d3120f3ba2 · - · - · origem=Ronald (tmux)

Censo parcial consolidável, todo por leitura; **não sustento fechamento de toda a árvore** com esta fatia. Os caminhos abaixo foram identificados em AST e conferidos no fonte; permissões efetivamente atribuídas em produção permanecem `[nao verificado]`.

**Método executado:** `python3 -B`, `ast.parse`, `ast.walk`, inspeção de `Call`, `FunctionDef` e mapa de `ImportFrom` com `asname`. Leitura de URLs, chamadas e escritores complementar; nenhum arquivo criado.

**Portas especialmente relevantes fora de chamados**

| Ação | Porta e URL | Guarda real | Gravação/trilha |
|---|---|---|---|
| Resolver dia: trabalhou/faltou/justificado | `app/ponto/urls.py:58`; `app/ponto/views.py:3352` | POST + `tem_acao(...,'regularizar_batida')`, `views.py:3355`; alias `_vc` importado em `3354`; guardas de negócio **inline** em `services/veredito_celula.py:220`, `243`, `249`, `309`, `334` | Batida via `registrar_batida`, `:323`; Ausencia aprovada via `criar_ausencia`, `:337`; perguntas via `fechar_por_admin`, `:190`; decisão SILENCIO, `:195`; chamado reconciliado e ocorrência, `:203`; `CelulaDia.lavrar_veredito`, `:352`; LogAuditoria, `:356`. |
| Inverter batida | `urls.py:59`; `views.py:3300` | View `:3303`; guardas inline `flip_batida.py:15` retratada, `:19` autor, `:22` `competencia_fechada`, `:26` tipo | `Batida.tipo`, via `registro_batida.anotar`, `flip_batida.py:30`; trilha antes/depois e recompute `:39`. |
| Retratar batida | `urls.py:61`; `views.py:3314` | `_retratar` → `retratar_humano.retratar`, import `:3316`; `_pode` chamável `retratar_humano.py:131`; motivo/ativo inline `:166`, `:170` | `retratar_batida`, `:179`, motivo estruturado `:181`; efeito no dia. |
| Desfazer retratação | `urls.py:62`; `views.py:3326` | `_desfazer` → `retratar_humano.desfazer`, import `:3328`; `_pode`, `:194`; retratada/origem/prazo inline `:197`, `:199`, `:203`; `pode_desfazer(batida,agora=AGORA)` existe em `:105` mas **não cobre a guarda toda** | `Batida.aprovada=True,retratada_em=None`, `:207`, trilha via `anotar`. |
| Aprovar/rejeitar justificativa | `urls.py:40`, `43`; `views.py:1914`, `1926` | POST + `avaliar_justificativas`, `:1916`, `:1928`; executores `justificativa.py:132`, `262`; guardas inline | Aprovação `status/aprovada_por/aprovada_em/batida_gerada`, `justificativa.py:204`; Batida S, `:188`; invalida fechamento `:215`; encerra origem `:217`; trilha `:230`. Rejeição campos `:283`, chamado `:291`. |
| Aprovar justificativas em lote | `urls.py:41`; `views.py:1766` | Inline `:1769`, pendente `:1783`, `:1797`, batida recusada `:1825`, saída já utilizada `:1829` | Escrita **própria** `Justificativa.status/aprovada_por/aprovada_em`, `:1799`; batida S `:1818`; `batida_gerada` `:1834`; invalidação `:1808`; encerra origem `:1840`. |
| Rejeitar justificativas em lote | `urls.py:42`; `views.py:1862` | Inline POST/permissão `:1865`, pendente `:1877` | Campos próprios `status/aprovada_por/aprovada_em/motivo_rejeicao`, `:1878`; save `:1882`; notificações. Trilha própria de sucesso não observada nessa função `[nao verificado]`. |
| Materializar saída retroativa | `urls.py:44`; `views.py:3146` | `materializar_saida` `:3158`; hora e motivo mínimo `:3173`, `:3175`; executor `justificativa.materializar_saida_dp`, `:3185` | Executor pode registrar saída e justificativa; campos completos `[nao verificado]`. |
| Lançar ausência | `urls.py:57`; `views.py:2400` | `editar_ausencias`, `:2402`; `corpo_chegou_inteiro`, `:2431`; guardas inline `ausencia.criar_ausencia`, `ausencia.py:242`, `247`, `249`, `261` e sobreposição posterior | Ausencia aprovada `views.py:2447`; afastamento altera `Colaborador.situacao`, `:2465`. |
| Operar ausência existente | `urls.py:54`–`56`, `65`–`76`; funções em `views.py:2279`, `2320`, `2356`, `2601`, `2631`, `2697`, `2715`, `2758`, `2791` | `_a4_decidir` `views.py:85` chama `ponto.services.ausencia.decidir`; edição e reversão usam serviços respectivos; guardas completas `[nao verificado]` | Estado/range/dados de Ausencia; campos completos por variante `[nao verificado]`. |
| Recalcular/aprovar/reabrir fechamento | `urls.py:31`–`36`; funções `views.py:1229`, `1260`, `1386`, `1437`, `1464`, `1523` | `editar_folha`/POST; `aprovar_colaborador` lê `fechamento_lido` `:1483` e `_classificar_export` alias `folha.export.classificar_export` `:1508`; seleção de travas inline `:1512` | Serviços `fechamento.aprovar_lote:1323`, `aprovar:1335`, `reabrir_lote:1344`, `reabrir:1349`, `trancar_periodo:1400`, `reabrir_periodo:1432`; status/carimbos e período, com efeitos adicionais. |
| Exportar folha | `folha/urls.py:14`; `folha/views.py:102` | Guarda completa `[nao verificado]` | `ExportacaoDominio.registrar`, `aprovar_lote`, `registrar_log` na função. |

O serviço `veredito_celula` **não pode ser chamado por uma sonda somente leitura**: guarda e execução estão misturadas e ele inicia transação/travas em `app/ponto/services/veredito_celula.py:224`, antes da decisão final. Uma sonda não deve copiar essas condições.

**Porta administrativa adicional**

`/admin/` é publicado em `app/config/urls.py:24`. `JustificativaAdmin` deriva diretamente de `admin.ModelAdmin`, sem `SoLeitura`, sem `readonly_fields` e sem override de permissão (`app/ponto/admin.py:18`). Campos editáveis incluem colaborador, turno, batida de entrada, tipo, saída informada, status, aprovador, carimbo, rejeição, batida gerada e chamado de origem (`app/ponto/models.py:136`–`179`). Quem possui as permissões Django em produção `[nao verificado]`; trilha `LogAuditoria` da aplicação por esta porta `[nao verificado]`.

Batida, ImportacaoAEJ e Ausencia herdam `SoLeitura` (`app/ponto/admin.py:11`, `25`, `37`): recusas reais de add/change/delete retornam `False` em `app/core/admin.py:59`, `62`, `65`; todos os campos readonly em `:68`.

**Comandos de escrita identificados por AST**

Caminhos abaixo relativos a `app/chamados/management/commands/`:

| Comando | Chamada de escrita | Guarda/restrição observada |
|---|---|---|
| `curar_perguntas_em_limbo` | `:67 carimbar_resposta_pela_trilha`; `:69 reabrir_para_correcao` | Comparação exata com conjunto MEDIDAS inline `:46`–`53`; nota encontrada `:55`; `--apply` `:64`. |
| `fechar_becos_disputa` | `:57 becos.fechar_beco` | Guarda **chamável** `becos.classificar(d)` `:39`; implementação `services/becos.py:25`–`33`, usa `d.pode_fechar()`. |
| `carimbar_por_batida_coincidente` | `:98 _ctx.renovar_contexto`; `:100 _vinc.apontar_celula`; trilha `:101` | `_ctx` → `chamados.services.contexto`; `_vinc` → `.vinculos` imports `:58`; seleção inline `:60`–`85`. |
| `encerrar_perguntas_com_fato` | `:68 p.encerrar_por_fato_no_espelho`; `:88 d.fechar` | Seleção inline `:31`–`43` exige materializada, não validada, via sem história e `fato_do_marco_no_espelho(p) is True`. |
| `perguntar_chamados_sem_pergunta` | `:29 pergunta_no_app.perguntar` | Guarda integral `[nao verificado]`. |
| `silenciar_chamados_isentos` | `:49 silencio_isentos.silenciar` | Guarda chamável `silencio_isentos.alvos()` `:36`; implementação `services/silencio_isentos.py:20`: flag isento, estado vivo, ainda sem silêncio. |
| `arquivar_prescritos` | `:94 arquivo.silenciar` | Recortes inline `:43`, `49`, `53`, `60`; verbo validar/cobrar/decidir; idade criado_em; não silenciado nem resgatado. |
| `arquivar_sem_dia_irrecuperavel` | `:94 arquivo.silenciar` | Recortes inline `:58`–`68`; escopo DIA, sem dia do fato e anterior à corrente. |
| `arquivar_competencia_encerrada` | `:288 arquivo.silenciar` | `_disputa_viva` chamável `:148`; conjunto/corte integral inline `handle`. |
| `reabrir_orfas_enterradas` | `:157 p.reabrir_fantasma`; `:165 d.reabrir_sistema`; `:172 pai.reabrir` | Recortes inline `:67`–`112`; via fantasma, lâmpada False, pergunta viva, janela fixa do carimbo, exportada fora, recusa/irmãs do pai. |
| `encerrar_por_competencia_trancada` | `:29 competencia_trancada.encerrar` | Guarda completa de planejamento **chamável** `services/competencia_trancada.medir`, `:90`–`185`; `encerrar(...,aplicar=False)` `:188`–`193` só devolve medida. |
| `nomear_marco_por_lampada` | `:109 contexto.renovar_contexto`; evento `:111` | Guardas inline `:47`–`85`; dia apurável, lâmpada apagada e desempate da hora/origem/única. |
| `relavrar_via_i6` | `:102 relavra_via.relavrar` | `planejar` chamável `:44`–`70`; executa `universo_i6()`, vias derivadas, exclusão por ordem, `via_de_hoje`. |
| `backfill_decisoes_pergunta` | `:324 DecisaoPergunta.registrar` | Guardas do apply misturadas à execução; detalhes `[nao verificado]`. |
| `dedup_perguntas_fato` | `:105 aplicar` → `backfill_decisoes_pergunta.aplicar` import `:104` | Grupos `:58`; `plano_de_retratacao` real `:81`; recorte/horizonte/merito/exportada no apply. |
| `lavrar_perguntas_stale` | `:89 veredito_lavrado.lavrar` | Guarda chamável `raiox.pks_stale_por_varredura` `:60`; candidatos pelo destino vivo `:57`–`58`; teto inline `:61`. |
| `backfill_veredito_lavrado` | `:143`, `:146 gravar_lavras_em_lote` | Juiz real `_pergunta_materializada_de_fato` `:105`; divergência e não apagar lavra por indeterminação inline `:120`, `124`. |
| `backfill_fio_morto_e4b` | `_executar_dia:125`: reabrir credor `:135`, registrar decisão `:141`, perguntar `:143`, materializar `:147` | Planejamento chamável `ato_do_dia:82`; usa `T.tarefa_do_dia` e `reabrir_credor_de_pergunta_viva(...executar=False)` `:90`. |
| `validar_classe_a` | `:100 validacao.validar_pergunta` | Recorte inline `:44`–`67`; só respostas sem veredito, classificação real CLASSE_A; autor existente `:90`. |
| `reatar_beco_folga_ao_limbo` | `limbo_folgas.reatar_perguntas_ao_limbo`, import `:50` | Guarda real de seleção `_folga_nao_definida_sem_porta(...agora=AGORA)` `:54`; pai na `qs_na_fila` `:59`; sem pai/fora da fila `:63`–`68`; reversão obrigatória `:39`–`43`. |
| `retratar_chamados_lei_cala` | `:59 retratar` → `chamados.reconciliador.retratar`, import `:29` | Guardas inline `:32`–`47`: dia passado e `classificar_falta(...)[1] is None`. |
| `auditar_chamados_orfaos --fechar` | `:68 transicionar_com_pauta(...ato=ch.reconciliar)`; ocorrência `:73` | Recorte inline `:35`–`44`; gate real dentro da transição. |
| `materializar_passivo_validado` | `:104`, `:113 passivo_validado.reparar` | Mais restrições/autor em `:70`–`83`; **até dry escreve arquivo** `:85`, `:117`–`130`. |
| `geofence_registrados_para_analise` | `:32 ch.analisar` | Guarda integral `[nao verificado]`. |
| `alertar_chamados_sla` | `:44 nota.anexar_ocorrencia`; push `:48` | Guardas reais chamáveis `sla.vencido`, `sla.ja_avisado`, `sla.vence_em` `:32`–`34`; teto da passada inline `:37`. | [nao verificado]

`arquivo.silenciar` grava `ChamadoColaborador.silenciado_ate`, `.silenciado_por`, `.silenciado_motivo`, `.atualizado_em` e trilha `evento` (`app/chamados/services/arquivo.py:156`–`173`). Não muda `status_local`.

`relavra_via.relavrar` grava `PerguntaDisputa.via_resolucao`, `.veredito_lavrado`, `.julgado_em`, `.janela_expira_em` e evento por pergunta (`app/chamados/services/relavra_via.py:118`–`138`).

`competencia_trancada.encerrar` hoje não carimba perguntas (`app/chamados/services/competencia_trancada.py:133`–`135`, `203`). Encerra chamados pela transição (`:209`), anexa ocorrência e evento (`:210`–`219`), fecha disputa pela porta (`:229`), pode criar Pauta DP (`:232`, `256`).

**Cuidados necessários para a sonda**

- Não chamar comandos em dry presumindo ausência de escrita: `materializar_passivo_validado` cria pasta/JSON mesmo sem apply (`:85`, `118`).
- Não chamar serviço mutador e fazer rollback: isso já executa escrita e sinais; não satisfaz a restrição.
- Guardas inline devem sair como **não sondável**, sem contagem inventada.
- `core.portas_da_tela` não fecha este censo: exclui API/admin em `app/core/portas_da_tela.py:24` e detecta POST por texto em `:52`; guardas por regex em `:57`.
- `core.views.painel_operacional` e seu lote são leitura (`app/core/views.py:589`, `621`), não outra porta de decisão.
- `regularizar_retorno` usa `request.user.colaborador` (`app/ponto/views.py:3208`): é ação do próprio colaborador, não uma porta administrativa genérica.
- Leitura completa dos comandos de outros apps, API, escala/cadastro, férias e ações de IA ainda é necessária para declarar **TODA ação**. `[nao verificado]`

## 14/11 18:47 · sessao 505e81710761 · - · - · origem=Ronald (tmux)

Leitura somente; AST executado com `python3 ast.parse` em `app/chamados/{urls,views,views_cobrar,views_arquivar,views_regularizacao}.py` e `services/{fio,acoes_disputa}.py`. `app/chamados/admin.py` está vazio; não encontrei forms Python nesse app. O fechamento global da árvore e os commands ficam com sua consolidação. [nao verificado]

**Método e aliases.** Resolver `ast.ImportFrom` inclusive dentro de funções, `ast.Call(path)` nas URLs e os ramos `acao` dos roteadores; seguir chamadas aos serviços. Aliases importantes:

| Alias | Destino e evidência |
|---|---|
| `vreg` | `chamados.views_regularizacao`, `app/chamados/urls.py:6` |
| `_ac` | `chamados.services.acoes_chamado`, `app/chamados/views.py:423` |
| `_ad` | `chamados.services.acoes_disputa`, `app/chamados/services/fio.py:665` |
| `_ac_geo` | `chamados.services.acoes_chamado`, `app/chamados/services/fio.py:572` |
| `_corrig` | `chamados.models.perguntas_corrigiveis`, `app/chamados/views.py:1419` |
| `_reabrir2` | `chamados.models.reabrir_para_correcao`, `app/chamados/views.py:1435` |
| `_viva_pend` | `chamados.models.pergunta_viva`, `app/chamados/views.py:1415` |
| `_muda_a_folha` | `services.cobranca_folha.muda_a_folha`, `app/chamados/views_cobrar.py:88` |
| `_nota` | `chamados.services.nota`, várias importações, p.ex. `app/chamados/views.py:879` |

**Portas HTTP e roteadores.**

- `/chamados/local/<id>/`: `urls.py:14` → `views.py:359`; POST tem guarda contextual `pode_agir_chamado` em `views.py:419`; `staff` passa `_is_gestor` em `views.py:35`.
- `/chamados/colaborador/<id>/`: `urls.py:21` → `views.py:659`; `_is_gestor` em `:662`; POST delega a `processar_post_fio` em `:674`.
- `/chamados/modal/<id>/`: `urls.py:39` → `views.py:869`; `_is_gestor` em `:872`; usa o mesmo roteador `services/fio.py:523`.
- Roteador fio/modal resolve chamado pertencente ao colaborador e guarda `pode_agir_chamado` em `services/fio.py:601` e `:604`; exige disputa aberta, exceto reabertura administrativa e validação, em `:611`.
- O detalhe exige disputa aberta para validar/recusar/corrigir por filtro, mas o fio permite validar pergunta viva em disputa fechada: `views.py:491`, `:510`, `:529`; `services/fio.py:633`, `:638`, `:644`. Isto é diferença real entre portas.

**Tabela de ações principais.** Todos os nomes de arquivo abreviados abaixo partem de `app/`.

| Ação/porta | Guarda real | Gravação/trilha |
|---|---|---|
| `validar_pergunta`, detalhe/fio/modal | `chamados/services/validacao.py:418` exige carimbo de resposta; `:421` não validada; `:429` implausível exige confirmação; entrada detalhe `views.py:483`, fio `services/fio.py:623` | `_carimbar_validacao`: `validada_em/validada_por` e `DecisaoPergunta.VALIDACAO`, `validacao.py:159`, `:164`; materialização em `:488`, `:494`; logs `:504`, `:517`; recusa de materialização pode desfazer carimbo, `:489` |
| `recusar_pergunta`, detalhe/fio/modal | Entrada `views.py:503`, `services/fio.py:663`; `validacao.py:591` motivo, `:602` não validada, `:605` respondida, `:609` não fora do fato | `recusa_motivo`, `validacao.py:619`, `:625`; reabertura e `DecisaoPergunta` ato `recusa`, `:628`; nota `:633`, evento `:637`, log `:643`, push `:647` |
| `corrigir_resposta`, detalhe/fio/modal | Entrada `views.py:522`, `services/fio.py:663`; `acoes_disputa.py:336` HH:MM, `:338` motivo, `:345` não validada, `:347` disputa aberta, `:349` tipo hora; **não exige resposta anterior** | Nota preserva `'(sem resposta)'` em `:353`, `:359`; core escreve `resposta_colab/respondida_em` em `disputa_emissao.py:1467`, `:1508`, decisão `RESPOSTA` em `:1511`; log `acoes_disputa.py:375`; validação/materialização em `:379`. Pode responder pergunta muda |
| `regularizar_disputa`, detalhe | Entrada `views.py:463`; disputa aberta em `:467`; `acoes_disputa.py:124` resolução, `:130` bloqueia viva sem carimbo de resposta | Materializa validadas em `:139`; log `:144`; fechamento/modelo depende resultado |
| `regularizar_disputa`, fio/modal | `services/fio.py:684`; resolução `:686`; **guarda própria inline** viva sem resposta em `:698` | Materialização `:708`, log `:713`; esta é cópia de guarda, não delega `acao_regularizar_disputa` |
| `reabrir_disputa`, fio/modal | `services/fio.py:679`; `acoes_disputa.py:95` conta pendentes, `:97` chama `perguntas_corrigiveis`; sem corrigíveis recusa em `:98` | `reabrir_para_correcao` em `:103`; nota `:106`, push `:108`, log `:113`; este ramo **não recobra** questionário já pendente |
| Endpoint reabrir/recobrar | URL `chamados/urls.py:20`; `views.py:1372`; gate especial `:1404`: só consulta `pode_agir_chamado` se **não** tem `gerir_chamados`; `_disputa_do_chamado` `:1410`; vivas `:1417`; corrigíveis `:1420` | Havendo corrigíveis, reabre em `:1436`, nota `:1440`, log `:1456`; sem corrigíveis e com pendente viva, chama `recobrar_questionario` em `:1425` |
| Recobrança efetiva | Escolha é inline em `views.py:1421`–`:1434`; serviço em `disputa_emissao.py:1697` não é juiz puro | Renova `ChamadoColaborador.prazo_sla` em `:1725`; nota `:1728`; push `:1735`; log `:1741`; não escreve resposta | [nao verificado]
| Reabertura administrativa de disputa fechada | Fio `:619` → `acoes_disputa.py:235`; `:245` staff/admin_ti; motivo válido `:251`; justificativa ≥20 `:253`; disputa fechada `:255` | `reabrir_admin` `:259`; nova pergunta de conversa `:263`; reabre chamado `:270`; nota `:274`, pushes `:276`/`:287`, log `:297` | [nao verificado]
| Conferir data | URL `urls.py:18`, view `views.py:1292`; POST `:1307`, `gerir_chamados` `:1309`; serviço `validacao.py:686` existente, `:690` justificativa, `:693` `data_do_fato_de`, `:694` divergência, `:697` ainda não conferida | `PerguntaDisputa.data_conferida_em/por`, `validacao.py:699`; save `:701`; log antes/depois `:702` |
| Aplicar declaração de texto | **Só detalhe:** `views.py:478`; `declaracao_texto.py:649` consulta propostas reais pelo followup; `:668` recusa pergunta já validada sem registro | Emite/reusa disputa `:659`; abre pergunta `:666`; core de resposta `:693`; valida com confirmação implausível em `:696` |
| Cobrar chamado | URL `urls.py:25`; `views_cobrar.py:24`; `gerir_chamados` `:25`, POST `:27`, `recusa_de_area` `:32`; throttle por colab/dia `:36`–`:42`; push precisa sucesso `:51` | Push `:43`; nota `[COBRANCA]`, `:55`; **não renova prazo** nesta porta | [nao verificado]
| Cobrar massa | URL `urls.py:31`; `views_cobrar.py:61`; fila filtrada `:71`, verbo `cobrar` `:79`, contextual `:92`, guarda real `_muda_a_folha` `:95`; throttle `:114`; `previa=1` leitor `:101` | Push `:119`; nota `:132`; sem mudança de resposta/prazo | [nao verificado]
| Cobrar dia | URL `urls.py:32`; `views_cobrar.py:306`; POST+`regularizar_batida` `:314`, departamento `:317`, data `:322`; serviço `cobrar_dia.py:28` recusa dia >=hoje | **Log antes do cartório**, `cobrar_dia.py:39`; `cartorio.julgar_celula(...forcar=True,toque=True)` `:41`; possível fábrica de disputa/perguntas `:50`; não é leitura nem mesmo quando resultado posterior recusa |
| Validar inline | URL `urls.py:33`; `views_cobrar.py:145`; permissão `:149`, área `:158`, disputa `:162`–`:167`; só perguntas vivas com `respondida_em`, `:169`; sem elas recusa `:193` | Delega `acao_validar_pergunta`, `:211`; mesmos efeitos da validação |
| Validar lote JSON/tela | URLs `urls.py:34`, `:37`; views `views_cobrar.py:232`, `:372`; gate contextual/permissão `:247`, `_pode_validar_lote` `:334`; classe real `validacao.py:770`, já validada `:767` | Delega `acao_validar_pergunta` `validacao.py:781`; tela acrescenta log `validou_lote_tela`, `views_cobrar.py:383` |
| Arquivar | URL `urls.py:26`; `views_arquivar.py:24`; `gerir_chamados`, POST, área `:26`, `:28`, `:32`; não exige idade no endpoint individual | `arquivo.silenciar` `:37`; `silenciado_ate/por/motivo/atualizado_em`, `services/arquivo.py:166`–`:172`; evento `:173`; nota `views_arquivar.py:44`; não muda `status_local` |
| Arquivar lote | URL `urls.py:28`; `views_arquivar.py:93`; idade >0 `:105`; filtro e verbos `:115`, `:125`; criado antes corte e silêncio nulo `:135`; contexto `:137` | `arquivo.silenciar`, `:140`; evento por chamado; sem nota individual nesta porta |
| Desarquivar | URL `urls.py:27`; `views_arquivar.py:52`; permissão/POST/área `:54`, `:56`, `:60`; via tranca tenta `ch.reabrir(humano=True)` `:70`, que pode recusar | `arquivo.levantar` `:77`, limpa véu e carimba `resgatado_em`; se tranca ressuscita perguntas `:86`; nota `:88`; reabertura pode ter efeitos próprios até na recusa |
| Assumir/liberar | URL `urls.py:41`; `views.py:1008`; `_is_gestor` `:1010`, `pode_agir_chamado` `:1015` | `_vinc=services.vinculos` `:1019`; `assumir_responsavel` `vinculos.py:121`, `atribuido_a/em/atualizado_em` `:132`–`:135`; liberar `:139`; nota `views.py:1027`/`:1031` e log `:1034` só se mudou |
| Mensagem rápida | URL `urls.py:40`; `views.py:961`; `gerir_chamados` `:967`, mensagem não vazia `:971` | `ChamadoColaborador.abrir` `:973` com módulo `mensagem_dp`; nota `:986`; push `:989`; ação é por colaborador, não carimba pergunta/dia | [nao verificado]
| Resposta livre e status | Detalhe `views.py:544` → `acoes_chamado.py:292`; fio `services/fio.py:746`; conteúdo e/ou status válido, contexto já guardado | Nota `acoes_chamado.py:301` / `fio.py:756`; logs `:304`, `:328`; `transicionar_com_pauta`, `:325` / `fio.py:760`, que pode lançar recusa depois da nota |
| Abrir modal/“ler” | URL `urls.py:39`, `views.py:869`; `_is_gestor` `:872` | **GET escreve** `FollowupChamado.lido_dp_em`: `views.py:880` → `services/nota.py:126`–`:133`. Não usar view inteira na sonda |
| Regularização externa criar | URL `urls.py:49`; `views_regularizacao.py:42`; `_gate` `:19` = `_is_gestor` e departamento supervisão; motivo `:49` | `regularizacao_ext.abrir_lote` `:310`; `RegularizacaoExterna` `:237`, chamado de rastro `:247`, vínculo `:257`; sem materializar neste ato |
| Reg externa adicionar/corrigir/remover item | URL `urls.py:51`; `views_regularizacao.py:74`; status `:80`; data/hora `:87`, `:97`; alvo do colab `:109`; horários exigidos `:114`, `:117`, diferentes `:120` | `RegularizacaoItemBatida.get_or_create`, `regularizacao_ext.py:326`: data, horas, acao, batida_alvo, intervalo |
| Reg externa excluir item | URL `urls.py:52`; `views_regularizacao.py:137`; status `:143` | `regularizacao_ext.remover_item` `:332`, `item.delete()` `:336` |
| Reg externa enviar | URL `urls.py:53`; `views_regularizacao.py:154`; status `:160`, itens `:163` | `regularizacao_ext.enviar_lote` `:340`, status `aguardando` em `:343`; pushes `views_regularizacao.py:170`, `:177`. Só colab confirma; materialização recusa `status != confirmada`, `regularizacao_ext.py:52`, ou qualquer disputa aberta do colab `:59`–`:66` | [nao verificado]

**Ramos genéricos adicionais do detalhe/fio, não omitir pelo nome do chamado:**

| Ação | Porta/guarda | Escrita |
|---|---|---|
| `aprovar_excecao` | `views.py:426`, `_pode_aprovar` `:41`; `acoes_chamado.py:25` só encerrou anteriormente; **serviço não exige categoria** | `registrar_batida` `acoes_chamado.py:44`, conflito `:54`, foto `:67`, nota `:69`, transição `:71`, log `:77` |
| `autorizar_geofence` e `_advertir` | `views.py:537`, fio `services/fio.py:571`; contexto `:579`; `acoes_chamado.py:182` encerrado; **serviço não exige categoria** | `_advertir` cria `Advertencia` `:199`; nota `:224`; transição `:229`; log `:230` |
| `recusar_geofence` | `views.py:540`, fio `:571`; `acoes_chamado.py:260` encerrado, `:263` motivo, `:265`–`:273` batida vinculada e pertencente ao colab | `retratar_batida` `:277`, nota `:281`, transição `:286`, log `:287` |
| `aceitar_folga_limbo` | `views.py:432`; `limbo_folgas.py:85` sugestão fixa/dias/template, `:88` template existente | `salvar_tipo` `:92`, altera folgas de `TipoEscala`; log `:95`; reconcilia chamados LIMBO do mesmo template `:99`–`:104` |
| `corroborar_regua` | `views.py:441`, procura vínculo ativo por template; `escala/regua_defesa.py:655` vínculo, `:657` ainda não corroborado | `gravar_regua` `:662`, confiança 0, dias baixo 2, avaliada/corroborada em/por `:666`–`:667`; nota `:672` |
| `realocar_regua` | `views.py:452`; `regua_defesa.realocar_para_destino` `:326`; destino/template `:305`, `:308`; cíclico recusado `:310`; **juiz puro existente** `colaboradores.services.vinculo.julgar_vinculo` chamado `:316`; no-op `:319` | `executar_vinculo` `:321`; nota `:333` |
| `pedir_confirmacao_ancora` | `views.py:459`, fio `:559`; `acoes_disputa.py:185` categoria, `:187` área; helper pode recusar `:190` | `criar_disputa_ancora_de_chamado` `:189`; log `:192`; fábrica em `disputa_emissao.py:1302` |
| `iniciar_disputa_orfaos` | Só fio/modal `services/fio.py:587`; `acoes_disputa.py:219` juiz real `chamado_orfao_e_regularizavel`, nenhum `:220`, repasse recusado `:225` | Fábrica `:222`, log `:229` |
| `aplicar_proposta_escala` | Fio/modal `services/fio.py:547`; `proposta_no_fio.py:136` gate do `INTENCOES`; `_recalcular` `:138` e `:96`; grade dia a dia recusada `:141` | `escritor_de(CHAVE_APLICAR)` `:151` = `escala.services.propositor.aplicar` pelo registro; efeitos finais desse escritor [nao verificado] |
| `recusar_proposta_escala` | Mesmo roteador; `proposta_no_fio.py:168` gate, `:170` motivo, `_recalcular` `:173` | `escala.services.propositor.recusar` `:176`; campos exatos [nao verificado] |

**Fechamento de status não é “permitido” só por `pode_transicionar`:**

`ChamadoColaborador.pode_transicionar` chama o motor em `models.py:119`–`:126`, mas `transicionar` tem guardas seguintes: justificativa pendente `:136`, disputa viva `:153`–`:160`, perguntas vivas ancoradas `:165`–`:171`, premissa de furo `:181`–`:198`. Assim, sonda que só chama `pode_transicionar` precisa rotular “geometria da transição”, não “alcance final da ação”. Há leitor puro do **reconciliar** `reconciliaria`, `models.py:658`–`:662`; ele não substitui todas as guardas da transição humana.

**Forms e diferença entre anunciado/executável:**

- Modal tem forms validar `templates/chamados/partials/modal_fio.html:263`, `:279`; recusar `:290`; corrigir `:301`; conferir data `:173`; regularizar `:353`; reabrir `:368`; administrativo `:388`; geofence `:503`, `:514`, `:525`; folga `:548`; corroborar `:564`; realocar `:573`; exceção `:596`; órfãos `:625`; âncora `:641`.
- Detalhe tem reabrir `templates/chamados/detalhe_local.html:9`, exceção `:256`, geofence `:285`, regularizar `:317`, resposta livre/status `:385`.
- Reg externa tem criar `templates/chamados/regularizacao_painel.html:14`; del item `regularizacao_editar.html:30`, add `:45`, enviar `:63`.
- Cartão de detalhe tem validar `partials/_pergunta_card.html:70`, `:95`, recusar `:112`. Não há corrigir nesse template.
- **Três forms do modal não têm ramo do roteador correspondente:** `aplicar_declaracao_texto` em `modal_fio.html:140`, `confirmar_correcao_pergunta` `:190`, `apagar_pergunta_cega` `:197`; o conjunto de ações AST de `services/fio.py:523` não os inclui e POST sem conteúdo chega a `:781` “Nada a fazer”. `aplicar_declaracao_texto` tem handler **só no detalhe** em `views.py:478`. Os outros dois só apareceram no template e tests, sem handler de produção na busca. Portanto forms anunciados não podem ser contados como porta operante.

**Chamadas reais puras úteis para sonda, com limites:**

| Chamável | Uso correto |
|---|---|
| `_is_gestor(user)`, `_pode_aprovar(user)` | Guardas de papel, `views.py:35`, `:41` |
| `pode_agir_chamado`, `pode_agir_departamento`, `recusa_de_area` | Guardas RBAC reais, `utils.py:68`, `:52`, `:75` |
| `classe_do_lote(perg)` | Guarda classificatória real chamada pelo lote, `validacao.py:880`; lote usa em `:770`; não basta para todos os gates |
| `motivo_implausivel_validacao(perg)` | Mesma guarda lida pelo validar em `validacao.py:429`; origem exportada em models, implementação não auditada aqui |
| `data_do_fato_de(perg)` | Mesma autoridade de divergência chamada por conferir em `validacao.py:693`; não imita as outras guardas |
| `propostas_do_chamado(ch)` | Guarda real de proposta da aplicação em `declaracao_texto.py:649`; retorno pode ter `aceita=False` `:633`–`:637`; não equivale sucesso final |
| `perguntas_corrigiveis(d)` | Conjunto real do reabrir, `acoes_disputa.py:97`, `views.py:1420` |
| `_disputa_do_chamado(ch)` | Resolver real de disputa, `services/fio.py:120`, usado `views.py:1410` |
| `chamado_orfao_e_regularizavel(ch)` | Guarda da emissão órfãos usada `acoes_disputa.py:219` |
| `muda_a_folha(ch, contador, cache)` | Guarda real adicional da cobrança massa, `views_cobrar.py:95`; identidade resolvida pelo alias `:88` |
| `ch.pode_transicionar(status, perfil)` | Só uma parte da guarda humana, `models.py:119`; não confundir com sucesso da porta |
| `ch.reconciliaria()` | Consulta real e explícita sem escrita da reconciliação, `models.py:658` |
| `julgar_vinculo(colab, post, perfil)` | Juiz real da realocação chamado em `regua_defesa.py:316`; gates prévios do destino seguem inline |
| `_recalcular(colab, assinatura, vinculo)` | Guarda de identidade da proposta, `proposta_no_fio.py:96`; pureza transitiva do propositor [nao verificado] |

**Não chamar mutators na sonda**, ainda que retornem dicionário/recusa: `validar_pergunta`, `recusar_pergunta`, `conferir_data_do_fato`, `acao_corrigir_resposta`, `aplicar_declaracao`, `cobrar_dia`, `materializar_regularizacao_externa`, `ch.reabrir`, `arquivo.*`, `acao_*` e views. Guardas completas dessas ações estão embutidas em corpos que escrevem; não há modo dry na assinatura auditada. Mesmo `cobrar_dia` escreve log antes de descobrir resultado final (`cobrar_dia.py:39`). A sonda pode imprimir cobertura dos **subgates reais** e marcar a decisão final não sondável em leitura, sem copiar seus `if`s.

Não inferir validar=0: `validacao.py:418` só examina carimbo, e universo de pergunta devida pode incluir carimbo com texto vazio; medir os campos reais e os juízes, não tratar o nome “sem resposta” como equivalência de `respondida_em is None`.

**Perguntas de lei sugeridas, todas como perguntas:**

1. Uma pergunta devida muda pode receber a hora do ADMIN pela ação `corrigir_resposta`, que não exige fala anterior do colaborador? (`acoes_disputa.py:345`–`:353`, `:371`)
2. A recobrança que renova `prazo_sla` deve alterar a idade e o efeito da pergunta já vencida? (`disputa_emissao.py:1725`)
3. O ADMIN tem a mesma autoridade contextual no endpoint de reabrir/recobrar e no fio, dado o gate condicional do primeiro? (`views.py:1404`, `services/fio.py:604`)
4. Pergunta viva de disputa fechada deve ser tratável igualmente pelo detalhe e pelo fio? (`views.py:491`, `services/fio.py:638`–`:647`)
5. Os forms do modal que não têm handler contam como ação disponível ao ADMIN? (`modal_fio.html:140`, `:190`, `:197`; `services/fio.py:781`)
6. É permitido oferecer regularização externa para dia retido por pergunta cuja disputa aberta bloqueia sua materialização? (`views_regularizacao.py:54`, `regularizacao_ext.py:59`)
7. Ações de exceção e geofence precisam estar vinculadas à categoria correspondente para o servidor aceitá-las? (`acoes_chamado.py:25`, `:182`)
8. Abrir o modal deve ser considerado ação de escrita no censo? (`views.py:880`, `services/nota.py:132`)

## 14/11 18:44 · sessao f2a8e5e329cd · - · - · origem=Ronald (tmux)

- `F._carga` chama a função real `carga_da_tarefa`, sem adaptação: `app/folha/fora_da_folha.py:58`, `:63`.
- O universo contém relógio implícito: `dias_que_seguram` → `_dias_que_retem` → `motivos_retencao_celula` → `_dias_sem_decisao`, cujo `hoje` usa `timezone.localdate()` quando ausente. Evidências: `app/folha/fora_da_folha.py:105`, `:144`; `app/folha/export.py:1212`, `:1238`. Portanto, passar `AGORA` apenas à carga não fixa toda a execução.
- `freezegun==1.5.5` já está declarado: `app/requirements.txt:43`. Usar `freeze_time(AGORA, tick=False)` ao redor de todas as leituras, com `timezone.override('America/Sao_Paulo')`; o fuso da aplicação é esse em `app/config/settings/base.py:93`.
- A carga passa seu instante à fila, C6, consulta do app e partição: `app/chamados/services/tarefa_do_dia.py:509`, `:549`, `:555`; `app/chamados/juizes.py:1570`, `:1573`. Os fallbacks reais ainda leem relógio quando não recebem instante: `app/chamados/juizes.py:592`; `app/chamados/services/painel.py:190`; `app/chamados/services/verbos.py:35`.
- O catálogo de ausências usa cache **somente em memória do processo**, sem cache externo: `_CACHE` é um dict (`app/ponto/catalogo/ausencias.py:257`); `vigente` lê `time.monotonic()` e atribui o dict (`:274`, `:279`); `_do_banco` apenas consulta os tipos (`:249`). Não é necessário substituir `vigente`.
- **Há escrita de arquivos já no startup padrão do shell:** `manage.py` usa `config.settings.saas` (`app/manage.py:9`), cujo logging configura `RotatingFileHandler` para `errors.log` e `ponto.log` (`app/config/settings/saas.py:136`, `:160`). Para respeitar nenhum arquivo, iniciar Python com `-B`, importar o módulo de settings e definir nele `LOGGING_CONFIG = None` **antes** de chamar `manage.py` via `runpy.run_path`; só então executar `shell -c` com o corpo em memória. Essa configuração proposta não foi executada [nao verificado].
- Proteção proposta de banco: dentro de `transaction.atomic()`, primeiro comando SQL `SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY`; aplicar `schema_context('juliani')` antes das consultas e validar `SHOW transaction_read_only`. Compatibilidade dessa sequência com o ambiente de produção não executada [nao verificado].

## 14/11 18:44 · sessao b8e11c4362a3 · - · - · origem=Ronald (tmux)

- **Cron responsável pelo acervo stale:** `lavrar_perguntas_stale` seleciona exclusivamente perguntas cujo `chamado_destino` está em `VIVOS`; consulta a varredura, limita a rodada a 500 por padrão e chama `lavrar(p)` quando recebe `--apply`. Pergunta sem destino ou com destino fora de `VIVOS` não entra nesse universo. (`app/chamados/management/commands/lavrar_perguntas_stale.py:35`, `:57`, `:58`, `:60`, `:61`, `:87`, `:89`)
- **Cadastro:** execução `*/5 * * * *`, `--apply`, estágio `continuo`, sem dependências; papel explícito **`lavra`**. Isso prova cadastro, não execução em produção. (`app/config/crons.py:390`, `:391`, `:2264`; execução: [nao verificado])
- **Indeterminado isolado não é stale:** a varredura aceita vazio, julgamento ausente/futuro, relógio vencido ou marco posterior; não testa o valor `indeterminado`. Portanto uma lavra indeterminada com carimbos vigentes não ganha nova tentativa exclusivamente por ser indeterminada. (`app/chamados/services/raiox.py:41`, `:43`, `:45`, `:46`)
- **GET não lavra:** o código registra expressamente a transferência dessa escrita para o cron, aceitando até um ciclo de idade do campo. (`app/chamados/services/raiox.py:112`, `:116`, `:117`, `:119`)
- **Nascimento pode ficar vazio:** campo tem `default=''`; nascimento de uma pergunta nova insere a instância sem chamada explícita a `lavrar` nesse ramo. Reuso com mudança de vínculos, ao contrário, relavra antes do save. (`app/chamados/models.py:2663`, `:2664`, `:3097`, `:3100`, `:3080`, `:3086`, `:3087`, `:3089`)
- **Porta de escrita:** `lavrar` julga, transforma exceção em `VL_INDETERMINADO`, atribui os três campos e salva. (`app/chamados/veredito_lavrado.py:254`, `:261`, `:265`, `:269`, `:270`, `:272`, `:273`, `:275`)
- **Evento de materialização:** materializador chama `p.carimbar(...lavrar_veredito=True)`; a porta executa `lavrar(salvar=False)` e grava o carimbo e a lavra no mesmo save. (`app/chamados/services/materializacao.py:1283`, `:1284`; `app/chamados/models.py:3232`, `:3233`, `:3234`, `:3235`)
- **Signal de fechamento do chamado:** S143 reage ao save de status encerrado e chama o resolvedor; este seleciona destino correspondente, materialização ausente e disputa aberta, depois carimba com `lavrar_veredito=True`. Há guardas adicionais, inclusive tranca e materialização em voo. (`app/chamados/signals.py:91`, `:97`, `:105`, `:117`; `app/chamados/services/disputa_emissao.py:1908`, `:1913`, `:1915`, `:1916`, `:1967`, `:1993`, `:1994`)
- **Outro signal:** S144 dispara `materializar_pendentes_aprovadas` quando uma disputa existente fecha; é gatilho da porta de materialização. (`app/chamados/signals.py:149`, `:155`, `:164`, `:165`)
- **Mudança do destino:** `_gravar_estado` relavra ao cruzar a fronteira de `ENCERRADOS`, para perguntas apontando ao chamado e sem validação. (`app/chamados/models.py:201`, `:273`, `:303`, `:309`, `:310`, `:312`)
- **Outras portas:** `reapontar_resolvedora` relavra junto da troca do destino; `rechavear_data_evento` relavra junto do dia/célula; `relavra_via.relavrar` relavra quando muda uma via derivada. (`app/chamados/services/fio.py:843`, `:964`, `:965`, `:966`, `:1018`, `:1076`, `:1081`, `:1082`; `app/chamados/services/relavra_via.py:106`, `:118`, `:121`, `:125`, `:126`, `:127`)
- **Batida/célula não significam relavra direta da pergunta:** `_julgar_dias` chama o cartório; o comentário registra que a relavra reativa de via foi retirada desse caminho pelo custo. (`app/ponto/signals.py:85`, `:93`, `:94`, `:102`, `:103`)
- **Backfill é sob ordem, não rotina:** cadastro declara esse papel; comando só inclui mudança do valor do veredito, deixa divergência exclusiva de relógio fora e não substitui lavra preenchida por indeterminado. (`app/config/crons.py:1420`; `app/chamados/management/commands/backfill_veredito_lavrado.py:115`, `:120`, `:122`, `:124`, `:131`, `:138`, `:143`, `:146`)

**Limite do censo:** não se pode atribuir falha de cron, signal ou porta a qualquer um dos 136 dias sem os campos das perguntas e seus destinos; execução e causa individual permanecem [nao verificado].

## 14/11 18:43 · sessao bcac34a25ccd · BO-DISPUTA-SEM-PERGUNTA · censo · origem=Code

Numerei as linhas dos anexos a partir da primeira linha de conteúdo, incluindo linhas vazias.

**Há duas limitações para a classificação:** a última pergunta examina `9bba8132b`, não o hash final; e mencionar uma triagem não comprova que ela descartou um achado. (`preverif6.q:1`; `opus_final_1_reprovado.txt:1`; `CLAUDE.md:384-386`)

Na tabela, **(a)\*** significa tema perguntado, mas achado específico não apontado nas respostas fornecidas. A presença da alteração final no hash anteriormente examinado fica **[nao verificado]**.

| Achado do opus | Classe | Trecho da pergunta | Trecho da resposta e justificativa |
|---|---|---|---|
| **1. Eleitores divergentes, escritor único e selo ausentes** | **Parcialmente visto; (a)\* no confronto específico com S160. (b) [nao verificado].** | `preverif6.q:4`: “Censo de TODO escritor de PerguntaDisputa.chamado_destino”; `preverif6.q:8`: “2o juiz, 2o escritor fora da porta […] derivacao paralela”. | `preverif6.r:1`: “não estabelece a regra nos escritores”; `preverif6.r:27`: “A eleição do chamado continua divergente entre lote e avulso”. Portanto, a incompletude **foi vista**. O confronto final `_destino_do_intervalo` × S160 não foi apontado nesses trechos; hoje os dois estão em `app/chamados/services/fio.py:747` e `app/chamados/services/disputa_emissao.py:167`. Descarte pela triagem: **[nao verificado]**. |
| **2. Ampliação do push diário para disputas abertas sem pergunta** | **(a)\*** para o encadeamento com o alerta diário; **(c)** para quantidade em produção. | `preverif3.q:12-14`: “Push […] e qualquer outro envio”, “em cada caminho”, “push sem pergunta local criada no ato”. | `preverif4.r:31`: “sem ata, permanece aberta”; `preverif4.r:40` identifica push direto “no caso HX sem pergunta”. Não aponta a ampliação pelo leitor diário, cujo filtro seleciona abertas sem exigir pergunta (`app/ponto/management/commands/alertar_disputas_pendentes.py:33-35`). Quantidade atingida: **[nao verificado]**. |
| **3. Protegidos receberam ata e perderam cobertura sem ata** | **(a)\*** quanto à perda de cobertura; integridade histórica **[nao verificado]**. | `preverif3.q:19-22`: “cada teste morde?” e protegidos “seguem intocados e valendo?”; `preverif4.q:22-24`: “seguem intocados?”. | `preverif4.r:95`: “Os asserts protegidos estão presentes”, mas integridade histórica “**[nao verificado]**”. Não aponta o deslocamento da premissa para ata presente. Na cópia final, ambos chamam o helper em `app/chamados/tests/test_hx_disputa_muda.py:45` e `:66`. |
| **4. Três fontes de dia em `_data_fallback`** | **(a)**: código descrito, violação específica não apontada. | `preverif2.q:11-13`: comparar “ramo a ramo: texto, destino, dia”; `preverif2.q:22-24`: “derivacao paralela de fonte unica”. | `preverif2.r:52` enumera “recuperação pela grade; data declarada do motivo; batida pelo turno” como ramos preservados. Reconhece as fontes, mas não as apresenta como o achado do opus. A cadeia permanece em `app/chamados/services/disputa_emissao.py:376`, `:398` e `:413`. |
| **5. Qualquer pergunta, inclusive respondida/morta, habilita push repetido** | **(a)\***. | `preverif3.q:12-14`: push “em cada caminho”, “com quais motivos”; `preverif4.q:13-15`: órfãos com `motivos_atualizados`. | `preverif4.r:40`: envia `motivos_atualizados` “inclusive na repetição do mesmo ato”. Não aponta a diferença entre existência de linha e pergunta viva. A guarda final exige somente `.exists()` em `app/chamados/services/disputa_emissao.py:1553`. |
| **6. Push no atomic; âncora/recobrança sem guarda; mensagem falsa** | **Visto parcialmente; (b) [nao verificado] para o atomic. (a)\* nos demais subachados.** | `preverif4.q:13-18`: push em cada caminho e efeitos externos escapando de rollback; `preverif3.q:12`: “qualquer outro envio”. | `preverif6.r:23`: “complementos e principal notificam dentro do atomic externo”; `preverif4.r:51`: envio imediato pode anteceder rollback. Esse risco **foi visto**. `preverif4.r:43` descreve o envio da âncora, sem apontar sua falta de condição. Os trechos não apontam recobrança com zero pendentes nem a mensagem baseada em `len(orfaos)` (`app/chamados/services/disputa_emissao.py:1976`, `:1992`; `app/chamados/services/acoes_disputa.py:232`). |
| **7. Agregação restrita a motivos com pergunta local; leitores não verificados** | **Restrição vista; (a) para a lacuna de leitores. (b) [nao verificado].** | `preverif3.q:24-25` e `preverif4.q:25-26`: “QUEM ESCREVE e QUEM LE” `DS.motivos`, incluindo significado divergente. | `preverif4.r:21`: “os motivos admitidos em A vêm das perguntas locais”. Portanto, a restrição **foi descrita**. O achado final é também a ausência de verificação dos leitores, apesar da pergunta explícita; a restrição está em `app/chamados/services/disputa_emissao.py:679-688`. Descarte desse risco pela triagem: **[nao verificado]**. |
| **8. Trilha engolida/I23 cego e pendentes fora do núcleo** | **Visto. Classe (b) [nao verificado] por faltar comprovação do descarte.** | `preverif5.q:4`: “Algum caminho […] sai sem trilha?”; `preverif6.q:3`: estado de cada achado anterior. | `preverif6.r:16`: “gravação pós-commit pode falhar”; `preverif6.r:21`: pendentes “após o retorno”. Os dois componentes **foram apontados**. O RELATO registra a triagem como encaminhada, sem apresentar seu resultado nessa linha (`app/docs/RELATO.md:858`). |
| **9. ERROR no RED c22 e mordida c24 fora do diff** | **(c)**. | `preverif6.q:8`: pergunta pelos “mutantes da entrega”; essa entrega é a c21 (`preverif6.q:1`). | `preverif6.r:19` trata dos mutantes anteriores; `preverif6.r:32` mantém suite e protegido ORM como não verificados. Não certifica RED c22 nem mutações c24. O ERROR e a ressalva sobre c24 constam do parecer final (`opus_final_1_reprovado.txt:10`); causa por fixture: **[nao verificado]**. | [nao verificado]

**Não há descarte de triagem comprovado para fechar (b), nem evidência suficiente para atribuir (d) MODO-COMUM.** A regra exige as linhas de triagem e define modo comum como omissão de ambos os leitores; esses elementos não estão demonstrados para os achados acima. (`CLAUDE.md:384-387`; `CLAUDE.md:393`; **[nao verificado]**)

## 14/11 18:42 · sessao 988e591b1cf0 · O50-DESLIGAR-ENCERRA-TUDO · verificador · origem=Code

**23 CONFERE, 2 DIVERGE, 0 SUMIU**, conforme as linhas abaixo. **F05 e F06 caem como provas do caminho RET→TXT/vigia**: a saída por rescisão ocorre antes da retenção. Os campos crus continuam presentes. (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`, `app/folha/export.py:1241`, `app/folha/export.py:1269`)

F01 aparece uma vez na tabela, pois foi repetido na amostra fornecida. A identidade Git do HEAD informado é **[não verificado]**.

Nos contraexemplos, **S** significa situação atual discordante de vínculo válido; **A**, EC inativa com vigência cobrindo o dia ou ativa com início futuro; **B**, igualdade entre dia, demissão e fim do vínculo. A autoridade aceita a igualdade e vínculos inativos datados, sem consultar situação ou demissão do colaborador. São possibilidades de código; ocorrência em produção é **[não verificado]**. (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`)

| ID | Arquivo:linha na cópia | Cru lido? | Predicado confere? | Alcance folha/fio e caminho comprovado | CE procede? | Veredito |
|---|---|---|---|---|---|---|
| **F01** | `app/folha/export.py:60` | **Sim**, D | **Sim:** D não nula e D≤fim. | **Folha sim.** `gerar_dominio_txt` (`app/folha/views.py:212`) → `classificar_export` (`app/folha/export.py:866`) → `sai_pela_rescisao` (`app/folha/export.py:60`). Retorna `fora/rescisao` e continua (`app/folha/export.py:867`, `app/folha/export.py:871`); a montagem recebe esses itens (`app/folha/views.py:268`) e exclui quem não está em `entra` (`app/folha/export.py:716`, `app/folha/export.py:726`). | **Sim**, como divergência entre rota financeira e vínculo válido, conforme autoridade acima. | **CONFERE** |
| **F02** | `app/folha/export.py:807`; `app/folha/export.py:810` | **Sim**, A+S | **Sim:** A=True, 12x36, âncora nula, S=ativo, participante dos fechamentos (`app/folha/export.py:808`, `app/folha/export.py:809`, `app/folha/export.py:811`). | **Folha sim.** `aprovar_fechamento` → `classificar_export` (`app/ponto/views.py:1299`) → seleção crua (`app/folha/export.py:806`). Produz `sem_ancora_12x36` e `travas_aprovacao` (`app/folha/export.py:835`, `app/folha/export.py:848`); o consumidor lê a trava, exclui os PKs e aprova somente os restantes (`app/ponto/views.py:1301`, `app/ponto/views.py:1311`, `app/ponto/views.py:1315`, `app/ponto/views.py:1365`). | **Sim**, S/A, conforme autoridade acima. | **CONFERE** |
| **F05** | `app/folha/export.py:1241`, agora em `_dias_sem_decisao` | **Sim**, D | **Sim localmente:** corta fim até D inclusive; janela colapsada retorna vazio (`app/folha/export.py:1242`, `app/folha/export.py:1243`, `app/folha/export.py:1246`). **Há guarda anterior nova:** desligado não chega ao helper pelo retentor (`app/folha/export.py:1207`, `app/folha/export.py:1212`). | **Não alcança TXT/vigia pelo caminho alegado.** Corte efetivo exige D<fim; F01 já dá `continue` antes do retentor (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`). `dias_que_seguram` chama `_dias_que_retem` somente para classificados `furo_espelho` (`app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`). Persiste diagnóstico: `recalcular_fechamento_mes` → `motivos_retencao_espelho` (`app/ponto/services/fechamento.py:1067`) → `motivos_retencao_celula` (`app/folha/export.py:1307`) → helper (`app/folha/export.py:1212`), gravado em `motivos_espelho` (`app/ponto/services/fechamento.py:1186`). | **Sim localmente**, para S≠desligado; **não demonstra a trava alegada**. Para desligado, o helper serve ao informativo (`app/folha/export.py:1226`, `app/folha/export.py:1228`). | **DIVERGE — alcance RET/TXT/vigia e guarda anterior.** |
| **F06** | `app/folha/export.py:1269`; `app/folha/export.py:1271` | **Sim**, D | **Sim localmente:** célula ausente só gera `sem_celula` com d<hoje e d≠D; também há exceção para célula sem veredito (`app/folha/export.py:1272`, `app/folha/export.py:1295`). Mesma guarda anterior de desligado (`app/folha/export.py:1207`). | **Não alcança TXT/vigia pelo caminho alegado.** A igualdade precisa ocorrer no laço d≤fim (`app/folha/export.py:1265`, `app/folha/export.py:1269`), portanto F01 já excluiu antes da retenção (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`). Restam diagnóstico de fechamento e leitura individual: `o_que_segura` → `_dias_que_retem` → retentor (`app/folha/fora_da_folha.py:224`, `app/folha/fora_da_folha.py:98`), seguida de leitura do fio (`app/folha/fora_da_folha.py:228`). | **Sim localmente**, B com S≠desligado; **não como prova de trava TXT/vigia**, pelo bloqueio anterior citado. | **DIVERGE — alcance RET/TXT/vigia e guarda anterior.** |
| **P34** | `app/ponto/services/fechamento.py:432`; `app/ponto/services/fechamento.py:438`; `app/ponto/services/fechamento.py:440` | **Sim**, S+D | **Sim:** S≠desligado OU desligado com D≥início da janela da empresa; sem teto superior (`app/ponto/services/fechamento.py:441`). | **Folha sim.** `recalcular_fechamento_mes` → `universo_da_competencia` (`app/ponto/services/fechamento.py:554`) → queryset retornado (`app/ponto/services/fechamento.py:445`) → laço (`app/ponto/services/fechamento.py:619`) → gravação do fechamento (`app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1160`). | **Sim:** desligado com D nula fica fora mesmo com EC válida, pelo predicado citado e autoridade acima. | **CONFERE** |
| **C35** | `app/escala/services/leitor_celula.py:365`; `app/escala/services/leitor_celula.py:389` | **Sim**, D | **Sim:** d>D OU d=D sem fato; também ramo anterior à admissão. Fato = minutos realizados OU lâmpadas não missing (`app/escala/services/leitor_celula.py:385`, `app/escala/services/leitor_celula.py:386`). | **Folha e fio sim.** Folha: `recalcular_fechamento_mes` → `grade_do_fechamento` (`app/ponto/services/fechamento.py:1014`) → `grade_da_celula` (`app/folha/export.py:338`) → `fora_vinculo` (`app/escala/services/leitor_celula.py:393`), excluído dos dias internos e trabalho/ausência (`app/ponto/services/fechamento.py:1043`, `app/ponto/services/fechamento.py:1046`), alterando previsto (`app/ponto/services/fechamento.py:1083`). Fio: `gerar_perguntas_disputa` → `_data_furo_da_grade` (`app/chamados/services/disputa_emissao.py:350`) → grade (`app/chamados/services/disputa_emissao.py:83`); fora de trabalho retorna None (`app/chamados/services/disputa_emissao.py:85`, `app/chamados/services/disputa_emissao.py:87`), podendo causar `continue` antes de abrir pergunta (`app/chamados/services/disputa_emissao.py:366`, `app/chamados/services/disputa_emissao.py:444`). | **Sim**, B, conforme autoridade inclusiva acima. | **CONFERE** |
| **P39** | `app/ponto/services/he_pendente_lavrado.py:92` | **Sim**, S | **Sim:** universo da empresa com S=ativo; produz retrato (`app/ponto/services/he_pendente_lavrado.py:160`, `app/ponto/services/he_pendente_lavrado.py:169`). | **Folha sim, condicional.** `lavrar_he_pendente.handle` injeta revogador e chama `apurar` (`app/ponto/management/commands/lavrar_he_pendente.py:56`, `app/ponto/management/commands/lavrar_he_pendente.py:62`) → callback por dia (`app/ponto/services/he_pendente_lavrado.py:143`) → `revogar_recusa_do_sistema` → marcação revogada (`app/ponto/portas/he.py:472`, `app/ponto/portas/he.py:375`) → decisões vigentes em `estado_por_dia` (`app/ponto/services/gestao_he.py:86`) → `medir` (`app/folha/porta_export.py:684`, `app/folha/porta_export.py:686`) → trava condicional e recusa (`app/folha/porta_export.py:230`, `app/folha/porta_export.py:853`). A recusa automática de ponta pequena estável, isoladamente, não comprova trava: ela rejeita acima do limite, faixa que o portão verifica (`app/ponto/portas/he.py:313`, `app/folha/porta_export.py:230`). | **Sim**, S pode omitir dia e impedir revogação pelo callback citado. | **CONFERE — alcance condicional.** |
| **P44** | `app/ponto/services/vigia_de_hora.py:117` | **Sim**, S | **Sim:** pula desligado; também pula isento (`app/ponto/services/vigia_de_hora.py:119`). | **Fio sim.** `rodar` → `julgar_celula` (`app/ponto/services/vigia_de_hora.py:131`) → `julgar_colab` (`app/ponto/services/cartorio.py:2040`) → emissão sob aplicação e demais guardas (`app/ponto/services/cartorio.py:1791`, `app/ponto/services/cartorio.py:1829`) → `ChamadoColaborador.abrir` (`app/ponto/management/commands/detectar_ausencias.py:694`). | **Sim**, S; inclui candidatos de hoje/ontem (`app/ponto/services/vigia_de_hora.py:114`, `app/ponto/services/vigia_de_hora.py:115`). | **CONFERE** |
| **P66** | `app/chamados/services/pergunta_no_app.py:186` | **Sim**, S | **Sim:** chamados em cobrança, módulos selecionados e S=ativo (`app/chamados/services/pergunta_no_app.py:185`). | **Fio sim, sob apply.** `Command.handle` → `medir` (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:21`) → achados (`app/chamados/services/pergunta_no_app.py:201`) → `perguntar` (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`) → `criar_disputa_a_partir_de_chamados_orfaos` (`app/chamados/services/pergunta_no_app.py:246`) → `_complementar_marcos_faltantes` (`app/chamados/services/disputa_emissao.py:1175`) → `_criar_pergunta_se_couber` (`app/chamados/services/disputa_emissao.py:881`) → `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:837`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **P65** | `app/chamados/services/limbo_folgas.py:65` | **Sim**, A | **Sim:** conta vínculos A=True do template. | **Não alcança decisão de folha/fio.** `aplicar_sugestao` → `texto_seco` → `reescrever_texto` (`app/chamados/services/limbo_folgas.py:70`); a contagem forma apenas sufixo textual (`app/chamados/services/limbo_folgas.py:51`). Aceitação usa outros dados (`app/chamados/services/limbo_folgas.py:82`, `app/chamados/services/limbo_folgas.py:85`). | **Sim**, A para o universo contado; efeito é textual, como citado. | **CONFERE** |
| **P22** | `app/ponto/selecao_periodo.py:278` | **Sim**, A | **Sim:** jornada de EC ativa; fallback 24h (`app/ponto/selecao_periodo.py:287`). | **Não alcança no fluxo indicado.** `turnos_em_turno_stored` retorna mapa (`app/ponto/selecao_periodo.py:293`) → `colabs_em_turno_stored` (`app/ponto/selecao_periodo.py:253`) → dashboard usa tamanho (`app/core/views.py:1160`, `app/core/views.py:1161`). | **Sim**, A, conforme autoridade acima. | **CONFERE** |
| **C64** | `app/ferias/management/commands/alertar_retornos.py:30` | **Sim**, S | **Sim:** S=ativo, hoje folga e amanhã trabalha (`app/ferias/management/commands/alertar_retornos.py:34`). | **Não alcança folha/fio decisório.** `handle` monta retornos (`app/ferias/management/commands/alertar_retornos.py:35`) → `push_dp`, aviso de disponibilidade (`app/ferias/management/commands/alertar_retornos.py:45`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **T13** | `app/colaboradores/views.py:619` | **Sim**, S | **Sim:** S=ativo, em outro posto. | **Não alcança.** `buscar_colabs_posto` termina em JSON limitado a 30 (`app/colaboradores/views.py:622`, `app/colaboradores/views.py:626`); o POST resolve o colaborador separadamente (`app/colaboradores/views.py:640`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **T26** | `app/colaboradores/views.py:2900` | **Sim**, S | **Sim:** após juiz de férias, exclui S=desligado. | **Não alcança.** `disponibilidade` acrescenta em `parados` (`app/colaboradores/views.py:2905`) e renderiza painel (`app/colaboradores/views.py:2933`). | **Sim**, S, havendo férias cobrindo hoje no fluxo citado. | **CONFERE** |
| **T39** | `app/colaboradores/views.py:48` | **Sim**, A | **Sim:** prefetch somente A=True. | **Não alcança.** `vincular_colaboradores` usa o prefetch para `sem_escala` (`app/colaboradores/views.py:71`, `app/colaboradores/views.py:73`), filtros (`app/colaboradores/views.py:97`) e render (`app/colaboradores/views.py:126`). | **Sim**, A, conforme autoridade acima. | **CONFERE** |
| **T52** | `app/colaboradores/views_fase.py:45` | **Sim**, A | **Sim:** última A=True por início. | **Não alcança.** `fase_12x36` usa template/âncora (`app/colaboradores/views_fase.py:47`, `app/colaboradores/views_fase.py:49`), passa EC aos previews (`app/colaboradores/views_fase.py:98`) e renderiza (`app/colaboradores/views_fase.py:115`). | **Sim**, A, conforme autoridade acima. | **CONFERE** |
| **P51** | `app/ponto/worklist.py:49` | **Sim**, A | **Sim:** primeira A=True fornece posto. | **Não alcança.** `coletar_gaps_dp` inclui posto no resultado (`app/ponto/worklist.py:50`, `app/ponto/worklist.py:66`) → `painel_inconsistencias_posto` agrupa e renderiza (`app/colaboradores/views.py:3025`, `app/colaboradores/views.py:3029`). | **Sim**, A para agrupamento, conforme autoridade acima. | **CONFERE** |
| **C11** | `app/escala/views_plano_folgas.py:176` | **Sim**, A | **Sim:** exige A=True; ausência gera erro `vinculo` e pula item (`app/escala/views_plano_folgas.py:178`, `app/escala/views_plano_folgas.py:179`). | **Folha e fio sim.** `plano_folgas_aplicar` → `aplicar_folgas_em_lote` (`app/escala/views_plano_folgas.py:185`) → `relavrar_celulas_da_foto` (`app/escala/folgas.py:418`) → `regenerar_celulas_vinculo` (`app/escala/folgas.py:293`) → `_o_chao_do_dia_mudou` (`app/ponto/portas/celula.py:392`) → julgamento (`app/ponto/portas/celula.py:472`), recálculo (`app/ponto/portas/celula.py:496`) e reavaliação/rejunção (`app/ponto/portas/celula.py:536`, `app/ponto/portas/celula.py:539`). | **Sim**, A, para vínculo inativo cobrindo dia editável. | **CONFERE** |
| **C26** | `app/escala/services/cadastro_realidade.py:575` | **Sim**, S | **Sim:** exclui S=desligado. | **Não alcança decisão de folha/fio por esse corte.** `_afastado_sem_registro` produz C4 sem data (`app/escala/services/cadastro_realidade.py:600`) → `lista` (`app/escala/services/cadastro_realidade.py:794`). Existe consumo estrutural pelo vigia: `ListaDoAdmin._carregar` (`app/chamados/services/vigia_fio.py:216`, `app/chamados/services/vigia_fio.py:218`) → `colab_na_lista` (`app/chamados/services/vigia_fio.py:251`) → causa d/e (`app/chamados/services/vigia_fio.py:682`). Porém desligados já ficam sem dias nesse universo pelo retentor (`app/chamados/services/vigia_fio.py:586`, `app/folha/export.py:1207`); C4 sem data também não entra no mapa de dias (`app/escala/services/cadastro_realidade.py:1033`). | **Sim**, S quanto à lista C4; não comprova alteração da trava/vigia, pelos bloqueios citados. | **CONFERE** |
| **C40** | `app/ponto/management/commands/reconciliar_grade.py:106` | **Sim**, S | **Sim:** S=ativo e universo de batidas/chamados/operação (`app/ponto/management/commands/reconciliar_grade.py:84`, `app/ponto/management/commands/reconciliar_grade.py:95`, `app/ponto/management/commands/reconciliar_grade.py:105`). | **Fio sim sob apply.** `handle` → `_emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:250`, `app/ponto/management/commands/reconciliar_grade.py:254`) → `ChamadoColaborador.abrir` ou `reabrir` (`app/ponto/management/commands/reconciliar_grade.py:507`, `app/ponto/management/commands/reconciliar_grade.py:530`); outro ramo chama `retratar` (`app/ponto/management/commands/reconciliar_grade.py:309`). Cron está sem apply (`app/config/crons.py:462`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |
| **C53** | `app/ponto/management/commands/reconciliar_fantasmas.py:174` | **Sim**, A | **Sim:** primeira A=True fornece diagnóstico. | **Não decide a transição.** EC → `_completude` → `detalhe` (`app/ponto/management/commands/reconciliar_fantasmas.py:205`, `app/ponto/management/commands/reconciliar_fantasmas.py:206`). Decisão usa núcleo e premissa separadamente (`app/ponto/management/commands/reconciliar_fantasmas.py:248`, `app/ponto/management/commands/reconciliar_fantasmas.py:256`); detalhe entra apenas no motivo passado à retratação (`app/ponto/management/commands/reconciliar_fantasmas.py:278`, `app/ponto/management/commands/reconciliar_fantasmas.py:280`). | **Sim**, A no diagnóstico, conforme autoridade acima. | **CONFERE** |
| **C70** | `app/chamados/management/commands/auditar_invariantes_chamados.py:1476` | **Sim**, S | **Sim:** S≠ativo é pulado em A4. | **Não alcança emissão/reabertura.** O corpo alimenta resultado A4 (`app/chamados/management/commands/auditar_invariantes_chamados.py:1485`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1505`), stdout e HTML opcional (`app/chamados/management/commands/auditar_invariantes_chamados.py:1841`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1853`). | **Sim**, S no universo auditado, conforme autoridade acima. | **CONFERE** |
| **A24** | `app/relatorios/furos.py:117` | **Sim**, S | **Sim:** S∈{ativo,afastado,ferias}. | **Não alcança.** `dataset_furos` monta e retorna cards (`app/relatorios/furos.py:142`, `app/relatorios/furos.py:167`) → view (`app/relatorios/views.py:1541`) → PDF ou render (`app/relatorios/views.py:1562`, `app/relatorios/views.py:1566`). | **Sim**, S em consulta histórica, conforme autoridade acima. | **CONFERE** |
| **A37** | `app/inteligencia/calculadores.py:139` | **Sim**, S | **Sim:** S∈{ativo,afastado,ferias}. | **Não alcança.** `preenchimento_marcos` calcula e retorna linhas (`app/inteligencia/calculadores.py:162`, `app/inteligencia/calculadores.py:177`) → `metricas.calcular` (`app/inteligencia/metricas.py:42`) → grava `MetricaSnapshot` (`app/inteligencia/metricas.py:47`). | **Sim**, S em janela histórica, conforme autoridade acima. | **CONFERE** |
| **A50** | `app/inteligencia/views.py:209` | **Sim**, S | **Sim:** S∈{ativo,ferias,afastado}, com empresa opcional. | **Não alcança.** A base de `api_kpi` alimenta ramos de JSON: ativos, sem escala, sem ausência, aniversariantes e disponíveis (`app/inteligencia/views.py:222`, `app/inteligencia/views.py:228`, `app/inteligencia/views.py:235`, `app/inteligencia/views.py:263`, `app/inteligencia/views.py:295`). | **Sim**, S, conforme autoridade acima. | **CONFERE** |

**Consolidação das 25 linhas: 23 conferem, 2 divergem, 0 sumiram.** Dos seis “folha diretamente”, **F05 e F06 caem na fundamentação RET→TXT/vigia**, enquanto **F01, F02, P34 e C35 mantêm os caminhos diretos documentados acima**. A consequência exata sobre antecipação/prazo depende do aval não fornecido: **[não verificado]**.

## 14/11 18:42 · sessao 89d24e9eeb22 · GRADE-RESOLVE · censo · origem=Code

**O PASSO 0 fica incompleto:** há **18 casos individualizados com dono ESTRUTURA escrito**, mas faltam os IDs de 63 unidades da ingestão e o rol autônomo das atas do Fernando. Portanto, **18 é o subtotal documental recuperável; o total integral para o cruzamento é [nao verificado]**. (app/docs/RELATO.md:8390, :8398; app/docs/BACKLOG.md:1102; app/docs/RELATO.md:52)

**(A) Fontes, unidades e contagens**

| Fonte | Onde mora o registro | O que contei como um caso | Contagem |
|---|---|---|---:|
| GRADE-MESTRA — SEGUE | app/docs/GRADE-MESTRA.md:33 até :522 | Uma linha de dados das sete tabelas por balde; excluí cabeçalhos e separadores | **456 obras** |
| GRADE-MESTRA — complementos | app/docs/GRADE-MESTRA.md:525 até :1551 | Uma linha de dados das tabelas `complemento`; excluí cabeçalhos e separadores | **548 complementos**, sob **119 mães** |
| Linhas HJ- do BACKLOG | app/docs/BACKLOG.md:734, :744–749, :784, :786, :1868, :1917, :2547–2555 | Um identificador-raiz HJ-NNN representado nas linhas; derivados agrupados na raiz | **11 raízes**; quantidade de casos pessoa+dia: **[nao verificado]** |
| Fernando — ingestão localizada | app/docs/BACKLOG.md:1868; app/docs/RELATO.md:8368, :8386 | Uma unidade pessoa+dia HJ-NNN-Uk | **80 unidades declaradas; 17 IDs recuperáveis** |
| Atas do Fernando — coleção autônoma | **NAO ACHADA**; há remissão a ATA-1111 em app/docs/PROMPTS.md:2125 | Rol original necessário para estabelecer a unidade | **[nao verificado]** |
| BO-0611 | app/docs/BACKLOG.md:1911–1912; app/docs/RELATO.md:1471, :1533–1534 | Um item numerado 01–14; 11 e 12 permanecem separados | **14** |
| BO-1311-ROTEIRO | Nome encontrado: BO-[nome]-1311-ROTEIRO, app/docs/BACKLOG.md:783; app/docs/RELATO.md:222, :226 | Uma ficha 01–10; perguntas vivas são subunidades | **10** |
| BO2910-ESTRUTURA | app/docs/BACKLOG.md:1102 | Uma das classes explicitamente enumeradas: 01, 02, 05, 06, 08 | **5** |
| BO-ADMIN-2710 | app/docs/CORTES.md:330, :332; app/docs/RELATO.md:3421–3429 | Um item 1–8, mais os complementos 8.1, 8.2 e 8.3 | **11**, confirmado em app/docs/RELATO.md:52 |

A recontagem da grade deu **75+5+155+177+39+1+4 = 456**. Os **548 complementos** foram contados diretamente nas linhas de dados; a soma dos números dos 119 subtítulos também deu 548. Os dois números do aval conferem. (app/docs/GRADE-MESTRA.md:15–22, :525–1551)

Nas linhas HJ-, encontrei **20 linhas: dez IDs HJ-NNN diretos, nove derivados e um instrumento de ingestão**. Os derivados representam HJ-091, HJ-075 e HJ-058; o TSV agrupa-os em suas raízes para evitar contar achados de opus e complementos como novos BOs originais. A granularidade original desses casos permanece **[nao verificado]**. (app/docs/BACKLOG.md:734, :744–749, :784, :786, :1868, :1917, :2547–2555)

O mapa integral da ingestão é remetido a `logs/hj_0511/mapa.md`, **NAO ACHADO nesta cópia**. A publicação permite recuperar 17 IDs dos 80; os outros **63 IDs ficam [nao verificado]**. O vínculo entre essa ingestão e a totalidade das “atas do Fernando” também é **[nao verificado]**. (app/docs/RELATO.md:8386, :8390–8398)

No BO-0611, o aval V2 **substitui o quadro anterior**. Por isso, BATIDA para 01/04 e CADASTRO para 06, escritos no quadro antigo, não foram transportados como donos vigentes; a V2 escreve CADASTRO para 11/12. (app/docs/PROMPTS.md:1925; app/docs/RELATO.md:1533, :1471)

As buscas abrangeram a árvore inteira, incluindo arquivos ocultos/ignorados e os documentos RELATO-ARQUIVO, LAPIDES, DOSSIES, CORTES e TICKETS. Os comandos executados seguem abaixo; a execução e a ausência física dos arquivos não possuem referência arquivo:linha. **[nao verificado]**

```sh
rg -n --hidden --no-ignore -i 'BO[-_]?0611|BO[-_]?1311[-_]?ROTEIRO|BO2910[-_]?ESTRUTURA|BO[-_]?ADMIN[-_]?2710' .
rg -n --hidden --no-ignore -i 'bo[-_]mauricio[-_]0611|bo[-_]mauricio[-_]1311|bo2910|bo2710' .
rg -n --hidden --no-ignore -i 'atas.{0,60}fernando|fernando.{0,60}atas|ata.{0,40}ingerid|ingerid.{0,40}ata' .
rg -n --hidden --no-ignore -i 'fernando|ingestao|ingestão|ingeridas|ingeridos|ATA-1111' .
rg -n --hidden --no-ignore 'HJ-' .
rg -n --hidden --no-ignore 'HJ-[0-9]{3}-U[0-9]+' .
rg --files --hidden --no-ignore | rg -i '0611|1311|2910|2710|fernando|ata|relato|lapides|dossies|cortes|tickets'
git grep -n -I -E 'BO[-_]?0611|BO[-_]?1311[-_]?ROTEIRO|BO2910-ESTRUTURA|BO-ADMIN-2710|Fernando|HJ-'
```

O `git grep` falhou porque a cópia não contém metadados Git; a identidade HEAD informada no pedido não pôde ser conferida. As buscas equivalentes usaram `rg`. **[nao verificado]**

Os testes encontrados com BO-0611 no nome documentam o caso 05 e cenários sintéticos; não fornecem o rol original dos 14 itens. (app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:1, :13; app/ponto/tests/test_bo0611_05_vinculo_geo_parcial_django.py:9)

**(B) TSV recuperável — 68 linhas**

Os donos da ingestão abaixo preservam a classificação escrita em **05/11**. A publicação posterior registra cinco mudanças de estado, porém marca a própria remedida como **[nao verificado]**; não fornece novo dono. Assim, os seis ESTRUTURA históricos não comprovam seis divergências atuais. (app/docs/RELATO.md:8390–8394, :52)

```tsv
fonte	id_do_caso	arquivo:linha	dono_L099	prova_do_dono
BACKLOG-HJ	HJ-058	app/docs/BACKLOG.md:2554	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-066	app/docs/BACKLOG.md:2547	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-068	app/docs/BACKLOG.md:2548	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-071	app/docs/BACKLOG.md:2549	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-073	app/docs/BACKLOG.md:2550	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-074	app/docs/BACKLOG.md:2551	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-075	app/docs/BACKLOG.md:2552	SEM_DONO	[nao verificado] dono L-099 fechado
BACKLOG-HJ	HJ-090	app/docs/BACKLOG.md:1917	SEM_DONO	[nao verificado] dono único fechado; linha pede classificação dos dias
BACKLOG-HJ	HJ-091	app/docs/BACKLOG.md:1917	SEM_DONO	[nao verificado] dono único fechado; derivados em BACKLOG:734 e :744-749
BACKLOG-HJ	HJ-092	app/docs/BACKLOG.md:784	BATIDA	app/docs/BACKLOG.md:784: FECHADA como BATIDA; confirmado em app/docs/RELATO.md:271
BACKLOG-HJ	HJ-093	app/docs/BACKLOG.md:786	SEM_DONO	[nao verificado] dono único fechado para o caso composto
HJ-INGESTAO-0511	HJ-001-U1	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391: divergente com dono CADASTRO
HJ-INGESTAO-0511	HJ-001-U2	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391: divergente com dono CADASTRO
HJ-INGESTAO-0511	HJ-002-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392: divergente com dono BATIDA; mudança de estado ressalvada em :52
HJ-INGESTAO-0511	HJ-003-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392: divergente com dono BATIDA
HJ-INGESTAO-0511	HJ-005-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA; mudança de estado ressalvada em :52
HJ-INGESTAO-0511	HJ-005-U2	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA; mudança de estado ressalvada em :52
HJ-INGESTAO-0511	HJ-005-U4	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA; mudança de estado ressalvada em :52
HJ-INGESTAO-0511	HJ-005-U5	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392: divergente com dono BATIDA; mudança de estado ressalvada em :52
HJ-INGESTAO-0511	HJ-012-U1	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394: indeterminado; dono [nao verificado]
HJ-INGESTAO-0511	HJ-012-U2	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-015-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392: divergente com dono BATIDA
HJ-INGESTAO-0511	HJ-016-U1	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391: divergente com dono CADASTRO
HJ-INGESTAO-0511	HJ-017-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-027-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392: divergente com dono BATIDA
HJ-INGESTAO-0511	HJ-030-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390: divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-031-U1	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394: indeterminado; dono [nao verificado]
HJ-INGESTAO-0511	HJ-050-U2	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394: indeterminado; dono [nao verificado]
BO-0611	01	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro substituído por app/docs/PROMPTS.md:1925
BO-0611	02	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] dono fechado; queixa não identificada
BO-0611	03	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono fechado
BO-0611	04	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro substituído por app/docs/PROMPTS.md:1925
BO-0611	05	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono fechado
BO-0611	06	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro substituído por app/docs/PROMPTS.md:1925
BO-0611	07	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono fechado
BO-0611	08	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] dono fechado; registro diz sem divergência
BO-0611	09	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono fechado
BO-0611	10	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono fechado
BO-0611	11	app/docs/RELATO.md:1471	CADASTRO	app/docs/RELATO.md:1471: 11/12 col921 CADASTRO -> lista do admin
BO-0611	12	app/docs/RELATO.md:1471	CADASTRO	app/docs/RELATO.md:1471: 11/12 col921 CADASTRO -> lista do admin
BO-0611	13	app/docs/RELATO.md:1534	SEM_DONO	[nao verificado] frase vigente de dono; item preservado em app/docs/BACKLOG.md:2025
BO-0611	14	app/docs/RELATO.md:1534	SEM_DONO	[nao verificado] frase vigente de dono; item preservado em app/docs/BACKLOG.md:2025
BO-1311-ROTEIRO	01	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	02	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	03	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	04	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	05	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	06	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	07	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	08	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	09	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO-1311-ROTEIRO	10	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] dono por ficha; numeração contestável em app/docs/RELATO.md:226
BO2910-ESTRUTURA	01	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA, cada uma como linha, item 01
BO2910-ESTRUTURA	02	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA, cada uma como linha, item 02
BO2910-ESTRUTURA	05	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA, cada uma como linha, item 05
BO2910-ESTRUTURA	06	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA, cada uma como linha, item 06
BO2910-ESTRUTURA	08	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA, cada uma como linha, item 08
BO-ADMIN-2710	1	app/docs/RELATO.md:3422	ESTRUTURA	app/docs/RELATO.md:3422: Dono ESTRUTURA
BO-ADMIN-2710	2	app/docs/RELATO.md:3423	ESTRUTURA	app/docs/CORTES.md:334: ESTRUTURA dentro do item 2
BO-ADMIN-2710	3	app/docs/RELATO.md:3424	ESTRUTURA	app/docs/RELATO.md:3424: ESTRUTURA
BO-ADMIN-2710	4	app/docs/RELATO.md:3425	CADASTRO	app/docs/RELATO.md:3425: CADASTRO; confirmado em app/docs/CORTES.md:337
BO-ADMIN-2710	5	app/docs/RELATO.md:3426	ESTRUTURA	app/docs/RELATO.md:3426: Dono ESTRUTURA
BO-ADMIN-2710	6	app/docs/RELATO.md:3427	ESTRUTURA	app/docs/RELATO.md:3427: Dono ESTRUTURA
BO-ADMIN-2710	7	app/docs/RELATO.md:3428	ESTRUTURA	app/docs/RELATO.md:3428: Dono ESTRUTURA
BO-ADMIN-2710	8	app/docs/RELATO.md:3429	CADASTRO	app/docs/RELATO.md:3429: CADASTRO, ativa=False sem data_fim
BO-ADMIN-2710	8.1	app/docs/RELATO.md:3429	ESTRUTURA	app/docs/RELATO.md:3429: item 8.1 Dono ESTRUTURA
BO-ADMIN-2710	8.2	app/docs/RELATO.md:3429	SEM_DONO	app/docs/RELATO.md:52: não fechado, 8.2 e 8.3
BO-ADMIN-2710	8.3	app/docs/RELATO.md:3429	SEM_DONO	app/docs/RELATO.md:52: não fechado, 8.2 e 8.3
```

**(C) Totais por fonte × dono**

Contagem das **68 linhas efetivamente individualizadas no TSV**, conforme as provas indicadas em cada linha:

| Fonte | ESTRUTURA | CADASTRO | BATIDA | DECISAO_ADMIN | SEM_DONO | Total | Prova |
|---|---:|---:|---:|---:|---:|---:|---|
| BACKLOG-HJ, por raiz | 0 | 0 | 1 | 0 | 10 | 11 | app/docs/BACKLOG.md:784, :1917, :2547–2555; demais registros acima |
| HJ-INGESTAO, IDs recuperáveis | 6 | 3 | 5 | 0 | 3 | 17 | app/docs/RELATO.md:8390–8394 |
| BO-0611 | 0 | 2 | 0 | 0 | 12 | 14 | app/docs/BACKLOG.md:1911–1912; app/docs/RELATO.md:1471 |
| BO-1311-ROTEIRO | 0 | 0 | 0 | 0 | 10 | 10 | app/docs/BACKLOG.md:783; dono por ficha [nao verificado] |
| BO2910-ESTRUTURA | 5 | 0 | 0 | 0 | 0 | 5 | app/docs/BACKLOG.md:1102 |
| BO-ADMIN-2710 | 7 | 2 | 0 | 0 | 2 | 11 | app/docs/RELATO.md:52, :3422–3429 |
| **Subtotal individualizado** | **18** | **7** | **6** | **0** | **37** | **68** | Soma das linhas acima |

Para a **ingestão inteira declarada**, a publicação histórica dá **80 = 6 ESTRUTURA + 3 CADASTRO + 5 BATIDA + 66 sem dono publicado**. Esses 66 são os 33 “sem divergência hoje” e 33 “indeterminados”; somente três têm ID exposto. As 63 unidades restantes não foram inventadas no TSV. (app/docs/RELATO.md:8390–8395, :8398)

A lista individualizada **“falta triagem” contém os 37 SEM_DONO do TSV**. Na ingestão há ainda 63 unidades sem ID recuperável e sem dono publicado; o conteúdo integral das atas permanece **[nao verificado]**. (provas do TSV; app/docs/RELATO.md:8393–8398)

**Entrada documental ESTRUTURA recuperável: 18 = 6 da ingestão histórica + 5 BO2910 + 7 BO-ADMIN-2710.** O universo integral e a situação atual das seis unidades históricas não estão certificados; por isso, o contrato completo de entrada ainda fica **[nao verificado]**. (app/docs/RELATO.md:8390, :52; app/docs/BACKLOG.md:1102)

## 14/11 18:39 · sessao 67ed0ab09ba7 · - · - · origem=Ronald (tmux)

Fonte recuperável das atas/relatos ingeridos: **HJ-INGESTAO-0511**, 56 TXT do Fernando → **80 unidades pessoa+dia HJ-NNN-Uk**; regra escrita em `app/docs/BACKLOG.md:1868`, ingestão em `app/docs/RELATO.md:8368`, contagem em `app/docs/RELATO.md:8386`.

O rol completo mora no **mapa privado `logs/hj_0511/mapa.md`**, remissão em `app/docs/RELATO.md:8398`; arquivo **NÃO ACHADO na cópia**, portanto os IDs das outras **63 unidades são [nao verificado]**. A árvore publica individualmente apenas estes **17 IDs**, expandindo abreviações `/U2/U4` da própria tabela (`app/docs/RELATO.md:8390`–`:8394`):

```tsv
fonte	id_do_caso	arquivo:linha	dono_L099	prova_do_dono
HJ-INGESTAO-0511	HJ-001-U1	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391 — divergente com dono CADASTRO; lista do admin
HJ-INGESTAO-0511	HJ-001-U2	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391 — divergente com dono CADASTRO; lista do admin
HJ-INGESTAO-0511	HJ-002-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392 — divergente com dono BATIDA; lista do admin
HJ-INGESTAO-0511	HJ-003-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392 — divergente com dono BATIDA; lista do admin
HJ-INGESTAO-0511	HJ-005-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-005-U2	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-005-U4	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-005-U5	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392 — divergente com dono BATIDA; lista do admin
HJ-INGESTAO-0511	HJ-012-U1	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394 — indeterminado; lavrado irregular sem segurar
HJ-INGESTAO-0511	HJ-012-U2	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-015-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392 — divergente com dono BATIDA; lista do admin
HJ-INGESTAO-0511	HJ-016-U1	app/docs/RELATO.md:8391	CADASTRO	app/docs/RELATO.md:8391 — divergente com dono CADASTRO; lista do admin
HJ-INGESTAO-0511	HJ-017-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-027-U1	app/docs/RELATO.md:8392	BATIDA	app/docs/RELATO.md:8392 — divergente com dono BATIDA; lista do admin
HJ-INGESTAO-0511	HJ-030-U1	app/docs/RELATO.md:8390	ESTRUTURA	app/docs/RELATO.md:8390 — divergente com dono ESTRUTURA
HJ-INGESTAO-0511	HJ-031-U1	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394 — indeterminado; lavrado irregular sem segurar
HJ-INGESTAO-0511	HJ-050-U2	app/docs/RELATO.md:8394	SEM_DONO	app/docs/RELATO.md:8394 — indeterminado; dia futuro
```

A publicação de **05/11** soma **6 ESTRUTURA + 3 CADASTRO + 5 BATIDA + 33 sem divergência + 33 indeterminados = 80** (`app/docs/RELATO.md:8390`, `:8391`, `:8392`, `:8393`, `:8394`, `:8395`). Para vocabulário solicitado, há **66 sem dono publicado**, mas somente três deles têm ID exposto (`app/docs/RELATO.md:8393`, `:8394`). O TSV recuperável soma **17 = 6 E + 3 C + 5 B + 3 SEM_DONO**; **63 sem ID**, sem inventar placeholders.

**Ressalva atual:** a linha mais recente registra mudanças de **estado**, não escreve novos donos: HJ-002-U1 D-B→S; HJ-005-U1 D-E→I; HJ-005-U2/U4 D-E→S; HJ-005-U5 D-B→S. A própria linha marca a remedida **[nao verificado]** por faltar arquivo:linha da saída (`app/docs/RELATO.md:52`). Portanto os seis ESTRUTURA acima são **donos escritos em 05/11**, e não prova de seis divergências atuais. Não retirei dono a partir de S/I por inferência.

**Atas como coleção autônoma:** **NÃO ACHADA**. Existe referência à **ATA-1111**, em aval que lista cinco itens e inclui HJ-066/068/071/073, mas ela não prova autor Fernando nem universo completo: `app/docs/PROMPTS.md:2125`, `app/docs/BACKLOG.md:2544` a `:2550`. O arquivo apontado `logs/CASO-0711/aval_caso_0711_hj003_ata_1111.txt` não está na cópia [nao verificado: conteúdo].

Greps feitos na árvore inteira, incluindo ocultos e ignorados:

```text
rg --files --hidden -g AGENTS.md
rg -n --hidden --no-ignore -i 'atas.{0,60}fernando|fernando.{0,60}atas|ata.{0,40}ingerid|ingerid.{0,40}ata' .
rg -n --hidden --no-ignore -i 'fernando' .
rg -n --hidden --no-ignore -i '\b(ingestao|ingestão|ingeridas|ingerida|ingerido|ingeridos|ingest)\b' .
rg -n --hidden --no-ignore -i '\batas\b' .
rg -n --hidden --no-ignore -i 'ATA-1111|ata 11/11|ata.{0,30}1111' .
rg -n --hidden --no-ignore 'HJ-[0-9]{3}-U[0-9]+' .
rg --files --hidden --no-ignore | rg -i 'fernando|ata|auditoria|laudo|bo[-_]'
```

Não somar os **80 ingeridos** como se fossem os **19 IDs de linhas de BACKLOG**: são granularidades diferentes, sem mapa completo para deduplicação (`app/docs/BACKLOG.md:1868`; `app/docs/RELATO.md:8398`).

## 14/11 18:39 · sessao d6c942a36ee4 · - · - · origem=Ronald (tmux)

(A) Fontes e contagem:

- **BO-0611:** encontrada como `BO-[nome]-0611` e V2, `app/docs/BACKLOG.md:1911`, `app/docs/BACKLOG.md:1912`; registro resumido vigente em `app/docs/RELATO.md:1471`. Unidade = número de item 01–14; **14 casos**, preservando 11 e 12 separados embora juntos no resumo. O quadro anterior está em `app/docs/RELATO.md:1533` e `app/docs/RELATO.md:1534`, mas foi **substituído explicitamente**, conforme `app/docs/PROMPTS.md:1925`. Assim, não usei seus donos históricos de 01/04/06 como donos vigentes. V2 escreve CADASTRO para 11/12; encaminhar outro item a uma obra não escreve seu dono. (`app/docs/RELATO.md:1471`)
- **BO-1311-ROTEIRO:** encontrada como `BO-[nome]-1311-ROTEIRO`; `app/docs/BACKLOG.md:783` enumera casos 01–10 e dez colaboradores; `app/docs/RELATO.md:226` diz que a numeração segue a ordem do UNIVERSO e é contestável. Unidade = ficha numerada, não pergunta viva; **10 casos**. Totais a/b/c/d são por pergunta, sem fechamento de dono L-099 por ficha. (`app/docs/RELATO.md:222`)
- **BO2910-ESTRUTURA:** registro completo das **cinco classes** 01,02,05,06,08 em `app/docs/BACKLOG.md:1102`, literalmente “classes ESTRUTURA ... cada uma como linha”. Unidade = classe enumerada nessa linha, não todos os BOs do dia 29/10. As queixas originais podem conter outros donos; não expandi a fonte. (`app/docs/RELATO.md:2829`, `app/docs/RELATO.md:2848`, `app/docs/RELATO.md:2849`, `app/docs/RELATO.md:2851`)
- **BO-ADMIN-2710:** oito itens originais em `app/docs/CORTES.md:330`; três complementos do item 8 em `app/docs/CORTES.md:332`; resultado em `app/docs/RELATO.md:3422` a `app/docs/RELATO.md:3429`. Unidade = item 1–8 mais 8.1,8.2,8.3; **11 casos**, expressamente confirmado por `app/docs/RELATO.md:52`. Item 2 teve CADASTRO preliminar corrigido: “ESTRUTURA dentro do item 2”, `app/docs/CORTES.md:334`. Resumo mais recente fecha **7 ESTRUTURA, 2 CADASTRO, 2 não fechados**, identificando estes como 8.2/8.3. (`app/docs/RELATO.md:52`)

Os registros privados `logs/bo_mauricio_0611/`, `logs/BO-[nome]-1311/`, `logs/bo2910/`, `logs/bo2710/` **NÃO ACHADOS nesta cópia**; `ls -ld logs .git` devolveu inexistência. [nao verificado por arquivo:linha; verificação de árvore nesta exploração]

Greps executados em toda a árvore, incluindo ocultos/ignorados:

```sh
rg --files --hidden --no-ignore -g 'AGENTS.md'
rg -n -i --hidden --no-ignore 'BO[-_]?0611|BO[-_]?1311[-_]?ROTEIRO|BO2910[-_]?ESTRUTURA|BO[-_]?ADMIN[-_]?2710' .
rg -n -i --hidden --no-ignore '1311.*ROTEIRO|ROTEIRO.*1311|BO.?06.?11|BO.?0611|BO.?1311' app/docs/RELATO-ARQUIVO.md app/docs/LAPIDES.md app/docs/DOSSIES.md app/docs/TICKETS.md app/docs/RELATO.md app/docs/BACKLOG.md app/docs/CORTES.md
rg -n -i --hidden --no-ignore 'bo_mauricio_0611|BO-[nome]-0611|BO-[nome]-1311|bo_mauricio_1311|bo2910|BO2910' .
rg --files --hidden --no-ignore | rg -i '0611|1311|2910|2710|relato|lapides|dossies|tickets'
```

Não existe metadado `.git` nesta cópia; portanto `git grep` não executável aqui, e buscas equivalentes foram feitas com `rg`. [nao verificado por arquivo:linha; verificação de árvore nesta exploração]

Testes com BO no nome encontrados: sete `app/ponto/tests/test_*bo0611*`; eles tratam o caso 05 e variantes sintéticas, sem fornecer registro numerado dos 14 casos nem dono L-099 dos outros casos. (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:1`, `app/ponto/tests/test_bo0611_05_templates_geo.py:1`, `app/ponto/tests/test_bo0611_05_leitores_geo.py:1`, `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:1`, `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial_django.py:9`, `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:1`)

(B) TSV:

```tsv
fonte	id_do_caso	arquivo:linha	dono_L099	prova_do_dono
BO-0611	01	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro 18:58 substituído por app/docs/PROMPTS.md:1925
BO-0611	02	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono; V2: queixa não identificada no dado
BO-0611	03	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono
BO-0611	04	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro 18:58 substituído por app/docs/PROMPTS.md:1925
BO-0611	05	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono
BO-0611	06	app/docs/RELATO.md:1533	SEM_DONO	[nao verificado] dono vigente; quadro 18:58 substituído por app/docs/PROMPTS.md:1925
BO-0611	07	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono
BO-0611	08	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono; sem divergência não é categoria L-099
BO-0611	09	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono
BO-0611	10	app/docs/RELATO.md:1471	SEM_DONO	[nao verificado] frase de dono
BO-0611	11	app/docs/RELATO.md:1471	CADASTRO	app/docs/RELATO.md:1471: 11/12 col921 CADASTRO #37165 -> lista do admin
BO-0611	12	app/docs/RELATO.md:1471	CADASTRO	app/docs/RELATO.md:1471: 11/12 col921 CADASTRO #37165 -> lista do admin
BO-0611	13	app/docs/RELATO.md:1534	SEM_DONO	[nao verificado] frase vigente de dono; vínculo ao item13 também em app/docs/BACKLOG.md:2025
BO-0611	14	app/docs/RELATO.md:1534	SEM_DONO	[nao verificado] frase vigente de dono; vínculo ao item14 também em app/docs/BACKLOG.md:2025
BO-1311-ROTEIRO	01	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	02	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	03	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	04	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	05	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	06	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	07	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	08	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	09	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO-1311-ROTEIRO	10	app/docs/BACKLOG.md:783	SEM_DONO	[nao verificado] frase de dono por caso; numeração contestável app/docs/RELATO.md:226
BO2910-ESTRUTURA	01	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA ... cada uma como linha ... (01)
BO2910-ESTRUTURA	02	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA ... cada uma como linha ... (02)
BO2910-ESTRUTURA	05	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA ... cada uma como linha ... (05)
BO2910-ESTRUTURA	06	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA ... cada uma como linha ... (06)
BO2910-ESTRUTURA	08	app/docs/BACKLOG.md:1102	ESTRUTURA	app/docs/BACKLOG.md:1102: classes ESTRUTURA ... cada uma como linha ... (08)
BO-ADMIN-2710	1	app/docs/RELATO.md:3422	ESTRUTURA	app/docs/RELATO.md:3422: Dono ESTRUTURA
BO-ADMIN-2710	2	app/docs/RELATO.md:3423	ESTRUTURA	app/docs/CORTES.md:334: ESTRUTURA dentro do item 2; corrige CADASTRO preliminar
BO-ADMIN-2710	3	app/docs/RELATO.md:3424	ESTRUTURA	app/docs/RELATO.md:3424: ESTRUTURA
BO-ADMIN-2710	4	app/docs/RELATO.md:3425	CADASTRO	app/docs/RELATO.md:3425: CADASTRO; confirmado app/docs/CORTES.md:337
BO-ADMIN-2710	5	app/docs/RELATO.md:3426	ESTRUTURA	app/docs/RELATO.md:3426: Dono ESTRUTURA
BO-ADMIN-2710	6	app/docs/RELATO.md:3427	ESTRUTURA	app/docs/RELATO.md:3427: Dono ESTRUTURA
BO-ADMIN-2710	7	app/docs/RELATO.md:3428	ESTRUTURA	app/docs/RELATO.md:3428: Dono ESTRUTURA
BO-ADMIN-2710	8	app/docs/RELATO.md:3429	CADASTRO	app/docs/RELATO.md:3429: CADASTRO (ativa=False sem data_fim)
BO-ADMIN-2710	8.1	app/docs/RELATO.md:3429	ESTRUTURA	app/docs/RELATO.md:3429: (8.1) ... Dono ESTRUTURA
BO-ADMIN-2710	8.2	app/docs/RELATO.md:3429	SEM_DONO	app/docs/RELATO.md:52: nao fechado 2 (8.2, 8.3)
BO-ADMIN-2710	8.3	app/docs/RELATO.md:3429	SEM_DONO	app/docs/RELATO.md:52: nao fechado 2 (8.2, 8.3)
```

(C) Totais desta exploração, derivados das linhas acima:

| Fonte | ESTRUTURA | CADASTRO | BATIDA | DECISAO_ADMIN | SEM_DONO | Total |
|---|---:|---:|---:|---:|---:|---:|
| BO-0611 | 0 | 2 | 0 | 0 | 12 | 14 |
| BO-1311-ROTEIRO | 0 | 0 | 0 | 0 | 10 | 10 |
| BO2910-ESTRUTURA | 5 | 0 | 0 | 0 | 0 | 5 |
| BO-ADMIN-2710 | 7 | 2 | 0 | 0 | 2 | 11 |
| **Subtotal** | **12** | **4** | **0** | **0** | **24** | **40** |

Provas das contagens e categorias: `app/docs/BACKLOG.md:1911`, `app/docs/PROMPTS.md:1925`, `app/docs/RELATO.md:1471`, `app/docs/BACKLOG.md:783`, `app/docs/BACKLOG.md:1102`, `app/docs/RELATO.md:52`.

## 14/11 18:39 · sessao 7f90b3c24142 · - · - · origem=Ronald (tmux)

Referência comum dos CE: `vinculo_cobre_a_data` aceita vínculo inativo datado quando cobre `d` (`app/escala/alimentacao.py:196`, `:214`, `:215`); `vinculo_do_dia` não consulta S e escolhe geradora/vigência (`app/escala/alimentacao.py:269`, `:279`, `:282`, `:285`, `:286`). CE abaixo são possibilidades do código; ocorrência em produção [não verificado].

| ID | Sítio HEAD | Cru? | Predicado | Folha/fio e caminho lido | CE | Veredito |
|---|---|---|---|---|---|---|
| T13 | `app/colaboradores/views.py:619` | sim, S | sim: S=ativo e posto diferente do alvo | Não alcança: `buscar_colabs_posto` termina em JSON com até 30 resultados (`app/colaboradores/views.py:622`, `:626`). O POST resolve colaborador independentemente, sem reutilizar QS (`:640`). | sim, CE S | CONFERE |
| T26 | `app/colaboradores/views.py:2900` | sim, S | sim: após `em_gozo_hoje`, exclui S=desligado | Não alcança: `disponibilidade` acrescenta em `parados` (`app/colaboradores/views.py:2905`) e renderiza painel (`:2933`). | sim, CE S, supondo férias cobrindo hoje | CONFERE |
| T39 | `app/colaboradores/views.py:48` | sim, A | sim: prefetch apenas A=True | Não alcança: `vincular_colaboradores` usa primeira prefetch para `sem_escala` (`app/colaboradores/views.py:71`, `:73`), filtros da tela (`:97`, `:101`) e render (`:117`, `:126`). | sim, CE A | CONFERE |
| T52 | `app/colaboradores/views_fase.py:45` | sim, A | sim: A=True, primeira por `-data_inicio` | Não alcança: `fase_12x36` usa template/âncora (`app/colaboradores/views_fase.py:47`, `:49`), passa EC para previews (`:98`) e renderiza (`:115`). | sim, CE A | CONFERE |
| P51 | `app/ponto/worklist.py:49` | sim, A | sim: primeira A=True fornece posto | Não alcança: `coletar_gaps_dp` transporta posto no resultado (`app/ponto/worklist.py:50`, `:66`, `:69`) → `painel_inconsistencias_posto` agrupa (`app/colaboradores/views.py:3025`, `:3026`) e renderiza (`:3029`). | sim, CE A | CONFERE |
| C26 | `app/escala/services/cadastro_realidade.py:575` | sim, S | sim: exclui S=desligado | Lista C4: `_afastado_sem_registro` retorna assinatura sem data (`app/escala/services/cadastro_realidade.py:600`, `:606`) → `lista` incorpora (`:794`, `:898`, `:917`, `:942`). Existe caminho **diagnóstico** até vigia: `ListaDoAdmin._carregar` (`app/chamados/services/vigia_fio.py:216`, `:218`) → `colab_na_lista` (`:248`, `:251`) → causa d/e de `censo_alcance` (`:682`, `:701`). Não altera total (`:700`); C4 não entra em `_dias`, pois não tem `data` (`app/escala/services/cadastro_realidade.py:1033`). Atenção: verificar redundância do filtro frente ao novo universo que já exclui desligados antes de classificar C26. | sim, CE S quanto à lista | Predicado CONFERE; impacto exige consolidação descrita abaixo |
| C53 | `app/ponto/management/commands/reconciliar_fantasmas.py:174` | sim, A | sim: primeira A=True fornece diagnóstico do chamado antigo | Não alcança decisão: EC → `_completude` (`app/ponto/management/commands/reconciliar_fantasmas.py:205`) → `detalhe` (`:206`). Decisão é `_tta_s141(colab,d)` (`:248`) e `premissa_avaliar` com fato turno fechado (`:256`). `detalhe` só entra no motivo (`:278`), passado a `retratar` (`:280`), e na auditoria (`:299`). | sim, CE A no diagnóstico | CONFERE |
| C70 | `app/chamados/management/commands/auditar_invariantes_chamados.py:1476` | sim, S | sim: S!=ativo incrementa piso e pula A4 | Não alcança: candidato entra em `_a4` (`app/chamados/management/commands/auditar_invariantes_chamados.py:1485`), depois resultado do auditor (`:1486`, `:1505`), stdout (`:1841`) e HTML opcional (`:1853`). Não executa reabertura. | sim, CE S | CONFERE |
| A24 | `app/relatorios/furos.py:117` | sim, S | sim: S∈{ativo,afastado,ferias} | Não alcança: `dataset_furos` monta/retorna cards (`app/relatorios/furos.py:142`, `:167`) → view consome (`app/relatorios/views.py:1541`) → PDF (`:1562`) ou render (`:1566`). | sim, CE S histórico | CONFERE |
| A37 | `app/inteligencia/calculadores.py:139` | sim, S | sim: mesmo conjunto de S | Não alcança: `preenchimento_marcos` monta numerador/denominador (`app/inteligencia/calculadores.py:162`, `:174`) e retorna linhas (`:177`) → `metricas.calcular` (`app/inteligencia/metricas.py:42`) grava `MetricaSnapshot` (`:47`). | sim, CE S histórico | CONFERE |
| A50 | `app/inteligencia/views.py:209` | sim, S | sim: S∈{ativo,ferias,afastado}, empresa opcional | Não alcança: base de `api_kpi`, consumida em ramos que retornam JSON (`app/inteligencia/views.py:220`, `:222`, `:226`, `:228`, `:232`, `:235`, `:260`, `:263`, `:293`, `:295`). | sim, CE S | CONFERE |

C26: o caminho estrutural até diagnóstico do vigia está comprovado. Para classificá-lo DIVERGE por “alcança fio”, considere que o filtro exclui apenas S=desligado; no HEAD `esta_desligado` é exatamente S=desligado (`app/colaboradores/services/desligamento.py:23`, `:32`), e `censo_alcance` recebe o universo de `dias_que_seguram` (`app/chamados/services/vigia_fio.py:586`). Se esse produtor já exclui desligados em todos os casos, o efeito de C26 na causa d/e é redundante nesse universo e a afirmação funcional “não/não” do censo continua sustentável. A definição do censo inclui universos/roteamento de vigia, portanto sugiro explicitar esse caminho e a redundância na consolidação, em vez de omiti-lo.

C53 está confirmado como independente: trocar/remover a EC selecionada altera diagnóstico/motivo, mas não os argumentos nem o resultado do núcleo e da premissa que autorizam retratação (`app/ponto/management/commands/reconciliar_fantasmas.py:204`, `:248`, `:256`, `:278`).

## 14/11 18:39 · sessao a198a5bfff52 · - · - · origem=Ronald (tmux)

Verificação dos seis: **4 CONFERE, 2 DIVERGE, 0 SUMIU**. F05/F06 permanecem como leitores de D, mas o caminho causal de retenção/TXT/vigia indicado no censo não se sustenta: D ≤ fim já causa saída por rescisão antes do retentor (`app/folha/export.py:866`, `app/folha/export.py:871`, `app/folha/export.py:968`).

| ID | Sítio atual; campo cru | Predicado | Alcance comprovado | CE | Veredito |
|---|---|---|---|---|---|
| F01 | `app/folha/export.py:60`; **sim**, D | **Sim**: D não nula e D ≤ fim. | `gerar_dominio_txt` (`app/folha/views.py:212`) → `classificar_export` (`app/folha/export.py:866`) → `sai_pela_rescisao` (`app/folha/export.py:60`); resultado `fora` e `continue` (`app/folha/export.py:867`, `app/folha/export.py:871`). O gerador passa os itens à montagem (`app/folha/views.py:268`), cujo conjunto `entra` exclui o colaborador (`app/folha/export.py:716`, `app/folha/export.py:726`). TXT sai em `app/folha/views.py:281`, `app/folha/views.py:285`. | **Sim**, como diferença entre rota financeira e existência do vínculo: a autoridade não consulta D (`app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:285`); ocorrência [não verificado]. | **CONFERE** |
| F02 | `app/folha/export.py:807`, `app/folha/export.py:810`; **sim**, A+S | **Sim**: A=True, 12x36, âncora nula, S=ativo, participante dos fechamentos (`app/folha/export.py:807`, `app/folha/export.py:808`, `app/folha/export.py:809`, `app/folha/export.py:810`, `app/folha/export.py:811`). | `aprovar_fechamento` → `classificar_export` (`app/ponto/views.py:1299`) → query (`app/folha/export.py:806`); produz trava (`app/folha/export.py:835`, `app/folha/export.py:836`, `app/folha/export.py:848`), lida em `app/ponto/views.py:1301`; `trava_do_grupo` (`app/ponto/views.py:1311`) impede inclusão em `_pks` (`app/ponto/views.py:1312`, `app/ponto/views.py:1315`), usados na aprovação (`app/ponto/views.py:1365`). Também impede carimbo na geração, sem impedir por si só a linha TXT (`app/folha/views.py:244`, `app/folha/views.py:247`, `app/folha/views.py:252`). | **Sim**: S não é consultada pela autoridade; EC inativa com fim cobrindo d continua candidata (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:285`). | **CONFERE** |
| F05 | Movido a `app/folha/export.py:1241`, `app/folha/export.py:1242`, no helper `_dias_sem_decisao`; **sim**, D | **Sim localmente**: D < fim reduz fim a D; janela colapsada devolve vazio (`app/folha/export.py:1241`, `app/folha/export.py:1243`, `app/folha/export.py:1246`). Mas agora o retentor só chega ao helper depois de excluir isento/desligado (`app/folha/export.py:1207`, `app/folha/export.py:1212`). | **Não alcança a trava TXT pelo RET descrito**: truncamento efetivo implica D < fim, já capturado por F01 e seu `continue` (`app/folha/export.py:866`, `app/folha/export.py:871`), antes da chamada do retentor (`app/folha/export.py:968`). O vigia usa somente classificados `furo_espelho` (`app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`), portanto também fica mascarado. Há alcance diagnóstico: `recalcular_fechamento_mes` → `_mre` (`app/ponto/services/fechamento.py:1067`) → `motivos_retencao_celula` (`app/folha/export.py:1307`) → helper (`app/folha/export.py:1212`) e persistência de `motivos_espelho` (`app/ponto/services/fechamento.py:1186`). Há leitura individual de fio via `o_que_segura` (`app/folha/fora_da_folha.py:224`, `app/folha/fora_da_folha.py:228`), para não desligado; isso não comprova o vigia alegado. | **Sim no helper para S≠desligado; não como CE causal de trava TXT/vigia**. Para desligado, retorna vazio antes do campo (`app/folha/export.py:1207`, `app/folha/export.py:1211`); o mesmo helper alimenta apenas informativo (`app/folha/export.py:1226`, `app/folha/export.py:1228`). | **DIVERGE: alcance e guarda anterior omitida** |
| F06 | Movido a `app/folha/export.py:1269`, `app/folha/export.py:1271`; **sim**, D | **Sim localmente**: `d==D`; célula ausente só gera `sem_celula` se d<hoje e d≠D (`app/folha/export.py:1269`, `app/folha/export.py:1270`, `app/folha/export.py:1271`, `app/folha/export.py:1272`). Exceção semelhante para célula sem veredito (`app/folha/export.py:1295`). Mesma guarda nova de F05 (`app/folha/export.py:1207`). | **Não alcança a trava TXT/vigia pelo caminho alegado**: para igualdade afetar o laço é preciso D=d≤fim (`app/folha/export.py:1265`, `app/folha/export.py:1269`); F01 já excluiu esse colaborador (`app/folha/export.py:866`, `app/folha/export.py:871`). Restam os caminhos diagnósticos/individuais descritos em F05 (`app/ponto/services/fechamento.py:1067`, `app/folha/fora_da_folha.py:224`). | **Sim localmente**, escolhendo S≠desligado, d passado, EC cobrindo D=data_fim, sem célula: autoridade inclui igualdade (`app/escala/alimentacao.py:215`, `app/escala/alimentacao.py:285`), helper dispensa (`app/folha/export.py:1271`). **Não demonstra a trava alegada** (`app/folha/export.py:866`). | **DIVERGE: alcance e guarda anterior omitida** |
| P34 | `app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:438`, `app/ponto/services/fechamento.py:440`; **sim**, S+D | **Sim**: S≠desligado OU S=desligado e D≥início por empresa; sem teto superior (`app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:438`, `app/ponto/services/fechamento.py:440`, `app/ponto/services/fechamento.py:441`). | `recalcular_fechamento_mes` → `universo_da_competencia` (`app/ponto/services/fechamento.py:554`) → queryset retornado (`app/ponto/services/fechamento.py:445`) → laço de colaboradores (`app/ponto/services/fechamento.py:619`) → grava fechamento (`app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1160`). | **Sim**: desligado com D nula é excluído pelo predicado, mesmo havendo EC válida; autoridade considera EC sem consultar S/D (`app/ponto/services/fechamento.py:440`, `app/ponto/services/fechamento.py:441`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:285`). | **CONFERE** |
| C35 | `app/escala/services/leitor_celula.py:365`, `app/escala/services/leitor_celula.py:389`; **sim**, D | **Sim**: d>D OU d=D e sem fato, além do ramo admissão; fato é minutos realizados OU n_celulas−n_missing>0 (`app/escala/services/leitor_celula.py:385`, `app/escala/services/leitor_celula.py:386`, `app/escala/services/leitor_celula.py:389`). | **Folha**: `recalcular_fechamento_mes` → `grade_do_fechamento` (`app/ponto/services/fechamento.py:1014`) → `grade_da_celula` (`app/folha/export.py:338`) → corte/`fora_vinculo` (`app/escala/services/leitor_celula.py:389`, `app/escala/services/leitor_celula.py:393`); excluído dos dias internos (`app/ponto/services/fechamento.py:1043`) e dos dias trabalho/ausência (`app/ponto/services/fechamento.py:1046`), com campos apurados (`app/ponto/services/fechamento.py:1048`, `app/ponto/services/fechamento.py:1083`). **Fio**: `gerar_perguntas_disputa` → `_data_furo_da_grade` (`app/chamados/services/disputa_emissao.py:350`) → `grade_da_celula` (`app/chamados/services/disputa_emissao.py:83`); só aceita `trabalho`, senão None (`app/chamados/services/disputa_emissao.py:85`, `app/chamados/services/disputa_emissao.py:87`); texto ausente causa `continue` (`app/chamados/services/disputa_emissao.py:360`, `app/chamados/services/disputa_emissao.py:366`) antes de `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:444`). | **Sim**: D=data_fim, sem fato; igualdade incluída pela autoridade (`app/escala/alimentacao.py:215`, `app/escala/alimentacao.py:282`), excluída pela grade (`app/escala/services/leitor_celula.py:389`). | **CONFERE** |

Para o fechamento solicitado: **F05 e F06 caem como provas dos caminhos RET→TXT e dias da folha→vigia apresentados no censo**, embora os sítios crus não tenham sumido e ainda alcancem diagnóstico/informativo/leitura individual (`app/folha/export.py:1241`, `app/folha/export.py:1269`, `app/ponto/services/fechamento.py:1067`, `app/folha/fora_da_folha.py:224`, `app/folha/export.py:1228`).

## 14/11 18:39 · sessao 8520b811ee6b · - · - · origem=Ronald (tmux)

Todos os oito sítios permanecem; predicados conferem. Recomendo **8 CONFERE**, com ressalva explícita sobre o alcance condicional de P39.

A autoridade aceita EC inativa com fim declarado cobrindo a data (`app/escala/alimentacao.py:196`, `:214`, `:215`); escolhe pela geradora ou pelos vínculos cobertos e não lê `situacao` (`app/escala/alimentacao.py:269`, `:279`, `:282`, `:285`, `:286`). Portanto CE S/A procedem como hipóteses de código nos oito casos; ocorrência de produção é [não verificado].

| ID | Sítio HEAD / leitura | Predicado | Caminho comprovado / alcance | CE | Veredito |
|---|---|---|---|---|---|
| P39 | `app/ponto/services/he_pendente_lavrado.py:92`, S: sim | Sim: empresa e `situacao='ativo'`; fornece dias/retrato em `:160`, `:169` | **Folha sim, condicional; fio não demonstrado.** Caminho original: `apurar` (`he_pendente_lavrado.py:92`, `:160`) → `lavrar` (`app/ponto/management/commands/lavrar_he_pendente.py:62`, `:71`; grava em `he_pendente_lavrado.py:182`) → `ler` (`app/ponto/management/commands/recusar_ponta_pequena.py:98`) → alvos (`:113`, `:156`) → `recusar_ponta_pequena` (`:170`) → `_gravar` (`app/ponto/portas/he.py:341`) → estado vivo (`app/ponto/services/gestao_he.py:86`) → `medir` (`app/folha/porta_export.py:684`, `:686`) → teste de trava (`:230`, `:232`, `:690`) e recusa (`:853`, `:865`). A pequena estável não trava: a recusa automática rejeita acima do limite (`he.py:313`) e TXT só trava acima (`porta_export.py:230`). Ainda pode alcançar com foto pequena e vivo posterior grande; comando exige apenas foto do mesmo dia (`recusar_ponta_pequena.py:105`, `:106`). **Caminho adicional mais direto:** lavrador injeta `_revogar` (`lavrar_he_pendente.py:56`, `:59`, `:62`) → `apurar` chama callback somente para os S ativos (`he_pendente_lavrado.py:92`, `:143`) → `revogar_recusa_do_sistema` marca revogada (`he.py:472`) → `_marcar_revogada.save` (`he.py:373`, `:375`) → `estado_por_dia` lê só vigentes (`gestao_he.py:86`, `:87`) → portão supracitado. | Sim, S atual pode omitir vínculo histórico válido e impedir a revogação (`he_pendente_lavrado.py:92`, `:143`; autoridade acima) | CONFERE, alcance condicional precisa da ressalva |
| P44 | `app/ponto/services/vigia_de_hora.py:117`, S: sim | Sim: pula desligado **ou isento** (`:117`, `:119`) | **Fio sim**, `rodar` → `julgar_celula` (`vigia_de_hora.py:131`) → `julgar_colab` (`app/ponto/services/cartorio.py:2040`) → `emissor._abrir_chamado` sob apply e demais gates (`cartorio.py:1791`, `:1829`) → `ChamadoColaborador.abrir` (`app/ponto/management/commands/detectar_ausencias.py:694`). Raw gate também impede julgamento/reconciliação, como alegado. Folha não demonstrada neste fluxo. | Sim S, inclusive ontem (candidatas de hoje/ontem em `vigia_de_hora.py:114`, `:115`) | CONFERE |
| P66 | `app/chamados/services/pergunta_no_app.py:186`, S: sim | Sim: COBRANCA, módulos `CALADOS_POR_ISENCAO`, colaborador ativo (`:185`, `:186`) | **Fio sim, sob apply**. `Command.handle` → `medir` (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:21`) → filtro/achados (`pergunta_no_app.py:186`, `:201`, `:203`) → `perguntar` (`command:29`; dry retorna em `:26`, `:28`) → `criar_disputa_a_partir_de_chamados_orfaos` (`pergunta_no_app.py:246`) → pai/disputa (`app/chamados/services/disputa_emissao.py:1104`, `:1123`) → `_complementar_marcos_faltantes` (`:1175`) → `_criar_pergunta_se_couber` (`:881`) → `PerguntaDisputa.abrir` (`:837`). Folha não demonstrada. | Sim S | CONFERE |
| P65 | `app/chamados/services/limbo_folgas.py:65`, A: sim | Sim: conta A=True do template, ou usa 1 sem tid (`:64`, `:65`) | **Não alcança folha/fio decisório neste sítio.** `aplicar_sugestao` → `texto_seco(sug,n_tpl)` → `reescrever_texto` (`:70`); n_tpl só forma sufixo textual (`:49`, `:51`, `:52`). Aceitação decide pelos dias/classe/tid (`:82`, `:85`), não pelo contador. | Sim A para contagem do alcance; não prova mudança de emissão (`:51`, `:65`) | CONFERE |
| P22 | `app/ponto/selecao_periodo.py:278`, A: sim | Sim: mapa de jornada por A=True; fallback 24h (`:269`, `:278`, `:280`, `:287`). Nota: `order_by(data_inicio)` + `setdefault` escolhe a **primeira mais antiga**, apesar do comentário dizer mais recente (`:276`, `:279`, `:280`); o censo não alegou ordem. | **Não alcança** no fluxo encontrado. `turnos_em_turno_stored` retorna mapa (`:292`, `:293`) → wrapper `colabs_em_turno_stored` (`:253`) → endpoint de dashboard usa tamanho (`app/core/views.py:1160`, `:1161`). | Sim A: pode usar jornada atual diferente daquela do turno histórico, ou fallback em EC inativa válida (`:278`, `:287`) | CONFERE |
| C64 | `app/ferias/management/commands/alertar_retornos.py:30`, S: sim | Sim: ativo; EC do mapa ativo (`:28`, `:31`), hoje False e amanhã True (`:34`) | **Não alcança folha/fio decisório.** `handle` → lista retornam (`:35`) → `push_dp` (`:45`), aviso para disponibilidade. | Sim S | CONFERE |
| C11 | `app/escala/views_plano_folgas.py:176`, A: sim | Sim: pk e A=True; ausente erro `vinculo` e continue (`:176`, `:178`, `:179`) | **Folha e fio sim.** `plano_folgas_aplicar` → `aplicar_folgas_em_lote` (`:185`) → `relavrar_celulas_da_foto` (`app/escala/folgas.py:418`) → `regenerar_celulas_vinculo` (`folgas.py:293`; reexport em `app/ponto/management/commands/gerar_celulas.py:13`) → `_o_chao_do_dia_mudou` (`app/ponto/portas/celula.py:392`) → `julgar_celula` (`:472`), `recalcular_por_evento` (`:496`), callback de rejunção/reavaliação (`:531`, `:536`, `:539`, `:540`). | Sim A; vínculo inativo cobrindo um dia editável é barrado antes da porta (`views_plano_folgas.py:176`, `:179`) | CONFERE |
| C40 | `app/ponto/management/commands/reconciliar_grade.py:106`, S: sim | Sim: ativo, empresa, IDs na união de batidas/chamados/em operação (`:84`, `:95`, `:105`, `:106`) | **Fio sim sob apply**. `handle` filtro → `_emitir_furo` (`:250`, `:254`) → `ChamadoColaborador.abrir` (`:507`), eventual `reabrir` (`:530`); ramo de retratação → `retratar` (`:305`, `:309`). Cron cadastrado **sem apply** (`app/config/crons.py:462`), como censo diz. Folha não demonstrada neste fluxo. | Sim S | CONFERE |

Não há metadado Git disponível nesta cópia para autenticar o hash por `git rev-parse`; identidade do HEAD informado pelo usuário é [não verificado]. Nenhuma conclusão acima depende de banco/log.

## 14/11 18:39 · sessao 260c338b86e1 · - · - · origem=Ronald (tmux)

Contagem reproduzível: `re.match(r'^\| (HJ-[^|]+?) \|', linha)` em `app/docs/BACKLOG.md` encontra **20 linhas**: **10 IDs exatos HJ-NNN**, **9 derivados**, **1 instrumento de ingestão**. Os intervalos comprobatórios são BACKLOG:734,744–749,784,786,1868,1917,2547–2555.

**Não equiparar automaticamente as 19 linhas sem instrumento a 19 BO originais.** HJ-091-FILA-1..4 são achados do opus; HJ-091-OPUS3-FILA reúne seis achados; HJ-091-LISTA registra complemento de pouso. BACKLOG:744–749. Uma regra de caso por **raiz HJ-NNN representada nas linhas** produz **11 casos**: os 10 IDs diretos abaixo mais HJ-091, documentado pelos derivados e pela menção expressa em BACKLOG:1917. O registro individual original dos 11 e sua granularidade pessoa+dia não estão completos na cópia. [nao verificado] [nao verificado]

TSV das **19 ocorrências de registro**, preservando cada ID literal; a classificação de obras da GRADE não foi usada como dono do caso. O único dono inequívoco de linha HJ encontrado é HJ-092, “FECHADA como BATIDA”. BACKLOG:784.

```tsv
fonte	id_do_caso	arquivo:linha	dono_L099	prova_do_dono
BACKLOG-HJ	HJ-091-REJEITADA-SOBREVIVE-A-JANELA	app/docs/BACKLOG.md:734	SEM_DONO	[nao verificado]; lei pendente na linha; sem dono único L-099 fechado localizado
BACKLOG-HJ	HJ-091-FILA-1	app/docs/BACKLOG.md:744	SEM_DONO	[nao verificado]; achado do opus sem dono L-099 fechado localizado
BACKLOG-HJ	HJ-091-FILA-2	app/docs/BACKLOG.md:745	SEM_DONO	[nao verificado]; achado do opus sem dono L-099 fechado localizado
BACKLOG-HJ	HJ-091-FILA-3	app/docs/BACKLOG.md:746	SEM_DONO	[nao verificado]; achado do opus sem dono L-099 fechado localizado
BACKLOG-HJ	HJ-091-FILA-4	app/docs/BACKLOG.md:747	SEM_DONO	[nao verificado]; achado do opus sem dono L-099 fechado localizado
BACKLOG-HJ	HJ-091-LISTA	app/docs/BACKLOG.md:748	SEM_DONO	[nao verificado]; lista DP/passivo não fecha dono único do caso HJ
BACKLOG-HJ	HJ-091-OPUS3-FILA	app/docs/BACKLOG.md:749	SEM_DONO	[nao verificado]; linha reúne seis achados sem dono único L-099 fechado localizado
BACKLOG-HJ	HJ-092	app/docs/BACKLOG.md:784	BATIDA	app/docs/BACKLOG.md:784: FECHADA como BATIDA; app/docs/RELATO.md:271 confirma
BACKLOG-HJ	HJ-093	app/docs/BACKLOG.md:786	SEM_DONO	[nao verificado]; linha mistura pergunta de decisão humana e realizado zerado, sem dono único fechado
BACKLOG-HJ	HJ-090	app/docs/BACKLOG.md:1917	SEM_DONO	[nao verificado]; exige dono L-099 dos dias com batida, mas não fornece dono único fechado
BACKLOG-HJ	HJ-066	app/docs/BACKLOG.md:2547	SEM_DONO	[nao verificado]; fila UI não equivale a dono L-099 fechado
BACKLOG-HJ	HJ-068	app/docs/BACKLOG.md:2548	SEM_DONO	[nao verificado]; fila UI não equivale a dono L-099 fechado
BACKLOG-HJ	HJ-071	app/docs/BACKLOG.md:2549	SEM_DONO	[nao verificado]; fila UI não equivale a dono L-099 fechado
BACKLOG-HJ	HJ-073	app/docs/BACKLOG.md:2550	SEM_DONO	[nao verificado]; censo pendente sem dono L-099 fechado localizado
BACKLOG-HJ	HJ-074	app/docs/BACKLOG.md:2551	SEM_DONO	[nao verificado]; vínculo com obra não equivale a dono do caso fechado
BACKLOG-HJ	HJ-075	app/docs/BACKLOG.md:2552	SEM_DONO	[nao verificado]; aguarda print; sem dono fechado localizado
BACKLOG-HJ	HJ-075-EC-FIRST	app/docs/BACKLOG.md:2553	SEM_DONO	[nao verificado]; diz fila 1 estrutural, sem frase fechando dono L-099 do caso
BACKLOG-HJ	HJ-058	app/docs/BACKLOG.md:2554	SEM_DONO	[nao verificado]; vínculo com obra não equivale a dono do caso fechado
BACKLOG-HJ	HJ-058-MEDIA-AUSENCIAS	app/docs/BACKLOG.md:2555	SEM_DONO	[nao verificado]; origem não verificada; censo antes da cura
```

No inventário literal das 19 linhas: ESTRUTURA 0 / CADASTRO 0 / BATIDA 1 / DECISAO_ADMIN 0 / SEM_DONO 18. Pela regra conservadora de **11 raízes**, BATIDA 1 / SEM_DONO 10; demais 0. A soma é derivada dos registros acima. BACKLOG:734,744–749,784,786,1917,2547–2555.

Para o TSV principal de **11 raízes**, use os dez HJ-NNN diretos e acrescente:

```tsv
BACKLOG-HJ	HJ-091	app/docs/BACKLOG.md:1917	SEM_DONO	[nao verificado]; menção expressa +HJ-091; derivados em app/docs/BACKLOG.md:734 e :744-749; nenhum dono único L-099 fechado localizado
```

Exclusões e cuidados:

- **HJ-INGESTAO-0511** é instrumento “56 HJ -> unidades pessoa+dia”, não outro BO. BACKLOG:1868.
- **HJ-057: NAO ACHADA como registro individual**; única menção localizada: “HJ-057 sem linha na copia”. RELATO:52. Grep executado: `rg -n --hidden --no-ignore 'HJ-057'`.
- **HJ-067/HJ-069/HJ-095** aparecem como referências, sem primeiro campo HJ próprio no BACKLOG: HJ-067 em BACKLOG:2544; HJ-069 em BACKLOG:2536; HJ-095 em BACKLOG:2153.
- **HJ-058** menciona “1 caso desde 30/09 (06/11), 4 no total”; não transformar isso em quatro linhas de caso sem IDs e registros recuperáveis. BACKLOG:2554.
- **HJ-093** diz “decisao humana” para a pergunta #50873, mas também relata a célula zerada e confirma a classe de pagamento. Atribuir DECISAO_ADMIN ao HJ inteiro apagaria essa mistura; falta dono único escrito. BACKLOG:786; RELATO:266.
- **HJ-091-G** consta como “obra ESTRUTURA”; isso prova tipo da obra, sem necessariamente fechar o dono único de todos os casos HJ-091 e passivo. RELATO:66.
- Coordenei com `/root/atas`: as unidades históricas HJ-001..031 identificadas no RELATO não coincidem com os IDs diretos HJ-058..093. O acervo de 80 unidades segue separado, sem inventar IDs faltantes. RELATO:8386,8390–8394.

Buscas realizadas: `rg -n '^\| HJ-' app/docs/BACKLOG.md`; `rg -n '\bHJ-[0-9]{3}\b' app/docs/BACKLOG.md`; `rg -n --hidden --no-ignore 'HJ-'` nos RELATO, RELATO-ARQUIVO, LAPIDES, DOSSIES, TICKETS e CORTES; busca ampla por HJ e dono/ESTRUTURA/CADASTRO/BATIDA na árvore. Nenhum arquivo foi criado, editado ou apagado. [nao verificado por diff: cópia sem .git]

## 14/11 18:37 · sessao 17c379e7a817 · - · - · origem=Ronald (tmux)

Censo por AST + buscas textuais em produção; não alterei arquivos. As linhas de `fio.py` e `disputa_emissao.py` precisam ser atualizadas após a cura do pai. [nao verificado]

**Escritores**

- `chamado_destino`: `chamados/models.py:367` (`ChamadoColaborador._vivas_entre`, simulação); `:3043` (`PerguntaDisputa._nascer`, reuso); `:3075` (`_nascer`, construtor). Os sítios `:2846` e `:2858` encaminham o argumento para `_sob_contrato`, sem atribuição própria. Na base, `chamados/services/fio.py:825`, `:829` (`_costura_nao_mata_o_fio`, simulação/restauro) e `:971` (`reapontar_resolvedora`). A cura em andamento já concentrou essas atribuições em `chamados/services/fio.py:790`, `:793`, `:796`.
- `DS.fechada_em`: `chamados/models.py:1988` (`fechar`, save `:1992`), `:2205` (`reabrir_admin`, save `:2210`), `:2229` (`reabrir_sistema`, save `:2233`).
- `DS.motivos`: `chamados/models.py:2022` (`aparar_motivo_respondido`), `:2119` (`remover_motivo`), `:2215` (`reabrir_admin`); `chamados/services/disputa_emissao.py:601` (`_agregar_motivos_na_dona`, save `:606`), e nascimentos `:712`, `:1354`, `:1625`.
- `Pergunta.respondida_em`: `chamados/models.py:1555` (`_universo_da_ressuscitacao`, simulação), `:3193` (`carimbar`), `:3224` (`_desfazer_em_memoria`), `:3323` (`descarimbar_em_lote`, recebe `**outros` de `:1613` e `disputa_emissao.py:1857`); `chamados/services/disputa_emissao.py:1732` (`responder_pergunta_disputa_core`), `:1917` (`responder_automaticamente`), `:1938` (`carimbar_resposta_pela_trilha`).

**DS.motivos — todos os leitores encontrados e pressuposto**

| Sítio | Uso / pressuposto |
|---|---|
| `chamados/models.py:2017` | `aparar_motivo_respondido`: lista de motivos restantes |
| `chamados/models.py:2088` | `remover_motivo`: lista completa decide remoção e fecho |
| `chamados/models.py:2212` | `reabrir_admin`: acrescenta ao agregado |
| `chamados/services/disputa_emissao.py:579` | `_contexto_do_ato`: presença determina contador de recorrência |
| `chamados/services/disputa_emissao.py:597` | `_agregar_motivos_na_dona`: diferença/união com agregado |
| `chamados/services/disputa_emissao.py:1306` | repasse: entrada do complemento |
| `chamados/services/disputa_emissao.py:1327` | repasse: seed e motivos existentes |
| `chamados/services/descarte_disputa.py:9` | fotografa agregado fechado |
| `chamados/services/descarte_disputa.py:10` | calcula somente motivos ainda não agregados |
| `chamados/services/fio.py:680` | corpo do push de regularização |
| `chamados/services/acoes_disputa.py:149` | corpo do push de regularização |
| `chamados/relatorios_disputa.py:52` | exibe labels do agregado |
| `chamados/management/commands/relatorio_pendencias_disputa.py:39` | exibe labels recebidos do relatório |
| `chamados/management/commands/auditar_disputas.py:88` | verifica contexto de cada motivo agregado |
| `chamados/management/commands/auditar_disputas.py:142` | relatório de disputa parada |
| `chamados/management/commands/auditar_disputas.py:169` | compara todos os motivos com todas as perguntas; supõe agregado completo |
| `chamados/management/commands/auditar_invariantes_chamados.py:50` | universo dos dias na disputa sem pergunta; supõe todos os motivos agregados |
| `ponto/management/commands/alertar_disputas_pendentes.py:82` | log de disputa velha |
| `api/foto_recebida.py:63` | filtro exige `foto_ausente` agregado |
| `api/foto_recebida.py:70` | restantes decidem aparar ou fechar; supõe agregado completo |
| `escala/signals.py:171` | filtro exige `ancora_ausente` agregado |
| `colaboradores/signals.py:74` | filtro exige `posto_desativado` agregado |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49` | descoberta exige `device_comprometido` agregado |
| `templates/chamados/partials/modal_fio.html:327` | fluxo de âncora depende de motivo agregado |
| `templates/chamados/detalhe_local.html:301` | exibe agregado |

Omitir um motivo sem pergunta local pode excluir a disputa dos três filtros de evento — foto, âncora e posto — e da descoberta de aparelhos compartilhados (`api/foto_recebida.py:63`, `escala/signals.py:171`, `colaboradores/signals.py:74`, `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49`). O drawer explicitamente usa perguntas pendentes em vez de `DS.motivos` (`colaboradores/services/drawer.py:75`).

**DS.fechada_em — leitores**

- `chamados/models.py`: `transicionar:190`; `ressuscitar_perguntas:1621`; `aberta:1871`; `fechar:1935`; `remover_motivo:2087`; `reabrir_admin:2194`; `reabrir_sistema:2223`; `reabrir_por_colab:2280`; `limpar_resolucao_residual:2318`; `trava_na_ordem:2729`.
- `chamados/signals.py`: `disputa_fechada_fecha_chamado_pai:29`; `retoma_regularizacoes_ao_fechar_disputa:131`; `materializa_perguntas_ao_fechar_disputa:155`.
- `chamados/juizes.py:2396` (`disputa_viva_do_chamado`).
- `chamados/reconciliador.py`: `lavrar_sem_turno_aberto:412`; `retratar_perguntas_absolvidas_por_celula:600,611,612`; `_orfas_abertas:1050`; `encerrar_orfas_pela_lampada:1161,1162`; `perguntas_orfas_reatavies:1504`.
- `chamados/services/disputa_emissao.py`: `colaborador_em_disputa:42`; `_agregar_motivos_na_dona:593`; `agregar_estrangeiras:647`; `_abrir_ou_agregar_disputa:671`; `criar_ou_atualizar_disputa:874,884`; `fecho_do_repasse:1192`; `notificar_complementos:1282`; agrupador `:1324,1554`; âncora `:1598`; resposta `:1786`; `_resolver_perguntas_por_chamado_resolvido:2173,2268,2292,2294,2353,2355`.
- `chamados/services/fio.py`: `_disputa_do_chamado:131,135`; `montar_chamados_fio:187`; `processar_post_fio:600,633`.
- `chamados/services/acoes_disputa.py:255,347`; `acoes_chamado.py:346`; `becos.py:27,56,59`; `cobranca.py:230`; `competencia_trancada.py:176,221`; `descarte_disputa.py:9`; `fila_por_causa.py:225,259,262`; `foto_e4b.py:182`; `materializacao.py:591,1322`; `pergunta_no_app.py:71`; `regularizacao_ext.py:60`; `relavra_via.py:183,260`; `validacao.py:171,237,240,242`; `vigia_fio.py:753`.
- `chamados/relatorios_disputa.py:41`; `chamados/views.py:471,495,512,530,911,1329`; `chamados/views_cobrar.py:161,164`.
- Comandos de `chamados/management/commands/`: `auditar_chamados_orfaos.py:41`; `auditar_disputas.py:72,87,136,152,167`; `auditar_invariantes_chamados.py:63,620,676,890,1851`; `encerrar_perguntas_com_fato.py:82`; `fechar_becos_disputa.py:32`; `reabrir_orfas_enterradas.py:96,131,164,165`.
- `api/foto_recebida.py:62`; `api/views.py:1013,1029,2671`; `escala/signals.py:170`; `colaboradores/signals.py:73`; `colaboradores/painel_cell.py:32`; `colaboradores/services/desligamento.py:164`; `drawer.py:70`; `situacional.py:117`.
- `ponto/management/commands/alertar_disputas_pendentes.py:34`; `escalonar_chamados_supervisao.py:58,149`; `reconciliar_fantasmas.py:214`; `ponto/services/entrada_sincronizada.py:57`.
- Template: `templates/chamados/partials/modal_fio.html:379`.

**Pergunta.respondida_em — leitores**

- `chamados/models.py`: `sem_veredito_entre:1900`; `estado:3106,3109`; `carimbar:3188,3192`; `q_vivas:3409`; `respondida:3559`; `DecisaoPergunta.linhas_da_projecao_sem_linha:3985,3992,3993,3994`.
- `chamados/juizes.py`: `_slot_esperado:348`; `_chave_memo_pv:852`; `projecao_temporal_da_pergunta:1385`; `respondida_sem_texto:1528`; `perguntas_corrigiveis:1673`; `perguntas_respondidas_a_validar:1680,1683`.
- `chamados/veredito_lavrado.py:207,208`; `chamados/relatorios_disputa.py:21`; `chamados/views.py:1280,1416`; `chamados/views_cobrar.py:169,176`.
- `chamados/services/fio.py:340,660`; `acoes_disputa.py:95,130`; `becos.py:30`; `cobranca.py:229`; `declaracao_texto.py:406,408,674`; `disputa_emissao.py:1934,1977,2085,2103,2243,2316,2333`; `foto_e4b.py:180`; `furo_sem_canal.py:81`; `historico.py:99`; `materializacao.py:675,1376`; `painel.py:67`; `passivo_validado.py:66`; `pergunta_no_app.py:71`; `raiox.py:138`; `validacao.py:416,603,608,632,951`; `verbos.py:335,336,342,345`; `vigia_fio.py:680,788`.
- Comandos: `chamados/management/commands/auditar_invariantes_chamados.py:261,676,1191,1658,1689`; `backfill_decisoes_pergunta.py:56,60`; `curar_perguntas_em_limbo.py:46,69`; `ponto/management/commands/reconciliar_grade.py:445`.
- `api/views.py:1160,1172,2556,2570,2613,2630`; `api/views_mensageria.py:569,570`.
- Templates: `modal_fio.html:185,217,220`; `_card_atendimento.html:94,100,137,155,384,385`; `_pergunta_card.html:24,29,32`, todos em `templates/chamados/partials/`.

**chamado_destino — leitores**

- `chamados/models.py`: `_gravar_estado:304,310,311`; `_ancoradas:345`; `_vivas_entre:361,362,366`; `_gate_pergunta_viva:436,443,444`; `fechar:1967`; `remover_motivo:2083`; `_nascer:3010,3029,3051,3057`.
- `chamados/juizes.py`: `_pergunta_materializada_de_fato:779`; `_chave_memo_pv:843,857`; `delta_da_resposta:1860`; `motivo_implausivel_validacao:1987`; `irmas_respondidas_sem_consumo:2034`; `_dias_das_perguntas_vivas:2346`.
- `chamados/reconciliador.py:652,1052,1502`; `chamados/relatorios_disputa.py:43`; `chamados/views_cobrar.py:165`.
- `chamados/services/fio.py`: `data_do_fato_de:80,81`; `chamado_que_lista:117`; `_disputa_do_chamado:135`; `montar_chamados_fio:226,227,331,396,397`; guarda, rechaveamento e censo de colaborador cruzado nas linhas em atualização pelo pai. [nao verificado]
- `chamados/services/acoes_chamado.py:367`; `acoes_disputa.py:354`; `becos.py:30,46,47`; `competencia_trancada.py:121,132,165`; `declaracao_texto.py:302`; `disputa_emissao.py:540,812,1183,1184,1295,1316,1520,1589,1686,2063,2171,2300`; `explicador.py:122`; `foto_e4b.py:259`; `historico.py:96`; `janela_colab.py:64,65`; `limbo_folgas.py:205,240,241`; `materializacao.py:342,533,587,588,717,800,1000,1082,1111,1113,1248,1269,1271`; `pergunta_no_app.py:199,222,225`; `raiox.py:57,62,137`; `relavra_via.py:101,103,156`; `tarefa_do_dia.py:377`; `validacao.py:309,341,455,869`; `vigia_carimbo.py:103`.
- Comandos: `chamados/management/commands/auditar_disputas.py:151`; `auditar_invariantes_chamados.py:610,616,621,672,673,679,942,978,979,982,1247,1662`; `backfill_veredito_lavrado.py:101`; `lavrar_perguntas_stale.py:58,88`; `migrar_data_evento_dia_do_turno.py:377,472`; `reabrir_orfas_enterradas.py:69`; `reatar_beco_folga_ao_limbo.py:87,122,124`; `relavrar_via_i6.py:56`; `ponto/management/commands/reconciliar_fantasmas.py:212`; `reconciliar_grade.py:444`.
- `api/views.py:1031,2548,2567,2568`; `colaboradores/services/calendario.py:474`; `ponto/services/veredito_celula.py:187`.

Além das portas de cobrança já destacadas, há dois pushes de resultado de disputa ao colaborador: `chamados/services/fio.py:681` e `chamados/services/acoes_disputa.py:150`, ambos com “Sua situacao foi regularizada”. Precisam constar do censo e ter tratamento explicitado sob a exigência literal “todos” (`chamados/services/fio.py:680`, `chamados/services/acoes_disputa.py:149`).

## 14/11 18:36 · sessao 15bbc60ce588 · - · - · origem=Ronald (tmux)

Autoridade recomendada: `fio.dia_do_turno_da_linha` em `chamados/services/fio.py:1654`, já usada pelo nascimento em `chamados/models.py:2936`. Ela delega episódio ao juiz do turno (`ponto/turnos.py:2927`) e marco ao dia da célula (`ponto/turnos.py:2939`).

A substituição pode montar a mesma instância não salva usada pelo nascimento, com `instancia_do_motivo` e `batida_origem_do_nascimento`, e chamar essa autoridade. A data explícita do contexto entra somente como chave `pedido`; retire a cascata grade → contexto → batida. Sem chave nem episódio, a autoridade devolve `None` (`chamados/services/fio.py:1687`). Motivo datado/hora deve retornar `InsumoAusente`, com trilha; não deve ganhar data civil por conversa (`chamados/services/disputa_emissao.py:505`). O chamador já publica causas de recusa (`chamados/services/disputa_emissao.py:661`).

Cuidados da eleição:

- `_marcos_de_intervalo` fornece os fatos explícitos do intervalo (`chamados/services/disputa_emissao.py:263`). `motivos_atendidos` **não inclui** `intervalo_ausente`; usar somente esse juiz perderia esses candidatos (`chamados/juizes.py:2147`).
- O S160 exclui chamado com pergunta não materializada (`chamados/services/disputa_emissao.py:224`); o eleitor de intervalo inclui chamados com pergunta existente (`chamados/services/fio.py:750`). A unificação precisa conservar essa inclusão para ser idempotente.
- O `None` do S160 permite HIST/c1/c2 no reconciliador (`chamados/reconciliador.py:1559`) e reatamento imediato de pergunta recém-criada (`chamados/services/disputa_emissao.py:541`). Com menor PK na ambiguidade, esses caminhos deixam de executar para múltiplos candidatos; continuam necessários quando não há candidato.
- As duas portas de declaração passam o chamado de origem para a emissão (`chamados/services/declaracao_texto.py:365`, `chamados/services/declaracao_texto.py:653`). A resposta deve ler o destino de `perg.chamado_destino`, sem nova eleição.

Casos úteis sem banco: autoridade devolver dia diferente da chave e texto/dedup usarem o dia devolvido; autoridade devolver `None` apesar de grade/contexto/batida disponíveis e nenhum fallback ser consultado; intervalo explícito vencer chamado genérico com PK menor; permutação de pedidos conservar destino. Execução desses testes: [nao verificado]. [nao verificado]

## 14/11 18:36 · sessao 96181d5931e8 · - · - · origem=Ronald (tmux)

Censo somente leitura, linhas da base antes da cura:

| Sítio | Predicado atual |
|---|---|
| `chamados/services/disputa_emissao.py:885` — `criar_ou_atualizar_disputa` | motivos novos; DS aberta relida; não exige pergunta viva pendente |
| `chamados/services/disputa_emissao.py:1283` — `notificar_complementos` | `notificar`, motivos, dona diferente da principal e DS aberta; sem C6 |
| `chamados/services/disputa_emissao.py:1555` — órfãos | `notificar`, motivos, qualquer pergunta existente, DS aberta; inclui respondida/morta |
| `chamados/services/disputa_emissao.py:1645` — âncora | após criar conversa/analisar; sem C6 |
| `chamados/services/disputa_emissao.py:1992` — recobrança | conta todas sem resposta, mas envia mesmo com zero |
| `chamados/services/disputa_emissao.py:2041` / `:2049` — driver | só ContextVar de silêncio; envia WebPush e FCM |
| `ponto/management/commands/alertar_disputas_pendentes.py:59` | DS aberta e `renovar_contexto` concedeu carimbo diário; sem pergunta |
| `chamados/services/acoes_disputa.py:109` — reabrir | existe pergunta corrigível e `_reabrir(qs)` rodou; sem C6 após ato |
| `chamados/services/acoes_disputa.py:288` — reabrir admin | admin autorizado, DS fechada reaberta e conversa criada; sem C6 |
| `chamados/views.py:1447` — reabrir questionário | `_reabrir2(qs)` rodou; sem C6 após ato |
| `chamados/services/validacao.py:640` — recusar resposta | motivo presente, pergunta existente, respondida e não validada; reabertura rodou; sem C6 |
| `chamados/services/cobranca.py:107` / `:112`, chamado em `:187` | `checar_cobranca` e `_pendentes` já usam `consulta_do_app`; prazo não correndo; envio fora do atomic local, mas pode herdar atomic externo |
| `chamados/views_cobrar.py:44` — individual | permissão, POST, área, throttle diário; nem vitalidade nem DS aberta |
| `chamados/views_cobrar.py:123` — massa | permissão, `particionar_verbos == cobrar`, vivos, área, folha alterável, throttle; sem consulta direta à lista do app |

Avisos de conclusão de disputa também encontrados: `chamados/services/acoes_disputa.py:150`, `:156` e `chamados/services/fio.py:681`, `:688`. São posteriores à materialização e anteriores até à verificação de resultado vazio; não exigem conclusão bem-sucedida nem C6. A regra literal “todo push ... por causa de disputa” os alcança; semanticamente são avisos de conclusão, não cobrança. [Interpretação; texto literal fornecido pelo usuário.]

A fonte adequada já existe: `consulta_do_app` em `chamados/services/pergunta_no_app.py:61`. Ela toma `perguntas_do_app` (`:45`), que chama o C6 `pergunta_viva` (`:58`), e admite pendentes somente com `respondida_em is None` e disputa aberta (`:71`). Para uma DS, filtrar essa lista por `p.disputa_id == disputa.pk` conserva a mesma decisão do app.

A cura deve envolver os envios de disputa num callback de `transaction.on_commit`, relendo `consulta_do_app` dentro do callback; guardar só um booleano antes do commit pode cobrar pergunta respondida/fechada no restante da transação. `push_colaborador` só decide existência de usuário (`chamados/push_utils.py:114`) e FCM só existência de token (`chamados/fcm_utils.py:101`): o gate pertence à porta da disputa.

Atenção ao driver compartilhado: `cobranca._tocar` também é usado para abertura genérica pelo DP em `chamados/services/abertura.py:204`; pôr gate de disputa diretamente nele perderia esse caso. O `cobrar_colaborador` é o chamador de disputa e já usa a lista correta (`chamados/services/cobranca.py:155`, `:163`, `:187`).

Exclusões identificadas pelo código: mensagens DP (`chamados/views.py:989`, `chamados/services/fio.py:726`, `chamados/services/acoes_chamado.py:310`), exceção autorizada (`chamados/services/acoes_chamado.py:72`), lote de regularização externa (`chamados/views_regularizacao.py:171`, `:178`), alertas de turno (`ponto/management/commands/processar_alertas_turno.py:267`), ausência/justificativa/cobertura (`ponto/services/ausencia.py:1088`, `ponto/services/justificativa.py:242`, `:310`, `ponto/services/cobertura.py:160`, `:171`). Não são envios causados por `DisputaSupervisao` nesses sítios. `push_supervisao`/`push_dp` têm outro destinatário, inclusive alerta agregado em `ponto/management/commands/alertar_disputas_pendentes.py:103`.

O texto falso de quantidade está em `chamados/services/acoes_disputa.py:232` (`len(orfaos)`, que conta chamados). Outro “Colaborador notificado” incondicional aparece em `:299`; ambos precisam depender do resultado real do envio, distinguindo agendamento de entrega.

## 14/11 18:21 · sessao ca67df423b4b · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura escrita: após a união dos períodos, soma acima do previsto retira todos os marcos do dia, zera os minutos e registra `pago_mais_trabalhado_passa_do_previsto`. Igualdade continua pagando (`ponto/services/efeito_ausencia.py:141–150`).

**Q1.** O previsto lavrado vem da ata (`ponto/services/cartorio.py:739`). Extraí a leitura existente da grade para `minutos_previstos_da_celula`: ata primeiro, inclusive zero; fallback em `minutos_previstos_do_dia`, sem cálculo paralelo (`escala/services/leitor_celula.py:354–366`). Grade e guarda compartilham essa leitura (`escala/services/leitor_celula.py:431`; `ponto/services/efeito_ausencia.py:145`). Isso evita chamar a grade inteira, que já lê os efeitos da justificativa (`escala/services/leitor_celula.py:379–380`).

O trabalhado usado na soma é `realizado_do_dia`, sobre todos os turnos do dia; pares fechados, pausas e E solta seguem a autoridade existente (`ponto/turnos.py:707–795`). `trabalhado_do_turno` permanece na DOBRA 2 (`ponto/services/efeito_ausencia.py:129`).

**Q2.** A fila anterior não consumia cobertura nem `conserva`. Acrescentei `justificativas_para_revisao`, reunindo recusa por soma, cobertura vazia da DOBRA e motivos de conservação (`ponto/services/efeito_ausencia.py:154–178`). A fila agora oferece **Revisar cobertura** e mostra o motivo (`ponto/views.py:1738–1743`; `templates/ponto/fila_justificativas.html:60,117`).

**Q3.** Não existe tolerância cadastrada **com juiz aplicável** encontrada: os campos existem (`core/models.py:59–62`), mas estão declarados sem efeito no cálculo (`core/regua_cct.py:72–75`). A tolerância de HE é constante (`ponto/motor_calculo_v2.py:51`). **TOLERANCIA-NA-JUSTIFICATIVA: sem construção**; +1 minuto recusa (`ponto/tests/test_justificativa_soma_sem_banco.py:104`).

O diff e seus escritores/leitores ficaram assim:

| Arquivo::função / campo | Escrita ou mudança | Leitores |
|---|---|---|
| `efeito_ausencia.py::periodos_justificados` — `cobre`, `minutos` | União e recusa integral: `ponto/services/efeito_ausencia.py:138–150` | Efeito financeiro: `:46–47`; grade: `escala/utils.py:1251–1276` |
| Mesmo serviço — `conserva`, `motivo` | Conservação em `:134`; motivo da soma em `:150` | Revisão em `:172–176`; fila em `ponto/views.py:1740`; template em `templates/ponto/fila_justificativas.html:117` |
| `leitor_celula.py::minutos_previstos_da_celula` — previsto | Extração da leitura: `escala/services/leitor_celula.py:354–366`; ata escrita por `ponto/services/cartorio.py:739` | Grade em `:431`; guarda em `ponto/services/efeito_ausencia.py:145` |
| Trabalhado/realizado | Autoridade existente: `ponto/turnos.py:707–795`; transporte à ata: `ponto/services/cartorio.py:740` | Guarda em `ponto/services/efeito_ausencia.py:147–148`; grade em `escala/utils.py:2064` |
| Testes | S1–S6, propriedade, fontes e fila: `ponto/tests/test_justificativa_soma_sem_banco.py:83–272`; adaptação das bordas v2: `ponto/tests/test_justificativa_periodo_v2_sem_banco.py:135–183` | Runner de mutantes: `ponto/tests/mutantes_justificativa_soma.py:73` |

O crédito continua chegando ao motor pelo mapa existente (`ponto/services/fechamento.py:755`; `ponto/services/espelho.py:1134`; `relatorios/pdf_espelho.py:407`), lido em `ponto/motor_calculo_v2.py:2628–2634`.

Resultados locais, para revisão pelo Code/suite e opus final: [nao verificado]

- **RED antes da cura:** 14 testes, 10 falhas de asserção — [saída preservada](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_red.txt:127). [nao verificado]
- **GREEN:** 32 testes sem banco — [saída](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_green.txt:35). [nao verificado]
- **Mutantes novos:** 16/16 mortos por asserção; o comando individual de cada um está na [saída](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_mutantes.txt:1).
- **Regressão v2:** 21/21 mutantes mortos — [saída](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_regressao_mutantes.txt:22).

Comandos do RED, GREEN e dos três mutantes exigidos, respectivamente (`ponto/tests/justificativa_soma_red.txt:127`; `ponto/tests/justificativa_soma_green.txt:35`; `ponto/tests/justificativa_soma_mutantes.txt:1–6`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco -v
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco ponto.tests.test_justificativa_periodo_v2_sem_banco -v
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma sem_guarda
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma limite_exclusivo
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma so_turno_da_ancora
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma --todos
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_periodo_v2 --todos
```

Três testes ORM foram escritos para a sessão executar; resultado **[nao verificado]** (`ponto/tests/test_justificativa_paga_o_periodo.py:325–369`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_justificativa_paga_o_periodo
```

Sem teto, desconto parcial ou tolerância nova na guarda (`ponto/services/efeito_ausencia.py:148–150`). Nenhum caminho proibido aparece entre os alterados; `git diff --check` passou (`ponto/tests/justificativa_soma_verificacao.txt:3,17`). Frota, DSR, TXT, banco produtivo e aplicação em competência: **[nao verificado]**.

`LEI-AKITA | origem=anexo justificativa_pela_soma.txt, Ronald 14/11 18:08 [nao verificado na copia] | testemunha=S1–S6 (ponto/tests/test_justificativa_soma_sem_banco.py:83–139) | RED=justificativa_soma_red.txt:129 | quem-mais-le=efeito/grade/fila (ponto/services/efeito_ausencia.py:46,172; escala/utils.py:1257; ponto/views.py:1740) | juizes novos=0 | padrao=Single Source of Truth / Aggregate Root (docs/GLOSSARIO.md:11; escala/services/leitor_celula.py:354–366)`

## 14/11 18:01 · sessao b161216b6cea · E3-V10-DEGRAU-DONO-E-PORTA · leitura · origem=Code

1. **No teste isolado de classe e, a leitura da v2 dá `fio='colab'` e `rota='porta_app'`.** A fixture cria pergunta sem resposta, sob chamado `resolvido`; sem decisões, a tarefa exige `responder/colaborador`. A porta do colaborador depende da pendência no app, sem exigir chamado na fila. ([test_e4_passivo.py:67](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:67), [tarefa_do_dia.py:142](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:142), [tarefa_do_dia.py:601](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:601), [tarefa_do_dia.py:625](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:625))

   Os relógios são diferentes:

   - A classe de testes congela **25/09/2026 às 12:00 UTC**, relógio sob o qual cria a disputa e a pergunta. `DisputaSupervisao.aberta_em` usa `auto_now_add`; `data_evento` da pergunta é explicitamente `DIA=01/09/2026`, não a data de criação. ([test_e4_passivo.py:26](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:26), [test_e4_passivo.py:34](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:34), [models.py:1803](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/models.py:1803), [test_e4_backfill.py:28](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:28))
   - `_rodar(aplicar=True)` congela **15/09/2026 às 12:00 UTC** durante o comando. O comando captura `timezone.localtime()` e, em apply, usa `agora.date()`, ignorando `--hoje`. Isso corresponde ao cabeçalho `15/09 09:00 −03` anexado. ([test_e4_backfill.py:98](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:98), [backfill_fio_morto_e4b.py:218](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:218))

   A pergunta criada sob o relógio de 25/09 continua carregada na consulta de 15/09: a carga seleciona perguntas por colaborador, `conversa=False` e `data_evento`, sem cortar pelo nascimento da disputa. O próprio contrato declara pergunta e identidade como estado atual. ([tarefa_do_dia.py:367](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:367), [tarefa_do_dia.py:392](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:392))

   **O predicado exato que o exclui do plano normal é `x[4] == 'sem_porta'`.** Com `porta_app`, o vigia contabiliza a porta e executa `continue`, antes de incrementar `fio_morto_alcance`; `_alcance` retém somente as linhas `sem_porta`. Portanto ele sai do subconjunto do backfill, não necessariamente de `alcance_universo`. O complemento de exportadas também não inclui competência cuja entrega seja `False`. ([vigia_fio.py:658](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:658), [vigia_fio.py:714](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:714), [backfill_fio_morto_e4b.py:76](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:76), [backfill_fio_morto_e4b.py:173](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:173))

   O mutante falha antes de testar a reabertura: ele espera erro contendo `ato=reabrir_credor`, mas a caracterização primeiro exige encontrar a linha do dia. ([test_e4_passivo.py:73](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:73), [test_e4_passivo.py:87](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:87))

2. **Os dois dias são 01/09 e 02/09/2026.** O primeiro é a célula-base, inicialmente sem pergunta, com entrada apagada e saída acesa. O segundo é `DIA+1`, com lâmpadas equivalentes e pergunta sem resposta sob chamado `resolvido`. ([test_e4_backfill.py:77](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:77), [test_e4_passivo.py:97](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:97))

   **Qual deles produziu `segue_sem_porta` na execução anexada: [não verificado].** O anexo fornece o agregado, sem as linhas `DIA` desse teste. O comando atribui `segue_sem_porta` quando o `cel_id` executado permanece no conjunto `sem_porta` do censo posterior; o agregado não identifica essa célula. ([backfill_fio_morto_e4b.py:368](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:368))

   Há uma incompatibilidade que a diferença de relógios, sozinha, **não explica**: pela leitura estática da fixture, a pergunta de 02/09 deve ter porta no app também em 25/09. C6 retorna viva pela célula `furo`; a partição do app considera pendente a pergunta sem resposta em disputa aberta; sem decisões, a tarefa exige responder e publica a porta. ([juizes.py:1448](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/juizes.py:1448), [pergunta_no_app.py:78](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/pergunta_no_app.py:78), [tarefa_do_dia.py:142](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:142), [tarefa_do_dia.py:601](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:601))

   Assim, não consigo derivar dessa cópia o contador inicial **2**, nem atribuir causalmente o residual a 02/09. O teste exige esse contador em 25/09 e depois compara o plano com ele; o estado adicional que faria essa sequência produzir o agregado anexado é **[não verificado]**. ([test_e4_passivo.py:110](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:110))

3. **Usam o mesmo juiz de porta, mas não a mesma carga nem sempre o mesmo instante.** O vigia abre uma carga em lote e lê `porta_da_tarefa`. O backfill obtém seu universo chamando esse vigia com o `agora` da borda; depois, para escolher atos, chama `tarefa_do_dia` sem fornecer carga, abrindo outra carga, individual. ([vigia_fio.py:647](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:647), [backfill_fio_morto_e4b.py:78](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:78), [backfill_fio_morto_e4b.py:87](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:87), [tarefa_do_dia.py:650](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:650))

   O censo posterior recebe **novo** `timezone.localtime()`, para enxergar os atos escritos. No teste misto, o censo externo usa 25/09, enquanto o comando roda em 15/09. ([backfill_fio_morto_e4b.py:350](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:350), [test_e4_passivo.py:110](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:110), [test_e4_backfill.py:103](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_backfill.py:103))

4. **A caracterização isolada “sem resposta sob terminal ⇒ precisa reabrir para ter alcance” perdeu essa premissa na v2:** quando a tarefa exige responder e a pergunta está pendente no app, já existe dono `colab` e porta `porta_app`. O vigia aceita essa porta antes de classificar qualquer causa e. ([tarefa_do_dia.py:577](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:577), [tarefa_do_dia.py:601](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:601), [vigia_fio.py:658](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:658))

   O ramo `e/pergunta_viva_sob_chamado_terminal` continua no código, mas só é alcançado depois de faltar porta e de não haver outro dono pelas leis consultadas. Sua presença no código não demonstra ocorrência no dado fornecido. ([vigia_fio.py:670](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:670), [vigia_fio.py:698](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:698))

   **Existe uma segunda avaliação no backfill para escolher reabertura.** Além de ler a tarefa, `ato_do_dia` percorre as perguntas e consulta `reabrir_credor_de_pergunta_viva`, independentemente das ações publicadas. Esse helper exige ausência de resposta, C6 vivo e âncora fora de `VIVOS`; não consulta `pendentes_app` e não recebe `agora`. Portanto são critérios diferentes: a tarefa pergunta por alcance atual; o helper pergunta pela elegibilidade de reabrir a âncora. Ambos usam C6, mas o helper o chama sem o horizonte temporal da tarefa. ([backfill_fio_morto_e4b.py:87](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:87), [disputa_emissao.py:1812](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/disputa_emissao.py:1812), [disputa_emissao.py:1828](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/disputa_emissao.py:1828))

Quanto aos campos, são resultados e variáveis locais nesses caminhos:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `fio`, `rota` | `_portas_da_tarefa`, no dicionário retornado. ([tarefa_do_dia.py:626](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:626)) | `porta_da_tarefa`; vigia conta rota e publica fio como papel. ([tarefa_do_dia.py:636](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/tarefa_do_dia.py:636), [vigia_fio.py:662](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:662)) |
| `fio_morto_alcance` | Vigia inicializa em zero e incrementa no caminho final sem porta. ([vigia_fio.py:578](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:578), [vigia_fio.py:714](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/services/vigia_fio.py:714)) | Backfill lê antes/depois; teste compara ao tamanho do plano. ([backfill_fio_morto_e4b.py:239](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:239), [backfill_fio_morto_e4b.py:415](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:415), [test_e4_passivo.py:114](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:114)) |
| `plano` | Comando cria a lista, acrescenta os dias e atribui resultados após execução/medição. ([backfill_fio_morto_e4b.py:251](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:251), [backfill_fio_morto_e4b.py:371](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:371)) | O comando executa/lista os itens e imprime `len(plano)` no resumo. ([backfill_fio_morto_e4b.py:262](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:262), [backfill_fio_morto_e4b.py:409](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:409)) |
| `partes` | `_resumo` do teste interpreta o texto `RESUMO`, remove `plano` e converte contagens em inteiros. O comando produz esse texto contando `p['res']`. ([test_e4_passivo.py:43](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:43), [backfill_fio_morto_e4b.py:405](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/management/commands/backfill_fio_morto_e4b.py:405)) | O teste verifica soma e distribuição esperada. ([test_e4_passivo.py:113](/home/ronald/codex_ws/b1acb79857134420f4e95290948d3fe3070338c8/app/chamados/tests/test_e4_passivo.py:113)) |

## 14/11 18:00 · sessao 37c906b22221 · - · - · origem=Ronald (tmux)

O vigia lê a porta publicada pela tarefa, sem refazer o predicado: abre `carga_da_tarefa(..., agora)`, chama `.tarefa` e `porta_da_tarefa` em `app/chamados/services/vigia_fio.py:647`, `:648`, `:649`. A alimentação anterior `alimentar_portas(..., agora=agora)` serve às causas do sem porta; é outra carga, embora receba o mesmo instante (`vigia_fio.py:611`, `:615`, `:621`).

Com porta, o vigia incrementa `alcance_<rota>`, registra papel=`porta['fio']` e encerra o processamento daquele dia (`vigia_fio.py:658`, `:662`, `:664`, `:665`). Sem porta, só depois dos baldes de lei calcula a causa e incrementa `fio_morto_alcance` (`vigia_fio.py:670`, `:672`, `:679`, `:714`). O detalhe `pergunta_viva_sob_chamado_terminal` é atribuído apenas nesse ramo já sem porta (`vigia_fio.py:706`, `:707`).

Na v2, a tarefa escreve `fio` e `rota` em `_portas_da_tarefa`; pergunta pendente no app com ação `responder/colaborador` dá dono `colab`, independentemente de âncora e prazo (`app/chamados/services/tarefa_do_dia.py:601`, `:602`, `:607`, `:608`, `:613`). Sem chamado vivo com dono, publica `porta_app` (`tarefa_do_dia.py:619`, `:624`, `:625`); a escrita final dos dois campos está em `:626`. `porta_da_tarefa` apenas copia campos da tarefa, retornando `None` quando não há rota (`:629`, `:636`). A matriz tem um caso explícito de só app → `(colab, porta_app, (), {})` (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:71`, `:73`).

O backfill usa o próprio vigia e filtra literalmente `x[4] == 'sem_porta'` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:76`, `:78`, `:79`). Assim, uma pergunta sem resposta sob terminal que publique `porta_app` deixa o conjunto de reparo, embora permaneça no universo de dias retidos (`vigia_fio.py:599`, `:658`).

Há outra seleção de elegibilidade no backfill: `ato_do_dia` lê `tarefa_do_dia(colab, dia, agora)` (`backfill_fio_morto_e4b.py:87`), mas também chama `reabrir_credor_de_pergunta_viva(p, hoje=hoje, executar=False)` para todas as perguntas do dia (`:89`, `:90`, `:96`). Esse helper não recebe `agora`, não consulta `porta_da_tarefa` nem pendentes do app: testa resposta crua, C6 sem instante, âncora e status fora de VIVOS (`app/chamados/services/disputa_emissao.py:1812`, `:1828`, `:1830`, `:1834`). Portanto há a mesma autoridade de **porta** no vigia/tarefa, e uma seleção adicional de **reabertura de credor** com horizonte diferente no backfill (mesmas linhas).

A hipótese de que simplesmente mudar 15/09 para 25/09 retire `porta_app` da mesma pergunta **não foi demonstrada**. Pelo código visível, pergunta sem resposta, disputa aberta e célula `furo` é viva em ambos os instantes (`app/chamados/juizes.py:1447`, `:1450`, `:1451`); a partição do app exige apenas `respondida_em is None` e `disputa.fechada_em is None` (`app/chamados/services/pergunta_no_app.py:78`, `:82`, `:83`); sem decisões E1 a tarefa exige responder (`tarefa_do_dia.py:142`, `:143`). A carga declara expressamente que lê a pergunta/identidade atual e não recorta sua criação (`tarefa_do_dia.py:367`, `:371`, `:372`). A explicação exata do `v0=2` naquele anexo depende de outro estado/efeito ainda não identificado. [não verificado]

Escritores e leitores:

- `fio`, `rota`: escritos no resultado transitório pela tarefa (`tarefa_do_dia.py:626`); copiados por `porta_da_tarefa` (`:636`); lidos pelo vigia (`vigia_fio.py:662`, `:663`, `:664`).
- `fio_morto_alcance`: inicializado e incrementado pelo vigia (`vigia_fio.py:578`, `:714`); lido pelo backfill (`backfill_fio_morto_e4b.py:239`) e pelo teste (`app/chamados/tests/test_e4_passivo.py:111`, `:114`).
- `plano`: criado/apendido pelo comando (`backfill_fio_morto_e4b.py:251`, `:257`); `RESUMO plano` imprime `len(plano)` (`:409`).
- `partes`: o comando escreve o agregado `conta` por prefixo de resultado (`backfill_fio_morto_e4b.py:405`, `:407`, `:408`, `:409`); o teste lê a linha RESUMO, retira `plano` e chama o restante de partes (`test_e4_passivo.py:43`, `:46`, `:47`, `:113`, `:116`). [nao verificado]

_5222 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
