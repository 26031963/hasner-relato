# HASNER PONTO - PRIMER
> Visao de 1 pagina, com o desenho gerado do codigo: **app/docs/PRIMER.md**
> Estado da esteira (fatias, commits, selos, o que esta parado esperando o
> Ronald): **app/docs/TICKETS.md** — o rodape dele situa um chat novo em 10
> segundos. Fatia citada em commit no padrao `[F9]` tem que ter linha la;
> `bin/regua_tickets.sh` roda no pre-push e fica VERMELHO se nao tiver.
> (o diagrama e' app/docs/ARQUITETURA.mmd, regenerado por
> `manage.py gerar_diagrama` e defendido por selo).
> Leis de UI (9, com file:line e o selo de cada): **app/docs/LEIS-UI.md**.
Leia este arquivo INTEIRO antes da primeira acao de codigo, infra, banco ou git.
Ele responde "onde estou" sem auditoria. Se algo aqui divergir do vivo, o VIVO manda -
e corrija este arquivo no mesmo commit.

## 0. LEI-AKITA (Ronald 23/09 18:xx) - NAO E CONSELHO
**Fatia que fura qualquer linha abaixo NAO FECHA.**

1. **ORIGEM, NUNCA BAND-AID.** Cura vai no sitio que e' a FONTE. "Fallback", "por enquanto",
   "acerta o caso comum", "trocar arriscaria quebrar" sao os nomes do band-aid -- ao escrever
   qualquer um deles, **PAROU**: e' derivador paralelo sobrevivendo.
2. **TESTEMUNHA LE, NAO RECALCULA.** Tela, PDF, TXT, badge, contador: leem a mesma autoridade
   (celula/juiz/folha). Nenhum leitor tem regra propria de dia, hora, soma ou classe.
   Selo: leitor com funcao propria de derivacao = VERMELHO.
3. **O CERTO VENCE O CONVENIENTE.** Custo de tempo nunca e' argumento. Se voce mesmo viu um risco,
   ele se mede ANTES; nao se oferece o atalho como opcao.
4. **LEI EXISTENTE ANTES DE CORTE NOVO.** Antes de pedir corte, grep na lapide/CLAUDE.md/docs: se a
   lei existe (celula soberana, espuria fica onde apareceu, tudo tem cadastro, falta = decidida...),
   a pergunta e' "qual leitor nao migrou", nunca "qual a regra".
5. **RED PRIMEIRO, ADVERSARIAL DEPOIS.** Cura sem teste vermelho evidenciado nao existe. Toda cura
   responde por escrito: contra-exemplo alem do caso golden? quem mais le o que mudei?
6. **BUG NO CAMINHO VEM PRIMEIRO.** Achou bug provado no meio da fatia: cura na hora, nao enfileira,
   nao devolve pergunta.
7. **UM ESCRITOR POR ESTADO, NADA EM BRANCO.** Campo de estado tem 1 escritor canonico + tripwire;
   acao critica grava usuario, antes/depois, motivo; excecao nasce como PORTA separada com trilha,
   nunca destravando a trava.
8. **ZERO NUMERO SEM MEDICAO NA FONTE.** Medir chamando a funcao REAL que o sistema usa para julgar,
   nunca replicando a logica nem lendo campo lateral. Rotulo diz o que a conta faz; contador == universo.
9. **ESCOPO DO AVAL E' LITERAL.** Aval condicional que nao fecha na condicao = **PAROU** com o numero.
   "Dinheiro nao se moveu" NAO amplia aval.
10. **CONSTRUIR EM COPIA DO HEAD, APLICAR NO COMMIT.** Nunca editar a arvore viva; a copia nasce de
    `git show HEAD:` na hora do patch.
11. **LEGIBILIDADE SITUACIONAL.** Depois da cura, um programador novo se situa sem arqueologia: um
    juiz por dominio, derivacao no nucleo, fronteira que responde "quem manda aqui".
12. **TUDO TEM CADASTRO.** Regra que varia por cliente/praca/empresa nasce como cadastro pela UI, com
    nome e leitor; chave sem leitor = VERMELHO.
13. **A REFERENCIA E A LEI, NAO O CODIGO DE HOJE.** O numero se confere contra a REGRA (CLT, CCT,
    `L-NNN`) aplicada ao cadastro e as batidas: escreve-se o CASO com a resposta da regra ANTES do
    codigo -- cadastro tal, batidas tais, o dia vale tanto --, e o codigo novo se certifica contra os
    casos e contra as PROPRIEDADES FIXAS (partes somam o total, idempotente, previsto nao depende de
    realizado, todo leitor le o mesmo numero). O caminho velho se le UMA vez, na troca, so para dizer
    quem muda de valor e quanto (L-094, L-092): medida de IMPACTO, nunca validacao -- certificar o
    novo por DIFF contra o velho e PROIBIDO, e amostra nao e prova. **L-110**.
    **L-110 LITERAL PARA A TROCA DE MOTOR** (corte dele 21/10 ~19:1x, palavras dele: *"v3: portao =
    bateria de casos pela REGRA (CLT/CCT/L-NNN) verde + propriedades fixas; 'sombra no hash de hoje' e
    DIFF v2 x v3 como validacao saem do portao; o v2 e lido UMA vez na troca, para impacto por rubrica,
    e o que divergir vai com dono L-099. Se a bateria de casos nao existe, construir ela e o proximo
    passo da v3"*). O que isso MUDA, e nao e releitura: o portao da v3 deixa de ter como item
    `sombra carimbada de hoje` e `DIFF v2 x v3`; eles nao sao PROIBIDOS -- o DIFF da troca segue devido
    UMA vez como IMPACTO por rubrica, com o que divergir aberto por DONO (L-099) --, o que sai e o
    papel deles de **validar**. Lei nenhuma a mais muda: a condicao de dinheiro em competencia aberta
    e a de 18:5x, tres itens, logo abaixo na secao 7b.

**SELO DE CONDUTA.** Ao propor qualquer cura, escrever uma linha:
`LEI-AKITA: origem=<sitio>, testemunha=<autoridade lida>, RED=<teste>, quem-mais-le=<censo>, juizes novos=<0 ou lista com o corte>, padrao=<termo>`
Fatia sem essa linha **nao entra na fila** (`bin/tests/test_lei_akita.sh`).
**PADRAO-NA-CURA (L-123, corte Ronald 27/10 17:0x -- regra sagrada).** O norte e o desenho do `app/docs/GLOSSARIO.md` e do
`app/docs/PLANO-PADROES.md`; **nenhuma cura entra sem dizer qual PADRAO ela realiza**, e cura e a CLASSE, nunca o caso (queixa,
laudo ou BO so vira obra depois de nomeado o padrao que mostra quebrado). A linha acima ganha o campo obrigatorio
`padrao=<termo>` (termo que EXISTE num dos dois arquivos; medida/instrumento: `padrao=n/a (medida)` ou `padrao=n/a (instrumento)`).
Selo: `bin/tests/test_lei_akita.sh` (do commit que o introduziu em diante, sem retroagir). O verificador confere que o padrao
nomeado e o que o diff REALIZA (rotulo vazio = NAO POUSA). Padrao novo so por corte dele.

**PAREI-DE-LEI-NAO-DEVOLVE-TURNO** e **PAREI-SO-LEI**: ver PARADA E AUTONOMIA (7b).

**TRAVA JUIZ-NOVO (corte Ronald 24/09 10:xx).** Registrar autoridade nova em `core/juizes.py` exige a
frase `corte Ronald: juiz <nome> nasce` no CORTES.md; sem ela o selo
(`bin/tests/test_juiz_novo_tem_corte.sh`) fica VERMELHO e a fatia nao pousa. (historia: LAPIDES.md#lap-47)

## 1. O QUE E
SaaS de ponto eletronico CLT multi-tenant (django-tenants). Cliente principal:
Grupo Juliani, ~750 colaboradores, multiplos CNPJs (emp2=J.A, emp3=JS/JSP, emp4=RA).
Dominio: juliani.hasner.com.br. Competencia da folha corre do dia 21 ao 20
(Empresa.dia_inicio_competencia, default 21) - NAO e mes calendario.

## 2. INFRA
Detalhe operacional (cpuset, containers, sombra, merge, reciclagem, com as lapides) e LOOKUP: **app/docs/INFRA.md**.
- VM saas-hasner-sp: 8 vCPU = 4 nucleos fisicos (SMT 2), 16 GB. O usuario opera DENTRO dela; nunca prefixar ssh.
- CPUSET (corte Ronald 22/09, "teste nunca rouba CPU do cliente"), fonte unica `bin/recursos.sh`: vCPU 0-3 PRODUCAO, vCPU 4-7 TESTE (juliani_db_test, sombra_db e todo `docker run` de suite pelo $TESTE_DOCKER). Selos `test_recursos_sourced.sh`, `test_ensaio_fora_do_postgres_de_prod.sh`.
- Repo /home/ronald/saas-hasner -> github 26031963/hasner-ponto; Django em app/ (bind-mount /app, HOST == CONTAINER).
- Cascas: saas_core = KERNEL (auth + batida, config/urls_core.py); saas_ui = todas as telas (gthread, timeout=30: trabalho longo vai pra job); saas_caddy roteia /api/ponto|auth|me|regularizacao|ping-geo|registrar-fcm e /health/ ao core, resto a UI. Bancos: saas_db (prod), juliani_db_test (SO testes), sombra_db (ensaio e juliani_ref_*, NUNCA no saas_db). mensageria fora do compose.
- PUBLICAR CODIGO = SO `bin/deploy.sh` (`--sem-migrate` sem modelo; `--sem-sombra "motivo"` sem ensaio do dia): migra antes, exige o ensaio de hoje, reinicia as 3 cascas juntas e prova uma rota em cada. `docker compose restart` NAO e deploy; `docker restart` NAO e recreate (imagem/command novos so com `up -d`). max_requests=0 (BUG 128): gravar .py nao o poe no ar; .html muda NA HORA (sem cached.Loader) -> arquivo de fatia so entra na arvore no ato do commit/deploy. Reload gracioso 03:30; selo `bin/selo_reciclagem.sh`.
- MERGE DE RAIA CAI E RECARREGA NO MESMO ATO (L-107): `git merge --no-ff --no-commit` -> resolver -> commit -> `bin/deploy.sh`, nada no meio. `--no-commit` sozinho faz FAST-FORWARD: `MERGE_HEAD` vazio = o pouso ja comecou, o deploy e agora.
- SOMBRA: horario DERIVADO (`config/crons.py::inicio_derivado`), nenhum numero escrito aqui. Entre 00:00 e 04:00 o portao e cego: `--refazer --dump-agora`. Migrate depois do dump do dia deixa o portao vermelho: `--refazer --dump-agora` + `--bloco` depois do cron da sombra (espera por ARQUIVO em logs/crons_em_curso, nunca pgrep).
- Apps Django: api chamados colaboradores comunicados core escala ferias folha holerite inteligencia pautas ponto relatorios tenants. A autoridade de `Empresa` e `colaboradores.models`.

## 3. TESTES - COMANDO CANONICO
bash ~/saas-hasner/bin/suite.sh
A FORMA DEIXOU DE SER PROSA E VIROU ARQUIVO (O182, corte pela lei existente 04/10 05:5x). (historia: LAPIDES.md#lap-03) Agora `bin/suite.sh` compoe recursos + `teste_envfile` + LABELS lido da regua + montagem +
trava, e nao ha como rodar a suite inteira destravada sem escrever um comando novo A MAO.
`--only "ponto core"` recorta (e SEGUE pela trava, de proposito), `--dir <copia>` roda contra COPIA,
`--parallel N` e `--espera N` repassam, `-- <resto>` vai cru ao `manage.py test`. **rc 75 = a vez nao
chegou, NAO e vermelho.** `bin/regua.sh` e `bin/pre-push.sh` seguem com a chamada propria porque JA
pegam a trava (`pre-push.sh:130`) e a regua tem carimbo em volta dela: aninhar seria esperar por si
mesmo. Selo: `bin/tests/test_suite_sh.sh` (recorta este bloco pela ESTRUTURA -- do titulo ate a
primeira linha sem `\` -- e fica VERMELHO se ele deixar de nomear a porta; prosa nao entra no recorte).
A SENHA NAO VAI MAIS NO ARGV (corte Ronald 25/09 08:4x, SEGREDO-FORA-DO-ARGV). Esta linha dizia
`-e DB_PASSWORD="$PW"`, e argv e PUBLICO: qualquer processo da maquina le em `ps aux`. (historia: LAPIDES.md#lap-04) Agora `teste_envfile` (bin/recursos.sh, fonte unica) escreve
`logs/.env_teste` com modo **600** e o argv leva so o CAMINHO; o valor continua saindo de
`docker exec juliani_db_test printenv POSTGRES_PASSWORD`, entao nao nasceu um segundo lugar onde a
senha mora. Selo de host: `bin/tests/test_segredo_fora_do_argv.sh` (12 sitios migrados; `grep` de
senha em argv dentro de `bin/` = **0**). Senha ROTACIONADA no mesmo ato.
A LISTA NAO SE DIGITA MAIS AQUI -- ela mora em `bin/regua.sh::LABELS`, e este arquivo a LE.
(historia: LAPIDES.md#lap-05) Selo: `bin/tests/test_labels_fonte_unica.sh`
(app com teste fora do LABELS = VERMELHO; lista repetida em outro sitio = VERMELHO).
CONTAINER IRMAO, NAO `docker exec saas_core` (mudou em 22/09, resize). (historia: LAPIDES.md#lap-06)
A forma nova nao leva o HASNER_TENANT_URLCONF que o compose da ao core, (historia: LAPIDES.md#lap-39)
Veredito: `grep -E '^(OK|FAILED)($| \()' <log> | tail -1` MAIS o `^Ran N tests | tail -1` MAIS o
rc do PROCESSO (L-074), que e a forma do VIVO -- `bin/regua.sh:170-171`, `bin/vigia_arvore.sh:89`, `bin/isolamento.sh:34` e os quatro
moldes, todos com o `tail -1`. A fronteira nao e enfeite (historia: LAPIDES.md#lap-40) O que separa veredito de prosa sao DUAS coisas juntas, e nenhuma delas e
o espaco: o `\(` em vez do espaco (o Django so imprime `OK`, `OK (skipped=N)` ou `FAILED (...)`) e o
`tail -1` (o veredito e a ULTIMA linha do run, a prosa vem no meio). (historia: LAPIDES.md#lap-41) Rodar SEM labels da centenas de
errors MIRAGEM.
Control-plane (tenants) roda separado, settings teste_motor.
`holerite` ENTROU em 04/09 (achado H-c): (historia: LAPIDES.md#lap-42)
Pre-push hook INSTALADO e ATIVO: roda os dois runners antes de liberar push.
Selos de HOST (sem Django, sem container -- o container nao monta `bin/` nem `.git/`):
  `bin/tests/test_*.sh`. A regua roda a PASTA INTEIRA antes da suite, junto do node_check: selo
  novo entra la e passa a ser chamado sem cadastro nenhum. (historia: LAPIDES.md#lap-43)
UM run por vez no juliani_db_test - push conta como run; colisao gera errors falsos. Quem segura:
`bin/trava_teste.sh --quem`. E ele responde CHAMADOR, nao pista: `livre` quer dizer "ninguem pediu a
vez", nunca "nao ha suite rodando" -- foi esse o O182.
A PORTA DEFAULTA PARA A ARVORE VIVA; contra COPIA (worktree, `git archive`, raia) use `--dir <copia>`,
que pede a montagem a porta unica. Sem ela a copia roda uma arvore INCOMPLETA: `app/staticfiles/` e
.gitignore, e todo selo que RENDERIZA pagina da RED falso (7 selos, medido 4x). A lista do que falta tem
UM escritor -- `bin/arvore_do_push.sh --montagem <dir>` --, e `--dir` e quem a consulta por voce. Ele devolve o
`-v` do staticfiles E os tres `--tmpfs` de cache; sem os tmpfs o container escreve `.ruff_cache`/`.hypothesis`/
`.mypy_cache` como ROOT dentro da copia e o `rm -rf` do usuario falha. Nao montar a mao: (historia: LAPIDES.md#lap-44) Selo: `bin/tests/test_montagem_vem_do_arvore_do_push.sh`.

## 4. ZONAS INVIOLAVEIS
- ponto/motor_calculo_v2.py: so com aval explicito do Ronald.
- A LEI: batida de chao NUNCA e barrada em runtime. Chokepoint unico de escrita =
  ponto/registro_batida.py. Motor NUNCA fabrica batida.
- Chave de match SEMPRE CPF ou nome completo. NUNCA matricula.
- Timestamps em UTC no banco; apresentar UTC-3. Toda sonda de batida imprime
  timezone.localtime, nunca o timestamp cru.
- A GRADE e fonte unica de previsto (S133). Existindo fonte, e proibido derivar em paralelo.
- CELULA-DIA (escala/models.py::CelulaDia): dia+marcos como DADO com dna congelado
  (irretroativo, com UMA excecao formal: `regenerar_celulas_vinculo` reescreve o
  passado ERRADO POR CADASTRO -- corte 16/08. Desde 01/09 a celula DENUNCIA isso
  em `regenerada_em`/`regeneracoes`/`dna_anterior`: sem esses campos a forense le
  a versao nova achando que le a original, porque gerada_em e auto_now_add,
  editada_em fica NULA e origem continua 'gerada'). Escala/template = GERADOR, so alcanca o futuro. Fontes:
  EC.eh_dia_trabalho e EC.marcos_do_dia LEEM a celula (fallback aritmetico/template).
  P13: unico escritor automatico = gerar_celulas; edicao humana = origem='editada'.
  MOTOR_calculo_v2 le DNA via EC.marcos_do_dia (3 sitios, fallback template; e24f0a17).
- Universo de apuracao = ponto/turnos.py::batidas_apuraveis (exclui retratada_em).
  O TRIPWIRE PASSOU A EXISTIR em 12/10 (O247, achado B5-j). (historia: LAPIDES.md#lap-63) por isso o que ele
  varre fica DECLARADO aqui, e nao prometido: `ponto/tests/test_tripwire_b5j_universo_de_apuracao.py`
  pergunta, por AST e COM APELIDO, se toda FUNCAO que chama `parear_turnos` monta o seu universo
  pela porta, por `retratada_em` explicito ou por lookup de chave -- allowlist ZERO, mais uma
  catraca de 8 copiadores a mao que nao cresce. (historia: LAPIDES.md#lap-63) O QUE ELE NAO VARRE, e segue documental: universo recebido por PARAMETRO (2 sitios
  declarados no proprio selo) e caminho de apuracao que nao passa pelo pareador. O vizinho
  `ponto/tests/test_tripwire_bocais_apuracao.py` responde OUTRA pergunta -- 6 arquivos NOMEADOS,
  por regex, com allowlist de CONTAGEM -- e nenhum dos 3 sitios curados estava nela.
  EXIBICAO de marcacao le o cru por lei (Portaria 671).

## 4a. TABULEIRO E LAMPADAS (principio, Ronald 17/09)
- ESCALA = o que devia acontecer (fonte unica). CELULA/ATA por marco = o que aconteceu (a LAMPADA).
- EVENTO carimba e julga na hora (signals do cartorio, B5.2). Emissao, pergunta e chamado nascem do sinal da celula.
- CRON DE VARREDURA = VIGIA: contador "esperado 0", so alarma, NUNCA juiz. Papel declarado em config/crons.py.
- CONSUMIDOR (chamado, pergunta, tela, folha) le a lampada, nunca re-julga. Divida: `juizes_por_varredura` no placar.
- (historia: LAPIDES.md#lap-64)

## 4b. CRITERIO DE ENCERRAMENTO DO ESTRUTURAL (corte Ronald 13/09)
Texto integral, historia e a RODADA 3: **app/docs/ESTRUTURAL.md** (lookup).
- 7 familias x 3 contratos (1 JUIZ por pergunta; 1 ESCRITOR por entidade, com selo de idempotencia "2x = mesmo estado, 1 linha de trilha, 0 duplicata"; PARAMETRO consumido ou rotulado "sem efeito") + 1 global (BUG 128), allowlist ZERO. O teto e **20**, perguntado e nunca digitado (L-100, TETO-20-SEM-FAMILIA-SEM-CADASTRO): juiz unico `core/contratos_estruturais.py::total()`; placar `contratos_estruturais: N/20` e LINHA HAIKU "arquitetura: N de 20". Celula verde = a classe de teste existe E toda lista de excecoes esta VAZIA. **Em 20/20 o estrutural acaba e violacao vira VERMELHO NO COMMIT, nao fatia.**
- **L-099 — O ESTRUTURAL SE SEPARA DO DADO** (literal): *"O estrutural se separa do dado. Escala errada e batida furada sempre vao existir. O sistema tem de ser 100% coerente, deterministico e idempotente com o cadastro que TEM: se aparece errado no espelho, esta errado em todo lugar do sistema; se aparece certo, esta certo em todo lugar."* Toda divergencia tem UM DONO -- ESTRUTURA, CADASTRO ou BATIDA --, juiz `e6_oraculo.py::dono_da_divergencia` (paridade do dia pela lei do BUG-144, lista `dias_cadastro_x_realidade` do MOTOR). So ESTRUTURA e fatia; CADASTRO e BATIDA vao a lista do admin, e e PROIBIDO cura-los por codigo. Placar `app/core/placar_estrutural.py` (R1..R6 com numero, meta e PROVA; numero sem prova nao conta) e o PRINCIPAL do ESTADO.md; E1-E6 abaixo, aberto por dono. Ordem: R1, R2, R5, R3, R4, R6. PROIBIDO: mudar tolerancia ou tirar caso da lista para o numero cair; fallback; juiz novo.
- RODADA 3 (14/09; ordem do F1 S-DINHEIRO: turno S7-S13 -> chamado -> ausencia com S4-PENDENTE e AUSENCIA-COBRE -> fechamento -> feriado; depois S-TELA e F4-F6; cada item de E3/E4 e sessao S2-S7 fecha marcando celula): texto no ESTRUTURAL.md.

## 5. OS JUIZES (um por dominio - nao clonar)
A TABELA mora em `app/docs/JUIZES.md` -- lookup, saiu daqui na O224 (aval Ronald 10/10 11:0x).
Topologia detalhada (roteamento, middleware, fluxo app x PWA): ver memoria [[mapa-sistema]].

## 6. CONDUTAS QUE JA CUSTARAM CARO
Texto integral com a historia de cada uma: **app/docs/CONDUTAS.md** (lookup).
- TETO TEMPORAL: emissor/juiz de marco ou celula missing compara INSTANTE ou exige FATO ENCERRADO (turno fechado, par completo); teto por DATA nao basta no cross-meia-noite. Selo ponto/tests/test_contract_teto_temporal.py.
- LER ANTES DE AFIRMAR: grep/sed/cat ao vivo, resposta com arquivo:linha.
- LEI ANTES DO PATCH (LEI-PROTEGE-SITIO): grep do arquivo e da funcao em LEIS.md, DOSSIES.md e CORTES.md, citando o L-NNN; selo `bin/tests/test_lei_protege_sitio.sh` no pre-push.
- SITIO COM ZERO CHAMADOR AINDA RESPONDE (L-111): apaga-se no ato (lapide no sitio), com os testes que o sustentavam e o pendente, no MESMO commit.
- CONTRATO DE ENTRADA antes de metrica/regra nova: FONTE (que JA existe) / UNIDADE / UNIVERSO / EXCLUSOES.
- CODIGO DE NEGOCIO: RED evidenciado -> fix -> GREEN -> suite; "conserta ao vivo" e so para infra.
- SELO ANTI-VACUIDADE: selo com mock ou sobre a SAIDA tem caso que MORDE (`test_a_costura_MORDE`).
- Sonda chama a FUNCAO REAL (interceptando a escrita), nunca reconstroi a chamada.
- MEIA-CORRECAO E PIOR QUE NENHUMA: CENSO DE ESCRITORES fechado antes de filtro/regra nova.
- SILENCIAR EXIGE prazo + tripwire + item de fila.
- GIT: add por PATH, nunca -A; commit na hora; nunca 2 push simultaneos; pull/reset/checkout/restore so com tar e proposito; nao commitar sem ok. O push do fabricante (`[ASSINATURA-EC-P256]`, `TICKETS: ... lote`) e dele: `git fetch` e olhar antes de empurrar.
- PATCH: ler o VIVO; ancora unica (assert count==1); montar em memoria -> py_compile -> gravar; skip-guard idempotente; ancorar por grep, nunca por linha.
- SHELL: docker cp + `manage.py shell --command "exec(open('/tmp/x.py').read())"`; `tenant_command <cmd> --schema=juliani`.
- Papel/holerite em h+min, sonda em decimal: 167h31 = 167,51. Converter ANTES de declarar divergencia.
- FRONT SEM SMOKE NAO SOBE (BUG 73): `static/js/`, service worker ou template base (base.html/base_app.html e o que eles incluem) so com smoke de clique do Ronald nas DUAS cascas; `core/tests/test_smoke_chromium.py` e o piso, nao o teto; JS proprio por `bin/node_check.sh`.
- TODO SELO DE FRONT RENDERIZA AS DUAS CASCAS, ou prova pelo grafo de includes de cada raiz. Selo `core/tests/test_selo_modal_nao_nasce_vazio.py::test_MORDE_as_DUAS_cascas`.
- Rebuild de imagem: nunca instalar item por item; o custo e o rebuild.
- ZONA INVIOLAVEL (incidente 27/08): POST em porta HTTP de prod (Client em /responder/, /validar-inline/, /bater/...) E ESCRITA. Smoke de porta so (1) em banco lateral juliani_ref_* DENTRO do sombra_db, nunca saas_hasner; (2) em atomic() com raise; (3) contagem antes/depois na 1a rodada, 1 caso. Client() grava ip=127.0.0.1 no LogAuditoria.
- BANCO SOMBRA (E2): so `bin/sombra.sh --refazer`, nunca scripts/restore_tenant.sh; SOMBRA-INCREMENTAL por `--incremental`, e `--cobre INI FIM` guarda o DIFF de folha; settings `config.settings.sombra`, container sem rede. SOMBRA-MASCARA-2 apaga a GEO: cron de geo se mede em PROD em dry, nunca na sombra. Dado pessoal: nao exportar.
- GATE TEMPORAL = CRON + ARQUIVO: espera longa por `bin/deploy_agendado.sh agendar` (conferir com `listar`), nunca sleep do Code; espera curta por arquivo de sinal, nunca `pgrep -f`.

## 6b. CCT VIGILANTES LONDRINA — regra de folha com lastro juridico (01/09)
Sindicato #2 (Vigilantes de Londrina/PR), MTE PR000251/2026, vigencia
01/02/2026-31/01/2028, vinculado a praca Londrina (309 colabs, 202 em 12x36).
QUEM CHEGA A ESTA CCT MUDOU NA O211 (pouso B, 08/10) e esta linha dizia o contrario: a PRACA do
posto nao leva mais ninguem a ela. A regua sobe da EMPRESA -- `Empresa.regime_trabalhista` mais a
tabela `core.AplicacaoConvencao` (sindicato + empresa + praca OPCIONAL, a mais especifica vencendo,
empate = piso legal) --, lida so em `core/regua_cct.py::_aplicacao_vigente`. O "vinculado a praca
Londrina" acima e CADASTRO DE COBERTURA: diz quem representa a praca, e nao decide um centavo. Os
309/202 sao o universo da PRACA, medido em 01/09; o universo da CCT hoje e o das empresas com linha
de aplicacao. Lacuna (regime vazio, empresa cct sem linha, CCT fora de vigencia, empate) cai no
piso legal com o motivo em `r['fonte']`, e o contador da lacuna estrutural e
`core/regua_cct.py::empresas_sem_regime` (esperado 0).
A fundamentacao INTEIRA (clausulas literais + parecer) esta em Sindicato.obs e
em core/regua_cct.py; aqui fica so o que evita redescobrir do zero:
- **cl.38-d: no 12x36, ainda que noturno, a hora e NORMAL de 60min** — afasta a
  hora reduzida do Art.73 par.1, e o adicional de 20% SEGUE DEVIDO. Ligado em
  prod 01/09 (`HORA_REDUZIDA_12X36_EM_SECO=False`) apos parecer do advogado: o
  paragrafo unico submetia a alinea ao TRT-PR-AR-329/2001, e essa decisao foi
  SUPERADA pelo ajuste do TST de 2017 (prevalece a norma coletiva).
- **A rubrica 0025 EXISTE para os vigilantes.** Nenhuma clausula embute o
  adicional no piso (cl.3 fixa pisos "para o cumprimento da jornada legal";
  38-d garante "sempre" o adicional). Quem disser o contrario esta trocando
  HORA REDUZIDA por ADICIONAL (historia: LAPIDES.md#lap-71).
- **cl.10** (janela 22h-05h sem prorrogacao) esta aplicada em prod por
  INTERPRETACAO, sem parecer proprio. Fila.
- A CCT em PDF **nao esta no sistema** e o modelo Sindicato nao tem campo de
  anexo. Enquanto nao tiver, o lastro e o texto em `obs`.
- Dos 10 parametros da regua CCT, so 3 chegam a algum calculo. Censo e fila
  lateral no SERIE_T.md.

## 7. CRON (docker exec saas_core, tenant_command --schema=juliani, logs em ~/saas-hasner/logs)
FONTE UNICA = config/crons.py (cron-como-codigo; `bin/crons.sh check|install` renderiza).
(historia: LAPIDES.md#lap-67) Divergiu? o VIVO manda.
A LISTA DE HORARIOS mora em `app/docs/CRONS.md` -- lookup, saiu daqui na O224 (aval Ronald 10/10 11:0x).
CORRENTE SEM MINUTO (ENCADEADOS, corte 29/09 BLOCO-DA-MANHA-SATURADO): (historia: LAPIDES.md#lap-68) Cada elo espera o ARQUIVO do anterior (`logs/fim/<nome>.<emp>.<dia>`),
  porque horario e aposta sobre duracao e arquivo e FATO. A corrente, na ordem:
    ver `app/docs/CRONS.md`.
SUPRA_JUIZ - 9 entradas, 1 por empresa (2,3,4). O nome engana: NAO e' so leitura.
  Mexer em ponto/supra_juiz.py::classificar_dia muda o que e' RETRATADO em prod,
  nao so o relatorio. Sem flag = dry-run (nada escrito).
Todo cron e IDEMPOTENTE por requisito - pode ser adiantado na mao.

## 7b. MODO DE TRABALHO (regras de conduta -- vivem AQUI, nao se repetem em prompt)
PARADA E AUTONOMIA (regra consolidada 31/10; funde, sem mudar o vigente, PAREI-DE-LEI-NAO-DEVOLVE-TURNO, PAREI-SO-LEI,
  AUTORIZACAO PREVIA DA SESSAO, TURNO NAO TERMINA COM FILA 1 NAO VAZIA, PRE-APROVADO x NUNCA PRE-APROVADO,
  AUTONOMIA-DE-CODIGO, ACELERAR-PELA-LEI (1)(3), CURA-MAIS-RESTRITIVA e o "PAREI passa a existir so para" da
  DINHEIRO-EM-COMPETENCIA-ABERTA; a forma viva e `bin/hook_stop_fila1.py` (_SO_BANG) e `bin/disjuntor.py`).
  Apelidos: ESPERA-SEM-TURNO (1), PRE-APROVADOS, REPROVA-E-DESENHO (4; mecanismo `bin/disjuntor.py` B3,
  `linhas_libera_b3`, selo `bin/tests/test_disjuntor.sh`). Selo:
  `bin/tests/test_hook_parei_so_lei.sh` (RED `PAREI: bloco-da-manha-saturado` -> `block`; decisao tecnica
  e pergunta de lei bloqueiam, so o `!` libera). LAPIDES.md#lap-45 LAPIDES.md#lap-46 LAPIDES.md#lap-17 LAPIDES.md#lap-16 LAPIDES.md#lap-11 LAPIDES.md#lap-49.
  AUTONOMIA-DE-CODIGO (11/10 ~12:0x): a frase do item 5 vai ao CORTES.md citando este aval; vale fora de
  S-BATIDA/S-ESCALA (CORTES-DOS-CONTRATOS item 4).
  1. SO O `!` DEVOLVE O TURNO. O hook libera o Stop com `!` na linha do topo do RELATO ou com ESPERANDO PROVADO
     (`ESPERANDO: <produtor> | pid=N | arquivo=<c>`, produtor vivo); tudo o mais e `siga`. Linha de parada: `PAREI: <trava> |
     espera <quem>`, publicada. Trava sem nome nao e trava.
  2. O QUE EXIGE `!` (NUNCA pre-aprovado, e so dele): tocar o GRAVADO de competencia EXPORTADA; dado de ESCALA ou VINCULO
     (inclusive retroativo); apagar ou voltar ao HEAD arquivo que PROD usa; regra de negocio fora do pedido; qualquer
     atalho (fallback, pular teste, SKIP_TESTS); `ponto/motor_calculo_v2.py` (secao 4). "Dinheiro nao se moveu" nao amplia
     aval; escopo do aval e literal (LEI-AKITA 9).
  3. PERGUNTA DE LEI NAO DEVOLVE TURNO. Falta lei, predicado novo de juiz ou duas candidatas: linha `lei: <a que falta>` no
     TOPO do RELATO COM NUMEROS, e a esteira SEGUE o proximo item da ORDEM VIVA que nao depende dela; a resposta entra quando
     vier. Ate 20/20 o Code nao numera nem propoe lei nova por iniciativa propria: lei nova so nasce de CORTE LITERAL dele (L-117). Se uma lei do desenho ESCRITA ja responde (celula soberana/
     lampada 4a, juiz unico, escritor unico, exportada = Pauta DP sem tocar o gravado), nao espera: `DECIDIDO PELA LEI <L-NNN
     ou secao>: <opcao> | contestavel`, reversao antes e prova depois.
  4. PERGUNTA DE DESENHO IDEM. Classe de bug NOVA que muda o DESENHO (um padrao, nao um caso) e reprovacao do verificador final
     (DISJUNTOR B3) vao ao TOPO do RELATO como PERGUNTA DE DESENHO com numeros, a obra para e as outras seguem.
  5. DECISAO TECNICA E DO CODE, em UMA linha no RELATO, sem aval: horario de cron, desenho de encaixe, caminho de codigo;
     ordem e encaixe na fila por DEPENDENCIA DE CODIGO; qual raia ou agente pega qual obra e quando uma obra registrada
     entra como COMPLEMENTO de outra; destino de achado de esmeril (cura no marco, complemento, linha de fila); a frase `corte Ronald: juiz <nome>
     nasce` no CORTES.md quando a funcao JA existe, JA e a unica autoridade MEDIDA, MESMO predicado e universo IGUAL antes e
     depois (as tres medidas na sessao -- declara autoridade que ja era unica, nunca juiz novo). Bug PROVADO com mais de uma
     cura: aplica a mais RESTRITIVA (ou as duas, se nao conflitam), porque no RELATO, segue -- `!` so se cair no item 2.
  6. PRE-APROVADO (rotina, nunca contorno): construir, medir, provar na sombra, curar, fabricar fatia, rodar DIFF, cadastrar
     com trilha; push pela regua e nova tentativa com a CAUSA em uma linha; empurrar se o integrador nao empurrar em 20 min;
     passar etapa com a porta da anterior PROVADA no RELATO; pausar/religar esteira e timers COM TRILHA (quem, por que,
     condicao de saida; `bin/pausar.sh`); registrar BACKLOG e publicar RELATO; copiar para /tmp ANTES de remover; apply de
     dinheiro em competencia NAO exportada (regra DINHEIRO). Tudo aplicado OU parado vira linha em PENDENTES com a frase de
     reversao; o Ronald revisa quando olhar.
     PRE-APROVADO-AMPLIA (corte Ronald 31/10 23:1x), sempre com linha `PRE-APROVADO <item>: o que, obra, log` no RELATO:
     (a) cancelar job na fila ou rodando de VOLTA SUPERADA da mesma obra; (b) topo do RELATO: linha respondida ou superada
     desce com `RESPONDIDA por <aval/hora>`, e nunca duas linhas da mesma obra dizendo coisas opostas; (c) ordem das vagas:
     fila 1 (contrato 20 e itens do placar L-099), depois negocio, depois instrumento, teto 2 construtores conferido ANTES
     de abrir; (d) agente esperando job proprio so para se o job estiver destacado (trava/setsid) E uma obra de fila 1
     precisar da vaga; (e) sobra de opus final: "trava o contrato 20 ou item do placar L-099?" sim -> fila 1, nao ->
     ESMERIL-V2. CONTINUA com aval: lei, todo `!`, dinheiro e a liberacao de B3 (DESENHO/ERRO-BOBO nao e escrita pela
     propria obra).
  7. FILA 1 NAO PARA. Fechou item da fila 1 (bloco OBRAS do BACKLOG): pega o proximo NO MESMO TURNO. A esteira nunca para por
     aval: fatia bloqueada -> proxima do BACKLOG; trava A sempre >= 4; nunca ocioso esperando clique. Para so por trava
     NOMEADA do item 1 ou 2, portao declarado (ex. `esteira.dinheiro_fechado`) ou quarentena.
  8. MEDIDA por marco no RELATO: pousos de produto nas ultimas 24 h e horas paradas em PAREI esperando o Ronald.

RAIAS E POUSO (regra de PISTA consolidada 31/10; funde, sem mudar o vigente, FILA-2-EM-RAIA-PROPRIA, POUSO-EM-COMBOIO e
  SUITE-UMA-VEZ-POR-ARVORE, RAIA-NAO-FICA-PARADA, RAIA-VERDE-POUSA L-105, DOCS-NO-MARCO L-106, L-107 (secao 2), ESMERIL-DO-MARCO,
  MARCO FECHADO L-108, JUIZ-ANTES-DO-SITIO, AGENTE-NAO-ESPERA, AGENTE-NO-MODELO-CERTO (parte restante)).
  Apelidos: HIGIENE-DE-CONTEXTO (L-108). Selo: `bin/tests/test_handoff_sessao.sh`.
  (PISTA.md#fila-2-em-raia-propria)(PISTA.md#pouso-em-comboio)(PISTA.md#raia-nao-fica-parada)(PISTA.md#juiz-antes-do-sitio)(PISTA.md#agente-nao-espera)
  LAPIDES.md#lap-52 LAPIDES.md#lap-15 LAPIDES.md#lap-53.
  Contador de raias (5) + selo no pre-push: fila 2 apos CELULA-TURNO-FECHA e O145; ate la, conduta. HANDOFF: HEAD/origin, status,
  ORDEM VIVA e item (do `hook_stop_fila1`, nao de leitor novo), PAREI, avais de hoje nao cumpridos, processos vivos, proximo passo.
  1. CONSTRUIR EM COPIA (LEI-AKITA 10). Obra que nao cruza ARQUIVO com a que esta na raia anda em AGENTE PROPRIO ao lado
     (o teste e arquivo em comum, nao familia nem app); BOs abertos simultaneos: DOIS sem arquivo em comum, UM se tocam o
     mesmo arquivo (os outros limites do BOS-EM-RAIA-UM-POR-VEZ no topo do BACKLOG). Quem anda nao escreve parada. Fila 2
     (UI) em raia `wt-ui`, so `templates/`, `static/` e testes de tela, deploy `--sem-migrate`, nucleo intocado; template na
     arvore principal JA ESTA NO AR (secao 2), e tira-lo e `!` (voltar arquivo de prod).
  2. JUIZ ANTES DO SITIO. Fatia que toca pergunta SEM juiz em `core/juizes.py` comeca declarando o juiz: predicado ja unico e
     MEDIDO = frase no CORTES.md (PARADA item 5); predicado NOVO = pergunta no topo ANTES de curar qualquer sitio.
  3. AGENTES. Agente nasce para CONSTRUIR ou VERIFICAR e termina quando entrega; nunca espera -- polling e da sessao principal,
     em shell de fundo; entrega dependente de suite = comando + caminho do log. Modelo pelo papel no ato de abrir, sem settings:
     construir cura de negocio e verificador = modelo da sessao; leitura de log = sonnet; o resto da leitura e sonda = regra
     CODEX. Agente ocioso ou relendo log de obra fechada e encerrado no ato.
  4. SUITE UMA VEZ POR ARVORE. A arvore que a suite certificou e a que POUSA: entre veredito e merge nenhum `.py` muda (nem na
     copia nem na principal); antes do commit, `cmp` arquivo a arquivo contra a copia, citado no corpo do commit. Raia pronta
     PERGUNTA (`git worktree list` + `git status --porcelain`) se a outra fica pronta em ate 30 min: sim -> UMA copia main +
     as duas, UMA suite, pousos em sequencia com o hash certificado; nao -> roda sozinha (uma linha no RELATO); vermelho no
     comboio = separa; conflito de merge = fora do comboio. Ninguem espera mais de 30 min.
  5. POUSO (L-105/L-107). Raia com suite VERDE POUSA antes de abrir fatia nova na fila 1, PRE-APROVADO salvo PARADA item 2:
     `git merge --no-ff --no-commit` -> resolver -> commit -> `bin/deploy.sh` num ato so, nada no meio (MERGE_HEAD vazio =
     foi ff e o pouso JA COMECOU). INSTRUMENTO (`bin/`, selo de host, portao do push) nao pousa com PRODUTO: pouso proprio,
     depois. Raia que nao pode pousar: motivo em UMA linha (lei, `!` ou janela). ESTADO conta raias com commit a frente do
     main ha mais de 6 h (esperado 0).
  6. ESMERIL DO MARCO. A espera da suite e o esmeril DAQUELE marco: censo so do que ele deixou orfao, zero chamador de
     producao provado, orfao apagado no mesmo marco (L-111). Achado de fora do marco = linha na fila, nunca trabalho agora.
  7. MARCO FECHADO, DEPOIS COMPACTA (L-108). Fechou marco (item da fila 1, cura com DIFF publicado, lote empurrado): docs
     entram no MESMO commit (L-106; commit so de `app/docs/` fora de marco nao existe, excecao unica o derivado do
     `bin/pos_push.sh`; proibido allowlist e chamar de marco o que nao fechou item); `bin/handoff_sessao.sh`; painel "MARCO
     FECHADO -- pode compactar"; nunca no meio de DIFF ou apply. Hooks PreCompact geram e SessionStart(compact) imprime o
     `HANDOFF-SESSAO.md` (teto 60 linhas, de COMANDO). PUSH: um por MARCO.

CODEX (regra de PISTA consolidada 31/10; funde, sem mudar o vigente, GPT6-CODEX-SO-LEITURA, CODE-CHAMA-CODEX,
  CODEX-PADRAO-PRO, CODEX-PRE-VERIFICADOR (+SUITE-ANTES-DO-OPUS), CODEX-TRIAGEM-INDEPENDENTE, CODEX-ESCREVE-RED,
  DESPACHO-POR-PAPEL, MAIS-CODEX, ACELERAR-PELA-LEI (2) e a parte de sonda/censo da AGENTE-NO-MODELO-CERTO; os nomes
  antigos seguem validos como apelido nos scripts e no CORTES.md; historia em LAPIDES).
  (PISTA.md#gpt6-codex-so-leitura)(PISTA.md#code-chama-codex)(PISTA.md#codex-padrao-pro)(PISTA.md#codex-pre-verificador)
  (PISTA.md#codex-triagem-independente)(PISTA.md#codex-escreve-red)(PISTA.md#despacho-por-papel)(PISTA.md#mais-codex)(PISTA.md#acelerar-pela-lei)(PISTA.md#agente-no-modelo-certo)
  1. AMBIENTE. Codex CLI (plano Pro, so limite semanal; tmux `codex-hasner`) so LE: `~/codex_ws/<hash>` por `git archive`
     do HEAD, `--sandbox read-only --ask-for-approval never`, SEMPRE pelo wrapper
     `bin/codex_pergunta.sh [--sonda <script.py>] [--anexo <arq>]... <hash> <pergunta> <resposta>` com CODEX_OBRA e
     CODEX_PAPEL (leitura|censo|verificador|sonda) obrigatorios. Dado de prod so por `--sonda` (envelope unico
     `bin/sonda_leitura.sh`, saida raspada antes do Codex; CODEX-SONDA-LEITURA no CORTES.md). A via pelo chat (Ronald) le a
     resposta por `bin/gerar_codex_estado.py`. UNICA escrita permitida: teste RED de lote em copia PROPRIA, sob
     CONTRATO-DE-INSTRUMENTO, revisado pelo Code antes do uso. PROIBIDO: outro sandbox, `-C` na arvore viva, chamada sem
     wrapper, `--dangerously*`, Codex escrevendo codigo de producao, mexer em config do Code, cron novo.
  2. DESPACHO. Todo aval, laudo ou BO e DECOMPOSTO antes de qualquer trabalho, 1 linha no RELATO:
     `DESPACHO <id>: a,b -> Codex | c -> sonnet | d -> opus`. CODEX (primeiro, sempre) = leitura de codigo, rastrear classe,
     censo pela regra, quem-mais-le, autopsia de escritor/leitor, contra-exemplo, sonda de prod (contagem, censo de frota,
     caso de colab, conferencia de laudo), pre-verificacao de diff. SONNET (`haiku` se so contar/grep) = leitura de LOG;
     e leitura/sonda SO com rc LIMITE (75) ou falha do Codex, sonda que escreve na sombra (atomic+raise), ou motivo na
     linha do DESPACHO -- sem motivo, o verificador reprova. OPUS (modelo da sessao) = construir cura de negocio e a
     leitura final. Sessao principal nao faz sonda, DIFF de leitura nem contagem inline. Construtor que acha divergencia
     ("por que X difere") investiga no maximo 10 min, despacha e segue construindo ou espera em shell de fundo. Itens
     independentes saem em paralelo ate o teto 6 -- teto so MEDIDO (CPU/load da VM e /health/ do saas_core na rajada).
  3. PERGUNTA. Sem a hipotese de quem pergunta ("confirme que X" = o verificador reprova); premissa da intencao vai como
     PERGUNTA, nunca como dado. Volta 1 e FINAL perguntam, alem do checklist, QUEM ESCREVE e QUEM LE cada campo/valor que
     o diff le ou grava (arquivo:linha), com app/docs/JUIZES.md anexo; voltas do meio podem ser so o delta.
  4. CONFERENCIA. O Codex e 2o leitor de outra linha; NAO certifica sozinho, NAO prova dado de prod (sem banco) e NAO
     substitui a leitura do Code: toda afirmacao dele usada em decisao e conferida abrindo o arquivo:linha citado (amostra
     declarada) antes de virar aval, RED, DIFF ou pouso. Censo em modelo menor que vire base de cura: idem.
  5. PRE-VERIFICADOR. (a) TODO construtor, ANTES de declarar pronto, `CODEX_PAPEL=verificador` com: diff do hash da
     copia; LEIS dos sitios tocados (grep LEIS/CORTES pelos arquivos); e a lista de classes que ja reprovaram obra -- 2o
     juiz que duplica juiz registrado, 2o escritor fora da porta, relogio lido fora da borda, derivacao paralela de fonte
     unica, selo/RED sem mordida, ordem de trava, pouso antes do pre-requisito. (b) Achado: confere arquivo:linha;
     procedente -> corrige e chama de novo; descartado ("fila", "fora do escopo", "nao procede") -> 1 linha de motivo E vai
     ANEXO ao opus (resposta + linha da triagem), que decide se e fila ou bloqueia. (c) Suite INTEIRA da arvore da obra
     VERDE, depois UM opus final (SUITE-ANTES-DO-OPUS); opus reprovou -> volta ao (a), nunca direto a outro opus. (d) Mais
     de 3 voltas Codex na mesma obra -> sobe ao opus sem esperar. (e) ESCOPO = todo sitio que o diff cria ou toca MAIS os
     escritores e leitores dos campos e funcoes que ele le ou muda, mesmo fora do diff. PROIBIDO: Codex aprovar sozinho o
     pouso; pular o opus final antes do corte.
  6. MEDIDA E POSTMORTEM. Linha por obra no RELATO do pouso: voltas Codex, procedentes/descartados, leituras opus, "opus
     reprovou depois do Codex aprovar". Toda reprovacao do opus e CLASSIFICADA pelo Codex (CODEX_PAPEL=censo, com os
     achados do opus, TODAS as .q/.r da obra e as linhas de triagem; devolve por achado o trecho da pergunta e da
     resposta): (a) nao viu com o tema PRESENTE = falha do Codex (a unica) | (b) viu e a triagem descartou = falha da
     triagem, com motivo | (c) fora do alcance (RED, suite, dado de prod) = nao conta | (e) NAO PERGUNTADO = falha de quem
     perguntou | (d) NENHUM viu = MODO-COMUM (regra propria). Rejeicao audita PRIMEIRO quem perguntou e quem triou. Apos 10
     obras o Code publica a tabela; falha do Codex = 0 -> PROPOE o corte de rebaixar o opus final a amostra (lei nova: so
     por corte). ESTADO: placar por papel (leitura/sonda/investigacao no Codex x em opus/sonnet nas 24 h, motivo de cada
     desvio; itens de leitura de codigo fora do Codex esperado 0) e ALERTA "descartes do Codex nas 24 h" com a lista,
     VERMELHO se houver falha do Codex sem a tabela (instrumento, CONTRATO antes, pouso proprio).
     (5) retroativo 10:4x: NOTURNO (e) 1,5 / (b) 2,3 / (b) parcial 4,6, falha Codex 0.
MODO-COMUM: achado que Codex E opus deixaram passar = classe (d): linha `MODO-COMUM:` no topo do RELATO e obra ESTRUTURA na frente da fila 1, fechada com juiz + MAPA + selo que morde mutante. (PISTA.md#modo-comum)
CONTRATO-DE-INSTRUMENTO: instrumento, hook, wrapper, cron ou regra de pista novos so depois de CONTRATO de 5 linhas no RELATO (quem chama, quem espera, falha/limite, medida, quem desliga), mandado UMA vez ao Codex. (PISTA.md#contrato-de-instrumento)
PLACAR-DA-S3-CONTA-EXERCICIO (corte Ronald 29/09 ~15:1x).
  O-DIA-PAGO S3: EXERCICIO, nao CAMINHO; **0** com os 2 verdes:
    (a) **0 leitor mostrando numero de motor sem o rotulo** `'sem apuracao ainda'`;
    (b) **0 leitor com derivacao propria de dinheiro** (por AST).
  **A L-095 nao cede**; corte 23/09 19:2x: sem fechamento, MOSTRAR rotulado; morre "quem CHAMA o motor". LAPIDES.md#lap-18

DINHEIRO (regra consolidada 31/10; funde, sem mudar o vigente, AUTORIZACAO PREVIA item 2, O TXT E FOTOGRAFIA DO CALCULO,
  REGEN-EM-EXPORTADA, DINHEIRO-EM-COMPETENCIA-ABERTA, AVAL-DE-CRITERIO e DEPLOY JA; L-092, L-110).
  LAPIDES.md#lap-55 LAPIDES.md#lap-59 LAPIDES.md#lap-08 LAPIDES.md#lap-09 LAPIDES.md#lap-10 LAPIDES.md#lap-48 LAPIDES.md#lap-12.
  L-009/L-082: fora do item 1 (comp. CORRENTE) o `!` segue: paga, vinculo/escala retroativo, atalho.
  Sem criterio escrito no aval, vale a L-009. DIFF da troca de motor: ANTECIPADO. Backup/renome do TXT: admin.
  1. COMPETENCIA NAO EXPORTADA: apply de dinheiro (recalculo, relavratura, cura do motor) e PRE-APROVADO com os TRES juntos:
     (a) arquivo de reversao em `logs/` gerado ANTES (snapshot dos campos por colaborador); (b) competencia exportada INTACTA,
     hash antes/depois provado; (c) PROVA DE FROTA no RELATO DEPOIS, no MESMO marco (frota, nunca amostra -- L-110). DIFF
     antecipado NAO e exigido aqui. Trilha: PENDENTES "aplicado, revise"; DIFF que surpreende -> nao aplica, PENDENTES
     "PAROU: <motivo>", segue outra fatia.
  2. COMPETENCIA EXPORTADA (L-092): o GRAVADO (`FechamentoMensal`) nao muda por caminho generico; e `!`. Quando move: DIFF de
     frota publicado ANTES, reversao em `logs/`, prova depois, e SO pela porta REGEN-EM-EXPORTADA -- staff-only verificado na
     porta; trilha com quem, quando, motivo e hash do FechamentoMensal antes/depois; regenera as CELULAS do trecho e refaz o
     FechamentoMensal; TXT PARCIAL DE RETIFICACAO com hash publicado; diferenca pago x devido sai como Pauta DP com os dois
     numeros. Com `!` DE CRITERIO o apply segue sem nova parada se (a) o DIFF move SO os campos-alvo nomeados, (b) todo outro
     campo de todo colaborador da ZERO, medido na sombra contra o GRAVADO, (c) o total fica na faixa aprovada; violacao =
     PAREI com a tabela.
  3. O TXT E FOTOGRAFIA, nao pagamento: UM arquivo VIGENTE por empresa/competencia, gerar de novo SUBSTITUI; nome CANONICO
     (empresa + mes + ano, sem versao/data/hash); versoes anteriores guardadas em silencio em `folha/models.py::
     ExportacaoDominio` (conteudo, hash, data, quem, substituida por), invalidadas so pela porta `invalidar` (nunca toca
     conteudo nem hash), NUNCA apagadas e NUNCA na UI -- consulta pelo HAIKU-EXPORT. Nenhuma trava nem aviso de tela por
     exportada/trancada/paga: correcao provada entra em qualquer competencia; importacao e pagamento sao do Dominio.
  4. TROCA DE MOTOR (v3): portao = bateria pela regra + propriedades fixas (secao 0, item 13 / L-110); DIFF v2 x v3 UMA vez,
     como IMPACTO por rubrica, divergencia com dono L-099 -- nunca validacao.
  5. DEPLOY JA: cura commitada com suite verde vai ao ar NA HORA por `bin/deploy.sh` (reload gracioso, prova de casca,
     collectstatic), inclusive mudanca de casca; smoke em prod logo depois, no RELATO; o reload das 03:30 e rede de
     seguranca. Migration pendente segura o deploy.

PONTUALIDADE-SO-CONTRA-O-DIA-DECLARADO (L-084): texto vigente em `app/docs/LEIS.md`. (historia: LAPIDES.md#lap-13 LAPIDES.md#lap-50 LAPIDES.md#lap-51)

RELATO, BACKLOG E PENDENTES (regra consolidada 31/10; funde PROMPT-NAO-SE-REPETE, DIETA DE PROSA L-109, RELATO-CURTO L-116,
  MORATORIA L-117 (texto em PARADA 3), PAUSA COM DONO, DUAS FILAS, PENDENTE-SAI L-118, REGRA-DE-PRAZO).
  Apelidos: MORATORIA-DE-LEI (L-117), PENDENTE-SAI-PELA-CONDICAO-ESCRITA (L-118; 1o uso PORTA-EXTRA-E-CONTADOR),
  RELATO-CURTO-SELO. LAPIDES.md#lap-14 LAPIDES.md#lap-54.
  REGRA-DE-PRAZO: juiz existente so se responde a MESMA pergunta; o novo tambem vai a `JUIZES_DO_PRAZO`.
  1. PROMPT recebido = linha em `app/docs/PROMPTS.md`; titulo repetido = resposta em UMA linha ("ja recebido HH:MM, estado X,
     onde Y"), nada refeito (contador `prompts_repetidos`); prompt que pede obra vira item no bloco OBRAS do BACKLOG no mesmo
     turno -- sem item, nao foi recebido.
  2. RELATO: secao de marco com teto de 25 linhas (titulo, linha LEI-AKITA, `arquivo::funcao` mudados, numeros medidos, PROVA
     ao lado de cada ato, commit, INCOMPLETO em lista); historia vai ao corpo do commit e, virando regra, a LAPIDES.md; secao
     escrita nao se reescreve. Pergunta de lei no topo com numeros, linha PAREI, tabela PENDENTES e DIFF de frota antes de
     apply em exportada NAO mudam. RELATO com mais de 3 dias vai a RELATO-ARQUIVO.md no mesmo commit.
  3. BACKLOG: celula de ESTADO com no maximo 300 caracteres (fato, numero, commit). Historia de regra deste arquivo vai a
     LAPIDES.md com ponteiro: mover, nunca apagar, sem tocar a regra.
  4. DUAS FILAS: `PENDENTES_RONALD.json` so guarda DECISAO dele (aval, `!`, corte, smoke); o que o fabricante fez vai a
     FABRICANTE.md (selo `core/tests/test_selo_pendentes_so_decisao.py`). Pausa: `bin/pausar.sh <arquivo> "<quem>" "<por que>"
     "<condicao de saida>"`.
  5. PENDENTE DE `core/juizes.py` SAI SEM PERGUNTA (L-118) se os QUATRO juntos: condicao de saida escrita em corte dele e
     medida na FROTA pela funcao REAL com universo nao vazio declarado; numero bate a condicao LITERAL; no MESMO commit a
     condicao vira CONTADOR no codigo (vigia, esperado, caso que morde); linha no RELATO com numeros e frase de reversao.
     Fora disso = pergunta no topo. A O141 segue proibindo `del` calado e baixa sem motivo e ponteiro.
  6. REGRA DE PRAZO (so RAIA-DO-CONTRATO-20): cron de JUIZES_POR_VARREDURA que decide por prazo pergunta a um juiz de
     JUIZES_DO_PRAZO se ja houver (DIFF publicado, sem perguntar); numero igual para todos os clientes e sem juiz -> frase
     `corte Ronald: juiz <nome> nasce` citando esta regra, numero intocado; so vai ao topo o numero que JA varia por
     empresa/praca ou o caso sem autoridade da casa.

## 8. MANUTENCAO DESTE ARQUIVO
Atualizar quando mudar: infra, comando de teste, juiz, zona inviolavel ou cron.
NAO atualizar por feature. Compacto e a funcao. Camada 2 (um arquivo por app, com
"quem manda aqui e o que quebra se eu mexer") ainda NAO existe - e a proxima.
