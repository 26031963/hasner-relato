# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 17:32** · publicado em **12/11/2026 17:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 17:32 · sessao d7de2a47bc6a · - · - · origem=Ronald (tmux)

Censo de escritores do HEAD por AST, sem execução do app e sem gravações. A árvore não contém `.git`; a identidade do HEAD informado não pôde ser verificada independentemente `[não verificado]`.

`R = respondida_em`; `T = resposta_colab`.

| arquivo:linha:função | papel | forma/campo | prova do modelo e porta |
|---|---|---|---|
| `app/chamados/models.py:1509:ChamadoColaborador._universo_da_ressuscitacao` | ESCRITOR, memória | `_q.respondida_em = None` — R | `_q = copy.copy(p)` em `:1495`; `p` vem de `self.perguntas_que_resolvem` em `:1484`, reverse FK de PerguntaDisputa declarada em `:3415`. Não salva a cópia; simula estado para `pergunta_viva` em `:1510`. |
| `app/chamados/models.py:1567:ChamadoColaborador.ressuscitar_perguntas` | ESCRITOR, delegação | `PerguntaDisputa.descarimbar_em_lote(..., respondida_em=None)` — R | `alvo` vem da mesma reverse FK em `:1551`. Passa pela porta de descarimbo em lote, não pelo core de resposta. |
| `app/chamados/models.py:2809:PerguntaDisputa.abrir` | encaminhamento de payload | `**campos` → `_sob_contrato` — R/T se recebidos | Método da classe PerguntaDisputa, encaminha sem selecionar campos. |
| `app/chamados/models.py:2821:PerguntaDisputa.abrir_conversa` | encaminhamento de payload | `**campos` → `_sob_contrato` — R/T se recebidos | Mesmo modelo; nascimento de conversa. |
| `app/chamados/models.py:2840:PerguntaDisputa._sob_contrato` | encaminhamento de payload | `cls._nascer(**kw)` — R/T se recebidos | `cls` de PerguntaDisputa; os dois chamadores estão em `:2809/:2821`. |
| `app/chamados/models.py:3039:PerguntaDisputa._nascer` | ESCRITOR | `p = cls(..., **campos)`; `p.save(force_insert=True)` em `:3042` — R/T | `cls` de PerguntaDisputa. Guarda `recusa_respondida_sem_texto` em `:2864/:2865`. Fora do core de resposta. O código aceita o payload; chamador de produção passando explicitamente R/T no nascimento não foi encontrado. |
| `app/chamados/models.py:3157:PerguntaDisputa.carimbar` | ESCRITOR | atribuição R; `campos.append('respondida_em')` em `:3158`; `save(update_fields=campos)` em `:3177` | `self` de PerguntaDisputa. Guarda própria exige texto em `:3155`. Fora do core e da função `recusa_respondida_sem_texto`. |
| `app/chamados/models.py:3188:PerguntaDisputa._desfazer_em_memoria` | ESCRITOR, memória | restaura R a partir de `antes` | `self` de PerguntaDisputa; restauração na recusa/exceção de `carimbar`, chamadas em `:3166/:3169/:3180`. Sem persistência própria. |
| `app/chamados/models.py:3289:PerguntaDisputa.descarimbar_em_lote` | ESCRITOR | queryset `.update(..., **outros)` termina em `:3291` — R/T conforme payload | Queryset explicitamente `PerguntaDisputa.objects`; recebe R em `:1567` e R/T no caminho `disputa_emissao.py:1630`. Fora do core; HEAD não valida o par aqui. |
| `app/chamados/models.py:3448:PerguntaDisputa.save` | persistência genérica | `super().save(*args, **kwargs)` — pode persistir R/T | `self` de PerguntaDisputa. HEAD só envolve a persistência em atomic, sem guarda do par (`:3438/:3448`). É infraestrutura comum dos saves, não reúne o caminho `QuerySet.update`. |
| `app/chamados/services/disputa_emissao.py:1467:responder_pergunta_disputa_core` | ESCRITOR | T em `:1467`, R em `:1468`; `_campos` em `:1469`; `save(update_fields=_campos)` em `:1508` | `p` vem comprovadamente de `PerguntaDisputa.objects` em `chamados/views.py:1239`, passado ao core em `:1283`. É a porta de resposta web/API declarada em `disputa_emissao.py:1425/:1429`; recusa texto vazio em `:1432/:1434`. |
| `app/chamados/services/disputa_emissao.py:1593:reabrir_para_correcao` | ESCRITOR | `dict(respondida_em=None, ..., resposta_colab='')`; `descarimbar_em_lote(qs, **campos)` em `:1630` ou `qs.update(**campos)` em `:1632` | Chamadores dão queryset explícito de PerguntaDisputa em `validacao.py:593`, `materializacao.py:688`, `curar_perguntas_em_limbo.py:69`. Fora do core; porta de reabertura. |
| `app/chamados/services/disputa_emissao.py:1657:responder_automaticamente` | ESCRITOR | T em `:1657`, R em `:1658`; `save(update_fields=['resposta_colab','respondida_em'])` em `:1660` | Modelo determinado pelo chamador de foto e seu conjunto de perguntas, não só pelo nome `p`; chamada `api/foto_recebida.py:48`. Guarda comum em `disputa_emissao.py:1655`. Fora do core de resposta web/API. |
| `app/chamados/services/disputa_emissao.py:1679:carimbar_resposta_pela_trilha` | ESCRITOR | atribui R; `save(update_fields=['respondida_em'])` em `:1681` | O comando obtém os objetos com `PerguntaDisputa.objects.filter(...)` em `curar_perguntas_em_limbo.py:55` e chama em `:67/:68`. Guarda própria em `disputa_emissao.py:1675`. Fora do core e da guarda comum. |

Há um escritor operacional adicional, explicitamente restrito à sombra:

| arquivo:linha:função | papel | forma/campo | prova do modelo |
|---|---|---|---|
| `bin/sombra_mascara_pessoal.py:57:<módulo>` | ESCRITOR | `setattr(o, c, novo)` — T | `M=apps.get_model(app,mod)` em `:46`; `o` vem de `M._base_manager` em `:51`; `TEXTO[('chamados','PerguntaDisputa')]` inclui T em `bin/sombra_regras_pessoais.py:85`. |
| `bin/sombra_mascara_pessoal.py:62:<módulo>` | ESCRITOR | `M._base_manager.bulk_update(lote,list(campos))` — T; lote cheio | Mesma origem acima. |
| `bin/sombra_mascara_pessoal.py:64:<módulo>` | ESCRITOR | mesmo `bulk_update` — T; sobra | Mesma origem acima. |

O script exige `connection.settings_dict['NAME'] == 'sombra'` em `bin/sombra_mascara_pessoal.py:10`; portanto não prova uma segunda porta viva no banco de produção. A presença do escritor de T no código está comprovada pelas linhas acima.

A função `app/chamados/services/fio.py:1578:recusa_respondida_sem_texto` **não é um escritor de PerguntaDisputa**: instancia apenas `SimpleNamespace` em `:1585`, consulta o predicado e pode levantar em `:1587`. No HEAD, os escritores que a chamam são `_nascer` (`models.py:2864`) e `responder_automaticamente` (`disputa_emissao.py:1655`). O core humano, a trilha, o carimbo e a reabertura têm caminhos distintos nos pontos da tabela. Assim, não há UMA porta que concentre todas as escritas físicas enumeradas.

Casos excluídos após prova:

- `app/chamados/services/explicador.py:78`: `base.update(...)` atualiza dict, não queryset; utiliza T apenas na construção dos textos em `:79/:80`.
- `app/chamados/veredito_lavrado.py:250`: `PerguntaDisputa.objects.bulk_update(...)` usa `CAMPOS_DA_LAVRA`, definido em `:237` sem R/T.
- `app/core/reexibir.py:49`: `setattr(obj,campo,valor)` genérico, sem persistência; os chamadores encontrados são `colaboradores/views.py:1097/:1262` e trabalham com Colaborador, não PerguntaDisputa.
- `app/chamados/services/passivo_validado.py:83`: `getattr(p,c,None)` percorre `CAMPOS_REVERSAO` (`:64/:66`) para um snapshot colocado na saída em `:367`; não há restauração desse snapshot no arquivo.
- `app/chamados/services/foto_e4b.py:180`: leitura dos dicts antes/depois, não atribuição.

Não houve escritor selecionado somente pelo nome da variável: os escritores da tabela foram atribuídos por classe, queryset/relação, chamador ou registro `apps.get_model`.

Sobre escritores que mudam o desenho:

- `responder_automaticamente` é outra operação persistente de resposta comprovada, chamada por upload de foto em `api/foto_recebida.py:48`; não passa pelo core humano (`disputa_emissao.py:1657/:1660`).
- `carimbar_resposta_pela_trilha` é outra operação persistente comprovada, chamada por management command com `--apply` em `curar_perguntas_em_limbo.py:64/:68`; não passa pelo core humano.
- `PerguntaDisputa.carimbar` pode também persistir R (`models.py:3157/:3177`), além do carimbo de materialização.
- Migrações que obtêm o modelo histórico: `0051_decisao_pergunta.py:23` atualiza somente `colaborador_id` em `:25` e `instancia_marco` em `:27`; `0053_pergunta_unica_por_familia.py:37` somente consulta colisões em `:39/:46`. Os campos R/T em `0014_perguntadisputa.py:24/:25` são declaração de schema. Não foi encontrado escritor de dados R/T nas migrations examinadas.
- Receivers de PerguntaDisputa fecham beco (`signals.py:229/:240`) ou emitem eventos (`:252/:259`), sem escrita direta do par nesses corpos.

No WIP fornecido, as atribuições e updates listados permanecem; acrescentam-se validações no `save` e no lote. Isso acrescenta uma guarda compartilhada a mais caminhos, mas não transfere as escritas para uma única função — prova no anexo `wip_1b.diff`, hunks de `models.py` iniciados em `@@ -3279` e `@@ -3442`. A execução dos testes e o estado efetivo de produção são `[não verificado]`. [nao verificado]

## 12/11 17:31 · sessao 1cf1df545dd1 · - · - · origem=Ronald (tmux)

Censo AST somente leitura concluído para `app/chamados/**/*.py`, excluindo `tests/`, `migrations/`, `models.py` e `juizes.py` (estes ficaram com o agente principal). Atributos `Load`, kwargs ORM, literais de projeção, `getattr`, índices e nomes provenientes de tuplas foram conferidos no código.

Legenda: `E=respondida_em`, `T=resposta_colab`; todas as linhas abaixo são **LEITOR**. Caminhos relativos a `app/chamados/`.

| arquivo:linha:função | forma/campo | origem/modelo |
|---|---|---|
| `canal.py:65:perguntas_mudas_com_resposta` | filtro T | `PerguntaDisputa.objects`, 58,64 |
| `reconciliador.py:365:lavrar_sem_turno_aberto` | atributo T | recebe pergunta; chamada em 1620 a partir de `reatar_pergunta_orfa`; origem por encadeamento, não anotação |
| `reconciliador.py:1083:veredito_orfa` | atributo T | universo `_orfas_abertas`, `PerguntaDisputa.objects` em 1048-1049; chamada 1136 |
| `views_cobrar.py:169:validar_inline` | atributo E | `_vivas_de(d)`/perguntas vivas; origem `PerguntaDisputa` |
| `views_cobrar.py:176:validar_inline` | filtro E | `PerguntaDisputa.objects` 175 |
| `views_cobrar.py:291:proposta_lote` | `x['pergunta'].resposta_colab` T | itens da fila/classificação de perguntas |
| `veredito_lavrado.py:207-208:marcos_do_lote` | filtro E, `**_ate('respondida_em')`, `Max(E)` | alias `_PD=PerguntaDisputa`, 201; `_ate` 163 gera filtro de corte |
| `views.py:1280:responder_pergunta_disputa` | atributo E | `PerguntaDisputa.objects.filter` 1239 |
| `views.py:1416:reabrir_disputa_supervisao` | filtro E | `PerguntaDisputa.objects` na própria linha |
| `relatorios_disputa.py:21:classificar` | atributo E | `disputa.perguntas.all()` 12; `vivas` 18 |
| `services/cobranca.py:229:_calados` | filtro E | `PerguntaDisputa.objects` 228 |
| `services/vigia_fio.py:681:censo_alcance` | atributo E | universo `PerguntaDisputa.objects` 615 |
| `services/vigia_fio.py:789,792:respondida_sem_resposta` | filtro E / `values_list(T)` | `PerguntaDisputa.objects` 789 |
| `services/passivo_validado.py:83:_reversao_de` | `getattr(p,c)` E | `CAMPOS_REVERSAO` 64-66; chamada 367, universo `classificar(empresas)` 324 |
| `services/materializacao.py:280,308:_tipo_pelo_marco` | atributos T | parâmetro `p` da cadeia de materialização; chamadas originam do queryset de perguntas 1375-1382 e da validação `PerguntaDisputa.objects.get` em `validacao.py:389,463` |
| `services/materializacao.py:359,405,412:_ts_tipo_do_plantio` | atributos T | mesma cadeia |
| `services/materializacao.py:438:estado_aplicacao_pergunta` | atributo T | chamada por `fio.py:356`, sobre perguntas do chamado; usa também constantes `PerguntaDisputa` 444 |
| `services/materializacao.py:474:_declarar_ausencia_de_pergunta` | atributo T | mesma cadeia de materialização |
| `services/materializacao.py:571:_materializar_ausencia_de_resposta` | atributo T de `q` | `disputa.perguntas.filter` 538 |
| `services/materializacao.py:633:perguntas_absorvidas_por` | filtro T | `base=PerguntaDisputa.objects.filter` 629 |
| `services/materializacao.py:674-675:devolver_absorvidas_da_ausencia_rejeitada` | atributos T/E; serialização para dict | `ps=perguntas_absorvidas_por` 671; origem PD 629 |
| `services/materializacao.py:835,876,977:_materializar_resposta_hora` | atributos T; dict de saída | mesma cadeia de materialização |
| `services/materializacao.py:1001,1010,1062:_materializar_resposta_data_hora` | atributos T; dict de saída | mesma cadeia |
| `services/materializacao.py:1376:materializar_perguntas_validadas_da_disputa` | filtro E | `disputa.perguntas.filter` 1375 |
| `services/validacao.py:141:_carimbar_validacao` | atributo T | `perg` carregada por `validar_pergunta`, `PerguntaDisputa.objects.get` 389 |
| `services/validacao.py:189,207:devolver_validacao_recusada` | `refresh_from_db(fields=[T])`; atributo T | chamada 464 com a instância carregada em 389 |
| `services/validacao.py:236:_e_aceite_de_folga` | atributo T | helper da validação PD |
| `services/validacao.py:393:validar_pergunta` | atributo E | `PerguntaDisputa.objects.get` 389 |
| `services/validacao.py:576,580-581,600:recusar_pergunta` | atributos E/T; leitura `antes[T]` | `PerguntaDisputa.objects...get` 569-570 |
| `services/validacao.py:851,853,855,860-863,866:classe_do_lote` | atributos T | fila de `perguntas_respondidas_a_validar`, chamada 921-922 |
| `services/validacao.py:924:fila_validar_classificada` | atributo E | `perguntas_respondidas_a_validar()` 918,921; parâmetro `qs` alternativo não é validado por tipo |
| `services/pergunta_no_app.py:71:consulta_do_app` | atributo E | `perguntas_do_app` 70, cuja consulta é `PerguntaDisputa.objects` 49 |
| `services/declaracao_texto.py:417,419:declarar_hora_colab` | atributos E/T | retorno `PerguntaDisputa.abrir` 376 |
| `services/declaracao_texto.py:687:aplicar_declaracao` | atributo E | retorno `PerguntaDisputa.abrir` 666 |
| `services/verbos.py:335-336,342,345:placar_aceite` | filtro E; `values(E)`; `pg[E]` | `PerguntaDisputa.objects` 333 |
| `services/disputa_emissao.py:1619,1627:reabrir_para_correcao` | `values_list(T,E)`; nomes `_txt`,`_resp` na condição | queryset de PD; método chama portas PD em 1609,1630 |
| `services/disputa_emissao.py:1647:responder_automaticamente` | atributo T | parâmetro `p`, contrato e chamadas de resposta PD |
| `services/disputa_emissao.py:1675,1683:carimbar_resposta_pela_trilha` | atributos E/T | parâmetro `p`; chamada no command `curar_perguntas_em_limbo` sobre PD |
| `services/disputa_emissao.py:1718:recobrar_questionario` | filtro E | `PerguntaDisputa.objects` 1717 |
| `services/disputa_emissao.py:1826,1844:reabrir_credor_de_pergunta_viva` | atributos E/T | `p` pergunta; `carga` de relações `perguntas_que_resolvem`, 1803-1805 |
| `services/disputa_emissao.py:1984,2057,2062,2074:_resolver_perguntas_por_chamado_resolvido` | atributos E/T | universo perguntas do chamado, PD importada 17 |
| `services/explicador.py:32:_explicar_validada` | atributo T | chamada 97 por `explicar_pergunta`; origem queryset PD 122,133 |
| `services/explicador.py:79-80:explicar_pergunta` | atributo T | `PerguntaDisputa.objects.filter` 122; chamada 133 |
| `services/foto_e4b.py:180:nascimentos` | `a[c]`,`d[c]`, c=E | somente ramo `chave.startswith('perguntas:')` 179; foto associa perguntas a PD 254,272,311 |
| `services/raiox.py:138,143-144:creditos_perguntas` | `values_list(E,T)`, `p[3]`,`p[4]` | `_pq=PerguntaDisputa.objects` 56, `_vivas` 74,135-138 |
| `services/painel.py:67:marcador_do_placar` | `Max(E)` | `PerguntaDisputa.objects.aggregate` 66 |
| `services/becos.py:30:classificar` | atributo E | `d.perguntas.all()` 29 |
| `services/acoes_disputa.py:95:acao_reabrir_disputa` | filtro E | `PerguntaDisputa.objects` na linha |
| `services/acoes_disputa.py:130:acao_regularizar_disputa` | atributo E | `_vivas_de(d)` |
| `services/acoes_disputa.py:353:acao_corrigir_resposta` | atributo T | `PerguntaDisputa.objects...get` 341 |
| `services/fio.py:174:aguarda_o_colaborador` | atributo E | coleção de perguntas recebida; chamadores e contrato PD |
| `services/fio.py:681:processar_post_fio` | atributo E | `perguntas_vivas_de` importada 680 |
| `services/fio.py:1254:canonica_do_grupo` | atributo T | contrato retorno PD 1248; grupos carregados de PD, por exemplo command dedup 74,76 |
| `services/fio.py:1266:merito_perdedor` | atributo T | perguntas do mesmo grupo; constante PD 1265 |
| `services/historico.py:99,105,109:_eventos_dos_chamados` | campo variável `getattr(p,campo)` E; atributo T | `PerguntaDisputa.objects` 96, laço 97-105 |
| `templatetags/chamados_tags.py:142:leitura_da_resposta` | `getattr(perg,T,'')` | parâmetro de filtro de template; passagem por juiz de resposta 144-147; tipo não validado em runtime |
| `management/commands/curar_perguntas_em_limbo.py:32:nota_da_resposta` | atributo T | `p` de queryset PD em `handle`, 46 |
| `management/commands/curar_perguntas_em_limbo.py:46,69:Command.handle` | filtros E/T | `PerguntaDisputa.objects` |
| `management/commands/validar_classe_a.py:80:Command.handle` | atributo T | `d.respostas_sem_veredito()` 54, guardadas em `pendentes` e `classe_a` |
| `management/commands/auditar_invariantes_chamados.py:177:_contraprova_i21a` | atributo E | chamada 1585 sobre subconjunto de `_i21_cru`, queryset PD 1572 |
| `management/commands/auditar_invariantes_chamados.py:592,1105-1106,1572,1575,1603,1655:Command.handle` | atributos E/T e filtros E/T | alias `PD.objects`; 1655 relação `perguntas_que_resolvem` 1652 |
| `management/commands/backfill_decisoes_pergunta.py:56,60-61:linhas_a_criar` | filtro E; `only(E,T)` | `PerguntaDisputa.objects` 56 |
| `management/commands/backfill_decisoes_pergunta.py:326:aplicar` | atributo T | itens de perguntas do backfill |
| `management/commands/backfill_decisoes_pergunta.py:504:Command.handle` | `only(T)` | `PerguntaDisputa.objects` na linha |
| `management/commands/dedup_perguntas_fato.py:75:Command.handle` | atributo T | `ps=PerguntaDisputa.objects...` 74 |

Casos que **não** devem entrar como escritor/leitor do modelo diretamente:

- `services/fio.py:1585:recusa_respondida_sem_texto`: kwargs E/T são de **`types.SimpleNamespace`**, importado em 1583; o código cria um sondador para o predicado, não `PerguntaDisputa`. É guarda do payload, não porta que persiste. `fio.py:1578-1585`.
- `services/validacao.py:605`: dict `depois` enviado ao observador `_evento`, não escrita ORM. A limpeza real passa por `reabrir_para_correcao` em 593-594.
- `services/disputa_emissao.py:1686`: chave E em dict de observabilidade, não escrita de modelo.
- `services/materializacao.py:674-678`: dict `_foto` guarda valores lidos, não os persiste de volta.
- `services/passivo_validado.py:66`: declaração de nomes; a leitura real é 83.
- `services/disputa_emissao.py:1469,1660,1681`: literais dos campos em `save`, portanto pertencem à tabela de **escritores**, não projeções leitoras.
- `services/disputa_emissao.py:1593`: dict de limpeza E/T, portanto payload de **escritor**; persistência em 1630 ou ramo subsequente.

Pontos de cautela de tipo: as funções auxiliares acima recebem parâmetros sem anotação; a evidência é o encadeamento de chamadas e querysets indicado. As funções `aguarda_o_colaborador`, `fila_validar_classificada(qs=...)`, filtros de template e helpers públicos não verificam em runtime que todo argumento é PD; afirmar isso para qualquer chamador externo seria `[nao verificado]`. Nenhum homônimo de outro modelo apareceu neste recorte da varredura AST; a exceção concreta é o `SimpleNamespace` em `fio.py:1585`.

## 12/11 17:31 · sessao 144396724f9c · - · - · origem=Ronald (tmux)

Censo AST de `.py` fora de `app/chamados`, excluindo diretórios `tests/` e `migrations/`; templates por expressões ativas, excluindo comentários. `E = respondida_em`; `T = resposta_colab`.

| arquivo:linha:função | papel | forma/campo | prova do modelo |
|---|---|---|---|
| `bin/censo_folha.py:28,29:<módulo>` | LEITOR | filtros ORM `E__isnull` | `PerguntaDisputa.objects` explícito; import `:12` |
| `app/folha/previa.py:142:_material` | LEITOR | atributo T | Chamador `:95`; mapa alimentado de `PerguntaDisputa.objects` em `:64-67` |
| `app/ponto/supra_juiz.py:545,548:pode_executar_csf` | LEITOR | atributo E, `is not None` | Itera `d.perguntas` e `ch.perguntas_que_resolvem` em `:543-548`; relações declaradas por `PerguntaDisputa` em `app/chamados/models.py:2436-2439,3410-3415` |
| `app/api/views.py:1159,1161:api_chamados_v2` | LEITOR | T; E duas vezes, serialização e guarda | `perguntas_por_chamado` alimentado de `consulta['pendentes'] + consulta['historico']` em `:1029-1035`; fonte `app/chamados/services/pergunta_no_app.py:49-58,70-75` |
| `app/api/views.py:2557,2559:api_disputa_perguntas._item` | LEITOR | T; E duas vezes, serialização e guarda | `consulta_do_app` em `:2532-2535`; chamadas `_item(p)` `:2562,2572`; mesma fonte acima |
| `app/api/views.py:2573:api_disputa_perguntas` | LEITOR | atributo E, `is not None` | `consulta['historico']`; fonte acima |
| `app/api/views.py:2616,2633:api_disputa_responder` | LEITOR | E comparação e serialização | Objeto consultado explicitamente por `PerguntaDisputa.objects` em `:2607-2609` |
| `app/api/views_mensageria.py:567,569,570:_item_validar` | LEITOR | T; E serialização e diferença de datas | Chamador `:540-546` usa `perguntas_respondidas_a_validar()`; origem explícita `PerguntaDisputa.objects` em `app/chamados/juizes.py:1672-1676` |
| `app/colaboradores/services/calendario.py:474:contexto_calendario` | LEITOR | `.exclude(T='')` | `_PD = PerguntaDisputa`, import `:469`; queryset `:473` |
| `app/colaboradores/services/ficha.py:171:ficha_do_colab` | LEITOR | atributo T | `perguntas_do_app` em `:166`; fonte explícita em `app/chamados/services/pergunta_no_app.py:49-58` |
| `app/ponto/services/cartorio.py:1641:_julgar_colab_corpo._tem_resposta_do_colab` | LEITOR | atributo T | Chamador fornece `x.perguntas_que_resolvem.all()` em `:1645`; relação em `app/chamados/models.py:3410-3415` |
| `app/ponto/services/dia_do_colab.py:70:dia_do_colab` | LEITOR | atributo T | `for p in PerguntaDisputa.objects.filter(...)` em `:64-65` |
| `app/ponto/services/veredito_celula.py:197:fechar_o_fio_do_dia` | LEITOR | atributo T | `vivas` de `PerguntaDisputa.objects` em `:181-182`; iteração `:189` |
| `app/ponto/management/commands/reconciliar_grade.py:461:Command._relavrar_orfa` | LEITOR | filtro ORM E | `PerguntaDisputa.objects.filter` em `:460-461` |
| `bin/sombra_mascara_pessoal.py:51,54:<módulo>` | LEITOR | `.only('pk', *campos)`; `getattr(o,c)` variável T | `R.TEXTO` → `apps.get_model(app,mod)` em `:44-46`; `bin/sombra_regras_pessoais.py:85` contém `('chamados','PerguntaDisputa')` e `resposta_colab` |
| `bin/sombra_selo_pessoal.py:77:<módulo>` | LEITOR | SQL dinâmico lê T em `coalesce` e regex | Itera `R.TEXTO` `:74-76`, tabela resolvida por `apps.get_model` `:24`; configuração `bin/sombra_regras_pessoais.py:85` |

O mascarador também é **ESCRITOR fora da porta**: `setattr(o,c,novo)` em `bin/sombra_mascara_pessoal.py:57`, persistido por `bulk_update(lote,list(campos))` em `:62,64`. Ele exige banco chamado `sombra` por `assert` em `:10`; não atribuir esse escritor a produção operacional sem delimitar o universo. O código prova a escrita na cópia da sombra; execução efetiva [não verificado].

Metadados, sem leitura do valor do campo: `bin/sombra_regras_pessoais.py:85` declara T; `bin/inventario_pessoal_dos_modelos.py:53-60` e `bin/sombra_selo_pessoal.py:26-29` usam essa declaração para inventariar colunas.

Templates:

| arquivo:linha:contexto | papel | forma/campo | prova do modelo |
|---|---|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:94,155,384` | LEITOR | condições E | Loops sobre `chamado.perguntas_disputa` `:76,384`; fonte `app/chamados/services/fio.py:242,261,271` |
| mesmo `:100` | LEITOR | filtro `default:T`; `E|date` | mesma origem |
| mesmo `:108,135,148,176` | LEITOR | condições/comparações T | mesma origem |
| mesmo `:137` | LEITOR | `default:T`, condição E, `E|date` | mesma origem |
| mesmo `:142,177` | LEITOR | interpolação T com `default` | mesma origem |
| mesmo `:385` | LEITOR | `default:T`, `E|date` | mesma origem |
| `app/templates/chamados/partials/modal_fio.html:185,217` | LEITOR | condições E | Loop `:158` sobre mesma `chamado.perguntas_disputa`; fonte acima |
| mesmo `:219,220` | LEITOR | interpolação T; filtro `E|date` | mesma origem |
| `app/templates/chamados/partials/_pergunta_card.html:24,29,32` | LEITOR | condições E; `E|date` | Contexto explícito `{'perg': _p}` de `_PD.objects.filter` em `app/chamados/views.py:398-405` |
| `app/templates/chamados/partials/_lote_preview.html:24` | LEITOR | interpolação `x.pergunta.T` | `views_cobrar.py:361-363` fornece `f['por_classe'][LOTE_A]`; `services/validacao.py:918-924` põe `pergunta=p` da fila `perguntas_respondidas_a_validar()`; queryset em `juizes.py:1672-1676` |

Nenhum caso dessa tabela foi decidido **somente pelo nome**: todos têm origem rastreável em queryset, chamada ou relação declarada, conforme a última coluna.

Delta dos templates no anexo: o wip retira as condições E das linhas HEAD `94,137,155,384`, substituindo por `resposta_enviada`; preserva a exibição E e as leituras T. No resultado projetado, E continua nas linhas wip `106,143,391`; T nas linhas `106,114,141,143,148,154,182,183,391` — linhas calculadas pelos hunks fornecidos, não por arquivo wip local. A atribuição `resposta_enviada` é outro atributo, não escrita dos dois campos censitados (`wip_1b.diff`, hunk `fio.py`, nova linha `358`).

## 12/11 17:31 · sessao bc99985d1006 · - · - · origem=Ronald (tmux)

- **Não há classificação global pelo nome `ata`/`dna`.** `Name` só ganha proveniência por declaração exata arquivo+função+nome, atribuição, `values_list` ou alias; qualquer `.ata`/`.dna` e `getattr(x, 'ata'/'dna')` é aceito sem comprovar a classe. (`bin/leitores_de_chave.py:127`, `bin/leitores_de_chave.py:140`, `bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:249`)

- **O 95 depende das declarações.** Desligar somente `PARAMS` produz 73; desligar `PARAMS+RETURNS` produz 62 em reprodução em memória. A formação do número está em `bin/leitores_de_chave.py:627` e `bin/leitores_de_chave.py:631`; os mapas estão em `bin/leitores_de_chave.py:39` e `bin/leitores_de_chave.py:66`.

- **Os 33 acessos de topo dependentes desses mapas**, agrupados por sítio, são: `app/chamados/juizes.py:438` (2); `app/escala/services/leitor_celula.py:68`, `:82`, `:91`, `:103`, `:105` (2), `:115`, `:175`, `:187`, `:190`; `app/ponto/services/cartorio.py:424`, `:426`, `:738` (2), `:739`, `:740`, `:741`, `:742`, `:745`, `:746`, `:762`, `:767`, `:771`, `:805`, `:812`, `:834`, `:839`, `:913`, `:917` (2); `app/relatorios/dia_da_frota.py:21`. As declarações correspondentes estão em `bin/leitores_de_chave.py:41` e `bin/leitores_de_chave.py:68`.

- **Há furo na validação das declarações:** ela confere existência da função, sem conferir existência do nome declarado. Renomear o parâmetro `ata` e suas referências em `_orfas_da_ata`, mantendo a função, faz desaparecerem os acessos `orfas`/`n_orfas`; a contagem cai de 95 para 93 sem violar essa validação. (`bin/leitores_de_chave.py:128`, `bin/leitores_de_chave.py:541`, `app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:190`)

- **Sítios corretamente fora:** a variável chamada `ata` em `app/relatorios/furos.py:132` é mapa da grade indexado por data, consumido em `app/relatorios/furos.py:134`; `dia.get('orfas')` em `app/escala/services/furos_diarios.py:22` recebe dia da grade por `app/escala/services/furos_diarios.py:43` e `app/escala/services/furos_diarios.py:54`; outras leituras da grade ficam em `app/folha/causas.py:43`, `app/folha/causas.py:100`, `app/ponto/services/bordas_realizado.py:49`, `app/relatorios/pdf_espelho.py:477` e `app/relatorios/pdf_espelho.py:552`. `FuroDiario.orfas` é inteiro, separado do JSON da célula. (`app/escala/models.py:1883`)

- **Sítios reais incluídos:** `getattr` em `app/chamados/juizes.py:966`, `app/chamados/reconciliador.py:701`, `app/ponto/services/lastro.py:89`, `:113`, `:128`, `:149`; comprehension lendo a raiz em `app/ponto/juiz_batida.py:161` e `app/relatorios/cartao_pela_celula.py:288`; desempacotamento e alias `a` em `app/ponto/services/vigia_pareador_celula.py:69`, `:73`, `:74`, `:78`. A cópia apenas em memória `_cj.ata` também conta como escrita de código. (`app/escala/utils.py:1886`, `app/escala/utils.py:1887`, `app/escala/utils.py:1895`)

- **Faltas reais declaradas:** leitura ORM de `dna__marcos` em `app/escala/management/commands/celulas_contra_o_template.py:49`; filtro ORM de `ata.lampadas` em `app/chamados/models.py:3356`. A exclusão é declarada em `bin/leitores_de_chave.py:19` e `bin/leitores_de_chave.py:34`.

- **Subchaves em laço comum não propagam proveniência:** `ata.get('lampadas')` é visto, mas `lamp['acesa']`, `lamp['luz']` e `lamp['dispensada']` não são ligados à ata. Isso limita censo de subchaves, embora a operação de topo já seja contada. (`app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:806`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`, `bin/leitores_de_chave.py:176`)

- **Alias de dois saltos:** o corte é explícito em `bin/leitores_de_chave.py:224`. Exemplo real de acesso de topo perdido exclusivamente por esse corte: [não verificado]. Atribuição encadeada `a=b=c.ata` liga ambos os alvos ao mesmo valor; não constitui automaticamente dois saltos. (`bin/leitores_de_chave.py:134`, `bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:166`)

## 12/11 17:29 · sessao 56a1fc7d5d3d · - · - · origem=Ronald (tmux)

Achados sobre consumidores e semântica:

- O emissor é `run()`: `--contar-crus` imprime `len(top)`; JSON imprime resumo e listas; texto imprime seções e seus comprimentos. São projeções do mesmo censo, mas seus números têm unidades diferentes: operações de topo, arquivos distintos e eventos selecionados. `bin/leitores_de_chave.py:627`, `bin/leitores_de_chave.py:631`, `bin/leitores_de_chave.py:645`, `bin/leitores_de_chave.py:659`.
- No texto, `len(sel)` inclui eventos `*`, descendentes e dinâmicos que casem com a seleção; no JSON, `resumo_producao` é global mesmo numa consulta de uma chave; `producao`/`testes` são filtrados. Não são números intercambiáveis. `bin/leitores_de_chave.py:635`, `bin/leitores_de_chave.py:645`, `bin/leitores_de_chave.py:652`, `bin/leitores_de_chave.py:660`.
- Cada evento guarda posição, caminho, papéis e forma; a deduplicação usa também coluna, papéis e forma. Não é uma unidade “linha”, “arquivo” ou necessariamente “nó AST único”. Um `setdefault`, `pop` ou `+=` conta um evento com dois papéis; somar leitores e escritores superconta as operações. `bin/leitores_de_chave.py:288`, `bin/leitores_de_chave.py:302`, `bin/leitores_de_chave.py:385`, `bin/leitores_de_chave.py:424`.
- Execução somente leitura confirmou `--contar-crus CelulaDia.ata` = 95; JSON produziu 12 chaves, 95 operações de topo, 27 arquivos, 103 eventos de produção e 89 pares arquivo:linha. Os totais são resultado da execução, sem arquivo de resultado persistido: [não verificado]. O cálculo correspondente está em `bin/leitores_de_chave.py:627`.
- Os quatro eventos `*` vistos foram cópias de JSON em `app/ponto/portas/celula.py:945`, `app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:1083`, `app/ponto/services/cartorio.py:917`; entram na listagem por chave, mas não no contador. `bin/leitores_de_chave.py:629`, `bin/leitores_de_chave.py:635`.
- O consumidor executável encontrado é o selo: chama a ferramenta, captura stdout/stderr e lê JSON, número e códigos de retorno. O JSON é lido para conferir sítios, formas e separação produção/testes; o número é lido para os casos real e sintético. `bin/tests/test_leitores_de_chave.sh:26`, `bin/tests/test_leitores_de_chave.sh:30`, `bin/tests/test_leitores_de_chave.sh:36`, `bin/tests/test_leitores_de_chave.sh:44`, `bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:106`, `bin/tests/test_leitores_de_chave.sh:112`.
- A implementação chama esse número de catraca `acessos_crus_ata`, mas conta todos os eventos de topo de produção; não existe exclusão do futuro acessor nem comparação com teto anterior. Portanto ainda não implementa “acessos crus fora do acessor, só encolhe”. `bin/leitores_de_chave.py:24`, `bin/leitores_de_chave.py:627`, `bin/tests/test_leitores_de_chave.sh:44`.
- Não encontrei consumidor da ferramenta em `gerar_estado`, disjuntor ou briefs disponíveis: integração futura e unidade pressuposta por esses leitores são [não verificado]. O juiz existente do CENSO exige referências textuais de leitores/escritores e decisões, sem conferir saída desse instrumento. `bin/disjuntor.py:1522`, `bin/disjuntor.py:1565`.
- `PARAMS`/`RETURNS` são fonte manual adicional de proveniência. O guard verifica somente existência da função; não verifica nome do parâmetro, tipo nem fluxo. Renomear só o parâmetro pode reduzir o censo sem disparar rc 2. `bin/leitores_de_chave.py:39`, `bin/leitores_de_chave.py:66`, `bin/leitores_de_chave.py:538`.
- O censo publicado, o envio prévio do contrato, a execução histórica do selo, o pouso e o aval em arquivos de `logs/` não estão demonstrados pelos arquivos disponíveis: [não verificado]. A referência local ao contrato é apenas comentário/docstring. `bin/leitores_de_chave.py:10`, `bin/tests/test_leitores_de_chave.sh:2`.

## 12/11 17:28 · sessao 6c4410570fab · - · - · origem=Ronald (tmux)

Auditoria estática; selo não executado, portanto resultado da execução é [não verificado].

- **(a) Morde presença mínima**, exigindo leitor e escritor de `orfas` em dois arquivos e contador numérico positivo; não verifica completude, funções/linhas exatas, valor esperado nem incremento por novo acesso (`bin/tests/test_leitores_de_chave.sh:36`, `bin/tests/test_leitores_de_chave.sh:39`, `bin/tests/test_leitores_de_chave.sh:44`).
- **(b) Mutante não é vazio para ausência global:** primeiro exige `sem_ancora` presente, remove a chave, exige rc 1 nomeado e preserva `tipo_dia` rc 0. Ferramenta que sempre responde “sem resultado” falha no controle (`bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:58`, `bin/tests/test_leitores_de_chave.sh:60`, `bin/tests/test_leitores_de_chave.sh:62`).
- **(c) Morde rc 1 nomeado**, mas isoladamente aceitaria uma ferramenta que sempre devolvesse rc 1; o controle de (b) cobre essa quebra global (`bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:67`).
- **(f) Morde remoção da função declarada**, exige rc 2, nome e stdout vazio; não comprova validade do parâmetro declarado ou origem real do objeto (`bin/tests/test_leitores_de_chave.sh:72`, `bin/tests/test_leitores_de_chave.sh:74`; `bin/leitores_de_chave.py:541`).
- **(g) Morde desaparecimento/duplicação das oito formas**, pela lista exata de `(caminho, forma)`, mas **não testa papéis**. Retirar `ESCRITOR` de `setdefault` e `ESCRITOR/EXCLUI` de `pop` preserva o observado por (g); o escritor literal real de `orfas` continua satisfazendo (a) (`bin/tests/test_leitores_de_chave.sh:100`, `bin/tests/test_leitores_de_chave.sh:101`; `bin/leitores_de_chave.py:427`, `bin/leitores_de_chave.py:429`; `app/ponto/services/cartorio.py:745`).
- **(e) Morde separação produção/testes e chave exclusiva de teste**, mas o teste de contador apenas compara stdout com `8`: ignora rc. Um contador sintético que imprime `8` e retorna erro passa essa asserção (`bin/tests/test_leitores_de_chave.sh:106`, `bin/tests/test_leitores_de_chave.sh:110`, `bin/tests/test_leitores_de_chave.sh:112`).
- **(d) Morde parse inválido:** o universo antes válido recebe arquivo inválido, e o teste exige rc 2, arquivo nomeado e ausência de número (`bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:116`, `bin/tests/test_leitores_de_chave.sh:118`).
- **Uso:** exige rc 64 por argumento faltando (`bin/tests/test_leitores_de_chave.sh:122`).
- **DNA não tem cobertura executável:** todas as invocações usam `CelulaDia.ata`; a ferramenta inteira para DNA pode quebrar sem esse selo perceber (`bin/tests/test_leitores_de_chave.sh:36`, `bin/tests/test_leitores_de_chave.sh:44`, `bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:122`).
- **Raiz:** resolve relativamente a `BASH_SOURCE[0]/../..` e prova presença de três arquivos antes de criar temporários. Funciona com cwd externo; transportar o selo para fora da posição `bin/tests` muda a raiz calculada. Prova apenas presença desses arquivos, não identidade do checkout/hash (`bin/tests/test_leitores_de_chave.sh:16`, `bin/tests/test_leitores_de_chave.sh:18`, `bin/tests/test_leitores_de_chave.sh:22`).
- **Gravações:** no caminho normal, saídas, cópias, mutações e fixtures ficam sob `$_tmp`, com Python `-B` (`bin/tests/test_leitores_de_chave.sh:28`, `bin/tests/test_leitores_de_chave.sh:50`, `bin/tests/test_leitores_de_chave.sh:58`, `bin/tests/test_leitores_de_chave.sh:78`). **Há furo se `mktemp` falhar:** seu retorno não é verificado, e há somente `set -u`; com `_tmp=''`, o selo tenta gravar `/out`, `/err`, `/copia` e `/sint`, fora do temporário (`bin/tests/test_leitores_de_chave.sh:15`, `bin/tests/test_leitores_de_chave.sh:22`, `bin/tests/test_leitores_de_chave.sh:28`, `bin/tests/test_leitores_de_chave.sh:50`, `bin/tests/test_leitores_de_chave.sh:78`).

## 12/11 17:23 · sessao b18576745da1 · TRANCA-VIVAS-08 · sonda · origem=Code

1. **Transcrição integral do dado fornecido.** A execução e o hash da sonda não foram conferidos independentemente nesta cópia. [nao verificado]

```text
sonda_leitura: sonda_v2.py [8c23d857275b021a] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora 2026-11-12T17:13:38.126957-03:00
PF#7 emp2 04/2026 universo=0 vivas_juiz=0 contador_medir=0
PF#8 emp2 05/2026 universo=24 vivas_juiz=0 contador_medir=0
PF#9 emp2 06/2026 universo=37 vivas_juiz=0 contador_medir=0
PF#10 emp2 07/2026 universo=152 vivas_juiz=0 contador_medir=0
PF#3 emp2 08/2026 universo=12898 vivas_juiz=4 contador_medir=0
  VIVA #20798 col367 disputa#2502 data_evento=2026-08-01 dia_turno=2026-08-01 conversa=False C20=False no_medir=False respondida=False validada=False materializada=True motivo=saida_sem_entrada
  VIVA #47879 col444 disputa#3309 data_evento=2026-08-03 dia_turno=2026-08-03 conversa=False C20=False no_medir=False respondida=True validada=False materializada=False motivo=orfao_14h
  VIVA #47881 col444 disputa#3309 data_evento=2026-08-04 dia_turno=2026-08-04 conversa=False C20=False no_medir=False respondida=True validada=False materializada=False motivo=intervalo_volta
  VIVA #47884 col444 disputa#3309 data_evento=2026-08-05 dia_turno=2026-08-05 conversa=False C20=False no_medir=False respondida=True validada=False materializada=False motivo=intervalo_volta
PF#11 emp3 04/2026 universo=0 vivas_juiz=0 contador_medir=0
PF#12 emp3 05/2026 universo=7 vivas_juiz=0 contador_medir=0
PF#13 emp3 06/2026 universo=28 vivas_juiz=0 contador_medir=0
PF#4 emp3 07/2026 universo=1165 vivas_juiz=0 contador_medir=0
PF#5 emp3 08/2026 universo=2048 vivas_juiz=0 contador_medir=0
PF#14 emp4 04/2026 universo=2 vivas_juiz=0 contador_medir=0
PF#15 emp4 05/2026 universo=63 vivas_juiz=0 contador_medir=0
PF#16 emp4 06/2026 universo=616 vivas_juiz=0 contador_medir=0
PF#2 emp4 07/2026 universo=493 vivas_juiz=0 contador_medir=0
PF#6 emp4 08/2026 universo=399 vivas_juiz=0 contador_medir=0
TOTAL {'universo': 17932, 'vivas': 4, 'medir': 0, 'classe:trancado_False': 4}
total_erros=0
== anexo: retira_acervo_tranca_2.txt ==
aval Ronald: RETIRA ACERVO-TRANCA-2 (publicado 16:3x, contraria CLAUDE.md 7b DINHEIRO item 3: nenhuma trava por exportada/trancada/paga; correcao provada entra em qualquer competencia). Nao executar os itens 1, 2, 3 e 5; nenhuma lei nova. Fica SO o item 4, que e lei existente (TRANCA-DE-VERDADE): as 4 perguntas vivas na comp 08 emp2 trancada -> listar pk e o degrau de chamados/juizes.py::pergunta_viva que as deixou vivas; curar na origem se for defeito; contador vivas_apesar_da_tranca na frota, esperado 0. Segue a fila; PAREI so em lei ou !
```

2. **Por PF, conforme a saída fornecida.** [nao verificado]

| PF | Empresa | Competência | Universo | Vivas juiz | Contador medir | Evidência |
|---|---|---|---:|---:|---:|---|
| 7 | 2 | 04/2026 | 0 | 0 | 0 | [nao verificado] |
| 8 | 2 | 05/2026 | 24 | 0 | 0 | [nao verificado] |
| 9 | 2 | 06/2026 | 37 | 0 | 0 | [nao verificado] |
| 10 | 2 | 07/2026 | 152 | 0 | 0 | [nao verificado] |
| 3 | 2 | 08/2026 | 12898 | 4 | 0 | [nao verificado] |
| 11 | 3 | 04/2026 | 0 | 0 | 0 | [nao verificado] |
| 12 | 3 | 05/2026 | 7 | 0 | 0 | [nao verificado] |
| 13 | 3 | 06/2026 | 28 | 0 | 0 | [nao verificado] |
| 4 | 3 | 07/2026 | 1165 | 0 | 0 | [nao verificado] |
| 5 | 3 | 08/2026 | 2048 | 0 | 0 | [nao verificado] |
| 14 | 4 | 04/2026 | 2 | 0 | 0 | [nao verificado] |
| 15 | 4 | 05/2026 | 63 | 0 | 0 | [nao verificado] |
| 16 | 4 | 06/2026 | 616 | 0 | 0 | [nao verificado] |
| 2 | 4 | 07/2026 | 493 | 0 | 0 | [nao verificado] |
| 6 | 4 | 08/2026 | 399 | 0 | 0 | [nao verificado] |

**Há defeito de UNIVERSO em `vivas_apesar_da_tranca`, quando usado para contar todas as vivas da competência.** A lista nasce vazia e só recebe perguntas no ramo em que pai ou destino pertence a `resgatados`; no ramo comum, apenas perguntas mortas são registradas, e as vivas ficam sem inclusão. Isso contraria o comentário “contador tem que ver o mesmo que quem valida” e a LEI-AKITA 8. (`app/chamados/services/competencia_trancada.py:81`, `app/chamados/services/competencia_trancada.py:119–134`, `CLAUDE.md:35–36`)

Esse recorte é mais estreito que a exceção humana do juiz: `resgatados` exige chamado vivo elegível e `resgatado_em > pf.fechado_em`; o juiz também reconhece “Cobrar este dia” e “Resolver dia”. (`app/chamados/services/competencia_trancada.py:89–106`, `app/chamados/juizes.py:1083–1089`)

Já `r['perguntas']` vazio tem semântica intencional: representa perguntas que seriam carimbadas pela operação, retirada após a mudança para tranca por leitura. Isso não justifica omitir vivas de `vivas_apesar_da_tranca`. (`app/chamados/services/competencia_trancada.py:126–134`)

O significado operacional de `no_medir=False` dentro da sonda, inclusive se representa ausência do filtro SQL ou ausência da lista final, é **[nao verificado]**.

3. **Degrau e motivo de sobrevivência de cada VIVA.**

`pergunta_viva()` delega a `_pergunta_viva_calc()`; a tranca é testada antes da resposta do colaborador. Portanto resposta não ultrapassa uma tranca que tenha retornado `True`. (`app/chamados/juizes.py:888–894`, `app/chamados/juizes.py:1391–1407`)

| VIVA | O que os campos publicados permitem identificar | [nao verificado]
|---|---|
| **#20798** | Com `respondida=False` e `materializada=True`, são compatíveis o retorno por **célula acusando** ou por **materialização sem testemunha suficiente**. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1473–1475`) |
| **#47879** | São compatíveis o ramo de **resposta com texto sem concordância comprovada com a luz**, **célula acusando** ou **ausência de materialização**. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1407–1419`, `app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1466–1467`) |
| **#47881** | Mesmas possibilidades: resposta nas condições do juiz, célula acusando ou ausência de materialização. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1407–1419`, `app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1466–1467`) |
| **#47884** | Mesmas possibilidades: resposta nas condições do juiz, célula acusando ou ausência de materialização. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1407–1419`, `app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1466–1467`) |

`respondida=True` sozinho não prova passagem pelo ramo da resposta: esse ramo também exige texto não vazio e ausência de decisão administrativa do dia. Esses campos não foram publicados. (`app/chamados/juizes.py:1405–1419`; valores individuais: [nao verificado])

Existe ainda o retorno por falta dentro da janela viva. Com `hoje=2026-11-12`, a janela calculada pelo código é 20/09–20/11, excluindo os quatro dias de agosto; o argumento efetivamente usado pela sonda é **[nao verificado]**. (`app/chamados/juizes.py:592–618`, `app/chamados/juizes.py:1454–1462`, `app/ponto/janelas.py:10–33`, `app/ponto/janelas.py:36–63`)

**Por que C20 não as calou:** nas quatro linhas aparecem `conversa=False`, dia preenchido e `C20=False`; logo, o dado não sustenta “conversa” nem “dia None” como explicação individual. [nao verificado]

O código transforma conversa em dia de turno `None`; para as demais perguntas usa `data_evento`. O juiz começa em `False`, exige dia/colaborador/empresa, procura o PF da competência calculada e, encontrando-o, retorna `not tocou`. (`app/chamados/juizes.py:334–338`, `app/chamados/juizes.py:991–992`, `app/chamados/juizes.py:1023–1027`, `app/chamados/juizes.py:1062–1079`)

Assim, **`C20=False` não identifica sozinho “sem PF” ou “toque posterior”**. O PF efetivamente encontrado, seu `fechado_em` e a trilha humana de cada pergunta são **[nao verificado]**.

Se o juiz encontrou PF vivo para aquele colaborador/dia, seu resultado `False` corresponde a toque reconhecido. Os atos admitidos são `cobrar_dia_manual`, `veredito_celula` e `desarquivar_chamado`, com usuário identificado diferente do sistema e instante estritamente posterior à tranca. **Qual ato ocorreu para #20798, #47879, #47881 e #47884: [nao verificado].** (`app/chamados/juizes.py:1078–1089`, `app/chamados/juizes.py:1252–1254`, `app/chamados/juizes.py:1268`, `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1315`)

4. **Escritores, leitores e significado de dia.**

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `conversa` | `abrir()` passa `False`; `abrir_conversa()` passa `True`; nascimento instancia e salva. (`app/chamados/models.py:2790`, `app/chamados/models.py:2802`, `app/chamados/models.py:3015–3018`) | O juiz usa o discriminador para retirar o dia civil da leitura de turno e da tranca. (`app/chamados/juizes.py:334–338`, `app/chamados/juizes.py:991–992`) |
| `data_evento` | A atribuição central é `gravar_dia_do_fato`; nascimento, costura de linha sem dia e rechaveamento passam por ela. (`app/chamados/services/fio.py:791–802`, `app/chamados/models.py:3017`, `app/chamados/services/fio.py:880`, `app/chamados/services/fio.py:1038`) | Fato recebe **dia do turno**; conversa recebe **dia civil local do pedido**, conservado pelo derivador. (`app/chamados/models.py:2784–2803`, `app/chamados/services/fio.py:1631–1636`) |
| `vivas_apesar_da_tranca` | `medir()` inicializa e acrescenta somente no ramo dos resgatados. (`app/chamados/services/competencia_trancada.py:81`, `app/chamados/services/competencia_trancada.py:119–124`) | `medir_frota()` lê seu tamanho, expõe a contagem por competência e a soma em `ficam_com_o_admin`. (`app/chamados/services/competencia_trancada.py:313–320`) |

**Os leitores não aplicam todos a mesma distinção de dia.** O juiz distingue conversa de turno; `medir()` seleciona pela faixa de `data_evento` cru, sem discriminar conversa, e `reabrir()` também usa esse campo cru. Portanto esses filtros podem incluir o dia civil de uma conversa no universo da competência. (`app/chamados/juizes.py:334–338`, `app/chamados/services/competencia_trancada.py:107–109`, `app/chamados/services/competencia_trancada.py:252–253`)

Outro leitor cru é a lavra, que passa `p.data_evento` a `expira_em`; a documentação dessa função ainda associa conversa a dia `None`, enquanto `abrir_conversa()` grava `localdate()`. (`app/chamados/veredito_lavrado.py:131`, `app/chamados/veredito_lavrado.py:273`, `app/chamados/models.py:2802`)

Na exposição do contador, o comando imprime `ficam_com_o_admin` como `decidir_pelo_dp`, mas seu total principal soma somente chamados, perguntas e disputas; a API entrega `por_competencia`, onde está a contagem própria das vivas. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:20–23`, `app/api/views_mensageria.py:1573–1574`)

A saída informa **`total_erros=0`**. [nao verificado]

## 12/11 17:21 · sessao d3492b1ae160 · - · - · origem=Ronald (tmux)

- **Defeito de universo do contador, quando apresentado como todas as perguntas vivas apesar da tranca.** `medir()` inicializa `vivas_apesar_da_tranca=[]` e só acrescenta perguntas quando o pai ou destino pertence a `resgatados`; o ramo geral acrescenta somente perguntas mortas a `perguntas_lidas_trancadas`, deixando vivas sem inclusão no contador. O próprio comentário exige contador igual à fila; LEI-AKITA 8 exige “contador == universo”. (`app/chamados/services/competencia_trancada.py:77`, `:119`, `:123`, `:124`, `:130`, `:133`; `CLAUDE.md:35`)
- A diferença de universo é mais estreita que “houve toque humano”: `resgatados` contém somente chamados com status VIVO, competência do chamado igual à medida, módulo sujeito à tranca e `resgatado_em > pf.fechado_em`. A exceção do juiz abrange também Cobrar dia e Resolver dia. (`app/chamados/services/competencia_trancada.py:89`, `:93`, `:97`, `:101`, `:105`; `app/chamados/juizes.py:1071`, `:1083`)
- **Não confundir o significado operacional de `perguntas=[]` com o defeito de `vivas_apesar_da_tranca`.** A primeira lista é vazia por construção porque as perguntas saem da fila por leitura e não são mais carimbadas; isso é semântica explicitamente documentada. A segunda pretende enxergar vivas que permanecem na fila, mas só tem o append restrito aos resgatados. (`app/chamados/services/competencia_trancada.py:126`, `:128`, `:120`, `:124`)
- A causa específica de cada `no_medir=False`, além do que literalmente diz a saída fornecida, é **[não verificado]**: não localizei fonte `sonda_v2.py` nesta exploração, e o código da sonda não foi fornecido.

**Escritor e leitores do contador:**

| Papel | Evidência |
|---|---|
| Inicialização e único append da lista | `app/chamados/services/competencia_trancada.py:81`, `:124` |
| `medir_frota()` lê a lista, acrescenta seu tamanho a `ficam_com_o_admin` e expõe `vivas_apesar_da_tranca` por competência | `app/chamados/services/competencia_trancada.py:313`, `:316`, `:317` |
| A soma de frota não inclui chave própria para total `vivas_apesar_da_tranca`; a informação fica dentro de `por_competencia` | `app/chamados/services/competencia_trancada.py:311`, `:318`, `:320` |
| Command lê `medir_frota`, imprime `ficam_com_o_admin` como `decidir_pelo_dp`; total principal soma só chamados + perguntas + disputas | `app/chamados/management/commands/chamados_em_competencia_trancada.py:13`, `:20`, `:23` |
| API lê `medir_frota` e devolve `ficam_com_o_admin` e `por_competencia` | `app/api/views_mensageria.py:1562`, `:1573`, `:1574` |
| Cliente HTTP e ferramenta propagam resposta da API | `mensageria/nucleo/core_client.py:239`, `:241`; `mensageria/nucleo/ferramentas.py:2035`, `:2036` |
| Placar lê somente total principal do command, sem campo próprio de vivas | `bin/placar_code.sh:287`, `:289` |
| Command de encerramento enumera várias listas, mas omite a lista `vivas_apesar_da_tranca` dessa impressão | `app/chamados/management/commands/encerrar_por_competencia_trancada.py:36`, `:38`, `:39` |

O `medir()` usa a janela de fechamento e filtro direto em `data_evento`; para chamados usa `competencia_do_chamado`. O juiz da tranca usa dia do turno e competência calculada por `janela_atual`; assim os leitores não usam todos a mesma expressão de dia, embora uma igualdade entre os dois dias seja mostrada nas quatro linhas da saída fornecida. (`app/chamados/services/competencia_trancada.py:87`, `:93`, `:108`; `app/chamados/juizes.py:984`, `:992`, `:1062`)

## 12/11 17:21 · sessao 2b245d6a8a87 · - · - · origem=Ronald (tmux)

Evidências dos campos:

- `conversa`: `abrir()` passa `False` em `app/chamados/models.py:2790`; `abrir_conversa()` passa `True` e `localdate()` em `app/chamados/models.py:2802`; `_nascer()` instancia e salva em `app/chamados/models.py:3015` e `app/chamados/models.py:3018`. O default é `False` em `app/chamados/models.py:2436`.
- Chamadores de conversa: reabertura administrativa em `app/chamados/services/acoes_disputa.py:263`; emissão sem dia de turno em `app/chamados/services/disputa_emissao.py:431` e `app/chamados/services/disputa_emissao.py:439`; `ancora_ausente` em `app/chamados/services/disputa_emissao.py:1371`.
- `data_evento`: atribuição centralizada em `app/chamados/services/fio.py:801`; nascimento chama e salva em `app/chamados/models.py:3017`; costura de linha sem dia chama em `app/chamados/services/fio.py:880` e salva em `app/chamados/services/fio.py:928`; rechaveamento chama em `app/chamados/services/fio.py:1038` e salva em `app/chamados/services/fio.py:1044`. A costura que data recusa conversa em `app/chamados/services/fio.py:876`; o rechaveamento também em `app/chamados/services/fio.py:1019`.
- Semântica do escritor: fatos recebem dia do turno; conversa recebe dia civil. As duas portas provam essa distinção em `app/chamados/models.py:2790` e `app/chamados/models.py:2802`; o derivador conserva o pedido/gravado da conversa em `app/chamados/services/fio.py:1631`.
- Leitor juiz: `_dia_de_turno_da_pergunta()` transforma conversa em `None` e lê `data_evento` nos demais casos em `app/chamados/juizes.py:334`; a tranca usa esse helper em `app/chamados/juizes.py:991`. `resolvedora_valida()` também reconhece conversa e retorna sem conferir turno em `app/chamados/juizes.py:2260`.
- Leitor `medir()`: filtra `data_evento` cru entre `ini/fim`, sem excluir `conversa`, em `app/chamados/services/competencia_trancada.py:107`. Portanto seu universo pode incluir dia civil de conversa; o universo do juiz distingue conversa. `reabrir()` também filtra o campo cru em `app/chamados/services/competencia_trancada.py:252`.
- Outro leitor cru: `lavrar()` calcula `expira_em(p.data_evento, ...)` em `app/chamados/veredito_lavrado.py:273`; a docstring de `expira_em` ainda associa conversa a dia `None` em `app/chamados/veredito_lavrado.py:131`, enquanto a porta grava `localdate()` em `app/chamados/models.py:2802`. Não afirmar que todos os leitores adotam a mesma interpretação.
- Contador `vivas_apesar_da_tranca`: lista inicializada em `app/chamados/services/competencia_trancada.py:81`; único `append` de produção encontrado no ramo dos chamados resgatados, condicionado ao juiz, em `app/chamados/services/competencia_trancada.py:119` e `app/chamados/services/competencia_trancada.py:124`. No ramo comum, pergunta viva não recebe append; só mortas entram em `perguntas_lidas_trancadas`, em `app/chamados/services/competencia_trancada.py:133`.
- `medir_frota()` lê `len` da lista tanto para `ficam_com_o_admin` quanto para a linha própria do contador em `app/chamados/services/competencia_trancada.py:316`; agrega apenas `ficam_com_o_admin` e os outros três totais em `app/chamados/services/competencia_trancada.py:318`.
- Comando da frota imprime `ficam_com_o_admin` como `decidir_pelo_dp` em `app/chamados/management/commands/chamados_em_competencia_trancada.py:20`; o total final soma somente chamados/perguntas/disputas, omitindo vivas/ficam_com_o_admin, em `app/chamados/management/commands/chamados_em_competencia_trancada.py:23`.

## 12/11 17:21 · sessao c74d56b50639 · - · - · origem=Ronald (tmux)

`pergunta_viva()` delega à lei em `_pergunta_viva_calc()`; com carga usa diretamente o cálculo, sem carga pode reutilizar memo por instância. (`app/chamados/juizes.py:862`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:890`, `app/chamados/juizes.py:894`)

A saída fornecida **não identifica o degrau exato nem o toque humano específico de nenhuma das quatro VIVAs**. [não verificado]

| Pergunta | Degraus compatíveis com os campos publicados, sem horizonte temporal diferente | [nao verificado]
|---|---|
| #20798: respondida=False, materializada=True | Célula acusa → `True` em `app/chamados/juizes.py:1441`; falta na janela viva → `True` em `app/chamados/juizes.py:1462`; carimbo sem materialização de fato → retorno em `app/chamados/juizes.py:1473`. Qual ocorreu: [não verificado]. |
| #47879, #47881, #47884: respondida=True, materializada=False | Resposta **com texto**, sem decisão admin e sem concordância comprovada com a luz → retorno em `app/chamados/juizes.py:1419`; célula acusa → `app/chamados/juizes.py:1441`; falta na janela → `app/chamados/juizes.py:1462`; ausência de materialização → `app/chamados/juizes.py:1467`. Qual ocorreu: [não verificado]. |

O carimbo `respondida=True` sozinho não prova que o ramo da resposta foi tomado: ele exige também texto não vazio e ausência da decisão admin descrita no código. (`app/chamados/juizes.py:1405`, `app/chamados/juizes.py:1407`, `app/chamados/juizes.py:1415`)

O ramo da falta depende da janela anterior/corrente, calculada com `hoje` recebido ou `date.today()`; não é lícito assumir que toda VIVA chegou nesse ramo. (`app/chamados/juizes.py:584`, `app/chamados/juizes.py:592`, `app/chamados/juizes.py:612`, `app/chamados/juizes.py:618`, `app/chamados/juizes.py:1455`)

`C20=False` explica apenas que a tranca não encerrou a pergunta no teste de `app/chamados/juizes.py:1395`; a causa individual não está publicada. [não verificado]

O leitor de dia retorna `None` para conversa e `data_evento` para as demais; `dia_da_pergunta_trancado()` passa esse dia ao juiz em lote. (`app/chamados/juizes.py:334`, `app/chamados/juizes.py:338`, `app/chamados/juizes.py:991`)

As quatro linhas dizem `conversa=False` e exibem dia não nulo, portanto não sustentam “era conversa” ou “dia None” como causa. [não verificado]

O juiz começa em `False`; só considera pares com dia, colaborador e empresa. Calcula a competência pela janela e procura PF vivo; apenas PF encontrado vai ao teste de toque, cujo resultado final é `not tocou`. (`app/chamados/juizes.py:1023`, `app/chamados/juizes.py:1025`, `app/chamados/juizes.py:1062`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:1069`, `app/chamados/juizes.py:1078`)

PF vivo significa empresa/mês/ano correspondentes e `reaberto_em IS NULL`. (`app/ponto/services/fechamento.py:1464`, `app/ponto/services/fechamento.py:1470`)

Assim, `C20=False` sozinho não separa ausência de empresa/PF resolvido de toque posterior. O PF correspondente efetivamente lido e seu `fechado_em` não foram publicados. [não verificado] [nao verificado]

Os atos admissíveis são `cobrar_dia_manual`, `veredito_celula` e `desarquivar_chamado`; a trilha deve ter usuário diferente do sistema e instante estritamente posterior à tranca. (`app/chamados/juizes.py:1083`, `app/chamados/juizes.py:1087`, `app/chamados/juizes.py:1252`, `app/chamados/juizes.py:1268`, `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1315`)

Qual desses atos ocorreu para cada VIVA: **[não verificado]**.

## 12/11 17:16 · sessao 4d8764033314 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · verificador · origem=Code

**REPROVO a pré-verificação:** há ampliação de escrita em **HOJE** pelo cron e D1 permite apagar julgamento sem garantir a trilha obrigatória. Também permanece a classe “relógio fora da borda”. Evidências: [cron](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/ponto/management/commands/gerar_celulas.py:321), [limpeza D1](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/ponto/portas/celula.py:1148), [trilha best-effort](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/core/observ.py:105), [relógio](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/ponto/services/cadastro_futuro.py:22).

**1. Portas e alcance no Salvar**

O suplemento comum seleciona somente células **geradas, existentes, posteriores a hoje, dentro da janela corrente**, respeitando `desde`/`datas`; chama a mesma porta com horizonte desligado. (`app/ponto/services/cadastro_futuro.py:22`, `:24`, `:28`, `:41`)

| Porta | Função/caminho | Alcança futura existente? |
|---|---|---|
| Estrutura: município | `atualizar_posto` / `definir_municipio_posto` → `_evento_do_municipio` → `reavaliar_pela_troca_de_municipio` | Sim. (`app/colaboradores/services/estrutura.py:59`, `:65`, `:88`; `app/colaboradores/services/vinculo.py:947`) |
| Criar/alterar vínculo | `executar_vinculo` → suplemento explícito | Sim. (`app/colaboradores/services/vinculo.py:330`) |
| Alterar vigência por `save` | `_ec_vigencia_regenera_celulas` | Sim. (`app/escala/signals.py:80`) |
| Fechar/encerrar vínculos | `fechar_vigencia`; `encerrar_vinculo` também provoca o signal | Sim. (`app/colaboradores/services/vinculo.py:649`, `:829`; `app/escala/signals.py:81`) |
| Reabrir vigência impossível | `reabrir_vigencia_impossivel` | Sim, no mesmo atomic. (`app/colaboradores/services/vinculo.py:716`, `:737`) |
| Restaurar snapshot | `reverter_para_snapshot` | Sim. (`app/colaboradores/services/vinculo.py:1016`, `:1020`) |
| Trabalhar em feriado | `marcar_trabalha_em_feriado` | Sim. (`app/colaboradores/services/vinculo.py:897`) |
| Âncora | `_ec_ancora_regenera_celulas` | Sim. (`app/escala/signals.py:132`) |
| Tipo pela tela | `salvar_tipo` → `_propagar_futuras_template` | Sim, incluindo vínculos encerrados. (`app/escala/services/cadastro_tipo.py:326`, `:373`) |
| Tipo pelo admin | `save_model` → `salvar_tipo_no_admin` | Sim; acrescenta somente propagação futura. (`app/escala/admin.py:11`; `app/escala/services/cadastro_tipo.py:380`) |
| Folga manual/foto | `lancar_folga`, `alterar_folga`, lote/reparentamento → `relavrar_celulas_da_foto` | Sim. (`app/escala/folgas.py:298`; `app/ponto/tests/test_cadastro_futuro_c3_folgas.py:51`, `:57`, `:71`, `:79`, `:86`) |
| Calendário de folga | Salvar calendário/trocar calendário → `sincronizar_foto_futura_do_calendario` | Sim. (`app/escala/folgas.py:598`, `:638`; `app/colaboradores/services/vinculo.py:459`) |
| Configuração de feriado | Formulário/importação/exclusão → signals → `regenerar_futuras_por_feriado` | Sim, nas datas afetadas, incluindo a data antiga quando movido. (`app/core/views_config.py:70`, `:83`, `:144`, `:201`; `app/core/signals.py:19`, `:33`, `:39`) |
| Escala automática | `aplicar_no_vinculo` / `desfazer` → regeneração explícita | Sim; aqui cria ausentes e estende horizonte por ato declarado. (`app/escala/services/escala_auto_executor.py:107`, `:147`) |
| Correção retroativa | `_aplicar` → trecho histórico + suplemento | Sim. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:231`, `:243`) |

Nos trechos históricos de cadastro examinados, o novo suplemento não amplia o alcance: vínculo mantém início→hoje; município mantém somente hoje quando coberto; template mantém janela anterior→hoje; folgas mantêm o ramo histórico; correção mantém piso/topo anteriores. (`app/colaboradores/services/vinculo.py:319`, `:323`, `:941`; `app/escala/services/cadastro_tipo.py:337`, `:339`, `:351`; `app/escala/folgas.py:290`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:225`, `:228`)

**Há, porém, ampliação proibida na obra:** o cron antigo enumerava somente `ativa=True`; o novo `_regenerar_hoje_existentes` regenera células de **HOJE de vínculo inativo** respondido pela juíza. O teste exige precisamente essa reescrita. (`app/ponto/portas/celula.py:829`, versão anterior no diff fornecido; `app/ponto/management/commands/gerar_celulas.py:316`, `:321`, `:323`; `app/ponto/tests/test_cadastro_futuro_c4.py:165`)

**2. D1: limpeza, trilha e fato humano**

A limpeza ocorre após reescrever DNA no ramo normal, no ramo sem vínculo e na reversão. Anula os 13 campos de julgamento e usa `salvar_e_emitir`, preservando a emissão do evento da célula. (`app/ponto/portas/celula.py:262`, `:307`, `:832`, `:1103`, `:1146`; `:948`)

**Defeito: “com trilha” não está garantido.** A função ignora o retorno de `evento`; esse registrador engole falhas, desfaz apenas a tentativa de trilha e retorna `None`. Assim, a limpeza pode confirmar sem o registro antes/depois. A própria `editar_dia` já contém o tratamento contrário: exige trilha e levanta erro se ela faltar. (`app/ponto/portas/celula.py:1149`; `app/core/observ.py:105`, `:124`, `:139`; `app/ponto/portas/celula.py:643`)

O único detector de decisão humana nessa limpeza é `veredito_via == 'admin'`: conserva o julgamento, registra “conservado” e retorna normalmente **depois de o DNA ter sido escrito**. Não inspeciona o conteúdo da ata nem interrompe o Salvar. Portanto, o comentário que chama isso de “PAREI” descreve conservação/contagem, não uma parada operacional. (`app/ponto/portas/celula.py:301`, `:1128`, `:1136`, `:1140`)

Ausência/folga/atestado externos permanecem intactos: a limpeza altera somente atributos da célula, incluindo sua FK `ausencia`. O teste preserva o objeto externo. Existência de ata contendo fato humano em produção: **[não verificado]**. (`app/ponto/portas/celula.py:1103`, `:1146`; `app/ponto/tests/test_cadastro_futuro_c5.py:165`)

| Escritor que não passa pela limpeza | Constatação | Escopo |
|---|---|---|
| `editar_dia` | Pode escrever DNA futuro e marcar origem editada; não chama a limpeza. (`app/ponto/portas/celula.py:617`, `:633`) | Fora de “Salvar regenerando cadastro”; é edição humana. |
| `julgar_colab` em lote | Os laços recebem células sem a guarda futura do avulso e podem lavrar/carimbar. (`app/ponto/services/cartorio.py:1212`, `:1353`, `:1851`; guarda avulsa `:2025`) | Fora da regeneração de cadastro; continua caminho de julgamento futuro. |
| `veredito_celula` | Pode criar ausência e lavrar julgamento admin sem guarda de data futura. (`app/ponto/services/veredito_celula.py:211`, `:332`, `:351`) | Fora de “Salvar regenerando”; continua capaz de produzir julgamento futuro. |

Esses caminhos não são omissões da chamada D1 no **Salvar regenerando**; são limites da promessa global “futura com ata/carimbo = 0”, que o vigia também observa. (`app/ponto/services/vigia_cadastro_futuro.py:29`, `:45`)

**3. D2 e `substitui_dono`**

Nos eventos de atributo, o suplemento pergunta `vinculos_dos_dias` e permite somente dono igual ao vínculo ou resposta `None`; não calcula outro dono localmente. Portanto, `None` permite seguir para a porta, que verifica cobertura. (`app/ponto/services/cadastro_futuro.py:34`, `:37`; `app/ponto/portas/celula.py:224`)

As chamadas com `substitui_dono=True` encontradas são somente atos de linha do tempo: executar vínculo, fechar/reabrir vigência, restaurar snapshot, correção retroativa e signal de vigência. (`app/colaboradores/services/vinculo.py:335`, `:651`, `:739`, `:1020`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:245`; `app/escala/signals.py:83`)

**Município sem o antigo filtro não é defeito D2.** A chamada de hoje continua condicionada à cobertura hoje; a chamada futura passa pelo suplemento com `substitui_dono=False` e seu filtro de dono. (`app/colaboradores/services/vinculo.py:941`, `:947`; `app/ponto/services/cadastro_futuro.py:34`)

**4. D3, D5, D6, D7 e feriado**

| Item | Verificação |
|---|---|
| D3 | Realizado: futura sem sucessor recebe geradora nula, `montar_dna(None, ...)`, DNA anterior e carimbos de regeneração; a célula permanece. (`app/ponto/portas/celula.py:246`, `:255`, `:256`, `:257`) |
| D5 | Realizado: reabertura e suplemento compartilham o atomic; suplemento só seleciona futuras existentes. (`app/colaboradores/services/vinculo.py:716`, `:737`; `app/ponto/services/cadastro_futuro.py:24`, `:28`) |
| D6 | Realizado: aplicar/desfazer declaram `estender_horizonte=True`; início/volta são futuros. (`app/escala/services/escala_auto_executor.py:100`, `:108`, `:143`, `:148`) |
| D7, corpo do gerador | Realizado: inativo somente em `d > hoje`, escolhido pela juíza; cria ausente e preserva futura existente. (`app/ponto/portas/celula.py:899`, `:904`, `:921`) |
| D7, cron completo | **Não fica restrito a `d > hoje`**: a casca adicional reescreve hoje de inativo, conforme achado 1. (`app/ponto/management/commands/gerar_celulas.py:323`; `app/ponto/tests/test_cadastro_futuro_c4.py:165`) |
| Feriado | Realizado: `DateField.to_python` canoniza na fronteira; salvar entrega datas nova/antiga, excluir entrega a excluída; serviço recebe datas e consulta a juíza. (`app/core/signals.py:19`, `:25`, `:33`, `:39`; `app/ponto/services/cadastro_futuro.py:57`, `:67`) |

**5. Vigia e linha de CADASTRO**

O vigia lê `vinculo_do_dia` com a geradora como alimentação; sem geradora, usa o carregador da mesma autoridade. A candidata (a) não foi implementada: o ramo da geradora continua sem exigir `ativa`. (`app/ponto/services/vigia_cadastro_futuro.py:41`, `:58`; `app/escala/alimentacao.py:269`, `:282`, `:284`)

Vínculo inativo sem fim respondido pela juíza entra em `na_lista_do_admin_c6`; a porta recusa regenerá-lo. A lista administrativa C6 usa o predicado canônico de vínculo sem vigência. Isso classifica CADASTRO, sem cura automática. (`app/ponto/services/vigia_cadastro_futuro.py:69`; `app/ponto/portas/celula.py:148`; `app/escala/services/cadastro_realidade.py:320`, `:829`)

Os quatro indicadores têm esperado zero; o cron mede antes da geração, lavra depois, e a API lê snapshot. Ausência de medição retorna `None`, não zero. O **N de produção exigido antes de construir**, comprovado pelo dado raspado fornecido: **[não verificado]**. (`app/ponto/services/vigia_cadastro_futuro.py:86`, `:99`, `:119`; `app/ponto/management/commands/gerar_celulas.py:346`, `:353`; `app/api/views_mensageria.py:647`)

**6. Quem escreve e quem lê os campos**

| Campo | Escritores persistentes | Leitores identificados |
|---|---|---|
| `CelulaDia.ata` | Lavra, carimbo de precedência e limpeza D1, todos na porta. (`app/ponto/portas/celula.py:1055`, `:1082`, `:1187`, `:1146`) | Grade/folha, cartório, chamados, relatórios e vigia. (`app/escala/services/leitor_celula.py:379`; `app/folha/previa.py:83`; `app/ponto/services/cartorio.py:451`; `app/chamados/juizes.py:450`; `app/relatorios/cartao_pela_celula.py:287`; `app/ponto/services/vigia_cadastro_futuro.py:45`) |
| `dna` | Conteúdo produzido por `montar_dna`; persistido por geração, regeneração, edição humana e reversão. (`app/ponto/management/commands/gerar_celulas.py:73`; `app/ponto/portas/celula.py:256`, `:301`, `:633`, `:819`, `:904`, `:915`) | Marcos/pausas da escala, grade, cartório, julgamento admin, chamados e vigia. (`app/escala/models.py:1124`, `:1189`, `:1235`; `app/escala/utils.py:46`; `app/ponto/services/cartorio.py:1851`; `app/ponto/services/veredito_celula.py:31`; `app/chamados/juizes.py:948`; `app/ponto/services/vigia_cadastro_futuro.py:60`, `:79`) |
| `regenerada_em` | Regeneração e reversão. (`app/ponto/portas/celula.py:258`, `:303`, `:826`) | Cobrança órfã/carimbos de reescrita, fase, lavra e cartório. (`app/escala/services/regeneracao.py:84`, `:101`, `:119`; `app/escala/services/fase_12x36.py:189`; `app/chamados/veredito_lavrado.py:193`; `app/ponto/services/cartorio.py:393`) |
| `regeneracoes` | Incrementos na regeneração e reversão. (`app/ponto/portas/celula.py:259`, `:304`, `:827`) | Os próprios incrementos leem o valor anterior; outro consumidor de produção: **[não verificado]**. |
| `dna_anterior` | Regeneração guarda; reversão limpa. (`app/ponto/portas/celula.py:257`, `:302`, `:820`) | Reversão e comando operacional. (`app/ponto/portas/celula.py:810`, `:816`; `app/ponto/management/commands/aplicar_col900.py:173`) |
| `EscalaColaborador.data_fim` | Porta de vínculo: reativar, fechar, reabrir, encerrar, corrigir/criar trecho, snapshot e desfazer automático. (`app/colaboradores/services/vinculo.py:271`, `:629`, `:717`, `:831`, `:958`, `:968`, `:1018`, `:1088`) | Cobertura, vínculo do dia, candidatura, signals e cadastro. (`app/escala/alimentacao.py:150`, `:189`, `:205`, `:228`; `app/escala/signals.py:25`, `:57`; `app/escala/services/cadastro_realidade.py:306`) |
| `EscalaColaborador.ativa` | Porta de vínculo, incluindo criação, absorção e restauração. (`app/colaboradores/services/vinculo.py:270`, `:286`, `:629`, `:717`, `:786`, `:830`, `:959`, `:1018`, `:1075`, `:1088`, `:1094`) | Candidatura/cobertura, vínculo do dia, D7 e cadastro. (`app/escala/alimentacao.py:150`, `:189`, `:205`, `:228`; `app/ponto/portas/celula.py:899`; `app/escala/services/cadastro_realidade.py:256`, `:306`) |

A tabela identifica escritores e leitores concretos; **censo global exaustivo de todos os consumidores de `ata`/`dna` e, especialmente, `data_fim`/`ativa`: [não verificado]**.

O JUIZES confirma autoridades distintas para vínculo do dia e cobertura, além de grade/precedência/ausência/feriado. Não autoriza substituir uma pergunta pela outra. (`app/docs/JUIZES.md:103`, `:105`, `:107`, `:108`, `:121`, `:123`)

**7. Classes de reprovação**

| Classe | Presença/ausência demonstrada |
|---|---|
| Segundo juiz | Ausente nos dois serviços novos: usam autoridades existentes de vínculo, cobertura e produtor de DNA. (`app/ponto/services/cadastro_futuro.py:35`, `:41`, `:67`; `app/ponto/services/vigia_cadastro_futuro.py:26`, `:58`, `:79`) |
| Segundo escritor fora da porta | Ausente nesses serviços: suplemento chama a porta; vigia não salva células. (`app/ponto/services/cadastro_futuro.py:41`; `app/ponto/services/vigia_cadastro_futuro.py:43`, `:83`) |
| Relógio fora da borda | **Presente:** defaults leem `localdate()` no interior; há chamadores reais sem `hoje`. É exatamente a armadilha descrita no plano. (`app/ponto/services/cadastro_futuro.py:22`, `:53`; `app/core/signals.py:33`; `app/escala/services/cadastro_tipo.py:386`; `app/docs/PLANO-PADROES.md:92`, `:94`) |
| Derivação paralela da fonte única | Ausente na decisão de vínculo dos serviços novos; comparação de cadastro reutiliza `montar_dna`. (`app/ponto/services/cadastro_futuro.py:36`; `app/ponto/services/vigia_cadastro_futuro.py:58`, `:79`) |
| Selo/RED sem mordida | Existem mutantes executáveis D1/D3 e D2/D5/D6/D7/feriado; **não cobrem a exigência de abortar quando falta trilha D1**. O teste de admin exige conservação com selo 1. (`app/ponto/tests/test_cadastro_futuro_c5.py:27`, `:151`, `:199`; `app/ponto/tests/test_cadastro_futuro_c5b.py:30`; `app/ponto/portas/celula.py:1149`) |
| Ordem de trava | Snapshot executa vínculo→célula; há teste dessa sequência. Concorrência real PostgreSQL: **[não verificado]**. (`app/colaboradores/services/vinculo.py:1005`, `:1011`; `app/ponto/tests/test_cadastro_futuro_c3.py:228`) |
| Pouso antes do pré-requisito | Pouso, ordem efetiva E3/E4B e certificação completa: **[não verificado]** pelo material autorizado. | [nao verificado]
| Campo com dois significados | `ativa` e vigência têm usos distintos explícitos; a exceção da geradora inativa sem fim segue o aval (b), com C6. Não demonstrado novo conflito semântico nos carimbos de regeneração. (`app/escala/alimentacao.py:189`, `:205`, `:282`; `app/ponto/services/vigia_cadastro_futuro.py:69`; `app/escala/models.py:1714`) |

**8. Leis dos sítios**

- **L-007: violação demonstrada na garantia de trilha D1.** O ato crítico pode confirmar sem antes/depois persistidos. (`CLAUDE.md:32`; `app/ponto/portas/celula.py:1149`; `app/core/observ.py:139`)
- **Proibição expressa do aval de ampliar `d <= hoje`: violada pela casca do cron**, conforme o caso de vínculo inativo descrito no item 1. (`app/ponto/management/commands/gerar_celulas.py:323`; `app/ponto/tests/test_cadastro_futuro_c4.py:165`)
- **L-020:** denúncia da reescrita futura está presente. (`app/ponto/portas/celula.py:257`, `:258`, `:259`, `:302`)
- **L-002:** vigia e suplemento consultam as autoridades existentes. (`app/ponto/services/vigia_cadastro_futuro.py:58`, `:79`; `app/ponto/services/cadastro_futuro.py:36`)
- **L-052:** guardas de lavra e célula humana permanecem na porta; abertura continua exigindo o ato `apesar_da_lavra`. (`app/ponto/portas/celula.py:213`, `:248`, `:279`, `:292`)
- **L-090:** janela vem do juiz de competência; mês civil da foto de folga serve à fase mensal, explicitamente. (`app/ponto/management/commands/gerar_celulas.py:290`; `app/escala/folgas.py:629`)
- Violação nova concreta de **L-006, L-009, L-023, L-050, L-071, L-086 ou L-091** nesta leitura: **[não verificado]**.

## 12/11 17:14 · sessao 18d0805c03a7 · - · - · origem=Ronald (tmux)

5. Vigia realizado conforme (b), sem cura:

- `medir` recebe `hoje`, seleciona `data > hoje`; selo D1 conta todas futuras, demais baldes só geradas (`app/ponto/services/vigia_cadastro_futuro.py:24`, `:31`, `:45`, `:49`).
- Com geradora, alimenta `vinculo_do_dia`; sem geradora, usa `vinculos_dos_dias` (`app/ponto/services/vigia_cadastro_futuro.py:41`, `:58`). O carregador delega à mesma juíza (`app/escala/alimentacao.py:358`).
- A juíza conserva geradora inativa sem fim se as datas não a excluem; a candidata (a) não foi implementada (`app/escala/alimentacao.py:269`, `:282`, `:284`).
- Inativa sem fim vira `na_lista_do_admin_c6`, fora do contador estrutural esperado zero (`app/ponto/services/vigia_cadastro_futuro.py:69`). A porta recusa regenerá-la (`app/ponto/portas/celula.py:148`). A lista real C6 consulta o predicado canônico, formata por vínculo e agrega ao cadastro (`app/escala/services/cadastro_realidade.py:320`, `:331`, `:829`).
- Os quatro indicadores levam esperado zero; cron mede antes de gerar e lavra depois; API lê o snapshot, ausência é `None`, nunca zero (`app/ponto/services/vigia_cadastro_futuro.py:86`, `:99`, `:119`; `app/ponto/management/commands/gerar_celulas.py:346`, `:353`; `app/api/views_mensageria.py:647`).
- N de produção exigido antes de construir não consta do dado fornecido [não verificado]. Há relato local afirmando N=4, 143 células/51 futuras, mas não considero esse relato substituto da sonda raspada autorizada (`app/docs/RELATO.md:15`).

7. Classes:

- **Relógio fora da borda: PRESENTE, prova direta.** A lei manda ler uma vez na borda e condena explicitamente `hoje=None` lendo por dentro (`app/docs/GLOSSARIO.md:19`; `app/docs/PLANO-PADROES.md:92`, `:94`). Ambos novos serviços fazem isso (`app/ponto/services/cadastro_futuro.py:22`, `:53`). Caminhos efetivos sem injeção: sinais de feriado (`app/core/signals.py:33`, `:39`); admin de template → propagador com `hoje=None` (`app/escala/services/cadastro_tipo.py:386`, `:368`, `:375`); fechar/reabrir vigência (`app/colaboradores/services/vinculo.py:649`, `:737`). O vigia e o command estão corretos neste ponto (`app/ponto/services/vigia_cadastro_futuro.py:24`; `app/ponto/management/commands/gerar_celulas.py:339`, `:346`).
- **Segundo juiz/derivação paralela: ausentes nos dois novos serviços inspecionados.** Eles chamam autoridades existentes de vínculo e produtor de DNA (`app/ponto/services/cadastro_futuro.py:35`, `:41`, `:67`; `app/ponto/services/vigia_cadastro_futuro.py:26`, `:58`, `:76`, `:79`). Executei o predicado real `_so_alimenta` sobre ambos; nenhum tomador por fora encontrado; definição do censo em `app/escala/tests/test_contract_juiz_escala.py:97`, `:124`.
- **Selo sem mordida: D1/D3 têm mordida demonstrada nos ensaios.** Fontes dos mutantes e aplicação em memória: `app/ponto/tests/test_cadastro_futuro_c5.py:27`, `:74`; `app/ponto/tests/test_cadastro_futuro_c5b.py:30`, `:65`. Resultado observado na sessão, sem arquivo gerado: seis módulos, **113 testes OK**; **24/24 mutantes c5+c5b rc=1**, dos quais 21 por AssertionError e três por erro semântico do corpo mutado. D1/D3: todos os 14 por asserção. Persistência dessa saída em arquivo [não verificado].
- Os três erros semânticos foram: D2 que remove teste de `None` → `None.pk` em `app/ponto/services/cadastro_futuro.py:38`; feriado salvo/excluído sem canonização → comparação `date < str` em `:57`. Não foram importação ou fixture.
- **Ordem de travas:** snapshot declara e executa vínculo→célula (`app/colaboradores/services/vinculo.py:1005`, `:1012`); suplemento trava células antes de regenerar (`app/ponto/services/cadastro_futuro.py:28`). Prova global de concorrência PostgreSQL [não verificado]; os testes assumem ORM em memória (`app/ponto/tests/test_cadastro_futuro_c3.py:1`).
- **Pouso antes do pré-requisito:** não se comprova pouso no dado fornecido [não verificado]. Relato local diz E4B já pousada antes de abrir construção (`app/docs/RELATO.md:97`); regra exige suíte inteira antes do opus (`CLAUDE.md:377`). Execução offline não certifica suíte inteira.
- **Campo com dois significados:** a distinção `ativa`/vigência está explicitada em duas autoridades diferentes por pergunta, não autoriza trocar uma pela outra (`app/escala/alimentacao.py:189`, `:205`, `:228`; `app/docs/JUIZES.md:121`). O caso inativo sem fim conserva dono e ganha C6, conforme aval (b) (`app/ponto/services/vigia_cadastro_futuro.py:69`).

8. Leis dos sítios:

- A violação nova demonstrada é **Clock Injection** acima (`app/docs/PLANO-PADROES.md:92`, `:94`; `app/ponto/services/cadastro_futuro.py:22`, `:53`).
- Evento no Salvar e cron vigia seguem o desenho de `CLAUDE.md:171`; testemunha lê a autoridade em `app/ponto/services/vigia_cadastro_futuro.py:58`; preservação do cadastro C6 segue L-099 em `CLAUDE.md:179`.
- L-020 denuncia a reescrita futura com DNA anterior/instante/contador (`app/ponto/portas/celula.py:256`, `:258`, `:259`, `:302`). Guardas de editada/exportada continuam no escritor (`app/ponto/portas/celula.py:248`, `:250`, `:285`, `:289`).
- L-090: janela das futuras vem do `_janela_default` e autoridade de janela; mês civil da foto tem propósito explícito de fase mensal (`app/ponto/services/cadastro_futuro.py:23`; `app/ponto/management/commands/gerar_celulas.py:291`; `app/escala/folgas.py:629`).
- Nenhuma violação concreta nova de L-086/L-091/L-071 provada nesta leitura [não verificado]; não ampliaria conclusões sobre cálculo/front com esses ensaios sem banco.

## 12/11 17:14 · sessao 2f1c44e0a6a0 · - · - · origem=Ronald (tmux)

Achado concreto: a casca do cron amplia o universo de escrita de HOJE para vínculo inativo. O novo `_regenerar_hoje_existentes` seleciona toda célula gerada de HOJE e regenera pelo vínculo respondido pelo juiz, sem exigir `ativa=True` (`app/ponto/management/commands/gerar_celulas.py:316`, `:321`, `:323`). O teste fixa o vínculo inativo (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:282`) e exige que a célula de HOJE seja reescrita por ele (`app/ponto/tests/test_cadastro_futuro_c4.py:165`, `:168`, `:171`). Executei esse teste isolado com `python3 -B`, sem banco: 1 teste, OK, rc 0; a execução não tem arquivo de evidência persistido [não verificado]. O diff fornecido mostra que o loop antigo enumerava somente `ativa=True`; portanto esse caso era excluído antes [não verificado: referência histórica disponível apenas no diff do prompt].

Censo das portas:

| Porta | Caminho que alcança futura existente | Alcance anterior preservado |
|---|---|---|
| Estrutura: município | `atualizar_posto` / `definir_municipio_posto` → evento → `reavaliar_pela_troca_de_municipio` (`app/colaboradores/services/estrutura.py:59`, `:65`, `:88`; `app/colaboradores/services/vinculo.py:947`) | Chamada direta permanece somente HOJE e quando cobre HOJE (`app/colaboradores/services/vinculo.py:941`) |
| Executar vínculo | Suplemento explícito após cadastro (`app/colaboradores/services/vinculo.py:333`) | Trecho `data_inicio..hoje`, horizonte desligado (`:319`, `:323`) |
| Encerrar vínculo | `save` em `encerrar_vinculo` → signal de vigência (`app/colaboradores/services/vinculo.py:829`; `app/escala/signals.py:81`) | Signal limita ramo histórico a HOJE (`app/escala/signals.py:72`) |
| Fechar vigência/desligamento | `fechar_vigencia` → suplemento; desligamento delega a ele (`app/colaboradores/services/vinculo.py:649`, `:849`) | Suplemento começa após menor fim antigo/novo e somente futuro (`:648`; `app/ponto/services/cadastro_futuro.py:24`) |
| Reabrir vigência impossível | Update e suplemento no mesmo atomic (`app/colaboradores/services/vinculo.py:716`, `:737`) | Somente futuras existentes pelo suplemento (`app/ponto/services/cadastro_futuro.py:24`, `:28`) |
| Snapshot | Restauração → suplemento (`app/colaboradores/services/vinculo.py:1016`, `:1020`) | Suplemento somente futuro (`app/ponto/services/cadastro_futuro.py:24`) |
| Flag trabalha em feriado | Chamada histórica separada do suplemento (`app/colaboradores/services/vinculo.py:891`, `:897`) | Janela anterior até HOJE (`:886`, `:888`, `:892`) |
| Âncora | Signal → suplemento (`app/escala/signals.py:132`) | Janela atual até HOJE (`:120`, `:125`) |
| Tipo pela tela | `salvar_tipo` → propagação histórica + propagação futura (`app/escala/services/cadastro_tipo.py:321`, `:326`) | Ativos, janela anterior até HOJE (`:337`, `:339`, `:351`) |
| Tipo pelo admin | `salvar_tipo_no_admin` → somente propagação futura (`app/escala/services/cadastro_tipo.py:380`, `:386`) | Nenhuma chamada histórica nova nesse caminho (`:382`) |
| Folga manual/foto | Portas convergem em `relavrar_celulas_da_foto`, que chama suplemento (`app/escala/folgas.py:215`, `:298`, `:337`) | Ramo histórico conserva desde até HOJE; rejulgamento mantém intervalo vazio específico (`:284`, `:290`, `:294`) |
| Calendário de folga | Editor → sincronização futura → suplemento (`app/escala/folgas.py:598`, `:638`); troca de calendário também chama sincronização (`app/colaboradores/services/vinculo.py:459`) | Fotos novas recortadas para amanhã em diante, preservando manuais (`app/escala/folgas.py:184`, `:187`, `:632`) |
| Feriado criar/editar/importar/excluir | ORM dispara signals → serviço futuro (`app/core/views_config.py:70`, `:83`, `:144`, `:201`; `app/core/signals.py:33`, `:39`) | Serviço aceita exclusivamente `hoje < data` (`app/ponto/services/cadastro_futuro.py:57`) |
| Escala auto aplicar/desfazer | Regeneração explícita com `estender_horizonte=True` (`app/escala/services/escala_auto_executor.py:107`, `:147`) | Início estritamente futuro / volta a partir de amanhã (`:100`, `:143`) |
| Corrigir escala retroativa | Suplemento após trecho histórico (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:243`) | Mesmo piso e topo limitado a HOJE (`:225`, `:228`, `:231`) |

D2: eventos de atributo consultam `vinculos_dos_dias` e só permitem dono igual ao vínculo ou juiz calado (`app/ponto/services/cadastro_futuro.py:34`). `substitui_dono=True` aparece somente em executar vínculo, fechar/reabrir vigência, snapshot, correção retroativa e signal de vigência (`app/colaboradores/services/vinculo.py:335`, `:651`, `:739`, `:1020`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:245`; `app/escala/signals.py:83`).

Município sem o antigo filtro sobre o universo futuro não é defeito D2: apenas a chamada de HOJE exige cobertura HOJE; o suplemento futuro mantém `substitui_dono=False` e aplica o filtro do juiz (`app/colaboradores/services/vinculo.py:941`, `:947`; `app/ponto/services/cadastro_futuro.py:34`).

D3: a futura encerrada sem sucessor recebe geradora nula, DNA produzido por `montar_dna(None, ...)` e denúncia de regeneração (`app/ponto/portas/celula.py:246`, `:255`, `:256`, `:257`). O produtor recusa `trabalha=True` sem vínculo (`app/ponto/management/commands/gerar_celulas.py:80`).

D5: realizado dentro do mesmo atomic do update (`app/colaboradores/services/vinculo.py:716`, `:737`).

D6: ambas chamadas declaram horizonte explicitamente e mantêm o efeito estritamente futuro (`app/escala/services/escala_auto_executor.py:100`, `:108`, `:143`, `:148`).

D7: o corpo `gerar_celulas_periodo` só admite inativo em `d > hoje`, mediante juiz, cria ausente e preserva futura existente (`app/ponto/portas/celula.py:899`, `:904`, `:921`). Isso não sana o achado separado da casca do cron para HOJE (`app/ponto/management/commands/gerar_celulas.py:323`).

Feriado: a fronteira converte pela autoridade `DateField.to_python`, preserva a data anterior e entrega datas ao serviço; salvar e excluir passam por ela (`app/core/signals.py:19`, `:25`, `:33`, `:39`). O serviço não converte representação e consulta `vinculo_do_dia` (`app/ponto/services/cadastro_futuro.py:57`, `:67`).

Não classificar `marcar_opera_em_feriado` como defeito de regeneração do DNA existente: ele grava o posto (`app/colaboradores/services/estrutura.py:82`), e a leitura para DNA documentada é o default de vínculo NOVO (`app/escala/models.py:905`, `:910`, `:911`; `app/colaboradores/services/vinculo.py:290`). `atualizar_empresa` também não dispara regeneração (`app/colaboradores/services/estrutura.py:18`), mas mudança de `dia_inicio_competencia` altera a autoridade da janela, não campo do produtor de DNA (`app/ponto/janelas.py:36`; `app/ponto/management/commands/gerar_celulas.py:104`); defeito nesta obra [não verificado].

## 12/11 17:14 · sessao 19a0673d17e0 · - · - · origem=Ronald (tmux)

Achados D1:

- A limpeza fica em `app/ponto/portas/celula.py:1115`; é chamada depois da reescrita normal (`:301-310`), no ramo futura sem vínculo (`:253-263`) e ao reverter regeneração (`:819-836`). Limpa os 13 campos de `CAMPOS_DO_JULGAMENTO` (`:1103-1104`), conserva antes/depois em evento (`:1146-1152`) e emite outbox pelo `salvar_e_emitir` (`:948-956`).
- **Defeito comprovado: trilha não é obrigatória na execução.** `desfazer_julgamento_da_futura` ignora o retorno de `evento` (`app/ponto/portas/celula.py:1149`); esse escritor engole falha com savepoint e retorna `None`, deixando o negócio confirmar (`app/core/observ.py:94-105`, `:124-139`). Diferentemente, `editar_dia` exige retorno da trilha e levanta erro (`app/ponto/portas/celula.py:643-654`).
- **PAREI literal não implementado.** O único detector humano é `cel.veredito_via == 'admin'`; ele conserva tudo, registra evento e retorna normalmente (`app/ponto/portas/celula.py:1140-1145`), depois do DNA já ter sido escrito (`:301-310`). Não há inspeção do conteúdo da ata (`:1136-1147`). A ocorrência de ata com fato humano em produção continua [não verificado].
- O fato externo Ausencia/folga/atestado não é apagado por essa função: ela só anula atributos da própria CelulaDia, inclusive a FK ausência (`app/ponto/portas/celula.py:1103-1104`, `:1146-1148`); o teste demonstra preservar o objeto externo (`app/ponto/tests/test_cadastro_futuro_c5.py:165-172`).
- Executei ensaio `python3 -B` puramente em memória com a fixture `C5Test.setUp` (`app/ponto/tests/test_cadastro_futuro_c5.py:93`), `julgar` (`:114`) e `regenerar` (`:127`): retorno da trilha forçado a `None` → regeneração retorna 1 e ata fica `None`; via admin → regeneração retorna 1, troca geradora para 2 e conserva ata admin. Isso reproduz os dois comportamentos acima, sem provar ocorrência em produção. O teste existente exige conservação e selo 1, em vez de bloqueio (`:199-208`).

Escritores futuros que não passam pela limpeza:

| Escritor | Evidência | Enquadramento |
|---|---|---|
| `editar_dia` | Escreve DNA e origem editada, sem limite futuro nem limpeza (`app/ponto/portas/celula.py:617-642`); sua cauda não julga futuras (`:459-465`). | Fora do escopo literal “Salvar regenerando cadastro”; pode conservar ata antiga ao mudar DNA humano. |
| `julgar_colab` | Aceita lista recebida (`app/ponto/services/cartorio.py:940`); laços não filtram futuras (`:1212`, `:1353`), lavra e carimba (`:1851-1874`). | Não regenera DNA e não é Salvar cadastro; a guarda futura existe no avulso `julgar_celula` (`:2025-2026`), não no lote. |
| `veredito_celula` | Serviço não verifica data futura antes de executar (`app/ponto/services/veredito_celula.py:211-227`); pode criar ausência (`:332-339`) e lavra admin (`:351-352`). | Fora de “Salvar regenerando”; é caminho que pode criar julgamento futuro. |

“Quem escreve / quem lê”:

| Campo | Escritores persistentes encontrados | Leitores identificados |
|---|---|---|
| `CelulaDia.ata` | `lavrar_veredito` (`app/ponto/portas/celula.py:1055`, `:1082`), `carimbar_precedencia` (`:1187-1193`), limpeza D1 (`:1146-1148`). Cartório usa a porta (`app/ponto/services/cartorio.py:1853`); admin usa a porta (`app/ponto/services/veredito_celula.py:352`). | Grade/folha `app/escala/services/leitor_celula.py:379`; `app/folha/previa.py:83`; `app/folha/validacao_pdf.py:47`; relatórios `app/relatorios/cartao_pela_celula.py:287`, `app/relatorios/dia_da_frota.py:61`; chamados `app/chamados/juizes.py:450`, `:493`, `:966`, `:1344`, `app/chamados/services/materializacao.py:172`, `app/chamados/services/disputa_emissao.py:940`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/reconciliador.py:701`, `:879`; pautas `app/pautas/services.py:432`; feriado do posto `app/colaboradores/services/feriado_do_posto.py:97`; batida `app/ponto/juiz_batida.py:157`; lastro `app/ponto/services/lastro.py:89`; worklist `app/ponto/worklist.py:22`; cluster `app/ponto/services/cluster_espurio.py:27`; cartório `app/ponto/services/cartorio.py:451`, `:1440`; vigias `app/ponto/services/vigia_entrada.py:80`, `:123`, `app/ponto/services/vigia_cadastro_futuro.py:21`, `:45`, `app/ponto/services/vigia_pareador_celula.py:71`; diagnósticos `app/ponto/services/diagnostico_escala.py:156`, `app/ponto/services/esmeril_espelho.py:125`; comandos `app/ponto/management/commands/vigia_b5_ausencia.py:79`, `app/ponto/management/commands/backfill_b5_insumos.py:73`, `app/ponto/management/commands/tripwire_celulas.py:258`, `:341`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:61`, `app/ponto/management/commands/detectar_intervalo_ausente.py:47`, `app/ponto/management/commands/curar_residuo_bug115.py:45`, `:145`, `app/ponto/management/commands/celula_veredito_velho.py:108`; `app/folha/services/sem_furo.py:22`. |
| `CelulaDia.dna` | Produtor do conteúdo: `app/ponto/management/commands/gerar_celulas.py:73`. Gravação: regeneração `app/ponto/portas/celula.py:256`, `:273`, `:288-289`, `:301`; edição `:633-642`; reversão `:819`; criação/backfill do gerador `:904-917`. | Marcos/pausas/extra `app/escala/models.py:1124`, `:1189`, `:1235`; utilitários `app/escala/utils.py:46`, `:97`, `:130`, `:166`; cartório `app/ponto/services/cartorio.py:1251`, `:1257`, `:1398`, `:1437`, `:1851`, `:1872`; admin `app/ponto/services/veredito_celula.py:31`; vigia novo `app/ponto/services/vigia_cadastro_futuro.py:60`, `:79`; juiz de batida `app/ponto/juiz_batida.py:158`; chamados `app/chamados/juizes.py:948`, `:1338`; pautas `app/pautas/services.py:424`; dia `app/ponto/services/dia_do_colab.py:46`; diagnósticos `app/ponto/services/diagnostico_escala.py:153`, `app/ponto/services/esmeril_espelho.py:116`; vigia entrada `app/ponto/services/vigia_entrada.py:126`; faltas hoje `app/ponto/services/faltas_de_hoje.py:43`; comandos `app/ponto/management/commands/tripwire_celulas.py:30`, `:99`, `:259`, `app/ponto/management/commands/detectar_intervalo_ausente.py:53`, `app/ponto/management/commands/celulas_contra_o_template.py` [não verificado: caminho correto é escala/management/commands/celulas_contra_o_template.py:46-49], `app/ponto/management/commands/censo_col900.py:119`, `app/ponto/management/commands/censo_fase_12x36.py:392`, `app/ponto/management/commands/aplicar_col900.py:76`, `:172`. |
| `regenerada_em` | Regeneração `app/ponto/portas/celula.py:258`, `:303`; reversão `:826`. | `app/escala/services/regeneracao.py:84`, `:101-105`, `:119-122`; `app/escala/services/fase_12x36.py:189`; `app/chamados/veredito_lavrado.py:193`; `app/ponto/services/cartorio.py:393`; `app/ponto/services/dia_do_colab.py:94`. |
| `regeneracoes` | Regeneração incrementa `app/ponto/portas/celula.py:259`, `:304`; reversão incrementa `:827`. | Próprios incrementos leem o valor anterior (`:259`, `:304`, `:827`); outro leitor de produção encontrado [não verificado]. |
| `dna_anterior` | Regeneração guarda anterior `app/ponto/portas/celula.py:257`, `:302`; reversão limpa `:820`. | Reversão `app/ponto/portas/celula.py:810`, `:816`; comando operacional `app/ponto/management/commands/aplicar_col900.py:173`. |
| `EscalaColaborador.data_fim` | Todas as atribuições EC localizadas ficam na porta de vínculo: reativação `app/colaboradores/services/vinculo.py:271`; fechar `:629`; reabrir `:717`, `:731`; encerrar `:831-832`; criar encerrada/corrigir `:958`, `:968`; snapshot `:1018`; desfazer auto `:1088`. | Juízes `app/escala/alimentacao.py:189`, `:205`, `:228`; candidatura em lote `:150`; signals fotografam/comparecem `app/escala/signals.py:25-28`, `:57-60`, `:67`; portas usam os valores anteriores e os novos (`app/colaboradores/services/vinculo.py:698`, `:731`, `:958`); cadastro realidade `app/escala/services/cadastro_realidade.py:306-315`. Censo global de todos os leitores de data_fim não fechado [não verificado]. |
| `EscalaColaborador.ativa` | Porta de vínculo: reativação `app/colaboradores/services/vinculo.py:270`; criação `:286`, `:978`, `:1075`, `:1094`; fechar `:629`; reabrir `:717`, `:732`; absorver `:786`; encerrar `:830-832`; criar encerrada `:959`; snapshot `:1018`; desfazer auto `:1088`. | Vigência declarada/cobertura e vínculo do dia `app/escala/alimentacao.py:189`, `:205`, `:228`; candidatura `:150`; gerador D7 `app/ponto/portas/celula.py:899`; leitores administrativos/cadastro `app/escala/services/cadastro_realidade.py:256`, `:306-315`. Censo global de todos os leitores de ativa não fechado [não verificado]. |

As atribuições de ata fora da porta em `app/escala/utils.py:1887` são sobre cópia em memória nunca salva (`:1886`); a equivalente da porta também declara cópia de leitura (`app/ponto/portas/celula.py:981-996`). Não são segundo escritor persistente por essa evidência.

Autoridades do JUIZES: marcos congelados na EC (`app/docs/JUIZES.md:22-23`); dia de trabalho/precedência/ausência/feriado (`:103-109`); folga manual da pessoa versus calendário do vínculo (`:113-119`); vínculo do dia e cobertura (`:121-125`). A denúncia da regeneração não muda significado: `regenerada_em` exclui edição humana (`app/escala/models.py:1718-1721`), `regeneracoes` conta reescritas (`:1723-1724`) e `dna_anterior` guarda somente a última versão (`:1714-1729`).

## 12/11 17:03 · sessao 5849d664db84 · TIPO-NA-FONTE · censo · origem=Code

A cópia permite confirmar **12 chaves de topo na ata** e **9 no DNA**, pelos escritores identificados; não sustenta uma enumeração de 72 chaves de topo (`app/ponto/services/cartorio.py:738`, `app/ponto/services/cartorio.py:767`, `app/ponto/portas/celula.py:1084`, `app/ponto/management/commands/gerar_celulas.py:95`, `app/ponto/management/commands/gerar_celulas.py:122`).

**Identidade desta cópia com o HEAD do main: [nao verificado].** O registro local menciona o push `38605f141`, mas isso não autentica a árvore recebida (`app/docs/RELATO.md:32`).

O script foi executado em memória, sem instalação. Resultados dessa execução sem artefato persistido: **[nao verificado]** para conferência externa. Abaixo estão o inventário verificável no código e o texto integral executado.

Para as contagens, a unidade é **operação AST com chave literal de topo**. Incluí `management`, construtores e cópias em memória; separei testes e excluí migrations. Cópias integrais, chaves dinâmicas e subchaves aparecem separadamente no instrumento. **Não apresento esse perímetro como prova de cobertura irrestrita de Python dinâmico. [nao verificado]** [nao verificado]

| Medida | Resultado no perímetro | Diferença aritmética do aval | Evidência |
|---|---:|---:|---|
| Chaves de topo da ata | 12 | −60 | `app/ponto/services/cartorio.py:738`, `app/ponto/services/cartorio.py:767`, `app/ponto/portas/celula.py:1084` |
| Acessos literais de topo da ata | 95 em 27 arquivos | −79 acessos; +6 arquivos | Inventário abaixo; escritores em `app/ponto/services/cartorio.py:738` e leitores em `app/escala/services/leitor_celula.py:68` |
| Chaves de topo do DNA | 9 | Aval sem número | `app/ponto/management/commands/gerar_celulas.py:95`, `app/ponto/management/commands/gerar_celulas.py:122` |
| Acessos literais de topo do DNA | 41 em 19 arquivos | Aval sem número | Inventário abaixo, incluindo restauração em `app/ponto/portas/celula.py:788` |
| `strftime('%H:%M')` em `app/` | 200 chamadas | +39 | Relação integral em C |
| Mesmo formato fora de `app/` | +1 chamada | Total 201; +40 | `bin/disjuntor.py:2266` |

A equivalência entre esses recortes e os greps do aval é **[nao verificado]**. Portanto, **95 não substitui silenciosamente a catraca 174**. Há operações adicionais de cópia e chave variável na lavra, além de consulta ORM ao JSON (`app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:998`, `app/ponto/portas/celula.py:999`, `app/chamados/models.py:3356`).

Os tipos abaixo são **tipos produzidos pelos caminhos de código**. O conteúdo efetivamente existente em produção é **[nao verificado]**: os campos são JSON sem esquema declarado, e a porta aceita `dict(ata)` (`app/escala/models.py:1697`, `app/escala/models.py:1778`, `app/ponto/portas/celula.py:995`).

**B — Ata.** Nesta tabela, R/W contam operações literais de topo; os locais completos dos leitores vêm logo depois.

| Chave | R/W | Tipo produzido | Escritores `arquivo:linha:função` |
|---|---:|---|---|
| `tipo_dia` | 5/1 | string ou null | `app/ponto/services/cartorio.py:738:ata_do_dia` |
| `lampadas` | 40/3 | lista de dict | `app/ponto/services/cartorio.py:738:ata_do_dia`; `app/ponto/services/cartorio.py:917:_ata_ate_o_teto`; `app/escala/utils.py:1892:montar_grade_prevista_periodo_por_turno` |
| `minutos_previstos` | 1/1 | int | `app/ponto/services/cartorio.py:739:ata_do_dia` |
| `minutos_realizados` | 3/1 | int | `app/ponto/services/cartorio.py:740:ata_do_dia` |
| `sem_ancora` | 1/1 | bool | `app/ponto/services/cartorio.py:741:ata_do_dia` |
| `n_orfas` | 4/1 | int | `app/ponto/services/cartorio.py:742:ata_do_dia` |
| `orfas` | 10/2 | lista de strings HH:MM, sem data | `app/ponto/services/cartorio.py:745:ata_do_dia`; `app/escala/utils.py:1895:montar_grade_prevista_periodo_por_turno`; formatação em `app/escala/utils.py:1832` |
| `n_missing` | 3/3 | int | `app/ponto/services/cartorio.py:746:ata_do_dia`; `app/ponto/services/cartorio.py:812:dispensar_por_ausencia`; `app/ponto/services/cartorio.py:917:_ata_ate_o_teto` |
| `n_celulas` | 4/1 | int | `app/ponto/services/cartorio.py:762:ata_do_dia` |
| `cobertura_parcial` | 3/1 | bool; True quando presente | `app/ponto/services/cartorio.py:767:ata_do_dia` |
| `periodo_parcial` | 3/1 | string: `sem_periodo`, `dentro`, `fora`, `nao_sei` | `app/ponto/services/cartorio.py:771:ata_do_dia`; vocabulário em `app/ponto/turnos.py:2618` |
| `precedencia` | 1/1 | dict | `app/ponto/portas/celula.py:1084:carimbar_precedencia`; conteúdo em `app/ponto/services/cartorio.py:2000:aplicar_precedencia_na_celula` |

As duas escritas de `escala/utils.py` são em cópia em memória; a cópia do objeto ocorre antes delas (`app/escala/utils.py:1886`). A atribuição da ata na lavra ocorre na porta (`app/ponto/portas/celula.py:1006`, `app/ponto/portas/celula.py:1033`).

Leitores, preservando multiplicidades na mesma linha:

```text
tipo_dia:
 app/chamados/juizes.py:438:ausencia_parcial_cobra_o_resto
 app/chamados/juizes.py:456:dia_sem_trabalho [2 operações]
 app/ponto/management/commands/tripwire_celulas.py:342:medir_ata_fantasma
 app/escala/services/leitor_celula.py:397:grade_da_celula

minutos_previstos:
 app/escala/services/leitor_celula.py:399:grade_da_celula

minutos_realizados:
 app/folha/validacao_pdf.py:48:referencia_do_espelho
 app/escala/services/leitor_celula.py:386:grade_da_celula
 app/escala/services/leitor_celula.py:423:grade_da_celula

sem_ancora:
 app/escala/services/leitor_celula.py:421:grade_da_celula

n_orfas:
 app/chamados/juizes.py:510:_celula_furada
 app/ponto/services/cartorio.py:839:insumos_da_ata
 app/ponto/services/vigia_pareador_celula.py:74:censo
 app/escala/services/leitor_celula.py:190:_orfas_da_ata

orfas:
 app/ponto/juiz_batida.py:161:periodos_do_dia
 app/relatorios/cartao_pela_celula.py:288:orfas_e_isencao_da_ata
 app/chamados/juizes.py:510:_celula_furada
 app/ponto/services/cluster_espurio.py:29:_leitura_da_celula
 app/ponto/services/cluster_espurio.py:31:_leitura_da_celula
 app/ponto/services/vigia_pareador_celula.py:74:censo
 app/ponto/services/esmeril_espelho.py:125:assinaturas_do_colab
 app/ponto/management/commands/tripwire_celulas.py:264:medir_sobra_com_tipo_trocado
 app/escala/services/leitor_celula.py:187:_orfas_da_ata
 app/chamados/services/materializacao.py:181:_plantio_preenche_furo_na_data

n_missing:
 app/escala/services/leitor_celula.py:105:n_ok_lavrado
 app/escala/services/leitor_celula.py:175:_celulas_da_ata
 app/escala/services/leitor_celula.py:385:grade_da_celula

n_celulas:
 app/ponto/management/commands/tripwire_celulas.py:344:medir_ata_fantasma
 app/escala/services/leitor_celula.py:91:n_ok_lavrado
 app/escala/services/leitor_celula.py:105:n_ok_lavrado
 app/escala/services/leitor_celula.py:385:grade_da_celula

cobertura_parcial:
 app/chamados/juizes.py:438:ausencia_parcial_cobra_o_resto
 app/ponto/services/cartorio.py:424:parcial_so_lista
 app/escala/services/leitor_celula.py:433:grade_da_celula

periodo_parcial:
 app/ponto/services/cartorio.py:426:parcial_so_lista
 app/escala/services/leitor_celula.py:435:grade_da_celula
 app/escala/services/leitor_celula.py:436:grade_da_celula

precedencia:
 app/ponto/portas/celula.py:1081:carimbar_precedencia

lampadas:
 app/folha/previa.py:84:montar_previa
 app/ponto/juiz_batida.py:160:periodos_do_dia
 app/ponto/worklist.py:22:linha_da_ata
 app/relatorios/cartao_pela_celula.py:289:orfas_e_isencao_da_ata
 app/relatorios/dia_da_frota.py:21:_faltando
 app/chamados/reconciliador.py:701:_ctx_c2
 app/chamados/juizes.py:505:_celula_furada
 app/chamados/juizes.py:508:_celula_furada
 app/chamados/juizes.py:966:fato_do_marco_no_espelho
 app/chamados/juizes.py:1344:_resposta_concorda_com_a_luz
 app/pautas/services.py:432:contexto_do_dia_texto
 app/folha/services/sem_furo.py:22:dias_com_furo
 app/colaboradores/services/feriado_do_posto.py:97:presenca_em_feriado
 app/ponto/services/lastro.py:89:_lampada
 app/ponto/services/lastro.py:113:marcos_inexistentes
 app/ponto/services/lastro.py:128:marcos_apagados
 app/ponto/services/lastro.py:149:marcos_pendentes
 app/ponto/services/vigia_entrada.py:80:por_dia_da_semana
 app/ponto/services/vigia_entrada.py:123:do_dia
 app/ponto/services/cartorio.py:451:_marco_venceu_apagado
 app/ponto/services/cartorio.py:805:dispensar_por_ausencia
 app/ponto/services/cartorio.py:834:insumos_da_ata
 app/ponto/services/cartorio.py:913:_ata_ate_o_teto
 app/ponto/services/dia_do_colab.py:30:dia_do_colab
 app/ponto/services/cluster_espurio.py:29:_leitura_da_celula
 app/ponto/services/cluster_espurio.py:32:_leitura_da_celula
 app/ponto/services/vigia_pareador_celula.py:78:censo
 app/ponto/services/esmeril_espelho.py:129:assinaturas_do_colab
 app/ponto/services/diagnostico_escala.py:156:diagnostico_escala
 app/ponto/management/commands/tripwire_celulas.py:264:medir_sobra_com_tipo_trocado
 app/ponto/management/commands/tripwire_celulas.py:342:medir_ata_fantasma
 app/ponto/management/commands/curar_residuo_bug115.py:45:classificar
 app/ponto/management/commands/curar_residuo_bug115.py:145:_lavrar
 app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:61:classificar
 app/ponto/management/commands/detectar_intervalo_ausente.py:64:furos_intervalo
 app/escala/services/leitor_celula.py:68:_n_dispensadas
 app/escala/services/leitor_celula.py:82:_justificado_lavrado
 app/escala/services/leitor_celula.py:103:n_ok_lavrado
 app/escala/services/leitor_celula.py:115:_celulas_da_ata
 app/chamados/services/materializacao.py:173:_plantio_preenche_furo_na_data
```

As subchaves declaradas de `lampadas[]` são `tipo`, `hora`, `acesa`, `luz`, `tipo_real`, `isencao`, `dispensada`, `pendente`; `hora` é previsão ou rótulo posicional, enquanto `luz` guarda HH:MM realizado ou null (`app/ponto/services/cartorio.py:675`, `app/ponto/services/cartorio.py:702`, `app/ponto/services/cartorio.py:720`, `app/ponto/services/cartorio.py:591`, `app/ponto/services/cartorio.py:596`, `app/ponto/services/cartorio.py:915`, `app/ponto/juiz_batida.py:193`).

`precedencia` recebe `tipo_do_dia`, `faixa`, `rotulo`, `suprimidos`, `era_dia_previsto`, `texto`; os valores vêm do juiz de precedência, e `suprimidos` é lista de strings (`app/ponto/services/cartorio.py:2000`, `app/ponto/precedencia.py:391`).

**Comparação de hora sem data: sim para `orfas` e para `lampadas.hora/luz`.** Há ordenação/distância de órfãs, pertinência de batida por HH:MM, igualdade de marco previsto e comparação da resposta com a luz (`app/ponto/services/cluster_espurio.py:34`, `app/ponto/services/cluster_espurio.py:35`, `app/ponto/services/vigia_pareador_celula.py:110`, `app/ponto/services/vigia_pareador_celula.py:111`, `app/ponto/services/lastro.py:90`, `app/ponto/juiz_batida.py:230`, `app/chamados/juizes.py:1349`, `app/chamados/juizes.py:1358`, `app/chamados/juizes.py:1362`).

Há ainda leitor ORM de `lampadas.pendente`, projeção da ata inteira e preservação dinâmica de `precedencia`; não os converti em falsos acessos literais adicionais (`app/chamados/models.py:3356`, `app/chamados/reconciliador.py:879`, `app/ponto/portas/celula.py:104`, `app/ponto/portas/celula.py:997`).

**B — DNA.** A restauração está incluída: `alvo` é atribuído ao DNA antes das duas leituras por chave (`app/ponto/portas/celula.py:786`, `app/ponto/portas/celula.py:788`, `app/ponto/portas/celula.py:789`).

| Chave | R/W | Tipo produzido | Escritor | Leitor |
|---|---:|---|---|---|
| `dna_versao` | 1/1 | int, 2 | `app/ponto/management/commands/gerar_celulas.py:96:montar_dna`; constante em `app/ponto/management/commands/gerar_celulas.py:16` | `app/ponto/services/cartorio.py:484:impressao_insumos` |
| `vinculo_id` | 1/1 | ID numérico | `app/ponto/management/commands/gerar_celulas.py:97:montar_dna` | `app/ponto/portas/celula.py:789:reverter_regeneracao_dia` |
| `tipo_escala_id` | 0/1 | ID numérico | `app/ponto/management/commands/gerar_celulas.py:98:montar_dna` | Sem leitura literal individual no perímetro; blob comparado em `app/ponto/portas/celula.py:263` |
| `tipo_ciclo` | 1/1 | string | `app/ponto/management/commands/gerar_celulas.py:99:montar_dna`; campo em `app/escala/models.py:117` | `app/ponto/services/vigia_entrada.py:129:do_dia` |
| `marcos` | 27/1 | dict ou null | `app/ponto/management/commands/gerar_celulas.py:100:montar_dna` | Relação abaixo |
| `minutos_jornada` | 0/1 | int | `app/ponto/management/commands/gerar_celulas.py:101:montar_dna`; campo em `app/escala/models.py:49` | Sem leitura literal individual no perímetro; blob comparado em `app/ponto/portas/celula.py:263` |
| `intervalo_livre` | 0/1 | bool | `app/ponto/management/commands/gerar_celulas.py:102:montar_dna` | Sem leitura literal individual no perímetro; blob comparado em `app/ponto/portas/celula.py:263` |
| `lampadas` | 1/1 | lista de dict ou null | `app/ponto/management/commands/gerar_celulas.py:103:montar_dna` | `app/escala/utils.py:153:lampadas_de_dna` |
| `extra_declarada` | 1/1 | dict `{antes:int,depois:int}` | `app/ponto/management/commands/gerar_celulas.py:122:montar_dna` | `app/escala/models.py:1235:EscalaColaborador.extra_declarada_do_dia` |

Leitores de `marcos`:

```text
app/chamados/juizes.py:948:fato_do_marco_no_espelho
app/chamados/juizes.py:1338:_resposta_concorda_com_a_luz
app/chamados/services/adesao.py:36:universo_adesao
app/escala/models.py:1124:EscalaColaborador.marcos_do_dia
app/escala/models.py:1189:EscalaColaborador.pausas_do_dia
app/escala/utils.py:53:pausas_dna_periodo
app/escala/utils.py:104:marcos_dna_periodo
app/escala/utils.py:137:pausas_de_celulas
app/pautas/services.py:424:contexto_do_dia_texto
app/ponto/juiz_batida.py:159:periodos_do_dia [2 operações]
app/ponto/management/commands/aplicar_col900.py:172:Command.handle
app/ponto/management/commands/censo_col900.py:118:Command.handle
app/ponto/management/commands/censo_col900.py:119:Command.handle
app/ponto/management/commands/detectar_intervalo_ausente.py:53:furos_intervalo
app/ponto/management/commands/tripwire_celulas.py:99:medir_pausa_faltando
app/ponto/management/commands/tripwire_celulas.py:259:medir_sobra_com_tipo_trocado
app/ponto/portas/celula.py:788:reverter_regeneracao_dia
app/ponto/services/cartorio.py:484:impressao_insumos
app/ponto/services/cartorio.py:615:lampadas_do_dia
app/ponto/services/cartorio.py:840:insumos_da_ata
app/ponto/services/dia_do_colab.py:46:dia_do_colab
app/ponto/services/diagnostico_escala.py:153:diagnostico_escala
app/ponto/services/esmeril_espelho.py:116:assinaturas_do_colab
app/ponto/services/faltas_de_hoje.py:43:faltas_de_hoje_sem_cobranca
app/ponto/services/veredito_celula.py:31:_marcos_do_dia
app/ponto/services/vigia_entrada.py:126:do_dia
```

As subchaves de `marcos` são `hi`, `hf`, `hii`, `hfi`, com HH:MM ou null, e os opcionais `hii2`, `hfi2`; a declaração canônica dos pares está no modelo (`app/ponto/management/commands/gerar_celulas.py:83`, `app/ponto/management/commands/gerar_celulas.py:93`, `app/escala/models.py:1652`).

`dna.lampadas[]` recebe `chave`, `tipo`, `hora`, `instante`. **Esse `instante` é ISO sem fuso**, construído por `datetime.combine(...).isoformat(...)`; não o classifico como instante com fuso (`app/ponto/management/commands/gerar_celulas.py:68`, `app/ponto/management/commands/gerar_celulas.py:69`). O consumidor lê `hora` e `instante` para montar um dict indexado por HH:MM (`app/escala/utils.py:154`, `app/escala/utils.py:155`).

`extra_declarada.antes/depois` são inteiros e ambos são lidos pelo acessor do vínculo (`app/ponto/management/commands/gerar_celulas.py:119`, `app/ponto/management/commands/gerar_celulas.py:122`, `app/escala/models.py:1238`).

**Comparação de hora sem data: sim para `marcos` e `lampadas`.** Inclui pertinência, chave de dict, distância em minutos e igualdade ATA/DNA (`app/ponto/services/cartorio.py:640`, `app/ponto/services/cartorio.py:648`, `app/ponto/services/cartorio.py:666`, `app/ponto/services/cartorio.py:846`, `app/ponto/juiz_batida.py:230`, `app/ponto/services/vigia_entrada.py:127`, `app/escala/utils.py:155`). As comparações do blob inteiro também leem as demais chaves (`app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:594`, `app/ponto/portas/celula.py:872`).

**C — Classificação integral de `strftime('%H:%M')`.** A relação abaixo contém as 200 chamadas de `app/`; números repetidos preservam chamadas distintas na mesma linha. Os caminhos desta relação são relativos a `app/`.

Legenda dos fatos em P/C: **B** batida; **M** marco realizado transportado da batida; **O** órfã; **R** resposta/plantio pretendido; **V** marco previsto; **T** relógio da tentativa/recepção; **A** período declarado de ausência; **E** evento da esteira; **X** tentativa de materialização que entrou no erro. A distinção entre tentativa e batida efetiva aparece no tratamento offline e nos contextos de tentativa (`app/api/views.py:508`, `app/api/views.py:627`, `app/ponto/services/triagem_batida.py:741`); o erro de materialização está em `app/ponto/services/veredito_celula.py:329`.

A classificação primária usa P antes de C quando ambos se demonstram: **P62, C53, R83, U2**. As seis sobreposições P+C são `app/escala/utils.py:291`, `app/escala/utils.py:730`, `app/escala/utils.py:749`, `app/ponto/motor_calculo_v2.py:1671`, `app/ponto/motor_calculo_v2.py:1672`, `app/ponto/management/commands/detectar_ausencias.py:484`. Assim, há **59 chamadas com uso comparativo**, contando as sobreposições.

```text
arquivo | PERSISTIDO [linha:fato] | COMPARADO [linha:fato] | RENDER [linhas] | U
api/views.py | 627:T,645:T | — | 355,535,681,800,1457,2035,2959,2960 | —
api/views_core.py | 618:T,636:T | — | 343,521,672,794,1280,1281 | —
chamados/detector_anomalias.py | 234:V,235:B | 213:B | — | 222
chamados/juizes.py | — | 1814:V | — | —
chamados/models.py | — | — | 4070,4072,4076,4078 | —
chamados/reconciliador.py | — | 210:B | — | —
chamados/services/acoes_chamado.py | — | — | 383 | —
chamados/services/explicador.py | — | — | 57 | —
chamados/services/materializacao.py | — | 182:R | 465 | —
chamados/services/painel.py | — | — | 395 | —
chamados/services/tarefa_do_dia.py | — | 78:V | — | —
chamados/views_cobrar.py | — | — | 42,57 | —
colaboradores/services/calendario.py | — | 625:B | — | —
colaboradores/services/ficha.py | — | — | 28 | —
colaboradores/views.py | — | — | 2312,2313 | —
core/esteira_vigia.py | 1304:E,1560:E,1569:E,1572:E,1580:E,1590:E | — | — | —
core/templatetags/hasner_filters.py | — | — | 102,102,147 | —
core/views.py | — | — | 825 | —
escala/models.py | — | — | 415,415,444,444,452,452,509,509,517,517,546,546,664,664,665,665 | —
escala/servico_jornada.py | — | 455:V,456:B | 201,201 | —
escala/utils.py | 291:V,326:M,621:O,662:O,712:V,730:V,749:V,799:O,1549:M,1828:M,1832:O,1834:O | 378:V,429:B,782:V,1675:O,1707:V,1719:V,1724:V,1742:V,1754:V,1755:V,2005:B/O,2028:B/O | — | —
pautas/services.py | — | — | 431 | —
ponto/calculador/alimentacao.py | — | 119:B | — | —
ponto/calculador/nucleo.py | 142:B,146:B,238:B | — | — | —
ponto/management/commands/censo_col900.py | 138:B | — | — | —
ponto/management/commands/desvio_o68b.py | — | — | 125,126 | —
ponto/management/commands/detectar_ausencias.py | 484:V,848:B | — | 410,418,429,436,452,782 | —
ponto/management/commands/detectar_intervalo_ausente.py | — | 67:V,74:V,79:V,82:V,84:V | — | 81
ponto/management/commands/detectar_par_relampago.py | 97:B,98:B | — | 393,394 | —
ponto/management/commands/diff_calculador.py | — | — | 460 | —
ponto/management/commands/diff_reclassificar_partido.py | — | — | 97,98 | —
ponto/management/commands/dna_x_batida_real.py | 122:V,123:V | — | — | —
ponto/management/commands/emitir_furo_retroativo.py | — | — | 152 | —
ponto/management/commands/emitir_turno_aberto_retroativo.py | 167:V | — | — | —
ponto/management/commands/gerar_celulas.py | 33:V | — | — | —
ponto/management/commands/processar_alertas_avancados.py | 136:V | — | — | —
ponto/management/commands/reconciliar_fantasmas.py | 121:B,123:B,134:B,134:V | — | — | —
ponto/management/commands/tripwire_celulas.py | — | 192:V,195:O,280:V | — | —
ponto/management/commands/tripwire_tipo_batida.py | — | — | 64 | —
ponto/marcos.py | — | 11:V | — | —
ponto/motor_cal

[truncado em 20000 de 54867 caracteres]

## 12/11 16:53 · sessao 55d549d73138 · - · - · origem=Ronald (tmux)

Censo por AST: **200 chamadas** em **185 linhas de 59 arquivos** de `app/`, excluindo diretórios `tests`, `__pycache__`, `.venv`, `venv` e arquivos `test_*.py`; zero erros de parse. São 162 chamadas fora de `management` e 38 dentro. A diferença contra 161 é **+39 chamadas**; o comando/recorte do grep do aval não foi fornecido, portanto equivalência entre as medições é [não verificado]. Incluindo o repositório inteiro com os mesmos filtros há mais uma chamada de render em `bin/disjuntor.py:2266`.

A classificação primária usa **P > C > R** quando há mais de um destino demonstrado: **P62 + C53 + R83 + U2 = 200**. `U` identifica valor descartado, fora da taxonomia solicitada. Sobreposição explícita P+C: seis chamadas — `app/escala/utils.py:291`, `app/escala/utils.py:730`, `app/escala/utils.py:749`, `app/ponto/motor_calculo_v2.py:1671`, `app/ponto/motor_calculo_v2.py:1672`, `app/ponto/management/commands/detectar_ausencias.py:484`. Contando essas sobreposições, são 59 chamadas com uso comparativo. Os totais são a soma da tabela abaixo; classificação de todos os destinos possíveis em execução é [não verificado].

Na tabela, linhas repetidas significam duas chamadas na mesma linha; cada número compõe `arquivo:linha`.

```text
arquivo                                                       PERSISTIDO                 COMPARADO                       RENDER                              SEM USO
app/api/views.py                                              627,645                                                     355,535,681,800,1457,2035,2959,2960
app/api/views_core.py                                         618,636                                                     343,521,672,794,1280,1281
app/chamados/detector_anomalias.py                             234,235                    213                                                                 222
app/chamados/juizes.py                                                                   1814
app/chamados/models.py                                                                                                   4070,4072,4076,4078
app/chamados/reconciliador.py                                                            210
app/chamados/services/acoes_chamado.py                                                                                    383
app/chamados/services/explicador.py                                                                                       57
app/chamados/services/materializacao.py                                                   182                            465
app/chamados/services/painel.py                                                                                          395
app/chamados/services/tarefa_do_dia.py                                                     78
app/chamados/views_cobrar.py                                                                                             42,57
app/colaboradores/services/calendario.py                                                   625
app/colaboradores/services/ficha.py                                                                                      28
app/colaboradores/views.py                                                                                              2312,2313
app/core/esteira_vigia.py                                     1304,1560,1569,1572,1580,1590
app/core/templatetags/hasner_filters.py                                                                                  102,102,147
app/core/views.py                                                                                                       825
app/escala/models.py                                                                                                    415,415,444,444,452,452,509,509,
                                                                                                                       517,517,546,546,664,664,665,665
app/escala/servico_jornada.py                                                             455,456                        201,201
app/escala/utils.py                                          291,326,621,662,712,730,749,  378,429,782,1675,1707,1719,
                                                            799,1549,1828,1832,1834     1724,1742,1754,1755,2005,2028
app/pautas/services.py                                                                                                  431
app/ponto/calculador/alimentacao.py                                                       119
app/ponto/calculador/nucleo.py                               142,146,238
app/ponto/management/commands/censo_col900.py                 138
app/ponto/management/commands/desvio_o68b.py                                                                              125,126
app/ponto/management/commands/detectar_ausencias.py           484,848                                                     410,418,429,436,452,782
app/ponto/management/commands/detectar_intervalo_ausente.py                                67,74,79,82,84                                                      81
app/ponto/management/commands/detectar_par_relampago.py       97,98                                                       393,394
app/ponto/management/commands/diff_calculador.py                                                                          460
app/ponto/management/commands/diff_reclassificar_partido.py                                                               97,98
app/ponto/management/commands/dna_x_batida_real.py            122,123
app/ponto/management/commands/emitir_furo_retroativo.py                                                                    152
app/ponto/management/commands/emitir_turno_aberto_retroativo.py 167
app/ponto/management/commands/gerar_celulas.py               33
app/ponto/management/commands/processar_alertas_avancados.py 136
app/ponto/management/commands/reconciliar_fantasmas.py        121,123,134,134
app/ponto/management/commands/tripwire_celulas.py                                         192,195,280
app/ponto/management/commands/tripwire_tipo_batida.py                                                                     64
app/ponto/marcos.py                                                                     11
app/ponto/motor_calculo_v2.py                                1671,1672                   1488,3069                      2221,2222,2229,2230
app/ponto/propositor.py                                                                 340,342,342,343,345
app/ponto/services/ausencia.py                               910,911,934,934
app/ponto/services/cartorio.py                                                          878,1927
app/ponto/services/dia_do_colab.py                                                                                       57
app/ponto/services/diagnostico_escala.py                                                 26
app/ponto/services/entrada_sincronizada.py                    28
app/ponto/services/espelho.py                                                                                           210
app/ponto/services/faltas_de_hoje.py                                                     45
app/ponto/services/flip_auto.py                                                         219,221,221,222,232            319
app/ponto/services/lastro.py                                                                                            313
app/ponto/services/passivo_s84.py                            103
app/ponto/services/teto_na_lista.py                          82,83
app/ponto/services/triagem_batida.py                         472,473,479,480,492,540,
                                                            586,611,741                                               187
app/ponto/services/veredito_celula.py                        320,329                     252
app/ponto/services/vigia_pareador_celula.py                                              110
app/ponto/utils_turno_orfao.py                                                                                          40
app/relatorios/pdf_espelho.py                                                           178,609,716,960,963             732,733,881,885,907,907,955
app/relatorios/services.py                                                                                             133
```

Os dois valores sem uso são:

- `app/chamados/detector_anomalias.py:222`: `horario_real` no retorno `informativa`. Os dois chamadores tratam `grave` e `leve`, sem consumir esse retorno: `app/api/views.py:701`, `app/api/views.py:702`, `app/api/views.py:705`, `app/api/views_core.py:692`, `app/api/views_core.py:693`, `app/api/views_core.py:696`.
- `app/ponto/management/commands/detectar_intervalo_ausente.py:81`: a hora compõe `marcos_faltantes`, mas o chamador usa apenas as datas e a quantidade de furos; a emissão está encerrada por `continue`: `app/ponto/management/commands/detectar_intervalo_ausente.py:120`, `app/ponto/management/commands/detectar_intervalo_ausente.py:121`, `app/ponto/management/commands/detectar_intervalo_ausente.py:156`, `app/ponto/management/commands/detectar_intervalo_ausente.py:157`.

Persistidos: fato e destino demonstrado:

| Origem | Fato/previsão | Destino/prova |
|---|---|---|
| `app/escala/utils.py:326`, `:1549`, `:1828` | Batida/marco realizado | A hora realizada da célula torna-se `lampadas[].luz`: `app/ponto/services/cartorio.py:711`, `:712`, `:736`, `:738`. |
| `app/escala/utils.py:621`, `:662`, `:799`, `:1832`, `:1834` | Órfã | A lista da grade vira `ata['orfas']` sem instante: `app/ponto/services/cartorio.py:743`, `:745`. |
| `app/escala/utils.py:291`, `:712`, `:730`, `:749` | Marco previsto | Produzem a régua `tipo/hora`; a régua alimenta a hora prevista das lâmpadas: `app/escala/utils.py:734`, `:753`, `:1401`; `app/ponto/services/cartorio.py:638`, `:648`, `:711`. |
| `app/chamados/detector_anomalias.py:234`, `:235` | Previsto e batida, respectivamente | `ctx_desvio` vira `contexto_json` de chamado: `app/api/views.py:714`, `:717`, `:726`; `app/api/views_core.py:708`, `:720`. |
| `app/api/views.py:627`, `:645`; `app/api/views_core.py:618`, `:636` | Hora da tentativa/recepção, pelo relógio do servidor | Compõem contexto de disputa: `app/api/views.py:497`, `:498`, `:625`, `:642`, `:765`. Não são necessariamente o instante efetivo offline da batida: `app/api/views.py:508`. |
| `app/ponto/services/triagem_batida.py:472`, `:479`, `:741` | Tentativa/batida web | Chamado JSON, sessão, disputa: `app/ponto/services/triagem_batida.py:470`, `:486`, `:735`. |
| `app/ponto/services/triagem_batida.py:473`, `:480`, `:492` | Previsto | Chamado/sessão e texto da justificativa da batida: `app/ponto/services/triagem_batida.py:470`, `:486`, `:690`, `:697`, `:702`. |
| `app/ponto/services/triagem_batida.py:540`, `:586`, `:611` | Batida | Texto humano salvo em ocorrência e descrição de justificativa: `app/ponto/services/triagem_batida.py:603`, `:609`, `:613`. |
| `app/ponto/services/entrada_sincronizada.py:28` | Batida sincronizada | Ocorrência, justificativa e resolução: `app/ponto/services/entrada_sincronizada.py:35`, `:42`, `:43`, `:60`. |
| `app/ponto/motor_calculo_v2.py:1671`, `:1672` | Batida real e previsto | Lista do motor levada a `MetricaSnapshot.detalhe`: `app/ponto/motor_calculo_v2.py:2719`; `app/ponto/services/cxr_na_lista.py:79`, `:86`, `:130`, `:145`, `:148`. |
| `app/ponto/services/teto_na_lista.py:82`, `:83` | Marcos previstos `hi/hf` | Snapshot: `app/ponto/services/teto_na_lista.py:113`, `:116`. |
| `app/ponto/services/ausencia.py:910`, `:911`, `:934` duas chamadas | Período declarado de ausência | Auditoria antes/depois: `app/ponto/services/ausencia.py:932`, `:933`, `:934`; `app/core/auditoria.py:22`. Realização do período é [não verificado]. |
| `app/ponto/services/veredito_celula.py:320`, `:329` | Batida corrigida; tentativa de materializar marco | Ações/erros salvos em auditoria: `app/ponto/services/veredito_celula.py:356`, `:358`. Sucesso da materialização do caso `:329` é [não verificado]. |
| `app/ponto/management/commands/detectar_par_relampago.py:97`, `:98` | Batidas do par | `contexto_json['par']`: `app/ponto/management/commands/detectar_par_relampago.py:383`. |
| `app/ponto/management/commands/detectar_ausencias.py:484` | Marco previsto | `contexto_json['marcos_faltantes']`: `app/ponto/management/commands/detectar_ausencias.py:706`, `:710`, `:712`. |
| `app/ponto/management/commands/detectar_ausencias.py:848` | Batida | Ocorrência textual: `app/ponto/management/commands/detectar_ausencias.py:852`. |
| `app/ponto/management/commands/gerar_celulas.py:33` | Marco previsto | Helper alimenta `dna.marcos` e `dna.lampadas[].hora`: `app/ponto/management/commands/gerar_celulas.py:68`, `:83`, `:93`, `:100`, `:103`. |
| `app/ponto/management/commands/processar_alertas_avancados.py:136`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:167` | Saída prevista | Descrição do chamado: `app/ponto/management/commands/processar_alertas_avancados.py:121`, `:133`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`, `:159`. |
| `app/ponto/management/commands/reconciliar_fantasmas.py:121`, `:123`, `:134` duas chamadas | Entrada/saída reais; saída real e saída prevista na linha 134 | Diagnóstico `detalhe` salvo em ocorrência/auditoria: `app/ponto/management/commands/reconciliar_fantasmas.py:205`, `:206`, `:278`, `:280`, `:286`, `:299`. |
| `app/ponto/calculador/nucleo.py:142`, `:146`, `:238` | Batidas | Arquivo CSV diagnóstico: `app/ponto/management/commands/e6_oraculo.py:330`, `:404`, `:444`, `:445`. |
| `app/ponto/management/commands/censo_col900.py:138` | Primeira batida | Arquivo JSON opcional: `app/ponto/management/commands/censo_col900.py:178`, `:179`. |
| `app/ponto/management/commands/dna_x_batida_real.py:122`, `:123` | Marcos previstos | Arquivo JSON opcional: `app/ponto/management/commands/dna_x_batida_real.py:155`, `:156`, `:157`. |
| `app/ponto/services/passivo_s84.py:103` | Batida | Arquivo de reversão: `app/ponto/management/commands/passivo_disputa_retro.py:139`, `:141`, `:146`, `:148`. |
| `app/core/esteira_vigia.py:1304`, `:1560`, `:1569`, `:1572`, `:1580`, `:1590` | Eventos da esteira; não batida/órfã/resposta/marco | Arquivos de relato/fim: `app/core/esteira_vigia.py:1329`, `:1559`, `:1570`, `:1578`, `:1589`; o argumento de `destravar` chega a `f.write`: `app/core/esteira_vigia.py:1027`, `:1028`. |

Comparados: fato e operação demonstrada:

| Origem | Fato/previsão e uso |
|---|---|
| `app/escala/utils.py:291`, `:730`, `:749`, `:782`, `:1707`, `:1719`, `:1724`, `:1742`, `:1754`, `:1755` | Marcos previstos: chaves de dict, pertinência e busca de coluna. `app/escala/utils.py:292`, `:730`, `:749`, `:785`, `:1708`, `:1719`, `:1724`, `:1745`, `:1754`, `:1755`. |
| `app/escala/utils.py:378` | Marco previsto encadeado: chave de dict para casar régua e instante. `app/escala/utils.py:378`, `:381`, `:382`. |
| `app/escala/utils.py:429` | Batida: igualdade com hora realizada da célula. `app/escala/utils.py:431`. |
| `app/escala/utils.py:1675` | Batida/órfã: pertinência na lista de órfãs convertida em conjunto. `app/escala/utils.py:1650`, `:1651`, `:1675`. |
| `app/escala/utils.py:2005`, `:2028` | Batidas/órfãs: pertinência, igualdade de listas e ordem textual da hora realizada. `app/escala/utils.py:2006`, `:2032`, `:2034`, `:2039`. |
| `app/ponto/motor_calculo_v2.py:1488`; `app/ponto/calculador/alimentacao.py:119` | Entrada real: chave `(dia, HH:MM)` e pertinência no conjunto de pares bloqueados. `app/ponto/calculador/alimentacao.py:74`, `:119`. |
| `app/ponto/motor_calculo_v2.py:1671`, `:1672` | Real/previsto: deduplicação por igualdade do item; previsto também casa com marco da célula. `app/ponto/services/cxr_na_lista.py:86`, `:107`; `app/ponto/janela_he.py:296` e continuação do corpo. |
| `app/ponto/motor_calculo_v2.py:3069` | Entrada real: igualdade com hora realizada do alinhamento. `app/ponto/motor_calculo_v2.py:3069`. |
| `app/ponto/propositor.py:340`, `:342` duas chamadas, `:343`, `:345`; `app/escala/servico_jornada.py:455`, `:456`; `app/ponto/services/flip_auto.py:219`, `:221` duas chamadas, `:222`, `:232` | Previstos e batidas convertidos à mesma representação de hora do dia para atribuição posicional. `app/ponto/propositor.py:351`; `app/escala/servico_jornada.py:455`; `app/ponto/services/flip_auto.py:233`, `:238`. |
| `app/ponto/marcos.py:11` | Previsto: helper utilizado em igualdades de hora. `app/ponto/marcos.py:31`, `:33`, `:37`, `:39`, `:47`, `:49`, `:63`, `:65`, `:108`. |
| `app/chamados/reconciliador.py:210` | Batida: chave de dict usada para recuperar os integrantes do par por HH:MM. `app/chamados/reconciliador.py:209`, `:213`. |
| `app/chamados/detector_anomalias.py:213` | Batida: igualdade com hora realizada do alinhamento. `app/chamados/detector_anomalias.py:214`. |
| `app/chamados/juizes.py:1814` | Previsto: retorna HH:MM para comparação com resposta; a resposta é reduzida a minutos do dia. `app/chamados/juizes.py:1852`, `:1858`, `:1863`, `:1868`, `:1878`. |
| `app/chamados/services/materializacao.py:182` | Instante pretendido pela resposta/plantio: atribuição contra slots e horas reais/órfãs. `app/chamados/services/materializacao.py:179`, `:180`, `:181`, `:182`. |
| `app/chamados/services/tarefa_do_dia.py:78` | Previsto: igualdade com a hora prevista da lâmpada. `app/chamados/services/tarefa_do_dia.py:78`. |
| `app/ponto/services/cartorio.py:878`, `:1927` | Previstos: conjunto de marcos vencidos e dict de instantes por `(tipo, HH:MM)`. `app/ponto/services/cartorio.py:447`, `:450`, `:1927`, `:1930`, `:1931`, `:1936`. |
| `app/ponto/services/veredito_celula.py:252` | Previsto: chave de dict para materialização do marco faltante. `app/ponto/services/veredito_celula.py:275`, `:276`. |
| `app/ponto/services/faltas_de_hoje.py:45` | Previsto: volta a `datetime` para comparação com agora. `app/ponto/services/faltas_de_hoje.py:48`, `:49`. |
| `app/ponto/services/diagnostico_escala.py:26` | Batida: helper serve render e chaves de dict; também distância entre primeira entrada e `dna.marcos.hi`. `app/ponto/services/diagnostico_escala.py:160`, `:163`, `:165`, `:167`, `:172`. |
| `app/colaboradores/services/calendario.py:625` | Batida: pertinência em órfãs e lookup por hora. `app/colaboradores/services/calendario.py:626`, `:631`. |
| `app/ponto/services/vigia_pareador_celula.py:110` | Batida: pertinência em órfãs civis. `app/ponto/services/vigia_pareador_celula.py:111`. |
| `app/relatorios/pdf_espelho.py:178`, `:609`, `:716`, `:960`, `:963` | Batidas/órfãs: pertinência por HH:MM, inclusive filtragem textual de anomalias. `app/relatorios/pdf_espelho.py:178`, `:609`, `:720`, `:964`. |
| `app/ponto/management/commands/tripwire_celulas.py:192`, `:195`, `:280` | Previsto, órfã e previsto, respectivamente: igualdade HH:MM antes de teste por instante. `app/ponto/management/commands/tripwire_celulas.py:192`, `:195`, `:280`, `:285`. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:67`, `:74`, `:79`, `:82`, `:84` | Previstos: lookup de lâmpadas e de instante encadeado por `(tipo, HH:MM)`. `app/ponto/management/commands/detectar_intervalo_ausente.py:67`, `:74`, `:79`, `:82`, `:84`. |
| `app/ponto/management/commands/detectar_ausencias.py:484` | Previsto: traduzido/comparado por `marco_para_motivo` e persistido nos marcos faltantes. `app/ponto/management/commands/detectar_ausencias.py:482`, `:484`, `:712`; `app/ponto/marcos.py:31`. |

A lei fornecida exclui `dna.hi/hf` previstos; o produtor desta cópia efetivamente os escreve como hora do dia por `_hhmm`: `app/ponto/management/commands/gerar_celulas.py:33`, `:83`. O principal escritor de fato em HH:MM está antes da lavra: `_celula_do_marco` grava a batida em `hora`, e o cartório a transporta para `lampadas[].luz`; órfãs são transportadas como strings: `app/escala/utils.py:326`; `app/ponto/services/cartorio.py:712`, `:745`.

A classificação P inclui texto humano salvo, CSV/JSON diagnóstico e arquivos da est

[truncado em 20000 de 20154 caracteres]

## 12/11 16:52 · sessao c79798754d32 · C20-O168 · verificador · origem=Code

**A guarda cobre os quatro sítios alterados e a cascata E2, mas não todos os fechamentos automáticos alcançados pelo evento. Há também divergência entre “pai vivo preservado” e o universo efetivamente contado.** (`app/ponto/services/cartorio.py:965`, `app/chamados/reconciliador.py:651`, `app/ponto/services/lastro.py:401`, `app/chamados/services/disputa_emissao.py:2091`.)

Execução da suíte, execução de mutantes e ocorrência dos achados em produção: **[nao verificado]**. [nao verificado]

**1. Caminhos que fecham e cobertura da guarda**

| Caminho | Passa por `pode_executar_csf`? | Prova |
|---|---|---|
| Cartório: adiados pela lâmpada | **Sim** | `app/ponto/services/cartorio.py:1054` → guarda em `app/ponto/services/cartorio.py:965`. |
| Cartório: RECONCILIA-MARCO | **Sim** | `app/ponto/services/cartorio.py:1589`. |
| Cartório: MARCO-INEXISTENTE | **Sim** | `app/ponto/services/cartorio.py:1602`. |
| Cartório: B5.3b | **Sim** | `app/ponto/services/cartorio.py:1615`, `app/ponto/services/cartorio.py:1623`. |
| E2 → `disputa.fechar` → S88 | **Sim, antes de fechar a disputa** | Lote consulta o juiz em `app/chamados/reconciliador.py:649`; preservação precede `.fechar()` em `app/chamados/reconciliador.py:621`, `app/chamados/reconciliador.py:629`. |
| Cartório: FIO-DUP e FIO-DUP-C8 | **Não** | Fechos em `app/ponto/services/cartorio.py:1701`, `app/ponto/services/cartorio.py:1716`; isenção própria por texto em `app/ponto/services/cartorio.py:1687`. |
| `reconciliar_chamados_colab` → premissa morta → `_retratar` | **Não** | `app/chamados/reconciliador.py:1475`, `app/chamados/reconciliador.py:1498`, `app/chamados/reconciliador.py:524`. |
| `reconciliar_chamados_colab` → órfãs → `lavrar_sem_turno_aberto` → disputa → S88 | **Não** | `app/chamados/reconciliador.py:1467`, `app/chamados/reconciliador.py:1653`, `app/chamados/reconciliador.py:413`. |
| Consumidor `EventoCelula` → E2 | **Sim** | `app/chamados/reconciliador.py:1399` → proteção central em `app/chamados/reconciliador.py:621`. |
| Consumidor → chamados absolvidos pela célula → `_retratar` | **Não** | `app/chamados/reconciliador.py:1402`, `app/chamados/reconciliador.py:1038`, `app/chamados/reconciliador.py:524`. |
| Consumidor → premissa morta → `_retratar` | **Não** | `app/chamados/reconciliador.py:1406`, `app/chamados/reconciliador.py:1498`. |
| Consumidor → órfãs pela lâmpada → disputa → S88 | **Não** | `app/chamados/reconciliador.py:1409`, `app/chamados/reconciliador.py:1205`. |
| Consumidor → lastro por marco → `reconciliar` | **Não** | `app/chamados/reconciliador.py:1417`, `app/ponto/services/lastro.py:395`, `app/ponto/services/lastro.py:401`. |
| S143/S128 → resolver perguntas → disputa → S88 | **Não** | S143 chama resolver em `app/chamados/signals.py:117`; candidatos incluem disputas abertas do colaborador em `app/chamados/services/disputa_emissao.py:2026`; fechamento em `app/chamados/services/disputa_emissao.py:2092`. |
| Fábrica → geração → reatar órfã | **Não nos fechos alcançados** | Geração chama reata em `app/chamados/services/disputa_emissao.py:459`; reata chega aos fechos em `app/chamados/reconciliador.py:1653`, `app/chamados/reconciliador.py:1682`. |
| Emissor do cartório → repasse de órfãos → auto-fecho da disputa | **Não** | Emissor chama repasse em `app/ponto/management/commands/detectar_ausencias.py:792`; auto-fecho em `app/chamados/services/disputa_emissao.py:1278`, `app/chamados/services/disputa_emissao.py:1284`. |

Há ainda fechamento por agrupamento LIMBO e por premissa de cluster, ambos sem CSF. O enquadramento desses dois domínios na lei específica de **cobrança sem furo** é **[nao verificado]**. (`app/chamados/services/limbo_folgas.py:284`, `app/ponto/services/cartorio.py:1085`, `app/ponto/services/cluster_espurio.py:190`.)

**S88 e o gate não completam a proteção ausente nos chamadores:** S88 chama a porta; `reconciliar` consulta motor, premissa e pergunta viva, sem CSF. (`app/chamados/signals.py:43`, `app/chamados/models.py:672`, `app/chamados/models.py:683`, `app/chamados/models.py:686`.)

As guardas existentes são diferentes: `_retratar` retém resposta **sem consumo**, enquanto CSF retém qualquer `respondida_em`, inclusive resposta já validada/materializada. C7 pergunta por fio vivo e resposta sem veredito. (`app/chamados/reconciliador.py:448`, `app/chamados/juizes.py:2018`, `app/ponto/supra_juiz.py:545`, `app/chamados/models.py:1843`, `app/chamados/models.py:1854`.)

O FIO-DUP está explicitamente declarado como lacuna em fila no próprio helper; não o apresento como cura prometida pelo delta. (`app/ponto/services/cartorio.py:948`.)

**2. Constância e mordida do teste** [nao verificado]

- **O desenho de consultas em lote existe nos dois helpers.** `_pais_com_conversa` carrega as duas famílias de perguntas; `_fecham_cobrancas_sem_furo` carrega origem/perguntas, recebendo as resolvedoras já carregadas pelo caminho real do cartório. (`app/chamados/reconciliador.py:649`, `app/ponto/services/cartorio.py:962`, `app/ponto/services/cartorio.py:128`, `app/ponto/services/cartorio.py:1369`.)
- **A comparação explícita 1 × 3 mede somente `_pais_com_conversa`.** Não mede o E2 completo nem os quatro sítios diretos. O E2 conserva, inclusive, `refresh_from_db` dentro do laço por disputa. (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:281`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:286`, `app/chamados/reconciliador.py:620`.)
- **Um mutante que acrescente uma query por disputa dentro do helper é alcançado pela igualdade:** acrescentaria +1 versus +3 às capturas. Isso é conclusão estática; execução do mutante **[nao verificado]**. Mutação fora desse trecho capturado não é demonstrada por esse teste. (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:275`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:282`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:286`.)
- Os tetos **222/282** estão registrados, mas o selo geral cobra `medido <= teto`; não compara o delta da guarda entre os cenários. CPU e volume de perguntas também não são constantes: o juiz percorre as coleções. (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `app/ponto/tests/test_selo_chokepoint_escrita.py:210`, `app/ponto/tests/test_selo_chokepoint_escrita.py:293`, `app/ponto/supra_juiz.py:543`.)

Portanto: **lote nos novos sítios; prova comparativa de constância somente no helper E2**, sem certificação do custo completo do evento. (`app/ponto/services/cartorio.py:962`, `app/chamados/reconciliador.py:649`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:282`.)

**3. Contador `csf_com_conversa_preservado`**

| Aspecto | Escritor/leitor e prova |
|---|---|
| Unidade implementada | `pk` de chamado distinto **por ato**, pelo conjunto em `app/ponto/services/cartorio.py:978`. |
| Incrementador | `_contar_csf_preservado`, em `app/ponto/services/cartorio.py:981`. |
| Chamador direto | Guarda negativa em `app/ponto/services/cartorio.py:966`. |
| Chamador pela cascata | Callback fornecido em `app/ponto/services/cartorio.py:1050`, invocado em `app/chamados/reconciliador.py:626`. |
| Releitura | Compartilha o conjunto e soma somente incrementos novos em `app/ponto/services/cartorio.py:1131`, `app/ponto/services/cartorio.py:1134`. |
| Duração | Conjunto removido no `finally`; o número permanece acumulado no `out` do lote. (`app/ponto/services/cartorio.py:1150`, `app/ponto/services/cartorio.py:977`.) |
| Inicialização | Surge no primeiro incremento; não consta no retorno inicial de `contadores()`. (`app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:2005`.) |
| Leitores | Consolidação da releitura e testes de resultado. (`app/ponto/services/cartorio.py:1133`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:244`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:298`.) |
| Exposição | `julgar_celula` devolve `out`; os resumos dos dois comandos não imprimem esse contador. (`app/ponto/services/cartorio.py:2105`, `app/ponto/management/commands/processar_cartorio.py:66`, `app/ponto/management/commands/lavrar_status_competencia.py:58`.) |

**Defeito de universo:** `_pais_com_conversa` anuncia pai VIVO, mas usa `not in ENCERRADOS`. Isso admite `cancelado` e `superado`, que não são vivos e já seriam recusados por RECONCILIAR. Esses pais podem segurar a disputa e alcançar o contador. Ocorrência real: **[nao verificado]**. (`app/chamados/reconciliador.py:638`, `app/chamados/reconciliador.py:651`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:111`, `app/chamados/reconciliador.py:626`.)

Também há divergência no contrato do callback: a documentação promete uma chamada por pai, mas a implementação chama por disputa preservada. O contador consegue deduplicar essas chamadas; o callback, isoladamente, não oferece essa garantia. (`app/chamados/reconciliador.py:559`, `app/chamados/reconciliador.py:618`, `app/chamados/reconciliador.py:626`, `app/chamados/models.py:1750`, `app/ponto/services/cartorio.py:979`.)

**4. Quem escreve e quem lê**

**É o mesmo juiz**, importado pelos dois helpers e pelo executor supra. C7 permanece outra pergunta, registrada: “a disputa pode fechar?”. (`app/ponto/services/cartorio.py:958`, `app/chamados/reconciliador.py:645`, `app/ponto/management/commands/supra_juiz.py:257`, `app/core/juizes.py:279`.)

| Campo/função | Quem escreve ou define | Quem lê na decisão auditada |
|---|---|---|
| `respondida_em` | Resposta humana, automática e recuperação pela trilha: `app/chamados/services/disputa_emissao.py:1468`, `:1653`, `:1674`; carimbo: `app/chamados/models.py:3133`; reversões: `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:1567`. | CSF: `app/ponto/supra_juiz.py:545`, `:548`; C7: `app/chamados/models.py:1845`; retenção: `app/chamados/juizes.py:2020`. |
| `resposta_colab` | `app/chamados/services/disputa_emissao.py:1467`, `:1652`; limpeza em `:1593`. | Isenção FIO-DUP: `app/ponto/services/cartorio.py:1691`; **CSF não lê texto**, lê carimbo em `app/ponto/supra_juiz.py:545`. |
| `disputa_origem` / `Disputa.chamado` | Relação declarada em `app/chamados/models.py:1750`; nascimento em `app/chamados/services/disputa_emissao.py:587`, `:1123`, `:1358`. | CSF: `app/ponto/supra_juiz.py:543`; lote E2: `app/chamados/reconciliador.py:649`; S88: `app/chamados/signals.py:33`. |
| `Disputa.perguntas` / `Pergunta.disputa` | Relação em `app/chamados/models.py:2417`; nascimento em `app/chamados/models.py:3015`. | CSF: `app/ponto/supra_juiz.py:544`; C7: `app/chamados/models.py:1854`. |
| `perguntas_que_resolvem` / `chamado_destino` | Relação em `app/chamados/models.py:3386`; nascimento/reuso em `:3015`, `:2983`; reaponte em `app/chamados/services/fio.py:926`. | CSF: `app/ponto/supra_juiz.py:547`; cartório: `app/ponto/services/cartorio.py:1439`, `:1695`; lote E2: `app/chamados/reconciliador.py:649`. |
| `status_local` | Núcleo `_gravar_estado`: `app/chamados/models.py:228`; nascimento: `app/chamados/models.py:1096`. | Motor: `app/chamados/models.py:674`; filtro novo: `app/chamados/reconciliador.py:651`; S88: `app/chamados/signals.py:34`. |
| `VIVOS` / `ENCERRADOS` | Fonte: `app/chamados/catalogo/motor.py:24`, `:30`. | Cartório usa VIVOS em `app/ponto/services/cartorio.py:1434`; helper E2 usa ENCERRADOS em `app/chamados/reconciliador.py:651`. |
| `fechada_em` | Fecha em `app/chamados/models.py:1933`; reabre em `:2150`, `:2174`. | E2: `app/chamados/reconciliador.py:620`, `:621`; S88: `app/chamados/signals.py:29`. |
| `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição conjunta em `app/chamados/models.py:3098`; persistência do carimbo em `:3153`; reversão em lote em `:3265`. | Seleção E2 em `app/chamados/reconciliador.py:663`; C7 em `app/chamados/models.py:1844`; retenção em `app/chamados/juizes.py:2019`. |
| `pode_executar_csf` | Definição pura em `app/ponto/supra_juiz.py:539`. | Supra em `app/ponto/management/commands/supra_juiz.py:261`; cartório em `app/ponto/services/cartorio.py:965`; E2 em `app/chamados/reconciliador.py:651`. |
| `_pais_com_conversa` | Produz mapa por disputa em `app/chamados/reconciliador.py:650`. | E2 em `app/chamados/reconciliador.py:617`, `:621`, `:626`; teste de custo em `app/ponto/tests/test_o168_guarda_csf_no_evento.py:282`. |
| `_fecham_cobrancas_sem_furo` | Produz lista apenas dos fechamentos confirmados em `app/ponto/services/cartorio.py:968`. | Quatro sítios: `app/ponto/services/cartorio.py:1054`, `:1589`, `:1602`, `:1623`. |
| E2: retorno e `ao_preservar` | Retorno continua número de perguntas carimbadas em `app/chamados/reconciliador.py:607`, `:634`; callback em `:626`. | Cartório em `app/ponto/services/cartorio.py:1050`; consumidor em `app/chamados/reconciliador.py:1399`; reconciliador em `:1466`. |
| `_csf_preservados_vistos` / `CHAVES_DO_ATO` | Criação/adição em `app/ponto/services/cartorio.py:978`; compartilhamento em `:1131`; declaração em `:2001`. | Deduplicação em `app/ponto/services/cartorio.py:979`; drenagem em `:1150`; selo em `app/ponto/tests/test_out_do_lote.py:195`, `:203`. |
| Filas/contadores existentes | Fechos alimentam `reconciliados`/`rejulgar` em `app/ponto/services/cartorio.py:1056`; corpo escreve `cods` em `:1527`. | Segunda passada em `app/ponto/services/cartorio.py:1060`; releitura/consolidação em `:1136`; devolução ao lote em `:1166`. |
| Tetos, registro de linhas e manifesto E4B | Literais alterados em `app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`; `app/core/tests/test_medida_turno_do_par.py:322`; `app/chamados/tests/e4b_c12_sitios.json:261`. | Cobrança do teto em `app/ponto/tests/test_selo_chokepoint_escrita.py:292`; igualdade das linhas em `app/core/tests/test_medida_turno_do_par.py:547`; manifesto em `app/chamados/tests/e4b_c12_verificar.py:17`, `:85`. |

O quadro identifica as portas e consumidores discriminados nesta auditoria; **censo global exaustivo de todos os leitores legados desses campos: [nao verificado]**.

**5. Classes que já reprovaram obra**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando autoridade | **Ausente nos helpers novos:** ambos chamam CSF; C7 continua registrado separadamente. (`app/ponto/services/cartorio.py:965`, `app/chamados/reconciliador.py:651`, `app/core/juizes.py:279`.) |
| Segundo escritor fora da porta | **Ausente no delta lido:** mantém `transicionar_com_pauta`, `carimbar` e `fechar`. (`app/ponto/services/cartorio.py:968`, `app/chamados/reconciliador.py:604`, `:629`.) |
| Relógio fora da borda | **Nenhuma leitura nova nos helpers.** A cadeia preexistente tem relógios em `carimbar`, `fechar` e `julgar_celula`; conformidade global com a borda **[nao verificado]**. (`app/chamados/models.py:3125`, `:1933`, `app/ponto/services/cartorio.py:2067`, `:2091`.) |
| Derivação paralela de fonte única | **Ausente nos helpers; permanece isenção não equivalente no FIO-DUP:** texto nas resolvedoras versus carimbo também na origem. (`app/ponto/services/cartorio.py:1691`, `app/ponto/supra_juiz.py:543`, `:547`.) |
| Selo/RED sem mordida | **Há pares controle/protegido de estado final** para os quatro sítios e cascata; o teste do evento seguinte declara provar somente estabilidade. Execução dos REDs/mutantes **[nao verificado]**. (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:114`, `:122`, `:131`, `:136`, `:143`, `:148`, `:157`, `:162`, `:227`, `:236`, `:259`.) |
| Ordem de trava | **Nenhuma inversão nova identificada:** E2 mantém trava do colaborador antes do carimbo; fechamento chama a porta de ordem. Implementação ordena Colaborador→Disputa→Chamado→Pergunta. (`app/chamados/reconciliador.py:601`, `app/chamados/models.py:1911`, `:2667`, `:2676`, `:2689`, `:2693`.) |
| Pouso antes do pré-requisito | **[nao verificado]**; suíte inteira precede opus, e Codex não aprova sozinho o pouso. (`CLAUDE.md:377`, `CLAUDE.md:380`.) | [nao verificado]
| ESMERIL-SEMÂNTICA | **Presente:** “pai vivo preservado” admite terminais absolutos e pode alimentar o contador. (`app/chamados/reconciliador.py:638`, `:651`, `:626`, `app/chamados/catalogo/motor.py:31`.) |

**6. Leis dos sítios**

- **O168 não está universalmente atendida nos caminhos solicitados:** persistem fechamentos sem consulta à guarda, especialmente lastro no consumidor e cascata S143/S128. (`app/ponto/services/lastro.py:401`, `app/chamados/services/disputa_emissao.py:2092`.)
- **Contador == universo:** há divergência demonstrável entre o rótulo “vivo preservado” e a seleção de pais terminais; é incompatível com o contrato anunciado e com a exigência de o rótulo dizer o que a conta faz. (`CLAUDE.md:35`, `app/ponto/services/cartorio.py:974`, `app/chamados/reconciliador.py:651`.)
- **L-061 e C7:** os helpers novos conservam leitura da célula e consulta aos juízes existentes; não identifiquei novo julgamento paralelo da lâmpada nesses helpers. (`app/docs/LEIS.md:95`, `app/chamados/reconciliador.py:604`, `:628`, `:661`.)
- **RELÓGIO-INJETADO:** o delta não acrescenta relógio; conformidade da cadeia inteira **[nao verificado]**, pelos sítios preexistentes discriminados acima. (`app/docs/CORTES.md:370`, `app/chamados/models.py:3125`, `app/ponto/services/cartorio.py:2067`.)
- **L-073, L-124, L-115 e L-113:** conformidade geral **[nao verificado]**; esta leitura não certifica teto temporal, horizonte, pareamento e cálculo/folha por ausência de alteração nesses mecanismos. (`app/docs/LEIS.md:100`, `:145`, `:151`, `:153`.)
- **E2-EMISSORES-COMPLEMENTO, LEI-TRANCA-E2-JUIZ e DOIS-AGENTES:** nenhuma violação adicional demonstrada nesta leitura; atendimento integral **[nao verificado]**. (`app/docs/CORTES.md:360`, `:427`, `:70`, `:259`.)

## 12/11 16:50 · sessao 151f9586a29f · - · - · origem=Ronald (tmux)

Censo ATA no perímetro literal conferido: **12 chaves de topo, 95 operações R/W, 27 arquivos**; excluindo `management`, **86 operações em 23 arquivos**. Unidade: operação AST com chave literal, incluindo escrita em construtor e em cópia apenas em memória; excluídos cópia integral `*`, chaves dinâmicas, aninhadas e lookup ORM. [não verificado: estatística reproduzida em Python em memória, sem arquivo de resultado]

| Chave | R | W | Tipo produzido | Escritor |
|---|---:|---:|---|---|
| `tipo_dia` | 5 | 1 | string ou `None` | `app/ponto/services/cartorio.py:738:ata_do_dia` |
| `lampadas` | 40 | 3 | lista de dict | `app/ponto/services/cartorio.py:738:ata_do_dia`; `app/ponto/services/cartorio.py:917:_ata_ate_o_teto`; `app/escala/utils.py:1892:montar_grade_prevista_periodo_por_turno` |
| `minutos_previstos` | 1 | 1 | int | `app/ponto/services/cartorio.py:739:ata_do_dia` |
| `minutos_realizados` | 3 | 1 | int | `app/ponto/services/cartorio.py:740:ata_do_dia` |
| `sem_ancora` | 1 | 1 | bool | `app/ponto/services/cartorio.py:741:ata_do_dia` |
| `n_orfas` | 4 | 1 | int | `app/ponto/services/cartorio.py:742:ata_do_dia` |
| `orfas` | 10 | 2 | lista de strings HH:MM | `app/ponto/services/cartorio.py:745:ata_do_dia`; `app/escala/utils.py:1895:montar_grade_prevista_periodo_por_turno`; formatação em `app/escala/utils.py:1832` |
| `n_missing` | 3 | 3 | int | `app/ponto/services/cartorio.py:746:ata_do_dia`; `app/ponto/services/cartorio.py:812:dispensar_por_ausencia`; `app/ponto/services/cartorio.py:917:_ata_ate_o_teto` |
| `n_celulas` | 4 | 1 | int | `app/ponto/services/cartorio.py:762:ata_do_dia` |
| `cobertura_parcial` | 3 | 1 | bool, `True` quando presente | `app/ponto/services/cartorio.py:767:ata_do_dia` |
| `periodo_parcial` | 3 | 1 | enum string: `sem_periodo`, `dentro`, `fora`, `nao_sei` | `app/ponto/services/cartorio.py:771:ata_do_dia`; origem `app/ponto/turnos.py:2618` |
| `precedencia` | 1 | 1 | dict | `app/ponto/portas/celula.py:1084:carimbar_precedencia`; conteúdo em `app/ponto/services/cartorio.py:2000` |

Os dois escritores de `app/escala/utils.py` são cópia em memória: a cópia do objeto está em `app/escala/utils.py:1886`; os demais caminhos de persistência passam pela porta que atribui `cel.ata` em `app/ponto/portas/celula.py:1006` e `app/ponto/portas/celula.py:1033`.

`lampadas` contém `tipo`, `hora`, `acesa`, `luz`, `tipo_real`, mais opcionais `isencao`, `dispensada`, `pendente`: construção em `app/ponto/services/cartorio.py:702`, `app/ponto/services/cartorio.py:720`; opcionais em `app/ponto/services/cartorio.py:591`, `app/ponto/services/cartorio.py:596`, `app/ponto/services/cartorio.py:915`. `luz` guarda HH:MM ou `None`; `hora` é marco previsto ou rótulo posicional `·I#`. `app/ponto/services/cartorio.py:703`; `app/ponto/juiz_batida.py:193`

Comparações de hora sem data comprovadas:

- `orfas`: ordenação e distância de minutos em `app/ponto/services/cluster_espurio.py:34` e `app/ponto/services/cluster_espurio.py:35`; pertinência de HH:MM em `app/ponto/services/vigia_pareador_celula.py:110` e `app/ponto/services/vigia_pareador_celula.py:111`.
- `lampadas.hora`: igualdade com hora pedida em `app/ponto/services/lastro.py:90`; igualdade entre marcos ATA/DNA em `app/ponto/juiz_batida.py:230`; comparação com DNA em `app/ponto/services/cartorio.py:848`.
- `lampadas.luz`: resposta `data_hora` perde a data em `app/chamados/juizes.py:1349`; depois compara minutos circulares com a luz em `app/chamados/juizes.py:1358` e `app/chamados/juizes.py:1362`.
- A reconstrução atual da data da luz está em `app/ponto/juiz_batida.py:97` e `app/ponto/juiz_batida.py:215`.

Fora do perímetro literal informado, há leitor ORM de `lampadas.pendente` em `app/chamados/models.py:3356`, projeção `values('ata')` em `app/chamados/reconciliador.py:879` e cópia dinâmica da chave `precedencia` por laço em `app/ponto/portas/celula.py:997`, cuja declaração está em `app/ponto/portas/celula.py:104`.

A diferença aritmética em relação às 72 do aval é **−60 chaves de topo**; a razão da divergência é [não verificado]. A lista completa de leitores por chave e declarações de parâmetros/retornos foi enviada nas mensagens anteriores. [não verificado: registro das mensagens]

Os tipos acima são os produzidos pelo código; o conteúdo efetivamente presente no banco é [não verificado]. A porta aceita `dict(ata)` em `app/ponto/portas/celula.py:995`, e o campo é JSON sem esquema declarado em `app/escala/models.py:1778`.

## 12/11 16:49 · sessao e2e181b5434c · - · - · origem=Ronald (tmux)

Complemento da exploração, só leitura:

1. **Fábrica do consumidor e do cartório alcança a cascata c1 já descoberta.** Consumidor chama fábrica em `app/chamados/reconciliador.py:1421`; fábrica percorre `perguntar_por_eventos` → `perguntar_pela_celula` → `perguntar` em `app/chamados/services/fabrica_da_celula.py:301`, `:219`, `:172-174`. A geração de perguntas chama `reatar_pergunta_orfa` em `app/chamados/services/disputa_emissao.py:459-464`; este pode chamar `lavrar_sem_turno_aberto` em `app/chamados/reconciliador.py:1653`, que fecha disputa sem consultar CSF em `:412-414`. O cartório chama a mesma fábrica em `app/ponto/services/cartorio.py:1076-1088`.

2. **Há cascata adicional S143/S128 → disputa.fechar → S88 sem CSF.** O ramo histórico de reatar chama `resolver_perguntas_por_chamado_resolvido` em `app/chamados/reconciliador.py:1682`; `_retratar` também o chama em `:537`; qualquer save terminal que escreva status acorda S143 em `app/chamados/signals.py:91-117`. O resolver forma candidatos incluindo todas as disputas abertas do colaborador em `app/chamados/services/disputa_emissao.py:2026-2028`; fecha candidatos pelo C7, com veto de pergunta em voo, em `:2089-2092`. Não chama CSF nesse fecho. O filtro de pai/destino vivo em `:2032-2037` limita o **carimbo sem credor**, não o laço final de fechamento em `:2074-2092`.

3. **Emissor chamado pelo cartório alcança outro auto-fecho.** `julgar_celula` injeta `detectar_ausencias.Command` em `app/ponto/services/cartorio.py:2066`, `:2091-2092`; corpo chama `_abrir_chamado` em `:1841`, `:1879`. Esse emissor chama `criar_disputa_a_partir_de_chamados_orfaos` em `app/ponto/management/commands/detectar_ausencias.py:792-803`. A porta de repasse fecha a disputa automaticamente quando C7 permite e não houve abstenção por ata em `app/chamados/services/disputa_emissao.py:1270-1287`, sem CSF.

4. **Agrupamento LIMBO também pode fechar pelo ramo do emissor**, com regra própria: emissor chama `agrupar_furo` em `app/ponto/management/commands/detectar_ausencias.py:759-763`; agrupador reconcilia em `app/chamados/services/limbo_folgas.py:284-287`, depois da costura das vivas e do carimbo de agrupamento (`:275-283`), sem CSF. É agrupamento sob pai vivo, portanto não afirmo que seja fecho “sem furo” abrangido pela lei O168: **[nao verificado]**.

5. **Outro ramo explícito do cartório é cluster:** chama `cluster_espurio.reconciliar_colab` em `app/ponto/services/cartorio.py:1081-1085`; esse reconcilia chamados que a premissa matou em `app/ponto/services/cluster_espurio.py:187-191`, sem CSF. O módulo é `cluster_batida_espuria` (`:11`), diferente da cobrança sem furo tratada no novo helper; aplicabilidade exata da lei O168 a esse domínio: **[nao verificado]**.

6. **Não encontrei fecho direto no dedup de nascimento de pergunta lido.** O reuso atualiza célula/destino e retorna em `app/chamados/models.py:2922-3009`; nascimento grava pergunta e retorna em `:3014-3032`. O núcleo de criação/agregação de disputa reutiliza aberta ou cria nova, sem fechar, em `app/chamados/services/disputa_emissao.py:530-603`. O fecho relevante está nos ramos acima, e não nessas operações de dedup.

São caminhos estruturais com chamada real de fechamento, sujeitos às demais guardas. Ocorrência concreta, resultado de execução e existência de dados que atravessem todas as condições: **[nao verificado]**.

## 12/11 16:49 · sessao b4c3d76f5cee · - · - · origem=Ronald (tmux)

DNA: 9 chaves de topo escritas pelo produtor; `DNA_CHAVES` declara apenas 8 e omite `extra_declarada`. Referências: `app/ponto/management/commands/gerar_celulas.py:17`, `:95`, `:122`.

| Chave | Escritor em `gerar_celulas.py:montar_dna` | Tipo produzido | Leitor por chave |
|---|---|---|---|
| `dna_versao` | :96 | int, atualmente 2 (:16) | `app/ponto/services/cartorio.py:484:impressao_insumos` |
| `vinculo_id` | :97 | ID do vínculo | Nenhum acesso por chave identificado; blob inteiro é comparado em `app/ponto/portas/celula.py:263`. |
| `tipo_escala_id` | :98 | ID do tipo | Nenhum acesso por chave identificado; comparação do blob em `app/ponto/portas/celula.py:263`. |
| `tipo_ciclo` | :99 | string (`app/escala/models.py:117`) | `app/ponto/services/vigia_entrada.py:129:do_dia` |
| `marcos` | :100 | dict ou null (:77,:83) | 26 acessos AST, lista abaixo. |
| `minutos_jornada` | :101 | int (`app/escala/models.py:49`) | Nenhum acesso por chave identificado; comparação do blob em `app/ponto/portas/celula.py:263`. |
| `intervalo_livre` | :102 | bool | Nenhum acesso por chave identificado; comparação do blob em `app/ponto/portas/celula.py:263`. |
| `lampadas` | :103 | list[dict] ou null (:78,:94) | `app/escala/utils.py:153:lampadas_de_dna` |
| `extra_declarada` | :122 | dict `{antes:int,depois:int}` (:119,:120) | `app/escala/models.py:1235:extra_declarada_do_dia` |

Esses são os tipos produzidos pelo código; conteúdo efetivamente armazenado em produção: [não verificado].

Leitores de `marcos`, contando chamadas AST e preservando duas chamadas na linha 159:

```text
app/chamados/juizes.py:948:fato_do_marco_no_espelho
app/chamados/juizes.py:1338:_resposta_concorda_com_a_luz
app/chamados/services/adesao.py:36:universo_adesao
app/escala/models.py:1124:marcos_do_dia
app/escala/models.py:1189:pausas_do_dia
app/escala/utils.py:53:pausas_dna_periodo
app/escala/utils.py:104:marcos_dna_periodo
app/escala/utils.py:137:pausas_de_celulas
app/pautas/services.py:424:contexto_do_dia_texto
app/ponto/juiz_batida.py:159:periodos_do_dia [2 chamadas]
app/ponto/management/commands/aplicar_col900.py:172:handle
app/ponto/management/commands/censo_col900.py:118:handle
app/ponto/management/commands/censo_col900.py:119:handle
app/ponto/management/commands/detectar_intervalo_ausente.py:53:furos_intervalo
app/ponto/management/commands/tripwire_celulas.py:99:medir_pausa_faltando
app/ponto/management/commands/tripwire_celulas.py:259:medir_sobra_com_tipo_trocado
app/ponto/services/cartorio.py:484:impressao_insumos
app/ponto/services/cartorio.py:615:lampadas_do_dia
app/ponto/services/cartorio.py:840:insumos_da_ata
app/ponto/services/dia_do_colab.py:46:dia_do_colab
app/ponto/services/diagnostico_escala.py:153:diagnostico_escala
app/ponto/services/esmeril_espelho.py:116:assinaturas_do_colab
app/ponto/services/faltas_de_hoje.py:43:faltas_de_hoje_sem_cobranca
app/ponto/services/veredito_celula.py:31:_marcos_do_dia
app/ponto/services/vigia_entrada.py:126:do_dia
```

As subchaves `marcos.hi/hf/hii/hfi` contêm HH:MM ou null; `hii2/hfi2` são opcionais. Escritas: `app/ponto/management/commands/gerar_celulas.py:83`, `:93`; vocabulário: `app/escala/models.py:1652`.

`lampadas[]` contém `{chave,tipo,hora,instante}`: `hora` é HH:MM; `instante` é ISO **sem fuso**, obtido por `datetime.combine(...).isoformat(...)`. Referências: `app/ponto/management/commands/gerar_celulas.py:68`, `:69`.

Comparação de hora sem data existe para `marcos`:

```text
app/ponto/services/cartorio.py:640:lampadas_do_dia [pertinência]
app/ponto/services/cartorio.py:648:lampadas_do_dia [chave de dict]
app/ponto/services/cartorio.py:650:lampadas_do_dia [consulta de dict]
app/ponto/services/cartorio.py:657:lampadas_do_dia [consulta de dict]
app/ponto/services/cartorio.py:666:lampadas_do_dia [distância em minutos]
app/ponto/services/cartorio.py:846:insumos_da_ata._um [igualdade; hi/hf passados em :855]
app/chamados/juizes.py:965:fato_do_marco_no_espelho [chave de dict]
app/chamados/juizes.py:968:fato_do_marco_no_espelho [consulta de dict]
app/chamados/juizes.py:1345:_resposta_concorda_com_a_luz [igualdade]
app/ponto/juiz_batida.py:230:periodos_do_dia [igualdade dos minutos]
app/ponto/services/vigia_entrada.py:127:do_dia [comparação hi-hf]
app/ponto/services/dia_do_colab.py:47:dia_do_colab [pertinência]
app/ponto/management/commands/censo_col900.py:120:handle [igualdade do dict]
```

Também existe para `lampadas`: `app/escala/utils.py:155:lampadas_de_dna` usa `lamp['hora']` como chave de dict.

Comparações do blob inteiro incluem `app/ponto/portas/celula.py:263`, `:594`, `:872` e `app/ponto/management/commands/tripwire_celulas.py:30`; portanto ausência de leitor individual não significa ausência de uso.

Mapa para o instrumento:

```text
PRODUTOR:
app/ponto/management/commands/gerar_celulas.py:73:montar_dna
  retorna dna em :123

PARÂMETROS DECLARADOS DNA:
app/escala/utils.py:149:lampadas_de_dna                 posição0 dna
app/ponto/services/cartorio.py:471:impressao_insumos    posição3 dna
app/ponto/services/cartorio.py:599:lampadas_do_dia      posição2 dna
app/ponto/services/cartorio.py:730:ata_do_dia           posição2 dna
app/ponto/services/cartorio.py:816:insumos_da_ata       posição1 dna

DESEMPACOTAMENTO ORM:
app/escala/utils.py:52:pausas_dna_periodo
app/escala/utils.py:103:marcos_dna_periodo
app/escala/utils.py:136:pausas_de_celulas
app/escala/utils.py:164:lampadas_dna_periodo
app/chamados/services/adesao.py:33:universo_adesao

APELIDO DE RETORNO:
app/ponto/management/commands/censo_col900.py:115:handle
  cand = montar_dna(...); leitura cand.get('marcos') em :118
```

`alvo` em `app/ponto/portas/celula.py:783` vem de `dna_anterior`; a restauração escreve o blob inteiro em `:786`, lê `marcos` em `:788` e `vinculo_id` em `:789`. Esses leitores não foram contados como acessos ao DNA atual.

Contagem AST limitada aos acessos explícitos às nove chaves de topo, em `app/**/*.py`, sem testes nem migrations: 30 leitores em 17 arquivos + 9 escritores em um arquivo adicional = 39 acessos em 18 arquivos; subtotal management = 15; restante = 24. Os locais acima e o literal escritor em `app/ponto/management/commands/gerar_celulas.py:95` sustentam a contagem. Não inclui blob inteiro, DNA anterior ou chaves aninhadas.

_4367 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
