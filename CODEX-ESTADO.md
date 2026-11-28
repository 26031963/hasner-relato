# CODEX-ESTADO

> Ultima resposta processada: **28/11/2026 12:42** · publicado em **28/11/2026 12:51** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 28/11 12:42 · sessao da9ef6c862e5 · CRON-15-SOMBRA-CABE · leitura · origem=Code

**Estado** — Medida inicial e dossiê colhidos. A construção aguarda resposta à pergunta de desenho. `relato.txt:15024,15027`  
**Trava** — Só os comandos em série têm p95 de 5.095 s, ante teto bruto de 2.200 s. Quatro trilhas ainda dão 3.997 s com as dependências e a ordem de escrita. `relato.txt:27,15024`  
**Próximo passo** — Ronald responder à pergunta de desenho. O aval exige manter todas as etapas e o cron de prod, com última prorrogação em 04/12. `app/config/crons.py:344–345`; `relato.txt:27,15027`; `backlog.txt:2248`  
**Lei aberta** — Zero perguntas de lei no dossiê. `relato.txt:15027`

- **Medida por etapa** — Faltam os recortes `.etapas` com início e fim datados por rodada, as duas linhas de espera e os TSVs dos comandos. Durações completas por rodada e hash do HEAD [nao verificado]. As linhas abaixo são da cópia. `logs_sombra.txt:1–3`; `bin/sombra.sh:380–398`; `bin/sombra_trava.sh:27–31`
- **Duas esperas pela trava** — Antes de `refazer` e de `bloco`. Cada espera vai de `t0` ao cálculo de `espera_s` em `sombra_espera.jsonl`. `bin/sombra.sh:438,451`; `bin/sombra_trava.sh:17,27–31`
- **Refazer e preparar dumps** — Refazer inteiro entre as travas ≈357 s no relato. Preparo, da garantia do banco até `sombra: dumps`, em `bin/sombra.sh:238–253`. `relato.txt:15024`
- **Recriar e restaurar** — Drop/criação `bin/sombra.sh:255–256`, restore `public` `bin/sombra.sh:257–258`, restore `juliani` `bin/sombra.sh:259–260`, resumo `bin/sombra.sh:263–264`.
- **Máscaras e selo pessoal** — Máscara 1 `bin/sombra.sh:266–268`, máscara 2 `bin/sombra.sh:272–274` (≈261 s no relato), selo pessoal `bin/sombra.sh:275–277`. `relato.txt:15024`
- **Divergência, completude e carimbo** — Das consultas ao resumo de divergência, seguido do carimbo. `bin/sombra.sh:286–301,302–305`
- **Bloco** — Preparo, lavra e lista `bin/sombra.sh:329–350`, execução `bin/sombra.sh:351–387`, fechamento `bin/sombra.sh:390–407`. P95 da soma dos comandos de 15–28/11 = 5.095 s. P95 ponta a ponta [nao verificado]. `relato.txt:15024`
- **Por comando** — O TSV mede de `t0` à gravação da duração. A reposição da lavra ocorre antes de `t0` e precisa de marcações próprias para ser medida. `bin/sombra.sh:378–385,398`
- **Três mais longos** — `selo_leitores` 1.222 s, `lavrar_he` 670 s, `lavrar_cxr` 634 s no p95. Na rodada reportada, 1.174 s, 670 s e 634 s, respectivamente. `relato.txt:15024,15132`
- **Hipótese de encaixe** — Testar leituras em lote na origem de `espelho_do_colab`, mantendo as três etapas, seus gatilhos e suas escritas. Na rodada reportada, mesmo zerando o custo das três na conta, sobram 2.617 s de comandos antes do refazer. O encaixe exige ganhos também no restante do bloco. Ganho efetivo [nao verificado]. `app/folha/porta_export.py:475–483`; `app/ponto/services/he_pendente_lavrado.py:80–81`; `app/ponto/services/cxr_na_lista.py:62–78`; `app/config/crons.py:1581–1583,1808–1820`; `relato.txt:15024,15132`

## 28/11 12:40 · sessao 88bb13529e50 · RAZAO-DA-SOBRA · censo · origem=Code

**Resumo · Perímetro da cura**, sob `app/` — `escala/{orfa_classe,utils}.py`, `escala/services/{leitor_celula,furos_diarios}.py`, `ponto/{turnos,juiz_batida,status_dia,supra_juiz}.py`, `ponto/services/{cartorio,fechamento,espelho,dia_pago,bordas_realizado,cluster_espurio,vigia_pareador_celula,vigia_de_hora,ponta_pequena}.py`, `ponto/portas/he.py` e `ponto/management/commands/{tripwire_celulas,recusar_ponta_pequena}.py`. `backlog.txt:2109`, `2ce5e49b5:app/escala/tests/test_razao_sobra_fonte.py:165`, `2ce5e49b5:app/ponto/tests/test_razao_sobra_leitores.py:30`, `app/docs/RELATO.md:14627`  
**Perímetro, continuação** — `chamados/{juizes,models,reconciliador,signals}.py`, `chamados/services/{declaracao_texto,materializacao,fabrica_da_celula,relavra_via,disputa_emissao,fio,cobrar_dia,vigia_fio}.py`, `folha/{causas,export}.py`, `relatorios/{cartao_pela_celula,pdf_espelho}.py`, `templates/ponto/espelho.html`, `core/juizes.py` e `docs/CORTES.{md,json}`. `2ce5e49b5:app/ponto/tests/test_razao_sobra_leitores.py:18`, `2ce5e49b5:app/ponto/tests/test_razao_sobra_leitores.py:155`, `2ce5e49b5:app/chamados/tests/test_razao_sobra_e2_fabrica.py:10`, `2ce5e49b5:app/chamados/tests/test_razao_sobra_repasse.py:11`, `a4a9b6bfe:app/core/juizes.py:1333`  
**Obras com alterações no perímetro fora do `main`** — PAR-PELO-TURNO, E1-PREVISAO-24H, ABONO-X-SAIDA-ANTECIPADA, DESLIGADO-PORTA-DO-CHAMADO, E4C-F5-INSTANTE, E4D-CONTRATO-ROTA-B, DIA-DA-BATIDA-JUIZ-UNICO, FERIADO-SEM-LIMITE-DO-SUPRIMIDO e O30-F3. Alterações sem commit [nao verificado]. `worktrees.txt:1`, `worktrees.txt:57`, `worktrees.txt:63–71`, `worktrees.txt:76`, `worktrees.txt:83–90`, `worktrees.txt:105`, `worktrees.txt:126`   [nao verificado]
**Interseção** — `turnos.py:PAR/ABONO/DIA`; `utils.py` e `espelho.py:PAR/E1`; `fechamento.py:PAR/E1/DIA/FERIADO`; `cartao_pela_celula.py` e `pdf_espelho.py:PAR`; `leitor_celula.py` e `cartorio.py:E1`; `dia_pago.py:E1/FERIADO`; `export.py:E1/E4C`; `core/juizes.py:E1/O30`; `CORTES.{md,json}:E1`; `chamados/{models,signals}.py:DESLIGADO/E4C`; `chamados/juizes.py`, `declaracao_texto.py` e `cluster_espurio.py:DESLIGADO`; `materializacao.py:DESLIGADO/E4C`; `disputa_emissao.py:DESLIGADO/E4C/E4D`; `fio.py:E4D`. `worktrees.txt:57`, `worktrees.txt:63–71`, `worktrees.txt:76`, `worktrees.txt:88`, `worktrees.txt:105`, `worktrees.txt:126`  
**Veredito — não.** A PAR ainda cruza a cura, e a ordem vigente põe a RAZAO-DA-SOBRA depois da PAR. `app/docs/RELATO.md:14627`, `app/docs/BACKLOG.md:1926`, `worktrees.txt:88`

- **PAR-PELO-TURNO** — c1v13 `84f8a88d4`, com c1v16 `aebeee441` no `wt-codex-par-pelo-turno-111717`. Nenhuma das duas pousou no `main`. `app/docs/RELATO.md:14630`, `worktrees.txt:88`
- **E1-PREVISAO-24H** — `8d81e64c5` no `wt-codex-e1-previsao-24h-083708`, E1a c2 `339368be7` e base c3 `8cc31f45c`. Esses commits seguem fora do `main`. `worktrees.txt:66–67`, `worktrees.txt:105`
- **ABONO-X-SAIDA-ANTECIPADA** — `6cc0c2a7e` e `f38300c20` alteram `turnos.py` e seguem fora do `main`, inclusive na cópia c1h `90b161ba3`. A c1d pousou por `19810f51c`. `worktrees.txt:6`, `worktrees.txt:11`, `worktrees.txt:17`, `worktrees.txt:57`
- **DESLIGADO-PORTA-DO-CHAMADO** — base `540ab73c0` e c5 `c28de9ee3`, ambas fora do `main`. `worktrees.txt:50`, `worktrees.txt:63`, `app/docs/RELATO.md:15149`
- **E4C-F5-INSTANTE** — c3 `330e159fb`, candidata a pouso `dbc5b8b4e`, cura5 `63680b06e`, cura6c `486a1ffc4` e OS6 `c9fded10a`, todos fora do `main`. `worktrees.txt:68–70`, `worktrees.txt:106–107`
- **E4D-CONTRATO-ROTA-B** — cura4 `f300431af`, fora do `main`. `worktrees.txt:71`
- **DIA-DA-BATIDA-JUIZ-UNICO** — F2 `a4d3675c7` e F4e `fb8194db0`, fora do `main`. `worktrees.txt:64–65`
- **FERIADO-SEM-LIMITE-DO-SUPRIMIDO** — c4d `28eb07f5d`, fora do `main`. **FERIADO-UM-JUIZ** pousou por `a497c0cce`, com sobreposição em folha, espelho e relatórios. `worktrees.txt:76`, `worktrees.txt:111`
- **O30-F3** — `ac4ad27ce` e candidata `e2955107b`, fora do `main`. **CODEX-CONSTROI-MOTOR** tinha alterações nos arquivos de cortes nas cópias c11/c12 `0b3939987`/`eea12c760`; a versão que pousou foi a c2, por `279aac944`. `worktrees.txt:81–84`, `worktrees.txt:126`, `app/docs/RELATO.md:14898`
- **Demais pousos com arquivos em comum** — PONTA-PEQUENA itens 2–3 `b8f12a692` e v5b `71efa33d7`; PAR-RELAMPAGO `e216b756a`; PORTA-CONFERE-TRAVA `eb76e7be7`; CODIGO-DOMINIO-VALIDO `3545ebc07`; RETRATO-PDF item 2 `b82a5ef08`. `backlog.txt:2109`, `backlog.txt:2440`, `app/docs/RELATO.md:108`, `app/docs/RELATO.md:495`, `worktrees.txt:61`, `worktrees.txt:96`

## 28/11 12:35 · sessao e4470d0b72a6 · RAZAO-DA-SOBRA · leitura · origem=Code

**Resumo**  
**Estado** — Obra descongelada. `reds1` commitada em `2ce5e49b5`, com seis módulos só de teste. Na VM, 52 testes, 30 falhas, 1 erro e 5 pulados (`relato.txt:14632`, `relato.txt:14654`).  
**Trava** — O perímetro da cura ainda cruzava PAR c1v13 e E1 na medição das 15:25. Cruzamento e vaga atuais [não verificado] (`relato.txt:14633`, `relato.txt:14645`, `relato.txt:14661`).  
**Próximo andável** — Codex leitura/censo reconfere o cruzamento antes do despacho. Com interseção zero e vaga, segue para Codex construtor (`relato.txt:14633`, `relato.txt:14645`, `relato.txt:14661`).  
**Lei aberta** — Zero no despacho, pelo aval RAZAO-LEIS-1-6-2611 (`relato.txt:14220`).

**Brief do Codex**

- Na sessão com as worktrees, contar e listar os arquivos em comum entre o perímetro do dossiê §8 e o `git diff --name-only <merge-base>..HEAD` de PAR e E1. Registrar o resultado por worktree e a disponibilidade de vaga (`relato.txt:14633`, `relato.txt:14645`, `relato.txt:14661`).
- Na cópia, seguir a sobra do pareador e da grade até o classificador e a ata. Conferir onde o instante vira `HH:MM` e onde dia de trabalho sem marco vazio recebe `INDEFINIDA` (`app/ponto/turnos.py:1192`, `app/escala/utils.py:1748`, `app/escala/utils.py:1883`, `app/escala/orfa_classe.py:53`, `app/escala/orfa_classe.py:68`, `app/ponto/services/cartorio.py:761`).
- No censo AST, localizar leituras cruas de `orfas` e `n_orfas`, incluindo ata antiga, espelho, PDF e efeito no fechamento. O aval de construção prevê o selo catraca e `sobras_sem_razao` (`app/escala/services/leitor_celula.py:180`, `app/ponto/services/espelho.py:703`, `app/relatorios/pdf_espelho.py:477`, `app/folha/causas.py:43`, `app/ponto/services/fechamento.py:1117`, `backlog.txt:2109`).
- **REDs registrados** — R1–R6, A1, A2, V1 e V2 (`relato.txt:14654`). Enunciados individuais e causa do erro [não verificado].

## 28/11 12:34 · sessao 4ff1449fdd7f · SONDA-ESPURIAS-RAMO2 · leitura · origem=Code

- **Estado.** Sonda s2 pronta com `rc 0`. Os quatro lados cobriram 963 colabs nas empresas 2/3/4, comp. 11, com 99 sobras dia a dia. RAMO2 mudou 0 dos 14 colabs do teto. (backlog.txt:2246, relato.txt:14229)
- **Trava.** A pausa da sonda foi retirada às 14:43. A c2u do RAMO2-CRUZA aguarda vaga do opus. (relato.txt:14632, 14447)
- **Próximo passo andável.** Ronald envia o aviso do CASO-LITERAL-14 para contraprova. A etapa do Codex na sonda já foi concluída. (backlog.txt:2111, relato.txt:14229)
- **Lei.** Nenhuma pergunta aberta registrada para a sonda. A regra do RAMO2 está decidida. A âncora preserva o trabalhado e L-097 só classifica a HE. (backlog.txt:2183, 2246)

## 28/11 12:28 · sessao 4b8be07b248d · TESTE-DO-ESTRANHO · leitura · origem=Code

**RESUMO — acertou 0 · parcial 5 · travou 0.** A nota considera todos os pontos explícitos de cada tarefa no gabarito. `gabarito.md:2`, `gabarito.md:6`, `gabarito.md:10`, `gabarito.md:12`, `gabarito.md:19`

- **T1 — parcial.** Acertou a ordem `ORDEM-VIVA-2811-1200`, as duas sequências e a meta. Faltou explicitar que o hook usa `ORDEM-VIVA-TOPO` como autoridade e que o marcador ficou na `1511-NOITE` até 28/11. Destino → glossário. `resposta.md:8`, `app/docs/BACKLOG.md:8`, `app/docs/BACKLOG.md:1926`, `app/docs/BACKLOG.md:1927`, `bin/hook_stop_fila1.py:188`
- **T2 — parcial.** A causa confere. As perguntas 61520/61521 foram emitidas em 23/11 às 12h36, quando a célula 122468 previa 11h/12h, e ficaram pendentes após a regeneração das 12h42 retirar o intervalo. É órfã de regeneração, com cura de classe no E2 `retirar_contexto`. Faltou identificar o 40588 como um dos seis da `CHAMADO-PRESO-CLASSE-PORTAS-2611` e dizer que a porta `admin_encerra` já estava no ar, sem tela que a oferecesse. Destino → diagrama. `resposta.md:18`, `resposta.md:20`, `app/docs/RELATO.md:15208`, `app/docs/BACKLOG.md:836`, `app/docs/BACKLOG.md:1640`
- **T3 — parcial.** Identificou o juiz da dobra, `MotorBase._feriado_dobra_no_dia`. Faltou explicar que a guarda L-097 soma os minutos a `minutos_fora_janela`, zera o trabalhado e colapsa o par. O `FERIADO-UM-JUIZ` pousou em `a497c0cce`. A cura da virada no 6x1, `dc876f5d1`, foi reprovada por perder 240 minutos e ficou fora do pouso. Destino → diagrama. `resposta.md:28`, `resposta.md:30`, `app/core/juizes.py:320`, `app/ponto/motor_calculo_v2.py:941`, `app/ponto/motor_calculo_v2.py:1585`, `app/docs/RELATO.md:14867`, `app/docs/RELATO.md:14872`, `app/docs/RELATO.md:15231`
- **T4 — parcial.** Faltaram o comando exato da suíte, a verificação na sequência geral, o arquivo de reversão em `logs/` e a linha `LEI-AKITA` no commit. Rito do gabarito → cópia `wt` → `bin/suite.sh --dir <copia> --motivo "pouso <OBRA>"` na árvore do pouso → verificador ou Opus final → reversão prévia em `logs/` → `git merge --no-ff --no-commit`, commit com `OBRA:` e `LEI-AKITA`, e `bin/deploy.sh` no mesmo ato. Reversão do merge → `git revert -m 1 <merge>` e `bin/deploy.sh`. `resposta.md:38`, `CLAUDE.md:318`, `CLAUDE.md:105`, `CLAUDE.md:347`, `CLAUDE.md:429`, `CLAUDE.md:456`, `CLAUDE.md:63`, `CLAUDE.md:91`, `bin/commit_msg.sh:29`, `app/docs/RELATO.md:15081`
- **T5 — parcial.** Começou pelo handoff e pulou a comparação com a ordem mais nova e a consulta explícita ao hook. A sequência é marcador × ordem mais nova → saída de `bin/hook_stop_fila1.py` → `PAREI` e `lei` no topo do RELATO → `python3 bin/rc_sem_colher.py` → processos e jobs vivos (`bin/trava_teste.sh --quem`, `bin/codex_vagas_estado.py` e pista) → `HANDOFF-SESSAO.md`. Destino → diagrama. `resposta.md:50`, `gabarito.md:20`, `app/docs/BACKLOG.md:1926`, `bin/hook_stop_fila1.py:188`, `CLAUDE.md:272`, `CLAUDE.md:278`, `CLAUDE.md:338`, `bin/rc_sem_colher.py:13`, `bin/trava_teste.sh:16`, `bin/codex_vagas_estado.py:2`, `app/docs/HANDOFF-SESSAO.md:3`

- TESTE-DO-ESTRANHO T1: explicitar a autoridade de `ORDEM-VIVA-TOPO` e registrar o marcador preso na ordem anterior -> glossário (`bin/hook_stop_fila1.py:188`, `app/docs/BACKLOG.md:1927`)
- TESTE-DO-ESTRANHO T2: ligar o 40588 à fatia dos seis chamados e mostrar a porta admin ainda sem tela -> diagrama (`app/docs/BACKLOG.md:836`, `app/docs/BACKLOG.md:1640`)
- TESTE-DO-ESTRANHO T3: mostrar onde a L-097 zera o par e registrar a reprovação da cura da virada 6x1 -> diagrama (`app/ponto/motor_calculo_v2.py:1585`, `app/docs/RELATO.md:14872`)
- TESTE-DO-ESTRANHO T4: registrar comando da suíte, verificação, arquivo de reversão e linha `LEI-AKITA` no rito de pouso -> inventário (`CLAUDE.md:105`, `CLAUDE.md:429`, `CLAUDE.md:456`, `CLAUDE.md:63`) [nao verificado]
- TESTE-DO-ESTRANHO T5: ordenar o diagnóstico da fila vazia, da comparação do marcador até o handoff -> diagrama (`gabarito.md:20`, `app/docs/BACKLOG.md:1927`, `app/docs/HANDOFF-SESSAO.md:3`)

## 28/11 12:27 · sessao 56360ec16f5a · FERIADO-L097-MOTOR · leitura · origem=Code

## Resumo

- A FERIADO-UM-JUIZ pousou em `a497c0cce` e o apply da 11 rodou. A L-097 segue fora do `main`, com a c2 `dc876f5d1` reprovada pelo ver2. `app/docs/RELATO.md:14867,14872,15264–15270`
- Seguem abertos os 240 min perdidos no feriado D+1 e a divergência de data entre autorização, pendência e oráculo. Faltam o DIFF da 10 e a lista final da J.A com reversão [nao verificado]. `app/docs/RELATO.md:14867,14872,14876`
- O `!` da herança já foi respondido no topo e o v2 manda tratar D+1 em volta própria **depois** do pouso da L-097. As perguntas de lei (0)–(2) têm decisões restritivas contestáveis. `app/docs/RELATO.md:16,29,14877`
- Já dá para transportar a cura do feriado do próprio turno, limitada à guarda, e revalidar pagamento, pendência e segmentos na base pós-FUJ. `app/docs/RELATO.md:40,14867,14877`
- Próximo passo na vaga opus é portar c1+c2, rodar RED e mutantes, inteira e opus final. Medir o DIFF da 10 e a lista da 11 na árvore candidata ao pouso. `app/docs/BACKLOG.md:1926,2190` · `app/docs/RELATO.md:14867,14872`

## BRIEF c3 — opus construtor

- **Base.** `main` HEAD `96e66c5f2`. Cherry-pick em ordem `05ec19a6f` → `dc876f5d1`, ou rebase dos dois desde `1d2eb496c`. A c2 isolada pressupõe a c1. A prévia de merge foi limpa em leitura. `app/docs/RELATO.md:14796–14797,14867,15270`
- **Guarda no HEAD.** Inserir o retorno no ramo de folga com escala certa, após `escala_certa_no_dia`. Exigir feriado, ciclo fora de `CICLOS_FERIADO_SIMPLES` e `_dia_do_turno_do_par(entrada)[0] == dia`. A partição usa o mesmo juiz e escolhe a rubrica. `app/ponto/motor_calculo_v2.py:963–978,1579–1590,3740` · `app/ponto/calculador/regras.py:410–432`
- **PROIBIDO.** Sem `except` novo, sem relógio solto, motor só no hunk da guarda. Fora do delta de produção ficam o laço de herança, data da autorização, publicação da pendência, partição, fechamento, guarda de competência e TXT. `app/docs/PROMPTS.md:3176` · `app/docs/RELATO.md:14867,14877` · `app/docs/PLANO-PADROES.md:97–103`
- **CENSO, escritores no HEAD.** Motor grava dia herdado e mapa por período, zera e colapsa o par, publica alerta e `{dia, hora, minutos}`. A porta de HE grava a decisão que o motor consulta. `app/ponto/motor_calculo_v2.py:2513–2522,2570–2574,1585–1602,2833,1569–1571` · `app/ponto/portas/he.py:44`
- **CENSO, escritores do pago.** Fechamento monta contexto por dia e por período, `particionar_ft` pode remapear dia/vínculo e escolhe rubrica, fechamento salva `FechamentoMensal` e chama as duas lavras de `DiaPago`. `app/ponto/services/fechamento.py:332–351,995–999,1181,1237,1300` · `app/ponto/calculador/regras.py:410–432` · `app/ponto/services/dia_pago.py:372`
- **CENSO, leitores da pendência.** Espelho traduz `sentido='dia'` e agrega dias. Retrato lê o espelho e grava `MetricaSnapshot`, Gestão de HE lê o retrato, portão do TXT lê pendência e decisão, PDF lê pendência e alertas. `app/ponto/services/espelho.py:757–763,939` · `app/ponto/services/he_pendente_lavrado.py:138,183–195` · `app/ponto/services/gestao_he.py:426–434` · `app/folha/porta_export.py:181–201,687–692` · `app/relatorios/pdf_espelho.py:935–945,968–979`
- **CENSO, leitores do par e do pago.** Oráculo cruza dia/hora da pendência com as pontas efetivas e consome a partição. TXT lê o mensal. Calendário lê `DiaPago`. `app/ponto/calculador/alimentacao.py:73–83,114–123,353–371,514–518` · `app/folha/export.py:434,460,734–742` · `app/colaboradores/services/calendario.py:684–689`
- **RED — feriado 6x1 no próprio turno, janela ativa.** Reativar o teste pulado. Certa e incerta totalizam 4 h com e sem autorização. A rubrica segue a partição: FT certa soma 4 h, ou HE100-feriado quando o feriado é a causa da folga. Conferir mensal e lavras. Mutante: retirar o retorno da guarda para a certa. `app/ponto/tests/test_feriado_um_juiz_porta.py:547–563` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:579–605`
- **RED — feriado em D, dois turnos.** D 18h–22h e D+1 03h–07h, vão de 5 h. Em 6x1→6x1 e 6x1→12x36, exigir 4 h HE100 em D e somente o par das 03h com 240 min pendentes. Mutante: retirar a igualdade do dia do turno e voltar a liberar pelo dia herdado. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:810–841,859–862` · `dc876f5d1:app/ponto/motor_calculo_v2.py:1599`
- **RED — pendência isolada.** Feriado liberado sem `sentido='dia'`. Folga comum com uma pendência de 240 min às 07h no motor e no espelho. Mutante: suprimir só o `append` da pendência e manter o bloqueio. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:903–923` · `app/ponto/motor_calculo_v2.py:1585–1602`
- **RED — segmentos.** Um segmento de 240 min no feriado. Folga comum sem segmento, com um `par_bloqueado_sem_previsto`. Mutante: omitir só o colapso de `efetivo_saida`, mantendo zeragem e pendência. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:925–936` · `app/ponto/motor_calculo_v2.py:1591–1602`
- **Controles da guarda.** Folga comum sem autorização zera e com autorização paga. Feriado sem folga e previsto zero segue bloqueado. 12x36 segue bloqueado mesmo com dobra ligada pela CCT. Mutantes respectivos: dispensar feriado, dispensar folga, trocar o teste de ciclo por `feriado_em_dobra`. Reexecutar os controles de intermitente por base e ciclo. `app/docs/RELATO.md:29` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:607–651` · `app/ponto/tests/test_feriado_um_juiz_porta.py:358–378`
- **Controles da virada.** Em 6x1→12x36 com feriado D+1 e 12x36→6x1 com feriado D, ambos os pares ficam bloqueados. Um único turno do feriado com pares 21h–00h e 01h–05h paga 7 h. Mutantes: trocar o ciclo pelo vínculo vizinho e trocar o dia do turno pela data civil da entrada. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:864–895` · `app/ponto/motor_calculo_v2.py:963–978`
- **D+1 na volta própria.** Preservar os dois `expectedFailure` de 4 h HE100, sem troca e em 12x36→6x1. O opus final decide se isola a asserção do pago para erro de fixture não contar como prova. Na volta D+1, o mutante é restaurar o bloqueio pelo dia herdado. `app/docs/RELATO.md:16,14877` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:850–857,880–884`
- **(0)** Feriado sem folga e com previsto zero entra na liberação? Corte atual mantém o bloqueio. `app/docs/RELATO.md:29`
- **(1)** 12x36 em folga real de feriado mantém o bloqueio integral ou só o corte da rubrica 59-A/CCT? Corte atual mantém o bloqueio integral. `app/docs/RELATO.md:29`
- **(2)** FT como única rubrica com trabalhadas zero entra no TXT? Classificar como INCOMPLETO na lista. O HEAD tem ressalva para HE100-feriado emitível. A classificação dos casos da frota está [nao verificado]. `app/docs/RELATO.md:29` · `app/folha/export.py:931–945,977–998`
- **Aceite.** Fixar janela, decisão e `agora` na fixture. Para cada RED, mostrar a asserção esperada na base sem a cura e a queda com seu mutante. Rodar inteira e anexar a triagem ver2 ao opus final. Execução na base c3 [nao verificado]. `app/docs/PROMPTS.md:3224–3225` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:903–923` · `app/docs/RELATO.md:14867–14872`
- **Rito de pouso.** DIFF da 10 pela L-113, publicado antes do pouso/ato de frota, com aviso ao DP. A triagem chama a 10 de exportada. O HEAD registra paga à mão, sem TXT e sem guarda. Situação atual em prod [nao verificado]. Preservar a 09 guardada. Lista J.A da 11 por colab, dia e minutos, com reversão antes do apply L-092 da L-097. `app/docs/RELATO.md:25,14872,14876` · `app/ponto/services/fechamento.py:37–50,603–629,1847–1853` · `app/docs/BACKLOG.md:2190`
- **Padrão.** `Single Writer Principle` / `juiz único`: a guarda decide a efetividade do par e a partição existente decide a rubrica. `app/docs/GLOSSARIO.md:12` · `app/ponto/motor_calculo_v2.py:1585–1602` · `app/ponto/calculador/regras.py:428–432`

## 28/11 12:25 · sessao ca3cc270be5e · TESTE-DO-ESTRANHO · leitura · origem=Code

## RESUMO

**5 resolvidas · 0 parciais · 0 travadas.** A ordem vigente está em `app/docs/BACKLOG.md:1926` e a causa do chamado 40588 em `app/docs/RELATO.md:15208`.  
**Principal limite:** o handoff é um retrato de 28/11 às 11h58. O estado ao vivo da sessão, das filas e da produção ficou [nao verificado]. `app/docs/HANDOFF-SESSAO.md:3`

## T1. O que está em construção agora, em que ordem e por quê?

**RESPOSTA** — Vale a **ORDEM-VIVA-2811-1200**. A meta é zerar os problemas atribuídos à Hasner na J.A, competência 11, até segunda, com uma sonda no domingo à noite. Codex, pista 2, pista 3 e Opus verificador devem receber trabalho sempre que houver item executável. Pré-requisito pendente herda a vez, e obras que disputam o mesmo arquivo precisam ser sequenciadas. `app/docs/BACKLOG.md:8` · `app/docs/BACKLOG.md:1926` · `CLAUDE.md:318` · `CLAUDE.md:444`

- **Opus construtor:** CUSTO F1 v7 → OBRA-PRONTA-ACORDA → FERIADO-L097-MOTOR c2 → DESLIGADO c4/c5 → CUSTO F2/F3. A F1 v7 já tinha commit em cópia e aguardava a coleta dos resultados de testes e mutantes. `app/docs/BACKLOG.md:1926` · `app/docs/RELATO.md:15212`
- **Codex e pistas:** canário E4C a3/c4 e E4D v4 → C7-TELA-ADMIN-ENCERRA, para os 88 casos → RETRATO-PORTA-PELO-MOTIVO, depois da OS6 → RAMO2-B e RAMO2-HERANCA-VAO → PAR → ORFAS-FOLHA-E-ALARME itens 2 e 3 → E4d → faixa do BOTAO → RAZAO-DA-SOBRA → CRON-15-SOMBRA-CABE → SONDA-ESPURIAS/CASO-LITERAL-14 → E1C. No último registro, a E4C a3 estava em aceite, a E4D v4 tinha entrega para aceite e a C7 tinha código em cópia, com testes, verificação e smoke pela frente. `app/docs/BACKLOG.md:1926` · `app/docs/RELATO.md:15234` · `app/docs/RELATO.md:15237` · `app/docs/RELATO.md:15241`
- **FERIADO-UM-JUIZ** já estava no ar às 11h36, com o apply da competência 11 registrado como concluído e o pacote ao Fernando preparado. O envio ficou com Ronald. `app/docs/RELATO.md:15231` · `app/docs/RELATO.md:15266` · `app/docs/RELATO.md:15270`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — a ordem e o andamento estão no BACKLOG e no RELATO. A ocupação das vagas após o retrato ficou [nao verificado]. `app/docs/HANDOFF-SESSAO.md:18` · `app/docs/BACKLOG.md:1926` **TEMPO** — ~1 min e 30 s [nao verificado].

## T2. Chamado 40588, de 20/11

**RESPOSTA** — As perguntas **61520 e 61521** foram emitidas em **23/11 às 12h36**, quando a célula **122468** previa os marcos de **11h e 12h**. Às **12h42**, a célula foi regenerada para **07h–19h sem intervalo**, e as duas perguntas continuaram pendentes. A causa é **órfã de regeneração**. `app/docs/PROMPTS.md:3223` · `app/docs/RELATO.md:15208`

A correção da classe pertence à fatia **CHAMADO-PRESO** da obra **ESCRITOR-UNICO-DE-CHAMADO**, no **E2 `retirar_contexto`**, após o pouso da E4C. **INTERVALO-SEM-PREVISTO**, da RAIA-INTERVALO, encontrou a causa. **C7-TELA-ADMIN-ENCERRA** é a via manual para o admin encerrar esses casos com motivo e trilha. `app/docs/BACKLOG.md:835` · `app/docs/BACKLOG.md:2654` · `app/docs/PROMPTS.md:3229`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — busquei `40588` no PROMPTS, RELATO e BACKLOG. O log bruto da sonda ficou [nao verificado]. `app/docs/RELATO.md:15208` **TEMPO** — ~1 min [nao verificado].

## T3. Feriado trabalhado em folga no 6x1

**RESPOSTA** — Na classificação mensal vigente desde a competência **11/2026**, os minutos apurados de feriado trabalhado em folga no **6x1** entram uma vez como **HE 100% de feriado**, ou seja, em dobro. O teste de 6x1 sem escala certa confirma **5h30 na rubrica de feriado e zero em horas comuns**. `app/folha/export.py:385` · `app/ponto/services/fechamento.py:1002` · `app/ponto/tests/test_he100_dia_sem_previsto.py:250`

O juiz da dobra é **`MotorBase._feriado_dobra_no_dia`**, que usa o feriado do **dia de início do plantão** e a regra do ciclo. Só o `12x36` consta da lista padrão de ciclos com feriado simples. **`rubrica_da_folga`** escolhe a rubrica, e **`feriado_vira_folga`** verifica se o feriado causou a folga daquele vínculo. `app/ponto/motor_calculo_v2.py:941` · `app/ponto/motor_calculo_v2.py:961` · `app/ponto/motor_calculo_v2.py:3740` · `app/ponto/calculador/regras.py:371` · `app/escala/models.py:1384`

A janela **L-097** ainda pode zerar o par antes da classificação. No apply registrado, os **13 dias de 10 colaboradores 6x1** do pacote seguiram com os mesmos valores. A cura para pagar o feriado diretamente pelo ciclo pertence à **FERIADO-L097-MOTOR**, que estava fora do HEAD registrado. `app/ponto/motor_calculo_v2.py:1562` · `app/ponto/motor_calculo_v2.py:1585` · `app/docs/PROMPTS.md:3176` · `app/docs/RELATO.md:15270`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — conferi motor, calculador, fechamento e testes. O pagamento efetivo ficou [nao verificado]. `app/ponto/calculador/regras.py:428` · `app/docs/RELATO.md:15270` **TEMPO** — ~1 min [nao verificado].

## T4. Como pousar uma obra com reversão

**RESPOSTA**

1. **Prepare em cópia.** Confirme escopo, `!` e B3. Faça RED antes da cura. Para mão única, concorrência, schema ou multi-tenant, passe por censo, mutantes, revisão do Codex e Opus final após a suíte. `CLAUDE.md:28` · `CLAUDE.md:255` · `CLAUDE.md:274` · `CLAUDE.md:423` [nao verificado]
2. **Certifique a árvore que vai pousar.** Exija suíte inteira verde. Se rodar na pista, exija `bin/pista_certifica.sh` com rc 0 ou rode na VM. Confira os arquivos com `cmp` contra a cópia certificada e registre a prova no commit. `CLAUDE.md:104` · `CLAUDE.md:129` · `CLAUDE.md:347`
3. **Prepare a reversão antes do ato.** Registre no RELATO `git revert -m 1 <commit-do-merge>` seguido de `bin/deploy.sh`. Para apply de folha, grave antes o snapshot dos campos por colaborador e prove depois a integridade das competências exportadas por hash, com prova de frota no mesmo marco. Para alterar gravado de competência guardada, exija `!`, DIFF prévio e a porta staff. `app/docs/PROMPTS.md:3202` · `app/docs/RELATO.md:351` · `CLAUDE.md:455`
4. **Pouse e publique no mesmo ato.** Faça `git merge --no-ff --no-commit` → resolva → commit com `OBRA: <id>` → `bin/deploy.sh`. Produto pousa primeiro e instrumento tem pouso próprio. O deploy confere os portões, migra quando aplicável, recarrega as três cascas e prova as rotas. `CLAUDE.md:284` · `CLAUDE.md:352` · `app/docs/INFRA.md:38` · `bin/deploy.sh:416`
5. **Faça o smoke e registre o resultado.** Se reprovar, execute o revert previsto, republique e repita as provas. Se houve escrita de dados, siga também a reversão específica pela porta, restaurando só os campos-alvo do snapshot. `CLAUDE.md:477` · `app/docs/RELATO.md:4575` · `app/docs/RELATO-ARQUIVO.md:32867`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — conferi o procedimento no CLAUDE, no deploy e em exemplos de reversão no RELATO. A execução ficou [nao verificado]. `CLAUDE.md:347` · `bin/deploy.sh:308` · `app/docs/RELATO.md:4575` **TEMPO** — ~2 min [nao verificado].

## T5. O Code parou e a esteira está vazia

**RESPOSTA** — Eu conferiria nesta ordem:

1. **Ordem e parada declarada.** Abra o `HANDOFF-SESSAO.md`, o topo do `RELATO.md` e o bloco `OBRAS` do BACKLOG. Confira se `ORDEM-VIVA-TOPO` aponta para um item aberto e a qual obra pertence o `PAREI: !` ou `ESPERANDO:`. Marcador ausente ou apontando para obra fechada pode dar a impressão de fila vazia. O hook de Stop hoje pode encerrar com item aberto e emitir só um aviso. `app/docs/HANDOFF-SESSAO.md:18` · `CLAUDE.md:272` · `bin/hook_stop_fila1.py:286` · `bin/hook_stop_fila1.py:1499`
2. **Sessão e trabalho em voo.** Veja o painel tmux e a última atividade do transcript. Para `ESPERANDO`, confira produtor, PID e arquivo de sinal. Confira também as travas de teste e sombra e os `.rc` concluídos ainda sem registro de coleta. Se houver recusa, leia o motivo na trilha do disjuntor ou do wrapper Codex. `bin/alarme_sessao_ociosa.py:18` · `CLAUDE.md:272` · `bin/handoff_sessao.sh:252` · `bin/rc_sem_colher.py:6` · `bin/disjuntor.py:4` · `bin/codex_pergunta.sh:517`
3. **Filas e portões da esteira.** Use o relatório `bin/esteira_status.sh --lista` para conferir a fila de fatias e a de integração. Veja se há fatia pronta e livre fora da integração, pronta com portão ou já no ar aguardando smoke. Confira `esteira.pausada` pelo dono, motivo e condição de saída, além de árvore vermelha, quarentena e controlador de teto. `bin/esteira_status.sh:3` · `bin/esteira_status.sh:41` · `bin/esteira_status.sh:63` · `bin/fabrica_estado.py:24` · `bin/esteira.sh:53`
4. **Descubra por que a vaga ficou vazia.** Se a fábrica estiver habilitada, confira o timer e o fim de `logs/fabricante.log`. A mensagem `registro sem item livre com alvo vivo` indica que o fabricante rodou e ficou sem alvo elegível. Se a sessão estiver tocando a fila, confira o próximo item da ordem, seu dossiê, pré-requisito e vaga. Uma trava de lei ou `!` de uma obra deixa seguir a próxima independente. `bin/hasner-fabricante.timer:7` · `bin/trava_a_vazia.sh:46` · `CLAUDE.md:278` · `CLAUDE.md:412` · `CLAUDE.md:444`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — o handoff registra `esteira.pausada` na árvore do host e indica que havia item executável na fila 1. O motivo da pausa e o estado ao vivo do tmux e dos timers ficaram [nao verificado]. `app/docs/HANDOFF-SESSAO.md:14` · `app/docs/HANDOFF-SESSAO.md:27` **TEMPO** — ~2 min [nao verificado].

## 28/11 12:23 · sessao ff0ce7fa6409 · - · - · origem=Ronald (tmux)

**T3 — RESPOSTA.** Desde a competência 11/2026, o fechamento lança todos os minutos apurados de feriado trabalhado em folga no 6x1 como HE 100% de feriado, uma vez. O TXT lê essa rubrica do fechamento mensal. O teste do 6x1 sem escala certa mostra 5h30 na rubrica de feriado e zero em horas comuns. `app/ponto/services/fechamento.py:997`, `app/ponto/services/fechamento.py:1002`, `app/folha/export.py:385`, `app/folha/export.py:457`, `app/ponto/tests/test_he100_dia_sem_previsto.py:250`.

O juiz da dobra é `MotorBase._feriado_dobra_no_dia` em `app/ponto/motor_calculo_v2.py:941`. Ele consulta o calendário no dia de início do plantão e a regra do ciclo. `CICLOS_FERIADO_SIMPLES` contém apenas `12x36`. A escolha da rubrica passa por `rubrica_da_folga`, que consulta, quando há escala certa, se o feriado causou a folga do vínculo. `app/ponto/motor_calculo_v2.py:955`, `app/ponto/motor_calculo_v2.py:961`, `app/ponto/motor_calculo_v2.py:3740`, `app/ponto/calculador/regras.py:371`, `app/ponto/calculador/regras.py:428`, `app/escala/models.py:1384`.

O relato registra o apply da competência 11 em produção e informa que 13 dias de 10 colaboradores 6x1 seguiram sem alteração. A correção da janela de autorização nesses casos ficou com `FERIADO-L097-MOTOR`, fora do HEAD registrado. `app/docs/RELATO.md:15264`, `app/docs/RELATO.md:15270`, `app/docs/PROMPTS.md:3176`.

**AUTO-AVALIAÇÃO.** Resolvi. `app/ponto/motor_calculo_v2.py:941`, `app/ponto/tests/test_he100_dia_sem_previsto.py:250`.

**ONDE TRAVOU.** A regra e a ressalva de produção aparecem no calculador, nos testes e no relato. O estado atual de produção está [nao verificado]. `app/ponto/calculador/regras.py:371`, `app/ponto/tests/test_he100_dia_sem_previsto.py:250`, `app/docs/RELATO.md:15270`.

**TEMPO.** Cerca de 1 minuto [nao verificado].

## 28/11 12:23 · sessao a92def7fa5f7 · - · - · origem=Ronald (tmux)

### T4. Pousar uma obra com reversão

1. Preparar na cópia do HEAD, com RED antes da cura, escopo literal e linha LEI-AKITA no commit. Classificar mão única ou mão dupla e conferir `!` e B3. Mão única, concorrência, schema e multi-tenant exigem censo, mutante, revisão Codex e Opus final após a suíte. Mão dupla pode pousar com as pendências registradas. (`CLAUDE.md:37–40`, `:62–69`, `:255–265`, `:274–287`, `:423–433`) [nao verificado]
2. Antes do pouso, registrar a reversão no RELATO. Para um merge de código, preparar `git revert -m 1 <merge>` seguido de `bin/deploy.sh`. Se a obra aplicar dinheiro em competência não exportada, salvar em `logs/` o snapshot dos campos por colaborador, provar por hash que as exportadas ficaram intactas e publicar a prova de frota depois, no mesmo marco. Na competência guardada, exigir `!`, DIFF prévio e a porta staff. (`app/docs/PROMPTS.md:3202`, `app/docs/RELATO.md:351`, `CLAUDE.md:455–469`)
3. Obter suíte inteira verde na árvore que vai pousar. Na pista, exigir `bin/pista_certifica.sh` com rc 0, senão rodar na VM. Manter os bytes certificados e registrar o `cmp` no commit. (`CLAUDE.md:104–107`, `:129`, `:347–351`)
4. Pousar antes de abrir nova fatia. Na árvore principal, fazer `git merge --no-ff --no-commit`, resolver, commitar com `OBRA: <id>` e executar `bin/deploy.sh` no mesmo ato. `MERGE_HEAD` vazio após o merge indica fast-forward e deploy imediato. Instrumento segue em pouso próprio, depois do produto. (`CLAUDE.md:284–287`, `:352–356`, `app/docs/INFRA.md:65–78`)
5. Conferir os portões do deploy, a migração quando aplicável, o HUP conjunto e as três provas de rota. Fazer o smoke em produção e registrar no RELATO. Se o smoke exigido reprovar, executar o revert previsto, repetir deploy e provas. Para front abrangido pela L-072, o push espera o clique nas duas cascas. (`bin/deploy.sh:120–125`, `:308–325`, `:416–419`, `CLAUDE.md:477–479`, `app/docs/LEIS.md:99`, `app/docs/RELATO.md:4575`, `:4938`)
6. Se também houve escrita de dados, seguir a reversão específica. O exemplo da O211 primeiro reverte e republica o código, depois restaura apenas os campos-alvo pela porta `restaurar_fechamento`. (`app/docs/RELATO-ARQUIVO.md:32867–32874`)

**Auto-avaliação** — resolvi. **Onde travou** — nenhuma etapa documental após consultar `CLAUDE.md:255`, `app/docs/INFRA.md:38`, `bin/deploy.sh:308` e `app/docs/RELATO.md:4575`. Execução em produção [nao verificado]. **Tempo** — cerca de 2 min [nao verificado].

## 28/11 12:22 · sessao bd5d477b8c38 · - · - · origem=Ronald (tmux)

**T2 — Resolvi.** As perguntas 61520 e 61521 do chamado 40588 cobravam 11h e 12h do dia 20/11. Elas foram emitidas em 23/11 às 12h36, quando esses marcos constavam no DNA da célula 122468. Às 12h42, a célula foi regenerada com jornada das 07h às 19h e sem intervalo previsto. As duas perguntas continuaram pendentes. [app/docs/PROMPTS.md:3223](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/PROMPTS.md:3223) · [app/docs/RELATO.md:15208](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/RELATO.md:15208)

A causa é da classe `ORFA-DE-REGENERACAO`. A correção pertence à fatia `CHAMADO-PRESO-CLASSE` da obra `ESCRITOR-UNICO-DE-CHAMADO`, no E2, após o pouso da E4C. `INTERVALO-SEM-PREVISTO`, da `RAIA-INTERVALO`, foi a investigação. `C7-TELA-ADMIN-ENCERRA` é a via manual pelo admin. [app/docs/BACKLOG.md:835](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/BACKLOG.md:835) · [app/docs/BACKLOG.md:2654](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/BACKLOG.md:2654) · [app/docs/PROMPTS.md:3229](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/PROMPTS.md:3229)

**Onde travou.** A busca por `40588` e pelos horários encontrou a conclusão em `PROMPTS.md`, `RELATO.md` e `BACKLOG.md`. O relato cita `logs/INTERVALO-SEM-PREVISTO/l1/sonda`, cujo conteúdo bruto está [não verificado]. [app/docs/RELATO.md:15208](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/RELATO.md:15208)

**Tempo aproximado.** 1 min [não verificado].

## 28/11 12:09 · sessao 15877447f162 · CUSTO-DE-COORDENACAO-OBRA-PRONTA-ACORDA · leitura · origem=Code

**Resumo.** `ESPERA_UNICA` consulta `rc_sem_colher.py` na entrada. Um `.rc` sem colher já acorda a espera, com retorno `3` do leitor. O laço só detecta `.rc` novo. `bin/rc_sem_colher.py:86–107` `bin/disjuntor.py:1964–1971`  
**ORDEM VIVA.** `_item_da_ordem_viva` julga o item em curso pelo marcador, hoje `ORDEM-VIVA-2811-1200`. Lista estruturada de membros e próximo passo por obra [nao verificado]. `app/docs/BACKLOG.md:8,1926` `bin/hook_stop_fila1.py:188–249`  
**Job vivo e `PAREI`.** Há prova de PID e censo, lease via `flock`, estado da pista e leitura de agentes. Juiz unificado de job vivo por obra [nao verificado]. O Stop valida `PAREI` do item em curso. Validação para qualquer obra da ordem [nao verificado]. `bin/hook_stop_fila1.py:599–626,725–765,1358–1423` `bin/codex_admissao.py:188–193` `bin/suite.sh:383–391` `bin/disjuntor.py:469–485`  
**Última atividade e lei.** O leitor mede `mtime` para julgar colheita. Juiz de última atividade por obra [nao verificado]. **0 perguntas de lei identificadas**. Semântica conjunta do limiar e do prazo de despertar, ano e desempate da recência [nao verificado]. `bin/rc_sem_colher.py:25–36,50–83` `app/docs/BACKLOG.md:1926–1927,2249` `aval_ordem.txt:1`  
**Próximo passo do Opus.** Depois da F1 v7, conferir sobreposição (diff exato [nao verificado]), provar os REDs com relógio injetado e estender o leitor existente e o selo do marcador. `app/docs/BACKLOG.md:1926,2249` `app/docs/RELATO.md:15212` `app/docs/PROMPTS.md:3225,3233` `app/docs/PLANO-PADROES.md:97–103`

- **Encaixe** `bin/rc_sem_colher.py:main/sem_colher`, `bin/tests/test_rc_sem_colher.sh:caso_espera` e `bin/tests/test_hook_nao_cobra_congelado.sh:_julga`. Reusar o juiz do hook e conferir `bin/disjuntor.py:ESPERA_UNICA` se mudar a forma da espera. `app/docs/BACKLOG.md:2249` `bin/tests/test_rc_sem_colher.sh:75–106` `bin/tests/test_hook_nao_cobra_congelado.sh:105–152` `bin/tests/test_agente_no_modelo.sh:291–297`
- **9 min / 600 s / 11 min** `bin/rc_sem_colher.py:main/sem_colher`. Sem `.rc` que acorde a espera, silêncio aos 9 min e aos 600 s, aviso com ID e próximo passo aos 11 min. Mutantes `>= 600`, limite removido, só `.rc` novo e aviso sem próximo passo. `app/docs/BACKLOG.md:2249` `bin/disjuntor.py:1965–1971`
- **Limiar durante a espera** `bin/disjuntor.py:ESPERA_UNICA`. Entrar aos 9 min e cruzar o limiar sem `.rc` novo. Mutante que consulta o leitor só na entrada. Prazo exato [nao verificado]. `app/docs/BACKLOG.md:2249` `bin/disjuntor.py:1964–1971`
- **`PAREI`** `bin/hook_stop_fila1.py:_topo/_parei_do_em_curso`. Aos 11 min, `PAREI` válido da própria obra suprime o aviso e o de outra obra deixa o aviso sair. Mutantes sem guarda e guarda global. `app/docs/BACKLOG.md:2249` `bin/hook_stop_fila1.py:462–484,1358–1423`
- **Job vivo** `bin/rc_sem_colher.py:main`. Aos 11 min, prova viva da própria obra suprime o aviso e produtor morto restaura o aviso. Mutantes sem prova e aceitação de PID declarado ou JSON residual. `app/docs/BACKLOG.md:2249` `bin/hook_stop_fila1.py:599–626` `bin/codex_admissao.py:188–193` `bin/codex_pergunta.sh:52–56`
- **Fora da ORDEM VIVA** `bin/hook_stop_fila1.py:_item_da_ordem_viva`. Obra andável com pasta em `logs/`, ociosa há 11 min e fora da ordem declarada fica sem aviso. Mutantes que aceitam qualquer pasta viva ou qualquer item andável. `app/docs/BACKLOG.md:2249` `bin/rc_sem_colher.py:55–60` `bin/hook_stop_fila1.py:205–249`
- **Marcador na mais nova** `bin/tests/test_hook_nao_cobra_congelado.sh:_julga`. Duas ordens abertas, marcador na mais nova, selo verde. Mutantes `>=` na comparação e reprovação sempre que há duas ordens. `aval_ordem.txt:1` `bin/tests/test_hook_nao_cobra_congelado.sh:105–132`
- **Marcador na mais velha** `bin/tests/test_hook_nao_cobra_congelado.sh:_julga`. Selo vermelho identifica o alvo e a ordem mais nova, inclusive quando a mais nova está fisicamente abaixo. Mutantes sem comparação de recência, só existência/estado e escolha pela posição da linha. `aval_ordem.txt:1` `app/docs/BACKLOG.md:1936,2666` `bin/tests/test_hook_nao_cobra_congelado.sh:105–132`

## 28/11 12:07 · sessao 0ce98f2df43e · CUSTO-DE-COORDENACAO-OBRA-PRONTA-ACORDA · censo · origem=Code

## 1. Mapa de `rc_sem_colher.py`

O leitor abre `app/docs/RELATO.md` e percorre `logs/<OBRA>`. Aceita `--raiz`, `--relato` e `--min-idade`. Considera pastas com ID em maiúsculas, dígitos e hífen e `mtime` de até 24 h. Dentro delas, percorre subpastas e arquivos na mesma janela, ignora arquivos que sejam links simbólicos e escolhe o `.rc` de maior `mtime`. Um arquivo de outro tipo mais de 5 s posterior ao `.rc`, ou uma marca correspondente no RELATO, tira o resultado da lista. O leitor atual só abre essas duas fontes. `bin/rc_sem_colher.py:21–36,50–83,86–107`

| Saída do leitor | Significado |
|---|---|
| `0` | Nenhum resultado, saída vazia. `bin/rc_sem_colher.py:104–107` |
| `3` | Um ou mais resultados. Imprime `RC SEM COLHER <OBRA> <caminho relativo> <HH:MM> rc=<valor>`, dos mais antigos aos mais novos. O `rc=<valor>` vem do conteúdo do arquivo, com `?` se não houver número. `bin/rc_sem_colher.py:23,39–43,78–83,104–107` |
| `1` | Argumento desconhecido ou exceção de leitura que chega ao processo. Erro na caminhada de diretórios é propagado. `bin/rc_sem_colher.py:13–14,46–47,62,90–111` |

- **`disjuntor.ESPERA_UNICA`** chama o leitor uma vez, antes do laço. Com `0`, espera até 300 s pelo primeiro `.rc` com `ctime` posterior à marca, consultando a cada 5 s. Com `3`, pula o laço e a lista já está na saída. Em ambos, faz o `find` final e sai `0`, inclusive se o `timeout` venceu. Com `1` ou outro código, pula o laço, ainda faz o `find` final e sai `1`. O `-cnewer` permite acordar com publicação por `rename` mesmo quando o `mtime` do temporário é anterior à espera. `bin/disjuntor.py:1964–1971`
- **`logs/espera_unica.sh`** aparece no RELATO como forma da espera pelo primeiro `.rc` novo ou 5 min. O script, a chamada ao leitor e o tratamento de cada código estão **[nao verificado]**. `app/docs/RELATO.md:13865`
- **Hooks**. O disjuntor usa `ESPERA_UNICA` como comando sugerido nas recusas de espera e polling. O `Stop` executa `hook_stop_fila1.py`, que decide pelo painel do RELATO e pelo BACKLOG. Na cópia, esses caminhos do hook não consomem `0`/`3`/`1` do leitor. `.claude/settings.json:3–9` `bin/disjuntor.py:2174–2209,2243` `bin/hook_stop_fila1.py:1489–1528`
- **Vercodex** é citado como leitor com `--min-idade 900`. O filtro atual exige idade estritamente maior que 900 s quando recebe essa opção. O tratamento de cada código no executável está **[nao verificado]**. Há registro de que uma tentativa anterior fez o vercodex contar outra classe como `RC SEM COLHER`. `bin/rc_sem_colher.py:2–4,76,96–97` `app/docs/RELATO.md:13907,14450`

## 2. Autoridades que já existem

| Pergunta | Juiz e alcance |
|---|---|
| **Quais obras estão na ORDEM VIVA?** | `_item_da_ordem_viva(texto)` responde **qual é o item em curso**. Na cópia, o marcador aponta `ORDEM-VIVA-2811-1200`, cuja linha declara em prosa as sequências por vaga e pista. `_itens_andaveis` e `disjuntor._pertence_fila1` respondem perguntas sobre o bloco OBRAS e a fila 1 do censo. A lista estruturada dos membros e dos próximos passos da ORDEM VIVA está **[nao verificado]**. A tentativa anterior de arquivo de IDs está registrada como estacionada. `app/docs/BACKLOG.md:8,1926,3047–3049` `bin/hook_stop_fila1.py:188–249,272–283` `bin/disjuntor.py:1797–1808,1846–1869` |
| **`pid=` e `ESPERANDO`** | `hook_stop_fila1._esperando_com_prova` lê o painel e valida os campos declarados. `_prova_pid` confere `/proc`, identidade do produtor, ausência de estado morto/zumbi e estabilidade do processo durante a leitura. `arquivo=` prova avanço de `mtime` entre avaliações e grava memória. A forma de `ESPERANDO` carrega produtor e prova, sem campo de obra. Para **censo**, `_prova_censo(obra)` já casa o wrapper vivo com `CODEX_OBRA` exato e papel `censo` ou `leitura`. `bin/hook_stop_fila1.py:599–626,639–647,725–765,783–833` |
| **Lease de vaga** | O `flock` do kernel em `logs/vagas/vaga.k` decide ocupação de vaga Codex. `codex_admissao._ocupada` faz a sondagem. `vaga.k.json` contém a projeção da chamada, incluindo a obra. PID no JSON e trilha de abriu/fechou são diagnósticos. O executor pode continuar segurando a lease depois da morte do titular. `bin/codex_admissao.py:188–193` `bin/codex_pergunta.sh:52–70,507–510,552–555` `bin/codex_vagas_estado.py:4–9` |
| **Pista e agentes** | Na pista, `suite.sh` lê `pista estado` e distingue `rodando`, `na fila` e `fim`; o estado traz ID de job, e o despacho registra ID e SHA. Para agentes de fundo, `disjuntor.agentes_sem_entrega` cruza lançamento e notificação de fim no transcript da sessão. Um juiz único de **job vivo por obra** cobrindo essas fontes e as leases está **[nao verificado]**. `bin/suite.sh:379–391,447–461` `bin/disjuntor.py:469–485` |
| **`PAREI` no topo** | `hook_stop_fila1._topo` recorta a faixa do censo e a janela do painel. `_parei_do_em_curso` verifica `PAREI:`, `!` e ID inteiro do item em curso, com as exclusões do Stop. `_relato_parou` ainda exige que nenhum outro item ande para devolver o turno. O alarme de sessão tem um leitor próprio de 40 linhas, para a parada da sessão. Um juiz que devolva `PAREI` legítimo **por qualquer obra da ORDEM VIVA** está **[nao verificado]**. `bin/hook_stop_fila1.py:408–416,462–484,1342–1424` `bin/alarme_sessao_ociosa.py:119–145` |
| **Última atividade** | `rc_sem_colher.sem_colher` já mede o maior `mtime` de `.rc` e o maior `mtime` de outro arquivo da obra para decidir **colheita**. `marcas()` casa `(obra, caminho relativo do rc, HH:MM do mtime do rc)` quando `colhido` ou `superado` é a primeira palavra depois de `- HH:MM `. Esse conjunto guarda a identidade do `.rc`, sem guardar o horário da ação de colheita. Um juiz de **última atividade da obra** que reúna esses sinais, jobs e `PAREI`, inclusive para obra sem `.rc`, está **[nao verificado]**. `bin/rc_sem_colher.py:25–36,50–83` |

O `PROXIMO PASSO` do handoff já aponta o item do marcador e remete ao BACKLOG e ao RELATO. A fonte de um próximo passo executável para **cada** obra da lista está **[nao verificado]**. `bin/handoff_sessao.sh:313–326` `bin/hook_stop_fila1.py:272–283,308–318`

## 3. Censo do marcador e das linhas

- **Escrita da ordem.** O marcador e o bloco OBRAS são mantidos à mão. A linha `ORDEM-VIVA-2811-1200` registra que a inclusão da ordem e a troca do marcador ocorreram no mesmo commit. O item B exige esse mesmo ato para todo aval de ordem. `gerar_backlog.py` relê e repõe a janela inteira ao regenerar o arquivo, com uma guarda para impedir que o marcador desapareça. `backlog_estado.py` regrava a célula `c[3]` do ID solicitado e consulta o hook antes e depois. `app/docs/BACKLOG.md:8,1926` `aval_ordem.txt:1` `bin/gerar_backlog.py:102–140` `bin/backlog_estado.py:29–68`
- **Leitura semântica.** O parser do marcador é `_item_da_ordem_viva`. O Stop e `_proximo_da_fila` o chamam. O handoff, `backlog_estado.py` e `interrompidas_estado.py` chegam ao mesmo juiz, diretamente ou por `_proximo_da_fila`. `gerar_estado.py` publica a projeção de interrompidas. O disjuntor lê linhas andáveis do bloco para o censo. `relato.sh` publica o BACKLOG inteiro, e `gerar_backlog_topo.py` seleciona IDs `O` seguidos de dígitos. Um leitor que enumere `ORDEM-VIVA-*` para julgar sua recência está **[nao verificado]**. `bin/hook_stop_fila1.py:188–224,308–318,1499–1512` `bin/handoff_sessao.sh:54–78` `bin/backlog_estado.py:56–63` `bin/interrompidas_estado.py:85–100` `bin/gerar_estado.py:231–243` `bin/disjuntor.py:1797–1808` `bin/relato.sh:135–147` `bin/gerar_backlog_topo.py:11–21`
- **Selos de host.** `test_hook_nao_cobra_congelado.sh` já abre o BACKLOG da árvore, chama o juiz e fica vermelho para marcador ausente, alvo inexistente ou parado. `test_hook_le_o_marcador.sh` usa BACKLOGs de teste e passa pelo Stop para provar que o marcador escolhe o item e que fila vazia libera. `test_hook_estado_e_a_coluna.sh`, `test_hook_parei_com_andavel.sh`, `test_hook_parei_trava_o_item_em_curso.sh`, `test_handoff_sessao.sh` e `test_interrompidas_estado.sh` também exercitam esse caminho. `bin/tests/test_hook_nao_cobra_congelado.sh:105–152` `bin/tests/test_hook_le_o_marcador.sh:38–103` `bin/tests/test_hook_estado_e_a_coluna.sh:41–65` `bin/tests/test_hook_parei_com_andavel.sh:27–49` `bin/tests/test_hook_parei_trava_o_item_em_curso.sh:22–42` `bin/tests/test_handoff_sessao.sh:122–145` `bin/tests/test_interrompidas_estado.sh:37–44,60–65`
- **Encaixe do item B.** A extensão cabe em `test_hook_nao_cobra_congelado.sh`, junto à checagem do BACKLOG da árvore. Usa o alvo devolvido pelo juiz atual e o reconhecedor de linha do hook. A comparação de recência é a asserção nova do selo. A régua já transforma falha desse selo em VERMELHO. `bin/tests/test_hook_nao_cobra_congelado.sh:78–83,105–152` `bin/hook_stop_fila1.py:116–129,205–224` `bin/regua.sh:134–145` `aval_ordem.txt:1`

## 4. Esmeril de «mais nova»

| ID na cópia | Data e hora disponíveis |
|---|---|
| `ORDEM-VIVA-2811-1200` | `DDMM-HHMM` no ID, `28/11 12:00` no texto do aval e `28/11` na última célula. `app/docs/BACKLOG.md:1926` |
| `ORDEM-VIVA-1511-NOITE` | Dia no ID, `20:1x` no texto e `15/11` na última célula. `app/docs/BACKLOG.md:1927` |
| `ORDEM-VIVA-0811` | Dia no ID, `08:5x` no texto e `08/11` na última célula. `app/docs/BACKLOG.md:1936` |
| `ORDEM-VIVA-1811-MANHA` | Dia no ID, `18/11 08:55` na descrição e última célula com caminho de log. A linha de 18/11 aparece fisicamente depois da de 08/11. `app/docs/BACKLOG.md:1936,2666` |

O hook percorre o bloco OBRAS e procura a primeira célula **igual ao alvo do marcador**. Não compara datas. A ordem física é usada por `_itens_andaveis` para devolver a lista da tabela. No caso dado, a comparação de 28/11 12:00 com 15/11 é inequívoca, e a cópia já aponta 28/11 12:00. Um contrato geral de ano, empate e precedência entre ID, coluna e prosa está **[nao verificado]**. `bin/hook_stop_fila1.py:205–249` `app/docs/BACKLOG.md:8,1926–1927` `backlog.txt:1926` `aval_ordem.txt:1`

## 5. Casos RED descritos

Para o primeiro grupo, usar relógio injetado e uma árvore sem `.rc` novo ou `RC SEM COLHER` capaz de acordar a espera pelo caminho antigo. O positivo precisa falhar na cópia pela ausência do novo aviso. Os negativos protegem as exclusões e precisam derrubar seus mutantes. `app/docs/PLANO-PADROES.md:97–103` `app/docs/PROMPTS.md:3225` `bin/tests/test_rc_sem_colher.sh:71–103,124–137`

| Caso | Asserção e mutante que deve cair |
|---|---|
| **Ociosa há 9 min × 11 min** | Mesma obra da ordem, sem job e sem `PAREI`. Aos 9 min, nenhum aviso de ociosidade. Aos 11 min, a espera retorna com o ID da obra **e** o próximo passo declarado. Mata o mutante que elimina o limite, o que só espera `.rc` novo e o que omite o próximo passo. Um controle em 600 s distingue `> 600` de `>= 600`. `backlog.txt:2249` `bin/disjuntor.py:1965–1971` |
| **Com `PAREI`** | Aos 11 min, `PAREI` válido no topo para a própria obra suprime o aviso. Uma trava de outra obra deixa passar o aviso da primeira. Mata a remoção da guarda e a guarda global por qualquer `PAREI`. `backlog.txt:2249` `bin/hook_stop_fila1.py:1358–1424` |
| **Com job vivo** | Aos 11 min, prova do job da própria obra suprime o aviso. Com a mesma declaração e produtor morto, o aviso volta. Mata a remoção da prova e a aceitação de PID escrito ou JSON residual como prova suficiente. `backlog.txt:2249` `bin/hook_stop_fila1.py:599–626` `bin/codex_pergunta.sh:52–56` |
| **Fora da ORDEM VIVA** | Obra andável no bloco OBRAS, com pasta em `logs/`, ociosa há 11 min e ausente da ordem declarada. Nenhum aviso. Mata a troca da pertinência à ORDEM VIVA por «qualquer pasta viva» ou «qualquer item andável». `backlog.txt:2249` `bin/rc_sem_colher.py:55–60` `bin/hook_stop_fila1.py:227–249` |
| **Marcador igual à linha mais nova** | Duas ordens de teste abertas, marcador na mais nova. Selo verde. Mata `>=` no lugar de `>` e o selo que fica vermelho sempre que há duas ordens. `aval_ordem.txt:1` `bin/tests/test_hook_nao_cobra_congelado.sh:105–132` |
| **Marcador mais velho** | Mesmas ordens abertas, marcador na mais velha. Selo VERMELHO pela recência, indicando o alvo e a linha posterior. Repetir com a mais nova fisicamente abaixo da anterior. Mata a remoção da comparação, o selo que só valida existência/estado e a escolha pela posição da linha. `aval_ordem.txt:1` `app/docs/BACKLOG.md:1936,2666` `bin/tests/test_hook_nao_cobra_congelado.sh:105–132` |

Há uma diferença entre o limiar e o prazo de despertar. Com a forma atual, uma obra com 9 min de ociosidade na entrada pode chegar a 14 min antes de o comando devolver o controle. O `find` dentro do laço só vê `.rc` novo. Assim, o par 9/11 cobre a decisão **na entrada**, e o requisito de acordar durante a mesma espera exige um caso com passagem do limiar e nenhum `.rc` novo. O BACKLOG registra tanto «mais de 10 min» quanto «acordando em até 10 min»; a semântica conjunta desse prazo está **[nao verificado]**. `bin/disjuntor.py:1964–1971` `backlog.txt:2249`

## 6. Arquivos da cura e cruzamento com F1 v7

| Arquivo | Toque previsto pelo encaixe |
|---|---|
| `bin/rc_sem_colher.py` e `bin/tests/test_rc_sem_colher.sh` | Leitor existente para o caso novo e selo da entrada normal da espera. O teste atual já exerce retorno imediato, timeout, publicação por `rename` e falha do leitor. `backlog.txt:2249` `bin/tests/test_rc_sem_colher.sh:75–106` |
| `bin/tests/test_hook_nao_cobra_congelado.sh` | Selo de recência junto ao veredito do marcador. `aval_ordem.txt:1` `bin/tests/test_hook_nao_cobra_congelado.sh:105–152` |
| `bin/hook_stop_fila1.py` | Dependência de leitura do marcador e do painel. O selo B pode usar as funções existentes. Uma edição para generalizar a prova de `PAREI` por obra cruzaria a F1. `bin/hook_stop_fila1.py:188–224,462–484,1358–1424` `app/docs/RELATO.md:15074,15160,15212` |
| `bin/disjuntor.py` | Dependência da entrada `ESPERA_UNICA`. O `3` já acorda quando o caso é detectado ao entrar. Se a cura alterar a forma da espera para tratar a passagem do prazo durante o bloqueio, há cruzamento com F1 e com o selo da forma publicada em `CLAUDE.md`. `bin/disjuntor.py:1964–1971` `bin/tests/test_agente_no_modelo.sh:292–297` `app/docs/RELATO.md:15074,15155,15212` |
| `bin/backlog_estado.py` | O selo B pode consultar o juiz sem editar esse escritor de célula. A v5 registrou o arquivo fora de `ARQUIVOS`; a entrega v7 registra seis arquivos sem enumerá-los. A presença dele no diff exato da v7 está **[nao verificado]**. `bin/backlog_estado.py:56–68` `app/docs/RELATO.md:15047,15212` |

A fila das 12:00 põe **CUSTO F1 v7 → OBRA-PRONTA-ACORDA** na vaga de opus construtor. O RELATO registra commit da v7 **na cópia**. O hash informado e o diff exato da v7 estão **[nao verificado]**. `backlog.txt:1926,2249` `app/docs/RELATO.md:15212`

**Padrão.** `Single Source of Truth` para as autoridades reaproveitadas, `Clock Injection` para o limiar e `Fitness Function` para o selo de recência. `app/docs/PLANO-PADROES.md:53–62,97–103`

## 28/11 12:06 · sessao 33722f5a8a31 · FERIADO-BOTAO-FAIXA · leitura · origem=Code

## 1. Mapa da faixa

A faixa roxa é o `<div>` que imprime `badge.label` sobre `background:{{ badge.cor }}`. O serviço entrega `label='Feriado'` e `cor='#7c3aed'`. A faixa tem estilo inline e sai do laço `dia.badges`. O ícone de bandeira e a amostra da legenda são elementos separados. [calendario.py:711](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/services/calendario.py:711) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142) [_calendario_grade.html:115](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:115) [_calendario_legenda.html:17](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_legenda.html:17)

**O mecanismo de corte na cópia é o limite de altura combinado com `overflow`.** Em `layout='mes'`, a célula tem `height:132px`, `min-height:132px` e `overflow:hidden`. Quando a ação é oferecida a quem pode editar, a célula vira uma coluna flex. A faixa fica dentro da região `flex:1;min-height:0;overflow-y:auto`, que recebe o espaço restante. Se a faixa cruza a borda dessa região, só a parte dentro dela aparece até a rolagem. O `z-index` ordena a pintura do botão e da região. O smoke registra o corte. Qual borda cortou a faixa no navegador do smoke e quantos pixels ficaram ocultos estão [nao verificado]. [backlog.txt:2047] [_calendario_grade.html:68](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:68) [_calendario_grade.html:95](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:95) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142)

| Casca | Caminho até a faixa |
|---|---|
| `base.html` | **A ficha de página inteira aparece aqui.** O bloco `content` recebe `detalhe.html`, que estende `base.html` literalmente. No `load`, a ficha busca `/colaboradores/<cid>/calendario/` por HTMX. A view devolve `calendario_partial.html`, que inclui `_calendario_grade.html` com `layout='mes'` e chega a `dia.badges`. [base.html:336](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/base.html:336) [detalhe.html:1](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/detalhe.html:1) [detalhe.html:215](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/detalhe.html:215) [views.py:2020](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/views.py:2020) [calendario_partial.html:67](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/calendario_partial.html:67) |
| `base_app.html` | **A ficha de página inteira é exclusiva de `base.html`.** Nas duas cascas, a ficha lateral pode carregar a mesma faixa. As duas incluem `_drawer_generico.html`, que busca `/colaboradores/<cid>/painel/`. A view renderiza `painel_colaborador.html`. A aba **Espelho** busca a mesma rota de calendário, que chega ao mesmo laço de badges. As rotas do painel e do calendário exigem `ver_cadastro`. [detalhe.html:1](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/detalhe.html:1) [base_app.html:154](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/base_app.html:154) [base.html:399](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/base.html:399) [_drawer_generico.html:91](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/core/_drawer_generico.html:91) [_drawer_generico.html:134](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/core/_drawer_generico.html:134) [painel_colaborador.html:182](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/painel_colaborador.html:182) [views.py:935](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/views.py:935) [views.py:1010](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/views.py:1010) [views.py:1986](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/views.py:1986) |

## 2. Censo e alcance

| Include direto da grade | Alcance da mudança |
|---|---|
| `calendario_partial.html` | Ficha inteira, aba Espelho do drawer e abertura direta do calendário pelo **Resolver dia** do Retrato da Folha. Mês, Quinzena, Semana e Competência passam pelo mesmo `layout='mes'` do template. Uma cura limitada ao ramo mensal com oferta alcança esses caminhos. [calendario_partial.html:49](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/calendario_partial.html:49) [calendario_partial.html:67](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/calendario_partial.html:67) [retrato_folha.py:1053](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/folha/retrato_folha.py:1053) [_retrato_o_que_fazer.html:74](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/folha/_retrato_o_que_fazer.html:74) |
| `_drawer_diagnostico.html` | **Escala × batidas** do Painel Situacional usa `layout='fita'`. Consome o estilo comum dos badges e fica fora do ramo mensal. [_drawer_diagnostico.html:33](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_drawer_diagnostico.html:33) [_calendario_grade.html:68](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:68) |
| `relatorios/furos.html` | **Auditoria de espelhos** em HTML passa apenas `dias` e `resumo`, com `only`. Consome o estilo comum dos badges e fica fora do ramo mensal. O PDF segue por um gerador próprio. [furos.html:42](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/relatorios/furos.html:42) [relatorios/views.py:1628](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/relatorios/views.py:1628) |

O CSS relevante está todo inline na grade. Uma edição geral no `<div>` do badge alcança também Extra 50%, Extra 100%, Atraso e Antecipada nos três includes. A classe `.icone-dia` do CSS compartilhado dimensiona a bandeira SVG. A legenda tem seu próprio estilo inline. [_calendario_grade.html:68](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:68) [_calendario_grade.html:99](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:99) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142) [calendario.py:714](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/services/calendario.py:714) [hasner-ponto.css:578](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/static/css/hasner-ponto.css:578) [_calendario_legenda.html:17](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_legenda.html:17)

## 3. Sobreposição com FERIADO-BOTAO-CORTADO

**Sim, a sobreposição é na mesma célula da mesma grade.** O anexo localiza o corte antigo no `overflow:hidden` da célula e no include do botão, então na linha 131. Na cópia, o include do botão está na linha 97, acima da região rolável da linha 99. A faixa continua dentro dessa região, nas linhas 142–144. O diff exato do pouso `627303c71` está [nao verificado]. [backlog.txt:2046] [backlog.txt:2047] [_calendario_grade.html:68](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:68) [_calendario_grade.html:97](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:97) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142)

## 4. Teste de tela e RED [nao verificado]

**Teste existente** — `SmokeCliqueFeriadoViraTrabalhoTest.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas`. A fixture cria o feriado de 20/11/2026 com quatro batidas. O cenário de página `base.html` abre a rota real da ficha e carrega o calendário. A página `base_app.html` do teste é um hospedeiro montado para o ensaio. O drawer usa o painel real nas duas cascas. As asserções cobram botão, popover e acesso à última batida por rolagem. O teste estrutural `test_feriado_botao_cortado.py` usa `badges=[]`. [test_smoke_clique_feriado_vira_trabalho.py:26](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:26) [test_smoke_clique_feriado_vira_trabalho.py:536](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:536) [test_smoke_clique_feriado_vira_trabalho.py:588](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:588) [test_smoke_clique_feriado_vira_trabalho.py:669](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:669) [test_smoke_clique_feriado_vira_trabalho.py:700](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:700) [test_smoke_clique_feriado_vira_trabalho.py:731](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:731) [test_feriado_botao_cortado.py:39](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/tests/test_feriado_botao_cortado.py:39)

**RED proposto** — Reusar essa fixture no Chromium, na ficha real e na aba Espelho das duas cascas. Selecionar o badge textual dentro da célula do dia 20. Manter a célula em 132 px e preparar conteúdo suficiente **antes** da posição atual do badge para cruzar o limite da região rolável. As quatro batidas, sozinhas, podem caber no cenário existente. Cobrar exatamente uma faixa, com os quatro lados visíveis já com `scrollTop=0`, e repetir a medição após rolar até a última batida. Cobrar também que o botão siga visível e clicável. **Mutante a matar:** devolver a faixa ao laço dentro da região rolável, preservando texto, cor, botão e batidas. A falha esperada é de recorte da faixa. RED e mutante executados [nao verificado]. [_calendario_grade.html:99](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:99) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142) [test_smoke_clique_feriado_vira_trabalho.py:329](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:329) [test_smoke_clique_feriado_vira_trabalho.py:469](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:469)

## 5. Arquivos para conferir colisão

- **Cura proposta** — `app/templates/colaboradores/partials/_calendario_grade.html` para colocar a faixa acima da região rolável no ramo mensal com oferta. Para reaproveitar o mesmo HTML nos dois locais, extrair o `<div>` atual para um novo `_calendario_badge.html` na mesma pasta. A seleção pode usar `dia.feriado` e o primeiro badge. Isso depende do contrato atual do produtor, que insere Feriado primeiro quando `dia.feriado` é verdadeiro. [_calendario_grade.html:95](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:95) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142) [calendario.py:711](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/services/calendario.py:711) [calendario.py:784](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/services/calendario.py:784)
- **Selo proposto** — `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py`, onde já ficam a sonda de geometria e os mutantes. A colisão com outras obras em voo está [nao verificado]. [test_smoke_clique_feriado_vira_trabalho.py:71](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:71) [test_smoke_clique_feriado_vira_trabalho.py:329](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:329)

**`padrao=fitness function / selo`** para a cura com o RED. O invariante é a faixa inteira à vista enquanto o conteúdo do dia continua rolável. A asserção é de geometria no navegador. [PLANO-PADROES.md:53](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/PLANO-PADROES.md:53) [GLOSSARIO.md:14](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/GLOSSARIO.md:14)

## 28/11 11:46 · sessao 35e63e4b2e65 · E4D-CONTRATO-ROTA-B · leitura · origem=Code

**B é um defeito da view HTMX. Os casos 11 e 44 chegam às asserções de negócio e precisam de curas próprias.** No 44, o traceback mostra a lista **obtida** `[]` e a **esperada** `[('resposta', T0, '08:00')]`, na primeira chamada do comando. Nos três mutantes de C, a falha é um `AssertionError` do controle inicial. Nos seis de B, o `VariableDoesNotExist` do teste interno vira `FAIL` na contraprova, que exige `resultado.errors == []`. Em ambos os grupos, o executor para antes de instalar o mutante. `app/chamados/views.py:394–408` · `app/chamados/tests/e4d_matriz_mutantes.py:570–612` · `aceite_v3.txt`, tracebacks de `/app/chamados/tests/test_e4d_matriz_reds.py:603,834`.

## B · Cartão no GET e no POST HTMX

`chamados:detalhe_local` resolve para `detalhe_chamado_local`. No GET, a view chama `montar_contexto_detalhe`, que lista as perguntas vivas da disputa aberta, fornece `pode_agir` e cria `folha_julga_pontualidade` como `SimpleLazyObject`. O template da página inclui o cartão com esse contexto. No POST `recusar_pergunta`, a mesma view passa `_resp_card` à ação. A recusa sem motivo retorna antes de qualquer escrita, e `_resp_card` renderiza o cartão diretamente com `perg` e `pode_agir`. A variável vem da montagem do GET. A guarda fica na linha 43 e o argumento que o Django tenta resolver fica na 44. `app/chamados/urls.py:14` · `app/chamados/views.py:377–408,506–519,550–552` · `app/chamados/services/acoes_chamado.py:337–358,409–423` · `app/chamados/services/acoes_disputa.py:94–102` · `app/chamados/services/validacao.py:934–937` · `app/config/settings/base.py:70–88` · `app/templates/chamados/detalhe_local.html:307–311` · `app/templates/chamados/partials/_pergunta_card.html:43–44`.

A v3 já percorre essas duas entradas com `self.client.get(url)` e `self.client.post(url, ..., HTTP_HX_REQUEST='true')`. Pela ordem do teste, a primeira HTMX tem validação vigente e pula a guarda da linha 43. Depois da `DEVOLUCAO`, o GET tem a chave e a segunda HTMX entra na guarda com o contexto incompleto. O traceback confirma o contexto do fragmento. O ordinal do request no log é [nao verificado]. `r_v3.md`, B[13] · `app/chamados/views.py:394–408` · `app/templates/chamados/partials/_pergunta_card.html:20–44`.

### B.1 · Contexto de produção

Em `app/chamados/views.py:404–405`, substitua as duas linhas que começam em `_r = _render_card(request, 'chamados/partials/_pergunta_card.html',` e terminam em `{'perg': _p, 'pode_agir': True})`. A âncora composta tem contagem 1. A consulta imediatamente acima já traz `disputa__colaborador`. `app/chamados/views.py:399–405` · `app/chamados/services/acoes_chamado.py:420–422`.

```python
            from django.utils.functional import SimpleLazyObject
            from chamados.juizes import folha_julga_pontualidade

            _r = _render_card(request, 'chamados/partials/_pergunta_card.html', {
                'perg': _p,
                'pode_agir': True,
                'folha_julga_pontualidade': SimpleLazyObject(
                    lambda: folha_julga_pontualidade(_p.disputa.colaborador),
                ),
            })
```

### B.2 · Assertos do mesmo teste HTTP [nao verificado]

Em `app/chamados/tests/test_e4d_matriz_reds.py`, no método `test_cura4_cartao_exibe_composta_no_get_e_no_htmx` inserido por B[13], substitua a linha `            self.assertContains(selo, f'✓ validada {validacao_vigente:%d/%m %H:%M}</span>')` pelo primeiro bloco. Substitua de `                    self.assertNotIn('opacity:.55;', abertura)` até imediatamente antes de `        p.refresh_from_db()` **desse método**, usando como âncora final composta `        p.refresh_from_db()\n        self.assertEqual((p.respondida_em, p.validada_em), (None, validacao_residual))`. Cada âncora tem contagem 1 na v3 aplicada. `r_v3.md`, B[13] · `app/chamados/tests/test_e4d_matriz_reds.py:1616` · `app/templates/chamados/partials/_pergunta_card.html:20–44,118–127`.

```python
            self.assertContains(
                selo, f'✓ validada {validacao_vigente:%d/%m %H:%M}</span>', msg_prefix='C1 hora do selo',
            )
```

```python
                    self.assertNotIn('opacity:.55;', abertura, 'C1 opacidade')
                    self.assertNotContains(retorno, '✓ validada ', msg_prefix='C1 selo residual')
                    self.assertContains(retorno, '⚠ aguardando validacao', msg_prefix='C1 resposta vigente')
                    self.assertContains(
                        retorno, f'— {resposta_em:%d/%m %H:%M}</span>', msg_prefix='C1 hora da resposta',
                    )
                    self.assertContains(
                        retorno, 'name="acao" value="recusar_pergunta"', msg_prefix='C1 acao',
                    )

```

Em `app/chamados/tests/e4d_matriz_mutantes.py`, substitua de `MC1Opacidade = r(` até imediatamente antes de `MC3ApiRele = recibo(`. Cada delimitação tem contagem 1 após B[16]. Os seis continuam ligados a `C1_TESTE`, que é o teste HTTP acima. `falha_em` usa a checagem já inserida por B[12]. `r_v3.md`, B[11–12,15–16] · `app/chamados/tests/e4d_matriz_mutantes.py:602–624`.

```python
MC1Opacidade = r('MC1Opacidade', 'C1.1', C1_TESTE, falha_em='C1 opacidade')
MC1Selo = r('MC1Selo', 'C1.2', C1_TESTE, falha_em='C1 selo residual')
MC1HoraDoSelo = r('MC1HoraDoSelo', 'C1.3', C1_TESTE, falha_em='C1 hora do selo')
MC1HoraDaResposta = r('MC1HoraDaResposta', 'C1.4', C1_TESTE, falha_em='C1 hora da resposta')
MC1Acao = r('MC1Acao', 'C1.5', C1_TESTE, falha_em='C1 acao')
MX3RespondidaCrua = r('MX3RespondidaCrua', 'X3', C1_TESTE, falha_em='C1 resposta vigente')
```

| Mutante | Mordida esperada com o fragmento renderizável, por leitura do código |
|---|---|
| `MC1Opacidade` | A validação residual da coluna esmaece o cartão após a devolução. `app/templates/chamados/partials/_pergunta_card.html:21` |
| `MC1Selo` | A validação residual da coluna exibe um selo após a devolução. `app/templates/chamados/partials/_pergunta_card.html:24–26` |
| `MC1HoraDoSelo` | A primeira HTMX exibe a hora residual no selo. `app/templates/chamados/partials/_pergunta_card.html:24` |
| `MC1HoraDaResposta` | A hora fica vazia porque a coluna `respondida_em` está vazia. `app/templates/chamados/partials/_pergunta_card.html:30–33` |
| `MC1Acao` | A coluna `validada_em` residual esconde o formulário de recusa. `app/templates/chamados/partials/_pergunta_card.html:43,118–127` |
| `MX3RespondidaCrua` | A coluna `respondida_em` vazia muda o estado exibido para `aguardando colaborador`. A checagem de estado vem antes das de hora e ação. `app/templates/chamados/partials/_pergunta_card.html:20,24–33` · `r_v3.md`, B[13,15] |

**Escritores e leitores.** `cartao` grava o texto e os carimbos residuais na projeção. A fixture grava `RESPOSTA`, as duas `VALIDACAO` e `DEVOLUCAO` por `registrar_na_fixture` e `DecisaoPergunta.registrar`. Em produção, `responder_pergunta_disputa_core` grava texto, instante e ato de resposta, e `validacao` grava e devolve a validação. `DecisaoPergunta.leitura` ordena os atos, tira a validação na `DEVOLUCAO` e conserva a resposta. `composta` entrega os instantes dessa leitura e o texto da projeção. O template lê os instantes pelo filtro `resposta_composta`, lê o booleano por `PerguntaDisputa.respondida` e o texto exibido por `resposta_display`. O GET lê `perguntas_disputa` pelo juiz de vitalidade. A view HTMX fornece `perg`, `pode_agir` e `HX-Trigger`, com a mensagem da porta de recusa. `app/chamados/tests/test_e4d_matriz_reds.py:166–175,215–229` · `app/chamados/tests/_ato_da_fixture.py:36–44` · `app/chamados/services/disputa_emissao.py:1531–1546,1564–1570` · `app/chamados/services/validacao.py:164–178,304–316,934–937` · `app/chamados/models.py:3958–3965,3985–3991,4450–4456,4486–4515,4590–4623` · `app/chamados/templatetags/chamados_tags.py:144–182` · `app/chamados/services/acoes_chamado.py:350–358,409–423` · `app/chamados/juizes.py:1499–1533,1644–1653,2081–2102` · `app/chamados/views.py:399–408,418–421`.

## C11 · Pedido de reabertura em pergunta vazia

**A regra da v3 está aberta em `f300431af`.** A assinatura carece de `pedido_id`. A porta só monta o ato quando encontra texto, resposta, validação, materialização ou validação vigente só na sequência. A fixture deixa as duas perguntas realmente vazias, e o primeiro `registrar([])` volta cedo. Mesmo que se retire esse filtro, a porta do lote hoje projeta RVM e acorda células, e `registrar` também acorda células. B[5]/B[6] tratam da recusa na passagem da ausência. A cura do 11 entra na reabertura e na persistência da decisão. `app/chamados/tests/test_e4d_matriz_reds.py:166–179,570–606` · `app/chamados/services/disputa_emissao.py:1631,1674–1719` · `app/chamados/models.py:3594–3618,3657–3660,4590–4623` · `apv3.py`, ramo `--cura`.

A proposta usa uma chave opaca opcional de até 128 caracteres, única por pergunta, no ato imutável. A porta consulta essa chave sob a trava existente. O contrato novo começa na chamada do serviço com pedido identificado. Os chamadores atuais de recusa e de reabertura histórica usam a assinatura sem chave, que também é exercitada pelo controle de pendente vazia. A emissão e o transporte da chave pelas telas atuais são [nao verificado]. `app/chamados/services/disputa_emissao.py:1674–1685` · `app/chamados/models.py:4405–4411` · `app/chamados/services/validacao.py:997–1000` · `app/chamados/services/acoes_disputa.py:120–141` · `app/chamados/views.py:1416–1441` · `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:816–820`.

### C11.1 · Campo, restrições e emissão compartilhada com C44

Em `app/chamados/models.py:4373`, insira logo após `    resposta_mantem_validacao = models.BooleanField(default=False, db_default=False)` (1).

```python
    pedido_id = models.CharField(max_length=128, null=True, blank=True)
```

Em `app/chamados/models.py:4395`, insira logo após `                name='decisao_mantem_validacao_so_na_resposta'),` (1).

```python
            models.CheckConstraint(
                condition=models.Q(pedido_id__isnull=True) | models.Q(ato='reabertura'),
                name='decisao_pedido_so_na_reabertura'),
            models.UniqueConstraint(
                fields=('pergunta', 'pedido_id'),
                condition=models.Q(pedido_id__isnull=False),
                name='decisao_reabertura_pedido_unico'),
```

Em `app/chamados/models.py:4590`, substitua `    def registrar(cls, linhas):` (1) pela assinatura abaixo. Em `:4622–4623`, substitua `            PerguntaDisputa.acordar_celulas(eventos)\n            return criadas` (1) pelo segundo bloco. A trava, o `bulk_create` e a invalidação do cache continuam na mesma porta. `app/chamados/models.py:4590–4623`.

```python
    def registrar(cls, linhas, *, acordar=True):
```

```python
            if acordar:
                PerguntaDisputa.acordar_celulas(eventos)
            return criadas
```

Texto integral da migração nova `app/chamados/migrations/0057_decisao_pedido_id.py`. O ponto de encadeamento na cópia é a `0056_decisao_resposta_mantem_validacao_historico`. O campo nasce anulável para as linhas existentes. `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:17–27` · `app/chamados/models.py:4344–4396`.

```python
from django.db import migrations, models


class Migration(migrations.Migration):
    dependencies = [
        ('chamados', '0056_decisao_resposta_mantem_validacao_historico'),
    ]

    operations = [
        migrations.AddField(
            model_name='decisaopergunta',
            name='pedido_id',
            field=models.CharField(max_length=128, null=True, blank=True),
        ),
        migrations.AddConstraint(
            model_name='decisaopergunta',
            constraint=models.CheckConstraint(
                condition=models.Q(pedido_id__isnull=True) | models.Q(ato='reabertura'),
                name='decisao_pedido_so_na_reabertura',
            ),
        ),
        migrations.AddConstraint(
            model_name='decisaopergunta',
            constraint=models.UniqueConstraint(
                fields=('pergunta', 'pedido_id'),
                condition=models.Q(pedido_id__isnull=False),
                name='decisao_reabertura_pedido_unico',
            ),
        ),
    ]
```

### C11.2 · Porta de reabertura

Em `app/chamados/services/disputa_emissao.py:1631`, substitua `def reabrir_para_correcao(qs, *, por=None, motivo='', ato='reabertura'):` (1). Em `:1665–1669`, substitua de `    # REABRIR-NAO-DESTROI (aval 21/10 RAIA D):` até antes de `    from django.db import transaction\n    from django.utils import timezone\n    from chamados.models import DecisaoPergunta, PerguntaDisputa\n    from chamados.services.trava_do_colaborador import exigir_colaboradores_travados\n    if ato not in DecisaoPergunta.ATOS_COM_FOTO:` (1 de cada) pelo segundo bloco. Em `:1675`, insira o terceiro bloco logo após `        raise ValueError('reabrir_para_correcao: ato %r nao leva foto (%s)' % (ato, DecisaoPergunta.ATOS_COM_FOTO))` (1). `app/chamados/services/disputa_emissao.py:1631,1664–1682`.

```python
def reabrir_para_correcao(qs, *, por=None, motivo='', ato='reabertura', pedido_id=None):
```

```python
    # A trilha guarda a foto anterior no mesmo atomic. Sem pedido identificado, só
    # o que há para compensar deixa linha. Um pedido novo identificado também
    # registra as perguntas vazias. A recusa tem seu próprio ato e a mesma foto.
```

```python
    com_pedido = pedido_id is not None
    if com_pedido and (ato != DecisaoPergunta.REABERTURA or not isinstance(pedido_id, str)
                       or not pedido_id.strip() or len(pedido_id) > 128):
        raise ValueError('pedido_id deve identificar uma reabertura (1 a 128 caracteres)')
```

No mesmo arquivo, substitua de `        # LASTRO-PARTE-3 (O-OUTBOX): o update do lote nao passa pelo post_save. Capture as linhas do evento sob a trava,` até antes de `def responder_automaticamente(p, texto, instante):` (1 de cada). O bloco aproveita `_agora` capturado sob a trava e mantém uma chamada a `DecisaoPergunta.registrar`. `app/chamados/services/disputa_emissao.py:1676–1723` · `app/chamados/models.py:3621–3660` · `app/chamados/tests/test_reabrir_nao_destroi.py:118–130`.

```python
        if com_pedido:
            ja_feitas = DecisaoPergunta.objects.filter(
                ato=DecisaoPergunta.REABERTURA, pedido_id=pedido_id,
            ).values('pergunta_id')
            qs = PerguntaDisputa.objects.filter(pk__in=qs.values('pk')).exclude(pk__in=ja_feitas)

        vazias = [
            pk for pk, texto, resposta, validada, materializada, via in qs.values_list(
                'pk', 'resposta_colab', 'respondida_em', 'validada_em', 'materializada_em', 'via_resolucao',
            )
            if via not in PerguntaDisputa.VIAS_FORA_DO_FATO
            and not ((texto or '').strip() or resposta is not None or validada is not None or materializada is not None)
        ]
        por_ato = {
            q.pk: DecisaoPergunta.composta(q)
            for q in (PerguntaDisputa.objects.filter(pk__in=vazias).prefetch_related('decisoes') if vazias else ())
        }
        origem = (
            'pk', 'resposta_colab', 'respondida_em', 'validada_em', 'validada_por_id', 'materializada_em', 'via_resolucao',
        )
        if com_pedido:
            origem += ('materializada_por_id',)
        atos = []
        projetar = set()
        for pk, texto, resposta, validada, autor, materializada, via, *autor_materializacao in qs.values_list(*origem):
            if via in PerguntaDisputa.VIAS_FORA_DO_FATO:
                continue
            lida = por_ato.get(pk)
            validada_no_ato = lida.validada_em if lida is not None else None
            tem_foto = any(((texto or '').strip(), resposta, validada, materializada, validada_no_ato))
            if not com_pedido and not tem_foto:
                continue
            linha = dict(
                pergunta_id=pk, ato=ato, ato_texto=texto or '', respondida_original_em=resposta,
                validada_original_em=validada if validada is not None else validada_no_ato,
                validada_original_por_id=autor, ato_em=_agora, ato_por=por, ato_motivo=(motivo or '')[:240],
            )
            if com_pedido:
                linha['pedido_id'] = pedido_id
                if (tem_foto or texto or via or autor is not None or autor_materializacao[0] is not None
                        or (lida is not None and lida.respondida_em is not None)):
                    projetar.add(pk)
            atos.append(linha)

        alvo_eventos = qs.filter(pk__in=projetar) if com_pedido else qs
        linhas_eventos = PerguntaDisputa.linhas_do_evento(alvo_eventos)
        DecisaoPergunta.registrar(atos, **({'acordar': False} if com_pedido else {}))
        if com_pedido and not projetar:
            return 0
        opcoes = {'escrever_so': projetar} if com_pedido else {}
        n = PerguntaDisputa.descarimbar_em_lote(qs, **opcoes, **campos)
        PerguntaDisputa.acordar_celulas(linhas_eventos)
        return n


```

### C11.3 · Contraprovas

O teste B[17] já distingue o primeiro pedido, o retry e outro pedido em `T2`. `M11DescartaReaberturaVazia` usa o par B[18], que exclui as vazias antes de `ATOS_COM_FOTO`. `M11RetryGanhaNovoPedido` usa o wrapper existente, que troca a chave na segunda chamada. Após a cura, o primeiro deve falhar na falta do ato inicial e o segundo na presença de uma linha adicional no retry. No aceite, ambos param na falta do primeiro ato do **controle inicial**. `r_v3.md`, B[17–18] · `app/chamados/services/disputa_emissao.py:1674` · `app/chamados/tests/e4d_matriz_mutantes.py:292–302,598–610,650–654` · `aceite_v3.txt`, falhas dos dois M11.

Em `app/chamados/tests/e4d_matriz_mutantes.py:650–654`, substitua de `M11DescartaReaberturaVazia = h(` até antes de `M12AutorDaFotoSetNull = h(` (1 de cada).

```python
M11DescartaReaberturaVazia = h(
    'M11DescartaReaberturaVazia', '11_descarta_reabertura_vazia',
    'test_caso_11_reabertura_vazia_grava_ato_sem_projetar',
    falha_em='Regra 11: o primeiro pedido deixa ato na pergunta vazia',
)
M11RetryGanhaNovoPedido = h(
    'M11RetryGanhaNovoPedido', '11_retry_ganha_novo_pedido',
    'test_caso_11_reabertura_vazia_grava_ato_sem_projetar',
    falha_em='Regra 11: o retry preserva a trilha',
)
```

**Escritores e leitores.** A porta de reabertura monta `ato`, `ato_em`, `ato_por`, `ato_motivo`, `ato_texto` e os três campos `*_original_*`; `registrar` aplica o relógio de compensação e insere a linha. A consulta `values_list(*campos_ato)` de B[17] lê esses valores, e `foto_dos_atos` lê a linha inteira, inclusive o PK. `cartao` monta a projeção de entrada. `descarimbar_em_lote` é quem escreve os sete campos RVM na saída, e `foto` os lê. `acordar_celulas` delega a criação de `EventoCelula` ao escritor do outbox, e `eventos` conta as linhas do colaborador. `app/chamados/services/disputa_emissao.py:1664,1682,1689–1719` · `app/chamados/models.py:3594–3618,3657–3660,4344–4353,4552–4586,4590–4623` · `app/chamados/tests/test_e4d_matriz_reds.py:51–54,166–175,208–229` · `app/ponto/portas/celula.py:981–991` · `r_v3.md`, B[17].

## C44 · Apply do estoque e identidade no retry

**A correspondência já existe. A escrita pedida pelo teste está aberta nessa entrada.** `linhas_a_criar` usa a correspondência por pergunta, ato e instante para medir as respostas faltantes. O `--aplicar` da cópia passa colisões e silêncios à porta, conforme o contrato explícito do comando. Por isso a resposta falta já no primeiro apply. A fixture usa dia dentro do horizonte. Seu `marco_do_estoque` tenta alterar o registro de uma migração ausente na cópia, e o comando examinado seleciona por dia do fato e projeção. O backfill de fio morto já registra respostas no recorte que seu juiz seleciona, por outra entrada. `app/chamados/management/commands/backfill_decisoes_pergunta.py:1–13,44–92,193–234,328–377,486–516,628–631` · `app/chamados/models.py:2714,4415–4448` · `app/chamados/tests/cortes_f2_f9_apoio.py:31–43` · `app/chamados/tests/test_e4d_matriz_re

[truncado em 20000 de 30480 caracteres]

## 28/11 11:42 · sessao dad8aafbdcba · - · - · origem=Ronald (tmux)

O caso 11 está vermelho por comportamento da porta. As perguntas vazias da fixture têm PKs selecionados, mas o predicado do escritor descarta ambas, e a assinatura atual não recebe identidade do pedido. O primeiro asserto da v3 é a falha observada, também nos controles iniciais de `M11DescartaReaberturaVazia` e `M11RetryGanhaNovoPedido`. A mutação só é instalada depois desse controle. `app/chamados/tests/test_e4d_matriz_reds.py:166–176,208–213,570–606` · `app/chamados/services/disputa_emissao.py:1631,1689–1719` · `app/chamados/tests/e4d_matriz_mutantes.py:570–612,650–653` · `aceite_v3.txt`, traceback do teste em `/app/chamados/tests/test_e4d_matriz_reds.py:603`.

A cura pertence a `reabrir_para_correcao` e à persistência de `DecisaoPergunta`. Enviei o bloco à raiz. A identidade opcional fica no ato, com unicidade por pergunta e chave. Sob a trava existente, a porta exclui pedidos já registrados, grava uma linha por pergunta elegível para um pedido novo e só projeta ou acorda células quando há estado para alterar. A supressão seletiva do outbox precisa alcançar `DecisaoPergunta.registrar`, que hoje emite mesmo quando o serviço evita a projeção. As chamadas sem chave conservam a semântica legada, coberta por testes vizinhos. `app/chamados/services/disputa_emissao.py:1674–1719` · `app/chamados/models.py:3621–3660,4344–4396,4590–4623` · `app/chamados/tests/test_e3_porta_A_reabrir.py:44–64` · `app/chamados/tests/test_e4_expand.py:205–227`.

Os marcadores de mordida são `Regra 11: o primeiro pedido deixa ato na pergunta vazia` para o descarte e `Regra 11: o retry preserva a trilha` para a troca de identidade. O mutante de descarte da v3 conserva âncora no teste de `ATOS_COM_FOTO`. O de retry já repassa a chave alterada na segunda chamada e preserva a assinatura inspecionada via `wraps`. `app/chamados/services/disputa_emissao.py:1674` · `app/chamados/tests/e4d_matriz_mutantes.py:292–302,650–653` · `r_v3.md`, bloco do caso 11 do anexo.

As âncoras entregues à raiz têm contagem 1 por leitura em memória. Execução ORM e migração [nao verificado]. `app/chamados/models.py:4373,4395,4590,4622` · `app/chamados/services/disputa_emissao.py:1631,1674–1675,1683,1723` · `app/chamados/tests/e4d_matriz_mutantes.py:650–654`.

## 28/11 11:42 · sessao 6746c6da5887 · - · - · origem=Ronald (tmux)

**Caso 44.** O log mostra `[] != [(RESPOSTA, T0, '08:00')]` na primeira importação. `M44DuplicaRetry` para no controle inicial, por uma asserção da regra. O wrapper duplicador ainda não entrou. `aceite_v3.txt` (anexo) · `app/chamados/tests/test_e4d_matriz_reds.py:809–830` · `app/chamados/tests/e4d_matriz_mutantes.py:248–266,585–605`

A fixture cai no horizonte, com `DIA=14/10/2026`, corte em 21/09 e competência iniciada no dia 21. A lacuna está no comando. `linhas_a_criar` mede as respostas faltantes e o `--aplicar` percorre colisões e silêncios. O `backfill_fio_morto_e4b` já preserva a identidade no seu recorte de fio morto, como mostra o teste com duas chamadas, e opera só na competência corrente. A importação geral de respostas do caso 44 exige cura própria. `app/chamados/tests/cortes_f2_f9_apoio.py:31–43` · `app/chamados/models.py:2714` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:1–13,44–92,328–377,628–631` · `app/chamados/management/commands/backfill_fio_morto_e4b.py:161–164,297–311` · `app/chamados/tests/test_e4_passivo.py:57–72`

Enviei blocos para um modo público `--aplicar-respostas-da-projecao`, com escrita dentro de `aplicar` e de seu `atomic` existente. A modalidade limita a seleção ao horizonte datado, confere exportação e relê a correspondência sob a trava `C → D → P`. O payload vem de `linhas_da_projecao_sem_linha`; `registrar` insere com o instante original e com a emissão de eventos desativada expressamente para esse lote. O teste mantém o primeiro ato, compara todos os campos, inclusive PK, no retry e prova a entrada posterior de outra pergunta. A modalidade anterior conserva seus chamadores. Âncoras conferidas em memória com contagem 1. Execução Django/PostgreSQL da proposta [nao verificado]. `app/chamados/management/commands/backfill_decisoes_pergunta.py:193,240–264,455–463,486–489` · `app/chamados/tests/test_e1_v7_catracas.py:253–292,512–516` · `app/chamados/services/fio.py:1471–1481,1716–1747` · `app/chamados/models.py:2940–3006,4415–4448,4590–4623` · `app/chamados/tests/test_e4d_matriz_reds.py:228–229,809–830`

A fixture escreve o par legado com `cartao`. Em produção, a porta de resposta grava o texto e o carimbo em `disputa_emissao`; a validação grava autor e instante em `validacao`; o modelo grava o trio de materialização. O backfill lê o par legado, `linhas_da_projecao_sem_linha` produz `RESPOSTA`, instante, texto, motivo e marca de conservação da validação, e `registrar` faz o `INSERT`. O teste lê o par e os demais RVM por `foto`, o ato por `values_list` e sua identidade integral por `foto_dos_atos`. `eventos()` conta as linhas de `EventoCelula`, cuja escrita passa por `emitir_evento_celula`. `app/chamados/tests/test_e4d_matriz_reds.py:52–53,166–176,208–229,809–830` · `app/chamados/services/disputa_emissao.py:1531–1569` · `app/chamados/services/validacao.py:164–179` · `app/chamados/models.py:3428–3432,4344–4373,4438–4442,4606–4623` · `app/ponto/portas/celula.py:981–991`

## 28/11 11:41 · sessao 909eef8a3d48 · - · - · origem=Ronald (tmux)

**B é uma falha no contexto de produção do POST HTMX.** O teste v3 já usa `self.client.get` e `self.client.post` com `HTTP_HX_REQUEST='true'` na URL `chamados:detalhe_local`. O GET chama `montar_contexto_detalhe`, que filtra as perguntas vivas, fornece `pode_agir` e passa `folha_julga_pontualidade` como `SimpleLazyObject`. O template da página inclui o cartão dentro do laço de perguntas. No POST, `_resp_card` carrega a pergunta e renderiza o mesmo cartão com apenas `perg` e `pode_agir`. A guarda do cartão está na linha 43 e o argumento obrigatório do filtro na 44. `app/chamados/urls.py:14` · `app/chamados/views.py:394–408,506–519,550–552` · `app/chamados/services/acoes_chamado.py:345–358,409–423` · `app/templates/chamados/detalhe_local.html:307–311` · `app/templates/chamados/partials/_pergunta_card.html:43–44` · `r_v3.md`, B[13].

Em `app/chamados/views.py:404–405`, substitua as duas linhas de `_render_card` que contêm `{'perg': _p, 'pode_agir': True}`. A âncora literal e os seis alvos do template tiveram contagem 1 na cópia. `app/chamados/views.py:399–405` · `app/templates/chamados/partials/_pergunta_card.html:20–44`.

```python
            from django.utils.functional import SimpleLazyObject
            from chamados.juizes import folha_julga_pontualidade

            _r = _render_card(request, 'chamados/partials/_pergunta_card.html', {
                'perg': _p,
                'pode_agir': True,
                'folha_julga_pontualidade': SimpleLazyObject(
                    lambda: folha_julga_pontualidade(_p.disputa.colaborador),
                ),
            })
```

A falta do argumento aparece na HTMX posterior à `DEVOLUCAO` pela ordem do teste e pelas guardas do cartão. O teste primeiro afirma que a recusa com motivo reconhece `ja_validada`, depois confere a hora do selo da primeira HTMX. O GET posterior recebe a variável e o POST posterior entra no trecho de ações da pergunta respondida. O trecho do log confirma o contexto do fragmento, enquanto o ordinal do request no trace é [nao verificado]. A recusa sem motivo retorna antes de escrever e sua mensagem chega ao `HX-Trigger` por `acoes_disputa`. `r_v3.md`, B[13] · `app/templates/chamados/partials/_pergunta_card.html:20–44` · `app/chamados/services/validacao.py:934–954` · `app/chamados/services/acoes_disputa.py:94–102` · `app/chamados/views.py:404–408`.

Os seis mutantes usam o mesmo teste no registro da v3. O executor roda o controle inicial e falha em `assertFalse(resultado.errors)` antes de instalar cada mutante. Os registros podem exigir `falha_em` e as respectivas asserções usar `msg_prefix` ou `msg`, conforme o método. `r_v3.md`, B[11–12,16] · `app/chamados/tests/e4d_matriz_mutantes.py:585–610,624` · `aceite_v3.txt`, B.

| Mutante | Asserção a marcar após a cura |
|---|---|
| `MC1Opacidade` | `C1 opacidade` na ausência de `opacity:.55;` após devolver. `app/templates/chamados/partials/_pergunta_card.html:21` |
| `MC1Selo` | `C1 selo residual` na ausência de `✓ validada ` após devolver. `app/templates/chamados/partials/_pergunta_card.html:24–26` |
| `MC1HoraDoSelo` | `C1 hora do selo` em `validacao_vigente` antes de devolver. `app/templates/chamados/partials/_pergunta_card.html:24` |
| `MC1HoraDaResposta` | `C1 hora da resposta` em `resposta_em` após devolver. `app/templates/chamados/partials/_pergunta_card.html:30–33` |
| `MC1Acao` | `C1 acao` na presença do formulário de recusa após devolver. `app/templates/chamados/partials/_pergunta_card.html:43,118–127` |
| `MX3RespondidaCrua` | `C1 resposta vigente` na presença de `⚠ aguardando validacao` após devolver. Verificar antes da hora e do formulário. `app/templates/chamados/partials/_pergunta_card.html:20,25–26,30` |

A fixture grava as colunas cruas via `cartao` e os atos via a porta real da sequência. Na produção, a resposta e sua projeção vêm de `responder_pergunta_disputa_core`, a validação de `_carimbar_validacao` e a devolução da porta em `validacao.py`. `DP.leitura` lê a sequência e desfaz só a validação ao encontrar `DEVOLUCAO`. `composta` fornece os dois instantes pelos atos e o texto pela projeção. O cartão lê a composta pelo filtro, `respondida` pela propriedade que chama o juiz e o texto de apresentação por `resposta_display`, que consulta o ato. `app/chamados/tests/test_e4d_matriz_reds.py:166–175` · `app/chamados/tests/_ato_da_fixture.py:36–44` · `app/chamados/models.py:3958–3965,3985–3991,4450–4456,4486–4515,4590–4609` · `app/chamados/services/disputa_emissao.py:1531–1546,1564–1570` · `app/chamados/services/validacao.py:164–178,304–316` · `app/chamados/templatetags/chamados_tags.py:144–147` · `app/chamados/juizes.py:1644–1653`.

Execução Django do bloco proposto [nao verificado].

## 28/11 11:37 · sessao 36528b0b772d · C7-TELA-ADMIN-ENCERRA · construtor · origem=Code

Implementei **Disputas** na ficha completa e o encerramento da disputa inteira pela porta existente. O POST renderiza o resultado na própria tela e atualiza a lista de disputas abertas. O reenvio de uma disputa fechada passa pela mesma porta para que ela decida a idempotência. [app/templates/chamados/fio_colaborador.html:11](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/templates/chamados/fio_colaborador.html:11) [app/chamados/views_disputa_admin.py:19](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/chamados/views_disputa_admin.py:19) [app/chamados/views_disputa_admin.py:43](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/chamados/views_disputa_admin.py:43)

### Diff por arquivo

- `app/chamados/urls.py` registra `disputas_admin_colaborador` e `encerrar_disputa_admin`. [app/chamados/urls.py:23](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/chamados/urls.py:23)
- `app/templates/chamados/fio_colaborador.html` mostra o link **Disputas** para `is_staff` ou grupo `admin_ti`, pelo predicado já entregue à ficha. [app/templates/chamados/fio_colaborador.html:11](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/templates/chamados/fio_colaborador.html:11) [app/chamados/views.py:708](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/chamados/views.py:708)
- `app/chamados/views_disputa_admin.py` exige login, restringe os métodos e devolve `403` sem chamar a porta quando falta permissão. O POST devolve `404` para PK ausente, malformado, inexistente ou de outro colaborador. Entrega `motivo` como recebido e apresenta sucesso, nenhuma alteração ou o texto da recusa. [app/chamados/views_disputa_admin.py:13](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/chamados/views_disputa_admin.py:13) [app/chamados/views_disputa_admin.py:28](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/chamados/views_disputa_admin.py:28) [app/chamados/views_disputa_admin.py:36](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/chamados/views_disputa_admin.py:36)
- `app/templates/chamados/disputa_admin.html` usa `base_tpl` e mostra todas as perguntas de cada disputa aberta, com marco, dia, texto e estado publicado, antes do formulário. Cada formulário tem CSRF, `disputa_pk` e motivo obrigatório no navegador. [app/templates/chamados/disputa_admin.html:1](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/templates/chamados/disputa_admin.html:1) [app/templates/chamados/disputa_admin.html:13](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/templates/chamados/disputa_admin.html:13) [app/templates/chamados/disputa_admin.html:29](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/templates/chamados/disputa_admin.html:29)

**except/fallback novos no diff: 1**. O `except ValueError as e` envolve somente `disputa.admin_encerra(request.user, motivo)` para entregar ao admin a mensagem original da porta. Zero fallbacks. [app/chamados/views_disputa_admin.py:50](/home/ronald/wt-codex-c7-tela-admin-encerra-113127/app/chamados/views_disputa_admin.py:50)

### Leitores e escritores dos campos

| Campo | Leitura nesta obra | Origem ou escrita existente |
|---|---|---|
| `request.user`, `is_staff`, `groups` | Autorização e ator da porta em `app/chamados/views_disputa_admin.py:14,52`. | A autenticação entrega o usuário em `app/config/settings/base.py:60`. Escritas de permissão em `app/core/credenciais.py:139` e `app/core/views_usuarios.py:295,299`. |
| `colaborador_id`, `Colaborador.pk`, `DisputaSupervisao.colaborador_id` | Captura da rota em `app/chamados/urls.py:23,24`. Busca do colaborador e escopo da disputa em `app/chamados/views_disputa_admin.py:32,47`. | Criação do colaborador em `app/colaboradores/services/colaborador.py:204`. A emissão associa o colaborador à disputa em `app/chamados/services/disputa_emissao.py:601,1137,1372`. |
| `disputa_pk`, `motivo` | `app/chamados/views_disputa_admin.py:40,49`. | O formulário envia o PK da disputa e o texto digitado em `app/templates/chamados/disputa_admin.html:31,33`. A porta normaliza o motivo e o fechamento grava `resolucao` em `app/chamados/models.py:2214,2201`. |
| `fechada_em` | Filtro da listagem em `app/chamados/views_disputa_admin.py:19`. A porta lê o campo na entrada e sob trava em `app/chamados/models.py:2099,2163`. | O fechamento grava o campo em `app/chamados/models.py:2199`. As reaberturas o limpam em `app/chamados/models.py:2419,2443`. |
| `colaborador.nome`, vínculo e PK das perguntas, `instancia_marco`, `data_evento`, `pergunta` | `app/templates/chamados/disputa_admin.html:3,18–23`. | Cadastro e edição do nome em `app/colaboradores/services/colaborador.py:204` e `app/colaboradores/views.py:1344`. O nascimento grava vínculo, texto e dia em `app/chamados/services/disputa_emissao.py:449` e `app/chamados/models.py:3330`. O marco é derivado em `app/chamados/models.py:3898`. O escritor do dia é `app/chamados/services/fio.py:950`. |
| `estado` da pergunta | `app/templates/chamados/disputa_admin.html:20`. | Propriedade calculada em `app/chamados/models.py:3351`. A leitura composta está em `app/chamados/models.py:4507` e a escrita da sequência em `app/chamados/models.py:4590`. |
| `base_tpl`, `app`, `via_app` | A tela usa a casca e preserva `?app=1` no formulário em `app/templates/chamados/disputa_admin.html:1,29`. | `app/core/context_processors.py:194` publica a casca. `app/core/utils.py:52,67` lê os sinais da sessão e da requisição. `app/core/views.py:1281` grava `via_app`. |

### REDs e mutantes para o Opus

| RED proposto | Mutante que deve falhar |
|---|---|
| Anônimo vai ao login. Usuário autenticado sem permissão recebe `403` e faz zero chamadas. Staff sem grupo e `admin_ti` sem staff passam. POST autorizado sem CSRF recebe `403` com a proteção ativa. GET na escrita e POST na listagem recebem `405`. `app/chamados/views_disputa_admin.py:13,28,36` · `app/config/settings/base.py:59` | Remover login, autorização, proteção CSRF ou restrição de método. Trocar `or` por `and` no predicado de admin. |
| GET mostra só disputas abertas do colaborador e todas as irmãs, inclusive com estados ou marcos distintos. POST com PK inválido ou de outro colaborador recebe `404` com zero chamadas. `app/chamados/views_disputa_admin.py:19,40` · `app/templates/chamados/disputa_admin.html:13,18` | Retirar o filtro de colaborador ou de abertura. Renderizar só a primeira irmã ou derivar o estado diretamente dos carimbos. |
| Motivo ausente, vazio ou só com espaços chega à porta uma vez e exibe a recusa literal, sem fechamento, carimbo, decisão ou evento do ato. Com uma irmã respondida sem veredito, a recusa identifica a pergunta e a disputa inteira permanece aberta. `app/chamados/views_disputa_admin.py:49` · `app/chamados/models.py:2122,2214` | Validar ou alterar o motivo na view, substituir o texto da porta ou fechar ignorando a irmã. |
| POST válido passa uma vez pela porta real, com disputa e ator corretos. Conferir campos do fechamento, carimbo das vivas elegíveis, `SILENCIO` e eventos. Reenvio do mesmo ator e motivo mostra “Nada mudou” e preserva os carimbos e as contagens. Motivo divergente mostra a recusa. `app/chamados/views_disputa_admin.py:43,52,65` · `app/chamados/models.py:2094,2184,2199` | Chamar duas vezes, usar `fechar` diretamente, filtrar disputas fechadas no POST ou tratar `False` como sucesso. |
| `RuntimeError` da porta e `ValueError` de fora da chamada sobem. A inspeção estrutural confirma ausência de escrita de domínio, relógio ou regra de C6, DNA e retenção na view. `app/chamados/views_disputa_admin.py:36,50,67` | Alargar o `except`, mover busca ou renderização para dentro do `try`, ou introduzir escrita ou decisão de domínio na view. |
| Smoke do Ronald nas sessões comum e `via_app`. Abrir a ficha, clicar em **Disputas**, conferir a casca e todas as irmãs, enviar pelo botão e conferir o feedback. Na ficha de usuário sem permissão que possa acessá-la, conferir a ausência do link. `app/templates/chamados/fio_colaborador.html:11` · `app/templates/chamados/disputa_admin.html:1,9,29` · `app/core/context_processors.py:194` | Fixar `base.html`, perder a casca entre GET e POST ou ocultar o feedback em uma das cascas. |

A checagem estática de sintaxe Python passou. A compilação do template ficou bloqueada porque o ambiente local não disponibilizou `django.template`. Suite, REDs, smoke de clique e efeitos sobre o chamado 40588, a célula 122468 e a folha **[nao verificado]**. [nao verificado]

## 28/11 11:32 · sessao 5209f6acefbe · E4C-F5-INSTANTE · leitura · origem=Code

## 1. Selo `core.observ` com outro import de `core`

As âncoras abaixo têm contagem **1** na a2 reconstruída em memória. `V` e `R` indicam os vereditos na cópia fornecida, na a2 reconstruída e com a cura. A equivalência Git da cópia com `486a1ffc4`, a execução de `486a1ffc4~1`, a suíte Django, o Ruff e a conferência pela sessão estão [nao verificado]. Rodei os métodos selecionados com `unittest.TestCase` e os analisadores reais. Os vermelhos foram `AssertionError`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:78–112` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165` · `app/chamados/tests/test_e4c_f5_c4_red.py:1557–1562` · `app/ruff.toml:4–6`

Em `_dicts_da_trilha`, substitua o bloco `else` de dois versos da a2 pelo trecho abaixo. Um `import core.X` sem alias mantém o vínculo com o pacote `core`. Um alias explícito continua sujeito à regra de religamento. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–109`

```diff
-                else:
+                elif not (isinstance(n, ast.Import) and not a.asname and a.name.startswith('core.')):
                     religados.add(nome)
```

Insira antes de `class SeloEscritorUnicoDoCarimbo(SimpleTestCase):`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:263`

```python
def _fonte_trilha_com_dois_imports_core(campo):
    return (
        'import core.observ\n'
        'import core.models\n'
        'def f():\n'
        f'    core.observ.evento(depois=dict({campo}=None))\n'
    )


```

No par, substitua o fecho do import da a2. `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:30`

```diff
     _dicts_da_trilha,
+    _fonte_trilha_com_dois_imports_core,
 )
```

No carimbo, insira antes de `def test_MORDE_dict_em_variavel_defaults_construtor_e_fields_em_variavel(self):`. No par, insira antes de `def test_MORDE_repasse_de_param_na_classe_do_modelo(self):`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:333` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:269`

```python
    def test_CONTROLE_trilha_com_dois_imports_core(self):
        self.assertEqual(escritas_do_carimbo(_fonte_trilha_com_dois_imports_core('materializada_em')), [])

```

```python
    def test_CONTROLE_trilha_com_dois_imports_core(self):
        self.assertEqual(escritas_do_par(_fonte_trilha_com_dois_imports_core('respondida_em')), [])

```

Nos dois selos, `test_CONTROLE_trilha_com_dois_imports_core` dá **486 V · base V esperada [nao verificado] · a2 R · cura V**. O mutante **M2-core** produz um achado onde cada teste exige `[]`. Original e mutado, contagem **1** na cura. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165`

```diff
-                elif not (isinstance(n, ast.Import) and not a.asname and a.name.startswith('core.')):
+                else:
                     religados.add(nome)
```

## 2. Ligações em campos `str` nos dois selos

Em `_dicts_da_trilha`, insira imediatamente antes de `    if 'dict' in religados:` da a2. Incluí os três campos `name` de parâmetros de tipo. A imagem da aplicação usa Python 3.13. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112` · `app/Dockerfile:1`

```python
        elif isinstance(n, (ast.ExceptHandler, ast.MatchAs, ast.MatchStar)) and n.name is not None:
            religados.add(n.name)
        elif isinstance(n, ast.MatchMapping) and n.rest is not None:
            religados.add(n.rest)
        elif isinstance(n, (ast.Global, ast.Nonlocal)):
            religados.update(n.names)
        elif isinstance(n, (ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)):
            religados.add(n.name)
```

Insira antes de `def _fonte_trilha_com_dois_imports_core(campo):`, introduzido no item 1. No par, insira `    _casos_trilha_ligacao_em_campo_str,` imediatamente depois de `    _casos_trilha_evento_religado,` do import da a2. O `nonlocal` usa um import legítimo na função externa, de modo que retirar a regra de `Nonlocal` derruba o teste mesmo com a regra de parâmetros ativa. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112,263` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:30`

```python
def _casos_trilha_ligacao_em_campo_str(campo):
    for nome in ('evento', 'dict'):
        inicio = (
            'from core.observ import evento\n'
            f'def g():\n    evento(depois=dict({campo}=None))\n'
        )
        chamada = f'evento(depois=dict({campo}=None))'
        casos = (
            ('ExceptHandler.name', f'def f():\n    try:\n        pass\n    except E as {nome}:\n', 8, '        '),
            ('MatchAs.name', f'def f(x):\n    match x:\n        case {nome}:\n', 7, '            '),
            ('MatchStar.name', f'def f(x):\n    match x:\n        case [*{nome}]:\n', 7, '            '),
            ('MatchMapping.rest', f'def f(x):\n    match x:\n        case {{**{nome}}}:\n', 7, '            '),
            ('Global.names', f'def f():\n    global {nome}\n', 6, '    '),
            ('Nonlocal.names',
             f'def f():\n    from core.observ import evento as {nome}\n'
             f'    def interna():\n        nonlocal {nome}\n', 8, '        '),
            ('TypeVar.name', f'def f[{nome}]():\n', 5, '    '),
            ('ParamSpec.name', f'def f[**{nome}]():\n', 5, '    '),
            ('TypeVarTuple.name', f'def f[*{nome}]():\n', 5, '    '),
        )
        for caso, corpo, linha, recuo in casos:
            yield caso, nome, inicio + corpo + recuo + chamada + '\n', linha


```

Insira os métodos nas mesmas âncoras de classe do item 1, carimbo e par respectivamente. Cada teste exige o achado no sítio afetado e em `g`, antes do religamento. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:222–225,333` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165,269`

```python
    def test_RED_trilha_acusa_ligacao_em_campo_str(self):
        for caso, nome, fonte, linha in _casos_trilha_ligacao_em_campo_str('materializada_em'):
            achados = escritas_do_carimbo(fonte)
            alvo = 'f.interna' if caso == 'Nonlocal.names' else 'f'
            for funcao, numero in ((alvo, linha), ('g', 3)):
                with self.subTest(caso=caso, nome=nome, funcao=funcao):
                    self.assertIn((funcao, numero, 'dict(materializada_em=)'), achados)

```

```python
    def test_RED_trilha_acusa_ligacao_em_campo_str(self):
        for caso, nome, fonte, linha in _casos_trilha_ligacao_em_campo_str('respondida_em'):
            achados = escritas_do_par(fonte)
            alvo = 'f.interna' if caso == 'Nonlocal.names' else 'f'
            for funcao, numero in ((alvo, linha), ('g', 3)):
                with self.subTest(caso=caso, nome=nome, funcao=funcao):
                    self.assertIn((funcao, numero, 'dict(respondida_em=)'), achados)

```

Nos dois selos, `test_RED_trilha_acusa_ligacao_em_campo_str` dá **486 R · base R esperada [nao verificado] · a2 R · cura V**. Cada mutante abaixo tem original de contagem **1** na cura e derruba quatro asserções por selo. `\n` indica uma quebra literal. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112,222–225` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165`

| Mutante | Original exato | Mutado exato |
|---|---|---|
| `ExceptHandler.name` | `(ast.ExceptHandler, ast.MatchAs, ast.MatchStar)` | `(ast.MatchAs, ast.MatchStar)` |
| `MatchAs.name` | `(ast.ExceptHandler, ast.MatchAs, ast.MatchStar)` | `(ast.ExceptHandler, ast.MatchStar)` |
| `MatchStar.name` | `(ast.ExceptHandler, ast.MatchAs, ast.MatchStar)` | `(ast.ExceptHandler, ast.MatchAs)` |
| `MatchMapping.rest` | `"        elif isinstance(n, ast.MatchMapping) and n.rest is not None:\n            religados.add(n.rest)\n"` | `""` |
| `Global.names` | `(ast.Global, ast.Nonlocal)` | `ast.Nonlocal` |
| `Nonlocal.names` | `(ast.Global, ast.Nonlocal)` | `ast.Global` |
| `TypeVar.name` | `(ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)` | `(ast.ParamSpec, ast.TypeVarTuple)` |
| `ParamSpec.name` | `(ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)` | `(ast.TypeVar, ast.TypeVarTuple)` |
| `TypeVarTuple.name` | `(ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)` | `(ast.TypeVar, ast.ParamSpec)` |

Atualize os dois mutantes da a2 cujas âncoras mudaram. `M2-import-global` mata `test_RED_trilha_acusa_import_local_homonimo` e `M2-sem-parametro` mata esse teste e `test_RED_trilha_acusa_dict_religado`. Em ambos os selos, esses dois testes dão **486 R · base R esperada [nao verificado] · a2 V · cura V**. `M2-grafia-dict` conserva o bloco exato da a2 e mata também o novo RED nos casos de `dict`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:85–112,222–225` · `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:144–165`

```diff
@@ M2-import-global
                 elif not (isinstance(n, ast.Import) and not a.asname and a.name.startswith('core.')):
-                    religados.add(nome)
+                    pass
```

`M2-sem-parametro` troca `"        elif isinstance(n, ast.arg):\n            religados.add(n.arg)\n"` por `""`, com contagem **1**. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:94–99`

## 3. Censo com uma tabela de ligações e registro de projeção não resolvida

Cada escopo passa a ter uma tabela `ligacoes`. A coleta registra uma vez os vínculos sintáticos. A inferência legada de modelo escreve a origem nas mesmas entradas, e a projeção lê os vínculos lexicais dessas entradas. A resolução segue atribuições diretas, aliases e desempacotamento literal. Para `for` e `with`, a ligação local encobre a externa e o valor projetado fica indeterminado. Projeção indeterminada gera `save` com `entidade=None`, no balde existente, e `projecao=?` na expressão. Isso inclui `projecao=None` explícita. O delta de produção acrescenta zero `except`. `app/core/censo_escritas.py:200–217,300–377,421–485,561–565` · `app/chamados/models.py:4789–4803`

Em `app/core/censo_escritas.py`, insira `from collections import deque` imediatamente depois de `import os`. Insira o bloco abaixo imediatamente antes de `class _Varredor(ast.NodeVisitor):`. Cada âncora tem contagem **1**. `app/core/censo_escritas.py:58–59,199–211`

```python
_NOS_DE_ESCOPO = (
    ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef, ast.Lambda,
    ast.ListComp, ast.SetComp, ast.DictComp, ast.GeneratorExp,
)


class _Ligacao:
    def __init__(self):
        self.vinculos = []  # (ordem, local, valor da projecao, evidencia do modelo)
        self.campos = set()
        self.origem = None


```

No `__init__` de `_Varredor`, substitua o intervalo de `        self._funcao = []` até antes de `        # E3-CENSO-APELIDO` por esta linha. Substitua depois o intervalo de `    # -- escopos` até antes de `    def visit_ClassDef(self, no):` pelo bloco completo. Cada delimitador tem contagem **1**. `app/core/censo_escritas.py:209–217,300–319`

```python
        self._escopos = []  # pilha de (no do escopo, ligacoes)
```

```python
    # -- escopos
    def _ligar_alvo(self, ligacoes, alvo, valor, origem, local, ordem):
        if isinstance(alvo, ast.Name):
            ligacoes.setdefault(alvo.id, _Ligacao()).vinculos.append((ordem, local, valor, origem))
        elif isinstance(alvo, ast.Starred):
            self._ligar_alvo(ligacoes, alvo.value, None, None, local, ordem)
        elif isinstance(alvo, (ast.Tuple, ast.List)):
            elts = ()
            if (isinstance(valor, (ast.Tuple, ast.List)) and len(alvo.elts) == len(valor.elts)
                    and not any(isinstance(e, ast.Starred) for e in (*alvo.elts, *valor.elts))):
                elts = valor.elts
            for i, item in enumerate(alvo.elts):
                evid = origem if i == 0 and isinstance(alvo, ast.Tuple) and isinstance(item, ast.Name) else None
                self._ligar_alvo(ligacoes, item, elts[i] if elts else None, evid, local, ordem)

    def _colher_ligacoes(self, no, inferir):
        # A origem do modelo usa a ordem em largura e pode ler escopos filhos,
        # como antes. A tupla usa apenas os vinculos lexicais do escopo.
        ligacoes = {}
        fila = deque((sub, True, True) for sub in ast.iter_child_nodes(no))
        ordem = 0
        while fila:
            sub, local, direta = fila.popleft()
            ordem += 1
            if not local and not inferir:
                continue
            filhos_locais = local and not isinstance(sub, _NOS_DE_ESCOPO)
            fila.extend((filho, filhos_locais, direta and isinstance(sub, ast.Expr))
                        for filho in ast.iter_child_nodes(sub))
            alvos, valor, origem = (), None, None
            if isinstance(sub, ast.Assign):
                alvos, valor, origem = sub.targets, sub.value, sub.value
                for alvo in alvos:
                    if (inferir and isinstance(alvo, ast.Attribute) and isinstance(alvo.value, ast.Name)
                            and alvo.attr in self.unicos):
                        ligacoes.setdefault(alvo.value.id, _Ligacao()).campos.add(self.unicos[alvo.attr])
            elif isinstance(sub, ast.AnnAssign) and sub.value is None:
                if local and isinstance(sub.target, ast.Name):
                    self._ligar_alvo(ligacoes, sub.target, sub, None, local, ordem)
            elif isinstance(sub, (ast.AnnAssign, ast.NamedExpr)):
                alvos, valor, origem = (sub.target,), sub.value, sub.value
            elif isinstance(sub, ast.AugAssign):
                alvos, origem = (sub.target,), sub.value
            elif isinstance(sub, (ast.For, ast.AsyncFor, ast.comprehension)):
                alvos, origem = (sub.target,), sub.iter
            elif isinstance(sub, ast.withitem) and sub.optional_vars is not None:
                alvos, origem = (sub.optional_vars,), sub.context_expr
            elif isinstance(sub, (ast.Delete, ast.TypeAlias)):
                alvos = sub.targets if isinstance(sub, ast.Delete) else (sub.name,)
            for alvo in alvos:
                self._ligar_alvo(ligacoes, alvo, valor if direta else None, origem, local, ordem)
            if not local:
                continue
            nomes = ()
            if isinstance(sub, (ast.Import, ast.ImportFrom)):
                nomes = (a.asname or a.name.split('.')[0] for a in sub.names)
            elif isinstance(sub, (ast.Global, ast.Nonlocal)):
                nomes = sub.names
            elif isinstance(sub, ast.arg):
                nomes = (sub.arg,)
            elif isinstance(sub, ast.MatchMapping):
                nomes = (sub.rest,) if sub.rest is not None else ()
            elif isinstance(sub, (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef, ast.ExceptHandler,
                                  ast.MatchAs, ast.MatchStar, ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)):
                nomes = (sub.name,) if sub.name is not None else ()
            for nome in nomes:
                ligacoes.setdefault(nome, _Ligacao()).vinculos.append((ordem, True, None, None))
        return ligacoes

    def _inferir_amarras(self, ligacoes):
        # Ponto fixo sobre os mesmos vinculos: qs = X.objects.filter(); for b in qs: b.save().
        evidencias = sorted(
            ((ordem, nome, valor) for nome, ligacao in ligacoes.items()
             for ordem, _, _, valor in ligacao.vinculos if valor is not None),
            key=lambda item: item[0],
        )
        mudou = True
        while mudou:
            mudou = False
            for _, nome, valor in evidencias:
                ligacao = ligacoes[nome]
                if ligacao.origem:
                    continue
                origem = self._origem(valor)
                if not origem and isinstance(valor, ast.Name) and valor.id in ligacoes:
                    origem = ligacoes[valor.id].origem
                    origem = None if origem == '__nova__' else origem
                if origem:
                    ligacao.origem = origem
                    mudou = True
        for ligacao in ligacoes.values():
            if not ligacao.origem and len(ligacao.campos) == 1:
                ligacao.origem = ('campo', next(iter(ligacao.campos)))

    def _visitar_escopo(self, no, inferir=False):
        ligacoes = self._colher_ligacoes(no, inferir)
        if inferir:
            self._inferir_amarras(ligacoes)
        self._escopos.append((no, ligacoes))
        self.generic_visit(no)
        self._escopos.pop()

    visit_Module = visit_Lambda = _visitar_escopo
    visit_ListComp = visit_SetComp = visit_DictComp = visit_GeneratorExp = _visitar_escopo

```

Em `visit_ClassDef`, aplique a primeira troca abaixo. Em `_visit_funcao`, substitua o intervalo de `        self._imports_locais.append(locais)` até antes de `        self._imports_locais.pop()` pelas duas linhas indicadas. Aplique também as trocas em `_no_registrar_da_decisao` e `_variavel`. Cada bloco ou delimitador tem contagem **1**. `app/core/censo_escritas.py:315–377,435–440,460–474`

```diff
@@ visit_ClassDef
-        self._visitar_escopo_da_projecao(no)
+        self._visitar_escopo(no)
         self._classe.pop()
@@ _no_registrar_da_decisao
-        if len(self._escopos_da_projecao) != 3:
+        if len(self._escopos) != 3:
             return False
-        _, (classe, _), (metodo, _) = self._escopos_da_projecao
+        _, (classe, _), (metodo, _) = self._escopos
@@ _variavel
-            for amarras in reversed(self._funcao):
-                origem = amarras.get(rec.id)
+            for _, ligacoes in reversed(self._escopos):
+                ligacao = ligacoes.get(rec.id)
+                origem = ligacao.origem if ligacao is not None else None
```

```python
        self._imports_locais.append(locais)
        self._visitar_escopo(no, inferir=True)
```

Substitua o intervalo de `    def _projecao(self, no, valor):` até antes de `        inst, campos = valor.elts` por este bloco. Cada delimitador tem contagem **1**. `app/core/censo_escritas.py:442–454`

```python
    def _valor_da_projecao(self, valor):
        limite = len(self._escopos) - 1
        while isinstance(valor, (ast.Name, ast.NamedExpr)):
            if isinstance(valor, ast.NamedExpr):
                valor = valor.value
                continue
            for i in range(limite, -1, -1):
                escopo, ligacoes = self._escopos[i]
                if isinstance(escopo, ast.ClassDef) and i != limite:
                    continue
                ligacao = ligacoes.get(valor.id)
                if ligacao is not None:
                    locais = [v for _, local, v, _ in ligacao.vinculos if local]
                    if locais:
                        break
            else:
                return None
            locais = [v for v in locais if not isinstance(v, ast.AnnAssign)]
            if len(locais) != 1 or locais[0] is None:
                return None
            ligado = locais[0]
            if (ligado.end_lineno, ligado.end_col_offset) > (valor.lineno, valor.col_offset):
                return None
            valor, limite = ligado, i
        return valor

    def _projecao(self, no, valor):
        """`projecao=(inst, campos)` = `inst.save(update_fields=campos)` neste sitio (`ESCRITA_POR_PROJECAO`)."""
        valor = self._valor_da_projecao(valor)
        if (not isinstance(valor, ast.Tuple) or len(valor.elts) != 2
                or any(isinstance(e, ast.Starred) for e in valor.elts)):
            self._anotar(no, None, 'save', '%s(projecao=?) [projecao]' % _texto(no.func))
            return
```

Insira os dois REDs em `CensoLeAProjecaoTest`, antes de `    def test_os_dois_saves_da_porta_ficam_visiveis(self):`, contagem **1**. `for` e `with` incluem uma tupla externa válida para que apagar a ligação local produza uma classificação errada e derrube a asserção. `app/chamados/tests/test_e4c_f5_c4_red.py:1557–1562,1576` · `app/core/censo_escritas.py:308–313,333–356,442–454`

```python
    def test_RED_projecao_resolve_ligacoes_lexicais(self):
        inicio = 'from chamados.models import DecisaoPergunta as DP\n'
        tupla = "(p, ['respon

[truncado em 20000 de 29418 caracteres]

## 28/11 11:30 · sessao 20970be2aa1e · C7-TELA-ADMIN-ENCERRA · leitura · origem=Code

A porta atual **encerra a disputa inteira**. Ela pode carimbar, no mesmo ato, perguntas que a célula ainda sustenta e perguntas que perderam o marco previsto. Para a obra que pede encerramento seletivo, falta uma decisão de escopo antes de ligar o botão. `app/chamados/models.py:2122`, `app/chamados/models.py:2184`, `app/chamados/services/disputa_emissao.py:1110`, `app/chamados/services/disputa_emissao.py:1167`

### 1. Porta e caso 40588

- `admin_encerra(user, motivo)` aplica `strip()`, rejeita motivo vazio com `ValueError` e chama `fechar` com `resolucao=texto` e `motivo_fechamento='admin_encerra'`. Para uma disputa aberta, `fechar` exige `user`. A permissão de staff/admin precisa ser aplicada na borda HTTP. `app/chamados/models.py:2094`, `app/chamados/models.py:2210`, `app/chamados/services/acoes_disputa.py:284`
- A guarda lê **todas** as perguntas da disputa. Uma resposta com instante e texto, sem validação nem materialização e fora de dia trancado, barra o ato inteiro. O `ValueError` nomeia as perguntas e manda validar ou recusar as respostas na mesa. A porta relê a disputa e repete a guarda sob trava. `app/chamados/juizes.py:1644`, `app/chamados/models.py:2041`, `app/chamados/models.py:2122`, `app/chamados/models.py:2151`, `app/chamados/models.py:2163`
- Para cada pergunta que o C6 considera viva, a porta tenta gravar `via_resolucao='admin_decidiu'`, `materializada_em` e `materializada_por`. Quando o carimbo grava, registra `DecisaoPergunta.SILENCIO` com autor, instante do carimbo, foto da resposta e motivo limitado a 240 caracteres, além de `pergunta_encerrada_com_a_disputa`. Depois grava `fechada_em`, `fechada_por`, `resolucao` e `motivo_fechamento` na disputa, emite `disputa_fechada` e retorna `True`. Os eventos de `LogAuditoria` são best-effort. O carimbo de identidade tem guardas próprias e pode devolver `False`, caso em que aquele `SILENCIO` e evento da pergunta são pulados. `app/chamados/models.py:2132`, `app/chamados/models.py:2184`, `app/chamados/models.py:2199`, `app/chamados/models.py:3372`, `app/chamados/models.py:3544`, `app/core/observ.py:94`, `app/core/observ.py:133`
- Mesma disputa, mesmo autor, mesmo motivo após `strip()` e mesmo código de fechamento devolvem `False` sem nova escrita ou trilha. Entrada diferente sobre disputa já fechada levanta `ValueError`. Se o fechamento concorrente só aparece na releitura sob a trava, devolve `False` sem regravar. `app/chamados/models.py:2094`, `app/chamados/models.py:2168`, `app/chamados/tests/test_e4_expand_orm.py:64`
- **61520/61521** entram no carimbo se estiverem vivas pelo C6 nas respectivas disputas abertas no instante do ato, se nenhuma irmã barrar o fechamento e se o carimbo gravar. A sonda do enunciado informa 88 vivas na comp. 11, 68 da emp2 e 20 da emp3, e descreve a emissão às 12:36 e a regeneração da célula 122468 às 12:42. PKs das disputas, lista integral de irmãs e estado atual das guardas **[nao verificado]**. `app/chamados/models.py:2122`, `app/chamados/models.py:2175`, `app/chamados/models.py:2184`
- **Disputa mista é possível.** O emissor reaproveita uma disputa aberta do colaborador e agrega nela perguntas de intervalo e de outros marcos ou chamados. O laço de encerramento usa o C6 sobre a disputa inteira. A falta de intervalo no DNA atual, por si só, não seleciona o que será carimbado. `app/chamados/services/disputa_emissao.py:1110`, `app/chamados/services/disputa_emissao.py:1167`, `app/chamados/services/disputa_emissao.py:1180`, `app/chamados/juizes.py:1551`, `app/chamados/models.py:2184`

**lei:** O aval permite encerrar a disputa inteira, inclusive uma irmã viva ainda sustentada pela célula, ou a irmã sustentada deve permanecer aberta? Se deve permanecer, qual porta existente o aval autoriza para o ato seletivo? `admin_encerra` recebe a disputa e o motivo, sem alvo por pergunta. `app/chamados/models.py:2184`, `app/chamados/models.py:2210`

### 2. Chamado 40588 e retenção do dia

- **Chamado.** O fechamento da disputa dispara `reconciliar()` para o chamado-pai vinculado. O chamado-destino da pergunta pode ser outro. O motor, a premissa, a conversa do pai e as perguntas vivas participam da decisão de fechar o chamado. O carimbo também emite evento da célula quando ela é localizada. O consumidor consulta o lastro de marcos **acesos**, e o cartório tem um ramo próprio para marcos **inexistentes** que também passa por `reconciliar()`. Se 40588 é pai ou destino e se de fato fecha após o clique **[nao verificado]**. `app/chamados/signals.py:19`, `app/chamados/signals.py:244`, `app/chamados/models.py:691`, `app/chamados/models.py:3600`, `app/chamados/services/fio.py:97`, `app/ponto/services/lastro.py:394`, `app/ponto/services/cartorio.py:1600`
- **Dia e folha.** `motivos_retencao_celula` lê o veredito e os protestos da `CelulaDia`. Considera dia passado sem decisão, vínculo e janela, isenção de ponto, desligamento, protestos integralmente não cobráveis e a exceção de furo só de pausa. Mudança de estado do chamado pode provocar nova lavra pelo cartório. `classificar_export` decide o TXT para a competência inteira, lê a retenção do espelho e aplica os demais crivos. Veredito e protestos da 122468 após o ato, outros dias retidos e resultado do TXT **[nao verificado]**. `app/folha/export.py:774`, `app/folha/export.py:984`, `app/folha/export.py:1189`, `app/folha/export.py:1221`, `app/folha/export.py:1267`, `app/folha/export.py:1292`, `app/folha/export.py:1329`, `app/chamados/signals.py:189`

**Texto ao admin** — “A disputa foi encerrada e o motivo ficou registrado. Confira no espelho se o dia ainda tem pendência para a folha.” `app/chamados/models.py:2199`, `app/folha/export.py:1192`

### 3. Mesa atual e desenho mínimo

| Casca | Caminho na cópia |
|---|---|
| `base.html` | A mesa de validação está na Central, `/chamados/painel/`. O painel carrega `/chamados/modal/<colaborador_id>/`, `modal_fio_colaborador`, com `chamados/partials/modal_fio.html`, onde ficam Validar e Rejeitar. A Central liga também à ficha completa `/chamados/colaborador/<colaborador_id>/`, `fio_colaborador`, com `chamados/fio_colaborador.html`. Há ainda ações no detalhe `/chamados/local/<chamado_id>/`, `detalhe_chamado_local`. `app/config/urls.py:28`, `app/chamados/urls.py:14`, `app/chamados/urls.py:21`, `app/chamados/urls.py:39`, `app/chamados/views.py:698`, `app/chamados/views.py:928`, `app/templates/chamados/painel_gestao.html:1`, `app/templates/chamados/painel_gestao.html:490`, `app/templates/chamados/painel_gestao.html:528`, `app/templates/chamados/partials/modal_fio.html:306`, `app/templates/chamados/detalhe_local.html:310` |
| `base_app.html` | `/chamados/meu-atendimento/`, `meu_atendimento`, renderiza `chamados/meu_atendimento.html` com `base_tpl`. É o fio do colaborador logado e passa `is_admin_ti=False`. Os templates do painel e do fio administrativo fixam `base.html`. Na cópia, a mesa administrativa fica nessa casca. `app/chamados/urls.py:16`, `app/chamados/views.py:559`, `app/chamados/views.py:633`, `app/chamados/views.py:650`, `app/templates/chamados/meu_atendimento.html:1`, `app/templates/chamados/fio_colaborador.html:1`, `app/templates/chamados/painel_gestao.html:1`, `app/core/context_processors.py:194` |

**Desenho proposto, condicionado ao aval de disputa inteira.** Link “Disputas” na ficha completa para uma página administrativa do colaborador, com um formulário simples por disputa aberta. A página usa `base_tpl`, identifica a disputa e mostra suas perguntas antes do envio. Isso cobre também disputas que compartilham um chamado-destino, pois o cartão do fio elege uma única disputa por chamado. Campo `motivo` com rótulo e `required`, botão **Encerrar disputa com motivo**, `{% csrf_token %}` e POST dedicado com `disputa_pk` explícito. GET e POST exigem sessão e `is_staff` ou grupo `admin_ti`, seguindo a ação administrativa vizinha. O POST carrega a disputa pelo PK e pelo colaborador da rota e chama a porta uma vez, inclusive no reenvio de disputa fechada. `app/chamados/services/fio.py:136`, `app/chamados/views.py:708`, `app/chamados/services/acoes_disputa.py:274`, `app/core/context_processors.py:194`, `app/config/settings/base.py:59`, `app/chamados/models.py:2094`

**lei:** O veto a `except` abrange a tradução de `ValueError` na borda HTTP? A porta levanta `ValueError` para motivo vazio, resposta sem veredito e fechamento anterior divergente. `resposta_acao` e `resposta_render` recebem uma mensagem pronta. O precedente da reabertura administrativa captura `ValueError` ao redor da chamada. Com o veto literal, a exibição da recusa precisa de um contrato autorizado antes de implementar o POST. `app/chamados/models.py:2094`, `app/chamados/models.py:2124`, `app/chamados/models.py:2214`, `app/core/respostas.py:20`, `app/core/respostas.py:74`, `app/chamados/services/acoes_disputa.py:299`

### 4. Censo

- **`admin_encerra`** tem zero chamadores diretos de produção na busca estática da cópia. Há chamadas de teste para motivo vazio, carimbo e trilha, repetição, corrida e cascata. `app/docs/BACKLOG.md:1640`, `app/chamados/tests/test_c7_red_minimos.py:250`, `app/chamados/tests/test_e4_expand_orm.py:64`, `app/chamados/tests/test_f5_contrato_v1_guarda_sob_trava.py:198`, `app/chamados/tests/test_guarda_disputa_status.py:69`
- **`DisputaSupervisao.fechar`**, chamadas diretas de produção. No modelo, `admin_encerra` e `remover_motivo`. Nas exceções, desligamento e competência trancada. No fluxo ordinário, becos, emissão e saída registrada, materialização de ausência e validação, três caminhos do reconciliador, foto recebida, entrada sincronizada e o comando de perguntas com fato. `app/chamados/models.py:2217`, `app/chamados/models.py:2342`, `app/colaboradores/services/desligamento.py:167`, `app/chamados/services/competencia_trancada.py:232`, `app/chamados/services/becos.py:61`, `app/chamados/services/disputa_emissao.py:1298`, `app/chamados/services/disputa_emissao.py:2224`, `app/chamados/services/materializacao.py:1159`, `app/chamados/services/materializacao.py:2160`, `app/chamados/reconciliador.py:415`, `app/chamados/reconciliador.py:719`, `app/chamados/reconciliador.py:1314`, `app/api/foto_recebida.py:126`, `app/ponto/services/entrada_sincronizada.py:60`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:89`
- **`motivo_fechamento`** é lido pelo próprio `fechar` na idempotência e pelo contador `disputa_vazia`, que filtra `FECHOU_VAZIA`. O inventário da sombra classifica a coluna como código de choices. Nenhum leitor de template apareceu na busca estática. `app/chamados/models.py:2094`, `app/chamados/services/vigia_fio.py:736`, `bin/sombra_regras_pessoais.py:175`
- **Selos de front.** Modal nasce oculto e preenchido, alcançável pelas duas cascas. A varredura de templates verifica também que o overlay tem um único dono. O selo de popover varre todos os `.html` e pega `<details>` de ação com lado fixo sem os atributos do componente. O smoke Chromium das duas cascas mede overlay e console, e há selo de toast nas duas. O smoke B8 procura POST em teste por heurística, cujo padrão atual de escrita não inclui `.admin_encerra(`. O smoke de clique desta obra precisa exercitar o botão explicitamente. `app/core/tests/test_selo_modal_nao_nasce_vazio.py:40`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:98`, `app/core/tests/test_selo_modal_nao_nasce_vazio.py:121`, `app/core/tests/test_selo_popover_acao.py:46`, `app/core/tests/test_selo_popover_acao.py:63`, `app/core/tests/test_selo_popover_acao.py:97`, `app/core/tests/test_smoke_chromium.py:172`, `app/core/tests/test_toast_le_o_motivo.py:100`, `app/core/tests/test_contract_smoke_porta.py:22`, `app/core/tests/test_contract_smoke_porta.py:64`, `app/docs/GRADE-MESTRA.md:650`

### 5. Arquivos em voo

- **Zero interseção com o anexo fornecido** para a proposta de quatro arquivos. Editar `app/chamados/urls.py` e `app/templates/chamados/fio_colaborador.html`. Criar `app/chamados/views_disputa_admin.py` e `app/templates/chamados/disputa_admin.html`. A Central já chega à ficha completa pelo botão “Ficha completa”. `app/chamados/urls.py:21`, `app/templates/chamados/fio_colaborador.html:1`, `app/templates/chamados/painel_gestao.html:257`, `app/templates/chamados/painel_gestao.html:490`
- Editar a porta no modelo conflita com as listas `486a1ffc4` e `c9fded10a` do anexo. Plugar o botão diretamente pelo modal, seu roteador ou seus cards toca `views.py`, `services/fio.py`, `modal_fio.html`, `_pergunta_card.html` ou `_card_atendimento.html`, listados em `f300431af`. Ponta atual das ramas e hash Git da cópia **[nao verificado]**. `app/chamados/models.py:2210`, `app/chamados/views.py:870`, `app/chamados/services/fio.py:657`, `app/templates/chamados/partials/modal_fio.html:306`, `app/templates/chamados/detalhe_local.html:310`, `app/templates/chamados/meu_atendimento.html:127`

### 6. RED para o Opus

| Caso | Asserção |
|---|---|
| Acesso e CSRF | POST de usuário autenticado sem staff/admin recebe `403`, com zero chamadas à porta. POST autorizado sem token recebe `403` com CSRF habilitado no cliente de teste. O endpoint de escrita só aceita POST. `app/chamados/services/acoes_disputa.py:284`, `app/chamados/views.py:960`, `app/config/settings/base.py:59` |
| Motivo vazio e resposta sem veredito | Enviar campo ausente, vazio e só com espaços. Ver a recusa, com zero fechamento, carimbo, decisões novas e eventos do ato. Repetir com uma irmã respondida sem veredito e conferir que o texto da porta aparece e identifica a pergunta. `app/chamados/models.py:2041`, `app/chamados/models.py:2124`, `app/chamados/models.py:2214`, `app/chamados/tests/test_c7_red_minimos.py:250` |
| POST válido e repetição | Espionar a porta real e provar uma chamada por POST com a disputa e o usuário corretos. No primeiro, verificar campos da disputa, carimbo, `SILENCIO` e eventos. Repetir com o mesmo ator e motivo, passando de novo pela porta, e provar `False`, mesmos carimbos e mesmas contagens. `app/chamados/models.py:2094`, `app/chamados/models.py:2184`, `app/chamados/tests/test_c7_red_minimos.py:261`, `app/chamados/tests/test_e4_expand_orm.py:64` |
| Duas cascas | Renderizar e submeter como admin em sessão comum e em sessão `via_app`. Conferir a casca efetiva, o mesmo formulário e o feedback. Colaborador comum fica sem a ação. `app/core/tests/test_casca_telas_colab.py:35`, `app/core/context_processors.py:194`, `app/templates/base.html:334`, `app/templates/base_app.html:92` |
| Nenhuma regra na view | Reusar os AST C7 que verificam juiz paralelo e escrita fora da entidade. No RED da nova view, provar a delegação única e vedar decisão sobre DNA, C6 ou retenção, escrita de estado e relógio. O AST existente de exceções verifica captura ampla que engole defeito, com alcance menor que o veto literal desta obra. `app/chamados/tests/test_c7_red_minimos.py:525`, `app/chamados/tests/test_c7_red_minimos.py:563`, `app/chamados/tests/test_c7_red_minimos.py:593`, `app/core/tests/test_contract_excecoes_engolidas.py:64` |

### 7. Brief para o construtor

Após os dois cortes de `lei:`, implementar somente código de `app/`, nos quatro caminhos da seção 5. Na ficha completa, acrescentar o acesso administrativo à lista de disputas do colaborador. Na página com `base_tpl`, identificar o alcance do ato e enviar `motivo` por POST com CSRF e PK explícito. A view fina autentica, autoriza, busca a disputa e chama `disputa.admin_encerra(request.user, motivo)` uma vez. Exibir a recusa pelo contrato autorizado e tratar `False` como nenhuma alteração. A trilha vem da porta. **Proibido** regra na view ou no template, `except`, relógio solto e juiz novo. Testes ficam com o Opus. `app/chamados/views.py:698`, `app/core/context_processors.py:194`, `app/chamados/models.py:2094`, `app/chamados/models.py:2184`, `app/chamados/models.py:2210`, `app/chamados/tests/test_c7_red_minimos.py:593`

Auditoria somente por leitura da cópia e do anexo. Execução dos REDs, smoke e efeitos em produção **[nao verificado]**.

_8922 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
