# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 01:57** · publicado em **11/11/2026 01:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 01:57 · sessao c24104626e4d · - · - · origem=Ronald (tmux)

| Achado | Veredito | Prova/input | Origem e cura fora dos proibidos |
|---|---|---|---|
| `[ch]` recostura P de intervalo externo no genérico | **PROCEDENTE; mesma classe do achado 1 da triagem5.** | Índice só fotografa o lote (`app/chamados/services/disputa_emissao.py:1296`); ausência da chave conserva `ch` (`:1044`); reuso atribui/salva (`app/chamados/models.py:2982`, `:3006`). Input: P `intervalo_saida`/D destinada ao intervalo PK1 em dona estrangeira; lote contém apenas genérico PK2. Ensaio em memória retornou destino PK2, em dona aberta e fechada; ORM **[não verificado]**. Double explicitamente descrito em `app/chamados/tests/test_bo_disputa_volta5l_memoria.py:64`. | Escritor efetivo desse input: `models.py:2982`. **Cabe fora:** a guarda compartilhada `_costura_nao_mata_o_fio` já é chamada pelo reuso (`models.py:2980`) e pelo outro escritor (`fio.py:909`); retornar falso preserva destino e mantém retorno da canônica (`models.py:2984`, `:3008`). Implementação/ORM da cura **[não verificado]**. |
| Lote escolhe primeiro; avulso escolhe menor PK | **PROCEDENTE como divergência estrutural; mecanismo NOVO c21, mesma classe de derivação paralela do achado 2.** | `setdefault` e ordem do lote (`disputa_emissao.py:1297`, `:1299`) versus `sorted(...pk)` (`:291`). Input: dois intervalos PK1/PK2 com `S`/D; lote `[PK2,PK1]` produz destino PK2; helper avulso retorna PK1. Ensaio em memória confirmado; ORM **[não verificado]**. Coerência/determinismo exigidos em `CLAUDE.md:176`. | Eleições escrevem pedidos em `disputa_emissao.py:293`, `:1299`; persistências em `models.py:2982`, `fio.py:925`. **Cabe fora:** unificar eleição e proteção na autoridade compartilhada permitida; qual PK deve vencer por mérito **[não verificado]**. |
| Replay conserva argumento `None`, mas porta rederiva | **PROCEDENTE como propriedade do código; residual anterior à c21.** | Gerador conserva origem (`disputa_emissao.py:545`), replay repassa argumentos (`:766`), mas `None or derivador(dona)` reconsulta dona (`models.py:2866`). Input concreto: `motivo='foto_ausente'`, argumento `batida_origem_id=None`, dona com `contexto={'ultima_batida':{'id':71}}`, batida71 do colaborador existente: `_bo=71`, não `None` (`models.py:3461`, `:3463`, `:3467`; família em `app/chamados/catalogo/motivos.py:43`). Reprodução ORM **[não verificado]**. | Seleção do replay: `models.py:2866`, arquivo proibido. Há também rederivação na inserção (`models.py:3448`, `:3449`). **Não certificar cura integral em uma linha.** |
| Derivação duplicada índice/B | **DESCARTADO: já coberto pela fotografia c21.** | Uma fotografia em `disputa_emissao.py:1296`, consumida no índice (`:1298`) e B (`:1398`). | — |
| Conversão civil→turno nesse helper | **DESCARTADO: leitura errada.** | `dia_do_turno_da_celula` retorna `DiaDoTurno(dia)` (`app/ponto/turnos.py:2943`), `NewType` (`app/core/dia_do_turno.py:13`). | — |
| Regularizável pode ter fotografia vazia | **DESCARTADO como defeito autônomo: descrição verdadeira, sem input admissível obrigatório demonstrado.** | Regularizabilidade delegada à fábrica (`disputa_emissao.py:1009`), helper exclui data inválida (`:274`). Input `marcos_faltantes=[{'tipo':'S','data':'invalida'}]` retorna `()` (`:273`, `:275`, `:278`). | Exigência de perguntar apesar de dados inválidos **[não verificado]**. |
| Helper não total para qualquer JSON | **DESCARTADO como novidade/bloqueio desta cura: pré-condições já estavam no código removido pelo patch anexo [não verificado].** | Input `contexto_json=['truthy']` gera AttributeError; `{'marcos_faltantes':7}` gera TypeError no `for`, ambos fora do try (`disputa_emissao.py:269`, `:272`). | Situação produtiva **[não verificado]**; não nasceu pela captura estreita de c21. |
| Troca de `Exception` por T/V altera objetos extraordinários | **DESCARTADO: fora do JSON ordinário; nenhuma ocorrência concreta fornecida.** | `dict` extraordinário cujo `.get('data')` lança RuntimeError propagaria (`disputa_emissao.py:273`, `:274`); entrada persistida desse tipo **[não verificado]**. | — |
| Tipo diferente de `S` vira volta | **DESCARTADO como novidade: comportamento preservado do patch anexo [não verificado].** | Input `{'tipo':'X','data':'2026-07-08'}` produz `intervalo_volta` (`disputa_emissao.py:276`). Obrigação de outro tratamento **[não verificado]**. | — |
| Detector escreve data civil potencialmente D+1 | **DESCARTADO como escritor persistente atual: emissão aposentada.** | Data encadeada civil em `app/ponto/management/commands/detectar_intervalo_ausente.py:82`; aposentadoria e `continue` em `:149`, `:156`. | — |
| Booleano da abertura inicial da declaração descartado | **DESCARTADO: booleano não é necessário para a decisão executada.** | Primeiro retorno confere a dona (`disputa_emissao.py:834`, `:835`); replay confere criação e identidade (`:842`, `:843`). Input que cause saída errada apenas por ignorar booleano **[não verificado]**. | — |
| Novos escritores persistentes fora da porta | **DESCARTADO: censo confirma escritores já registrados.** | `models.py:2982`, `:3014`; `fio.py:925`; contrato em `app/core/tests/test_contract_escritores_estado.py:61`. | Defeito é regra não aplicada aos escritores, não escritor novo. |
| Campos com dois significados | **DESCARTADO como acusação genérica; PROCEDENTE apenas na oscilação de destino já contabilizada.** | Cenários DS (`models.py:1743`) e identidade P (`:2427`) são campos distintos; conversa usa porta/data própria (`:2794`, `:2801`). | Oscilação fica no primeiro achado. |

Menor hunk **para impedir especificamente a rederivação de `None` na seleção do replay**, uma linha em `app/chamados/models.py:2866`:

```diff
-                _bo = campos.get('batida_origem_id') or cls.batida_origem_do_nascimento(disputa, motivo)
+                _bo = campos['batida_origem_id'] if 'batida_origem_id' in campos else cls.batida_origem_do_nascimento(disputa, motivo)
```

Esse hunk não certifica a inserção, porque `save` chama `_chave_de_identidade` (`models.py:3420`, `:3421`) e ela rederiva origem nula (`:3448`, `:3449`).

Checklist de referências neutras conferidas: atribuições temporárias/restauradas (`fio.py:779`, `:783`); reatribuição do mesmo PK (`models.py:366`, `:367`); FK SET_NULL (`:3387`); declarações colab/admin (`declaracao_texto.py:365`, `:653`); cadeia estática analisar→signal→cartório→fábrica (`models.py:806`, `signals.py:216`, `app/ponto/services/cartorio.py:1008`, `fabrica_da_celula.py:172`, `:174`, `:219`); contexto persistido (`contexto.py:76`, `:102`); grade usa categoria Ausencia e marcos sem data (`reconciliar_grade.py:498`, `:502`); data atribuída por porta (`fio.py:800`); materialização lê destino/data (`materializacao.py:1076`, `:1079`, `:1345`). Execução real dessa cadeia e efeitos em produção **[não verificado]**.

## 11/11 01:56 · sessao 40cf756ad74a · - · - · origem=Ronald (tmux)

| Achado | Veredito | Prova/origem | Fora dos proibidos? |
|---|---|---|---|
| `motivo=None` permite fechar sem consultar ata | **PROCEDENTE**, mesma classe da triagem5 item 5; não introduzido pela c21. Input: chamado genérico datado, `_tem_base=True`, `_classificar_falta → (None,None,None)`, ata ausente. Resultado no ensaio AST: fechada, zero consultas à ata, zero luz. ORM/prod **[não verificado]**. | `app/chamados/services/disputa_emissao.py:1487-1490` pula a leitura; `:1565-1570` solicita fechamento; escritor de `fechada_em`: `app/chamados/models.py:1932`. A HX exige conservar aberta sem ata: `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51-61`. | **Sim**: gate/ordenação no emissor, antes dos `continue` em `:1447` e `:1488`. |
| Veto adicional para sem-data/base/marco inválido/falha de leitura | **LEI para conservar essa extensão**; autorização literal específica **[não verificado]** nos avais fornecidos. É novo no diff c21 **[não verificado: anexo sem linhas]**. | Flags em `app/chamados/services/disputa_emissao.py:1097`, `:1103`, `:1397`, `:1453`, `:1482`, `:1485`; veto em `:1562`. Juiz único declarado: `app/chamados/models.py:1848-1853`, `:1858-1863`; `app/docs/JUIZES.md:62`; escopo literal `CLAUDE.md:37`. | **Sim** para restaurar o escopo HX/C7 existente no emissor; manter a extensão exige decisão de lei. |
| Aviso afirma ata ausente sem ter consultado ata | **PROCEDENTE**, novo c21 **[não verificado: diff anexo sem linhas]**. Input: chamado datado, sem origem, `_tem_base=False`, ata existente/vazia. Ensaio AST: aberta, zero consultas à ata, aviso acionado. | Origem da classificação indevida: `app/chamados/services/disputa_emissao.py:1485`, `:1562-1564`; mensagem persistida somente no logger: `app/chamados/services/declaracao_texto.py:70-72`. | **Sim**: emissor/porta da luz. |
| I7a/I7b não leem o WARNING | **DESCARTADO** como defeito: o aval exige contadores do estado da disputa, não interpretação de log **[não verificado: anexo sem linhas]**. | Contadores consultam abertas sem linhas P e a autoridade da ata: `app/chamados/management/commands/auditar_invariantes_chamados.py:62-74`. | — |
| Extensão técnica não tem contador próprio | **DESCARTADO**: ausência de contador exclusivo não significa falta de leitura; aberta sem P e sem dia entra I7a. | `app/chamados/management/commands/auditar_invariantes_chamados.py:64-73`. | — |
| H2 com ata lida e nenhum faltante deixou de fechar | **DESCARTADO**, já coberto. | Contraste protegido: `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:63-79`; caso c21: `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:185-191`. | — |
| Regularizável pode ter fotografia vazia | **DESCARTADO como bloqueador autônomo**: leitura correta, mas não prova violação adicional dos avais. | Regularizabilidade trata admissão pela fábrica: `app/chamados/services/disputa_emissao.py:1003-1009`; fotografia exige dados válidos: `:266-276`. | — |
| Helper não total para qualquer JSON | **DESCARTADO como achado novo**: `.get` e iteração desprotegidos já existiam no índice do patch fornecido **[não verificado: anexo sem linhas]**; ocorrência produtiva **[não verificado]**. | Sítios atuais: `app/chamados/services/disputa_emissao.py:269-274`. | — |
| `RuntimeError` extraordinário deixou de ser capturado no B | **DESCARTADO**: exemplo demanda objeto extraordinário, não JSON ordinário; não demonstra regressão global nem ocorrência na produção **[não verificado]**. | Captura atual `app/chamados/services/disputa_emissao.py:272-275`; captura antiga somente no patch anexo **[não verificado]**. | — |
| Tipo diferente de `S` vira volta | **DESCARTADO como novo defeito**: comportamento mantido pelo patch **[não verificado: anexo sem linhas]**; regra autorizando rejeição de outros tipos **[não verificado]**. | `app/chamados/services/disputa_emissao.py:276`. | — |
| Conversão civil→turno nesta derivação | **DESCARTADO**, leitura errada. | `app/core/dia_do_turno.py:13`; identidade em `app/ponto/turnos.py:2939-2943`. | — |
| Detector prova procedência atual de `marcos[].data` | **DESCARTADO**: a emissão desse detector está aposentada. | `app/ponto/management/commands/detectar_intervalo_ausente.py:149-156`. | — |

**Para c22:** corrigir o caminho sem testemunho de ata antes dos `continue`, preservar o contraste protegido HX/h2 e corrigir a falsa mensagem de ausência de ata. (`app/chamados/services/disputa_emissao.py:1447`, `:1488`, `:1562-1567`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51-79`.)

**Para Opus:** decidir somente se pretende manter uma nova regra de conservação da disputa para sem-data/base/marco inválido/falha técnica; essa extensão específica não está escrita nos avais fornecidos **[não verificado]**. Não há necessidade demonstrada de editar `models.py` para corrigir os achados deste recorte. (`app/chamados/services/disputa_emissao.py:1485`, `:1562`; juiz existente em `app/chamados/models.py:1848-1853`.)

## 11/11 01:56 · sessao dcc8ff78aebf · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 11/11 01:56 · sessao 69cf86931e4d · - · - · origem=Ronald (tmux)

Triagem referenciada para consolidação:

- **Harness:** não basta substituir `sys.modules`. O teste instala filhos falsos (`django.db`, `escala.models`, `chamados.services.nota`) em `app/chamados/tests/test_e4b_classe_e_memoria.py:189`, mas a produção importa `nota` e `tarefa_do_dia` pelo pacote pai em `app/chamados/services/disputa_emissao.py:1810` e `app/chamados/management/commands/backfill_fio_morto_e4b.py:80`. Atributos preexistentes no pai podem devolver o módulo real, enquanto seus imports internos encontram filhos falsos. O double de `django.db` não tem `IntegrityError` (`test_e4b_classe_e_memoria.py:191`), e o de escala não tem `EscalaColaborador` (`test_e4b_classe_e_memoria.py:204`); os imports reais falham em `app/chamados/services/nota.py:83` e `app/ponto/turnos.py:2975`.

- **Reprodução em memória:** o teste `test_P3_carga_inclui_validada_ressuscitavel` passa sozinho; pré-carregando no pacote pai `chamados.services.nota` com o corpo real de `anexar_ocorrencia`, produz exatamente `ImportError: cannot import name 'IntegrityError' from '<unknown module name>'`. Caminho: `app/chamados/tests/test_e4b_classe_e_memoria.py:580` → `:486` → `app/chamados/services/disputa_emissao.py:1846` → `app/chamados/services/nota.py:83`. A execução usou `PYTHONDONTWRITEBYTECODE=1`, sem escrita.

- **Cura mínima do harness:** contexto único que substitua e restaure tanto filhos em `sys.modules` quanto atributos correspondentes dos pacotes pais; usar esse contexto em `resolver`, `_porta`, `_comando` e nos blocos próprios de testes. Esses pontos hoje usam somente `patch.dict` em `app/chamados/tests/test_e4b_classe_e_memoria.py:235`, `:413`, `:479`, `:503`. Não remediar importando modelos reais ou acrescentando apenas `IntegrityError`: isso preservaria a fuga para código real. As duas fugas são demonstradas pelos imports anteriores.

- **Contrato de vitalidade:** o juiz é `chamados.juizes.pergunta_viva`/`q_vivas`, explicitamente em `app/chamados/tests/test_contract_leitores_vitalidade.py:2` e `:321`; a porta E4B pergunta corretamente a C6 em `app/chamados/services/disputa_emissao.py:1812`. O ofensor novo `:1789` entrou no delta c3.

- **Ressalva decisiva para a cura do contrato:** `:1789` enumera carga de escrita, reproduzindo o conjunto efetivamente relavrado por `_gravar_estado` em `app/chamados/models.py:309`. Substituí-lo por `q_vivas` pode omitir perguntas mortas por C6 que o escritor ainda relavra; C6 também mata por tranca, identidade, fato e célula, além de validação (`app/chamados/juizes.py:1324`). O contrato reconhece exceções estritas para enumeração/elegibilidade de escritor (`app/chamados/tests/test_contract_leitores_vitalidade.py:107`, `:132`) e explica por que substituir uma guarda de escritor por C6 pode mudar sua pergunta (`:62`). Cura coerente: compartilhar o seletor do escritor mediante hunk autorizado em `models.py`, ou justificar uma entrada estrita da função na allowlist como **carga de escrita**, mantendo C6 na decisão de vitalidade e acrescentando contraprova de carga completa. Não maquiar a comparação para escapar do AST.

- **Sumido F1 h:** força K6 entregue e exige chamado resolvido em `app/chamados/tests/test_o261e_f1_porta_encerramento.py:113`. O c3 introduziu consulta de competência e motivo impeditivo em `app/chamados/services/disputa_emissao.py:1826`, e somente chama reabrir se não houver motivo em `:1840`. O S143 também interrompe a sequência após essa recusa em `:1914`. Portanto existe mudança comportamental pertinente ao sumiço. O teste usa relógio congelado (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:46`). Sem traceback baseline, qual assert falhava antes: **[nao verificado]**; intermitência: **[nao verificado]**.

- **Sumido R4b:** cria exportação vigente em `app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:101`, reata e exige preservar `resolvido` em `:312`. A cerca nova acima impede renascimento por E4B; a composição K6 lê a exportação vigente por janela em `app/chamados/services/vigia_fio.py:822`. Compatível com correção comportamental do c3. Causa histórica exata da falha suite1 e intermitência: **[nao verificado]**.

- **Não confirmar “24 comuns sem tocar delta”:** os 14 ERRORs comuns são do próprio arquivo alterado `app/chamados/tests/test_e4b_classe_e_memoria.py`; os tracebacks dados atravessam `app/chamados/services/disputa_emissao.py:1913`, `:1841`, `:1846` ou `app/chamados/management/commands/backfill_fio_morto_e4b.py:83`. Portanto essa afirmação é falsa. Os 24 comuns se decompõem em 14 ERRORs do harness e 10 FAILs; os dois conflitos de passivo continuam em `app/chamados/tests/test_e4_passivo.py:62` e `:95`. A pertença aos conjuntos vem dos anexos sem numeração: **[nao verificado]** como referência arquivo:linha.

## 11/11 01:56 · sessao 16c3dd523cd9 · - · - · origem=Ronald (tmux)

**1+4: alcance da antecipação e da reversão insuficiente.** O backfill expande a carga somente das âncoras das perguntas do dia; os demais chamados vivos entram em `chs` sem expansão das perguntas/disputas que a ressuscitação poderá escrever (`app/chamados/management/commands/backfill_fio_morto_e4b.py:197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:200`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:206`).

O caminho completo está no código: `reabrir()` → `_gravar_estado()` → `save()` → receiver → `julgar_celula()` → `x.ressuscitar_perguntas()` para outros vivos do dia (`app/chamados/models.py:1442`, `app/chamados/models.py:296`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:1602`, `app/ponto/services/cartorio.py:1633`). A autoridade da ressuscitação pode devolver perguntas de outro dia e reabrir disputas fechadas: o seletor parte de `perguntas_que_resolvem`, sem recorte de data, e a escrita chama `reabrir_sistema()` (`app/chamados/models.py:1483`, `app/chamados/models.py:1566`, `app/chamados/models.py:1574`).

Existe ainda uma segunda expansão que o brief deve exigir explicitamente: havendo adiados pela lâmpada, o cartório chama E2 para o colaborador inteiro; seu queryset não filtra data/competência, o juiz filtra em Python, a porta carimba e pode fechar a disputa (`app/ponto/services/cartorio.py:962`, `app/ponto/services/cartorio.py:970`, `app/chamados/reconciliador.py:627`, `app/chamados/reconciliador.py:661`, `app/chamados/reconciliador.py:595`, `app/chamados/reconciliador.py:611`). Portanto, “expandir todas as cargas dos vivos” cura o contraexemplo anunciado, mas sua suficiência para todas as escritas da cascata permanece **[nao verificado]**.

A ordem canônica é C→D→Ch→P; cada grupo em PK crescente, e `trava_na_ordem` é o único adquirente das quatro famílias (`app/chamados/models.py:2631`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:2692`). A expansão deve terminar **antes** da aquisição de D/Ch/P, e a cerca de exportada/limbo precisa consultar essa mesma carga, não somente a carga do credor direto (`app/chamados/management/commands/backfill_fio_morto_e4b.py:208`, `app/chamados/services/disputa_emissao.py:1820`, `app/chamados/services/disputa_emissao.py:1826`, `app/chamados/services/disputa_emissao.py:1828`).

**Reversão:** `_foto_do_dia()` fotografa somente P do dia; é chamada antes de entrar no atomic e antes de `_travar_lote()` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:215`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:303`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:307`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:310`). Deve fotografar sob trava a carga efetiva inteira e reler a reversão antes de escrever, preservando o requisito já implementado em `_gravar_reversao()` (`app/chamados/management/commands/backfill_fio_morto_e4b.py:52`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:59`).

Campos cuja fotografia atual é insuficiente:

| Família | Escrita a preservar |
|---|---|
| P: assinatura e carimbo | Ressuscitação limpa `validada_em`, `validada_por`, `respondida_em` e descarimba; fotografia inclui `validada_em`, mas omite `validada_por` (`app/chamados/models.py:1566`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:219`). |
| P: lavra | S2 relavra perguntas de destino não validadas; `lavrar` escreve `veredito_lavrado`, `julgado_em`, `janela_expira_em`; trio ausente da fotografia (`app/chamados/models.py:309`, `app/chamados/veredito_lavrado.py:270`, `app/chamados/veredito_lavrado.py:275`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:219`). |
| D: fechamento | Reabertura limpa `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; fotografia registra apenas IDs das disputas abertas (`app/chamados/models.py:2173`, `app/chamados/models.py:2177`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:225`). |
| Ch: ciclo e urgência | Núcleo limpa instante/via e restaura urgência; fotografia contempla esses campos somente nos credores derivados das P do dia (`app/chamados/models.py:248`, `app/chamados/models.py:253`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:230`). |

**Decisão de sítio para 1+4:** a sessão deve escolher entre preservar a cascata e antecipar sua carga completa, ou recortar formalmente a cascata acionada pela porta E4B; a alteração cabe nos pontos `backfill:_travar_lote`, snapshot e cerca, ou nos chamadores da cascata (`app/chamados/management/commands/backfill_fio_morto_e4b.py:179`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:211`, `app/chamados/services/disputa_emissao.py:1820`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:970`, `app/ponto/services/cartorio.py:1633`). Há `cartorio.calado()` existente, mas sua finalidade declarada é competência **trancada**, e ele faz o receiver pular o julgamento; reaproveitá-lo para a competência aberta exige decisão sobre os efeitos que deixarão de ocorrer (`app/ponto/services/cartorio.py:90`, `app/ponto/services/cartorio.py:92`, `app/chamados/signals.py:210`).

**2: corrida de exportação — nenhum protocolo existente na cópia fecha a janela.** K6 faz leitura simples dos registros; `registrar()` faz `create()` sem passar pelo protocolo C/D/Ch/P (`app/folha/export.py:581`, `app/folha/models.py:178`, `app/folha/models.py:182`). Repetir K6 imediatamente antes de `reabrir()` conserva o intervalo entre leitura e escrita (`app/chamados/services/disputa_emissao.py:1826`, `app/chamados/services/disputa_emissao.py:1840`). Trancar apenas registros ED existentes não cobre a primeira exportação de uma competência sem linha; prova concorrente disso na cópia **[nao verificado]**.

Os dois chamadores de produção encontrados chamam diretamente `registrar()`; mudar somente o request não cobre o comando de regeração (`app/folha/views.py:297`, `app/folha/management/commands/regerar_txt_dominio.py:118`). `folha/porta_export.py` é conferência de carimbo/medição, não a porta comum do registro (`app/folha/porta_export.py:7`, `app/folha/porta_export.py:14`).

Menor **cura existente** fora de `models.py`: **não identificada [nao verificado]**. Uma trava física nova da tabela ED no topo do atomic do backfill poderia ser candidata fora de models; sua compatibilidade, abrangência e segurança concorrente não estão demonstradas pela cópia **[nao verificado]**. Logo o brief deve escrever: **“decisão do opus/hunk da sessão: protocolo compartilhado com `ExportacaoDominio.registrar`, sítio `app/folha/models.py:174`, antes do `create` em `app/folha/models.py:182`; ou autorizar explicitamente protocolo alternativo fora de models, com prova concorrente e ordem global. Não considerar reconferência K6 como cura.”** A necessidade decorre dos sítios de leitura/escrita atuais (`app/folha/export.py:581`, `app/folha/models.py:182`).

**REDs concretos para o brief:**

- Outro vivo Ch12, P90 ressuscitável fora do dia e D90 fechada: provar inclusão de P90/D90 antes de qualquer P, cerca de competência/limbo e reversão de todos os campos; a composição capaz de escrever existe nos sítios acima (`app/ponto/services/cartorio.py:1633`, `app/chamados/models.py:1562`, `app/chamados/models.py:1574`).
- Adiados pela lâmpada com P absolvida de outro dia: provar que o alcance E2 não escapa da antecipação/cerca/snapshot (`app/ponto/services/cartorio.py:970`, `app/chamados/reconciliador.py:627`, `app/chamados/reconciliador.py:595`).
- Primeira exportação entre última consulta K6 e `reabrir`: duas conexões e barreiras, exigir serialização pelo protocolo decidido; execução real **[nao verificado]** (`app/folha/models.py:182`, `app/chamados/services/disputa_emissao.py:1826`, `app/chamados/services/disputa_emissao.py:1840`).
- Snapshot sob trava, round trip tipado, preservando lavra, assinatura, disputa fechada reaberta e urgência, com ato compensatório para `DecisaoPergunta`, conforme contrato já declarado (`app/chamados/management/commands/backfill_fio_morto_e4b.py:31`, `app/chamados/veredito_lavrado.py:270`, `app/chamados/models.py:1566`, `app/chamados/models.py:2177`).

MAPA exato da fila: `FIO-E0-V4-E4B`, fila 1, backfill classes a–e por porta canônica, DRY default, competência 11 primeiro e exportadas em lista para `!` (`app/docs/BACKLOG.md:1875`).

## 11/11 01:54 · sessao b789c3943605 · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**NAO LIBERA.** Ainda há caminho que troca o destino de uma pergunta de intervalo para o chamado genérico. A c21 corrige pedidos do gerador e da declaração, mas não estabelece a regra nos escritores; também acrescenta um veto ao autofechamento cuja autorização literal não está demonstrada. (`app/chamados/services/disputa_emissao.py:542`, `:824`, `:1044`, `:1296`, `:1562`; `app/chamados/models.py:2982`.)

Referências abreviadas: serviços em `app/chamados/services/`; `models.py` em `app/chamados/`; testes de disputa em `app/chamados/tests/`; auditor em `app/chamados/management/commands/auditar_invariantes_chamados.py`.

**1. Estado dos achados da preverif5.r e das duas falhas**

| Achado | Estado em 9bba8132b e fundamento |
|---|---|
| Exceção do gerador deixar nascimento incompleto | **Curado, mantido:** nascimento e geração estão no atomic; exceção propagada desfaz o conjunto. `disputa_emissao.py:679`, `:709`, `:730`. |
| Geração vazia confirmar nascimento no núcleo | **Curado, mantido:** ausência de motivo local admitido desfaz o nascimento. Não elimina o h2 autorizado do agrupador. `disputa_emissao.py:737`, `:739`, `:1565`. |
| Canônica estrangeira deixar nova DS sem pergunta | **Curado quanto ao nascimento; parcial quanto à costura/replay:** rollback local permanece; destino e origem ainda têm os resíduos abaixo. `disputa_emissao.py:737`, `:766`, `:1524`; `models.py:2866`, `:2982`. |
| Declaração pública aceitar ausência de pergunta | **Curado, mantido:** porta pública exige a pergunta; ensaio sem emissão permanece separado. `disputa_emissao.py:779`, `:803`, `:807`. |
| Seed estrangeiro indevidamente admitido na local | **Curado para admissão nova:** a guarda continua específica ao reuso estrangeiro de `orfao_14h`; não limpa acervo anterior. `disputa_emissao.py:1352`, `:1506`, `:1508`. |
| Segundo agregador na âncora | **Curado, mantido:** agregação permanece na porta comum. `disputa_emissao.py:602`, `:1652`. |
| Luz HX sem leitor | **Parcial:** I7a/I7b medem disputas abertas sem nenhuma P; não leem o WARNING. A extensão técnica da c21 não ganhou contador próprio. `auditar_invariantes_chamados.py:62`, `:69`, `:73`; `declaracao_texto.py:63`; `disputa_emissao.py:1562`. |
| Dona fechada: trilha e alarme | **Parcial, igual:** encaminha ao registrador, mas gravação pós-commit pode falhar; I23 isolado não determina exit 2. `disputa_emissao.py:608`; `descarte_disputa.py:19`, `:37`; `auditar_invariantes_chamados.py:1942`. | [nao verificado]
| Replay rederivar episódio conservado como `None` | **Igual, residual:** o gerador conserva o argumento, mas a porta usa `valor or derivador(dona)`. `disputa_emissao.py:545`, `:766`; `models.py:2866`. |
| Analisar chamado após replay divergente | **Curado nesse ramo:** análise exige destino confirmado e não recusado. `disputa_emissao.py:1535`, `:1545`, `:1547`. |
| Mutantes antigos morrerem na montagem | **Curado:** montagem precede o runner e errors não contam como RED. `test_bo_disputa_volta5_memoria.py:389`, `:415`. |
| “Paridade histórica” comparar fonte atual consigo mesma | **Curada a alegação:** o teste declara repetibilidade, não equivalência histórica. Histórico independente: **[nao verificado]**. `test_bo_disputa_volta3_memoria.py:252`, `:254`. | [nao verificado]
| Pendentes fora da fronteira do núcleo | **Igual:** wrapper chama pendentes após o retorno; helper abre P e depois agrega DS. `disputa_emissao.py:893`, `:340`, `:351`, `:354`. |
| Push dos novos motivos estrangeiros no núcleo misto | **Igual:** retorno dos novos da dona estrangeira não é coletado; wrapper usa `novos` da local. `disputa_emissao.py:664`, `:706`, `:894`. | [nao verificado]
| Push antes da confirmação externa | **Igual:** complementos e principal notificam dentro do atomic externo. `disputa_emissao.py:1285`, `:1307`, `:1574`. | [nao verificado]
| Retornos `(p, criada)` integralmente consumidos | **Igual literalmente:** primeira abertura da declaração descarta o booleano, embora confira a dona; replay confere criação e identidade. `disputa_emissao.py:834`, `:835`, `:842`, `:843`. |
| Doubles não certificarem ORM/signals/concorrência | **Igual:** limite explicitamente declarado. `test_bo_disputa_volta5m_memoria.py:6`, `:7`. |
| Fechamento vazio por recusa técnica | **Parcial:** falta de data/base, fotografia vazia e falha do complemento passaram a vetar o autofecho; outros `continue` ainda antecedem a consulta à ata. `disputa_emissao.py:1103`, `:1397`, `:1453`, `:1482`, `:1485`, `:1447`, `:1488`. |
| Derivação duplicada índice/B | **Curado nesse par:** ambos consomem a mesma fotografia. A eleição do chamado continua divergente entre lote e avulso. `disputa_emissao.py:1296`, `:1298`, `:1398`, `:291`, `:1299`. |
| Declaração/pendentes recosturarem no genérico | **Parcial:** entradas novas normalizam o pedido, mas há agrupador sem intervalo no lote e outras entradas diretas nos escritores. `disputa_emissao.py:542`, `:824`, `:1044`; `reconciliador.py:1655`. |
| Falha suite16: catraca das linhas congeladas | **Curada na verificação local:** 35 testes passaram; recolocar os números antigos produz uma failure, zero errors. `app/core/tests/test_medida_turno_do_par.py:268`, `:269`, `:540`, `:541`. |
| Falha suite16: `test_RED_F1_lote_misto_local_vazia_nao_ganha_seed_estrangeiro` | **Parcial:** o retorno simulado ao gerador passou com a correção; a assertion ORM continua exigindo destino `intervalo.pk`. Execução real nesta árvore: **[nao verificado]**. `test_bo_disputa_volta5m_memoria.py:82`, `:98`; `test_bo_disputa_volta5b_orfaos.py:157`. | [nao verificado]

Suíte inteira desta árvore, protegido ORM, concorrência, produção e efeitos externos: **[nao verificado]**. Os testes locais não certificam essas fronteiras. (`test_bo_disputa_volta5m_memoria.py:6`, `:7`.) [nao verificado]

**2. Censo de escritores e contraexemplos**

Os escritores persistentes encontrados continuam sendo estes; a própria porta registra os dois arquivos autorizados. (`fio.py:807`; `app/core/tests/test_contract_escritores_estado.py:61`.)

| Escritor | Escrita |
|---|---|
| Nascimento de P | Construção e inserção: `models.py:3014`, `:3017`. |
| Reuso da canônica | Atribuição e save: `models.py:2982`, `:3006`. |
| `reapontar_resolvedora` | Atribuição e save: `fio.py:925`, `:927`. |
| Banco, ao apagar chamado | FK `SET_NULL`; não escolhe outro chamado. `models.py:3385`, `:3387`. |

As atribuições de `fio.py:779`/`:783` são temporárias e restauradas; `models.py:366`/`:367` reassocia a instância do mesmo PK. (`fio.py:778`, `:783`; `models.py:366`.)

O alcance dos pedidos é:

| Caminho | Situação |
|---|---|
| Agrupador, complementos e B | Usa apenas o índice dos intervalos **do lote**; replay conserva esse pedido. `disputa_emissao.py:1044`, `:1296`, `:1534`. |
| Gerador, inclusive pendentes | Normaliza antes de `abrir`. `disputa_emissao.py:340`, `:344`, `:542`, `:546`. |
| Declaração de colaborador/admin | Ambas chegam ao helper normalizado; reaponte e replay usam os argumentos resultantes. `declaracao_texto.py:365`, `:653`; `disputa_emissao.py:824`, `:829`, `:842`. |
| Retorno do julgamento da célula | `analisar` → signal → cartório → fábrica → gerador alcança a normalização. Signals reais executados: **[nao verificado]**. `models.py:800`, `:806`; `app/chamados/signals.py:208`, `:216`; `app/ponto/services/cartorio.py:1008`; `fabrica_da_celula.py:172`, `:174`, `:219`. |
| Outros pedidos a reapontar | Permanecem diretos: reconciliador, gate do chamado, LIMBO e migração de fio. `app/chamados/reconciliador.py:954`, `:1642`, `:1655`; `models.py:437`; `limbo_folgas.py:244`, `:282`; `fio.py:2174`. |

**Sim, permanece recostura para o genérico.** Contraexemplo: existe P `intervalo_saida` na dona estrangeira, destinada ao intervalo; o agrupador recebe somente `[ch]`. O índice fica sem aquela chave, o fallback mantém `ch`, e o reuso pode gravá-lo na P existente. Os juízes conferem colaborador/turno/fio, sem eleger o chamado do marco explícito. (`disputa_emissao.py:1296`, `:1044`, `:1055`; `models.py:2972`, `:2980`, `:2982`; `app/chamados/juizes.py:2185`, `:2208`, `:2217`.)

A reprodução em memória conservou a dona e trocou destino **intervalo→ch**, tanto com dona aberta quanto fechada. A porta, ORM e signals desse ensaio são doubles; execução ORM do contraexemplo: **[nao verificado]**. (`test_bo_disputa_volta5m_memoria.py:6`; `test_bo_disputa_volta5l_memoria.py:64`; caminho produtivo: `disputa_emissao.py:1044`, `:1055`, `:1296`.)

| Contraexemplo pedido | Resultado e limite |
|---|---|
| Só `ch`, sem intervalo externo | Passa no ensaio: complemento fica em `ch`. Não cobre o intervalo existente fora do lote. `test_bo_disputa_volta5l_memoria.py:111`; `disputa_emissao.py:1296`. |
| Só intervalo | Passa no ensaio de B. `test_bo_disputa_volta5l_memoria.py:118`. |
| `[ch, intervalo]` e `[intervalo, ch]` | Passam nos ensaios com um único intervalo explícito. `test_bo_disputa_volta5l_memoria.py:81`, `:87`, `:102`. |
| Dona fechada e replay 2x | Passam nos cenários existentes; não impedem o contraexemplo `[ch]` acima. `test_bo_disputa_volta5l_memoria.py:83`, `:88`, `:104`, `:109`; `models.py:2982`. |
| Avulso sem chave explícita | Helper conserva o destino recebido. Se a canônica era do intervalo, nada nessa seleção impede pedir o genérico. Reproduzido em memória; ORM: **[nao verificado]**. `disputa_emissao.py:289`, `:292`, `:294`, `:542`; `models.py:2982`. |

Há ainda duas eleições para a mesma chave: o agrupador escolhe **primeiro do lote**, enquanto o avulso escolhe **menor PK**. Com dois intervalos para o mesmo motivo/dia, o ensaio observou mudança de destino entre eles. (`disputa_emissao.py:1299`, `:291`, `:292`; limite do ensaio: `test_bo_disputa_volta5m_memoria.py:6`.)

**3. `_marcos_de_intervalo`: unicidade, regularizabilidade e exceções**

**Uma redação da derivação para índice/B: sim. Uma garantia universal de concordância semântica: não.** A fotografia é produzida em `:1296` e consumida por ambos; avulsos executam novamente o mesmo helper sobre outro universo. (`disputa_emissao.py:257`, `:292`, `:1296`, `:1298`, `:1398`.)

Para o mesmo `data` válido, o dia concorda com `dia_do_turno_da_celula`: ela retorna `DiaDoTurno(dia)`, e `DiaDoTurno` é `NewType`. Não há conversão civil→turno nessa chamada. (`disputa_emissao.py:273`; `app/ponto/turnos.py:2939`, `:2943`; `app/core/dia_do_turno.py:13`.)

Regularizável não significa “possui marco válido”: o predicado admite a fábrica; o helper exige categoria exata, descarta marcos não dict/datas inválidas e transforma qualquer tipo diferente de `S` em volta. Portanto, regularizável com fotografia vazia é possível pelo código. (`disputa_emissao.py:1003`, `:1009`, `:266`, `:270`, `:274`, `:276`.)

Também não é função total para qualquer JSON: contexto truthy não dict falha no `.get`; `marcos_faltantes` truthy não iterável falha no `for`. Esses pontos ficam fora do `try` e já ficavam fora da captura antiga. (`disputa_emissao.py:269`; comparação histórica pelo patch fornecido: **[nao verificado]** quanto ao Git.)

No antigo B, a captura genérica foi substituída por `TypeError/ValueError`. Uma outra subclasse de `Exception` lançada dentro da expressão protegida — por exemplo, `RuntimeError` de um `.get` extraordinário — agora propaga nesse sítio. O índice antigo já tinha captura estreita; isso não demonstra uma mudança global para JSON ordinário. Ocorrência concreta em produção: **[nao verificado]**. (`disputa_emissao.py:272`, `:274`; histórico da captura conforme patch fornecido: **[nao verificado]** quanto ao Git.)

O construtor de dia também entrou no `try`: seus eventuais T/V passam a ser capturados, embora a implementação atual seja identidade. (`disputa_emissao.py:273`; `app/ponto/turnos.py:2943`.)

Finalmente, o tipo não certifica a procedência do dia: o detector calcula `marcos[].data` pela data civil do instante encadeado, potencialmente D+1. Sua emissão está aposentada; não é escritor persistente atual desses chamados. (`app/ponto/management/commands/detectar_intervalo_ausente.py:77`, `:82`, `:149`, `:156`.)

**4. Qual luz, quem escreve/lê e qual lei**

`acender_sem_ata` escreve **somente WARNING**, com a mensagem “sem_celula: ata nao lavrada”. Não incrementa contador nem persiste o flag técnico. (`declaracao_texto.py:63`, `:70`, `:71`.)

I7a/I7b não leem esse WARNING: procuram DS abertas com zero linhas P, reconstroem dias e consultam a ata atual. Sem dia vai para ESTRUTURA/I7a; somente dias todos `sem_celula` vão para I7b. (`auditar_invariantes_chamados.py:62`, `:64`, `:68`, `:69`, `:73`, `:74`.)

Os novos escritores são de memória: `ato['insumo_incompleto']` e `_insumo_incompleto`; seu leitor decisório é o veto em `:1562`. Não nasceu escritor persistente, mas nasceu **predicado adicional de autofechamento**, capaz de impedir a chamada a `fechar` mesmo depois de `pode_fechar()` responder sim. (`disputa_emissao.py:1097`, `:1103`, `:1314`, `:1397`, `:1453`, `:1482`, `:1485`, `:1561`, `:1562`, `:1567`.)

A autoridade registrada para “disputa pode fechar?” é `DisputaSupervisao.pode_fechar`. Portanto, a afirmação da entrega de que não há novo julgamento não está demonstrada apenas por continuar chamando esse método: existe outra condição decidindo o autofecho. (`app/docs/JUIZES.md:62`; `models.py:1848`, `:1853`; `disputa_emissao.py:1562`.)

DUAS-RESPOSTAS preserva HX sem ata; DUAS-LEIS preserva h2 com ata e nada a perguntar; LIBERA-B3 autoriza cura na origem sob escritor único. Autorização literal para estender o veto a sem-data/base/marco inválido: **[nao verificado]** nos avais fornecidos. Escopo literal e unicidade são exigidos por `CLAUDE.md:37`, `:175`.

Há dois limites adicionais:

- Sem base, o código pode emitir “ata não lavrada” sem consultar a ata. Essa mensagem não demonstra a causa anunciada. (`disputa_emissao.py:1484`, `:1485`, `:1564`; `declaracao_texto.py:71`.)
- Se `_classificar_falta` devolver motivo `None`, há saídas anteriores à consulta da ata. O ensaio com ata simulada `sem_celula` terminou fechado, sem consulta nem luz; ORM: **[nao verificado]**. (`disputa_emissao.py:1446`, `:1447`, `:1456`, `:1487`, `:1488`, `:1490`, `:1567`; doubles: `test_bo_disputa_volta5m_memoria.py:6`.)

**5. Catraca: mesmo sítio ou outra coisa**

**Os números novos apontam aos mesmos sítios de extração de data**, pelo cotejo da cópia com o delta fornecido:

| Registro atual | Conteúdo |
|---|---|
| `[928, 929]` | `de = _tz.localtime(t.entrada.timestamp).date()` e `ds = _tz.localtime(t.saida.timestamp).date()`. `disputa_emissao.py:928`, `:929`. |
| `[1445]` | `data_entrada = _tz.localtime(batida_origem.timestamp).date()`. `disputa_emissao.py:1445`. |

Não apontam ao helper novo. O censo B1 reconhece essas extrações `.date()`, e a assertion continua comparando o mapa completo de linhas efetivas. (`app/core/medida_turno_do_par.py:217`, `:225`; `app/core/tests/test_medida_turno_do_par.py:534`, `:540`, `:541`.)

A catraca passou nos 35 testes locais; restaurar os números antigos produziu uma failure, zero errors. Isso prova a mordida do registro atual, não correção semântica dos sítios congelados. (`app/core/tests/test_medida_turno_do_par.py:541`; sítios: `disputa_emissao.py:928`, `:929`, `:1445`.)

Identidade histórica independente pelo Git: **[nao verificado]**; a cópia não disponibilizou repositório Git.

**6. Classes que já reprovaram obra**

| Classe | Resultado |
|---|---|
| Segundo juiz | **Não certifico zero:** o novo veto técnico decide o autofecho além da resposta do juiz registrado. `disputa_emissao.py:1561`, `:1562`; `app/docs/JUIZES.md:62`. |
| Segundo escritor fora da porta | **Nenhum novo persistente encontrado**, mas a regra não governa todos os pedidos aos escritores existentes. `fio.py:807`; `models.py:2982`; `disputa_emissao.py:1044`. |
| Relógio fora da borda | **Nenhuma leitura nova de relógio identificada na c21**; extrações civis congeladas e `now()` legados permanecem. `disputa_emissao.py:273`, `:595`, `:928`, `:929`, `:1371`, `:1445`. |
| Derivação paralela de fonte única | **Curada para `(motivo,dia)` no índice/B; presente na eleição do destino:** primeiro do lote versus menor PK. `disputa_emissao.py:1296`, `:1398`, `:1299`, `:291`. |
| Selo/RED sem mordida | **Curado nos mutantes executados:** failures por assertion, zero errors; montagem não conta como RED. `test_bo_disputa_volta5m_memoria.py:68`, `:214`, `:218`; `test_bo_disputa_volta5l_memoria.py:157`, `:162`. |
| Campo com dois significados | `data_evento` distingue conversa/fato pela porta; `DS.motivos` é cenário/seed, não identidade de P; `chamado_destino` continua podendo oscilar entre genérico e explícito. `models.py:1743`, `:2427`, `:2794`; `disputa_emissao.py:1508`, `:1044`; `models.py:2982`. |

Resultados locais observados: controle c21 **6 testes OK**; sete mutantes c21 com **3, 4, 1, 2, 1, 4, 1 failures**, zero errors; quatro c20 com **5, 5, 1, 1 failures**, zero errors. Os runners exigem controle verde e RED por assertion. (`test_bo_disputa_volta5m_memoria.py:28`, `:207`, `:214`, `:218`; `test_bo_disputa_volta5l_memoria.py:154`, `:162`.) [nao verificado]

Os mutantes antigos `nasce_sem_pergunta` e `replay_insere` também produziram uma failure cada, zero errors. Isso não certifica ORM/signals/cartório. (`test_bo_disputa_volta5_memoria.py:393`, `:394`, `:415`; `test_bo_disputa_volta5m_memoria.py:6`.)

**7. Quem escreve e quem lê os campos envolvidos**

| Campo/insumo | Quem escreve | Quem lê nesta cura e efeito relevante |
|---|---|---|
| `Ch.categoria`, `colaborador`, PK | Nascimento pela porta/inserção. `models.py:961`, `:971`. | Recorte exato, identidade da fotografia e desempate. `disputa_emissao.py:266`, `:289`, `:291`, `:1296`. |
| `Ch.status_local` | `_gravar_estado`; inserção inicial pela porta. `models.py:201`, `:228`, `:971`. | Avulso usa `VIVOS`; autoridade dos estados permanece no catálogo. `disputa_emissao.py:14`, `:290`; `app/docs/JUIZES.md:59`. |
| `Ch.contexto_json.marcos_faltantes[].tipo/data` | Portas de contexto/nascimento. Detector calcula formato com data, mas emissão está aposentada. Grade atual também escreve `marcos_faltantes`, em categoria `Ausencia` e sem `data` por marco. `contexto.py:76`, `:102`; `models.py:971`; `app/ponto/management/commands/detectar_intervalo_ausente.py:80`, `:149`; `app/ponto/management/commands/reconciliar_grade.py:498`, `:502`. | Helper deriva fatos; índice, B e avulso consomem. `disputa_emissao.py:269`, `:273`, `:276`, `:292`, `:1298`, `:1398`. |
| `P.chamado_destino` | Nascimento/reuso, reapontar ou FK `SET_NULL`. `models.py:3014`, `:3017`, `:2982`, `:3006`, `:3387`; `fio.py:925`, `:927`. | Juiz, filtro/complemento e materialização. `app/chamados/juizes.py:2185`, `:2208`; `disputa_emissao.py:1320`, `:1341`; `materializacao.py:1076`, `:1079`. |
| `P.motivo` | Pedido derivado pelo helper; persistência no nascimento. `disputa_emissao.py:276`; `models.py:3014`, `:3017`. | Chave de destino, identidade e materialização. `disputa_emissao.py:292`, `:1044`; `models.py:2427`; `materializacao.py:1077`. |
| `P.data_evento` | Atribuição exclusivamente por `gravar_dia_do_fato`; nascimento passa por ela. `fio.py:790`, `:800`; `models.py:3016`. | Chave motivo/dia, dedup, juiz e plantio. `disputa_emissao.py:292`, `:1044`, `:1698`; `fio.py:1608`; `materializacao.py:1345`. |
| `P.disputa` | Inserção; reuso conserva a dona. `models.py:3014`, `:3008`. | Encaminhamento estrangeiro, rollback e replay. `disputa_emissao.py:654`, `:835`, `:1065`, `:1524`. |
| `P.batida_origem_id` | Derivação no nascimento e preparação da identidade. `models.py:2866`, `:2868`, `:3449`. | Argumento do replay e juiz do dia/fato. `disputa_emissao.py:545`, `:766`; `models.py:2875`; `fio.py:1633`. |
| `P.celula` | Nascimento e ajuste no reuso. `models.py:2896`, `:2966`, `:3014`. | Costura/lavra e despertar das células afetadas. `models.py:2972`, `:3004`, `:3007`; `fio.py:933`. |
| `P.pergunta`, `tipo_resposta`, `opcoes` | Catálogo/gerador e payload da declaração; inserção pela porta. `disputa_emissao.py:484`, `:502`, `:543`; `decl

[truncado em 20000 de 23684 caracteres]

## 11/11 01:50 · sessao 168b267eaf81 · - · - · origem=Ronald (tmux)

Censo somente leitura concluído. Nenhum arquivo alterado.

**C6: chamadas produtivas diretas e por alias identificadas por AST**

| Arquivo | Linhas das chamadas |
|---|---|
| `chamados/relatorios_disputa.py` | 18 |
| `chamados/views.py` | 1242, 1257, 1417 |
| `chamados/juizes.py` | 1417, 1422, 1434, 1504, 1605, 2267 |
| `chamados/models.py` | 368, 1509, 1896, 3044, 3139, 3375 |
| `chamados/reconciliador.py` | 398, 661, 758, 1086, 1594 |
| `api/views.py` | 2620 |
| `chamados/management/commands/reabrir_orfas_enterradas.py` | 83 |
| `chamados/management/commands/auditar_invariantes_chamados.py` | 202, 540, 858, 894, 1653, 1710, 1769, 1782 |
| `chamados/management/commands/migrar_data_evento_dia_do_turno.py` | 449 |
| `chamados/management/commands/disparar_perguntas_competencia.py` | 109 |
| `chamados/services/relavra_via.py` | 242 |
| `chamados/services/fio.py` | 238, 606, 781, 960, 2128, 2133 |
| `chamados/services/competencia_trancada.py` | 122, 132, 142 |
| `chamados/services/disputa_emissao.py` | 1843, 1925 |
| `chamados/services/acoes_chamado.py` | 348 |
| `chamados/services/pergunta_no_app.py` | 58, 136 |
| `chamados/services/vigia_fio.py` | 755, 813 |
| `ponto/services/dia_do_colab.py` | 71 |
| `colaboradores/services/calendario.py` | 476 |

São 52 chamadas em 19 arquivos pelo critério acima. `chamados/models.py:4083` reexporta a função; as propriedades `estado` e `viva` delegam em `chamados/models.py:3044` e `chamados/models.py:3375`. O template lê essas propriedades em `templates/chamados/partials/_card_atendimento.html:77` e `templates/chamados/partials/_card_atendimento.html:183`. Chamadas dinâmicas sem identificador literal: [nao verificado].

**Leitores reais do dict da tarefa**

O único consumidor produtivo externo identificado é `chamados/management/commands/diff_e3_sombra.py:125`. Ele lê:

- `rota`, `fila`, `verbos`, `fio`: `chamados/management/commands/diff_e3_sombra.py:44`.
- `razao`, para classificar isento com fila residual: `chamados/management/commands/diff_e3_sombra.py:68`.

A candidata retorna `None` quando `rota=None`, portanto não mede a mudança `fio='admin', porta=None`. Esse comportamento é explicitamente exigido pelo teste atual em `chamados/tests/test_e3_diff_sombra.py:136`. A sombra necessária para esta fase deve comparar o dict inteiro, conservando `admin` sem porta. (`chamados/management/commands/diff_e3_sombra.py:44`)

`painel.py` fornece a fila; `declaracao_texto.py` fornece a posição; `juizes.py::alimentar_fila` fornece os verbos e IDs. Não consomem o dict. (`chamados/services/painel.py:186`, `chamados/services/declaracao_texto.py:78`, `chamados/juizes.py:1452`)

A agregação interna lê `instancias[].acoes`, `acao.porta`, `acao.de`, `celula_id`, `nao_sei`; publica `rota/fila/verbos/fio`. (`chamados/services/tarefa_do_dia.py:425`)

**Limites de vocabulário e exportação**

Viva sem resposta produz `responder/colaborador` pela E1, e nenhum verbo existente representa mera exposição administrativa. (`chamados/services/tarefa_do_dia.py:129`, `chamados/services/tarefa_do_dia.py:49`)

FIO-LE-O-JUIZ manda mostrar viva de disputa fechada ao ADMIN sem reabrir/push, mas não autoriza validar sem resposta. A própria porta tem teste que exige recusa sem log nesse caso. (`chamados/services/fio.py:212`, `chamados/tests/test_fio_le_o_juiz.py:235`) O modal declara que Validar só aparece depois da resposta. (`templates/chamados/partials/modal_fio.html:440`)

A base deliberadamente conserva ação exigível na exportada. (`chamados/tests/test_e2_tarefa_do_dia.py:29`) A fronteira `dia_em_competencia_exportada` foi removida por lei escrita no código. (`chamados/juizes.py:1982`) Há testes exigindo mostrar Validar e efetivamente validar em disputa fechada exportada, sem reabrir/push. (`chamados/tests/test_fio_le_o_juiz.py:277`, `chamados/tests/test_fio_le_o_juiz.py:291`)

Assim, “consulta somente leitura, sem executar escritor” cabe na base; “suprimir qualquer verbo potencialmente escritor na exportada” modifica o comportamento exigido por esses testes. O pedido atual prevalece, mas essa diferença precisa constar da entrega. [nao verificado]

**Testes diretamente pertinentes encontrados** [nao verificado]

- C6/precedência/memo: `chamados/tests/test_pergunta_viva.py:15`, `chamados/tests/test_selo_memo_pergunta_viva.py:86`, `chamados/tests/test_contract_pergunta_viva.py:25`, `chamados/tests/test_pergunta_respondida_e_marco_aceso.py`.
- Fio, fechamento e portas: `chamados/tests/test_fio_le_o_juiz.py:32`, `chamados/tests/test_o261e_f1_porta_encerramento.py:75`, `chamados/tests/test_o261e_f2_porta_carimbo.py:665`, `chamados/tests/test_pvmuda_carimbo_em_voo.py:73`.
- App/dia: `chamados/tests/test_pergunta_no_app.py:145`, `chamados/tests/test_n1_perguntas_do_app.py`, `api/tests/test_dia_do_colab.py`.
- Tranca: `chamados/tests/test_competencia_trancada.py:123`, `chamados/tests/test_tranca_de_verdade.py:52`, `chamados/tests/test_snc2_toque_em_lote.py:85`.
- Calendário: `colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:52`.
- Reconciliador/relavra: `chamados/tests/test_reconciliador_c1_escopo.py:48`, `chamados/tests/test_reabrir_orfas_enterradas.py:101`, `chamados/tests/test_orfas_pelo_evento.py:102`, `ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:84`.
- Tarefa/sombra: `chamados/tests/test_e2_tarefa_do_dia.py:137`, `chamados/tests/test_e3_portas_tarefa.py:36`, `chamados/tests/test_e3_diff_sombra.py:131`, `chamados/tests/e3_mutantes.py:76`, `chamados/tests/e4_verificar.py:50`.

Não executei testes neste subtrabalho; cobertura completa de todos os consumidores: [nao verificado]. [nao verificado]

O RED c8 exige cuidado: chama `_tarefa_pura` sem insumo de vida, e seu helper não fornece `pergunta_viva`. Será necessário ampliar somente a infraestrutura do helper para alimentar o novo argumento, preservando os testes/asserts. Usar `p.viva_c6` como autoridade produtiva fabricaria um predicado fora do juiz. (`chamados/tests/test_e3_tarefa_admin_sem_banco.py:74`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:77`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:123`)

## 11/11 01:50 · sessao 1a7b48f4d91d · - · - · origem=Ronald (tmux)

**Derivação e exceções**

- A duplicação índice/B foi removida: `_marcos_de_intervalo` deriva `(motivo,dia)` em `disputa_emissao.py:257-278`; a fotografia nasce em `:1296`, índice lê `:1298` e B lê `:1398`. Avulsos chamam o mesmo derivador novamente (`:292`).
- Para o mesmo dado válido, o dia concorda: `dia_do_turno_da_celula` devolve `DiaDoTurno(dia)` sem conversão (`app/ponto/turnos.py:2939-2943`); `DiaDoTurno` é `NewType` (`app/core/dia_do_turno.py:13`).
- Regularizável não garante fotografia não vazia: o predicado permite a fábrica (`disputa_emissao.py:1003-1009`), enquanto o helper exige categoria exata e descarta marcos/data inválidos (`:266-276`). Tipo desconhecido vira `intervalo_volta` (`:276`).
- Não é total para todo JSON: contexto truthy não dicionário causa `AttributeError`; `marcos_faltantes` truthy não iterável causa `TypeError` fora da captura (`:269`). Esses pontos também ficavam fora do `try` antigo, portanto não são efeito novo da captura.
- A antiga captura genérica de B para `.get/fromisoformat` virou `(TypeError,ValueError)` (`:273-275`): outras subclasses de `Exception`, como `RuntimeError`, passam a propagar nesse sítio. Para dict/list/string JSON ordinários, não identifiquei exceção adicional normal; comportamento extraordinário concreto em produção: **[nao verificado]**. O índice antigo já capturava somente T/V, conforme patch fornecido.
- O construtor de dia passou para dentro do `try` (`:273`): T/V dele também seriam agora engolidos, embora sua implementação atual seja identidade (`app/ponto/turnos.py:2943`).

**Fonte do dia**

- O detector calcula `marcos[].data` pela data civil do instante encadeado (`app/ponto/management/commands/detectar_intervalo_ausente.py:77-84`), potencialmente D+1 num turno de célula D; o NewType não certifica semanticamente esse insumo (`app/ponto/turnos.py:2939-2943`).
- Entretanto, a emissão desse detector está aposentada: só reconcilia legado, com `continue` antes de nascimento (`detectar_intervalo_ausente.py:122-139`, `:149-157`). Não deve ser apresentado como escritor persistente atual.
- Escritores atuais de `marcos_faltantes` incluem nascimento na grade (`app/ponto/management/commands/reconciliar_grade.py:491-502`), relavra pela porta (`:431`, `:457`) e detectar ausências (`app/ponto/management/commands/detectar_ausencias.py:497`, `:558-559`, `:675-711`). Esses formatos normalmente levam tipo/hora/motivo, sem data; o nascimento da grade usa categoria `Ausencia`, fora do helper de intervalo (`reconciliar_grade.py:498-502`; `disputa_emissao.py:266`).

**Luz e autoridade**

- `acender_sem_ata` escreve somente WARNING, com a mensagem literal “sem_celula: ata nao lavrada” (`app/chamados/services/declaracao_texto.py:63-72`). Os contadores não leem esse log nem o flag.
- I7a/I7b leem DS abertas sem nenhuma linha P, reconstroem os dias e consultam a ata atual (`app/chamados/management/commands/auditar_invariantes_chamados.py:62-74`). Sem dia é ESTRUTURA/I7a (`:64-73`); o relatório publica INFO para I7b e VIOL para I7a positivo (`:89-95`, `:1925-1927`). Falha dessa leitura não tem captura local (`:69-71`).
- Os novos escritores de memória são `ato['insumo_incompleto']` (`disputa_emissao.py:1097`, `:1103`) e `_insumo_incompleto` (`:1314`, `:1397`, `:1453`, `:1482`, `:1485`); o único leitor decisório é `:1562`.
- Não nasceu escritor persistente; nasceu **predicado adicional de auto-fechamento**, que veta chamar `fechar` mesmo quando `pode_fechar()` responde sim (`:1561-1567`). A autoridade “disputa pode fechar?” continua registrada em `app/docs/JUIZES.md:62` e definida em `app/chamados/models.py:1848-1853`. Os anexos autorizam HX sem ata; autorização literal para estendê-la a sem-base/data/marco inválido: **[nao verificado]**.
- O novo WARNING pode afirmar ausência de ata sem tê-la consultado, por exemplo no ramo sem base (`disputa_emissao.py:1484-1485`, `:1562-1564`). I7a/I7b classificam depois por outra leitura, portanto o aviso não determina o contador (`auditar_invariantes_chamados.py:68-74`).

**Resíduos concretos**

- Marco válido + inválido não marca incompletude: o helper descarta o inválido, e B só marca quando a fotografia inteira está vazia (`disputa_emissao.py:274-275`, `:1396-1397`). “Todo marco inválido conserva a disputa” é mais forte que o implementado.
- Continua fechamento vazio sem testemunho de ata quando `_classificar_falta` devolve motivo `None`, tanto por origem quanto sem origem (`:1446-1448`, `:1487-1489`); a leitura da ata vem depois (`:1456`, `:1490`).
- Reproduzi esse segundo ramo com AST produtiva/Ambiente5m: base=True, classificar=(None,None,None), estado de ata sem_celula → DS fechada, zero P, zero luz, zero consultas ao juiz da ata. Estrutura dos doubles: `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:6-7`, `:74-78`; caminho produtivo: `disputa_emissao.py:1487-1489`, `:1565-1567`. ORM: **[nao verificado]**.
- Reproduzi também ata vazia disponível + sem_base → DS aberta, zero P, luz=1, zero consultas à ata. Demonstra que o novo veto pode conservar disputa num universo em que o aval h2 manda conservar o fechamento vazio; código do veto: `disputa_emissao.py:1484-1485`, `:1562-1564`; enquadramento ORM concreto: **[nao verificado]**.

## 11/11 01:49 · sessao ce9dd6eece4b · - · - · origem=Ronald (tmux)

Achado principal: **a cura continua parcial na origem**. O agrupador só indexa intervalos do lote; quando recebe apenas `ch`, pode recosturar para `ch` uma pergunta já destinada a intervalo fora do lote. A normalização nova existe no gerador/declarador, mas não governa os escritores persistentes. (`app/chamados/services/disputa_emissao.py:1296`, `:1299`, `:1044`, `:542`, `:824`; `app/chamados/models.py:2982`, `:3006`.)

Reproduzi em memória, com `Ambiente5m`, estes casos:

- DS local vazia, dona estrangeira com P `intervalo_saida` destinada a intervalo=1, genérico `ch`=2; agrupador recebe `[ch]`: P permanece na dona e troca destino **1→2**, tanto dona aberta quanto fechada. A porta/ORM/signals são doubles; execução ORM **[nao verificado]**. (`app/chamados/tests/test_bo_disputa_volta5m_memoria.py:6`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:101`, `:104`; causa produtiva: `app/chamados/services/disputa_emissao.py:1044`, `:1055`, `:1296`.)
- Avulso do gerador sem chave explícita: intervalo com `marcos_faltantes=[]` ou fora de `VIVOS`; helper devolve genérico e canônica troca intervalo→genérico. Demonstração em memória; ORM **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:269`, `:289`, `:290`, `:294`, `:542`, `:546`; `app/chamados/models.py:2982`.)
- Dois intervalos explícitos i1=1/i2=2 para mesmo motivo/dia: lote `[i2,i1,ch]` elege i2; gerador posterior elege i1. Observei **2→1** em memória. Portanto há arbitragem paralela de **quem recebe**: primeiro do lote versus menor PK. (`app/chamados/services/disputa_emissao.py:1299`, `:291`, `:292`; limite: `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:6`.)

Censo persistente:

| Escritor | Prova |
|---|---|
| Inserção de P | `app/chamados/models.py:3014`, `:3017` |
| Reuso canônico | `app/chamados/models.py:2982`, `:3006` |
| `reapontar_resolvedora` | `app/chamados/services/fio.py:925`, `:927` |
| FK ao apagar chamado | `app/chamados/models.py:3385`, `:3387` |

As atribuições de `fio.py:779`/`:783` são temporárias; `models.py:367` reassocia instância do mesmo PK. O contrato enumera os dois arquivos escritores. (`app/chamados/services/fio.py:778`, `:783`, `:807`; `app/chamados/models.py:366`; `app/core/tests/test_contract_escritores_estado.py:61`.)

Chamadores diretos que continuam sem a seleção nova:

- Reconciliador: reemissão, histórico e derivação avulsa pedem `reapontar_resolvedora` diretamente. O derivador legado exclui candidato que já tenha P não materializada. (`app/chamados/reconciliador.py:954`, `:1642`, `:1655`; `app/chamados/services/disputa_emissao.py:226`, `:229`.)
- Gate de fechamento de chamado; reata/agrupamento LIMBO; migração de fio. (`app/chamados/models.py:433`, `:437`; `app/chamados/services/limbo_folgas.py:244`, `:282`; `app/chamados/services/fio.py:2174`.)

Os escritores conferem colaborador/turno e se a costura mata fio; não verificam preferência por chamado com marco explícito. Destino não terminal passa imediatamente pelo segundo gate. (`app/chamados/juizes.py:2179`, `:2185`, `:2208`, `:2217`; `app/chamados/services/fio.py:771`, `:772`, `:907`, `:925`; `app/chamados/models.py:2972`, `:2982`.)

Rota de retorno do julgamento: `analisar`→`_gravar_estado`→signal→cartório→fábrica→gerador alcança a normalização nova; há guarda contra julgamento recursivo. Não executei esses signals reais. (`app/chamados/models.py:800`, `:806`; `app/chamados/signals.py:208`, `:210`, `:216`; `app/ponto/services/cartorio.py:1008`; `app/chamados/services/fabrica_da_celula.py:172`, `:174`, `:219`; `app/chamados/services/disputa_emissao.py:542`, `:546`.)

Para os cenários já existentes — só ch sem intervalo externo, só intervalo, ambas ordens, replay 2x/dona fechada — os selos conferem o resultado em memória, não ORM. (`app/chamados/tests/test_bo_disputa_volta5l_memoria.py:81`, `:87`, `:109`, `:111`, `:118`; limite `:6`.)

**Bloqueio de desenho preservado:** seleção corrigida em chamadores específicos continua permitindo recostura divergente pela mesma porta; o contraexemplo `[ch]` com intervalo externo basta para negar cura universal. (`app/chamados/services/disputa_emissao.py:1044`, `:1296`; `app/chamados/models.py:2982`, `:3006`.)

## 11/11 01:49 · sessao 6cedf4aad9ed · - · - · origem=Ronald (tmux)

**Catraca: curada e preserva os mesmos sítios.** Os novos números apontam às extrações `de = _tz.localtime(t.entrada.timestamp).date()`, `ds = _tz.localtime(t.saida.timestamp).date()` e `data_entrada = _tz.localtime(batida_origem.timestamp).date()`; são deslocamentos das expressões existentes, não troca pelo helper `_marcos_de_intervalo`. (`app/chamados/services/disputa_emissao.py:928`, `:929`, `:1445`; `app/core/tests/test_medida_turno_do_par.py:268`, `:269`; anexo `diff_c21.patch` [nao verificado quanto ao histórico Git, ausente na cópia].)

O detector B1 reconhece `.date()` sem argumentos sobre operando marcado; `fromisoformat`/`dia_do_turno_da_celula` não substituem esses três sítios no censo. A assertion compara todas as linhas efetivas, mantendo identidade/critério na catraca separada. (`app/core/medida_turno_do_par.py:217`, `:225`; `app/core/tests/test_medida_turno_do_par.py:534`, `:540`.)

Executei somente em memória, com `PYTHONDONTWRITEBYTECODE=1 python3 -B`: [nao verificado]

| Execução | Resultado observado | Sítio |
|---|---|---|
| Catraca carregada trocando apenas `SimpleTestCase` por `unittest.TestCase` | **35 testes OK** | `app/core/tests/test_medida_turno_do_par.py:540` |
| Mutante do registro recolocando `[885,886]` e `[1393]` | **1 failure, zero errors** | `app/core/tests/test_medida_turno_do_par.py:541` |
| Controle c21 | **6 testes OK** | `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:197` |
| Sete mutantes c21 | **3, 4, 1, 2, 1, 4, 1 failures; zero errors** | `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:28`, `:214`, `:218` |
| Quatro mutantes c20 | **5, 5, 1, 1 failures; zero errors** | `app/chamados/tests/test_bo_disputa_volta5l_memoria.py:157`, `:162` |
| Antigos `nasce_sem_pergunta` / `replay_insere` | **1 failure cada, zero errors** | `app/chamados/tests/test_bo_disputa_volta5_memoria.py:112`, `:224`, `:415` |
| Regressão dos 16 scripts memória presentes | **16 processos rc=0** | Entrada ilustrativa `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:207`; contagem agregada sem arquivo persistido [nao verificado] |

**Os mutantes mordem por assertion.** C21 compila e monta antes do runner, exige exatamente um sítio mutado e rejeita resultado com errors ou sem failures; não transforma falha de montagem em RED. (`app/chamados/tests/test_bo_disputa_volta5m_memoria.py:68`, `:214`, `:215`, `:218`.)

**Segunda falha da suite16: parcial, não certificada como curada.** O protegido exige a P local destinada ao `intervalo.pk`, e isso continua intocado; os ensaios em memória passam pelo destino, incluindo retorno simulado do signal/cartório, ambas as ordens, dona fechada e replay. A execução real do protegido ORM nesta árvore continua **[nao verificado]**. (`app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:157`; `app/chamados/tests/test_bo_disputa_volta5m_memoria.py:82`, `:98`; `app/chamados/tests/test_bo_disputa_volta5l_memoria.py:81`, `:87`, `:109`.)

**Limite decisivo:** os próprios testes declaram ORM, signals/cartório, ata e fechamento como doubles; os resultados acima não tornam verde a suíte inteira nem certificam concorrência. (`app/chamados/tests/test_bo_disputa_volta5m_memoria.py:6`, `:7`.)

## 11/11 01:49 · sessao 035cb0e9da4c · - · - · origem=Ronald (tmux)

Não encontrei bloqueio de lei que obrigue mudar o predicado. A alimentação completa exige alcançar mais dependências que a ausência da testemunha. (`chamados/juizes.py:1393`, `chamados/juizes.py:1406`)

Plano viável:

- Acrescentar `carga=None` ao C6 e propagar aos auxiliares. Sem carga, conservar exatamente as chamadas atuais. Com carga, trocar somente as fontes dos insumos. (`chamados/juizes.py:839`, `chamados/juizes.py:1324`)
- Alimentar `classificar_falta` com célula, todas as ausências relevantes, vínculo e turnos das duas janelas que ela consulta: D..D e D−1..D+1. A última pertence a `_turno_noturno_completo`. (`escala/servico_jornada.py:293`, `escala/servico_jornada.py:334`, `escala/servico_jornada.py:344`, `escala/servico_jornada.py:241`, `escala/servico_jornada.py:383`)
- Para os turnos, pré-carregar batidas/células/vínculos e chamar `_turnos_do_colab_puro` na janela exata de cada chamada. Recortar turnos já pareados de uma janela larga não preserva necessariamente o resultado. (`ponto/turnos.py:1918`, `ponto/turnos.py:1938`, `ponto/turnos.py:1970`)
- O conjunto completo dos vínculos pode alimentar `vinculo_do_dia`: o próprio juiz verifica vigência e escolhe a geradora; não é preciso copiar o filtro do carregador. Preservar a ordem em possíveis empates. (`escala/alimentacao.py:267`, `escala/alimentacao.py:279`, `escala/alimentacao.py:282`)
- Alimentar também a materialização: slot, plantadas, batida na janela, trilha de retratação e turnos de completude. O slot tipo `hora` chega à derivação única do plantio, que precisa receber grade/vínculo/marcos carregados. (`chamados/juizes.py:738`, `chamados/juizes.py:740`, `chamados/juizes.py:802`, `chamados/juizes.py:282`, `chamados/juizes.py:363`, `chamados/services/materializacao.py:371`, `chamados/services/materializacao.py:386`, `chamados/services/materializacao.py:421`)

Apenas alimentar ausências resolve a leitura instrumentada da testemunha, mas deixa os demais caminhos capazes de consultar por pergunta. Não sustentaria o contrato geral de custo constante. (`escala/servico_jornada.py:334`, `escala/servico_jornada.py:344`, `chamados/juizes.py:740`)

Nenhum arquivo escrito nesta exploração. [nao verificado por diff próprio]

## 11/11 01:45 · sessao 3066302ca157 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura implementada. [Entrega C21](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/BO_DISPUTA_ENTREGA_C21.txt:6) contém censo, vereditos 3–6, REDs, mutantes e comandos.

Passaram [122 testes de memória](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/BO_DISPUTA_ENTREGA_C21.txt:153) e [35 da catraca](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/BO_DISPUTA_ENTREGA_C21.txt:161). [nao verificado]

Protegido ORM e suíte Django inteira: [nao verificado]. [nao verificado]

## 11/11 01:34 · sessao f3f0a41e4168 · FIO-E0-V4-E3 · sonda · origem=Code

**`lei: PARE` antes da cura:** `col348` tem **61 células `trabalha=False` e nenhuma `True`**, condição expressamente incluída no aval. Isso não prova escala cadastrada errada: o código do ciclo intermitente retorna `False` por desenho. `col574` sai como **CADASTRO**, pois o dado informa `situacao="desligado"`. [não verificado — COLAB col348/col574 e aval fornecidos]; `app/escala/models.py:820-823`.

**Outro impedimento técnico encontrado:** trocar a folga para `concorde` perde a garantia específica de rejulgamento quando chegar a adesão: o gancho atual depende da **palavra** `nunca_bateu`. `app/ponto/services/cartorio.py:1366-1375`.

Os valores da sombra abaixo são transcritos do dado fornecido, sem confirmação independente; por isso cada linha recebe `[não verificado]`. As referências `app/...:linha` são da cópia consultada. [não verificado — origem do dado fornecido]

**1. Censo dos 15**

`ativo:null` não significa inativo: o script usa `getattr(c, 'ativo', None)`, enquanto o cadastro declara `situacao`. O modelo não declara campo `ativo` para o colaborador. `app/colaboradores/models.py:283-380`; [não verificado — script fornecido].

**Zero batidas vivas implica adesão `None` pelo cálculo atual do cartório**, que procura a primeira batida com `retratada_em IS NULL`. Para quem tem batidas vivas, o censo permite afirmar existência de adesão, mas não sua data. `app/ponto/services/cartorio.py:1165-1173`.

| Colab | Situação / ativo | Admissão | Demissão | Isento | Batidas total/vivas → adesão | Fonte |
|---|---|---|---|---|---|---|
| col144 | ativo / null | 2023-04-19 | None | false | 0/0 → None | [não verificado] |
| col209 | ativo / null | 2018-06-16 | None | false | 0/0 → None | [não verificado] |
| col237 | ativo / null | 2024-02-05 | None | false | 82/82 → existe; data não medida | [não verificado] |
| col268 | ativo / null | 2024-08-21 | None | false | 0/0 → None | [não verificado] |
| col312 | ativo / null | 2025-02-13 | None | false | 0/0 → None | [não verificado] |
| col348 | ativo / null | 2025-05-10 | None | false | 62/58 → existe; data não medida | [não verificado] |
| col427 | ativo / null | 2025-10-14 | None | false | 0/0 → None | [não verificado] |
| col574 | desligado / null | 2026-03-12 | 2027-07-06 | false | 0/0 → None | [não verificado] |
| col650 | ativo / null | 2025-09-26 | None | false | 0/0 → None | [não verificado] |
| col969 | ativo / null | 2024-02-07 | None | false | 0/0 → None | [não verificado] |
| col1005 | ativo / null | 2026-10-27 | None | false | 0/0 → None | [não verificado] |
| col1009 | ativo / null | 2026-11-01 | None | false | 0/0 → None | [não verificado] |
| col1018 | ativo / null | 2026-11-04 | None | false | 0/0 → None | [não verificado] |
| col1019 | ativo / null | 2026-11-03 | None | false | 0/0 → None | [não verificado] |
| col1027 | ativo / null | 2026-11-04 | None | false | 0/0 → None | [não verificado] |

Nos vínculos, `∞` representa o `"None"` fornecido. A coluna final identifica **cobertura por vigência**, não comprova qual vínculo a célula nomeia: o script não entrega `escala_geradora_id`. O juiz prioriza a geradora e pode devolver `None` se ela estiver fora da vigência, mesmo existindo outro candidato. `app/escala/alimentacao.py:269-286`.

| Colab | Vínculos: pk; tipo; início→fim; ativa; posto | Cobre todas as datas X1 por vigência | Fonte |
|---|---|---|---|
| col144 | 118; 12x36 07–19; 2026-04-01→2026-06-23; false; 22<br>844; 12x36 flexível (só horas); 2026-06-24→∞; true; 22 | **844** | [não verificado] |
| col209 | 178; 12x36 13–21; 2026-07-21→∞; true; 422 | **178** | [não verificado] |
| col237 | 204; 12x36 07–19; 2026-07-21→∞; true; 487 | **204** | [não verificado] |
| col268 | 234; 12x36 07:30–17:30; 2026-07-21→∞; true; 508 | **234** | [não verificado] |
| col312 | 274; 12x36 flexível (só horas); 2026-07-21→∞; true; 437 | **274** | [não verificado] |
| col348 | 306; mrv sdf · intermitente · 06:30–18:30; 2026-07-21→∞; true; 467 | **306** | [não verificado] |
| col427 | 366; 12x36 19–07; 2026-07-21→∞; true; 448 | **366** | [não verificado] |
| col574 | 489; 6x1 Seg/Ter/Qua/Qui/Sex/Dom 07–15:20; 2024-01-01→2027-07-06; false; 1 | **489**, embora inativo | [não verificado] |
| col650 | 695; 12x36 07:30–17:30; 2026-07-21→∞; true; 449<br>1075; mesmo tipo; **2026-08-26→2026-07-20**; false; 449 | **695**; 1075 tem intervalo invertido | [não verificado] |
| col969 | 1367; 5x2 Seg–Sex 08–17; 2026-09-21→2026-10-17; false; 655<br>1434; TESTE12X36 · 12x36 07–19; 2026-10-18→∞; true; 655 | **1367**: X1 é 26–27/09 e 03–04/10 | [não verificado] |
| col1005 | 1471; 6x1 08:30–17:30; 2026-10-27→∞; true; 657 | **1471** | [não verificado] |
| col1009 | 1501; 6x1prochet · 6x1 Seg–Sab 07–16; 2026-11-01→∞; true; 454 | **1501** | [não verificado] |
| col1018 | 1517; 6x1 Seg–Sab 07–16; 2026-11-04→∞; true; 649 | **1517** | [não verificado] |
| col1019 | 1518; 12x36 06–18; 2026-11-03→∞; true; 454 | **1518** | [não verificado] |
| col1027 | 1527; 12x36 06–18; 2026-11-04→∞; true; 649 | **1527** | [não verificado] |

Um vínculo inativo **com data final** pode cobrir suas datas históricas; `ativa=false` isoladamente não significa SEM VÍNCULO. `app/escala/alimentacao.py:189-216`.

Na próxima tabela, cada distribuição é `veredito:quantidade`; `NB=nunca_bateu`. Os números abrangem **21/09–20/11**, não apenas X1. A retenção usa `HOJE=10/11` no script. [não verificado — script fornecido]

| Colab | Células | `trabalha=False` × veredito | `trabalha=True` × veredito | TRABALHO retido com NB | X1 | Fonte |
|---|---:|---|---|---:|---:|---|
| col144 | 61 | None:6; NB:25 | None:5; NB:25 | 25 | 25 | [não verificado] |
| col209 | 61 | None:6; NB:25 | None:5; NB:25 | 25 | 25 | [não verificado] |
| col237 | 61 | None:5; concorde:24; fato_em_ausencia:1 | None:6; cobrado:1; concorde:24 | 0 | 1 | [não verificado] |
| col268 | 61 | None:6; NB:25 | None:5; NB:25 | 25 | 25 | [não verificado] |
| col312 | 61 | None:6; NB:25 | None:5; NB:25 | 25 | 25 | [não verificado] |
| col348 | 61 | None:11; concorde:41; furo:9 | **nenhuma** | 0 | 4 | [não verificado] |
| col427 | 61 | None:6; NB:25 | None:5; concorde:6; NB:19 | 19 | 24 | [não verificado] |
| col574 | 61 | None:1; NB:7 | None:10; NB:43 | 43 | 7 | [não verificado] |
| col650 | 61 | None:5; NB:25 | None:6; NB:25 | 25 | 25 | [não verificado] |
| col969 | 61 | None:21; NB:4 | None:24; NB:12 | 12 | 4 | [não verificado] |
| col1005 | 25 | None:2; NB:1 | None:9; NB:13 | 13 | 1 | [não verificado] |
| col1009 | 20 | None:2; concorde:2; NB:1 | None:9; concorde:4; NB:2 | 2 | 1 | [não verificado] |
| col1018 | 17 | None:2; NB:1 | None:9; NB:5 | 5 | 1 | [não verificado] |
| col1019 | 18 | None:6; NB:3 | None:5; NB:4 | 4 | 3 | [não verificado] |
| col1027 | 17 | None:5; NB:3 | None:6; NB:3 | 3 | 3 | [não verificado] |

Somam **226 dias de TRABALHO retidos com NB**, antes das exclusões cadastrais. Não são 226 dias X1. [não verificado — soma dos COLAB fornecidos]

**2. Classificação pela condição do aval**

“Entra” abaixo significa que **os campos disponíveis não mostram impedimento cadastral**, sujeito à lacuna da geradora e à identificação de teste. A parada global por `col348` continua valendo. [não verificado — aplicação do aval ao dado fornecido] [nao verificado]

| Colab | Classificação | Critério e campo | Fonte |
|---|---|---|---|
| col144 | **(iii) entra** | `situacao=ativo`, adesão None; vínculo 844 cobre X1; 30 células True, 25 trabalhos NB retidos | [não verificado] |
| col209 | **(iii) entra** | ativo, adesão None; vínculo 178; 30 True, 25 trabalhos NB retidos | [não verificado] |
| col237 | **(iii) sem exclusão cadastral demonstrada**, mas sem alvo NB atual demonstrado | ativo, vínculo 204; `batidas_vivas=82`; **nenhuma célula NB** na janela | [não verificado] |
| col268 | **(iii) entra** | ativo, adesão None; vínculo 234; 30 True, 25 trabalhos NB retidos | [não verificado] |
| col312 | **(iii) entra** | ativo, adesão None; vínculo 274; 30 True, 25 trabalhos NB retidos | [não verificado] |
| col348 | **(ii) PARE** | `trab_x_veredito` contém somente False; tipo informado intermitente | [não verificado] |
| col427 | **(iii) entra** | ativo, adesão None; vínculo 366; 30 True, 19 trabalhos NB retidos | [não verificado] |
| col574 | **(i) CADASTRO — fora da cura** | `situacao=desligado`; a demissão futura informada não autoriza ignorar esse campo | [não verificado] |
| col650 | **(iii) entra pelo vínculo 695** | ativo, adesão None; 31 True, 25 trabalhos NB retidos; vínculo 1075 invertido merece lista cadastral separada | [não verificado] |
| col969 | **TESTE não comprovado; classificação pendente** | `tipo` contém TESTE12X36, empresa 1/posto 655; porém o vínculo que cobre X1 é o 1367, 5x2 | [não verificado] | [nao verificado]
| col1005 | **(iii) entra** | ativo, adesão None; vínculo 1471; 22 True, 13 trabalhos NB retidos | [não verificado] |
| col1009 | **(iii) entra** | ativo, adesão None; vínculo 1501; 15 True, 2 trabalhos NB retidos | [não verificado] |
| col1018 | **(iii) entra** | ativo, adesão None; vínculo 1517; 14 True, 5 trabalhos NB retidos | [não verificado] |
| col1019 | **(iii) entra** | ativo, adesão None; vínculo 1518; 9 True, 4 trabalhos NB retidos | [não verificado] |
| col1027 | **(iii) entra** | ativo, adesão None; vínculo 1527; 9 True, 3 trabalhos NB retidos | [não verificado] |

**Não há SEM VÍNCULO demonstrado pelos intervalos fornecidos**; a autoridade efetiva por célula permanece não medida pela ausência da geradora no relatório. [não verificado — vínculos fornecidos]; `app/escala/alimentacao.py:269-286`.

Sobre TESTE: [nao verificado]

- O colaborador não tem flag `teste`; `situacao` admite ativo, afastado, férias e desligado. `app/colaboradores/models.py:283-380`.
- Existe critério de **empresa de diagnóstico na UI**: CNPJ iniciado por `11222333`. Isso não é uma classificação universal usada pelo export. `app/colaboradores/queries.py:78-87`.
- O raspado fornece apenas IDs de empresa/posto, sem CNPJ, nomes ou praça; portanto **empresa 1/posto 655 não comprovam teste**. [não verificado — campos fornecidos] [nao verificado]
- `TESTE12X36` sugere cadastro de teste **[inferência]**, mas é rótulo de escala; apelido não define sua regra. `app/escala/models.py:111-114`; `app/escala/models.py:420-428`; [não verificado — tipo de col969 fornecido].
- “Flexível (só horas)” em col144/col312 não comprova isenção: o juiz de isenção lê exclusivamente `isento_ponto`. `app/escala/models.py:425-428`; `app/core/constants.py:13-18`.

**3. X1 com ausência**

O resultado fornecido é **1 dia: col237, 2026-11-07**; os outros 14 colaboradores têm zero. A data é identificável porque esse colaborador tem somente essa data em `DADOS`. [não verificado — COLAB col237 e script fornecidos]

Isso demonstra somente **`CelulaDia.ausencia_id` preenchido** em uma célula X1. O script não consulta aprovação, tipo, período nem integralidade da ausência; portanto “ausência aprovada sobreposta” está **[não verificado]**. O campo é uma FK lavrada na célula. `app/escala/models.py:1792-1798`; [não verificado — expressão `x1_com_ausencia` do script].

**4. Leitores e efeito de NB → `concorde`**

A mudança sai do conjunto acusatório porque NB pertence a `VEREDITOS_ACUSAM` e `concorde` não pertence. Os efeitos abaixo são condicionais à execução de cada leitor; não provam que perguntas/chamados já tenham sido alterados na sombra. `app/escala/models.py:1767-1768`; [não verificado — execução na sombra].

| Leitor | Efeito para a folga que passar a `concorde` | Arquivo:linha |
|---|---|---|
| Retenção do export, `motivos_retencao_celula` | O dia deixa de reter por `espelho_nunca_bateu`; também não entra no ramo de veredito ausente | `app/folha/export.py:1233-1256` |
| Vigia de isenção por pausa | Sai do universo acusatório contado como isento pela pausa | `app/folha/services/vigia_pausa.py:155-157` |
| Lista C5, `colabs_nunca_bateu` | O dia sai da lista; o colaborador permanece se tiver outros dias NB elegíveis | `app/escala/services/cadastro_realidade.py:181-200` |
| Universo de adesão | O dia deixa de alimentar `dias/prev/fur`; trabalhos NB restantes continuam no universo | `app/chamados/services/adesao.py:32-40` |
| Predicado `dia_nunca_bateu_sem_adesao` | Passa a retornar falso | `app/chamados/services/fabrica_da_celula.py:87-89` |
| `motivos_a_perguntar` | Sai do retorno exclusivo NB; folga continua sem pergunta por `dia_sem_trabalho`, e `concorde` também não acusa | `app/chamados/services/fabrica_da_celula.py:105-121` |
| `perguntar_pela_celula` | Cai a saída antecipada NB; folga não passa na condição de cadastro sem turno | `app/chamados/services/fabrica_da_celula.py:183-202` |
| `motivo_fora_da_fabrica` | Some a causa `celula_nunca_bateu`; nos demais ramos pode aparecer `pergunta_do_chamado_ja_encerrada` | `app/chamados/services/disputa_emissao.py:758-773` |
| `_resposta_concorda_com_a_luz` | Pode reconhecer concordância, depois das demais verificações de resposta/batida | `app/chamados/juizes.py:1319-1321` |
| `_pergunta_viva_calc` — C6 | Some a sustentação pelo veredito acusador; a pergunta pode deixar de ser viva, respeitadas as outras precedências | `app/chamados/juizes.py:1374-1385` |
| `PerguntaDisputa.q_vivas` | Sai de `_celula_acusa` e pode entrar em `_celula_absolve`; resposta e ata provisória ainda interferem | `app/chamados/models.py:3340-3358` |
| Candidatas a perguntas absolvidas | Passam a poder entrar no universo; C6 e tranca ainda são consultados antes do resultado | `app/chamados/reconciliador.py:641-667` |
| `_absolvido_pela_celula` | Pode permitir retratação do chamado, sem marcos pendentes e respeitando proteção da resposta | `app/chamados/reconciliador.py:990-1029` |
| Filhos que seguram pai de limbo | Filho absolvido deixa de segurar o pai por esse caminho | `app/chamados/services/limbo_folgas.py:191-195` |
| Silêncio por ausência, `_premissa_morre` | Se a premissa do catálogo morreu, o veredito deixa de impedir o encerramento | `app/chamados/services/silencio_ausencia.py:28-41` |
| Recebimento de resposta tardia | Deixa de tentar renascer o chamado pelo ramo de célula acusadora; segue o tratamento tardio | `app/chamados/services/resposta_tardia.py:45-59` |
| Vigia de fio morto | O dia sai das células `acesas` desse universo | `app/chamados/services/vigia_fio.py:331`; `app/chamados/services/vigia_fio.py:378-379` |
| Contador de fio mudo/sem adesão | Deixa de alimentar `_dias_sem_adesao`; folga/concorde não ganha motivo de pergunta por essa troca | `app/chamados/management/commands/disparar_perguntas_competencia.py:111-121` |
| Veto de cobrança por marco | **Continua vetando por FOLGA**, antes da leitura do conjunto que contém NB | `app/ponto/marcos.py:119`; `app/ponto/marcos.py:155-162` |
| Vigia ausência × veredito | Sai do universo acusatório comparado com ausência integral aprovada | `app/ponto/services/vigia_ausencia_veredito.py:41-57` |
| Ausência sem efeito na célula | Deixa de incrementar `nunca_bateu`; ainda pode contar `sem_efeito` por lâmpada apagada | `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:58-63` |
| Relatório dia da frota | **Sem mudança para False**: conta folga e continua antes de testar ACUSAM | `app/relatorios/dia_da_frota.py:47-55` |
| Status da lavra / acusação da parcial | Folga simples sem batida já recebe `folga`; com ausência parcial, retirar NB pode alterar a classificação de irregularidade | `app/ponto/status_dia.py:68-70`; `app/ponto/status_dia.py:104-110`; `app/ponto/status_dia.py:139-162` |

As auditorias também leem o conjunto, sem realizar cura automaticamente:

| Auditoria | Efeito | Arquivo:linha |
|---|---|---|
| Contraprova F1t | Se o item continuar no piso indevidamente, passa a apontar célula que não acusa | `app/chamados/management/commands/auditar_invariantes_chamados.py:198` |
| Contraprova do piso | Folga já fica fora pelo `not c.trabalha` | `app/chamados/management/commands/auditar_invariantes_chamados.py:377` |
| Recorte A4 | Folga continua no piso correspondente por `not trabalha` | `app/chamados/management/commands/auditar_invariantes_chamados.py:1447` |
| C2/C3 | Pode entrar na medição de pergunta morta pela célula ou cobrança viva em célula concorde, conforme os demais filtros | `app/chamados/management/commands/auditar_invariantes_chamados.py:1601-1606` |
| Piso F1t | `concorde` deixa de satisfazer a condição acusatória | `app/chamados/management/commands/auditar_invariantes_chamados.py:1698-1713` |

Leitores derivados relevantes:

- Cadastro × Realidade formata a C5; não possui outra regra para NB. `app/escala/services/cadastro_realidade.py:204-207`; `app/escala/services/cadastro_realidade.py:878`.
- `vigia_retido_calado` usa os **colaboradores** da C5 para roteamento. Havendo trabalho NB restante, a pessoa pode continuar roteada; trocar a célula isoladamente não reescreve o retrato já lavrado. `app/folha/vigia_retido_calado.py:33-48`.
- `vigiar_fio_morto` publica o contador composto, e o pipeline o acompanha com dono CADASTRO. `app/chamados/management/commands/vigiar_fio_morto.py:182`; `app/chamados/management/commands/vigiar_fio_morto.py:241`; `app/core/management/commands/pipeline_placar.py:136`.
- O placar publica o contador de NB em dia coberto. `bin/placar_code.sh:419`.

Não confundir com outros campos homônimos: o `nunca_bateu` dos rankings de furos é calculado por batidas/previstos/furos; `subcategoria='nunca_bateu'` em pós-vínculo é leitura de chamado. `app/escala/services/furos_diarios.py:211-242`; `app/escala/services/furos_diarios.py:283`; `app/escala/services/furos_diarios.py:334`; `app/colaboradores/services/pos_vinculo.py:36`.

**O gancho HX-CARTORIO-NB-REJULGA**

**Não há garantia de que a folga convertida continue sendo rejulgada somente porque a adesão chegou.** O funcionamento atual é:

1. A primeira batida viva escolhe entre ramo sem adesão e ramo normal. `app/ponto/services/cartorio.py:1165-1173`.
2. Sem adesão, qualquer veredito preenchido pode ser considerado em dia se a impressão coincidir. `app/ponto/services/cartorio.py:1207-1216`.
3. Com adesão, a exceção é literalmente `cel.veredito != 'nunca_bateu'`: NB impede o pulo; `concorde` permite o pulo. `app/ponto/services/cartorio.py:1369-1375`.
4. A impressão não contém adesão global; contém os demais insumos do julgamento. Uma batida próxima, outro insumo alterado ou `forcar` pode provocar rejulgamento, mas adesão distante, sozinha, não o garante. `app/ponto/services/cartorio.py:443-522`; `app/ponto/services/cartorio.py:1318-1319`; `app/ponto/services/cartorio.py:1359-1375`.

Portanto, o comentário de `1242-1250` corresponde ao mecanismo implementado: **depende da palavra, no ramo normal; não de uma memória persistida de que a célula passou pelo ramo sem adesão**. O teste existente cobre NB com primeira batida três dias depois; não comprova preservação para folga `concorde`. `app/ponto/services/cartorio.py:1242-1252`; `app/ponto/tests/test_cartorio_nb_readesao.py:11-23`.

A precedência já é chamada no ramo sem adesão, mas seu resultado é sobrescrito para NB quando não há cobertura integral. A porta atual resolve o literal antes do derivador em `923`, não na antiga referência `564` do aval. `app/ponto/services/cartorio.py:1237-1253`; `app/ponto/portas/celula.py:919-925`.

**5. Limites e discrepâncias do instrumento**

- **Nenhuma das 15 linhas fornecidas contém `ERRO`; o encerramento informa `n=15` e `rc_sombra=0`.** Isso não elimina limitações semânticas dos campos. [não verificado — saída fornecida]
- **`x1_dias` conta a lista estática `DADOS`**, sem conferir se cada célula continua False/NB/retida. A soma é 174, mas col237 tem 1 X1 e col348 tem 4 X1 apesar de ambos terem **zero NB em toda a janela**. Logo, o censo não confirma “174 folgas atualmente retidas por NB”. [não verificado — script e COLAB fornecidos]
- A distribuição contém **169 células False/NB na janela inteira**; esse número não é a interseção com X1. Por exemplo, col427 tem 25 False/

[truncado em 20000 de 21593 caracteres]

## 11/11 01:33 · sessao a555301a0a81 · FIO-E0-V4-E3 · construtor · origem=Code

**PARE: a cura não foi aplicada.** O C6 ainda pode consultar ausências por pergunta; não consegui garantir carga constante sem alterar sua alimentação no arquivo proibido. A testemunha executável está em [test_e3_tarefa_admin_sem_banco.py:148](/home/ronald/wt-codex-fio-e0-v4-e3-010719/app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:148). [nao verificado]

Os dois REDs falharam por assertion: receberam `fio='nada'`, esperando `admin`, sob hipótese explícita de última resposta textual E1. Não houve GREEN nem mutantes da cura. [Resultados e comandos](/home/ronald/wt-codex-fio-e0-v4-e3-010719/app/E3_TAREFA_ENTREGA.txt:181).

O [relatório entregue](/home/ronald/wt-codex-fio-e0-v4-e3-010719/app/E3_TAREFA_ENTREGA.txt:1) contém o censo, as saídas condicionais e os limites. Os testes ORM interromperam por falta de Django; seu resultado permanece **[nao verificado]**. [nao verificado]

## 11/11 01:32 · sessao 3de619f39c74 · - · - · origem=Ronald (tmux)

**Cadastro/teste.** `Colaborador` tem `situacao` e `isento_ponto`, mas não tem campo `ativo` nem marcador `teste`; `ativo:null` do script decorre do `getattr(..., None)`, não comprova inatividade. Os campos estão em `app/colaboradores/models.py:283-398`.

Existe um reconhecimento limitado de **empresa de diagnóstico** na UI: `empresas_visiveis()` exclui CNPJ começando por `11222333` e empresas inativas; o próprio código declara que exportação e lógica interna não usam esse filtro. Praça de teste/fantasma é escondida por `ativa=False`. O raspado não traz CNPJ, nomes, praça ou esses flags; `empresa=1/2/3` e `posto=...` não bastam para identificar teste. (`app/colaboradores/queries.py:78-94`)

`TESTE12X36` no col969 sugere teste **[inferência]**, mas é nome de **tipo de escala**: o apelido é apenas rótulo, e `nome_canonico()` o antepõe ao ciclo. Não prova que a pessoa seja de teste. Ademais, os quatro X1 de col969 são setembro/início de outubro, cobertos temporalmente pelo vínculo 1367; o 1434, com esse rótulo, começa em 18/10. (`app/escala/models.py:111-114`, `app/escala/models.py:420-424`; datas: anexo fornecido, sem arquivo local **[não verificado]**)

**Vínculo por dia.** Cobertura temporal exige início ≤ dia, fim nulo ou ≥ dia, e vínculo ativo **ou** com fim declarado. Portanto, `ativa=False` não elimina cobertura histórica; o vínculo 489 de col574 tem candidatura temporal nos X1 apesar do desligamento cadastral. (`app/escala/alimentacao.py:190-216`)

A autoridade efetiva é `CelulaDia.escala_geradora`: se apontar vínculo fora da vigência, a juíza devolve `None`; só sem geradora escolhe entre candidatos por vigência. O script não coleta geradora/DNA por dia, logo permite listar candidatos temporais, não certificar “qual vínculo o juiz escolheu” para cada X1. (`app/escala/alimentacao.py:269-286`, `app/escala/models.py:1692-1700`)

Pelas datas fornecidas, candidatos X1: 144→844; 209→178; 237→204; 268→234; 312→274; 348→306; 427→366; 574→489; 650→695; 969→1367; 1005→1471; 1009→1501; 1018→1517; 1019→1518; 1027→1527. O 1075 de col650 tem fim anterior ao início e não cobre nenhum dia pela regra. Datas do raspado **[não verificado]**; regra em `app/escala/alimentacao.py:214-216`.

**Condição PARE.** Col348 apresenta 61 células `trabalha=False`, nenhuma `True`, e vínculo rotulado intermitente: aciona o exemplo literal “nenhuma célula trabalha=True na janela” da condição do aval. Entretanto, isso não prova erro de escala: intermitente retorna `False` por desenho, pois não há dia previsto; o controle continua sendo entrada+saída quando trabalha. “Intermitente não pede batida” seria afirmação errada. (`app/escala/models.py:36-43`, `app/escala/models.py:816-823`; contagem/aval fornecidos **[não verificado]**)

**Adesão.** O juiz e o cartório usam a primeira batida com `retratada_em IS NULL`; nenhuma viva implica adesão `None`. O script conta o mesmo universo, mas não imprime a data da primeira viva: para col237 e col348 só demonstra adesão existente, sem data. (`app/ponto/services/adesao.py:31-45`, `app/ponto/services/cartorio.py:1165-1167`; contagens do anexo **[não verificado]**)

“Flexível (só horas)” não equivale a isenção: esse texto nasce da ausência de horário formatado; o juiz de isenção lê somente `Colaborador.isento_ponto`. (`app/escala/models.py:425-428`, `app/core/constants.py:13-18`)

**Ausência.** `x1_com_ausencia` conta células nas datas da lista de entrada cujo `ausencia_id` está preenchido. Não consulta aprovação, tipo, período, cobertura integral/parcial nem sobreposição real; o campo é uma FK lavrada na célula. Portanto, a saída demonstra **uma associação, col237 em 07/11/2026**, mas aprovação/cobertura requer **[não verificado]**. (`app/escala/models.py:1792-1798`; expressão e data no anexo fornecido **[não verificado]**)

**Outras limitações do script.**

- `x1_dias` é quantidade de datas na lista `DADOS`, não recontagem de células ainda `False/nunca_bateu` na sombra. Explica haver col237 e col348 no universo apesar de seus histogramas atuais não terem `nunca_bateu`. **[não verificado: anexo fornecido]**
- `trab_x_veredito` cobre 21/09–20/11 inteiros, não somente X1; `retidos` e trabalho retido usam essa janela, com `hoje=10/11` fixo. **[não verificado: anexo fornecido]**
- O retentor corta admissão/demissão e dispensa isento; veredito acusador pode reter sem corte por `hoje`, enquanto veredito ausente só retém dia passado. Assim, seus totais não significam “dias X1 retidos pelo ramo adesão=None”. (`app/folha/export.py:1192-1205`, `app/folha/export.py:1233-1256`)
- `retencao_chaves` é apenas a lista de chaves do primeiro detalhe retornado; `cargo_vazio` é apenas `not c.cargo`; nenhum dos dois identifica teste. **[não verificado: expressão do anexo fornecido]** [nao verificado]
- Nenhuma das quinze linhas fornecidas possui chave `ERRO`; isso não constitui nova execução/verificação na sombra. **[não verificado: saída fornecida]**

## 11/11 01:32 · sessao e6f4c8ed7d43 · - · - · origem=Ronald (tmux)

O gancho **depende da palavra `nunca_bateu`**, dentro do ramo normal, e não de uma memória persistida do ramo anterior: `app/ponto/services/cartorio.py:1369`.

- A adesão seleciona o ramo por primeira batida não retratada: `app/ponto/services/cartorio.py:1165-1173`.
- Sem adesão, a impressão é calculada com batidas/chamados vazios; qualquer veredito preenchido permite pular se a impressão coincidir: `app/ponto/services/cartorio.py:1207-1216`.
- Com adesão, o teste explícito `cel.veredito != 'nunca_bateu'` impede o pulo para NB; `concorde` satisfaz esse teste e pode ser pulado: `app/ponto/services/cartorio.py:1369-1375`.
- A impressão não inclui adesão global; inclui batidas próximas, cobertura, chamados, DNA, veto, teto, regra, fio, parcialidade e isenção: `app/ponto/services/cartorio.py:443-522`.
- Portanto, folga convertida para `concorde` **perde a garantia desse gancho**. Rejulga se a impressão mudar ou houver `forcar`; só a chegada de adesão fora da janela não assegura isso: `app/ponto/services/cartorio.py:1318-1319`, `app/ponto/services/cartorio.py:1359-1375`.
- O teste existente exercita NB com primeira batida três dias depois, fora da janela local; não demonstra preservação para `concorde`: `app/ponto/tests/test_cartorio_nb_readesao.py:11-23`.

Outros leitores nas pastas atribuídas:

| Leitor | Efeito NB → concorde em folga |
|---|---|
| `celula_veta_cobranca` | Continua vetando por `FOLGA`, pois lê `trabalha=False` antes do conjunto que contém NB. `app/ponto/marcos.py:119`, `app/ponto/marcos.py:155-162`. |
| `vigia_ausencia_veredito.censo` | Sai do universo de vereditos acusatórios e, consequentemente, das discordâncias com ausência integral aprovada. `app/ponto/services/vigia_ausencia_veredito.py:41-57`. |
| `ausencias_sem_efeito_na_celula.classificar` | Deixa de incrementar `nunca_bateu`; ainda pode incrementar `sem_efeito` se a ata tiver lâmpada apagada. `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:58-63`. |
| `dia_da_frota` | Sem efeito para `trabalha=False`: a folga já é contada e o laço continua antes da leitura de `VEREDITOS_ACUSAM`. `app/relatorios/dia_da_frota.py:47-55`. |
| `status_da_lavra` / `parcial_acusa_o_resto` | NB integra `_IRREGULAR` e `_ACUSA_NA_PARCIAL`; folga simples sem batida já recebe `folga`, mas uma ausência parcial pode deixar de ser classificada como irregular ao passar a concorde. `app/ponto/status_dia.py:68-70`, `app/ponto/status_dia.py:104-110`, `app/ponto/status_dia.py:139-162`. |

As referências de `config/crons.py` ao conjunto são comentários junto das chamadas aos vigias, não um novo predicado: `app/config/crons.py:2213-2217`, `app/config/crons.py:2231-2236`. O `nunca_bateu` de pós-vínculo é subcategoria de chamado, não leitura de `CelulaDia.veredito`: `app/colaboradores/services/pos_vinculo.py:36`.

## 11/11 01:32 · sessao 300317ecdcfd · - · - · origem=Ronald (tmux)

Leitores diretos executáveis encontrados em `chamados/`, `folha/`, `escala/`; efeito abaixo é leitura estática condicionada à troca da célula de `nunca_bateu` para `concorde`, sem execução nem evidência de produção. [`app/escala/models.py:1767`]

| Leitor | Efeito da troca para `concorde` |
|---|---|
| `motivos_retencao_celula` | Deixa de entrar no ramo acusador; esse dia deixa de reter por `espelho_nunca_bateu`. O ramo seguinte exige veredito `None`, portanto `concorde` também não o aciona. [`app/folha/export.py:1233`, `app/folha/export.py:1257`] |
| `_dias_isentos` | Dia deixa do universo contado como isenção por pausa; exige veredito acusador e `nao_retem_pela_pausa`. [`app/folha/services/vigia_pausa.py:155`] |
| `colabs_nunca_bateu` | Dia sai da lista C5; pessoa só sai se não restar outro dia admitido no universo. Desligados ainda passam pelo filtro do retrato da folha. [`app/escala/services/cadastro_realidade.py:181`, `app/escala/services/cadastro_realidade.py:197`] |
| `universo_adesao` | Dia sai da contagem `dias/prev/fur` usada por `sincronizar_adesao`. Trabalho `nunca_bateu` restante continua no universo. [`app/chamados/services/adesao.py:32`, `app/chamados/services/adesao.py:58`] |
| `dia_nunca_bateu_sem_adesao` | Predicado deixa de reconhecer o dia. [`app/chamados/services/fabrica_da_celula.py:87`] |
| `motivos_a_perguntar` | Cai o retorno antecipado por NB, mas folga continua sem pergunta pelo juiz `dia_sem_trabalho`; além disso `concorde` cai no retorno por não acusar. [`app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:113`, `app/chamados/services/fabrica_da_celula.py:120`] |
| `perguntar_pela_celula` | Cai o retorno antecipado NB. Folga não entra na pauta de cadastro sem turno, pois esta exige `not dia_sem_trabalho(cel)`; motivos seguem sujeitos às guardas acima. [`app/chamados/services/fabrica_da_celula.py:183`, `app/chamados/services/fabrica_da_celula.py:202`] |
| `motivo_fora_da_fabrica` | Causa `celula_nunca_bateu` desaparece. Havendo pergunta já materializada e alcançando esse ramo, `concorde` gera `pergunta_do_chamado_ja_encerrada`; os demais critérios continuam na ordem existente. [`app/chamados/services/disputa_emissao.py:758`, `app/chamados/services/disputa_emissao.py:772`] |
| `_resposta_concorda_com_a_luz` | Se todos os demais testes de batida/resposta concordante já passaram, passa a retornar `True`, pois `concorde` está fora de ACUSAM. [`app/chamados/juizes.py:1319`, `app/chamados/juizes.py:1321`] |
| `_pergunta_viva_calc` | Some o retorno `True` do ramo acusador; pode absolver a pergunta, respeitando precedência de resposta/identidade e o teto de marcos pendentes. [`app/chamados/juizes.py:1324`, `app/chamados/juizes.py:1374`, `app/chamados/juizes.py:1385`] |
| `PerguntaDisputa.q_vivas` | Célula passa de `_celula_acusa` para `_celula_absolve`, salvo ata provisória. Resposta não vazia pode manter a candidata; é pré-filtro generoso, não igualdade com o juiz. [`app/chamados/models.py:3340`, `app/chamados/models.py:3355`, `app/chamados/models.py:3357`] |
| `qs_perguntas_absolviveis` | Perguntas do dia entram no universo candidato. `perguntas_absolvidas_pela_celula` ainda consulta C6 e tranca antes de devolver; o escritor pode carimbar/reconciliar e fechar a disputa pela porta. [`app/chamados/reconciliador.py:641`, `app/chamados/reconciliador.py:661`, `app/chamados/reconciliador.py:667`, `app/chamados/reconciliador.py:608`] |
| `_absolvido_pela_celula` | Pode devolver `(dia, cel)` se não houver marcos pendentes; permite a `retratar_chamados_absolvidos_por_celula` retratar chamado de turno pela porta existente. Resposta esperando validação continua sujeita à proteção de `_retratar`. [`app/chamados/reconciliador.py:1027`, `app/chamados/reconciliador.py:1029`, `app/chamados/reconciliador.py:1004`, `app/chamados/reconciliador.py:990`] |
| Leitor de filhos que seguram pai limbo | Filho com célula `concorde` passa pelo `continue`; deixa de segurar o pai por esse caminho. [`app/chamados/services/limbo_folgas.py:191`, `app/chamados/services/limbo_folgas.py:194`] |
| `_premissa_morre` | Se premissa do catálogo já morreu, passa a autorizar morte da cobrança por ausência; a ausência e outras guardas pertencem ao chamador. [`app/chamados/services/silencio_ausencia.py:28`, `app/chamados/services/silencio_ausencia.py:36`, `app/chamados/services/silencio_ausencia.py:41`] |
| `resposta_tardia.receber` | Não tenta renascer o chamado pela célula; resposta segue como tardia, com nota/log e eventual pauta por conteúdo. [`app/chamados/services/resposta_tardia.py:45`, `app/chamados/services/resposta_tardia.py:53`, `app/chamados/services/resposta_tardia.py:59`] |
| `vigia_fio` | Dia sai da lista `acesas`, logo sai desse universo de lâmpadas acusadoras usado no censo de fio morto. [`app/chamados/services/vigia_fio.py:331`, `app/chamados/services/vigia_fio.py:378`] |
| `disparar_perguntas_competencia.contador_fio_mudo` | Dia deixa de alimentar `_dias_sem_adesao`; folga/concorde não gera motivos pelo juiz da fábrica, portanto não vira alvo de fio mudo por essa troca. C5 informativa é relida pela função única. [`app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `app/chamados/management/commands/disparar_perguntas_competencia.py:115`, `app/chamados/management/commands/disparar_perguntas_competencia.py:54`] |

Auditorias que leem ACUSAM, sem curar automaticamente:

- `_contraprova_f1t`: se item continuar indevidamente no piso F1t, agora acusa contraprova falsa porque célula não acusa. [`app/chamados/management/commands/auditar_invariantes_chamados.py:198`]
- `contraprova_piso`: para folga, permanece fora do teste de trabalho porque `not c.trabalha` já basta; trocar NB não altera esse corte. [`app/chamados/management/commands/auditar_invariantes_chamados.py:377`]
- Recorte A4: folga já vai ao piso `_a4c` por `not trabalha`; continua ali com concorde. [`app/chamados/management/commands/auditar_invariantes_chamados.py:1447`]
- C2: pergunta sem materialização/validação/resposta pode entrar no contador de morta por célula; C3 mede explicitamente cobrança viva em célula concorde. [`app/chamados/management/commands/auditar_invariantes_chamados.py:1601`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1606`]
- F1t: dia concorde deixa de satisfazer o piso de célula acusadora. [`app/chamados/management/commands/auditar_invariantes_chamados.py:1713`]

Leitores indiretos relevantes:

- `_nunca_bateu` formata C5 para Cadastro × Realidade, sem juiz adicional. [`app/escala/services/cadastro_realidade.py:204`, `app/escala/services/cadastro_realidade.py:207`, `app/escala/services/cadastro_realidade.py:878`]
- `vigia_retido_calado` usa os **colaboradores** presentes em C5 para rotear linhas sem fio; se pessoa continuar com trabalho NB, segue roteada. Esse vigia lê retratos já lavrados, portanto a troca isolada de célula não reescreve seu retrato. [`app/folha/vigia_retido_calado.py:33`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`]
- `vigiar_fio_morto` imprime/carimba o contador composto `nunca_bateu_sem_adesao`; não é leitura independente da palavra da célula. [`app/chamados/management/commands/vigiar_fio_morto.py:182`, `app/chamados/management/commands/vigiar_fio_morto.py:241`]

Não confundir `nunca_bateu` calculado nos rankings de `furos_diarios` com leitor do veredito da célula: ali os booleanos vêm de zero batida/previstos/furos, e a consulta de `subcategoria='nunca_bateu'` é de chamado. [`app/escala/services/furos_diarios.py:211`, `app/escala/services/furos_diarios.py:242`, `app/escala/services/furos_diarios.py:283`, `app/escala/services/furos_diarios.py:334`]

## 11/11 01:29 · sessao 812549853dff · FIO-E0-V4-E4B · verificador · origem=Code

**NAO LIBERA.** As curas diretas existem, mas P3/P5 não abrangem toda a cascata acionada pela reabertura; também sobreviveram mutações relevantes fora do catálogo. Referências: `app/chamados/services/disputa_emissao.py:1782`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:1633`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`, `app/chamados/tests/test_e4b_classe_e_memoria.py:610`.

1. **P1..P6 — existência, origem e fechamento**

   | Item | Verificação |
   |---|---|
   | **P1** | **Cura presente na porta e nos chamadores do backfill:** `hoje` chega ao C6, ao planejamento, complemento e execução. Fecha a perda de parâmetro apontada nesses caminhos. O S143 passa `None`, coerentemente com sua assinatura sem relógio. Referências: `app/chamados/services/disputa_emissao.py:1796`, `app/chamados/services/disputa_emissao.py:1812`, `app/chamados/services/disputa_emissao.py:1913`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:86`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:131`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:171`, `app/chamados/signals.py:115`. |
   | **P2** | **Fecha a duplicação para recusa já conhecida:** não chama `reabrir()`; grava somente a nota deduplicada. Não certifica recusa que apareça na segunda consulta, dentro de `reabrir()`: nesse caminho os efeitos internos continuam possíveis e a nota E4B informa apenas `porta_nao_reabriu`. Referências: `app/chamados/services/disputa_emissao.py:1837`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/disputa_emissao.py:1845`, `app/chamados/models.py:1390`. |
   | **P3** | **Fecha a cascata direta do credor enumerado:** une não validadas da relavra S2 e IDs da autoridade de ressuscitação; inclui a própria pergunta, inclusive com âncora no pai. Recusa entregue e indeterminada. **Não fecha toda a cascata dos receivers nem a corrida com exportação**, detalhadas no item 5. Referências: `app/chamados/services/disputa_emissao.py:1788`, `app/chamados/services/disputa_emissao.py:1823`, `app/chamados/services/disputa_emissao.py:1826`, `app/chamados/models.py:309`, `app/chamados/signals.py:216`. |
   | **P4** | **Cura restritiva presente:** usa timestamp nulo e `texto_vazio`, recusa sem limpar/carimbar o texto; evento interrompe o resolver, backfill retira o dia dos atos e imprime LIMBO. A quantidade real continua **[nao verificado]**. Referências: `app/chamados/services/disputa_emissao.py:1828`, `app/chamados/services/disputa_emissao.py:1914`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:286`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:357`. |
   | **P5** | **Universo documentado e histórico terminal alheio retirado da antecipação.** A lista explícita segue C→D→Ch→P, mas a expansão não cobre todos os efeitos indiretos do ato. Portanto, **não fecha o achado integralmente**. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:8`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:208`. |
   | **P6** | **Os dois sobreviventes antigos foram cobertos:** C7 isolada e pai adicional morrem por assertion. O catálogo inteiro também morreu assim; contudo, apareceram sobreviventes adicionais sobre identidade da recusa e completude dos PKs. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:51`, `app/chamados/tests/test_e4b_classe_e_memoria.py:243`, `app/chamados/tests/test_e4b_classe_e_memoria.py:319`, `app/chamados/tests/test_e4b_classe_e_memoria.py:708`. |

   **Destino versus pai:** a autoridade continua preferindo destino, independentemente do status; sem destino, usa o pai. A inclusão de `p` na carga fecha a fuga direta do pai sem destino. A respondida continua fora da porta E4B e segue pelo HX existente, cuja reabertura do pai não passa pela cerca nova. Referências: `app/chamados/services/fio.py:117`, `app/chamados/services/disputa_emissao.py:1812`, `app/chamados/services/disputa_emissao.py:1823`, `app/chamados/services/disputa_emissao.py:1974`.

   **Laço exportada:** a aresta direta “S143 → E4B → reabrir credor exportado” fica cortada quando a consulta já enxerga a exportação. Isso não prova fechamento global do laço diante das cascatas e da janela concorrente do item 5; ciclo completo em ORM **[nao verificado]**. Referências: `app/chamados/services/disputa_emissao.py:1826`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/disputa_emissao.py:1914`.

2. **Quem escreve, quem lê e significado**

   | Campo/valor | Escritores e leitores relevantes |
   |---|---|
   | `respondida_em` / `resposta_colab` | Resposta humana, correção, resposta automática e recuperação pela trilha escrevem o par; E4B lê. C6, projeção `estado`, HX e histórico também leem. Referências: `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/services/disputa_emissao.py:1652`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/services/disputa_emissao.py:1828`, `app/chamados/juizes.py:1341`, `app/chamados/models.py:3045`, `app/chamados/services/historico.py:99`. |
   | Validação, materialização e via da pergunta | Validação e portas de carimbo/descarimbo escrevem; ressuscitação pode limpar validação, resposta e materialização. C6 e a enumeração E4B leem. Referências: `app/chamados/services/validacao.py:134`, `app/chamados/models.py:3103`, `app/chamados/models.py:3252`, `app/chamados/models.py:1566`, `app/chamados/juizes.py:1325`, `app/chamados/services/disputa_emissao.py:1789`. |
   | `chamado_destino` e pai da disputa | Abertura/reuso e reaponte escrevem destino; E4B lê a âncora por `chamado_que_lista`. Raiox possui outra regra de crédito, preexistente. Referências: `app/chamados/models.py:2982`, `app/chamados/models.py:3014`, `app/chamados/services/fio.py:925`, `app/chamados/services/fio.py:117`, `app/chamados/services/raiox.py:146`. |
   | Status, `resolvido_em`, via e urgência do credor | E4B delega a `reabrir()`→`_gravar_estado`; o núcleo limpa instante/via e restaura urgência. S143, recusa administrativa e crédito leem esses estados. Referências: `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/models.py:1442`, `app/chamados/models.py:248`, `app/chamados/signals.py:96`, `app/chamados/models.py:1342`, `app/chamados/services/raiox.py:146`. |
   | Fechamento da disputa | Ressuscitação pode chamar `reabrir_sistema`, que limpa fechamento/resolução e anota no pai. E4B filtra disputa aberta no evento; a trava lê também disputas da carga. Referências: `app/chamados/models.py:1574`, `app/chamados/models.py:2163`, `app/chamados/services/disputa_emissao.py:1908`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:202`. |
   | Lavra da pergunta | `_gravar_estado` relavra destinos não validados; `lavrar` escreve veredito, instante e expiração. `tarefa_do_dia` lê a lavra vigente. Referências: `app/chamados/models.py:309`, `app/chamados/veredito_lavrado.py:270`, `app/chamados/services/tarefa_do_dia.py:352`. |
   | Competência entregue | O escritor é `ExportacaoDominio.registrar`; invalidar altera vigência. E4B consulta a composição janela→K6, sem escrever TXT. Referências: `app/folha/models.py:174`, `app/folha/models.py:143`, `app/chamados/services/vigia_fio.py:822`, `app/folha/export.py:581`. |
   | `atos`, `pids`, `res` | São dados do planejamento, consumidos pela execução e impressão; não constituem novo estado de domínio persistido. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:85`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:121`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:284`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:353`. |

   **Protocolo dos motivos:** o escritor produz `credor_carrega_limbo:pk1,pk2[:credor_toca_exportada]` ou `credor_toca_exportada`; o retorno acrescenta `recusa:`. Dicionário e ordenação tornam os PKs únicos e crescentes. Referências: `app/chamados/services/disputa_emissao.py:1820`, `app/chamados/services/disputa_emissao.py:1824`, `app/chamados/services/disputa_emissao.py:1831`, `app/chamados/services/disputa_emissao.py:1851`.

   O backfill é o parser numérico nominal encontrado: separa o terceiro segmento por vírgula e aplica `int`. PKs fora de ordem são aceitos e a impressão normaliza com `sorted(set(...))`; `30,31` significa dois IDs. Vírgula inicial/final ou dupla produz elemento vazio e `ValueError`; no planejamento isso acontece fora do tratamento de erro por dia. O escritor atual não produz esse formato inválido. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:89`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:282`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:333`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:358`.

   S143 só reconhece prefixos; o backfill reconhece exportada também no sufixo combinado. Outros parsers externos à cópia: **[nao verificado]**. Referências: `app/chamados/services/disputa_emissao.py:1914`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:90`.

   **Notas:** `anexar_ocorrencia` persiste conteúdo/autoria e a chave; `tipo` e `objeto_ref` entram no hash, não são colunas próprias. A referência E4B contém pergunta, motivo completo e `resolvido_em`; a unicidade é, portanto, por esse fato, não uma nota global por credor. Histórico/fio leem genericamente as notas; SLA considera fala do DP, não a nota de sistema. Referências: `app/chamados/services/nota.py:62`, `app/chamados/services/nota.py:88`, `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/models.py:1698`, `app/chamados/services/historico.py:80`, `app/chamados/services/sla.py:21`.

   **Esmeril temporal:** `hoje` é uma data; `agora`, `respondida_em` e `resolvido_em` representam instantes. `--hoje` não altera o `agora` usado pela tarefa, logo não constitui replay integral do ato. `data_evento` possui a distinção preexistente: dia do turno nas perguntas factuais e dia civil nas conversas; C6 respeita o discriminador, enquanto a cerca nova consulta diretamente essa data para competência. Equivalência de jurisdição para todas as conversas: **[nao verificado]**. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:258`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:83`, `app/chamados/juizes.py:332`, `app/chamados/services/disputa_emissao.py:1827`.

3. **Predicados de limbo**

   E4B coincide com `carimbar_resposta_pela_trilha`: timestamp nulo e texto não vazio após `strip()`. A autoridade existente para vazio textual é `texto_vazio`. Referências: `app/chamados/services/disputa_emissao.py:1828`, `app/chamados/services/disputa_emissao.py:1670`, `app/chamados/juizes.py:1441`.

   **Não coincide exatamente com o comando histórico:** este exclui somente `resposta_colab=''`; espaços entram no conjunto medido, mas não na cerca E4B nem no carimbo pela trilha. Além disso, aquele comando exige os PKs fixos de `MEDIDAS`, não cura genericamente qualquer limbo novo. Referências: `app/chamados/management/commands/curar_perguntas_em_limbo.py:21`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:48`.

   Isso não demonstra juiz novo introduzido pela cura: a composição inline reutiliza `texto_vazio`, conforme o brief; a divergência está no seletor legado. A interpretação “voz testemunhada versus resto de questionário” permanece nas portas do corte, não na E4B. Referências: `app/chamados/juizes.py:1441`, `app/chamados/services/disputa_emissao.py:1829`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:9`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.

   A impressão é `LIMBO colNN DATA perguntas=PKS`; `limbo=N` conta **dias do plano**, não perguntas distintas. Quantidade real: **[nao verificado]**. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:357`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:365`.

4. **Idempotência**

   Para sucesso sequencial, a segunda passagem encontra credor vivo e sai antes da nota. Para recusa conhecida e fato estável, há uma nota Followup por chave e **zero nova `reabertura_recusada` em LogAuditoria**. Isso fecha o problema original de chamar repetidamente a porta recusada. Referências: `app/chamados/services/disputa_emissao.py:1818`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:109`.

   Consultar veto/irmão por `recusa_da_reabertura()` não executa seus efeitos. Entrar em `reabrir()` pode registrar veto, auditar admin/premissa ou migrar ao irmão. Se a recusa surgir somente nessa segunda consulta, E4B não desfaz esses efeitos; esse cenário não está coberto pelo selo de recusa estável. Referências: `app/chamados/models.py:1338`, `app/chamados/models.py:1354`, `app/chamados/models.py:1391`, `app/chamados/models.py:1394`, `app/chamados/models.py:1411`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`.

   Portanto, “2× = mesmo estado, uma trilha, zero duplicata” está sustentado pelos casos sequenciais dublados, **não certificado integralmente em PostgreSQL/cascatas concorrentes [nao verificado]**. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:516`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`.

5. **Universo, trava e reconferência**

   O universo implementado é `sem_porta` do vigia, somado ao complemento entregue/indeterminado em `[hoje−45, hoje)`; não há corte geral de 45 dias. A competência atual entra sempre na autoridade; a anterior depende de não entregue. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:74`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:164`, `app/chamados/services/vigia_fio.py:583`, `app/ponto/services/he_pendente_lavrado.py:220`.

   **A ordem explícita está correta:** C primeiro, depois D, Ch e P, cada conjunto por PK. **O universo antecipado é incompleto para a cascata:** expande perguntas somente das âncoras do lote; os demais chamados vivos são acrescentados sem expandir suas próprias cargas. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:190`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:206`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:2692`.

   **Contraexemplo em memória:** acrescentando outro chamado vivo `12`, com pergunta ressuscitável `90` fora do dia e disputa fechada `90`, a função real antecipou Ch12, mas omitiu P90/D90. O teste atual cobre carga da âncora principal, não essa composição. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:208`, `app/chamados/tests/test_e4b_classe_e_memoria.py:692`.

   **Há caminho de escrita para a carga omitida:** reabrir salva o credor; o receiver chama o cartório; este pode ressuscitar perguntas de outro chamado vivo do dia. Havendo adiados, também chama E2 para o colaborador inteiro, cujo seletor não recorta competência/data. Assim, a cerca direta não prova “exportada nunca escrita”, nem a antecipação prova a ordem de todas as aquisições posteriores. Caminho completo em ORM: **[nao verificado]**. Referências: `app/chamados/models.py:296`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:1602`, `app/ponto/services/cartorio.py:1633`, `app/ponto/services/cartorio.py:970`, `app/chamados/reconciliador.py:595`, `app/chamados/reconciliador.py:627`.

   **A reconferência existe**, inclusive do plano sob trava. Escritores canônicos de nascimento/reaponte compartilham C, protegendo contra essas mudanças concorrentes. Contudo, exportação não compartilha essa trava: K6 lê sem lock e `ExportacaoDominio.registrar()` faz `create()` sem C/D/Ch/P. Existe janela entre a última consulta e `reabrir()`. Ocorrência real da corrida: **[nao verificado]**. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:318`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:323`, `app/chamados/models.py:2845`, `app/chamados/services/fio.py:889`, `app/folha/export.py:581`, `app/folha/models.py:182`, `app/chamados/services/disputa_emissao.py:1840`.

   Há ainda um ramo defensivo incompleto: se a porta devolver recusa nova em `_executar_dia`, ele acumula a recusa e continua os demais atos. Em memória, recusa de limbo seguida de `perguntar` chamou a fábrica. Atingibilidade desse intervalo em ORM: **[nao verificado]**. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:131`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:139`.

6. **Relógio**

   Não identifiquei chamada nova a `now()`/`today()` nos hunks de produção; os testes novos usam `AGORA` fixo e o B2d tocado tem `freeze_time`. Referências: `app/chamados/services/disputa_emissao.py:1796`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:258`, `app/chamados/tests/test_e4b_classe_e_memoria.py:22`, `app/chamados/tests/test_b2d_resposta_visivel.py:79`.

   Permanecem relógios preexistentes fora desse repasse: C6 do evento pode chegar a `date.today()`; a autoridade de ressuscitação consulta C6 sem `hoje`; a lavra lê `timezone.now()`. Logo, P1 corrige a chamada nova, mas não transforma a cascata inteira em replay determinístico. Referências: `app/chamados/juizes.py:577`, `app/chamados/models.py:1509`, `app/chamados/veredito_lavrado.py:271`.

7. **REDs, mutantes e Ruff**

   Executei somente os selos em memória: **51 testes OK**, abrangendo `test_e4b_classe_e_memoria`, `test_e4_expand` e `test_e4_efeitos`; os **31 mutantes catalogados morreram por `AssertionError`**, com original verde e alvo único exigidos pelo harness. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:708`, `app/chamados/tests/test_e4_expand.py:127`, `app/chamados/tests/test_e4_efeitos.py:8`.

   Reconstruí somente em memória a pré-imagem de emissão: seu blob resultou em `d901e43c14a0d4dbe34a5e8e27e3fd8762b72fdc`, compatível com o índice antigo do patch fornecido. Os quatro REDs P1–P4 produziram **quatro falhas por assertion, zero errors**. A associação independente desse blob ao commit `345f15a87` permanece **[nao verificado]**, porque o runner depende de `git show` e esta cópia não contém repositório Git acessível. Referências dos asserts/runner: `app/chamados/tests/test_e4b_classe_e_memoria.py:493`, `app/chamados/tests/test_e4b_classe_e_memoria.py:545`, `app/chamados/tests/test_e4b_classe_e_memoria.py:558`, `app/chamados/tests/test_e4b_classe_e_memoria.py:615`, `app/chamados/tests/test_e4b_classe_e_memoria.py:736`.

   **Sobreviventes adicionais comprovados em memória:**

   - Trocar a referência da nota por somente `str(p.pk)` manteve os 36 testes verdes; perderia distinção entre motivos/encerramentos diferentes. O teste reinicia a fixture para cada motivo. Referências: `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`.
   - Emitir somente `limbo[:1]` manteve os 36 testes verdes; omite os demais PKs da nota e da lista. Os cenários atuais verificam um PK de limbo por vez. Referências: `app/chamados/services/disputa_emissao.py:1831`, `app/chamados/tests/test_e4b_classe_e_memoria.py:610`, `app/chamados/tests/test_e4b_classe_e_memoria.py:621`.
   - Inverter a ordenação da carga também sobreviveu; falta assert de formato com múltiplos PKs. Referências: `app/chamados/services/disputa_emissao.py:1824`, `app/chamados/tests/test_e4b_classe_e_memoria.py:621`.

   Os defaults ligados ao laço preservam os asserts apresentados no patch. Os quatro blobs atuais coincidiram com os índices novos fornecidos, inclusive o teste `9a2abe712…`; a reconstrução das nove correções individuais contra uma fonte anterior ao

[truncado em 20000 de 23299 caracteres]

## 11/11 01:26 · sessao 5118d86613c6 · - · - · origem=Ronald (tmux)

P3/P5 têm cobertura direta, mas não certificam toda a cascata de `reabrir()`: a enumeração nova cobre relavra S2 e ressuscitação do credor; o `save()` desse credor também dispara o cartório. Referências: `app/chamados/services/disputa_emissao.py:1788`, `app/chamados/models.py:296`, `app/chamados/signals.py:188`.

Achados:

- **A trava não cobre todas as linhas potencialmente tocadas pela cascata.** `_travar_lote` expande somente as âncoras das perguntas do dia; acrescenta os demais chamados vivos sem expandir suas perguntas/disputas. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:197`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:206`.
  
  Em ensaio somente em memória, acrescentei outro chamado vivo `12`, da célula do lote, com pergunta ressuscitável `90` fora do dia, em disputa fechada `90`. A função real produziu:
  ```
  [call([7]),
   call([7], disputas=[20], chamados=[10, 11, 12], perguntas=[30])]
  ```
  Portanto, incluiu o chamado e omitiu sua pergunta/disputa. O ensaio utiliza os doubles existentes e executa a função real; o caso completo em ORM permanece [nao verificado]. Referências da fixture e da enumeração: `app/chamados/tests/test_e4b_classe_e_memoria.py:164`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:197`.

- **Há caminhos concretos no código para essa carga adicional ser tocada.** O cartório agrupa chamados dos `FURO_MODULES` pelo dia de `data_do_chamado`, seleciona os vivos e pode chamar `ressuscitar_perguntas()` de outro chamado vivo. Referências: `app/ponto/services/cartorio.py:117`, `app/ponto/services/cartorio.py:125`, `app/ponto/services/cartorio.py:1316`, `app/ponto/services/cartorio.py:1345`, `app/ponto/services/cartorio.py:1602`, `app/ponto/services/cartorio.py:1633`.

  Uma pergunta validada é morta para o fio; a autoridade de ressuscitação avalia uma cópia sem validação/carimbo e pode devolvê-la viva. Assim, o teste de fio morto não elimina esse caminho. Referências: `app/chamados/juizes.py:1325`, `app/chamados/juizes.py:1434`, `app/chamados/models.py:1506`, `app/chamados/models.py:1509`.

  Além disso, havendo adiados, o cartório chama E2 para **o colaborador inteiro**; E2 seleciona perguntas sem recorte de data/competência, carimba e pode fechar disputas. Referências: `app/ponto/services/cartorio.py:962`, `app/ponto/services/cartorio.py:970`, `app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:595`, `app/chamados/reconciliador.py:608`, `app/chamados/reconciliador.py:627`.

  Logo, a lista explicitamente ordenada C→D→Ch→P não prova a ordem de todas as aquisições posteriores nem “exportada nunca escrita” na cascata completa. Demonstração integral concorrente/ORM: [nao verificado].

- **Existe janela entre conferir exportação e escrever.** A última leitura da cerca ocorre antes de `credor.reabrir()`; K6 consulta `ExportacaoDominio` sem trava. O escritor `ExportacaoDominio.registrar()` faz `exists()` e `create()` sem adquirir C/D/Ch/P. Referências: `app/chamados/services/disputa_emissao.py:1826`, `app/chamados/services/disputa_emissao.py:1840`, `app/folha/export.py:580`, `app/folha/models.py:174`, `app/folha/models.py:182`.

  Portanto, a trava do colaborador bloqueia os escritores canônicos concorrentes de pergunta/reaponte, mas não serializa a exportação. Referências: `app/chamados/models.py:2845`, `app/chamados/services/fio.py:889`, `app/folha/models.py:182`. Ocorrência real dessa corrida: [nao verificado].

- **A reconferência existe:** o backfill relê competência e plano sob a trava, e a porta volta a enumerar a carga. Isso fecha mudanças anteriores a essas leituras, não as cascatas posteriores descritas acima. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:318`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:323`, `app/chamados/services/disputa_emissao.py:1820`.

- **O teste P5 atual não cobre o contraexemplo:** exercita a carga da âncora principal e histórico terminal alheio, sem outro chamado vivo com carga própria. Referência: `app/chamados/tests/test_e4b_classe_e_memoria.py:692`.

O universo declarado corresponde à composição implementada: `sem_porta` do vigia, mais entregue/indeterminada em `[hoje−45, hoje)`; não existe corte geral de 45 dias. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:8`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:74`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:164`, `app/chamados/services/vigia_fio.py:583`.

**NAO LIBERA P3/P5**, pela fronteira de exportação sem serialização e pela carga indireta não abrangida pela enumeração/trava; ocorrências em produção e execução completa das cascatas em ORM permanecem [nao verificado].

## 11/11 01:25 · sessao 985f431d56cb · - · - · origem=Ronald (tmux)

P6 melhora, mas deixa lacunas:

- Executei o comando em memória dos três módulos: **51 testes OK**, com os **31 mutantes mortos por `AssertionError`**. O harness exige original verde, alvo único e mensagem esperada. `app/chamados/tests/test_e4b_classe_e_memoria.py:708`.
- Os dois sobreviventes antigos agora morrem: C7 tem rodada com `trancado=False`; pai adicional é proibido por estado e chamada. `app/chamados/tests/test_e4b_classe_e_memoria.py:319`, `app/chamados/tests/test_e4b_classe_e_memoria.py:243`.
- **Mutante adicional sobrevive:** substituir `objeto_ref` por somente `str(p.pk)` manteve os 36 testes verdes, inclusive o catálogo. Isso perderia notas de novas recusas/novos fechamentos. Os testes reiniciam a fixture para cada motivo e não verificam sua mudança preservando o histórico. `app/chamados/services/disputa_emissao.py:1850`, `app/chamados/tests/test_e4b_classe_e_memoria.py:538`.
- **Outro sobrevivente:** inverter `sorted(carga)` para ordem decrescente manteve os 36 testes verdes. Não há caso com múltiplas PKs de limbo exigindo formato e ordem estáveis. `app/chamados/services/disputa_emissao.py:1824`, `app/chamados/tests/test_e4b_classe_e_memoria.py:621`.
- O `--volta1` falhou antes dos testes porque não há repositório Git acessível; a mordida contra o objeto exato `345f15a87` permanece **[nao verificado]**. O runner depende diretamente de `git show`. `app/chamados/tests/test_e4b_classe_e_memoria.py:736`.
- Relógio dos testes novos é fixo; B2d tocado está congelado. `app/chamados/tests/test_e4b_classe_e_memoria.py:22`, `app/chamados/tests/test_e4b_classe_e_memoria.py:396`, `app/chamados/tests/test_b2d_resposta_visivel.py:79`.
- P1 não torna a cascata inteira reproduzível: enumeração chama o C6 sem `hoje`; lavra lê `timezone.now()` existente. `app/chamados/services/disputa_emissao.py:1788`, `app/chamados/models.py:1509`, `app/chamados/veredito_lavrado.py:271`.
- Os defaults vinculados ao laço preservam os asserts apresentados; a autenticação das nove alterações individuais de Ruff exige a fonte anterior separada **[nao verificado]**. `app/chamados/tests/test_e4b_classe_e_memoria.py:556`, `app/chamados/tests/test_e4b_classe_e_memoria.py:638`, `app/chamados/tests/test_e4b_classe_e_memoria.py:644`.
- **Ramo concreto da recusa nova:** `_executar_dia` registra recusa e continua os demais atos. Reproduzi em memória `recusa:credor_carrega_limbo:31` seguida de uma chamada à fábrica. Atingibilidade concorrente em ORM: **[nao verificado]**. `app/chamados/management/commands/backfill_fio_morto_e4b.py:131`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:139`.

ORM, suíte inteira, banco/log, efeitos reais e concorrência: **[nao verificado]**. [nao verificado]

_3600 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
