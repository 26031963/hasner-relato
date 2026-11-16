# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 13:20** · publicado em **16/11/2026 13:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 13:20 · sessao db8b5d6bc7f1 · - · - · origem=Ronald (tmux)

- **“Salvar é a única função escritora” — NÃO PROCEDE.** `salvar_calendario_de_folga` faz `update_or_create` das grades e as apaga ao trocar para `semana_dia`; `criar_calendario_de_folga` também cria as grades. A porta canônica declarada é o **módulo** `escala/folgas.py`, não exclusivamente a função salvar (`app/escala/folgas.py:588`, `app/escala/folgas.py:594`, `app/escala/folgas.py:645`, `app/escala/folgas.py:659`, `app/core/portas.py:473`).

- **Criador rápido — PROCEDE; corte no servidor.** `wizard_calendario_criar` recebe dias, valida `1..31` e chama `criar_calendario_de_folga`; esta percorre `(31,30,28)` e grava somente `d <= tam` (`app/escala/views_wizard.py:140`, `app/escala/views_wizard.py:179`, `app/escala/views_wizard.py:188`, `app/escala/views_wizard.py:192`, `app/escala/folgas.py:658`, `app/escala/folgas.py:661`).

- **Fonte — PROCEDE com precisão:** cada registro de `FolgaCalendarioTamanho` guarda uma grade; o calendário pode ter registros de 28,30,31, exclusivos por calendário/tamanho; não existe exigência de que os três estejam presentes ou preenchidos (`app/escala/models.py:1563`, `app/escala/models.py:1565`, `app/escala/models.py:1570`, `app/escala/models.py:1575`). `dias_por_tamanho()` lê as linhas existentes, sem completar grades ausentes (`app/escala/models.py:1558`).

- **Contexto e POST — PROCEDE.** A view define 31,30,28, lê os marcados, monta os três elementos e entrega o contexto ao editor (`app/escala/views.py:318`, `app/escala/views.py:362`, `app/escala/views.py:369`). No POST `dias_mes`, percorre os três tamanhos, transforma campos ausentes em listas vazias, valida o intervalo e passa o dicionário completo à porta (`app/escala/views.py:340`, `app/escala/views.py:342`, `app/escala/views.py:349`, `app/escala/views.py:356`).

- **Censo de leitores diretos:** `dias_por_tamanho()` é consumido em produção por `datas_que_o_calendario_manda`, que delega a `folgas_do_mes`; este escolhe o tamanho real, usando 28 para fevereiro bissexto, e devolve vazio quando a grade falta/está vazia (`app/escala/folgas.py:56`, `app/escala/folgas.py:12`, `app/escala/folgas.py:14`). A função de datas alimenta divergência, vigia, geração de foto e sincronização futura (`app/escala/folgas.py:99`, `app/escala/folgas.py:127`, `app/escala/folgas.py:158`, `app/escala/folgas.py:629`). Lista e editor também leem diretamente `cal.tamanhos` (`app/escala/views.py:292`, `app/escala/views.py:362`).

- **RED do POST: a obrigação já existe no código e em teste.** `test_post_cria_calendario_e_3_tamanhos` envia valores distintos nos três campos e confere o dicionário persistido; criar e editar usam a mesma view (`app/escala/tests/test_folga_cadastro.py:18`, `app/escala/tests/test_folga_cadastro.py:30`, `app/escala/urls.py:35`, `app/escala/urls.py:36`). O teste de porta também confere as três grades e idempotência (`app/escala/tests/test_porta_escala.py:95`). Logo um teste apenas dessa condição **não evidencia RED da mudança visual**, por inspeção desses caminhos; resultado de execução é [nao verificado]. Para testemunhar “pela porta”, uma verificação adicional precisaria observar a chamada da função real sem substituir a persistência; seria reforço de contrato, não o RED visual (`app/escala/views.py:354`, `app/escala/views.py:356`, `app/escala/folgas.py:594`).

- **Outros escritores em testes, separados de produção:** preparações com `objects.create` em `app/colaboradores/tests/test_vincular_folga_view.py:31`, `app/colaboradores/tests/test_vincular_folga_view.py:35`, `app/colaboradores/tests/test_vinculo_folga_padrao.py:25`, `app/escala/tests/test_folga_semana_dia.py:65`, `app/escala/tests/test_torneira_folgas.py:99`, `app/escala/tests/test_folga_models.py:11`, `app/escala/tests/test_gerador_folgas.py:32`, `app/escala/tests/test_gerador_folgas.py:34`, `app/escala/tests/test_smoke_clique_folga_calendario.py:112`, `app/escala/tests/test_editor_folga_modo.py:29`, `app/escala/tests/test_folga_aplicar.py:37`, `app/escala/tests/test_foto_folga_continua.py:64`, `app/escala/tests/test_selo_foto_folgas_relavra.py:46`; alteração por instância em `app/escala/tests/test_foto_folga_continua.py:145` e `app/escala/tests/test_foto_folga_continua.py:146`. A migração encontrada cria o esquema, sem carga de dados (`app/escala/migrations/0015_folgacalendario_folgacalendariotamanho.py:29`). A guarda de arquitetura proíbe escritores de produção fora da porta, com allowlist vazia (`app/escala/tests/test_chokepoint_escala_gate.py:27`, `app/escala/tests/test_chokepoint_escala_gate.py:55`).

## 16/11 13:20 · sessao 099ae93ca4b9 · - · - · origem=Ronald (tmux)

- **PROCEDE — item 1 alcança o editor previsto na O148.** O complemento histórico nomeia `editar_folga_calendario` como “EDITAR-NAO-PUBLICA”; O148-EDITAR contempla edição com uso por versão. Referências: `app/docs/O148-COMPLEMENTO.md:15`, `app/docs/CORTES.md:357`.

- **NAO PROCEDE — afirmar que versionamento já seja lei vigente ou que exista reserva de arquivo em obra ativa.** Os registros dizem “só registrar”, “não constrói agora” e “edição por versão espera corte de lei”. O148-EDITAR aparece na seção “CORTES que são ATO, não lei”. Referências: `app/docs/CORTES.md:101`, `app/docs/CORTES.md:357`, `app/docs/BACKLOG.md:819`, `app/docs/LEIS.md:206`, `app/docs/LEIS.md:313`. Concorrência efetiva no mesmo arquivo: **[nao verificado]**.

- **NAO PROCEDE — conflito necessário com leis de UI.** O aviso pode seguir componente da casa, sem diálogo nativo, e consumir o mesmo estado usado pela ação; essas são as exigências aplicáveis. Referências: `app/docs/LEIS-UI.md:23`, `app/docs/LEIS-UI.md:77`, `app/docs/LEIS-UI.md:87`. A raia normalmente permite templates/static/testes; a exceção nominal da view consta apenas no anexo fornecido **[nao verificado na cópia]**; regra geral: `CLAUDE.md:312`.

- **Achado material — a porta declarada não é o único escritor.** `criar_calendario_de_folga` também cria `FolgaCalendarioTamanho`; o descarte acima do tamanho ocorre no servidor. Referências: `app/escala/folgas.py:645`, `app/escala/folgas.py:658`, `app/escala/folgas.py:661`.

- **Achado material — salvar já produz efeitos além da definição das grades.** A porta sincroniza fotos futuras dos vínculos e chama regeneração futura pela porta da célula. A descrição histórica “grava e redireciona sem rematerializar” está superada nesse ponto. Referências: `app/escala/folgas.py:600`, `app/escala/folgas.py:639`, `app/escala/folgas.py:640`; descrição histórica: `app/docs/O148-COMPLEMENTO.md:15`.

## 16/11 12:57 · sessao 24f76b5f4000 · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

1) **NAO PROCEDE a objeção às premissas.**

- A fonte existe, recebe `(cal, ano, mes)` e concentra a escolha entre os dois modos; os resolvedores ficam abaixo dela (`app/escala/folgas.py:43`, `app/escala/folgas.py:54`). Há selo contra chamadas dos resolvedores fora desse módulo (`app/escala/tests/test_foto_folga_continua.py:281`).
- Leitores diretos encontrados: `divergencia_foto_x_calendario` (`app/escala/folgas.py:99`), `calendario_com_template_vazio_no_mes` (`app/escala/folgas.py:127`), `gerar_foto_folgas` (`app/escala/folgas.py:158`) e `sincronizar_foto_futura_do_calendario` (`app/escala/folgas.py:629`).
- A rota recebe `cid` inteiro e a view busca `FolgaCalendario.pk=cid` (`app/escala/urls.py:36`, `app/escala/views.py:319`).
- O bloco e a caixa existem. O texto vazio é **“— usar as folgas da escala (padrão do template) —”**, conforme a descrição do aval; não é literalmente “sem calendario” (`app/templates/colaboradores/partials/painel_vinculo.html:186`, `app/templates/colaboradores/partials/painel_vinculo.html:197`).
- `painel_vinculo` escolhe o primeiro vínculo ativo, fornece os calendários e renderiza o partial. A opção selecionada compara o calendário com **`escala_ativa.folga_calendario_id`**, não com o padrão do TipoEscala (`app/colaboradores/views.py:268`, `app/colaboradores/views.py:295`, `app/colaboradores/views.py:297`, `app/templates/colaboradores/partials/painel_vinculo.html:199`).

2) **NAO PROCEDE colisão no escopo declarado.**

- Ler essa fonte atende à testemunha que não recalcula e ao juiz único da tela (`app/docs/LEIS.md:60`, `app/docs/LEIS-UI.md:23`).
- O grep encontrou referências a outro sítio de `colaboradores/views.py`, a `reparentar_folga` e à porta de troca do calendário; nenhuma exige alteração nesta projeção (`app/docs/LEIS.md:64`, `app/docs/CORTES.md:349`, `app/docs/LEIS.md:189`). Outra referência específica impeditiva: `[nao verificado]`.
- O148-EDITAR permanece **“SO REGISTRAR”**, com edição por versão aguardando corte; abrir a edição existente não implementa esse complemento (`app/docs/CORTES.md:356`, `app/escala/views.py:369`).
- A exceção nominal do anexo cobre o contexto `.py` diante da restrição geral da raia UI (`CLAUDE.md:312`); registro dessa exceção em arquivo: `[nao verificado]`.
- Comparação com a lista BUG-PAR fornecida: nenhum arquivo em comum com os dois alvos declarados; confirmação externa dos arquivos efetivamente em voo: `[nao verificado]`.

3) **NAO PROCEDE impedimento literal; há cuidados obrigatórios de implementação.**

- A fonte aceita cada mês separadamente, sem restringir corrente/próximo; a decisão dos dias permanece integralmente nela (`app/escala/folgas.py:43`, `app/escala/folgas.py:55`).
- **Somente contexto inicial deixaria a projeção velha ao trocar a caixa:** não há atualização no select, o POST usa `hx-swap="none"` e retorna apenas toast (`app/templates/colaboradores/partials/painel_vinculo.html:193`, `app/templates/colaboradores/partials/painel_vinculo.html:195`, `app/colaboradores/views.py:251`). Isso cabe resolver nos alvos autorizados, projetando pela fonte os calendários já fornecidos e alternando sua apresentação no partial (`app/colaboradores/views.py:295`, `app/escala/folgas.py:43`).
- O RED pode comparar HTML com a fonte real; há precedente de GET e inspeção do painel (`app/colaboradores/tests/test_agenda_14dias.py:39`, `app/colaboradores/tests/test_agenda_14dias.py:44`). Para morder derivação paralela, deve também substituir o retorno da fonte por datas sentinela e exigir sua exibição. Execução desse RED/mutante: `[nao verificado]`.
- Ajuda por componente e ausência de diálogo nativo continuam exigíveis (`app/docs/LEIS-UI.md:77`, `app/docs/LEIS-UI.md:87`).

4) **NAO PROCEDE alcance adicional necessário.**

A projeção chama um leitor que devolve datas; abrir a edição por GET não entra no ramo de escrita (`app/escala/folgas.py:52`, `app/escala/views.py:320`, `app/escala/views.py:362`). Não exige alterar as portas que trocam vínculo ou materializam folgas (`app/colaboradores/views.py:239`, `app/colaboradores/views.py:245`). O `!` protege mudança do **dado** de escala/vínculo e do gravado de exportada (`CLAUDE.md:265`). Patch, migração, efeitos financeiros e cumprimento posterior desses limites: `[nao verificado]`.

VEREDITO: EXECUTA

## 16/11 12:55 · sessao 01ed894fced5 · - · - · origem=Ronald (tmux)

Não identifiquei impedimento intrínseco no literal; há uma armadilha de atualização que deve entrar no RED.

- A fonte aceita `(cal, ano, mes)` e retorna `set[date]`; resolve os modos internamente e aceita calendário ausente. Pode ser chamada duas vezes, inclusive na virada de ano, sem derivar folgas fora dela (`app/escala/folgas.py:43`, `app/escala/folgas.py:52`, `app/escala/folgas.py:54`).
- A seleção inicial vem de `escala_ativa.folga_calendario_id`; o vazio já tem o texto “usar as folgas da escala (padrão do template)”. A expressão literal “sem calendario” está na resposta do escritor, não nessa opção (`app/templates/colaboradores/partials/painel_vinculo.html:197`, `app/templates/colaboradores/partials/painel_vinculo.html:199`, `app/colaboradores/views.py:250`).
- Uma projeção estática ficaria velha ao mudar/salvar: o select não tem atualização e o formulário usa `hx-swap="none"`; o POST HTMX devolve corpo vazio e toast. O carregamento existente só se repete automaticamente após swap da linha (`app/templates/colaboradores/partials/painel_vinculo.html:193`, `app/templates/colaboradores/partials/painel_vinculo.html:195`, `app/colaboradores/views.py:251`, `app/templates/colaboradores/vincular.html:165`).
- Isso pode ser resolvido nos locais autorizados: a view já disponibiliza todos os calendários; o contexto pode acrescentar os dois retornos da fonte por calendário, e o partial alternar a projeção e o link conforme a seleção, sem calcular folgas. Viabilidade por leitura: `app/colaboradores/views.py:295`, `app/colaboradores/views.py:298`, `app/escala/folgas.py:43`; implementação e desempenho `[nao verificado]`.
- Para o mutante, apenas comparar datas reais não prova dependência da fonte. Proposta: substituir temporariamente o retorno da fonte por datas sentinela durante o GET e exigir essas datas no HTML e chamadas para os dois meses; execução desse RED `[nao verificado]`. O GET do painel já possui precedente de teste com `Client` e inspeção do HTML/contexto (`app/colaboradores/tests/test_agenda_14dias.py:39`, `app/colaboradores/tests/test_agenda_14dias.py:44`).

## 16/11 12:55 · sessao a627924a613d · - · - · origem=Ronald (tmux)

- **Pergunta 2: NAO PROCEDE, no escopo literal do item 1.** Ler a fonte única segue L-002 (`app/docs/LEIS.md:60`) e L2 de UI (`app/docs/LEIS-UI.md:23`). A regra geral de `wt-ui` limita a templates/static/teste (`CLAUDE.md:309`–313); a exceção nominal para contexto na view consta somente do anexo fornecido `[nao verificado em arquivo]`.

- Busca pelos arquivos/funções em `LEIS.md`/`CORTES.md`: `colaboradores/views.py` aparece por **form_empresa**, outro sítio (`app/docs/LEIS.md:64`); `escala/folgas.py` por **reparentar_folga**, outro predicado (`app/docs/CORTES.md:349`). Não encontrei referências exatas a `painel_vinculo`, `editar_folga_calendario`, `escala/urls.py` ou `vincular_folga` nesses dois documentos `[nao verificado: ausência não possui linha positiva]`.

- **O148-EDITAR não exige implementar versões nesta obra.** Está expressamente em “SO REGISTRAR”, com edição por versão aguardando corte (`app/docs/CORTES.md:356`). Item 1 pode apenas apontar à edição existente: GET busca o calendário e renderiza (`app/escala/views.py:319`, `app/escala/views.py:362`, `app/escala/views.py:369`); a escrita fica no ramo POST (`app/escala/views.py:320`, `app/escala/views.py:356`).

- **Pergunta 4: NAO PROCEDE se restrita a exibição/contexto/link.** `!` protege alteração de **dado** de escala/vínculo e gravado de exportada (`CLAUDE.md:265`–268); L-007 protege escritores e trilha (`app/docs/LEIS.md:65`, `app/docs/LEIS.md:189`). O resolver devolve datas sem persistir (`app/escala/folgas.py:43`–56).

- **Ponto para pergunta 3:** a caixa atual não dispara requisição ao mudar; `selected` compara o calendário **salvo** do vínculo (`app/templates/colaboradores/partials/painel_vinculo.html:195`–201). Apenas contexto inicial acompanha esse calendário salvo; acompanhar seleção ainda não salva exigiria mecanismo adicional `[nao verificado: intenção exata do anexo]`.

- O texto existente para ausência é exatamente “— usar as folgas da escala (padrão do template) —”, não a expressão “sem calendário” (`app/templates/colaboradores/partials/painel_vinculo.html:197`).

## 16/11 12:48 · sessao 680469d3a22e · O10-D-PERMUTA-RECIPROCA · censo · origem=Code

A conferência preserva o contrato do construtor: provocar rollback ao receber `'recusada'`, ordenar as quatro células e permitir `d1=d2`. A correção relevante é retirar a suposição do censo de que as datas precisam ser distintas. (`app/docs/RELATO.md:1640`; `app/docs/BACKLOG.md:2177`)

Os anexos abaixo são referenciados pela numeração do **texto colado**, contando linhas vazias e começando após o delimitador de cada anexo. Distingo as matrizes homônimas como `matriz_F2.md` e `matriz_F4.md`. Identidade textual com o commit histórico e vínculo Git da cópia ao hash informado: [nao verificado]. [nao verificado]

**1. Conferência do censo**

Para encurtar a tabela, `P` significa `app/ponto/portas/celula.py` e `G` significa `app/ponto/management/commands/gerar_celulas.py`. “MOVEU” significa que localizei o comportamento descrito em outra linha; comparação textual com a versão histórica: [nao verificado].

| Afirmação | Linha no censo e referências antigas | Arquivo:linha hoje | Resultado |
|---|---|---|---|
| A porta captura a recusa; duas chamadas dentro de um `atomic` externo não garantem rollback integral | `censo_v2.r:1`; P:652,694 | P:690,732,737 | **MOVEU**; comportamento preservado. |
| Cartas travadas por `pk`, exclusivamente em `Ausencia`; células ordenadas por colaborador/data e travadas dentro de `editar_dia` | `censo_v2.r:5`; P:657,673,593 | P:695,711,631 | **MOVEU**. |
| Ordenação é por chamada, não pelas quatro células; composição pode produzir espera circular | `censo_v2.r:7`; P:657,673 | P:695,711 | **MOVEU**; permanece dedução estática. Deadlock ocorrido: [nao verificado]. |
| Teste citado inspeciona AST, sem executar concorrência | `censo_v2.r:9`; teste:314 | `app/ponto/tests/test_o10b_editar_dia_troca.py:314`, :319, :322 | **IGUAL**. Ensaio concorrente da composição: [nao verificado]. |
| Cada edição consulta empresa/lavra antes de decidir `'igual'`; existe bypass `apesar_da_lavra` | `censo_v2.r:11`; P:513,568,594,676 | P:551,606,632,714 | **MOVEU**. |
| Não existe pré-validação das quatro; consulta seguinte acontece depois da edição anterior | `censo_v2.r:13`; P:675 | P:713 | **MOVEU**. |
| Consulta da exportação não trava seus registros; TOCTOU documentado | `censo_v2.r:13`; G:161; BACKLOG:1876 | G:170; `app/docs/BACKLOG.md:2176` | **MOVEU**. |
| Carimbo individual por edição; quatro trilhas individuais e duas do par, quando quatro alterações acontecem | `censo_v2.r:15`; P:598,611,686 | P:636,649,724 | **MOVEU**. |
| Deduplicação do recálculo é local; quatro alterações podem agendar quatro recálculos | `censo_v2.r:19`; P:450,458,627; fechamento:1824 | P:488,496,665; `app/ponto/services/fechamento.py:1824` | **MOVEU** na porta; **IGUAL** em fechamento. |
| Callback por alteração contém rejunção e reavaliação; rejunção exige FK nula; reavaliação recorta vivos/dias | `censo_v2.r:20`; P:490,502; demais:60,17,76 | P:528,540; `app/chamados/rejuncao.py:60`, :62; `app/chamados/services/feriado_reavaliar.py:17`, :76 | **MOVEU** na porta; demais sítios **IGUAIS**. |
| Rejulgamento síncrono pode observar composição incompleta; pula futuro/humano e captura falhas | `censo_v2.r:21`; P:427,434,437,627 | P:465,472,475,665 | **MOVEU**. |
| Marca órfã ocorre na transação, consulta carimbos e atualiza contexto | `censo_v2.r:22`; P:625; G:241,255 | P:663,664; G:250,264 | **MOVEU**. |
| Segunda chamada relê cartas/colaboradores; edição relê célula e vínculo | `censo_v2.r:26`; P:656,657,593; alimentação:355 | P:694,695,631; `app/escala/alimentacao.py:355`, :358 | **MOVEU** na porta; alimentação **IGUAL**. |
| Cartório relê célula; identity map é limitado às instâncias recebidas e restaurado ao sair | `censo_v2.r:28`; cartório:1976; alimentação:43,47 | `app/ponto/services/cartorio.py:2027`; `app/escala/alimentacao.py:43`, :47 | Cartório **MOVEU**; mapa **IGUAL**. |
| Um lado `'igual'` não impede o outro; só retorna `'igual'` quando ambos iguais; lado igual não escreve seus efeitos | `censo_v2.r:30`; P:594,596,673,679,690 | P:632,634,711,717,728 | **MOVEU**. |
| Igualdade é do estado da célula; não considera novo pedido, carta, motivo ou autor | `censo_v2.r:32`; P:594,675 | P:632,713 | **MOVEU**. |
| Segunda recusa, seguida de saída normal do externo, deixa a primeira elegível ao commit | `censo_v2.r:36`; P:652,694 | P:690,732,737 | **MOVEU**; dedução da captura fora do `atomic` interno. | [nao verificado]
| Rollback externo alcança escritas transacionais e auditoria; callbacks dependem do commit externo | `censo_v2.r:37`; observ:133; P:625,434,502; fechamento:1802 | `app/core/observ.py:133`; P:663,472,540; `app/ponto/services/fechamento.py:1802`, :1824 | Porta **MOVEU**; demais sítios **IGUAIS**. |
| Rejulgamento pode alcançar abertura de cobrança e push síncrono, que rollback SQL não recolhe | `censo_v2.r:39`; cartório:1740; detector:778; push:40 | `app/ponto/services/cartorio.py:1791`; `app/ponto/management/commands/detectar_ausencias.py:778`, :779; `app/chamados/push_utils.py:40` | Cartório **MOVEU**; demais **IGUAIS**. Envio concreto: [nao verificado]. |
| Helper de disponibilidade declara não validar 11h | `censo_v2.r:45`; utils:152 | `app/ponto/utils.py:152`, :153 | **IGUAL**. Existência de outro validador completo pronto: [nao verificado]. |
| `marcos_turno` e `fim_turno` fornecem insumos temporais, incluindo meia-noite | `censo_v2.r:46`; jornada:61,89 | `app/escala/servico_jornada.py:69`, :88, :97 | **MOVEU**. Juiz completo da colisão: [nao verificado]. |
| Permissão por ação e por departamento do chamado existe separadamente | `censo_v2.r:47`; mixins:322; utils:68 | `app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68` | **IGUAL**. Ação específica da permuta e composição contextual das quatro: [nao verificado]. |
| Lavra já possui juiz separado K9; helpers enumeram competências e produzem motivo | `censo_v2.r:48`; export:589; juízes:322; G:126,208 | `app/folha/export.py:597`; `app/core/juizes.py:353`; G:135,217 | **MOVEU**. |
| Portas não chamam permissão, 11h ou colisão; lavra já está separada | `censo_v2.r:50`; P:559,568,646,675 | P:597,606,684,713 | **MOVEU**. |
| `ausencias_sobrepostas` responde sobre períodos de `Ausencia` | `censo_v2.r:52`; turnos:2795 | `app/ponto/turnos.py:2817`, :2839 | **MOVEU**; não é veredito de colisão horária entre plantões. |
| `evento` aceita antes/depois; auditoria tem objeto inteiro e JSONs, sem coluna própria de correlação | `censo_v2.r:56`; observ:94,133; models:298,300 | `app/core/observ.py:94`, :133; `app/core/models.py:298`, :300, :301 | **IGUAL**. |
| `evento_kw` encaminha somente os campos conhecidos | `censo_v2.r:58`; observ:142 | `app/core/observ.py:142`, :146 | **IGUAL**; argumento adicional isolado não vira metadado persistido. |
| Precedente `lote='arquivar_lote'` é rótulo fixo, incorporado ao JSON | `censo_v2.r:60`; views:146; arquivo:133 | `app/chamados/views_arquivar.py:146`; `app/chamados/services/arquivo.py:133`, :134 | **IGUAL**. Identificador exclusivo de permuta já existente: [nao verificado]. |
| `trocar_plantao` não recebe ID de permuta; audita carta e dois lados; FK de origem não é copiada | `censo_v2.r:62`; P:632,682,686; models:786 | P:670,720,724,728; `app/ponto/models.py:786` | Porta **MOVEU**; FK **IGUAL**. |
| Para quatro coordenadas distintas, pessoas/datas vêm das quatro cartas relidas | `censo_v2.r:66`; P:659,673 | P:697,711,713 | **MOVEU**; afirmação permanece condicional a coordenadas distintas. |
| Exige cartas presentes/encontradas, colaboradores distintos e documento preenchido; não compara nem abre arquivos | `censo_v2.r:73`; P:653,660,662,664,684 | P:691,698,700,702,722 | **MOVEU**. |
| Não existe consumo/exclusividade global da carta, nem exigência de datas iguais dentro de uma chamada | `censo_v2.r:75`; P:656,673 | P:694,711,713 | **MOVEU**. |
| Tipo/status/período/pedido não participam da elegibilidade documental atual; lavra/vínculo são separados | `censo_v2.r:77`; P:664,675,568,584 | P:702,713,606,622 | **MOVEU**. |
| “BACKLOG:1879 trata BO-0511-G2-TRIAGEM” | `censo_v2.r:79` | `app/docs/BACKLOG.md:1879`, :2181; obra em :2177 | **MUDOU** o endereço físico: :1879 trata H12; BO está em :2181; O10-D em :2177. |
| Quatro células “exigem duas datas distintas” | `censo_v2.r:81` | `app/docs/RELATO.md:1640`; `app/docs/PROMPTS.md:2015` | **MUDOU**: lei permite `d1=d2`. |
| Universo das 11h permaneceu pergunta aberta | `censo_v2.r:85` | `aval_lei_permuta.txt:1`; `app/docs/RELATO.md:1640` | **MUDOU**: previsto anterior → assumido → seguinte, para ambos, após permuta. |
| Empresa/vínculo/tipos de escala permaneceram pergunta aberta | `censo_v2.r:86`; P:584,673 | P:622,711; `aval_lei_permuta.txt:1` | Código **MOVEU**; pergunta **RESPONDIDA**: mesma empresa, ativos/vigentes nas duas datas; escalas diferentes elegíveis sob as guardas. |
| Elegibilidade das cartas permaneceu pergunta aberta | `censo_v2.r:87`; P:664 | P:702; `aval_lei_permuta.txt:1` | Código **MOVEU**; pergunta **RESPONDIDA**: ato permuta aprovado, dia exato, ligado ao ID. |
| `folga_trabalhada` permaneceu pergunta aberta para a permuta | `censo_v2.r:88`; RELATO:449; BACKLOG:1876 | `app/docs/RELATO.md:2120`, :1640; `app/docs/BACKLOG.md:2176`; `aval_lei_permuta.txt:1` | Registro histórico **MOVEU**; conclusão **MUDOU**: permuta formal é compensação, com guarda de DSR. |

Nenhum dos comportamentos de código citados foi classificado como **SUMIU**; as mudanças substantivas encontradas são documentais e de interpretação da lei. (`app/ponto/portas/celula.py:670`; `app/docs/RELATO.md:1640`)

Sobre o contrato:

- **Rollback permanece obrigatório no compositor.** A recusa de negócio é capturada e devolvida como `'recusada'`; erros de autor/motivo e falha da trilha ainda podem propagar exceções. (`app/ponto/portas/celula.py:684`; :686; :730; :732; :737)
- **Ordenar as quatro permanece necessário.** A porta ordena somente os dois lados da chamada e trava cartas antes das células; o desenho da composição precisa considerar ambas as aquisições. (`app/ponto/portas/celula.py:695`; :711; `app/docs/RELATO.md:1640`)
- **`d1=d2` precisa ser aceito.** Nesse caso, duas chamadas inversas revisitam as mesmas duas coordenadas; `editar_dia` recebe um booleano e monta DNA pelo vínculo individual. Representar a troca dos turnos nesse caso é pendência técnica, não autorização para proibir datas iguais. (`app/ponto/portas/celula.py:622`; :629; :711; :713; `app/docs/RELATO.md:1640`)

**2. Arquivos da obra**

O censo fornecido **não contém uma lista fechada de arquivos a alterar**: contém referências de comportamento, dependências, testes existentes e documentação. A lista efetiva de alterações de produção/teste é [nao verificado]. O registro da obra também descreve o contrato sem enumerar arquivos de implementação ou novos testes. (`censo_v2.r:1`; :19; :45; :56; :66; `app/docs/BACKLOG.md:2177`)

O inventário rastreável do censo reúne **21 arquivos de produção, um teste e dois documentos**; ele aparece integralmente na tabela seguinte. Isso não transforma dependências citadas em alterações obrigatórias. (`censo_v2.r:5`; :9; :13; :19; :20; :26; :28; :37; :39; :45; :46; :47; :48; :52; :56; :60; :62; :79; :88`) [nao verificado]

**3. Cruzamento**

A tabela cruza **o inventário citado**, pois o conjunto efetivo de arquivos que O10-D mudará permanece [nao verificado].

Fontes das células:

- **A:** lista integral de arquivos BUG-PAR fornecida no item 3(a), tratada como `BUG-PAR:1`.
- **B:** inventário de migração do predesenho e avaliação da c9, iniciados em `predesenho.r:23` e :610. `n` significa ausência de indicação de mudança nesse inventário.
- **C:** diff próprio declarado da F2: somente `chamados/models.py` e `test_e4c_f2_models_le_o_ato.py`. (`matriz_F2.md:11`; :12)
- **D:** arquivos da F4 enumerados na introdução e costura com `chamados/juizes.py`; o contrato preserva também sítios por allowlist. (`matriz_F4.md:11`; :12; seção “CONTRATO DE SAIDA”, numeração física desta última: [nao verificado])

| Arquivo citado | Fonte no censo | (a) BUG-PAR | (b) F1 B2 pelo predesenho | (c) F2 | (d) F4 |
|---|---:|---|---|---|---|
| `app/ponto/portas/celula.py` | :1 | n · A | n · B | n · C | n · D |
| `app/ponto/management/commands/gerar_celulas.py` | :13 | n · A | n · B | n · C | n · D |
| `app/ponto/services/fechamento.py` | :19 | n · A | n · B | n · C | n · D |
| `app/chamados/rejuncao.py` | :20 | n · A | n · B | n · C | n · D |
| `app/chamados/services/feriado_reavaliar.py` | :20 | n · A | n · B | n · C | n · D |
| `app/escala/alimentacao.py` | :26 | n · A | n · B | n · C | n · D |
| `app/ponto/services/cartorio.py` | :28 | n · A | **s · `predesenho.r:131`**, R138 | n · C | n · D |
| `app/core/observ.py` | :37 | n · A | n · B | n · C | n · D |
| `app/ponto/management/commands/detectar_ausencias.py` | :39 | n · A | n · B | n · C | n · D |
| `app/chamados/push_utils.py` | :39 | n · A | n · B | n · C | n · D |
| `app/ponto/utils.py` | :45 | n · A | n · B | n · C | n · D |
| `app/escala/servico_jornada.py` | :46 | n · A | n · B | n · C | n · D |
| `app/colaboradores/mixins.py` | :47 | n · A | n · B | n · C | n · D |
| `app/chamados/utils.py` | :47 | n · A | n · B | n · C | n · D |
| `app/folha/export.py` | :48 | n · A | n · B; citado como contexto em :249 | n · C | n · D |
| `app/core/juizes.py` | :48 | n · A | n · B | n · C | n · D |
| `app/ponto/turnos.py` | :52 | n · A | n · B | n · C | n · D |
| `app/core/models.py` | :56 | n · A | n · B | n · C | n · D |
| `app/chamados/views_arquivar.py` | :60 | n · A | **s\* · `predesenho.r:617`** | n · C | n · D |
| `app/chamados/services/arquivo.py` | :60 | n · A | n · B | n · C | n · D |
| `app/ponto/models.py` | :62 | n · A | n · B | n · C | n · D |
| `app/ponto/tests/test_o10b_editar_dia_troca.py` | :9 | n · A | n · B | n · C | n · D |
| `app/docs/BACKLOG.md` | :13 | n · A | n · B | n · C | n · D |
| `app/docs/RELATO.md` | :88 | n · A | n · B | n · C | n · D |

`\*` O predesenho recomenda aproveitar a passagem de `agora/ator/causa` da c9, citando `views_arquivar.py:83`; isso demonstra presença no aproveitamento proposto, mas sua atribuição definitiva à **B2**, depois da B1, é [nao verificado]. (`predesenho.r:617`)

Assim, há referência de migração da F1 em `cartorio.py` e referência condicionada em `views_arquivar.py`. **Não há base para declarar o cruzamento definitivo da O10-D fechado**, porque o censo não fornece seu conjunto de alterações. (`predesenho.r:131`; :617; lista efetiva de mudanças da O10-D: [nao verificado])

**4. Perguntas de lei**

As quatro perguntas finais do censo foram respondidas pelo aval fornecido; o registro atual confirma expressamente as seis originais e as quatro residuais como respondidas. (`aval_lei_permuta.txt:1`; `app/docs/RELATO.md:1640`)

| Pergunta residual do censo | Resposta fornecida |
|---|---|
| Quais turnos entram nas 11h? | Fim previsto do anterior → início do assumido; fim do assumido → início do seguinte, para ambos, sobre a célula após a permuta; sem exceção de CCT. (`censo_v2.r:85`; `aval_lei_permuta.txt:1`) |
| Restrições entre empresas, vínculos e escalas? | Mesma empresa/CNPJ; ambos ativos e vigentes nas duas datas; escalas diferentes permitidas se passarem nas guardas. (`censo_v2.r:86`; `aval_lei_permuta.txt:1`) |
| Quais cartas são elegíveis? | Ato/Ausencia tipo permuta, aprovado, período do dia exato, vinculado ao ID; ausência preexistente de outro tipo não serve. (`censo_v2.r:87`; `aval_lei_permuta.txt:1`) |
| Permuta produz `folga_trabalhada` e decisão do admin? | Permuta formal é compensação, sem HE nem dobra de DSR; falta de DSR na semana barra. (`censo_v2.r:88`; `aval_lei_permuta.txt:1`) |

Não identifiquei outra decisão de negócio exigida pelos insumos. A confirmação pelo admin já consta do corte da troca; a ação RBAC específica, o desenho de `d1=d2` e os validadores são lacunas de implementação cuja solução não foi demonstrada. (`app/docs/CORTES.md:252`; `censo_v2.r:45`; :46; :47; `app/ponto/portas/celula.py:629`; :713; implementação: [nao verificado])

A homologação dos itens 1, 3 e 6 com o DP continua prevista; sua realização é [nao verificado]. (`aval_lei_permuta.txt:1`)

perguntas de lei: 0

## 16/11 12:42 · sessao 174dfb4261cd · - · - · origem=Ronald (tmux)

Todas as referências de `censo_v2.r` a `app/ponto/portas/celula.py` permanecem semanticamente compatíveis com a afirmação associada e **MOVEU +38 linhas**; nenhuma delas SUMIU. Conferência abaixo refere-se à semântica descrita no anexo, pois o arquivo histórico integral não está disponível nesta cópia. [nao verificado: identidade textual com 3803e6623]

| Afirmação do censo | Referências antigas → atuais | Resultado |
|---|---|---|
| Recusa capturada pela porta; `atomic` termina antes do `except` | `celula.py:652→690`, `694→732` (retorno em `737`) | MOVEU; a porta continua devolvendo `'recusada'`; `ValueError` de autor/motivo e `RuntimeError` de trilha continuam propagando (`celula.py:684`, `686`, `730`). |
| Cartas travadas por pk, somente `Ausencia` | `657→695` | MOVEU; `.select_for_update(of=('self',))`, `.order_by('pk')` (`celula.py:695`). |
| Ordem das células por colaborador/data, por chamada | `673→711` | MOVEU; o `sorted` recebe somente cedente/substituto dessa chamada (`celula.py:711`). |
| Trava individual dentro de editar | `593→631` | MOVEU; `CelulaDia.select_for_update()` (`celula.py:631`). |
| Risco de ordem composta A/d1 B/d1 A/d2 B/d2 | `657→695`, `673→711` | MOVEU; dedução estática ainda cabível; ocorrência concreta de deadlock [nao verificado]. |
| Teste concorrente citado é inspeção AST | `test_o10b_editar_dia_troca.py:314→314` | IGUAL; inspeciona chamadas `select_for_update` e `atomic`, sem executar concorrência (`test_o10b_editar_dia_troca.py:319`, `322`). | [nao verificado]
| Empresa atual e competências consultadas em cada edição, antes de igual | `513→551`, `568→606`, `594→632`, `676→714` | MOVEU; empresa consultada em `605`, competências em `607`, igualdade em `632`; bypass em `606`. |
| Não há pré-validação das quatro; chamada seguinte depois da anterior | `675→713` | MOVEU; edição ocorre dentro do laço, antes de avaliar o próximo lado (`celula.py:711`, `713`, `714`). |
| Carimbo/autor individual e quatro trilhas editar + duas trocar, se quatro mudanças | `598→636`, `611→649`, `686→724` | MOVEU; `timezone.now` por edição e evento individual/par (`celula.py:636`, `649`, `724`). |
| Deduplicação por competência local à execução | `450→488`, `458→496`, `627→665` | MOVEU; dicionário é local ao helper e `editar_dia` passa conjunto unitário (`celula.py:488`, `494`, `496`, `665`). |
| Rejunção e reavaliação em callback por alteração | `490→528`, `502→540` | MOVEU; callback reúne as duas, `on_commit` por chamada ao helper (`celula.py:531`, `536`, `539`, `540`). |
| Rejulgamento síncrono; pula futuro/humano, captura falhas | `427→465`, `434→472`, `437→475`, `627→665` | MOVEU; filtro humanos, teste futuro, chamada síncrona, tratamento (`celula.py:465`, `469`, `472`, `475`, `665`). | [nao verificado]
| Marca cobrança órfã na transação | `625→663` | MOVEU; import em `663`, chamada em `664`, dentro do `atomic` iniciado em `630` (`celula.py:630`, `663`). |
| Reler cartas/colaboradores; argumentos só pk | `656→694`, `657→695`, `593→631` | MOVEU; leitura nova da carta com colaborador e célula com trava (`celula.py:694`, `695`, `631`). |
| Um lado igual não impede outro; igual somente ambos | `594→632`, `596→634`, `673→711`, `679→717`, `690→728` | MOVEU; retorno individual antes da escrita, visita ambos e trilha com resultados (`celula.py:632`, `634`, `711`, `717`, `728`). |
| Igualdade não inclui pedido/cartas/motivo/autor; inversão pode reescrever | `594→632`, `675→713` | MOVEU; igualdade contém origem/trabalha/dna/escala e cada papel passa booleano oposto (`celula.py:632`, `711`, `713`). |
| Rollback exterior precisa ser provocado pelo compositor para evitar primeira confirmada | `652→690`, `694→732` | MOVEU; recusa é consumida depois do `atomic` interno (`celula.py:690`, `732`, `737`). |
| Rollback exterior inclui marca/síncrono; callbacks dependem de commit | `625→663`, `434→472`, `502→540` | MOVEU para chamadas da porta (`celula.py:630`, `663`, `472`, `540`); propriedades de Django/transações e outras fontes devem ser consolidadas pelo leitor dessas fontes. | [nao verificado]
| Permissão, 11h e colisão não são guardas da porta | `559→597`, `568→606`, `646→684`, `675→713` | MOVEU; valida autor/motivo/booleano e lavra/vínculo em editar; em trocar, autor/motivo/cartas/colaboradores/documento (`celula.py:597`, `599`, `601`, `606`, `622`, `684`, `691`, `698`, `700`, `702`). |
| Trocar não recebe ID próprio; trilha objeto carta e JSON lados | `632→670`, `682→720`, `686→724` | MOVEU; assinatura e JSON não possuem ID de permuta (`celula.py:670`, `720`, `724`, `728`). |
| Coordenadas vêm das cartas relidas | `659→697`, `673→711` | MOVEU; papéis/colaborador/data lidos da nova instância (`celula.py:697`, `711`, `713`). |
| Quatro registros para quatro coordenadas distintas | `659→697`, `673→711` | MOVEU, condicional a coordenadas distintas (`celula.py:697`, `711`). |
| Ausências presentes/encontradas, colaboradores distintos, documentos preenchidos | `653→691`, `660→698`, `662→700`, `664→702` | MOVEU; nenhuma comparação/abertura dos arquivos no caminho (`celula.py:691`, `698`, `700`, `702`, `722`). |
| Nome do documento apenas na trilha | `684→722` | MOVEU (`celula.py:722`). |
| Sem consumo/exclusividade global; sem exigir datas iguais dentro da chamada | `656→694`, `673→711` | MOVEU; seleção pelos IDs e edição pelas datas próprias, sem marca de consumo (`celula.py:694`, `695`, `711`, `713`). |
| Tipo/status/período/pedido não integram elegibilidade documental | `664→702`, `675→713`, `568→606`, `584→622` | MOVEU; documento é único campo documental testado; lavra/vínculo separados (`celula.py:702`, `713`, `606`, `622`). |

Mapa integral de linhas de `celula.py` citadas no censo, para completar referências de linhas/afirmações agrupadas:

```text
427→465 434→472 437→475 450→488 458→496
490→528 502→540 513→551 559→597 568→606
584→622 593→631 594→632 596→634 598→636
611→649 625→663 627→665 632→670 646→684
652→690 653→691 656→694 657→695 659→697
660→698 662→700 664→702 673→711 675→713
676→714 679→717 682→720 684→722 686→724
690→728 694→732
```

Sobre o contrato do construtor:

- **Rollback:** a obrigação de o compositor abortar o `atomic` externo quando receber `'recusada'` permanece; a porta captura `_TrocaRecusada`, embora outras exceções possam propagar (`app/ponto/portas/celula.py:690`, `app/ponto/portas/celula.py:732`, `app/ponto/portas/celula.py:737`, `app/ponto/portas/celula.py:731`).
- **Ordem das quatro:** ordenar quatro células continua requisito do compositor/desenho; a porta ordena somente dois lados e também adquire cartas antes das células (`app/ponto/portas/celula.py:695`, `app/ponto/portas/celula.py:711`). A ordem global deve considerar também as cartas para não adquirir cartas de pares opostos antes de tentar as restantes; ocorrência concreta de deadlock [nao verificado].
- **d1=d2:** a porta não proíbe datas iguais e não conhece `d1/d2` de uma composição; a lei fornecida permite expressamente `d1=d2` (`aval_lei_permuta.txt:1`, anexo; `app/ponto/portas/celula.py:711`). Portanto, a frase final do censo “quatro células — que exige duas datas distintas” **MUDOU frente à lei**, e não deve virar validação impeditiva (`aval_lei_permuta.txt:1`, anexo).
- **Limite material da composição no mesmo dia:** `editar_dia` monta DNA a partir do vínculo do próprio colaborador e de um booleano; não recebe turno do outro (`app/ponto/portas/celula.py:622`, `app/ponto/portas/celula.py:629`, `app/ponto/portas/celula.py:572`). Duas trocas inversas no mesmo dia reescrevem as mesmas duas coordenadas com booleanos opostos; a segunda não representa quatro células distintas (`app/ponto/portas/celula.py:711`, `app/ponto/portas/celula.py:713`). A representação da troca de turnos distintos no mesmo dia no esquema atual exige investigação/desenho técnico; não é uma nova regra de negócio dada a permissão explícita do aval (`aval_lei_permuta.txt:1`, anexo).

## 16/11 12:41 · sessao 7cdb8c89aa6e · - · - · origem=Ronald (tmux)

| Afirmação/referência do censo | Sítio hoje | Veredito |
|---|---|---|
| Recálculo posterior por `on_commit`: `fechamento.py:1824`; rollback externo condiciona callback: `:1802` | `app/ponto/services/fechamento.py:1824` registra `_rodar`; `:1802` declara dependência do commit; `_rodar` recalcula uma competência em `:1809`–`:1810`. | **IGUAL** nos sítios citados. |
| Rejunção só alcança FK nula: `rejuncao.py:60` | `app/chamados/rejuncao.py:60` filtra vivos; `:62` exige `celula__isnull=True`; escritor em `:98`, contador condicionado à escrita em `:107`. | **IGUAL**. |
| Reavaliação recorta dias/chamados vivos: `feriado_reavaliar.py:17/:76` | `app/chamados/services/feriado_reavaliar.py:18` filtra vivos; `:77`–`:80` recortam datas e colaborador; `:84` confirma dia e executa encerramento. | **IGUAL**; referências antigas `:17/:76` eram linhas adjacentes às expressões relevantes e continuam existentes. |
| Consulta das competências exportadas sem lock: `gerar_celulas.py:161` | `app/ponto/management/commands/gerar_celulas.py:170` consulta `ExportacaoDominio.objects.filter(...).order_by().values_list(...).distinct()`; juiz chamado em `:182`; a função está em `:135`. | **MOVEU**: consulta `161 → 170` (+9). Não há `select_for_update` nesse corpo (`:135`–`:189`). |
| Marca órfã consulta carimbos recentes: `gerar_celulas.py:241` | `app/ponto/management/commands/gerar_celulas.py:250` chama `carimbos_de_reescrita`; filtro colaborador/vivos em `:252`–`:254`; juiz em `:257`. | **MOVEU**: `241 → 250` (+9). |
| Marcador atualiza contexto: `gerar_celulas.py:255` | `app/ponto/management/commands/gerar_celulas.py:264` chama `renovar_contexto(..., orfa_regeneracao=...)`; docstring “só marca” em `:235`. | **MOVEU**: `255 → 264` (+9). |
| Cartório relê célula: `cartorio.py:1976` | `app/ponto/services/cartorio.py:2027` consulta `CelulaDia.objects.filter(colaborador=..., data=...).first()`. | **MOVEU**: `1976 → 2027` (+51). |
| Rejulgamento pode alcançar abertura de cobrança: `cartorio.py:1740` | `app/ponto/services/cartorio.py:1791` chama `emissor._abrir_chamado`, condicionado a `apply_`, à regra de lista e à âncora (`:1776`, `:1790`). | **MOVEU**: `1740 → 1791` (+51). |
| Abertura pode enviar push imediato: `detectar_ausencias.py:778` | `app/ponto/management/commands/detectar_ausencias.py:778` guarda `not _flexivel`; chamada síncrona `push_supervisao` em `:779`; falha capturada em `:785`. | **IGUAL**. |
| Push alcança envio externo: `push_utils.py:40` | `app/chamados/push_utils.py:40` chama `webpush(...)` diretamente. | **IGUAL**. Envio concreto durante permuta permanece **[nao verificado]**. |
| Recarregamento de vínculos: `alimentacao.py:355` | `app/escala/alimentacao.py:355` carrega células; `:357` carrega escalas; `:358` chama juiz do vínculo por par. | **IGUAL**. |
| Identity map limitado às instâncias recebidas e restaurado: `alimentacao.py:43/:47` | `app/escala/alimentacao.py:43` salva atributo anterior, `:44` instala mapa; `:47`–`:51` restauram/removem atributo. | **IGUAL**. |
| Savepoint da auditoria não é commit independente: `observ.py:133` | `app/core/observ.py:133` abre `transaction.atomic()` interno; `:134`–`:137` criam auditoria. | **IGUAL** no código; rollback externo alcança essa escrita transacional, como dedução estática do atomic aninhado (`app/core/observ.py:133`). |
| TOCTOU documentado e pergunta sobre `folga_trabalhada`: `BACKLOG.md:1876` | Fila O10-B em `app/docs/BACKLOG.md:2176`, com TOCTOU e pergunta histórica de FT. | **MOVEU**: conteúdo da fila hoje em `:2176`. A linha física `:1876` agora trata CODEX-CONSTRUTOR-NEGOCIO (`app/docs/BACKLOG.md:1876`). |
| “BACKLOG.md:1879 trata BO-0511-G2-TRIAGEM” | BO está em `app/docs/BACKLOG.md:2181`; `:1879` agora trata H12-DEMISSAO-NA-JANELA. A obra O10-D está em `:2177`. | **MUDOU** a afirmação sobre o sítio físico; BO **MOVEU** para `:2181`. A referência correta da obra passou a existir em `:2177`. |
| Pergunta histórica FT aberta em `RELATO.md:449` | Pergunta histórica em `app/docs/RELATO.md:2120`; `:449` agora registra estado E3. Resposta específica da permuta formal registrada em `app/docs/RELATO.md:1640` e lei fornecida pelo usuário. | **MOVEU** o registro histórico para `:2120`; **MUDOU** a conclusão “continua não respondida” para O10-D, porque `:1640` registra as 6+4 respondidas. |

O contrato documental atual reafirma expressamente: porta retorna `'recusada'` sem raise, compositor deve levantar para rollback; ordenar as quatro células; `d1=d2` é permitido e o censo supunha datas distintas (`app/docs/RELATO.md:1640`). A obra também consta no BACKLOG com composição de duas trocas, pré-validação das quatro, rollback integral e identificação própria na trilha (`app/docs/BACKLOG.md:2177`).

Nos efeitos transitivos examinados, o limite material do rollback permanece: a abertura chama push sincronicamente e este alcança `webpush`, portanto rollback SQL não desfaz envio já realizado; ocorrência concreta nessa composição **[nao verificado]** (`app/ponto/services/cartorio.py:1791`; `app/ponto/management/commands/detectar_ausencias.py:779`; `app/chamados/push_utils.py:40`).

## 16/11 12:41 · sessao 262d1a369f1a · - · - · origem=Ronald (tmux)

Seções 2 e 3 conferidas somente por leitura; nenhuma execução Django/teste ou escrita. [nao verificado]

| Afirmação do censo | Referência antiga → atual | Situação |
|---|---|---|
| Helper de cobertura declara que não valida 11h | `app/ponto/utils.py:152` → `:152` (item explícito em `:153`) | **IGUAL**. |
| `marcos_turno` dá datetimes crescentes e trata meia-noite | `app/escala/servico_jornada.py:61` → `:69` (laço `:88-93`) | **MOVEU**, sentido preservado. |
| `fim_turno` fornece término | `app/escala/servico_jornada.py:89` → `:97` (retorno `:101`) | **MOVEU**, sentido preservado. |
| `tem_acao` verifica ações sem gravar | `app/colaboradores/mixins.py:322` → `:322` (corpo `:326-332`) | **IGUAL**. |
| `pode_agir_chamado` verifica departamento dono | `app/chamados/utils.py:68` → `:68` (delegação `:72`) | **IGUAL**. |
| Lavra é juiz separado, bool/fontes | `app/folha/export.py:589` → `:597` (fontes/retorno `:622-634`) | **MOVEU**, sentido preservado. |
| Registro K9 aponta para esse juiz | `app/core/juizes.py:322` → `:353` | **MOVEU**, sentido preservado. |
| `_competencias_exportadas` enumera janelas candidatas e consulta juiz | `app/ponto/management/commands/gerar_celulas.py:126` → `:135` (consulta `:184`, janela `:187`) | **MOVEU**, sentido preservado. |
| `autoridade_da_lavra` compõe frase sem gravação | `app/ponto/management/commands/gerar_celulas.py:208` → `:217` (corpo `:225-229`) | **MOVEU**, sentido preservado. |
| `ausencias_sobrepostas` julga períodos de Ausencia, não colisão horária de plantões | `app/ponto/turnos.py:2795` → `:2817` (consulta `:2839-2848`) | **MOVEU**, sentido preservado. |
| `evento` não tem argumento correlação/lote/grupo; aceita antes/depois | `app/core/observ.py:94` → `:94` | **IGUAL**. |
| JSONs de auditoria persistem antes/depois | `app/core/observ.py:133` → `:133` (campos em `:136`) | **IGUAL**. |
| `LogAuditoria` tem objeto_id inteiro e JSONs | `app/core/models.py:298`, `:300` → mesmas linhas (`valor_depois :301`) | **IGUAL**. |
| `evento_kw` só encaminha campos conhecidos | `app/core/observ.py:142` → `:142` (encaminhamento `:146-148`) | **IGUAL**. |
| Precedente `lote='arquivar_lote'` é rótulo fixo | `app/chamados/views_arquivar.py:146` → `:146` | **IGUAL**. |
| Metadado extra é incorporado no JSON | `app/chamados/services/arquivo.py:133` → `:133` (incorporação `:134`, evento `:137-138`) | **IGUAL**. |
| `Ausencia.chamado_origem` pode referir pedido/chamado | `app/ponto/models.py:786` → `:786` | **IGUAL**; FK é especificamente para `ChamadoColaborador` (`:787-789`). |

**Limite dos controles prontos:** ausência de juiz completo de 11h/colisão pronto continua **[nao verificado]**; o helper declara explicitamente não validar 11h (`app/ponto/utils.py:152-153`), e os helpers geométricos só produzem marcos/término (`app/escala/servico_jornada.py:69`, `:97`). Busca textual por `11h|interjornada|permuta|permutar_plantoes` em `app/ponto`, `app/escala` e registro não localizou compositor/validador específico; isso não prova inexistência sob outro nome **[nao verificado]**.

**Escopo:** o censo fornecido enumera sítios existentes e insumos; não contém uma lista explícita “arquivos a modificar”. Lista fechada de mudanças de produção/teste **[nao verificado]**. Não converter todos os arquivos referenciados em arquivos que a obra muda. [nao verificado]

**Documentos atuais úteis para consolidação:**

- `app/docs/BACKLOG.md:2177`: contrato `permutar_plantoes(A,d1,B,d2)`, duas portas no atomic, pré-validação das quatro, rollback integral, recusa falante, ID/trilha e escala-base intocada; registra compositor levantar e ordenação das quatro.
- `app/docs/RELATO.md:1640`: declara respondidas as seis perguntas v1 e quatro residuais v2, mantém rollback por raise do compositor, ordenação das quatro e corrige a suposição do censo: **d1=d2 permitido**.
- `app/docs/PROMPTS.md:2015`: resumo das seis respostas da lei, inclusive mesma empresa, carta ato aprovado, compensação e guarda DSR.
- `app/docs/RELATO.md:1602`: “sem copia de construcao”; faltam construtor, selos RED 1–6 e etapas seguintes.
- `app/docs/RELATO.md:1645`: referência histórica ao BACKLOG:1879 reescrito; a linha atual da obra é `app/docs/BACKLOG.md:2177`.
- `app/docs/RELATO.md:5`: censo físico vive em logs não disponíveis na cópia e papel PREPARA foi objeto de revisão; portanto linhas físicas dos anexos apenas colados devem ter convenção explícita ou **[nao verificado]**.

As quatro perguntas finais do censo são expressamente tratadas como respondidas pela documentação atual (`app/docs/RELATO.md:1640`); correspondência semântica com a lei fornecida: turnos previstos anterior/seguinte (§1), empresa/vínculo/tipo (§3), cartas (§5), compensação sem folga_trabalhada (§6). Fonte física da lei anexa **[nao verificado]**.

## 16/11 12:36 · sessao 929ffb0f1659 · CORTES-F2-F9-1611 · construtor · origem=Code

Entregues quatro **TESTE-RED** e uma **CARACTERIZACAO**, com fixtures salvas copiadas do [teste vizinho](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:49), reunidas no [apoio novo](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/cortes_f2_f9_apoio.py:37). [nao verificado]

| Degrau | Decisão e resposta-alvo após F2 | Arquivo escrito e mutante |
|---|---|---|
| **M4** | **TESTE-RED**. Sem viva/validação/efeito, RESPOSTA no ato → `RESPONDIDA`, mesmo com projeção vazia. Hoje a queda lê `respondida_em` (`app/chamados/models.py:3158`); RESPOSTA já significa aguardar validação (`app/chamados/services/tarefa_do_dia.py:159`). | [M4](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_cortes_f2_f9_m4.py:51). Mutante `m4_pendente`: força a queda para `PENDENTE`. |
| **M13** | **TESTE-RED**. `_ancoradas` exclui a pergunta validada pelo ato e conserva a pendente. Hoje filtra pelo campo (`app/chamados/models.py:344`); VALIDACAO é decisão existente (`app/chamados/services/tarefa_do_dia.py:167`). | [M13](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_cortes_f2_f9_m13.py:38). Mutante `m13_inclui_validadas`: retira o filtro de validação. |
| **M14** | **TESTE-RED**. Relavra a pendente e preserva `julgado_em` das validadas, inclusive só no ato. Hoje a seleção usa `validada_em` (`app/chamados/models.py:310`); a lavra real altera o instante (`app/chamados/veredito_lavrado.py:272`). | [M14](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_cortes_f2_f9_m14.py:41). Mutante `m14_relavra_validadas`: inclui validadas na relavra. |
| **M15** | **TESTE-RED**. Após REABERTURA, SILENCIO fotografa resposta vigente `''`, preservando a anterior no histórico. A porta zera a resposta (`app/chamados/services/disputa_emissao.py:1609`); hoje SILENCIO fotografa os brancos residuais da projeção (`app/chamados/models.py:2007`). | [M15](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_cortes_f2_f9_m15.py:40). Mutante `m15_resposta_anterior`: fotografa `'07:13'`. |
| **M18** | **CARACTERIZACAO da cópia**. O C6 real recebe cópia com resposta/carimbos zerados; original e banco ficam intactos. Mesmo alvo antes/depois, conforme o trecho não migrado (`app/chamados/models.py:1541`, `app/chamados/models.py:1553`). | [M18](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_cortes_f2_f9_m18.py:41). Mutante `m18_texto_residual`: injeta texto na cópia. |

**M19 — ALVO-INDETERMINADO.** Qual API existente deve responder pelo contrato dos ramos prefetch, `pk=None` e duplo, sem pressupor `_leitura_dos_atos` da c3? Correspondência desse helper no main: **[nao verificado]**.

**M20 — ALVO-INDETERMINADO.** Qual lista de leituras residuais permitidas, identificada por `Classe.metodo`, define o oráculo do selo nesta base? Aplicabilidade de `F2_RESIDUO` da c3: **[nao verificado]**.

Comandos individuais, a partir de `app/`, também registrados nas docstrings dos arquivos:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m4 --settings=config.settings.ci
CORTES_F2_MUTANTE=m4_pendente PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m4 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m13 --settings=config.settings.ci
CORTES_F2_MUTANTE=m13_inclui_validadas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m13 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m14 --settings=config.settings.ci
CORTES_F2_MUTANTE=m14_relavra_validadas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m14 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m15 --settings=config.settings.ci
CORTES_F2_MUTANTE=m15_resposta_anterior PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m15 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m18 --settings=config.settings.ci
CORTES_F2_MUTANTE=m18_texto_residual PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m18 --settings=config.settings.ci
```

Em **cada degrau**, falha/verde efetivos, ausência de erro de setup e mordida dos mutantes: **[nao verificado]**. Testes e mutantes não foram executados. Conservação integral da materialização e invariância do `supra_juiz` em competência exportada: **[nao verificado]**. [nao verificado]

## 16/11 12:21 · sessao 7b8a48377983 · CORTES-F2-F9-1611 · leitura · origem=Code

Para A, **NÃO para o contrato completo “instante do ato de materialização, valor conservado”**. Hoje existe um instante persistido, mas o efeito lido é o **campo atual `materializada_em`**: ele pode ser substituído ou apagado. A projeção não o reconstrói de um ato conservado. (`app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1400`, `app/chamados/models.py:3295`, `app/chamados/models.py:3318`, `app/chamados/models.py:3393`.)

As provas abaixo são por **leitura das asserções**; resultados de execução, diferenças históricas por Git e efeitos sobre dados de produção: **[nao verificado]**.

A projeção atual recebe diretamente `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em` e `via_resolucao`. Seu índice `[3]` é `materializada_em`; quando recebe `agora`, apenas oculta carimbos posteriores ao instante consultado. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1396`, `app/chamados/juizes.py:1400`.)

**Os dois leitores pedidos não consomem diretamente essa tupla:**

- `sem_veredito_entre` exige `materializada_em is None`, validação ausente e resposta por instante **ou** texto; aplica também a tranca, salvo dispensa explícita. Portanto qualquer carimbo exclui pela materialização, independentemente da via. (`app/chamados/models.py:1923`, `app/chamados/models.py:1924`, `app/chamados/models.py:1926`.)
- `estado` retorna `RESOLVIDA` imediatamente para validada com carimbo. Sem validação, consulta primeiro `pergunta_viva`; somente se não estiver viva aceita o carimbo como `RESOLVIDA`. **Sair da lista não garante, sozinho, `RESOLVIDA`.** (`app/chamados/models.py:3151`, `app/chamados/models.py:3153`, `app/chamados/models.py:3154`, `app/chamados/models.py:3156`.)

| Ramo | Quem escreve o valor | Quem lê e o que significa hoje |
|---|---|---|
| **Carimbo de identidade** | `carimbar_identidade` escolhe `quando or _tz.now()` e salva. A atribuição comum escreve `materializada_em`, `materializada_por_id` e `via_resolucao`. Com `manter_carimbo=True`, conserva instante e autor anteriores. (`app/chamados/models.py:3292`, `app/chamados/models.py:3295`, `app/chamados/models.py:3300`, `app/chamados/models.py:3209`, `app/chamados/models.py:3210`, `app/chamados/models.py:3211`.) | A projeção lê o instante atual; o C6 combina via e carimbo; lista e estado leem o campo conforme a precedência acima. Não há seleção especial de identidade para formar `[3]`. (`app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1416`, `app/chamados/juizes.py:1444`, `app/chamados/models.py:1923`, `app/chamados/models.py:3153`.) |
| **`materializada_em` solto, via vazia** | `carimbar` recebe a via, escolhe o instante e repassa ao escritor comum; não há rejeição explícita de `''` nesse trecho. Escritor histórico dos registros soltos: **[nao verificado]**. (`app/chamados/models.py:3228`, `app/chamados/models.py:3234`, `app/chamados/models.py:3239`, `app/chamados/models.py:3209`.) | `[3]` recebe esse instante e a lista exclui a pergunta. Para `estado`, a via vazia não elimina a precedência de `pergunta_viva`; validada com carimbo resolve diretamente. (`app/chamados/juizes.py:1390`, `app/chamados/models.py:1923`, `app/chamados/models.py:3153`, `app/chamados/models.py:3154`.) |
| **Materialização de fato, sem carimbo** | `_pergunta_materializada_de_fato` fornece uma decisão booleana; o materializador usa essa decisão como guarda, captura o instante depois e chama `carimbar`. A presença do fato e a escrita do carimbo são passos distintos. (`app/chamados/juizes.py:666`, `app/chamados/services/materializacao.py:1239`, `app/chamados/services/materializacao.py:1250`, `app/chamados/services/materializacao.py:1283`.) | Sem carimbo, `[3]` permanece `None`. E sozinho não exclui por materialização nem produz `RESOLVIDA`; ambos os retornos `RESOLVIDA` exigem `materializada_em`. No C6, o degrau sem carimbo retorna viva antes da chamada final a E. (`app/chamados/juizes.py:1390`, `app/chamados/models.py:1923`, `app/chamados/models.py:3153`, `app/chamados/models.py:3156`, `app/chamados/juizes.py:1482`, `app/chamados/juizes.py:1489`.) |
| **`materializada_conservada_em`** | Não foi localizado esse campo/helper na cópia: **[nao verificado]**. O escritor efetivamente encontrado atribui `materializada_em`. (`app/chamados/models.py:3209`.) | A projeção atual não contém esse ramo: devolve o campo atual, eventualmente recortado por `agora`. (`app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1396`, `app/chamados/juizes.py:1400`.) |

Quanto ao relógio, **a projeção e o filtro de materialização de `sem_veredito_entre` não consultam `today/now`**. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1400`, `app/chamados/models.py:1923`.) Entretanto, **`estado` pode consultar relógio fora da borda**, transitivamente: chama `pergunta_viva(self)` sem instante; o cálculo pode alcançar `date.today()` na janela e `_tz.now()` na classificação da falta. (`app/chamados/models.py:3154`, `app/chamados/juizes.py:898`, `app/chamados/juizes.py:1471`, `app/chamados/juizes.py:596`, `app/chamados/juizes.py:1475`, `app/escala/servico_jornada.py:387`.)

Em `supra_juiz`, os leitores indiretos são as chamadas de `sem_veredito_entre` para **pai** e **destino**, ambas com `ignorar_tranca=True`; `pode_executar_csf` decide pela ausência dessas perguntas. (`app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:574`, `app/ponto/supra_juiz.py:588`.) Mudança efetiva em competência exportada: **[nao verificado]**.

A parte “instante do ato” tem correspondência operacional: o materializador captura `agora` e o persiste no carimbo. (`app/chamados/services/materializacao.py:1250`, `app/chamados/services/materializacao.py:1283`, `app/chamados/models.py:3262`.) A parte “valor conservado” não está fixada como contrato geral: conservar é opção na identidade; descarimbar apaga o valor; e o catálogo atual de `DecisaoPergunta` não lista ato de materialização. (`app/chamados/models.py:3292`, `app/chamados/models.py:3295`, `app/chamados/models.py:3318`, `app/chamados/models.py:3393`, `app/chamados/models.py:4024`, `app/chamados/models.py:4026`.)

Para B, “BASE mudou?” compara o comportamento atual com a **BASE transcrita no anexo**; confirmação por diff histórico: **[nao verificado]**. As hipóteses sobre leitura conservada da F1 antiga não podem ser transportadas para a projeção atual, que lê os campos diretamente. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1400`.)

| Degrau | Sítio hoje | BASE mudou? | Prova hoje | Nota |
|---|---|---|---|---|
| **[DEPENDE-F1] 1 — solto deixa a lista** | `app/chamados/models.py:1923`; projeção em `app/chamados/juizes.py:1390`. | **Não**: qualquer `materializada_em` continua excluindo, como a BASE. | Forma exigida em `app/chamados/tests/test_contract_dedup_status.py:143`; valor materializado via CSF em `app/ponto/tests/test_supra_juiz_executor.py:85`. | O teste T:189 citado no anexo, aplicado à árvore atual: **[nao verificado]**. |
| **[DEPENDE-F1] 2 — assinatura passiva × face SQL** | A validação atual é lida em `app/chamados/models.py:3151` e `app/chamados/juizes.py:1405`. | **Não nesses leitores**: continuam por campo. `_q_validada_pelo_ato` não localizado: **[nao verificado]**. | Asserção do confronto específico passivo/validação/face citada: **[nao verificado]**. | A paridade existente em `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:81` compara **resposta**, não validação. |
| **[DEPENDE-F1] 3 — desaparecimento do relógio** | Efeito em `app/chamados/juizes.py:1390`; `estado` chama o C6 em `app/chamados/models.py:3154`. | **Não no efeito por campo**. A conclusão ampla “leitores sem relógio” não vale para `estado`: caminho até `app/chamados/juizes.py:596`. | Asserção específica variando o relógio nesses leitores: **[nao verificado]**. | Não há E formando `[3]` no corpo atual; ainda há relógio transitivo no C6. (`app/chamados/juizes.py:1400`, `app/chamados/juizes.py:1471`.) |
| **[DEPENDE-F1] 4 — continência de `q_vivas`** | SQL por campo em `app/chamados/models.py:3496`; C6 por campo em `app/chamados/juizes.py:1405`. | O degrau de validação continua por campo; a quebra hipotética por migração para atos não está nesses corpos. | `app/chamados/tests/test_contract_pergunta_viva.py:60`; por degrau em `app/chamados/tests/test_viva_delega_ao_juiz.py:45`. | Cobertura específica com ato divergindo da projeção: **[nao verificado]**. |
| **[DEPENDE-F1] 5 — guarda `pk None`, N1/M7** | Leitura direta em `app/chamados/juizes.py:1389`; opções em `app/chamados/models.py:3706`. | **Não nesses caminhos**: não passam pela guarda conservada descrita no anexo. | Instâncias não salvas: `app/chamados/tests/test_pergunta_estado.py:47`, `app/chamados/tests/test_valor_rotulo.py:12`, `app/chamados/tests/test_valor_rotulo.py:16`. | São provas da leitura atual por campos, não de `_leitura_conservada`. |
| **[DEPENDE-F1] 6 — NameError no teste de memória antigo** | Identificação do teste/sítio correspondente hoje: **[nao verificado]**. | **[nao verificado]**. | **[nao verificado]**. | O anexo não fornece arquivo e símbolo suficientes para vincular aquele erro a uma asserção atual: **[nao verificado]**. | [nao verificado]

| Degrau | Sítio hoje | BASE mudou? | Prova hoje — asserção | Nota |
|---|---|---|---|---|
| **M1b** | `app/chamados/models.py:3151`, `app/chamados/models.py:3153`. | **Não**: validada com materialização → `RESOLVIDA`. | **Sim:** `app/chamados/tests/test_pergunta_estado.py:47`. | Fixture combina ambos os carimbos em `app/chamados/tests/test_pergunta_estado.py:45`. |
| **M4** | Queda final em `app/chamados/models.py:3158`. | **Não**: decide por `respondida_em`. | Prova que isole especificamente essa queda: **[nao verificado]**. | Asserções de rótulo existem em `app/chamados/tests/test_pergunta_estado.py:30` e `app/chamados/tests/test_pergunta_estado.py:36`, mas não isolam C6 falso sem validação/materialização. |
| **M7** | `app/chamados/models.py:3693`, `app/chamados/models.py:3701`, `app/chamados/models.py:3706`. | **Não**: fonte continua `resposta_colab`. | **Sim, opções:** `app/chamados/tests/test_valor_rotulo.py:12` e `app/chamados/tests/test_valor_rotulo.py:16`. | Dropdown posto/escala e texto vindo de ato: **[nao verificado]**. |
| **M11** | `app/chamados/models.py:1926`. | **Não**: tranca exclui, salvo `ignorar_tranca`. | **Sim:** `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:304`; dispensa da tranca em `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:305`. | A tranca é substituída por `True` em `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:303`. |
| **M13** | `_ancoradas`, filtro em `app/chamados/models.py:344`. | **Não**: usa `validada_em__isnull=True`. | **Parcial, AST:** `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:284`. | Exige presença dos campos; não distingue `isnull=True` de `False`, pois a extração retém o nome do campo. (`app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:283`.) |
| **M14** | Filtro e relavra em `app/chamados/models.py:310`, `app/chamados/models.py:312`. | **Não**: relavra não validadas por campo. | **Parcial:** relavra e ordem após salvar em `app/chamados/tests/test_o261e_f1_codex_v5.py:90`, `app/chamados/tests/test_o261e_f1_codex_v5.py:91`. | Asserção que distinga “só não validadas” de incluir também validadas: **[nao verificado]**. |
| **M15** | SILENCIO e foto em `app/chamados/models.py:2006`, `app/chamados/models.py:2007`. | **Não**: `ato_texto` vem de `resposta_colab`. | **Parcial:** existência/autoria do SILENCIO em `app/chamados/tests/test_e1_entradas.py:125`. | Essa asserção não consulta `ato_texto`; prova da fonte da fotografia: **[nao verificado]**. |
| **M17** | Conserva R/V em `app/chamados/models.py:1613`; limpa em `app/chamados/models.py:1629`. | **Sim**: agora também zera `resposta_colab=''`. (`app/chamados/models.py:1630`.) | **Sim:** par/carimbo em `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:276`, `:277`, `:278`; ausência de devolução em `:281`; trilha exata em `:285`; fila em `:329`, todos no mesmo arquivo. | Continua sem REABERTURA/RECUSA; conserva atos R/V, não “nenhum ato”. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:281`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:285`.) |
| **M18** | Cópia e julgamento em `app/chamados/models.py:1541`, `app/chamados/models.py:1557`. | **Sim**: simulação agora zera também texto. (`app/chamados/models.py:1556`.) | **Parcial:** seleção pelo C6 em `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:139`, `:142`, `:149`, `:150`, todos no mesmo arquivo. | Asserção específica da limpeza do texto **na cópia**: **[nao verificado]**. |
| **M19** | `_leitura_dos_atos` não localizado: **[nao verificado]**. Leitores atuais em `app/chamados/models.py:3665`, `app/chamados/models.py:3688`, `app/chamados/models.py:1923`. | Os leitores encontrados continuam por campo; não mostram a migração c3. | Prova dos ramos prefetch/`pk None`/duplo daquele helper: **[nao verificado]**. | `respostas_anteriores` lê decisões, mas é outro método. (`app/chamados/models.py:3667`, `app/chamados/models.py:3675`.) |
| **M3** | `app/chamados/models.py:3154`, `app/chamados/models.py:3156`. | **Não**: viva precede carimbo; não viva com carimbo resolve. | **Sim, solto:** `app/chamados/tests/test_pergunta_estado.py:53`, com fixture em `app/chamados/tests/test_pergunta_estado.py:52`. | Prova E-sem-carimbo: **[nao verificado]**; E não fornece o efeito atual. (`app/chamados/juizes.py:1390`.) |
| **M9** | `app/chamados/models.py:1924`, `app/chamados/models.py:1925`. | **Não**: instante **ou** texto continuam suficientes. | **Sim:** resposta em `app/ponto/tests/test_supra_juiz_executor.py:72`; texto sem instante em `:81`; sem resposta em `:69`, todos no mesmo arquivo. | Alcançam o filtro real pela delegação em `app/ponto/tests/test_supra_juiz_executor.py:46`. |
| **M10** | `app/chamados/models.py:1923`. | **Não**: qualquer `materializada_em` exclui. | **Sim:** valor via CSF em `app/ponto/tests/test_supra_juiz_executor.py:85`; forma textual em `app/chamados/tests/test_contract_dedup_status.py:143`. | Prova E-sem-carimbo: **[nao verificado]**; esse ramo não compõe `[3]` hoje. (`app/chamados/juizes.py:1400`.) |
| **M20** | `F2_RESIDUO`/`SeloF2Test` da c3 não localizado: **[nao verificado]**. | Comparação daquele selo: **[nao verificado]**. | Prova equivalente fora de `estado`: **[nao verificado]**. | O selo em `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:233` fiscaliza **escritores do par**; não comprova o selo de leitores pedido. |

## 16/11 12:19 · sessao 371319897e7d · - · - · origem=Ronald (tmux)

| Degrau | Sítio hoje | BASE mudou? | Prova hoje, por leitura | Nota |
|---|---|---|---|---|
| M17 | `app/chamados/models.py:1560`, `:1613`, `:1629` | **Sim**: descarimbo agora também zera `resposta_colab=''` (`:1630`). Continua conservando R/V antes de limpar, sem REABERTURA (`:1613`, `:1629`). | `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:276`, `:277`, `:278`, `:281`, `:285`, `:329`. | `:281` proíbe REABERTURA/RECUSA; `:285` exige trilha exata R/V. Não significa “sem ato nenhum”. |
| M18 | `app/chamados/models.py:1515`, `:1541`, `:1553` | **Sim**: simulação agora zera texto junto dos instantes (`:1554`, `:1555`, `:1556`). Continua cópia em memória consultando `pergunta_viva` (`:1557`). | `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:139`, `:142`, `:149`, `:150`, `:181`, `:184`. | Mordem seleção pelo C6 e célula do julgamento. Asserção específica da nova limpeza de texto **na cópia**: [nao verificado]. |
| M19 | Helper `_leitura_dos_atos` não localizado [nao verificado]. Leitores atuais: `app/chamados/models.py:3665`, `:3688`, `:1923`. | Leitores encontrados seguem campos; a migração descrita para c3 não é o corpo atual (`:3665`, `:3688`, `:1923`). | Provas dos ramos prefetch/pk None/duplo daquele helper: [nao verificado]. | `respostas_anteriores` é outro leitor, de decisões, em `app/chamados/models.py:3667`, `:3675`. |
| M3 | `app/chamados/models.py:3154`, `:3156`, `:3158` | **Não**, ante BASE fornecida: viva precede o teste de materialização e o efeito lido é `materializada_em`. | `app/chamados/tests/test_pergunta_estado.py:53`; fixture sem resposta/validação, apenas materialização, em `:52`. | Morde solto via vazia. Caso E-sem-carimbo: [nao verificado]; esse efeito não é critério de RESOLVIDA no corpo atual (`models.py:3156`). |
| M9 | `app/chamados/models.py:1923`, `:1924`, `:1925` | **Não**: instante **ou** texto continuam suficientes. | `app/ponto/tests/test_supra_juiz_executor.py:72` (resposta), `:81` (texto sem instante), `:69` (sem resposta). Também `app/chamados/tests/test_e3_10_cura_no_escritor.py:132`, `:133`, `:134`. | O teste do CSF delega ao corpo real em `test_supra_juiz_executor.py:46`; só a tranca é substituída em `:58`. |
| M10 | `app/chamados/models.py:1923` | **Não**: qualquer `materializada_em` exclui. | Valor via CSF: `app/ponto/tests/test_supra_juiz_executor.py:85`. Forma textual: `app/chamados/tests/test_contract_dedup_status.py:143`. | `:85` combina origem validada e pergunta direta materializada; cada universo deve liberar o CSF. E-sem-carimbo: [nao verificado]. |
| M20 | F2_RESIDUO/SeloF2Test da c3 não localizado [nao verificado]. | Comparação daquele selo com BASE: [nao verificado]. | Prova equivalente cobrindo leitores fora de `estado`: [nao verificado]. | Existe selo de **escrita do par**, outro escopo: `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:233`; usa qualname (`:189`, `:192`) e morde método novo (`:265`, `:266`). Não demonstra o F2_RESIDUO descrito. |

Asserções citadas foram lidas; resultado de execução: [nao verificado].

## 16/11 12:19 · sessao 0c12813eb48b · - · - · origem=Ronald (tmux)

A conclusão é **NÃO** para “já fixa o efeito como instante do ato de materialização, valor conservado”: hoje `[3]` é o campo atual `p.materializada_em`, sem leitura de ato de materialização; as portas podem substituí-lo e apagá-lo. Há conservação pontual opcional do carimbo, mas não o contrato descrito para `be2b8d733`. Fontes: `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1400`, `app/chamados/models.py:3292`, `app/chamados/models.py:3295`, `app/chamados/models.py:3318`, `app/chamados/models.py:3393`.

**Fonte campo a campo e efeito real**

- A projeção recebe `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em`, `via_resolucao` diretamente da pergunta; se houver `agora`, apenas recorta datas posteriores. Não há leitura de `DecisaoPergunta` nesse corpo. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1400`.
- `materializada_em` é `DateTimeField` anulável, com descrição “Quando pergunta foi processada (batida criada + chamado fechado)”. `materializada_por` é o autor; `via_resolucao` declara via, admitindo vazio por legado. `app/chamados/models.py:3530`, `app/chamados/models.py:3534`, `app/chamados/models.py:2666`.
- O escritor comum em memória atribui os três: `self.materializada_em = quando`, `self.materializada_por_id = autor_id`, `self.via_resolucao = via`. `app/chamados/models.py:3207`, `app/chamados/models.py:3209`, `app/chamados/models.py:3210`, `app/chamados/models.py:3211`.
- `sem_veredito_entre` **não chama a projeção temporal**: exclui qualquer `materializada_em is not None`, independentemente da via ou de E; também exige validação ausente e instante de resposta ou texto, com tranca opcional. `app/chamados/models.py:1922`, `app/chamados/models.py:1923`, `app/chamados/models.py:1924`, `app/chamados/models.py:1926`.
- `estado` **não chama a projeção diretamente**: validada com carimbo → `RESOLVIDA`; sem validação, pergunta viva → `PENDENTE`/`RESPONDIDA`; só depois, não viva com carimbo → `RESOLVIDA`. Portanto ter carimbo exclui da lista, mas não garante `RESOLVIDA` em todos os ramos. `app/chamados/models.py:3151`, `app/chamados/models.py:3153`, `app/chamados/models.py:3154`, `app/chamados/models.py:3156`, `app/chamados/models.py:3158`.

| Ramo | Quem escreve | Quem lê e consequência |
|---|---|---|
| Carimbo de identidade | `carimbar_identidade` aceita as vias de identidade, escolhe `quando or _tz.now()` e salva; com `manter_carimbo=True`, reutiliza instante/autor anteriores. A atribuição efetiva é `_escrever_carimbo`. Exemplos: `fechar_por_admin` e recusa por colisão. `app/chamados/models.py:3278`, `app/chamados/models.py:3288`, `app/chamados/models.py:3292`, `app/chamados/models.py:3295`, `app/chamados/models.py:3300`, `app/chamados/models.py:3209`, `app/chamados/models.py:3174`, `app/chamados/services/materializacao.py:1105`. | Projeção lê o campo atual; C6 lê via+carimbo nos degraus anterior à resposta e de identidade; lista lê apenas presença do campo; `estado` segue a precedência acima. `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1416`, `app/chamados/juizes.py:1443`, `app/chamados/juizes.py:1444`, `app/chamados/models.py:1923`, `app/chamados/models.py:3151`. |
| `materializada_em` solto, via vazia | O campo admite nulo e a via admite `''`; `_escrever_carimbo` é o sítio de atribuição. A porta `carimbar` não rejeita explicitamente `''`; recebe `via` e repassa ao escritor. Origem histórica ou escritor concreto dos registros soltos: **[nao verificado]**. `app/chamados/models.py:3530`, `app/chamados/models.py:2666`, `app/chamados/models.py:3228`, `app/chamados/models.py:3234`, `app/chamados/models.py:3239`, `app/chamados/models.py:3209`. | `[3]` recebe esse instante; `sem_veredito_entre` exclui a pergunta mesmo com via vazia. Em `estado`, validada+carimbo resolve; sem validação, o C6 ainda pode considerá-la viva. `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1400`, `app/chamados/models.py:1923`, `app/chamados/models.py:3153`, `app/chamados/models.py:3154`. |
| Materialização de fato E sem carimbo | E devolve booleano; não atribui `[3]` nem grava o carimbo. O materializador usa E como guarda e depois chama `carimbar` com instante capturado. A porta de pendentes também testa E e carimba depois. `app/chamados/juizes.py:666`, `app/chamados/juizes.py:763`, `app/chamados/juizes.py:765`, `app/chamados/services/materializacao.py:1239`, `app/chamados/services/materializacao.py:1250`, `app/chamados/services/materializacao.py:1283`, `app/chamados/services/materializacao.py:1422`, `app/chamados/services/materializacao.py:1433`, `app/chamados/services/materializacao.py:1437`. | Sem carimbo, `[3]` continua `None`. E sozinho não exclui da lista nem produz `RESOLVIDA`: a lista exige o carimbo para exclusão por materialização; `estado` exige carimbo para ambos retornos `RESOLVIDA`; C6 retorna viva antes de E quando chega ao degrau `materializada is None`. `app/chamados/juizes.py:1390`, `app/chamados/models.py:1923`, `app/chamados/models.py:3153`, `app/chamados/models.py:3156`, `app/chamados/juizes.py:1482`, `app/chamados/juizes.py:1483`, `app/chamados/juizes.py:1489`. |
| `materializada_conservada_em` | Não há equivalente desse valor na projeção atual: o valor efetivamente escrito/lido é `materializada_em`. Existência histórica ou fora da cópia: **[nao verificado]**. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1400`, `app/chamados/models.py:3209`. | Não há ramo conservado nesse corpo; o retorno é o campo atual, eventualmente recortado por `agora`. `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1396`, `app/chamados/juizes.py:1400`. |

**Relógio fora da borda**

- A projeção e `sem_veredito_entre` não consultam relógio nesses corpos; usam os valores da pergunta e, na projeção, o `agora` recebido. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1391`, `app/chamados/models.py:1922`.
- `estado` pode consultar relógio transitivamente: chama `pergunta_viva(self)` sem `hoje/agora`; o cálculo consulta `_dentro_da_janela_viva`, cujo fallback é `date.today()`. A classificação da falta também recebe `agora=None` e pode usar `_tz.now()`. `app/chamados/models.py:3154`, `app/chamados/juizes.py:898`, `app/chamados/juizes.py:1471`, `app/chamados/juizes.py:596`, `app/chamados/juizes.py:1475`, `app/escala/servico_jornada.py:387`.
- E também pode alcançar `date.today()` pela janela, mas **não é a fonte de `[3]` hoje**. `app/chamados/juizes.py:697`, `app/chamados/juizes.py:790`, `app/chamados/juizes.py:596`, `app/chamados/juizes.py:1390`.
- `supra_juiz` lê o efeito indiretamente ao chamar `sem_veredito_entre` nos universos pai e destino, ambos com `ignorar_tranca=True`; a decisão do CSF é a ausência dessas perguntas. `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:574`, `app/ponto/supra_juiz.py:588`. Mudança efetiva em competência exportada, sem banco/log/diff histórico: **[nao verificado]**.

**Por que NÃO, embora exista instante persistido**

O materializador captura um instante e o persiste pelo carimbo; a leitura atual reutiliza esse campo, sem gerar um novo instante. Isso atende parcialmente à descrição operacional de instante persistido. `app/chamados/services/materializacao.py:1250`, `app/chamados/services/materializacao.py:1283`, `app/chamados/models.py:3209`, `app/chamados/models.py:3262`, `app/chamados/juizes.py:1390`.

Mas o contrato de valor conservado não está fixado: `carimbar_identidade` só conserva mediante opção; caso contrário escreve outro instante. `descarimbar` e a forma em lote apagam o valor. O catálogo de `DecisaoPergunta` contém resposta, validação, devolução, recusa, reabertura, silêncio, retratação e absorção, sem ato de materialização listado. `app/chamados/models.py:3292`, `app/chamados/models.py:3295`, `app/chamados/models.py:3318`, `app/chamados/models.py:3393`, `app/chamados/models.py:4024`, `app/chamados/models.py:4026`.

## 16/11 12:18 · sessao 5cf5a08ff617 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo seguem a BASE descrita no anexo: os leitores atuais usam os campos da projeção, conforme referências da tabela. Não houve execução de testes; resultado de execução é [nao verificado]. [nao verificado]

| Degrau | Sítio hoje | BASE mudou? | Prova hoje, por leitura | Nota |
|---|---|---|---|---|
| M1b | `app/chamados/models.py:3151`, `:3153` | Não: `validada_em != None` com `materializada_em != None` retorna RESOLVIDA (`:3153`). | **SIM**, `app/chamados/tests/test_pergunta_estado.py:47`; fixture com validação e materialização em `:45`. | Instância não salva criada pelo helper em `test_pergunta_estado.py:20`; esta prova morde o ramo atual por campos, não uma leitura pelo ato (`models.py:3153`). |
| M4 | `app/chamados/models.py:3158` | Não: queda final retorna PENDENTE/RESPONDIDA por `respondida_em` (`:3158`). | **[nao verificado]** para o ramo final específico. | `test_pergunta_estado.py:30`, `:36`, `:59` afirmam os rótulos, mas não isolam `pergunta_viva=False` e ausência de validação/materialização; a precedência está em `models.py:3151`, `:3154`, `:3156`. |
| M7 | `app/chamados/models.py:3686`; posto `:3693`; escala `:3701`; opções `:3706` | Não: todas as fontes seguem `resposta_colab` (`:3688`, `:3693`, `:3701`, `:3708`). | **SIM, opções**: `app/chamados/tests/test_valor_rotulo.py:12` exige “Falta”; `:16` exige opção string “Confirma”. | Fixture não salva e por campo (`test_valor_rotulo.py:7`). Dropdown posto/escala e resposta oriunda de ato: **[nao verificado]**. |
| M11 | `app/chamados/models.py:1926` | Não: exclui dia trancado, salvo `ignorar_tranca` (`:1926`). | **SIM**: `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:304` exige lista vazia com tranca; patch True em `:303`. Asserção `:305` exerce a retratação que ignora a tranca. | O juiz é chamado por `respostas_sem_veredito` em `models.py:1906`; portanto `:304` alcança o corpo atual. |
| M13 | `app/chamados/models.py:336`; filtro `:344`; âncora `:345`; carga `:346` | Não: filtro por `validada_em__isnull=True`, com destino/container (`:344`, `:345`). | **PARCIAL, AST**: `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:284` exige presença dos campos `validada_em`, `chamado_destino_id`, `disputa` no filtro; extração em `:283`. | Não prova valor de `isnull` nem retorno do queryset: o scanner usa primeiro segmento do kwarg, conforme contrato em `:193`. `test_e4b_classe_e_memoria.py:443` só exige chamada do método mockado (`:440`), não morde o corpo real. |
| M14 | `app/chamados/models.py:309`; filtro `:310`; relavra `:312` | Não: relavra somente `validada_em__isnull=True` do destino (`:310`). | **PARCIAL**: `app/chamados/tests/test_o261e_f1_codex_v5.py:90` exige relavra não vazia; `:91` exige estado já salvo; `:101` exige zero relavra quando save falha. | Fixture de `:81` contém pergunta sem validação; essas asserções não distinguem filtro “somente não validadas” de um filtro que também inclua validadas. Prova específica desse recorte: **[nao verificado]**. |
| M15 | `app/chamados/models.py:2005`; ato SILENCIO `:2006`; foto `:2007` | Não: `ato_texto=_q.resposta_colab or ''` (`:2007`). | **PARCIAL, existência/autoria do SILENCIO**: `app/chamados/tests/test_e1_entradas.py:125` exige `[('silencio', self.dp.pk)]`, após porta em `:124`. | A asserção não consulta `ato_texto` (`:125`); prova específica da fonte da fotografia: **[nao verificado]**. |

Uma ressalva para não superestimar M13: `app/chamados/tests/test_e4b_classe_e_memoria.py:467` afirma que o S128 **inclui** a validada, e não testa o filtro de `_ancoradas`; o caso se chama `test_RED_S128_conserva_validada_pendente_fora_das_ancoradas` em `:463`.

## 16/11 11:46 · sessao ae3a5fdf0c1e · ROTULO-APLICADA-SEM-BATIDA · censo · origem=Code

**O retorno incorreto está confirmado:** havendo resposta em pergunta de hora, `materializada_em` basta para devolver `aplicada`, salvo `paridade_recusada`; esse ramo não verifica batida nem `recusa_motivo`. (`app/chamados/services/materializacao.py:438–448`)

**Total na frota: [não verificado].** O anexo informa três perguntas — 47199, 43643 e 44051 —, mas não fornece censo completo nem permite reconstituir suas transações. [não verificado]

**(a) O que a função devolve**

As condições abaixo estão na ordem de execução; “qualquer” inclui batida criada, inexistente ou posteriormente desfeita. (`app/chamados/services/materializacao.py:438–465`)

| Condição | Retorno | Evidência |
|---|---|---|
| Tipo diferente de `hora`, ou resposta vazia após `strip()`; quaisquer carimbos, vias e batidas | `None` | `app/chamados/services/materializacao.py:438–439` |
| Hora com resposta; materializada; via `paridade_recusada`; com ou sem recusa/batida | `{'estado': 'recusada_paridade', 'detalhe': 'encerrada SEM criar batida: fato de chao prevalece; quem discordar corrige a batida vizinha'}` | `app/chamados/services/materializacao.py:440–447` |
| Hora com resposta; materializada; **qualquer outra via**, inclusive vazia; com ou sem recusa/batida | `{'estado': 'aplicada', 'detalhe': ''}` | `app/chamados/services/materializacao.py:448` |
| Hora com resposta; não materializada; derivação não produz timestamp | `invalida`; detalhe = primeiro erro, ou vazio | `app/chamados/services/materializacao.py:449–452` |
| Não materializada; timestamp derivado; batida encontrada na janela e aprovada | `ja_satisfeita`; detalhe explica que validar só encerra a pendência | `app/chamados/services/materializacao.py:453–457` |
| Não materializada; sem satisfação anterior; vizinha do mesmo tipo; plantio não preenche furo | `bloqueada_paridade`; detalhe informa o par quebrado | `app/chamados/services/materializacao.py:458–462` |
| Não materializada; demais casos | `aplicavel`; detalhe anuncia tipo e hora que validar registraria | `app/chamados/services/materializacao.py:463–465` |

`recusa_motivo` não participa dessa decisão. Sem `materializada_em`, a via também não participa; `respondida_em` e `validada_em` não são condições dessa função. (`app/chamados/services/materializacao.py:438–465`)

Além disso, a busca de satisfação usa `Batida.objects` sem excluir retratadas; a guarda de vizinhança também consulta batidas cruas. Portanto, essa previsão não equivale a uma consulta de “batida viva”. (`app/chamados/juizes.py:238–250`; `app/chamados/services/materializacao.py:95–104`)

**(b) Censo dos escritores**

O campo da pergunta nesta cópia é `PerguntaDisputa.materializada_em`. O próprio `help_text` ainda associa processamento a “batida criada + chamado fechado”, embora existam resoluções sem plantio. (`app/chamados/models.py:3502–3505`; `app/chamados/services/materializacao.py:1069–1086`)

A tabela distingue persistência física de chamadores operacionais: estes últimos chegam às portas abaixo. (`app/chamados/models.py:3156–3167`)

| Escritor físico | Forma e comportamento |
|---|---|
| `_escrever_carimbo` — `app/chamados/models.py:3180–3184` | Atribui instante, autor e via **em memória**; não cria Batida. |
| `carimbar` — `app/chamados/models.py:3235` | Persiste por `save(update_fields=campos)`; aplica o carimbo, consulta C6 e recusa se a pergunta continuar viva. Isso não é uma exigência física universal de Batida. (`:3211–3228`) |
| `carimbar_identidade` — `app/chamados/models.py:3273` | Persiste por `save(update_fields)`; aceita as vias de identidade, sem consultar C6. (`:3251–3273`) |
| `descarimbar` — `app/chamados/models.py:3291–3295` | Zera os três campos; pode devolver os campos ao chamador sem salvar. |
| `descarimbar_em_lote` — `app/chamados/models.py:3363–3365` | `QuerySet.update(materializada_em=None, materializada_por=None, via_resolucao='', ...)`; exclui linhas fora do fato. |

`_desfazer_em_memoria` restaura o estado anterior após recusa/exceção, sem persistência própria. (`app/chamados/models.py:3242–3249`)

| Via/função operacional que carimba | Cria Batida? Pode ficar sem correspondente viva? |
|---|---|
| `_materializar_ausencia_de_resposta`: `declaracao` ou `ausencia_dia` — `app/chamados/services/materializacao.py:555–567` | Cria **Ausencia**, carimba perguntas irmãs; não cria Batida para elas. Pode resolver sem Batida. (`:527–535`) |
| `_materializar_resposta_hora`: `paridade_recusada` — `app/chamados/services/materializacao.py:906–931` | Não planta; exige marco aceso e retorna após o carimbo de identidade. O estado de aplicação distingue essa via. (`:444–447`) |
| `_carimbar_recusa`: `fato_ocupado` — `app/chamados/services/materializacao.py:1095–1109` | Não cria Batida. Só `COLISAO`, com carimbo ainda nulo, gera esse carimbo. Outras recusas gravam somente o motivo; **uma recusa posterior preserva o carimbo anterior**. |
| `_materializar_uma`: `declaracao` ou `via_pelo_fato`, com fallback `chamado_encerrado` — `app/chamados/services/materializacao.py:1267–1287` | Hora cria ou reutiliza; `data_hora` registra Batida; escolha resolve expressamente sem Batida. (`:865–877`, `:932–940`, `:1012–1025`, `:1069–1086`) |
| `materializar_pendentes_aprovadas`: `juiz_aprovou` — `app/chamados/services/materializacao.py:1424–1438` | Não cria; consulta lastro e carimba. O juiz admite resoluções sem testemunha física da pergunta. (`app/chamados/juizes.py:682–710`) |
| `lavrar_sem_turno_aberto`: `sem_turno_aberto` — `app/chamados/reconciliador.py:363–406` | Não cria; pode encerrar sem Batida correspondente. |
| `retratar_perguntas_absolvidas_por_celula`: `celula_absolveu` — `app/chamados/reconciliador.py:550–596` | Não cria; a absolvição da célula fundamenta o carimbo. Pode terminar sem Batida. |
| `_resolver_perguntas_por_chamado_resolvido`, ramo destino — `app/chamados/services/disputa_emissao.py:1987–2009` | Não cria; usa `via_pelo_fato` e `resposta='validar'`. A função declara encerramento sem plantio. (`:1907–1913`) |
| Mesma função, ramo sem credor — `app/chamados/services/disputa_emissao.py:2065–2082` | Não cria; usa `via_pelo_fato` ou `chamado_encerrado`, com `resposta='responder'`. |
| `fechar_por_admin`: `admin_decidiu` — `app/chamados/models.py:3133–3147` | Não cria. Chamado pelo encerramento administrativo/desligamento da disputa (`app/chamados/models.py:1982–1985`), fechamento do fio do dia (`app/ponto/services/veredito_celula.py:190`) e aceite de folga (`app/chamados/services/validacao.py:314`, `:354`). Pode resolver por folga, sem Batida. |
| `DisputaSupervisao.remover_motivo`: `motivo_retratado` — `app/chamados/models.py:2118–2126` | Não cria; pode manter instante e autor do carimbo anterior. |
| `fio.retratar_duplicata`: `duplicata` — `app/chamados/services/fio.py:1384–1401` | Não cria; registra referência à canônica e pode preservar carimbo anterior. |
| `encerrar_fantasma`: `fantasma` — `app/chamados/models.py:3378–3386` | Não cria; usa `carimbar`/C6. Chamador em `app/chamados/reconciliador.py:1148`. |

`cadastro_folga` não possui uma atribuição literal separada ao carimbo: nasce de `via_pelo_fato`, quando há agrupamento sob limbo/folga e nenhuma batida apurável no dia; o mesmo derivador pode retornar `adesao`, `ausencia_dia`, `completude_turno` ou `chamado_encerrado`. Entra pelos chamadores desse derivador listados acima. (`app/chamados/juizes.py:1772–1816`)

Sobre **plantio desfeito pelo juiz**, o caminho atual de `_materializar_uma` reprova dentro do `atomic`: desfaz o plantio e o carimbo que pertencerem àquela transação. Depois, o `finally` grava a recusa, cuja porta relê e preserva eventual carimbo já existente. Não se pode concluir que um carimbo criado naquele mesmo `atomic` sobreviva ao rollback. (`app/chamados/services/materializacao.py:1224–1249`, `:1281–1303`, `:1095–1109`)

Há também retração posterior: `retratar_batida` grava `aprovada=False` e `retratada_em`; essa escrita não altera diretamente o carimbo da pergunta. Assim, plantio ocorrido e Batida atualmente viva são fatos diferentes. A sequência concreta das perguntas 43643/44051 permanece restrita ao relato do anexo. (`app/ponto/registro_batida.py:269–282`) [não verificado]

Os caminhos de **limpeza** também integram o censo:

| Chamador | Escrita alcançada |
|---|---|
| `ressuscitar_perguntas` | Lote em `app/chamados/models.py:1613`. |
| `reabrir_para_correcao` | Lote em `app/chamados/services/disputa_emissao.py:1648`. |
| `desfazer_carimbo_sem_lastro` | `descarimbar` em `app/chamados/services/relavra_via.py:242–246`. |
| `reabrir_fantasma` | `descarimbar` em `app/chamados/models.py:3395–3397`; comando chama em `app/chamados/management/commands/reabrir_orfas_enterradas.py:157`. |
| `reabrir_por_resposta` | Limpa em memória e devolve campos em `app/chamados/models.py:3412–3414`; escritor da resposta os incorpora em `app/chamados/services/disputa_emissao.py:1499`. |
| `reabrir_por_competencia` | `descarimbar` em `app/chamados/models.py:3419–3421`; chamador em `app/chamados/services/competencia_trancada.py:279`. |

A limpeza de `_universo_da_ressuscitacao` ocorre apenas numa cópia em memória para julgamento. (`app/chamados/models.py:1541–1556`)

As demais formas pedidas ficaram assim:

- **`bulk_update`:** nenhum escritor operacional adicional do carimbo foi localizado. O detector cobre essa forma, kwargs indiretos, construtores, `update` e `setattr`; a exaustividade para reflexão arbitrária permanece [não verificado], pois o detector declara limites para `__dict__` e nomes externos. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:99–115`, `:174–197`)
- **Migrations:** `0016` adiciona o campo, sem materialização de dados; `0051` atualiza colaborador/instância, e `0053` consulta colisões. Não foi localizada migration de dados escrevendo esse carimbo. (`app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:21–24`; `app/chamados/migrations/0051_decisao_pergunta.py:25–27`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:36–46`) [não verificado quanto a escritor dinâmico não identificável]
- **Passivo:** chama a mesma `_materializar_uma`; `CAMPOS_REVERSAO` é fotografia para saída, não restauração do carimbo nesse serviço. (`app/chamados/services/passivo_validado.py:39`, `:64–83`, `:370`, `:522`)
- **Vias sem escritor temporal novo:** `competencia_trancada` mantém apenas acervo/leitura; `fato_materializado` só troca a via. (`app/chamados/models.py:3149–3154`, `:3367–3375`)
- **Homônimo:** `RegularizacaoExterna.materializada_em` pertence a outro modelo; seu escritor é `app/chamados/services/regularizacao_ext.py:199–201`. Pode apenas remover/corrigir Batida, sem criar outra. (`app/chamados/models.py:4128`; `app/chamados/services/regularizacao_ext.py:132–164`)
- **Testes:** fixtures e mutantes não foram contados como vias operacionais; exemplificam gravação artificial de estados, como a pergunta materializada sem plantio do teste da função. (`app/chamados/tests/test_estado_aplicacao.py:37–39`, `:66–72`)

**(c) Leitores e apresentação**

| Caminho | O que lê/mostra |
|---|---|
| Reexportação | `chamados.models` exporta a função real. (`app/chamados/models.py:4231`) |
| `montar_chamados_fio` | Único chamador operacional localizado: calcula `p.estado_aplicacao`; sem disputa ou em exceção, atribui `None`. (`app/chamados/services/fio.py:373–376`) |
| Modal administrativo | Monta o fio e renderiza o modal; o fragmento usa o mesmo montador. (`app/chamados/views.py:889`, `:927–931`; `app/chamados/services/fio.py:484`, `:534–535`) |
| Template consumidor | Exibe detalhe vermelho de `bloqueada_paridade` e azul de `ja_satisfeita`. Não imprime os outros estados nesse bloco. (`app/templates/chamados/partials/modal_fio.html:230–248`) |
| Precedência visual | `pendencia_admin` mostra pendência e motivo; senão, `validada_em` mostra **“✓ Validada”**. Logo, a legenda literal “aplicada” do relato não está confirmada por esse template. (`app/templates/chamados/partials/modal_fio.html:222–229`) |
| Meu Atendimento / fio em página inteira | Executam o montador, mas não foi localizado consumo visual desse atributo nos respectivos templates. (`app/chamados/views.py:569`, `:632–635`, `:677`, `:697–703`) [não verificado fora da cópia] |
| API de perguntas | Publica `p.estado` e `aguarda_admin`, não esse estado de aplicação. (`app/api/views.py:2550–2578`) |
| Contadores do modal | Derivam de fila de chamados, followups e disputas abertas; não desse retorno. (`app/chamados/views.py:900–912`) |
| Testes | Exercitam satisfação, paridade, aplicabilidade, aplicação e fora de escopo; há teste próprio da via recusada. (`app/chamados/tests/test_estado_aplicacao.py:44–72`; `app/chamados/tests/test_badge_via_paridade.py:30–32`) |

Não foi localizado contador operacional de `estado='aplicada'` nem outro consumidor desse retorno nas APIs da cópia. [não verificado quanto a clientes externos] [nao verificado]

**(d) Sonda proposta, somente como texto**

Universo: todas as perguntas `hora` de `juliani`, sem restringir empresa, competência, situação do colaborador ou disputa aberta. A função real decide quais são `aplicada`; tipos diferentes ficam fora. Resposta vazia e paridade recusada ficam fora do numerador pela própria função. (`app/chamados/services/materializacao.py:438–448`)

Batida viva usa `batidas_apuraveis`, incluindo pendentes do DP e excluindo retratadas. Ausência de FK sozinha não basta: o acervo anterior pode não ter `pergunta_origem`. A correspondência usa os auxiliares reais de slot, vínculo e janela. (`app/ponto/turnos.py:2868–2895`; `app/ponto/models.py:30–44`; `app/chamados/juizes.py:238–282`, `:344–377`)

Slot não derivável ou falha engolida fica **indeterminado**, separado do total sem correspondente demonstrável. Não uso `_pergunta_materializada_de_fato` como prova física, pois ele pode absolver sem Batida. (`app/chamados/juizes.py:682–708`; `app/core/observ.py:57–82`)

O texto abaixo não foi executado nem validado contra produção. Os comandos de dados são SELECT; `schema_context` pode emitir `SET search_path`, além do controle transacional. [não verificado]

```bash
PGOPTIONS='-c default_transaction_read_only=on -c default_transaction_isolation=repeatable\ read' \
PYTHONDONTWRITEBYTECODE=1 python manage.py shell <<'PY'
from collections import Counter, defaultdict
from django.db import connection, transaction
from django_tenants.utils import schema_context

from chamados.models import PerguntaDisputa
from chamados.services.materializacao import estado_aplicacao_pergunta
from chamados.juizes import (
    _slot_esperado,
    _plantio_na_hora_do_slot,
    _batida_existente_na_janela,
)
from ponto.turnos import batidas_apuraveis
from core.observ import engolidos

por_via = defaultdict(Counter)
estados = Counter()

with schema_context("juliani"), transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("""
            SELECT current_schema(),
                   current_setting('transaction_read_only'),
                   current_setting('transaction_isolation')
        """)
        schema, readonly, isolamento = cur.fetchone()

    assert schema == "juliani", schema
    assert readonly == "on", readonly
    assert isolamento == "repeatable read", isolamento

    perguntas = (
        PerguntaDisputa.objects.filter(tipo_resposta="hora")
        .select_related("disputa__colaborador", "chamado_destino")
        .order_by("pk")
    )

    for p in perguntas.iterator(chunk_size=1000):
        via = p.via_resolucao or "<vazia>"
        c = por_via[via]
        c["universo_hora"] += 1

        # A classificação vem integralmente da função real.
        r = estado_aplicacao_pergunta(p.disputa, p)
        estado = r["estado"] if r is not None else "<fora_escopo>"
        estados[(via, estado)] += 1
        if estado != "aplicada": [nao verificado]
            continue

        c["aplicada"] += 1 [nao verificado]
        vivas = batidas_apuraveis(p.disputa.colaborador)
        vinculadas = vivas.filter(pergunta_origem_id=p.pk)
        if not vinculadas.exists():
            c["sem_vinculo_vivo"] += 1

        antes = engolidos()["total"]
        ts, tipo = _slot_esperado(p)
        if engolidos()["total"] != antes or ts is None:
            c["indeterminada"] += 1
            continue

        corresponde = (
            _plantio_na_hora_do_slot(p, ts, vinculadas)
            or _batida_existente_na_janela(
                p.disputa.colaborador, ts, tipo, batidas=vivas
            ) is not None
        )
        chave = (
            "com_correspondente_viva" if corresponde
            else "sem_correspondente_viva"
        )
        c[chave] += 1

    assert not connection.needs_rollback
    for c in por_via.values():
        assert c["aplicada"] == ( [nao verificado]
            c["com_correspondente_viva"]
            + c["sem_correspondente_viva"]
            + c["indeterminada"]
        )

campos = (
    "universo_hora", "aplicada", "sem_vinculo_vivo", [nao verificado]
    "com_correspondente_viva", "sem_correspondente_viva",
    "indeterminada",
)
print("via", *campos, sep="\t")
total = Counter()
for via, c in sorted(por_via.items()):
    print(via, *(c[k] for k in campos), sep="\t")
    total.update(c)
print("<TOTAL>", *(total[k] for k in campos), sep="\t")

print("via\testado_real\tquantidade")
for (via, estado), n in sorted(estados.items()):
    print(via, estado, n, sep="\t")
PY
```

`sem_correspondente_viva` será a contagem conforme a correspondência atual do código; `sem_vinculo_vivo` é apenas diagnóstico adicional. Os indeterminados impedem declarar um total físico absoluto sem investigação complementar. (`app/chamados/juizes.py:238–282`, `:344–377`) **Resultado numérico: [não verificado].**

**(e) Encaixe, em uma linha:** **F4 da E4c**, pois o defeito está no leitor do materializador e F4 cobre materialização/passivo; RAIZ-DA-DISPUTA trata agregado, destinos, fechamento e pares. (`app/chamados/services/materializacao.py:431–448`; `app/docs/RELATO.md:254`, `:292`; `app/docs/BACKLOG.md:2031`)

O conteúdo literal de `logs/ESPINHA-BRIEFS/F4` está **[não verificado]**; o brief e a matriz aparecem referidos no registro documental da cópia. (`app/docs/RELATO.md:133`, `:254`)

## 16/11 11:45 · sessao f38ef04e825c · APP-LE-PROXIMA-BATIDA · sonda · origem=Code

**1. A saída fecha aritmeticamente.** Janela fornecida: `2026-11-09T14:42:13.404576+00:00` a `2026-11-16T14:42:13.404576+00:00`. Os valores abaixo são do envelope; execução, consulta e correspondência com a base: **[nao verificado]**.

| origem | provider | total | sem_accuracy | sem_accuracy_dentro_False | sem_accuracy_dentro_None |
|---|---|---:|---:|---:|---:|
| apk | - | 11 | 11 | 0 | 2 |
| sem_provider(pwa?) | - | 20 | 0 | 0 | 0 |
| **TOTAL calculado** | | **31** | **11** | **0** | **2** |

Conferência sobre o texto: `11 + 20 = 31`, igual ao TOTAL; `11 + 0 = 11` sem accuracy. A saída informa `total_erros 0`; comprovação independente desse contador: **[nao verificado]**.

**2. Existem fontes melhores de contexto, mas nenhuma identifica diretamente a origem de cada PingGeo persistido.** O modelo guarda colaborador, entrada e sinais técnicos; não tem `origem`, `canal`, `device_id` ou vínculo com aparelho. O comentário atribui os campos técnicos ao APK Kotlin e os descreve vazios em PWA. [app/ponto/models.py:1005](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/models.py:1005)

As três portas de criação da aplicação gravam `accuracy` recebido, opcional; ausência pode ficar `NULL`, permitido pelo modelo. [app/ponto/models.py:1017](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/models.py:1017)

| Porta | Quem escreve PingGeo | Leitura → gravação de accuracy |
|---|---|---|
| `/api/ping-geo/`, core | [app/api/views_core.py:951](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:951) | [864](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:864) → [957](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:957) |
| `/api/ping-geo/`, API completa | [app/api/views.py:2421](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2421) | [2334](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2334) → [2427](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2427) |
| `/ponto/ping-geo/`, web | [app/ponto/views.py:3065](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3065) | [3014](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3014) → [3071](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3071) |

As fontes alternativas têm estes limites:

- **`batida_entrada.canal`** registra `apk`/`pwa`, mas identifica a porta da **batida de entrada**, não a origem dos pings seguintes. [app/ponto/models.py:46](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/models.py:46), [app/ponto/models.py:1010](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/models.py:1010)
- **`DeviceColaborador.plataforma`** distingue app, APK, iOS e PWA. A API recebe `device_id` para validar a assinatura, mas não o persiste no ping. [app/colaboradores/models.py:868](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/colaboradores/models.py:868), [app/colaboradores/models.py:915](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/colaboradores/models.py:915), [app/api/views_core.py:879](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:879), [app/api/views_core.py:951](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:951)
- **Cliente declarado na requisição/sessão:** existem `X-Hasner-Client`, `plataforma_sessao` e `cliente_app`; esses valores não são gravados pelos escritores acima. O app também abre sessão Django em WebView, portanto a porta web não comprova PWA. [app/core/middleware.py:117](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/core/middleware.py:117), [app/core/views.py:1280](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/core/views.py:1280)
- **Assinatura é indício insuficiente:** ausência retorna sucesso com rótulo `pwa_sem_assinatura`; assinatura inválida gera warning e a criação prossegue. [app/api/views_core.py:64](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:64), [app/api/views_core.py:885](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:885)

O PWA desta cópia envia `accuracy: pos.coords.accuracy`, sem provider/assinatura. A origem real dos 31 pings continua **[nao verificado]**; mantenho os rótulos inferidos da sonda. [app/static/pwa.js:404](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/static/pwa.js:404)

Fora das portas de captura, há criação em teste sem accuracy, máscara que altera apenas coordenadas e comando que apaga pings. [app/ponto/tests/test_selo_presenca_lote.py:54](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/tests/test_selo_presenca_lote.py:54), [bin/sombra_regras_pessoais.py:79](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/bin/sombra_regras_pessoais.py:79), [bin/sombra_mascara_pessoal.py:35](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/bin/sombra_mascara_pessoal.py:35), [app/ponto/management/commands/zerar_dados_operacionais.py:98](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/management/commands/zerar_dados_operacionais.py:98)

**3. Confirmada para o alerta de fora gerado pelo próprio ping:** fora do raio efetivo + `accuracy=None` resulta em `gps_incerto`; as portas persistem `dentro_geofence=None` e exigem `is False` para alertar. [app/ponto/services/geofence.py:114](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/services/geofence.py:114)

| Porta | Conversão, juiz e estado persistido | Decisão de alerta |
|---|---|---|
| Core | [app/api/views_core.py:931](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:931) | Atual False, não mock, dois últimos não mock em 2h ambos False → `push_dp`. [967](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:967) |
| API completa | [app/api/views.py:2401](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2401) | Mesma condição → `push_dp`. [2437](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2437) |
| Web | [app/ponto/views.py:3045](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3045) | Atual False, não mock, ao menos dois falsos no turno → chamado e `push_supervisao`. [3081](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3081) |

Precisões necessárias:

- No web, a consulta filtra os falsos antes de contar: um ping incerto não dispara, mas também não interrompe essa contagem histórica. [app/ponto/views.py:3082](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3082)
- O HTTP pode responder `dentro_geofence=false` enquanto o registro fica `None`; resposta e estado persistido são distintos nas três portas. [app/api/views_core.py:943](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:943), [app/api/views.py:2413](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2413), [app/ponto/views.py:3057](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/views.py:3057)
- Os dois `dentro_None` fornecidos não comprovam dois pings fora: intervalo também força `None`. [app/api/views_core.py:948](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:948)
- “Não alerta” não deve ser generalizado para **batidas**: esse outro fluxo cria `AlertaGeofence(tipo='gps_incerto')`. [app/ponto/services/geofence.py:301](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/ponto/services/geofence.py:301)

## 16/11 11:45 · sessao d83f05d050b7 · E4C-F1-NOVA-POUSO-B1 · verificador · origem=Code

**Veredito: o bloqueante documental está fechado na cópia; a certificação integral do delta fica [não verificado].** A frase agora está na fonte e sobrevive à regeneração (`app/docs/CORTES.json:1871`, `bin/gerar_cortes.py:107`). A cópia disponível não contém histórico Git: os dois diffs e `git log -2 --format=%B 99eccedf9` não puderam ser conferidos **[não verificado]**.

**1) FECHADO — frase na fonte e preservada pelo gerador.**

Quem **escreve** o Markdown é `bin/gerar_cortes.py`: lê `app/docs/CORTES.json`, renderiza todas as entradas e sobrescreve `app/docs/CORTES.md`; também atualiza o RELATO (`bin/gerar_cortes.py:23`, `bin/gerar_cortes.py:31`, `bin/gerar_cortes.py:106`, `bin/gerar_cortes.py:116`, `bin/gerar_cortes.py:136`).

Quem **lê a frase exigida** é o selo: extrai autoridades de `core/juizes.py`, subtrai a base e procura literalmente, sem distinguir maiúsculas, `corte Ronald: juiz <nome> nasce` no Markdown (`bin/tests/test_juiz_novo_tem_corte.sh:28`, `bin/tests/test_juiz_novo_tem_corte.sh:38`, `bin/tests/test_juiz_novo_tem_corte.sh:41`).

A conferência em memória encontrou **83 autoridades extraídas pelo selo, 54 na base e 29 exigindo frase; nenhuma ficaria sem frase após regenerar**. Abaixo estão todas as 29 e a localização da frase na fonte, seguindo a extração e comparação do selo (`bin/tests/test_juiz_novo_tem_corte.sh:28`, `bin/tests/test_juiz_novo_tem_corte.sh:38`).

| Autoridade exigida | Frase no JSON |
|---|---|
| `chamados/juizes.py::dia_da_pergunta_trancado` | `app/docs/CORTES.json:1729` |
| `chamados/juizes.py::dia_sem_trabalho` | `app/docs/CORTES.json:1736` |
| `chamados/juizes.py::fio_vivo` | `app/docs/CORTES.json:614` — campo `estado` |
| `chamados/juizes.py::pergunta_respondida` | `app/docs/CORTES.json:1855` e `app/docs/CORTES.json:1871` |
| `chamados/juizes.py::porta_alcancavel_do_dia` | `app/docs/CORTES.json:1771` |
| `chamados/models.py::DisputaSupervisao.pode_fechar` | `app/docs/CORTES.json:765` |
| `chamados/models.py::DisputaSupervisao.respostas_sem_veredito` | `app/docs/CORTES.json:1414` |
| `chamados/services/fabrica_da_celula.py::pauta_cadastro_existente` | `app/docs/CORTES.json:1764` |
| `chamados/services/tarefa_do_dia.py::tarefa_do_dia` | `app/docs/CORTES.json:1785` |
| `colaboradores/services/calendario.py::resolver_dia_oferecido` | `app/docs/CORTES.json:758` |
| `colaboradores/services/desligamento.py::esta_desligado` | `app/docs/CORTES.json:1743` |
| `escala/alimentacao.py::vinculo_cobre_a_data` | `app/docs/CORTES.json:875` |
| `escala/alimentacao.py::vinculo_do_dia` | `app/docs/CORTES.json:854` |
| `escala/servico_jornada.py::eh_turno_partido` | `app/docs/CORTES.json:477` |
| `folha/export.py::competencia_entregue` | `app/docs/CORTES.json:786` |
| `folha/export.py::competencia_fechada_para_celula` | `app/docs/CORTES.json:791` |
| `ponto/juiz_batida.py::periodos_do_dia` | `app/docs/CORTES.json:469` |
| `ponto/precedencia.py::folga_lancada_do_dia` | `app/docs/CORTES.json:1750` |
| `ponto/registro_batida.py::selfie_ausente` | `app/docs/CORTES.json:846` — campo `estado` |
| `ponto/services/furos_vetados.py::motivo_do_veto` | `app/docs/CORTES.json:1757` |
| `ponto/services/geofence.py::classificar_posicao` | `app/docs/CORTES.json:841` |
| `ponto/services/geofence.py::tem_gps` | `app/docs/CORTES.json:1792` |
| `ponto/status_dia.py::status_do_dia_lavrado` | `app/docs/CORTES.json:1778` |
| `ponto/supra_juiz.py::pode_executar_csf` | `app/docs/CORTES.json:1407` |
| `ponto/turnos.py::dia_encerrado` | `app/docs/CORTES.json:533` |
| `ponto/turnos.py::lembrete_fim_turno` | `app/docs/CORTES.json:868` |
| `ponto/turnos.py::par_completo` | `app/docs/CORTES.json:541` |
| `ponto/turnos.py::periodo_da_parcial_no_dia` | `app/docs/CORTES.json:1125` |
| `ponto/turnos.py::turno_aberto_ha` | `app/docs/CORTES.json:861` |

As duas frases em `estado` também sobrevivem, porque o gerador reproduz esse campo tanto na tabela dos pendentes quanto na tabela completa (`bin/gerar_cortes.py:90`, `bin/gerar_cortes.py:107`).

O Markdown inteiro coincidiu, em memória, com `markdown(JSON, agora=2026-11-16 11:38)`; portanto o conteúdo atual é integralmente reproduzível pelo gerador (`bin/gerar_cortes.py:97`, `app/docs/CORTES.md:3`). O selo shell não foi executado: ele cria e remove um temporário, contrariando a regra desta revisão; sua execução literal fica **[não verificado]** (`bin/tests/test_juiz_novo_tem_corte.sh:57`, `bin/tests/test_juiz_novo_tem_corte.sh:60`).

**2) [NÃO VERIFICADO] — preservação histórica dos `.py` pelo merge.**

Sem os objetos Git, não foi possível comparar, por arquivo de `py_b1.txt`, `ab616d1c7..51678cbb2` contra `f76eee7c6..99eccedf9`. Logo, não certifico que nenhum conteúdo da obra mudou **[não verificado]**. O RELATO registra a certificação antiga e um ensaio do comboio, sem substituir esses diffs (`app/docs/RELATO.md:18`, `app/docs/RELATO.md:19`).

No arquivo compartilhado, o retrato **atual** contém os registros dos sítios da obra e do main: `_dia_da_batida`, `_dia_do_contexto_fraco` e `regularizar_retorno`, com suas linhas correspondentes (`app/core/tests/test_medida_turno_do_par.py:258`, `app/core/tests/test_medida_turno_do_par.py:259`, `app/core/tests/test_medida_turno_do_par.py:339`).

Executei os **35 testes dessa catraca em memória: zero falhas e zero erros**, pelo procedimento documentado, sem banco ou bytecode. Isso verifica a coerência atual de identidades, composição e linhas; não prova equivalência histórica dos dois diffs (`app/core/tests/test_medida_turno_do_par.py:20`, `app/core/tests/test_medida_turno_do_par.py:541`, `app/core/tests/test_medida_turno_do_par.py:557`).

As duas intenções funcionais estão presentes no resultado: posto e histórico no mesmo `atomic`; ressurreição conserva a trilha, zera o par inteiro e evita fabricar REABERTURA (`app/colaboradores/views.py:333`, `app/colaboradores/views.py:335`, `app/chamados/models.py:1613`, `app/chamados/models.py:1622`, `app/chamados/models.py:1629`). **Que o merge preservou exatamente ambas continua [não verificado].**

**3) HÁ INTERAÇÃO — os caminhos examinados são compatíveis na leitura estática.**

Existe compartilhamento explícito da porta de travas: o serviço de colaboradores chama `PerguntaDisputa.trava_na_ordem` para travar colaboradores; a ressurreição usa a mesma autoridade com Colaborador → Disputa → Chamado → Pergunta (`app/colaboradores/services/colaborador.py:72`, `app/colaboradores/services/colaborador.py:80`, `app/chamados/models.py:1612`, `app/chamados/models.py:2770`, `app/chamados/models.py:2779`, `app/chamados/models.py:2793`, `app/chamados/models.py:2797`).

| Entidade/campos | Quem escreve | Quem lê |
|---|---|---|
| **Pergunta**: `respondida_em`, `resposta_colab`, validação e materialização | Ressurreição e correção passam por `descarimbar_em_lote`, que efetiva o update (`app/chamados/models.py:1629`, `app/chamados/services/disputa_emissao.py:1648`, `app/chamados/models.py:3391`). | `_C21` aponta para `pergunta_respondida`; a face SQL serve `q_vivas` e a fila de validação (`app/core/juizes.py:306`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1580`, `app/chamados/models.py:3481`, `app/chamados/juizes.py:1751`). |
| **DecisaoPergunta**: ato, instante e resposta anterior | `registrar` grava as linhas; ressurreição preserva RESPOSTA/VALIDAÇÃO, correção registra seu ato, e fechamento pode registrar SILENCIO (`app/chamados/models.py:4120`, `app/chamados/models.py:1613`, `app/chamados/services/disputa_emissao.py:1631`, `app/chamados/models.py:2005`). | `tarefa_do_dia` carrega a sequência ordenada e julga pelo ato (`app/chamados/services/tarefa_do_dia.py:421`, `app/chamados/services/tarefa_do_dia.py:142`). |
| **CelulaDia**: veredito, ata e trabalho | A porta `lavrar_veredito` escreve o veredito; reabertura/ressurreição emitem o evento para acordar células (`app/escala/models.py:1826`, `app/ponto/portas/celula.py:1093`, `app/chamados/models.py:3358`). | `q_vivas` lê veredito/ata; `tarefa_do_dia` carrega as células; telas do main também as leem (`app/chamados/models.py:3488`, `app/chamados/models.py:3493`, `app/chamados/services/tarefa_do_dia.py:406`, `app/core/views.py:395`). |
| **HistoricoVinculo**: antes/depois, vigência, posto e autor | `registrar_mudanca`/`registrar_evento_de_vinculo`, chamados por posto e desligamento (`app/colaboradores/services/historico_vinculo.py:7`, `app/colaboradores/services/historico_vinculo.py:25`, `app/colaboradores/views.py:335`, `app/colaboradores/services/desligamento.py:138`). | A tela de detalhe lê o histórico (`app/colaboradores/views.py:306`). Ausência global de outros leitores **[não verificado]**. |

A interação mais relevante é **desligamento → fechamento → SILENCIO → tarefa**: desligamento preserva disputas com respostas sem veredito; nos demais casos chama `fechar`, que registra SILENCIO. A tarefa distingue esse ato de REABERTURA, que devolve ao colaborador. A B1 conserva essa distinção ao ressuscitar (`app/colaboradores/services/desligamento.py:164`, `app/chamados/models.py:2005`, `app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/models.py:1622`).

`vincular_posto` grava posto e trilha no mesmo `atomic`; o save preserva a situação lida sob trava. A regeneração antiga por simples troca do posto cadastral está explicitamente aposentada nesse serviço (`app/colaboradores/views.py:333`, `app/colaboradores/services/colaborador.py:152`, `app/colaboradores/services/colaborador.py:158`, `app/colaboradores/services/colaborador.py:118`).

Também há caminho indireto por INSS: alteração de situação e criação da ausência dentro do mesmo `atomic`, seguida do signal que solicita julgamento das células (`app/ponto/views.py:2469`, `app/ponto/views.py:2472`, `app/ponto/views.py:2489`, `app/ponto/signals.py:143`). Execução concorrente e efeitos no hash em produção ficam **[não verificado]**.

**4) ESTRUTURA COERENTE; proveniência dos logs e ausência histórica de dado pessoal novo [não verificado].**

O patch fornecido acrescenta **19 entradas**, conferidas na fonte:

| Grupo | Entradas conferidas |
|---|---|
| 10 restaurações | `app/docs/CORTES.json:1725`, `1732`, `1739`, `1746`, `1753`, `1760`, `1767`, `1774`, `1781`, `1788` |
| 8 avais | `app/docs/CORTES.json:1802`, `1810`, `1818`, `1826`, `1834`, `1842`, `1850`, `1858` |
| Nascimento de `pergunta_respondida` | `app/docs/CORTES.json:1866` |

A validação em memória encontrou **255 IDs únicos**, nenhum campo `id/quando/estado/fatia/frase` vazio nas 19 adições e datas aceitas pela regra do gerador, inclusive minutos com `x` (`app/docs/CORTES.json:1725`, `app/docs/CORTES.json:1871`, `bin/gerar_cortes.py:35`).

Há duas ressalvas de campos:

- As **10 restaurações não têm chave `onde`**; guardam referências históricas em `estado`/`frase`. Portanto, se `onde` for obrigatório em toda entrada, essas dez não atendem. O gerador não exige esse campo (`app/docs/CORTES.json:1725`, `app/docs/CORTES.json:1788`, `bin/gerar_cortes.py:107`).
- As outras **9 possuem `onde` em `logs/AVAIS-1515/`**. Existência e conteúdo desses anexos ficam **[não verificado]**; `logs/` é ignorado pelo repositório (`app/docs/CORTES.json:1806`, `app/docs/CORTES.json:1814`, `app/docs/CORTES.json:1822`, `app/docs/CORTES.json:1830`, `app/docs/CORTES.json:1838`, `app/docs/CORTES.json:1846`, `app/docs/CORTES.json:1854`, `app/docs/CORTES.json:1862`, `app/docs/CORTES.json:1870`, `.gitignore:53`).

Além das adições, há **13 alterações de estado**, todas citando o censo `logs/CORTES-RECEBIDO-FORA-DA-PAUSA/c1.r`; a comprovação desse censo fica **[não verificado]** (`app/docs/CORTES.json:902`, `909`, `916`, `923`, `1021`, `1028`, `1056`, `1063`, `1070`, `1091`, `1098`, `1154`, `1161`).

Quanto a dados pessoais, a associação de primeiro nome a uma pergunta na restauração já aparece em outros documentos da cópia; os interlocutores nomeados nos avais também têm ocorrências anteriores no JSON (`app/docs/CORTES.json:1771`, `app/docs/BACKLOG.md:961`, `app/docs/RELATO.md:2856`, `app/docs/CORTES.json:1807`, `app/docs/CORTES.json:1188`, `app/docs/CORTES.json:1223`). Não identifiquei CPF, telefone ou email nas 19 adições examinadas; certificar ausência de dado pessoal adicional contra as árvores anteriores fica **[não verificado]**.

**5) PARCIALMENTE VERIFICADO — nenhuma dessas falhas demonstrada no conteúdo atual; histórico dos commits pendente.** [nao verificado]

| Classe de reprovação | Veredito |
|---|---|
| Frase só no derivado | **Ausente para as 29 autoridades cobradas:** todas têm lastro no JSON; a da obra está em duas entradas (`app/docs/CORTES.json:1855`, `app/docs/CORTES.json:1871`; tabela da resposta 1). |
| Segundo juiz duplicando registrado | **Não identificado no caminho examinado:** `_C21` registra `pergunta_respondida`; `q_respondida_com_texto` é sua face SQL, com teste de igualdade por PK. A execução desse teste com banco fica **[não verificado]** (`app/core/juizes.py:306`, `app/chamados/juizes.py:1575`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:73`). |
| Derivado escrito à mão | **Nenhuma divergência atual:** o Markdown inteiro é reproduzível pelo gerador. Que o commit efetivamente o produziu executando o gerador fica **[não verificado]** (`bin/gerar_cortes.py:97`, `app/docs/CORTES.md:3`). |
| Hash errado no corpo do commit | **[Não verificado]** — os corpos dos dois commits não estão disponíveis nesta cópia sem Git. | [nao verificado]

A prova das **283 no hash novo**, a suite inteira desse hash e a aprovação opus do delta permanecem **[não verificado]**. A prova registrada no RELATO pertence a `51678cbb2`, enquanto o aval exige repeti-la no hash que pousa (`app/docs/RELATO.md:19`, `app/docs/CORTES.json:1855`).

## 16/11 11:44 · sessao d3ef81fb503c · LOGIN-PUBLICO-HERDA-SESSAO · censo · origem=Code

(a) **Confirmo a premissa no código:** os dois `api_login` são `AllowAny`, aceitam POST e não declaram `authentication_classes`; o padrão contém `JWTComCredencial` seguido de `SessionAuthentication`. Fontes: [views_core.py:188](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:188), [views.py:202](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:202), [base.py:32](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/settings/base.py:32).

O módulo padrão de inicialização é `config.settings.saas`, que importa `base`. O seletor `HASNER_TENANT_URLCONF` muda o **URLconf**, preservando essa cadeia de settings; seu fallback é `config.urls`. Não encontrei sobrescrita de `REST_FRAMEWORK` nessa cadeia. Fontes: [wsgi.py:14](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/wsgi.py:14), [saas.py:10](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/settings/saas.py:10), [saas.py:76](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/settings/saas.py:76).

| Kernel previsto na cópia | URLconf | API |
|---|---|---|
| `saas_core` | `config.urls_core`, selecionado pelo ambiente | `api.urls_core` → `views_core`: [urls_core.py:6](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/urls_core.py:6), [api/urls_core.py:5](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls_core.py:5) |
| `saas_ui` | `config.urls`, fallback | `api.urls` → `views`: [urls.py:37](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/urls.py:37), [api/urls.py:2](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls.py:2) |

Esse desenho também está registrado em [test_rota_pede_sessao.py:50](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/core/tests/test_rota_pede_sessao.py:50). O Caddy encaminha `/api/auth/*` ao core; portanto, no desenho versionado, o login externo chega ao core, embora a UI também tenha implementação: [Caddyfile:8](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/Caddyfile:8).

Os valores **efetivos** de `DJANGO_SETTINGS_MODULE` e `HASNER_TENANT_URLCONF` dos containers são [nao verificado]. O entrypoint usa `setdefault`, e o compose de produção e os arquivos de ambiente são ignorados pelo versionamento: [wsgi.py:14](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/wsgi.py:14), [.gitignore:15](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/.gitignore:15), [.gitignore:40](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/.gitignore:40).

(b) **Censo local: três implementações públicas sem declaração de autenticação.** Nos três blocos, também não há `@authentication_classes` nem `@csrf_exempt` explícito:

| Kernel | Endpoint e método de escrita | Permissão e implementação | Rota |
|---|---|---|---|
| core | **POST** `/api/auth/login/` | `AllowAny`: [views_core.py:188](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:188) | [api/urls_core.py:9](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls_core.py:9) |
| UI | **POST** `/api/auth/login/` | `AllowAny`: [views.py:202](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:202) | [api/urls.py:60](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls.py:60) |
| UI | **POST** `/api/diag/cam/` | `permission_classes([])`: [views.py:2642](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:2642) | [api/urls.py:102](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls.py:102) |

Há uma ressalva para fechar o universo de classes: os dois `HasnerTokenRefreshView` herdam de `TokenRefreshView` externo. A ausência de atributo **local** não prova que herdem o padrão do projeto. Estão em [views_core.py:125](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:125) e [views.py:69](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:69), roteados em [api/urls_core.py:12](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls_core.py:12) e [api/urls.py:63](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/urls.py:63). Os atributos efetivos da base externa são [nao verificado]; a cópia declara a dependência, mas não traz sua implementação: [requirements.txt:15](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/requirements.txt:15).

Exclusões relevantes:

- `api_app_version` é função Django que retorna `JsonResponse`, sem `@api_view`: [views.py:3042](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:3042).
- `api_mensageria_push` tem `@csrf_exempt` e `@require_POST`, mas é view Django, fora do censo DRF: [views_mensageria.py:476](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_mensageria.py:476).
- `comunicados.api_pendentes` é DRF, mas exige `IsAuthenticated` e aceita apenas GET: [comunicados/views.py:83](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/comunicados/views.py:83).

(c) **Para cada uma das três linhas, a previsão é bloqueio por `CSRF Failed`**, supondo sessão reconhecida de usuário ativo, ausência de `Authorization` e verificações CSRF ativas. Isso coincide com o relato fornecido para login; reprodução nesta cópia e ocorrência em `diag/cam` são [nao verificado].

O mecanismo esperado é: autenticação ocorre antes da permissão; sem JWT, `SessionAuthentication` encontra o usuário da sessão e chama `enforce_csrf`; a falta do token interrompe a requisição antes do handler. `AllowAny` e permissões vazias não desligam autenticação. A isenção de CSRF aplicada pelo wrapper DRF tampouco elimina essa checagem interna. **A implementação desse mecanismo não está na cópia; portanto, sua confirmação na versão instalada é [nao verificado].** A inclusão de SessionAuthentication está comprovada em [base.py:39](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/settings/base.py:39); a versão declarada do DRF está em [requirements.txt:14](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/requirements.txt:14).

(d) **Não localizei cobertura da combinação login + sessão válida + CSRF ativo; cobertura global dessa combinação: [nao verificado].** Os testes encontrados deixam lacunas concretas: [nao verificado]

| Teste existente | O que monta | [nao verificado]
|---|---|
| [test_api_login.py:58](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/tests/test_api_login.py:58) | `APIClient()` novo por POST, sem estabelecer sessão; verifica sucesso e tokens em `:167`. |
| [test_login_campos.py:27](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/tests/test_login_campos.py:27) | `Client()` novo, sem ativar checagem CSRF. |
| [test_contrato_trocar_senha.py:127](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/core/tests/test_contrato_trocar_senha.py:127) | Chamada direta ao login core com `APIRequestFactory`, sem montar sessão. |
| [test_api_mensageria_push.py:67](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/tests/test_api_mensageria_push.py:67) | Ativa CSRF, mas testa mensageria, sem cookie de sessão. |

A suite ainda usa `config.urls_test`, que concatena core antes da UI. Assim, testar apenas `/api/auth/login/` nessa configuração não cobre as duas implementações: [settings_test.py:9](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/settings_test.py:9), [urls_test.py:11](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/config/urls_test.py:11).

Esboço do RED, **não criado nem executado [nao verificado]**. Fixtures: dois colaboradores ativos distintos, A para a sessão e B para as credenciais:

```python
for urlconf in ("config.urls_core", "config.urls"):
    with self.subTest(urlconf=urlconf):
        with override_settings(ROOT_URLCONF=urlconf):
            client = APIClient(enforce_csrf_checks=True)
            client.force_login(usuario_a)

            self.assertIn(settings.SESSION_COOKIE_NAME, client.cookies)
            self.assertEqual(
                client.session["_auth_user_id"], str(usuario_a.pk)
            )
            client.cookies.pop(settings.CSRF_COOKIE_NAME, None)

            # Sem Authorization, X-CSRFToken ou csrfmiddlewaretoken.
            response = client.post(
                "/api/auth/login/",
                {"username": usuario_b.username, "password": senha_b},
                format="json",
                HTTP_HOST="juliani.hasner.com.br",
            )

            self.assertEqual(response.status_code, 200, response.content)
            self.assertEqual(
                response.data["colaborador"]["id"], colaborador_b.pk
            )
            self.assertTrue(
                {"access", "refresh"} <= response.data["tokens"].keys()
            )
```

A expectativa de responder pelas credenciais de B acompanha os dois handlers, que consultam `autenticar_colaborador` com os campos do corpo: [views_core.py:195](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views_core.py:195), [views.py:209](/home/ronald/codex_ws/f76eee7c607d244e06e0e9097806c255b868a487/app/api/views.py:209). O RED esperado antes da correção é falhar na exigência de 200 por recusa CSRF; resultado executado: [nao verificado].

## 16/11 11:44 · sessao 55cbce51d5ec · - · - · origem=Ronald (tmux)

Proposta de sonda, **não executada**. [não verificado]

A ausência de `pergunta_origem` não basta: o campo também fica vazio em batidas de chão e plantios anteriores a 06/09 (`app/ponto/models.py:30-44`). A autoridade de batidas vivas é `batidas_apuraveis`, que inclui pendentes do DP e exclui retratadas (`app/ponto/turnos.py:2868-2895`).

Para correspondência, usar os auxiliares reais: `_slot_esperado` delega a derivação ao materializador (`app/chamados/juizes.py:344-377`); `_plantio_na_hora_do_slot` verifica a hora do vínculo preservando o overnight (`app/chamados/juizes.py:253-282`); `_batida_existente_na_janela` aceita QuerySet alimentado (`app/chamados/juizes.py:238-250`). Não usar `_pergunta_materializada_de_fato`: vias sem testemunha podem retornar `True` sem batida (`app/chamados/juizes.py:682-708`).

Universo proposto: todas as perguntas `tipo_resposta='hora'` do schema `juliani`, sem corte de empresa, competência, situação do colaborador ou estado da disputa; cada pergunta passa pela função real. Perguntas de `data_hora`, texto e escolha ficam fora. Resposta vazia e paridade recusada não entram no numerador porque a função retorna respectivamente `None` e `recusada_paridade` (`app/chamados/services/materializacao.py:438-448`). Slot não derivável sem vínculo vivo fica **indeterminado**, evitando transformar falta de informação em inexistência física (`app/ponto/models.py:38-44`; `app/chamados/juizes.py:755-764`).

```bash
PGOPTIONS='-c default_transaction_read_only=on -c default_transaction_isolation=serializable' \
PYTHONDONTWRITEBYTECODE=1 python manage.py shell <<'PY'
from collections import Counter, defaultdict

from django.db import connection, transaction
from django_tenants.utils import schema_context

from chamados.models import PerguntaDisputa
from chamados.services.materializacao import estado_aplicacao_pergunta
from chamados.juizes import (
    _slot_esperado,
    _plantio_na_hora_do_slot,
    _batida_existente_na_janela,
)
from ponto.turnos import batidas_apuraveis
from core.observ import engolidos

contagens = defaultdict(Counter)
estados = Counter()

with schema_context("juliani"), transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute("""
            SELECT current_schema(),
                   current_setting('transaction_read_only'),
                   current_setting('transaction_isolation')
        """)
        schema, leitura, isolamento = cursor.fetchone()
    assert schema == "juliani", schema
    assert leitura == "on", leitura
    assert isolamento == "serializable", isolamento

    perguntas = (
        PerguntaDisputa.objects
        .filter(tipo_resposta="hora")
        .select_related("disputa__colaborador", "chamado_destino")
        .order_by("pk")
    )

    for p in perguntas.iterator(chunk_size=1000):
        via = p.via_resolucao or "<vazia>"
        c = contagens[via]
        c["universo_hora"] += 1

        resultado = estado_aplicacao_pergunta(p.disputa, p)
        estado = resultado["estado"] if resultado is not None else "<fora_escopo>"
        estados[(via, estado)] += 1
        if estado != "aplicada": [nao verificado]
            continue

        c["aplicada"] += 1 [nao verificado]
        vivas = batidas_apuraveis(p.disputa.colaborador)
        vinculadas = vivas.filter(pergunta_origem_id=p.pk)
        tem_vinculo = vinculadas.exists()
        if not tem_vinculo:
            c["sem_vinculo_vivo"] += 1

        antes = engolidos()["total"]
        ts, tipo = _slot_esperado(p)
        if engolidos()["total"] != antes:
            c["indeterminada"] += 1
            continue

        if ts is None:
            # O juiz preserva o vínculo vivo quando o slot não é derivável.
            if tem_vinculo:
                c["com_correspondente_viva"] += 1
            else:
                c["indeterminada"] += 1
            continue

        corresponde = (
            _plantio_na_hora_do_slot(p, ts, vinculadas)
            or _batida_existente_na_janela(
                p.disputa.colaborador, ts, tipo, batidas=vivas
            ) is not None
        )
        c[
            "com_correspondente_viva" if corresponde
            else "sem_correspondente_viva"
        ] += 1

    assert not connection.needs_rollback
    for c in contagens.values():
        assert c["aplicada"] == ( [nao verificado]
            c["com_correspondente_viva"]
            + c["sem_correspondente_viva"]
            + c["indeterminada"]
        )

print("schema", schema)
print("via\tuniverso_hora\taplicada\tsem_vinculo_vivo\t"
      "com_correspondente_viva\tsem_correspondente_viva\tindeterminada")
campos = (
    "universo_hora", "aplicada", "sem_vinculo_vivo", [nao verificado]
    "com_correspondente_viva", "sem_correspondente_viva", "indeterminada",
)
for via, c in sorted(contagens.items()):
    print(via, *(c[k] for k in campos), sep="\t")

print("via\testado_real\tquantidade")
for (via, estado), quantidade in sorted(estados.items()):
    print(via, estado, quantidade, sep="\t")
PY
```

`PGOPTIONS` solicita transações somente leitura e snapshot serializable; os `assert`s impedem apresentar resultado se a configuração não tiver sido aplicada. Validação de execução nesse ambiente: [não verificado]. `schema_context` é o mecanismo utilizado pelo próprio projeto, inclusive com default `juliani` (`app/ponto/management/commands/propor_reorganizacao.py:17-23`); ele pode emitir `SET search_path`, comando de controle de conexão, sem DML. O script proposto consulta dados apenas por SELECT; essa execução futura não foi realizada. [não verificado]

O contador de falhas engolidas existe em memória e `registrar_engolido` apenas incrementa o contador e registra logging, sem escrever no banco (`app/core/observ.py:57-82`). Essa verificação evita contar como ausência de batida uma correspondência derivada com erro silencioso. [não verificado]

Limite importante: `sem_correspondente_viva` é a contagem segundo os auxiliares de correspondência atuais; `indeterminada` precisa permanecer separada. O total físico absoluto da frota não é verificável somente pelo anexo. [não verificado]

## 16/11 11:44 · sessao 503e17526781 · - · - · origem=Ronald (tmux)

Censo da árvore ativa concluído só por leitura; não importei Django nem consultei banco.

**Escritores físicos atuais do carimbo de PerguntaDisputa**

| Sítio | Escrita |
|---|---|
| `app/chamados/models.py:3180-3184` `_escrever_carimbo` | Única atribuição em memória de `materializada_em`, autor e via. |
| `app/chamados/models.py:3235` `carimbar` | `save(update_fields=campos)`; lista começa em `:3211`. Antes de persistir aplica carimbo e consulta C6 em `:3222`; se viva restaura em memória e retorna `False` em `:3226-3228`. |
| `app/chamados/models.py:3273` `carimbar_identidade` | `save(update_fields=campos)`; campos em `:3269`; aceita somente vias de identidade (`:3261-3262`), sem consultar C6. |
| `app/chamados/models.py:3295` `descarimbar` | `save(update_fields)` zerando os três campos (`:3291-3295`). |
| `app/chamados/models.py:3363-3365` `descarimbar_em_lote` | Único `QuerySet.update(materializada_em=None, ...)`; exclui vias fora do fato. |
| `app/chamados/models.py:3245` `_desfazer_em_memoria` | Restaura apenas memória após carimbo recusado/exceção, sem persistência própria. |

Executei apenas Python AST (`python3 -B`), extraindo o detector existente em `app/chamados/tests/test_o261e_f2_porta_carimbo.py:99-202`, sem imports do app. Varri todos `.py` fora `tests`, `test_*`, `logs`, `node_modules`, `__pycache__`, **incluindo migrations**. Resultado: somente `_escrever_carimbo`, `descarimbar_em_lote` e o homônimo de regularização abaixo. Nenhum `bulk_update` do carimbo na árvore ativa. O detector cobre atribuição, `setattr`, construtor/create/update, kwargs indiretos e bulk_update (`:174-197`), com limitações declaradas (`:111-115`).

**Todas as vias/chamadores operacionais que carimbam**

| Função/via | Evidência | Cria Batida? / pode carimbar sem Batida |
|---|---|---|
| `_materializar_ausencia_de_resposta`: `declaracao` ou `ausencia_dia` | `materializacao.py:555-567` | Cria **Ausencia** (`:527-535`), sem criar Batida para as irmãs. Pode terminar sem Batida. |
| `_materializar_resposta_hora`: `paridade_recusada` | `materializacao.py:906-931` | Não cria; exige marco aceso (`:907`), carimba identidade (`:930`) e retorna antes do registrar. |
| `_carimbar_recusa`: `fato_ocupado` | `materializacao.py:1095-1109` | Não cria; só COLISAO, e só sem carimbo anterior, carimba (`:1099-1106`). Demais recusas atualizam somente motivo (`:1107-1109`); **recusa posterior preserva carimbo anterior**. |
| `_materializar_uma`: `declaracao` ou `via_pelo_fato`, fallback `chamado_encerrado` | `materializacao.py:1267-1287` | Hora: cria ou reusa (`:865-877`, `:932-940`); data_hora: registrar (`:1012-1025`); escolha: explicitamente sem Batida (`:1069-1086`). Carimbo negado/lastro reprovado causa rollback (`:1238-1249`, `:1281-1287`). |
| `materializar_pendentes_aprovadas`: `juiz_aprovou` | `materializacao.py:1424-1438` | Não cria; consulta lastro e carimba. Juiz admite resolução sem testemunha para vias próprias (`juizes.py:682-710`), portanto não exige necessariamente Batida da pergunta. |
| `lavrar_sem_turno_aberto`: `sem_turno_aberto` | `reconciliador.py:363-406` | Não cria; somente sem resposta e sem turno aberto, com C6 morto; pode terminar sem Batida. |
| `retratar_perguntas_absolvidas_por_celula`: `celula_absolveu` | `reconciliador.py:550-596` | Não cria; absolvição da célula e C6; pode terminar sem Batida. |
| `_resolver_perguntas_por_chamado_resolvido`, ramo destino | `disputa_emissao.py:1987-2009` | Não cria; `via_pelo_fato`, resposta validar. Função declara SEM criar Batida (`:1907-1913`). |
| Mesma função, ramo sem credor | `disputa_emissao.py:2065-2082` | Não cria; `via_pelo_fato` ou `chamado_encerrado`, resposta responder. |
| `PerguntaDisputa.fechar_por_admin`: `admin_decidiu` | `models.py:3133-3147` | Não cria; porta de identidade. Chamadores: `DisputaSupervisao.fechar` desligamento/admin_encerra (`models.py:1982-1985`); `veredito_celula.fechar_o_fio_do_dia` (`ponto/services/veredito_celula.py:190`); aceite folga (`services/validacao.py:314`, `:354`). Esses atos podem resolver por folga/ausência, sem Batida. |
| `DisputaSupervisao.remover_motivo`: `motivo_retratado` | `models.py:2118-2126` | Não cria; preserva carimbo anterior quando existe (`manter_carimbo=True`). |
| `fio.retratar_duplicata`: `duplicata` | `services/fio.py:1384-1401` | Não cria; grava recusa apontando canônica; preserva carimbo anterior quando existe. |
| `PerguntaDisputa.encerrar_fantasma`: `fantasma` | `models.py:3378-3386` | Não cria; carimbar/C6. Chamador `reconciliador.py:1148`. |

`via_pelo_fato` devolve `cadastro_folga` para agrupamento sob pai limbo/folga sem batida apurável no dia (`juizes.py:1772-1798`), senão `adesao` (`:1799`); ausência aprovada → `ausencia_dia` (`:1805-1807`); TM com saída e fora intervalo → `completude_turno` (`:1808-1815`); restante → `chamado_encerrado` (`:1816`). Portanto `cadastro_folga` não tem escritor literal separado: entra pelos chamadores de `via_pelo_fato` listados acima.

`competencia_trancada` **não tem escritor novo**: porta antiga removida, via mantida para acervo e leitura, declarado em `models.py:3149-3154`. `fato_materializado` só troca via de pergunta já carimbada, sem gravar `materializada_em` (`models.py:3367-3375`); `relavra_via.relavrar` também só altera via (não é escritor do campo temporal).

**Todos os limpadores/chamadores**

- `ChamadoColaborador.ressuscitar_perguntas` → lote `models.py:1613`.
- `_universo_da_ressuscitacao` → `descarimbar(salvar=False)` apenas na cópia para julgar (`models.py:1541-1556`).
- `disputa_emissao.reabrir_para_correcao` → lote `services/disputa_emissao.py:1648`.
- `relavra_via.desfazer_carimbo_sem_lastro` → `descarimbar` depois C6 vivo (`services/relavra_via.py:242-246`).
- `PerguntaDisputa.reabrir_fantasma` → descarimbar (`models.py:3395-3397`); comando chama em `management/commands/reabrir_orfas_enterradas.py:157`.
- `reabrir_por_resposta` → descarimbar sem salvar (`models.py:3412-3414`); escritor da resposta agrega os campos em `services/disputa_emissao.py:1499`.
- `reabrir_por_competencia` → descarimbar (`models.py:3419-3421`); chamador `services/competencia_trancada.py:279`.

**Comandos, migrations e homônimos**

- `passivo_validado` é fila da mesma unidade: importa `_materializar_uma` em `services/passivo_validado.py:39` e chama em `:522`; não escritor adicional. `CAMPOS_REVERSAO` (`:64-66`) só lido por `_reversao_de` (`:81-83`) e guardado no resultado (`:370`); nenhuma restauração dinâmica ali.
- Migrations `0016_s84_pergunta_chamado_destino.py:23` e `0021_regularizacaoexterna_regularizacaoitembatida_and_more.py:26` apenas definem campos. Não achei migration de dado escrevendo `materializada_em`, inclusive fora chamados.
- Migration de dado com PD: `0051_decisao_pergunta.py:25-27` altera somente colaborador/instância; `0053_pergunta_unica_por_familia.py:36-46` só consulta e recusa colisões.
- Homônimo **RegularizacaoExterna**, outro modelo (`models.py:4128`): `services/regularizacao_ext.py:199-201` atribui e `save(update_fields)`. Pode apenas remover (`:132-141`) ou corrigir (`:151-164`) sem nova Batida; criação ocorre em `:172-189`. Não é pergunta nem campo equivalente de aplicação da pergunta.
- `tests/e4b_c6_bases` contém snapshots `.py.txt`, não runtime; nenhum `materializada_em =` nessa subárvore. Fixtures e mutantes dos testes não devem ser misturados com os escritores operacionais; o selo explica esse propósito em `tests/test_o261e_f2_porta_carimbo.py:17-23`, e exclui testes/migrations da própria varredura em `:207-211` (minha AST incluiu migrations).
- Retração posterior tem porta própria: `ponto/registro_batida.py:269-276` grava `aprovada=False` e `retratada_em`, sem alterar carimbo da Pergunta. Logo existência no momento do plantio não garante Batida viva depois; para atribuir “juiz desfez depois” a IDs concretos, somente o anexo fornecido, **[não verificado na cópia]**.

_6020 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
