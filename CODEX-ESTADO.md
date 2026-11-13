# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 12:28** · publicado em **13/11/2026 12:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 12:28 · sessao 9e24452e1e42 · HJ-090 · censo · origem=Code

Censo da cópia: execução efetiva desse código em produção, quantidade de períodos divergentes e valores atuais dos registros: **[nao verificado]**.

1. **Escritores de `dias_concedidos` e `dias_vendidos`**

| Escritor | Lê → altera → grava | `atomic` | `select_for_update` do período |
|---|---|---|---|
| `registrar_gozo_historico` | Sim, sobre objeto recebido: `concedidos=min(direito, concedidos+ag.dias)`; `vendidos+=vendidos`. | Próprio. | Não. [services.py:270–302](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:270) |
| `executar_agendamento` | Sim, sobre objeto recebido: cria agendamento e faz `concedidos+=dias`. | Próprio. | Não; a consulta de duplicata também não trava. [services.py:306–326](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:306) |
| `agendar_ferias_porta` | Delega o gozo ao anterior; relê com `refresh_from_db` e faz `vendidos+=dias_vendidos`. | Próprio, envolvendo gozo e venda. | Não; `refresh_from_db` não acrescenta trava. [services.py:156–165](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:156) |
| `cancelar_agendamento_dp` | Sim, sobre linha relida: `concedidos-=vivo.dias`. | Próprio. | **Sim**, via `_travar_agendamento`. [services.py:379–385](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:379) |
| `executar_cancelamento_admin` | Sim, sobre linha relida: `concedidos-=vivo.dias`. | Próprio. | **Sim**, pelo mesmo helper. [services.py:402–417](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:402) |
| `_trocar_janela` | Sim: `concedidos=concedidos-vivo.dias+novos_dias`. | Próprio. | **Sim**, já nesta cópia. [services.py:433–452](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:433) |
| `aprovar_solicitacao` | Sim: lê período, cria agendamento e faz `concedidos+=solic.dias`. | Sem `atomic` próprio; o chamador de produção encontrado fornece `atomic`. | **Não do período**; a view trava a **solicitação**. [services.py:469–508](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:469), [views.py:739–753](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/views.py:739) |
| `vender_dias_do_periodo` | Sim, sobre objeto recebido: `vendidos+=dias_v`. | Sem `atomic` próprio ou no chamador encontrado. | Não. [services.py:580–584](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/services.py:580), [views.py:938–958](/home/ronald/codex_ws/7621a973fc0dea79ce58ba3cb35d7a49aa98c4fe/app/ferias/views.py:938) |

O helper trava primeiro o agendamento, recusa pelo status relido e depois trava o período. Portanto, a proteção FERIAS-CANCELAR-2X já existe nos dois cancelamentos e na troca de janela. Cancelado é recusado; concluído também é recusado no cancelamento comum, mas pode entrar no cancelamento administrativo especial. (`app/ferias/services.py:357–369,380,406–407`; `app/ferias/models.py:302–305`)

Há também **inicialização**: `garantir_periodo_aquisitivo` usa `get_or_create`, recebendo os dois contadores zerados da view; a criação dos ciclos está em `atomic`. Quando o período existe, essa porta atualiza somente status/carimbo. (`app/ferias/services.py:254–266`; `app/ferias/views.py:165,181–185`)

As migrations dos campos são `CreateModel` com concedidos zero e `AddField` com vendidos zero; não calculam soma de agendamentos. O admin de férias registra apenas `Afastamento`; o `save` de período só verifica sobreposição e delega ao Django, sem recompor contadores. (`app/ferias/migrations/0001_initial.py:39–51`; `app/ferias/migrations/0004_periodoaquisitivo_dias_vendidos.py:12–16`; `app/ferias/admin.py:1–5`; `app/ferias/models.py:201–203`)

Não foram localizados outros escritores desses contadores em commands, signals ou migrations de dados da cópia; escritores externos à cópia: **[nao verificado]**.

O clamp relevante é o `min` do histórico: o agendamento conserva os dias integrais, mas concedidos fica limitado ao direito. **Inferência aritmética do código:** partindo de concedidos zero/direito 30, um histórico de 33 grava agendamento 33 e concedidos 30. Ocorrência concreta em produção: **[nao verificado]**. (`app/ferias/services.py:281,298–302`)

Os cancelamentos subtraem os dias integrais, sem piso e sem devolver vendidos. Assim, **se** a linha travada tiver concedidos 30 e o agendamento cancelável tiver 33, o resultado gravado será **−3**, mesmo com a proteção contra segundo cancelamento. (`app/ferias/services.py:380–385,407–410`)

O BACKLOG ainda descreve `_trocar_janela` como sem trava e aprovação como sem `atomic`; a implementação atual já apresenta trava na primeira e transação fornecida pela view na segunda. (`app/docs/BACKLOG.md:1849`; `app/ferias/services.py:433–434`; `app/ferias/views.py:739–753`)

2. **Leitores e ESMERIL-SEMANTICA**

O significado central é **concedidos = gozo reservado/consumado; vendidos separado**. O saldo é `direito − concedidos − vendidos`, sem clamp. Nenhuma dessas properties recompõe concedidos pela soma dos agendamentos. (`app/templates/ferias/_secao_colab.html:49–51`; `app/ferias/models.py:216–239`)

Além das leituras dos próprios escritores inventariadas acima, os leitores diretos encontrados são:

| Leitor | O que usa ou pressupõe |
|---|---|
| `status_de_periodo` | Calcula direito−concedidos−vendidos; saldo **≤0**, inclusive negativo, vira `concedido`. (`app/ferias/models.py:95–102`) |
| `dias_saldo` | Retorna a diferença crua; admite negativo. (`app/ferias/models.py:216–217`) |
| `saldo_para_remarcar` | Retorna saldo+`agendamento.dias`; pressupõe que a reserva antiga está debitada em concedidos. Não verifica soma, status ou vínculo nesse método. (`app/ferias/models.py:219–222`) |
| `pode_agendar` | Exige status armazenado `adquirido` e saldo positivo. (`app/ferias/models.py:230–231`) |
| `dias_vendaveis` / `pode_vender` | `max(0,...)` limita somente o teto restante de venda; vender exige adquirido, teto positivo e saldo positivo. (`app/ferias/models.py:234–239`) |
| KPI/filtro `sem_agend` | Equipara adquirido+concedidos zero a “sem agendamento”; não consulta existência/soma e não considera vendidos nesse filtro. (`app/ferias/views.py:31,60`; `app/relatorios/services.py:290`) |
| `_validar_agendamento` | Compara pedido ao saldo acumulado; consulta não cancelados para duplicata/fracionamento, sem recompor saldo. (`app/ferias/views.py:227–243`) |
| `checar_ferias_com_abono` | Compara gozo+venda ao saldo; `saldo_depois` também não tem piso. (`app/ferias/services.py:195–205`) |
| Aprovação de solicitação | Compara pedido ao saldo do primeiro período adquirido; excesso gera aviso sem materialização naquele ramo. (`app/ferias/services.py:483–506`) |
| Correção/remarcação | Compara novos dias a `saldo_para_remarcar`. (`app/ferias/views.py:478–480,680–682`) |
| Auditoria administrativa | Fotografa concedidos antes/depois de cancelar, desfazer concluída e corrigir datas. (`app/ferias/views.py:368,383,416,427,486,518`) |
| Edição do período | Impede direito menor que concedidos; **não acrescenta vendidos nessa guarda**. (`app/ferias/views.py:812–813`) |
| Venda pela tela | Compara venda ao teto vendável e ao saldo, separadamente. (`app/ferias/views.py:950–953`) |
| `situacao_de_ferias` | Delega à property; detecta negativo como inconsistência, mas soma somente saldos positivos e devolve somente períodos positivos. (`app/ferias/situacao.py:68–69,178–189,296–297`) |
| Avisos vencido/vencendo | Consideram somente saldo positivo. (`app/ferias/situacao.py:192–216`) |
| `pautas_de_ferias` | Usa a property; negativo entra em cadastro inconsistente. (`app/ferias/management/commands/pautas_de_ferias.py:73,79–80,96–97`) |
| `alertar_ferias_vencendo` | Guarda saldo cru após sincronizar status e o apresenta como dias a usar. (`app/ferias/management/commands/alertar_ferias_vencendo.py:31–41,71–83`) |
| IA: seleção/defaults | Seleciona períodos com saldo positivo, sem exigir adquirido nessa seleção; duração padrão `min(30,saldo)`. (`app/core/views_ia.py:258–272,489–503`) |
| IA: “vender o resto” | `max(0,saldo−dias)` corta a **venda proposta**, não o saldo individual. (`app/core/views_ia.py:532–538`) |
| API de situação | Publica o total/lista já filtrados por `situacao_de_ferias`; mantém os avisos. (`app/api/views_mensageria.py:835–846`) |
| API geral/ficha | Publica saldo cru dos períodos selecionados para vencimento. (`app/api/views_mensageria.py:954,1103`) |
| PDF/CSV | Exibe saldo cru, sem recompor soma. (`app/relatorios/services.py:316`; `app/relatorios/views.py:370`) |
| Tela de saldo | Exibe concedidos e saldo cru; botão exige adquirido+saldo positivo. (`app/templates/ferias/saldo.html:37,41,46`) |
| Seção do colaborador | Exibe “Gozo”, saldo e vendidos separados; formulário usa `pode_agendar`, saldo como valor/max; negativo cai na mensagem genérica de indisponibilidade. (`app/templates/ferias/_secao_colab.html:49–57,67–68,77`) |
| Listagem, formulário individual, lote e relatório HTML | Exibem saldo cru; formulário individual também usa saldo como valor/max. (`app/templates/ferias/_tabela_ferias.html:27`; `app/templates/ferias/agendar.html:7,22`; `app/templates/ferias/agendar_lote.html:40`; `app/templates/relatorios/ferias_vencendo.html:48`) |

**Não é semanticamente uniforme em todos os usos:**

- A tela distingue gozo de vendidos, mas o aviso de inconsistência chama **concedidos+vendidos** de “dias gozados”. (`app/templates/ferias/_secao_colab.html:49–51`; `app/ferias/situacao.py:178–183`)
- O saldo individual negativo aparece nas telas citadas; o resumo/API **omite negativos da soma e da lista**, embora produza aviso. (`app/templates/ferias/saldo.html:41`; `app/ferias/situacao.py:178–189,296–297`; `app/api/views_mensageria.py:835–846`)
- O docstring de `saldo_total` fala em períodos adquiridos; a implementação carrega todos os períodos e filtra apenas saldo positivo. (`app/ferias/situacao.py:146,162–163,296–297`)
- A igualdade concedidos=soma dos não cancelados é uma pressuposição operacional das contas de devolver/substituir dias, **não uma igualdade conferida por esses leitores**. (`app/ferias/models.py:217–222`; `app/ferias/services.py:381,408,436`)

3. **Duas views, entrada da porta e demais chamadores**

`editar_datas_admin` consulta `saldo_para_remarcar` em 478 e chama `corrigir_datas_agendamento`; `editar_agendamento` consulta em 680 e chama `remarcar_agendamento`. Ambos os wrappers encaminham diretamente a `_trocar_janela`. (`app/ferias/views.py:478–503,680–688`; `app/ferias/services.py:457–466`)

A porta recebe **quatro argumentos**: objeto de agendamento, novo início, novo fim e novos dias. Não recebe saldo, request, usuário ou motivo. Ela relê o agendamento pelo PK e obtém o período vinculado à linha relida. (`app/ferias/services.py:422,434`; `app/ferias/services.py:364–368`)

O ponto correspondente à descida da pergunta descrita no aval fica **após `_travar_agendamento`, na linha 434, antes da alteração de concedidos na 436 e da substituição de `vivo.dias` na 437**: ali existem período e agendamento relidos sob trava, ainda com a reserva antiga. Atualmente esse bloco não consulta `saldo_para_remarcar`. (`app/ferias/services.py:433–437`)

Os dois chamadores diretos de produção encontrados de `_trocar_janela` são esses wrappers; os testes também exercitam ambos. (`app/ferias/services.py:459,466`; `app/ferias/test_cancelar_2x.py:239–248`; `app/ferias/test_remarcar_limpa_janela_f346.py:198`)

4. **Status que entram na soma “não cancelados”**

O catálogo contém somente **`agendado`, `em_gozo`, `concluido`, `cancelado`**. Portanto, excluir cancelados inclui os outros três, inclusive concluídos; o fracionamento já usa explicitamente esse trio. Não há quinto status declarado. Valores fora do catálogo existentes em produção: **[nao verificado]**. (`app/ferias/models.py:293–298,360–363`)

`recusado` pertence a **`confirmacao`**, não ao status do agendamento. A resposta de recusa grava apenas confirmação; não cancela nem devolve dias, portanto permanece incluída na definição “não cancelados”. (`app/ferias/models.py:321–324`; `app/ferias/services.py:567–575`)

“Agendamentos vivos” em `situacao_de_ferias` é outro recorte: apenas agendado/em_gozo. Não equivale a “não cancelados”, que inclui concluído. (`app/ferias/situacao.py:164–168`; `app/ferias/models.py:293–298`)

5. **Trilha e molde DRY/apply/reversão**

**Existe infraestrutura compatível:** `LogAuditoria` guarda modelo, objeto, descrição, JSON antes/depois, usuário/ator e data. `core.observ.evento` aceita esses elementos sem request, identifica processo `manage.py` como `sistema:<comando>` e retorna registro ou `None`. (`app/core/models.py:295–307`; `app/core/observ.py:22–32,94–102,133–139`)

Essa porta é **best effort**, com savepoint próprio; falha da trilha não desfaz automaticamente o negócio. O adaptador `registrar_log` também captura falhas e não devolve o registro. (`app/core/observ.py:105,133–139`; `app/core/auditoria.py:8–29`)

Em férias, o molde de antes/depois/motivo já aparece nos cancelamentos administrativos e na correção de datas. Entretanto, os logs ficam **nas views, após a chamada ao serviço**, com o “antes” capturado anteriormente à trava; `_trocar_janela` não registra auditoria no seu próprio bloco. (`app/ferias/views.py:366–384,414–428,484–518`; `app/ferias/services.py:422–454`)

O molde vizinho mais completo encontrado é **`rederivar_minutos_h06_comp11`**, pela combinação dos seguintes elementos:

| Elemento existente | Evidência |
|---|---|
| DRY padrão, `--apply`, `--reverter SNAPSHOT`, autor obrigatório para escrita. | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:195–214` |
| Transação e releitura sob trava por linha; DRY retorna antes de chamar escritor. | Mesmo arquivo: `137–164`. |
| Snapshot antes da escrita; chamada à porta existente; conferência do depois. | Mesmo arquivo: `165–180`. |
| Auditoria antes/depois/motivo; trilha ausente provoca desfazimento da linha. | Mesmo arquivo: `181–192`. |
| Snapshot em `logs/h12h06b`, criação exclusiva e `flush/fsync`; confirmação posterior à aplicação. | Mesmo arquivo: `215,248–257,273–274`. |
| Reversão apenas de tentativas confirmadas, com guardas contra atos posteriores; listagem, resultado e contadores. | Mesmo arquivo: `152–159,220–241,261–288`. |

No próprio férias, `sincronizar_status_ferias` fornece DRY/apply, listagem, contador e escrita pela porta, mas seu corpo não contém snapshot/reversão. (`app/ferias/management/commands/sincronizar_status_ferias.py:20–57`)

Porta específica já existente para realinhar concedidos à soma e implementação do contador `ferias_concedidos_x_soma`: **não localizadas na cópia [nao verificado]**. O contador atual de divergência examina status de agendamentos/períodos. (`app/ferias/services.py:71–95`)

6. **Entidade de `#103` e `#114`**

**Não é possível identificar a entidade desses dois números do aval: [nao verificado].**

A ocorrência exata de `#103` encontrada no repositório está em documentação sobre **disputas**, junto de `#1257`; não estabelece vínculo com HJ-090 nem prova que seja o mesmo registro citado no aval. (`app/docs/RELATO-ARQUIVO.md:2834–2837`)

Não foram localizados `#114` exato, HJ-090 ou o arquivo `complemento_hj_090_1221.txt` na cópia que qualificassem esses IDs. Classificá-los como PK de período, agendamento, chamado ou HJ permanece **[nao verificado]**.

## 13/11 12:27 · sessao dc1313b29315 · HJ-090 · sonda · origem=Code

Não executei: `sonda2.py` não foi localizado na cópia. [nao verificado] [nao verificado]

Saída integral fornecida no enunciado:

```text
sonda_leitura: sonda2.py [092f19c733df6958] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

total 52
por origem {'agendamento': 1, 'historico': 51}
por status do periodo usado {'concedido': 40, 'em_curso': 12}
origem x status {'agendamento/concedido': 1, 'historico/concedido': 39, 'historico/em_curso': 12}
ag colab inicio dias status_ag origem pa_usado status_pa anteriores_com_saldo(pa:saldo)
12 170 2026-05-28 30 concluido agendamento 1019 concedido 1018:30
56 95 2026-07-13 30 concluido historico 350 concedido 349:30
57 247 2026-07-28 30 concluido historico 427 concedido 426:30
58 369 2026-08-17 30 concluido historico 1051 concedido 631:30
59 41 2026-07-13 30 concluido historico 74 concedido 73:30
61 125 2026-08-03 30 concluido historico 587 concedido 586:30
62 121 2026-08-11 30 concluido historico 547 concedido 546:30
63 163 2026-08-13 30 concluido historico 966 concedido 965:30
64 104 2026-07-20 30 concluido historico 398 concedido 397:30
65 128 2026-08-06 20 concluido historico 615 em_curso 614:30
66 138 2026-08-03 30 concluido historico 706 concedido 705:30
67 149 2026-08-06 30 concluido historico 813 concedido 812:30
74 70 2026-09-08 30 concluido historico 157 concedido 156:30
75 118 2026-09-16 30 concluido historico 534 concedido 533:30
76 136 2026-09-15 20 concluido historico 701 em_curso 700:30
79 357 2026-07-07 20 concluido historico 387 em_curso 386:30
81 191 2026-07-08 30 concluido historico 343 concedido 342:30
83 219 2026-07-16 30 concluido historico 384 concedido 383:30
84 198 2026-08-05 30 concluido historico 1005 concedido 1004:30
87 355 2026-08-12 20 concluido historico 946 em_curso 945:30
88 367 2026-08-06 20 concluido historico 1054 em_curso 1047:30
89 189 2026-08-10 30 concluido historico 329 concedido 328:30
91 340 2026-09-02 30 concluido historico 294 concedido 293:30
92 218 2026-09-01 30 concluido historico 646 concedido 645:30
93 187 2026-09-08 30 concluido historico 412 concedido 411:30
94 193 2026-09-22 30 concluido historico 990 concedido 989:30
95 325 2026-08-12 30 concluido historico 187 concedido 186:30
96 188 2026-09-02 30 concluido historico 789 concedido 788:30
97 245 2026-08-19 30 concluido historico 775 concedido 774:30
98 354 2026-08-10 30 concluido historico 650 concedido 649:30
99 341 2026-08-20 30 concluido historico 133 concedido 132:30
100 371 2026-09-16 24 concluido historico 1056 em_curso 467:30
101 373 2026-09-02 24 concluido historico 1058 em_curso 664:30
102 306 2026-10-01 30 concluido historico 216 concedido 215:30
103 344 2026-10-06 33 concluido historico 639 concedido 638:30
104 347 2026-10-08 30 concluido historico 375 concedido 374:30
105 303 2026-10-20 30 em_gozo historico 496 concedido 495:30
106 274 2026-10-22 30 em_gozo historico 1059 concedido 207:30
107 374 2026-09-15 30 concluido historico 1060 concedido 601:30
108 236 2026-10-05 30 concluido historico 873 concedido 872:30
109 168 2026-10-22 28 em_gozo historico 997 em_curso 996:30
110 119 2026-10-22 30 em_gozo historico 536 concedido 535:30
111 215 2026-10-05 30 concluido historico 544 concedido 543:30
112 366 2026-09-01 30 concluido historico 1057 concedido 408:30
114 127 2026-09-02 31 concluido historico 598 concedido 597:30
115 101 2026-11-09 20 em_gozo historico 394 em_curso 393:30
116 61 2026-11-11 30 em_gozo historico 68 concedido 67:30
117 80 2026-11-16 24 agendado historico 238 em_curso 237:30
118 165 2026-11-24 20 agendado historico 972 em_curso 971:30
119 148 2026-12-14 20 agendado historico 809 em_curso 808:30
120 93 2026-12-21 30 agendado historico 1063 concedido 319:30
122 309 2026-10-29 30 em_gozo historico 652 concedido 1066:30
ERROS 0
```

## 13/11 12:26 · sessao 96d571efac7b · TIPO-NA-FONTE · censo · origem=Code

Referências às linhas das transcrições fornecidas, contando linhas vazias e excluindo os separadores `== anexo... ==`. Em **(a)**, distingo descrever um comportamento de denunciá-lo como achado.

1. **(a) Codex não apontou o achado, com o tema presente.**  
   Pergunta: “Algum leitor trata LEGADO por fallback mudo [...] em vez de pelo nome?” (`preverif.q:6`). Resposta: o tripwire “Entrega `None`”, mas sem denunciar o fallback posterior por parâmetro; a denúncia ficou nos acessores e em `_orfas_da_ata` (`preverif.r:17,35`). A triagem registra esse terceiro resíduo somente na matriz posterior ao opus; os anteriores foram aceitos e curados, não descartados (`triagem_linhas.md:1,4`).

2. **(a) Codex não apontou a irregularidade, com o tema presente.**  
   Pergunta: “QUEM ESCREVE e QUEM LE” inclui expressamente `ata['orfas_instantes']`; também pergunta por “derivacao paralela de fonte unica” (`preverif.q:10,12`). Resposta: descreve o campo adicional e seus leitores em testes, mas não denuncia a proibição nem a onda de regravações; chega a registrar “ISO entra em chave adicional” (`preverif.r:80,93`). **A presença da proibição literal do aval `tipo_na_fonte_o168_teto` nos anexos efetivamente entregues ao Codex é [não verificado]**: o conteúdo desses avais não foi transcrito (`preverif.q:2`; `veredito.md:4`). [nao verificado]

3. **(a) Codex não apontou o achado, com o tema presente.**  
   Pergunta: “QUEM ESCREVE” e “2o escritor fora da porta” (`preverif.q:10,12`). Resposta: “O caminho visível de escrita continua passando por `gravar_orfas` e pela lavratura”; não identifica o escritor da grade nem sua exclusão do universo do censo (`preverif.r:102`). Na segunda resposta, “Segundo escritor produtivo fora da porta: Não identificado”, expressamente limitado ao delta (`preverif2.r:85,88`). A triagem nomeia o escritor da grade na matriz posterior ao opus (`triagem_linhas.md:1`).

4. **(a) Codex descreveu o comportamento, mas não apontou a perda do estado.**  
   Perguntas: preservação para “ata NULA”, tratamento pelo nome e comportamento do vigia para “ata None” (`preverif.q:5,6`; `preverif2.q:6`). Resposta: “Todos viram `{}`” e “`None` é normalizado antes da porta”, apresentados como preservação do resultado; não denuncia `NAO_LAVRADA` sendo classificada como `LEGADO_SEM_CHAVES` (`preverif2.r:28,32,33`). Não há descarte fundamentado desse achado na triagem anterior; ele aparece na matriz posterior (`triagem_linhas.md:1,4,6`).

5. **(c) Fora do alcance daquela verificação: o selo específico veio depois.**  
   Pergunta: a segunda volta é “so o delta” até `ed4b38773` e pergunta pelo selo `ChaveVelhaSoNaPorta` (`preverif2.q:1,2,6`). Resposta: examina esse selo de constantes literais e suas exclusões (`preverif2.r:45,47`). O selo AST `test_tipo_item3_selo_classe_fallback.py` é registrado posteriormente, após a segunda reprovação e sua cura; suas brechas constam do veredito posterior (`triagem_linhas.md:3,4`; `veredito.md:7`).

6. **(c) Fora do alcance: causa efetiva do RED.**  
   Pergunta: “qual teste fica vermelho em cada?” (`preverif.q:9`). Resposta: “Por inspeção das asserções; execução dos mutantes [não verificado]” (`preverif.r:65`). Isso não certifica que o RED ocorreu por valor; o `AttributeError` é informação do veredito fornecido (`veredito.md:8`). [nao verificado]

7. **(c) Fora do alcance, tomando o número agregado como certificação do pouso.**   [nao verificado]
   Perguntas: preservação por leitor e publicação do contador no tripwire (`preverif.q:5,7`). Respostas: DIFF na frota, suíte e mutantes permanecem “[não verificado]” (`preverif.r:3,109`; `preverif2.r:94`). A própria triagem determina “FORA DO ALCANCE — DIFF por leitor = a cadeia” (`triagem_linhas.md:6`). **Ressalva do agrupamento:** a exigência específica de publicação em `gerar_estado.py` não aparece nas perguntas transcritas; sua inclusão em anexos não transcritos é [não verificado] (`preverif.q:7`; `preverif2.q:5–9`). A divergência da árvore certificada é informação posterior do veredito (`veredito.md:9`). [nao verificado]

Não atribuo **(b)**: a triagem aceita e cura os bloqueantes anteriores; “não bloqueantes → backlog” não identifica descarte fundamentado destes achados específicos (`triagem_linhas.md:4,6`). Tampouco atribuo **(d)**: afirmar que ninguém da sessão viu anteriormente excederia o registro fornecido **[não verificado]**.

**Contagem, uma classe por número:** **(a) 4; (b) 0; (c) 3; (d) 0; (e) 0. Total: 7**, com a ressalva do item agregado 7 (`veredito.md:3–9`).

## 13/11 12:26 · sessao 4ca8c4e65a17 · - · - · origem=Ronald (tmux)

Censo dos leitores diretos de produção e semântica:

| Leitor | Uso / hipótese |
|---|---|
| `app/ferias/models.py:95` | `status_de_periodo` calcula direito − concedidos − vendidos; saldo ≤0 vira `concedido`, portanto saldo negativo recebe o mesmo status de saldo zerado (`:96-97`). |
| `app/ferias/models.py:217` | `dias_saldo` devolve direito − concedidos − vendidos sem clamp; concedidos e vendidos são gastos distintos. |
| `app/ferias/models.py:222` | `saldo_para_remarcar` devolve saldo + dias do agendamento; pressupõe que os dias desse agendamento estão debitados no campo concedidos, sem conferir soma/status/vínculo. |
| `app/ferias/models.py:231` | `pode_agendar` exige status armazenado `adquirido` e saldo positivo. |
| `app/ferias/models.py:235` e `:239` | `dias_vendaveis` corta em zero apenas o teto restante de venda; `pode_vender` exige adquirido, teto positivo e saldo positivo. Não corta `dias_saldo`. |
| `app/ferias/views.py:31` e `:60` | KPI/filtro `sem_agend` equiparam adquirido + concedidos=0 a “sem agendamento”; não consultam existência/soma dos agendamentos e não consideram vendidos nesse filtro. |
| `app/ferias/views.py:233-234` | `_validar_agendamento` compara pedido ao saldo armazenado; usa não cancelados para duplicata (`:235-243`), não para recalcular saldo. |
| `app/ferias/services.py:197-205` | `checar_ferias_com_abono` verifica teto de venda, gozo+abono ≤saldo; `saldo_depois` também fica sem clamp. |
| `app/ferias/views.py:368`, `:383`, `:416`, `:427`, `:486`, `:518` | Fotografias antes/depois de concedidos para auditoria em cancelar-admin, desfazer concluída e corrigir datas. |
| `app/ferias/views.py:478-480` e `:680-682` | Correção administrativa e remarcação comparam novos dias ao saldo+reserva antiga fornecido por `saldo_para_remarcar`. |
| `app/ferias/views.py:812-813` | Editar período impede direito<concedidos; **não inclui vendidos nessa guarda**, apesar de a fórmula do saldo subtrair os dois. |
| `app/ferias/views.py:950-953` | Venda verifica teto de venda e saldo restante, separadamente. |
| `app/ferias/services.py:489-490` | Aprovação de solicitação compara solic.dias ao saldo do primeiro período adquirido; excesso vira aviso, não materializa agendamento nesse ramo (`:483-493`). |
| `app/ferias/situacao.py:68-69` | `_saldo` delega integralmente a `dias_saldo`. |
| `app/ferias/situacao.py:178-183` | Negativo gera aviso de cadastro inconsistente; contudo o texto chama **concedidos+vendidos** de “mais dias gozados”, mistura terminológica. |
| `app/ferias/situacao.py:192-216` | Avisos de vencido/vencendo só consideram saldo>0. |
| `app/ferias/situacao.py:296-297` | `saldo_total` soma só saldos positivos e `periodos` devolve só PAs com saldo>0. Valores negativos são **omitidos da agregação/lista**, embora detectados no aviso. O docstring fala “períodos já adquiridos” (`:146`), mas implementação carrega todos os PAs (`:162-163`) e só filtra pelo saldo (`:296-297`), sem status ou aquisição. |
| `app/ferias/management/commands/pautas_de_ferias.py:73`, `:80`, `:96-97` | `_saldo` usa fórmula do modelo; vencendo exige >0; saldo<0 entra em cadastro inconsistente. Texto nomeia explicitamente “saldo negativo” (`:136-138`). |
| `app/ferias/management/commands/alertar_ferias_vencendo.py:36`, `:41` | Guarda o saldo cru nos alertas após sincronizar status (`:31-38`); push individual mostra o número como “dias a usar” (`:71-83`). |
| `app/core/views_ia.py:261-269` | `_periodo_do_colab` seleciona só saldos>0 e informa o número de dias; sem filtro por status. |
| `app/core/views_ia.py:492-503` | Defaults da IA escolhem PA mais antigo com saldo>0; duração padrão min(30,saldo); sem filtro por status nessa escolha. |
| `app/core/views_ia.py:532` | “Vender o resto” usa max(0,saldo−dias), clamp da **quantidade proposta para venda**, não do saldo. |
| `app/api/views_mensageria.py:835-841` | API `situacao-ferias` expõe `saldo_dias=saldo_total` e os períodos filtrados de `situacao_de_ferias`; portanto o total/lista omite negativos, mantendo avisos (`:846`). |
| `app/api/views_mensageria.py:954` e `:1103` | Payloads gerais/ficha de vencimento expõem saldo cru dos PAs selecionados como vencendo; não recalculam soma. |
| `app/relatorios/services.py:290` | PDF “sem agendamento” usa o mesmo adquirido+concedidos=0. |
| `app/relatorios/services.py:316` | PDF mostra saldo cru como string. |
| `app/relatorios/views.py:370` | CSV mostra saldo cru; universo anterior é filtrado pelo juiz `vencendo` (`:358-359`). |
| `app/templates/ferias/saldo.html:37`, `:41`, `:46` | Exibe concedidos e saldo cru, inclusive negativo; botão só com adquirido+saldo>0. |
| `app/templates/ferias/_secao_colab.html:49-51` | Nomeia concedidos “Gozo” com tooltip “já agendados/gozados”, saldo “Direito − Gozo − Vendidos”, vendidos separados; número do saldo cru. |
| `app/templates/ferias/_secao_colab.html:57`, `:67-68`, `:77` | `pode_agendar` controla formulário; input recebe saldo como valor/max. Mensagem “sem saldo” exige ==0, negativo cai no genérico “Indisponível”. |
| `app/templates/ferias/_tabela_ferias.html:27` | Saldo cru na listagem. |
| `app/templates/ferias/agendar.html:7`, `:22` | Saldo cru no texto e valor/max do input. |
| `app/templates/ferias/agendar_lote.html:40` | Saldo cru no lote. |
| `app/templates/relatorios/ferias_vencendo.html:48` | Saldo cru no texto e `data-value`. |

Os leitores de saldo usam o acumulador `dias_concedidos`, não uma soma derivada de agendamentos (`app/ferias/models.py:217`, `app/ferias/views.py:233-235`). A semântica pretendida de concedidos como gozo reservado/consumado separado de vendidos está explícita na tela (`app/templates/ferias/_secao_colab.html:49-51`), mas não é idêntica em todos os usos: o aviso soma vendido ao que chama “gozado” (`app/ferias/situacao.py:178-183`), a edição do direito ignora vendido na guarda (`app/ferias/views.py:812-813`) e o total da API exclui períodos negativos (`app/ferias/situacao.py:296-297`; `app/api/views_mensageria.py:835-841`).

Status:

- Catálogo de `AgendamentoFerias` contém apenas `agendado`, `em_gozo`, `concluido`, `cancelado`; “não cancelados” abrange os três primeiros (`app/ferias/models.py:293-298`). O juiz por datas produz exatamente agendado/em_gozo/concluido e deixa cancelamento humano de fora (`app/ferias/models.py:273-285`).
- O fracionamento já conta explicitamente os três não cancelados (`app/ferias/models.py:360-363`). Não há quinto status declarado nesse catálogo (`app/ferias/models.py:293-298`); presença de valores fora do catálogo na produção [nao verificado].
- `recusado` existe em **confirmacao**, não em status (`app/ferias/models.py:321-324`). Recusar pelo colaborador só grava confirmacao=recusado; não cancela nem devolve dias (`app/ferias/services.py:567-575`). Logo continua não cancelado e continua contando pela definição solicitada.
- “Agendamentos vivos” de `situacao_de_ferias` significa apenas agendado/em_gozo (`app/ferias/situacao.py:164-168`), enquanto “não cancelados” também inclui concluído (`app/ferias/models.py:293-298`); são universos distintos.

## 13/11 12:26 · sessao 035078559140 · - · - · origem=Ronald (tmux)

Censo só leitura dos escritores encontrados na cópia:

| Escritor | Campos / conta | Lê-altera-grava? | `atomic` | `select_for_update` do período |
|---|---|---|---|---|
| `registrar_gozo_historico` | `concedidos=min(direito, concedidos+ag.dias)`; `vendidos+=vendidos` | Sim, objeto recebido | Próprio | Não — corpo inteiro usa `alvo` recebido. `app/ferias/services.py:270`, `:277`, `:295`, `:299`, `:302` |
| `executar_agendamento` | `concedidos+=dias` após criar agendamento | Sim, objeto recebido | Próprio | Não; consulta duplicata por existência, sem lock. `app/ferias/services.py:306`, `:315`, `:316`, `:321`, `:324`, `:326` |
| `agendar_ferias_porta` | Chama anterior e depois `vendidos+=dias_vendidos` | Sim; `refresh_from_db` antes da venda | Próprio, cobrindo gozo e venda | Não; refresh não usa lock. `app/ferias/services.py:156`, `:157`, `:161`, `:162`, `:165` |
| `cancelar_agendamento_dp` | `concedidos-=vivo.dias` | Sim, linha relida | Próprio | Sim, helper trava agendamento e período. `app/ferias/services.py:379`, `:380`, `:381`, `:383`, `:364`, `:367` |
| `executar_cancelamento_admin` | `concedidos-=vivo.dias` | Sim, linha relida | Próprio | Sim, mesmo helper. `app/ferias/services.py:402`, `:406`, `:407`, `:408`, `:410`, `:364`, `:367` |
| `_trocar_janela` | `concedidos=concedidos-vivo.dias+novos_dias` | Sim, linha relida | Próprio | **Sim, já nesta cópia**. `app/ferias/services.py:433`, `:434`, `:436`, `:452`, `:364`, `:367` |
| `aprovar_solicitacao` | Cria agendamento e `concedidos+=solic.dias` | Sim | Não próprio; **sim no único chamador de produção encontrado**, `avaliar_solicitacao` | Não do período; view trava somente `SolicitacaoFerias`. `app/ferias/services.py:469`, `:483`, `:503`, `:504`, `:506`; `app/ferias/views.py:739`, `:740`, `:753` |
| `vender_dias_do_periodo` | `vendidos+=dias_v` | Sim, objeto recebido | Não próprio, nem no único chamador de produção encontrado | Não. `app/ferias/services.py:580`, `:582`, `:584`; `app/ferias/views.py:938`, `:942`, `:958` |

Inicialização: `garantir_periodo_aquisitivo` cria período com `defaults` do chamador; a view passa ambos os contadores zerados e envolve a criação de ciclos em `atomic`; não é RMW desses contadores nem lock explícito de período. Se já existe, a porta só atualiza status/carimbo. `app/ferias/services.py:254`, `:259`, `:261`, `:266`; `app/ferias/views.py:165`, `:181`, `:185`.

Migrations encontradas para os campos são de esquema/default: `CreateModel` com `dias_concedidos=0` e `AddField` de `dias_vendidos=0`; esta última preenche o campo novo, sem calcular soma. Não encontrei migration de dados que escreva esses contadores; ausência global fora da cópia é [nao verificado]. `app/ferias/migrations/0001_initial.py:39`, `:47`; `app/ferias/migrations/0004_periodoaquisitivo_dias_vendidos.py:12`, `:16`.

O `save` de `PeriodoAquisitivo` apenas executa guarda de sobreposição e delega ao Django; não recalcula os contadores pela soma. `app/ferias/models.py:201`.

O Django admin do app registra somente `Afastamento`; não há escritor desses contadores nele. Não encontrei signal ou command com atribuição desses campos na cópia; execução em produção é [nao verificado]. `app/ferias/admin.py:1`, `:4`; `app/ferias/apps.py:3`.

**Clamp e cancelamento:**

- O único clamp direto encontrado em `dias_concedidos` é o `min` do histórico. Exemplo aritmético inferido do código: histórico de 33 dias num período de direito 30 e concedidos zero salva agendamento com 33 e contador com 30, criando diferença de 3; ocorrência real é [nao verificado]. `app/ferias/services.py:281`, `:298`, `:299`.
- Cancelamento não tem clamp nem piso: subtrai os dias inteiros do agendamento. Se a linha travada tiver 30 concedidos e o agendamento cancelável tiver 33, grava `-3`; o código não compara contador com `vivo.dias`. `app/ferias/services.py:380`, `:381`, `:383`; `:407`, `:408`, `:410`.
- FERIAS-CANCELAR-2X já está incorporado: cancelado é recusado antes de obter o período; DP/admin comum também recusam concluído; admin especial com `permitir_concluido=True` recusa somente cancelado. `app/ferias/services.py:364`, `:365`, `:367`, `:380`, `:406`, `:407`; `app/ferias/models.py:302`, `:304`, `:305`.
- Vendidos não são devolvidos pelos cancelamentos, pois ambos salvam somente `dias_concedidos/status`. `app/ferias/services.py:383`, `:410`.
- Não encontrei `max(0, concedidos...)` ou outro clamp na escrita desses contadores. O `max(0,...)` existente em `dias_vendaveis` é propriedade de leitura do teto de abono. `app/ferias/models.py:234`, `:235`.

**Divergência documental importante:** BACKLOG ainda enumera `_trocar_janela` como sem trava, mas a implementação atual já trava ambos os registros dentro de `atomic`; também chama `aprovar_solicitacao` de “sem atomic”, embora a view atual forneça transação e lock da solicitação. A porta continua sem transação própria e sem lock do período. `app/docs/BACKLOG.md:1849`; `app/ferias/services.py:433`, `:434`, `:367`, `:469`; `app/ferias/views.py:739`, `:740`, `:753`.

As chamadas reais do gozo histórico, agendamento individual/lote, cancelamento DP/admin e venda são delegações para essas portas, sem segundo escritor dos contadores nas views. `app/ferias/views.py:211`, `:264`, `:299`, `:330`, `:370`, `:418`, `:531`, `:575`, `:636`, `:753`, `:958`.

## 13/11 12:26 · sessao 4283216c4ef4 · - · - · origem=Ronald (tmux)

5. **Há infraestrutura de auditoria reutilizável; não há trilha de realinhamento de `PeriodoAquisitivo` identificada na porta atual.** `LogAuditoria` comporta modelo, objeto_id, descrição/motivo, JSON antes/depois, usuário, ator de sistema e data; `core.observ.evento(...)` é a porta que retorna o registro criado ou `None` em falha. [app/core/models.py:285; app/core/models.py:295; app/core/models.py:300; app/core/observ.py:94; app/core/observ.py:133; app/core/observ.py:139]

- **Service/comando sem request:** pode chamar `evento(acao, modelo, objeto_id, descricao=..., antes=..., depois=..., ator=...)`; o processo `manage.py` recebe ator `sistema:<comando>`. A escrita da trilha usa savepoint próprio e é **best effort**: falhar não desfaz automaticamente o negócio. [app/core/observ.py:22; app/core/observ.py:94; app/core/observ.py:105; app/core/observ.py:117; app/core/observ.py:133]
- **Adaptador para view:** `registrar_log(request, ..., valor_antes, valor_depois)` extrai IP/usuário e chama `evento`; captura exceção e não devolve a linha criada. [app/core/auditoria.py:8; app/core/auditoria.py:17; app/core/auditoria.py:22; app/core/auditoria.py:25]
- **Molde existente dentro de férias:** cancelamento admin registra status/dias/datas/concedidos antes e status/concedidos/ausências rejeitadas depois; correção de concluídas acrescenta justificativa; correção de datas também registra datas/dias/status/concedidos antes/depois e justificativa. Os três logs são feitos **na view, após a porta e refresh**, com a fotografia de “antes” coletada antes de entrar no escritor. [app/ferias/views.py:366; app/ferias/views.py:370; app/ferias/views.py:376; app/ferias/views.py:378; app/ferias/views.py:414; app/ferias/views.py:418; app/ferias/views.py:423; app/ferias/views.py:484; app/ferias/views.py:503; app/ferias/views.py:511]
- `_trocar_janela` recebe somente agendamento, início, fim e quantidade; seu bloco atual altera agendamento/período e retorna, sem chamar a auditoria nesse bloco. Portanto não há motivo/autor explícitos na assinatura dessa porta. [app/ferias/services.py:422; app/ferias/services.py:433; app/ferias/services.py:450; app/ferias/services.py:452; app/ferias/services.py:454]

**Molde mais próximo de DRY/apply com reversão em `logs/`:** `ponto/management/commands/rederivar_minutos_h06_comp11.py`, vizinho de férias, combina todos os elementos pedidos; a classificação “mais próximo” é comparação do código examinado. [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:126; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:195; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:215]

- DRY sem flag; `--apply`; `--reverter SNAPSHOT`; `--por` obrigatório para escrever. [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:196; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:199; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:205]
- Por linha, abre atomic, relê com `select_for_update`, verifica identidade/valor esperado; DRY retorna `mudaria` sem chamar escritor; apply fotografa antes e chama o escritor existente. [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:149; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:163; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:165; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169]
- Exige trilha com antes/depois/motivo; se `evento` retorna `None`, lança exceção e desfaz a transação da linha. [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:181; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:186; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:191]
- Snapshot `logs/h12h06b/snapshot_<ato>_<timestamp>.jsonl`, com flush/fsync; confirmação posterior ao commit identifica linhas efetivamente aplicadas; reversão confere identidade, valor e marcas de atos posteriores. Há resultado JSON, hash do snapshot, listagem e contadores. [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:215; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:220; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:248; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:253; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:261; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:273; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:282; app/ponto/management/commands/rederivar_minutos_h06_comp11.py:288]
- No próprio férias, `sincronizar_status_ferias` oferece DRY/apply, lista/contador e apply pela porta; não tem snapshot/reversão nesse comando. `pautas_de_ferias` também oferece DRY/apply, mas produz pautas, não realinha contadores. [app/ferias/management/commands/sincronizar_status_ferias.py:20; app/ferias/management/commands/sincronizar_status_ferias.py:37; app/ferias/management/commands/sincronizar_status_ferias.py:44; app/ferias/management/commands/sincronizar_status_ferias.py:55; app/ferias/management/commands/pautas_de_ferias.py:39; app/ferias/management/commands/pautas_de_ferias.py:82; app/ferias/management/commands/pautas_de_ferias.py:91]

6. **A entidade de `#103` e `#114` citados no aval é [nao verificado].** O texto fornecido não qualifica os números e não contém fotografia de linhas dessas entidades. [nao verificado]

- Busca exata no repositório por `#103`/`#114` com fronteira numérica, `HJ-090`, `FERIAS-UM-JUIZ`, `complemento_hj_090` e `concedidos_x_soma`: somente `#103` aparece, em contexto de **disputas**, junto de `#1257`; o trecho anterior identifica “disputa 2336” e perguntas vivas. Essa ocorrência não vincula o `#103` ao HJ-090 nem prova que seja o mesmo número do aval. [app/docs/RELATO-ARQUIVO.md:2834; app/docs/RELATO-ARQUIVO.md:2837]
- Não localizei o arquivo `complemento_hj_090_1221.txt`, `HJ-090`, `#114` exato nem referência dos dois números no app férias que estabeleça a entidade; ausência de correspondência positiva permanece [nao verificado].
- A referência documental pertinente encontrada é a fila **FERIAS-SALDO-SEM-TRAVA**, que enumera escritores e a ausência de locks/atomic no estado então documentado; não identifica `#103/#114`. [app/docs/BACKLOG.md:1849]
- Não apareceram `AGENTS.md` na árvore pesquisada nem nos ancestrais `/`, `/home`, `/home/ronald`, `/home/ronald/codex_ws` e raiz da cópia; existência de instruções adicionais fora desse alcance [nao verificado].

## 13/11 12:24 · sessao e5674ab8c689 · HJ-090 · sonda · origem=Code

Execução local: [nao verificado]. Transcrição integral da saída fornecida [nao verificado]:

```text
sonda_leitura: sonda1.py [556f5fa16dc5c31a] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

periodos 1049
divergentes ferias_concedidos_x_soma 3
por empresa {'emp4': 1, 'emp3': 1, 'emp2': 1}
por status {'concedido': 3}
saldo negativo apos realinhar 2
negativos HOJE (concedidos+vendidos>direito) 0
pk_periodo colab empresa status direito concedidos soma_nao_cancelados vendidos delta
15 31 4 concedido 30 30 20 0 -10
598 127 3 concedido 30 30 31 0 1
639 344 2 concedido 30 30 33 0 3
ERROS 0
```

Total de divergentes: 3 [nao verificado]  
Saldo negativo depois do realinhamento: 2 [nao verificado]  
ERROS = 0: sim [nao verificado]

## 13/11 12:17 · sessao 1f97972bb4dd · UI-RESCISAO · censo · origem=Code

**(A) QUADRO porta por porta**

Referências à cópia disponível. Correspondência Git com o HASH informado, execução em produção e estado de outras cópias: **[nao verificado]**.

| Item | Existe / falta / diverge | Sítio(s) arquivo:linha | Observação |
|---|---|---|---|
| **1. Espelho 21 → desligamento** | **EXISTE** emissão livre; **FALTA** a combinação livre + registro auditável | `app/relatorios/services.py:165`, `app/relatorios/services.py:211`, `app/relatorios/services.py:235`, `app/relatorios/services.py:250` | Informar início/fim faz o lote ir diretamente ao PDF informacional. A referência do anexo corresponde, nesta cópia, a **relatorios/services.py**. Devolve PDF agregado, pulados, quantidade emitida, informacionais e sem movimento (`app/relatorios/services.py:260`). |
| **1. Datas e universo** | **EXISTE**, com limites | `app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:602`; `app/relatorios/services.py:102`, `app/relatorios/services.py:217` | Dias **inclusivos** nas duas pontas; coletor aplica teto em ontem. O lote usa colaboradores explicitamente fornecidos ou seleção da empresa, cujo padrão é ativo/afastado/férias: desligado não entra automaticamente. Avulso recusa fim hoje/futuro e diferença superior a 180 dias (`app/relatorios/views.py:1083`, `app/relatorios/views.py:1087`). Leitores/chamadores: avulso e job do lote (`app/relatorios/views.py:1065`, `app/relatorios/lote_espelho.py:205`). |
| **1. Espelho auditável** | **EXISTE**, por competência | `app/relatorios/pdf_espelho.py:1074`, `app/relatorios/pdf_espelho.py:1095`, `app/relatorios/services.py:49` | Recebe **mês/ano**, devolve bytes + `RegistroPDFAuditavel`; wrapper devolve também followup. Não recebe período arbitrário. A janela respeita o corte da empresa, não necessariamente 21 (`app/relatorios/pdf_espelho.py:654`, `app/ponto/janelas.py:54`). Bloqueio explícito: disputa aberta do colaborador, sem recorte temporal (`app/relatorios/pdf_espelho.py:1084`). Exigência de competência fechada nesse corpo: **[nao verificado]**. |
| **2. DiaPago — escritores** | **EXISTE** | `app/ponto/models.py:293`; `app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:405`, `app/ponto/services/dia_pago.py:592` | Unidade: dia da entrada do turno, mais linha de **ajuste sem data**; versões motor/oráculo; rubricas em horas e campos de grade em minutos (`app/ponto/models.py:303`, `app/ponto/models.py:325`). `lavrar` substitui as linhas do colaborador/competência/versão (`app/ponto/services/dia_pago.py:359`); fechamento chama motor e oráculo (`app/ponto/services/fechamento.py:1191`, `app/ponto/services/fechamento.py:1265`). `retratar_exportada` escreve o retrato da exportada; `apurar_furo_da_exportada` atualiza furo/carimbo nas linhas existentes (`app/ponto/services/dia_pago.py:538`, `app/ponto/services/dia_pago.py:671`). |
| **2. DiaPago — leitura até demissão** | **EXISTE** | `app/ponto/services/dia_pago.py:752`, `app/ponto/services/dia_pago.py:833`, `app/ponto/services/dia_pago.py:855` | `linhas_do_periodo(colab,ini,fim)` devolve `{date: DiaPago}`; `soma_do_periodo` devolve somas por colaborador e conjuntos `sem_lavratura`/`sem_apuracao`. Ambas incluem **ini e fim**, apenas `tipo=dia`, padrão motor. Pode receber demissão como fim; não a aplica automaticamente. **Exclui DSR/banco**, pertencentes ao ajuste da competência, sem rateio (`app/ponto/services/dia_pago.py:755`). A soma da competência inclui ajuste (`app/ponto/services/dia_pago.py:550`). |
| **3. Ausências no período** | **EXISTE**, com universos explícitos | `app/ponto/turnos.py:3142`, `app/ponto/turnos.py:2511`, `app/ponto/turnos.py:2523`, `app/ponto/turnos.py:2735` | `ausencias_vigentes` devolve lançamentos sobrepostos, **todos os status**, inclusive nas bordas; ausência sem fim depende dos tipos declarados abertos (`app/ponto/turnos.py:3158`). `ausencias_do_periodo(...,para='folha')` lê aprovadas; `para='cobranca'`, não rejeitadas. `cobertura_ausencia_periodo` devolve mapa por dia, recortado inclusivamente; empate vence maior `(data,pk)` (`app/ponto/turnos.py:2753`). Decisão histórica, inclusive rejeição, tem outra porta: `decididos_do_periodo` (`app/ponto/services/dia_decidido.py:529`). |
| **4. Férias — cálculo do aquisitivo** | **EXISTE / DIVERGE** | `app/ferias/views.py:139`, `app/ferias/views.py:161`; `app/ferias/services.py:523` | Criação ancora na admissão: aniversário até aniversário seguinte **menos um dia**, inclusivos; concessão até fim + um ano. Cria ciclos até **hoje**, não até demissão; trata 29/2 (`app/ferias/views.py:143`, `app/ferias/views.py:169`). **Edição usa início + 364 dias e concessão + 365 dias**, podendo produzir bordas diferentes em ano bissexto (`app/ferias/services.py:528`). |
| **4. Férias — saldo, vencidas e proporcionais** | **EXISTEM** saldo/status; **DIVERGEM** alguns leitores; proporcionais **FALTAM na busca [nao verificado]** | `app/ferias/models.py:65`, `app/ferias/models.py:141`, `app/ferias/models.py:216`; `app/ferias/services.py:111`; `app/relatorios/services.py:285` | Saldo em **dias** = direito − concedido − vendido, sem proporcionalidade temporal. Juiz aceita data de referência: saldo ≤ 0 é concedido; último dia aquisitivo ainda em curso; vencido somente quando limite **< referência** (`app/ferias/models.py:94`). Faltas contam **registros** aprovados de tipo literal `falta`, com início dentro do aquisitivo, inclusivo; faixas 30/24/18/12/0 (`app/ferias/models.py:52`, `app/ferias/models.py:144`). `periodos_vencendo` e PDF gestão leem status armazenado; inteligência lê juiz, podendo divergir (`app/inteligencia/views.py:13`). Fórmula de avos/proporcionais rescisórios não localizada **[nao verificado]**. |
| **5. informativo_rescisao** | **FALTA nesta busca [nao verificado]** | `app/folha/fora_da_folha.py:95`, `app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:229`; `app/folha/export.py:1196` | Não foram localizados `informativo_rescisao` nem `esta_desligado` **[nao verificado]**. O retorno concreto de `o_que_segura` contém dias de **retenção**, motivo, fio e destrava. O retentor recorta admissão/demissão, mas não apresenta isenção global de desligado nesse corpo; célula acusando ainda pode reter (`app/folha/export.py:1206`, `app/folha/export.py:1241`, `app/folha/export.py:1261`). A construção descrita em outra cópia pelo anexo não foi inspecionada **[nao verificado]**. |
| **6. RegistroPDFAuditavel — escrita** | **EXISTE** trilha; **FALTA arquivo no modelo** | `app/relatorios/models.py:14`, `app/relatorios/models.py:30`; `app/relatorios/pdf_base.py:152` | Uma linha por emissão auditável: hash, tipo, autor/instante, colaborador, filtros, quantidade, IP, commit e host. **Não guarda bytes nem FileField**, nem início/fim normalizados (`app/relatorios/models.py:43`, `app/relatorios/models.py:50`). Escritor central cria registro e devolve bytes + registro (`app/relatorios/pdf_base.py:180`). |
| **6. Registro — leitura para dossiê** | Leitura por hash **EXISTE**; porta colaborador/período **FALTA na busca [nao verificado]** | `app/chamados/views.py:1185`, `app/chamados/views.py:1210`; `app/relatorios/services.py:75`, `app/relatorios/views.py:1020` | Verificação pública lê hash completo ou prefixo ≥16 caracteres, devolvendo autenticação/metadados. Espelho pode guardar o PDF **fora do registro**, em `FollowupChamado.anexo_pdf`; download é por **followup_id**, não colaborador/período. Não foi localizada porta pronta que monte dossiê histórico por colaborador/período **[nao verificado]**. |
| **7. rescisao_modulo_proprio** | **EXISTE** | `app/folha/export.py:866`; `app/folha/views.py:594`; `app/folha/retrato_folha.py:33`; `app/ponto/services/fechamento_lista.py:25`; `app/relatorios/views.py:1659`; `mensageria/nucleo/ferramentas.py:1196` | Produtor devolve item `status='fora'`, motivo e detalhe de verbas pelo módulo próprio do Domínio. Raio-X separa rescisões; retrato coloca em **modulo_proprio**, excluindo do bloqueado; fechamento traduz para `rescisao`; relatório traduz e marca fora da folha; mensageria separa como **ressalva**, sem somar ao que trava (`app/folha/retrato_folha.py:89`, `app/relatorios/views.py:1669`, `mensageria/nucleo/ferramentas.py:1246`, `mensageria/nucleo/ferramentas.py:1289`). |
| **8. H12 — sai pela rescisão** | **EXISTE, já com corte pelo fim nesta cópia** | `app/folha/export.py:54`, `app/folha/export.py:60`, `app/folha/export.py:815`, `app/folha/export.py:866`; `app/ponto/services/fechamento_lista.py:51`, `app/ponto/services/fechamento_lista.py:109` | Predicado booleano: demissão não nula e **demissão ≤ fim da janela**, sem limite inferior. Depois do fim não sai pela rescisão daquela competência. Consumidores diretos encontrados: classificador e resumo da lista; contador lê a linha (`app/ponto/views.py:1215`). Universo do classificador: fechamentos fornecidos ou empresa/mês/ano, não todos os desligados históricos (`app/folha/export.py:782`). |
| **9. Julgar/agendar desligamento** | **EXISTE**; mensagem da UI **DIVERGE** | `app/colaboradores/urls.py:52`; `app/colaboradores/views.py:1422`, `app/colaboradores/views.py:1433`; `app/colaboradores/services/desligamento.py:15`, `app/colaboradores/services/desligamento.py:50` | Juiz por colaborador/ato devolve erro ou data/motivo/situação anterior/agendado. Recusa já desligado, data inválida, motivo vazio e data **> hoje+90**. Futuro estritamente **> hoje**, até o teto inclusive, agenda (`app/colaboradores/services/desligamento.py:18`, `app/colaboradores/services/desligamento.py:41`, `app/colaboradores/services/desligamento.py:58`). Agendado só grava intenção/histórico e retorna; view e confirmação dizem bloqueio imediato mesmo assim (`app/colaboradores/services/desligamento.py:87`, `app/colaboradores/views.py:1438`, `app/templates/colaboradores/editar.html:272`). |
| **9. Executor efetivo** | **EXISTE**; encerramento total da O50 ainda **FALTA** nos casos indicados | `app/colaboradores/services/desligamento.py:108`, `app/colaboradores/services/desligamento.py:127`, `app/colaboradores/services/desligamento.py:144`, `app/colaboradores/services/desligamento.py:168` | Grava situação/data/motivo, bloqueia credencial, registra histórico, fecha vínculos pela porta, percorre disputas/chamados, cria `desligamento_revisao`, notifica DP e audita. **Pula disputa com resposta sem veredito** e pode manter chamado cuja transição foi recusada (`app/colaboradores/services/desligamento.py:145`, `app/colaboradores/services/desligamento.py:160`). Contexto do novo chamado não lista dia/pergunta/resposta; contém data/motivo/ator/instante (`app/colaboradores/services/desligamento.py:182`). |
| **9. Vínculos, células e cron** | Portas **EXISTEM**; alcance total **FALTA** | `app/colaboradores/services/vinculo.py:855`, `app/colaboradores/services/vinculo.py:622`, `app/colaboradores/services/vinculo.py:641`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38` | Wrapper oferece só vínculos `ativa=True`; fecha início **≤ demissão**, recusa futuros e devolve quantidade, descartando lista de recusados (`app/colaboradores/services/vinculo.py:862`). Regeneração alcança células geradas já existentes, amanhã inclusive em diante, dentro do horizonte padrão (`app/ponto/services/cadastro_futuro.py:26`, `app/ponto/services/cadastro_futuro.py:31`). Cron seleciona demissão **≤ hoje**, situação diferente de desligado, e reutiliza juiz/executor; DRY padrão, `--apply` efetiva (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:46`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`). Agenda declarada às 06:06; execução real **[nao verificado]** (`app/config/crons.py:440`). |
| **10. Telas, ficha corrida e F2** | Superfícies parciais **EXISTEM**; dossiê único **FALTA na busca [nao verificado]** | `app/templates/colaboradores/editar.html:268`; `app/colaboradores/services/ficha.py:99`; `app/docs/ADVERTENCIA-FICHA.md:65`; `app/docs/BACKLOG.md:1285` | Cadastro mostra desligar/data/motivo/reativar (`app/templates/colaboradores/editar.html:281`). Ficha de serviço devolve vínculo, trocas, horas e pendências da competência atual; não é dossiê de PDFs (`app/colaboradores/services/ficha.py:119`, `app/colaboradores/services/ficha.py:233`). Documento pede quinta aba Ficha; drawer atual contém Dados/Espelho/Mensagens/Férias (`app/templates/colaboradores/partials/painel_colaborador.html:50`, `app/templates/colaboradores/partials/painel_colaborador.html:217`). F2 registrada é visão de faltas/ausências decididas **pelo aquisitivo**, na mesma ficha (`app/docs/BACKLOG.md:1285`). |

Complementos de leitores e diferenças de universo:

- **DiaPago:** espelho/PDF, calendário, app, extrato parcial, cartão, conferência da exportação, gestão de HE e autorização de HE leem essa lavratura (`app/ponto/services/espelho.py:406`, `app/relatorios/pdf_espelho.py:835`, `app/colaboradores/services/calendario.py:290`, `app/api/views.py:1439`, `app/relatorios/views.py:482`, `app/relatorios/cartao_pela_celula.py:142`, `app/folha/porta_export.py:267`, `app/ponto/services/gestao_he.py:234`, `app/ponto/services/autorizacao_he_periodo.py:172`).
- **Ausências:** tela e API leem lançamentos; fechamento/efeito/espelho/escala leem cobertura; histórico do drawer lê a linha do tempo (`app/ponto/views.py:2178`, `app/api/views_mensageria.py:703`, `app/ponto/services/fechamento.py:638`, `app/ponto/services/efeito_ausencia.py:28`, `app/ponto/services/espelho.py:579`, `app/escala/utils.py:1174`, `app/ponto/views.py:2014`). A linha do tempo contém lançamentos raiz de todos os status e não recebe recorte temporal nativo; o leitor de lote aplica o recorte depois (`app/ponto/services/dia_decidido.py:566`, `app/ponto/services/atestados_lote.py:46`).
- **Férias:** saldo/seção, situação de férias, inteligência, relatório e cron consomem períodos/saldos (`app/ferias/views.py:110`, `app/ferias/views.py:767`, `app/ferias/situacao.py:162`, `app/inteligencia/views.py:13`, `app/relatorios/services.py:268`, `app/ferias/management/commands/alertar_ferias_vencendo.py:22`). O histórico do drawer escolhe período contendo **hoje**, ou último iniciado; não recorta a linha do tempo nesse aquisitivo (`app/ponto/views.py:2015`, `app/ponto/views.py:2022`).
- **RegistroPDFAuditavel:** escritores indiretos do gerador central são espelho, histórico de chamados, gestão de férias, gestão de HE e retrato da folha (`app/relatorios/pdf_espelho.py:1103`, `app/relatorios/pdf_chamados.py:158`, `app/relatorios/services.py:338`, `app/ponto/views.py:867`, `app/folha/views_retrato.py:436`). Existe ainda atualização operacional de nome pelo mascarador de sombra, fora da emissão produtiva (`bin/sombra_mascara.py:87`).
- **H12 — outros usos de “rescisão”:** prontidão rotula denominador zero com demissão **antes do início**, estritamente `<`; isso responde prontidão, não saída do TXT (`app/folha/export.py:1051`, `app/folha/export.py:1068`). Raio-X e retentor cortam o fim na demissão; no retentor, o dia da demissão fica dispensado apenas quando falta célula/veredito, não quando a célula acusa (`app/folha/views.py:612`, `app/folha/export.py:1208`, `app/folha/export.py:1237`, `app/folha/export.py:1263`).
- **Outras apresentações:** fechamento mostra filtro/badge/data de rescisão; folha/exportação avisam sobre módulo próprio; fio mostra situação/motivo (`app/templates/ponto/fechamento.html:65`, `app/templates/ponto/_fech_linha.html:4`, `app/templates/folha/index.html:57`, `app/templates/folha/exportar.html:16`, `app/templates/chamados/fio_colaborador.html:70`). O histórico de chamados é outro universo: eventos e ausências **criadas** desde `agora−dias`, não aquisitivo nem dossiê PDF (`app/chamados/services/historico.py:118`, `app/chamados/services/historico.py:129`).
- **Ato a reutilizar:** a futura tela deve seguir a composição já usada por view e cron: `julgar_desligamento` → `executar_desligamento(...,hoje=...)`; cadastro, credenciais, vínculos e fios permanecem nos serviços chamados por ela (`app/colaboradores/views.py:1433`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`, `app/colaboradores/services/desligamento.py:115`, `app/colaboradores/services/desligamento.py:127`, `app/colaboradores/services/desligamento.py:147`).

**(B) ARQUIVOS EM COMUM COM O50-DESLIGAR-ENCERRA-TUDO**

Interseção das portas e dos leitores encontrados; quais arquivos serão efetivamente editados por cada construção futura: **[nao verificado]**.

| Fatia O50 | Arquivos compartilhados no circuito |
|---|---|
| **(a) Vínculos e células** | `app/colaboradores/services/desligamento.py:127`; `app/colaboradores/services/vinculo.py:622`, `app/colaboradores/services/vinculo.py:855`; `app/ponto/services/cadastro_futuro.py:9`; porta da célula chamada em `app/ponto/services/cadastro_futuro.py:22`, `app/ponto/services/cadastro_futuro.py:44`. |
| **(b) Leitores crus — desligamento/ficha** | `app/colaboradores/views.py:1417`; `app/colaboradores/services/desligamento.py:18`; `app/colaboradores/services/vinculo.py:863`; `app/colaboradores/services/drawer.py:23`; `app/colaboradores/services/ficha.py:234`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`. |
| **(b) Leitores crus — folha/ponto/calendário** | `app/folha/export.py:807`, `app/folha/export.py:810`, `app/folha/export.py:1208`; `app/folha/views.py:612`; `app/ponto/services/fechamento.py:432`; `app/ponto/services/fechamento_lista.py:60`; `app/ponto/turnos.py:2464`; `app/ponto/views.py:2061`; `app/colaboradores/services/calendario.py:258`. |
| **(b) Leitores crus — relatórios/férias/app/templates** | `app/relatorios/services.py:105`; `app/relatorios/views.py:455`, `app/relatorios/views.py:492`; `app/ferias/views.py:605`; `app/ferias/situacao.py:172`; `app/api/views.py:116`, `app/api/views.py:312`; `app/templates/colaboradores/editar.html:211`; `app/templates/colaboradores/partials/painel_colaborador.html:163`. |
| **(c) Selo** | Selo existente de vínculo de desligado exercita o ato normal: `app/colaboradores/tests/test_selo_bug95_vinculo_de_desligado.py:64`. Há selo AST da C5 contra derivação própria: `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:288`. Arquivo do selo AST + mutante específico da O50 e compartilhamento com futura UI: **[nao verificado]**. |
| **(d) Disputas, perguntas e chamados** | `app/colaboradores/services/desligamento.py:144`, `app/colaboradores/services/desligamento.py:160`, `app/colaboradores/services/desligamento.py:182`; `app/chamados/models.py:1618`, `app/chamados/models.py:1887`, `app/chamados/models.py:1919`; `app/chamados/signals.py:20`, `app/chamados/signals.py:43`; cron em `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`. Precedente de competência trancada: `app/chamados/services/competencia_trancada.py:119`. |

**(C) perguntas de lei: 3**

1. **Férias proporcionais no desligamento:** avos, frações e efeito do aviso sobre o direito proporcional; saldo existente é integral, fórmula rescisória não localizada (`app/ferias/models.py:216`; **[nao verificado]**).
2. **Aviso prévio na liquidação:** modalidade trabalhado/indenizado e projeção sobre datas e direitos; porta atual recebe data/motivo e distingue agendamento, sem representar essas modalidades (`app/colaboradores/services/desligamento.py:50`, `app/colaboradores/services/desligamento.py:87`; regra de projeção **[nao verificado]**).
3. **Liquidação dos ajustes da competência parcial:** destino de DSR e banco no acerto até a demissão; leitura de intervalo explicitamente os exclui, sem regra rescisória localizada (`app/ponto/services/dia_pago.py:755`; **[nao verificado]**).

## 13/11 12:13 · sessao 921327d13a14 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo relativos à raiz da cópia.

**9 — EXISTE a porta de ato; DIVERGE a informação da UI para agendado.**

- Rota `POST <cid>/desligar/`: `app/colaboradores/urls.py:52`; exige login, POST e ação `editar_cadastro`, com gate de disputa isento: `app/colaboradores/views.py:1422`. A view consulta `julgar_desligamento`, devolve erro ou chama `executar_desligamento(..., hoje=timezone.localdate())`: `app/colaboradores/views.py:1433`.
- Juiz por **um colaborador/ato**, devolve dict `erro`, `data_demissao`, `motivo`, `situacao_antes`, `agendado`; recusa já desligado, data inválida, data maior que hoje+90 dias e motivo vazio: `app/colaboradores/services/desligamento.py:15`, `:40`, `:47`, `:50`. Futuro **estritamente > hoje**, até hoje+90 inclusive, vira agendado: `:41`, `:58`.
- Agendado grava apenas data/motivo, evento em `HistoricoVinculo` datado da demissão e observabilidade; retorna `None`, preservando situação/acesso/vínculo/chamados: `app/colaboradores/services/desligamento.py:87`. O histórico usa a porta `registrar_evento_de_vinculo`: `app/colaboradores/services/historico_vinculo.py:20`.
- Efetivo, em transação: grava desligado/data/motivo (`:108`), bloqueia credencial pela porta (`:115`), registra histórico (`:118`), fecha vínculos via `fechar_vinculos_ativos` (`:127`), percorre disputas e chamados (`:144`, `:155`), cria chamado DP/RH `auditoria_clt`, `desligamento_revisao`, com contexto de data/motivo/ator/instante (`:168`), dispara push (`:192`); auditoria geral após o bloco transacional (`:201`). Todos em `app/colaboradores/services/desligamento.py`.
- Não fecha todas as disputas: **pula resposta sem veredito** em `app/colaboradores/services/desligamento.py:145`. As demais passam por `DisputaSupervisao.fechar(..., motivo=FECHOU_DESLIGAMENTO)`: `:147`. A própria porta recusa resposta sem veredito (`app/chamados/models.py:1887`), encerra perguntas mudas por `fechar_por_admin` com trilha (`:1919`) e fecha disputa (`:1933`). O signal pede reconciliação ao chamado pai pela porta: `app/chamados/signals.py:20`, `:43`.
- Chamado com pergunta viva pode recusar `encerrar` e permanecer: `app/colaboradores/services/desligamento.py:160`; guarda efetiva em `app/chamados/models.py:1618`. **O50(d) ainda não está realizado nessa cópia**, apesar do aval raspado: não há fechamento da resposta sem veredito nem lista dia/pergunta/resposta no contexto do novo chamado, que contém apenas data/motivo/ator/instante (`app/colaboradores/services/desligamento.py:145`, `:182`).
- Vínculos: `fechar_vinculos_ativos` só oferece `ativa=True` à porta (`app/colaboradores/services/vinculo.py:855`, `:863`); `fechar_vigencia` fecha somente `data_inicio <= demissao`, inclusive, e devolve `(fechados, recusados_pks)` (`:622`, `:639`, `:641`, `:671`); o wrapper descarta recusados e devolve **número** (`:862`). Futuro fica ativo e logado como recusa (`:665`), portanto **O50(a) ainda falta para futuro** (`:858`). Existe outra porta `absorver_vigencias_posteriores` que desativa futuros sem inventar `data_fim`, mas o executor de desligamento não a chama (`:756`, `:788`, `:799`; `app/colaboradores/services/desligamento.py:127`).
- Fechamento de vigência alcança células pela porta `regenerar_futuras_do_cadastro`, somente após o menor fim antigo/novo (`app/colaboradores/services/vinculo.py:657`). Esse suplemento limita-se a células **já existentes**, `origem='gerada'`, janela padrão, de **amanhã inclusive** em diante; chama `ponto.portas.celula.regenerar_celulas_vinculo` (`app/ponto/services/cadastro_futuro.py:26`, `:31`, `:44`). Não equivale a encerrar todo vínculo futuro nem comprova remoção de toda célula após demissão (`app/colaboradores/services/vinculo.py:858`; efeito total no passivo [nao verificado]).
- Cron usa os mesmos juiz/executor, para `data_demissao <= hoje` inclusive e situação diferente de desligado, DRY por padrão e `--apply` para efetivar: `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:22`, `:38`, `:46`, `:51`, `:57`. Agenda escrita às 06:06 com `--apply`: `app/config/crons.py:440`; execução real [nao verificado].
- **DIVERGE apresentação:** view sempre responde “desligado; acesso bloqueado” (`app/colaboradores/views.py:1438`) e confirmação do formulário sempre diz bloqueio imediato (`app/templates/colaboradores/editar.html:272`), mesmo quando executor só agenda (`app/colaboradores/services/desligamento.py:87`).
- Tela futura deveria reutilizar **`julgar_desligamento` + `executar_desligamento`**, seguindo o precedente view/cron, e seus serviços chamados, em vez de duplicar escrita: `app/colaboradores/views.py:1433`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`. A porta atual não é calculadora de verbas: recebe data/motivo e executa cadastro/acesso/vínculo/fio (`app/colaboradores/services/desligamento.py:50`, `:108`).

**10 — EXISTEM superfícies parciais; FALTA dossiê único de liquidação comprovado.**

- Cadastro editar mostra formulário de desligamento; desligado mostra data e motivo, e reativação: `app/templates/colaboradores/editar.html:268`, `:281`, `:285`; rotas editar/desligar/reativar em `app/colaboradores/urls.py:50`.
- Drawer do colaborador exibe badge desligado (`app/templates/colaboradores/partials/painel_colaborador.html:90`) e botão reativar (`:163`); contém Dados/Espelho/Mensagens/Férias, sem quinta aba Ficha nessa cópia (`:50`, `:181`, `:188`, `:217`). Documento pede **quinta aba Ficha**, extrato com filtros e PDF, mas é especificação (`app/docs/ADVERTENCIA-FICHA.md:65`, `:77`, `:84`).
- `ficha_do_colab` existe como dict de um colaborador, com vínculo/trocas/horas/pendências; janela é competência atual da empresa, datas da troca inclusivas (`app/colaboradores/services/ficha.py:99`, `:119`, `:132`, `:233`). Horas vêm de `espelho_do_colab`, com fonte dos totais (`:198`, `:209`), não de dossiê `RegistroPDFAuditavel`. Leitores são cabeçalhos e copiloto (`:265`, `:273`); API `/mensageria/ficha-do-colab/` em `app/api/urls.py:27`.
- Cuidado com homônimo: `chamados.services.historico.historico_do_colab` junta eventos de chamados e ausências **criadas** desde `agora - dias`, não período aquisitivo nem dossiê PDF (`app/chamados/services/historico.py:118`, `:126`, `:129`). API o consome em `app/api/views_mensageria.py:2191`.
- Histórico do drawer de ausência **existe**: `_historico_do_colab` consome `dia_decidido.linha_do_tempo`, resumo e período aquisitivo que contém **hoje**, com fallback para o mais recente já iniciado (`app/ponto/views.py:2007`, `:2014`, `:2022`). Tela/painel o lê em `:2100`; template em `app/templates/ponto/partials/painel_ausencia.html:168`; link PDF histórico em `app/ponto/views.py:2036`, rota `app/ponto/urls.py:73`.
- **F2 VISAO-FALTAS-FERIAS** registrada como visão por período aquisitivo, mesma ficha corrida e faixas art.130/art.131: `app/docs/BACKLOG.md:1285`. Não confundir com “F2” dos comentários do histórico de ausências acima: ali a seleção do PA é por hoje e a linha do tempo não é recortada nesse PA (`app/ponto/views.py:2015`, `:2022`).
- Fechamento mostra badge/filtro Rescisão (`app/templates/ponto/fechamento.html:65`), data na linha (`app/templates/ponto/_fech_linha.html:4`) e rótulo “verbas pela rescisão” (`:11`).
- Folha/exportação avisam que rescisão sai no módulo próprio: `app/templates/folha/index.html:57`, `app/templates/folha/exportar.html:16`; rotas folha em `app/folha/urls.py:7`, `:13`, `:23`.
- Fio do colaborador mostra aviso desligada/motivo e consulta histórica: `app/templates/chamados/fio_colaborador.html:70`.
- Manual ainda instrui “altere para demitido — acesso bloqueado automaticamente”: `app/templates/core/manual.html:117`; não descreve a semântica agendada da porta (`app/colaboradores/services/desligamento.py:87`).

**Arquivos em comum com O50 — dependências demonstradas; arquivo futuro efetivamente editado [nao verificado].**

- `(a)` `app/colaboradores/services/desligamento.py:127`; `app/colaboradores/services/vinculo.py:580`, `:855`; `app/ponto/services/cadastro_futuro.py:9`; `app/ponto/portas/celula.py` chamado em `app/ponto/services/cadastro_futuro.py:22`, `:44`.
- `(b)` leitores crus diretamente presentes no circuito: `app/colaboradores/views.py:1417`, `:2848`; `app/colaboradores/services/desligamento.py:18`; `app/colaboradores/services/vinculo.py:91`, `:863`; `app/colaboradores/services/drawer.py:22`; `app/colaboradores/services/ficha.py:234`; `app/ponto/views.py:2061`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`; templates `app/templates/colaboradores/editar.html:211`, `app/templates/colaboradores/partials/painel_colaborador.html:163`.
- `(c)` existência de arquivos de selo foi listada, mas sem leitura interna [nao verificado]; não afirmar que selo novo/AST/mutante de O50 já está implementado.
- `(d)` `app/colaboradores/services/desligamento.py:144`; `app/chamados/models.py:1606`, `:1827`, `:1856`, `:2499`; `app/chamados/signals.py:20`, `:92`; `app/chamados/services/competencia_trancada.py:119` é o precedente citado pelo aval; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51` compartilha executor.

**Lacuna de lei possível para consolidar:** modalidade de aviso prévio (trabalhado/indenizado), projeção temporal dos direitos e data financeira da liquidação: porta só aplica teto de 90, compara data com hoje e recebe data/motivo; não calcula aviso por anos de casa ou verbas (`app/colaboradores/services/desligamento.py:9`, `:40`, `:50`, `:87`). Não perguntar como lacuna se quem está no aviso segue ativo até a data: isso já está escrito (`:36`, `:55`; cron `:3`).

## 13/11 12:13 · sessao 85f5295c51c5 · - · - · origem=Ronald (tmux)

Itens 1, 6, 7 e 8 — censo somente leitura.

**1. Espelho 21→demissão: FALTA a combinação “período livre + RegistroPDFAuditavel”; EXISTE emissão livre informacional.**

- A referência `folha/services.py` do anexo não corresponde à cópia: o serviço está em `app/relatorios/services.py:165`. Para `data_ini` e `data_fim` informadas, usa essas datas (`:211-214`) e **não tenta o auditável**: marca `motivo='periodo livre'` (`:235-247`), chama `gerar_pdf_espelho_informacional_bytes` (`:250`), devolvendo `(pdf_bytes|None, pulados, n_ok, info, sem_movimento)` (`:260-264`).
- Universo do lote: colaboradores explícitos, ordenados por nome, ou `colaboradores_do_lote` (`app/relatorios/services.py:217`); esse seletor filtra empresa e situações (`:103-105`), com padrão do lote `ativo/afastado/ferias` (`:102`, `:165`). Não inclui desligado automaticamente; seleção explícita permite fornecê-lo (`:217`).
- A porta livre retorna **bytes de um PDF por colaborador**, sem hash e sem registro (`app/relatorios/pdf_espelho.py:1038-1071`). Dias são filtrados inclusivamente por `data_ini <= data <= data_fim` (`:602`). O coletor aplica teto em ontem, com `max(data_ini, ate_ontem(data_fim))` (`:285-287`); o avulso recusa data final hoje/futura pela porta `recusa_por_data_final` (`app/relatorios/views.py:1087-1098`; `app/relatorios/services.py:13-46`) e diferença entre datas superior a 180 dias (`app/relatorios/views.py:1083-1086`).
- Consumidores: avulso (`app/relatorios/views.py:1065-1098`) e lote (`app/relatorios/services.py:250`); o job chama o serviço do lote com as datas gravadas (`app/relatorios/lote_espelho.py:205-207`). Rotas: `app/relatorios/urls.py:24-34`.
- A porta auditável existe, mas recebe **mes/ano**, não intervalo: `gerar_pdf_espelho_bytes` (`app/relatorios/pdf_espelho.py:1074-1114`), embrulhada por `gerar_pdf_espelho` (`app/relatorios/services.py:49-96`). A competência deriva do corte da empresa, não necessariamente 21 (`app/relatorios/pdf_espelho.py:654-663`; `app/relatorios/cartao_pela_celula.py:21-24`; `app/ponto/janelas.py:36-57`).
- Seus filtros persistidos são mes/ano/tipo_escala/dias_renderizados/turnos, sem datas explícitas (`app/relatorios/pdf_espelho.py:1095-1101`). Bloqueio explícito do corpo: disputa aberta **do colaborador**, não recortada ao período (`:1084-1089`). Não afirmar que exige competência fechada: esse bloqueio não está no corpo atual (`:1074-1114`).
- O corpo atual ainda faz cálculos/leitura viva para emissão (`app/relatorios/pdf_espelho.py:1092-1093`, `:1045-1046`); não é uma porta de leitura de documento histórico congelado.

**6. RegistroPDFAuditavel: EXISTE trilha; FALTA porta de dossiê por colaborador/período e FALTA arquivo no próprio registro.**

- Unidade: uma emissão auditável; guarda hash único, tipo, gerado_por/em, colaborador_id/nome/matrícula, filtros_json, total_registros, IP, commit e hostname (`app/relatorios/models.py:14-65`). **Não possui FileField nem bytes do PDF** nesse modelo (`:30-65`).
- Escritor central de produção: `gerar_pdf_auditavel`, que cria registro e devolve `(bytes, registro)` (`app/relatorios/pdf_base.py:92-122`, `:152-180`).
- Chamadores/escritores indiretos:
  - espelho mensal: `app/relatorios/pdf_espelho.py:1103-1114`;
  - histórico de chamados: `app/relatorios/pdf_chamados.py:158-177`;
  - gestão de férias: `app/relatorios/services.py:338-348`;
  - gestão de HE: `app/ponto/views.py:867-875`;
  - retrato da folha: `app/folha/views_retrato.py:436-442`.
- O espelho mensal pode guardar os **bytes fora do registro**, em `FollowupChamado.anexo_pdf`, com hash denormalizado (`app/relatorios/services.py:75-90`). O download lê esse anexo por **followup_id**, não colaborador/período (`app/relatorios/views.py:1020-1045`).
- Leitor direto encontrado: verificação pública por hash inteiro ou prefixo ≥16 caracteres (`app/chamados/views.py:1185-1222`); retorna registro/autêntico, não busca por colaborador/período (`:1209-1221`).
- Não encontrei porta pronta `colaborador + período → registros/PDFs`: **FALTA**; ausência em toda árvore é resultado de busca, portanto, se exigido rigor absoluto de prova negativa, registrar `[nao verificado]`. O modelo permite filtrar `colaborador_id` e JSON, mas não declara período normalizado nem arquivo (`app/relatorios/models.py:43-56`).
- Escritor operacional fora de produção: mascarador de sombra altera `colaborador_nome` em massa (`bin/sombra_mascara.py:87-88`); regras de raspagem tratam `ip_origem` (`bin/sombra_regras_pessoais.py:73`). Não confundir com emissão produtiva.

**7. Motivo `rescisao_modulo_proprio`: EXISTE; consumidores identificados.**

- Produtor: `classificar_export` emite `status='fora'`, motivo e detalhe de saída pelo módulo de rescisão do Domínio (`app/folha/export.py:866-871`).
- `folha/views.py`: vocabulário compartilhado de tela/PDF (`app/folha/views.py:11-22`); Raio-X separa rescisões (`:594`), exclui do balde “outros” (`:598-600`) e mostra pílula própria (`:641`).
- `folha/retrato_folha.py`: `FORA_DA_CONTA` (`app/folha/retrato_folha.py:33`), `_balde` manda para `modulo_proprio` (`:89-95`), fora do bloqueado; contadores leem essas mesmas linhas (`:114-122`). Também exclui dos eixos dono/tipo/saldo (`:364-365`, `:378-379`, `:414-415`).
- `ponto/services/fechamento_lista.py`: traduz motivo para `rescisao` na linha, preservando motivo cru e detalhe (`app/ponto/services/fechamento_lista.py:25-37`, `:100`).
- `relatorios/views.py`: ranking de furos traduz motivo para “rescisao” e marca `folha=False`, consultando `classificar_export` (`app/relatorios/views.py:1655-1669`).
- `mensageria/nucleo/ferramentas.py`: `NAO_E_BLOQUEIO` (`:1196`), tradução (`:1202`), separa em ressalvas em vez de bloqueios (`:1246-1253`), publica total que trava sem rescisão (`:1266-1268`) e agregado de ressalvas de rescisão (`:1288-1290`).
- `app/core/placar_estrutural.py:331` contém o literal apenas em texto de evidência/descrição, não decide saída.

**8. H12: EXISTE e já usa fim da janela nesta cópia; não repetir como situação atual o backlog “qualquer data”.**

- Autoridade `sai_pela_rescisao(colab, fim_janela)` responde **booleano**: data_demissao não nula **e `<= fim_janela`**, sem exigir `>= início` (`app/folha/export.py:54-60`).
- Consumidores diretos encontrados: `classificar_export`, que obtém janela da empresa (`app/folha/export.py:815`, `:866`); `montar_resumos`, que obtém fim por empresa e preenche `rescisao` (`app/ponto/services/fechamento_lista.py:51`, `:103-109`).
- Consumidor do resultado da linha: contador `n_rescisao` do fechamento (`app/ponto/views.py:1212-1215`); indicação visível/filtrável é documentada no mesmo trecho.
- Universo do classificador: fechamentos fornecidos ou todos `FechamentoMensal` da empresa/mes/ano, por nome (`app/folha/export.py:782-788`); portanto a ausência de limite inferior do predicado não significa que ele busca todos os desligados históricos.
- Existem **outras perguntas** com a palavra “rescisao”, mas não cópias do gate de saída do TXT:
  - `prontidao` chama de rescisão o denominador zero com demissão **antes do início**, estrito `<` (`app/folha/export.py:1051-1071`); fallback recorta fim na demissão e responde rescisão se `fim < ini` (`:1081-1091`). Unidade: prontidão/horas previstas, não elegibilidade do TXT.
  - Raio-X recorta fim na demissão (`app/folha/views.py:609-616`).
  - retenção da célula recorta até demissão, inclusive (`app/folha/export.py:1206-1214`, `:1233`), com exceção especial do dia da demissão apenas para célula ausente/sem veredito (`:1237-1241`, `:1263-1264`). Célula acusando ainda pode reter nesse corpo (`:1241-1262`).
- Essas diferenças de perguntas/universos devem ser explicadas; não classificá-las automaticamente como divergência da mesma regra. A divergência atual entre retenção e a nova LEI-DESLIGADO deve ser consolidada pelo agente responsável pelo item 5, considerando que o corpo auditado não tem isenção global de desligado (`app/folha/export.py:1196-1266`).

## 13/11 12:13 · sessao e6ed0e93ffdb · - · - · origem=Ronald (tmux)

Item 2 — **EXISTE**.

- Modelo: uma linha por dia da entrada do turno + linha `ajuste` sem data para DSR/banco; versões `motor` e `oraculo`; rubricas em horas, abonos/previstos/realizados em minutos. Não contém valor monetário em reais. `app/ponto/models.py:293`, `app/ponto/models.py:303`, `app/ponto/models.py:315`, `app/ponto/models.py:325`, `app/ponto/models.py:349`.
- Escritor normal: `lavrar` substitui linhas por colaborador/competência/versão, apagando e recriando. Chamado no fechamento para motor e, desde 10/2026, oráculo. `app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/fechamento.py:1191`, `app/ponto/services/fechamento.py:1200`, `app/ponto/services/fechamento.py:1265`, `app/ponto/services/fechamento.py:1277`.
- Outras portas escritoras: `retratar_exportada`, retrato dos três campos de grade do fechamento gravado; `apurar_furo_da_exportada`, escreve apenas furo/carimbo nas linhas existentes. `app/ponto/services/dia_pago.py:405`, `app/ponto/services/dia_pago.py:425`, `app/ponto/services/dia_pago.py:538`, `app/ponto/services/dia_pago.py:592`, `app/ponto/services/dia_pago.py:671`.
- Leitura arbitrária até demissão **já existe**, passando demissão como `fim`: `linhas_do_periodo(colab,ini,fim)` → `{date: DiaPago}`, versão padrão motor, tipo dia, datas inclusivas; não recorta automaticamente pela demissão. `app/ponto/services/dia_pago.py:833`, `app/ponto/services/dia_pago.py:855`.
- `soma_do_periodo(colaboradores,ini,fim)` → `(somas,sem_lavratura,sem_apuracao)`, agrupado por colaborador, inclusivo, somente tipo dia. Exclui explicitamente ajuste de DSR/banco; não há rateio parcial nessa porta. `app/ponto/services/dia_pago.py:752`, `app/ponto/services/dia_pago.py:755`, `app/ponto/services/dia_pago.py:784`, `app/ponto/services/dia_pago.py:830`.
- `soma_da_competencia` inclui dia+ajuste, arredonda agregados em duas casas; retorna `None` sem lavratura. `app/ponto/services/dia_pago.py:550`.
- Leitores: espelho e, por ele, PDF (`app/ponto/services/espelho.py:406`, `app/relatorios/pdf_espelho.py:835`); calendário (`app/colaboradores/services/calendario.py:290`); app (`app/api/views.py:1439`); extrato parcial (`app/relatorios/views.py:482`); cartão (`app/relatorios/cartao_pela_celula.py:142`); porta exportação/conferência (`app/folha/porta_export.py:75`, `app/folha/porta_export.py:267`, `app/folha/porta_export.py:332`); gestão HE (`app/ponto/services/gestao_he.py:234`); prévia autorização HE (`app/ponto/services/autorizacao_he_periodo.py:172`); foto de passivo (`app/chamados/services/foto_e4b.py:289`).

Item 3 — **EXISTE**; diferentes universos são explícitos, não uma divergência semântica acidental.

- Inventário de todos lançamentos que tocam período: `ausencias_vigentes(de,ate,colaborador=None)` → QuerySet, sem filtro de status/tipo; sobreposição inclusiva. Ausência sem fim é aberta somente nos `TIPOS_EM_ABERTO`, nos demais tipos é dia único. `app/ponto/turnos.py:3142`, `app/ponto/turnos.py:3158`.
- Efeito financeiro: `ausencias_do_periodo(colab,ini,fim,para='folha')` → todos lançamentos **aprovados** que tocam `[ini,fim]`; `para='cobranca'` → não rejeitados. `app/ponto/turnos.py:2511`, `app/ponto/turnos.py:2523`.
- Cobertura por dia: `cobertura_ausencia_periodo` → `{date:Ausencia}`, recortado inclusivamente; sobreposição vence maior `(data,pk)`. Mesmos filtros de efeito. `app/ponto/turnos.py:2735`, `app/ponto/turnos.py:2753`, `app/ponto/turnos.py:2765`.
- Decisões por dia, inclusive rejeitadas: `decididos_do_periodo` → `{date:cartao_do_dia}`, sobreposição inclusiva, maior `(data,pk)`; não equivale a “cobertura financeira”. `app/ponto/services/dia_decidido.py:529`, `app/ponto/services/dia_decidido.py:550`.
- Histórico: `linha_do_tempo` → lista de lançamentos raiz, todos status, sem recorte temporal nativo; leitor em lote aplica sobreposição inclusiva depois da montagem. `app/ponto/services/dia_decidido.py:566`, `app/ponto/services/dia_decidido.py:587`, `app/ponto/services/atestados_lote.py:46`.
- Mesa DP: `ausencias_do_dp` exclui férias e rejeitadas, enquanto lista de ausências permite ambos. Diferença de universo está escrita. `app/ponto/turnos.py:3115`, `app/ponto/turnos.py:3138`, `app/ponto/views.py:2178`.
- Leitores: tela ausências (`app/ponto/views.py:2178`); copiloto/API mensageria (`app/api/views_mensageria.py:703`, `app/api/views_mensageria.py:1032`); fechamento (`app/ponto/services/fechamento.py:638`, `app/ponto/services/fechamento.py:680`); efeito ausência (`app/ponto/services/efeito_ausencia.py:28`); espelho (`app/ponto/services/espelho.py:579`); calendário (`app/colaboradores/services/calendario.py:531`); escala (`app/escala/utils.py:1174`); histórico drawer F2 e lote F3 (`app/ponto/views.py:2014`, `app/ponto/services/atestados_lote.py:54`).
- Histórico anual médico é outro universo: tipos médicos do catálogo, aprovadas, início no ano; conta dias ocupados entre 1º/1 e `min(31/12,hoje)`, inclusivo. `app/ponto/views.py:2837`, `app/ponto/views.py:2870`.

Item 4 — **DIVERGE** para cálculo/bordas do ciclo; saldo/vencimento **EXISTEM**; proporcionais rescisórias **FALTAM** na busca da cópia [nao verificado].

- Modelo armazena período aquisitivo por colaborador: início, fim, limite concessão, direito, concedido, vendido. Unidade: dias. `app/ferias/models.py:105`, `app/ferias/models.py:116`.
- Cálculo de ciclo existe na view: ancora admissão; `[aniversário,aniversário seguinte−1 dia]`; limite=fim+um ano; cria até ciclo vigente em **hoje**, não até demissão. Trata 29/2 com fallback 28/2. Escrita idempotente por `garantir_periodo_aquisitivo`. `app/ferias/views.py:139`, `app/ferias/views.py:145`, `app/ferias/views.py:161`, `app/ferias/views.py:180`, `app/ferias/services.py:254`.
- **Segundo sítio diverge:** edição calcula `fim=inicio+364 dias`, `limite=fim+365 dias`, em vez de aniversário/ano-calendário. Pode mudar borda em ano bissexto. `app/ferias/services.py:523`.
- Saldo = direito−concedido−vendido, sem proporcionalidade temporal. `app/ferias/models.py:216`.
- `status_de_periodo(pa,hoje)` é puro: saldo≤0 → concedido; fim≥referência → em_curso; limite<referência → vencido; demais adquirido. Último dia aquisitivo ainda em curso; dia limite concessão ainda não vencido. Aceita referência demissão, mas as telas passam hoje. `app/ferias/models.py:65`, `app/ferias/models.py:94`, `app/ferias/views.py:109`, `app/ferias/views.py:767`.
- Faltas do aquisitivo: conta registros `tipo='falta'`, aprovados, cujo início está inclusivamente no período; não expande range nessa property. Direito por faixas 30/24/18/12/0; `aplicar_art130` escreve direito. `app/ferias/models.py:52`, `app/ferias/models.py:141`, `app/ferias/services.py:243`.
- Outros sítios de leitura usam estado materializado: `periodos_vencendo` filtra status cru adquirido e limite≤hoje+dias; PDF gestão filtra status cru vencido/adquirido. Inteligência consulta juiz puro, logo podem responder diferente se o estado armazenado divergir. `app/ferias/services.py:111`, `app/relatorios/services.py:285`, `app/inteligencia/views.py:13`.
- Leitores de saldo/períodos: saldo e seção férias (`app/ferias/views.py:110`, `app/ferias/views.py:767`); situação férias (`app/ferias/situacao.py:138`, `app/ferias/situacao.py:162`); inteligência (`app/inteligencia/views.py:13`); relatório gestão (`app/relatorios/services.py:268`); drawer F2 lê só período contendo hoje ou último iniciado, rótulo armazenado (`app/ponto/views.py:2007`); alertas cron sincronizam e leem saldo (`app/ferias/management/commands/alertar_ferias_vencendo.py:22`).
- Nenhuma fórmula de avos, férias proporcionais ou indenização rescisória foi localizada na busca produção da cópia [nao verificado]. Saldo integral do aquisitivo não prova direito proporcional [nao verificado].

Arquivos em comum com O50(b), evidência de leitura crua nestas áreas:

- `app/ponto/turnos.py:2464`, `app/ponto/turnos.py:2483`: escala `ativa=True`.
- `app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:440`: data demissão/situação.
- `app/colaboradores/services/calendario.py:258`: escala ativa.
- `app/relatorios/views.py:455`, `app/relatorios/views.py:492`, `app/relatorios/views.py:833`: situação/escala ativa.
- `app/relatorios/services.py:105`: situação.
- `app/api/views.py:116`, `app/api/views.py:312`, `app/api/views.py:403`, `app/api/views.py:905`: situação/escala ativa.
- `app/ferias/views.py:605`: exclui desligado.
- `app/ferias/situacao.py:172`: situação ativo.
- Inclusão efetiva de cada arquivo em construção futura não está comprovada [nao verificado].

Lacunas candidatas a perguntas de lei, sem proposta:

1. Férias proporcionais na data de desligamento: avos, fração e reflexo do aviso; saldo atual é integral, fórmula proporcional não localizada. `app/ferias/models.py:216`; [nao verificado].
2. Destino/liquidação de DSR e banco da competência parcial de rescisão: porta de intervalo deliberadamente os exclui e não rateia. `app/ponto/services/dia_pago.py:755`.
3. Valor de liquidação das férias adquiridas/vencidas: código classifica dias/status; cálculo monetário de rescisão não localizado. `app/ferias/models.py:65`, `app/ferias/models.py:216`; [nao verificado].

## 13/11 12:10 · sessao f68b07bfd9af · DESLIGAR-LIQUIDA-TUDO · censo · origem=Code

Censo da cópia, sem alterações. Correspondência da cópia ao HEAD/hash informado: [nao verificado]. O material fornecido não traz linhas nem contagens da frota de produção; passivo, distribuição por competência e contador atual: [nao verificado].

**1. Escritores de `PerguntaDisputa.via_resolucao`**

| Sítio | Escrita |
|---|---|
| `app/chamados/models.py:2563` | Default `''` no nascimento. |
| `app/chamados/models.py:3015`, `app/chamados/models.py:3018` | `_nascer` insere com `**campos`; pode receber a via por esse argumento. Caller de produção fornecendo via assim: [nao verificado]. |
| `app/chamados/models.py:3102` | `_escrever_carimbo` atribui a via recebida em memória, juntamente com materialização e autor. |
| `app/chamados/models.py:3130`, `app/chamados/models.py:3153` | `carimbar` persiste a via recebida se o C6 considerar a pergunta morta. Identidade/tranca geram erro; pergunta ainda viva desfaz a tentativa: `app/chamados/models.py:3119`, `app/chamados/models.py:3144`. |
| `app/chamados/models.py:3163` | `_desfazer_em_memoria` restaura a via anterior após tentativa recusada/falha; não persiste por si. |
| `app/chamados/models.py:3184`, `app/chamados/models.py:3186`, `app/chamados/models.py:3191` | `carimbar_identidade` persiste a via recebida, restrita a `VIAS_DE_IDENTIDADE`; preserva o carimbo anterior quando solicitado. |
| `app/chamados/models.py:3209`, `app/chamados/models.py:3213` | `descarimbar` grava `''`, limpando também materialização e autor. |
| `app/chamados/models.py:3265` | `descarimbar_em_lote` executa `UPDATE` para `via_resolucao=''`; poupa `duplicata` e `motivo_retratado`. |
| `app/chamados/models.py:3276` | `encerrar_por_fato_no_espelho` grava diretamente `fato_materializado`. |
| `app/chamados/services/relavra_via.py:125`, `app/chamados/services/relavra_via.py:127` | `relavrar` troca a via pela calculada por `via_de_hoje`, somente quando a anterior é `declaracao` ou `completude_turno`. |

Cada chamador que determina a escrita:

| Sítio | Via gravada pela porta |
|---|---|
| `app/chamados/models.py:3065` | `fechar_por_admin` → `admin_decidiu`. |
| `app/chamados/models.py:1922` | Fechamento da disputa por desligamento/admin → `fechar_por_admin` → `admin_decidiu`. |
| `app/chamados/services/validacao.py:289` | Aceite quando o dia já é folga → `admin_decidiu`. |
| `app/chamados/services/validacao.py:329` | Aceite que declara a folga → `admin_decidiu`. |
| `app/ponto/services/veredito_celula.py:190` | Decisão do dia → `admin_decidiu`. |
| `app/chamados/models.py:2062` | Remoção do motivo → `motivo_retratado`. |
| `app/chamados/services/fio.py:1360` | Retratação da duplicata → `duplicata`. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:331` | Retratação pelo backfill → porta `retratar_para_canonica` → `duplicata`; delegação em `app/chamados/services/fio.py:1342`. |
| `app/chamados/services/materializacao.py:930` | Recusa por paridade → `paridade_recusada`. |
| `app/chamados/services/materializacao.py:1105` | Colisão com fato existente → `fato_ocupado`. |
| `app/chamados/reconciliador.py:406` | Reconciliação sem turno aberto → `sem_turno_aberto`. |
| `app/chamados/reconciliador.py:596` | Absolvição pela célula → `celula_absolveu`. |
| `app/chamados/models.py:3288` | `encerrar_fantasma` → `fantasma`; chamado por `app/chamados/reconciliador.py:1148`. |
| `app/chamados/services/materializacao.py:555`, `app/chamados/services/materializacao.py:567` | Absorção pela ausência → `declaracao` se declarou hora; senão `ausencia_dia`. |
| `app/chamados/services/materializacao.py:1283` | Plantio → `declaracao` se declarou hora; senão `via_pelo_fato`, ou `chamado_encerrado` sem destino: `app/chamados/services/materializacao.py:1271`, `app/chamados/services/materializacao.py:1274`. |
| `app/chamados/services/materializacao.py:1437` | Materialização já comprovada pelo juiz → `juiz_aprovou`. |
| `app/chamados/services/disputa_emissao.py:1986` | Resolução pelo chamado → resultado de `via_pelo_fato`. |
| `app/chamados/services/disputa_emissao.py:2058`, `app/chamados/services/disputa_emissao.py:2060` | Escolha sem credor → `via_pelo_fato` do pai, ou `chamado_encerrado` sem pai. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68` | Fato comprovado no espelho → `fato_materializado`. |
| `app/chamados/models.py:1507` | Simulação da ressuscitação → `''` **somente na cópia em memória**. |
| `app/chamados/models.py:1567` | Ressuscitação → `''` em lote. |
| `app/chamados/services/disputa_emissao.py:1485` | Nova resposta reabre carimbo sem lastro → `''`, restrito pelas vias em `app/chamados/models.py:3314`. |
| `app/chamados/services/disputa_emissao.py:1630` | Reabertura para correção → `''` em lote. |
| `app/chamados/services/relavra_via.py:246` | Desfaz carimbo que o C6 considera vivo → `''`. |
| `app/chamados/models.py:3299` | Reabre `fantasma` → `''`; caller em `app/chamados/management/commands/reabrir_orfas_enterradas.py:157`. |
| `app/chamados/models.py:3323` | Reabre `competencia_trancada` → `''`; caller em `app/chamados/services/competencia_trancada.py:279`. |

Os retornos possíveis de `via_pelo_fato` são, individualmente: `cadastro_folga` em `app/chamados/juizes.py:1749`; `adesao` em `app/chamados/juizes.py:1750`; `ausencia_dia` em `app/chamados/juizes.py:1758`; `completude_turno` em `app/chamados/juizes.py:1766`; `chamado_encerrado` em `app/chamados/juizes.py:1767`.

O antigo escritor de `competencia_trancada` foi removido; a porta atual `carimbar_identidade` não aceita essa via: `app/chamados/models.py:3067`, `app/chamados/models.py:3179`.

**2. Leitores de `via_resolucao`: ramo conhecido e valor novo**

| Leitor | Comportamento atual; valor desconhecido |
|---|---|
| `app/chamados/juizes.py:679` — `_pergunta_materializada_de_fato` | Vias em `VIAS_SEM_TESTEMUNHA`, com materialização, recebem tratamento próprio. Identidade+tranca retornam verdadeiro em `app/chamados/juizes.py:691`; as demais consultam janela/lei. Desconhecida passa aos ramos por tipo: escolha aceita o carimbo; hora/data-hora procuram prova da batida; texto procura completude: `app/chamados/juizes.py:706`, `app/chamados/juizes.py:708`, `app/chamados/juizes.py:816`. |
| `app/chamados/juizes.py:853` — memo do C6 | Inclui a via na chave. Qualquer valor é dado da chave; não classifica nem rejeita. |
| `app/chamados/juizes.py:1386` — projeção temporal | Materialização posterior ao instante consultado é desconsiderada; a via também é apagada nessa projeção, exceto `duplicata`/`motivo_retratado`: `app/chamados/juizes.py:1394`. Nova via seria apagada nessa condição. |
| `app/chamados/juizes.py:1412` — C6, antes da resposta | `competencia_trancada`, `duplicata`, `motivo_retratado`, com materialização, matam antes de ler a resposta. Desconhecida não entra. |
| `app/chamados/juizes.py:1417` — C6, resposta | Resposta carimbada com texto mantém a pergunta viva, salvo decisão admin na célula ou concordância comprovada com a luz. Desconhecida não vence esse ramo: `app/chamados/juizes.py:1425`, `app/chamados/juizes.py:1429`. |
| `app/chamados/juizes.py:1437` — C6, identidade | Identidade+tranca materializadas matam. `admin_decidiu` entra aqui **depois** do ramo da resposta. Desconhecida segue para marco/célula/lei/lastro: `app/chamados/juizes.py:1444`, `app/chamados/juizes.py:1450`, `app/chamados/juizes.py:1476`, `app/chamados/juizes.py:1483`. |
| `app/chamados/models.py:3056` — `fechar_por_admin` | Apenas `admin_decidiu` já materializada é no-op. Outra via pode ser sobrescrita por `admin_decidiu`. |
| `app/chamados/models.py:3119` — `carimbar` | Identidade+tranca geram `ValueError`; desconhecida é submetida ao C6. |
| `app/chamados/models.py:3179` — `carimbar_identidade` | Qualquer via fora das cinco identidades gera `ValueError`; inclui tranca e nova desconhecida. |
| `app/chamados/models.py:3181` — guarda de terminalidade | Linha fora do fato não recebe via que a devolva ao fato. |
| `app/chamados/models.py:3202` — `fora_do_fato` | Verdadeiro somente para `duplicata`/`motivo_retratado`; desconhecida retorna falso. |
| `app/chamados/models.py:3256` — descarimbo em lote | Proíbe passar `via_resolucao` em `**outros`, qualquer que seja o valor. |
| `app/chamados/models.py:3266` — filtro do descarimbo | Poupa apenas fora-do-fato; nova via pode ser apagada se selecionada pelo chamador. |
| `app/chamados/models.py:3274` — encerrar pelo fato | `fato_materializado` ou fora-do-fato são no-op; outra via pode ser sobrescrita. |
| `app/chamados/models.py:3285` — encerrar fantasma | Só `fantasma` já materializada é no-op; outra via passa pela tentativa de carimbar fantasma/C6. |
| `app/chamados/models.py:3297` — reabrir fantasma | Só toca `fantasma`; nova ignorada. |
| `app/chamados/models.py:3314` — reabrir por resposta | Só toca `chamado_encerrado`/`legado_sem_lastro`; nova ignorada. |
| `app/chamados/models.py:3321` — reabrir por competência | Só toca `competencia_trancada`; nova ignorada. |
| `app/chamados/models.py:3351` — `q_vivas` | Identidade+tranca entram no teste de identidade; somente `VIAS_QUE_VENCEM_A_RESPOSTA` vencem resposta: `app/chamados/models.py:3353`. Desconhecida pode continuar no pré-filtro amplo: `app/chamados/models.py:3360`. |
| `app/chamados/services/relavra_via.py:118` — re-lavra | Só reescreve `declaracao`/`completude_turno`; desconhecida ignorada. |
| `app/chamados/services/relavra_via.py:155` — seleção por dia | Seleciona somente essas duas vias; desconhecida ignorada. |
| `app/chamados/services/relavra_via.py:181` — carimbo desfazível | Protege `VIAS_DE_IDENTIDADE`; tranca/desconhecida podem ser candidatas. A decisão de desfazer exige depois C6 vivo: `app/chamados/services/relavra_via.py:242`. |
| `app/chamados/services/materializacao.py:444` — estado de aplicação | Para pergunta de hora com texto e materialização, `paridade_recusada` vira `recusada_paridade`; **qualquer outra via, incluindo admin, tranca e desconhecida, vira `aplicada`**: `app/chamados/services/materializacao.py:448`. |
| `app/chamados/services/materializacao.py:632` — absorvidas por ausência | Só inclui `ausencia_dia`/`declaracao`; desconhecida ignorada. |
| `app/chamados/services/passivo_validado.py:141` — balde da recusa | Qualquer via não vazia que mudou vira `via:<valor>`, inclusive desconhecida. |
| `app/chamados/services/fio.py:1244` — mérito perdedor | Apenas `admin_decidiu` com resposta recebe proteção de mérito contra retratação. Tranca/desconhecida não recebem. |
| `app/chamados/services/fio.py:1351` — retratar duplicata | Só `duplicata` é no-op; desconhecida pode seguir à retratação, sujeita às guardas da porta. |
| `app/chamados/services/fabrica_da_celula.py:149` | Exclui apenas `duplicata`; desconhecida permanece candidata. |
| `app/chamados/services/tarefa_do_dia.py:230` | Exclui fora-do-fato, com exceção temporal da retratação. Desconhecida continua como pergunta do fato; decisões e vida C6 são lidas depois: `app/chamados/services/tarefa_do_dia.py:234`, `app/chamados/services/tarefa_do_dia.py:245`, `app/chamados/services/tarefa_do_dia.py:306`. |
| `app/ponto/services/cartorio.py:1650` | Ramo especial exige que todas sejam `duplicata`; desconhecida impede esse ramo. |
| `app/ponto/services/flip_auto.py:129` | Fila de inversão seleciona apenas `paridade_recusada`; desconhecida ignorada. |
| `app/chamados/services/competencia_trancada.py:276` | Reabertura seleciona apenas `competencia_trancada`; desconhecida ignorada. |
| `app/chamados/models.py:2056` | Remover motivo poupa quem já é `motivo_retratado`; desconhecida não é poupada por esse teste. |
| `app/chamados/models.py:3222` | Mudança no campo integra os campos que acordam a célula, independentemente do valor. |

Em cada sítio abaixo, o teste exclui **somente `duplicata`/`motivo_retratado`**. Admin, tranca e uma via nova permanecem no universo; esse teste não aceita nem recusa a resposta: [nao verificado]

| Sítio | Universo afetado |
|---|---|
| `app/chamados/models.py:1489` | Candidatas à ressuscitação do chamado. |
| `app/chamados/models.py:2725` | `do_fato`, ramo de episódio. |
| `app/chamados/models.py:2756` | `q_dos_fatos`/`linhas_dos_fatos`. |
| `app/chamados/models.py:3486` | Índice único por marco. |
| `app/chamados/models.py:3492` | Índice único por episódio. |
| `app/chamados/services/disputa_emissao.py:328` | Dedup de motivo sem data. |
| `app/chamados/services/disputa_emissao.py:400` | Dedup de motivo datado. |
| `app/chamados/services/disputa_emissao.py:419` | Dedup de furo por dia. |
| `app/chamados/services/disputa_emissao.py:437` | Outro ramo de dedup de furo por dia. |
| `app/chamados/services/fio.py:1101` | Contador de reuso sem rechaveamento. |
| `app/chamados/services/fio.py:1168` | Contador de saída sem dia. |
| `app/chamados/services/fio.py:1434` | Universo `_no_fato`. |
| `app/chamados/services/fio.py:1854` | Contador de fecho cruzando dias. |
| `app/chamados/services/fio.py:1890` | Contador de pergunta sobre batida retratada. |
| `app/chamados/services/fio.py:1912` | Universo do grupo. |
| `app/chamados/services/fio.py:1957` | Acervo considerado nas colisões. |

Apresentação, auditoria e comandos:

| Sítio | Comportamento; desconhecida |
|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:186` | Frases próprias para `chamado_encerrado`, `declaracao`, `completude_turno`, `sem_turno_aberto`, `admin_decidiu`, `adesao`, `fato_ocupado`, `paridade_recusada`, `duplicata`. Demais vias não vazias usam `get_via_resolucao_display`; tranca recebe seu choice. Resultado desse método para valor sem choice na instalação: [nao verificado]. |
| `app/chamados/services/explicador.py:93` | Se o juiz de lastro aprova, mostra resolvida/“Nada a fazer”, com display da via em `app/chamados/services/explicador.py:95`; desconhecida segue o juiz, sem ramo próprio. |
| `app/chamados/models.py:1921`, `app/chamados/models.py:1930` | Via anterior na trilha do fechamento; qualquer valor é transportado. |
| `app/chamados/models.py:2061` | Via anterior no texto da retratação; qualquer valor é transportado. |
| `app/chamados/reconciliador.py:1144`, `app/chamados/reconciliador.py:1155` | Foto antes/depois do fantasma; qualquer valor é transportado. |
| `app/chamados/services/disputa_emissao.py:2050`, `app/chamados/services/disputa_emissao.py:2065`, `app/chamados/services/disputa_emissao.py:2068` | Foto e trilha do fecho sem credor; sem classificação adicional. |
| `app/chamados/services/materializacao.py:674` | Foto reversível da absorção; qualquer valor é transportado. |
| `app/chamados/services/passivo_validado.py:64`, `app/chamados/services/passivo_validado.py:83` | Foto reversível; qualquer valor é transportado. |
| `app/chamados/services/passivo_validado.py:521` | Guarda via anterior para comparar o resultado. |
| `app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:136` | Trilha da re-lavra; transporta os valores anterior/novo. |
| `app/chamados/services/relavra_via.py:245`, `app/chamados/services/relavra_via.py:252` | Trilha do descarimbo; transporta a via anterior. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:101` | Seleciona `admin_decidiu` materializada sem validação para reconstruir atos; tranca/nova ignoradas. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:314` | Repete a guarda de `admin_decidiu` sob trava; nova ignorada. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:330`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:335` | Foto/trilha anterior à retratação. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:398`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:404` | Diff conserva qualquer via nas intocadas e projeta `duplicata` nas retratadas. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:504` | Carrega a via para o plano de retratação; classificação delegada às portas citadas. |
| `app/chamados/management/commands/relavrar_via_i6.py:58` | Fora de `declaracao`/`completude_turno`, imprime “via de DECISAO, nao se re-lavra”. Nova recebe esse rótulo, mesmo sem representar decisão. |
| `app/chamados/management/commands/relavrar_via_i6.py:88` | Conta pares de vias; admite qualquer valor. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:67` | Seleciona apenas `fantasma`; nova ignorada. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:129`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:142`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:160` | Snapshot/trilha da via. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:33` | Exclui `fato_materializado`; nova pode entrar na consulta inicial. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40` | Só sobrescreve `''`, `legado_sem_lastro`, `None`; nova fica intocada. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:49`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:66`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:75` | Contagem e trilha das vias. |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70` | Sem batida fonte, `ausencia_dia`/`declaracao` recusam como absorção; outra via recusa como `sem_fonte`: `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:72`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:48` | Piso I6 deriva de `VIAS_SEM_TESTEMUNHA ∪ VIAS_SEM_LASTRO`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:472` | Se uma desconhecida alcançar `_i6_pares`, fica fora do piso e cai em violação: `app/chamados/management/commands/auditar_invariantes_chamados.py:483`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:202` | Contraprova F1t exige via preenchida e pergunta morta; nova morta recebe “carimbo/celula” se não validada/não identidade: `app/chamados/management/commands/auditar_invariantes_chamados.py:205`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:433` | Agrupa contagem por qualquer via. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:542` | Devolve via com PK do candidato I6. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:955` | I9 acusa materializada com via vazia; nova não vazia passa nesse teste. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1710` | Piso F1t admite via preenchida se C6 declarar morta, inclusive nova reconhecida pelo juiz. |

Enumerações fechadas encontradas:

| Enumeração | Conteúdo/efeito relevante |
|---|---|
| `app/chamados/models.py:2394` — `VIAS_FORA_DO_FATO` | `duplicata`, `motivo_retratado`; altera identidade, dedup e índices. |
| `app/chamados/models.py:2511` — `VIAS_DE_IDENTIDADE` | `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`, `motivo_retratado`; controla a porta de identidade. |
| `app/chamados/models.py:2525` — `VIAS_QUE_VENCEM_A_RESPOSTA` | Tranca + fora-do-fato; fonte do ramo anterior à resposta no C6 e SQL. |
| `app/chamados/models.py:2526` — `VIAS` | Choices do campo; inclui tranca e admin. Campo limitado a 24 caracteres: `app/chamados/models.py:2563`. |
| `app/chamados/models.py:3096` — `vias_que_encerram_por_identidade` | Identidades + tranca; alimenta C6, SQL e `_ABSOLVEM_SEMPRE`. |
| `app/chamados/models.py:3733` — `VIAS_SEM_TESTEMUNHA` | `completude_turno`, `fantasma`, `sem_turno_aberto`, `juiz_aprovou`, `admin_decidiu`, `paridade_recusada`, `duplicata`, `fato_ocupado`, `adesao`, `cadastro_folga`, `celula_absolveu`, `ausencia_dia`, `competencia_trancada`, `motivo_retratado`. |
| `app/chamados/models.py:3769` — `VIAS_SEM_LASTRO` | `chamado_encerrado`, `legado_sem_lastro`; permite reabrir por resposta. |
| `app/chamados/services/relavra_via.py:77` — `VIAS_DERIVADAS` | `declaracao`, `completude_turno`; permite reescrever a via. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40` — `SEM_HISTORIA` | 

[truncado em 20000 de 43754 caracteres]

## 13/11 12:05 · sessao f2a5216fc256 · - · - · origem=Ronald (tmux)

Censo de `via_resolucao`, somente leitura. Os caminhos abaixo são relativos à raiz da cópia.

**Escritores físicos e portas**

| Sítio | Valor gravado / comportamento |
|---|---|
| `app/chamados/models.py:3015`–`:3018` | `_nascer` insere a pergunta; o campo nasce `''` pelo default de `:2563`, salvo valor recebido via `**campos`. Chamador de produção passando uma via por esse `**campos`: [nao verificado]. |
| `app/chamados/models.py:3098`–`:3102` | `_escrever_carimbo` atribui `materializada_em`, autor e a via recebida; quem persiste é a porta. |
| `app/chamados/models.py:3104`, `:3130`, `:3153` | `carimbar(via, ...)` salva a via recebida somente se o C6 disser que a pergunta fica morta; se viva, desfaz tudo e retorna `False` (`:3144`). Recusa vias de identidade/tranca com `ValueError` (`:3119`). Via desconhecida pode ser tentada, mas passa pelo C6. |
| `app/chamados/models.py:3169`, `:3179`, `:3184`, `:3191` | `carimbar_identidade` grava a via recebida, mas somente as cinco `VIAS_DE_IDENTIDADE`; desconhecida e `competencia_trancada` geram `ValueError`. |
| `app/chamados/models.py:3204`–`:3213` | `descarimbar` grava `''` e limpa materialização/autor; não descarimba `duplicata`/`motivo_retratado`. |
| `app/chamados/models.py:3253`–`:3267` | `descarimbar_em_lote` faz `UPDATE` de via para `''`, com os dois outros campos; exclui `VIAS_FORA_DO_FATO`. |
| `app/chamados/models.py:3269`–`:3277` | `encerrar_por_fato_no_espelho` grava diretamente `fato_materializado`; recusa no-op se já nessa via ou fora do fato. |
| `app/chamados/services/relavra_via.py:118`–`:128` | `relavrar` reescreve somente vias originais `declaracao`/`completude_turno`, para `via_de_hoje`; salva também o veredito lavrado. Destinos: `declaracao` em `:98`, ou os destinos de `via_pelo_fato` em `:103`; conversa/sem destino não são tocados (`:101`). |

**Cada chamador de escrita encontrado**

| Sítio | Via |
|---|---|
| `app/chamados/models.py:3051`–`:3065` | `fechar_por_admin` → `admin_decidiu`. |
| `app/chamados/models.py:1922` | `DisputaSupervisao.fechar` chama `fechar_por_admin` nas perguntas selecionadas; trilha declara `admin_decidiu` em `:1931`. |
| `app/chamados/services/validacao.py:289` | `fechar_por_admin` → `admin_decidiu`. |
| `app/chamados/services/validacao.py:329` | Outro ramo de `validar_pergunta` chama `fechar_por_admin` → `admin_decidiu`. |
| `app/ponto/services/veredito_celula.py:190` | `fechar_por_admin` → `admin_decidiu`. |
| `app/chamados/models.py:2062` | `remover_motivo` → `motivo_retratado`. |
| `app/chamados/services/fio.py:1360` | `retratar_duplicata` → `duplicata`, preservando materialização anterior quando existente. |
| `app/chamados/services/materializacao.py:930` | Guarda de paridade → `paridade_recusada`. |
| `app/chamados/services/materializacao.py:1105` | `_carimbar_recusa`, colisão → `fato_ocupado`. |
| `app/chamados/reconciliador.py:406` | `sem_turno_aberto`. |
| `app/chamados/reconciliador.py:596` | `celula_absolveu`. |
| `app/chamados/models.py:3288` | `encerrar_fantasma` → `fantasma`; chamado no reconciliador em `app/chamados/reconciliador.py:1148`. |
| `app/chamados/services/materializacao.py:555`, `:567` | Absorção por ausência → `declaracao` se `declarou_hora`, senão `ausencia_dia`. |
| `app/chamados/services/materializacao.py:1283` | Materialização → `declaracao` se declarou hora; senão `via_pelo_fato` (`:1271`) ou `chamado_encerrado` sem destino (`:1274`). |
| `app/chamados/services/materializacao.py:1437` | `juiz_aprovou`. |
| `app/chamados/services/disputa_emissao.py:1986` | `via_pelo_fato(chamado, dia, motivo)`. |
| `app/chamados/services/disputa_emissao.py:2058`–`:2060` | Escolha sem credor → `via_pelo_fato(pai, ...)` ou `chamado_encerrado` sem pai. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68` | `encerrar_por_fato_no_espelho` → `fato_materializado`. |
| `app/chamados/models.py:1507` | `perguntas_corrigiveis` limpa via **em cópia em memória**, sem persistir. |
| `app/chamados/models.py:1567` | `ressuscitar_perguntas` → `''` em lote, junto com limpeza da resposta/validação por carimbo. |
| `app/chamados/services/disputa_emissao.py:1485` | `reabrir_por_resposta` → `''`, somente vias `chamado_encerrado`/`legado_sem_lastro` carimbadas (`models.py:3314`). |
| `app/chamados/services/disputa_emissao.py:1630` | `reabrir_para_correcao` → `''` em lote. |
| `app/chamados/services/relavra_via.py:246` | `desfazer_carimbo_sem_lastro` → `''`, após C6 declarar viva (`:242`). |
| `app/chamados/models.py:3299` | `reabrir_fantasma` → `''`; chamado em `app/chamados/management/commands/reabrir_orfas_enterradas.py:157`. |
| `app/chamados/models.py:3323` | `reabrir_por_competencia` → `''`; chamado em `app/chamados/services/competencia_trancada.py:279`. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:331` | Chama `retratar_para_canonica`, que chama a porta de `duplicata` em `app/chamados/services/fio.py:1342`. |

`via_pelo_fato` pode devolver `cadastro_folga`, `adesao`, `ausencia_dia`, `completude_turno` ou `chamado_encerrado`; cada retorno está em `app/chamados/juizes.py:1749`, `:1750`, `:1758`, `:1766`, `:1767`. O escritor antigo de `competencia_trancada` foi removido; a lápide registra isso em `app/chamados/models.py:3067`–`:3072`.

**Leitores decisórios: comportamento das vias e de valor desconhecido**

| Leitor | Valores conhecidos e valor novo |
|---|---|
| `app/chamados/juizes.py:679` | `_pergunta_materializada_de_fato`: somente `VIAS_SEM_TESTEMUNHA` + materialização entram no atalho. Dentro dele, identidade+tranca absolvem sempre (`:689`–`:692`); demais vias sem testemunha consultam lei/janela (`:693`–`:705`). Desconhecida fica fora e passa pelos ramos de tipo de resposta: escolha aceita materialização (`:706`), hora exige batida (`:708`), demais exigem testemunha de completude conforme docstring `:663`–`:669`. |
| `app/chamados/juizes.py:853` | Chave do memo de `pergunta_viva`: qualquer string entra como dado; alteração da via invalida a identidade do memo. Não classifica nem rejeita valor novo. |
| `app/chamados/juizes.py:1386`, `:1394` | Projeção temporal: se materialização está no futuro, zera via, exceto `duplicata`/`motivo_retratado`. Via nova seria zerada nessa projeção. |
| `app/chamados/juizes.py:1412` | C6: `competencia_trancada`, `duplicata`, `motivo_retratado` materializadas morrem **antes da resposta**. Via nova desconhecida não entra. |
| `app/chamados/juizes.py:1417`–`:1429` | C6: resposta com texto vence via desconhecida; continua viva salvo decisão admin na célula ou concordância com a luz. |
| `app/chamados/juizes.py:1437`–`:1439` | C6: identidade+tranca materializadas morrem; `admin_decidiu` passa aqui, mas somente **depois** do ramo da resposta. Via desconhecida segue para fato/célula/lei/materialização (`:1444`, `:1450`, `:1476`, `:1483`). Logo não há um default único “desconhecida = morta”. |
| `app/chamados/models.py:3056` | `fechar_por_admin`: somente `admin_decidiu` materializada é no-op; desconhecida pode ser sobrescrita por `admin_decidiu` se chegar a esta porta. |
| `app/chamados/models.py:3119` | `carimbar`: identidade+tranca produzem erro; desconhecida segue ao C6. |
| `app/chamados/models.py:3179` | `carimbar_identidade`: tudo fora das cinco identidades produz erro, incluindo via nova. |
| `app/chamados/models.py:3181` | Pergunta já fora do fato não pode receber via que a devolva ao fato; desconhecida seria recusada nesse caso. |
| `app/chamados/models.py:3202` | `fora_do_fato`: verdadeiro somente para `duplicata`/`motivo_retratado`; nova é falsa. |
| `app/chamados/models.py:3256` | `descarimbar_em_lote` proíbe fornecer `via_resolucao` em `**outros`, independentemente do valor. |
| `app/chamados/models.py:3266` | Descarimbo em lote poupa somente fora-do-fato; via nova fica elegível ao apagamento. |
| `app/chamados/models.py:3274` | Encerrar por fato é no-op em `fato_materializado`/fora-do-fato; desconhecida pode ser sobrescrita pela porta. |
| `app/chamados/models.py:3285` | Encerrar fantasma é no-op somente em `fantasma` materializada; desconhecida pode ser tentada como `fantasma`, sob C6. |
| `app/chamados/models.py:3297` | Reabrir fantasma só toca `fantasma`; nova ignorada. |
| `app/chamados/models.py:3314` | Reabrir por resposta só toca `chamado_encerrado`/`legado_sem_lastro`; nova ignorada. |
| `app/chamados/models.py:3321` | Reabrir por competência só toca `competencia_trancada`; nova ignorada. |
| `app/chamados/models.py:3348`–`:3362` | `q_vivas`: identidade+tranca sem resposta ficam excluídas por `_identidade`; só tranca+fora-do-fato vencem resposta em `_trancada`. Via nova não tem esses privilégios e pode continuar no queryset, que é pré-filtro generoso. |
| `app/chamados/services/relavra_via.py:118` | Re-lavra apenas `declaracao`/`completude_turno`; via nova ignorada. |
| `app/chamados/services/relavra_via.py:155` | Seleção da re-lavra por dia só inclui essas duas; via nova ignorada. |
| `app/chamados/services/relavra_via.py:181` | `q_carimbo_desfazivel` protege apenas `VIAS_DE_IDENTIDADE`; tranca e nova podem entrar como candidatas. Depois o C6 impede desfazer carimbo legítimo (`:242`). |
| `app/chamados/services/materializacao.py:444`–`:448` | **Risco de UI:** pergunta de hora com resposta e `materializada_em`: `paridade_recusada` vira `recusada_paridade`; **toda outra via, inclusive tranca/admin/nova desconhecida, vira `aplicada`**, sem conferir batida neste ramo. |
| `app/chamados/services/materializacao.py:632` | `perguntas_absorvidas_por`: inclui somente `ausencia_dia`/`declaracao`; nova ignorada. |
| `app/chamados/services/passivo_validado.py:141`–`:145` | `_balde_da_recusa`: qualquer via não vazia que mudou vira balde `via:<valor>`; nova seria classificada assim, sem enumeração. |
| `app/chamados/services/fio.py:1244` | `merito_perdedor`: somente `admin_decidiu` com resposta é mérito que impede retratação silenciosa; nova não recebe essa proteção. |
| `app/chamados/services/fio.py:1351` | Idempotência da retratação só reconhece `duplicata`; via nova não é no-op. |
| `app/chamados/services/fabrica_da_celula.py:149` | Lista exclui apenas `duplicata`; nova entra, e a elegibilidade para descarimbar usa `q_carimbo_desfazivel` em `:150`. |
| `app/chamados/services/tarefa_do_dia.py:230`–`:235` | Exclui fora-do-fato, exceto retratação posterior ao instante; via nova segue como pergunta do fato, podendo depois ser excluída por decisão `retratacao`. |
| `app/ponto/services/cartorio.py:1650` | Reconciliação especial somente se **todas** as perguntas forem `duplicata`; nova impede esse ramo especial. |
| `app/ponto/services/flip_auto.py:129` | Fila de inversão só seleciona `paridade_recusada`; nova ignorada. |
| `app/chamados/services/competencia_trancada.py:276` | Reabertura seleciona apenas `competencia_trancada`; nova ignorada. |

**Cada leitor de identidade/dedup/contadores que exclui fora-do-fato**

Nestes sítios, `duplicata`/`motivo_retratado` são excluídas; **todos os demais valores, inclusive admin, tranca e desconhecida, continuam no universo**, sem decisão financeira por esse teste: [nao verificado]

- Perguntas corrigíveis do chamado: `app/chamados/models.py:1489`.
- `do_fato`, ramo de episódio: `app/chamados/models.py:2725`.
- `q_dos_fatos`, usado por `linhas_dos_fatos`: `app/chamados/models.py:2756`.
- Índice único de marco: `app/chamados/models.py:3486`.
- Índice único de episódio: `app/chamados/models.py:3492`.
- Dedup de motivo sem data: `app/chamados/services/disputa_emissao.py:328`.
- Dedup de motivo datado sem destino obrigatório: `app/chamados/services/disputa_emissao.py:400`.
- Dedup de furo por dia: `app/chamados/services/disputa_emissao.py:419`.
- Outro dedup de furo por dia: `app/chamados/services/disputa_emissao.py:437`.
- Contador `reuso_sem_rechave`: `app/chamados/services/fio.py:1101`.
- Contador `sem_dia_na_saida`: `app/chamados/services/fio.py:1168`.
- Universo `_no_fato`: `app/chamados/services/fio.py:1434`.
- Contador `fecho_cruza_dias`: `app/chamados/services/fio.py:1854`.
- Contador `perguntas_sobre_batida_retratada`: `app/chamados/services/fio.py:1890`.
- `universo_do_grupo`: `app/chamados/services/fio.py:1912`.
- Acervo no agrupamento de colisões: `app/chamados/services/fio.py:1957`.

**Cada leitor de apresentação/auditoria/comandos**

| Sítio | Ação e valor novo |
|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:186` | Frases explícitas para `chamado_encerrado`, `declaracao`, `completude_turno`, `sem_turno_aberto`, `admin_decidiu`, `adesao`, `fato_ocupado`/`paridade_recusada`, `duplicata`; demais não vazias usam `get_via_resolucao_display|lower`. Tranca usa o rótulo dos choices. Valor novo sem choices aparecerá pela resolução padrão desse método [nao verificado na implementação do Django desta cópia]. |
| `app/chamados/services/explicador.py:93`–`:95` | Se juiz de materialização aprova, apresenta resolvida/“Nada a fazer”, motivo pelo display da via; vazia vira “gravada”. Nova segue o resultado do juiz, sem ramo próprio. |
| `app/chamados/services/materializacao.py:674` | Captura via em foto de reversão da absorção; não classifica. |
| `app/chamados/services/passivo_validado.py:64`, `:83` | Captura via em foto reversível; qualquer valor é dado. |
| `app/chamados/services/passivo_validado.py:521` | Guarda via anterior para comparação do resultado da tentativa; nova é dado. |
| `app/chamados/models.py:1921`, `:1930` | Captura via anterior na trilha do fecho por admin. |
| `app/chamados/models.py:2056` | `remover_motivo` exclui quem já é `motivo_retratado`; nova não é excluída. |
| `app/chamados/models.py:2061` | Inclui via anterior em `recusa_motivo`; nova é texto de trilha. |
| `app/chamados/reconciliador.py:1144`, `:1155` | Foto antes/depois do encerramento de fantasma; não classifica. |
| `app/chamados/services/disputa_emissao.py:2050`, `:2065`, `:2068` | Antes/depois e descrição do fecho sem credor; não classifica via lida. |
| `app/chamados/services/relavra_via.py:123`, `:134`, `:136` | Trilha da re-lavra, quaisquer valores anterior/novo. |
| `app/chamados/services/relavra_via.py:245`, `:252` | Trilha de descarimbo, qualquer via anterior. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:101` | Seleciona somente `admin_decidiu` materializada sem validação para criar atos faltantes; nova ignorada. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:314` | Guarda repetida sob trava: fora de `admin_decidiu` ignora. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:330`, `:335` | Foto/trilha da via anterior à retratação. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:398`–`:404` | Diff do plano preserva via qualquer nas intocadas e projeta `duplicata` nas retratadas. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:504` | Carrega campo na consulta das perguntas; comportamento seguinte depende das portas já citadas. |
| `app/chamados/management/commands/relavrar_via_i6.py:58`–`:69` | Fora de `declaracao`/`completude_turno`, contabiliza “via de DECISAO, nao se re-lavra”. Nova cai nesse ramo, ainda que não represente decisão. |
| `app/chamados/management/commands/relavrar_via_i6.py:88` | Conta pares de vias para apresentação. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:67` | Seleciona apenas `fantasma`; nova ignorada. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:129`, `:142`, `:160` | Snapshot/trilha de via, sem classificar. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:33` | Exclui `fato_materializado`; nova entra na consulta. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`–`:43` | Só `''`, `legado_sem_lastro`, `None` podem ser sobrescritas no comando; nova fica intocada. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:49`, `:66`, `:75`, `:76` | Contagem e trilha das vias; sem ramo adicional. |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70`–`:72` | Sem batida fonte: `ausencia_dia`/`declaracao` recusam como “absorcao”; qualquer outra, inclusive nova, recusa como “sem_fonte”. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:47`–`:48` | Piso I6 é união de `VIAS_SEM_TESTEMUNHA` e `VIAS_SEM_LASTRO`; desconhecida fica fora desse piso. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:202`–`:207` | Contraprova F1t exige via não vazia + pergunta morta; rótulos: validação, identidade se em `VIAS_DE_IDENTIDADE`, senão “carimbo/celula”. Nova morta cai no último rótulo. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:433` | Agrupa contagem por qualquer via, nova inclusive. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:542` | Retorna via junto ao PK do universo I6; sem enumeração. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:955` | I9 acusa materializada com via vazia; nova não vazia não acusa aqui. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1710` | Piso F1t admite qualquer via não vazia, desde que C6 declare morta; nova pode entrar. |
| `app/chamados/models.py:3222` | `via_resolucao` consta entre campos que acordam célula; qualquer valor novo altera o mesmo campo. |

**Enumerações fechadas e distinção admin/tranca**

- `VIAS_FORA_DO_FATO=('duplicata','motivo_retratado')`: `app/chamados/models.py:2394`. A classe reexporta em `:2515`; mexer nela muda identidade/dedup/índices, não apenas vitalidade.
- `VIAS_DE_IDENTIDADE=(admin_decidiu, duplicata, fato_ocupado, paridade_recusada, motivo_retratado)`: `app/chamados/models.py:2511`.
- `VIAS_QUE_VENCEM_A_RESPOSTA=(competencia_trancada,)+VIAS_FORA_DO_FATO`: `app/chamados/models.py:2525`. Esta é indispensável para a morte anterior à resposta no C6 e em `q_vivas` (`juizes.py:1412`; `models.py:3353`).
- `VIAS`/choices: lista em `app/chamados/models.py:2526`–`:2543`, campo com `max_length=24` em `:2563`.
- `vias_que_encerram_por_identidade`: soma `competencia_trancada` às identidades, em `app/chamados/models.py:3096`; alimenta C6 (`juizes.py:1437`), SQL (`models.py:3348`) e `_ABSOLVEM_SEMPRE` (`juizes.py:689`).
- `VIAS_SEM_TESTEMUNHA`: `app/chamados/models.py:3733`–`:3764`: `completude_turno`, `fantasma`, `sem_turno_aberto`, `juiz_aprovou`, `admin_decidiu`, `paridade_recusada`, `duplicata`, `fato_ocupado`, `adesao`, `cadastro_folga`, `celula_absolveu`, `ausencia_dia`, `competencia_trancada`, `motivo_retratado`.
- `VIAS_SEM_LASTRO=(chamado_encerrado,legado_sem_lastro)`: `app/chamados/models.py:3769`.
- `VIAS_DERIVADAS=(declaracao,completude_turno)`: `app/chamados/services/relavra_via.py:77`.
- `SEM_HISTORIA=('',legado_sem_lastro,None)`: `app/chamados/management/commands/encerrar_perguntas_com_fato.py:40`.
- Tupla local de absorção `(ausencia_dia,declaracao)`: `app/chamados/services/materializacao.py:632`; tupla igual para motivo de recusa de migração: `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70`.
- `admin_decidiu` é identidade, sem testemunha e possui proteções de mérito/backfill; tranca adicionalmente vence resposta e tem reabertura própria: `models.py:2511`, `:2525`, `:3321`, `:3735`, `:3760`; `fio.py:1244`; `backfill_decisoes_pergunta.py:101`.
- Nova via **não deve ser inserida em todas essas listas por analogia**: cada lista aciona operações diferentes, comprovadas nos leitores acima (`models.py:2756`, `:3266`, `:3314`; `relavra_via.py:118`).

A busca direta pelo campo não encontrou leitores em folha/motor/TXT/inteligência; ausência absoluta de leitura indireta nesses domínios é [nao verificado]. A principal leitura indireta que eles podem receber é a decisão do C6/juiz de materialização, cujos comportamentos estão discriminados acima (`app/chamados/juizes.py:679`, `:1399`).

## 13/11 12:05 · sessao 7b81833cadb3 · - · - · origem=Ronald (tmux)

Q7 — auditoria `desligamento_revisao`, cada sítio:

- **Emissor:** `app/colaboradores/services/desligamento.py:168` cria o chamado associado ao mesmo colaborador, área `dp_rh`, categoria `auditoria_clt`, subcategoria `registrar_resposta_oficial`, módulo `desligamento_revisao`; descrição atual contém demissão, autor, motivo e instrução de responder, mas nenhuma lista de perguntas/respostas (`:170–188`).
- **JSON atual:** `app/colaboradores/services/desligamento.py:182` grava somente `data_demissao`, `motivo_informado`, `executado_por_username`, `executado_por_id`, `executado_em` (`:183–187`).
- **Notificação:** `app/colaboradores/services/desligamento.py:192` envia push ao DP com URL `/chamados/local/<pk>/` (`:195`). Entrega real do push [nao verificado].
- **Rota existente:** `app/chamados/urls.py:14` liga essa URL a `detalhe_chamado_local`.
- **View existente:** `app/chamados/views.py:363` busca o chamado local; gestores podem acessá-lo e não gestores precisam ser o próprio colaborador (`:364–370`); contexto construído por `montar_contexto_detalhe` e renderização de `detalhe_local.html` em `:549–551`.
- **Contexto do detalhe:** `app/chamados/services/acoes_chamado.py:409` entrega `chamado` e `followups` diretamente (`:411–412`). Perguntas mostradas pelo bloco de disputa vêm apenas da disputa **aberta** (`:345–347`) e filtradas por pergunta viva (`:357–358`); esse bloco não recuperaria automaticamente as perguntas mortas para o novo audit.
- **Template detalhe, descrição completa:** `app/templates/chamados/detalhe_local.html:94` exibe `{{ chamado.descricao }}` sem truncamento (`:97`). É um encaixe existente para a lista textual informativa dia/pergunta/resposta.
- **Template detalhe, timeline:** `app/templates/chamados/detalhe_local.html:112` percorre followups. Uma ocorrência informativa também aproveitaria a timeline existente, pela porta `nota.anexar_ocorrencia`, usada no próprio executor em `app/colaboradores/services/desligamento.py:163`.
- **Painel lateral existente:** `app/chamados/views.py:347` entrega chamado/followups ao template `painel_chamado_turno.html` (`:351–355`); `app/templates/chamados/partials/painel_chamado_turno.html:33` percorre genericamente todo `contexto_json.items` (`:35–37`). Lista estruturada no JSON apareceria ali pela representação padrão do valor, sem formatação própria de itens.
- **Modal DP existente:** `app/chamados/views.py:889` monta fio com explicação, renderizado em `:927`; `app/templates/chamados/partials/modal_fio.html:456` mostra descrição truncada em 200 caracteres. Apenas acrescentar a lista à descrição não garante lista completa nesse modal.
- **Modal DP, timeline:** `app/templates/chamados/partials/modal_fio.html:461` percorre followups e imprime conteúdo completo em `:473`.
- **Fio completo DP:** `app/chamados/views.py:677` monta os chamados e renderiza `fio_colaborador.html` em `:697`; `app/templates/chamados/fio_colaborador.html:231` também trunca descrição em 200 caracteres; followups entram em `:235`.
- **Cartão de atendimento:** `app/templates/chamados/partials/_card_atendimento.html:39` mostra descrição truncada em 200 caracteres.
- **Painel do DP:** `app/chamados/views.py:721` usa `montar_contexto_painel` (`:726–738`); `app/chamados/services/painel.py:306` aceita filtro explícito de status. Sem filtro, restringe a `COBRANCA` (`:316`), excluindo `registrado`; o drill `?status=registrado` é documentado junto à regra (`:310–312`).
- **Natureza do audit:** `app/chamados/catalogo/natureza.py:18` inclui `desligamento_revisao` em `MODULOS_AUDITORIA` (`:20`); `eh_acionavel` retorna falso (`:36–38`) e `dono_do_chamado` retorna `registro` antes das perguntas (`:44–45`).
- **Rótulo:** `app/chamados/services/desenlace.py:30` traduz para “Revisao de desligamento”; ramo `registro` fornece “Registro de auditoria” em `:68–71`.
- **Texto humano de registro:** `app/chamados/services/verbos.py:176`: “Desligamento registrado aguardando conferencia de rotina do RH.”
- **Catálogo do dia:** `app/chamados/catalogo/modulos.py:141` declara `data_demissao` como chave de data, horizonte `DIA`, dono `dp`; leitor genérico `app/chamados/juizes.py:100` passa JSON ao catálogo em `:113–114`.
- **Tranca:** `app/chamados/catalogo/modulos.py:265` inclui esse audit em `ATOS_DO_DP_FORA_DA_TRANCA`; `fica_fora_da_tranca` reconhece-o em `:272`, e `app/chamados/models.py:1042` retorna falso para competência trancada nesse caso.
- **App do colaborador:** `app/chamados/services/janela_colab.py:36` considera-o módulo interno; a lista do colaborador o exclui em `:74`.
- **Resposta oficial DP:** `app/chamados/views.py:546` despacha resposta livre; `app/chamados/services/acoes_chamado.py:301` anexa ocorrência e `:322–325` permite transição de status.

**Encaixe sem tela nova:** lista estruturada no `contexto_json` do audit (`desligamento.py:182`) + apresentação no detalhe já existente junto à descrição (`detalhe_local.html:94`) ou ocorrência informativa na timeline (`modal_fio.html:461–473`, `painel_chamado_turno.html:45–56`). Só preencher JSON não cria apresentação estruturada no detalhe/modal atual: o detalhe entrega o objeto (`acoes_chamado.py:411`) e seus usos específicos de JSON no template são outros blocos, enquanto o único loop genérico encontrado é `painel_chamado_turno.html:35`.

**Tensão concreta com o selo:** cascade encerra os chamados preexistentes em `app/colaboradores/services/desligamento.py:155–163`; o audit nasce **depois**, em `:168`. `ChamadoColaborador.criar` deriva evento `REGISTRAR` pela natureza (`app/chamados/models.py:1047–1065`); o motor o converte para `registrado` (`app/chamados/catalogo/motor.py:133–134`). `registrado` pertence tanto a `VIVOS` (`:24`) quanto a `NAO_FECHADOS` (`:32`). Assim, quando o nascimento é permitido, o próprio ato deixa o audit do desligado não fechado; isso conflita com contador literal zero de todos os `NAO_FECHADOS` do desligado. É inferência estática desses ramos, sem contagem de frota [nao verificado].

Q8 — callers e outras portas de situação:

1. **UI:** `app/colaboradores/views.py:1426::desligar_colaborador`; exige `editar_cadastro` (`:1428`), chama juiz (`:1434`) e executor (`:1437`) com `hoje`.
2. **Cron/comando:** `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38` seleciona demissão preenchida até hoje e situação diferente de desligado (`:40`); DRY sem `--apply` (`:46–47`), revalida pelo juiz (`:51–56`) e executa pela mesma porta (`:57`).
3. **Agenda declarada:** `app/config/crons.py:440` cadastra esse comando às `06:06`, com `--apply`, log `desligamento_agendado.log` (`:441`). Execução real do cron [nao verificado].
4. **Agendamento futuro no executor:** `app/colaboradores/services/desligamento.py:87` grava data/motivo (`:90–91`) e retorna (`:106`), sem atribuir `situacao='desligado'`; consumação concreta atribui em `:109` e persiste via `salvar_inteiro` em `:113`.
5. **Outro escritor concreto de desligado:** nenhum encontrado fora de `executar_desligamento` na busca da cópia; ausência universal de execução dinâmica/SQL externo [nao verificado].
6. **Porta genérica capaz de persistir situação:** `app/colaboradores/services/colaborador.py:32` permite o campo na operação `situacao`; `gravar` aplica valores por `setattr` e salva (`:55–61`). Não veta o valor `desligado` nem executa cascade.
7. **Porta genérica com atribuição externa:** `app/colaboradores/services/colaborador.py:65–69::salvar_campos` salva campos previamente atribuídos. Também não veta desligado nem chama cascade.
8. **Porta genérica de objeto inteiro:** `app/colaboradores/services/colaborador.py:73–76::salvar_inteiro` confere apenas nome da operação e chama `save()` completo; pode regravar situação do objeto sem cascade.
9. **Porta genérica bulk:** `app/colaboradores/services/colaborador.py:80–88::atualizar_em_lote` aceita `situacao` pela mesma declaração e faz `qs.update(**campos)`; nenhum caller concreto fornecendo desligado encontrado.
10. **Porta genérica criação:** `app/colaboradores/services/colaborador.py:91–93::criar_colaborador` passa `**campos` ao ORM; pode criar diretamente com desligado se assim chamada, sem cascade. Caller de admissão encontrado fixa ativo em `app/colaboradores/views.py:1208`.
11. **Edição web:** `app/colaboradores/views.py:1314–1341` atribui campos explicitamente, sem situação, e salva inteiro em `:1345`; nenhum POST de situação encontrado nesse caminho.
12. **Django admin:** `app/colaboradores/admin.py:48` herda `SoLeitura`; `app/core/admin.py:59–66` nega adicionar/alterar/excluir e `:68–75` torna campos somente leitura.
13. **Escritor de afastamento, não desligamento:** `app/ponto/views.py:2435` grava `afastado`.
14. **Reversor de afastamento, não desligamento:** `app/ponto/management/commands/reverter_situacao_afastado.py:73` grava `SITUACAO_VOLTA`; constante é `ativo` em `app/ponto/services/afastado_avisa.py:101`.
15. **Reativação:** `app/colaboradores/services/desligamento.py:215` atribui ativo e persiste em `:219`; não é outro desligador.
16. **Saneamento de vínculo de desligado:** `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:35` busca quem **já** está desligado para fechar vínculo; não chama executor nem grava desligado.
17. **Outros callers de produção do executor, ModelForm/serializer gravando situação, importador gravando desligado ou `.update/bulk_update` concreto com desligado:** não encontrados nas buscas; execução externa à cópia e estado real de produção [nao verificado].

## 13/11 12:04 · sessao 26f32bc54fa1 · - · - · origem=Ronald (tmux)

Censo Q5/Q6, somente leitura, com caminhos relativos à raiz da cópia.

**Q5 — guarda de pergunta viva e campo que mata**

- `ChamadoColaborador.encerrar()` consulta o motor para `ENCERRAR`, devolve `False` se a transição não é permitida e depois consulta `_gate_pergunta_viva`; se o gate recusa, retorna `False` antes de gravar estado. O docstring “sem guard” está desatualizado em relação ao corpo. `app/chamados/models.py:1606-1621`.
- `_ancoradas()` busca perguntas **não validadas**, ancoradas por `chamado_destino=self` ou, sem destino, pelo chamado pai da disputa. `app/chamados/models.py:336-347`.
- `_vivas_entre()` simula o chamado no estado de destino; seleciona perguntas ainda ancoradas, cuja competência é aberta (`competencia_do_dia_entregue(...) is False`) e cujo juiz `pergunta_viva` retorna verdadeiro. `app/chamados/models.py:349-372`.
- O gate só atua ao sair de `VIVOS` para `TERMINAIS`; sem perguntas vivas libera; se todas puderem ser reatadas a outra cobrança válida do mesmo colaborador/dia, reata e libera; caso contrário recusa com trilha `fechamento_recusado`, motivo `pergunta_viva_na_competencia_aberta`, IDs das perguntas e estados de origem/destino. `app/chamados/models.py:374-395`, `app/chamados/models.py:410-445`.
- `pergunta_viva` é a porta com memo por instância, cujo cálculo está em `_pergunta_viva_calc`; a property `PerguntaDisputa.viva` delega diretamente a ela. `app/chamados/juizes.py:862-889`, `app/chamados/models.py:3365-3376`.
- **`validada_em` preenchido mata imediatamente** no primeiro ramo do juiz. `via_resolucao` preenchida, sozinha, não mata. `app/chamados/juizes.py:1399-1413`.
- A tranca do dia, por leitura da autoridade, mata antes de considerar resposta; depois, `via ∈ VIAS_QUE_VENCEM_A_RESPOSTA` com `materializada_em` preenchido também mata antes da resposta. A tupla atual é `(competencia_trancada,) + VIAS_FORA_DO_FATO`. `app/chamados/juizes.py:1403-1413`, `app/chamados/models.py:2517-2525`.
- Resposta com `respondida_em` e texto continua viva se o admin não decidiu o dia e não existe luz concordante na célula; uma via desconhecida com carimbo **não vence esse ramo**. `app/chamados/juizes.py:1414-1429`.
- Depois da resposta, vias retornadas por `vias_que_encerram_por_identidade()` com `materializada_em` matam; outros ramos examinam fato do marco, veredito da célula, falta do dia e lastro externo. Portanto não há um único campo universal: validação mata de imediato; fechamento terminal sem validação precisa de via reconhecida no degrau adequado e carimbo de materialização. `app/chamados/juizes.py:1430-1489`.
- `q_vivas()` é o pré-filtro SQL amplo; exclui validadas e vias que vencem a resposta com materialização, antes de aceitar resposta preenchida. A via nova deve respeitar a mesma fonte usada pelo juiz para não haver divergência. `app/chamados/models.py:3327-3363`.

**Q6 — leitores da resposta com efeito no dia/dinheiro**

1. **Roteador no próprio ato da resposta:** `responder_pergunta_disputa_core` consulta `ESCAPES_QUE_VIRAM_AUSENCIA` e chama `_materializar_ausencia_de_resposta`; `FALTA_DIA → falta/aprovada`, `ATESTADO → atestado/status decidido por estado_inicial`. Isso ocorre quando o colaborador responde, independentemente de uma futura via de desligamento. `app/chamados/services/disputa_emissao.py:1536-1541`, `app/chamados/services/materializacao.py:600-606`.

2. **Materialização de ausência pela resposta:** cria a `Ausencia` pela porta `criar_ausencia`, usando data da pergunta, tipo/status recebidos do roteador; não inventa minutos fixos. Também tenta carimbar irmãs do dia e registra absorção. Uma ausência já criada por esse caminho é fato anterior ao desligamento; conservar ou reverter esse fato por uma via futura é [nao verificado]. `app/chamados/services/materializacao.py:502-535`, `app/chamados/services/materializacao.py:538-579`.

3. **Validação humana de folga:** `_e_aceite_de_folga` compara a resposta com `RESPOSTA_DE_FOLGA`; `_aceitar_a_folga` consulta `checar_declaracao`, carimba validação e chama `declarar_dia(..., tipo='folga')`; em sucesso, fecha a pergunta por admin; em recusa, mantém/desfaz o ato. Não é a morte da pergunta que declara a folga. `app/chamados/services/validacao.py:234-236`, `app/chamados/services/validacao.py:305-343`.

4. **Validação humana geral:** `validar_pergunta` exige `respondida_em`, recusa já validada, consulta plausibilidade, carimba validação e dispara materialização/recalculo. A função não consulta `pergunta_viva` nem recusa uma via de desligamento inexistente antes de validar: um futuro acesso direto a uma pergunta morta ainda precisaria ser auditado conforme a implementação. `app/chamados/services/validacao.py:389-413`, `app/chamados/services/validacao.py:425-469`.

5. **Seleção para materialização normal:** `materializar_perguntas_validadas_da_disputa` seleciona `respondida_em != NULL`, `validada_em != NULL` e `materializada_em == NULL`; somente essas passam à unidade `_materializar_uma`. Uma pergunta encerrada com `materializada_em` e sem validação não entra nesse lote. `app/chamados/services/materializacao.py:1375-1385`.

6. **Unidade de materialização:** escolhe implementação por `tipo_resposta` (`hora`, `data_hora`, `escolha`); para hora/data_hora exige aprovação do juiz do lastro; reprovação levanta dentro do atomic e desfaz o plantio. Não transforma via desconhecida em aprovação automaticamente nesse despacho. `app/chamados/services/materializacao.py:1224-1249`.

7. **Derivação de tipo pelo texto:** `_tipo_pelo_marco` lê a hora da resposta, compara aos marcos da grade e resolve tipo `E/S`; divergência entra em observabilidade. `app/chamados/services/materializacao.py:274-310`.

8. **Derivação de instante:** `_ts_tipo_do_plantio` recusa respostas maiúsculas/`[descartado`, criva defaults de relógio incompatíveis com a régua, deriva tipo e usa a hora respondida; em intervalos fixos pode usar a hora do marco; depois ajusta overnight. É derivação reutilizada tanto para leitura quanto para plantio. `app/chamados/services/materializacao.py:354-428`.

9. **Plantio de hora:** `_materializar_resposta_hora` exige texto; sem ele grava recusa e não planta; com texto deriva instante, reusa batida existente ou passa pelas guardas de plantio. A simples morte da pergunta não chama esta função. `app/chamados/services/materializacao.py:790-800`, `app/chamados/services/materializacao.py:835-865`, `app/chamados/services/materializacao.py:871-888`.

10. **Plantio de data+hora/âncora:** `_materializar_resposta_data_hora` interpreta `resposta_colab` como data/hora, chama `registrar_batida` tipo `E`, já aprovada, e não escreve fase do vínculo. `app/chamados/services/materializacao.py:984-1035`.

11. **Resposta de escolha/falta declarada:** `_materializar_resposta_escolha` chama `_declarar_ausencia_de_pergunta` em `motivo='declara_falta'`; esse leitor usa `_DECLARA_FALTA_MAPA.get(resp)`, cria ausência pelo tipo reconhecido e ignora resposta desconhecida/“Compareci” quando o mapa retorna `None`. `app/chamados/services/materializacao.py:468-499`, `app/chamados/services/materializacao.py:1069-1086`.

12. **Recusa humana é ato próprio:** `recusar_pergunta` exige motivo, recusa já validada, grava `recusa_motivo`, chama reabertura com ato `recusa`, limpa resposta/carimbos pela porta e conserva texto anterior na trilha. Uma via de desligamento preenchida não equivale, por si só, a esse ato de recusar. `app/chamados/services/validacao.py:561-605`.

13. **Juiz de slot/lastro:** `_slot_esperado` lê diretamente a resposta em `data_hora` e delega a `_ts_tipo_do_plantio` para `hora`; `_pergunta_materializada_de_fato` confronta isso com batida real/slot. Pergunta `escolha` com `materializada_em` retorna `True`, mesmo sem validação, pelo ramo próprio. Uma nova via sem tratamento pode, portanto, cair em lastro **positivo para escolha**, mas isso não cria uma ausência/batida; é leitura de resolução da pergunta. `app/chamados/juizes.py:341-374`, `app/chamados/juizes.py:662-708`, `app/chamados/juizes.py:752-761`.

14. **Concordância resposta/luz:** `_resposta_concorda_com_a_luz` compara hora respondida com luz do marco pela régua classe A e exige célula sem veredito acusatório; isso pode dispensar validação quando o fato já está no espelho, não cria o fato a partir da resposta. `app/chamados/juizes.py:1342-1375`, `app/chamados/juizes.py:1425-1429`.

15. **Classificação de resposta para validar:** `delta_da_resposta` compara hora do texto com marco esperado; `classificar_resposta_validacao` classifica escolha afirmativa como A, outras escolhas como B, e horas pelo delta; `motivo_implausivel_validacao` usa a mesma resposta para a recusa de implausibilidade. Não decidem dinheiro diretamente; orientam a validação que pode materializar fato. `app/chamados/juizes.py:1846-1878`, `app/chamados/juizes.py:1952-1993`.

16. **Classificação de lote:** `classe_do_lote` classifica escape/posto/confirmacão sem marco em C, texto ilegível em D, sem marco em grupo próprio, horas fora do envelope/conflito em B, demais em A. A fila global de IA/lote usa `q_vivas` e confirma `pergunta_viva`, portanto uma pergunta reconhecidamente morta sai dessa fila. `app/chamados/services/validacao.py:845-877`, `app/chamados/juizes.py:1678-1684`.

17. **Materializador ao fechar disputa (S144):** percorre perguntas ainda sem `materializada_em`, exige `_pergunta_materializada_de_fato` e tenta carimbar `VIA_JUIZ_APROVOU`; não fabrica batida. Se a porta de desligamento já carimbar materialização, a pergunta não entra neste laço. `app/chamados/services/materializacao.py:1414-1438`.

18. **UI que diria falsamente “aplicada”: ponto crítico.** `estado_aplicacao_pergunta` ignora perguntas que não sejam `hora` ou não tenham texto. Havendo `materializada_em`, só distingue `VIA_PARIDADE_RECUSADA`; para **qualquer outra via, inclusive uma nova desconhecida, retorna `aplicada`**. O carregador do fio atribui esse resultado a `_p.estado_aplicacao`. Esse leitor precisa distinguir a via de desligamento para não apresentar encerramento informativo como hora aplicada. `app/chamados/services/materializacao.py:431-448`, `app/chamados/services/fio.py:334-338`.

19. **Prévia da folha:** carrega todas as perguntas do colaborador/dia, sem filtro de vida/validação; `_material` devolve `'resposta do colaborador'` se houver texto, antes de consultar `fio_vivo`. Uma pergunta morta por desligamento continua aparecendo como material disponível, mas a função é de leitura e não aplica resposta. `app/folha/previa.py:28-31`, `app/folha/previa.py:63-67`, `app/folha/previa.py:139-146`.

20. **Cartório, proteção contra ressuscitação automática:** `_tem_resposta_do_colab` olha apenas texto; o laço de fio morto pula chamado com qualquer resposta, preservando-o para materializador/admin; o mesmo predicado é reutilizado no dedup. A via nova não muda essa isenção enquanto o texto permanecer. `app/ponto/services/cartorio.py:1637-1650`, `app/ponto/services/cartorio.py:1661-1666`.

21. **Cartório, efeito indireto da morte do fio:** conta chamados vivos com `fio_vivo` e entrega `cobrancas_vivas` ao `classificar_dia`. O suprajuiz usa ausência de cobrança viva em ramos de protesto do dia. Matar pergunta/chamado pode mudar o diagnóstico em relavra; não é aceitação nem recusa da resposta. `app/ponto/services/cartorio.py:1384-1389`, `app/ponto/services/cartorio.py:1464-1470`, `app/ponto/supra_juiz.py:313-316`, `app/ponto/supra_juiz.py:454-456`.

22. **Retenção de export/TXT:** `motivos_retencao_celula` lê veredito/protestos da célula, não a resposta da pergunta; retém célula acusatória salvo os protestos isentos e a regra da pausa. Logo não se deve afirmar “zero efeito possível na folha” apenas por não validar: o fio participa indiretamente dos protestos, e os protestos participam da retenção. Efeito concreto no valor/TXT do caso futuro [nao verificado]. `app/folha/export.py:1176-1184`, `app/folha/export.py:1232-1266`.

23. **Saída de rescisão do TXT:** `classificar_export` põe fora do TXT quem `sai_pela_rescisao`, com motivo `rescisao_modulo_proprio`; demais competências continuam pelos crivos seguintes. Não usa a resposta como fato aceito/recusado nesse gate. `app/folha/export.py:851-875`.

24. **Consulta de dia do colaborador:** expõe a resposta e `ainda_viva=pergunta_viva(p)` separadamente; não infere aceite a partir de resposta/morte. `app/ponto/services/dia_do_colab.py:62-71`.

**Conclusão delimitada para Q6:** encerrar com `materializada_em`, via própria e `validada_em=NULL` evita o lote normal de plantio; não equivale ao ato humano de aceite ou recusa. Entretanto existem leituras que confundiriam encerramento com aplicação (`estado_aplicacao_pergunta`), lastro de `escolha` que aceita o carimbo, material informativo da prévia que continua reconhecendo o texto e participação indireta do fio nos protestos/retencão. `app/chamados/services/materializacao.py:1375-1379`, `app/chamados/services/materializacao.py:438-448`, `app/chamados/juizes.py:706-707`, `app/folha/previa.py:139-146`, `app/ponto/services/cartorio.py:1389`, `app/folha/export.py:1241-1264`.

## 13/11 12:03 · sessao c959e78cd199 · DESLIGAR-LIQUIDA-TUDO · sonda · origem=Code

Não executei: `sonda1.py` não foi localizado na cópia. Transcrevo integralmente a saída fornecida, sem reexecução. [nao verificado] [nao verificado]

```text
sonda_leitura: sonda1.py [6159c0e80e54cd34] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

desligados 388
== (a) disputas abertas de desligado 26 {'emp2 dem-comp 08/2026': 5, 'emp2 dem-comp 09/2026': 4, 'emp2 dem-comp 10/2026': 5, 'emp2 dem-comp 11/2026': 4, 'emp3 dem-comp 09/2026': 1, 'emp3 dem-comp 10/2026': 1, 'emp3 dem-comp 11/2026': 3, 'emp4 dem-comp 02/2026': 1, 'emp4 dem-comp 11/2026': 2}
== (b) com respostas_sem_veredito 7 respostas 39 {'emp2 dem-comp 10/2026': 1, 'emp2 dem-comp 11/2026': 4, 'emp3 dem-comp 11/2026': 2}
   col373 dem=2026-10-31 disputas=[(3676, 1)]
   col427 dem=2026-10-28 disputas=[(5149, 16)]
   col457 dem=2026-09-25 disputas=[(2828, 1)]
   col752 dem=2026-10-29 disputas=[(6680, 2)]
   col769 dem=2026-11-03 disputas=[(5100, 4)]
   col893 dem=2026-11-03 disputas=[(4271, 14)]
   col981 dem=2026-10-27 disputas=[(7395, 1)]
== (c) perguntas vivas de desligado 598 com resposta 0 em disputa FECHADA 58 {'emp2 comp 09/2026': 275, 'emp2 comp 10/2026': 130, 'emp2 comp 11/2026': 126, 'emp3 comp 09/2026': 7, 'emp3 comp 10/2026': 19, 'emp3 comp 11/2026': 33, 'emp4 comp 09/2026': 1, 'emp4 comp 10/2026': 3, 'emp4 comp 11/2026': 4}
    col42:4 col104:1 col138:1 col143:1 col270:4 col373:38 col388:1 col393:2 col417:3 col422:4 col427:124 col432:3 col437:38 col457:1 col460:21 col510:4 col518:5 col559:15 col569:6 col659:15 col666:13 col669:1 col698:5 col718:1 col752:18 col769:32 col782:55 col863:1 col878:2 col886:12 col893:150 col906:6 col925:4 col951:1 col963:2 col973:2 col981:2
== (d) chamados nao fechados de desligado 396 {'emp1 registrado desligamento_revisao': 1, 'emp2 aberto batida_ausente': 25, 'emp2 aberto disputa_supervisao': 1, 'emp2 aberto sem_previsao_escala': 1, 'emp2 em_analise batida_ausente': 1, 'emp2 em_analise desligamento_revisao': 24, 'emp2 em_analise disputa_supervisao_manual': 2, 'emp2 em_analise sem_previsao_escala': 2, 'emp2 registrado desligamento_revisao': 88, 'emp2 resolvido ancora_limbo': 1, 'emp2 resolvido desligamento_revisao': 63, 'emp2 resolvido disputa_supervisao': 7, 'emp29 resolvido desligamento_revisao': 2, 'emp3 aberto batida_ausente': 6, 'emp3 em_analise batida_ausente': 4, 'emp3 em_analise desligamento_revisao': 4, 'emp3 registrado desligamento_revisao': 25, 'emp3 resolvido ancora_limbo': 9, 'emp3 resolvido desligamento_revisao': 22, 'emp3 resolvido disputa_supervisao': 1, 'emp3 resolvido disputa_supervisao_manual': 2, 'emp4 aberto batida_ausente': 1, 'emp4 em_analise batida_ausente': 3, 'emp4 em_analise desligamento_revisao': 3, 'emp4 registrado desligamento_revisao': 2, 'emp4 resolvido ancora_limbo': 1, 'emp4 resolvido batida_ausente': 40, 'emp4 resolvido chamados': 2, 'emp4 resolvido desligamento_revisao': 4, 'emp4 resolvido disputa_supervisao': 3, 'emp4 resolvido disputa_supervisao_manual': 20, 'emp4 resolvido entrada_bloqueada_turno_aberto': 10, 'emp4 resolvido intervalo_pendente': 2, 'emp4 resolvido justificativa': 6, 'emp4 resolvido turno_aberto': 8}
    col31:3 col34:78 col35:1 col42:2 col46:1 col47:12 col48:1 col52:1 col57:1 col58:1 col64:1 col65:1 col68:2 col71:1 col72:1 col73:1 col74:1 col77:1 col79:1 col86:1 col88:1 col90:1 col91:1 col96:1 col102:1 col103:1 col104:1 col105:1 col122:1 col124:1 col126:1 col132:1 col135:1 col138:1 col140:1 col143:1 col147:1 col153:1 col156:2 col158:1 col160:1 col162:1 col164:1 col167:3 col172:1 col175:1 col176:1 col178:1 col179:1 col180:1 col182:1 col187:1 col208:1 col223:1 col228:1 col229:1 col240:1 col244:1 col270:1 col273:1 col295:1 col299:1 col305:1 col308:1 col311:1 col314:1 col315:1 col316:1 col318:1 col325:1 col330:1 col333:1 col338:1 col343:1 col350:1 col352:1 col353:1 col356:1 col367:1 col370:1 col372:1 col373:9 col375:1 col378:1 col381:1 col385:1 col386:1 col388:1 col393:1 col395:1 col397:1 col400:1 col403:1 col408:1 col409:1 col412:1 col417:1 col422:1 col426:1 col427:3 col429:1 col430:1 col432:1 col433:1 col437:2 col441:1 col443:1 col447:1 col448:1 col457:1 col458:1 col460:1 col462:1 col463:1 col471:1 col480:1 col481:1 col484:1 col486:1 col492:1 col494:1 col498:1 col506:1 col508:1 col509:1 col510:4 col513:1 col518:1 col519:1 col520:1 col525:1 col528:1 col535:1 col543:1 col544:1 col545:1 col554:1 col555:1 col559:1 col568:1 col569:1 col571:1 col576:1 col581:1 col593:2 col606:1 col609:1 col613:1 col621:1 col623:1 col628:1 col630:1 col640:1 col641:1 col643:1 col651:1 col652:1 col653:1 col656:1 col659:1 col660:1 col661:1 col663:1 col666:1 col669:1 col676:1 col697:1 col698:1 col699:1 col703:1 col705:1 col715:2 col718:1 col720:1 col723:1 col728:1 col729:2 col730:1 col731:2 col738:2 col740:2 col742:1 col743:1 col744:1 col745:1 col746:1 col752:1 col753:1 col754:1 col755:1 col759:1 col760:1 col761:1 col769:11 col778:1 col782:2 col783:1 col786:1 col815:1 col816:1 col817:1 col818:1 col828:1 col831:1 col837:1 col840:1 col842:2 col846:1 col847:1 col853:1 col855:1 col856:1 col858:1 col862:1 col863:1 col870:1 col875:1 col878:1 col880:1 col886:1 col890:1 col893:21 col894:1 col901:1 col904:1 col906:1 col907:1 col912:1 col914:1 col925:1 col927:1 col929:1 col931:1 col937:1 col939:1 col942:1 col943:1 col946:1 col951:1 col954:1 col955:1 col959:1 col963:1 col970:1 col973:1 col977:1 col981:1 col984:1 col997:1
== vivos_de_desligado (a)+(c)+(d) = 1020 colabs distintos 249
ERROS 0
```

(a) 26 disputas abertas de desligado. [nao verificado]  
(b) 7 com respostas_sem_veredito; 39 respostas. [nao verificado]  
(c) 598 perguntas vivas; com resposta 0; em disputa FECHADA 58. [nao verificado]  
(d) 396 chamados não fechados de desligado. [nao verificado]  
vivos_de_desligado: 1020; colaboradores distintos: 249; ERROS 0; nenhum traceback no dado fornecido; causa de erro não apresentada. [nao verificado]

## 13/11 11:48 · sessao 942025444273 · O50-DESLIGAR-ENCERRA-TUDO · censo · origem=Code

**Há leitores que alcançam folha e fio.** Os caminhos comprovados incluem retenção/TXT, universo do fechamento, emissão e reabertura de perguntas, adesão e roteamento do `vigia_retido_calado`. Portanto, existe o achado que aciona a exceção temporal descrita no aval anexado. (`app/folha/export.py:1208`, `app/ponto/services/fechamento.py:440`, `app/chamados/services/fabrica_da_celula.py:112`, `app/chamados/models.py:1006`, `app/chamados/services/adesao.py:67`, `app/folha/vigia_retido_calado.py:43`; prazo do aval: [não verificado na árvore])

O inventário abaixo contém **260 sítios**, contando uma query multilinha como um sítio e contando separadamente os campos quando o mesmo sítio usa dois. Incluí seletores de tela, diagnósticos e escolhas de escala para apresentar o perímetro completo; a coluna de impacto distingue os que chegam às travas. As contagens são a consolidação da AST executada em memória, sem arquivo de resultado persistido. [não verificado por artefato persistido]

O método foi: `ast.parse` dos `.py`; captura de `Attribute` em contexto `Load`, keywords ORM por segmentos de `__`, strings de lookup, `getattr`, dicionários e `values_list`; resolução dos imports e acompanhamento das variáveis que transportam os campos; depois triagem semântica e grep dos templates. Exemplos que exigiram isso: `_ECtv`, `_Qc`, `_EC`, `_Col`, `_Esc`, `_ColU`, a tupla `adm/dem/sit` e `_dem`. (`app/folha/export.py:805`, `app/ponto/services/fechamento.py:428`, `app/ponto/services/triagem_batida.py:40`, `app/colaboradores/views.py:2606`, `app/holerite/views.py:280`, `app/relatorios/views.py:1543`, `app/holerite/matriz.py:87`, `app/escala/services/leitor_celula.py:365`)

A execução local percorreu 889 arquivos Python elegíveis, sem erro de parsing; não importou Django nem consultou banco/log. Nenhum arquivo foi criado, editado ou apagado. [não verificado por artefato persistido]

Para ler as tabelas:

- **S** = `Colaborador.situacao` **atual**; **D** = `data_demissao`; **A** = `EscalaColaborador.ativa`. O modelo também admite `ferias`, por isso os dois filtros exclusivos desse valor aparecem no inventário. (`app/colaboradores/models.py:295`, `app/colaboradores/models.py:323`, `app/colaboradores/models.py:325`)
- **F/Fio** contém **sim/não**, nessa ordem. `não/não` identifica apresentação, relatório ou diagnóstico sem caminho de trava no fluxo indicado; efeitos adicionais não demonstrados são [não verificado].
- **CE S**: situação atual desligado/afastado com vínculo válido em `d`; numa consulta histórica, `d` pode ser anterior à mudança. Em tela exclusivamente atual, basta o cadastro atual discordar da vigência que cobre hoje. A autoridade não consulta `situacao`. (`app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:285`)
- **CE A**: vínculo `ativa=False`, com `data_inicio ≤ d ≤ data_fim`, é omitido pelo filtro ativo e aceito pela autoridade; inversamente, `ativa=True` com início futuro pode ser escolhido pelo filtro seco. (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:282`)
- **CE D**: demissão em 10/11, vínculo ainda cobrindo 11/11: o corte pela demissão retira o dia e o vínculo pode aceitá-lo. **CE B**: `d = demissão = data_fim`; a vigência inclui a igualdade. São hipóteses de código; ocorrência em produção é [não verificado]. (`app/escala/alimentacao.py:215`, `app/escala/alimentacao.py:282`)

Os caminhos reutilizados nas tabelas são:

| Caminho | Destino comprovado |
|---|---|
| **TXT** | `classificar_export` determina os participantes do TXT. (`app/folha/export.py:714`, `app/folha/porta_export.py:469`) |
| **RET** | `motivos_retencao_celula` → `motivos_retencao_espelho` → `classificar_export` → TXT. (`app/folha/export.py:1275`, `app/folha/export.py:968`, `app/folha/export.py:716`) |
| **UF** | Universo da competência → recálculo do fechamento. (`app/ponto/services/fechamento.py:445`, `app/ponto/services/fechamento.py:554`) |
| **CEL** | Regeneração → julgamento dos dias tocados, recálculo e reavaliação/rejunção de chamados. Assim, a seleção de quem recebe a correção pode preservar uma célula que retém ou um fio incorreto. (`app/ponto/portas/celula.py:392`, `app/ponto/portas/celula.py:472`, `app/ponto/portas/celula.py:496`, `app/ponto/portas/celula.py:536`, `app/ponto/portas/celula.py:539`) |
| **GRADE** | Grade → dias internos do fechamento; `fora_vinculo` é excluído. Na emissão, esse tipo também retira o dia de trabalho. (`app/folha/export.py:338`, `app/ponto/services/fechamento.py:1043`, `app/chamados/services/disputa_emissao.py:83`) |

**Folha: fechamento, exportação, retenção, retrato e TXT.**

| ID | Sítio | Pergunta efetiva e predicado | F/Fio; caminho | Divergência |
|---|---|---|---|---|
| F01 | `app/folha/export.py:60` D | Sai pela rescisão? D não nula e **D ≤ fim da janela**. | **sim/não**; classificação rescisão em `app/folha/export.py:866` → TXT. | CE D; decide rota financeira mesmo havendo vínculo. |
| F02 | `app/folha/export.py:807`, `app/folha/export.py:810` A+S | Trava por âncora? A=True, 12x36, âncora nula, S=ativo, participante dos fechamentos. | **sim/não**; `sem_ancora_12x36` em `app/folha/export.py:835` → aprovação em `app/ponto/views.py:1296`. | CE S/A. |
| F03 | `app/folha/export.py:1068` D | Com previsto zero, chama de rescisão se **D < início da janela**; senão `sem_escala`. | não/não; motivo de prontidão retornado em `app/folha/export.py:1070`. | CE D. |
| F04 | `app/folha/export.py:1087` D | Prontidão: reduz `fim` se **D < fim**; se `fim < ini`, retorna rescisão. A grade posterior é pedida separadamente. | não/não; retorno em `app/folha/export.py:1089`, grade em `app/folha/export.py:1095`. | CE D; o corte pode colapsar a janela. |
| F05 | `app/folha/export.py:1208` D | Quais dias podem reter? Corta a janela até D **inclusive**; D anterior ao início produz vazio. | **sim/sim**; RET, também fornece os dias da folha ao vigia de alcance. (`app/folha/fora_da_folha.py:119`) | CE D. |
| F06 | `app/folha/export.py:1237` D | No próprio dia da demissão, célula ausente não retém por `sem_celula`; exige `d < hoje AND d != D`. | **sim/sim**; RET e dias da folha. | CE B: vínculo existe em D, mas a ausência de célula recebe exceção. |
| F07 | `app/folha/views.py:612` D | Raio-X considera furos até D inclusive; D anterior à janela elimina o trecho. | não/não; linhas de relatório em `app/folha/views.py:616`. | CE D. |
| F08 | `app/folha/services/ranking_he.py:133` A | Entra no filtro de ciclo do ranking conforme EC ativa atual. | não/não; lista em `app/folha/services/ranking_he.py:144`. | CE A. |
| F09 | `app/folha/views_retrato.py:107` A | Qual posto/praça/escala aparece no retrato? Última EC ativa. | não/não; enriquecimento em `app/folha/views_retrato.py:110`. | CE A. |
| P20 | `app/ponto/portas/celula.py:908` A | Materializa o dia? Após `vinculo_cobre_a_data`, EC inativa é pulada se **d ≤ hoje**, ou se não for a escolhida no futuro. | **sim/sim**, via célula criada em `app/ponto/portas/celula.py:914` e seus leitores RET/FIO. | CE A: vínculo encerrado válido hoje é aceito pelo juiz e pulado aqui. |
| P21 | `app/ponto/portas/regen_exportada.py:180` A | Estende horizonte da regeneração somente se A for verdadeira. | **sim/sim**; regeneração e recálculo em `app/ponto/portas/regen_exportada.py:179`, `app/ponto/portas/regen_exportada.py:184`; CEL. | CE A. |
| P34 | `app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:440` S+D | Entra na competência se **S != desligado OR (S=desligado AND D ≥ início da janela da empresa)**. Sem limite superior; desligado com D nula não entra. | **sim/não**; UF. | Desligado com D nula e EC válido sai; ativo sem vínculo pode entrar. |
| P35 | `app/ponto/services/fechamento_lista.py:60` A | Qual EC rotula a linha do fechamento? A=True, mapa por colaborador. | não/não; classificação TXT é separada em `app/ponto/services/fechamento_lista.py:70`. | CE A. |
| P39 | `app/ponto/services/he_pendente_lavrado.py:92` S | Quem recebe foto de HE pendente na competência? S=ativo. | **sim/não**, condicional: foto → alvos de recusa automática em `app/ponto/management/commands/recusar_ponta_pequena.py:113`, `app/ponto/management/commands/recusar_ponta_pequena.py:170`; pendência viva pode reter TXT quando o portão está ligado. (`app/folha/porta_export.py:495`, `app/folha/porta_export.py:853`) | CE S; a foto pode omitir quem ainda tem dia pendente. |
| C35 | `app/escala/services/leitor_celula.py:389` D | Dia fica fora se **d > D OR (d = D AND não houve fato)**; há também ramo anterior à admissão. `_teve_fato` usa minutos realizados ou lâmpadas não missing. | **sim/sim**; vira `fora_vinculo` em `app/escala/services/leitor_celula.py:393` → GRADE. | CE B: D=data_fim, sem fato; o juiz inclui D e a grade exclui. |

O último sítio **não é o corte atual de retenção por célula**: sua influência comprovada é a grade/universo de dias do fechamento e da emissão. O retentor atual consulta células diretamente e tem os cortes próprios F05/F06. (`app/ponto/services/fechamento.py:1043`, `app/folha/export.py:1275`)

**Chamados, perguntas, adesão e decisões do fio.**

| ID | Sítio | Pergunta efetiva e predicado | F/Fio; caminho | Divergência |
|---|---|---|---|---|
| T03 | `app/colaboradores/queries.py:26` S | Universo comum: S=ativo, empresa ativa e usuário operacional. `em_uso` e `em_operacao` herdam esse corte atual. | **não/sim**; `em_operacao` em `app/colaboradores/queries.py:65` → detector em `app/ponto/management/commands/detectar_ausencias.py:100` → abertura em `app/ponto/management/commands/detectar_ausencias.py:694`. `em_uso` também alimenta alertas de turno. (`app/ponto/management/commands/processar_alertas_turno.py:50`, `app/ponto/management/commands/processar_alertas_turno.py:335`) | CE S, inclusive para ontem. |
| T07 | `app/colaboradores/services/vinculo.py:91` S | Pode receber escala? S=desligado veta o julgamento. | **não/sim**; impede o executor e o ramo de aceite que cria chamado. (`app/colaboradores/services/vinculo.py:390`, `app/colaboradores/services/vinculo.py:410`) | S desligado atual bloqueia pedido referente a início histórico com vínculo válido. |
| P24 | `app/ponto/services/afastado_avisa.py:41` S | Sem ausência confirmada pelo juiz, **S=afastado ainda devolve True**, com `situacao_sem_ausencia`. | **não/sim**; a triagem usa o resultado; o vigia também o interpreta como porta `afastado`. (`app/ponto/services/furos_sem_cobranca.py:90`, `app/ponto/services/furos_sem_cobranca.py:95`) | S=afastado, EC válido, nenhuma ausência cobrindo d. |
| P26 | `app/ponto/services/cartorio.py:1748` S | Emite furo/cobrança morta/limbo apenas se **S != desligado** e emissão não calada. | **não/sim**; guarda da emissão em `app/ponto/services/cartorio.py:1765`. | CE S. |
| P27 | `app/ponto/services/cartorio.py:1813` S | Emite furo parcial se FURO_PARCIAL, **S != desligado** e dentro do piso. | **não/sim**; `_abrir_chamado` em `app/ponto/services/cartorio.py:1828`. | CE S. |
| P28 | `app/ponto/services/cluster_espurio.py:183` S | Reconcilia cluster somente se S=ativo. | **não/sim**; fechamento ou abertura em `app/ponto/services/cluster_espurio.py:189`, `app/ponto/services/cluster_espurio.py:196`. | CE S. |
| P29 | `app/ponto/services/cluster_espurio.py:185` A | Reconcilia cluster somente com A=True **e data_fim nula**. | **não/sim**; mesmo caminho de P28. | EC datada válida é excluída, mesmo que A=True. |
| P36 | `app/ponto/services/furos_sem_cobranca.py:62` S | Vigia enxerga furo da janela atual até ontem somente de S=ativo. | **não/sim**; universo do vigia antes dos testes de chamado vivo/tranca. (`app/ponto/services/furos_sem_cobranca.py:83`) | CE S. |
| P38 | `app/ponto/services/geofence.py:46` S | Pode julgar divergência do vínculo GPS? S diferente de ativo retorna `None`. | **não/sim**; `None` é conservador e pode manter chamado vivo na reconciliação. (`app/ponto/services/geofence.py:67`, `app/chamados/reconciliador.py:300`) | CE S; histórico deixa de ser julgável. |
| P40 | `app/ponto/services/pautas_dos_discordantes.py:70` S | Quais dias discordantes sem fio entram na pauta da competência? Somente S=ativo. | **não/sim**; corta antes de `_fio_do_dia`. (`app/ponto/services/pautas_dos_discordantes.py:75`, `app/ponto/management/commands/pautas_discordantes.py:67`) | CE S. |
| P42 | `app/ponto/services/triagem_batida.py:92` A | Existe escala para acesso? Qualquer A=True; ausência gera diagnóstico de sem escala. | **não/sim**; cria chamado `acesso_bloqueado`. **A batida continua permitida.** (`app/ponto/services/triagem_batida.py:107`, `app/ponto/services/triagem_batida.py:123`) | CE A. |
| P44 | `app/ponto/services/vigia_de_hora.py:117` S | Vigia de hoje/ontem pula S=desligado. | **não/sim**; impede `julgar_celula` e emissão. (`app/ponto/services/vigia_de_hora.py:131`) | CE S, sobretudo ontem. |
| P47 | `app/ponto/services/vinculo_na_lista.py:152` S | Continua em revisão se **S != desligado** e posto continua igual ao da lavra. | **não/sim**, no ramo de geofence: revisão troca acusação/push por registro de divergência cadastral. (`app/ponto/services/geofence.py:77`, `app/ponto/services/geofence.py:308`, `app/ponto/services/geofence.py:319`) | CE S. |
| P48 | `app/ponto/supra_juiz.py:194` D | **Depois de o juiz devolver vínculo=None**, célula de trabalho só acusa cadastro se **d ≤ D ou D nula**. | **sim/sim**; insumo do classificador usado pelo cartório. (`app/ponto/supra_juiz.py:195`, `app/ponto/services/cartorio.py:1145`) | **Não diverge na existência do vínculo**: já recebeu None; acrescenta uma exceção própria de cobrabilidade pela demissão. |
| P52 | `app/chamados/detector_anomalias.py:60` A | Há colisão se mais de uma EC tem A=True e fim nulo, sem recorte pelo dia. | **não/sim**; anomalia → disputa na batida. (`app/api/views.py:434`, `app/api/views.py:757`, `app/api/views.py:764`) | Dois vínculos ativos de vigências disjuntas podem contar colisão; encerrado vigente desaparece. |
| P53 | `app/chamados/juizes.py:1809` A | Qual marco espera uma pergunta datada? EC ativa atual mais recente. | **não/sim**; fallback da defesa/classificação da resposta. (`app/chamados/juizes.py:1860`, `app/chamados/juizes.py:1990`) | Pergunta antiga passa a usar EC nova; CE A. |
| P54 | `app/chamados/models.py:1006` S | Pode renascer? `getattr(S,'') != ativo` retorna False. | **não/sim**; veta `reabrir`. (`app/chamados/models.py:1020`) | CE S. |
| P55 | `app/chamados/reconciliador.py:234` A | Ainda falta âncora se existe EC ativa com âncora nula; sem data do fato. | **não/sim**; premissa → decisão → retratação/fechamento. (`app/chamados/reconciliador.py:292`, `app/chamados/reconciliador.py:524`) | EC atual sem âncora pode sustentar premissa de chamado histórico. |
| P56 | `app/chamados/reconciliador.py:821` S | Reemite chamado do dia somente se S=ativo. | **não/sim**; retorna antes da costura/emissão. (`app/chamados/reconciliador.py:835`) | CE S. |
| P57 | `app/chamados/services/adesao.py:34` S | Universo de adesão na janela: célula `nunca_bateu` de S=ativo. | **não/sim**; sincronização → abertura do pai. (`app/chamados/services/adesao.py:59`, `app/chamados/services/adesao.py:75`) | CE S. |
| P58 | `app/chamados/services/adesao.py:67` S | Segundo gate: S=desligado pula adesão. | **não/sim**; impede abrir/reabrir pai. (`app/chamados/services/adesao.py:75`, `app/chamados/services/adesao.py:93`) | CE S; leitura própria mesmo após P57. |
| P62 | `app/chamados/services/disputa_emissao.py:1419` S | Havendo pergunta prévia, S diferente de ativo equivale a “já perguntou”. | **não/sim**; impede `PerguntaDisputa.abrir`. (`app/chamados/services/disputa_emissao.py:832`, `app/chamados/services/disputa_emissao.py:837`) | CE S. |
| P63 | `app/chamados/services/fabrica_da_celula.py:112` S | Motivos a perguntar ficam vazios se **S != ativo**, ou dia sem trabalho. | **não/sim**; motivos → nascimento da pergunta. (`app/chamados/services/fabrica_da_celula.py:207`, `app/chamados/services/fabrica_da_celula.py:217`) | CE S. |
| P64 | `app/chamados/services/fabrica_da_celula.py:201` S | Pauta cadastro sem turno exige célula passada, **S=ativo**, dia cobrável. | **não/sim**; ramo de pauta antes do retorno. (`app/chamados/services/fabrica_da_celula.py:203`) | CE S. |
| P66 | `app/chamados/services/pergunta_no_app.py:186` S | Vigia de cobradores sem pergunta no app mede somente S=ativo. | **não/sim**; achados → comando perguntar → criação de disputa. (`app/chamados/services/pergunta_no_app.py:200`, `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20`, `app/chamados/services/pergunta_no_app.py:240`) | CE S. |
| P67 | `app/chamados/views.py:442` A | Ação de corroborar régua escolhe EC ativa mais recente do template mencionado. | **não/sim**; passa esse vínculo à porta que altera o veto da cobrança. (`app/chamados/views.py:436`, `app/chamados/views.py:443`) | CE A; ação sobre fato antigo pode alcançar EC atual. |
| C23 | `app/escala/services/cadastro_realidade.py:186` S | Para S=desligado, C5 conserva somente dias segurados na lavra de retenção; demais dias são retirados. | **não/sim**; corte em `app/escala/services/cadastro_realidade.py:198` → `vigia_retido_calado` conta roteados/calados. (`app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`) | CE S. |
| C25 | `app/escala/services/cadastro_realidade.py:477` S | Beco de folga considera células indefinidas da janela inclusiva somente de S=ativo. | **não/sim**; universo do beco → porta que reata perguntas. (`app/escala/services/cadastro_realidade.py:573`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:110`) | CE S. |

Estes sítios do domínio de chamados só alteram apresentação, opção ou diagnóstico no trecho observado:

| ID | Sítio | Pergunta/predicado; saída | F/Fio | Divergência |
|---|---|---|---|---|
| P23 | `app/ponto/services/abriu_nao_bateu.py:194` A | Já passou a hora? Primeira ativa; sem EC retorna True e afeta contador. (`app/ponto/services/abriu_nao_bateu.py:196`) | não/não | CE A. |
| P30 | `app/ponto/services/cluster_espurio.py:260` S | Flag de simulação `ativo = S == ativo`. | não/não | CE S; é flag diagnóstica. |
| P37 | `app/ponto/services/furos_vetados.py:62` S | Relatório de vetados exclui S=desligado. | não/não | CE S. |
| P43 | `app/ponto/services/triagem_batida.py:333` A | Primeira ativa fornece título/rótulo da anuência; o chamado é criado independentemente. (`app/ponto/services/triagem_batida.py:335`) | não/não | CE A. |
| P45 | `app/ponto/services/vinculo_na_lista.py:71` S | Achado GPS/cadastro exclui S=desligado. | não/não | CE S. |
| P46 | `app/ponto/services/vinculo_na_lista.py:83` S | Evidência por colegas do posto considera apenas S=ativo. | não/não | CE S. |
| P59 | `app/chamados/services/desvio_escala.py:44` A | Texto de desvio dos últimos 14 dias exige EC ativa; sem ela retorna None. (`app/chamados/services/desvio_escala.py:45`) | não/não | CE A. |
| P60 | `app/chamados/services/desvio_escala.py:106`, `app/chamados/services/desvio_escala.py:107` A+S | Colegas do mesmo template: A=True e S=ativo; alimentam texto explicador. | não/não | CE S/A. |
| P61 | `app/chamados/services/disputa_emissao.py:341` A | Opções da pergunta usam A=True e fim nulo; não decide nascimento nesse ponto. | não/não | CE A. |
| P65 | `app/chamados/services/limbo_folgas.py:65` A | Alcance do template conta vínculos A=True para texto de sugestão. | não/não | CE A. |

**App/API.**

| ID | Sítio | Pergunta/predicado | F/Fio; caminho | Divergência |
|---|---|---|---|---|
| A01 | `app/api/login_helpers.py:141` S | Autentica somente se `getattr(S,'ativo') == ativo`. | não/não; recusa de login. Efeito em posterior requisição do fio é [não verificado]. | Afastado com vínculo válido hoje é recusado; CE S. |
| A02 | `app/api/views.py:116` S | Refresh exige S=ativo; senão 401. | não/não; acesso. | CE S. |
| A03 | `app/api/views_core.py:172` S | Mesmo gate de refresh no kernel core. | não/não; acesso. | CE S. |
| A04 | `app/api/views.py:403` S | Pode bater somente se **S ∈ {ativo, afastado

[truncado em 20000 de 58730 caracteres]

## 13/11 11:43 · sessao ccecfb700358 · FERIADO-USA-A-LEI-DE-1609 · verificador · origem=Code

**REPROVA:** o delta corrige os casos nomeados do item B, mas restam formas não declaradas que passam pelo selo; além disso, preservar a célula exportada não garante preservar os números dos leitores vivos (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:137`; `app/ponto/services/fechamento.py:949`).

**A. O reparo cobre os casos da volta 2, mas a catraca continua incompleta.**

| Caso | Morde pelo motivo anunciado? | Evidência |
|---|---|---|
| t1 | Sim: nascimento pelo reverso `escalas`, sem flag. | `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:127`, `:312`, `:347` |
| t2 | Sim: reverso `vinculos`, inclusive pela cadeia `filter().get_or_create()`. | Mesmo arquivo`:122`, `:313`, `:348` |
| t3 | Sim: `defaults` escreve a flag do existente, embora o nascimento por `create_defaults` seja válido. | Mesmo arquivo`:207`, `:314`, `:349` |
| t4 | Sim: `defaults` sobrescreve a keyword de busca; acusa `True`. | Mesmo arquivo`:159`, `:316`, `:350` |
| t5 | Sim: `**kw` posterior torna a flag opaca. | Mesmo arquivo`:143`, `:318`, `:351` |
| t6 | Sim: última chave literal vence; acusa `True`. | Mesmo arquivo`:145`, `:320`, `:352` |
| t7 | Sim: reconhece `acreate` sem flag. | Mesmo arquivo`:57`, `:322`, `:353` |
| t8 | Sim: reconhece escrita crua por `aupdate`. | Mesmo arquivo`:59`, `:215`, `:323`, `:354` |
| t9 | Sim: encontra a atribuição dentro do desempacotamento. | Mesmo arquivo`:172`, `:224`, `:324`, `:355` |
| u1 | Sim: reconhece `get_model` com nome posicional. | Mesmo arquivo`:96`, `:325`, `:356` |
| u2 | Sim: rastreia o alias anotado. | Mesmo arquivo`:87`, `:326`, `:357` |
| u3 | Não morde corretamente: primeiro argumento só atualiza `ativa`; segundo fornece `create_defaults` com cópia válida. | Mesmo arquivo`:150`, `:164`, `:207`, `:328` |
| u4 | Não morde corretamente: reconhece o reverso e aceita a flag explícita copiada. | Mesmo arquivo`:105`, `:127`, `:330` |

Há, pelo menos, duas formas possíveis de produção omitidas de `LIMITES`:

```python
(EC := EscalaColaborador).objects.create(colaborador=c)
```

O alias por `NamedExpr` não é rastreado: `_apelidos` cobre `Assign` e `AnnAssign`; `_eh_modelo` não reconhece esse receptor. Assim o nascimento pode escapar sem flag explícita, usando o default ainda existente (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:96`, `:125`; `app/escala/models.py:896`). Essa forma não consta dos limites (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:34`).

```python
campo = 'trabalha_em_feriado'
EC.objects.get_or_create(
    colaborador=c,
    defaults={
        'trabalha_em_feriado': origem.trabalha_em_feriado,
        campo: True,
    },
)
```

A chave dinâmica posterior sobrescreve a literal, mas `_do_dict` ignora chaves que não sejam constantes ou `**`; conserva a cópia aceita e deixa passar o nascimento com `True`. Também não está declarado em `LIMITES` (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:137`, `:105`, `:199`, `:34`).

São contraexemplos da cobertura do selo; ocorrência dessas formas em um escritor já existente na produção: **[não verificado]**. Os quatro nascimentos explícitos identificados na cópia perguntam ao juiz ou copiam a flag de origem (`app/colaboradores/services/vinculo.py:296`, `:975`, `:1093`, `:1114`).

**B. A-3 e F-6 sustentam-se como FILA; nenhum bloqueia isoladamente este pouso.** [nao verificado]

A-3 é real: `_o_chao_do_dia_mudou` relê `timezone.localdate()` no interior. Porém o teto declara exatamente esse sítio, e o complemento proíbe ampliar a cura naquela volta; o BACKLOG registra a pendência (`app/ponto/portas/celula.py:464`; `app/ponto/tests/test_cadastro_futuro_c5e.py:316`, `:368`; `app/docs/BACKLOG.md:2062`). O lote transmite seu `hoje` à porta e à regeneração (`app/escala/management/commands/aplicar_feriado_lei_1609.py:48`, `:73`; `app/colaboradores/services/vinculo.py:905`).

F-6 também existe: `sincronizar_feriado_admin` decide pelo cargo e aplica pela porta. Esse comando não integra a cadeia do apply desta obra, cujo contador pergunta ao juiz de 16/09 (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:19`, `:47`, `:58`; `app/escala/management/commands/aplicar_feriado_lei_1609.py:46`, `:73`; `app/escala/models.py:960`). A classificação histórica como preexistente vem da triagem fornecida: **[não verificado]** por histórico da cópia.

**C. Escritores, leitores e significado.**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `trabalha_em_feriado` no nascimento | `executar_vinculo` pergunta ao juiz; trecho retroativo, ajuste e desfazer copiam a origem. | `app/colaboradores/services/vinculo.py:296`, `:975`, `:1093`, `:1114` |
| Flag de vínculo existente | Atribuição e `save` em `marcar_trabalha_em_feriado`. | `app/colaboradores/services/vinculo.py:893` |
| Flag para decidir/corrigir | Contador compara com o juiz; comando legado compara com cargo; porta compara antes de salvar. | `app/escala/models.py:960`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`; `app/colaboradores/services/vinculo.py:890` |
| Flag para efeito no dia/dinheiro | `feriado_vira_folga`, usado pelo gerador, precedência e separação das rubricas. | `app/escala/models.py:1406`, `:1427`; `app/ponto/precedencia.py:377`; `app/ponto/services/fechamento.py:949` |
| `minutos_previstos` / `previsto_em` | Recalculo soma a grade e carimba; falha produz `None`; persiste por `save` ou `update`. | `app/ponto/services/fechamento.py:1048`, `:1083`, `:1094`, `:1144`, `:1181`, `:1188` |
| Invalidação de `previsto_em` | Signal chama `invalidar_previsto`, que zera o carimbo e avança `atualizado_em`. | `app/escala/signals.py:138`; `app/ponto/services/fechamento.py:1607` |
| Restauração desses campos | Porta genérica restaura os campos pedidos. | `app/ponto/services/fechamento.py:1380`, `:1393` |
| Leitura do previsto mensal | Prontidão; cartão/espelho; fotografia e hash dos campos gravados. | `app/folha/export.py:1045`; `app/relatorios/cartao_pela_celula.py:117`; `app/chamados/services/foto_e4b.py:290`; `app/ponto/portas/regen_exportada.py:58` |

`previsto_em` significa frescor do previsto; `minutos_previstos` mede minutos da grade, convertidos em horas pelos consumidores. Não demonstram congelamento da competência nem frescor do registro inteiro (`app/ponto/models.py:430`, `:454`; `app/folha/export.py:1050`; `app/relatorios/cartao_pela_celula.py:117`).

A divergência relevante é temporal: a célula conserva a decisão histórica, enquanto a separação de rubricas consulta a flag atual da linha do vínculo que responde pelo dia (`app/escala/models.py:1131`; `app/escala/servico_jornada.py:61`; `app/ponto/services/fechamento.py:949`). Portanto, os leitores não garantem a mesma fotografia histórica.

**D. Classes de reprovação.**

- **Segundo juiz:** existe a decisão por cargo, fora do caminho deste lote; o lote usa o juiz existente (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`; `app/escala/models.py:960`).
- **Segundo escritor fora da porta:** não aparece no apply; ele chama a porta, onde ficam atribuição e `save` (`app/escala/management/commands/aplicar_feriado_lei_1609.py:73`; `app/colaboradores/services/vinculo.py:893`). A garantia AST continua incompleta pelos contraexemplos de A (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:137`).
- **Relógio interior:** presente, explicitamente em fila, conforme B (`app/ponto/portas/celula.py:464`; `app/docs/BACKLOG.md:2062`).
- **Derivação paralela:** `fechamento_lido` chama o próprio recalculador; não cria outra conta. O problema demonstrado é usar cadastro atual dentro dessa conta histórica (`app/ponto/services/fechamento.py:1759`, `:949`).
- **Selo/prova sem cobertura suficiente:** os novos mutantes têm expectativas específicas; o hash do gravado, porém, não cobre os números derivados pelos leitores vivos (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:347`; `app/ponto/portas/regen_exportada.py:37`, `:71`; `app/chamados/services/foto_e4b.py:328`).
- **Pré-requisito:** o comando verifica o universo e fecha o arquivo exclusivo de reversão antes da primeira mutação (`app/escala/management/commands/aplicar_feriado_lei_1609.py:64`, `:69`, `:73`). Inversão concreta de travas, pouso efetivo e cumprimento operacional dos pré-requisitos: **[não verificado]**.
- **Campo com dois significados:** não demonstrei duas unidades para o previsto; demonstrei mistura entre decisão congelada e flag atual no significado histórico do feriado (`app/ponto/models.py:432`; `app/escala/models.py:1131`, `:1427`; `app/ponto/services/fechamento.py:949`).

**G. A exportada pode mudar nos leitores vivos.**

O encadeamento informado procede: o apply salva por vínculo; o signal invalida todo fechamento do colaborador com `previsto_em`, sem filtro de competência. Essa operação altera carimbos, conservando os campos numéricos gravados (`app/escala/management/commands/aplicar_feriado_lei_1609.py:73`; `app/colaboradores/services/vinculo.py:894`; `app/escala/signals.py:146`; `app/ponto/services/fechamento.py:1611`).

| Leitor, depois do lote | Fonte do número |
|---|---|
| `folha.export.prontidao` | Sem carimbo, calcula pela grade; `trab` continua vindo de `fech.horas_trabalhadas`. `app/folha/export.py:1036`, `:1045`, `:1095` |
| Lista do fechamento | Campos do FM gravado, com prontidão calculada separadamente. `app/ponto/views.py:1135`; `app/ponto/services/fechamento_lista.py:85`, `:107` |
| Painel lateral | Cálculo vivo por `fechamento_lido`. `app/ponto/views.py:993` |
| PDF do TXT histórico | Snapshot de `ExportacaoDominio.conteudo`. `app/folha/pdf_export.py:63` |
| PDF cartão/espelho | Totais vivos por `fechamento_lido`. `app/relatorios/pdf_espelho.py:628`; `app/relatorios/cartao_pela_celula.py:73`, `:323` |
| TXT histórico baixado | Conteúdo gravado. `app/folha/views.py:561` |
| Novo TXT da mesma competência | Cálculo vivo injetado na montagem. `app/folha/views.py:208`, `:268` |
| `foto_e4b` | Guarda FM gravado inteiro **e** `dinheiro_online` vivo. `app/chamados/services/foto_e4b.py:290`, `:328` |
| `regen_exportada.hash_do_gravado` | Campos gravados; exclui `previsto_em` e `atualizado_em`. `app/ponto/portas/regen_exportada.py:37`, `:71` |
| `regenerar_em_exportada`, se executada depois | Ato adicional que regenera apesar da lavra e recalcula. `app/ponto/portas/regen_exportada.py:179`, `:184` |

**O previsto da prontidão lê a célula preservada.** A grade usa `ata.tipo_dia`/`cel.trabalha` e os minutos da ata; no fallback do previsto, `eh_dia_trabalho` devolve diretamente `cel.trabalha`. Portanto, a flag nova, sozinha, não reduz esse previsto quando célula/ata estão preservadas e coerentes (`app/escala/services/leitor_celula.py:397`, `:411`; `app/escala/utils.py:2266`; `app/escala/models.py:1131`). Contudo, se o snapshot mensal já divergir da grade, invalidar o carimbo muda a fonte exibida; a invalidação não verifica essa igualdade (`app/folha/export.py:1045`, `:1095`; `app/ponto/services/fechamento.py:1611`). Existência dessa divergência no lote real: **[não verificado]**.

**O dinheiro vivo também lê a flag atual.** Considere vínculo weekday corrigido de `True` para `False`, em competência exportada, com feriado já congelado como folga, par trabalhado e escala certa. O motor continua colocando o par em `periodos_ft`, pois consulta a célula preservada (`app/ponto/motor_calculo_v2.py:2448`, `:2492`). Mas `feriado_vira_folga` passa de falso para verdadeiro pela alteração da flag (`app/escala/models.py:1427`, `:1432`).

Consequência: as horas desse par saem de `horas_folga_trabalhada` e entram em `horas_extras_100_feriado`, `horas_extras_100` e `horas_extras`; o subconjunto noturno também pode mudar. Isso ocorre na leitura, sem regenerar a célula exportada ([app/ponto/services/fechamento.py:949](/home/ronald/codex_ws/b4e060c4bda347dc5302a7805ff28eb37b07e311/app/ponto/services/fechamento.py:949), `:955`, `:962`, `:965`).

Assim, painel, cartão PDF e fotografia online podem mudar; um novo TXT pode refletir a mudança conforme as rubricas cadastradas. TXT/PDF históricos continuam mostrando o snapshot (`app/ponto/views.py:993`; `app/relatorios/cartao_pela_celula.py:73`; `app/chamados/services/foto_e4b.py:328`; `app/folha/export.py:443`, `:446`; `app/folha/views.py:561`; `app/folha/pdf_export.py:63`). Ocorrência e valores concretos desse cenário na produção: **[não verificado]**.

**REPROVA — A: lacunas não declaradas da catraca (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:137`); G/F-4: números vivos de exportada podem mudar, sem cobertura pelo hash do gravado (`app/ponto/services/fechamento.py:949`; `app/ponto/portas/regen_exportada.py:37`).**

## 13/11 11:40 · sessao 7795cd9bc9de · - · - · origem=Ronald (tmux)

G: há cenário de mudança de número de competência exportada, mesmo com célula preservada: o cálculo vivo reclassifica folga trabalhada usando a flag atual do vínculo. A ocorrência desse cenário na produção raspada é **[não verificado]**; o caminho existe no código (`app/ponto/services/fechamento.py:949`, `app/escala/models.py:1427`).

- O apply chama a porta por vínculo (`app/escala/management/commands/aplicar_feriado_lei_1609.py:73`). A porta salva a flag (`app/colaboradores/services/vinculo.py:893`), disparando o signal (`app/escala/signals.py:137`), que invalida **todos** os fechamentos com `previsto_em`, sem filtro de competência exportada: zera o carimbo e altera `atualizado_em`, conservando os números gravados (`app/ponto/services/fechamento.py:1611`).
- A guarda preserva células da exportada: encontra as competências exportadas (`app/ponto/portas/celula.py:221`) e pula seus dias antes da geração (`app/ponto/portas/celula.py:284`).

| Leitor | Fonte após o lote |
|---|---|
| `folha.export.prontidao` | Com carimbo invalidado, calcula pela grade; o snapshot só vale com `previsto_em` (`app/folha/export.py:1045`, `app/folha/export.py:1095`). |
| Lista de fechamento | Conserva FM gravado e chama prontidão para a coluna correspondente (`app/ponto/views.py:1135`, `app/ponto/services/fechamento_lista.py:85`). |
| Painel lateral | `fechamento_lido`, cálculo vivo (`app/ponto/views.py:993`, `app/ponto/services/fechamento.py:1759`). |
| PDF do TXT histórico | `ExportacaoDominio.conteudo`, snapshot (`app/folha/pdf_export.py:63`, `app/folha/views.py:834`). |
| PDF cartão/espelho | Totais vivos: PDF chama `folha_manda`, que chama `totais_da_folha`, que chama `fechamento_lido` (`app/relatorios/pdf_espelho.py:628`, `app/relatorios/cartao_pela_celula.py:323`, `app/relatorios/cartao_pela_celula.py:73`). |
| TXT histórico baixado | Conteúdo gravado intacto (`app/folha/views.py:561`). |
| Novo TXT da mesma competência | Recalcula em leitura e injeta esses objetos na montagem (`app/folha/views.py:208`, `app/folha/views.py:268`). |
| `foto_e4b` | Fotografa FM gravado inteiro **e**, separadamente, `dinheiro_online` vivo (`app/chamados/services/foto_e4b.py:290`, `app/chamados/services/foto_e4b.py:328`). |
| `regen_exportada.hash_do_gravado` | Hash dos números gravados; exclui precisamente os carimbos alterados pela invalidação (`app/ponto/portas/regen_exportada.py:37`, `app/ponto/portas/regen_exportada.py:71`). |
| `regenerar_em_exportada` executada depois | É ato adicional que abre a lavra, regenera e recalcula o FM (`app/ponto/portas/regen_exportada.py:179`, `app/ponto/portas/regen_exportada.py:184`). |

**Previsto da prontidão:** o fallback lê `ata.tipo_dia`/`cel.trabalha` e `ata.minutos_previstos`; sem minutos lavrados, chama o juiz (`app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:411`). O juiz consulta `eh_dia_trabalho` (`app/escala/utils.py:2266`), que devolve diretamente `cel.trabalha` quando existe célula (`app/escala/models.py:1131`). Portanto, **não há cadeia que faça apenas a flag nova reduzir o previsto de uma célula/ata preservada e coerente**; `precedencia.py` não entra nesse leitor (`app/escala/services/leitor_celula.py:397`).

**Contraprova de invariância dos demais números:** considere vínculo do lote weekday com `True → False`, em feriado que já estava congelado como folga, com par trabalhado de `h` horas e escala certa. O contador permite tal correção (`app/escala/models.py:960`); o motor mantém o par em `periodos_ft` porque lê a célula como folga (`app/ponto/motor_calculo_v2.py:2448`, `app/ponto/motor_calculo_v2.py:2492`). Antes, `feriado_vira_folga` devolve `False` pela flag `True`; depois devolve `True` pela flag `False` e feriado do lugar (`app/escala/models.py:1427`, `app/escala/models.py:1432`). O fechamento vivo retira essas `h` horas de `horas_folga_trabalhada`, adiciona-as a `horas_extras_100`, `horas_extras_100_feriado` e `horas_extras`, e pode mover também o subconjunto noturno (`app/ponto/services/fechamento.py:949`, `app/ponto/services/fechamento.py:955`, `app/ponto/services/fechamento.py:962`, `app/ponto/services/fechamento.py:965`).

Assim, painel, cartão PDF, fotografia online e eventual novo TXT podem mudar; TXT/PDF históricos conservam a fotografia anterior (`app/ponto/views.py:993`, `app/relatorios/cartao_pela_celula.py:73`, `app/chamados/services/foto_e4b.py:328`, `app/folha/views.py:208`, `app/folha/views.py:561`, `app/folha/pdf_export.py:63`). O novo TXT distingue os eventos de feriado e folga trabalhada, embora rubricas cadastradas possam fazê-los convergir no arquivo — essa configuração é **[não verificado]** (`app/folha/export.py:443`, `app/folha/export.py:446`).

O hash inalterado do gravado prova apenas que seus campos incluídos não mudaram; **não prova invariância dos leitores vivos** (`app/ponto/portas/regen_exportada.py:37`, `app/ponto/portas/regen_exportada.py:71`).

## 13/11 11:40 · sessao 1911af74687d · - · - · origem=Ronald (tmux)

A cura os casos descritos na volta 2 e os mutantes novos mordem os motivos esperados. Reexecutei o scanner em memória, sem criar arquivos: `achados(app) == []`, contagem do escritor autorizado igual a 1, e a lista esperada integral dos mutantes coincidiu. Evidência da verificação pode ser reproduzida pelo scanner e pelas expectativas em `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:183`, `:253`, `:260`, `:333`.

| Caso | Resultado e motivo | Referências |
|---|---|---|
| t1/t2 | Acusam nascimento sem flag pelos reversos `escalas`/`vinculos`, inclusive cadeia `filter().get_or_create()`. | selo `:122`, `:127`, `:312`, `:313`, `:347`, `:348`; reversos reais `app/escala/models.py:872`, `:892`. |
| t3 | Acusa escrita do existente por `defaults`; nascimento com `create_defaults` copiado permanece aceito. | selo `:159`, `:207`, `:314`, `:349`. |
| t4 | `defaults` sobrescreve a flag de busca e o valor literal `True` é acusado. | selo `:159`, `:205`, `:316`, `:350`. |
| t5 | `**kw` posterior à flag torna o valor opaco e é acusado. | selo `:137`, `:143`, `:203`, `:318`, `:351`. |
| t6 | Última chave literal vence; acusa `True`. | selo `:142`, `:145`, `:320`, `:352`. |
| t7/t8 | `acreate` acusa ausência da flag; `aupdate` acusa escrita crua. | selo `:55`, `:59`, `:196`, `:215`, `:322`, `:353`. |
| t9 | Atribuição desempacotada acusa escrita fora da porta. | selo `:172`, `:224`, `:324`, `:355`. |
| u1/u2 | `get_model` com nome em argumento posicional e alias anotado são reconhecidos e acusam ausência da flag. | selo `:87`, `:96`, `:325`, `:356`. |
| u3 | Limpo pelo motivo certo: segundo argumento fornece `create_defaults` com cópia; primeiro `defaults` só contém `ativa`. | selo `:150`, `:164`, `:207`, `:328`. |
| u4 | Limpo pelo motivo certo: reverso é reconhecido, e nascimento recebe cópia aceita. | selo `:105`, `:127`, `:330`. |

Persistem formas possíveis em código de produção que o selo não acusa e `LIMITES` não declara — **não encontrei ocorrência dessas formas no HEAD**; ocorrência real é `[não verificado]`:

1. **Chave dinâmica posterior em `defaults`:**
   ```python
   campo = 'trabalha_em_feriado'
   EC.objects.get_or_create(
       colaborador=c,
       defaults={'trabalha_em_feriado': origem.trabalha_em_feriado, campo: True},
   )
   ```
   O scanner retorna `[]`: `_do_dict` ignora a chave dinâmica e conserva a cópia aceita, embora a última chave efetiva seja `True`. Trata `**` como opaco e constantes como chaves conhecidas, sem tratamento das demais chaves. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:137`, `:143`, `:145`, `:105`, `:199`; limites enumerados em `:34`.

2. **Alias por expressão de atribuição:**
   ```python
   (EC := EscalaColaborador).objects.create(colaborador=c)
   ```
   Scanner retorna `[]`: rastreia `Assign` e `AnnAssign`, mas não `NamedExpr`; `_eh_modelo` não reconhece esse receptor. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:96`, `:125`; limites em `:34`. Esse contraexemplo reproduz diretamente a volta do default proibido, ainda presente em `app/escala/models.py:896`.

3. **`get_model` com keywords:**
   ```python
   apps.get_model(app_label='escala', model_name='EscalaColaborador').objects.create(colaborador=c)
   ```
   Scanner retorna `[]`, pois `_eh_modelo` só consulta `no.args`, não keywords. Compatibilidade da chamada com implementação Django não incluída na cópia: `[não verificado]`; dependência declarada é Django 5.2.14 em `app/requirements.txt:13`. Falha do reconhecimento AST é verificável em `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:96`; ausência no inventário em `:34`.

Os quatro nascimentos explícitos atuais continuam corretos: pergunta ao juiz em `app/colaboradores/services/vinculo.py:287`/`:296`, cópias em `:971`/`:975`, `:1090`/`:1093`, `:1111`/`:1114`.

_4576 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
