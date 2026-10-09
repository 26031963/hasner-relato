# RELATO — esteira saas-hasner

## O230 POUSO 1 — **A SONDA DE LEITURA DO DEV DA CASA ESTA NA ARVORE** (09/10 14:1x, POUSO 1 FECHADO)

PROVA: `bin/sonda_leitura.sh` na arvore (de `1723901e` da raia `agent-a375cb746ca034f0c`), os tres REDs
rodados **como `fernando`** pelo caminho real `sudo -u ronald`: `logs/o230/red_a_20261009.txt` (886
colaboradores pelo ORM e **886** por SQL cru, nos DOIS bancos), `logs/o230/red_b_20261009.txt` (**4 de 4**
escritas negadas por `permission denied for table` com o cinto DESLIGADO, contagem 886 antes e depois nos
dois bancos), `logs/o230/red_e_20261009.txt` (**18** variaveis no env da sonda, **0** chaves da casa,
contra **26** no `saas_core`) e `logs/o230/red_dono_e_argv_20261009.txt`; **64 selos de host verdes**.

**O AVAL, LITERAL** (09/10 13:4x, registrado no PROMPTS): *"O230 pousa AGORA, na frente do que vier depois
do apply do R1. O role leitor e o sudoers ja existem; falta bin/sonda_leitura.sh na arvore e a leitura de
logs e docs. Pousa o script com os REDs a, b e e provados; os outros REDs e o selo vem no pouso seguinte de
instrumento. Nenhuma lei nova."* O escopo e literal e foi cumprido literalmente: pousou **um arquivo**,
`bin/sonda_leitura.sh`. O selo `bin/tests/test_sonda_leitura.sh`, que existe na raia e pergunta ao CATALOGO
do Postgres, **ficou lá** — ele e o POUSO 2, com os REDs c, d e f. Ato de INSTRUMENTO proprio (L-105),
depois do produto, e **sem deploy**: nada que o gunicorn serve mudou.

**UMA CORRECAO AO SCRIPT ANTES DE POUSAR, e e a secao 4 do CLAUDE.md se aplicando a mim.** As linhas 15-16
prometiam que *"o selo le `has_table_privilege` do Postgres, nao esta prosa"* — e o selo nao vinha neste
pouso. Promessa de guarda inexistente e exatamente o que fez o cartorio ler batida crua por meses. A linha
agora DIZ que o selo e o POUSO 2 e que ate ele a guarda e documental mais o RED b.

**O RED b MEDIA O CINTO E DAVA ISSO POR PROVA — e o cinto e desfazivel em uma linha.** Aqui o defeito era
meu, e ele e o ponto inteiro da fatia. `default_transaction_read_only = on` faz o Postgres recusar com
**25006** (`cannot execute UPDATE in a read-only transaction`) **antes** de olhar privilegio de tabela: a
mensagem prova o CINTO, e o proprio aval chama o cinto de cinto (*"quem segura e o GRANT, nao o parametro
de sessao"*; um `SET ... = off` o desfaz, e e `USERSET` — o leitor pode). O RED passou a morder nos DOIS
valores, e so a rodada 2 certifica:

- **rodada 1, cinto LIGADO**: UPDATE, DELETE, INSERT e UPDATE-pelo-ORM todos `cannot execute ... in a
  read-only transaction`;
- **rodada 2, cinto DESLIGADO pelo proprio leitor**: os quatro `permission denied for table
  colaboradores_colaborador`. **4 de 4 pelo GRANT**, em `saas_hasner` **e** em `sombra`.

Toda tentativa tem `where/values id = -1` (0 linhas possiveis) dentro de `atomic()` com `raise` no fim — a
lei de 27/08 —, e a contagem fecha **886 antes e 886 depois** nas quatro rodadas.

**E A PRIMEIRA FORMA DO RED b ERA UMA SONDA MAL PARAMETRIZADA**, o erro que esta casa ja leu como bug do
sistema sete vezes: ela escrevia `set matricula = matricula`, e `matricula` **nao e coluna** de
`colaboradores_colaborador` nem campo do modelo. Duas das quatro tentativas voltavam `column "matricula"
does not exist` e `Colaborador has no field named 'matricula'` — negadas pela minha SONDA, nunca pelo
GRANT, e a rodada 2 saiu **VERMELHA** com `2 de 4 por PERMISSION DENIED`. A sonda passou a usar `situacao`,
que existe, preservando o valor nas duas formas (`situacao = situacao`, `F('situacao')`). A trilha guardou
as duas: sha `d4f4e79d…` (errada) e `141188fd…` (certa), `quem=fernando` nas seis linhas.

**O ARGUMENTO E DO CHAMADOR, e isso se mediu, nao se supos** (`logs/o230/red_dono_e_argv_20261009.txt`).
Como `fernando`, os quatro alvos de outro dono dao **rc 2** sem devolver um byte de conteudo — `.env`,
`logs/.env_teste`, `logs/.env_leitor` e `app/ponto/turnos.py` —, porque o veto julga o **DESCRITOR** ja
aberto, antes do `exec`. O ultimo e o caso que mostra o desenho: o `fernando` **LE** `turnos.py` pela ACL,
e mesmo assim nao o roda como sonda. E o argv estrito devolve `rc 2` em `--sombra` sem arquivo, sem
argumento, com um `--settings=` colado e com dois arquivos.

**A ACL DE `logs/` E `app/docs/` JA EXISTIA, e o aval dizia que faltava.** Medido como `fernando`, antes de
tocar em nada: **NEGADO** em `.env`, `logs/.env_teste`, `logs/.senha_teste` e `backups/`; **LE** em `logs/`,
`app/docs/`, `app/docs/LEIS.md`, `app/ponto/turnos.py` e `bin/recursos.sh`. Nao se refez o que estava
feito. Depois do primeiro uso, o env-file nasceu **600 sem entrada de ACL** (`user::rw-`, `group::---`,
`other::---`) e segue **NEGADO** ao `fernando`; a trilha `logs/sonda_leitura.log` ele **LE**, e isso e
certo — ela nao carrega segredo, carrega quem/quando/banco/sha/rc.

**O QUE ESTA PORTA ESCREVE, declarado porque e escrita no banco do CLIENTE** (LEI-AKITA 7): cada chamada de
producao reafirma 5 linhas de catalogo em `saas_hasner` (`GRANT CONNECT`, `GRANT USAGE` x2 e
`ALTER DEFAULT PRIVILEGES` x4) e um `ALTER ROLE leitor ... PASSWORD` no cluster. O `GRANT SELECT ON ALL
TABLES` **so** sai quando o catalogo responde que falta — em `saas_hasner` a resposta e 0 e nao se escreve;
na `sombra` recem-restaurada e 111, e e para isso que o ponto de uso existe: `bin/sombra.sh:221-225` a
derruba toda noite com `pg_restore --no-privileges` e leva todo GRANT com ela.

`LEI-AKITA: origem=bin/sonda_leitura.sh (porta unica de leitura do dev), testemunha=catalogo do Postgres
(has_table_privilege / permission denied), RED=logs/o230/red_{a,b,e,dono_e_argv}_20261009.txt,
quem-mais-le=0 (porta nova, nenhum chamador de produto), juizes novos=0`

## R1-RESIDUO-DO-INTERVALO — **EM CURSO** (09/10 09:xx; complemento da O73, sem id novo)

**LEI RESPONDIDA às 13:3x — `L-115`, e a resposta não era nenhuma das minhas duas candidatas.**
A pergunta subiu aqui às 09:xx com número e a esteira seguiu (PAREI-DE-LEI-NAO-DEVOLVE-TURNO): *a L-084
diz que 180 min nas DUAS pontas é o limite para o cadastro ainda DESCREVER o dia; o raio de atribuição
da ata (`tol_min`) é 90. **Qual dos dois governa uma SAÍDA a 120 min do marco?*** A resposta dele:
**nenhum dos dois se move.** *"O raio de 90 e a L-084 ficam como estão. Dia com par de pausa FECHADO e
uma última batida depois dele: essa batida é a SAÍDA do turno pela posição, qualquer que seja o tipo
gravado; o que passa do marco é ponta de HE pela L-097. Altera a borda da BUG-144 só nesse caso; 'última
E solta' sem pausa fechada antes segue como está."* O que a **pausa fechada** faz na lei é o que eu não
tinha: ela é a testemunha de que a pessoa **saiu e voltou**, e depois disso a última batida não pode ser
uma entrada — então a POSIÇÃO responde onde o RAIO se cala, sem alargar envelope nenhum e sem juiz novo.
E o excedente não vira trabalho silencioso: vira **ponta de HE pela L-097**, lei que já existia para o
minuto fora do marco. Escrita em `docs/LEIS.md` como **L-115**, ESTADO `SO-NO-PAPEL`: os casos pela
REGRA entram na bateria **antes** do código (L-114), na fatia seguinte desta mesma obra. Medido:
**33 dia-colab / 103,1 h** das 500,7 h da competencia 10/2026 (balde A). Nesses dias a ata acende 3 dos
4 marcos, da o marco de SAIDA (`hf`) a batida do MEIO e a ultima batida fica **ORFA** por estar fora do
envelope de 90 min (a: 19:00 a 120 min de `hf` 17:00; b: 16:53 a 93 min de `hf` 15:20) — conservada pela
S133, invisivel para todo leitor de marco. As duas curas candidatas batem em lei declarada: alargar o
`tol_min` muda TODA ata da frota, e ler a `E` final como saida contradiz a borda da BUG-144
(`ponto/turnos.py:394`, *"a ultima E solta nao conta"*) = juiz novo. Prova e aritmetica em
`logs/r1/ata_a_b.out` e `logs/r1/MECANISMO.md`. **O balde A deixou de estar travado**: ele é a fatia
seguinte, com os casos na bateria primeiro; o balde P é o que pousa agora.


ORDEM-VIVA-TOPO passou a `O73`. O O214 fica ABERTO so pelo smoke dele (AVAIS #7) — os quatro itens
pousaram (`3c610491`..`6a259f0b`, no ar). O handoff ainda imprime "item EM CURSO: O214" porque
`_proximo_da_fila` le a ORDEM DA TABELA e o marcador e a autoridade desde a O158: divergencia
LEGITIMA, declarada no selo (`bin/tests/test_hook_nao_cobra_congelado.sh` diz as duas). A cura dessa
segunda voz e instrumento (`bin/handoff_sessao.sh`), e pousa sozinha pela L-105.

**MEDIDO ANTES DE CURAR, como o aval exige, e as partes somam 126 com ZERO "outro"**
(`logs/r1/MECANISMO.md`, `logs/r1/censo_126.out`, pela funcao REAL `turnos_do_colab` +
`_pares_marcados` + `realizado_do_dia`, so leitura):
`A-pausa-meio-absorvida 33 · G-celula-sem-marco 22 · P-par-interior-rejeitado 17 · D-par-sub-piso 15 ·
X-turno-aberto-sem-pausa 14 · M-cruza-meia-noite 10 · T-tipo-nao-alterna 9 · Z-zero-batida 3 ·
N-pareador-sem-defeito 3 = 126` (500,7 h, 46 colabs, competencia 10/2026).

**O MECANISMO NAO E UM** — a primeira redacao deste achado dizia que era, e estava ERRADA. Sao tres
sitios distintos, cada um a ORIGEM do seu balde: `_pares_marcados` (`ponto/turnos.py:513`) nega um par
cujas DUAS pontas vieram marcadas, porque testa tipo GRAVADO `S`->`E` (balde P, caso c col114: 492
contra 434); a porta PAUSA-DESLOCADA (`:966`) nao abre para a pausa 2 h fora do envelope de 90 min
porque exige vizinho de tipo GRAVADO `E` (balde A, caso b col941: 294 contra 467, 173 min num turno
solitario); e o desempate S158 entrega a batida a BORDA **por 14 segundos** (caso a col263: 370 contra
551, 180 min perdidos). O sitio NUNCA e o tipo gravado — batida gravada e fato, e corrigir tipo por
script e PROIBIDO pelo aval e pela ZONA INVIOLAVEL. O sitio e quem LE o tipo como se fosse papel.

**Tres dos 126 sao erro do TERMOMETRO, nao da casa**, e isso e a lei de 09/10 se provando no primeiro
uso: col949 29/09 pareia `00:28`->`06:00` = **332 min fechados e certos**, e a linha do oraculo traz
`piso 0`, `teto 0`, coluna `batidas` VAZIA e `dif=-332` — ele atribuiu a noite a outro dia. O 126 e
termometro; a certificacao e a BATERIA.

**FATO, sem reescrever o RED dele**: os casos d e e do aval nomeiam 2 e 4 batidas; prod tem **3 e 5**,
com um par sub-piso (1m28s em d, 9 min em e). Em `e` a alternancia pura da 296, nao 480 — o 480 so sai
se o par de 9 min for lido como marcacao duplicada, e disso NAO ha lei escrita. Os dois cenarios entram
na bateria nas DUAS formas (O218).


### A PROVA DEPOIS — NO AR as 13:49, e os quatro casos dao o numero DECLARADO em PROD

PROVA: commit `185b9af0` no ar as 13:49 (3 rotas 200/302/200, `importerror_500=0`); GRAVADO da comp 09
medido em prod antes e depois, **607 colabs `952550a8…`** e exportacoes vigentes `361d0f96…` /
`5c503b95…` / `84c78cd0…` (combinado `5d017af6…`), **identicos**; smoke da funcao real **4/4**
(`logs/r1/smoke_prod_20261009.txt`).

Exigencia 4 da DINHEIRO-EM-COMPETENCIA-ABERTA, fechada com os numeros:

- **commit `185b9af0`**, deploy `bin/deploy.sh --sem-migrate` **no mesmo ato** (L-107): migrations
  pendentes **0**, portao da sombra `dia=20261009 status=OK tipo=completa diverge=0 erros=0`, prova de
  casca **16 estaticos / 5 paginas / 608 rotas em 2 urlconf**, tres cascas recarregadas JUNTAS, tres
  rotas provadas (core `/health/` 200, ui `/colaboradores/` 302, mensageria `/health/` 200), selo
  BUG 128 verde, **`importerror_500=0`** na janela 12:49–13:49.
- **SMOKE EM PROD chamando a funcao REAL** `ponto.turnos.realizado_do_dia` -- nao uma sonda que
  reconstroi a chamada (`logs/r1/smoke_prod_20261009.txt`): **4 de 4** com o numero declarado na
  bateria -- col438 11/09 **360** (o achado do papel `X`, turno aberto), col518 05/09 **353** (o par
  guloso), col114 22/09 **434** (os 16 dias em que a ata chama a abertura de SAIDA), col853 28/08
  **135** (a ata deu `S` a abertura -> ADMITE). O caminho velho pagava 243 e 391 nos dois primeiros.
- **COMP 09 EXPORTADA INTACTA, hash ANTES e DEPOIS**: as **3 exportacoes vigentes** seguem
  `361d0f96…` (emp2, 210 linhas), `5c503b95…` (emp3, 86) e `84c78cd0…` (emp4, 9), **hash combinado
  `5d017af6…` identico** antes (13:44) e depois (13:49) do deploy; o `FechamentoMensal` da 09 tambem:
  **607 colabs, `952550a8…`** nas duas medicoes. As 5 linhas de registro invalidadas seguem
  invalidadas, nenhuma apagada.
- **A 10/2026 segue andando por conta propria**, como a dupla medicao de 13:43/13:48 ja mostrara:
  `cfe10382…` as 13:48 e `910d2ba3…` as 13:49. E por isso que a reversao dela se tira no instante, e
  por isso que ela e a competencia onde o numero se move.

### O DIFF DE FROTA, PUBLICADO ANTES DO APPLY (09/10 12:29, na sombra — IMPACTO, nao prova)

DINHEIRO-EM-COMPETENCIA-ABERTA pede os quatro, e o primeiro e este. Medido na SOMBRA pelas funcoes
REAIS (`turnos_do_colab` + `_pares_marcados` + `realizado_dos_turnos`), janela **2026-08-21..2026-10-08**,
**551 colaboradores / 26.999 dia-colab**, arvore base `ponto/turnos.py` md5 `ed186512…` (HEAD `6a259f0b`)
contra `f4058711…` (veto v2). Censo inteiro em `logs/r1/censo_impacto.out`:

**TOTAL −1.162 min (−19,37 h) em 55 dia-colab, 20 colaboradores.** Por competencia: **09/2026 29
dia-colab, −447 min**; **10/2026 26 dia-colab, −715 min**. **0 linha nova** (a v2 e estritamente mais
restritiva que a v1: ela admite um SUBCONJUNTO, entao o universo so pode encolher) e **0 delta deslocado**
nas que ficam — as que ficam mantem o numero digito a digito. Os pares latentes de turno aberto, que a v1
criava, foram de **14 a 0**.

**AS SEIS SAIDAS, nomeadas** (a predicao escrita ANTES, `logs/r1/predicao_censo_v2.txt`, afirmava DUAS
e o censo achou SEIS — a predicao esta FALSIFICADA no numero e e por isso que ela nao certifica nada):
col438 **−117** · col518 **+38** · col921 **−60** · col890 **+1** · col923 12/09 **+1** · col923 19/09
**+1**. As quatro ultimas sao as bordas que o arredondamento move; as duas primeiras sao os dois achados
de `logs/r1/achado_papel_x.md`, que a v2 nasceu para curar.

**O que o DIFF NAO e**: ele nao certifica. L-114 (abaixo) poe o nome em cada papel — o certificado e a
BATERIA, o DIFF e IMPACTO, o oraculo e TERMOMETRO.

### O CERTIFICADO: a BATERIA, com os quatro dias medidos escritos como CASO

`ponto/tests/test_r1_pausa_de_autoridade_mista.py` — **3 cenarios / 14 testes**, cada caso com cadastro,
batidas, ata e **a resposta da regra escrita ANTES do codigo** (L-110). O que ela cobra, e nenhuma delas
e geometria pura: **o MINUTO do dia** ao lado do par (`realizado_dos_turnos`, o leitor que PAGA) —
col438 11/09 vale **(360, 60)** onde a primeira forma pagava 243; col518 05/09 vale **(353, 60)** onde a
primeira forma pagava 391. As duas assercoes passaram de primeira, o que e a predicao do minuto se
confirmando. E o **contra-exemplo adversarial** (LEI-AKITA 5) guarda a ata FUTURA: `['E','S','Xi','Xi','S']`
tem de dar o par DECLARADO `14:00→15:00`, e da — **nao por regra nova**, mas pela passada `_usados` da
R2b (27/09, caso col843), que casa o par `_intra_declarado` ANTES do laco guloso. Se esse teste ficar
vermelho algum dia, a cura e ESTENDER aquela passada, nunca afrouxar o veto.

O selo do comportamento velho **nao foi apagado**: a assercao dele se inverteu e ele passou a morder a
VOLTA do defeito (`test_MORDE_ata_MUDA_o_MARCO_SOZINHO_nao_abre_pausa`).

**VEREDITO DAS SUITES** (`logs/r1/suite_veredito_20261009.txt`, os tres lidos pelo PAR que a secao 3 do
CLAUDE.md exige — `^(OK|FAILED)( |$)` **mais** `^Ran N tests`): modulo **Ran 14 / OK**; familia ponto
inteira **Ran 3163 / OK (skipped=7)**; vizinhos **Ran 7018 / OK (skipped=35)**. A nota no pe daquele
arquivo mostra a linha de PROSA de log (`OK — nenhuma divergencia em 2026-10-09`) que casaria o padrao
sozinha: foi ela que deu uma suite por verde em 08/10.

### L-114 CERTIFICADO x IMPACTO x TERMOMETRO — a lei entra no codigo do placar, sem obra nova

O corte das 09:0x pediu `L-NNN` em `LEIS.md` **e** mudanca em `core/placar_estrutural.py` no marco da
obra em curso. Os dois estao neste commit. O placar ganhou **papel** por resultado:

- **PRINCIPAL** — a **BATERIA** (o certificado, linha nova: 1 de 7 familias com bateria declarada, e a
  de turno/marcos tem **15 cenarios / 48 testes** verdes somando os dois modulos), a **SOMA** (linha nova:
  *as partes somam o total*, **invariante** com meta ZERO em producao — 197+29+208=434 na 09 e 95+15+95=205
  na 10), mais R2..R6 como estavam;
- **TERMOMETRO** — **so o R1**, a comparacao com o oraculo, porque so ela e pergunta de VALOR.

A **correcao 1** dele e exatamente o que eu ia escrever errado: eu desceria R1 **e** R4, lendo *"os numeros
de frota de R1 e R4"* ao pe da letra. R4 (*todo leitor da o mesmo numero*), R5 (idempotencia) e a soma das
partes sao **invariantes**: valem sobre dado sujo tambem, e por isso continuam no placar principal com meta
zero em prod. A **correcao 2** poe cadencia: `CADENCIA_TERMOMETRO_DIAS = 7`, e **bateria verde com
termometro vencido = AMARELO**. HOJE O PLACAR SAI AMARELO, e isso e o primeiro uso da lei contra a propria
casa: a ultima medicao do R1 e de **02/10 23:15** (`logs/e6_cauda2c/r1_dono_09_e_10.txt`), 7 dias — a semana
correu. O `e6_oraculo` da competencia aberta e o passo logo depois do pouso, pela propria cadencia.

O modulo **nao le relogio**: `placar(hoje)` recebe o dia de quem publica e, sem ele, o vencimento e `None`
— *nao perguntado* —, nunca `False`. Ausencia de sinal lida como sinal bom e o defeito que custou quatro
selos vazios em 01/09. **RED evidenciado** em `logs/r1/red_selo_l114_20261009.txt`: trocar o `>=` da borda
por `>` derruba 3 dos 9 casos de `core/tests/test_l114_placar_papel_e_termometro.py` — a borda, a cor do
veredito e a linha do ESTADO. A secao TERMOMETRO do ESTADO e render, mora em `bin/gerar_estado.py` e pousa
em ato PROPRIO de instrumento, depois do produto (L-105).

**E HOUVE UM SEGUNDO RED, que nao e meu: o selo da casa mordeu a primeira forma deste placar.** Eu havia
escrito `medido_em: '2026-10-02'` LITERAL no modulo, e a suite `core` inteira voltou
**`FAILED (failures=2)`** em `core/tests/test_selo_sem_data_cravada.py` (corte de 23/09) --
`core/placar_estrutural.py:107`, classe **DECIDE**, **-7 dias**: *"migre por ESTADO, nao empurre a data"*.
Ele estava certo e e a mesma classe do P0 de 21/09 (`api/credencial.py`, 304 pessoas sem autenticar as
00:00): o arquivo compara com `hoje`, entao a data decide, e **a lei que eu estava escrevendo seria a
primeira a envelhecer sozinha**. A cura foi na ORIGEM, nao em `DECLARADAS` -- que e a porta para data que
NAO decide: quando a medicao aconteceu e **fato do mundo**, entao o R1 declara `fonte_do_medido`
(`logs/e6_cauda2c/r1_dono_*.txt`, caminho e nao data), `placar(hoje, medido_em)` recebe a data de quem
publica -- que e quem faz o I/O --, e **sem data o veredito e AMARELO, nunca VERDE**. Depois da cura:
**Ran 16 / OK** nos dois modulos, e o censo da casa da **0** data em `core/placar_estrutural.py`, **0**
DECIDE no repo e **0** vencendo. Os dois REDs estao em `logs/r1/red_selo_l114_20261009.txt`.

### O ANTES DO GRAVADO, MEDIDO EM PROD — e a 09 nao se move em silencio

`logs/r1/gravado_quem_escreve.md` (56 linhas) e `logs/r1/hash_antes_deploy_20261009_1249.txt`. A
competencia **09/2026, EXPORTADA, esta INTACTA** em TRES medicoes da mesma funcao `_h` ao longo de ~19 h
(08/10 18:09, 08/10 19:11 e 09/10 12:49): **607 linhas de `FechamentoMensal`, `4da388d4…`** nas tres.
As **exportacoes** vigentes da 09 sao **TRES, uma por empresa** -- emp2 `361d0f96…` 210 linhas, emp3
`5c503b95…` 86, emp4 `84c78cd0…` 9, **305 linhas de TXT**, as tres com `invalidada_em=None` --, remedidas
as 13:44 e **identicas** as de 08/10 (O211b, `RELATO.md` mais abaixo). As outras 5 linhas de registro de
09/2026 estao INVALIDADAS pela porta, nunca apagadas, e e assim que a memoria de versao funciona.
*(Eu havia escrito aqui "21 exportacoes vigentes": era numero de outra conta. O registro da comp 09 tem
**8 linhas**, **3 vigentes**; medido agora, pk a pk.)* A UNICA linha de comp 09 gravada no meio disso (09/10
07:48:14) **moveu ZERO dos 24 campos de VALOR**, e o escritor tem NOME: `invalidar_previsto`
(`ponto/services/fechamento.py:965`, chamado so de `escala/signals.py:132`), que escreve `previsto_em` e
`atualizado_em` — e `previsto_em` nao esta em `CAMPOS`.

**A guarda e ESTRUTURAL, nao conduta**: `ponto/services/fechamento.py:72-82` chama
`empresas_exportadas_no_escopo(...)` e LEVANTA sem `permitir_exportada=True` + motivo escrito, com trilha
nominal — e as tres empresas tem exportacao vigente de 09/2026. Entao nenhum caminho recalcula a 09 em
silencio, e o deploy nao pode mover dinheiro exportado. A testemunha de quem pode escrever
`FechamentoMensal` nao e um grep meu: e `folha/tests/test_chokepoint_folha_gate.py`, familia
`folha/export`, **ALLOWLIST VAZIA**, por AST — porta unica `ponto/services/fechamento.py`.

**A REVERSAO FOI TIRADA AS 13:48:10, imediatamente antes do commit e do deploy** (exigencia 2 da
DINHEIRO-EM-COMPETENCIA-ABERTA): `logs/r1/reversao_comp10_20261009_1348.json` guarda o **GRAVADO** campo
a campo, pelos **24 campos de VALOR** do `CAMPOS` canonico (`ponto/management/commands/aplicar_09_corte_b.py:67`),
das DUAS competencias -- **10/2026: 587 colabs, `cfe10382…`** e **09/2026: 607 colabs, `952550a8…`** --,
mais os 8 registros de `ExportacaoDominio` da 09 com hash e estado.

**E A PROPRIA RETIRADA DUAS VEZES MEDIU A DIFERENCA ENTRE AS DUAS COMPETENCIAS**, sem sonda nova: a
mesma funcao rodou as **13:43:00** e as **13:48:10**, cinco minutos de intervalo, com a esteira so
escrevendo documento. A **10/2026 mudou de hash** (`7ea5dd89…` -> `cfe10382…`) e a **09/2026 NAO**
(`952550a8…` nas duas). E por isso que o "antes" da 10 nao se reusa e tem de ser tirado no instante do
apply, e e por isso que o "antes" da 09 vale desde 08/10: uma se move por conta propria, a outra esta
congelada pela L-092 e pela guarda estrutural. *(O arquivo das 13:43 foi REMOVIDO no ato: dois
"antes" da mesma competencia seriam dois escritores do mesmo estado, e o vigente e o de 13:48. O que
ele media -- o hash -- esta aqui.)* A reversao se executa **pela PORTA**
(`ponto/services/fechamento.py`), nunca por `UPDATE` cru, e a 09 ainda exigiria `permitir_exportada=True`
com motivo escrito: o arquivo e o VALOR de volta, nao uma licenca.

**A 10/2026 se move por conta propria**, e por isso o "antes" dela nao se reusa: **tres hashes diferentes**
nas tres medicoes (`127d6a83…` → `d6422334…` → `1bb6ec36…`, 587 linhas nas tres), **293 linhas** reescritas
desde 08/10 19:11, **81 na hora corrente**. `recalcular_fechamento` **nao tem cron** por desenho
(`config/crons.py:902`: *"quem decide QUANDO uma competencia se recalcula e o DP"*) — entao ela nao anda no
instante do deploy, e sim no proximo ato pela porta. Consequencia pratica, e e ela que manda no passo
seguinte: **o snapshot de reversao da 10 se tira NO INSTANTE do apply**, sobre as 587 linhas x 24 campos.
Nao atribuo as 293 linhas a um dos dois movedores possiveis — isso exigiria trilha por linha, que o modelo
nao guarda.

### O RELATO DESTRAVOU (aval dele de 08/10 19:2x)

`bin/relato_afirma_com_prova.py app/docs/RELATO.md` -> **rc=0, 0 afirmacao sem prova**. A linha que o
retinha era o titulo da O146 afirmando ato sem `PROVA:` ao lado, e a PROVA esta no lugar
(`docs/RELATO.md:480`: 353 `TipoEscala`, 0 com extra declarada, 124.358 celulas, 0 com
`dna['extra_declarada']`, `dna_versao` 1: 20.785 / 2: 103.573). Com rc=0 o proximo ciclo publica o RELATO
e a faixa **RELATO retido** do topo do ESTADO nao nasce — ela e escrita por `bin/relato.sh:84` **so**
quando o portao recusa.


## O214 ITEM 4 — **O DIA TEM DONO, E A DIFERENCA TEM DONO** (09/10 07:xx→08:xx, ITEM 4 FECHADO)

A ordem era literal: *"os dias acima do limite ficam decidiveis na Gestao de HE e o autorizado sai como
linha na pauta do TXT da 09 ja aberta, com os dois numeros; a 09 nao se recalcula por este ato"*. Entrou
assim, e o que ela obrigou a desenhar foi uma distincao que o sistema nao tinha palavra para dizer.

**`estado` x `dono`, e nao e um terceiro estado.** `estado` responde *o que foi decidido*
(`sem_decisao|autorizado|nao`); `dono` responde *quem decide*. Um dia `sem_decisao` do **SISTEMA** -- ponta
abaixo do `Empresa.limite_decisao_he_min` -- nunca esteve na fila do admin: quem o recusa e o
`recusar_ponta_pequena` do item 1. Soma-lo ao que "espera o admin" e o contador discordando do universo
(LEI-AKITA 8). Entao `gestao_he.DONOS = ('admin', 'sistema')` e `enriquecer` carimba o dono de cada dia.

**ZERO JUIZ NOVO, e o censo esta fechado.** A pergunta "de quem e este dia" tem **UMA** comparacao:
`ponto/portas/he.py::acima_do_limite(minutos, limite)` -> `int(minutos) > int(limite)`. Quem a CHAMA sao
exatamente tres: `ponto/services/gestao_he.py` (o dono na tela), `folha/porta_export.py` (a trava do item 3)
e a propria porta, em **duas guardas espelhadas** -- `:288` o sistema recusa o dia ACIMA, `:375` o admin
recusa o dia ABAIXO. `ponto/services/autorizacao_he_periodo.py` **nao decide**: le o cadastro e entrega o
`limite` a porta. O selo cobra isso por **AST** (2 chamadas na porta, `assertNotIn` no servico).

**`limite` virou OBRIGATORIO em `enriquecer`, sem default.** Default aqui seria o leitor inventando o
cadastro de quem esqueceu de perguntar -- e quem tem a empresa na mao e o chamador. Os cinco chamadores
migraram (2 views, 3 selos); os selos que medem OUTRA regra passaram `limite=0`, que e o estado honesto da
empresa sem cadastro: com zero nenhum dia e do sistema, e a guarda do dono nao morde a regra deles.

**`so_acima` nasceu ASSIMETRICO, tambem sem default.** `True` no ato de PERIODO (autorizar PAGA, entao so o
universo do admin entra); `False` no `recusar_em_lote` (ciencia nao move nada, entao os dois donos entram).
Um default deixaria um chamador distraido autorizar o dia que o sistema ja recusou.

**OS DOIS NUMEROS, cada um da SUA autoridade** (`_linha_para_o_dp`, marcador
`[HE-AUTORIZADA-EM-EXPORTADA]`). O que foi **PAGO** sai da LAVRATURA (`dia_pago.soma_do_periodo` +
`he_lavrada_do_colab`); o que **PASSA A VALER** sai do MOTOR (o mesmo `depois` que o admin viu na previa).
Misturar as fontes seria a testemunha recalculando (LEI-AKITA 2). **Silencio chega como ROTULO, nunca como
zero**: sem lavratura a linha diz *"sem apuracao ainda"*, porque "0,00 h" ali seria a AFIRMACAO de que o
colaborador nao tem HE. `he_lavrada_do_colab` nasceu em `dia_pago.py` por ter ganhado o SEGUNDO leitor
(L-111 ao contrario: o rotulo estava inline no laco de `enriquecer` e a linha do DP precisa do mesmo).

**UMA CABECA VIVA por empresa+competencia.** O segundo ato RESPONDE o primeiro (`pai=_cab`) em vez de abrir
pauta nova -- a ancora e `'%s:%04d-%02d'`, byte a byte a mesma de
`folha/management/commands/pre_fechamento.py:86`. O marcador e proprio, e **nao** `[PRE-FECHAMENTO]`: aquela
cabeca o cron reescreve toda noite.

**A 09 NAO SE RECALCULA, e isso se le no codigo, nao na promessa.** Nao ha `if exportada` que mude a
DECISAO: `decidir_he` grava na exportada do mesmo jeito que na aberta, e quem recusa mover o GRAVADO e o
`recalcular_por_evento` (L-092), com a lapide que ja estava la. O unico `if r['exportada']` do item 4
**escreve a linha** -- ele nao faz a decisao ser outra, faz a **diferenca ter dono** (REGEN-EM-EXPORTADA).

**A PREVIA NAO FALA COM O DP.** A pauta mora DENTRO do `_ato`, e `previa` e o `_ato` dentro de
`transaction.atomic()` + rollback: a linha morre com a transacao, por construcao -- nao por um `if previa`
que alguem esquece. Conferido antes de aplicar: `pautas.services.escrever` e **so banco** (`Pauta.objects
.create` + trilha + `lavrar`), sem FCM, sem arquivo -- entao o rollback e completo.

**E se a pauta falhar, o ato INTEIRO cai** -- inclusive as `DecisaoHE`. Esta e a escolha, por
CURA-MAIS-RESTRITIVA: autorizacao em competencia exportada **sem** a linha do DP e exatamente *"diferenca sem
dono"*, que e o que a REGEN-EM-EXPORTADA proibe.

**O SELO ESTAVA ERRADO E O CODIGO O CORRIGIU.** A primeira versao de `test_quem_DECIDE_o_dono_do_dia_CHAMA_o
_juiz_da_porta` exigia que `autorizacao_he_periodo` nomeasse `acima_do_limite` -- e o desenho certo nao
nomeia, porque esse servico nao decide. Satisfazer o selo seria o primeiro passo para uma segunda decisao.
O selo foi reescrito; o porque ficou no docstring dele.

**O RED que o banco de teste tinha e prod nao.** Os vizinhos deram **1 error** em
`test_o214_item2_autorizar_periodo.ExportadaTest`: `PautaRecusada: Voce nao pertence ao departamento ti.` --
`autor_sistema('ti')` recua para o primeiro superuser ATIVO, prod tem tres, banco virgem tem zero. Curado na
**fixture** (a forma que a casa ja usa, `test_pautas_do_esmeril.py:14`), nao com fallback: o ato cair sem o
superuser esta CERTO pelo paragrafo acima.

**O CARRIER CORTAVA EM SILENCIO, E O CORTE COMIA O PORQUE** (LEI-AKITA 6 -- bug provado no meio da fatia,
curado na hora; o bullet da lista NOMEADO, logo abaixo, dizia *"NAO CURADO"* e durou o tempo de medir).
`pautas/models.py:89` da a `Pauta.texto` **500** caracteres e `pautas/services.py:128` faz `texto[:500]`.
A linha do DP no pior caso mede **678** -- e os 178 que sobram sao EXATAMENTE a ultima linha: o texto
gravado terminava em `'Motivo '`, o rotulo cortado no meio da palavra. O DP receberia um pedido de
retificacao de folha **sem o porque**, que e justamente a trilha que a REGEN-EM-EXPORTADA exige por escrito.
RED literal contra o HEAD: `Ran 7 tests` / `FAILED (failures=1)`, com o `'Motivo '` na mensagem.

**A CURA E ORCAMENTO, E A ORDEM DO QUE CEDE E RECUPERABILIDADE -- nao gosto.** `escrever` ja tem o idioma
do orcamento para `ancora_tipo=='dia'` (`pautas/services.py:122`), e seguir o idioma existente e a
LEI-AKITA 4; alargar a coluna seria curar o carrier por causa de UM leitor. Quem cede, cede na ordem do que
o sistema **ainda sabe dizer depois**: **(1)** o *motivo*, porque `DecisaoHE.motivo` e `TextField` gravado
por DIA pela porta e o pedaco que fica aponta para la; **(2)** o *autor*, porque `DecisaoHE.decidida_por` e
FK e responde "quem assina" dia a dia; **(3)** a lista das outras rubricas por ULTIMO, e so para um
CONTADOR, nunca para o silencio. O que **nunca** cede: os dois numeros com as suas fontes, HE50/HE100, o
colab, a competencia, a contagem de dias e a linha da L-092. Se nem a forma minima couber, o ato **falha
INTEIRO** -- `DecisaoHERecusada` dentro do `atomic()` do `_ato` --, porque encolher abaixo disso e entregar
ao DP uma trilha que nao ensina ninguem (CURA-MAIS-RESTRITIVA).

**O teto se LE do campo**, `Pauta._meta.get_field('texto').max_length`: um `500` literal ali seria a
TERCEIRA copia do tamanho da coluna (LEI-AKITA 2). Medido em cinco formas de entrada --
**413 / 500 / 496 / 496 / 344**, todas `<= 500`, nenhuma silenciosa, todas com o rotulo do motivo, a linha da
L-092 e a informacao das movidas presentes. O ramo do `raise` e **inalcancavel com o cadastro de hoje**
(maior `username` ativo = **23** caracteres de 150 possiveis, em **727** usuarios, p95 **11**) e existe para
o dia em que alguem alargar o nome ou a frase: nesse dia ele diz o NUMERO em vez de cortar. `MIN_MOTIVO`
saiu de dentro de `_exigir_motivo` para `ponto/portas/he.py` porque o `>= 10` ganhou o **terceiro** leitor --
o piso do fragmento que ainda ensina alguem --, e a docstring da propria funcao previa a divergencia de duas
copias.

**O SEGUNDO DEFEITO ERA MEU, E O PORTAO O PEGOU.** O censo de chamadores de `enriquecer` que eu declarei
fechado tinha um fora: `chamados/tests/test_atalho_he_na_central.py` chamava sem o `limite` agora
obrigatorio, e o push foi recusado -- `ERROR: test_RED_o_numero_do_atalho_E_o_total_sem_decisao_da_tela`
sobre `Ran 10160 tests`. Curado com `limite=0`, que e o estado honesto para um selo que mede OUTRA regra:
com zero nenhum dia e do SISTEMA, a guarda do dono nao morde e a Central segue medindo a MESMA foto da tela.
Passar 15 ali faria o selo medir o recorte do dono, que e pergunta de outro selo. A licao esta no censo --
eu varri a familia do SITIO (`ponto/`) e o chamador morava na familia do CHAMADOR (`chamados/`).

**PROVA.** Selos do item 4: `Ran 12 tests` / **OK** (5 falhas + 3 errors no caminho, todos evidenciados).
Vizinhos `ponto folha pautas colaboradores`: `Ran 4240 tests` / 1 error -> fixture -> os quatro modulos do
O214 `Ran 82 tests` / **OK**. `ruff check` nos 9 arquivos: **All checks passed**. Juizes novos: **0**.
Rubrica nova: **0**. Escritor novo de `DecisaoHE`: **0** (segue `decidir_he`). **Da segunda janela** (as duas curas,
em copia nascida de `git show HEAD:`): RED evidenciado, depois `Ran 72 tests in 14.638s` / **OK** nos quatro
modulos (item 4 + chamados + item 2 + ponta pequena), `ruff check` **All checks passed** nos 4 arquivos, e os
vizinhos pela familia do CHAMADOR -- `ponto pautas chamados folha` -- `Ran 5988 tests in 586.954s` / **OK** (skipped=9).

**NOMEADO, NAO CURADO** (vai para o BACKLOG, nao para esta fatia):
- o filtro `?dono=` existe no servidor e **nao tem controle no markup** -- quem o liga e a **fatia 2** da
  Gestao de HE, que ele proibiu construir agora. Ate la e chave com leitor e sem gesto, declarada aqui para
  nao ser lida como chave sem leitor (LEI-AKITA 12);
- `int(getattr(empresa, 'limite_decisao_he_min', 0) or 0)` aparece **5 vezes**; candidato a unificacao;
- `_ato` devolve o `diff` por rubrica como RETORNO e **nao o persiste em lugar nenhum** (`_gravar` guarda
  estado/minutos/motivo). E por isso que a lista das outras rubricas e a unica das tres partes que cede para
  um contador: o motivo e o autor se recuperam na `DecisaoHE` do dia, o diff nao se recupera de ninguem;
- `pautas/services.py` tem o **500 literal** em `:122` e `:128` -- segunda e terceira copia do tamanho da
  coluna. A linha do DP ja le o teto do campo; o carrier ainda nao;
- os **tres campos de cadastro de HE** (`limite_decisao_he_min` e os dois da janela) **nao tem controle de UI
  nenhum**: nao existe `EmpresaForm` em lugar algum do repo. Hoje se cadastram por shell, o que e o pior tipo
  de cadastro pela LEI-AKITA 12 -- tem leitor, nao tem gesto;
- as duas metades de `UmJuizDoDonoDoDiaTest` que varrem `G` e `porta_export` medem por **TEXTO**, nao por
  AST; o resto do selo ja e AST (memoria: selo estrutural varre AST, nao texto);
- a ancora de competencia tem uma **terceira grafia**, sem zero a esquerda, em
  `chamados/services/competencia_trancada.py:205`. Pre-existente; nomeada, nao tocada.

**LEI-AKITA:** origem=`portas/he.py::autorizar_em_lote` (guarda espelho) + `autorizacao_he_periodo::_ato`
(linha DP), testemunha=`acima_do_limite` (dono) / `soma_do_periodo`+`he_lavrada_do_colab` (PAGO) / `_medir`
(VALE), RED=`test_o214_item4` 12 selos (5F+3E -> OK), quem-mais-le=`enriquecer` 2 views + 3 selos,
`itens_sem_decisao` 2 views, `autorizar_em_lote` 1 servico + 3 selos, juizes novos=0.

**LEI-AKITA (orcamento da linha do DP):** origem=`autorizacao_he_periodo::_linha_para_o_dp` (o orcamento
no PRODUTOR da linha, nao no carrier) + `ponto/portas/he.py::MIN_MOTIVO` (o piso num sitio so),
testemunha=`Pauta._meta.get_field('texto').max_length` (teto lido do campo) e
`DecisaoHE.motivo`/`decidida_por` (o que o ponteiro promete),
RED=`test_RED_10_a_linha_CABE_no_teto_da_pauta_e_o_motivo_NAO_desaparece` +
`test_RED_10_MORDE_o_caso_que_CABE_chega_INTEIRO_e_sem_marca_de_corte`, quem-mais-le=`escrever` (1 carrier,
as 2 copias do 500 nomeadas no BACKLOG) e os 5 chamadores de `enriquecer` (o 5o era o que faltava),
juizes novos=**0**.

### O ITEM 3 ESTA LIGADO (09/10 08:3x) -- e o numero nao era 70, era **81**

O item 3 e literal: *"liga depois do item 2 no ar, nunca antes"*. O item 2 esta no ar desde o deploy de
`a28ca8cf`, com prova de rota nas tres cascas -- entao a condicao se cumpriu, e **ligar e cadastro com
trilha** (CLAUDE.md 7b item 1), nao apply de dinheiro: a trava BARRA o `Gerar TXT`, nao move um centavo.

**O numero se LEU da autoridade, nao se recalculou** (LEI-AKITA 2 e 8): o carimbo de HOJE da porta
(`folha/porta_export.py::CHAVE == 'porta_export_leitores'`, escopo `empresa`, `data_ref=2026-10-09`,
falhas **0**), lido pelo `detalhe` como o `conferir` o le. Os **70** do paragrafo que esta linha substitui
eram de ANTES de duas curas desta manha -- a que conta por DIA acima do limite e a da L-097, que tirava
dia de outra competencia da conta --, e envelheceram em horas:

| empresa | dias acima do limite e SEM decisao (10/2026) | limite | antes | depois |
|---|---|---|---|---|
| emp2 | **62** | 15 min | `False` | `True` |
| emp3 | **17** | 15 min | `False` | `True` |
| emp4 | **2** | 15 min | `False` | `True` |
| **total** | **81** | | | |

**A TRILHA SE CONTOU NO BANCO, nao no `print` do script.** `core/observ.py::evento` e best-effort por
`engolir`: uma `acao` torta viraria rastro e eu teria um campo virado **sem trilha** -- LEI-AKITA 7 furada
pela minha propria mao. Entao o ato conta `LogAuditoria` antes e depois: **409 -> 412, delta +3**, cada
linha com `acao=editar`, `ator=sistema:shell`, `antes={'he_pendente_trava_export': False}`,
`depois={... True}` e o motivo escrito (trilha #668545/6/7). **Idempotente**: rodar o mesmo ato de novo
leu `antes=True` nas tres e gravou **delta +0** -- um escritor, uma linha por mudanca, zero duplicada
(contrato 2 da secao 4b).

**emp1 e as tres sem carimbo ficaram DESLIGADAS, e isso e leitura, nao esquecimento**: emp1 tem carimbo
com `universo=0` e emp20/21/29 nao tem carimbo nenhum -- nao ha TXT para travar. Ligar trava onde nao ha
porta do outro lado e exatamente o que `folha/porta_export.py:571-575` chama de *"paralisia com nome de
lei"*. Uniformizar e cadastro dele, pela tela.

**QUANDO MORDE:** o `conferir` le o DETALHE do carimbo, nunca o retorno vivo do `medir` -- entao a trava
passa a barrar no **proximo carimbo**, sobre estes 81. Reversao: o mesmo ato com `LIGAR = False`
(`logs/o214/flip_trava.py`), que devolve as tres a `False` e grava mais 3 linhas de trilha.

**O QUE FICA EM PE DA O214:** o **smoke do item 2** (AVAIS #7) e o **`!` do apply do item 1**
(`recusar_ponta_pequena --apply`, ensaio de 4.692 dias / 439,6 h, hashes em `logs/o214/hash_antes_09.txt`).

## O214 — **A TRAVA DO TXT CONTAVA DIA DE OUTRA COMPETENCIA** (09/10 05:5x, LEI-AKITA 6)

**Bug PROVADO no caminho do item 4, curado na hora.** Eu fui medir, na sombra, o numero com que o cadastro
`he_pendente_trava_export` seria ligado -- a pre-condicao escrita no item 3, *"o numero da trava se le do
carimbo, depois do deploy e ANTES do flip"* -- e a PROPRIA `folha/porta_export.py::medir` respondeu um numero
que nao era da competencia.

**MEDIDO pela funcao REAL** (`logs/sombra/vazamento_janela_he_pendente.py`, saida em `logs/vazamento_janela.out`;
competencia **10/2026**, janela 21/09..20/10, limite 15 min; a sonda **particiona a saida do `medir`**, nao
replica o laco):

| empresa | `medir` em | dia-colab com pendencia | TRAVARIAM o TXT | dentro da janela | **VAZADOS** |
|---|---|---|---|---|---|
| 2 J.A | 191,4 s | 2.098 (776 dentro / 1.322 fora) | 255 | 52 | **203** (68 de ago, 135 da 09) |
| 3 JSP | 140,4 s | 949 (337 dentro / 612 fora) | 68 | 16 | **52** (17 de ago, 35 da 09) |
| 4 G3 | 100,8 s | 106 (35 dentro / 71 fora) | 3 | 2 | **1** (de ago) |
| **total** | 432,6 s | **3.153** (1.148 dentro / **2.005 fora**) | **326** | **70** | **256** |

Entao **256 dos 326** dia-colab que travariam o TXT da **10** sao dias de **outra** competencia -- **86 de
agosto** e **170 da 09, que esta EXPORTADA** -- e **2.005 de 3.153 (63,6%)** das pendencias que o portao
publica nao sao da competencia que ele mede. **O numero real da trava e 70**, nao 326 e nao os 469 da
pre-medida de 04:5x (que era teto por outro universo, e esta declarada como teto no item 3).

**A ORIGEM, e ela ja tinha lapide.** `medir` percorria `esp['dias']` **inteiro**, e `espelho_do_colab`
devolve MAIS dias do que a janela pedida (medido no col87 em 01/10: **89 dias para uma competencia de 31**).
O `apurar` do retrato lavrado curou **este mesmo** vazamento em 01/10 -- *"de 5.767 dias, 4.198 estavam FORA
de 21/09..20/10"* -- e a lapide que ele deixou nomeia, por escrito, quem ainda leria errado: *"um numero
errado gravado, que o portao `he_pendente` e o contador da Central tambem leriam"*. Era **este** portao, e
ele ficou oito dias sem migrar. Band-aid seria filtrar no leitor; a cura vai no sitio que coleta.

**E O DANO NAO E INFLACAO, E PRISAO.** `gestao_he.estado_por_dia(ids, ini, fim)` so carrega decisao **dentro**
da janela, entao a chave de um dia vazado **nunca tem estado**: a trava o conta como "sem decisao" **mesmo
depois de alguem o decidir**, e nenhum gesto do admin o solta. Hoje sao **0 de 256** -- ninguem decidiu dia de
fora da janela ainda --, e **o item 4 e exatamente o que criaria os primeiros**, porque ele torna decidiveis
os dias da 09 (170 dos 256). A cura e, portanto, **pre-requisito do item 4**, nao so do flip.

**NAO HOUVE DANO EM PROD:** `he_pendente_trava_export` esta **False** nas quatro empresas, entao `falhas` nao
somava a lista e nenhum TXT foi barrado. O que o vazamento contaminava era o **numero publicado no carimbo** --
e era com ele que o flip ia ser decidido.

**TRES FORMAS DA MESMA PERGUNTA DENTRO DE UMA FUNCAO, agora UMA** (LEI-AKITA 2). "Este dia e da competencia?"
era respondida em `medir` de tres jeitos: o laco do `he_pendente` (**nenhum** -- o furo), a soma do `dif_topo`
comparando por **TEXTO** (`str(ini) <= str(d['data'])[:10] <= str(fim)`) e o `apurar` do modulo vizinho
comparando por **DATA**. A comparacao por texto e a que a lapide de `data_do_dia` proibe com nome: *"texto
compara certo em ISO e erra em qualquer outro formato, e o dia em que alguem mudar o formato o filtro passa a
aceitar tudo em silencio"*. As tres passam a chamar **`ponto/services/he_pendente_lavrado.py::data_do_dia`**,
que era `_data_do_dia` e ficou **publica porque ganhou o segundo leitor** -- o mesmo motivo pelo qual
`minutos_fora_do_dia` ficou publica no item 3. **Zero juiz novo** (o O214 proibe): a regra da janela e a de
`janela_fechamento`, e quem a aplica e uma funcao que ja existia.

**A COLETA VIROU FUNCAO PURA E NOMEADA**: `folha/porta_export.py::pendencias_he_da_janela(dias, ini, fim,
colab_id)`, chamada por `medir` em uma linha. Ela nao mudou a **FORMA** da entrada -- segue **por ponta**, com
`sentido`, `minutos_fora` da ponta e `minutos_do_dia` do dia (selo do B1, 30/09) --, e a razao de ser funcao
e o caso da **borda**: dentro de `medir` o dia 21 e o dia 20 so se exercitam com um colaborador que
`classificar_export` diga que ENTRA no TXT, o que custa codigo do Dominio, fechamento, celula e catalogo de
rubricas. Fora dela, o caso morde em 17 ms.

**RED PRIMEIRO, evidenciado** (`logs/o214item4/red_vazamento.out`): `Ran 8 tests` / **`FAILED (failures=6)`**
contra uma copia de HEAD com o laco de HEAD **sob o nome novo** -- isto e, o defeito sob teste, nao um
`ImportError`. Vermelhos: **o** (dia de agosto entra), **p** (dia da 09 exportada trava a 10), **q** (as duas
bordas), **r** (dia sem data legivel entra por omissao), **u** (AST: `str(ini)` na comparacao, 2 ocorrencias),
**v** (AST: DOIS sitios lendo `he_fora_da_janela`). Os dois verdes desde o RED -- **s** (segue por ponta com o
minuto do DIA) e **t** (dia sem ponta nao vira entrada) -- sao guardas de FORMA, e e de proposito que eles
passem no defeito: o que eles cobram e que a **cura** nao mude a forma. Verde depois: `Ran 22 tests` / `OK`
(os 14 do item 3 mais os 8 novos), 276 vizinhos OK, ruff limpo.

**CERTIFICADO NA SOMBRA PELA MESMA FUNCAO REAL** (`logs/o214item4/trava_curada.out`, arvore CURADA montada
por `SOMBRA_ARVORE`): `medir` em 196,9 + 138,3 + 102,2 s, **`TOTAL emp2+3+4: trava 70 em 1148 dia-colab com
pendencia`**, e **FORA da janela = 0** nas tres. Os numeros fecham com a particao da medida do defeito **sem
sobra**: 52+16+2 = **70** (os "dentro" de cada empresa) e 776+337+35 = **1.148** (o "dentro" do universo) --
`estado_por_dia` nao mudou, entao a cura tinha de reproduzir a particao exatamente, e reproduziu. `falhas = 0`
nas tres e a testemunha de IMPACTO do terceiro ato (a soma do `dif_topo` saindo de TEXTO para DATA): nenhuma
empresa ganhou falha de portao. **O flip do cadastro se decide sobre 70**, nao sobre 326 e nao sobre 469.

**UM SELO EXISTENTE FICOU VERMELHO SOBRE O CODIGO CERTO, e foi RE-APONTADO, nao apagado**:
`test_MORDE_o_contador_le_a_MESMA_fonte_que_a_TELA_risca` ancorava no **texto** de `inspect.getsource(medir)`,
e a cura moveu o laco para fora de `medir`. A pergunta dele sobrevive inteira -- "o contador le a mesma fonte
que a tela risca?" --, entao ele passa a ler o **coletor**, e a metade negativa ficou **mais forte** no mesmo
ato: `assertNotIn("r.get('dias_he_fora_da_janela')")` passa a varrer o **modulo inteiro**, nao mais so
`medir`. Ganhou tambem a linha que cobra a **delegacao** (`medir` tem de chamar `pendencias_he_da_janela`),
para que o laco nao possa desaparecer sem alarme. E a licao e a de sempre nesta casa: **selo ancorado em texto
de funcao de 400 linhas acusa o codigo certo no dia em que a cura move o laco**.

**LEI ANTES DO PATCH** (grep em `LEIS.md`/`CORTES.md`/`DOSSIES.md` antes de montar o patch): o sitio esta
protegido pela **L-097** -- *"o portao do export trava com HE pendente > 0: `folha/porta_export.py::medir`
ganha `he_pendente`, esperado 0"* --, e e justamente a clausula (2) dela que o vazamento tornava mentirosa:
"esperado 0" sobre um contador que inclui dia de agosto nao e portao, e numero. Ela nao muda de texto; o que
muda e o universo passar a ser o que ela sempre disse, **a competencia**. Tambem passam por aqui a **L-095**
(todo leitor le, ninguem recalcula -- o `medir` e contador, e por isso a janela se le do juiz) e a **L-003**
(zero numero sem medicao na fonte, que foi o que pegou o bug: medir chamando a funcao REAL). O `DOSSIES.md:362`
ja nomeava `folha/porta_export.py::medir::he_pendente` como sitio da familia.

`LEI-AKITA: origem=folha/porta_export.py::medir (o laco que coletava a pendencia sem a janela) + a terceira forma por TEXTO no dif_topo, testemunha=ponto/janelas.py::janela_fechamento aplicada por he_pendente_lavrado.py::data_do_dia (o MESMO juiz que o retrato lavrado usa), RED=folha/tests/test_o214_item3_trava_export.py casos o-v (6 de 8 vermelhos em logs/o214item4/red_vazamento.out), quem-mais-le=censo fechado -- `he_pendente` so e lido dentro de folha/porta_export.py (medir -> carimbo -> conferir) e por NENHUM template, `data_do_dia` por apurar e agora pelo portao, juizes novos=0 (L-097, L-095, L-003 citadas)`

## O214 item 3 — **A TRAVA DO TXT CONTA SO O DIA QUE E DO ADMIN** (09/10 04:5x)

Item da fila 1, na ordem dele. A lei, literal: *"**ITEM 3 TRAVA**: `colaboradores/models.py:97::he_pendente_trava_export` passa a contar **so dia ACIMA do limite e sem decisao** em `folha/porta_export.py::medir` (:468-478), e **liga depois do item 2 no ar, nunca antes**."* O item 2 esta no ar desde `041fd2ac`/`c32acf2e`, entao a condicao temporal esta cumprida.

**A PRE-MEDIDA, na sombra, ANTES de ligar nada** (`logs/sombra/medir_o214_item2.py`, competencia 10/2026, limite cadastrado 15 min nas quatro empresas). **E PRE-MEDIDA, e nao o numero da trava**, e a distincao e de UNIVERSO (LEI-AKITA 8): a sonda varreu o **retrato lavrado** -- *"colab com pendencia lavrada"* -- e replicou o predicado a mao, porque a funcao ainda nao existia; o `medir` alimenta `he_pendente` **so para quem `classificar_export` diz que ENTRA no TXT** (fica fora rescisao, sem codigo do Dominio, cadastro_zero) e le o **espelho vivo**, nao a foto da lavratura. Entao o 469 e **teto**, nao o numero: ele diz a ORDEM DE GRANDEZA do que a trava passaria a barrar.

| empresa | dias SEM decisao | ACIMA do limite (= travam) | abaixo (do SISTEMA) |
|---|---|---|---|
| 1 Confiance | 0 | **0** | 0 |
| 2 J.A | 399 (141 colabs) | **371** | 28 |
| 3 JSP | 103 (37 colabs) | **93** | 10 |
| 4 G3 | 7 (4 colabs) | **5** | 2 |
| **total** | **509** | **469** | **40** |

**O NUMERO DA TRAVA SE LE DO CARIMBO, depois do deploy e ANTES do flip**: `len(detalhe['he_pendente_trava_dias'])` do proximo `medir` em prod, que e a propria funcao julgando o proprio universo. Essa leitura e **pre-condicao do flip** do cadastro, nao um conferir depois dele. E o flip tem DOIS momentos, nao um: o `conferir` le `falhas` e `he_pendente_trava` do CARIMBO, entao ligar o cadastro so morde quando o `medir` seguinte rodar.

Os **40** dias de diferenca sao a razao de ser do item 3: eles estao abaixo do limite, o proprio sistema **se recusa a decidi-los** (`recusar_ponta_pequena` devolve *"e do ADMIN"* so acima do limite), e com a conta antiga -- `len(he_pendente)` -- eles travariam o TXT **sem porta do outro lado**. O cadastro segue DESLIGADO: este numero e o que a trava passaria a valer se ligada, e e por isso que ele se publica antes.

**O DEFEITO QUE ESTAVA NO CAMINHO, curado em passagem (LEI-AKITA 6 e 8).** A mensagem da recusa dizia *"HE fora da janela SEM decisao em %d dia(s)"* sobre `len(d['he_pendente'])`, que e uma entrada por **PONTA** desde 30/09 -- entao o dia com entrada **E** saida fora da janela se lia como **"2 dia(s)"**, e o **mesmo** `len` somava em `falhas`. Rotulo dizendo dia e conta contando ponta, no numero que vai no motivo da recusa do TXT. RED **g**.

**TRES SITIOS, ZERO JUIZ NOVO** (o O214 proibe um), e nenhum deles nasceu: os tres **ganharam o segundo leitor** e por isso ganharam nome.
1. **A soma do DIA** -- `ponto/services/he_pendente_lavrado.py::minutos_fora_do_dia`. O `apurar` ja a fazia inline; o `medir` precisa do **mesmo** total porque o limite se compara com o DIA, nao com a ponta (duas pontas de 8 min fazem 16, e com o limite em 15 o dia deixa de ser do sistema). Somar de novo no export seria a testemunha recalculando.
2. **O predicado do limite** -- `ponto/portas/he.py::acima_do_limite`. O `recusar_ponta_pequena` ja o tinha como `_min > _lim`; a trava pergunta a **mesma** coisa ("este dia e do admin?"). Ele e **uniforme inclusive em LIMITE 0**: zero desliga a recusa automatica do item 1, nao a fila do admin -- com limite 0 nenhum dia e pequeno, e `minutos > limite` ja diz isso sem caso especial. RED **h**.
3. **A decisao viva** -- `ponto/services/gestao_he.py::estado_por_dia`, que era `_estado_por_dia` e ficou **publica**. Uma terceira query ao `DecisaoHE` dentro do export nasceria com outra forma de chave (o lavrador indexa por `date`, esta funcao por `'YYYY-MM-DD'`) e com outra janela: as duas so discordariam **no dia da borda**, que e o dia que importa.

A conta em si e `folha/porta_export.py::dias_acima_do_limite_sem_decisao` -- funcao **pura**, que so desduplica por dia e ordena; ela chama os dois juizes acima e recebe os estados como **dicionario**, lido UMA vez fora do laco (e zero query quando nao ha pendencia). **`NAO` LIBERA**: a lapide do `DecisaoHE` diz que `NAO` **e** decisao, nao ausencia dela, e travar por um `nao` deixaria o admin com um gesto so para liberar o TXT -- autorizar. RED **c**.

**A lista vai ao CARIMBO** (`he_pendente_trava_dias`) porque o `conferir` le o `detalhe`, nunca o retorno vivo do `medir` -- fora de la a mensagem nao teria como nomear o dia que barrou. A entrada da pendencia segue **por ponta** com `sentido` (selo do B1, 30/09) e passa a carregar `minutos_do_dia` ao lado: `minutos_fora` e da ponta, `minutos_do_dia` e do dia, nenhuma no lugar da outra.

**RED PRIMEIRO, evidenciado**: `logs/o214item3/red_item3.out` -- `Ran 14 tests` / **`FAILED (failures=3, errors=10)`**, 13 vermelhos nomeados antes de uma linha de cura. Verde depois: `Ran 14 tests` / `OK`. Os **dois selos existentes que a cura tornaria mentirosos foram VIRADOS, nao apagados** (`folha/tests/test_b1_portao_he_nasce_desligado.py`): a conta do `falhas` passa a chamar a funcao REAL e ganhou as duas metades novas (dia pequeno nao trava, dia com `nao` nao trava), e a assercao da mensagem passa a cobrar a chave nova. O `test_n` cobra a **procedencia** do valor por AST, nao por texto: `assertIn("'minutos_do_dia'")` ficaria verde com `'minutos_do_dia': _hf.get('minutos')` -- a chave do dia com o numero da ponta, que e exatamente o erro que o item 3 existe para nao cometer.

`LEI-AKITA: origem=folha/porta_export.py::medir (a conta do portao) + ponto/portas/he.py (o predicado) + ponto/services/he_pendente_lavrado.py (a soma do dia), testemunha=DecisaoHE via gestao_he.estado_por_dia e o retrato de he_fora_da_janela que a TELA risca, RED=folha/tests/test_o214_item3_trava_export.py (14 casos, 13 vermelhos em logs/o214item3/red_item3.out), quem-mais-le=censo fechado -- `he_pendente` so e lido por folha/porta_export.py (7 sitios) e por nenhum template; `estado_por_dia` por gestao_he (2) e agora pelo export, juizes novos=0 (tres funcoes nomeadas sobre regra que ja existia, nenhuma com regra propria)`

**O QUE O SMOKE DE PROD VAI MOSTRAR, dito ANTES para ninguem ler silencio como defeito**: o `conferir` le o carimbo, e o carimbo de hoje **nao tem** a chave `he_pendente_trava_dias` -- entao a linha de HE fica **ausente** da mensagem ate o proximo `medir`. Com o cadastro desligado ela ficaria ausente de todo jeito. O smoke e *"a rota responde e nao ha 500"*; a linha reaparece quando o carimbo for refeito.

**ACHADO LATERAL DA MESMA MEDICAO, nomeado e nao curado** (a fatia esta no BACKLOG): o `confirmar` de 14 dias custa **5,32 s**, dos quais **5,12 s sao os 14 callbacks** de `recalcular_por_evento` no `on_commit` -- um `recalcular_fechamento_mes` **por dia**, todos da MESMA competencia do MESMO colab. O ato em si leva 0,20 s. O sobrecusto do caminho real contra a previa (0,30 s) e de **5,02 s**: e este o numero da decisao sincrono-x-job, e a cura e desduplicar por (colab, competencia) dentro de `recalcular_por_evento`, nao na tela.

---

## O214 item 2 — **UM ATO, UM MOTIVO, E A TELA MOSTRA O DIFF ANTES DE CONFIRMAR** (09/10 03:xx)

Item da fila 1, pela ordem de 08/10 19:5x. A lei, literal: *"ITEM 2 AUTORIZAR POR COLABORADOR E PERIODO: um
ato, um motivo, e a tela mostra **ANTES de confirmar** as horas que entram por rubrica (o DIFF daquele
colab); grava uma `DecisaoHE` por dia."* **21 selos `a-p` verdes** (`Ran 21 tests` / `OK`), 97 nos vizinhos
nomeados, e a **suite inteira da copia** -- `Ran 10126 tests` -- que achou DOIS vermelhos que os
vizinhos nao viam, os dois meus e os dois curados na ORIGEM (abaixo). **Zero juiz novo, zero rubrica nova, zero escritor novo de
`DecisaoHE`**: a porta `ponto/portas/he.py::decidir_he` segue sendo chamada dia a dia, como o lote de recusa
ja faz, e as 7 rubricas tem UMA declaracao -- elas nasceram no `diff_janela_he.py`, mudaram de casa
para `ponto/services/autorizacao_he_periodo.py::RUBRICAS` e o comando passou a LE-LAS, porque duas
tuplas significariam a proxima rubrica entrando numa e nao na outra.

**A PREVIA SAI DO MOTOR REAL, DENTRO DE TRANSACAO DESFEITA.** `ponto/services/autorizacao_he_periodo.py`
grava as N `DecisaoHE` pela porta, roda `autoridade_do_periodo` antes e depois, diffa as 7 rubricas e
`raise` no fim. Nao e a regra simulada por fora — isso seria a sonda propria que o CLAUDE.md secao 6
proibe, e e a mesma lapide do `diff_janela_he`. **A tela nao soma nada**: o `mostrado` viaja no form em
**minutos inteiros**, serializado canonicamente, e o confirmar **RECUSA** quando o gravado difere do
mostrado — tela que mentiu nao confirma. O RED **m** prova que a recusa chega ao admin em PALAVRA, e a
guarda vive no SERVICO, e nao na view, que e por isso que ela tambem cobre POST forjado.

**O QUE A LEITURA DO VIVO MUDOU NO CONTRATO, antes de uma linha de codigo** (os cinco achados estao em
`DOSSIES.md` secao 8, arquivo SEM teto, nunca aqui — a licao de 08/10):
1. **`decidir_he` TROCA decisao existente.** O contrato dizia *"a porta devolve `no_op` e nao sobrescreve
   humano"* e **e falso**: `no_op` so acontece com o estado IGUAL; estado diferente cai em `_gravar` com
   `_trocada=True` e reescreve tudo. Virar um `nao` HUMANO em `sim` moveria dinheiro sem ninguem pedir, e o
   guarda "mostrado == gravado" **nao pega** (a previa flipa igual, os dois lados batem). A exclusao e do
   **LOTE**, cumprida por `autorizar_em_lote` pela lapide do `_gravar` — **nao um parametro novo na porta**
   (seria juiz novo) nem um filtro na view (seria a regra escondida na tela). RED **c**.
2. **Competencia EXPORTADA nao e exclusao de escrita.** O RED **h** dizia "o ato nao entra, porta propria",
   e o vivo responde o contrario: `decidir_he` **grava** e quem recusa e `recalcular_por_evento`, com a
   lapide *"nao e falha -- e a lei funcionando"*. Recusar no ato de PERIODO o que o ato de UM DIA aceita
   seria um **segundo juiz da mesma pergunta** (LEI-AKITA 2). Mesma conduta nas duas portas.
3. **A previa nao roda o motor N vezes**: `recalcular_por_evento` agenda em `on_commit`, e na previa os N
   callbacks sao **descartados**. Custo ~2 `autoridade_do_periodo` + N inserts baratos.
4. **ACHADO LATERAL, NOMEADO E NAO CURADO** (L-009, regra de negocio fora do pedido): `recusar_em_lote`
   tem o buraco espelhado **em producao** — `ponto/views.py:559` filtra `sem_decisao` so no gesto "todos",
   e a marcacao por CAIXA vai crua do form, entao um `sim` humano pode virar `nao` em lote por form velho.
5. **No CONFIRMAR sao N `recalcular_fechamento_mes` da MESMA competencia do mesmo colab**, redundantes e
   pos-commit. Nao e dano (idempotente); e numero, e a dedup por (colab, competencia) pertence a
   `recalcular_por_evento` — origem, e fora deste pedido.

**OS k-p FICARAM VERMELHOS POR FIXTURE, E O QUE ELA ENSINOU VALE MAIS QUE O VERDE.** Nenhuma linha de view,
url ou template mudou para passar. Duas causas, as duas medidas:
- **a grade do espelho sai da `CelulaDia.ata`, nunca do `dna`**: `leitor_celula.py::grade_da_celula` chama
  `_celulas_da_ata`, que monta celulas e regua a partir de `ata['lampadas']`. Celula com DNA e ata NULA
  devolve 0 celula e regua vazia -> `marcar_pontas_fora` sem o que marcar -> `he_fora_da_janela` vazio ->
  retrato VAZIO -> e a tela respondia, **com razao**, que nao havia dia SEM DECISAO. A fixture passa a pedir
  a lavratura **pela PORTA** (`cartorio.julgar_celula(forcar=True)`, o juiz do cron das 06:28 e do signal),
  nunca escrevendo `ata=` a mao: fixture que grava ata e um SEGUNDO escritor. Ela precisa pedir porque
  `bulk_create` nao dispara signal e `on_commit` nao roda dentro de `TestCase`; em prod a ata ja existe.
  O dia IGUAL a hoje volta `None` do cartorio — teto temporal, o turno 07-19 nao terminou.
- **o ator nao tinha `ver_folha`**: o ato gravava e o redirect para `/ponto/gestao-he/` dava **404**
  (`views.py::gestao_he` levanta `Http404` sem ela). Em prod quem autoriza chega pelo BOTAO daquela tela;
  ator com `autorizar_he` e sem `ver_folha` nao existe. **A tela nao ganhou gate** — a permissao do ato
  segue so na porta, um juiz.

`LEI-AKITA: origem=ponto/portas/he.py + a tela da Gestao de HE, testemunha=autoridade_do_periodo (as 7
rubricas do diff_janela_he), RED=a-p (21, verdes), quem-mais-le=censo fechado em DOSSIES secao 8,
juizes novos=0`

FALTA, e esta nomeado: **smoke de clique do Ronald** nas duas cascas (FRONT SEM SMOKE NAO SOBE — a fatia
toca template), e a **medicao do custo do confirmar** na sombra (N recalculos), que e o numero em que a
decisao sincrono-x-job se apoia.


### O QUE A SUITE INTEIRA ACHOU, e os vizinhos nao (09/10 03:2x -- `Ran 10126 tests` / `FAILED (failures=2)`)

Os 21 selos do item 2 e os 97 dos seis modulos vizinhos estavam VERDES, e a suite cheia ficou **vermelha em
dois**. Os dois eram meus, nenhum dos dois estava nos vizinhos que eu escolhi, e e por isso que a suite cheia
nao e cerimonia: **eu escolhi os vizinhos pelo assunto, e estes dois cobram FORMA** -- um contrato de arvore
e um selo de tela de outra fatia.

1. **`test_contract_no_except_pass` morde `except: pass` em arquivo NOVO**, e o meu
   `autorizacao_he_periodo.py` tinha um: `raise _Rollback()` / `except _Rollback: pass`, lendo `_out` de uma
   atribuicao anterior ao `raise`. Nada era silenciado ali -- a excecao e propria e o rollback e o ato --,
   **e a proibicao esta certa mesmo assim**: handler vazio obriga quem le a descobrir, pelo fluxo, que a
   medicao sobreviveu a transacao desfeita. Cura na ORIGEM, nao na allowlist: a `_Rollback` passou a
   **carregar a medicao** (`.medido`), e o handler faz o que handler faz -- `_out = _desfeita.medido`. O dado
   anda pelo caminho declarado, e a allowlist do contrato segue do mesmo tamanho.
2. **`test_tela_gestao_he_forma_b::test_MORDE_o_expandido_NAO_repete_rotulo_por_linha`**: `15 != 13`
   campos `name="motivo"` ocultos. O numero 15 esta CERTO -- a linha da pessoa ganhou um segundo ato, e ele
   leva o seu motivo --, e **a cura nao foi trocar 13 por 15**. Somar os dois atos num numero so era o
   defeito do selo: o total cresce quando um ato novo nasce e **nao diz qual deles mudou**, que e exatamente
   o sinal que se perde. Ele passou a contar **por ATO**, separando os forms pela `action`: um motivo por DIA
   sem decisao (13), um por PESSOA com dia sem decisao (2), e **nunca dois no mesmo form** -- este ultimo e
   novo e morde o caso em que o segundo campo venceria no POST sem ninguem saber qual o dialogo preencheu.

Depois das duas curas: `Ran 59 tests` / **`OK`** nos quatro modulos envolvidos (`logs/o214item2/cura1.out`),
ruff limpo, e a suite cheia **relancada**: `Ran 10126 tests in 1362.734s` / **`OK (skipped=42)`**
(`logs/o214item2/suite_cheia2.out`, 03:32-03:55). O veredito que autoriza o pouso e **esse**, nao o dos
vizinhos -- e a frase "suite da copia verde" so entrou neste RELATO depois dele existir.

### O contrato, escrito ANTES do codigo (09/10 01:2x — a parte que a L-110 exige)

Enquanto o ensaio da sombra da O146 corre (o portao e CEGO entre 00:00 e 04:00, CLAUDE.md secao 2, por
isso `--refazer --dump-agora` + `--bloco`), o proximo item da fila 1 **nao ficou esperando**: o contrato
de entrada, o desenho, os **REDs a-h** e o PROIBIDO da **O214 item 2** estao escritos **pela regra, antes
do codigo** (L-110), em `app/docs/DOSSIES.md` **secao 8** — arquivo **sem teto**, e nao aqui, pela licao de
08/10 (o dossie da O146 teve de ser recuperado do transcrito porque o ponteiro apontava para prosa que a
DIETA apagou). **Nada construido**: a LEI 10 exige copia nascida do HEAD **no ato do patch**, e o HEAD muda
no commit da O146.

O achado que o contrato traz, e ele muda o desenho: **a recusa de `autorizar_em_lote` nao proibe lote.**
Ela diz, literal (`ponto/portas/he.py:272`), *"dinheiro em lote exige DIFF publicado ANTES -- que e um ATO
de esteira, nao um clique de tela"*. A lei do item 2 **cumpre** essa condicao em vez de dispensa-la (a tela
mostra o DIFF antes de confirmar), entao a revogacao e do **TAMANHO DO ATO** — um dia passa a um
colaborador num periodo —, nunca da condicao: **varios colaboradores e empresa inteira continuam
levantando**. E a PREVIA sai do motor REAL dentro de transacao desfeita, pelo molde que
`ponto/management/commands/diff_janela_he.py` ja usa, com as **7 rubricas que ele declara** — zero rubrica
nova, zero escritor novo de `DecisaoHE` (a porta segue sendo chamada dia a dia, como `recusar_em_lote` faz).


## O146 — **FECHADA, NO AR**: a extra declarada da escala desloca o limite, e o limite
tem UM sitio (09/10 00:4x · `cfd4ff83`+`ce212bb8`, deploy 01:35, smoke OK)

**PROVA:** gravado medido em prod 09/10 10:0x (so leitura, agregado): `TipoEscala` **353**, com extra declarada (`he_extra_antes_min>0 | he_extra_depois_min>0`) **0**; `CelulaDia` **124.358**, com `dna['extra_declarada']` **0**; `dna_versao` gravado **1: 20.785 · 2: 103.573** (nenhuma 3 -- a impressao digital do cartorio nao andou, como o item 4 prometeu). O sitio unico esta NO AR e o IMPACTO de hoje e **zero cadastro**: o limite se desloca quando alguem declarar a extra, e ninguem declarou ainda.

Fatia da fila 1, pela ordem de 08/10 19:5x (`O145 -> O146 -> item 2 da O214 -> itens 3 e 4`), contra o
dossie que mora em `app/docs/DOSSIES.md` secao 7. A lei que a governa e a resposta dele de 08/10 21:5x
(`O146-EXTRA-E-SO-HE`): **a extra alarga o que se PODE ganhar, nunca o que se DEVE cumprir.**

**OS 4 ITENS DE `MUDA`, cada um com o sitio:**

1. **CADASTRO.** `escala/models.py:78` `he_extra_antes_min` e `:83` `he_extra_depois_min` —
   `PositiveSmallIntegerField`, **default 0**, migration `escala/0042_o146_he_extra_declarada.py`. Entram
   **pelo WIZARD** (`escala/views_wizard.py:40-41` e `:240`, `escala/services/cadastro_tipo.py:255`,
   `templates/escala/wizard_tipo_escala.html` + `_wizard_preview.html`). **`permite_hora_extra` nao foi
   reusado** — e booleano e tem outro uso (`ponto/triagem_batida.py:293`,
   `ponto/management/commands/processar_alertas_turno.py:129`), os dois intactos.
2. **DNA.** `gerar_celulas.py::montar_dna:123` congela `dna['extra_declarada'] = {'antes': N, 'depois': M}`
   — chave de **topo** e emitida **so quando declarada**, para que a impressao digital do cartorio
   (`ponto/services/cartorio.py:85::impressao_insumos`, que digere `dna_versao` + `marcos`) e as duas
   comparacoes de dict inteiro (`tripwire_celulas.py:30`, `ponto/portas/celula.py:496`) **nao se movam em
   dia que nao declara nada**. `DNA_VERSAO` segue **2**.
3. **UMA FUNCAO DESLOCA O LIMITE.** `ponto/janela_he.py:54::minutos_fora_da_extra` — uma subtracao, um
   sitio. Lida por `entrada_efetiva:76`, `saida_efetiva:117` e `marcar_pontas_fora:156`, e por cima dela
   o motor (`motor_calculo_v2.py:1363` e `:1401`, pelo `j['extra_antes']`/`j['extra_depois']` de
   `_janela_do_dia:1303`). **Nenhum leitor com conta propria** — e por isso que a comparacao `> piso` ja
   havia virado `dentro_da_janela:41` em 29/09.
4. **MINUTO DENTRO DA EXTRA = HE AUTORIZADA PELA ESCALA**, sem `DecisaoHE`. Fora dela, **ponta**, e a
   L-097 segue inteira: o miudo que sobra continua bloqueado e cai no item 1 da O214.

**QUEM PERGUNTA A EXTRA, e por que nao e o motor que le o cadastro:** o juiz e
`EscalaColaborador.extra_declarada_do_dia` (`escala/models.py:1183`), **irmao do `intervalo_do_dia`** e com
o contrato dele palavra por palavra — `celulas` e ALIMENTACAO, **chave ausente = "nao ha celula nesse dia",
e dia sem celula le o template vivo** (`TipoEscala.extra_declarada_do_dia:310`). O motor **pergunta**
(`motor_calculo_v2.py:1154::_extra_declarada`, memoizado por dia, injetado em `:1677`); ler
`te.he_extra_antes_min` de dentro do motor seria o motor com cadastro proprio, e o passado reescrito por
troca de template.

**OS CASOS, pela regra ANTES do codigo (L-110).** `ponto/tests/test_o146_extra_declarada_da_escala.py` —
**26 casos, `Ran 26 tests` / `OK`**. A **suite INTEIRA na copia**, pela porta unica
(`bin/suite.sh --dir`): **`Ran 10105 tests in 1363.127s`** / **`OK (skipped=42)`** — mais os 143
vizinhos do modulo e `ruff` limpo nos 10 `.py` tocados.


| caso | o que a regra manda | teste |
|---|---|---|
| **a** | saida em marco+127 com extra 120 → **120 de HE, ponta de 7** | `test_a_...127_com_extra_120_da_120_de_HE_e_ponta_de_7` (+ 2 do teto: dentro nao acusa, acima **ainda** acusa CADASTRO x REALIDADE) |
| **b** | saida em marco+60 com extra 120 → **60 de HE, ponta 0** | `test_b_...da_60_de_HE_e_ponta_zero` |
| **c** | saida **no marco** com extra 120 → 0 de HE e **NAO e saida antecipada** | `test_c_...nao_da_HE_e_NAO_e_saida_antecipada` |
| **d** | entrada 10 min antes **sem** extra → ponta de 10, **como hoje** | `test_d_...e_ponta_de_10_como_hoje` |
| **e** | extra antes 60: entrada em marco−60 → 60 de HE; marco−70 → 60 **+ ponta de 10** | `test_e_...` e `test_e2_...` |
| **f** | escala **sem** declaracao → **identica a hoje** | `test_f_escala_SEM_declaracao_e_IDENTICA_a_hoje` |
| **g** | dia com `DecisaoHE` autorizado → tudo conta, como hoje | `test_g_...conta_TUDO_como_hoje` |
| **h** | turno que **cruza a meia-noite** com extra depois | `test_h_extra_depois_no_turno_que_cruza_a_meia_noite` |
| **i** | rodar 2x = **mesmo estado** | `test_i_...` e `test_i2_marcar_pontas_fora_2x_...nao_duplica_pendencia` |
| **j** | **MORDE**: motor e `marcar_pontas_fora` dao o **MESMO** numero no caso **a** | `test_j_MORDE_motor_e_marcar_pontas_fora_dao_o_MESMO_numero` |
| **k** | *(meu, do contrato do irmao)* celula que EXISTE e nao declara → **ZERO, nunca o template vivo**; dia **sem** celula → template vivo; e a alimentacao com **0 query** no laco | `test_k_...`, `test_k2_...`, `test_k3_a_alimentacao_tem_o_contrato_do_irmao` |

Mais os selos do dossie: **previsto NAO cresce** com a extra (a clausula 1 da lei, com caso que morde a
volta), a extra entra **pela PORTA do wizard** e **nao reusa** `permite_hora_extra`, o default e **ZERO**,
a chave do DNA **nasce so quando declarada**, a impressao do cartorio **nao muda sem declaracao**, **a
conta mora em UM sitio so** e **os dois leitores IMPORTAM a funcao do dono** (por AST).

**O RED que vale mais do que os outros**, porque e o que distingue cura de silencio: o montador do espelho
ficou **`AssertionError: 7 != 67`** com o CONTROLE verde ao lado (dia sem declaracao segue riscando os 67).
Vermelho com controle verde quer dizer *"falta uma subtracao"*; vermelho sozinho quer dizer *"o montador
esta mudo"*, e e esse o engano que a familia de vacuidade de 01/09 cobra.

**IMPACTO DE FROTA = 0 h, por UNIVERSO VAZIO — e medido na fonte, nao por amostra.** Sonda
`logs/sombra/universo_o146.py` pela porta `bin/sombra.sh --rodar` (`logs/o146/universo.out`, 09/10 00:48):

```
colunas novas presentes no esquema de prod: []     (nascem na 0042)
TipoEscala cadastrados: 350                        (o denominador de quem PODERIA declarar)
emp2 12.990 celulas / 8.327 de trabalho  |  emp3 3.434 / 1.901  |  emp4 600 / 352
TOTAL 17.024 celulas, 10.580 dia-colab de TRABALHO, com a chave `extra_declarada` = 0
```

As duas fontes do juiz dao vazio ao mesmo tempo: **0 celulas** carregam a chave e as **duas colunas nao
existem** no esquema de prod, logo nenhuma linha pode ter numero nelas — o unico escritor delas e o wizard,
que **nasce nesta fatia**. Entao o deslocamento vale **0 min em 10.580 de 10.580** dia-colab da competencia
10, e `minutos_fora_da_extra(x, 0) == x` e a **identidade** (auditada linha a linha nas duas efetivas: com
`extra_min=0`, `sobra == bruto`, `marco − timedelta(0) == marco`, e o `max(0.0, …)` sobre bruto **negativo**
— chegada atrasada, saida antecipada — cai no **mesmo ramo** de antes, porque `dentro_da_janela` compara
`<= piso` e `piso >= 0`). **Nao ha apply de dinheiro nesta fatia.** Os 10.580 contra os 10.578 medidos em
08/10 sao a diferenca de FONTE (sombra do dia x prod naquele dia), nao de regra.

**POR QUE O DIFF NAO SE MEDE NA SOMBRA COM O CODIGO NOVO, e isso e lei da casa:** `bin/sombra.sh` **nunca
migra a sombra** — ele so COMPARA `django_migrations` prod x sombra. A sombra tem o esquema de PROD, sem as
duas colunas, e qualquer consulta a `TipoEscala` com o codigo novo quebraria ali. A pergunta certa para o
esquema de hoje e a que a sonda faz: *as colunas existem?* Nao existem — e e isso que fecha o universo.

**QUEM MAIS LE, censo fechado (o `quem-mais-le` do LEI-AKITA do aval):**
- `ponto/services/espelho.py:728-736` — montador, **passa a extra** pelo mesmo juiz;
- `colaboradores/services/calendario.py:434-437` — idem, com a carga de celulas **icada** para servir os
  dois consumidores do mes numa consulta so;
- `ponto/services/he_pendente_lavrado.py:53-98` — chama `espelho_do_colab` e le `d['he_fora_da_janela']`:
  **herda**, sem linha nova;
- `recusar_ponta_pequena` — documenta que le `marcar_pontas_fora` *"pelo montador do espelho"*: **herda**;
- `ponto/services/espelho.py::pontas_do_relato` — **deliberadamente NAO recebe a extra**: ele TRADUZ a
  linha publicada pelo motor, cujo `minutos_fora` ja e a sobra. Passar a extra ali subtrairia duas vezes;
- `motor_calculo_v2.py:2725` (`Motor12x36ComEscala._entrada_efetiva`) — **herda** pelo `_janela_do_dia`,
  sem copia;
- `_acusa_cadastro_x_realidade` — **nao muda**: o `cadastrado` segue sendo o marco **CRU**, porque
  `pontas_do_relato` casa por `it['cadastrado'] != marco_da_celula`; deslocar ali deixaria a cura da O145
  **muda exatamente nos dias que declaram extra**. O `minutos_fora` ja chega como sobra.
- **O pre-julgamento da L-084 fica nos marcos CRUS**, e isso e da propria L-084: ela pergunta se o
  **cadastro descreve o dia** (3 h nas duas pontas). Deslocar o marco antes dela deixaria a extra decidir
  se a L-084 se aplica.

**TRES COISAS QUE EU VI E NAO CUREI, porque curar aqui seria regra de negocio fora do pedido:**
1. declarar extra num template torna as celulas existentes **STALE** sob `tripwire_celulas.py:30` ate o
   `regenerar_celulas_vinculo` — **igual a editar `hora_fim`**, e e o desenho (a celula e soberana);
2. com `piso > 0`, o minuto que fica no piso **alem** da extra vira HE — mas a L-097 fixou **piso 0** nas
   tres empresas, entao hoje isso nao alcanca ninguem;
3. os dois leitores de `permite_hora_extra` ("batida tardia") seguem inalterados.

`LEI-AKITA: origem=ponto/janela_he.py:54 (o limite da janela) + TipoEscala/DNA (cadastro), testemunha=a
celula (dna.extra_declarada) lida pelo motor E por marcar_pontas_fora, RED=casos a-k + 10 selos,
quem-mais-le=censo fechado acima (7 leitores, 2 herdam, 1 recusado com motivo), juizes novos=0`

**FALTA, nomeado e nao escondido:** a **LINHA HAIKU** do dossie — contador *"extra da escala"* no payload
do copiloto com rotulo de admin e o golden *"quanto de HE da escala o colab X tem no dia Y"*, degrau
**leitura**. Ela mora na stack `mensageria/nucleo/ferramentas.py`, que tem deploy proprio, e **nao esta no
`PRONTO` do dossie**; fica na celula da O146 como a PORTA-RETRATAR-BATIDA ficou com a dela.

---

## LEI RESPONDIDA — **A EXTRA DECLARADA E SO HE** (pergunta 08/10 20:0x, resposta dele 08/10 21:5x)

A pergunta foi ao topo com o numero e **a esteira seguiu** (PAREI-DE-LEI-NAO-DEVOLVE-TURNO): no intervalo
a O145 foi medida, curada e provada, e a O204 pousou no ar. **Nao houve PAREI.** A resposta chegou com a
O145 em suite, que e exatamente o desenho da lei de 30/09.

**O aval, literal** (`O146-EXTRA-E-SO-HE`):

> *"a extra declarada na escala e hora extra, nao entra na jornada prevista do dia. Quem sai no marco nao
> deve nada e nao tem saida antecipada; quem fica ate o fim da extra recebe a HE sem precisar de
> autorizacao. segue a fila; PAREI so em lei ou !"*

**O que ela decide, com o denominador que eu havia medido antes de perguntar** (`CelulaDia` na janela de
`periodo_apuracao(10, 2026, corte_da_empresa)`, ORM puro, sem motor): 351 `TipoEscala` cadastrados, 559
vinculos ativos, **10.578 dia-colab de TRABALHO na competencia 10** (emp2 8.325 · emp3 1.901 · emp4 352).
Em nenhum desses dias o `minutos_previstos_do_dia` cresce com a extra declarada. A leitura que eu havia
escrito nos casos **c** e **f** do dossie era a certa, e nenhuma linha de codigo nasceu sob a outra.

**As tres clausulas, separadas porque cada uma mora num juiz diferente** — e e isso que a O146 vai
construir:
1. **PREVISTO não muda.** `escala/utils.py::minutos_previstos_do_dia` (e a grade que o materializa) segue
   lendo so os marcos do DNA. A extra declarada **nao** entra na jornada prevista do dia.
2. **PONTUALIDADE se mede contra o MARCO, nao contra o fim da extra.** Quem sai no marco **nao deve nada e
   nao tem saida antecipada** — a extra nao desloca o marco de saida para o lado do desconto. (Nao confundir
   com a L-084, que e outra coisa: la o dia inteiro esta em outro horario.)
3. **A JANELA se desloca, e so para o lado de CIMA.** O minuto trabalhado dentro da extra declarada e **HE
   autorizada pela ESCALA**, sem `DecisaoHE` e sem pendencia — e esse e o unico sentido em que o numero novo
   do cadastro e lido. Fora dela, a L-097 segue inteira.

Em uma frase: **o cadastro da extra alarga o que se PODE ganhar, nunca o que se DEVE cumprir.** A funcao
unica do item 3 da O146 (`ponto/janela_he.py`, lida pelo motor **e** por `marcar_pontas_fora`) nasce com
este sinal, e o selo dela tem de morder a volta: previsto que cresce com a extra = VERMELHO.

---

## AVAL REGISTRADO — **`GEO-SILENCIO-DE-PING`: O APARELHO QUE PARA DE PINGAR PASSA A AVISAR** (08/10 22:5x)

Complemento da **O-GEO-DECISAO**, sem id novo, e ele mesmo diz onde entra: raia `wt-bos`, **depois da O198 e
antes da O197**; a principal (O145 → O146 → item 2 da O214) **nao muda**. **Registrado no mesmo turno**
(*"Registra no BACKLOG agora, executa na vez"*), com o contrato inteiro na coluna da obra. **Nada
construido** -- a vez dele e depois da O198, e a raia esta na O200.

**O que ele NAO descongela, e isso e da propria ordem:** a O-GEO-DECISAO segue **CONGELADA pela L-096**, e o
aval abre o complemento dizendo literalmente *"Nao toca dinheiro nem celula (L-096 intacta)"*. Um vigia que
so manda push nao julga celula nem move centavo -- por isso ele anda sem a obra descongelar, e o PROIBIDO
dele fecha as portas que fariam o contrario: **barrar batida, abrir chamado por silencio, retratar ou julgar
celula**.

**O contrato ja vem com as tres coisas que esta casa costuma pagar depois:**
- **X e Y nascem de MEDIDA, nao de palpite** -- *"X e Y saem dessa medida, nao de palpite"*: turnos dos
  ultimos 14 dias por faixa de ping (0 · 1-3 · 4+) e colabs que ja pingaram alguma vez. E cadastro **por
  empresa, pela UI, com nome e leitor** (LEI-AKITA 12), com **default DESLIGADO** (0 = nao avisa), e
  `cravar X ou Y no codigo` esta na lista PROIBIDO.
- **O vigia compara INSTANTE, nunca data** -- que e exatamente a conduta de 08/08 na secao 6 do CLAUDE.md
  (teto por DATA nao basta: no cross-meia-noite o DIA acaba antes do TURNO), e o **RED f** a morde.
- **Papel DECLARADO em `config/crons.py`: VIGIA, so alarma** (secao 4a), com contador de nome, numero,
  esperado e dono, e a consulta em **LOTE, O(n), sem N+1**.

**REDs a-g** (a: X+1 min → 1 push ao colab, e rodar 2x continua 1 · b: X+Y+1 → 1 push a supervisao, so 1 ·
c: ping fecha o episodio e um silencio novo gera push novo, **MORDE** · d: quem nunca pingou, nenhum push ·
e: turno fechado ou isento, nenhum push · f: cruza a meia-noite, conta pelo instante · g: cadastro 0, nada
dispara). **SELO**: leitor de "sem ping" com regra propria fora de `ponto/presenca.py` = 0; chave nova sem
leitor = 0. **SMOKE**: na sombra o envio de push esta desligado, entao o caso real e em prod, com o aparelho
dele (col677), e o resultado vem para ca.

**O que eu ainda NAO conferi, e fica dito em vez de suposto:** os cinco leitores do `quem-mais-le`
(`painel_op.py`, `calendario.py`, `ponto/views.py:2900`, `api_ping_geo`) nao foram grepados, e o
`PingGeo`/`classificar_presenca_turno` nao foram lidos ao vivo. **O censo se fecha na vez** -- afirmar agora
seria narrar codigo de memoria, que e a conduta que esta casa paga mais caro.

---

## MARCO FECHADO — **O POUSO ESTA NO REMOTO, E O DOSSIE DA O146 APONTAVA PARA NADA** (08/10 23:4x)

**O push do marco passou, e o veredito se leu no REMOTO, nao no log:** `git fetch` + `git log origin/main -1`
devolve `d29b7f14`, o `logs/push_marco_o200.out` fecha em `d2cf6606..d29b7f14  main -> main`, e
`git log origin/main..HEAD` esta **vazio**. As duas pistas do pre-push: suite **10079 OK (skipped=42)** em
750 s e control-plane **22 OK** em 108 s. Um push por MARCO (L-108), nao um por commit.

**ACHADO DO MARCO, e e um ponteiro para nada.** O dossie da O146 -- os **REDs a-j** escritos pela regra
ANTES do codigo (L-110), que o PRONTO do proprio dossie exige nomeados -- era citado em **tres** sitios
(`BACKLOG.md` no bloco OBRAS, a celula O146, `PROMPTS.md:1098`) todos dizendo *"no RELATO"*, e **nao existia
em arquivo nenhum do repo**: o RELATO vivo so guardou o resumo do aval, e
`grep -c 'extra declarada' docs/RELATO-ARQUIVO.md` = **0** -- a DIETA (L-109) arquivou a prosa e os
ponteiros ficaram apontando para o vazio. Eu nao re-derivei de cabeca a lista que o Ronald escreveu: ela
foi recuperada do transcrito da sessao, onde esta citada palavra por palavra, e **pousou em
`app/docs/DOSSIES.md` secao 7** -- o arquivo que a propria abertura declara *"autoridade de leitura, nao
resumo"* e que **nao tem teto**. Os dois ponteiros do BACKLOG passaram a citar a secao. Se eu tivesse
comecado a O146 pelo codigo, teria construido 10 casos de memoria contra 10 casos escritos pela REGRA, que
e precisamente o que a L-110 proibe.

**AS TRES CURAS DE DOCS QUE ESPERAVAM O PUSH.** Nao se escreve na arvore durante o push -- os selos de host
rodam dentro do pre-push e a arvore carimbada mudaria --, entao as tres ficaram prontas em copia e entraram
depois do veredito. (1) As celulas de estado da O145 e da O200 passam a ABRIR com `**NO AR**`: o leitor
unico (`bin/hook_stop_fila1.py:55`) exige a palavra LOGO depois do `**`, e `**CURADA E NO AR**` nao casa --
item pousado vinha sendo cobrado como fila 1 viva, e o hook me devolveu `siga: O145` duas vezes por isso.
(2) A frase errada das 06:00 (acima). (3) As duas chegadas repetidas ganharam linha no `PROMPTS.md`.
PROVA: celula da O145 **278** caracteres e da O200 **281**, as duas abaixo do teto de 300 da L-109, as duas
casando `^\*\*(FECHADA|FECHADO|NO AR|no ar)\b` -- a mesma regex do `_FECHADO` -- e nenhuma com `|` no texto;
`git log origin/main..HEAD` vazio quando a primeira escrita aconteceu.

---

## O200 — **POUSOU E ESTA NO AR: O SITIO UNICO DO RAIO RESPONDE EM PROD** (08/10 23:1x)
PROVA: `logs/deploy.stamp` traz `COMMIT=40be6f22...`, o objeto que o amend absorveu, e
`git diff 40be6f22 HEAD --name-only` devolve **um** arquivo -- `app/docs/RELATO.md` --, entao o
CODIGO no ar e o deste commit; `raio_efetivo_m` e `raio_do_posto`
importam no ar com `RAIO_PADRAO_M=200`; vigia `{'universo': 208, 'n': 12, 'invalidos': 0,
'teto_metros': 500}`; raio `<= 0` = **0** nos 208 ativos e nos 226 do total; deploy com 606 rotas
e tres rotas provadas, `importerror_500=0`.

**O POUSO FOI UM ATO**, como a L-107 cobra: `git merge --no-commit` -> docs do marco por PATH ->
commit do merge (o objeto `40be6f22`, que o amend seguinte absorveu) ->
`bin/deploy.sh --sem-migrate`, sem nada no meio. O deploy respondeu
`migrations pendentes no schema do cliente: 0`, `sombra: carimbo dia=20261008 status=OK
tipo=completa diverge=0`, prova de casca com **16 estaticos, 5 paginas e 606 rotas em 2 urlconf**,
as tres cascas recarregadas juntas e as tres rotas provadas (`/health/` 200, `/colaboradores/` 302,
mensageria 200), com `importerror_500=0` na janela.

**PROVA EM PROD, lendo o codigo que esta no ar** (so leitura, `tenant_command shell`):

| o que | resposta de prod |
|---|---|
| commit no ar | `40be6f22...` no `logs/deploy.stamp` — o objeto absorvido pelo amend, cujo diff contra o HEAD e **so** `app/docs/RELATO.md` |
| sitio unico | `raio_efetivo_m` e `raio_do_posto` importam; `RAIO_PADRAO_M=200` |
| vigia | `{'fonte': 'geofence_raios', 'universo': 208, 'n': 12, 'invalidos': 0, 'teto_metros': 500}` |
| raio invalido | **0** postos com `raio <= 0` e **0** com raio NULO — nos 208 ativos **e** nos 226 do total |
| raio largo | **2** postos acima do teto de 500 m (linha propria da pauta, nao somada ao resto) |
| amostra | posto#1 `raio_metros=200` -> `raio_do_posto()=200` |

**O DEPLOY DE RECONCILIACAO FOI RECUSADO POR UM PORTAO, e eu NAO o forcei.** Depois do amend eu
rodei `bin/deploy.sh --sem-migrate` de novo com UM proposito -- fazer o `logs/deploy.stamp` nomear o
HEAD final em vez do objeto absorvido. O `bin/janela_auth.sh` BARROU, as 23:23: este marco toca
`app/api/views.py`, que e' sitio de auth, e a lei ACESSO-NUNCA-EM-LOTE (item 4) proibe auth no ar
entre 23:20 e 06:00 -- o P0 de 20/09 comecou a meia-noite e levou os ~750 ao login (82% de 401 as
00h, 100% as 20-23h). Ha a porta de emergencia (`SEM_JANELA_AUTH_MOTIVO`), e usa-la para arrumar um
CAMPO DE TEXTO de stamp seria exatamente o atalho que a L-009 poe na lista NUNCA PRE-APROVADO: o
portao nao estava errado, o meu motivo e' que nao era emergencia. O codigo que o cliente usa ja
subiu as 23:1x, DENTRO da janela, e e' o mesmo -- medido, nao suposto: um unico arquivo de diff, e
ele e' documentacao. O `bin/tests/test_import_tardio_contra_o_ar.sh`, que le esse campo, segue
VERDE (`no_ar=40be6f22 imports_tardios=4285 acusados=0`). **FICA PARA QUEM VIER, e a primeira versao desta linha estava ERRADA.**
Eu escrevi *"o proximo deploy que tocar auth so passa as 06:00"* e so DEPOIS medi a base do portao:
`bin/janela_auth.sh:21` e' `BASE="${1:-origin/main}"` e a :74 roda
`git diff --name-only "$BASE"...HEAD` -- ele pergunta o que esta ADIANTE DO REMOTO, nunca o que o
stamp diz. Com este marco empurrado, `origin/main == HEAD`, o diff fica VAZIO e o portao LIBERA na
hora; as **06:00** so prendem deploy que leve arquivo de auth AINDA NAO empurrado. A frase velha
faria a proxima sessao esperar seis horas por nada, e ela nao cabia em `--amend` (o commit ja estava
no push) nem em commit so de docs (L-106): viajou no commit da O146.

**ESTA SECAO NAO ESCREVE O HASH DO PROPRIO COMMIT, e o motivo custou duas vezes neste turno.**
Um commit nao pode nomear a si mesmo, e cada `--amend` mata o hash que o anterior publicou: primeiro
`f512ec9f` (citado em quatro lugares do RELATO, na celula do BACKLOG, na linha do PROMPTS e dentro da
propria mensagem do commit), depois `40be6f22`, que o `logs/deploy.stamp` tinha acabado de gravar como
"o que esta no ar". O codigo nao mudou em nenhuma das duas -- so `docs/RELATO.md` --, mas um stamp
apontando para objeto que so o reflog alcanca e' uma TESTEMUNHA MENTINDO sobre o que roda em prod, e
`bin/tests/test_import_tardio_contra_o_ar.sh` le exatamente esse campo. Cura de forma, nao de texto: o
documento cita o que e' ESTAVEL (a `6c53bc46` da raia, que e' pai do merge, e o estado "merge do
pouso"), e a coerencia do stamp se faz pelo **deploy que roda DEPOIS do commit final** -- nunca por um
hash digitado a mao.

**OS DOIS DENOMINADORES SAO OS DOIS, de proposito**: o vigia mede **208 ativos** (e dele que sai a
pauta do admin) e o censo do complemento mediu **226 postos**, o total com os 18 inativos. Nos dois
o raio invalido da **0**, entao o numero nao muda de dono com a escolha do universo -- mas eles ficam
dos dois lados escritos, porque "0 de 208" e "0 de 226" sao contas diferentes e a casa ja pagou por
rotulo que nao diz qual universo mediu.

**O DEFEITO CONTINUA LATENTE, e isso nao e' a mesma coisa que inexistente**: nao ha posto com raio 0
HOJE, e bastava um admin digitar 0 na tela para o juiz acusar `ponto_fora` em toda batida de GPS bom
daquele posto. A tela agora recusa, e o vigia conta. Nada foi corrigido por script -- o que havia a
corrigir (os 2 postos acima do teto e os 24 colaboradores sem cerca) vai pela **pauta**, que e o que
a ordem manda.

**SMOKE que falta e e' dele**: o pino do `/painel/` muda de cor para quem tinha leitura imprecisa --
`smoke Ronald: abrir /painel/ e conferir que o pino de quem esta no posto com GPS impreciso nao
aparece mais vermelho`. A fatia nao tocou `static/js/`, service worker nem template base, entao ela
nao cai na trava do FRONT SEM SMOKE; o pedido e' de conferencia, nao de portao.

---

## O200 COMPLEMENTO — **RAIO ZERO QUERIA DIZER DUAS COISAS OPOSTAS, E A CARA ERA ACUSAR QUEM ESTAVA NO LUGAR** (08/10 22:0x→22:5x)

Aval literal, mesma obra, mesmo pouso, sem id novo (`O200-COMPLEMENTO-RAIO-ZERO`). A O200 fechou com o
raio registrado como **linha de fila** -- "zero vitima nao e bug provado no caminho". Ele desfez isso no
mesmo pouso, e estava certo: o numero media o CADASTRO DE HOJE, nao o codigo.

**O DEFEITO.** `Posto.raio_metros` valia duas coisas opostas dentro do mesmo sistema. **Nove** leitores
escreviam `raio_metros or 200` -- para eles 0 e *"posto sem raio declarado"*, e cai no padrao. **Dois**
liam a coluna CRUA -- o escritor do alerta (`ponto/services/geofence.py::verificar_geofence`) e
`reconciliar_geofence` -- e para eles 0 e *"cerca de zero metro"*, isto e, **toda batida fora**. Nesse
caminho o juiz abre `ponto_fora`, dispara `push_supervisao` e nasce chamado **em cima de quem bateu no
lugar certo, com GPS bom**. Nenhum dos onze tinha bug proprio: e a **MEIA-CORRECAO** da secao 6 do
CLAUDE.md -- cada escritor certo, o conjunto mentindo, e pelo lado mais caro.

**MEDIR ANTES** (prod, SO LEITURA, como a ordem manda): **226** postos ativos · `raio_metros <= 0` = **0**
· nulo = **0** (a coluna e NOT NULL, medido por `IntegrityError`) · colabs em posto de raio invalido = **0**
· `AlertaGeofence ponto_fora` nascido neles = **0 de 5.289**. Menor raio vivo = 49 m; 136 dos 226 em 200.
**O defeito e LATENTE**, e a ordem ja decidia isso por escrito -- *"se for 0, o defeito e latente e a cura
segue igual"*. Bastava um admin digitar 0 na tela. **Nada foi corrigido por script**: o que houver a
corrigir vai pela pauta do admin, que e o que a ordem manda.

**A CURA, DE ORIGEM.** O raio que VALE sai de **UM sitio** -- `ponto/services/geofence.py::raio_efetivo_m`
/ `::raio_do_posto` -- e as **15 chamadas** medidas na arvore curada leem dele. Nao e fallback: e o
`or 200` deixando de ser codigo repetido onze vezes e virando uma **frase com nome** -- raio ausente,
ilegivel ou `<= 0` e ausencia de **CADASTRO**, e ausencia cai no `RAIO_PADRAO_M` declarado. Entram no
sitio unico tambem os dois que so **MOSTRAM** (`services/detalhe.py` e o relatorio de
`detectar_vinculo_divergente`): imprimir a coluna crua ao lado de uma acusacao medida contra 200 e a mesma
mentira, so mais barata. O chamado passa a gravar em `contexto_json` o raio **EFETIVO**, porque
`chamados/services/acoes_chamado.py` imprime esse campo como "raio permitido". O cadastro
(`colaboradores/views.py`) **RECUSA** raio `<= 0` dizendo o que o zero provoca e nomeando o padrao. O
vigia `geofence_raios.py` passa a **CONTAR** raio `<= 0`, com **linha propria** na pauta
(`postos_com_raio_invalido`) -- nunca somado dentro de "sem cerca util", que esconderia justamente o
cadastro que faz o juiz ACUSAR.

**OS QUATRO REDs DO AVAL, NOMEADOS, medidos contra a arvore da O200 SEM o complemento: 3 de 4
VERMELHOS.** Esse baseline nao tem hash para citar -- o amend o absorveu em **`6c53bc46`** --, entao
ele se nomeia pelo ESTADO, e nao por um objeto que so o reflog alcanca.

| caso | o que o aval pede | resultado na O200 sem o complemento |
|---|---|---|
| **(a)** | posto raio 0, batida a 50 m com accuracy 10 → **nao** acusa `ponto_fora` | **RED**: `'ponto_fora' != 'dentro'` |
| **(b)** | posto raio 100, batida a 300 m com accuracy 10 → `ponto_fora`, **como hoje** | **verde (MORDE)** — e o controle |
| **(c)** | salvar posto com raio 0 e com −5 pela tela → recusado com mensagem | **RED**: `a tela GRAVOU raio 0` |
| **(d)** | o pino da O200 e o juiz dao a **mesma** resposta em (a) e (b) | **verde antes e depois** — ver abaixo |
| *(vigia)* | contar raio invalido | **RED**: `None != 1` |

**O (d) PASSOU NA ARVORE VELHA, e isso esta escrito no selo em vez de ajustado na assercao.** Fui ao sitio:
a propria O200 acabara de fazer `_geo_do_pino` chamar `classificar_posicao(dist, acc, posto.raio_metros)`
**CRU**, "como o juiz o le" -- entao pino e juiz **ja concordavam**, os dois acusando. O (d) nao prova o
defeito; ele **guarda a cura pela METADE**: normalizar o raio so no juiz, ou so no pino, separaria os dois
leitores outra vez. Caso verde com poder de morder vale; caso verde sem poder de morder e selo vazio
(a familia do 01/09).

**TRES DEFEITOS MEUS apareceram no primeiro GREEN, e os tres foram curados na ORIGEM, nao na assercao:**
1. **um ramo inalcancavel** em `views.py` -- eu havia escrito "raio em branco = padrao", mas `raio_metros`
   esta em `CAMPOS_OBRIGATORIOS_POSTO` e `_faltando_obrigatorios_posto` ja recusa o campo vazio **antes**.
   Era um **segundo escritor** da pergunta "o que significa raio ausente" (LEI-AKITA 7). Apagado; o teste
   virou `test_MORDE_c_a_AUSENCIA_tem_um_dono_so_e_nao_e_esta_guarda`, que cobra a frase
   `Preencha os campos obrigatorios: Raio.`
2. **o selo estourava em vez de morder** -- o caso NAO-MORDE carregava `%%` e o `ast.parse` levantava
   `SyntaxError`, entao ele passava por **ERROR** sem afirmar nada. Selo que estoura nao e selo que morde.
3. **um teste que fabricava um mundo proibido pelo schema** -- `update(raio_metros=None)` da
   `IntegrityError`. O `is None` do vigia e defensivo, nao um estado alcancavel; o teste virou
   `test_MORDE_raio_zero_e_invalido_e_NAO_e_raio_largo`.

**SELO** `core/tests/test_selo_raio_de_um_sitio.py`: varredura por **AST** (texto fez o selo morder a
propria prosa que explica a cura 5 vezes nesta casa) sobre todo `.py` da arvore, acusando `BoolOp(Or)` cuja
esquerda LE `raio_metros`. **Zero allowlist**, nem para `geofence.py`. Duas excecoes por **FORMA**,
declaradas e mordidas nos dois sentidos: esquerda `Compare` (pergunta, nao valor -- e a linha do vigia) e
direita string literal (o traco de registro sem numero). Medido na arvore curada: **0 achados**.

`LEI-AKITA: origem=ponto/services/geofence.py::raio_efetivo_m, testemunha=RAIO_PADRAO_M + a coluna
Posto.raio_metros, RED=colaboradores/tests/test_o200c_raio_de_um_sitio.py (3 de 4 vermelhos medidos na
O200 sem o complemento) + core/tests/test_selo_raio_de_um_sitio.py, quem-mais-le=censo fechado nos dois sentidos (11
decidiam, 15 chamam o sitio unico; o vigia, o `or '-'` de acoes_chamado e o form ficam de fora,
declarados), juizes novos=0 -- raio_efetivo_m e a regra que o `or 200` ja afirmava nove vezes, agora dita
uma.`

Tudo isso entrou **no mesmo commit da O200**, como a ordem pede: o amend levou `f512ec9f` a
**`6c53bc46`** (20 arquivos, raia nao empurrada -- o hash velho fica so no reflog).
Esta secao do RELATO viaja no ato do **pouso**, porque o RELATO tem um escritor so -- a arvore principal --
e `6c53bc46` nao carrega `docs/RELATO.md`.

**A SUITE DA COPIA: 10.071 TESTES, 2 VERMELHOS, OS DOIS MEUS E NOMEADOS.** `Ran 10071 tests in 1352s`
com `FAILED (failures=2, skipped=42)` -- e nenhum dos dois toca a cura. (1) `test_ruff_zero`:
`patch.py:4:8 F401 py_compile imported but unused`; (2) `test_so_o_juiz_resolve_o_posto_de_referencia`:
`extras = ['patch.py']`. A causa e a mesma e e minha: os dois scripts de patch moram **dentro** da
arvore que patcham (`D = dirname(__file__)`, a forma da LEI-AKITA 10), entao foram copiados para a
copia junto da cura, e os selos ESTRUTURAIS varrem todo `.py` da arvore -- inclusive um script que
carrega o codigo da cura em STRING e, por isso, parece um segundo leitor do posto. Tirados os dois
arquivos, os **5** testes desses dois contratos voltam `OK` (`logs/o200c_recorte.out`). Nao e selo
frouxo: e selo acertando sobre lixo meu. **A licao e de forma**: script de patch sai da arvore ANTES
de qualquer selo, suite ou commit -- e e' por isso que o veredito se le com `^Ran` + `^OK$`/`^FAILED`
e nao com `grep ^OK`, que neste mesmo log casa a prosa `OK:   31`.

**A AUTORIDADE E A ARVORE MEDIDA, NAO O SCRIPT QUE A CONSTRUIU** -- e este era o jeito de commitar
coisa que ninguem testou. Rodar `patch.py`/`patch2.py` na raia reproduziu **12 dos 15** arquivos e
divergiu em **3**: `geofence_raios.py` (`largo` como PERGUNTA, `p.raio_metros is not None and > TETO`,
em vez do `or 0` que inventava raio 0 para comparar), `colaboradores/views.py` (a guarda deixou de ter
ramo proprio para o campo VAZIO -- a ausencia tem um dono so, `CAMPOS_OBRIGATORIOS_POSTO`) e
`geofence.py` (o payload do chamado leva o raio EFETIVO `_raio`, nao a coluna crua). Os tres sao as
curas que nasceram DEPOIS dos scripts, durante a construcao: quem reaplica o script sozinho **reverte
em silencio** a cura refinada e commita uma arvore que nunca rodou. Entao o que foi para o commit sao
os **15 arquivos da arvore MEDIDA**, provados por `md5sum` nos tres lugares -- cura, copia da suite e
raia -- com **0 divergentes**.

**SEIS SELOS DE HOST DAO VERMELHO FALSO QUANDO RODADOS DA RAIZ DE UMA RAIA, e isso e fila, nao cura.**
Rodei `bin/tests/` inteiro de `/home/ronald/wt-bos` antes do pouso (a conduta de "pasta de selos antes
do push"): **58 verdes, 6 vermelhos**. Os mesmos 6, rodados da arvore principal, dao **rc=0 todos**:
`test_hook_nao_e_copia` (a raia nao tem `.git/hooks` -- o `.git` dela e um arquivo que aponta para o
repo principal), `test_handoff_sessao` (`settings.json` ilegivel), `test_import_tardio_contra_o_ar`
(`logs/deploy.stamp` sem `COMMIT=`), `test_commit_so_o_declarado`, `test_furo_encadeado_ao_cartorio` e
`test_inventario_pessoal_no_commit`. Eles perguntam pelo AMBIENTE (hooks, stamp, logs), que mora na
arvore principal, e a raia nao o tem. O que importa para esta fatia: **`test_lei_protege_sitio.sh`
passou** na raia, com o diff do complemento inteiro -- nenhum dos 13 arquivos toca `arquivo::funcao`
da coluna PROTEGE. Fila de instrumento (L-105, pouso proprio): nomear os 6 e decidir se eles leem a
raiz do REPO em vez do `cwd`, ou se declaram que so respondem na principal.

---

## O145 — **PROVA DEPOIS DO DEPLOY: 45 DIAS E 8.095 MIN MUDARAM DE BALDE, E NADA SUMIU** (08/10 22:4x)

Cura no ar em `32782d0d` (deploy feito). A PROVA e a MESMA sonda de antes, no MESMO lugar (sombra, por
`bin/sombra.sh --rodar`), na MESMA competencia 10/2026, com a MESMA autoridade dos dois numeros --
`espelho_do_colab`, a chamada que a tela, o PDF e o portao `he_pendente` usam. Nao e sonda nova: e o
instrumento de antes apontado para o depois, que e a unica forma de o numero querer dizer algo.

| balde | ANTES (`logs/o145_dois_v3.out`) | DEPOIS (`logs/o145-prova-controle.out`) |
|---|---|---|
| **A)** motor ACUSA e a testemunha CALA | **78 dias** · 17.349 min · 35 colabs | **33 dias** · 9.254 min · 18 colabs |
| **B)** os dois falam | 112 dias · motor 9.952 · testemunha 6.556 | **157 dias** · motor 18.047 · testemunha **18.716** |
| **C)** so a testemunha fala (<= teto, e o certo pela L-097) | 2.975 · 25.678 min | 2.975 · **25.678 min** |
| **D)** nenhum dos dois | 13.125 | 13.125 |

**A PROPRIEDADE QUE FAZ DISSO PROVA, e nao duas medicoes parecidas** (LEI-AKITA 13, propriedades fixas):
**as partes somam o total.** A perdeu 45 dias (78 -> 33) e B ganhou exatamente 45 (112 -> 157). A perdeu
8.095 min (17.349 -> 9.254) e `B_motor` ganhou exatamente 8.095 (9.952 -> 18.047). **C e D nao se mexeram**
-- 2.975 e 13.125 identicos, 25.678 min identicos. Nenhum dia foi criado, nenhum foi engolido: 45 dias
**trocaram de balde**, que e literalmente o que a cura promete -- a testemunha passou a dizer o que o motor
ja acusava. E `B_testemunha` saiu de 6.556 para **18.716 min**: sao 12.160 min (202 h) de ponta de HE que
existiam no julgamento e **nao chegavam a tela, ao PDF nem ao portao**.

**A PREVISAO ERROU, e por isso ela se MEDE.** Antes do deploy eu havia escrito no BACKLOG o esperado:
bucket A **78 -> 34** dias e **17.349 -> 10.517** min. Medido: **33** e **9.254**. A cura moveu **45** dias
onde a previsao dizia 44, e 8.095 min onde dizia 6.832 -- **um dia e 1.263 min a mais**. A diferenca esta
no tamanho da fatia: ela enumerava **44** dias curaveis e o tradutor alcancou **45**. **Nao decompus o dia
extra** -- o "esperado" era projecao da sonda de antes, nao uma conta fechada --, e isso fica dito em vez
de arredondado. O esperado fica escrito aqui ao lado do medido, que e a unica forma de a previsao ter custo;
o que a PROVA sustenta e a conservacao acima, nao a projecao.

**O CONTROLE SE INVERTEU COM A CURA, e isso teve de ser dito antes de o numero valer.** A primeira rodada
pos-deploy carimbou `PAROU: o controle duplo nao bateu` -- e estava **certa**: o controle esperava `col253
02/10` no balde **A**, e A era o DEFEITO. Um controle que exige o defeito reprova a cura. Invertido: col253
02/10 e col207 02/10 esperam **B** ("os dois falam"), e **col37 06/10 fica em A** de proposito, porque
controle sem um caso fora do balde esperado deixa de DISCRIMINAR -- passaria mesmo se tudo caisse em B.
Os tres bateram: `caiu em=B OK`, `caiu em=B OK`, `caiu em=A OK`.

**OS 33 QUE SOBRARAM TEM DONO, e nao sao residuo da O145** (2a passada, por `grade_da_celula`):
- **16 dias · 2.042 min · 9 colabs — celula ATRIBUIDA e a testemunha ainda calou.** Esta e **outra causa**,
  e ela nao se zera na conta da O145. O retrato mostra o alvo com `hora_marco=None` e `hora_prevista='·I2'`
  (col346 03/10, col830 26/09) ou com `status=atraso` e a hora cadastrada vazia de marco (col915, 4 dias,
  160-178 min). Vai para a fila com o numero, nao para o rodape desta fatia.
- **17 dias · 7.212 min · 9 colabs — o dia nao tem celula na grade** (`alvo=None`): col37 06/10 959 min,
  col250 30/09 960, col382 01/10 1.369, col594 03/10 1.080, col788 3 dias de 415-480, col511 03/10 169. E
  **CADASTRO/ESTRUTURA pela L-099**, nao calculo: sem celula, nao ha marco contra o que a ponta se descreva.
  **Dono declarado: O229.** Curar isso por codigo aqui seria o fallback que a LEI-AKITA 1 proibe.

Os dois somam 33 -- o balde A inteiro, aberto por dono, **sem um dia sem nome**.

---

## O145 — **O "127 MIN DE PONTA" NAO E UMA PONTA: E A SOMA DO DIA, E O RAIO NAO FOI FURADO** (08/10 19:5x→20:0x)

Ordem dele, literal: *"Medir primeiro, na O145: por que o dia 02/10 do col207 aparece com ponta de 127 min
se o raio de atribuicao e 90 (escala/utils.py ~319, tol_min=90)."* **Medido — e a resposta refuta a premissa
da pergunta, nao o sistema.** Medi sem motor: `grade_da_celula` -> `montar_realizado_grade` ->
`marcar_pontas_fora`, e conferi no retrato JA lavrado (a medicao com motor sobre frota vai na sombra).

```
col207  empresa=2   CADASTRO DA JANELA: ativa=True piso_min=0 saida_ativa=True desde=2026-08-21
CELULA 2026-10-02 id=115424 origem=gerada   dna.marcos = {'hi':'08:00','hf':'12:00'}  previstos=240
  #116131  02/10 06:30:07  E  -> delta  -90  -> ponta 90 'antes'   (EXATAMENTE no limite)
  #116447  02/10 12:37:45  S  -> delta  +37  -> ponta 37 'depois'
  ata: n_orfas=0  n_missing=0  2 lampadas acesas
  SOMA das pontas do dia = 127
autoridade (a mesma que a tela le): HPL.ler(emp2, 10, 2026), retrato calculado_em 2026-10-08 09:35:58 UTC
  -> col207 dia 2026-10-02: minutos=127  estado=sem_decisao  com as DUAS pontas listadas
  -> universo do col207 no retrato: 1.211 min em 14 dias, 14 sem_decisao
```

**Tres coisas que a medicao estabelece:**

1. **O 127 e uma SOMA, nao uma ponta.** O somador e `ponto/services/he_pendente_lavrado.py:108` —
   `sum(... for x in _fora)` sobre as pontas do DIA. 90 + 37 = 127, e **nenhuma das duas passa do raio**.
2. **O 90 e raio do TURNO, nao do MARCO.** `escala/utils.py:319-322`: `ini = abs_marcos[0][1]`,
   `fim = abs_marcos[-1][1]`, janela `[ini-90, fim+90]`. Dentro do envelope o pareamento e DP + o
   cluster-guard, e a distancia por marco **nao tem teto**. A linha 348 ja diz isso em voz alta:
   *"O raio tol_min segue sendo raio de ATRIBUICAO."* A comparacao e **inclusiva**, e e por isso que a
   batida de 06:30 — exatamente `08:00 menos 90` — casou com o marco em vez de virar orfa.
3. **A TESTEMUNHA NAO MENTE** (conferi o rotulo, LEI-AKITA 8). `templates/ponto/gestao_he.html:231` abre a
   soma nas duas pontas (`{{ s.antes }} min antes da entrada` · `{{ s.depois }} min depois da saida`), e as
   linhas 271-285 listam **uma linha por ponta** sob a coluna `ponta`. O cabecalho (`:22`) diz *"minutos
   batidos fora do marco"* — rotulo de SOMA. O "127 min de ponta" era leitura do enunciado, nao da tela.
   **Nenhuma cura de rotulo a fazer.**

**CONSEQUENCIA PARA A O145: este dia NAO e um caso dela.** A ponta nao desapareceu — apareceu inteira e
esta no retrato lavrado, `sem_decisao`, esperando o admin. A O145 (a ponta que SOME porque a batida passou
do raio, a batida ficou sem par e o marco foi a `missing`) **segue de pe e sem caso medido**: achar um e
pergunta de FROTA, e por isso vai na sombra, nao em prod.

O par de RED da O145 ja esta nomeado, e o primeiro ja esta medido aqui: batida em `marco-90` -> ponta 90
VISIVEL (este dia); batida em `marco-91`, entrada unica -> marco `missing`, `marcar_pontas_fora` devolve
`[]` e **a ponta nao nasce** (os minutos NAO somem do calculo: o motor conta a jornada do marco e acusa a
distancia -- quem fica muda e a testemunha). **Dois valores, dois destinos** — o caso que MORDE. E a cura e do LEITOR: alargar
`tol_min` re-pareia batida em toda a frota, e o dossie proibiu tocar o raio.

**Achado lateral, dono CADASTRO (L-099), NAO e fatia:** col207 tem **14 de 14** dias com ponta na
competencia, 1.211 min, numa escala de 4 h (08:00-12:00) com entrada as 06:30. Se os outros 13 dias
repetem a forma, o DNA nao descreve o turno real — isso vai para a lista **CADASTRO x REALIDADE** pelo
`e6_oraculo.py::dono_da_divergencia`, com o numero, e **nao se cura por codigo**.

---

## O145 — **A FATIA: 44 DIA-COLAB EM QUE A TESTEMUNHA ESTA MUDA, E A CURA E UM TRADUTOR** (08/10 20:4x→21:3x)

**O defeito, na forma exata.** A batida que cai a mais de 90 min do seu marco nao e atribuida
(`escala/utils.py::_alinhar`, `tol_min=90`), a celula daquele marco nasce `missing` e a batida fica **ORFA**.
`ponto/janela_he.py:166` pula `missing` — e so por isso `dia['he_fora_da_janela']` volta `[]`. Nada se perde
do calculo: o MOTOR pareia pelo marco, clipa pela janela (L-097) e **acusa** a distancia em
`dias_cadastro_x_realidade`. O dinheiro esta CERTO e **nenhum centavo se move nesta fatia**. Quem fica muda e
a testemunha — tela, PDF, `he_pendente` do portao, lavratura e calendario.

**O ESCOPO, medido na sombra com controle duplo que PASSOU** (`logs/o145_dois_v3.out`; col253 02/10 esperado
no bucket A e col207 02/10 esperado no B, os dois OK — sem os dois, nenhum numero abaixo seria prova):

| bucket | o que e | dia-colab | minutos |
|---|---|---|---|
| A | motor ACUSA e testemunha VAZIA | 78 | 17.349 |
| B | os dois falam (coerente) | 112 | 9.952 motor / 6.556 testemunha |
| C | so a testemunha fala (dentro do teto: e o certo) | 2.975 | 25.678 |
| D | nenhum dos dois | 13.125 | — |

E o bucket A **nao e homogeneo** — foi preciso uma 2a passada para nao curar 78 com uma cura de 44:

| causa do A | dia-colab | minutos | colabs | e fatia? |
|---|---|---|---|---|
| (a) celula MISSING, batida fora do raio de 90 | **44** | **6.832** | 21 | **SIM — a O145** |
| (c) celula ATRIBUIDA e a testemunha calou | 16 | 2.042 | 9 | nao: os dois leitores alinharam a marcos DIFERENTES (delta 48/50/56 contra motor 64/172/178) |
| (d) o dia nao tem celula na grade | 17 | 7.212 | 9 | nao: 959 a 1.369 min, familia da borda da meia-noite |
| (e) nao ha celula do tipo da ponta | 1 | 1.263 | 1 | nao |

**E (a) nao e so chegada**: col51 24/09 e SAIDA (17:56 contra o marco 16:00, 116 min) e col204 29/09 tambem
(17:29 contra 15:20, 130). Cura que olhasse so a entrada deixaria metade do defeito de pe.

**A CURA E UM TRADUTOR, e isso e LEI-AKITA 2.** `ponto/janela_he.py` ganha `pontas_do_relato(...)`: le as
linhas que o motor JA publicou e as entrega na forma da pendencia que a casa ja tem
(`hora/marco/minutos/sentido`). Ela nao compara minuto com piso nem com teto, nao resolve marco, nao pareia
batida. `ponto/services/espelho.py` a chama no sitio da **Gestao de HE** onde a pendencia nasce -- o que
alimenta tela, PDF, portao e lavratura --, logo depois de `marcar_pontas_fora`. **O CALENDARIO NAO e curado
aqui, e isso se diz com o numero**: `colaboradores/services/calendario.py:374` e o SEGUNDO chamador de
`marcar_pontas_fora` (censo `grep -rn marcar_pontas_fora app --include=*.py | grep -v /tests/`: 2 chamadores
de producao), monta a linha com a celula do LEITOR (`grade_da_celula`) e **nao tem o `resultado_v2` na mao**.
Depois do deploy, o espelho dira 124 min no dia do col253 e o calendario seguira calado no MESMO dia -- a
L-099 ao contrario, por 1 dia-colab de cada vez, ate a **O197**, que e a fatia dele pelo aval
`BOS-EM-RAIA-UM-POR-VEZ` (limite 3: a O197 so abre depois do pouso da O146, mesmo arquivo). A divergencia
esta nomeada na **O229** como item, nao deixada para alguem descobrir na tela. **`tol_min` e o raio nao foram tocados** (o dossie proibiu, e alargar o raio re-pareia
batida em toda a frota). **Juizes novos: 0** — o limiar segue em `entrada_efetiva`/`saida_efetiva`.

Cinco guardas, cada uma com o caso medido que a pediu: **(1)** so linha com `'janela de HE'` na causa — o
escritor da L-084 (`motor_calculo_v2.py:1543`) escreve na MESMA lista com outra forma de dict e **sem** chave
`causa`; **(2)** dedup — a lista repete o dia (o col253 aparece duas vezes em cada data); **(3)** nada onde a
testemunha ja falou (bucket B, 112 dias), senao a pendencia nasceria em DOBRO e o `he_pendente` contaria 2;
**(4)** so quando a celula da ponta esta `missing` (CURA-MAIS-RESTRITIVA: 44, nao 78); **(5)** a linha do
motor tem de nomear o MESMO marco que a celula nomeia — divergiram, o dia que o motor relatou nao e o dia que
a grade montou, e ai a tela nao afirma. Mais o cadastro de sempre: `janela_he_saida_ativa` desligada, a ponta
de saida nao nasce.

**RED evidenciado** (`ponto/tests/test_o145_testemunha_le_a_acusacao_do_motor.py`, 8 casos; primeiro teste da
casa a atravessar a cadeia inteira da testemunha sem mockar `espelho_do_colab`): na copia do HEAD,
`Ran 8 tests` -> **`FAILED (failures=4)`**; na copia curada, `Ran 8 tests in 3.379s` -> **`OK`**. A
autoridade vai citada na falha — *"a batida de 18:56 esta 124 min antes do marco
21:00 e a testemunha esta MUDA ([]) — o motor acusa [('chegada fora da janela de HE', '18:56', '21:00',
124)]"*. Os casos que MORDEM: a acusacao do motor existe no MESMO run (sem ela o RED mediria o nada — a
vacuidade de 01/09); o numero e 124 exato com marco '21:00'; o INTERVALO nao vira ponta, inclusive com o
almoco 120 min fora dos marcos e a ultima saida `missing` (o corte col369 de 01/10 pela porta de tras); a
ponta de 90 min do bucket B continua sendo **UMA**, nao duas; e a saida DESLIGADA no cadastro nao gera ponta.

**O que a cura muda para quem le** (censo fechado, 7 leitores, nenhum com regra propria):
- **o portao do export NAO passa a barrar**: `he_pendente` cresce (**esperado 44 dia-colab, a MEDIR depois do
  deploy** -- o censo mediu o codigo VELHO, e numero sem medicao na fonte nao conta, LEI-AKITA 8), e
  `he_pendente_trava_export` e
  **False nas tres empresas** — medido no CADASTRO na sombra (`logs/o145_trava.out`), nao no default do
  modelo. `folha/porta_export.py:478` so soma esse numero ao que bloqueia com a flag ligada.
- **o cron da recusa, que entrou no ar HOJE as 19:08, nao recusa nenhuma delas**: ele recusa a ponta abaixo
  de `Empresa.limite_decisao_he_min`, que o cadastro diz ser **15 min** nas tres empresas
  (`logs/o145_limite.out`). As 44 estao todas acima de 90 por construcao. Elas vao para a lista do admin,
  que e onde a L-097 as quer.
- **a tela passa a mostrar numeros grandes, e isso e ESPERADO** (previsao, a conferir na tela depois do
  deploy): col174 24/09 deve dizer *"HE fora da janela: 1108 min"*. O dia esta no bucket (a) com o dict
  inteiro impresso -- `motor=[('chegada fora da janela de HE', '02:32', '21:00', 1108)]` e
  `alvo={'tipo': 'E', 'missing': True, 'hora_prevista': '21:00'}` (`logs/o145_dois_v3.out`) --, entao as
  cinco guardas o deixam passar. Nao e bug da cura — e a acusacao do motor ficando visivel. Ler aquilo
  como erro novo e ler o cadastro errado pela primeira vez.

**A PRIMEIRA CURA FICOU MUDA, E O QUE A DENUNCIOU FOI O GREEN SER IDENTICO AO RED** (medido 08/10 21:3x).
A copia curada voltou as MESMAS 4 falhas da copia do HEAD -- nao 2, nao 1: as quatro, iguais. Sem rodar o
lado verde eu teria commitado um tradutor que nunca traduz, com o RED legitimamente vermelho ao lado dele
servindo de prova de que a cura era necessaria -- e nenhum selo da casa morderia isso, porque a funcao
EXISTE, e importada, e chamada.

A causa, lida na fonte: **a celula `missing` tem outra FORMA**. `escala/utils.py:809`
(`montar_realizado_grade`) monta `{'missing': True, 'tipo', 'hora', 'chamado_id'}` e poe a **hora do marco em
`hora`** -- nao ha batida para ocupar essa chave. A celula ATRIBUIDA, no mesmo laco, monta
`{'tipo','hora','status','delta','hora_marco','tipo_real','divergente'}`, com o marco em `hora_marco` e a
BATIDA em `hora`. O `grade_da_celula` do calendario, por sua vez, entrega `hora_prevista` com o marco e
`hora_marco: None` (impresso em `logs/o145_dois_v3.out`). Tres formas, tres chaves, para a mesma pergunta --
*qual e o marco desta celula?* A guarda (5) lia `hora_marco or hora_prevista`, comparava `'21:00'` com `None`
e descartava TODA linha. Cura: ler `hora_marco or hora_prevista or hora`, nessa ordem, **e so porque a guarda
acima dela ja exigiu `missing`** -- na celula atribuida `hora` e a BATIDA, e compara-la com o marco casaria
por acidente. `ponto/janela_he.py:180-182` ja documentava a diferenca de chaves entre construtor e leitor,
**mas so para a celula atribuida**; a forma da `missing` nao estava escrita em lugar nenhum. Que nenhuma
funcao responda *"qual o marco desta celula"* -- cada leitor soletra a ordem das chaves a mao -- e o **item 8
da O229**.

`LEI-AKITA: origem=ponto/janela_he.py (o laco que pula a celula missing) + ponto/services/espelho.py (o
unico escritor da pendencia), testemunha=dias_cadastro_x_realidade do MOTOR (a mesma autoridade que ja clipa
a ponta), RED=ponto/tests/test_o145_testemunha_le_a_acusacao_do_motor.py (4 falhas evidenciadas sem a cura, GREEN 8 OK),
quem-mais-le=7 leitores de he_fora_da_janela, todos medidos com o numero, juizes novos=0 (tradutor; o limiar
segue em entrada_efetiva/saida_efetiva)`

**A SUITE INTEIRA, NA COPIA CURADA: `Ran 10049 tests in 1356.316s` -> `OK (skipped=42)`** (08/10 21:53→22:16,
`logs/o145_suite_copia.out`, copia montada pela porta unica `bin/arvore_do_push.sh --montagem`). E os cinco
selos que LEEM `app/docs/` foram re-rodados DEPOIS, com os docs de agora sincronizados na copia: `Ran 46
tests` -> `OK (skipped=10)`. Eles precisavam disso porque a copia nasceu ANTES das edicoes da lei da O146 e
do registro do complemento da O200 -- selo de doc rodado em copia velha afirma sobre um arquivo que nao
existe mais, e passaria verde dizendo nada. Cura, RED e docs entram num commit SO, e o `bin/deploy.sh
--sem-migrate` vem no MESMO ato (L-107). A PROVA na sombra -- bucket A **78 -> esperado 34**, minutos
**17.349 -> 10.517**, pelo `logs/sombra/censo_o145_dois_leitores_v3.py` com o controle duplo (col253 02/10 no
A, col207 02/10 no B) -- se mede DEPOIS do deploy, contra o codigo no ar, e volta aqui com o numero medido.

---

## O204 — **A HORA DO APARELHO TEM UM SITIO, E O EPOCH DEIXA DE VIRAR DATA DE 1791** (pousada 08/10 20:02)

Primeiro BO da raia `wt-bos` (aval `BOS-EM-RAIA-UM-POR-VEZ`). Veio verde da raia, **pousou pela L-105** —
`git merge --ff-only` + `bin/deploy.sh --sem-migrate` **num ato so** (L-107), sem nada no meio.

```
commit d2cf6606  (ff puro: 1b485f9c..d2cf6606)   6 arquivos, +601/-59
suite da raia:  OK (skipped=28)   Ran 4744 tests in 595.769s   0 FAIL/ERROR nomeado
deploy 20:02:   migrations pendentes 0 · sombra dia=20261008 OK diverge=0 · prova de casca 16 estaticos,
                5 paginas, 606 rotas em 2 urlconf · 3 rotas provadas · selo BUG 128 verde · importerror_500=0
limite 2 do aval conferido: nenhum dos cinco arquivos travados (motor_calculo_v2, janela_he, portas/he,
                escala/models, escala/utils) aparece em `git diff --name-only HEAD raia-bos`
```

**SMOKE EM PROD, SO LEITURA** (nenhum POST — script que POSTa em porta de prod e ESCRITA, lei de 27/08).
Chamei a funcao REAL que acabou de subir, com os dois epochs que o proprio BO provou:

```
batidas=77.937   com timestamp_dispositivo=0
1791071932000              -> aparelho 2026-10-03 20:58:52-03:00  motivo='antigo_demais'  (defasagem 7148,2 min)
   HORA_DO_APARELHO_DESCARTADA colab=None motivo=antigo_demais lido=2026-10-03T20:58:52-03:00
1791158355000              -> aparelho 2026-10-04 20:59:15-03:00  motivo='antigo_demais'  (defasagem 5707,8 min)
2026-10-08T20:00:00-03:00  -> aceita, motivo='offline_aceita'
```

O primeiro epoch e **o caso col218 do BO**: `parse_datetime` o lia como **1791-07-19 20:00:00**, e agora ele
devolve `2026-10-03 20:58:52` — **a hora real da batida**, a mesma que o BO media. Descartada por velha,
**com a trilha que antes nao existia** (o BO dizia *"grava a hora da CHEGADA sem log"*). O terceiro caso
prova que a janela valida continua aceitando. Os `0 de 77.824` do aval seguem `0 de 77.937`: a conta exata
das batidas atingidas e **impossivel sem ESCRITOR**, e nenhuma batida gravada foi tocada.

**Banco da rodada (device que a celula do BACKLOG nomeava e a raia nao achou):** esta suite correu no banco
**compartilhado, pela trava** — `grep -c REGUA_DB` na saida = **0**. A decisao do banco de teste mora em
`config/settings/ci.py:17` (`REGUA_DB`), e `bin/db_teste.sh:21` (`NOME=juliani_db_test`) e o nome do
CONTAINER, nao do banco. A proxima rodada da raia leva `REGUA_DB=test_juliani_bos`. De todo modo o PUSH
serializa com a principal de qualquer jeito, porque `bin/pre-push.sh:130` pega a trava.

**ACHADO DA RAIA, e ele e meu: o portao de arquivo NAO COBRIA ESCRITA EM DOCS.** O merge dela caiu as
19:51 (`git merge --ff-only` recusado pelo git, nao conflito) porque `app/docs/BACKLOG.md` estava sujo na
minha mao — e `logs/principal_em_ato.em_curso` estava **AUSENTE**. O arquivo protegia merge, push e deploy,
e nao protegia escrita de docs; logo nao podia proteger um merge que toca docs. **Ausencia de sinal lida
como sinal bom**, a mesma familia do `[]` de dois sentidos. Curado na conduta no mesmo ato: o
`bin/pausar.sh` deste pouso declara `COBRE=merge, deploy, push E escrita em app/docs/`. A arvore viva ficou
**intacta** durante a recusa (HEAD seguiu em `1b485f9c`, sem `MERGE_HEAD`), e a raia pegou e **soltou** a
pista em vez de segurar ociosa.

**L-109 cumprida no mesmo ato, no pior violador:** a celula de ESTADO da O204 tinha **2.500 caracteres** de
historia acumulada (o teto e 300). Ela foi reescrita em **295**, e a historia e os avais estao aqui, que e
onde a lei diz que moram. E o estado passou a abrir com `**NO AR`, porque
`bin/hook_stop_fila1.py:56` casa `\*\*(FECHADA|FECHADO|NO AR|no ar)\b` — **`**CURA NO AR` nao casa**, e o
hook seguiria cobrando a O204 como fila 1 com a cura ja no ar. **PROVA:** `FECHADO pelo hook: True`, e
`test_hook_nao_cobra_congelado.sh` verde com o marcador em `O145`.

---

## DOIS AVAIS DELE, REGISTRADOS SEM PARAR A FILA (08/10 19:5x e 20:0x)

Os dois viraram linha em `PROMPTS.md` e item no BACKLOG **no mesmo turno** (PROMPT-NAO-SE-REPETE: *"se nao
virou item, nao foi recebido, foi lido"*).

**19:5x — ordem final da principal:** `O145 -> O146 -> O214 item 2 -> itens 3 e 4`, *"as duas mexem no mesmo
sitio"*. O `<!-- ORDEM-VIVA-TOPO -->` saiu de `O214` para **`O145`** — e esse marcador e a AUTORIDADE que o
`bin/handoff_sessao.sh:58` le.

**20:0x — `BOS-EM-RAIA-UM-POR-VEZ`:** os BOs andam em `wt-bos` **um de cada vez**, na ordem
`O204 -> O200 -> O206 -> O199 -> O198 -> O197 -> O207 -> O44 itens 2-8`. As 8 linhas de BO ganharam
`**portao: RAIA wt-bos**`, que e a forma exata que o `_NAO_ANDA` do hook reconhece — sem ela o hook cobraria
oito itens de raia como fila 1 da principal. A ordem antiga (`1o O197, 2o O204...`) vivia repetida em
**quatro** celulas; as quatro foram trocadas por um ponteiro para o aval novo, porque **ordem em dois
sitios sao duas verdades**.

Os limites 1 e 5 **ratificam device que ja existia**: a pista e o deploy sao da principal (e e para isso que
serve `logs/principal_em_ato.em_curso`, levantado neste pouso as 20:01 com dono, motivo e condicao de
saida), e `rc 75` da trava e *"a vez nao chegou"*, nunca vermelho.

**A O204 esta pousada, entao a O200 e a proxima a abrir pelo limite 5.**

---

## FILA — linhas que nasceram deste turno (nao sao trabalho de agora)

- **13 celulas de ESTADO do BACKLOG seguem acima do teto de 300 da L-109** (eram 14; a da O204 foi curada
  neste ato, 2.500 -> 295). As piores restantes: **O206 1.835**, **O197 1.807**, **O200 1.733** — e as tres
  sao celulas de raia, que e onde a historia mais se acumula.
- **Comentario que mente em `escala/models.py:58-59`**: promete *"motor contabiliza a extra normal"* e o
  motor nao le `permite_hora_extra`. Cura no marco que der a extra um leitor de verdade (a O146), nunca
  antes — e **nao** reusando `permite_hora_extra`, que `triagem_batida.py:290` e
  `processar_alertas_turno.py:123` ja consomem com outro sentido.
- **Dia-colab de `col207` como caso de CADASTRO x REALIDADE** (14/14 dias, acima), pelo oraculo, sem codigo.


## O214 ITEM 1 — **O APPLY DA 10, E O HASH QUE NAO E TESTEMUNHA** (08/10 19:0x→19:2x)

**Aval dele de 19:1x, literal:** *"o apply da recusa de ponta pequena da 10 foi rodado por mim no shell em
08/10 19:07: 2685 dias, 259,6 h, hash do fechamento identico nas 4 empresas, segundo ensaio = 0, reversao em
logs/ponta_pequena (...). registra a prova no RELATO sem rodar de novo."* **Nao rodei de novo.** O que segue
e LEITURA do que ficou gravado — e eu perguntei a autoridade, nao ao resumo (LEI-AKITA 8).

### O numero dele confere, e abre
`DecisaoHE` na janela da 10/2026 (`periodo_apuracao(10, 2026, corte_da_empresa)` — nunca 21 cravado):

```
emp2 janela 2026-09-21..2026-10-20   origem=sistema 1971   origem=admin 0
emp3 janela 2026-09-21..2026-10-20   origem=sistema  596   origem=admin 0
emp4 janela 2026-09-21..2026-10-20   origem=sistema  118   origem=admin 0
TOTAL origem=sistema na 10/2026 = 2685
```

**2.685**, exatamente o numero dele, e `origem='admin'` = **0** nas tres — o automatismo nao pisou em
decisao de gente nenhuma.

### O CORTE SE PROVA NOS DOIS LADOS (e aqui havia um numero para explicar)
As reversoes listam **3.075 pares** (2279 + 675 + 121), nao 2.685. A diferenca de **390** nao e perda: e o
proprio limite aparecendo, medido nos dois lados:

```
COM linha gravada: n=2685   minutos min=1    max=15    soma=15.577 min = 259,6 h
SEM linha gravada: n= 390   minutos min=16   max=129
```

Nenhum dia acima de 15 min foi decidido, e nenhum dia de 15 ou menos ficou sem decisao. A reversao guarda os
**candidatos** (o dia com HE pendente), a decisao e so da **ponta pequena** — e os 390 de 16 a 129 min sao
justamente os que ficam para o admin. As **259,6 h** sao a soma dos minutos gravados, nao uma conta paralela.

### O HASH: o da 09 e testemunha, o da 10 NAO E — e esta e a correcao ao resumo
O que o aval da O214 pedia esta **intacto e medido**:

```
FechamentoMensal 09/2026   607 linhas  4da388d4...e37a3e5   IDENTICO ao de antes
exp#24 emp3 09/2026  hash=5c503b95f9f9cd35  invalidada_em=None   IDENTICO
exp#25 emp4 09/2026  hash=84c78cd0871f5f52  invalidada_em=None   IDENTICO
exp#27 emp2 09/2026  hash=361d0f9685f86d3a  invalidada_em=None   IDENTICO
(e as 6 exportacoes mais antigas, todas identicas, nenhuma invalidada)
```

**Mas o `FechamentoMensal` da 10/2026 MUDOU** — `127d6a83...` -> `d6422334...`, com as mesmas 587 linhas. Eu
ia carimbar `MOVEU — PAREI` com esse numero. **Nao e isso, e o erro era meu de testemunha**: a 10 e a
competencia ABERTA, e ela e relavrada o dia inteiro. Medido, em hora LOCAL:

```
linhas da 10/2026 atualizadas HOJE: 358 de 587
por hora: 01h=1 05h=7 06h=23 07h=24 08h=1 10h=2 11h=4 12h=5 13h=55 14h=9 15h=28 16h=26 17h=45 18h=75 19h=53
atualizadas ANTES das 19:07 (a hora do apply dele) = 353
atualizadas 19:07 ou depois                        =   5
```

**353 das 358 mudaram antes de ele rodar**, espalhadas por **quinze horas diferentes do dia**. O hash de uma
competencia aberta nao responde *"nada se moveu"*: ele muda por desenho, porque a corrente de cron relavra a
10 continuamente. A testemunha de que nenhum centavo saiu do lugar e o **gravado da 09** e o **TXT vigente**,
e os dois estao identicos — que e a condicao LITERAL do aval. Mover a 10 e exatamente o que a
**DINHEIRO-EM-COMPETENCIA-ABERTA** pre-aprova, com DIFF publicado, reversao em `logs/` e prova depois: os
tres estao.

Fica a licao, que e de LEITOR e nao de dinheiro: **eu comparei o hash de um universo que tem outro escritor
andando.** Hash so e testemunha de imobilidade onde ha UM escritor e ele esta parado.

### A 09 SEGUE SEM APPLY, e isso e um numero, nao uma impressao
Medido na mesma autoridade, janela `periodo_apuracao(9, 2026, corte)`:

```
emp2 09/2026 janela 2026-08-21..2026-09-20   sistema=0   admin=0
emp3 09/2026 janela 2026-08-21..2026-09-20   sistema=0   admin=0
emp4 09/2026 janela 2026-08-21..2026-09-20   sistema=0   admin=0
```

O seu aval de 19:1x fala **so** da 10, e a 09 esta como estava: **zero** decisao gravada, ensaio pronto e
publicado (4.692 dias / 26.374 min / **439,6 h**, 681 dias restando para o admin — 540+135+6), hashes de
antes em `logs/o214/hash_antes_09.txt`. Ela espera **uma linha sua** (o `!`), e esta ao pe deste RELATO: o
classificador do harness me recusou a execucao duas vezes, com a mensagem de permissao — nao foi lei nem
duvida minha.

### O CRON DO ITEM 1 ESTA INSTALADO
Aval dele de 19:0x: *"o noturno.py de 02:33 esta avalizado junto, e hora derivada; instala."* O `check` de
antes mostrou **quatro** linhas de diferenca e nada mais — as tres caudas
`encadeado.sh recusar_ponta_pequena lavrar_he_pendente - 900 --apply` nas correntes do cartorio (06:27 emp2,
06:29 emp3, 06:31 emp4) e o `noturno.py` 02:37 -> 02:33. `bin/crons.sh install` -> **102 linhas**, e o
`check` depois: `crontab == config/crons.py (102 linhas)`. Reversao:
`logs/crontab_backup_20261008_190813.txt`.

---

## O214 ETAPA 0 + ITEM 1 — **A PONTA PEQUENA E RECUSADA PELO SISTEMA, COM TRILHA, E NENHUM CENTAVO SE MOVE** (08/10 14:2x→17:3x)

**O aval dele de 05/10 18:0x, literal:** *"ITEM 1 PONTA PEQUENA: dia com minutos fora do marco <= limite e
RECUSADO pelo SISTEMA (`DecisaoHE` estado `nao`, ator `sistema`, com trilha), uma vez por dia, idempotente,
sem mover dinheiro; o limite e CADASTRO por empresa e nasce 15 -- campo NOVO, que nao se confunde com
`colaboradores/models.py:69::janela_he_piso_min` (piso da janela, hoje 0)."* E a O214 abre com a **ETAPA 0 --
ESMERIL, antes de qualquer tela**: o censo publicado de toda fonte de HE, com o juiz e os leitores de cada uma.

> **PERGUNTA DE LEI NO TOPO, e o turno NAO volta** (PAREI-DE-LEI-NAO-DEVOLVE-TURNO): a **L-111** diz que sitio
> com zero chamador de producao e um SEGUNDO juiz esperando leitor, e manda apagar no ato. O `ponto/calculador/`
> (o `oraculo`) declara as **13 rubricas** e **nao paga nenhuma**: tem **2 chamadores de producao**
> (`alimentacao` e a constante `NAO_JULGA_PONTUALIDADE` em `chamados/juizes.py:1079`), **nenhum deles de HE**.
> Ele e o sucessor em construcao COM DIFF publicado — e a L-111 **nao distingue** o sucessor declarado do orfao.
> **A pergunta:** sucessor declarado fica fora da L-111 enquanto o DIFF corre, ou a L-111 vale literal e ele
> sai? Eu **nao** toquei nele, e a esteira seguiu.
>
> **`!` na mesa, sem travar a fila** (os dois entraram no `PENDENTES_RONALD.json`, e por isso aparecem no
> `AVAIS.md`): **(a)** o cron da recusa esta **DECLARADO e NAO INSTALADO** — `bin/crons.sh install` e
> all-or-nothing e ligaria tambem o `reverter_situacao_afastado --apply` (O91, espera o seu `!`), entao nada
> dispara de madrugada e o item 1 age em prod **so pela mao**; **(b)** a **09** nao e tocada pelo item 1 porque
> o **portao de frescor do proprio comando a recusa** (retrato de 01/10 12:04, exigencia "de hoje"), e relavrar
> a foto de uma competencia EXPORTADA so para alimentar a recusa e decisao sua, nao minha.

### 1. ETAPA 0 — O CENSO DE TODA FONTE DE HE, com o juiz e os leitores
Medido na arvore viva em `037ae715` e em PROD por SQL de leitura (sem motor, sem trava).
Vocabulario: o motor fala `horas_extra_*` (SINGULAR) e o gravado `horas_extras_*` (PLURAL) --
a traducao mora em UM sitio, `ponto/services/fechamento.py:436-442`, e e por isso que grep pelo
nome do gravado dentro do motor devolve 1 linha de comentario.

#### A. AS DUAS AUTORIDADES DE DINHEIRO, e elas CONCORDAM (prova, nao suposicao)

| guarda | quem escreve | quem le |
|---|---|---|
| `ponto.FechamentoMensal` (mes, Decimal) | `ponto/services/fechamento.py::recalcular_fechamento_mes` (+ a porta `restaurar_fechamento`) | TXT (`folha/export.py`), cartao e **PDF** (`relatorios/cartao_pela_celula.py::totais_da_folha`, desenhado por `relatorios/pdf_base.py::gerar_pdf_auditavel`), ranking (`folha/services/ranking_he.py`) |
| `ponto.DiaPago` (dia, Float, `versao='motor'`) | `ponto/services/dia_pago.py` (bulk_create por competencia) | relatorio de periodo (`relatorios/views.py:486`), Gestao de HE (`ponto/services/gestao_he.py:226`), porta do export (`folha/porta_export.py:132,355`) |

**PROPRIEDADE MEDIDA (L-110, "partes somam o total"), prod 08/10 12:2x, 10 rubricas x 2 competencias:**
`sum(DiaPago) x FechamentoMensal` -- **09: 607 colabs, 0 divergencia em TODAS as 10**;
**10: 587 colabs, 0 divergencia em TODAS as 10**; **0 fechamento sem lavra de dia** nas duas.
NAO E VACUIDADE: sao **22.330 linhas-dia** com `he50=31,66 + he100=730,25 (dos quais 674,94 de
feriado) + noturna=18.786,53 + folga_trab=1.859,96 + intra=2.323,64 + banco=-9.540,71` na 09 e
`he50=5,96 + he100=20,33 + noturna=10.010,27 + folga_trab=331,40 + intra=1.284,30 + banco=-17.222,56`
na 10. O `he50=31,66` da 09 CONFERE com a sonda dele de 05/10 17:59 (`31,68`) -- mesma grandeza,
medida por caminho diferente.
**MAS SAO TRES AUTORIDADES, e nao duas.** A terceira e o **MOTOR VIVO**: `ponto/services/espelho.py`
nao le campo gravado nenhum -- ele apura na hora e MOSTRA o numero do motor rotulado (corte Ronald 23/09
19:2x, e e por isso que o grep por nome de campo gravado nao o encontra). Logo a pergunta da etapa 0 nao
se responde no armazem: ela ja tem **selo construido**, e a LEI-AKITA 4 manda cita-lo em vez de abrir
corte novo -- `relatorios/management/commands/selo_leitores_no_mesmo_numero.py`, tolerancia ZERO, sem
allowlist, que mede 6 pares de uma vez pela funcao que RECUSA o TXT (`folha/porta_export.py::medir`).
**VEREDITO PUBLICADO (R4, `core/placar_estrutural.py:121-146`, remedido 03/10 15:3x):** CINCO pares em
ZERO nas duas competencias (tela x PDF, cartao x TXT, fechamento x soma do DiaPago, topo x soma das
linhas, calendario x espelho), o par 6 (app x tela) ZERO **por construcao** com selo de AST
(`api/tests/test_r4_par6_app_le_a_tela.py`), e o par **espelho x DiaPago** com **1 divergencia na 09**
(col935, 05/09, 10,97 h x 11,10 h = 8 min) e **6 na 10** (col882). O dono dessas 7 e **CADASTRO**, nao
estrutura: marco de TEMPLATE num dia sem celula e fora do vinculo. **O que esta medicao de hoje
acrescenta** e o par 4 remedido em 08/10 com universo MAIOR (607 e 587, frota inteira, nao os 214/21 do
universo do TXT) e tolerancia 0,01 h: segue ZERO.
**O SELO LAVRA CARIMBO e chama o motor**, entao ele se roda na SOMBRA, nunca em prod (sonda de frota com
motor chegou a 266% de CPU com o cliente batendo ponto). Rodada de hoje: na fila, depois do refazer.

#### B. AS 7 [nome] DE HE, com o JUIZ de cada

| fonte | rubrica gravada | JUIZ (sitio unico) | observacao medida |
|---|---|---|---|
| excedente da jornada (1a faixa) | `horas_extras_50` | `motor_calculo_v2::PeriodoCalculo.minutos_extra_50` somado em `ResultadoMes.horas_extra_50` | a REGUA sobe do cadastro pela O211 que pousou HOJE: `core/regua_cct.py::regua_para` -> `get_motor_cct` (:380) injeta `regua_excedente`, e ele tem **dois** valores vivos -- `relogio` (piso legal) e `legais` (CCT vigente, martelo Ronald 30/07, `regua_cct.py:346`) |
| **excedente SEMANAL (44 h)** | **nenhuma** | `motor_calculo_v2:2316,:2357` sob `regua_excedente == 'semanal_44'` | **NAO E FONTE VIVA: 0 produtor de `'semanal_44'` em producao** (censo 08/10, fora de teste e do proprio motor). O ramo existe e nenhum caminho de cadastro o alcanca -- a O214 nomeia "excedente semanal" como fonte a censar, e a resposta medida e *nao ha* |
| excedente 2a faixa (dia normal) | `horas_extras_100` menos o recorte feriado | `ResultadoMes.horas_extra_100_normal` | NAO tem campo proprio no gravado: e DIFERENCA de dois campos, feita em `folha/export.py:279` |
| dobra de feriado | `horas_extras_100_feriado` | `motor_calculo_v2::CICLOS_FERIADO_SIMPLES` (:2971) + `minutos_extra_100_feriado` | 674,94 h em 09 (cai o 07/09); 0,00 em 10 |
| HE em janela noturna | `horas_extras_50_noturna`, `horas_extras_100_noturna` | `motor_calculo_v2::he_noturna` (:298) -- UMA funcao para as duas | **SUBSET, nao ADDEND**, declarado em `:258-262` e repetido pelos leitores (`folha/export.py:283`, `relatorios/views.py:524`). Somar = pagar duas vezes |
| folga trabalhada | `horas_folga_trabalhada` | `resultado.periodos_ft` + `escala/services/escala_certa.py::escala_certa_no_dia` (CLASSE3-FOLGA-100) | a hora entra em `horas_trabalhadas` mesmo sem escala certa; o 100% e que depende da lavra |
| intrajornada nao concedida | `horas_intra_indenizada` | `motor_calculo_v2::TOLERANCIA_INTRA_MIN` (:69) | 2.323,64 h na 09 |
| banco de horas | `saldo_banco_horas` | `motor_calculo_v2:2628` (`saldo_total / 60`, no ramo COMERCIAL) -- no armazem e linha de AJUSTE do `DiaPago` (`tipo='ajuste'`), nao do dia | NEGATIVO nas duas competencias (-9.540,71 e -17.222,56) |
| reflexo de extras no DSR | `horas_reflexo_dsr` | `motor_calculo_v2:1845-1861` (extras da semana / dias trabalhados), Sumula 172; zerado explicitamente em `:2636` no outro ramo | tambem linha de AJUSTE: so existe no MES |
| **ponta fora do marco** | **nenhuma por si** | `ponto/janela_he.py` (`entrada_efetiva`, `saida_efetiva`, `marcar_pontas_fora`) + `ponto.DecisaoHE` | e a fonte da O214: ela CLIPA a ponta antes de virar `extra_50/100`. Leitor unico do clip: `motor_calculo_v2::_entrada_efetiva`/`_saida_efetiva` (:1280, :1315), e a `DecisaoHE` se consulta em UM sitio (:1253) |

**Cadastro da janela** (`colaboradores/models.py:54-92`): `janela_he_ativa`, `janela_he_desde`,
`janela_he_piso_min` (**hoje 0** -- piso da janela), `janela_he_teto_min`, `janela_he_saida_ativa`,
`janela_he_saida_teto_min`. O LIMITE do item 1 (nasce 15) e campo **NOVO** e nao se confunde com
`janela_he_piso_min` -- a O214 ja nomeia essa confusao, e a leitura de hoje confirma: o piso entra
em `dentro_da_janela(minutos_antes, piso_min)`, que decide se a ponta e' DESPREZIVEL, nao se ela
e' DECIDIVEL.

#### C. SITIO COM REGRA PROPRIA DE HE -- censo fechado

**O CRITERIO NAO E GREP DE NOME DE CAMPO** -- e foi essa a troca de pergunta do
PLACAR-DA-S3-CONTA-EXERCICIO (29/09 15:1x): *(a) 0 leitor mostrando numero de motor sem o rotulo, (b) 0
leitor com derivacao propria de dinheiro, por AST*. Entao o que responde aqui e o selo de AST, nao a
minha varredura de texto -- e a memoria da casa diz por que (selo que varre texto morde a prosa que
explica a cura, 5x).
Para o censo FICA O MAPA, que e o que a etapa 0 pede: os 14 arquivos que nomeiam campo de HE fora de
`tests/` e `management/`: `core/juizes.py` (registro), `folha/export.py`, `folha/porta_export.py`,
`folha/services/ranking_he.py`, `ponto/calculador/{alimentacao,regras}.py`, `ponto/models.py`,
`ponto/motor_calculo_v2.py`, `ponto/services/{dia_pago,fechamento,gestao_he}.py`,
`relatorios/cartao_pela_celula.py`, `relatorios/views.py`, e
`holerite/test_fech_encerrada_pelo_juiz.py`. Somam-se a eles dois leitores que o grep por
campo NAO pega porque leem o JUIZ e nao a rubrica: `colaboradores/services/calendario.py:374`
(`marcar_pontas_fora`, o mesmo par que o espelho le) e `api/views.py::api_espelho_v2` (o app, que o
par 6 do R4 prova por AST que le a TELA).

#### D. ACHADOS (registrados, com numero -- nenhum e cura desta etapa)

1. **A LAPIDE DO `DiaPago` DESMENTE O VIVO.** `ponto/models.py:280` diz *"ESTA FATIA E ADITIVA:
   nenhum leitor le daqui ainda, e e proibido ler antes de o contador da S2 zerar"* -- e hoje ha
   **3 leitores de producao** (`relatorios/views.py:486`, `ponto/services/gestao_he.py:226`,
   `folha/porta_export.py:132`). A proibicao caiu com a O-DIA-PAGO S2/S3 e a lapide nao foi
   reescrita. Cura: o texto se corrige no proximo commit que tocar o arquivo, com a medicao citada.
2. **`horas_extra_100_normal` nao tem campo gravado** -- o TXT o reconstitui subtraindo
   (`folha/export.py:279`). Nao e derivacao paralela (subtracao de dois campos do mesmo guarda),
   mas e a unica rubrica do TXT que nenhum leitor pode conferir contra o armazem sem fazer a conta.
3. **`holerite/test_fech_encerrada_pelo_juiz.py` esta FORA de um diretorio `tests/`** e nomeia
   campos de HE -- o `holerite` ja ficou fora da regua uma vez (achado H-c, 04/09).
4. **`ponto/calculador/` (o `oraculo`) declara as 13 RUBRICAS e NAO as paga**: 2 chamadores de
   producao (`alimentacao`, e a constante `NAO_JULGA_PONTUALIDADE` em `chamados/juizes.py:1079`),
   nenhum deles de HE. E o sucessor em construcao COM DIFF publicado, nao um juiz escondido --
   mas a L-111 fala de "sitio sem chamador de producao" e **nao distingue** o sucessor declarado
   do orfao. **PERGUNTA DE LEI, no topo do RELATO, sem devolver turno.**
### 2. O ITEM 1: tres pecas, e cada uma decide UMA coisa

| peca | sitio | o que ela decide |
|---|---|---|
| **cadastro** | `colaboradores/models.py::Empresa.limite_decisao_he_min` (migration `0057`, **default 15**) | *quanto e "pequena"* — por empresa, com nome e leitor (L-012). **A edicao e o admin do Django**, nao uma tela: `colaboradores/admin.py` o poe no `list_display` e o `ModelAdmin` sem `fields` o deixa editavel; censo na arvore = 0 view, 0 rota, 0 template, igual ao vizinho `janela_he_piso_min`, que nunca teve tela. E por isso que a **L-006 segue PELA-METADE**, e quem a fecha e a tela da O223 — este commit nao melhora nem piora esse numero. **`limite 0` desliga a recusa automatica**, e isso esta declarado no emissor: cadastro que desliga nao e fallback, e cadastro |
| **ator** | `ponto/models.py::DecisaoHE.origem` (migration `0071`, `admin`/`sistema`, `db_index`) | *quem decidiu* — a trilha que faltava para distinguir recusa do sistema de recusa de gente |
| **porta** | `ponto/portas/he.py::recusar_ponta_pequena` | *se este dia pode ser recusado* — e e o UNICO sitio do `<=` |
| **emissor** | `ponto/management/commands/recusar_ponta_pequena.py` | *quais dias chegam a porta* — e **nada mais**: ele declara no proprio docstring que nao tem regra propria e nomeia os quatro donos (`janela_he.py::marcar_pontas_fora`, `he_pendente_lavrado.py::apurar`+`lavrar`, a porta, e o campo de cadastro) |

**O `dry_run` e o unico sitio do `<=`, e isso nao e conveniencia.** O comando precisa CONTAR antes de aplicar;
a tentacao era ele comparar `minutos <= limite` por conta, que seria a SEGUNDA leitura da mesma regra
(LEI-AKITA 2) com um caminho possivel onde a conta do ensaio e a do apply divergem e nada fica vermelho. Com o
flag, o ensaio passa por TODAS as guardas da porta e devolve o que o apply devolveria, parando so antes de
`_gravar`.

**O QUE A PORTA NAO FAZ, e cada "nao" tem lei atras:**
- **nao esconde o dia** — subir o `janela_he_piso_min` tiraria o dia da lista inteira: sem denominador, sem
  trilha, invisivel. Aqui o dia FICA visivel com `estado='nao'` e os minutos gravados como foto: sai do
  NUMERADOR (`pendentes`) e permanece no DENOMINADOR (`dias`). **Subir o piso ESCONDE; recusar REGISTRA** — e e
  por isso que sao dois campos de cadastro e nao um;
- **nao sobrescreve humano** — linha que existe, autorizada OU recusada, devolve `ja_decidido` e nao escreve
  nada. Autorizado de 9 min CONTINUA autorizado;
- **nao move dinheiro** — ela nao chama `recalcular_por_evento`, porque recusar e o PADRAO da L-097 (o minuto
  fora da janela ja nao conta): o gravado ja esta como se recusado. **E isso e RED, nao promessa** — o comando
  tira o `_hash_dos_fechamentos` antes e depois e sai com `SystemExit('O GRAVADO MUDOU …')` se mexer;
- **nao mede nada** — `minutos` e `limite` vem de quem chamou, como na porta do admin e pela mesma razao.

**OS PORTOES DO EMISSOR, na ordem em que ele os abre:** `limite 0` (cadastro desliga) → retrato inexistente →
**frescor** (`_idade != _hoje` → *RECUSEI AGIR*) → **celula julgada** (`CelulaDia.veredito is not None`) →
**turno encerrado** (`cartorio.marcos_vencidos(...)[1] == []`) → **ja decidido**. Os dois do meio sao a lei do
**FATO ENCERRADO** da secao 6 do CLAUDE.md, e nao um teto por DATA: no cross-meia-noite o dia acaba antes do
turno, e foi assim que 08/08 fez tres vitimas num dia.

### 3. OS 6 REDs DELE, nomeados — e quais sao do item 1

| # | o RED dele (literal) | de quem | onde ele mora |
|---|---|---|---|
| 1 | *"9 min fora -> recusado pelo sistema, sai da fila, dinheiro IDENTICO"* | **item 1** | tres sitios, um por verbo: `PontaPequenaDaPortaTest::test_RED_9_min_sao_RECUSADOS_pelo_SISTEMA_com_a_foto_e_a_trilha`, `FilaDoRetratoTest::test_RED_recusado_pelo_SISTEMA_sai_do_NUMERADOR_e_FICA_no_DENOMINADOR` e `ComandoDoSistemaTest::test_RED_o_comando_RECUSA_a_ponta_pequena_e_o_GRAVADO_nao_se_move` (+ a BORDA: `test_RED_a_BORDA_15_exatos_e_RECUSADA_porque_o_aval_diz_menor_ou_IGUAL`) |
| 2 | *"16 min fora -> fica na fila e nada muda sozinho"* | **item 1** | `PontaPequenaDaPortaTest::test_RED_16_min_FICAM_do_admin_e_a_porta_nao_recusa_em_silencio` (a porta levanta `DecisaoHERecusada`, nao devolve `False` em silencio) e `FilaDoRetratoTest::test_RED_16_min_SEGUE_pendente_e_nada_muda_sozinho` |
| 3 | *"mesmo dia 2x -> uma `DecisaoHE` e uma linha de trilha"* | **item 1** | `PontaPequenaDaPortaTest::test_RED_o_mesmo_dia_DUAS_vezes_da_UMA_decisao_e_UMA_trilha` (a porta) e `ComandoDoSistemaTest::test_RED_o_mesmo_comando_DUAS_vezes_da_UMA_decisao` (o emissor) |
| 4 | *"autorizar 5 dias de um colab num ato -> 5 `DecisaoHE`, e o numero MOSTRADO antes == o GRAVADO depois"* | item 2 | `test_tela_gestao_he_fatia2_lote_limite.py` (REGISTRADA, nao construir — ordem 01/10 20:5x) |
| 5 | *"empresa com trava ligada e 1 dia grande sem decisao -> export RECUSA, decidido LIBERA, e dia pequeno recusado pelo sistema NAO trava"* | item 3 | idem |
| 6 | *"limite trocado no cadastro -> a fila muda **sem deploy**"* | **item 1** | `ComandoDoSistemaTest::test_RED_6_o_limite_trocado_no_CADASTRO_muda_a_fila_SEM_DEPLOY`, e o par dele `::test_RED_limite_ZERO_no_cadastro_DESLIGA_a_recusa_e_o_comando_DIZ` |

**E "sai da fila" se afirma pelo que o admin VE, nao pela contagem de linhas da tabela.** O RED 1 ficou mais
duro de proposito: alem de `APLICADO`/`IDENTICO`, ele cobra
`enriquecer(ler(emp, 9, 2026)[0])['totais']['sem_decisao'] == 0` **e** que o retrato NAO se moveu
(`dias == 1`, `pendentes == 1`). E isso prova o MECANISMO: a tela nao esta relendo uma foto nova, ela esta
SOBREPONDO a decisao viva — e e exatamente essa sobreposicao que faz a promessa valer com a recusa rodando
DEPOIS da lavratura da noite.

### 4. RED PRIMEIRO — o vermelho evidenciado TRES vezes (LEI-AKITA 5)

| o que foi tirado | resultado |
|---|---|
| a copia no **HEAD** (nem campo, nem porta, nem emissor) | `Ran 16 tests` → **`FAILED (failures=1, errors=15)`** |
| a copia **curada menos o emissor** | `Ran 10 tests` → **`FAILED (errors=10)`**, com 9× `CommandError: Unknown command: 'recusar_ponta_pequena'` |
| o **contador da Central** com UMA decisao gravada (achado da secao 5) | `Ran 10 tests` → **`FAILED (failures=1)`**, `AssertionError: 2 != 1` |

**VERDE depois:** o modulo do item 1 `Ran 26 tests in 0.607s` → **`OK`**; a **familia inteira de HE**, 11
modulos (tela, filtros, cascata, forma B, competencia, rota, index, atalho da Central, item 1, fatia 2,
calendario) `Ran 150 tests in 12.147s` → **`OK (skipped=7)`**; `ruff` nos 6 arquivos tocados →
`All checks passed!`.
### 5. ACHADO NO CAMINHO (LEI-AKITA 6) — O CONTADOR DA CENTRAL NAO LIA A DECISAO, E A O214 ERA QUEM IA REVELAR

**O fato.** `ponto/services/gestao_he.py::pendentes_na_central` somava `retrato['pendentes']` — o INTEIRO
congelado no `MetricaSnapshot` pela lavratura da noite. A tela e o PDF leem a MESMA foto, mas SOBREPOEM a
decisao VIVA: `enriquecer` chama `_estado_por_dia` (`:220`), reescreve `linha['estado']` (`:244`) e
recalcula `sem_decisao` (`:246`, `:302`). Entao o admin decidia um dia, a tela o tirava da fila na hora, e
o atalho da Central seguia contando esse dia ate a lavratura seguinte (D+1 06:27+). Contador != universo
(LEI-AKITA 8), com DOIS leitores: o badge da Central (`relatorios/views.py:42`) e o painel de gestao
(`chamados/views.py:759`).

**O QUE EU IA ESCREVER E ESTA ERRADO, e a medicao e que corrige:** eu ia publicar *"defeito vivo para toda
decisao humana desde 30/09"*. **`DecisaoHE.objects.count()` em prod, 08/10 16:16 = 0.** Nenhum admin decidiu
um dia ainda, entao **ninguem viu numero errado**: o defeito era LATENTE, nao vivo. O que o torna urgente e
a outra ponta — o item 1 grava **milhares** de `DecisaoHE` num ato (2.685 dias so na 10), e seria o primeiro
escritor da tabela. A divergencia nasceria na mesma noite em que a fatia entrasse.

**O selo que devia pegar passava VAZIO.**
`chamados/tests/test_atalho_he_na_central.py::test_RED_o_numero_do_atalho_E_o_total_sem_decisao_da_tela`
compara os dois numeros — e fabricava o retrato **sem nenhuma linha de `DecisaoHE`**. Com a tabela vazia os
dois lados valem 2 por coincidencia; e a mesma coincidencia que fazia a medicao de 01/10 em prod (1.817)
bater. E o `[]` de dois sentidos da secao 6 do CLAUDE.md: ausencia de sinal lida como sinal bom.

**RED evidenciado:** o caso com UM `Colaborador` real e UMA `DecisaoHE(ORIGEM_SISTEMA)` gravada no dia do
retrato → `Ran 10 tests`, `FAILED (failures=1)`,
`AssertionError: 2 != 1 : o numero do atalho (2) nao e o total da tela (1) -- contador != universo (L1)`.

**Cura na ORIGEM (LEI-AKITA 1).** O contador passa a contar a pendencia VIVA pela MESMA autoridade da tela
— `_estado_por_dia(ids, ini, fim)`, chave ausente = sem decisao — percorrendo os dias do retrato. **Nao se
chama `enriquecer` aqui de proposito**, e isso esta escrito no sitio: `enriquecer` monta `tira`, `padrao` e
cadastro para CADA colaborador, trabalho de APRESENTACAO que a tela de destino paga para UMA empresa por
clique e que o badge pagaria para TODAS em cada carga do painel. Nao nasce juiz, nao nasce aritmetica nova, e
`sem_retrato` continua NOMEANDO a empresa sem foto. O selo virou MORDE: a asercao so passa se os dois lados
lerem a decisao.
### 6. A FILA DA 09 E DA 10, **ANTES E DEPOIS** — medida na SOMBRA, com o apply exercido

Instrumento: sombra completa de hoje (`carimbo dia=20261008 status=OK tipo=completa diverge=0`), container sem
rede, `config.settings.sombra`, cpuset de teste pela fonte unica, trava por arquivo. A corrida inteira esta em
`logs/o214/frota_sombra_1621.txt`: migrate das duas migrations → relavratura do retrato da 10 → fila ANTES →
recusa em ENSAIO → recusa APLICADA → fila DEPOIS → o MESMO apply de novo → o portao da 09.

**A 10/2026 (21/09–20/10, competencia ABERTA):**

| | dias pendentes | horas pendentes | dias ≤15 min | dias >15 min |
|---|---|---|---|---|
| **ANTES** | **3.087** | **537,3 h** | 2.693 | 394 |
| recusado pelo SISTEMA | −2.693 | −260,9 h | | |
| **DEPOIS** | **394** | **276,3 h** | **0** | 394 |

Por empresa, o recusado: **emp2 1.978 dias / 196,0 h**, **emp3 597 / 57,1 h**, **emp4 118 / 7,8 h**. **A fila
do admin cai 87% em dias e 51% em horas** — e a assimetria e o ponto: sai a grande maioria dos DIAS carregando
a menor parte das HORAS. E isso que o item 1 e. O que resta e o que o aval chama de *"dia grande"*: 394 dias
com 276,3 h, a fila que de fato precisa de gente.

**ENSAIO == APLICADO, dia por dia e empresa por empresa** (2.693 dias / 15.654 min nos dois): o `dry_run` passa
pelas mesmas guardas e e o unico sitio do `<=`.

**PROVA:** `logs/o214/frota_sombra_1621.txt:29` e `:40` --- `ENSAIO: 2693 dia(s), 15654 min (260.9 h)` e
`APLICADO: 2693 dia(s), 15654 min (260.9 h)`; por empresa, as linhas `:23/:25/:27` (ensaio) e `:31/:34/:37`
(aplicado), iguais nas tres.

**DINHEIRO IDENTICO, por hash, nas tres empresas:** `FechamentoMensal` `0f09a54ac09a → 0f09a54ac09a` (emp2),
`38517a0f25c6 → 38517a0f25c6` (emp3), `8e035894d976 → 8e035894d976` (emp4). Nao e promessa do docstring: o
comando tira o hash antes e depois e sai com `SystemExit` se ele mexer.

**IDEMPOTENCIA EXERCIDA (contrato 2):** o MESMO apply rodado de novo → **0 dia(s), 0 min**, com
`ja decidido 1978 / 597 / 118`. Nenhuma segunda linha, nenhuma segunda trilha.

**OS DOIS PORTOES DE FATO ENCERRADO NAO MORDERAM NESTA FROTA** — `turno ABERTO 0` e `sem celula julgada 0` nas
tres empresas —, e eu registro isso como MEDIDA e nao como virtude: nesta janela nenhum dia com ponta estava com
turno aberto ou celula sem veredito. Quem sustenta as duas guardas sao os selos, que MORDEM:
`test_RED_o_12x36_que_entrou_as_19h_e_nao_saiu_NAO_recebe_recusa` e
`test_RED_dia_sem_CELULA_JULGADA_nao_recebe_recusa`.

**A 09/2026 (21/08–20/09, competencia EXPORTADA): ANTES = DEPOIS, e quem recusou foi o CODIGO.**

| | dias pendentes | horas pendentes |
|---|---|---|
| **ANTES** | **5.370** | **949,1 h** |
| **DEPOIS** | **5.370** (nada escrito) | **949,1 h** |

As tres empresas responderam a mesma coisa: *"RECUSEI AGIR -- o retrato e de 01/10 e hoje e 08/10"*. O portao de
frescor e anterior ao laco, e **nao e `--apply` que o liga**: nem o ensaio passa. Eu nao escolhi poupar a 09 —
**o comando a poupou**, e a lei que ele cumpre e a de que decisao sobre foto velha e decisao sobre minuto que
pode nao existir mais.

**O TAMANHO DO QUE A 09 OFERECERIA, para a decisao ser dele e com numero** (medido em prod, 08/10 16:16, so
leitura): dos 5.370 dias pendentes, **4.689 estao em ≤15 min e somam 439,1 h**; sobrariam **681 dias / 510,0 h**.
Para alcanca-los seria preciso **relavrar a foto de uma competencia exportada** — ato que a L-092 nao proibe (o
retrato nao e o gravado) mas que reescreve a testemunha datada de uma competencia ja fotografada. **ELE AVALIZOU as 17:0x**, e o
aval esta no `PENDENTES_RONALD.json` como `O214-RELAVRAR-A-09` com o texto inteiro no campo `resposta`:
*"relavra o retrato de HE da 09 e recusa as 4.689 pontas pequenas (439,1 h), pelo HORIZONTE-PADRAO e pela
L-113; ficam 681 dias para o admin. condicao: nenhum centavo se move, provado por hash do gravado da 09 e do
TXT vigente identicos antes e depois; se algum hash mudar, PAREI com o numero."* O ato **corre depois deste
pouso**, nao dentro dele: o portao e de DOIS hashes (o gravado da 09 e o TXT vigente dela), e o caminho e o
mesmo do item 1 — relavrar o RETRATO por `lavrar_he_pendente` para o portao de frescor passar, e so entao a
recusa. A 09 esta **dentro do HORIZONTE-PADRAO** (aberta + anterior: hoje 10/2026 e 09/2026), que e o que
dispensa a pergunta de borda que esta secao levantou as 16:16.

**POR QUE OS NUMEROS DA SOMBRA E DE PROD NAO SAO IGUAIS NA 10, e o delta esta medido:** prod tem **3.075 dias /
534,1 h** (retrato das 06:35) contra **3.087 / 537,3 h** na sombra (retrato das 16:20) — **+12 dias, +3,2 h**,
que sao as batidas de hoje entre as duas lavraturas. O censo de colaboradores COM ponta tambem anda com elas: emp2 323 nos dois lados, emp4 16 nos dois, e a emp3 com **94 na sombra contra 95 em prod** -- um colaborador cujo unico dia com ponta saiu da janela entre as 06:35 e as 16:20.
A 09 bate EXATAMENTE nos dois lados (5.370 / 949,1 h), porque o retrato dela nao foi refeito em lugar nenhum.
### 7. O CRON — **declarado, AVALIZADO para instalar, e PARADO na quarta linha do `check`**

`config/crons.py` ganhou **um ENC**, nao um horario: `ENC('recusar_ponta_pequena',
gatilho='lavrar_he_pendente', args='--apply', log='ponta_pequena.log', estagio='auditoria', teto_s=900,
depende=('lavrar_he_pendente',))`. Cron encadeado nao tem hora propria — ele espera o ARQUIVO
`logs/fim/<nome>.<emp>.<dia>` do gatilho, que e a lei de GATE TEMPORAL POR ARQUIVO (nunca `pgrep`). Papel
declarado em `PAPEL_DO_CRON` como **`agenda`**, com a folga escrita: sem a entrada,
`core/tests/test_contract_tabuleiro.py:16` fica VERMELHO, e ela existe para o cron nao nascer sem dizer o que e.
Diagrama regenerado por `manage.py gerar_diagrama`: **+1 no, +1 aresta**, `56 nos diarios` → **57**,
`arestas depende declaradas: 40` → **41**; `docs/MAPA.md` saiu identico.

**ELE AVALIZOU O INSTALL as 17:0x, COM UMA CONDICAO LITERAL — E A CONDICAO NAO FECHOU.** O aval
(`O214-CRON-RECUSA-INSTALAR`, no `PENDENTES_RONALD.json` com o texto inteiro no campo `resposta`): *"instala.
antes, publica no RELATO a saida do bin/crons.sh check com tudo que o install liga e desliga; se for so a recusa
de ponta pequena, os contadores ja declarados e o reverter_situacao_afastado com o tripwire dele, instala no
mesmo ato; se aparecer qualquer outra coisa, PAREI com a lista."*

**A SAIDA DO `check`, publicada como ele pediu** (`bin/crons.sh check`, 08/10 17:21, rc=1, arquivo
`logs/o214/crons_check_1721.txt`): **4 linhas de delta**, e tres sao a obra:

| o que o install faria | linha | e o que ele avalizou |
|---|---|---|
| **LIGA** `recusar_ponta_pequena` encadeado em `lavrar_he_pendente` na cadeia da **emp2** | `27 6 * * *` | **SIM** |
| **LIGA** o mesmo na cadeia da **emp3** | `29 6 * * *` | **SIM** |
| **LIGA** o mesmo na cadeia da **emp4** | `31 6 * * *` | **SIM** |
| **MOVE** `eval/noturno.py` de **02:37** para **02:33** | `33 2 * * *` contra `37 2 * * *` no host | **NAO esta no aval** |

**ENTAO O INSTALL NAO CORREU, pela condicao dele e pela LEI-AKITA 9** (aval condicional que nao fecha na
condicao = PAROU com o numero). E a quarta linha nao e um cron novo nem meu: `config/crons.py:445` declara o
`noturno.py` com `inicio_derivado('noturno.py', vizinho='03:20', piso='00:15')` — **hora DERIVADA**, que anda
com `config/crons_duracao.json`, e esse arquivo foi reescrito pela medicao do proprio `check --medir` das 04:05
de hoje (20 duracoes mudaram; `esmeril_espelho` 98 → 124 s, `auditar_invariantes_chamados` 134 → 144 s). O host
carrega o 02:37 de ANTES dessa medicao. Nao e divergencia de codigo: e a derivacao funcionando e o crontab
atrasado em relacao a ela.

**O `reverter_situacao_afastado`, que esta linha prometia que o install ligaria, NAO APARECE no delta** — logo o
aval dele sobre ele (`--apply` avalizado junto) nao tem nada a executar aqui, e isso tambem e medicao, nao
esquecimento.

**O QUE ISSO NAO PARA:** a fila 1 segue (PAREI-NAO-DEVOLVE-TURNO — a lista vai ao RELATO e a esteira anda), e o
item 1 **age em prod pela MAO**, com o comando, que e o que a secao 8 prova. O que o deploy publica e a
CAPACIDADE: a declaracao aparece como `_crons_falta` no placar (o `check` nao reprova nada —
`bin/placar_code.sh:37` engole o rc e so conta linhas `>`/`<`). Nada dispara de madrugada ainda. **A saida do
PAREI e de UMA linha**: ou ele diz que o `noturno.py` de 02:33 esta avalizado junto, ou o install espera a
proxima vez em que o crontab e a derivacao estiverem iguais.

**A ORDEM DA CADEIA ESTA CERTA MESMO COM A RECUSA DEPOIS DA LAVRATURA, e isto era um medo meu que a leitura
desfez.** O meu proprio desenho (`o214_item1_universo.md` §5) dizia que a recusa teria de rodar ANTES de
`lavrar_he_pendente`, senao o numerador do retrato ficaria 24 h velho. Esta errado: a tela e o PDF **sobrepoem a
decisao VIVA** — `enriquecer` chama `_estado_por_dia` (`:220`) e recalcula `sem_decisao` (`:246`, `:302`) —,
entao o dia sai da fila do admin NA HORA. O que carrega a defasagem de uma noite e so o INTEIRO congelado
`numerador` do `MetricaSnapshot`, que e foto datada e por desenho nao se re-lavra por decisao. Censo dos leitores
desse inteiro: `inteligencia/` nao plota `chave='he_pendente'` em lugar nenhum, e os dois leitores que havia
(badge da Central e painel de gestao) deixaram de ler o inteiro nesta fatia — e a secao 5.

### 8. NO AR

**PROVA: deploy as 17:22 por `bin/deploy.sh`** — as duas migrations pousaram (`colaboradores.0057` add
`limite_decisao_he_min` a `empresa`, `ponto.0071` add `origem` a `decisaohe`), `sombra: dia=20261008 status=OK
tipo=completa diverge=0 erros=0`, prova de casca (16 estaticos, 5 paginas, 606 rotas em 2 urlconf), reload das
TRES cascas juntas e as tres rotas provadas (`saas_core /health/` 200, `saas_ui /colaboradores/` 302,
`mensageria /health/` 200), selo BUG 128 verde e `importerror_500=0` na janela 16:22–17:22.

**O CODIGO ESTA NO AR E RESPONDE EM PROD, medido as 17:23 pelo ENSAIO (sem escrita, `--apply` ausente):**

| empresa | limite | dias que o SISTEMA recusaria | horas | acima do limite (ficam para o admin) |
|---|---|---|---|---|
| **emp2** | 15 min | **1.971** | **195,0 h** | 308 |
| **emp3** | 15 min | **596** | **56,8 h** | 79 |
| **emp4** | 15 min | **118** | **7,8 h** | 3 |
| **total** | | **2.685** | **259,6 h** | **390** |

`ja decidido 0`, `turno ABERTO 0`, `sem celula julgada 0`, `no-op 0`, `erro 0` nas tres, e o hash do
`FechamentoMensal` **IDENTICO** nas tres (`14181156476c`, `223c7e0b6ce1`, `f69a6965b49b`) — o portao de dinheiro
do proprio comando, exercido em prod. Arquivo: `logs/o214/ensaio_prod_10.txt`.

**O `--apply` em prod NAO CORREU, e o motivo nao e lei nem `!`: o classificador do harness recusou o comando**
(`docker exec saas_core ... recusar_ponta_pequena --apply`), com o ensaio do MESMO comando liberado. O aval dele
cobre o ato (o item 1 nao move dinheiro, e o hash acima prova isso na frota inteira); o que falta e permissao de
ferramenta, e isso e linha para ele, nao trava de fila. **A fila SEGUE**: o proximo e a O204, que ele acabou de
por na frente.

**O ACHADO DO MEU PROPRIO POUSO, e ele custou 34 corridas de cron** (16 min, 17:06 → 17:22). Eu dividi a copia em
dois atos para proteger a janela de cron — 13 arquivos primeiro, os dois `models.py` por ultimo, junto do
migrate —, e **a divisao estava errada**: `colaboradores/admin.py` cita `limite_decisao_he_min` no `list_display`,
e o **system check do Django** resolve isso no import, ANTES de qualquer query. Entao o arquivo que eu chamei de
neutro de schema quebrou TODO `manage.py` do `saas_core` com `admin.E108`, e nao houve query nenhuma. Depois,
entre a copia dos models e o migrate, o erro trocou de forma e passou a ser `column
colaboradores_empresa.limite_decisao_he_min does not exist`. MEDIDO por log, as duas assinaturas:

| log | `admin.E108` (17:06→17:19) | coluna ausente (17:19→17:22) |
|---|---|---|
| `placar_situacional.log` | 6 | 4 |
| `vigia_de_hora.log` | 3 | 4 |
| `alertas.log` | 2 | 2 |
| `ausencias.log` | 2 | 2 |
| `escalonamento.log` | 2 | 2 |
| `lavrar_perguntas_stale.log` | 2 | 0 |
| `abriu_nao_bateu.log` | 1 | 0 |
| `reconciliador.log` | 1 | 0 |
| `reconciliar_fantasmas.log` | 1 | 0 |
| **total** | **20** | **14** |

Nenhum dano: os crons sao idempotentes por requisito e o tick seguinte ao deploy ja correu limpo; as cascas de
gunicorn nunca viram o erro, porque o worker vivo nao rele o disco (BUG 128). **A LICAO, na origem:** nao existe
copia parcial segura. A janela de cron nao se protege adiando o `models.py`, porque **qualquer** arquivo que
NOMEIE o campo novo — admin, serializer, form, `list_display` — basta para derrubar o import. A unica ordem
segura e **arvore inteira e `deploy.sh` no mesmo ato**, que e exatamente o que a L-107 ja manda para merge e
que eu nao li como valendo para copia de fatia. Vira linha na fila (nao se abre achado no meio do marco,
ESMERIL-DO-MARCO), e a L-107 e a lei que ja responde.

### 9. O PUSH RECUSADO AS 17:55 — **A SUITE PASSOU E O `rm` DA COPIA NAO**, e o arquivo era MEU

`logs/o214/push_o214.out`: `Ran 10029 tests in 736.073s` / `OK (skipped=42)`, control-plane
`Ran 22 tests in 108.602s` / `OK`, `pre-push: OK — push liberado.` **E depois disso:**

```
rm: cannot remove '/tmp/prepush-arvore.mMQRkp/app/logs/ponta_pequena/emp1328_092026_20260930_090000.json': Permission denied
error: failed to push some refs to 'https://github.com/26031963/hasner-ponto.git'
```

O arquivo tem **423 bytes**, dono **root**, e e a reversao que o `apply` do MEU teste
(`ponto/tests/test_o214_ponta_pequena.py::ComandoDoSistemaTest`, dez `apply=True`) escreve DE VERDADE em
`recusar_ponta_pequena.py:49` (`REVERSAO_DIR = '/app/logs/ponta_pequena'`). Dentro do container a suite e
**root**; o `app/logs` da copia nasce do host (`bin/arvore_do_push.sh:88`), entao o diretorio
`ponta_pequena/` fica de root e o `rm -rf` do host nao consegue desligar o arquivo de dentro dele.
**O portao esta certo**: foi ele que impediu a copia orfa numero 93 -- as 92 de 03/10 (2,1 GB, 58.229
entradas de root) sao desta mesma familia.

**A ORIGEM E O TESTE, NAO O PORTAO** (LEI-AKITA 1). E o RED mostrou os dois defeitos de uma vez, porque
eram o mesmo: eu escrevia a reversao e **ninguem a lia** -- o comando promete *"ATO SEM REVERSAO NA MAO
NAO COMECA"* e nenhum selo tocava o arquivo (`grep -n 'reversao'` no teste devolvia **0** linhas; a
promessa passava por AUSENCIA DE SINAL, a familia dos quatro selos vazios de 01/09). O caso novo LE a
reversao, e por isso ele acusa onde ela caiu -- `logs/o214/red_reversao.out`:

```
AssertionError: 0 != 1 : a reversao nao foi escrita no ato do apply: emp1   09/2026 limite 15 min | APLICADO: 1 dia(s) = 9 min (0.1 h) | ...
        reversao: /app/logs/ponta_pequena/emp1_092026_20260930_090000.json
```

GREEN depois da cura, na MESMA copia (`logs/o214/green_reversao.out`): `Ran 27 tests in 0.652s` / `OK`.
E `find <copia> ! -user ronald` devolve **so os tres pontos de tmpfs** (`.hypothesis`, `.mypy_cache`,
`.ruff_cache`): diretorios VAZIOS de dono root dentro de pai do ronald -- e por isso que o `rm` deles
sempre funcionou e o do arquivo nao (desligar arquivo pede escrita no DIRETORIO que o contem).

**AS DUAS CURAS, e elas nao conflitam** (CURA-MAIS-RESTRITIVA):

1. **PRODUTO, neste commit** -- o `setUp` da classe cria o descartavel (`tempfile.mkdtemp` +
   `addCleanup`) e troca nele o `REVERSAO_DIR` do modulo (`mock.patch`, `addCleanup`), mais o caso
   `test_MORDE_a_reversao_se_escreve_ANTES_do_apply_e_diz_os_pares`, que cobra UM arquivo por corrida que
   toca alguem, os `pares` daquela corrida, o limite do cadastro, o filtro `origem='sistema'` escrito, e
   **nenhuma** reversao na corrida sem alvo. O comando de producao nao muda UMA linha: nao nasce flag,
   nao nasce setting, nao nasce fallback.
2. **INSTRUMENTO, pouso PROPRIO depois (L-105)** -- `bin/arvore_do_push.sh:66` monta `--tmpfs` para os
   tres caches e **nao** para `app/logs` nem `app/media`, os dois que ele mesmo cria em `:88` por "a
   suite escreve neles". Enquanto isso nao pousa, qualquer teste que escreva ARQUIVO (nao log) ali
   repete o caso -- a cura 1 fecha o meu, nao a classe. Vai com o selo
   `bin/tests/test_montagem_vem_do_arvore_do_push.sh` no mesmo ato.

A copia orfa das 17:55 foi apagada por container root (eram **20 KB** -- o `rm` do hook ja havia levado o
resto antes de parar no arquivo): `docker run --rm -v /tmp:/host alpine rm -rf`, e depois
`ls -d /tmp/prepush-arvore.*` devolve **0 copia**.

### 10. O POUSO DO INSTRUMENTO — **a CLASSE morre na porta**, e o caso media veio junto (08/10 19:0x)

O item 2 acima era a divida da secao 9, e pousa aqui **sozinho** (L-105: instrumento nao pousa com
produto). `bin/arvore_do_push.sh --montagem` passa a entregar `--tmpfs /app/logs --tmpfs /app/media`
ao lado dos tres caches, e o selo `bin/tests/test_montagem_vem_do_arvore_do_push.sh` passa a **cobrar
os cinco**.

**O DETALHE QUE EXPLICA POR QUE OS TRES CACHES NUNCA DOERAM** -- e que eu nao sabia quando escrevi a
secao 9: tmpfs deixa o ponto de montagem **VAZIO**, e `rmdir` de diretorio vazio pede permissao no
**PAI**, que e do usuario. Por isso `<copia>/app/.ruff_cache` sai como root em todo push e o `rm -rf`
nunca reclamou. Um **ARQUIVO** de root dentro de um **DIRETORIO** de root nao se desfaz -- e e so
isso que separa o vazamento de 03/10 (que se limpava) do push morto de 17:55 (que nao).

**MEDIDO, com a porta de HEAD** (copia por `bin/arvore_do_push.sh HEAD`, container no cpuset de teste
escrevendo um arquivo em CADA um dos dois):

```
--- root na copia:
<copia>/app/.hypothesis  <copia>/app/.mypy_cache  <copia>/app/.ruff_cache
<copia>/app/media/fotos  <copia>/app/media/fotos/f.jpg
<copia>/app/logs/ponta_pequena  <copia>/app/logs/ponta_pequena/emp1_092026.json
--- rm -rf: rc=1   (cannot remove ... Permission denied, nos DOIS)
--- sobrou? /tmp/prepush-arvore.dLNrx3   A COPIA FICOU
```

**MEDIDO, com a porta curada**, mesmo container e mesmos dois arquivos (o container os ESCREVE e os
ve -- `ls` devolve `emp1_092026.json`; eles morrem com ele):

```
--- root na copia (depois do container):
<copia>/app/.hypothesis  <copia>/app/.ruff_cache  <copia>/app/.mypy_cache
--- rm -rf: rc=0
--- sobrou? (vazio)        copias orfas: 0
```

**RED do selo, literal**, antes de a porta mudar: `a porta nao entrega` + `--tmpfs /app/logs` e a
mesma linha para `/app/media` -> `RED (2)`, rc=1. Depois: `OK -- uma porta entrega o que falta na
copia`.

**O QUE NAO MUDOU, e foi medido antes de escrever a linha:** ninguem le `<copia>/app/logs` depois do
container (**0** leitores em `bin/`; o `pre-push.sh` le stdout, o `vigia_arvore.sh` cria os dois por
conta propria em `:65` sobre copia de `rsync`, que nao perde o .gitignore). E na **ARVORE VIVA** esta
porta nao e consultada: `bin/suite.sh:70` so pergunta quando ha `--dir`. Entao tmpfs aqui nao esconde
saida de ninguem e nao alcanca `app/logs` de producao.

A pergunta 1 do selo (UM ESCRITOR DA LISTA) **nao** cresceu: os seis scripts da sombra montam esses
mesmos dois tmpfs sobre a **arvore viva** `:ro`, que nao e copia, e puni-los seria acusar inocente --
a prosa do selo diz isso desde 03/10 e segue dizendo, agora com a razao de as duas coisas conviverem.

## O211 POUSO B — **O APPLY DA 10 EXECUTADO E PROVADO, E O CADASTRO QUE DECIDE A REGUA SAI DA EDICAO LIVRE DO ADMIN** (08/10 13:1x→13:4x)

**O aval dele de 08/10 12:4x, literal:** *"O211 pouso B, completar o apply da 10: recalcular o gravado dos 70 colabs da
emp2 que o DIFF das 11:25 nomeia, pela porta, com a foto de reversao das 11:45 e a 09 intacta por hash;
prova no RELATO. achado (3): Empresa.regime_trabalhista e AplicacaoConvencao saem da edicao livre do
Django admin (so-leitura, molde O124) ate a tela do O223 existir, com selo, em commit proprio agora.
achados (1), (2) e (4) vao para a fila de instrumento, so registrar."*

### 1. O APPLY: executado as 13:16, pela porta, e o numero visto DUAS VEZES

Porta: `ponto/services/fechamento.py::recalcular_fechamento_mes(10, 2026, empresa_id=2,
colaborador_ids=<70>)` — **rc=OK, processados=70, 18,1 s**. Artefato inteiro em
`logs/o211b_apply10_prova_1319.txt` (8 secoes), previsao somente-leitura das 13:11 em
`logs/o211b_apply10_previsao_1311.txt`.

**O TIE-OUT, e as duas medicoes ficam LADO A LADO de proposito** — dobrar uma na outra esconderia o que
cada uma mede:

| medicao | onde | quando | colabs que movem | soma do delta `horas_noturnas` |
|---|---|---|---|---|
| DIFF de frota (recalculo x recalculo, MESMOS dados) | SOMBRA, dump de hoje 04:00 | 08/10 11:25 | 70 de 587 | **−1204,86 h** |
| previsao somente-leitura (antes do apply) | PROD | 08/10 13:11 | 67 de 70 | **−1190,80 h** |
| GRAVADO, foto x foto (depois do apply) | PROD | 08/10 13:16 | 67 de 70 | **−1190,80 h** |

A previsao e o realizado dao o MESMO numero — e e isso que torna o apply uma execucao, e nao uma
descoberta. A diferenca entre sombra e prod esta MEDIDA, nao arredondada: `−1204,86` menos
`col489+col879+col964` (`−14,27`) = `−1190,59`, contra `−1190,80` de prod, **gap 0,21 h**. Os tres que
faltam na conta de prod sao EXATAMENTE as tres linhas das 70 cujo `atualizado_em` e posterior ao deploy
das 11:52 — o `recalcular_por_evento` ja as havia reescrito com a regua nova no evento de uma batida. O
gap de 0,21 h e batida chegada entre o dump das 04:00 e o apply.

**As quatro condicoes da DINHEIRO-EM-COMPETENCIA-ABERTA, uma a uma:**
- **(1) DIFF publicado ANTES** — `logs/o211b_diff_frota_081125.txt`, publicado neste RELATO as 11:2x.
- **(2) reversao em `logs/`, agora com as DUAS tabelas** — `FechamentoMensal`
  (`logs/o211b_foto_reversao_202610_130856.csv`, 587 linhas, md5 `09a95eb5140e802e352a14e8fd800450`) **e**
  `DiaPago` (`logs/o211b_foto_reversao_diapago_202610_131413.csv`, 2.408 linhas, md5
  `46bebe9792c0146b53013961ab0b07e5`). A segunda foto nao e zelo: o modo de ESCRITA da porta lavra tambem
  o `DiaPago` (`fechamento.py:623`), nas versoes `'motor'` E `'oraculo'`, e `folha/porta_export.py:63,355`
  compara exatamente as duas — reverter so o `FechamentoMensal` deixaria o `DiaPago` com a regua nova.
  A FOTO DAS 11:45 QUE O SEU AVAL CITA fica como a referencia dele; **a que reverte e a de 13:08**, e o
  motivo e medido: **254 das 587 linhas** sao reescritas por evento ao longo do dia, e a foto mais velha
  desfaria o efeito LEGITIMO das batidas do intervalo junto com o meu apply.
- **(3) a 09 EXPORTADA intacta** — md5 do gravado da 09 (16 campos de dinheiro, 587 linhas)
  `ad605834b1237c85efab10ef4bd04e5a` **identico** antes e depois; `folha_exportacaodominio mes=9` com 8
  registros e md5 `bd6d13650cf0520ab3ba4aea9933b4c0` nos dois lados.
- **(4) prova depois** — e esta secao.

**IDEMPOTENCIA (contrato 2 do estrutural):** a mesma previsao somente-leitura rodada DE NOVO depois do
apply da `COLABS_QUE_MEXEM=0 de 70, OUTROS=NENHUM`. `status` dos 70: `aberto|70` antes e depois — a porta
nao mexe em carimbo.

**O QUE MEXEU FORA DO ALVO, com o fato que causou cada um** (7 campo-colab em 4 colabs, nenhum deles
`horas_noturnas`): `col441` atestado **#4779** (05/10 a 12/10) aprovado 06/10 13:05 UTC, depois da ultima
lavra da linha → `dias_previstos +2`, `minutos_abonados +1320`, `semanas_dsr` 2/3 → 3/2; `col245` e
`col584` com 1 batida criada depois da lavra; `col572` com **0** batida nova e **0** celula tocada desde
22/09 — nele o minuto mudou por CODIGO novo (lavra de 08/10 01:55 UTC, anterior ao commit `265e7e87`), nao
por fato novo. **O discriminante ESTRUTURAL e mais forte que os quatro casos:** na sombra o DIFF foi
recalculo x recalculo sobre os MESMOS dados e deu **ZERO** campo fora de `horas_noturnas` em 587 colabs —
logo o pouso B nao move esses campos; em prod a comparacao e linha VELHA x recalculo FRESCO, e linha velha
carrega o fato e o CODIGO do dia em que foi lavrada. O commit `037ae715` nao toca sitio nenhum que calcule
`minutos_realizados`, `dias_previstos`, `minutos_abonados` ou `semanas_dsr_*`.

**E 4 colabs FORA DOS 70 mexeram, e NAO fui eu:** `col592`, `col618`, `col962`, `col970` — cada um com UMA
batida criada 0,4 s antes da sua linha ser reescrita, dentro da janela das fotos. A porta nao podia te-los
tocado: o escopo e `pk__in` dos 70 (`ponto/services/fechamento.py:96-97`).

**O QUE A PORTA RECUSOU, e por que nao e dano:** `col366` e `col960` tiveram a lavratura do `DiaPago`
`versao='oraculo'` RECUSADA pela propria porta — *"2 campo(s) de dia sem dono declarado:
horas_extras_100_noturna, horas_extras_50_noturna"*. E a recusa declarada de 02/10 (13 de 15 campos com
dono; escrever ZERO perderia hora paga em silencio). Os dois tem **0** linha `'oraculo'` — nunca tiveram,
nada ficou velho — e 23 linhas `'motor'` cada, lavradas agora; ninguem le `'oraculo'` hoje (calendario e
`porta_export` filtram `versao='motor'`). Fica NOMEADO: espera a LEI do ancoramento do trecho extra.

### 2. ACHADO (3): o cadastro que decide a regua sai da edicao livre do admin

**RED EVIDENCIADO ANTES DA CURA** (copia do HEAD, `bin/suite.sh --dir <copia>`): `Ran 13 tests` →
`FAILED (failures=2, errors=2)`. As quatro mensagens, que sao a propria medicao:
- `colaboradores.Empresa.regime_trabalhista voltou a ser editavel no admin (EmpresaAdmin)`, e o assert
  imprimiu os **15 campos editaveis** de `Empresa`;
- `core.AplicacaoConvencao` → `KeyError` / `not found in {...}` com os **15 registros** do admin listados.

**A MEDICAO CORRIGIU METADE DO SEU PEDIDO, e isso fica dito.** O aval diz *"saem da edicao livre do Django
admin"* para as duas. Para `Empresa` era literal: a classe estava **PELADA** (`list_display` e
`search_fields` nao trancam campo nenhum), entao `regime_trabalhista` — o campo que `regua_cct.py::regua_para`
le para escolher entre piso legal e CCT — se trocava em um clique, sem porta e sem trilha, movendo a folha
da empresa inteira. Para `core.AplicacaoConvencao` **nao havia edicao livre para tirar**: censo de
`admin.register` na arvore, 08/10 13:2x — **13 registros, nenhum dela**. Nao e porta aberta, e cadastro que
decide dinheiro e **nao se le em tela alguma**.

**O QUE FOI FEITO, e por que registrar em vez de deixar fora:**
PROVA: `colaboradores/admin.py:9` `readonly_fields = ["regime_trabalhista"]` e `core/admin.py:63`
`@admin.register(AplicacaoConvencao)` com `(SoLeitura, admin.ModelAdmin)`; `Ran 13 tests OK` no selo,
contra `FAILED (failures=2, errors=2)` na copia do HEAD; `ruff check` limpo nos 4 arquivos; **64 de 64**
selos de host verdes.
`EmpresaAdmin.readonly_fields =
["regime_trabalhista"]` (UM campo — o escopo do aval e literal, LEI-AKITA 9: os outros campos de `Empresa`
seguem como estavam) e `AplicacaoConvencaoAdmin(SoLeitura, admin.ModelAdmin)` em `core/admin.py` —
**visivel, nao editavel**, que e o que o molde manda e o que da ao selo algo que MORDE. Fora do registro, um
selo so poderia afirmar AUSENCIA DE SINAL, e no dia que alguem a registrasse pelada nada ficaria vermelho.
O molde, ao contrario do que o aval supoe, **nao e mais a lista escrita do O124**: o O167 (corte dele 04/10
00:0x) a substituiu por `core/admin.py::SoLeitura`, que deriva os campos do `_meta` — justamente porque a
lista de 3 nomes do O124 deixou `latitude`/`longitude`/`raio_metros` editaveis no `PostoAdmin` por 10 dias.
**GREEN:** `Ran 13 tests ... OK`, e os cinco modulos que enumeram admin ou a tabela juntos `Ran 56 tests ...
OK`. O escritor da `AplicacaoConvencao` continua UM e com trilha: `semear_aplicacao_convencao`. O selo e
`core/tests/test_admin_so_leitura.py::AdminNaoEditaCadastroDaReguaTest`, **6 casos**, e a suite cheia na
copia se confere pelo par `^Ran N tests` + `^OK`.

**O CONTRATO DA O211 FICOU VERMELHO, e a cura foi a PERGUNTA do selo, nao a lei** (molde do O158):
`test_a_ARVORE_INTEIRA_so_tem_os_DOIS_sitios_da_AplicacaoConvencao` enumerava DOIS sitios com papel
declarado, e `core/admin.py` era um terceiro. O contrato existe para proibir um **segundo juiz** de *"onde
esta convencao se aplica"* — e o admin so-leitura nao julga, **EXIBE**. Entao o terceiro papel entra
DECLARADO (`'EXIBE em somente-leitura (nao le para decidir, nao escreve)'`) e **traz prova no mesmo caso**,
para a enumeracao nao virar allowlist de nome: por AST, `core/admin.py` nao pode ter `objects` — quem
consulta decide. Que ele nao EDITA e julgado pelo juiz dessa pergunta
(`test_admin_so_leitura.py::AdminNaoEditaCadastroDaReguaTest`), e nao se re-julga aqui (LEI-AKITA 2).

**NO AR, e provado no PROCESSO DE PROD, nao no disco** (DEPLOY JA): suite cheia na copia
`Ran 10003 tests` -> `OK (skipped=42)`, 0 FAIL e 0 ERROR; commit `f189ce0a` (esta linha dizia `15a401cd`, que era o objeto ANTES do `--amend` que acrescentou esta propria prova -- citar o proprio commit dentro dele e uma afirmacao que o amend seguinte desmente); `bin/deploy.sh --sem-migrate`
as 14:23 com as tres rotas provadas e `importerror_500=0`. O smoke foi feito por INTROSPECCAO no
`saas_ui` vivo, porque o que importa nao e o texto do arquivo e o que o registry do admin responde:
PROVA: `EmpresaAdmin.get_readonly_fields` -> `['regime_trabalhista']`; `AplicacaoConvencaoAdmin` com
`mro = [AplicacaoConvencaoAdmin, SoLeitura, ModelAdmin]`, **0 campo editavel fora do readonly** e
`add=False change=False delete=False` -- as DUAS TRANCAS do molde O167 de pe no processo que o cliente usa.

### 3. ACHADO (6) e a DIVIDA QUE A TRANCA DEIXA — nomeada, nao tapada

- **(6) os outros 14 campos de `Empresa` seguem editaveis no admin**, e a lista saiu do proprio RED:
  `ativa`, `cnpj`, `dia_inicio_competencia`, `em_rollout`, `he_pendente_trava_export`, `id`,
  `janela_he_ativa`, `janela_he_desde`, `janela_he_piso_min`, `janela_he_saida_ativa`,
  `janela_he_saida_teto_min`, `janela_he_teto_min`, `nome_fantasia`, `razao_social`. Nao e escopo deste
  aval e **nao foi tocado** (LEI-AKITA 9), mas tres deles mandam em dinheiro — `dia_inicio_competencia`
  move a JANELA da folha, e os seis `janela_he_*` sao o cadastro do portao de HE que a **O214** esta
  construindo. Fica registrado, com a lista, esperando a sua palavra.
- **`regime_trabalhista` nao tem escritor em CODIGO nenhum**, medido: `grep -rn` na arvore volta so
  LEITORES (`core/regua_cct.py`), a definicao do modelo, duas migrations e tres linhas de prosa em
  `contratos_estruturais.py`/`configuracao_efeito.py`. O `semear_aplicacao_convencao` **nao** o escreve
  (ele grava `ativo`/`motivo` da `AplicacaoConvencao`), e a emp1 virou `'cct'` em 09:01 por ato de shell
  **com trilha** no `LogConfiguracao`. Ou seja: com a tranca de hoje, o campo que decide a regua de uma
  empresa so se escreve por shell com trilha ate a tela existir. **Isso AMPLIA o O223**: ele tem de cobrir
  o CAMPO `regime_trabalhista`, e nao apenas a tabela de aplicacao — a celula dele foi corrigida nesse
  sentido neste commit.
- **(7) o contrato de configuracao NAO VE nem `Empresa` nem `AplicacaoConvencao`** — e o sitio da lista
  nao e o que eu escrevi primeiro, entao fica o nome LIDO: `ENTIDADES_COM_ADMIN` nao mora em
  `core/configuracao_efeito.py`, mora em **`core/tests/test_contract_configuracao_nao_mente.py:35`**, e
  tem seis nomes (`ParametroSistema`, `Praca`, `Posto`, `TipoAusencia`, `Feriado`, `Sindicato`). Duas
  consequencias medidas: (a) `test_MORDE_todo_campo_editavel_esta_declarado` varre
  `admin.site._registry` INTEIRO e descarta tudo que nao esta nessa tupla, logo o `regime_trabalhista`
  nunca foi cobrado por ele e o registro novo da `AplicacaoConvencao` tambem nao e — foi por isso que os
  56 testes dos cinco modulos ficaram verdes sem eu declarar nada; (b) **o que o contrato VIGIA esta
  declarado no SELO, e nao na declaracao** — quem quiser saber se um campo de configuracao e cobrado tem
  de abrir o teste. Declarar `Empresa` ali seria a cura estrutural de verdade e **nao esta no aval**;
  pior, mexer em `configuracao_efeito` move o denominador do teto (L-100). Fica para a sua palavra.
  No mesmo sitio, e nao e deste aval: o `editaveis_dos_admins` le `madmin.readonly_fields` CRU, e nao
  `get_readonly_fields()` — entao o que o mixin `SoLeitura` deriva do `_meta` e **invisivel** para ele, e
  um dos seis nomes da tupla trancado por mixin passaria por editavel.

### 4. AS DUAS LINHAS MINHAS QUE A MEDICAO DESMENTIU, corrigidas no mesmo ato

Pela sua LEIS-LINHA-DESMENTIDA-CORRIGE-NO-PROXIMO-TOQUE (08/10 06:4x): eu publiquei no pouso B, em dois
lugares, que a `core.AplicacaoConvencao` *"se edita pelo mesmo admin do Django"* — a celula de ESTADO da
**L-006** e a linha derivada *"clausula FORA do codigo"*. **O censo de 13:2x desmente**: ela nao estava em
admin nenhum. As duas linhas foram corrigidas com a medicao citada; o veredito `PELA-METADE` e a contagem
`1 de 3` **nao mudaram** — a lei nao andou, o que andou foi a verdade sobre qual porta existia. O achado
(5) deste RELATO tambem foi corrigido no lugar.

### 5. A SUA ORDEM DE 13:3x, REGISTRADA NO MESMO TURNO

**Obra `DIETA-DO-CLAUDE-MD` = item `O224`**, fila 1 **entre a O146 e os BOs**, com a sua lista PROIBIDO
inteira na celula (nao mover regra para skill nem outro arquivo, nao reescrever regra, nao apagar em vez de
mover, nao tocar codigo nem teste) e o seu criterio de PRONTO (`/context` antes e depois no RELATO, CLAUDE.md
**pelo menos 10k tokens** menor, prova de que nenhuma linha de regra mudou). O marcador `ORDEM-VIVA-TOPO`
**nao mudou** (segue `O214`), porque o senhor disse *"nao muda a ordem ate la"*. O que falta DESENHAR, e esta
dito no item: a prova de *"nenhuma linha de regra mudou"* tem de sair de **DIFF de REGRA**, nunca de contagem
de linha — e e a mesma forma da **L-109**, que ja mandou a narrativa para o `LAPIDES.md` em 04/10.

**E os achados (1), (2) e (4) foram para a fila de instrumento como voce mandou — SO REGISTRADOS**, itens
`O225`, `O226` e `O227`, que **nao abrem** antes do pouso de instrumento depois da O211.

### 6. A LINHA DO VEREDITO DA SUITE NO CLAUDE.md ESTAVA ERRADA, e ela era MINHA

Achado no caminho, medido as 14:03, e e o caso do *"selo que passa por ausencia de sinal"* na minha propria
mao. A secao 3 do CLAUDE.md mandava conferir a suite por `grep -E '^(OK|FAILED)( |$)'` — eu segui a linha
ao pe da letra num laco de espera e **ele deu a suite por TERMINADA com ela ainda correndo**, casando a
linha de log `OK — nenhuma divergencia em 2026-10-08.` do cron de conferencia. O `( |$)` que a propria
linha ensinava como cura (ela mesma conta que o `^(OK|FAILED)` pelado ja havia dado um `FAILED` por verde)
**nao cura nada**: `OK --` e `OK —` tem `OK` seguido de espaco.

**O VIVO ja estava certo, e e ele que manda** (secao 8 do CLAUDE.md): `bin/regua.sh:170-171`,
`bin/vigia_arvore.sh:89`, `bin/isolamento.sh:34`, `bin/regua_calendario.sh:47` e os quatro moldes usam
`'^(OK|FAILED)( |\(|$)' ... | tail -1`. São DUAS guardas juntas, e nenhuma delas e o espaco: o `\(` —
porque o Django so imprime `OK`, `OK (skipped=N)` ou `FAILED (...)` — **e** o `tail -1`, porque o veredito
e a ULTIMA linha do run e a prosa vem no meio. A linha do CLAUDE.md nao tinha nem um nem outro. Corrigida
neste commit com os sitios citados; nenhum script mudou, porque nenhum script estava errado.


**O211 POUSO B NO AR as 11:52 de 08/10 -- commit de titulo `O211 pouso B: a regua de dinheiro sobe da
EMPRESA...` --, e com ele a O211 esta FECHADA: a regua de dinheiro sobe da EMPRESA, e a praca so entra por
linha DECLARADA.** (O hash NAO se cita aqui de proposito: este paragrafo entra NO proprio commit, entao
todo hash que eu escrevesse morreria no amend seguinte -- morreu uma vez, as 12:0x. Quem quer o hash le o
`git log`; `logs/deploy.stamp` guarda o do ato do deploy, cuja arvore de CODIGO e identica a esta -- o que
mudou depois dele foi so este RELATO.)
**PROVA:** medida em PROD depois do reload, nao de memoria -- `bin/deploy.sh` **rc=0** as 11:52 --
`colaboradores.0056` aplicada no schema do cliente, sombra `dia=20261008 diverge=0 erros=0`, collectstatic,
prova de casca (16 estaticos, 5 paginas, **599 rotas em 2 urlconfs**), tres cascas recarregadas juntas,
tres rotas provadas, selo do BUG 128 verde, `importerror_500=0`. **A REGUA, pela funcao REAL
(`regua_para(colab, ini)`, somente leitura, sem motor):** `emp1 col678 regime='cct' ->
'Sindicato dos Vigilantes de Londrina'`, `emp2 col43 'cct' -> Londrina`, `emp4 col27 'cct' -> Londrina`
e **`emp3 col49 'clt' -> 'legal (empresa em CLT: Juliani Seguranca Patrimonial)'`** -- o discriminante da
lei de pe no ar, na janela `2026-09-21..2026-10-20`. **A 09 EXPORTADA INTACTA (L-092):** os 8 registros,
3 vigentes, hash a hash IDENTICOS antes e depois do deploy (`logs/o211b_hash_export_antes_deploy.txt` x
`..._depois_deploy.txt`, md5 `bd6d13650cf0520ab3ba4aea9933b4c0` nos dois).
**O CONTADOR DO APPLY, medido duas vezes e com a razao MEDIDA, nao suposta:** linhas da 10 com
`horas_noturnas` diferente da foto de 11:45 = **0 de 587** as 11:53 e ainda **0** as 12:00, contra um teto
de **70**. O que NAO esta zerado e o escritor: **11 linhas da 10 foram reescritas depois do reload**, uma
por minuto (11:53:41 a 12:00:07), 8 delas da emp2 -- o `recalcular_por_evento` esta rodando com a regua
nova. **Por que o contador nao subiu com elas:** as 11 tem `horas_noturnas` **0,00 antes E depois**, ou
seja nenhuma pertence ao universo que a cl.38-d move. O universo real na 10 e `emp2 151 de 445 com noturna`
(soma 7.273,53 h), `emp3 43 de 117`, `emp4 7 de 21`, `emp1 0 de 4` -- e os 70 do DIFF saem dos 151 da emp2.
O contador sobe quando um dos **151** produzir um fato, nao quando qualquer colab bater. O que eu NAO ia
fazer era forcar relavratura de frota para o numero parecer pronto, porque **quem decide quando a
competencia se recalcula e o DP** (`config/crons.py`, `recalcular_fechamento` em `FORA_DE_PIPELINE`).
**ESTE PARAGRAFO E HISTORIA DAS 12:00, e o bloco do topo o SUPERA:** o senhor avalizou o apply as 13:0x,
nomeando os 70 da emp2, e ele foi executado pela porta as 13:16 -- 67 colabs movendo `horas_noturnas` em
`-1190,80 h`. O contador de `0 de 587` acima e o estado de ANTES do seu aval, nao uma pendencia viva.
**DIFF ANTES DO APPLY, como a DINHEIRO-EM-COMPETENCIA-ABERTA manda** (artefato inteiro em
`logs/o211b_diff_frota_081125.txt`): medido 08/10 11:25 na SOMBRA (dump de hoje 04:00), competencia **10**,
`antes`=HEAD e `depois`=a fatia, pela porta unica `bin/simular_folha.sh par o211b` -- UMA trava, `rc=3` que
e **IMPACTO, nao falha**.
- **TXT:** `emp2 DIFERENTE` (143 -> 143 linhas, `entraram=0 sairam=0 mudaram=46`, todas por **horas**, 0
  por motivo e 0 por apto_folha); `emp3 IGUAL`; `emp4 IGUAL`. `TXT=48 RETIDOS=46 DIFF_FOLHA=94`.
- **GRAVADO** (`ponto_fechamentomensal`, campo a campo, 587 colabs = **546 ativos + 41 desligados**):
  `emp1` ZERO, `emp3` ZERO, `emp4` ZERO; `emp2` **UM** campo mexido -- `horas_noturnas`, **70 colabs**,
  soma do delta **-1204.86**.
- **VEREDITO=LIMPO, e o veredito sao TRES perguntas, nao uma.** (1) SUBCONJUNTO: `universo=587
  TROCAM_DE_REGUA=248 MEXERAM_CENTAVO=70`, e `MEXERAM_E_NAO_TROCAM_DE_REGUA=0 []` -- ninguem move centavo
  sem que a regua dele tenha trocado. (2) DISCRIMINANTE por empresa: `emp1 4/2/0 · emp2 445/243/70 ·
  emp3 117/0/0 · emp4 21/3/0`, todas as trocas `'legal' -> 'Sindicato dos Vigilantes de Londrina'` -- a
  **emp3 e `clt` e da ZERO troca e ZERO centavo**, que e o discriminante que a lei pede. (3) por que os
  178 mudos sao mudos: 157 tem `horas_noturnas` ZERO no antes, e os 21 restantes caem fora do alcance das
  duas clausulas.
- **AS DUAS CLAUSULAS, nomeadas, porque e o que explica as razoes NAO uniformes:** a `cl.38-d` tira a hora
  reduzida **so no 12x36** (razao exata 60/52,5 = 1,142857) e a `cl.10` tira a prorrogacao **so depois das
  05h**. Quem acumula as duas cai ~1,37x; quem acumula uma cai 1,14x; quem nao alcanca nenhuma fica em
  ZERO **estando com a regua trocada**. Nao e dispersao, sao duas regras com alcance diferente.
- **os dois casos que exigiram medicao e nao fe.** `col457` apareceu como `mexeu e nao troca de regua` no
  censo das 11:02 e nao era deriva: era **COBERTURA do censo** -- ele e DESLIGADO e eu havia pedido
  `situacao='ativo'` (546 de 587). Recenseado no universo EXATO do DIFF, o fora caiu a 0 e os TROCAM
  subiram de 211 para 248. `col373` troca de regua, tem 1,30 h noturna e nao moveu: a noturna dele nasce
  em dias cujo **DNA diz `tipo_ciclo='6x1'`** (`ec#325` ate 06/10, `ec#1378` em 12x36 so a partir de
  07/10) e termina 22:30 -- a 38-d so alcanca 12x36 e a prorrogacao so alcanca hora pos-05h. **ZERO e a
  resposta certa.**
- **e um limite do MEU instrumento, nomeado em vez de escondido:** a coluna `ciclo` do censo le
  `EscalaColaborador.ativa` enquanto o motor le o **DNA do DIA**. E derivacao paralela: ela **explica,
  nunca julga** -- o veredito usa a assinatura da regua, nao essa coluna. Foi ela que me fez chamar o
  `col373` de 12x36.
**PAUTA DP DA 09, com os DOIS numeros** (artefato em `logs/o211b_pauta_dp_09_081130.txt`): medida 08/10
11:30 na sombra pela porta declarada (`recalcular_fechamento_mes(9, 2026, permitir_exportada=True,
somente_leitura=True)`), com `ESCRITAS_NA_09_NOS_ULTIMOS_30MIN=0` carimbado nas DUAS rodadas. A 09 e
**EXPORTADA e nao foi tocada** -- os 3 hashes vigentes estao no artefato, intactos (`emp2 361d0f96…` 210
linhas, `emp3 5c503b95…` 86, `emp4 84c78cd0…` 9). Leitura x leitura: `emp1`, `emp3` e `emp4` ZERO;
`emp2` tres campos -- `horas_noturnas` **80 colabs -2340.85**, `horas_extras_100_noturna` **9 colabs
-19.25**, `horas_extras_50_noturna` **1 colab -0.24**; `COLABS_QUE_MEXERAM_NA_09=82`. **Isto nao e ordem
de pagamento e nao pede trava**: e a diferenca entre o que o Dominio JA RECEBEU e o que a conta de hoje
diria, com os dois numeros na mesa, como a REGEN-EM-EXPORTADA manda. Se entra por correcao LA, por TXT de
retificacao, ou se fica na 10, e seu e do DP.
**O DEPLOY E O APPLY, E E APPLY DE GRAVADO -- eu havia escrito o contrario nesta mesma linha, e a
medicao me desmentiu antes do commit.** O que eu afirmei as 11:3x foi *"o `ponto_fechamentomensal` de prod
nao se move sozinho"*, com base em `config/crons.py` declarando `recalcular_fechamento` em
`FORA_DE_PIPELINE` -- *"quem decide QUANDO uma competencia se recalcula e o DP"* -- e no
`pre_fechamento --apply` das 05:10 escrever **PAUTA** e nao fechamento. As duas leituras estao certas e a
CONCLUSAO estava errada: ela respondia *"ha cron de FROTA?"* e eu a li como *"ha escritor?"*.
**O QUE DESMENTIU, com numero:** a foto de reversao das 11:45 **nao bateu** com a das 10:18 -- **18 linhas
de 587 diferentes** em `horas_trabalhadas`, `turnos_abertos`, `minutos_realizados`, `horas_saida_antecipada`
e `inconsistencias`. Fui ao carimbo em vez de supor: **254 das 587 linhas da 10 foram reescritas HOJE**,
uma ou duas por minuto (11:05, 11:08, 11:15, 11:16, 11:20, 11:21, 11:28, 11:30, 11:32, 11:33, 11:45,
11:46:26 a ultima), que e assinatura de EVENTO e nao de lote.
**O ESCRITOR, nomeado:** `ponto/services/fechamento.py::recalcular_por_evento` -> `recalcular_fechamento_mes`
com **UM** colaborador e a competencia DAQUELE dia, em `transaction.on_commit`. Quatro chamadores de
producao: a **BATIDA** (`ponto/registro_batida.py:136`), a decisao de HE (`ponto/portas/he.py:130`), a porta
de celula (`ponto/portas/celula.py:318`) e a validacao de pergunta (`chamados/services/validacao.py:159`).
Hoje bateram ponto **238 colabs em 333 batidas** -- e e isso que move 254 linhas.
**O QUE ISSO MUDA NA REVERSAO, e e a parte que eu teria errado:** o gravado volta em DUAS etapas e **nesta
ordem** -- (1) `git revert <este commit>` + `bin/deploy.sh`, para o LEITOR voltar; (2) so entao os valores
de `logs/o211b_foto_reversao_202610_114522.csv` (587 linhas, md5 `a4ea966cd55dedf903f789b3f6147fb0`) voltam
**pela PORTA** `ponto/services/fechamento.py::restaurar_fechamento(colaborador_id, mes, ano, campos,
motivo=...)`, **e so nos campos que o DIFF nomeia** -- nunca por `FechamentoMensal.objects.update()` direto,
que o selo do chokepoint (`folha.tests.test_chokepoint_folha_gate`, allowlist VAZIA) barra com razao:
**reverter tambem e gravar**. A foto e a PROVA DO ANTES, nao o mecanismo -- e restaurar a linha INTEIRA
apagaria o efeito legitimo das batidas posteriores a 11:45 em `horas_trabalhadas` e `minutos_realizados`.
Na ordem inversa a reversao nao segura: a primeira batida de cada colab chamaria `recalcular_por_evento` e
reescreveria o valor pela regua nova. **O que NAO tem cron continua sem cron**: a
relavratura de FROTA segue sendo ato do DP; o que anda sozinho e o colab que bate ponto.
**ACHADOS REGISTRADOS, NAO CURADOS** (regra de negocio fora do pedido pede o seu `!`), somando aos tres do
pouso A: (4) **a guarda da L-092 recusa `somente_leitura`** -- para medir a 09 sem gravar eu tive de passar
`permitir_exportada=True` junto, isto e, a porta nao distingue *"ler o que daria"* de *"gravar na
exportada"*, e quem so quer LER precisa pedir permissao de ESCRITA; (5) `core.AplicacaoConvencao` nasceu
com **0 view, 0 rota, 0 template** (censo na arvore, 08/10), e por isso o **O223** nasce neste commit como
item de fila 2 em vez de a divida sumir junto do carimbo FECHADA. **CORRIGIDO as 13:3x, e a frase que
estava aqui era minha:** eu escrevi que *"a unica porta hoje e o admin do Django"* -- o censo de
`admin.register` da arvore (13 registros) mostrou que ela **nao estava em admin nenhum**. Nao havia porta
de edicao: havia cadastro que decide dinheiro e que nao se lia em tela alguma. O achado (3) a registrou em
`core/admin.py` com o mixin `SoLeitura` (visivel, nao editavel) -- ver o bloco do topo.
**UM VERMELHO QUE NAO ERA VERMELHO, e o erro era meu:** um `ImportError` no meio da construcao parecia
RED de teste e era **colisao de `# -*- coding: ascii -*-`** -- arquivo que declara ascii e recebe um byte
nao-ASCII falha no IMPORT, com cara de ERROR de suite e corpo de lapso de autoria. Aconteceu duas vezes
hoje, e fica aqui para nao gastar uma terceira meia hora.
**O QUE A MIGRATION 0056 CUSTA NO PORTAO, e sai dito antes de doer:** ela pousa **depois** do dump de hoje
(04:00), entao o proximo `bin/sombra.sh --refazer` puro vai carimbar **`d_mig=1`** (o `--conferir` LE o
carimbo, quem o escreve e o refazer) -- nao
e dano, e o portao certo dizendo a verdade (o `sombra.sh` **compara** `django_migrations` prod x sombra e
**nunca migra a sombra**). Qualquer deploy a mais hoje exige **`--refazer --dump-agora`** primeiro, e a
espera e **pelo ARQUIVO** `logs/crons_em_curso/sombra.sh_-.*`, nunca por `pgrep`.

**O211 POUSO A NO AR, e o commit do marco e ESTE -- o CADASTRO da aplicacao de convencao nasce, e nenhum centavo se move.**
**PROVA:** medida ao vivo as 09:0x de 08/10 contra o schema `juliani`, nao de memoria: `AplicacaoConvencao`
com **3 ativas / 3 totais** -- `emp1 -> sind2 praca=None`, `emp2 -> sind2 praca=None`,
`emp4 -> sind2 praca=None`; regimes relidos `emp1='cct'`, `emp2='cct'`, `emp3='clt'`, `emp4='cct'`; trilha
unica `Empresa.regime_trabalhista[emp1] '(vazio)' -> 'cct'` em `LogConfiguracao`, 08/10 **09:01:05** em
UTC-3 (o banco guarda `12:01:05+00`); `showmigrations core --schema=juliani` com a
`0017_aplicacaoconvencao` marcada; `bin/deploy.sh` rc=0 com sombra `dia=20261008 diverge=0 erros=0`, prova
de casca (16 estaticos, 5 paginas, 599 rotas em 2 urlconfs), tres cascas recarregadas juntas e tres rotas
provadas.
REVERSAO EM UMA LINHA, se o senhor nao quiser: `git revert <este commit>` tira o modelo, o command e o selo; o
cadastro ja gravado sai por `AplicacaoConvencao.objects.filter(ativo=True).update(ativo=False)` e o regime
da emp1 volta a `''` pela mesma porta com trilha. Nenhum numero de folha depende disso hoje.
- **ZERO CENTAVO SE MOVE, e isso nao e promessa minha: e o ramo do codigo que esta no ar.**
  `core/regua_cct.py:245` so ramifica em `== 'clt'`; `'cct'` e vazio caem no MESMO caminho da praca. O
  pouso A e o cadastro NASCENDO; quem passa a LER a tabela e o pouso B, e e la que o DIFF de frota se
  mede.
- **TRES linhas, nao duas** -- o seu adendo `EMP1-E-CCT` de 07/10 23:4x. O `--motivo` gravado cita os dois
  avais de 05/10 (16:4x e o adendo do cadastro 16:5x) mais esse, e o `--usuario` e nominal. A assuncao
  declarada segue de pe: o sindicato e o `sind2` por ser o UNICO cadastrado -- se a CCT da emp1 for outra,
  ela nasce como cadastro antes, nunca como literal no codigo.
- **o unico VERMELHO da suite cheia, e a cura veio da propria mensagem do assert.** `Ran 9982 tests` com
  `FAILED (failures=1, skipped=42)`: `chamados/tests/test_contract_crons.py::test_todo_command_tem_casa`
  dizendo `['semear_aplicacao_convencao'] != []` -- command novo sem casa. Declarei em
  `config/crons.py::FORA_DE_PIPELINE` com o motivo ESCRITO: a tabela e CADASTRO, o escritor de rotina e a
  TELA (obra posterior), e cron que repassasse isso todo dia seria SEGUNDO escritor do cadastro
  (LEI-AKITA 7), repondo linha que o DP tirou de proposito. Recorte GREEN depois: `Ran 32 tests` / `OK`.
- **e e a TERCEIRA vez que esta casa paga o mesmo pedagio**: o vizinho de um arquivo NOVO nao e quem o
  importa -- e o CONTRATO QUE ENUMERA O DIRETORIO, e ele nao importa nada meu.
- **o veredito da suite nao se le pelo `grep -E '^(OK|FAILED)'` que este CLAUDE.md ensina.** Naquele
  `.out` esse grep devolvia `OK:   31` e `OK -- nenhuma divergencia em 2026-10-08`, as duas PROSA de log,
  e eu quase dei a suite por verde com um vermelho dentro. O veredito e `^(OK|FAILED)( |$)` mais o
  `Ran N tests`.
- **TRES achados REGISTRADOS, nao curados** -- regra de negocio fora do pedido pede o seu `!`: (1)
  `core/templatetags/core_extras.py:34::rotulo_efeito` tem **ZERO** chamadores em template (`grep -rln`
  nos `*.html` volta vazio) enquanto a taxonomia do `core/configuracao_efeito.py` promete *"a tela TEM de
  dizer isso"* -- os 7 avisos de hoje sao HTML a mao em `templates/core/config/sindicato_form.html`:
  promessa sem mecanismo, e por ser classe que muda o DESENHO vai ao topo do PENDENTES, nao a uma cura
  minha; (2) `core/regua_cct.py:71-98` tem `SEM_EFEITO_NO_CALCULO` e um `rotulo_de_efeito(campo)`
  PROPRIOS -- segunda verdade pre-existente da MESMA pergunta que o `configuracao_efeito` responde; (3)
  `('Empresa','regime_trabalhista')` e editavel por `colaboradores/admin.py:7::EmpresaAdmin` (sem
  `fields`, entao TODO campo entra) e `Empresa` esta FORA de `ENTIDADES_COM_ADMIN` -- campo de
  configuracao com leitor de producao e SEM declaracao, e a regua da O211 passa a DEPENDER dele.
- **`empresas_sem_regime` vai de 1 para 0 com este ato**: a emp1 era a unica com regime vazio E colab
  ativo (3). As emp20, emp21 e emp29 seguem vazias com **zero** ativos -- ficam no contador por cadastro,
  nao por risco.
- **o que FALTA, e e o proximo da fila 1**: pouso B. `regua_para` passa a ler a `AplicacaoConvencao` por
  `_aplicacao_vigente` (mais especifica vence; duas ativas do mesmo nivel = piso legal com o contador
  acusando), os 7 REDs evidenciados contra o pouso A, a `ReguaIntactaNestePousoTest` **invertida** (ela
  nao se apaga: a assercao passa a morder a VOLTA), e o DIFF de frota da **10** na sombra contra o
  GRAVADO publicado ANTES do apply, com reversao em `logs/` e a **09 EXPORTADA intacta por hash**.

**O158 FECHADA -- o marcador `ORDEM-VIVA-TOPO` passa a ser AUTORIDADE, e a sua ordem de hoje pode pousar.**
REVERSAO EM UMA LINHA, se o senhor nao quiser: `git revert <este commit>` devolve a igualdade
`marcador == 1o aberto da tabela` e o selo volta a cobrar `PLACAR-ESTRUTURAL`.
- **por que entrou agora, e nao e a fila lateral de instrumento**: o seu aval das 07:5x poe a `O211` na
  frente e manda o marcador acompanhar; com o marcador em `O211`,
  `bin/tests/test_hook_nao_cobra_congelado.sh` ficava VERMELHO (1 de 64), porque ainda exigia que o
  marcador FOSSE o 1o aberto da tabela. A lei que resolve e sua e esta escrita desde 03/10 19:3x
  (O158), entao nao e PAREI -- e commit de INSTRUMENTO proprio (L-105), ato minimo para a ordem poder
  pousar. A fila lateral de instrumento (O201, O203, selo do `tabela()`, `bin/dieta_relato.sh`,
  `bin/ff_pouso.sh`) segue FECHADA ate o pouso de instrumento depois da O211.
- **REARRANJAR LINHA NAO RESOLVIA, e o motivo e estrutural -- medido pelo criterio do proprio hook**
  (`_linha_de_item_re`, `_FECHADO` em c[3]/c[2], `_NAO_ANDA` em c[3]): o bloco OBRAS tem duas tabelas de
  cabecalhos diferentes; a 1a tem **exatamente 1** linha aberta (`PLACAR-ESTRUTURAL`, 6 celulas) e
  precede a 2a INTEIRA, onde estao os 13 itens que o aval nomeia; a 2a tem **128** abertas, de 5 a 8
  celulas. Mover linha entre tabelas muda o significado da coluna que o hook le como estado; hastear a
  tabela 2 poria essas 128 abertas, que o senhor NAO nomeou, na frente do R6.
- **a cura, na origem**: a PERGUNTA do selo. Sai `primeiro != esperado`; entra "o marcador aponta linha
  que EXISTE no bloco OBRAS e esta ABERTA", com `_linha_de_item_re`/`_FECHADO`/`_NAO_ANDA`
  **importados do hook**, nunca copiados -- selo que copia valor fica vermelho quando a cura move a
  fonte, e aqui o vocabulario tem um dono so.
  PROVA: tres vereditos contra o BACKLOG **real**, so trocando o marcador -- `ID-QUE-NAO-EXISTE` ->
  RED "nao existe linha com esse id"; `CELULA-TURNO-FECHA` (FECHADA hoje) -> RED "esta FECHADO ou
  PARADO"; `O211` -> verde. E o par que MORDE, embutido no selo, classifica quatro BACKLOGs sinteticos
  (vivo/fechado/congelado/inexistente) mais o sem-marcador; MUTANDO a porta para dizer sempre `ok` o
  par fica VERMELHO nos dois sentidos (`ausente` e `parado`). O selo do HEAD, no mesmo arranjo, da o
  RED de hoje: *"o 1o aberto do bloco OBRAS e 'PLACAR-ESTRUTURAL', e o marcador diz 'O211'"*.
- **o que NAO curei, e fica registrado**: `_proximo_da_fila()` segue derivando o 1o aberto pela ORDEM DA
  TABELA, entao o `siga:` do hook do Stop nomeia `PLACAR-ESTRUTURAL` enquanto a cabeca declarada e a
  `O211` -- **segundo leitor** de "qual o item em curso". Nao e porta: medido agora, o `PAREI` NAO
  depende dele (`_relato_parou` consulta a lista `_ids_que_nao_andam()`, e `O211` esta entre os **129**
  vivos), entao um `PAREI: ! ... O211` e aceito. Cura = fila de INSTRUMENTO, depois do pouso da O211.
- **ESTE MARCO NAO PEDE DEPLOY, e o censo e a razao** (DEPLOY JA vale para o que o worker importa). O
  unico arquivo de produto dos tres commits e `app/core/esteira_vigia.py`. CENSO fechado agora:
  `grep` de `esteira_vigia` em `app/**/*.py` fora de testes da `core/placar_tickets.py`,
  `core/integrador_lote.py` e o command `alarme_esteira` -- e **nenhum** `views/urls/middleware/signals`
  o alcanca, nem em 2o nivel (as 3 mencoes a `placar_tickets`/`integrador_lote` em `gate_cobranca.py`,
  `placar_estrutural.py` e `registro_baixa.py` sao PROSA, nao import). Os leitores sao scripts de host
  e `tenant_command`: processo novo a cada chamada, le o DISCO. Entao nao ha worker de gunicorn com a
  versao velha em memoria -- e sem isso, chamar `bin/deploy.sh` seria reiniciar as tres cascas por
  nada.

**O ALARME DA ESTEIRA VOLTOU A SER LIDO PRIMEIRO -- e o bug era meu, de ~6 dias atras.**
`core/esteira_vigia.py::no_relato` promete no proprio docstring que a linha "entra no TOPO do RELATO,
onde se le primeiro". Ela ancorava em `\n## PENDENTES DO RONALD`, e a dieta a mao de `2082e03d`
ARQUIVOU essa secao: desde entao o `i < 0` caia no `else` e DEPOSITAVA o alarme no FIM do arquivo --
lido por ultimo, contra o que o docstring diz. Nao e um atalho que eu escrevi hoje: e um atalho que ja
existia e se tornou o caminho UNICO no dia que a ancora saiu de baixo dele.
- **o dano, medido, e o molde vai COLADO em cada numero** (LEI-AKITA 8: rotulo diz o que a conta faz).
  Universo = linha de uma linha so no molde `^**DD/MM HH:MM `, que e o que o `no_relato` escreve:
  **126 vivas** no RELATO (114 delas carregam `(ALARME)`), **300 ja no `RELATO-ARQUIVO.md`** (178 com
  `(ALARME)`). Das 126 vivas, **90 estavam na CAUDA**, depois do ultimo titulo datado.
- **as outras 36 contam a historia pior, e nao foram movidas de proposito**: todas as 36 estao
  ENGOLIDAS pelo bloco `## 04/10 00:5x`, com carimbos de 02/10 22:35 a 04/10 09:05 debaixo de um
  titulo de 04/10 00:5x. E a prova viva do risco que a dieta tem de tratar: o `else` depositava no EOF
  e um titulo datado foi anexado DEPOIS delas. Ficam onde apareceram -- os carimbos sao <= a data do
  bloco, entao a dieta as arquiva corretamente junto com ele.
- **a repeticao, com o universo nomeado**: 115 das 126 vivas -- **91%** do que o vigia escreveu no
  RELATO -- sao UMA frase so (`a esteira esta parada e o vigia nao esta destravando`), de hora em hora
  desde **02/10 22:35** ate 08/10 05:55. (Eu havia escrito "12 copias": isso era so a janela de 07/10
  18:35 a 08/10 05:55, verdadeira e sem o rotulo que dizia ser janela.) **13** chamadores escrevem por
  essa porta, nao nove: ZUMBIDO, quarentena, AUTO-REVERT, lote rejeitado, relance, teto religado.
- **a cura, na origem**: a ancora passa a ser uma secao PINADA propria, `## ALARMES DA ESTEIRA`, com
  tres ramos EXPLICITOS (secao existe / nao existe / nao ha titulo algum) em vez de um `else` que cai
  no EOF. O ramo "nao existe" insere na posicao do primeiro `## `, de modo que **`s[:j]` fica
  byte-identico** -- e e isso que preserva o `PAREI:` dentro das 40 primeiras linhas que o
  `bin/alarme_sessao_ociosa.py:120` le. O nome da secao foi escolhido sem nenhuma palavra de ATO,
  porque `##` torna a linha *forte* para o portao de publicacao.
- **PROVA, em producao e pela funcao real.** RED evidenciado contra o codigo do HEAD, de pe:
  `falhas=6`, inclusive a literal `(iii) MORDE: o alarme ficou na ULTIMA linha -- depositado no EOF`;
  GREEN na cura, `falhas=0`. Selo na suite: `core/tests/test_no_relato_tem_secao_pinada.py` --
  `Ran 5 tests` -> `OK`, rc=0; `ruff` limpo. **E o vigia provou sozinho**: o tique das 07:00 escreveu
  `**08/10 07:00 vigia da esteira (ALARME)**` LOGO ABAIXO do cabecalho da secao -- esperado pelo
  ARQUIVO, nunca por `pgrep`. (O tique das 06:55 nao escreveu, e isso tambem foi medido: o carimbo
  `vigia_sem_efeito` era 05:55:44 e o tique chegou 34 s antes dos `cada_min=60`.)
- **conservacao das 90 realocadas**: multiset de alarmes **126 -> 126**, as **4.975** linhas que nao
  sao alarme identicas e na MESMA ordem, e delta de **+23 B** = exatamente `## ALARMES DA ESTEIRA`
  mais os separadores.
- **DEPLOY: nao e preciso, e agora com o censo fechado.** O vigia e um `systemd --user` TIMER que
  nasce um `python3` do HOST a cada tique e le o `.py` do disco. Medido: **0 urlconf** importa
  `esteira_vigia`, `placar_tickets` ou `integrador_lote`, entao nenhum worker de gunicorn guarda o
  modulo (BUG 128 vale para as tres cascas, nao para ele); os dois importadores do lado do container
  so o citam em docstring (`config/crons.py:921`, `alarme_esteira.py:1`); e os 3 chamadores de
  `integrador_lote.py` correm no timer do integrador, tambem do HOST.
- **O CENSO ACHOU UM SEGUNDO ESCRITOR, e por isso este marco tem tres commits.**
  `bin/vigia_arvore.sh:160-168` reimplementa o `no_relato` em bash -- clone byte-a-byte da logica
  quebrada, com o MESMO `i = s.find('\n## PENDENTES DO RONALD')`, o MESMO `else` no EOF e o mesmo
  comentario prometendo o topo. Curar so o lado Python seria a meia-correcao que o CLAUDE.md proibe
  nominalmente ("censo de escritores fechado antes"), entao ele passa a CHAMAR a funcao, pelo idioma
  que `bin/esteira.sh:96` e `bin/fabricante.sh:44` ja usam. Vai em commit PROPRIO: `bin/` e
  INSTRUMENTO e nao pousa com produto (L-105).
- **COMMIT 2 (produto): o `vigia_sem_efeito` passa a LER a pausa -- e a lei nao e nova.** O corte de
  27/09 10:5x esta escrito 60 linhas ACIMA, no mesmo arquivo, e a `trava_a_vazia` ja o obedece por
  `_fab_desligado_com_dono` (exige `QUEM=` **e** `SAIDA=`). Este era o leitor que faltou migrar --
  LEI-AKITA 4: a pergunta e "qual leitor nao migrou", nunca "qual a regra". A frase que ele repetia
  ("a esteira esta parada e o vigia nao esta destravando") era FALSA no juizo: o vigia estava
  respeitando a pausa que o Ronald declarou em 26/09 10:01 com dono, motivo e condicao de saida. Cura
  de **2 linhas** (`and not _fab_desligado_com_dono` no `if`, `or _fab_desligado_com_dono` no `elif`
  que RESOLVE): sem a segunda metade a linha de ontem ficaria de pe para sempre e o contador nunca
  voltaria a zero. **NAO e silenciar emissor** (L-062 pede prazo + tripwire + fila): o tripwire ja
  existe e e OUTRO caminho -- pausa ANONIMA segue alarmando por `pausa_sem_dono` (L-079), e o caso
  (iii) do selo e o que impede o silencio de virar desculpa.
  **PROVA:** selo novo `core/tests/test_vigia_sem_efeito_respeita_pausa_com_dono.py`, 4 casos com o
  relogio CRAVADO em 08/10 10:00 (dentro da janela 00:00-22:40, fora do vao 03:40-04:45 -- borda de
  relogio solto da verde por acidente as 23:00). VERMELHO contra o HEAD de pe, numa copia de
  `git archive HEAD` rodada por `--dir`: `Ran 4 tests / FAILED (failures=2)`, a primeira dizendo
  literalmente `[('alarme','vigia_sem_efeito', ...)] != []`. VERDE com a cura, e junto dos vizinhos do
  mesmo assunto: `Ran 13 tests / OK` rc=0 (4 novos + 4 do selo irmao de 27/09 + 5 da cura #1). Ruff
  limpo nos dois arquivos. Os dois casos MORDE ficam VERDES nos **dois** lados da cura, de proposito:
  se tivessem virado verde so depois, o selo teria passado por o vigia ter ficado cego.
  `sem_efeito_seguidas` estava em **1699**; os dois disparos do ramo que EXECUTA (`== 2` ->
  `auto_revert`) foram 24/09 12:30 e 01/10 16:00, os dois terminando `-> arvore nao esta vermelha` --
  **zero revert executado**, logo o que se apaga e RUIDO, nao guarda. Lateral anotado, NAO curado: com
  o contador em 1699 o gatilho `== 2` esta morto ate um reset, e a cura acima e justamente quem volta
  a zera-lo.
- **COMMIT 4 (instrumento, pouso PROPRIO -- L-105): o `bin/vigia_arvore.sh` para de REIMPLEMENTAR
  o `no_relato`, e o selo que ficou cego para a cura 1 reabre o universo pela lei que ele mesmo
  declara.** Duas curas de `bin/`, nenhuma de produto, e o motivo de nao terem vindo com os commits
  2 e 3 e a L-105: instrumento nao pousa junto com produto.
  - **o clone**: `bin/vigia_arvore.sh:160-168` tinha 9 linhas de python embutido que abriam o
    RELATO, procuravam a ancora `## PENDENTES DO RONALD` e escreviam a linha -- byte-a-byte a
    pergunta de que `app/core/esteira_vigia.py::no_relato` e o DONO. Medido: a ancora que ele
    procurava tem **0** ocorrencia no RELATO do HEAD, pelo molde do proprio clone (inicio de
    linha; ver PROVA abaixo), desde que a dieta de 02/10 (`2082e03d`) a
    arquivou, e o clone caia para o EOF -- por ~6 dias TODO alarme desta casa foi depositado na
    ULTIMA linha do arquivo, lido por fim, o oposto do que o proprio comentario prometia. A cura
    da secao pinada (commit 3) **nao alcancaria** este arquivo: dois escritores do mesmo estado
    mentem mesmo estando cada um certo, e curar so um lado e a meia-correcao que o CLAUDE.md
    proibe nominalmente. Agora ele DELEGA (`python3 -c` que importa `core.esteira_vigia` e chama
    `ev.no_relato`), sem `2>/dev/null` -- erro de delegacao tem de aparecer. Saiu tambem o
    `RELATO=$R/app/docs/RELATO.md` da linha 24: **0 usos** no codigo depois da cura, e era uma
    segunda declaracao de onde o RELATO mora (L-111).
  - **o censo que licenciou a cura estreita** (MEIA-CORRECAO exige censo fechado ANTES): os
    escritores do RELATO em `bin/` sao tres. `relato.sh:88` escreve `$TMP/ESTADO.md`, pergunta
    outra. `deploy_agendado.sh:33-47` escreve no RELATO mas na secao PROPRIA que ele cria
    (`## DEPLOYS AGENDADOS` = **1** no HEAD), com queda para `## NO AR HOJE` = **0** -- latente,
    fica como lateral nomeada, nao e o alarme desta casa.
    PROVA: contado no RELATO do **HEAD** (`git show HEAD:app/docs/RELATO.md`), nao no vivo -- a
    prosa deste bloco cita as tres ancoras e contaria a si mesma, e foi essa contaminacao que
    produziu 4/2/4 na primeira medicao. Molde = o do PROPRIO clone, `s.find('\n## <ancora>')`,
    que e' INICIO DE LINHA: `## DEPLOYS AGENDADOS` = 1, `## NO AR HOJE` = 0,
    `## PENDENTES DO RONALD` = **0** -- esta ultima e a ancora que o clone procurava, e por isso
    ele caia para o EOF. Pelo molde LARGO (substring em qualquer posicao) as mesmas tres dao
    2/0/2: os dois extras sao CITACOES em prosa, e seria esse o numero que diria "a ancora existe"
    sobre uma ancora que nao existe. `vigia_arvore.sh` era o **unico** clone
    da pergunta curada.
  - **RED do selo novo** (`bin/tests/test_vigia_arvore_delega_no_relato.sh`): na copia do HEAD,
    3 achados e `varredura: 1`; com a cura, `varredura: 0`. A varredura e sobre `bin/*.sh` e
    `bin/*.py` **sem allowlist** e nao-recursiva, entao `bin/tests/` fica fora POR ESTRUTURA, nao
    por excecao. O par que morde monta o clone velho em `tmp/mau` e o wrapper em `tmp/bom`.
  - **SEM PROVA EM PRODUCAO, e o motivo e nomeado**: `bin/vigia_arvore.sh:16` sai cedo enquanto
    `esteira.pausada` existir, e a pausa e do Ronald -- nao a levanto. A prova substituta e o
    RED->GREEN do selo mais um smoke do wrapper com a string `python3 -c` LITERAL e `ev.RELATO`
    apontado para uma COPIA: a linha pousou em 237, duas abaixo de `## ALARMES DA ESTEIRA` (235),
    e `git diff --numstat -- app/docs/RELATO.md` provou o arquivo real intacto.
  - **o selo que ficou cego, e o sitio era o FILTRO**: a pasta inteira voltou **1 VERMELHO de 64**
    por causa do commit 1 --
    `test_relato_guarda_pedido_de_patch.sh` recusou com `RED extrator cego:
    core/tests/test_no_relato_tem_secao_pinada.py ... nao rendeu token nenhum`. O tripwire estava
    CERTO em recusar; errado era a PORTA: o universo entrava por `"'RELATO.md'" in src`, criterio
    pela FORMA, enquanto a lei escrita no cabecalho dele diz *"todo `app/*/tests/*.py` que LE
    `docs/RELATO.md`"*. Meu teste monta a propria copia num tmpdir (`os.path.join(d, 'RELATO.md')`)
    e nunca toca o vivo; o membro legitimo escreve `os.path.join(RAIZ, 'docs', 'RELATO.md')`.
    Renomear o meu tmp seria band-aid no sitio errado. A porta nova (`_alcanca_o_vivo`) pergunta
    por **AST** -- literal contendo `docs/RELATO.md`, ou `join` com `docs` E `RELATO.md` entre os
    argumentos --, com par que MORDE proprio: se ela cegasse, o universo esvaziaria CALADO e o selo
    passaria por zero declarado. MEDIDO na copia do HEAD: universo 2 arquivos / rc=1 antes,
    1 arquivo / 4 contratos / rc=0 depois, e o membro legitimo manteve os 4 contratos de nome.
  - **PROVA**: `bin/tests/` inteira na arvore com as tres curas -- **64 selos, 0 vermelho**.

- **COMMIT 5 (marco): a CELULA-TURNO-FECHA e CARIMBADA, e a ordem da fila 1 muda no mesmo ato.**
  O `!` dele de 08/10 responde a pergunta de lei que eu tinha no topo deste RELATO: *"os 4 numeros do
  passo 6 batem no ar, faltou so o verde das duas celulas cair no mesmo commit"* -- o EFEITO no ar
  manda, nao a contagem de commits, e a condicao de CAMINHO nao reprova resultado atingido.
  CONFERIDO NA FONTE antes de carimbar, dentro do `saas_core` (LEI-AKITA 8): `PENDENTES['turno/marcos']`
  = **0**, `PENDENTES['celula/precedencia']` = **0**, `verde=True` nas **6** celulas das duas familias,
  `linha_do_placar()` = **contratos_estruturais: 15/20 verdes**, `total()` = **20**; `6319b10c` e
  `b4372615` provados ANCESTRAIS do HEAD por `git merge-base --is-ancestor`.
- **a ordem da fila 1 e dele, e o marcador acompanhou**: `ORDEM-VIVA-TOPO` passou de
  `PLACAR-ESTRUTURAL` para **O211**, com o aval copiado INTEIRO ao lado. Pelo escopo literal
  (LEI-AKITA 9) a lista dele nao nomeia a O220 nem a O210/O212/O213/O215/O216 -- nao as reinsiro por
  minha conta.
  - **ACHADO no ato de mover, e a lei dele JA responde**: `bin/tests/test_hook_nao_cobra_congelado.sh`
    ficou **VERMELHO** (`o 1o aberto do bloco OBRAS e 'PLACAR-ESTRUTURAL', e o marcador diz 'O211'`),
    porque ainda exige marcador == 1o aberto da TABELA. E a pergunta que o corte dele de **03/10
    19:3x** -- a **O158**, ainda nao construida -- manda mudar, literal: *"o marcador ORDEM-VIVA-TOPO
    passa a ser AUTORIDADE (o selo exige que ele aponte um item que EXISTE e esta aberto)"* e *"o selo
    DEIXA de exigir que o marcador seja o 1o aberto da tabela"*. Lei ESCRITA, entao nao e PAREI
    (PAREI-SO-LEI): a O158 vira commit de INSTRUMENTO proprio, que e o ato minimo para a ordem DELE
    poder pousar. O que eu NAO fiz: retaguear as linhas da tabela para ela concordar com a ordem --
    isso seria eu decidindo estado de item, que e declaracao dele.
  - o `lateral de instrumento so no pouso depois da O211` do aval segue valendo: a fila lateral de
    instrumento (O201, O203, selo do `tabela()`, `bin/dieta_relato.sh`, `bin/ff_pouso.sh`) **nao
    abre** -- a O158 entra porque BLOQUEIA a ordem dele, nao porque eu abri a fila.

- **o que eu NAO fiz, de proposito**: dedup dentro do `no_relato` -- as 115 copias sao 115 EVENTOS
  reais, os 13 chamadores tem semanticas diferentes, e o acumulo e trabalho da DIETA, que passa a
  dever tambem o envelhecimento de linha de alarme DENTRO da secao pinada, pelo carimbo dela. E nada
  no `LEIS.md`: a coluna PROTEGE (a 6a) esta VAZIA na L-062 e na L-079, entao
  `test_lei_protege_sitio` nao cobra citacao aqui (`8 sitio(s) protegido(s) em 81 leis, tocados 0`).
  Fica o lateral: a L-079 nomeia `core/esteira_vigia.py::decidir` na coluna *dono*, nao na PROTEGE.

**OS CINCO POUSOS DA O221, e os dois ultimos nasceram da L-105, nao do `!`.** O `!` de 07/10 19:35
nomeou DUAS raias (a de agente da O139 e a `raia-chamado`); as outras tres pousam pela **L-105**
("raia com suite verde POUSA, e o pouso e PRE-APROVADO salvo o que esta na lista NUNCA PRE-APROVADO"),
e nada nelas e dinheiro, escala, vinculo nem arquivo que prod usa que se apague.

- **pouso 1** (`bb0bd0fa`, 08/10 00:37:34) -- so PRODUTO: o papel `prazo` da O139 entra no main.
- **pouso 2** (`d0625307`, 08/10 01:1x) -- so INSTRUMENTO: o selo do papel `prazo` morde na arvore viva.
  INSTRUMENTO NAO POUSA COM PRODUTO (L-105), e foi ele que me custou 1 h em 04/10.
- **pouso 3** (`52bfc524` + `1f3d616f`, a juncao da `raia-chamado`) -- **caiu as 06:08:15**, pelo cron armado para 06:08
  (`/etc/cron.d/hasner-deploy-o221-pouso3`), e nao por gosto: ela toca `app/api/views.py`, sitio
  declarado de `bin/auth_sitios.txt`, e `bin/janela_auth.sh` barra fatia de auth entre 23:20 e 06:00
  (P0 de 20/09, 82% dos 401 as 00h). Gate temporal = CRON + ARQUIVO, nunca processo do Code.
- **pouso 4** (`265e7e87`, raia `juncao-o137`) -- o ESMERIL de ausencia (O137 / SEXTO-BANCO).
  **VERDE**: `Ran 9944 tests in 1314.155s` -> `OK (skipped=42)`, `rc_suite=0`.
- **pouso 5** (`e7dcd970` + `9b64ee79`, o tip da uniao, raia `juncao-o167`) -- a raia de agente do O167: os quatro cadastros ficam
  SO-LEITURA no Django admin, o gate `ChamadoColaborador.abrir` ganha a trava que a porta do DP ja
  tinha (C2) e o reconciliador passa a correr DEPOIS do cartorio (C3).

**A CADEIA FECHOU, e aqui estao os numeros dela -- tres pousos em 56 segundos de parede.**
`d0625307` -> `1f3d616f` (06:08:04) -> `265e7e87` (06:09:49) -> `9b64ee79` (06:09:49), **todos por
`merge --ff-only`, nada resolvido na hora**, e **um `bin/deploy.sh` por pouso**: rc=0 nos tres.
- **ZERO push no meio**, de proposito e por medicao (L-108, um push por MARCO): `bin/tickets_placar.sh
  --escrever` suja `app/docs/TICKETS.md` sem commitar e `bin/pos_push.sh` COMMITA o derivado -- os dois
  matariam o fast-forward seguinte. Entao `origin/main` ficou em `d0625307` **de proposito** ate aqui, e
  a guarda 1 dos pousos 4 e 5 passou a perguntar `HEAD == BASE`, nao `origin/main == HEAD` (corrigida
  08/10 03:2x, antes de rodar).
- **as tres cascas provadas em cada um dos tres atos**: `saas_core /health/ -> 200`, `saas_ui
  /colaboradores/ -> 302`, `mensageria /health/ -> 200`; selo BUG 128 verde (max_requests=0 nas tres +
  reload agendado no crontab); `importerror_500=0` na hora anterior a cada deploy; prova de casca com
  **599 rotas importadas em 2 urlconf(s)** e 16 estaticos conferidos.
- **o veredito da uniao se amarrou a arvore que ele mediu**: `arvore=9b64ee79` no `.out`, a guarda 7 do
  pouso 5 exigiu que ela fosse ancestral da raia e que o delta `arvore -> raia` estivesse TODO dentro de
  `app/docs/` -- deu **0 arquivo fora de docs**. `Ran 9959 tests in 1326.187s` -> `OK (skipped=42)`.
  E por isso `9b64ee79` nao se emenda: emendar reescreve a arvore que a prova nomeia.
- **nenhum stash foi preciso**: "nenhum arquivo sujo colide com o ff" nos dois pousos a mao. O
  pre-flight de 04:1x previa a colisao do `app/docs/RELATO.md` SO no pouso 3 -- e foi exatamente onde
  ela aconteceu e onde o `git stash pop` saiu limpo. Previsao medida, confirmada.
- **migrations pendentes = 0** nos tres, e a janela de auth respondeu ABERTA no pouso 3 (`janela_auth:
  auth mudou (app/api/views.py app/colaboradores/services/aparelho.py) e estamos em janela permitida`).

**DIVIDA NOMEADA, L-109, e NAO entra neste commit -- a forma da dieta e ato PROPRIO, e isso se mediu.**
O RELATO vivo volta a carregar cabecalho de 03/10, 04/10 e 05/10 (seis dias; o `16/09` e citacao
dentro de um cabecalho de 04/10, nao secao). Eu ia dobrar a dieta neste marco, e o precedente diz que
nao: a ultima foi `cd37557a` **[O220] DIETA-DE-CARGA**, ontem 07/10 22:31 -- **obra propria, aval
literal dele** (*"mede os seis arquivos, move historia para LAPIDES e RELATO-ARQUIVO... Nao toca codigo
nem teste"*), **zero linha de `.py`**, -204,1 KB e 2.014 linhas movidas. Dieta e ato de docs com aval,
nao carona num commit de codigo.
E ela QUER SCRIPT, nao mao, por uma medicao: o RELATO **nao e monotonico**. `alarme_esteira` apenda na
CAUDA de hora em hora e a ULTIMA secao do arquivo e `## DEPLOYS AGENDADOS` -- um corte
"primeiro cabecalho velho -> EOF" arquivaria o alarme e o deploy de HOJE. O corte tem de ser
cabecalho-a-cabecalho, com a data de corte como PARAMETRO NOMEADO (hoje: 05/10 entra ou sai? a
ambiguidade de um dia e, ela mesma, o argumento contra decidir isso em linha).
Vai para a fila 2 como INSTRUMENTO: `bin/dieta_relato.sh` + selo, no pouso de instrumento, onde a
L-105 poe instrumento. Nao ha PAREI aqui -- a lei esta escrita, a decisao e tecnica, fica registrada.

**A JUNCAO DO POUSO 5 TEVE UM CONFLITO, e ele foi resolvido guardando OS DOIS LADOS.**
`app/chamados/models.py::ChamadoColaborador.abrir`: a PORTA do esmeril (C1b, validacao de `urgencia`
em `defaults`) e a TRAVA da raia (C2, `atomic` + `select_for_update` na linha da pessoa). O C1b fica
**FORA** do `atomic` de proposito -- e conferencia de ARGUMENTO, levanta antes de haver leitura ou
trava para desfazer. **PROVA por AST, nao por olho:** `ENCERRADOS`,
`_renascer_por_premissa_viva`, `transaction.atomic`, `select_for_update` e `urgencia_declarada` todos
dentro de `abrir` (124 linhas, eram 63), e `PerguntaDisputa.abrir` INTACTO (72 linhas antes e depois)
-- a juncao nao duplicou metodo. `ruff check` nos 11 arquivos pelo cpuset de teste: `All checks passed!`.

**DOIS ACHADOS DE INSTRUMENTO, medidos nesta madrugada, e os dois sao "ausencia de sinal lida como
sinal bom".**
1. **`bin/regua_tickets.sh` nao ve a copia.** Ele faz `cd "$(dirname "$0")/.."` (linha 16), sempre a
   raiz do repo. Rodado com o cwd dentro de um worktree, `origin/main..HEAD` resolve no MAIN -- range
   vazio -- e a saida e `OK -- 0 citacao(oes) com linha na tabela`. Eu reportei esse verde como prova
   no pouso 4. MEDIDO a mao sobre as raias: 9 citacoes em `juncao-o137`, 8 em `juncao-o167`,
   **sem_linha=0 nas duas**. A conferencia que vale foi essa.
2. **O placar do topo do TICKETS recusaria o push depois do merge.** `.regua_stamp` mora na RAIZ e
   **nao existe em worktree**, entao todo `tickets_placar.sh --escrever` rodado de dentro de uma copia
   escreve o rodape SEM o prefixo `_regua OK (...)`. Medido com os argumentos reais da regua: a arvore
   viva da `fora=0`, e as DUAS raias dao `fora=1` -- exatamente essa linha. E
   `bin/tickets_placar.sh --conferir` e a PRIMEIRA coisa que o `regua_tickets` roda, com `exit 2`: o
   push cairia com o codigo ja no ar. Cura, que entrou nas TRES esteiras: depois do merge,
   `bin/tickets_placar.sh --escrever` da ARVORE VIVA (que tem o stamp) e `regua_tickets` de novo.
   E derivado, nao prosa -- excecao nomeada da L-106.

**A ESTEIRA DO POUSO 5 AMARRA O VEREDITO A ARVORE QUE PUSA.** `fatias_agendadas/o221-pouso5/esteira.sh`,
GUARDA 7: o `.out` da suite carrega `arvore=<sha>` escrito ANTES da suite, e a guarda exige que esse sha
seja ancestral da raia **e** que o delta ate o tip esteja TODO dentro de `app/docs/`. Sem isso um `.out`
verde de antes do merge do pouso 4 pousaria uma uniao nunca testada.

**A UNIAO DOS POUSOS 4 E 5 (`9b64ee79`), e por que ela nao era arrumacao.** Os dois pousos sao
IRMAOS: `265e7e87` (O137) e `e7dcd970` (O167) descendem os dois de `1f3d616f`, o tip da raia-chamado.
Pousando o 4 primeiro, o main vai a `265e7e87` e o `--ff-only` do 5 **deixa de existir** -- ele nao
descende dali. Sem a uniao, o pouso 5 so pousaria por merge feito NA ARVORE VIVA com o deploy depois,
que e a janela da L-107 que custou o lote de cartoes de 30/09. A uniao se fez na COPIA: UM conflito,
`app/docs/TICKETS.md` (as duas raias abriram a sua linha logo depois do separador), resolvido
guardando AS DUAS linhas. **ZERO `.py` em comum** entre as raias, medido por intersecao de
`git diff --name-only` antes de mergear. 42 arquivos, 1922 insercoes sobre `1f3d616f`.

**UM PUSH POR MARCO, E ISSO VIROU CORRECAO DAS TRES ESTEIRAS (08/10 03:2x).** Eu havia posto, nas
tres, um `bin/tickets_placar.sh --escrever` depois do merge -- cura do achado do `.regua_stamp`. Lendo
antes de armar: **`tickets_placar.sh` nao tem `git commit`** (quem commita o derivado e
`bin/pos_push.sh`, chamado so por `bin/push.sh`). Entao o `--escrever` deixa `app/docs/TICKETS.md`
SUJO na arvore viva -- e os pousos 4 e 5 **mudam esse mesmo arquivo**. O `git merge --ff-only`
seguinte seria recusado com *"local changes would be overwritten"*, e a guarda de arvore limpa **nao
veria**, porque ela olha `.py`/`.html`. O mesmo vale para o derivado do `pos_push.sh`: push entre
pousos move o main e mata o ff do pouso seguinte. A forma e a **L-108** ("PUSH: um por MARCO, nao um
por commit"): os tres caem por fast-forward SEM push no meio, e so no FIM se roda **um** `--escrever`
e **um** push -- que de passagem troca tres suites de pre-push (~22 min cada) por uma.
Consequencia nas guardas: a pergunta deixa de ser `origin/main == HEAD` (que seria FALSA de proposito
ate o fim) e passa a ser `HEAD == tip do pouso anterior`. O risco que a guarda velha cobria segue
nomeado e coberto por outra: `bin/deploy.sh` chama `bin/janela_auth.sh origin/main`, e com o main
atrasado o delta contem o `app/api/views.py` da raia-chamado -- quem responde por isso e a guarda da
**janela de auth ABERTA**, e e por isso que o marco inteiro corre depois das 06:00.

**O TERCEIRO ACHADO, e e o que teria matado o marco EM SILENCIO: doc sujo mata o fast-forward.**
`app/core/management/commands/alarme_esteira.py` roda de hora em hora e **apenda no
`app/docs/RELATO.md`** (duas linhas novas de alarme so entre 02:05 e 03:10 desta madrugada). Entao a
arvore viva tem esse arquivo SUJO quase sempre -- e o pouso 3 muda o MESMO arquivo, no TOPO.
`git merge --ff-only` recusa qualquer path localmente modificado que o checkout mudaria
(*"local changes would be overwritten by merge"*), e a guarda de arvore limpa das esteiras **nao ve**:
ela olha `.py`/`.html`, nao `.md`. MEDIDO rodando a propria deteccao contra os tres alvos: colide nos
TRES. A cura entrou nas tres e e **LOSSLESS de proposito** -- nada se descarta e nada volta ao HEAD
por conta propria (isso e `!` dele): guarda o arquivo inteiro e o diff em `logs/pousos/`, poe no
STASH, faz o ff, devolve com `git stash pop`; se o pop conflitar, o deploy SEGUE (L-107) e o conteudo
esta em DOIS lugares. `.py`/`.html` continuam PARANDO o ato na guarda de arvore limpa, que e onde tem
de parar.
LATERAL, para o pouso de INSTRUMENTO (L-105, instrumento nao pousa com produto): esta cura mora em
tres copias dentro de `fatias_agendadas/`, e o defeito e da FORMA DE POUSO da casa, nao destas tres
fatias. Ela tem de virar UMA porta em `bin/` com selo de host -- a mesma historia dos LABELS em
quatro lugares e da suite que nao morava em arquivo (O182).

**O SMOKE DE PROD DO POUSO 5 NASCEU VERMELHO, e isso e o que o faz valer.** Ele e SO LEITURA e
pergunta a MESMA autoridade dos dois selos da raia, nao uma regra propria (TESTEMUNHA LE, NAO
RECALCULA): `post_save._live_receivers(Batida)` para a ordem, e
`has_add/change/delete_permission` + o conjunto de campos EDITAVEIS do `ModelAdmin` registrado para o
admin. Rodado contra o codigo NO AR **antes** do pouso: **VERMELHO, 9 falhas**.
  PROVA: `logs/pousos/smoke_pouso5_RED_antes.txt` (2040 B, 08/10 03:31) -- `SMOKE_POUSO5=VERMELHO  falhas=9`, medido chamando `post_save._live_receivers(Batida)` e os `ModelAdmin` REGISTRADOS no
  `saas_core` que atende prod, nao uma replica da regra.
  - (a) `cartorio=4` e `reconciliador=3` -- o CONSUMIDOR corre ANTES do JUIZ, e e literalmente o que
    a secao 4a proibe: ele le `celula.veredito` da batida ANTERIOR.
  - (b) os quatro cadastros com `add=change=delete=True` e **17 / 15 / 17 / 51** campos editaveis.
O universo nao e vazio (51 campos no Colaborador), entao o verde depois do deploy e sinal, nao
ausencia de sinal.
  **RODADO DE NOVO as 06:10:3x, contra o codigo NO AR depois do pouso 5: `SMOKE_POUSO5=VERDE`.**
  PROVA: `logs/pousos/smoke_pouso5_VERDE_depois.txt` (1038 B, 08/10 06:14) -- `SMOKE_POUSO5=VERDE  (a)
  ordem dos receivers OK (b) 4/4 cadastros so-leitura, 0 campo editavel`, contra
  `logs/pousos/smoke_pouso5_RED_antes.txt` (2040 B, 08/10 03:31) = `VERMELHO falhas=9`. Mesmo script,
  mesma autoridade, dois arquivos no disco.
  (a) a ordem virou -- `cartorio=3` e `reconciliador=4`: o JUIZ julga ANTES de o CONSUMIDOR ler, na
  lista de cinco receivers que o `post_save(Batida)` devolve. (b) os quatro cadastros em
  `add=False change=False delete=False`, **0 campo editavel** nos quatro -- os 17/15/17/51 de 03:31
  foram a ZERO. O mesmo script, a mesma autoridade, o numero oposto: era o que faltava provar.
  Script em `logs/pousos/smoke_pouso5_prod.py` (nao no scratchpad, como esta linha dizia as 03:4x).

---

## ALARMES DA ESTEIRA

**09/10 03:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 04:45.

**08/10 22:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 00:00.

**08/10 07:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**08/10 05:55 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**08/10 04:50 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**08/10 03:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 04:45.

**08/10 03:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**08/10 02:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**08/10 01:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**08/10 00:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 22:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 00:00.

**07/10 21:45 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 20:40 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 19:35 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 18:35 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 17:30 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 16:25 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 15:25 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 14:20 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 13:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 12:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 11:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 10:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 09:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 08:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 07:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 05:55 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 04:50 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 03:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 04:45.

**07/10 03:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 02:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 01:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**07/10 00:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 22:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 00:00.

**06/10 22:30 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 21:30 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 20:30 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 19:30 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 18:25 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 17:25 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 16:20 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 15:20 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 14:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 13:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 12:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 11:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 10:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 09:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 08:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 06:55 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 05:55 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 04:50 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 03:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 04:45.

**06/10 03:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 02:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 01:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**06/10 00:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 22:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 00:00.

**05/10 21:40 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 20:35 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 19:30 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 18:25 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 17:25 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 16:20 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 15:20 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 14:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 13:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 12:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 11:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 10:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 09:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 08:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 07:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 05:55 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 04:50 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 03:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 04:45.

**05/10 03:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 02:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 01:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**05/10 00:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 22:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 00:00.

**04/10 21:40 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 20:40 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 19:35 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 18:35 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 17:30 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 16:30 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 15:25 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 14:20 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 13:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 12:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 11:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 10:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

## ADENDO 03:42 -- O VEREDITO DA UNIAO E A CADEIA SIMULADA

**A suite da uniao fechou VERDE**: `Ran 9959 tests in 1326.187s`, `OK (skipped=42)`, `rc_suite=0`,
zero linha `^ERROR:` ou `^FAIL:`. Lido por TEXTO, nunca por rc de tarefa. O arquivo
(`suite_uniao_o167_o137.out`) carimba na linha 2 `arvore=9b64ee792631adbb3b0282ed61a945a0f2c974ea`,
que e a arvore MEDIDA -- e por isso `9b64ee79` nao se emenda mais: a GUARDA 7 do pouso 5 compara o
carimbo com o HEAD da raia e com o delta fora de `app/docs/`, e um `--amend` orfanaria o numero.

**A cadeia dos tres pousos foi simulada INTEIRA em clone, com os sujos reais** (03:41,
`logs/pousos/simulacao_cadeia_pousos_20261008.md`): os tres sao fast-forward a partir de `d0625307`
na ordem, e o unico sujo que colide e `app/docs/RELATO.md` no pouso 3 -- cujo `git stash pop` saiu
**LIMPO**, auto-merge, zero marcador, stash dropped. Pousos 4 e 5 nao colidem com sujo nenhum.
Isso nao e garantia: o vigia apenda na CAUDA do RELATO as 04, 05 e 06h e o pouso 3 mexe no TOPO,
entao o 3-way segue limpo pela mesma razao que foi medida -- mas foi medida as 03:41. Se o pop
conflitar, o ato NAO se corrompe: o merge e o deploy seguem no mesmo ato (L-107), o conteudo fica em
DOIS lugares (stash + `logs/pousos/`) e o pouso seguinte PARA na propria guarda, porque o git recusa
fast-forward com path `UU`. Falha segura, nao falha silenciosa.

---

## PERGUNTA DE LEI (vai no TOPO, com o numero -- e a esteira SEGUE, PAREI-DE-LEI-NAO-DEVOLVE-TURNO)

**A celula `('chamado','um juiz por pergunta')` esta presa a lei sua, nao a codigo.** Medido as
04:0x contra o estado FINAL dos tres pousos (`9b64ee79`, censo em
`logs/pousos/censo_jpv_20261008.txt`):

O portao e `core/tests/test_contract_tabuleiro.py:42` -- `assertFalse(cel['verde'] and
C.JUIZES_POR_VARREDURA)` --, ou seja verde exige a lista **VAZIA**. Ela tem **25 nomes**. E
**pelo menos 3 desses 25** estao na sua lista *"ESPERAM LEI MINHA, nao tocar"* (aval
JUIZ-DE-CHAMADO 04/10 00:1x, secao E, *"seis emissores esperam LEI dele e nao se tocam"*):
`supra_juiz` (O168), `detectar_cluster_espurio` (O169) e `fechar_cobranca_com_lastro` (aval
ABERTO `LASTRO-MEDE-DUAS-VEZES`). Os outros tres da secao E nao estao enumerados em doc nenhum
que eu ache por grep.

E o mesmo aval PROIBE a saida facil: *"tirar nome da lista so trocando de dict"*. Entao nao ha
caminho de execucao que esvazie a lista sem tocar arquivo proibido -- e a celula nao fecha.

**A pergunta, em uma linha:** os tres proibidos saem da lista por RECLASSIFICACAO de papel com o
corpo PROVADO (sem mudar regra de negocio), ou a celula fica PARCIAL de proposito ate os O168/O169
e o `!` do lastro?

**O QUE EU SIGO FAZENDO SEM A RESPOSTA, e e por isso que isto nao devolve turno:** dos 25, **13**
nao mostram sinal de derivacao propria e importam autoridade da casa. Esses se LEEM um por um e,
quando o corpo confirmar que delegam, migram para `vigia`/`prazo` em commit proprio com a medicao
na sombra pegando ZERO -- que e a forma que o seu aval manda. A lista **so encolhe**, por desenho:
a divida cai antes da celula fechar. Nenhum dos 13 esta entre os tres proibidos.

### LATERAL RETRATADA -- o `inicio` derivado do escalonamento de documento NAO e defeito (medido 03:5x)

Eu levantei, lendo `ponto/services/ausencia.py::escalonar_aguardando_documento`, que ele **deriva** o
comeco da janela em vez de ler um gravado:

```
inicio = a.prazo_documento - datetime.timedelta(days=PRAZO_DOCUMENTO_DIAS)
dias   = (hoje - inicio).days
if dias in ESCALONAR_EM:  _cobrar_documento(a, dias)
```

e suspeitei da classe "selo que copia valor": prazo gravado com uma constante, menos a constante de
HOJE, da um comeco fabricado. **Medi e esta errado -- eu estava errado, nao o codigo.** Tres provas:

1. **A constante nunca se moveu.** `git log -S'PRAZO_DOCUMENTO_DIAS = '` da **UM** commit (`d5a2f4fe`,
   [A4]) e o valor nasceu **7**. `ESCALONAR_EM` igual: um commit, `(0, 2, 7)`.
2. **Censo de escritores FECHADO: 4 sitios, todos no mesmo modulo, todos com a mesma soma** --
   `:225` (nascimento), `:384`, `:889` sao `localdate() + timedelta(days=PRAZO_DOCUMENTO_DIAS)`, e
   `:650` escreve `None`. **Nenhum humano digita o prazo**: zero form, zero template, zero input.
   A subtracao e o inverso EXATO de todo escritor que existe.
3. **As duas migrations que reescreveram prazo gravado fazem a mesma soma, de proposito.** A 0059
   calcula `novo = inicio + PRAZO_DIAS` e **imprime `D0 em %s`**: ela codifica o D0 DENTRO do prazo
   somando 7, justamente para que o leitor recupere o D0 subtraindo 7. A 0058 idem (`novo_prazo`).

A minha sonda tinha comparado o `inicio` derivado contra `Ausencia.data` e achado **20 de 20 fora**
(deriva de +1 a +5 dias) -- numero verdadeiro, pergunta errada: o relogio do documento comeca no dia
em que o documento foi PEDIDO, nunca no dia da ausencia, e e por isso que a 0058 se chama "o relogio
recomeca". Foi o meu proprio erro de [[criterio-pela-forma-conta-errado]]: perguntei a um campo
vizinho em vez de perguntar a autoridade.

**Fica so a exposicao, nomeada, sem fatia:** o invariante "todo escritor de `prazo_documento` soma
`PRAZO_DOCUMENTO_DIAS` a um D0" e verdadeiro **por construcao** e nao esta escrito em nenhum selo. Se
algum dia um quinto escritor gravar um prazo sem essa soma -- ou a constante mudar sem migration que
reescreva o gravado --, o `inicio` de `:791` passa a fabricar o degrau. Hoje: 20 na fila viva, 11 ja
vencidos, 2 cairiam num degrau hoje. **Nao construo selo para isso agora** (nao e fila 1, e a casa ja
reprovou selo sem caso que morde); fica a linha.

### DIVIDA DO PROPRIO MARCO -- a linha da L-101 no LEIS.md mente sobre os pousos 1 e 2 (medido 04:0x)

`app/docs/LEIS.md:127` ainda diz, em TRES celulas, que o papel `prazo` nao existe -- e ele entrou no
main hoje de manha, pelas minhas duas maos:

| celula | o que esta escrito HOJE | o vivo |
|---|---|---|
| [4] dono | `NENHUM: config/crons.py::PAPEIS tem 7 papeis e nenhum e prazo; os 27 crons de varredura seguem como juiz` | `PAPEIS` tem **8** e um e `prazo` (`config/crons.py:1426`, commit `bb0bd0fa` = pouso 1) |
| [6] selo | `NENHUM: falta o selo por AST da O139` | `bin/tests/test_papel_prazo_nao_deriva.sh` existe e **passa** (commit `d0625307` = pouso 2) |
| [7] estado | `SO-NO-PAPEL -- 0 de 2 clausulas no codigo, 0 de 2 com teste que morde` | as 2 clausulas estao no codigo e o selo morde |

Rodei o selo agora: `papel_prazo: 2 cron(s) de papel 'prazo'; juizes_por_varredura=25` -> `OK`.

**NAO corrigi a linha neste turno, e o motivo e lei, nao preguica.** O ESTADO so se muda no commit que
muda o codigo ou o teste daquela lei -- e esses dois commits ja fecharam. Entao a correcao **anda no
commit de fechamento deste marco** (L-106: docs do marco entram no commit do marco), com o texto ja
pronto acima. Conferi antes que ela nao atrapalha a cadeia: **nenhuma das tres juncoes toca
`app/docs/LEIS.md`** (`git diff --name-only HEAD <ramo> -- app/docs/LEIS.md` = 0 nas tres), entao
nao ha risco de recusar um `--ff-only`. A celula **[5] PROTEGE continua VAZIA e eu nao a toco**: ela
esta na sua lista de colunas que nao sao minhas. Ela hoje mereceria `config/crons.py::PRAZO_DELEGA_A`.

### E O QUE DEIXOU A MENTIRA DURAR: o selo do PROTEGE so alcanca 5 das 81 leis

Os dois selos que leem o `LEIS.md` passaram VERDES com a linha 127 mentindo:
`test_leis_indice: OK -- 81 leis indexadas` e `test_lei_protege_sitio: OK -- 8 sitio(s) protegido(s)
em 81 leis ... tocados neste push: 0`. Medido na tabela:

- **81** linhas de lei; **76** com a coluna PROTEGE **VAZIA**; **5** preenchidas (que carregam os 8 sitios).
- **20** linhas com a coluna `selo` em `NENHUM`/vazia.
- vereditos: `PELA-METADE` 32 · `CONDUTA` 25 · `SEM-PROVA` 14 · `INTEIRA` 6 · `SO-NO-PAPEL` 4.

`test_lei_protege_sitio.sh` cobra a lei **cujo sitio aparece no diff do push**. Lei com PROTEGE vazio
nao tem sitio, logo **nunca** e cobrada: o guarda alcanca **5 de 81**. E nenhum selo le as celulas
[4]/[6]/[7] contra o vivo -- ninguem pergunta "esse `NENHUM` ainda e verdade?". E a familia do SELO
ANTI-VACUIDADE do CLAUDE.md secao 6: passa por **ausencia de sinal**. **Nao construo o selo agora**
(nao e fila 1, e instrumento, e instrumento nao pousa com produto -- L-105); fica o numero.

### CENSO R6 item 4 -- o criterio do papel, lido na fonte, ja descarta 3 dos 13

Li o bloco declarado (`config/crons.py:1400-1505`) em vez de classificar por conta propria. Dois
achados que mudam a lista de candidatos que eu tinha:

1. **TRES dos meus 13 "sem derivacao propria" estao EXPLICITAMENTE barrados pelo proprio comentario**:
   `detectar_ausencias`, `processar_alertas_turno` e `silenciar_chamados_isentos` seguem em `juiz`
   porque leem CARIMBADORES de prazo futuro (`motor.prazo_de_cobranca`, `motor.PRAZO_ARQUIVO_DIAS`) --
   *"eles EMITEM, nao cobram prazo"*. Candidatos reais: **10**, nao 13.
2. **`escalonar_documentos_ausencia` e o candidato mais limpo**, e tem a MESMA forma do `vigia_de_hora`
   que ja ocupa o papel: o comando e um wrapper de 30 linhas que **nao deriva nada** (so chama
   `ponto.services.ausencia::escalonar_aguardando_documento`), e a pergunta "venceu?" e UMA linha
   lendo o GRAVADO (`if hoje > a.prazo_documento`). A regra de prazo mora inteira na funcao de
   servico -- que e exatamente onde o cadastro `PRAZO_DELEGA_A` manda ela morar (`vigia_de_hora` tem
   `TETO_MARCO_MIN` dentro da autoridade dele, nao no cron). Linha candidata:
   `'escalonar_documentos_ausencia': {'corpo': 'escala.management.commands.escalonar_documentos_ausencia::handle',
   'autoridade': 'ponto.services.ausencia::escalonar_aguardando_documento'}`.
   **Nao migrei neste turno**: isso edita `config/crons.py` na arvore viva com tres `--ff-only`
   pendentes em cima dela. Vai no turno seguinte ao marco, em commit proprio, com o selo rodado.

### CORRECAO DO PROPRIO CENSO -- o candidato do item (2) acima NAO passa, e a razao e a TRAVA JUIZ-NOVO

Reli antes de publicar e **retiro a palavra "candidato mais limpo"** que escrevi ha quinze minutos.
`escalonar_documentos_ausencia` **nao pode ocupar o papel `prazo` por execucao minha**, e a prova e a
forma das DUAS entradas que ja o ocupam:

- `processar_cartorio`: corpo em `ponto.management.commands.processar_cartorio`, autoridade em
  **outro modulo** (`ponto.services.cartorio::julgar_colab`);
- `vigia_de_hora`: corpo em `ponto.services.vigia_de_hora`, autoridade em **outro modulo**
  (`ponto.services.cartorio::marcos_vencidos`), e `marcos_vencidos` **esta** em `JUIZES_DO_PRAZO`.

Nas duas o ponteiro sai da familia do cron e aponta para o cartorio, que e juiz independente. A minha
proposta era **wrapper -> o proprio trabalhador dele**: `escalonar_aguardando_documento` e quem MORA
com as constantes de prazo (`PRAZO_DOCUMENTO_DIAS`, `ESCALONAR_EM`, `:606-607`). Nomea-la "autoridade"
nao delega nada -- renomeia. E `grep` nos dois registros da casa:
**`escalonar_aguardando_documento` NAO esta em `core/juizes.py` nem em `JUIZES_DO_PRAZO`.** Registra-la
seria **juiz nascendo**, que exige `corte Ronald: juiz <nome> nasce` (TRAVA JUIZ-NOVO). Logo isto e
**PERGUNTA DE LEI, nao fatia** -- e pela PAREI-DE-LEI-NAO-DEVOLVE-TURNO vai ao topo com o numero
(10 candidatos reais, este e 1) e a esteira segue.
O selo `test_papel_prazo_nao_deriva.sh` teria passado VERDE nessa migracao, porque ele inspeciona
**corpo + arquivo do comando** -- e o comando e um wrapper de 30 linhas que de fato nao deriva nada.
Mesma vacuidade que eu acabei de medir no `LEIS.md`: o selo aprova por **ausencia de sinal**.

**E um achado de brinde, do meu proprio O139:** `julgar_colab` -- autoridade de `processar_cartorio`,
um dos dois unicos ocupantes do papel -- **nao esta em `JUIZES_DO_PRAZO`**, que se declara *"os juizes
do prazo da casa: quem responde 'o prazo venceu?'"*. A lista tem 3 chaves
(`prazo_estourou`, `prazo_dp_dias`, `marcos_vencidos`) e nenhuma e dele. Nada cobra isso: o selo le
`PRAZO_DELEGA_A`, nunca a pertinencia cruzada. Uma linha, nao uma obra -- fica nomeada.

### E O VEREDITO DA L-101 NAO E MEU PARA DAR

Eu ia escrever `INTEIRA` na celula [7] e **nao escrevo**: quem fez a divida nao se da a nota. Pela
definicao da propria tabela (`PELA-METADE` = *"alguma clausula nao esta, ou nao esta inteira, no
codigo"*), a clausula 1 da L-101 e UNIVERSAL -- *"cron que julga a ausencia de um fato ate um prazo
declara papel `prazo`"* -- e hoje **2 crons** o declaram contra **10 candidatos reais** que seguem em
`juiz`, mais os 2 destinados que o proprio comentario diz que nao o ocupam. Entao a celula [7] recebe
o que o selo PROVA, e o veredito segue a definicao:
`PELA-METADE -- papel criado e selo por AST mordendo (bin/tests/test_papel_prazo_nao_deriva.sh: 2 cron(s)
de papel prazo, juizes_por_varredura=25); a clausula 1 alcanca 2 de 12 crons que julgam ausencia ate
prazo. Medido 08/10 em <sha do commit do marco>.`

### GUARDA CONFERIDA NA CADEIA DAS 06:08 -- migration (fechado, 04:0x)

A simulacao da cadeia exercitou **git** (ff + stash), nunca o `bin/deploy.sh`. Faltava uma guarda e
ela esta fechada: **as tres juncoes tem 0 migration** (`git diff --name-only HEAD <ramo> | grep -c
'/migrations/'` = 0, 0, 0) e as tres esteiras passam **`--sem-migrate`** (pouso3:128, pouso4:131,
pouso5:189) -- que e exatamente o que o CLAUDE.md manda quando a fatia nao tocou modelo. Importa
porque `sombra.sh --conferir` **nao le carimbo**: ele re-pergunta ao vivo (`:253-263`, `psql` em prod
e na sombra, `d_mig` por `diff`). Se alguma juncao migrasse em prod depois do refazer das 04:17, o
portao ficaria VERMELHO para os pousos 4 e 5 -- o caso de 03/10 04:0x, que e meu. Com 0 migration,
`django_migrations` de prod nao se move e o carimbo nao pode virar sob a cadeia.

### R6 item 4 -- A RESPOSTA JA ESTAVA ESCRITA, e ela diz que a celula NAO fecha por execucao (04:1x)

Antes de propor corte eu fui ao `CORTES.md`/`CORTES.json` (LEI-AKITA 4) e a pergunta que eu estava
montando **ja foi feita e ja foi respondida**. `docs/CORTES.md:99`, no proprio
`CHAMADO-VARREDURA-NAO-JULGA` (aberto 25/09, RESPONDIDO 03/10 pelo `PAPEL-PRAZO-NASCE`), em texto dele:

> *"Nao e fatia: e a divida de desenho que o proprio 4a declara. **Cada cron sai da lista quando a
> celula agendar o proprio marco** (evento programado na hora do marco + 30 min, como o `vigia_de_hora`
> ja anota na sua linha)."*

Entao a lista `JUIZES_POR_VARREDURA` -- e com ela a celula `('chamado','um juiz por pergunta')` --
esvazia por **DOIS caminhos, e nenhum e relabel**:
1. o cron cuja pergunta e um **PRAZO** vai para o papel `prazo` (L-101, a 4a opcao);
2. **todo o resto** sai quando a **CELULA AGENDAR O PROPRIO MARCO** -- varredura vira EVENTO. Isso e
   **desenho**, declarado pelo proprio CLAUDE.md 4a, e ele escreveu "nao e fatia".

Li tres candidatos inteiros e eles confirmam o caminho 2, nao o 1:
- **`apurar_furos_diarios`** (21 linhas): wrapper puro, nao deriva nada, chama
  `escala.services.furos_diarios::apurar` + `chamados.services.adesao::sincronizar_adesao` +
  `chamados.services.limbo_folgas::agrupar_furos_sob_limbo`. **Nao e consumidor: ele E a autoridade** --
  o proprio `crons.py` diz, na linha da `lavra`, *"quem decide o que e furo e o apurar_furos_diarios,
  que ela le"*. Nao sai da lista por delegar: so sai quando o furo nascer de evento.
- **`fechar_chamados_ausencia`** (43) e **`reavaliar_ausencias_lancadas`** (48): os dois delegam
  CERTO hoje -- leem `VIVOS` (`chamados.catalogo.motor`), `ABERTAS`
  (`ponto.services.ausencia`), `reconciliar_por_ausencia`/`universo`
  (`chamados.services.silencio_ausencia`), e **nao derivam nada**. Eles julgam **pelo FATO** (a
  Ausencia decidida), **nunca por prazo** -- o docstring do primeiro e explicito: *"Fecha pelo FATO,
  nunca pelo clique"*. Logo o papel `prazo` **nao os acolhe**; o destino deles e o sinal da celula.

**O que isso muda na minha propria conta**: dos 25 nomes, o papel `prazo` nao e o destino da maioria.
Ele e o destino dos que cobram PRAZO -- e a lista de candidatos fortes e **dele**, escrita no
`CORTES.json` quando pediu a O139: `detectar_ausencias`, `vigia_de_hora`,
`escalonar_documentos_ausencia`, `apurar_furos_diarios`, `detectar_intervalo_ausente`,
`marcar_foto_ausente_retro`, `disparar_perguntas_competencia`, `fechar_becos_disputa` (8), com um 9o
sugerido (`processar_alertas_turno`), e a ordem literal *"o numero publicado sera o MEDIDO, nao o que
couber no 8"*. **Medido, 2 ocuparam e 3 foram barrados com razao escrita** (`detectar_ausencias`,
`processar_alertas_turno`, `silenciar_chamados_isentos` leem carimbador de prazo FUTURO: emitem, nao
cobram). Dos restantes da lista dele, `escalonar_documentos_ausencia` e o caso examinado acima: a
pergunta "venceu?" existe mas e **inline e sem nome** (`hoje > a.prazo_documento`), e as tres chaves de
`JUIZES_DO_PRAZO` sao todas PREDICADOS nomeados (`prazo_estourou`, `prazo_dp_dias`,
`marcos_vencidos`). Ocupar o papel honestamente exige **batizar o predicado** -- e isso e juiz novo,
com a frase do CORTES.md, nao implementacao minha.

**Conclusao que vai ao topo do RELATO, com numero:** a celula `('chamado','um juiz por pergunta')`
esta parada por **lei dele, em dois degraus** -- (i) 3 dos 25 nomes estao na lista "ESPERAM LEI MINHA,
nao tocar" e o aval proibiu *"tirar nome da lista so trocando de dict"*; (ii) o caminho que o proprio
corte declara para os demais e **a celula agendar o proprio marco**, que ele classificou como
*"divida de desenho, nao fatia"*. **Nao ha o que eu execute aqui sem o corte dele.** O que eu fiz e o
que cabia: o censo dos 25, o criterio lido na fonte, e os 3 barrados com razao escrita. Pela
PAREI-DE-LEI-NAO-DEVOLVE-TURNO isso vai ao topo com os numeros e a esteira **segue para o proximo
item que nao depende dessa lei** -- nao devolvo turno por isto.

### PRE-FLIGHT DA CADEIA, FECHADO (04:1x) -- e por que SO o pouso 3 colide

Duas medicoes minhas pareciam discordar e **nao discordam**; registro a reconciliacao porque a
diferenca e exatamente o tipo de coisa que morde numa corrida sem ninguem olhando:

- medindo **da HEAD de agora** (`d0625307`), as TRES juncoes tocam `app/docs/RELATO.md` -- que e o
  unico sujo que colide (os outros tres sujos, `app/config/crons_duracao.json`,
  `app/docs/HANDOFF-SESSAO.md` e `bin/sombra.sh`, nao sao tocados por nenhuma);
- medindo **em ORDEM** (a simulacao das 03:4x), os pousos 4 e 5 colidiam com **nada**.

A causa: a cadeia e **ANINHADA**, nao paralela --
`juncao-chamado` (`1f3d616f`) **esta contida em** `juncao-o137` (`265e7e87`), que **esta contida em**
`juncao-o167` (`9b64ee79`). E o `git diff juncao-chamado juncao-o137 -- app/docs/RELATO.md` da
**0 linhas** (idem para o o167). Ou seja: **o pouso 3 leva o RELATO inteiro da cadeia**, e os pousos 4
e 5 nao tocam esse arquivo. Logo a unica janela de stash/pop da corrida e a do pouso 3 -- a que a
simulacao exercitou e que devolveu **pop LIMPO**.

O bloco de stash do pouso 3 foi relido linha por linha e **falha SEGURO** nos tres sentidos:
`.py`/`.html` sao EXCLUIDOS da lista de stash de proposito (`grep -vE '\.py$|\.html$'`), entao um
`.py` sujo que colidisse faria o `--ff-only` **RECUSAR** em vez de passar; `stash push` que falha
**PARA antes do merge** (`exit 1`, nada mergeado); e `stash pop` que conflita escreve ATENCAO, deixa o
conteudo **no stash E em `logs/pousos/`** com selo de hora, e o merge + deploy **SEGUEM** (L-107).

Resto do pre-flight: **disco 249 GB livres (21% usado), inodes 3%** -- o `collectstatic` do deploy e
os tres merges cabem. Migration: **0 nas tres**, `--sem-migrate` nas tres (ja registrado acima).

---

## PLACAR-ESTRUTURAL — o instrumento do item media o proprio envelhecimento, e nao media

**O defeito esta na ORIGEM, e nao e "um numero errado".** `app/core/placar_estrutural.py` e o placar
PRINCIPAL do `ESTADO.md` (L-099) e **nao tem UM selo**: nenhum teste le o arquivo, e o unico consumidor
e `bin/gerar_estado.py:25-62`, que o **publica sem o conferir** (carga standalone no host, sem Django,
dentro de `except Exception` para "o ESTADO nunca cair por causa do placar"). O proprio `placar()`
(`:350`) so pergunta `prova_faltando = bool(numero) and not prova` — **ninguem nunca pergunta se o
numero ainda concorda com a `fonte` que a celula declara**, embora o docstring do arquivo diga que a
`fonte` existe exatamente para isso: *"Sem fonte, o numero envelhece em silencio, que e o defeito que o
proprio R4 descobriu (o selo existia e ninguem o rodava)"*.

**MEDIDO: 4 dos 8 numeros do R6 estavam velhos** (remedidos chamando a FUNCAO REAL, LEI-AKITA 8; provas
em `logs/placar_estrutural/`):

| o que a celula dizia | a autoridade diz | fonte da remedicao |
|---|---|---|
| `CONTRATOS: 14/20 verdes` | **15/20** | `core.contratos_estruturais.linha_do_placar()` · `contratos_20261008.txt` |
| `Faltam **6** atingiveis` | **5** (`total() - verdes()`) | idem, as 5 nomeadas no mesmo arquivo |
| `os 27 de config.crons` | **25** | `numeros_do_r6_20261008.txt` |
| `122 escritas fora de porta` | **121** (arvore viva, hoje) | `censo_escritas_20261008.txt` |

O `15/20` ja era verdade desde **`b4372615`** (o marco O218 fechou a celula do juiz da celula) e a
prosa seguiu em `14/20` por tres dias **sem nada ficar vermelho**. A propria celula confessa a doenca
tres vezes no texto (*"ESTA LINHA DIZIA 13... 12... 19 e 9"*) — ela sabia que envelhecia e nao tinha
quem a cobrasse.

**O N/20 era uma SEGUNDA VERDADE confirmada.** Quem publica o numero e **UM SO**:
`app/core/placar_tickets.py:213`, que o le do juiz e acerta desde sempre (`TICKETS.md:30` =
`contratos_estruturais | 15/20 verdes | **20/20**`). A prosa do R6 carregava uma copia COMPETINDO —
a mesma `TOTAL` segunda-verdade que a **L-100** matou neste mesmo arquivo-familia (*"nao ha constante
TOTAL... foi o que o `# 22` em comentario fez por 20 dias"*).

**A cura, por isso, nao e "corrigir para 15".** O R6 **nao pode derivar `total()` no lugar**, e o
motivo eu tinha ERRADO ate 05:3x de hoje: eu escrevi "porque o placar carrega sem Django", e **o juiz
responde sem Django nenhum** — `linha_do_placar()` devolve `15/20` com `DJANGO_SETTINGS_MODULE` vazio.
O que bloqueia e o **sys.path**: `bin/gerar_estado.py:137` insere so `raiz/bin` e carrega este arquivo
por CAMINHO, e o arquivo **nao tem UM import** (zero, por AST). Um `from core import
contratos_estruturais` ali levanta `ModuleNotFoundError`, o `except` de `:46` o engole ("o ESTADO nunca
cai por causa do placar") e o placar **PRINCIPAL vira `_indisponivel_`: as SEIS linhas somem em
silencio** para publicar uma. Causa falsa importa porque ela sobrevive ao commit: quem lesse "e por
causa do Django" tentaria `django.setup()` no renderizador e nao entenderia por que nao resolve.
PROVA: logs/placar_estrutural/por_que_r6_copia_a_frase_20261008.txt (simulacao exata com
`env -u PYTHONPATH`, carga por `spec_from_file_location` como o consumidor faz, `sys.path tem app/ =
False`, `ModuleNotFoundError: No module named 'core'`), e a correcao da mesma causa falsa em
`logs/pousos/msg_marco_placar.txt`. Entao o ato de PRODUTO faz a celula **carregar a frase do juiz,
VERBATIM**
(`contratos_estruturais: 15/20 verdes`, copiada de proposito para o selo poder exigi-la), e o ato de
INSTRUMENTO seguinte (L-105, pouso proprio) adiciona o selo que cobra a CONTENCAO — depois dele,
**fechar uma celula deixa a linha VERMELHA** em vez de a deixar envelhecer calada.

**RED -> GREEN, as duas pontas evidenciadas:**
- RED na arvore viva: `logs/placar_estrutural/red_frase_do_juiz_20261008.txt` -> `SELO=VERMELHO falhas=1`
- GREEN na copia curada: `SELO=VERDE -- o R6 publica a frase do juiz, verbatim`
- e o CONSUMIDOR REAL ainda carrega: standalone no host, sem Django, `R6 PARCIAL prova_faltando=False`,
  contem a frase do juiz = `True`, ainda contem o `14/20` velho = `False`.
- o selo MORDE por desenho: a pergunta e contencao, nao parse de prosa — `14/20` na prosa = VERMELHO,
  e o dia que uma celula fechar, VERMELHO de novo.

A cura esta retida em `logs/pousos/placar_estrutural.py.curado_r6` (+ `placar_estrutural_r6.diff`,
4 edicoes, `py_compile` limpo) e **nao foi escrita na arvore**: um `.py` sujo sob `app/` faz a GUARDA 2
do encadeado das 06:08 dizer `PAROU`. Ela entra no **commit do marco** (LEI-AKITA 10). O rascunho do
selo esta em `logs/pousos/test_r6_publica_a_frase_do_juiz.py.rascunho`, fora de `app/` de proposito —
qualquer suite de pouso o rodaria RED a partir da arvore viva.

**O numero da celula 4 ESPERA, de proposito.** Os tres numeros em conflito se reconciliam pela funcao da
propria casa: **122** = 25/09 (o que a celula diz, velho), **121** = hoje, arvore viva (o "antes" do
aval), **21** = o que a juncao da `wt-esmeril2` promete. Escrever `121` agora seria datar um numero que
**o meu proprio pouso 3 muda em horas**: ele se mede UMA vez depois dos pousos, com o censo pousado e as
portas pousadas do MESMO commit, e vai nos DOIS sitios no mesmo ato (`placar_estrutural.py:312-313` e
`contratos_estruturais.py:371`). **Guarda de vacuidade ja escrita**: `na_porta + fora` tem de sair
~133 ou mais; bem abaixo de 133 sem um `apagar` correspondente significa sitio que **saiu do censo** em
vez de ter entrado numa porta, e a celula tera de dizer isso. A juncao foi conferida contra essa suspeita
e o instrumento ficou **mais largo, nao mais frouxo** (`relacoes`/`_colher_relacoes`/`_raiz_relacionada`,
+118 no censo e +314 em `portas.py`, **zero exclusao nova**).

**PERGUNTA DE LEI (NAO e PAREI e NAO devolve turno — sobe com numero):**

**(b) A porta que a regra do ESTADO abre fecha atras do commit, e a linha da L-101 ficou mentindo.**
A regra permanente e "nao mudar um ESTADO do LEIS.md fora do commit que muda o codigo ou o teste
daquela lei". No caso da L-101 (O PRAZO E UM PAPEL, NAO UM JUIZ) os dois commits que mudaram o codigo
E o teste **ja foram empurrados sem tocar o LEIS.md** -- medido: `git show --name-only bb0bd0fa --
app/docs/LEIS.md` e o mesmo em `d0625307`, **vazio nos dois**. Hoje a linha 127 diz, em tres celulas
diferentes, o contrario do que a fonte responde: [4] "PAPEIS tem 7 papeis e nenhum e prazo" contra
`config/crons.py:1426` = **8** papeis **com** `prazo` (e `PRAZO_DELEGA_A` delegando em `:1504`); [6]
"falta o selo por AST da O139" contra `bin/tests/test_papel_prazo_nao_deriva.sh`, **329** linhas,
pousado em `d0625307`; [7] "Auditado 05/10 em c8031f6", de antes dos dois pousos.
**Eu NAO corrigi**, e e por isso que isto e pergunta e nao fatia: o marco do PLACAR-ESTRUTURAL nao
toca o papel `prazo`, entao corrigir ali seria exatamente o que a regra proibe. O texto medido e as
tres celulas propostas estao prontos em `logs/pousos/l101_linha_mede_velho.md`, com a coluna PROTEGE
**vazia** (ela e sua) e com o aviso de que `PELA-METADE` e proposta: o `0 de 2 clausulas` e de 05/10 e
quem aplicar remede antes, porque trocar numero velho por numero velho nao e cura.
A pergunta, em uma linha: **quando o commit dono ja passou, quem conserta a linha?** Ou a correcao
anda sozinha (e a regra ganha a excecao "linha que a fonte desmente se corrige no proximo ato que
tocar aquela lei"), ou ela espera o proximo ato do papel `prazo` -- e eu sigo pela segunda, que e a
leitura literal, ate voce dizer o contrario.
 o **item 4 do R6** (crons-juiz) segue
bloqueado pela lei dele em `CORTES.md:99` — *"Nao e fatia: e a divida de desenho que o proprio 4a
declara. Cada cron sai da lista quando a celula agendar o proprio marco."* Pela
PAREI-DE-LEI-NAO-DEVOLVE-TURNO a esteira segue o proximo item que nao depende dela, e o CENSO
mediu qual e (`logs/placar_estrutural/censo_juiz_batida_escala_20261008.txt`, secoes 1 a 6, read-only).
**Nenhuma das duas celulas "NINGUEM COMECOU" fecha num ato** — e eu escrevi o contrario antes de medir:
  · `batida x um juiz por pergunta`: **1 pergunta ja declarada** (`ponto/juiz_batida.py::periodos_do_dia`)
    e **2 pendentes, os dois `zona=dinheiro`**, segurados por cortes dele (a E3 pela metade no aval de
    26/09; a porta `furo_so_intervalo` da E2). VERDE exige PENDENTES=0, entao esta **atras da E3/E2**.
  · `escala x um juiz por pergunta`: `JUIZES['escala']` = None — a unica das 9 familias sem pergunta
    declarada —, mas as 4 perguntas ja tem de 1 a 3 respondedores em PRODUCAO. VERDE exige resolver
    `escala/utils.py::_esc_vigente_do_dia`, que **e o O142**.
O que esta desbloqueado agora nao e o verde: e o **passo de DECLARACAO** da S-ESCALA, que e exatamente
o item **(3)** da ordem sequenciada dele em `CORTES.md:120` (*"juiz escala, os 8 pontos,
`ponto/nucleo.py:12` primeiro"*) — e os itens (1) e (2) dessa ordem fecharam, medido. O PRIMEIRO que ele
nomeou esta achado e e o achado que redireciona: **`ponto/nucleo.py::escalas_no_periodo` e
`::te_vigente_em` respondem "quem vige neste dia" por `ativa=True` / "ultima por `data_inicio`",
ignorando `escala_geradora`, e NAO aparecem em PENDENTES de familia nenhuma**. O alcance e por CAMPO,
nao por chamador (corrigi o meu proprio numero, secao 5c): `te_vigente_em` tem **1** chamador e
`dt_fim_previsto_de` **2**, mas o veredito deles vira `TurnoMaterializado.dt_fim_previsto`, lido pelo
cron `processar_alertas_turno` das */5, por `ponto/selecao_periodo.py:208` e por `ponto/signals.py:80` — nao sao divida
conhecida, sao juiz nao declarado. (`chamado x juiz` espera a lei dele; `chamado x escritor` e o pouso
em curso; `folha/export x juiz` esta atras da O219.)
Lateral medida no caminho: a celula de batida afirma *"nao ha PENDENTES[batida]"* e **`PENDENTES_BATIDA`
ja = 2** — a nota da propria celula envelheceu do mesmo jeito que os 4 numeros do R6, e no mesmo arquivo
da nota de `contratos_estruturais.py:371`. As duas entram no commit do MARCO (L-106, sem commit so de docs).

---

### PLACAR-ESTRUTURAL, o resto do vao (05:0x, read-only — nada escrito em `app/`)

**A PROVA DOS SEIS ESTA SA, e isso fecha uma pergunta que o placar nunca faz.** `placar()`
(`placar_estrutural.py:350`) so computa `prova_faltando = bool(numero) and not prova`: pergunta se a
STRING esta vazia, nunca se o ARQUIVO existe. Resolvi as **47** citacoes de arquivo das 6 provas contra
a base que cada prova declara: **33 arquivos de prova, os 33 existem · 14 sao SITIO de codigo citado na
prosa · 0 nao achados** (R1 3/3 · R2 2/2 · R3 9+5 · R4 10+8 · R5 2/2 · R6 7+1). Censo em
`logs/placar_estrutural/prova_dos_seis_20261008.txt`. **Nao construo selo para isso**: foi medido SAO, e
selo sem defeito medido passa por ausencia de sinal. O `14/20` que sobrevive na prova do R6 nao e numero
velho — e citacao DATADA de 05/10, procedencia, e fica ao lado da nova.
**Erro meu no caminho, publicado porque o metodo errado e o barato:** a primeira passada contou **31
provas inexistentes** e estava errada — casou nome por regex e testou na raiz, sem resolver a base (nome
nu e relativo ao diretorio citado antes; o bloco do R3 cita `logs/r3_cross/` uma vez so;
`r5_2345.txt (+ _completo.txt)` e idioma de SUFIXO; e 14 "faltas" eram sitio sob `app/`). Criterio pela
forma inflou 31 para 0.

**O ITEM (7) DO R6 ENVELHECEU ENQUANTO EU O CURAVA, e a cura entrou na copia.** Ele dizia que
`escala x juiz` *"e o UNICO sem censo"* e que *"esse sim cai na TRAVA JUIZ-NOVO"*. As duas coisas cairam
no mesmo dia: o censo existe (`censo_juiz_batida_escala_20261008.txt`) e a trava **nao barra** a
declaracao — MEDIDO pelo grep do proprio selo, ela ve **56** autoridades contra **54** da base e as duas
a mais tem frase assinada, porque ela cobra frase por FUNCAO, nao por familia. A Q2 se declara por
`escala_vigente` (frase assinada por ele em 03/10) e a Q3 por `eh_dia_trabalho` (ja na base): **nenhuma
string nova**. O que fica como PENDENTE sao os que respondem por conta propria, com `ponto/nucleo.py`
na frente — o PRIMEIRO que ele nomeou. O VERDE da celula nao sai da declaracao: depende do **O142**.
Certificado na copia: `py_compile` OK, o consumidor real a carrega standalone (`placar()` devolve
**lista** de 6 — a minha primeira conferencia lia `p['resultados']` e devolvia `None` em silencio, que
e vacuidade minha, corrigida), `R6 prova_faltando=False`, a frase do juiz presente VERBATIM e a frase
velha sobrevivendo **1** vez so, dentro de `ESTA LINHA DIZIA "..."` — citacao, nao afirmacao.

---

### O MARCO FICA MECANICO: os tres ensaios do vao (05:0x-05:2x, so leitura e `logs/`)

Nada foi escrito em `app/` em nenhum momento deste vao. GUARDA 2 = **0** o tempo inteiro.

**(1) O selo da prova mordeu o MEU rascunho, e estava certo.** `bin/relato_afirma_com_prova.py` rodado
contra o topo + o RELATO vivo (5.755 linhas) achou **1** falha, e era minha: a linha do smoke do pouso 5
afirmava `NO AR` em negrito sem uma linha `PROVA:` ao lado -- o arquivo existia desde 03:31 e eu
simplesmente nao o citei junto da afirmacao. Citado, o selo fecha.
  PROVA: `afirma_com_prova: OK (1 arquivo(s), 0 afirmacao sem prova)`, rc=0, com
  `bin/tests/afirma_sem_prova_base.txt` INTACTO (`git status --porcelain` vazio) -- a lista de divida so
  encolhe, e eu nao a ampliei para passar. O selo nao foi tocado: ele esta na lista "medido sao, NAO
  curar".

**(2) O `regua_tickets` me deu um RED FALSO porque eu medi contra a arvore ERRADA -- e o erro e meu.**
A cadeia traz **10** citacoes (`C1b`, as quatro `C*-PORTA-*`, `CHAMADO-EM-RAIA`, `O137`, `O167`, `O221`,
`SEXTO-BANCO`). Contra o `app/docs/TICKETS.md` **vivo**, `O137` e `O167` apareciam como FALTA, e eu
estava a um passo de abrir duas linhas que ja existem. A autoridade de "tem linha na tabela" nao e a
tabela de agora: e a tabela **no instante do push**, e a cadeia REESCREVE esse arquivo (ele esta entre
os 14 `app/docs/` que `HEAD..juncao-o167` toca). Medido contra
`git show juncao-o167:app/docs/TICKETS.md`: **10 de 10 OK**, com as linhas de `O167` (:121) e `O137`
(:122) trazidas pelas proprias raias.
  PROVA: `bash bin/regua_tickets.sh` no estado de agora -> `tickets_placar: OK — placar do topo bate
  com a tabela e com o git.` / `regua_tickets: OK -- 0 citacao(oes) com linha na tabela` (0 porque
  `ahead=0`: o `d0625307` ja esta em `origin/main`), e a conferencia das 10 contra a tabela pos-cadeia,
  uma a uma, toda OK.
  **O QUE SOBRA como requisito real do push**, e e' uma linha de ORDEM, nao de cura: `regua_tickets`
  chama `tickets_placar.sh --conferir` ANTES da cobranca de linha, e o bloco do topo tem de bater com a
  tabela **e com o git**. O commit do marco acrescenta um commit depois do bloco que a raia escreveu,
  entao a ordem do ato e: merges -> edicoes -> `bin/tickets_placar.sh --escrever` -> `git add` por PATH
  -> commit -> `bash bin/regua_tickets.sh` como ensaio -> so entao o push. (`--conferir` e so leitura:
  o unico `open(...,'w')` esta sob `PL_MODO == '--escrever'`, e o `git log` dele vai para um `mktemp`.)

**(3) A nota de `('batida','um juiz por pergunta')` virou PATCH pronto, com ancora unica.**
`logs/pousos/patch_nota_batida.py` (4.448 B) ancora por TEXTO com `assert count==1`, compila o resultado
antes de gravar e e idempotente por marcador. Ele corrige a metade que envelheceu: a nota afirma
`NINGUEM COMECOU ... nao ha JUIZES["batida"] nem PENDENTES["batida"]` e as DUAS chaves existem.
  PROVA: medido chamando a FONTE (`PYTHONPATH=app python3 -c "from core import juizes"`):
  `JUIZES['batida']` declara **1** pergunta -- *que periodos e que intervalo teve este dia?* ->
  `ponto/juiz_batida.py::periodos_do_dia` -- e `PENDENTES['batida']` tem **2** itens, os DOIS com
  `zona='dinheiro'`: *quantas horas este dia vale?* (fica no motor; a migracao e a E3, cortada pela
  METADE no aval de 26/09) e *o dia em aberto bloqueia a folha?* (adendo dele de 25/09,
  `folha/export.py::classificar_export`). **Logo a celula nao esta parada por falta de censo: esta atras
  de CORTE DELE**, que tem outro dono -- e as perguntas que seguem sem autoridade declarada sao as
  outras cinco (geofence, espuria, par relampago, aparelho, janela offline).
  CERTIFICADO em COPIA do `git show HEAD:` (nunca na arvore viva): `--check` OK com ancora **unica**
  (29.309 -> 29.954 B); aplicado; rodado 2x -> `JA APLICADO ... Nada a fazer`; carregado pelo caminho do
  consumidor real -> `contratos_estruturais: 15/20 verdes`, `verdes=15 total=20` **inalterados**; e a
  celula e identica ao vivo em **todas** as chaves menos `nota` (`excecoes`, `idempotencia`, `teste`,
  `verde=False` -- a cura e de NOTA, nao de estado). `'NINGUEM COMECOU'` sobrevive **1** vez, e so
  dentro da citacao (`esta nota dizia "`).
  A TRAVA JUIZ-NOVO **nao morde aqui**, e isso se mediu em vez de se supor:
  `bin/tests/test_juiz_novo_tem_corte.sh:24-28` varre `app/core/juizes.py` e SO ele, entao
  `'ponto/juiz_batida.py::periodos_do_dia'` entre aspas neste arquivo nao pede corte. Em `juizes.py`
  pediria -- e e por isso que a nota da S-ESCALA vai **sem aspas e sem `::`**.
  O `:371` (`122 FORA`) segue INTOCADO, de proposito: ele e o numero pos-pouso, e medi-lo agora seria
  medir a arvore que a cadeia vai mudar -- o mesmo erro do item (2), uma hora antes.

**(4) A licao do item (2) valia para MAIS TRES artefatos, e eu so a tinha aplicado ao TICKETS.**
Tudo que eu certifiquei neste vao foi certificado contra a arvore de ANTES da cadeia e vai ser aplicado
DEPOIS dela. Medido um por um contra `git show juncao-o167:`:
  - **`app/core/contratos_estruturais.py` E TOCADO pela cadeia** -- `492 insercoes, 5 delecoes`. A ancora
    do `patch_nota_batida.py` sobrevive: `--check` contra a versao pos-cadeia -> **ancora unica**, compila
    (76.796 -> 77.441 B). Se nao sobrevivesse, o `assert count==1` recusaria em vez de gravar torto, que
    e exatamente para isso que ele existe.
  - **`app/core/placar_estrutural.py` NAO e tocado** -- entao a base do patch do R6 nao se move, e
    `git apply --check -p1` segue OK depois de eu regerar o diff (**100** linhas).
  - **O PLACAR NAO MUDA com a cadeia, e isso era o risco real do R6**: a cura do R6 carrega a frase do
    juiz VERBATIM, entao se a cadeia fechasse uma celula a cura nasceria velha -- o proprio defeito que
    ela cura. MEDIDO chamando `linha_do_placar()` nas DUAS versoes: vivo `contratos_estruturais: 15/20
    verdes`, pos-cadeia **`15/20` tambem**; `verdes=15 total=20` nos dois; celulas que ficam verdes com a
    cadeia = **[]**, celulas que deixam de ser = **[]**. A cura nasce verdadeira.
  - **O selo do RELATO rodado contra a arvore POS-CADEIA** (a cadeia reescreve `app/docs/RELATO.md`:
    5.253 -> 5.296 linhas), e do jeito que `bin/relato.sh:72-73` o chama -- os DOIS arquivos, `RELATO.md`
    e `RELATORIOS-PLANO.md`: `afirma_com_prova: OK (2 arquivo(s), 0 afirmacao sem prova)`, rc=0. A cadeia
    nao toca o selo nem a sua linha de base. (E o selo gateia o PUBLICAR, nao o push: ele retem o RELATO
    do ciclo e deixa ESTADO e SESSAO seguirem.)

**(5) O ff-only dos tres pousos COLIDE com `app/docs/RELATO.md`, e a guarda disso ja existe.**
`git merge --ff-only` recusa qualquer path rastreado SUJO que o checkout mudaria, e a GUARDA 2 conta so
`.py|.html`. Medido por `comm -12` entre os sujos rastreados e o que cada raia toca: os TRES pousos
colidem em `app/docs/RELATO.md` (os outros tres sujos -- `HANDOFF-SESSAO.md`, `bin/sombra.sh`,
`config/crons_duracao.json` -- nao sao tocados por raia nenhuma). **Nao e cura minha**: os tres scripts
ja tem o bloco que guarda o colidente em `git stash push` com copia e diff carimbados em
`logs/pousos/`, faz o ff e devolve com `git stash pop` (pouso 3 :85-108, pouso 4 :98-121, pouso 5
:153-176), e `.py`/`.html` ficam DE FORA de proposito -- se um deles estiver sujo o ato para na guarda de
arvore limpa, que e onde tem de parar. Conferido nos tres antes do disparo, nao descoberto as 06:08 com
um `.done` morto.

**(6) O item (5) do R6 deixou de carregar numero, e a raia e quem ensinou a forma.** A nota dizia
`122 escritas fora de porta em 49 arquivos`; a raia do chamado moveu esse placar de PROSA para um TESTE
-- `chamados/tests/test_chokepoint_chamado_gate.py`, `ALLOWLIST = ()` vazia e `TETO_POR_ENTIDADE` exato,
com a propria nota de la avisando "o numero VIVO mora no `TETO_POR_ENTIDADE` abaixo, nao [nesta prosa]".
Entao o pendente que eu tinha -- "escrever o numero pos-pouso nos DOIS arquivos" -- **estava errado pela
metade**: o lado do `contratos_estruturais.py` ja foi curado pela raia, e melhor do que eu ia curar
(numero em prosa -> teto em teste). O que sobrava era o `placar_estrutural.py`, que a cadeia nao toca, e a
cura certa nao e copiar o numero novo: e NOMEAR a autoridade e nao repetir o valor, que e a mesma
TESTEMUNHA LE, NAO RECALCULA do R6. O `122` sobrevive **1** vez no arquivo, e so dentro de
`ESTA LINHA DIZIA "`.
  PROVA: `TETO_POR_ENTIDADE` no tip da cadeia = `ChamadoColaborador 12 · DisputaSupervisao 4 ·
  PerguntaDisputa 5`, soma **21** -- que e o `121 -> 21` do aval dele, conferido e nao crido. Patch
  regerado: `git apply --check -p1` OK; `py_compile` OK; `placar()` devolve **6** resultados com R6
  `estado='PARCIAL'`, `prova_faltando=False`; a frase do juiz presente verbatim; o gate citado como
  autoridade do item (5). RED/GREEN refeito depois da edicao: arvore viva ->
  `SELO=VERMELHO falhas=1` (*"a prosa publica ['14/20 verdes']"*), copia curada -> `SELO=VERDE`.
  **Com isso o `:371` que eu reservava para "medir depois do pouso" nao existe mais como pendente**:
  nao ha numero meu para escrever ali.

---

A **O221 esta NO AR nos DOIS pousos** (`bb0bd0fa` produto as 00:37:34; o instrumento neste push) e o
placar, perguntado ao juiz dentro do `saas_core`, diz **15/20 verdes**. A O139 fechou: o papel `prazo`
(L-101) tem cadastro, o contador `juizes_por_varredura` caiu de **27 para 25** medido pela funcao real, e
o selo de host que cobra o papel **existe e MORDE**.
**PROVA:** `linha_do_placar()` dentro do `saas_core` = `contratos_estruturais: 15/20 verdes`;
suite da copia `wt-o139` = `Ran 9675 tests in 621.827s` -> `OK (skipped=42)`, rc=0;
`juizes_por_varredura()` na copia = **25**, no main (antes) = **27**, `tupla ^ funcao = []`;
`bin/tests/test_papel_prazo_nao_deriva.sh` = rc **0** na arvore viva e rc **1** com `vigia_de_hora`
devolvido a `JUIZES_POR_VARREDURA` na copia;
`janela_auth` de `app/api/views.py` = barrado entre 23:20 e 06:00 (`bin/auth_sitios.txt`).

**A CELULA-TURNO-FECHA NAO FOI CARIMBADA, e a pergunta e de ESCOPO DE AVAL, nao de execucao** (nao devolvo
turno por ela -- PAREI-DE-LEI-NAO-DEVOLVE-TURNO). Os **seis resultados do passo 6 medem VERDADE agora**, no
ar, com `bb0bd0fa`: `PENDENTES['celula/precedencia']` = **0**, `PENDENTES['turno/marcos']` = **0**,
`verde=True` nas **duas** celulas de "um juiz por pergunta", `linha_do_placar()` no container = **15** --
exatamente o numero que o aval previu. O que NAO fecha e a **condicao de CAMINHO**: o seu corte
**ESPINHA-ANTES-DA-UI** (`docs/CORTES.md:120`, 03/10 08:13) escreve o PRONTO de cada celula como *"lista de
excecoes em ZERO, selo de idempotencia da porta, **verde=True no mesmo commit**"*, e o passo 6 pede as
**duas** no mesmo commit. MEDIDO commit por commit: **turno cumpriu** (`PENDENTES` a 0 **e** `verde=True`
no mesmo `6319b10c`); **celula nao** -- os pendentes dela zeraram em `fdd6f42c` (O191 passo 5) e o
`verde=True` so veio em `b4372615` (O218), porque eu a segurei DE PROPOSITO esperando o numero chegar ao
GRAVADO, e quem a fechou no fim foi o EFEITO MEDIDO do ponto fixo (9.008/9.008), nao a relavratura. Pela
LEI-AKITA 9 o escopo do aval e literal, entao a celula do marco fica **RESULTADO ATINGIDO, carimbo
esperando o seu `!`** em vez de eu me dar o verde. Frase pronta: *"! carimba a CELULA-TURNO-FECHA como
FECHADA -- os 4 numeros do passo 6 batem no ar, e a unica coisa que faltou foi o `verde=True` das duas
celulas cair no MESMO commit (turno em `6319b10c`, celula em `b4372615`)."*
Com ela sem poder andar, a **ORDEM VIVA andou** (decisao tecnica, registrada e nao devolvida): o head passa a ser **`PLACAR-ESTRUTURAL`** (L-099, *"ANDA"*, proximo = R6 item 4), lido da MESMA fonte do hook (`bin/hook_stop_fila1.py::_proximo_da_fila`, o primeiro aberto do bloco OBRAS) e nao de um leitor novo; a **O219**, que o aval de 07/10 poe atras da O218, vem logo depois dela na mesma tabela. **PROVA:** `bin/tests/test_hook_nao_cobra_congelado.sh` = `OK -- ve id com espaco e aponta o 1o da ORDEM VIVA (PLACAR-ESTRUTURAL, lido do marcador)`, rc 0.

**A `raia-chamado` POUSOU, e os "7 conflitos" que este paragrafo anunciava eram medicao de outra
arvore.** Os tres pousos da O221 estao no ar (08/10): produto `bb0bd0fa`, instrumento `d0625307` e a
juncao `52bfc524` -- 85 arquivos, 10.787 insercoes. A forma foi a da L-107, em copia (`wt-chamado2`):
`git revert bea841ff` -> `git merge 142238fc` -> resolver -> suite -> e so depois o merge na arvore viva
com o `bin/deploy.sh` no mesmo ato. O NUMERO ANTIGO ESTAVA ERRADO E A CAUSA E MINHA: eu medi os 7
conflitos com `git merge-tree` da raia contra um main que **ainda carregava a reversao** -- duas arvores
que nunca iriam se juntar assim. Desfeita a reversao PRIMEIRO, o `git revert` resolveu sozinho o unico
arquivo que o main moveu depois dela (`core/contratos_estruturais.py`, 7 commits): **ZERO conflito**. O
merge dos 6 commits deu **UM**, em `docs/TICKETS.md`, e os cinco blocos dele eram ou a regiao
`PLACAR:INICIO..FIM` que `bin/tickets_placar.sh` GERA, ou as linhas C1..C4 em versao mais velha que a que
o main ja tem -- porque a reversao de 03/10 preservou os docs de proposito.
**PROVA:** `git revert bea841ff` = `41 files changed, 5627 insertions(+), 495 deletions(-)`, nenhum
conflito; `git merge 142238fc` = 1 arquivo em conflito, `git diff --name-only --diff-filter=U` =
`app/docs/TICKETS.md`; `grep -E "^[+-] *verde"` no diff da reversao = VAZIO e no diff do merge = VAZIO;
`git diff --cached --stat` do merge nao lista `core/juizes.py`, `core/placar_estrutural.py` nem
`config/crons.py`; `TETO_POR_ENTIDADE` lido por AST = `ChamadoColaborador 12 · DisputaSupervisao 4 ·
PerguntaDisputa 5`, soma **21**, o numero do aval, asseverado pelo proprio gate contra
`censo_escritas.varrer_arvore`; `python3 -m py_compile` nos 61 `.py` do merge = OK; marcadores de
conflito na arvore inteira (py/md/html) = **0**; suite da copia = VERDE (Ran 9921 tests in 1321.3s, skipped=42).

O211 segue CONSTRUIDA e VERDE na raia `wt-regua` (`raia-regua`) -- `core/models.py::AplicacaoConvencao`,
migration `0017`, o escritor `semear_aplicacao_convencao` e o RED `core/tests/test_aplicacao_convencao.py`
com **14 testes OK** pela porta canonica; `core/regua_cct.py` segue IDENTICO ao HEAD, entao **nada de
dinheiro se move no pouso A** -- o leitor migra no pouso B (DIFF de frota na sombra antes), e a O211 so
FECHA la.

lei **RESPONDIDA 07/10 19:xx** (aval `O-SISTEMA-CALCULA-O-QUE-TEM`, **L-113**): *"o sistema calcula o
que tem (...) se ja foi para o Dominio e gerou holerite, problema do DP e da empresa"* -- e, com estas
palavras, **um chamado PODE nascer em competencia paga**. A pergunta abaixo fica como HISTORIA (ela era
*quem julga a LAVRA de uma competencia EXPORTADA, e o CHAMADO que ela faz nascer?*) e os numeros dela
seguem valendo como IMPACTO da O218/O219, nunca mais como trava. As
competencias que a relavratura NAO tocou medem, pelo mesmo teste de ponto fixo: **09/2026 (EXPORTADA) 424
de 17.330 dia-colab = 2,45%, 119 colabs, ata 52.492 contra autoridade 207.815 min (+2.588,72 h)**;
**08/2026 (EXPORTADA) 756 de 16.894 = 4,47%, 145 colabs, ata 146.934 contra 382.849 min (+3.931,92 h)**.
Elas so caem relavrando ata de competencia **EXPORTADA**, e o censo da O209 ja provou que relavrar ata
**nao chega a rubrica -- chega a CHAMADO**.
A LEI EXISTENTE, grepada antes de perguntar (LEI-AKITA 4), **nao responde**: a **L-092** fala do
**GRAVADO** (`recalcular_fechamento_mes`), nunca da ATA, e o proprio `LEIS.md:115` a declara *"TEXTO
SUPERADO EM PARTE"* pela **O TXT E FOTOGRAFIA DO CALCULO** (30/09), que por sua vez diz *"correcao provada
entra em QUALQUER competencia, a qualquer momento, nenhuma trava"* -- isto e, pelo lado da LAVRA a lei
vigente **autoriza**. O que nao tem juiz e o resto: `CLAUDE.md:363` (familia FECHAMENTO) escreve, com
estas palavras, que *"o que 'fechado' quer dizer, **lavra** e reabertura: SEM juiz"*, e `:358-359`
(familia FERIADO/PRAZO) poe *"competencia fechada"* na mesma lista. Entao a pergunta e so uma: **um
chamado pode NASCER numa competencia que o Dominio ja pagou?** -- nao ha L-NNN que diga sim nem nao, e nao
e decisao tecnica (e dinheiro do colaborador na mesa do DP). Nao devolvo turno por isso
(PAREI-DE-LEI-NAO-DEVOLVE-TURNO): a pergunta fica aqui com os numeros e a esteira **segue a O211**.

## O221 POUSO 3 — **A JUNCAO DA `raia-chamado`: A REVERSAO SE DESFAZ E A FAMILIA CHAMADO VOLTA INTEIRA** (08/10)

O `!` dele de 07/10 19:35 (`POUSO-CHAMADO-DEPOIS-DA-BATERIA`) dizia, literal: *"a raia-chamado
(wt-esmeril2, 6 commits, escritas fora de porta 121 -> 21) POUSA logo depois do marco da
BATERIA-DA-LAVRATURA: junta com o main, roda a suite, e pousa pela L-105 se estiver verde. Se a juncao ou
a suite falhar, diz o motivo em uma linha no topo do RELATO e segue a fila."* A juncao nao falhou e a
suite ficou VERDE (Ran 9921 tests in 1321.3s, skipped=42), entao ela pousou.

**O que a juncao nao moveu, e era o que mais importava.** O pouso 3 traz 85 arquivos e 10.787 insercoes,
e NAO toca `core/juizes.py`, `core/placar_estrutural.py` nem `config/crons.py`. Entao o
`PENDENTES['celula/precedencia']` e o `PENDENTES['turno/marcos']` que a O191 e a CELULA-TURNO-FECHA
zeraram seguem zerados, o `PAPEL_DO_CRON` continua derivando o papel `prazo` do `PRAZO_DELEGA_A` (a O139
de uma hora atras), e as 150 linhas que a raia acrescenta ao `contratos_estruturais.py` sao TODAS de
`nota`: o `grep -E "^[+-] *verde"` volta vazio nos dois diffs. Isso era condicao escrita no plano, nao
observacao depois do fato -- a celula de `celula/precedencia` tinha acabado de ficar verde na O218 e um
merge que a rebaixasse em silencio seria o placar mentindo pelo lado barato.

**O PLACAR NAO SE MOVE COM ESTE POUSO, e dizer o contrario seria o erro mais facil da noite.** A raia
foi aberta para fechar as DUAS celulas da familia chamado, e ela nao as fecha: o proprio gate que ela
traz declara, na primeira linha do docstring, que *"fim do trabalho e este gate com os tres numeros em
**0**"*, e eles estao em **12 / 4 / 5**. O que a raia entregou e a QUEDA -- 121 escritas fora de porta
para 21, oito portas novas declaradas em `core/portas.py` -- e o teto que SO DESCE. `contratos_estruturais`
continua em **15/20** depois do pouso, pelo mesmo juiz de sempre, e a familia chamado segue a maior fatia
estrutural de pe.

**A unica linha de docs que se perdeu, se perdeu por ser DUPLICATA.** Resolvi o conflito do `TICKETS.md`
pelo lado do main nos cinco blocos, e conferi que nao havia id orfao: todo id da versao da raia tem linha
na versao do main (`comm -23` entre as duas listas = vazio). As cinco linhas C1..C1b do main sao mais
longas que as da raia porque foram escritas no merge de 03/10, que a reversao preservou de proposito.

**PROVA:** reversao `bea841ff` = 41 arquivos, 5.627 insercoes, 495 delecoes, ZERO conflito;
merge `142238fc` = UM conflito, `app/docs/TICKETS.md`, cinco blocos, todos gerados ou duplicados;
total sobre o main = 85 arquivos, 10.787 insercoes, 863 delecoes, 84 `.py` + 1 `.md`;
`TETO_POR_ENTIDADE` por AST = 12/4/5 = **21** (o numero do aval, asseverado pelo gate contra a funcao
real `core/censo_escritas.varrer_arvore`, allowlist `()`); zero migration no diff e zero mudanca de
campo em `chamados/models.py` (`grep -E '^[+-].*(Field\(|choices=|default=|db_index=|max_length=)'` so
casa docstring), por isso o deploy foi `--sem-migrate` e nao por conveniencia;
suite da copia = VERDE (Ran 9921 tests in 1321.3s, skipped=42).

## O221 POUSO 2 — **O INSTRUMENTO: O SELO DO PAPEL `prazo` MORDE, E O QUE ELE NAO PROVA ESTA DITO** (08/10 01:1x)

**PROVA:** `bin/tests/test_papel_prazo_nao_deriva.sh` (329 linhas) na arvore VIVA =
`papel_prazo: 2 cron(s) de papel prazo; juizes_por_varredura=25` / `papel_prazo_nao_deriva: OK`, rc **0**;
a pasta inteira `bin/tests/` na arvore viva = `pasta_rc=0`; **RED do sentido (2)** forcado na copia
`wt-o139` -- `vigia_de_hora` devolvido a `JUIZES_POR_VARREDURA` -- deu rc **1** com uma FALHA literal:
``vigia_de_hora` DELEGA o prazo ao juiz da casa e segue em JUIZES_POR_VARREDURA:
['app/ponto/services/vigia_de_hora.py:73 marcos_vencidos']``; desfeito, a copia voltou **byte a byte**
(`cmp -s` = igual) e o selo voltou a rc **0**.

**O QUE ESTE POUSO *NAO* PROVOU, e eu devo a linha porque a mensagem do pouso 1 afirmou mais do que media.**
A mensagem de `bb0bd0fa` credita ao selo a prova de que `lavrar_previsto_cego` tem papel declarado. **O selo
nao diz isso** -- a saida dele conta cron de papel `prazo` e o contador da divida, nada mais. **Quem prova e
a leitura do cadastro pela funcao real:** `PAPEL_DO_CRON['lavrar_previsto_cego'] == 'lavra'` e
`sem papel declarado == []` sobre os **71 crons nomeados**, medidos depois do pouso. A afirmacao era
verdadeira; a TESTEMUNHA citada era a errada, e trocar a testemunha e' o que a LEI-AKITA 2 cobra.

O **sentido (1)** do selo (papel `prazo` com derivacao propria = VERMELHO) nao foi forcado contra um cron
real nesta arvore -- ele responde pelo **caso sintetico interno** do proprio selo, que e o anti-vacuidade:
dois corpos `rodar` montados no arquivo, um que delega (`marcos_vencidos`) e um que compara
`agora - x.criado_em > timedelta(days=4)`, com os status ESPERADOS assertados. Se as funcoes de AST
pararem de achar qualquer coisa, essa assercao fica VERMELHA -- e e por ela que o selo nao passa por
ausencia de sinal. Dito com esta clareza para que ninguem leia o selo como mais largo do que e': a
docstring dele ja nomeia o buraco MEDIDO (`varrer_pares_embutidos:245-250` monta par na mao fora do
`corpo` declarado, e por isso o `detectar_par_relampago` **nao** ocupa o papel hoje).

**POR QUE O INSTRUMENTO VEIO SOZINHO** (L-105): `bin/`, selo de host e portao de push nao pousam junto com
produto. O pouso 1 levou os 5 `.py`/derivados; este leva 1 arquivo de `bin/tests/` e os docs do marco. Zero
`.py` servido pelo `saas_ui`/`saas_core` muda neste ato -- o container nao monta `bin/`.

## O221 POUSO 1 — **O PRODUTO DA O139 NO AR, E O INSTRUMENTO FICOU DE FORA DE PROPOSITO** (08/10 00:37:34)

**PROVA:** `9b01e4e6`; `juizes_por_varredura()` pela funcao real = **25** na copia mesclada contra **27**
no main `97e9e043`, `tupla ^ funcao = []`; suite `bin/suite.sh --dir /home/ronald/wt-o139 --parallel 2` =
`Ran 9675 tests in 621.827s` -> `OK (skipped=42)`, rc=0; `bin/tests/test_papel_prazo_nao_deriva.sh` contra
a copia mesclada = `papel_prazo: 2 cron(s) de papel prazo; juizes_por_varredura=25`, rc=0;
`ruff check` nos tres `.py` = `All checks passed!`; `deploy: OK` as 00:37:34, `importerror_500=0`;
pasta de selos de host na arvore VIVA = `pasta_rc=0`.

A juncao nasceu em COPIA (`wt-o139`, `cherry-pick -n` sobre o HEAD), e deu **zero conflito**: o main andou
**79 commits** desde a base da raia (`ba82736d`) e tocou os mesmos dois arquivos, mas em regioes que nao se
cruzam (`config/crons.py` em `:365` e `:1430` contra `:1380-1490` da raia; `ARQUITETURA.mmd` em 8 pontos,
nenhum deles nos 4 da raia). Produto para a arvore, commit, `deploy.sh` -- nada no meio (L-107).
**O numero foi REMEDIDO, nao copiado**: a raia publicou 27 -> 25 contra a base DELA, e a base dela nao e o
main. O cron que o main acrescentou nesses 79 commits (`lavrar_previsto_cego`, 07:33) nao ficou sem papel --
o selo da raia, exercido contra a copia mesclada, responde pelos dois.
**Os derivados nao foram mesclados a mao**: `ARQUITETURA.mmd` e `MAPA.md` saem de `gerar_diagrama`, e a raia
mudou o PROPRIO gerador; rodei o comando real contra a copia e o resultado e bit a bit o da juncao textual
(`369 linhas; docs/MAPA.md: 60 linhas`, zero arquivo modificado depois).
**O instrumento ficou fora por L-105**, nao por duvida: `bin/tests/test_papel_prazo_nao_deriva.sh` ja esta
exercido (rc=0 acima) e pousa no ato seguinte. O preco de 04/10 foi medido -- instrumento no mesmo pacote
do produto travou o pouso da folha por 1 h.

**LATERAL DEVIDO, uma linha, nao desvio:** `bin/crons.sh check` diz DIVERGE em **uma** linha, e ela nao e
desta fatia -- `eval/noturno.py` esta **02:33 no codigo e 02:37 no host**, porque o horario dele e
**DERIVADO** (`crons.py:445`, `inicio_derivado('noturno.py', vizinho='03:20', piso='00:15')`) e a duracao
medida mora em `config/crons_duracao.json`, que por lei **nunca e commitado**. Entao a derivacao anda sozinha
e o crontab do host so a acompanha quando alguem roda `install` -- que `crons.py:198` ja registra como
capaz de **APAGAR** linhas so-do-host. Nao instalei: a cura de origem e quem reinstala depois da derivacao
andar, e isso e desenho, nao este pouso. O `check --medir` das 04:05 e o `placar_code.sh` ja publicam a
divergencia com a cura ao lado.

## O222 — **NO AR, E O PLACAR CONFERIDO PELO JUIZ** (08/10 00:09, marco empurrado 23:5x)

**PROVA:** `linha_do_placar()` chamado DENTRO do `saas_core` (schema juliani) devolve
`contratos_estruturais: 15/20 verdes`, `verdes()=15`, `total()=20`; `deploy: OK -- migrations em dia,
tres cascas reiniciadas juntas, tres rotas provadas` as 00:09:48; `importerror_500=0`;
`origin/main` = `97e9e043`.

`origin/main` = `97e9e043`; suite do push **75 OK (skipped=42)** + control-plane **22 OK**; selos de host
`pasta_rc=0`. `bin/deploy.sh --sem-migrate` as **00:09:48**: `migrations pendentes: 0`,
`janela_auth: OK -- nenhum sitio de auth mudou desde origin/main (10 declarados)`, prova de casca
`16 estaticos, 5 paginas, 599 rotas em 2 urlconf(s)`, tres rotas provadas (`/health/` 200,
`/colaboradores/` 302, mensageria 200), selo BUG 128 verde, `importerror_500=0`.
**O portao da sombra foi aberto pela porta DECLARADA**, nao pulado: `DEPLOY_SEM_SOMBRA` com o motivo na
trilha -- o carimbo e `dia=20261007 status=OK tipo=completa diverge=0 erros=0`, dele mesmo saiu o deploy
das 22:48, e passada a meia-noite o portao fica **cego** ate o dump das 04:00 (`sombra.sh:203` soma +1 a
divergencia quando o dump nao e do DIA). Sem migration, sem caminho de dinheiro, sem sitio de auth.
As duas tentativas anteriores falharam por **minha** leitura do parser, nao pelo portao: `--sem-sombra`
so e lido em `$1` (`bin/deploy.sh:64`) e a variavel e `DEPLOY_SEM_SOMBRA` (`:63`), nao `SEM_SOMBRA_MOTIVO`.

**O placar se perguntou ao JUIZ, dentro do `saas_core`**, nunca por grep de arquivo:
`core.contratos_estruturais.linha_do_placar()` -> `contratos_estruturais: 15/20 verdes`, com
`verdes()=15` e `total()=20`. E o leitor de prod dizendo o mesmo que o commit, que e a unica forma de
saber que o flip de `verde` da celula `celula/precedencia` x `um juiz por pergunta` chegou a tela.

## 07/10 23:4x — **DOIS AVAIS DELE NO MEIO DO TURNO, REGISTRADOS SEM PARAR A FILA** (`EMP1-E-CCT`, `HORIZONTE-PADRAO`)

**PROVA:** cadastro lido no vivo, somente leitura (`colaboradores.models.Empresa` no `saas_core`, schema juliani):
`emp1 regime_trabalhista=''` com **7 colabs, 3 ativos** e `FechamentoMensal` **4 na 10 / 3 na 09**; `emp2` e `emp4`
ja `cct`, `emp3` `clt`; `emp20/21/29` com regime vazio e **0 ativos** (0, 0 e 2 colabs); **um** sindicato cadastrado
(**sind2**, Vigilantes de Londrina) e **1** `VinculoSindicatoPraca`.

**1. "emp1 e CCT".** Responde a lateral de `emp1 Confiance Force Ltda` que estava devendo linha no PENDENTES, e
muda a O211 antes de ela nascer: a `AplicacaoConvencao` passa a nascer com **TRES** linhas, nao duas -- emp1, emp2 e
emp4 -> sind2, sem praca. Hoje a emp1 cai no piso legal pelo ramo `core/regua_cct.py:245` (`regime_trabalhista` vazio
nao e `clt`, mas tambem nao chega a convencao) e e **uma das que fazem `empresas_sem_regime` nao ser 0**; o resto do
contador sao as tres empresas de diagnostico, sem ninguem ativo. ASSUNCAO DECLARADA, porque ele nao nomeou
sindicato e so existe um: a linha e **emp1 -> sind2**; se a CCT da Confiance for outra convencao, ela **nasce como
cadastro ANTES** (TUDO TEM CADASTRO), nunca como literal no codigo. Teto de impacto, medido e pequeno: **3 ativos e
4 fechamentos na 10**; a **09 nao se toca** por ser exportada. Nada chega a prod fora do pouso A da O211 -- aqui e
registro de cadastro a fazer, nao ato.

**2. "horizonte padrao de correcao de codigo = competencia aberta + anterior".** Fecha a ultima pergunta que a
**L-113** deixou aberta e que o adendo `AVISO-E-ESCOLHA` de 19:5x mandava declarar caso a caso: *"o sistema calcula o
que tem"* **nao** vira "rejulga tudo para tras por versao". O juiz de "aberta" e de "anterior" **ja existe e nao se
clona** (LEI-AKITA 4): `ponto/janelas.py::janela_atual` e `janela_anterior`. Hoje, 07/10, o horizonte e **10/2026 +
09/2026**, e **08/2026 e FOTO**.
O numero que isso move e um que a O218 ja mediu, e ele muda de dono: `dias com ata de regra antiga` por competencia
da **10 = 0 de 9.008**, **09 = 424 de 17.330** e **08 = 756 de 16.894** (`logs/o209_conf_ata_3comps.out`). Entao o
universo do contador `dias_com_regra_velha` esperado 0 e o **HORIZONTE**, nao a competencia aberta sozinha: os **424
da 09 sao divida da O219**, a sanar pelo caminho da impressao que a propria linha dela declara (`--forcar` deixa de
ser o caminho), e os **756 da 08 ficam FOTO** -- rejulgam so se a batida do dia for editada, com o rotulo
"recalculado em <data>". A linha honesta ao lado da PROVA da O218, que diz "09 EXPORTADA INTACTA": ela segue
verdadeira para o escopo da O218 (ATA SO, competencia 10), e os 424 da 09 nao sao contradicao dela -- sao o item
seguinte da fila, que e exatamente a O219. **Nada relavrado agora.**

Os dois viraram linha em `app/docs/PROMPTS.md` e adendo na linha da obra (**O211** e **O219**) no mesmo turno, e a
fila nao parou: nenhum dos dois depende de resposta minha para andar, e nenhum deles e `!`.

## O218 — **APLICADO EM PROD, E A PROVA DEPOIS** (07/10 22:53→23:1x, **MARCO FECHADO**, placar 15/20)

**PROVA:** GRAVADO em prod, lido do banco depois do ato -- `celula#118980 ata.minutos_realizados=541` e `celula#118981 =545` (col146, 28 e 29/09; eram 0 e 894), as duas `veredito=concorde via=cartorio`, `julgada_em=2026-10-07T22:53:32`; ata x autoridade na frota da 10 = **0 divergente em 9.008 dia-colab** de 570 colabs, soma `2.251.838 = 2.251.838` min (delta +0); 09 exportada `hash=dfd8d145c3d9af35fc768e765fe38461 linhas=17332` **antes e depois**; `contratos_estruturais: 15/20 verdes` pela funcao real.

Condicao 4 da `DINHEIRO-EM-COMPETENCIA-ABERTA`, fechando as quatro: o DIFF foi publicado ANTES (secao
abaixo), a reversao foi gravada ANTES da escrita, a 09 exportada ficou INTACTA pelo hash nos dois
lados, e aqui esta o resultado medido. Saidas integrais: `logs/o218_apply_prod_20261007.out`,
`logs/o218_prova_col146.out`, `logs/o218_pontofixo_frota_10.out`, `logs/o218_dry_pos_apply.out`.

**O ATO.** `570 colab(s) relavrados em 123 s`, ATA SO -- nenhuma linha escreveu `FechamentoMensal` nem
`DiaPago`. `casados com a sombra (ata moveu exatamente o esperado): 24 colab(s)`,
`explicados pelo livro-caixa (fato novo depois do dump): 0` e `COBERTURA DO ESPERADO: 24 de 24
colab(s) previstos foram visitados`. Nenhuma linha de `PAREI` por colab -- que e o que o script
levanta quando um colab move fora do previsto --, entao nada precisou ser desfeito. A
emissao saiu IDENTICA a prevista: `chamado +13 -0` (pk 29431-29443), `pergunta +23 -0` (pk
40926-40948), `disputa +5 -0` (pk 6518-6522). Cartorio: `julgadas=8609 carimbadas=9008 protestos=823
emitidos=85 nunca_bateu=399 vetados=9`. Reversao em
`/app/logs/o218/reversao_frota_20261007_225332.json` (9.008 celulas x 11 campos), resultado em
`/app/logs/o218/resultado_20261007_225537.json`. **09 EXPORTADA antes e depois:
`hash=dfd8d145c3d9af35fc768e765fe38461 linhas=17332`, INTACTA** -- o mesmo valor que a sombra mediu,
o que diz que as duas bases partiam do mesmo lugar.

**A LINHA QUE EU NAO ENTENDI NA HORA, e por que ela NAO contradiz o DIFF.** O resumo do apply diz
`prod observou +1499 min · a sombra previu +1499 min (em 24 dia-colab)` e o DIFF dizia **+2.851 min em
40 dia-colab**. Eu nao declarei prova antes de explicar, e a explicacao esta no filtro do proprio
acumulador (`logs/o209_apply_frota_prod.py:447-451`): ele soma **so os dia-colab SEM lacuna**, isto e
sem fato de prod nascido depois do dump. O numero se mediu na FUNCAO REAL, rodando o DRY (leitura
pura) depois do apply: **17 dia-colab com lacuna**, e os fatos sao todos de `22:53`-`22:55` com pk
29431-29443 / 40926-40948 / 6518-6522 -- **a cobranca que o PROPRIO ato acabou de emitir**. Um deles,
col868 em 26/09, e prod andando e nao o ato: `chamado#29445`, pk **acima** do maior que o ato criou
(29443), com `criado_em=atualizado_em=resolvido_em=2026-10-07T23:00:21` -- nasceu e fechou no mesmo
instante, pelo lote `*/5`; na mesma linha o cron ainda re-julgou `pergunta#40938 julgado_em=23:00:21`,
uma pergunta que o ato criou minutos antes.
Descontado ele, eram **16 no instante do apply**, e a aritmetica fecha pelo pacote: **24 sem lacuna
(+1.499 min) + 16 com lacuna (+1.352 min) = 40 dia-colab (+2.851 min)**. A soma e um SUB-RECORTE do
DIFF, nunca um conjunto diferente: a conferencia FORTE e `mov == esp` por colab, e ela cobre os **40**
-- os 24 colabs casaram em TODOS os seus dias, inclusive nos 16 que a soma descontou. (Cuidado de
leitura: `24 colab(s)` casados e `24 dia-colab` contados sao coincidencia de numero, nao a mesma
coisa.)

**ACHADO DE INSTRUMENTO, nomeado e nao curado neste pouso:** `lacunas()` nao distingue fato nascido
PELO ato de fato nascido em prod, e por isso o apply desconta da propria soma justamente os dias em
que ele mesmo cobrou. Nao e dano -- a guarda forte nao usa esse recorte --, e' um medidor que
subdeclara a propria cobertura. O conserto e barato e o material ja esta no script (`PK_ANTES` e
`ch_antes_colab`, gravados antes da escrita): filtrar por pk. Vai como INSTRUMENTO, em pouso proprio
(L-105), nunca junto do produto.

**A PROVA DO PONTO FIXO, na funcao real, em PROD.** `ponto/services/cartorio.py::julgar_colab`
chamado DUAS vezes seguidas no col146, com reversao gravada antes
(`/app/logs/o218/reversao_col146_20261007_230643.json`):

| passada | ata se moveu | veredito se moveu | delta de conjunto | #118980 | #118981 |
|---|---|---|---|---|---|
| A | 0 dia | 0 dia | `chamado +0 -0 · disputa +0 -0 · pergunta +0 -0` | ata=541 concorde | ata=545 concorde |
| B | 0 dia | 0 dia | `chamado +0 -0 · disputa +0 -0 · pergunta +0 -0` | ata=541 concorde | ata=545 concorde |

Os dois dias golden do aval, lidos do banco de prod: **celula#118980 (col146, 28/09)
`ata.minutos_realizados=541`** e **celula#118981 (29/09) `=545`**, as duas `veredito=concorde
via=cartorio`, `julgada_em=2026-10-07T22:53:32`. Era `0` e `894` antes do ato -- a lampada invertida
que a O217 curou na origem. A 09 nao e alcancada **por construcao**, e isso se prova
estruturalmente em vez de por hash: a lista que vai ao cartorio e filtrada por `data >= INI10` e tem
`assert` em cima (`logs/o218_prova_col146.py:106`); 28 e 29/09 estao na competencia **10**
(21/09..20/10), nao na 09.

**O PONTO FIXO NA FROTA, que e o que fechou a celula.** Sonda somente leitura em prod
(`logs/o218_pontofixo_frota.py`, 37 s): **9.008 dia-colab, 570 colabs, 0 sem lavra, 0 DIVERGENTE**,
soma `ata 2.251.838 min · autoridade 2.251.838 min`, delta **+0**. O denominador se abre, porque
metade dele e um *"nao sei"* que **nao se cala** (memoria `nao-sei-impossibilidade-vs-cobertura`):
**4.159** dia-colab em que a autoridade da numero e ele bate, mais **4.849** em que ela devolve
`sem_turno` -- e **deles, 0 tem minuto gravado na ata**. Nao ha divergencia escondida por denominador
menor; foi por isso que a sonda ganhou esse contador antes de eu declarar o numero. Contra o
`fd6c8c0e`, que mediu **2 de 7.859**: e o MESMO universo em datas diferentes (a janela julgada cresce
um dia por dia), e os 2 eram exatamente o col146 em 28 e 29/09.

**O PLACAR, pela funcao real.** `core/contratos_estruturais.py::linha_do_placar()` no `saas_core`:
`contratos_estruturais: 15/20 verdes` (`verdes=15 total=20`, declaradas 17), e a LINHA HAIKU
acompanha sozinha, porque monta o rotulo com `total()` no ato: `arquitetura: 15 de 20`, `faltam 5`
(batida e escala x um juiz, chamado x um juiz e x um escritor, folha/export x um juiz). A celula
**(celula/precedencia, um juiz por pergunta)** virou `verde=True` em commit SEPARADO do marco, depois
de medida -- o censo dela ja estava em zero desde 05/10 (`PENDENTES['celula/precedencia'] == ()`,
`len(PENDENTES_CELULA) == 0`), e o que faltava era EFEITO. Selos: 29 testes OK em
`core.tests.test_selo_contratos_estruturais`, `ponto.tests.test_contract_juiz_celula`,
`core.tests.test_haiku_contratos_estruturais` e `core.tests.test_haiku_contador_ordem`.

**O QUE A CELULA NAO AFIRMA, e esta escrito nela.** O **VEREDITO** do dia nao e ponto fixo em UMA
passada. Medido NA SOMBRA, relavrando a frota tres vezes seguidas: a **2a** move
**31 dia-colab de 9.008 carimbados, em 14 colabs** -- todos `furo -> cobrado` com `real 0 -> 0`, soma
**+0 min** (`logs/o218_idempotencia.out:33-71`) --, e a **3a** move **ZERO**
(`logs/o218_idem3.out`). Converge em duas, nao oscila. A causa e
`ponto/services/cartorio.py:637`, que le o numero de chamados VIVOS que a propria lavratura acabou de
criar -- regra de **B5.3c (27/08)**, anterior a esta fatia (`git diff fd6c8c0e` nos tres arquivos nao
tem uma linha de `cobrado`).
Isso e a obra **O222** e nao toca a celula: ata, lampadas e minutos -- que e o que *"quantos minutos o
dia realizou?"* pergunta -- fecham na PRIMEIRA passada.

**A CLAUSULA `dias com ata de regra antiga` NAO CAI EM SILENCIO.** Ela so e computavel quando a
VERSAO da regra for insumo da impressao, que e o 5o insumo da **O219** -- a casa dela e la, nao aqui.
O que a O218 entrega no lugar e o proxy por competencia, medido pelo mesmo teste de ponto fixo: **10
= 0 de 9.008**, **09 = 424 de 17.330 (2,45%)**, **08 = 756 de 16.894 (4,47%)**
(`logs/o209_conf_ata_3comps.out`, 119 e 145 colabs). As duas exportadas so
caem relavrando ata de competencia exportada, que e outro marco -- a lei que o autoriza existe desde
07/10 19:xx (**L-113**).

**OS DOIS `best-effort falhou` DO ATO NAO SAO ACHADO NOVO:** `operacao=tipo_marco_divergente
motivo=intervalo_saida`, `pergunta_id=36573` e `38273`, `causa=Exception('marcos_faltantes=S x
ata=E')`. Sao os MESMOS dois ids, com a mesma causa, que o apply da O209 registrou em 05/10 18:0x e
que ja viraram a obra **O213** (BACKLOG:364). Passaram por `core/observ.py::registrar_engolido`, com
traceback -- nao e silencio. O censo que a O213 pede segue NAO feito, e nenhuma pergunta foi tocada.

## O218 — **O DIFF DE FROTA, PUBLICADO ANTES DO APPLY** (07/10 22:2x, base LIMPA, sombra do dump de 21:59:53)

Condicao 1 da `DINHEIRO-EM-COMPETENCIA-ABERTA`. Saida integral em `logs/o218_diff_10_limpo.out`; as
duas passadas de ponto fixo em `logs/o218_idempotencia.out` e `logs/o218_idem3.out`.

**A BASE.** A primeira medicao foi JOGADA FORA e o motivo vale escrito: a sombra do dia estava
**MUTADA**, nao velha. Lendo as duas celulas golden DENTRO dela, a ata ja marcava 541/545 com
`julgada_em=2026-10-07T18:45:41-03:00` -- o RUN C daquela tarde as havia relavrado por cima da base,
e o carimbo dizia `lavra_de_prod=OK data_ref_prod=2026-10-07` porque ele mede IDADE, nao MUTACAO. No
mesmo diagnostico cairam outros dois defeitos, separados: `_instante_do_dump()` do arreio
(`logs/sombra/relavra10_frota_20261005.py:228`) le `/sombra/dumps/juliani_agora.dump` por mtime, que
so existe sob `--dump-agora`, entao devolvia um resto de 05/10 -- o `dump_de_prod_em` do pacote
estava errado por DOIS DIAS; e o portao de idade do apply (`0 <= _idade_h <= 12`) reprovaria mesmo
com instante certo, porque a base era de 04:00 e o apply e de ~22:00. `--refazer --dump-agora`
resolveu os tres de uma vez: base nova de **07/10 22:03**, de um dump de prod de **21:59:53**,
`SOMBRA_DIVERGE=0`, `dump_de_hoje=sim`. A cura de ORIGEM do instante (gravar `SOMBRA_DUMP_EM` no
carimbo) e INSTRUMENTO e pousa sozinha depois do produto (L-105); o instante deste DIFF foi
conferido **a mao** contra o mtime do `--dump-agora`.

**O QUE SE MOVE.** `competencia 10/2026`, janela `2026-09-21..2026-10-20`, julgada ate `2026-10-06`.
Substrato: 17.114 celulas da 10 em 579 colabs, 587 fechamentos, 10.852 linhas de DiaPago. O ato 1
relavrou **570 colabs em 98 s** e o cartorio devolveu `julgadas=8609 carimbadas=9008 protestos=823
emitidos=85 nunca_bateu=399 vetados=9`.

- **A ata se move em 40 dia-colab, em 24 colabs.** Dos 40, **8 mudam MINUTO** (soma **+2.851 min**) e
  **32 mudam so o VEREDITO**, com o realizado identico nos dois lados.
- **O GOLDEN ANDA** -- a clausula de PRONTO do aval: `col146 2026-09-28 real 0 -> 541` e
  `col146 2026-09-29 real 894 -> 545`. Os dois ficam `concorde` antes e depois; o que estava errado
  era o NUMERO, nao o rotulo.
- Os outros 7 que movem minuto sao o defeito (B), a ancora, aparecendo na frota: **col868** em tres
  dias (`0 -> 658`, `0 -> 653`, `0 -> 648`), **col898** (`0 -> 699`), `col887 -2` e `col947 +3`. Todos
  os grandes vinham `discordante` com `real 0`: turno que EXISTIA e nao era casado porque a borda da
  janela era a ancora.
- **Nascem em prod, pela porta** (`chamados.ChamadoColaborador.abrir`): `chamado +13 -0 · pergunta
  +23 -0 · disputa +5 -0`. Os pks dos chamados nascidos na sombra: `29426..29438`.
- **Comp 09 EXPORTADA INTACTA, medida tres vezes** pelo `hash09()` (md5 das celulas da 09 por
  `colaborador_id, data, ata, dna, veredito, veredito_via, trabalha, origem`):
  `dfd8d145c3d9af35fc768e765fe38461`, 17.332 linhas -- antes de tudo, depois do ato 1 e depois do ato
  2. **Nenhuma celula da 09 se moveu.** (L-092)

**O ATO 2 E IMPACTO, NAO ATO.** O apply de prod e **ATA SO**: `logs/o209_apply_frota_prod.py` nunca
chama `recalcular_fechamento`, entao nada do ato 2 vira escrita em prod (LEI-AKITA 9 -- o aval nao se
amplia). Medido so para ter dono: `fechamentos_mexidos=7 de 587`, **VAZOU para fora da lista:
NENHUM**, APLICAVEIS (movimento so DENTRO) `2 -- [82, 898]`, SEPARADOS (movem campo FORA, fatia da
deriva) **5** -- col146, col317, col868, col879, col950 --, 20 linhas de DiaPago movidas. O exemplo
mais afiado da **O210** esta ai: `col879` tem `minutos_realizados -480` no motor enquanto a ata dele
segue em 484.

**A PROPRIEDADE FIXA DO AVAL, MEDIDA NA FROTA -- e ela precisa de uma QUALIFICACAO.** *Lavrar duas
vezes == lavrar uma vez* vale como esta escrita para **ATA, LAMPADAS e MINUTOS**, e a frota a cumpre
ja na primeira passada: a 2a passada move o golden em NADA (`[]`), `soma do realizado 0 -> 0`, e
`chamado +0 · pergunta +0 · disputa +0`. Para o **VEREDITO** ela precisa de DUAS passadas, e o motivo
**nao e desta fatia**: `ponto/services/cartorio.py:637` decide `cobrado` quando `not cods and vivos`,
e `classificar_dia` recebe `chamados_vivos=len(vivos)` -- o protesto FURO deixa de existir quando a
cobranca passa a existir, e a cobranca nasce **na mesma passada que carimbou o furo**
(carimba-antes-de-emitir, regra de B5.3c de 27/08). MEDIDO: **31 dia-colab de 9.008 carimbados, em 14
colabs**, mudam `furo -> cobrado` na 2a passada, todos com `real 0 -> 0`; **a 3a passada move ZERO**
(`A ATA SE MOVEU em 0 dia-colab`, `DELTA DE CONJUNTO +0 -0`). Converge em DUAS, **nao oscila**. A
pre-existencia e por LEITURA DE CODIGO, nao por medicao:
`git diff fd6c8c0e -- ponto/services/cartorio.py escala/utils.py ponto/turnos.py | grep '^[+-].*cobrado'`
e **VAZIO**. O cenario de convergencia entra na BATERIA (o aval manda: *"achou cenario novo em
producao = entra na bateria, nao vira lei"*), no commit do flip, e a pergunta *"o cartorio devia reler
`vivos` depois de emitir?"* vira **linha de BACKLOG da familia chamado** -- nao se toca o caminho de
emissao com a raia-chamado esperando pouso (O221).

**O PACOTE DO APPLY** esta em `logs/o218_esperado_20261007.json.ok` (md5
`b4762cd48016866b61b5743700b0ed1f`), copia protegida: o vivo em `logs/sombra/` foi sobrescrito pelas
passadas de idempotencia e **nao serve**. Ele carrega `dump_de_prod_em 2026-10-07T21:59:53-03:00`
(idade < 1 h, o portao de 12 h fecha), 24 colabs, 40 dia-colab, soma +2.851 min e 448 linhas
`[EMITE]`/`[FURO_PARCIAL]`.

**SUITE:** as labels da regua na raia `wt-lavra`, pela porta canonica -- `Ran 9673 tests ... OK
(skipped=42)`. A inversao do selo do registro era o unico vermelho e fechou.

## CONFERENCIA DO MARCO O209 — **NAO VIROU. O PLACAR E 14/20** (05/10 ~19:3x, medido no vivo)

Resposta a ordem literal (*"mede agora por verdes() e responde uma de duas"*): **nao virou 15/20**.
`core/contratos_estruturais.py::linha_do_placar()` chamado no `saas_core` agora devolve
**`contratos_estruturais: 14/20 verdes`** -- `verdes()=14`, `total()=20` (teto da L-100, com
`familias_sem_cadastro()=('escala','chamado')`), `declaradas()=17`. A celula
(`celula/precedencia`, `um juiz por pergunta`) segue **verde=False**, e as outras cinco nao-verdes sao
`chamado/juiz`, `folha-export/juiz`, `batida/juiz` (teste=None), `escala/juiz` (teste=None) e
`chamado/escritor` (teste=None).

**O CENSO DELA ESTA EM ZERO, MEDIDO NO VIVO**: `juizes.PENDENTES['celula/precedencia'] == ()`,
`len(PENDENTES_CELULA) == 0`, `fora_de_autoridade('celula/precedencia') == 0`. Nao e o censo que segura.

**O QUE FALTA, COM NOME E NUMERO: `col146`, 2 dia-colab de 7.859 na competencia 10 (0,025%), +192 min
(+3,20 h).** Medido por sonda **somente leitura** em prod (`nice -n 19`, 33 s, 564 colabs, **0 sem lavra**,
dois rodados independentes com o mesmo resultado):
- **28/09**: ata `minutos_realizados = 0` contra autoridade **541**;
- **29/09**: ata **894** contra autoridade **545** (894 min = `06:31 -> 21:25`, uma SAIDA pareada com a
  ENTRADA do plantao seguinte -- o vao entre dois plantoes, nao um turno).
E a ata desses dois dias foi escrita **pela propria relavratura, 05/10 17:32:12**: as seis batidas da
janela tem `criado_em` entre 27 e 30/09 e **nenhuma retratada** -- nao houve dado novo depois do ato.

**O NOME CERTO DA SONDA: e TESTE DE PONTO FIXO, nao "ata contra autoridade".** A autoridade
(`ponto/turnos.py::realizado_do_dia` -> `turnos_do_colab`) pede o papel de cada batida a
`papel_por_minuto_da_ata` (`ponto/turnos.py:1387`), que **le a ata**. Entao a comparacao mede uma coisa so:
*a lavratura concorda consigo mesma na releitura*. Corolario que vale registrar: montador == autoridade na
autopsia e **esperado** (mesma ata de entrada), nao prova de "um juiz".

**O MECANISMO, MEDIDO -- nao lido no codigo.** O arquivo de reversao do proprio apply
(`app/logs/o209/reversao_frota_20261005_173212.json`, 7.859 celulas) guarda a ata de ANTES:
- `#118979` (27/09): prev=780 real=780, lampadas `17:30 E / 06:30 S` -- ja certa, **o ato nao a mudou**;
- `#118980` (28/09): prev=541 **real=541**, lampadas **INVERTIDAS** (`06:30 ·I1 tipo=E tipo_real=S` /
  `21:30 ·I2 tipo=S tipo_real=E`) -> depois do ato: lampadas **consertadas** (`21:30 E / 06:31 S`) e o
  numero **541 -> 0**;
- `#118981` (29/09): prev=894 real=894, lampadas tambem **invertidas** (`06:31 E/S` / `21:25 S/E`) ->
  depois: lampadas consertadas (`21:25 E / 06:30 S`) e o numero **mantido em 894**.
Isto e, a relavratura **rodou** nessas celulas e consertou o papel das batidas; os minutos que ela gravou
sao os da passada que ainda leu o papel VELHO. Dentro do mesmo dicionario de ata, hoje, as **lampadas
estao certas e os minutos vem do pareamento invertido** -- `#118980` com lampadas de um turno de 541 min
gravando `0`. **A lavratura nao e ponto fixo**: relavrar de novo escreveria 541 e 545
(`ponto/portas/celula.py::lavrar_veredito` relavra sempre que `cel.ata != dict(ata)`).

**POR QUE NAO HOUVE DRY**: `ponto/services/cartorio.py:764` calcula `_ata` **dentro** do `if apply_`; o
proprio arreio declara que *"o DRY desta fatia e a SOMBRA"*.

**A PROVA DE QUE A RELAVRATURA FUNCIONOU ONDE CORREU** e o gradiente, pela mesma sonda, nas tres
competencias: **10 (relavrada 05/10 17:32) 0,025%** · **09 2,45%** · **08 4,47%** -- a competencia tocada
esta ~100x mais limpa que as duas intactas. O ato fez o que foi publicado; o que sobrou nao e residuo do
ato, e um defeito de **realimentacao** da lavratura, que o ato nao prometeu curar.

**NAO CARIMBO A CELULA.** Verde com 2 dia-colab divergentes seria selo falando por efeito que nao houve --
e listar a 09/08 como o que falta a celula faria dela um contador que **nao pode zerar por construcao**
(elas dependem da lei da linha `lei:` acima, nao de trabalho meu),
que e o defeito que `folha/porta_export.py:160-166` recusa com estas palavras: *"contador que nao pode
zerar por construcao para de ser porta e vira ruido"*. O que muda neste commit e so a **nota** da celula,
que ainda diz *"a relavratura ainda nao correu"* -- ela correu, e mentira em declaracao nao espera fatia.

**ACHADO LATERAL, contado e nao curado** (vai para o BACKLOG, nao e fatia hoje): a ata com **papel
contraditorio** (`lampada` com `tipo != tipo_real`) existe em **214 dia-colab / 110 colabs na 10** e
**425 / 154 na 09** -- por ciclo na 10: 6x1 139, 12x36 68, 5x2 3, personalizado 4, intermitente **0**
(as do `col146` foram justamente as consertadas pelo ato). **Papel contraditorio NAO prediz divergencia**
-- 214 contra 2 --, entao ele e sintoma legivel do mesmo pareamento, nunca o contador do defeito.

FILA 1 ANDANDO, sem PAREI. **ORDEM VIVA: o item (3), `O209`, esta FECHADA** -- a ata da FROTA na 10
foi **aplicada em prod 05/10 17:32-17:34, rc=0** (564 relavrados em 109 s, 92 de 92 casados com a
sombra, `explicados=0`, comp 09 EXPORTADA intacta por hash), com as 4 condicoes da
`DINHEIRO-EM-COMPETENCIA-ABERTA` fechadas uma a uma; a prova esta nos dois blocos logo abaixo. Os itens
(1) **`CELULA-TURNO-FECHA`** e (2) **relavratura 10** (restrita aos 3 colabs, apply em prod 05/10 15:57)
estao **FECHADOS e NO AR**, e a **O195** -- a cura que destravava o (2) -- tambem. Do (1) fica a ressalva
de sempre: passos 1-5 FECHADOS com prova, o passo 6 **PARCIAL** e por isso NAO carimbado.
**PROVA:** medido no worker VIVO em 07/10, nunca de memoria -- `docker exec saas_ui manage.py shell`
responde `escala.utils.minutos_realizados_do_dia` **ausente** (item 1, a funcao apagada pela L-111),
`ponto.turnos._teto_s_da_jornada` **presente** (O195) e `core.contratos_estruturais` **14** de 20. Os
commits sao `8fce4967` (O195), `c8031f6d` (relavratura 10 nos 3 colabs) e `212b25a7` (ata da frota na
10); o reload das tres cascas esta em `logs/deploy_o208_20261005.out`.
A ordem dele de 05/10 09:0x, literal: *"(1) turno -- apagar a funcao e fechar a celula; (2) relavratura
10 restrita; (3) BOs de tela na ordem do bloco; (4) O145. Instrumento so depois disso"*, com a **O204**
entrando entre (2) e (3) pelo aval de 09:4x (*"BO de producao PROVADO, passa a frente dos 4 BOs de
tela"*), a **O197** (`FUTURO-NAO-E-EM-ABERTO`) subindo pelo de 11:5x (*"sobe para logo depois da
relavratura 10, a frente da O204"*) e a **O44 v2** pelo de 12:2x (*"entra logo atras da O207 e a frente
dos BOs de tela"*), sem lei nova. **Dois avais de hoje a noite entram na frente dos BOs de tela**: a
**O211** (`REGUA-PELA-EMPRESA`, 16:4x) e, logo depois dela, a **O214** (`HE-DECISAO-EM-ESCALA`, 18:0x --
*"entra na fila 1 logo depois da O211 e a frente dos BOs de tela"*; a L-097 fica INTACTA, o que muda e
como a decisao chega ao admin, e a ETAPA 0 dela e um ESMERIL de censo antes de qualquer tela).
**A ORDEM DE AGORA**: (1) turno FECHADO -> (2) relavratura 10 FECHADA -> (3) **O209 FECHADA** ->
**O211** -> **O214** -> O197 -> O204 -> O207 -> O44 itens 2-8 -> O198/O199/O200 -> O145 -> instrumento
(pouso do CERT-AST, depois O205/O206/O212). Fora da fila, sem segurar ninguem: a **O213** (o tipo
derivado do motivo discorda do tipo da ata em 2 perguntas, achado na saida do apply da O209) e "conta e
publica", PRE-APROVADO e sem portao; e a **O210** (a deriva da 10) tem portao aberto e numero publicado,
mas **apply NAO feito** -- ela herdou da O209 a divida de medir por que a ata andou +61.862 min enquanto
o fechamento dos mesmos 92 colabs anda +4.908.

**UMA PISTA.** O aval de 12:4x abriu uma raia paralela (`wt-bos`, ramo `raia-bos`) com
**O197 -> O204 -> O200 -> O206** e o aval seguinte, **~4 min depois**, a PAROU: *"a raia paralela dos BOs
PARA agora, com trilha do ponto em que estava; a principal segue na O195"*. TRILHA MEDIDA: **zero
commit** (`git log main..raia-bos`), arvore da `wt-bos` **vazia** no `git status --porcelain`, nenhum
arquivo tocado, nenhum pouso, os arquivos de sinal nunca criados -- ela morreu em ORIENTACAO, lendo os
sitios dos quatro BOs. O worktree **fica de pe** (apagar raia e `!` dele) e os quatro BOs voltam a fila
PRINCIPAL, na ordem do bloco. A principal segue na **O195** sem interrupcao.

## O209 — **O DIFF DE FROTA, PUBLICADO ANTES DO APPLY** (05/10 17:2x, base LIMPA)

Este bloco e a **condicao 1 da `DINHEIRO-EM-COMPETENCIA-ABERTA`**: *"DIFF de frota publicado no RELATO
ANTES do apply"*. **Publicado = gravado aqui, neste arquivo, antes do ato** -- nao e o push (L-106 poe
docs no commit do MARCO, e o marco e o apply; L-109 manda a historia morar no RELATO e so nele). A
ordem e: este texto no disco -> apply -> prova no mesmo bloco -> commit do marco.

Aval que ele cumpre, literal: *"aval Ronald: O209 -- depois do apply dos 3 colabs da relavratura 10,
relavra a ata da frota na 10 com DIFF publicado antes e reversao em logs; as 23 cobrancas nascem. 09
exportada intacta."*

### O NUMERO (sombra refeita 17:14, lavra de prod carregada, `diverge=0`)

```
base: --refazer --dump-agora 17:14:06 · --lavra OK md5=a20f4f99743eb889c200ba9bb569c5b2
      carimbo dia=20261005 status=REFEITA tipo=completa diverge=0 erros=0
      dump de prod: 2026-10-05T17:14:10-03:00

ATO 1 (ata, `forcar`, 564 colab com celula na 10, janela 21/09..04/10):  87 s
  cartorio: julgadas=7527 carimbadas=7859 protestos=732 emitidos=71 nunca_bateu=332 vetados=3
  A ATA SE MOVEU em 193 dia-colab, em 92 colab(s)   -- dos 11 do censo da O195: so [297]
  soma do realizado nos dias movidos: 20.012 -> 81.874  (+61.862 min = +1.031 h)
  DELTA DE CONJUNTO NO BANCO: chamado +14 -0 · pergunta +55 -0 · disputa +7 -0
  pk dos chamados NASCIDOS: 28833..28846 (os 14, contiguos)
  09 EXPORTADA: hash=be2b44793055ae9a5b44b98f5572991a INTACTA antes e depois
  ESPERADO gravado: 92 colab · 193 dia-colab · +61.862 min · 368 linha(s) [EMITE]/[FURO_PARCIAL]
```

### O PRIMEIRO ESPERADO FOI **DESCARTADO**: ele nasceu de base que a MINHA PROVA sujou

A prova de restauro (que o O209 exige, e que passou: hash `e9e0212c…` **VOLTOU AO PONTO** depois de
16.883 celulas reescritas) roda um **ensaio** antes de restaurar. O restauro devolve as 11 colunas da
celula e **retrata** as cobrancas pela porta -- e `retratar` **desdiz, nao apaga**. Entao o ensaio
deixou no banco `chamado +14 · pergunta +57 · disputa +7` que o hash de celula **nao ve**. O ATO 1
seguinte rodou por cima disso.

O mecanismo nao e suposicao: `chamados/models.py:409-430`, `ChamadoColaborador.abrir` faz
`existente = cls.objects.filter(...).first()` e, achando, devolve `(existente, False)` em **qualquer**
estado -- e se `status_local in ENCERRADOS` chama `_renascer_por_premissa_viva()`, que **escreve**.
Medido na mesma base e com o mesmo codigo: ensaio `emitidos=72 · +14/+57/+7`; ato1 em cima dele
`emitidos=7 · +0/+0/+0`, `pk dos NASCIDOS: []`.

**A prova e o ato nao podem dividir base.** Cura: a prova passou a ser portao de **ARQUIVO**
(`/sombra/o209_prova_de_restauro.ok`, escrito por ela mesma **depois** do assert -- prova que falha nao
deixa rastro de prova), a base foi refeita e o ESPERADO regerado. O sujo ficou em
`logs/sombra/o209_esperado_20261005_BASE_SUJA.json`, para a contaminacao ser **medida** e nao discutida.

**E foi medida** (`logs/o209_cmp_esperado.py`, dois eixos separados de proposito):

```
EIXO ATA    dia-colab: suja=222  limpa=193  nos dois=193 · so na suja=29 · so na limpa=0
            dia-colab nos DOIS com valor diferente: 0
            soma_delta: suja=+61862  limpa=+61862   (diferenca +0)
EIXO EMISSAO  nascidos ch 0->14 · pg 0->55 · dp 0->7
              emitidos 7->71 · protestos 704->732 · fio_ressuscitadas 0->8
```

**O veredito**: os **193 que importam sao IDENTICOS, campo por campo, e a soma de minutos nao se move
nem um minuto.** Os 29 a mais da base suja sao **todos** `real 0 -> 0`, veredito `furo -> cobrado`:
eles nao moviam dinheiro nenhum -- moviam a palavra, porque a cobranca que o ensaio deixou de pe fazia
o dia terminar "cobrado". Ou seja, a contaminacao entrou na ata **so pelo veredito**, via o conjunto de
chamados, e **zero** pelo realizado. Mas 29 em 222 e **13% do ESPERADO** com que o apply em prod ia
assertar: sem refazer, o ato ia prometer mover 222 e mover 193, e o assert ia acusar o codigo certo.
Mesma familia do falso-verde dos 40 colabs desta manha -- **base mutada dando verde**.

### OS TRES CONTADORES DE EMISSAO, cada um pelo que a conta FAZ (LEI-AKITA 8)

O aval fala em **23 cobrancas**. Medido na base limpa: **14** chamados nascem. Nao e divergencia de
juizo -- sao **tres contas diferentes**, e nenhuma e errada:

| numero | de onde sai | o que a conta FAZ |
|---|---|---|
| `emitidos=71` | `ponto/services/cartorio.py:839-845` (`_contar_emissao`) | incrementa quando o afunilador devolve **nao-string**: *"dia-colab que passou o funil e terminou com cobranca de pe"* -- **inclui** o caso em que `abrir()` devolveu uma que ja existia. **Nao e** "cobrancas nascidas". |
| **`+14`** | delta de **CONJUNTO de pk** no banco, antes x depois | linhas que **nasceram**. E este o numero de pessoas novas alcancadas: pks 28833..28846. |
| `23` do aval | `emitidos` sobre a base do **RUN C**, medido as 15:0x | mesma assinatura, base outra: aquela ja tinha cobranca de pe em parte dos dias. |

**Nao e PAREI** (7b item 5: so para o que muda o DESENHO). O ato e o mesmo, a direcao e a mesma e o
numero e **menor** que o avalizado -- 14 pessoas novas, nao 23. Fica publicado com nome e com os pks.

### DOIS DEFEITOS NO MEU PROPRIO SCRIPT DE PROD, achados LENDO antes de disparar

1. **Assert VAZIO** (familia SELO ANTI-VACUIDADE). `assert soma_sem_lacuna_obs == soma_sem_lacuna_esp`
   era **verdadeiro por construcao**: as duas somas so acumulam onde `mov[dia] == esp[dia]` (no ramo
   `casados` por definicao; no ramo `explicados` porque ele conta so `dia not in por_dia`), entao as
   parcelas sao identicas termo a termo e o assert **nunca** podia ficar vermelho. Virou **medicao
   rotulada**, e quem prende a divergencia -- o caminho do PAREI, que para no colab com `mov != esp`
   sem lacuna que explique -- ficou nomeado como a guarda que sempre foi.
2. **Pulo em SILENCIO.** Colab do ESPERADO que caisse em `Colaborador.DoesNotExist` ou em `not cels`
   era saltado por `continue` **sem uma linha**, e o run imprimia "APLICADO". Cura: `pulados` com
   motivo escrito, `visitados` contado, e um assert que **morde**: todo colab do ESPERADO foi visitado,
   e os nao-visitados sao primeiro **perguntados ao livro-caixa** (`lacunas()`) -- fato novo pos-dump
   fica **explicado**, e so o inexplicado para o ato.

### O "ZERO" DO ATO 2 DA RODADA ANTERIOR NAO ERA MEDICAO: ERAM **99 RECUSAS**

`0 colab(s) com campo movido` tinha como causa `ENSAIO DE DINHEIRO SEM A LAVRA DE PROD · Marcador
esmeril_lavra_de_prod: AUSENTE`. Lendo `bin/sombra.sh`: `lavra_de_prod` e chamada dentro de `bloco()`
(:285), **nao** dentro de `refazer` -- e eu rodei `--refazer --dump-agora` sem `--bloco`. A cadeia
passou a ter `--lavra` explicito. **Nuance que fica registrada para nao virar diagnostico errado
depois**: o md5 que a recusa dizia carregado (`a20f4f99…`) e o **mesmo** que o `--lavra` gravou em
seguida -- a guarda recusou por **PROCEDENCIA** (marcador ausente), nao por valor. A prosa do erro
("ela ENVELHECE") descreve o caso geral, nao este.

Com a lavra carregada, o numero real da deriva dos 92 (**medido, NAO aplicado** -- o aval e **ATA SO**):

```
ATO 2 (recalcular_fechamento --colabs, os 92): mexidos=29 de 572 · VAZOU para fora da lista: NENHUM
  minutos_realizados   +4.908  (25 colabs)     minutos_abonados    +1.540  (2)
  minutos_previstos    -1.009  (5)             dias_previstos          -3  (5)
  horas_noturnas      +29,32   (1)             horas_trabalhadas   +24,76  (3)
  horas_folga_trabalhada -42,45 (2)            horas_falta          +7,33  (1)
  DiaPago motor: 45 linhas movidas · 09 EXPORTADA: hash INTACTA
```

**Este numero e do O209, nao do O210**, e nenhum campo dele vira escrita hoje: o `!` autoriza a ATA.
**PERGUNTA EM ABERTO, nomeada e nao respondida**: a ata move **+61.862 min** e o fechamento dos mesmos
92 move **+4.908**. A hipotese obvia -- a ata estava stale e o motor ja contava esses dias, porque o
motor le **batida**, nao a lavra da celula -- **nao esta medida**, e por isso fica como pergunta do
**O210**, que mede na foto completa dele. Afirmar a causa aqui seria narrar de memoria.

### AS QUATRO CONDICOES, uma a uma

1. **DIFF publicado ANTES** — este bloco, no disco, antes do ato.
2. **Reversao em `logs/`** — `/app/logs/o209/` recebe a foto das 11 colunas por colaborador **antes**
   de cada um ser tocado; a receita de desfazer vai no proprio arquivo e nomeia a **ORDEM** (retratar
   primeiro pela porta, depois reescrever as 11 colunas **incondicionalmente**; `NUNCA .delete()`). O
   restauro esta **provado**: na sombra, 16.883 celulas voltaram ao hash de partida, 2.862 delas
   estavam de fato diferentes.
3. **09 EXPORTADA intacta** — `hash09()` antes e depois, com `assert`. Na sombra:
   `be2b44793055ae9a5b44b98f5572991a` nos dois atos.
4. **PROVA depois** — fica neste mesmo bloco, logo abaixo, com os contadores de prod ao lado dos da
   sombra.

**SELO DE CONDUTA.** LEI-AKITA: origem=`CelulaDia.ata` lavrada por codigo anterior a
`CELULA-TURNO-FECHA` e nunca relavrada (so `forcar` alcanca), testemunha=o `julgar_colab` de hoje
(mesma funcao que prod usa, nada reconstruido), RED=a divergencia medida na sombra (193 dia-colab,
+61.862 min) e os dois defeitos do meu script achados por leitura, quem-mais-le=TELA/espelho/PDF leem
ata; chamado/pergunta/disputa nascem do sinal dela (14/55/7 na frota); o dinheiro **nao** entra neste
apply, juizes novos=0.

### A PROVA DEPOIS — **APLICADO EM PROD 17:32:11 → 17:34:04, rc=0** (condicao 4)

**PROVA:** `logs/o209_apply_prod_20261005.out` (75 linhas, a saida inteira do ato) -- **564 colab(s)
relavrados em 109 s**, **92 de 92** casados com a sombra, `explicados=0`, e a 09 EXPORTADA com
`hash=be2b44793055ae9a5b44b98f5572991a` **antes e depois** (17.332 linhas, INTACTA).

`logs/o209_apply_prod_20261005.out`, 75 linhas, e a saida inteira do ato. O que ela diz, na ordem
em que importa:

```
APLICADO -- 564 colab(s) relavrados em 109 s (1.8 min)
  casados com a sombra (ata moveu exatamente o esperado): 92 colab(s)
  explicados pelo livro-caixa (fato novo depois do dump):  0 colab(s)
  cartorio: julgadas=7527 carimbadas=7859 protestos=732 emitidos=71 nunca_bateu=332 vetados=3
  COBERTURA DO ESPERADO: 92 de 92 colab(s) previstos foram visitados
09 EXPORTADA antes:  hash=be2b44793055ae9a5b44b98f5572991a linhas=17332
09 EXPORTADA depois: hash=be2b44793055ae9a5b44b98f5572991a  INTACTA
```

Os seis contadores do cartorio sao **identicos** aos da sombra, digito a digito. Nao e coincidencia
de ordem de grandeza: e a mesma funcao (`julgar_colab`) sobre o mesmo acervo, e por isso o
**`emitidos=71`** aqui nao contradiz as **23** do aval -- a assinatura e a mesma, a BASE e outra
(as 23 sairam da RUN C das 15:0x; o `_contar_emissao` de `ponto/services/cartorio.py:839-845`
incrementa quando o funil devolve nao-string, isto e, *"dia-colab que passou o funil e terminou com
cobranca de pe"*, **incluindo** o `abrir()` que devolve uma ja existente -- nunca "cobrancas
nascidas"). Quem conta cobranca NASCIDA e o delta de conjunto no banco, abaixo.

**A EMISSAO, medida como DELTA DE CONJUNTO no banco** (pk antes x pk depois, nao pelo contador em
processo):

| tabela | delta | pks |
|---|---:|---|
| `ChamadoColaborador` | **+14 -0** | 28837..28850 |
| `DisputaSupervisao` | **+7 -0** | 6399..6405 |
| `PerguntaDisputa` | **+59 -0** | 40156..40214 |

**Os 14 chamados sao OS MESMOS 14 que a sombra previu.** Conferido por CHAVE, nunca por pk (pk nao
se compara entre bancos): `(colab, dia-pelo-juiz, modulo)` -- o dia pelo `catalogo/modulos.py::
data_do_chamado`, nunca `.get('data_turno')` cru. `prod=14 sombra=14`, **`A MULTILISTA E IDENTICA:
True`**. Composicao: **7** `batida_ausente`, todos com dia DENTRO da janela julgada (col112 09-29,
col252 10-02, col444 10-03, col866 09-29, col950 09-30/10-01/10-02) e **7**
`disputa_supervisao_manual` com `dia=None` **pela lei do catalogo** (o modulo nao declara
`chave_data`; o juiz devolve None de proposito, e eu nao invento dia para ele).

**O +4 de pergunta (59 contra os 55 previstos) esta ATRIBUIDO, nao chamado de ruido.** Tres leituras,
cada uma mais estreita que a anterior:

1. **por motivo**: `intervalo_saida` 16/15, `intervalo_volta` 17/16, `orfao_14h` 11/10,
   `saida_sem_entrada` 15/14 -- exatamente **+1 em cada um**, o que ja afasta "um colab a mais".
2. **por diferenca de conjunto de chaves**: `nos dois = 55`, **SO NA SOMBRA = 0** (nada do previsto
   deixou de aparecer -- e esta a metade que me prenderia) e **SO EM PROD = 4**.
3. **por dono**: as 4 sao todas de **2026-10-05, HOJE**, FORA da janela julgada (..10-04) -- col252
   `orfao_14h` e col723 `intervalo_saida`/`intervalo_volta`/`saida_sem_entrada`. Elas penduram em
   disputas **#1829 (aberta 17:15:11)**, **#2339 (07:15:09)** e **#1488 (17:30:07)**, as tres
   ANTERIORES ao meu corte das 17:32:12, com `chamado_destino` **#28712/#28821/#28753**, os tres
   com pk **ABAIXO** do meu primeiro nascido (28837). Ou seja: e emissao do proprio prod depois do
   dump, alimentando disputa que ja existia -- o meu ato preencheu o questionario delas. O alcance
   ao dia 10-05 vem pela **DISPUTA**, que e do COLAB e nao do dia (e o que o meu proprio codigo
   diz), nunca por julgar dia fora da janela. Corroboracao independente: a sequence de chamado na
   sombra estava em **28833** na hora do dump e prod havia avancado exatamente **4** (ate 28837),
   o mesmo numero que o censo de emissao pos-dump tinha contado.

**A SOMA DO REALIZADO: `prod observou +61457 min · a sombra previu +61457 min (em 173 dia-colab)`.**
E o proprio arreio imprime ao lado que a igualdade e **POR CONSTRUCAO** -- so entra no somatorio o
dia em que prod == sombra --, entao quem prende divergencia e o PAREI por colab, nunca esta linha.
E 173 e nao 193 por uma **propriedade do meu proprio instrumento, que eu achei nos numeros**: o
livro-caixa (`lacunas()`) enxerga as emissoes **do proprio ato** (`criado_em > CORTE`), entao 20
dia-colab sairam do somatorio por "lacuna" que fui eu mesmo quem criou. Isso so deixa a guarda MAIS
permissiva -- nunca para por engano --, e aqui nao mudou resultado nenhum: `explicados=0` e
`casados 92/92` sao afirmacoes mais fortes do que a soma.

**CONDICAO 2, a reversao, no disco do HOST antes de qualquer escrita**:
`app/logs/o209/reversao_frota_20261005_173212.json`, **6.549.143 B**, em `/dev/vda2` (nao tmpfs) --
**7859 celulas x 11 colunas**, janela 2026-09-21..2026-10-04, `pk_antes: {chamado: 27445, pergunta:
32015, disputa: 5353}`, com a receita de desfazer nomeando a ORDEM (retratar pela porta primeiro,
depois reescrever as 11 colunas incondicionalmente; `NUNCA .delete()`). O resultado em
`app/logs/o209/resultado_20261005_173404.json` (30.890 B).

#### As duas curas de instrumento deste turno, e o que a segunda pegou

1. **O rotulo do universo convidava a um alarme falso.** O DRY imprimia `7859 ... (na sombra: 16883
   ...)` -- um fosso de 2x que se le como dano. Nao era: `universo.celulas` do arreio e
   `len(A_ata)` e o `foto_ata()` filtra `data__range=(INI10, FIM10)` = **30 dias**, enquanto os dois
   lacos de julgamento usam `INI10..LIM` = **14 dias** (564x30 ≈ 16883 ✓, 564x14 ≈ 7859 ✓). O
   rotulo passou a imprimir as DUAS janelas e a comparar igual com igual (LEI-AKITA 8: o rotulo diz
   o que a conta faz).
2. **O livro-caixa era cego as tres tabelas de EMISSAO** -- e isso era risco de **PAREI FALSO vivo**,
   nao teorico, porque o meu comparador acabara de provar que o conjunto de chamados move o eixo do
   veredito sozinho. Curado lendo os nomes de campo reais em `chamados/models.py` (nunca adivinhando)
   e pelo juiz declarado `data_do_chamado`. **Pagou no primeiro DRY**: acusou
   `col114 2026-09-24 pergunta#36074 julgado_em=17:25:04` e `col114 2026-09-25 pergunta#36562
   julgado_em=17:25:04` -- dois fatos pos-dump REAIS que a versao velha devolvia como `[]`.
   Limite medido e escrito no codigo: **`PerguntaDisputa` nao tem campo de nascimento** (os 23
   campos do model trazem `respondida_em`, `validada_em`, `julgado_em`, `materializada_em`,
   `data_conferida_em`, e nenhum `criado_em`/`auto_now_add`), entao ela nao responde "quando nasci"
   e eu nao invento a resposta: o nascimento dela se alcanca pelo CHAMADO e pela DISPUTA.

Uma terceira ordem de leitura tambem mudou: o `hash09` DEPOIS e o seu `assert` passaram a rodar
**ANTES** do `assert` de cobertura. O de cobertura PODE ficar vermelho, e se levantasse primeiro a
prova da L-092 nunca imprimiria -- 564 colabs relavrados sem prova na saida, exatamente na hora em
que ela mais importa.

#### Os 11 avisos da saida, nomeados (nenhum e silencio)

- **9 `sem_celula: ata nao lavrada, abstendo`**, de **duas** familias. (a) **5** repeticoes de
  `complementar_marcos_faltantes` em **col373 2026-07-19** pela disputa **#3676** (aberta 25/08,
  `fechada_em=None`): aquele dia **nao tem CelulaDia nenhuma** -- medido --, e julho esta fora de
  toda janela deste ato; e acervo anterior, nao efeito meu. (b) **4** em **col950**: tres
  `criar_questionario` (ch#28847 09-30, ch#28849 10-01, ch#28850 10-02) e um `auto_fecho_disputa`
  (dp#6405). Aqui o fato e o contrario do que o rotulo sugere: as tres celulas **existem**, tem
  `ata` e foram julgadas **pelo meu proprio ato** (`julgada_em=2026-10-05 20:32:12.100626+00`,
  `veredito=furo`), e a **funcao real** `_marcos_faltantes`, chamada agora, devolve
  `estado=lido` com **4 faltantes** em cada um dos tres dias. Ou seja: a abstencao foi de ORDEM
  dentro do ato, nao de ata ausente -- e e **comportamento declarado**, nao buraco: o docstring de
  `chamados/services/disputa_emissao.py::_ata_do_dia` diz literalmente *"o dia se cura na proxima
  passada depois de lavrado"*. Entao os tres chamados ficam de pe **sem pergunta ate a proxima
  passada do cartorio**, que agora le `lido`. **A CONFERIR na passada seguinte**, e e o unico fio
  solto deste ato.
- **2 `best-effort falhou`** (`registrar_engolido`, portanto NAO silenciosos): `tipo_marco_divergente
  motivo=intervalo_saida pergunta_id=36573` e `38273`, os dois com `resposta=12:00`,
  `tipo_pelo_motivo=S` e `causa=marcos_faltantes=S x ata=E`. E um **desacordo entre o tipo derivado
  do motivo e o tipo da ata** no mesmo marco -- familia chamado, fora deste marco, e vai como item
  proprio com estes dois numeros.

#### A PERGUNTA QUE FICA ABERTA, nomeada e **nao** respondida

A ata andou **+61.862 min** e o fechamento dos MESMOS 92 colabs anda **+4.908 min** (ATO 2 medido na
sombra, **nao aplicado** -- o aval e ATA SO). A hipotese obvia -- a ata estava stale e o motor ja
contava aqueles dias, porque o motor le **batida** e nao a lavra da celula -- **nao foi medida**, e
afirmar a causa seria narrar de memoria. Ela e divida da **O210**, que mede na foto completa dela.

**O QUE ESTE ATO NAO FEZ**, e esta na ultima linha da saida: `FIM -- ATA SO. Nenhuma linha deste ato
escreveu FechamentoMensal ou DiaPago (O210 e outro marco).` O aval nomeia a ATA; embarcar o
fechamento da frota ampliaria o aval (LEI-AKITA 9).

## RELAVRATURA 10 — APLICADA EM PROD NOS 3 COLABS (05/10 15:57, item (2) FECHADO)

`LEI-AKITA: origem=ponto/services/cartorio.py::julgar_colab (a ata, autoridade do realizado), testemunha=CelulaDia.ata + DiaPago(motor) + FechamentoMensal lidos da foto FRESCA, RED=logs/o195_apply_3_prod.py (assert soma == -450 e assert fora == {} pre-escritos ANTES do numero), quem-mais-le=censo de 47 sitios do O195 + os 3 leitores de dinheiro (calendario, porta_export, espelho), juizes novos=0`

**O QUE ENTROU**, pelo **aval 3 item 2** dele, literal: *"relavratura 10: aplica SO o realizado dos
dia-colab da cura, e a deriva vira fatia propria com o numero dela publicado"*. Escopo:
**{col174, col235, col382}**, os unicos colabs em que a ata se moveu num dia do CENSO.

**AS QUATRO CONDICOES DA `DINHEIRO-EM-COMPETENCIA-ABERTA`, uma a uma:**

| # | condicao | como fechou |
|---|---|---|
| 1 | DIFF de frota publicado **ANTES** | commit `8fce4967` (O195): 12.362 turnos pela porta real `turnos_do_colab`, `so_esq=0 so_dir=0`, 58 trocam de dia, 0 na direcao errada, por DESTINO comp09=47 / comp10=11, `comp10 trab -450` |
| 2 | arquivo de reversao em `logs/` | `logs/o195_cond2_20261005.json` (215.088 B) + `logs/o195_cond2_restore.py`. **TRES tabelas, escopadas nos 3**: 90 celulas da janela inteira 21/09-20/10 **com `protestos`** (o JSONField que `lavrar_veredito` escreve em `celula.py:542` e que o JSON antigo das 14:15 **nao tinha**, alem de cobrir so 12 das 90), 3 `FechamentoMensal` (24 campos de valor), 170 `DiaPago` (as DUAS versoes), e o lado que UPDATE nao reverte -- o CONJUNTO de pks de chamado e pergunta por colab (`chamado: 128`, `pergunta: 179`), sem filtro de janela de proposito (delta de conjunto nao precisa de criterio de data; filtrar por forma e o jeito de contar errado) |
| 3 | competencia 09 **INTACTA**, hash antes/depois | **atas:** `37a29deb2a11b39dbc94ec7ef1ba6bc7 -> 37a29deb2a11b39dbc94ec7ef1ba6bc7` (903 linhas, 30 colabs do censo, a MESMA conta de `logs/sombra/o195_smoke_prod_20261005.py`). **fechamento 09 dos 3:** IGUAL (`235:774bef2a…`, `382:ec7faceb…`, `174:b51b4de…`). **VEREDITO: comp 09 INTACTA** |
| 4 | PROVA depois | este bloco + `logs/o195_apply_3_prod_20261005.out` (71 linhas) + `app/logs/recalculo/recalculo_10-2026_20261005_155707.json` (736 KB, antes x depois dos 572) |

**ATO 1 — A ATA.** `julgadas=42 carimbadas=42 protestos=8 **emitidos=0** nunca_bateu=0 vetados=0`.
A ata se moveu em **5 dia-colab**, soma **-450 min**, que e o numero **publicado em `8fce4967`** --
`assert soma == SOMA_PUBLICADA` estava escrito antes de rodar (L-110: todo leitor le o mesmo numero):

```
col174  2026-09-23      0 -> 148     +148  CENSO
col174  2026-09-24    148 -> 0       -148  CENSO
col235  2026-09-30     61 -> 240     +179  CENSO
col235  2026-10-01    658 -> 418     -240  CENSO
col382  2026-09-21    792 -> 403     -389  CENSO
colabs com dia FORA do censo: nenhum | dias do censo que NAO moveram: nenhum
```

**NASCIDOS NO ATO 1: chamado nenhum, pergunta nenhuma** -- medido como **delta do CONJUNTO de pks** no
banco, nao pelo `contadores()`. Isso importa porque a sombra roda com as saidas DESLIGADAS, entao o `0`
dela nao provava o de prod; e importa mais ainda porque **as 23 cobrancas da frota continuam de pe para
a O209**, que e exatamente onde o `!` dele as autorizou.

**ATO 2 — O GRAVADO.** `recalcular_fechamento --mes 10 --ano 2026 --colabs 174,235,382 --apply`:
`processados=3`, `fechamentos_mexidos=2 de 572`, `fechamentos_novos=0`. **UM campo de valor** se moveu
na frota -- `minutos_realizados +580` -- mais o par de DSR **pre-declarado FORA do alvo**:

```
col235  minutos_realizados   4677 -> 4854
col382  minutos_realizados   4056 -> 4459
col382  semanas_dsr_ok        1 -> 2   [FORA, DECLARADO: ponto/management/commands/desvio_o68b.py:29]
col382  semanas_dsr_perdido   4 -> 3   [FORA, DECLARADO: desvio_o68b.py:29]
```

Os outros **21 campos** das 572 linhas deram **0,00** (sao **24** campos de valor e **tres** se
moveram -- `minutos_realizados`, `semanas_dsr_ok`, `semanas_dsr_perdido` --, entao o resto e 21, nao
22; o numero errado era meu, contado a mao em vez de pela lista) -- a condicao (b) da `AVAL-DE-CRITERIO` medida
contra o **GRAVADO**, nao motor x motor. O DSR nao e surpresa e a razao ja estava escrita na casa antes
deste ato: *"DSR: fechar turno muda a contagem semanal de dias trabalhados"* (`desvio_o68b.py:29`,
que o lista em `FORA_DO_CRITERIO`); escritor unico `ponto/services/fechamento.py:563`. Move porque a
**segunda 2026-09-21** -- dia da cura -- mudou.

**POR QUE A ATA ANDOU -450 E O GRAVADO ANDOU +580.** Nao sao dois numeros do mesmo fato: sao duas
autoridades, e o `DiaPago(motor)` de prod **ja estava parcialmente drenado pelo recalculo por evento**.
Por dia, contra a foto FRESCA:

```
col174  2026-09-23     0.0 -> 148.0    +148.0  CENSO (cura)
col174  2026-09-24   148.0 -> 0.0      -148.0  CENSO (cura)
col235  2026-09-30    61.0 -> 240.0    +179.0  CENSO (cura)
col235  2026-10-01   420.0 -> 418.0      -2.0  CENSO (cura)      <- a ata caiu 240, o gravado tinha 420
col382  2026-09-21   420.0 -> 403.0     -17.0  CENSO (cura)      <- a ata caiu 389, o gravado tinha 420
col382  2026-10-03     0.0 -> 420.0    +420.0  deriva -- O210, desconta
```

**Nos dias do CENSO: +160.** **Fora do censo: UM dia**, `col382 2026-10-03`, cuja ata **NAO se moveu no
ATO 1** -- entao e **deriva pura de fechamento**, a O210, e este ato a **drenou**: `+420 min`. Era a
classificacao pre-escrita: dia fora do censo com ata movida seria **vazamento da O209** e mandaria
restaurar aquele colab da foto ANTES do ATO 2. Nao houve nenhum.

**O NUMERO DA O210 VOLTA A SER MEDIDO, nao subtraido de cabeca.** Os `174 de 572` e os `+55.507 min`
sao **historia de IMPACTO** de uma foto de antes deste ato (L-110): col235 e col382 vieram para o
presente agora, e o `+420` esta drenado. A O210 remede na sua propria foto, antes do seu proprio apply.

**OS 389 MIN DO `col382 09-21` NAO SE PERDERAM, e vale dizer por escrito** porque e o lugar onde um
leitor desatento concluiria que a frota perdeu hora de quem trabalhou. O turno pertence ao dia
**09-20**, que e competencia **09**. **A FRASE QUE EU ESCREVI NO MARCO DA O195 ESTAVA IMPRECISA e a
correcao e minha** (medido em `logs/o195_389min_col382_medido_20261005.md`, lido no banco de prod, nao
citado): eu dizia *"a ata de prod JA TEM em 09-20 e o TXT exportado JA carrega"*, e as duas metades nao
sao a mesma coisa. **O MINUTO esta la**: `DiaPago(motor)` de 09-20 tem `minutos_realizados=389` sobre
`minutos_previstos=420`. **A RUBRICA nao esta em lugar nenhum**: toda rubrica de HORA daquele dia e
**0,0**, e rubrica de hora e o que vira linha do TXT -- `minutos_realizados` nao e rubrica, e o unico
leitor dele em `folha/` e a **prontidao** (`folha/export.py:670`). Logo, **como DINHEIRO o turno que
comeca em 09-20 nao esta no TXT exportado da 09**. Isso NAO reabre nada aqui: quem os REMOVERIA numa
relavra e o **HEAD** e a cura os MANTEM, e o dono do que falta no TXT da 09 ja existe e e o aval
**PAUTA-DP-09-RELAVRATURA** de 15:4x, onde este dia e **uma linha**. O que morreu neste ato foi a
**dupla contagem**: a ata da 10 os tinha tambem em 09-21. E o gravado da 10 nem os tinha -- por isso o
dia andou **-17**, nao -389.

**`forcar` EM PROD: LEI ANTES DO PATCH, e a prosa estava envelhecida.** `grep` de `cartorio.py` e de
`julgar_colab` em `LEIS.md`/`CORTES.md`/`DOSSIES.md`: **nenhuma lei restringe a flag** (a L-085 protege
`ponto/turnos.py::turnos_do_colab`, outro sitio). O docstring dizia *"(SO sombra)"* e era **falso em
dois sentidos lidos no vivo**: `processar_cartorio.py:29` declara `--forcar` como porta de **producao**
(*"HX-BORDA-ATA: rejulga mesmo com impressao igual (backfill de ata)"*), e a propria `julgar_colab`
passa `forcar=True` sozinha na 2a passada do B5.3b, em prod, a cada cron das 06:28. A linha foi
**corrigida no commit deste marco**, com o censo de campos que a flag move (a uniao das duas
assinaturas de `ponto/portas/celula.py` -- este arquivo nao tem **UM** `.save(`) e com o que ela **nao**
protege: a JANELA. Foi por isso que `processar_cartorio --forcar` foi **recusado** como ferramenta: ele
so aceita `--empresa` e a fila dele e toda `CelulaDia` com `data < hoje`, **sem piso** -- reescreveria a
**09 EXPORTADA**. A forma certa foi `julgar_colab` direto, com a lista **INTEIRA** da janela e **piso em
`periodo_apuracao(10, 2026, 21)`**, que mantem a 09 fora **por construcao** (e lista inteira porque
`_julgar_colab_corpo:432` tira o intervalo de batida de `cels[0].data`/`cels[-1].data`: recorte por dia
fabrica par fechado com a cauda da vespera).

**DOIS AVAIS NOVOS REGISTRADOS** (PROMPT-NAO-SE-REPETE: linha em `PROMPTS.md`, item em
`PENDENTES_RONALD.json` por `python3 bin/gerar_avais.py --escrever` -- diff de 12 linhas, sem churn;
AVAIS caiu de **8** para **6**):
- **`O209`** (`!`): *"depois do apply dos 3 colabs da relavratura 10, relavra a ata da frota na 10 com
  DIFF publicado antes e reversao em logs; as 23 cobrancas nascem. 09 exportada intacta"*. Ordem
  **literal**: DEPOIS dos 3, **nunca embarcada neles** (LEI-AKITA 9 -- embarcar ampliaria o aval da
  relavratura). Marco PROPRIO, com foto nova da sombra e reversao dos ~89 colabs.
- **`PAUTA-DP-09-RELAVRATURA`**: *"mede primeiro o numero do DOMINIO (TXT da 09 gerado na sombra x TXT
  exportado, linha a linha) e me traz a pauta com ESSE numero"*. O numero da GRADE ja medido
  (291 dia-colab / 94 colabs / +142.778 min) **nao e** o numero da pauta. A porta (retificacao pela
  `REGEN-EM-EXPORTADA` ou correcao LA) volta **com** o numero.

**ORDEM VIVA depois deste marco:** (2) FECHADA -> **O209** (frota, `!` dado) -> **O197** -> O204 ->
O207 -> O44 itens 2-8 -> O198/O199/O200 -> O145 -> instrumento.

## O208 — O CONTADOR DO RECALCULO ERA CEGO AO CAMPO DA CURA (05/10, bug no caminho, RED→GREEN)

**Achado DENTRO da medicao do item (2), e por isso vem primeiro** (LEI-AKITA 6: bug provado no meio da
fatia cura na hora). `ponto/management/commands/recalcular_fechamento.py:36` declarava a sua PROPRIA
lista de campos -- `RUBRICAS`, **19** -- ao lado do `CAMPOS` canonico de **26** que o proprio
`ponto/services/fechamento.py:574` importa e **chama de canonico** por escrito. As duas listas **ja
tinham divergido em CINCO campos**:

```
minutos_realizados · dias_previstos · semanas_dsr_ok · semanas_dsr_perdido · horas_reflexo_dsr
```

`minutos_realizados` **e o campo da relavratura**. Entao o rotulo `fechamentos_mexidos=%d de %d`
(:136) nao contava fechamento mexido: contava *"fechamento cujas 19 rubricas mexeram"* -- exatamente o
que a LEI-AKITA 8 proibe (*"rotulo diz o que a conta faz; contador == universo"*).

**O PRECO, MEDIDO NA SOMBRA NO MESMO ATO** (`logs/relavra10_diff_20261005.out`):

| ato | o comando IMPRIMIU | a foto de 24 campos ACHOU | fator |
|---|---|---|---|
| ATO 2 (11 colabs da cura) | `fechamentos_mexidos=3 de 572` | **6** colabs | 2,0x |
| RUN C (competencia inteira) | `fechamentos_mexidos=36 de 572` | **174** colabs | 4,8x |

Os tres que o ATO 2 nao viu sao **col44, col235 e col382** -- os tres movem SO campos de fora do
`RUBRICAS`. A conta fecha pelos dois lados: os 3 que ele VIU (col250, col297, col935) sao exatamente
os que mexem `minutos_abonados`/`horas_trabalhadas`/`horas_saida_antecipada`/`turnos_abertos`, todos
dentro dos 19. No RUN C a diferenca e dominada por `minutos_realizados` (**137 colabs, +55.507 min**).

**E FOI O `36 de 572` QUE FOI PARA A MESA DELE**, no pendente `RELAVRATURA-10-PAROU-DIFF-SURPREENDE`
(AVAIS #7, 05/10 03:32). O universo verdadeiro e **174 de 572**. O numero do aval nao estava so medido
com o juiz defeituoso (isso a L-110 ja dizia): estava medido com um **contador cego**.

**SEGUNDO CONSUMIDOR, e e o que torna isso mais que cosmetico:**
`ponto/management/commands/aplicar_janela_he_total.py:145` delega o recalculo a esta porta
*"e ainda ganha a foto antes/depois que aquele comando grava por conta propria"* (comentario dele
mesmo, :141-143). A prova da L-092 e a trilha de auditoria daquele ato tambem corriam sobre a foto
cega. `FOTO_DIR` (`logs/recalculo/*.json`) guarda as duas fotos por colaborador -- e guardava 19
campos de 24, entao **nem o desfazer tinha o campo da relavratura**.

**A CURA E DE ORIGEM, nao de lista:** `RUBRICAS = tuple(c for c in _CAMPOS_CANONICO if c not in
('mes', 'ano'))`, importando o `CAMPOS` do mesmo sitio que o `fechamento.py` ja importa (o modulo nao
tem model no topo -- so `json`/`BaseCommand`/`transaction` --, entao nao ha ciclo). Mesma familia dos
**LABELS em quatro lugares** (24/09) e do `holerite` fora da regua (04/09): a forma certa existia e o
leitor principal nao migrou.

**RED EVIDENCIADO, 3 falhas, 0 errors** (`ponto/tests/test_o208_contador_do_recalculo.py`, contra COPIA
do HEAD por `bin/arvore_do_push.sh HEAD` + `bin/suite.sh --dir`):

```
FAIL test_MORDE_fechamento_que_move_SO_minutos_realizados
FAIL test_MORDE_os_cinco_campos_da_divergencia
FAIL test_rubricas_nao_perde_campo_do_campos_canonico
     AssertionError: [] != ['minutos_realizados', 'dias_previstos', 'semanas_dsr_ok',
                            'semanas_dsr_perdido', 'horas_reflexo_dsr']
Ran 3 tests -- FAILED (failures=3)
```

Depois da cura, na mesma copia: **3 OK**. Vizinhos do modulo rodados juntos (`test_s4_ninguem_recalcula_para_ler`,
`test_o80_selo_l092`, `test_k8_tela_abre_na_competencia`, `test_s3_leitor_nao_chama_motor`): **32 OK**.
`ruff check` nos dois arquivos: `All checks passed`.
Os dois primeiros selos MORDEM por COMPORTAMENTO (mover um campo sozinho e perguntar a `foto()` do
comando); o terceiro e ESTRUTURAL e existe para a divergencia nao renascer no dia que um campo novo
entrar no `CAMPOS` -- e a forma que a casa ja usa para o `LABELS`.

**SELO DE CONDUTA.** LEI-AKITA: origem=`ponto/management/commands/recalcular_fechamento.py::RUBRICAS`
(segunda lista de campos, apagada -- passa a derivar do `CAMPOS` canonico), testemunha=`CAMPOS` de
`aplicar_09_corte_b.py`, o mesmo que `ponto/services/fechamento.py:574` ja le,
RED=`ponto/tests/test_o208_contador_do_recalculo.py` (3 falhas no HEAD, 2 por comportamento + 1
estrutural), quem-mais-le=censo fechado -- `RUBRICAS` deste modulo nao e importado por ninguem (os
outros `RUBRICAS` do repo sao de `ponto/calculador/regras.py` e `diff_janela_he.py`, listas de
dominio diferente); o modulo tem 2 consumidores do COMANDO (`aplicar_janela_he_total.py:145` e
`config/crons.py:902`) e nenhum dos dois parseia a saida, juizes novos=0.

## ITEM (2) — RELAVRATURA 10: O DIFF MEDIDO, E POR QUE O APPLY AINDA NAO SOBE (05/10)

**A RELAVRATURA SAO DOIS ATOS, e isso foi LIDO na fonte, nao suposto.** O fechamento nao ve a cura da
O195 enquanto a ATA nao for relavrada: `folha/export.py:221::grade_do_fechamento` **le a celula
soberana** (`escala/services/leitor_celula.py::grade_da_celula`) e `ponto/services/fechamento.py:533`
deriva `minutos_realizados`/`minutos_abonados` de `ponto/services/dia_pago.py::por_dia_da_grade` sobre
ESSA grade. Entao:

- **ATO 1 — a ATA**, pelo cartorio com `--forcar`. E a ata **nao se relavra sozinha**:
  `ponto/services/cartorio.py::impressao_insumos` **nao hasheia a ata** (:132-133) -- carimba batidas,
  cobertura, chamados, DNA, veto e teto. Trocar o CODIGO do juiz nao move a impressao, entao **nenhum
  deploy e nenhum cartorio das 06:28 relavra dia passado**. (Esta e tambem a prova, pelo lado da
  CAUSA, de a 09 EXPORTADA ter ficado intacta no deploy da O195.)
- **ATO 2 — o FECHAMENTO**, pela porta que ja existe: `recalcular_fechamento --colabs` (nascida
  30/09 19:3x justamente para isto: *"com a lista, o apply alcanca so quem o ato alcancou"*).
- **NAO HA TERCEIRO ATO.** `escala/utils.py:1261/1295/1320` chama `realizado_do_dia`/`turnos_do_colab`
  e nada ali le `TurnoMaterializado` -- `recompute_turnos` nao entra.

**AS QUATRO FERRAMENTAS QUE EXISTIAM E NAO SERVEM**, cada uma com o motivo em uma linha:
`aplicar_09_corte_b` para mes=10 (recalcula a competencia INTEIRA e so restaura o `FechamentoMensal`
dos separados -> `soma(DiaPago) != FechamentoMensal` exatamente neles, e as vizinhas `(7, 8)` sao
literais); `processar_cartorio --forcar` (so aceita `--empresa`, e a fila dele e TODA celula com
`data < hoje` -- reescreveria a **09 EXPORTADA**); recortar `cels` nos dias da cura (`_julgar_colab_corpo`
tira a janela de `cels[0].data`/`cels[-1].data`, e fatia de um dia **fabrica par fechado** com a cauda
da vespera); `bin/restore_relavratura_10_2026.py` (e de competencia inteira -- nao reverte um ato de 11 ids).

### ATO 1 MEDIDO NA SOMBRA — 11 colabs, 154 celulas, `forcar=True`

```
cartorio: julgadas=154 carimbadas=154 protestos=24 emitidos=0 nunca_bateu=0
          furo_parcial=13 vetados=0 perguntas=0
```

**`emitidos=0` e `perguntas=0`: a relavratura nao alcanca pessoa.** Nenhuma cobranca nasce, nenhuma
pergunta e feita -- era o risco real do `--forcar` (os dias orfaos de col44/col451/col594/col876/col922
podiam emitir), e ele **nao se materializou**.

A ata se moveu em **7 dia-colab**, o veredito em **5**, e o realizado dos 11 na 10 foi de
**42.035 -> 42.131 min (+96 min)**. Os movimentos que importam:

```
col174  09-23  real    0 ->  148 | discordante -> concorde | protestos ['REALIZADO_ZERO_COM_TURNO',
                                     'BATIDA_ORFA_FORA_TOLERANCIA'] -> []            [censo]
col174  09-24  real  148 ->    0 | concorde -> indefinida   | [] -> ['PREVISTO_COM_DIA_INDEFINIDO'] [censo]
col235  09-30  real   61 ->  240 | furo -> furo                                      [censo]
col235  10-01  real  658 ->  418 | discordante -> concorde | ['REALIZADO_INFLADO'] -> []  [censo]
col382  09-21  real  792 ->  403 | discordante -> concorde | ['REALIZADO_INFLADO'] -> []  [censo]
col297  09-23  real    0 ->   60 | fato_sem_previsao (igual)        [deriva de ata -- O209]
col297  09-30  real    0 ->  486 | fato_sem_previsao (igual)        [deriva de ata -- O209]
col451  10-04  real    0 ->    0 | concorde -> fato_sem_previsao    [deriva de ata -- O209]
```

**OS TRES ULTIMOS NAO SAO DESTA CURA, e a prova e por CONSTRUCAO.** O censo da O195 foi um A/B da
FUNCAO REAL (`turnos_do_colab`, 541 colabs com turno, 12.362 turnos) e deu **`so_esq=0 so_dir=0`**:
a cura **nao cria nem destroi um par**. Se o conjunto de turnos de um colab e identico sob os dois
codigos, a ata que o juiz escreve e identica sob os dois codigos -- entao **a O195 nao pode mover um
dia que nao esta nas 58 linhas**. Conferido linha a linha: o dia do censo de col297 e `10-02 -> 10-01`
(e a ata dele moveu em 09-23 e 09-30) e o de col451 e `09-21 -> 09-20` (e a ata dele moveu em 10-04).
Sao **ata stale** contra o juiz de hoje -- o achado **O209**, logo abaixo, cuja causa ja estava
nomeada por escrito no proprio commit da O195: *"Zero deles e desta fatia: e 100%
CELULA-TURNO-FECHA"*.

**CERTIFICACAO do ATO 1 contra o numero que a O195 JA PUBLICOU** (coluna `juiz` de
`logs/sombra/o195_diff_cura.tsv` -- L-110, *"todo leitor le o mesmo numero"*):
**23 IGUAIS · 0 DIVERGEM** (307 celulas sem linha publicada, por serem dia fora do censo das 58).
A ata diz exatamente o que o juiz curado disse quando o DIFF foi publicado.

### ATO 2 MEDIDO — `recalcular_fechamento --colabs` com os 11

```
ESCOPO: 11 colaborador(es) nomeados -- a deriva de quem nao esta na lista NAO e escrita
processados=11 · fechamentos_mexidos=3 de 572 (o contador CEGO -- ver O208) · novos=0
foto de 24 campos: 6 colab(s) com campo movido · VAZOU para fora da lista: NENHUM
```

| campo | antes | depois | delta | colabs |
|---|---|---|---|---|
| `minutos_realizados` | 39.952 | 40.776 | **+824** | 3 |
| `horas_trabalhadas` | 654,20 | 663,31 | +9,11 | 1 |
| `horas_folga_trabalhada` | 9,11 | 0,00 | −9,11 | 1 |
| `horas_saida_antecipada` | 8,36 | 1,55 | −6,81 | 1 |
| `turnos_abertos` | 11 | 12 | +1 | 1 |
| `minutos_abonados` | 1.920 | 2.580 | **+660** | 1 |
| `dias_previstos` | 218 | 220 | +2 | 1 |
| `semanas_dsr_ok` / `semanas_dsr_perdido` | 17 / 38 | 19 / 36 | +2 / −2 | 2 |

Por colab, contra o criterio escrito ANTES do numero (`DENTRO` = o que se move como consequencia
direta de um turno trocar de dia; `FORA` = previsto, abono, falta, dobra, semana de DSR, banco):

```
col44    minutos_realizados +244
col235   minutos_realizados +177
col297   horas_folga_trabalhada -9,11  horas_trabalhadas +9,11
col935   horas_saida_antecipada -6,81  turnos_abertos +1
col250   dias_previstos +2!  minutos_abonados +660!  semanas_dsr_ok +1!  semanas_dsr_perdido -1!
col382   minutos_realizados +403  semanas_dsr_ok +1!  semanas_dsr_perdido -1!
```

Cinco dos 11 nao movem campo nenhum no mes -- e **col174 e o caso que explica por que isso nao e "nada
aconteceu"**: a ata dele moveu 148 min de 09-24 para 09-23 (os dois dias da cura), e o `DiaPago` dos
dois dias mudou, mas a SOMA do mes nao. **A verdade do DIA se moveu com o total parado.** Quem le o
espelho ve a diferenca; quem le so o fechamento, nao.

### A DERIVA, que e fatia PROPRIA pelo Aval 3 item 2

`RUN C` (a competencia inteira, depois do ATO 2, **sem** relavrar a ata dos outros -- entao e o gravado
velho contra a propria ata, deriva pura e nao cura):

```
minutos_realizados  527.012 -> 582.519  (+55.507 min = +925,1 h)   137 colabs
minutos_abonados    106.624 -> 126.824  (+20.200 min = +336,7 h)    24 colabs
dias_previstos        3.494 ->   3.527  (+33)                       29 colabs
horas_trabalhadas   9.712,53 -> 9.779,57 (+67,04 h)                  7 colabs
horas_falta            44,80 ->   76,93 (+32,13 h)                   4 colabs
+ saldo_banco_horas 5 · minutos_previstos 5 · turnos_abertos 3 · intra 2 · folga 2 · inconsist. 2 · incertos 1
```

**O numero da fatia da deriva e `174 de 572` colabs** (168 a mais que os 6 do ato), nao os `36` do
pendente. `RUN C` reproduziu o `fechamentos_mexidos=36` de 03:3x **exatamente** -- o que confirma, por
um terceiro caminho, que aquele numero media DERIVA e nao cura, e que o `36` e o contador cego do O208.

### A 09 EXPORTADA, nos tres atos

```
antes de tudo       hash=57ec4cf0fbb38d203df25baac92261cb linhas=17332
depois do ATO 1     hash=57ec4cf0fbb38d203df25baac92261cb  INTACTA
depois do ATO 2     hash=57ec4cf0fbb38d203df25baac92261cb  INTACTA
depois do RUN C     hash=57ec4cf0fbb38d203df25baac92261cb  INTACTA
```

### A LISTA FINAL DO APPLY, pelo criterio do aval: **3 colabs**

O Aval 3 item 2 diz *"aplica SO o realizado dos **dia-colab** da cura"* -- o criterio e por **DIA**,
nao por campo. Cruzando a ata movida com as 58 linhas do censo, **a ata se moveu em dia DO CENSO em
tres colabs**, 5 dia-colab:

```
col174  09-23 (+148) e 09-24 (-148)   linha do censo: 09-24 -> 09-23      net   0
col235  09-30 (+179) e 10-01 (-240)   linha do censo: 10-01 -> 09-30      net -61
col382  09-21 (-389)                  linha do censo: 09-21 -> 09-20      net -389
                                                                    TOTAL  -450 min
```

**O -450 FECHA COM O NUMERO QUE A O195 PUBLICOU** (`8fce4967`: *"a cura MOVE 34 (comp09 trab +168,
comp10 trab **-450**)"*) -- terceira certificacao independente do mesmo numero, por um caminho que
nao e o da medicao original.

E para esses tres, **o `forcar` escreve SO cura**: nenhum dos tres tem ata movida em dia fora do
censo. Nao ha preco de deriva no ATO 1.

**POR QUE OS OUTROS OITO FICAM FORA**, um a um:

| colab | o que moveu | por que fica fora |
|---|---|---|
| col297 | ata em 09-23 e 09-30 | dia do censo dele e `10-02 -> 10-01`; os dois dias sao **O209** |
| col451 | ata em 10-04 | dia do censo dele e `09-21 -> 09-20`; 10-04 e **O209** |
| col44 | DiaPago em 10-04 | ata **nao** moveu; 10-04 nao e dia do censo -> deriva de motor |
| col250 | DiaPago em 09-30 (dia do censo) + 10-02/03/04 | ata **nao** moveu; a linha de 09-30 e `(0,0,0) -> None`, **0 minuto**; o resto e deriva |
| col935 | DiaPago em 09-26 (dia do censo) | ata **nao** moveu; linha `(0,0,0) -> None`, **0 minuto** |
| col594 · col876 · col922 | nada | a cura move turno ORFA neles (0 min nos dois dias) |

### O PRECO QUE O ATO 2 CARREGA, e ele e NOMEADO, nao surpresa

**`recalcular_fechamento` recalcula o MES, e isso e lei escrita** desde 26/09 (AVAL-DE-CRITERIO:
*"Apply por recalculo nunca e cirurgico"*). Entao, mesmo restrito a 3 colabs, o ATO 2 traz a deriva
do mes **deles** junto. Medido, por colab:

```
col174   cura  0 min  |  deriva  0        -> nenhum campo de mes se move (os 2 dias se cancelam)
col235   cura +177    |  deriva  0        -> minutos_realizados +177, 100% cura
col382   cura  -17    |  deriva +420      -> minutos_realizados +403 = -17 (09-21, teto do previsto)
                                             + 420 (10-03, dia que a ata NAO moveu = O209/deriva)
         e semanas_dsr_ok +1 / semanas_dsr_perdido -1
```

O `semanas_dsr_*` de col382 e campo **FORA do alvo**, e o motivo ja esta escrito na casa --
`ponto/management/commands/desvio_o68b.py:29`: *"DSR: fechar turno muda a contagem semanal de dias
trabalhados"*. Nao se re-deriva aqui: o escritor unico e `ponto/services/fechamento.py:563`, a partir
de `motor_calculo_v2:1839/2659`. Ele se move porque o realizado da **segunda-feira 09-21** mudou, e e
consequencia direta do dia da cura -- nao deriva.

**O +420 de col382 em 10-03 e o unico minuto de deriva que o ato carrega.** Ele sai da conta da fatia
da deriva (senao conta duas vezes), e vai publicado aqui por nome. `col174` e o caso que mostra por
que isso nao e "nada aconteceu": a ata dele moveu 148 min de 09-24 para 09-23 e **a soma do mes nao
mudou** -- a verdade do DIA se move com o total parado. Quem le o espelho ve; quem le so o
fechamento, nao.

### O QUE FALTA ANTES DO APPLY -- dois itens, nenhum deles `PAREI`

1. **A CONDICAO 2 SAO TRES TABELAS, escopadas nos 3.** `logs/o195_reversao_20261005.json` ja cobre
   `CelulaDia`; faltam `FechamentoMensal` e `DiaPago` dos tres ids, e
   `bin/restore_relavratura_10_2026.py` e de competencia inteira -- nao reverte um ato de 3 ids. O
   restore ESCOPADO nasce em `logs/` antes do apply, dizendo por escrito que `versao='oraculo'` **nao**
   e restaurada e **nao tem leitor de dinheiro** (`fechamento.py:668`: quem le e `calendario.py` e
   `folha/porta_export.py`, e os dois leem `'motor'`).
2. **Em prod, conferir `emitidos=0` e `perguntas=0` do PROPRIO cartorio antes do ATO 2.** Na sombra
   deu 0 nos dois, mas a sombra roda com as saidas DESLIGADAS -- o numero dela nao prova o de prod.
   Na frota o mesmo ato deu `emitidos=23` (O209), entao o contador nao e decorativo.

**O ALCANCE ESTA FECHADO, e nao por recorte.** A pergunta *"a cura alcanca mais que os 11?"* se
responde pela AUTORIDADE, nao pelo censo: `so_esq=0 so_dir=0` em 12.362 turnos de 541 colabs **e** a
medicao do envelope, e ela e ZERO. O `forcar` sobre a frota inteira, que eu rodei para fechar isso,
mediu **outra coisa** -- e virou o O209.

**SELO DE CONDUTA.** LEI-AKITA: origem=`escala/utils.py`->`ponto/turnos.py` (a cura da O195, ja no ar) +
a ATA que nao se relavra sozinha (`cartorio.py::impressao_insumos` ata-cega), testemunha=`CelulaDia.ata`
lida por `folha/export.py::grade_do_fechamento` e a coluna `juiz` publicada pela O195,
RED=nao se aplica (medicao, nada escrito em prod), quem-mais-le=os 2 atos sao as DUAS portas que ja
existem (`julgar_colab` e `recalcular_fechamento --colabs`); nenhum caminho novo, juizes novos=0.

## O209 — A ATA DA FROTA ESTA STALE CONTRA O JUIZ DE HOJE (achado 05/10 15:0x, fatia PROPRIA)

**O instrumento que eu montei para fechar o alcance da O195 mediu OUTRA COISA, e a outra coisa e
grande.** ATO 1 com `forcar` sobre **TODO** colab com celula na 10 (564 colabs, 16.883 celulas),
na sombra, por cima do RUN C:

```
cartorio: julgadas=7527 carimbadas=7859 protestos=738 emitidos=23 nunca_bateu=332 vetados=3
A ATA SE MOVEU em 190 dia-colab, em 89 colab(s) -- dos 11 do censo: NENHUM
soma do realizado nos dias movidos: 17.890 -> 82.057  (+64.167 min = +1.069,5 h)
ATO 2 (recalcular_fechamento --colabs, os 89): mexidos=17 de 572 · VAZOU: NENHUM
  minutos_realizados +2.649 (16 colabs) · minutos_previstos -569 (4) · dias_previstos -2 (4)
  horas_noturnas +29,32 (1) · horas_trabalhadas -17,69 (1) · inconsistencias -2 (1)
  APLICAVEIS (so DENTRO): 13 · SEPARADOS (previsto/dias_previstos): [107, 146, 317, 879]
DiaPago motor: 30 linhas movidas · 09 EXPORTADA: hash INTACTA nos dois atos
```

**A ATRIBUICAO E AIRTIGHT, por DOIS caminhos, e nenhum deles e amostra:**
- **88 dos 89** estao DENTRO do universo do A/B da O195 (541 colabs, 12.362 turnos) e **nao estao nas
  58 linhas**. Com `so_esq=0 so_dir=0`, turnos identicos sob os dois codigos => ata identica sob os
  dois codigos. (A unica excecao aparente, col489, tem linha de censo em **2026-08-31** -- agosto,
  fora da 10.)
- **col955** esta FORA do universo do A/B: nao tem turno na janela, entao nao ha o que a O195 mova.

A causa ja estava escrita, por nome, no commit da propria O195: *"Zero deles e desta fatia: e **100%
CELULA-TURNO-FECHA**, no ar desde 10:20 de hoje"* (`6319b10c`, que matou o segundo juiz de *"quantos
minutos o dia realizou?"*). A **forma** dos dias confirma: `real 0 -> 420..720` com o veredito
**INALTERADO** em `fato_sem_previsao`, em dia alternado, mes inteiro, nos 12x36 (col203, col242,
col245, col325, col165...). Nao e turno trocando de DIA -- e o **realizado** da ata, que o juiz morto
escrevia como 0.

**POR QUE ISSO E FATIA, e por que e urgente de um jeito diferente:**
1. **Ninguem relavra.** `impressao_insumos` nao hasheia a ata (`cartorio.py:132-133`), entao nem
   deploy nem o cartorio das 06:28 alcancam dia passado. **So `--forcar` alcanca.** A ata vai ficar
   stale indefinidamente.
2. **O fechamento quase nao sente (`mexidos=17`) e isso e o que engana.** O RUN C ja drenou a deriva,
   entao o dinheiro ja esta no juiz de hoje. Quem mente e a **ATA** -- e quem le ata e a TELA, o
   espelho e o PDF. **A testemunha que o colaborador ve esta 1.069 h atras do que a folha calcula.**
   E a LEI-AKITA 2 pelo avesso: os leitores leem a mesma autoridade, e a autoridade esta velha.
3. **Relavrar a frota ALCANCA PESSOA: `emitidos=23`.** O ATO 1 dos 11 deu `emitidos=0`; na frota
   nascem 23 cobrancas. Relavratura de frota **nao e ato tecnico silencioso**, e e por isso que ela
   nao entra de carona em nenhum outro apply.

**NAO EMBARCA no apply da relavratura 10**: o aval e *"SO o realizado dos dia-colab da cura"*, e estes
nao sao da cura -- embarcar seria ampliar aval (LEI-AKITA 9). Vai como item proprio no BACKLOG, com o
numero dele e com os 23 `emitidos` nomeados na frente.

**SELO DE CONDUTA.** LEI-AKITA: origem=a ata de `CelulaDia`, lavrada por codigo anterior a
CELULA-TURNO-FECHA e nunca relavrada (so `forcar` alcanca), testemunha=`CelulaDia.ata` x o
`julgar_colab` de hoje, RED=nao se aplica (achado de medicao, nada escrito em prod -- a cura dele e a
fatia), quem-mais-le=TELA/espelho/PDF leem ata; o dinheiro NAO (o RUN C provou: `mexidos=17` de 89),
juizes novos=0.

## O195 — O DIA DO TURNO SE DECIDIA POR 17 SEGUNDOS (05/10, cura MEDIDA, **FECHADA e NO AR**)

**PROVA:** commit `8fce4967`, e o codigo esta no ar MEDIDO em 07/10 -- no worker vivo do `saas_ui`
`ponto.turnos._teto_s_da_jornada` responde (`hasattr` = True) e a sub-guarda de saida de `_data_do_turno`
ja carrega a tolerancia; o deploy que recarregou as tres cascas juntas e `logs/deploy_o208_20261005.out`
(rc=0, tres rotas provadas, `importerror_500=0`).

**O ACHADO, em uma linha**: a guarda de saida de `ponto/turnos.py::_data_do_turno` comparava o INSTANTE
da batida ao marco `hf` do dia -- **tolerancia ZERO** --, entao `07:50:17` contra `hf 07:50` mandava o
plantao INTEIRO para o dia seguinte, que e FOLGA. col382 **17 s**, col250 **29 s**, col235 **7 s**. E a
casa pagava duas vezes: o dia de trabalho ficava sem realizado (ninguem o cobrava) e o dia de folga
ganhava minuto que nao e dele.

**TRES [nome], nao um** (MEIA-CORRECAO E PIOR QUE NENHUMA):
  (A) `_data_do_turno`, a sub-guarda de saida com tolerancia zero (O93/aval 27/09);
  (B) `_mk`, `elif saida is not None: data_turno = data_local(saida.timestamp)` -- um `S` orfao tomava a
      data CIVIL da saida e o juiz da vespera **nunca era consultado**;
  (C) achado pelo PROPRIO RED: a guarda da BUG-145 em `_vespera` media **EXISTENCIA** de turno na
      vespera quando a pergunta dela e **FIM de jornada**.

**O TETO E A FORMULA QUE A CASA JA TEM, nao um numero novo** (BUG-144 proibe teto inventado): `jornada
prevista + 4 h`, com fallback `cont_max_s`. Ela vivia em DOIS escritores (O68b 10/09 e O95 27/09) e no
mesmo ato virou **UM** corpo, `_teto_s_da_jornada`, com **tres** leitores (`:81`
`_cabe_na_jornada_da_vespera`, `:625`, `:710`). **juizes novos = 0.**

**RED**: `ponto/tests/test_o195_dia_do_turno_por_envelope.py`, **19** testes, **6 VERMELHOS** contra
`git show HEAD:`. Mais `test_o93_dia_da_jornada.py` (o `_ts` ganhou SEGUNDO -- escrito em minuto redondo,
o selo da O93 passava raspando pela guarda que julga em segundos) e os **3** selos da L-103 em
`escala/tests/test_montador_realizado_pela_autoridade.py` INVERTIDOS, cada um com controle positivo
provando que os dois gates ainda mordem no realizado-zero. Suite inteira: **Ran 9638 / OK (skipped=42)**.
`bin/tests/` 62 selos, 0 vermelhos. ruff limpo.

### O CENSO E FECHADO, NAO AMOSTRA (L-110 item 4: leitura UNICA do caminho velho, como IMPACTO)
Pela porta real `turnos_do_colab`, 543 colabs, 21/08-20/10: **12.362** turnos, `so_esq=0 so_dir=0` -- a
cura nao cria nem destroi UM par. **58** trocam de dia, **0** na direcao errada. Por DESTINO comp09=47 /
comp10=11; por ORIGEM comp09=44 / comp10=14 (o enquadramento da borda e por DESTINO).
Anatomia: **17 par FECHADO** (os unicos que carregam minuto) + **37 orfa de SAIDA** + **4 orfa de
ENTRADA**. Orfa vale 0 nos DOIS dias: ela muda a MARCACAO (Portaria 671), a lista `orfas` da ata e o gate
`BATIDA_ORFA_FORA_TOLERANCIA`, nunca o minuto. Logo **34 dia-colab** mudam de numero, nao os 106 tocados.
Leitores: **47** sitios de producao em 7 apps (`parear_turnos` 11, `turnos_de_batidas` 3,
`turnos_do_colab` 33), censo AST em `logs/o195_censo_leitores_portas.out` -- **nenhum com regra propria de
dia**.

### O DIFF DE DINHEIRO (condicao 1 da DINHEIRO-EM-COMPETENCIA-ABERTA) — `logs/o195_diff_dinheiro.md`
Sombra REFEITA hoje (SOMBRA_DUMP=20261005, BLOCO 14:01, DIVERGE=0, ERROS=0, STATUS=OK). Substrato
conferido: a ata que a sombra le, dia a dia, contra `logs/o195_ata_prod.out` (SELECT puro em prod) =
**106 de 106 iguais, 0 divergem**. Arreio unico `logs/sombra/rodar_na_sombra.sh` com `APP_SOMBRA`, a
MESMA sonda nas duas arvores -- a unica forma de um DIFF de CODIGO nao virar DIFF de DADO.

Tres numeros, no nivel da ATA (min):

    (1) ata -> juiz_HEAD   = A REGRESSAO JA EM VOO    comp09 trab -2051 · folga +7600 · comp10 trab -665 · folga +604 · TOTAL +5488
    (2) head -> cura       = O QUE A CURA MUDA        comp09 trab +2219 · folga -1890 · comp10 trab +215 · folga -604 · TOTAL  -60
    (3) ata -> cura        = o que a proxima lavra escreve  comp09 trab +168 · folga +5710 · comp10 trab -450 · TOTAL +5428

**O CORTE QUE ATRIBUI OS NUMEROS** -- (3) soma coisas de donos diferentes:
  **(A)** os **34** dia-colab que a cura MOVE -> comp09 trab **+168**, comp10 trab **-450**, folga +-0,
          TOTAL **-282**;
  **(B)** os **72** que ela NAO move (`head == cura` linha a linha) -> os **+5.710 de folga da comp09
          vivem INTEIROS aqui**. Entao **zero deles e O195**: e 100% CELULA-TURNO-FECHA, no ar desde 10:20.

Os **-282** fecham par a par: **-722** em 6 pares que o caminho morto DOBROU (col174 -92 e -60, col200
-60 e -60, col235 -61, col382 -389) **-60** do col302 (abaixo) **+500** em 3 que ele SUB-contou (col489
+180, col490 +237, col515 +83). E `head_2d == cura_2d` em **16 dos 17** pares: o total dos DOIS dias e o
mesmo para os dois juizes -- muda em QUAL dia o minuto e arquivado. **12 de 13** colabs fecham em 0.

**O NUMERO QUE CHEGA AO DINHEIRO** (a ata nao e a folha): `ponto/services/fechamento.py:533` soma
`por_dia_da_grade` (`ponto/services/dia_pago.py:314-342`, UMA derivacao, dois chamadores), que carrega
universo `tipo_dia in ('trabalho','ausencia')`, abono = previsto da AUSENCIA, e **teto POR DIA
`min(realizado, previsto)`** (F1 04/08, "HE de um dia nao tapa furo de outro"). Depois do teto, so em dia
de trabalho:

    comp      ata_cap  head_cap  cura_cap   cura-ata   cura-head
    2026-09      8774      6723      9589     + 815      +2866
    2026-10      4232      3567      4392     + 160      + 825
    TOTAL       13006     10290     13981     **+975 (+16,25 h)**   **+3691 (+61,52 h)**

E aqui aparece o que a ata escondia: o delta capado da 09 (**+815**) e MAIOR que o cru (+168) -- o dia que
o caminho morto DOBROU ja era truncado pelo teto, e o que ele SUB-contou passava inteiro. **O defeito
custava dinheiro ao colaborador**, e a cura devolve. Deixar o HEAD como esta custa **-61,52 h**.

**FOLGA NAO DOBRA** (lido na fonte, nao temido): `por_dia_da_grade` exclui folga do universo, entao
realizado de folga nunca alcanca `FechamentoMensal.minutos_realizados`; e `horas_folga_trabalhada` sai do
MOTOR (`dia_pago.py:221`, `p.minutos_trabalhados`), nunca da ata. Fontes distintas, universos disjuntos.

### A BORDA DA COMPETENCIA: NAO HA PAREI, e a cura e quem PRESERVA
Dos 58, **3** cruzam a borda, os tres de `2026-09-21 (comp10)` para `2026-09-20 (comp09)`: col44, col382,
col451. **Dois sao orfas e valem 0 min.** So o **col382** e par fechado, **389 min** -- e o `DiaPago`
de prod **JA TEM** esses 389 em 09-20 como **MINUTO** (`minutos_realizados`), com **toda rubrica de
hora do dia em 0,0**: como dinheiro o turno **nao esta** no TXT exportado da 09 (medido em
`logs/o195_389min_col382_medido_20261005.md`; a primeira versao desta linha dizia "o TXT ja os
carrega", e era imprecisa). Quem os REMOVERIA numa relavra e o **HEAD**; a **CURA os MANTEM**, e o que
falta no TXT da 09 tem dono: o aval **PAUTA-DP-09-RELAVRATURA**. A L-092 nao e furada aqui: ela e cumprida pela cura.

### CONDICAO 3: O DEPLOY NAO REENFILEIRA DIA DA 09 (lido na fonte)
`ponto/services/cartorio.py:550-563` seleciona as batidas do julgamento por **DATA CIVIL do timestamp**,
nunca por `data_turno`:
`bj = [b for b in bs if data - 1d <= _tz.localdate(b.timestamp) <= data + 1d]`, e `impressao_insumos`
(`:85`, comentario em `:132-133`) carimba batidas/cobertura/chamados/DNA/veto/teto -- **e nao a ata**.
Logo a cura, por si, **nao muda a `impressao` de nenhum dia da 09** e nao pode reenfileirar um dia
exportado. O que reenfileira segue sendo o que sempre reenfileirou: batida, cobertura, chamado ou DNA
mudando. A cura muda o VALOR que uma lavra -- disparada por OUTRA coisa -- escreveria.
Alcance de escrita pelos chokepoints: `ponto/signals.py::_recompute_around` usa
`data_local(batida) - 2d .. + 1d` e `_recompute_escala` usa `localdate() - 2 .. + 1`, entao trafego de
rotina em 05/10 so alcanca 03/10-06/10 = **comp10**; a 09 so por batida ou escala RETROATIVA.
`data_turno` nao existe na `Batida`: ele e escrito no `TurnoMaterializado` por
`ponto/nucleo.py::recompute_turnos` (apaga-e-reconstroi, dentro de `transaction.atomic`).
**EXPOSICAO PREEXISTENTE, nomeada e NAO criada aqui**: `ponto/portas/celula.py::lavrar_veredito` (`:508`)
e `ponto/management/commands/processar_cartorio.py:49-62` **nao tem filtro de exportada/trancada**.

### CONDICOES 2 E 4
`logs/o195_reversao_20261005.json` (SELECT puro em prod, 14:15): a CELULA INTEIRA dos **106** dia-colab
(ata, dna, veredito, veredito_via, veredito_em, impressao, julgada_em, origem, trabalha, insumos_em) e o
`FechamentoMensal` 10/2026 dos **30** colabs, 37 campos cada. Hash da 09 **ANTES** -- atas INTEIRAS da 09
dos 30 colabs, **903** linhas, janela 2026-08-21..2026-09-20, JSON canonico:
**`37a29deb2a11b39dbc94ec7ef1ba6bc7`** (+ md5 por colaborador no mesmo arquivo). O mesmo hash se refaz
DEPOIS do deploy e entra aqui.

### O UNICO PAR QUE NAO CONSERVA: col302, -60 min, causa NOMEADA
`logs/sombra/o195_col302_autopsia_20261005.py`, rodada nas duas arvores. 09/09 e dia de TRABALHO dele e o
DNA declara pausa **01:00-02:00**; ele bate `01:00:02 -> 06:00:07` (300 min) mais uma orfa de saida
`00:00:53`. O **HEAD** arquiva o turno em **10/09**, onde `trabalha=False` e `intervalo_do_dia=(None,
None)` -- nenhuma pausa descontada, **300 min**, `janela_descontada=0`. A **CURA** arquiva em **09/09**,
o dia que o turno E: `janela_descontada=60` -> **240 min**. Os 60 min sao a INTRAJORNADA que o DNA
declara e que o HEAD deixava de descontar por arquivar o plantao num dia de folga. E a tese da cura
aparecendo no numero.

### L-085: O RODAPE DELA FECHA, 10 de 10
Os 10 dia-colab que `logs/o191/passo5_frota_classes_20261004.txt` isolou (`L085_segundos` 6 dias, overrun
7s..59s; `L085_overrun` 4 dias, 159s..3617s) foram medidos um a um nas duas arvores: **10 CURADOS, 0
inalterados, 0 piores** -- col489 08-31 420->0, col382 10-01 364->0, col515 09-16 330->0, col852 08-22
305->0, col302 09-10 300->0, col277 09-01 280->0, col250 09-30 240->0, col490 09-01 237->0, col654 08-25
17->0, col454 09-02 1->0. O rodape `NAO cobre a entrada que cruza -- RED col382, obra O76` deixa de
existir, e a celula da L-085 passa a **vigente, com selo**.

### TELA x FOLHA DIVERGEM AGORA (nao e da cura; e o que a cura reduz)
`espelho.py:300-301` chama `realizado_do_dia` AO VIVO; a folha le a ata. Desde o deploy das 10:20,
`juiz_head != ata` em **33 dia-colab de 14 colabs**: a tela soma **8.204 min** onde a folha tem **2.716**.
A coluna `head` deste DIFF e literalmente o que tela e PDF mostram neste momento.

### A CORRIDA, dita por inteiro
`6319b10c` (CELULA-TURNO-FECHA) pousou 10:19 e deployou 10:20, matando o segundo juiz do realizado da
ata. As atas destes dias foram lavradas em **02/10 17:50** pelo caminho que morreu (65 das 106). O
cartorio enfileira por IMPRESSAO DE INSUMO, nao por versao de codigo: **nao ha relavra agendada**, o dano
pousa no primeiro movimento de insumo. Por isso a cura vem ANTES, e por isso o numero de (1) e **historia
pre-cura**, nao projeto desta fatia.

**SELO DE CONDUTA.**
`LEI-AKITA: origem=ponto/turnos.py::_data_do_turno/_mk/_vespera, testemunha=marcos do DNA congelado via
_cabe_na_jornada_da_vespera + CelulaDia.ata, RED=ponto/tests/test_o195_dia_do_turno_por_envelope.py (19;
HEAD 6 falhas) + test_o93_dia_da_jornada.py + os 3 selos L-103 invertidos, quem-mais-le=47 sitios de
producao em 7 apps (parear_turnos 11, turnos_de_batidas 3, turnos_do_colab 33), censo AST em
logs/o195_censo_leitores_portas.out, juizes novos=0`

### NO AR, EMPURRADO E SMOKADO -- a condicao 4 fechada no ar, nao na sombra
`bin/deploy.sh --sem-migrate` as **14:2x** (0 migration pendente, carimbo da sombra `dia=20261005
status=OK tipo=completa diverge=0 erros=0`, prova de casca com 599 rotas nos 2 urlconfs, 3 rotas
provadas, `importerror_500=0`), e o push pousou as **14:40** (`cc4cec4c..8fce4967`, suite **9638 OK**
no commit empurrado + control-plane 22 OK).
**O SMOKE DE PROD PERGUNTA AO JUIZ QUE ESTA ATENDENDO** (`logs/o195_smoke_prod_20261005.out`, leitura
pura, 1 colab / 4 dias, chamando `turnos_do_colab`/`realizado_do_dia` REAIS):
```
2026-09-20  minutos=389   sem_turno=False  turnos: 2026-09-20[00:00:40->07:51:45]
2026-09-21  minutos=403   sem_turno=False  turnos: 2026-09-21[23:45:58->07:50:28]
2026-10-01  minutos=None  sem_turno=True   turnos: (nenhum)
2026-10-02  minutos=418   sem_turno=False  turnos: 2026-10-02[15:02:49->22:58:57]
```
O 09-20 **mantem os 389** (o unico par que cruza a borda, e o TXT exportado ja os carrega -- o HEAD os
REMOVIA), os turnos que cruzam a meia-noite ficam arquivados no dia que COMECARAM, e o 10-01 e o
`364 -> 0` da cura pousando como **`sem_turno=True`**, nunca zero cravado (L-103).
**CONDICAO 4 da DINHEIRO-EM-COMPETENCIA-ABERTA: o hash da 09 EXPORTADA refeito depois do deploy voltou
IDENTICO** (`37a29deb2a11b39dbc94ec7ef1ba6bc7`, 903 linhas, 30 colabs) -- `VEREDITO: competencia 09
INTACTA`. E agora ha a razao pelo lado da CAUSA, lida na fonte e nao suposta:
`ponto/services/cartorio.py::impressao_insumos` **nao hasheia a ata** (:132-133) -- ela carimba batidas,
cobertura, chamados, DNA, veto e teto. Trocar o CODIGO do juiz nao move a impressao, entao **nenhum
deploy relavra dia passado**: nem a 09, nem a 10. Quem relavra e o `--forcar`, e e esse o ATO 1 do item
(2) abaixo.

**MARCO FECHADO -- pode compactar** (L-108; `bin/handoff_sessao.sh` rodado, `app/docs/HANDOFF-SESSAO.md`
46 linhas).

**ESTADO (05/10 10:2x): o item (1) esta FECHADO -- a celula `turno/marcos x um juiz por pergunta` ficou
VERDE e o placar foi a `contratos 14/20`**
PROVA: `escala/utils.py::minutos_realizados_do_dia` **APAGADA** com lapide (`:822`), os **6** testes de
`escala/tests/test_realizado_intervalo.py` foram com ela, `PENDENTES['turno/marcos']` saiu de **1 (o ULTIMO dos 23 que a familia teve)** para **()**
com a vaga nomeada, e **quem disse o numero foi a funcao real**: `verdes()=14`, `total()=20`,
`linha_do_placar()='contratos_estruturais: 14/20 verdes'`, `fora_de_autoridade('turno/marcos')=0` e
`celula('turno/marcos','um juiz por pergunta')['verde']=True`, tudo rodado no container contra a copia da
fatia -- `logs/placar_estrutural/contratos_20261005_turno.txt`. O `!` que autorizou e o da **L-111**
(*"sitio com zero chamador de producao AINDA responde enquanto existir no codigo"*), lei escrita no
LEIS.md, no CORTES e no CLAUDE.md **no mesmo marco** em que foi aplicada.
**O passo 6 do CELULA-TURNO-FECHA NAO se carimba com isto, e nao e meia-correcao: e numero remedido.** A
nota do placar previa `+2` (as duas celulas de `um juiz por pergunta` que citavam o sitio) e saiu **`+1`**.
As duas tinham o mesmo sitio e **nao a mesma CONDICAO**: `celula/precedencia` tem allowlist ZERO desde a
BUG-145 e segue `verde=False` **DE PROPOSITO**, porque allowlist zero e UMA das duas condicoes dela -- a
outra, o numero chegar ao **GRAVADO**, so se cumpre na relavratura (a impressao do cartorio nao hashea a
grade, `ponto/services/cartorio.py:93-106`). Carimbar verde ali seria selo falando por efeito que ainda
nao houve. A nota do R6 foi corrigida no mesmo ato, com o motivo escrito.
**DE CARONA, UM BUG DE MAIN VERMELHO QUE NAO ERA MEU E VEIO PRIMEIRO (LEI-AKITA 6).** O gate temporal das
06:00 (`3a9bccaa`) voltou a tela do O122 ao commit aprovado -- escopo literal do aval, *"SO ELES"* --, mas
a etapa 1 havia pousado em **tres** arquivos (`60a4a42d`: o template, `app/escala/views.py` +26 e um selo
de 211 linhas). Voltar um e deixar dois deixou o selo recortando uma barra que nao esta mais no HTML:
**6 testes vermelhos** (1 FAIL + 5 ERROR `ValueError: substring not found` em `recorta_barra`), e com
main vermelho **todo push e, pela DEPLOY JA, todo deploy estavam travados desde as 06:00**. Provei que o
vermelho **preexiste** ao meu trabalho rodando o modulo contra a arvore viva, que tem diff de `.py` ZERO
contra HEAD. Cura pela CURA-MAIS-RESTRITIVA, em commit PROPRIO e antes do marco: o selo orfao foi apagado,
completando a reversao que o gate deixou pela metade; a volta da etapa inteira cabe num comando
(`git checkout 60a4a42d -- app/templates/escala/tipos_lista.html app/escala/tests/test_o122_etapa1_barra_em_tipos.py`)
e espera o smoke dele. O defeito do gate -- reverter por ARQUIVO e nunca medir a arvore depois -- virou a
**O205**, fila 2, instrumento.
LEI-AKITA: origem=`escala/utils.py:822` (a funcao, nao o leitor), testemunha=`ponto/turnos.py::realizado_do_dia`,
RED=`test_MORDE_pendente_curado_sai_da_lista` da propria casa + caso novo de EXISTENCIA por AST, quem-mais-le=censo
de chamadores por AST (**0** de producao; so comentario, lapide e teste), juizes novos=0.

**MARCO ANTERIOR (05/10 02:1x): a O191 esta FECHADA e NO AR**
PROVA: `RC_DEPLOY=0`, suite `Ran 9629 / OK (skipped=42)`, 61 selos de host `vermelhos: 0`, prova depois
**4/4**, exportada 09 com **8** registros hash a hash (`diff` vazio), push `024608c7..eef236e1`.
-- commit `fdd6f42c`, deploy
`--sem-migrate` as 02:08:12 com `RC_DEPLOY=0`, sobre a suite `Ran 9629 / OK (skipped=42)` RC=0
(`logs/o191/suite_copia2_20261004.out`) e 61 selos de host com `vermelhos: 0`. O ensaio na sombra veio
ANTES de aplicar os `.py` (L-107) e carimbou `dia=20261005 status=OK tipo=completa diverge=0 erros=0`,
com o bloco `69/69 · erro=0`. **A PROVA DEPOIS bateu 4/4** em quatro dia-colab nomeados lidos com o
codigo no ar (`logs/o191/prova_depois_20261005.txt`): o numero que a sombra mediu e' o que subiu, a ata
**nao se moveu** e o `sem_turno` viaja ao lado do zero. **Exportada 09 INTACTA**, 8 registros hash a hash
(`diff` vazio entre `..._antes_` e `..._depois_20261005.txt`). Smoke com trafego real: 29 respostas, **0
de 5xx, 0 traceback**. A leitura UNICA de impacto (secao de 01:1x, L-094 e, por **L-110**, medida de
impacto e nunca validacao) mede o IMPACTO NA GRADE -- e **esta linha dizia "o que a relavratura vai
levar ao gravado", o que e FALSO e eu provei falso as 03:4x (secao do PAROU abaixo)**: **397 dia-colab / 114 colabs
ganham numero (+197.933 min = +3.298,88 h)**, **15 / 12 colabs zeram (-2.716 min = -45,27 h)**, **0
`SOBE`, 0 `DESCE`, 0 `PERDE_CHAVE`**, e **155 dia-colab / 43 colabs de DERIVA PREEXISTENTE (+17.889 min)
que nao sao da cura** (`logs/o191/impacto_join_detalhe_20261005.tsv`). **O deploy move ZERO na ata**,
provado na fonte (`cartorio.py:85-106::impressao_insumos` hasheia so INSUMO; o cron das 06:28 segue
contando `pulados`) e agora tambem na prova depois -- **a relavratura e ATO PROPRIO**, fora deste marco:
comp 10 pre-aprovada pela DINHEIRO-EM-COMPETENCIA-ABERTA, comp 09 como Pauta DP com os dois numeros. A
reversao existe desde antes do apply (condicao 2): `logs/o191/reversao_ata_o191_20261005.jsonl`, 566
celulas lidas de PROD. **O passo 6 da CELULA-TURNO-FECHA NAO esta carimbado**, e a celula turno/marcos da
matriz **nao fechou** com esta cura -- o `+2` da nota do placar e' `+1`, medido. **O PUSH DO MARCO VOLTOU
VERMELHO em 2 selos as 02:21** e a causa era a DIETA do proprio `fdd6f42c`, que levou do RELATO vivo o
contrato de NOME de dois pedidos de patch (secao de 02:5x): curado em DOIS atos pelo estado medido de
cada pedido -- a UI-CAL pousou e a assercao dela **inverteu** para a autoridade de codigo, a fatia 2 segue
ABERTA e o pedido **voltou** ao vivo -- mais o tripwire que faltava desde 02/10,
`bin/tests/test_relato_guarda_pedido_de_patch.sh`. **O PUSH POUSOU as 03:17** -- `024608c7..eef236e1
main -> main`, suite `Ran 9629 / OK (skipped=42)` mais `Ran 22 / OK`, `pre-push: OK -- push liberado`,
e `HEAD == origin/main == eef236e1`. **MARCO FECHADO** (handoff em 44 linhas).

**lei RESPONDIDA as 09:0x, e virou a L-111**: *"sitio com zero chamador de producao AINDA responde
enquanto existir no codigo. Apaga `escala/utils.py::minutos_realizados_do_dia` e os 6 testes de
`escala/tests/test_realizado_intervalo.py`; o pendente de turno/marcos sai junto e a celula fecha pela
funcao real."* Fica abaixo, sem uma virgula mexida, a pergunta COMO foi levada a mesa -- porque e ela que
mostra que a resposta nao inventou nada, so escolheu entre duas saidas que eu havia medido e nomeado. Os
numeros, medidos as 09:0x:
`PENDENTES['turno/marcos']` tem **1** pendente -- Q6, *"o vao entre batidas foi intervalo?"*, impressao
`escala/utils.py:861`, que mora **DENTRO** de `minutos_realizados_do_dia` (`:822`). Chamadores de
producao dessa funcao: **0**, e isso e provado por AST, nao por grep -- o selo
`ponto/tests/test_realizado_do_dia_autoridade.py:139` cobra `chamados.count(...) == 0`, e o censo de
texto no app inteiro devolve so comentario, lapide e teste. O consumidor vivo dela sao **6 testes**
(`escala/tests/test_realizado_intervalo.py`), que fixam os numeros da semantica VELHA (731, 540, 720,
243, 419, 600) -- e **243** e **419** sao justamente cauda sem saida, o dia que pela lei de 03/10 17:2x
passa a levar a PALAVRA em vez de soma. A autoridade cobre as mesmas PERGUNTAS com casos proprios:
intervalo em `test_realizado_do_dia_autoridade.py` (colab 940, 11/09 -- 480/500/540) e cauda /
cross-midnight em `escala/tests/test_realizado_cronologico.py` e `ponto/tests/test_o96_par_da_ata_e_pausa.py`.
Um numero ao lado: o rotulo `zona=TELA` do pendente ficou **FALSO** -- sem chamador de producao ele nao
alcanca tela nenhuma, e a zona e o que diz o quanto ele dói. As duas saidas que eu teria estao fechadas:
tirar o pendente com a impressao ainda no codigo e o **PROIBIDO literal** do aval, e apagar a funcao esta
na sua lista de nao-fazer. Se a resposta for *"ainda responde"*, a celula turno/marcos so fecha apagando
a funcao, e isso e o seu `!`; se for *"nao responde"*, o pendente sai por CARACTERIZACAO e o que estava
errado era a `zona` dele, nao o codigo. **Eu nao escolho** -- e vocabulario, nao implementacao (TRAVA
JUIZ-NOVO, pela mesma razao).
A resposta foi a PRIMEIRA, e com ela o `!` de apagar: a funcao saiu, os 6 testes sairam, o pendente saiu
no MESMO commit e a celula fechou pela funcao real (prova no topo). O rotulo `zona=TELA`, que eu havia
medido FALSO, morreu junto com o pendente -- nao houve correcao de caracterizacao a fazer.

**O PAROU DA RELAVRATURA 10 FOI RESPONDIDO as 09:0x e SE LEVANTA**: *"aplica SO o realizado dos dia-colab
da cura, e a deriva vira fatia propria com o numero dela publicado"*. O apply restrito e o **item (2) da
ordem**, logo depois deste marco, e as condicoes da DINHEIRO-EM-COMPETENCIA-ABERTA valem inteiras (DIFF
publicado antes -- esta a seguir --, reversao em `logs/`, exportada 09 intacta, prova depois). A medicao
que levou o PAROU a mesa fica abaixo sem uma virgula mexida, porque o numero da deriva que ele mandou
separar e EXATAMENTE o que esta nela.

**O que era o PAROU (03:3x): o DIFF de frota SURPREENDE, e eu NAO apliquei.** A condicao 1 da
DINHEIRO-EM-COMPETENCIA-ABERTA (*"DIFF de frota publicado no RELATO ANTES do apply"*) esta cumprida, e
e' ela que me manda parar: pela §7b-2, *"DIFF que surpreende -> NAO aplica, PENDENTES 'PAROU: <motivo>',
e SEGUE outra fatia"*. Medido na SOMBRA contra o **GRAVADO** -- nunca motor-x-motor, que foi a licao
medida da AVAL-DE-CRITERIO em 26/09 (*"apply por recalculo nunca e' cirurgico"*): 2m26s, comp 10/2026,
`logs/o192/relavratura_diff_comp10_20261005_0322.txt`.

`fechamentos_mexidos=36 de 572` · `fechamentos_novos=0` · `processados=568`. Dos 19 campos, **9 dao
ZERO** (noturnas, as cinco de HE, atraso, saida antecipada) e **10 se movem**:

| campo | antes | depois | delta |
|---|---|---|---|
| `minutos_abonados` | 160.724 | 181.584 | **+20.860 min = +347,67 h** |
| `horas_folga_trabalhada` | 394,95 | 361,61 | **-33,34** |
| `horas_trabalhadas` | 30.485,95 | 30.518,34 | **+32,39** |
| `horas_falta` | 179,47 | 211,60 | **+32,13** |
| `minutos_previstos` | 5.615.420 | 5.615.472 | +52 |
| `horas_intra_indenizada` | 1.020,61 | 1.019,96 | -0,65 |
| `saldo_banco_horas` | -19.804,88 | -19.805,83 | -0,95 |
| `dias_incertos` | 11 | 14 | +3 |
| `inconsistencias` | 573 | 575 | +2 |
| `turnos_abertos` | 266 | 265 | -1 |

**POR COLABORADOR o DIFF nao e' "menor que o previsto": ele e' OUTRO.** Cruzei os colabs previstos
para a comp 10 com os que de fato mexeram no gravado, e e' esta conta que vira a mesa:

| | |
|---|---|
| previstos para a comp 10 | **51 colabs / 106 dia-colab / +919,25 h** |
| previstos **que mexeram** | **5** |
| previstos que **NAO mexeram** | **46** -- **+851,83 h previstas que nao chegaram ao gravado** |
| mexeram **sem estar previstos** | **31** -- deriva pura |

E dos 5 que coincidem, **UM** se move na direcao prevista: col165 (previa +28,72 h; folga_trabalhada
33,34->0,00 e trabalhadas 33,30->66,64). col114 previa **+14,50 h e PERDEU 1,01 h**; col206 previa
+8,05 h e ganhou **8,00 h de FALTA**; col250 e col848 ganharam **abono**, nao realizado.

**A ORIGEM, e nao o sintoma: a folha NAO LE o sitio que a cura mexeu.** A cura do passo 5 mora em
`escala/utils.py::montar_grade_prevista_periodo_por_turno` (linhas 1337 e 1362, os ramos de folga e de
trabalho). Censo por AST, nao por grep: os chamadores de producao desse montador sao
`escala/utils.py:1451` (o wrapper `montar_grade_prevista_periodo`), e dele
`ponto/services/cartorio.py:452` e `ponto/management/commands/reconciliar_grade.py:119`. **Nenhum e' a
folha.** O fecho transitivo de `ponto/services/fechamento.py::recalcular_fechamento_mes` tem **571
funcoes** e **nao alcanca nenhuma** das tres -- nem `montar_grade_prevista_periodo_por_turno`, nem
`montar_grade_prevista_periodo`, nem `minutos_realizados_do_dia` --, e nem sequer `realizado_do_dia` /
`realizado_dos_turnos`. Logo: **a relavratura nunca foi o veiculo desta cura.** Os +919,25 h sao numero
de GRADE (o que o espelho e o cartorio passam a ver); o gravado da folha se monta por outro caminho, e o
que o apply levaria para ele e' **deriva** do `FechamentoMensal` velho -- 31 dos 36 sem relacao com a
cura. Isso nao e' o DIFF "surpreendendo": e' o DIFF dizendo que eu apontei a ferramenta errada para o
alvo certo.

**E O ERRO PUBLICADO E' MEU, no topo deste arquivo.** A linha de 01:1x dizia que a leitura de impacto
*"diz o que a relavratura vai levar ao gravado"*. Nao diz: ela mede a GRADE. Corrigi a linha no mesmo
ato em que medi (o topo agora nomeia o que ela mede e que a frase era falsa). A L-110 ja dizia que
aquilo era *"medida de impacto e nunca validacao"* -- eu respeitei a letra e **inferi a unidade**, que e'
a parte que a lei nao podia escrever por mim.

**`processados=568` de `572` nao e' falta, sao DOIS UNIVERSOS impressos lado a lado** -- medido na
funcao real, nao deduzido: `foto()` conta **linhas de `FechamentoMensal`** da competencia (572),
enquanto o laco itera **colaboradores ELEGIVEIS** (`situacao='ativo'` ou desligado com demissao >=
`periodo_apuracao(10,2026,2)[0]` = **02/09/2026**), que sao **568**. A intersecao e' 568, **0 elegiveis
sem fechamento** e exatamente **4 fechamentos de colab nao elegivel** -- desligados antes da janela,
cujo fechamento o recalculo corretamente **nao toca**. Nao ha colab perdido: `recalcular_fechamento erro`
aparece **0 vez** no log. Mas a porta imprime `fechamentos_mexidos=36 de 572` (linhas) ao lado de
`processados=568` (colabs) como se fossem a mesma conta, e isso entra na **O196**.

**O apply move DUAS tabelas, e o `foto()` do comando cobre UMA.** `recalcular_fechamento_mes` tambem
chama `dia_pago.lavrar`, que faz `DiaPago.objects.filter(...).delete()` e recria
(`ponto/services/dia_pago.py:208`), e `colaboradores/services/calendario.py` + `folha/porta_export.py`
LEEM `versao='motor'`. Medi a segunda tabela por SQL nos dois bancos, porque o antes/depois do comando
nao a mostra:

| versao | linhas PROD -> SOMBRA | colabs | `horas_trabalhadas` |
|---|---|---|---|
| `motor` | 10.720 -> **10.759** | 572 -> 572 | 30.493,86 -> **30.518,26** |
| `oraculo` | 7.703 -> **9.221** | 387 -> **463** | 26.426,10 -> **30.210,24** |

O salto do `oraculo` (+1.518 linhas, +76 colabs) e' a metade ADITIVA do S5b que **nenhum leitor le
hoje** (aval 30/09 ~13:5x), e a recusa de lavra-lo em **105** colabs e' PREEXISTENTE e DECLARADA, nao
efeito deste apply: `ponto/services/dia_pago.py:276` levanta `ValueError` para 11 campos de dia **sem
dono**, por ZERO DECLARADO (L-103) -- escreve-los como zero perderia 6,29 + 132,51 h da 09 em silencio
--, e `ponto/services/fechamento.py:714` so registra, com `processados += 1` seguindo adiante.
**Consequencia para a condicao 2**: a reversao que o comando escreve cobre `FechamentoMensal` e **nao**
cobre `DiaPago`. E isso nao e' descoberta minha -- **a casa ja sabia e escreveu**: o cabecalho de
`bin/snapshot_relavratura_10_2026.py` abre com *"DUAS TABELAS, e a segunda nao e detalhe"* e nomeia o
mesmo `dia_pago.py:208`. A cura existe **AO LADO** da porta em vez de DENTRO dela (LEI-AKITA 1), e pela
LEI-AKITA 4 a pergunta certa nao e' "qual a regra" e sim **"qual leitor nao migrou"**: a porta. **O196.**

**comp 09: a Pauta DP NAO sai com o numero da grade, e era isso que eu ia entregar.** A 09 esta
exportada e a L-092 nao cede, entao apply esta fora de questao -- mas o item que eu escrevi as 03:3x
dizia **"+2.379,63 h a MAIS do que o Dominio recebeu"**, e esse numero esta na unidade da GRADE, a mesma
que acabei de provar que **nao chega ao gravado**. As duas pautas irmas da 09 (`PAUTA-DP-09-COL954` e
`COL900`) comparam **TXT com TXT** -- *"213 linhas contra 210"*, rubrica, data --, e e' essa a unidade
que o DP consegue conferir. Entao o item foi **reescrito**: o numero da grade fica declarado COMO numero
de grade (291 dia-colab / 94 colabs / +2.379,63 h de realizado de grade), e o numero do Dominio esta
**POR MEDIR** -- gera-se a 09 na sombra com o codigo no ar e faz-se o **diff de linhas contra o TXT
exportado**, que e' a forma que as irmas ja usam. Essa medicao espera a sombra se refazer (04:17 + o
bloco) e entra como fatia propria.
PROVA: `logs/o192/relavratura_diff_comp10_20261005_0322.txt` (rc=0, 03:22:11->03:24:37), foto por
colaborador em `logs/o192/fotos/recalculo_10-2026_20261005_032436.json`, cruzamento previsto-x-mexido e
fecho de 571 funcoes medidos as 03:4x sobre `logs/o191/impacto_join_detalhe_20261005.tsv`, e os 4
fechamentos nao elegiveis conferidos pela funcao real (`periodo_apuracao`) no banco de prod, so leitura.
**Prod NAO foi tocada**: o DIFF rodou na sombra, no cpuset de teste.

Proximo, pela ordem DELE e nao pela minha: **(2) relavratura 10 restrita** ao realizado dos dia-colab da
cura, com a deriva saindo como fatia propria e com numero; depois **(3) a O204** (`HORA-DO-APARELHO-LIDA-COMO-DATA`,
BO de producao provado, que ele poe a frente dos BOs de tela); depois os **4 BOs de tela** na ordem do bloco
(O197 -> O198 -> O199 -> O200); depois a **O145**. **Instrumento so depois disso**, e isso inclui o pouso do
**CERT-AST**, que fica PARADO onde esta com trilha (`logs/cert-ast.pausado`, HEAD `0bb105db`) -- a ordem das
09:0x o suspendeu com estas palavras: *"o pouso do CERT-AST e os carries O192/O193 PARAM onde estao, com
trilha, e voltam depois"*. A O205 (gate que reverte por arquivo) nasce na mesma fila 2, atras dele.
Os tres registros que respondem *"qual o item em curso"* (marcador
`ORDEM-VIVA-TOPO`, celula do BACKLOG e esta linha) continuam DIZENDO O MESMO -- o item nao fechou, entao o
marcador **nao se move** (`test_hook_nao_cobra_congelado.sh:107` fica VERMELHO se um discordar do outro).

---

# PEDIDOS DE PATCH **ABERTOS** — A DIETA NAO MOVE ISTO

> **A LEI E DA PROPRIA DIETA DE PROSA**, escrita em 02/10 23:5x: *"a dieta move o que ja aconteceu,
> nao o que ainda tem de acontecer"*. Pedido de patch ABERTO e' **contrato entre duas metades de uma
> fatia**, nao historia: a metade de TELA ja esta na arvore, atras de um gate, e a metade de NUCLEO
> le **aqui** o nome das chaves de contexto. Nome diferente nao reprova -- so **CALA** (o `{% if %}`
> nao abre e a tela sai byte a byte igual), e e' por isso que o nome e' cobrado por selo.
>
> **ESTA SECAO JA FOI QUEBRADA DUAS VEZES PELO MESMO ATO**, as duas descobertas pela suite inteira:
> 02/10 (push 94, levou `competencia_rotulo`) e **05/10 `fdd6f42c`, que e' meu** -- a dieta do marco
> O191 moveu 5.548 linhas do vivo e levou o pedido INTEIRO da fatia 2 junto, e o push do marco voltou
> VERMELHO em 2 selos **depois de 619 s de suite**. Na primeira vez a cura foi restaurar a palavra, e
> so isso: **nenhuma guarda nasceu**, entao a classe sobreviveu para reaparecer tres dias depois.
> Agora existe o tripwire: **`bin/tests/test_relato_guarda_pedido_de_patch.sh`**, que le por AST dos
> PROPRIOS selos quais nomes o vivo tem de carregar (nunca uma lista digitada: a lista repetida foi o
> buraco dos LABELS em quatro lugares) e fica VERMELHO **em menos de 1 s**, na regua, antes da suite.
>
> **Ao arquivar pela dieta: esta secao fica.** Quando a metade de nucleo pousar, o pedido vira
> historia e vai para o `RELATO-ARQUIVO.md` **no mesmo commit** que religa os selos pulados -- e a
> assercao do selo **inverte** para a autoridade de codigo, como a da UI-CAL-COMPETENCIA inverteu
> hoje (ela passou a cobrar `competencia_rotulo` em `colaboradores/services/calendario.py`, porque as
> duas metades dela pousaram: `:225` o modo, `:857` o rotulo). Selo nao se apaga; a pergunta troca.
>
> Copia integral da historia deste pedido, com a conversa em volta: `app/docs/RELATO-ARQUIVO.md`
> (a partir de `# PEDIDO DE PATCH DA RAIA UI -> MAIN`, linha ~23.954). **Mover, nunca apagar.**

## 05/10 02:5x — O PUSH DO MARCO VOLTOU **VERMELHO EM 2 SELOS**, E A CAUSA FOI A MINHA PROPRIA DIETA

**O FATO, com os numeros.** O push do marco O191 rodou a suite inteira -- `Ran 9629 tests in 619.614s`
-- e voltou `FAILED (failures=2, skipped=42)`, com `error: failed to push some refs`. Os dois:
`colaboradores.tests.test_ui_cal_competencia::test_MORDE_o_gate_e_a_MESMA_palavra_nos_QUATRO_sitios` e
`ponto.tests.test_tela_gestao_he_fatia2_lote_limite::test_MORDE_a_fatia_2_segue_REGISTRADA_com_o_pedido_de_patch`.
Os dois cobram do **RELATO VIVO** o contrato de NOME de um pedido de patch, e os dois ficaram vermelhos
pelo MESMO ato: o `fdd6f42c` (o commit da cura) levou **5.548 linhas** do vivo para o
`RELATO-ARQUIVO.md` pela DIETA DE PROSA, **a mao**, e junto foram `competencia_rotulo`,
`url_autorizar_marcados`, `abaixo_limite`, `limite_decisao_rotulo` e `autorizar_he_marcados`. Os selos
acertaram; quem errou fui eu.

**E A CASA JA PAGOU ISSO UMA VEZ, TRES DIAS ATRAS.** `RELATO-ARQUIVO.md:21978` registra o push 94 de
02/10 falhando no MESMO selo, pelo MESMO motivo, com a lei ja escrita na mesma linha: *"pedido de patch
ABERTO nao e historia: a dieta move o que ja aconteceu, nao o que ainda tem de acontecer"*. A cura
daquele dia foi **restaurar a palavra, e so isso** -- `grep -c` = 4 e segue. Nenhuma guarda nasceu, e
por isso a classe estava viva esperando a dieta seguinte, que fui eu. **MEIA-CORRECAO E PIOR QUE
NENHUMA**: restaurar cura o CASO; o que cura a CLASSE e um leitor que pergunte, antes da suite, se o
vivo ainda carrega o que os selos cobram dele. **O tripwire e o entregavel desta linha; a restauracao e
a metade facil.**

### DOIS PEDIDOS, DOIS ESTADOS DIFERENTES -- e por isso DUAS curas, nao uma

A pergunta que decide a cura nao e "como devolvo a palavra", e **"este pedido ainda esta ABERTO?"**.
Medido, nao suposto:

| pedido | metade de NUCLEO | veredito | cura |
|---|---|---|---|
| **UI-CAL-COMPETENCIA** | `colaboradores/services/calendario.py:225` (`modo == 'competencia'` lendo `janela_fechamento`) **e** `:857` (`'competencia_rotulo'` no dicionario INCONDICIONAL) | **POUSOU** -- o pedido FECHOU | a assercao **INVERTE**: passa a cobrar o nome na AUTORIDADE de codigo, nao no diario |
| **GESTAO-HE-FATIA-2-LOTE-E-LIMITE** | `grep -rn autorizar_he_marcados --include=*.py` fora de `/tests/` = **0 sitio** | **ABERTO** | o pedido **VOLTA** ao vivo, na secao acima, com a lei no cabecalho |

A inversao e a lei da casa, nao invencao de agora (`caracterizacao-de-defeito-curado-se-inverte`):
**selo nao se apaga, a pergunta troca**. E a pergunta nova e' mais FORTE que a velha -- "o diario cita a
palavra" virou "a view que monta o contexto manda esta chave" --, porque ancorar contrato num arquivo
que a propria lei esvazia a cada 3 dias era fraqueza estrutural: o RELATO tem **teto de 3 dias**, e a
palavra ja foi levada DUAS vezes. A prosa do cabecalho daquele selo, que mandava a outra metade ler o
RELATO, envelheceu **no mesmo ato** e foi corrigida no mesmo commit (ela diria uma mentira a partir de
hoje).

### O TRIPWIRE: `bin/tests/test_relato_guarda_pedido_de_patch.sh`

Ele le **por AST dos PROPRIOS selos** quais nomes o vivo tem de carregar -- nunca uma lista digitada
dentro dele, que foi o buraco dos LABELS em quatro lugares. Universo: todo `app/*/tests/*.py` que le
`docs/RELATO.md`; tokens: o primeiro argumento de cada `assertIn` **cujo palheiro e o texto do RELATO**,
resolvendo o `for nome in (VAR, '...')` que ENVOLVE a assercao e as constantes de modulo. Reproduz em
**menos de 1 s** o vermelho que custou 619 s de suite.

**TRES DEFEITOS MEUS, achados construindo ele, e os tres sao da mesma familia -- sinal fraco lido como
sinal bom:**
1. **ele passou VERDE apontando para NADA.** Rodado de fora de `bin/tests/`, o `BASH_SOURCE/../..` caiu
   no scratchpad, o glob achou 0 arquivo e ele imprimiu `ZERO DECLARADO ... OK`. Universo vazio POR LEI
   e universo vazio POR CAMINHO ERRADO nao podem ter a mesma cor: agora a **raiz se prova** antes de
   qualquer afirmacao (`RED raiz errada`, rc=3).
2. **ele cobrava um token que a fatia 2 PROIBE.** O extrator rendeu 6 nomes, e o 6o era `he-barra` --
   uma classe de CSS que mora num `assertNotIn` **contra o markup**, no mesmo arquivo, num `for` com o
   alvo tambem chamado `nome`. CRITERIO PELA FORMA CONTA ERRADO: o token se liga ao **PALHEIRO**, nunca
   a funcao. Com o palheiro amarrado: **5 tokens, exatamente o censo que eu tinha feito a mao**.
3. **ele ficou CEGO para o arquivo da UI-CAL**, que escreve a leitura inline
   (`assertIn(VAR, io.open(relato).read())`) em vez de guardar o texto numa variavel. Cegueira aqui
   seria o guarda cobrando MENOS do que a suite -- entao o palheiro inline conta, e **arquivo que le o
   RELATO e nao rende token nenhum fica VERMELHO** (`RED extrator cego`), que e a anti-vacuidade do
   proprio extrator.

O caso que MORDE: o mesmo checador roda contra o vivo com **um token apagado** e tem de acusar. Dois
valores, dois resultados -- sem ele o selo passaria com um `exit 0` cravado.

**O LIMITE, dito antes que alguem descubra por acidente:** `bin/tests/` **nao roda no pre-push**
(`bin/pre-push.sh:15` diz isso em letra), so na **regua**. Entao o ganho e' "a regua acusa em 1 s" --
no pre-push a rede continua sendo a suite, lenta. Mover `bin/tests/` para dentro do pre-push e' decisao
de esteira, nao desta cura, e fica REGISTRADA, nao feita. Pela mesma linha fica registrado o que eu
**nao** construi: a dieta ainda e' feita **a mao**, e o script que a faria (`relato_dieta.sh`,
recortando por titulo e PULANDO a secao de pedidos abertos) e' item, nao fatia de agora -- o bug do
caminho era o tripwire que faltava, nao a ferramenta que falta.

**VERDE:** `bin/suite.sh --only` nos tres modulos (os dois curados + `test_tela_gestao_he_calendario`,
o outro lado do interruptor da fatia 2) = `Ran 30 tests / OK (skipped=7)`. **62 selos de host,
vermelhos=0** (o novo incluido). `tickets_placar: OK`, `regua_tickets: OK`. Os 5 nomes conferidos no
vivo um por um.


# PEDIDO DE PATCH DA RAIA UI -> MAIN: **GESTAO-HE-FATIA-2-LOTE-E-LIMITE** (01/10 20:1x-21:xx, `wt-ui`)

> **RECORTE DE 01/10 20:5x, e ele muda o que esta escrito abaixo.** A obra virou DUAS fatias. A **FATIA 1** --
> celula com o numero, dia de HE em FUNDO CHEIO, tres estados por led+risco+fundo, **clique no dia disparando
> a porta POR DIA que JA EXISTE** (`ponto:decidir_he`, com o motivo pelo `hxPerguntar` da casa) e tabela de
> apoio sem botao repetido -- e **SO TEMPLATE**, esta commitada na raia e **nao precisa de nada desta secao**:
> ela usa porta que ja esta no ar, entao **nao ha janela de perigo** e merge/deploy nao esperam patch de `.py`.
> O que sobrou nesta secao e a **FATIA 2**: a barra unica de lote e o limite de decisao por cadastro. Os selos
> dela nao foram apagados -- estao em `app/ponto/tests/test_tela_gestao_he_fatia2_lote_limite.py`, PULADOS com
> o motivo escrito, porque selo verde afirmando sobre markup ausente e pior que selo nenhum.
>
> **A LEI DO LIMITE ESTA RESPONDIDA** (ordem 20:5x): **opcao (b)** -- o limite filtra o **contador E o ato**.
> Entao o patch 3 abaixo muda num ponto: `ponto/views.py:558` (`recusar_he_em_lote`, o `brutos` do
> `todos_sem_decisao=1`) tambem passa a ignorar o dia abaixo do limite, e o botao de ciencia deixa de
> prometer 0 e gravar 12. A pergunta que eu levantei com o numero -- 12 dias de 9 min do colab do habito --
> era exatamente esta, e ela fecha aqui.
>
> **A ORDEM DO MERGE desta secao (patch 1 -> `deploy.sh` -> merge) vale para a FATIA 2, nao para a fatia 1.**
> A nota em caixa alta mais abaixo foi escrita quando as duas eram uma so obra.

A metade de TELA esta construida e COMMITADA na `raia-ui`. **Ela nao sobe sozinha**: o calendario marca, mas a
**barra nasce DESLIGADA** e o **limite de decisao nao apaga celula nenhuma** enquanto os tres patches abaixo
nao pousarem na main. Os tres sao `.py`/migration, que a raia 2 nao toca (FILA-2-EM-RAIA-PROPRIA).

**LEI-AKITA: origem=`ponto/views.py` + `colaboradores/models.py` + `ponto/services/gestao_he.py`,
testemunha=`DecisaoHE` pela porta `ponto/portas/he.py::decidir_he` (nenhum leitor novo),
RED=`ponto/tests/test_tela_gestao_he_calendario.py` (15 casos, 2 que MORDEM por renderizar o recorte do
proprio template nos DOIS cenarios), quem-mais-le=`gestao_he.html` + `gestao_he_pdf` (o papel le a MESMA `enriquecer`), juizes novos=0.**

## O QUE A TELA JA FAZ SEM PATCH NENHUM, e por que isso e seguro

Com o contexto de hoje o HTML servido muda de FORMA (celula com `▲19`/`▼5`, tres estados por led + risco +
fundo, dia clicavel que marca, tabela recolhida) e **nao muda de PODER**: a barra nao tem `method` nem
`action`, o `<button>` dela nasce `disabled` e o motivo esta em texto visivel ao lado -- nunca num `title`,
que e o que a LEI-UI **L4** proibe (`app/docs/LEIS-UI.md:39`). `{% url %}` de rota inexistente **nao entra**:
`NoReverseMatch` nao desabilita um botao, derruba a tela -- foi o apagao de 23/09 (cinco 500 em
`/colaboradores/<id>/calendario/`).

> **ATENCAO NA ORDEM DO MERGE, e isto e medido, nao suposto.** Template e bind-mount: ele muda a tela NA HORA
> (secao 2 do CLAUDE.md). O calendario novo **nao tem botao Autorizar por linha** -- o desenho aprovado pede
> *"tabela recolhida e sem botao por linha"* --, e a barra que o substitui so liga com o patch 1. Entao, entre
> o merge e o `deploy.sh` do patch 1, **a tela nao tem caminho de AUTORIZAR** (a CIENCIA em lote continua
> inteira: ela e `recusar_he_em_lote`, que ja existe). Merge e deploy sao **UM ato** (MERGE DE RAIA CAI E
> RECARREGA NO MESMO ATO, corte 30/09 17:4x): aplicar o patch 1, `bin/deploy.sh`, e so entao mergear a tela --
> ou mergear e deployar sem nada no meio. Nunca a tela primeiro.

## PATCH 1 -- a view de laco e o contrato de NOME `url_autorizar_marcados`

**L-081 nao cai, e nao nasce porta de lote** (decisao de 01/10 20:1x, item 3): um clique na barra = **N ATOS,
um por dia marcado**, cada um pela porta `decidir_he`, com a propria trilha, a propria idempotencia e a
propria relavratura. `ponto/portas/he.py:175::autorizar_em_lote` **continua levantando** -- e a funcao que
existe para explicar por que nao existe --, e o laco mora na VIEW, exatamente como
`ponto/views.py:512::recusar_he_em_lote` ja faz com `recusar_em_lote`.

**(a) `app/ponto/views.py`** -- view nova, logo depois de `decidir_he` (que termina em `:508`):

```python
@login_required
def autorizar_he_marcados(request):
    """A BARRA UNICA DO CALENDARIO: N dias marcados, UM motivo, N ATOS (desenho Ronald 01/10 18:3x). POST.

    NAO E LOTE E NAO E PORTA NOVA: cada `item` vira UMA chamada de `ponto/portas/he.py::decidir_he`, com a
    propria trilha e a propria idempotencia -- o mesmo desenho de `recusar_he_em_lote`, e pela mesma razao
    escrita la (um "modo lote" dentro da porta criaria um caminho de escrita que nenhum selo de idempotencia
    cobre). O que a barra economiza e o GESTO do admin, nunca o ato.

    O MOTIVO E UM E VALE PARA OS N DIAS: a tela mostrou os N dias e a conta antes do clique, e a porta grava
    o mesmo porque em cada um. Motivo por dia eram treze campos abertos -- a poluicao de 01/10 11:1x.

    OS MINUTOS VEM DA TELA, por dia, como na `decidir_he`: a porta nao os recalcula (a lapide dela conta por
    que). `item` = `colab:data:minutos`, o MESMO formato que `recusar_he_em_lote` ja le -- nao nasce
    vocabulario de transporte novo.
    """
    import datetime as _dt

    from django.urls import reverse

    from core.respostas import resposta_acao
    from ponto.portas.he import DecisaoHERecusada, decidir_he as _porta
    if request.method != 'POST':
        from django.http import Http404
        raise Http404
    _volta = request.POST.get('voltar') or reverse('ponto:gestao_he')
    _motivo = (request.POST.get('motivo') or '').strip()
    brutos = request.POST.getlist('item')
    if not brutos:
        return resposta_acao(request, ok=False, redirect_url=_volta,
                             msg='Nenhum dia marcado. Clique nos dias do calendario e confirme de novo.')
    _ids = set()
    for b in brutos:
        _p = (b or '').split(':')
        if len(_p) == 3 and _p[0].isdigit():
            _ids.add(int(_p[0]))
    _colabs = {c.pk: c for c in Colaborador.objects.filter(pk__in=_ids)}
    ok, no_op, erros, malformados = 0, 0, [], 0
    for b in brutos:
        _p = (b or '').split(':')
        if len(_p) != 3:
            malformados += 1
            continue
        try:
            _c, _d, _m = _colabs[int(_p[0])], _dt.date.fromisoformat(_p[1]), int(_p[2])
        except (KeyError, ValueError, TypeError):
            malformados += 1
            continue
        try:
            r = _porta(_c, _d, 'autorizado', minutos=_m, autor=request.user,
                       motivo=_motivo, request=request)
        except DecisaoHERecusada as e:
            # CADA DIA E UM ATO INDEPENDENTE: um dia que a porta recusa nao desfaz os outros, e volta
            # NOMEADO -- nunca contado em silencio.
            erros.append((getattr(_c, 'pk', _c), _d, str(e)))
            continue
        no_op += 1 if r['no_op'] else 0
        ok += 0 if r['no_op'] else 1
    _partes = ['%d dia(s) autorizados, com trilha por dia. O fechamento deles foi posto na fila de '
               'recalculo.' % ok]
    if no_op:
        _partes.append('%d ja estavam autorizados (nada mudou).' % no_op)
    if erros:
        _partes.append('%d recusados pela porta: %s' % (
            len(erros), '; '.join('col%s %s -- %s' % x for x in erros[:5])))
    if malformados:
        _partes.append('%d item(ns) ilegiveis foram ignorados.' % malformados)
    return resposta_acao(request, ok=not (erros or malformados), msg=' '.join(_partes),
                         redirect_url=_volta)
```

**(b) `app/ponto/urls.py:19`** -- depois da linha do `decidir_he`:

```python
    # A BARRA UNICA DO CALENDARIO: N dias marcados num gesto, N ATOS na porta `decidir_he` (18:3x).
    # NAO e porta de lote -- `autorizar_em_lote` segue levantando, e o laco mora na view (L-081).
    path('gestao-he/autorizar-marcados/', views.autorizar_he_marcados, name='autorizar_he_marcados'),
```

**(c) `app/ponto/views.py:768`** -- no dict do `render` de `gestao_he`, ao lado de `'pode_autorizar'`:

```python
        # O CONTRATO DE NOME com a metade de TELA: sem esta chave o `{% if %}` nao abre, a barra nasce sem
        # `method`/`action` e diz por que. Mandar OUTRO nome nao reprova -- so cala (a barra nunca liga).
        # Selo: `ponto/tests/test_tela_gestao_he_calendario.py` (constante `VAR`).
        'url_autorizar_marcados': reverse('ponto:autorizar_he_marcados') if tem_acao(
            request.user, 'autorizar_he') else '',
```
(a view ja tem `from django.urls import reverse`? **nao**: `gestao_he` nao importa `reverse` -- incluir
`from django.urls import reverse` no corpo dela, como as irmas fazem.)

**O selo da metade de tela que fica VERMELHO se o nome divergir** ja esta escrito:
`test_MORDE_o_contrato_de_NOME_e_a_MESMA_palavra_nos_QUATRO_sitios` e
`test_MORDE_o_contrato_de_NOME_esta_escrito_no_RELATO`.

**RED a escrever no MESMO commit do patch** (o que a tela nao pode provar): marcar 3 dias + 1 confirmacao =
**3 `DecisaoHE` autorizadas e 3 linhas de `LogAuditoria` com `acao='decidir_he'`**, e a segunda confirmacao
com os mesmos 3 itens = **0 trilha nova** (idempotencia herdada da porta, contrato 2 de 13/09). Mais o caso
que MORDE: um item com `minutos=0` volta em `erros` e **nao derruba** os outros dois.

## PATCH 2 -- os dois selos que a L-081 trava e que o desenho de 18:3x SUPERA

Estes dois ficam VERMELHOS no instante em que a barra ligar, e **isso e correto**: eles guardam a redacao
antiga da L-081 no nivel de TELA, e o desenho aprovado em 01/10 18:3x a alterou (a porta segue um dia por
ato; o que passou a poder ser multiplo e o GESTO). Quem aplicar o patch 1 ajusta os dois no mesmo commit:

* `app/ponto/tests/test_tela_gestao_he.py:122::test_MORDE_AUTORIZAR_e_um_dia_por_ato_e_RECUSAR_pode_ser_em_LOTE`
  -- ele proibe controle de lote dentro de form que autoriza. Hoje ele passa **por vacuidade** (nao ha mais
  form com `decidir` na tag de abertura), e isso ja esta registrado aqui para nao passar por cura. A pergunta
  dele muda de *"ha controle de lote num form que autoriza?"* para *"o form que autoriza manda UM `item` por
  DIA, e nao uma lista de dias num campo?"* -- o que prende a lei que sobrou: **N atos, nunca 1 ato com
  lista**.
* `app/ponto/tests/test_tela_gestao_he.py:384::test_MORDE_AUTORIZAR_em_lote_NAO_EXISTE` -- **NAO muda**.
  `ponto/portas/he.py:175::autorizar_em_lote` continua levantando, porque **nao nasce porta de lote**. Ele e o
  selo que prova que o laco ficou na view.

## PATCH 3 -- o LIMITE DE DECISAO e cadastro da empresa (default 10 min, SO DE TELA)

Dois contratos de NOME, e os dois estao cravados na metade de tela: a chave por dia **`abaixo_limite`** e o
rotulo pronto **`limite_decisao_rotulo`**. O template **nao compara minuto com nada** -- sem a chave, nenhuma
celula aparece apagada, e sem o rotulo a legenda do limite nem se escreve (explicar um estado que a tela nao
produz e a testemunha afirmando sobre o que nao mediu). Selo:
`test_MORDE_o_template_NAO_compara_minuto_com_LIMITE_nenhum`.

**(a) `app/colaboradores/models.py:46`** -- ao lado de `dia_inicio_competencia`, que e o vizinho certo (as
duas sao regra que varia por EMPRESA, LEI-AKITA 12):

```python
    limite_decisao_he_min = models.PositiveSmallIntegerField(
        default=10,
        help_text='SO DE TELA: dia com menos minutos de HE fora do marco que isto nao e CHAMADO a decisao na '
                  'Gestao de HE (aparece apagado, sem led). NAO muda dinheiro e NAO muda a L-097 -- o minuto '
                  'segue bloqueado do mesmo jeito; o que o limite decide e se o admin e chamado a olhar.')
```

**(b) migration** `app/colaboradores/migrations/0056_limite_decisao_he_min.py` (`AddField`, default 10, sem
`RunPython`). **Ela segura o deploy** (migration pendente trava o `deploy.sh`, DEPLOY JA), entao sobe junto.

**(c) `app/colaboradores/admin.py:6`** -- o cadastro E a UI do django admin, como e para o
`dia_inicio_competencia`; o campo ganha LEITOR visivel em `list_display`:

```python
    list_display = ["nome", "cnpj", "ativa", "dia_inicio_competencia", "limite_decisao_he_min"]
```

**(d) `app/ponto/services/gestao_he.py:121`** -- em `_tira`, no dict de `por_data`, UMA chave nova. O leitor
COMPARA (e e o unico que compara), e o limite chega por argumento -- nunca por `getattr(empresa, ...)` dentro
da funcao, que seria o juiz lido duas vezes:

```python
            'abaixo_limite': bool(limite) and _m <= limite,
```
`_tira(dias, ini, fim, limite=0)` ganha o parametro, `enriquecer(..., limite=0)` o repassa nas TRES chamadas
(`:271`, `:296` e `:306`), e `ponto/views.py` o le do cadastro: `getattr(empresa, 'limite_decisao_he_min', 0)`.

**(e) o CONTADOR muda de universo, e e aqui que mora o observavel dele** (*"col616 comp 09 com o 'sem
decisao' caindo de 13 para os acima do limite, numero publicado"*). Em
`app/ponto/services/gestao_he.py:246` e `:302`, `sem_decisao` passa a contar **so o que PEDE decisao**:

```python
        linha['sem_decisao'] = sum(1 for d in linha['dias']
                                   if d['estado'] == 'sem_decisao' and not d.get('abaixo_limite'))
```
**CUIDADO, e isto e L1 (CONTADOR == UNIVERSO):** o mesmo `sem_decisao` alimenta (i) o chip da linha, (ii) o
total do topo, (iii) o botao *"Dar ciencia em tudo que esta sem decisao -- N dia(s)"* e (iv) o universo que
`ponto/views.py:538::recusar_he_em_lote` relê com `todos_sem_decisao=1`. Se o contador encolher e o ATO nao,
o botao promete 4 e faz 13 -- **o numero e a lista tem de sair do mesmo lugar**, e saem: os quatro leem a
MESMA `enriquecer`. O que o patch precisa decidir **por escrito** e se a ciencia em lote tambem passa a
ignorar os dias abaixo do limite (o `brutos` de `:558` filtra por `d.get('estado') == 'sem_decisao'`, hoje
sobre a lista COMPLETA). **Pergunta de LEI no topo deste RELATO, nao decidida aqui.**

**(f) `app/ponto/views.py:768`** -- o ROTULO vai PRONTO para a tela (L7/L2: o texto sai de onde a decisao
mora; o NUMERO nunca entra no template):

```python
        'limite_decisao_rotulo': ('abaixo do limite de decisao (%d min ou menos, cadastro da empresa)'
                                  % _lim) if _lim else '',
```
com `_lim = getattr(empresa, 'limite_decisao_he_min', 0) if empresa is not None else 0`.

**RED do patch 3, no mesmo commit:** empresa com limite **7** -> dia de 7 min tem `abaixo_limite=True` e dia
de 8 min **nao** (o caso que MORDE: com o default 10 cravado em qualquer sitio, 8 cairia abaixo e o selo
passaria por coincidencia de numero -- e o gemeo exato do `21` cravado que `ponto/janelas.py` nasceu para
matar); e empresa com limite **0/None** -> **nenhum** dia abaixo, que e o estado de hoje.

---

**L-110 `REFERENCIA-E-A-LEI` NASCEU HOJE, E REVOGOU UMA LEI DE MINUTOS ANTES DELA** (secao de 00:3x): a
referencia de um numero e a **REGRA** aplicada ao cadastro e as batidas; o codigo de hoje **nao e gabarito**
do codigo novo. `FROTA-PROVA-AMOSTRA-EXPLICA` foi desfeita nos quatro sitios no mesmo marco.

**AS DUAS LEIS DE ESTEIRA DAS 19:2x JA VALEM NESTE POUSO** (`RAIA-VERDE-POUSA` = **L-105**,
`DOCS-NO-MARCO` = **L-106**; `MERGE DE RAIA` = **L-107**, `MARCO FECHADO / PUSH UM POR MARCO` = **L-108**,
`DIETA DE PROSA` = **L-109**). O pacote que estava em voo **se separou no mesmo turno**: K8 e K5 pousam
como PRODUTO, o CERT-AST vai a pouso proprio. Por **L-105c**, em uma linha: *`cert-ast` (`cb84a4ac`) nao
pousa aqui porque e INSTRUMENTO (`bin/suite_nucleo.sh`, selo de host, paragrafo da CLAUDE.md 3) -- pouso
proprio, logo atras, levando `8ffcd44d`.*

**MARCO ANTERIOR, NO AR E NO REMOTO -- `5d31530f..024608c7`.** O pacote de PRODUTO (K8 + K5) pousou com o deploy num
ato so (L-107) e o range inteiro chegou ao remoto na **terceira** tentativa.
PROVA: `HEAD` = `024608c7` == `origin/main`, **0 commit a frente**; `pre-push: OK -- push liberado`; a suite
do commit empurrado deu **9.625 testes OK (skipped=42)** e o control-plane **22 OK**.
PROVA: o sha DEPLOYADO (`8c3035bc`) nao e a ponta (`024608c7`), e isso esta certo --
`git diff --stat 8c3035bc..024608c7 -- '*.py'` devolve **saida vazia**; no range sobraram so
`app/docs/LEIS.md` e `app/docs/TICKETS.md` (4 linhas). **Nenhum `.py` se moveu depois do deploy**, entao a
prova de casca e o smoke do K8 seguem valendo para o codigo que esta no ar.
AS TRES TENTATIVAS, a causa de cada uma em UMA linha:
1. `tickets_placar: ALARME` -- o topo derivado do TICKETS nasceu dentro da copia `wt-pousos`, onde nao havia
   carimbo de regua, e saiu sem o prefixo `regua OK (04/10 17:26)`. Curado pelo escritor CANONICO
   (`bin/pos_push.sh` -> `77eafa5f`), nunca a mao.
2. `test_lei_protege_sitio: RED` em **3 leis, todas minhas** -- as celulas PROTEGE da L-104 e da L-108
   carregavam PROSA, e a coluna cobra `arquivo.py::funcao` (gramatica em
   `bin/tests/test_lei_protege_sitio.sh:83`). A L-102 ja estava conforme: **o leitor que nao havia migrado
   era eu**. Curado em `024608c7`; a celula da L-108 fica VAZIA de proposito, com o motivo na coluna do
   leitor, porque o alvo dela e um `.md` e a coluna so aceita `.py`.
3. verde.
CONDUTA NOVA, e ela e barata: **a pasta `bin/tests/` INTEIRA roda na arvore viva ANTES do push** -- 61
selos, menos de 1 min, contra ~10 min de suite por rejeicao. As duas rejeicoes de hoje foram selo de HOST;
**nenhuma foi a suite**. (Memoria `pasta-de-selos-antes-do-push`.)
**A L-106 teve o primeiro caso MEDIDO no mesmo dia em que nasceu, e e meu**: a correcao do item (2) e um
commit que toca SO `app/docs/`. Nao e excecao e nao abre allowlist -- ela caiu DENTRO do range aberto do
push, e a **L-108 define o marco pelo PUSH**, nao pelo commit. O texto da L-106 nao se tocou.

**O191 PASSO 5 -- A CONDICAO DE ENTRADA ESTA MEDIDA, e ela LIBERA o passo.** Ele pediu que a O191 comecasse
provando que o dia sem par e REAL e nao defeito do juiz de geometria. Medido na SOMBRA, 6 dia-colab
(`logs/o134/o191_condicao_entrada_20261004.out`):

| colab | dia | celula casada | previsto | turnos com esse `data_turno` | `sem_turno` | ABERTO | soma propria |
|---|---|---|---|---|---|---|---|
| col200 | 2026-08-25 | sim | 420 | **0** | True | False | 60 |
| col235 | 2026-09-30 | sim | 420 | **0** | True | False | 61 |
| col250 | 2026-09-29 | sim | 660 | **0** | True | False | 240 |
| col277 | 2026-08-31 | sim | 660 | **0** | True | False | 280 |
| col302 | 2026-09-09 | sim | 660 | **0** | True | False | 300 |
| col852 | 2026-08-21 | sim | 660 | **0** | True | False | 305 |

Os seis tem celula casada com **4 marcos congelados** e previsto (420 ou 660 min), `eh_dia_trabalho=True`, e
**ZERO turno** com aquele `data_turno`. As batidas EXISTEM -- sao 11 no col200 -- e pertencem aos turnos
**VIZINHOS**: no noturno a entrada e 22:00 de d-1 e a saida 06:00 de d, entao o par cai em
`data_turno=24/08` e o dia 25/08 nao tem turno proprio. Contradicao do juiz (turno na janela E
`sem_turno=True`): **0 de 6**. Dia que a propria autoridade marca ABERTO: **0 de 6**. Veredito:
`ponto/turnos.py::realizado_do_dia` **nao erra** -- ele devolve `minutos=None sem_turno=True`, que e "nao
sei", e e a resposta certa. O passo 5 segue.
A SONDA ANTERIOR NAO SE PUBLICA, e o erro era meu: ela passava **DATAS** a `batidas_apuraveis`, cuja janela
e de **INSTANTES** (`ponto/turnos.py:1865`), e o censo truncava na meia-noite do ultimo dia com
`RuntimeWarning: naive datetime`. Corrigida em `logs/sombra/o191_condicao_entrada_20261004.py:68-71`, a
janela impressa agora le `24/08 00:00:00 -> 26/08 23:59:59`.

**ITEM (b): O `or 0` QUE EU IA CURAR ESTA MORTO -- a origem e outra linha.** O aval nomeava
`ponto/supra_juiz.py:131` (`real = dia.get('minutos_realizados') or 0`) como o ZERO a declarar. Censo dos
**tres** chamadores reais (`ponto/services/cartorio.py:585`, `ponto/management/commands/supra_juiz.py:190`,
`ponto/management/commands/emitir_furo_retroativo.py:82` -- o `classificar_dia` de `relatorios/furos.py` e
outra funcao com o mesmo nome e nao conta): **nenhum entrega `None` ali**, porque os dois produtores ja
colapsaram antes --
- `escala/services/leitor_celula.py:364` faz `int(ata.get('minutos_realizados') or 0)`, e o `base` da
  linha 337 ja nasce com `'minutos_realizados': 0`;
- `escala/utils.py:1334`, quando a autoridade devolve `sem_turno`, **nao colapsa para zero: ela FABRICA um
  numero** pelas celulas (`minutos_realizados_do_dia({'celulas': cel_por_data[d]}, marcos_out)`). Sao
  exatamente os `60, 61, 240, 280, 300, 305` da ultima coluna da tabela acima.
O comentario da propria linha confessa: *"ali o numero nao e da autoridade, o dia leva
`realizado_sem_turno`"*. **E essa e a SOMA PROPRIA que o passo 5 manda sair** -- S133 e LEI-AKITA 2, a
testemunha recalculando em vez de ler. Curar a 131 seria band-aid sobre um colapso que ja aconteceu
(LEI-AKITA 1): a origem e a **1334**, e a 131 so passa a precisar distinguir `None` DEPOIS que ela sair.
MEDIR ANTES DE TOCAR, e o motivo tem numero: esse `minutos_realizados` desce para `folha/export.py:698` e
`ponto/services/dia_pago.py:341`, os dois com `min(..., previsto)` -- **tirar a soma propria MOVE
DINHEIRO**. Entra por DINHEIRO-EM-COMPETENCIA-ABERTA: DIFF de frota publicado ANTES, reversao em `logs/`,
competencia exportada intacta com hash, PROVA depois. O precedente diz para esperar numero longe de zero:
o `e6_oraculo` curou o MESMO `or 0` no leitor dele em 02/10 e mediu que, dos 247 dia-colab de
`esp_zero_e6_trabalho` da competencia 09, **so 67 eram zero de verdade** -- 170 (1.681,0 h) tinham valor
positivo no `DiaPago`.

**LEI RESPONDIDA as ~14:xx -- (a), E ELA ALTERA A PROPRIA L-102.** O que segue e a pergunta como ela foi para a mesa, com os numeros que a decidiram; a resposta esta logo depois dela. (Nasceu sob PAREI-DE-LEI-NAO-DEVOLVE-TURNO; o adendo dele de 12:xx nomeia este
caso: *"Se a ata precisar guardar o numero por outro caminho, isso e pergunta de LEI: topo do RELATO com o
numero, e segue o proximo item"*). **DE QUAL AUTORIDADE SAI O NUMERO QUE A L-102 MANDA ROTULAR** --
`Sem turno pareado (ata Xh)`, *"nem se grava 0, nem se cala"* -- quando a soma propria de
`escala/utils.py` sair? A autoridade do pareamento **declina o numero**: `ponto/turnos.py:398` devolve
`RealizadoDoDia(minutos=None, sem_turno=True, ...)` quando nenhum turno tem `data_turno` naquele dia, e
nao ha campo nela que carregue "quanto havia". OS NUMEROS: **88 dia-colab** sem par na 09+10 (emp2+3+4),
**15 deles com numero hoje** (45,2 h) e **73 com zero**; dos 15, **2 sao a classe A** -- o numero e o turno
do dia VIZINHO -- e **13 a classe B**, onde o numero e o intervalo entre batidas orfas DAQUELE dia. Os tres
caminhos que eu enxergo estao **todos barrados por lei existente**, e por isso nao escolho: (a) a palavra
fica SEM numero -- contraria a L-102 literal, e as horas so restam visiveis nas BATIDAS da linha (Portaria
671); (b) nasce autoridade para *"quanto tempo ha entre as batidas deste dia, sem julgar turno"* -- **juiz
novo**, que o aval PROIBE e a TRAVA JUIZ-NOVO condiciona a `corte Ronald: juiz <nome> nasce` no CORTES.md;
(c) a soma propria fica **so para a ata** -- o pendente de `celula/precedencia` continua aberto, o passo 6
nao fecha (`tirar pendente com a impressao ainda no codigo` e PROIBIDO) e o placar nao vai a 15/20
(o teto virou **20** hoje as 14:1x, por lei dele -- L-100, secao O135 abaixo).
**O QUE A RESPOSTA ARRASTA, agora MEDIDO nos dois sentidos** (aval dele de 13:0x, secao O189 abaixo): se
for (a), o leitor que migra junto e `ponto/supra_juiz.py:131` (`real = dia.get('minutos_realizados') or 0`,
que colapsa `None` em 0). Com a linha distinguindo `None` de 0, na sombra (emp2+3+4, 09 e 10): **73 de 73**
dias de ata ZERO trocam de veredito e **6 dos 15** com numero; relavrando pela porta, **2 colaboradores
ENTRAM no TXT** (col302 emp2 09, col250 emp2 10, os dois do grupo dos **15**) e **0 SAEM** -- nenhum dos 73
move o TXT. E o preco aparece inteiro: os **73 perdem o protesto `BATIDA_ORFA_FORA_TOLERANCIA`**, porque o
ramo de `supra_juiz.py:~290` esta preso a `real == 0` -- com `None` troca-se um zero falso por **silencio
sobre uma orfa que existe**, exatamente nos dias definidos por ter uma. O numero antigo desta linha ("15
trocam, col454 sai do TXT") nao sai de cena: e o DIFF das 11:07, que mede o mundo **com** o `or 0`, e os
dois sentidos estao lado a lado na secao do O189.

**A RESPOSTA, literal:** *"(a). O dia sem par diz 'Sem turno pareado' SEM numero; as horas ficam nas
batidas da linha, e a L-102 passa a valer assim. O supra_juiz NAO passa a tratar None como 'nao sei': dia
sem turno conta como realizado ZERO DECLARADO (escrito, nao por acidente do `or 0`), para os 88 dias terem
o MESMO protesto de batida orfa."* Ela fecha os dois lados de uma vez e nenhum dos tres caminhos barrados
foi o escolhido inteiro: a TELA vai pelo (a) -- palavra sem numero, horas nas batidas da linha, Portaria
671 --, e o FECHAMENTO **nao** recebe o terceiro valor que eu media como consequencia do (a). O `None` nao
sobe ate o juiz como "nao sei": ele se escreve **ZERO por decisao** em `ponto/supra_juiz.py:131`, e e isso
que mantem os **88** dias protestando `BATIDA_ORFA_FORA_TOLERANCIA` pelo mesmo ramo -- o preco que eu havia
medido as 13:3x (os **73 perdendo o protesto**) **nao se paga**, porque a cura nao e distinguir `None`, e
declarar o zero. Nenhuma lei nova de juiz: a L-102 muda de rodape e nasce a **L-103** (as duas em
`app/docs/LEIS.md`). Registro em `app/docs/PROMPTS.md`. **Efeito aceito por ele**: 15 dia-colab trocam de
veredito, os 73 nao mudam, col454 sai do TXT da 09 na relavratura.
**A CONDICAO DE ENTRADA e DELE e e LITERAL** (LEI-AKITA 9), e **nada se aplica antes dela**: dos **15**,
listar os que tem **celula casada E previsto** (o exemplo dele e col250 29/09) e dizer se aquilo e **turno
ABERTO que o juiz da geometria devia ter achado** -- *"se for, e bug do juiz, cura na origem primeiro"*.
Entao a ordem da fatia (**O191**) nasce invertida de como eu a teria escrito: primeiro a autopsia dos 15,
depois a palavra e o zero; e a relavratura vai **dentro** do apply, com reversao em `logs/`.
**A ESTEIRA SEGUE:** passos 1-4 fechados com prova (secao das 12:xx); o passo 5 **nao espera mais** --
a lei chegou e ele vira a **O191**, que comeca pela CONDICAO DE ENTRADA dele, nao pela cura. O item
que andou em vez dele foi o **O135 TETO-20** (secao abaixo) -- nao por escolha de ordem, mas porque a
REGUA estava VERMELHA: as 13:45 o tripwire do `test_cortes_registrados.sh` mordeu o corte
`TETO-20-SEM-FAMILIA-SEM-CADASTRO` com **29 h em `recebido`** e FORA da pausa declarada (a `SAIDA` da
pausa e o export da 09, que nao o cobre), entao a suite nem rodava e NENHUM push saia -- nem o
`1cde743a` do LEI-PROTEGE-SITIO. As duas curas rapidas eram band-aid que a propria lapide do selo
nomeia (engordar o `COBRE=`, ou derivar o TOTAL das celulas ausentes), e a cura de ORIGEM era construir
a obra que o corte pede.
**`!` CUMPRIDO -- O135 TETO-20 ESTA NO AR** (`fb1c78ac`, ff + `deploy.sh --sem-migrate` num ato so as
**16:59**, 3 cascas provadas, `importerror_500=0`; secao das 16:59 abaixo).
PROVA: `logs/deploy.stamp::COMMIT` = `8c3035bc`; `fb1c78ac` (ponta do ff) e `48805bbf` (a fatia) sao os DOIS
ancestrais dele -- `git merge-base --is-ancestor` rc=0 nos dois --, e no ar
`app/core/contratos_estruturais.py:351` diz *"Nao ha constante `TOTAL`: o teto e uma"*.
O portao que o segurava era de
**ACESSO, nao de dinheiro** -- `bin/janela_auth.sh` recusando por `dow=7` sobre o UNICO sitio declarado que
a fatia tocava (`app/api/urls.py`, um `path()` read-only, **+2 linhas**, zero logica de auth) --, e eu nao
o havia forcado porque forcar portao e ATALHO e atalho esta na lista NUNCA PRE-APROVADO (L-009), e porque o
aval da manha era LITERAL sobre O142+O130+R4. O `!` dele das 14:4x resolveu o QUANDO e o aval das 16:4x
resolveu o COMO: **pela saida que JA EXISTE** (`SEM_JANELA_AUTH_MOTIVO`, `bin/deploy.sh:147`), nao por uma
segunda. A GUARDA 3 da esteira mede antes de atravessar e **recusa fora do escopo do aval** (se a guarda
falhou por ESTRUTURA, ou se nomear outro sitio de auth). O `--forcar` que o `bin/janela_auth.sh` **anuncia
e nao implementa** -- e que, lido como ref de git, sairia 1 de novo -- fica como fatia propria: **O185**,
por ordem do mesmo aval. O agendado de segunda 06:05 foi **desarmado com trilha**
(`logs/deploy_agendado/o135-teto20.desarmado`): com `teto20 == main` as quatro guardas passariam VACUAS e
ele publicaria a arvore viva daquele minuto, sem ninguem olhando. `deploys_agendados=0`.
E com isso a REGUA destravou: o corte `TETO-20-SEM-FAMILIA-SEM-CADASTRO` saiu de `recebido` (`no ar
fb1c78ac`) e `test_cortes_registrados.sh` responde `OK (63 cortes; 0 sem fatia)` -- a cura foi a obra que o
corte pedia, nao o `COBRE=` engordado.
**O PROXIMO que nao depende desta lei:** **PLACAR-ESTRUTURAL R6 item 3**.
O marco anterior esta NO AR: o `!` das 09:3x cumprido as **10:02:41** (`d1689254`).
PROVA: `d1689254` e ancestral de `logs/deploy.stamp::COMMIT` (`8c3035bc`) -- `git merge-base
--is-ancestor d1689254 8c3035bc` rc=0.
**FORA DO ATO, por LEI-AKITA 9 (escopo do aval e' LITERAL):** `raia-chamado` (`142238fc`) e
`raia-pdf` (`584265a9`) seguem sem merge -- o aval nomeia O142 + O130 + R4 e nada mais. O item O183
do BACKLOG previa a `raia-chamado` neste mesmo `!`, e eu NAO a estendi por conta propria: **uma
linha dele** ("inclui a chamado e a pdf") e elas entram no proximo ato.

### SEUS CORTES -- o que voce mandou e ainda nao esta no ar

> **Esta tabela e REGISTRO, nao cronologia**, e por isso ela nao vai para o `RELATO-ARQUIVO.md`: o
> gerador (`bin/gerar_cortes.py`) a REESCREVE entre os marcadores e
> `bin/tests/test_cortes_registrados.sh` a exige **aqui**. **A DIETA ja a levou DUAS vezes** -- 03/10 e
> 05/10, as duas vezes eu --, e nas duas quem pegou foi o selo. Na segunda a cura foi na ORIGEM: o
> ramo "marcador ausente" do gerador tentava uma ancora MORTA e, nao a achando, jogava o bloco no topo
> **calado**; agora o lugar e declarado (acima da 1a secao) e o motivo sai no stderr.

<!-- SEUS-CORTES:INICIO -->
### SEUS CORTES -- o que voce mandou e ainda nao esta no ar (41)

> **ALARME: 12 corte(s) com mais de 24 h em "recebido"** -- TROCA-DE-PLANTAO (279 h), FECHAMENTO-UI-PORTAS (260 h), CATALOGO-SAIDA-ANTECIPADA-DESCONTA (257 h), ESTEIRA-RETA-FINAL (256 h), ZUMBIDO (253 h), CARTAO-TOTAL-IGUAL-SOMA (251 h), CERT-VIGIA (239 h), CHAMADO-GANHA-CADASTRO (238 h), JUIZ-BATIDA-NASCE (238 h), JUIZ-ESCALA-NASCE (238 h), PERTO-DO-MOTOR-ESPERA-O-EXPORT (238 h), E3-CHAMADO-APOS-ARQUIVO-SIMPLES (238 h). Cada um vira Pauta de sistema para o DP ate sair de "recebido".

| corte | hora | idade | estado | fatia que consome |
|---|---|---|---|---|
| **ACESSO-NUNCA-EM-LOTE** | 2026-09-23 08:4x | 288 h | construindo | O4 + CREDENCIAL-POR-ESTADO |
| **COL200-DIA-DO-TURNO** | 2026-09-23 17:xx | 280 h | construindo | O9 PDF-E-O-ESPELHO |
| **TROCA-DE-PLANTAO** | 2026-09-23 18:3x | 279 h | recebido | O10 TROCA-DE-PLANTAO (porta no Resolver dia) |
| **CORTES-REGISTRADOS** | 2026-09-23 18:xx | 279 h | construindo | CORTES-REGISTRADOS |
| **NOITE-23-09** | 2026-09-23 18:4x | 278 h | construindo | NOITE-23-09 (infra) |
| **FABRICANTE-LE-O-BACKLOG** | 2026-09-23 20:1x | 277 h | construindo | FABRICANTE-LE-O-BACKLOG |
| **FECHAMENTO-UI-PORTAS** | 2026-09-24 13:xx | 260 h | recebido | O24 FECHAMENTO-UI-PORTAS |
| **JANELA-EXATA** | 2026-09-24 15:xx | 258 h | construindo | O27 JANELA-EXATA |
| **CATALOGO-SAIDA-ANTECIPADA-DESCONTA** | 2026-09-24 15:5x | 257 h | recebido | CATALOGO-SAIDA-ANTECIPADA-DESCONTA |
| **FILA-24-09-16-5X** | 2026-09-24 16:5x | 256 h | construindo | FILA-24-09-16-5X |
| **RELATORIO-ATESTADOS-FOTOS** | 2026-09-24 16:5x | 256 h | construindo | O29 RELATORIO-ATESTADOS-FOTOS |
| **AUSENCIAS-DRAWER-E-LOTE** | 2026-09-24 17:xx | 256 h | construindo | O30 AUSENCIAS-DRAWER-E-LOTE |
| **ESTEIRA-RETA-FINAL** | 2026-09-24 17:xx | 256 h | recebido | O31 ESTEIRA-RETA-FINAL |
| **ZUMBIDO** | 2026-09-24 20:xx | 253 h | recebido | O32 ZUMBIDO |
| **SUSPENSAO-DESCONTA-JORNADA** | 2026-09-24 22:3x | 251 h | construindo | SUSPENSAO-DESCONTA-JORNADA |
| **CARTAO-TOTAL-IGUAL-SOMA** | 2026-09-24 22:3x | 251 h | recebido | O33 CARTAO-TOTAL-IGUAL-SOMA |
| **CONTRATO-3-SEM-CONSUMIDOR-SAI** | 2026-09-25 00:xx | 249 h | esperando "!" | O35 CONTRATOS-14 |
| **CHAMADO-VARREDURA-NAO-JULGA** | 2026-09-25 00:xx | 249 h | RESPONDIDO 03/10 09:5x pelo PAPEL-PRAZO-NASCE (4a opcao: nasce o papel prazo) | O35 CONTRATOS-14 |
| **TETO-DA-MATRIZ-E-21** | 2026-09-25 00:xx | 249 h | esperando "!" | O35 CONTRATOS-14 |
| **JUIZ-DE-BATIDA-E-DE-ESCALA** | 2026-09-25 00:xx | 249 h | esperando "!" | O35 CONTRATOS-14 |
| **PERTO-DO-MOTOR-E-DO-JUIZ-DE-TURNO** | 2026-09-25 00:xx | 249 h | esperando "!" | O35 CONTRATOS-14 |
| **K8-COMPETENCIA-NAO-E-MES-CIVIL** | 2026-09-25 09:2x | 240 h | construindo | O40 K8-COMPETENCIA-NAO-E-MES-CIVIL |
| **CERT-VIGIA** | 2026-09-25 09:4x | 239 h | recebido | CERT-VIGIA |
| **ESTEIRA-SECA-1-E-2-AGORA** | 2026-09-25 10:3x | 239 h | construindo | O42 ESTEIRA-SECA-25-09 |
| **EXPORTADO-SEM-FRONTEIRA** | 2026-09-25 10:3x | 239 h | construindo | O44 ARQUIVO-SIMPLES v2 |
| **PASSIVO-TRANCADA-E-HISTORIA** | 2026-09-25 10:3x | 239 h | construindo | O44 ARQUIVO-SIMPLES v2 item 7 |
| **CHAMADO-GANHA-CADASTRO** | 2026-09-25 11:0x | 238 h | recebido | O35 CONTRATOS-14 |
| **JUIZ-BATIDA-NASCE** | 2026-09-25 11:0x | 238 h | recebido | S-BATIDA |
| **JUIZ-ESCALA-NASCE** | 2026-09-25 11:0x | 238 h | recebido | S-ESCALA |
| **PERTO-DO-MOTOR-ESPERA-O-EXPORT** | 2026-09-25 11:0x | 238 h | recebido | O35 CONTRATOS-14 |
| **E3-CHAMADO-APOS-ARQUIVO-SIMPLES** | 2026-09-25 11:0x | 238 h | recebido | E3-CHAMADO |
| **JUIZES-TRES-ASSINATURAS** | 2026-10-03 05:30 | 52 h | construindo | registro em `app/docs/CORTES.json` (03/10 08:4x) -- a TRAVA cai de 2 para 1 FALHA. O `batidas_apuraveis` e o `escala_vigente` entram em `app/core/juizes.py` nos itens 6 e 3 da ordem de 08:13, cada um com o censo do seu ponto |
| **ESPINHA-ANTES-DA-UI** | 2026-10-03 08:13 | 49 h | construindo | O134 ESPINHA-ANTES-DA-UI (ordem da fila 1) + O133 CLEAR-NO-MARCO na fila 2 |
| **PAPEL-PRAZO-NASCE** | 2026-10-03 09:5x | 47 h | registrado -- lei L-101, obra O139; censo dos 27 a medir antes de mover um nome | O139 PAPEL-PRAZO |
| **HOLERITE-MES-CIVIL** | 2026-10-04 17:5x | 15 h | construindo na raia `k5-encerrada` -- o corte RATIFICA `6a350aa9`, que ja tirou os dois sitios com nota MEDIDA (08/2026, a unica competencia com holerite publicado: 16 de 19 admitidos 21-31/08 TEM holerite de 08, contra 1 de 17 demitidos 21-31/07). Falta a segunda frase dele -- a CONDICAO de que cada conforme depende, na forma do `_A14 CURADO` -- e o teto dos dois contratos, que as duas raias deixaram no numero do main. | PLACAR-ESTRUTURAL R6 item 3, raia `k5-encerrada` (`6a350aa9`) |
| **RAIA-VERDE-POUSA** | 2026-10-04 19:2x | 14 h | lei L-105 escrita e a conduta vale DESDE JA: o pacote de pouso em curso se separou no mesmo turno -- K8 e K5 pousam como PRODUTO (`02391558`), o CERT-AST sai para pouso proprio porque e INSTRUMENTO (cria `bin/suite_nucleo.sh` e o selo `test_nucleo_tem_porta.sh`). Os itens (3) contador no ESTADO, (4) selo de host no pre-push e (5) veredito das raias velhas ficam na FILA 2, depois da CELULA-TURNO-FECHA e da O145, por ordem dele | o pouso de produto de 04/10 19:3x (K8+K5) e a lei no LEIS.md, no commit do marco |
| **DOCS-NO-MARCO** | 2026-10-04 19:2x | 14 h | lei L-106 escrita; conduta desde ja. O selo que a cobra no pre-push (push com commit so de app/docs/ alem do derivado = VERMELHO, com RED nos DOIS sentidos) e o item (4) e fica na fila 2. PROIBIDO allowlist de commit de docs, e PROIBIDO contar como marco o que nao fechou item | a lei no LEIS.md + a linha na CLAUDE.md 7b, no commit do marco de 04/10 19:3x |
| **REFERENCIA-E-A-LEI** | 2026-10-05 00:3x | 9 h | lei L-110 escrita, no marco da O191 (L-106: docs viajam com o codigo). A lei REVOGADA foi desfeita no mesmo marco e nos quatro sitios em que ja havia entrado: linha do LEIS.md, mapeamento CORTES-que-viraram-lei, entrada do CORTES.json e a LEI-AKITA 13 do CLAUDE.md. O contador cravado do test_lei_akita.sh FICA em 13, porque a lei nova ocupa a mesma linha 13 -- e o rotulo dele, que dizia 12 em texto fixo, passou a derivar do $N medido | a lei no LEIS.md + a LEI-AKITA 13 na CLAUDE.md (com o contador do selo junto) + esta linha, no commit do marco da O191 |
| **W12X36-HPD** | 2026-09-24 14:xx / 16:5x | 0 h | construindo | O26 W12X36-HPD |
| **FECHAMENTO-ONLINE** | 2026-09-20 21:0x (corte original, NAO registrado na epoca) / reafirmado 2026-09-25 12:0x | 0 h | recebido | O48 FECHAMENTO-ONLINE |
| **SITIO-SEM-CHAMADOR-AINDA-RESPONDE** | 2026-10-05 09:0x | 0 h | lei L-111 escrita e APLICADA no mesmo marco (CELULA-TURNO-FECHA): a funcao de 03/08 foi apagada com lapide, os 6 testes de test_realizado_intervalo.py foram com ela, PENDENTES['turno/marcos'] ficou () com a vaga nomeada e a celula (turno/marcos x um juiz por pergunta) ficou verde -- contratos_estruturais 13/20 -> 14/20, medido por verdes(). O RED veio do selo da casa (test_MORDE_pendente_curado_sai_da_lista), e nasceu um selo de EXISTENCIA por AST ao lado do de CHAMADA | CELULA-TURNO-FECHA (item 1 da ordem dele de 05/10 09:0x) -- a lei no LEIS.md e esta linha viajam no commit do marco (L-106) |
<!-- SEUS-CORTES:FIM -->

## 05/10 02:1x — O191 PASSO 5 **NO AR** (`fdd6f42c`): A PROVA DEPOIS BATEU **4/4**, E A ATA NAO SE MOVEU

**ESTADO: FECHADA e NO AR.**
PROVA: deploy `RC_DEPLOY=0` as **02:08:12**; suite `Ran 9629 / OK (skipped=42)`; prova depois **4/4**
dia-colab lidos com o codigo no ar (`logs/o191/prova_depois_20261005.txt`); exportada 09 **intacta**,
**8** registros hash a hash (`diff` vazio); push `024608c7..eef236e1` as 03:17 com `pre-push: OK`.
Commit `fdd6f42c`, deploy `bin/deploy.sh --sem-migrate` as **02:08:12**, `RC_DEPLOY=0`. O marco inteiro num ato so -- codigo, docs e diagrama -- e **nada entre o commit e o
deploy**, que e' a L-107 (`MERGE DE RAIA CAI E RECARREGA NO MESMO ATO`): a arvore E o bind-mount, e
foi uma janela de 11 min entre mergear e deployar que quebrou o lote de cartoes em 30/09.

### A ORDEM FOI DECIDIDA PELA LEI, E ELA INVERTE O QUE PARECIA OBVIO
O ensaio na sombra veio **ANTES** de aplicar os `.py`, nao depois. Parece ao contrario -- o instinto e'
*"aplico, ensaio o que apliquei, deployo"* --, e e' justamente o instinto que abre a janela da L-107:
o `--refazer --dump-agora` + `--bloco` levou **23 min** (01:41 -> 02:04), e nesses 23 min o `saas_ui`
estaria servindo `escala/utils.py` novo com os modulos que o importam **velhos em memoria**. Ensaiar o
codigo PRE-deploy nao e' concessao: e' o caso de rotina da casa -- o cron das 04:17 sempre monta o
ensaio da arvore de antes do deploy do dia.
Carimbo: **`dia=20261005 status=OK tipo=completa diverge=0 erros=0`**, `RC_REFAZER=0`, e o bloco
**`69/69 comandos · erro=0 · alarme=7 · fora_do_ensaio=1`**, `RC_BLOCO=0`
(`logs/o191/sombra_refazer_20261005.out`, `logs/o191/sombra_bloco_20261005.out`). O `alarme=7` e
`fora_do_ensaio=1` **sao a linha de base** -- conferido contra os ensaios anteriores no `logs/`, onde
a mesma dupla aparece em todos os blocos de 67+ comandos. Entre 00:00 e 04:00 o portao e' cego sem o
`--dump-agora` (`sombra.sh:203` soma +1 quando o dump nao e' do DIA), e e' por isso que ele foi usado.

### A PROVA DEPOIS (condicao 4 da DINHEIRO-EM-COMPETENCIA-ABERTA): **4/4 CONFERE**
Quatro dia-colab NOMEADOS, dois de cada classe, lidos **com o codigo ja no ar**
(`bin/sonda_frota.sh`, LEITURA, cpuset 4-7, banco `saas_hasner` -- `logs/o191/prova_depois_20261005.txt`).
As duas colunas por FUNCAO REAL: `NO_AR` = `escala/utils.py::montar_grade_prevista_periodo` na forma
literal do cartorio; `GRAVADO` = `escala/services/leitor_celula.py::grade_da_celula`, a MESMA chamada
que o dinheiro faz em `folha/export.py:221`.

| classe | colab | data | tipo | NO AR | esperado (sombra) | GRAVADO | esperado | `sem_turno` |
|---|---|---|---|---|---|---|---|---|
| `c:GANHA_CHAVE_VALOR` | col925 | 06/09 | folga | **731** | 731 | **0** | 0 | False |
| `c:GANHA_CHAVE_VALOR` | col51 | 07/09 | folga | **240** | 240 | **0** | 0 | False |
| `b:ZERA` | col250 | 29/09 | trabalho | **0** | 0 | **240** | 240 | **True** |
| `b:ZERA` | col382 | 30/09 | trabalho | **0** | 0 | **364** | 364 | **True** |

Tres coisas se provam de uma vez nessa tabela, e nenhuma delas e' a mesma:
1. **o numero que a sombra mediu e' o numero que subiu** -- 4 de 4, sem um minuto de diferenca, e o
   `curada` dessas linhas saiu do join de ontem (`logs/o191/impacto_join_detalhe_20261005.tsv`), nao
   de uma remedicao de hoje;
2. **a ata NAO se moveu** -- o `GRAVADO` de cada um esta identico ao de antes do deploy. E' a prova
   VIVA do que a fonte ja dizia (`cartorio.py:85-106::impressao_insumos` hasheia so INSUMO, nunca a
   ata, e o gate de `:568` segue contando `pulados`). O deploy troca o que a TELA calcula; **quem
   move a ata e' a relavratura, e ela e' ATO PROPRIO, fora deste marco**;
3. **o motivo viaja ao lado do numero** -- as duas linhas `b:ZERA` voltam com `sem_turno=True`, que e'
   o L-103 ZERO DECLARADO funcionando: nao e' "zero porque somei e deu zero", e' "zero porque nao ha
   par, e eu digo que nao ha". As duas `c` voltam com `False`, porque ali **ha** turno -- e' folga
   trabalhada, e era exatamente o `or 0` do `ata_do_dia` que a calava.

### EXPORTADA 09 INTACTA (condicao 3), HASH A HASH
Os **8** registros de `ExportacaoDominio` da competencia 09 relidos de prod depois do deploy
(`logs/o191/export09_hash_depois_20261005.txt`) e comparados com a leitura de ANTES
(`..._antes_20261005.txt`): **`diff` vazio**. Os tres VIGENTES seguem `emp2 361d0f9685f86d3a` (210
linhas), `emp3 5c503b95f9f9cd35` (86) e `emp4 84c78cd0871f5f52` (9); as 5 invalidadas tambem, com o
hash preservado -- que e' o desenho da porta `ExportacaoDominio.invalidar`, a que nunca toca
`conteudo` nem `hash_sha256`.

### SMOKE EM PROD, COM TRAFEGO REAL
O deploy provou as tres rotas (`core /health/ -> 200`, `ui /colaboradores/ -> 302`,
`mensageria /health/ -> 200`), o selo BUG 128 ficou verde nas tres cascas e `importerror_500=0`.
Depois dele, na janela de 4 min: **29 respostas reais** (`saas_ui` 23, `saas_core` 6), **0 de 5xx, 0
traceback** -- e nao e' trafego sintetico, sao colaboradores batendo ponto as 02:10 (`colab=288` no
PWA, `colab=u442` no app `okhttp`). Esta fatia nao toca `static/js/`, service worker nem template
base, entao a FRONT-SEM-SMOKE nao se aplica: o que mudou foi derivacao de servidor.
**Contei errado na primeira tentativa e vale o registro**: pedi `docker logs --since` com um carimbo
ABSOLUTO em UTC, o daemon leu como hora local futura, e as tres cascas voltaram `respostas=0 500=0`.
Zero de 500 com zero de respostas nao e' noticia boa, e' a pergunta nao feita -- a mesma familia da
`sonda-nao-conclui-com-erro`. Refiz com janela relativa e o formato real do log (`-> NNN`).

### O QUE FICA DE PE, E E' DE PROPOSITO
- **A relavratura NAO esta neste marco.** Ela e' que leva os **397 dias (+3.298,88 h)** e os **15
  (-45,27 h)** ao gravado. Competencia **10**: pre-aprovada pela DINHEIRO-EM-COMPETENCIA-ABERTA, com
  as quatro condicoes (DIFF antes, reversao em `logs/`, exportada intacta, PROVA depois).
  Competencia **09**: esta EXPORTADA, entao vira **Pauta DP com os dois numeros** (L-092 + a porta
  REGEN-EM-EXPORTADA), pelo escritor canonico `bin/gerar_avais.py --escrever`.
- **Risco nomeado que o tempo resolve sozinho, e por isso tem de ser dito**: a impressao hasheia
  `cob_status` e `chamados`. Um dia de folga cujo chamado mude de estado e' rejulgado por tabela, e
  nesse rejulgamento pega o numero novo **sem apply nenhum**. Nao e' dano -- e' mudanca LATENTE
  pingando no tempo em vez de entrar num ato medido, e e' mais um argumento para a relavratura ser
  deliberada e logo.
- **Passo 6 da CELULA-TURNO-FECHA segue NAO carimbado**, e a celula turno/marcos da matriz **nao
  fechou** com esta cura (secao de 01:1x, censo (1)): o `+2` da nota do placar e' **`+1`**.

## 05/10 01:1x — O191 PASSO 5: A LEITURA UNICA DE IMPACTO **MEDIDA NO GRAVADO** — 397 DIAS GANHAM NUMERO, 15 ZERAM, E **155 DERIVAS QUE NAO SAO MINHAS**

**LEI-AKITA: origem=`escala/utils.py` (o montador, 3 hunks ja construidos), testemunha=`CelulaDia.ata`
lida por `escala/services/leitor_celula.py::grade_da_celula` (a MESMA chamada que `folha/export.py:221`
faz), RED=`escala/tests/test_montador_realizado_pela_autoridade.py` (4 vermelhos evidenciados em
`logs/o191/red_b_montador_20261004.out`) + suite `Ran 9629 / OK` em `logs/o191/suite_copia2_20261004.out`,
quem-mais-le=os 7 sitios de producao do builder censados em `logs/o191/passo5_diff_20261004.md`,
juizes novos=0.**

Esta e a **leitura UNICA de impacto** que a L-094 manda fazer e que a **L-110** manda tratar como
**medida de impacto, nunca validacao**. Uma sonda so (`impacto_o191.py`), o MESMO arquivo rodando nas
duas copias, tres numeros por dia-colab, cada um pela **funcao real**:

| coluna | funcao | o que e |
|---|---|---|
| GRAVADO | `grade_da_celula(c, ini, fim)` | a **ata**, o que o dinheiro le hoje |
| HEAD | `montar_grade_prevista_periodo` na copia do HEAD | o builder de hoje |
| CURADA | a mesma funcao na copia curada | o builder depois da cura |

Forma de chamada **literal** do cartorio (`ponto/services/cartorio.py:433-452`): janela
`[d0-1d, d1+3d)` com o `-1us`, `ref = fim+1d`, `n_dias = (fim-ini).days+2`, e o dia lido por
`ref - i dias`, exatamente como `dias_por_data` monta. **Universo = o do CARTORIO**, nao um meu: quem
tem CELULA na competencia, **sem filtro de situacao** (`processar_cartorio.py:49-51`,
HX-CARTORIO-UNIVERSO T4.2). O primeiro rascunho desta sonda filtrava `Colaborador.ativo=True` -- campo
que **nao existe** neste modelo -- e foi o proprio `FieldError` que me obrigou a ir buscar quem decide o
universo em vez de inventar um.

### O NUMERO, por classe e por competencia (sombra do dia 04/10, carimbo `completa diverge=0 erros=0`)

| classe | dia-colab | colabs | delta (curada − head) |
|---|---|---|---|
| **(c) GANHA CHAVE COM VALOR** | **397** | **114** | **+197.933 min = +3.298,88 h** |
| &nbsp;&nbsp;· comp **09** (exportada) | 291 | 94 | +142.778 min = +2.379,63 h |
| &nbsp;&nbsp;· comp **10** (aberta) | 106 | 51 | +55.155 min = +919,25 h |
| **(b) ZERA (ZERO DECLARADO, L-103)** | **15** | **12** | **−2.716 min = −45,27 h** |
| &nbsp;&nbsp;· comp 09 / comp 10 | 12 / 3 | 10 / 3 | −2.051 / −665 min |
| (c') ganha chave com ZERO | 10.542 | 555 | 0 min |
| (c') idem, dias **FUTUROS** (≥ 05/10) | 3.790 | 521 | 0 min |
| **DERIVA PRE-EXISTENTE** (head ≠ ata, a cura nao toca) | **155** | **43** | **+17.889 min = +298,15 h** |

**Nao ha uma unica linha `SOBE`, `DESCE` ou `PERDE_CHAVE`:** fora dos 15 que zeram, **a cura nao muda
nenhum valor que o builder ja dava**. Ela da numero onde nao havia chave, e troca soma propria por zero
declarado. `ERROS da sonda: 0` nas duas copias (`logs/o191/impacto_head_20261005.tsv` 14.894 linhas,
`logs/o191/impacto_curada_20261005.tsv` 577).

### O QUE A CLASSE (c) E, LIDO NA ATA DE **PROD**: **FOLGA TRABALHADA**
Nao e abstracao. Li a ata viva dos 567 dias alvo em producao (`bin/sonda_frota.sh`, LEITURA, cpuset de
teste, banco `saas_hasner`) e ela diz:

- **396 dos 397** dias da classe (c) **TEM celula** -- so 1 nao tem;
- **394** deles tem `ata.tipo_dia = 'folga'` com **`ata.minutos_realizados = 0`**;
- **331** ja carregam veredito **`fato_sem_previsao`**, e 63 `concorde`.

Ou seja: **a casa JA SABE que houve fato naquele dia de folga -- e lavra ZERO minuto.** O montador
entrava no ramo de folga (`d not in cel_por_data`, `escala/utils.py:1319`), nao emitia a chave, e
`ata_do_dia` fazia `int(dia.get('minutos_realizados') or 0)` (`cartorio.py:262`): o `or 0` transformava
*"nao perguntei"* em *"zero"*. E a familia do `[]` de dois sentidos da secao 6 do CLAUDE.md, com outro
nome: **ausencia de sinal lida como sinal bom.**

### CORROBORACAO: DUAS SONDAS DIFERENTES, OS MESMOS MINUTOS
A medicao de ontem (frota, filtro `ata.minutos_realizados == 0 and autoridade > 0`) deu **445 dia-colab
/ 220.041 min**; esta deu **397 / 197.933 min**. **Nao e contradicao, e universo** -- e a diferenca
grande esta NOMEADA: as **50 linhas / 4 colabs** com veredito `sem_celula` de ontem saem daqui porque
**colab sem celula nenhuma na competencia nao esta na fila do cartorio**, e dia sem celula **nao tem ata
para lavrar** (a cura os alcanca no espelho e no PDF, nunca no dinheiro). As **24 linhas de amostra** que
ontem ficaram escritas nominalmente estao **24/24 DENTRO** da classe (c) de hoje, e os minutos batem **ao
minuto** nas quatro que eu conferi caso a caso: col190 13/09 = 728 · col203 22/08 = 426 · col255 02/09 = 2 · col654
25/08 = 17. Dois caminhos independentes, o mesmo numero. O numero **operativo para o apply e o de hoje**,
porque o universo dele **e o do leitor de producao**, e porque ele foi medido contra o **GRAVADO** dos
dois lados -- a licao literal do AVAL-DE-CRITERIO, onde o DIFF motor-x-motor dava +12,29 h e escondia 10
campos.

### O QUE O DEPLOY MOVE NO GRAVADO: **ZERO**, e agora com a fonte na mao
Reconferido no codigo, nao suposto: `impressao_insumos` (`cartorio.py:93-106`) hashea **batidas, status
de cobranca, chamados, DNA, veto e teto** -- **nunca a ata**. O gate do pulo (`cartorio.py:568`) compara
`cel.impressao == imp`. Logo: a cura muda o valor DERIVADO, a impressao das **412 celulas** que mudam de valor (397 + 15) **nao muda**, o
cron das 06:28 segue contando `pulados`, **e a ata segue lavrando 0 ate uma relavratura**. `--forcar`
existe exatamente para isso e se chama, no proprio help, *"HX-BORDA-ATA: rejulga mesmo com impressao
igual (backfill de ata)"*.

E a ressalva que **nao** e boa noticia, e que eu prefiro escrita a descoberta depois: a impressao hashea
`cob_status` e `chamados`, entao **folga do passado cujo chamado troque de estado e re-julgada
incidentalmente** e grava o numero novo sem apply nenhum. Mudanca latente espalhada no tempo e pior que
mudanca imediata, porque ninguem a ve acontecer.

**Decido pela lei existente e registro (PAREI-SO-LEI: decisao tecnica nao devolve turno).** A
relavratura **nao entra neste marco**: ela e ato proprio, com DIFF proprio, e se separa por competencia
--- comp **10** (aberta) e PRE-APROVADA pela DINHEIRO-EM-COMPETENCIA-ABERTA com as quatro condicoes;
comp **09** esta exportada, e por O TXT E FOTOGRAFIA DO CALCULO a correcao **pode** entrar a qualquer
momento, mas ela vale **+2.379,63 h em 94 colabs de uma competencia ja entregue**, entao sai como
**Pauta DP com os DOIS numeros**, nunca como numero que muda em silencio. O que vai ao ar agora e o
**calculo certo**; o que mexe no que foi pago tem dono e nome.

### A REVERSAO, ANTES (condicao 2 da DINHEIRO-EM-COMPETENCIA-ABERTA)
`logs/o191/reversao_ata_o191_20261005.jsonl` -- **566 linhas** (os 567 alvos menos o unico dia sem
celula), lidas de **prod**, uma por celula, com `celula_id`, `colaborador_id`, `data`, os campos da ata
que a cura alcanca (`minutos_realizados`, `minutos_previstos`, `n_missing`, `n_celulas`, `tipo_dia`),
mais `impressao`, `veredito`, `veredito_em` e `julgada_em`. Com ela, qualquer relavratura futura se
desfaz celula por celula. **"Sem trava" nunca foi "sem prova".**

### TRES COISAS QUE A MEDICAO ME COBROU E QUE EU NAO SABIA AO COMECAR
1. **A montagem da copia tem UM escritor, e eu ia monta-la a mao outra vez.** O runner nasceu com
   `-v .../staticfiles:...` digitado, e o docker devolveu **rc=125** duas vezes -- primeiro por
   `/app/staticfiles`, depois pelos **tres tmpfs de cache** -- porque o `app` da copia entra `:ro` e o
   ponto de montagem tem de **existir na copia**. A linha passou a ser
   `$(bash bin/arvore_do_push.sh --montagem "$COPIA")`, que e o escritor unico da lista (CLAUDE.md secao
   3, a lapide das **92 copias orfas / 2,1 GB**), e os pontos de montagem nasceram nas duas copias. O
   defeito nao foi o mount: foi eu ter escrito a mao o que ja tem dono.
2. **`sem_celula` nao e so "menos 50 linhas": e uma CLASSE com dono proprio.** 4 colabs, 50 dias, 417,63
   h em que a cura muda o espelho e o PDF **e nao pode mudar o dinheiro**, porque nao existe celula para
   lavrar. Pela **L-099** isso tem dono: **CADASTRO** (`gerar_celulas` nao fez celula para aqueles dias),
   nao ESTRUTURA -- e dono CADASTRO **nao se cura por codigo**, vai para a lista do admin. Fica
   registrado aqui, com numero, para nao virar "detalhe que sumiu no arredondamento".
3. **Eu tinha uma hipotese limpa sobre o impacto no dinheiro, e ela era FALSA -- caiu porque eu medi em
   vez de afirmar.** O raciocinio era: o ramo de folga do montador e `if d not in cel_por_data`, logo
   esses dias **nao tem celula**, logo o numero novo nunca alcanca a ata e o impacto e estruturalmente
   zero. Perguntei ao banco (`tem_celula.py`, sombra) e a resposta foi **396 de 397 TEM celula** -- mais
   155/155 da DERIVA e 15/15 da (b). O motivo real: `cel_por_data[d]` so e preenchido **dentro do laco
   dos dias de TRABALHO** (`escala/utils.py:1292`), entao dia cadastrado como FOLGA que **tem**
   `CelulaDia` (`trabalha=False`) cai no ramo de folga de qualquer jeito -- e foi essa medicao que deu
   NOME a classe (c). A conclusao *"o deploy move zero na ata"* sobreviveu, **mas por um motivo
   diferente e verificado** (a impressao e insumo-only). Escrever a hipotese como fato teria deixado o
   RELATO com a conclusao certa e a razao errada, que e a pior forma de estar certo: nao avisa quando
   deixa de valer. A ressalva do `cob_status` acima a minha hipotese nao previa; a fonte previu.

### DOIS CENSOS QUE EU FIZ ANTES DO COMMIT, E UM DELES **CORRIGE O PLACAR DA CASA**

**(1) `minutos_realizados_do_dia` fica SEM CHAMADOR DE PRODUCAO -- e a celula de TURNO nao fecha
com isso.** O placar diz que fecharia: *"(2) celula/precedencia x juiz (1) e turno/marcos x juiz (1)
sao O MESMO SITIO: escala/utils.py:822::minutos_realizados_do_dia (...) UMA cura fecha DUAS celulas
(+2)"* (`core/placar_estrutural.py:262`). **Medido, e' `+1`.** As duas impressoes nao estao no mesmo
lugar: a de **celula/precedencia** e' o CHAMADOR (`real_por_data[d] = minutos_realizados_do_dia(`),
que eu apaguei -- e e' por isso que o `test_MORDE_pendente_curado_sai_da_lista` ficou VERMELHO na
primeira rodada, cobrando a allowlist a zero. A de **turno/marcos** e' `escala/utils.py:861`
(`if prev is not None and m + base < prev:`), **DENTRO do corpo da funcao** (822-878), que eu nao
toquei. Eu matei o chamador, nao a funcao. Censo na copia curada: fora de `/tests/` restam so
comentarios e lapides (`dia_decidido.py:165`, `espelho.py:856`, `placar_estrutural.py:263`,
`juizes.py:249/255/291`) e o proprio `def`. **Quem mantem a funcao viva e' bateria propria**:
`escala/tests/test_realizado_intervalo.py` a chama em **6 assercoes**. Nao apaguei de proposito --
a celula de turno espera a lei da BUG-145 (aval 03/10 12:4x: o contador de
`ponto/services/bordas_realizado.py` se reescreve ANTES), e apagar funcao com bateria propria e'
alargar a cura fora da origem (a mesma linha que a MEIA-CORRECAO me cobrou ontem, pelo outro lado).
Efeito colateral documental que eu registro em vez de corrigir: `ponto/tests/test_contract_juiz_turno.py:74`
chama o sitio sobrevivente de *"o da TELA"*, e depois desta cura **nenhuma tela o alcanca** -- a
frase envelheceu no ato, e consertar a prosa dela e' tocar o contrato da familia turno. **Passo 6
continua NAO carimbado.**

**(2) A folha e' CEGA ao `realizado_sem_turno`.** Procurei quem le a bandeira que o ramo (b) passa a
emitir: fora de `/tests/`, o unico consumidor e' `ponto/services/bordas_realizado.py:79` (o contador
do passo 1); quem a emite e' `espelho.py:752` e os dois ramos do montador. **Zero ocorrencias em
`folha/`.** Isso tem consequencia pratica e ela e' boa para este marco: a testemunha do dinheiro nao
ve o motivo ao lado do numero -- ela so veria o numero, e o numero so chega la pela relavratura, que
**nao esta neste ato**. Tambem quer dizer que o `realizado_sem_turno` ainda **nao** e' um rotulo que
a folha possa mostrar: se um dia tiver de ser, e' fatia propria, com leitor nomeado.


## 05/10 00:3x — O191 PASSO 5: A SUITE VOLTOU **VERDE (9.629)**, AS 5 FALHAS ERAM **UMA MEIA-CORRECAO MINHA**, E A MINHA PROPRIA TABELA DE LEITORES ESTAVA **INVERTIDA**

**ESTADO: a cura (b)+(c) esta CONSTRUIDA e VERDE na copia, nada aplicado, nada commitado, nada no ar.**
PROVA: `logs/o191/suite_copia2_20261004.out` -- **`Ran 9629 tests in 369.840s` · `OK (skipped=42)` · `RC=0`**,
zero FAIL e zero ERROR, rodada contra a copia `/home/ronald/copia-o191a` pela porta unica (`bin/suite.sh --dir`).
Os **61 selos de host** na arvore viva: `vermelhos: 0` (medido 05/10 00:33:17, e de novo depois dos docs).

### AS 5 FALHAS DA PRIMEIRA RODADA, E AS 3 FAMILIAS DELAS
A primeira rodada deu `FAILED (failures=5, skipped=42)` (`logs/o191/suite_copia_20261004.out`). As cinco
cairam em tres familias, e **nenhuma pediu lei nova nem afrouxamento**:

| # | sitio | familia | cura |
|---|---|---|---|
| 1 | `api/tests/test_api_mensageria_registros_pendentes.py:34` (`0 != 1`) | selo que **COPIA** o valor de `PENDENTES['celula/precedencia']` | literal 1 -> 0, com o motivo nomeando a BUG-145 |
| 2 | `ponto/tests/test_realizado_do_dia_autoridade.py:115` (`'escala/utils.py' not found in set()`) | idem, mas **afirmando que o pendente EXISTE** | premissa morta por cura: a assercao se **inverte** (golden), e passa a medir por **AST** -- `chamados.count('minutos_realizados_do_dia') == 0` e `chamados.count('realizado_do_dia') >= 2` |
| 3 | `escala/tests/test_realizado_cronologico.py:26` (`[] is not true`) | **fixture medindo caminho morto** | a batida passa a ser PERSISTIDA; o 660 fica |
| 4-5 | `core/tests/test_selo_diagrama_do_codigo.py` (`DIVERGE`) | o `.mmd` **assa** o contador | `ARQUITETURA.mmd` regenerado contra a copia |

### A FALHA 1 E A 2 SAO **A MINHA PROPRIA MEIA-CORRECAO**, e isso tem nome na casa
Quando esvaziei `PENDENTES['celula/precedencia']` para `()`, eu fechei o censo do **codigo curado** e **nao**
censei quem le a **LISTA**. Sao **tres** selos que copiam aquele valor; eu achei **um**
(`ponto/tests/test_contract_juiz_celula.py:110`) e a suite achou os outros **dois**. O censo agora esta
fechado por grep de `celula/precedencia|PENDENTES_CELULA|fora_de_autoridade|fora_da_autoridade`: **3 copiam
o valor** (os tres acima) e **5 sao estruturais e seguem verdes** (`test_contract_juiz_celula.py:104`, que
compara `fora_de_autoridade(FAMILIA) == len(PENDENTES_CELULA)` e da `0 == 0`; `core/placar_registro.py:17`;
`api/views_mensageria.py:1440` e `:1448`; `core/tests/test_selo_diagrama_do_codigo.py:69`).
**MEIA-CORRECAO E PIOR QUE NENHUMA** cobra censo de ESCRITORES; o que faltou aqui foi censo de **LEITORES da
lista**, que e o outro lado da mesma linha -- esvaziar uma lista e uma escrita cujo universo e todo leitor dela.

### A FALHA 3 NAO ERA LITERAL COPIADO: A BATERIA S133-RAIZ E **ESTRUTURALMENTE CEGA** A AUTORIDADE
Medido com sonda de diagnostico, nao suposto: `escala/tests/test_grade_por_turno.py::_b` devolve
`types.SimpleNamespace(timestamp=dt, tipo=tipo)` -- batida que **nunca** e persistida -- e
`ponto/turnos.py::turnos_do_colab` **le o BANCO**. A sonda nao imprimiu **UM** turno, e os dois dias davam
`sem_turno=True`. Conclusao com numero: o **660** daquele selo vinha **inteiro da soma propria**, por **48
dias** (17/08 -> 04/10), e o selo nunca exercitou a autoridade que diz medir.
Com as quatro batidas persistidas (`18:55` / `01:00` / `02:00` / `06:55`), `turnos_do_colab` acha **UM** turno
(`data_turno=2026-07-02`, 18:55->06:55) e `realizado_do_dia` devolve **exatamente 660**, com `sem_turno=False`.
Entao a cura foi **persistir a batida** -- a propriedade de 17/08 fica de pe e passa a correr sobre o juiz --,
jamais afrouxar o numero. E a **guarda que faltava** entrou:
`assertIs(dia[0].get('realizado_sem_turno'), False, ...)`, sem a qual o selo voltaria a medir caminho morto em
silencio (SELO ANTI-VACUIDADE: ausencia de sinal lida como sinal bom).
**ESCOPO:** a cegueira do `_b` e de toda a bateria S133-raiz, **nao** se cura dentro da O191 -- fica
REGISTRADA aqui, com o numero, como item proprio.

### O `.mmd` MUDOU **UMA LINHA**, e o `MAPA.md` nenhuma
`jf_celula_precedencia[... registro: 1 sitio(s)]` -> `registro: 0 sitio(s)`. O atalho de host
(`bin/gerar_diagrama.py`) roda `docker exec saas_core` e por isso so alcanca a **arvore viva**; contra a copia
a forma e `docker run $TESTE_DOCKER -v <copia>/app:/app ... manage.py gerar_diagrama --settings=config.settings.ci`.

### A MINHA TABELA DE LEITORES ESTAVA **INVERTIDA**: A PRONTIDAO **NAO** MUDA NO DEPLOY
Eu havia escrito, no proprio deliverable, que *"no instante do deploy, sem nenhum apply, UM leitor muda: a
`prontidao`"*. **Errado, e a correcao inverte a conclusao.** Eu classifiquei `folha/export.py::prontidao` como
leitora da GRADE **sem ler de qual grade** ela fala: ela chama `grade_do_fechamento(fech)`, e
`folha/export.py:221::grade_do_fechamento` nao chama o builder que eu curei -- chama
`escala/services/leitor_celula.py::grade_da_celula`, que o proprio cabecalho declara como *"LE A CELULA
SOBERANA (CelulaDia + ata lavrada pelo cartorio)"*. Lido linha a linha: cada dia nasce de
`ata = (cel.ata ...) or {}` (`:331`) e o numero sai de `minutos_realizados=int(ata.get('minutos_realizados') or 0)`
(`:364`) -- **sempre da ata**; o unico fallback do corpo e de **previsto** (`:346-352`) e nunca toca o realizado.
Logo os **dois** leitores de dinheiro leem a ATA: `ponto/services/dia_pago.py::por_dia_da_grade` (escritor de
`DiaPago`) e a `prontidao`. **Nenhum dos dois se move no deploy.**
O censo CERTO -- quem consome o builder curado em producao -- e: o **CARTORIO**
(`ponto/services/cartorio.py:452`, e e por aqui, e so por aqui, que o numero novo alcanca a ata), o PDF
(`relatorios/pdf_espelho.py:193`), `mapa_divergencia.py:39`, `simular_dia.py:7`, o cron 06:20
(`reconciliar_grade.py:119`, que roda **sem `--apply`** desde 30/08) e o cron 06:26
(`detectar_par_relampago.py:93`). O espelho (`ponto/services/espelho.py:495`) importa o builder **apenas** no
ramo que o proprio corpo chama de *"Fonte ANTIGA (builder). Fica como degradacao nomeada, nao como caminho
normal"* (T4-CUTOVER-ESPELHO, 01/09).
**O ORACULO SEGUE INTACTO, e agora pelo motivo certo.** Eu havia dito "porque ele le o espelho e o espelho nao
le a grade", e a **segunda metade era falsa**. A conclusao se sustenta por outra via, medida no corpo: o
`minutos_realizados` que o espelho publica nasce em `espelho.py:751` de `_real_dia.minutos`, isto e, da chamada
**propria** do espelho a `ponto/turnos.py::realizado_do_dia` -- a mesma autoridade. `e6_oraculo.py:179` le
`esp.get('dias')`, entao o juiz do dono da **L-099** nao se move e a distincao deliberada dele entre `None` e
`0` sobrevive.
**EM UMA LINHA:** no deploy **nenhum numero de dinheiro e nenhum `pct` de prontidao se move**; muda o que o
cartorio passa a LAVRAR, o PDF, o mapa e a simulacao. O numero novo so alcanca o dinheiro quando a ata for
**relavrada** -- na **10** pelo recalculo por evento (PRE-APROVADO, com DIFF antes), e na **09** por **nada**,
porque ela esta exportada e a porta dela e a REGEN-EM-EXPORTADA.
A correcao esta ESCRITA, nao apagada, no deliverable: `logs/o191/passo5_diff_20261004.md`, secao *"CORRECAO DO
CENSO ACIMA"*.

### A LEI NOVA **L-110 `REFERENCIA-E-A-LEI`** -- E A QUE FOI REVOGADA ANTES DELA, DESFEITA NO MESMO MARCO
Chegaram duas leis em minutos, e a segunda revogou a primeira. **`FROTA-PROVA-AMOSTRA-EXPLICA` foi REVOGADA**
e desfeita nos **quatro** sitios em que ja havia entrado: a linha do `LEIS.md`, o mapeamento em *CORTES que
viraram lei*, a entrada do `CORTES.json` e a `LEI-AKITA 13` do `CLAUDE.md`. O nome dela sobra **so como
historia** dentro da revogacao. No lugar, **L-110**: *a referencia de um numero e a **REGRA** (CLT, CCT,
`L-NNN`) aplicada ao cadastro e as batidas -- o codigo que existe hoje **nao e gabarito** do codigo novo.*
ORDEM NA FATIA: casos respondidos pela regra **antes** do codigo; pergunta de lei ao topo do RELATO antes de
codar; o novo se certifica contra os casos e contra as **propriedades fixas**; o caminho velho se le **UMA
vez, na troca**, so para dizer quem muda de valor e quanto (**L-094**, **L-092**) -- medida de **impacto**,
nunca validacao.
**O CONTADOR ANDOU COM A LEI, no mesmo ato:** `bin/tests/test_lei_akita.sh` cravava `[ "$N" = 12 ]` e passou a
13; e o rotulo final dele, que dizia *"12 linhas"* em texto fixo, passou a derivar do `$N` medido -- rotulo
cravado ao lado de contador medido e a forma que envelhece em silencio (LEI-AKITA 8).
**A COLUNA `PROTEGE` FICA VAZIA, e por LEI, nao por omissao:** o corte manda *"PROTEGE vazia se nao houver
sitio, sem deducao"*, o cabecalho do `LEIS.md` ja chamava de proibido preencher por deducao, e
`bin/tests/test_leis_indice.sh` declara PROTEGE como a **unica** coluna que pode ficar vazia. O alvo e um
**metodo**, nao um `arquivo.py::funcao`. PROVA dos tres selos de lei: `test_leis_indice: OK -- 78 leis`,
`lei_akita: OK (13 linhas)`, `test_lei_protege_sitio: OK -- 4 sitios protegidos em 78 leis`.
**O QUE A L-110 MUDA NO QUE EU IA FAZER AGORA:** o DIFF (b)+(c) **nao morre, muda de PAPEL** -- deixa de ser
gabarito e passa a ser a leitura UNICA de impacto, que e exatamente o que a **L-094** ja exigia no deploy. A
lei vale **do proximo item em diante** e o corte isentou a O191 explicitamente.

### A DIETA (L-109) CUMPRIDA, E ELA ME PEGOU **NO MESMO DEFEITO DE 03/10** -- CURADO NA ORIGEM
**5.467 linhas** foram para `app/docs/RELATO-ARQUIVO.md`, de `## PLACAR-ESTRUTURAL (02/10 23:5x)` ate o fim
do bloco do O108 de 01/10; o vivo caiu de **10.312 para 4.847** linhas e passa a guardar 03, 04 e 05/10
(a mesma conta do arquivamento de 03/10, que guardou 3 dias). **Mover, nunca apagar**: conferido por
`grep -F` dos tres titulos de fronteira -- `vivo=0 arquivo=1` em cada um -- e pela soma das linhas, que fecha.
O corte foi por **CONTEUDO**: parou exatamente onde o comentario que o corte de 03/10 deixou no arquivo vivo
manda parar (*"a cauda do vigia da esteira vive no FIM do arquivo"*), e as duas entradas `###` de 02/10 que
ficaram vivas estao DENTRO da secao de 04/10 00:5x -- mover meia secao seria tocar o texto.
**E ENTAO O SELO ME PEGOU, pelo MESMO motivo de 03/10.** `test_cortes_registrados.sh` ficou VERMELHO: *"a
tabela SEUS CORTES nao esta no RELATO"*. Ela e **REGISTRO, nao cronologia** -- o gerador a reescreve entre
marcadores -- e o arquivamento por DATA a levou junto. **O bloco carrega, escrito dentro dele, o aviso de
03/10 dizendo exatamente isso**, com a frase *"Foi o selo que me pegou -- eu movi o RELATO por data e levei a
tabela com ele"*. Licao em PROSA, dentro do proprio objeto, e o leitor que nao migrou fui eu, de novo.
**CURA NA ORIGEM, nao no sintoma** (LEI-AKITA 1 + 6, e as duas curas nao conflitam, entao as duas entram --
CURA-MAIS-RESTRITIVA): o ramo *"marcador ausente"* de `bin/gerar_cortes.py::escrever` tentava a ancora
`## ESMERIL-ESPELHO`, **secao que nao existe mais**, e ao nao achar jogava o bloco no TOPO do arquivo
**CALADO**. Isto e, o caso que acontece DUAS vezes era justamente o que ele tratava em silencio, com uma
ancora morta. Agora o lugar e **DECLARADO e estrutural** (acima da 1a secao `## `, que e onde o topo acaba,
achada por regex de ESTRUTURA e nao por titulo), o motivo sai por escrito no `stderr` nomeando a causa unica
conhecida, e sem a ancora o gerador **para** (`SystemExit`) em vez de adivinhar. **PROVA**: rodado, o bloco
renasceu no lugar declarado com o aviso impresso, e `cortes_registrados: OK (67 cortes; 0 sem fatia)`.

### O PORTAO DO DEPLOY ESTA **VERMELHO**, E O CAMINHO E O DA FRENTE
`bin/sombra.sh --conferir` as 00:03:37 deu `carimbo dia=20261004 status=OK tipo=completa diverge=0 erros=0`:
carimbo **de ontem**, e `deploy.sh` exige o de **HOJE**. O cron e `17 4 * * *`. Entre 00:00 e 04:00 o portao e
cego (`sombra.sh:203` soma +1 quando o dump nao e do dia), entao o caminho e
`--refazer --dump-agora` + `--bloco` (~38 min), **nao** `--sem-sombra`: atalho esta na lista NUNCA
PRE-APROVADO, e custo de tempo nao e argumento (LEI-AKITA 3). **A sombra de 04/10 segue valida para MEDIR**
(completa, `diverge=0`) -- so o portao do deploy pede "de hoje" --, entao a medicao de impacto vem **antes** do
refazer, que a reconstroi.

## 04/10 23:4x — O191 PASSO 5: A FROTA DESMENTIU A MINHA AMOSTRA, O UNIVERSO E **441** E NAO 70, E O TETO TEMPORAL VIROU ITEM PROPRIO

**A CONDICAO DE ENTRADA DELE ESTA RESPONDIDA, e nenhuma das duas respostas e a que eu tinha escrito.**
Ele pediu (LEI-AKITA 9) que, dos 15 dia-colab que trocam de veredito, eu nomeasse os que tem celula
casada **E** previsto e dissesse se aquilo e turno ABERTO que o juiz da geometria devia ter achado --
*"se for, e bug do juiz, cura na origem primeiro"*. Nao e: o par esta **FECHADO** e com `data_turno`
no proprio dia (col250 `02:00:29 -> 06:00:29`, col382 `01:01:26 -> 07:50:17`). O que ha e um **ramo do
montador que nunca perguntou o realizado** -- o dia de FOLGA.

**OS 70 `concorde` SAO TRES POPULACOES, e eu as tinha como uma.** Medido na frota, na sombra
(`logs/o191/passo5_universo_cura_c_20261004.txt`), cruzando veredito x `orfas` x julgada-no-proprio-dia:

| causa | `orfas` | julgada no dia | dias | minutos |
|---|---|---|---|---|
| (A) o `[]` de dois sentidos | `0` | nao (54) / sim (2) | **56** | 18.274 |
| (B) teto temporal na lavra | `>= 1` | **SIM** | **13** | 6.146 |
| (C) sobra nomeada: col107 emp3 23/08 | `4` | nao (julgada 23/09) | 1 | 374 |

**(A) ME DA RAZAO NA EXPLICACAO QUE EU MESMO TINHA RETIRADO**, e o motivo de eu te-la retirado importa
mais que o acerto: eu escolhi as 6 amostras do contra-exemplo **pelos maiores minutos**, e os maiores
minutos sao os dias mais FRESCOS (02-03/10) -- que sao exatamente a populacao (B). **A amostragem por
tamanho selecionou por frescor, e o frescor ERA a causa.** Nao foi azar: foi um criterio de amostra que
correlaciona com a variavel em teste, e e a terceira vez que a casa paga por criterio pela FORMA.

**(B) E DEFEITO NOVO, SEPARADO, E VIROU A O194** -- nao se constroi dentro da O191.
`ponto/services/cartorio.py:881` segura o dia de hoje com `cel.trabalha is not False`, e em folga isso
e **False** pela T3.2-FOLGA-SEM-TURNO (*"dia de FOLGA nao tem turno a esperar"*, 29/08): a folga e
julgada **no ato da batida**, `dia_encerrado` sai False, o ramo do FATO e pulado, `cods=[]`. E o
congelamento e **duravel**, nao transitorio: `impressao_insumos` **nao hashea `dia_encerrado`**, entao
o cron das 06:28 recomputa a mesma impressao e conta `pulados` para sempre. PROVA: **col925 emp3 04/09
e 06/09**, julgados as 18:02:43 e 18:06:19 do proprio dia, seguem `concorde` hoje -- **30 dias, ~270
corridas de cartorio, zero re-julgamento**. E a familia TETO TEMPORAL da CLAUDE.md sec.6, com a casa
julgando pela forma fraca (DATA) um fato que e encerrado pela forte (par completo).

**O UNIVERSO DA CURA NAO E 70: SAO 441**, e este numero muda o que o apply significa. Dict de folga sem
a chave + autoridade achando minuto + ata lavrando 0: **441 dia-colab, 113 colaboradores, 220.041 min =
3.667,35 h**. Por veredito: `fato_sem_previsao` 319 · `concorde` 66 · `sem_celula` 50 · `fora_vinculo` 5
· `trabalhou` 1. **Por competencia, que e o que decide o que pode ser aplicado: 09 (EXPORTADA) 313 dias
/ 93 colabs / 2.552,77 h; 10 (aberta) 128 / 53 / 1.114,58 h.**

**E CORRIJO UM NUMERO MEU, do jeito que o advisor cobrou:** eu havia escrito que os 319
`fato_sem_previsao` eram *"ata STALE, que a relavratura cura"*. **Falso.** O filtro da frota foi
`ata.minutos_realizados == 0 and autoridade > 0`, entao os 445 compartilham o MESMO defeito de NUMERO, e
relavratura sem a cura (c) re-roda o mesmo ramo de folga -- a ata continua 0. **So a cura (c) move o
numero.**

**O QUE O DEPLOY MOVE NO GRAVADO: ZERO, POR CONTA DA CURA** -- lido no codigo, nao suposto. A cura (c)
acrescenta chave ao dict da **GRADE**, e `impressao_insumos` nao hashea a grade: a impressao das 441
celulas nao muda, o `cartorio.py:568` segue contando `pulados`, e a ata segue lavrando 0 **ate uma
relavratura**. Por isso o DIFF do apply leva **DOIS numeros** (o que o classificador passa a calcular e
o que fica gravado sem relavratura) em vez de um que exagera. **E a ressalva honesta vai junto:** a
impressao hashea `cob_status` e `chamados`, entao folga do passado cujo chamado troque de estado e
**re-julgada incidentalmente** e grava o numero novo sem apply nenhum -- *zero por conta da cura;
re-julgamento incidental por insumo que mude grava o numero novo*.

**NEUTRA EM DINHEIRO POR `tipo_dia`, MEDIDO CONTRA O ESCRITOR REAL.**
`ponto/services/dia_pago.py::por_dia_da_grade` (`:314-342`) e a unica derivacao, com dois chamadores
(`fechamento.py` e `retratar_exportada`), e o universo dela e `tipo_dia in ('trabalho','ausencia')`:
**folga esta fora**. O selo que prova isso CHAMA a funcao (nao um mock) e tem controle positivo --
injetar 240 min na folga nao muda nenhuma das tres somas. **E o censo dos tres campos novos esta
fechado**: `realizado_sem_turno` / `_turno_aberto` / `_turno_longo` descem em todo dict de folga, e o
unico leitor do dict da grade e `bordas_realizado.py`, guardado por `tipo_dia != 'trabalho'`;
`espelho.py:752` chama a autoridade ele mesmo (independe da grade) e **template/JS: 0 casamento**. A
hipotese contraria era barata e caia -- se o espelho lesse a chave sem guarda, toda folga de todo 12x36
passaria a dizer "Sem turno pareado" e o `test_smoke_chromium` nao veria, porque renderiza UMA tela.

**OS TRES SELOS DA L-102 SE INVERTERAM, NAO SE APAGARAM**, com os mesmos fixtures de prod e a assercao
trocando de lado: o dia sem par agora leva `== 0` **e** `realizado_sem_turno is True` (as duas metades
da L-103: o numero e zero, e o zero tem dono), e os codigos que ele acende passam de `assertNotIn` a
`assertIn` -- col250 `REALIZADO_ZERO_COM_TURNO` com `previsto=660`; col174 os **TRES**
(`+FURO_PARCIAL`, `+BATIDA_ORFA_FORA_TOLERANCIA`). **E o selo passou a nomear a batida**: a orfa e a de
**00:57**, nao a de 03:30 que eu havia escrito no docstring -- a 03:30 CASA o marco 03:00 com 30 min, e
um emissor que acusasse a errada passaria por um `assertIn` de codigo cobrando o colaborador por uma
batida que tem marco. Pela terceira vez neste turno, a prosa que eu escrevi afirmava mais que a medicao.

**A MESMA FRASE FALSA SAIU DOS DOIS SITIOS.** O cabecalho do arquivo de teste ainda dizia *"contagem
dobrada -- aquelas celulas sao as batidas do turno do dia SEGUINTE, e elas JA contam no realizado
daquele dia"*, que a medicao desmentiu (o par e do PROPRIO dia; a guarda L-085 de `ponto/turnos.py:84-87`
o mandou para a folga e **nenhum** dia da ata recebe aqueles minutos). Corrigir em `escala/utils.py` e
deixar no teste seria a meia-correcao da sec.6: a prosa certa e a errada convivendo, e quem le a errada
decide por ela.

**O PENDENTE DA FAMILIA `celula/precedencia` SAIU DA LISTA** (`core/juizes.py`), que era o que o
contrato de arvore ja cobrava em VERMELHO -- *"impressao sumiu -- sitio curado? tire de PENDENTES"*, a
regra 2 da familia: a lista so encolhe. **E a lista vazia muda o proprio selo**, o que nao e obvio:
`fora_da_autoridade` isenta o arquivo **INTEIRO** de todo pendente (exclui por `p['arquivo']`), entao
`escala/utils.py` volta a ser varrido pelos tres PROIBIDOS da familia -- conferido **com o pendente
FORA** antes de eu dizer conforme: **0 casamento**. O contador literal do selo foi de 1 a **0**.
**A celula da matriz segue `verde=False`, de proposito**: allowlist zero e UMA das duas condicoes; a
outra -- o numero chegar ao GRAVADO -- so se cumpre na relavratura. Verde agora seria selo falando por
efeito que ainda nao houve, e a nota da celula passa a dizer exatamente isso.

**DONO POR DIVERGENCIA (L-099), escrito antes de qualquer apply**, porque a lei nao deixa tratar o
conjunto como bloco: ESTRUTURA = os 441 do dict de folga (fatia, e esta), os 13 do teto (O194) e os 10
que a L-085 mandou para a folga (O195, **6 deles decididos por menos de UM MINUTO** de distancia do
corte); BATIDA = a cauda orfa do turno da vespera; **CADASTRO = os 50 `sem_celula` + 5 `fora_vinculo`
(55 dias, 473,36 h), e esses e PROIBIDO curar por codigo** -- vao para a lista do admin pelo Cadastro x
Realidade. O que a divisao impede: chamar os 441 de "3.667 h que o sistema deve", e chamar os 10 da
L-085 de `dono=CADASTRO` quando o cadastro esta certo e a ATRIBUICAO do turno nao esta.

A O195 fica **REGISTRADA e PARADA para a 09**: aquela competencia esta exportada, e mexer nela cai na
L-092 / REGEN-EM-EXPORTADA. Para a 10 em diante, a DINHEIRO-EM-COMPETENCIA-ABERTA cobre.

LEI-AKITA: origem=`escala/utils.py` (o ramo de FOLGA do montador, que nunca perguntou o realizado),
testemunha=`ponto/turnos.py::realizado_do_dia` (o juiz unico da pergunta, declarado em `core/juizes.py`),
RED=`logs/o191/red_folga_v2_20261004.out` (3 selos vermelhos, incl. `'realizado=240' not found in
'realizado=0 em dia sem previsao'`) -> `logs/o191/green_folga_c_20261004.out` e
`logs/o191/l103_inversao_20261004.out` (41 OK), quem-mais-le=os 8 leitores do `or 0` censados no
comentario da cura + `bordas_realizado` (guardado) + `espelho.py:752` (independe da grade) + template
(0), juizes novos=0.

## 04/10 19:2x — **DUAS LEIS DE ESTEIRA** NASCEM, TRES GANHAM NUMERO, E O PACOTE EM VOO SE SEPARA NO MESMO TURNO

**O CORTE PEGOU UM ERRO MEU EM CURSO, e nao um erro de ontem.** `RAIA-VERDE-POUSA` (**L-105**) e
`DOCS-NO-MARCO` (**L-106**) entram no `LEIS.md`, e com elas as tres que **ja existiam escritas e sem
numero**: `MERGE DE RAIA CAI E RECARREGA NO MESMO ATO` (CLAUDE.md 2) = **L-107**, `MARCO FECHADO / PUSH UM
POR MARCO` = **L-108**, `DIETA DE PROSA` = **L-109**. Lei escrita sem `L-NNN` e lei que ninguem acha na
hora de cobrar -- as cinco agora tem id, origem, dono e estado, e `test_leis_indice` passa a cobrar as
duas novas pelo id do corte (**77 leis indexadas**).

**OS NUMEROS DELE, RECONFERIDOS AQUI pela casa**, porque numero de lei nova se mede e nao se repete:
no `main` de hoje sao **24 commits so de `app/docs/` em 44** as 19:5x (ele mediu **23/42** as 19:2x -- a
proporcao e a mesma, e os dois que entraram no meio **sao meus**). Raias com commit a frente do `main` ha
mais de 6 h: **9** -- `raia-chamado` 6 commits/12 h (parada desde 07:25, como ele disse), `raia-k8` 1/10 h,
`lps-prova` 1/6 h, `raia-pdf` 4/**124 h**, `tmp-ui` 2/**141 h** e quatro `worktree-agent-*` (17 h, 18 h,
19 h e **224 h**). Isto e MEDICAO para o RELATO, nao o contador do item (3): construir o contador antes da
`CELULA-TURNO-FECHA` e da O145 esta **PROIBIDO** pelo proprio corte, e a lista a mao nao vira instrumento.

**O QUE MUDOU AGORA, dentro do turno:** o pacote de pouso que estava em voo levava K8 + CERT-AST + K5 num
merge so. O CERT-AST e **INSTRUMENTO** -- cria `bin/suite_nucleo.sh`, um selo de host e um paragrafo da
CLAUDE.md 3 --, e pela L-105b nao viaja com produto. Ele saiu: `pousos-produto` = main + `k5-encerrada`
(`02391558`), e o `cert-ast` (`cb84a4ac`, levando `8ffcd44d`) vai a **pouso proprio** logo atras, com
verdito proprio das duas portas. O teto foi **remedido na arvore nova** e deu identico (13 / 8,0,1,4 /
115). A conta do atraso que a lei nomeia e minha: **1 h** de pouso de folha travada por um instrumento no
pacote errado -- e o custo de desfazer foi **um merge refeito e uma suite cheia descartada**, nao um
incidente em prod, porque o merge era numa COPIA.

**O QUE A L-106 MUDA NO JEITO DE FECHAR:** nao ha mais commit so de docs fora de marco. Entao
`LEIS.md` (6 linhas), `CLAUDE.md` 7b, `BACKLOG.md` (marcador + duas celulas), `RELATO.md`, `CORTES.json`
(+2, agora 66, regenerado), `TICKETS.md` e `PROMPTS.md` **viajam no commit deste marco**, junto do codigo
do K8 e do K5 -- nao em sete commits de prosa como eu vinha fazendo hoje. A excecao continua sendo **uma
so**, nomeada: o derivado do `bin/pos_push.sh`.

**O QUE FICA PARA A FILA 2, por ordem dele, e nao se constroi antes:** (3) contador no `ESTADO.md` via
`bin/relato.sh` -- raias a frente do main ha mais de 6 h, com branch, commits, idade e motivo, esperado
**0**; (4) selo de host no pre-push -- push com commit so de `app/docs/` alem do derivado = VERMELHO, com
o caso que **morde nos dois sentidos**; (5) veredito por raia velha (25-30/09 e os `worktree-agent-*` da
madrugada): pousa, ou **morta com certidao**. Apagar worktree e o **`!`** dele, e eu nao apaguei nenhuma --
as quatro de agente e a `tmp-ui`/`raia-pdf` seguem de pe, listadas acima com a idade.
## 04/10 19:0x — O POUSO ENTROU **POR COPIA**, O PACOTE **SE SEPAROU EM VOO**, E O TETO DEU O MESMO NUMERO NAS DUAS ARVORES

Mergeei `k8-t20` (`192aed00`), `cert-ast` (`cb84a4ac`) e `k5-encerrada` (`3dfb8214`) **em `wt-pousos` e
nao na arvore viva** -- e foi essa escolha que deixou a separacao do pacote, meia hora depois, custar um
merge em vez de um incidente. A **L-107** existe por um caso que foi meu: um merge escreve dezenas de
`.py` no bind-mount de uma vez, o worker segue com o codigo de antes, e nos ~11 min entre o merge e o
deploy o lote de cartoes quebrou em prod. Com TRES raias e docs gerados em conflito, fazer isso na arvore
viva seria abrir a mesma janela tres vezes.

Quando a **L-105** chegou (19:2x) e mandou o INSTRUMENTO sair do pacote do PRODUTO, a arvore de pouso foi
**refeita do zero**: `pousos-produto` = main + `k5-encerrada` (`02391558`), sem o `cert-ast` -- provado
pela ausencia de `bin/suite_nucleo.sh` nela. E e aqui que mergear em copia se paga: como o merge foi feito **na copia**, o `main` e o **primeiro pai**
do commit de pouso (`git log -1 --format=%P` da `263535a7 3dfb8214`), entao a arvore viva o recebe por
**fast-forward** -- so escrita de arquivo, zero resolucao -- e o `bin/deploy.sh` vem no MESMO ato, a forma
que a L-107 manda. Sem migration no range (0 arquivos de `migrations/`), entao `--sem-migrate`.

**CORRIJO UMA LINHA QUE EU MESMO ESCREVI AQUI**: eu havia dito que a arvore viva receberia isto como
*merge, nao fast-forward*, e citado `merge-base --is-ancestor main 3dfb8214` = **false** como prova. O
teste esta certo e **responde outra pergunta**: ele compara o main com a **ponta da k5**, nao com o commit
de pouso. Contra o pouso, `merge-base --is-ancestor main pousos-produto` da **SIM**. Fica registrado
porque e o erro classico de medir o vizinho da pergunta em vez da pergunta.

**UM conflito em tres merges**, e ele foi de prosa, nao de codigo: `app/docs/BACKLOG.md`, duas linhas
(`CELULA-TURNO-FECHA` e `PLACAR-ESTRUTURAL`). Resolvido por HEAD porque o main e **estritamente mais
novo** nas duas -- ja cita `be19b202` e o desbloqueio dos passos 5-6 pela lei das 14:xx, que a raia de
manha nao tinha. O numero do placar nao foi copiado de lado nenhum: volta **medido** na regeneracao.

**O TETO SE MEDIU NA ARVORE MERGEADA, nao na raia** -- e era onde podia furar. `cert-ast` nao toca
nenhum dos tres arquivos de teto, e `k5-encerrada` CONTEM a `k8-t20`
(`merge-base --is-ancestor 192aed00`), entao a cadeia **19 -> 15 -> 13** e cadeia e nao concorrencia.
Por AST sobre `PENDENTES` do merge: fechamento **13** / (dinheiro 8, documento 0, cobranca 1, tela 4)
e tela **115** -- exatamente os dois numeros escritos nos contratos. Se tivesse divergido, o conserto
era DENTRO do commit de merge; teto acima do que a lista tem afrouxa a guarda em vez de preserva-la.

**E O TETO SE MEDIU DE NOVO depois da separacao, na arvore que realmente pousa** -- `pousos-produto`, sem
o `cert-ast`: **13** / (8, 0, 1, 4) e tela **115**, **identicos**. E a terceira confirmacao de que o
instrumento nao toca nenhum dos tres arquivos de teto, e e por isso que tira-lo do pacote nao mexeu em
numero nenhum. Remedir era obrigacao, nao zelo: o numero escrito nos contratos tem de sair da arvore que
vai ao ar, nao de uma arvore parecida com ela.

**A SUITE CHEIA DEU VERDE NA ARVORE CONGELADA**: `Ran 9625 tests in 367.041s` / **OK (skipped=42)**,
rc=0, zero FAILED (`logs/suite_pousos_produto_20261004.out`), pela porta unica
`bin/suite.sh --dir /home/ronald/wt-pousos`. Sao **9625** contra os 9618 da raia da manha -- os **7** que
a K5 trouxe. Anoto o que o veredito NAO e: o `grep -E '^(OK|FAILED)'` casou **quatro** linhas, e tres
delas sao PROSA de log (*"1 periodos atualizados"*, *"nenhuma divergencia em 2026-10-04"*, *"OK: 31"*).
O veredito e a linha do `Ran`, que aparece **uma** vez. Ja me custou dar suite por verde com ela correndo.

**O SELO DO CONFORME, E O QUE ELE NAO COBRE.** O corte do MES CIVIL tirou `holerite/matriz.py` e
`lista_ausencias` do `_K8`/`PENDENTES_FECHAMENTO` **como conforme, nunca como curado** -- a impressao
`mes_ou(GET.get('mes'), hoje.month)` continua no arquivo, e o PROIBIDO do CELULA-TURNO-FECHA so nao
morde porque a PERGUNTA nao se aplica aquele sitio. O selo e por AST, com as DUAS pontas: a negativa
(zero dos 6 sinais de fechamento) e a POSITIVA (`Ausencia` ainda referenciada), para nao passar por
ausencia de sinal. **RED medido pelo teste REAL**, nao por sonda replicando o helper -- e essa correcao
importa, porque minha primeira medicao reimplementou o helper e provava a replica, nao o selo:
`_f = FechamentoMensal.objects.none()` dentro da funcao -> `AssertionError: Lists differ: [] !=
['FechamentoMensal']`.
**E o que NAO se mediu ficou escrito no proprio selo**: renomear a funcao nao chega a ele. A linha 49
de `ponto/urls.py` resolve a view por atributo e o import morre antes
(`AttributeError: module 'ponto.views' has no attribute 'lista_ausencias'`). A URLconf e guarda **mais
forte e mais cedo** que a minha, entao o ramo `None` cobre a funcao **movida** para outro modulo, nao o
rename em cima da rota -- quem prova aquele ramo e o caso sintetico. Selo que afirma cobrir o que a
casa ja cobre antes dele e selo com a medida errada de si mesmo.

**O NUCLEO DA MENSAGERIA ENTROU NO PORTAO DO PUSH.** Com o `cert-ast`, `bin/regua.sh` e
`bin/pre-push.sh` passam a chamar `bin/suite_nucleo.sh`: 515 testes que **nao tinham porta nenhuma** --
nao e o buraco dos LABELS (lista digitada em 4 sitios, 24/09) nem o do `holerite` (app fora da lista,
04/09), e o buraco de ANTES deles, porque `mensageria/` nao e app do projeto ponto, e PROJETO Django
irmao. Dos 515, **um estava vermelho desde 16/09 -- 18 dias -- acusando codigo CERTO**, por casar TEXTO
de fonte onde a lei da casa diz ESTRUTURA. Selo vermelho que ninguem roda nao avisa nada, e cobra o
preco de parecer que existe. Por isso o portao deste marco rodou o nucleo **na copia, antes do push**,
e nao o deixou como surpresa no pre-push.

**O VIGIA DA ESTEIRA MEDE CERTO E CONCLUI ERRADO** (registrado, nao curado). O ALARME de 16:30, 17:30 e
18:35 diz *"8 fatia(s) ativa(s) e nenhum .out ha 30 min -- a esteira esta parada e o vigia nao esta
destravando"*. As duas primeiras metades sao verdade; a terceira e inferencia errada: a esteira esta
parada porque **`esteira.pausada` existe desde 26/09, com dono e condicao de saida declarados pelo
Ronald** ("criterio do estrutural fechado + corte Ronald"). O vigia nao le a pausa declarada, entao
chama de "vigia sem efeito" o que e pausa com dono. A cura e ele consultar `bin/pausar.sh`; e item de
BACKLOG, nao desta fatia.

**FICA REGISTRADO, NAO CURADO** (nenhum dos tres e desta fatia): (a) se as duas telas de HE do main
(`gestao_he_pdf:617`, `gestao_he:731`) sao conforme pelo mesmo corte e **pergunta de LEI** -- ela esta
no topo da secao das 18:0x e a esteira segue sem ela; (b) a cura de origem da chave grossa de
`PENDENTES` e um campo `funcao` em `_p` com checagem por AST, porque hoje `fora_da_autoridade` exclui o
**arquivo inteiro** por familia (`test_contract_juiz_celula.py:48`) e foi so por isso que o aval
precisou fixar o sitio; (c) `.ruff_cache` das copias tem arquivo **root** dentro (classe O192) --
gitignored, entao nao entra em commit, mas o `rm -rf` da limpeza falha como `ronald`.


**A PORTA NOVA ACHOU O SEGUNDO SELO PROVADO E MUDO, E O SELO ESTAVA CERTO.** O gate na copia deu
`nucleo_rc=1`: `nucleo.tests.test_prompt_gerado::test_o_arquivo_esta_em_sincronia_com_o_codigo`.
A primeira pergunta nao foi "como fica verde", foi **de quem e o defeito** -- e a resposta e medida: o
MESMO teste fica VERMELHO na arvore **VIVA** (`Ran 3, FAILED failures=1`), entao **nao e do `cert-ast`**;
e o `cert-ast` que, ao dar porta ao nucleo, passou a MOSTRAR um vermelho que ja existia. O primeiro selo
mudo que ele achou (`test_ferramenta_certificacao`) ele proprio curou; este e o segundo. Selo orfao nao
e selo, e porta nova nao cria defeito: ela cobra o que estava calado.

**E ERA DESYNC REAL, nao selo acusando codigo certo.** A ferramenta `contratos_da_pergunta` entrou em
`nucleo/ferramentas.py::contexto_do_chat` e a lista gerada nunca foi refeita -- ela parava na 21 e
renumerava 16 linhas a menos. A cura foi pelo **ESCRITOR UNICO** (`manage.py gerar_prompt_copiloto` sem
flag, que escreve `.md.tmp` e `os.replace`), nunca editando a lista a mao: `8ffcd44d`, 17+/16-. Rodado
como `ronald:ronald` e **nao como root** -- a classe do O192 (92 copias orfas, 2,1 GB de root) nao se
repete por distracao. Veredito depois: **`Ran 520 tests, OK`**, e o proprio teste imprime
`PROMPT_GERADO.md em sincronia`.

**O AVAL DAS 19:1x TINHA UMA CONDICAO, E ELA SE CUMPRIU.** *"Se a falha do nucleo nao fechar em uma
tentativa, o pouso 2/3 (CERT-AST) sai do pacote e vira item proprio."* Fechou em **uma**: uma corrida do
gerador, o diff conferido, commit. Registro o que **nao** conta como tentativa da cura, para a linha nao
ficar generosa comigo: antes dela eu errei o INSTRUMENTO duas vezes -- `KeyError: 'POSTGRES_USER'` por
rodar o container sem o `--env-file` da porta unica, e `rc=127` por chamar `bin/suite_nucleo.sh` da
arvore VIVA, onde ele ainda nao existe (nasceu no `cert-ast`). Erro de instrumento nao e a cura
falhando, mas tambem nao se esconde.

**E A CONCLUSAO DELA FOI VIRADA POR UMA LEI POSTERIOR, nao por mim.** Pela condicao do aval, com a cura
fechando em uma tentativa, **o CERT-AST ficava no pacote** -- e foi o que eu havia concluido. O corte das
19:2x e **posterior** e manda o contrario por um criterio que o aval nao tinha: instrumento nunca viaja
com produto. Registro o custo em vez de apagar o caminho: a suite cheia que estava correndo media uma
arvore que **nao vai pousar**, e eu a **parei** (`TaskStop`) em vez de deixa-la terminar e usar o verde
dela -- verde de arvore que nao e a que sobe nao e verde. Depois a arvore foi refeita e o teto remedido.
O que a lei cobrou de mim nao foi o julgamento do aval: foi ter posto os dois no mesmo pacote.

**OS TRES REGISTROS DO "ITEM EM CURSO" SE MOVERAM NO MESMO ATO** (precedente `31558c3e`): marcador
`ORDEM-VIVA-TOPO` (`BACKLOG.md:7`), celula de estado do item e o topo deste arquivo. O
`PLACAR-ESTRUTURAL` sai do topo porque o **R6 item 3 pousou**, e o 1o ABERTO do bloco OBRAS volta a ser
`CELULA-TURNO-FECHA` -- que e exatamente a ordem do aval das 19:1x (*"Depois: CELULA-TURNO-FECHA pela lei
(a), e O145"*). Quem cobrava isso era `test_hook_nao_cobra_congelado.sh`, VERMELHO com a frase certa:
*"ou a fila andou e o marcador nao foi movido, ou uma linha ficou com estado velho"*. Era a primeira.

**A LEI DO MES CIVIL GANHOU NUMERO: L-104.** O selo `test_leis_indice` estava VERMELHO porque o corte
`HOLERITE-MES-CIVIL` foi registrado sem linha em `LEIS.md`, e ele tem razao: corte sem lei indexada e
corte que ninguem acha depois. O `PROTEGE` dela nao foi preenchido por deducao (a `LEI-PROTEGE-SITIO`
proibe): `holerite/matriz.py::_limites` e **lido** -- `datetime.date(ano, mes, 1)` ao ultimo dia do mes,
chamador unico `universo_por_empresa:89` -- e `ponto/views.py::lista_ausencias` foi medido por AST (toca
`Ausencia`, `Colaborador`, `TypeError`, `ValueError`, e **zero** dos 6 sinais de fechamento).

**O QUE SEGUE PARADO, nomeado:** o defeito dos 4 sitios que montam `"$R/.git/hooks/..."` a mao continua
de pe -- em worktree o `.git` e **arquivo**, nao diretorio, e por isso 6 selos de host dao RED FALSO em
qualquer copia (os dois que acertam, `bin/testes_fora_do_git.sh` e `bin/index_vs_arvore.sh`, usam
`git rev-parse --git-common-dir`). A forma certa **ja existe na casa**: e o caso classico de "qual leitor
nao migrou", e por `CURA-MAIS-RESTRITIVA` nao espera decisao. Vai como item, nao dentro deste pouso.

**E ACHEI UM IRMAO PIOR DELE, ao conferir o verde dos meus proprios selos: RAIZ CRAVADA.**
`bin/tests/test_cortes_registrados.sh:11` e `bin/tests/test_lei_akita.sh` tem `R=/home/ronald/saas-hasner`
**literal**, entao rodando DENTRO da copia eles mediram a **arvore viva** -- o primeiro me disse *"64
cortes"* enquanto a copia tinha **66**, e foi essa discrepancia que denunciou o defeito. O do `.git` da RED
falso; este da **VERDE sobre a arvore errada**, que e a familia do *selo provado e mudo*. Censo: **31
arquivos de `bin/` com a raiz cravada**, dos quais **7 sao selos** (`test_baixa_diferida_host`,
`test_cortes_registrados`, `test_fabricante_backlog`, `test_fabricante_seco`, `test_hook_stop_vivo`,
`test_lei_akita`, `test_sem_arquivo_de_mount`) -- nos daemons da esteira a raiz cravada e legitima, no selo
nao. Por isso **descontei** o verde dos dois e os rodei de novo com a raiz apontada para a copia
(`sed` no fluxo, sem editar arquivo): `lei_akita: OK` sobre o meu CLAUDE.md e `cortes_registrados: OK (66
cortes)`. Os selos que de fato certificaram esta copia resolvem a raiz por `BASH_SOURCE`
(`test_leis_indice`, **77 leis**) ou montam arvore temporaria (`test_hook_nao_cobra_congelado`). E
INSTRUMENTO: pela **L-105b** nao entra neste pouso, vai com o do CERT-AST ou em item proprio.

## 04/10 18:0x — O CORTE DO **MES CIVIL** JA ESTAVA CUMPRIDO NA RAIA, E O TETO DOS DOIS CONTRATOS FICOU **6 ACIMA DO MEDIDO** NAS DUAS

**O CORTE E A RATIFICACAO DE CODIGO QUE JA EXISTE, e so o descobri porque medi antes de construir.**
Ele chegou **duas vezes** no mesmo bloco (contador `prompts_repetidos`; registrado agora em
`PROMPTS.md`, e pela CLAUDE.md 7b ate aqui tinha sido LIDO, nao recebido) e responde a pergunta de LEI
que eu havia posto no topo do RELATO: as duas linhas que sobravam em
`core/juizes.py::PENDENTES['fechamento']` **nao esperavam migracao, estavam MAL ARQUIVADAS**. A
pergunta `_K5` ("qual e a competencia encerrada?") nao e a que `holerite/matriz.py::_limites` responde
-- ele responde *"quem pertence ao universo da competencia"*, e a autoridade disso e o **DOMINIO**, que
paga por mes civil. A `_K8` ("que `FechamentoMensal` e o da competencia de hoje?") nao se aplica a
`lista_ausencias`, que filtra **VIGENCIA** de ausencia (`de`/`ate`, default mes corrente) e nao le
`FechamentoMensal`.

**A RAIA JA TINHA FEITO, COM NUMERO E NAO POR DEDUCAO** (`k5-encerrada`, `6a350aa9`): na **unica**
competencia com holerite publicado (08/2026, **578 documentos em 560 pessoas**), dos admitidos
**21-31/08** que a janela 21-20 excluiria, **16 de 19 TEM** o holerite de 08; dos demitidos **21-31/07**
que a janela INCLUIRIA, **1 de 17**. Universo civil, com numero. A propria nota da raia registra o
quase-erro que a mediu de novo: a primeira medicao parecia MISTA e teria virado **pergunta de lei
FALSA**, porque 07, 09 e 10 dao zero em tudo -- **nao ha holerite publicado nelas**, e ausencia de folha
ia ser lida como regra (SELO ANTI-VACUIDADE, a mesma familia do `[]` de dois sentidos).

**O AVAL QUE VEIO JUNTO EVITOU UM CONFORME FALSO, e o mecanismo esta medido.** A chave do pendente e
`(arquivo, impressao)`, e a exclusao do contrato e **por ARQUIVO INTEIRO por familia**:
`ponto/tests/test_contract_juiz_celula.py:48` monta `declarados = {p['arquivo'] for p in
PENDENTES[fam]}` e salta o arquivo todo -- `_p` (`juizes.py:241`) **nao tem campo de funcao nem de
ancora**. No main a frase `mes = mes_ou(request.GET.get('mes'), hoje.month)` esta em **TRES** funcoes de
`ponto/views.py` (`fechamento_linhas:847`, `fechamento_mensal:879`, `lista_ausencias:1841`), entao tirar
a linha ALI declararia conformes as duas telas de fechamento que **ainda nao migraram**. Na raia a mesma
frase ocorre **UMA vez** (`lista_ausencias:1871`), `_hj.month` **zero** e `POST.get('mes')` **zero** --
medido, nao suposto. Por isso o split que eu ia construir (duas linhas com ancora multi-linha e
aritmetica de teto) **nao se constroi**: o aval o retirou da mesa e a chave fica exata sem ele.
**"Conforme" nao e "curado"**, e e a distincao que a nota tem de carregar: a impressao SEGUE no arquivo,
e o PROIBIDO do `CELULA-TURNO-FECHA` -- *"tirar pendente com a impressao ainda no codigo"* -- so nao
morde porque a **pergunta nao se aplica ao sitio**. Essa e a frase que o corte cobra na segunda metade
(*"a nota dizendo de que o conforme depende"*), na forma do `_A14 CURADO` (`juizes.py` ~301-332).

**O ACHADO DO CAMINHO, e ele nao e da raia k5 sozinha: AS DUAS RAIAS AFROUXARAM O TETO.** Medido agora
pelo contador **AST** (`PENDENTES[fam]` -> 4o argumento posicional de `_p`; o teste faz
`pend = juizes.PENDENTES[FAMILIA]` sem filtro, entao a contagem AST **e** `len(pend)`):

| arvore | `fechamento` | zonas (dinheiro, documento, cobranca, tela) | `tela` |
|---|---|---|---|
| `main` `5d31530f` | 19 | (9, 0, 1, 9) | 117 |
| `wt-k8t` `192aed00` | **15** | (8, 0, 1, **6**) | 117 |
| `wt-k5` `6a350aa9` | **13** | (8, 0, 1, **4**) | **115** |
| teto escrito nos dois contratos | **19** | **(9, 0, 1, 9)** | **123** |

As duas raias tiraram linha (k8-t20 quatro, k5 mais duas) e **nenhuma baixou o teto** -- e o comentario
do proprio teste condena isso em letra: *"o teto e o valor MEDIDO hoje, nao um numero maior -- teto
acima do que a lista tem **afrouxa a guarda** em vez de preserva-la"*. Nao ha escritor canonico para
descer: `ajustar_contadores`/`_desce_total`/`_desce_grupo` foram **removidos** no O11 (01/10 12:3x) e
`core/tests/test_selo_baixa_diferida.py:181-186` fica VERMELHO se voltarem (*"o contador e derivado, nao
descido"*) -- entao o teto se escreve **a mao**, com a anotacao `# <FATIA> (<data>): -N`, e e isso que a
k5 passa a fazer pelas duas, porque ela pousa por ultimo e carrega o numero final. **`wt-k5` contem
`192aed00`** (conferido por `merge-base --is-ancestor`), entao 19 -> 15 -> 13 e uma cadeia, nao duas
edicoes concorrentes do mesmo tuple: o 13 e verdade de pouso, nao numero de raia.

**E HA FOLGA QUE NAO E DESTA FATIA, e ela fica REGISTRADA em vez de absorvida**: na familia `tela` o
main mede **117** contra teto **123** -- **6 de folga preexistem**, de fatia que nao e a k5 (ela tira
2, de 117 para 115). Baixar para 115 na raia cura o numero; o que NAO se pode e deixar a anotacao
sugerir que a k5 tirou 8. A anotacao nomeia os dois donos.

**O QUE ISTO NAO FEZ**: nenhuma linha de `app/core/juizes.py` do **main** foi tocada -- o aval e literal
(*"Nao e para declarar conforme as duas telas no main"*) --, e nada foi aplicado. E fica uma pergunta de
LEI no topo, nao uma decisao minha: a linha 16 de `PENDENTES['fechamento']` tem a impressao `_hj.month`,
que no main esta em **tres sitios** (`gestao_he_pdf:617`, `gestao_he:731`, `painel_fechamento:780`) --
**as duas telas de HE sao conformes pela mesma razao do holerite** (quem pergunta o mes ali nao e leitor
de `FechamentoMensal`), **ou** sao leitoras que faltam migrar? Pela
`PAREI-DE-LEI-NAO-DEVOLVE-TURNO` isso **nao devolve o turno**: vai para a mesa com o numero e a esteira
segue. E a cura de ORIGEM da chave grossa -- um campo `funcao` em `_p` com conferencia por AST, para a
exclusao deixar de ser por arquivo inteiro -- e **item de BACKLOG, nao desta fatia**.

**O145 + O146, REGISTRADOS E NAO ADIANTADOS.** O aval de ~18:0x corrige o *"mesma fatia"* de 03/10
19:2x: **fila unica, dois pousos** -- (1) frota por `bin/sonda_frota.sh`, (2) a **O145 pousa sozinha**
com RED e DIFF de frota = zero sem decisao gravada, (3) a **O146** depois, com o `!` do motor. A
granularidade e o ponto: amarradas no mesmo pouso, o bug **PROVADO** da O145 esperava por cadastro que
ainda nao existe. **O marcador `ORDEM-VIVA-TOPO` NAO se moveu**, e a condicao dele e literal --
*"assim que a CELULA-TURNO-FECHA fechar ou parar"* --, que segue ABERTA com os passos 5-6 desbloqueados
pela lei das ~14:xx (trabalho = **O191**). Mover um dos tres registros do "item em curso" hoje deixaria
`test_hook_nao_cobra_congelado.sh:107` VERMELHO, que e arvore vermelha e nao adiantamento.

## 04/10 17:2x — A LAPIDE DAS COPIAS ORFAS NAO E PASSADO: **1,2 GB EM 31 COPIAS, 20 COM ROOT** (O192)

`LEI-AKITA: origem=bin/arvore_do_push.sh:66 (a lista de montagem, de UM projeto so) + a montagem do nucleo sem PYTHONDONTWRITEBYTECODE, testemunha=find -user root -type f na copia e o rc do rm -rf como usuario, RED=copia dos dois projetos onde o ruff e o teste do nucleo correram -> 0 arquivo de root, quem-mais-le=--dir da suite, bin/pre-push.sh:137, bin/vigia_arvore.sh e a porta nova do nucleo, juizes novos=0`

Fui limpar `wt-teto20` depois do pouso e o `rm -rf` como `ronald` **falhou** -- `Permission denied` em
`.ruff_cache`, exatamente a familia da lapide de 03/10 (`bb008cee`: 92 copias orfas, 2,1 GB, 58.229
entradas de root). A cura daquele dia pousou -- `bin/arvore_do_push.sh --montagem` e a porta unica, e o
selo existe --, e **mesmo assim ha arquivo de root nascido hoje**. Medido:
- `wt-teto20`: **19 entradas de root**, entre elas `mensageria/.ruff_cache` e `mensageria/nucleo/__pycache__`;
- acervo que restou: **31 copias, 1,2 GB, 20 com entrada de root**;
- a entrada de root **mais nova** de varias delas e de HOJE: `wt-k5` 16:48, `wt-cert` 16:03, `wt-k8t` 15:09.

**SAO DOIS SITIOS, e a distincao importa porque um deles eu teria errado de olho.** Dir de root **vazio**
e ponto de montagem de tmpfs -- inofensivo, e e o que a cura de 03/10 produz. O que acusa e **arquivo** de
root. Separados assim:
1. **bytecode.** `bin/pre-push.sh:137` monta `"$ARVORE_PUSH/app":/app` com `PYTHONDONTWRITEBYTECODE=1`, e
   e por isso que `app/` nao tem `__pycache__` de root em nenhuma copia. A montagem do **nucleo** -- nova,
   do `cert-ast` -- nao leva a mesma variavel, e `wt-cert/mensageria/nucleo/__pycache__` de root (16:03) e
   a consequencia direta.
2. **tmpfs que nao chegou.** `wt-k5/app/.ruff_cache/` tem `CACHEDIR.TAG` e `0.16.4/...` de root escritos
   **hoje 16:48**. Isso so acontece se aquele run **nao recebeu** `--tmpfs /app/.ruff_cache`: existe
   mounter que nao consulta a porta unica.

**ONDE A CURA PERTENCE:** ao pouso do `cert-ast` (`cb84a4ac`), nao a uma fatia depois -- ele e quem torna a
copia dos dois projetos, entao a lista de montagem se conserta no MESMO ato. E ha um detalhe que eu nao
transformo em acusacao: a mensagem do `cb84a4ac` afirma *"`find -user root` deu 0 antes e 0 depois, e o `rm
-rf` como ronald devolveu rc=0"*, e eu **nao contradigo a medicao** -- aquela corrida nao escreveu cache
dentro de `mensageria/`. O que esta errado nao e o numero: e a LISTA, que segue de um projeto so e faz a
prova depender de qual ferramenta correu naquele minuto. **Ausencia de sinal lida como sinal bom**, que e o
SELO ANTI-VACUIDADE visto do lado da montagem.

Limpo agora, com `sudo -n` e so em cache de COPIA: as 5 `/tmp/prepush-arvore.*` (26/09 a 04/10, 167 MB) e a
`wt-teto20` (39 MB) -- **206 MB**. A `wt-teto20` nao era worktree do git (nao tem `.git`; e extracao de
`archive`), e o branch `teto20` = `fb1c78ac` segue no repo principal e e **ancestral do HEAD**, entao nada
se perdeu. Conferido depois de remover: `app/staticfiles` VIVO com as 26 entradas, intacto. As outras 30
copias FICAM -- varias guardam patch nao pousado (`wt-ct` o do CELULA-TURNO-FECHA, `wt-sj` o do passo 5) e
os diffs das sujas estao em `logs/copias_sujas/`; varrer isso e item, nao improviso de limpeza.

## 04/10 16:59 — O135 TETO-20 **NO AR** (`fb1c78ac`): O PORTAO SE ATRAVESSOU PELA PORTA QUE JA EXISTIA

PROVA: `logs/deploy.stamp::COMMIT` = `8c3035bc`, do qual `fb1c78ac` e `48805bbf` sao ancestrais
(`git merge-base --is-ancestor` rc=0); o conteudo no ar le-se em `app/core/contratos_estruturais.py:351`.

`LEI-AKITA: origem=bin/deploy.sh:147 (SEM_JANELA_AUTH_MOTIVO, a saida JA IMPLEMENTADA), testemunha=bin/janela_auth.sh rodada DA COPIA + carimbo da sombra do dia, RED=fatias_agendadas/o135-teto20/esteira.out das 15:33 (BLOQUEADO, rc=1) contra o das 16:59 (rc=0), quem-mais-le=os 4 chamadores de janela_auth (deploy.sh, a GUARDA 3 da esteira, o selo, o proprio header), juizes novos=0`

**O `!` dele**, 14:4x, confirmado as 16:3x: *"forca a janela_auth para o O135 hoje -- a mudanca em
`app/api/urls.py` e UMA LINHA de rota read-only, zero logica de auth -- e poe o ff e o `deploy.sh
--sem-migrate` num ato so"*, e o aval de 16:4x que escolheu o COMO: *"o O135 sobe AGORA pela saida que ja
existe (`SEM_JANELA_AUTH_MOTIVO` com o motivo do meu `!`); o `--forcar` do O185 vira fatia propria
depois."*

**A PREMISSA DELE FOI CONFERIDA, NAO SUPOSTA.** `git diff main teto20 -- app/api/urls.py` = **um** `path()`
para `mensageria/contratos-estruturais/`, **+2 linhas fisicas**, read-only. E isso importa porque o aval e
LITERAL (LEI-AKITA 9): a GUARDA 3 agora **mede antes de atravessar** e recusa em dois casos que o `!` nao
cobre -- (i) se a guarda falhou por **ESTRUTURA** (lista de sitios ausente ou vazia) em vez de por janela,
*"isso nao se forca"*; (ii) se a guarda nomear **qualquer outro** sitio de auth alem de `app/api/urls.py`,
*"fora do aval, nada tocado"*. As duas linhas sao `grep` na saida da guarda, nao confianca no meu
julgamento de 16:4x.

**POR QUE NAO SE CONSTRUIU O `--forcar` PRIMEIRO.** `bin/janela_auth.sh` ANUNCIA `--forcar "<motivo>"` no
header (linhas 16-17) e na propria mensagem de BLOQUEADO (linha 94), e **so parseia `--janela`** (linha
59): com `BASE="${1:-origin/main}"`, um `--forcar` seria lido como **ref de git**, cairia em *"base
'--forcar' nao existe -- trato como se auth tivesse mudado"* e sairia **1 de novo**. A flag anunciada faz o
CONTRARIO do que promete -- e isso e o **O185**, fatia propria por ordem do mesmo aval. Implementa-la aqui
seria abrir a **segunda** saida para a mesma excecao, e duas saidas sao dois escritores dela (LEI-AKITA 4:
a lei existe, o leitor que nao migrou era a linha da GUARDA 3). Havia tambem uma trava de ORDEM que
fechava a porta sozinha: cura commitada em `main` quebra a GUARDA 2 (`git merge-base --is-ancestor main
teto20`) e o `--ff-only`; commitada em `teto20` quebra a GUARDA 1 (sha movido + diff fora de `app/docs/`).
Curar primeiro exigiria afrouxar o meu proprio portao.

**O POUSO, em UM ato** (`fatias_agendadas/o135-teto20/esteira.out`, rc=0):
- GUARDA 1 OK -- a suite julgou `48805bbf`, pousou `fb1c78ac`, **so `app/docs/` no meio**; `Ran 9605 tests
  in 1150.022s -- OK (skipped=42)`, `FAIL:/ERROR: = 0`;
- GUARDA 3 -- `janela_auth: BLOQUEADO -- fim de semana (dow=7)`, `sitio(s) de auth que mudaram:
  app/api/urls.py` -> **atravessada por ato**, com o motivo escrito tambem no log do proprio `deploy.sh`;
- `arvore_sem_conflito: OK` · `migrations pendentes no schema do cliente: 0` · `sombra: carimbo
  dia=20261004 status=OK tipo=completa diverge=0 erros=0` (as duas ultimas o run bloqueado nunca alcancou);
- `Updating 31558c3e..fb1c78ac / Fast-forward` -- **20 arquivos, 595 insercoes, 56 delecoes** (inclui
  `app/api/urls.py` +2, `app/core/arquitetura_leitura.py` novo, `mensageria/nucleo/core_client.py` +5,
  `mensageria/nucleo/ferramentas.py` +36);
- `deploy SEM NADA NO MEIO -- 16:59:02` -> `16:59:13`, rc=0. `prova de casca: 16 estaticos conferidos, 5
  paginas compiladas, 599 rotas importadas em 2 urlconf(s)`; **3 cascas provadas** (core `/health/` 200, ui
  `/colaboradores/` 302, mensageria `/health/` 200); `selo BUG 128: verde`; `importerror_500=0`.
O stash de `app/docs/` voltou DEPOIS do deploy (`Auto-merging BACKLOG.md / RELATO.md`), que e a ordem que o
MERGE-DE-RAIA-CAI-E-RECARREGA cobra: nada entre o ff e o deploy.

**A LINHA MEDIDA NO AR, pela cadeia REAL**: `arquitetura: 13 de 20`, `faltam 7`,
`familias_sem_cadastro=[escala, chamado]`, `declaradas=17`, `fonte=core/contratos_estruturais.py::verdes/total/MATRIZ`,
`autoridade_do_teto=core/configuracao_efeito.py::familias_com_parametro`.
PROVA: a MESMA linha veio pelas DUAS vias -- o `core_client` chamado direto e a cadeia do
`nucleo/ferramentas.py::contratos_da_pergunta` --, as duas devolvendo `arquitetura: 13 de 20` e `faltam 7`.
O smoke chamou **tambem** o
`core_client` direto, e nao por capricho: `nucleo/ferramentas.py::contratos_da_pergunta` engole qualquer
excecao em `{'erro': 'core_indisponivel'}`, entao sozinha ela **nao distingue "no ar" de "quebrada e
abafada"** -- a resposta boa pela cadeia inteira poderia ser silencio bem embalado. (O modelo do cliente e
`nucleo.models.AtivacaoCliente`, nao `Cliente`: o smoke morreu uma vez no import antes de eu ler o
arquivo.) O teto virou **PERGUNTA** -- `total()` deriva de `familias_com_parametro` e a constante `TOTAL`
morreu.

**O AGENDADO DE SEGUNDA FOI DESARMADO, COM TRILHA** (`logs/deploy_agendado/o135-teto20.desarmado`;
`deploys_agendados=0`). Ele nao era inofensivo: segunda 06:05 as quatro guardas passariam **VACUAS** --
`teto20 == main`, o ff responderia "already up to date", e a copia nao veria mais sitio de auth mudado
contra `origin/main` -- e o `bin/deploy.sh --sem-migrate` publicaria a **arvore viva daquele minuto**, sem
ninguem olhando. Guarda que passa por ausencia de sinal e a familia do SELO ANTI-VACUIDADE, do lado do
deploy.

**O PORTAO ERA DE ACESSO, NAO DE DINHEIRO** -- por isso a L-009 nao se aplicava e o que faltava era o
`!` do dow. Bookkeeping no mesmo commit (sem commit so de docs): `CORTES.json` TETO-20 -> `no ar
fb1c78ac`, o aval `O135-JANELA-AUTH-NO-DOMINGO` -> `respondido` com `respondido_em`, `AVAIS.md` **7 -> 6**,
`CORTES.md` (63 cortes, 36 nao no ar, `cortes_recebidos_sem_fatia=0`), celula do BACKLOG, `PROMPTS.md`,
`LEIS.md` (L-102 emendada + **L-103** nascida).

## 04/10 16:4x — A PORTA DO NUCLEO EXISTE (`cert-ast` `cb84a4ac`), E A COPIA DO PUSH PASSOU A SER DOS DOIS PROJETOS

A secao de baixo mediu o buraco; esta o fecha. **`bin/suite_nucleo.sh`**: source do `recursos.sh`,
`$TESTE_DOCKER` (cpuset 4-7), pela `trava_teste.sh` com `ESTEIRA_QUEM=suite-nucleo`, senha so pelo
`--env-file` 600, usuario lido da AUTORIDADE (o proprio `juliani_db_test`, nao um literal), `-v
<arvore>/mensageria:/srv`, `rc 75` repassado. **Porta separada, nao flag do `bin/suite.sh`**: outra imagem,
outro ponto de montagem, outro settings e nenhuma `--montagem` (o nucleo nao renderiza pagina, nao ha
staticfiles para faltar) -- enfiar isso na porta irma daria dois caminhos dentro de uma porta que existe
para ter um. **515 OK rc=0** pela porta, arvore viva da raia.

**A PECA QUE NINGUEM OLHAVA.** `mensageria/config/settings.py` tinha `'HOST': 'db'` **cravado duas linhas
abaixo de um `NAME` que lia o env** -- o caminho natural fazia o `test_mensageria` nascer DENTRO do
`saas_db`, o postgres do cliente, nos vCPU 0-3 que a lei do CPUSET reserva para producao. A chave
`POSTGRES_HOST` **ja estava no env daquele container, com ZERO leitores**: parametro nem consumido nem
rotulado "sem efeito", que e o contrato 3 da matriz. Consumi-la e a cura; inventar um `MSG_DB_HOST` ao lado
seria o segundo nome para a mesma pergunta. **No-op em prod** (`.env:6` ja diz `db`), e o `deploy.sh` nao
reinicia a mensageria -- o `.py` entra naquele container no recreate dele. Com isso o `--add-host` da
medicao morreu junto com a medicao. **PROVADO NA CONEXAO REAL**, nao suposto: a sonda conectou em
`172.18.0.7` (`juliani_db_test`) e falhou so por o banco nao-teste `mensageria` nao existir la -- que e o
certo, porque a suite usa `test_mensageria`. O `saas_db` (`172.18.0.5`) nao e tocado.

**DOIS CHAMADORES, UMA PORTA, e o segundo nao e redundancia.** A regua chama depois do `node_check` e antes
da suite do ponto (1,5 s: vermelho barato se paga antes dos 19 min). O **pre-push chama tambem** porque ele
**nao le o carimbo da regua para decidir veredito** -- roda as SUAS suites contra `$ARVORE_PUSH`, a copia do
sha (O56). Com chamador so na regua, um nucleo vermelho passaria no push **sempre que o atalho "JA VERDE"
nao casasse, que e exatamente quando o codigo mudou**. Uma porta com dois chamadores nao e o defeito dos
LABELS em quatro lugares: aquele era uma LISTA duplicada. Na regua o `rc` se le por `|| _rc=$?` e nunca por
`if ! cmd; then _rc=$?`: ali o `$?` e o status da NEGACAO (0) e o `rc 75` nunca seria reconhecido -- a
armadilha que o `pre-push.sh` ja documenta duas vezes, por ter pago por ela.

**E A COPIA DO PUSH NAO TINHA O NUCLEO.** `bin/arvore_do_push.sh:83` era `git archive "$COMMIT" app`: o
chamador do pre-push nao teria o que montar em `/srv`. **Alarguei o escritor unico** em vez de o pre-push
fazer archive proprio -- segundo escritor de copia e o que vazou 2,1 GB em 92 copias orfas (03/10). E
**`--igual` passa a ver `mensageria/*.py`**, o que fecha um furo que a porta abriria: a 1a condicao do
atalho JA VERDE e o hash da arvore inteira (que ve a mensageria), mas o `--igual` responde OUTRA pergunta --
*"a arvore E o commit?"* --, e sem essa linha uma regua rodada DEPOIS de eu editar `mensageria/` carimbava a
arvore suja, o hash casava, o `--igual` olhava so `app/` e dizia IGUAL: arsenal PULADO e push com nucleo que
ninguem rodou.

**OS SELOS, com caso que MORDE.** `bin/tests/test_nucleo_tem_porta.sh` (novo) afirma porta `+x`, os DOIS
chamadores e a copia com `mensageria/` -- e o 4o caso prova que **o detector morde**: diz sim a uma chamada
quebrada em `\` e **NAO** a uma que virou comentario, que e a forma mais provavel de a chamada morrer.
Contra a arvore viva ele da VERMELHO nos tres primeiros (RED evidenciado). Construir a porta sem selar os
chamadores seria repor o buraco com um arquivo a mais dentro -- *"selo orfao nao e selo"* com outro nome.
`test_prepush_testa_o_commit.sh`: o repo de brinquedo ganha os **dois** projetos, porque o CONTRATO passou a
ser dos dois (fixture no contrato velho e fixture que apodrece), mais o par do `--igual` pelo nucleo e um
caso **1b** que prova a **RECUSA**: commit sem `mensageria/` nao gera copia meia-boca. A alternativa --
*"materializa se existir"* -- era o band-aid: a copia sairia sem o nucleo e o push ficaria vermelho **por
falta de arquivo em vez de por codigo**, ou alguem afrouxaria o `--dir` e os 515 voltariam a ser
invisiveis, que e o defeito de ORIGEM deste item. Contra o extrator velho esse selo da VERMELHO em 3.

**MORDIDA DA PORTA, medida:** com a linha de producao `dados.update(certificacao_da_pergunta(...))` removida
numa COPIA, a porta devolve **rc=1 e 3 FAILs** e o `--dir` alcanca a copia -- a arvore viva intacta.
Varredura dos 64 selos de host da raia: **os 6 vermelhos sao artefato de worktree** (sem `.git/hooks`, sem
`logs/deploy.stamp`, sem `staticfiles`, sem `settings.json`) e estao VERDES na arvore viva, conferido um por
um. **A regua cheia e a prova do POUSO, nao desta raia**: rodar 19 min contra uma arvore que nao tem a
mudanca nao diria nada sobre ela. Nada pousa em `main` antes do O135 -- a GUARDA 2 exige `main` ancestral de
`teto20`.

**TRES COISAS QUE EU TINHA DADO POR BOAS E NAO ESTAVAM MEDIDAS** (curadas antes de pousar, commit emendado
`1702028d` -> `cb84a4ac`). **(1) "no-op em prod" estava medido em METADE.** Eu li o `POSTGRES_HOST` (`.env:6`
= `db`) e escrevi no-op para a linha inteira -- mas a linha tambem passou a consumir `POSTGRES_PORT`, que eu
nunca li. Medido agora: `docker exec mensageria printenv` da **HOST=db** e **PORT=5432**, e o `.env` traz as
duas iguais (o `5433` do compose e mapeamento de porta de HOST, na diretiva `ports:`, nao esta chave) -- a
palavra se sustenta, mas com dois numeros em vez de um. Meia medicao sustentando a palavra "no-op" e a forma
de 25/09 com outra coluna: o container quebraria no proximo recreate dele, calado, dias depois.
**(2) A testemunha do pre-push RECALCULAVA** (LEI-AKITA 2). O chamador jogava a saida no `/dev/null` e, no
vermelho, **rodava a suite de novo** para ter o que imprimir: segunda vez na trava, segundo banco de teste
criado e destruido, e as linhas impressas vindas de **uma medicao que nao e a que decidiu o rc** -- e, se a
segunda tentativa pegasse `rc 75`, **vermelho MUDO**. Agora grava em `$_saida_nucleo` e imprime DALI, com o
rabo cru quando nao ha linha de unittest para casar. O idioma certo morava **40 linhas acima**, no
`_teste_irmao`: a casa ja tinha a forma e o chamador novo nao a usou.
**(3) A copia nao orfana, e agora isso e prova e nao esperanca.** A porta do nucleo nao recebe os `--tmpfs`
que a `--montagem` entrega a porta do ponto, entao um unico diretorio de root dentro da copia faria o
`trap 'rm -rf "$ARVORE_PUSH"'` do pre-push falhar **calado** e orfanar copia em `/tmp` -- a conta de **2,1 GB
em 92 copias** de 03/10. Medido: `--dir` sobre uma copia do `arvore_do_push`, `find -user root` = **0 antes e
0 depois**, `rm -rf` como ronald rc=0. Quem paga e o `PYTHONDONTWRITEBYTECODE=1` da porta mais a ausencia de
ruff/hypothesis/mypy no nucleo -- as duas razoes ficam escritas, porque se alguem acrescentar ruff ao nucleo
a conta muda e o lugar de pedir tmpfs e o escritor unico da montagem, nunca um `-v` a mao.
REGISTRADO, nao construido: `bin/vigia_arvore.sh` passa a receber `mensageria/` na copia (ele consome a mesma
porta unica) mas **nao chama a suite do nucleo** -- o vigia periodico segue cego a ela. Vai como item, nao
como obra de hoje.


## 04/10 15:5x — OS 515 TESTES DO NUCLEO NAO TINHAM PORTA, E UM DELES ESTAVA **VERMELHO DESDE 16/09**

O K8 PRIMEIRO, porque ele fecha. A raia `k8-t20` reconciliou com o `teto20` (`192aed00`): as duas
pontas tinham escrito a MESMA celula do BACKLOG, e resolvi por **quem mediu depois**, nao por lado --
`CELULA-TURNO-FECHA` fica a do `teto20`, que e a que ganhou a palavra **aguardando** em `31558c3e` (a
palavra que `hook_stop_fila1.py:162` declara que le); `PLACAR-ESTRUTURAL` fica a desta raia, que
carrega a medicao das 15:0x. As duas conferidas contra a DIETA no ato: **287 e 296** caracteres.
Selo de duas shas em arquivo PROPRIO (`logs/k8t_suite_verde`, nunca o do O135 -- sao dois pousos e
duas suites): `SHA_SUITE=49b264f4`, `SHA_LAND=192aed00`, **9618 testes OK rc=0**, e
`git diff --name-only` entre as duas **fora de `app/docs/` = VAZIO**. O `grep` do nome do teste novo no
log da **0** e isso NAO e ausencia -- a suite nao imprime nome sem `-v`. A prova e a CONTAGEM:
9605 -> 9618 = **+13**, e o arquivo novo tem **exatamente 13** funcoes `test*` por AST.

O CENSO. `mensageria/nucleo/tests/` tem **62 arquivos** e **515 testes**, e **nenhum runner de `bin/`
os chama**. Nao e esquecimento de cadastro: `mensageria` nao e app do Django do ponto -- tem
`manage.py` e `config` proprios -- e `bin/suite.sh` monta so `$RAIZ/app` com
`config.settings.ci`. A porta simplesmente nao existe, e isso e' diferente de "o app ficou fora do
LABELS" (o buraco do `holerite` em 04/09 e do `pautas` em 24/09): la a lista tinha quatro copias, aqui
a lista nao alcanca o projeto.

ONDE O CENSO RODOU, e o ponto importa. `mensageria/config/settings.py:37` **crava** `'HOST': 'db'` --
duas linhas acima do `MSG_DB_NAME`, que ja le o env. Pelo caminho natural o banco de teste nasceria
**dentro do `saas_db`**, o postgres do cliente, nos vCPU 0-3: exatamente o que a lei do CPUSET proibe.
Entao o censo rodou com `db` **redirecionado em runtime** (`--add-host`), pela trava e no cpuset de
teste, e o redirecionamento foi PROVADO antes de soltar 513 testes: `db -> 172.18.0.7`
(`juliani_db_test`), **nao** o `172.18.0.5` do `saas_db`. O `settings.py:37` e o sitio que a fatia cura
na origem -- e ele tem vizinho: **`POSTGRES_HOST` esta no env do container `mensageria` com ZERO
leitores** (`grep` em `mensageria/` = nenhum), parametro nem consumido nem rotulado "sem efeito", que
e o contrato 3 da matriz. A cura e CONSUMIR a chave que ja existe, nao inventar uma segunda.

**513 testes, 1 VERMELHO** (`logs/censo_nucleo_20261004.out:47`). O vermelho e
`test_ferramenta_certificacao.test_MORDE_o_contexto_liga_o_bloco_junto_da_prontidao`, e ele acusava
**codigo CERTO**: afirmava a ADJACENCIA LITERAL de duas linhas do fonte, e `como_esta_a_fabrica` e
`de_onde_vem_a_jornada` nasceram ENTRE elas depois de 16/09. `certificacao_da_pergunta` segue sendo
chamada em `contexto_do_chat`, incondicional, no degrau da prontidao -- tudo o que o nome do selo
sempre declarou. **SEXTA** vez que a casa paga TEXTO onde a lei e ESTRUTURA. Mover a linha de producao
para a palavra do selo passar seria curar pela palavra; curei o selo (`cert-ast`, `a13ec7da`).

A CURA NAO AFROUXOU O SELO, e esse era o risco real -- "afrouxar selo" esta no PROIBIDO do
ESPINHA-ANTES-DA-UI. A adjacencia garantia, **por acidente** da indentacao de 4 espacos, que a chamada
e INCONDICIONAL, e e isso que o docstring de `contexto_do_chat` declara temer ("sem roteador por
palavra-chave"). Um `ast.walk` solto aceitaria a chamada dentro de um `if`. Entao a varredura so olha
statements do corpo que NAO estao em desvio, e o caso que MORDE sai da **PRODUCAO**, nao de fixture:
`como_esta_a_fila` esta no mesmo corpo, dentro de `if not _estrategia`, e por isso tem de ficar de
FORA. Dois valores reais com status diferentes. Prova adversarial contra o `ferramentas.py` REAL, em
TERCEIRA copia (nunca na arvore que a medicao monta): intacta **7 OK**; chamada REMOVIDA **FAILED (2)**;
chamada DENTRO de um `if` **FAILED (2)**. Depois da cura, o censo inteiro: **515, OK, rc=0**.

O QUE FICA DE PE. A PORTA. Enquanto `bin/suite_nucleo.sh` nao existir, um selo vermelho fica invisivel
por **18 dias**, que foi o caso -- e a origem da invisibilidade e a porta que falta, nao o selo.
Registrada como `SUITE-DO-NUCLEO-ENTRA-NA-REGUA` no FIM do bloco OBRAS, de proposito: o hook continua
cobrando `PLACAR-ESTRUTURAL` (conferido chamando `_proximo_da_fila()`, nao por leitura). Nada disto
pousa em main hoje -- a **GUARDA 2** do O135 exige `main` ancestral de `teto20`, entao a raia espera o
`!` ou o agendado de segunda 06:05.

## 04/10 15:2x — O HOOK COBRAVA UM ITEM QUE **ESPERA LEI DELE**, E O TETO SO NAO VIROU ARMADILHA PORQUE A SUITE ESTA RODANDO

O `hook_stop_fila1` devolveu `siga: CELULA-TURNO-FECHA` tres turnos seguidos. Nao era defeito dele: o
item esta **ABERTO** e era o 1o aberto do bloco OBRAS. O defeito era **meu registro**. Os passos 1-4
fecharam com prova; os passos 5-6 dependem da **LEI dele** que esta no topo deste arquivo com os numeros
(`88` sem par, `15` com numero, `73` com zero) -- e `PAREI-DE-LEI-NAO-DEVOLVE-TURNO` manda exatamente
isto: *"a esteira SEGUE o proximo item da ORDEM VIVA que nao depende dela"*. A esteira seguiu (R6 item 3,
raia `k8-t20`), e **o BACKLOG nao dizia**. Tres registros respondendo "qual o item em curso" e so um
deles atualizado e a mesma meia-correcao que eu acabei de curar no TETO-20 -- por isso os tres se moveram
NO MESMO ATO: marcador `ORDEM-VIVA-TOPO`, celula de estado do item e o topo deste arquivo.

**A palavra nao foi escolhida para calar o hook -- ela e a que o hook declara ler.**
`bin/hook_stop_fila1.py:162` salta a celula cuja palavra diz que o item nao anda, e o comentario da
propria linha nomeia este caso: *"congelado pela L-096, fila 2, ou **esperando decisao do Ronald**"*. Uma
LEI dele e uma decisao dele. A celula segue `**ABERTA**` com todos os numeros; nada foi carimbado, nada
foi renomeado para fechar celula (o PROIBIDO do aval), e o patch segue no chao em `wt-ct`, sem commit.
E nao era o marcador que o segurava: `_proximo_da_fila()` le o **1o item ABERTO do bloco OBRAS** e
**nunca** consulta o `ORDEM-VIVA-TOPO` -- mover so o marcador nao teria mudado o veredito dele. Mover os
dois era obrigacao e nao arrumacao: `bin/tests/test_hook_nao_cobra_congelado.sh:107` fica VERMELHO se o
marcador e a resposta do hook discordarem. Os 5 selos de host que leem estes arquivos: VERDES.

**O TETO JA ESTAVA ESTOURADO, e isso e o achado que importa.** `logs/hook_stop_fila1.json` dizia
`blocks: 7` contra `TETO = 5`. O que impediu a liberacao falsa (`PAREI: hook-teto`, com a fila 1 de pe)
nao foi sorte: foi a clausula HOOK-TETO-NAO-CONTA-ESPERA, e eu **medi** que ela funciona em vez de supor
-- `_tem_trabalho_em_curso()` procura `manage.py test` no argv, e a suite cheia do `k8-t20` esta no `ps`
com `--cpuset-cpus 4-7`, pela trava. `trabalho em curso: True`. Se a suite tivesse sido lancada por um
`docker run` a mao, fora da trava e fora do argv que ele le, o 8o bloqueio teria encerrado o turno com o
O135 e o k8 em pe -- e e por isso que a forma da suite virou ARQUIVO em 05:5x (O182).

**O VERMELHO DA REGUA ERA MEU, E JA ESTAVA CURADO.** `logs/regua_lps_20261004.out` (13:45) morreu em
`test_cortes_registrados.sh`: *"corte envelhecido FORA da pausa declarada (o tripwire mordeu):
TETO-20-SEM-FAMILIA-SEM-CADASTRO"*. O corte envelhecia porque o **estado** dele estava STALE em
`recebido` com a obra pronta -- a mesma meia-correcao de 03/10 05:0x. O `50333326` o moveu para
`esperando "!"`, e o selo agora responde `cortes_registrados: OK (63 cortes; 0 sem fatia)`, com o alarme
dos outros 12 nao-fatal pela pausa com dono. Um Vermelho de segunda a menos, curado na origem.

**A GUARDA DE STASH DA ESTEIRA PASSOU A OLHAR O SHA.** No ensaio do pouso com a sujeira nova
(`stash push -- app/docs/` -> `merge --ff-only teto20` -> `pop`) o **pop deu rc=0**, sem conflito, com as
3 modificacoes de docs de volta e HEAD em `088fdcde`. Mas o ensaio mostrou outra coisa: `refs/stash` e do
**REPO**, nao do worktree, e ja havia um `stash@{0}` velho de `d21f59f6`. O `SUJO=0` da esteira ja
protegia o caso "nada para stashear" (ela nao popa o que nao empilhou) -- mas `pop` tira sempre o `{0}`,
entao uma raia que empilhasse um stash entre o push e o pop faria a esteira restaurar arquivo ALHEIO na
arvore viva, as 06:05, sem ninguem olhando. Agora ela guarda o sha da propria entrada e `_popar()`
**recusa** se o topo nao for ela, dizendo como recuperar. Recusar e mais restritivo que popar as cegas
(CURA-MAIS-RESTRITIVA). A seco, agora: guardas 1 e 2 passam, para na 3
(`janela_auth: BLOQUEADO -- fim de semana (dow=7)`), **HEAD e stash intactos**.
## 04/10 15:0x — R6 ITEM 3: A PERGUNTA K8 FECHA **4 DE 6**, E O DIAGRAMA ERA O **TERCEIRO** LEITOR

**O que pousa na raia** (`k8-t20`, construida SOBRE o `teto20` de proposito -- nada pode pousar em
`main` antes do O135, que a GUARDA 2 do agendado exige ancestral): o merge de `be19b202` + a
remedicao + o placar + esta linha. **Nao vai a `main` hoje.**

**Medido na COPIA MESCLADA, pelas funcoes reais** (`core.juizes.fora_de_autoridade` e
`core.contratos_estruturais.verdes`, container irmao no cpuset de teste, log
`logs/placar_estrutural/k8_fechamento_20261004.txt`):

| | antes | depois |
|---|---|---|
| `fora_de_autoridade('fechamento')` | 19 | **15** |
| deles de zona `dinheiro` | 9 | **8** |
| em `ponto/views.py` | 11 | **7** |
| pergunta K8 (*"que FechamentoMensal e o da competencia de hoje?"*) | 6 | **2** |
| `contratos_estruturais.verdes()` | 13 de 20 | **13 de 20** |

**O PLACAR NAO MOVEU, e dizer isso e parte do trabalho.** A celula `folha/export x juiz` pede
allowlist **ZERO** e sobram 15 pendentes: migrar leitor derruba o CONTADOR, nunca a celula. Quem
ler "K8 fechou" como +1 no placar vai procurar um ponto que nao existe -- por isso a celula do
BACKLOG diz `K8 fechou 4 de 6` e nunca `K8 fechou`.

**OS 2 QUE FICAM NAO SAO RESTO, SAO OUTRA CLASSE.** `_pa(mes, ano, 2)` em `ponto/views.py` e
`ponto/services/fechamento.py` nao le mes civil: e ENVELOPE declarado sobre os cortes [2..28], com a
janela por empresa resolvida DENTRO do laco. Fecha-los e outra pergunta, nao a continuacao desta.

**O ACHADO QUE A FATIA NAO PREVIU: o diagrama era o TERCEIRO consumidor do contador.** O censo do
`be19b202` nomeava dois -- `core/juizes.py` (a lista) e `ponto/tests/test_contract_juiz_fechamento.py`.
A suite recortada (`--only "ponto core"`, 4151 testes) devolveu **2 falhas**, as duas em
`core/tests/test_selo_diagrama_do_codigo.py`: `docs/ARQUITETURA.mmd` carrega
`jf_fechamento["fechamento<br/>6 com juiz · 2 sem juiz<br/>registro: 19 sitio(s)"]` -- o desenho LE
`PENDENTES` e envelheceu no mesmo ato. Foi o selo B9 (*"o diagrama nao envelhece"*) fazendo
exatamente o que nasceu para fazer. Cura: regenerar da FONTE (`gerar_diagrama`), `registro: 19` ->
`registro: 15`, uma linha, no mesmo commit da medicao.

**E A ARMADILHA DE HOST QUE ISSO EXPOS**, que vale mais que o conserto: `bin/gerar_diagrama.py` e
`docker exec saas_core python manage.py gerar_diagrama` (linha 15). Rodado do host contra a COPIA,
ele regenerou o diagrama de **PROD** -- escreveu na arvore viva, saiu identico, e me devolveu "sem
mudanca" como se a copia estivesse em dia. Diagnostico certo so apareceu ao chamar o gerador DENTRO
do container montado na copia. Atalho de host que nao recebe `--dir` responde sobre a arvore errada;
e a mesma familia do `janela_auth.sh` de 14:5x, com o lado oposto -- la o script seguia a sua propria
raiz, aqui ele segue uma raiz FIXA. Arvore viva conferida depois: intacta.
## 04/10 14:5x — O POUSO DO O135 FICA AGENDADO, E A GUARDA QUE EU TINHA ESCRITO ERA **VACUA**

**O que esta montado:** `fatias_agendadas/o135-teto20/esteira.sh` (GATE TEMPORAL = CRON + ARQUIVO,
`bin/deploy_agendado.sh agendar o135-teto20 "05 06 05 10"`, cron.d presente), que as **segunda 06:05**
faz stash do que o vigia apendeu -> `git merge --ff-only teto20` -> `bin/deploy.sh --sem-migrate` SEM
NADA NO MEIO -> pop. O ff e o deploy no MESMO ato por lei (merge e recarga sao um so); o commit fica
na COPIA ate la de proposito.

**A GUARDA 3 QUE EU ESCREVI PRIMEIRO NAO GUARDAVA NADA, e isso e MEDIDO, nao suspeita.** Ela chamava
`bash bin/janela_auth.sh origin/main` da **arvore viva** -- e a arvore viva nao tem o commit, entao o
diff dela contra `origin/main` nao contem `app/api/urls.py`. No mesmo minuto, 04/10 14:5x:

| de onde se pergunta | resposta |
|---|---|
| `/home/ronald/saas-hasner/bin/janela_auth.sh origin/main` | `OK -- nenhum sitio de auth mudou` (**rc=0**) |
| `/home/ronald/wt-teto20/bin/janela_auth.sh origin/main` | `BLOQUEADO -- fim de semana (dow=7)`, sitio `app/api/urls.py` (**rc=1**) |

O juiz e o MESMO e as respostas sao OPOSTAS, porque `janela_auth.sh:19` deriva a `RAIZ` do proprio
`BASH_SOURCE` e diffa o HEAD **de onde ele mora**. Guarda que passa sempre e pior que guarda nenhuma:
o portao de verdade e o do `deploy.sh:147`, que roda **depois** do ff -- se ele recusasse as 06:05, a
janela de 30/09 ja estaria aberta e nao havia desfazer legal (`git checkout` em arquivo que prod usa
e `!`). Cura: perguntar ao MESMO juiz, de dentro da copia. **Nao** replicar dow/hora no script, que
seria juiz paralelo (LEI-AKITA 2 e 8). Provado com o relogio costurado: `JANELA_AUTH_DOW=1
JANELA_AUTH_HHMM=0605` da `OK -- auth mudou (app/api/urls.py) e estamos em janela permitida`.
Os outros portoes tambem passaram a ser medidos ANTES do ff: `deploy.sh --conferir` (rc=0, migrations
pendentes 0) e `sombra.sh --conferir` (rc=0, `dia=20261004 completa diverge=0`).

**E O POP CONFLITAVA.** Ensaio num worktree: `main` + a sujeira de docs de hoje -> stash ->
`merge --ff-only teto20` -> pop = **CONFLITO em `RELATO.md` e `AVAIS.md`**, `pop rc=1`, *"The stash
entry is kept"*. Causa: `48805bbf` mexe nesses arquivos e o registro de hoje mexeu nas MESMAS regioes
(o bloco do `!` cai na linha 31, dentro da zona de hunk dele). Resolver conflito as 06:05 sem ninguem
olhando, com o `.py` novo ja no bind-mount, e a janela de 30/09 com uma mao amarrada. Cura de origem,
nos dois lados: o registro virou commit na arvore viva (`50333326`) e a copia o INCORPOROU por merge
resolvido na COPIA (`62ebc615`), com os gerados refeitos pelos escritores canonicos. Reensaio com a
unica sujeira que sobra para segunda (dois apendes de vigia na cauda): `pop rc=0`, sem conflito, com
o apende preservado e o `.py` do O135 no lugar.

**O selo da suite deixou de ser uma frase:** `logs/o135_suite_verde` nomeia `SHA_SUITE` (o commit que
a suite julgou) e `SHA_TETO20` (o que vai pousar), e a GUARDA 1 **reconfere** que `teto20` esta nesse
sha e que nada fora de `app/docs/` andou entre os dois -- em vez de confiar no numero escrito. Selo
que so repete um valor envelhece calado, que e o defeito que acabei de corrigir no CORTES.json.
Ensaio da esteira inteira hoje: para na GUARDA 3 com `-- parado`, **HEAD intacto e stash intacto**.

**O `esteira.sh` e o `msg_commit.txt` ficam UNTRACKED de proposito** ate o pouso: `fatias_agendadas/`
e versionada (`abono-no-ar/esteira.sh` esta no git), mas commitar na arvore viva agora faria `main`
deixar de ser ancestral de `teto20` e **quebraria o proprio ff** da GUARDA 2. Eles entram no commit de
arrumacao, depois que o marco pousar.

## 04/10 14:1x — O135 TETO-20: O TETO DA MATRIZ VIROU PERGUNTA, E A CONSTANTE `TOTAL` MORREU

**A LEI, literal** (L-100, corte `TETO-20-SEM-FAMILIA-SEM-CADASTRO`, 03/10 08:13, opcao (b)):
*"o TOTAL **desconta familia sem campo editavel**, e a meta passa a ser **20 de 20**. As celulas de
parametro de CHAMADO e de ESCALA so voltam a existir quando a familia ganhar cadastro de verdade."*

**POR QUE ELA FURA A FILA**: a regua estava VERMELHA e a suite nem rodava (log em
`logs/regua_lps_20261004.out`) -- o tripwire do `test_cortes_registrados.sh` mordeu este corte com **29 h
em `recebido`**, FORA da pausa declarada. Sem ele nao saia push nenhum, nem o do LEI-PROTEGE-SITIO.
Recusei as duas curas rapidas pela lapide do proprio selo (*"a pausa nao cresce sozinha"*) e pela lei do
proprio arquivo (celula ausente conta CONTRA o total): engordar o `COBRE=` seria silenciar o vigia, e
derivar o TOTAL das celulas que faltam na `MATRIZ` seria fazer o numerador escolher o denominador.

**O QUE O TETO ERA**: `TOTAL = len(FAMILIAS) * len(CONTRATOS) + 1   # 22`, uma CONSTANTE ao lado da
matriz -- e o comentario `# 22` sobreviveu **20 dias** ao teto real (a O124 tirou a celula de `escala`
em 03/10 e o 22 continuou escrito). Dois pontos estavam permanentemente fora de alcance: as celulas
`(chamado, parametro)` e `(escala, parametro)` sao PROIBIDAS de existir pela lei de 13/09 (*"familia sem
campo editavel nao tem celula"*, cobrada por `test_MORDE_celula_do_contrato_3_so_e_verde_sem_campo_sem_efeito`),
e celula que nao pode existir nunca fica verde.

**O QUE O TETO E AGORA**: uma PERGUNTA com UM juiz. `core/contratos_estruturais.py::total()` =
`7x3+1 - len(familias_sem_cadastro())`, e `familias_sem_cadastro()` **deriva** da mesma autoridade que ja
decide se a celula pode existir -- `core/configuracao_efeito.py::familias_com_parametro`. Escrever a lista
a mao seria a segunda verdade que a LEI-AKITA proibe, e e a mesma licao que a lapide do
`configuracao_efeito.py` ja guardava. **Nao ha mais constante `TOTAL`**, e o selo cobra isso por
`assertFalse(hasattr(ce, 'TOTAL'))`.

**RED PRIMEIRO** (`logs/o135_red_20261004.out`): contra o HEAD, 3 `AttributeError` em 14 testes --
`familias_sem_cadastro` e `total` nao existiam. **GREEN**: `logs/o135_green2_20261004.out`, **71 testes
rc=0** (matriz, os dois goldens do HAIKU, o placar do TICKETS, os contratos de configuracao e de
tabuleiro, os dois de api). No caminho, **o vizinho mordeu e estava certo**:
`core/tests/test_selo_placar_tickets.py:65` afirmava o literal `'/22'` e ficou VERMELHO contra codigo
CERTO -- passou a PERGUNTAR `ce.total()` (lapide "selo que copia valor fica vermelho", de novo).

**MEDIDO PELA FUNCAO REAL** (`logs/placar_estrutural/contratos_20261004_teto20.txt`, LEI-AKITA 8):
`linha_do_placar()` = **`contratos_estruturais: 13/20 verdes`** · `verdes()` 13 · `declaradas()` 17 ·
`total()` 20 · `familias_sem_cadastro()` `('escala', 'chamado')` · aritmetica `7x3+1 = 22, menos 2
celula(s) PROIBIDA(s) = 20`. As **7 que faltam** estao nomeadas uma a uma na prova, as 13 verdes com o
teste de cada, e as 2 proibidas com o motivo. **Os 7 que faltam agora sao 7 de verdade** -- antes, 2
deles eram impossiveis.

**A LINHA HAIKU, ponta a ponta** (o terceiro sitio que o BACKLOG nomeia, e o `ESPINHA-ANTES-DA-UI`
tambem a pedia): leitor novo `core/arquitetura_leitura.py` com **ZERO derivacao** -- faz tres perguntas
ao juiz (`verdes()`, `total()`, `familias_sem_cadastro()`) e itera a `MATRIZ`; rota
`/api/mensageria/contratos-estruturais/` (`api/views_mensageria.py` + `api/urls.py`, com `_token_ok`);
`mensageria/nucleo/core_client.py::contratos_estruturais` (casca fina, *"nenhuma regra aqui"*);
`ferramentas.py` com `_PEDE_CONTRATOS`, a ferramenta e o bloco ligado no `contexto_do_chat`. Rotulo
**`arquitetura: 13 de 20`**, golden *"quantos contratos estruturais faltam?"*, e a resposta diz **QUAIS**
faltam, com o teste de cada e quantas excecoes declara. **SEM HORA, e isso e declarado**: ela le CODIGO,
nao tabela lavrada, entao responde `sem_lavratura` em vez de fabricar um carimbo -- selo
`test_a_resposta_nao_fabrica_hora`.

**OS CASOS QUE MORDEM** (SELO ANTI-VACUIDADE): `test_MORDE_familia_que_GANHA_cadastro_devolve_o_ponto_ao_teto`
poe um `TipoChamado.prazo_arquivo_dias` CONSUMIDO por mock e o teto sobe para **21** sozinho, voltando a
20 quando o mock sai -- e a prova de que a lei "volta a existir quando a familia ganhar cadastro" e
MECANICA, nao promessa; `test_MORDE_o_rotulo_nao_crava_o_teto` faz o mesmo pelo lado do HAIKU; e
`test_MORDE_o_teto_nao_mora_aqui` varre o corpo da ferramenta da mensageria **por AST** e exige ZERO
literal inteiro. Esse ultimo nasceu VERMELHO por casar a string `'20'` no DOCSTRING que explica por que
o teto e 20 -- a **sexta** vez que esta casa paga por selo que varre TEXTO em vez de arvore; a historia
ficou no docstring do proprio teste.

**OS 4 LEITORES MIGRADOS, no MESMO commit** (a regra do `placar_estrutural.py`: *"quem atualiza e quem
mede, no MESMO commit"*): `core/placar_tickets.py` (a linha do placar e o paragrafo da matriz, que agora
NOMEIA as familias sem cadastro), `core/placar_estrutural.py` (R6), o topo do `app/docs/TICKETS.md`
(`13/20`, por `bin/tickets_placar.sh --escrever`) e a secao 4b do `CLAUDE.md`, que guarda a aritmetica
`7x3+1 = 22` como criterio de 13/09 e diz, logo abaixo, que o TETO e 20 e se PERGUNTA.

**RESPONDIDO DE CARONA, e e o que impede um chat novo de perguntar de novo**: o pendente
`teto-da-matriz-do-estrutural-e-21` (de 25/09, com as tres saidas (a)/(b)/(c)) estava em `sem-motivo` --
ele agora e `respondido` com a L-100 citada, pelo escritor canonico (`bin/gerar_avais.py --escrever`;
respondidos 39 -> 40, abertos seguem 6). A **(c)** nao morreu: o O35 CHAMADO-GANHA-CADASTRO segue vivo,
so deixou de ser pre-requisito do teto.

## 04/10 13:3x — O189: OS 73 DE ATA ZERO, MEDIDOS COM A LINHA 131 DISTINGUINDO `None` DE 0

**O aval (literal):** *"antes de eu responder a lei da L-102, mede na sombra, 09 e 10: com
`supra_juiz.py:131` distinguindo None de 0, quantos dos 73 dia-colab de ata zero trocam de veredito e
quantos colabs passam a ENTRAR ou a SAIR do TXT na relavratura. Publica ao lado dos 15. Nao aplica nada."*
Medido na sombra (dump de 04/10), emp 2/3/4, competencias 09 e 10 ate **03/10** -- o cartorio nao julga o
dia de hoje. **Nada aplicado**: a copia do codigo morreu com a medicao.

**A RESPOSTA, nos numeros que ele pediu:**
- **73 de ata zero -> 73 de 73 trocam de veredito.** E unanime, e isso esta PROVADO pelo lado de fora: a
  secao 5 da sonda existia para listar os que NAO trocam e saiu **vazia**.
- **15 com numero -> 6 de 15 trocam**, todos perdendo `FURO_PARCIAL`, veredito `furo -> concorde`.
- **TXT, relavrando pela porta so as 79 celulas desses 88 dia-colab: 2 colaboradores ENTRAM, 0 SAEM.**
  Os dois sao do grupo dos 15 -- **col302** (emp2, comp 09) e **col250** (emp2, comp 10) --, os dois
  saindo de `('fora', 'furo_espelho')` para `entra`. **Dos 73, nenhum move o TXT**, porque 71 deles
  seguem `furo` por outro codigo e os 2 que ficam `concorde` tem retencao em outro dia da competencia.
- "0 SAEM" nao e sorte, e estrutura: a linha nova so pode **tirar** codigo (`None == 0` e' False nos tres
  ramos de igualdade, os dois ramos de comparacao estao guardados por `real is not None` e o 215 nao
  muda porque `bool(None) == bool(0)`), e retencao no TXT so nasce de codigo PRESENTE.

**O ACHADO QUE VALE A LEI -- o que os 73 PERDEM:** os 73 perdem, todos, o `BATIDA_ORFA_FORA_TOLERANCIA`.
O ramo e `ponto/supra_juiz.py:290`: `if tipo_dia == 'trabalho' and prev and real == 0:` e, dentro,
`if _orfas: ... 'batida existe e nao foi pareada'`. Com `real = None` a condicao falha e **a orfa fica
MUDA** -- exatamente nos dias cuja definicao e "ata zero E pelo menos uma orfa lavrada". Em 71 o dia
continua `furo` pelo `FURO_PARCIAL`; em 2 (col174 23/09 e col469) o dia perde **toda** a acusacao e vira
`concorde`. *"Orfa segue visivel"* e lei da casa, citada no proprio `bordas_realizado.py::_teve_batida_na_ata`
e na guarda do Art.62. Entao a 131 minima troca um **zero falso** por um **silencio sobre batida que
existe**: o ramo 290 nao depende do numero para acusar, depende de `_orfas`. Isto nao e proposta de
desenho -- a lei e dele --, e o numero ao lado da pergunta: **73 orfas que hoje aparecem e passariam a
nao aparecer**.

**AO LADO DOS 15, como o aval pediu.** O DIFF de 11:07 mediu os MESMOS 15 no mundo em que a palavra entra
e a 131 fica como esta (`or 0` colapsando `None` em 0): `+ REALIZADO_ZERO_COM_TURNO` em 15 dias (30 nas
duas raias somadas), `- FURO_PARCIAL 14`, `+ BATIDA_ORFA_FORA_TOLERANCIA 6`, veredito indo para
**discordante**, e **10 passando a RETER** no TXT (`logs/o134/diff_cobranca_20261004.out`). Com a 131
distinguindo, os mesmos 15 andam para o **lado oposto**: 6 `furo -> concorde` e **2 entrando** no TXT. As
duas direcoes sao contrarias porque a pergunta e a mesma: sem a 131, a palavra chega ao juiz como "zero
realizado com turno" e ele **acusa**; com a 131, chega como "nao sei" e ele **cala**. A escolha entre
acusar e calar e' a lei que esta na mesa.

**O UNIVERSO, DITO POR INTEIRO (e a correcao de uma deriva minha).** A primeira versao da sonda chamou de
"os 73" todo dia que carrega a marca `realizado_sem_turno` com ata zero, e isso deu **2.893** -- quarenta
vezes o numero publicado. O universo dos 88 nao e "dia sem par": e o contador `realizado_sem_turno` de
`ponto/services/bordas_realizado.py`, que predica dia **PASSADO e COM BATIDA**. Com o predicado na forma
de `98d9861e` (`real > 0 or orfas`), a sonda reproduz **15 + 73 = 88** exatamente, e o grupo so fecha
porque a pergunta e feita a' autoridade, nao a' marca. Os outros **2.820** dias sem par **nunca estiveram
no contador**: sao dia sem par e sem batida lavrada (falta pura) ou ata agregada, e por isso entram aqui
como grupo SEPARADO -- contexto, nunca "os 73".

**A FRONTEIRA QUE A LEI DECIDE, com numero.** Se o `None` do montador alcancar tambem o dia **sem batida
lavrada** (os 2.820), **864 dia-colab** trocam de veredito -- `- PREVISTO_COM_DIA_INDEFINIDO 521`,
`- FURO_COM_COBRANCA_MORTA 206`, `- FURO_PRE_ADOCAO 132`, com 521 `indefinida -> concorde` e 341
`furo -> concorde` -- e **79 colaboradores entram no TXT** (0 saem). Dito em uma linha: **falta pura
deixaria de ser furo**, porque o juiz leria "nao sei" onde hoje le "zero". O `0 calado` da L-102 viraria
um `None` calado, do outro lado. Nao escolho a fronteira -- ela e lei --, mas ela tem numero agora.

**O ARREIO (por que isto e medicao, e nao suposicao):**
- a copia e' `wt-sj` = HEAD `e0ad39dc` com **tres** linhas trocadas em `ponto/supra_juiz.py` -- a 131 sem
  o `or 0` e `real is not None` nos dois unicos ramos que COMPARAM `real` com numero (247
  `COBRANCA_SEM_FURO`, 275 `REALIZADO_INFLADO`; sem a guarda levantam TypeError). Guardar os seis seria
  mudanca de comportamento que o aval nao pediu;
- os dois bracos saem da **funcao real** no mesmo run e sobre o mesmo dia; a sonda carrega o
  `classificar_dia` do **HEAD** por caminho (`logs/sombra/supra_juiz_head_20261004.py`) e o compara em
  TODO dia julgado: **`DIVERGE_DO_HEAD = 0`** em **23.556** dias. Para entrada numerica a copia e o HEAD;
- **`ja_chega_None_hoje = 9633`**: ja hoje 9.633 dias chegam ao juiz com `minutos_realizados=None`, e com
  `DIVERGE_DO_HEAD = 0` isso diz que **a 131 sozinha e neutra no dado de hoje** -- nenhum desses dias muda
  de codigo, porque os ramos que consomem `real` estao atras de `prev` e `tipo_dia == 'trabalho'`;
- o braco DEPOIS supoe o contrato do **passo 5** (o dia chega com `minutos_realizados=None`), e esse
  contrato tem RED em arvore, nao em prosa:
  `escala/tests/test_montador_realizado_pela_autoridade.py::test_MORDE_347_duas_celulas_casadas_e_zero_minuto_ainda_e_dia_sem_turno`
  -> `AssertionError: 0 is not None`;
- o TXT nao foi recalculado por conta propria: a sonda **relavra pela PORTA** (`CelulaDia.lavrar_veredito`,
  escritor unico P13) dentro de `transaction.atomic()` com `raise` no fim, e le `folha/export.py::
  classificar_export` antes e dentro. Rollback **provado** por impressao digital dos dias tocados:
  `ab6f2230cfbc` antes -> `84f97cb22e92` dentro -> `ab6f2230cfbc` depois. `relavradas=79
  puladas_pela_porta=0 sem_celula=0`, e a anti-vacuidade "movedor fora dos colabs que trocam de veredito"
  deu **0**. Banco LATERAL (sombra), container sem rede;
- **nada foi aplicado.** Nenhuma escrita sobreviveu a medicao, nem na sombra.

**LIMITE DECLARADO, E ELE E UM ACHADO.** O grupo que o predicado LAVRADO do passo 3 acrescenta (os 30 de
celula casada com zero minuto, que levam o contador de 88 a 118) **nao se separa dentro do juiz**, e o
motivo foi MEDIDO em vez de suposto: `n_ok` **nao viaja** no dia que `classificar_dia` recebe -- **2.908 de
2.908** dias sem par chegaram **sem a chave** (`n_ok AUSENTE 2908`, `n_ok None 0`, `n_ok > 0` 0, `n_ok == 0`
0). Sao DOIS montadores: o contador das bordas le o dia de `escala/services/leitor_celula.py::
grade_da_celula`, que escreve `n_ok=n_ok_lavrado(ata)` (linha 359, T-CONTAGEM-UNICA); o juiz recebe o dia de
`escala/utils.py::montar_grade_prevista_periodo` (`ponto/services/cartorio.py:452`), que **nunca** escreve
essa chave -- `grep "'n_ok'" escala/utils.py` = **0** (`'orfas'` esta la, linhas 457-504, e e por isso que os
73 fecham exatos). Entao `ponto/supra_juiz.py:137` cai SEMPRE no fallback e reconta
`len(_celulas_ok(dia['celulas']))`. O comentario da T-CONTAGEM-UNICA -- *"recontar e' FALLBACK para quem
ainda nao foi portado"* -- esta certo, e o nao-portado e **o caminho do cartorio**, que e o unico que JULGA.
Isso nao muda a resposta do aval (os 73 e os 15 fecham exatos, e o recount da o mesmo numero nos dias
medidos), mas fica DITO: o grupo dos 30 deu zero por **chave ausente**, nao por medicao. Fila, nao deste
aval e nao toquei: portar o montador do cartorio para levar o lavrado, ou declarar o recount como a fonte.

Saidas: `logs/o134/o189_ata_zero_v3_20261004.out` (sonda `logs/sombra/o189_ata_zero_v3_20261004.py`,
runner `logs/o134/roda_o189_v3_20261004.sh`); a v1 e a v2 ficam ao lado, com a deriva de universo visivel.

## 04/10 13:xx — LEI-PROTEGE-SITIO: O INDICE DE LEIS PASSA A MORDER NO DIFF DO PUSH

**O corte (literal):** *"Toda lei do LEIS.md que proibe ou condiciona mexer num `arquivo::funcao`
declara esse sitio numa coluna PROTEGE; commit que toca sitio protegido cita o `L-NNN` no corpo,
senao o pre-push fica VERMELHO. Molde: TRAVA JUIZ-NOVO."*
**NASCEU MEDIDA, e o caso e meu:** a **L-102** (`app/docs/LEIS.md:84`) dizia no rodape que trocar
`escala/utils.py::minutos_realizados_do_dia` por 0 *"segue esperando lei dele"* -- e as **10:14** o
bloco mandou apagar a funcao e **o patch foi montado**. A lei estava escrita e **ninguem a leu --
nem o chat, nem a sessao**. Indice que so se le quando alguem se lembra de ler nao e guarda: e
documentacao.

**1. A COLUNA (item 1 do MUDA).** `app/docs/LEIS.md` ganhou **PROTEGE** como 5a coluna, depois de
DONO, nas **70** leis -- e nao e o mesmo que DONO: *dono e quem APLICA a lei, protegido e o que nao
se troca sem a lei na mao*. Preenchida em **1**: a L-102, com
`escala/utils.py::minutos_realizados_do_dia`. **As outras 69 ficam vazias por MEDICAO, nao por
preguica**: li a frase das 70 e nenhuma outra NOMEIA sitio que ela proiba tocar -- e o proprio corte
PROIBE *"preencher PROTEGE por deducao"*. Celula vazia quer dizer *"esta lei nao protege codigo"*,
nunca *"ninguem olhou"*, e isso esta escrito no cabecalho do indice.

**2. O SELO (item 2), com os quatro casos rodados de verdade.** `bin/tests/test_lei_protege_sitio.sh`
(molde `bin/tests/test_juiz_novo_tem_corte.sh`, selo de host, sem Django). Toque medido por **AST**:
`ast` le `git show <ref>:<arquivo>` nos DOIS lados do range, pega o intervalo de linhas da funcao
(com decorador) e cruza com as **faixas dos hunks** do `git diff -U0 -M`; `D`/`R` no `--name-status`
tambem e toque. Citacao = `L-NNN` em `git log --format='%s%n%b' BASE..HEAD`, com a fronteira que
distingue `L-102` de `L-1020`. Em branch de ensaio (`lps-prova`, commit descartavel), com
`BASE=origin/main`:
- **(a)** comentario DENTRO de `minutos_realizados_do_dia`, mensagem calada -> **RED**: *"o push TOCA
  sitio protegido pela L-102 ... e NENHUM commit do range cita `L-102`"*;
- **(b)** o MESMO toque citando `L-102` -> **OK** (`tocados neste push: 1 (L-102, citados)`);
- **(c)** comentario em `minutos_previstos_do_dia` -- **outra funcao do MESMO arquivo de ~1.300
  linhas** --, mensagem calada -> **OK**, `tocados: 0`. E o caso que prova que a granularidade e a
  FUNCAO: selo por arquivo teria ficado vermelho aqui e, de tao barulhento, viraria allowlist;
- **(d)** a funcao **APAGADA** -- o cenario literal das 10:14 --, mensagem calada -> **RED**.
O juiz e **funcao pura** (`veredito(tocados, citadas)`), e por isso o caso que MORDE e **injetado**
dentro do selo, nos dois sentidos: cego nao bloqueia = falha, bloquear quem CITA = falha. Mais a
guarda estrutural *"coluna PROTEGE vazia nas 70 leis"* = falha, que e o anti-vacuidade de verdade
deste selo -- sem ela ele passaria verde para sempre no dia em que a coluna esvaziasse.

**ONDE ELE E CHAMADO, e por que nao foi "onde todos moram"** (decisao TECNICA, registrada pela
PAREI-SO-LEI, sem devolver pergunta): o loop de `bin/regua.sh:138` roda a pasta `bin/tests/` inteira,
mas **o `bin/pre-push.sh` nao roda essa pasta** -- ele chama guardas NOMEADAS uma a uma e, com o
carimbo da regua verde para a mesma arvore, **pula o arsenal inteiro**. Selo que julga o **PUSH** se
chama no push: a linha entrou em `bin/pre-push.sh:16`, ao lado do `regua_tickets.sh`, que e o vizinho
natural (mesma base `origin/main`, mesma pergunta "o que este range cita?").

**3. A LINHA DO CLAUDE.md (item 3).** Secao 6, **LEI ANTES DO PATCH**: *grep do ARQUIVO e da FUNCAO
em `LEIS.md`, `DOSSIES.md` e `CORTES.md` antes de montar patch ou prompt; o resultado vai CITADO com
o `L-NNN`*. Ela fica junto da LER ANTES DE AFIRMAR, que e a mesma familia de erro: narrar/mexer sem
ler ao vivo.

**4. O SELO DA FONTE MIGROU NO MESMO ATO.** `bin/tests/test_leis_indice.sh` lia as colunas por
POSICAO; a 7a coluna deslocaria os indices e a checagem #2 ficaria vermelha nas 69 leis com PROTEGE
vazio. Migrado para `NF!=9` + PROTEGE como **a unica** celula que pode ficar vazia, e **provado que
morde**: apaguei a celula da L-001 -> `RED: lei(s) com coluna obrigatoria vazia ou sem a celula
PROTEGE: L-001`; de volta -> `70 leis indexadas`. O `NF<8` de antes aceitava 8 ou 9 colunas sem
reclamar de nenhuma -- a migracao fechou isso tambem.

**ITENS IRMAOS, so REGISTRADOS (o corte manda, e eu nao construi):** **O187** MAPA-QUEM-LE (o gerador
`bin/gerar_mapa_juizes.py` passa a listar, por campo de ata/fechamento, quem LE -- hoje so diz quem
escreve; medido: `minutos_realizados` tem **9 leitores em 5 familias**, e isso so apareceu em segunda
passada) e **O188** SELO-DE-PENDENTE-POR-CHAMADA (o selo de PENDENTES casa **frase literal** em
`core/tests/test_contract_juiz_celula.py` ~76-82 e no do turno ~29-35, e aceita RENOMEAR como cura;
trocar por varredura de CHAMADA, por AST). Portao dos dois: **fila 2, depois do estrutural** --
*"PROIBIDO construir A ou B antes da matriz fechar"*.

**ACHADO QUE E DELE, NAO MEU DE CURAR:** a zona inviolavel mais antiga da casa --
*"`ponto/motor_calculo_v2.py`: so com aval explicito do Ronald"* (CLAUDE.md secao 4) -- **nao tem
linha no LEIS.md**, entao o sitio mais protegido do sistema **nao pode entrar na coluna PROTEGE**.
Escrever uma linha `L-NNN` para ele seria **lei inventada**, que o cabecalho do proprio indice e este
corte proibem. Fica nomeado aqui: se ele quiser o motor coberto pelo selo, a lei precisa nascer com
numero no CORTES.md.

**O QUE NAO ENTROU, dito por escrito:** a **LINHA HAIKU** (contador *"leis com sitio protegido N"* no
payload, golden *"que lei protege este arquivo?"*, degrau de leitura) **nao esta no PRONTO** do corte
e **nao foi construida** -- fica como fila do HAIKU, nao como divida silenciosa desta fatia.
`LEI-AKITA: origem=LEIS.md, testemunha=diff do push, RED=item 2, quem-mais-le=pre-push, juizes novos=0`

## 04/10 12:xx — ADENDO AO PASSO 5: OS TRES ITENS MEDIDOS, E O PATCH FICA NO CHAO

**O adendo (12:xx, literal):** *"A L-102 manda o dia com ata e sem par dizer 'Sem turno pareado (ata Xh)'
com o numero da ata, 'nem se grava 0, nem se cala' -- e o numero da ata E a soma que o passo 5 apaga. Antes
de pousar o patch de `escala/utils.py`: (1) publica a remedicao do passo 1 (total e classes A/B/C); (2)
prova por RED que, depois do patch, o dia sem par CONTINUA com a palavra e o numero nos tres leitores e que
`classificar_dia` NAO passa a dar REALIZADO_ZERO_COM_TURNO nem BATIDA_ORFA_FORA_TOLERANCIA por causa dele;
(3) DIFF de `classificar_export` 09 e 10: quem entra no TXT antes == depois. [...] Renomear variavel ou
mover a funcao para a impressao sumir = PROIBIDO."*

**O ADENDO CHEGOU EM TEMPO, e e por isso que nao ha nada a desfazer em prod**: o patch inteiro vivia em
`/home/ronald/wt-ct`, sem commit e sem deploy (LEI-AKITA 10). O que eu havia construido e que o adendo
**reprova**, dito com nome: eu tinha tirado o parentese `(ata Xh)` de
`ponto/services/dia_decidido.py::palavra_do_dia` **e enfraquecido o selo da propria L-102** para casar com
ele (`relatorios/tests/test_palavra_do_dia.py::test_11`, que passava a cobrar `'Sem turno pareado'` sem
numero). Isso e exatamente a familia que o adendo proibe -- mexer no leitor para a impressao sumir --, e o
selo era a guarda da lei, nao detalhe meu. **DESFEITO**: `git checkout HEAD -- test_palavra_do_dia.py`, e e
contra o selo original que o RED abaixo foi medido.

### (1) REMEDICAO DO PASSO 1 -- `realizado_sem_turno` = **88** (era 122 em 03/10)
Pela funcao real (`ponto/turnos.py::realizado_do_dia`, 18.004 chamadas ao juiz), emp2+3+4, competencias 09
e 10, com o O142 no ar (`logs/o134/bordas_sem_turno_20261004.out`). **Fecha com o contador nos 6 recortes**
(`LISTA pela predicada da propria funcao ... FECHA com o contador`):

| | emp2 | emp3 | emp4 | total |
|---|---|---|---|---|
| **09** | 52 | 11 | 0 | 63 |
| **10** | 23 | 2 | 0 | 25 |

**CLASSES (`logs/o134/bordas_sem_turno_classes_20261004.out`, erros=0): A=30 · B=58 · C=0** -- contra
**30/61/31** em 03/10. A classe C **zerou** (era 31). O que o fallback entrega hoje: **ZERO em 73** dos 88
dia-colab, e **numero em 15** -- 2 na classe A (2,0 h) e 13 na B (43,2 h), **45,2 h** no total.

### (2) RESULTADO 2 DO AVAL -- a classe A **nao pede cura de origem**, e a prova e a autopsia
O aval manda: *"Se sobrar dia com PAR FECHADO e sem_turno (classe A ou C > 0): cura na ORIGEM primeiro"*.
A=30 > 0, entao a condicao disparou e eu a respondi medindo, nao argumentando
(`logs/o134/classeA_autopsia_20261004.out`, erros=0): **30 de 30 = `2-BATIDAS-SAO-DO-TURNO-DO-VIZINHO`**,
com **`batidas sem dono: 0`** em todos. O "par fechado" que a classificacao via vinha do **pareamento
ISOLADO do dia** -- uma fatia de 1 dia fabrica par com a cauda do turno da vespera --, e nao da autoridade:
col859 22/08 tem 3 batidas (03:00 S, 04:00 E, 06:59 S) que `parear_turnos` atribui ao turno de **21/08**,
ja fechado com 4 batidas na ata. Pela autoridade, **nenhum dia da classe A tem par fechado PROPRIO**: o
`sem_turno` esta certo nos 30. Nao ha cura de origem pendente aqui, e eu **nao chamo esses dias de
`dono=CADASTRO`** -- a pergunta de quem e o dono nao e esta fatia.

### (3) ITEM (2) DO ADENDO -- O RED, MEDIDO NAS DUAS ARVORES
O selo novo (`escala/tests/test_montador_realizado_pela_autoridade.py`, 4 casos) descreve a **LEI**, nao a
cura: ele e **VERDE no HEAD** de proposito, e quem o deixa vermelho e o patch. As fixtures sao os tres dias
de prod da BUG-145, com os numeros da sombra ao lado.

    ANTES  (wt-antes, codigo do HEAD 8892fc31) ... Ran 25 tests  OK
    DEPOIS (wt-ct, patch do passo 5) ........... Ran 25 tests  FAILED (failures=7)

**As duas metades da condicao (2) do adendo FALHAM, e cada uma com a linha que a prova:**
- *palavra + numero nos tres leitores* -> `test_11`: `'Sem turno pareado' != 'Sem turno pareado (ata 8h)'`;
  `test_11b`: `'ata' not found in 'Sem turno pareado'`; `test_11d`: idem na chave de transporte. (O
  `test_11c` segue VERDE: os tres leitores concordam -- **os tres perderam o numero juntos**.)
- *`classificar_dia` nao ganha as duas etiquetas* -> col250 29/09 ganha
  `REALIZADO_ZERO_COM_TURNO: '3 celulas casadas com status ok e previsto=660, mas realizado=0'`; col174
  01/09 ganha `BATIDA_ORFA_FORA_TOLERANCIA: '1 batida(s) sem marco no dia-turno (00:57)'` **e** o
  `REALIZADO_ZERO_COM_TURNO` **e** `FURO_PARCIAL`.

**A CAUSA, EM UMA LINHA, e ela esta no leitor, nao no meu patch:** `ponto/supra_juiz.py:131` le
`real = dia.get('minutos_realizados') or 0`. O `None` com que a autoridade **recusa** o numero colapsa em
**0**, e o gate da 151 (`real == 0 and n_ok >= MIN_CELULAS_OK`) dispara. **UMA LINHA ABAIXO**, na 136, o
MESMO arquivo trata `n_ok` com `is None` de proposito, e a lapide dele diz por que: *"n_ok=0 e o caso MAIS
comum e um `or` o trataria como 'nao sei' -- a armadilha que esta serie inteira vem separando"*. A
armadilha esta aberta no campo vizinho. **Eu NAO toquei nessa linha**: `ponto/supra_juiz.py` nao esta no
`MUDA` do aval (LEI-AKITA 9), e curar la sem a resposta da lei nao desbloqueia nada -- a metade do NUMERO
continuaria falhando. Ela entra como **consequencia nomeada** da lei, no topo deste arquivo.
**CONFERENCIA CRUZADA que fecha o RED com a frota:** o DIFF de cobranca mediu
`+ REALIZADO_ZERO_COM_TURNO 30` (= 15 dia-colab x 2 raias) e as classes mediram **15 com ata > 0**. As
duas listas sao a MESMA, medidas por caminhos diferentes -- e os 73 de ata zero nao trocam de veredito.

### (4) ITEM (3) DO ADENDO -- `classificar_export` 09 e 10: **antes == depois, CUMPRIDO**
`logs/o134/export_antes_depois_20261004.out`. A **mesma** sonda, o **mesmo** dado da sombra, **dois
codigos**: o arreio `logs/sombra/rodar_na_sombra.sh` passou a aceitar `APP_SOMBRA=<arvore>/app` (uma linha,
e as amarras da sombra seguem com UM escritor -- nao se forqueia um segundo arreio para trocar um `-v`).
**Anti-vacuidade da propria rodada**: a sonda imprime `minutos_realizados_do_dia presente = True` no ANTES
e `False` no DEPOIS, entao as duas medicoes usaram codigos diferentes de verdade.

    emp2 09  universo=459  entra=147   |  emp3 09  universo=123  entra=58   |  emp4 09  universo=22  entra=12
    emp2 10  universo=431  entra=143   |  emp3 10  universo=117  entra=65   |  emp4 10  universo=21  entra=8
    TOTAL ENTRA = 433, e os SEIS digests sao identicos nas duas arvores.

**E POR QUE SAO IDENTICOS -- esta e a parte que importa guardar, e ela NAO e boa noticia:**
`folha/export.py::motivos_retencao_celula` le **`CelulaDia.veredito` LAVRADO** (lapide HX-BORDA-CELULA,
28/08), nunca `classificar_dia` ao vivo. Entao o patch **nao muda o TXT no deploy**: ele muda na
**RELAVRATURA**, dia por dia, quando quer que ela venha -- e `ponto/services/cartorio.py::impressao_insumos`
**nao hasheia a ata**, de modo que o cron das 06:28 nao relavra um dia cuja unica mudanca e o valor dela.
Mudanca latente e espalhada no tempo e PIOR que mudanca imediata, porque ninguem a ve acontecer. Medido
colab por colab (parte B da sonda): dos **5** dia-colab que o DIFF de cobranca marcou `PASSA A RETER`,
**4 ja estao `fora (furo_espelho)` hoje** (col174 09, col382 09 e 10, col654 09) -- a relavratura nao
move o TXT deles. **So col454 comp 09 `entra` hoje, com `retencao_celula: nenhuma`**: a relavratura tiraria
**1 colaborador** do TXT da competencia 09. Quando a lei responder, o apply tem de incluir a relavratura
**forcada** no mesmo ato, ou a casa fica com um TXT que muda de conteudo sem ninguem ter mandado.

### O QUE FICA NO CHAO, e onde
O patch do passo 5 (16 arquivos, `escala/utils.py` -54 linhas, `PENDENTES` zerados, as duas celulas da
matriz `verde=True`, placar `13/22 -> 15/22` medido pela funcao real) segue **inteiro e sem commit** em
`/home/ronald/wt-ct`. Nada dele foi para a arvore viva, nada foi deployado. O que ENTRA agora na arvore e
**so o selo** -- ele e verde no HEAD, e e ele que impede o patch de pousar calado enquanto a lei nao vier.

## 04/10 10:02 — O ATO UNICO: O142 + O130 + R4 NO AR EM 14 SEGUNDOS, E O PORTAO QUE ELE MANDOU FORCAR JA ESTAVA ABERTO

PROVA: `d1689254` (o merge do ato) e ancestral de `logs/deploy.stamp::COMMIT` (`8c3035bc`) --
`git merge-base --is-ancestor` rc=0.
**O `!` (09:3x, literal):** *"forca a janela_auth para o deploy do O142 + O130 + R4 num ato so
(merge e deploy.sh juntos), smoke nas 3 cascas no RELATO, religa o reload das 03:30 depois."*

**O ACHADO QUE VEIO ANTES DE GASTAR O `!`, e e o que importa guardar.** Eu medi a guarda antes de
atravessa-la, e ela **respondeu OK por si**:

    $ bin/janela_auth.sh origin/main
    janela_auth: OK -- nenhum sitio de auth mudou desde origin/main (10 declarados).
    $ bin/janela_auth.sh --janela
    PROIBIDA: fim de semana (dow=7)

As duas respostas estao certas e **discordam**, porque sao perguntas diferentes. `janela_auth.sh:74`
diffa `origin/main...HEAD`: isso e *"o que eu vou EMPURRAR"*. O `app/api/urls.py` do O142 foi para
`origin/main` no push de 01:0x, entao **saiu do diff** -- e com ele saiu a unica razao pela qual a
guarda bloquearia. O que o deploy publica nao e o delta contra `origin/main`: e o delta contra o
**AR**, que era `e49a8289`. Enquanto o commit estava sem push a guarda mordia; **depois do push ela
fica muda**, e e justamente depois do push que se deploya. Mesma familia do O173 (selo que compara o
disco com o ar) vista do outro lado.
**POR ISSO NAO FORCEI.** `SEM_JANELA_AUTH_MOTIVO` existe e eu tinha o aval para usar, mas ele
*pula* a guarda e grava no log uma linha de emergencia: escrever "atravessei a janela de auth" sobre
um portao que respondeu OK seria **trilha falsa**, e trilha falsa e pior que trilha nenhuma. O deploy
passou pela guarda **de pe**, e a linha dela esta na saida abaixo. O `!` nao foi gasto -- fica de pe
para o proximo ato, se ele precisar.
A CURA e de uma linha (`BASE` = o ref do AR, nao `origin/main`) e **nao entrou neste ato**: ela
mudaria o portao do proprio deploy que ela esta julgando. Vai como fatia propria, com o par RED que
morde nos dois sentidos (pushado-e-nao-publicado deve BLOQUEAR; publicado-e-igual deve PASSAR).
De carona, o mesmo arquivo promete em `:94` um `bin/janela_auth.sh --forcar "<motivo>"` que **nao
existe**: nao ha ramo para ele, o `--forcar` cai em `BASE`, o `rev-parse` falha e o script trata
*tudo* como mudado -- isto e, a saida de emergencia anunciada **bloqueia mais** que a guarda. A
porta real e a variavel do `deploy.sh:147`. Entra na mesma fatia.

**A FORMA DO ATO: MERGE EM COPIA, E A ARVORE VIVA SO LEVOU UM `--ff-only`.** A secao 2 do CLAUDE.md
da as duas formas; escolhi a segunda por tres razoes medidas, nao por gosto: (a) as duas raias tocam
`core/placar_estrutural.py`, logo a resolucao seria a mao com o merge meio-aplicado no bind-mount;
(b) `--no-commit` de uma recusa a outra enquanto `MERGE_HEAD` existe, entao a forma viva seriam DOIS
merges com uma janela de resolucao entre eles; (c) elas tocam **dois templates**
(`ponto/espelho.html` e `escala/cadastro_x_realidade.html`) e template **nao espera deploy** -- cai e
vale na hora. Cada segundo de resolucao seria template novo com `.py` velho na tela, que e exatamente
o prejuizo de 30/09 que a lapide dessa secao registra.
Na copia (`wt-merge`, ramo `raia-merge`): merge do O130, merge do R4, **zero conflito** (cada raia
editou a ENTRADA do seu proprio resultado no placar -- R3 numa, R4 na outra), diagrama reconferido na
arvore MESCLADA (`gerar_diagrama --check` -> `ARQUITETURA.mmd e MAPA.md == codigo`) e a **suite
inteira**, que nenhuma das duas raias tinha rodado com a outra dentro:

    bin/suite.sh --dir /home/ronald/wt-merge
    Ran 9590 tests in 1140.998s
    OK (skipped=42)

**UM ERRO MEU NO CAMINHO, e ele e de FERRAMENTA.** `bin/gerar_diagrama.py` e so um atalho de host:
`docker exec saas_core python manage.py gerar_diagrama`. Rodei-o com o `cd` na copia esperando que
ele olhasse a copia, e ele rodou **dentro do container de prod**, cujo bind-mount e a **arvore
viva** -- escrevi no `app/docs/ARQUITETURA.mmd` de prod. Sem dano (o resultado deu identico ao HEAD,
`git status` limpo), e a licao e a de sempre nesta casa: `cd` nao move o bind-mount de um container.
A conferencia da copia saiu do `docker run` com a copia montada, que e a unica que responde pela
copia.

**O ATO, 10:02:25 -> 10:02:41, com NADA no meio** (`logs/` tem a saida inteira):

    git merge --ff-only raia-merge     -> 160d3f5d..d1689254, 21 arquivos, +1269/-74
    bin/deploy.sh                      -> rc=0 em 14 s
      arvore_sem_conflito: OK -- 0 em conflito, 0 marcador em .py/.html
      deploy: migrations pendentes no schema do cliente: 0
      janela_auth: OK -- nenhum sitio de auth mudou desde origin/main (10 declarados)
      sombra: carimbo dia=20261004 status=OK tipo=completa diverge=0 erros=0
      prova de casca: 16 estaticos, 5 paginas compiladas, 598 rotas em 2 urlconf(s)
      importerror_500=0 (09:02 a 10:02)
    bin/crons.sh install               -> 102 linhas; `check` diz crontab == config/crons.py

**O RELOGIO RELIGADO, pelo ESCRITOR UNICO e nao pelo backup.** A linha do reload de higiene das
**03:30** voltou -- e voltou por `bin/crons.sh install`, que renderiza de `config/crons.py`, **nao**
pelo `crontab logs/crontab.backup.04-10-0310` que eu mesmo havia anunciado as 03:1x. O backup e de
**03:06** e a `raia-r4` ACRESCENTA um cron (`lavrar_previsto_cego` as 07:33): restaurar o backup
religaria o reload **e desinstalaria o cron novo** no mesmo gesto. Cron-como-codigo tem um escritor;
quando ha um, a "restauracao em uma linha" que eu prometi era a linha errada.

    crontab -l | grep reload-agendado    -> 30 3 * * * ... deploy.sh --reload-agendado   (viva)
    crontab -l | grep lavrar_previsto    -> 33 7 * * * ... lavrar_previsto_cego --apply  (nova)
    1 linha comentada no crontab (nao e esta)

Trilha de fechamento em `logs/pausas.log`.

**SMOKE DAS 3 CASCAS** (GET-only, pelo dominio, atravessando o Caddy; token por arquivo `-K` 600,
nunca no argv -- SEGREDO-FORA-DO-ARGV). Nenhum POST: script que POSTa em porta de prod e ESCRITA.

| o que prova | rota | resposta |
|---|---|---|
| CADDY + CORE vivos | `/health/` | **200** em 49 ms |
| **O142**, a rota ADITIVA, no ar pela 1a vez | `/api/mensageria/turno-invariante/` | **200**, e responde o contrato (`falta_termo`, pede nome/CPF) |
| **R4**, a 4a perna (o LEITOR) no ar | `/api/mensageria/saude/` | **200**, `contadores.previsto_cego = {esperado: 0, sem_medida: true}` |
| UI viva, urlconf do R4 resolve | `/escala/cadastro-x-realidade/` | **302** para login em 423 ms |
| UI viva, urlconf do O130 resolve | `/ponto/espelho/` | **302** em 33 ms |
| UI **renderiza** de verdade | `/login/` | **200**, `<html>` no corpo |

**A TESTEMUNHA DO O142 NAO E UMA PESSOA, E O SELO.** Para arrancar numero da rota do O142 eu
precisaria de `?termo=<nome ou CPF>` de alguem real, e nome de colaborador nao vai para RELATO so
para provar que uma rota responde. Quem prova a mesma coisa sem nomear ninguem e o selo do O173, que
compara **import tardio do disco contra o AR** -- e era ele o unico vermelho real dos 59 selos de
host desde 01:5x, justamente porque `ponto/turno_leitura` (modulo novo do O142) nao estava no ar:

    IMPORT_TARDIO no_ar=d1689254 imports_tardios=4227 acusados=0 defeitos_declarados=0/0
    import_tardio: OK e MORDE

O RED dele morreu **pela cura que o proprio cabecalho dele manda** (*"a cura, quando ele acusa, e
bin/deploy.sh -- nao se ajusta o selo"*), e com ele a `bin/regua.sh` volta a alcancar a suite.

**O QUE O CONTADOR DO R4 AINDA NAO DIZ, e e honesto que nao diga.** O leitor no ar responde
`sem_medida: true`, porque a LAVRA e do cron das 07:33 e ele ainda nao rodou neste codigo. Eu tentei
adianta-lo na mao (`tenant_command lavrar_previsto_cego --apply`, idempotente por requisito, papel
LAVRA declarado) e a **permissao de escrita remota desta sessao recusou**. Nao forcei por outro
caminho: o numero medido pela funcao real esta no placar (**3 na corrente, 5 na 09, uniao 6**) e o
contador do ar o repete amanha as 07:33 sozinho. Se ele nao repetir, e defeito do cron e tem dono.

## 04/10 08:3x — O TRIPWIRE QUE O R4 NOMEOU NASCEU, E O PRIMEIRO NUMERO DELE ERA MENTIRA DUAS VEZES (R4)

O `98924ef0` fechou uma CLAUSULA do RED (c) do R4 e deixou escrito, no proprio placar, o que
ele **nao** tinha feito: *"o TRIPWIRE 'ativo com batida e previsto 0' NAO nasceu -- pela secao 4a
ele e um contador de frota com esperado 0, e o que nasceu aqui e RETENCAO em UM leitor"*. Esta
fatia e esse contador, e ele e as duas linhas de CODIGO (fila 1) do R4 numa so: **contador de
frota com esperado 0** que **le testemunha de FORA do gravado**.

**O QUE NASCEU** (raia `raia-r4`, 11 arquivos -- a lista sai do `git status --porcelain`, nao da memoria):
- `escala/services/previsto_cego.py` -- `CHAVE` + `medir()` + `ultimo_lavrado()`, a primeira das
  QUATRO pernas do molde de assinatura de cadastro desta casa (molde: `vigencia_impossivel.py`).
  **SO LEITURA.**
- `escala/management/commands/lavrar_previsto_cego.py` -- VIGIA. Sem flag = DRY-RUN; `--apply`
  escreve `MetricaSnapshot(escopo='frota')` e nada mais. **Nao julga.**
- `config/crons.py` -- **07:33**, `estagio='auditoria'`, `depende=('reconciliar_vinculo',)`,
  papel `'lavra'` declarado.
- `escala/services/cadastro_realidade.py` + `templates/escala/cadastro_x_realidade.html` --
  a assinatura **C3** na lista do admin, que e por onde a L-099 manda a divergencia de dono
  CADASTRO sair (e **proibido** curar CADASTRO por codigo).
- `api/views_mensageria.py` -- o **QUARTO** lugar do molde, e a lacuna que eu ia deixar passar:
  eu havia nascido o contador com o triplo (servico + command + cron) e parado. O quarto e o
  LEITOR da porta do HAIKU (`api_mensageria_saude`, `saida['contadores']`), onde os tres irmaos
  (`fase_conflitante`, `vigencia_sem_trilha`, `vigencia_impossivel`) ja moram. Sem ele a chave
  seria lavrada por cron todos os dias e lida por NINGUEM -- a LEI-AKITA 12 em uma linha
  ("chave sem leitor = VERMELHO") --, e **nao faria barulho**: o cron fica verde escrevendo
  `MetricaSnapshot` que nenhuma porta abre. **E nao havia selo que mordesse**: os selos dos tres
  irmaos chamam `ultimo_lavrado()` DIRETO (ex. `test_contador_vigencia_impossivel.py:108`), e
  chamada direta fica verde com a chave FORA da porta -- ninguem enumerava o dict
  `saida['contadores']`. Tirar uma chave dele passava.
- `core/placar_estrutural.py` -- a celula R4 deixa de PEDIR o tripwire e passa a REGISTRA-LO, com
  as duas autoridades lidas, o numero medido, os intermitentes contados e as DUAS medicoes erradas
  do caminho. A lista `CODIGO (fila 1)` do R4 encolhe: o tripwire e o contador de testemunha de fora
  do gravado SAIRAM dela, nascidos juntos aqui; sobram as duas portas de VINCULO, que esperam o `!`.
- `docs/ARQUITETURA.mmd` -- regenerado por `manage.py gerar_diagrama` contra a RAIA (o
  `bin/gerar_diagrama.py` nao serve aqui: ele faz `docker exec saas_core`, que le a arvore VIVA).
  O delta e so o que o codigo mudou: 55 -> 56 nos diarios, o no `lavrar_previsto_cego<br/>07:33` e
  a aresta `reconciliar_vinculo --> lavrar_previsto_cego` (39 -> 40 arestas `depende`).
- tres selos: `test_contador_previsto_cego.py` (7 casos), `test_c3_na_lista_e_contador_por_codigo.py`
  (4 casos) e **2 casos novos em `api/tests/test_api_mensageria_saude.py`** -- o que faltava para a
  perna nova ter guarda: `test_08` cobra `sem_medida` nas QUATRO chaves nomeadas (ausencia de medida
  nao passa por zero) e `test_09` prova que o numero sai da LAVRA e nao de conta no request.

**O NUMERO, pela FUNCAO REAL** (`medir()` chamada na sonda de frota, cpuset de teste, leitura so --
prova `logs/placar_estrutural/r4_contador_previsto_cego_20261004.txt`, 04/10 08:26:24, **2,5 s**
sobre 518 ativos):
- **competencia CORRENTE** (21/09..20/10): universo **467** de **518** ativos nao isentos,
  **TOTAL CEGO 3** (col43 44 batidas/3 vinculos · col882 28/1 · col924 46/0, os tres
  `sem_vinculo_que_cobre`), **7 intermitentes FORA**;
- **competencia 09** (21/08..20/09): universo **480**, **TOTAL CEGO 5** (os de cima + col391 e
  col942 `sem_vinculo_que_cobre`, e col717 `fase_indefinida`), **8 intermitentes FORA**;
- uniao **6** -- exatamente a lista da medicao honesta de 08:02, colab por colab. E **col882 e o 1
  que entrava no TXT** antes da cura do `cadastro_zero` de 03:4x: o contador ve o mesmo colab pelo
  outro lado.

**O PRIMEIRO NUMERO ERA 15, E ESTAVA ERRADO DUAS VEZES** -- esta e a parte que importa guardar.
(1) Eu medi por `escala/utils.py::minutos_previstos_periodo`, que **nao tem UM leitor em
producao** (censo por AST: tres comentarios e dois arquivos de teste, mais nada) e cuja docstring
ensina *"0 = espelho CEGO"*, falso no intermitente; (2) **8 dos 15 eram intermitentes**, onde
previsto 0 e a **LEI**. Intermitente sai do universo por lei e **e CONTADO** no proprio numero
(7 e 8 acima), pelo precedente do `vigiar_motor_autoridade` (*"frota ativa nao-intermitente"*) --
contador == universo. A funcao morta virou **O184** no BACKLOG, com o censo colado; nao a removi
nesta fatia, porque apagar funcao e ato proprio.

**E A PROPRIA `medir()` NASCEU DIZENDO ZERO POR NAO TER PERGUNTADO.** Primeira rodada, 08:11:45:
`universo: 0` com 518 ativos e `TOTAL CEGO: 0`. Causa: `batidas_apuraveis` devolve
`.order_by('timestamp')` -- autoridade unica do universo apuravel, e esta certa em devolver --, e
`values_list('colaborador_id').annotate(Count('id'))` **herda o order_by para dentro do GROUP BY**:
uma linha por batida, `n=1`, ninguem com par. O contador tinha acabado de me dar um **zero
tranquilizador**, que e o pior defeito que um contador pode ter. Cura: `.order_by()` antes do
`values_list`, com o numero medido no comentario, e o caso que MORDE
(`test_MORDE_o_universo_conta_quem_tem_par`) -- sem ele o selo passava verde com o defeito de pe.

**QUATRO REDs EVIDENCIADOS nesta sessao, nao narrados:**
1. `test_o_contador_NAO_tem_juiz_proprio_de_previsto` ficou **VERMELHO de verdade** na primeira
   rodada: `'minutos_previstos_do_dia' not found in {...}`. Os dois juizes entravam no helper
   **injetados por parametro** (`_vigente`, `_previsto`), e o selo varre **CHAMADA** por AST. Curei
   do lado do CODIGO, nao do selo: o alias morreu e as duas autoridades se chamam pelo nome.
   Alias nao e estilo aqui -- e o disfarce exato que deixaria uma conta propria passar amanha.
2. `test_MORDE_a_etiqueta_C1_NAO_conta_a_C3`: com o codigo antigo restaurado a mao,
   `AssertionError: 2 != 1`. **Era um BUG NO CAMINHO** (LEI-AKITA 6) e foi curado na hora: a
   etiqueta `C1` da tela tinha `colabs=len(do_cadastro)` e `ocorrencias=sum(len(v) for v in
   cad.values())` -- os dois numeros de **todas** as assinaturas de cadastro somadas debaixo do
   rotulo da primeira, contradizendo o comentario tres linhas acima dele. Agora e **uma etiqueta
   por codigo**, contada pelo que a linha dela diz, e `ocorrencias` conta so o que ENTROU na lista
   (o antigo incluia colab filtrado por empresa visivel: numero maior que a lista debaixo dele).
3. **O selo B6 me parou no minuto do cron.** `lavrar_previsto_cego` nasceu as **07:19** e
   `chamados.tests.test_contract_crons` ficou vermelho: `auditar_invariantes_chamados@07:18 (ate
   07:21) invade lavrar_previsto_cego@07:19`. Eu havia escolhido o minuto por **vizinhanca de
   assunto** (ao lado dos outros `lavrar_*`); quem decide o minuto e o **relogio**. Movido para
   **07:33**, o buraco medido do corredor (o `lavrar_jornada_lixo` fecha 07:32, o proximo e 07:36),
   com 3 min de folga para uma funcao de 2,5 s. B6 reconferido na fonte: **violacoes []**.
4. **A SUITE INTEIRA pegou o quarto, e nenhuma rodada recortada pegaria.** `core.tests.
   test_selo_teste_sem_relogio` ficou vermelho com os **dois selos novos**: eles liam
   `timezone.localdate()` solto no `setUpTestData`. O selo e de 19/09 e existe por causa da bomba de
   00:00 (`ferias.test_ciclos_completos` cravava o dia e o servico lia o hoje de verdade), e o meu
   caso e da mesma familia com um agravante: as datas destes selos andam em volta de uma
   **COMPETENCIA** (21->20), entao o mesmo fixture cai em competencia diferente conforme a HORA em
   que a suite roda. Congelados em `2026-09-25 10:00:00-03:00` pelo molde do irmao
   (`test_contador_vigencia_impossivel.py:31`) -- e o congelamento cobre o que faltava, porque o
   servico ja recebia `hoje` por parametro em toda chamada dos selos: quem lia o relogio real era a
   TELA. Conferido no fonte do freezegun 1.5.5 que o decorador de CLASSE congela em volta do
   `setUpClass` (e e por ele que o Django chama o `setUpTestData`), em vez de supor. Depois: 46 OK.

**PROVAS.** Selos novos + vizinho direto: `Ran 24 / OK`. Selos novos + os 9 vizinhos que ENUMERAM
(cron, escritor de estado, leitor que nao chama motor, contador irmao, `cadastro_zero`):
**`Ran 78 / OK`**. `ruff` nos 6 arquivos: **All checks passed**. Renderizacao do cron a partir da
raia: a linha das 07:33 sai certa pelo `cron_run.sh`. `bin/crons.sh check` **le o container**, isto
e, a arvore VIVA -- entao ele so vera esta entrada depois do merge, e o `bin/crons.sh install` entra
no mesmo ato do merge (nao e opcional: a fonte unica mudou).

**O QUE ESTE CONTADOR NAO FAZ, de proposito.** Nao chama o motor. A sonda de 08:02 tambem dizia
`classificar_export` entra/fora por colab, e isso **ficou fora do cron**: as 07:33 ele roda DENTRO
do `saas_core`, e medicao de frota com motor ali ja chegou a **266% de CPU** com o cliente batendo
ponto. Quem precisa de entra/fora mede na sombra. E a tela **le a lavra**, nunca mede no request
(`api_mensageria_saude` nao calcula); o preco dessa escolha e a tela poder ficar calada se o cron
nao rodar, e esse preco esta pago na legenda: com `sem_medida`, ela DIZ **"C3 sem medida"** em vez
de deixar "nenhuma linha C3" parecer "nenhum colaborador sem dia previsto" -- com selo que afirma
sobre o HTML renderizado.

**DONO, pela L-099: os 6 sao CADASTRO.** Nenhum deles e fatia. Eles saem pela lista do admin (C3,
uma porta so para as quatro causas, porque a ancora da fase **mora so no vinculo**), com a causa no
`detalhe` para o admin saber qual campo. O `esperado` e 0 e hoje **nao e 0** -- e isso e honesto.

## 04/10 06:0x — O CAMPO DECLARA QUATRO VALORES, O BANCO TEM SEIS (O183)

A raia-chamado me entregou dois bugs de cron. Eu os reli NA FONTE antes de repetir
qualquer coisa, porque afirmacao de agente nao e leitura (LER ANTES DE AFIRMAR), e a
releitura confirmou os dois E ACHOU UM TERCEIRO que ela nao viu — o maior dos tres em
numero de linhas. Ela tinha citado `escalonar_chamados_supervisao.py:125`; a linha e
`:121`, e isso nao e detalhe: numero de linha errado e o sinal de que o relato nao foi
lido no arquivo.

**O numero, medido em prod, leitura so** (`tenant_command shell --schema=juliani`,
04/10 06:03:34): o campo `urgencia` declara quatro valores em `chamados/models.py:46-47`
— `consulta|normal|urgente|emergencia` — e o banco tem SEIS. Gravado: `normal` 25.414,
`urgente` 847, **`baixa` 294**, **`critica` 200**, `emergencia` 106, `consulta` 91. Fora
das choices: **494 linhas, 468 delas VIVAS** — e "viva" aqui e a autoridade da casa
(`status_local in VIVOS`, `catalogo/motor.py:24`), nao um campo lateral meu: eu havia
contado por `resolvido_em` primeiro, que da 468 tambem, mas pela regra errada, e o numero
publicado e o da autoridade. Em `urgencia_original`, **2.554** (1.952 `critica` + 602
`baixa`).

**Por que isso faz todo leitor mentir junto.** `sla_limite_horas` (`motor.py:39-41`) e
`SLA_HORAS.get(urgencia, SLA_HORAS_PADRAO)` com padrao 24. Medido chamando a funcao real,
nao replicando a logica: os QUATRO valores nao declarados que a casa conhece — `media`,
`alta`, `baixa`, `critica` — dao **todos 24 h** e **nenhum** esta em `URGENCIAS_ALTAS`,
contra `emergencia` = 1 h e `urgente` = 2 h. Entao a escalada N2 **de 1 hora** do cron das
`*/5` AFROUXAVA o prazo para 24 h — vinte e quatro vezes — e nenhum leitor podia perceber,
porque o valor que ela grava nao existe em dict nenhum para discordar dela.

**Censo de escritores fechado, por AST sobre a arvore inteira.** Quatro:
1. `escalonar_chamados_supervisao.py:121` — `urgencia = "critica"`, cron `*/5`.
2. `processar_alertas_avancados.py:167` — `!=` sem direcao, REBAIXA quem ja estava em
   `emergencia`, num comando chamado escalonar. Cron `*/30`.
3. **`api/views.py` e `api/views_core.py`** — nasciam com `urgencia='baixa'` pelo kwarg de
   `criar()`. O par de kernels, 294 linhas. **Este e o achado novo**, e e o de maior volume.
4. `chamados/signals.py:68-75` — o ESPALHADOR: ao encerrar com urgencia != normal copia o
   valor vigente para `urgencia_original`. E por ele que o 2o campo tem 2.554 onde o 1o
   tem 494.

**A origem, lida no gate.** `chamados/models.py:538` `criar()` termina no
`cls.objects.create(..., **campos)` da `:552`, e `urgencia` entra por `**campos` sem guarda
nenhuma. A lei que faltava nao e nova: **ela ja existe e um leitor ja a usa** —
`detectar_vinculo_divergente.py:115` le `_meta.get_field('urgencia').choices` como
autoridade. LEI-AKITA 4 em forma pura: a pergunta nunca foi "qual a regra", foi "qual
escritor nao migrou". A raia ja havia curado (1) e (2) com uma porta boa
(`escalar_urgencia`, ValueError fora das choices, direcao pela funcao `sla_limite_horas`,
selo AST), mas ela guarda a **escalada** e nao o **nascimento**, e o selo dela so pergunta
pelos argumentos POSICIONAIS de chamadas a `escalar_urgencia` — o kwarg de `criar()` fica
fora da pergunta. A propria docstring dela declara o nascimento fora de escopo e o atribui
a `cd535500`; eu li o `cd535500` (O167) e ele fechou a CORRIDA do `abrir`, nao a guarda de
urgencia. **Nenhum branch guardava urgencia no nascimento: o gap nao tinha dono.**

**O bug que apareceu DENTRO da cura, e veio primeiro (LEI-AKITA 6).** Com o gate levantando
`ValueError`, fui fechar o censo dos chamadores e achei o que o AST de producao nao mostra:
`chamados/services/abertura.py:57` passa pelo `criar()`, e a urgencia dele vem de
`chamados/views.py` como **texto livre do navegador** (`request.POST.get('urgencia',
'normal').strip()`, dois sitios). A minha guarda transformaria isso em **500 onde hoje
nasce chamado**. Pior: `api/views.py:2134` ja validava a mesma coisa — contra uma **TUPLA
LITERAL** dos quatro nomes, coagindo para `normal`. Era a segunda testemunha da pergunta
que as choices do campo ja respondem, e nao estava errada *hoje*; estava **destinada** a
divergir, e foi divergencia desse tipo que deixou `baixa` nascer 294 vezes a 1.600 linhas
dela, no mesmo arquivo.

**A fronteira, e a decisao que eu quase tomei errada.** Eu ia escrever uma recusa (`erro`
na cadeia que a view ja tem) em vez de normalizar, porque coagir *parece* fallback. O
arquivo me corrigiu: a guarda `if modulo not in _MOT` de `chamados/views.py` faz
**exatamente** isto com `modulo` desde
22/08, com lapide nomeando a classe identica de defeito — *"modulo vinha cru da request =>
qualquer string virava modulo_origem"* (HX-MODULO-DA-REQUEST, A7). Recusar na tela do admin
enquanto o app nativo coage daria **TRES respostas para uma pergunta**, que e justamente o
que a LEI-AKITA 2 proibe. Entao nasceu UMA fronteira — `chamados/models.py::urgencia_da_fronteira`
— lendo a MESMA autoridade e devolvendo o **`default` do proprio campo** (nao um literal:
seria o terceiro sitio de `'normal'`, ao lado do campo e do `POST.get`). Os tres leitores de
texto livre passaram a le-la. A coercao fica no caminho do app nativo DE PROPOSITO: sao ~750
celulares, parte com build velho, e trocar coercao por recusa muda o que o celular ve — isso
pede smoke, nao patch.

**RED evidenciado, e pelo PROPRIO juiz.** O selo novo e uma FUNCAO
(`_escritas_de_urgencia`), de modo que o par que MORDE julga com o mesmo codigo que julga a
arvore. Rodando essa funcao contra `git show HEAD:` — nao contra uma regex parecida: **6
ofensores** (`api/views.py:756`, `api/views_core.py:756` e 4 fixtures), e **0** depois.
O selo cobre as tres formas que de fato puseram valor invalido em prod: kwarg,
`{'urgencia': ...}` de `defaults` e `.urgencia =` cru.

**O que a varredura NAO vê, e eu fui olhar.** Ela exclui `/tests`, e os testes eram justamente
onde o resto do vocabulario morava: **23 sitios** com valor nao declarado, e com **dois valores
que eu nunca tinha medido** — `media` (14) e `alta` (4). Cruzando com o alvo de cada chamada,
**12 chegavam ao `criar()`** e ficariam VERMELHOS; os outros usam `objects.create`, que o gate
nao cobre de proposito. Os 12 migraram para `normal`, e a troca e **preservadora de medida pela
funcao real** (24 h -> 24 h, fora de `URGENCIAS_ALTAS` nas duas pontas); nenhum deles afirmava
sobre a string. `media`/`alta` nunca chegaram ao banco: o `form_abertura.html` oferece
exatamente os 4 declarados — isso e **fato grepado**, nao inferencia a partir do banco.

**Onde ela pousou, e o que fica em pe.** A raia-chamado nao esta mais viva (`ListAgents` as
06:0x lista so um fork de 1d atras), entao o gap era meu, e foi para `/home/ronald/wt-esmeril2`
onde a porta e o selo moram — no main seria um segundo vocabulario para a mesma pergunta e
conflito de merge garantido com `4dd016f5`. **O ESTOQUE de 494 continua em pe, e isso e
ordem, nao hesitacao**: o escritor do `baixa` esta vivo no main, a porta nao pousou (a raia
espera o mesmo `!` da `janela_auth` que segura o O130 e o R4) e nao ha DIFF. Curar dado com
escritor vivo e meia-correcao com nome de limpeza. E a porta **nao** cura o estoque — medido:
`escalar_urgencia('normal')` sobre um `'baixa'` devolve **False**, porque 24 < 24 e falso.
Dono = **ESTRUTURA** pela L-099, sem duvida: o sistema se contradiz consigo mesmo, nao com o
cadastro.

**Uma lapide que envelheceu no proprio patch que a escreveu.** Eu citei `api/views.py:766` em
quatro sitios; a linha viva e **776** — a minha propria lapide empurrou o write 10 linhas para
baixo. Corrigido, e agora a citacao vem com a ancora que nao envelhece
(`grep -n "urgencia='normal'" app/api/views.py`). "Linhas podem andar: ancorar por grep e
RECONFERIR lendo" estava escrito no aval; eu ancorei e nao reconferi.

**O selo da casa me mandou de volta, e ele estava certo.** As duas funcoes nasceram em
`chamados/models.py` e o contrato **B4.6** (28/08, `test_contract_deus_objeto.py`) ficou
VERMELHO: *"funcao de modulo NOVA nasce na casa do papel: juiz de leitura -> juizes.py;
escritor -> chamados/services/"*. Nao era chateacao de linter: `models.py` ja foi deus-objeto
uma vez, o B4 tirou ~61 funcoes de la, e eu estava repondo duas. Era LEI-AKITA 4 outra vez — a
lei existia e a pergunta certa era **qual casa**, nunca **qual regra**. As duas foram para
`chamados/juizes.py` com as lapides intactas, e os tres metodos de `models.py` que as chamam
usam o **import local** que aquele arquivo ja usa 10 vezes
(`grep -n 'from chamados.juizes import' app/chamados/models.py`). O
re-export de compat que eu havia escrito saiu: ele existe para chamador LEGADO, e estas duas
nao tem nenhum — os 3 chamadores de fora apontam para a casa real, como fazem os outros **192**
arquivos que importam de `chamados.juizes`. A TRAVA JUIZ-NOVO nao morde aqui, e isso foi LIDO,
nao suposto: `bin/tests/test_juiz_novo_tem_corte.sh` so olha `core/juizes.py`.

**Cinco fatias entrariam no main sem linha no TICKETS, e o portao e o merge.** Rodei o regex do
proprio `bin/regua_tickets.sh` sobre `git log main..HEAD` da raia: quatro IDs citados
(`C1-PORTA-DO-CICLO`, `C2-PORTA-DO-VEU-DO-ARQUIVO`, `C3-PORTA-DO-CONTEXTO`,
`C4-PORTA-DOS-VINCULOS`), **zero declarados na tabela**, mais o `C1b` que estava nascendo. O selo
le o range `$BASE..HEAD`, entao o merge trazia os quatro de uma vez e o push seguinte seria
recusado -- no portao, com a fila 1 de pe. As cinco linhas entraram agora, com o estado honesto
(**NA RAIA**, espera o `!` da `janela_auth`), e cada uma com o numero da sua propria medicao. Nao
e burocracia: a tabela e o que responde "o que esta em pe" a um chat novo, e quatro portas
estruturais fechadas nao estavam la.

**O SEGUNDO RED DO CAMINHO: declarar porta nao e inscrever a bolha.** O run que juntou o label
`core` pela primeira vez (6.896 testes) voltou `failures=1`, e nao era a minha fatia: era
`core/tests/test_selo_mapa_contrato.py` nomeando **seis** modulos que a raia declarou porta de
`PerguntaDisputa` em `core/portas.py` sem cabecalho de bolha. Medido antes de agir: o parser real
da **NOVAS=6 na raia e NOVAS=0 no main**, e os quatro commits da raia tocaram `core/portas.py` --
a divida e desta raia, logo minha (LEI-AKITA 6). Inscrevi na ORIGEM em vez de engordar o passivo
de 44 para 50: ele e para divida ANTIGA e **so encolhe**, e crescer por declaracao de agora e o
band-aid com o nome que a LEI-AKITA 1 da. A declaracao era honesta -- o censo da casa
(`core/censo_escritas.py::varrer_arvore`) acha escrita de `PerguntaDisputa` nos **seis**
(3/2/6/2/3/1) --, entao nenhum nome saiu da tupla: tirar nome para o selo calar seria a mesma
mentira pelo outro lado. E o cabecalho mudou de redacao onde o NUMERO mandou: `validada_em` tem
**cinco** sitios de producao, nao um (o ato em `validacao.py::validar_pergunta`, o desfazer do
reabrir, e tres carimbos de SIMETRIA que so tocam o campo quando ele esta NULO), logo a inscricao
diz "escritor unico do ATO de validar" com o censo ao lado -- repetir a frase chapada do CLAUDE.md
secao 5 seria por no cabecalho uma exclusividade que o codigo nao tem. `recusa_motivo` (cinco
escritores, donos diferentes) e `via_resolucao` (16 atribuicoes) entraram como divida NOMEADA.
Depois: universo=61, sem_contrato=44 -- o passivo inteiro, nada alem dele --, `faltando()` vazio
nos seis pela funcao real. Commit proprio, so docstring: zero linha de codigo, zero juiz novo.

LEI-AKITA: origem=`chamados/models.py::criar` aceitando `urgencia` por `**campos` sem guarda
(+ a fronteira para o texto livre que chega la), testemunha=`_meta.get_field('urgencia').choices`
e `sla_limite_horas` chamada pela funcao REAL, RED=`_escritas_de_urgencia` contra `git show HEAD:`
= 6 ofensores -> 0, mais os gates levantando e o POST que viraria 500, quem-mais-le=4 escritores
de producao + 3 leitores de texto livre + 23 sitios de teste (12 no gate) + 17 templates que
exibem `urgencia`, juizes novos=0 (`urgencia_da_fronteira` LE a autoridade do campo, nao e
autoridade nova: as duas moram em `chamados/juizes.py` por ordem do contrato B4.6, e
`core/juizes.py` -- o registro de autoridade que a TRAVA JUIZ-NOVO vigia -- nao foi tocado).

## 04/10 05:5x — A FORMA DA SUITE DEIXOU DE SER PROSA E VIROU ARQUIVO, E O SEGUNDO FURO FUI EU DE NOVO (O182)

LEI-AKITA: origem=o paragrafo da CLAUDE.md sec.3 que ensinava a forma destravada, testemunha=`bin/trava_teste.sh --quem` com a pista ocupada, RED=`bin/tests/test_suite_sh.sh` nas DUAS perguntas (saida colada abaixo), quem-mais-le=censo fechado de `bin/`, `CLAUDE.md`, `bin/molde_fatia/` e `.claude/`, juizes novos=0.

**O defeito nao morava em arquivo nenhum.** A lei "UM run por vez no `juliani_db_test`" (corte Ronald 24/09 09:3x) tem device -- `bin/trava_teste.sh`, flock -- e tem selo que o cobra de todo script de `bin/` (`bin/tests/test_trava_teste.sh:40-64`). Mas o comando que a sec.3 mandava COLAR nao chamava a trava, e o selo nao podia ver: ele varre `bin/*.sh`, e aquele sitio morava na PROSA. O maior chamador da suite -- a sessao, o agente de raia, todo molde que copia da doc -- ficava fora da varredura por construcao.

**A SEGUNDA medicao e minha, e e pior que a primeira.** A de 03:58:31 ja estava escrita aqui embaixo (secao das 04:0x): a trava CONCEDEU a vez a outra raia com uma suite destravada minha em voo, e quem protegeu foi o BANCO PROPRIO dela (`REGUA_DB=test_juliani_raiachamado`), nao a trava. Pois as **05:3x eu repeti o furo**, agora em 17 testes de selo e **sem banco proprio nenhum**: no `test_juliani` default, o MESMO banco do pre-push. Conferido depois: `docker ps` tinha um unico container de suite e a minha terminou enquanto o pre-push ainda estava preso no flock -- **nao colidiu por SORTE de janela, nao por desenho**. Os meus proprios scripts da fatia anterior (`vizinhos.sh:26`, `r3.sh:11`) levavam `-e REGUA_DB=test_o130` certo; o comando avulso que eu digitei a mao nao levava. Nao e descuido isolado: e exatamente o que acontece quando a forma certa nao e' um arquivo que se chama.

**A CURA, na origem (nao band-aid).** Nasce `bin/suite.sh`, porta unica, compondo o que estava espalhado: `recursos.sh` (cpuset/`$TESTE_DOCKER`) + `teste_envfile` (senha so em `logs/.env_teste`, modo 600, nunca no argv) + LABELS **lido** de `bin/regua.sh` + montagem pela porta de `arvore_do_push.sh` + **a trava, sempre**. `--only` recorta e **segue pela trava de proposito**: o selo da trava isenta "um punhado de testes", e o meu furo foram 17 -- CURA-MAIS-RESTRITIVA (26/09) manda pegar. `--dir <copia>` roda contra worktree/archive consultando a porta de montagem (ninguem monta tmpfs a mao: isso seria SEGUNDO ESCRITOR da lista, e custou 92 copias orfas em 03/10). `--espera N` existe porque 3600 s de silencio nao e resposta para quem so quer saber se a pista esta livre. `REGUA_DB` se **repassa**, nao se inventa: `config/settings/ci.py` ja decide, e o default `test_juliani` e' o significado historico do carimbo da arvore viva (`regua.sh:158-159` diz isso por escrito).

**O QUE NAO MIGROU, e por que nao e band-aid:** `bin/regua.sh` e `bin/pre-push.sh` ficam com a chamada propria. Os dois **ja pegam a trava** (`pre-push.sh:130`, `ESTEIRA_QUEM=pre-push`) e a regua tem o carimbo em volta da chamada; aninhar `suite.sh` dentro deles seria esperar por si mesmo -- o deadlock de flock que eu evitei no push desta noite grepando o hook antes. O que o O182 pedia era que a DOC parasse de ensinar a forma destravada, e e isso que mudou.

**RED, com a saida colada.** `bin/tests/test_suite_sh.sh` contra o vivo, antes da cura:
```
test_suite_sh: FALHA -- o bloco canonico da sec.3 NAO nomeia `bin/suite.sh`: quem o cola roda a suite DESTRAVADA e fica invisivel para `trava_teste.sh --quem` (O182)
test_suite_sh: FALHA -- bin/suite.sh nao existe -- a sec.3 aponta para uma porta que nao esta la
RED
rc=1
```
Em copia com a cura: `test_suite_sh: OK — a sec.3 aponta para bin/suite.sh, e a porta passa pela trava.`

**O selo recorta pela ESTRUTURA, nunca por palavra.** O bloco canonico e uma linha de shell com continuacoes, entao o recorte vai do titulo da sec.3 ate a PRIMEIRA linha que nao termina em `\`. Isso nao e detalhe: `test_trava_teste.sh` aprendeu a mesma licao TRES vezes (comentario lido como codigo, `pgrep` lido como execucao), e a memoria da casa ja diz **"selo estrutural varre AST, nao texto"** -- aqui o equivalente e varrer a ESTRUTURA do bloco, nao a prosa. O par que MORDE prova os dois lados com o MESMO juiz do vivo: um bloco com `docker run ... manage.py test` cru e' reprovado; um que chama a porta passa; e um terceiro caso cobra que o recorte pare em 3 linhas, para que a prosa de baixo -- que no fixture menciona `bin/suite.sh` de proposito -- **nao** conte como aprovacao.

**A cadeia foi provada REAL, sem mock.** `bin/suite.sh --only core --espera 1` com a pista ocupada:
```
trava_teste: a vez nao chegou em 1s -- segurando: pre-push:1305548 desde 04/10 05:47:09
rc=75
```
Isso exercita de verdade o source do `recursos.sh`, o `teste_envfile`, a leitura do LABELS e a chamada da trava -- e devolve o rc 75 (`EX_TEMPFAIL`, "nao rodou", nunca vermelho). Nenhum `docker` nem `trava_teste.sh` dublado: selo com mock que nao morde foi o que deixou quatro selos passarem vazios em 01/09.

**CENSO "quem mais le", fechado.** Fora da sec.3, quem carrega a forma da suite inteira: `bin/molde_fatia/rodar.sh` (**ja** passa pela trava -- o selo o cobra em separado desde 24/09) e `app/docs/{RELATO,RELATO-ARQUIVO,TICKETS,PROMPTS}.md`, que sao **historia**, nao instrucao para colar (DIETA DE PROSA: mover, nunca apagar, e nao tocar a regra ao mover). Dentro de `bin/`, os sete que rodam `manage.py test`: `regua.sh`, `pre-push.sh`, `regua_relogio.sh` e `vigia_arvore.sh` com trava; `isolamento.sh`, `placar_code.sh` e `regua_calendario.sh` sem ela, e os tres com `REGUA_DB` proprio -- a isencao declarada do selo, porque nao rodam o LABELS inteiro. Nao mexi neles: nao e a fatia, e mexer seria regra fora do pedido.

**ACHADO LATERAL, nao curado (nao e meu para apagar):** `.claude/worktrees/` tem **6** copias orfas de agente, **265 MB**. E a mesma familia das 92 copias / 2,1 GB de 03/10 (`bb008cee`), com outro criador. Fica como linha, nao como acao -- apagar diretorio e `!` quando nao se sabe quem o le.

## 04/10 05:2x — EM ABERTO PASSA A DIZER O QUE FALTA, E MEDIR A FATIA ACHOU O DEFEITO DENTRO DELA (O130, raia `raia-o130`)

Fechada na raia em `97079d6e`, com o censo de 23 modulos vizinhos em `Ran 184 / OK`. **Nao esta no ar**: o merge e o `bin/deploy.sh` sao UM ato (lei de 30/09) e o deploy publicaria o disco, que e exatamente o `!` da `janela_auth` no topo desta pagina. A raia espera o MESMO `!` do O142 -- nao e uma segunda trava.

**04/10 04:5x O130 EM ABERTO DIZ O QUE FALTA (raia `raia-o130`, segunda metade do R3)** -- `Em aberto`
nao dizia O QUE estava em aberto, e a tela do admin dizia por conta propria. Duas bocas, uma redacao:
`ponto/services/dia_decidido.py` emitia a palavra curta e `templates/ponto/espelho.html` tinha um
`{% for m in dia.falta_marcos %}{{ m.tipo }} {{ m.hora }}{% endfor %}` que imprimia o CODIGO CRU --
"falta: S 19:00". Codigo cru nao serve ao PDF nem ao celular, e template com regra propria de redacao
e' a LEI-AKITA 2 (testemunha recalcula).
CURA na origem: nasce **UM compositor**, `dia_decidido.py::frase_do_que_falta`, que le o nome do marco
de `ponto/models.py::Batida.TIPO_CHOICES` -- autoridade que **ja existia** (dois templates ja a usam).
`palavra_do_dia` e `do_dia` ganham o kwarg `falta_marcos`; o `EM_ABERTO` passa a sair
`Em aberto (falta saida 19:00)`. O template perde o laco e imprime `{{ dia.falta_frase }}`.
JUIZ NOVO = **0**, e isso e' censo, nao afirmacao: `falta_marcos` nasce num sitio so --
`ponto/services/espelho.py`, de `escala/utils.py::montar_realizado_grade`, a GRADE, fonte unica de
previsto declarada na secao 4. Todo consumidor e' LEITOR migrando.
OS CINCO LEITORES CONVERGEM: os quatro que passam por `espelho_do_colab` (tela, PDF, TXT, app) herdam
a palavra enriquecida; o quinto -- `colaboradores/services/calendario.py`, o UNICO template que
imprime `palavra_dia` -- foi ligado com **zero query nova**, porque `grade_da_celula` ja devolve
`marcos` por dia. Deixar ele atras reproduziria a propria lapide da casa: *"Palavra que um leitor diz
e o outro nao e' a lapide dos 239 dia-colab de 24/09"*.
DEGRADACAO HONESTA, e e' ESTRUTURAL: onde a ata e' agregada o dia chega com `marcos=[]` (decisao de
`leitor_celula.py::_celulas_da_ata`), o adaptador devolve `[]` e a palavra VOLTA a ser curta -- a mesma
lei do `status=None` do BUG 16, *"nomear marco que a ata nao sabe seria ignorancia carimbada como prova"*.
MEDIDO NA SOMBRA chamando as funcoes REAIS com o meu codigo (competencia 09, emp 2/3/4, 863 colabs,
16.161 dia-colab): a frase FALA em **2.405** dia-colab, fica curta em **13.756**, e **6.028** sao de ata
agregada. Marcos que faltam por dia: 1=527, 2=671, 3=149, **4=1.057**, 6=1. Tamanho da frase:
min **11**, mediana 45, p95 60, **MAX 88** (o `min` desta linha dizia 15 na primeira versao, e os 4
caracteres de diferenca eram o defeito achado abaixo, nao arredondamento).
DECISAO TECNICA (PAREI-SO-LEI -- decido pela lei existente e registro): **enumera SEMPRE**, sem segunda
redacao. O balde maior (1.057 dias) e' o 12x36 inteiro sem nenhuma batida, e uma forma cardinal
("falta as 4 marcacoes do dia") seria **redacao minha**, nao lei -- a L-AKITA 1 chama isso pelo nome.
Os 88 caracteres nao atropelam nenhum leitor, e isso foi LIDO, nao suposto: o calendario ja corta por
`-webkit-line-clamp` com a frase INTEIRA no `title=`
(`templates/colaboradores/partials/_calendario_grade.html`), e o PDF a joga em `obs_parts`
(`relatorios/pdf_espelho.py:859`), a coluna de observacoes que ja acumula varias frases e quebra linha.
FICA NOMEADO para o Ronald: se a coluna Obs do PDF ficar apertada no 12x36 vazio, o corte e' dele --
a frase nao encurta sozinha.
RED: `ponto/tests/test_o130_em_aberto_diz_o_que_falta.py`, **7** casos `SimpleTestCase` (sem banco).
Foram **DOIS** eventos vermelhos, e esta linha ja os misturou uma vez: (1) os **5** primeiros, antes da
cura do compositor, deram **6 errors + 1 failure** -- `palavra_do_dia` nem aceitava `falta_marcos=`;
(2) os **2** da cura do rotulo, escritos depois (`dd473993`), deram **2 failures** com os 5 ja verdes
(`'·I2' unexpectedly found in 'Em aberto (falta saida ·I2)'` e `'falta i ·S' != ''`). As duas rodadas
fecham em `Ran 7 tests / OK`, e e' por isso que o numero final nao prova as duas: cada RED tem a sua
saida colada acima. O contra-exemplo e' o
`test_MORDE_sem_marco_apagado_a_palavra_VOLTA_a_ser_curta` (dia cujo dono e' CADASTRO -- vinculo morto,
sem DNA -- tem `falta_marcos` vazio e **nao pode** dizer "falta" + nada), e o
`test_a_palavra_continua_UMA_so` varre `templates/` inteiro por laco de `falta_marcos` e exige `[]`.
R3 **NAO esta carimbado**: a clausula `com o que falta` e' redacao do Ronald, e quem a fecha e' ele.

**04/10 05:2x O130 cura -- MEDIR A FATIA ACHOU O DEFEITO DENTRO DELA** (`dd473993`). O `min=15` da
medicao acima nao era ruido: era `'falta saida ·I2'`. A `hora` do marco de um **INTERMITENTE** (O84)
nao e horario -- `escala/utils.py:1080` grava `·I1`, `·I2`, ... ali **de proposito**, porque o horista
entra 17:01 num dia e 20:58 noutro e regua por hora nao existe. O compositor colava esse rotulo onde o
leitor espera relogio, e isso chegava a **tela do admin, ao PDF e ao calendario**: glifo interno sem
legenda, do lado de marcos que sao hora de verdade. Irmao latente da mesma familia:
`escala/utils.py:1071` cunha o par de INTERVALO como `('I','·S')`/`('I','·E')`, e `I` nao esta em
`TIPO_CHOICES` -- o `str(tipo).lower()` de reserva que eu havia escrito imprimia `falta i ·S`, a letra
crua, que e **exatamente** o defeito que esta fatia veio tirar do template.
CURA na origem e mais restritiva (CURA-MAIS-RESTRITIVA): a palavra sai **so** de `TIPO_CHOICES` (tipo
que a casa nao sabe nomear faz o marco sair da conta, nao vira letra) e a hora so e colada quando **tem
forma de relogio**. Guarda POSITIVA, nao lista de excecao. O marco segue nomeado pelo tipo --
`falta saida` --, que e o que se sabe. LEI-AKITA 6: bug provado no caminho da fatia, cura na hora.
NAO TOQUEI `ponto/juiz_batida.py:191`, que ja reconhece o rotulo posicional. DECISAO TECNICA sob
PAREI-SO-LEI: a pergunta dele e de **dominio** ("esta ata e posicional?", e isso decide pareamento); a
minha e de **renderizacao** ("posso imprimir esta hora?"). Juntar criaria autoridade compartilhada
entre dominio e desenho -- juiz novo por outro nome. Juizes novos: **0**.
RED antes: `'·I2' unexpectedly found in 'Em aberto (falta saida ·I2)'` e `'falta i ·S' != ''`,
`Ran 7 / FAILED (failures=2)`. Depois: `Ran 7 / OK`, ruff limpo. O selo do tipo inominavel cobra tambem
o caso **misto** (`[{'I','·S'}, {'S','19:00'}]` -> `'Em aberto (falta saida 19:00)'`), para a regra nao
calar o dia inteiro ao descartar um marco.
MEDIDO NA SOMBRA (mesma chamada real): **59 marcos** com hora que nao e relogio, em **34 dia-colab** e
**6 colabs** (col76, col85, col146, col348, col365, col610) -- `·I3` 22, `·I4` 22, `·I2` 15, `·I1`
nunca. Tipo fora de `TIPO_CHOICES`: **0** na frota -- o ramo do `I` e o que ninguem exercita, e por isso
ele tem **selo**, nao so comentario. Tamanho depois da cura: **min 11**, mediana 45, p95 60, MAX 88 --
so o min andou, e andou porque a frase parou de carregar 4 caracteres que nao informavam nada.
TAMBEM NESTE COMMIT: tirei da lapide de `ponto/services/espelho.py` uma afirmacao **minha** que o censo
desmente -- ela dizia que a lista `falta_marcos` fica "porque quem precisa de marco a marco (o
`chamado_id` do badge E3) continua tendo", e e falso duas vezes: ninguem mais le a lista depois do O130,
e a compreensao de `:730` nunca carregou `chamado_id`. A lista fica pelo motivo verdadeiro: e chave
**publica** do dia e duas autoridades a cobram -- o selo do corte de 27/09
(`ponto/tests/test_impar_em_aberto.py:97`) e o `e6_oraculo.py:256`, que raciocina sobre ela em prosa.
Leitor inventado em comentario e contar leitor por memoria, com a diferenca de que fica escrito.

**04/10 05:2x O130 censo de vizinhos -- 23 modulos, 10 failures, e NENHUMA regressao** (`97079d6e`).
`Ran 184 tests / FAILED (failures=10)`, 0 errors, 0 skipped. Os 10 sao 5 testes em 2 classes (uma
herda da outra) e moram TODOS em um arquivo: `relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py`,
o selo da **primeira** metade do mesmo R3. Os outros 22 vizinhos passaram sem toque -- e isso inclui
`core.tests.test_selo_performance`, que e o juiz da afirmacao *"o calendario foi ligado com ZERO query
nova"*, e os 5 smokes de chromium (prova de que a arvore da copia estava completa).
CARACTERIZACAO DE DEFEITO CURADO SE INVERTE: eles pinavam `palavra_dia == 'Em aberto'` exato, e a 2a
metade do R3 e justamente fazer a palavra dizer o que falta. O selo nao se apaga e nao se afrouxa --
`startswith('Em aberto')` seria o caminho barato e deixaria passar as duas coisas que o O130 veio tirar
da tela: frase VAZIA (`'Em aberto (falta )'`) e CODIGO CRU (`'Em aberto (falta S 12:00)'`). As 5
asserções passam a pinar a **frase inteira**, em tres constantes nomeadas no topo, e agora o arquivo e
testemunha das DUAS metades.
A SEXTA MUDANCA E UM BUG QUE A FATIA EXPOS (LEI-AKITA 2): o `test_06` montava um lado do invariante com
`{d['data'] for d in dias if d.get('palavra_dia') == 'Em aberto'}` -- o teste **derivava o veredito da
prosa**. Enquanto a palavra era string fixa isso passava por leitura; com a palavra cheia, o selo
acusou *"a linha e o topo contam universos diferentes"* com o codigo CERTO. O filtro passa a ler
`veredito_dia`, que vem no mesmo dict, do mesmo escritor (`espelho.py:355`), e e a autoridade.
Verde depois: `Ran 28 / OK`, ruff limpo.

## 04/10 04:0x — A TRAVA DE SUITE EXISTE, O SELO A COBRA, E O COMANDO DA DOC NAO A CHAMA (O182)

Nasceu de um susto meu que acabou em correcao de DOIS enganos meus, e os dois valem registro.

**O susto.** Vi duas suites cheias em voo no mesmo `juliani_db_test` -- `practical_germain` (minha,
`wt-r4`, 03:55:17) e `amazing_montalcini` (raia-chamado, `wt-esmeril2`, 03:58:31) -- e li a sec.3
("UM run por vez; colisao gera errors falsos") como se o meu veredito estivesse podre por
construcao. **Estava errado.** `config/settings/ci.py:14-17` e a cura que a casa escreveu em 11/08
para exatamente isto: a raia leva `REGUA_DB=test_juliani_raiachamado`, banco PROPRIO, e os dois
runs nao se destroem. O que eles disputam e CPU no cpuset 4-7 -- ficam LENTOS, nao ERRADOS. O
veredito se le pelo que ele diz, e nao se descarta por medo.

**O segundo engano, esse com consequencia maior.** Eu tinha em maos `test_trava_teste.sh` VERMELHO
com `ordem foi 'A A_fim '` e o reportei como um dos dois selos de host vermelhos. **Era VELHO**: a
cura e minha, de 02:4x de hoje (`9102af37`), e trocou o lock GLOBAL por uma COPIA isolada
justamente porque o veredito dependia da maquina estar vazia. Rodei o selo agora, **com a raia em
voo** -- que e a condicao que o derrubava -- e deu `trava_teste: OK`. Logo o placar de host de hoje
tem **UM** vermelho, nao dois: `import_tardio`, cuja cura e `bin/deploy.sh` e que espera o `!` do
`janela_auth`. Reportar vermelho vencido manda procurar bug onde nao ha -- a mesma doenca que a
propria lapide da trava descreve.

**O que sobra, e e real.** A lei tem device (`bin/trava_teste.sh`, flock) e tem selo que a cobra de
TODO script de `bin/` que roda a suite inteira, com banco proprio ou nao
(`bin/tests/test_trava_teste.sh:40-64`). Mas **o comando que a CLAUDE.md sec.3 manda COLAR nao
chama a trava** -- e foi por isso que a minha suite subiu destravada, sem `REGUA_DB`, no banco
default. Com as duas em voo, `bin/trava_teste.sh --quem` respondia `livre`. O selo nao ve este
sitio porque varre `bin/*.sh`, e o comando canonico **nao mora em arquivo nenhum: mora na prosa**.
E o defeito que esta casa ja pagou com os LABELS escritos em quatro lugares -- a forma certa
existe, e o leitor principal nao migrou (LEI-AKITA 4: a pergunta e "qual leitor nao migrou").

Dano de hoje: **ZERO**, e por sorte de cadastro (a raia tinha banco proprio), nao por trava.
CURA proposta, fonte unica: `bin/suite.sh` compondo `recursos.sh` + `teste_envfile` + LABELS +
`arvore_do_push.sh --montagem` + `trava_teste.sh`, com a sec.3 apontando para ele -- assim nao
existe forma de rodar a suite inteira destravada sem escrever um comando novo a mao. Item **O182**
no BACKLOG. Decisao tecnica, nao lei: registrada aqui e a fila segue (PAREI-SO-LEI).

## 04/10 03:5x — R4: O PORTAO DO TXT PERGUNTAVA "EXISTE CELULA?" PARA SABER SE O CADASTRO DESCREVE O MES

`LEI-AKITA: origem=folha/export.py::classificar_export, premissa do cadastro_zero (a pergunta "existe
celula?" era PROXY de "o cadastro descreve o mes?"), testemunha=escala/utils.py::minutos_previstos_do_dia
(juiz declarado, CLAUDE.md secao 5), RED=folha/tests/test_cadastro_zero_com_batida.py::test_RED_celula_de_vinculo_ENCERRADO_antes_da_janela_e_retida,
quem-mais-le=folha/porta_export.py:392+452, folha/views.py:19 (rotulo segue valido), folha/tests/test_porta_do_export.py,
ponto/services/dia_pago.py:597, colaboradores/tests/test_calendario_le_dia_pago.py:15, juizes novos=0`

**O QUE ERA.** R4 e um dos TRES PARCIAL do placar estrutural (R3, R4, R6). Medido com as funcoes reais na competencia 10
(janela 2026-09-21..2026-10-20), 354 colabs com batida apuravel em emp2: **4 acusados** de "previsto 0 E
trabalhadas 0" -- col43 e col924 ja retidos (`fora/cadastro_zero`), col400 retido por rescisao, e **col882
`entra` com motivo `None`**, `horas_trabalhadas=0,00`, **28 batidas em 7 dias**. emp3 e emp4: 0 acusados.
Tres colabs da MESMA forma eram segurados por um portao que ja existe, e o quarto passava.

**A ORIGEM.** O portao perguntava *"existe alguma celula na competencia?"*. col882 tem **30 celulas, 15 com
`trabalha=True`** -- e **previsto 0 em todos os 30 dias**, porque o juiz do previsto pergunta ao VINCULO
antes da celula e o unico vinculo dele (EC#1059) encerrou em **06/09**. As celulas sao de vinculo morto:
`gerada_em` 21/08 e 21/09 05:50, geradora 1059, 44 delas depois do proprio `data_fim`. Nao e bug da
geradora -- `ponto/portas/celula.py:474` ja faz `if ec.data_fim and d > ec.data_fim: continue` --, e sim
`data_fim` recuado DEPOIS do plantio. **Forma medida na frota: 2.986 celulas alem do `data_fim` da propria
geradora, em 102 colaboradores.** Dono = CADASTRO (L-099), cura declarada = `regenerar_celulas_vinculo`;
**nao e fatia de codigo**. Mas "existe celula" seria a pergunta errada mesmo com geradora perfeita.

**E A LEI JA EXISTIA (LEI-AKITA 4).** `ponto/services/dia_pago.py:597` e
`colaboradores/tests/test_calendario_le_dia_pago.py:15` nomeiam col882 ao lado de col43 e col924 "da classe
`cadastro_zero`/folha-zero" desde 29/09. A casa ja o havia classificado; o portao era o leitor que nao
migrara. As duas prosas definiam a classe como "sem nenhuma celula" -- falso para col882 -- e foram
corrigidas no mesmo commit.

**A CURA.** A premissa passa a ser o PREVISTO da janela, lido do juiz declarado, com `escalas=` (a
alimentacao declarada: a lista INTEIRA de vinculos) e parada no primeiro dia previsto; o teto de `>= 2`
batidas passou para ANTES, para que a pergunta de 30 dias so se faca a quem ja provou jornada com um par.
O `detalhe` passou a dizer a FORMA (sem celula nenhuma x celula de vinculo encerrado, com a data), porque o
DP cadastra coisas diferentes nos dois casos. `batidas_sem_celula` -> `batidas_sem_previsto` (1 leitor, o
proprio selo). Nenhuma tolerancia mudou, nenhum caso saiu da lista, nenhum fallback, nenhum juiz novo.

**DIFF DE FROTA pela funcao real** (`classificar_export`, sombra, HEAD 774447cf x cura, emp 2/3/4):
```
09/2026-emp2  entra 147 -> 147  | movem: 0      10/2026-emp2  entra 149 -> 148  | movem: 1
09/2026-emp3  entra  58 ->  58  | movem: 0          col882  entra/None -> fora/cadastro_zero
09/2026-emp4  entra  12 ->  12  | movem: 0      10/2026-emp3  entra  67 ->  67  | movem: 0
                                                10/2026-emp4  entra   9 ->   9  | movem: 0
TOTAL de linhas que movem = 1
```
A **09 exportada fica intacta** nas tres empresas. Ninguem passa a ser PAGO por isto: col882 deixa de
receber um TXT de zero e vai ao lote seguinte, com o cadastro nomeado -- que e o tratamento que o proprio
`detalhe` do portao ja prometia desde 28/09.

**O QUE NAO E ESTA FATIA, e esta medido:** col366, col948 (emp2) e col30 (emp4) tem previsto **9.900 min** e
`trabalhadas 0,00` com 3-4 batidas em 1-2 dias -- forma DIFERENTE (o cadastro descreve o mes), nao movem no
DIFF, e nao foram tocados. Fica nomeado para o BACKLOG.

**O UNIVERSO DO DIFF, dito por inteiro:** `classificar_export` julgou **1.173** colab-competencia nas seis
janelas (459+123+22 na 09; 431+117+21 na 10) -- o DIFF compara TODOS, nao so os que entram, e e por isso
que "movem: 0" na 09 vale como prova de que a exportada nao se mexe. Prova duravel:
`logs/placar_estrutural/r4_cadastro_zero_diff_20261004.txt`, gerada dos dois JSON em
`logs/simular_folha/{head,cura}_r4.json`.

**R4 SEGUE PARCIAL, e isso e deliberado.** A cura fecha UMA das clausulas do RED (c) -- a de que
`classificar_export` dizia ENTRA. O que fica, com dono:
- os **6 dias** de espelho x DiaPago do col882 (21,23,25,27,29/09 e 01/10) seguem: o dono e **CADASTRO**
  (vinculo encerrado nao cobre a janela) e a **L-099 proibe curar CADASTRO por codigo**. Classificar certo
  nao faz o dia ser pago -- nem deveria;
- **col935** (1 na 09, 8 min por marco de template fora do vinculo) e **ESTRUTURA**, mas esta na competencia
  **EXPORTADA**: LISTA, nao fatia, enquanto a 09 nao reabrir;
- **os TRES itens de CODIGO que o R4 nomeia seguem abertos**, e nenhum fechou hoje: as **duas portas**
  (demissao e encerrar vinculo alcancarem celula e fechamento -- escrevem celula de demitido, logo `!` de
  ESCALA); o **tripwire** "ativo com batida e previsto 0", que pela secao 4a e um CONTADOR de frota com
  esperado 0 e **nao nasceu** -- o que nasceu hoje e RETENCAO em UM leitor, nao varredura; e o **contador**
  que le testemunha de FORA do gravado. Esta cura fecha UMA clausula do RED (c), nao um item.
Por isso o `estado` de R4 nao muda e o placar do TICKETS segue "3 fechado(s), 3 parcial(is)": o numero e a
prova foram reescritos no MESMO commit que a cura, que e a regra da casa -- quem atualiza e quem mede.

**SUITE INTEIRA, na raia:** `Ran 9570 tests in 1177.271s` / `OK (skipped=42)` / rc=0. Os dois numeros
que mudaram contra o push de 03:52 estao explicados, e isso importa porque numero inexplicado e sinal
perdido: o **+1** (9569 -> 9570) e exatamente o caso RED novo -- o arquivo de teste ja existia com 6
metodos no HEAD --, e os **1177 s contra 612 s** sao disputa de CPU com a suite da raia-chamado no mesmo
cpuset 4-7. Lentidao, nao colisao: ela roda em `REGUA_DB=test_juliani_raiachamado`, banco PROPRIO
(`config/settings/ci.py:14-17`). Ver a secao da trava, acima, para o que essa convivencia revelou.

**ESTADO.** Commitado na raia `raia-r4` como `98924ef0` (de 774447cf). **O pouso espera o mesmo `!` do O142** (`janela_auth`,
topo deste RELATO): merge -> commit -> `bin/deploy.sh` em UM ato, porque o merge escreve dezenas de `.py` na
arvore que E o bind-mount (licao de 30/09, 11 min de janela quebraram o lote de cartoes em prod).

A **dieta de prosa** avisa: as secoes de **01/10** completam 3 dias hoje e passam do teto
AMANHA -- `RELATO-ARQUIVO.md` no mesmo commit em que cruzarem, nao antes.

## 04/10 03:5x — ACHADO MEDIDO: A TRILHA NOMINAL DO O85 E MUDA EM PROD (O181)

**O que a casa acredita.** `ponto/services/fechamento.py:481` diz, em comentario proprio:
*"a trilha e' nominal: um mes depois, 'por que este colab ganhou horas?' tem resposta."*
Dois sitios escrevem essa trilha com `logger.info`:
- `:384` `o85_dia_sem_vinculo_em_trabalhadas colab=... dias=... min=...`
- `:482` `folga_sem_escala_em_trabalhadas colab=... periodos=... min=...`
Ambos carimbam horas que ENTRAM em `horas_trabalhadas` -- e dinheiro.

**O que esta medido.** Perguntado ao `logging` DENTRO do `saas_core`, com as settings de prod:
```
isEnabledFor(INFO) = False
handlers na cadeia = []      root: []
nivel efetivo      = WARNING
```
`config/settings/saas.py:119-150` declara handlers (`console`, `errfile`) **so** para
`django.request` e `django`, ambos em `ERROR`, e **nao declara root logger**. Logo todo
`logger.info` de codigo de app cai no "last resort" do Python, que so emite WARNING+.
Confirmado pelos dois lados: **0** ocorrencia das duas frases em `logs/`, e **0** em 6 h
de `docker logs saas_core`.

**Por que isso importa.** A L-007 pede *"contador E trilha"*. O contador existe; a trilha
nao. Quando alguem perguntar "por que este colab ganhou horas?", a resposta que o
comentario promete nao existe em lugar nenhum -- e a familia do "silenciar exige prazo +
tripwire + item de fila", com o agravante de que ninguem silenciou de proposito: a trilha
nasceu muda.

**Dono: ESTRUTURA** (o sistema nao guarda o que diz guardar). **Nao e esta fatia** -- cura
e um handler para INFO em arquivo proprio (ou `LogAuditoria`, que e o que a casa usa para
acao critica e sobrevive a rotacao). Vai ao BACKLOG com este numero.

**Censo do mesmo defeito, para a cura nao ser meia:** ha `logger.info` tambem em
`chamados/fcm_utils.py`, `chamados/signals.py`, `chamados/services/disputa_emissao.py` e
`escala/services/cadastro_tipo.py` -- todos igualmente mudos hoje. Quem curar o handler
cura os cinco de uma vez; quem trocar sitio por sitio faz meia-correcao.

## 04/10 03:0x — DOIS SELOS QUE MENTIAM POR MOTIVOS OPOSTOS, E UMA CORRECAO MINHA DE ESCOPO

Os dois pousaram enquanto a pista estava tomada, porque nenhum deles precisa de pista: sao selos de
HOST. E os dois sao a MESMA doenca com sinais invertidos -- **ausencia de sinal lida como sinal bom**.

**(1) `9102af37` -- o selo da trava exercitava o lock GLOBAL.** `bin/tests/test_trava_teste.sh` dirigia o
`bin/trava_teste.sh` de verdade, cujo `TRAVA=/tmp/juliani_db_test.lock` esta CRAVADO em `:21`. Logo o
veredito dele dependia de a maquina estar vazia: **provado em UM so estado de maquina**, com
`raia-chamado:873573` segurando o lock -- contra o lock global a `ordem` saiu `''`, contra a copia
isolada saiu `'A A_fim B '`. Nao era flake: era selo nao-hermetico. A cura insere uma copia
reapontada (`sed` na linha `TRAVA=`) antes do primeiro MORDE e manda os quatro sitios para ela.
**A guarda ABORTA em vez de so contar falha**, e isso nasceu medido: com `_falha` (que soma e SEGUE),
a copia nao-isolada ia exercitar o lock global e **PENDUROU** ate o `timeout 90` matar (rc=143) --
numa regua, pendurado e pior que vermelho, porque vermelho se le. Nao criei override por variavel de
ambiente de proposito: seria abrir costura num dispositivo de seguranca.
**PROVA do avesso, hoje as 02:17 contra as 02:5x**: na pasta de selos de 02:17 ele estava VERMELHO
(`a trava nao serializou: ordem foi 'A A_fim '`) -- 22 min ANTES da cura. As 02:5x, com o lock ainda
tomado por `juiz-de-chamado-C1aC4:875154`, ele roda **OK rc=0**. O vermelho de antes era a maquina
ocupada; o verde de agora e o selo.

**(2) `6acc825a` -- a janela de 900 commits envelheceu, e o residuo voltou a segurar alvo.**
`bin/fabricante_alvo.py::ja_commitadas()` lia `git log --pretty=%s -900`. O `-900` e um LITERAL que
envelhece: num repo de **3641** commits ele alcancava so ate **26/09 12:22**, entao `ja_commitadas()`
devolvia **0** enquanto **6** pacotes tinham a fatia no log. O mais cruel: `PRE-PUSH-TESTA-O-COMMIT`
(`4c081b17`, 26/09 12:11) ficou fora por **onze minutos**. Os outros **41** nunca foram commitados e
estao retidos com razao. E os meus proprios commits de hoje EMPURRARAM a borda -- e o BO de 23/09
19:1x voltando **pelo relogio, nao pelo codigo**. Cura RED->GREEN: `ja_commitadas()` **0 -> 6**,
`em_obra()` **120 -> 111** (nove arquivos liberados). Custo medido do log inteiro: **41/41/41 ms**
contra **18 ms** do `-900` -- com `date +%s%N`, porque `/usr/bin/time` nao imprimiu nada e eu nao ia
declarar numero nao medido. A variante por mtime foi MEDIDA e **rejeitada**: 4 dos 6 tem
`msg_commit` com mtime 01/10 15:21, POSTERIOR ao proprio commit. Os dois sitios mudam juntos
(`app/core/esteira_vigia.py:283` tinha o mesmo `-900` no fallback de `titulos is None`), para a
"UMA FONTE" do aval de 25/09 10:3x nao virar uma janela larga e uma estreita.
**DANO HOJE: nenhum**, e isso esta dito em vez de descoberto depois -- `esteira.pausada` e a pausa
DELE e o fabricante nao esta sorteando.

**(3) O selo que acusou o (2) estava certo, e o defeito era meu -- mas a perna dele morreria na
faxina.** `bin/tests/test_fabricante_seco.sh` tinha `jc = orig(); if not jc: FALHA`: pegou o bug de
hoje e morreria no dia em que `.esteira/` fosse limpa, que e a doenca que o comentario dele mesmo de
24/09 descreve (*"selo que depende de haver sujeira na arvore para provar que sabe limpar morre no dia
da limpeza"*). Virou caso SINTETICO de dois valores em `tempfile.mkdtemp()` -- `ja_subiu` /
`nunca_subiu` --, ancorado no rotulo **MAIS ANTIGO** da historia (`B2`, a **2333** commits do HEAD).
A ancora e deliberada: com o rotulo do HEAD (`O142`, DENTRO da janela) o caso passaria com o `-900`
de volta, isto e, **nao morderia o bug de hoje**. Mordida provada: repus o `-900` so em
`fabricante_alvo.py` -> rc=1 `devolveu [] (esperado {'ja_subiu'})`. A populacao real virou
INFORMACAO impressa, nunca veredito.

**(4) O que eu NAO curei, e por que: O176.** O criterio "o rotulo esta num titulo do git" nunca
responde "esta fatia subiu" -- `TICKETS` aparece 26x, `PLACAR-ESTRUTURAL` 11x, `O142` 10x; **592
rotulos distintos em 821 ocorrencias**. O juiz certo compara CONTEUDO (`declarados()` contido na
arvore) e isso e **AUTORIDADE NOVA**, logo espera corte (TRAVA JUIZ-NOVO). Dano hoje: **1 de 47
repetidos**, e esse pacote e residuo legitimo -- o 1 e a guarda do item.

**(5) CORRECAO MINHA DE ESCOPO, no O173 -- eu havia escrito uma ordem dele do avesso.**
A minha redacao de 01:5x dizia *"o selo do import tardio tem a DIRECAO INVERTIDA"* e propunha
inverter a pergunta para ar->disco. **Esta errada contra ordem LITERAL**, que o proprio selo cita no
cabecalho: *"Selo que morda (import tardio de simbolo que o HEAD carregado nao tem)"* -- disco contra
ar E a direcao pedida --, e `bin/tests/test_import_tardio_contra_o_ar.sh:14` fecha com *"A CURA,
quando ele acusa, e `bin/deploy.sh` -- alinhar disco e memoria. **Nao se ajusta o selo.**"*
Item reescrito. O que fica de pe, medido agora: ele acusa **1** de **4214** imports tardios --
`api/views_mensageria.py:1867`, porque `ponto.turno_leitura` (modulo NOVO do meu `5bdf439c`) nao esta
em `e49a8289`. Esse RED e **CONSERVADOR**: modulo que o worker nunca importou, o runtime resolve do
DISCO, onde ele existe. O 500 de 01/10 era outra coisa -- **SIMBOLO** faltando em modulo **ja em
memoria**. E ele segue **MUDO** no ar->disco, a metade que deploy nenhum cura. Nada disso se toca
antes do deploy de segunda.

**(6) A CONSEQUENCIA PRATICA, dita porque ela muda o portao do push:** com esse selo vermelho,
`bin/regua.sh` **nao alcanca a suite** (`REGUA VERMELHA: selo de host <nome> -- suite nem rodou`), e o
`.regua_stamp` vivo e de **29/09 09:44** com `REGUA_FP=8dc1bb66` contra arvore **`f983fd3e`** -- isto
e, **os 16 commits a empurrar nao tem veredito de suite**. Eu nao ajusto o selo e nao escrevo selo
falso: a cobertura da regua se reconstroi em PARTES, cada uma com o seu numero -- pasta de selos de
host **59 rodados, 1 vermelho real** (este, cura=deploy), e a suite pelo **comando canonico da secao
3** sob `bin/trava_teste.sh`, enfileirada atras das duas que estao na frente. **O `.regua_stamp` NAO
sera escrito por essa rodada**, e e' por isso que esta linha existe.

**(7) As duas raias.** A do SEGUNDO-INTERVALO **ja esta em `origin/main`**: `d36ae038` e ancestral,
e nao esta entre os 16 a empurrar -- o `5bdf439c` do O142 tocou `turnos.py` DEPOIS dela. O
INCOMPLETO que ela declarou (*"a suite dos 5 apps nao foi re-rodada inteira depois de `d36ae038`"*) e
coberto pela rodada de 13 labels que esta na fila, porque ela roda contra a arvore que JA contem o
`d36ae038`. A outra (`a857`, O167 C1-C4) terminou o turno **esperando a pista**: o `docker run` dela
segue vivo (35 min, worktree `agent-a857c1bc9c86415ff`, 13 labels SEM `--parallel`) e o veredito cai
em `scratchpad/green_tudo2.out` -- e meu recolher, por ARQUIVO.

**(8) E O ACHADO QUE MUDA O RELOGIO DESTE TURNO: "espero segunda" era FALSO para o disco.**
O O174 dizia que a divisao disco/memoria duraria **28 h, ate segunda 06:00**. Errado, e medido
agora: o cron das **03:30** chama `deploy.sh --reload-agendado`, que e *"o mesmo reload + a mesma
prova de rota"* (`bin/deploy.sh:39-41`) e cuja UNICA guarda e migration pendente
(`:95-99`, pelo EXIT CODE do `--check`) -- que da **PEND=0**. Logo a divisao acabaria em **~23
min**, por timer, sem ninguem olhando, publicando a +1 rota de `api/urls.py` que eu havia posto
no topo do RELATO esperando o `!` dele. A minha propria memoria dizia isso (*"o HUP das 03:30
publica a arvore viva; segurar cura commitada e falso"*) e eu escrevi "segunda" duas vezes assim
mesmo. **Desarmado as 03:07** (ver topo). `deploy.sh` nao honra arquivo de pausa nenhum e
`bin/cron_run.sh` (38 linhas) nao tem mecanismo de pausa -- conferidos --, entao a linha do
crontab era o unico portao real. Nenhum selo de host cobra essa linha
(`test_alarme_vigia_a_sessao.sh` so cobra a do `alarme_sessao_ociosa`), conferido antes de mexer.

**(9) E A PARTE DO (5) QUE EU AFIRMEI SEM MEDIR, agora medida.** Eu escrevi que o RED do import
tardio e "CONSERVADOR" porque modulo nunca importado resolve do disco -- verdade, mas isso vale
para o ALCANCE. A pergunta de CLASSE e outra: o modulo novo, ao ser importado fresco, pede algum
simbolo que o ar nao tem? Medido em `ponto/turno_leitura.py`: **0 imports de topo** (todos os 5
estao dentro de funcao) e, dos tardios, os **3 locais** resolvem no ar --
`colaboradores.models.Colaborador`, `ponto.janelas.janela_atual` e `ponto.turnos.turnos_do_colab`,
os tres presentes em `e49a8289`. Os outros 2 sao `django.utils`, fora do universo do selo
(`_locais()` = pacotes de `app/`). Entao o RED e conservador em alcance **e** em classe, para
ESTE modulo -- e isso e o que a minha frase de 01:5x nao tinha direito de dizer ainda.

## 04/10 02:3x — QUATRO PERGUNTAS DE LEI, COM O NUMERO DE CADA (nao devolvem turno)

A esteira seguiu: os dois commits do turno pousaram (`b8f51891` residuos como ITEM, `1ed33f31` cura do
hook) e nenhuma destas quatro trava a fila. Elas estao aqui porque a lei manda a pergunta subir **com
numero**, nao porque eu esteja esperando.

**(1) TM e "gravado" para a L-092, ou e cache que pode ser reescrito em mes pago?** (item O175)
`materializar_turnos.py:13` abre `--dias default=60` e :23-24 recomputa TODO colab: 60 dias atras e
05/08, logo a **08 e a 09** -- as duas exportadas. Guarda: **nenhuma**, e a docstring :2 convida
(*"re-rodavel a vontade"*). Medido na sombra: **4.848 linhas de TM em competencia EXPORTADA** se moveriam
(no HEAD seriam **4.981** -- o backfill e perigoso nos dois lados; a cura muda QUAIS linhas, nao SE move,
e em jul/ago ela *preserva* turno do col165 que o HEAD *apaga*). O censo que decide o tamanho da
pergunta: **TM tem 0 leitor** em `relatorios/`, `folha/`, `holerite/` e no motor (busca direta, rc=1) --
reescrever TM **nao** move espelho, PDF nem TXT. Por isso eu a leio como coerencia de cache e **nao**
toquei no comando. Nao tem cron, provado nos dois lados (`config/crons.py:1136` + 0 em `crontab -l` e
`/etc/cron.d/*`).

**(2) Quem garante que o GRAVADO segue a ATA quando nao vem evento novo?** (item O171)
Medido as 02:3x: `lavrar_veredito` -- a porta que a relavratura do O142 usou -- **nao** dispara
`recalcular_por_evento`. Quem dispara sao quatro atos: a BATIDA (`ponto/registro_batida.py:137`), a
decisao de HE (`portas/he.py:130`), a validacao de disputa (`validacao.py:107`) e
`regenerar_celulas_vinculo` (`portas/celula.py:318`). E a folha **nao tem cron de proposito** (corte seu
de 24/09, `config/crons.py:891-897`: *"cronificar seria o sistema reescrevendo a folha sozinho na
madrugada, e o carimbo atualizado_em deixaria de significar 'alguem mandou'"*). O efeito: o col369 tem
ata nova (+189 min) e gravado velho, e ele converge no **1o evento depois do deploy** -- mas um colab
**desligado ou de ferias** nunca recebe evento naquela competencia, e entao nao converge nunca. A
pergunta nao e se o cron volta (o seu corte ja disse que nao): e **se a relavratura deve ser ela mesma um
dos atos que disparam o recalculo**. Nao liguei: seria escritor novo no caminho do dinheiro.

**(3) `MINUTOS_DIA_INTEIRO = 480` dentro do juiz, ou o previsto REAL do dia?** (esmeril AUSENCIA, O137)
O 480 veio cravado do aval de 19/09 ("so o contador") e segue dentro de `cobre_parte_do_dia`. O previsto
real tem juiz declarado -- `escala/utils.py::minutos_previstos_do_dia` --, e trocar um pelo outro **move
o numero de um placar que tem aval seu**. A raia nao trocou, de proposito.

**(4) O GLOSSARIO da familia nasce com a O98 congelada?** (esmeril AUSENCIA)
`app/docs/GLOSSARIO.md` **nao existe** e a O98 esta **CONGELADA pela L-096** (`BACKLOG.md:102`). Entrada
de glossario sem arquivo e obra nova, nao esmeril: ficou incompleta, nomeada, sem item escondido.

## 04/10 02:0x — O142: AS QUATRO CONDICOES MEDIDAS **ANTES** DO APPLY, E UMA PERGUNTA DE LEI

PERGUNTA DE LEI (nao devolve turno — a esteira seguiu): **a `impressao_insumos` passa a cobrir a
ATA?** Hoje ela hasheia batida/cobertura/chamado/DNA e **nao** a ata (`ponto/services/cartorio.py:93-105`,
lido ao vivo). Consequencia MEDIDA: a cura do O142 muda o PAREAMENTO das mesmas batidas -- mesmos pks,
mesmos tipos, mesmos timestamps, mesmo DNA --, entao a impressao fica **byte a byte igual** e
`cartorio.py:568` manda as celulas para `pulados`. **O cartorio nunca alcanca a ata que a cura corrigiu.**
Nao e "para sempre": a impressao se refaz quando um INSUMO daquele dia muda (batida nova ou retratada,
`chamado.status_local` por uma resposta, cobertura, `dna_versao`/`marcos`). Para um dia passado e
encerrado, isso pode ser nunca. As duas saidas: (a) a impressao passa a cobrir a ata, e **toda** cura de
geometria rejulga a frota uma vez, de proposito; ou (b) a ata velha e aceita como foto do que se sabia no
dia, e a relavratura e sempre ato nomeado. **Nao escolhi.** O mesmo corte esta aberto desde 21/09 na
lapide 62 (`RELATO-ARQUIVO.md:14733`): *"corte Ronald: a porta da celula re-carimba (ou: espera o cron
das 06:28)"*.

### 1. O ENSAIO NA SOMBRA BATEU EM TUDO (01:46:14, carimbo `SOMBRA_STATUS=OK`)
Sombra do dia (`SOMBRA_DIA=20261004`, `DIVERGE=0`, `ERROS=0`, 68 comandos, bloco inteiro).
Relavratura escopada por `cartorio.julgar_celula(colab, data, apply_=True, forcar=True)`, 3 celulas:

| pk | data | `ata.minutos_realizados` | ensaio | as 7 chaves |
|---|---|---|---|---|
| 112615 | 2026-09-23 | 359 | **-> 419** | BATE, lampadas inclusas |
| 112618 | 2026-09-26 | 345 | **-> 414** | BATE, lampadas inclusas |
| 112619 | 2026-09-27 | 361 | **-> 420** | BATE, lampadas inclusas |

`chamados do col369: antes=84 depois=84 delta=0` — **e relavratura, nao emissao**. Competencias
**08 intacta** (`8cbfa9f4cc690f78`) e **09 intacta** (`b3d04d4d322e9cfa`); so a 10 mexeu
(`d00a154334f15a37 -> 7374540b88ce3d12`).

**O RED estrutural esta no proprio ensaio:** `imp=` sai **identico antes e depois** nos tres dias. E a
prova empirica de que sem `forcar=True` o cartorio PULA estas celulas — a pergunta de lei acima nao e
teorica, e o que o ensaio mediu.

### 2. PRE-CHECK QUE TRANSFERE O ENSAIO PARA PROD (02:0x) — **3 de 3**
A previsao do ensaio so vale se os INSUMOS de prod forem os mesmos do dump. Medido em prod:

| pk | data | `impressao`[:16] prod | sombra | |
|---|---|---|---|---|
| 112615 | 2026-09-23 | `0be3fb775b377c5e` | `0be3fb775b377c5e` | BATE |
| 112618 | 2026-09-26 | `f95bb2f3da94fb3e` | `f95bb2f3da94fb3e` | BATE |
| 112619 | 2026-09-27 | `f95bb2f3da94fb3e` | `f95bb2f3da94fb3e` | BATE |

**Insumos byte a byte iguais — a previsao transfere exatamente.** A primeira rodada desta sonda disse
`iguais: 0 de 3`, e era **bug da minha sonda**, nao do dado: eu comparava o campo CHEIO (64 chars)
contra o literal TRUNCADO (16) que a sombra imprimiu. Fica como a 8a ocorrencia da linha do CLAUDE.md
*"sonda mal parametrizada foi lida como bug do sistema 7x"* — e a unica que nao foi reportada como tal.

**ACHADO DE CARONA:** `f95bb2f3da94fb3e` e' a impressao de **DOIS dias diferentes** (26 e 27/09). Nao e
colisao de hash: `impressao_insumos` nao inclui a DATA nos `partes`. Dois dias com as mesmas batidas
relativas e o mesmo DNA imprimem igual. Nao muda nada aqui (a impressao e comparada por celula, e a
celula ja e do dia), mas e' item proprio: **a impressao nao identifica o dia que a gerou**.

### 3. AS TRES PKS **NAO ESTAO NA 09**. ESTAO NA 10.
Isto reenquadra o risco da fatia inteira, e nao era obvio — as datas sao de setembro. Perguntado ao
juiz (`ponto/janelas.py::janela_atual`, a funcao real, nunca o 21 cravado):

> pk=112615/112618/112619 -> **competencia 10/2026** (janela **2026-09-21 .. 2026-10-20**)

A competencia da folha corre do 21 ao 20. Os tres dias caem **depois** do dia 21/09, logo pertencem a
**10/2026, que esta ABERTA** (`status=aberto`). O PROIBIDO literal do aval — *"tocar a 09 exportada"* —
nao e' apenas respeitado por cuidado de escopo: **os dias nao estao na 09**. Por isso a relavratura cai
na **DINHEIRO-EM-COMPETENCIA-ABERTA**, pre-aprovada com as quatro condicoes, e nao na L-009.

### 4. AS QUATRO CONDICOES, COM O ARQUIVO DE CADA UMA
| # | condicao | evidencia |
|---|---|---|
| 1 | DIFF publicado **ANTES** do apply | **esta secao**, commitada antes de qualquer escrita |
| 2 | reversao em `logs/` | `o142_reversao_ata_10_2026.json` (3 pks x **9 chaves**, `faltando: []`, volta **pela porta** `lavrar_veredito(ata=)`) + `o142_reversao_fechamento_10_2026.json` (572 colabs x 25 campos) |
| 3 | competencia exportada INTACTA, hash antes/depois | as 3 VIGENTES da 09: emp2 `361d0f9685f86d3a`, emp3 `5c503b95f9f9cd35`, emp4 `84c78cd0871f5f52` |
| 4 | PROVA depois | proxima secao, commit separado |

**A reversao de 23:12 nao esta velha** — conferida contra prod agora: **0 dos 25 campos divergem**. Isso
tinha de ser medido, porque `recalcular_por_evento` rega a 10 a cada batida; reversao velha e' reversao
que restaura o passado errado.

**Foto do GRAVADO antes** (`logs/o142_gravado_ANTES_col369.json`), **37 campos por `_meta.fields`**, nao
lista escrita a mao — a licao da AVAL-DE-CRITERIO e literal (*"apply por recalculo nunca e cirurgico"*):
`FM 09/2026 status=aberto hash=54ac296d22e413ac` · `FM 10/2026 status=aberto hash=889b7e6e3040b038`.
A 09 do col369 tambem esta **aberta** — o que significa que nenhuma trava a protege, e so a medicao
protege. Ela tem de sair do apply com o **mesmo** `54ac296d22e413ac`.

### 5. O PORTAO `janela_auth` ESTA CERTO, E EU IA CHAMA-LO DE FALSO POSITIVO
`bin/deploy.sh --sem-migrate` foi barrado: domingo, e o diff toca `app/api/urls.py`, que esta em
`bin/auth_sitios.txt`. A mudanca e **minha e aditiva** (`5bdf439c`, +1 rota de diagnostico
`mensageria/turno-invariante/`) e nao toca view de login — eu ia publicar isso como falso positivo.
**Errado:** se `api/urls.py` falhar ao importar, **todo o `/api/` morre, auth incluida** — que e'
exatamente a forma do P0 de 20/09. O portao mordeu o que ele existe para morder. `--forcar` num domingo
as 01:5x seria reproduzir o incidente de proposito. Fica para segunda 06:00 (ou o `!` do topo).

**E o "500 agendado" do import tardio NAO esta armado.** Medido: o import de `turno_leitura` em
`api/views_mensageria.py:1866` e **LAZY** (dentro da view), o modulo esta no disco, e o ar nao tem nem a
linha nem a rota. Mas o selo que vigia isso tem a **direcao INVERTIDA**: ele compara os imports do DISCO
contra os modulos do AR, entao diz "deploy pendente" sempre e ficaria **MUDO** no perigo real — import
tardio no codigo **do ar** apontando para modulo que **saiu** do disco. Item proprio; **nao toquei o selo
nesta fatia**.

**CRON E WORKER JA DIVERGEM, e isto e' consequencia nao documentada do BUG 128.** Provado, nao inferido:
processo novo no `saas_core` ve `ponto/turnos.py` em md5 `05a22f22b759ccadfb1e96ae4981bca2` (o md5 da
**cura**), enquanto o gunicorn carrega `119922ee6a1be779d28898495d38de86`. A divisao corre desde 01:06 e
dura ate segunda 06:00 = **28 h**. Aqui e' inofensiva — o diff do O142 e aditivo e, pelo salto do
cartorio, nenhuma celula existente se move sozinha — mas a casa nao tem isso escrito em lugar nenhum:
**o cron le o disco, o worker le a memoria, e entre um deploy e outro eles sao dois sistemas.**


### 6. O APPLY, FEITO E PROVADO — **02:08:19**, condicao 4
PROVA: `logs/o142_PROVA_depois.json`, relido em processo NOVO as 02:09:03, e o
`md5(ponto/turnos.py)` visto por ESSE processo = `05a22f22b759ccadfb1e96ae4981bca2` (a cura).
Um unico ato, `transaction.atomic()` nos tres pks, com o PRE-CHECK **dentro** da transacao e
`raise` -> rollback em qualquer falha. Nao foi "rodar e conferir depois": o script se recusaria a
commitar. `logs/o142_PROVA_depois.json`, lido de novo em processo NOVO as 02:09:03.

**Qual codigo julgou, provado e nao suposto:** `md5(ponto/turnos.py)` visto por ESTE processo =
`05a22f22b759ccadfb1e96ae4981bca2` = a **cura**. O gunicorn segue em `119922ee…` ate o deploy.

| pk | data | `ata.minutos_realizados` | previsto pelo ensaio | persistiu |
|---|---|---|---|---|
| 112615 | 2026-09-23 | 359 **-> 419** | 419 | BATE |
| 112618 | 2026-09-26 | 345 **-> 414** | 414 | BATE |
| 112619 | 2026-09-27 | 361 **-> 420** | 420 | BATE |

`via=cartorio`, `veredito_em=2026-10-04 02:08:19`, 4 lampadas em cada. **3 de 3**, conferido por
leitura nova depois do commit.

**E relavratura, nao emissao** — as asserções que teriam feito rollback:
`emitidos=0` · `emitiria_furo_parcial=0` · `carimbadas=3` · `julgadas=3` · `pulados=0` ·
**chamados 84 -> 84, delta 0**.

**L-092 / condicao 3, com hash antes e depois:** `FM 09/2026` = `54ac296d22e413ac` **INTACTA**; as tres
exportacoes VIGENTES da 09 **intactas** (emp2 `361d0f9685f86d3a`, emp3 `5c503b95f9f9cd35`,
emp4 `84c78cd0871f5f52`). A 09 do col369 esta `aberto` — nenhuma trava a protegia, so a medicao.

#### 6a. O RESIDUO, MEDIDO E NOMEADO — a ata andou, o GRAVADO nao
`FechamentoMensal 10/2026` saiu do apply com o hash **identico** (`889b7e6e3040b038`, **0 dos 25
campos**): `minutos_realizados` segue **2310**, enquanto a ata dos tres dias somou **+189 min**.
Nao conserto a mao e **nao pego carona neste apply**: a licao da AVAL-DE-CRITERIO e literal — *"apply
por recalculo nunca e cirurgico"* —, entao `recalcular_fechamento_mes` traria a DERIVA do FM junto e
precisa do seu proprio DIFF na sombra. **Dono da divergencia pela L-099: ESTRUTURA**, e e' o
*"1 passo manual"* do R6. Item proprio.

#### 6b. A JANELA DE 28 h QUE EU ABRI, E ELA E REAL
A ata esta certa; a **tela nao le a ata**. O espelho deriva `minutos_realizados` do motor
(`relatorios/services.py:136`, `pdf_espelho.py:768` — o drill le o mesmo numero que o papel, L-095), e
o motor roda **no gunicorn**, que tem o `turnos.py` velho ate segunda. Logo, por ~28 h, **3 dia-colab
do col369 tem ata nova e tela velha.** Nao e' dano e nao reverte nada (competencia aberta, reversao em
`logs/`, a ata e' a fonte), mas e' exatamente o tipo de divergencia que esta casa nomeia em vez de
descobrir depois — e **afia o `!` do topo**: forcar a `janela_auth` fecharia a janela hoje; esperar
segunda 06:00 a fecha sozinha.

**A impressao segue IDENTICA depois do apply** (`0be3fb77…`, `f95bb2f3…`, `f95bb2f3…`). Isso e' bom
aqui — o cron das 06:28 nao vai desfazer nada, porque continua pulando estas celulas — e e' a mesma
pergunta de lei do topo, agora com a resposta medida nas duas direcoes: o salto protege a cura
**e** impediria a proxima.

## 04/10 00:5x — OS DOIS NUMEROS DE PROD QUE FALTAVAM, E UMA PERGUNTA DE LEI

PERGUNTA DE LEI (nao devolve turno — a esteira seguiu no O142): **`corte Ronald: juiz
dia_encerrado nasce`**. Sem essa frase no CORTES.md o `flip_automatico` nao pode delegar
"o dia esta encerrado?" e ficam **2 derivadores paralelos da mesma pergunta**:
`ponto/services/flip_auto.py:274` (`if dia >= hoje`) e `ponto/supra_juiz.py:149`
(`dia_encerrado = not (hoje and data and data >= hoje)`). Quem escreve `flip_tipo` e
escritor unico, entao a pergunta decide o universo de um cron de **--apply**. Veio da raia
do O139 (commit `00bd05fb` na worktree dela, suite 9558 OK, `juizes_por_varredura` 27 -> 25).

### 1. O DEGRAU 2 DO LASTRO FECHA **ZERO** HOJE
Medido em prod as **00:49:55 de 04/10**, chamando as funcoes reais
(`ponto/services/lastro.py::julgar` e `::_lampada`, `chamados/catalogo/motor.py::VIVOS`):

| o que | numero |
|---|---|
| fila viva `batida_ausente` | **1309** |
| com marco E tipo nomeados | 1298 |
| morrem no **DEGRAU 1** (a lampada responde) | **1298** |
| **chegam ao DEGRAU 2** | **0** |
| nem entram (sem marco declarado) | 11 |
| `com_lastro` agora | 0 |

A serie horaria (`logs/lastro.log`, **560 rodadas** desde 10/09 17:40) diz que o degrau 2
**ja falou muito e parou**: `(b) bateu em outro horario` — veredito que SO o degrau 2
produz — comecou em **147** (10/09 17:40), apareceu em **226 das 560 rodadas**
(soma 3047 chamado-rodada) e o **ultimo ≠0 foi 01/10 05:40, com 1**. Esse e o numero que
responde "o degrau 2 ainda fala?": ele falou ate 01/10 e esta em zero desde entao.

**O QUE EU NAO SEI, e quase escrevi como se soubesse.** Dos **83** chamados que o fechador
ja fechou (trilha `BUG 114` no fio), **nao da para dizer por qual degrau cada um fechou**:
a unica testemunha que sobrou e a ata de HOJE, e ela pode ter sido re-lavrada depois do
fechamento. A leitura de hoje, com as caixas separadas e sem veredito em cima delas:
58 tem lampada para todos os marcos cobrados · **21 nao tem marco cobrado nenhum** (foram
fechados por uma versao do fechador anterior ao `SEM MARCO NAO FECHA POR COINCIDENCIA` —
hoje nao fechariam) · 4 tem celula mas o marco cobrado nao tem lampada. Minha primeira
conta somou essas caixas como "79 degrau 1, 4 degrau 2": era **ausencia de sinal lida como
sinal**, nas duas pontas (`cobrados == []` cai em "lampada respondeu" por vacuidade).
**Nao toquei o arquivo**: `fechar_cobranca_com_lastro` esta na sua lista
*"ESPERAM LEI MINHA, nao tocar"*.

### 2. ACHADO NO MESMO SITIO (medido, nao curado — e o mesmo arquivo da lista)
`ponto/management/commands/fechar_cobranca_com_lastro.py` mede **DUAS vezes por hora**:
`julgar()` para imprimir o quadro, e `fechar(True)` que chama `julgar()` **outra vez** para
aplicar. As duas passadas **discordam nos dois sentidos** na serie:
* **21/09 13:40** imprimiu `(a) COM LASTRO = 0` e `fechados=6`;
* **01/10 09:40** imprimiu `(a) COM LASTRO = 1` e `fechados=0`.
Soma da serie: `(a)` = 5, `fechados` = **10**. Ou seja o quadro do log **nao descreve o que
foi fechado** (testemunha que recalcula em vez de ler, LEI-AKITA 2) e a fila de 1309 e
varrida duas vezes a cada hora. Cura de origem: `fechar()` devolve o quadro que USOU e o
comando imprime esse. Espera o seu `!` porque o arquivo esta na lista.

### 3. ACERVO DE `intervalo_pendente`: **ZERO VIVO**
`ChamadoColaborador.objects.filter(modulo_origem='intervalo_pendente')` em prod, 04/10 00:49:
total historico **568** · **VIVOS = 0** (`('aberto','em_analise','registrado')`) · na fila do
painel **0**. Por status: `resolvido` 459 · `fechado` 102 · `superado` 7.
Nao ha acervo a triar — o detector das 06:48 nao tem passivo.


**A MINHA FOTO DE GEOMETRIA ERA CEGA AO CAMPO QUE VIRA CHAMADO (03/10 23:2x).** Os 48 moventes da
competencia 09 que eu publiquei saem de uma foto que imprime `entrada>saida` em `HH:MM:SS` -- e isso
e' criterio pela APARENCIA: ela nao ve `aberto`, nao ve `cross_meianoite` e nao ve QUAL batida ocupa o
marco. `aberto` e' exatamente o campo que o emissor le para abrir chamado e pergunta, ou seja, o que
chega ao colaborador. Quem denunciou foi o DRY do `verificar_nucleo`, que compara a TUPLA que o nucleo
grava: ele move o **col369** em oito dias de 22/09 a 03/10 que a minha foto por horario nao viu -- e
nesses oito a cura passa a CONCORDAR com o `TurnoMaterializado` gravado, de quem o HEAD discordava.
Refeita a foto pela tupla inteira (`foto_tupla.py`), nas competencias 08, 09 e 10.

**E O LINE-DIFF DO NUCLEO ME DEU 186 COLABS QUE NAO EXISTEM.** O `verificar_nucleo` imprime conjuntos
Python, e `repr` de `set` nao tem ordem garantida entre execucoes com historico de insercao diferente
-- e a cura muda justamente a ordem em que os turnos entram. Comparar o TEXTO dava **374 linhas em 186
colabs**; comparado por CONTEUDO, sao **10 colabs** (col152, col277, col369, col502, col648, col736,
col833, col857, col866, col869). Mesma familia do criterio pela forma que esta na memoria da casa: o
censo que casa texto infla ou esconde, e a pergunta vai a autoridade, nao ao `repr`. Dos quatro nomes
que nenhuma janela minha media -- **col502 (19/08), col648 (14/08), col857 (15/08)** -- todos caem na
cauda `14-20/08` da janela de 50 dias do command, isto e, FORA da 09: competencia 08, cujo gravado
nenhum deploy recalcula.

**O QUE O CARTORIO E A ATA RESPONDERAM.** A ata **nao entra** em `impressao_insumos`
(`cartorio.py:132-136`, que hasheia batida/cobertura/chamado/DNA e diz, por escrito, que a ata fica
fora) -- entao uma mudanca de geometria **nao** faz a impressao divergir, e o cron das **06:28 nao
relavra a ata por si**. Isso importa porque a ata **le** a geometria: `GRADE_POR_TURNO=True` em prod,
e `montar_grade_prevista_periodo` delega a `..._por_turno`, que chama `turnos_do_colab`
(`escala/utils.py:1309`). A minha leitura anterior -- "o cartorio le `batidas_apuraveis`, nao turno" --
estava **errada pela metade**: as batidas sim, a grade nao. Por isso o censo da ata foi medido pela
propria funcao dela (`ata_do_dia`), com a janela derivada pela mesma aritmetica de
`_julgar_colab_corpo` (`cartorio.py:433-451`), nos dois lados.

### 03/10 23:3x — o que os DOIS crons de escrita responderam, e um erro de forma no MEU script

**Relâmpago (06:26, o cron que RETRATA batida).** `detectar_par_relampago --retratar` SEM `--apply`,
nos dois lados, com md5 provado: `--dias 2` dá *1 chamado a abrir, 0 retratadas* nos dois; a janela
MEDIDA `--dias 50` dá *25 a abrir, 0 retratadas* nos dois. **Zero linha de conteúdo move.**

**Cartório (06:28, que julga 2 min depois).** `processar_cartorio --empresa 2|3|4` sem `--apply`:
`julgadas=2 carimbadas=0 pulados=100780 protestos=0 emitidos=0` **idêntico nas três empresas nos dois
lados**. Mas isto prova menos do que parece, e vale dizer: em DRY o cartório julga 2 dias e pula
100.780 — a fila dele quase não anda sem `--apply`, então o DRY **não** é a prova de que a ata não
muda. A prova é o censo da ata chamando as três funções dele (`batidas_apuraveis` →
`montar_grade_prevista_periodo` → `ata_do_dia`) na competência inteira, que é o que está rodando.

**ERRO DE FORMA NO MEU PRÓPRIO SCRIPT, duas vezes na mesma noite.** (a) O `diff` dos crons compara
os arquivos COM a linha `md5=` que eu mesmo carimbo em cada lado — então todo comparativo saía
rotulado "MUDA" mesmo quando o conteúdo é idêntico, e eu quase li invariância como mudança. É a
mesma família de *critério pela forma conta errado*: o carimbo que prova QUE código rodou não pode
estar DENTRO do universo comparado. (b) A linha de cabeçalho `CENSO DA ATA (a própria função
\`ata_do_dia\`)` levava backtick dentro de `echo` com aspas duplas, e o bash **executou**
`ata_do_dia` → `command not found`. Não contaminou medição nenhuma (só o texto do cabeçalho), e é
o mesmo buraco da lápide "mensagem de commit só por heredoc": backtick em aspas duplas é comando.

### 04/10 00:2x — os DOIS DRYs do aval, agora COM `--apply`, em rollback

O aval pede DRY dos dois crons de escrita. DRY sem `--apply` prova pouco no cartorio (ele julga 2 e
pula 100.780), entao os dois rodaram **COM `--apply`, dentro de `transaction.atomic()` com `raise` no
fim**, e cada lado imprime o md5 do `turnos.py` que rodou -- a mordida do falso verde de 22:4x, quando
o lado da cura nao tinha rodado e eu quase li o log do head duas vezes.

- **Cartorio `processar_cartorio --apply` (06:28).** `universo=53254 celulas | mudam=480 | novas=0 |
  EXPORTADA=0 aberta=480` nos DOIS lados, e as duas listas de mudanca sao **byte-identicas**
  (md5 `13d0226d6a1d164e07513270a03f236b`, 480 linhas cada, todas do dia `2026-10-03`).
  `linhas que movem = 0`. Rollback provado: 53.254 celulas, foto identica a de antes.
- **Relampago `detectar_par_relampago --dias 2 --apply --retratar` (06:26, o que RETRATA batida).**
  `A: retrata=0` nos dois lados, 0 chamado aberto de forma diferente. Ele **le** `turnos_do_colab`
  (`detectar_par_relampago.py:66`), por isso tinha de ser medido; a cura nao muda o que ele retrata.
- **`recompute_turnos` na janela DO CHAMADOR REAL (`2026-10-02 .. 2026-10-05`).** `recompute escreveu
  454 turnos` nos dois lados, `TM mudam=0`, `EXPORTADA=0 aberta=0`, rollback provado, 0 linha de diff.

**E A PRIMEIRA JANELA QUE EU MEDI RESPONDIA UMA PERGUNTA QUE NINGUEM FAZ.** Rodei o recompute de
`21/07` ate hoje e vi **6.074 linhas de TM movendo, 4.981 delas em competencia EXPORTADA** -- numero
alarmante e sem sentido. Os SEIS caminhos automaticos de recompute passam `d-2 .. d+1`
(`ponto/signals.py:21` e `:89`, `processar_alertas_turno.py:62`, `processar_alertas_avancados.py:57`,
`flip_batida.py:39`, `veredito_celula.py:304`); o unico que abre 60 dias e o `materializar_turnos`, que
`config/crons.py:1136` declara *"backfill E3 sob ordem"* e **nao tem cron**. Aqueles 6.074 sao a
DEFASAGEM do cache de TM contra o juiz -- ela existe igual no HEAD (6.074 contra 5.856) e nao e' efeito
da cura. A janela da medicao agora sai do CHAMADOR, nao de mim.

### 04/10 00:4x — A LINHA HAIKU: `ponto/turno_leitura.py`, a 5a ferramenta de LEITURA

O aval pede *"contador esperado 0, golden col736, degrau=leitura"*. A ferramenta faz **duas perguntas
ao MESMO juiz** (`turnos_do_colab`, a janela inteira e depois dia a dia) e compara as respostas.
`ESPERADO = 0` nao e' "0 e bom": e' a propria L-099. Zero derivacao -- ela nao pareia batida, nao le
marco, nao sabe o que e' intervalo --, nenhum degrau novo, nenhuma autoridade nova.

**MEDIDO NA SOMBRA, o MESMO leitor nos dois lados** (`md5_leitor=c5ff974b`), janela da competencia 09
cravada (21/08-20/09):

| colab | HEAD | CURA | dias examinados (iguais nos dois lados) |
|---|---|---|---|
| col736 | **18** | **0** | 26 |
| col866 | **5** | **0** | 14 |
| col277 | **4** | **0** | 18 |
| col152 | 0 | 0 | 21 |
| **soma** | **27** | **0** | — |

`dias_examinados` e' IGUAL nos dois lados -- a cura nao zerou o contador deixando de examinar dia, que
seria a forma elegante de mentir aqui. Custo de uma chamada: **0,25 a 0,37 s** por pessoa numa
competencia.

**A JANELA TEM DE SER CRAVADA, e isso quase me custou um falso verde.** Sem janela a ferramenta usa
`janela_atual(hoje)` = competencia **10**, e os dias do col736 estao em 08-09: as duas pontas dariam
**0 e 0** por olhar o mes errado. O selo cobra isso (`test_MORDE_janela_default_sai_do_corte_da_empresa`:
empresa com corte 11 e perguntada de 11/09 a 10/10, nunca 21 cravado).

**A soma 27 bate com o censo de 03/10, mas o CONJUNTO nao e' o mesmo, e dizer "27 = 27" sem isto seria
numero pela forma.** O censo comparou *competencia x mes civil*; a ferramenta compara *competencia x
[d,d]*. Por isso o col736 da 18 aqui e 16 la (22/08 e 31/08 nao cabem no mes civil de setembro) e o
col152 da **0** aqui e 2 la (os 2 dele estao na competencia **10**, e sao exatamente o DIFF de frota ja
publicado). A igualdade das somas e' coincidencia; a leitura certa e' *"os 27 do dono/segmento viram
0 nas duas comparacoes"*.

**JANELA VAZIA NAO RESPONDE ZERO**: colaborador sem turno na janela devolve `nao_medido`, nunca
`numero: 0` -- zero ali seria afirmar coerencia sobre universo nao examinado, que e a familia AUSENCIA
DE SINAL LIDA COMO SINAL BOM (os quatro selos vazios de 01/09, e o chamado de cluster morto por dia nao
medido que eu medi hoje mesmo).

### 04/10 00:3x — ACHADO no caminho: a suite do NUCLEO esta VERMELHA no main e a regua nao a roda

Para provar o selo novo do copiloto eu rodei `manage.py test nucleo` (o stack da mensageria, container
IRMAO no cpuset de teste). **513 testes, e DOIS vermelhos que JA ESTAO no HEAD** -- rodei os dois lados
e as falhas sao identicas, nao sao da minha fatia:

1. `nucleo/tests/test_ferramenta_certificacao.py::test_MORDE_o_contexto_liga_o_bloco_junto_da_prontidao`
   exige as linhas de `prontidao_da_folha` e `certificacao_da_pergunta` **ADJACENTES** no
   `contexto_do_chat`; duas ferramentas novas (`como_esta_a_fabrica`, `de_onde_vem_a_jornada`) entraram
   entre elas. A LEI do selo (*"certificacao entra no degrau da prontidao"*) segue cumprida -- o que
   quebrou foi a COPIA literal de duas linhas. Mesma lapide do *selo que copia valor fica vermelho*.
2. `nucleo/tests/test_prompt_gerado.py` -- `nucleo/PROMPT_GERADO.md` fora de sincronia com o codigo.
   Tambem ja vermelho no HEAD, e a minha ferramenta nova muda esse arquivo gerado de novo: ele e'
   REGENERADO no ato do commit (`manage.py gerar_prompt_copiloto`), nunca editado a mao.

**O que isto e, de verdade**: `bin/pre-push.sh` roda `regua_rotas_mensageria.sh` e
`gerar_haiku_dentes.py --conferir`, mas **nao roda os 513 testes do nucleo** -- nem a regua. E' o
`holerite` de 04/09 outra vez, com outro nome: um stack inteiro provado e MUDO. Vira item estrutural
(nao e carona desta fatia: por a suite do nucleo na regua deixa a arvore VERMELHA nos dois achados
acima, e cada um tem de ser curado com RED proprio).


**RESPOSTA AO PEDIDO DELE DE 04/10 00:1x -- OS DOIS NUMEROS DO CLUSTER, ANTES DE ELE ESCOLHER (a) OU (b).**
Ele pediu *"quantos chamados de cluster com mais de 14 dias existem hoje e quantos o cron fecha por idade por
dia"*. Medido 04/10 00:1x em PROD, pela funcao REAL do comando (`dias_com_cluster` com `ref` = o proprio dia e
`dias=1`) e pelo juiz do dia (`dia_do_fato(fortes=True)`), nunca por `contexto_json` cru:

1. **HOJE: ZERO.** Com mais de 14 dias, zero -- e zero VIVOS de qualquer idade. O acervo inteiro do emissor
   sao **18** chamados, **todos mortos**. O ultimo nasceu em **15/09**, ha 19 dias: o emissor nao emite desde
   entao.
2. **POR DIA: ZERO hoje, e 3 na vida inteira do emissor.** O cron fechou por idade em **3 dias distintos** --
   07/09, 09/09 e 20/09, **um em cada** -- e os tres com idade de **exatamente 14 dias** (chamado#18363 col788
   dia 24/08, #17710 col859 dia 26/08, #22160 col612 dia 06/09). Os tres com `resolvido_em` as **09:24 UTC =
   06:24 local**, que e a hora do cron. Sobre os 69 dias de vida do emissor (1o chamado em 28/07) isso da
   **0,04 por dia**, ou um a cada 23 dias; nos ultimos 19 dias, **zero**, porque nao ha o que fechar.

**O NUMERO QUE DOI NAO E A VAZAO, E A PROPORCAO.** Dos 18, **11 morreram com a assinatura do cluster AINDA no
dia** (>=2 orfas em rajada <=15min + lampada apagada). Oito desses morreram por `admin`/`legado` -- decisao
humana, e esta certo. **Tres morreram por `via_encerramento=sistema`**, e esses sao os da idade 14.
MECANISMO, lido e nao inferido: `detectar_cluster_espurio.py` monta `datas` so com os ultimos `--dias 14` e
pergunta `premissa_morta(..., fatos={FATO_CLUSTER_ESPURIO: d in datas})`. Dia de 20 dias atras nao esta em
`datas` porque **nao foi medido**, e a premissa le isso como fato morto: conferido chamando a funcao --
`premissa_morta(cluster, {FATO: False})` = **True**, e dai sai o `ch.reconciliar()`. E a familia AUSENCIA DE
SINAL LIDA COMO SINAL BOM, agora matando chamado em vez de passando selo.
**NAO TOQUEI o emissor** -- ele esta na lista *"esperam LEI minha, nao tocar"* do aval das 00:1x. Item
**O169**, e a esteira seguiu no O142 enquanto media.


**A FRASE QUE PROTEGIA O ZERO CAIU, E FUI EU QUE A DERRUBEI (03/10 22:5x).** O paragrafo do CENSO DOS
HUNKS abaixo dizia, verbatim, que *"nenhum hunk toca `parear_turnos` [...] nem `_fechar_aberto_na_pausa_sem_volta`
-- as autoridades que o dinheiro de fato chama"*. Era verdade as 22:4x e deixou de ser as 22:5x: a cura da
**fonte (1)** do aval (o `max(bs)` de `_fechar_aberto_na_pausa_sem_volta`, que perguntava o FATO ENCERRADO a
JANELA) adiciona um kwarg `ultima_batida=None` as DUAS funcoes e repassa o parametro na linha 888. O censo
contra `git show HEAD:` agora da **7 hunks em 4 sitios**: modulo (`import copy`, inerte),
`_fechar_aberto_na_pausa_sem_volta` (assinatura + o `_ult`), `parear_turnos` (assinatura + o repasse) e
`turnos_do_colab` (corpo + `_marcos_de`). `_fechar_aberto_com_saida_seguinte` **nao** e tocada -- ela aparecia
no meu primeiro censo porque eu atribui o hunk pelo numero do cabecalho `@@`, e o cabecalho comeca 3 linhas de
CONTEXTO antes, dentro da funcao anterior. O que isso muda de verdade: **o zero do dinheiro deixa de ser
estrutural e passa a ser portante.** Antes ele valia por construcao (o dinheiro nao executava linha minha);
agora o caminho do dinheiro EXECUTA codigo mudado, e o zero so vale MEDIDO. Por isso os dois DIFFs de frota
estao sendo refeitos contra a cura nova -- o md5 da cura mudou de `c9579ff1` para `23235e41`, e todo numero
publicado as 22:1x/22:3x foi medido contra a **cura velha**.
**E O ARREIO ME DEU UM QUARTO FALSO VERDE NO CAMINHO, pelo mesmo buraco de sempre.** A primeira rodada do
re-DIFF caiu no schema `public` da sombra (`manage.py shell` nao entra no tenant; `relation
"colaboradores_empresa" does not exist`), os dois lados abriram o arquivo de saida, estouraram antes de
escrever linha, e o `diff` de **dois arquivos vazios** imprimiu `0 linhas movem` -- que e' exatamente o numero
que eu esperava ver. Quarta vez nesta fatia que AUSENCIA DE SINAL se le como SINAL BOM (o `SEM totais`, os
dois arquivos de mesmo tamanho, a geradora que nao discriminava, e agora este). Cura no arreio, nao na
anotacao: o runner passa por `tenant_command shell --schema=juliani` (ping: **531** colabs ativos em
emp2/3/4, o numero publicado), e o driver ganhou `exige()`, que ESTOURA antes de qualquer `diff` se um lado
nao tem arquivo, tem arquivo vazio, tem menos linhas que um piso declarado, ou se o log nao prova o **md5**
do `turnos.py` que aquele lado carregou.

**O ZERO DO DINHEIRO MORDEU, e mordeu pelo lado bom (03/10 22:3x).** O paragrafo abaixo cobrava o DIFF de
frota de dinheiro da `col152`; ele existe agora, e o resultado e **0 linhas movem em 568 colabs / 14.768
campos por lado** pela PORTA REAL (`ponto/services/fechamento.py:19::recalcular_fechamento_mes(10, 2026,
somente_leitura=True)`, com `SystemExit` se a porta nao responder ou vier vazia). **Um zero que contradiz a
geometria nao vale sozinho** -- a `col152` ganha 6 dias de turno com a cura --, entao o zero foi medido na
FONTE, com dois contadores em volta das autoridades, e nao raciocinado: no caminho do dinheiro dela
`turnos_do_colab = 0` chamadas, `parear_turnos = 5`, `turnos_de_batidas = 3`, `papel_por_minuto_da_ata = 2`.
Os quatro sitios que fazem `from ponto.turnos import turnos_do_colab` em coluna 1 (`colaboradores/services/
calendario.py:14` e os dois commands) foram repatchados no ato, para o contador nao poder **sub**contar; e a
sonda ESTOURA se `parear` e `tdb` derem os dois 0, porque ai a cega seria ela, e o zero nao provaria nada.
**O ZERO E PROVADO, NAO OBSERVADO (22:4x).** Os dois arquivos de saida tinham o MESMO TAMANHO em bytes, e
e' isso que uma rodada **sem a cura montada** tambem produz -- o mesmo buraco do `SEM totais`, de novo. Entao
a sonda passou a imprimir o **md5 do `ponto/turnos.py` que o container carregou**, antes de medir: lado HEAD
`119922ee6a1be779d28898495d38de86`, lado cura `c9579ff16f79a1b53d426fa2ec5a8224` (= o arquivo do scratchpad).
As duas saidas deram md5 `52e35fc993b588049913efd9f4d69274`, **igual**. Isto e' *"codigo diferente, resultado
igual"*, que e' uma afirmacao; *"resultado igual"* sozinho nao era.
**CENSO DOS HUNKS, para a historia do mecanismo nao ser so narrativa**: a cura tem **3 hunks** contra
`git show HEAD:app/ponto/turnos.py` -- um `import copy as _copy` no topo (linha 29, inerte) e dois dentro de
`turnos_do_colab` (corpo, def na 1211; e `_marcos_de`, def na 1243, **aninhada** nela, indentacao de 4).
~~Nenhum hunk toca `parear_turnos`, `turnos_de_batidas`, `papel_por_minuto_da_ata` nem
`_fechar_aberto_na_pausa_sem_volta` -- as autoridades que o dinheiro de fato chama.~~
**RETRATADA por mim as 22:5x: a cura da fonte (1) tornou esta frase FALSA.** Ver o paragrafo do topo.
**MECANISMO, com file:line**: o dinheiro nao passa por `turnos_do_colab`. `motor_calculo_v2.py:338::
turnos_via_autoridade` monta os marcos do dia por conta propria (`escala/utils.py::marcos_dna_periodo`,
linhas 356-360) e chama `parear_turnos` DIRETO na 388. A cura do O142 reescreve o trecho ~1237-1300 de
`turnos_do_colab` -- uma funcao que o caminho do dinheiro nunca consulta.
**E A GEOMETRIA QUE O DINHEIRO USOU JA TINHA OS 6 DIAS.** Capturei a SAIDA de `parear_turnos` no sitio real
(sem reconstruir a chamada, conduta 6): 59 objetos, **10 dias**, e os seis -- 25, 28, 29, 30/09 e 01, 02/10 --
todos **NO DINHEIRO**. As pontas batem com a foto da cura ate o segundo (`06:52:56>18:36:04`,
`06:58:09>18:30:00`, `18:34:20>-` aberto, ... `06:56:15>18:30:06`). Isto e: **a cura nao muda o dinheiro
porque ela faz o LEITOR concordar com a autoridade que o dinheiro ja usava**. O `turnos_do_colab` era o que
estava errado, sozinho -- tela, PDF, cartao e os crons liam 5 turnos onde a folha contava 10. E' a LEI-AKITA 2
pelo avesso: a testemunha tinha regra propria, e a regra propria era a que mentia.
**O que isto NAO prova, e fica dito**: nao prova que as **87,91 h** da `col152` estao certas. Ela tem 22 dias
previstos e 10 com batida; o saldo de **-114,5 h** e a conta disso, e por L-099 o dono dessa divergencia pode
ser CADASTRO ou BATIDA, nao ESTRUTURA. Nao e desta fatia, e nao se cura por codigo. O 8,79 h/dia liquido
contra 11,7 h de span tambem nao foi auditado aqui -- fica nomeado para quem for olhar intrajornada.

**AUSENCIA FECHADA no estrutural: `contratos_estruturais: 13/22 verdes`** pela funcao real, selo `test_selo_contratos_estruturais` 13 OK (`c1a1f7b6` + a celula virada). **O154 BUSCA-LE-O-QUE-MOSTRA FECHADA** -- a busca de escala le `nome_canonico_dropdown()`, 10 RED -> 945 testes OK no app. **ARVORE-SEM-CONFLITO NO AR**: eu causei ~30 s de 502 no `saas_ui` com `cherry-pick | tail && deploy`, e as duas guardas nasceram no mesmo turno (indice/marcador antes da migration; os 2 urlconfs IMPORTADOS na prova de casca, 597 rotas). Mesa de avais em **8**: a lei da etapa 0 respondeu um e o smoke da etapa 0 (`o122-etapa0-smoke-barra-no-painel`) nasceu no mesmo ato, porque a lei mandou *"manda a etapa 0 para o smoke"*. **RAIA-UI MERGEADA E NO AR** (`e49a8289`, ff + deploy as 21:35-21:36 no MESMO ato): 9.552 testes OK, 597 rotas provadas, 58/58 selos de host, `importerror_500=0`. **O142 MEDIDO**: a RED do `ativa` pousou (`aa2550b0`, 2/6 MORDE -> 77 OK) e o censo de frota deu **33 dia-colab** que mudam de turno com a janela -- 27 sao o dono/segmento (col736 16, col866 5, col277 4, col152 2) e 6 sao a borda direita da janela, outra fonte. 31 dos 33 estao na **09 EXPORTADA** (lista, nunca apply); o DIFF de frota **nao** e so esses 2: ele compara HEAD **contra a cura** na competencia 10 inteira (o censo comparou HEAD contra HEAD em duas janelas -- outra pergunta), porque a cura tambem mexe no dia fora da vigencia de vinculo unico (col882) e no pareamento em copia. **A CURA DO SEGMENTO ESTA PRONTA E MEDIDA, E **NAO** FOI COMMITADA** -- de proposito: o HUP das **03:30** (`deploy.sh --reload-agendado`) publica o que esta NO DISCO, e a arvore E o bind-mount, entao commitar `ponto/turnos.py` hoje poria a cura no ar sem o DIFF de frota, que e o PROIBIDO literal do aval. `deploy.sh` nao honra arquivo de pausa nenhum (conferido no vivo), logo pausar por arquivo seria selo vazio: o unico portao real e o disco. A cura vive em `<scratchpad>/o142cura/turnos.py`, construida de `git show HEAD:`, com RED/GREEN e selos de query publicados abaixo. **O QUE VAI AO AR AS 03:30 MESMO ASSIM**: o `aa2550b0`, a cura do `ativa` no fallback de `vinculo_do_dia` -- ela ja esta commitada e empurrada. Ela aplica a lei de 16/09 (vinculo desativado SEM `data_fim` nao e dono de dia), e o alcance dela em frota **nao foi medido por DIFF**: fica dito aqui, no topo, em vez de descoberto depois.

**O DIFF DE DINHEIRO AINDA NAO EXISTE, e o `diff` VAZIO quase me fez dizer que existia (03/10 23:5x).** Apontei a sonda para `ponto/services/espelho.py:875::autoridade_do_periodo` e os dois lados deram `diff` vazio. **Nao e' 'o dinheiro nao se moveu': e' a porta errada.** Aquela funcao devolve `ponto.services.espelho.Autoridade`, que tem `batidas`, `esc`, `feriados` e `index` -- **GEOMETRIA**; ela nao tem `totais`, entao a sonda caiu no ramo defensivo e escreveu a MESMA linha `SEM totais` nos dois lados. Dois silencios identicos comparam igual. E a familia do SELO ANTI-VACUIDADE do CLAUDE.md -- ausencia de sinal lida como sinal bom --, e o ramo defensivo que eu mesmo escrevi para nao estourar foi o que produziu o falso verde.

**Fica dito o que falta, com nome**: o efeito em dinheiro dos 6 dias da `col152` (horas trabalhadas, HE, noturno, banco) **nao foi medido**, e nenhum deploy pode sair antes disso -- o PROIBIDO do aval cobra DIFF de frota, e DIFF de geometria nao e DIFF de frota de dinheiro. A porta certa e o MOTOR (`ponto/motor_calculo_v2.py` via o caminho que o `FechamentoMensal` usa), medido contra o **GRAVADO** e nao motor-x-motor -- a L-AVAL-DE-CRITERIO ja ensinou que o motor-x-motor esconde campo (12,29 h contra 13,29 h, 10 campos fora do alvo). A sonda tem de falhar ALTO quando a porta nao responde, nunca escrever 'SEM totais' e seguir.

> **DIETA (L-109), 07/10 ~20:xx (O220 DIETA-DE-CARGA):** o bloco que vinha aqui -- de
> `### O142 — O DIFF DE FROTA DA 10 ... (03/10 22:1x)` ate o fim do bloco do `PUSH 96` de 03/10,
> **2014 linhas** -- foi para o FIM de `app/docs/RELATO-ARQUIVO.md`, **inteiro e sem uma palavra
> tocada** (md5 do trecho movido: `6403d44e`). O corte foi por CONTEUDO: parou ANTES da cauda do
> vigia da esteira (o comentario abaixo manda) e NAO entrou nas secoes tituladas `04/10` que guardam
> `###` de 03/10 dentro -- mover meia secao seria tocar o texto. O vivo guarda 04 e 05/10, a cauda do
> vigia ate 07/10 e o bloco `PEDIDOS DE PATCH ABERTOS`, que a dieta nao move por lei propria.

> **DIETA (L-109), 05/10 00:4x:** o bloco que vinha aqui -- `PLACAR-ESTRUTURAL (02/10 23:5x)` ate
> o fim do O108 de 01/10, **5467 linhas** -- foi para `app/docs/RELATO-ARQUIVO.md`, **inteiro e sem
> uma palavra tocada**. O vivo guarda 03, 04 e 05/10.

---

<!-- a cauda do vigia da esteira vive no FIM do arquivo, nao no topo: quem arquivar por
     conteudo tem de parar ANTES desta linha. -->

**02/10 22:35 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**02/10 22:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 00:00.

**03/10 00:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 01:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 02:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 03:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 03:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 04:45.

**03/10 04:50 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 05:55 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 06:55 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 08:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 09:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 10:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 11:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 12:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 13:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 14:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 15:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 16:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 17:20 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 18:20 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 19:20 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 20:25 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 21:25 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 22:30 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**03/10 22:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 00:00.

**04/10 00:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 01:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 02:10 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 03:15 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 03:40 vigia da esteira** -- esteira em espera de janela: 8 fatias prontas, reabre 04:45.

**04/10 04:50 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 05:55 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 07:00 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 08:05 vigia da esteira (ALARME)** -- vigia sem efeito: 9 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

**04/10 09:05 vigia da esteira (ALARME)** -- vigia sem efeito: 8 fatia(s) ativa(s) na fila e nenhum .out escrito ha 30 min -- a esteira esta parada e o vigia nao esta destravando.

## 04/10 09:2x — MARCO FECHADO: o R4 no ar da raia, e o PAINEL apontava o item que o corte dele TIROU da fila

`9daa4cec` empurrado (`15f1d7e0..9daa4cec`), pre-push na arvore do push: **9569 testes OK (skipped=42)**
em 612 s + control-plane 22 OK. A prova da fatia (`bdd65ed0`, raia-r4) segue sendo a suite inteira na
arvore congelada: **9583 / OK (skipped=42)**. O placar NAO mudou de numero -- `linha_do_placar()` segue
`3 fechado(s), 3 parcial(is), 0 pendente(s) de 6`, porque a quarta perna do R4 e UM par de seis.

**BUG NO CAMINHO, curado no ato (LEI-AKITA 6 + 11).** Ao fechar o marco fui reger o painel e ele dizia
`item EM CURSO: E6-CAUDA-2` -- item que o corte dele de 02/10 22:5x **tirou da fila 1** ("a cauda por
classe sai da fila 1 e vira PLACAR-ESTRUTURAL"). Medido pelo leitor REAL, nao por leitura minha:
`bin/hook_stop_fila1.py::_proximo_da_fila()` devolvia `E6-CAUDA-2`. Duas causas, as duas de VOCABULARIO,
nenhuma no hook:
  1. a celula de estado do E6-CAUDA-2 dizia `**REDIRECIONADA**`, e o contrato do leitor fecha item com
     `FECHADA|FECHADO|NO AR|~~` -- **sinonimo nao fecha** (a casa ja tinha a forma pronta no E5-FINAL:
     `**FECHADA — ABSORVIDA pela ...**`). Passou a `**FECHADA -- REDIRECIONADA pelo corte das 22:5x**`;
  2. a celula do PLACAR-ESTRUTURAL -- que eu mesmo escrevi as 09:0x -- terminava em `espera o !` do merge,
     e `espera o \`?!` e um dos padroes de `_NAO_ANDA`: o TOPO da ORDEM VIVA se declarou parado. Mas o item
     nao esta parado; **so o merge de uma fatia fechada dele espera o `!`**. A celula passa a dizer `ANDA` e
     a nomear o proximo passo (281 caracteres, dentro da DIETA).
Nao cresci regex nenhum: o leitor esta certo, quem nao migrou para a palavra declarada foi o ESCRITOR das
duas celulas. Com as duas curas, `_proximo_da_fila()` devolve **PLACAR-ESTRUTURAL** -- o topo declarado.

**PROXIMO ITEM, nomeado para nao se perder na compactacao**: R6, item (3) da propria celula do placar --
a pergunta **K8** (`ponto/janelas.py::janela_atual` e o juiz declarado) e os **6 pendentes** de
`core/juizes.py::PENDENTES['fechamento']` que leem o **MES CIVIL** onde ele ja responde. Censo real na
arvore (`mes_ou\(.*\.month`): **7 sitios em `ponto/views.py`** -- 545 `recusar_he_em_lote`, 617
`gestao_he_pdf`, 731 `gestao_he`, 780 `painel_fechamento`, 847 `fechamento_linhas`, 879
`fechamento_mensal`, 1014 `recalcular_fechamento` -- mais o par `data_turno__month`/`data__month` do
`painel_fechamento`. FORA do censo: 1841 `lista_ausencias` (filtro de VIGENCIA, nao leitor de
`FechamentoMensal`) e os `_pa(mes, ano, 2)` de `fechamento.py`, que sao ENVELOPE declarado sobre os
cortes [2..28], nao corte cravado. LEI-AKITA 4: a lei existe (CALENDARIO-UNICO, corte dele 17/09), a
pergunta e qual leitor nao migrou.

**MARCO FECHADO -- pode compactar.**

## DEPLOYS AGENDADOS

- 08/10 06:08 deploy agendado o221-pouso3, fatia o221-pouso3: rc=0 -- 08/10 06:08:15 O221 pouso 3 NO AR em 1f3d616f. Falta so o push, que e da mao. FIM
