# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 23:24** · publicado em **16/11/2026 23:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 23:24 · sessao ee701c2b51bc · - · - · origem=Ronald (tmux)

Achado indireto em Python:

| Saída | Quem vê | Evidência | Pílula? |
|---|---|---|---|
| Formulário de detalhe/edição do `TipoEscala` no Django admin: campo `tipo_base`, com seleção do valor gravado e rótulos das choices | Admin com acesso ao modelo | Registro com `ModelAdmin` sem exclusão/customização de campos: `app/escala/admin.py:7`; campo e choices: `app/escala/models.py:163`, `app/escala/models.py:170`; comentário confirma ModelForm do admin: `app/escala/services/cadastro_tipo.py:383`; rota: `app/config/urls.py:24` | Outra coisa: campo de formulário |

As choices são “12x36 — vigilância padrão”, “Turno partido — dois períodos/dia”, “Comercial — horário fixo semanal”, “Plantão — 24x48 ou similar” e “Intermitente-horista — apenas horas cumpridas (par E/S)” (`app/escala/models.py:164`).

Caso limítrofe: mensagens de validação dizem “tipo base 'Comercial'” quando a base é `comercial` (`app/escala/models.py:358`, `app/escala/models.py:361`, `app/escala/models.py:366`). A edição preserva a base (`app/escala/services/cadastro_tipo.py:179`), chama `save()` (`app/escala/services/cadastro_tipo.py:307`), que executa `full_clean()` (`app/escala/models.py:798`); a view entrega `e.messages` ao usuário (`app/escala/views_wizard.py:110`) por toast/messages (`app/core/respostas.py:63`, `app/core/respostas.py:68`). É aviso condicional de validação, não identificador do tipo nem pílula (`app/escala/models.py:360`).

Saídas verificadas que **não apresentam `tipo_base`**:

- API `/me`: `nome_canonico()` e `tipo_ciclo` (`app/api/views_core.py:254`); API de escala: `template_nome`/`descricao` e display de `tipo_ciclo` (`app/api/views.py:948`).
- PDF do espelho: código, `tipo_ciclo` e horários (`app/relatorios/pdf_espelho.py:730`).
- Builder de relatórios, coluna Escala: `str(ec.tipo_escala)` (`app/relatorios/views.py:1317`, `app/relatorios/views.py:1335`); `__str__` retorna nome canônico ou código/descrição (`app/escala/models.py:801`), e nome canônico usa `tipo_ciclo_canonico()` (`app/escala/models.py:424`).
- Tag `rotulo_escala`: parte `tipo` vem de `nome_canonico_partes()` (`app/core/templatetags/hasner_filters.py:72`), que lê ciclo (`app/escala/models.py:484`); o descritor adicional chama `rotulo_do_desenho()` (`app/core/templatetags/hasner_filters.py:145`).

## 16/11 23:24 · sessao ad6cef41af58 · - · - · origem=Ronald (tmux)

Tela: `espelho_ponto` chama `espelho_do_colab` e entrega `dias` ao template; use diretamente esse serviço na sonda, preservando o caminho real sem simular request. `app/ponto/views.py:160-163`, `app/ponto/views.py:195-200`, `app/ponto/services/espelho.py:1294-1296`.

Separação concreta: o realizado carrega `turnos_do_colab` diretamente e usa `_realizado_do_dia_tela`; a geometria `dia.turnos` provém dos períodos do motor. `app/ponto/services/espelho.py:614`, `app/ponto/services/espelho.py:675-676`, `app/ponto/services/espelho.py:421-422`, `app/ponto/services/espelho.py:450-460`, `app/ponto/services/espelho.py:767`.

O motor começa pelos turnos da autoridade e passa por `_periodos_pelo_marco`; quando a ata responde, remove **todos** os períodos cujo dia do juiz corresponde ao dia e acrescenta **um** período entrada–saída da ata. Esse mecanismo pode produzir a forma “dois turnos no realizado, um no motor”; sua execução específica para col922 depende da sonda [nao verificado]. `app/ponto/motor_calculo_v2.py:530-534`, `app/ponto/motor_calculo_v2.py:616`, `app/ponto/motor_calculo_v2.py:636-640`, `app/ponto/motor_calculo_v2.py:679-688`.

Não atribuir isso a retratada: ambos os carregamentos excluem `retratada_em` preenchida. `app/ponto/services/espelho.py:1022`, `app/ponto/turnos.py:2888-2895`, `app/ponto/turnos.py:1868-1871`.

Precisão necessária na resposta: o template não desenha início–fim de `dia.turnos`; usa essa lista para badges/alertas. Os horários visíveis vêm prioritariamente de `realizado_grade`, depois de `dia.batidas`. `app/templates/ponto/espelho.html:192-204`, `app/templates/ponto/espelho.html:298-305`, `app/templates/ponto/espelho.html:231-247`.

`montar_realizado_grade` só adapta células e órfãs; não devolve turnos nem minutos. A função antiga que calculava minutos da grade foi removida para evitar um segundo juiz. Portanto, a métrica “minutos dos turnos desenhados” precisa declarar que lê `dia.turnos` e restringe o realizado real a esses turnos, ou a contagem confundirá dinheiro do motor com geometria visível. `app/escala/utils.py:1078-1110`, `app/escala/utils.py:1113-1125`.

A janela real do request atual vai de `min(piso_visual, apur_ini)` até hoje, mesmo navegando competência anterior; passar `hoje=apur_fim` muda esse caminho. `app/ponto/views.py:100-101`, `app/ponto/views.py:161`, `app/ponto/services/espelho.py:1225-1227`, `app/ponto/services/espelho.py:1262-1263`.

## 16/11 23:24 · sessao 54c77838a647 · - · - · origem=Ronald (tmux)

Achados independentes:

| Saída | Prova | Quem vê | Classe |
|---|---|---|---|
| Wizard criando sob pai: mostra `pai.tipo_base` gravado, em “canônico herdado” | `app/templates/escala/wizard_tipo_escala.html:56` | Usuário autenticado autorizado em `editar_escalas` — `app/escala/views_wizard.py:63` e `:64` | Outra coisa: texto auxiliar |
| Wizard editando: mostra `tipo.tipo_base` gravado | `app/templates/escala/wizard_tipo_escala.html:61` | Mesmo público — `app/escala/views_wizard.py:63` e `:64` | Outra coisa: texto auxiliar |
| Wizard criação sem pai: opções de Base vêm de `TIPO_BASE_CHOICES` | `app/templates/escala/wizard_tipo_escala.html:37`; fonte `app/escala/views_wizard.py:79`, rótulos em `app/escala/models.py:163` | Mesmo público | Seletor de cadastro: exibe vocabulário gravável, mas não rótulo de registro já gravado; ramo restrito por `wizard_tipo_escala.html:26` |
| Wizard criação personalizada: opções “escala-base de referência” são rótulos literais | `app/templates/escala/wizard_tipo_escala.html:42`, `:44`, `:45`, `:46`, `:47` | Mesmo público | Seletor de cadastro, mesma ressalva; também dentro do ramo de criação de `:26` |

Os achados do wizard existem tanto na página inteira quanto no drawer: `app/escala/views_wizard.py:88`, `:89`, `:90`, `:91`, `:92`.

A pílula protegida lê `desenho_do_turno` em `app/templates/escala/tipos_lista.html:55`, delega a `rotulo_do_desenho` em `app/core/templatetags/hasner_filters.py:59` e `:60`; a função decide pelo intervalo e cruzamento da meia-noite em `app/escala/servico_jornada.py:193` e `:194`, monta rótulo em `:195` a `:205`.

A **cor continua vindo do tipo gravado**: azul para `12x36` (`app/templates/escala/tipos_lista.html:51`); verde para `comercial` (`:52`); amarelo para `plantao` (`:53`); cinza para os demais (`:54`).

Exclusões fundamentadas:

- `tipo_base` em inputs hidden não é exibição ao usuário: `app/templates/escala/wizard_tipo_escala.html:19`.
- A pílula azul do tag `rotulo_escala` usa `p['tipo']` (`app/core/templatetags/hasner_filters.py:81`), que vem de `tipo_ciclo_canonico()` (`app/escala/models.py:484`), mapeamento do ciclo (`app/escala/models.py:389`), e não de `tipo_base`.
- `rotulo_escala_texto` também lê `nome_canonico_partes()` (`app/core/templatetags/hasner_filters.py:207`), portanto sua parte “tipo” tem a mesma origem em ciclo (`app/escala/models.py:484`).
- O teste O74 verifica texto nas linhas-raiz (`app/escala/tests/test_o74_pilula_le_desenho.py:59`, `:65`, `:66`, `:69`, `:74`, `:75`); verde no HEAD não foi executado [nao verificado].

## 16/11 23:23 · sessao 1181ba86e409 · - · - · origem=Ronald (tmux)

Censo dos templates/tags: **2 pontos de exibição de valor gravado**, ambos no wizard, fora da pílula protegida:

| Ponto | Quem vê | Conteúdo |
|---|---|---|
| `app/templates/escala/wizard_tipo_escala.html:56` | Usuário autenticado com ação `editar_escalas` — `app/escala/views_wizard.py:63-64` | Ao criar sob pai, mostra `{{ pai.tipo_base }}` cru em “canônico herdado”. Pai carregado do banco — `app/escala/views_wizard.py:73-74`. |
| `app/templates/escala/wizard_tipo_escala.html:61` | Usuário autenticado com ação `editar_escalas` — `app/escala/views_wizard.py:63-64` | Ao editar, mostra `{{ tipo.tipo_base }}` cru em “Canônico (não editável)”. Tipo carregado do banco — `app/escala/views_wizard.py:67`. |

São a mesma página, também servida como drawer conforme `HX-Request` — `app/escala/views_wizard.py:88-92`; rotas criar e editar em `app/escala/urls.py:19,23`.

**Vocabulário de cadastro, separado do censo de valores gravados:** o `<select>` Base lê `bases` e imprime suas labels — `app/templates/escala/wizard_tipo_escala.html:36-37`; `bases` vem de `TipoEscala.TIPO_BASE_CHOICES` — `app/escala/views_wizard.py:79`, labels em `app/escala/models.py:163-168`. Esse ramo só aparece ao criar sem pai — `app/templates/escala/wizard_tipo_escala.html:26`, portanto não mostra o tipo gravado de registro existente. O seletor `tipo_base_ref` também só é cadastro; labels literais em `app/templates/escala/wizard_tipo_escala.html:42-47`.

**Pílula O74:** texto usa `t|desenho_do_turno` — `app/templates/escala/tipos_lista.html:55`; filtro delega a `rotulo_do_desenho` — `app/core/templatetags/hasner_filters.py:52-60`. Cor vem do **tipo gravado**: `12x36` azul, `comercial` verde, `plantao` amarelo, demais cinza — `app/templates/escala/tipos_lista.html:51-54`. Público autenticado com `ver_escalas` — `app/escala/views.py:77-79`.

**Outra pílula/tag que não é achado:** `rotulo_escala` constrói a pílula com `p['tipo']` — `app/core/templatetags/hasner_filters.py:72-81`; esse valor vem de `tipo_ciclo_canonico()`, não `tipo_base` — `app/escala/models.py:479-486`; seu descritor adicional também lê o desenho — `app/core/templatetags/hasner_filters.py:145-150`.

**Admin implícito a verificar na consolidação:** `TipoEscalaAdmin` é `admin.ModelAdmin` sem `fields`, `fieldsets`, `exclude` ou formulário próprio — `app/escala/admin.py:7-15`; modelo declara `tipo_base` com choices — `app/escala/models.py:170-174`. Isso implica widget padrão de choices na edição Django Admin, mas a renderização concreta do framework nesta cópia é **[não verificado]**.

## 16/11 23:23 · sessao 0de1d50dfbd0 · - · - · origem=Ronald (tmux)

Recomendações para a sonda:

- Chamar `turnos_do_colab(c, ini, fim)`; o wrapper carrega células, vínculos e papéis reais e exclui retratadas. O retorno contém `Turno(data_turno, entrada, saida, batidas, aberto, cross_meianoite)`. Serializar IDs das bordas e batidas, sem recalcular casamento. `app/ponto/turnos.py:43`, `app/ponto/turnos.py:1850`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1920`.
- Ler os carimbos nas instâncias de `t.batidas`: `_papel_ata`, `_intra_dur`, `_intra_ata`, `_intra_declarado`, `_eco_flush`, mais `is_intervalo`. São respostas do pareador; ele trabalha com cópias por segmento. `app/ponto/turnos.py:1310`, `app/ponto/turnos.py:1325`, `app/ponto/turnos.py:1370`, `app/ponto/turnos.py:1984`.
- `intervalos_do_turno(t)` entrega as pausas reconhecidas; `pausas_abaixo_do_piso(t)` entrega os pares marcados rejeitados pelo piso. Não derivar pausas examinando tipos adjacentes. `app/ponto/turnos.py:542`, `app/ponto/turnos.py:563`.
- Batida crua ausente dos turnos retornados deve sair como “ausente dos turnos retornados”; isso não demonstra que não recebeu papel. O pareador consome intervalo/`X` sem turno corrente e pode não incorporá-lo a retorno algum. Para imprimir o papel da ata desses casos, chamar `papel_por_minuto_da_ata(...)` e `_papel_da_ata(b, mapa)`, preservando o nome “papel da ata”. `app/ponto/turnos.py:1371`, `app/ponto/turnos.py:1395`, `app/ponto/turnos.py:890`, `app/ponto/turnos.py:1565`.
- Universo cru com retratadas deve ser lido separadamente por `Batida`; imprimir `timestamp` e `retratada_em` por `timezone.localtime`. Retratada não entra na geometria, mas continua exibível. `app/ponto/models.py:23`, `app/ponto/models.py:64`, `app/ponto/turnos.py:1868`, `app/ponto/turnos.py:45`.
- Célula: imprimir `trabalha`, `escala_geradora_id` e `CelulaDia.marcos_presentes((cel.dna or {}).get("marcos"))`. Evitar imprimir DNA inteiro. `app/escala/models.py:1671`, `app/escala/models.py:1685`, `app/escala/models.py:1692`, `app/escala/models.py:1697`.
- Pausa efetivamente declarada: chamar `ec.intervalo_do_dia(d, celulas=mapa_composto)` ou `ec.pausas_do_dia(...)`. Célula existente sem marcos responde nenhuma pausa; `marcos_do_dia` pode retornar marcos do template nessa mesma situação, por isso imprimir ambos com rótulos distintos. `app/escala/models.py:1124`, `app/escala/models.py:1160`, `app/escala/models.py:1182`, `app/escala/models.py:1202`.
- Número do realizado pelo caminho exato da tela: `_realizado_do_dia_tela(c, d, turnos, escalas_periodo, celulas)`, que chama o juiz real passando vínculo, modo livre e intervalo declarado do dia. `app/ponto/services/espelho.py:267`, `app/ponto/services/espelho.py:286`, `app/ponto/services/espelho.py:290`.

Armadilha de erros: `papel_por_minuto_da_ata` captura silenciosamente exceções de `periodos_do_dia` e se abstém; o `except` externo da sonda não verá esses erros internos. Caso a sonda pretenda contabilizá-los, precisa chamar também `periodos_do_dia(cel, te)` sob seu próprio contador, sem substituir a regra real. `app/ponto/turnos.py:1631`.

## 16/11 23:23 · sessao fae23013399b · - · - · origem=Ronald (tmux)

- O leitor do E6 é `minutos_do_espelho`: prioriza `minutos_realizados`; só usa `pago_h * 60` quando o primeiro é `None`; sem ambos retorna ausência, não zero (`app/ponto/management/commands/e6_oraculo.py:242`, `:263-269`).
- O campo `minutos_realizados` vem da chamada de `_realizado_do_dia_tela`, que usa `vinculo_do_dia`, `tipo_escala._intervalo_livre()` e `ec.intervalo_do_dia(...)`; não há cálculo próprio no wrapper (`app/ponto/services/espelho.py:267-294`, `:675-676`, `:783`).
- A autoridade final é `realizado_do_dia → realizado_dos_turnos` (`app/ponto/turnos.py:774-795`). A seleção diária usa **`t.data_turno == data`** (`:719`). O ramo usual obtém o fim por `_fim_fechado`, desconta `intervalos_do_turno` ou janela declarada e acumula `liquido` (`:735-771`). Há ramo diário especial L-115 por `_borda_posicional`, anterior à soma usual (`:725-732`).
- Os turnos usados por essa conta vêm de `turnos_do_colab(colaborador, data_ini, data_fim_mes)` (`app/ponto/services/espelho.py:611-614`). Essa função consulta batidas **sem retratação**, com janela de pad de dois dias antes e um depois, carrega células e escalas e delega ao corpo único `_turnos_do_colab_puro` (`app/ponto/turnos.py:1847`, `:1850-1920`). O corpo lê o papel da ata em `papel_por_minuto_da_ata` (`:1940-1944`).
- O desenho usa **outra lista**: o dicionário `resultados_por_entrada` recebe `resultado_v2.periodos + resultado_v2.periodos_ft` (`app/ponto/services/espelho.py:421-422`). A ponte entrada → pk da batida → `dia_das_batidas` atribui cada período ao dia (`:450-460`); `turnos_dia` é essa lista diária (`:655-656`), exportada como `dia['turnos']` (`:767`).
- Portanto, existe separação verificável de fontes entre períodos do motor (`app/ponto/services/espelho.py:421-422`) e turnos do juiz (`:614`). O uso do juiz para **atribuir datas** aos cards (`:450-460`) não converte os períodos do motor nos turnos do juiz. A causa específica da ausência dos turnos de col922 precisa da execução da sonda [nao verificado].
- As duas cadeias filtram retratadas: o motor recebe `batidas_apuraveis` (`app/ponto/services/espelho.py:1019-1022`); os turnos do juiz filtram `retratada_em__isnull=True` (`app/ponto/turnos.py:1867-1871`). A diferença de filtros de retratação não está provada como causa.
- Os envelopes carregados diferem: motor usa `[ini - 1 dia, fim 23:59:59 + 1 dia]` (`app/ponto/services/espelho.py:1019-1022`); juiz usa `janela_com_pad`, que começa em `ini - 2 dias` (`app/ponto/turnos.py:1847`, `:1867-1871`). Isso é diferença de carga comprovada; sua influência em col922 é [nao verificado].
- Para obter parcelas **sem replicar regras**, uma sonda pode observar `realizado_dos_turnos.__code__` via `sys.settrace`: no evento de linha de `total += liquido` captura `t`, `fim`, `bruto`, `pausas` e `liquido` já calculados pelo código real (`app/ponto/turnos.py:770`). No retorno especial captura `_t0`, `_ult`, `_bruto`, `_pausa` e o resultado L-115 (`:725-732`). Deve rotular esse ramo como parcela diária, pois ele não distribui contribuição por turno.
- Para contar a **mesma classe de geometria**, não comparar diretamente `minutos_realizados` com `p.minutos_trabalhados`: o próprio código distingue FATO de dinheiro apurado e registra exemplo de valores diferentes legítimos (`app/ponto/services/espelho.py:398-402`). Comparação fiel: chamar novamente `_realizado_do_dia_tela` com os **mesmos** escalas/células e somente os turnos do juiz representados pelos cards reais, preservando a autoridade de intervalo e a regra L-115 (`:267-294`; `app/ponto/turnos.py:725-732`). Eventual card sem correspondência deve gerar `ERRO`, não ser descartado em silêncio [nao verificado].

## 16/11 23:22 · sessao 75ff2010922c · - · - · origem=Ronald (tmux)

Padrões pertinentes:

- **Single Source of Truth / Aggregate Root**, alias “célula soberana / fonte única”: `app/docs/GLOSSARIO.md:11`; o norte explicita **single source of truth / um juiz por pergunta** em `app/docs/PLANO-PADROES.md:55-59`. Se as montagens do mesmo turno entregam universos incompatíveis para número e testemunha, esse é o padrão principal quebrado — interpretação da separação a demonstrar no código.
- **Duplicated business logic / logic drift**, alias “derivação paralela”, proibida pela LEI-AKITA 2: `app/docs/GLOSSARIO.md:25`. Aplicável se a separação revelar dois derivadores concorrentes da mesma pergunta; não basta constatar duas funções com finalidades diferentes.
- **CQRS com projeções reconstruíveis** inclui explicitamente espelho, DiaPago, fechamento, retrato e pílulas: `app/docs/GLOSSARIO.md:24`; O-PROJECOES exige fonte, versão do juiz e comando de reconstrução: `app/docs/PLANO-PADROES.md:11`. A divergência sozinha não prova defeito de versão ou reconstrução.
- **Immutable event log + compensating event** vincula “batida só retratada” ao universo `batidas_apuraveis`: `app/docs/GLOSSARIO.md:27`. Só nomear como quebra se houver diferença concreta no tratamento da retratada.
- O selo deve verificar o **significado da classe** — dia da batida, competência, vínculo do dia, retratada — nunca apenas a forma: `app/docs/PLANO-PADROES.md:9`.
- Não atribuir automaticamente quebra de **Single Writer Principle**: o glossário identifica esse padrão em `app/docs/GLOSSARIO.md:12`, mas duas projeções divergentes não provam dois escritores.

Universo da frota usado pelo E6:

```python
for empresa in Empresa.objects.filter(ativa=True).order_by("pk"):
    ini, fim = janelas.janela_fechamento(mes, ano, empresa)
    ids = FechamentoMensal.objects.filter(
        mes=mes, ano=ano, colaborador__empresa=empresa
    ).values_list("colaborador_id", flat=True)
    for colaborador in Colaborador.objects.filter(pk__in=ids):
        ...
```

É a seleção real em `app/ponto/management/commands/e6_oraculo.py:303-314`: empresas ativas e colaboradores com FechamentoMensal daquela competência/empresa; não filtra `Colaborador.ativo`. `--so-txt` é filtro adicional opcional em `app/ponto/management/commands/e6_oraculo.py:307-313`.

As competências devem chamar `janela_fechamento(mes, ano, empresa)`, que aplica o corte próprio da empresa: `app/ponto/janelas.py:54-57`. A delimitação por corte está em `app/ponto/janelas.py:10-21`; não são outubro/novembro civis.

Fonte numérica efetivamente comparada pelo E6: prefere `minutos_realizados`; sem ele usa `pago_h * 60`; sem ambos retorna ausência de lavratura: `app/ponto/management/commands/e6_oraculo.py:263-269`.

A borda usada pelos insumos E6 não é idêntica à janela da tela: `InsumosDoDono` lê `batidas_apuraveis` de início menos 12 h a fim mais um dia e 14 h (`app/ponto/management/commands/e6_oraculo.py:149-153`), e oferece `turnos_do_colab` de início menos um dia a fim mais um dia (`app/ponto/management/commands/e6_oraculo.py:156-160`). Isso pertence à comparação E6 e não deve substituir as funções reais da tela no censo solicitado.

## 16/11 23:22 · sessao d01e48c2ba58 · - · - · origem=Ronald (tmux)

Na cópia, a regra é L-099: separar defeito estrutural de escala/batida; apenas ESTRUTURA entra como obra, CADASTRO/BATIDA vão à lista do administrador (`app/docs/LEIS.md:122`).

A precedência **executada**, que deve orientar a resposta, é:

1. `folga_nao_definida`, `sem_vinculo_no_dia` ou `sem_vinculo_em_dia_de_trabalho` → CADASTRO (`app/ponto/management/commands/e6_oraculo.py:98`).
2. `batida_fora_de_turno=True` → BATIDA (`app/ponto/management/commands/e6_oraculo.py:104`).
3. Quantidade ímpar em `horas_do_dia` atribuídas **pelo E6** → BATIDA (`app/ponto/management/commands/e6_oraculo.py:106`).
4. `fase_deslocada=True`, lida da lavra mais recente que cobriu o dia → CADASTRO (`app/ponto/management/commands/e6_oraculo.py:108`, `app/ponto/management/commands/e6_oraculo.py:174`).
5. Dia presente na lista `dias_cadastro_x_realidade` → CADASTRO; restante → ESTRUTURA (`app/ponto/management/commands/e6_oraculo.py:110`).

A docstring que diz BATIDA primeiro e o comentário do laço de comparação estão incompletos diante das precedências acrescentadas; citar o corpo executável acima (`app/ponto/management/commands/e6_oraculo.py:61`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:374`).

**Chamada correta, sem copiar regra:**

```python
from ponto import janelas
from ponto.services.espelho import espelho_do_colab
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono, minutos_do_espelho, batida_fora_de_turno,
)
from ponto.supra_juiz import cadastro_do_dia_carregado

ini, fim = janelas.janela_fechamento(mes, ano, c.empresa)
esp = espelho_do_colab(c, hoje, ini, fim, agora=agora)
ins = InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)
turnos = ins.turnos()
dono = ins.dono(dia)
```

Essas mesmas chamadas aparecem no comando real: janela por empresa, espelho e carregador único (`app/ponto/management/commands/e6_oraculo.py:304`, `app/ponto/management/commands/e6_oraculo.py:316`, `app/ponto/management/commands/e6_oraculo.py:328`, `app/ponto/management/commands/e6_oraculo.py:378`).

**Insumos úteis para imprimir:**

- `ins.oraculo.minutos`, `.horas`, `.impares`, `.envelope`, `.brutas`, `.cortes`, `.pausa`; o E6 soma pares por posição e atribui pela primeira batida do fragmento (`app/ponto/calculador/nucleo.py:236`, `app/ponto/calculador/nucleo.py:245`, `app/ponto/calculador/nucleo.py:254`).
- `ins.cxr()`: conjunto das chaves `dia`, sem recalcular distâncias (`app/ponto/management/commands/e6_oraculo.py:162`).
- Linhas originais de `esp["resultado"].dias_cadastro_x_realidade`: cada uma pode trazer causa, real/cadastrado e minutos fora, ou ambas as pontas e deltas (`app/ponto/motor_calculo_v2.py:1622`, `app/ponto/motor_calculo_v2.py:1670`, `app/ponto/motor_calculo_v2.py:1992`).
- `ins.fase()` e `cadastro_do_dia_carregado(c.pk, dia)` são as autoridades reais das outras entradas do juiz (`app/ponto/management/commands/e6_oraculo.py:174`, `app/ponto/supra_juiz.py:179`).
- `batida_fora_de_turno(c, dia, turnos)` não é sinônimo de “qualquer batida não usada”: exige `realizado_do_dia(... livre=False, intervalo=(), turnos=turnos).aberto` **e** entrada real de turno aberto sem saída no universo apurável (`app/ponto/management/commands/e6_oraculo.py:198`, `app/ponto/management/commands/e6_oraculo.py:208`).

CXR não deve ser inferido só por “escala diurna versus batidas noturnas”: o juiz lê a lista publicada pelo motor. A regra das duas pontas exige ambas a mais de 180 minutos dos marcos, e o motor também publica causas de janela/HE na mesma lista (`app/ponto/management/commands/e6_oraculo.py:170`, `app/ponto/motor_calculo_v2.py:1931`, `app/ponto/motor_calculo_v2.py:1946`, `app/ponto/motor_calculo_v2.py:1670`).

`minutos_do_espelho(de)` lê `minutos_realizados`, depois `pago_h*60`; ausentes ambos retorna `(None, "sem lavratura")`, nunca zero (`app/ponto/management/commands/e6_oraculo.py:263`).

O universo E6 usa batidas apuráveis da competência com pad de 12 horas antes e 14 horas depois; `ins.turnos()` usa `turnos_do_colab` com um dia a mais em cada extremidade. Portanto não construir o E6 apenas com as três datas impressas (`app/ponto/management/commands/e6_oraculo.py:149`, `app/ponto/management/commands/e6_oraculo.py:156`).

Nenhum dado fornecido decide os insumos concretos atuais dos 12 casos; seus donos permanecem [nao verificado].

## 16/11 23:20 · sessao 0204aa9d588e · PREPARA-F2-F4-F7 · censo · origem=Code

## 1 DEPENDE e premissas

**BASE examinada: `520d7b55720888d80b46ffbca2ca73e59da40277`.** Os arquivos centrais foram conferidos contra os blobs desse commit. A cópia fornecida não tem `.git`; seu `HEAD` local: [nao verificado]. Banco, execução de sondas, ocorrência em produção e resultado de suíte: [nao verificado].

Os leitores continuam projetivos: `estado`, `respondida`, `resposta_display`, retenção e SQL de vitalidade ainda leem os campos. A migração descrita pela coluna c3 não está nesses corpos. (`app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/models.py:1923`; `app/chamados/models.py:3496`)

Nos quadros abaixo, `estados.md` e `matriz.md` são os anexos fornecidos; as linhas incluem vazias. Todos os demais caminhos são da BASE.

| Origem | Premissa julgada | Veredito na BASE |
|---|---|---|
| `matriz.md:14` | F1 vigente é `be2b8d733` | **CAIU.** A projeção vigente lê os campos atuais; o descarimbo apaga o instante. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1390`; `app/chamados/models.py:3318`) |
| `matriz.md:27` | Existem leitura conservada, helper dos atos e materialização conservada descritos ali | **CAIU.** Os corpos vigentes são leitura dos campos; aqueles helpers não foram localizados nesses módulos. (`app/chamados/juizes.py:1382`; `app/chamados/models.py:3664`; `app/chamados/models.py:3686`) |
| `[DEPENDE-F1]` 1 — `matriz.md:70` | `[3]` conserva o instante, inclusive após descarimbo | **CAIU.** `[3]` é `materializada_em` atual; descarimbar escreve `None`. (`app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1400`; `app/chamados/models.py:3318`) |
| `[DEPENDE-F1]` 2 — `matriz.md:72` | A assinatura conservada é reaplicada no caso passivo | **CAIU como premissa da BASE.** A projeção recorta valores futuros dos campos, sem reaplicar essa assinatura. Resultado do antigo T148: [nao verificado]. (`app/chamados/juizes.py:1391`; `app/chamados/juizes.py:1400`) |
| `[DEPENDE-F1]` 3 — `matriz.md:74` | Não há testemunha E em `[3]`, eliminando aquele relógio | **CONFERE o resultado**, embora a explicação pela F1 histórica tenha caído: `[3]` vem do campo. (`app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1400`) |
| `[DEPENDE-F1]` 4 — `matriz.md:75` | F1 sozinha já lê atos e rompe a continência de `q_vivas` por essa divergência | **CAIU.** C6 e `q_vivas` continuam projetivos; continência continua sendo o contrato, não igualdade SQL/C6. (`app/chamados/juizes.py:1404`; `app/chamados/models.py:3496`; `app/chamados/models.py:3467`) |
| `[DEPENDE-F1]` 5 — `matriz.md:76` | A guarda de PK nula daquele helper determina os resultados de instâncias não salvas | **CAIU para os leitores atuais.** Eles leem campos diretamente; o teste de opções usa instância não salva. (`app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/tests/test_valor_rotulo.py:7`) |
| `[DEPENDE-F1]` 6 — `matriz.md:77` | O NameError identificável do harness F1 c1 continua herdado | **CAIU por leitura.** O ambiente agora fornece `SimpleNamespace`, `texto_vazio` e o juiz real de respondida. Execução: [nao verificado]. (`app/chamados/tests/test_e4b_classe_e_memoria.py:308`; `app/chamados/tests/test_e4b_classe_e_memoria.py:310`; `app/chamados/tests/test_e4b_classe_e_memoria.py:374`) |
| `matriz.md:112` | L1 ainda é residual por falta de instante histórico conservado | **CAIU como pendência vigente**, pelos complementos fornecidos. O resultado a conservar é o do campo atual, leitor a leitor; não recuperação histórica. (`app/chamados/juizes.py:1390`; `app/chamados/models.py:3318`; `app/docs/CORTES.md:487`) |
| `matriz.md:116` | Os leitores do main continuam por campo | **CONFERE.** O resumo histórico não ativa os helpers da F1 antiga. (`app/chamados/models.py:1923`; `app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/models.py:3496`) |

**Resultado dos seis itens numerados: CAIU 5 de 6**, com a ressalva causal do item 3 indicada acima. (`app/chamados/juizes.py:1390`; `app/chamados/models.py:3496`; `app/chamados/tests/test_e4b_classe_e_memoria.py:308`)

As premissas da BUG-PAR que a F2 deve carregar agora são estas. Os nomes c4 pedidos não estavam explicitados nas linhas antigas dos consumidores; sua atribuição literal a uma linha de brief/matriz: [nao verificado]. As referências abaixo indicam as linhas dos anexos cujo consumo precisa dessa atualização.

| Origem afetada | Premissa vigente | Julgamento |
|---|---|---|
| `estados.md:7`, `estados.md:8`; M1a/M1b | Os novos nomes são **estados de aplicação**, não novos valores de `PerguntaDisputa.estado` | **CONFERE.** `estado` mantém seus retornos; a testemunha possui vocabulário próprio. (`app/chamados/models.py:3151`; `app/chamados/services/materializacao.py:705`; `app/chamados/services/materializacao.py:706`; `app/chamados/services/materializacao.py:852`) |
| Mesmos consumidores | Sem dia do fato deve retornar `bloqueada_sem_dia_do_fato`, sem chamar K6 | **CONFERE.** Não equivale a competência entregue. (`app/chamados/services/materializacao.py:784`; `app/chamados/services/materializacao.py:810`; `app/chamados/services/validacao.py:233`; `app/chamados/tests/test_bug_par_volta.py:319`) |
| Mesmos consumidores | Competência entregue retorna `bloqueada_competencia_entregue` | **CONFERE**, em ramo separado do anterior. (`app/chamados/services/materializacao.py:811`; `app/chamados/services/materializacao.py:812`; `app/chamados/services/validacao.py:234`) |
| `estados.md:7`, `estados.md:8`, `estados.md:17`; explicação/tela | BUG-PAR **L2** — F6 fechada/aprovada, não entregue — conserva o toque Validar; a recusa deve conservar `COMPETENCIA_APROVADA` | **CONFERE.** A testemunha retorna `bloqueada_competencia`; a porta pode produzir VALIDACAO+DEVOLUCAO sem batida, com o motivo de competência. (`app/chamados/services/materializacao.py:909`; `app/chamados/services/materializacao.py:935`; `app/chamados/tests/test_bug_par_volta.py:249`; `app/chamados/tests/test_bug_par_volta.py:258`; `app/chamados/tests/test_bug_par_volta.py:261`) |
| Mesmos consumidores | O motivo L2 vale também no laço e no passivo | **CONFERE por provas existentes lidas**; execução: [nao verificado]. (`app/chamados/tests/test_bug_par_volta.py:378`; `app/chamados/tests/test_bug_par_volta.py:391`; `app/chamados/services/materializacao.py:1469`) |
| Leitura da guarda pela testemunha | `k6_consulta_a_pergunta` significa pergunta não nula, metade de intervalo, fora U163 | **CONFERE.** Reduzi-lo apenas a “fora U163” **CAIU**. (`app/chamados/services/materializacao.py:792`; `app/chamados/services/materializacao.py:795`) |
| Leitura da irmã | A irmã é sempre buscada para K6 | **CAIU.** A c4c dispensa a busca quando K6 já consulta a própria pergunta; caso contrário, procura a irmã. (`app/chamados/services/validacao.py:219`; `app/chamados/services/validacao.py:224`; `app/chamados/services/materializacao.py:139`) |
| Porta visual administrativa | Só competência entregue retira Validar | **CAIU como descrição suficiente.** Também retiram: ausência do dia do fato, espera DECLARADO-X-GRAVADO e indisponibilidade; F6 conserva o toque. (`app/templates/chamados/partials/_pergunta_card.html:44`; `app/templates/chamados/partials/_pergunta_card.html:47`; `app/templates/chamados/partials/_pergunta_card.html:49`; `app/templates/chamados/partials/modal_fio.html:276`) |
| Ordem da testemunha | K6 precede F6, que precede a espera declarado/gravado | **CONFERE nos ramos correspondentes.** (`app/chamados/services/materializacao.py:885`; `app/chamados/services/materializacao.py:909`; `app/chamados/services/materializacao.py:922`; `app/chamados/services/materializacao.py:933`; `app/chamados/services/materializacao.py:936`) |
| C1, item 9a, retirado pelo C2 | `_escrever_carimbo` é o único persistidor do trio | **CAIU.** Há lote direto e escritores somente da via. (`app/chamados/models.py:3391`; `app/chamados/models.py:3393`; `app/chamados/models.py:3402`; `app/chamados/services/relavra_via.py:125`) |
| C2, item 9a, corrigido pelo C3; `estados.md:18` | Descarimbar basta para voltar à lista | **CAIU.** Continuam necessárias ausência de validação, instante ou texto e tratamento da tranca; a via terminal também impede o descarimbo. (`app/chamados/models.py:1923`; `app/chamados/models.py:1924`; `app/chamados/models.py:1926`; `app/chamados/models.py:3316`; `app/chamados/models.py:3392`) |
| C3; `estados.md:29` | Ressuscitar é o controle que zera instante e texto e não retorna à retenção por resposta | **CONFERE.** Não escreve REABERTURA. (`app/chamados/models.py:1622`; `app/chamados/models.py:1629`; `app/chamados/models.py:1630`; `app/chamados/models.py:1924`) |

O **CENSO C2 = 0** confere somente para o predicado completo “leitor da F2 dependente do instante apagado para consultar antes do descarimbo”. A projeção parte do campo atual; lista e estado não recebem esse horizonte histórico. Isso não significa ausência de leitores temporais. (`app/chamados/juizes.py:1389`; `app/chamados/models.py:1910`; `app/chamados/models.py:3154`; `app/ponto/supra_juiz.py:573`; `app/chamados/veredito_lavrado.py:203`)

**Delta `71e9576bf → 520d7b557` nos sítios citados:**

| Sítio | Resultado da conferência |
|---|---|
| `models.py`, `juizes.py`, `tarefa_do_dia.py`, `pergunta_no_app.py`, API, `supra_juiz.py`, `_card_atendimento.html` | Sem alteração nesses arquivos no diff pedido; referências de `estados.md` continuam casando. Os leitores centrais continuam nos mesmos locais. (`app/chamados/models.py:3151`; `app/chamados/juizes.py:1404`; `app/chamados/services/tarefa_do_dia.py:158`; `app/api/views.py:2553`; `app/ponto/supra_juiz.py:573`) |
| `explicador.py` | Antigas **72→95, 73→96, 74→97, 77→100, 85→108, 93→116, 139→177**. A antiga 102 mudou de conteúdo: agora há tabela/helper, recusa com `gravou=False` e espera declarada antes de cadastro/sistema. (`app/chamados/services/explicador.py:33`; `app/chamados/services/explicador.py:95`; `app/chamados/services/explicador.py:108`; `app/chamados/services/explicador.py:124`; `app/chamados/services/explicador.py:131`; `app/chamados/services/explicador.py:142`; `app/chamados/services/explicador.py:177`) |
| `fio.py` | Antiga **187 permanece; 357→359; 652→660**. Entraram carregamento de empresa e tratamento restritivo da testemunha ausente/excepcional. (`app/chamados/services/fio.py:187`; `app/chamados/services/fio.py:217`; `app/chamados/services/fio.py:359`; `app/chamados/services/fio.py:378`; `app/chamados/services/fio.py:660`) |
| `_pergunta_card.html` | **23/25/29/31 permanecem**; aplicação e bloqueios foram acrescentados no corpo da ação. (`app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:31`; `app/templates/chamados/partials/_pergunta_card.html:43`) |
| `modal_fio.html` | Antigas **436→463, 440→467**; antiga 442 mudou: agora “Não gravado” e `it.quem_resolve`. Novos estados também atingem o botão Validar. (`app/templates/chamados/partials/modal_fio.html:230`; `app/templates/chamados/partials/modal_fio.html:276`; `app/templates/chamados/partials/modal_fio.html:463`; `app/templates/chamados/partials/modal_fio.html:467`; `app/templates/chamados/partials/modal_fio.html:469`) |
| Consumidores adicionais | A ação usa a mensagem real da porta; desenlace lê `gravou=False`; passivo reconhece a metade já materializada pela irmã no mesmo ato. (`app/chamados/services/acoes_disputa.py:63`; `app/chamados/services/desenlace.py:67`; `app/chamados/services/passivo_validado.py:534`) |
| UI-VINCULO / EDITOR-CAL | O diff acrescenta apresentação do calendário padrão e confirmação de substituição da grade; não muda os corpos F2 acima. (`app/templates/colaboradores/partials/painel_vinculo.html:211`; `app/templates/escala/folga_calendario_editar.html:126`) |

## 2 Censo escritores e leitores

Convenções: caminhos abaixo relativos a `app/`; **R**=`respondida_em`, **T**=`resposta_colab`, **V/VP**=`validada_em/validada_por`, **M/MP**=`materializada_em/materializada_por`. **NOVO** significa ausente da enumeração dos anexos, não necessariamente criado no diff.

O levantamento alcançou atributos AST, chamadas, aliases, `getattr`, campos ORM e templates. Completude para nomes construídos dinamicamente: [nao verificado].

**Escritores físicos e portas:**

| Campos | Escritores encontrados |
|---|---|
| R/T no nascimento — **NOVO** | `_nascer` recebe os campos, verifica o par, instancia e persiste; conserva linhas legadas. (`chamados/models.py:2949`; `chamados/models.py:3124`; `chamados/models.py:3127`; `chamados/models.py:3129`) |
| R/T — resposta humana | `responder_pergunta_disputa_core`: atribuição, persistência e RESPOSTA. (`chamados/services/disputa_emissao.py:1481`; `chamados/services/disputa_emissao.py:1483`; `chamados/services/disputa_emissao.py:1525`) |
| R/T — automática | `responder_automaticamente`, com RESPOSTA. (`chamados/services/disputa_emissao.py:1674`; `chamados/services/disputa_emissao.py:1677`; `chamados/services/disputa_emissao.py:1678`) |
| R — **NOVO** | `carimbar_resposta_pela_trilha`, com RESPOSTA. (`chamados/services/disputa_emissao.py:1696`; `chamados/services/disputa_emissao.py:1698`; `chamados/services/disputa_emissao.py:1699`) |
| R/V/VP pelo carimbo | `carimbar`, condicionado a texto não vazio; persiste após a guarda C6. (`chamados/models.py:3240`; `chamados/models.py:3242`; `chamados/models.py:3245`; `chamados/models.py:3253`; `chamados/models.py:3262`) |
| V/VP | `_carimbar_validacao`; classe A da resposta humana; devolução recusada; devolução BUG-PAR **NOVO**. (`chamados/services/validacao.py:168`; `chamados/services/disputa_emissao.py:1494`; `chamados/services/validacao.py:273`; `chamados/services/validacao.py:349`) |
| M/MP/via individual | `_escrever_carimbo` atribui; persistem `carimbar`, `carimbar_identidade` e `descarimbar`. (`chamados/models.py:3209`; `chamados/models.py:3210`; `chamados/models.py:3211`; `chamados/models.py:3262`; `chamados/models.py:3299`; `chamados/models.py:3322`) |
| Trio e outros campos em lote | `descarimbar_em_lote`; chamadores que zeram R/T/V/VP: ressuscitar e reabrir para correção. (`chamados/models.py:3391`; `chamados/models.py:3393`; `chamados/models.py:1629`; `chamados/services/disputa_emissao.py:1609`; `chamados/services/disputa_emissao.py:1648`) |
| Somente via | `encerrar_por_fato_no_espelho`; `relavra_via.relavrar`. (`chamados/models.py:3402`; `chamados/models.py:3403`; `chamados/services/relavra_via.py:125`; `chamados/services/relavra_via.py:127`) |
| Apenas memória | Simulação da ressuscitação e rollback do carimbo. (`chamados/models.py:1553`; `chamados/models.py:1554`; `chamados/models.py:1556`; `chamados/models.py:3272`) |

`RegularizacaoExterna` é outro modelo: suas escritas não pertencem ao censo da pergunta. (`chamados/services/regularizacao_ext.py:199`; `chamados/models.py:4152`)

**Sequência DP e pré-requisito de conservação:**

| Item | Censo |
|---|---|
| Escritor físico | `DecisaoPergunta.registrar`; escrita em lote centralizada. (`chamados/models.py:4110`; `chamados/models.py:4120`) |
| Produtores dos atos | RESPOSTA: `disputa_emissao.py:1525,1678,1699`; VALIDACAO: `validacao.py:173`; DEVOLUCAO: `validacao.py:279,354`; ABSORCAO: `materializacao.py:1045`; RECUSA/REABERTURA: `disputa_emissao.py:1631`; SILENCIO: `models.py:2005`, `ponto/services/veredito_celula.py:195`, `management/commands/backfill_decisoes_pergunta.py:324`; RETRATACAO: `models.py:2137`. Todos os serviços/comandos sem prefixo nessa célula estão sob `chamados/`. |
| Leitores de `linhas_da_projecao_sem_linha` | `chamados/models.py:1613,3129`; `chamados/services/disputa_emissao.py:1480,1673`; `chamados/services/validacao.py:167,272,345`; `chamados/services/tarefa_do_dia.py:430`; `chamados/management/commands/backfill_decisoes_pergunta.py:67`; **NOVO** `chamados/management/commands/backfill_fio_morto_e4b.py:141`. |
| Outros leitores DP — **NOVO** | Histórico `chamados/models.py:3675`; motivo BUG-PAR `chamados/services/materializacao.py:275`; prefetch `chamados/services/fio.py:263`; tarefa `chamados/services/tarefa_do_dia.py:421`; fotografia `chamados/services/foto_e4b.py:280`; backfill `chamados/management/commands/backfill_decisoes_pergunta.py:99,315`. |
| Limites | Os tipos continuam sem MATERIALIZACAO; ressuscitar continua sem REABERTURA. (`chamados/models.py:4024`; `chamados/models.py:4025`; `chamados/models.py:1622`) |

**Consumidores das funções da fatia:**

| Função | Chamadores produtivos encontrados |
|---|---|
| `PerguntaDisputa.estado` | `api/views.py:2550`; `chamados/services/explicador.py:95`; `chamados/services/fio.py:660`; `chamados/services/vigia_fio.py:259`; `templates/chamados/partials/_card_atendimento.html:189`. |
| `respondida` | Property em `chamados/models.py:3664`; nenhum acesso produtivo direto localizado. Cobertura dinâmica: [nao verificado]. |
| `resposta_display` | `api/views.py:2562`; `_pergunta_card.html:31`; **NOVO** `api/views.py:1164`, `chamados/services/disputa_emissao.py:1560`, `templates/chamados/partials/_card_atendimento.html:106,143,183,391`. |
| `sem_veredito_entre` | `chamados/models.py:1906`; `chamados/juizes.py:2105`; `ponto/supra_juiz.py:573,574`. |
| `respostas_sem_veredito` | `chamados/models.py:1934,1967`; `chamados/reconciliador.py:1164`; `chamados/management/commands/validar_classe_a.py:54`; `chamados/services/competencia_trancada.py:126`; `chamados/services/fila_por_causa.py:226`; `colaboradores/services/desligamento.py:165`; `colaboradores/services/ficha.py:169`. |
| `pode_fechar` | `chamados/relatorios_disputa.py:24`; **NOVO** `api/foto_recebida.py:83`; `chamados/services/explicador.py:177`; **NOVO** `chamados/services/becos.py:33,56`, `materializacao.py:1067,1989`, `disputa_emissao.py:1292,2113`, `competencia_trancada.py:178,227`, `chamados/reconciliador.py:412,612,1171`, `ponto/services/entrada_sincronizada.py:59`. Os três serviços abreviados estão sob `chamados/services/`. |
| `q_vivas` | `chamados/juizes.py:1751`; `chamados/services/fio.py:256`; `pergunta_no_app.py:53`; `vigia_fio.py:711`; **NOVO** `janela_colab.py:63`, `cobranca.py:237`, `raiox.py:74`, `ponto/services/veredito_celula.py:181`. Serviços abreviados sob `chamados/services/`. |
| `_ancoradas` | `chamados/models.py:223,334,381`; **NOVO** `chamados/services/disputa_emissao.py:1821,1962`. Não chama `q_vivas` no corpo atual. (`chamados/models.py:344`) |
| `_gravar_estado` | `chamados/models.py:450,655,835,853,1489,1683,1703`. |
| `DisputaSupervisao.fechar` | `chamados/models.py:2031,2153`; `colaboradores/services/desligamento.py:167`; `chamados/services/materializacao.py:1068,2006`; `disputa_emissao.py:1298,2114`; `competencia_trancada.py:229`; `becos.py:57`; `chamados/management/commands/encerrar_perguntas_com_fato.py:88`; `chamados/reconciliador.py:413,613,1172`; `api/foto_recebida.py:84`; `ponto/services/entrada_sincronizada.py:60`. Serviços abreviados sob `chamados/services/`. |
| `remover_motivo` | `colaboradores/signals.py:77`; `escala/signals.py:174`. |
| `ressuscitar_perguntas` | `chamados/models.py:1498`; **NOVO** `chamados/views_arquivar.py:86`; `ponto/services/cartorio.py:1673`. |
| `_universo_da_ressuscitacao` | `chamados/models.py:1507,1512,1593`. |
| `projecao_temporal_da_pergunta` | `chamados/juizes.py:351,1404`; `escala/services/cadastro_realidade.py:502`. |
| `irmas_respondidas_sem_consumo` | `chamados/reconciliador.py:448`; `chamados/services/materializacao.py:1244`; alias em `chamados/services/disputa_emissao.py:2032`; alias no auditor **NOVO** `chamados/management/commands/auditar_invariantes_chamados.py:984`. |

**Leitores diretos dos campos, além dos corpos e portas já enumerados.** Cada grupo inclui cargas/fotos/filtros ORM; não significa que todo sítio julgue o estado.

| Arquivo | Campos e sítios |
|---|---|
| `api/views.py` | R `:1165,2563,2577,2620,2637`; T `:1163,2561`. |
| **NOVO** `api/views_mensageria.py` | R `:569,570`; T `:567`. |
| **NOVO** `chamados/canal.py` |

[truncado em 20000 de 59742 caracteres]

## 16/11 23:20 · sessao f2d6b5cd3a7e · PREPARA-F2-F4-F7 · censo · origem=Code

## 1 DEPENDE e premissas

**A premissa central caiu: a F1 pousada fornece o juiz `pergunta_respondida`, baseado em instante e texto da projeção; não fornece o contrato D01 descrito nos anexos.** O juiz do slot continua passando a pergunta real ao derivador. (`app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:376`)

BASE fornecida: `520d7b55720888d80b46ffbca2ca73e59da40277`. A cópia não contém metadados Git que permitam confirmar independentemente esse SHA. [nao verificado]

As linhas dos anexos abaixo foram contadas sobre o texto fornecido, começando pelo título de cada anexo; correspondência com arquivos originais externos à cópia: [nao verificado]. Nenhuma medição atual de produção nem execução de testes foi fornecida para esta leitura. [nao verificado] [nao verificado]

Para compactar as referências seguintes:

| Sigla | Arquivo |
|---|---|
| M | `app/chamados/services/materializacao.py` |
| D | `app/chamados/services/disputa_emissao.py` |
| J | `app/chamados/juizes.py` |
| V | `app/chamados/services/validacao.py` |
| MD | `app/chamados/models.py` |
| PV | `app/chamados/services/passivo_validado.py` |
| RV | `app/chamados/services/relavra_via.py` |
| FC | `app/chamados/services/fabrica_da_celula.py` |
| T | Diretório `app/chamados/tests/` |

Cada referência `M:795`, por exemplo, significa `app/chamados/services/materializacao.py:795`.

**Cada ocorrência de `[DEPENDE-F1]`:**

| Linha do anexo | Premissa | Julgamento e BASE |
|---|---|---|
| brief:38 | Absorção depende do D01 sem ramo ABSORCAO | **CAIU o contrato D01. CONFERE permanecer no carimbo**, por corte posterior. (`J:1544`, `M:1105`, `app/docs/CORTES.md:487`) |
| brief:106–107 | Assinatura `leitura(...decisoes,agora,materializada_conservada_em)` | **CAIU.** O contrato pousado é `pergunta_respondida(p) → bool`. (`J:1544`, `J:1552`) |
| brief:117 | Carga fornece decisões ao D01 do juiz | **CAIU.** A carga fornece `agora` e slots; chama `_slot_esperado(p,carga=self)`, que recebe a projeção. (`app/chamados/services/carga_pergunta_viva.py:88`, `app/chamados/services/carga_pergunta_viva.py:119`, `J:364`, `J:376`) |
| brief:137 | Ordem `(ato_em,pk)` é contrato da F1 | **CONFERE a ordem da sequência; CAIU atribuí-la ao redutor F1 pousado.** (`MD:4056`, `J:1544`) |
| brief:144 | RED de absorção não pode ser servido pelo D01 | **CAIU a dependência dessa interface.** A permanência no carimbo está decidida separadamente. (`J:1544`, `app/docs/CORTES.md:487`) |
| brief:162–164 | Absorção ainda exige pergunta de lei | **CAIU.** M9b/M10 ficam no carimbo na F4; o predicado por ato pertence à E4d. (`app/docs/CORTES.md:487`) |
| matriz:20, contrato:22–28 | `LeituraDosAtos` e redutor D01 constituem a entrada | **CAIU.** A entrada real é o par projetado; ordenação da sequência continua existindo, sem provar esse redutor. (`J:1382`, `J:1423`, `J:1544`, `MD:4056`) |
| matriz:40, M7 | Testemunha recebe decisões/agora da F1 | **CAIU.** A assinatura é `(disputa,p)`; lê projeção e chama o derivador. (`M:852`, `M:869`, `M:873`) |
| matriz:43, M9b | “Ninguém lê ABSORCAO hoje” | **CAIU.** `tarefa_do_dia` lê ABSORCAO e o estado da ausência. (`app/chamados/services/tarefa_do_dia.py:178`) |
| matriz:44, M10 | Absorção permanece pergunta de lei dependente da F1 | **CAIU a pergunta aberta; CONFERE manter o carimbo.** (`app/docs/CORTES.md:487`, `M:1105`) |
| matriz:72, E9 | REABERTURA escrita alimenta D01 | **CONFERE a escrita; CAIU o leitor D01.** (`D:1631`, `D:1648`, `J:1389`, `J:1544`) |
| matriz:91, J1 | Juiz usa `_PerguntaParaPlantio` com texto do ato | **CAIU.** Passa `p` diretamente; `data_hora` também lê `p.resposta_colab`. (`J:364`, `J:376`) |
| matriz:94–99 | Juiz D01 e materializador já leem fontes diferentes | **CAIU na BASE.** Ambos recebem a projeção. A obrigação documental de ordem/contador permanece escrita. (`J:376`, `M:895`, `M:1328`, `app/docs/CORTES.md:487`) |
| matriz:106 | Núcleo pode migrar para `leitura().resposta` da F1 pousada | **CAIU o suporte desse contrato.** L-130 define “respondida”; não determina, sozinha, substituir o texto vigente pelo último ato. (`J:1544`, `app/docs/LEIS.md:151`) |
| matriz:107–108 | Fila M23 selecionará por D01 | **CAIU o suporte D01.** A fila atual seleciona R/V/C da projeção. (`M:2063`, `M:2064`, `M:2065`, `M:2066`) |
| matriz:110 | Três chamadores entregarão decisões/agora iguais | **CAIU nesse formato e nessa abrangência.** Existem cinco sítios de chamada; o helper recebe `carga`, sem parâmetros `decisoes/agora`. (`M:599`, `M:214`, `M:222`, `M:895`, `M:1328`, `J:376`) |
| matriz:121–122 | Lei de absorção permanece aberta | **CAIU.** Respondida pelo corte posterior. (`app/docs/CORTES.md:487`) |
| matriz:131 | Resumo das marcas do contrato antigo | **CAIU como descrição das dependências atuais.** Não acrescenta um contrato ao juiz pousado. (`J:1544`, `app/docs/LEIS.md:151`) |

**Premissas sobre a BUG-PAR e premissas adicionais afetadas:**

| Linha do anexo | Julgamento |
|---|---|
| brief:35;148–151; matriz:40;48 — BUG-PAR reescreve testemunha, plantio e acrescenta leitores | **CONFERE o comportamento na BASE.** Existem derivador compartilhado, composição K6 da pergunta/irmã, par projetado e contador. Os offsets antigos precisam ser substituídos pelos atuais. (`M:175`, `M:295`, `M:876`, `M:885`, `M:907`, `M:1830`) |
| brief:148–153;184; matriz:12–14 — BUG-PAR ainda fora do main | **CAIU.** Há registro de pouso c4+c4c e seus estados estão implementados. Comparação histórica do diff de `6e2fc9133`, sem Git local: [nao verificado]. (`app/docs/BACKLOG.md:2081`, `M:705`, `M:706`) |
| brief:152–153 — passivo também recebe alterações da BUG-PAR | **CONFERE.** O passivo relê o carimbo e evita materializar novamente a metade gravada pela irmã. (`PV:531`, `PV:535`, `PV:537`) |
| brief:153;184 — refazer censo depois da BUG-PAR | **CONFERE.** Há leitores e chamadores adicionais que o censo antigo não cobria. (`M:199`, `M:214`, `M:718`, `M:770`, `M:1707`, `V:349`) |
| brief:156–157 — F1 adicionará D01 e costura pelo embrulho | **CAIU a versão da F1; CONFERE a chamada juiz → derivador.** (`J:376`, `J:1544`) |
| brief:115–118; matriz:110 — três caminhos bastam para paridade | **CAIU.** O par de intervalo acrescenta duas derivações, uma por metade. (`M:214`, `M:222`) |
| brief:166–169; matriz:123–124 — guardas de reparo ainda pedem lei | **CAIU.** A allowlist de reparo já foi cortada. (`app/docs/CORTES.md:487`) |
| brief:176–186 — números antigos demonstram o requisito atual de frota | **CAIU como medição atual.** Os valores são históricos fornecidos; estoque atual e efeito após B2: [nao verificado]. O medidor real continua disponível. (`MD:4081`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`) |

## 2 Censo escritores e leitores

`NOVO` significa **não nomeado como esse sítio no brief**, sem afirmar que nasceu recentemente. O censo abaixo combina AST de atributos/keywords/strings ORM, grep de templates e conferência das portas; chamadas dinâmicas fora dessas formas têm exaustividade semântica [nao verificado].

**Escritores, distinguindo persistência de simulação:**

| Campo | Escritores/portas na BASE |
|---|---|
| `resposta_colab` | Resposta `D:1481`; automática `D:1674`; reabertura por lote `D:1609`→`D:1648`; **NOVO** ressuscitação `MD:1629`→`MD:1630`; nascimento por `**campos` em `MD:3124`/`MD:3127`. `MD:1556` é simulação para julgamento. |
| `respondida_em` | `D:1482`, `D:1675`, reparo retroativo `D:1696`, lote `D:1609`→`D:1648`, ressuscitação `MD:1629`, `carimbar(resposta=)` em `MD:3242`, nascimento `MD:3124`. Simulação/reversão em memória: `MD:1555`, `MD:3273`. |
| `validada_em/por` | `V:168`/`V:169`; devolução `V:273`/`V:274`; **NOVO BUG-PAR** `V:349`/`V:350`; nova resposta `D:1494`/`D:1495`; lote `D:1609`; ressuscitação `MD:1629`; `carimbar('validar')` em `MD:3245`/`MD:3246`; nascimento `MD:3124`. |
| `materializada_em/por` | Porta `_escrever_carimbo`, `MD:3209`/`MD:3210`; portas de descarimbo e lote, `MD:3320`, `MD:3393`. `regularizacao_ext.py:199` pertence a outro modelo. (`app/chamados/services/regularizacao_ext.py:199`, `MD:4152`) |
| `via_resolucao` | `MD:3211`, lote `MD:3393`, encerramento específico `MD:3402`, relavra `RV:125`. |
| `recusa_motivo` | Escape do colaborador `D:1511`; motivo técnico `M:1332`; recusa `M:1665` ou extra de `carimbar_identidade` em `MD:3298`; recusa administrativa `V:827`. |
| `pendencia_admin` | Materializador escreve junto do motivo técnico. **NOVO leitor:** par rejeita irmã com pendência. (`M:1332`, `M:1333`, `M:197`) |
| `data_conferida_em/por` | Porta de conferência `V:907`/`V:908`; leitores **NOVO** `_data_em_disputa` e unidade de materialização. (`M:169`, `M:1851`) |
| `fechada_em` da Disputa | `fechar`, `reabrir_admin`, `reabrir_sistema`. (`MD:2013`, `MD:2230`, `MD:2254`) |
| Linhas de `DecisaoPergunta` | Escritor persistente único `registrar`, com `bulk_create` interno. (`MD:4110`, `MD:4120`) |

**O furo `carimbar(resposta=)` continua:** atribui R/V sem registrar RESPOSTA/VALIDACAO; absorção registra somente ABSORCAO, e resolução de chamado usa `resposta='responder'`. A quantidade atual desses casos não foi medida. (`MD:3240`, `MD:3242`, `MD:3245`, `MD:3262`, `M:1043`, `M:1045`, `D:2082`) [nao verificado quanto à frota]

**Chamadores das funções alcançadas pela fatia:**

| Função | Sítios diretos encontrados |
|---|---|
| `_tipo_pelo_marco` | `M:656` |
| `_ts_tipo_do_plantio` | `J:376`; **NOVO** `M:214`, `M:222`; `M:895`, `M:1328` |
| `_materializar_uma` | **NOVO** irmã em `M:1707`, par em `V:555`, chamada individual em `V:687`; `PV:537`, `M:2084` |
| `_materializar_resposta_hora/data_hora/escolha` | `M:1889`, `M:1891`, `M:1893` |
| `_materializar_ausencia_de_resposta` | `D:1555` |
| `materializar_perguntas_validadas_da_disputa` | `app/chamados/management/commands/backfill_fio_morto_e4b.py:147`; **NOVO na lista do brief:** `app/chamados/services/acoes_disputa.py:141`, `app/chamados/services/fio.py:716`, `V:698` |
| `materializar_pendentes_aprovadas` | `app/chamados/signals.py:165` |
| `estado_aplicacao_pergunta` | `app/chamados/services/fio.py:376`; **NOVO:** contador `M:339`, espera `M:757`, explicador `app/chamados/services/explicador.py:124`, templatetag `app/chamados/templatetags/chamados_tags.py:136` |
| **NOVO** `par_do_intervalo` | `M:907`, `M:1695`, `V:545` |
| **NOVO** `metade_irma_do_intervalo` | `M:194`, `M:2019`, `V:206` |
| **NOVO** `competencia_do_fato` desta materialização | `M:330`, `M:808`, `V:338`; `app/chamados/management/commands/bug_par_103.py:58`, `:74`, `:116` |
| **NOVO** `k6_consulta_a_pergunta` | `M:806`, `V:224` |
| **NOVO** `_k6_que_barra` | `M:825`, `V:225` |
| **NOVO** `_competencia_que_barra` | `M:909`, `M:933`, `M:1450`, `M:1830` |
| **NOVO** `_declarado_da_metade` / `divergencia_declarado_x_gravado` | `M:728` / `M:741` |
| **NOVO** `_espera_do_aval_163` / `espera_na_escrita` | `M:922`, `M:936`, `M:1451` / `M:1877` |
| **NOVO** `espera_declarado_x_gravado` | `V:611`; `app/chamados/services/explicador.py:132`; `app/chamados/templatetags/chamados_tags.py:128` |
| **NOVO** `respostas_intervalo_recusadas_paridade` | `app/chamados/management/commands/bug_par_103.py:79` |
| **NOVO** `paridade_absolvida_bug_par_103` / `rotulo_bug_par_103` | `M:327`, `V:334` / `M:880` |
| `perguntas_absorvidas_por` / devolução | `M:1147` / `app/ponto/signals.py:262` |
| `_carimbar_recusa` | `M:1318`, `M:1430`, `M:1871`, `M:1966`, `PV:566`, `V:562` |
| `responder_pergunta_disputa_core` | `app/api/views.py:2627`; `app/chamados/services/acoes_disputa.py:373`; `app/chamados/services/declaracao_texto.py:419`, `:693`; `app/chamados/views.py:1283` |
| `responder_automaticamente` | `app/api/foto_recebida.py:48` |
| `carimbar_resposta_pela_trilha` | `app/chamados/management/commands/curar_perguntas_em_limbo.py:67` |
| `reabrir_para_correcao` | `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`; `MD:2318`; `app/chamados/services/acoes_disputa.py:105`; `M:1164`; `V:836`; `app/chamados/views.py:1436` |
| `recobrar_questionario` | `app/chamados/views.py:1425` |
| `reabrir_credor_de_pergunta_viva` | `app/chamados/management/commands/backfill_fio_morto_e4b.py:90`, `:135`, `:175`; `D:1969` |
| `resolver_perguntas_por_chamado_resolvido` | `app/chamados/reconciliador.py:537`, `:1649`; `app/chamados/services/becos.py:54`; `app/chamados/signals.py:117`; `app/ponto/management/commands/reconciliar_fantasmas.py:281` |
| `_resolver_perguntas_por_chamado_resolvido` / `_fechar_disputa_se_quitada` | `D:1897`, `D:1903` / `M:2086`, `PV:413` |
| `relavrar` / `via_de_hoje` | `app/chamados/management/commands/relavrar_via_i6.py:102`, `RV:157` / comando `:64`, `RV:120` |
| `q_carimbo_desfazivel` | `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:446`; `FC:150`; `RV:238`; `app/chamados/services/vigia_fio.py:769` |
| `desfazer_carimbo_sem_lastro` | `FC:169`, `FC:209` |
| `reparar` / `_plantar_o_dia` | `app/chamados/management/commands/materializar_passivo_validado.py:104`, `:113` / `PV:443` |
| `motivos_a_perguntar` / `perguntar` | `app/chamados/management/commands/disparar_perguntas_competencia.py:117`, `FC:207`, `FC:242` / `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`, `FC:219` |

Os usos homônimos de `competencia_do_fato` no backfill e em `ponto` importam outra função; não são leitores da composição BUG-PAR acima. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:53`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:111`, `M:776`)

**Censo global dos campos — referências de produção por nome.** Inclui leitura, escrita, filtros, fotos e listas de campos; a classificação dos escritores está acima. Homônimos não provam identidade de modelo. As referências exteriores à lista nominal do brief constituem ampliação **NOVO do censo**, sem afirmação sobre quando entraram.

Legenda: `R=resposta_colab`, `r=respondida_em`, `V=validada_em`, `v=validada_por[/_id]`, `C=materializada_em`, `M=recusa_motivo`, `I=via_resolucao`, `D=data_conferida_em`, `F=fechada_em`.

```text
app/api/foto_recebida.py:66,78 [F]
app/api/views.py:1018,1034,2678 [F]; :1163,2561 [R]; :1165,2563,2577,2620,2637 [r]
app/api/views_mensageria.py:567 [R]; :569,570 [r]
app/chamados/canal.py:65 [R]
app/chamados/juizes.py:292,683,711,858,1390,1502 [C]; :2466 [F];
 :683,695,857,1390 [I];
 :364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052 [R];
 :856,1389 [V]; :352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753 [r]
app/chamados/management/commands/auditar_chamados_orfaos.py:41 [F]
app/chamados/management/commands/auditar_disputas.py:71,150 [C]; :87,136,167 [F]
app/chamados/management/commands/auditar_invariantes_chamados.py:
 :470,538,588,905,955,1075,1156,1577,1604,1769 [C]; :592,806 [F];
 :202,206,433,542,955,1712 [I]; :536,1075,1112 [M]; :1106,1657 [R];
 :205,1075,1576,1604,1768,1780 [V]; :177,592,1105,1605 [r]; :1076 [v]
app/chamados/management/commands/backfill_decisoes_pergunta.py:101,325 [C];
 :101,314,330,335,336,398,504 [I]; :61,326,504 [R]; :56,60,102 [V]; :56,60 [r]; :60 [v]
app/chamados/management/commands/bug_par_103.py:66 [C,I]; :69 [M]; :68 [V,v]
app/chamados/management/commands/curar_perguntas_em_limbo.py:32,46 [R]; :46,69 [r]
app/chamados/management/commands/dedup_perguntas_fato.py:75 [R]
app/chamados/management/commands/encerrar_perguntas_com_fato.py:32 [C,V];
 :82 [F]; :33,42,49,66,75,76 [I]
app/chamados/management/commands/fechar_becos_disputa.py:32 [F]
app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147 [C]; :70,71,72 [I]
app/chamados/management/commands/reabrir_orfas_enterradas.py:67,88,91,129,142,160 [C];
 :96,131,164,165 [F]; :67,129,142,160 [I]; :68 [V]
app/chamados/management/commands/relavrar_via_i6.py:58,59,62,66,68,88 [I]
app/chamados/management/commands/validar_classe_a.py:80 [R]
app/chamados/models.py:
 :1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485 [C];
 :190,1638,1870,1888,1960,1964,2013,2017,2020,2112,2114,2219,2230,2235,2248,2254,2258,2305,2343,2778,2781 [F];
 :1535,2001,2010,2011,2136,2141,2829,2860,3165,3211,3236,3238,3296,3311,3320,3331,3365,3392,3393,3400,3402,3403,3411,3423,3440,3447,3483,3485,3652,3658 [I];
 :3298,3299 [M];
 :1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105 [R];
 :310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104 [V];
 :1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100 [r];
 :1614,1629,3237,3246,3247,3275,4105 [v]
app/chamados/reconciliador.py:367,630,1050,1145,1155,1505 [C];
 :412,600,611,612,1161,1162 [F]; :1144,1155 [I]; :365,1083 [R]; :631,1050 [V]
app/chamados/relatorios_disputa.py:46 [F]
app/chamados/services/acoes_chamado.py:346 [F]
app/chamados/services/acoes_disputa.py:257,349 [F]; :83 [M]; :355 [R]; :347 [V]; :97,132 [r]
app/chamados/services/becos.py:27,56,59 [F]; :30 [r]
app/chamados/services/competencia_trancada.py:176,221 [F]; :276,282 [I]
app/chamados/services/declaracao_texto.py:417 [R]; :227 [V]; :417,419,687 [r]
app/chamados/services/disputa_emissao.py:
 :228,776,1084,1104,1340,1637,1930,2062,2073,2090 [C];
 :41,541,992,1111,1347,1536,2026,2050,2052,2111,2113 [F];
 :337,409,428,446,1637,2072,2073,2087,2090 [I]; :1511,1513 [M];
 :1481,1483,1609,1635,1663,1674,1677,1692,1700,1861,2079 [R];
 :1484,1485,1494,1496,1537,1538,1609,1636,2062 [V];
 :1482,1483,1609,1635,1675,1677,1692,1696,1698,1703,1735,1843,1861,2074,2091 [r];
 :1484,1495,1496,1537,1538,1609,1636 [v]
app/chamados/services/explicador.py:118 [I]; :113 [M]; :55,102,103 [R]
app/chamados/services/fabrica_da_celula.py:171 [C]; :149 [I]
app/chamados/services/fila_por_causa.py:225,259,262 [F]
app/chamados/services/fio.py:131,646 [F];
 :1147,1214,1290,1397,1480,1921,1957,1979,2024 [I]; :564,1407 [M];
 :377,1279,1291,1613 [R]; :187,706,1613 [r]
app/chamados/services/foto_e4b.py:180 [C,V,r]; :182 [F]
app/chamados/services/historico.py:103 [C]; :109 [R]; :101 [V]; :99 [r]
app/chamados/services/materializacao.py:
 :148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127 [C];
 :169,1851 [D]; :1067,1989 [F]; :261,292,862,868,1108,1150,1652 [I];
 :1332,1333,1652,1655,1663,1665,1666 [M];
 :546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619 [R];
 :199,201,1152,2065,2082 [V]; :1151,2064 [r]; :1152 [v]
app/chamados/services/painel.py:67 [C,r]; :67,270,271,272,273 [V]; :262 [v]
app/chamados/services/passivo_validado.py:64,534,535,536 [C];
 :64,140,141,142,530,535 [I]; :66 [M,V,r,v]
app/chamados/services/pergunta_no_app.py:81 [F,r]
app/chamados/services/raiox.py:135,182 [C]; :138,147 [R,r]
app/chamados/services/regularizacao_ext.py:199,201,220 [C, outro modelo]; :60 [F]
app/chamados/services/relavra_via.py:181,244,252 [C]; :260 [F];
 :118,121,123,125,127,134,136,155,181,245,252 [I]
app/chamados/services/tarefa_do_dia.py:256,257 [I]
app/chamados/services/validacao.py:262,264,341,363,556,681,701,717 [C];
 :905,907,909 [D]; :182,288,291,293 [F]; :341,363 [I];
 :181,262,266,282,299,301,302,344,364,825,827,833,847 [M];
 :175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109 [R];
 :168,171,173,181,262,269,273,276,300,302,343,349,351,364,489,553,597,701,810,975,996 [V];
 :594,813,824,848,1167 [r]; :169,171,181,262,274,276,300,302,343,350,351,364,489 [v]
app/chamados/services/verbos.py:264,270 [V]; :335,336,342,345 [r]; :264,265,266,268 [v]
app/chamados/services/vigia_carimbo.py:98 [V]
app/chamados/services/vigia_fio.py:749 [R]; :636,746 [r]
app/chamados/signals.py:29,131,155 [F]
app/chamados/templatetags/chamados_tags.py:139,166 [R]
app/chamados/veredito_lavrado.py:203,204 [C]; :207,208 [r]
app/chamados/views.py:1257 [C]; :471,911,1329 [F]; :1280,1416 [r]
app/chamados/views_cobrar.py:161,164 [F]; :291 [R]; :169,176 [r]
app/colaboradores/painel_cell.

[truncado em 20000 de 57180 caracteres]

## 16/11 23:20 · sessao ad1e7f942ffb · PREPARA-F2-F4-F7 · censo · origem=Code

## 1 DEPENDE e premissas

**BASE examinada: cópia identificada no pedido como `520d7b55720888d80b46ffbca2ca73e59da40277`.** A cópia não contém `.git`; a confirmação independente do HEAD e de sua ancestralidade fica **[nao verificado]**. Os anexos foram fornecidos no enunciado; as referências abaixo usam sua numeração textual, contando o cabeçalho como linha 1. Não houve acesso a banco nem execução de testes **[nao verificado quanto aos resultados em execução]**.

**Achado principal:** o contrato F1 antigo do brief não corresponde ao contrato atual. A BASE declara `pergunta_respondida(p)` como juiz de instante **e** texto; declara também que os leitores B/C/D serão migrados na B2. `estado`, `respondida` e `resposta_display` ainda leem a projeção. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1550`; `app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`.)

Nesta tabela, **CAIU** significa que a premissa não pode orientar a F7 contra esta BASE; não significa que o texto descrevesse incorretamente sua cópia histórica.

| Origem | Marca ou premissa | Julgamento na BASE |
|---|---|---|
| brief:85–87,171 | `[DEPENDE-F1]`: medir U1/U2 somente depois de pousar a antiga `leitura` | **CAIU.** Há funções reais para medir projeção sem linha e identidade; a comparação entre os antigos folds puro/conservado permanece **[nao verificado]**. (`app/chamados/models.py:3197`; `app/chamados/models.py:4081`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`.) |
| brief:89 | `[DEPENDE-F1]`: escolher `leitura` pura ou `_leitura_conservada` | **CAIU como contrato disponível.** O contrato atual nomeia `pergunta_respondida`, sobre o par projetado. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1550`.) |
| brief:89–94 | `[DEPENDE-F1]`: accessor público de `LeituraDosAtos`; F2 já forneceria propriedades pelo ato | **CAIU.** As propriedades atuais continuam na projeção; a classificação B/C/D está destinada à B2. (`app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`; `app/chamados/juizes.py:1550`.) |
| brief:100 | `[DEPENDE-F1]`: juiz da matriz = `leitura(p)` | **CAIU.** Vitalidade lê `projecao_temporal_da_pergunta`; resposta usa `pergunta_respondida`. (`app/chamados/juizes.py:1382`; `app/chamados/juizes.py:1404`; `app/chamados/juizes.py:1423`.) |
| brief:116–119 | `[DEPENDE-F1]`: R1b deve escolher entre duas portas de fold | **CAIU como alternativa executável confirmada.** A conservação por correspondência existe, mas isso não prova a disponibilidade daqueles dois leitores. (`app/chamados/models.py:4081`; `app/chamados/models.py:4094`; `app/chamados/models.py:4102`.) |
| matriz:13–17,30 | `[DEPENDE-F1]`: assinatura e resultado `LeituraDosAtos` | **CAIU.** O contrato pousado descrito pelo código é o juiz L-130. (`app/chamados/juizes.py:1544`; `app/docs/LEIS.md:151`.) |
| matriz:38 — C1 | `[DEPENDE-F1]`: vitalidade passaria a ler o ato | **CAIU.** V ainda encerra no primeiro degrau. (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1405`.) |
| matriz:44 — C2 | `[DEPENDE-F1]`: `resposta_enviada` viria de instantes/texto do fold | **CAIU.** Seu escritor continua negando `espera_o_texto_do_colaborador`, que consulta I e o predicado de resposta sem texto. (`app/chamados/services/fio.py:187`; `app/chamados/services/fio.py:359`.) |
| matriz:50,55,58,64,67,75,83 — C3/C4/C5/C7/C8/C10/C12 | Cada `[DEPENDE-F1]`: texto, data e prefill pelo ato | **CAIU como dependência satisfeita.** As leituras cruas continuam nos mesmos pontos. (`app/templates/chamados/partials/_card_atendimento.html:106`; `app/templates/chamados/partials/_card_atendimento.html:114`; `app/templates/chamados/partials/_card_atendimento.html:143`; `app/templates/chamados/partials/_card_atendimento.html:148`; `app/templates/chamados/partials/_card_atendimento.html:154`; `app/templates/chamados/partials/_card_atendimento.html:182`; `app/templates/chamados/partials/_card_atendimento.html:391`.) |
| matriz:79 — C11 | `[DEPENDE-F1]`: V pelo ato e estado herdado da F2 | **CAIU como implementação presente.** O estado continua dando precedência a V/materialização. (`app/chamados/models.py:3151`; `app/chamados/models.py:3156`.) |
| matriz:88 — P0 | `[DEPENDE-F1]`: F1 faria U2 entrar na lista | **CAIU.** O juiz atual ainda mata por V; o detalhe filtra por esse juiz. (`app/chamados/juizes.py:1405`; `app/chamados/services/acoes_chamado.py:358`.) |
| matriz:93–97,100 — P1/P2/P3/P4/P5/P6 | Cada `[DEPENDE-F1]`: fase, texto, instante e gate pelo fold | **CAIU como migração presente.** I/V continuam literais no template. (`app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:24`; `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/_pergunta_card.html:32`; `app/templates/chamados/partials/_pergunta_card.html:42`.) |
| matriz:103 — P7 | `[DEPENDE-F1]`: filtro leria texto do ato | **CAIU.** O filtro lê R por `getattr` e chama a classificação atual. (`app/chamados/templatetags/chamados_tags.py:166`; `app/chamados/templatetags/chamados_tags.py:171`.) |
| matriz:107 — M0 | `[DEPENDE-F1]`: universo pelo ato; prefetch disponível | **CAIU quanto ao ato; CONFERE quanto ao prefetch.** (`app/chamados/juizes.py:1405`; `app/chamados/services/fio.py:263`.) |
| matriz:109,113,114 — M1/M3/M4 | Cada `[DEPENDE-F1]`: I/R/V pelo fold | **CAIU como migração presente.** (`app/templates/chamados/partials/modal_fio.html:185`; `app/templates/chamados/partials/modal_fio.html:217`; `app/templates/chamados/partials/modal_fio.html:219`; `app/templates/chamados/partials/modal_fio.html:220`; `app/templates/chamados/partials/modal_fio.html:228`.) |
| matriz:115,117 — M5/M6 | Cada `[DEPENDE-F1]`: aplicação/frase derivadas do contrato antigo | **CAIU como contrato.** Aplicação vem da testemunha real; frase ainda depende da projeção. (`app/chamados/services/fio.py:376`; `app/chamados/templatetags/chamados_tags.py:166`; `app/chamados/templatetags/chamados_tags.py:171`.) |
| matriz:123 — L1 | `[DEPENDE-F1]`: preview pelo texto do ato | **CAIU como migração presente.** A célula continua lendo R. (`app/templates/chamados/partials/_lote_preview.html:24`.) |
| matriz:130,133,135 | Marcas do contrato de saída: accessor único antigo, universo pelo ato, R1b puro/conservado | **CAIU quanto à API pressuposta.** Manter um juiz único continua decidido; a API antiga não pode ser presumida. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`; `app/chamados/services/acoes_chamado.py:358`.) |
| matriz:147 | Contagem dos 25 degraus `[DEPENDE-F1]` | **CAIU como conjunto liberado pelo antigo fold:** os 25 destinos precisam ser relidos pelo contrato atual. A B2 está explicitamente encarregada dos leitores B/C/D. (`app/chamados/juizes.py:1550`; `app/docs/LEIS.md:151`.) |

As premissas sobre a BUG-PAR ficam assim:

| Origem | Premissa | Julgamento na BASE |
|---|---|---|
| brief:15; matriz:8–10 | Os quatro templates continuam iguais ao censo; BUG-PAR está fora da base | **CAIU como premissa atual.** Card do colaborador e lote conservam os pontos antigos; pergunta/modal receberam novos gates. A ancestralidade dos hashes antigos é **[nao verificado]**. (`app/templates/chamados/partials/_card_atendimento.html:106`; `app/templates/chamados/partials/_lote_preview.html:24`; `app/templates/chamados/partials/_pergunta_card.html:43`; `app/templates/chamados/partials/modal_fio.html:230`.) |
| brief:77–78,149; matriz:103 | `chamados_tags.py` é compartilhado com BUG-PAR | **CONFERE como dependência atual.** Contém os adaptadores da espera e da aplicação. (`app/chamados/templatetags/chamados_tags.py:121`; `app/chamados/templatetags/chamados_tags.py:132`.) |
| brief:149 | Card ganharia apenas `{% with esp=perg\|espera_declarado_x_gravado %}` envolvendo o gate | **CAIU.** O código atual usa `ap=perg|aplicacao_da_resposta`, além da leitura da frase. (`app/templates/chamados/partials/_pergunta_card.html:43`.) |
| brief:149; matriz:100 | Colisão no gate exterior de Validar do card | **CONFERE.** V continua sendo o gate exterior; os novos impedimentos estão dentro dele. (`app/templates/chamados/partials/_pergunta_card.html:42`; `app/templates/chamados/partials/_pergunta_card.html:44`.) |
| brief:149,156–158; matriz:117 | Colisão no ramo “Validada” e nas ações do modal | **CONFERE**, com escopo maior: há ramos de aplicação sob V e sem V, além do gate das ações. (`app/templates/chamados/partials/modal_fio.html:228`; `app/templates/chamados/partials/modal_fio.html:230`; `app/templates/chamados/partials/modal_fio.html:252`; `app/templates/chamados/partials/modal_fio.html:274`.) |
| brief:149 | BUG-PAR altera contexto do fio | **CONFERE.** A testemunha e o fallback indisponível são anexados à pergunta. (`app/chamados/services/fio.py:376`; `app/chamados/services/fio.py:377`; `app/chamados/services/fio.py:384`.) |
| brief:95–96; matriz:87–91,134 | Detalhe/fragmento sem prefetch; modal já tem | **CONFERE.** O custo adicional de um accessor futuro é **[nao verificado]**. (`app/chamados/services/acoes_chamado.py:352`; `app/chamados/views.py:399`; `app/chamados/services/fio.py:263`.) |
| brief:80–87 | Todos os escritores mantêm concordância; divergência só no legado | **CAIU como conclusão universal.** `carimbar` também preenche I/V e salva, sem registrar decisão nesse corpo. Ocorrência divergente em produção: **[nao verificado]**. (`app/chamados/models.py:3242`; `app/chamados/models.py:3245`; `app/chamados/models.py:3262`.) |
| brief:175–176; matriz:136 | Template e Python precisam pousar juntos | **CONFERE como regra escrita de publicação.** Execução de deploy nesta tarefa: nenhuma **[nao verificado por registro externo]**. (`CLAUDE.md:90`.) | [nao verificado]

## 2 Censo escritores e leitores

R = `resposta_colab`; I = `respondida_em`; V = `validada_em`; autor = `validada_por`. A separação entre projeção e sequência está declarada no model. (`app/chamados/models.py:4007`; `app/chamados/models.py:4011`.)

**NOVO** identifica sítio ou consumidor ausente do censo do brief, inclusive dentro de arquivo que o brief já mencionava.

| Escritor | Escrita e conservação | Delta |
|---|---|---|
| `responder_pergunta_disputa_core` | Escreve R/I, desfaz V/autor e registra RESPOSTA. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1525`.) | **NOVO** |
| `reabrir_para_correcao` | Limpa os quatro campos; conserva snapshot/decisão e usa a porta de lote. (`app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1648`.) | Já listado |
| `responder_automaticamente` | Conserva projeção anterior, escreve R/I e RESPOSTA. (`app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/disputa_emissao.py:1674`; `app/chamados/services/disputa_emissao.py:1678`.) | **NOVO** |
| `carimbar_resposta_pela_trilha` | Preenche I e registra RESPOSTA. (`app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1699`.) | **NOVO** |
| `_carimbar_validacao` | Escreve V/autor e VALIDACAO no mesmo atomic. (`app/chamados/services/validacao.py:167`; `app/chamados/services/validacao.py:168`; `app/chamados/services/validacao.py:170`; `app/chamados/services/validacao.py:173`.) | **NOVO** |
| `devolver_validacao_recusada` | Limpa V/autor e registra DEVOLUCAO. (`app/chamados/services/validacao.py:272`; `app/chamados/services/validacao.py:273`; `app/chamados/services/validacao.py:279`.) | Já listado; linhas mudaram |
| `devolver_paridade_bug_par_103` | Consulta K6 antes da escrita; limpa V/autor e registra DEVOLUCAO. (`app/chamados/services/validacao.py:338`; `app/chamados/services/validacao.py:349`; `app/chamados/services/validacao.py:354`.) | **NOVO** |
| `ressuscitar_perguntas` | Conserva linhas anteriores e zera o par inteiro pela porta de lote, sem REABERTURA. (`app/chamados/models.py:1613`; `app/chamados/models.py:1615`; `app/chamados/models.py:1629`.) | **NOVO** |
| `PerguntaDisputa.carimbar` | Pode preencher I e V/autor antes do `save`; seu rollback restaura memória. (`app/chamados/models.py:3242`; `app/chamados/models.py:3245`; `app/chamados/models.py:3262`; `app/chamados/models.py:3273`.) | **NOVO** |
| `descarimbar_em_lote` | Valida a composição do par e escreve os campos recebidos em `outros`. (`app/chamados/models.py:3370`; `app/chamados/models.py:3383`; `app/chamados/models.py:3391`.) | **NOVO**, infraestrutura |
| Nascimento da pergunta | Salva os campos de entrada e conserva projeção sem linha. (`app/chamados/models.py:3124`; `app/chamados/models.py:3127`; `app/chamados/models.py:3129`.) | **NOVO**, infraestrutura |
| `DecisaoPergunta.registrar` | Porta material da sequência, por `bulk_create`. (`app/chamados/models.py:4110`; `app/chamados/models.py:4120`.) | Porta a acompanhar |

Os caminhos de entrada também precisam acompanhar esses escritores:

| Porta | Chamadores de produção encontrados |
|---|---|
| Resposta manual | `app/api/views.py:2627`; `app/chamados/views.py:1283`; **NOVO** `app/chamados/services/declaracao_texto.py:419`; `app/chamados/services/declaracao_texto.py:693`; `app/chamados/services/acoes_disputa.py:373` |
| Resposta automática/trilha | **NOVO** `app/api/foto_recebida.py:48`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:67` |
| Reabertura/recusa | `app/chamados/models.py:2318`; `app/chamados/views.py:1436`; `app/chamados/services/acoes_disputa.py:105`; `app/chamados/services/materializacao.py:1164`; `app/chamados/services/validacao.py:836`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69` |
| Carimbo da pergunta | **NOVO** `app/chamados/reconciliador.py:406`; `app/chamados/reconciliador.py:596`; `app/chamados/models.py:3414`; `app/ponto/services/cartorio.py:1314`; `app/ponto/services/cartorio.py:1873`; `app/chamados/services/materializacao.py:1043`; `app/chamados/services/materializacao.py:1944`; `app/chamados/services/materializacao.py:2139`; `app/chamados/services/disputa_emissao.py:2008`; `app/chamados/services/disputa_emissao.py:2082` |
| Devolução BUG-PAR | **NOVO** `app/chamados/management/commands/bug_par_103.py:106` |

Os homônimos `carimbar` das APIs de tokens foram excluídos: importam `api.credencial`, não a porta de pergunta. (`app/api/views.py:23`; `app/api/views_core.py:22`.)

O grep/AST ampliado encontrou os seguintes sítios de leitura, seleção ou transporte de R/I/V/autor. Strings de auditoria e nomes de campos são candidatos estáticos, não necessariamente julgamento; resolução de acessos inteiramente dinâmicos permanece **[nao verificado]**.

| Arquivo | Sítios BASE |
|---|---|
| Model/juízes | `app/chamados/models.py:310`, `:344`, `:1533`, `:1923`, `:2007`, `:2139`, `:2949`, `:3151`, `:3237`, `:3331`, `:3370`, `:3496`, `:3568`, `:3584`, `:3665`, `:3688`, `:4091`; `app/chamados/juizes.py:352`, `:364`, `:856`, `:1360`, `:1389`, `:1423`, `:1534`, `:1541`, `:1552`, `:1572`, `:1580`, `:1744`, `:1753`, `:1779`, `:1926`, `:2030`, `:2035`, `:2052` |
| Views/contexto conhecidos | `app/chamados/views.py:1280`, `:1416`; `app/chamados/views_cobrar.py:169`, `:176`, `:291`; `app/chamados/services/fio.py:187`, `:377`, `:706`, `:1279`, `:1291`, `:1613`; `app/chamados/templatetags/chamados_tags.py:139`, `:166` |
| Escritores também leitores — **NOVOS sítios** | `app/chamados/services/validacao.py:173`, `:181`, `:262`, `:269`, `:343`, `:380`, `:489`, `:553`, `:594`, `:597`, `:701`, `:810`, `:823`, `:843`, `:975`, `:996`, `:1094`, `:1167`; `app/chamados/services/disputa_emissao.py:1484`, `:1537`, `:1635`, `:1663`, `:1692`, `:1700`, `:1735`, `:1843`, `:1861`, `:2062`, `:2074`, `:2091` |
| Materialização — **NOVOS sítios** | `app/chamados/services/materializacao.py:199`, `:201`, `:546`, `:574`, `:625`, `:671`, `:678`, `:718`, `:733`, `:770`, `:869`, `:950`, `:1047`, `:1109`, `:1150`, `:1311`, `:1352`, `:1534`, `:1558`, `:1567`, `:1619`, `:2064`, `:2082` |
| **NOVO** ações/declarador | `app/chamados/services/acoes_disputa.py:97`, `:132`, `:347`, `:355`; `app/chamados/services/declaracao_texto.py:227`, `:417`, `:687`; `app/chamados/services/becos.py:30` |
| **NOVO** explicação/histórico/foto | `app/chamados/services/explicador.py:55`, `:102`; `app/chamados/services/historico.py:99`, `:101`, `:109`; `app/chamados/services/foto_e4b.py:180`; `app/chamados/services/raiox.py:138`, `:147` |
| **NOVO** filas/app | `app/chamados/services/painel.py:67`, `:262`, `:270`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/verbos.py:264`, `:335`; `app/chamados/canal.py:65` |
| **NOVO** reparo/vigias | `app/chamados/services/passivo_validado.py:66`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/vigia_fio.py:636`, `:746`, `:749`; `app/chamados/reconciliador.py:365`, `:631`, `:1050`, `:1083`; `app/chamados/veredito_lavrado.py:207` |
| **NOVO** APIs | `app/api/views.py:1163`, `:2561`, `:2577`, `:2620`, `:2637`; `app/api/views_mensageria.py:567`, `:569` |
| **NOVO** colaboradores/folha | `app/colaboradores/services/calendario.py:504`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142` |
| **NOVO** ponto | `app/ponto/services/cartorio.py:1641`; `app/ponto/services/dia_do_colab.py:70`; `app/ponto/services/flip_auto.py:130`; `app/ponto/services/procedencia_batida.py:71`; `app/ponto/services/veredito_celula.py:197`; `app/ponto/management/commands/reconciliar_grade.py:461` |
| **NOVO** auditoria | `app/chamados/management/commands/auditar_invariantes_chamados.py:177`, `:205`, `:592`, `:1075`, `:1105`, `:1576`, `:1604`, `:1657`, `:1768`, `:1780` |
| **NOVO** comandos de acervo | `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`, `:60`, `:102`, `:326`, `:504`; `app/chamados/management/commands/bug_par_103.py:68`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:32`, `:46`, `:69`; `app/chamados/management/commands/dedup_perguntas_fato.py:75`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:68`; `app/chamados/management/commands/validar_classe_a.py:80` |
| **NOVO** sondas existentes | `bin/censo_folha.py:28`; `bin/censo_folha.py:29`; `bin/sombra_regras_pessoais.py:85` |

Para as funções e propriedades intermediárias, o grafo atual é:

| Campo/função | Escritor ou autoridade → leitores |
|---|---|
| `viva` / `estado` / `respondida` / `resposta_display` | `viva` delega; os outros três ainda consultam projeção. (`app/chamados/models.py:3512`; `app/chamados/models.py:3151`; `app/chamados/models.py:3665`; `app/chamados/models.py:3688`.) Os leitores de template estão integralmente na seção TEMPLATES. |
| `resposta_enviada` | Escritor `fio.py:359`; leitores card:100,143,161,390. A função subjacente também é lida pela cobrança. (`app/chamados/services/fio.py:359`; `app/chamados/services/fio.py:174`; `app/chamados/services/cobranca.py:241`; `app/templates/chamados/partials/_card_atendimento.html:100`; `:143`; `:161`; `:390`.) |
| `estado_aplicacao` | Escritor de apresentação `fio.py:376`, fallbacks:377/384; leitores modal:230–274. **NOVO** adaptador equivalente no filtro do card. (`app/chamados/services/fio.py:376`; `:377`; `:384`; `app/chamados/templatetags/chamados_tags.py:132`; `app/templates/chamados/partials/modal_fio.html:230`; `:274`.) |
| `estado_aplicacao_pergunta` | Leitores diretos: contador, espera, explicador, filtro e fio. (`app/chamados/services/materializacao.py:339`; `:757`; `app/chamados/services/explicador.py:124`; `app/chamados/templatetags/chamados_tags.py:136`; `app/chamados/services/fio.py:376`.) |
| `espera_declarado_x_gravado` | Leitores: filtro, validação e explicador. (`app/chamados/templatetags/chamados_tags.py:127`; `app/chamados/services/validacao.py:611`; `app/chamados/services/explicador.py:132`.) |
| `l

[truncado em 20000 de 58269 caracteres]

## 16/11 23:13 · sessao a9ed8c02f370 · - · - · origem=Ronald (tmux)

Numeração dos anexos contada por transcrição integral em strings em memória; nenhum arquivo escrito. `brief.md` tem 186 linhas e `matriz.md` 136 linhas nesta transcrição; equivalência com arquivos locais dos anexos = [nao verificado].

| Origem | Julgamento | Evidência na BASE |
|---|---|---|
| brief:38 — F1 sem ramo absorção | **CAIU como dependência do contrato D01**: a F1 real é `pergunta_respondida`, sem leitura de atos. **CONFERE** permanecer no carimbo, por corte posterior. | `app/chamados/juizes.py:1544`; `app/chamados/services/materializacao.py:1105`; `app/docs/CORTES.md:487` |
| brief:106-107 — assinatura `leitura(...decisoes,agora,materializada_conservada_em)` | **CAIU**: o contrato positivo pousado é `pergunta_respondida(p)` → booleano I∧T; não é assinatura substituída por outra versão de D01. | `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1552` |
| brief:117 — carga fornece decisões ao juiz e texto D01 | **CAIU**: carga mantém `agora` e `slots`, mas chama `_slot_esperado(p,carga=self)`; juiz lê `p.resposta_colab` ou passa `p` real ao derivador. | `app/chamados/services/carga_pergunta_viva.py:88`; `app/chamados/services/carga_pergunta_viva.py:119`; `app/chamados/juizes.py:364`; `app/chamados/juizes.py:376` |
| brief:137 — ordem `(ato_em,pk)` “do F1” | **CONFERE na sequência**, **CAIU atribuição ao D01/F1 pousado**. | `app/chamados/models.py:4056`; `app/chamados/juizes.py:1544` |
| brief:144 — teste de absorção não servido pelo D01 | **CAIU contrato D01**; permanência do leitor por carimbo **CONFERE pelo corte**, sem necessidade de perguntar de novo. | `app/chamados/services/materializacao.py:1105`; `app/docs/CORTES.md:487` |
| brief:162-164 — absorção ainda pergunta de lei e sem ato que desfaz | **CAIU pergunta aberta**: corte manda M9b/M10 permanecer no carimbo. A devolução efetivamente registra REABERTURA antes de limpar a projeção. | `app/docs/CORTES.md:487`; `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/services/disputa_emissao.py:1648` |
| matriz:20,22-28 — contrato de entrada D01 completo | **CAIU**: juiz real I∧T e projeção temporal, sem `LeituraDosAtos`. Ordem da sequência **CONFERE**, mas não prova o redutor descrito. | `app/chamados/juizes.py:1382`; `app/chamados/juizes.py:1544`; `app/chamados/models.py:4056` |
| matriz:40 M7 — decisões/agora da F1 no estado de aplicação | **CAIU**: testemunha recebe `(disputa,p)` e consulta projeção/derivador. | `app/chamados/services/materializacao.py:852`; `app/chamados/services/materializacao.py:869`; `app/chamados/services/materializacao.py:873` |
| matriz:43 M9b — “ninguém lê ABSORCAO hoje” | **CAIU**: tarefa lê ABSORCAO e status da ausência, retornando quatro estados específicos. | `app/chamados/services/tarefa_do_dia.py:178` |
| matriz:44 M10 — absorção como pergunta de lei dependente F1 | **CAIU pergunta aberta**; **CONFERE leitor mantido no carimbo**, por corte. | `app/docs/CORTES.md:487`; `app/chamados/services/materializacao.py:1105` |
| matriz:72 E9 — REABERTURA alimenta D01 | **CONFERE escrita da REABERTURA**, **CAIU leitor D01**. | `app/chamados/services/disputa_emissao.py:1631`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1544` |
| matriz:91 J1 — embrulho `_PerguntaParaPlantio` na F1 pousada | **CAIU**: juiz passa `p` diretamente; ramo data_hora lê projeção. | `app/chamados/juizes.py:364`; `app/chamados/juizes.py:376` |
| matriz:94-99 — divergência instalada juiz D01 × materializador projeção | **CAIU na BASE**: juiz e materializador continuam recebendo projeção; a obrigação documental de ordem/contador permanece escrita. | `app/chamados/juizes.py:376`; `app/chamados/services/materializacao.py:895`; `app/docs/CORTES.md:487` |
| matriz:106 — leitores do núcleo passam a `leitura().resposta` | **CAIU suporte da F1 pousada**. O contrato vigente define um juiz de “respondida”, sem decidir por si só que texto do último ato substitui a projeção. | `app/chamados/juizes.py:1544`; `app/docs/LEIS.md:151` |
| matriz:107-108 — fila M23 por D01 | **CAIU suporte D01**; fila atual documenta R/V/C da projeção. | `app/chamados/services/materializacao.py:2032`; `app/chamados/services/materializacao.py:2035` |
| matriz:110 — três chamadores com decisões/agora D01 | **CAIU nesse formato**: juiz continua chamando o derivador com pergunta real e carga opcional. | `app/chamados/juizes.py:376` |
| matriz:121-122 — lei de absorção aberta | **CAIU**: respondida no adendo matriz:133-136 e no corte da BASE. | `app/docs/CORTES.md:487` |
| matriz:131 — contagem das marcas | É resumo editorial, não dependência adicional; contagem de marcas atuais do contrato = [nao verificado]. | [nao verificado] |

Premissas BUG-PAR:

- **brief:35;148-153 e matriz:40;48 — CONFERE que a BUG-PAR reescreveu a testemunha e introduziu leitores**: `_estado_pelo_derivador`, K6 da pergunta/irmã, par projetado e classificação do contador existem na BASE. Os offsets e identidade do diff `6e2fc9133` **CAÍRAM como descrição da versão pousada**; o RELATO registra c4+c4c e os estados L1/L5 estão implementados. (`app/chamados/services/materializacao.py:876`; `app/chamados/services/materializacao.py:885`; `app/chamados/services/materializacao.py:907`; `app/chamados/services/materializacao.py:339`; `app/docs/RELATO.md:5`)
- **brief:152-153;184 e matriz:12-14 — CONFERE a necessidade de recensear depois da BUG-PAR**; premissa “BUG-PAR fora do main/aguardando” **CAIU**, pois a BASE registra pouso. Comparação histórica dos commits sem `.git` = [nao verificado]. (`app/docs/RELATO.md:5`)
- **brief:156-157 — CAIU F1 como adição de `atos_pergunta`/embrulho**, embora **CONFIRA** o vínculo de chamada juiz → `_ts_tipo_do_plantio`. (`app/chamados/juizes.py:376`; `app/chamados/juizes.py:1544`)

Leis:

- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 3 | contestavel**: absorção M9b/M10 permanece no carimbo na F4; predicado por ato pertence à E4d. (`app/docs/CORTES.md:487`)
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 4 | contestavel**: guardas E11/E14/E17 permanecem na projeção por allowlist exata; saída futura na E4d com lápide. (`app/docs/CORTES.md:487`)
- **DECIDIDO PELA LEI L-130 | contestavel**: respondida exige instante e texto após trim; os dois meios pares são ilegais; reabrir/ressuscitar limpa o par. Isso não fornece sozinho a antiga semântica D01. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`)
- As perguntas sobre absorção e limbo do brief:162-169 / matriz:121-124 não sobram. A condição de pouso/J1 continua documentada, mas o estado atual da costura tem de ser reconferido depois da B2. (`app/docs/CORTES.md:487`; `app/chamados/juizes.py:376`)
- A questão “ressuscitar devolve ao colaborador” está registrada como não decidida, porém fora deste pedido de leitura da F4; não ampliar a lista de lei com ela. (`app/docs/LEIS.md:151`)

PRE-BACKFILL:

- Mora em `DecisaoPergunta.linhas_da_projecao_sem_linha`, **`app/chamados/models.py:4081`**. É leitura que devolve dicionários; exige correspondência **pergunta + ato + instante**, sem esconder resposta anterior atrás de qualquer RESPOSTA existente. Produz RESPOSTA para `respondida_em` e VALIDACAO para `validada_em`, preservando autor da validação. (`app/chamados/models.py:4091`; `app/chamados/models.py:4094`; `app/chamados/models.py:4098`; `app/chamados/models.py:4102`)
- O medidor real `linhas_a_criar()` usa esse helper, mas mede horizonte/sem dia/conversa, distribuindo competência pela função real; portanto precisa distinguir esse recorte do total integral da frota. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:44`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:55`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:79`)
- **O command atual não aplica as linhas R/V do item 1**: o texto declara outro ato, com aval próprio; `--aplicar` cobre itens 2 e 4. Não dizer que executar `backfill_decisoes_pergunta --aplicar` satisfaz esse pré-requisito. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:3`)
- Exigência verificável antes de trocar um leitor por atos: zerar as faltas correspondentes no universo efetivamente migrado, incluindo re-respostas posteriores à reabertura; cobertura atual e necessidade quantitativa = [nao verificado]. A função conserva projeção vigente e não reconstrói texto já sobrescrito. (`app/chamados/models.py:4084`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:12`)
- Números do brief são históricos anexados; números atuais = [nao verificado]. As duas sondas necessárias são helper direto em lotes sobre todas as perguntas e `linhas_a_criar()` para o recorte oficial; nenhuma precisa executar o `Command.handle`, que no DRY faz simulação de apply com escrita/rollback. (`app/chamados/models.py:4081`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:578`)

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

_6221 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
