# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 23:52** · publicado em **15/11/2026 23:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 23:52 · sessao 323261a980c3 · ESPELHO-VERDADE-FECHO-V0 · leitura · origem=Code

Estados existentes: `FEITO`, `EM_CURSO`, `FALTA` e `ESPERA='ESPERA_RONALD'` no espelho; `FECHADO`, `PARCIAL` e `PENDENTE` no estrutural. Contei os estados declarados: `EM_CURSO`/`ESPERA_RONALD` e todo resultado diferente de `FECHADO`. (app/core/espelho_verdade.py:35; app/core/placar_estrutural.py:45)

Na coluna de obra, **“sem obra” significa sem vínculo comprovado de fechamento da pendência atual**. Registrei também obras relacionadas, sem afirmar que encerram os casos atuais quando isso está [nao verificado]. O54 é o programa geral; R1-REMEDE fechou a medição, distinguindo cinco itens medidos e quatro fora de alcance. (app/docs/BACKLOG.md:1395; app/docs/BACKLOG.md:1854)

| fonte arquivo:linha | id do item | estado literal | obra que fecha ou “sem obra” | estado da obra | o que destrava | dono — origem |
|---|---|---|---|---|---|---|
| app/core/espelho_verdade.py:49 | E1-passivo-53 | `EM_CURSO` | **O50** — app/docs/GRADE-MESTRA.md:862 | `PARADA (passivo espera !)` — app/docs/GRADE-MESTRA.md:862 | O passivo depende do `!` caso a caso: a prova distingue 43 congelados e 10 livres ao propositor. (app/core/espelho_verdade.py:57–58) | **Ronald**, para o `!`; executor: **nao nomeado**. (app/docs/BACKLOG.md:1388) |
| app/core/espelho_verdade.py:61 | E1-sem-previsao | `EM_CURSO` | **sem obra de fecho específico comprovada**; O54 inclui expressamente censo e tratamento das células sem previsão, mas vínculo de fechamento dos 35/28 atuais [nao verificado]. (app/docs/BACKLOG.md:1395; app/core/espelho_verdade.py:61) | **nao nomeado** para fecho específico; O54: `E0 em curso`. (app/docs/BACKLOG.md:1395) | Zerar dia de colaborador ativo sem previsão ou com previsão de vínculo inválido é a porta declarada. (app/core/espelho_verdade.py:40–41) | **nao nomeado** na medição atual. (app/core/espelho_verdade.py:61) |
| app/core/espelho_verdade.py:94 | E3-turno-aberto | `EM_CURSO` | **sem obra de fecho comprovada**; O86 investiga turnos abertos, mas sua correspondência com os atuais 347/262 dias [nao verificado]. (app/docs/BACKLOG.md:1427; app/core/espelho_verdade.py:94) | **nao nomeado** para fecho atual; O86: `REGISTRADA (guarda com DIFF zero; investigação residual)`. (app/docs/GRADE-MESTRA.md:881) | **nao nomeado**: a prova atual mede dias ímpares e casos fora da faixa, sem declarar qual ato encerra a pendência. (app/core/espelho_verdade.py:94) | **nao nomeado** para os casos atuais. (app/core/espelho_verdade.py:94) |
| app/core/espelho_verdade.py:96 | E3-separados-30 | `ESPERA_RONALD` — constante `ESPERA` em app/core/espelho_verdade.py:35 | **CORTE-B-30** — app/docs/GRADE-MESTRA.md:845 | `PARADA (congelada)` — app/docs/GRADE-MESTRA.md:845 | Descongelamento e corte do lote B são as dependências registradas. (app/docs/GRADE-MESTRA.md:845) | **outro — Gabriel**, segundo a prova; Ronald aparece no estado literal de espera. (app/core/espelho_verdade.py:96) |
| app/core/espelho_verdade.py:143 | E4-rotulo-do-desenho | `EM_CURSO` | **sem obra** de execução nomeada; ESPELHO-VERDADE-REMEDIDO registra a tarefa, mas sua entrega é atualizar provas. (app/docs/BACKLOG.md:1579; app/docs/GRADE-MESTRA.md:89) | **nao nomeado** para execução. (app/docs/BACKLOG.md:1579) | Falta a pílula de `templates/escala/tipos_lista.html:42` ler `desenho_do_turno`, na fila 2. (app/core/espelho_verdade.py:143) | **nao nomeado**. (app/core/espelho_verdade.py:143) |
| app/core/espelho_verdade.py:151 | E5-le-da-celula | `EM_CURSO` | **O48** — app/docs/GRADE-MESTRA.md:860; complemento **STATUS-ENCERRADO-DA-COMPETENCIA-NAO-EXISTE** — app/docs/BACKLOG.md:2578 | O48: `REGISTRADA (passos no ar; status e exportação pendentes)` — app/docs/GRADE-MESTRA.md:860 | Falta o status `encerrado`; escrever esse estado nas linhas da 10 exige DIFF e `!`, enquanto migration e censo não pedem aval. (app/core/espelho_verdade.py:151; app/docs/BACKLOG.md:2578) | **Ronald**, para autorizar a escrita; execução: **nao nomeado**. (app/docs/BACKLOG.md:2578) |
| app/core/espelho_verdade.py:181 | E6-sistema-bate | `EM_CURSO` | **sem obra de fecho atual comprovada**; R1-ESTRUTURA-91 existe, mas causa/destino dos atuais E26/E9 não foram medidos. (app/docs/GRADE-MESTRA.md:102; app/core/placar_estrutural.py:130) | **nao nomeado** para fecho atual; R1-ESTRUTURA-91: `REGISTRADA (restante pendente)`. (app/docs/GRADE-MESTRA.md:102) | Com batidas completas, espelho e oráculo precisam bater; a divergência é declarada “bug nosso”. (app/core/espelho_verdade.py:180) | **ESTRUTURA**, explicitamente medida na prova. (app/core/espelho_verdade.py:181) |
| app/core/espelho_verdade.py:185 | E6-exportavel | `EM_CURSO` | **sem obra de fecho atual comprovada**; O54 contém a certificação geral e O125 trata destino administrativo, mas fechamento dos casos atuais [nao verificado]. (app/docs/BACKLOG.md:1395; app/docs/GRADE-MESTRA.md:894) | **nao nomeado** para fecho atual; O125: `REGISTRADA`. (app/docs/GRADE-MESTRA.md:894) | Certificação pelo oráculo é a condição do item; a entrada efetiva dos divergentes na lista do admin não foi medida. (app/core/espelho_verdade.py:184–185) | **ESTRUTURA / CADASTRO / BATIDA**: fila 1 para ESTRUTURA e lista do admin para os demais. (app/core/espelho_verdade.py:185) |
| app/core/espelho_verdade.py:191 | E6-piso-185 | `EM_CURSO` | **sem obra de fecho atual comprovada**; O83 registra classes anteriores, mas causa/destino dos atuais E26/E9 não estão nomeados. (app/docs/BACKLOG.md:1438; app/core/espelho_verdade.py:191; app/core/placar_estrutural.py:130) | **nao nomeado** para fecho atual; O83: `REGISTRADA (curso atual deste id não demonstrado)`. (app/docs/GRADE-MESTRA.md:879) | Falta nomear a causa por colaborador, incluindo os 26+9 dias de ESTRUTURA cuja causa a prova não nomeia. (app/core/espelho_verdade.py:190–191) | **ESTRUTURA / CADASTRO / BATIDA**, na decomposição dos divergentes; causa dos casos ESTRUTURA: **nao nomeado**. (app/core/placar_estrutural.py:118; app/core/espelho_verdade.py:191) |
| app/core/placar_estrutural.py:74 | BATERIA | `PARCIAL` | **sem obra** comprovada para a meta integral; BATERIA-QUEIXAS-ADMIN cobre quatro queixas, sem declarar fechamento de todas as famílias. (app/docs/BACKLOG.md:1867; app/core/placar_estrutural.py:79–100) | **nao nomeado** para meta integral; BATERIA-QUEIXAS-ADMIN: `REGISTRADA`. (app/docs/GRADE-MESTRA.md:103) | Todas as famílias precisam ter bateria declarada e verde, sem allowlist, e nenhum dia ESTRUTURA novo pode ficar sem destino. (app/core/placar_estrutural.py:79–80) | **nao nomeado** para execução integral. (app/core/placar_estrutural.py:74–111) |
| app/core/placar_estrutural.py:146 | R3 | `PARCIAL` | **PLACAR-ESTRUTURAL** — app/docs/GRADE-MESTRA.md:62; O130 já consta no ar. (app/docs/BACKLOG.md:1280) | `PARADA (resultados parciais; espera autorização)` — app/docs/GRADE-MESTRA.md:62 | A grade precisa alcançar os mesmos 48/48 dias de cartão/tela/API; o corte manda manter R3 aberto por divergência de leitor. (app/docs/BACKLOG.md:338–342) | **ESTRUTURA**, nomeado no corte. (app/docs/BACKLOG.md:341) |
| app/core/placar_estrutural.py:211 | R4 | `PARCIAL` | **PLACAR-ESTRUTURAL** — app/docs/GRADE-MESTRA.md:62; **O269** cobre a relavra da 09, sem comprovação de fechamento integral do R4. (app/docs/BACKLOG.md:1611) | PLACAR-ESTRUTURAL: `PARADA (resultados parciais; espera autorização)`; O269: `PORTAO DE DATA: apply so apos 14/10 18h`. (app/docs/GRADE-MESTRA.md:62; app/docs/BACKLOG.md:1611) | A meta é ZERO em cada par; há registros distintos de Pauta DP sem tocar a 09 e de relavra posterior condicionada na O269. (app/core/placar_estrutural.py:215; app/docs/BACKLOG.md:334–337; app/docs/BACKLOG.md:1611) | **ESTRUTURA** para os seis casos novos; **Ronald** para os cortes sobre a 09. (app/core/placar_estrutural.py:291–292; app/docs/BACKLOG.md:334–337,1611) |
| app/core/placar_estrutural.py:433 | R6 | `PARCIAL` | **PLACAR-ESTRUTURAL**, incluindo lote do R6 — app/docs/GRADE-MESTRA.md:62; **O35**, para contratos — app/docs/GRADE-MESTRA.md:896 | PLACAR-ESTRUTURAL: `PARADA (resultados parciais; espera autorização)`; O35: `REGISTRADA (partes construídas; contratos incompletos)`. (app/docs/GRADE-MESTRA.md:62,896) | Chegar a 20/20 contratos e zero passo manual; o registro declara que resta o lote. (app/core/placar_estrutural.py:434–440) | **nao nomeado** como responsável global. (app/docs/GRADE-MESTRA.md:62,896) |

**Contagem do recorte:** 8 `EM_CURSO`, 1 `ESPERA_RONALD` e 4 `PARCIAL`: **13 linhas**. Pela definição conservadora acima, **7 ficaram “sem obra” de fechamento comprovado**: E1-sem-previsao, E3-turno-aberto, E4-rotulo-do-desenho, E6-sistema-bate, E6-exportavel, E6-piso-185 e BATERIA. (app/core/espelho_verdade.py:49,61,94,96,143,151,181,185,191; app/core/placar_estrutural.py:74,146,211,433; vínculos documentais na tabela)

**Espera Ronald:**

- **E1-passivo-53:** `!` caso a caso para os dez livres; O50 mantém o passivo esperando autorização. (app/core/espelho_verdade.py:58; app/docs/BACKLOG.md:1388)
- **E5-le-da-celula:** `!` para escrever `encerrado` nas linhas da 10 após export; migration e censo estão dispensados desse aval. (app/docs/BACKLOG.md:2578)
- **R4:** a linha do programa ainda registra espera de `!`, e as portas de demissão/encerramento de vínculo também o exigem; há cortes separados para Pauta DP e para O269, sem prova aqui de satisfação integral dessas condições. (app/docs/BACKLOG.md:1230; app/core/placar_estrutural.py:389–390; app/docs/BACKLOG.md:334–337,1611)
- **E3-separados-30 — ressalva:** consta literalmente `ESPERA_RONALD`, mas a prova diz “09 está com o Gabriel”; a decisão concreta ainda exigida de Ronald é **nao nomeado** nessa prova. (app/core/espelho_verdade.py:96; app/docs/BACKLOG.md:1579)

R3 tem resposta registrada: permanece aberto pela divergência da grade, com dono ESTRUTURA; a redação antiga sobre não carimbar a própria meta não demonstra uma espera atual de Ronald. (app/docs/BACKLOG.md:338–342; app/core/placar_estrutural.py:185–188)

**perguntas de lei: 0**, no recorte acima. As antigas perguntas referidas pelo R6 têm respostas posteriores registradas; a pergunta sobre dia aberto bloquear a folha consta fechada pela L-118, e “SEM juiz” na tabela de consulta não constitui, por si, lei faltante. (app/core/placar_estrutural.py:593; app/docs/BACKLOG.md:1538,1546,1742; app/docs/JUIZES.md:7–12,105,109)

## 15/11 23:50 · sessao fc196c01b64b · - · - · origem=Ronald (tmux)

Constantes em `app/core/espelho_verdade.py:35`: `FEITO='FEITO'`, `EM_CURSO='EM_CURSO'`, `FALTA='FALTA'`, `ESPERA='ESPERA_RONALD'`. Seleção: **8 EM_CURSO + 1 ESPERA_RONALD = 9 itens**, confirmada também pelo balanço de `app/docs/BACKLOG.md:1854`.

| item / fonte | vínculo documental comprovado | destrava / dono |
|---|---|---|
| E1-passivo-53 — `app/core/espelho_verdade.py:48`, estado em `:49` | **O50**, `app/docs/GRADE-MESTRA.md:862`, estado literal `PARADA (passivo espera !)`; registro detalhado em `app/docs/BACKLOG.md:1388`, estado `! para o passivo dos 62 (DRY por colab no RELATO); a fatia de codigo espera vez` | Passivo: 43 congelados e 10 livres ao propositor com `!` caso a caso, `app/core/espelho_verdade.py:57` e `:58`; dono **Ronald para o !**, pelo corte nominal em `:49`; execução de cadastro pelo propositor, `:58`. |
| E1-sem-previsao — `app/core/espelho_verdade.py:60`, estado em `:61` | O programa **O54** contém literalmente “censo e cura das celulas de trabalho sem previsao valida”, `app/docs/BACKLOG.md:1395`; existe em `app/docs/GRADE-MESTRA.md:72`, estado `REGISTRADA (programa parcial pelas fontes vivas...)`. Obra específica para os **35/28 atuais**: não nomeada em `app/core/espelho_verdade.py:61`. | Zerar dias sem previsão válida é porta E1, `app/core/espelho_verdade.py:40`; causa/dono atuais **nao nomeado**, `:61`. |
| E3-turno-aberto — `app/core/espelho_verdade.py:93`, estado em `:94` | **O86**, `app/docs/GRADE-MESTRA.md:881`, estado `REGISTRADA (guarda com DIFF zero; investigação residual)`, trata diretamente turnos abertos, `app/docs/BACKLOG.md:1427`. Entretanto o registro não liga seus casos antigos aos **347/262 atuais**; fechamento integral [nao verificado]. | A fonte atual mede ímpares e fora da faixa, sem nomear execução/dono, `app/core/espelho_verdade.py:94`. O diagnóstico antigo da O86 separa saída não registrada de geometria e cadastro, `app/docs/BACKLOG.md:1427`; não transportar essa classificação para a medida atual. |
| E3-separados-30 — `app/core/espelho_verdade.py:95`, estado em `:96` | **CORTE-B-30**, `app/docs/GRADE-MESTRA.md:845`, estado `PARADA (congelada)`; `app/docs/BACKLOG.md:1224`, estado `CONGELADA (L-096) -- idem: lista colab a colab, e a obra corre`. | Descongelamento/corte do lote B, `app/docs/GRADE-MESTRA.md:845`; o item diz **ESPERA_RONALD**, mas sua prova diz “09 esta com o **Gabriel**”, `app/core/espelho_verdade.py:96`; mesmo registro em `app/docs/BACKLOG.md:1579`. |
| E4-rotulo-do-desenho — `app/core/espelho_verdade.py:142`, estado em `:143` | **sem obra específica de execução identificada**. A tarefa consta em **ESPELHO-VERDADE-REMEDIDO**, `app/docs/BACKLOG.md:1579`, mas essa obra atualiza provas, conforme `app/docs/GRADE-MESTRA.md:89`; não demonstra executar a mudança da pílula. | Pílula de `templates/escala/tipos_lista.html:42` ler `desenho_do_turno`, fila 2/tela, `app/core/espelho_verdade.py:143`; dono **nao nomeado**, mesma fonte. |
| E5-le-da-celula — `app/core/espelho_verdade.py:150`, estado em `:151` | **O48**, `app/docs/GRADE-MESTRA.md:860`, estado `REGISTRADA (passos no ar; status e exportação pendentes)`; registro detalhado em `app/docs/BACKLOG.md:1386`. | Falta status `encerrado`, `app/core/espelho_verdade.py:151`; detalhado como migration+censo e pós-export, `app/docs/BACKLOG.md:1386`. Dono atual **nao nomeado**; a mesma linha preserva espera de `!` para passo 5 e leitura do DIFF pelo Ronald. |
| E6-sistema-bate — `app/core/espelho_verdade.py:180`, estado em `:181` | **R1-ESTRUTURA-91** é relacionada: `app/docs/GRADE-MESTRA.md:102`, estado `REGISTRADA (restante pendente)`; `app/docs/BACKLOG.md:1822` descreve famílias antigas. Causa/destino dos **E26+E9 atuais não medidos**, explicitamente `app/core/placar_estrutural.py:130`; obra que fecha esses casos integralmente **nao nomeada**. | Com batidas completas, espelho e oráculo baterem; divergência é “bug nosso”, `app/core/espelho_verdade.py:180`. Dono **ESTRUTURA**, explicitamente `:181`. |
| E6-exportavel — `app/core/espelho_verdade.py:184`, estado em `:185` | Programa **O54** contém certificação E6 e portas, `app/docs/BACKLOG.md:1395`, existe em `app/docs/GRADE-MESTRA.md:72`. **O125** trata alcance da lista, `app/docs/GRADE-MESTRA.md:894` / `app/docs/BACKLOG.md:1289`, mas de gap antigo de sete colaboradores; vínculo aos atuais [nao verificado]. | Certificação e destino conforme dono; entrada efetiva na lista administrativa não medida, `app/core/espelho_verdade.py:185`. Donos **ESTRUTURA / CADASTRO / BATIDA**, literais na mesma linha. |
| E6-piso-185 — `app/core/espelho_verdade.py:190`, estado em `:191` | **O83** trata classes de piso anteriores: `app/docs/GRADE-MESTRA.md:879`, estado `REGISTRADA (curso atual deste id não demonstrado)`; `app/docs/BACKLOG.md:1438`, estado `em curso`. Fechamento dos **E26+E9 atuais não nomeado**, `app/core/espelho_verdade.py:191` e `app/core/placar_estrutural.py:130`. | Nomear causa por colaborador, `app/core/espelho_verdade.py:190`; causas ESTRUTURA atuais não nomeadas, `:191`. Donos **ESTRUTURA / CADASTRO / BATIDA** na decomposição atual de `app/core/placar_estrutural.py:130`. |

Cuidados para consolidação:

- **R1-REMEDE não fecha nenhum desses itens**: está `FECHADA` como medição e registra cinco com número e quatro fora de alcance, `app/docs/BACKLOG.md:1854`.
- **O91 não fecha o passivo**: trata contador/instalação, `app/docs/BACKLOG.md:1433`; o espelho registra que instalação já ocorreu e a prova estava velha, `app/core/espelho_verdade.py:49`–`:59`.
- A contagem de “sem obra” depende de aceitar o programa geral **O54** e obras históricas apenas relacionadas como “obra que fecha”. Para vínculo integral comprovado aos números atuais, faltam vínculos específicos em E1-sem-previsao, E3-turno-aberto, E4-rotulo-do-desenho, E6-sistema-bate, E6-exportavel e E6-piso-185; não converter relação temática em prova de fechamento. As lacunas estão expressas nas respectivas fontes acima.
- Espera Ronald inequívoca: **E1-passivo-53**, `app/core/espelho_verdade.py:58` / `app/docs/GRADE-MESTRA.md:862`; **E3-separados-30** pelo estado literal, com ressalva Gabriel, `app/core/espelho_verdade.py:96`. **E5/O48** preserva espera de `!` na obra, mas o impedimento específico do item é status encerrado, `app/core/espelho_verdade.py:151` / `app/docs/BACKLOG.md:1386`.

## 15/11 23:50 · sessao 601f0f0373d8 · - · - · origem=Ronald (tmux)

Para o recorte dos 9 itens do espelho e dos 4 resultados parciais, recomendo **“perguntas de lei: 0”**: nenhuma lei ainda faltante foi confirmada nas pendências selecionadas; as duas antigas referidas em `app/core/placar_estrutural.py:593` têm respostas posteriores registradas em `app/docs/BACKLOG.md:1538`, `app/docs/BACKLOG.md:1546` e `app/docs/BACKLOG.md:1742`. A família escala também consta fechada em `app/docs/JUIZES.md:129`.

**Espera Ronald — registros confirmados:**

| Item | Espera escrita |
|---|---|
| E1-passivo-53 | Dez vínculos livres para propositor com `!` caso a caso; 43 congelados pela L-092 (`app/core/espelho_verdade.py:57`). O50: `PARADA (passivo espera !)` (`app/docs/GRADE-MESTRA.md:862`); BACKLOG exige `!` para o passivo (`app/docs/BACKLOG.md:1388`). |
| E5-le-da-celula | Falta status encerrado (`app/core/espelho_verdade.py:151`). O `!` dele é exigido para escrever o novo estado nas linhas da 10 após export; migration e censo não pedem aval (`app/docs/BACKLOG.md:2578`). |
| R4 | A linha PLACAR-ESTRUTURAL registra espera do `!` para os próximos casos da 09 exportada e paga (`app/docs/BACKLOG.md:1230`); portas de demissão/encerrar vínculo esperam `!` L-009 (`app/core/placar_estrutural.py:389`). Há corte para Pauta DP, sem apply, mantendo 09 intocada (`app/docs/BACKLOG.md:334`). |

**Cuidados para consolidar:**

- **E3-separados-30:** estado literal `ESPERA_RONALD`, mas a prova atribui a 09 ao **Gabriel**, não nomeia decisão pendente de Ronald (`app/core/espelho_verdade.py:95`; `app/docs/BACKLOG.md:1579`). A obra CORTE-B-30 está congelada (`app/docs/BACKLOG.md:1224`; `app/docs/GRADE-MESTRA.md:845`). Pode aparecer na lista como estado literal de espera, explicitando a divergência de responsável.
- **R3:** não classificar como espera atual de Ronald apenas pela redação antiga “placar não carimba a própria meta” (`app/core/placar_estrutural.py:185`). O corte já respondeu: grade 25/48 versus demais leitores 48/48 é divergência de leitor, **dono ESTRUTURA**, e R3 não fecha antes dessa concordância (`app/docs/BACKLOG.md:338`).
- **R6:** “dia em aberto bloqueia a folha?” já tinha lei a executar (`app/docs/BACKLOG.md:435`) e os dois pendentes de batida foram encerrados pela L-118 (`app/docs/BACKLOG.md:1546`). “SEM juiz” na tabela de lookup não basta para contar lei faltante (`app/docs/JUIZES.md:105`; `app/docs/JUIZES.md:109`).
- **BATERIA:** a pendência descrita é seis famílias sem bateria declarada, não lei faltante (`app/core/placar_estrutural.py:99`).
- **E6-exportavel:** donos CADASTRO/BATIDA seguem lista do admin e ESTRUTURA fila 1; entrada efetiva na lista não foi medida (`app/core/espelho_verdade.py:185`). Isso não documenta espera de Ronald nem lei faltante.

## 15/11 23:49 · sessao c9d31d83fb60 · - · - · origem=Ronald (tmux)

Constantes do placar: `FECHADO`, `PARCIAL`, `PENDENTE` (`app/core/placar_estrutural.py:45`). Há **4 resultados selecionados, todos PARCIAL**: BATERIA, R3, R4, R6 (`app/core/placar_estrutural.py:74`, `:146`, `:211`, `:433`). `PENDENTE` também pode ser atribuído por prova ausente (`app/core/placar_estrutural.py:649–651`); os quatro selecionados têm `prova` declarada (`:101`, `:197`, `:266`, `:591`).

| fonte | id | estado | obra existente e estado | destrava e dono |
|---|---|---|---|---|
| `app/core/placar_estrutural.py:74` | BATERIA | PARCIAL | **Sem obra específica comprovada para fechar a meta inteira.** A meta exige todas as famílias; faltam 6 de 7 (`app/core/placar_estrutural.py:79`, `:99–100`). `BATERIA-QUEIXAS-ADMIN` existe, mas abrange Q1–Q4, não declara fechar todas as famílias (`app/docs/BACKLOG.md:1867`; `app/docs/GRADE-MESTRA.md:103`, estado `REGISTRADA`). | Todas as famílias com bateria declarada e verde, sem allowlist, e nenhum dia ESTRUTURA novo sem destino (`app/core/placar_estrutural.py:79–80`). Dono da execução integral: **nao nomeado**; a origem dos casos ESTRUTURA é explícita (`:77–80`). A validação **Ronald+DP** só está registrada para `BATERIA-QUEIXAS-ADMIN` (`app/docs/BACKLOG.md:1867`). |
| `app/core/placar_estrutural.py:146` | R3 | PARCIAL | `PLACAR-ESTRUTURAL` — `PARADA (resultados parciais; espera autorização)` (`app/docs/GRADE-MESTRA.md:62`). `O130` existe e está `NO AR 04/10 10:02`, mas não fecha o restante (`app/docs/BACKLOG.md:1280`, `:338–342`). | Grade alcançar 48/48, concordando com cartão/tela/API; o complemento manda manter R3 aberto e classifica como divergência do leitor, **dono ESTRUTURA** (`app/docs/BACKLOG.md:338–342`). |
| `app/core/placar_estrutural.py:211` | R4 | PARCIAL | `PLACAR-ESTRUTURAL` — `PARADA (resultados parciais; espera autorização)` (`app/docs/GRADE-MESTRA.md:62`); a linha própria ainda diz `espera o !` (`app/docs/BACKLOG.md:1230`). | Meta ZERO em cada par (`app/core/placar_estrutural.py:215`); o corte posterior manda **Pauta DP com os dois números e não tocar a 09**, recusando REGEN para esses quatro colaboradores (`app/docs/BACKLOG.md:334–337`). Dono **ESTRUTURA** para os seis casos novos (`app/core/placar_estrutural.py:291–292`); decisões sobre a 09 são de **Ronald**, pelo corte referido como dele (`app/docs/BACKLOG.md:324–325`, `:334–337`). |
| `app/core/placar_estrutural.py:433` | R6 | PARCIAL | `PLACAR-ESTRUTURAL` — `PARADA (resultados parciais; espera autorização)`, explicitando lote do R6 (`app/docs/GRADE-MESTRA.md:62`). `O35` cobre contratos, estado `REGISTRADA (partes construídas; contratos incompletos)` (`app/docs/GRADE-MESTRA.md:896`). | Chegar a **20/20 e zero passo manual**, restando lote no registro do resultado (`app/core/placar_estrutural.py:434–440`); o critério dos contratos é exceções ZERO, selo da porta e `verde=True` no mesmo commit (`app/docs/BACKLOG.md:1444`). Dono global: **nao nomeado** nessas fontes; não converter `ESPINHA-E` em pessoa responsável sem fonte (`app/docs/GRADE-MESTRA.md:62`, `:896`). |

**Divergências que afetam a consolidação:**

- R3 não deve aparecer esperando apenas o carimbo de Ronald por causa de `app/core/placar_estrutural.py:185–188`: o complemento de 11/10 diz expressamente **“R3 — NÃO FECHA”**, pela divergência da grade, dono ESTRUTURA (`app/docs/BACKLOG.md:324`, `:338–342`).
- R4 conserva espera `!` em prosa antiga (`app/core/placar_estrutural.py:313–315`; `app/docs/BACKLOG.md:1230`), mas há corte posterior **R4-PAR6 = Pauta DP e 09 intocada** (`app/docs/BACKLOG.md:334–337`). Relatar ambos; não transformar o corte em autorização de apply.
- `O130` é id existente, não ausente dos docs (`app/docs/BACKLOG.md:1280`).
- `O35` existe e cobre contratos, mas não demonstra sozinho fechar a outra metade do R6, o lote de reatividade (`app/docs/GRADE-MESTRA.md:896`; `app/core/placar_estrutural.py:438–440`).
- `JUIZES.md` dizer **SEM juiz** não é registro de lei faltante; o arquivo declara ser lookup (`app/docs/JUIZES.md:7–12`, `:105`, `:109`).
- A obra `BATERIA-QUEIXAS-ADMIN` tem espera de validação Ronald+DP, porém não há vínculo explícito afirmando que ela fecha integralmente BATERIA (`app/docs/BACKLOG.md:1867`; `app/core/placar_estrutural.py:79–100`).
- Para perguntas de lei, o R6 cita historicamente duas em prova antiga (`app/core/placar_estrutural.py:591–593`), mas não basta para contá-las como **ainda faltantes**: a própria nota narra cortes posteriores (`:541–548`).

## 15/11 23:49 · sessao f29fd6469b4f · E4C-F1NOVA-B1 · verificador · origem=Code

**O delta fecha os três contraexemplos antigos de cache, mas não garante o par sob concorrência; o contrato também conserva uma falsa absolvição e introduz um falso positivo.** (`app/chamados/models.py:3572`; `app/chamados/models.py:3552`; `app/chamados/tests/test_contract_par_resposta.py:75`; `app/chamados/tests/test_contract_par_resposta.py:93`)

Identidade independente de `HEAD`, comparação Git com `1fcb5d0b6` e execução com Django/banco: **[não verificado]**. A análise abaixo confronta a cópia com o delta fornecido.

1. **PROCEDE o fechamento de 2a, 2b, 2c e 3; o item 5 fecha somente o exemplo anterior.**

   | Item da volta 2 | Execução mental no código atual |
   |---|---|
   | **2a — outra instância zerou o par** | O último `p.save(update_fields=['respondida_em'])` relê `(None, '')`, monta `(I, '')` e recusa. Há teste explícito desse caso. (`app/chamados/models.py:3572`; `app/chamados/models.py:3574`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:146`) |
   | **2b — rollback e repetição do save parcial** | Depois do rollback, relê `(None, '')`; repetir somente o instante monta `(I, '')` e recusa. A comparação não usa estado memorizado após o save anterior. (`app/chamados/models.py:3572`; `app/chamados/models.py:3576`; `app/chamados/services/fio.py:1606`) |
   | **2c — legado intocado após rollback** | Relê `(None, 'legado')`; memória e banco coincidem, portanto retorna sem recusar. O teste novo reproduz essa sequência. (`app/chamados/models.py:3576`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:159`) |
   | **3 — fotografia tratada como banco atual** | A guarda usa leitura local do banco, sem consultar os atributos antigos. (`app/chamados/models.py:3568`; `app/chamados/models.py:3572`) |
   | **5 — saída antecipada aninhada que não governa a escrita** | O exemplo antigo agora é acusado: a saída antecipada precisa estar diretamente em `fn.body`. Isso fecha aquele exemplo, mas rejeita também uma forma legítima, demonstrada no item 5 abaixo. (`app/chamados/tests/test_contract_par_resposta.py:93`; `app/chamados/tests/test_contract_par_resposta.py:162`) |

2. **PROCEDE existir contraexemplo concorrente; NÃO PROCEDE considerar a leitura dentro do `atomic` suficiente.**

   Intercalação entre dois saves, partindo de banco e instância `p` com `(I, 'bati')`: a leitura é simples, a igualdade dispensa a recusa e a escrita acontece depois. (`app/chamados/models.py:3570`; `app/chamados/models.py:3572`; `app/chamados/models.py:3576`; `app/chamados/models.py:3552`)

   ```python
   # A inicia:
   p.save(update_fields=['respondida_em'])
   # A lê (I, 'bati') e passa pela igualdade; pausa antes do UPDATE.

   # B executa e confirma:
   q.respondida_em, q.resposta_colab = None, ''
   q.save(update_fields=['respondida_em', 'resposta_colab'])

   # A continua: escreve apenas respondida_em=I.
   # Resultado: (I, ''), ilegal, sem recusa de A.
   ```

   B pode obedecer à `trava_na_ordem`; isso não serializa A, cujo `save` não adquiriu essa trava antes da leitura. Se ambos os atos já seguram a trava regulamentar antes de entrar no save, essa intercalação fica excluída. Ocorrência em produção, isolamento efetivo e reprodução SQL: **[não verificado]**. (`app/chamados/models.py:2727`; `app/chamados/models.py:2734`; `app/chamados/models.py:3548`)

   Nos demais caminhos pedidos, com valores concretos e `update_fields` por keyword:

   | Caminho | Resultado da guarda |
   |---|---|
   | `create` | Com `adding=True`, valida ambas as metades da memória. (`app/chamados/models.py:3569`; `app/chamados/models.py:3574`) |
   | `only/defer`, refresh integral ou parcial | Não há cache próprio a invalidar; a guarda relê o banco. Sem `update_fields`, acessa ambas as metades antes do `super`, podendo carregar campos diferidos. (`app/chamados/models.py:3572`; `app/chamados/models.py:3574`; `app/chamados/models.py:3552`) |
   | Save parcial tocando o par | A metade excluída vem do banco. Save excluindo ambas retorna imediatamente. (`app/chamados/models.py:3566`; `app/chamados/models.py:3574`) |
   | PK explícito, `adding=False` | Consulta a linha pelo PK; não depende de ter passado por `from_db`. (`app/chamados/models.py:3569`) |
   | Linha apagada antes da guarda | `_banco=None`: valida ambas as metades, sem absolvição por igualdade com a linha antiga. O comportamento subsequente de UPDATE/INSERT do Django: **[não verificado]**. (`app/chamados/models.py:3573`; `app/chamados/models.py:3574`; `app/chamados/models.py:3576`) |
   | Rollback externo | O save seguinte relê o banco revertido; não herda cache do save desfeito. (`app/chamados/models.py:3572`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:159`) |
   | `bulk_update`/`QuerySet.update` fora do save | Não demonstram falha **deste save**. A própria fixture introduz legado por `update` direto. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:62`) |

   Não encontrei nessas sequências, sem concorrência, recusa nova de legado cujo par efetivamente permanece igual: a igualdade explícita o libera. A implementação interna do Django para saves diferidos permanece **[não verificado]**. (`app/chamados/models.py:3576`)

   Há ainda uma ressalva já presente na forma anterior, portanto sem atribuição ao delta: a guarda lê `update_fields` somente de `kwargs`, enquanto encaminha `*args`. Se o Django aceitar `p.save(False, False, None, ['respondida_em'])`, banco `(None, '')` e memória `(I, 'bati')` passam pela guarda completa, mas gravam somente o instante. Aceitação posicional nessa instalação: **[não verificado]**. (`app/chamados/models.py:3549`; `app/chamados/models.py:3552`; `app/chamados/models.py:3574`)

3. **NÃO PROCEDE haver leitor executável localizado de `_par_carregado` ou `_par_lido`.**

   O grep encontrou somente menção histórica em `app/docs/RELATO.md:9163`. `from_db` aparece no catálogo genérico de métodos, sem dependência específica de PerguntaDisputa; a fixture continua usando o refresh normal. Não localizei dependência dos overrides removidos em código ou testes; dependência dinâmica: **[não verificado]**. (`app/core/management/commands/describe.py:68`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:66`; `app/chamados/models.py:3561`)

4. **PROCEDE o respeito à lei de trava e à catraca A; NÃO PROCEDE inferir disso segurança concorrente.**

   A consulta nova não adquire `select_for_update`; respeita a exclusividade de `trava_na_ordem`. Executei o varredor AST isoladamente: `varrer_travas=[]`. A catraca exige essa lista vazia, mas não prova que todo chamador do save já adquiriu a trava. (`app/chamados/models.py:2729`; `app/chamados/models.py:3572`; `app/chamados/tests/test_e1_v7_catracas.py:158`; `app/chamados/tests/test_e1_v7_catracas.py:390`)

   **NÃO PROCEDE haver teste localizado com `assertNumQueries` envolvendo save de PerguntaDisputa tocando o par.** Os dois candidatos de resposta automática medem o segundo chamado, que retorna antes do save. Contagem executada com banco: **[não verificado]**. (`app/chamados/tests/test_porta_chamado_e3.py:47`; `app/chamados/tests/test_e3_10_cura_no_escritor.py:84`; `app/chamados/services/disputa_emissao.py:1663`; `app/chamados/services/disputa_emissao.py:1677`)

   Custo explícito da guarda: uma leitura para instância existente quando o par é tocado ou `update_fields=None`; zero quando o par é excluído ou `adding=True`. Carregamentos diferidos podem somar consultas. (`app/chamados/models.py:3566`; `app/chamados/models.py:3569`; `app/chamados/models.py:3574`)

5. **PROCEDE falsa absolvição e PROCEDE acusação de escritor legítimo.**

   **Falsa absolvição persistente:** executei o helper real em memória; este exemplo retorna `None`. A guarda pode ler `p`, enquanto o update escreve outra pergunta `q`: com texto em `p` e par vazio em `q`, carimba `q` sem texto. `_le` verifica somente o nome do atributo, sem vincular a linha lida à escrita. (`app/chamados/tests/test_contract_par_resposta.py:75`; `app/chamados/tests/test_contract_par_resposta.py:94`; `app/chamados/tests/test_contract_par_resposta.py:111`)

   ```python
   def f(p, q, agora):
       if not (p.resposta_colab or '').strip():
           return
       PD.objects.filter(pk=q.pk).update(respondida_em=agora)
   ```

   **Falso positivo introduzido pela restrição a `fn.body`:** este exemplo retorna `'respondida_em'`, embora toda execução do save passe pela guarda de texto da própria instância. O `if` interno governa a escrita, mas não está diretamente no corpo da função. (`app/chamados/tests/test_contract_par_resposta.py:93`; `app/chamados/tests/test_contract_par_resposta.py:97`; `app/chamados/tests/test_contract_par_resposta.py:113`)

   ```python
   def f(p, habilitado, agora):
       if habilitado:
           if not (p.resposta_colab or '').strip():
               return
           p.respondida_em = agora
           p.save(update_fields=['respondida_em'])
   ```

   A varredura atual retornou `_ofensores() == []`: o falso positivo demonstra uma forma legítima rejeitada, sem escritor atual localizado nessa situação. Os dois escritores examinados anteriormente continuam corretamente absolvidos pela forma. (`app/chamados/tests/test_contract_par_resposta.py:116`; `app/chamados/models.py:3231`; `app/chamados/models.py:3253`; `app/chamados/services/disputa_emissao.py:1692`; `app/chamados/services/disputa_emissao.py:1698`)

## 15/11 23:49 · sessao 2d5da68b7f6c · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

Saída integral fornecida: [nao verificado]

```text
sonda_leitura: script.py [fe38ad6dc95a1ace] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "tmpfs" to rootfs at "/app/logs": create mountpoint for /app/logs mount: make mountpoint "/app/logs": mkdirat /var/lib/docker/rootfs/overlayfs/d3b43d2121a485aa3f2f6c0439523bd7423882de7644711b73b9080567ea8b89/app/logs: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

O dado fornecido contém a falha de execução acima, sem a saída da parte 4 de 6. [nao verificado]

- `SOMA=True`: [nao verificado].
- `ESTRUTURA_DIAS_LINHAS=True`: [nao verificado].
- Há `INCOMPLETO` na parte 4: [nao verificado].

## 15/11 23:49 · sessao 2f8340c4a6ad · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

[nao verificado] Saída integral fornecida; o anexo não contém a parte 3/6 solicitada:

```text
sonda_leitura: script.py [82c88c2988af3846] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "tmpfs" to rootfs at "/app/logs": create mountpoint for /app/logs mount: make mountpoint "/app/logs": mkdirat /var/lib/docker/rootfs/overlayfs/c1fc8c1b796422444d13121295ed0346a8470b12b8787743a7783a2d97d043fa/app/logs: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

- SOMA é True: [nao verificado]; linha ausente no dado fornecido.
- ESTRUTURA_DIAS_LINHAS é True: [nao verificado]; linha ausente no dado fornecido.
- Há INCOMPLETO na parte: [nao verificado]; o marcador não aparece no dado fornecido, mas a parte solicitada não foi apresentada.

## 15/11 23:48 · sessao 1d41892ae2fa · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

Transcrição integral do dado fornecido; identificação como “parte 2 de 6 (R1 remedido)” [nao verificado]:

```text
sonda_leitura: script.py [23c7f47877c9a3fd] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "tmpfs" to rootfs at "/app/logs": create mountpoint for /app/logs mount: make mountpoint "/app/logs": mkdirat /var/lib/docker/rootfs/overlayfs/520bfee192a3646b1980c3d4742cc404e62946397531ee22586a173a28f97e78/app/logs: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

- **SOMA é True:** [nao verificado]; a linha não consta do dado fornecido.
- **ESTRUTURA_DIAS_LINHAS é True:** [nao verificado]; a linha não consta do dado fornecido.
- **Há INCOMPLETO na parte:** [nao verificado]; o marcador não aparece no trecho fornecido.

## 15/11 23:48 · sessao 9bc987ad1fda · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

A parte 1 de 6 não consta do dado fornecido; segue todo o trecho recebido, sem resumir. [nao verificado]

```text
sonda_leitura: script.py [4e9a5641d912b65d] no cpuset 4-7, lendo sombra como leitor (LEITURA)
sonda_leitura: arvore=/home/ronald/v0-arvore-3580b35e0 (copia), banco segue sombra como leitor
docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error mounting "tmpfs" to rootfs at "/app/logs": create mountpoint for /app/logs mount: make mountpoint "/app/logs": mkdirat /var/lib/docker/rootfs/overlayfs/81c87712d91a033ef84050839a7038dfca63323d1bce2600137f8590821b7e6e/app/logs: read-only file system

Run 'docker run --help' for more information
rc_sombra=125
```

- **SOMA=True:** não confirmável; a linha está ausente. [nao verificado]
- **ESTRUTURA_DIAS_LINHAS=True:** não confirmável; a linha está ausente. [nao verificado]
- **INCOMPLETO:** o marcador literal não aparece no trecho; há erro do Docker e `rc_sombra=125`. O estado da parte 1 de 6 é [nao verificado].

## 15/11 23:48 · sessao 228d59997109 · - · - · origem=Ronald (tmux)

**Item 2: PROCEDE contraexemplo concorrente; não achei contraexemplo sequencial com `update_fields` por keyword e valores concretos.** A guarda lê o banco sem trava, compara e só depois chama `super().save`; há uma janela entre validação e escrita. (`app/chamados/models.py:3548`; `app/chamados/models.py:3552`; `app/chamados/models.py:3570`; `app/chamados/models.py:3576`)

Contraexemplo:

1. Banco e instância A: `(I, 'bati')`.
2. A chama `save(update_fields=['respondida_em'])`; sua leitura retorna `(I, 'bati')` e a igualdade dispensa a recusa.
3. Antes do UPDATE de A, B grava `(None, '')` e confirma.
4. A grava somente `respondida_em=I`; resultado `(I, '')`, ilegal segundo o juiz. B pode adquirir a trava regulamentar; A nunca a adquiriu. (`app/chamados/models.py:3572`; `app/chamados/models.py:3574`; `app/chamados/models.py:3576`; `app/chamados/models.py:3552`; `app/chamados/models.py:2727`; `app/chamados/juizes.py:1534`)

Ocorrência em produção, isolamento efetivo e execução SQL: **[não verificado]**. A configuração `prod` declara PostgreSQL. (`app/config/settings/prod.py:9`)

**Outro problema: `update_fields` posicional.** A guarda consulta apenas `kwargs`, mas encaminha `*args` intactos ao Django. (`app/chamados/models.py:3549`; `app/chamados/models.py:3552`)

```python
# banco (None, ''), memória (I, 'bati')
p.save(False, False, None, ['respondida_em'])
```

A guarda interpreta save completo, aprova memória legal e o Django escreveria só instante, produzindo `(I, '')`. Também pode recusar legado intocado: banco `(None,'legado')`, memória `(None,'outro')`, `p.save(False, False, None, ['pergunta'])`; a guarda recusa texto sem instante apesar de o save excluir ambas as metades. A aceitação desses argumentos pelo Django instalado é **[não verificado]**: a cópia fixa Django 5.2.14, mas o pacote não está instalado neste ambiente. (`app/chamados/models.py:3549`; `app/chamados/models.py:3574`; `app/chamados/services/fio.py:1610`; `app/requirements.txt:13`)

Os casos sequenciais pedidos:

- **Create:** valida ambas as metades da memória; não consulta linha existente com `_state.adding=True`. (`app/chamados/models.py:3569`; `app/chamados/models.py:3574`)
- **`only/defer`, sem `update_fields`:** a guarda acessa ambas as metades antes do `super`; campos diferidos são carregados. O comportamento exato do `update_fields` automático do Django permanece **[não verificado]**. (`app/chamados/models.py:3574`; `app/chamados/models.py:3552`)
- **Refresh integral/parcial:** a guarda não usa fotografia própria; consulta novamente antes de compor o par. (`app/chamados/models.py:3572`)
- **PK explícito com `adding=False`:** linha existente segue comparação normal; linha ausente deixa `_banco=None` e valida ambas as metades da memória. (`app/chamados/models.py:3569`; `app/chamados/models.py:3574`)
- **Linha apagada antes da guarda:** nenhuma absolvição por igualdade ocorre; nascimento ilegal é recusado. O eventual fallback de UPDATE para INSERT do Django é **[não verificado]**. (`app/chamados/models.py:3573`; `app/chamados/models.py:3576`; `app/chamados/models.py:3579`)
- **Rollback externo:** próximo save relê o estado revertido; fecha os três exemplos antigos de cache. Os testes novos cobrem 2a e 2c expressamente. (`app/chamados/models.py:3572`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:146`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:159`)
- **`QuerySet.update` fora do save:** a fixture cria legado assim; isso não demonstra que ESTE save o gravou. Pode fornecer a escrita B da corrida. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:62`)
- **Gerador em `update_fields`:** `set(update_fields)` consome o iterador, que é encaminhado esgotado ao `super`; resultado exato — no-op ou erro no Django — **[não verificado]**. (`app/chamados/models.py:3565`; `app/chamados/models.py:3552`)

## 15/11 23:46 · sessao 6c8eada32dad · - · - · origem=Ronald (tmux)

**3 — PROCEDE o fechamento da dependência do cache.** `rg -n --hidden '_par_carregado|_par_lido' .` encontrou somente relato histórico em `app/docs/RELATO.md:9163`; nenhum leitor/escritor executável ou teste. A implementação atual usa apenas leitura do banco e variáveis locais (`app/chamados/models.py:3561`, `app/chamados/models.py:3572`, `app/chamados/models.py:3574`). Grep de `\bfrom_db\b` em Python encontrou apenas o catálogo genérico em `app/core/management/commands/describe.py:68`; nenhum override de `from_db`/`refresh_from_db` em models.py. Chamadas normais continuam, por exemplo a fixture legada em `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:66`; isso exige a operação herdada, sem dependência demonstrada do override removido. Dependências dinâmicas não detectáveis por esse grep: **[não verificado]**.

**4 — PROCEDE o respeito à lei e à catraca A.** A lei reserva `select_for_update` à `trava_na_ordem` e permite releitura simples após obter as travas (`app/chamados/models.py:2727`, `app/chamados/models.py:2734`); a nova consulta é simples (`app/chamados/models.py:3572`). A catraca procura atributos `select_for_update`, referências via `getattr` e SQL literal de trava (`app/chamados/tests/test_e1_v7_catracas.py:116`, `app/chamados/tests/test_e1_v7_catracas.py:133`, `app/chamados/tests/test_e1_v7_catracas.py:137`).

Executei isoladamente, somente em memória, as funções AST reais: **766 fontes de produção, `varrer_travas=[]`**. O percurso e a agregação estão em `app/chamados/tests/test_e1_v7_catracas.py:57` e `app/chamados/tests/test_e1_v7_catracas.py:158`; o teste exige essa lista vazia (`app/chamados/tests/test_e1_v7_catracas.py:387`). Não executei Django/banco: **[não verificado]**.

**NÃO PROCEDE haver teste localizado com `assertNumQueries` envolvendo um save de PerguntaDisputa que toque o par.** Examinei por AST todos os blocos no diretório de testes de chamados. Os dois candidatos de resposta automática medem o **segundo chamado/no-op** (`app/chamados/tests/test_porta_chamado_e3.py:46`, `app/chamados/tests/test_porta_chamado_e3.py:47`; `app/chamados/tests/test_e3_10_cura_no_escritor.py:83`, `app/chamados/tests/test_e3_10_cura_no_escritor.py:84`): retornam antes do save pela existência de texto (`app/chamados/services/disputa_emissao.py:1663`), cujo caminho de escrita está em `app/chamados/services/disputa_emissao.py:1677`. Os testes de nove consultas chamam o leitor `consulta_do_app` (`app/chamados/tests/test_e3_v4_orm.py:91`, `app/chamados/tests/test_e3_v4_orm.py:98`; composição do leitor em `app/chamados/services/pergunta_no_app.py:73`). Contagem real e cobertura contra chamadas dinâmicas: **[não verificado]**.

**Custo estático:** `_recusa_par_ilegal` faz uma consulta simples para instância existente quando `update_fields` toca o par ou é `None`; faz zero se a lista exclui o par ou `_state.adding=True` (`app/chamados/models.py:3565`, `app/chamados/models.py:3569`, `app/chamados/models.py:3572`). Acessar atributos diferidos pode acrescentar consultas, além da leitura explícita (`app/chamados/models.py:3574`).

## 15/11 23:46 · sessao 98c609aa8e2a · - · - · origem=Ronald (tmux)

**Item 5 — PROCEDE falsa absolvição e PROCEDE falso positivo.** O caso antigo do `return` aninhado foi fechado, mas a restrição a `fn.body` recusa também uma guarda aninhada que efetivamente governa a escrita. (`app/chamados/tests/test_contract_par_resposta.py:87-97`)

Executei os helpers extraídos por AST, somente em memória, com `python3 -B`: [nao verificado]

```python
# ACUSA: meia_par -> 'respondida_em'
def f(p, qs, habilitado):
    if habilitado:
        if not p.resposta_colab:
            return
        qs.update(respondida_em=agora)
```

Tomando `qs` como a linha de `p` e texto válido quando não vazio, esse escritor é legítimo: toda execução do update passa pela guarda. `_governada` não reconhece o `if` interno como saída antecipada porque examina apenas instruções diretamente em `fn.body`; o `if habilitado` externo não lê a outra metade. (`app/chamados/tests/test_contract_par_resposta.py:88`; `app/chamados/tests/test_contract_par_resposta.py:93-96`)

```python
# ABSOLVE: meia_par -> None
def f(p, qs):
    if not p.resposta_colab:
        return
    qs.update(respondida_em=agora)
```

Se `p` tem texto e `qs` seleciona outra pergunta vazia, grava instante sem texto. `_le` aceita qualquer atributo com nome `resposta_colab`, sem vincular objeto/linha ao escritor; `_governada` aceita essa saída antecipada. Isso independe da polaridade, expressamente excluída do selo. (`app/chamados/tests/test_contract_par_resposta.py:75-76`; `app/chamados/tests/test_contract_par_resposta.py:93-96`; `app/chamados/tests/test_contract_par_resposta.py:21-24`)

O contraexemplo antigo (`update` depois do `if habilitado`, fora dele) agora retorna `'respondida_em'`, conforme o caso acrescentado no teste. (`app/chamados/tests/test_contract_par_resposta.py:162`)

A varredura `_ofensores()` executada sobre a cópia retornou `[]`: nenhum escritor atual é acusado pelo contrato; o falso positivo acima é uma regressão de capacidade, não um ofensor atual encontrado. (`app/chamados/tests/test_contract_par_resposta.py:116-136`)

## 15/11 23:31 · sessao 0a3fd6325a9e · EM-DISPUTA-TRES-DERIVACOES · leitura · origem=Code

**(a) Censo.** O aval confere: há **três derivações variáveis de `em_disputa`**, com perguntas e fontes diferentes; também há **cinco escritores de `False` constante** nos retornos de duplicata, colisão e reuso. (`app/api/views_core.py:753-765`; `app/api/views.py:759-771`; `app/api/views.py:2569`; `app/api/views.py:1336`; `app/ponto/services/resposta_batida.py:29`.)

| Derivação | Escritor e fonte | Pergunta efetivamente respondida | Leitor/publicação |
|---|---|---|---|
| Anomalia, core | Lista local `motivos_anomalia`; se não vazia, chama emissão da disputa e atribui `True`. | “Este processamento da batida terminou com algum motivo de anomalia?” Não consulta existência de pergunta. | Atribuição: `app/api/views_core.py:753-765`; JSON: `app/api/views_core.py:815-816`. |
| Anomalia, UI | Mesmo procedimento no gêmeo. | Mesma pergunta. | Atribuição: `app/api/views.py:759-771`; JSON: `app/api/views.py:821-822`. |
| Pendência no app | `bool(consulta_do_app(colab)['pendentes'])`. | “A consulta atual oferece alguma pergunta pendente?” | Consulta: `app/api/views.py:2537-2539`; flag/lista: `app/api/views.py:2569-2578`. |
| Disputa aberta | `colaborador_em_disputa`: primeira disputa do colaborador com `fechada_em IS NULL`. | “O colaborador tem disputa aberta?” Não consulta perguntas. | Autoridade: `app/chamados/services/disputa_emissao.py:34-42`; chamada: `app/api/views.py:1336`; JSON: `app/api/views.py:1670`. |

A lista de anomalias **não vem apenas de `detectar_anomalias`**: recebe contribuições de selfie, mock, assinatura/liveness, entrada/saída incompleta e desvio grave; pode perder o motivo de saída sem turno. A fonte do flag é a lista **final**, depois desses passos. (`app/api/views.py:433-495`; `app/api/views.py:604-645`; `app/api/views.py:703-707`; `app/api/views.py:748-771`.)

| Escritores constantes/propagadores | Significado |
|---|---|
| `app/api/views_core.py:529`; `app/api/views.py:543` | Duplicata: `em_disputa=False`, motivos vazios. |
| `app/api/views_core.py:681`; `app/api/views.py:690` | Colisão oposta ignorada: `False`, motivos vazios. |
| `app/ponto/services/resposta_batida.py:7-30` | Reuso sem marca transitória `criada_neste_toque`: `False`, motivos vazios. |
| `app/api/views_core.py:684-686`; `app/api/views.py:693-695`; `app/ponto/services/triagem_batida.py:723-725` | Propagam o retorno do serviço de reuso. |

O comentário da selfie permanece exatamente em `app/api/views_core.py:426-427`; a regra executável está em `app/api/views_core.py:433-439`: consulta `selfie_ausente`, com canal derivado do `device_id` e tempo decorrido zero. Portanto, aquela referência trata a contribuição **selfie** para os motivos; não estabelece equivalência entre anomalia e pergunta disponível. (`app/ponto/registro_batida.py:53-61`; `app/ponto/registro_batida.py:80-91`.)

O roteamento das cascas é este:

| Casca | Caminho verificado na cópia |
|---|---|
| Caddy | Encaminha `/api/ponto/*`, `/api/auth/*`, `/api/me/`, `/api/regularizacao/*`, `/api/ping-geo/`, `/api/registrar-fcm/` e `/health/` ao core; restante à UI. Juliani importa esse bloco. (`Caddyfile:8-10`; `Caddyfile:32-38`.) |
| Core | `config/urls_core.py` inclui `api.urls_core`, que importa `views_core`; `/me/` e `/ponto/bater/` usam esse módulo. (`app/config/urls_core.py:5-6`; `app/api/urls_core.py:5`; `app/api/urls_core.py:13-14`.) |
| UI | Inclui `api.urls`; oferece espelho/v2, chamados/v2, perguntas e resposta. O gêmeo `/ponto/bater/` existe nesse URLconf, mas a regra do Caddy encaminha essa URL ao core. (`app/config/urls.py:37`; `app/api/urls.py:67-93`; `Caddyfile:8-10`.) |

Essa divisão coincide com a seção 2 do primer; o ambiente efetivamente carregado em produção permanece [nao verificado]. (`CLAUDE.md:84-90`; seleção por variável em `app/config/settings/saas.py:76-78`.)

Os equivalentes de apresentação e seus leitores são:

| Chave/valor | Quem calcula | Quem lê/propaga |
|---|---|---|
| `pendentes`, `historico` | `consulta_do_app` aplica C6 e separa as vivas; pendente exige ainda resposta ausente e disputa aberta. (`app/chamados/services/pergunta_no_app.py:45-85`.) | GET perguntas: `em_disputa`, `perguntas`, `respondidas`, `historico`; `disputa_id` e `chamado_id` ancoram as pendentes. (`app/api/views.py:2566-2578`.) |
| `questionarios_pendentes` | Comprimento de `pendentes`. (`app/chamados/services/pergunta_no_app.py:100-101`.) | `/me/` core e UI; dashboard; painel administrativo do colaborador. (`app/api/views_core.py:303-307`; `app/api/views_core.py:346-348`; `app/api/views.py:315-319`; `app/api/views.py:358-360`; `app/core/views.py:252-268`; `app/colaboradores/views.py:940-941`.) |
| `chamados_badge` | Prioriza pendentes com “Responder DP”; depois chamados com nota não lida; senão `None`. (`app/chamados/services/pergunta_no_app.py:102-118`.) | Propagado pelos dois `/me/`. (`app/api/views_core.py:348`; `app/api/views.py:360`.) |
| Link, badge e rótulo web | Leem `questionarios_pendentes`. | Decidem `?aba=cham`, badge e “Responder DP”. (`app/templates/core/dashboard_colaborador_v2.html:78-80`.) |
| Apresentação “No app dele agora” | Lê a projeção central. | Total e descrição no painel administrativo. (`app/templates/colaboradores/partials/painel_colaborador.html:39`; `app/templates/colaboradores/partials/painel_colaborador.html:190-191`.) |
| `perguntas_disputa` em chamados/v2 | Reutiliza pendentes + histórico vivo, exclui disputa fechada e ancora no destino vivo ou pai. | Serialização em cada chamado. (`app/api/views.py:996-1040`; `app/api/views.py:1148-1168`.) |
| `pode_solicitar_correcao` | Exige perguntas, nenhuma aguardando colaborador e limite de reaberturas. | Payload de chamados/v2. (`app/api/views.py:1174-1180`.) |
| Aviso de pergunta pendente | `aviso_texto_livre` lê a mesma lista pendente. | Orienta abrir o chamado da pergunta; não interpreta mensagem como resposta. (`app/chamados/services/aviso_pendencia.py:7-20`.) |
| Push de disputa | Emissor envia URL `/meu-atendimento/` e FCM `tipo='disputa'`; recobrança também envia esse tipo. | Tratamento nativo desses sinais [nao verificado]. (`app/chamados/services/disputa_emissao.py:1747-1752`; `app/chamados/services/disputa_emissao.py:1778-1806`; `app/chamados/fcm_utils.py:75-79`.) |

Há leitores adicionais da derivação **disputa aberta**, fora da abertura do questionário:

- O reexport em `chamados.models` alimenta `gate_disputa`, que lê a disputa para recusar POST; GET e operações isentas passam antes. (`app/chamados/models.py:4234-4236`; `app/colaboradores/mixins.py:291-313`.)
- Os decoradores não isentos aparecem em holerite e operações de colaboradores. (`app/holerite/views.py:1144`; `app/colaboradores/views.py:1368`; `app/colaboradores/views.py:1603`; `app/colaboradores/views.py:1769`; `app/colaboradores/views.py:1877`.)
- `motor_bloqueado_por_disputa` delega ao mesmo helper; o espelho o lê e publica `disputa_aberta`, consumida pela faixa do template. (`app/ponto/services/disputa.py:15-31`; `app/ponto/views.py:140-144`; `app/ponto/views.py:207`; `app/templates/ponto/espelho.html:13-18`.)
- O PDF lê o wrapper para bloquear emissão. (`app/relatorios/pdf_espelho.py:1082-1090`; `app/relatorios/pdf_espelho.py:1113`.)
- O painel tem derivadores próprios do mesmo predicado: `exists(disputa aberta)` ou pertencimento ao conjunto recebido; o leitor transforma isso em LED vermelho. O lote monta seu próprio conjunto de disputas abertas. (`app/colaboradores/painel_cell.py:20-32`; `app/colaboradores/painel_cell.py:68`; `app/colaboradores/services/situacional.py:56-59`; `app/colaboradores/services/situacional.py:117`; `app/colaboradores/services/situacional.py:210`.)

Esses homônimos não devem ser confundidos: `em_disputa_fechada` controla ações administrativas; `pode_responder` de chamados/v2 testa chamado não terminal; a regularização externa lista outro modelo. (`app/chamados/services/fio.py:264-268`; `app/templates/chamados/partials/modal_fio.html:289`; `app/api/views.py:1145`; `app/api/views_core.py:1271-1289`.)

O `HANDOFF.md` anuncia contratos do app, mas suas seções tratam login, ausência, plataforma e senha: não documenta o contrato de `em_disputa`/perguntas. A condição executada pelo Android 3.9.3 para abrir a Activity, o campo que ele efetivamente lê e a ocorrência relatada permanecem **[nao verificado]**; o comentário da selfie não substitui esse leitor. (`app/docs/HANDOFF.md:1-219`; `app/api/views_core.py:426-427`.)

**(b) Sonda.** A contagem histórica exata pedida está **[nao verificado]**. `Batida` não conserva o flag/lista daquele retorno; `ultima_batida` é contexto corrente que pode ser substituído. Além disso, o detector inicial não recebe instante e lê cadastro atual. (`app/ponto/models.py:8-95`; `app/api/views_core.py:753-765`; `app/chamados/services/disputa_emissao.py:569-575`; `app/chamados/detector_anomalias.py:19-123`.)

Os juízes aceitam `agora` em parte do caminho, mas não reconstituem todo o estado passado: a projeção preserva identidade atual, a tranca e o veredito são atuais, e a partição lê os carimbos atuais. Passar o instante da batida não produziria uma contagem contemporânea exata. (`app/chamados/juizes.py:1381-1419`; `app/chamados/juizes.py:1450-1466`; `app/chamados/services/pergunta_no_app.py:52-85`.)

O script abaixo, **somente texto e não executado**, deixa a métrica histórica como `None` e entrega duas aproximações separadas: detector inicial atual e desvio grave recalculado. São as funções reais chamadas pelo escritor; não representam seu flag final. Cada uma é cruzada com os dois candidatos de (c), no estado atual. (`app/api/views_core.py:419-420`; `app/api/views_core.py:694-698`; `app/api/views_core.py:753-765`.)

O canal é o gravado; `NULL` permanece desconhecido, inclusive no trecho anterior ao nascimento do campo, sem inferência por timestamp do aparelho. (`app/ponto/models.py:46-55`.)

```python
# SOMENTE TEXTO. Para shell Django ja selecionado no tenant juliani.
# Nao executado nesta leitura.
#
# Fontes:
# app/api/views_core.py:419-420       detector inicial do escritor
# app/api/views_core.py:694-698       componente desvio grave
# app/api/views_core.py:753-765       flag usa motivos FINAIS, nao so estes componentes
# app/chamados/detector_anomalias.py:19-123  detector atual, sem instante passado
# app/ponto/utils.py:103-133         escrita/push interceptados antes da tentativa
# app/chamados/juizes.py:865,1402-1492       C6 real
# app/chamados/services/pergunta_no_app.py:45-85  pendencia atual do app
# app/core/observ.py:60-82           contador de erros engolidos
# app/ponto/models.py:23,45-55,67,79 universo, canal e carimbos
#
# APROXIMACAO:
# unidade = Batida persistida, timestamp nos ultimos 30 dias corridos;
# elegibilidade = origem ATUAL == "app", sem afirmar qual endpoint a recebeu;
# inclui retratadas: nao e universo de apuracao nem contagem de requests/retries;
# sinais = dois componentes reais, recalculados separadamente no snapshot atual;
# portas = C6 atual e pendentes atuais do app, sem escolher entre eles;
# NAO reconstitui em_disputa historico nem pergunta disponivel naquele instante.
#
# Cada componente soma o mesmo universo elegivel, inclusive indecidiveis.
# Resultados por batida: um colaborador com varias batidas pesa varias vezes.
# Nenhum nome, CPF, resposta, contexto ou texto de excecao vai para a saida.

import io
import json
import logging
from collections import Counter, defaultdict
from contextlib import redirect_stdout, redirect_stderr
from datetime import timedelta
from types import SimpleNamespace
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone

universo = None
processadas = 0
elegiveis = 0
janela = None
fatal = None
erros = []
interceptadas = Counter()
canais = defaultdict(Counter)
contagens = defaultdict(Counter)
memo_base = {}
memo_portas = {}
nivel_logging = logging.root.manager.disable


def emitir(obj):
    print(json.dumps(obj, ensure_ascii=False, sort_keys=True))


def canal_gravado(b):
    if b.canal in ("apk", "pwa"):
        return b.canal
    if b.canal is None:
        return "NULL_desconhecido"
    return "fora_das_choices"


def interceptar_criacao(*args, **kwargs):
    # bloquear_se_em_ferias usa somente pk no push e devolve o objeto. [nao verificado]
    # O detector usa o booleano; seu contexto nao e emitido nem medido.
    interceptadas["ChamadoColaborador.criar"] += 1
    return SimpleNamespace(pk=0)


def interceptar_push(*args, **kwargs):
    interceptadas["push_dp"] += 1 [nao verificado]
    return None


class ErroInternoDoDetector(Exception):
    pass


def medir(etapa, func, *args, pessoa=None, batida=None, **kwargs):
    antes = engolidos()["total"]
    valor = None
    falha = None
    try:
        # Savepoint permite continuar se uma tentativa incidental de SQL
        # proibido for recusada pela transacao READ ONLY.
        with transaction.atomic():
            with redirect_stdout(io.StringIO()), redirect_stderr(io.StringIO()):
                valor = func(*args, **kwargs)
    except Exception as exc:
        falha = type(exc).__name__

    delta = engolidos()["total"] - antes
    if falha is not None or delta:
        erro = {
            "etapa": etapa,
            "erro": falha or "erro_engolido",
            "engolidos": delta,
        }
        if pessoa is not None:
            erro["pessoa"] = pessoa
        if batida is not None:
            erro["batida"] = batida
        erros.append(erro)
        return None
    return valor


def ler_base(colab):
    motivos, contexto = detectar_anomalias(colab)
    # Estes erros sao capturados pelo detector sem passar por engolidos().
    # Nunca interpretar resultado parcial como ausencia de anomalia.
    if any(str(k).startswith("_erro") for k in contexto):
        raise ErroInternoDoDetector()
    return bool(motivos)


def ler_grave(b):
    estado, _contexto = detectar_batida_fora_escala(
        b.colaborador, b.timestamp, b.tipo
    )
    if estado not in ("ok", "leve", "grave", "informativa"):
        raise ErroInternoDoDetector()
    return estado == "grave"


def ler_portas(colab, agora):
    # Universo C6 completo do colaborador, sem substituir C6 por q_vivas.
    qs = (
        PerguntaDisputa.objects
        .filter(disputa__colaborador=colab)
        .select_related("disputa", "celula", "chamado_destino")
        .order_by("pk")
    )
    perguntas = list(
        um_colaborador_por_identidade(qs, {colab.pk: colab})
    )
    with tranca_da_consulta(perguntas):
        n_c6 = sum(
            1 for p in perguntas if pergunta_viva(p, agora=agora)
        )

    # Le a particao real; nao reproduz seus filtros na sonda.
    consulta = consulta_do_app(colab, agora=agora)
    return n_c6, len(consulta["pendentes"])


def contabilizar(canal, componente, sinal, portas):
    c = contagens[(canal, componente)]
    c["universo_elegivel"] += 1

    if sinal is None:
        c["sinal_indecidivel"] += 1
        return
    if not sinal:
        c["sinal_ausente"] += 1
        return

    c["sinal_presente"] += 1
    if portas is None:
        c["sinal_presente_portas_indecidiveis"] += 1
        return

    n_c6, n_pendentes = portas
    c["sinal_presente_portas_decididas"] += 1
    if n_c6 == 0:
        c["sinal_presente_sem_C6_viva_no_snapshot"] += 1
    if n_pendentes == 0:
        c["sinal_presente_sem_pendente_no_snapshot"] += 1


try:
    logging.disable(logging.CRITICAL)

    from chamados.models import ChamadoColaborador, PerguntaDisputa
    from chamados.detector_anomalias import (
        detectar_anomalias,
        detectar_batida_fora_escala,
    )
    from chamados.juizes import (
        pergunta_viva,
        tranca_da_consulta,
        um_colaborador_por_identidade,
    )
    from chamados.services.pergunta_no_app import consulta_do_app
    from core.observ import engolidos
    from ponto.models import Batida

    if connection.vendor != "postgresql":
        fatal = "exige_postgresql"
    elif connection.in_atomic_block:
        fatal = "iniciar_fora_de_atomic"
    elif getattr(connection, "schema_name", None) != "juliani":
        fatal = "exige_tenant_juliani"

    if fatal is None:
        with transaction.atomic():
            with connection.cursor() as cursor:
                cursor.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )

            agora = timezone.now()
            inicio = agora - timedelta(days=30)
            janela = {
                "inicio_inclusivo": timezone.localtime(inicio).isoformat(),
                "fim_exclusivo": timezone.localtime(agora).isoformat(),
                "campo_do_recorte": "Batida.timestamp",
                "instante_das_portas": timezone.localtime(agora).isoformat(),
            }

            qs = (
                Batida.objects
                .filter(timestamp__gte=inicio, timestamp__lt=agora)
                .select_related(
                    "colaborador__empresa",
                    "colaborador__posto",
                )
                .order_by("colaborador_id", "pk")
            )
            universo = qs.count()

            # Intercepta os efeitos conhecidos ANTES de chamar o detector.
            # Nao chama views, emissao de disputa nem endpoints HTTP.
            with patch.object(
                ChamadoColaborador,
                "criar",
                side_effect=interceptar_criacao,
            ), patch(
                "chamados.push_utils.push_dp", [nao verificado]
                side_effect=interceptar_push,
            ):
                for b in qs.iterator(chunk_size=500):
                    canal = canal_gravado(b)
                    canais[canal]["universo"] += 1
                    processadas += 1

                    if b.retratada_em is not None:
                        canais[canal]["retratadas_no_snapshot"] += 1

                    if b.origem != "app":
                        canais[canal]["outras_origens_atuais"] += 1
                        continue

                    elegiveis += 1
                    canais[canal]["origem_app_atual"] += 1
                    cid = b.colaborador_id
                    pessoa = f"col{cid:03d}"

                    if cid not in memo_base:
                        memo_base[cid] = medir(
                            "detector_inicial_atual",
                            ler_base,
                            b.colaborador,
                            pessoa=pessoa,
                        )
                    if cid not in memo_portas:
                        memo_portas[cid] = medir(
                            "portas_atuais",
                            ler_portas,
                            b.colaborador,
                            agora,
                            pessoa=pessoa,
                        )

                    grave = medir(
                        "desvio_grave_recalculado",
                        ler_grave,
                        b,
                        pessoa=pessoa,
                        batida=b.pk,
                    )
                    portas = memo_portas[cid]
                    contabilizar(
                        canal, "detector_inicial_atual",
                        memo_base[cid], portas,
                    )
                    contabilizar(
                        canal, "desvio_grave_recalculado",
                        grave, portas,
                    )

        if processadas != universo:
            fatal = "universo_incompleto"

except Exception as exc:
    fatal = fatal or type(exc).__name__
finally:
    logging.disable(nivel_logging)

if fatal is not None:
    erros.append({"etapa": "fatal", "erro": fatal})

componentes = (
    "detector_inicial_atual",
    "desvio_grave_recalculado",
)
campos = (
    "universo_elegivel",
    "sinal_indecidivel",
    "sinal_ausente",
    "sinal_presente",
    "sinal_presente_portas_indecidiveis",
    "sinal_presente_portas_decididas",
    "sinal_presente_sem_C6_viva_no_snapshot",
    "sinal_prese

[truncado em 20000 de 25241 caracteres]

## 15/11 23:26 · sessao ba4226c49212 · - · - · origem=Ronald (tmux)

Censo de derivadores e propagadores, com referências atuais:

| Categoria | Escritor / fonte | Pergunta real / leitor |
|---|---|---|
| Anomalia, UI | `app/api/views.py:759-771` | Lista local `motivos_anomalia` não vazia; chama criador da disputa, ignora retorno e publica em `:821`. |
| Anomalia, core | `app/api/views_core.py:753-765` | Mesmo predicado; publica em `:815`. |
| Perguntas pendentes | `app/api/views.py:2537-2539,2569` | `bool(consulta_do_app(colab)['pendentes'])`; lista publicada em `:2576`. |
| Disputa aberta | `app/chamados/services/disputa_emissao.py:34-42` | `DisputaSupervisao` do colaborador com `fechada_em IS NULL`; retorno `.first()`. `api/views.py:1336` chama e `:1670` publica booleano. |
| Constante de dedup | `app/api/views.py:543`; `app/api/views_core.py:529` | Resposta da duplicata: `False`, motivos vazios em `:544` / `:530`. |
| Constante de colisão oposta | `app/api/views.py:690`; `app/api/views_core.py:681` | Resposta de batida ignorada: `False`, motivos vazios em `:691` / `:682`. |
| Constante de reuso | `app/ponto/services/resposta_batida.py:7-30` | `False` quando não há marca transiente `criada_neste_toque`; propagadores: `api/views.py:693-695`, `api/views_core.py:684-686`, `ponto/services/triagem_batida.py:723-725`. |

Portanto, **três derivações variáveis** do campo JSON, mais **cinco sítios de constante `False`**; as quatro citações de derivação do aval conferem nas linhas atuais acima. A segunda variável significa pendente, mais restrita que simplesmente “pergunta viva”: `consulta_do_app` chama C6 em `app/chamados/services/pergunta_no_app.py:73`; `particao_do_app` exige ainda `respondida_em is None` e disputa aberta em `:76-85`.

Fonte completa de motivos da anomalia: detector inicial em `app/api/views.py:433-434`; selfie em `:447-453`; mock em `:457-464`; assinatura/liveness em `:465-495`; E/S incompleta em `:604-645`; desvio grave em `:703-707`; ajuste da saída sem turno em `:748-756`. Só chamar `detectar_anomalias(colab)` não reproduz o flag final — os sítios posteriores acrescentam e removem motivos nessas linhas.

A referência da selfie também confere: `app/api/views_core.py:426-427` é comentário do desvio APK/POST separado; a regra executável está em `:433-439` [essas linhas finais precisam conferência do root; equivalentes verificados em `app/api/views.py:447-453`].

Aliases e leitores da derivação “disputa aberta”:

- Reexport compatível: `app/chamados/models.py:4234-4236`.
- Leitor REST: `app/api/views.py:1335-1336,1670-1672`.
- Leitor de autorização: `app/colaboradores/mixins.py:304-312`, só POST; isentos passam antes em `:301-303`. Decoradores ativos: `app/holerite/views.py:1144`; `app/colaboradores/views.py:1368,1603,1769,1877`. Decoradores isentos: `app/colaboradores/views.py:1410,1425`; `app/ferias/views.py:287,347,686,773`.
- Wrapper `motor_bloqueado_por_disputa`: `app/ponto/services/disputa.py:15-31`. Leitores: `app/ponto/views.py:140-144,207` → faixa em `app/templates/ponto/espelho.html:13-18`; PDF `app/relatorios/pdf_espelho.py:1082-1090,1113` → execução dos bloqueios em `app/relatorios/pdf_base.py:129-131`.
- Variável homônima do painel: `app/colaboradores/painel_cell.py:23` lê conjunto recebido ou `:31-32` calcula `exists(disputa aberta)`; `:68` transforma em LED vermelho. Único chamador de produção encontrado passa sem mapas em `:81`; template lê LED em `app/templates/colaboradores/partials/_ps_linha.html:1,6`.
- Fonte paralela do LED em lote: `app/colaboradores/services/situacional.py:117` monta conjunto de colaboradores com disputa aberta; `:56-59` lê e `:210` publica LED.
- `em_disputa_fechada` é outra variável: `app/chamados/services/fio.py:264-268`; consumidor admin em `app/templates/chamados/partials/modal_fio.html:289`; não equivale ao sinal de abertura da tela de perguntas.

Equivalentes de apresentação verificados:

- Autoridade `app/chamados/services/pergunta_no_app.py:88-119`: `questionarios_pendentes = len(pendentes)` em `:101`; `chamados_badge` prioriza pendentes em `:108-113`; sem perguntas usa chamados não lidos calculados em `:102-107`.
- Propagação `/me`: `app/api/views.py:315-319,359-360`; `app/api/views_core.py:303-307,347-348`.
- Dashboard web: `app/core/views.py:239,252-253,268`; leitores do link/badge/rótulo em `app/templates/core/dashboard_colaborador_v2.html:78-80`.
- Painel do colaborador: `app/colaboradores/views.py:940-941` → `app/templates/colaboradores/partials/painel_colaborador.html:39,190-191`.
- Listagem `/chamados/v2`: consulta em `app/api/views.py:997-998`; pendentes e histórico vivo de disputa aberta em `:1033-1035`; destino vivo ou pai em `:1036-1040`; serialização `perguntas_disputa` em `:1148-1167`.

Não localizei consumidor HTML/JS literal de `em_disputa` ou `motivos_disputa`; consumidor nativo precisa ser sustentado pelo HANDOFF ou marcado `[não verificado]`. Os únicos usos locais de `motivos_disputa` são os payloads citados e a prosa de `app/ponto/services/aviso_da_batida.py:14`; o serviço lê a lista interna para a frase em `:69-71`, chamado em `app/api/views.py:812` / `app/api/views_core.py:806`.

Impedimentos concretos para uma sonda histórica exata:

- `Batida` não guarda flag nem lista de motivos; campos em `app/ponto/models.py:8-95`.
- `detectar_anomalias(colaborador)` não aceita instante (`app/chamados/detector_anomalias.py:19`), lê escalas ativas atuais em `:58-62`, `timezone.now()` em `:73` e cadastro de posto atual em `:101-111`.
- A função **pode escrever e enviar push**: chama `bloquear_se_em_ferias` em `app/chamados/detector_anomalias.py:114-121`; este cria chamado em `app/ponto/utils.py:103-125` e envia push em `:127-133`. O detector engole o erro em `contexto['_erro_ferias']`, sem contador `engolidos`, em `detector_anomalias.py:120-121`. Interceptar escritor antes da tentativa é necessário se a sonda chamar esse caminho; barreira SQL somente leitura deve ser adicional.
- `ultima_batida` guarda episódio corrente em `app/api/views.py:761-765`, mas o agregado substitui outro episódio em `app/chamados/services/disputa_emissao.py:569-575`; não é histórico completo.
- Canal é dado persistido em `app/ponto/models.py:46-55`: `NULL` anterior a 27/10/2026 ou sem aparelho; o próprio contrato proíbe inferir canal por `timestamp_dispositivo` em `:48`.

## 15/11 23:25 · sessao 24e606ed885e · - · - · origem=Ronald (tmux)

Roteamento e contrato, somente pela cópia:

- `CLAUDE.md:84-89` descreve as cascas; `Caddyfile:8-10` confirma: `/api/ponto/*`, `/api/auth/*`, `/api/me/`, `/api/regularizacao/*`, `/api/ping-geo/`, `/api/registrar-fcm/`, `/health/` → core; restante → UI. O domínio Juliani importa esse bloco em `Caddyfile:32-38`.
- Core: `app/config/urls_core.py:5-6` inclui `api.urls_core`; `app/api/urls_core.py:5` importa `views_core`; `:13` aponta `/api/me/`, `:14` `/api/ponto/bater/`.
- UI: `app/config/urls.py:37` inclui `api.urls`; `app/api/urls.py:2` importa `views`; `:81` espelho/v2; `:92` disputa/perguntas; `:93` disputa/responder; `:73` chamados/v2. Seu gêmeo bater (`:67`) existe no URLconf UI, mas o Caddy encaminha essa URL ao core.
- Configuração aceita a escolha do URLconf por variável em `app/config/settings/saas.py:76-78`; definição real do ambiente/container rodando não foi inspecionada [não verificado].

Equivalentes de apresentação:

| Fonte e escritor | Leitores/serializadores |
|---|---|
| `consulta_do_app` retorna partição pendentes/histórico (`app/chamados/services/pergunta_no_app.py:64-85`). Pendente = C6 viva, `respondida_em is None`, disputa aberta (`:73`, `:80-85`). | `/api/disputa/perguntas/`: consulta em `app/api/views.py:2537-2539`; `em_disputa=bool(perguntas)` em `:2569`; `perguntas` em `:2576`; `respondidas` como subconjunto histórico em `:2577`; `historico` em `:2578`. |
| `projecao_do_app`: `questionarios_pendentes=len(pendentes)` (`app/chamados/services/pergunta_no_app.py:100-101`); badge prioriza pendentes com rótulo “Responder DP”, depois chamados não lidos, depois `None` (`:102-113`). | Core `/me`: recebe em `app/api/views_core.py:303-307`, transmite em `:346-348`; UI gêmeo `app/api/views.py:315-319`, `:358-360`. Android usando esses campos para abrir tela [não verificado]. |
| Dashboard conta a mesma lista (`app/core/views.py:252-253`) e transmite em `:268`. | `app/templates/core/dashboard_colaborador_v2.html:78-80`: decide sufixo do link `?aba=cham`, badge e rótulo “Responder DP”. |
| Projeção administrativa (`app/colaboradores/views.py:940-941`). | `app/templates/colaboradores/partials/painel_colaborador.html:39`, `:190-191`: total de mensagens e descrição “No app dele agora”. |
| Chamados/v2 reutiliza `consulta_do_app`, junta pendentes+histórico, conserva só disputas abertas e ancora em destino vivo ou pai (`app/api/views.py:996-1040`). | Serializa `perguntas_disputa` em `:1148-1168`; `pode_solicitar_correcao` usa `aguarda_o_colaborador` em `:1174-1180`. `pode_responder` em `:1145` diz chamado não terminal, portanto não é autorização individual de PerguntaDisputa. |

Outras chaves que devem ser distinguidas:

- `/me` mantém `regularizacao_obrigatoria=False` e, por isso, `entradas_pendentes=[]` (`app/api/views_core.py:314-341`).
- `/api/regularizacao/pendente/` lista **RegularizacaoExterna** com `status='aguardando'`, retornando `pendentes`; trata outro modelo/universo (`app/api/views_core.py:1271-1289`; rota `app/api/urls_core.py:15`).
- `aviso_texto_livre` também consulta as pendentes atuais e retorna orientação para abrir chamado/pergunta, sem novo predicado (`app/chamados/services/aviso_pendencia.py:7-20`).

Contrato e relato:

- `app/docs/HANDOFF.md:1-219` cobre login, ausência, plataforma, troca de senha; não contém contrato de `em_disputa` ou `/api/disputa/perguntas/` [não verificado como contrato existente].
- `app/api/views_core.py:426-427` e `app/api/views.py:440-441` mencionam em comentário o APK preso em `PerguntasDisputaActivity`. Isso documenta a intenção/comportamento relatado; não demonstra um leitor Android executável.
- Não foi localizado código Kotlin/Java/Swift/Dart na busca da cópia [não verificado]. Android 3.9.3, a condição executada para abrir a Activity e o texto mostrado no aparelho continuam [não verificado].
- O texto web “Nenhuma pendência” está condicionado a `not pendencias` em `app/templates/chamados/meu_atendimento.html:117-120`; não prova a tela nativa mencionada no relato.

## 15/11 23:25 · sessao 2f274523329a · - · - · origem=Ronald (tmux)

Candidatos distintos, sem escolher:

1. **Receber ou corrigir resposta pela porta:** `pergunta_viva(p, hoje=None, *, carga=None, agora=None)`, registrado como C6 em `app/core/juizes.py:112` e `app/core/juizes.py:286`; implementação/memo em `app/chamados/juizes.py:865`, predicado em `app/chamados/juizes.py:1402`. A propriedade `.viva` explicita “Pode receber (ou corrigir) resposta” e delega ao juiz (`app/chamados/models.py:3473-3484`). A porta POST efetivamente consulta C6 para a pergunta pertencente ao colaborador, sem exigir disputa aberta nem resposta ausente (`app/api/views.py:2609-2620`).

   Predicado em ordem: validação mata; tranca atual mata; vias terminais materializadas vencem resposta; resposta carimbada com texto mantém viva salvo decisão administrativa e concordância com luz; identidade materializada mata; marco materializado no espelho mata; célula acusadora vive, célula absolutória completa mata; falta na janela viva mantém; sem materialização vive; com materialização depende testemunha externa (`app/chamados/juizes.py:1403-1492`). **Universo:** uma pergunta recebida pelo juiz, podendo ser de conversa e podendo estar em disputa fechada — nenhuma guarda de fechamento consta no corpo (`app/chamados/juizes.py:1402-1492`).

2. **Pergunta pendente que a lista do APK oferece:** `consulta_do_app(colab, *, agora=None)` → `particao_do_app(perguntas_do_app(...))` (`app/chamados/services/pergunta_no_app.py:64-85`). `perguntas_do_app` consulta perguntas de todas as disputas daquele colaborador, pré-filtra pelo generoso `q_vivas`, ordena por id e confirma C6 (`app/chamados/services/pergunta_no_app.py:45-61`). `particao_do_app` considera pendente **C6 viva E `respondida_em is None` E `disputa.fechada_em is None`**; demais vivas vão ao histórico (`app/chamados/services/pergunta_no_app.py:76-85`). GET `/disputa/perguntas/` usa essa lista e escreve `em_disputa=bool(pendentes)` (`app/api/views.py:2537-2539`, `app/api/views.py:2566-2578`). **Diferença:** C6 admite correção de viva já respondida e viva sob disputa fechada; a lista pendente exclui ambas (`app/chamados/models.py:3475`, `app/api/views.py:2612-2620`, `app/chamados/services/pergunta_no_app.py:81-84`).

`fio_vivo(perguntas)` é a redução registrada C15: `any(pergunta_viva(p) for p in perguntas)`; universo é explicitamente do chamador, vazio dá falso; não acrescenta os filtros da lista pendente (`app/core/juizes.py:123-125`, `app/core/juizes.py:295`, `app/chamados/juizes.py:1507-1516`).

C19 `tarefa_do_dia` é outro domínio: ações exigíveis por **instância de marco de um colaborador-dia**, após decisões e conferência de efeitos, com estado `nao_sei`; não substitui a pergunta de alcance geral do questionário (`app/core/juizes.py:299`, `app/docs/JUIZES.md:78-93`, `app/chamados/services/tarefa_do_dia.py:1-26`). Seu `fio=colab` exige ação `responder` de colaborador **e pergunta pendente no app**, delegando a C6 + `particao_do_app` (`app/chamados/services/tarefa_do_dia.py:37-50`). C17 exige âncora na fila e pergunta sobre porta do dia/painel, não disponibilidade geral de responder (`app/chamados/juizes.py:1667-1696`, `app/chamados/juizes.py:1704-1713`).

**Tempo passado: aceita parâmetro, mas não replay integral.** `agora` chega ao C6 e muda o dia da janela (`app/chamados/juizes.py:875-890`); a projeção apenas oculta validação/resposta/materialização posteriores e conserva identidade atual (`app/chamados/juizes.py:1381-1399`). Permanecem:

- Pré-filtro `q_vivas` do estado atual, anterior ao C6 (`app/chamados/services/pergunta_no_app.py:52-61`).
- Partição lê `respondida_em` e `fechada_em` atuais, sem projeção (`app/chamados/services/pergunta_no_app.py:73-85`).
- Tranca não recebe `agora`; lê `periodo_trancado` atual e toques posteriores ao fechamento atual (`app/chamados/juizes.py:977-995`, `app/chamados/juizes.py:1065-1082`).
- C6 lê `celula.veredito/veredito_via` atuais (`app/chamados/juizes.py:1417-1419`, `app/chamados/juizes.py:1450-1466`).

Portanto rotular `consulta_do_app(colab, agora=batida.criado_em)` como “pergunta existente e respondível no instante da batida” seria incorreto pelas próprias leituras acima. A sonda pode entregar **aproximação explicitamente atual**: detector de anomalia da batida chamado agora × ausência de pendentes atuais do colaborador, e em paralelo ausência de C6 vivas atuais; resultado contemporâneo exato continua `[nao verificado]` sem reconstrução histórica integral. Se preferir projeção parcial, rotulá-la literalmente como tal e conservar os dois resultados separados (`app/chamados/services/pergunta_no_app.py:45-85`, `app/chamados/juizes.py:1381-1399`).

Para sonda sem escrita: leitores do app retornam listas sem `.save()` (`app/chamados/services/pergunta_no_app.py:45-85`); C6 apenas memoiza na instância (`app/chamados/juizes.py:893-905`), embora erros possam ser engolidos nos degraus inferiores (`app/chamados/juizes.py:1476-1492`). `engolidos()` fornece fotografia cumulativa, não zerar (`app/core/observ.py:60-70`); `registrar_engolido` incrementa contador e emite logging (`app/core/observ.py:73-82`). Assim comparar contador antes/depois da função e desabilitar logging como no molde; exceção ou incremento torna classificação indecidível (`app/core/observ.py:60-82`).

O helper terceira derivação é só existência de disputa aberta, sem perguntas: `colaborador_em_disputa(colaborador)` retorna primeira `DisputaSupervisao` com `fechada_em__isnull=True` (`app/chamados/services/disputa_emissao.py:34-42`).

## 15/11 23:18 · sessao bcbcdfa003d8 · E4C-F1NOVA-B1 · verificador · origem=Code

**Persistem dois problemas: cache desatualizado na guarda do `save` e falsa absolvição no contrato AST.** (`app/chamados/models.py:3588`; `app/chamados/tests/test_contract_par_resposta.py:92`)

Identidade independente do HEAD, comparação Git e execução com Django/banco: **[não verificado]**. Os contraexemplos abaixo decorrem da cópia; os helpers foram executados isoladamente em memória, sem banco. (`app/chamados/models.py:3592`; `app/chamados/tests/test_contract_par_resposta.py:101`)

**1. Fechamento dos achados procedentes da volta 1**

| Achado | Veredito sobre o fechamento |
|---|---|
| **1 — par persistido** | **NÃO PROCEDE o fechamento integral.** Fecha o exemplo original: a metade excluída de `update_fields` passa a vir de `_par_do_banco`. Entretanto, esse método pode devolver cache desatualizado; contraexemplos no item 2. (`app/chamados/models.py:3601`; `app/chamados/models.py:3602`; `app/chamados/models.py:3588`) |
| **4 — guarda do contrato** | **NÃO PROCEDE o fechamento integral.** Os dois exemplos antigos agora são acusados, mas basta um `if` com saída antecipada em linha anterior, mesmo num caminho que não será executado. (`app/chamados/tests/test_contract_par_resposta.py:156`; `app/chamados/tests/test_contract_par_resposta.py:157`; `app/chamados/tests/test_contract_par_resposta.py:92`) |
| **5 — fixture mista** | **PROCEDE.** O teste adicional recria ambas com texto, retira os instantes por `update` e verifica uma materialização, nenhuma resolução adicional, nenhum fechamento e disputa aberta. Identidade histórica dos asserts com `e84a167f4`: **[não verificado]**. (`app/chamados/tests/test_fio_disputa_resolvido.py:62`; `app/chamados/tests/test_fio_disputa_resolvido.py:100`; `app/chamados/tests/test_fio_disputa_resolvido.py:103`; `app/chamados/tests/test_fio_disputa_resolvido.py:105`; `app/chamados/tests/test_fio_disputa_resolvido.py:108`) |
| **6 — JUIZES.md** | **PROCEDE.** A tabela registra o juiz, sua face SQL e o escopo B1/B2/E-F, acompanhando o registro vivo. (`app/docs/JUIZES.md:99`; `app/docs/JUIZES.md:103`; `app/core/juizes.py:306`) |
| **7 — instante automático** | **PROCEDE.** O comentário agora diz PROCESSAMENTO, conforme o relógio usado pelo chamador. (`app/chamados/services/disputa_emissao.py:1671`; `app/api/foto_recebida.py:42`; `app/api/foto_recebida.py:48`) |

**2. Sequências que quebram a garantia — PROCEDE**

**a) Duas instâncias, sem concorrência nem escrita crua.** Usando `_cria` da fixture existente e `I` como instante válido: (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:58`)

```python
p = self._cria(respondida_em=I, resposta_colab='bati')
q = PD.objects.get(pk=p.pk)

q.respondida_em, q.resposta_colab = None, ''
q.save(update_fields=['respondida_em', 'resposta_colab'])

p.save(update_fields=['respondida_em'])
```

Após salvar `q`, o banco contém `(None, '')`, mas o cache de `p` continua `(I, 'bati')`. No último save, `_par_gravado` calcula `(I, 'bati')`; a comparação com o mesmo cache dispensa a recusa. Só o instante é escrito: o banco fica **`(I, '')`, ilegal**. (`app/chamados/models.py:3554`; `app/chamados/models.py:3588`; `app/chamados/models.py:3601`; `app/chamados/models.py:3608`)

**b) Rollback externo seguido de save repetido.** Partindo de `(None, '')`, salvar `(I, 'bati')` dentro de um `atomic` externo atualiza `_par_carregado`; se esse `atomic` depois falhar, o atributo não é restaurado. Repetir `save(update_fields=['respondida_em'])` produz a mesma divergência do exemplo anterior. O `atomic` interno não adia a atualização do cache até o commit externo. (`app/chamados/models.py:3552`; `app/chamados/models.py:3554`; `app/chamados/models.py:3608`)

**c) Legado ilegal intocado passa a ser recusado.** A fixture `_legado` fornece o estado inicial: (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:62`)

```python
p = self._legado(resposta_colab='legado')  # banco: (None, 'legado')

try:
    with transaction.atomic():
        p.respondida_em = I
        p.save()                        # cache: (I, 'legado')
        raise RuntimeError()
except RuntimeError:
    pass

p.respondida_em = None                  # memória igual ao banco novamente
p.pergunta = 'outro enunciado'
p.save()
```

O último save compara `(None, 'legado')` com o cache `(I, 'legado')`, considera uma transição e levanta `TextoSemCarimbo`, embora o par gravado permaneça intocado. Isso viola a decisão (1) expressa no teste. (`app/chamados/models.py:3608`; `app/chamados/models.py:3611`; `app/chamados/services/fio.py:1610`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:5`)

O refresh integral recompõe o cache a partir dos campos carregados; refresh parcial que toca o par o torna desconhecido. Essas medidas cobrem o caso diferido acrescentado no RED, mas não invalidam outras instâncias nem revertem o cache após rollback externo. (`app/chamados/models.py:3573`; `app/chamados/models.py:3577`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:137`; `app/chamados/models.py:3554`)

**3. `_par_carregado` — NÃO PROCEDE que represente sempre “banco agora”**

| Acesso | Local |
|---|---|
| Escreve após save | `app/chamados/models.py:3554` |
| Escreve no `from_db` | `app/chamados/models.py:3566` |
| Escreve após refresh integral | `app/chamados/models.py:3573` |
| Escreve `None` após refresh parcial do par | `app/chamados/models.py:3577` |
| Lê em `_par_do_banco` | `app/chamados/models.py:3587` |
| Lê em `_par_gravado`, quando nenhum campo do par será salvo | `app/chamados/models.py:3600` |

**NÃO PROCEDE que algum desses leitores interprete `None` como par vazio.** `_par_do_banco` trata como desconhecido e consulta a linha existente; `_par_gravado` apenas propaga o desconhecido no caminho que não escreve o par. Porém, valor conhecido significa efetivamente uma fotografia carregada, não necessariamente o banco atual. (`app/chamados/models.py:3588`; `app/chamados/models.py:3590`; `app/chamados/models.py:3600`)

**4. Custo — PROCEDE, condicionado ao estado do cache**

Contagem de consultas feitas especificamente por `_par_do_banco`, para instância existente: (`app/chamados/models.py:3585`)

| Estado e save | SELECTs |
|---|---:|
| Cache conhecido | **0** — retorna o cache. (`app/chamados/models.py:3588`) |
| Cache desconhecido; `update_fields` sem o par | **0** — ambos os caminhos dispensam consulta. (`app/chamados/models.py:3600`; `app/chamados/models.py:3606`) |
| Cache desconhecido; ambas as metades em `update_fields`, ou save sem `update_fields` | **1** — comparação da recusa. (`app/chamados/models.py:3608`) |
| Cache desconhecido; somente uma metade em `update_fields` | **2** — composição e comparação; a primeira leitura não é memorizada. (`app/chamados/models.py:3601`; `app/chamados/models.py:3608`; `app/chamados/models.py:3590`) |

Sem `update_fields`, carregar atributos ainda diferidos pode acrescentar consultas próprias antes da comparação. A contagem real com Django/banco é **[não verificado]**. (`app/chamados/models.py:3597`; `app/chamados/models.py:3577`)

Nos fluxos de produção localizados após refresh parcial:

- `devolver_validacao_recusada` atualiza parcialmente `resposta_colab`, invalidando o cache, mas salva só validação: **zero SELECTs do par**. (`app/chamados/services/validacao.py:213`; `app/chamados/services/validacao.py:227`; `app/chamados/models.py:3606`)
- `_carimbar_recusa` atualiza parcialmente campos fora do par e salva recusa ou carimbo de identidade: **zero SELECTs do par**. (`app/chamados/services/materializacao.py:1095`; `app/chamados/services/materializacao.py:1105`; `app/chamados/services/materializacao.py:1109`; `app/chamados/models.py:3291`)
- `_aceitar_a_folga` pode repetir a operação após refresh dos campos de validação; as escritas correspondentes excluem o par. (`app/chamados/services/validacao.py:370`; `app/chamados/services/validacao.py:374`; `app/chamados/services/validacao.py:162`; `app/chamados/models.py:3291`)

Não localizei fluxo `only/defer → save` que efetivamente introduza essas consultas. Os `only` relevantes alimentam atos, contadores ou caminhos DRY; frequência real em produção e completude contra chamadas dinâmicas: **[não verificado]**. (`app/chamados/models.py:1613`; `app/chamados/services/fio.py:1235`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:60`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:504`)

**5. Contrato — PROCEDE falsa absolvição; NÃO PROCEDE acusar os dois escritores citados**

Este contraexemplo é absolvido pelo helper real: (`app/chamados/tests/test_contract_par_resposta.py:92`; `app/chamados/tests/test_contract_par_resposta.py:108`)

```python
def f(p, qs, habilitado):
    if habilitado:
        if not p.resposta_colab:
            return
    qs.update(respondida_em=agora)
```

Com `habilitado=False` e a pergunta vazia, a guarda nem executa. A absolvição ocorre porque o `return` está numa linha anterior à escrita, sem comprovação do caminho de controle. **É uma falha adicional à polaridade expressamente excluída do selo.** A leitura da outra metade também não exige identidade entre as instâncias. (`app/chamados/tests/test_contract_par_resposta.py:92`; `app/chamados/tests/test_contract_par_resposta.py:22`; `app/chamados/tests/test_contract_par_resposta.py:76`)

As duas absolvições de produção examinadas são corretas pela forma:

- `PerguntaDisputa.carimbar` escreve o instante dentro da guarda de texto após trim e inclui o campo no save. (`app/chamados/models.py:3231`; `app/chamados/models.py:3233`; `app/chamados/models.py:3234`; `app/chamados/models.py:3253`)
- `carimbar_resposta_pela_trilha` retorna antes da escrita quando já existe instante ou falta texto após trim; depois salva o instante. (`app/chamados/services/disputa_emissao.py:1692`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1698`)

**6. Descartes 2 e 3 — PROCEDEM sob as classificações declaradas na triagem; conferência nas fontes originais [não verificado]**

- **2, I21:** a regra documentada permite à classe A nomear o juiz no B1; o código reconhece explicitamente a mudança de predicado para trim. A classificação específica de I21 no censo, a exceção no predesenho e “brancos = 0” constam da triagem fornecida, mas sua confirmação nos documentos originais é **[não verificado]**. (`app/docs/JUIZES.md:102`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1572`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`)
- **3, espera pelo texto:** a regra documentada exclui E/F da migração, portanto o descarte é coerente **se** a função é classe E, como informa a triagem. Essa classificação nominal no censo é **[não verificado]**. O complemento continua no código; sua existência, isoladamente, não demonstra violação desse escopo. (`app/docs/JUIZES.md:103`; `app/chamados/services/fio.py:187`; `app/chamados/juizes.py:1552`)

## 15/11 23:16 · sessao d9a18c11cd9b · - · - · origem=Ronald (tmux)

Item 3:

- Escrevem `_par_carregado`: `save` (`app/chamados/models.py:3554`), `from_db` (`:3566`), `refresh_from_db` integral (`:3573`) e parcial que toca o par (`:3577`).
- Leem diretamente: `_par_do_banco` (`:3587`) e `_par_gravado` quando nenhum campo do par vai ao UPDATE (`:3600`).
- `None` significa desconhecido: `_par_do_banco` consulta a linha existente (`:3588-3590`); `_par_gravado` apenas o propaga no caminho sem escrita do par (`:3599-3600`). Nenhum desses leitores transforma `None` em `(None, '')`.
- O significado “banco agora” depende de o cache continuar válido; `save` o atualiza antes de eventual rollback de transação externa, sem mecanismo de reversão do atributo (`:3552-3554`).

Item 4:

| Estado/caminho | SELECTs de `_par_do_banco` |
|---|---|
| Cache conhecido | 0 (`app/chamados/models.py:3587-3589`) |
| Cache desconhecido, `update_fields` sem par | 0 (`:3599-3600`, `:3606-3607`) |
| Cache desconhecido, ambas as metades ou `save()` | 1 na comparação (`:3608`, `:3590`) |
| Cache desconhecido, somente uma metade | **2**, pois há chamadas em `:3601` e `:3608`, sem memorizar o resultado em `:3590` |

Sem `update_fields`, o acesso às duas metades acontece antes da comparação (`app/chamados/models.py:3596-3597`); cargas diferidas podem somar consultas próprias, além do SELECT acima. Execução com Django/banco: [não verificado].

Não identifiquei chamador de produção atual que efetivamente acrescente essas consultas:

- `only` com **ambas** as metades: ressuscitação usa instâncias apenas para construir atos (`app/chamados/models.py:1612-1613`); DRY do backfill também inclui ambas (`app/chamados/management/commands/backfill_decisoes_pergunta.py:60`).
- `only` **sem ambas**: contador de destino (`app/chamados/services/fio.py:1235-1244`) e DRY de colisões (`app/chamados/management/commands/backfill_decisoes_pergunta.py:504-510`); não salvam essas instâncias nesses fluxos.
- `devolver_validacao_recusada` invalida o cache ao atualizar só `resposta_colab` do par (`app/chamados/services/validacao.py:213-214`), mas salva apenas validação (`:227`): **0** consultas do par. Depois, o materializador usa novas instâncias de um queryset (`:494`; `app/chamados/services/materializacao.py:1375-1382`).
- `_carimbar_recusa` faz refresh parcial sem o par (`app/chamados/services/materializacao.py:1095`) e salva recusa (`:1109`) ou carimbo de identidade (`:1105`; `app/chamados/models.py:3287-3291`): **0** consultas do par.
- `_aceitar_a_folga` faz refresh de validação (`app/chamados/services/validacao.py:370`) e pode repetir a operação (`:374`); saves seguem sem o par (`:162`; `app/chamados/models.py:3165`, `:3291`).
- Outros refreshs parciais terminam em leitura/retorno: validação (`app/chamados/services/validacao.py:497`) e classificação do passivo (`app/chamados/services/passivo_validado.py:140-145`).

Quantidade/frequência real de consultas em produção: [não verificado].

## 15/11 23:15 · sessao a574bcd3a4b2 · - · - · origem=Ronald (tmux)

**Achado 4: PROCEDE; o delta fecha os exemplos antigos, mas não fecha a exigência “guarda governa a escrita”.** Os novos mutantes cobrem `if` solto e escrita anterior à guarda; `_governada` ainda absolve qualquer guarda com `return`/`raise` cuja linha preceda a escrita, sem verificar bloco, caminho de execução ou escopo. (`app/chamados/tests/test_contract_par_resposta.py:156`; `app/chamados/tests/test_contract_par_resposta.py:157`; `app/chamados/tests/test_contract_par_resposta.py:86`; `app/chamados/tests/test_contract_par_resposta.py:92`)

Contraexemplo absolvido pelo helper real (`meia_par` retorna `None`), executado em memória com `python3 -B`:

```python
def f(p, qs, habilitado):
    if habilitado:
        if not p.resposta_colab:
            return
    qs.update(respondida_em=agora)
```

Com `habilitado=False`, `p` e `qs` apontando para uma pergunta `(None, '')`, grava `(agora, '')`. A guarda não foi executada; a absolvição decorre exclusivamente de `_sai_cedo(g)` e `g.lineno < escrita.lineno`. Não é o limite declarado de polaridade. (`app/chamados/tests/test_contract_par_resposta.py:79`; `app/chamados/tests/test_contract_par_resposta.py:92`; `app/chamados/tests/test_contract_par_resposta.py:22`)

Também são absolvidos: guarda numa função interna nunca chamada; guarda que lê `q.resposta_colab` enquanto escreve `p.respondida_em`. A busca usa `ast.walk(fn)` e `_le` compara somente o nome do atributo. (`app/chamados/tests/test_contract_par_resposta.py:75`; `app/chamados/tests/test_contract_par_resposta.py:86`)

**Item 5: PROCEDE quanto a falsos negativos; NÃO PROCEDE quanto a acusação dos dois escritores legítimos citados.** A varredura real extraída por AST retornou `[]`; ambos foram absolvidos. A implementação da varredura está em `app/chamados/tests/test_contract_par_resposta.py:113`; evidência persistida dessa execução: **[não verificado]**.

- `PerguntaDisputa.carimbar`: só atribui instante sob guarda de texto não vazio após trim; inclui `respondida_em` nos campos salvos. A absolvição é correta. (`app/chamados/models.py:3231`; `app/chamados/models.py:3233`; `app/chamados/models.py:3234`; `app/chamados/models.py:3253`)
- `carimbar_resposta_pela_trilha`: retorna antes da escrita se já tem instante ou não tem texto após trim; depois atribui e salva somente o instante. A absolvição é correta. (`app/chamados/services/disputa_emissao.py:1692`; `app/chamados/services/disputa_emissao.py:1693`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/services/disputa_emissao.py:1698`)

## 15/11 23:15 · sessao ff3804764101 · - · - · origem=Ronald (tmux)

Achados 5, 6 e 7: **fechados pelo delta**.

- **5:** o teste novo recria duas perguntas com texto pelo helper, que fornece texto e instante, e depois zera os instantes de ambas por `update`; conserva os asserts de uma materialização, zero novas resoluções, zero fechamentos e disputa aberta. (`app/chamados/tests/test_fio_disputa_resolvido.py:56`, `:62`, `:94`, `:100`, `:103`, `:105`, `:106`, `:108`). Igualdade com os asserts históricos de `e84a167f4`: **[não verificado]**, pois não há Git na cópia.
- **6:** a tabela agora registra `pergunta_respondida`, a face SQL, a recusa simétrica e o escopo B1/B2/E-F; corresponde ao registro `_C21`. (`app/docs/JUIZES.md:99`, `:100`, `:101`, `:102`, `:103`; `app/core/juizes.py:306`).
- **7:** o comentário agora diz instante do **PROCESSAMENTO**, conferindo com `agora = _tz.now()` passado pelo chamador. (`app/chamados/services/disputa_emissao.py:1671`; `app/api/foto_recebida.py:42`, `:48`).

Descartes 2 e 3: **conferência documental específica [não verificado]**. Os arquivos `logs/JUIZ-RESPONDIDA/censo.md`, predesenho e aval citados não foram encontrados na cópia; portanto não é possível confirmar independentemente “I21 = linha 29/classe A”, “espera = linha 87/classe E”, a exceção “I21 sem trim”, ou a medição de brancos zero. **[não verificado]**

O que a cópia confirma:

- I21 usa a face SQL com trim, e o comentário reconhece expressamente a diferença em relação ao predicado anterior. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572`, `:1573`, `:1575`).
- `espera_o_texto_do_colaborador` continua calculando o complemento do juiz por instante ausente ou instante sem texto. (`app/chamados/services/fio.py:187`; `app/chamados/juizes.py:1534`, `:1552`).
- A regra geral registrada para B1 é migrar classe A, postergar B/C/D e conservar E/F; isso corrobora a lógica dos descartes se as classificações nominais da triagem estiverem corretas, mas não prova essas classificações. (`app/docs/JUIZES.md:102`, `:103`; `app/chamados/juizes.py:1550`, `:1551`).

_5858 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
