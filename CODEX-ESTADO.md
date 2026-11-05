# CODEX-ESTADO

> Ultima resposta processada: **04/11/2026 15:44** · publicado em **05/11/2026 11:03** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 04/11 15:44 · sessao a3f628326e9b · E4-APP-MAPA · verificador · origem=Code

**Parecer: há um defeito demonstrável no payload:** dia sem batidas lavrado `irregular` retorna `status='alerta'`, mas `inconsistente=False`; com batidas, o mesmo alerta produz `inconsistente=True`. Severidade **média**; efeito na tela compilada **[nao verificado]**. (`app/api/views.py:1518`, `app/api/views.py:1599`, `app/api/views.py:1615`)

**1. Quem escreve e quem lê**

| Chave | Escritor no payload |
|---|---|
| `status` | Tradução do D4 pelo adaptador, nos dois ramos. (`app/api/views.py:1515`, `app/api/views.py:1516`, `app/api/views.py:1599`, `app/api/views.py:1600`) |
| `status_dia` | Palavra original do D4, nos dois ramos. (`app/api/views.py:1562`, `app/api/views.py:1618`) |
| `falta` | Somente no card sem batidas: `_st == 'falta'`. O dict com batidas não inclui essa chave. (`app/api/views.py:1543`, `app/api/views.py:1620`) |
| `inconsistente` | Com batidas: turno aberto ou alerta; sem batidas: `False` literal. **Achado médio acima.** (`app/api/views.py:1518`, `app/api/views.py:1559`, `app/api/views.py:1615`) |
| `turno_aberto` | Com batidas: geometria de `turnos_do_colab`; sem batidas: `False`. (`app/api/views.py:1449`, `app/api/views.py:1514`, `app/api/views.py:1560`, `app/api/views.py:1616`) |
| `ausencia_pendente` | Cobertura para cobrança, comparada com `ABERTAS`, aplicada aos dois ramos. (`app/api/views.py:1575`, `app/api/views.py:1582`, `app/api/views.py:1584`, `app/api/views.py:1592`, `app/api/views.py:1625`) |
| `previsto_inicio`, `previsto_fim`, dois intervalos | Somente sem batidas: `eh_dia_trabalho` determina se haverá horários; o template fornece os quatro marcos. (`app/api/views.py:1595`, `app/api/views.py:1597`, `app/api/views.py:1621`) |

As leituras internas relevantes são `status → inconsistente` e `_st → falta`; a resposta entrega a lista construída localmente. (`app/api/views.py:1518`, `app/api/views.py:1620`, `app/api/views.py:1628`, `app/api/views.py:1675`)

Não encontrei consumidor de produção desse payload em `static/`, `templates/` ou outro endpoint por busca textual; leitores indiretos **[nao verificado]**. A rota é `api/urls.py:81`; a v1 apenas responde 410 com endereço da v2, sem reutilizar seu dict. (`app/api/urls.py:81`, `app/api/views.py:839`)

O HTML do espelho consome outro dict, proveniente de `_esp['dias']`; suas leituras de `status`, `inconsistente`, `turno_aberto` e previsto não comprovam compatibilidade com a API. (`app/ponto/views.py:157`, `app/ponto/views.py:193`, `app/templates/ponto/espelho.html:151`, `app/templates/ponto/espelho.html:166`, `app/templates/ponto/espelho.html:193`, `app/templates/ponto/espelho.html:207`)

Os leitores de teste dessas chaves incluem mapa/status/falta, geometria, pendência/previsto e isenção. (`app/api/tests/test_e4_app_mapa.py:83`, `app/api/tests/test_e4_app_mapa.py:90`, `app/api/tests/test_espelho_app_fonte_e_geometria.py:338`, `app/api/tests/test_espelho_app_ausencia_pendente_aditivo.py:16`, `app/escala/tests/test_art62_nunca_acusa.py:221`)

Não encontrei fonte nativo pelas extensões usuais; comportamento do compilado **[nao verificado]**. A casca aponta para um aplicativo da Play Store. (`app/templates/core/gate_apk.html:6`)

Há mudanças semânticas deliberadas: falta automática sem decisão passa a neutro; entrada antiga sem saída pode conservar `turno_aberto=True` com status neutro; `aberto` também pode significar dia em andamento com saídas registradas. Dependência do compilado nesses sentidos antigos **[nao verificado]**. (`app/api/tests/test_espelho_app_ausencia_pela_autoridade.py:61`, `app/api/tests/test_espelho_app_fonte_e_geometria.py:338`, `app/ponto/status_dia.py:283`)

**2. Vocabulário e possíveis KeyErrors**

O conjunto produzido pelo adaptador é exatamente `{ok, falta, previsto, folga, neutro, alerta, aberto}`; suas entradas cobrem as onze palavras do núcleo. (`app/ponto/status_dia.py:79`, `app/ponto/status_dia.py:86`)

Não há omissão de chave por vínculo, desligamento, futuro ou ausência de escala: a view solicita o mês inteiro e o D4 escreve uma resposta para cada par. Sem escala, fatos, batidas ou lavra, chega a `sem_dados`. Isso prova completude das chaves quando a execução termina normalmente, não correção de cadastro residual. (`app/api/views.py:1497`, `app/ponto/status_dia.py:255`, `app/ponto/status_dia.py:269`, `app/ponto/status_dia.py:319`, `app/ponto/precedencia.py:385`)

**Existe caminho condicional para 500:** o D4 repassa `cel.status_dia` persistido sem validar sua pertença ao vocabulário. Palavra inválida com rótulo preenchido chega ao adaptador e causa `KeyError`; sem rótulo, pode quebrar antes em `ROTULO_STATUS`. Os escritores normais derivam de regras fechadas; existência desse dado inválido em produção **[nao verificado]**. (`app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:289`, `app/ponto/status_dia.py:91`, `app/ponto/portas/celula.py:686`)

**3. Relógio**

O instante de julgamento é compartilhado: `agora` nasce na view e é repassado ao espelho, ao D4 e à regra de encerramento; o D4 só chama `now()` quando não recebe parâmetro. (`app/api/views.py:1350`, `app/api/views.py:1369`, `app/api/views.py:1498`, `app/ponto/status_dia.py:252`, `app/ponto/turnos.py:2164`)

Entretanto, há leitura de relógio fora da borda, **herdada**: carga → pareamento → interpretação da ata → `intervalo_cadastrado` → `date.today()`. Efeito dessa leitura sobre o status retornado **[nao verificado]**. (`app/ponto/turnos.py:2995`, `app/ponto/turnos.py:1937`, `app/ponto/turnos.py:1631`, `app/ponto/juiz_batida.py:270`, `app/ponto/juiz_batida.py:125`)

**4. Custo**

A carga acrescenta **cinco consultas básicas**, com consultas condicionais para geradoras ausentes e última batida. Não são cinco consultas por dia. (`app/ponto/turnos.py:2943`, `app/ponto/turnos.py:2949`, `app/ponto/turnos.py:2953`, `app/ponto/turnos.py:2955`, `app/ponto/turnos.py:2982`, `app/escala/alimentacao.py:123`, `app/ponto/turnos.py:2962`)

Quando algum dia precisa da natureza, A2 calcula o mês inteiro uma vez por colaborador: aproximadamente mais cinco consultas básicas com vínculos, além das condicionais e relações carregadas sob demanda. (`app/ponto/status_dia.py:303`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:282`, `app/ponto/precedencia.py:289`, `app/ponto/precedencia.py:319`, `app/ponto/precedencia.py:333`)

**Achado médio de desempenho: existe N+1 por dia comum.** Sem célula com `trabalha` definido, A2 chama a aritmética da escala; em ciclos aplicáveis, cada dia previsto consulta feriado, mesmo quando não é feriado. Há ainda consulta por feriado para seu nome. Portanto, “custo constante para 31 dias” seria incorreto. (`app/ponto/precedencia.py:359`, `app/ponto/precedencia.py:368`, `app/escala/models.py:1340`, `app/escala/models.py:1363`, `app/core/feriados.py:95`, `app/ponto/precedencia.py:407`)

Também há repetição de CPU: para cada data, o D4 pede novamente o encerramento da janela inteira — cerca de 31 varreduras de 31 dias; os turnos permanecem em cache. (`app/ponto/status_dia.py:274`, `app/ponto/turnos.py:3004`, `app/ponto/turnos.py:2158`, `app/ponto/turnos.py:2987`)

O bloco removido custava uma consulta básica de grade, mas também podia consultar por dia quando faltavam minutos previstos na ata. Saldo exato e latência por chamada **[nao verificado]**. (`app/escala/services/leitor_celula.py:350`, `app/escala/services/leitor_celula.py:376`)

**5. Classes de reprovação**

- **Segundo juiz: não identificado no adaptador.** É tradução por tabela; o juiz registrado continua sendo `status_do_dia_lavrado`, que delega ao lote. (`app/ponto/status_dia.py:79`, `app/ponto/status_dia.py:89`, `app/core/juizes.py:187`, `app/ponto/status_dia.py:217`)
- **Segundo escritor persistente: não identificado no trecho alterado.** A view monta payload; a atribuição à célula continua na porta. (`app/api/views.py:1543`, `app/api/views.py:1603`, `app/ponto/portas/celula.py:701`)
- **Status paralelo remanescente: não nesses três insumos.** `_trab` e marcos alimentam previsto; `_cob_app` alimenta pendência. Porém, há **achado médio herdado nos marcos**: a view consulta o template, contrariando a autoridade de marcos congelados do vínculo. (`app/api/views.py:1584`, `app/api/views.py:1595`, `app/api/views.py:1597`, `app/docs/JUIZES.md:22`, `app/escala/models.py:1094`)
- **Mordida real, parcial.** Os testes exigem irregular com/sem batida e falta decidida; o teste invertido compara o resultado sob três respostas da grade. O teste de colaboradores que apenas procura nomes no arquivo inteiro é fraco isoladamente, pois comentários bastam. (`app/api/tests/test_e4_app_mapa.py:77`, `app/api/tests/test_e4_app_mapa.py:85`, `app/api/tests/test_e4_app_mapa.py:92`, `app/colaboradores/tests/test_t4_lei_status_none_alerta.py:167`, `app/colaboradores/tests/test_f1c_lote3_status_celula.py:67`)
- **Lacuna dos testes:** o caso irregular sem batidas não verifica `inconsistente`; o teste mensal compara carga mensal com D4 sem carga, embora a janela possa alterar o pareamento. (`app/api/tests/test_e4_app_mapa.py:90`, `app/api/tests/test_e4_app_mapa.py:109`, `app/ponto/turnos.py:2930`)
- **Pré-requisito:** há exigência documental de D5 anterior às telas e ordem de pouso; há também dívida registrada para competências antigas. Cumprimento efetivo em produção e ordem real **[nao verificado]**. (`app/docs/BACKLOG.md:802`, `app/docs/BACKLOG.md:846`, `app/docs/BACKLOG.md:1732`, `app/docs/BACKLOG.md:1736`)

O censo local executado retornou `numero=0`, `novos=[]`; isso não certifica booleanos, indireções ou HTML/JS, limitações declaradas pelo instrumento. Execução da suíte Django e prova RED/GREEN **[nao verificado]**. (`app/core/medida_leitor_do_dia.py:355`, `app/core/medida_leitor_do_dia.py:342`)

**6. Contraexemplos pela regra**

| Cenário | Resultado verificável no código |
|---|---|
| Ausência parcial lavrada | Se a parcial acusa o restante, lavra `irregular → alerta`; sem essa acusação, pode lavrar `ausencia → neutro`. Integração completa no app **[nao verificado]**. (`app/ponto/status_dia.py:104`, `app/ponto/status_dia.py:139`, `app/ponto/status_dia.py:154`, `app/ponto/status_dia.py:79`) |
| Folga trabalhada encerrada | Lavra `folga_trabalhada → ok`. (`app/ponto/status_dia.py:157`, `app/ponto/status_dia.py:80`) |
| Feriado sem batidas e sem lavra | Natureza de feriado retorna `folga`; trabalho em feriado depende do vínculo. (`app/ponto/status_dia.py:312`, `app/ponto/precedencia.py:373`) |
| Hoje antes/durante o turno | Trabalho sem batida não encerrado retorna `previsto`; turno vivo ou batida no dia ainda em curso retorna `aberto`, inclusive com saídas registradas. (`app/ponto/status_dia.py:280`, `app/ponto/status_dia.py:283`, `app/ponto/status_dia.py:316`) |
| Depois do encerramento | Lê a lavra; trabalho sem lavra retorna `sem_certidao → neutro`. (`app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:294`, `app/ponto/status_dia.py:316`, `app/ponto/status_dia.py:84`) |
| Cruza meia-noite | O fim previsto é encadeado para o dia seguinte; há, porém, dívida de janela no pareamento de saída órfã: mapa mensal não vazio sem marcos da véspera impede o uso do template. (`app/escala/servico_jornada.py:79`, `app/ponto/turnos.py:1297`, `app/ponto/turnos.py:285`) |

A dívida noturna não demonstra, sozinha, regressão exclusiva deste diff: a view anterior já perguntava geometria na janela mensal. Um dia concreto de produção que o app mostrava corretamente e agora mostra incorretamente **[nao verificado]**. (`app/api/views.py:1445`, `app/api/views.py:1449`)

**7. Código morto**

Não identifiquei variável, import ou helper tornado órfão dentro da função: cobertura continua necessária para pendência; células/folgas, para previsto; períodos, para horas; conversão de horário, para decisão. Permanecem comentários desatualizados atribuindo à cobertura a escolha do status. (`app/api/views.py:1540`, `app/api/views.py:1553`, `app/api/views.py:1568`, `app/api/views.py:1581`, `app/api/views.py:1584`, `app/api/views.py:1595`)

## 04/11 15:42 · sessao 09b04ac65aaa · - · - · origem=Ronald (tmux)

Correção do custo: a estimativa “~10” só vale quando não há consultas diárias de feriado; existe N+1 em dias comuns sem `celula.trabalha` definido (`app/ponto/precedencia.py:359–368`; `app/escala/models.py:1340–1363`; `app/core/feriados.py:29`, `:82`, `:95`).

Fórmula estática para o trecho acrescentado:

`5 + G + L + A × (4 + F + G2 + U + H + FK) + C`

- `5`: carga básica — células, candidatos, vínculos, escala da ata e batidas (`app/ponto/turnos.py:2943–2955`, `:2982–2985`; `app/ponto/turnos.py:1556–1558`).
- `G/G2`: consulta adicional quando faltam geradoras nos candidatos da carga/A2; `L`: consulta preguiçosa de última batida (`app/escala/alimentacao.py:123–127`; `app/ponto/turnos.py:2962–2963`; `app/ponto/precedencia.py:319–331`).
- `A`: A2 foi necessária, 0 ou 1; A2 calcula a janela inteira uma única vez por colaborador (`app/ponto/status_dia.py:303–307`).
- `4`: ausências, folgas lançadas, feriados e candidatos; `F`: mais uma consulta da foto quando há vínculos (`app/ponto/precedencia.py:273`, `:282`, `:287–289`, `:319–333`).
- `U`: datas da janela inteira sem `celula.trabalha` definido que atingem `base=True`, ciclo weekday e `trabalha_em_feriado=False`; **uma consulta por data, mesmo sem feriado** (`app/ponto/precedencia.py:359–368`; `app/escala/models.py:1337–1363`; `app/core/feriados.py:95`, `:82`, `:29`).
- `H`: mais uma consulta por feriado para seu nome; `FK`: relações lazy do local, inclusive `self.colaborador`, posto e município (`app/ponto/precedencia.py:407`; `app/core/feriados.py:100`, `:113–114`; `app/escala/models.py:1363`).
- `C`: catálogo frio, normalmente 0 ou 1, condicionado ao cache de 60 segundos e ao aquecimento anterior da view (`app/ponto/catalogo/ausencias.py:249`, `:257–280`, `:293–295`).

Exemplo estrutural: mês futuro 5x2 sem células pode acrescentar `U` próximo do número de dias úteis, além da base; número medido [nao verificado] (`app/escala/models.py:32`, `:1340–1363`; `app/ponto/precedencia.py:359–368`).

`fim_turno`/`marcos_do_dia` no percurso de `Carga.dias_encerrados` usam as células alimentadas e não criam consulta por dia: a carga entrega o dict até `ec.marcos_do_dia`, que o consulta diretamente; `tipo_escala` já vem por `select_related` (`app/ponto/turnos.py:2953`, `:3004–3005`, `:2164–2165`, `:2119`; `app/escala/servico_jornada.py:92`, `:69`; `app/escala/models.py:1094–1095`).

O saldo deve descontar a grade removida: uma consulta básica e eventuais consultas antigas por célula sem `ata.minutos_previstos`; portanto o saldo exato da chamada continua [nao verificado] (`app/escala/services/leitor_celula.py:350`, `:376–383`; `app/escala/utils.py:2279–2290`).

## 04/11 15:42 · sessao ac38caed6925 · - · - · origem=Ronald (tmux)

Q5 — estrutura e testes: [nao verificado]

- **Não identifiquei segundo juiz no adaptador:** `STATUS_DO_APP` é tabela e `status_do_app` apenas indexa o status recebido (`app/ponto/status_dia.py:79-91`). O juiz registrado continua sendo `status_do_dia_lavrado` (`app/core/juizes.py:183-187`), cujo corpo singular delega ao lote (`app/ponto/status_dia.py:217-218`).
- **Não há novo escritor persistente no trecho alterado:** a view consulta D4 e monta dicionários (`app/api/views.py:1495-1499`, `1543-1563`, `1603-1626`); a gravação de `CelulaDia.status_dia` continua na porta (`app/ponto/portas/celula.py:701-706`, `752-775`).
- **Os remanescentes não escolhem `status`:** `_trab` e `marcos_do_dia` montam horários; `_cob_app` alimenta `ausencia_pendente` (`app/api/views.py:1584-1586`, `1595-1600`, `1621-1625`). **Há derivação paralela de marcos:** usa o template, apesar da autoridade registrada ser `EscalaColaborador.marcos_do_dia` (`app/api/views.py:1597`; `app/core/juizes.py:201`).
- **Mordida real, mas parcial:** a bateria nova exige `irregular→alerta` com e sem batida e `falta→falta/True` (`app/api/tests/test_e4_app_mapa.py:77-97`); o teste invertido da grade compara resultados sob `CEL_OK`, `CEL_MISSING` e `CEL_NAO_SEI` (`app/colaboradores/tests/test_t4_lei_status_none_alerta.py:167-173`). A asserção do outro teste é fraca isoladamente: procura dois nomes no arquivo inteiro, permitindo satisfação por comentário (`app/colaboradores/tests/test_f1c_lote3_status_celula.py:62-69`). Execução RED/GREEN destes bytes: **[nao verificado]**.
- **Pré-requisitos:** a ordem documental exige leitores anteriores e portão de 04/11 às 18h (`app/docs/BACKLOG.md:846`, `1732`). D5 e L1 aparecem registrados como publicados (`app/docs/BACKLOG.md:802`, `839`); estado efetivo em produção e ordem real de pouso: **[nao verificado]**.

Q6 — cenários e achados:

- **Média: `alerta` sem `inconsistente`.** Dia sem batidas e lavrado `irregular` recebe `status='alerta'`, porém `inconsistente=False` literal (`app/api/views.py:1599-1620`). Com batidas, o mesmo alerta determina `inconsistente=True` (`app/api/views.py:1515-1518`). O teste novo verifica `falta=False`, mas não verifica esse booleano (`app/api/tests/test_e4_app_mapa.py:85-90`).
- **Parcial aprovada:** a lavra parcial que acusa o restante vira `irregular`, chegando ao app como `alerta`; sem acusação vira `ausencia→neutro` (`app/ponto/status_dia.py:104-110`, `139-155`, `79-84`). A regra possui testes diretos, inclusive exceção do isento (`app/ponto/tests/test_e3_status_lavrado.py:78-98`); integração desses casos no app: **[nao verificado]**.
- **Folga trabalhada e feriado:** lavra `folga_trabalhada→ok`; sem batidas e sem lavra, natureza de feriado/folga retorna `folga` (`app/ponto/status_dia.py:79-84`, `287-319`). Antes do encerramento, turno vivo ou batida no dia em curso retorna `aberto`; sem batida de trabalho retorna `previsto`; encerrado sem lavra retorna `sem_certidao→neutro` (`app/ponto/status_dia.py:273-319`).
- **Dívida concreta de janela, sem atribuição de regressão exclusiva ao diff:** template 21–05, uma única saída em 18/09 às 05h e DNA somente em 01/09 fazem o pareador atribuir a saída a 17/09 quando o mapa é vazio/`None`, mas a 18/09 quando o mapa mensal contém apenas aquele DNA distante. O ramo usa template somente quando o mapa inteiro é vazio (`app/ponto/turnos.py:1297-1300`); sem marcos da véspera, `_data_do_turno` fica na data civil (`app/ponto/turnos.py:283-296`). A carga transforma mapa vazio em `None` (`app/ponto/turnos.py:2991-2998`; `app/escala/utils.py:95-108`). Reproduzi as funções puras em memória, com adaptador de timezone da biblioteca padrão; integração Django: **[nao verificado]**.
- **O teste mensal não cobre essa dívida:** consulta a view com carga mensal, mas compara com D4 sem carga (`app/api/views.py:1498-1499`; `app/api/tests/test_e4_app_mapa.py:99-113`). A geometria mensal já existia na view (`app/api/views.py:1445-1451`), portanto esse exemplo sozinho não demonstra “app antes certo, depois errado”. Contraexemplo completo dessa forma: **[nao verificado]**.

## 04/11 15:41 · sessao 09b04ac65aaa · - · - · origem=Ronald (tmux)

Q2–Q4, somente inspeção estática; contagens executadas e incidência em produção [nao verificado].

- O mapa entrega exclusivamente `{ok, falta, previsto, folga, neutro, alerta, aberto}` e cobre as 11 palavras do núcleo; `status_do_app` indexa diretamente a tabela (`app/ponto/status_dia.py:79–91`).
- Nenhuma data solicitada desaparece por falta de vínculo, desligamento, futuro ou ausência de escala: a view solicita o mês completo; o D4 percorre cada par e todos os ramos escrevem sua saída (`app/api/views.py:1497–1499`; `app/ponto/status_dia.py:255–319`). Sem escala/natureza/lavra/batidas, termina em `sem_dados` (`app/ponto/precedencia.py:385–390`; `app/ponto/status_dia.py:308–319`).
- Há caminho condicional de `KeyError`: o D4 repassa `cel.status_dia` sem validar o vocabulário; com palavra inválida e `status_rotulo` preenchido, o erro ocorre no adaptador; sem rótulo, pode ocorrer antes em `ROTULO_STATUS` (`app/ponto/status_dia.py:287–289`, `:91`). Escritores normais da porta derivam de regras fechadas; ocorrência de palavra inválida no dado [nao verificado] (`app/ponto/portas/celula.py:686–705`; `app/escala/models.py:1781–1785`).
- O `agora` é o mesmo: capturado na view, entregue ao espelho e ao D4, depois à regra de encerramento (`app/api/views.py:1350`, `:1369`, `:1498`; `app/ponto/status_dia.py:252–254`, `:274`; `app/ponto/turnos.py:3001–3005`, `:2164–2166`).
- Existe leitura de relógio fora da borda, herdada: `Carga.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado → date.today()` (`app/ponto/turnos.py:2995`, `:1937`, `:1631`; `app/ponto/juiz_batida.py:270`, `:125`). O consumidor de papel usa entrada/saída/papéis, sem consumir o intervalo cadastrado; efeito dessa leitura no status D4 [nao verificado] (`app/ponto/turnos.py:1636`, `:1680–1683`).
- A carga acrescenta **5 consultas base**, mais até duas condicionais: células, candidatos, vínculos, escala da ata, batidas; condicionais para geradora ausente e última batida (`app/ponto/turnos.py:2943–2955`, `:2962–2963`, `:2982–2985`; `app/escala/alimentacao.py:116–127`; `app/ponto/turnos.py:1556–1558`).
- Com a carga mensal, A2 é chamada **uma vez por colaborador**, quando necessário, aproximadamente mais cinco consultas usuais, além de condicionais/FKs e uma consulta por feriado para buscar seu nome (`app/ponto/status_dia.py:303–307`; `app/ponto/precedencia.py:273`, `:282`, `:287–289`, `:319–333`, `:407`; `app/core/feriados.py:100`, `:113–114`). Não são cinco consultas multiplicadas por 31 dias; existe N+1 herdado por feriado (`app/ponto/precedencia.py:407`; `app/core/feriados.py:100`).
- Comparação: saiu uma consulta básica da grade; a grade antiga também podia consultar por dia quando faltava `ata.minutos_previstos` (`app/escala/services/leitor_celula.py:350`, `:376–383`; `app/escala/utils.py:2279–2290`). Saldo exato de consultas [nao verificado].
- Há custo de CPU repetido: o D4 pede `dias_encerrados` a cada data, e a carga recalcula a janela inteira em cada chamada; para 31 datas, aproximadamente 31 varreduras de 31 dias, sem novas consultas dessas varreduras (`app/ponto/status_dia.py:274`; `app/ponto/turnos.py:3004–3005`, `:2158–2167`). Os turnos ficam em cache (`app/ponto/turnos.py:2987–2999`).
- Ressalva de semântica: o D4 não consulta admissão/demissão para filtrar dias; a grade removida fazia esse filtro. Célula residual fora da vida pode agora fornecer status lavrado; incidência no dado [nao verificado] (`app/ponto/status_dia.py:269–319`; `app/escala/services/leitor_celula.py:351`, `:363–370`).

## 04/11 15:40 · sessao 93f2b159d10b · - · - · origem=Ronald (tmux)

Q1 — escritores e leitores do payload:

| Chave | Quem escreve |
|---|---|
| `status`, `status_dia` | D4 + adaptador nos dois ramos: `app/api/views.py:1498`, `:1515`, `:1516`, `:1561`, `:1562`, `:1599`, `:1600`, `:1617`, `:1618`. |
| `falta` | Apenas card sem batida: `_st == 'falta'`, `app/api/views.py:1620`. Card com batida não inclui essa chave: dict `app/api/views.py:1543`. |
| `inconsistente` | Com batida: turno aberto ou `status == 'alerta'`, `app/api/views.py:1518`, `:1559`. Sem batida: constante `False`, `app/api/views.py:1615`. |
| `turno_aberto` | Geometria carregada em `app/api/views.py:1445`, `:1449`, avaliada em `:1514`, escrita em `:1560`; sem batida, constante `False`, `:1616`. |
| `ausencia_pendente` | Cobertura para cobrança + `ABERTAS`, `app/api/views.py:1574`, `:1575`, `:1582`, `:1584`; inserida em ambos os ramos, `:1592`, `:1625`. |
| `previsto_inicio`, `previsto_fim`, intervalos | Apenas card sem batida: `eh_dia_trabalho` + template `TipoEscala.marcos_do_dia`, `app/api/views.py:1595`, `:1597`, escrita em `:1621`, `:1622`, `:1623`, `:1624`. |

Leituras internas: `status` alimenta `inconsistente`, `app/api/views.py:1518`; `_st` alimenta `falta`, `:1620`. O dict local é entregue em `:1675`.

Nenhum consumidor de produção do endpoint encontrado em `static/`, `templates/` ou outros endpoints por busca textual; leitores indiretos [nao verificado]. Rota encontrada: `app/api/urls.py:81`. V1 somente responde 410 com endereço v2, sem reutilizar dict: `app/api/views.py:839`, `:843`, `:844`.

O HTML do espelho usa **outro dict**, `_esp['dias']`: `app/ponto/views.py:157`, `:159`, `:193`, `:196`. Seus leitores são `d.status/status_rotulo`, `app/templates/ponto/espelho.html:151`; `dia.inconsistente`, `:166`; `t.par.turno_aberto`, `:193`; `dia.previsto/previsto_canon`, `:207`, `:218`. Não são consumidores do payload API.

Casca inclui `pwa.js`, `app/templates/base_app.html:20`; SW apenas repassa os demais fetches, `app/static/sw.js:75`. Nenhum fonte nativo encontrado pelas extensões usuais; contrato efetivo do compilado [nao verificado]. Existe referência ao pacote Play Store `com.hasner.wk`, `app/templates/core/gate_apk.html:6`, `:7`.

Leitores de teste relevantes: [nao verificado]

- `status/status_dia/falta`: `app/api/tests/test_e4_app_mapa.py:83`, `:90`, `:97`, `:112`; `app/api/tests/test_espelho_app_ausencia_pela_autoridade.py:53`, `:59`, `:66`, `:70`; `app/api/tests/test_espelho_app_hoje_no_teto.py:47`, `:52`, `:57`.
- `turno_aberto/inconsistente`: `app/api/tests/test_espelho_app_fonte_e_geometria.py:338`, `:344`, `:345`, `:346`; `app/api/tests/test_espelho_app_le_o_juiz_do_dia.py:77`.
- `ausencia_pendente` e presença das antigas chaves, incluindo previsto: `app/api/tests/test_espelho_app_ausencia_pendente_aditivo.py:16`, `:30`, `:36`, `:40`, `:46`.
- Comparação de status/inconsistente sob mock da grade: `app/colaboradores/tests/test_t4_lei_status_none_alerta.py:164`, `:165`, `:171`.
- Isento: lê status/falta/inconsistente/turno_aberto, `app/escala/tests/test_art62_nunca_acusa.py:221`.

Achado **MÉDIO**: `status='alerta', status_dia='irregular', inconsistente=False` ocorre para irregular lavrado sem batida, enquanto irregular com batida produz `inconsistente=True`; derivações divergentes em `app/api/views.py:1518` e `:1615`. O caso novo sem batida não verifica `inconsistente`, `app/api/tests/test_e4_app_mapa.py:85`, `:90`. Impacto em tela compilada [nao verificado].

Mudanças semânticas que exigem confirmar consumidor externo: falta automática virou somente decidida, `app/api/tests/test_espelho_app_ausencia_pela_autoridade.py:61`, `:66`; turno aberto antigo pode ter status neutro com `turno_aberto=True`, `app/api/tests/test_espelho_app_fonte_e_geometria.py:338`, `:340`, `:344`. Leitor externo dependente desses sentidos [nao verificado].

## 04/11 15:15 · sessao 8855056474c6 · E4-ESPELHO · verificador · origem=Code

**Há dois achados P2 de custo: consultas podem crescer por dia e o encerramento da janela é recalculado quadraticamente. Não encontrei contraexemplo funcional confirmado de cor ausente ou `KeyError` com entradas válidas.** (`app/ponto/precedencia.py:368`, `app/ponto/precedencia.py:407`, `app/ponto/status_dia.py:253`, `app/ponto/turnos.py:2163`, `app/templates/ponto/espelho.html:113`)

1. **Quem escreve e quem lê**

   | Fluxo | Escritor | Leitor |
   |---|---|---|
   | Faixa do espelho | Copia `status` e `rotulo` do D4 para os dois campos. | `<a>` e `<span>` usam classe e `title`. |
   | Preenchimento semanal | Cria `status='fora'`, sem `status_rotulo`. | A mesma faixa, com `.st-fora`. |
   | Calendário | Monta outro dicionário com status/rótulo do D4. | Resolvedor, resumo, grade e relatório de furos. |
   | Painel | Coloca a resposta D4 em `linha['dia']`. | Card do painel. |
   | Previsão de escala | Escreve outro `status_rotulo`. | Template da escala. |

   Referências: espelho em `app/ponto/services/espelho.py:181`, preenchimento em `app/ponto/services/espelho.py:192`, leitores em `app/templates/ponto/espelho.html:151` e CSS em `app/templates/ponto/espelho.html:124`; calendário em `app/colaboradores/services/calendario.py:699`, `app/colaboradores/services/calendario.py:700`, `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:746`, `app/templates/colaboradores/partials/_calendario_grade.html:69`, `app/relatorios/views.py:1550`; painel em `app/core/services/painel_op.py:338`, `app/templates/core/_painel_card.html:67`; escala em `app/escala/views.py:60`, `app/templates/escala/minha_escala.html:75`.

   **PDF não consome os campos alterados:** chama `espelho_do_colab`, recorta seus dias e não passa por `montar_semanas`. O app também chama esse miolo, mas escreve outro payload `status`, com vocabulário próprio — inclusive `alerta/neutro`. Essa derivação paralela permanece; não foi introduzida pelo diff. (`app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:598`, `app/api/views.py:1369`, `app/api/views.py:1570`, `app/api/views.py:1591`, `app/api/views.py:1650`, `app/api/views.py:1673`)

   Os includes do espelho são o ícone de ausência e o relógio riscado; não leem esses campos. `dia.ausencia.status` pertence à ausência; `c.status`, ao marco. O JavaScript da faixa só posiciona a semana atual. (`app/templates/ponto/espelho.html:191`, `app/templates/ponto/espelho.html:237`, `app/templates/core/_relogio_riscado.html:15`, `app/templates/ponto/espelho.html:159`)

   Não identifiquei consumidor dos campos alterados esperando `atencao/abono/sem_registro/vazio`; essas palavras continuam no detector histórico. Os testes alterados leem status/rótulo para comparar com D4. (`app/templates/ponto/espelho.html:151`, `app/core/medida_leitor_do_dia.py:131`, `app/ponto/tests/test_caracterizacao_espelho.py:87`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:77`)

2. **Contrato e cobertura da carga**

   D4 exige datas dentro da carga e colaboradores presentes nela; rejeita violações com `ValueError`. O parâmetro opcional `fatos` exige carga e, quando fornecido, suas chaves são acessadas diretamente. (`app/ponto/status_dia.py:238`, `app/ponto/status_dia.py:241`, `app/ponto/status_dia.py:244`, `app/ponto/status_dia.py:280`)

   **A carga cobre todas as datas originais:** `min/max` vêm da mesma lista usada nos pares. O preenchimento fora desses limites nasce depois, com `status='fora'`, sem consultar D4. (`app/ponto/services/espelho.py:176`, `app/ponto/services/espelho.py:178`, `app/ponto/services/espelho.py:192`)

   **Não há caminho visível nessa view para `colaborador=None` com dias não vazios:** começa com `dias=[]` e só preenche dentro de `if colaborador`; a montagem retorna imediatamente para lista vazia. Chamador externo passando `None` com dias viola essa condição e pode atingir acesso a `.pk`. (`app/ponto/views.py:124`, `app/ponto/views.py:143`, `app/ponto/views.py:159`, `app/ponto/services/espelho.py:172`, `app/ponto/turnos.py:2164`)

   A janela visual pode começar antes do vínculo/cadastro, pois usa `min(piso_visual, apur_ini)`. Outro vínculo da mesma pessoa é resolvido pela geradora e vigência do dia; geradora fora da vigência devolve `None`, sem escolher outra silenciosamente. (`app/ponto/services/espelho.py:1225`, `app/ponto/services/espelho.py:1262`, `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:250`)

   Antes do cadastro, sem célula, vínculo, batida, ausência ou feriado, o resultado é `sem_dados`. Ausência/feriado podem fornecer natureza; uma célula já lavrada é lida antes desse ramo, mesmo sem vínculo vigente. Não há descarte universal por admissão nesse leitor. (`app/ponto/precedencia.py:385`, `app/ponto/status_dia.py:266`, `app/ponto/status_dia.py:289`, `app/ponto/status_dia.py:297`)

3. **Relógio**

   O trecho novo passa corretamente o mesmo instante da borda para dias e semanas. D4 só chama `timezone.now()` se `agora is None`. (`app/ponto/views.py:97`, `app/ponto/views.py:158`, `app/ponto/views.py:184`, `app/ponto/status_dia.py:231`)

   **Existe leitura transitiva herdada fora da borda:** carga → pareamento → leitura da ata → `periodos_do_dia` → `intervalo_cadastrado` → `date.today()`. Não é chamada textual nova no diff. (`app/ponto/turnos.py:2995`, `app/ponto/turnos.py:1937`, `app/ponto/turnos.py:1631`, `app/ponto/juiz_batida.py:270`, `app/ponto/juiz_batida.py:125`)

   Essa leitura alimenta intervalo; o consumidor da ata usa papéis e bordas. Alteração do novo status por esse relógio: **[nao verificado]**. (`app/ponto/juiz_batida.py:299`, `app/ponto/turnos.py:1636`, `app/ponto/turnos.py:1680`)

4. **Custo — dois achados P2**

   **P2 — não é estritamente O(1) em dias.** Para lista não vazia, a carga possui cinco consultas estruturais: células, candidatos, vínculos, ata e batidas; pode acrescentar uma para completar geradoras e uma para última batida. Lista vazia acrescenta zero. (`app/ponto/turnos.py:2943`, `app/ponto/turnos.py:2949`, `app/ponto/turnos.py:2953`, `app/ponto/turnos.py:2955`, `app/ponto/turnos.py:2962`, `app/ponto/turnos.py:2982`, `app/escala/alimentacao.py:123`, `app/ponto/services/espelho.py:172`)

   Quando algum dia alcança natureza, D4 chama A2 uma vez para a janela inteira. Há quatro consultas estruturais adicionais, mais complementação de geradoras, foto de folgas e carregamentos condicionais. (`app/ponto/status_dia.py:282`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:282`, `app/ponto/precedencia.py:289`, `app/ponto/precedencia.py:319`, `app/ponto/precedencia.py:333`)

   Há dois caminhos concretos de N+1:

   - Cada feriado da janela pode consultar novamente seu nome. (`app/ponto/precedencia.py:407`, `app/core/feriados.py:100`)
   - Sem célula com `trabalha` definido, o cálculo pelo vínculo pode consultar feriado por data nos ciclos aplicáveis. Portanto, o pior caso cresce com os dias. (`app/ponto/precedencia.py:368`, `app/escala/models.py:1340`, `app/escala/models.py:1363`, `app/core/feriados.py:82`)

   **P2 — CPU O(D²):** para cada data pedida, D4 chama `carga.dias_encerrados`; esse método não memoriza a resposta e percorre novamente toda a janela. O pareamento é memorizado, o encerramento não. (`app/ponto/status_dia.py:248`, `app/ponto/status_dia.py:253`, `app/ponto/turnos.py:2987`, `app/ponto/turnos.py:3004`, `app/ponto/turnos.py:2163`)

   Quantidade efetiva de consultas, duração e incidência em produção: **[nao verificado]**.

5. **Classes de reprovação**

   - **Segundo juiz:** não identificado no trecho novo; ele copia D4, cuja autoridade já está registrada. (`app/ponto/services/espelho.py:177`, `app/ponto/services/espelho.py:181`, `app/core/juizes.py:187`)
   - **Segundo escritor fora da porta:** não identificado; a atribuição nova é no dicionário de apresentação. A escrita persistente permanece na porta. (`app/ponto/services/espelho.py:181`, `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:709`)
   - **Derivação paralela:** removida da faixa; a do app permanece declarada. (`app/ponto/services/espelho.py:152`, `app/core/medida_leitor_do_dia.py:335`)
   - **Dublê unitário:** morde geometria, cards e preenchimento; não protege status/rótulo nem argumentos da carga. Executei as quatro funções de teste isoladas em memória: passaram também removendo a atribuição dos dois campos. Isso corresponde ao escopo declarado, portanto não reprova sozinho a migração. (`app/ponto/tests/test_caracterizacao_espelho_unit.py:21`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:27`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:46`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:53`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:68`)
   - **Selo dedicado:** possui dois resultados fixos `irregular`, além da paridade; não depende apenas de comparar o leitor consigo mesmo. Já o teste auxiliar da regex só testa uma string local — **P3, sem mordida no produto**. (`app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:46`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:56`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:64`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:110`)
   - **Pré-requisito e ordem de trava/pouso:** há exigências documentadas de D5 anterior aos leitores, reset às 18h e sequência da NOITE-0411. Execução real, RED observado e suíte certificada: **[nao verificado]**. (`app/docs/BACKLOG.md:802`, `app/docs/BACKLOG.md:1732`)

6. **Pendente reancorado e dente do contrato**

   **A impressão nova existe exatamente uma vez no arquivo alvo**, na chamada D4, e há um registro correspondente. O pendente foi mantido explicitamente, sem baixa silenciosa. (`app/ponto/services/espelho.py:177`, `app/core/juizes.py:863`, `app/core/juizes.py:866`)

   **O dente sintático continua; o semântico é limitado — P3.** O contrato verifica presença da substring. Não exige chamada executável, uso da resposta ou ocorrência única no fonte; comentário contendo a impressão bastaria. O teste de duplicidade verifica registros repetidos, não ocorrências no arquivo. (`app/core/tests/test_contract_juiz_tela.py:34`, `app/core/tests/test_contract_juiz_tela.py:37`, `app/core/tests/test_contract_juiz_tela.py:52`)

7. **Contraexemplo de cor ou `KeyError`**

   Não encontrei palavra válida do núcleo sem CSS: conferi as 11, todas cobertas. `fora` é preenchimento externo ao vocabulário e possui regra própria. (`app/ponto/status_dia.py:30`, `app/ponto/status_dia.py:49`, `app/templates/ponto/espelho.html:113`, `app/templates/ponto/espelho.html:124`, `app/ponto/services/espelho.py:192`)

   Há uma hipótese condicional: D4 devolve `cel.status_dia` sem validar o vocabulário nesse ponto. Valor inválido com rótulo preenchido chegaria sem CSS; sem rótulo, causaria `KeyError` em `ROTULO_STATUS[...]`. O modelo declara as 11 escolhas e a porta deriva os valores pelo núcleo; existência de coluna inválida ou vítima em produção: **[nao verificado]**. (`app/ponto/status_dia.py:266`, `app/ponto/status_dia.py:268`, `app/escala/models.py:1781`, `app/ponto/portas/celula.py:693`)

## 04/11 15:14 · sessao 8cc010661e79 · - · - · origem=Ronald (tmux)

**Q2 — contrato e cobertura**

- `montar_semanas` monta a carga com `min/max` das próprias datas perguntadas: nenhuma data original de `dias` pode ficar fora dessa janela. O preenchimento das semanas acontece depois e usa `status='fora'`, sem consultar D4 para esses espaços. `app/ponto/services/espelho.py:176-178,183-192`.
- D4 exige datas em `[carga.ini,carga.fim]` e colaboradores presentes em `carga._colabs`; violações levantam `ValueError`. `fatos` fornecidos exigem carga e mapa completo; chave ausente não recebe tentativa alternativa. `app/ponto/status_dia.py:238-246,279-286`.
- Na view, colaborador ausente deixa `dias=[]`; a chamada a `espelho_do_colab` só ocorre dentro de `if colaborador`, e `montar_semanas` retorna antes de acessar `.pk` para lista vazia. Não há caminho visível nessa view para `None` com dias não vazios. Chamadas externas incompatíveis continuam capazes de falhar. `app/ponto/views.py:110-124,143-159,184`; `app/ponto/services/espelho.py:172-180`.
- A janela visual pode começar antes do vínculo/admissão: `vis_ini=min(piso_visual(...),apur_ini)` e `montar_dias` enumera todas as datas até hoje; não há filtro por admissão no D4. `app/ponto/services/espelho.py:1225-1227,1262-1266,482,660-661`; `app/ponto/status_dia.py:248-298`.
- Outro vínculo da mesma pessoa é tratado pelo vínculo do dia, escolhido pela geradora e vigência; a carga inclui todos os vínculos para encerramento, além dos candidatos e geradoras para pareamento. Geradora fora da vigência devolve `None`, sem escolher outra silenciosamente. `app/ponto/turnos.py:2943-2955,3004-3005,2164`; `app/escala/alimentacao.py:237-254`.
- Antes do cadastro, sem célula, vínculo, batida, ausência ou feriado: A2 produz faixa `None` e D4 responde `sem_dados`; ausência/feriado existentes podem responder sua natureza. Uma célula já lavrada é lida antes da natureza, inclusive sem vínculo vigente. `app/ponto/precedencia.py:349-385,386-390`; `app/ponto/status_dia.py:265-298`.

**Q3 — relógio**

- O diff passa corretamente o mesmo `_agora_do_ato` da borda à montagem dos dias e das semanas; D4 só chama `timezone.now()` quando recebe `agora=None`. `app/ponto/views.py:97-98,158,182-184`; `app/ponto/status_dia.py:231-233`.
- **Ressalva herdada:** existe leitura transitiva fora da borda: `Carga.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado → date.today()`. Não é linha nova do diff. `app/ponto/turnos.py:2995-2998,1937,1631`; `app/ponto/juiz_batida.py:270,125`.
- Essa leitura alimenta o resultado de intervalo; o consumidor do pareamento usa papéis, entrada, saída e `em_aberto`, sem ler aquele intervalo. Efeito dessa leitura sobre o novo status não foi demonstrado. `app/ponto/juiz_batida.py:299-309`; `app/ponto/turnos.py:1634-1636,1680-1683` — efeito no status `[nao verificado]`.

**Q4 — custo e achados**

- **Média — a consulta não é estritamente O(1) em dias.** Para render não vazio, a carga adiciona **5 consultas base + G + U**, com `G∈{0,1}` para completar geradoras e `U∈{0,1}` para última batida: células, candidatos, vínculos, ata e batidas. `app/ponto/turnos.py:2943-2963,2978-2998`; `app/escala/alimentacao.py:116-127`; `app/ponto/turnos.py:1554-1559`.
- Havendo algum dia sem lavra/batida que alcance natureza, A2 roda uma vez para toda a janela: **+4 + G₂ + F + H + M + R + C**, onde `G₂` completa geradoras, `F` consulta foto de folgas se houver vínculos, `H` conta nomes de feriados, `M` conta consultas de feriado nos dias calculados sem célula, `R` são relações carregadas sob demanda e `C` eventual leitura do catálogo sem cache válido. `app/ponto/status_dia.py:275-286`; `app/ponto/precedencia.py:273-289,319-333,407`; `app/ponto/catalogo/ausencias.py:267-280,293`.
- **N+1 concreto:** para cada feriado encontrado na janela, A2 chama `nome_do_feriado`, que consulta novamente aquele dia. `app/ponto/precedencia.py:407`; `app/core/feriados.py:98-102,27-29`.
- **N+1 concreto e O(dias) no pior caso:** faltando célula com `trabalha` definido, A2 calcula o dia pelo vínculo; em ciclos `5x2/6x1/semanal`, trabalho pode chamar `eh_feriado` novamente por data. O vínculo carregado inclui `tipo_escala`, mas não `colaborador/posto/municipio`, podendo acrescentar consultas por vínculo usado. `app/ponto/precedencia.py:359-370`; `app/escala/models.py:1340,1357-1363,32`; `app/core/feriados.py:91-95,70-83,113-114`; `app/escala/alimentacao.py:120`.
- **Média — CPU quadrática:** D4 chama `carga.dias_encerrados` para cada data; esse método não memoriza o resultado e percorre novamente toda a janela. Portanto, há O(D²) avaliações do encerramento para D dias, além do pareamento já memorizado. `app/ponto/status_dia.py:248-253`; `app/ponto/turnos.py:3001-3005,2158-2167,2987-2999`.
- Essa repetição de encerramento **não** consulta célula/marcos por dia: passa o mapa completo até `EC.marcos_do_dia`; célula ausente usa o template carregado. O N+1 identificado está na natureza/A2. `app/ponto/turnos.py:2164-2165,2119`; `app/escala/servico_jornada.py:68-71,89-93`; `app/escala/models.py:1094-1108`.
- Número efetivo de consultas, duração e incidência desses caminhos em produção: `[nao verificado]`.

## 04/11 15:13 · sessao 1c67dccb65e3 · - · - · origem=Ronald (tmux)

1. No fluxo do espelho, `montar_semanas` escreve `dia['status']` e `dia['status_rotulo']` copiando o D4; escreve também `status='fora'` nos preenchimentos da semana, sem rótulo. O leitor dos dois campos é a faixa, nos elementos `<a>` e `<span>`. (`app/ponto/services/espelho.py:177`, `app/ponto/services/espelho.py:181`, `app/ponto/services/espelho.py:192`, `app/templates/ponto/espelho.html:151`)

2. PDF e app usam o miolo `espelho_do_colab`, sem passar por `montar_semanas`: o PDF coleta/recorta seus dias; o app constrói outro payload e calcula seu próprio `status` — `aberto/alerta/ok` com batidas, `ok/falta/neutro/previsto/folga` sem batidas. Portanto o vocabulário paralelo do app permanece, mas não é consumidor do campo alterado neste diff. (`app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:598`, `app/api/views.py:1369`, `app/api/views.py:1570`, `app/api/views.py:1591`, `app/api/views.py:1643`, `app/api/views.py:1650`, `app/api/views.py:1653`, `app/api/views.py:1656`, `app/api/views.py:1673`)

3. Os includes diretos do espelho são o ícone de ausência e o relógio riscado. O caminho do primeiro vem do catálogo e produz SVG; o segundo recebe contexto `only` e lê minutos/sentido/marco. As leituras `dia.ausencia.status` e `c.status` existentes no template pertencem à ausência e ao marco, respectivamente. (`app/templates/ponto/espelho.html:191`, `app/templates/ponto/espelho.html:237`, `app/ponto/services/espelho.py:856`, `app/ponto/catalogo/ausencias.py:178`, `app/templates/core/_icone_dia_falta.html:9`, `app/templates/core/_relogio_riscado.html:15`)

4. Outros produtores de dicionários de dia na árvore: calendário escreve os dois campos; previsão da escala escreve apenas `status_rotulo`; painel coloca a resposta D4 em `linha['dia']`. Seus leitores são o resolvedor e resumo do calendário, a grade do calendário, o mapa de furos, o card do painel e a previsão de escala. Nenhum desses pontos consome os campos produzidos por `montar_semanas`. (`app/colaboradores/services/calendario.py:637`, `app/colaboradores/services/calendario.py:699`, `app/colaboradores/services/calendario.py:700`, `app/escala/views.py:54`, `app/escala/views.py:60`, `app/core/services/painel_op.py:336`, `app/core/services/painel_op.py:338`, `app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:746`, `app/colaboradores/services/calendario.py:764`, `app/templates/colaboradores/partials/_calendario_grade.html:69`, `app/templates/colaboradores/partials/_calendario_grade.html:99`, `app/templates/colaboradores/partials/_calendario_grade.html:143`, `app/relatorios/views.py:1550`, `app/templates/core/_painel_card.html:67`, `app/templates/escala/minha_escala.html:75`, `app/templates/escala/minha_escala.html:84`)

5. Não identifiquei leitor de **status do dia** ainda comparando `atencao/abono/sem_registro/vazio`; as palavras permanecem na catraca como vocabulário histórico, e `abono` continua sendo tipo de ausência. (`app/core/medida_leitor_do_dia.py:131`, `app/core/medida_leitor_do_dia.py:134`, `app/ponto/catalogo/ausencias.py:102`, `app/templates/ponto/espelho.html:151`)

7. Não há contraexemplo confirmado de palavra válida do núcleo sem regra CSS: as 11 palavras têm regra; o preenchimento `fora`, externo às 11, também tem CSS próprio e já é gerado separadamente. (`app/ponto/status_dia.py:30`, `app/ponto/status_dia.py:49`, `app/templates/ponto/espelho.html:113`, `app/templates/ponto/espelho.html:124`, `app/ponto/services/espelho.py:192`)

Possibilidade condicional, **sem ocorrência demonstrada**: no ramo lavrado, D4 devolve `cel.status_dia` sem validar pertinência ao vocabulário. Uma coluna inválida com rótulo preenchido chega à classe sem CSS; sem rótulo, `ROTULO_STATUS[cel.status_dia]` levanta `KeyError`. O modelo declara somente as 11 escolhas e a porta deriva o valor pelas funções do núcleo; existência de coluna inválida em produção é `[nao verificado]`. (`app/ponto/status_dia.py:266`, `app/ponto/status_dia.py:268`, `app/escala/models.py:1781`, `app/escala/models.py:1784`, `app/ponto/portas/celula.py:693`, `app/ponto/portas/celula.py:698`)

## 04/11 15:13 · sessao caedc4e36b15 · - · - · origem=Ronald (tmux)

5. Classes conferidas no diff:

- **Segundo juiz:** não nasce nova derivação no trecho; `montar_semanas` lê D4 e copia status/rótulo. O juiz já está registrado e `_status_dia` foi removida, deixando lápide. (`app/ponto/services/espelho.py:152`, `app/ponto/services/espelho.py:177`, `app/ponto/services/espelho.py:181`, `app/core/juizes.py:187`)
- **Segundo escritor fora da porta:** o trecho novo escreve apenas o dicionário de apresentação; a escrita persistente continua na porta de célula. Não há novo escritor de `CelulaDia.status_dia/status_rotulo` nesse trecho. (`app/ponto/services/espelho.py:181`, `app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:709`)
- **Fonte única:** o novo caminho lê a resposta do D4; preserva-se o vocabulário antigo no detector para acusar sua volta, e a retirada de `DECLARADOS` corresponde à eliminação da função. (`app/ponto/services/espelho.py:177`, `app/core/medida_leitor_do_dia.py:131`, `app/core/medida_leitor_do_dia.py:329`)
- **Selo unitário com dublê — limitação, baixa:** ele não morde status, rótulo, passagem de `agora` nem construção correta da carga: substitui ambos e só afirma geometria/cards/padding. Uma atribuição constante de status aos dias reais passaria por seus asserts. Isso corresponde ao escopo declarado de geometria, portanto não reprova isoladamente a migração. (`app/ponto/tests/test_caracterizacao_espelho_unit.py:21`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:27`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:46`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:53`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:68`)
- **Selo dedicado tem resultado discriminante:** os dois casos fixam `irregular`, contrariando as respostas antigas `ok`/`sem_registro`; outro compara status e rótulo com o D4. Não é tudo tautologia/paridade. (`app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:46`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:56`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:64`)
- **Selo auxiliar sem mordida no produto — baixa:** `test_MORDE_o_padrao_de_cor_acusa_palavra_sem_regra` só testa regex contra string local; nenhuma mutação do template o derruba. O teste anterior, porém, lê o template e cobra todas as palavras. (`app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:99`, `app/ponto/tests/test_e4_esp_espelho_le_o_d4.py:110`)
- **RED efetivamente observado, suite verde, pré-requisitos e ordem operacional de trava/pouso:** `[nao verificado]`. Há requisito escrito de reset 18:00, item 3 após E4-8 e OTIMIZA 1–3, com um pouso por vez; isso não prova sua execução. (`app/docs/BACKLOG.md:1732`) A própria regra de revisão exige suite inteira antes da leitura final. (`CLAUDE.md:369`)

6. Pendente reancorado:

- A nova impressão ocorre **exatamente uma vez no fonte alvo**, linha 177; o registro exato também ocorre uma vez, linha 866. Conferi por contagem de texto e AST em memória, sem import de aplicação. (`app/ponto/services/espelho.py:177`, `app/core/juizes.py:866`)
- **Dente sintático preservado:** remover essa substring faz o contrato detectar impressão desaparecida. O teste de duplicidade verifica apenas duplicação de registros `(arquivo, impressão)`. (`app/core/tests/test_contract_juiz_tela.py:34`, `app/core/tests/test_contract_juiz_tela.py:52`)
- **Limitação semântica, baixa:** o contrato não exige chamada executável, posição em `montar_semanas`, cópia da resposta nem ocorrência única; comentário/chamada morta com a impressão bastaria. Logo, continua guardando presença do endereço, não a cura. (`app/core/tests/test_contract_juiz_tela.py:35`, `app/core/tests/test_contract_juiz_tela.py:37`)
- A retenção do pendente é explícita e remete à L-118; não houve baixa silenciosa do registro. A L-118 exige medição de frota, contador e reversão, que o material fornecido não comprova. (`app/core/juizes.py:863`, `app/docs/LEIS.md:139`)

## 04/11 12:25 · sessao b12a53e8fff6 · DEPLOY-[nome]-ELO-DA-CORRENTE · verificador · origem=Code

**Reprovo o aval literal:** há consumidores que continuam ignorando falha de reload, a trava nova não estabelece Single Writer da árvore, e `--so-conferir` pode executar deploy. (`app/core/integrador_lote.py:638`; `app/core/esteira_vigia.py:1267`; `bin/vigia_logins.sh:71`; `bin/trava.sh:64`; `bin/deploy.sh:60`.)

1. **Modos e passagem pela recusa**

   - Normal, `--sem-migrate`, `--sem-sombra`, `--reload-copia` e `--reload-agendado`, quando passam pelas guardas, convergem no mesmo `docker kill -s HUP`, **sem chamar `recusa_restart_com_cron`**. Isso é deliberado para o HUP; não há restart nesses ramos. (`bin/deploy.sh:295`; `bin/deploy.sh:300`; `bin/deploy.sh:315`.)
   - O único `docker restart` fica no ramo `RESTART=1`, imediatamente depois da recusa; há outra recusa antes das migrations. Sem motivo, `--restart` termina com rc 2. (`bin/deploy.sh:72`; `bin/deploy.sh:112`; `bin/deploy.sh:297`; `bin/deploy.sh:298`.)
   - **`--so-conferir` não é reconhecido.** O parser reconhece `--conferir`; a grafia perguntada mantém `SO_CONFERIR=0` e pode chegar a migration e HUP. Argumentos desconhecidos também não são rejeitados. (`bin/deploy.sh:56`; `bin/deploy.sh:60`; `bin/deploy.sh:167`; `bin/deploy.sh:315`.)
   - O verdadeiro `--conferir` termina sem reload e com rc 0, inclusive quando o mensageria não respondeu ao check. Os demais encerramentos anteriores ao sinal são recusas explícitas. (`bin/deploy.sh:147`; `bin/deploy.sh:171`; `bin/deploy.sh:89`.)
   - Há **sucesso possível sem provar a troca dos workers**: lista anterior vazia apenas avisa e pula; lista atual vazia satisfaz “nenhum antigo restou”. A prova posterior abre outro Python por `docker exec`, não consulta os workers HTTP. (`bin/deploy.sh:318`; `bin/deploy.sh:321`; `bin/deploy.sh:324`; `bin/deploy.sh:353`.)

2. **Segundo caminho, fallback e Single Writer**

   Não localizei outro emissor operacional de HUP/restart das três cascas em `bin/`: as emissões encontradas estão no deploy. Falha do HUP encerra; o laço posterior somente observa workers e também encerra por falha. Não há fallback por `ESPERA_CRON_S` nem operação compose nesse fluxo. (`bin/deploy.sh:298`; `bin/deploy.sh:315`; `bin/deploy.sh:320`; `bin/deploy.sh:328`.)

   **Passar pela recusa não garante ausência de cron no instante do restart.** Um envelope pode criar sua marca depois da segunda consulta e antes do comando seguinte: criação e restart não compartilham trava. (`bin/cron_run.sh:9`; `bin/cron_run.sh:11`; `bin/deploy.sh:297`; `bin/deploy.sh:298`.)

   **Single Writer global continua furado:** o vigia usa sua trava própria para executar `git revert`, enquanto a porta da árvore usa `/tmp/cadeia.lock`. A nova trava impede duas passadas desse vigia, mas não impede concorrência com uma cadeia. (`bin/vigia_logins.sh:71`; `bin/vigia_logins.sh:87`; `bin/trava.sh:64`; `bin/molde_fatia/cadeia.sh:11`.)

3. **Guarda de migration do mensageria**

   A guarda fecha corretamente diante de ausência de resposta: qualquer rc diferente de zero vira `PEND_MSG=1`, incluindo rc diferente de 1 e container indisponível. Ela descarta a distinção entre pendência e erro de execução, além de ocultar stderr. (`bin/deploy.sh:141`; `bin/deploy.sh:144`.)

   Isso bloqueia normal, cópia e agendado **antes da migration do core e do reload**. Só conferência e restart são exceções; cópia/agendado não ligam nenhuma dessas exceções. (`bin/deploy.sh:57`; `bin/deploy.sh:79`; `bin/deploy.sh:147`; `bin/deploy.sh:167`.)

   **Recusar às 03:30 é coerente com o contrato:** o HUP publica a árvore viva e não executa a migration existente no comando de boot. Liberá-lo com pendência publicaria código sem cumprir esse pré-requisito. O custo é perder a higiene de memória daquela rodada; necessidade concreta ou ocorrência em produção: **[não verificado]**. (`bin/deploy.sh:187`; `docker-compose.mensageria.yml:20`; `app/config/crons.py:300`; `app/config/crons.py:305`.)

4. **Escritores, leitores e consumidores restantes**

   | Valor | Quem escreve | Quem lê |
   |---|---|---|
   | `RESTART` | Inicialização em 0; apenas `--restart` põe 1. (`bin/deploy.sh:67`; `:70`.) | Recusa inicial, exceção da guarda, escolha do sinal e mensagem final. (`bin/deploy.sh:112`; `:147`; `:295`; `:400`.) |
   | `PEND_MSG` | Resultado do check: sucesso 0; qualquer falha 1. (`bin/deploy.sh:141`; `:144`.) | Diagnóstico e guarda. (`bin/deploy.sh:146`; `:147`.) |
   | `logs/crons_em_curso` | Envelope cria/escreve e remove por `trap EXIT`; **deploy também remove** marcas cujo PID considera morto. (`bin/cron_run.sh:9`; `:10`; `bin/deploy.sh:106`.) | `em_curso`, chamado pela recusa. (`bin/deploy.sh:105`; `:108`.) |
   | `logs/vigia_logins.lock` | Vigia abre em append e adquire flock; selo ocupa o mesmo arquivo no fixture. (`bin/vigia_logins.sh:71`; `:72`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:58`.) | O flock decide entrada ou recusa. (`bin/vigia_logins.sh:72`.) |
   | `deploy: OK` | Ambos os finais do deploy preservam o prefixo. (`bin/deploy.sh:401`; `:403`.) | Os dois moldes procuram apenas `^deploy: OK`: continuam compatíveis. (`bin/molde_fatia/cadeia.sh:90`; `bin/molde_autocura/cadeia.sh:87`.) |
   | rc em `_subir` | `_rodar` devolve o returncode real; `_subir` o captura. (`app/core/integrador_lote.py:247`; `:753`.) | Falha grava diagnóstico e retorna antes de FIM/NO_AR; `_fechar` persiste o registro. (`app/core/integrador_lote.py:754`; `:761`; `:763`; `:766`; `:825`.) |

   **Consumidores não migrados e afirmações excessivas:**

   - O integrador ignora o rc do `--reload-copia` e começa a suíte mesmo após recusa do reload. (`app/core/integrador_lote.py:638`; `:639`.)
   - `esteira_vigia.auto_revert` ignora os resultados dos dois deploys e pode anunciar restauração da tela com base apenas nos testes. (`app/core/esteira_vigia.py:1267`; `:1269`; `:1275`; `:1287`.)
   - Os dois `desfazer_copia` usam `set +e`, ignoram o rc do reload e anunciam “recarregada”. (`bin/molde_fatia/cadeia.sh:26`; `:32`; `:33`; `bin/molde_autocura/cadeia.sh:25`; `:31`; `:32`.)
   - O CLI do integrador ainda devolve 0 depois de `rodar`, mesmo com veredito `deploy falhou`. (`app/core/integrador_lote.py:856`; `:858`.)
   - A mensagem nova “PROD SEGUE no código de antes” **não decorre de rc não zero**: pode haver falha depois do sinal, nas rotas ou no selo. O estado efetivo nesse caso é **[não verificado]**. (`app/core/integrador_lote.py:758`; `bin/deploy.sh:315`; `:374`; `:381`.)

5. **Os selos mordem, mas deixam lacunas**

   O selo do deploy tem mordida sobre a escolha do comando: exige sucesso, um HUP, zero restart e três trocas; o restart falso mata os sleeps registrados. Porém, a sobrevivência ao HUP é comportamento implementado pelo próprio falso, não prova do runtime Docker. (`bin/tests/test_deploy_reload_gracioso.sh:62`; `:64`; `:67`; `:103`.)

   Há cobertura ausente para cópia, conferência, mensageria com rc diferente de 1, HUP falho e workers sem troca. O sleep também pode terminar antes do sinal se a execução demorar, tornando vácua a parte temporal da sobrevivência. (`bin/tests/test_deploy_reload_gracioso.sh:83`; `:91`; `:111`; `:145`.)

   O selo do vigia possui controle positivo útil e verifica recusa com trava ocupada. Mas o deploy falso termina imediatamente: não testa manutenção da trava durante um deploy demorado nem detecta retirada do `7>&-`. O caso `Revert` só exige ausência de commits/deploy, sem exigir mensagem ou rc do ramo esperado. (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:30`; `:52`; `:62`; `:70`; `:71`.)

   **Existe risco condicional de tocar Docker real:** ambos preservam o restante do `PATH`, sem verificar qual executável foi resolvido. Se o stub não puder executar — por exemplo, temporário em filesystem `noexec` — a busca pode alcançar um Docker posterior no `PATH`; o deploy copiado mantém os nomes reais das cascas. As guardas de raiz não isolam o daemon. Condições reais desse cenário: **[não verificado]**. (`bin/tests/test_deploy_reload_gracioso.sh:36`; `:73`; `:88`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:26`; `:38`; `:45`; `bin/deploy.sh:315`.)

   O teste Python acrescentado também é textual: remover o retorno antecipado de `_subir` não viola suas asserções sobre presença e ordem das strings. (`app/core/tests/test_selo_pronta_nao_e_residuo.py:219`; `:224`; `app/core/integrador_lote.py:761`.)

   Execução GREEN/RED dos dois selos: **[não verificado]**; eles criam e removem arquivos, contrariando a regra desta verificação. (`bin/tests/test_deploy_reload_gracioso.sh:33`; `:98`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:22`; `:54`.)

6. **fd 7, fechamento e `Revert "`**

   O uso mostrado está correto para serializar este vigia: o pai mantém o fd 7 aberto enquanto espera; `7>&-` fecha apenas o descritor recebido pelo deploy. A trava cobre as leituras decisórias, o revert e a espera pela publicação. (`bin/vigia_logins.sh:71`; `:76`; `:87`.)

   A guarda é apenas um **prefixo literal e sensível a maiúsculas**: títulos editados podem escapar, e um commit comum intitulado `Revert "..."` também será bloqueado. O fixture testa justamente esse segundo caso, criando um commit comum com esse título. (`bin/vigia_logins.sh:78`; `:80`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:42`; `:70`.)

   **O buraco principal é de recuperação:** se o revert funcionar e o deploy falhar antes do HUP, a próxima passada vê `Revert "` e não tenta publicar novamente. A reversão pode ficar somente no disco, sem retry automático; o selo não cobre porque seu deploy sempre retorna zero. (`bin/vigia_logins.sh:80`; `:87`; `bin/deploy.sh:147`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:30`.)

## 04/11 12:23 · sessao 9bfce67a1876 · - · - · origem=Ronald (tmux)

Achados dos itens 5–6 (análise estática; execução GREEN/RED e condições reais de produção [não verificado]):

1. **Buraco de recuperação:** se `git revert` funcionar e o deploy falhar antes do HUP, o próximo ciclo encontra `Revert "` e pula também a publicação. Assim, pode deixar o rollback apenas no disco, com os workers anteriores ainda vivos. O ramo de erro só imprime mensagem; não registra publicação pendente nem tenta publicar novamente (`bin/vigia_logins.sh:80–87`; exemplo de falha antes do HUP: `bin/deploy.sh:147–149`). O selo não cobre: seu deploy falso sempre retorna zero (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:30`).

2. **Os selos podem alcançar Docker real sob condição de ambiente:** os stubs são escolhidos exclusivamente por prefixo de `PATH`, preservando o restante; não há verificação de que o `docker` resolvido é o falso (`bin/tests/test_deploy_reload_gracioso.sh:73–89`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:38–45`). Se o temporário estiver em filesystem `noexec` ou o stub não puder executar, a busca pode encontrar outro Docker no `PATH`. O selo do deploy mantém os nomes das cascas reais, e o script copiado contém o HUP/restart desses nomes (`bin/deploy.sh:298`, `bin/deploy.sh:315`). As guardas de raiz só procuram o literal `/home/ronald/saas-hasner`; não isolam o daemon Docker (`bin/tests/test_deploy_reload_gracioso.sh:36–39`; `bin/tests/test_vigia_logins_sem_sobreposicao.sh:25–28`). Existência de Docker real acessível/noexec neste ambiente: [não verificado].

3. **O selo do deploy morde regressões de escolha do comando:** exige rc zero, um HUP, zero restart e três mensagens de troca nos modos que exercita; o restart falso mata os sleeps registrados (`bin/tests/test_deploy_reload_gracioso.sh:64–69`, `bin/tests/test_deploy_reload_gracioso.sh:101–117`). Entretanto:
   - Não exercita `--reload-copia`, `--conferir`, rc de mensageria diferente de 1, falha do HUP ou workers que não trocam (`bin/tests/test_deploy_reload_gracioso.sh:110–150`).
   - “O exec sobreviveu ao HUP” é comportamento imposto pelo próprio falso; o ramo `kill` só avança gerações. Não é prova do runtime Docker (`bin/tests/test_deploy_reload_gracioso.sh:59–69`).
   - O sleep dura 5/9 s e não há confirmação de que ainda estava vivo no instante do sinal: essa parte pode ser temporalmente vácua se o deploy demorar (`bin/tests/test_deploy_reload_gracioso.sh:83–91`, `bin/tests/test_deploy_reload_gracioso.sh:111–120`).

4. **O selo do vigia possui controle útil, mas cobertura incompleta:** o primeiro caso exige que o ramo automático faça um novo commit e uma chamada de deploy; o segundo exige recusa com lock externo ocupado (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:50–65`). Contudo, o deploy falso retorna imediatamente, portanto remover `7>&-` ou liberar a trava antes do término do deploy não seria detectado por esses casos (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:30`, `bin/tests/test_vigia_logins_sem_sobreposicao.sh:58–65`). O caso `Revert` só exige ausência de commits/deploy: não exige rc nem mensagem esperados, aceitando também erro que ocorra nesse caso (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:70–72`).

5. **fd 7/fechamento estão corretos no caminho mostrado:** abre o arquivo em append, adquire `flock -n 7` antes das leituras usadas para decidir o revert e mantém o descritor do processo pai até terminar. `7>&-` fecha somente a cópia do descritor recebida pelo deploy; o pai continua segurando a trava enquanto aguarda o comando (`bin/vigia_logins.sh:71–87`). Isso coordena passadas deste vigia; outros escritores da árvore não adquirem essa trava automaticamente (`bin/vigia_logins.sh:71–87`).

6. **A guarda `Revert "` é apenas um prefixo literal e sensível a maiúsculas:** cobre o assunto convencional produzido pelo próprio `git revert --no-edit`, mas não identifica semanticamente reversões com título editado; também bloqueia qualquer commit normal cujo título comece assim (`bin/vigia_logins.sh:78–87`). O fixture cria um commit normal intitulado `Revert "..."`, não uma reversão real, e por isso testa precisamente o prefixo (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:39–42`, `bin/tests/test_vigia_logins_sem_sobreposicao.sh:70`).

## 04/11 12:22 · sessao c2861ff43f2f · - · - · origem=Ronald (tmux)

Mapa de escritores/leitores e falhas:

- **RESTART**: valor local inicializado em `0`, alterado para `1` apenas por `MODO=--restart`; motivo obrigatório (`bin/deploy.sh:67-72`). Leitores: primeira recusa (`bin/deploy.sh:112-114`), exceção da guarda mensageria (`bin/deploy.sh:147`), escolha restart/HUP (`bin/deploy.sh:295-315`), frase final (`bin/deploy.sh:400-403`). O contrato examina textualmente esse ramo (`bin/contrato_deploy.sh:41-49`).

- **PEND_MSG**: escrito pelo resultado de `docker exec mensageria … migrate --check`, `0` em sucesso e `1` em qualquer falha (`bin/deploy.sh:141-145`); lido na saída e na guarda (`bin/deploy.sh:146-149`). O fake do selo fornece o rc por `FAKE_PEND_MSG` (`bin/tests/test_deploy_reload_gracioso.sh:55`).

- **logs/crons_em_curso**: envelope cria diretório/marca `${N}_${E}.$$`, escreve horário e remove em `trap EXIT` (`bin/cron_run.sh:9-10`). Deploy percorre marcas e consulta `/proc/<pid>`; **também escreve**, removendo marcas consideradas mortas (`bin/deploy.sh:105-106`). Leitura para recusa usa `em_curso` (`bin/deploy.sh:107-110`). O selo cria marca falsa de PID vivo (`bin/tests/test_deploy_reload_gracioso.sh:85`).

- **logs/vigia_logins.lock**: `vigia_logins` abre fd 7 e tenta flock exclusivo não bloqueante (`bin/vigia_logins.sh:71-75`); o selo mantém a mesma trava ocupada no fixture (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:58`). A trava cobre as leituras decisórias e o revert/deploy (`bin/vigia_logins.sh:76-87`).

- **“deploy: OK”**: escritores preservam prefixo nos dois modos (`bin/deploy.sh:401-403`); leitores usam apenas `^deploy: OK`, portanto compatíveis com nova redação (`bin/molde_fatia/cadeia.sh:89-90`, `bin/molde_autocura/cadeia.sh:86-87`). Ambos têm `set -e`, além da conferência textual (`bin/molde_fatia/cadeia.sh:4`, `bin/molde_autocura/cadeia.sh:3`).

- **rc em `_subir`**: `_rodar` retorna o `subprocess.run(...).returncode` real (`app/core/integrador_lote.py:242-248`); novo ramo lê o rc, grava `veredito`/`deploy_rc` e retorna antes de FIM/NO_AR (`app/core/integrador_lote.py:753-766`). `_fechar` persiste o registro em LOTES e no log (`app/core/integrador_lote.py:825-829`). Entretanto, `main rodar` sempre retorna `0`, portanto o shell ainda recebe sucesso mesmo com veredito `deploy falhou` (`app/core/integrador_lote.py:856-858`, `bin/integrador.sh:52`).

Falhas a destacar:

1. **Consumidor não migrado: reload da cópia do integrador ignora rc** e entra na suite imediatamente, mesmo quando a nova guarda recusou reload (`app/core/integrador_lote.py:638-640`).

2. **Consumidor não migrado: `esteira_vigia.auto_revert` ignora rc dos dois deploys**, inicial e revert do revert (`app/core/esteira_vigia.py:1267-1268`, `app/core/esteira_vigia.py:1275`). Pode concluir “a tela volta” porque testes passaram sem provar publicação (`app/core/esteira_vigia.py:1269-1273`, `app/core/esteira_vigia.py:1287-1291`).

3. **Alarme novo afirma estado de prod que o rc não prova.** Para qualquer rc não zero informa “PROD SEGUE no codigo de antes do lote” (`app/core/integrador_lote.py:754-760`), porém falhas podem acontecer depois de restart/HUP, na espera de workers, rotas ou selo (`bin/deploy.sh:298`, `bin/deploy.sh:315`, `bin/deploy.sh:328`, `bin/deploy.sh:374`, `bin/deploy.sh:381`). Estado efetivo nessas falhas: **[não verificado]**.

4. **Trava do vigia não estabelece Single Writer global.** Vigia usa trava própria e faz `git revert` (`bin/vigia_logins.sh:71-87`), enquanto a porta compartilhada da árvore é `/tmp/cadeia.lock` (`bin/trava.sh:57-70`), usada pelas cadeias (`bin/molde_fatia/cadeia.sh:11`, `bin/molde_autocura/cadeia.sh:10`). `auto_revert` também reverte sem adquirir essa porta dentro da função (`app/core/esteira_vigia.py:1242-1291`). Sobreposição real em prod: **[não verificado]**.

## 04/11 12:22 · sessao 00bdd655fe03 · - · - · origem=Ronald (tmux)

1. **Existe HUP sem `recusa_restart_com_cron`, por desenho:** normal, `--sem-migrate`, `--sem-sombra`, `--reload-copia` e `--reload-agendado` convergem no único `docker kill -s HUP`; a recusa só é chamada para `RESTART=1` (`bin/deploy.sh:112-114`, `bin/deploy.sh:295-315`). O único `docker restart` está imediatamente após a segunda recusa (`bin/deploy.sh:297-298`).

   **`--so-conferir` não existe no parser.** Só `--conferir` liga `SO_CONFERIR`; `--so-conferir` atravessa o fluxo normal, podendo migrar e emitir HUP (`bin/deploy.sh:56-62`, `bin/deploy.sh:167`, `bin/deploy.sh:171-174`, `bin/deploy.sh:315`). Não encontrei um modo válido silenciosamente esquecendo reload: as saídas anteriores são recusas e o modo de conferência (`bin/deploy.sh:89`, `bin/deploy.sh:147-174`, `bin/deploy.sh:213-235`, `bin/deploy.sh:267-273`).

2. **No deploy não há retry de sinal nem fallback restart:** HUP falho chama `falhou`; o laço apenas observa PIDs e também termina em falha (`bin/deploy.sh:315-328`). O único restart explícito é `bin/deploy.sh:298`; os outros comandos Docker de partida localizados em scripts operacionais são para os bancos de teste/sombra (`bin/db_teste.sh:70`, `bin/sombra_db.sh:100`). Não há operação compose no fluxo de deploy; só comentários indicando o ato separado de infraestrutura (`bin/deploy.sh:20-22`, `bin/deploy.sh:65-66`).

   **Limitação Single Writer:** as duas recusas não excluem um cron que comece entre checagem e restart; o envelope cria a marca sem trava compartilhada, e a segunda consulta é seguida pelo restart (`bin/cron_run.sh:9-11`, `bin/deploy.sh:105-110`, `bin/deploy.sh:297-298`). Há passagem pela recusa, mas não exclusão mútua com o início dos crons nesses pontos.

   **Prova de workers permite vazio:** PIDs anteriores vazios só avisam e pulam; consulta atual vazia satisfaz “nenhum antigo restou” e aprova (`bin/deploy.sh:318`, `bin/deploy.sh:321-324`). A prova de rota executa um processo Python novo por `docker exec`, sem consultar o worker HTTP (`bin/deploy.sh:353-356`).

3. **Guarda mensageria:** qualquer rc não zero, incluindo container indisponível e rc diferente de 1, define `PEND_MSG=1`; bloqueia antes da migration do core e do reload em todos os modos, salvo `--conferir` e `--restart` (`bin/deploy.sh:141-150`, `bin/deploy.sh:167`). Isso inclui agendado e cópia, pois os dois mantêm `SO_CONFERIR=0`, `RESTART=0` (`bin/deploy.sh:57-79`). Conferência pode sair zero mesmo sem resposta do mensageria, apenas imprimindo o estado (`bin/deploy.sh:146-147`, `bin/deploy.sh:171-174`).

   Recusar às 03:30 com migration pendente é coerente com o desenho registrado: o cron declara que não deve publicar fatia inacabada, o HUP publica a árvore viva, e a migration mensageria está no comando de boot (`app/config/crons.py:303-308`, `bin/deploy.sh:187-189`, `docker-compose.mensageria.yml:20`). O custo é deixar a higiene de memória sem ocorrer até correção; o próprio propósito desse cron é pagar o vazamento retirado da reciclagem por contagem (`app/config/crons.py:300-306`). Ocorrência real em produção: [não verificado].

**Nota para consolidação do item 4:** rc não zero não prova “PROD SEGUE no código de antes”: erros podem acontecer depois de emitir HUP, por rota ou selo (`bin/deploy.sh:315`, `bin/deploy.sh:374`, `bin/deploy.sh:381`); a mensagem do integrador é forte demais nesse aspecto (`app/core/integrador_lote.py:757-760`).

## 04/11 11:33 · sessao c5de69a10b63 · DEPLOY-[nome]-ELO · verificador · origem=Code

1. **Veredito: bloqueia. As curas da marca e da pauta são parciais.** Há caminho para restart com cliente vivo e caminho para pauta aberta sem fechamento automático (`bin/em_casca.sh:28-38`; `bin/admissao_cron.sh:202-208`; `bin/deploy.sh:368-375`). Execução da suíte, morte efetiva dos mutantes e ocorrência desses caminhos em produção: **[nao verificado]**.

   Para cada PROCEDENTE da seção `codex_v2.r`:

   | Item | Resultado em e8f72370 | Gravidade |
   |---|---|---|
   | SIGKILL com exec vivo | Cura quando a transferência **já terminou**. Continua descoberto entre lançar o filho e publicar sua marca, ou quando `sed/mv` falha. A transferência ainda introduz a corrida de varredura descrita no item 2 (`bin/em_casca.sh:28-38`). | **bloqueia** |
   | Pauta perdida; resolver falhando | Resolver não zero conserva a recusa. Entretanto, pauta pode nascer sem recusa ou depois de sua remoção; nesses casos `_tira_recusa` não a alcança (`bin/deploy.sh:368-375`; `app/core/integrador_lote.py:773-780`). | **bloqueia** |
   | CLI por AST aceitava `!=` | O caso agora executa a porta positiva e rejeita a inversão direta dessa condição. Falta testar outra porta; uma condição excessivamente abrangente ainda passa (`bin/tests/test_deploy_nao_mata_elo.sh:930-934`; `app/core/esteira_vigia.py:1652`). | Curado no alvo específico; lacuna em **fila** |
   | Falha da pauta do lote | O cenário injeta 70 e exige aviso no relato; a produção trata qualquer retorno verdadeiro de `_pauta` (`bin/tests/test_deploy_nao_mata_elo.sh:789-792`; `app/core/integrador_lote.py:780-781`). | Curado por leitura; execução **[nao verificado]** |
   | Justificativa da espera por env | A correção procede: `ADM_ESPERA_ADMISSAO_S` pode alongar a espera, pois não há limite superior. Isso não altera o teto declarado do restart (`bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:62-68`, `bin/admissao_cron.sh:243`). | **fila** |

   A regressão mais direta é o segundo escritor de marcas, fora da trava usada pela porta original: publicar a marca nova antes de remover a velha **não garante** que uma varredura veja alguma delas (`bin/em_casca.sh:33-36`; `bin/admissao_cron.sh:202-203`).

2. **Caminhos de `bin/em_casca.sh`.**

   | Caminho | Marca, processo e retorno | Gravidade |
   |---|---|---|
   | Filho termina antes da leitura do starttime | Se `/proc` já sumiu, não transfere; `wait` recupera o resultado e EXIT normalmente remove a marca do wrapper. Não resta comando vivo nesse caminho normal (`bin/admissao_cron.sh:84-91`; `bin/em_casca.sh:23`, `bin/em_casca.sh:32`, `bin/em_casca.sh:41-42`). | Sem bloqueio nesse caminho |
   | `sed` ou `mv` falha | O comando **já começou**; permanece somente a marca do wrapper. Enquanto ele vive, protege. Se morrer antes do filho, o reaper remove essa marca pelo PID morto do wrapper, deixando o cliente vivo sem proteção (`bin/em_casca.sh:28`, `bin/em_casca.sh:34-38`; `bin/admissao_cron.sh:169-175`, `bin/admissao_cron.sh:208`). | **bloqueia** |
   | SIGTERM no wrapper | Há somente trap EXIT. Após transferência, preserva a marca do filho vivo; antes dela ou no caminho de falha, preserva um arquivo cuja identidade ainda é a do wrapper morto. O retorno observado é o término do wrapper, não o resultado futuro do comando (`bin/em_casca.sh:23`, `bin/em_casca.sh:28-38`). | **bloqueia** antes da transferência; retorno do comando **[nao verificado]** após o sinal |
   | SIGKILL no wrapper | Depois da transferência, a marca continua identificando o filho. Antes dela, ou após falha de transferência, a marca identifica o wrapper morto e pode ser recolhida enquanto o cliente vive (`bin/em_casca.sh:28-38`; `bin/admissao_cron.sh:169-177`). | **bloqueia** |
   | Filho acaba após SIGKILL do wrapper | Não há wrapper para desmarcar: fica arquivo órfão até a próxima varredura, que verifica identidade e o remove (`bin/em_casca.sh:23`, `bin/em_casca.sh:33-36`; `bin/admissao_cron.sh:199-208`). | **fila**, comportamento de reaper |
   | PID do filho reutilizado | EXIT usa apenas `kill -0`, podendo conservar arquivo por causa de outro processo. O reaper compara starttime; se a reutilização anteceder a captura, pode registrar a identidade substituta. `exe` é substring da cmdline, não identidade do executável (`bin/em_casca.sh:23`, `bin/em_casca.sh:32-34`; `bin/admissao_cron.sh:175-177`). | **fila** |
   | rc 64 | Uso/nome inválido sai antes de marcar. Alvo fora das cascas marca primeiro, mas sai antes de lançar o comando e desmarca pelo EXIT (`bin/em_casca.sh:12-13`, `bin/em_casca.sh:21-28`). | Sem execução escondida |
   | Admissão 75/70 | Retorna antes de lançar o comando. 75 vem da espera pela trava; 70, das falhas de preparação/publicação da marca (`bin/em_casca.sh:16-19`; `bin/admissao_cron.sh:241-248`). | Retornos previstos |
   | Execução normal | O resultado de `wait` é devolvido; o resultado da transferência não o substitui (`bin/em_casca.sh:41-42`). | Sem troca de rc nesse caminho |

   **A corrida existe mesmo sem sinal:** a varredura final expande o glob contendo apenas a marca velha; o wrapper publica a nova e remove a velha; o teste `-e` pula a velha desaparecida; a nova não pertence à enumeração. A varredura retorna vazio e o restart começa com o cliente vivo (`bin/admissao_cron.sh:202-203`; `bin/em_casca.sh:35-36`; `bin/reinicio_sob_admissao.sh:43-45`, `bin/reinicio_sob_admissao.sh:66`). **Bloqueia.**

   **Sim, a marca nova também pode ser publicada depois da verificação vazia:** se o wrapper morrer após lançar o processo externo `mv`, mas antes de ele publicar, a varredura pode recolher a marca do wrapper morto e concluir vazio; o `mv` remanescente publica sem consultar fd8 (`bin/em_casca.sh:35`; `bin/admissao_cron.sh:170`, `bin/admissao_cron.sh:208`; `bin/reinicio_sob_admissao.sh:44-45`). É uma interleaving deduzida do código; reprodução e ocorrência em prod **[nao verificado]**.

3. **Quem escreve, lê, remove e trata os retornos.**

   **Marcas `logs/crons_em_curso/*`:**

   | Papel | Implementação |
   |---|---|
   | Escritor original | `adm_marcar` publica `${nome}_${emp}.$$`, com `ini`, `pid`, `starttime`, `exe`, `casca` e `cmd`, usando temporário e `mv` sob fd8 (`bin/admissao_cron.sh:239-250`). |
   | Chamadores da porta | Comando e alarme do cron; descoberta da corrente; `em_casca` (`bin/cron_run.sh:16`, `bin/cron_run.sh:52`; `bin/encadeado.sh:48`; `bin/em_casca.sh:16`). |
   | Segundo escritor | `em_casca` publica `${N}_-.${filho}`, altera pid/starttime/exe, conserva os demais campos e remove a marca anterior, sem retomar fd8 (`bin/em_casca.sh:33-36`). |
   | Leitor da identidade | `adm_marca_viva` usa o PID **do nome do arquivo**; não usa `pid=`. Compara starttime e procura `exe` na cmdline (`bin/admissao_cron.sh:169-177`). |
   | Leitor da proteção | `adm_segura` lê `casca`; formato antigo segura; `casca=0` ainda consulta descendentes (`bin/admissao_cron.sh:181-186`). |
   | Varredor/reaper | `adm_varrer` verifica identidade, desconsidera ancestrais como bloqueadores e remove órfãs (`bin/admissao_cron.sh:199-208`). |
   | Consumidor do restart | `adm_seguram` filtra a varredura; o restart consulta antes e depois de tomar fd8 (`bin/admissao_cron.sh:213`; `bin/reinicio_sob_admissao.sh:40-45`). |
   | Remoção explícita | `adm_desmarcar`, chamado pelos envelopes e pelo EXIT (`bin/admissao_cron.sh:254`; `bin/cron_run.sh:22`, `bin/cron_run.sh:51`, `bin/cron_run.sh:58`; `bin/encadeado.sh:50`; `bin/em_casca.sh:23`). |

   O contrato verifica a presença textual dessa infraestrutura; os casos do selo também leem as marcas nas raízes falsas (`bin/contrato_deploy.sh:55-57`; `bin/tests/test_deploy_nao_mata_elo.sh:635`, `bin/tests/test_deploy_nao_mata_elo.sh:675`).

   **Recusas e pautas:**

   | Estado | Cria/abre | Lê | Apaga/fecha |
   |---|---|---|---|
   | `recusa_cron.<id>.pendente` | Deploy, somente no ramo restart75, com ID e argv separados por NUL (`bin/deploy.sh:284-287`). | Agendado valida ID e lê argv; retentar lê `auto_revert`; contador conta nomes; limpeza lê ID/mtime (`bin/deploy_agendado.sh:83-87`, `bin/deploy_agendado.sh:101`; `app/core/esteira_vigia.py:1344-1354`; `bin/deploy_recusado.sh:22-27`; `bin/deploy.sh:369`, `bin/deploy.sh:382`). | Deploy por `_tira_recusa`; **também agendado**, preparando IDs e substituindo a recusa anterior (`bin/deploy.sh:375-382`; `bin/deploy_agendado.sh:95`, `bin/deploy_agendado.sh:104`, `bin/deploy_agendado.sh:110`). |
   | `deploy_recusado:integrador` | `_deploy_do_lote` (`app/core/integrador_lote.py:780`). | `pauta_viva` (`app/core/management/commands/alarme_esteira.py:13-15`). | `_tira_recusa` chama o resolver (`bin/deploy.sh:370-375`). |
   | `deploy_recusado:auto_revert` | Primeiro deploy e retentativa (`app/core/esteira_vigia.py:1331`, `app/core/esteira_vigia.py:1356`). | Mesmo leitor (`app/core/management/commands/alarme_esteira.py:13-15`). | Mesmo resolvedor (`bin/deploy.sh:370-375`). |
   | `deploy_recusado:vigia_logins` | Falha do deploy do revert (`bin/vigia_logins.sh:91-95`). | Mesmo leitor (`app/core/management/commands/alarme_esteira.py:13-15`). | Mesmo resolvedor (`bin/deploy.sh:370-375`). |

   A escrita da pauta continua pela porta `escrever`; o fechamento passa por `marcar_feita_pelo_sistema`. Não surgiu escrita direta de `feito_em` nesse trecho (`app/core/management/commands/alarme_esteira.py:27-31`, `app/core/management/commands/alarme_esteira.py:54-56`; `app/pautas/services.py:269-275`).

   **Os três IDs que abrem pauta podem ficar sem fechamento automático**, por dois caminhos:

   - Todos abrem para rc diferente de zero, mas somente restart75 produz recusa. Falha sem arquivo deixa `_tira_recusa` sem entrada para resolver (`app/core/integrador_lote.py:773-780`; `app/core/esteira_vigia.py:1326-1331`; `bin/vigia_logins.sh:91-95`; `bin/deploy.sh:284-290`, `bin/deploy.sh:368`). **Bloqueia.**
   - Outro deploy pode resolver antes de o chamador abrir a pauta e apagar a recusa. Resolver pauta inexistente retorna normalmente; depois o chamador abre a pauta sem arquivo restante (`app/core/management/commands/alarme_esteira.py:28-33`; `bin/deploy.sh:370-375`; `app/core/esteira_vigia.py:1325-1331`). **Bloqueia.**

   `manual` e `agendado.*` também produzem recusas, mas os abridores acima usam os três IDs fixos; não há abertura correspondente a cada ID do agendado (`bin/deploy.sh:52-54`; `bin/deploy_agendado.sh:94-105`; `app/core/integrador_lote.py:780`; `bin/vigia_logins.sh:95`).

   **Retorno de `deploy.sh` nos chamadores:** [nao verificado]

   | Chamador | Tratamento |
   |---|---|
   | Integrador: reload inicial | Descarta rc e segue para a suíte (`app/core/integrador_lote.py:638-639`). **Fila.** |
   | Integrador: publicação final | Lê rc; falha impede FIM e retirada da fila (`app/core/integrador_lote.py:748-752`, `app/core/integrador_lote.py:771-782`). |
   | Auto-revert inicial | Lê rc e interrompe antes da conferência em falha (`app/core/esteira_vigia.py:1276-1284`). |
   | Revert do revert | Lê rc e tenta incluí-lo na pauta `auto_revert`; pauta já viva não é reescrita (`app/core/esteira_vigia.py:1289-1294`; `app/core/management/commands/alarme_esteira.py:38-41`). **Fila.** |
   | Retentar | Lê rc; CLI retorna 0 e vigia usa `tail`, sem decidir pelo status (`app/core/esteira_vigia.py:1354-1362`, `app/core/esteira_vigia.py:1652-1654`; `bin/vigia_arvore.sh:28`). |
   | Vigia de logins | Lê rc e abre pauta em falha; o próprio vigia termina em 2 nesse caminho de alarme (`bin/vigia_logins.sh:90-96`, `bin/vigia_logins.sh:105`). |
   | Agendado | Captura rc da esteira e da repetição; repetição bem-sucedida vira 3 pela cauda não executada; devolve `PIOR` (`bin/deploy_agendado.sh:96-106`, `bin/deploy_agendado.sh:123-135`). |
   | Dois moldes de cadeia | Reload inicial guardado; publicação final sujeita a `set -e`; rollback ignora rc sob `set +e` (`bin/molde_fatia/cadeia.sh:4`, `bin/molde_fatia/cadeia.sh:26-33`, `bin/molde_fatia/cadeia.sh:48`, `bin/molde_fatia/cadeia.sh:89`; `bin/molde_autocura/cadeia.sh:3`, `bin/molde_autocura/cadeia.sh:25-32`, `bin/molde_autocura/cadeia.sh:45`, `bin/molde_autocura/cadeia.sh:86`). Rollbacks: **fila**. |
   | `o135-teto20` | Conferência guardada; publicação captura e propaga rc (`fatias_agendadas/o135-teto20/esteira.sh:80`, `fatias_agendadas/o135-teto20/esteira.sh:116-124`). |
   | `abono-no-ar` | `pipefail` e condicional convertem falha do deploy em 1 (`fatias_agendadas/abono-no-ar/esteira.sh:21`, `fatias_agendadas/abono-no-ar/esteira.sh:37-38`). |
   | Reload diário RAW | Passa pelo envelope que registra e devolve rc (`app/config/crons.py:307`; `bin/cron_run.sh:24`, `bin/cron_run.sh:29`, `bin/cron_run.sh:70`). |

   **RC do resolver:** qualquer não zero é reduzido a 1 em `_tira_recusa`, conservando arquivo. Esse 1 é ignorado pelos chamadores internos, e o deploy termina em `echo`, devolvendo 0; portanto rc0 não prova pauta fechada nem recusa removida (`bin/deploy.sh:370-385`). **Fila**, pois o restart pode ter passado e a pendência continua visível.

4. **Os casos mordem alguns erros, mas deixam estas mutações plausíveis passar.** Sobrevivência experimental dos mutantes: **[nao verificado]**; abaixo, dedução por leitura.

   | Caso | Mordida e mutação sobrevivente |
   |---|---|
   | 19(a) | Cobra marca protetora, recusa75 e zero restart. Remover a transferência ainda passa esse caso isolado: o wrapper continua vivo; o laço não exige que encontrou PID diferente (`bin/tests/test_deploy_nao_mata_elo.sh:632-640`). |
   | 19(f) | Cobra SIGKILL **depois** de esperar a transferência. Trocar EXIT por `trap adm_desmarcar EXIT` passa (a)/(f), mas quebra TERM com filho vivo; SIGKILL não executa EXIT (`bin/tests/test_deploy_nao_mata_elo.sh:670-676`; `bin/em_casca.sh:23`). |
   | 21 limpeza | Executa o trecho real e verifica os resolvedores. Apagar a criação real do marcador passa: o teste fabrica `.restart_inicio.$$`; a extração do caso8 também começa depois da criação (`bin/tests/test_deploy_nao_mata_elo.sh:233`, `bin/tests/test_deploy_nao_mata_elo.sh:752-766`; `bin/deploy.sh:282`). |
   | 21 resolver | Injeta somente 0 e 1. Reter arquivo só para rc1, apagando para 70/75, passa esses cenários e perde a pendência em falha de admissão (`bin/tests/test_deploy_nao_mata_elo.sh:765-771`; `bin/deploy.sh:370-375`). |
   | 21 pauta70 | Trocar `if rp` por `if rp == 70` passa e omite aviso para 75; o teste também não exige o número correto no aviso (`app/core/integrador_lote.py:781`; `bin/tests/test_deploy_nao_mata_elo.sh:789-792`). |
   | 23 retentar | Trocar `if rc != 0` por `if rc == 75` passa os cenários 75/0 e anuncia sucesso para rc1 (`app/core/esteira_vigia.py:1355`; `bin/tests/test_deploy_nao_mata_elo.sh:907-917`). |
   | 23 CLI | Usar `if argv[1:2] != ['auto-revert']` aceita retentar e passa a sentinela, mas sequestra outras portas. Não há chamada negativa, apesar do comentário (`app/core/esteira_vigia.py:1652`; `bin/tests/test_deploy_nao_mata_elo.sh:925-934`). |

   As lacunas isoladas são **fila de mordida**; a falta de cobertura das janelas de transferência acompanha o **bloqueio** do item 2 (`bin/tests/test_deploy_nao_mata_elo.sh:670-671`; `bin/em_casca.sh:28-38`).

5. **Classes históricas no delta da volta 3:**

   | Classe | Resultado |
   |---|---|
   | (a) Segundo juiz registrado | Não identifiquei duplicação de juiz de domínio: o novo fechamento chama o comando existente, que usa as portas de pauta (`bin/deploy.sh:370-371`; `app/core/management/commands/alarme_esteira.py:27-31`). O registro vivo, e não a tabela de consulta, é a autoridade declarada (`app/docs/JUIZES.md:10-12`). |
   | (b) Segundo escritor | **Aparece:** transferência escreve marca fora de `adm_marcar` (`bin/em_casca.sh:33-36`; `bin/admissao_cron.sh:239-250`). **Bloqueia.** |
   | (c) Relógio fora da borda | Não identifiquei leitura nova de relógio no trecho de transferência ou limpeza acrescentado (`bin/em_casca.sh:32-38`; `bin/deploy.sh:366-383`). |
   | (d) Derivação paralela | A lista fixa foi retirada; o fechamento deriva ID do arquivo. Essa parte melhora a origem (`bin/deploy.sh:369-371`; `app/core/esteira_vigia.py:1359`). |
   | (e) Selo sem mordida | Há mordida parcial e lacunas concretas, enumeradas no item 4 (`bin/tests/test_deploy_nao_mata_elo.sh:759`, `bin/tests/test_deploy_nao_mata_elo.sh:789-792`, `bin/tests/test_deploy_nao_mata_elo.sh:930-934`). **Fila**, além da prova insuficiente do caminho bloqueante. |
   | (f) Trava/fd | **Aparece:** troca fora de fd8 quebra a garantia da segunda varredura. O fechamento original do fd antes dos filhos continua correto (`bin/admissao_cron.sh:250`; `bin/em_casca.sh:33-36`; `bin/reinicio_sob_admissao.sh:43-45`). **Bloqueia.** |
   | (g) Pouso antes do pré-requisito | Pouso e atendimento dos pré-requisitos operacionais: **[nao verificado]**. O aval os exige antes de PRONTO (`AVAL_DEPLOY-ELO-B3.txt:14`, anexo). | [nao verificado]
   | (h) Fallback/env solta | **Aparece fallback:** falha da transferência mantém a marca do wrapper, insuficiente após sua morte (`bin/em_casca.sh:37-38`). **Bloqueia.** A env sem limite superior permanece na admissão (`bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:243`). **Fila.** |
   | (i) Erro engolido | Não há novo `try/except` nesse trecho; há equivalente em shell: erro de leitura pode virar ID vazio e remoção, e erro do resolver não altera o rc final do deploy (`bin/deploy.sh:369-385`). **Fila.** |
   | (j) Restart com exec vivo | **Aparece** pela troca de marca e permanece no caminho deploy × deploy (`bin/em_casca.sh:33-36`; `bin/reinicio_sob_admissao.sh:44-66`; `bin/deploy.sh:300`, `bin/deploy.sh:316`). **Bloqueia.** |

6. **Triagem para FILA: motivos sustentados e exceções.**

   - **Deploy × deploy: não sustenta como autorização de fechamento. Bloqueia.** A exclusão do vigia inteiro permite que ele lance o deploy automático sem marca permanente protetora; migrate/provas continuam fora da admissão. Podem começar depois da última varredura de outro restart. A existência anterior da corrida em outros chamadores não protege esse caminho, nem revoga a proibição escrita de reiniciar sobre exec vivo (`bin/admissao_cron.sh:36`, `bin/admissao_cron.sh:229-231`; `bin/vigia_arvore.sh:28`; `bin/deploy.sh:102`, `bin/deploy.sh:124`, `bin/deploy.sh:300`; `bin/reinicio_sob_admissao.sh:43-66`; `AVAL_DEPLOY-ELO-B3.txt:13`, anexo).

   - **SIGKILL não trapável: não sustenta a transferência incompleta. Bloqueia.** O problema não exige trapar KILL: exige que o comando lançado continue representado durante toda a janela. Hoje ele começa antes da transferência e há fallback para a identidade do wrapper (`bin/em_casca.sh:28-38`).

   - **Outros consumidores que ignoram `_pauta`: motivo de escopo sustenta fila.** `_pauta` anuncia falha em stderr, mas chamadas continuam sem tratar seu retorno; isso não prova persistência da pauta nesses usos (`app/core/esteira_vigia.py:900-905`, `app/core/esteira_vigia.py:1274`, `app/core/esteira_vigia.py:1291`, `app/core/esteira_vigia.py:1632-1637`). **Fila.**

   - **Revert do revert: fila, com justificativa imprecisa.** O rc pode ser engolido por pauta já viva. Também não há ID explícito: usa o ambiente do processo pai ou `manual`; o `env` local do primeiro deploy não altera esse ambiente (`app/core/esteira_vigia.py:1289-1294`, `app/core/esteira_vigia.py:1324-1325`; `bin/deploy.sh:52`; `app/core/management/commands/alarme_esteira.py:38-41`).

   - **Chave antiga sem sufixo: fila; existência em prod [nao verificado].** O fechamento atual deriva `deploy_recusado:<id>` do arquivo, e os abridores atuais usam sufixos (`bin/deploy.sh:369-371`; `app/core/esteira_vigia.py:1331`; `app/core/integrador_lote.py:780`; `bin/vigia_logins.sh:95`).

   - **Lacunas do caso8, marcador fabricado, segundo deploy só75 e universo derivado do classificador: fila de mordida sustentada.** Os testes cobram existência da recusa, fabricam o marcador, injetam75 no segundo deploy e derivam o universo da própria autoridade (`bin/tests/test_deploy_nao_mata_elo.sh:208`, `bin/tests/test_deploy_nao_mata_elo.sh:759`, `bin/tests/test_

[truncado em 20000 de 24144 caracteres]

## 04/11 11:27 · sessao 931f9f956368 · - · - · origem=Ronald (tmux)

Leitura estática concluída; execução/mutantes/suite verde: **[nao verificado]**. O selo cria e apaga arquivos, portanto não o executei (`bin/tests/test_deploy_nao_mata_elo.sh:44`).

1. **Os casos novos mordem alvos concretos, mas têm lacunas.**
   
   | Caso | O que efetivamente cobra | Mutação plausível que deixa passar |
   |---|---|---|
   | 19(a) | Marca `t19a_-|1`, recusa 75, zero restart, retorno final 0 e diretório vazio (`bin/tests/test_deploy_nao_mata_elo.sh:632-640`). | Remover transferência ao filho ainda passa **19(a)** isoladamente: a marca do wrapper vivo basta; o laço não afirma que encontrou PID diferente (`:634`). O 19(f) acrescentado cobre essa mutação (`:670-676`). |
   | 19(f) | Mata wrapper com SIGKILL depois de esperar a transferência, exige cliente vivo, recusa e limpeza posterior (`:668-676`). | Trocar `bin/em_casca.sh:23` por `trap adm_desmarcar EXIT`: passa (a), que termina normalmente, e (f), cujo SIGKILL não dispara EXIT; quebra SIGTERM com filho vivo. Esses testes não exercitam TERM nem falha de `sed`/`mv` (`:632-640`, `:668-676`; produção `bin/em_casca.sh:34-38`). |
   | 21 limpeza | Executa o trecho real, verifica os dois resolvedores e a sobrevivência apenas da recusa posterior (`:752-767`); com resolver 1 exige conservar as três (`:769-771`). | Apagar a criação real de `.restart_inicio.$$` em `bin/deploy.sh:282`: o teste fabrica o marcador (`:759`), e o caso8 começa a extração depois da criação (`:233`). |
   | 21 resolver | Distingue resolver 0 e 1 (`:765-771`). | Reter recusa somente quando resolver devolve 1, apagando para 70/75: passa esses cenários, mas perde recusa quando admissão falha; produção trata todos os não zero (`bin/deploy.sh:370-375`). |
   | 21 pauta70 | Executa `_deploy_do_lote` com `_pauta` devolvendo 70 e busca `PAUTA NAO ABRIU` (`:789-792`, `:844`). | Em `app/core/integrador_lote.py:781`, substituir `if rp` por `if rp == 70`: passa e omite falha 75. Hardcode de rc no texto também passa: o teste verifica só a expressão, não o número (`:792`). |
   | 23 retentar | Exercita ausência, recusa75 e sucesso0, argv/id e quantidade de relato/pautas (`:904-917`). | Em `app/core/esteira_vigia.py:1355`, substituir `if rc != 0` por `if rc == 75`: passa e anuncia sucesso para rc1. |
   | 23 CLI | Executa `main(['x','retentar-deploy'])` com sentinela e exige rc0 (`:930-934`). | Em `app/core/esteira_vigia.py:1652`, usar `if argv[1:2] != ['auto-revert']`: aceita retentar, passa o teste e sequestra zumbido/outras portas. O comentário promete “e outra porta nao”, mas não há chamada negativa (`:925-934`). |

   **Gravidade:** lacunas de mordida, **fila**, exceto quando deixam sem prova exigência explícita do aval; exigência/mutantes efetivamente mortos do anexo: **[nao verificado]**.

2. **Regressão de composição: o segundo apagador continua burlando o novo contrato de limpeza.** `_tira_recusa` conserva arquivo quando resolver falha (`bin/deploy.sh:370-373`), mas essa falha não vira o resultado do deploy, que termina com `echo` (`:377-385`). O agendado apaga `_rec_ant` sempre que recebe rc0 (`bin/deploy_agendado.sh:110`), podendo remover justamente a recusa que o resolvedor conservou. A explicação “só substitui a própria recusa” não sustenta a alegação geral “só tira se pauta fechou” (`bin/deploy.sh:363-365`). IDs próprios do agendado abrirem pauta: **[nao verificado]**; não afirmar pauta órfã automaticamente nesse cenário. **Gravidade: fila**, ou bloqueio se o contrato reivindicado for universal.

3. **FILA deploy × deploy: o fato está confirmado; a autorização para fechá-lo como fila não.** A admissão fecha somente dentro do reinício (`bin/reinicio_sob_admissao.sh:40-45`) e abre com a saída desse script (`:73`); a prova posterior usa exec sem marca (`bin/deploy.sh:300-305`). A chamada automática está antes da pausa do vigia (`bin/vigia_arvore.sh:24-29`), cujo run é excluído do universo inteiro pela declaração/classificação (`bin/admissao_cron.sh:34`, `:229-231`). Portanto outro restart pode alcançar esses execs. O aval fornecido proíbe reiniciar sobre exec vivo e condiciona PRONTO a provas; citar como **anexo AVAL_DEPLOY-ELO-B3.txt [nao verificado]** se não houver arquivo local. **Gravidade: bloqueia**, apesar de preexistir parcialmente; mudar o mecanismo necessário não prova que o risco esteja fora da condição escrita.

4. **Demais motivos de FILA que consegui conferir.**
   
   - `_pauta` já expõe stderr e devolve rc (`app/core/esteira_vigia.py:900-905`), mas vários chamadores descartam retorno (`:959`, `:1179`, `:1241`, `:1244`, `:1274`, `:1291`, `:1393`, `:1400`, `:1632`, `:1637`). Motivo de escopo dos três chamadores é coerente com o texto do aval fornecido **[nao verificado]**. **Fila**.
   - Revert do revert realmente usa outra pauta (`auto_revert`) e não informa ID no subprocess (`app/core/esteira_vigia.py:1289-1294`); o padrão do deploy é `manual` se não houver ID no ambiente (`bin/deploy.sh:52`). Pauta já viva não é reescrita (`app/core/management/commands/alarme_esteira.py:38-41`). Assim a alegação “rc chega à pauta” não vale sempre. **Fila**, mas considerar não comprovada a cobertura operacional.
   - Falta de teste da criação do marcador é real (`bin/tests/test_deploy_nao_mata_elo.sh:759` versus `bin/deploy.sh:282`). **Fila de mordida**.
   - Revert do revert só recebe segundo deploy75 no teste (`bin/tests/test_deploy_nao_mata_elo.sh:828-835`). **Fila de mordida**.
   - Mutante que remove a limpeza de `_rec_ant` não é morto pelo cenário persistente, que cobra existência, não quantidade (`bin/tests/test_deploy_nao_mata_elo.sh:208`); cenário rc1 cobra quantidade1, mas não percorre a substituição bem-sucedida (`:218-225`). **Fila de mordida**.
   - Remoção de `_rec_ant` antecede a validação do conteúdo do ID novo (`bin/deploy_agendado.sh:110`, `:115`); produtor escreve ID do ambiente (`bin/deploy.sh:286`). Premissa operacional de produtor íntegro sustenta fila, mas não torna a sequência atômica. **Fila**.
   - Reload inicial do integrador ignora rc (`app/core/integrador_lote.py:638`); rollback dos dois moldes também (`bin/molde_fatia/cadeia.sh:32`, `bin/molde_autocura/cadeia.sh:31`). Classificação como preexistentes depende do diff fornecido **[nao verificado]**. **Fila**.
   - `ADM_ESPERA_ADMISSAO_S` realmente não tem limite superior (`bin/admissao_cron.sh:50`, `:243`), enquanto teto de restart só pode baixar (`:62-68`). Correção da justificativa procede. **Fila**, sem confundir os dois tetos.
   - CLI imprime resultado e retorna0 (`app/core/esteira_vigia.py:1652-1654`); vigia passa por `tail` sem decidir (`bin/vigia_arvore.sh:28`). Motivo de fila procede para esse uso. **Fila**.
   - Campos argv vazios realmente são descartados (`app/core/esteira_vigia.py:1352`); argv produzido pelo auto-revert nessa chamada não contém campo vazio (`:1276`). Motivo específico procede. **Fila**.
   - Retentar não refaz conferência/marcação: rc0 somente relata e retorna (`app/core/esteira_vigia.py:1359-1362`), enquanto auto-revert original confere e marca (`:1283`, `:1299-1303`). **Fila**, sem apresentar a tentativa como prova de cura.
   - Corte por mtime ocorre antes da espera (`bin/deploy.sh:282-283`), conservando arquivos posteriores (`:382`). No caso declarado isso superconta; motivo de lado conservador procede. **Fila**.

5. **Classes históricas relevantes no delta3.**
   
   - **(b)/(f)/(h)/(j):** transferência escreve marca fora de `adm_marcar`/fd8 e recua à marca do wrapper se falha (`bin/em_casca.sh:32-38`), enquanto a porta canônica fecha/abre fd8 (`bin/admissao_cron.sh:242-250`); os detalhes do risco devem vir da análise do agente de marcas.
   - **(d):** a lista paralela `DEPLOY_RECUSA_IDS` não aparece no trecho atual; fecho deriva ID do arquivo (`bin/deploy.sh:369-371`; `app/core/esteira_vigia.py:1311-1315`). Remoção no delta: anexo **[nao verificado]**.
   - **(e):** lacunas acima; não confundir testes que mordem alguns mutantes com prova de todos os caminhos (`bin/tests/test_deploy_nao_mata_elo.sh:670-676`, `:759`, `:789-792`, `:930-934`).
   - **(a)/(c)/(g)/(i):** não identifiquei instância nova nos trechos examinados; conclusão global sobre delta3 **[nao verificado]**.
   - Conferência da hipótese de rede: o harness chama HTTP dentro de `_TIRO` (`eval/harness.py:14-24`), executado sob em_casca (`:53-56`); não é uma chamada HTTP direta do run no host nessa implementação. Linha correspondente no RELATO exigida pelo aval: **[nao verificado]**.

## 04/11 11:27 · sessao 953adbce7ef6 · - · - · origem=Ronald (tmux)

Achados principais, por leitura estática:

- **Bloqueia — cura da pauta incompleta e regressão da recuperação anterior.** Os três chamadores abrem `deploy_recusado:<id>` para qualquer rc diferente de zero: `app/core/integrador_lote.py:773-780`, `app/core/esteira_vigia.py:1326-1333`, `bin/vigia_logins.sh:91-96`. A recusa só nasce quando o restart devolve 75 (`bin/deploy.sh:284-290`). Logo, rc 1 antes/depois do restart pode abrir pauta sem arquivo; `_tira_recusa` retorna imediatamente sem arquivo (`bin/deploy.sh:368`), e o antigo fechamento dos IDs sem recusa foi removido, restando apenas relato após sucesso (`app/core/esteira_vigia.py:1359-1362`). Esses três IDs podem, portanto, ter pauta aberta sem caminho automático que a feche. A ocorrência em prod é [nao verificado].

- **Bloqueia — corrida abertura versus fechamento continua.** Após o deploy recusado retornar, o chamador ainda vai abrir a pauta (`app/core/esteira_vigia.py:1325-1331`; `app/core/integrador_lote.py:771-780`; `bin/vigia_logins.sh:90-95`). Outro deploy pode primeiro resolver uma pauta inexistente — operação que retorna normalmente (`app/core/management/commands/alarme_esteira.py:28-33`) — e apagar a recusa (`bin/deploy.sh:370-375`). O primeiro chamador então abre pauta depois do apagamento; não há arquivo que permita um fechamento seguinte (`bin/deploy.sh:368`). É interleaving permitido pelo código; ocorrência em prod [nao verificado].

- **Fila — erro de leitura vira apagamento.** `_tira_recusa` não verifica o resultado de `head|tr|sed` (`bin/deploy.sh:369`); ID vazio pula o resolver e chega ao `rm` (`bin/deploy.sh:370-375`). Arquivo ilegível/malformado pode perder a testemunha sem fechar pauta. O produtor normal escreve via `.novo` e `mv` (`bin/deploy.sh:286-287`), portanto corrupção real [nao verificado].

- **Fila — corrida no mesmo ID também alcança a nova limpeza.** `_tira_recusa` lê ID, executa um comando externo e depois remove o pathname sem confirmar que é o mesmo arquivo (`bin/deploy.sh:369-375`). Uma recusa nova do mesmo ID escrita nesse intervalo (`bin/deploy.sh:286-287`) pode ser apagada. Não há trava envolvendo essa sequência (`bin/deploy.sh:366-383`).

Mapa de recusas e pautas:

| Estado | Escreve/cria | Lê | Apaga/fecha |
|---|---|---|---|
| `recusa_cron.<id>.pendente` | `bin/deploy.sh:286-287`, exclusivamente no ramo restart rc 75 (`:284`) | agendado valida primeiro campo (`bin/deploy_agendado.sh:83-87`), lê argv (`:101`); auto-revert lê arquivo fixo (`app/core/esteira_vigia.py:1344-1354`); contador conta filenames (`bin/deploy_recusado.sh:22-27`); limpeza lê ID/mtime (`bin/deploy.sh:369`, `:382`) | deploy via `_tira_recusa` (`bin/deploy.sh:375`, `:377-382`); **também agendado**, antes de iniciar IDs próprios (`bin/deploy_agendado.sh:95`, `:104`) e substituindo recusa anterior (`:110`) |
| `deploy_recusado:integrador` | `app/core/integrador_lote.py:780` → `_pauta` em `app/core/esteira_vigia.py:900-905` | `pauta_viva`, `app/core/management/commands/alarme_esteira.py:13-15` | comando resolver disparado por `_tira_recusa`, `bin/deploy.sh:370-375` |
| `deploy_recusado:auto_revert` | `app/core/esteira_vigia.py:1331`, `:1356` | mesmo leitor acima | mesmo fechamento acima |
| `deploy_recusado:vigia_logins` | `bin/vigia_logins.sh:94-95` | mesmo leitor acima | mesmo fechamento acima |

O comando não reescreve pauta viva (`app/core/management/commands/alarme_esteira.py:38-41`), cria por `pautas.services.escrever` (`:54-56`) e fecha pela porta `marcar_feita_pelo_sistema` (`:27-31`; escrita efetiva em `app/pautas/services.py:269-275`). IDs `manual`/`agendado.*` podem gerar recusas (`bin/deploy.sh:52-54`; `bin/deploy_agendado.sh:94-105`), mas nenhum dos três abridores usa essas chaves (`app/core/integrador_lote.py:780`; `app/core/esteira_vigia.py:1331`, `:1356`; `bin/vigia_logins.sh:95`). A chave antiga sem sufixo não tem abridor/fechador automático específico no código atual; existência em prod [nao verificado].

RCs dos chamadores encontrados:

| Chamador | Tratamento |
|---|---|
| integrador, reload inicial | Resultado descartado; segue para suite (`app/core/integrador_lote.py:638-639`). |
| integrador, publicação final | Lê rc (`app/core/integrador_lote.py:771-782`); impede `FIM` em falha (`:748-752`). |
| auto-revert, primeiro deploy | Lê rc e para antes dos testes (`app/core/esteira_vigia.py:1276-1284`); pauta/relato em falha (`:1325-1334`). |
| auto-revert, desfazer revert | Lê rc e põe no texto da pauta `auto_revert` (`app/core/esteira_vigia.py:1289-1294`); pauta já viva pode engolir texto (`app/core/management/commands/alarme_esteira.py:38-41`). Não fornece explicitamente `DEPLOY_RECUSA_ID`, herdando ambiente/default `manual` (`app/core/esteira_vigia.py:1289-1290`; `bin/deploy.sh:52`). |
| retentar | Lê rc (`app/core/esteira_vigia.py:1354-1362`); CLI sempre retorna 0 (`:1652-1654`); `vigia_arvore` usa pipeline `tail -1` (`bin/vigia_arvore.sh:28`). |
| vigia_logins | Lê rc e abre pauta em falha (`bin/vigia_logins.sh:90-96`); seu próprio rc final é 2 (`:105`). |
| deploy_agendado | Primeira esteira rc capturado (`bin/deploy_agendado.sh:96-99`); repetição rc capturado/validado (`:105-120`); sucesso repetido vira 3 por cauda não executada (`:123-124`); retorna `PIOR` (`:126`, `:135`). |
| molde_fatia | Reload inicial guardado (`bin/molde_fatia/cadeia.sh:48`); rollback roda com `set +e` e ignora rc (`:26`, `:32-33`); deploy final sujeito a `set -e` (`:4`, `:89`), depois exige texto OK (`:90`). |
| molde_autocura | Equivalente: `bin/molde_autocura/cadeia.sh:3`, `:25`, `:31-32`, `:45`, `:86-87`. |
| fatia `o135-teto20` | `--conferir` guardado (`fatias_agendadas/o135-teto20/esteira.sh:80`); publicação captura e propaga rc (`:116-124`). |
| fatia `abono-no-ar` | Pipeline protegido por `pipefail` e condicional, traduz falha em 1 (`fatias_agendadas/abono-no-ar/esteira.sh:21`, `:37-38`). |
| reload diário RAW | Declarado em `app/config/crons.py:307-308`; execução genérica pelo cron wrapper registra/trata rc e sai com ele (`bin/cron_run.sh:64-70`). |

**RC do resolver:** qualquer não zero do `em_casca`/comando entra no `if !`, gera aviso, preserva arquivo e devolve 1 (`bin/deploy.sh:370-373`). Esse 1 **não é propagado como rc do deploy**: chamadas em `:377`/`:381` não condicionam o resultado final, e o último `echo` devolve 0 (`:385`). Assim `retentar_deploy` pode anunciar “no ar” e deixar recusa/pauta pendentes (`app/core/esteira_vigia.py:1354-1362`; `bin/deploy.sh:370-385`). O restart pode de fato ter passado; o rc 0 não prova que a limpeza fechou.

O segundo apagador `deploy_agendado` já pertence ao diff total fornecido, não ao delta3; sua existência contradiz o comentário “ninguém mais apaga” (`bin/deploy_recusado.sh:6-8` versus `bin/deploy_agendado.sh:110`). Não identifiquei novo escritor direto de `feito_em` na volta 3: fechamento continua pela porta `app/pautas/services.py:256-275`.

## 04/11 11:27 · sessao 9fcf87ff9ab8 · - · - · origem=Ronald (tmux)

**BLOQUEIA — transferência sem fd8 permite restart com exec vivo.** `adm_marcar` solta fd8 antes do comando (`bin/admissao_cron.sh:250`); a transferência publica outra marca e remove a anterior sem retomar a trava (`bin/em_casca.sh:32-36`). A varredura expande o glob uma vez (`bin/admissao_cron.sh:202`): se ele contém apenas a velha e a transferência ocorre antes do teste `-e`, a velha é pulada (`bin/admissao_cron.sh:203`) e a nova não pertence àquela enumeração. Isso também vale para a varredura final sob fd8 (`bin/reinicio_sob_admissao.sh:43-45`), permitindo alcançar o restart com cliente docker vivo (`bin/reinicio_sob_admissao.sh:66`). O comentário “nunca vê zero” não se sustenta (`bin/em_casca.sh:31`).

**Caminhos do item 2:**

- **Filho acaba antes da leitura do starttime:** se `/proc` já desapareceu, `adm_starttime` falha (`bin/admissao_cron.sh:84-91`); não há transferência, `wait` recolhe o rc e EXIT remove a marca caso aquele PID não exista (`bin/em_casca.sh:23`, `bin/em_casca.sh:32`, `bin/em_casca.sh:41-42`). O caminho normal devolve o rc do comando; não há exec vivo nesse intervalo.
- **Falha de sed/mv:** mantém somente a marca do wrapper (`bin/em_casca.sh:34-38`). Enquanto o wrapper vive, ela segura; se ele morrer antes do filho, o reaper a considera órfã pelo PID do wrapper e a apaga (`bin/admissao_cron.sh:169-175`, `bin/admissao_cron.sh:208`), embora o cliente ainda possa viver. **Bloqueia**, pois a cura de SIGKILL não cobre o caminho de falha.
- **SIGTERM no wrapper:** não há trap TERM, apenas EXIT (`bin/em_casca.sh:23`). Depois da transferência, EXIT preserva a marca enquanto `kill -0` do filho responde; antes dela ou após falha de transferência, preserva arquivo cujo PID ainda é do wrapper (`bin/em_casca.sh:23`, `bin/em_casca.sh:34-38`), e o reaper o remove após a morte. O status observado pelo chamador é o de término do wrapper, não o resultado futuro do comando; ocorrência de produção **[nao verificado]**.
- **SIGKILL no wrapper:** depois da transferência concluída, a marca identifica o filho e continua válida por PID/starttime/exe (`bin/em_casca.sh:33-36`, `bin/admissao_cron.sh:169-177`). Antes da transferência, ou no fallback sed/mv, a marca morre junto com o wrapper para o leitor, embora o filho tenha sido iniciado (`bin/em_casca.sh:28`, `bin/em_casca.sh:32-38`). **Bloqueia**.
- **PID do filho reutilizado:** o EXIT usa somente `kill -0`, sem comparar starttime (`bin/em_casca.sh:23`); pode deixar arquivo após o comando acabar. O reaper distingue reutilização comparando starttime (`bin/admissao_cron.sh:175`). Se a reutilização ocorrer *antes* da captura de starttime, pode registrar a identidade do processo substituto; `exe` é apenas substring da cmdline, e não `/proc/PID/exe` (`bin/em_casca.sh:32-34`, `bin/admissao_cron.sh:176-177`). **Fila**, para retenção espúria; ocorrência concreta **[nao verificado]**.
- **rc64 de uso/nome:** sai antes da admissão (`bin/em_casca.sh:12-13`); alvo fora das cascas sai depois de marcar, mas antes de iniciar o comando, e EXIT desmarca porque `_c` está vazio (`bin/em_casca.sh:21-28`). Sem execução escondida nesse caminho.
- **Admissão 75/70:** não chega ao lançamento do comando (`bin/em_casca.sh:16-19`, `bin/em_casca.sh:28`). O 75 nasce da espera pela trava e o 70 da criação/abertura/starttime/publicação (`bin/admissao_cron.sh:241-248`).
- **Marca nova literalmente depois de uma verificação vazia:** é uma interleaving possível se o wrapper for morto após lançar o `mv`, mas antes de o filho `mv` publicar: o reaper pode remover a marca velha pelo wrapper morto, enquanto `mv` ainda publica sem fd8 (`bin/em_casca.sh:35`, `bin/admissao_cron.sh:170`, `bin/admissao_cron.sh:208`). Dedução de concorrência; reprodução e ocorrência de produção **[nao verificado]**. A corrida do glob acima já prova o defeito sem sinais.

**Quem escreve/lê as marcas:**

- Porta original: `adm_marcar` escreve `${nome}_${emp}.$$`, com `ini/pid/starttime/exe/casca/cmd`, por temporário e `mv` sob fd8 (`bin/admissao_cron.sh:239-250`).
- Chamadores da porta: `cron_run.sh` para comando e alarme (`bin/cron_run.sh:16`, `bin/cron_run.sh:52`); descoberta da corrente (`bin/encadeado.sh:48`); `em_casca` (`bin/em_casca.sh:16`).
- **Segundo escritor novo:** `em_casca` escreve `${N}_-.${filho}`, altera pid/starttime/exe, conserva os demais campos e remove a marca anterior (`bin/em_casca.sh:33-36`).
- Leitor de identidade: `adm_marca_viva` toma PID **do nome do arquivo**, não do campo `pid=`, e lê starttime/exe (`bin/admissao_cron.sh:169-177`).
- Leitor de proteção: `adm_segura` lê `casca`; formato antigo também segura; casca=0 ainda consulta a árvore viva (`bin/admissao_cron.sh:181-186`).
- Leitor/reaper: `adm_varrer` aplica identidade, exclui ancestrais da proteção, chama `adm_segura` e apaga órfãs (`bin/admissao_cron.sh:199-209`).
- Consumidor efetivo: restart consulta `adm_seguram` antes e depois de tomar fd8 (`bin/reinicio_sob_admissao.sh:40-45`); `adm_seguram` filtra `adm_varrer` (`bin/admissao_cron.sh:213`).
- Remoção explícita: `adm_desmarcar` (`bin/admissao_cron.sh:254`), usada por EXIT/comando e alarme do cron (`bin/cron_run.sh:22`, `bin/cron_run.sh:51`, `bin/cron_run.sh:58`), descoberta da corrente (`bin/encadeado.sh:50`) e EXIT do `em_casca` (`bin/em_casca.sh:23`).
- Contrato apenas verifica presença textual da infraestrutura; não lê marcas de execução (`bin/contrato_deploy.sh:55-57`). Selos usam raízes de teste e leem `adm_varrer` (`bin/tests/test_deploy_nao_mata_elo.sh:635`, `bin/tests/test_deploy_nao_mata_elo.sh:675`).

A cura da volta 3 cobre o cenário **já transferido antes do SIGKILL**, mas introduz o segundo escritor sem fd8 e deixa janelas anteriores/fallback abertas (`bin/em_casca.sh:28-38`).

## 04/11 11:08 · sessao 74fc7acb1b74 · DEPLOY-[nome]-ELO · verificador · origem=Code

1. **Os PROCEDENTES tiveram cura parcial. Eu não daria fechamento: permanecem caminhos sem garantia contra restart sobre exec vivo, proibido pelo aval.** A serialização cobre o restart, enquanto outros execs do deploy ficam fora dela; o wrapper também pode perder sua marca após timeout (`bin/reinicio_sob_admissao.sh:43`, `bin/deploy.sh:124`, `bin/em_casca.sh:21`, `bin/admissao_cron.sh:208`; `AVAL_DEPLOY-ELO-B3.txt:13`, anexo).

   Referências aos anexos abaixo usam a numeração das linhas do texto fornecido. Suíte executada nesta cópia: **[não verificado]**. Não executei o selo porque ele cria e remove arquivos (`bin/tests/test_deploy_nao_mata_elo.sh:176`, `bin/tests/test_deploy_nao_mata_elo.sh:193`).

   | PROCEDENTE da triagem | Verificação em 4f7035a3 |
   |---|---|
   | Recusa apagada antes da nova tentativa | **Curado no cenário apontado:** `_rec_ant` preserva a anterior quando a tentativa dá rc1 sem arquivo novo; o caso 8 exige essa preservação. A autorização para substituir por recusa nova verifica apenas existência, ressalva de **fila** detalhada no item 6 (`bin/deploy_agendado.sh:102`, `bin/deploy_agendado.sh:110`, `bin/tests/test_deploy_nao_mata_elo.sh:224`). |
   | `_pauta` ignorava rc | **Parcial:** agora devolve rc e escreve diagnóstico; três chamadores consomem esse retorno. Os demais, inclusive a resolução e a pauta do revert do revert, continuam descartando-o. **Fila** para os consumidores restantes (`app/core/esteira_vigia.py:902`, `app/core/esteira_vigia.py:905`, `app/core/esteira_vigia.py:1291`, `app/core/esteira_vigia.py:1364`; censo no item 3). |
   | Revert do revert ignorava rc do deploy | **Leitura curada; comunicação parcial:** `rrd` entra no texto da pauta, mas seu envio pode falhar ou encontrar pauta já viva; o relato e o retorno não carregam o rc. **Fila** (`app/core/esteira_vigia.py:1289`, `app/core/esteira_vigia.py:1293`, `app/core/esteira_vigia.py:1295`, `app/core/esteira_vigia.py:1298`, `app/core/management/commands/alarme_esteira.py:38`). |
   | Chave única engolia pauta de outro chamador | **Curado entre os três novos motivos**, mas o fechamento continua condicionado ao sucesso de um retry do auto-revert. Há também **regressão de compatibilidade, fila:** a chave antiga perde seu resolvedor automático (`app/core/integrador_lote.py:780`, `bin/vigia_logins.sh:95`, `app/core/esteira_vigia.py:1332`, `app/core/esteira_vigia.py:1349`, `app/core/esteira_vigia.py:1364`). |
   | 19: `exit 0` sobrevivia | **Curado para esse mutante:** agora exige rc3 do comando e desmarcação (`bin/tests/test_deploy_nao_mata_elo.sh:660`, `bin/tests/test_deploy_nao_mata_elo.sh:661`). |
   | 21: limpeza provada só por grep | **Curado quanto ao bloco de limpeza:** ele é executado. Ainda não prova a criação real do marcador; lacuna de **fila** (`bin/tests/test_deploy_nao_mata_elo.sh:734`, `bin/tests/test_deploy_nao_mata_elo.sh:738`, `bin/tests/test_deploy_nao_mata_elo.sh:740`). |
   | 23: CLI não exercitada | **Parcial:** passou de grep para AST, mas continua sem executar a CLI e aceita comparação invertida. **Fila de mordida** (`bin/tests/test_deploy_nao_mata_elo.sh:904`, `bin/tests/test_deploy_nao_mata_elo.sh:905`; `app/core/esteira_vigia.py:1657`). |
   | 20: cron sem duração excluído calado | **Curado para a fonte viva considerada pelo selo:** informa ausentes e exige zero nesse caminho; lacunas da cópia são apenas informadas. O dado fornecido declara 13 lacunas na cópia e nenhuma no vivo; não houve nova medição de produção (`bin/tests/test_deploy_nao_mata_elo.sh:699`, `bin/tests/test_deploy_nao_mata_elo.sh:707`; `triagem_v1.txt:11`, anexo). |

2. **FILA/DESCARTADO: duas justificativas não bastam para fechamento diante do PROIBIDO escrito; outra contém afirmação factual incorreta.** O aval não escreve exceção para DEPLOY × DEPLOY ou morte do wrapper (`AVAL_DEPLOY-ELO-B3.txt:13`, anexo). [nao verificado]

   | Item triado | O motivo sustenta-se? Gravidade |
   |---|---|
   | Execs internos do deploy: “DEPLOY × DEPLOY, fora do aval” | O diagnóstico técnico sustenta-se; a dispensa, **não**. Migrate, collectstatic e prova de rota executam sem marca própria, enquanto o mutex cobre somente o restart. Dois deploys podem sobrepor esses trechos. **Bloqueia pelo PROIBIDO**, embora não seja introdução do delta da volta 2; ocorrência em produção **[não verificado]** (`bin/deploy.sh:124`, `bin/deploy.sh:205`, `bin/deploy.sh:316`, `bin/reinicio_sob_admissao.sh:43`, `bin/reinicio_sob_admissao.sh:73`). |
   | Timeout/SIGKILL deixa docker sem marca | “SIGKILL não se trapa” explica a limitação, mas não a cura. Há timeout sobre o wrapper; ele só trata EXIT, identifica sua própria vida e roda docker como filho. Morto o wrapper, o reaper pode apagar sua marca sem comprovar término do exec. **Bloqueia a garantia exigida por MUDA 1/PROIBIDO**; sobrevivência concreta em produção **[não verificado]** (`eval/harness.py:56`, `eval/juiz.py:245`, `bin/em_casca.sh:21`, `bin/em_casca.sh:26`, `bin/admissao_cron.sh:175`, `bin/admissao_cron.sh:208`; `AVAL_DEPLOY-ELO-B3.txt:6`, `:13`, anexo). |
   | Marca casca=0 antes do rc64 | Sustenta-se: a validação vem após marcar, mas antes de executar; EXIT desmarca. **Fila**, sem execução indevida demonstrada (`bin/em_casca.sh:16`, `bin/em_casca.sh:21`, `bin/em_casca.sh:22`, `bin/em_casca.sh:26`). |
   | Marca sobra após morte sem EXIT | **DESCARTA sustentado quanto à limpeza da marca:** PID/starttime/cmdline são conferidos e órfãs são removidas. Isso não resolve o filho órfão do item anterior (`bin/admissao_cron.sh:167`, `bin/admissao_cron.sh:175`, `bin/admissao_cron.sh:177`, `bin/admissao_cron.sh:208`). |
   | Reload inicial do integrador ignora rc | Sustenta-se: não guarda retorno e segue à suíte. **Fila**; não é o deploy final do lote tratado por MUDA 3 (`app/core/integrador_lote.py:638`, `app/core/integrador_lote.py:639`, `app/core/integrador_lote.py:748`). |
   | Rollback dos moldes ignora rc | Sustenta-se: executa reload e anuncia “recarregada” sem conferir sucesso. **Fila**, fora do delta fornecido (`bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:33`, `bin/molde_autocura/cadeia.sh:31`, `bin/molde_autocura/cadeia.sh:32`). |
   | fd7 liberado cedo | **DESCARTA como defeito atual sustentado:** o pai conserva fd7; `7>&-` fecha nos filhos. **Fila** para ampliar mordida do teste (`bin/vigia_logins.sh:70`, `bin/vigia_logins.sh:71`, `bin/vigia_logins.sh:90`, `bin/vigia_logins.sh:95`). |
   | Corte por mtime nasce antes da espera | Sustenta-se para o cenário descrito: recusa produzida durante a espera pode sobreviver ao restart que a superou. É **supercontagem, fila** (`bin/deploy.sh:282`, `bin/deploy.sh:283`, `bin/deploy.sh:367`). |
   | Mesmo id concorrente disputa `.novo` | Sustenta-se: temporário e destino derivam só do id. As travas dos chamadores não constituem mutex comum de deploy inteiro. **Fila**, ligada ao primeiro bloqueio (`bin/deploy.sh:54`, `bin/deploy.sh:286`, `bin/integrador.sh:9`, `bin/vigia_arvore.sh:23`, `bin/vigia_logins.sh:70`). |
   | CLI retorna 0 / `tail` perde rc | Sustenta-se: a CLI imprime o texto e retorna 0; o vigia não decide pelo rc. **Fila** para esse contrato, sem provar sucesso da resolução (`app/core/esteira_vigia.py:1658`, `app/core/esteira_vigia.py:1659`, `bin/vigia_arvore.sh:28`). |
   | “Cascas não reiniciaram” para qualquer rc1 | A objeção procede: rc1 também pode acontecer depois do restart, na prova de rota ou selo. **Fila de comunicação**, pois o estado afirmado pode estar errado (`app/core/esteira_vigia.py:1329`, `bin/deploy.sh:283`, `bin/deploy.sh:337`, `bin/deploy.sh:344`). |
   | Retry não refaz conferência/marcação do auto-revert | Sustenta-se. O sucesso resolve pautas e relata; não volta aos testes nem à marcação das fatias. **Fila**: MUDA 5 exige retry automático, mas não explicita essa continuação (`app/core/esteira_vigia.py:1283`, `app/core/esteira_vigia.py:1303`, `app/core/esteira_vigia.py:1362`, `app/core/esteira_vigia.py:1367`; `AVAL_DEPLOY-ELO-B3.txt:10`, anexo). |
   | argv perde campo vazio | Sustenta-se; o produtor atual passa argumentos não vazios. **Fila** (`app/core/esteira_vigia.py:1353`, `app/core/esteira_vigia.py:1276`). |
   | Cabeçalho 3300 → 1586 | Correção conferida contra o JSON da cópia (`bin/admissao_cron.sh:31`, `app/config/crons_duracao.json:82`). |
   | Env de admissão “só encurta” | **Motivo parcialmente falso:** o valor é aceito sem limite superior e passado ao `flock`; também pode aumentar a espera. É verdadeiro que não altera o teto do restart. **Fila**, preexistente no delta (`bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:243`, `bin/admissao_cron.sh:58`, `bin/reinicio_sob_admissao.sh:18`). |

   **CONFERIR:** no harness, o HTTP está dentro de `_TIRO`, executado pelo docker exec protegido; não é HTTP direto do host nessa função. O código atende à marca nesse trecho. Execução da conferência e eventual linha no RELATO: **[não verificado]** (`eval/harness.py:14`, `eval/harness.py:18`, `eval/harness.py:24`, `eval/harness.py:54`; `AVAL_DEPLOY-ELO-B3.txt:11`, anexo).

3. **Censo de escritores e leitores.**

   **Motivos e tupla:**

   | Elemento | Quem escreve | Quem lê |
   |---|---|---|
   | `deploy_recusado:auto_revert` | Abertura em `_deploy_do_revert` e na falha do retry (`app/core/esteira_vigia.py:1332`, `app/core/esteira_vigia.py:1357`). | Resolução pelo loop dos ids (`app/core/esteira_vigia.py:1364`). |
   | `deploy_recusado:integrador` | `_deploy_do_lote` (`app/core/integrador_lote.py:780`). | Mesmo loop (`app/core/esteira_vigia.py:1364`). |
   | `deploy_recusado:vigia_logins` | Shell do vigia (`bin/vigia_logins.sh:95`). | Mesmo loop (`app/core/esteira_vigia.py:1364`). |
   | `DEPLOY_RECUSA_IDS` | Uma definição literal (`app/core/esteira_vigia.py:1316`). | Um consumidor: universo do loop de resolução (`app/core/esteira_vigia.py:1362`). |
   | Estado efetivo da pauta | O comando delega abertura/fechamento aos serviços (`app/core/management/commands/alarme_esteira.py:27`, `app/core/management/commands/alarme_esteira.py:31`, `app/core/management/commands/alarme_esteira.py:54`). | A localização da pauta viva usa prefixo do texto; pauta já viva impede reescrever o conteúdo (`app/core/management/commands/alarme_esteira.py:15`, `app/core/management/commands/alarme_esteira.py:38`). |

   **Chave antiga — regressão, fila:** não resta produtor ou resolvedor automático específico de `deploy_recusado` sem sufixo. A porta genérica ainda aceita essa chave, mas os novos resolvedores procuram `ESTEIRA deploy_recusado:<id>:` e não encontram o texto antigo `ESTEIRA deploy_recusado: ...`. As ações genéricas atuais também não produzem essa resolução (`app/core/management/commands/alarme_esteira.py:15`, `app/core/management/commands/alarme_esteira.py:24`, `app/core/esteira_vigia.py:1364`, `app/core/esteira_vigia.py:490`, `app/core/esteira_vigia.py:504`, `app/core/esteira_vigia.py:530`, `app/core/esteira_vigia.py:597`). Pauta antiga viva em produção: **[não verificado]**.

   **Rc de `_pauta`:** nasce do subprocess, é diagnosticado centralmente e devolvido (`app/core/esteira_vigia.py:900`, `app/core/esteira_vigia.py:902`, `app/core/esteira_vigia.py:905`).

   | Chamador | Retorno lido? |
   |---|---|
   | `_deploy_do_lote` | **Sim:** acrescenta falha ao relato (`app/core/integrador_lote.py:780`, `app/core/integrador_lote.py:781`). |
   | `_deploy_do_revert` | **Sim:** acrescenta falha ao relato (`app/core/esteira_vigia.py:1332`, `app/core/esteira_vigia.py:1334`). |
   | `retentar_deploy`, abertura | **Sim:** acrescenta falha à string devolvida, impressa pela CLI (`app/core/esteira_vigia.py:1357`, `app/core/esteira_vigia.py:1359`, `app/core/esteira_vigia.py:1658`). |
   | `_relance_nao_pegou` | **Não** (`app/core/esteira_vigia.py:959`). |
   | `curar_zumbido` | **Não** (`app/core/esteira_vigia.py:1179`). |
   | `conferir_quarentena`, prazo e depósito | **Não**, nos dois pontos (`app/core/esteira_vigia.py:1241`, `app/core/esteira_vigia.py:1244`). |
   | `auto_revert`, revert que não aplicou e revert desfeito | **Não**, nos dois pontos (`app/core/esteira_vigia.py:1274`, `app/core/esteira_vigia.py:1291`). |
   | `retentar_deploy`, resolução | **Não** (`app/core/esteira_vigia.py:1364`). |
   | `curar_por_relogio`, quarentena e vermelho | **Não**, nos dois pontos (`app/core/esteira_vigia.py:1398`, `app/core/esteira_vigia.py:1405`). |
   | `passada`, ações genéricas de alarme e resolução | **Não**, nos dois pontos (`app/core/esteira_vigia.py:1637`, `app/core/esteira_vigia.py:1642`). |

   **Pendentes `recusa_cron.<id>.pendente`:**

   | Papel | Sítios |
   |---|---|
   | Criar/sobrescrever em produção | `deploy.sh` escreve `.novo` e faz `mv`, exclusivamente no ramo rc75 (`bin/deploy.sh:286`, `bin/deploy.sh:287`, `bin/deploy.sh:290`). |
   | Apagar em produção | Próprio id no fim do deploy; recusas anteriores pelo corte; nomes de tentativas do agendado antes de iniciar; recusa anterior substituída pelo agendado (`bin/deploy.sh:363`, `bin/deploy.sh:367`, `bin/deploy_agendado.sh:95`, `bin/deploy_agendado.sh:104`, `bin/deploy_agendado.sh:110`). |
   | `_rec_ant` | Recebe o caminho anterior; seu único consumidor é o `rm` condicionado a rc0 ou rc75 com arquivo novo (`bin/deploy_agendado.sh:102`, `bin/deploy_agendado.sh:110`). |
   | Ler conteúdo | Agendado valida id e recupera argv; retry lê somente o arquivo do auto-revert (`bin/deploy_agendado.sh:83`, `bin/deploy_agendado.sh:101`, `app/core/esteira_vigia.py:1345`, `app/core/esteira_vigia.py:1348`). |
   | Ler existência/nomes | Loop de resolução e contador; placar consome o contador e soma ao TOTAL (`app/core/esteira_vigia.py:1363`, `bin/deploy_recusado.sh:22`, `bin/deploy_recusado.sh:25`, `bin/placar_code.sh:464`, `bin/placar_code.sh:542`). |
   | Criar/apagar em testes | Fixtures dos casos 8, 21 e 23, além dos blocos extraídos de produção (`bin/tests/test_deploy_nao_mata_elo.sh:176`, `bin/tests/test_deploy_nao_mata_elo.sh:189`, `bin/tests/test_deploy_nao_mata_elo.sh:238`, `bin/tests/test_deploy_nao_mata_elo.sh:716`, `bin/tests/test_deploy_nao_mata_elo.sh:736`, `bin/tests/test_deploy_nao_mata_elo.sh:858`, `bin/tests/test_deploy_nao_mata_elo.sh:870`, `bin/tests/test_deploy_nao_mata_elo.sh:879`). |

   **Rc do deploy do revert do revert:** o único consumidor de `rrd` é a composição do texto enviado à pauta; não controla retry, relato ou retorno. Esse segundo deploy não fixa o id `auto_revert`: herda o ambiente ou usa `manual`, cujo pendente não é lido pelo retentador automático. **Fila** (`app/core/esteira_vigia.py:1289`, `app/core/esteira_vigia.py:1293`, `app/core/esteira_vigia.py:1295`, `app/core/esteira_vigia.py:1298`, `bin/deploy.sh:52`, `app/core/esteira_vigia.py:1345`).

4. **Os casos ganharam mordida, mas não provam toda a cura anunciada.**

   | Caso | Mordida e mutação sobrevivente |
   |---|---|
   | 8, rc1 | Mata a exclusão incondicional da recusa anterior. Entretanto, remover a exclusão de `_rec_ant` inteiramente deixa acumular recusas substituídas: a persistência exige existência, sem exigir quantidade. **Fila de cobertura** (`bin/deploy_agendado.sh:110`, `bin/tests/test_deploy_nao_mata_elo.sh:207`, `bin/tests/test_deploy_nao_mata_elo.sh:224`). |
   | 19(e) | Mata `exit 0`: exige rc3 e nenhuma marca restante. **Cura específica sustentada** (`bin/em_casca.sh:27`, `bin/tests/test_deploy_nao_mata_elo.sh:660`, `bin/tests/test_deploy_nao_mata_elo.sh:661`). |
   | 21, limpeza | Mata alterações do bloco que preservem a velha, apaguem a nova ou deixem marcador. **Sobrevive apagar a criação real do marcador:** o próprio teste o fabrica. **Fila** (`bin/deploy.sh:282`, `bin/deploy.sh:366`, `bin/tests/test_deploy_nao_mata_elo.sh:738`, `bin/tests/test_deploy_nao_mata_elo.sh:740`). |
   | 21, chave do lote | Mata voltar à chave única. Porém retirar “PAUTA NÃO ABRIU” do relato passa: `_pauta` foi substituída por coletor que retorna `None`, sem cenário de falha. **Fila** (`bin/tests/test_deploy_nao_mata_elo.sh:750`, `bin/tests/test_deploy_nao_mata_elo.sh:808`, `app/core/integrador_lote.py:781`). |
   | 21, revert do revert | Mata ignorar completamente o rc, mas só injeta 75. Alterar a composição para anunciar apenas rc75, silenciando rc1, passa. **Fila** (`bin/tests/test_deploy_nao_mata_elo.sh:796`, `bin/tests/test_deploy_nao_mata_elo.sh:813`, `app/core/esteira_vigia.py:1293`). |
   | 20, lacuna | Detecta ausência na fonte viva dentro do universo construído. Contudo, retirar um cron desse universo pelo classificador também o retira da checagem; só dois nomes têm presença explicitamente exigida. Mutante completo executado: **[não verificado]**. **Fila de independência da prova** (`bin/tests/test_deploy_nao_mata_elo.sh:689`, `bin/tests/test_deploy_nao_mata_elo.sh:699`, `bin/tests/test_deploy_nao_mata_elo.sh:707`, `bin/tests/test_deploy_nao_mata_elo.sh:711`). |
   | 23, CLI | **Sobrevivente confirmado em memória:** trocar `==` por `!=` mantém a aceitação da AST, mas impede atender o comando correto. O teste verifica string e chamada, sem verificar o operador. **Fila** (`app/core/esteira_vigia.py:1657`, `bin/tests/test_deploy_nao_mata_elo.sh:904`, `bin/tests/test_deploy_nao_mata_elo.sh:905`). |
   | 23, resolução por id | Mata resolver o integrador com sua recusa ainda presente. Não testa falha do resolvedor, cura por outro deploy nem migração da chave velha; `_pauta` é coletor. **Fila** (`bin/tests/test_deploy_nao_mata_elo.sh:865`, `bin/tests/test_deploy_nao_mata_elo.sh:878`, `bin/tests/test_deploy_nao_mata_elo.sh:888`). |

   Essas lacunas não tornam todos os REDs inúteis: existem mutantes específicos mortos. Impedem, porém, tratar AST como execução da CLI ou chamada ao coletor como resolução efetiva de pauta (`bin/tests/test_deploy_nao_mata_elo.sh:661`, `bin/tests/test_deploy_nao_mata_elo.sh:808`, `bin/tests/test_deploy_nao_mata_elo.sh:865`, `bin/tests/test_deploy_nao_mata_elo.sh:904`; exigência RED em `AVAL_DEPLOY-ELO-B3.txt:12`, anexo).

5. **Classes da casa no delta da volta 2:**

   | Classe | Resultado |
   |---|---|
   | (a) Segundo juiz registrado | **Não identificado no delta.** A pauta continua passando pelo comando e pelos serviços; o lookup aponta o registro vivo, não constitui outra autoridade (`app/core/management/commands/alarme_esteira.py:27`, `app/docs/JUIZES.md:10`). |
   | (b) Segundo escritor fora da porta | **Presente na gestão das recusas, fila:** o delta mantém no agendado uma exclusão de estado também gerido pelo deploy. A substituição pode ser correta, mas contradiz “ninguém mais apaga” do contador. Para estado ORM de pauta, a porta permanece canônica (`bin/deploy_agendado.sh:110`, `bin/deploy.sh:367`, `bin/deploy_recusado.sh:7`, `app/pautas/services.py:180`, `app/pautas/services.py:272`). |
   | (c) Relógio fora da borda | **Não identifiquei nova leitura de relógio no código de produção do delta.** O corte por mtime já está no diff total e tem a supercontagem descrita acima (`bin/deploy.sh:282`, `bin/deploy.sh:367`; `diff_v6_delta.patch`, anexo). |
   | (d) Derivação/lista paralela | **Sim, fila:** `DEPLOY_RECUSA_IDS` enumera separadamente os mesmos ids escritos pelos produtores; eles não consultam a tupla (`app/core/esteira_vigia.py:1316`, `app/core/esteira_vigia.py:1325`, `app/core/integrador_lote.py:771`, `bin/vigia_logins.sh:90`). |
   | (e) RED sem mordida | **Mordida parcial, fila**, especialmente CLI por AST e limpeza com marcador fabricado; não são provas integrais dos caminhos anunciados (`bin/tests/test_deploy_nao_mata_elo.sh:738`, `bin/tests/test_deploy_nao_mata_elo.sh:904`). |
   | (f) Ordem de trava | Não demonstrada nova inversão no delta; o fd7 continua pertencendo ao pai. **[não verificado]** quanto à ausência de toda corrida possível (`bin/vigia_logins.sh:70`, `bin/vigia_logins.sh:90`). |
   | (g) Pouso antes do pré-requisito | O FIM do lote está depois da guarda de sucesso do deploy. Pouso próprio e janela autorizada: **[não verificado]** (`app/core/integrador_lote.py:748`, `app/core/integrador_lote.py:751`; `AVAL_DEPLOY-ELO-B3.txt:14`, anexo). |
   | (h

[truncado em 20000 de 22609 caracteres]

## 04/11 11:02 -- HISTORICO (mais de 24 h) · sessao c956083d529c · - · - · origem=Ronald (tmux)

Revisão só leitura das FILA/DESCARTADO:

| Triagem | Resultado e gravidade | Evidência |
|---|---|---|
| DEPLOY × DEPLOY | O diagnóstico técnico sustenta-se: execs do deploy ficam fora da admissão; só o restart é serializado. A justificativa “fora do aval” não elimina a proibição expressa de restart sobre exec vivo. **Bloqueia pelo PROIBIDO do aval**, se aplicado literalmente; ocorrência em produção [não verificado]. | `bin/deploy.sh:102`, `bin/deploy.sh:124`, `bin/deploy.sh:205`, `bin/deploy.sh:315`; `bin/reinicio_sob_admissao.sh:28`, `bin/reinicio_sob_admissao.sh:43`, `bin/reinicio_sob_admissao.sh:66`; novo caminho `app/core/esteira_vigia.py:1355`, `bin/vigia_arvore.sh:28`. |
| Wrapper morto por timeout/SIGKILL | Só há trap EXIT e a marca identifica o wrapper, enquanto docker roda como filho. Morto o wrapper, o reaper elimina a marca; falta garantia de proteção do filho órfão. “SIGKILL não se trapa” explica a falha, mas não satisfaz a proibição de restart sobre exec vivo. **Bloqueia pelo PROIBIDO do aval**; sobrevivência concreta em produção [não verificado]. | `bin/em_casca.sh:21`, `bin/em_casca.sh:26`; `bin/admissao_cron.sh:167`, `bin/admissao_cron.sh:175`, `bin/admissao_cron.sh:208`; timeouts em `eval/harness.py:56`, `eval/gerador.py:66`, `eval/juiz.py:245`. |
| Marca casca=0 antes de rc64 | Sustenta-se: marca nasce antes da validação; o comando não roda e EXIT desmarca. **Fila**, sem violação operacional demonstrada. | `bin/em_casca.sh:16`, `bin/em_casca.sh:21`, `bin/em_casca.sh:22`, `bin/em_casca.sh:24`, `bin/em_casca.sh:26`. |
| Marca órfã após morte sem EXIT | DESCARTA sustenta-se quanto à remoção: pid/starttime/cmdline são conferidos e a marca órfã é apagada. Isso não cura o filho órfão do item anterior. | `bin/admissao_cron.sh:167`, `bin/admissao_cron.sh:175`, `bin/admissao_cron.sh:177`, `bin/admissao_cron.sh:208`. |
| Reload inicial integrador ignora rc | Sustenta-se; segue imediatamente à suite. **Fila**, preexistência consta do diff fornecido. | `app/core/integrador_lote.py:638`, `app/core/integrador_lote.py:639`. |
| Rollback dos moldes ignora rc | Sustenta-se: comando de reload seguido de mensagem de sucesso sem guarda de retorno. **Fila**, fora do delta fornecido. | `bin/molde_fatia/cadeia.sh:25`, `bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:33`; `bin/molde_autocura/cadeia.sh:24`, `bin/molde_autocura/cadeia.sh:31`, `bin/molde_autocura/cadeia.sh:32`. |
| fd7 liberado cedo | DESCARTA como defeito atual sustenta-se: pai mantém fd7; `7>&-` fecha apenas nos filhos. **Fila** para mordida do selo. | `bin/vigia_logins.sh:70`, `bin/vigia_logins.sh:71`, `bin/vigia_logins.sh:90`, `bin/vigia_logins.sh:95`. |
| Corte mtime antes da espera | Sustenta-se no cenário descrito: recusa produzida durante espera fica, embora o restart posterior possa tê-la superado; supercontagem. **Fila**. | `bin/deploy.sh:282`, `bin/deploy.sh:283`, `bin/deploy.sh:366`, `bin/deploy.sh:367`. |
| Mesmo id concorrente disputa `.novo` | Sustenta-se: destino e temporário derivam apenas do id. Chamadores normais possuem travas próprias, mas deploy inteiro não tem mutex comum. **Fila**, ligada à corrida DEPLOY × DEPLOY acima. | `bin/deploy.sh:52`, `bin/deploy.sh:54`, `bin/deploy.sh:286`, `bin/deploy.sh:287`; `bin/integrador.sh:9`, `bin/integrador.sh:10`; `bin/vigia_arvore.sh:23`, `bin/vigia_arvore.sh:24`; `bin/vigia_logins.sh:70`. |
| CLI retorna 0 / tail come rc | Sustenta-se: main retorna 0 e shell usa pipeline sem decidir pelo resultado. **Fila** quanto ao contrato atual; não prova resolução bem-sucedida. | `app/core/esteira_vigia.py:1657`, `app/core/esteira_vigia.py:1658`, `app/core/esteira_vigia.py:1659`; `bin/vigia_arvore.sh:28`. |
| Texto “não reiniciaram” para rc1 | Sustenta-se como defeito: rc1 pode vir depois do restart, inclusive prova de rota. **Fila de texto**, mas o relato pode descrever estado incorreto. | `app/core/esteira_vigia.py:1329`, `app/core/esteira_vigia.py:1331`; `bin/deploy.sh:283`, `bin/deploy.sh:337`, `bin/deploy.sh:344`. |
| Retentar não confere nem marca auto-revert | Sustenta-se: sucesso apenas resolve pautas, relata e retorna; não volta ao bloco de testes/marcação. **Fila**, salvo interpretação mais ampla do fechamento MUDA5. | `app/core/esteira_vigia.py:1283`, `app/core/esteira_vigia.py:1301`; `app/core/esteira_vigia.py:1362`, `app/core/esteira_vigia.py:1365`, `app/core/esteira_vigia.py:1367`. |
| argv perde campos vazios | Sustenta-se; produtor atual passa dois campos não vazios. **Fila**. | `app/core/esteira_vigia.py:1353`, `app/core/esteira_vigia.py:1276`. |
| Cabeçalho 1586 | Correção confere com JSON da cópia. Números vivos permanecem apenas no dado fornecido. | `bin/admissao_cron.sh:31`; `app/config/crons_duracao.json:82`. |
| ADM_ESPERA_ADMISSAO_S “só encurta” | **Motivo parcialmente falso**: env livre também aumenta acima de 900, sem clamp. Verdadeiro que não altera teto do restart. **Fila**; não é alteração do delta. | `bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:243`; teto separado em `bin/admissao_cron.sh:55`, `bin/admissao_cron.sh:58`; `bin/reinicio_sob_admissao.sh:18`, `bin/reinicio_sob_admissao.sh:48`. |

CONFERIR: a rede do harness está dentro da string `_TIRO`, enviada ao `docker exec` que passa pelo wrapper; portanto o código cobre aquele trecho pela marca. Não há chamada HTTP direta do host nessa função. Prova de execução da conferência/linha no RELATO [não verificado]. `eval/harness.py:14`, `eval/harness.py:18`, `eval/harness.py:24`, `eval/harness.py:53`, `eval/harness.py:54`.

Outro detalhe do delta para consolidar: o deploy do revert do revert lê o rc, mas não fixa `DEPLOY_RECUSA_ID=auto_revert`; se recusado, usa id herdado ou `manual`, enquanto o retentador só lê `recusa_cron.auto_revert.pendente`. O rc apenas aparece em pauta `auto_revert`; o retorno dessa `_pauta` continua ignorado. `app/core/esteira_vigia.py:1289`, `app/core/esteira_vigia.py:1291`, `app/core/esteira_vigia.py:1311`, `app/core/esteira_vigia.py:1345`; `bin/deploy.sh:52`.

## 04/11 11:02 -- HISTORICO (mais de 24 h) · sessao f949e30a8f81 · - · - · origem=Ronald (tmux)

Censo e achados, apenas por leitura:

1. **Pauta por chamador: cura parcial; fechamento incompleto — fila.** Os emissores passam as novas chaves em `app/core/esteira_vigia.py:1332`, `app/core/esteira_vigia.py:1357`, `app/core/integrador_lote.py:780` e `bin/vigia_logins.sh:95`. A resolução automática existe somente no caminho de sucesso de `retentar_deploy`, em `app/core/esteira_vigia.py:1362-1364`. Um deploy terceiro pode apagar a recusa de auto-revert em `bin/deploy.sh:367`; depois disso `retentar_deploy` retorna antes da resolução em `app/core/esteira_vigia.py:1349-1350`. Logo, “fecha quando a recusa já não está de pé” excede o comportamento implementado (`app/core/esteira_vigia.py:1314-1315`).

2. **Falha de resolução fica sem nova tentativa — fila.** `_pauta` devolve rc em `app/core/esteira_vigia.py:905`, mas o loop de resolver ignora esse retorno em `app/core/esteira_vigia.py:1364`. A recusa própria já foi apagada pelo deploy de sucesso (`bin/deploy.sh:363`); a próxima passada retorna “nada pendente” (`app/core/esteira_vigia.py:1349-1350`).

3. **Chave antiga órfã — fila; existência em produção [não verificado].** Não há chamada específica atual com `--resolver deploy_recusado` sem sufixo; a única resolução específica usa `deploy_recusado:%s` (`app/core/esteira_vigia.py:1364`). A porta genérica continua aceitando qualquer chave (`app/core/management/commands/alarme_esteira.py:21-24`), procurando `ESTEIRA <chave>:` (`app/core/management/commands/alarme_esteira.py:13-15`). As novas resoluções não encontram texto antigo `ESTEIRA deploy_recusado: ...` (`app/core/management/commands/alarme_esteira.py:15`). Ações genéricas de resolver em `app/core/esteira_vigia.py:1642` recebem as chaves produzidas por `decidir`; seus resolvedores concretos são `pausa_sem_dono`, `teto_morto`, `trava_a_vazia`, `vigia_sem_efeito` (`app/core/esteira_vigia.py:490`, `:504`, `:530`, `:597`).

4. **`DEPLOY_RECUSA_IDS`: lista paralela — fila, classe (d).** A tupla é definida em `app/core/esteira_vigia.py:1316` e lida somente pelo loop em `app/core/esteira_vigia.py:1362`. Os emissores repetem os IDs em literais independentes: auto-revert (`app/core/esteira_vigia.py:1325`, `:1332`, `:1354`, `:1357`), integrador (`app/core/integrador_lote.py:771`, `:780`) e login (`bin/vigia_logins.sh:90`, `:95`). Trocar/adicionar um produtor não atualiza automaticamente o universo de fechamento (`app/core/esteira_vigia.py:1362-1364`).

5. **Revert do revert: rc lido, comunicação parcial — fila.** `rrd` nasce do `.returncode` em `app/core/esteira_vigia.py:1289-1290` e seu único leitor é o ternário que acrescenta texto à pauta em `app/core/esteira_vigia.py:1293-1294`. O retorno dessa `_pauta` é ignorado (`app/core/esteira_vigia.py:1291`), o relato não inclui `rrd` (`app/core/esteira_vigia.py:1295-1297`) e o retorno continua “revert desfeito” (`app/core/esteira_vigia.py:1298`). Uma pauta `auto_revert` já viva impede reescrever o novo rc (`app/core/management/commands/alarme_esteira.py:38-41`); haver tal pauta em produção é [não verificado].

6. **Revert do revert não usa explicitamente o ID auto-revert — fila.** O segundo deploy não recebe `env=...DEPLOY_RECUSA_ID` (`app/core/esteira_vigia.py:1289-1290`); herda o ambiente, ou usa `manual` pela regra de `bin/deploy.sh:52`. Seu rc75 deixa recusa correspondente (`bin/deploy.sh:286-290`), enquanto a repetição automática só lê `recusa_cron.auto_revert.pendente` (`app/core/esteira_vigia.py:1311`, `:1345-1351`).

7. **Arquivos de recusa, produção:**
   - Criador/sobrescritor: `bin/deploy.sh:286-287`, formato `id` + argv NUL; nome definido em `bin/deploy.sh:54`.
   - Apagador próprio: `bin/deploy.sh:363`; apagador global das anteriores: `bin/deploy.sh:366-368`.
   - Agendado remove o nome da tentativa antes de iniciá-la (`bin/deploy_agendado.sh:95`, `:104`); substitui a recusa anterior sob condição (`bin/deploy_agendado.sh:102`, `:110`).
   - Agendado lê conteúdo/ID em `bin/deploy_agendado.sh:83-87` e argv em `bin/deploy_agendado.sh:101`.
   - `retentar_deploy` lê conteúdo da recusa auto-revert (`app/core/esteira_vigia.py:1345-1353`) e existência das três recusas (`app/core/esteira_vigia.py:1363`).
   - Contador lê nomes/existência, sem conteúdo (`bin/deploy_recusado.sh:22-25`); placar lê sua saída e soma (`bin/placar_code.sh:464-466`, `:542`).
   - Testes criam/apagam recusas nas fixtures do caso 8 (`bin/tests/test_deploy_nao_mata_elo.sh:181-190`), caso 21 (`:716-737`) e caso 23 (`:858`, `:869`, `:879`); executam os blocos extraídos de produção em `:238-240` e `:738-739`.

8. **`_rec_ant`: cura o rc1 sem recusa nova.** O caminho antigo fica preservado em `bin/deploy_agendado.sh:102` e só é removido com rc0 ou rc75 e arquivo novo em `bin/deploy_agendado.sh:110`. A validação de ID da recusa nova vem depois, em `bin/deploy_agendado.sh:115`; portanto o teste `-f` que autoriza apagar a anterior não valida seu conteúdo (`bin/deploy_agendado.sh:110`, `:83-87`). Isso é divergência pequena de contrato, não prova de colisão real [não verificado].

9. **Porta canônica de pautas preservada.** A abertura e fechamento passam por `escrever`/`marcar_feita_pelo_sistema` (`app/core/management/commands/alarme_esteira.py:27`, `:31`, `:54-56`); as escritas de estado ficam em `app/pautas/services.py:180` e `:272-273`. Os emissores do delta não escrevem diretamente estado ORM de pauta (`app/core/integrador_lote.py:780`, `app/core/esteira_vigia.py:1332`, `:1364`, `bin/vigia_logins.sh:94-95`).

_1527 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
