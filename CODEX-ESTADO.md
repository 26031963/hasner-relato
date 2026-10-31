# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 22:20** · publicado em **30/10/2026 22:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 22:20 · sessao 7af13b19947f · E1-BATERIA · red · origem=Code

Criado somente [app/ponto/tests/test_e1_lote3.py](/home/ronald/codex_ws/red_21b9db3852a8_20261030_221425_3149041/app/ponto/tests/test_e1_lote3.py), com 20 casos. Sintaxe e estrutura verificadas sem gerar arquivos auxiliares. Suite Django não executada; RED ainda não medido. [nao verificado]

Valores abaixo em minutos, salvo DSR.

| ID | Esperado | Regra | Origem |
|---|---|---|---|
| L01 | Trabalhado 480; atraso/antecipada 0; dia na lista | L-084 | S14 corrigido |
| L02 | Skip: status não definido | L-084; ADR | S14 corrigido |
| L03 | `ok` nos sete leitores | CLT 58/74; ADR | S01 ampliado |
| L04 | `folga` | CLT 67; L-024 | S05 ampliado |
| L05 | `ausencia` | Lei 605/49 art.6; L-089 | S07 ampliado |
| L06 | `falta` decidida | L-039; Lei 605/49 art.6 | S08 ampliado |
| L07 | Início noturno: `ok` | CLT 59-A; L-085; D3 | S15 ampliado |
| L08 | Cauda no dia seguinte: `folga` | L-085; L-024; D3 | S16 ampliado |
| L09 | Pré-admissão: `sem_dados` | CLT 4; L-112; D3 | S20 ampliado |
| L10 | Sem batida, antes da entrada: `previsto` | D4 | S03 ampliado |
| L11 | Skip: regime 24x48 integral | CLT 58/59/71/73 | Novo |
| L12 | Skip: 24x48 com saída às 06h | CLT 58/59/71/73 | Novo |
| L13 | Sábado: trabalhado 240; HE 0 | CLT 58/59/71; L-024 | Novo |
| L14 | Sábado: trabalhado 300; HE50 60 | CLT 59 §1; 71 §1 | Novo |
| L15 | Intermitente: trabalhado 180; descontos 0 | CLT 452-A/4 | Novo |
| L16 | Intermitente sem convocação: trabalho/falta 0 | CLT 452-A | Novo |
| L17 | Municipal local: trabalhado 480; dobra 480 | Lei 605/49 arts.8/9; Súmula 146 | Novo |
| L18 | Outra praça: trabalhado 480; dobra/HE 0 | Alcance municipal; Lei 605/49 | Novo |
| L19 | DSR: 0 semanas mantidas; 1 perdida | Lei 605/49 art.6; L-039 | Novo |
| L20 | DSR: 1 semana mantida; 0 perdidas | Lei 605/49 arts.1/6; CLT 67 | Novo |

A grade de 44h usa ciclo **semanal**, com cinco jornadas de 8h e sábado de 4h: são seis dias de trabalho.

Perguntas registradas como skips:

- **L02:** qual palavra e retenção correspondem ao cadastro × realidade?
- **L11/L12:** qual norma autoriza o 24x48 e define intervalo, noturno e HE? As alternativas numéricas estão abertas nas docstrings.
- **Subtests dos leitores:** qual tradução corresponde aos vocabulários paralelos sem mapa declarado?
- **L07, subtests específicos:** como consultar o dia histórico em `minha_escala` e no painel, cujas portas não oferecem esse recorte?

## 30/10 22:19 · sessao fee802eca215 · O261-V-FASE · verificador · origem=Code

**Há achados procedentes de integração: a chamada direta das pautas ignora a fase, e a resolução da lista ainda rebaixa C10 apesar de seu dono agora vir do e6.** Há também diferença temporal entre as lavras lidas pelo juiz e pela C10. (`app/ponto/management/commands/pautas_discordantes.py:94`; `app/escala/services/cadastro_realidade.py:708`; `app/ponto/management/commands/e6_oraculo.py:162`; `app/escala/services/cadastro_realidade.py:339`.)

1. **Pautas: falta repassar a fase, mas só acrescentar o argumento não resolve.** O pré-filtro chama `dono_da_divergencia(horas, False)` e pede o motor para qualquer resposta diferente de BATIDA. Se `cxr()` falhar, descarta o colaborador antes da decisão completa por `_ins.dono()`. (`app/ponto/management/commands/pautas_discordantes.py:93-104`.)

   Contraexemplo: único dia discordante, fase lavrada, cadastro válido, nenhuma batida e motor indisponível. O juiz completo devolveria CADASTRO sem motor; o pré-filtro força `cxr()` e produz apuração vazia. Mesmo passando `fase_deslocada=True`, `CADASTRO != BATIDA` continua verdadeiro: é preciso consumir a decisão com os insumos completos e sua economia existente. (`app/ponto/management/commands/e6_oraculo.py:107`; `app/ponto/management/commands/e6_oraculo.py:169-176`; `app/ponto/management/commands/pautas_discordantes.py:94-101`.)

   Em `--apply`, essa ausência pode fechar pauta existente, pois a sincronização fecha as chaves que saíram do alvo. (`app/ponto/management/commands/pautas_discordantes.py:113`; `app/ponto/services/pautas_dos_discordantes.py:165-179`.)

2. **C10 agora é resposta do e6, mas `dias_na_lista` continua privilegiando C9.** O censo grava `dono_do_dia` perguntando ao juiz; entretanto, `DONO_PELO_JUIZ` contém somente C9 e o comentário ainda atribui C10 à função apagada. Isso contradiz o contrato local de que donos divergentes do mesmo nível produzem `None`. (`app/ponto/management/commands/censo_fase_12x36.py:260-274`; `app/escala/services/cadastro_realidade.py:706-719`.)

   Contraexemplos: C9=CADASTRO/C10=BATIDA resulta CADASTRO; C9=ESTRUTURA/C10=CADASTRO resulta ESTRUTURA. O segundo mantém o alarme mesmo diante de outra resposta do mesmo juiz. A combinação ESTRUTURA/CADASTRO é inclusive exigida pelo selo antigo; ocorrência dessas combinações na frota real **[nao verificado]**. (`app/escala/services/cadastro_realidade.py:716-719`; `app/escala/tests/test_o261_v_fase_na_lista.py:326-332`; `app/chamados/services/vigia_fio.py:266`.)

3. **C10 perde o corte temporal recebido pela lista.** O juiz lê `ultimo_lavrado(self.hoje)`; C10 chama `ultimo_lavrado()` sem limite, embora `lista()` receba `hoje` e o vigia o encaminhe. (`app/ponto/management/commands/e6_oraculo.py:162`; `app/escala/services/fase_12x36.py:103-105`; `app/escala/services/cadastro_realidade.py:339`; `app/escala/services/cadastro_realidade.py:610`; `app/chamados/services/vigia_fio.py:259`.)

   Contraexemplo: existem lavras de 30 e 31/10, com conjuntos de dias diferentes. E6 com `--hoje 30/10` usa até 30/10; vigia com `hoje=30/10` pode consumir C10 de 31/10. O caminho de código permite essa divergência; ocorrência real **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:278`; `app/escala/services/fase_12x36.py:103-105`; `app/escala/services/cadastro_realidade.py:339`.)

O mapa de escrita e leitura ficou assim:

| Insumo ou leitor | Quem escreve / quem lê / referência temporal |
|---|---|
| `fase_deslocada` | `InsumosDoDono.dono()` monta o booleano pela presença da data em `fase()`; o juiz apenas consome o booleano, sem medir fase. (`app/ponto/management/commands/e6_oraculo.py:107`; `:159-170`.) |
| `fase()` / `dias_de_fase` | Sem injeção, lê os itens do colaborador na última lavra com `data_ref <= hoje`. O censo injeta os dias desta corrida, dispensando essa leitura. (`app/ponto/management/commands/e6_oraculo.py:135`; `:159-164`; `app/ponto/management/commands/censo_fase_12x36.py:260-261`.) |
| `dono_do_dia` da lavra | O censo obtém cada dono por `_ins.dono()` e o grava nos itens; `fase_12x36.lavrar()` persiste uma fotografia por dia. (`app/ponto/management/commands/censo_fase_12x36.py:262-276`; `app/escala/services/fase_12x36.py:84-92`.) |
| E6 / R1 | Usa `HOJE` do argumento ou do instante do ato; consulta o dono pela montagem única. R1 aponta para esse comando. (`app/ponto/management/commands/e6_oraculo.py:266`; `:278`; `:309`; `:359`; `app/core/placar_estrutural.py:115-138`.) |
| `pautas_discordantes` | A decisão final usa `InsumosDoDono(...hoje...)`; o pré-filtro direto é a lacuna descrita acima. (`app/ponto/management/commands/pautas_discordantes.py:82`; `:94`; `:104`.) |
| C9 | Na medição, cria insumos por competência do dia, com o `hoje` da medição, e grava o dono. Depois a lista lê a lavra C9 mais recente, sem limite temporal. (`app/ponto/services/teto_na_lista.py:49-50`; `:67-86`; `:120-129`; `app/escala/services/cadastro_realidade.py:319-322`.) |
| C10 / `dias_na_lista` / vigia | C10 copia o dono já gravado; `dias_na_lista` resolve os donos por colaborador/data; o vigia aceita BATIDA ou CADASTRO desse mapa, sem chamar novamente e6. (`app/escala/services/cadastro_realidade.py:339-342`; `:735-742`; `app/chamados/services/vigia_fio.py:259-266`.) |

**O novo ramo realiza BATIDA antes de fase**, nesta ordem: exceções CADASTRO de cadastro → batida aberta real → contagem ímpar → fase → lista do motor/resíduo. Portanto, “BATIDA primeiro” vale perante o novo ramo, mas não é absoluto. Por exemplo, ímpar + `sem_vinculo_no_dia=True` + fase retorna CADASTRO; fase + contagem par + batida aberta real retorna BATIDA. São regras anteriores explicitamente documentadas, não uma inversão introduzida pelo ramo de fase. (`app/ponto/management/commands/e6_oraculo.py:77-109`.)

**Não encontrei circularidade operacional no censo:** ele injeta os dias recém-julgados, inicializa `_fase` e só depois persiste a fotografia; esse caminho não lê a lavra anterior. `fase_12x36.dono_do_dia` foi apagado com lápide L-111. A arbitragem problemática restante é a prioridade por assinatura em `_dono_resolvido`, não outra medição da fase. (`app/ponto/management/commands/censo_fase_12x36.py:255-276`; `app/ponto/management/commands/e6_oraculo.py:135`; `:160`; `app/escala/services/fase_12x36.py:78-81`; `app/escala/services/cadastro_realidade.py:716-719`.)

O custo adicional da **leitura de fase** é uma query por instância consultada, com cache; com `dias_de_fase`, zero queries de lavra. Isso não significa uma query total por `InsumosDoDono`: há outras cargas de batidas, turnos e cadastro. A seleção é pela fotografia de `hoje`, não pela fotografia histórica de cada dia julgado; o censo mede corrente e anterior. (`app/ponto/management/commands/e6_oraculo.py:135-170`; `app/escala/services/fase_12x36.py:102-105`; `app/ponto/management/commands/censo_fase_12x36.py:83`.)

Os selos novos **têm mordida** na delegação censo→e6, na leitura da lavra e na prioridade da batida ímpar. Não exercem os contraexemplos das pautas nem duas lavras temporais; os selos antigos ainda exigem a hierarquia C9>C10. (`app/escala/tests/test_o261_v_fase_na_lista.py:345-379`; `:319-332`.)

`JUIZES.md` manda prevalecer o código vivo, mas sua tabela não registra e6 nem o censo como autoridades dessas perguntas; ela termina com vínculo e cobertura da data. (`app/docs/JUIZES.md:14-15`; `:74-80`.)

As reproduções foram em memória, com dependências simuladas, sem escrita de arquivos; suite Django e efeitos na base real **[nao verificado]**. [nao verificado]

## 30/10 22:17 · sessao 6384ea4d6b62 · - · - · origem=Ronald (tmux)

Achados nos leitores:

1. **Pautas omite fase no chamador direto e pode apagar a pauta válida.** O pré-filtro chama `dono_da_divergencia(horas, False)` sem `fase_deslocada`, exige `cxr()` para qualquer dia par e descarta o colaborador se o motor falhar; a decisão final só acontece depois desse descarte. Referências: `app/ponto/management/commands/pautas_discordantes.py:93`, `:97`, `:101`, `:104`. Contraexemplo executado somente em memória, com AST real e dependências simuladas: dia sem batida, fase lavrada, motor indisponível → `apurar=[]`; o juiz com fase devolve `CADASTRO` sem motor (`app/ponto/management/commands/e6_oraculo.py:107`, `:169`, `:173`). O pré-filtro repetido já estava registrado em `app/docs/BACKLOG.md:847`, mas agora a nova fase torna essa omissão relevante ao corte.

2. **C10 não herda o corte temporal.** `lista(hoje=...)` chama `_fase_deslocada(empresas)` sem hoje; ela consulta `ultimo_lavrado()` sem `ate`. Referências: `app/escala/services/cadastro_realidade.py:542`, `:548`, `:610`, `:335`, `:339`. Já o juiz usa `ultimo_lavrado(self.hoje)` (`app/ponto/management/commands/e6_oraculo.py:159`, `:162`), cujo filtro é `data_ref__lte=ate` (`app/escala/services/fase_12x36.py:103`). Contraexemplo: lavra de 31/10 existente; vigia rodado com `hoje=30/10` lê C10 de 31/10, enquanto e6 com `--hoje 30/10` lê até 30/10. O vigia propaga hoje para lista (`app/chamados/services/vigia_fio.py:259`), mas C10 o perde.

3. **Resolução C9/C10 conserva hierarquia anterior ao corte.** C10 agora grava dono pelo e6 (`app/ponto/management/commands/censo_fase_12x36.py:260`, `:262`, `:273`), porém `DONO_PELO_JUIZ` contém somente C9 (`app/escala/services/cadastro_realidade.py:706`, `:708`). `_dono_resolvido` dá precedência a C9, embora declare que divergência entre donos do mesmo nível deve produzir `None` (`:714`, `:716`, `:719`). O teste ainda exige que C9 `ESTRUTURA` vença C10 `CADASTRO` (`app/escala/tests/test_o261_v_fase_na_lista.py:326`). Reprodução em memória confirmou esse resultado. **Ressalva:** C9 `ESTRUTURA` é contraexemplo sintético usado pelo próprio teste; alcançabilidade na geometria real `[nao verificado]`.

Mapa temporal e de leitores:

- **e6/R1:** instante capturado e HOJE por argumento/default em `app/ponto/management/commands/e6_oraculo.py:266`, `:278`; `InsumosDoDono(...HOJE, agora...)` em `:309`; dono em `:359`. A referência R1 aponta para esse comando em `app/core/placar_estrutural.py:115`, `:138`.
- **Pautas:** `InsumosDoDono(...hoje...)` em `app/ponto/management/commands/pautas_discordantes.py:82`; captura do instante/default de hoje em `:110`.
- **C9:** `medir` captura instante/default em `app/ponto/services/teto_na_lista.py:49`; cria insumos por competência do dia, sempre com o hoje da medição (`:67`, `:72`), e grava dono (`:86`). A leitura da C9 pega a lavra mais recente sem limite temporal (`:120`, `:123`; `app/escala/services/cadastro_realidade.py:319`).
- **C10:** lê diretamente os `itens` e o `dono_do_dia` gravado, sem chamar novamente e6 (`app/escala/services/cadastro_realidade.py:339`, `:342`).
- **dias_na_lista/vigia:** agrupa assinaturas por colaborador/data e resolve os donos (`app/escala/services/cadastro_realidade.py:735`); vigia consulta esse mapa e aceita `BATIDA`/`CADASTRO` (`app/chamados/services/vigia_fio.py:255`, `:259`, `:266`).

`app/docs/JUIZES.md:14` declara que o código vivo prevalece e o lookup deve acompanhar a mudança; a tabela termina em `:80` sem registrar o juiz de dono e6 nem o censo de fase (`app/docs/JUIZES.md:19`).

## 30/10 22:17 · sessao 7961abb2b164 · - · - · origem=Ronald (tmux)

Achado procedente: **a C10 passou a lavrar o dono pelo e6, mas `dias_na_lista` continua classificando apenas C9 como decisão do juiz**. O censo pergunta ao e6 e grava a resposta em `dono_do_dia` (`app/ponto/management/commands/censo_fase_12x36.py:260`, `:273`); porém `DONO_PELO_JUIZ = {'C9'}` permanece, com comentário atribuindo a C10 à função apagada (`app/escala/services/cadastro_realidade.py:706`). A resolução continua privilegiando C9 em qualquer divergência (`:716`–`:719`).

Contraexemplos confirmados executando em memória as funções extraídas por AST, sem Django e sem escrita:

- C9=`CADASTRO`, C10=`BATIDA` → `CADASTRO`; assim uma classificação BATIDA agora lavrada pelo e6 na C10 pode ser descartada (`app/escala/services/cadastro_realidade.py:716`).
- C9=`ESTRUTURA`, C10=`CADASTRO` → `ESTRUTURA`, preservando alarme mesmo quando a C10 contém resposta do mesmo juiz (`:716`). A existência dessas combinações na base real não foi medida `[nao verificado]`.

Os testes antigos cristalizam a prioridade: exigem C9=`BATIDA` vencer C10=`CADASTRO` e C9=`ESTRUTURA` vencer C10=`CADASTRO`, apesar da nova origem da C10 (`app/escala/tests/test_o261_v_fase_na_lista.py:319`, `:326`). É lacuna da atualização do consumidor, não segunda medição da fase (`app/escala/services/cadastro_realidade.py:711`).

O caminho de gravação **evita circularidade operacional**: o censo calcula os dias em desvio e os injeta explicitamente em `dias_de_fase` (`app/ponto/management/commands/censo_fase_12x36.py:255`–`:262`); isso inicializa `_fase` e impede `fase()` de ler a lavra anterior (`app/ponto/management/commands/e6_oraculo.py:135`, `:159`–`:164`). O escritor persiste uma fotografia por dia em `MetricaSnapshot`, chave `desvio_de_cadastro_12x36`, escopo tenant (`app/escala/services/fase_12x36.py:75`, `:84`–`:93`).

A função antiga `dono_do_dia` foi removida e recebeu lápide L-111 (`app/escala/services/fase_12x36.py:78`–`:81`). O novo selo verifica a delegação por mock e exige pelo menos uma chamada com `fase_deslocada=True` (`app/escala/tests/test_o261_v_fase_na_lista.py:345`–`:353`); os selos seguintes verificam leitura real da lavra, prioridade da batida ímpar e juiz puro (`:365`–`:379`). Não executei os testes Django `[nao verificado]`.

`app/docs/JUIZES.md` não registra o e6 nem o censo como autoridade: sua tabela termina com as perguntas de escala, vínculo e cobertura da data (`app/docs/JUIZES.md:74`–`:80`). O próprio documento exige reconciliar a consulta quando divergir do código (`:14`–`:15`); os endereços vivos desta obra estão documentados nos módulos (`app/escala/services/fase_12x36.py:3`–`:8`, `app/ponto/management/commands/e6_oraculo.py:90`–`:95`).

## 30/10 22:16 · sessao 808aaa25ab96 · - · - · origem=Ronald (tmux)

Achado procedente: **`pautas_discordantes` omite fase na pré-decisão e pode excluir um dia que já tem dono CADASTRO.** O comando chama diretamente `dono_da_divergencia(horas, False)` para decidir se roda o motor; em dia par de fase lavrada isso retorna ESTRUTURA, força `cxr()`, e uma falha do espelho descarta o colaborador antes da classificação final. Já `InsumosDoDono.dono()` receberia `fase_deslocada=True` e retornaria CADASTRO sem consultar o motor. Contraexemplo: único item discordante em 25/10, fase lavrada, `horas=[]`, cadastro válido, sem batida aberta e espelho levantando exceção ⇒ `apurado=[]`. Reproduzido executando em memória a AST real de `apurar` com dependências simuladas: `cxr=1`, `dono=0`; não executei Django/DB. Âncoras: `app/ponto/management/commands/pautas_discordantes.py:93`, `:97`, `:101`, `:104`; `app/ponto/management/commands/e6_oraculo.py:169`, `:173`, `:175`.

O novo ramo respeita BATIDA antes de fase: batida aberta real ⇒ BATIDA; paridade ímpar ⇒ BATIDA; fase ⇒ CADASTRO; lista do motor/resíduo por último. Entretanto, BATIDA não é absolutamente o primeiro ramo: folga indefinida e ausência de vínculo retornam CADASTRO antes dele. Contraexemplo executado em memória: `dono_da_divergencia(['07:00'], False, sem_vinculo_no_dia=True, fase_deslocada=True)` ⇒ CADASTRO. Isso é regra anterior preservada pelo delta, com documentação e testes explícitos; recomendo apresentar como ressalva da leitura literal, não como regressão autônoma. Também `horas=[]`, fase e batida aberta real ⇒ BATIDA, embora não haja contagem ímpar, preservando outro aval anterior. Fontes: `app/ponto/management/commands/e6_oraculo.py:77`, `:85`, `:97`, `:103`, `:105`, `:107`; `app/ponto/tests/test_dono_le_a_tela.py:32`; `app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:164`, `:172`.

Não há circularidade operacional lavra→juiz→lavra: o censo injeta `dias_de_fase` calculados nesta corrida; isso inicializa `_fase` e impede a leitura da lavra anterior. Só depois são montados os itens e chamada `lavrar`. A função antiga `fase_12x36.dono_do_dia` foi removida, com lápide. Fontes: `app/ponto/management/commands/censo_fase_12x36.py:255`, `:260`, `:261`, `:262`, `:272`; `app/ponto/management/commands/e6_oraculo.py:135`, `:160`; `app/escala/services/fase_12x36.py:78`.

O custo adicional da fase é **uma consulta por instância** de `InsumosDoDono` que consulta `dono`, não por dia: `fase()` guarda o conjunto em `_fase`; `ultimo_lavrado` termina em uma `.first()`. Com dias injetados, são zero consultas de lavra. O custo por instância inclui percorrer todos os itens da lavra para filtrar o colaborador. Fontes: `app/ponto/management/commands/e6_oraculo.py:135`, `:159`, `:162`, `:163`, `:170`; `app/escala/services/fase_12x36.py:102`, `:105`.

Os selos novos têm mordida para a ligação censo→juiz e para a leitura da lavra pelo carregador: um troca o retorno do juiz e exige esse retorno gravado; outro lavra de verdade, exige CADASTRO no dia sem batida e BATIDA após inserir batida ímpar. Não cobrem a pré-decisão de `pautas_discordantes`, seleção temporal entre lavras, ausência de circularidade com lavra antiga conflitante nem contagem de consultas. Fontes: `app/escala/tests/test_o261_v_fase_na_lista.py:345`, `:348`, `:352`, `:353`, `:365`, `:369`, `:375`.

`JUIZES.md` determina que o código vivo prevalece e aponta o registro `core/juizes.py`; sua tabela não documenta o juiz da fase nem o dono e6. Fontes: `app/docs/JUIZES.md:11`, `:14`, `:19`, `:80`.

## 30/10 22:15 · sessao c18b7a251a62 · O261-E-CELULA-20 · verificador · origem=Code

**1. (a) Sim; (b) depende de qual instância é lida ao final. O teste não garante, sozinho, identidade das listas.** Ele verifica quantidade de carregamentos, cegueira e valor no carimbo. ([test_o261e_f5_alcance.py:254](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261e_f5_alcance.py:254))

O `censo()` fica efetivamente cego: busca chamados vivos vinculados à célula e, com universo vazio, retorna `cego=True`, preservando `fio_do_admin_sem_resposta=0`, antes de consultar a lista. ([vigia_fio.py:285](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:285), [vigia_fio.py:316](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:316), [vigia_fio.py:323](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:323))

Valores deduzidos do código para `reg['censo']['fio_do_admin_sem_resposta']`:

| Costura | Valor | Resultado previsto |
|---|---:|---|
| Atual: uma lista, leitura depois dos dois universos | **1** | Satisfaz a assertiva; o command transfere o contador da lista para `d`. |
| (a) Apagar a leitura final, ou antecipá-la para antes do alcance | **0** | Vermelho na assertiva que exige 1. |
| (b) Listas separadas; leitura final observa a lista entregue ao `censo()` | **0** | Vermelho: essa instância permaneceu intocada. |
| (b) Listas separadas; leitura final observa a lista entregue ao alcance | **1** | Escapa deste teste: apenas o alcance carrega, portanto `m.call_count` continua 1. | [nao verificado]

A fundamentação da tabela é o retorno antecipado do `censo`, o carregamento tardio, o registro da falha pelo alcance e a leitura final do command. ([vigia_fio.py:187](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:187), [vigia_fio.py:208](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:208), [vigia_fio.py:567](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:567), [vigiar_fio_morto.py:166](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:166))

O contraexemplo concreto é trocar somente a chamada do `censo()` por `censo(..., lista=ListaDoAdmin(hoje, empresas))`, mantendo `lista` no alcance e na leitura final. São duas instâncias, mas as assertivas dessa fixture continuam satisfeitas por inferência estática. ([vigiar_fio_morto.py:167](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:167), [test_o261e_f5_alcance.py:254](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261e_f5_alcance.py:254))

O RED fornecido mostra `1 != 0`, mas não identifica a alteração aplicada; comprovação experimental individual dos dois mutantes: **[não verificado]**. [nao verificado]

**2. A linha explica parcialmente os números: explicita o segundo, mas omite o universo do primeiro.** ([vigia_fio.py:793](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:793))

- **`FIO_DO_ADMIN`** conta dias classificados nesse balde pelo **censo antigo**, cujo universo parte de chamados vivos com célula; `celulas` lista IDs desse mesmo balde, limitada por `limite`. ([vigia_fio.py:285](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:285), [vigia_fio.py:380](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:380))
- **`sem_resposta_da_RODADA`** conta pares distintos `(colaborador, data)` que chegaram ao predicado e encontraram a lista sem resposta, nos dois universos. Não conta carregamentos: usa um conjunto e retorna seu tamanho. ([vigia_fio.py:185](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:185), [vigia_fio.py:204](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:204), [vigia_fio.py:227](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:227))
- O alcance possui **`alcance_fio_do_admin` separado**, publicado na linha `VFM|ALCANCE`; ele não é somado ao primeiro número acima. ([vigia_fio.py:569](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:569), [vigia_fio.py:623](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:623), [vigia_fio.py:795](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/services/vigia_fio.py:795))

Quanto aos leitores:

- O command imprime a linha e grava o dicionário no carimbo; a chave JSON continua **`fio_do_admin_sem_resposta`**. ([vigiar_fio_morto.py:169](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:169), [vigiar_fio_morto.py:216](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/management/commands/vigiar_fio_morto.py:216))
- Os testes leem essas chaves diretamente e pelo carimbo. ([test_o261_vigia_fio.py:579](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261_vigia_fio.py:579), [test_o261e_f5_alcance.py:256](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/chamados/tests/test_o261e_f5_alcance.py:256))
- **O ESTADO ainda não expõe essas chaves.** `fio_estado.py` abre o carimbo, mas omite ambas da seleção devolvida; `gerar_estado.py` usa esse leitor. A inclusão está registrada como pendência. ([fio_estado.py:82](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/bin/fio_estado.py:82), [gerar_estado.py:260](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/bin/gerar_estado.py:260), [BACKLOG.md:785](/home/ronald/codex_ws/971374506f7cb6bbadef6f435103f0f3fd59fd23/app/docs/BACKLOG.md:785))
- Outro parser específico da linha literal, ou consumidor externo à cópia: **[não verificado]**.

## 30/10 22:15 · sessao 39a15541c049 · - · - · origem=Ronald (tmux)

- A linha é **parcialmente explícita**: descreve `sem_resposta_da_RODADA` como dias distintos dos dois universos, mas não identifica `FIO_DO_ADMIN` e `celulas` como exclusivos do `censo()` antigo. Eles vêm de `d['fio_do_admin']` e `d['celulas_fio_do_admin']`, incrementados somente naquele censo; seu universo começa por chamados vivos vinculados à célula. `app/chamados/services/vigia_fio.py:793`, `app/chamados/services/vigia_fio.py:795`, `app/chamados/services/vigia_fio.py:380`, `app/chamados/services/vigia_fio.py:285`.
- O segundo número conta pares distintos `(colab_id, data_iso)` que **chegaram ao predicado** `dia_do_admin()` e encontraram a lista sem resposta; não conta tentativas de carregar nem todos os dias dos universos. O `set` deduplica e a propriedade devolve seu tamanho. `app/chamados/services/vigia_fio.py:185`, `app/chamados/services/vigia_fio.py:204`, `app/chamados/services/vigia_fio.py:208`, `app/chamados/services/vigia_fio.py:227`.
- O alcance mantém o próprio `alcance_fio_do_admin`, publicado na linha `VFM|ALCANCE|porta_fila=...|fio_do_admin=...`; não soma esse valor ao primeiro número de `VFM|FIO_DO_ADMIN`. `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:569`, `app/chamados/services/vigia_fio.py:623`, `app/chamados/services/vigia_fio.py:627`, `app/chamados/services/vigia_fio.py:795`.
- O command consome `linhas(d)` para stdout e guarda o dicionário inteiro em `reg['censo']`, preservando o nome JSON `fio_do_admin_sem_resposta`; só o rótulo textual virou `sem_resposta_da_RODADA`. `app/chamados/management/commands/vigiar_fio_morto.py:169`, `app/chamados/management/commands/vigiar_fio_morto.py:173`, `app/chamados/management/commands/vigiar_fio_morto.py:216`, `app/chamados/services/vigia_fio.py:795`.
- O consumidor do carimbo `bin/fio_estado.py` abre o JSON, mas sua seleção devolvida não inclui essas duas chaves; a comparação genérica considera somente `esperado`, e o command não inclui nenhuma delas no esperado. Assim o ESTADO atual não as mostra nem as avalia; `bin/gerar_estado.py` usa esse leitor/compositor. `bin/fio_estado.py:64`, `bin/fio_estado.py:72`, `bin/fio_estado.py:82`, `bin/fio_estado.py:92`, `app/chamados/management/commands/vigiar_fio_morto.py:87`, `app/chamados/management/commands/vigiar_fio_morto.py:95`, `bin/gerar_estado.py:260`.
- Há leitores de teste: verificam `fio_do_admin`, `fio_do_admin_sem_resposta`, deduplicação e carimbo; o teste do carimbo antigo compara todo `reg['censo']`. `app/chamados/tests/test_o261_vigia_fio.py:565`, `app/chamados/tests/test_o261_vigia_fio.py:579`, `app/chamados/tests/test_o261_vigia_fio.py:588`, `app/chamados/tests/test_o261_vigia_fio.py:780`, `app/chamados/tests/test_o261e_f5_alcance.py:256`.
- A inclusão dessas chaves no ESTADO está **registrada como pendência**, não implementada no leitor atual. `app/docs/BACKLOG.md:785`, `bin/fio_estado.py:82`.
- Não localizei parser específico da linha `VFM|FIO_DO_ADMIN` na busca integral com arquivos ocultos/ignorados; inexistência de outros consumidores externos à cópia permanece **[não verificado]**.

## 30/10 22:14 · sessao d200e8aa973f · - · - · origem=Ronald (tmux)

A leitura permite afirmar:

- Fixture sem `_chamado()`: `app/chamados/tests/test_o261e_f5_alcance.py:248`; `setUp` apenas cria/lavra célula acusando (`:58–63`), enquanto criação explícita de chamado está no helper separado (`:66–73`). A confirmação no próprio teste é `assertTrue(reg['censo']['cego'])`, em `:255`. O RED anexado chega à assertiva seguinte (`:256`), portanto passou pela de cegueira; identificação exata do mutante usado nesse RED: **[não verificado]**.
- `censo()` parte de chamados vivos vinculados à célula (`app/chamados/services/vigia_fio.py:284–288`); sem eles, retorna `cego=True` antes de acessar a lista (`:323–325`), preservando `fio_do_admin_sem_resposta=0`, inicializado em `:316`. Portanto ele está cego nessa fixture.
- Fluxo correto: command cria objeto único, passa aos dois censos e só então lê `sem_resposta` (`app/chamados/management/commands/vigiar_fio_morto.py:166–169`). Alcance consulta lei/lista (`app/chamados/services/vigia_fio.py:567–568`); `dia_do_admin` registra `(colab_id,data)` no conjunto quando a lista falha (`:204–209`), e `sem_resposta` retorna o tamanho (`:225–227`). Carimbo conserva `d` (`app/chamados/management/commands/vigiar_fio_morto.py:216`).
- Mutante (a), apagar leitura final `:169`: carimbo `reg['censo']['fio_do_admin_sem_resposta']=0`; teste vermelho em `test_o261e_f5_alcance.py:256`. Leitura final antecipada antes do alcance tem mesmo resultado, pela preguiça do carregador (`vigia_fio.py:188–190`) e retorno cego anterior. [nao verificado]
- Mutante (b), listas separadas: **não há garantia geral de vermelho**. Se `lista` continua sendo objeto passado ao censo e alcance recebe `ListaDoAdmin(...)` novo, leitura final vê lista intocada → carimbo **0** e vermelho. O loader será chamado **uma vez**, pelo alcance, então assertiva `m.call_count==1` (`test:254`) passa.
- Contraexemplo do mutante (b): alterar `command:167` para `censo(..., lista=ListaDoAdmin(hoje, empresas))`, mantendo `command:168–169` como está. Cada universo recebe sua instância; a instância do censo jamais carrega, e `lista` do alcance contabiliza falha → carimbo **1**, `m.call_count==1`, `cego=True`, e teste pode ficar verde. Evidência: retorno cedo (`vigia_fio.py:323–325`), carregamento preguiçoso (`:187–190`), assertivas apenas comportamentais (`test:254–258`). **Não executar esse contraexemplo; resultado é inferência estática.** [nao verificado]

O teste vizinho que põe o dia em ambos universos exige um único carregamento (`app/chamados/tests/test_o261e_f5_alcance.py:206–215`), mas ele fornece a lista compartilhada diretamente às funções, sem exercitar a costura do command; portanto não fecha esse contraexemplo no command.

## 30/10 21:51 · sessao 03e08bc51dac · O261-E-CELULA-20 · verificador · origem=Code

**1. Carregador, predicado e igualdade**

Há **1 sítio que chama `lista(...)`**, seguido de `dias_na_lista(...)`, e **1 sítio que aplica o predicado BATIDA/CADASTRO**. Os dois censos chegam a ele por `dono_fora_do_fio`. ([vigia_fio.py:198](app/chamados/services/vigia_fio.py:198), [vigia_fio.py:210](app/chamados/services/vigia_fio.py:210), [vigia_fio.py:263](app/chamados/services/vigia_fio.py:263))

O command passa a mesma `ListaDoAdmin` aos dois. Isso garante **o mesmo resultado do predicado para o mesmo par colaborador/data**, com no máximo uma tentativa de carregamento, inclusive quando falha. ([vigiar_fio_morto.py:166](app/chamados/management/commands/vigiar_fio_morto.py:166), [vigia_fio.py:187](app/chamados/services/vigia_fio.py:187))

Isso **não garante o mesmo balde final**: `censo` aceita pergunta viva antes de consultar a lista; `censo_alcance` exige uma porta pela fila/app e pode continuar até a lista. Portanto, compartilhar a lista elimina divergência nessa consulta, mas não torna os universos equivalentes. ([vigia_fio.py:359](app/chamados/services/vigia_fio.py:359), [vigia_fio.py:556](app/chamados/services/vigia_fio.py:556), [vigia_fio.py:567](app/chamados/services/vigia_fio.py:567))

Sem `lista`, cada chamada cria sua instância: mesmo algoritmo, caches independentes, até dois carregamentos. A igualdade depende de obterem a mesma resposta da fonte; não há snapshot compartilhado. ([vigia_fio.py:339](app/chamados/services/vigia_fio.py:339), [vigia_fio.py:550](app/chamados/services/vigia_fio.py:550))

**2. Quem escreve e quem lê `fio_do_admin_sem_resposta`**

| Papel | Local |
|---|---|
| Acumula dias distintos sem resposta em um conjunto | [vigia_fio.py:208](app/chamados/services/vigia_fio.py:208) |
| Inicializa a chave com zero | [vigia_fio.py:316](app/chamados/services/vigia_fio.py:316) |
| Copia o tamanho do conjunto ao terminar `censo` | [vigia_fio.py:425](app/chamados/services/vigia_fio.py:425) |
| **Sobrescreve após os dois censos**, incluindo consultas exclusivas do alcance | [vigiar_fio_morto.py:169](app/chamados/management/commands/vigiar_fio_morto.py:169) |
| Lê para imprimir `FIO_DO_ADMIN…sem_resposta` | [vigia_fio.py:795](app/chamados/services/vigia_fio.py:795) |
| Transporta a chave no dicionário do carimbo | [vigiar_fio_morto.py:216](app/chamados/management/commands/vigiar_fio_morto.py:216) |
| Testes leem diretamente esperando 0, 1 e 3; outro compara o dicionário inteiro | [test_o261_vigia_fio.py:571](app/chamados/tests/test_o261_vigia_fio.py:571), [579](app/chamados/tests/test_o261_vigia_fio.py:579), [588](app/chamados/tests/test_o261_vigia_fio.py:588), [780](app/chamados/tests/test_o261_vigia_fio.py:780) |

O teste novo de falha lê **`lista.sem_resposta`**, sem conferir a chave final do command. ([test_o261e_f5_alcance.py:226](app/chamados/tests/test_o261e_f5_alcance.py:226))

`bin/fio_estado.py` não publica essa chave; sua comparação genérica só considera campos de `esperado`, onde ela também não consta. Assim, mudar somente esse contador no carimbo atual não muda sua saída. ([fio_estado.py:69](bin/fio_estado.py:69), [fio_estado.py:82](bin/fio_estado.py:82), [vigiar_fio_morto.py:95](app/chamados/management/commands/vigiar_fio_morto.py:95))

**Contra `21b9db38`: [não verificado]**, pois essa versão não está no material disponível. Contra o pai descrito no patch, existe mudança observável: se a lista falha e apenas o alcance consulta um dia, texto/carimbo passam de zero para um nessa chave; agora ela recebe a união dos dias consultados pelos dois censos. ([vigiar_fio_morto.py:167](app/chamados/management/commands/vigiar_fio_morto.py:167), [vigia_fio.py:208](app/chamados/services/vigia_fio.py:208))

**3. Classificação e força dos testes** [nao verificado]

Para **os mesmos resultados das fontes**, o delta fornecido preserva `(d)/(e)`: `(d)` exige colaborador na lista e dia fora dela; dia presente com ESTRUTURA/nenhum continua em `(e)` com detalhe. A mudança explícita é o rótulo publicado de `(d)`, agora `colab_na_lista_cxr_dia_fora`. ([vigia_fio.py:474](app/chamados/services/vigia_fio.py:474), [vigia_fio.py:588](app/chamados/services/vigia_fio.py:588), [vigia_fio.py:592](app/chamados/services/vigia_fio.py:592))

A causa `(b)` também preserva a expressão anterior: carimbo presente e texto vazio após `.strip()`. Houve extração do predicado, usado tanto na recusa da porta quanto na causa. ([vigia_fio.py:559](app/chamados/services/vigia_fio.py:559), [vigia_fio.py:584](app/chamados/services/vigia_fio.py:584), [vigia_fio.py:675](app/chamados/services/vigia_fio.py:675))

**Sim, testes novos passam com código errado:** [nao verificado]

- Criar outra `ListaDoAdmin` no command para o alcance, ou remover a atualização final do contador, escapa aos novos testes: eles passam a instância manualmente e não executam essa ligação do command. ([test_o261e_f5_alcance.py:206](app/chamados/tests/test_o261e_f5_alcance.py:206), [217](app/chamados/tests/test_o261e_f5_alcance.py:217), [vigiar_fio_morto.py:168](app/chamados/management/commands/vigiar_fio_morto.py:168))
- O selo AST aceita trocar `CADASTRO` por `ESTRUTURA`: suas contagens continuam iguais, embora o predicado fique errado. Testes anteriores rejeitam esse mutante; isso não significa sobrevivência à suíte inteira. ([test_o261e_f5_alcance.py:234](app/chamados/tests/test_o261e_f5_alcance.py:234), [vigia_fio.py:168](app/chamados/services/vigia_fio.py:168), [test_o261_vigia_fio.py:553](app/chamados/tests/test_o261_vigia_fio.py:553))

Ocorrência de dias produtivos afetados e resultado da suíte Django: **[não verificado]**. [nao verificado]

## 30/10 21:51 · sessao 17ddc5b11cdf · - · - · origem=Ronald (tmux)

Há lacunas nos testes novos, sem evidência de falha atual do código nesses pontos. [app/chamados/tests/test_o261e_f5_alcance.py:206-240] [nao verificado]

- **O selo AST aceita predicado errado.** Ele conta uma constante `BATIDA`, uma leitura de `DONOS_DA_MESA_DO_ADMIN` e chamadas simples `lista`/`dias_na_lista`; não verifica `CADASTRO`, o operador de pertinência nem a regra aplicada. Trocar a constante de `('BATIDA', 'CADASTRO')` para `('BATIDA', 'ESTRUTURA')` preserva todas as suas asserções, embora inverta indevidamente a aceitação de CADASTRO/ESTRUTURA. Demonstrei isso em memória, extraindo a classe e reproduzindo as contagens, sem gravar arquivos. [app/chamados/tests/test_o261e_f5_alcance.py:234-240; app/chamados/services/vigia_fio.py:168; app/chamados/services/vigia_fio.py:210]
- **Esse mutante não sobreviveria à suíte inteira:** testes anteriores exigem CADASTRO e rejeitam ESTRUTURA; o teste F5 anterior também rejeita ESTRUTURA. [app/chamados/tests/test_o261_vigia_fio.py:553-570; app/chamados/tests/test_o261e_f5_alcance.py:173-182]
- **A integração do command fica fora dos testes novos.** Eles criam `ListaDoAdmin` e a passam manualmente aos dois censos. Um mutante que crie outra lista no `censo_alcance` do command deixa esses testes intactos, mas reintroduz duas cargas na rodada real. [app/chamados/tests/test_o261e_f5_alcance.py:206-227; app/chamados/management/commands/vigiar_fio_morto.py:166-168]
- **A atualização final do contador no command também não é cobrada pelo teste novo de falha.** Remover `d['fio_do_admin_sem_resposta'] = lista.sem_resposta` não muda seu caminho de execução: ele verifica o objeto compartilhado e os contadores de fio morto, sem executar o command. Para revelar a diferença, seria necessário um dia sem resposta exclusivo do alcance, acrescentado depois do retorno de `censo`. [app/chamados/management/commands/vigiar_fio_morto.py:167-169; app/chamados/tests/test_o261e_f5_alcance.py:222-227]
- **O caso novo de falha mede apenas um dia compartilhado.** Uma implementação errada que devolvesse apenas `int(bool(_sem_resposta))` também satisfaria suas asserções; o teste anterior de três dias a reprovaria. [app/chamados/tests/test_o261e_f5_alcance.py:219-227; app/chamados/services/vigia_fio.py:225-227; app/chamados/tests/test_o261_vigia_fio.py:581-588]

A suíte Django não foi executada; execução e resultados completos permanecem **[não verificado]**. [nao verificado]

## 30/10 21:50 · sessao 6bf930cd4c2f · - · - · origem=Ronald (tmux)

- **Um sítio de carregamento** (`lista(...)`, seguido de `dias_na_lista(...)`) e **um sítio do predicado BATIDA/CADASTRO**, ambos em `ListaDoAdmin`; a cadeia compartilhada o chama uma vez em `dono_fora_do_fio`. Referências: `app/chamados/services/vigia_fio.py:198`, `:199`, `:210`, `:263`.

- O command passa **a mesma instância** aos dois censos. O carregador memoriza sucesso ou falha, portanto faz **no máximo uma tentativa por rodada**, somente se necessário; o mesmo `(colab_id, data_iso)` recebe o mesmo resultado de `dia_do_admin`. Referências: `app/chamados/management/commands/vigiar_fio_morto.py:166`, `:167`, `:168`; `app/chamados/services/vigia_fio.py:188`, `:190`, `:204`.

- Essa igualdade vale **para a consulta à lei do admin**, sem garantir igualdade do contador/balde final dos universos: `censo` sai antes se há pergunta viva; `censo_alcance` exige porta na fila ou pergunta acessível pela fila/app antes de consultar a lei. Referências: `app/chamados/services/vigia_fio.py:359`, `:367`, `:556`, `:559`, `:567`.

- Contraexemplo dedutível: dia passado e retido, chamado vivo silenciado, pergunta viva, nenhuma das três leis anteriores aplicável e dia BATIDA na lista. `censo` classifica `fio_vivo`; `censo_alcance`, sem porta, classifica `alcance_fio_do_admin`. O silêncio retira o chamado da fila. Isso é possibilidade do código, sem ocorrência produtiva demonstrada. Referências: `app/chamados/services/vigia_fio.py:359`, `:263`, `:559`, `:567`, `:570`; `app/chamados/services/painel.py:188`. Ocorrência em produção: **[não verificado]**.

- **Sem `lista`**, cada chamada constrói sua própria `ListaDoAdmin`: mesmo algoritmo, caches independentes e até duas tentativas de carregar. Com entradas e fonte estáveis, o predicado coincide; a implementação não garante o mesmo retrato se a lista mudar ou uma chamada falhar e a outra conseguir carregar. Referências: `app/chamados/services/vigia_fio.py:339`, `:550`, `:184`, `:198`, `:201`, `:209`, `:210`.

## 30/10 21:50 · sessao 6a378797b384 · - · - · origem=Ronald (tmux)

Q2 — rastreio:

- O estado nasce em `_sem_resposta=set()`, recebe pares distintos `(colab_id,data_iso)` somente em `dia_do_admin` quando `_dias is None` e é exposto por `len(set)` (`app/chamados/services/vigia_fio.py:185`, `:208`, `:227`).
- A chave nasce com `0` no dicionário de `censo`; recebe snapshot de `lista.sem_resposta` ao final normal; o command a sobrescreve após os dois censos, incluindo dias exclusivos do alcance (`app/chamados/services/vigia_fio.py:316`, `:425`; `app/chamados/management/commands/vigiar_fio_morto.py:166`–`:169`). No retorno antecipado de `censo`, permanece o zero inicial (`app/chamados/services/vigia_fio.py:323`–`:325`).
- `linhas(d)` lê a chave e publica `VFM|FIO_DO_ADMIN=...|sem_resposta=...`; o command chama esse leitor e inclui o `d` inteiro em `censo` do carimbo (`app/chamados/services/vigia_fio.py:793`–`:795`; `app/chamados/management/commands/vigiar_fio_morto.py:173`, `:216`).
- Os testes antigos leem a chave nos cenários sucesso/uma falha/três dias, esperando `0/1/3`; o teste de carimbo lê implicitamente por comparação do dicionário inteiro (`app/chamados/tests/test_o261_vigia_fio.py:571`, `:579`, `:588`, `:780`).
- O teste novo de falha lê **`lista.sem_resposta`**, não a chave devolvida nem o valor carimbado; usa o mesmo dia nos dois universos (`app/chamados/tests/test_o261e_f5_alcance.py:219`, `:226`).
- `bin/fio_estado.py` não publica essa chave: projeta campos específicos e compara genericamente apenas os presentes em `esperado`; o command não inclui `fio_do_admin_sem_resposta` em `ESPERADO_DO_CARIMBO` (`bin/fio_estado.py:69`–`:73`, `:82`–`:92`; `app/chamados/management/commands/vigiar_fio_morto.py:87`–`:95`). Portanto aumento só nessa chave não muda o retorno/linha desse leitor com o carimbo produzido atualmente (mesmas referências).

Mudança demonstrável **contra o pai fc9beece do patch fornecido**: se a lista falha, `censo` consulta zero dias e alcance consulta um dia, texto/carimbo passam de `sem_resposta=0` para `1`, pois o command agora copia a união após alcance (`app/chamados/management/commands/vigiar_fio_morto.py:167`–`:169`; anexo do usuário [não verificado] quanto à versão histórica executada). Contra **21b9db38**, falta a versão-base; conclusão exata [não verificado].

Lacuna de selo: remover a atribuição do command em `:169` não é detectado pelo novo teste da falha, que consulta só a propriedade e usa um dia comum, sem dia exclusivo do alcance (`app/chamados/tests/test_o261e_f5_alcance.py:217`–`:227`). O teste antigo que compara carimbo a `censo()` avulso deixa de ser uma identidade universal sob falha com dias exclusivos do alcance (`app/chamados/tests/test_o261_vigia_fio.py:780`; `app/chamados/management/commands/vigiar_fio_morto.py:168`–`:169`).

## 30/10 21:31 · sessao fb6e2bc2df4b · O261-E-CELULA-20 · verificador · origem=Code

**1. Classificação: a retirada dos dois filtros é semanticamente redundante com o juiz atual; não identifiquei mudança de dia decorrente dela.** As causas continuam sendo escolhidas sobre `ps`, já filtrado por vitalidade. ([vigia_fio.py:495](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:495), [vigia_fio.py:566](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:566))

A cadeia exata é:

- As perguntas vêm de `PerguntaDisputa.objects.filter(celula_id__in=_cels)`: identidade pela FK da célula, sem casamento por `data_evento`. Todas entram em `perg_todas`; somente as aprovadas por `pergunta_viva(p)` entram em `perg`. ([vigia_fio.py:495](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:495))
- `ps = perg.get(c, []) if c else []`. ([vigia_fio.py:531](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:531))
- `pergunta_viva` delega a `_pergunta_viva_calc`, cujo primeiro degrau retorna `False` para `validada_em is not None`. O memo inclui esse campo na chave. Logo, para toda pergunta admitida em `ps`, os termos removidos eram verdadeiros. ([juizes.py:795](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/juizes.py:795), [juizes.py:1006](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/juizes.py:1006), [juizes.py:768](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/juizes.py:768))

O julgamento do dono recebe **`perg_todas`**, preservando a leitura das validadas fora de `ps`. ([vigia_fio.py:549](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:549))

**2. Testes: há mordida, mas restam duas lacunas demonstráveis por inspeção.** [nao verificado]

- **Porta antes do teto: cobertura parcial.** O teste novo cria apenas chamado com célula e verifica `porta_fila`. Um mutante com ordem **`porta_fila → teto → porta_pergunta`** passa essa asserção, embora classifique incorretamente uma pergunta alcançável de hoje. Falta repetir o cenário com chamado **sem célula**, pergunta no app e âncora na fila. ([teste:211](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/tests/test_o261e_f5_alcance.py:211), [vigia_fio.py:532](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:532))
- **Dia listado sem dono: morde corretamente.** A assinatura preserva a chave do dia com valor `None`; o helper exige a linha da célula, e a asserção exige exatamente `('e', 'dia_na_lista_dono_nenhum')`. Portanto distingue a atribuição indevida a `(d)`. ([cadastro_realidade.py:669](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/escala/services/cadastro_realidade.py:669), [teste:98](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/tests/test_o261e_f5_alcance.py:98), [teste:217](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/tests/test_o261e_f5_alcance.py:217))
- **A garantia de `ps` não está protegida pelos dois testes novos:** nenhum cria pergunta. Eles passam se a inclusão em `perg` deixar de consultar o juiz. O contrato AST também não exige essa consulta: procura comparações cruas de carimbo. Falta um negativo com pergunta validada que seria `(b)/(c)` se admitida indevidamente. ([teste:211](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/tests/test_o261e_f5_alcance.py:211), [contrato:288](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/tests/test_contract_leitores_vitalidade.py:288))

**3. Quem escreve e quem lê: nenhum resultado desses consumidores muda sob a equivalência acima.**

| Papel | Caminho confirmado |
|---|---|
| Produz o dict | `censo_alcance` acumula total, causas, detalhes e lista. ([vigia_fio.py:588](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:588)) |
| Lê causas para impressão | `linhas_alcance` publica causas e detalhes; o comando imprime suas linhas. ([vigia_fio.py:610](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/services/vigia_fio.py:610), [comando:181](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/management/commands/vigiar_fio_morto.py:181)) |
| Escreve o carimbo | O comando guarda o dict inteiro em `alcance` e replica o total em `censo`; `_carimbar` serializa e substitui o arquivo. ([comando:211](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/management/commands/vigiar_fio_morto.py:211), [comando:130](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/management/commands/vigiar_fio_morto.py:130)) |
| Lê o carimbo para ESTADO | `fio_estado` compara `censo` com `esperado`, incluindo o total de alcance; não consulta suas causas. `gerar_estado` consome esse leitor. ([fio_estado.py:69](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/bin/fio_estado.py:69), [gerar_estado.py:260](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/bin/gerar_estado.py:260)) |

O alcance continua fora de `ALARMA_EM_0`; `gritou` é calculado antes de `alc`, usando o censo antigo. ([comando:81](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/management/commands/vigiar_fio_morto.py:81), [comando:163](/home/ronald/codex_ws/64eddfe4b5b344d55139c719de8d5ffbdf1b2b11/app/chamados/management/commands/vigiar_fio_morto.py:163))

Execução da suíte, sobrevivência empírica dos mutantes e leitores externos à cópia: **[nao verificado]**. [nao verificado]

## 30/10 21:30 · sessao 0d5e0d25562d · - · - · origem=Ronald (tmux)

Os dois casos novos têm mordida, mas a cobertura da ordem das portas é parcial.

- `test_MORDE_porta_vence_o_teto_de_hoje` cria chamado com célula e exige `(porta_fila, teto) == (1, 0)`, portanto distingue teto antes de `porta_fila`. Porém passa com mutante que coloca o teto **entre** `porta_fila` e `porta_pergunta`: o caso não cria pergunta; os casos existentes de pergunta usam `DIA=01/09` e `HOJE=25/09`, portanto também não exercitam pergunta de hoje. (`app/chamados/tests/test_o261e_f5_alcance.py:25`, `:26`, `:84`, `:120`, `:211`; ordem atual em `app/chamados/services/vigia_fio.py:532`, `:535`, `:538`.)

- `test_dia_na_lista_SEM_dono_lavrado_e_outra_e_nao_cadastro` é direto e não vazio: injeta assinatura com data sem dono; `dias_na_lista` guarda a chave com valor `None`; `_causa_do_dia` exige exatamente uma linha da célula; a asserção exige `('e','dia_na_lista_dono_nenhum')`. Mata a condição antiga de `(d)` que verificava só presença do colaborador e ignorava presença do dia. (`app/chamados/tests/test_o261e_f5_alcance.py:163`, `:217`, `:98`; `app/escala/services/cadastro_realidade.py:669`; `app/chamados/services/vigia_fio.py:570`, `:574`.)

A confiança em `ps` não é provada pelos testes F5: `_pergunta` nunca planta `validada_em`; os dois casos novos nem criam perguntas. Um mutante que substitua o filtro `if pergunta_viva(p)` por inclusão incondicional não é distinguido pelas fixtures examinadas. O contrato de vitalidade procura comparações de carimbo via AST; não exige que o consumidor chame o juiz, portanto esse mutante também passa sua condição estrutural. (`app/chamados/tests/test_o261e_f5_alcance.py:75`, `:211`; `app/chamados/services/vigia_fio.py:495`; `app/chamados/tests/test_contract_leitores_vitalidade.py:288`, `:299`.)

`AGORA` é literalmente 15:00 UTC, enquanto o decorator contém `12:00:00` sem `tz_offset`; nenhuma asserção confere a igualdade anunciada pelo comentário. Para as classificações examinadas, os usos são presença de `respondida_em` e silêncio deslocado em três dias; a fila continua consultando `timezone.now()`. (`app/chamados/tests/test_o261e_f5_alcance.py:28`, `:31`, `:73`, `:82`; `app/chamados/services/vigia_fio.py:525`; `app/chamados/services/painel.py:188`.)

Execução dos testes e sobrevivência empírica de mutantes: [não verificado]. [nao verificado]

## 30/10 21:30 · sessao 24bb4feb6c4f · - · - · origem=Ronald (tmux)

O delta não muda o formato do dict nem do carimbo; altera apenas os predicados que escolhem causas `b/c`, e o incremento de total permanece comum a todas as causas (`app/chamados/services/vigia_fio.py:566`, `:568`, `:588`, `:593`).

- **Escritor do dict:** `censo_alcance` inicializa contadores, causas e listas, depois acumula resultado por dia (`app/chamados/services/vigia_fio.py:458`, `:461`, `:588`).
- **Leitor direto produtivo:** `vigiar_fio_morto.handle` chama `censo_alcance`, passa o resultado a `linhas_alcance` e imprime as linhas (`app/chamados/management/commands/vigiar_fio_morto.py:181`).
- **Leitor das causas:** `linhas_alcance` lê os cinco contadores e `alcance_e_detalhe`; portanto uma eventual reclassificação mudaria essa saída (`app/chamados/services/vigia_fio.py:610`).
- **Escritor do carimbo:** o comando grava o dict inteiro em `registro['alcance']` e replica só o total em `registro['censo']['fio_morto_alcance']`; `_carimbar` serializa JSON e substitui o arquivo (`app/chamados/management/commands/vigiar_fio_morto.py:211`, `:213`, `:130`). Só a rodada sem recorte carimba (`app/chamados/management/commands/vigiar_fio_morto.py:200`, `:219`).
- **Leitor do carimbo:** `bin/fio_estado.py` compara dinamicamente `registro['censo']` contra `registro['esperado']`; o total de alcance participa dessa comparação porque está no esperado. Ele não usa causas/detalhes do bloco `alcance` (`bin/fio_estado.py:69`, `:72`; `app/chamados/management/commands/vigiar_fio_morto.py:94`). `gerar_estado` consome `ler/linha` desse módulo (`bin/gerar_estado.py:260`).
- **Alarme/saída:** alcance não integra `ALARMA_EM_0`; `gritou` é calculado com o censo antigo antes de obter `alc`. A mera redistribuição entre causas não muda o código de saída (`app/chamados/management/commands/vigiar_fio_morto.py:81`, `:163`, `:181`, `:222`). O cron configura `--alarme --alarme-passivo`, mas usa essas mesmas listas (`app/config/crons.py:1654`).
- **Testemunhas:** teste F5 verifica esperado, total nos dois lugares do carimbo e linha impressa, sem comparar causas/detalhes completos (`app/chamados/tests/test_o261e_f5_alcance.py:247`). O teste antigo compara o bloco `censo`, incluindo apenas o total de alcance (`app/chamados/tests/test_o261_vigia_fio.py:780`).

Assim, condicionada à equivalência semântica confirmada pelo juiz, nenhum desses leitores muda de resultado; se surgisse diferença entre `b/c/e`, ela apareceria nas linhas de causas e no bloco completo `alcance`, enquanto o painel/alarme continuaria usando o mesmo total (`app/chamados/services/vigia_fio.py:588`, `:610`; `app/chamados/management/commands/vigiar_fio_morto.py:211`; `bin/fio_estado.py:72`). Leitores externos à cópia e comportamento efetivo em produção: [nao verificado].

## 30/10 21:30 · sessao 86e7a786e1b8 · - · - · origem=Ronald (tmux)

Não encontrei mudança de classificação decorrente da retirada de `validada_em` em (b)/(c): ambas iteram exclusivamente `ps`, e toda pergunta de `ps` já passou por `pergunta_viva(p)`; esse juiz devolve `False` imediatamente quando `validada_em is not None`. Assim, nas duas condições antigas o termo removido era sempre verdadeiro. (`app/chamados/services/vigia_fio.py:497`, `app/chamados/services/vigia_fio.py:531`, `app/chamados/juizes.py:1006`)

Cadeia exata:

- O universo nasce dos dias de `fora_da_folha`, por empresa e competência; as células correspondentes são buscadas por colaborador/data. (`app/chamados/services/vigia_fio.py:466`, `app/chamados/services/vigia_fio.py:480`)
- A query de perguntas usa **FK `celula_id__in=_cels`**, com `select_related('disputa','celula')`; não deriva identidade por `data_evento`. (`app/chamados/services/vigia_fio.py:495`)
- `perg_todas` recebe todas; `perg` recebe apenas as aprovadas por `pergunta_viva`; `ps = perg.get(c, []) if c else []`. (`app/chamados/services/vigia_fio.py:496`, `app/chamados/services/vigia_fio.py:497`, `app/chamados/services/vigia_fio.py:531`)
- `pergunta_viva` delega a `_pergunta_viva_calc` em `juizes.py:795`; o primeiro degrau mata a validada em `juizes.py:1006`. O memo inclui `validada_em` na chave, portanto o cache normal não contorna esse degrau após mudança do campo. (`app/chamados/juizes.py:768`, `app/chamados/juizes.py:791`)
- O dono recebe **`perg_todas`**, necessário para reconhecer folga validada, e não o conjunto vivo `ps`; isso não foi removido. (`app/chamados/services/vigia_fio.py:549`, `app/chamados/services/vigia_fio.py:199`)
- (b) continua exigindo carimbo de resposta e texto vazio; (c) continua exigindo resposta carimbada e âncora terminal. A âncora vem de `chamado_destino_id`, com fallback para o chamado da disputa. (`app/chamados/services/vigia_fio.py:525`, `app/chamados/services/vigia_fio.py:566`, `app/chamados/services/vigia_fio.py:568`, `app/chamados/services/fio.py:116`)

A alteração resolve uma violação **estrutural** do contrato AST: ele procura comparação crua de `validada_em`/`materializada_em` com `None`, sem provar que o conjunto já foi filtrado. O teste contratual rejeitaria as duas leituras antigas mesmo sendo redundantes semanticamente. (`app/chamados/tests/test_contract_leitores_vitalidade.py:288`, `app/chamados/tests/test_contract_leitores_vitalidade.py:301`)

Nenhuma execução ou mutação realizada nesta exploração; comportamento observado em produção além dos anexos: [não verificado].

## 30/10 21:12 · sessao 2792b753efd0 · O261-E-CELULA-20 · verificador · origem=Code

**1. A extração não altera os contadores de `censo()`**, considerando os mesmos dados e respostas dos leitores. A cadeia preserva a precedência do pai transcrito no diff: decisão → ausência → folga validada → admin. ([vigia_fio.py:194](/home/ronald/codex_ws/6a8c5300a64ff11b7fa84c1584c27615dfe38cd9/app/chamados/services/vigia_fio.py:194))

| Comparação pai × novo | Resultado |
|---|---|
| Decisão | Mesmo `resolver_dia_oferecido_na_data(colab, data)`; incrementa o contador, acrescenta `c.pk` a `pendentes` e continua. `app/chamados/services/vigia_fio.py:194`, `app/chamados/services/vigia_fio.py:329` |
| Ausência | Mesmo `ausencia_cobre(..., para='cobranca')`, teste de não nulo e `status in ABERTAS`; mesmo incremento e `continue`. `app/chamados/services/vigia_fio.py:196`, `app/chamados/services/vigia_fio.py:333` |
| Folga | Mesmo `any`, estado `VALIDADA` e `_e_aceite_de_folga`; preservados contador, lista `celulas_escala_nao_descreve_o_dia`, limite e `continue`. `app/chamados/services/vigia_fio.py:199`, `app/chamados/services/vigia_fio.py:336` |
| Admin | Continua sendo o último teste; preservados contador, lista `celulas_fio_do_admin`, limite e `continue`. `app/chamados/services/vigia_fio.py:201`, `app/chamados/services/vigia_fio.py:341` |

Os argumentos são o mesmo colaborador, data e **todas** as perguntas da célula (`ps`). `lambda c=c: _fio_do_admin(c)` captura a célula corrente e mantém a chamada adiada; a falha da lista continua incrementando `fio_do_admin_sem_resposta` uma vez por dia que chega a esse teste. `app/chamados/services/vigia_fio.py:304`, `app/chamados/services/vigia_fio.py:328`, `app/chamados/services/vigia_fio.py:284`, `app/chamados/services/vigia_fio.py:297`.

Permanecem os filtros anteriores, os incrementos de morto, `celulas_do_fio_morto`, `classes_do_fio_morto` e a classificação posterior de `pendentes`; portanto, os contadores derivados também conservam suas entradas. `app/chamados/services/vigia_fio.py:315`, `app/chamados/services/vigia_fio.py:346`, `app/chamados/services/vigia_fio.py:358`, `app/chamados/services/vigia_fio.py:380`.

**2. Não encontrei contraexemplo residual no delta de `(d)`, dentro do contrato declarado.** O teste anterior de véu impede `(d)` com chamado vivo; `not ps` exige ausência de pergunta viva; a nova condição exige colaborador listado **e dia ausente**. `app/chamados/services/vigia_fio.py:497`, `app/chamados/services/vigia_fio.py:557`, `app/chamados/services/vigia_fio.py:569`.

Sem porta e vencidas as leis anteriores:

- Dia listado com BATIDA/CADASTRO sai para `alcance_fio_do_admin`. `app/chamados/services/vigia_fio.py:548`.
- Dia listado com ESTRUTURA/sem dono, sem pergunta viva, recebe `(e, dia_na_lista_dono_estrutura|nenhum)`. A presença da chave distingue “sem dono” de “dia ausente”. `app/chamados/services/vigia_fio.py:573`, `app/escala/services/cadastro_realidade.py:670`.
- Sem célula recebe `(e, sem_celula)`; com pergunta viva segue as causas da pergunta. `app/chamados/services/vigia_fio.py:560`, `app/chamados/services/vigia_fio.py:564`, `app/chamados/services/vigia_fio.py:579`.

Nenhum dia do universo some ou duplica no processamento normal: `setdefault((colab, data), ...)` deduplica a identidade; cada chave passa uma vez pelo laço; cada saída incrementa um balde e continua, ou incrementa exatamente um morto e uma causa. `limite` trunca somente a lista. `app/chamados/services/vigia_fio.py:471`, `app/chamados/services/vigia_fio.py:529`, `app/chamados/services/vigia_fio.py:532`, `app/chamados/services/vigia_fio.py:551`, `app/chamados/services/vigia_fio.py:587`.

O teste cobre explicitamente ESTRUTURA, mas falta a asserção correspondente a `dia_na_lista_dono_nenhum`. `app/chamados/tests/test_o261e_f5_alcance.py:174`.

**3. Sim: o mutante que antecipa hoje sobrevive à cobertura do arquivo, por análise estática.** Os outros três possuem asserções que os detectariam. `app/chamados/tests/test_o261e_f5_alcance.py:114`, `app/chamados/tests/test_o261e_f5_alcance.py:141`, `app/chamados/tests/test_o261e_f5_alcance.py:185`, `app/chamados/tests/test_o261e_f5_alcance.py:193`.

| Mutante | Resultado pela leitura dos testes | [nao verificado]
|---|---|
| Hoje antes das portas | **Sobrevive:** portas são testadas no dia passado; o caso `hoje=DIA` não cria porta. Falta cruzar hoje com cada porta e conferir o balde. `app/chamados/tests/test_o261e_f5_alcance.py:25`, `:114`, `:121`, `:206` |
| Remover filtro do app | Detectado pelo negativo fora do app: surgiria uma porta indevida. `app/chamados/tests/test_o261e_f5_alcance.py:185` |
| Remover exclusão da âncora na fila do véu | Detectado pela resposta vazia: mudaria `(b)` para `(a)`; também quebra o caso fora do app. `app/chamados/tests/test_o261e_f5_alcance.py:141`, `:185`; `app/chamados/services/vigia_fio.py:557` |
| Contar `alcance_lista_sem_resposta` duas vezes | Detectado pelo `assertEqual(1, ...)` específico; as somas gerais não incluem esse contador. `app/chamados/tests/test_o261e_f5_alcance.py:197`, `:92` |

Execução da suíte, execução dos mutantes e ocorrência desses cenários em produção: **[nao verificado]**. [nao verificado]

## 30/10 21:11 · sessao 4caceba3830f · - · - · origem=Ronald (tmux)

Não encontrei erro residual na correção de (d), considerando o contrato declarado e as fontes da cópia:

- **Sem pergunta viva + colaborador listado + dia fora da lista:** cai em (d). O `elif veu` anterior impede atribuir (d) quando há chamado vivo; `ps` contém somente perguntas vivas. `app/chamados/services/vigia_fio.py:494-498`, `:557-570`.
- **Dia listado, dono BATIDA/CADASTRO:** sai para `alcance_fio_do_admin`; **ESTRUTURA ou None**, sem pergunta viva, permanece em (e), detalhe `dia_na_lista_dono_estrutura` ou `dia_na_lista_dono_nenhum`. `app/chamados/services/vigia_fio.py:543-553`, `:573-576`. A leitura conserva presença e dono lavrado do dia, sem derivação nova. `app/escala/services/cadastro_realidade.py:655-670`.
- **Com pergunta viva:** não entra em (d) nem no novo detalhe do dia listado; segue as causas e detalhes da pergunta. **Sem célula:** recebe `(e, sem_celula)`, antes de (d). São precedências expressas no código. `app/chamados/services/vigia_fio.py:560-586`.
- **Nenhum dia do universo some ou duplica no processamento normal:** o universo é um dicionário por `(colab, data)`; cada chave é percorrida uma vez; as portas, hoje e quatro leis incrementam um balde e continuam; o restante incrementa exatamente um morto e uma causa. O limite trunca apenas a amostra. `app/chamados/services/vigia_fio.py:465-475`, `:529-553`, `:587-592`.
- **A competência não duplica dias:** as janelas consecutivas têm início no corte e fim no corte menos um do mês seguinte; as datas vêm da mesma janela usada por `fora_da_folha`. `app/ponto/janelas.py:10-21`, `:54-72`; `app/folha/fora_da_folha.py:119-139`, `:168`.
- **Detalhe literal `outra`:** não identifiquei entrada regular que o alcance. Sem véu, terminal ou âncora ausente, as âncoras têm de estar na fila; se alguma pergunta aparece no app e tem resposta válida, já seria porta; se tem resposta vazia, cai em (b). Pergunta viva nunca tem `validada_em` preenchida. `app/chamados/catalogo/motor.py:19-31`; `app/chamados/juizes.py:1005-1007`; `app/chamados/services/vigia_fio.py:535`, `:557-586`. Estado fora do catálogo ou inconsistência de FK em produção: [nao verificado].

O teste já verifica explicitamente ESTRUTURA contra (d), mas não inclui o ramo `None`/`dia_na_lista_dono_nenhum`. `app/chamados/tests/test_o261e_f5_alcance.py:174-183`.

## 30/10 21:10 · sessao a3d62278a9a7 · - · - · origem=Ronald (tmux)

**Q1: a extração preserva os valores de `censo()` para os mesmos dados e respostas dos leitores.** As quatro condições do pai no diff anexo aparecem na mesma ordem e com os mesmos argumentos no helper: `resolver_dia_oferecido_na_data(colab, data)`; `ausencia_cobre(colab, data, para='cobranca')` seguida do teste `status in ABERTAS`; `any(p.estado == p.VALIDADA and _e_aceite_de_folga(p)...)`; chamada do admin. Cada retorno encerra a cadeia no primeiro resultado verdadeiro. (`app/chamados/services/vigia_fio.py:194`, `:196`, `:199`, `:201`.)

| Item comparado ao pai do diff anexo | Resultado na cópia |
|---|---|
| Ponto da extração | Continua depois dos filtros de outro vocabulário, fio vivo e hoje/futuro; esses filtros mantêm seus incrementos e `continue`. (`app/chamados/services/vigia_fio.py:315`, `:320`, `:323`.) |
| Argumentos | São o mesmo colaborador, a mesma data e a lista completa `ps`, obtida por célula. (`app/chamados/services/vigia_fio.py:304`, `:314`, `:328`.) |
| Lambda do admin | `lambda c=c: _fio_do_admin(c)` captura a célula corrente e só chama `_fio_do_admin` se as três leis anteriores falharem; não antecipa a consulta da lista. (`app/chamados/services/vigia_fio.py:328`, `:201`.) |
| Decisão pendente | Incrementa o mesmo contador, acrescenta `c.pk` a `pendentes` e continua. (`app/chamados/services/vigia_fio.py:329`.) |
| Ausência aberta | Incrementa o mesmo contador e continua. (`app/chamados/services/vigia_fio.py:333`.) |
| Folga validada | Incrementa o mesmo contador; preserva `len(lista) < limite`, o mesmo `c.pk` e o `continue`. (`app/chamados/services/vigia_fio.py:336`.) |
| Fio do admin | Incrementa o mesmo contador; preserva `len(lista) < limite`, o mesmo `c.pk` e o `continue`. (`app/chamados/services/vigia_fio.py:341`.) |
| Falha da lista | Continua armazenada uma vez em `_na_lista`, e cada dia que efetivamente chega ao admin incrementa `fio_do_admin_sem_resposta` uma vez. (`app/chamados/services/vigia_fio.py:284`, `:296`, `:297`.) |
| Nenhuma lei satisfeita | Cai nos mesmos incrementos de morto, abertura por ausência/presença de perguntas, cobrança e listas. (`app/chamados/services/vigia_fio.py:346`.) |
| Contadores derivados | `mortos` segue alimentando a classificação; `pendentes` segue alimentando `decisao_do_dia_pendente_com_alvo`, sem mudança nas operações segundo o diff. (`app/chamados/services/vigia_fio.py:358`, `:380`.) |

Os testes antigos contêm verificações para precedência decisão/ausência, BATIDA/CADASTRO contra ESTRUTURA/sem dono, falha da lista uma vez por rodada e a lista da folga validada; a execução deles nesta análise não foi realizada **[nao verificado]**. (`app/chamados/tests/test_o261_vigia_fio.py:531`, `:553`, `:581`, `:626`.)

A equivalência com o pai foi examinada pelo **diff fornecido**, não por checkout ou execução da revisão pai **[nao verificado]**.

## 30/10 21:10 · sessao 295113302444 · - · - · origem=Ronald (tmux)

O mutante **“hoje antes das portas” sobrevive à cobertura deste arquivo por análise estática**: as portas são plantadas em `DIA=01/09`, enquanto `HOJE=25/09`; o único caso que chama `hoje=DIA` não cria porta. Falta cruzar **dia de hoje + porta existente**, conferindo qual balde recebe o dia (`app/chamados/tests/test_o261e_f5_alcance.py:25`, `:26`, `:114`, `:121`, `:206`; ordem atual em `app/chamados/services/vigia_fio.py:532`, `:535`, `:538`).

| Mutante | Resultado estático | Evidência |
|---|---|---|
| Mover teto de hoje antes das portas | Sobrevive à cobertura do arquivo; nenhum caso cruza hoje com porta. | `app/chamados/tests/test_o261e_f5_alcance.py:114`, `:121`, `:206` |
| Remover `p.pk in no_app(cid)` da porta | Morto pelo negativo fora do app: o mutante produz porta e perde a linha esperada em `(e)`. | `app/chamados/services/vigia_fio.py:535`; `app/chamados/tests/test_o261e_f5_alcance.py:185`, `:190`, `:191` |
| Remover `chamado_que_lista(p) not in fila_pk` do véu | Morto pelo caso de resposta vazia: a âncora `aberto`, sem célula e na fila, passa a compor o véu; `(a)` ganha precedência sobre `(b)`. Também quebra o negativo fora do app, de `(e)` para `(a)`. | `app/chamados/tests/test_o261e_f5_alcance.py:141`, `:142`, `:146`, `:185`, `:191`; `app/chamados/services/vigia_fio.py:557`, `:562`, `:564`; `app/chamados/catalogo/motor.py:24` |
| Incrementar `alcance_lista_sem_resposta` duas vezes por dia | Morto: teste da lista falhando exige exatamente `1`. | `app/chamados/services/vigia_fio.py:543`, `:546`; `app/chamados/tests/test_o261e_f5_alcance.py:193`, `:197` |

As somas auxiliares não detectam duplicação de `alcance_lista_sem_resposta`, pois esse contador não participa delas; quem detecta é a asserção específica `==1` (`app/chamados/tests/test_o261e_f5_alcance.py:90`, `:92`, `:95`, `:97`, `:197`).

Resultado de execução dos testes/mutantes: **[nao verificado]**. [nao verificado]

_771 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
