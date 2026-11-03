# CODEX-ESTADO

> Ultima resposta processada: **02/11/2026 22:14** · publicado em **02/11/2026 22:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 02/11 22:14 · sessao e603127b50ca · E4CAL-L1-P3 · verificador · origem=Code

1. **Sim: o delta cura os itens 4 e 5 por inspeção dos testes. Execução verde: [nao verificado].** O teste real agora desliga o mock, congela o relógio durante a chamada inteira e passa `hoje=25/04/2026` ao censo; exige efetivamente `(fio_morto, decisao_do_dia_pendente) = (0, 1)`. (`app/chamados/tests/test_o261_vigia_fio.py:498`, `app/chamados/tests/test_o261_vigia_fio.py:503`, `app/chamados/tests/test_o261_vigia_fio.py:507`.)

   **O `freeze_time` cobre o relógio da porta curta.** `_agora` é uma variável local criada por `timezone.now()`, do `django.utils.timezone`, a cada montagem do contexto. A porta curta chama esse contexto dentro do congelamento; `hoje` deriva de `_agora`, e o mesmo instante segue para o D4. Não vem do parâmetro `hoje` do censo. (`app/colaboradores/services/calendario.py:22`, `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:141`, `app/colaboradores/services/calendario.py:579`.)

   Com o corte padrão 21 da empresa da fixture, em 25/04 a competência corrente começa em 21/04; o dia testado, 16/04, fica anterior a ela. Portanto, desaparece a contradição temporal apontada na volta 1. (`app/chamados/tests/test_o261_vigia_fio.py:26`, `app/chamados/tests/test_o261_vigia_fio.py:34`, `app/colaboradores/models.py:46`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`, `app/colaboradores/services/calendario.py:711`.)

   O item 5 ganha expectativa literal para **F = `('irregular', True)` na tela e `True` pela porta curta**. Isso cobre a lacuna da folga lavrada irregular; rejeição efetivamente executada de mutantes: [nao verificado]. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:67`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:75`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:77`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:83`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:86`.)

2. **Sim: F chega ao ramo lavrado do D4, com a fixture atual.** F é 19/08, criado com `trabalha=False`; `_fixar()`, chamado depois das demais preparações, grava diretamente `status_dia='irregular'`. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:38`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:40`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:54`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:100`.)

   Há um detalhe decisivo: **`dna.marcos={}` não encerra a busca de horários**. Como `if m` é falso, o código cai no template, que na fixture tem horário 07–19; o método de horários não decide trabalho/folga. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:68`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:89`, `app/escala/models.py:1099`, `app/escala/models.py:1108`, `app/escala/models.py:331`, `app/escala/models.py:343`.)

   Esse fim previsto já passou no relógio congelado em 17/09; a comparação temporal devolve encerrado. No D4, `em_curso=False` permite ler a coluna irregular e sair com `fonte='lavrado'` **antes** do ramo que classificaria a natureza como folga. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:32`, `app/escala/servico_jornada.py:92`, `app/ponto/turnos.py:2122`, `app/ponto/status_dia.py:255`, `app/ponto/status_dia.py:266`, `app/ponto/status_dia.py:268`, `app/ponto/status_dia.py:269`, `app/ponto/status_dia.py:291`.)

   **Limite da prova:** F verifica a leitura/oferta de uma coluna irregular fixada numa folga. Não verifica se a derivação produtiva da lavra produziria irregular para esses fatos, pois `_fixar()` escreve a coluna diretamente, enquanto a porta produtiva chama `status_da_lavra`/`status_do_admin`. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:100`, `app/ponto/portas/celula.py:693`, `app/ponto/portas/celula.py:698`.)

3. **Para o passo 3 completo, descartar ambos apenas por serem preexistentes é insuficiente: retirar `eh_trabalho` amplia o conjunto de dias atingíveis pelos defeitos.** O código registra a retirada desse veto e mantém apenas último recurso + passado + status. O v2 cura o relógio do teste específico; a cadeia produtiva continua sem transportar `hoje` à porta. (`app/colaboradores/services/calendario.py:89`, `app/colaboradores/services/calendario.py:103`, `app/chamados/tests/test_o261_vigia_fio.py:504`, `app/chamados/services/vigia_fio.py:400`, `app/colaboradores/services/calendario.py:107`.)

   - **Exceção engolida — agravamento condicional do efeito:** uma falha depois de carregar a janela pode deixar `_perg_viva` vazio/parcial; ausência nesse mapa permite último recurso. Num dia antes vetado por `eh_trabalho=False/None`, com status que pede decisão, essa falha agora pode liberar oferta indevida. A retirada não demonstra maior probabilidade de exceção; demonstra maior alcance quando ela ocorre. Frequência real: [nao verificado]. (`app/colaboradores/services/calendario.py:436`, `app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:451`, `app/colaboradores/services/calendario.py:711`, `app/colaboradores/services/calendario.py:103`.)

     Esse contraexemplo demonstra oferta indevida na tela/porta. Não implica automaticamente erro no contador do censo: pergunta viva já provoca desvio antes da consulta à porta. (`app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/colaboradores/services/calendario.py:115`, `app/chamados/services/vigia_fio.py:392`, `app/chamados/services/vigia_fio.py:400`.)

   - **Relógios distintos — também há ampliação condicional:** mantendo `censo(hoje=17/04)` e a porta em 25/04, o dia 16/04 pertence à competência corrente para o primeiro relógio e à anterior para o segundo. No universo sem vínculo da fixture, o veto antigo mascarava a divergência; após sua retirada, a porta pode oferecer e o censo contar decisão pendente. O v2 alinha os relógios nesse teste, sem corrigir o contrato produtivo. (`app/chamados/tests/test_o261_vigia_fio.py:40`, `app/chamados/tests/test_o261_vigia_fio.py:122`, `app/ponto/janelas.py:24`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:440`, `app/chamados/services/vigia_fio.py:400`, `app/chamados/services/vigia_fio.py:401`, `app/chamados/tests/test_o261_vigia_fio.py:503`.)

4. **Escritores e leitores do que muda ou passa a ser exercitado:**

   | Objeto | Quem escreve/publica | Quem lê |
   |---|---|---|
   | Tempo do teste real | O teste fixa o relógio e fornece `hoje`. (`app/chamados/tests/test_o261_vigia_fio.py:503`) | Censo lê `hoje`; contexto lê `timezone.now()`; D4 recebe `_agora`. (`app/chamados/services/vigia_fio.py:313`; `app/colaboradores/services/calendario.py:140`; `app/colaboradores/services/calendario.py:579`) |
   | Célula/status de F | `_celula` cria a célula e `_fixar` grava a coluna. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:90`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:100`) | D4 lê a coluna; calendário publica o retorno; golden lê status e oferta. (`app/ponto/status_dia.py:266`; `app/colaboradores/services/calendario.py:637`; `app/colaboradores/services/calendario.py:699`; `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:67`) |
   | `resolver_dia_oferecido` | Calendário carimba o resultado do predicado em memória. (`app/colaboradores/services/calendario.py:742`) | Porta curta e grade leem o campo; vigia lê a porta, compartilhada por censo e alcance. (`app/colaboradores/services/calendario.py:115`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:289`; `app/chamados/services/vigia_fio.py:400`; `app/chamados/services/vigia_fio.py:610`) |
   | Invariante do MAPA | Fonte é o contrato no módulo; gerador lê a inscrição e escreve o Markdown. (`app/colaboradores/services/calendario.py:12`; `bin/gerar_mapa.py:43`; `bin/gerar_mapa.py:110`; `bin/gerar_mapa.py:119`) | O artefato agora corresponde ao predicado; execução efetiva da regeneração e consumidores do Markdown: [nao verificado]. (`app/docs/MAPA/calendario-dia-da-tela.md:12`; `app/colaboradores/services/calendario.py:103`) |

   Os consumidores indiretos do contexto incluem view, drawer e relatório de furos. (`app/colaboradores/views.py:1946`, `app/colaboradores/services/drawer.py:121`, `app/relatorios/furos.py:195`.)

## 02/11 22:13 · sessao 66015e8d000e · - · - · origem=Ronald (tmux)

A triagem precisa separar **delta v2** de **passo 3**: o código registra que P3 retirou o veto `eh_trabalho`, deixando último recurso + passado + status; portanto ser preexistente não basta para descartar agravamento por ampliação do domínio. (`app/colaboradores/services/calendario.py:89`, `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:620`.)

- **Exceção engolida: há agravamento condicional do efeito.** `_ini_comp_atual` é preenchido antes da consulta/julgamento das perguntas; uma exceção posterior é capturada, deixando `_perg_viva` vazio/parcial. O cálculo interpreta ausência no mapa como permissão. Para um dia anteriormente vetado por `eh_trabalho=False/None`, passado, de competência anterior e com status `irregular`, esse estado passa a produzir oferta; antes a perna removida ainda a barrava. Isso demonstra ampliação dos casos atingidos, **não aumento da probabilidade da exceção**; frequência real [nao verificado]. (`app/colaboradores/services/calendario.py:436`, `app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:451`, `app/colaboradores/services/calendario.py:711`, `app/colaboradores/services/calendario.py:103`.)

  O contraexemplo seguro é a **oferta na tela/porta curta**. Não atribuir automaticamente essa mesma falha ao contador do vigia: `censo` já desvia perguntas vivas antes de consultar a oferta. (`app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/colaboradores/services/calendario.py:115`, `app/chamados/services/vigia_fio.py:392`, `app/chamados/services/vigia_fio.py:400`.)

- **Relógios distintos: também amplia os dias cujo resultado pode divergir.** A fixture documenta exatamente um novo universo: sem vínculo, anteriormente `eh_trabalho=None`, agora oferecendo `sem_certidao`. O dia é 16/04 e o `_censo` tem default 17/04; a porta, sem parâmetro temporal, chama contexto que lê `timezone.now()`. (`app/chamados/tests/test_o261_vigia_fio.py:26`, `app/chamados/tests/test_o261_vigia_fio.py:40`, `app/chamados/tests/test_o261_vigia_fio.py:122`, `app/colaboradores/services/calendario.py:107`, `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:140`.)

  Contraexemplo estático: manter `censo(hoje=17/04)` e porta em 25/04, corte 21. Em 17/04, 16/04 está na competência corrente e não tem último recurso; em 25/04, fica antes de 21/04 e pode oferecê-lo. Antes o veto de vínculo mascarava isso nesse universo; agora o vigia pode classificar como decisão pendente. O v2 cura o teste específico usando 25/04 em ambos, mas a API continua sem transportar `hoje`. (`app/colaboradores/models.py:46`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`, `app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:711`, `app/chamados/services/vigia_fio.py:313`, `app/chamados/services/vigia_fio.py:400`, `app/chamados/services/vigia_fio.py:401`, `app/chamados/tests/test_o261_vigia_fio.py:503`.)

Escritor/leitores produtivos: calendário publica `status` lido do D4, constrói `pode_ultimo_recurso` e carimba `resolver_dia_oferecido`; porta curta e grade leem esse carimbo; `dono_fora_do_fio` lê a porta e serve `censo`/`censo_alcance`. (`app/colaboradores/services/calendario.py:636`, `app/colaboradores/services/calendario.py:699`, `app/colaboradores/services/calendario.py:711`, `app/colaboradores/services/calendario.py:742`, `app/colaboradores/services/calendario.py:115`, `app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/chamados/services/vigia_fio.py:289`, `app/chamados/services/vigia_fio.py:400`, `app/chamados/services/vigia_fio.py:610`.)

## 02/11 22:12 · sessao b98dec301141 · - · - · origem=Ronald (tmux)

**F chega ao ramo lavrado, por inspeção estática.**

- O R8 define F=19/08/2026, chama `_celula(... trabalha=False, status='irregular')` e aplica `_fixar()` depois das batidas dos demais casos. `_celula` grava `trabalha=False`, `dna={'marcos': {}}`, associa a geradora e guarda o status desejado; `_fixar` escreve diretamente `status_dia` e `status_rotulo`. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:38`, `:40`, `:54`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:87`, `:89`, `:90`, `:91`, `:92`, `:97`, `:100`.)

- **Detalhe decisivo:** `{}` em `dna.marcos` não significa “sem previsto” neste código: `if m` falha e `EscalaColaborador.marcos_do_dia` cai no template. O template da fixture fornece 07–19 e seu método não decide trabalho/folga; assim F ganha fim previsto em 19/08 às 19h. (`app/escala/models.py:1099`, `:1100`, `:1108`, `:326`, `:331`, `:343`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:66`, `:68`; `app/escala/servico_jornada.py:68`, `:69`, `:81`, `:89`, `:92`, `:93`.)

- A geradora está dentro da vigência: vínculo começa em D−40, com D=17/09, portanto antes de F; a seleção por geradora devolve esse vínculo quando a data está na vigência. Com relógio congelado em 17/09, o fim de F já passou e `_dia_encerrado_motivo_puro` retorna `None`; a carga usa esse mesmo julgamento. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:46`, `:82`, `:83`; `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:32`; `app/escala/alimentacao.py:237`, `:247`, `:250`, `:252`; `app/ponto/turnos.py:2119`, `:2122`, `:2164`, `:3004`.)

- Não há batida de F na fixture; as batidas criadas são B e E. No D4, motivo `None` dá `em_curso=False`; a coluna `irregular` entra em `if not em_curso and cel is not None and cel.status_dia`, e o `continue` impede alcançar o ramo de natureza/folga. O teste portanto exercita o ramo `fonte='lavrado'`, embora não faça asserção explícita sobre `fonte`. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:40`, `:43`, `:44`, `:47`, `:48`; `app/ponto/status_dia.py:252`, `:253`, `:255`, `:266`, `:267`, `:268`, `:269`, `:275`, `:291`.)

- A tela recebe o D4 e publica o status sem transformação; o predicado oferece para irregular/passado/último recurso. Os golden novos exigem `('irregular', True)` na tela e `True` na porta curta. Logo a lacuna do item 5 foi coberta no código do teste. Execução/verde e rejeição efetiva de mutantes: **[nao verificado]**. (`app/colaboradores/services/calendario.py:579`, `:636`, `:637`, `:699`, `:94`, `:103`, `:104`, `:711`, `:742`; `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:67`, `:75`, `:77`, `:83`, `:86`.)

**Escritores/leitores úteis à consolidação:**

- Escritor de fixture: `_celula` cria célula/geradora e `_fixar` escreve coluna; leitores: `CargaDaJanela` carrega as células e D4 lê `cel.status_dia`; calendário publica; golden lê tela/porta curta. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:90`, `:91`, `:100`; `app/ponto/turnos.py:2943`, `:2946`, `:2947`, `:2970`, `:2972`; `app/ponto/status_dia.py:251`, `:266`; `app/colaboradores/services/calendario.py:699`; `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:67`, `:86`.)

- Escritor produtivo do status: porta `_status_alvo` chama `status_da_lavra`/`status_do_admin`, `_aplicar_status` atribui coluna; a fixture contorna essa derivação ao fixar coluna diretamente. Assim F prova leitura/oferta de uma coluna irregular numa folga, sem provar que `status_da_lavra` produziria irregular para esses fatos. (`app/ponto/portas/celula.py:693`, `:694`, `:695`, `:698`, `:701`, `:705`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:100`.)

- Leitores produtivos diretos da oferta: porta curta lê o campo, template lê com `pode_veredito`, vigia chama porta curta. (`app/colaboradores/services/calendario.py:112`, `:115`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:280`, `:289`.)

- Mapa novo corresponde ao invariante já publicado no módulo. (`app/docs/MAPA/calendario-dia-da-tela.md:12`; `app/colaboradores/services/calendario.py:12`, `:13`, `:14`.)

## 02/11 22:09 · sessao 42eeb25ed201 · E4CAL-L1-P3 · verificador · origem=Code

1. **Predicado literal: procedente.** São exatamente `pode_ultimo_recurso` + `data < hoje` + status em `('falta', 'irregular', 'aberto', 'sem_certidao')`, sem consultar `eh_trabalho`. A porta curta monta o calendário e lê seu resultado, sem repetir a regra. (`app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:115`.)

   **“Pela lavra” não significa exigir coluna preenchida.** Sem status lavrado, o D4 ainda decide: encerrado com batida → `sem_certidao`; sem batida, ausência descontável → `falta`, ausência abonada → `ausencia`, folga/feriado → `folga`, trabalho encerrado → `sem_certidao`, nenhum fato → `sem_dados`. Portanto, dias sem lavra podem ganhar ou perder a oferta pela regra central, conforme previsto no COMPLEMENTO. (`app/ponto/status_dia.py:270`, `app/ponto/status_dia.py:273`, `app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:290`, `app/ponto/status_dia.py:295`, `app/ponto/status_dia.py:297`.)

   **Outro caminho de ganho/perda: procedente, preexistente ao patch fornecido.** O bloco de último recurso engole exceções. Se falhar antes de obter a competência, `_ini_comp_atual=None` elimina a oferta; se falhar depois, durante a leitura/julgamento das perguntas, `_perg_viva` pode permanecer vazio ou parcial e liberar indevidamente um dia com pergunta viva respondida. Ocorrência em produção: [nao verificado]. (`app/colaboradores/services/calendario.py:436`, `app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:451`, `app/colaboradores/services/calendario.py:711`.)

2. **Escritores e leitores: contrato identificado.**

   | Campo lido | Quem publica no calendário | Fonte |
   |---|---|---|
   | `data` | Dicionário do dia | Janela escolhida pelo GET/modo, percorrida diariamente. (`app/colaboradores/services/calendario.py:154`, `:175`, `:581`, `:695`, `:727`.) |
   | `status` | `status = _st['status']`, publicado sem transformação | D4, alimentado pela mesma carga de células/turnos e pelos fatos A2. (`app/colaboradores/services/calendario.py:480`, `:579`, `:636`, `:699`.) |
   | `pode_ultimo_recurso` | Dicionário do dia | `janela_atual(hoje, empresa)` e ausência de pergunta viva **com resposta**; a vitalidade vem de `pergunta_viva`. (`app/colaboradores/services/calendario.py:440`, `:447`, `:449`, `:711`.) |
   | `resolver_dia_oferecido` | Carimbo após montar os dias | Única chamada produtiva direta ao predicado. (`app/colaboradores/services/calendario.py:742`.) |

   A coluna persistida de status é escrita pela porta da célula, que chama `status_da_lavra` ou `status_do_admin`; o método do modelo delega à porta. (`app/ponto/portas/celula.py:693`, `app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:705`, `app/escala/models.py:1794`.)

   **Leitores produtivos diretos encontrados:** campo → porta curta e grade; porta curta → `dono_fora_do_fio`, que classifica como `decisao_do_dia_pendente`. Esse último caminho é compartilhado por `censo` e `censo_alcance`. (`app/colaboradores/services/calendario.py:115`, `app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/chamados/services/vigia_fio.py:289`, `app/chamados/services/vigia_fio.py:400`, `app/chamados/services/vigia_fio.py:610`.)

   **Leitores indiretos:** view do calendário, drawer e relatório de furos montam o mesmo contexto. A grade exige também `pode_veredito`; o relatório inclui a grade com `only`, sem essa permissão, portanto não apresenta o botão. (`app/colaboradores/views.py:1946`, `app/colaboradores/views.py:1957`, `app/colaboradores/services/drawer.py:121`, `app/colaboradores/services/drawer.py:154`, `app/relatorios/furos.py:195`, `app/templates/relatorios/furos.html:42`, `app/templates/colaboradores/partials/_calendario_grade.html:125`.)

   **Testes que leem/injetam o contrato:** `test_o261_vigia_fio`, `test_o261e_f5_alcance`, `test_calendario_eh_trabalho_le_a_celula`, `test_e4cal_l1_r8_resolver_pela_lavra`, `test_calendario_mostra_o_carro` e a fixture de `test_ui_pilulas_quebram_linha`. (`app/chamados/tests/test_o261_vigia_fio.py:478`, `:897`, `:920`; `app/chamados/tests/test_o261e_f5_alcance.py:66`, `:214`, `:322`, `:374`; `app/colaboradores/tests/test_calendario_eh_trabalho_le_a_celula.py:65`; `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:59`, `:79`; `app/colaboradores/tests/test_calendario_mostra_o_carro.py:142`, `:176`, `:194`; `app/colaboradores/tests/test_ui_pilulas_quebram_linha.py:55`.)

   **Leitor dependente de `eh_trabalho` esquecido: não encontrado no censo textual.** Restam injeção deliberada de `False`/`None` e testes proibindo a chave; o calendário não a publica. Acesso por chave construída dinamicamente, inclusive JS/API: [nao verificado]. (`app/chamados/tests/test_o261_vigia_fio.py:910`, `app/colaboradores/tests/test_calendario_eh_trabalho_le_a_celula.py:80`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:86`, `app/colaboradores/services/calendario.py:694`.)

   **Documentação desatualizada: procedente.** O mapa ainda descreve “dia de trabalho” como perna do predicado. (`app/docs/MAPA/calendario-dia-da-tela.md:12`.)

3. **Conjunto paralelo ao D4: não procedente como segundo juiz.** `STATUS_QUE_PEDEM_DECISAO` pertence ao juiz da **oferta**, em `calendario.py`; o D4 define o vocabulário e decide **qual status o dia tem**. São perguntas diferentes. O patch fornecido conserva a constante, e `_TT11` já aponta para esse predicado. (`app/colaboradores/services/calendario.py:94`, `app/ponto/status_dia.py:30`, `app/ponto/status_dia.py:49`, `app/ponto/status_dia.py:200`, `app/core/juizes.py:704`, `app/core/juizes.py:718`.)

4. **Mocks: isolamento legítimo, mas com perda de integração nos cenários mockados.** O par SIM/NÃO continua testando se o vigia respeita a resposta da autoridade; isso não demonstra o comportamento do juiz real nesses mesmos cenários. No alcance, o caso SIM também permanece mockado. (`app/chamados/tests/test_o261_vigia_fio.py:46`, `app/chamados/tests/test_o261_vigia_fio.py:481`, `app/chamados/tests/test_o261e_f5_alcance.py:66`, `app/chamados/tests/test_o261e_f5_alcance.py:214`.)

   **O novo caso real morde, mas tem relógio solto: procedente.** Ele desliga o mock e exige mudança efetiva dos contadores. A fixture realmente deixa `status_dia` nulo: lavra `furo` sem `fatos_do_status`, situação em que a porta devolve status nulo. (`app/chamados/tests/test_o261_vigia_fio.py:94`, `app/chamados/tests/test_o261_vigia_fio.py:496`, `app/chamados/tests/test_o261_vigia_fio.py:500`, `app/ponto/portas/celula.py:696`.)

   Entretanto, `_censo` usa **17/04/2026**, enquanto a porta curta lê a data real da máquina. Congelando também o calendário em 17/04, o dia 16/04 estaria na competência corrente e não ofereceria Resolver, contrariando a expectativa desse teste. É um contraexemplo estático à estabilidade do selo; execução dessa falha: [nao verificado]. (`app/chamados/tests/test_o261_vigia_fio.py:26`, `app/chamados/tests/test_o261_vigia_fio.py:122`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:711`, `app/ponto/janelas.py:60`.)

   Falta, nesses testes alterados, um caso de `censo_alcance` que retire o mock e use o juiz real. O R8 cobre separadamente a porta real, mas não sua integração com esse universo do vigia. (`app/chamados/tests/test_o261e_f5_alcance.py:66`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:74`.)

5. **RED e mutantes nomeados: mordida procedente por inspeção.** O R8 exige respostas literais para trabalho sem lavra, folga com batida sem lavra, irregular lavrado sem vínculo ativo, folga sem lavra em competência anterior e lavrado `ok`; repete os cinco pela porta curta. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:65`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:77`.)

   | Mutante | Asserção que o rejeita |
   |---|---|
   | P3a: `and dia.get('eh_trabalho')` | Positivos sem a chave passam a devolver falso. (`app/chamados/tests/test_o261_vigia_fio.py:889`, `:897`.) |
   | P3b: `and dia.get('eh_trabalho', True)` | `False` e `None` explícitos devem continuar oferecendo. (`app/chamados/tests/test_o261_vigia_fio.py:909`, `:910`.) |
   | P3c: republicar a chave | `assertNotIn('eh_trabalho', dia)`. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:86`.) |

   **Variantes sobreviventes aos casos P3: possível e demonstrável por inspeção.** O teste cruza todos os status com chave ausente, mas testa `False`/`None` somente com status `falta`. Um gate seletivo para `irregular` com `eh_trabalho=False` escaparia dessas asserções. Como a chave não é publicada atualmente, essa variante isolada não alteraria o calendário real. (`app/chamados/tests/test_o261_vigia_fio.py:889`, `app/chamados/tests/test_o261_vigia_fio.py:896`, `app/chamados/tests/test_o261_vigia_fio.py:909`, `app/colaboradores/services/calendario.py:694`.)

   A variante material — voltar a consultar a célula e vetar **folga lavrada irregular** — também não é exercitada pelos cinco golden: B é sem lavra; C usa `trabalha=True`. Sobrevivência na suíte inteira: [nao verificado]. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:39`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:49`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:87`.)

   Execuções históricas RED/verde e relatórios dos mutantes P3a/P3b/P3c: [nao verificado]. [nao verificado]

6. **Classes de reprovação anteriores:**

   | Classe | Veredito |
   |---|---|
   | Segundo juiz | **Não procedente no P3:** autoridade registrada e porta curta delegante. (`app/core/juizes.py:718`; `app/colaboradores/services/calendario.py:112`.) |
   | Segundo escritor fora da porta | **Não identificado no caminho alterado:** calendário publica o retorno do D4; persistência continua na porta. (`app/colaboradores/services/calendario.py:637`, `:699`; `app/ponto/portas/celula.py:705`.) |
   | Relógio dentro do predicado | **Não procedente:** recebe `hoje`; calendário deriva `hoje` de `_agora` e passa o mesmo instante ao D4. (`app/colaboradores/services/calendario.py:97`, `:140`, `:141`, `:579`, `:742`.) |
   | Relógios distintos na cadeia do vigia | **Procedente:** `censo(hoje=...)` não transmite esse tempo à porta curta. (`app/chamados/services/vigia_fio.py:313`, `:400`, `:289`; `app/colaboradores/services/calendario.py:107`, `:140`.) |
   | Derivação paralela | **Não procedente no P3:** status é lido do D4; fatos A2 são alimentação compartilhada. (`app/colaboradores/services/calendario.py:480`, `:579`, `:636`.) |
   | Selo/RED sem mordida | **Não como acusação geral:** há expectativas literais e casos negativos; permanece o defeito temporal do novo teste real. (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:65`, `:95`; `app/chamados/tests/test_o261_vigia_fio.py:122`, `:499`.) |
   | Ordem de trava | **Nenhuma inversão introduzida identificada:** expressão pura e carimbo em memória. Concorrência de toda a cadeia: [nao verificado]. (`app/colaboradores/services/calendario.py:103`, `:742`.) |
   | Pouso antes do pré-requisito | **[nao verificado].** O comando permite completar status de competência entregue, mas sua existência não prova aplicação do D5 nem ordem de deploy. (`app/ponto/management/commands/lavrar_status_competencia.py:43`, `:56`.) |

   D5 aplicado na sombra/no ar, novos dias ≤50, comparação HEAD×L1 nas competências 06–09, hashes e ordem efetiva de pouso: [nao verificado]. [nao verificado]

7. **Contraexemplos além dos golden: não encontrei inversão causada pela retirada de `eh_trabalho` nas condições abaixo.**

   | Situação adicional, passado e com último recurso | Resultado pelo código |
   |---|---|
   | Desligado/sem escala ativa, D4 devolvendo `irregular` | **Oferece.** O predicado não tem condição de situação/vínculo. (`app/colaboradores/services/calendario.py:103`.) |
   | Antes da admissão, com célula e status lavrado `irregular` | **Oferece**, se o D4 chega ao ramo lavrado; esse ramo não consulta admissão. (`app/ponto/status_dia.py:266`; `app/colaboradores/services/calendario.py:103`.) |
   | Célula `trabalha=False`, lavrada `irregular` | **Oferece**, se chega ao ramo lavrado: a coluna precede a natureza sem lavra. (`app/ponto/status_dia.py:266`, `:275`; `app/colaboradores/services/calendario.py:94`.) |
   | Mesmo cadastro, lavrado `folga` ou `ok` | **Não oferece.** Esses status estão fora do conjunto. (`app/colaboradores/services/calendario.py:94`, `:104`.) |

   Esses resultados seguem a regra de oferta pelo status; os novos golden não cobrem desligado, antes da admissão com célula ou folga **lavrada irregular**. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:76`, `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:38`, `:49`.)

   **Há contraexemplo condicional na cadeia completa:** dia lavrado irregular, anterior à competência corrente, com pergunta viva respondida; se a consulta/julgamento dessa pergunta lançar exceção após carregar a janela, ele pode oferecer indevidamente. É o caminho preexistente do item 1, não efeito da remoção de `eh_trabalho`; ocorrência real: [nao verificado]. (`app/colaboradores/services/calendario.py:440`, `:447`, `:451`, `:711`.)

## 02/11 22:07 · sessao 17f5c627a035 · - · - · origem=Ronald (tmux)

1. **Mocks: isolamento procedente, cobertura integral não demonstrada.** O padrão NÃO passa pela mesma costura que os testes já usavam; os casos SIM sobrepõem o patch, e a comparação SIM/NÃO continua mordendo a classificação do vigia (`app/chamados/tests/test_o261_vigia_fio.py:39-48`, `:472-489`). Na classe de alcance, o SIM também permanece mockado (`app/chamados/tests/test_o261e_f5_alcance.py:58-68`, `:213-218`). Integração real específica de `censo_alcance` não está demonstrada por esses testes; sobrevivência na suíte completa [nao verificado].

2. **Novo caso real tem mordida, mas relógio solto — procedente.** Ele desliga o patch e exige `fio_morto=0` / `decisao_do_dia_pendente=1` (`app/chamados/tests/test_o261_vigia_fio.py:491-501`). A fixture efetivamente deixa status nulo: cria célula e lavra `furo` sem `fatos_do_status` (`:94-96`); `_status_alvo` devolve `(None,None)` quando faltam fatos e muda o veredito (`app/ponto/portas/celula.py:686-698`). Contudo, `_censo` injeta 17/04/2026 (`app/chamados/tests/test_o261_vigia_fio.py:115-123`), enquanto a porta curta chama calendário sem instante (`app/colaboradores/services/calendario.py:107-115`), que lê `timezone.now()` (`:139-141`). Isso contradiz “nenhum caminho deste selo lê relógio real” (`app/chamados/tests/test_o261_vigia_fio.py:116`). Sob relógio 17/04/2026, 16/04 permanece na competência corrente e perde último recurso (`app/colaboradores/services/calendario.py:711-712`); falha executada [nao verificado].

3. **RED pela regra: procedente por inspeção.** R8 cobre trabalho sem lavra, folga com batida sem lavra, lavrado irregular sem vínculo ativo, folga sem lavra e lavrado ok; exige os pares literais status/oferta (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:33-72`). Repete os cinco pela porta curta real (`:74-81`), exige ausência da chave (`:83-86`) e mantém competência corrente sem oferta (`:88-96`). A fixture fixa explicitamente as colunas após as batidas (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:97-100`). Execução RED/verde e relatórios P3a/P3b/P3c [nao verificado].

4. **Mutantes nomeados: mordida estática procedente.**
   
   - P3a, `and dia.get('eh_trabalho')`: morre inclusive no caso positivo sem chave (`app/chamados/tests/test_o261_vigia_fio.py:888-897`).
   - P3b, `and dia.get('eh_trabalho', True)`: morre com False/None explícitos (`:905-910`).
   - P3c, republicar a chave: morre em R8 (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:83-86`) e no teste histórico (`app/colaboradores/tests/test_calendario_eh_trabalho_le_a_celula.py:78-80`).

5. **Variantes além das três: lacuna procedente; sobrevivência completa [nao verificado].** O teste positivo percorre todos os status, sempre sem `eh_trabalho`; o teste False/None usa somente status padrão `falta` (`app/chamados/tests/test_o261_vigia_fio.py:888-910`). Portanto, não cobre o produto status × valor do campo. Um gate seletivo para célula `trabalha=False` **lavrada irregular** não é exercitado pelos novos golden: B é sem lavra/`sem_certidao`, D é `folga`, C usa `trabalha=True` por padrão (`app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:38-50`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:87-92`). Os novos casos também não montam desligado ou antes da admissão com célula; `_colab` cria ativo e não informa admissão (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:76-85`).

## 02/11 22:06 · sessao 3fc40b9d8f18 · - · - · origem=Ronald (tmux)

1. **Segundo juiz: não procedente no passo 3.** A oferta já tem autoridade registrada como `_TT11` em `app/core/juizes.py:704` e `app/core/juizes.py:718`, com corte anterior documentado em `app/docs/CORTES.md:271`. A porta curta monta o calendário e lê o campo; não repete o predicado (`app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:115`).

2. **Constante paralela ao D4: não procedente como duplicação da mesma pergunta.** `STATUS_QUE_PEDEM_DECISAO` classifica status para a oferta (`app/colaboradores/services/calendario.py:94`); o D4 define o vocabulário e responde qual status o dia tem (`app/ponto/status_dia.py:30`, `app/ponto/status_dia.py:49`, `app/ponto/status_dia.py:200`). São perguntas distintas e o patch fornecido conserva a constante preexistente [nao verificado quanto ao histórico Git local, ausente].

3. **Segundo escritor de status: não identificado no caminho alterado.** O calendário recebe a resposta do D4 e atribui `status = _st['status']`, publicando-a no dicionário (`app/colaboradores/services/calendario.py:579`, `app/colaboradores/services/calendario.py:636`, `app/colaboradores/services/calendario.py:699`). A coluna persistida é atribuída em `_aplicar_status`, dentro da porta (`app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:705`), chamada pela lavra (`app/ponto/portas/celula.py:752`, `app/ponto/portas/celula.py:773`).

4. **Relógio fora da borda do predicado: não procedente.** `resolver_dia_oferecido` recebe `hoje` (`app/colaboradores/services/calendario.py:97`); o calendário lê `_agora` e deriva `hoje` uma vez para esse caminho (`app/colaboradores/services/calendario.py:140`), passa esse `_agora` ao D4 (`app/colaboradores/services/calendario.py:579`) e usa `hoje` ao carimbar a oferta (`app/colaboradores/services/calendario.py:742`). Há outra leitura de relógio no mesmo calendário, porém exclusivamente para prazo de desfazer batida (`app/colaboradores/services/calendario.py:227`, `app/colaboradores/services/calendario.py:231`).

5. **Derivação paralela do status: não procedente no passo 3.** O antigo gate por grade foi removido e o calendário lê o D4 (`app/colaboradores/services/calendario.py:620`, `app/colaboradores/services/calendario.py:636`). Os fatos A2 são carregados por autoridade e fornecidos ao D4 como alimentação (`app/colaboradores/services/calendario.py:480`, `app/colaboradores/services/calendario.py:579`); o D4 consome o mapa fornecido sem fallback (`app/ponto/status_dia.py:279`).

6. **Caminho de ganho/perda por outra regra: procedente, preexistente.** O último recurso depende da competência atual e de pergunta viva com resposta (`app/colaboradores/services/calendario.py:440`, `app/colaboradores/services/calendario.py:447`, `app/colaboradores/services/calendario.py:711`). O bloco captura qualquer exceção: falha antes de obter a competência mantém `_ini_comp_atual=None` e elimina a oferta; falha depois de obter a competência pode deixar `_perg_viva` vazio/parcial e permitir oferta apesar de pergunta viva ainda não lida (`app/colaboradores/services/calendario.py:436`, `app/colaboradores/services/calendario.py:451`, `app/colaboradores/services/calendario.py:711`). Falha da A2, diferentemente, propaga (`app/colaboradores/services/calendario.py:488`).

7. **Ordem de trava: alteração nova não identificada no trecho examinado.** O predicado é expressão pura, a porta curta chama o calendário e o carimbo altera apenas o dicionário retornado (`app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:112`, `app/colaboradores/services/calendario.py:742`). Reprodução de concorrência ou prova dinâmica de toda a cadeia [nao verificado].

8. **Pré-requisito D5/pouso: não certificável.** O comando da cópia permite competência entregue, usa `completar_status=True` e oferece `--apply` explícito (`app/ponto/management/commands/lavrar_status_competencia.py:30`, `app/ponto/management/commands/lavrar_status_competencia.py:43`, `app/ponto/management/commands/lavrar_status_competencia.py:56`). O completador grava pela porta apenas quando `apply_` é verdadeiro (`app/ponto/services/cartorio.py:889`). Execução do D5 na sombra/no ar, contagens após D5, comparação HEAD×L1 nas competências 06–09, limites de novos dias, hashes e ordem efetiva de deploy [nao verificado]. O dado fornecido contém condições normativas, sem resultados dessas medições [nao verificado].

9. **Documentação local antiga não deve contrariar o aval anexado.** O ADR ainda afirma que competência entregue não se toca (`app/docs/ADR/0002-celula-e-o-dia.md:71`); o comando já documenta a emenda do COMPLEMENTO que permite completar seu status (`app/ponto/management/commands/lavrar_status_competencia.py:10`). A execução efetiva dessa emenda permanece [nao verificado].

## 02/11 22:06 · sessao af1a8cc5bdcf · - · - · origem=Ronald (tmux)

1. **Escritores do contrato do predicado:** `contexto_calendario` publica `data` em `app/colaboradores/services/calendario.py:695`, `status` em `:699`, `pode_ultimo_recurso` em `:711` e, depois, o resultado do único predicado em `:742`. A data vem da janela escolhida pelos modos/GET (`:154`, `:175`, `:187`), começa em `data_ini` (`:581`) e avança diariamente (`:727`). O status vem exclusivamente do retorno do D4 consultado em `:579`, lido em `:636` e `:637`.

2. **Fonte de último recurso:** início da competência atual vem de `ponto.janelas.janela_atual(hoje, empresa)`, em `app/colaboradores/services/calendario.py:439`; perguntas são filtradas por dia, com resposta não vazia, e submetidas ao juiz `pergunta_viva`, em `:447` e `:449`. O campo exige data anterior ao início atual e ausência dessa pergunta viva em `:711`.

3. **Leitores produtivos diretos completos encontrados:** predicado chamado pelo carimbo em `app/colaboradores/services/calendario.py:742`; campo lido pela porta curta em `:115` e pelo template em `app/templates/colaboradores/partials/_calendario_grade.html:125`; porta curta chamada em `app/chamados/services/vigia_fio.py:289`. O vigia prioriza essa resposta e devolve `decisao_do_dia_pendente` em `:290`, antes das ausências e demais baldes (`:291`, `:294`, `:296`).

4. **Views, drawer e relatórios:** perfil monta contexto em `app/colaboradores/views.py:1946`, acrescenta permissão em `:1957` e renderiza em `:1962`; template inclui a grade em `app/templates/colaboradores/calendario_partial.html:62`. Drawer monta calendário em `app/colaboradores/services/drawer.py:121`, publica `cal`/permissão em `:153` e `:154`, e inclui a grade em `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`. Relatórios montam `cal` em `app/relatorios/furos.py:195`; `app/templates/relatorios/furos.html:42` inclui a grade com `only`, sem passar `pode_veredito`, portanto não desenha o Resolver condicionado por essa permissão em `_calendario_grade.html:125`.

5. **Leitores em testes:** juiz direto em `app/chamados/tests/test_o261_vigia_fio.py:894`, `:900`, `:908`; campo/template em `:920`; costura da porta curta em `:478`, juiz real exercitado em `:491`. Costuras de alcance em `app/chamados/tests/test_o261e_f5_alcance.py:66`, `:214`, `:322`, `:374`. Novos testes integram campo e porta curta em `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:59`, `:76`; testes anteriores leem campo em `app/colaboradores/tests/test_calendario_eh_trabalho_le_a_celula.py:65` e `app/colaboradores/tests/test_calendario_mostra_o_carro.py:142`. Fixtures de apresentação injetam campo em `test_calendario_mostra_o_carro.py:176` e `app/colaboradores/tests/test_ui_pilulas_quebram_linha.py:55`.

6. **Apagamento de `eh_trabalho`: não encontrei leitor executável remanescente dependente da chave.** O predicado lê apenas três pernas em `app/colaboradores/services/calendario.py:103`; o dicionário publicado não contém a chave (`:694` até `:725`). As ocorrências executáveis restantes são a injeção proposital `False`/`None` para garantir independência em `app/chamados/tests/test_o261_vigia_fio.py:910` e as proibições da chave em `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:86` e `app/colaboradores/tests/test_calendario_eh_trabalho_le_a_celula.py:80`. JS/API/JSON não apresentaram referências ao identificador na busca integral [nao verificado quanto a acesso dinâmico por chave construída].

7. **Documentação derivada ficou velha:** `app/docs/MAPA/calendario-dia-da-tela.md:12` ainda descreve o predicado com “dia de trabalho”; `app/docs/BACKLOG.md:1785` contém trecho histórico `dia.eh_trabalho`. Isso não é leitor executável, mas o mapa contradiz o contrato atual de `app/colaboradores/services/calendario.py:103`.

8. **Outro caminho que altera a oferta sem lavra, preexistente ao diff fornecido:** o `try` do último recurso engole qualquer exceção em `app/colaboradores/services/calendario.py:451`. Se falhar a janela, `_ini_comp_atual=None` inicial de `:436` elimina oferta em `:711`; se a janela já respondeu e falhar a carga/julgamento das perguntas, `_perg_viva` pode ficar vazio ou parcial (`:437`, `:450`), permitindo oferta que pergunta viva com resposta deveria impedir (`:712`). A ocorrência real desse erro não consta dos dados [nao verificado].

## 02/11 21:29 · sessao 9f7ef7d7568c · E4CAL-L1-P1 · verificador · origem=Code

**1. O delta fecha o item 3 específico da v1.** A lavra ocorre em T0=12h; o D5 passa às 13h e repete às 14h. Regravar `julgada_em` ou `veredito_em` com o relógio dessas passadas passa a alterar a foto. O mesmo vale para os demais timestamps fotografados, inclusive `FechamentoMensal.atualizado_em`, que é `auto_now`. (`app/ponto/tests/test_e3_status_lavrado.py:31`, `:298`, `:309–331`; `app/ponto/models.py:479`)

Isso é conclusão estática sobre o teste. O resultado do mutante citado pela triagem e sua execução nesta revisão são **[não verificado]**. (`triagem_v1.txt:2`, anexo) [nao verificado]

**Há escapes fora dessa foto:**

- `Colaborador.atualizado_em` é `auto_now`, há colaborador no fixture e essa tabela não entra na foto. Um mutante hipotético que apenas atualizasse esse campo com o relógio escaparia da comparação; ocorrência dessa escrita no D5 atual **[não verificado]**. (`app/colaboradores/models.py:381`; `app/ponto/tests/test_e3_status_lavrado.py:170`, `:309–313`)
- `EventoCelula` também fica fora. Um mutante que inserisse outbox sem alterar os campos fotografados escaparia. No código atual, salvar somente status não emite outbox: apenas `veredito` e `ata` o despertam. (`app/ponto/tests/test_e3_status_lavrado.py:309–313`; `app/ponto/portas/celula.py:623`, `:634–642`)
- `LogAuditoria` fica fora e recebe, legitimamente, uma nova linha no ramo de status. Portanto, a foto não demonstra “nenhuma outra tabela escrita”. (`app/ponto/portas/celula.py:761–765`; `app/core/observ.py:134–138`)

A igualdade das fotos demonstra igualdade do **estado observado**; não detecta um `UPDATE` que conserve o valor nem escrita fora das tabelas/linhas fotografadas. (`app/ponto/tests/test_e3_status_lavrado.py:309–331`)

**2. Os descartes têm alcances diferentes.**

**`veredito_via=NULL`: o defeito estrutural continua existindo.** Os dois gates de atualidade não exigem via; `_completar_status` chama a porta com `via='cartorio'`. Com via nula, falha a igualdade exigida pelo ramo somente status: a porta escreve `veredito_via`, `veredito_em`, veredito/protestos e emite outbox por incluir `veredito` nos campos. (`app/ponto/services/cartorio.py:1186–1189`, `:1345–1348`, `:889–891`; `app/ponto/portas/celula.py:753–775`, `:638–642`)

**Pode deixar de bloquear este apply delimitado, sem considerar o defeito corrigido.** A triagem relata sombra sobre as 35.436 células do universo exato, com zero mudanças fora do status, incluindo comparação de via e timestamp. Isso é evidência relatada no anexo; o conteúdo dos scripts e sua execução são **[não verificado]**. (`triagem_v1.txt:4`, anexo)

Para essa evidência sustentar o descarte, precisaria ser verdade:

- A sombra executou o caminho real de `--apply`, inclusive a porta, sobre o mesmo universo e estado destinados ao pouso. O dry sozinho não serve: ele não chama a lavra. (`app/ponto/services/cartorio.py:888–891`; equivalência da sombra **[não verificado]**)
- `CAMPOS_FORA` comparou cópias independentes dos valores persistidos antes/depois, sem perder células, incluindo efetivamente `veredito_via` e `veredito_em`. A implementação de `logs/sombra/e4cal_r8_d5_entregue.py:37` é **[não verificado]**.
- A prova de produção cobriu o mesmo alcance, os campos proibidos e os hashes exigidos; sua execução e resultado são **[não verificado]**. (`triagem_v1.txt:4`; `AVAL_L1_COMPLEMENTO.md:3`, `:10`, anexos)

**PAREI posterior não equivale a impedir ou desfazer a escrita.** O command não envolve o lote inteiro em uma transação; a porta tem atomicidade por chamada de salvamento. Se a prova apenas detecta a divergência depois, é necessário saber como a reversão prevista é executada. Esse comportamento dos scripts externos é **[não verificado]**. (`app/ponto/management/commands/lavrar_status_competencia.py:32–62`; `app/ponto/portas/celula.py:634–642`; `AVAL_L1_COMPLEMENTO.md:3`, anexo)

Assim: **sombra válida mais prova operacional válida sustentam o apply deste universo, deixando D5-VIA-NULA na fila; somente a menção a “zero” e “PAREI” não permite certificar isso nesta cópia.** (`triagem_v1.txt:4`, anexo; condições externas **[não verificado]**)

**O descarte da auditoria procede.** MUDA 1 restringe a alteração da célula ao status; a trilha é outra entidade e atende à exigência de registrar ação crítica. O ramo grava descrição e antes/depois por `evento`, que insere `LogAuditoria`. Isso não autoriza consumidor/outbox, que continuam excluídos do ramo somente status. (`AVAL_L1_COMPLEMENTO.md:3`, anexo; `CLAUDE.md:32–34`; `app/ponto/portas/celula.py:757–765`; `app/core/observ.py:134–138`)

**3. Escritores e leitores relevantes.**

| Campo/artefato | Quem escreve | Quem lê |
|---|---|---|
| `status_dia`, `status_rotulo` | Porta `lavrar_veredito` → `_aplicar_status` → salvamento. O teste os zera para simular legado. (`app/ponto/portas/celula.py:701–706`, `:753–760`; `app/ponto/tests/test_e3_status_lavrado.py:307`) | D5 verifica status existente; D4 lê status/rótulo; a porta compara o par. (`app/ponto/services/cartorio.py:882`; `app/ponto/status_dia.py:212–214`; `app/ponto/portas/celula.py:703`) |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | Porta de lavra; a precedência também escreve sua chave da ata. (`app/ponto/portas/celula.py:767–775`, `:811–822`) | D5 reutiliza veredito/protestos; a porta lê via/ata para escolher o ramo; `_foto` observa todos. (`app/ponto/services/cartorio.py:889`; `app/ponto/portas/celula.py:726–753`; `app/ponto/tests/test_e3_status_lavrado.py:309–311`) |
| `julgada_em`, `impressao`, `insumos_em` | Operação `carimbar`. (`app/ponto/portas/celula.py:800–807`) | Gates de atualidade e foto. (`app/ponto/services/cartorio.py:1186`, `:1345`; `app/ponto/tests/test_e3_status_lavrado.py:309–311`) |
| FM/ED fotografados | O fixture cria os registros. (`app/ponto/tests/test_e3_status_lavrado.py:299–302`) | `_foto` lê todas as colunas; D5 lê ED indiretamente pelo juiz de entregue. (`app/ponto/tests/test_e3_status_lavrado.py:312–313`; `app/ponto/management/commands/lavrar_status_competencia.py:43`; `app/folha/export.py:569–575`) |

**Não localizei selo que case o texto alterado dos ADRs 0002/0003 ou da docstring D5; ausência absoluta de leitores externos é [não verificado].** O selo da guarda da linha consulta AST, `_meta` e catálogo próprio, explicitamente “nunca texto”; o selo de naturezas do ADR 0003 está declarado como futuro/fila. (`app/escala/tests/test_d3_guarda_da_linha_celula.py:11–19`, `:167–173`; `app/docs/ADR/0003-celula-tres-naturezas.md:125–130`)

Existe leitor genérico de docstrings no MAPA, mas ele exige `@bolha`, ausente na docstring D5. O casamento documental da LEI-AKITA consultado lê `GLOSSARIO.md` e `PLANO-PADROES.md`. Portanto, não encontrei nesses mecanismos dependência das frases modificadas neste delta. (`app/core/mapa/contrato.py:45–48`, `:83–88`; `app/ponto/management/commands/lavrar_status_competencia.py:1–16`; `bin/tests/test_lei_akita.sh:94–108`)

## 02/11 21:27 · sessao 0fd26c7d7b50 · - · - · origem=Ronald (tmux)

1. **O delta fecha o defeito específico de relógio na foto:** lavra em T0=12h, D5 em T1=13h e repetição em T2=14h. Regravar `julgada_em`, `veredito_em`, `insumos_em` ou outro campo fotografado com o relógio desses atos altera o estado comparado. (`app/ponto/tests/test_e3_status_lavrado.py:31`, `:298`, `:309-331`)

2. **`auto_now` de FM está coberto:** `_foto()` inclui `.values()` de todas FM; `FechamentoMensal.atualizado_em` é `auto_now=True`. (`app/ponto/tests/test_e3_status_lavrado.py:312`, `app/ponto/models.py:478-479`)

3. **Existe escape concreto fora da foto:** `Colaborador.atualizado_em` é `auto_now=True`, há colaborador no fixture e `_foto()` não fotografa essa tabela. Um mutante hipotético que regrave esse campo com o relógio do D5 passaria por essa comparação; não localizei tal escrita no caminho atual. (`app/colaboradores/models.py:380-381`, `app/ponto/tests/test_e3_status_lavrado.py:170-173`, `:309-313`; ocorrência real desse mutante [não verificado])

4. **Outras tabelas também ficam fora:** `EventoCelula.criado_em` e `processado_em`, `LogAuditoria.criado_em`, `DiaPago.lavrado_em` (`auto_now=True`). A foto observa somente células desse colaborador, FM e ED. (`app/escala/models.py:1822-1828`, `app/core/models.py:307`, `app/ponto/models.py:380`, `app/ponto/tests/test_e3_status_lavrado.py:309-313`)

5. **Trilha não é escape acidental da regra atual:** o ramo status chama `evento('veredito_celula', ...)`, que insere `LogAuditoria`; sua ausência da foto permite essa escrita prevista. (`app/ponto/portas/celula.py:761-765`, `app/core/observ.py:134-138`)

6. **Outbox mereceria testemunha própria se a afirmação for “não acorda consumidor”:** status somente não dispara evento porque `CAMPOS_QUE_ACORDAM={'veredito','ata'}`; porém mutante que inserisse um `EventoCelula` sem mudar os campos fotografados escaparia dessa foto. (`app/ponto/portas/celula.py:622-642`, `app/ponto/tests/test_e3_status_lavrado.py:309-313`)

7. **O furo `veredito_via=NULL` é estrutural e real:** gates de carimbo não exigem via; `_completar_status()` chama lavra com `via='cartorio'`; comparação da porta exige igualdade da via e, se falha, escreve via/veredito/protestos/`veredito_em`, além de acordar outbox por incluir `veredito`. O fixture normal lavra previamente pela porta e não introduz via nula. (`app/ponto/services/cartorio.py:1186-1189`, `:1345-1348`, `:889-891`, `app/ponto/portas/celula.py:753-775`, `:622-642`, `app/ponto/tests/test_e3_status_lavrado.py:189-194`, `:298-307`)

8. **Limite da prova:** igualdade das fotos prova estado final nos registros observados; não prova ausência de SQL redundante, escrita com valor igual, alteração seguida de restauração ou escrita fora do universo fotografado. (`app/ponto/tests/test_e3_status_lavrado.py:309-331`; execução SQL [não verificado])

## 02/11 21:27 · sessao f68fd42d7647 · - · - · origem=Ronald (tmux)

Para a pergunta 3:

- **ADR 0002/0003 e docstring D5:** não localizei leitor que case o texto alterado por selo; ausência absoluta ou leitores externos **[não verificado]**. O command mantém `help` próprio, sem `help = __doc__` (`app/ponto/management/commands/lavrar_status_competencia.py:24`).

- **Selo existente do ADR 0002:** verifica AST e `_meta`, expressamente “nunca texto”; seu catálogo declara `status_dia`/`status_rotulo` como saídas escritas pela porta (`app/escala/tests/test_d3_guarda_da_linha_celula.py:11`, `:33`, `:64`). `_estado` lê AST de `ponto/portas/celula.py` e `ponto/services/cartorio.py`; o teste compara o resultado ao `CATALOGO`, sem ler ADR Markdown (`app/escala/tests/test_d3_guarda_da_linha_celula.py:75`, `:167`, `:171`).

- **ADR 0003:** o selo de naturezas está declarado como obra futura/fila, inclusive com nome provisório (`app/docs/ADR/0003-celula-tres-naturezas.md:125`, `:127`, `:130`).

- **Existe leitor genérico de docstrings:** MAPA usa `ast.get_docstring` (`app/core/mapa/contrato.py:83`, `:88`), mas devolve `{}` quando falta `@bolha` (`:45`, `:47`). A docstring D5 não contém essa inscrição (`app/ponto/management/commands/lavrar_status_competencia.py:1`, `:16`); o universo obrigatório do MAPA deriva dos registros `core/juizes.py`/`core/portas.py` (`app/core/mapa/contrato.py:157`, `:164`, `:171`, `:180`).

- **LEI-AKITA:** há casamento textual de vocabulário documental, mas lê `GLOSSARIO.md` e `PLANO-PADROES.md`, incluindo os ADRs internos deste último; não os arquivos ADR 0002/0003 (`bin/tests/test_lei_akita.sh:94`, `:102`, `:105`).

- **Teste alterado:** o executor chama D5 via `call_command` (`app/ponto/tests/test_e3_status_lavrado.py:264`, `:266`); a foto lê todas as colunas das células do colaborador, excetuando apenas status, e todas as linhas/colunas de FM/ED (`:309`, `:310`, `:312`). A conferência dos novos horários e fotos ocorre em `:318`, `:327`, `:329`, `:331`; execução efetiva nesta revisão **[não verificado]**.

## 02/11 21:04 · sessao da8569571a0a · E4CAL-L1-P1 · verificador · origem=Code

**Parecer: a garantia irrestrita de “só status” não está fechada.** O caminho normal preserva os demais campos da célula, mas escreve auditoria; uma célula elegível com `veredito_via=NULL` pode alcançar lavra completa; e o teste deixa sobreviver uma regravação de `julgada_em` com o mesmo instante congelado. (`app/ponto/portas/celula.py:753`; `app/core/observ.py:134`; `app/escala/models.py:1739`; `app/ponto/tests/test_e3_status_lavrado.py:286`)

**1) Quem escreve, quem lê e alcance financeiro**

O escritor produtivo dos dois campos é `_aplicar_status`, dentro da porta: atribui em `app/ponto/portas/celula.py:705`, devolve os dois nomes em `:706` e persiste por `salvar_e_emitir`, cujo save real está em `:640`. O método do modelo apenas delega. (`app/escala/models.py:1794`)

As entradas são D5, cartório normal nos dois ramos e decisão administrativa. (`app/ponto/services/cartorio.py:889`; `app/ponto/services/cartorio.py:1229`; `app/ponto/services/cartorio.py:1789`; `app/ponto/services/veredito_celula.py:330`)

| Leitor produtivo | Uso e destino do valor |
|---|---|
| `app/ponto/status_dia.py:212` | Lê ambos e devolve status/rótulo/fonte em `:213–214`, sem guarda de exportada. A interface singular delega ao mesmo corpo em `:179`. |
| `app/core/services/painel_op.py:336` | Consome o D4 e coloca a resposta em `linha['dia']`, em `:338`. Termina no badge/tooltip HTML: `app/templates/core/_painel_card.html:66–67`. |
| `app/ponto/services/cartorio.py:882` | Lê `status_dia` para pular preenchimento já realizado; status ausente segue à porta em `:889`. |
| `app/ponto/portas/celula.py:697` | Preserva ambos quando faltam fatos e o veredito permanece igual; a comparação de idempotência está em `:703`. |
| `app/ponto/portas/celula.py:723` | Copia status para auditoria; descrição e JSON são escritos em `:762–765` e `:777–780`. A consulta administrativa exibe essa trilha. (`app/core/views.py:705`; `app/core/views.py:733`) |

Para esses leitores, não encontrei passagem do **valor das duas colunas** para número de folha, TXT, FM, pauta DP ou emissão de chamado/pergunta/disputa: o consumidor externo encontrado termina em HTML; o D5 retorna antes da fábrica; e a retenção do TXT consulta veredito/protestos/trabalha. (`app/core/services/painel_op.py:338`; `app/templates/core/_painel_card.html:67`; `app/ponto/services/cartorio.py:949`; `app/folha/export.py:1215`; `app/folha/export.py:1228`; `app/folha/export.py:1237`)

O painel atual consulta pares na janela de hoje menos dois dias até hoje; portanto esse caminho não varre as competências 06–09/2026 na data desta revisão. (`app/core/services/painel_op.py:327`)

O calendário ainda produz seu próprio dicionário de status/rótulo; seus templates não estão lendo essas colunas. “Minha escala” também monta rótulo de previsão. Esses homônimos não são leitores adicionais de `CelulaDia.status_rotulo`. (`app/colaboradores/services/calendario.py:895`; `app/colaboradores/services/calendario.py:896`; `app/escala/views.py:54`; `app/escala/views.py:60`)

Fora de produção, há escritas explícitas de preparação de testes e leituras de asserção; a migração apenas adiciona as colunas. (`app/ponto/tests/test_e3_status_lavrado.py:277`; `app/ponto/tests/test_e3_status_lavrado.py:307`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:52`; `app/ponto/tests/test_e4_1_cron_le_o_juiz_nao_a_lavra.py:80`; `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13`)

**TXT de 09:** comparando duas regenerações com todos os demais insumos fixos e somente essas duas colunas alteradas, o conteúdo permanece igual: o regerador usa `montar_linhas_export`, que lê FM para eventos e os juízes existentes para inclusão/faltas. Isso não prova igualdade com o TXT histórico já armazenado, que pode ter divergências anteriores. (`app/folha/management/commands/regerar_txt_dominio.py:85`; `app/folha/export.py:695`; `app/folha/export.py:700`; `app/folha/export.py:711`; `app/folha/export.py:721`) Igualdade real dos TXT de produção: **[nao verificado]**.

**2) O que `completar_status=True` realmente grava**

O caminho verifica impressão/carimbo/veredito nos dois ramos, chama `_completar_status` e continua antes da lavra normal. O invólucro retorna antes de reconciliação, segunda passada e fábrica. (`app/ponto/services/cartorio.py:1186–1189`; `app/ponto/services/cartorio.py:1345–1348`; `app/ponto/services/cartorio.py:948–950`)

Com via válida de cartório e veredito/protestos preservados, a porta salva **somente `status_dia/status_rotulo` na célula**. Não passa por `carimbar`, onde seriam escritos `julgada_em`, impressão e insumos; tampouco inclui campos de edição/regeneração no save. (`app/ponto/portas/celula.py:753–760`; `app/ponto/portas/celula.py:706`; `app/ponto/portas/celula.py:640`; `app/ponto/portas/celula.py:800–807`)

**Há outra escrita:** o ramo chama `evento('veredito_celula', ...)`, que tenta inserir `LogAuditoria`, com tratamento best effort. A trilha possui `criado_em` automático. Na célula decidida pelo admin, o ramo especial salva status e retorna sem essa auditoria. (`app/ponto/portas/celula.py:761–765`; `app/core/observ.py:105`; `app/core/observ.py:133–138`; `app/core/models.py:307`; `app/ponto/portas/celula.py:738–744`)

No ramo normal de status apenas, não nasce `EventoCelula`: somente `veredito` ou `ata` acordam o outbox. A auditoria do command, sem usuário humano, também não satisfaz a exceção de “admin tocou o dia”. (`app/ponto/portas/celula.py:623`; `app/ponto/portas/celula.py:638–642`; `app/core/observ.py:118`; `app/chamados/juizes.py:992–999`)

A operação usa `save`, portanto não fundamenta afirmar “nenhum signal é disparado”. Não localizei receptor de `CelulaDia` na cópia; execução do despacho de signals: **[nao verificado]**. (`app/ponto/portas/celula.py:640`)

**Ressalva estrutural:** os gates não verificam `veredito_via`, e o modelo aceita NULL. Uma célula elegível, com veredito válido de cartório e via nula, falha na igualdade de `:753` e cai em `:769–775`: grava também protestos/veredito/via/`veredito_em` e emite outbox. A existência desse estado no conjunto raspado é **[nao verificado]**. (`app/ponto/services/cartorio.py:1186`; `app/ponto/services/cartorio.py:1345`; `app/escala/models.py:1739`; `app/ponto/portas/celula.py:753`; `app/ponto/portas/celula.py:769–775`)

**3) Mordida do teste** [nao verificado]

- **“Guarda volta”: sim, por inspeção.** A saída antecipada impede as contagens e o preenchimento exigidos no dry/apply. Execução do mutante: **[nao verificado]**. (`app/ponto/tests/test_e3_status_lavrado.py:317`; `app/ponto/tests/test_e3_status_lavrado.py:321`)
- **“Também escreve `julgada_em`”: não necessariamente.** A foto inclui o campo, mas a lavra inicial e o command usam o mesmo `AGORA` congelado. Regravar `julgada_em=agora` ou `timezone.now()` pode manter exatamente o mesmo valor e passar. (`app/ponto/tests/test_e3_status_lavrado.py:189–194`; `app/ponto/tests/test_e3_status_lavrado.py:286`; `app/ponto/management/commands/lavrar_status_competencia.py:44`; `app/ponto/portas/celula.py:800`)
- A foto cobre células **do colaborador do fixture**, excetuando ambos os status, e todas as linhas de FM/ED disponíveis no teste. Não cobre auditoria, outbox, `DiaPago`, chamados, perguntas, disputas nem células de outros colaboradores. (`app/ponto/tests/test_e3_status_lavrado.py:309–313`)
- Não confere os rótulos; só exige status preenchido e os valores específicos de duas células. A última passada verifica contadores, sem nova foto. (`app/ponto/tests/test_e3_status_lavrado.py:310`; `app/ponto/tests/test_e3_status_lavrado.py:321–325`)

Para matar a regravação de carimbo, a lavra inicial precisa ocorrer em T0 e o D5 em T1 distinto, ou a prova precisa inspecionar os campos efetivamente persistidos. A igualdade temporal atual é a brecha. (`app/ponto/tests/test_e3_status_lavrado.py:189`; `app/ponto/tests/test_e3_status_lavrado.py:286`)

**4) Aval e classes reprovadas**

O caminho canônico usa o mesmo juiz e a mesma porta; a competência entregue virou informação; carimbo velho fica na fila; e o relógio funcional é capturado uma vez na borda. Não aparece segundo juiz, segundo escritor produtivo ou derivação nova nesse caminho. (`app/ponto/management/commands/lavrar_status_competencia.py:43–57`; `app/ponto/services/cartorio.py:885–891`; `app/ponto/portas/celula.py:693–698`)

Não há escrita de FM/ED nesse percurso. Há, porém, **leitura indireta de ED** por `competencia_entregue`, contrariando a frase documental de que o command não lê ED. (`app/ponto/management/commands/lavrar_status_competencia.py:13`; `app/ponto/management/commands/lavrar_status_competencia.py:43`; `app/folha/export.py:569–575`)

A auditoria não é explicitamente proibida pelo MUDA 1 fornecido; já a possibilidade de lavra completa/outbox com via nula impede certificar seu contrato para qualquer célula elegível. O selo temporal também não está fechado. (`app/ponto/portas/celula.py:761`; `app/ponto/portas/celula.py:753–775`; `app/ponto/tests/test_e3_status_lavrado.py:286`)

Não foi introduzida ordem explícita nova de locks. As células são carregadas antes e o atomic do save não revalida a impressão; segurança contra alteração concorrente permanece **[nao verificado]**. (`app/ponto/management/commands/lavrar_status_competencia.py:54`; `app/ponto/services/cartorio.py:1345`; `app/ponto/portas/celula.py:639–640`)

Os ADRs ainda descrevem a guarda retirada e precisam de reconciliação documental. (`app/docs/ADR/0002-celula-e-o-dia.md:69–71`; `app/docs/ADR/0003-celula-tres-naturezas.md:113–114`)

Apply das 06–09/2026, reversão prévia, hashes FM/TXT e cumprimento operacional dos pré-requisitos de pouso: **[nao verificado]**. [nao verificado]

**5) Outras guardas e retorno a NULL**

O cartório de lote e o julgamento unitário não têm guarda de exportada nesses caminhos. As guardas restantes identificadas protegem regeneração/reversão de DNA; seus saves não incluem status. (`app/ponto/management/commands/processar_cartorio.py:49–65`; `app/ponto/services/cartorio.py:1950–1967`; `app/ponto/portas/celula.py:238–242`; `app/ponto/portas/celula.py:264–265`; `app/ponto/portas/celula.py:520–539`)

Existe mecanismo capaz de produzir `(None, None)` quando falta `fatos_do_status` e muda o veredito, mas não é guarda de exportada. Os chamadores produtivos encontrados do cartório fornecem os fatos; o admin usa seu mapeamento próprio. (`app/ponto/portas/celula.py:694–698`; `app/ponto/services/cartorio.py:889–891`; `app/ponto/services/cartorio.py:1229–1234`; `app/ponto/services/cartorio.py:1789–1794`)

O D5 também pula qualquer `status_dia` não nulo, mesmo com `status_rotulo=NULL`; o leitor então usa o rótulo do vocabulário. Não encontrei outra guarda de exportada que anule o status. (`app/ponto/services/cartorio.py:882–884`; `app/ponto/status_dia.py:214`) Regressão ou divergência real após apply: **[nao verificado]**.

## 02/11 21:03 · sessao 53f77a91c3e3 · - · - · origem=Ronald (tmux)

Q2 — caminho real e efeitos:

- `handle` lê a competência entregue apenas como informação, captura `agora`/`hoje`, seleciona células até ontem e chama `julgar_colab(... completar_status=True)`; não há trava de exportada nesse caminho (`app/ponto/management/commands/lavrar_status_competencia.py:43`, `:44`, `:49`, `:54`, `:56`).
- O corpo constrói a mesma grade, verifica impressão/carimbo/veredito e, nos dois ramos, chama `_completar_status` seguido de `continue`; o wrapper retorna antes de reconciliação, segunda passada, fábrica e cluster (`app/ponto/services/cartorio.py:1113`, `:1186`, `:1187`, `:1189`, `:1345`, `:1346`, `:1348`, `:949`).
- `_completar_status` pula status existente, conta carimbo velho como fila e, em apply, chama a porta com o veredito/protestos existentes e os fatos da grade (`app/ponto/services/cartorio.py:882`, `:885`, `:888`, `:889`).
- `CelulaDia.lavrar_veredito` só delega à porta (`app/escala/models.py:1787`, `:1794`).
- No ramo canônico `veredito_via='cartorio'`, a porta compara veredito/via/protestos, aplica os dois campos e executa `cel.save(update_fields=['status_dia','status_rotulo'])`; não carimba `julgada_em`, `insumos_em`, edição ou regeneração (`app/ponto/portas/celula.py:703`, `:705`, `:706`, `:753`, `:760`, `:640`). A escrita de `julgada_em` pertence a `carimbar`, que os `continue` anteriores não alcançam (`app/ponto/portas/celula.py:800`, `:805`; `app/ponto/services/cartorio.py:1189`, `:1348`).
- **Há escrita adicional de trilha:** depois do save de status, chama `evento('veredito_celula',...)`, que efetivamente executa `LogAuditoria.objects.create`, em savepoint próprio e best effort (`app/ponto/portas/celula.py:761`, `:762`, `:764`; `app/core/observ.py:105`, `:133`, `:134`). A tabela possui timestamp automático `criado_em` (`app/core/models.py:307`).
- Em célula decidida pelo admin, o ramo especial aplica status e retorna sem essa chamada de auditoria (`app/ponto/portas/celula.py:738`, `:739`, `:742`, `:744`).
- **Não cria outbox nesse ramo:** somente `veredito` ou `ata` acordam `EventoCelula`; o conjunto de campos de status não intersecta esses campos (`app/ponto/portas/celula.py:623`, `:638`, `:640`, `:641`, `:631`). Não localizei receiver de `CelulaDia` no código; dispatch interno do Django não foi inspecionado [nao verificado].

Ressalva estrutural importante:

- A promessa “só status” pressupõe via válida existente (`cartorio` ou `admin`); a elegibilidade do D5 não testa `veredito_via`, enquanto o modelo permite `NULL` (`app/ponto/services/cartorio.py:1186`, `:1345`; `app/escala/models.py:1739`).
- Se existir célula elegível com `veredito_via=NULL` e veredito válido de cartório, a comparação de igualdade falha e a porta escreve também `protestos`, `veredito`, `veredito_via`, `veredito_em`, além de emitir outbox e auditoria (`app/ponto/portas/celula.py:753`, `:769`, `:772`, `:773`, `:775`, `:777`, `:638`). Existência dessa condição no dado fornecido: [nao verificado].
- Não existe `select_for_update` no caminho examinado; as células são carregadas antes e a transação envolve o save, sem revalidar impressão/veredito sob lock (`app/ponto/management/commands/lavrar_status_competencia.py:54`; `app/ponto/portas/celula.py:639`, `:640`). Segurança contra edição concorrente no intervalo: [nao verificado]. O diff não acrescenta nem inverte locks (`diff.txt` fornecido [nao verificado como arquivo local]).

Q5 — outras guardas:

- O cron normal do cartório seleciona todas as células passadas e chama o mesmo julgamento, sem filtro de competência exportada (`app/ponto/management/commands/processar_cartorio.py:49`, `:61`, `:65`).
- A unidade `julgar_celula` filtra futuro, inexistência e turno ainda em andamento; depois chama o mesmo cartório sem guarda exportada (`app/ponto/services/cartorio.py:1950`, `:1952`, `:1958`, `:1962`, `:1965`).
- Há guardas de exportada nas operações de **regenerar DNA** e **reverter regeneração**, respectivamente `app/ponto/portas/celula.py:196`, `:199`, `:238`, `:242` e `:520`, `:525`, `:526`; seus saves não incluem status (`app/ponto/portas/celula.py:264`, `:265`, `:538`, `:539`). Elas não anulam status lavrado.
- A porta possui um ramo que devolve `(None,None)` quando faltam fatos e muda o veredito (`app/ponto/portas/celula.py:694`, `:697`); todos os chamadores de produção encontrados do cartório fornecem fatos (`app/ponto/services/cartorio.py:889`, `:890`, `:1229`, `:1233`, `:1789`, `:1793`), e o admin usa o mapeamento próprio (`app/ponto/services/veredito_celula.py:330`; `app/ponto/portas/celula.py:694`, `:695`).
- Não encontrei guarda “exportada não se toca” aplicada à gravação de `status_dia/status_rotulo` que os faça voltar a nulo; o alcance concreto permanece condicionado à impressão em dia e status ausente (`app/ponto/services/cartorio.py:882`, `:885`, `:1186`, `:1345`). Qualquer divergência real em produção: [nao verificado].

## 02/11 21:02 · sessao 313f69e78b3a · - · - · origem=Ronald (tmux)

Q3 — **guarda volta morde; mutante `julgada_em=agora/now()` pode sobreviver.**

- A guarda antiga faz o command sair antes dos contadores e da lavra; o teste exige contagem de completáveis no dry e todos os julgados com status após apply. Portanto a volta literal da guarda derruba as asserções de `app/ponto/tests/test_e3_status_lavrado.py:317` e `app/ponto/tests/test_e3_status_lavrado.py:321`. Execução do mutante: [nao verificado].
- A comparação inclui `julgada_em`, pois exclui somente `status_dia/status_rotulo` da fotografia. Contudo o teste inteiro congela `AGORA`; `_lavrar()` também recebe esse mesmo instante e o passa ao julgamento. O carimbo inicial grava precisamente esse `agora`. Logo um mutante que volte a salvar `julgada_em=timezone.now()` ou o `agora` do command pode escrever o mesmo valor e passar: `app/ponto/tests/test_e3_status_lavrado.py:286`, `app/ponto/tests/test_e3_status_lavrado.py:189`, `app/ponto/services/cartorio.py:1809`, `app/ponto/portas/celula.py:800`, `app/ponto/management/commands/lavrar_status_competencia.py:44`. Execução desse mutante: [nao verificado].
- A foto abrange todos os campos das células **somente do colaborador do fixture**, exceto os dois status; FM e ED são fotografados integralmente nas tabelas disponíveis ao teste. Não fotografa células de outros colaboradores nem tabelas como `LogAuditoria`, `EventoCelula`, `DiaPago`, chamados, perguntas ou disputas: `app/ponto/tests/test_e3_status_lavrado.py:309`.
- Não valida os valores dos rótulos, que ficam fora da foto; exige status não nulo dos julgados e só confere valores específicos de duas células (`ok` e `folga`): `app/ponto/tests/test_e3_status_lavrado.py:310`, `app/ponto/tests/test_e3_status_lavrado.py:321`.
- A foto é comparada após dry+apply, sem fotografia própria depois do dry. A última passada verifica somente os contadores impressos, sem nova foto ou contagem de auditoria/eventos: `app/ponto/tests/test_e3_status_lavrado.py:315`, `app/ponto/tests/test_e3_status_lavrado.py:324`.
- Correção necessária para a mordida do carimbo: fixture julgado em T0 e command em T1 diferente, preservando a igualdade de todos os campos exceto status; alternativamente inspecionar os campos efetivamente enviados ao save. A necessidade decorre da igualdade temporal atual: `app/ponto/tests/test_e3_status_lavrado.py:189`, `app/ponto/tests/test_e3_status_lavrado.py:286`, `app/ponto/management/commands/lavrar_status_competencia.py:44`.

Q4 — **MUDA 1 é atendida no caminho de status, mas a prova de “nenhuma outra escrita” seria falsa.**

- O command usa o juiz existente de competência e passa `completar_status=True`; não cria juiz do status nem escritor paralelo. O status continua calculado pela regra existente dentro da porta: `app/ponto/management/commands/lavrar_status_competencia.py:43`, `app/ponto/management/commands/lavrar_status_competencia.py:56`, `app/ponto/portas/celula.py:686`, `app/ponto/portas/celula.py:701`.
- O relógio funcional do command é lido uma vez na borda; `hoje` deriva desse instante: `app/ponto/management/commands/lavrar_status_competencia.py:44`.
- Célula com fingerprint velho é contada como fila; os dois ramos desviam para completar status e continuam antes da lavra normal, e o invólucro retorna antes da segunda passada/fábrica: `app/ponto/services/cartorio.py:875`, `app/ponto/services/cartorio.py:1186`, `app/ponto/services/cartorio.py:1345`, `app/ponto/services/cartorio.py:949`.
- O ramo cartório com mesmo veredito/protestos/via salva apenas os dois campos de status. `salvar_e_emitir` desperta outbox somente para `veredito` ou `ata`; portanto esse save não cria `EventoCelula`: `app/ponto/portas/celula.py:753`, `app/ponto/portas/celula.py:760`, `app/ponto/portas/celula.py:623`, `app/ponto/portas/celula.py:638`.
- **Há escrita fora da célula:** esse ramo chama `evento('veredito_celula',...)`, que tenta criar `LogAuditoria` em atomic próprio; o teste novo não a compara. Isso contradiz “nenhuma outra escrita”, mas o MUDA 1 fornecido não proíbe explicitamente auditoria: `app/ponto/portas/celula.py:761`, `app/core/observ.py:94`, `app/core/observ.py:133`, `app/ponto/tests/test_e3_status_lavrado.py:309`.
- Nenhuma nova aquisição/ordem explícita de locks foi introduzida no command; o caminho de save abre atomic para a célula, depois a trilha abre seu atomic próprio: `app/ponto/management/commands/lavrar_status_competencia.py:54`, `app/ponto/portas/celula.py:639`, `app/core/observ.py:133`. Ausência de deadlock em execução concorrente: [nao verificado].
- O fingerprint é verificado sobre células previamente carregadas; o `save` posterior não refaz essa comparação dentro do atomic. A garantia sob alteração concorrente de insumos/veredito não é demonstrada pelo teste serial: `app/ponto/management/commands/lavrar_status_competencia.py:54`, `app/ponto/services/cartorio.py:1345`, `app/ponto/portas/celula.py:639`, `app/ponto/tests/test_e3_status_lavrado.py:319`.
- O passo 1 não migra leitor do calendário: seu próprio escopo executa apenas command e teste anexados; a implementação local do command permanece nessa mesma porta: `app/ponto/management/commands/lavrar_status_competencia.py:32`. Apply, reversão, hashes reais FM/TXT e cumprimento da ordem operacional de pouso: [nao verificado].
- Há documentação desatualizada: ADR 0002 ainda diz que o D5 não toca competência entregue, e ADR 0003 ainda registra a guarda retirada. O complemento está registrado no BACKLOG, mas os dois ADRs conservam a descrição anterior: `app/docs/ADR/0002-celula-e-o-dia.md:69`, `app/docs/ADR/0003-celula-tres-naturezas.md:113`, `app/docs/BACKLOG.md:814`.

## 02/11 21:02 · sessao 1e8d64194bdb · - · - · origem=Ronald (tmux)

Censo produtivo de `CelulaDia.status_dia/status_rotulo`:

| Sítio | Operação e alcance |
|---|---|
| `app/ponto/portas/celula.py:701` | Única atribuição produtiva explícita: compara em `:703`, atribui ambos em `:705`, devolve os dois campos em `:706`. |
| `app/ponto/portas/celula.py:686` | `_status_alvo` lê/preserva ambos quando não há fatos e o veredito permanece igual (`:697`); com fatos chama `status_da_lavra` (`:698`), com decisão humana chama `status_do_admin` (`:694`). |
| `app/ponto/portas/celula.py:709` | `lavrar_veredito` usa esse escritor no ramo admin (`:739`), status apenas (`:754`) e lavra completa (`:773`). Grava via `salvar_e_emitir`, cujo save real é `:640`. |
| `app/ponto/services/cartorio.py:882` | Lê `status_dia` para pular a célula já preenchida; status não decide folha nem cobranças nesse ramo; completa pela porta em `:889`. |
| `app/ponto/status_dia.py:212` | Leitor funcional da palavra e rótulo lavrados: retorna status/rótulo/fonte em `:213-214`; não consulta condição de exportação. |
| `app/ponto/portas/celula.py:723` | Copia status anterior para auditoria; descrição e JSON antes/depois em `:762-765` e `:777-780`. |

Chamadores do escritor: delegação do modelo em `app/escala/models.py:1787-1796`; D5 em `app/ponto/services/cartorio.py:889`; cartório normal em `:1229` e `:1789`; resolução admin em `app/ponto/services/veredito_celula.py:330`. No cartório normal, a porta prepara os campos com `save=False`, e `carimbar` salva junto depois; D5 chama com o padrão `save=True` (`app/ponto/services/cartorio.py:889`, `app/ponto/portas/celula.py:709`, `:787`).

O único consumidor produtivo externo do D4 encontrado é `app/core/services/painel_op.py:325-338`: `_d4_em_lote` chama `status_dos_dias_lavrados` (`:336`) e põe o resultado em `linha['dia']` (`:338`). A fonte de par único apenas delega ao mesmo corpo em `app/ponto/status_dia.py:178-179`. O painel consulta entradas das últimas 24h (`app/core/services/painel_op.py:76-84`), seleciona linhas “encerrado” em `:234-236`, determina o dia do turno e exige seu encerramento em `:327-335`. Portanto, o painel atual não varre 06–09/2026 com a data atual; usa janela de hoje menos dois dias até hoje (`:327`). A palavra lavrada só é desenhada no badge e tooltip em `app/templates/core/_painel_card.html:66-67`. Os contadores e cobertura usam presença, sem `linha['dia']` (`app/core/services/painel_op.py:239-243`, `:281-295`), e os dois consumidores do construtor renderizam HTML (`app/core/views.py:607-617`, `:632`, `:679-681`).

Não confundir homônimos:

- Calendário ainda deriva seu próprio status em `app/colaboradores/services/calendario.py:95-148` e seu próprio `status_rotulo` em `:896-898`; os templates `app/templates/colaboradores/partials/_calendario_grade.html:99-107` leem esse dict, não a coluna. Drawer usa esse calendário em `app/colaboradores/services/drawer.py:120-121`.
- Minha Escala constrói um rótulo de previsão em `app/escala/views.py:54-60`; `app/templates/escala/minha_escala.html:75` e `:84` leem esse dict.
- Furos usa veredito e grade em `app/relatorios/furos.py:45-60`; vigia_fio consulta `veredito`, não essas colunas (`app/chamados/services/vigia_fio.py:360-362`).
- `_status_dia` de espelho é uma função homônima, não acesso à coluna (`app/ponto/services/espelho.py:152`, `:183`).

Alcance para folha/TXT/FM/pauta/chamado/pergunta/disputa/cron emissor: não apareceu caminho produtivo das duas colunas para esses destinos no censo; o caminho D5 termina antes de fábrica/segunda passada (`app/ponto/services/cartorio.py:948-950`), e não produz outbox, porque só `veredito`/`ata` acordam consumidor (`app/ponto/portas/celula.py:622-642`). TXT monta eventos a partir de FM (`app/folha/export.py:695-725`) e retenção lê `veredito`/`protestos`/`trabalha` (`:1215-1238`). Regerador usa essa mesma montagem (`app/folha/management/commands/regerar_txt_dominio.py:85`, `:95-98`). Assim, mantendo os demais insumos fixos, completar apenas essas colunas não altera conteúdo regenerado; igualdade real contra o TXT de produção permanece [nao verificado].

Há efeito lateral real: o ramo cartório de status apenas registra `veredito_celula` (`app/ponto/portas/celula.py:761-765`), e `evento` cria `LogAuditoria` (`app/core/observ.py:133-138`) com ator de processo (`:117`, `:123`; função em `:22-32`) e relógio `criado_em` do modelo (`app/core/models.py:307`). Essa auditoria é exibida em `app/core/views.py:705-738`, `app/templates/core/log_auditoria.html:55-74`. Não aciona a exceção de toque humano da tranca: ela exige usuário não nulo e diferente do sistema (`app/chamados/juizes.py:992-999`), enquanto o comando passa `ator=None` pela chamada em `app/ponto/services/cartorio.py:889` e o gravador resolve usuário em `app/core/observ.py:118`. A célula já decidida pelo admin segue ramo diferente, que grava status e retorna sem esse evento (`app/ponto/portas/celula.py:738-744`).

Nada foi escrito ou executado contra banco; conclusões acima são inspeção estática. Execução de testes/mutantes e resultados de produção: [nao verificado]. [nao verificado]

## 02/11 20:13 · sessao 3d6058ec1077 · DEPLOY-[nome]-ELO-DA-CORRENTE · verificador · origem=Code

**1. Os seis furos não estão integralmente fechados.** Uso a enumeração resumida no contrato fornecido. (`contrato.txt:14`, anexo)

| Furo | Fechamento encontrado | Resultado |
|---|---|---|
| Teto → aviso → restart | No teto, retorna 75 sem reiniciar e nomeia o impedimento. Porém `_espera` soma apenas os sonos, excluindo varreduras e espera pelo flock; não é teto absoluto de tempo decorrido. (`bin/reinicio_sob_admissao.sh:29–39`) | **Parcial** |
| Agendador não repete | Existe repetição, mas perde falhas anteriores e associa a recusa somente por arquivo global/mtime. (`bin/deploy_agendado.sh:73–94`) | **Parcial** |
| Descoberta da corrente sem marca | A descoberta agora chama `adm_marcar` antes do Docker e desmarca depois. (`bin/encadeado.sh:46–53`) | **Fechado no fluxo normal** |
| RAW/híbridos dentro das cascas | Consulta descendentes vivos, mas declara expressamente que o intervalo entre dois exec permanece exposto. (`bin/admissao_cron.sh:18`, `63–68`, `87–92`) | **Aberto** |
| Corrida entre consulta e restart | Marca nova usa a mesma trava; deploy repete a varredura sob exclusão. Isso fecha o nascimento de marcas participantes, mas não a entrada posterior de um híbrido já admitido. (`bin/admissao_cron.sh:116–124`; `bin/reinicio_sob_admissao.sh:29–31`) | **Parcial** |
| Medida “137 no deploy” | Contador específico existe, mas fontes ausentes viram zero e nem todo restart tentado deixa registro. (`bin/rc137_no_restart.sh:26–50`; `bin/reinicio_sob_admissao.sh:43–49`) | **Parcial** |

**2. Sim, há caminho para restart com cron de casca vivo e marca válida.**

Um RAW nasce com `casca=0`; a última varredura ocorre entre seus execs e não encontra descendente Docker; o deploy conserva `.admissao`; o RAW inicia o exec seguinte sem passar novamente pela admissão. A sentinela oferece esse caminho concreto: é RAW e executa quatro comandos sucessivos em `mensageria`. A trava protege a criação da marca, não cada entrada posterior na casca. (`app/config/crons.py:669`; `mensageria/bin/sentinela_diario.sh:14–16`; `bin/admissao_cron.sh:117–124`; `bin/reinicio_sob_admissao.sh:30–43`)

Para um C/ENC com `casca=1`, marca criada antes da aquisição aparece na segunda varredura; um runner novo durante o restart espera antes de marcar e executar. Essa parte do protocolo fecha a corrida usual. (`bin/admissao_cron.sh:90`, `116–124`; `bin/reinicio_sob_admissao.sh:29–31`, `43–48`)

Marca antiga existente, contendo somente a data, é tratada conservadoramente: PID existente significa viva, e ausência de `casca` significa que segura. PID reutilizado pode causar recusa indevida; não autoriza restart. Migração de todos os escritores antigos para o protocolo novo: **[não verificado]**. (`bin/admissao_cron.sh:75–91`)

Há ainda uma entrada fora da porta: quando a admissão falha, o comando principal não roda, mas o vigia chama `docker exec ... cron_quebrado` sem admissão. (`bin/cron_run.sh:18–22`, `44–45`)

**3. Há caminhos de execução perdida e de espera sem limite garantido.**

- **Cron recusado não é reexecutado pelo runner:** vencidos os 900 segundos padrão, retorna 75 sem executar e sem carimbo de fim. O agendador novo repete deploy, não esse cron. (`bin/admissao_cron.sh:27`, `117`; `bin/cron_run.sh:18–25`, `34–38`; `bin/deploy_agendado.sh:90`)
- **Fechamento da admissão sem teto garantido:** `docker restart` não tem timeout e recebe o fd 8 aberto. Por inferência da herança do descritor, um filho sobrevivente pode conservar o flock após morrer o shell. Ocorrência em produção: **[não verificado]**. Saída normal sem filhos sobreviventes libera o descritor; ausência de trap, isoladamente, não prova vazamento. (`bin/reinicio_sob_admissao.sh:20`, `29`, `43–49`)
- **Espera infinita configurável:** com `ADM_PASSO_S=0` e marca impeditiva persistente, `_espera` nunca avança. Mesmo no padrão, o contador exclui o tempo das varreduras e dos flocks. (`bin/reinicio_sob_admissao.sh:17`, `27–39`)
- **Envelope morto com filho vivo:** a validade depende do PID do envelope; a varredura remove a marca se ele morreu, sem procurar seus filhos sobreviventes. O runner tem somente limpeza no `EXIT`, sem protocolo de término dos filhos. (`bin/admissao_cron.sh:75–76`, `99–102`; `bin/cron_run.sh:19–20`)
- **Marca retida após o trabalho:** o placar é escrito antes do vigia, mas a marca só sai no `EXIT`; um Docker de alarme/recuperação preso mantém a marca. (`bin/cron_run.sh:19`, `25`, `45–50`)

O RC do comando, quando o runner chega ao final, é preservado: captura em `RC`, grava e devolve esse valor. Entretanto, erro ao gravar o placar é ignorado; morte do envelope antes da gravação pode deixar a execução sem registro. Na descoberta, 70/75 ficam no CLOG e viram saída 2, sem linha própria no placar. (`bin/cron_run.sh:20–25`, `50`; `bin/encadeado.sh:52–61`)

**4. O repete não garante argv exato nem “pior tentativa”.**

Argumentos usuais, inclusive espaços e argumentos vazios explícitos, são preservados por array → NUL → `mapfile` → array. A exceção é **argv sem argumentos**: o `printf` grava um NUL, que volta como um argumento vazio; portanto a promessa de exatidão universal falha. (`bin/deploy.sh:48`, `278`; `bin/deploy_agendado.sh:87`, `90`)

A recusa é global, sobrescrita diretamente e identificada apenas por mtime em segundos. O loop não exige que a esteira tenha retornado 75; pode consumir recusa de outro deploy ou do mesmo segundo e repetir após falha em outro passo. (`bin/deploy.sh:278`; `bin/deploy_agendado.sh:73`, `81–90`)

**Pode executar entre 06:00 e 07:30:** o horário de agendamento é livre e a repetição não consulta essa proibição. A janela existente no deploy é de autenticação. Isso não cumpre o aval enquanto o instrumento ainda não pousou; cumprimento efetivo do aval em produção: **[não verificado]**. (`bin/deploy_agendado.sh:51–57`, `82–90`; `bin/deploy.sh:162–167`; `contrato.txt:16`, anexo)

`rc` é sobrescrito a cada repetição e só depois alimenta `PIOR`: 75 seguido de sucesso resulta em zero. Entre fatias, guarda o último não zero, sem comparação de gravidade. O próprio selo exige zero após recusa seguida de sucesso. (`bin/deploy_agendado.sh:83`, `91–94`, `103`; `bin/tests/test_deploy_nao_mata_elo.sh:162–165`)

**5. O contador tem mordida prevista, mas também verde sem testemunha.** [nao verificado]

O texto detalhado do placar corresponde ao cálculo `[ini−5, fim+60]`. “No minuto” é inexato: `fim` inclui a espera de subida das cascas. Conta qualquer linha com exit 137 nessa janela, sem filtrar casca ou demonstrar causalidade; não possui limite histórico de 26 horas. O recorte aos restarts registrados está declarado. (`bin/placar_code.sh:460`; `bin/rc137_no_restart.sh:6–8`, `27–48`; `bin/reinicio_sob_admissao.sh:42–48`)

**Fonte ausente devolve zero:** ignora `FileNotFoundError` de ambos os arquivos; sem janelas, nem abre o placar. Linhas inválidas também podem ser descartadas até restar zero. Outros erros de leitura não capturados produzem desconhecido, que o placar pesa como 1. (`bin/rc137_no_restart.sh:26–50`; `bin/placar_code.sh:459–461`)

Há lacuna no escritor: falha do próprio `docker restart` sai antes de registrar; falha no append final é seguida de `exit 0`. Assim, uma tentativa parcialmente destrutiva ou um registro perdido pode escapar da medida. (`bin/reinicio_sob_admissao.sh:43`, `48–49`)

A fixture exige 1/rc 2 para o caso casado e zero para janela distante; o contador entra no TOTAL. Execução integral do selo e dos mutantes: **[não verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:170–178`; `bin/placar_code.sh:537`)

**6. Escritores, leitores e incompatibilidades.**

| Arquivo/campo | Quem escreve/remove | Quem lê |
|---|---|---|
| `crons_em_curso/*` — nome/empresa/PID, `ini,pid,starttime,exe,casca,cmd` | `adm_marcar`, chamado pelo runner e descoberta; limpeza pelo EXIT, chamada explícita ou varredura. (`bin/admissao_cron.sh:113–128`; `bin/cron_run.sh:16–19`; `bin/encadeado.sh:48–50`) | PID do nome, `starttime` e `exe` validam identidade; `casca` decide proteção ou consulta árvore. `ini`, `pid` interno e `cmd` não participam dessa decisão. (`bin/admissao_cron.sh:73–100`) |
| `.admissao` | Runner/descoberta e restart abrem o arquivo; serve de flock, sem campos de conteúdo. (`bin/admissao_cron.sh:116–124`; `bin/reinicio_sob_admissao.sh:20–32`) | Os mesmos participantes disputam a exclusão. (`bin/admissao_cron.sh:117`; `bin/reinicio_sob_admissao.sh:29`) |
| `recusa_cron.pendente` — argv NUL | Deploy grava na recusa; deploy normal concluído remove. (`bin/deploy.sh:278`, `353`) | Agendador consulta existência/mtime e lê argv. (`bin/deploy_agendado.sh:85–93`) |
| `restarts.log` — `ini fim`, epoch | Helper, após subida ou falha da subida. (`bin/reinicio_sob_admissao.sh:46–48`) | Contador temporal. (`bin/rc137_no_restart.sh:27–30`) |
| `placar.jsonl` — exit 70/75 | Runner registra recusa como linha do placar. (`bin/cron_run.sh:16–25`) | Vigia imediato, `crons_quebrados`, pipeline e duração. (`bin/cron_run.sh:44–45`; `app/core/services/crons_quebrados.py:33–47`; `app/core/management/commands/pipeline_placar.py:69–80`; `bin/crons_duracao.py:69–75`) |

**Sim, leitores passam a representar recusa como execução quebrada:** 70/75 entram em `crons_quebrados`; a pauta diz “terminou com erro”; o pipeline incrementa `rodou` e `erros`, embora o comando não tenha rodado. A duração também consome essas linhas sem filtrar exit. (`app/core/services/crons_quebrados.py:16`, `46–47`, `68`; `app/core/management/commands/pipeline_placar.py:73–80`; `bin/crons_duracao.py:69–75`; `bin/cron_run.sh:22`)

O handoff não interpreta o novo formato nem inclui `.admissao` na lista de recursos: consulta somente teste e sombra nesse trecho. (`bin/handoff_sessao.sh:223–230`)

**7. Classes de reprovação.**

- **Execução fora da porta:** confirmada nos execs posteriores dos híbridos e no Docker do vigia após recusa. (`mensageria/bin/sentinela_diario.sh:14–16`; `bin/cron_run.sh:44–45`)
- **Selo sem mordida suficiente:** confirmada para transição do híbrido e corrida antes da aquisição; os casos existentes exercitam híbrido já em exec e chegada após restart iniciado. (`bin/tests/test_deploy_nao_mata_elo.sh:90–98`, `114–126`)
- **Segundo juiz registrado / regra de negócio lendo relógio fora da borda:** **[não verificado]**. O defeito temporal demonstrado aqui é a atribuição da recusa por mtime, sem identidade da tentativa. (`bin/deploy_agendado.sh:81–93`)
- **Inversão de locks:** ciclo concreto **[não verificado]**. A cadeia toma árvore e depois régua; o retry toma régua, mas não foi demonstrado elo inverso que forme deadlock. (`bin/molde_fatia/cadeia.sh:11–14`; `bin/deploy_agendado.sh:90`)
- **Pouso antes dos pré requisitos:** ocorrência efetiva **[não verificado]**. Existe, porém, caminho para repetir deploy sem provar que a recusa pertence àquela esteira. (`bin/deploy_agendado.sh:83–90`)

**8. O selo não demonstra mordida para todos os furos.**

| Furo | Cobertura e mutante/caso faltante |
|---|---|
| Teto → restart | Exercita diretamente o helper. Não executa o deploy real; remover sua chamada em `deploy.sh` não é exercitado pelo selo comportamental. (`bin/tests/test_deploy_nao_mata_elo.sh:35`, `62`, `74`; `bin/deploy.sh:275`) |
| Repetição | Exercita recusa/sucesso e recusa persistente. Não pega perda da pior tentativa; exige o resultado que a mascara. Falta sequência de falhas diferentes entre tentativas/fatias. (`bin/tests/test_deploy_nao_mata_elo.sh:149–168`) |
| Descoberta | Testa espera pela admissão. Não verifica marca protegendo uma descoberta em andamento contra restart, nem sinais/filhos sobreviventes. (`bin/tests/test_deploy_nao_mata_elo.sh:138–146`) |
| RAW/híbrido | Falta híbrido entre dois execs e cobertura efetiva de RAW em `mensageria`/`saas_ui`; o caso positivo usa `saas_core`. (`bin/tests/test_deploy_nao_mata_elo.sh:90–98`, `203–211`) |
| Corrida | **Retirar a segunda varredura** não é provocado pelo caso existente: ele inicia o cron depois que o restart já começou. Falta injetar marca entre primeira varredura e flock. (`bin/reinicio_sob_admissao.sh:27–31`; `bin/tests/test_deploy_nao_mata_elo.sh:114–126`) |
| Medida | Falta mutante “fonte ausente → zero”, erro no registro, restart com falha e retirada da contribuição ao TOTAL. (`bin/tests/test_deploy_nao_mata_elo.sh:170–178`; `bin/placar_code.sh:537`) |

Também faltam marca antiga, expiração dos 900 segundos, sinais, flock herdado e teto medido por tempo decorrido. Sobrevivência efetivamente executada desses mutantes: **[não verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:102–146`)

O selo está ligado à régua pelo glob; o placar continua chamando o contrato textual. (`bin/regua.sh:138–145`; `bin/placar_code.sh:130–132`)

**Veredito final: BLOQUEIA.**

- Híbrido válido ainda atravessa o restart; vigia executa Docker fora da admissão. (`bin/admissao_cron.sh:18`; `bin/cron_run.sh:45`)
- Teto não absoluto, restart sem timeout e crons descartados ao vencer a admissão. (`bin/reinicio_sob_admissao.sh:27–43`; `bin/admissao_cron.sh:117`)
- Repetição sem identidade da tentativa, perda do pior rc e ausência da guarda temporária de horário. (`bin/deploy_agendado.sh:81–94`; `contrato.txt:16`, anexo)
- Contador pode declarar zero sem fonte ou sem registro do restart; consumidores confundem recusa com execução quebrada. (`bin/rc137_no_restart.sh:31–50`; `bin/reinicio_sob_admissao.sh:43`; `app/core/management/commands/pipeline_placar.py:73–79`)
- Selo não cobre as interleavings e falhas acima. (`bin/tests/test_deploy_nao_mata_elo.sh:90–178`)

## 02/11 20:11 · sessao 5b5ae1ad4b7e · - · - · origem=Ronald (tmux)

- **argv usual preservado**, por array → NUL → `mapfile` → expansão de array. **argv vazio não é exato**: `printf '%s\0'` sem operandos grava um NUL, lido como um argumento vazio. Confirmei essa semântica somente em memória/pipe. (`bin/deploy.sh:48`; `bin/deploy.sh:278`; `bin/deploy_agendado.sh:87`; `bin/deploy_agendado.sh:90`)

- **Pode executar entre 06:00 e 07:30**: `agendar` aceita horário sem essa restrição; `rodar` executa esteira e repetição sem conferência de janela. A guarda de `deploy.sh` é de autenticação e não implementa o aval temporário. (`bin/deploy_agendado.sh:51`; `bin/deploy_agendado.sh:55`; `bin/deploy_agendado.sh:82`; `bin/deploy_agendado.sh:90`; `bin/deploy.sh:162`; aval: `avais.txt`, anexo, linha não fornecida)

- **`PIOR` não representa a pior tentativa**: o `rc` inicial é sobrescrito por cada repetição; a agregação ocorre somente depois. Uma recusa seguida de sucesso resulta em zero — comportamento explicitamente esperado pelo selo. Entre fatias, `PIOR` guarda o último resultado não zero, sem comparação de gravidade. (`bin/deploy_agendado.sh:83`; `bin/deploy_agendado.sh:91`; `bin/deploy_agendado.sh:94`; `bin/deploy_agendado.sh:103`; `bin/tests/test_deploy_nao_mata_elo.sh:162`)

- **Recusa sem identidade de tentativa/fatia**: é um arquivo global, associado apenas por `mtime` em segundos. A repetição não exige `rc=75`; pode consumir recusa de outro deploy ou uma recusa anterior criada no mesmo segundo. Logo, existe caminho lógico para repetir deploy após falha da esteira em outro passo. (`bin/deploy_agendado.sh:73`; `bin/deploy_agendado.sh:81`; `bin/deploy_agendado.sh:83`; `bin/deploy_agendado.sh:85`)

- **Admissão pode permanecer fechada sem teto**: `docker restart` não usa `timeout` e herda o fd 8; o script não fecha esse fd antes dos filhos. Por inferência, cliente pendurado mantém a trava; se o shell morrer e esse filho sobreviver, o filho continua segurando-a. Novos crons esgotam os 900 segundos e saem 75 sem executar. (`bin/reinicio_sob_admissao.sh:20`; `bin/reinicio_sob_admissao.sh:29`; `bin/reinicio_sob_admissao.sh:43`; `bin/admissao_cron.sh:27`; `bin/admissao_cron.sh:117`; `bin/cron_run.sh:17`)

- **Teto de espera não é absoluto no relógio**: `_espera` acumula somente os sonos, omitindo varreduras e cada espera de `flock -w "$TETO"`. Com `ADM_PASSO_S=0`, `_espera` não avança e cron vivo impede o término indefinidamente. (`bin/reinicio_sob_admissao.sh:17`; `bin/reinicio_sob_admissao.sh:27`; `bin/reinicio_sob_admissao.sh:29`; `bin/reinicio_sob_admissao.sh:34`; `bin/reinicio_sob_admissao.sh:39`)

- **Não há ciclo de locks demonstrado**: retry toma somente `regua.lock`; a cadeia toma árvore e depois régua. Não encontrei aquisição de árvore pelo deploy enquanto segura régua; portanto, deadlock por inversão e segundo escritor concorrente confirmado são **[não verificado]**. (`bin/deploy_agendado.sh:74`; `bin/deploy_agendado.sh:90`; `bin/molde_fatia/cadeia.sh:11`; `bin/molde_fatia/cadeia.sh:14`; `bin/trava.sh:64`)

- **Pouso efetivo antes de pré requisito: [não verificado]**. A cópia não possui histórico Git consultável; execução de produção e cumprimento do aval não foram demonstrados pelos anexos. **[não verificado]** [nao verificado]

## 02/11 20:11 · sessao 69891d8d3687 · - · - · origem=Ronald (tmux)

Achados para os itens 5 e 8:

- **Fonte ausente vira zero silencioso:** confirmei, em execução somente de leitura, que ambas as fontes inexistentes produzem `rc137_no_restart=0`, rc 0. `FileNotFoundError` é ignorado; sem janelas, nem tenta abrir o placar. (`bin/rc137_no_restart.sh:26–36`, `:49–55`)
- **Outros erros de leitura não viram zero:** não são capturados pelo Python; saída vazia vira `?` e pesa 1 no placar. JSON inválido e `fim` inválido são descartados silenciosamente. (`bin/rc137_no_restart.sh:37–46`; `bin/placar_code.sh:459–461`)
- **Rótulo:** a explicação detalhada do placar corresponde ao cálculo `[ini−5, fim+60]`; “no minuto” é abreviação inexata, porque `fim` vem depois das provas de subida. Não filtra cron de casca, apenas qualquer registro com exit 137; é correlação temporal, sem demonstração de causalidade. Universo histórico explicitamente limitado aos restarts registrados. (`bin/rc137_no_restart.sh:6–8`, `:29–48`; `bin/reinicio_sob_admissao.sh:42–48`; `bin/placar_code.sh:460`)
- **Buracos do registro:** falha de `docker restart` sai sem registrar janela; falha ao gravar a janela final é seguida de `exit 0`. Uma tentativa parcialmente destrutiva ou registro perdido pode escapar do contador. (`bin/reinicio_sob_admissao.sh:43`, `:48–49`)
- **Mordida positiva existe como fixture:** registro 137 casado deve contar 1/rc 2; janela distante deve contar 0. Não há caso de fonte ausente/corrompida ou de restart com erro. Execução do selo/mutantes: **[não verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:170–178`)

Cobertura dos seis furos resumidos no contrato:

| Furo | Caso existente | Falta relevante |
|---|---|---|
| Teto → restart | Recusa/nome/nenhum restart no teto; espera término. (`bin/tests/test_deploy_nao_mata_elo.sh:59–79`) | O selo chama o helper diretamente; não copia/executa o deploy real. Mutante que desliga a chamada do helper no deploy não é exercitado. (`:35`, `:62`, `:74`; `bin/deploy.sh:275`) |
| Agendador não repete | Recusa seguida de sucesso; recusa persistente. (`:149–168`) | Expectativa explícita de rc 0 após uma recusa impede demonstrar “pior tentativa”; sem sequência de erros diferentes entre fatias. (`:162–168`) |
| Descoberta sem marca | Retém `.admissao` e verifica horário do `docker exec`. (`:138–146`) | Sinais, filhos sobreviventes e erro durante descoberta não são exercitados. (`:138–146`) |
| RAW/híbrido | Host puro e híbrido já dentro do exec. (`:83–98`) | Intervalo entre dois exec não é testado; tampouco RAW em `mensageria`/`saas_ui`. O censo positivo só renderiza C/saas_core e ENC/saas_core. (`:90–98`, `:203–211`) |
| Corrida | Cron chega depois de `docker restart` já ter sido chamado. (`:114–126`) | **Mutante retirar a segunda varredura** não é provocado: não injeta marca entre primeira varredura e aquisição da trava. (`bin/reinicio_sob_admissao.sh:27–31`; teste `:116–117`) |
| Medida 137 | Fixture positivo/negativo. (`:171–178`) | Ausência de fontes, erro do writer, restart com falha, integração do contador no TOTAL e universo de crons não são exercitados. (`:171–178`; `bin/placar_code.sh:537`) |

Também faltam casos de marca antiga, expiração de `ADM_ESPERA_ADMISSAO_S`, sinais, descritor herdado e teto de tempo efetivamente decorrido; as marcas do teste de órfãs são todas do formato novo e o teste de chegada tardia espera só três segundos. (`bin/tests/test_deploy_nao_mata_elo.sh:102–109`, `:114–126`; `bin/admissao_cron.sh:27`, `:77–91`)

O selo está conectado à régua pelo glob, que bloqueia quando falha; porém o atalho `JA VERDE` precede esse loop. O pre-push declara que não executa o loop de selos de host. O placar chama o contrato textual, sem executar este selo comportamental. (`bin/regua.sh:84–94`, `:138–145`; `bin/pre-push.sh:13–16`; `bin/placar_code.sh:130–132`)

O selo usa placar local ou fallback absoluto de produção para conferir duração; o dado fornecido não comprova os max/p95 declarados nos comentários. (`bin/tests/test_deploy_nao_mata_elo.sh:181–197`; `bin/admissao_cron.sh:20–23`)

## 02/11 20:11 · sessao eec33bfe3a9d · - · - · origem=Ronald (tmux)

- **Marcas `logs/crons_em_curso/*`:** `adm_marcar` escreve nome/empresa/PID no caminho e `ini,pid,starttime,exe,casca,cmd` no conteúdo, com temporário oculto e `mv`; é chamado pelo runner e pela descoberta da corrente. (`bin/admissao_cron.sh:113`; `bin/admissao_cron.sh:120`; `bin/cron_run.sh:16`; `bin/encadeado.sh:48`)
- **Leitores das marcas:** `adm_marca_viva` lê PID do nome, `starttime` e `exe`; `adm_segura` lê `casca` e, se zero, consulta descendentes vivos; `adm_varrer` remove órfãs. `ini`, `pid` do conteúdo e `cmd` são testemunhas sem uso decisório nesse leitor. (`bin/admissao_cron.sh:73`; `bin/admissao_cron.sh:87`; `bin/admissao_cron.sh:95`)
- **Quem apaga:** runner pelo `trap EXIT`; descoberta por chamada explícita após Docker; varredura apaga órfãs. (`bin/cron_run.sh:19`; `bin/encadeado.sh:50`; `bin/admissao_cron.sh:102`; `bin/admissao_cron.sh:128`)
- **`.admissao`:** runner/descoberta abrem fd8 e tomam exclusão antes de escrever a marca, fechando fd8 antes de rodar o comando. Restart abre o mesmo arquivo, toma exclusão, repete varredura e conserva fd8 durante Docker/subida. (`bin/admissao_cron.sh:116`; `bin/admissao_cron.sh:117`; `bin/admissao_cron.sh:124`; `bin/reinicio_sob_admissao.sh:20`; `bin/reinicio_sob_admissao.sh:29`; `bin/reinicio_sob_admissao.sh:30`; `bin/reinicio_sob_admissao.sh:43`)
- **`recusa_cron.pendente`:** deploy grava argv separado por NUL quando recebe 75; agendador lê por `mapfile -d ''`, consulta mtime e repete; deploy normal que chega ao final remove. (`bin/deploy.sh:278`; `bin/deploy_agendado.sh:85`; `bin/deploy_agendado.sh:87`; `bin/deploy_agendado.sh:90`; `bin/deploy.sh:353`)
- **`restarts.log`:** restart grava `ini fim` depois da subida ou de falha da subida; contador lê ambos como epoch. Falha do próprio `docker restart` sai antes da gravação. (`bin/reinicio_sob_admissao.sh:43`; `bin/reinicio_sob_admissao.sh:46`; `bin/reinicio_sob_admissao.sh:48`; `bin/rc137_no_restart.sh:27`)
- **`placar.jsonl`:** runner registra `RC` do comando ou 70/75 da admissão; RC do comando permanece em variável e é devolvido ao final. Falha ao anexar JSON não impede execução dos passos seguintes nem altera `RC`; não há verificação da gravação. (`bin/cron_run.sh:16`; `bin/cron_run.sh:20`; `bin/cron_run.sh:25`; `bin/cron_run.sh:50`)

**Consumidores incompatíveis com “não rodou”:** [nao verificado]

- 70/75 viram “cron quebrado” no vigia imediato e no serviço da última corrida, apesar de o comando não ter rodado. O serviço só exclui 0/2 e o texto da pauta diz “terminou com erro”. (`bin/cron_run.sh:18`; `bin/cron_run.sh:22`; `bin/cron_run.sh:44`; `app/core/services/crons_quebrados.py:16`; `app/core/services/crons_quebrados.py:46`; `app/core/services/crons_quebrados.py:68`)
- `pipeline_placar` conta ambos como `rodou += 1` e como erro; isso passa a contabilizar tentativa recusada como execução. (`app/core/management/commands/pipeline_placar.py:73`; `app/core/management/commands/pipeline_placar.py:74`; `app/core/management/commands/pipeline_placar.py:78`)
- `crons_duracao.py` também recebe duração da recusa: não filtra `exit`, e usa `dur` das linhas no horário. (`bin/crons_duracao.py:69`; `bin/crons_duracao.py:71`; `bin/crons_duracao.py:75`)
- Handoff não acompanha a admissão: sua relação de travas consultadas contém somente teste e sombra; não passa a interpretar o novo formato das marcas. (`bin/handoff_sessao.sh:223`; `bin/handoff_sessao.sh:224`; `bin/handoff_sessao.sh:230`)

**Segundo caminho Docker fora da porta:** depois de admissão recusada, o runner chama `docker exec ... cron_quebrado` mesmo sem marca; o comando principal fica bloqueado, mas o alarme entra na casca durante admissão fechada. O Docker do vigia tampouco tem timeout. (`bin/cron_run.sh:18`; `bin/cron_run.sh:22`; `bin/cron_run.sh:44`; `bin/cron_run.sh:45`)

**Prisão/atraso:** a marca permanece até terminar o vigia, porque limpeza está no EXIT; assim, Docker do alarme/recuperação preso mantém marca após o RC principal já estar no placar. (`bin/cron_run.sh:19`; `bin/cron_run.sh:25`; `bin/cron_run.sh:45`; `bin/cron_run.sh:47`; `bin/cron_run.sh:50`)

**Herança de trava:** fd8 fica aberto nos subprocessos do restart, sem fechamento nos filhos; `docker restart` não tem timeout. Um Docker que demore além de `ADM_ESPERA_ADMISSAO_S` faz o cron sair 75 sem execução. A possibilidade de filho sobrevivente reter o flock é inferência da herança de descritor; ensaio de sinais: **[não verificado]**. (`bin/reinicio_sob_admissao.sh:20`; `bin/reinicio_sob_admissao.sh:43`; `bin/reinicio_sob_admissao.sh:45`; `bin/admissao_cron.sh:26`; `bin/admissao_cron.sh:117`; `bin/cron_run.sh:18`)

## 02/11 19:35 · sessao 629e02d70665 · E4-CALENDARIO-L1 · censo · origem=Code

Uso a numeração dos anexos transcritos, incluindo linhas vazias; em `triagem.txt`, preservo os números fornecidos. Desdobro dois achados compostos para distinguir falha de revisão de ausência de evidência.

A cópia local conserva trechos anteriores ao delta; portanto, ela não confirma integralmente o estado final descrito pelo opus. (`app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/colaboradores/services/calendario.py:619`; `app/core/juizes.py:839`)

1. **BLOQUEIA — D5 nas competências entregues: (e), NÃO PERGUNTADO no requisito necessário.**

   **Pergunta:** “pouso antes do pre-requisito”. (`v1_pergunta.txt:10`)   [nao verificado]
   **Resposta:** “sequência efetiva de pouso/apply: **[nao verificado]**”. (`v1_resposta.txt:46`) [nao verificado]

   O tema genérico estava presente, mas faltou o requisito específico: retirar a guarda e completar 06–09 antes do leitor. A triagem admite literalmente: “a pergunta ao Codex nao anexou AVAL_L1_COMPLEMENTO.md”. Isso sustenta **(e)**; não transforma a guarda visível em questão exclusivamente de produção. (`triagem.txt:5`; `AVAL_L1_COMPLEMENTO.md:3`; `AVAL_L1_COMPLEMENTO.md:9`; `app/ponto/management/commands/lavrar_status_competencia.py:40`)

   **Deveria levar:** COMPLEMENTO integral e cobrança explícita do PRONTO(1), separando comprovação pelo código da comprovação pelos dados. (`AVAL_L1_COMPLEMENTO.md:10`)

2. **BLOQUEIA — rodapé sem cards `sem_certidao`/`sem_dados`: (e), NÃO PERGUNTADO na exigência específica.**

   **Pergunta:** “ainda deriva status ou rotulo por regra propria”. (`v1_pergunta.txt:8`)  
   **Resposta:** “rótulo próprio no calendário — bloqueia”. (`v1_resposta.txt:17`)

   Revisar derivação textual não explicita a obrigação de representar essas duas contagens. A pergunta não identifica o REEQ entre seus anexos; a triagem identifica nele a exigência de rodapé. O conteúdo integral desse aval não foi fornecido aqui. (`v1_pergunta.txt:1`; `triagem.txt:62`; exigência literal completa: **[nao verificado]**)

   **Deveria levar:** REEQ integral e lista dos status que o rodapé deve representar, incluindo os dois ausentes. (`triagem.txt:5`; `triagem.txt:62`)

3. **BLOQUEIA — “Sem registro” sobre `resumo.faltas`: (a), tema presente e achado perdido.**

   **Pergunta:** “Algum leitor […] ainda deriva status ou rotulo por regra propria”. (`v1_pergunta.txt:8`)  
   **Resposta:** “o rodapé afirma que todo `aberto` ‘não teve saída’”. (`v1_resposta.txt:20`)

   O Codex examinou a fidelidade dos textos do rodapé, mas não apontou esse outro rótulo. A associação entre “Sem registro”, ausência de batidas e `resumo.faltas` está diretamente visível no template. (`app/templates/colaboradores/partials/_calendario_grade.html:145`; `app/templates/colaboradores/partials/_calendario_grade.html:146`; `app/templates/colaboradores/partials/_calendario_grade.html:147`; `opus_final.txt:2`)

   **Deveria levar:** conferir cada título e tooltip contra o predicado real do respectivo contador, inclusive falta lavrada ou decidida. (`opus_final.txt:2`)

4. **BLOQUEIA — publicação de `eh_trabalho`: (e), faltou a inclusão explícita na L1.**

   **Pergunta:** “passa a LER status e rotulo do dia”. (`v1_pergunta.txt:2`)  
   **Resposta:** “Ausente no classificador removido do calendário”. (`v1_resposta.txt:40`)

   O objeto declarado era status/rótulo. O BACKLOG disponível já registrava `eh_trabalho`, mas como fila da E4-CALENDARIO; a inclusão específica na L1 depende do DOMINIO MUDA 4 mencionado pelo opus, cujo texto integral não foi fornecido. (`app/docs/BACKLOG.md:827`; `opus_final.txt:2`)

   **Deveria levar:** DOMINIO MUDA 4 e pergunta explícita sobre quem publica `eh_trabalho` para o resolver e o vigia. (`opus_final.txt:2`; `triagem.txt:5`)

5. **FILA B — paridade mensal/unitária e `carga=`: (b), visto e encaminhado à fila.**

   **Pergunta:** “foi a fila como obra propria do pareador”. (`v2_pergunta.txt:1`)  
   **Resposta:** “fila; triagem procede pelos anexos, com ressalva”. (`v2_resposta.txt:34`)

   O Codex identificou o contraexemplo e depois aceitou a preexistência com ressalva. O motivo **procede para a divergência já existente**, mas não comprova a inocuidade do novo contrato com `carga=`: a própria resposta registra resultados de status diferentes entre base e atual. (`v1_resposta.txt:60`; `v2_resposta.txt:34`; `v2_resposta.txt:36`; `opus_final.txt:2`)

   **Linha da triagem que decidiu o encaminhamento:** **[nao verificado]**; essa decisão aparece na segunda pergunta, não nas linhas de triagem fornecidas.

   **Deveria levar:** separar preexistência do pareador, responsabilidade do D4 pela janela recebida e registro efetivo da fila no BACKLOG. (`v2_resposta.txt:36`; `opus_final.txt:2`)

6. **FILA C — constantes nos cards `escala_suspeita`/Abertos: (b), visto; rebaixamento com documentação incompleta.**

   **Pergunta:** “QUEM ESCREVE e QUEM LE […] ROTULO_EM_ANDAMENTO e o texto do card”. (`v2_pergunta.txt:2`)  
   **Resposta:** “Card `escala_suspeita` — bloqueia”; “fila para duplicação textual”. (`v2_resposta.txt:5`; `v2_resposta.txt:7`)

   Portanto, o Codex **viu os dois problemas**. O envio do bloqueio à fila consta do relato inicial do pedido; a linha da triagem e seu motivo específico são **[nao verificado]**. Uma justificativa baseada em precisar tocar a lavra não sustentaria a duplicação dos cards: a resposta identifica o próprio template como escritor e registra a constante central existente. (`v2_resposta.txt:15`; `v2_resposta.txt:16`)

   **Deveria levar:** decisão literal da triagem, distinguindo corrigir o vocabulário central de substituir cópias textuais nos leitores. (`v2_resposta.txt:5`; `v2_resposta.txt:7`)

7. **FILA A2 — propagação derrubando consumidores inteiros: (a), tema presente; alcance não examinado.**

   **Pergunta:** menciona “furos, vigia_fio” e cobra o teste de propagação da A2. (`v1_pergunta.txt:8`; `v2_pergunta.txt:2`)   [nao verificado]
   **Resposta:** “Retirar apenas o `raise` atual deixa de satisfazer esse contrato”. (`v2_resposta.txt:24`)

   O Codex verificou que o teste exige propagação, mas não discutiu sua consequência nos consumidores coletivos. O relatório de furos chama o calendário dentro do processamento dos colaboradores. (`v2_resposta.txt:24`; `app/relatorios/furos.py:187`; `app/relatorios/furos.py:195`; `opus_final.txt:2`)

   **Deveria levar:** rastrear uma exceção A2 pelos chamadores e explicitar qual unidade de processamento ela aborta. (`opus_final.txt:2`)

8. **FILA A2 — frequência e impacto real em produção: (c), fora do alcance evidenciado.**

   **Pergunta:** cobra se o teste “morde […] o defeito”. (`v2_pergunta.txt:2`)   [nao verificado]
   **Resposta:** “exige a exceção injetada e o log específico”. (`v2_resposta.txt:24`)

   Isso demonstra contrato de teste, não incidência operacional. O opus declara que não verificou logs de produção; os dados fornecidos não quantificam falhas A2. (`opus_final.txt:2`; frequência e impacto observado: **[nao verificado]**) [nao verificado]

   **Deveria levar:** medição raspada de falhas A2 e execuções afetadas; sem ela, manter o impacto observado como não verificado.

9. **FILA — pendente TT6 obsoleto: (a), tema documental presente.**

   **Pergunta:** anexa JUIZES e pede avaliação de baixa de pendente em `core/juizes.py`. (`v1_pergunta.txt:1`; `v1_pergunta.txt:12`)  
   **Resposta:** “A impressão removida pertencia ao classificador apagado”. (`v1_resposta.txt:58`)

   A resposta tratou o pendente nomeado, mas não identificou a incoerência residual do TT6 descrita pelo opus. A reancoragem final não é confirmada pela cópia local, que ainda conserva a âncora antiga. (`opus_final.txt:2`; `app/core/juizes.py:839`; `app/core/juizes.py:840`)

   **Deveria levar:** conferir todos os pendentes alterados ou tornados obsoletos pela migração ao D4, comparando âncora e motivo. (`opus_final.txt:2`)

10. **FILA — comentário antigo no `_dd`: (a), tema A2 presente.**

    **Pergunta:** cobra a propagação de falha da A2. (`v2_pergunta.txt:2`)  
    **Resposta:** “Retirar apenas o `raise` atual deixa de satisfazer esse contrato”. (`v2_resposta.txt:24`)

    O Codex reconheceu o novo fluxo de exceção, mas não apontou o comentário que descrevia continuidade após falha. Esse comentário existe na cópia; sua obsolescência na árvore final é sustentada pelo anexo do opus. (`app/colaboradores/services/calendario.py:869`; `app/colaboradores/services/calendario.py:870`; `opus_final.txt:2`)

    **Deveria levar:** revisar comentários que descrevem caminhos eliminados pela propagação obrigatória da A2. (`opus_final.txt:2`)

**Contagem das 10 unidades acima:** **(a) 4; (b) 2; (c) 1; (e) 3; (d) 0.** Os dois casos **(b)** têm evidência de que o Codex viu o problema; as linhas específicas das decisões históricas da triagem permanecem **[nao verificado]**. (`v2_resposta.txt:5`; `v2_resposta.txt:7`; `v2_resposta.txt:34`)

## 02/11 19:33 · sessao 9ae37b8be3a2 · - · - · origem=Ronald (tmux)

A cópia local não reproduz os trechos pós-delta descritos nos anexos: tooltip antigo permanece no template, A2 não propaga exceção, e TT6 conserva a âncora antiga. (`app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/colaboradores/services/calendario.py:619`; `app/core/juizes.py:839`)

- **Rodapé:** não contém cards `sem_certidao`/`sem_dados`; `resumo.faltas` aparece entre tooltip “Dia previsto de trabalho sem nenhuma batida registrada.” e título “Sem registro”. (`app/templates/colaboradores/partials/_calendario_grade.html:138`; `:145`; `:146`; `:147`; `:173`)
- **Textos próprios:** card `escala_suspeita` literal em `:161`; card Abertos ainda usa “Turno começou e não teve saída registrada.” em `:169`. (`app/templates/colaboradores/partials/_calendario_grade.html:161`; `:169`)
- **`eh_trabalho`:** calendário calcula `_fora`, chama `ec.eh_dia_trabalho` e usa resultado no predicado que oferece resolução. (`app/colaboradores/services/calendario.py:777`; `:780`; `:197`)
- **A2:** cópia local captura e registra erro sem `raise`; não confirma o comportamento pós-delta. Furos chama calendário e documenta execução para aproximadamente 750 colaboradores. (`app/colaboradores/services/calendario.py:619`; `:620`; `app/relatorios/furos.py:187`; `:195`)
- **Comentário `_dd`:** localmente descreve fallback ainda presente; inexequibilidade na árvore final depende dos anexos. (`app/colaboradores/services/calendario.py:619`; `:869`; `:871`; `:872`)
- **TT6:** localmente continua ancorado em `def status_do_dia`, motivo “deveria ler idem”; reancoragem final não confirmada pela cópia. (`app/core/juizes.py:839`; `:840`)

Identidade da cópia com `15d4f10b`: **[nao verificado]**.

_1490 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
