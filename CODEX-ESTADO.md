# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 16:05** · publicado em **16/11/2026 16:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 16:05 · sessao 5da8ba141dd0 · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

**ITEM 2 — AVAL-DEVOLVIDO para esclarecer o literal do MORDE:** hoje “sem dias e sem link” significa ausência **visível**; dias e links dos demais calendários continuam no HTML, ocultos. (`app/templates/colaboradores/partials/painel_vinculo.html:204–208`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:130–137`)

1. **As premissas conferem, com uma distinção na caixa vazia.**

   Hoje não há título geral acima dos dias: aparecem apenas os rótulos mensais “Folgas Mês/Ano:” e o link “abrir este calendario”. A opção vazia diz “usar as folgas da escala (padrão do template)”; escolhê-la oculta todos os blocos. Na abertura, a visibilidade acompanha a FK do vínculo. (`app/templates/colaboradores/partials/painel_vinculo.html:195–207`)

   O contexto contém `escala_ativa`, seu `tipo_escala`, todos os calendários e `folgas_projecao`, organizada por calendário, mês e dias. São o mês corrente e o próximo, incluindo a virada de dezembro para janeiro; o calendário selecionado vem de `escala_ativa.folga_calendario_id`. (`app/colaboradores/views.py:268`; `app/colaboradores/views.py:300–311`; `app/templates/colaboradores/partials/painel_vinculo.html:200`)

   **Salvar vazio tem dois resultados:** havendo `tipo_escala.folga_calendario_padrao`, grava esse `FolgaCalendario`; sem padrão, grava `NULL`. Quem resolve o padrão é `vincular_folga`; quem atribui e persiste a FK é `trocar_calendario_de_folga`. A docstring desta porta diz “None = padrão”, mas seu corpo não procura padrão: salva exatamente o argumento recebido. (`app/colaboradores/views.py:230–239`; `app/colaboradores/services/vinculo.py:450–462`)

   Há também herança do padrão na criação/reutilização do vínculo, preservando calendário já escolhido. Leitores relevantes incluem o painel, a geração da foto, a comparação foto/calendário e o vigia de calendário vazio; a previsão lê as `FolgaDia` materializadas e o ciclo/âncora. Portanto, FK nula não significa pessoa sem folgas. (`app/colaboradores/services/vinculo.py:151–161`; `app/colaboradores/services/vinculo.py:284–298`; `app/templates/colaboradores/partials/painel_vinculo.html:200`; `app/escala/folgas.py:99`; `app/escala/folgas.py:127`; `app/escala/folgas.py:157–158`; `app/escala/models.py:1280–1295`)

   A assinatura é `datas_que_o_calendario_manda(cal, ano, mes) -> set[date]`: `None` retorna vazio; os modos usam células semana/dia ou dias por tamanho do mês. Seu universo é **aquele calendário naquele mês**, sem pessoa, vigência, folgas manuais ou precedência. O contexto já contém a projeção de todos os calendários e a identificação do padrão pelo tipo; basta reutilizá-la, sem nova chave obrigatória nem derivação paralela. (`app/escala/folgas.py:43–56`; `app/escala/models.py:89–93`; `app/colaboradores/views.py:300–311`)

2. **Não identifiquei colisão necessária com as regras consultadas.**

   Ler essa fonte mantém tela e escritor na mesma derivação, como exige L2; o título delimita corretamente o calendário, enquanto as folgas efetivas dependem de outros fatos e do vínculo do dia. (`app/docs/LEIS-UI.md:23–25`; `app/escala/folgas.py:158`; `app/docs/JUIZES.md:119–135`)

   O link já existe, aponta para `escala:editar_folga_calendario` e depende de `editar_escalas`, a mesma ação exigida no destino. O complemento pode preservá-lo sem construir edição/versionamento. (`app/templates/colaboradores/partials/painel_vinculo.html:207`; `app/colaboradores/views.py:309`; `app/escala/views.py:315`)

   O148-EDITAR continua registrado, aguardando corte para versionamento de calendário usado; o editor de três grades tem precedência sobre o painel, na mesma raia de tela. A projeção proposta não exige invadir essas obras. Conclusão atual do editor em execução: **[não verificado]**. (`app/docs/CORTES.md:357`; `app/docs/BACKLOG.md:820`; `app/docs/RELATO.md:61`)

3. **O encaixe técnico fecha; o MORDE precisa de precisão textual.**

   “Contexto já existente” é suficiente: calendário padrão, calendários oferecidos, meses e dias já são alcançáveis pelos dados existentes. CSS novo também é dispensável: `.font-semibold` já está definido. O título é rótulo; não exige acrescentar prosa de ajuda nem diálogo nativo. (`app/colaboradores/views.py:300–311`; `app/escala/models.py:89`; `app/static/css/hasner-ponto.css:514`; `app/docs/LEIS-UI.md:77–89`)

   A ressalva literal é dupla: vínculo sem FK explícita pode ter padrão no template; e ausência visual não equivale a ausência no HTML. O teste atual mantém outros calendários na fixture e verifica que nenhum bloco está visível. Exigir ausência absoluta de dias/links no HTML mudaria esse contrato existente de alternância. (`app/colaboradores/views.py:236`; `app/templates/colaboradores/partials/painel_vinculo.html:196–207`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:120–137`)

   O conteúdo integral do “censo do brief original”, citado pelo aval fornecido, permanece **[não verificado]**.

4. **A projeção pode ficar fora de exportada, dinheiro e novo `!`.**

   O trecho da GET consulta a fonte e monta contexto. Já o POST existente grava a FK, sincroniza fotos futuras e materializa os dois meses; alterar esses caminhos ultrapassaria a apresentação examinada e alcançaria dado de vínculo/escala, protegido por `!`. (`app/colaboradores/views.py:300–306`; `app/colaboradores/views.py:239–246`; `app/colaboradores/services/vinculo.py:461–465`; `CLAUDE.md:265–268`)

VEREDITO: AVAL-DEVOLVIDO — No MORDE, “sem calendário nenhum” significa seleção vazia **e template sem padrão**, e “sem dias e sem link” significa **sem projeção/link visíveis**, preservando os blocos ocultos dos outros calendários? (`app/colaboradores/views.py:236`; `app/templates/colaboradores/partials/painel_vinculo.html:204–208`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:130–137`)

## 16/11 16:04 · sessao 4fb58aa99879 · - · - · origem=Ronald (tmux)

- Não há colisão necessária com LEIS-UI: a fonte única já existe, é usada pela leitura e pelo escritor; manter a projeção nela atende L2 e LEI-AKITA 2. O título é rótulo, enquanto L7 disciplina texto de ajuda. (`app/escala/folgas.py:43`, `app/escala/folgas.py:158`, `app/docs/LEIS-UI.md:24`, `app/docs/LEIS-UI.md:78`, `CLAUDE.md:20`)

- O148-EDITAR permanece **só registro**: versionamento de calendário usado espera corte; o item 2 deve ficar na apresentação e preservar a porta de escrita. O editor de três grades está antes do complemento do painel, na mesma raia/agente. (`app/docs/CORTES.md:357`, `app/docs/BACKLOG.md:820`, `app/docs/RELATO.md:61`)

- O link atual é **“abrir este calendario”**, condicionado por `pode_abrir_calendario`, obtido de `tem_acao(..., 'editar_escalas')`; o destino usa a mesma ação. Ele não é novo neste complemento. (`app/templates/colaboradores/partials/painel_vinculo.html:207`, `app/colaboradores/views.py:309`, `app/escala/views.py:315`)

- A projeção do calendário não equivale ao julgamento das folgas efetivas da pessoa: estas dependem do vínculo do dia e da precedência. O título específico “Folgas que este calendário manda” respeita essa distinção. (`app/escala/folgas.py:44`, `app/docs/JUIZES.md:119`, `app/docs/JUIZES.md:127`, `app/docs/JUIZES.md:135`)

- **CSS novo é dispensável:** a classe `.font-semibold` já existe e a folha é carregada na base. (`app/static/css/hasner-ponto.css:514`, `app/templates/base.html:17`)

- Alterar apenas a projeção não requer tocar o gravado exportado, dinheiro nem dado de escala/vínculo; essas escritas continuam fora do alcance e exigiriam `!`. O GET do painel monta contexto; a edição persistente é outra porta, em POST, que atualiza calendário e sincroniza fotos futuras. (`app/colaboradores/views.py:262`, `app/colaboradores/views.py:304`, `app/colaboradores/views.py:306`, `CLAUDE.md:265`, `app/escala/views.py:320`, `app/escala/folgas.py:571`, `app/escala/folgas.py:600`)

- O brief/censo bruto citado em `logs/` não está disponível nesta cópia; a enumeração dos seus leitores permanece **[não verificado]**. O RELATO conserva a proposta original e registra expressamente a ausência do título, além de pôr a caixa vazia na D1. (`app/docs/RELATO.md:4`, `app/docs/RELATO.md:63`, `app/docs/RELATO.md:65`)

## 16/11 16:04 · sessao 17f5ba206479 · - · - · origem=Ronald (tmux)

- **Caixa vazia grava o padrão quando há; grava nulo quando não há.** O POST resolve vazio para `ec.tipo_escala.folga_calendario_padrao` ou `None`, e chama a porta; a porta atribui exatamente esse objeto/nulo e salva o campo (`app/colaboradores/views.py:230`, `app/colaboradores/views.py:236`, `app/colaboradores/views.py:239`; `app/colaboradores/services/vinculo.py:459`, `app/colaboradores/services/vinculo.py:461`). O campo admite nulo e usa `SET_NULL` (`app/escala/models.py:890`).

- **A documentação da porta é imprecisa:** diz `None = padrao do template`, mas o corpo não resolve padrão: grava `None`; quem resolve o padrão nesta ação é a view (`app/colaboradores/services/vinculo.py:451`, `app/colaboradores/services/vinculo.py:461`; `app/colaboradores/views.py:236`).

- **O teste existente confirma as duas ramificações no código de teste:** vazio com padrão exige a FK padrão; vazio sem padrão exige FK nula, foto de calendário removida e manual preservada. Não executei testes (`app/colaboradores/tests/test_vincular_folga_view.py:74`, `app/colaboradores/tests/test_vincular_folga_view.py:81`, `app/colaboradores/tests/test_vincular_folga_view.py:83`, `app/colaboradores/tests/test_vincular_folga_view.py:95`, `app/colaboradores/tests/test_vincular_folga_view.py:99`).

- **Na tela atual, vazio só oculta a projeção:** a opção diz “usar as folgas da escala (padrão do template)”; `onchange` oculta todo bloco cujo PK difira do valor vazio; os blocos iniciais seguem a FK explícita do vínculo (`app/templates/colaboradores/partials/painel_vinculo.html:196`, `app/templates/colaboradores/partials/painel_vinculo.html:198`, `app/templates/colaboradores/partials/painel_vinculo.html:205`). Não há título geral “Folgas que este calendario manda”; há rótulos mensais `Folgas M/Y:` e link “abrir este calendario” (`app/templates/colaboradores/partials/painel_vinculo.html:206`, `app/templates/colaboradores/partials/painel_vinculo.html:207`).

- **“Sem dias e sem link” precisa significar sem dias/link visíveis naquele bloco.** Todos os calendários oferecidos têm projeção renderizada, ainda que oculta; o link sai nos blocos se `pode_abrir_calendario` for verdadeiro. O teste atual verifica ausência de blocos **visíveis**, não ausência de dias/link do HTML (`app/templates/colaboradores/partials/painel_vinculo.html:204`, `app/templates/colaboradores/partials/painel_vinculo.html:205`, `app/templates/colaboradores/partials/painel_vinculo.html:207`; `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:130`, `app/colaboradores/tests/test_ui_vinculo_mostra_calendario.py:135`).

- **O contexto existente basta como insumo.** Ele já tem `escala_ativa` com `tipo_escala`, todos os calendários e suas projeções do mês atual e próximo; a FK padrão pertence ao tipo. Pode-se selecionar/reutilizar a projeção do padrão sem nova derivação de datas nem nova fonte; a view já pergunta a fonte para todos os calendários (`app/colaboradores/views.py:268`, `app/colaboradores/views.py:300`, `app/colaboradores/views.py:301`, `app/colaboradores/views.py:302`, `app/colaboradores/views.py:304`, `app/colaboradores/views.py:307`, `app/colaboradores/views.py:311`; `app/escala/models.py:89`). Uma implementação que acrescente chave ao contexto teria de ser julgada contra o literal do aval; ela não é tecnicamente necessária para disponibilizar calendário/dias/mês, pois já estão nessas estruturas (`app/colaboradores/views.py:304`, `app/colaboradores/views.py:308`, `app/colaboradores/views.py:311`).

- **A fonte é `datas_que_o_calendario_manda(cal, ano, mes) -> set[date]`.** `None` devolve conjunto vazio; modo `semana_dia` usa células; demais usam os dias por tamanho. Ela não recebe pessoa/vínculo, não filtra vigência e não calcula todas as folgas efetivas da pessoa: interpreta o calendário no mês (`app/escala/folgas.py:43`, `app/escala/folgas.py:52`, `app/escala/folgas.py:54`, `app/escala/folgas.py:56`). O universo de vínculos ativos cuja vigência alcança o mês pertence à função separada `vinculos_com_calendario_no_mes`, não à fonte (`app/escala/folgas.py:59`, `app/escala/folgas.py:74`, `app/escala/folgas.py:78`).

- **Escritores/leitores relevantes:** a herança na criação/reutilização copia o padrão somente se houver e se o vínculo ainda não tiver calendário; a ação da tela e a aplicação em lote usam `trocar_calendario_de_folga`; ajustes automáticos copiam a FK ao novo vínculo (`app/colaboradores/services/vinculo.py:151`, `app/colaboradores/services/vinculo.py:157`, `app/colaboradores/services/vinculo.py:161`, `app/colaboradores/services/vinculo.py:284`, `app/colaboradores/services/vinculo.py:298`; `app/colaboradores/views.py:239`; `app/escala/views.py:408`; `app/colaboradores/services/vinculo.py:1092`, `app/colaboradores/services/vinculo.py:1113`). Leem a FK o painel, a geração da foto, a divergência, os vigias e a sincronização futura (`app/templates/colaboradores/partials/painel_vinculo.html:200`, `app/escala/folgas.py:99`, `app/escala/folgas.py:127`, `app/escala/folgas.py:157`, `app/escala/folgas.py:627`).

- **Sem calendário não significa pessoa sem folgas.** A geração passa `None` à fonte e reconcilia a foto de calendário, preservando manual; a previsão de trabalho lê as `FolgaDia` e, sem foto, calcula pelo ciclo/âncora, podendo ainda aplicar feriado (`app/escala/folgas.py:153`, `app/escala/folgas.py:157`, `app/escala/folgas.py:180`, `app/escala/folgas.py:187`; `app/escala/models.py:1286`, `app/escala/models.py:1289`, `app/escala/models.py:1365`, `app/escala/models.py:1369`). Portanto, o título proposto delimita corretamente “o que o calendário manda”, mas não prova uma previsão total das folgas da pessoa (`app/escala/folgas.py:43`; `app/escala/models.py:1296`, `app/escala/models.py:1319`).

- **O POST existente tem efeitos reais além da FK:** a porta sincroniza foto futura quando o calendário muda, e a view gera fotos do mês atual e próximo. A leitura/projeção da GET usa somente a fonte e não chama esses escritores (`app/colaboradores/services/vinculo.py:463`, `app/colaboradores/services/vinculo.py:465`; `app/colaboradores/views.py:245`; `app/colaboradores/views.py:304`). A sincronização futura chama regeneração sem recálculo (`app/escala/folgas.py:640`). A mudança visual pode ficar isolada disso mantendo o POST, mas efeitos de eventual implementação ainda são [não verificado].

## 16/11 15:54 · sessao faf0dcc58538 · BUG-PAR-O-PAR · verificador · origem=Code

**DEVOLVE: a separação K6/F6 foi implementada, mas o estado novo ainda reúne “entregue” e “sem dia para consultar K6”; além disso, a prova executada não cobre a matriz nem os mutantes de integração exigidos.** (`app/chamados/services/materializacao.py:780`, `app/chamados/services/materializacao.py:800`, `app/chamados/services/materializacao.py:863`, `app/chamados/tests/bug_par_mutantes.py:123`, `app/chamados/tests/bug_par_mutantes.py:164`.)

A identidade Git de HEAD/base e a aplicação histórica do delta sobre `b8b951978` ficam **[nao verificado]**; o exame abaixo confronta a cópia com o diff e os anexos fornecidos.

**1. Escritores, leitores e significados**

| Campo ou retorno | Quem escreve/produz | Quem lê; significado |
|---|---|---|
| `bloqueada_competencia` e `detalhe` | Testemunha produz pelo impedimento F6, na metade ou no par. (`app/chamados/services/materializacao.py:884`, `app/chamados/services/materializacao.py:908`.) | Card e modal exibem o detalhe e conservam Validar; contador classifica em `outras`. (`app/templates/chamados/partials/_pergunta_card.html:44`, `app/templates/chamados/partials/_pergunta_card.html:100`, `app/templates/chamados/partials/modal_fio.html:274`, `app/chamados/services/materializacao.py:347`.) |
| `bloqueada_competencia_entregue` e `detalhe` | Testemunha converte qualquer impedimento de `entrega_que_barra_a_validacao` nesse estado; porta faz o mesmo antes da validação e na reconsulta com rollback. A porta usa `msg`, não a chave `detalhe`. (`app/chamados/services/materializacao.py:861`, `app/chamados/services/validacao.py:614`, `app/chamados/services/validacao.py:649`.) | Card/modal escondem Validar; `fio.py` e filtro transportam o dict; casca da ação usa `ok/msg`. (`app/templates/chamados/partials/_pergunta_card.html:47`, `app/templates/chamados/partials/modal_fio.html:274`, `app/chamados/services/fio.py:374`, `app/chamados/templatetags/chamados_tags.py:136`, `app/chamados/services/acoes_disputa.py:50`.) |
| `irma_da_validacao` | Retorna `None` para resposta fora do crivo HORA; caso contrário consulta `metade_irma_do_intervalo`. Esta exige irmã única, respondida, não materializada, da mesma disputa/data e motivo complementar. (`app/chamados/services/validacao.py:197`, `app/chamados/services/materializacao.py:140`.) | Porta usa a identidade para travar destinos/clones; composição K6 consulta pergunta e irmã. Encontrar a irmã não garante que o par será gravável: o julgamento posterior tem outros impedimentos. (`app/chamados/services/validacao.py:600`, `app/chamados/services/validacao.py:217`, `app/chamados/services/materializacao.py:197`, `app/chamados/services/materializacao.py:225`.) |
| `entrega_que_barra_a_validacao` | Compõe a guarda existente com pergunta e irmã, `somente_entregue=True`; retorna texto impeditivo ou `''`. (`app/chamados/services/validacao.py:206`, `app/chamados/services/validacao.py:217`.) | Testemunha e duas consultas da porta usam a verdade do retorno. **O retorno também contém falta de dia, sem entrega comprovada.** (`app/chamados/services/materializacao.py:800`, `app/chamados/services/materializacao.py:861`, `app/chamados/services/validacao.py:614`, `app/chamados/services/validacao.py:649`.) |
| `competencia_do_fato` | Produz `None` sem `data_evento`; com data, retorna `(mes, ano, bool(K6))`, delegando à janela e ao juiz da entrega. (`app/chamados/services/materializacao.py:772`, `app/chamados/services/materializacao.py:780`, `app/chamados/services/materializacao.py:784`.) | Guarda, contador, devolução U103 e comando consomem esse contrato. (`app/chamados/services/materializacao.py:799`, `app/chamados/services/materializacao.py:330`, `app/chamados/services/validacao.py:321`, `app/chamados/management/commands/bug_par_103.py:58`.) |
| `data_evento` | Atribuição central em `gravar_dia_do_fato`; os chamadores persistem no ato correspondente. (`app/chamados/services/fio.py:835`, `app/chamados/services/fio.py:845`.) | K6 toma a competência desse dia do fato; a guarda posicional usa célula explícita, depois esse dia; sem pergunta usa dia civil do instante. (`app/chamados/services/materializacao.py:784`, `app/chamados/services/materializacao.py:421`.) |

**Há dois significados no estado novo — reproduzido em memória pelas funções da cópia:** `data_evento=None` → `competencia_do_fato=None` → texto “sem dia do fato para consultar K6” → `bloqueada_competencia_entregue`. A porta devolve o mesmo rótulo, embora o juiz K6 nem tenha sido consultado; ocorrência dessa entrada em produção: **[nao verificado]**. (`app/chamados/services/materializacao.py:780`, `app/chamados/services/materializacao.py:800`, `app/chamados/services/materializacao.py:863`, `app/chamados/services/validacao.py:616`.)

| Chave do contador | Escritor e significado | Leitores encontrados |
|---|---|---|
| `pendentes_aval` | `universo + len(b103)`: não materializadas mais U103 atualmente devolvíveis. (`app/chamados/services/materializacao.py:351`, `app/chamados/services/materializacao.py:356`.) | Comando e testes. (`app/chamados/management/commands/bug_par_103.py:82`, `app/chamados/tests/test_bug_par_103.py:88`.) |
| `congelados_no_recorte` | `len(qs)`, após seleção U163 e filtro de disputas. (`app/chamados/services/materializacao.py:321`, `app/chamados/services/materializacao.py:324`, `app/chamados/services/materializacao.py:356`.) | Comando e testes migrados. (`app/chamados/management/commands/bug_par_103.py:83`, `app/chamados/tests/test_bug_par_volta.py:88`, `app/chamados/tests/test_bug_par_projetado.py:533`.) |
| `resolvidas` | Incrementa para carimbada que não atende mais ao predicado U103; não significa necessariamente validação exitosa. (`app/chamados/services/materializacao.py:326`, `app/chamados/services/materializacao.py:328`.) | Soma da partição no teste novo. (`app/chamados/tests/test_bug_par_103.py:94`.) |
| `bug_par_103_pauta_dp` | Candidatas carimbadas com K6 verdadeiro, como `(pk, 'MM/AAAA')`. (`app/chamados/services/materializacao.py:330`, `app/chamados/services/materializacao.py:334`.) | Teste novo; o comando monta sua pauta diretamente da seleção congelada. (`app/chamados/tests/test_bug_par_103.py:93`, `app/chamados/management/commands/bug_par_103.py:57`.) |
| `bug_par_103_sem_dia` | Candidatas carimbadas cujo helper retorna `None`. (`app/chamados/services/materializacao.py:331`, `app/chamados/services/materializacao.py:355`.) | Soma da partição no teste. (`app/chamados/tests/test_bug_par_103.py:95`.) |
| `total_aval`, removida | Não integra mais o retorno. (`app/chamados/services/materializacao.py:352`.) | Os consumidores encontrados foram migrados; resta o literal na asserção de ausência no stdout. Consumidores externos à cópia: **[nao verificado]**. (`app/chamados/tests/test_bug_par_c2_sem_banco.py:63`, `app/chamados/tests/test_bug_par_projetado.py:548`, `app/chamados/tests/test_bug_par_103.py:99`.) |

A linha publicada passa a ser `BUG-PAR-103 MODO elegiveis=N exportadas=N pendentes_aval=N congelados_no_recorte=N`; os dois últimos números vêm do contador com o mesmo recorte empresarial. `elegiveis` continua contando todas as U103 presentes, enquanto `bug_par_103` do contador agora conta somente candidatas devolvíveis — são medidas diferentes. Leitores externos desse stdout: **[nao verificado]**. (`app/chamados/management/commands/bug_par_103.py:53`, `app/chamados/management/commands/bug_par_103.py:79`, `app/chamados/services/materializacao.py:327`.)

No eixo temporal, os escritores locais de `slots/marcas` extraem `hora`, `luz` acesa e `orfas` da ata; o cartório produz essas lâmpadas e horas. `eixo_do_turno` normaliza slots/marcas; `_hora` usa a **data real** do plantio menos `cel.data`, por `hora_no_eixo`; `plantio_preenche_furo` lê tudo como minutos no mesmo eixo. (`app/chamados/services/materializacao.py:435`, `app/ponto/services/cartorio.py:711`, `app/ponto/services/cartorio.py:738`, `app/chamados/services/materializacao.py:443`, `app/ponto/propositor.py:87`, `app/ponto/propositor.py:104`, `app/ponto/propositor.py:265`.)

Isso não torna todas as unidades temporais iguais: K6 usa competência do fato; F6 usa a data local do instante; ausência de pergunta conserva a consulta civil; o corte U103 é estritamente anterior a 18/10 00:00 no fuso configurado. As janelas incluem o dia do corte inicial e vão até o dia anterior ao próximo corte; a tolerância de atribuição é de 240 minutos. (`app/chamados/services/materializacao.py:784`, `app/ponto/janelas.py:84`, `app/chamados/services/materializacao.py:422`, `app/chamados/services/materializacao.py:258`, `app/chamados/services/materializacao.py:262`, `app/ponto/janelas.py:10`, `app/ponto/propositor.py:78`.)

Há leitor produtivo fora do diff: `flip_auto` continua usando `atribuir_global_turno`, agora delegado à função extraída; a regra de normalização foi preservada nessa extração. (`app/ponto/services/flip_auto.py:233`, `app/ponto/propositor.py:101`, `app/ponto/propositor.py:110`.)

**2. F1..F5: origem, caso e pendências**

| Achado anterior | Julgamento da c3 |
|---|---|
| **F1** | Corrige a origem da divergência K6/F6: testemunha e porta compartilham a composição, e ambos os HTMLs conservam o toque F6. **Permanece a duplicidade “entregue/sem dia” no estado novo.** (`app/chamados/services/materializacao.py:861`, `app/chamados/services/validacao.py:614`, `app/templates/chamados/partials/_pergunta_card.html:47`, `app/templates/chamados/partials/modal_fio.html:274`, `app/chamados/services/materializacao.py:800`.) |
| **F2** | Corrige a origem temporal, sem especializar por PK: célula dona e plantio entram no eixo do turno. O esperado agora é `True` literal; `dia_civil`, `eixo_civil`, `eixo_pela_hora` e a supressão adicional da absolvição falharam por asserção. Execução sobre o retrato real de produção: **[nao verificado]**. (`app/chamados/services/materializacao.py:439`, `app/chamados/tests/test_bug_par_sem_banco.py:50`, `app/chamados/tests/test_bug_par_sem_banco.py:51`, `app/chamados/tests/test_bug_par_sem_banco.py:83`.) |
| **F3** | Cura a lacuna do teste: o mutante restaura exatamente `de outro(s) dia(s)` e morreu pela `assertNotRegex`. (`app/chamados/tests/bug_par_c2_mutantes.py:26`, `app/chamados/tests/test_bug_par_c2_sem_banco.py:102`.) |
| **F4** | Corrige a origem do rótulo/recorte e separa pendência de pertencimento. **A prova nova não demonstra zeramento pela validação:** zera trocando K6 para entregue, com `resolvidas=0`. (`app/chamados/services/materializacao.py:326`, `app/chamados/services/materializacao.py:356`, `app/chamados/management/commands/bug_par_103.py:79`, `app/chamados/tests/test_bug_par_103.py:86`, `app/chamados/tests/test_bug_par_103.py:89`.) |
| **F5** | Corrige a origem comum da ausência de data: comando publica linha falante para U103 e fora163; devolução retorna antes de escrever. Falta cobertura da testemunha/validação fora163 sem dia, justamente onde aparece o estado ambíguo. (`app/chamados/services/materializacao.py:780`, `app/chamados/management/commands/bug_par_103.py:62`, `app/chamados/management/commands/bug_par_103.py:75`, `app/chamados/services/validacao.py:322`, `app/chamados/tests/test_bug_par_103.py:111`, `app/chamados/tests/test_bug_par_103.py:119`.) |

**3. Autoridades, escritores, relógio e universo**

Não identifiquei segundo juiz no delta: competência do fato compõe janela existente e K6; bloqueio F6 continua no juiz registrado; irmã e entrega da validação extraem a composição da porta. (`app/chamados/services/materializacao.py:784`, `app/core/juizes.py:343`, `app/core/juizes.py:315`, `app/chamados/services/validacao.py:197`, `app/chamados/services/validacao.py:206`.)

Não identifiquei segundo escritor de batida/carimbo nem nova leitura produtiva do relógio introduzida pelo delta: permanecem `registrar_batida`, as portas de carimbo e os instantes dos atos existentes. (`app/chamados/services/materializacao.py:1409`, `app/chamados/models.py:3207`, `app/chamados/services/validacao.py:165`, `app/chamados/services/validacao.py:338`.)

As listas novas são partições da consulta U163, não outro universo de pertencimento; o pertencimento continua exclusivamente por PK no arquivo congelado. O problema de “campo com dois significados” existe no estado entregue, conforme demonstrado acima. (`app/chamados/services/materializacao.py:321`, `app/chamados/services/materializacao.py:331`, `app/chamados/services/universo_bug_par.py:35`, `app/chamados/services/materializacao.py:800`, `app/chamados/services/materializacao.py:863`.)

**4. Mordida dos testes** [nao verificado]

Executei **15 testes sem banco verdes**, **17 mutantes não integrados mortos** e **seis mutantes c2 mortos**; os runners exigem falhas por asserção, sem erros/skips, e aplicam as alterações em memória. (`app/chamados/tests/e4_apoio.py:8`, `app/chamados/tests/bug_par_mutantes.py:178`, `app/chamados/tests/bug_par_mutantes.py:196`, `app/chamados/tests/bug_par_c2_mutantes.py:64`, `app/chamados/tests/bug_par_c2_mutantes.py:67`.)

| Grupo | Resultado por mutante |
|---|---|
| Principal sem integração | Mortos: `dxg_frota`, `dxg_desligado`, `dia_civil`, `k6_so_comando`, `k6_omitido`, `unidade_sem_k6`, `marca_ignorada`, `hash_ignorado`, `bool_inteiro`, `chaves_ignoradas`, `repeticao_aceita`, `intersecao_aceita`, `selecionar_recusa`, `nova_nota`, `recalcular_f2`, `eixo_civil`, `eixo_pela_hora`. (`app/chamados/tests/bug_par_mutantes.py:26`, `app/chamados/tests/bug_par_mutantes.py:110`, `app/chamados/tests/bug_par_mutantes.py:196`.) |
| Runner c2 | Mortos: `c2_contador_alias`, `c2_103_alias`, `c2_aviso`, `c3_aviso_base`, `c2_vocabulario`, `c2_fixture_recusa`. (`app/chamados/tests/bug_par_c2_mutantes.py:16`, `app/chamados/tests/bug_par_c2_mutantes.py:67`.) |
| Integração | **[nao verificado]** para `par_parcial`, `par_ordem_volta`, `retry_reativa`, `contador_recusa`, `s139_digitada`, `command_txt`, `103_k6_constante`, `103_troca_ato`, `103_retry`, `card_libera`, `f6_esconde`, `f1_testemunha_sem_irma`, `f4_pauta_no_pendente`, `f5_sem_dia_calado`, `fundir_a_em_b`, `fundir_a_em_b_porta`. Sem `--django`, até `todos` exclui os 16. (`app/chamados/tests/bug_par_mutantes.py:123`, `app/chamados/tests/bug_par_mutantes.py:161`.) |

F2 deixou de calcular o esperado pelo próprio propositor; os testes matriciais têm asserções sobre estado, HTML e persistência. Entretanto, o teste F4 não exercita a validação prometida, e a busca por Validar no modal considera **qualquer formulário**, sem restringi-lo à pergunta examinada. (`app/chamados/tests/test_bug_par_sem_banco.py:50`, `app/chamados/tests/test_bug_par_volta.py:213`, `app/chamados/tests/test_bug_par_volta.py:218`, `app/chamados/tests/test_bug_par_103.py:89`, `app/chamados/tests/test_bug_par_volta.py:203`.)

A execução Django original dos vizinhos e dos testes matriciais fica **[nao verificado]**; a tentativa de importação encontrou ausência de `django.test`. (`app/ponto/tests/test_f2_eixo_turno.py:4`, `app/core/tests/test_medida_turno_do_par.py:95`.)

**5. Escopo protegido e avais**

O delta fornecido não altera diretamente `motor_calculo_v2.py`, fórmulas financeiras, escala/vínculo ou o JSON congelado; confronto Git independente: **[nao verificado]**. A normalização extrai o algoritmo existente, e o carregador mantém hash/cardinalidade/conjuntos congelados. (`app/ponto/propositor.py:101`, `app/ponto/propositor.py:104`, `app/chamados/services/universo_bug_par.py:15`, `app/chamados/services/universo_bug_par.py:20`.)

Há efeito potencial sobre dinheiro pela mudança da absolvição e consequente criação de batidas, que dispara recálculo pela porta existente; não é alteração apenas visual. Entregue fora163 e devolução U103 mantêm K6 antes da escrita. (`app/chamados/services/materializacao.py:448`, `app/chamados/services/materializacao.py:1409`, `app/ponto/registro_batida.py:180`, `app/chamados/services/materializacao.py:1761`, `app/chamados/services/validacao.py:321`.)

O código do caminho `--apply` também foi alterado para consumir o helper, mas sua execução nesta verificação fica **[nao verificado]**; o caminho continua chamando a porta de devolução. (`app/chamados/management/commands/bug_par_103.py:106`, `app/chamados/management/commands/bug_par_103.py:116`.)

A contradição de desenho remanescente é chamar de “entregue” uma competência não consultada; a exigência documental de zeramento pela validação ganhou implementação compatível, mas não ganhou a prova específica desse percurso. (`app/chamados/services/materializacao.py:800`, `app/chamados/services/materializacao.py:863`, `app/docs/PROMPTS.md:2235`, `app/chamados/tests/test_bug_par_103.py:89`.)

**6. Matriz COMPLEMENTO-1**

A persistência abaixo descreve a porta administrativa `validar_pergunta`; os resultados executados dessa matriz permanecem **[nao verificado]**. (`app/chamados/tests/test_bug_par_volta.py:207`, `app/chamados/tests/test_bug_par_volta.py:220`, `app/chamados/tests/test_bug_par_volta.py:234`, `app/chamados/tests/test_bug_par_volta.py:245`.)

| Linha | Juiz e operação da porta | Persistido | Testemunha, card e modal | Prova e mutante |
|---|---|---|---|---|
| **L1 — entregue fora163** | K6 decide a entrega; composição usa competência do fato; porta recusa antes de carimbar. (`app/folha/export.py:556`, `app/chamados/services/materializacao.py:784`, `app/chamados/services/validacao.py:614`.) | **Nada:** retorna antes da escrita. (`app/chamados/services/validacao.py:616`.) | `bloqueada_competencia_entregue`; card e modal exibem detalhe e escondem Validar. (`app/chamados/services/materializacao.py:863`, `app/templates/chamados/partials/_pergunta_card.html:44`, `app/templates/chamados/partials/_pergunta_card.html:47`, `app/templates/chamados/partials/modal_fio.html:252`, `app/templates/chamados/partials/modal_fio.html:274`.) | Teste L1 cobra estado, ambos HTMLs, nenhuma batida e fotografia inalterada. Mutantes de fusão na testemunha/porta declarados; mortes **[nao verificado]**. (`app/chamados/tests/test_bug_par_volta.py:207`, `app/chamados/tests/bug_par_mutantes.py:99`, `app/chamados/tests/bug_par_mutantes.py:103`.) |
| **L2 — F6, não entregue** | F6 decide; porta recebe o toque; `registrar_batida` recusa plantio institucional em competência fechada. (`app/ponto/janelas.py:75`, `app/chamados/services/validacao.py:617`, `app/ponto/registro_batida.py:156`.) | **Atos VALIDACAO e DEVOLUCAO**, motivo de recusa; validação final removida, sem batida/carimbo final de materialização. (`app/chamados/services/validacao.py:170`, `app/chamados/services/validacao.py:256`, `app/chamados/services/validacao.py:262`, `app/chamados/services/materializacao.py:1610`, `app/ponto/registro_batida.py:161`.) | `bloqueada_competencia`; ambos mostram detalhe e conservam Validar. (`app/chamados/services/materializacao.py:886`, `app/chamados/services/materializacao.py:910`, `app/templates/chamados/partials/_pergunta_card.html:44`, `app/templates/chamados/partials/_pergunta_card.html:100`, `app/templates/chamados/partials/modal_fio.html:254`, `app/templates/chamados/partials/modal_fio.html:306`.) | Teste L2 cobra tela, `nao_gravou/devolvida` e nenhuma batida; não fotografa os atos descritos. `f6_esconde` declarado; morte **[nao verificado]**. (`app/chamados/tests/test_bug_par_volta.py:220`, `app/chamados/tests/test_bug_par_volta.py:231`, `app/chamados/tests/bug_par_mutantes.py:94`.) |
| **L3 — U163 divergente** | Gate compara declarado/derivado e consulta pertencimento congelado; porta retorna espera antes da validação. (`app/chamados/services/materializacao.py:720`, `app/chamados/services/materializacao.py:738`, `app/chamados/services/validacao.py:571`.) | **Nada pela porta administrativa:** retorno antes do carimbo. (`app/chamados/services/validacao.py:575`, `app/chamados/services/validacao.py:617`.) | `espera_declarado_x_gravado`; card/modal mostram espera e escondem Validar. (`app/chamados/services/materializacao.py:741`, `app/templates/chamados/partials/_pergunta_card.html:49`, `app/templates/chamados/partials/modal_fio.html:264`, `app/templates/chamados/partials/modal_fio.html

[truncado em 20000 de 23235 caracteres]

## 16/11 15:52 · sessao b4dfeca660fc · - · - · origem=Ronald (tmux)

Auditoria somente leitura dos testes: [nao verificado]

- Executei **15 testes verdes**, cobrindo `test_bug_par_sem_banco`, `test_bug_par_c2_sem_banco` e `test_universo_bug_par`; usam funções reais extraídas por AST e sobreposição somente em memória. (`app/chamados/tests/e4_apoio.py:8`, `app/chamados/tests/e4_catracas.py:5`.)
- Executei os **17 mutantes sem integração** do catálogo principal: todos morreram por falha, sem erro/skip; executei os **6 c2**, também todos mortos. A condição de morte está em `app/chamados/tests/bug_par_mutantes.py:196` e `app/chamados/tests/bug_par_c2_mutantes.py:67`.
- **Os 16 mutantes de integração continuam [nao verificado]**: `todos` sem `--django` os elimina explicitamente. Os dez pedidos no COMPLEMENTO-1 estão nesse conjunto. (`app/chamados/tests/bug_par_mutantes.py:123`, `app/chamados/tests/bug_par_mutantes.py:161`.)
- Todos os 33 alvos textuais do catálogo principal aparecem exatamente uma vez na cópia; a cobrança de unicidade está em `app/chamados/tests/bug_par_mutantes.py:176`.

**F2 está curado na origem do teste e da conversão:** o esperado agora é `True` literal; `dia_civil` e `eixo_civil` morrem na asserção da linha 51; `eixo_pela_hora` morre na linha 83. Um mutante adicional `if False` na absolvição também morreu na linha 51, com duas falhas. Portanto a lacuna concreta de c2 não permanece. (`app/chamados/tests/test_bug_par_sem_banco.py:50`, `app/chamados/tests/test_bug_par_sem_banco.py:51`, `app/chamados/tests/test_bug_par_sem_banco.py:83`, `app/chamados/services/materializacao.py:443`.)

**F3 está curado:** `c3_aviso_base` restaura exatamente `de outro(s) dia(s)` e morreu pela `assertNotRegex`; não morreu por import/erro. (`app/chamados/tests/bug_par_c2_mutantes.py:26`, `app/chamados/tests/test_bug_par_c2_sem_banco.py:102`.)

Lacunas de prova:

1. O novo teste F4 chega a zero alterando K6 de aberto para entregue, **não executando validação**; `resolvidas` permanece zero na fixture. Não prova o prometido zeramento pela validação nem protege contra `pendentes_aval += resolvidas`. (`app/chamados/tests/test_bug_par_103.py:87`, `app/chamados/tests/test_bug_par_103.py:89`, `app/chamados/tests/test_bug_par_103.py:94`.)
2. Não há mutante de **template** declarado. `fundir_a_em_b` e `fundir_a_em_b_porta` alteram testemunha e porta; `f6_esconde` também altera testemunha. A morte da fusão nos templates fica **[nao verificado]**. (`app/chamados/tests/bug_par_mutantes.py:94`, `app/chamados/tests/bug_par_mutantes.py:99`, `app/chamados/tests/bug_par_mutantes.py:103`.)
3. O teste matricial L3 cita `dxg_desligado`, mas o catálogo aponta esse mutante para o teste puro, **não para o teste matricial**; a morte de L3/HTML por esse mutante não foi executada. (`app/chamados/tests/test_bug_par_volta.py:234`, `app/chamados/tests/bug_par_mutantes.py:27`.)
4. L4 é declarado controle e não tem mutante próprio associado no catálogo. (`app/chamados/tests/test_bug_par_volta.py:245`, `app/chamados/tests/bug_par_mutantes.py:25`.)
5. A comparação HTML da matriz procura **qualquer formulário Validar no modal**, sem restringir à pergunta; a espera é restringida pela PK. Na fixture comum as duas perguntas recebem os mesmos bloqueios, mas esse assert pode ocultar divergência por pergunta em cenários mistos. (`app/chamados/tests/test_bug_par_volta.py:201`, `app/chamados/tests/test_bug_par_volta.py:203`, `app/chamados/tests/test_bug_par_volta.py:204`.)

`command_txt` **não deve ser chamado sobrevivente por leitura estática**: o teste verifica `col%s`, que nessa fixture aparece na PAUTA; retirar `exp` elimina essa linha, pois só a saída é respondida em `_preparar`. Morte efetiva continua **[nao verificado]**. (`app/chamados/tests/test_bug_par_103.py:73`, `app/chamados/tests/test_bug_par_projetado.py:521`, `app/chamados/management/commands/bug_par_103.py:62`.)

Os quatro testes vizinhos de eixo passaram substituindo somente `django.test.SimpleTestCase` por `unittest.TestCase` em memória; o algoritmo propositor permaneceu real. Não contam como suíte Django executada. (`app/ponto/tests/test_f2_eixo_turno.py:10`, `app/ponto/tests/test_f2_eixo_turno.py:18`, `app/ponto/tests/test_f2_eixo_turno.py:23`, `app/ponto/tests/test_f2_eixo_turno.py:29`.)

## 16/11 15:52 · sessao c86d117e9ca7 · - · - · origem=Ronald (tmux)

Achado concreto: o estado novo tem dois significados. Sem `data_evento`, `competencia_do_fato` retorna `None`; a guarda produz impedimento por falta de dia; testemunha e porta rotulam esse impedimento como **`bloqueada_competencia_entregue`**, embora K6 nem seja consultado. Reproduzido pelas funções reais extraídas em memória, sem banco nem escrita. (`app/chamados/services/materializacao.py:780`, `app/chamados/services/materializacao.py:800`, `app/chamados/services/materializacao.py:861`, `app/chamados/services/validacao.py:614`.)

| Linha | Juiz e operação | Persistência | Testemunha e HTMLs | Prova declarada |
|---|---|---|---|---|
| L1 entregue fora163 | K6: `app/folha/export.py:556`; composição pelo fato: `app/chamados/services/materializacao.py:772`; porta recusa antes do carimbo: `app/chamados/services/validacao.py:614`. | Nada: retorno em `app/chamados/services/validacao.py:616`. | Estado entregue: `app/chamados/services/materializacao.py:863`; card mostra detalhe/esconde Validar: `app/templates/chamados/partials/_pergunta_card.html:44`; modal idem: `app/templates/chamados/partials/modal_fio.html:252`, `app/templates/chamados/partials/modal_fio.html:274`. | Teste: `app/chamados/tests/test_bug_par_volta.py:207`; mutantes testemunha/porta: `app/chamados/tests/bug_par_mutantes.py:99`, `app/chamados/tests/bug_par_mutantes.py:103`; execução [nao verificado]. |
| L2 F6 sem entrega | F6: `app/ponto/janelas.py:75`; toque entra: `app/chamados/services/validacao.py:617`; chokepoint recusa batida: `app/ponto/registro_batida.py:156`. | Atos VALIDACAO e DEVOLUCAO: `app/chamados/services/validacao.py:170`, `app/chamados/services/validacao.py:262`; validação final nula: `app/chamados/services/validacao.py:256`; motivo: `app/chamados/services/materializacao.py:1610`; sem batida. | `bloqueada_competencia`: `app/chamados/services/materializacao.py:886`, `app/chamados/services/materializacao.py:910`; card conserva formulário: `app/templates/chamados/partials/_pergunta_card.html:100`; modal conserva: `app/templates/chamados/partials/modal_fio.html:306`. | Teste: `app/chamados/tests/test_bug_par_volta.py:220`; mutante `f6_esconde`: `app/chamados/tests/bug_par_mutantes.py:94`; execução [nao verificado]. |
| L3 U163 divergente | Comparação: `app/chamados/services/materializacao.py:720`; gate: `app/chamados/services/materializacao.py:732`; porta retorna antes de escrever: `app/chamados/services/validacao.py:571`. | Nada: `app/chamados/services/validacao.py:575`; materializador também retorna antes do bloco de escrita: `app/chamados/services/materializacao.py:1808`. | Espera: `app/chamados/services/materializacao.py:741`; card mostra espera e esconde Validar: `app/templates/chamados/partials/_pergunta_card.html:49`; modal idem: `app/templates/chamados/partials/modal_fio.html:264`, `app/templates/chamados/partials/modal_fio.html:274`. | Teste matriz: `app/chamados/tests/test_bug_par_volta.py:234`; **`dxg_desligado` aponta ao teste sem banco, não à matriz**: `app/chamados/tests/bug_par_mutantes.py:27`; morte da matriz [nao verificado]. |
| L4 nada barra | Derivador: `app/chamados/services/materializacao.py:900`, `app/chamados/services/materializacao.py:914`; porta valida/materializa: `app/chamados/services/validacao.py:617`, `app/chamados/services/validacao.py:645`. | Batidas: `app/ponto/registro_batida.py:176`; carimbos: `app/chamados/services/materializacao.py:1875`; atos: `app/chamados/services/validacao.py:170`. | `aplicavel`: `app/chamados/services/materializacao.py:900`; ambos mostram Validar: `app/templates/chamados/partials/_pergunta_card.html:100`, `app/templates/chamados/partials/modal_fio.html:306`. | Teste: `app/chamados/tests/test_bug_par_volta.py:245`; assert persistência: `app/chamados/tests/test_bug_par_projetado.py:103`; mutantes de par apontam outros testes: `app/chamados/tests/bug_par_mutantes.py:63`; execução [nao verificado]. |

`fio.py` e o filtro do card transportam o dict sem fundir os estados; só adaptam ausência/exceção para indisponível. (`app/chamados/services/fio.py:373`, `app/chamados/templatetags/chamados_tags.py:134`.) Os HTMLs decidem pelo estado e exibem o detalhe, sem interpretar seu texto. (`app/templates/chamados/partials/_pergunta_card.html:44`, `app/templates/chamados/partials/modal_fio.html:252`.)

A fusão no template não tem mutante declarado; os mutantes existentes atingem testemunha e porta. O runner modifica funções Python, sem mecanismo de mutação de template. (`app/chamados/tests/bug_par_mutantes.py:99`, `app/chamados/tests/bug_par_mutantes.py:182`.) As asserções de tela da L1 deveriam detectar a fusão que reofereça Validar, mas sua morte aplicada permanece [nao verificado]. (`app/chamados/tests/test_bug_par_volta.py:214`.)

O teste F5 não cobre essa duplicidade semântica: verifica comando e devolução U103, sem chamar testemunha ou `validar_pergunta` para a pergunta fora163 sem dia. (`app/chamados/tests/test_bug_par_103.py:101`.)

## 16/11 15:51 · sessao 3efa062c8f48 · - · - · origem=Ronald (tmux)

F4 foi corrigido na origem do rótulo e do recorte: o contador separa pendência, pertencimento congelado, resolvidas, entregues e sem dia; o comando pede o mesmo recorte por empresa e imprime os dois números do próprio contador. (`app/chamados/services/materializacao.py:321`, `app/chamados/services/materializacao.py:326`, `app/chamados/services/materializacao.py:354`, `app/chamados/management/commands/bug_par_103.py:79`.)

| Valor | Escritor | Leitores/semântica |
|---|---|---|
| `pendentes_aval` | `universo + len(b103)` | Comando e teste; trabalho ainda não carimbado, mais U103 atualmente devolvível. (`app/chamados/services/materializacao.py:356`, `app/chamados/management/commands/bug_par_103.py:82`, `app/chamados/tests/test_bug_par_103.py:88`.) |
| `congelados_no_recorte` | `len(qs)` após filtro de disputas | Comando e testes; pertencimento U163 presente no recorte, independentemente do efeito. (`app/chamados/services/materializacao.py:321`, `app/chamados/services/materializacao.py:324`, `app/chamados/services/materializacao.py:356`, `app/chamados/management/commands/bug_par_103.py:83`.) |
| `resolvidas` | Incremento para carimbadas que já não atendem `paridade_absolvida_bug_par_103` | Só encontrei leitura na soma de partição do teste novo; significa “não é mais caso”, incluindo causas diversas, não necessariamente validação exitosa. (`app/chamados/services/materializacao.py:327`, `app/chamados/services/materializacao.py:355`, `app/chamados/tests/test_bug_par_103.py:94`.) |
| `bug_par_103_pauta_dp` | Lista de candidatas atuais cujo K6 é verdadeiro | Só encontrei leitura no teste; `(pk, 'MM/AAAA')`. (`app/chamados/services/materializacao.py:330`, `app/chamados/services/materializacao.py:334`, `app/chamados/tests/test_bug_par_103.py:93`.) |
| `bug_par_103_sem_dia` | Lista de candidatas atuais cujo helper retorna `None` | Só encontrei leitura na soma de partição do teste. (`app/chamados/services/materializacao.py:331`, `app/chamados/services/materializacao.py:355`, `app/chamados/tests/test_bug_par_103.py:95`.) |
| `total_aval`, removida | Não há escritor atual | O único literal restante encontrado é a asserção de ausência no stdout; os consumidores encontrados foram migrados. (`app/chamados/tests/test_bug_par_103.py:99`, `app/chamados/tests/test_bug_par_c2_sem_banco.py:63`, `app/chamados/tests/test_bug_par_projetado.py:533`, `app/chamados/tests/test_bug_par_volta.py:88`.) |

A prova nova de F4 **não demonstra zeramento pela validação**: mede `(1,1)`, muda o mock K6 para entregue e mede `(0,1)`; não valida nem devolve entre essas duas medições. O mutante declarado só ataca a classificação da entregue. Um acréscimo indevido de `resolvidas` a `pendentes_aval` não seria observado nessa fixture, porque ela mantém `resolvidas=0`. (`app/chamados/tests/test_bug_par_103.py:86`, `app/chamados/tests/test_bug_par_103.py:89`, `app/chamados/tests/test_bug_par_103.py:91`, `app/chamados/tests/bug_par_mutantes.py:116`.)

O código permite cair pela validação: pergunta carimbada deixa o ramo não materializado e entra em `resolvidas` quando não é mais caso U103; a verificação desse percurso pelo contador após validar permanece ausente no teste novo. (`app/chamados/services/materializacao.py:326`, `app/chamados/services/materializacao.py:328`, `app/chamados/tests/test_bug_par_projetado.py:556`, `app/chamados/tests/test_bug_par_projetado.py:560`.)

F5 foi corrigido na origem comum: `competencia_do_fato` devolve `None` antes de consultar K6 quando falta `data_evento`; com data, delega à janela existente e ao juiz K6 existente. Guarda, contador, devolução e comando consomem esse mesmo contrato. (`app/chamados/services/materializacao.py:780`, `app/chamados/services/materializacao.py:784`, `app/chamados/services/materializacao.py:799`, `app/chamados/services/materializacao.py:330`, `app/chamados/services/validacao.py:321`, `app/chamados/management/commands/bug_par_103.py:58`.)

O comando publica a linha sem dia tanto para U103 quanto para fora163, e a devolução retorna sem descarimbar, desvalidar ou registrar DEVOLUÇÃO; o teste prepara ambas as perguntas sem data e exige as duas linhas e fotografia inalterada. (`app/chamados/management/commands/bug_par_103.py:62`, `app/chamados/management/commands/bug_par_103.py:75`, `app/chamados/services/validacao.py:322`, `app/chamados/services/validacao.py:329`, `app/chamados/tests/test_bug_par_103.py:107`, `app/chamados/tests/test_bug_par_103.py:116`, `app/chamados/tests/test_bug_par_103.py:123`.)

Há **ambiguidade residual do estado entregue**: falta de dia fora163 também produz texto impeditivo, que `entrega_que_barra_a_validacao` propaga; a testemunha e a porta o rotulam `bloqueada_competencia_entregue`, embora K6 sequer tenha sido consultado. Portanto esse estado significa tanto “entregue” quanto “não sei a competência por ausência de dia”. (`app/chamados/services/materializacao.py:780`, `app/chamados/services/materializacao.py:800`, `app/chamados/services/validacao.py:218`, `app/chamados/services/materializacao.py:863`, `app/chamados/services/validacao.py:616`.)

A seleção do stdout `elegiveis` continua sendo todas as PKs U103 presentes no recorte, enquanto a chave `bug_par_103` agora seleciona apenas candidatas atuais devolvíveis; os dois números não são equivalentes. (`app/chamados/management/commands/bug_par_103.py:53`, `app/chamados/services/materializacao.py:327`, `app/chamados/services/materializacao.py:335`.)

As listas novas particionam a consulta U163 congelada; não reconstroem seu pertencimento por recusa, lâmpada ou K6. O carregador continua validando hash, cardinalidade e conjuntos imutáveis. (`app/chamados/services/materializacao.py:320`, `app/chamados/services/materializacao.py:321`, `app/chamados/services/universo_bug_par.py:14`, `app/chamados/services/universo_bug_par.py:20`, `app/chamados/services/universo_bug_par.py:30`.)

Execução dos testes de integração e mortes dos mutantes F4/F5: **[nao verificado]**; não acessei banco nem logs. [nao verificado]

## 16/11 15:50 · sessao de3443daed46 · O50-CELULA-APOS-DESLIGAMENTO · verificador · origem=Code

**O item 1 precisa voltar para fechar o alcance temporal, a associação das retratadas ao turno e a prova do efeito novo.** A operação existente muda conforme o dia, e as funções públicas de associação de batidas excluem retratadas. (`app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`; `app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2942`.)

Identidade Git da cópia com o HASH informado, execução em produção e hashes antes/depois: [nao verificado].

**1. Premissas da operação e das entradas**

A operação é `regenerar_celulas_vinculo`, dentro da operação registrada `gerar` da porta canônica de `CelulaDia`; o command `gerar_celulas.py` apenas a reexporta. Chamá-la pelas portas de desligamento/vínculo preserva o escritor único. (`app/ponto/portas/celula.py:74`; `app/ponto/portas/celula.py:112`; `app/core/portas.py:112`; `app/ponto/management/commands/gerar_celulas.py:13`.)

| Dia fora da vigência | O que a porta grava hoje |
|---|---|
| **Futuro civil: `d > hoje`** | Conserva a linha; grava `trabalha=False`, `escala_geradora=None`, DNA sem vínculo, `dna_anterior`, `regenerada_em` e incremento de `regeneracoes`; chama o desfazimento do julgamento futuro. Exige origem gerada, geradora igual ao vínculo, competência não bloqueada e nenhum outro vínculo cobrindo o dia. (`app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:259`; `app/ponto/portas/celula.py:265`; `app/ponto/portas/celula.py:267`.) |
| **Hoje/passado: `d <= hoje`** | Conserva a linha e a geradora; grava **somente** `trabalha=False` e `montar_dna(ec,d,False)`. Esse DNA mantém `vinculo_id=ec.id`; o ramo não grava os três campos de denúncia. (`app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:277`; `app/ponto/management/commands/gerar_celulas.py:104`.) |

Portanto, “a operação que já existe grava DNA sem vínculo” confere **apenas para o ramo futuro civil**, não para qualquer dia posterior ao fim. “Após D” e “após hoje” são limites diferentes no código. (`app/ponto/portas/celula.py:229`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`.)

Os caminhos já existem:

- `executar_desligamento` → `fechar_vinculos_ativos` → `fechar_vigencia`: fecha por `update()` e chama a regeneração de futuras existentes com `recalculo=False`. O desligamento agendado retorna antes desse caminho. (`app/colaboradores/services/desligamento.py:107`; `app/colaboradores/services/desligamento.py:126`; `app/colaboradores/services/desligamento.py:147`; `app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:662`; `app/colaboradores/services/vinculo.py:862`.)
- `encerrar_vinculo`: salva `ativa=False/data_fim`; o signal regenera hoje/passado e chama o suplemento das futuras. Já há efeito sobre células nessa entrada. (`app/colaboradores/services/vinculo.py:843`; `app/escala/signals.py:73`; `app/escala/signals.py:80`.)

Esses caminhos **não entregam automaticamente as novas exclusões**: os ramos fora da vigência não consultam batida, afastamento ou `competencia_ja_paga`; usam a guarda atual de lavra. (`app/ponto/portas/celula.py:218`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`.)

A justificativa sobre leitores precisa ser estreitada: sem célula, `eh_dia_trabalho` consulta a **aritmética do vínculo**, não diretamente o template; `marcos_do_dia` consulta o template tanto sem célula quanto com `dna.marcos` vazio. Isso prova a possibilidade de retorno ao cálculo/template, não que “o previsto voltaria” em todo caso: o juiz de minutos retorna zero sem vínculo ou quando o dia não trabalha. (`app/escala/models.py:1094`; `app/escala/models.py:1096`; `app/escala/models.py:1124`; `app/escala/models.py:1133`; `app/escala/utils.py:2260`; `app/escala/utils.py:2266`.)

**2. Conferência literal das condições (1) a (5)**

**(1) Não paga.** `competencia_ja_paga` está registrada como obra, incluindo exportação vigente ou pagamento externo por cadastro; implementação disponível nessa cópia: [nao verificado]. O registro condiciona o pouso à espinha e exige migração conjunta dos leitores. Esperar esse pouso fecha como dependência futura; não autoriza substituí-lo pelos juízes atuais. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)

Hoje a célula usa `_competencias_exportadas`, que enumera candidatos e pergunta a `competencia_fechada_para_celula`; esta compõe exportação vigente e holerite publicado da pessoa. Isso não constitui prova de pagamento externo cadastrado. (`app/ponto/portas/celula.py:221`; `app/ponto/management/commands/gerar_celulas.py:155`; `app/ponto/management/commands/gerar_celulas.py:181`; `app/folha/export.py:618`; `app/folha/export.py:622`.)

**(2) Protegidos.** Editada e outro vínculo têm guardas existentes nos dois ramos. `_outro_vinculo_cobre` mora no **command**, sendo importada pela porta; consulta outros vínculos e delega a cobertura a `vinculo_cobre_a_data`. (`app/ponto/portas/celula.py:142`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`; `app/ponto/management/commands/gerar_celulas.py:269`; `app/ponto/management/commands/gerar_celulas.py:283`.)

Excluir passivo não resolve sozinho o caso de **novo encerramento com fim passado**: a porta aceita `data_fim` validada contra o início, e o signal alcança dias até hoje. O contrato precisa dizer se esses dias entram no efeito novo; se entrarem, o ramo atual não fornece DNA sem vínculo nem denúncia da reescrita. (`app/colaboradores/services/vinculo.py:835`; `app/escala/signals.py:60`; `app/escala/signals.py:73`; `app/ponto/portas/celula.py:272`.)

**(3) Qualquer batida, pelo turno OU pelo civil.** As autoridades existentes respondem perguntas diferentes:

| Associação | Função e limite |
|---|---|
| Dia civil local | `data_local(timestamp)` usa `timezone.localtime(...).date()`. (`app/ponto/turnos.py:45`.) |
| Batidas reclamadas pelo turno | `batidas_do_dia_turno` lê `turnos_do_colab` e seleciona `data_turno == dia`; inclui a cauda noturna, mas exclui retratadas por construção. (`app/ponto/turnos.py:3096`; `app/ponto/turnos.py:3107`; `app/ponto/turnos.py:3115`.) |
| Dia de cada batida, incluindo órfã viva | `dia_das_batidas` entrega o dia do turno quando reclamado; para as demais, entrega o civil. **Não entrega os dois dias.** (`app/ponto/turnos.py:2898`; `app/ponto/turnos.py:2961`; `app/ponto/turnos.py:2967`.) |
| Retratada ligada ao turno | `dia_das_batidas` filtra retratadas e `dia_do_turno_da_batida` declara `None` para elas. Associação da retratada noturna à véspera: [nao verificado]. (`app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2939`.) |

A condição exige uma **união explícita**: uma saída de madrugada pode proteger tanto o dia do turno quanto seu dia civil. Consultar apenas `dia_das_batidas`, ou juntar turno vivo com cru civil, não prova a proteção da véspera para uma retratada. Essa associação precisa fechar sem clonar a geometria. (`app/ponto/turnos.py:2965`; `app/ponto/turnos.py:2967`; `app/ponto/turnos.py:2942`.)

**(4) Afastado.** Existe `para='cobranca'`, mas **não existe argumento `data=`**. A chamada real é `afastado_hoje(colab, dia=dia_celula, para='cobranca')`; retorna `Ausencia` ou `None`. Cobrança exclui apenas rejeitadas, e o afastamento consulta atualmente `afastamento_inss`. (`app/ponto/turnos.py:2737`; `app/ponto/turnos.py:2743`; `app/ponto/turnos.py:2530`; `app/ponto/turnos.py:2716`.)

**(5) Hash igual no efeito novo.** O hash existente `hash_do_gravado` cobre **somente `FechamentoMensal`**, com exclusões declaradas, incluindo carimbos de controle. Não cobre `CelulaDia` nem `DiaPago`; sozinho não certifica a condição composta. (`app/ponto/portas/regen_exportada.py:37`; `app/ponto/portas/regen_exportada.py:56`; `app/ponto/portas/regen_exportada.py:62`; `app/ponto/portas/regen_exportada.py:71`.)

Para provar o **efeito novo**, o brief precisa definir os campos/linhas protegidos e comparar o mesmo ato com e sem o novo efeito, incluindo callbacks após commit. Comparar simplesmente antes/depois de `encerrar_vinculo` mistura a obra com o comportamento anterior: o `save()` já invalida previsto sem filtro de competência, e a regeneração pode agendar recálculo. (`app/escala/signals.py:145`; `app/ponto/services/fechamento.py:1607`; `app/ponto/portas/celula.py:483`; `app/ponto/services/fechamento.py:1824`.) Resultado dessa prova: [nao verificado].

**3. Cortes, dinheiro e retroatividade**

Há uma correção na pergunta: **LU-13 não contém “remover”** no texto disponível; fixa direção, “futuras = após D”, exceções e `!` retroativo. **“Remover com trilha” está na LU-14.** Conservar a linha respeita as exclusões da LU-13; equivaler a “remover” da LU-14 não está demonstrado no literal. Essa é a pergunta de interpretação que o próprio complemento manda levantar, sem presumir equivalência. (`app/docs/CORTES.md:470`; `app/docs/CORTES.md:471`.)

Se o efeito novo alcançar hoje/passado, reutilizar o ramo atual sem alteração confronta a exigência de denúncia por `regenerada_em/regeneracoes/dna_anterior`. A auditoria geral da regeneração também captura falha e segue; sua existência não garante gravação obrigatória da trilha em todo ato. (`CLAUDE.md:149`; `CLAUDE.md:152`; `app/ponto/portas/celula.py:277`; `app/ponto/portas/celula.py:369`; `app/ponto/portas/celula.py:388`.)

A proibição de resolver cadastro/vínculo futuro por código está alinhada à L-099; a porta atual preserva os vínculos que começam depois do fim informado. (`app/docs/LEIS.md:122`; `app/colaboradores/services/vinculo.py:665`.)

**Dinheiro é alcançável pelo caminho escolhido**, mesmo sem tocar `motor_calculo_v2.py`: o rabo da regeneração chama recálculo por competência; este grava `FechamentoMensal` e lavra `DiaPago`. A lavratura pode apagar e recriar todas as linhas daquela competência/versão. Logo, o alcance financeiro não fica limitado fisicamente à célula-dia. (`app/ponto/portas/celula.py:483`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1200`; `app/ponto/services/dia_pago.py:359`.)

A regra consolidada exige reversão anterior, hash de exportada intacta e prova de frota para dinheiro não exportado; mantém `!` para paga/vínculo ou escala retroativo e caminho próprio para alteração de exportada. Reutilizar uma porta existente não dispensa essas condições. (`CLAUDE.md:404`; `CLAUDE.md:406`; `CLAUDE.md:411`.)

**4. Quem escreve e quem lê os campos e funções envolvidos**

O mapa abaixo identifica consumidores concretos; exaustividade do futuro censo previsto no PREPARA: [nao verificado].

| Campo/função | Quem escreve ou produz | Quem lê |
|---|---|---|
| `Colaborador.situacao`, `data_demissao` | `executar_desligamento`, pela porta do colaborador. (`app/colaboradores/services/desligamento.py:129`.) | Situação exibida lê desligamento; grade lê a borda da demissão. (`app/colaboradores/services/situacao_exibida.py:56`; `app/escala/services/leitor_celula.py:389`.) |
| Vínculo: `data_inicio`, `data_fim`, `ativa`; cobertura do sucessor | Encerramento escreve `ativa/data_fim`; criação/alteração de `data_inicio` no escopo novo: [nao verificado]. (`app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:843`.) | Signal compara vigência; `_outro_vinculo_cobre` chama `vinculo_cobre_a_data`. (`app/escala/signals.py:25`; `app/escala/signals.py:56`; `app/ponto/management/commands/gerar_celulas.py:283`; `app/escala/alimentacao.py:214`.) |
| `CelulaDia.colaborador/data/trabalha/escala_geradora/dna` | Nascimento e regeneração na porta; DNA produzido por `montar_dna`. (`app/ponto/portas/celula.py:293`; `app/ponto/portas/celula.py:259`; `app/ponto/management/commands/gerar_celulas.py:73`.) | Trabalho: `eh_dia_trabalho`; geradora: `vinculo_do_dia`; DNA/marcos: `marcos_do_dia`; cobrança: `dia_sem_trabalho`. (`app/escala/models.py:1094`; `app/escala/alimentacao.py:270`; `app/escala/models.py:1124`; `app/chamados/juizes.py:459`.) |
| DNA sem vínculo: `vinculo_id`, `tipo_escala_id`, `tipo_ciclo`, `marcos`, `minutos_jornada`, `intervalo_livre`, `lampadas` | `montar_dna(None,...)` produz essas chaves nulas. (`app/ponto/management/commands/gerar_celulas.py:80`.) | Reversão lê `vinculo_id/marcos`; leitor de marcos lê `dna.marcos`; consumo exaustivo das demais chaves: [nao verificado]. (`app/ponto/portas/celula.py:829`; `app/ponto/portas/celula.py:832`; `app/escala/models.py:1124`.) |
| `origem`, `editada_em`, `editada_por` | `editar_dia`. (`app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:14`.) | Regeneração protege origem não gerada; juiz de reescrita lê `editada_em`. (`app/ponto/portas/celula.py:251`; `app/escala/services/regeneracao.py:84`.) |
| `dna_anterior`, `regenerada_em`, `regeneracoes` | Regeneração futura e regeneração comum. (`app/ponto/portas/celula.py:262`; `app/ponto/portas/celula.py:307`.) | Reversão lê DNA anterior e contador; cobrança órfã lê carimbos de reescrita. (`app/ponto/portas/celula.py:818`; `app/ponto/portas/celula.py:835`; `app/escala/services/regeneracao.py:87`.) |
| Julgamento: `ata`, `veredito/via/em`, `protestos`, `status_dia/rotulo`; carimbos `julgada_em/impressao/ausencia/tem_entrada/tem_saida/insumos_em` | Operações `lavrar_veredito/carimbar`; desfazimento futuro zera a lista, preservando decisão via admin. (`app/ponto/portas/celula.py:1096`; `app/ponto/portas/celula.py:1112`; `app/ponto/portas/celula.py:1155`; `app/ponto/portas/celula.py:1165`.) | Grade lê ata/veredito; juiz de cobrança lê ata/trabalho; desfazimento lê os campos e a via. (`app/escala/services/leitor_celula.py:397`; `app/escala/services/leitor_celula.py:424`; `app/chamados/juizes.py:454`; `app/ponto/portas/celula.py:1149`.) |
| `Batida.colaborador/timestamp/tipo/retratada_em` | `registrar_batida`; retratação por `retratar_batida` → `anotar`. (`app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:158`; `app/ponto/registro_batida.py:276`.) | Apuração e associação por dia; as consultas públicas citadas filtram retratada. (`app/ponto/turnos.py:2888`; `app/ponto/turnos.py:2927`.) |
| `Turno.data_turno`; dia civil | `_mk` recebe a resposta de `_data_do_turno`; civil produzido por `data_local`. São respostas calculadas, não nova escrita em `Batida`. (`app/ponto/turnos.py:43`; `app/ponto/turnos.py:318`; `app/ponto/turnos.py:45`.) | `_dia_das_batidas_puro` e `batidas_do_dia_turno`. (`app/ponto/turnos.py:2965`; `app/ponto/turnos.py:2967`; `app/ponto/turnos.py:3117`.) |
| `Ausencia.tipo/data/data_fim/status`; `afastado_hoje` | Registro/edição pela porta de ausência; estado por `mudar_estado`. O juiz só consulta. (`app/ponto/services/ausencia.py:262`; `app/ponto/services/ausencia.py:648`; `app/ponto/services/ausencia.py:976`; `app/ponto/turnos.py:2743`.) | `afastamentos_no_dia/_pelo_efeito`; situação exibida chama o juiz. (`app/ponto/turnos.py:2727`; `app/ponto/turnos.py:2730`; `app/ponto/turnos.py:2530`; `app/colaboradores/services/situacao_exibida.py:63`.) |
| `competencia_ja_paga` e cadastro de pagamento externo | Escritor operacional: [nao verificado]; cadastro e trilha são obrigação registrada. (`app/docs/BACKLOG.md:1978`.) | Migração conjunta dos leitores, inclusive célula/fechamento/fio, está registrada; funcionamento após pouso: [nao verificado]. (`app/docs/BACKLOG.md:1978`.) |
| Valores de `FechamentoMensal` e rubricas/minutos de `DiaPago` | `recalcular_fechamento_mes` e `dia_pago.lavrar`; campos financeiros declarados em `CAMPOS_DIA/CAMPOS_AJUSTE`. (`app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1200`; `app/ponto/services/dia_pago.py:34`; `app/ponto/services/dia_pago.py:359`.) | Export lê fechamento; cartão lê fechamento/lavratura por seus serviços. (`app/folha/export.py:402`; `app/relatorios/cartao_pela_celula.py:79`; `app/relatorios/cartao_pela_celula.py:142`.) |
| `FechamentoMensal.previsto_em/atualizado_em` | `invalidar_previsto`, acionado pelo `save()` do vínculo, sem filtro de competência. (`app/escala/signals.py:145`; `app/ponto/services/fechamento.py:1610`.) | Prontidão do fechamento. (`app/folha/export.py:1025`.) |
| Hash; lista nominal/Pauta DP da nova obra | Hash atual produzido por `hash_do_gravado`; escritor da lista/Pauta específica da O50: [nao verificado]. (`app/ponto/portas/regen_exportada.py:62`.) | Verificação dos três modelos e destino administrativo no efeito novo: [nao verificado]. |

**VEREDITO: AVAL-DEVOLVIDO**

- “Remover” da **LU-14** aceita neutralizar conservando a linha, ou exige exclusão física? O texto disponível da LU-13 não resolve essa equivalência. (`app/docs/CORTES.md:470`; `app/docs/CORTES.md:471`.)
- O efeito novo alcança somente `dia > hoje`, ou também dias passados após um novo encerramento retroativo? No segundo caso, como fecha DNA sem vínculo, denúncia e autorização retroativa? (`app/ponto/portas/celula.py:272`; `CLAUDE.md:149`; `CLAUDE.md:404`.)
- Qual autoridade dará o dia do turno da **retratada**, compondo a união com o civil sem segundo juiz? (`app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2942`; `app/ponto/turnos.py:2967`.)
- Corrige-se a chamada para `afastado_hoje(colab, dia=dia_celula, para='cobranca')`? (`app/ponto/turnos.py:2737`.)
- Quais valores/linhas entram nos hashes dos três modelos e como se isola o efeito novo dos efeitos anteriores e após commit? (`app/ponto/portas/regen_exportada.py:71`; `app/ponto/services/fechamento.py:1610`; `app/ponto/services/fechamento.py:1824`.)

## 16/11 15:47 · sessao d773d8ad8c47 · - · - · origem=Ronald (tmux)

- A porta canônica de `CelulaDia` é `app/ponto/portas/celula.py`, com as operações registradas em `OPERACOES`; chamar esse módulo pelas portas de desligamento/vínculo mantém um escritor, enquanto escrita direta nessas portas contrariaria o registro. (`app/core/portas.py:107-113`; `app/ponto/portas/celula.py:74-84`.)

- **A premissa da operação existente é parcialmente correta.** Fora da vigência, **futuro** (`d > hoje`) exige célula gerada pelo vínculo encerrado, competência fora da lavra e ausência de outro vínculo; grava `trabalha=False`, `escala_geradora=None`, `dna=montar_dna(None,…)`, `dna_anterior`, `regenerada_em` e incremento de `regeneracoes`; depois desfaz o julgamento futuro. (`app/ponto/portas/celula.py:251-271`.) O DNA sem vínculo tem `vinculo_id`, `tipo_escala_id`, `tipo_ciclo`, `marcos`, `minutos_jornada`, `intervalo_livre` e `lampadas` nulos. (`app/ponto/management/commands/gerar_celulas.py:73-84`.)

- Para **hoje/passado** fora da vigência, a operação existente é diferente: exige `trabalha is not False`, conserva a geradora e grava somente `trabalha=False` e `montar_dna(ec,d,False)`; conserva `dna.vinculo_id=ec.id`, sem os três campos de denúncia nesse ramo. Portanto, “usa a operação existente, DNA sem vínculo” precisa explicitar seu alcance futuro ou reconhecer mudança dessa operação para hoje/passado. (`app/ponto/portas/celula.py:272-282`; `app/ponto/management/commands/gerar_celulas.py:104-112`.)

- Os dois ramos não consultam batidas, afastamento ou `competencia_ja_paga`; atualmente a guarda de competência vem de `_competencias_exportadas`, salvo `apesar_da_lavra` preenchido. (`app/ponto/portas/celula.py:218-221`; `app/ponto/portas/celula.py:251-279`.) `_competencias_exportadas` enumera TXT/holerite publicado e consulta `folha.export.competencia_fechada_para_celula`; isso não é a autoridade futura nomeada no contrato. (`app/ponto/management/commands/gerar_celulas.py:155-188`.)

- `executar_desligamento` já chama `fechar_vinculos_ativos`, que chama `fechar_vigencia`; o `update()` de vigência é seguido de `regenerar_futuras_do_cadastro(..., recalculo=False, substitui_dono=True)`. (`app/colaboradores/services/desligamento.py:147-149`; `app/colaboradores/services/vinculo.py:855-865`; `app/colaboradores/services/vinculo.py:641-664`.) Esse suplemento seleciona somente futuras geradas já existentes e chama a porta por dia, sem horizonte estendido. (`app/ponto/services/cadastro_futuro.py:26-45`.) O agendado retorna antes desse caminho; vínculos com início posterior ao fim são recusados e preservados. (`app/colaboradores/services/desligamento.py:107-126`; `app/colaboradores/services/vinculo.py:622`; `app/colaboradores/services/vinculo.py:665-670`.)

- `encerrar_vinculo` já salva `ativa=False/data_fim`; o signal de vigência chama a porta para hoje/passado e o suplemento para futuro. (`app/colaboradores/services/vinculo.py:835-851`; `app/escala/signals.py:54-83`.) Logo, já existe caminho acionável sem segundo escritor, mas reaproveitá-lo sem acrescentar as novas guardas não implementa as condições literais do aval. (`app/core/portas.py:112`; `app/ponto/portas/celula.py:251-279`.)

- `_outro_vinculo_cobre` **mora no command**, sendo importada pela porta; consulta outros vínculos da pessoa e pergunta a `escala.alimentacao.vinculo_cobre_a_data`. (`app/ponto/portas/celula.py:141-143`; `app/ponto/management/commands/gerar_celulas.py:269-287`.) A proteção vale nos dois ramos fora da vigência; geradora diferente também fica preservada. (`app/ponto/portas/celula.py:251-276`.)

- `eh_dia_trabalho` lê `cel.trabalha`; sem célula, volta a `eh_dia_trabalho_calculado`, **não diretamente ao template**. (`app/escala/models.py:1094-1096`.) `marcos_do_dia` usa o template sem célula **e também com célula cujo `dna.marcos` é vazio**; portanto conservar a linha não elimina sozinho todo retorno aos marcos do template. (`app/escala/models.py:1124-1133`.) O comentário documenta esse comportamento inclusive para folga. (`app/escala/models.py:1151-1158`.)

- A operação chama `_marcar_cobrancas_orfas` e registra evento de regeneração quando altera células; a trilha de regeneração é best-effort, com exceção capturada. (`app/ponto/portas/celula.py:327`; `app/ponto/portas/celula.py:369-389`.) O rabo comum pode rejulgar células passadas, recalcular dinheiro por competência e agendar rejunção/reavaliação de falta; `recalculo=False` elimina julgamento/dinheiro, mas não elimina todo o rabo de eventos. (`app/ponto/portas/celula.py:460-496`; `app/ponto/portas/celula.py:501-528`.)

Mapa de campos e consumidores para o censo:

| Campo/função | Quem escreve | Quem lê |
|---|---|---|
| `EscalaColaborador.ativa/data_fim` | `fechar_vigencia`, `encerrar_vinculo`. (`app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:843-845`.) | Signal compara vigência anterior/atual; `_outro_vinculo_cobre` alimenta juiz de cobertura. (`app/escala/signals.py:25-28`; `app/escala/signals.py:56-68`; `app/ponto/management/commands/gerar_celulas.py:283-287`.) |
| `CelulaDia.trabalha` | `regenerar_celulas_vinculo`, nos ramos fora da vigência. (`app/ponto/portas/celula.py:259-266`; `app/ponto/portas/celula.py:277-279`.) | `eh_dia_trabalho`; juiz `dia_sem_trabalho`; leitor da grade. (`app/escala/models.py:1094-1096`; `app/chamados/juizes.py:459-460`; `app/escala/services/leitor_celula.py:398-428`.) |
| `CelulaDia.escala_geradora` | Ramo futuro limpa; ramo comum regenera. (`app/ponto/portas/celula.py:260-266`; `app/ponto/portas/celula.py:306-311`.) | `vinculo_do_dia` lê o carimbo. (`app/escala/alimentacao.py:270`.) |
| `CelulaDia.dna` | Porta usa produtor `montar_dna`. (`app/ponto/portas/celula.py:261`; `app/ponto/portas/celula.py:278`; `app/ponto/management/commands/gerar_celulas.py:73`.) | `marcos_do_dia`. (`app/escala/models.py:1124-1133`.) |
| `dna_anterior/regenerada_em/regeneracoes` | Ramo futuro e regeneração comum. (`app/ponto/portas/celula.py:262-266`; `app/ponto/portas/celula.py:307-311`.) | Carregador/juiz de reescrita consulta `regenerada_em`; reversão usa denúncia conforme contrato da função. (`app/escala/services/regeneracao.py:72-105`; `app/ponto/portas/celula.py:247-249`.) |
| `origem/editada_em/editada_por` | Operação humana declarada `editar_dia`. (`app/ponto/portas/celula.py:14-16`; `app/ponto/portas/celula.py:83-84`.) | Regeneração exige `origem='gerada'`; juiz de reescrita lê `editada_em`. (`app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`; `app/escala/services/regeneracao.py:84-105`.) |
| Julgamento futuro (`ata`, `veredito`, `status_*`, carimbos etc.) | `desfazer_julgamento_da_futura` zera lista de campos por `salvar_e_emitir`, preservando veredito via admin com trilha obrigatória. (`app/ponto/portas/celula.py:1112-1113`; `app/ponto/portas/celula.py:1155-1175`.) | A própria operação verifica existência do julgamento e via admin. (`app/ponto/portas/celula.py:1149-1155`.) |
| `FechamentoMensal` previsto | `invalidar_previsto` é chamado em todo post-save do vínculo, sem filtro de competência no signal. (`app/escala/signals.py:137-146`.) | A leitura seguinte recalcula previsto conforme docstring do signal; comprovação do caminho completo cabe ao censo do fechamento. (`app/escala/signals.py:139-143`.) |

A enumeração acima demonstra consumidores concretos, não exaustividade do futuro censo de leitores/escritores pedido pelo contrato. [nao verificado]

## 16/11 15:47 · sessao 73548f0a5ded · - · - · origem=Ronald (tmux)

- **(4) há erro literal de argumento:** assinatura é `afastado_hoje(colab, dia=None, *, para)`; `para='cobranca'` existe, mas `data=` não existe. A forma executável é `afastado_hoje(colab, dia=dia_celula, para='cobranca')`; retorna `Ausencia` ou `None`, não booleano estrito. (`app/ponto/turnos.py:2737`; `app/ponto/turnos.py:2743`.)

- **O efeito cobrança é suportado:** exclui apenas ausência `status='rejeitada'`; folha aceita somente aprovada. Afastamento é restrito a `TIPOS_AFASTADO=('afastamento_inss',)`, com cobertura por data única, intervalo fechado ou aberto nos tipos autorizados; vence maior data e maior pk. (`app/ponto/turnos.py:2523`; `app/ponto/turnos.py:2528`; `app/ponto/turnos.py:2716`; `app/ponto/turnos.py:2727`; `app/ponto/turnos.py:2730`; `app/ponto/turnos.py:2743`.)

- **(3) o dia do turno vivo já tem autoridade:** `turnos_do_colab` carrega dois dias anteriores e um posterior, pareia por vínculos/ata e devolve turnos com `data_turno` no intervalo; `batidas_do_dia_turno` coleta os pks desses turnos. Isso contempla saída no dia seguinte. (`app/ponto/turnos.py:1834`; `app/ponto/turnos.py:1847`; `app/ponto/turnos.py:1850`; `app/ponto/turnos.py:1997`; `app/ponto/turnos.py:2000`; `app/ponto/turnos.py:3115`.)

- **O dia civil é `data_local(timestamp)`, usando `timezone.localtime(...).date()`.** O juiz `dia_das_batidas` atribui primeiro o dia do turno por pk e somente às não reclamadas atribui o civil; sua saída não fornece a união turno **e** civil exigida pela cláusula “OU”. A união deve ser explícita: a mesma saída de madrugada pode proteger as duas células. (`app/ponto/turnos.py:45`; `app/ponto/turnos.py:48`; `app/ponto/turnos.py:2957`; `app/ponto/turnos.py:2965`; `app/ponto/turnos.py:2967`.)

- **Retratada permanece uma lacuna do desenho literal:** `batidas_apuraveis`, `turnos_do_colab` e `dia_das_batidas` excluem `retratada_em`; `dia_do_turno_da_batida` declara `None` para retratada. Portanto, ler o cru pelo civil não prova proteção da véspera à qual uma retratada noturna pertencia. O contrato precisa nomear como obter essa associação sem clonar juiz. (`app/ponto/turnos.py:2888`; `app/ponto/turnos.py:1868`; `app/ponto/turnos.py:1869`; `app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2939`; `app/ponto/turnos.py:2942`.)

- **Não confundir com `turnos_de_batidas`:** aceita lista carregada e chama o pareador, mas alimenta marcos do template, enquanto `turnos_do_colab` consulta vínculo do dia e papel da ata. Sua equivalência para retratadas não está demonstrada. (`app/ponto/turnos.py:1517`; `app/ponto/turnos.py:1533`; `app/ponto/turnos.py:1535`; `app/ponto/turnos.py:1932`; `app/ponto/turnos.py:1944`; `app/ponto/turnos.py:1965`.)

- **`competencia_ja_paga`:** nenhuma ocorrência em `app/**/*.py` na busca desta cópia; implementação disponível: **[nao verificado]**. A obra e a definição “exportação vigente OU paga fora por cadastro com trilha” estão no BACKLOG; seu pouso continua condicionado à espinha no registro. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)

Mapeamento de campos e funções destas condições:

| Dado/função lido | Quem escreve | Quem lê |
|---|---|---|
| `Batida.colaborador`, `timestamp`, `tipo` | `registrar_batida`, nos dois `create` (`app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:158`) | `turnos_do_colab` (`app/ponto/turnos.py:1868`), `dia_das_batidas` (`app/ponto/turnos.py:2927`), `batidas_apuraveis` (`app/ponto/turnos.py:2888`) |
| `Batida.retratada_em`, `aprovada=False` | `retratar_batida` chama `anotar` (`app/ponto/registro_batida.py:269`; `app/ponto/registro_batida.py:276`) | Os filtros anteriores; significado registrado no modelo (`app/ponto/models.py:64`; `app/ponto/models.py:67`) |
| `Turno.data_turno` | `_mk` chama `_data_do_turno`, construindo o `namedtuple`, sem campo persistido em `Batida` (`app/ponto/turnos.py:43`; `app/ponto/turnos.py:318`; `app/ponto/turnos.py:323`) | `batidas_do_dia_turno` e `_dia_das_batidas_puro` (`app/ponto/turnos.py:3117`; `app/ponto/turnos.py:2965`) |
| `Ausencia.tipo`, `data`, `data_fim` | criação por `criar_ausencia` e `save`; edição em `editar_ausencia_registro` (`app/ponto/services/ausencia.py:262`; `app/ponto/services/ausencia.py:562`; `app/ponto/services/ausencia.py:648`; `app/ponto/services/ausencia.py:659`) | `afastamentos_no_dia` (`app/ponto/turnos.py:2727`; `app/ponto/turnos.py:2730`) |
| `Ausencia.status` | `mudar_estado`, que grava status e campos derivados (`app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:976`; `app/ponto/services/ausencia.py:988`) | `_pelo_efeito`, usado pelo afastamento (`app/ponto/turnos.py:2528`; `app/ponto/turnos.py:2530`; `app/ponto/turnos.py:2727`) |
| `afastado_hoje` | função leitora, sem escrita no corpo (`app/ponto/turnos.py:2737`; `app/ponto/turnos.py:2743`) | situação exibida e aviso de batida (`app/colaboradores/services/situacao_exibida.py:63`; `app/ponto/services/afastado_avisa.py:39`) |
| `competencia_ja_paga` | escritor do futuro cadastro: **[nao verificado]** | migração dos leitores é obrigação da obra registrada (`app/docs/BACKLOG.md:1978`) |

O chokepoint registrado de `Batida` é `ponto/registro_batida.py`; não haveria motivo para a O50 escrever esses campos ao apenas protegê-los. (`app/core/portas.py:99`; `app/core/portas.py:103`.)

## 16/11 15:47 · sessao 8fec5aad9b26 · - · - · origem=Ronald (tmux)

- **Correção de referência:** o texto disponível da LU-13 não diz “remover”; define direção, “futuras = após D”, protegendo exportada/editada/com batida e exigindo medição antes e `!` para apply retroativo. “Remover com trilha” aparece na **LU-14**. Conservar a linha atende às exclusões da LU-13, mas sua equivalência literal a “remover” da LU-14 não está estabelecida; deve subir a pergunta prevista pelo próprio complemento, sem presumir revogação. (`app/docs/CORTES.md:470`; `app/docs/CORTES.md:471`.)

- **A operação não é uniforme para todo dia após `data_fim`:** no futuro civil (`d > hoje`), a porta grava `trabalha=False`, `escala_geradora=None`, `dna=montar_dna(None,...)`, `dna_anterior`, `regenerada_em` e `regeneracoes`, e desfaz o julgamento anterior. No dia corrente/passado, grava apenas `trabalha=False` e DNA com `ec`, conservando a geradora e sem os três campos de denúncia. Logo, a premissa genérica “operação existente = DNA sem vínculo” só confere para o ramo futuro. (`app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:259`; `app/ponto/portas/celula.py:265`; `app/ponto/portas/celula.py:267`; `app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:277`.)

- **Risco literal ainda aberto:** um novo encerramento informado com `data_fim` passada pode produzir dias novos da obra que também são passados civis; excluir o “passivo já existente” não fixa expressamente esse limite. O ramo existente não fornece a denúncia que a seção 4 exige para reescrita de passado. O contrato precisa restringir a atuação nova a `dia > hoje` ou explicar a autorização e a denúncia para o outro ramo. (`app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:279`; `CLAUDE.md:149`; `CLAUDE.md:152`; `app/docs/CORTES.md:470`.)

- **Trilha atual não é garantia suficiente de sucesso:** existe chamada de auditoria ao final da regeneração, mas ela está em `try/except`, e sua falha é apenas registrada no logger; portanto, “com trilha obrigatória e atômica” não decorre dessa implementação. (`app/ponto/portas/celula.py:369`; `app/ponto/portas/celula.py:372`; `app/ponto/portas/celula.py:388`.)

- **Exportada e dinheiro:** a consolidação vigente mantém `!` para paga/vínculo ou escala retroativo; dinheiro não exportado tem preaprovação condicionada à reversão anterior, hash de exportada intacta e prova de frota no mesmo marco. Exportada que muda o gravado exige `!`, DIFF, reversão e porta própria. O novo contrato pode ser mais restritivo e excluir paga/exportada, mas não deve apresentar L-092 isolada como regra não alterada: L-113 revogou antigas recusas, enquanto a regra consolidada delimita os caminhos e aprovações atuais. (`CLAUDE.md:404`; `CLAUDE.md:406`; `CLAUDE.md:407`; `CLAUDE.md:411`; `CLAUDE.md:413`; `app/docs/LEIS.md:155`.)

- **Hash existente não cobre a condição (5) inteira:** `hash_do_gravado` consulta apenas `FechamentoMensal`, derivando campos concretos com uma lista de exclusões; entre os excluídos estão `previsto_em` e `derivado_em`. Não cobre `CelulaDia` nem `DiaPago`. Portanto, reaproveitá-lo sozinho não prova a condição conjunta do complemento; o futuro brief precisa nomear os campos, identidades e existência das linhas comparadas, além de isolar o efeito novo do efeito anterior do encerramento. (`app/ponto/portas/regen_exportada.py:37`; `app/ponto/portas/regen_exportada.py:56`; `app/ponto/portas/regen_exportada.py:62`; `app/ponto/portas/regen_exportada.py:71`.)

- **L-099:** proibir resolver cadastro/vínculo futuro por código e mandar os casos para administração está alinhado à separação ESTRUTURA/CADASTRO/BATIDA; alterar esse alcance exigiria outra autorização. (`app/docs/LEIS.md:122`; `CLAUDE.md:179`.)

- **Dependência de pagamento:** o cadastro “paga fora do sistema” e a migração conjunta dos leitores estão descritos na obra ainda registrada; o corte exige pouso atrás da espinha e medição na sombra. Não há base nesses registros para afirmar que já pousou. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)

## 16/11 15:33 · sessao bdfad5118ce7 · UI-EDITOR-CALENDARIO-TRES-GRADES · verificador · origem=Code

1. **PROCEDE — curas do item 3; NÃO PROCEDE — cura completa do item 5.**

O `finally` cobre o reenvio síncrono por `requestSubmit()`: o guarda encontra `confirmado=true`, e a autorização é zerada ao retornar, inclusive se o `required` impedir o evento ou ocorrer exceção. Também cobre o retorno de `form.submit()`; esse fallback dispensa validação e evento `submit`, mas só é chamado após resposta afirmativa. Não identifiquei novo caminho do editor que envie grade vazia em `dias_mes` sem pergunta. (`app/templates/escala/folga_calendario_editar.html:138`, `app/templates/escala/folga_calendario_editar.html:143`, `app/templates/escala/folga_calendario_editar.html:147`, `app/templates/escala/folga_calendario_editar.html:151`)

Editar o nome agora apenas redesenha, sem repor `dias_*` após `fcModo()` esvaziá-los. (`app/templates/escala/folga_calendario_editar.html:128`, `app/templates/escala/folga_calendario_editar.html:180`)

**Resta uma lacuna no selo das grades:** ele verifica somente a abertura das `section`, sem medir sua visibilidade após o JS. Contraexemplo por inspeção: acrescentar ao final da inicialização a ocultação de `fc-tam-30` e `fc-tam-28` preservaria as asserções estáticas; os cliques programáticos do smoke continuariam alcançando seus dias. O delta protege `hidden/display:none` diretamente na seção inicial, mas ainda não protege integralmente “três grades visíveis ao mesmo tempo”. (`app/escala/tests/test_editor_calendario_tres_grades.py:37`, `app/escala/tests/test_editor_calendario_tres_grades.py:89`, `app/escala/tests/test_editor_calendario_tres_grades.py:92`, `app/escala/tests/test_smoke_clique_folga_calendario.py:51`)

Execução desse mutante: **[não verificado]**.

2. **PROCEDE — leitores e escritores depois do delta.**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `confirmado` | Inicialização em falso; resposta afirmativa põe verdadeiro; `finally` limpa. (`app/templates/escala/folga_calendario_editar.html:136`, `:150`, `:151`) | Guarda de `submit`. (`app/templates/escala/folga_calendario_editar.html:138`) |
| `dias_<tam>` no formulário | `syncHidden()` serializa `sel`; inicialização, alternar dia, repetir e voltar a `dias_mes` o acionam. `fcModo()` limpa em `semana_dia`. (`app/templates/escala/folga_calendario_editar.html:109`, `:110`, `:119`, `:125`, `:180`, `:181`) | Navegador/sonda serializam; view lê, filtra e entrega à porta; porta persiste `dias`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:56`; `app/escala/views.py:342`, `:347`, `:356`; `app/escala/folgas.py:594`) |
| Canônico do editor | `renderGrade()` usa seleção ordenada e nome atual; input do nome chama esse redesenho. (`app/templates/escala/folga_calendario_editar.html:80`, `:94`, `:106`, `:128`) | Exibição em `fc-canonico-*`; não é campo enviado à view. (`app/templates/escala/folga_calendario_editar.html:57`, `:106`; `app/escala/views.py:321`, `:342`) |
| Canônico da lista | View monta a partir de nome, tamanho e dias persistidos. (`app/escala/views.py:293`, `:297`) | Template da lista imprime `t.canonico`. (`app/templates/escala/folga_calendarios_lista.html:22`) |

3. **PROCEDE — o novo teste morde a autorização velha, por inspeção.** [nao verificado]

A sonda apaga o nome antes do primeiro Sim e restaura antes do segundo Salvar. Sem a limpeza no `finally`, o primeiro reenvio barrado pelo `required` deixa a autorização verdadeira; o segundo clique passa sem perguntar, faltando `modal2` e aparecendo POST. As duas asserções pertinentes exigem segundo modal e `SEM-POST`. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:151`; `app/escala/tests/test_smoke_clique_folga_calendario.py:209`, `:210`, `:212`, `:213`)

**NÃO PROCEDE — reabrir os descartes como bloqueadores:**

- Trocar para `semana_dia` enquanto o modal está pendente não demonstra envio indevido em `dias_mes`. Contudo, a justificativa “é overlay” não garante bloqueio do teclado: o componente foca o Sim, mas não implementa contenção de foco. (`app/templates/escala/folga_calendario_editar.html:139`; `app/templates/core/_dialogo.html:160`, `:195`)
- A contagem sem `_reenvia()` continua redundante; `SEM-POST` é a asserção efetiva. (`app/escala/tests/test_smoke_clique_folga_calendario.py:175`, `:192`, `:199`, `:200`)
- O smoke contém leitura real do modal no Chromium, além da regex; resultado efetivamente executado não foi fornecido. (`app/escala/tests/test_smoke_clique_folga_calendario.py:69`, `:71`, `:124`, `:187`)

GREEN, RED e mutante executados: **[não verificado]**.

4. **NÃO PROCEDE — nova infração identificada de LEIS-UI ou dos selos estruturais.**

A ajuda apontada na volta 1 foi removida, curando aquele item 6; o trecho agora entra diretamente no laço das grades. O delta mantém `hxConfirmar` e não introduz novo markup nas alterações do guarda. (`app/docs/LEIS-UI.md:77`; `app/templates/escala/folga_calendario_editar.html:39`, `:127`, `:146`, `:151`)

Execução dos selos de template: **[não verificado]**.

**VEREDITO: DEVOLVE 5.**

## 16/11 15:31 · sessao 20b59d9b1e2c · - · - · origem=Ronald (tmux)

**PROCEDE — cura da autorização.** `confirmado=true` agora vale apenas durante o reenvio e o `finally` o limpa mesmo quando `requestSubmit()` retorna sem gerar `submit` por causa do `required`, ou quando a chamada lança exceção. O guarda síncrono do reenvio encontra `true`. (`app/templates/escala/folga_calendario_editar.html:138`, `:150`, `:151`)

**PROCEDE, com limite explícito — fallback.** O `finally` também limpa após `form.submit()`. Esse fallback não passa pelo evento `submit` nem pela validação HTML, mas só é alcançado depois de `ok=true`; portanto não cria POST sem pergunta. A garantia de bloqueio pelo `required` não se aplica a esse fallback. (`app/templates/escala/folga_calendario_editar.html:146`, `:147`, `:151`)

**NÃO PROCEDE — novo caminho demonstrado de POST vazio sem pergunta em `dias_mes`.** No fluxo do formulário, o guarda consulta o modo, identifica seleção vazia e cancela antes de abrir a pergunta; Cancelar retorna sem reenvio. (`app/templates/escala/folga_calendario_editar.html:139`, `:140`, `:143`, `:146`, `:147`)

**PROCEDE — novo teste tem mordida por inspeção.** A sonda apaga o nome antes do primeiro Sim e o restaura antes de clicar novamente. Sem o `finally` — inclusive restaurando o código anterior que limpava somente no evento `submit` — o primeiro `requestSubmit()` é barrado pelo `required`, a autorização sobra, o segundo clique passa sem modal e a sonda registra POST. Assim falham `modal2` e/ou `SEM-POST`. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:151`; `app/escala/tests/test_smoke_clique_folga_calendario.py:209`, `:210`, `:212`, `:213`, `:52`, `:57`) Execução efetiva e mutante executado: **[não verificado]**.

**NÃO PROCEDE — descarte do modal/troca de modo como bloqueador desta obra.** O overlay existe, mas o componente não implementa contenção de foco; só trata Escape e Enter no campo do próprio modal. A possibilidade de troca por teclado não está eliminada. Contudo, confirmar e reenviar em `semana_dia` segue a regra desse modo, enquanto voltar a `dias_mes` e submeter normalmente encontra o guarda. (`app/templates/core/_dialogo.html:112`, `:160`, `:163`, `:195`; `app/templates/escala/folga_calendario_editar.html:139`, `:176`) Reprodução humana: **[não verificado]**.

**NÃO PROCEDE — contagem redundante como defeito bloqueador.** A comparação antes/depois não demonstra ausência de gravação HTTP: o POST Django só ocorre em `_reenvia()`, ausente nesse teste. A asserção efetiva é `SEM-POST`, alimentada pela sonda que respeita `defaultPrevented`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:53`, `:165`, `:175`, `:192`, `:199`, `:200`)

**PROCEDE — smoke vai além da regex de `hxConfirmar`.** O código do smoke lê texto e botões do modal aberto no DOM do Chromium e exige sua presença; isso protege comportamento quando executado. Não há, no dado recebido, relatório de execução que permita afirmar GREEN/RED observado. (`app/escala/tests/test_smoke_clique_folga_calendario.py:69`, `:71`, `:124`, `:187`, `:188`) Execução observada: **[não verificado]**.

## 16/11 15:31 · sessao fa2574325564 · - · - · origem=Ronald (tmux)

**Pergunta 4 — NÃO PROCEDE: nova infração de LEIS-UI ou dos selos estruturais não identificada no delta.** A ajuda questionada desapareceu; o trecho agora passa diretamente do contêiner ao laço de grades. A L7 está em `app/docs/LEIS-UI.md:77`; trecho atual em `app/templates/escala/folga_calendario_editar.html:39`. As adições restantes são comentários e JS, sem diálogo nativo ou novo markup interpolado (`app/templates/escala/folga_calendario_editar.html:127`, `app/templates/escala/folga_calendario_editar.html:146`, `app/templates/escala/folga_calendario_editar.html:151`). Os selos examinam diálogo nativo, atributos inválidos, fonte Python, comentários vazados e interpolação crua (`app/core/tests/test_contract_l8_dialogo.py:42`; `app/core/tests/test_contract_template_bem_formado.py:162`, `:169`, `:174`, `:180`). Execução desses selos: **[não verificado]**.

**Cura das grades ocultas — PROCEDE para o mutante declarado.** `_secoes()` devolve somente a tag de abertura de cada seção, portanto a regex nova não acusa a classe `hidden` legítima dos spans internos; acusa `hidden` ou `display:none` na própria seção (`app/escala/tests/test_editor_calendario_tres_grades.py:37`, `:40`, `:92`; `app/templates/escala/folga_calendario_editar.html:47`).

A prova continua limitada: o selo não mede visibilidade calculada, ancestrais, CSS por seletor ou o próprio `fc-grade` escondido; verifica apenas ID da grade e atributos da seção. O smoke também usa `.click()` programático (`app/escala/tests/test_editor_calendario_tres_grades.py:89`, `:92`; `app/escala/tests/test_smoke_clique_folga_calendario.py:51`). Contraexemplo estático: adicionar `hidden` ao `div#fc-grade-*` preservaria essas asserções; execução do mutante: **[não verificado]**.

## 16/11 15:28 · sessao a6729505dd53 · O50-CELULA-APOS-DESLIGAMENTO · verificador · origem=Code

**O item 1 precisa voltar para fechar condições literais**, principalmente pagamento, células editadas e universo de batidas; aval condicional não autoriza completar essas condições por inferência. (`app/docs/LEIS.md:61`; `app/docs/CORTES.md:470`.)

Revisão estática da cópia. Identidade Git do HASH, comportamento em execução e impacto em produção: [nao verificado].

1. **As portas existem, mas não fazem hoje a remoção proposta.**

   `executar_desligamento` grava situação, data e motivo da demissão, bloqueia acesso, registra histórico e chama `fechar_vinculos_ativos`; também executa a cascata de chamados/disputas e cria o chamado de auditoria ao DP. O ramo agendado grava a intenção e retorna antes dessa cascata. (`app/colaboradores/services/desligamento.py:107`; `app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:147`; `app/colaboradores/services/desligamento.py:164`; `app/colaboradores/services/desligamento.py:195`.)

   O desligamento fecha apenas vínculos ativos cujo início não seja posterior ao fim informado; vínculos futuros recusados permanecem. Esse caminho usa `update()` e regenera somente células futuras, com `recalculo=False`. Portanto, não equivale à proposta de remover células e atualizar os valores do fechamento. (`app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:652`; `app/colaboradores/services/vinculo.py:662`; `app/colaboradores/services/vinculo.py:855`.)

   `encerrar_vinculo` valida a vigência, grava `ativa=False` e `data_fim` por `save()` e registra antes/depois. Esse `save()` dispara regeneração de células passadas e futuras, além da invalidação do previsto do fechamento. **Já há efeito indireto sobre célula e fechamento nessa porta.** (`app/colaboradores/services/vinculo.py:835`; `app/colaboradores/services/vinculo.py:842`; `app/escala/signals.py:72`; `app/escala/signals.py:80`; `app/escala/signals.py:145`.)

   A **porta da célula** é `ponto/portas/celula.py`, registrada para `CelulaDia`; gerar, regenerar, editar, lavrar e carimbar pertencem a esse módulo. `gerar_celulas.py` reexporta as funções, sem constituir outro escritor. Não há operação declarada de remoção de `CelulaDia`; fora da vigência, o código conserva a linha e altera trabalho/DNA. (`app/core/portas.py:107`; `app/ponto/portas/celula.py:74`; `app/ponto/management/commands/gerar_celulas.py:10`; `app/ponto/portas/celula.py:239`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`.)

   “Ninguém remove” seria absoluto demais: excluir o colaborador tem cascata sobre células; excluir o vínculo apenas anula sua geradora. (`app/escala/models.py:1681`; `app/escala/models.py:1692`.)

   A **porta do fechamento** é `ponto/services/fechamento.py`. `recalcular_por_evento` agenda, após commit, `recalcular_fechamento_mes` para o colaborador e a competência do fato; esta grava `FechamentoMensal` e também lavra `DiaPago`. Apenas invalidar carimbos não equivale a atualizar seus valores. (`app/ponto/services/fechamento.py:1315`; `app/ponto/services/fechamento.py:1798`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/fechamento.py:1144`; `app/ponto/services/fechamento.py:1191`; `app/ponto/services/fechamento.py:1652`.)

2. **A direção da obra é compatível, mas precisa conservar restrições vigentes.**

   A célula é dado congelado; a exceção formal para passado errado por cadastro exige denúncia da reescrita. LU-13/LU-14 autorizam a direção de remover com trilha, preservando **exportada, editada e com batida**, com medição prévia e `!` próprio para retroativo. O item 1 fornecido não repete a exceção “editada”; revogação dessa proteção: [nao verificado]. (`CLAUDE.md:149`; `app/docs/CORTES.md:470`; `app/docs/CORTES.md:471`.)

   DESLIGADO-NAO-TRAVA já faz o desligado retornar sem motivos de retenção e entrega os dias como informativo ao DP. Isso não depende de apagar células; a O50 não deve ser apresentada como condição para aquela liberação. (`app/folha/export.py:1197`; `app/folha/export.py:1207`; `app/folha/export.py:1216`.)

   A cura nas portas é coerente com L-099; usar código para escolher cadastro correto, apagar batida ou resolver vínculo futuro extrapolaria a separação entre ESTRUTURA e CADASTRO/BATIDA. (`app/docs/LEIS.md:122`; `app/colaboradores/services/vinculo.py:855`.)

   L-113 alterou as recusas históricas de L-092, mas o texto consolidado mantém alteração genérica de exportada fora da preaprovação, com porta e `!` específicos. Portanto, a rotina genérica de recálculo não pode servir como garantia automática de “exportada intacta”. (`app/docs/LEIS.md:155`; `CLAUDE.md:411`; `app/ponto/portas/celula.py:436`.)

3. **Há condições que ainda não fecham no literal.**

   **“Não paga”:** `competencia_entregue` reconhece TXT vigente; `competencia_fechada_para_celula` acrescenta holerite publicado da pessoa; `marcos_da_competencia['paga']` também consulta holerite publicado. Isso não comprova ausência de pagamento externo. A obra do juiz `competencia_ja_paga`, incluindo cadastro de pagamento externo, está registrada com pouso condicionado à espinha. (`app/folha/export.py:588`; `app/folha/export.py:618`; `app/folha/export.py:622`; `app/ponto/services/fechamento.py:1539`; `app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)

   **“Tenha batida”:** o universo apurável exclui retratadas, enquanto a regra distingue apuração de exibição crua. A associação ao dia também distingue batida reclamada pelo turno de órfã no dia civil. A proibição de remover precisa escolher explicitamente o universo e proteger a passagem da meia-noite. (`CLAUDE.md:158`; `CLAUDE.md:167`; `app/ponto/turnos.py:2888`; `app/ponto/turnos.py:2898`.)

   **“Afastado”:** há juiz registrado, `afastado_hoje`, com data e argumento obrigatório `para`; cobrança considera ausência não rejeitada, folha considera aprovada. Consultar apenas `situacao='afastado'` não representa esse contrato. Falta definir data e efeito do veto. (`app/core/juizes.py:272`; `app/ponto/turnos.py:2737`; `app/colaboradores/services/situacao_exibida.py:18`; `app/colaboradores/models.py:444`.)

   **“Posterior ao fim”:** a célula pertence à pessoa/dia, com geradora opcional; pode existir sucessor ou readmissão. A própria regeneração atual preserva dia coberto por outro vínculo. Remover indiscriminadamente toda célula após o fim de um vínculo perderia essa proteção. (`app/escala/models.py:1681`; `app/escala/models.py:1685`; `app/escala/models.py:1692`; `app/ponto/portas/celula.py:254`; `app/docs/BACKLOG.md:1411`.)

4. **Dinheiro está no escopo; seus efeitos ainda precisam ser delimitados.**

   Atualizar `FechamentoMensal` toca valores financeiros e pode alcançar a competência inteira e `DiaPago`. Valor efetivamente alterado ou garantia de nenhuma falta nova: [nao verificado]. A preaprovação exige reversão anterior, exportada intacta comprovada por hash e prova de frota no mesmo marco; paga e vínculo/escala retroativo continuam sujeitos a `!`. (`app/ponto/services/fechamento.py:902`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1191`; `CLAUDE.md:404`; `CLAUDE.md:406`.)

   A remoção também anula referências de chamados e perguntas; ausência da célula faz leitores recorrerem ao cálculo/template. Logo, “a linha sumiu” não prova sozinho que previsto, cobrança e dinheiro ficaram coerentes. (`app/chamados/models.py:50`; `app/chamados/models.py:2519`; `app/escala/models.py:1094`; `app/escala/models.py:1098`.)

**VEREDITO: AVAL-DEVOLVIDO**

- A execução aguardará `competencia_ja_paga` ou qual autoridade provará ausência de pagamento externo? (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)
- Permanecem excluídas células editadas, dias de sucessor/readmissão e todo passivo reservado ao `!`? (`app/docs/CORTES.md:470`; `app/docs/BACKLOG.md:1411`.)
- “Tenha batida” inclui retratada e órfã, com associação ao dia por qual autoridade? (`CLAUDE.md:158`; `CLAUDE.md:167`; `app/ponto/turnos.py:2898`.)
- “Afastado” será consultado em qual data e com qual `para`? (`app/ponto/turnos.py:2737`; `app/colaboradores/services/situacao_exibida.py:18`.)
- “Exportada → nada muda” vale para todo o ato ou apenas para o efeito novo da O50, considerando que o `save()` atual invalida previsto sem filtro de competência? (`app/escala/signals.py:145`; `app/ponto/services/fechamento.py:1607`.)

## 16/11 15:27 · sessao 982166043b41 · - · - · origem=Ronald (tmux)

- A porta canônica é `app/ponto/portas/celula.py`, registrada para `CelulaDia` em `app/core/portas.py:112`; declara gerar, lavrar, carimbar e editar, sem operação de remover célula (`app/ponto/portas/celula.py:74`).
- O censo estático real `core.censo_escritas.varrer_arvore` encontrou 10 escritas atribuídas a `CelulaDia`, todas nessa porta, sem `delete`; o instrumento declara limite para operações não atribuíveis (`app/core/censo_escritas.py:43`). Os sítios são `app/ponto/portas/celula.py:265`, `:279`, `:293`, `:310`, `:638`, `:647`, `:836`, `:914`, `:926`, `:1237`.
- Fora da vigência, hoje a célula permanece: futuro zera previsto, limpa geradora, guarda DNA anterior e desfaz julgamento (`app/ponto/portas/celula.py:251`); passado grava `trabalha=False` e DNA sem marcos (`app/ponto/portas/celula.py:272`). O comentário explicita “A celula NAO se apaga” porque há perguntas apontando para ela (`app/ponto/portas/celula.py:239`).
- O `delete` da porta remove `FuroDiario`, não `CelulaDia` (`app/ponto/portas/celula.py:1253`, `:1266`).
- A guarda vigente da célula é `competencia_fechada_para_celula`: TXT vigente entregue OU holerite com lote publicado da pessoa; não representa pagamento externo declarado (`app/folha/export.py:597`, `:618`, `:622`). O atual `marcos_da_competencia()['paga']` também só consulta holerite com lote publicado (`app/ponto/services/fechamento.py:1539`). O corte COMPETENCIA-PAGA consta “construindo”, condicionado à espinha (`app/docs/CORTES.md:495`).
- “Tenha batida” não fecha por inferência: o universo apurável exclui retratadas (`app/ponto/turnos.py:2868`, `:2888`); associação ao dia distingue batida reclamada por turno e órfã no dia civil (`app/ponto/turnos.py:2898`, `:2957`). É preciso dizer se a proibição inclui retratadas e como protege órfãs e turnos que cruzam meia-noite; o literal do anexo não decide isso [nao verificado].
- Excluir a célula anula referências de chamados e perguntas (`app/chamados/models.py:50`, `:2519`). Excluir o colaborador causa cascata para células (`app/escala/models.py:1681`); excluir vínculo apenas anula `escala_geradora` (`app/escala/models.py:1692`). A restauração de snapshot apaga ECs e protege células editadas contra essa anulação (`app/colaboradores/services/vinculo.py:1022`).
- A seleção deve preservar dia de sucessor: o gerador trabalha por vínculo vigente e, se a célula não existe, cria uma (`app/ponto/portas/celula.py:906`, `:914`); a regeneração também cria célula ausente no dia coberto (`app/ponto/portas/celula.py:289`). A célula pertence à pessoa e ao dia, enquanto a geradora é proveniência opcional (`app/escala/models.py:1681`, `:1685`, `:1692`).
- Remoção precisa conferir leitores: sem célula, `eh_dia_trabalho` recai no cálculo do vínculo (`app/escala/models.py:1094`); `marcos_do_dia` declara retorno ao template vivo quando falta célula/DNA (`app/escala/models.py:1098`). Ausência da linha não prova ausência de previsto.
- O dinheiro deve ser acionado pelo ato, não por sinal de `CelulaDia` (`app/ponto/portas/celula.py:432`). A própria porta registra que o caminho ordinário hoje pode recalcular exportada, após L-113 (`app/ponto/portas/celula.py:436`); O50 necessita guarda explícita do seu alcance antes de chamar esse caminho.

## 16/11 15:27 · sessao c81e9bf48af4 · - · - · origem=Ronald (tmux)

Achados para consolidar, só item 1:

- **Restrição omitida no aval novo:** LU-13 manda O50 “só DIREÇÃO”, define futuras como após D e exclui **exportada/editada/com batida**, exige medir frota antes e `!` próprio para retroativo. A condição **editada** precisa continuar explícita; o item 1 fornecido não a nomeia. (`app/docs/CORTES.md:470`; cláusula do anexo novo sem arquivo local: [nao verificado].)
- **Irretroatividade não se resolve só por “posterior ao fim”:** célula é dado congelado e passado só se reescreve pela exceção formal de cadastro com denúncia; “posterior ao fim” pode alcançar datas já passadas. Precisa separar evento futuro, célula futura já existente e passivo reservado ao `!`, sem autorizar por inferência a remoção de passado. (`CLAUDE.md:149-156`; `app/docs/BACKLOG.md:1410-1411`; `app/docs/CORTES.md:470`.)
- **Paga é dependência real, sem juiz pousado nesta cópia:** a obra `COMPETENCIA-PAGA-UM-JUIZ` declara o futuro `competencia_ja_paga`, cuja definição é entregue/exportação vigente OU cadastro “paga fora do sistema”, e proíbe meia migração; ainda está registrada/construindo, com pouso depois da espinha. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.) A busca em Python de produção não achou `competencia_ja_paga`, `CompetenciaPaga`, `paga_fora` ou `competencia_paga`: ausência absoluta além desses nomes [nao verificado].
- **Exportação tem juiz, pagamento não pode ser inferido dele:** `competencia_entregue` pergunta por TXT vigente não invalidado; `competencia_fechada_para_celula` compõe essa resposta com holerite publicado do próprio colaborador. Nenhum desses contratos diz representar pagamento externo. (`app/folha/export.py:556-594`, especialmente `:564-578`; `:597-628`; `app/core/juizes.py:343-353`.)
- **Afastado exige pergunta mais precisa:** `situacao='afastado'` permanece nas choices, mas o modelo declara que afastamento deixou de ser valor de cadastro e é lido do juiz. (`app/colaboradores/models.py:295-299`; `:444-449`.) O juiz registrado é `afastado_hoje`, que aceita data e exige `para`; tipos atualmente estreitados a `afastamento_inss`. (`app/core/juizes.py:272`; `app/ponto/turnos.py:2716-2744`.) `para='folha'` significa aprovada, `para='cobranca'` não rejeitada. (`app/ponto/turnos.py:2523-2532`.) Logo falta dizer se “tocar afastado” veta pela data do ato ou da célula e qual efeito/status; a apresentação dá precedência ao desligado, portanto não basta consultá-la depois de demitir. (`app/colaboradores/services/situacao_exibida.py:18-25`; `:56-63`.)
- **DESLIGADO-NAO-TRAVA é retentor, não licença de remoção:** hoje retorna vazio para desligado em qualquer dia da competência; informativo da rescisão só lê e declara proibido mexer em célula/batida e lançar falta automática naquela lei. O item 1 novo é autorização específica futura, não deve ser descrito como reparo de trava ainda vigente. (`app/folha/export.py:1197-1213`; `:1216-1228`; `app/docs/CORTES.md:440`.) O juiz do desligado lê só estado gravado e exclui agendado/afastado. (`app/colaboradores/services/desligamento.py:23-32`; `app/core/juizes.py:273-276`.)
- **Dinheiro está expressamente no escopo:** atualizar `FechamentoMensal` não é operação neutra. Regra consolidada exige `!` para paga e vínculo/escala retroativo; competência não exportada tem preaprovação condicionada a reversão antes, exportada intacta com hash e prova de frota no mesmo marco. Exportada só porta REGEN-EM-EXPORTADA com `!`, DIFF e trilha. (`CLAUDE.md:404-417`.) L-113 alterou as recusas históricas da L-092, mas não autoriza ignorar o item 1 que restringe expressamente exportada/paga; o texto consolidado atual é o acima. (`app/docs/LEIS.md:155`; `CLAUDE.md:411-422`.)
- **L-099 favorece cura na origem e lista para dado:** separar ESTRUTURA de CADASTRO/BATIDA está literalmente na lei; células com batida não viram caso especial de código. (`app/docs/LEIS.md:122`.)
- **Universo da guarda “tenha batida” continua ambíguo:** apuração canônica exclui retratadas, exibição usa cru. `batidas_apuraveis` confirma que pendente de DP conta, retratada não. Qual desses universos governa a proibição de apagar célula precisa ser fechado; não é lícito assumir que a lei de apuração responde pergunta de preservação de dado. (`CLAUDE.md:158-167`; `app/ponto/turnos.py:2868-2895`.)
- **Registro anterior de escopo maior:** O50-DESLIGAR-ENCERRA-TUDO absorve O50-CELULA e inclui encerrar vínculos vigentes/futuros; não transportar automaticamente esse escopo maior para esta revisão do item 1. (`app/docs/BACKLOG.md:1409`.) A preservação de último dia, turno noturno e readmissão já está declarada no complemento O50 e deve orientar os cenários do PREPARA. (`app/docs/BACKLOG.md:1411`.)

Perguntas mais fortes para AVAL-DEVOLVIDO: dependência do juiz paga; manutenção da exclusão editada e do limite direção/futuro; universo de batida e dia do turno; data/efeito do veto afastado. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:470`; `CLAUDE.md:149-167`; `app/ponto/turnos.py:2523-2532`, `:2716-2744`, `:2868-2895`.)

## 16/11 15:16 · sessao ed7650d71c30 · UI-EDITOR-CALENDARIO-TRES-GRADES · verificador · origem=Code

Análise por leitura da cópia e do diff. Execução da suíte, navegador e mutantes: **[não verificado]**. Os números de produção citados nos comentários não têm demonstração no dado fornecido. (`app/escala/tests/test_editor_calendario_tres_grades.py:6`)

1. **PROCEDE — leitores e escritores identificados.**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `dias_<tam>` | `syncHidden()` serializa `sel` em inputs. (`app/templates/escala/folga_calendario_editar.html:113`) | A view lê, filtra `1..tam` e entrega à porta; a porta persiste. (`app/escala/views.py:342`, `app/escala/views.py:347`, `app/escala/views.py:356`, `app/escala/folgas.py:594`) |
| `data-fc-vazia` | HTML inicial usa `t.marcados`; JS atualiza pela seleção atual. (`app/templates/escala/folga_calendario_editar.html:44`, `app/templates/escala/folga_calendario_editar.html:100`) | `_vazia()` nos testes. O guarda consulta diretamente `sel.size`. (`app/escala/tests/test_editor_calendario_tres_grades.py:44`, `app/templates/escala/folga_calendario_editar.html:140`) |
| `fc-marca-vazia` | Template cria o texto e a classe; `renderGrade()` atualiza a visibilidade. (`app/templates/escala/folga_calendario_editar.html:50`, `app/templates/escala/folga_calendario_editar.html:99`) | Usuário recebe o texto; teste procura o span fora dos scripts. (`app/templates/escala/folga_calendario_editar.html:51`, `app/escala/tests/test_editor_calendario_tres_grades.py:51`) |
| `sel[tam]` | Inicialização pelo seed, `fcTog()` e `fcRepetir()`. (`app/templates/escala/folga_calendario_editar.html:82`, `app/templates/escala/folga_calendario_editar.html:122`, `app/templates/escala/folga_calendario_editar.html:127`) | Grade, contagem, marca, chips, inputs, repetir e confirmação. (`app/templates/escala/folga_calendario_editar.html:89`, `:97`, `:116`, `:127`, `:140`) |
| `modo` | Radios; contexto vindo do calendário; persistência pela porta. (`app/templates/escala/folga_calendario_editar.html:20`, `app/escala/views.py:370`, `app/escala/folgas.py:585`, `app/escala/folgas.py:590`) | View, porta, guarda e alternância visual. (`app/escala/views.py:325`, `app/escala/folgas.py:584`, `app/templates/escala/folga_calendario_editar.html:139`, `:175`) |
| `t.marcados` | View projeta `cal.tamanhos.all()`/`t.dias`; ausência vira conjunto vazio. (`app/escala/views.py:362`, `app/escala/views.py:366`) | Estado inicial, contagem, marca e seed do JS. (`app/templates/escala/folga_calendario_editar.html:44`, `:49`, `:50`, `:77`) |

A origem persistida também pode nascer pelo minicadastro já existente; ele permanece em `folgas.py`. Isso não foi criado pelo diff. (`app/escala/folgas.py:645`, `app/escala/folgas.py:659`)

2. **NÃO PROCEDE — segundo escritor, juiz novo ou diálogo nativo.**

A view continua chamando `salvar_calendario_de_folga`; a gravação permanece nessa porta. As duas entidades têm `escala/folgas.py` como arquivo autorizado de escrita. (`app/escala/views.py:354`, `app/escala/folgas.py:594`, `app/core/portas.py:472`)

A seleção editável produz tanto a apresentação quanto os campos enviados e a pergunta sobre vazio; não surgiu outra derivação das datas efetivas de folga. Essa derivação continua em `datas_que_o_calendario_manda()`. (`app/templates/escala/folga_calendario_editar.html:97`, `:116`, `:140`; `app/escala/folgas.py:43`)

O diálogo utilizado é `hxConfirmar`, sem `confirm/alert/prompt/hx-confirm`. O próprio selo distingue essas chamadas. (`app/templates/escala/folga_calendario_editar.html:146`, `app/core/tests/test_contract_l8_dialogo.py:41`)

3. **PROCEDE COM RESSALVA — existe confirmação que pode ficar latente.**

No fluxo normal, clique, Enter e `requestSubmit()` encontram o guarda do formulário. Em `dias_mes`, grade vazia executa `preventDefault()` antes de chamar o modal; Cancelar não reenvia. (`app/templates/escala/folga_calendario_editar.html:137`, `:139`, `:143`, `:146`, `:147`)

Modal indisponível não libera esse envio: o cancelamento já aconteceu; quando o componente consegue responder à falta de elementos, devolve falso sem `naoBloquear`. (`app/templates/escala/folga_calendario_editar.html:143`, `:146`; `app/templates/core/_dialogo.html:184`)

**Contraexemplo por inspeção:** abrir a pergunta com nome válido, apagar o nome enquanto ela está pendente e confirmar. O `requestSubmit()` pode ser impedido pelo `required`, deixando `confirmado=true`; preencher novamente e salvar consome essa autorização antiga sem perguntar novamente. O booleano só é limpo no próximo evento `submit`. Reprodução em navegador: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:148`, `:149`; `app/templates/core/_dialogo.html:160`, `:195`)

Em `semana_dia`, uma nova submissão não abre o modal. Entretanto, uma pergunta aberta anteriormente em `dias_mes` não é fechada por `fcModo()` e pode permanecer após uma troca de modo pelo teclado. Reprodução: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:139`, `:174`; `app/templates/core/_dialogo.html:193`)

Há outra inconsistência na sequência **trocar para semana → editar nome**: `fcModo()` limpa `dias_*`, mas o input do nome chama `renderTudo()` e os repõe. A view ignora esses campos em `semana_dia`; isso não demonstra gravação indevida. (`app/templates/escala/folga_calendario_editar.html:112`, `:130`, `:178`; `app/escala/views.py:327`)

4. **NÃO PROCEDE — repetir fora do tamanho pelo fluxo normal.**

Os dias clicáveis nascem em `1..t.n`; repetir conserva somente `d <= t.n`, exatamente o corte do criador. (`app/templates/escala/folga_calendario_editar.html:90`, `:127`; `app/escala/folgas.py:661`)

O corte não valida o limite inferior, e a leitura do legado não saneia os dias. Portanto, garantia absoluta para calendários previamente malformados não está demonstrada; existência desses registros: **[não verificado]**. (`app/escala/views.py:362`, `app/templates/escala/folga_calendario_editar.html:127`)

5. **PROCEDE — os quatro mutantes indicados têm mordida identificável, mas há lacunas.**

As falhas abaixo são previstas pela leitura; não são resultados de execução. **[não verificado]**

| Mutante | Asserção que deve falhar |
|---|---|
| Tirar a marca | Contagem de marcas vazias; se todas ficarem visíveis, contraponto do calendário completo. (`app/escala/tests/test_editor_calendario_tres_grades.py:99`, `:109`) |
| Tirar o guarda de submit | Exigência de modal e de `SEM-POST` ao cancelar. (`app/escala/tests/test_smoke_clique_folga_calendario.py:174`, `:183`, `:186`) |
| Repetir sem corte | Comparação dos campos capturados **antes** da filtragem da view. (`app/escala/tests/test_smoke_clique_folga_calendario.py:195`, `:196`) |
| Sonda em captura | Cancelar encontrará POST capturado em vez de `SEM-POST`. A sonda atual respeita `defaultPrevented` em bolha. (`app/escala/tests/test_smoke_clique_folga_calendario.py:51`, `:57`, `:186`) |

**Lacuna relevante:** esconder as seções 30/28 pode manter os testes verdes. O teste das três grades verifica seções/IDs; a marca “visível” considera apenas `hidden` no próprio span; o smoke usa `.click()` programático sem medir visibilidade. Assim, a promessa “três grades ao mesmo tempo” não está protegida contra ocultação. (`app/escala/tests/test_editor_calendario_tres_grades.py:82`, `:48`; `app/escala/tests/test_smoke_clique_folga_calendario.py:50`)

Há vacuidade **local** na contagem de registros antes/depois de Cancelar: esse teste nunca chama `_reenvia()`, que realiza o POST Django. A asserção `SEM-POST` continua útil. (`app/escala/tests/test_smoke_clique_folga_calendario.py:162`, `:179`, `:186`, `:187`)

A regex de `hxConfirmar` não prova execução; o `wraps` prova uma chamada real no cadastro, mas não exclui um segundo update redundante nem cobre esse spy na edição. (`app/escala/tests/test_editor_calendario_tres_grades.py:118`, `app/escala/tests/test_folga_cadastro.py:19`, `:43`, `:45`)

6. **PROCEDE — infração à L7.**

A ajuda adicional “Cada tamanho de mês...” está hardcoded em um `<div>`, fora de tooltip/componente e de mapa compartilhado. Isso confronta diretamente a L7. A marca visível exigida pelo aval é questão distinta. (`app/templates/escala/folga_calendario_editar.html:40`, `app/docs/LEIS-UI.md:77`)

**NÃO PROCEDE — infração identificada aos selos estruturais examinados.** Não há diálogo nativo ou `title` cru novo; não identifiquei atributo inválido, fonte Python ou comentário vazando. Resultado executado dos selos: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:50`, `:146`; `app/core/tests/test_contract_l8_dialogo.py:42`, `app/core/tests/test_contract_template_bem_formado.py:162`, `:169`, `:174`)

7. **NÃO PROCEDE — relógio novo fora da borda ou dois significados para vazio.**

`t.marcados` é a seleção carregada; `sel[tam]`, sua versão editável. Marca e atributo significam “seleção vazia” e acompanham o mesmo conjunto. Não significam “a pessoa não terá nenhuma outra origem de folga”. (`app/escala/views.py:362`, `app/templates/escala/folga_calendario_editar.html:51`, `:82`, `:97`, `:100`, `:145`)

O relógio civil permanece na porta e é repassado aos consumidores. A classe “selo sem mordida” procede parcialmente pelas lacunas descritas no item 5. (`app/escala/folgas.py:599`, `app/escala/folgas.py:601`; `app/escala/tests/test_editor_calendario_tres_grades.py:82`)

**VEREDITO: DEVOLVE 3, 5, 6.**

## 16/11 15:15 · sessao 5db8ccbd2ba6 · - · - · origem=Ronald (tmux)

Auditoria por leitura; testes não executados. [não verificado] [nao verificado]

**5 — PROCEDE: os quatro mutantes indicados têm mordida identificável.**

| Mutante | Teste que fica vermelho | [nao verificado]
|---|---|
| Retirar a marca | `app/escala/tests/test_editor_calendario_tres_grades.py:99`; remover apenas condição e deixar todas visíveis falha em `:109`. |
| Retirar guarda do submit | Smoke exige modal em `app/escala/tests/test_smoke_clique_folga_calendario.py:174` e `:183`; Cancelar exige `SEM-POST` em `:186`. A regex isolada `test_editor_calendario_tres_grades.py:121` sobreviveria se a chamada continuasse em código morto. |
| Repetir sem corte | FormData anterior à view precisa conter dias exatos em `app/escala/tests/test_smoke_clique_folga_calendario.py:195` e `:196`; a persistência isolada esconderia esse defeito, conforme `:193`. |
| Sonda em captura | Registraria o evento antes do guarda; Cancelar encontraria JSON em vez de `SEM-POST`, falhando em `app/escala/tests/test_smoke_clique_folga_calendario.py:186`. Ordem atual: listener de documento em bolha e respeito a `defaultPrevented` em `:51–57`; guarda no form em `app/templates/escala/folga_calendario_editar.html:137`. |

**PROCEDE: existem limites e afirmações excessivas nos selos.**

- A contagem antes/depois em Cancelar é vacuamente verdadeira quanto à persistência: esse teste não chama `_reenvia`, único ponto que POSTa ao Django; `app/escala/tests/test_smoke_clique_folga_calendario.py:162`, `:179`, `:187`. O `SEM-POST` continua sendo uma asserção útil, `:186`.
- “Três grades ao mesmo tempo” não está protegido contra ocultação: teste verifica seções/IDs, `app/escala/tests/test_editor_calendario_tres_grades.py:82`; `_marcas_visiveis` considera apenas `hidden` do próprio span, `:48`. Esconder seções 30/28 com CSS pode sobreviver, pois smoke usa `.click()` programático sem medir visibilidade, `app/escala/tests/test_smoke_clique_folga_calendario.py:50`.
- `wraps` e `call_count` provam chamada real e resultado do cadastro, não ausência universal de outros escritores: um segundo update com valores iguais sobreviveria; `app/escala/tests/test_folga_cadastro.py:43`, `:45`, `:49`. Edição não passa por esse spy, `:19`.

**6 — PROCEDE: L7 merece devolução.**

A ajuda adicional é prosa fixa em `<div>`, fora de tooltip/componente/mapa: `app/templates/escala/folga_calendario_editar.html:40`; L7 proíbe ajuda hardcoded por tela em `app/docs/LEIS-UI.md:77`. O texto de confirmação usa o componente autorizado, mas continua fixo no template sem mapa compartilhado: `app/templates/escala/folga_calendario_editar.html:144`. Não conte o texto visível da marca como infração por ser visível: foi exigido expressamente pelo usuário; sua implementação está em `:50`.

**NÃO PROCEDE: diálogo nativo/title cru novos.** A chamada é `hxConfirmar`, `app/templates/escala/folga_calendario_editar.html:146`; o varredor distingue essa chamada de `confirm/alert/prompt/hx-confirm`, `app/core/tests/test_contract_l8_dialogo.py:41`. A marca tem texto e não usa `title`, `app/templates/escala/folga_calendario_editar.html:50`.

**NÃO PROCEDE: violação estrutural identificada nos varredores examinados.** Novos elementos não trazem atributos inválidos, fonte Python ou comentário vazando; critérios em `app/core/tests/test_contract_template_bem_formado.py:162`, `:169`, `:174`. A regex real de interpolação crua não encontrou correspondência neste template, critério `:123` e teste `:180`; verificação feita em memória, sem arquivo. A confirmação resolve `hxConfirmar` no handler, `app/templates/escala/folga_calendario_editar.html:146`, sem captura no load proibida por `app/core/tests/test_hx_nao_se_captura_no_load.py:21`.

**7 — NÃO PROCEDE: relógio fora da borda introduzido.** `setTimeout(200)` serve à sonda, `app/escala/tests/test_smoke_clique_folga_calendario.py:67`; não lê data/hora civil, alvo do selo em `app/core/tests/test_selo_teste_sem_relogio.py:32`.

## 16/11 15:15 · sessao 3c88f6c0f911 · - · - · origem=Ronald (tmux)

**Q3 — NÃO PROCEDE no fluxo normal; há duas ressalvas de estado.**

- Clique, Enter e `requestSubmit()` encontram o guarda instalado no formulário; grade vazia em `dias_mes` cancela o envio antes de chamar `hxConfirmar`. (`app/templates/escala/folga_calendario_editar.html:137`, `:139`, `:140`, `:143`, `:146`)
- Cancelar não reenvia; o único `form.submit()` local fica depois da resposta positiva. (`app/templates/escala/folga_calendario_editar.html:147`, `:149`)
- Modal ausente não libera envio: `preventDefault()` já aconteceu; o componente retorna falso quando faltam seus elementos e `naoBloquear` não foi passado. Se a função inteira estiver indisponível, a exceção também acontece depois do cancelamento. (`app/templates/escala/folga_calendario_editar.html:143`, `:146`; `app/templates/core/_dialogo.html:184`)
- Submeter em `semana_dia` não abre confirmação. Voltar para `dias_mes` reconstitui os campos da seleção. (`app/templates/escala/folga_calendario_editar.html:139`, `:174`, `:178`, `:179`)

**Ressalva 1 — PROCEDE: confirmação pode ficar latente.** `confirmado=true` só é consumido pelo próximo evento `submit`. Se o `requestSubmit()` não produzir esse evento por validação inválida, a autorização fica para uma tentativa posterior. Contraexemplo por inspeção: abrir o modal com nome válido, alcançar o nome pelo teclado e apagá-lo enquanto a promessa está pendente, confirmar, preencher novamente e salvar; não haverá nova pergunta. O componente não prende o foco nem torna o formulário inerte. Esse caminho já passou por uma confirmação anterior; não equivale a “nunca chamou `hxConfirmar`”. Execução em navegador: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:148`, `:149`; `app/templates/core/_dialogo.html:119`, `:160`, `:195`)

**Ressalva 2 — PROCEDE: trocar modo e depois editar nome repõe `dias_*`.** `fcModo()` limpa os campos em `semana_dia`, mas editar o nome chama `renderTudo()` → `syncHidden()` sem conferir o modo. A view ignora esses dias ao processar `semana_dia`, portanto isso não prova gravação indevida. (`app/templates/escala/folga_calendario_editar.html:112`, `:117`, `:130`, `:178`; `app/escala/views.py:327`, `:339`)

**Modal visível depois da troca para `semana_dia`:** pode permanecer um modal aberto anteriormente em `dias_mes`, pois `fcModo()` não o fecha; nova submissão em `semana_dia` não o abre. Reprodução por teclado: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:139`, `:174`; `app/templates/core/_dialogo.html:193`)

_6094 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
