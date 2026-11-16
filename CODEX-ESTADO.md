# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 17:03** · publicado em **16/11/2026 17:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 17:03 · sessao abdee5dbe35b · BUG-PAR-O-PAR · verificador · origem=Code

1. **Q1 — Confere e fecha a delimitação.** Sem `data_evento`, o helper retorna antes de consultar K6; U163 é pulado antes dessa verificação. Portanto, L5 descreve a metade fora163 sem dia, não ausência de consulta a qualquer juiz. Isso corrige a premissa da L5 original sem exigir mudança na ordem da porta. (`app/chamados/services/materializacao.py:780`; `app/chamados/services/materializacao.py:785`; `app/chamados/services/materializacao.py:797`; `app/chamados/services/materializacao.py:801`.)

2. **Q2 — Confere como precedência relativa das linhas da testemunha; a porta tem outra ordem de decisão.** Na testemunha, a guarda K6 antecede F6; F6 antecede declarado × gravado; `aplicavel` vem depois. Existem guardas intermediárias de data, derivação, satisfação e paridade, preservadas pelo “sem mudança”. (`app/chamados/services/materializacao.py:861`; `app/chamados/services/materializacao.py:866`; `app/chamados/services/materializacao.py:871`; `app/chamados/services/materializacao.py:874`; `app/chamados/services/materializacao.py:884`; `app/chamados/services/materializacao.py:897`; `app/chamados/services/materializacao.py:908`; `app/chamados/services/materializacao.py:911`.)

   Na **porta**, a sequência é: consulta antecipada da espera → implausibilidade → guarda explícita K6 → carimbo de validação → materialização, onde F6 pode recusar. A consulta antecipada percorre a testemunha, mas extrai **somente** seu estado de espera; não devolve automaticamente os bloqueios K6/F6 encontrados nela. (`app/chamados/services/validacao.py:573`; `app/chamados/services/validacao.py:577`; `app/chamados/services/validacao.py:614`; `app/chamados/services/validacao.py:617`; `app/chamados/services/validacao.py:645`; `app/chamados/services/materializacao.py:753`; `app/chamados/services/materializacao.py:754`; `app/ponto/registro_batida.py:156`.)

   Assim, uma U163 divergente bloqueada por F6 recebe **L2 na testemunha**, e a consulta antecipada não a devolve como L3. A diferença das ordens não cria colisão residual: o complemento fixa a unidade na testemunha, preserva expressamente a porta e distingue seus retornos. (`app/chamados/services/materializacao.py:886`; `app/chamados/services/materializacao.py:910`; `app/chamados/services/materializacao.py:754`; `complemento3_espinha_bug_par_c4_1611.txt:1`; `complemento3_espinha_bug_par_c4_1611.txt:3`; `complemento3_espinha_bug_par_c4_1611.txt:11`, texto fornecido.)

3. **Q3 — Confere; L5 fora163 já não entrava no contador.** A seleção é exclusivamente U163. Retirar o balde projetado não remove um leitor efetivo de L5 nem altera o universo contado. O balde existente `bug_par_103_sem_dia` trata candidatas U103 carimbadas, outra situação; a lista do comando já inclui fora163 sem dia como linha falante. Isso fecha a correção da L5 original sem deixar balde a decidir pelo construtor. (`app/chamados/services/materializacao.py:321`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:331`; `app/chamados/management/commands/bug_par_103.py:71`; `app/chamados/management/commands/bug_par_103.py:75`; `app/chamados/management/commands/bug_par_103.py:77`.)

4. **Q4 — Confere; a fixture é construível com K6 inalterado durante o percurso.** Já existe fixture com K6 estável em falso, `U60=U163={saida}`, respostas coincidentes com os marcos e validação administrativa que grava o par, reduzindo `universo` de 1 para 0. (`app/chamados/tests/test_bug_par_projetado.py:51`; `app/chamados/tests/test_bug_par_projetado.py:80`; `app/chamados/tests/test_bug_par_projetado.py:203`; `app/chamados/tests/test_bug_par_projetado.py:209`; `app/chamados/tests/test_bug_par_projetado.py:211`; `app/chamados/tests/test_bug_par_projetado.py:212`; `app/chamados/tests/test_bug_par_projetado.py:217`.)

   Por inspeção, essa construção permite cobrar `pendentes_aval: 1→0`, `resolvidas: 0→1` e `congelados_no_recorte=1`. L3 continua pendente, portanto zero global não decorre do contrato. O F4 atual ainda obtém zero mudando K6; não prova a exigência corrigida. A execução da nova prova é **[nao verificado]**. (`app/chamados/tests/test_bug_par_projetado.py:47`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:351`; `app/chamados/services/materializacao.py:356`; `app/chamados/tests/test_bug_par_103.py:89`.)

Quanto a exportada, dinheiro e `!`, as quatro respostas não ampliam a autorização original. A validação já agenda recálculo antes do plantio; ausência de batida nova não demonstra ausência de escrita financeira. Tocar gravado exportado continua sujeito ao `!`; efeito produtivo concreto é **[nao verificado]**. (`complemento3_espinha_bug_par_c4_1611.txt:1`, texto fornecido; `app/chamados/services/validacao.py:627`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/fechamento.py:1160`; `CLAUDE.md:265`; `CLAUDE.md:411`.)

VEREDITO: CONFERE

## 16/11 17:02 · sessao 863272485906 · - · - · origem=Ronald (tmux)

**CONFERE, explicitando a diferença das ordens.** A unidade fixada no complemento3 é o estado da testemunha; nessa unidade, a precedência relativa informada confere: K6, F6, DXG, aplicável, preservadas as guardas intermediárias. (`app/chamados/services/materializacao.py:861`; `app/chamados/services/materializacao.py:884`; `app/chamados/services/materializacao.py:897`; `app/chamados/services/materializacao.py:908`; `app/chamados/services/materializacao.py:911`; `app/chamados/services/materializacao.py:914`.)

A porta consulta DXG antes, depois implausibilidade, K6 explícito e materialização/F6; Q1 preserva expressamente essa ordem, e a NOTA separa os retornos. Portanto, a diferença deve constar no relatório, mas não constitui colisão residual que exija devolver o aval. (`app/chamados/services/validacao.py:571`; `app/chamados/services/validacao.py:577`; `app/chamados/services/validacao.py:614`; `app/chamados/services/validacao.py:645`; `app/chamados/services/materializacao.py:753`; `app/chamados/services/materializacao.py:754`.)

## 16/11 17:01 · sessao 863272485906 · - · - · origem=Ronald (tmux)

- **Q1 confere quanto à premissa e ao universo.** `_competencia_que_barra` pula U163 antes de consultar a data; fora163 sem dia gera o impedimento atual. `competencia_do_fato` retorna `None` antes de chamar K6. Assim, “K6 não consultado por falta de dia” corrige o “nenhum juiz consultado” de L5 sem exigir mudança do helper. (`app/chamados/services/materializacao.py:797`; `app/chamados/services/materializacao.py:799`; `app/chamados/services/materializacao.py:800`; `app/chamados/services/materializacao.py:780`; `app/chamados/services/materializacao.py:785`.)

- **Q1 não garante que a porta devolva L5 em qualquer sobreposição.** A porta consulta a testemunha para extrair exclusivamente DXG; se a testemunha disser o atual impedimento por falta de dia, essa consulta devolve vazio. A porta ainda pode retornar `implausivel` antes da guarda explícita de K6. Portanto, Q1 preserva corretamente a ordem atual, mas uma promessa em Q2 de “K6/L5 primeiro também na porta” conflita com essa ordem. (`app/chamados/services/materializacao.py:753`; `app/chamados/services/materializacao.py:754`; `app/chamados/services/materializacao.py:861`; `app/chamados/services/materializacao.py:863`; `app/chamados/services/validacao.py:571`; `app/chamados/services/validacao.py:577`; `app/chamados/services/validacao.py:614`.)

- **Q3 confere: L5 fora163 já não entrava no contador.** O queryset é exclusivamente U163; retirar o balde projetado preserva o universo e não elimina um leitor efetivo de L5. O balde existente `bug_par_103_sem_dia` pertence às candidatas U103 já carimbadas, portanto não representa a L5 fora163. (`app/chamados/services/materializacao.py:321`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:331`; `app/chamados/services/materializacao.py:332`.)

- **A lista do comando já cobre fora163 sem dia.** Ela exclui U163, consulta `competencia_do_fato` e acrescenta `None` à pauta falante. Não depende do contador para listar essa situação. (`app/chamados/management/commands/bug_par_103.py:71`; `app/chamados/management/commands/bug_par_103.py:72`; `app/chamados/management/commands/bug_par_103.py:74`; `app/chamados/management/commands/bug_par_103.py:75`; `app/chamados/management/commands/bug_par_103.py:77`.)

- **Leitores de tela precisam reconhecer o estado novo**, como já autorizado pela matriz: hoje o card e o modal mostram detalhe/escondem Validar pela comparação com `bloqueada_competencia_entregue`; apenas trocar o vocabulário deixa essas comparações sem o novo nome. (`app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/modal_fio.html:252`; `app/templates/chamados/partials/modal_fio.html:274`.)

- **Q1/Q3 não oferecem autorização adicional para exportada ou dinheiro.** A validação existente chama recálculo por evento depois de carimbar; ausência de nova batida não prova ausência de outras escritas. Tocar gravado exportado continua exigindo `!`; efeito produtivo concreto é [nao verificado]. (`app/chamados/services/validacao.py:617`; `app/chamados/services/validacao.py:627`; `app/chamados/services/validacao.py:628`; `CLAUDE.md:265`; `CLAUDE.md:411`.)

## 16/11 17:01 · sessao ef3aaae5c3ee · - · - · origem=Ronald (tmux)

Q4 é construível por inspeção, sem alterar K6: a fixture mantém `competencia_entregue=False`, declara `U60=U163={saida}` e oferece as horas coincidentes com os marcos. (`app/chamados/tests/test_bug_par_projetado.py:51`; `app/chamados/tests/test_bug_par_projetado.py:79`; `app/chamados/tests/test_bug_par_projetado.py:203`; `app/chamados/tests/test_bug_par_projetado.py:209`.)

A construção existente já mostra contador com `universo=1`, seguida de validação administrativa da volta, gravação das duas metades e `universo=0`. A asserção de gravação exige `materializada_em` presente e `VIA_DECLARACAO` nas duas. (`app/chamados/tests/test_bug_par_projetado.py:211`; `app/chamados/tests/test_bug_par_projetado.py:212`; `app/chamados/tests/test_bug_par_projetado.py:215`; `app/chamados/tests/test_bug_par_projetado.py:217`; `app/chamados/tests/test_bug_par_projetado.py:103`.)

Por consequência estática, essa fixture permite exigir `pendentes_aval: 1→0`, `resolvidas: 0→1` e `congelados_no_recorte=1`: U103 está vazio na fixture, perguntas materializadas não elegíveis ao predicado U103 vão a `resolvidas`, e o total congelado depende da seleção por PK, não do estado. (`app/chamados/tests/test_bug_par_projetado.py:47`; `app/chamados/services/materializacao.py:321`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:356`.)

Q4 fecha a distinção entre recorte resolvível e zero global e preserva a contabilização de L3: `espera_declarado` entra em `universo`, que soma em `pendentes_aval`. A identificação literal da métrica na prova deve ser `pendentes_aval`, pois `congelados_no_recorte` permanece constante. (`app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:351`; `app/chamados/services/materializacao.py:356`.)

O teste F4 atual ainda não prova a nova exigência: ele obtém zero mudando K6 para verdadeiro, encaminhando a pergunta a Pauta DP. (`app/chamados/tests/test_bug_par_103.py:89`; `app/chamados/tests/test_bug_par_103.py:93`.) A execução da fixture com as novas asserções é [nao verificado].

## 16/11 16:38 · sessao 055290d9de8e · AUSENCIA-EFEITO-PELO-ESTADO · sonda · origem=Code

Não executei a sonda em produção; reproduzo literalmente a saída fornecida. [nao verificado] [nao verificado]

```text
sonda_leitura: sonda_rejeitadas_que_abonam.py [b4c16ca1b67d644c] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

STATUS existentes {'aprovada': 4476, 'aguardando_decisao': 160, 'rejeitada': 404, 'aguardando_documento': 10}
STATUS sem efeito considerados ['rejeitada']
UNIVERSO ausencias com status sem efeito 404 | por efeito do tipo {'abona': 201, 'desconta': 203}
COM TIPO QUE ABONA 201 | por status {'rejeitada': 201} | por tipo {'atestado': 170, 'abon_hr': 2, 'ferias': 4, 'declaracao': 10, 'reciclagem': 1, 'aviso_home_office': 1, 'folga_abonada': 1, 'troca_de_plantao': 5, 'folga_compensatoria': 1, 'folga': 2, 'abono': 3, 'licenca_luto': 1}
total_erros 0
```

Resumo dos números fornecidos: 404 rejeitadas, sendo 201 com tipo que abona e 203 com tipo que desconta; 0 erros reportados. [nao verificado]

## 16/11 16:38 · sessao ea09b070c83e · AUSENCIA-EFEITO-PELO-ESTADO · leitura · origem=Code

**A coluna Efeito mostra o efeito cadastrado para o tipo, independentemente do status da ausência.** Por isso o código permite “ABONA” junto de “Rejeitada”: são duas leituras separadas. Isso não demonstra abono aplicado no cálculo. `app/ponto/models.py:850`, `app/templates/ponto/ausencias.html:158`, `app/templates/ponto/ausencias.html:176`

O registro específico, o cadastro vigente em produção e eventual repercussão financeira não estão demonstrados pelo dado fornecido. [não verificado]

**(a) Caminho da coluna Efeito**

| Etapa | Origem e leitura |
|---|---|
| Rota | `/ponto/ausencias/` chama `lista_ausencias`. `app/ponto/urls.py:53` |
| View/consulta | `lista_ausencias` obtém as ausências vigentes; `?registro=` permite buscar diretamente pelo PK. `app/ponto/views.py:2165`, `app/ponto/views.py:2188`, `app/ponto/views.py:2191` |
| Contexto | Materializa `ausencias_lista` e entrega como **`ausencias`** ao template `ponto/ausencias.html`. `app/ponto/views.py:2248`, `app/ponto/views.py:2254` |
| Template | Itera `{% for a in ausencias %}`; a célula Efeito lê **`a.efeito`** e escolhe ABONA/DESCONTA/SUPRIME. Não testa status nessa célula. `app/templates/ponto/ausencias.html:142`, `app/templates/ponto/ausencias.html:158` |
| Tag/filtro | A leitura é direta da propriedade, sem tag ou filtro intermediário de efeito. `app/templates/ponto/ausencias.html:158` |
| Propriedade | **`Ausencia.efeito` → `efeito_vigente(self.tipo)`**. Nenhuma consulta a `self.status`. `app/ponto/models.py:850` |
| Catálogo | `efeito_vigente(tipo)` retorna o efeito do catálogo vigente. O catálogo lê `TipoAusencia` ativo — incluindo `TipoAusencia.efeito` — com fallback para `CATALOGO`. `app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:278`, `app/ponto/catalogo/ausencias.py:293` |
| Status separado | Outra célula testa `aprovada`/`rejeitada` e renderiza o badge correspondente. `app/templates/ponto/ausencias.html:176` |

Os contadores e filtros “Abona/Desconta/Suprime” também classificam **pelo tipo**; os contadores são montados antes do filtro de status. `app/ponto/views.py:2201`, `app/ponto/views.py:2208`, `app/ponto/views.py:2220`

**(b) Outros leitores: onde o tipo basta e onde o status já interfere**

| Leitor | O que exibe/lê | Rejeitada ou aberta muda a apresentação? |
|---|---|---|
| **Lista de ausências** | ABONA/DESCONTA/SUPRIME por `a.efeito`. `app/templates/ponto/ausencias.html:158` | **Não muda o efeito exibido.** Status aparece separadamente. `app/templates/ponto/ausencias.html:176` |
| **Drawer — “Esta ausência”** | Nome por `a.get_tipo_display`; status no cabeçalho. O seletor de edição agrupa tipos em COM/SEM ABONO. `app/templates/ponto/partials/painel_ausencia.html:24`, `app/templates/ponto/partials/painel_ausencia.html:48`, `app/templates/ponto/partials/painel_ausencia.html:152`, `app/ponto/catalogo/ausencias.py:313` | O status muda o cabeçalho e as ações; não muda o nome do tipo nem os grupos do seletor. Não há nessa montagem a coluna Efeito da lista. `app/templates/ponto/partials/painel_ausencia.html:24`, `app/templates/ponto/partials/painel_ausencia.html:293`, `app/templates/ponto/partials/painel_ausencia.html:320` |
| **Drawer — Histórico** | Imprime `h.rotulo`/`h.cor`, vindos de `linha_do_tempo`. `app/ponto/views.py:2017`, `app/templates/ponto/partials/painel_ausencia.html:191`, `app/templates/ponto/partials/painel_ausencia.html:198` | **Sim.** Abertas viram pendentes; rejeitada prevalece sobre o efeito do tipo. `app/ponto/services/dia_decidido.py:448`, `app/ponto/services/dia_decidido.py:461`, `app/ponto/services/dia_decidido.py:610` |
| **Grade/calendário — chip** | Ícone/rótulo pelo tipo; indicador `falta` por `_a.efeito == DESCONTA`. `app/colaboradores/services/calendario.py:705`, `app/templates/colaboradores/partials/_calendario_grade.html:97` | A montagem do chip não testa status, **mas sua entrada já é cobertura de cobrança**: rejeitada fica fora; abertas podem entrar. `app/colaboradores/services/calendario.py:516`, `app/ponto/turnos.py:2531` |
| **Grade/calendário — palavra do dia** | Palavra/cor/veredito por `do_dia`, usando cobertura de folha e decisões. `app/colaboradores/services/calendario.py:564`, `app/colaboradores/services/calendario.py:747`, `app/templates/colaboradores/partials/_calendario_grade.html:98` | **Sim.** A decisão pode representar rejeição; pedidos abertos têm tratamento próprio. Cobertura de folha tem precedência na composição. `app/ponto/services/dia_decidido.py:363`, `app/ponto/services/dia_decidido.py:420`, `app/ponto/services/dia_decidido.py:461` |
| **Espelho web/PWA — chip** | Ícone e nome do tipo truncado; cor distingue literalmente `tipo == 'falta'`. `app/templates/ponto/espelho.html:191` | Rejeitada fica fora pela cobertura de cobrança. Abertas entram; o sufixo `(pend.)` procura somente o literal legado `pendente`. `app/ponto/services/espelho.py:582`, `app/templates/ponto/espelho.html:191` |
| **Cartão PDF** | A coluna Obs imprime `dia['palavra_dia']`, preparada pela composição compartilhada. `app/relatorios/pdf_espelho.py:636`, `app/relatorios/pdf_espelho.py:923`, `app/ponto/services/espelho.py:340` | **Sim.** Não desenha o efeito diretamente de `a.efeito`; recebe a palavra que considera cobertura, rejeição e pendência. `app/ponto/services/dia_decidido.py:399`, `app/ponto/services/dia_decidido.py:420`, `app/ponto/services/dia_decidido.py:461` |
| **App — API do espelho** | Publica `veredito_dia`, `palavra_dia`, `cor_dia` e `ausencia_pendente`. `app/api/views.py:1601`, `app/api/views.py:1619` | Recebe a palavra julgada; a pendência usa `ABERTAS`. A renderização efetiva no cliente móvel é [não verificado]. `app/api/views.py:1567`, `app/api/views.py:1574` |
| **App — GET `/api/ausencias/`** | Retorna nome do tipo, status e `status_display` separados; não retorna campo `efeito`. `app/api/views.py:1859`, `app/api/views.py:1863` | **Status não altera o nome retornado.** Rejeitadas permanecem elegíveis ao histórico de até 30 registros. O nome vem do cadastro e pode carregar texto de abono; o fallback de atestado contém “com abono”. `app/api/views.py:1853`, `app/api/views.py:1865`, `app/ponto/catalogo/ausencias.py:250`, `app/ponto/catalogo/ausencias.py:95` |
| **PWA — solicitar ausência** | Grupos COM ABONO/SEM ABONO e `data-pago` nas opções. É apresentação do tipo escolhido para novo pedido. `app/templates/ponto/submeter_ausencia.html:24`, `app/templates/ponto/submeter_ausencia.html:27`, `app/ponto/catalogo/ausencias.py:320` | Essa escolha não recebe status de um registro existente. `app/ponto/views.py:2631`, `app/templates/ponto/submeter_ausencia.html:21` |
| **Relatórios — Absenteísmo e CSV** | Tipo, ocorrências e dias; não imprime badge de efeito. `app/relatorios/views.py:209`, `app/relatorios/views.py:222` | **Só aprovadas entram.** `app/relatorios/views.py:197` |
| **Relatórios — Atestados acumulados e CSV** | “Dias empresa” por `abonam_vigentes`; “Dias INSS” para `atestado_inss`. `app/relatorios/views.py:276`, `app/relatorios/views.py:302`, `app/relatorios/views.py:313` | **Só aprovadas médicas entram.** `app/relatorios/views.py:277` |
| **Histórico anual do colaborador** | Nome do tipo, dias, “Empresa paga”/“INSS assume”; abono pelo conjunto vivo de tipos. `app/templates/ponto/historico_atestados.html:23`, `app/templates/ponto/historico_atestados.html:40`, `app/ponto/views.py:2951` | **Só aprovadas médicas entram.** `app/ponto/views.py:2916` |
| **Atestados em lote** | Reutiliza `linha_do_tempo` e depois filtra tipo/status. `app/ponto/services/atestados_lote.py:54`, `app/ponto/services/atestados_lote.py:57`, `app/ponto/services/atestados_lote.py:62` | **Sim.** Herda o tratamento de pendente/rejeitada da linha do tempo. `app/ponto/services/dia_decidido.py:610` |
| **Exportação da folha** | Tipos que descontam alimentam faltas; tipos abonados, menos `FORA_DO_8932`, alimentam ausência justificada. `app/folha/export.py:138`, `app/folha/export.py:193`, `app/folha/export.py:201` | **Só aprovadas entram no universo consultado.** `app/folha/export.py:87` |

**(c) Status e autoridades existentes**

O vocabulário de **Ausencia** é `aguardando_documento`, `aguardando_decisao`, `aprovada`, `rejeitada`; o default é `aguardando_decisao`. `cancelada` não consta nesses choices. `app/ponto/models.py:701`, `app/ponto/models.py:809`

- **Cancelar individual/em lote pede `rejeitada`**, usando “cancelada” em mensagem/motivo. `app/ponto/views.py:2378`, `app/ponto/views.py:2811`
- **`cancelada` existe em `OrdemSubstituicao`**, outro modelo. `app/ponto/models.py:1051`, `app/ponto/models.py:1057`
- **`pendente` permanece como agrupamento da lista**, traduzido para as duas `ABERTAS`; ainda há leitores literais legados na API de chamados e no chip do espelho. `app/ponto/views.py:2212`, `app/ponto/services/ausencia.py:912`, `app/api/views.py:1061`, `app/templates/ponto/espelho.html:191`
- A máquina de transições está em `_TRANSICOES`, incluindo retorno de rejeitada para aprovada ou para uma das abertas. `app/ponto/services/ausencia.py:914`

As respostas existentes são distintas:

| Pergunta | Autoridade e resultado |
|---|---|
| **Qual o efeito deste tipo?** | `efeito_vigente(tipo)`; não considera status. `app/ponto/catalogo/ausencias.py:293` |
| **Esta ausência participa da cobertura financeira ou da cobrança?** | `_pelo_efeito(qs, para)`: **folha aceita somente aprovada**; **cobrança exclui rejeitada**. É composto por `ausencia_cobre` e `cobertura_ausencia_periodo`, que acrescentam período e desempate. `app/ponto/turnos.py:2523`, `app/ponto/turnos.py:2550`, `app/ponto/turnos.py:2747` |
| **Quais efeitos chegam ao cálculo neste período?** | `efeitos_da_ausencia`: entrega `justificadas`, `suprimidas`, `min_parcial_pagos`, `dia_inteiro`; consulta com `para='folha'`. ABONA justifica; SUPRIME retira previsto; parcial paga gera crédito em minutos. **Não é booleano por registro.** `app/ponto/services/efeito_ausencia.py:28`, `app/ponto/services/efeito_ausencia.py:47`, `app/ponto/services/efeito_ausencia.py:53`, `app/ponto/services/efeito_ausencia.py:63` |
| **Como apresentar o estado deste lançamento?** | `veredito_do_lancamento` trata abertas como pendentes; `veredito_de` passa tipo e status; `veredito_do_tipo` trata rejeitada antes do catálogo; `rotulo_de` compõe a palavra. `app/ponto/services/dia_decidido.py:439`, `app/ponto/services/dia_decidido.py:461`, `app/ponto/services/dia_decidido.py:476`, `app/ponto/services/dia_decidido.py:484` |

O espelho e o PDF consomem `efeitos_da_ausencia`; o motor recebe os conjuntos preparados e os usa para separar falta e abono. O fechamento também seleciona cobertura de folha antes de ler `a.efeito`. `app/ponto/services/espelho.py:1068`, `app/relatorios/pdf_espelho.py:332`, `app/ponto/motor_calculo_v2.py:2595`, `app/ponto/services/fechamento.py:683`

Há uma diferença adicional entre leitores: efeito desconhecido suprime no fechamento (`!= DESCONTA`), enquanto `efeitos_da_ausencia` exige igualdade com SUPRIME. Isso não demonstra ocorrência desse caso em produção. `app/ponto/services/fechamento.py:700`, `app/ponto/services/efeito_ausencia.py:61` [não verificado quanto à ocorrência]

**A célula não equivale ao efeito financeiro individual:** o cartório carimba cobertura de **cobrança**; `CelulaDia.ausencia` está declarado como sombra. Já a grade financeira consulta cobertura de **folha** antes de ler tipo/label. `app/ponto/services/cartorio.py:1356`, `app/ponto/services/cartorio.py:1873`, `app/escala/models.py:1788`, `app/escala/utils.py:1188`

**(d) Quem escreve status e tipo**

| Escritor/porta | Campos e chamadores |
|---|---|
| `criar_ausencia` | Monta `tipo` e `status` explícito ou inicial, instancia e grava. `app/ponto/services/ausencia.py:475`, `app/ponto/services/ausencia.py:483`, `app/ponto/services/ausencia.py:529`, `app/ponto/services/ausencia.py:562` |
| `editar_ausencia_registro` | Atribui `tipo`, grava e, se mudou, rejulga somente status aberto. `app/ponto/services/ausencia.py:648`, `app/ponto/services/ausencia.py:659`, `app/ponto/services/ausencia.py:674`, `app/ponto/services/ausencia.py:687` |
| `mudar_estado` | Valida transição, atribui `status` e grava. `app/ponto/services/ausencia.py:972`, `app/ponto/services/ausencia.py:976`, `app/ponto/services/ausencia.py:988` |
| `decidir` | Aprovação/rejeição delega à mudança de estado. Views individuais e de lote usam `_a4_decidir`. `app/ponto/services/ausencia.py:1065`, `app/ponto/services/ausencia.py:1088`, `app/ponto/views.py:85`, `app/ponto/views.py:2302`, `app/ponto/views.py:2338`, `app/ponto/views.py:2714`, `app/ponto/views.py:2775` |
| Documento/prazo/reabertura | Anexar pode levar a `aguardando_decisao`; vencimento também; reversão recalcula estado pelo tipo/anexo e passa pela porta. `app/ponto/services/ausencia.py:1033`, `app/ponto/services/ausencia.py:1180`, `app/ponto/services/ausencia.py:1298`, `app/ponto/services/ausencia.py:1320` |
| **Exceção: cancelamento de férias** | `rejeitar_ausencias_de_ferias` atribui diretamente `rejeitada`, grava e registra trilha; chamado pelo serviço de férias. `app/ponto/services/ausencia.py:1496`, `app/ponto/services/ausencia.py:1498`, `app/ponto/services/ausencia.py:1499`, `app/ferias/services.py:683` |
| Cadastro `TipoAusencia` | A tela escreve efeito/nome/flags por `gravar_tipo_ausencia`; isso é escrita do **cadastro do tipo**, diferente de trocar `Ausencia.tipo`. `app/ponto/views_tipos_ausencia.py:67`, `app/ponto/views_tipos_ausencia.py:82`, `app/ponto/services/ausencia.py:1761` |

Chamadores da criação, com seus estados solicitados:

- Admin: aprovada; PWA e API deixam o estado inicial ao serviço. `app/ponto/views.py:2472`, `app/ponto/views.py:2477`, `app/ponto/views.py:2607`, `app/api/views.py:1812`
- Veredito da célula e checagem do copiloto: aprovada. `app/ponto/services/veredito_celula.py:337`, `app/ponto/services/checar_ausencia.py:46`
- Materialização de disputa: tipo/status conforme resposta; regularização e abandono provisório também passam pela criação. `app/chamados/services/materializacao.py:491`, `app/chamados/services/materializacao.py:527`, `app/ponto/services/regularizacao.py:118`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76`
- Espelho de férias: tipo `ferias`, aprovada. `app/ponto/services/ausencia.py:1389`

O formulário valida o tipo sem escrever o registro; o Django Admin de Ausencia é somente leitura; `Ausencia.save` lê status/tipo para derivar carimbo, minutos e período, sem atribuir esses dois campos nesse corpo. `app/ponto/forms.py:41`, `app/ponto/forms.py:60`, `app/ponto/admin.py:38`, `app/ponto/models.py:878`

**Quem lê, além das superfícies de (a)/(b) e do cálculo de (c):**

| Leitor | Uso de status/tipo |
|---|---|
| Propriedades do modelo | Aberta, exige documento, pode aprovar e efeito. `app/ponto/models.py:710`, `app/ponto/models.py:723`, `app/ponto/models.py:732`, `app/ponto/models.py:850` |
| Precedência | Exclui rejeitadas; usa tipo/período para o fato vencedor. `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:408` |
| Emissão da jornada | Consulta ausências para cobrança. `app/escala/servico_jornada.py:347` |
| Silêncio de cobrança | Diferencia abertas/aprovadas e usa tipo na trilha. `app/chamados/services/silencio_ausencia.py:66`, `app/chamados/services/silencio_ausencia.py:78`, `app/chamados/services/silencio_ausencia.py:94` |
| Crédito parcial | Seleciona folha e tipo pago/parcial para somar minutos. `app/ponto/services/credito_parcial.py:26` |
| Signals | Usa tipo/intervalo para acordar os dias atingidos no cartório. `app/ponto/signals.py:115`, `app/ponto/signals.py:143` |
| Alertas médicos | Cobertura de folha e tipos médicos. `app/ponto/services/alertas_ausencia.py:48` |
| Vigias | Estado vivo, documento/decisor, parcialidade e divergência com veredito. `app/ponto/services/vigia_ausencia.py:59`, `app/ponto/services/vigia_ausencia.py:108`, `app/ponto/services/vigia_ausencia.py:116`, `app/ponto/services/vigia_ausencia_veredito.py:46` |
| Contador “sem efeito na célula” | Cobertura de folha; exclui DESCONTA e parciais dessa classe de divergência. `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49` |
| Consistência de férias | Compara tipo/status/origem com agendamento. `app/ferias/services.py:147`, `app/ferias/services.py:155` |
| Cadastro de tipos | Conta usos de `Ausencia.tipo`. `app/ponto/views_tipos_ausencia.py:23` |

**Anexo:** [app/docs/JUIZES.md](/home/ronald/codex_ws/ebc7af1a97d9b6ed91910127b1d9a1541551f90b/app/docs/JUIZES.md:1). O documento aponta cobertura e `ABERTAS`, explicita que `pendente` não é status e declara primazia do código vivo; o registro distingue efeito do tipo, cobertura, estado inicial e aberta/decidida. `app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:55`, `app/core/juizes.py:260`

## 16/11 16:36 · sessao b937b08b7800 · - · - · origem=Ronald (tmux)

Mapa confirmado dos demais leitores:

| Leitor | O que exibe | O status interfere? | Referências |
|---|---|---|---|
| Grade/calendário: chip | Ícone e rótulo pelo tipo; cor de falta por `a.efeito == DESCONTA`. | A construção do chip não testa status, mas recebe cobertura `para='cobranca'`, que exclui rejeitadas e inclui abertas. | `app/colaboradores/services/calendario.py:516`, `:705`, `:706`; `app/ponto/turnos.py:2531`; `app/templates/colaboradores/partials/_calendario_grade.html:97` |
| Grade/calendário: palavra | Palavra/cor/veredito por `do_dia`, cobertura de folha e decisões do período. | Sim: a decisão inclui rejeitadas; `veredito_do_tipo` devolve `REJEITADO` antes de consultar efeito. | `app/colaboradores/services/calendario.py:564`, `:565`, `:747`; `app/ponto/services/dia_decidido.py:461`, `:550`, `:561`; `app/templates/colaboradores/partials/_calendario_grade.html:98` |
| Espelho PWA | Chip com `get_tipo_display` truncado e ícone; não possui coluna Efeito. | Rejeitada é excluída pela cobertura de cobrança. Aberta permanece; marca visual procura apenas o literal legado `pendente`. | `app/templates/ponto/espelho.html:191`; `app/ponto/services/espelho.py:582`; `app/ponto/turnos.py:2531` |
| Cartão PDF | Coluna Obs imprime `dia['palavra_dia']`. | Sim: passa pela mesma montagem `aplicar_palavra_do_dia → do_dia`; não lê `a.efeito` diretamente no desenho. | `app/relatorios/pdf_espelho.py:636`, `:638`, `:923`; `app/ponto/services/espelho.py:340`; `app/ponto/services/dia_decidido.py:461` |
| App: API do espelho | Copia `veredito_dia`, `palavra_dia`, `cor_dia` do espelho; acrescenta `ausencia_pendente`. | Sim: recebe palavra já julgada; pendência usa `ABERTAS` sobre cobertura de cobrança. Renderização efetiva no cliente móvel: [não verificado]. | `app/api/views.py:1361`, `:1387`, `:1393`, `:1567`, `:1574`, `:1578`, `:1601`, `:1619` |
| App: GET `/api/ausencias/` | Nome do tipo, status e `status_display` separados; **não envia campo efeito**. O nome vivo pode conter “com abono”. | Status não modifica nome do tipo; rejeitada continua no histórico retornado. | `app/api/urls.py:82`; `app/api/views.py:1853`, `:1859`, `:1865`, `:1867`; `app/ponto/catalogo/ausencias.py:250`, `:303`, `:95` |
| PWA: solicitar ausência | Grupos “COM ABONO - nao desconta”/“SEM ABONO - desconta ou suprime”; opções com `data-pago`. | É escolha de tipo para novo pedido, sem status de registro nessa montagem. | `app/ponto/views.py:2631`; `app/templates/ponto/submeter_ausencia.html:24`, `:27`; `app/ponto/catalogo/ausencias.py:313`, `:320`, `:322` |
| Relatório Absenteísmo + CSV | Tipo cru, ocorrências e dias. Não exibe badge de efeito. | Só aprovadas entram. | `app/relatorios/views.py:197`, `:209`, `:222`, `:228`; `app/templates/relatorios/absenteismo.html:41` |
| Relatório Atestados acumulados + CSV | Dias empresa por pertencimento a `abonam_vigentes`; dias INSS para `atestado_inss`. | Só aprovadas médicas entram. | `app/relatorios/views.py:276`, `:277`, `:283`, `:302`, `:304`, `:313`; `app/templates/relatorios/atestados.html:40`, `:47` |
| Histórico anual de atestados | `get_tipo_display`, dias; cartões “Empresa paga” e “INSS assume”. | Só aprovadas médicas entram; abono usa conjunto vivo de tipos. | `app/ponto/views.py:2916`, `:2921`, `:2951`, `:2957`; `app/templates/ponto/historico_atestados.html:23`, `:27`, `:40` |
| Relatório de atestados em lote | Reutiliza `linha_do_tempo`; filtra depois por tipo/status. | Sim: linha do tempo usa `veredito_do_lancamento`, que trata abertas como pendentes, e rejeitada prevalece no juiz visual. | `app/ponto/services/atestados_lote.py:54`, `:57`, `:62`; `app/ponto/services/dia_decidido.py:610`, `:613`, `:448`, `:461` |
| Exportação folha | Tipos que descontam alimentam faltas; tipos que abonam, descontado `FORA_DO_8932`, alimentam ausência justificada. | Só aprovadas entram em `ausencias_na_janela`. | `app/folha/export.py:88`, `:138`, `:140`, `:180`, `:197`, `:198`, `:204` |

A autoridade visual já existente é `veredito_do_lancamento` para o estado do pedido (`app/ponto/services/dia_decidido.py:439`), `veredito_de` para ausência decidida (`:476`) e `rotulo_de` para sua palavra (`:484`). O núcleo testa rejeição antes do efeito do tipo (`:461`) e consulta o catálogo vivo (`:468`).

Os efeitos numéricos compartilhados por espelho/PDF vêm de `efeitos_da_ausencia`: ausências `para='folha'`, abono integral, supressão e crédito parcial (`app/ponto/services/efeito_ausencia.py:47`, `:53`, `:55`, `:59`, `:61`, `:63`); ambos consumidores estão em `app/ponto/services/espelho.py:1048` e `app/relatorios/pdf_espelho.py:332`.

## 16/11 16:36 · sessao 74d3dd04992b · - · - · origem=Ronald (tmux)

- **Vocabulário atual:** `aguardando_documento`, `aguardando_decisao`, `aprovada`, `rejeitada`; default `aguardando_decisao`. Não há `cancelada` nos choices de **Ausencia**. `pendente` é histórico, convertido por migração, e continua como agrupamento de tela. `app/ponto/models.py:690`, `app/ponto/models.py:809`, `app/ponto/migrations/0057_a4_backfill_pendente.py:35`, `app/ponto/views.py:2212`.
- **“Cancelar ausência” grava `rejeitada`:** individual em `app/ponto/views.py:2797` e chamada em `app/ponto/views.py:2811`; lote em `app/ponto/views.py:2357` e `app/ponto/views.py:2378`. “Cancelada” aparece em mensagens, descrição e motivo, mas o valor persistido pedido é `rejeitada`. `app/ponto/views.py:2812`, `app/ponto/views.py:2818`, `app/ponto/views.py:2389`.
- **`cancelada` em outro modelo:** é status de `OrdemSubstituicao`, não Ausencia. `app/ponto/models.py:1051`, `app/ponto/models.py:1057`.
- **Máquina de estados:** abertas → decisão/rejeição; aprovada → rejeitada; rejeitada → aprovada ou uma das abertas. `ABERTAS=('aguardando_documento','aguardando_decisao')`. `app/ponto/services/ausencia.py:912`, `app/ponto/services/ausencia.py:914`.
- **Autoridade que considera status para efeito:** `_pelo_efeito(qs, para)` aceita **folha somente `aprovada`**; **cobrança todas exceto `rejeitada`**. É usada por `ausencia_cobre`, `ausencias_do_periodo`, `cobertura_ausencia_periodo`. `app/ponto/turnos.py:2523`, `app/ponto/turnos.py:2535`, `app/ponto/turnos.py:2550`, `app/ponto/turnos.py:2747`.
- **Não confundir efeito do tipo com efeito aplicado:** `Ausencia.efeito` apenas chama `efeito_vigente(self.tipo)`; este lê a posição do efeito no catálogo vigente, sem status. Catálogo consulta `TipoAusencia` ativo, com fallback para catálogo estático. `app/ponto/models.py:850`, `app/ponto/catalogo/ausencias.py:293`, `app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:278`. O catálogo estático declara atestado como ABONA; o cadastro vigente de produção não consta no dado fornecido [não verificado]. `app/ponto/catalogo/ausencias.py:95`.
- **Função existente que entrega os efeitos ao cálculo:** `efeitos_da_ausencia(colaborador, ini, fim)` devolve `justificadas`, `suprimidas`, `min_parcial_pagos`, `dia_inteiro`; seleciona com `para='folha'` tanto na coleção de parciais quanto na cobertura. ABONA acrescenta dia justificado, SUPRIME acrescenta dia suprimido, DESCONTA deixa o dia previsto. Parcial paga gera crédito em minutos e não abona o dia inteiro. Não é booleano por registro; entrega agregados por período. `app/ponto/services/efeito_ausencia.py:28`, `app/ponto/services/efeito_ausencia.py:47`, `app/ponto/services/efeito_ausencia.py:53`, `app/ponto/services/efeito_ausencia.py:55`, `app/ponto/services/efeito_ausencia.py:63`.
- **Motor:** recebe conjuntos já preparados pela aplicação; encontra faltas nos dias previstos que não foram trabalhados nem justificados, e conta abono nos dias previstos sem trabalho mas justificados. `app/ponto/motor_calculo_v2.py:2581`, `app/ponto/motor_calculo_v2.py:2595`, `app/ponto/motor_calculo_v2.py:2606`, `app/ponto/motor_calculo_v2.py:2620`.
- **Alimentação no espelho:** chama `efeitos_da_ausencia`, retira suprimidas do previsto e injeta justificadas e dias de ausência inteira no motor. `app/ponto/services/espelho.py:1068`, `app/ponto/services/espelho.py:1111`, `app/ponto/services/espelho.py:1133`. PDF também chama a função. `app/relatorios/pdf_espelho.py:332`.
- **Alimentação da folha:** usa cobertura `para='folha'`, depois lê `a.efeito`, separa justificadas e suprimidas, passa ao motor; o ramo de tipo desconhecido é próprio desse leitor (`!= DESCONTA` suprime), diferente da igualdade `SUPRIME` na função extraída. `app/ponto/services/fechamento.py:683`, `app/ponto/services/fechamento.py:693`, `app/ponto/services/fechamento.py:700`, `app/ponto/services/fechamento.py:731`, `app/ponto/services/fechamento.py:739`.
- **Grade:** já seleciona cobertura de folha, portanto rejeitada e ambas abertas não entram; depois lê tipo/label, marca `falta_aprovada` para falta, ou `ausencia_tipo`, `ausencia_label`, `ausencia_id`, `cobertura_parcial` para demais. `app/escala/utils.py:1188`, `app/escala/utils.py:1213`, `app/escala/utils.py:1220`, `app/escala/utils.py:1230`.
- **Célula não equivale a efeito financeiro:** o cartório consulta `ausencia_cobre(...,para='cobranca')` e carimba essa ausência na célula; esse campo está declarado sombra, com allowlist de leitores vazia no contrato. `app/ponto/services/cartorio.py:1356`, `app/ponto/services/cartorio.py:1873`, `app/escala/models.py:1788`, `app/escala/tests/test_contract_sombra_celula_ausencia.py:28`.
- **Estado do dia lavrado é outra pergunta:** `CelulaDia.status_dia/status_rotulo` são escritos por `lavrar_veredito` com `status_da_lavra`; a documentação aponta `status_do_dia_lavrado`, sem declarar que seja o efeito de uma Ausencia individual. `app/escala/models.py:1807`, `app/docs/JUIZES.md:125`.
- **Juiz de extensão:** `cobre_parte_do_dia` verifica tipo que aceita horas, um dia e minutos entre 0 e 480; `ausencia_explica_o_dia_inteiro` compõe tipo diferente de falta com ausência não parcial; `dias_com_ausencia_de_dia_inteiro` exige adicionalmente efeito ABONA e recebe cobertura da folha. `app/ponto/turnos.py:2597`, `app/ponto/turnos.py:2682`, `app/ponto/turnos.py:2692`.

Leitores backend adicionais para o mapa (d):

| Leitor | Leitura e consequência | Fonte |
|---|---|---|
| Precedência `fatos_do_periodo` | Exclui rejeitadas; lê tipo e período para decidir fato vencedor do calendário. Abertas podem entrar nesse domínio. | `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:408` |
| Cartório | Cobertura por cobrança, carimba ausência na célula. | `app/ponto/services/cartorio.py:1356`, `app/ponto/services/cartorio.py:1873` |
| Silêncio de cobrança | Lê `ABERTAS` para suspender cobrança; aprovada pode encerrar após checar premissa; usa tipo na trilha. | `app/chamados/services/silencio_ausencia.py:66`, `app/chamados/services/silencio_ausencia.py:78`, `app/chamados/services/silencio_ausencia.py:94` |
| Emissão da jornada | Consulta ausências `para='cobranca'`, lê tipo/minutos/datas. | `app/escala/servico_jornada.py:347` |
| Signals | Save/delete de Ausencia acorda cartório; lê tipo e intervalo para determinar dias atingidos. | `app/ponto/signals.py:115`, `app/ponto/signals.py:143`, `app/ponto/signals.py:150` |
| Alertas médicos | Cobertura `para='folha'` e tipos médicos do cadastro; conta registros/dias. | `app/ponto/services/alertas_ausencia.py:48` |
| Vigia de ausência | Exclui rejeitadas no universo vivo; lê tipo para cobertura/catálogo; conta aguardando decisão, aprovada sem documento ou decisor. | `app/ponto/services/vigia_ausencia.py:59`, `app/ponto/services/vigia_ausencia.py:84`, `app/ponto/services/vigia_ausencia.py:108`, `app/ponto/services/vigia_ausencia.py:116`, `app/ponto/services/vigia_ausencia.py:121` |
| Vigia ausência × veredito | Lê aprovação, parcialidade e tipo para contar divergências. | `app/ponto/services/vigia_ausencia_veredito.py:46`, `app/ponto/services/vigia_ausencia_veredito.py:54` |
| Contador sem efeito na célula | Usa cobertura de folha; exclui DESCONTA e parciais da classe “sem efeito”. | `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:49` |
| Consistência férias | Lê tipo férias/status/origem e compara agendamento; cobertura financeira passa por `_pelo_efeito(...,'folha')`. | `app/ferias/services.py:147`, `app/ferias/services.py:155` |

**Anexo/registro:** `app/docs/JUIZES.md:7` declara documento como lookup e `app/docs/JUIZES.md:14` dá primazia ao código vivo; `app/docs/JUIZES.md:55` aponta cobertura e ABERTAS e explicita que `pendente` não é status; `app/docs/JUIZES.md:121` repete os juízes de cobertura. O registro vivo distingue efeito do tipo (`efeito_vigente`), dias ocupados (`cobertura_ausencia_periodo`), documento (`estado_inicial`) e aberta/decidida (`ABERTAS`). `app/core/juizes.py:260`.

## 16/11 16:35 · sessao 174a99204d3a · - · - · origem=Ronald (tmux)

Mapa de escritores vivos, sem consultar banco ou executar aplicação:

| Operação | Chamador | Escrita efetiva |
|---|---|---|
| Criar ausência | `app/ponto/services/ausencia.py:262` | `tipo` e `status` montados em `app/ponto/services/ausencia.py:483`; instância em `:529`; gravação em `:562`. |
| Determinar estado inicial | `app/ponto/services/ausencia.py:475` | `estado_inicial` usa tipo/anexo: `app/ponto/services/ausencia.py:926`, retornos em `:938`. |
| Editar tipo | Tela: `app/ponto/views.py:2873` | Atribuição em `app/ponto/services/ausencia.py:648`, gravação em `:659`; tipo alterado chama rejulgamento em `:674`. |
| Rejulgar estado pelo tipo | `app/ponto/services/ausencia.py:680` | Somente estados de `ABERTAS`, preservando decisões: `:687`; usa `estado_inicial` em `:689` e `mudar_estado` em `:692`. |
| Mudar status | `app/ponto/services/ausencia.py:955` | Valida transição em `:972`, atribui em `:976`, grava em `:988`. |
| Aprovar/rejeitar | `app/ponto/services/ausencia.py:1057` | Aceita somente `aprovada`/`rejeitada` em `:1065`; delega a `mudar_estado` em `:1088`. |
| Anexar documento | Web `app/ponto/views.py:2159`; API `app/api/views.py:3117`; reenvio API `:1845` | `aguardando_documento → aguardando_decisao` em `app/ponto/services/ausencia.py:1033`. |
| Reverter rejeição | `app/ponto/views.py:2655` | `app/ponto/services/ausencia.py:1281` chama `_reabrir`; estado pelo tipo/anexo em `:1298`; escrita pela porta em `:1320`. |
| Prazo de documento vencido | Comando `app/ponto/management/commands/escalonar_documentos_ausencia.py:25` | `aguardando_decisao`, pela porta, em `app/ponto/services/ausencia.py:1180`. |
| Rejulgar passivo pelo tipo | `app/ponto/management/commands/rejulgar_status_por_tipo.py:35` | `rejulgar_status_pelo_tipo → mudar_estado`, `app/ponto/services/ausencia.py:692`. |
| Cancelar férias | `app/ferias/services.py:683` | **Exceção explícita à porta geral**: `rejeitar_ausencias_de_ferias` atribui diretamente `rejeitada` em `app/ponto/services/ausencia.py:1496` e grava em `:1498`, com trilha em `:1499`. |

Chamadores de criação:

- Admin lança `aprovada`: `app/ponto/views.py:2472`, status em `:2477`.
- Colaborador web: `app/ponto/views.py:2607`; status determinado pelo serviço, conforme chamada sem parâmetro em `:2611`.
- App/API: `app/api/views.py:1812`; serviço determina estado inicial em `:1815`.
- Copiloto: `app/core/views_ia.py:985 → app/ponto/services/checar_ausencia.py:46`, com `aprovada` em `:47`.
- Veredito da célula: `app/ponto/services/veredito_celula.py:337`, com `aprovada` em `:338`.
- Disputa validada: `app/chamados/services/materializacao.py:491`, deixando estado inicial ao serviço em `:493`.
- Resposta da disputa: `app/chamados/services/materializacao.py:527`, passa tipo/status em `:528` e `:532`; contrato falta confessada aprovada/atestado pelo estado inicial em `:505`.
- Regularização: `app/ponto/services/regularizacao.py:118`, tipo `falta` em `:119`, estado pelo serviço em `:121`.
- Abandono provisório: `app/ponto/management/commands/marcar_abandono_provisorio.py:76`, tipo `falta` em `:79`, estado pelo serviço em `:82`.
- Férias: `app/ferias/models.py:394` e `:449` chamam sincronização; criação `ferias/aprovada` em `app/ponto/services/ausencia.py:1389`.

Decisões pelas views usam o adaptador `_a4_decidir → decidir`: `app/ponto/views.py:85`, `:88`. Chamadores: aprovar lote `:2302`; rejeitar lote `:2338`; cancelar lote `:2378`; aprovar individual `:2714`; rejeitar individual `:2775`; cancelar individual `:2811`; edição de período aprovado `:2747`.

“Cancelar” grava **`rejeitada`**, inclusive em lote; “cancelada pelo admin” aparece como motivo/descrição, não novo status: `app/ponto/views.py:2378`, `:2379`, `:2811`, `:2812`. Vocabulário do modelo: `aguardando_documento`, `aguardando_decisao`, `aprovada`, `rejeitada`: `app/ponto/models.py:701`. Default: `aguardando_decisao`: `:809`. Transições em `app/ponto/services/ausencia.py:914`.

O Django Admin é leitor: `AusenciaAdmin(SoLeitura, ...)` em `app/ponto/admin.py:38`, apresenta/filtra tipo e status em `:44`; bloqueios de adição/alteração/exclusão em `app/core/admin.py:59`. O formulário valida tipo, mas é `forms.Form`, sem escrita própria: `app/ponto/forms.py:41`, `:60`.

`Ausencia.save` lê status/tipo para carimbo, minutos e período, sem atribuir status/tipo no corpo inspecionado: `app/ponto/models.py:878`, `:882`, `:887`, `:912`, `:918`.

A busca encontrou criação/atribuições diretas do fato somente no serviço acima; a garantia absoluta contra formas dinâmicas de escrita é [não verificado]. Há contratos estáticos dedicados à porta única em `app/ponto/tests/test_contract_porta_ausencia.py:68` e `app/ponto/tests/test_contract_um_produtor_de_ausencia.py:88`; não foram executados.

## 16/11 16:10 · sessao a83cfe3ea860 · BUG-PAR-O-PAR · verificador · origem=Code

**As decisões 1(1) e 1(2) procedem como correções de origem; a abertura 1(3) precisa devolver as condições literais de L5, do contador/F4 e da precedência L2/L3.** A ambiguidade do estado e a perda do motivo estão no código; os pontos devolvidos abaixo exigem delimitação do contrato. (`app/chamados/services/materializacao.py:801`; `app/chamados/services/materializacao.py:863`; `app/chamados/services/validacao.py:527`; `app/chamados/services/materializacao.py:356`.)

1. **Premissas, origem e leitores**

O caso atual nasce assim: `competencia_do_fato` retorna `None` quando falta `data_evento`, antes de chamar K6; `_competencia_que_barra` transforma isso em texto impeditivo; testemunha e validação convertem qualquer impedimento desse helper em `bloqueada_competencia_entregue`. Portanto, hoje “entregue” também significa “faltou dia para consultar entrega”. (`app/chamados/services/materializacao.py:780`; `app/chamados/services/materializacao.py:785`; `app/chamados/services/materializacao.py:799`; `app/chamados/services/materializacao.py:863`; `app/chamados/services/validacao.py:614`.)

Há uma delimitação importante: esse ramo **pula U163 antes de conferir a data**. Dentro de U163, o derivador aceita data do contexto; sem data também no contexto, a testemunha pode devolver `invalida`. Logo, separar o retorno ambíguo existente não equivale automaticamente a instituir L5 para todas as perguntas sem `data_evento`. (`app/chamados/services/materializacao.py:797`; `app/chamados/services/materializacao.py:613`; `app/chamados/services/materializacao.py:618`; `app/chamados/services/materializacao.py:871`.)

| Leitor | Comportamento atual |
|---|---|
| Card | Mostra detalhe de competência e suprime Validar pelo estado entregue; F6 conserva o botão. (`app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/_pergunta_card.html:100`.) |
| Modal | Mostra detalhe e suprime Validar por comparação do estado. (`app/templates/chamados/partials/modal_fio.html:252`; `app/templates/chamados/partials/modal_fio.html:274`.) |
| `fio.py` | Transporta o dict da testemunha; ausência/falha vira `indisponivel`. (`app/chamados/services/fio.py:374`; `app/chamados/services/fio.py:377`; `app/chamados/services/fio.py:382`.) |
| Filtro | Consulta a testemunha e adapta ausência/falha para indisponibilidade. (`app/chamados/templatetags/chamados_tags.py:136`; `app/chamados/templatetags/chamados_tags.py:139`.) |
| Contador | Lê o estado das não carimbadas; estados sem ramo específico caem em `outras`. (`app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:347`.) |
| Comando | Consulta `competencia_do_fato` diretamente; já lista ausência de dia separadamente da entrega. (`app/chamados/management/commands/bug_par_103.py:58`; `app/chamados/management/commands/bug_par_103.py:62`; `app/chamados/management/commands/bug_par_103.py:74`.) |
| Casca da ação | Para recusas não especializadas, apresenta `msg` conforme `ok=False`. (`app/chamados/services/acoes_disputa.py:50`.) |

Um estado próprio **não exige juiz novo**: `ESTADOS` é vocabulário de retorno, cobrado pelo selo AST; ausência de data já é distinguida pelo helper existente, e K6/F6 já possuem autoridades registradas. Entretanto, acrescentar somente o nome à tupla não ensina os HTMLs a mostrar o detalhe e esconder Validar: suas comparações também precisam reconhecer o estado. (`app/chamados/services/validacao.py:51`; `app/chamados/tests/test_porta_validacao_pergunta.py:164`; `app/chamados/services/materializacao.py:780`; `app/core/juizes.py:315`; `app/core/juizes.py:343`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/modal_fio.html:274`.)

2. **L2: motivo perdido, persistência e O44**

No caminho do par, a validação da pergunta principal ocorre antes de `_validar_o_par`. O savepoint valida a irmã e tenta materializar; se não houver duas perguntas materializadas, levanta `_ParDesfeito`. O tratamento recarrega a principal e retorna `None`, sem transportar `e_par/r_par`, que somente seriam incorporados no sucesso. O chamador então executa a metade sozinha. (`app/chamados/services/validacao.py:617`; `app/chamados/services/validacao.py:519`; `app/chamados/services/validacao.py:525`; `app/chamados/services/validacao.py:527`; `app/chamados/services/validacao.py:531`; `app/chamados/services/validacao.py:645`.)

Na tentativa interna, F6 pode recusar o plantio da irmã; o savepoint descarta suas escritas. Na repetição isolada, a irmã novamente sem validação impede o par, e a principal cai na recusa de paridade. Portanto, nesse percurso, o motivo final pode ser **PARIDADE**, apesar da causa impeditiva encontrada na tentativa conjunta ser a competência. Esse diagnóstico não se estende a toda L2: um plantio direto pode conservar corretamente a recusa F6. (`app/ponto/registro_batida.py:156`; `app/chamados/services/materializacao.py:1662`; `app/chamados/services/materializacao.py:199`; `app/chamados/services/materializacao.py:1353`; `app/chamados/services/materializacao.py:1388`; `app/chamados/services/materializacao.py:1897`.)

O resultado desse caminho recusado é:

- Ato **VALIDACAO** da principal preservado e ato **DEVOLUCAO** acrescentado; `validada_em/validada_por` voltam a nulo. (`app/chamados/services/validacao.py:170`; `app/chamados/services/validacao.py:256`; `app/chamados/services/validacao.py:262`.)
- `recusa_motivo` contém texto técnico da recusa, classificado pelo catálogo; a devolução copia esse motivo para seu ato. No percurso descrito, prevalece a classificação PARIDADE. (`app/chamados/services/materializacao.py:1598`; `app/chamados/services/materializacao.py:1610`; `app/chamados/catalogo/recusa.py:45`; `app/chamados/services/validacao.py:265`.)
- Havendo chamado de destino e mudança de motivo, anexa ocorrência “Ponto NAO gravado no espelho [código]…”. A ausência de carimbo de materialização é condição para devolver a validação. (`app/chamados/services/materializacao.py:1612`; `app/chamados/services/materializacao.py:1615`; `app/chamados/services/validacao.py:247`.)

**Corrigir o motivo cabe no O44**, preservando o toque: O44 conserva o ato administrativo, enquanto F6 continua recusando o plantio institucional; o próprio catálogo declara que `COMPETENCIA_APROVADA` sobrevive ao O44. Manter o motivo verdadeiro nas portas existentes é compatível com L-007, sem criar outro escritor. (`app/chamados/services/validacao.py:562`; `app/ponto/registro_batida.py:156`; `app/chamados/catalogo/recusa.py:16`; `app/docs/LEIS.md:65`; `app/chamados/services/materializacao.py:1591`.)

3. **Condições que precisam fechar no literal**

- **Q1 — L5 significa “K6 não consultado” ou realmente “nenhum juiz consultado”, e cobre qual universo?** A porta chama a espera declarado × gravado e o juiz de implausibilidade antes da guarda explícita de entrega. A composição também usa `declarou_hora` para procurar a irmã. “Nenhum juiz” exige mudar essa ordem; “K6 não consultado por falta de dia” descreve o helper atual. Além disso, U163 é pulado nesse ramo, conforme apontado acima. (`app/chamados/services/validacao.py:571`; `app/chamados/services/validacao.py:577`; `app/chamados/services/validacao.py:614`; `app/chamados/services/validacao.py:190`; `app/chamados/services/validacao.py:201`; `app/chamados/services/materializacao.py:797`.)

- **Q2 — Qual precedência vale quando as condições se sobrepõem?** A chamada antecipada de declarado × gravado consulta a testemunha; nela, entrega vem primeiro, seguida de outras guardas, e F6 antecede o portão DXG. Assim, uma pergunta U163 divergente e também bloqueada por F6 pode sair como `bloqueada_competencia`, não como L3. F6 usa a competência da **data local do plantio**; K6 usa a competência do **dia do fato**. A matriz precisa preservar expressamente essa precedência ou declarar a alteração pretendida. (`app/chamados/services/materializacao.py:753`; `app/chamados/services/materializacao.py:861`; `app/chamados/services/materializacao.py:884`; `app/chamados/services/materializacao.py:897`; `app/chamados/services/materializacao.py:908`; `app/chamados/services/materializacao.py:911`; `app/ponto/janelas.py:84`; `app/chamados/services/materializacao.py:784`.)

- **Q3 — “Balde próprio” de qual contador?** O contador seleciona exclusivamente U163. Já existe `bug_par_103_sem_dia` para candidatas U103 carimbadas; não carimbadas com estado diferente caem em `outras` e continuam somando em `pendentes_aval`. Fora163 aparece na lista separada do comando. Criar uma partição sem dia dentro de U163 é compatível com o universo atual; incluir fora163 nessa conta altera seu universo, contrariando a equivalência exigida pela LEI-AKITA 8 se o rótulo continuar prometendo U163. (`app/chamados/services/materializacao.py:321`; `app/chamados/services/materializacao.py:331`; `app/chamados/services/materializacao.py:347`; `app/chamados/services/materializacao.py:356`; `app/chamados/management/commands/bug_par_103.py:71`; `CLAUDE.md:35`.)

- **Q4 — F4 exige zero num cenário resolvível ou zero global?** Um teste com validação efetiva, K6 estável e pertencimento constante é viável: materializadas que deixam de atender ao predicado U103 passam a `resolvidas`. O teste atual apenas muda K6 para verdadeiro. Porém, L3 continua somando pendência; o zeramento global não decorre da matriz, e sua possibilidade em produção é **[nao verificado]**. O contrato deve nomear `pendentes_aval` e o recorte testado; `congelados_no_recorte` permanece constante. (`app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:356`; `app/chamados/tests/test_bug_par_103.py:89`; `app/docs/PROMPTS.md:2235`.)

Os estados da testemunha e da porta também precisam permanecer identificados na prova: hoje L2 mostra `bloqueada_competencia`, mas a validação devolve `nao_gravou`; L4 mostra `aplicavel`, mas a porta devolve `validada`. Exigir literalmente o mesmo estado das duas seria alteração adicional. (`app/chamados/services/materializacao.py:886`; `app/chamados/services/validacao.py:687`; `app/chamados/services/materializacao.py:900`; `app/chamados/services/validacao.py:705`.)

O runner **aceita `--django` hoje**, configura o ambiente CI e utiliza o runner Django. Sem essa opção, exclui os mutantes integrados; a morte exige falha por asserção, sem erros/skips. Sua execução efetiva para a c4 é **[nao verificado]**, não uma impossibilidade do argumento. A correção da busca HTML também procede: o helper atual procura qualquer formulário de validação, sem restringir `pergunta_id`. (`app/chamados/tests/bug_par_mutantes.py:129`; `app/chamados/tests/bug_par_mutantes.py:150`; `app/chamados/tests/bug_par_mutantes.py:155`; `app/chamados/tests/bug_par_mutantes.py:164`; `app/chamados/tests/bug_par_mutantes.py:196`; `app/chamados/tests/test_bug_par_volta.py:202`.)

4. **Universo protegido, dinheiro e `!`**

O pertencimento permanece exclusivamente por PK, com hash, cardinalidades, disjunção e serialização conferidos pelo carregador. Alterar estado ou motivo não exige descongelar o JSON; a fixture já permite representar conjuntos sintéticos em memória. O corte fora163 exige preservar integralmente entregue nos três caminhos e listar para DP. (`app/chamados/services/universo_bug_par.py:15`; `app/chamados/services/universo_bug_par.py:20`; `app/chamados/services/universo_bug_par.py:26`; `app/chamados/services/universo_bug_par.py:35`; `app/chamados/tests/test_bug_par_volta.py:24`; `app/docs/BACKLOG.md:2066`.)

**Não cabe certificar “sem dinheiro” pela ausência de batida nova.** A validação agenda recálculo antes do plantio; ele roda após commit e chama o escritor de fechamento, inclusive com persistência dos campos calculados em fechamento não aberto. Esse efeito já existe; resultado financeiro concreto ou toque produtivo em exportada nesta obra é **[nao verificado]**. A matriz de persistência L2 precisa ser entendida como persistência da pergunta/atos, não prova de ausência de outras escritas. (`app/chamados/services/validacao.py:627`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/fechamento.py:1160`.)

A correção restrita de estado/motivo não requer, por si, tocar gravado exportado; qualquer execução que o faça continua sujeita ao `!` e às provas previstas. Não há liberação adicional pelo simples fato de o plantio ter sido recusado. (`CLAUDE.md:265`; `CLAUDE.md:406`; `CLAUDE.md:411`; `app/ponto/registro_batida.py:161`.)

A quantidade de perguntas vivas de intervalo sem `data_evento`, exigida em 3.e, é **[nao verificado]**: o material fornecido não contém essa medição produtiva. A lista atual do comando não substitui esse censo, pois suas seleções não aplicam o predicado de pergunta viva. (`app/chamados/management/commands/bug_par_103.py:53`; `app/chamados/management/commands/bug_par_103.py:71`.)

VEREDITO: AVAL-DEVOLVIDO — L5 dispensa somente K6 e cobre qual universo? Qual precedência vale entre L2/L3/L5? O balde sem dia permanece restrito a U163? F4 exige zero de `pendentes_aval` em cenário resolvível ou zero global?

## 16/11 16:08 · sessao 0aa3a2dd9f8b · - · - · origem=Ronald (tmux)

1. **Estado próprio não exige juiz novo.** `ESTADOS` é vocabulário de retorno, conferido por AST; o selo exige que os estados devolvidos pertençam à tupla, sem registrar autoridade nova (`app/chamados/services/validacao.py:51`, `app/chamados/services/validacao.py:57`, `app/chamados/tests/test_porta_validacao_pergunta.py:140`, `app/chamados/tests/test_porta_validacao_pergunta.py:164`). K6 e F6 já têm autoridades registradas (`app/core/juizes.py:315`, `app/core/juizes.py:343`). A composição existente declara que não é juiz novo (`app/chamados/services/materializacao.py:773`, `app/chamados/services/validacao.py:206`).

2. **A ambiguidade sem dia está confirmada:** `competencia_do_fato` retorna `None` sem consultar K6 quando `data_evento=None`; a composição converte isso em texto impeditivo; testemunha e porta traduzem qualquer texto em `bloqueada_competencia_entregue` (`app/chamados/services/materializacao.py:780`, `app/chamados/services/materializacao.py:799`, `app/chamados/services/materializacao.py:863`, `app/chamados/services/validacao.py:614`).

3. **L5 precisa precisar seu universo.** A composição ignora U163 antes de verificar ausência de data; o estado ambíguo desse ramo nasce para metades fora163 (`app/chamados/services/materializacao.py:797`). Dentro de U163, o derivador admite a data do contexto na ausência de `data_evento`; sem ambas, produz erro e a testemunha retorna `invalida` (`app/chamados/services/materializacao.py:613`, `app/chamados/services/materializacao.py:618`, `app/chamados/services/materializacao.py:871`). Portanto L5 universal “sem dia do fato” não corresponde somente à separação localizada do retorno atualmente ambíguo (`app/chamados/services/materializacao.py:797`, `app/chamados/services/materializacao.py:613`).

4. **L5 “nenhum juiz consultado” não fecha literalmente na ordem atual.** Antes da guarda explícita de entrega, a porta chama a espera declarado × gravado e o juiz de implausibilidade; a espera consulta a testemunha, que por sua vez passa pela composição de entrega (`app/chamados/services/validacao.py:571`, `app/chamados/services/validacao.py:577`, `app/chamados/services/validacao.py:614`, `app/chamados/services/materializacao.py:753`, `app/chamados/services/materializacao.py:861`). A própria composição encontra a irmã usando `_resposta_de_hora`, que consulta `declarou_hora`; portanto “K6 não consultado por ausência de competência” é uma condição distinta e demonstrável (`app/chamados/services/validacao.py:190`, `app/chamados/services/validacao.py:201`, `app/chamados/services/validacao.py:215`, `app/chamados/services/materializacao.py:780`).

5. **Leitores exigidos pelo aval estão presentes.** Card mostra detalhe e elimina Validar para o estado entregue; modal faz o mesmo por comparação de estado (`app/templates/chamados/partials/_pergunta_card.html:44`, `app/templates/chamados/partials/_pergunta_card.html:47`, `app/templates/chamados/partials/modal_fio.html:252`, `app/templates/chamados/partials/modal_fio.html:274`). `fio.py` transporta o dict da testemunha e converte ausência/falha em indisponibilidade; filtro faz adaptação equivalente (`app/chamados/services/fio.py:374`, `app/chamados/services/fio.py:377`, `app/chamados/services/fio.py:382`, `app/chamados/templatetags/chamados_tags.py:136`, `app/chamados/templatetags/chamados_tags.py:140`). Um estado novo sem atualizar as comparações dos HTMLs não ganha automaticamente detalhe e supressão de Validar (`app/templates/chamados/partials/_pergunta_card.html:44`, `app/templates/chamados/partials/_pergunta_card.html:100`, `app/templates/chamados/partials/modal_fio.html:274`, `app/templates/chamados/partials/modal_fio.html:306`).

6. **“Contador põe em balde próprio” precisa nomear população e rótulo.** O contador consulta exclusivamente U163; o balde atual `bug_par_103_sem_dia` trata somente candidatas U103 carimbadas sem data; perguntas não carimbadas com outros estados caem em `outras`; fora163 não participa dessa conta (`app/chamados/services/materializacao.py:321`, `app/chamados/services/materializacao.py:326`, `app/chamados/services/materializacao.py:331`, `app/chamados/services/materializacao.py:347`). LEI-AKITA 8 exige rótulo correspondente à conta e contador igual ao universo (`CLAUDE.md:35`). Criar um balde para estado sem dia entre as não carimbadas é compatível com uma partição de U163; incluir fora163 mudaria o universo atual (`app/chamados/services/materializacao.py:321`, `app/chamados/services/materializacao.py:351`, `app/chamados/services/materializacao.py:356`).

7. **Comando já publica linha falante, mas sua lista não é censo de perguntas vivas.** Ele lista U103 por pertencimento e fora163 com resposta/texto/hora e entrega ou ausência de data; não aplica nessa seleção um filtro explícito de pergunta viva (`app/chamados/management/commands/bug_par_103.py:53`, `app/chamados/management/commands/bug_par_103.py:62`, `app/chamados/management/commands/bug_par_103.py:71`, `app/chamados/management/commands/bug_par_103.py:75`). A quantidade produtiva solicitada no item 3.e não consta no dado fornecido: **[nao verificado]**.

8. **F4 pode provar zero de pendência com K6 estável, sem alterar pertencimento; o teste atual não o faz.** O teste existente zera mudando K6 para verdadeiro, sem chamar validação do admin (`app/chamados/tests/test_bug_par_103.py:89`, `app/chamados/tests/test_bug_par_103.py:91`). O contador separa `pendentes_aval` de `congelados_no_recorte`; carimbadas que deixam de atender ao predicado U103 vão para `resolvidas`, preservando U163 (`app/chamados/services/materializacao.py:326`, `app/chamados/services/materializacao.py:328`, `app/chamados/services/materializacao.py:356`). O predicado exige via paridade e carimbo anterior a 18/10; uma recusa nova também pode deixar de atendê-lo, portanto zero isolado não prova plantio correto (`app/chamados/services/materializacao.py:259`, `app/chamados/services/materializacao.py:262`). A origem documental exige “esperado 0 pela validação do admin” (`app/docs/PROMPTS.md:2235`). Zero global de produção, incluindo esperas L3 e sem dia L5 ainda pendentes, permanece **[nao verificado]**; tais estados continuam somados no universo não carimbado (`app/chamados/services/materializacao.py:339`, `app/chamados/services/materializacao.py:351`, `app/chamados/services/materializacao.py:356`).

9. **O runner aceita `--django` hoje.** O argumento existe, configura `config.settings.ci`, executa `django.setup()` e utiliza o runner Django; sem a opção, remove os mutantes de integração ou rejeita um nome integrado solicitado individualmente (`app/chamados/tests/bug_par_mutantes.py:129`, `app/chamados/tests/bug_par_mutantes.py:150`, `app/chamados/tests/bug_par_mutantes.py:155`, `app/chamados/tests/bug_par_mutantes.py:161`). A morte exige falha por asserção, sem erros nem skips (`app/chamados/tests/bug_par_mutantes.py:196`). Execução efetiva dos mutantes de integração nesta revisão: **[nao verificado]**.

10. **A correção do escopo HTML pedida pelo contrato é necessária.** O helper atual procura qualquer formulário contendo a ação de validação, sem limitar `pergunta_id`, enquanto o modal é renderizado com todo o contexto do fio (`app/chamados/tests/test_bug_par_volta.py:197`, `app/chamados/tests/test_bug_par_volta.py:202`, `app/chamados/tests/test_bug_par_volta.py:203`).

## 16/11 16:07 · sessao 47b3c4c134ed · - · - · origem=Ronald (tmux)

- **Mudar somente o motivo da L2 cabe no O44**, preservando o toque: a porta admite o ato do admin antes do plantio e não deve barrá-lo pela tranca; a exigência de registrar o motivo verdadeiro está na L-007. (`app/chamados/services/validacao.py:562-566`; `app/chamados/services/materializacao.py:1813-1815`; `app/docs/LEIS.md:65`; `CLAUDE.md:32-34`.)
- **Estado novo não implica juiz novo** se nomear a falta de insumo já retornada por `competencia_do_fato`; K6 e F6 já possuem autoridades registradas. A ausência de data retorna antes de K6. (`app/chamados/services/materializacao.py:778-785`; `app/core/juizes.py:315`; `app/core/juizes.py:343`.)
- O literal disponível do **CORTE-BUG-PAR-FORA163** exige K6 nos três caminhos fora163 antes do plantio, preservação integral em entregue e lista para DP; não autoriza alterar o universo congelado. (`app/docs/BACKLOG.md:2066`; `app/docs/PROMPTS.md:2382`.)
- **U163 é pertencimento por PK**, independente de recusa/lâmpada/devolução; carregador verifica hash, 60+103, disjunção e serialização. O SHA256 atual confere com a constante. (`app/chamados/services/universo_bug_par.py:7-35`; `app/chamados/data/bug_par_163_2026_11_13.json:1`.)
- **O “balde próprio” precisa explicitar o universo**: o contador consulta apenas U163; carimbadas U103 elegíveis sem data já têm `bug_par_103_sem_dia`, enquanto não carimbadas com outro estado vão para `outras` e continuam em `pendentes_aval`. Fora163 sem data aparece na pauta do comando, mas não nesse contador. (`app/chamados/services/materializacao.py:320-356`; `app/chamados/management/commands/bug_par_103.py:71-85`.)
- A **LEI-AKITA 8** obriga rótulo e universo compatíveis; ampliar esse contador para todas as perguntas vivas alteraria o universo hoje declarado. A medição de produção do contrato 3e não está fornecida: **[nao verificado]**. (`CLAUDE.md:35-36`; `app/chamados/services/materializacao.py:321`; anexo do usuário, sem arquivo local: **[nao verificado]**.)
- **O teste atual F4 não prova validação**: troca K6 para `True` e observa migração para pauta, com `resolvidas` sem avanço. Uma prova nova pode manter K6 e conjunto sintético fixos durante a validação sem editar JSON: a fixture já substitui o carregador em memória. (`app/chamados/tests/test_bug_par_103.py:78-95`; `app/chamados/tests/test_bug_par_volta.py:24-30`.)
- **Dinheiro é efeito possível da validação/materialização**, pois a validação chama recálculo por evento e criar batida dispara o fechamento; corrigir apenas estado/motivo não acrescenta essa chamada. Resultado financeiro concreto: **[nao verificado]**. (`app/chamados/services/validacao.py:617-629`; `app/ponto/registro_batida.py:176-180`.)
- Tocar gravado exportado exige `!`; dinheiro em competência aberta exige reversão, hash da exportada intacto e prova de frota. A abertura restrita a estado/motivo não autoriza apply ou movimentação adicional. (`CLAUDE.md:265-268`; `CLAUDE.md:406-416`; `app/docs/PROMPTS.md:2242`.)

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

_6106 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
