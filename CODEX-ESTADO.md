# CODEX-ESTADO

> Ultima resposta processada: **28/11/2026 06:01** · publicado em **28/11/2026 06:02** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 28/11 06:01 · sessao 0d86d5029112 · CUSTO-DE-COORDENACAO · verificador · origem=Code

Verifiquei a cópia fornecida em modo somente leitura. Rodei `bash -n` nos seis scripts shell do delta e fiz sondas com entradas e mutações em memória. O hash Git independente, o RED anterior à cura, os selos de host que criam fixtures, a suíte integral, o pré-requisito de pouso e as métricas de produção ficam **[nao verificado]**. `bin/tests/test_hook_stop_borda.sh:335`, `bin/tests/test_mao_unica.sh:88`, `CLAUDE.md:428`, `brief_f1.md:9`

## Procedentes da volta 2

| # | Cura da classe e mordida do selo |
|---|---|
| **1** | **Parcial.** O CENSO lê as perguntas no brief inteiro e cobre título irmão, item `+`, prosa interrogativa e fecho ATX. Os quatro mutantes têm casos que distinguem essas regressões. Uma variante válida ainda passa. Com `### __Perguntas de lei__` seguido de `- Pode gravar?`, confirmei em memória que a lista fica vazia e o CENSO aprova. O reconhecedor só aceita a ênfase por asteriscos. `bin/disjuntor.py:1639`, `bin/disjuntor.py:1690`, `bin/disjuntor.py:1701`, `bin/disjuntor.py:1738`, `bin/tests/test_censo_antes_da_vaga.sh:117`, `bin/tests/test_censo_antes_da_vaga.sh:825` |
| **2** | **Parcial.** Seis dígitos, slug exato e empate entre leituras reais estão cobertos. `codex-sem-validar-digitos` e `codex-prefixo-nao-exato` mordem esses casos. A gramática segue duplicada e divergente. O Agent converte o caminho a minúsculas antes de testar o nome. Em memória, `wt-CODEX-OBRA-X-000001` ainda gerou o candidato `OBRA-X`, que a igualdade literal do wrapper rejeita. Isso mantém a classe de candidato Codex indevido. `bin/disjuntor.py:1833`, `bin/disjuntor.py:1840`, `bin/codex_pergunta.sh:460`, `bin/tests/test_censo_antes_da_vaga.sh:739`, `bin/tests/test_censo_antes_da_vaga.sh:858` |
| **3** | **Parcial.** Estado sem negrito e nota em outra coluna têm mutantes que mordem. O próprio alvo `SUPERADA por ITEM-B` ainda sai como `parado` para o topo e como andável para a lista. `**ABERTA** (a anterior foi **FECHADA**)` também continua sendo lida como fechada. Na cópia, confirmei `ORDEM-VIVA-1511-NOITE` como topo `ok`, embora a linha de 18/11 declare que a substitui. O selo cobre o topo sintético e preserva a leitura das linhas fora do topo. `bin/hook_stop_fila1.py:56`, `bin/hook_stop_fila1.py:172`, `bin/hook_stop_fila1.py:236`, `bin/hook_stop_fila1.py:261`, `app/docs/BACKLOG.md:8`, `app/docs/BACKLOG.md:1926`, `app/docs/BACKLOG.md:2649`, `bin/tests/test_hook_stop_borda.sh:125`, `bin/tests/test_hook_stop_borda.sh:357` |
| **4** | **Cura o colapso em `max`, com uma transição ainda invisível.** O Stop vê mudanças em qualquer contador e a trilha distingue as impressões. `b3-pelo-maximo`, `b3-sem-n` e `b3-trilha-sem-cauda` têm asserções sobre avisos e IDs distintos. Confirmei em memória que trocar as voltas procedentes de `{1,2,3}` para `{2,3,4}` mantém `(1,3)` e produz a mesma impressão. Uma volta nova pode, portanto, ficar muda quando outra deixa de contar entre dois Stops. `bin/disjuntor.py:1000`, `bin/disjuntor.py:1006`, `bin/hook_stop_fila1.py:1434`, `bin/hook_stop_fila1.py:1657`, `bin/tests/test_hook_stop_borda.sh:222`, `bin/tests/test_hook_stop_borda.sh:365` |
| **5** | **Parcial, com regressão na porta do merge.** Constantes, atribuições, `.update()`, referências compostas e o registro da árvore candidata entram nas fixtures. Os mutantes de projeção e de omissão da árvore mordem essas formas. O classificador agora executa Python da árvore ainda por julgar, antes de consultar o B3, com `exec` irrestrito e sem prazo. Além disso, a extração continua parcial. `agenda/café.py::f` some em memória porque a regex perde a referência antes da normalização de acentos. `bin/commit_msg.sh:38`, `bin/disjuntor.py:1306`, `bin/disjuntor.py:1313`, `bin/mao_unica.py:44`, `bin/mao_unica.py:54`, `bin/mao_unica.py:99`, `bin/tests/test_mao_unica.sh:62`, `bin/tests/test_mao_unica.sh:157`, `bin/tests/test_mao_unica.sh:239` |
| **8** | **Parcial, com regressão de ordem.** Stop/PAREI e os blocos do HANDOFF compartilham a leitura. Os mutantes de releitura têm dentes, e o do CENSO cobre só a obra inferida. Com obra explícita e exceção posterior, confirmei duas leituras no `main_pretool`. No Stop, antepor o BACKLOG a B4 e orçamento abriu outro caminho de falha. Com UTF-8 inválido e uma decisão B4 de bloquear disponível, a sonda saiu em `ALLOW` antes de chamar B4. `bin/disjuntor.py:1902`, `bin/disjuntor.py:1954`, `bin/disjuntor.py:2321`, `bin/hook_stop_fila1.py:187`, `bin/hook_stop_fila1.py:1586`, `bin/hook_stop_fila1.py:1666`, `bin/handoff_sessao.sh:309`, `bin/tests/test_censo_antes_da_vaga.sh:727`, `bin/tests/test_hook_stop_borda.sh:359` |
| **9** | **Parcial.** A prova obrigatória vem antes da borda. O prazo do `flock` preserva o TSV quando a trava está ocupada. Os mutantes de ordem, prazo e morte dentro de `diz` distinguem os casos respectivos. A classe de **confirmar aviso que nunca foi registrado** sobrevive. `_trilha` engole a falha de escrita e `_avisos_por_borda` grava mesmo assim. Injetei a falha em memória e o Stop seguinte calou o aviso. A serialização começa depois da coleta, então um Stop com fotografia antiga também pode gravá-la depois de um Stop com fotografia nova. `bin/hook_stop_fila1.py:45`, `bin/hook_stop_fila1.py:909`, `bin/hook_stop_fila1.py:952`, `bin/hook_stop_fila1.py:968`, `bin/hook_stop_fila1.py:1578`, `bin/hook_stop_fila1.py:1642`, `bin/tests/test_hook_stop_borda.sh:92`, `bin/tests/test_hook_stop_borda.sh:371` |
| **10** | **Cura a classe triada.** A prova antecede qualquer transição da borda, e a regravação ou zeragem preserva o inode que a prova compara. `borda-desvincula` reintroduz a perda de identidade e tem um caso com hardlink que a distingue quando o Stop grava. O selo ainda aceita um subprocesso que saiu antes de gravar, pois ignora seu resultado e só verifica a rejeição do alias depois. `bin/hook_stop_fila1.py:672`, `bin/hook_stop_fila1.py:683`, `bin/hook_stop_fila1.py:940`, `bin/hook_stop_fila1.py:1578`, `bin/tests/test_hook_stop_borda.sh:278`, `bin/tests/test_hook_stop_borda.sh:387` |
| **11** | **Parcial.** `faltam-vazio` caiu com `rc=1` e `RED anti-vacuidade` no programa extraído do selo. A outra vacuidade permanece no veredito. Removi `abaixo_limite` da entrada em memória. O controle deu `rc=1`. Com apenas o `if faltam` do veredito trocado por `if False`, deu `rc=0` e ainda imprimiu `o caso MORDE`. A contraprova compartilha o comparador, mas tem uma decisão de saída própria. `bin/tests/test_relato_guarda_pedido_de_patch.sh:297`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:311`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:323`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:354` |
| **12** | **Cura a concatenação implícita, parcialmente a classe.** `peneira-textual` caiu com `rc=1` e `RED porta cega`. O caminho com literais adjacentes entra, mas confirmei que a expressão constante `'app/docs/' + 'RELATO' + '.md'` fica de fora. A porta só procura o caminho já reunido num literal ou os dois componentes nos argumentos de uma chamada. `bin/tests/test_relato_guarda_pedido_de_patch.sh:117`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:211`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:234`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:355` |
| **13** | **Parcial.** Os dois mutantes caíram em memória pelos motivos declarados. O `open` interno e `Path(...).open('a')` recebem o veredito certo. Ainda confirmei dois erros da classe. Uma leitura por `Path(...).read_text()` dentro de um `open` externo em `'w'` fica de fora. `Path(...).open('a' + 'b')` entra como leitor porque o modo precisa ser um `ast.Constant`. `bin/tests/test_relato_guarda_pedido_de_patch.sh:88`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:235`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:244`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:356` |

## Escritores, leitores e significado

| Campo ou arquivo | Quem escreve, quem lê e onde divergem |
|---|---|
| **`hook_stop_borda.tsv`, `.lock` e `id=` da trilha** | `_borda_grava` escreve `tipo\|obra<TAB>hash16` pela transação `_avisos_por_borda`. `_borda_le` consome a impressão, `_prova_arquivo` consulta a identidade do TSV e da trava, e `_borda_trava` cria e toma a `.lock` por descritor. `_diz` passa a mesma chave e impressão à trilha. O formato concorda. A garantia semântica diverge, pois a presença no TSV pode representar aviso apenas tentado. Os outros escritores vistos são fixtures. `bin/hook_stop_fila1.py:672`, `bin/hook_stop_fila1.py:903`, `bin/hook_stop_fila1.py:923`, `bin/hook_stop_fila1.py:940`, `bin/hook_stop_fila1.py:968`, `bin/hook_stop_fila1.py:1656`, `bin/tests/test_hook_stop_borda.sh:233`, `bin/tests/test_disjuntor.sh:351` |
| **B3 `(n, nc)`** | `decide_b3` escreve o par em memória a partir de `reprovados` e `codex`. `_com_estado` persiste o estado e `rearma_b3` limpa as listas pela mesma porta. `julga_b3` repassa o mapa, o Stop usa os números, o merge usa presença via `estado_b3`, e o PreToolUse dispara a varredura. O ESTADO lê eventos `DISJUNTOR` e `LIBERA`. Os contadores concordam no caminho normal. No erro de gravação, `(0,0)` também significa **obra segue parada porque a liberação falhou**. O merge respeita a presença e o Stop imprime esses zeros como contagens. A cauda da trilha guarda a impressão, sem expor os números. `bin/disjuntor.py:365`, `bin/disjuntor.py:1006`, `bin/disjuntor.py:1056`, `bin/disjuntor.py:1073`, `bin/disjuntor.py:1082`, `bin/disjuntor.py:1116`, `bin/disjuntor.py:2286`, `bin/disjuntor.py:2433`, `bin/gerar_estado.py:297`, `bin/hook_stop_fila1.py:1434`, `bin/hook_stop_fila1.py:1657` |
| **`JUIZES` da casa e da árvore a pousar** | `app/core/juizes.py` declara família, pergunta e autoridade. O integrador também regrava o arquivo para baixar `PENDENTES`. `commit_msg.sh` captura a árvore do índice, e `mao_unica` lê casa e blob candidato, executa os dois textos e projeta os caminhos. O gerador do diagrama consulta `JUIZES` por família. Para o registro, o campo identifica quem responde. Para o merge, vira um conjunto de caminhos sensíveis. A projeção perde referências válidas como a do #5. `app/docs/JUIZES.md:10`, `app/core/juizes.py:7`, `app/core/juizes.py:190`, `app/core/integrador_lote.py:435`, `app/core/integrador_lote.py:442`, `bin/commit_msg.sh:20`, `bin/mao_unica.py:59`, `bin/mao_unica.py:76`, `bin/mao_unica.py:97`, `app/core/management/commands/gerar_diagrama.py:752` |
| **BACKLOG, foto e HANDOFF** | Marcador e bloco são escritos à mão. `backlog_estado.py` muda a coluna `[3]`, e `gerar_backlog.py` preserva e republica a janela. Stop, CENSO e HANDOFF leem os mesmos dados, com os vereditos divergentes do #3. O HANDOFF inicializa o temporário no shell, grava a foto no primeiro bloco Python e a lê nos dois blocos finais. Foto vazia pode representar fila vazia ou primeiro bloco que falhou antes da gravação. O painel publicado vai ao SessionStart, e o Stop lê seu `mtime`. O medidor de interrompidas ainda usa o alvo mesmo com veredito `parado`, e o alarme mantém seu próprio predicado de abertura sobre a linha inteira. `bin/backlog_estado.py:58`, `bin/gerar_backlog.py:103`, `bin/gerar_backlog.py:138`, `bin/disjuntor.py:1800`, `bin/hook_stop_fila1.py:1586`, `bin/handoff_sessao.sh:25`, `bin/handoff_sessao.sh:60`, `bin/handoff_sessao.sh:309`, `bin/handoff_sessao.sh:331`, `bin/handoff_sessao.sh:345`, `.claude/settings.json:40`, `bin/hook_stop_fila1.py:1063`, `bin/interrompidas_estado.py:93`, `bin/interrompidas_estado.py:121`, `bin/alarme_sessao_ociosa.py:89` |
| **Modo de `open` no selo do RELATO** | O autor do teste escreve o modo na chamada. `_em_escrita` lê sua posição ou `mode=` no AST, e `_entra` decide se o teste integra o universo. O selo lê o RELATO vivo e altera só uma cópia em memória. As rotinas do vigia escrevem o RELATO por `no_relato` e `no_topo`. A semântica coincide para os modos literais cobertos. Diverge quando a AST atribui ao caminho o modo de outra abertura ou deixa de reconhecer um modo computado, como no #13. `app/ponto/tests/test_tela_gestao_he_fatia2_lote_limite.py:239`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:88`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:126`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:262`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:309`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:326`, `app/core/esteira_vigia.py:815`, `app/core/esteira_vigia.py:865` |

## Checklist 5(a) e escopo

- **Juiz e derivação.** `mao_unica` classifica o merge sem reproduzir os juízos de domínio. As derivações paralelas com efeito no delta são as gramáticas do nome da cópia, o veredito de topo versus andável e a projeção incompleta das autoridades. A duplicação do veredito do selo aparece no #11. `CLAUDE.md:258`, `bin/mao_unica.py:141`, `bin/disjuntor.py:1840`, `bin/codex_pergunta.sh:461`, `bin/hook_stop_fila1.py:236`, `bin/hook_stop_fila1.py:261`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:311`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:326`
- **Escritor, relógio e trava.** O TSV tem um escritor de produção pela transação. A foto do HANDOFF tem dois sítios de escrita no mesmo script, com a ambiguidade de vazio descrita acima. O delta acrescenta zero leituras de relógio em produção. O relógio do CLI já estava na borda e o prazo novo é do `flock`. Na ordem inspecionada, o orçamento solta a trava antes da borda, e a borda já recebe os recados do disjuntor. Ciclo inverso **[nao verificado]**. `bin/hook_stop_fila1.py:940`, `bin/handoff_sessao.sh:26`, `bin/handoff_sessao.sh:61`, `bin/disjuntor.py:2824`, `bin/hook_stop_fila1.py:909`, `bin/hook_stop_fila1.py:1118`, `bin/hook_stop_fila1.py:1642`, `bin/hook_stop_fila1.py:1664`
- **`except` novo de código: zero.** As duas linhas adicionadas são os textos da âncora e da substituição de `handoff-proximo-rele`. O `except` executável do HANDOFF é preexistente. `bin/tests/test_hook_stop_borda.sh:362`, `bin/tests/test_hook_stop_borda.sh:364`, `bin/handoff_sessao.sh:334`
- **`test_disjuntor.sh`: procede.** O brief declara o arquivo. O delta só atualiza a âncora e a substituição para `(0,0)`, preservando a asserção de merge recusado se a liberação falhar ao gravar. Apliquei a mutação em memória: o controle deu `PARADA` e o mutante deu `LIVRE`. Os arquivos fora da lista literal do brief são `bin/handoff_sessao.sh` e `bin/tests/test_relato_guarda_pedido_de_patch.sh`. Eles correspondem às curas pedidas na triagem. A autorização formal de ampliar `ARQUIVOS` fica **[nao verificado]**. `brief_f1.md:2`, `triagem.md:2`, `triagem.md:3`, `bin/tests/test_disjuntor.sh:852`, `bin/tests/test_disjuntor.sh:1821`, `bin/disjuntor.py:1056`, `bin/handoff_sessao.sh:25`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:297`
- **Fila e descarte.** #6 fica no parser isolado de `R100`, fora da via que usa `--no-renames`. #7 fica no critério que une `HEAD...MERGE_HEAD` ao delta da árvore a gravar. #14 permanece descartado pela triagem. `bin/mao_unica.py:165`, `bin/mao_unica.py:172`, `bin/mao_unica.py:195`, `triagem.md:4`, `triagem.md:5`

**REPROVA — bloqueantes nº 1, 2, 3, 4, 5, 8, 9, 11, 12 e 13. Fila nº 6 e 7.** `CLAUDE.md:428`, `triagem.md:4`

## 28/11 05:59 · sessao 6c06e82e37fb · CRON-15-SOMBRA-CABE · leitura · origem=Code

# CRON-15-SOMBRA-CABE · Passo 1 · Medida por etapa

**Só a soma dos comandos já excede o corredor com a folga do aval.** Na janela de 15 a 28/11, o p95 do bloco medido é **5.095 s (84min55s)**. Aplicando ×1,5, dá **127min22,5s**, **72min22,5s acima dos 55 min**. O p95 de ponta a ponta é **nao medido [nao verificado]**. O cronômetro do histórico começa depois da reposição da lavra de cada comando e cobre apenas a execução e a classificação do retorno. `hist.tsv:1445–2507`, `bin/crons_duracao.py:46–51`, `bin/sombra.sh:378–398`.

## 1. Duração diária do bloco

Somei a coluna `segundos`, incluindo os retornos de alarme. A linha `pulado` entra na contagem de linhas e soma 0 s. As referências dos anexos contam a primeira linha de dados como linha 1. O histórico guarda um arquivo por data, que pode ser sobrescrito por outra corrida no mesmo dia. `hist.tsv:1–2507`, `bin/sombra.sh:355–357,380–398`.

| Dia | Linhas | Soma (s) | min:s | Fonte |
|---|---:|---:|---:|---|
| 28/10 | 79 | 2865 | 47:45 | `hist.tsv:1–79` |
| 29/10 | 79 | 3022 | 50:22 | `hist.tsv:80–158` |
| 30/10 | 80 | 3055 | 50:55 | `hist.tsv:159–238` |
| 31/10 | 80 | 3260 | 54:20 | `hist.tsv:239–318` |
| 01/11 | 80 | 3245 | 54:05 | `hist.tsv:319–398` |
| 02/11 | 80 | 3424 | 57:04 | `hist.tsv:399–478` |
| 03/11 | 80 | 3212 | 53:32 | `hist.tsv:479–558` |
| 04/11 | 80 | 3239 | 53:59 | `hist.tsv:559–638` |
| 05/11 | 80 | 2902 | 48:22 | `hist.tsv:639–718` |
| 06/11 | 80 | 2720 | 45:20 | `hist.tsv:719–798` |
| 07/11 | 80 | 2699 | 44:59 | `hist.tsv:799–878` |
| 08/11 | 80 | 2728 | 45:28 | `hist.tsv:879–958` |
| **09/11** | 81 | **3441** | **57:21** | `hist.tsv:959–1039` |
| 10/11 | 81 | 3217 | 53:37 | `hist.tsv:1040–1120` |
| 11/11 | 81 | 3388 | 56:28 | `hist.tsv:1121–1201` |
| 12/11 | 81 | 3281 | 54:41 | `hist.tsv:1202–1282` |
| 13/11 | 81 | 3268 | 54:28 | `hist.tsv:1283–1363` |
| 14/11 | 81 | 3211 | 53:31 | `hist.tsv:1364–1444` |
| 15/11 | 81 | 3360 | 56:00 | `hist.tsv:1445–1525` |
| 16/11 | 81 | 3293 | 54:53 | `hist.tsv:1526–1606` |
| 17/11 | 81 | 3504 | 58:24 | `hist.tsv:1607–1687` |
| **18/11** | 82 | **4161** | **69:21** | `hist.tsv:1688–1769` |
| 19/11 | 82 | 4244 | 70:44 | `hist.tsv:1770–1851` |
| 20/11 | — | nao medido | — | `[nao verificado]` |
| 21/11 | 82 | 3632 | 60:32 | `hist.tsv:1852–1933` |
| **22/11** | 82 | **4515** | **75:15** | `hist.tsv:1934–2015` |
| 23/11 | 82 | 4546 | 75:46 | `hist.tsv:2016–2097` |
| 24/11 | 82 | 4815 | 80:15 | `hist.tsv:2098–2179` |
| 25/11 | 82 | 4882 | 81:22 | `hist.tsv:2180–2261` |
| 26/11 | 82 | 4797 | 79:57 | `hist.tsv:2262–2343` |
| 27/11 | 82 | 5145 | 85:45 | `hist.tsv:2344–2425` |
| 28/11 | 82 | 5095 | 84:55 | `hist.tsv:2426–2507` |

O primeiro salto sustentado de novembro começa **09/11**, com **+713 s** sobre 08/11. `lavrar_cxr_do_motor --apply` aparece nesse dia com **549 s**. Em **18/11**, o bloco sobe mais **657 s** sobre 17/11, com a entrada de `selo_leitores_no_mesmo_numero` a **830 s**. Em **22/11**, sobe **883 s** sobre 21/11, dos quais **646 s** vêm do aumento do selo e **175 s** do CXR. `hist.tsv:879–1039,995,1607–1769,1726,1852–2015`.

Em 28/11, os dois comandos que entraram depois de 01/11 somam **1.808 s** dos **1.850 s** de aumento do bloco entre as duas datas. O saldo das demais entradas é **+42 s**, incluindo mudanças de argumentos. `hist.tsv:319–398,2426–2507,2462,2464`.

## 2. Medida por comando

Usei o **comando completo, com argumentos**, como unidade. Os sete valores de cada linha correspondem a **22, 23, 24, 25, 26, 27 e 28/11**. Para os percentis, usei **15–28/11**, com 13 datas observadas devido à lacuna de 20/11. O p50 é a mediana e o p95 segue a função da cópia, que seleciona o índice `round(0.95 × (n−1))` da série ordenada, começando em zero. `n` conta execuções medidas. `Δ` é 28/11 menos 01/11 para os mesmos argumentos. `—` indica **nao medido [nao verificado]**. Os zeros de `pipeline_placar` são registros de `pulado`. `hist.tsv:319–398,1445–2507`, `bin/crons_duracao.py:46–51`, `bin/sombra.sh:355–357`.

### Top 10 por p95 e por crescimento

Valores em segundos. O ranking de crescimento usa as variantes com medida nas duas pontas. As fontes de cada comando estão na tabela detalhada. `hist.tsv:319–398,1445–2507`.

| Posição | Por p95 | p95 | Por crescimento | Δ |
|---:|---|---:|---|---:|
| 1 | `selo_leitores_no_mesmo_numero --rollout --origem cron` | 1222 | `esmeril_espelho --apply` | +151 |
| 2 | `lavrar_he_pendente --apply` | 670 | `processar_cartorio --empresa 2 --apply` | +102 |
| 3 | `lavrar_cxr_do_motor --apply` | 634 | `auditar_invariantes_chamados --html` | +55 |
| 4 | `processar_cartorio --empresa 2 --apply` | 497 | `pre_fechamento --apply` | +45 |
| 5 | `auditar_invariantes_chamados --html` | 334 | `gerar_celulas` | +19 |
| 6 | `esmeril_espelho --apply` | 333 | `processar_cartorio --empresa 3 --apply` | +14 |
| 7 | `vigiar_fio_morto --alarme --alarme-passivo` | 168 | `reconciliar_grade --empresa 2,3,4` | +14 |
| 8 | `calcular_scores` | 147 | `censo_fase_12x36 --min-plantoes 6 --lavrar` | +9 |
| 9 | `processar_cartorio --empresa 3 --apply` | 95 | `fechar_becos_disputa --alarme` | +6 |
| 10 | `reconciliar_grade --empresa 2,3,4` | 90 | `supra_juiz --empresa 2 --encerrada --alarme` | +6 |

Da primeira observação à última, o custo do CXR passa de **549 para 634 s (+85 s)** e o selo de leitores de **830 para 1.174 s (+344 s)**. A base de 01/11 para ambos é **nao medido [nao verificado]**. `hist.tsv:319–398,995,1726,2462,2464`.

### Série por comando em segundos

A última coluna indica a última ocorrência da variante em `hist.tsv`. Os percentis e a base usam as janelas indicadas acima. `hist.tsv:1–2507`.

| Comando | 22–28/11 | n | p50 | p95 | 01/11 | Δ | Linha |
|---|---|---:|---:|---:|---:|---:|---:|
| `calcular_metricas` | 17 17 18 18 17 18 17 | 13 | 18 | 18 | 18 | −1 | 2426 |
| `calcular_scores` | 111 130 147 149 141 147 141 | 13 | 114 | 147 | 190 | −49 | 2427 |
| `pre_fechamento --apply` | 68 66 70 72 66 72 70 | 13 | 66 | 72 | 25 | +45 | 2428 |
| `health_crawl` | 3 2 2 2 2 3 2 | 13 | 2 | 3 | 2 | 0 | 2429 |
| `lavrar_foto_folgas --apply` | 3 3 3 3 3 3 2 | 13 | 3 | 3 | 3 | −1 | 2430 |
| `gerar_celulas` | 37 36 37 37 34 35 36 | 13 | 34 | 37 | 17 | +19 | 2431 |
| `termometro_regua --avaliar --lavrar` | 79 78 79 81 78 83 80 | 13 | 79 | 82 | 75 | +5 | 2432 |
| `tripwire_celulas` | 19 19 19 18 18 18 16 | 13 | 18 | 19 | 16 | 0 | 2433 |
| `efetivar_desligamentos_agendados --apply` | 1 1 1 2 1 1 2 | 13 | 1 | 2 | 2 | 0 | 2434 |
| `tripwire_vinculo_orfao --alarme` | 2 2 2 1 2 2 1 | 13 | 2 | 2 | 1 | 0 | 2435 |
| `sincronizar_status_ferias --apply` | 2 1 2 2 1 2 2 | 13 | 2 | 2 | 1 | +1 | 2436 |
| `reverter_situacao_afastado --apply` | 1 2 1 1 2 2 1 | 13 | 1 | 2 | 2 | −1 | 2437 |
| `tripwire_status_ferias --alarme` | 2 2 2 2 1 2 2 | 13 | 2 | 2 | 1 | +1 | 2438 |
| `tripwire_situacao_afastado --alarme` | 2 1 2 2 2 2 2 | 13 | 2 | 2 | 2 | 0 | 2439 |
| `reconciliar_grade --empresa 2,3,4` | 87 86 90 88 82 91 84 | 13 | 83 | 90 | 70 | +14 | 2440 |
| `detectar_cluster_espurio --alarme` | 4 3 4 5 4 4 4 | 13 | 4 | 4 | 4 | 0 | 2441 |
| `detectar_par_relampago --dias 2 --apply --retratar` | 19 20 21 21 20 — — | 11 | 20 | 21 | 17 | — | 2278 |
| `detectar_par_relampago --dias 2 --apply` | — — — — — 15 15 | 2 | 15 | 15 | — | — | 2442 |
| `processar_cartorio --empresa 2 --apply` | 473 482 497 488 481 487 508 | 13 | 481 | 497 | 406 | +102 | 2443 |
| `processar_cartorio --empresa 3 --apply` | 89 91 95 92 92 96 95 | 13 | 92 | 95 | 81 | +14 | 2444 |
| `processar_cartorio --empresa 4 --apply` | 19 19 20 20 19 19 20 | 13 | 19 | 20 | 16 | +4 | 2445 |
| `apurar_furos_diarios` | 22 22 21 22 23 24 23 | 13 | 23 | 30 | 31 | −8 | 2446 |
| `disparar_perguntas_competencia --comp-corrente --apply` | — — — — — — — | 0 | — | — | 37 | — | 1061 |
| `disparar_perguntas_competencia --comp-corrente --alarme` | 6 6 8 14 15 17 17 | 13 | 17 | 38 | — | — | 2447 |
| `reconciliar_perguntas_orfas --apply` | — — — — — — — | 0 | — | — | — | — | 102 |
| `reconciliar_perguntas_orfas --alarme` | 2 3 4 3 4 3 4 | 13 | 3 | 4 | 2 | +2 | 2448 |
| `lavrar_deslogados --apply` | 7 7 6 6 6 7 7 | 13 | 6 | 7 | 5 | +2 | 2449 |
| `lavrar_he_pendente --apply` | 565 570 594 604 586 676 670 | 13 | 565 | 670 | 846 | −176 | 2450 |
| `recusar_ponta_pequena --apply` | 3 3 4 — — — — | 9 | 5 | 8 | 4 | — | 2123 |
| `recusar_ponta_pequena --alarme` | — — — 27 23 24 14 | 4 | 23,5 | 27 | — | — | 2451 |
| `vigiar_txt_dominio --alarme` | 15 16 15 15 15 15 15 | 13 | 15 | 16 | 12 | +3 | 2452 |
| `lavrar_retrato_folha --apply` | 43 42 45 45 45 47 46 | 13 | 45 | 47 | 87 | −41 | 2453 |
| `vigiar_pausa_export --alarme` | 9 10 10 11 11 10 10 | 13 | 11 | 11 | 11 | −1 | 2454 |
| `vigiar_retido_calado --alarme` | 2 1 2 1 1 2 2 | 13 | 2 | 2 | 2 | 0 | 2455 |
| `vigiar_fio_morto --alarme --alarme-passivo` | 129 130 156 160 161 170 168 | 13 | 151 | 168 | 261 | −93 | 2456 |
| `reconciliar_geofence --empresa 2 --alarme` | 2 1 1 1 2 1 2 | 13 | 1 | 2 | 1 | +1 | 2457 |
| `vigiar_carimbo_sem_registro --alarme` | 42 42 45 44 45 46 45 | 13 | 45 | 47 | 42 | +3 | 2458 |
| `vigiar_ausencia_x_veredito --alarme` | 7 7 7 8 7 7 7 | 13 | 7 | 7 | 6 | +1 | 2459 |
| `reconciliar_geofence --empresa 3 --alarme` | 2 2 1 1 2 2 2 | 13 | 2 | 2 | 1 | +1 | 2460 |
| `vigia_pareador_celula --alarme` | 2 3 3 3 3 3 3 | 13 | 3 | 5 | 3 | 0 | 2461 |
| `lavrar_cxr_do_motor --apply` | 470 508 548 562 553 634 634 | 13 | 516 | 634 | — | — | 2462 |
| `reconciliar_geofence --empresa 4 --alarme` | 2 2 1 2 2 2 2 | 13 | 2 | 2 | 2 | 0 | 2463 |
| `selo_leitores_no_mesmo_numero --rollout --origem cron` | 1129 1114 1160 1157 1135 1222 1174 | 10 | 1132 | 1222 | — | — | 2464 |
| `reconciliar_vinculo --alarme` | 2 2 1 2 2 2 2 | 13 | 2 | 2 | 1 | +1 | 2465 |
| `detectar_intervalo_ausente --alarme` | 4 3 4 4 4 4 4 | 13 | 4 | 4 | 4 | 0 | 2466 |
| `verificar_nucleo` | 15 15 16 16 15 15 15 | 13 | 15 | 16 | 14 | +1 | 2467 |
| `supra_juiz --empresa 2 --executar` | 26 27 32 32 33 35 35 | 13 | 35 | 42 | 48 | −13 | 2468 |
| `supra_juiz --empresa 3 --executar` | 7 8 8 9 9 9 9 | 13 | 9 | 11 | 13 | −4 | 2469 |
| `supra_juiz --empresa 4 --executar` | 3 2 3 3 3 4 3 | 13 | 3 | 4 | 4 | −1 | 2470 |
| `reavaliar_ausencias_feriado --empresa 2 --apply` | — — — — — — — | 0 | — | — | 18 | — | 682 |
| `reavaliar_ausencias_feriado --empresa 2 --alarme` | 22 23 23 23 23 23 24 | 13 | 23 | 24 | — | — | 2471 |
| `reavaliar_ausencias_feriado --empresa 3 --apply` | — — — — — — — | 0 | — | — | 5 | — | 683 |
| `reavaliar_ausencias_feriado --empresa 3 --alarme` | 7 6 7 7 6 7 6 | 13 | 6 | 7 | — | — | 2472 |
| `reavaliar_ausencias_feriado --empresa 4 --apply` | — — — — — — — | 0 | — | — | 3 | — | 684 |
| `reavaliar_ausencias_feriado --empresa 4 --alarme` | 2 3 2 2 3 2 3 | 13 | 3 | 3 | — | — | 2473 |
| `silenciar_chamados_isentos --alarme` | 2 1 1 1 1 1 1 | 13 | 1 | 1 | 1 | 0 | 2474 |
| `mapa_divergencia --mes=$(date +\%-m) --ano=$(date +\%Y)` | 70 69 72 70 69 73 72 | 13 | 70 | 73 | 74 | −2 | 2475 |
| `fechar_becos_disputa --alarme` | 17 18 20 20 20 21 20 | 13 | 18 | 20 | 14 | +6 | 2476 |
| `flip_automatico --competencia --apply` | 3 3 3 10 6 6 7 | 13 | 7 | 12 | 9 | −2 | 2477 |
| `lavrar_fase_conflitante --apply` | 2 1 2 2 2 2 2 | 13 | 2 | 2 | 1 | +1 | 2478 |
| `lavrar_vigencia_sem_trilha --apply` | 1 2 1 1 1 2 2 | 13 | 2 | 2 | 2 | 0 | 2479 |
| `lavrar_vigencia_impossivel --apply` | 2 2 2 2 2 2 1 | 13 | 2 | 2 | 1 | 0 | 2480 |
| `auditar_invariantes_chamados --html` | 295 291 317 313 316 330 326 | 13 | 313 | 334 | 271 | +55 | 2481 |
| `vigia_b5_ausencia` | 17 17 17 18 18 18 18 | 13 | 17 | 18 | 14 | +4 | 2482 |
| `supra_juiz --empresa 2 --encerrada --alarme` | 42 42 44 45 43 43 43 | 13 | 42 | 45 | 37 | +6 | 2483 |
| `tripwire_tipo_batida --dias 60 --limiar 60` | 16 15 17 16 15 16 16 | 13 | 16 | 17 | 16 | 0 | 2484 |
| `marcar_foto_ausente_retro --apply` | 2 2 3 2 3 3 3 | 13 | 3 | 3 | 2 | +1 | 2485 |
| `supra_juiz --empresa 3 --encerrada --alarme` | 12 11 12 12 12 12 12 | 13 | 12 | 12 | 11 | +1 | 2486 |
| `vigia_plantio_overnight --dias=45` | 53 49 50 54 58 55 58 | 13 | 55 | 58 | 62 | −4 | 2487 |
| `supra_juiz --empresa 4 --encerrada --alarme` | 3 4 4 4 4 4 3 | 13 | 3 | 4 | 3 | 0 | 2488 |
| `lavrar_jornada_lixo --apply` | 3 3 3 3 3 3 3 | 13 | 3 | 3 | 3 | 0 | 2489 |
| `lavrar_previsto_cego --apply` | 3 4 4 4 4 4 5 | 13 | 4 | 6 | 4 | +1 | 2490 |
| `supra_juiz --empresa 2 --alarme` | 27 27 32 33 33 34 36 | 13 | 34 | 41 | 47 | −11 | 2491 |
| `supra_juiz --empresa 3 --alarme` | 7 7 8 8 9 9 9 | 13 | 9 | 11 | 13 | −4 | 2492 |
| `lavrar_furo_sem_canal --apply` | 3 3 4 4 3 4 4 | 13 | 4 | 9 | 1 | +3 | 2493 |
| `supra_juiz --empresa 4 --alarme` | 3 3 3 3 3 3 3 | 13 | 3 | 4 | 4 | −1 | 2494 |
| `censo_fase_12x36 --min-plantoes 6 --lavrar` | 24 18 18 25 25 18 26 | 13 | 20 | 25 | 17 | +9 | 2495 |
| `pipeline_placar --alarme` | 0 0 0 0 0 0 0 | 0 | — | — | — | — | 2496 |
| `lavrar_teto_do_dia --apply` | 15 15 15 16 16 16 16 | 13 | 16 | 19 | 15 | +1 | 2497 |
| `alertar_justificativas_sla` | 1 1 2 2 1 1 2 | 13 | 1 | 2 | 2 | 0 | 2498 |
| `alertar_retornos` | 3 2 3 2 2 3 2 | 13 | 2 | 3 | 2 | 0 | 2499 |
| `alertar_ferias_vencendo` | 2 2 2 3 3 3 3 | 13 | 3 | 3 | 2 | +1 | 2500 |
| `pautas_de_ferias --apply` | 2 2 2 1 1 1 2 | 13 | 2 | 2 | 2 | 0 | 2501 |
| `vigia_entrada_faltante` | 2 2 2 3 2 2 2 | 13 | 2 | 3 | 2 | 0 | 2502 |
| `escalonar_documentos_ausencia --apply` | 2 2 1 1 2 2 1 | 13 | 2 | 2 | 1 | +1 | 2503 |
| `alertar_disputas_pendentes` | 2 2 3 3 3 3 2 | 13 | 3 | 4 | 3 | −1 | 2504 |
| `suporte_hasner --fila` | 2 1 2 2 1 2 2 | 13 | 2 | 2 | 1 | +1 | 2505 |
| `esmeril_espelho --apply` | 291 285 306 309 311 333 345 | 13 | 296 | 333 | 194 | +151 | 2506 |
| `pautas_do_esmeril` | 1 1 1 2 2 1 2 | 13 | 2 | 2 | 2 | 0 | 2507 |

## 3. Fases fora dos comandos cronometrados

A cópia agenda o **backup das 04:00** em uma entrada própria. A entrada da sombra chama **`--refazer && --bloco`**. No caminho diário, `--refazer` garante o banco da sombra, escolhe o dump mais recente por mtime de cada schema, recria o banco, restaura `public` e depois `juliani`, roda máscara 1, máscara 2 e selo pessoal. Em seguida, compara colunas e migrations, verifica a completude e carimba a sombra refeita. `app/config/crons.py:282,350–352`, `bin/sombra.sh:236–305,311–325`.

| Fase | O que o código faz | Medida e ponto de coleta |
|---|---|---|
| Dump | O backup percorre schemas em série. Cada `pg_dump` grava `.part`, o script renomeia o arquivo e valida o catálogo. O caminho opcional `--dump-agora` faz `juliani` e depois `public` em primeiro plano, em arquivos separados. `scripts/backup_all.sh:5–8`, `scripts/backup_tenant.sh:9–18`, `bin/sombra.sh:239–253` | **nao medido [nao verificado]**. O nome é criado antes do `pg_dump`. Mediria início e fim por schema, mais a validação se a fronteira for o dump pronto para restore. `sombra_tail.log:5,19,33,47`, `scripts/backup_tenant.sh:9–16`, `bin/sombra.sh:244–245` |
| Restore | `dropdb` → `createdb` → `pg_restore public` → `pg_restore juliani`, no mesmo banco, em primeiro plano e sem `-j`. Depois conta erros de restore fora da exceção de schema `public` já existente. `bin/sombra.sh:255–264` | **nao medido [nao verificado]**. O tail registra `public rc=1`, `juliani rc=0` e **0** erros não aceitos nos quatro dias. Mediria antes e depois de cada restore e também o trecho `dropdb`/`createdb`. `sombra_tail.log:6,20,34,48`, `bin/sombra.sh:255–260` |
| Máscara 1 | Em uma transação, usa valores temporários antes de gravar CPF e login sintéticos, anonimiza nomes e e-mails, neutraliza push e mascara o nome nos registros de PDF. Confere unicidade e os temporários ao final. `bin/sombra_mascara.py:38–39,51–103` | **nao medido [nao verificado]**. O tail registra contagens e `MASCARA1 ok`. Mediria a chamada completa do container. `sombra_tail.log:7–8,21–22,35–36,49–50`, `bin/sombra.sh:266–268` |
| Máscara 2 | Em outra transação, aplica regras de vazio, nulo, marcador, cifrado, zero, ano de nascimento e mascaramento de texto e JSON. O timer interno começa após os imports e termina após a transação. `bin/sombra_mascara_pessoal.py:6–66`, `bin/sombra_regras_pessoais.py:29–157` | **25/11 267 s · 26/11 259 s · 27/11 268 s · 28/11 261 s**. A duração de parede da chamada completa é **nao medido [nao verificado]**. Mediria em volta do `container` para incluir a inicialização. `sombra_tail.log:9,23,37,51`, `bin/sombra.sh:272–274` |
| Selo pessoal | Consulta o catálogo e confronta colunas e conteúdo com as regras do inventário. Se encontra pendência, sai com código 3 e `--refazer` falha. `bin/sombra_selo_pessoal.py:23–97`, `bin/sombra.sh:275–277` | **nao medido [nao verificado]**. Nos quatro dias, o tail registra **154/154** colunas inventariadas, **66** isentas e `OK`. Mediria em volta da chamada completa. `sombra_tail.log:10–11,24–25,38–39,52–53`, `bin/sombra.sh:275–277` |

**Serialização e independência pelo código.** A sequência do `--refazer` é síncrona. Os dois dumps opcionais leem a origem e escrevem arquivos distintos, sem consumir a saída um do outro. Os restores usam o mesmo banco e a independência entre os objetos dos dois arquivos fica **[nao verificado]**. As máscaras escrevem em tabelas comuns, como `Colaborador` e `RegistroPDFAuditavel`, e o selo de conteúdo lê o estado mascarado. Depois dos commits, as consultas de catálogo, migrations e contagens em prod e sombra fornecem resultados independentes até a comparação. As verificações de conteúdo do selo também são leituras independentes entre si até a consolidação. Todas rodam em série na cópia. `bin/sombra.sh:239–305,313–325`, `bin/sombra_mascara.py:64–88`, `bin/sombra_regras_pessoais.py:29–91`, `bin/sombra_selo_pessoal.py:23–96`.

O `ts` de `espera.jsonl` é emitido após adquirir a trava ou atingir o timeout. Nos pares diários abaixo, ambas as esperas são **0 s**. A diferença entre os `ts` mede o intervalo **entre as duas travas**. Esse intervalo inclui o `--refazer`, suas verificações e a transição até `--bloco`. `bin/sombra_trava.sh:21–35`, `bin/sombra.sh:437–451`.

| Dia | Trava `--refazer` | Trava `--bloco` | Intervalo (s) | Fonte |
|---|---|---|---:|---|
| 21/11 | 04:11:01 | 04:18:14 | 433 | `espera.jsonl:69–70` |
| 22/11 | 04:11:01 | 04:17:00 | 359 | `espera.jsonl:96–97` |
| 23/11 | 04:11:01 | 04:16:46 | 345 | `espera.jsonl:117–118` |
| 24/11 | 04:11:01 | 04:16:55 | 354 | `espera.jsonl:146–147` |
| 25/11 | 04:11:01 | 04:17:00 | 359 | `espera.jsonl:282–283` |
| 26/11 | 04:11:01 | 04:16:58 | 357 | `espera.jsonl:458–459` |
| 27/11 | 04:11:01 | 04:17:08 | 367 | `espera.jsonl:492–493` |
| 28/11 | 04:11:01 | 04:16:58 | 357 | `espera.jsonl:521–522` |

Em 25–28/11, descontando o timer interno da máscara 2, sobram **92, 98, 99 e 96 s** no intervalo entre travas para **todas** as demais tarefas e a transição. Em 20/11, o `--refazer` adquiriu a trava após **1.650 s** e a tentativa registrada de `--bloco` atingiu o timeout de **1.800 s** com `ok=0`. `espera.jsonl:16,19,282–283,458–459,492–493,521–522`, `sombra_tail.log:9,23,37,51`, `bin/sombra_trava.sh:26–35`.

## 4. Ordem do bloco e dependências de dado

**São 82 entradas e 81 invocações ensaiadas em série.** `--bloco` pega a trava da sombra, importa a lavra de prod e chama `manage.py crons sombra` no container do ensaio. O renderer filtra comandos diários de horário fixo, expande os que rodam por empresa, insere cada encadeado no último minuto do seu `gatilho` mais um e ordena por minuto, preservando a ordem de inserção nos empates. O `while` usa essa lista em primeiro plano, sem esperar os horários exibidos. Registra `pipeline_placar` como `pulado` porque o comando usa o placar do host. Cada uma das 81 invocações tem uma reposição da lavra antes de `t0`, além da importação inicial. `sombra_tail.log:44–46,58–60`, `bin/sombra.sh:120–125,328–386,451`, `bin/sombra_trava.sh:21–38`, `app/core/management/commands/crons.py:43–48`, `app/config/crons.py:17,1946–1977`.

No crontab que a cópia renderiza, `encadeado.sh --corrente` percorre a corrente em primeiro plano. Cada elo usa `flock` e aguarda os carimbos do gatilho p

[truncado em 20000 de 27319 caracteres]

## 28/11 05:57 · sessao d84a35fe1893 · - · - · origem=Ronald (tmux)

O `--refazer` diário consome os dumps prontos das 04:00. O backup percorre os schemas em série, grava cada dump em `.part`, renomeia e valida o arquivo. O horário no nome vem antes do `pg_dump`. O ramo opcional `--dump-agora` exporta `juliani` e depois `public` em chamadas bloqueantes, com saídas independentes no shell. Duração dos dumps não medida. O ponto de medição é cada `pg_dump`. `app/config/crons.py:282,350-352`, `scripts/backup_all.sh:5-8`, `scripts/backup_tenant.sh:9-18`, `bin/sombra.sh:236-253`.

| Ordem após selecionar os dumps | Implementação | Tempo |
|---|---|---|
| Restore | Recria o banco da sombra e restaura `public` antes de `juliani`. Valida os erros dos dois logs. | Não medido. Medir antes e depois de `dropdb`, `createdb` e cada `pg_restore`. `bin/sombra.sh:255-264`. |
| Máscara 1 | Em uma transação, troca CPF e login para temporários antes dos definitivos, mascara nomes e e-mails, neutraliza push e trata nomes de PDFs. Verifica unicidade e temporários restantes. | Não medido. Medir o contêiner. `bin/sombra.sh:266-268`, `bin/sombra_mascara.py:51-103`. |
| Máscara 2 | Em outra transação aplica inventário de campos pessoais, incluindo caminhos, cifrados, ano de nascimento, texto e JSON. Usa lotes para atualizações. | 25 a 28/11, **267, 259, 268 e 261 s** internos, sem o arranque do contêiner e do Django. `bin/sombra.sh:272-274`, `bin/sombra_mascara_pessoal.py:14-66`, `sombra_tail.log:9,23,37,51`. |
| Selo pessoal | Consulta catálogo e valores conforme o inventário. Campo pessoal fora dele ou conteúdo cru provoca saída 3 e falha do `--refazer`. | Não medido. Medir o contêiner. `bin/sombra.sh:275-277`, `bin/sombra_selo_pessoal.py:23-97`. |
| Divergência e carimbo | Compara colunas e migrations, verifica completude e grava o estado. | Não medido. Medir o conjunto e as consultas. `bin/sombra.sh:286-305,311-325`. |

Os restores escrevem o mesmo banco. A independência entre seus objetos é [nao verificado]. As duas máscaras escrevem nas mesmas tabelas, incluindo `Colaborador` e `RegistroPDFAuditavel`. O selo verifica os dados após a máscara 2 e isenta campos tratados pela máscara 1. As leituras de colunas, migrations e contagens não consomem a saída umas das outras, mas sua consolidação e o carimbo precisam dos resultados. `bin/sombra.sh:255-277,292-305,311-325`, `bin/sombra_mascara.py:66-73,86-88`, `bin/sombra_regras_pessoais.py:50-52,73,80,165-171`.

Intervalo da aquisição da trava de `--refazer` até a aquisição de `--bloco` em 21–28/11, **433, 359, 345, 354, 359, 357, 367 e 357 s**, com **0 s** em ambas as esperas. As sobras após a duração interna da máscara 2 em 25–28/11 foram **92, 98, 99 e 96 s** para o restante do intervalo inteiro. Em 20/11 a trava do `--refazer` veio após **1.650 s**. O `--bloco` registrou timeout após **1.800 s**. `bin/sombra_trava.sh:26-35`, `espera.jsonl:16,19,69-70,96-97,117-118,146-147,282-283,458-459,492-493,521-522`.

Na janela 15–28/11 com 13 dias observados, o p95 das somas do bloco é **5.095 s**, pelo critério de arredondamento do índice da cópia. Com margem dá **7.642,5 s**, **4.342,5 s acima de 55 min**. Somar o intervalo entre travas nos oito dias em que existe dá limite parcial conservador para a janela, **5.452 s** de p95 parcial e **8.178 s** após a margem, **4.878 s acima de 55 min**. O alvo equivale a p95 bruto **≤ 2.200 s**. O p95 recente de ponta a ponta é **não medido** no anexo. O cron registra início, fim e duração no envelope de `placar.jsonl`. `hist.tsv:1445-2507`, `espera.jsonl:69-70,96-97,117-118,146-147,282-283,458-459,492-493,521-522`, `bin/crons_duracao.py:46-51`, `bin/cron_run.sh:5-13`, `app/config/crons.py:350-352`.

A cópia guarda **5.188 s já com margem**, carimbados em **31/10**, para janela declarada de sete dias. Equivalem a **86m28s**, **31m28s acima de 55 min**. O medidor preserva um valor anterior quando não tem amostra nova. `app/config/crons_duracao.json:78,106-110`, `bin/crons_duracao.py:55-76,83-96`.

## 28/11 05:55 · sessao a867fa742add · - · - · origem=Ronald (tmux)

A cópia gera **82 entradas**. O bloco percorre a lista em série, executa **81 comandos** em primeiro plano e registra `pipeline_placar` como pulado. Cada execução é precedida de uma reposição da lavra, que fica fora do cronômetro dos comandos. O render ordena pelo minuto, expande por empresa e usa `gatilho` para inserir encadeados. O campo `depende` é verificado nos contratos de agenda. `bin/sombra.sh:339–386,451`, `app/core/management/commands/crons.py:43–48`, `app/config/crons.py:1946–1977`, `app/chamados/tests/test_contract_crons.py:269–298`.

As dependências funcionais verificadas incluem:

- `lavrar_foto_folgas` grava `FolgaDia`, que o cálculo usado por `gerar_celulas` lê. `tripwire_celulas`, `reconciliar_grade` e `processar_cartorio` consultam as células. `reconciliar_grade` também lê o veto da régua gravada por `termometro_regua --avaliar`. `app/escala/folgas.py:194`, `app/escala/models.py:1297–1321`, `app/ponto/portas/celula.py:947–953`, `app/ponto/management/commands/tripwire_celulas.py:49`, `app/ponto/management/commands/reconciliar_grade.py:125–126,498–502`, `app/ponto/management/commands/processar_cartorio.py:49–65`, `app/escala/management/commands/termometro_regua.py:41–44`, `app/escala/regua_defesa.py:557–565,769`.
- O cartório grava ata e veredito. `apurar_furos_diarios` lê a ata e grava `FuroDiario`, consumido por `lavrar_deslogados` e `auditar_invariantes_chamados`. O cartório também alimenta os leitores de veredito, como `supra_juiz --executar` e o retrato da folha. `app/ponto/services/cartorio.py:1905–1931`, `app/escala/services/furos_diarios.py:38–43,96–109`, `app/ponto/services/deslogados.py:94–103`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1332,1405`, `app/ponto/management/commands/supra_juiz.py:161–166,245–258`, `app/config/crons.py:1628–1636`.
- `lavrar_he_pendente` grava o retrato que `recusar_ponta_pequena --alarme` consulta. `lavrar_retrato_folha` grava `RetratoFolha`, lido por `vigiar_retido_calado`. O vigia da pausa roda após o retrato e consulta a porta sobre células vivas. `app/ponto/services/he_pendente_lavrado.py:183–210`, `app/ponto/management/commands/recusar_ponta_pequena.py:66–77,99–100`, `app/folha/management/commands/lavrar_retrato_folha.py:77–79`, `app/folha/vigia_retido_calado.py:52–59`, `app/folha/services/vigia_pausa.py:116–133`.
- `vigiar_fio_morto` lê o snapshot CXR mais recente já existente. A lavra CXR do bloco vem depois dele. O selo de leitores é disparado após essa lavra e faz sua própria medição pela porta do export. `app/chamados/management/commands/vigiar_fio_morto.py:184–191`, `app/chamados/services/vigia_fio.py:192–198`, `app/escala/services/cadastro_realidade.py:1225–1230`, `app/ponto/services/cxr_na_lista.py:142–156`, `app/config/crons.py:1746–1748,1805–1820`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:70–72`.
- `pautas_do_esmeril` lê a chave que `esmeril_espelho` grava. Na sombra, recebe a versão reposta de produção antes da sua execução. `app/ponto/services/esmeril_espelho.py:350,371`, `app/ponto/services/pautas_do_esmeril.py:57–58`, `bin/sombra.sh:364–379`.

Conferência independente das somas de 09/11, 18/11, 19/11 e 28/11 resultou em **3.441, 4.161, 4.244 e 5.095 s**. O primeiro salto de fim de patamar ocorre em 09/11 ante 08/11, **+720 s**, dos quais **549 s** do CXR que aparece naquele dia. `hist.tsv:879–1039,1688–1851,2426–2507`.

## 28/11 05:55 · sessao df20b9a593bb · - · - · origem=Ronald (tmux)

A cópia renderiza **82 entradas**. O loop invoca **81 comandos em série** e registra `pipeline_placar` como `pulado`, com 0 s. O `flock` protege a invocação inteira. O renderer filtra crons diários fixos, expande empresas, insere cada elo em `max(minuto do gatilho) + 1` e ordena por minuto. O shell percorre essa lista sem esperar pelos horários. `bin/sombra.sh:120-125`, `bin/sombra.sh:346-398`, `bin/sombra.sh:451`, `bin/sombra_trava.sh:21-38`, `app/core/management/commands/crons.py:43-48`, `app/config/crons.py:1947-1977`.

Em produção declarada, o renderer anexa `encadeado.sh --corrente` a cada disparo da raiz. O executor da corrente usa trava por elo, espera pelo carimbo das empresas pertinentes e aceita carimbo para rc 0 ou 2. Na sombra, a chamada direta a `tenant_command` usa a ordem da lista. `app/config/crons.py:1870-1890`, `app/config/crons.py:1908-1929`, `bin/encadeado.sh:45-68`, `bin/encadeado.sh:78-104`, `bin/cron_run.sh:22-26`, `bin/sombra.sh:380-386`.

Dependências de dados confirmadas:

- A foto grava `FolgaDia` e relavra células. O gerador lê a programação do vínculo e grava `CelulaDia`. `tripwire_celulas` lê as células geradas e `reconciliar_grade` lê lâmpadas do DNA. `app/escala/management/commands/lavrar_foto_folgas.py:164-170`, `app/escala/folgas.py:193-218`, `app/escala/models.py:1297-1321`, `app/ponto/portas/celula.py:947-953`, `app/ponto/management/commands/tripwire_celulas.py:12-31`, `app/ponto/management/commands/reconciliar_grade.py:123-126`.
- O cartório grava `veredito` e `ata` na célula. A apuração lê a grade da célula e grava `FuroDiario`; `lavrar_deslogados` lê `FuroDiario`, embora siga o gatilho `reconciliar_perguntas_orfas`. `app/ponto/management/commands/processar_cartorio.py:49-65`, `app/ponto/services/cartorio.py:1908`, `app/escala/services/furos_diarios.py:38-54`, `app/ponto/portas/celula.py:1278-1304`, `app/ponto/services/deslogados.py:94-109`, `app/config/crons.py:1566-1568`.
- `lavrar_he_pendente` lê o espelho e grava o retrato de HE. `recusar_ponta_pequena --alarme` lê esse retrato e exige cálculo do dia. `app/ponto/services/he_pendente_lavrado.py:80-81`, `app/ponto/management/commands/lavrar_he_pendente.py:62-72`, `app/ponto/management/commands/recusar_ponta_pequena.py:66-79`.
- `lavrar_retrato_folha` grava `RetratoFolha`, lido por `vigiar_retido_calado`. O gatilho desse vigia é `vigiar_pausa_export`, que lê diretamente `FechamentoMensal` e motivos de retenção de célula. `app/folha/management/commands/lavrar_retrato_folha.py:64-79`, `app/folha/vigia_retido_calado.py:52-77`, `app/config/crons.py:1689-1692`, `app/folha/services/vigia_pausa.py:100-143`.
- `lavrar_cxr_do_motor` é o gatilho do selo dos leitores. O selo mede empresa por empresa, lê classificação de exportação, espelho e fechamento. O `depende` do selo indica `processar_cartorio`. A leitura do snapshot CxR pelo selo ficou [nao verificado]. `app/config/crons.py:1818-1820`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:43-51`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:141`, `app/folha/porta_export.py:475-483`.
- `esmeril_espelho --apply` grava o snapshot que `pautas_do_esmeril` sabe ler. Na sombra, o loop repõe o conteúdo de produção antes de `pautas_do_esmeril`, e o leitor confere o hash. Cada reposição roda antes de `t0`, fora dos segundos do anexo. `app/ponto/services/esmeril_espelho.py:321-355`, `app/ponto/services/pautas_do_esmeril.py:52-61`, `app/ponto/services/esmeril_espelho.py:291-302`, `app/ponto/services/esmeril_espelho.py:359-375`, `bin/sombra.sh:141-163`, `bin/sombra.sh:378-385`.

O ponto de coleta de parede do cron inteiro é `ini`/`fim`/`dur` do envelope, que envolve `--refazer && --bloco`. O JSON versionado tem **5.188 s já multiplicados pela folga** em janela de 7 dias gerada em 31/10. `bin/cron_run.sh:7-13`, `app/config/crons.py:350-352`, `app/config/crons.py:1903-1906`, `app/config/crons_duracao.json:78`, `app/config/crons_duracao.json:106-109`, `bin/crons_duracao.py:55-76`.

## 28/11 04:51 · sessao 1feb474a2766 · CUSTO-DE-COORDENACAO · verificador · origem=Code

**REPROVA.** Uma falha na borda de avisos pode liberar um Stop antes da prova obrigatória do RELATO. O CENSO ainda aceita pergunta sem corte em formas válidas de Markdown, e o B3 pode classificar como mão dupla um merge que introduz uma autoridade de célula. `bin/hook_stop_fila1.py:1553`, `bin/hook_stop_fila1.py:1637`, `bin/disjuntor.py:1674`, `bin/mao_unica.py:29`, `bin/disjuntor.py:1302`

## Achados da volta 2

1. **Bloqueia · V1 #2 · O CENSO fecha os exemplos do selo.** `## CENSO` com referências e decisões válidas, seguido de `## Perguntas de lei` e `- Pode gravar?` → aprovado. O corte da seção descarta o título antes de o reconhecedor de perguntas vê-lo. Dentro da seção, `### Perguntas de lei` com `+ Pode gravar?` também passa. No outro sentido, `### **Perguntas de lei** ###` com uma pergunta já cortada → negado, porque o `###` final vira uma pergunta. Confirmei os três resultados em memória. `bin/disjuntor.py:1635`, `bin/disjuntor.py:1639`, `bin/disjuntor.py:1674`, `bin/disjuntor.py:1686`, `bin/tests/test_censo_antes_da_vaga.sh:104`

2. **Bloqueia · V1 #3 · A ambiguidade real fecha, mas o reconhecedor cria candidatos onde a forma Codex é inválida.** Com `OBRA-X` e `CODEX-OBRA-X` abertas, `/home/ronald/wt-codex-obra-x-1` → `None`. É uma cópia genérica de `CODEX-OBRA-X`. O wrapper exige seis dígitos para a leitura Codex, mas a inferência remove `codex-` sem essa validação. No outro sentido, `/home/ronald/wt-codex-obra-x-000001`, com `OBRA-X` e `OBRA-X-000001` abertas → `OBRA-X-000001`, tomando o número da cópia como parte da obra. Confirmei ambos em memória. Quando as duas gramáticas realmente dão obras distintas, `None` é a resposta restritiva prevista pela cura. `bin/codex_pergunta.sh:461`, `bin/disjuntor.py:1816`, `bin/disjuntor.py:1823`, `bin/disjuntor.py:2291`, `bin/tests/test_censo_antes_da_vaga.sh:730`

3. **Bloqueia · V1 #5 · O painel corrige o topo marcado em negrito e a borda percebe a troca de coluna.** `| ITEM-A | descrição | SUPERADA por ITEM-B | … |` → topo `ok`. Já `**ABERTA** (a anterior foi **SUPERADA**)` → topo `parado` e lista de andáveis `['ITEM-A']`. O topo reconhece a palavra em qualquer posição da coluna e a lista usa outro predicado. Na cópia, `ORDEM-VIVA-1511-NOITE` ainda resolve como `ok`. O marcador aponta para a linha antiga, que mantém `ABERTA`, embora a linha de 18/11 declare a substituição. A mesma impressão também gera aviso quando muda só uma nota ou data da linha. `bin/hook_stop_fila1.py:60`, `bin/hook_stop_fila1.py:230`, `bin/hook_stop_fila1.py:258`, `bin/hook_stop_fila1.py:1627`, `bin/handoff_sessao.sh:64`, `app/docs/BACKLOG.md:8`, `app/docs/BACKLOG.md:1926`, `app/docs/BACKLOG.md:2649`

4. **Bloqueia · V1 #5 · O B3 vê aumento de `max(n, nc)`.** Uma obra parada por três voltas Codex recebe a primeira reprovação do verificador final. `(0, 3) → (1, 3)` continua produzindo `[n=3]` e o Stop cala a nova reprovação. O selo injeta `3 → 4` diretamente na saída do juiz. Na trilha, mesmo quando o valor muda, o corte de 46 caracteres elimina o sufixo `[n=]`. `bin/disjuntor.py:994`, `bin/disjuntor.py:1003`, `bin/disjuntor.py:1014`, `bin/disjuntor.py:1018`, `bin/hook_stop_fila1.py:1410`, `bin/hook_stop_fila1.py:48`, `bin/tests/test_hook_stop_borda.sh:148`

5. **Bloqueia · V1 #7 · A cura cobre `escala/models.py` e o subconjunto literal do registro.** Com as entradas atuais preservadas, `"agenda/novo.py::f"` em um `dict` literal de família selecionada → mão única. A mesma referência via `SITE`, via `JUIZES['fechamento']['q2'] = …`, via `.update(...)` ou via chave de família com apelido → pode sumir da leitura e dar mão dupla. Lista ou tupla literal com duas referências separadas entra pelo `ast.walk`. A entrada composta `'ponto/a.py::a + agenda/novo.py::f'` só captura o primeiro arquivo. O problema também existe com AST literal válido: se a árvore a pousar acrescenta o registro e `app/agenda/novo.py`, a classificação ainda lê o registro da CASA. Com B3 armado, esses dois caminhos podem dar `rc 0` de mão dupla antes de consultar o B3. Confirmei os formatos em memória. `app/core/juizes.py:7`, `app/core/juizes.py:190`, `bin/mao_unica.py:29`, `bin/mao_unica.py:47`, `bin/mao_unica.py:66`, `bin/commit_msg.sh:14`, `bin/commit_msg.sh:38`, `bin/disjuntor.py:1303`

6. **Fecha na via real · V1 #8 · Caminhos com espaço, aspas, tab e acento.** `-z` preserva os bytes dos nomes. A normalização casa `célula` com `celula`. `--no-renames` faz o renome sair como `D` e `A`, inclusive quando só o caminho antigo é sensível. **Fila para o parser isolado:** um `R100` com dois caminhos retorna `None` por paridade ímpar. Dois `R100` podem dar paridade par e deslocar os campos, por exemplo `R100␀app/x.py␀ponto/a.py␀R100␀app/y.py␀app/z.py␀` perde `ponto/a.py` como caminho. Essa entrada fica fora da chamada real com `--no-renames`. `bin/mao_unica.py:32`, `bin/mao_unica.py:122`, `bin/mao_unica.py:127`, `bin/mao_unica.py:135`, `bin/tests/test_mao_unica.sh:97`, `bin/tests/test_mao_unica.sh:137`

7. **Fecha para o índice do hook · V1 #9.** O `write-tree` roda com o ambiente recebido e o mesmo SHA vai ao B3 depois do `unset`. A resolução sensível presente só no índice alternativo entra no diff contra `HEAD`. A classificação ainda une esse delta a `HEAD...MERGE_HEAD`. Assim, uma alteração em `ponto/` já presente em `HEAD`, mas trazida pelo primeiro pai desde a base, ainda pode classificar como mão única um resultado que só grava documentação. Esse critério é distinto da releitura do índice corrigida aqui. `bin/commit_msg.sh:19`, `bin/commit_msg.sh:26`, `bin/commit_msg.sh:28`, `bin/commit_msg.sh:38`, `bin/mao_unica.py:141`, `bin/mao_unica.py:153`

8. **Bloqueia · V1 #11 · O caminho normal do CENSO compartilha a leitura. O Stop ainda relê o BACKLOG no PAREI.** Um Stop lê `ITEM-A` aberto, com `PAREI: ! ITEM-A` e mais nenhum item. Entre essa leitura e `_ids_que_nao_andam`, o escritor fecha `ITEM-A`. Resultado reproduzido em memória: o Stop combina as duas versões e registra `AVISO fila 1 com item aberto`. Com a primeira versão íntegra, registraria `o RELATO declara PAREI`. O selo conta só `_backlog_texto` e usa RELATO neutro. O HANDOFF também relê nas seções `item EM CURSO`, `LISTA TRAVADA` e `PROXIMO PASSO`, podendo publicar IDs diferentes no mesmo painel. A recuperação de exceção do CENSO volta a classificar a abertura e pode obter outra leitura. `bin/disjuntor.py:2289`, `bin/disjuntor.py:1935`, `bin/hook_stop_fila1.py:337`, `bin/hook_stop_fila1.py:1503`, `bin/hook_stop_fila1.py:1585`, `bin/tests/test_hook_stop_borda.sh:123`, `bin/handoff_sessao.sh:55`, `bin/handoff_sessao.sh:73`, `bin/handoff_sessao.sh:302`, `bin/handoff_sessao.sh:322`

9. **Bloqueia · V1 #11 e #19 · A trava serializa, mas uma falha de aviso agora passa na frente das portas do Stop.** Um processo suspenso com `BORDA.lock` tomada → os Stops seguintes esperam sem prazo. `PermissionError` na borda com RELATO sem prova → silêncio e `rc 0` em vez de `block`, confirmado em memória. A correção tira o traceback do `finally`, porém apaga o estado cedo: morte após `remove` e antes de `replace` → o Stop seguinte repete o mesmo aviso. Morte após `replace` e antes da trilha → o seguinte pode calar um aviso que nunca foi registrado. Uma saída com prazo é limitar a espera da trava à transação de avisos, depois de julgar as portas. Com prazo esgotado, preservar o TSV e a decisão já tomada, sem confirmar a transição de aviso. O orçamento já tem um exemplo de espera limitada. O censo da ordem mostra borda → disjuntor/orçamento/espera, e um caminho inverso está **[nao verificado]**. `bin/hook_stop_fila1.py:901`, `bin/hook_stop_fila1.py:910`, `bin/hook_stop_fila1.py:955`, `bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1553`, `bin/hook_stop_fila1.py:1630`, `bin/tests/test_hook_stop_borda.sh:169`

10. **Bloqueia · V1 #18 · O selo protege os dois nomes enquanto o inode original está acessível.** Com um hardlink preexistente para o TSV, `_borda_toma` desvincula o nome original antes da prova. `_prova_arquivo` já não encontra esse inode na lista protegida. Com o mtime do alias registrado e depois avançado, `ESPERANDO arquivo=<alias>` → prova aceita. Confirmei em memória a mudança de `arquivo do proprio hook` para prova válida. O teste atual chama a prova diretamente com o original presente. `bin/hook_stop_fila1.py:665`, `bin/hook_stop_fila1.py:675`, `bin/hook_stop_fila1.py:688`, `bin/hook_stop_fila1.py:728`, `bin/hook_stop_fila1.py:910`, `bin/hook_stop_fila1.py:1591`, `bin/tests/test_hook_stop_borda.sh:195`

11. **Fecha o mutante de porta sempre falsa · V1 #12. Bloqueia pela outra vacuidade.** Em memória, a porta sempre falsa agora dá `ZERO DECLARADO` com `rc 1`. Já trocar o comparador principal por `faltam = []` deixa o selo com `rc 0` e a mensagem `o caso MORDE`. A contraprova usa outro `any(...)` sobre a cópia alterada em memória, sem passar pelo comparador que decide o resultado. `bin/tests/test_relato_guarda_pedido_de_patch.sh:237`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:274`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:286`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:302`

12. **Bloqueia · V1 #15 · A lista de inclusão e o caminho literal passam a entrar.** `open('app/docs/' 'RELATO' '.md').read()` → fora. É um caminho literal concatenado pelo Python, mas a peneira textual retorna antes do AST. A justificativa de que o nome cru é condição necessária falha para essa entrada. Confirmei `_entra(...) == False` em memória. `bin/tests/test_relato_guarda_pedido_de_patch.sh:110`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:204`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:250`

13. **Bloqueia · V1 #16 · Fecha os modos mistos diretos.** `open(open('app/docs/RELATO.md').read().strip(), 'w')` → fora, embora o `open` interno leia o RELATO. O classificador sobe todos os ancestrais e atribui ao caminho o modo do `open` externo. No sentido inverso, `Path('app/docs/RELATO.md').open('a')` → entra, pois o modo posicional de um método ligado fica em `args[0]`. Confirmei ambos em memória. `bin/tests/test_relato_guarda_pedido_de_patch.sh:86`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:119`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:225`

14. **Fecha o sobrevivente · V1 #17.** Retirar individualmente `RELATO.md`, `RELATO-ARQUIVO.md` ou `HANDOFF-SESSAO.md` da dispensa acrescenta aquele arquivo aos achados e quebra a igualdade exata. Confirmei as três remoções em memória usando `_ruins`. A alteração é na fixture e a lista de dispensas permanece a mesma. O brief fornecido veda tocar `app/`. A autorização para levar essa alteração do selo F4 junto com a F1 é **[nao verificado]**. `app/pautas/tests/test_contract_vocabulario.py:23`, `app/pautas/tests/test_contract_vocabulario.py:29`, `app/pautas/tests/test_contract_vocabulario.py:45`, `app/pautas/tests/test_contract_vocabulario.py:66`

## Escritores, leitores e significado

| Arquivo ou campo | Escrita, leitura e divergência |
|---|---|
| `hook_stop_borda.tsv` | `_avisos_por_borda` grava `tipo\|obra<TAB>hash16` por `replace`. `_borda_toma` também muda o estado ao apagar o arquivo. `_borda_le` lê para as duas entradas, e `_prova_arquivo` consulta o inode. O comentário de escritor único descreve a gravação do TSV, mas omite a transição destrutiva. Ausência pode significar Stop que saiu sem avisos ou Stop interrompido antes de terminar. Fixtures também escrevem ou removem o TSV. `bin/hook_stop_fila1.py:890`, `bin/hook_stop_fila1.py:910`, `bin/hook_stop_fila1.py:924`, `bin/hook_stop_fila1.py:928`, `bin/hook_stop_fila1.py:942`, `bin/hook_stop_fila1.py:675`, `bin/tests/test_hook_stop_borda.sh:86`, `bin/tests/test_disjuntor.sh:351` |
| `hook_stop_borda.tsv.lock` | `_borda_trava` cria, abre e toma o flock, chamada por `_borda_toma` e `_avisos_por_borda`. A posse é do kernel pelo descritor, retido até a saída do processo. `_prova_arquivo` lê metadados, e o selo também abre e toma a trava. Nenhum consumidor de conteúdo apareceu nesses sítios. `bin/hook_stop_fila1.py:675`, `bin/hook_stop_fila1.py:896`, `bin/hook_stop_fila1.py:901`, `bin/hook_stop_fila1.py:912`, `bin/hook_stop_fila1.py:945`, `bin/tests/test_hook_stop_borda.sh:169` |
| `BACKLOG.md`, marcador e linha crua | O marcador e o bloco são declarados à mão. `gerar_backlog.py` lê, preserva e regrava a janela. `backlog_estado.py` lê a linha e reescreve a coluna `[3]`. Leitores do bloco/linha afetados são o Stop, o CENSO, o HANDOFF e o medidor de interrompidas. O Stop usa a linha inteira como impressão, enquanto o CENSO usa os IDs andáveis e o HANDOFF usa o veredito do marcador. `interrompidas_estado.py` usa o ID do marcador mesmo quando o veredito é `parado`, podendo publicar `VOLTOU` para o mesmo topo que o HANDOFF descarta. O alarme também tem um leitor próprio de abertura, por texto na linha inteira. `bin/gerar_backlog.py:102`, `bin/gerar_backlog.py:111`, `bin/gerar_backlog.py:138`, `bin/backlog_estado.py:30`, `bin/backlog_estado.py:58`, `bin/hook_stop_fila1.py:184`, `bin/hook_stop_fila1.py:1629`, `bin/disjuntor.py:1785`, `bin/handoff_sessao.sh:55`, `bin/interrompidas_estado.py:89`, `bin/interrompidas_estado.py:121`, `bin/alarme_sessao_ociosa.py:89` |
| B3 `reprovados`, `codex`, `disparado` e `[n=]` | `decide_b3` e `rearma_b3` alteram os campos pelo gravador `_com_estado`. `julga_b3` expõe `max(n, nc)`. `estado_b3`, usado pelo merge, lê a presença da obra no mapa. O Stop formata `[n=]` e a borda consome a mensagem para o hash. O ESTADO lê eventos `DISJUNTOR` e `LIBERA` da trilha do disjuntor, com semântica de eventos. No Stop, `[n=]` é o máximo entre contagens, insuficiente como identidade de transição. `bin/disjuntor.py:334`, `bin/disjuntor.py:990`, `bin/disjuntor.py:1071`, `bin/disjuntor.py:1080`, `bin/disjuntor.py:1114`, `bin/disjuntor.py:2401`, `bin/hook_stop_fila1.py:949`, `bin/hook_stop_fila1.py:1410` |
| `HANDOFF-SESSAO.md` | `handoff_sessao.sh` escreve por `mv` ou por truncamento seguido de `append` no ramo de teto. `PreCompact` chama o gerador, e `SessionStart` lê o texto após compact ou clear. O Stop consulta só o `mtime` para CLEAR e ESCADA. O selo de vocabulário pula o documento pelo nome. Um mesmo `mtime` pode autenticar um painel recém-gerado com leituras contraditórias do BACKLOG. `bin/handoff_sessao.sh:330`, `bin/handoff_sessao.sh:335`, `.claude/settings.json:20`, `.claude/settings.json:29`, `.claude/settings.json:40`, `.claude/settings.json:53`, `bin/hook_stop_fila1.py:1044`, `bin/hook_stop_fila1.py:1259`, `bin/hook_stop_fila1.py:1408`, `bin/disjuntor.py:1471`, `app/pautas/tests/test_contract_vocabulario.py:45` |

## Mordida dos selos e classes de reprovação

- **CENSO.** `perguntas-so-com-dois-pontos`, `prosa-zera-perguntas`, `caminho-mais-longo-entre-leituras`, `obra-do-caminho-desligada`, `agent-sem-porta`, `censo-rele-a-fila` e `caminho-rele-a-fila` têm casos que distinguem a mutação. Confirmei as duas releituras em memória. A sonda de contagem ignora o veredito final, então uma negativa incondicional depois de uma leitura também passa nessa sonda. Os casos de herança, subpasta e mtime exercitam seus respectivos ramos. `bin/tests/test_censo_antes_da_vaga.sh:100`, `bin/tests/test_censo_antes_da_vaga.sh:718`, `bin/tests/test_censo_antes_da_vaga.sh:738`, `bin/tests/test_censo_antes_da_vaga.sh:796`, `bin/tests/test_censo_antes_da_vaga.sh:817`
- **Stop.** `aviso-repetido-no-mesmo-estado`, `sumiu-nao-zera`, `fila-sempre-avisa`, `fila-sem-linha-crua`, `fila-rele-o-backlog`, `b3-sem-n`, `borda-sem-trava`, `borda-prova-espera` e `handoff-imprime-topo-parado` têm uma asserção que muda com a mutação. `limpeza-no-finally` depende de a permissão realmente impedir o `remove` e não testa o `block` obrigatório. `borda-sem-trava` cobra precisamente o timeout externo de um Stop preso. O selo do HANDOFF também fica verde se a linha existir e disser sempre `nenhum`. Os quatro mutantes de chave, leitura de estado e tratamento de superado têm casos distintos nas fixtures. `bin/tests/test_hook_stop_borda.sh:45`, `bin/tests/test_hook_stop_borda.sh:66`, `bin/tests/test_hook_stop_borda.sh:93`, `bin/tests/test_hook_stop_borda.sh:123`, `bin/tests/test_hook_stop_borda.sh:148`, `bin/tests/test_hook_stop_borda.sh:169`, `bin/tests/test_hook_stop_borda.sh:181`, `bin/tests/test_hook_stop_borda.sh:228`, `bin/tests/test_hook_stop_borda.sh:259`
- **Mão única.** `juizes-ignorado`, `registro-ilegivel-vira-dupla`, `sem-forma`, `arvore-relida` e `commit-msg-sem-arvore` têm contrapartida nas asserções. `sem-z` cai pela incompatibilidade do formato inteiro, inclusive no controle de documentação. `sem-arvore-ainda-le` cai ao entregar `None` ao subprocesso, sem demonstrar uma releitura real do índice. O caso do `commit-msg` distingue mão dupla pela mensagem, aceita `B3-NAO-SEI` como evidência para a árvore sensível e ignora o `rc` do hook. A cópia do mutante sequer inclui `test_lei_akita.sh`, chamado depois do B3. `bin/tests/test_mao_unica.sh:40`, `bin/tests/test_mao_unica.sh:91`, `bin/tests/test_mao_unica.sh:104`, `bin/tests/test_mao_unica.sh:129`, `bin/tests/test_mao_unica.sh:142`, `bin/tests/test_mao_unica.sh:169`, `bin/tests/test_mao_unica.sh:206`, `bin/commit_msg.sh:42`
- **Juiz, relógio, `except` e allowlist.** O wrapper `_item_da_ordem_viva` delega ao mesmo juiz. As derivações que divergem são a projeção parcial do registro por AST e os significados de topo/andável descritos acima. Há duas linhas adicionadas no diff com `time.time()`: a do CLI preserva a leitura de borda que já existia e a outra pertence ao teste. O diff fornecido adiciona zero `except` a código executado. A dispensa sintática nova do selo é a expressão com string avulsa. A `ALLOWLIST` de vocabulário segue vazia. `bin/hook_stop_fila1.py:193`, `bin/mao_unica.py:53`, `bin/disjuntor.py:2792`, `bin/tests/test_censo_antes_da_vaga.sh:744`, `bin/tests/test_relato_guarda_pedido_de_patch.sh:110`, `app/pautas/tests/test_contract_vocabulario.py:23`

Li na íntegra os arquivos alterados e consultei `CLAUDE.md`, `JUIZES.md`, o registro e os consumidores citados. Fiz sondas em memória e `bash -n` nos seis scripts alterados. A cópia não expõe metadados Git. O hash independente, os selos de host que criam fixtures em disco, a suite integral, o pré-requisito de pouso e as duas métricas de produção ficam **[nao verificado]**. `CLAUDE.md:352`, `CLAUDE.md:428`, `bin/tests/test_hook_stop_borda.sh:28`, `bin/tests/test_mao_unica.sh:87`

## 28/11 04:02 · sessao f08fe66d4eaf · CUSTO-DE-COORDENACAO · verificador · origem=Code

**Parecer pré-verificador 5(a), volta 1: bloqueia.** Há caminhos em que o B3 aceita como mão dupla um merge que toca matéria de mão única, o CENSO aprova pergunta de lei sem corte e a cura do selo deixa leitores reais fora da varredura. `bin/mao_unica.py:39` · `bin/disjuntor.py:1303` · `bin/disjuntor.py:1681` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:63`

Conferi o diff fornecido contra a cópia em leitura. O selo do RELATO saiu com rc 0, um leitor e quatro contratos. A sintaxe dos seis scripts shell alterados passou em `bash -n`. A cópia veio sem `.git`. A cadeia de commits, os demais rc de execução, os falsos de host, os avais em `logs/`, o dossiê e as duas métricas de produção ficam [nao verificado]. `bin/tests/test_relato_guarda_pedido_de_patch.sh:228` · `app/ponto/tests/test_tela_gestao_he_fatia2_lote_limite.py:236`

## 1. Aderência ao brief

1. **Nit — F1 (1), ficaram restos do freio.** Os cortes de cota saíram da decisão do Agent, do Stop e do Codex. A palavra `escada` continua acionando o B4, o parser compartilhado continua alimentando ANEXO-ENXUTO e o ESTADO conserva as duas medidas. O caso de 99% cobre o PreToolUse do Agent e a permissão Bash para chamar o wrapper. `VOLTA_CONTRATO` ainda diz que acima do orçamento se vai sem Codex, e `codex_decide` ainda repassa relógio e transcript a uma função que deixou de usá-los. As referências restantes a B2 no hook e no disjuntor são lápides. `bin/disjuntor.py:1422` · `bin/disjuntor.py:1449` · `bin/disjuntor.py:1481` · `bin/disjuntor.py:1580` · `bin/disjuntor.py:1603` · `bin/disjuntor.py:2648` · `bin/disjuntor.py:2684` · `bin/tests/test_disjuntor.sh:525`

2. **Bloqueia — F1 (2a–e), o conteúdo ainda depende da forma do rótulo.** O coletor pega `c1/brief.md` sem citação, usa o `mtime` do descritor, herda referências e exige decisões no vivo. Também recusa ausência de brief, de referências e de corte nas perguntas que reconhece. Contraexemplo verificado em memória: um brief com leitores, escritores e decisões válidos, seguido de `### Perguntas de lei` e `- Pode gravar?`, é aprovado. O cabeçalho sem `:` é descartado e a lista vira zero perguntas. Uma linha de prosa entre `perguntas de lei:` e a lista também zera o rótulo. Além do pedido, o glob restringe o universo a `.md` e todo empate de `mtime` vira vários briefs vivos. Um único `brief.txt` válido fica fora. `bin/disjuntor.py:1630` · `bin/disjuntor.py:1656` · `bin/disjuntor.py:1696` · `bin/disjuntor.py:1746` · `bin/tests/test_censo_antes_da_vaga.sh:151` · `bin/tests/test_censo_antes_da_vaga.sh:173`

3. **Bloqueia — F1 (2d), o caminho pode escolher outra obra.** Com `OBRA-X` e `CODEX-OBRA-X` abertas, `/home/ronald/wt-codex-obra-x-000001` resolve para `CODEX-OBRA-X`: o código disputa o slug bruto com o slug sem `codex-` e prefere o token mais longo. Se só `CODEX-OBRA-X` tiver CENSO aprovado, o Agent da cópia de `OBRA-X` recebe a aprovação da outra obra. A via wrapper mantém o ID declarado e passa a mesma leitura da fila ao juiz. `bin/disjuntor.py:1804` · `bin/disjuntor.py:2274` · `bin/disjuntor.py:2716` · `bin/codex_pergunta.sh:487`

4. **Fila — F1 (3), o universo do diff diverge do comando literal.** O brief pede `HEAD MERGE_HEAD`. O código usa a união de `HEAD...MERGE_HEAD` com `--cached HEAD` e soma a classificação de exclusões de arquivos presumidos de produção, coerente com a regra já escrita na PARADA 2. Exemplo da diferença: um `c.py` introduzido só na principal depois da bifurcação aparece como `D` entre pontas e fica fora da união. Uma edição nova de resolução entra pelo índice. O caminho de mão dupla grava o aviso e retorna antes de `estado_b3`; o merge continua exigindo `OBRA:`. As perdas de guarda estão na pergunta 2. `bin/mao_unica.py:30` · `bin/mao_unica.py:85` · `bin/tests/test_mao_unica.sh:47` · `bin/tests/test_mao_unica.sh:64` · `bin/disjuntor.py:1297` · `CLAUDE.md:278`

5. **Bloqueia — F1 (4), o Stop compara o texto apresentado, perde mudanças de estado e mantém o topo antigo em leitores do painel.** Trocar só a coluna de estado de `**em voo**` para `**em voo** volta 2` não gera aviso, e o selo exige esse silêncio. Uma nova reprovação da mesma obra já armada também pode manter o mesmo texto B3. `MERGE` e `ERRO-BOBO` aparecem como instruções estáticas dentro do B3, sem aviso próprio com estado próprio. No BACKLOG da cópia, `ORDEM-VIVA-1511-NOITE` segue como topo e o juiz retorna `ok`. Num exemplo com `**SUPERADA**`, o juiz esvazia `EM CURSO`, mas o handoff ainda imprime o ID em `ORDEM VIVA (topo declarado)` e a lista dos andáveis ainda o inclui. Com `SUPERADA` sem negrito, até `EM CURSO` permanece. `bin/hook_stop_fila1.py:56` · `bin/hook_stop_fila1.py:226` · `bin/hook_stop_fila1.py:251` · `bin/hook_stop_fila1.py:1385` · `bin/hook_stop_fila1.py:1596` · `bin/disjuntor.py:990` · `bin/disjuntor.py:1317` · `bin/tests/test_hook_stop_borda.sh:47` · `bin/handoff_sessao.sh:63` · `app/docs/BACKLOG.md:8` · `app/docs/BACKLOG.md:1926`

6. **Fila — documentação e Pronto.** O diff atualiza o PRINCÍPIO, a PARADA 4 e o DOSSIE/CENSO. A frase “cota 7d e orçamento Codex só no ESTADO” omite o aviso de cota do Agent na trilha. O selo de 60 Stops conta apenas `ALLOW AVISO fila 1 com item aberto`, e o ESTADO soma negativas de CENSO sem separar as de forma. Esses contadores deixam sem prova `< 50 avisos/dia` e `CENSO de forma = 0`. As alterações em `app/pautas/tests/` e no selo do RELATO pertencem às fatias adicionais identificadas na entrega. `CLAUDE.md:264` · `CLAUDE.md:288` · `CLAUDE.md:448` · `bin/disjuntor.py:1443` · `bin/disjuntor.py:2513` · `bin/tests/test_hook_stop_borda.sh:34` · `bin/tests/test_hook_stop_borda.sh:45` · `app/pautas/tests/test_contract_vocabulario.py:14` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:51`

## 2. Guarda de mão única

7. **Bloqueia — o classificador perde alterações no juiz da célula.** Uma mudança de uma linha em `EscalaColaborador.eh_dia_trabalho`, por exemplo trocar a leitura de `cel.trabalha`, toca a autoridade registrada da célula. O único caminho do diff, `app/escala/models.py`, resulta em mão dupla. Para uma obra já armada, o novo retorno `0` pula o B3. `CLAUDE.md:257` · `app/core/juizes.py:190` · `app/escala/models.py:1080` · `app/escala/models.py:1105` · `bin/mao_unica.py:39` · `bin/disjuntor.py:1303`

8. **Bloqueia — a representação do caminho pode enfraquecer até a regra literal.** `_name_status` lê texto com aspas e escapes sem decodificá-los. Com saída `M<TAB>"ponto/a\tb.py"` — `\t` representando um tab no nome — o primeiro segmento fica `"ponto` e o classificador devolve mão dupla. `app/relatorios/célula.py` também dá mão dupla mesmo quando fornecido já decodificado, pois o termo procurado é `celula`. O caso `None` preserva o B3 e uma exceção que escapa faz o hook recusar pelo rc. Os dois contraexemplos chegam como dados aceitos, portanto passam pelo retorno de mão dupla. `bin/mao_unica.py:19` · `bin/mao_unica.py:39` · `bin/mao_unica.py:55` · `bin/mao_unica.py:70` · `bin/disjuntor.py:1303` · `bin/commit_msg.sh:36`

9. **Bloqueia — a árvore julgada pode divergir da árvore que será gravada.** `commit_msg.sh` faz `git write-tree` com o ambiente recebido, depois apaga `GIT_INDEX_FILE`. O Python consulta novamente `--cached HEAD`. Num merge com alteração documental na raia e resolução em `app/ponto/` presente apenas no índice alternativo usado pelo commit, o B3 pode ver só a alteração documental. O teste de resolução usa um único índice, e o selo limpa `GIT_*` na preparação. `bin/commit_msg.sh:19` · `bin/commit_msg.sh:27` · `bin/mao_unica.py:94` · `bin/tests/test_mao_unica.sh:18` · `bin/tests/test_mao_unica.sh:69`

## 3. Escritores, leitores e significados

10. **Fila — inventário dos contratos.** As divergências de unidade, borda ou significado com contraexemplo estão nas linhas da tabela. O fuso efetivo dos processos em produção fica [nao verificado].

| Campo ou arquivo | Quem escreve, quem lê e qual é o contrato |
|---|---|
| `cota.json` | A status line grava `seven_day_pct` em %, `seven_day_resets_at` e `hora` em segundos Unix, além de `contexto_tokens`, `contexto_pct` e `session_id`. O disjuntor usa só o par semanal, com `reset > agora`. O aviso inclui `pct >= 75` e converte o restante em horas. O ESTADO arredonda o percentual para exibição. Contraexemplo de validação divergente: o produtor rejeita `bool`, mas o leitor aceita `true` como percentual num JSON legado ou inválido. `bin/statusline_cota.py:26` · `bin/statusline_cota.py:45` · `bin/disjuntor.py:1329` · `bin/disjuntor.py:1439` · `bin/disjuntor.py:2680` |
| Cota e orçamento Codex | O produtor externo do JSONL de sessão fica [nao verificado]. `leituras_semanais` lê `primary` de 10.080 minutos, timestamp convertido a epoch, porcentagem e reset. `orcamento_do_dia` usa meia-noite local, delta em pontos percentuais, orçamento em pp/dia, comparação estrita `>` e reset exclusivo. `ler` e `_codex_txt` publicam a medida. `bin/codex_limite_estado.py:131` · `bin/codex_limite_estado.py:165` · `bin/disjuntor.py:2441` · `bin/disjuntor.py:2648` |
| `disjuntor_estado.json` | `_com_estado` grava sob `flock`. B1 grava por agente `checado` em epoch, `disparado` e `motivo`; o Stop lê os disparados ainda vivos. B3 acumula IDs de reprovas e voltas, SHA de marcas consumidas e `disparado_em` em epoch. `decide_b3` libera com `mtime >= disparado_em` e SHA novo, e é consultado pelo Stop, PreToolUse e merge de mão única. `b4_para.desde` é escrito e lido por `b4_stop` como epoch do primeiro Stop da ESCADA; o `mtime` do handoff considera igualdade suficiente. O produtor de `b4.negou_desde` saiu. `bin/disjuntor.py:334` · `bin/disjuntor.py:570` · `bin/disjuntor.py:810` · `bin/disjuntor.py:988` · `bin/disjuntor.py:1024` · `bin/disjuntor.py:1080` · `bin/disjuntor.py:1460` · `bin/hook_stop_fila1.py:1020` · `bin/hook_stop_fila1.py:1385` · `bin/handoff_sessao.sh:330` |
| `triagem.log` e contador B3 | `triagem` escreve instante local, obra, ID da volta e veredito, usando o sidecar do wrapper. `_triagens` lê o último veredito por volta sob a trava do estado e usa `b3_triagem_linhas` como piso de integridade do total de linhas. `decide_b3` conta voltas procedentes distintas. `bin/disjuntor.py:868` · `bin/disjuntor.py:891` · `bin/disjuntor.py:920` · `bin/disjuntor.py:980` |
| `disjuntor.log` e ESTADO | `trilha` grava data local sem offset, com resolução de segundos. `ler` conta `DISJUNTOR`, `NEGOU`, `LIBERA` e falhas de RELATO. O B4 no placar agora conta disparos de ESCADA no **Stop**. A negativa ao Agent segue em `NEGOU` sem novo disparo B4. `_censo_24h` recorta `corte <= t <= agora`, embora um comentário declare limite inferior aberto. Contraexemplo: uma linha à hora exata do corte entra nos dois contadores e uma linha futura entra em `ler`, mas fica fora do CENSO. `bin/disjuntor.py:192` · `bin/disjuntor.py:1469` · `bin/disjuntor.py:2285` · `bin/disjuntor.py:2316` · `bin/disjuntor.py:2377` · `bin/disjuntor.py:2510` · `bin/disjuntor.py:2668` · `bin/gerar_estado.py:294` |
| Briefs, `CENSO APROVADO` e `chamadas.log` | O escritor concreto dos briefs vivos fica [nao verificado]. O CENSO lê conteúdo e `mtime` em segundos Unix, ordena por `mtime` e grava aprovação com obra, via e nomes dos briefs. O wrapper fornece PID e hash e grava `ini` e `fim` em hora local. `_linhas_chamadas` lê o formato e `_censo_24h` casa por obra, PID e hash, até 600 s antes do `ini`, com ambos os limites inclusivos. `APROVADO` do wrapper significa brief aceito. `ini` significa tentativa iniciada. `bin/disjuntor.py:1660` · `bin/disjuntor.py:1890` · `bin/disjuntor.py:2460` · `bin/disjuntor.py:2482` · `bin/disjuntor.py:2542` · `bin/codex_pergunta.sh:487` · `bin/codex_pergunta.sh:566` · `bin/codex_pergunta.sh:613` · `bin/codex_limite_estado.py:49` |
| RELATO, marcas e BACKLOG | `relato_negacao` formata a negativa e usa `esteira_vigia.no_relato`. A marca `PAREI: censo-…` usa `no_topo`, sob a mesma trava, e `prova_parei_censo` exige forma literal e processo vivo. Stop, handoff e alarme consultam essa porta. B3 lê `DESENHO` e `ERRO-BOBO` pelo topo e compara a marca com a fotografia do disparo. `ORDEM-VIVA-TOPO` tem escrita manual declarada e juiz `_item_da_ordem_viva`, consumido por Stop, handoff e ESTADO. As marcas de `.rc` de `ESPERA_UNICA` são lidas por obra, caminho e `HH:MM` local do `mtime`; o produtor dessas linhas específicas fica [nao verificado]. `bin/disjuntor.py:273` · `bin/disjuntor.py:300` · `app/core/esteira_vigia.py:846` · `bin/hook_stop_fila1.py:773` · `bin/handoff_sessao.sh:139` · `bin/alarme_sessao_ociosa.py:141` · `bin/disjuntor.py:723` · `bin/disjuntor.py:1024` · `bin/gerar_estado.py:233` · `bin/interrompidas_estado.py:93` · `bin/disjuntor.py:1963` · `bin/rc_sem_colher.py:25` · `bin/rc_sem_colher.py:76` |
| `hook_stop_borda.tsv`, trilha e saída do Stop | `_avisos_por_borda` lê via `_borda_le` e grava `tipo\|obra<TAB>sha1[:16]`. `_borda_fecha` apaga a memória nas saídas antecipadas. `main` consome os avisos novos. `_trilha` grava hora local, decisão, motivo truncado, `item` truncado e HEAD. O cliente recebe JSON `decision=block` ou silêncio de stdout com rc 0. O auxiliar dos selos lê ambos os `ALLOW` da fila como `siga-aviso`, inclusive o que significa silêncio de aviso. `bin/hook_stop_fila1.py:39` · `bin/hook_stop_fila1.py:137` · `bin/hook_stop_fila1.py:884` · `bin/hook_stop_fila1.py:918` · `bin/hook_stop_fila1.py:1530` · `bin/hook_stop_fila1.py:1588` · `.claude/settings.json:9` · `bin/tests/_saida_do_stop.sh:11` |

## 4. Classes de reprovação

11. **Bloqueia — segunda leitura da fonte e transição sem trava.** O Stop calcula `item` a partir de uma leitura do BACKLOG e relê o arquivo para a chave. Se o marcador passar de `A` para `B` entre as duas leituras, grava a impressão de `A` sob `FILA1|B`. Para inferir a obra do Agent, o CENSO também lê a fila uma vez na inferência e outra na porta. Na borda, dois Stops podem ler o mesmo TSV anterior e ambos emitir o mesmo aviso. `os.replace` protege a troca do arquivo. A sequência de ler, decidir e gravar pode intercalar com outra sequência ou com a remoção no `finally`. `bin/hook_stop_fila1.py:313` · `bin/hook_stop_fila1.py:890` · `bin/hook_stop_fila1.py:918` · `bin/hook_stop_fila1.py:1565` · `bin/hook_stop_fila1.py:1596` · `bin/disjuntor.py:1812` · `bin/disjuntor.py:1875` · `bin/disjuntor.py:2276`

12. **Bloqueia — o RED da cura passa com a porta sempre falsa.** Substituí `_alcanca_o_vivo` por `return False` em memória. Resultado: `ZERO DECLARADO`, rc 0. A saída pelo universo vazio vem antes dos nove casos que deveriam julgar a porta, e apaga da verificação até o leitor real que cobra quatro contratos. `bin/tests/test_relato_guarda_pedido_de_patch.sh:189` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:215` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:258` · `app/ponto/tests/test_tela_gestao_he_fatia2_lote_limite.py:236`

13. **Fila — há mutantes com caso correspondente e lacunas concretas.** Mão única sempre falsa, herança desligada, inferência desligada, aviso repetido, marcador superado e volta do freio têm casos declarados que exercitam a mudança. O selo da borda aceita a perda da coluna de estado e testa `parado` só pelo retorno do juiz, por isso deixa passar o topo antigo no handoff. O mutante de freio Codex consulta a **cota Claude**, e a checagem AST vigia uma lista fixa de funções. Um freio em `_b4_so_aviso` que negasse só a 100% passaria pelo AST e pelos percentuais exercitados. `bin/tests/test_mao_unica.sh:129` · `bin/tests/test_censo_antes_da_vaga.sh:760` · `bin/tests/test_hook_stop_borda.sh:47` · `bin/tests/test_hook_stop_borda.sh:101` · `bin/tests/test_hook_stop_borda.sh:139` · `bin/tests/test_disjuntor.sh:451` · `bin/tests/test_disjuntor.sh:531` · `bin/tests/test_disjuntor.sh:553` · `bin/tests/test_disjuntor.sh:1655`

14. **Fila — allowlists, juízes, relógio e `except`.** Há duas exceções de classificação: os três caminhos de diário da F4 e a presunção de que `tests/`, `test*.py` e `.md` estão fora de produção para fins de exclusão. Exemplo do segundo caso: um módulo importado em produção chamado `app/core/testemunho.py` seria dispensado dessa guarda. No diff fornecido, contei **0 tratadores novos**, **1 `except` reposicionado** na F4 e **1 tratador removido** do B4. A pergunta do classificador é sobre o diff, distinta das perguntas de domínio do registro. O relógio novo do merge continua vindo da entrada CLI. A dupla leitura de relógio do Stop e a leitura própria de `ler_chamadas` já aparecem nas rotas existentes. `app/pautas/tests/test_contract_vocabulario.py:29` · `app/pautas/tests/test_contract_vocabulario.py:47` · `bin/mao_unica.py:9` · `bin/mao_unica.py:23` · `app/core/juizes.py:190` · `bin/disjuntor.py:2285` · `bin/disjuntor.py:2776` · `bin/hook_stop_fila1.py:1381` · `bin/hook_stop_fila1.py:1537` · `bin/hook_stop_fila1.py:1589` · `bin/codex_limite_estado.py:71`

## 5. Cura de `_alcanca_o_vivo`

15. **Bloqueia — uma lista de inclusão é tomada por exclusão.** A forma abaixo lê o RELATO vivo se `paths` contém os caminhos sob `APP`, como numa varredura filtrada. A porta devolveu `False` em memória. O único uso de `D` está à direita de `in`, e a análise ignora que esse ramo executa a leitura. A forma também fica fora pela peneira anterior, que exige o literal isolado `'RELATO.md'`. `bin/tests/test_relato_guarda_pedido_de_patch.sh:63` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:84` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:189`

    ```python
    D = ('docs/RELATO.md',)
    for rel in paths:
        if rel in D:
            relato = open(os.path.join(APP, rel), encoding='utf-8').read()
    ```

16. **Bloqueia — `a+` também lê e passa pela peneira anterior.** `with open(os.path.join(APP, 'docs', 'RELATO.md'), 'a+') as f:`, seguido de `f.seek(0)` e `f.read()`, foi classificado `False` pela porta em memória. O teste de modo vê o `a` e ignora o `+`. Esse contraexemplo contém `'RELATO.md'` isolado, portanto a exclusão vem da cura. O leitor real encontrado na cópia usa `io.open(...).read()` e entra. `bin/tests/test_relato_guarda_pedido_de_patch.sh:71` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:95` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:189` · `app/ponto/tests/test_tela_gestao_he_fatia2_lote_limite.py:236`

## 6. F4 e o vocabulário

17. **Resposta: sim, dentro do universo varrido. Fila — cobertura parcial da lista.** Uma docstring Python ou prosa em `app/docs/OUTRO.md` contendo `post-it` derruba o teste. Ele varre `.py`, `.html`, `.js` e `.md` sob `app/`, com os filtros gerais de diretório e a exclusão do nome do próprio selo. Quem define diário de máquina é a tupla `DIARIOS_DE_MAQUINA`, por três caminhos exatos: `app/docs/RELATO.md`, `app/docs/RELATO-ARQUIVO.md` e `app/docs/HANDOFF-SESSAO.md`. A dispensa vale para o arquivo inteiro, inclusive prosa escrita à mão. O código cita o aval F4, cujo teor fica [nao verificado]. O fixture cobre só o primeiro diário. O mutante que remove a dispensa do arquivo morto e do handoff sobreviveria nesta cópia: a leitura direta dos dois arquivos encontrou zero ocorrências pelos padrões do selo. `app/pautas/tests/test_contract_vocabulario.py:14` · `app/pautas/tests/test_contract_vocabulario.py:22` · `app/pautas/tests/test_contract_vocabulario.py:29` · `app/pautas/tests/test_contract_vocabulario.py:39` · `app/pautas/tests/test_contract_vocabulario.py:66` · `CLAUDE.md:500` · `bin/handoff_sessao.sh:330`

## 7. Outros defeitos

18. **Bloqueia — o Stop pode usar a própria atividade como prova de trabalho.** `hook_stop_borda.tsv` ficou fora dos arquivos recusados por `_prova_arquivo`. Com `ESPERANDO: agente-x | arquivo=logs/hook_stop_borda.tsv`, se o TSV já existe, um Stop registra seu `mtime` e segue sem prova. O próprio Stop substitui o TSV mesmo com conteúdo idêntico. No próximo Stop, o `mtime` maior pode provar a espera sem atividade do agente. `bin/hook_stop_fila1.py:644` · `bin/hook_stop_fila1.py:666` · `bin/hook_stop_fila1.py:701` · `bin/hook_stop_fila1.py:831` · `bin/hook_stop_fila1.py:930` · `bin/hook_stop_fila1.py:1560`

19. *

[truncado em 20000 de 20434 caracteres]

## 28/11 02:29 · sessao a7747a4bca90 · CODIGO-DOMINIO-VALIDO · leitura · origem=Code

```python
# SONDA_ARVORE=/caminho/da/copia/app bin/sonda_leitura.sh --sombra /caminho/s12.py
# O executor monta /app e /sonda em leitura (bin/sonda_leitura.sh:226-240).
# O executor prepara credencial e grants, copia a sonda para um temporário e
# grava a trilha (bin/sonda_leitura.sh:115-143,153-181,216-218,245-250).

import logging
import sys
from collections import Counter
from datetime import date

from django.conf import settings
from django.db import connection, transaction
from django_tenants.utils import schema_context

# Guarda da sombra (app/config/settings/sombra.py:29,49-50,60).
if (
    getattr(settings, "HASNER_SOMBRA", False) is not True
    or connection.settings_dict["NAME"] != "sombra"
):
    raise SystemExit(1)


def falha_sem_dados(tipo, _valor, _traceback):
    print(f"S12_FALHA|{tipo.__name__}", file=sys.stderr)


# Suprime logs e mensagens de falha que podem conter nomes
# (app/ponto/services/fechamento.py:1327-1332, app/folha/export.py:23-33).
logging.disable(logging.CRITICAL)
sys.excepthook = falha_sem_dados

from colaboradores.models import Empresa
from folha.export import (
    classificar_export,
    janela_competencia,
    montar_linhas_export,
    validar_layout_dominio,
)
from ponto.services.fechamento import recalcular_fechamento_mes


def tipo_layout(erro):
    # Rótulos derivados dos tipos fixos do validador (app/folha/export.py:266-307).
    if erro.startswith("reg10 campo "):
        campo = erro.split(" ", 3)[2]
        return f"reg10_campo_{campo}_nao_numerico"
    for prefixo, tipo in (
        ("reg10 len ", "reg10_len"),
        ("reg10 const ", "reg10_const"),
        ("reg11 soma ", "reg11_soma_8792"),
        ("reg11 len ", "reg11_len"),
        ("reg11 malformado:", "reg11_malformado"),
        ("reg11 data invalida:", "reg11_data_invalida"),
        ("linha desconhecida:", "linha_desconhecida"),
    ):
        if erro.startswith(prefixo):
            return tipo
    raise SystemExit("S12_FALHA|tipo_layout_desconhecido")


mes, ano = 11, 2026
erros_por_col = []

# Schema e READ ONLY do molde (bin/sonda_leitura.sh:239-240,
# bin/gate_cobranca.sh:23-25). REPEATABLE READ mantém o mesmo snapshot
# nas remontagens de diagnóstico (app/folha/export.py:720-754).
with schema_context("juliani"), transaction.atomic(durable=True):
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY, ISOLATION LEVEL REPEATABLE READ")

    # Lookup por pk da entrega (app/folha/views.py:123).
    empresa = Empresa.objects.get(pk=2)

    # Guarda da janela do anexo (app/folha/export.py:47-51, app/ponto/janelas.py:36-57).
    if janela_competencia(empresa, mes, ano) != (date(2026, 10, 21), date(2026, 11, 20)):
        raise SystemExit(2)

    # Mesma chamada da entrega (app/folha/views.py:215-217,
    # app/ponto/services/fechamento.py:468-471,1152-1160,1333-1334).
    fechamentos = list(
        recalcular_fechamento_mes(
            mes, ano, empresa_id=empresa.pk, somente_leitura=True, retornar_objeto=True
        ).values()
    )

    # Mesmo veredito da entrega (app/folha/views.py:219-220, app/folha/export.py:774-798).
    itens = classificar_export(empresa, mes, ano, fechamentos=fechamentos)

    # Mesmos itens e fechamentos em memória (app/folha/views.py:273-276,
    # app/folha/export.py:699-755).
    linhas, entra_ids = montar_linhas_export(
        empresa, mes, ano, itens=itens, fechamentos=fechamentos
    )

    # A view valida quando há linhas (app/folha/views.py:278-283,
    # app/folha/export.py:266-307).
    erros_layout = validar_layout_dominio(linhas) if linhas else []

    if erros_layout:
        posicao = 0
        erros_conferidos = []
        for fech in fechamentos:
            pk = fech.colaborador_id
            if pk not in entra_ids:
                continue

            # Atribui pelo pk do fechamento. O montador concatena os blocos
            # na ordem recebida, e cada bloco não vazio começa com reg10
            # (app/folha/export.py:34-45,215-221,726-755).
            bloco, _ = montar_linhas_export(
                empresa, mes, ano, itens=itens, fechamentos=[fech]
            )
            fim = posicao + len(bloco)
            if linhas[posicao:fim] != bloco:
                raise SystemExit("S12_FALHA|blocos_txt_divergentes")
            posicao = fim

            # O erro de soma se refere ao reg10 seguido dos reg11
            # (app/folha/export.py:266-307).
            for erro in validar_layout_dominio(bloco):
                erros_conferidos.append(erro)
                erros_por_col.append((tipo_layout(erro), pk))

        if posicao != len(linhas) or erros_conferidos != erros_layout:
            raise SystemExit("S12_FALHA|atribuicao_layout_divergente")

    transaction.set_rollback(True)

# Campos do veredito (app/folha/export.py:847-859,876-891).
# O motivo None é impresso como "None".
contagens = Counter((i["status"], str(i["motivo"])) for i in itens)
for (status, motivo), total in sorted(contagens.items()):
    print(f"CONTAGEM|{status}|{motivo}|{total}")

invalidos = sorted(
    (i["colaborador_id"], i["status"], i["motivo"])
    for i in itens
    if i["motivo"] == "codigo_dominio_invalido"
)
for pk, status, motivo in invalidos:
    print(f"ITEM|col{pk:03d}|{status}|{motivo}")

col1041 = next((i for i in itens if i["colaborador_id"] == 1041), None)
if col1041 is None:
    print("COL1041|ausente")
else:
    print(f"COL1041|{col1041['status']}|{col1041['motivo']}")

print(f"TXT|linhas={len(linhas)}|erros_layout={len(erros_layout)}")
for tipo, pk in erros_por_col:
    print(f"ERRO|{tipo}|col{pk:03d}")

# entra_ids conta o veredito, inclusive aptos sem linha (app/folha/export.py:725,734-755).
entra = len(entra_ids)
print(
    f"S12|itens={len(itens)}|entra={entra}|fora={len(itens) - entra}"
    f"|codigo_dominio_invalido={len(invalidos)}"
    f"|col1041_presente={int(col1041 is not None)}"
    f"|linhas={len(linhas)}|erros_layout={len(erros_layout)}"
)

# Premissas
# - A cópia corresponde ao HEAD f0d213df9 e é a montada em /app [nao verificado].
# - A sombra tem empresa pk=2, janela esperada e integração Domínio ativa [nao verificado].
# - Execução na sombra, cobertura do recálculo e resultados de S12, inclusive
#   presença e motivo de col1041 [nao verificado].
# - Paridade dos dados da sombra com produção e liberação da entrega pela view
#   [nao verificado].
```

## 28/11 02:27 · sessao 3c40eae37ee0 · CODIGO-DOMINIO-VALIDO · construtor · origem=Code

O diff altera somente as três docstrings solicitadas. A comparação com `f0d213df9` confirmou que o texto fora delas é idêntico à base e que o parágrafo da O256 foi restaurado literalmente. Suíte e RED [não verificado]. [cadastro.py:54](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/services/cadastro.py:54), [retrato_folha.py:90](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:90), [retrato_folha.py:1100](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:1100)

```diff
diff --git a/app/colaboradores/services/cadastro.py b/app/colaboradores/services/cadastro.py
index 196fb6eaf..e6e9c608c 100644
--- a/app/colaboradores/services/cadastro.py
+++ b/app/colaboradores/services/cadastro.py
@@ -51,7 +51,12 @@ NOME_NO_MODELO = {'codigo_folha': 'codigo_dominio'}
 
 
 def codigo_dominio_e_numerico(codigo):
-    """O código, após strip(), contém um ou mais dígitos ASCII."""
+    """Responde se o código contém um ou mais dígitos ASCII.
+
+    O chamador entrega o código já sem espaços após `strip()` em `colaboradores/views.py`
+    ou `folha/export.py`. O limite de 43 caracteres da linha do layout em `folha/export.py`
+    não é conferido aqui.
+    """
     return codigo.isascii() and codigo.isdigit()
 
 
diff --git a/app/folha/retrato_folha.py b/app/folha/retrato_folha.py
index e53c46436..cce2d3312 100644
--- a/app/folha/retrato_folha.py
+++ b/app/folha/retrato_folha.py
@@ -87,7 +87,10 @@ def rotulo_do_motivo(motivo):
 
 
 def detalhe_do_motivo(linha):
-    """Complemento do rótulo com o código da linha lavrada."""
+    """Para `codigo_dominio_invalido`, exibe o VALOR GRAVADO da chave `codigo` da linha lavrada,
+    quando presente, com o prefixo fixo `codigo Dominio = ` e sem gerar texto, conforme o aval
+    CODIGO-DOMINIO-VALIDO-2811-0115 para o Retrato e o PDF.
+    """
     if linha.get('motivo') == 'codigo_dominio_invalido' and linha.get('codigo'):
         return f"codigo Dominio = {linha['codigo']}"
     return ''
@@ -1094,8 +1097,14 @@ def o_que_fazer(linha, hoje=None):
     escolhe nada: cada linha e' (chave GRAVADA -> juiz que ja existe -> rotulo do mapa que ja
     existe).
 
-    Os valores sao ROTULOS de mapa, as mesmas palavras da coluna "Por quê" e das pilulas da barra.
-    O complemento do motivo é um dado da linha lavrada, exibido ao lado do rótulo.
+    NENHUM TEXTO SE GERA (ordem Ronald 13/10 01:3x, literal: *"montado SO das chaves gravadas na
+    linha do retrato (motivo, dono, tipo, datas), sem texto gerado"*). Os valores sao ROTULOS de
+    mapa -- as MESMAS palavras da coluna "Por quê" e das pilulas da barra --, nunca frase montada.
+    A frase e' o item (4) da mesma ordem (o Haiku por cima deste bloco), e escrever prosa aqui seria
+    fazer o (4) fora de ordem.
+    Pelo aval CODIGO-DOMINIO-VALIDO-2811-0115, a exceção é o campo `detalhe` do item 'motivo', que
+    exibe o VALOR GRAVADO da chave `codigo` da linha lavrada, com prefixo fixo e sem gerar texto,
+    conforme a exibição do código gravado no Retrato e no PDF.
 
     VALOR VAZIO CONTINUA APARECENDO, com o travessao: linha sem `dono` gravado (retrato legado) ja
     tem resposta pelos juizes (`fio_da_linha` cai no `_fio_cru`), e o que nao tem resposta e' um
```

**Casos RED e mutantes para o opus**

- **P1.** No Quem Cobrar, exigir que a coluna Folha mostre o rótulo de `codigo_dominio_invalido`. Mutante proposto: remover a entrada do mapa local para que a coluna mostre a chave crua. [relatorios/views.py:1729](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views.py:1729), [relatorios/views.py:1739](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views.py:1739)
- **P2.** No Raio-X do Retrato, renderizar uma retenção com `codigo` gravado como `VIGIA` e exigir `codigo Dominio = VIGIA`. Mutante proposto: remover a renderização de `l.detalhe_motivo` do template. [folha/retrato_folha.py:94](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:94), [templates/folha/_retrato_raiox.html:73](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_raiox.html:73)
- **Rescisão + `VIGIA`.** Para um colaborador que sai pelo módulo de rescisão e tem código `VIGIA`, exigir `status='fora'` e `motivo='rescisao_modulo_proprio'`. Mutante proposto: mover a guarda de código inválido para antes da guarda de rescisão. [folha/export.py:876](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/folha/export.py:876), [folha/export.py:886](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:886)

## 28/11 01:56 · sessao b70766398533 · CODIGO-DOMINIO-VALIDO · verificador · origem=Code

**Parecer 5a — a retenção funciona no caminho do TXT, com pendências no censo e na cobertura.** `classificar_export` continua sendo o juiz registrado e o montador pula o colaborador que ele marcou `fora`. A Prévia da proposta pode contar esse mesmo colaborador como alguém que passaria a entrar. Dois arquivos de produção ficaram sem teste que acuse a reversão da mudança. `app/core/juizes.py:331–337`, `app/folha/export.py:723–737,882–891`, `app/folha/previa.py:117–134,170–174`, `app/relatorios/tests/test_quem_cobrar_folha_juiz.py:34–66`, `app/folha/tests/test_retrato_raiox.py:134–141,162–207`.

Conferi em somente leitura as 15 pós-imagens pelos hashes de blob do `obra.diff` anexo e executei verificações isoladas em memória. Commit e ancestrais, reversões com a suíte Django, GREEN de 7.853 testes e ocorrência em produção [não verificado]. [nao verificado]

## 1. Escritores e leitores

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `codigo_folha` → `Colaborador.codigo_dominio` | O formulário de admissão lê o POST e o de edição exibe o valor da instância. As duas views atribuem o código aparado e persistem por `salvar_campos` ou `salvar_inteiro`. Na recusa, os aliases só preenchem o rascunho em memória. O Django Admin usa `SoLeitura`. `app/templates/colaboradores/admitir.html:58`, `app/templates/colaboradores/editar.html:62`, `app/colaboradores/views.py:1117–1139,1239–1245,1287–1298,1364–1375`, `app/core/reexibir.py:21–46`, `app/colaboradores/services/colaborador.py:140–160`, `app/colaboradores/admin.py:47–48`, `app/core/admin.py:53–75`. |
| Código aparado e matrícula do TXT | O juiz copia o valor aparado para o item. O montador relê o campo do colaborador e aplica `zfill(10)`. A prévia e o PDF de retidos mostram o código do item. O PDF de validação, o PDF histórico e a comparação histórica usam código aparado e preenchido para conciliar matrículas. O holerite configurável também pode casar por `codigo_dominio`. `app/folha/export.py:723–737,847–852`, `app/folha/views.py:424–430`, `app/folha/retidos_pdf.py:88–95`, `app/folha/validacao_pdf.py:65–72`, `app/folha/pdf_export.py:144–150`, `app/folha/haiku_export.py:109–117`, `app/holerite/views.py:1741`, `app/holerite/motor.py:158–164,216–225`. |
| `status`, chave `motivo`, texto `detalhe` e `travas_aprovacao` | `classificar_export` emite os quatro. O montador lê `status`. O POST usa elegibilidade e travas para o carimbo. Prévia, PDF de retidos e lista do fechamento leem a chave e o detalhe. `_K2` é a entrada no TXT e `_K1` é a pergunta de aprovação. Ambos apontam para a mesma função. `app/docs/JUIZES.md:119–122`, `app/core/juizes.py:331–337`, `app/folha/export.py:840–859,876–891`, `app/folha/views.py:223–276,356–374,424–430,885–886`, `app/folha/retidos_pdf.py:83–95`, `app/ponto/services/fechamento_lista.py:32–41,103–106`. |
| `RetratoFolha.linhas` e complemento | O lavrador persiste `codigo` aparado, `status`, chave `motivo`, `dono`, `tipo` e `saldo`. `TIPO_POR_MOTIVO` dá `cadastro`, que alimenta também o saldo, e `_fio_cru` dá `admin`. `detalhe_do_motivo` lê o código lavrado e monta `codigo Dominio = …`. Lista, PDF, Raio-X do Retrato e drawer individual leem essa projeção. `app/folha/retrato_folha.py:89–93,221–235,380–397,430–433,1121–1129,1243–1257`, `app/folha/management/commands/lavrar_retrato_folha.py:65–79`, `app/folha/views_retrato.py:194–201,286–293,421–433,508–515`, `app/colaboradores/views.py:997–1017`, `app/templates/folha/_retrato_lista.html:65`, `app/templates/folha/_retrato_raiox.html:73`, `app/templates/folha/_retrato_o_que_fazer.html:37`. |
| Mensagem da recusa | `_ERRO_CODIGO_DOMINIO` sai pelas duas views antes da criação ou da edição. `resposta_render` entrega a mensagem pelo Django messages ou pelo gatilho HTMX e registra a recusa pela porta existente. `app/colaboradores/views.py:1104,1135–1139,1205–1212,1295–1298,1365–1375`, `app/core/respostas.py:74–85`, `app/core/recusa_critica.py:70–86`. |

**Atribuição explícita e escrita física.** Admissão e edição são as atribuições explícitas encontradas. Vincular posto, transferir, desligar e reativar também podem regravar o campo por `save()` integral. Exemplo estático: a transferência carrega `VIGIA`, uma edição grava `00123`, e a transferência então adquire a trava e salva a instância antiga. A guarda relê a situação, portanto esse caminho pode restaurar `VIGIA`. Ocorrência em produção [não verificado]. `app/colaboradores/views.py:331–346,1239–1245,1365–1375,1410–1428`, `app/colaboradores/services/colaborador.py:84–91,152–160,184–200`, `app/colaboradores/services/desligamento.py:128–133,235–247`.

**Listas, rótulos e contagens.**

- **Cobertos:** `MOTIVO_RH` dá o rótulo à prévia, ao PDF de retidos e ao Retrato. O fechamento tem destino e ramo próprios. Pendências de Cadastro inclui a nova natureza para pessoas `apurado` e mostra a descrição e a recomendação. Quem Cobrar mostra o rótulo na coluna e no PDF. Sua contagem e seu filtro “Sem código” comparam o literal do rótulo antigo, então contam apenas ausentes. O Raio-X ao vivo conta a chave em “Outras pendências” e mostra o detalhe. `app/folha/views.py:15–25,374,415–430,616–628,663–668,718–720,885–886`, `app/folha/retidos_pdf.py:29–38,83–95`, `app/folha/retrato_folha.py:76–86`, `app/ponto/services/fechamento_lista.py:25–41`, `app/templates/ponto/_fech_linha.html:15–19`, `app/relatorios/views_pend.py:48–71,79–85`, `app/templates/relatorios/pendencias_cadastro.html:29,47–49`, `app/relatorios/views.py:1724–1746,1763–1767`, `app/templates/relatorios/ranking_furos.html:48,68,78`.
- **Chave crua em dois leitores.** A mensageria omite a chave em `EM_PORTUGUES` e devolve `codigo_dominio_invalido` ao copiloto pelo fallback. A API e o copiloto preservam a contagem. A Prévia da proposta imprime a chave no HTML e no CSV. Ela pode ainda somar em `mudariam_de_lado` um `VIGIA` com batida e proposta que cubra todos os dias acusados pelo espelho. A projeção considera os dias para qualquer item `fora`, e o cadastro continuaria barrando o TXT. Nesses dois caminhos, a chave nova chega por fallback ou cópia direta, sem `KeyError`. `mensageria/nucleo/ferramentas.py:1196–1203,1246–1253,1264–1267`, `app/api/views_mensageria.py:223–229,261–276,283–302`, `app/folha/views.py:758–771,812–820,835–839`, `app/folha/previa.py:97–134,170–174`, `app/folha/export.py:886–891`.
- **Outras contagens.** `fora_da_folha`, as métricas de prontidão, o simulador, o censo e o vigia preservam chaves dinamicamente. Já o contador cadastral `travam_a_folha` mede código faltante e deixa de contar `VIGIA` nesse recorte. O diagnóstico de cadastro da mensageria conta `NULL` e `''`, portanto também deixa de fora um código feito só de espaços. `app/folha/fora_da_folha.py:147–173,249`, `app/inteligencia/calculadores.py:210–234,271–296`, `app/folha/management/commands/simular_folha.py:82–90,133–152`, `bin/censo_folha.py:14–23,40–47`, `app/folha/services/vigia_txt.py:107–116,231–232`, `app/colaboradores/services/cadastro.py:79–92,115–134`, `app/api/views_mensageria.py:1275–1288`.

## 2. Precedência

A ordem efetiva é `rescisao_modulo_proprio` → `sem_codigo_dominio` → `codigo_dominio_invalido` → `ferias_com_batida` → `cadastro_zero` → `furo_espelho` → `nao_homologado` → `entra`. Demissão até o fim da janela pode coexistir com código inválido e vence. Ausência e inválido preenchido são mutuamente exclusivos após `strip()`. As condições posteriores podem coexistir com o inválido e perdem para ele. As travas de aprovação são calculadas antes da cadeia e seguem em campo próprio. `app/folha/export.py:54–60,840–859,876–900,918–996`.

**Contraexemplo concreto da fixture:** `VIGIA` com batida em 03/06 durante férias de 01/06 a 10/06 cairia em `ferias_com_batida` com o ramo novo removido. Com o ramo, cai em `codigo_dominio_invalido`. O controle `00123` com as mesmas férias e batida segue em `ferias_com_batida`. Equivalente em produção [não verificado]. `app/folha/export.py:882–900`, `app/folha/tests/test_codigo_dominio_invalido.py:128–157`.

## 3. Predicado e semântica

Os três chamadores fazem `strip()` e compartilham um único teste de conteúdo para código preenchido. O helper exige `isascii() and isdigit()` e recebe a string já aparada. `app/colaboradores/services/cadastro.py:53–55`, `app/colaboradores/views.py:1133–1139,1295–1298`, `app/folha/export.py:847–850,882–891`.

| Entrada | Juiz, após a guarda de rescisão | Admissão | Edição |
|---|---|---|---|
| `00123`, ` 00123 ` | Usa `00123` e segue os demais crivos. | Grava `00123`. | Grava `00123`. `app/folha/export.py:850,882–892`, `app/colaboradores/views.py:1135–1139,1239–1245,1295–1298,1364–1375`. |
| `00 123`, `VIGIA`, `１２３`, `²` | `codigo_dominio_invalido`. | Recusa com mensagem. | Recusa com mensagem. `app/colaboradores/services/cadastro.py:53–55`, `app/folha/export.py:886–891`, `app/colaboradores/views.py:1135–1139,1295–1298`. |
| `''`, só espaços, campo omitido | Código vazio dá `sem_codigo_dominio`. | Cadastro novo fica com o padrão `''`. | Conserva o código anterior, inclusive um inválido legado. `app/folha/export.py:850,882–885`, `app/colaboradores/models.py:311–314`, `app/colaboradores/views.py:1135–1139,1211–1245,1295–1298,1364–1375`. |
| `None` literal | Vira vazio. | Campo omitido cai no padrão `''`. Se um POST sintético devolver `None` literal, `.strip()` levanta `AttributeError`. | Vira vazio e conserva o anterior. `app/folha/export.py:850,882–885`, `app/colaboradores/views.py:1135,1295–1298,1364–1375`. |

**O layout tem `isdigit()` próprio.** Ele checa fatias do registro serializado, inclusive matrícula. O parser também tem essa checagem. Em memória, `１２３` e `²` foram rejeitados pelo helper cadastral e aceitos pelo layout de uma linha sintética de 43 caracteres. `[cpf]` passou pelo helper, produziu 44 caracteres e foi recusado pelo layout. O modelo declara até 20 caracteres. Portanto há um teste cadastral compartilhado e checagens de formato com domínio diferente. O código do Retrato é o valor aparado na lavra. A matrícula do TXT recebe preenchimento até dez caracteres, sem truncamento. `app/colaboradores/models.py:311–314`, `app/colaboradores/services/cadastro.py:53–55`, `app/folha/export.py:34–45,246–254,272–279,737,850`, `app/folha/retrato_folha.py:89–93,1243–1247`.

## 4. TXT, contadores, relógio e trava

- **O item com motivo novo não emite linha.** O juiz dá `fora`. `montar_linhas_export` forma `entra_set` apenas de `entra` e pula o colaborador antes da matrícula, dos eventos e das faltas. No POST, juiz e montador recebem os mesmos fechamentos em memória. O validador roda sobre as linhas restantes antes da entrega. Em chamadas do montador sem `fechamentos=`, a função consulta os fechamentos novamente e continua filtrando pelos IDs do veredito recebido. `app/folha/export.py:723–755,886–891`, `app/folha/views.py:215–223,273–289`.
- **Os totais têm universos diferentes.** A prévia de exportação conta todos os itens do juiz sobre `FechamentoMensal` gravado. O POST injeta o recálculo em memória, e o montador restringe a emissão a `aberto` e `aprovado`. Exemplo estático: um fechamento gravado `exportado` pode ser elegível na prévia e o montador que leia esse gravado pula o status. Além disso, `colabs` da trilha conta IDs elegíveis, `linhas` conta registros emitidos, e `retidos` da trilha e do aviso conta só `furo_espelho`. O motivo novo soma em “Ficam de fora” da prévia e fica fora desse contador do POST. Paridade na frota [não verificado]. `app/ponto/models.py:398–410`, `app/folha/export.py:696,723–748,793–798`, `app/folha/views.py:215–223,265–276,297–313,356–372,397–399`.
- **Relógio e ordem de trava.** Os ramos novos de validação e apresentação não introduzem leitura de relógio nem aquisição de trava. O fallback preexistente do retentor ainda pode ler `localdate()` abaixo da borda. No caminho de entrega, a ordem existente é transação → trava da empresa. A revisão de batidas ordena empresa → colaborador → batidas. A edição usa a trava do colaborador no serviço, com a ressalva da instância carregada antes da trava descrita acima. **`except` novo no diff anexo: 0.** `app/colaboradores/views.py:1135–1139,1295–1303,1371–1384`, `app/folha/export.py:556–562,886–891,984–989,1247–1254,1317–1323`, `app/folha/retrato_folha.py:89–93`, `app/folha/views.py:103–105,133–136`, `app/ponto/services/par_relampago_na_lista.py:172–187`, `app/colaboradores/services/colaborador.py:72–91,152–160`.

## 5. Mordida por arquivo

A tabela indica a falha esperada ao retirar **um arquivo de produção por vez**, mantendo os REDs. Conferi em memória as mutações simples dos mapas e a busca do ramo do template de Ponto. Reversão integral por arquivo com execução Django [não verificado]. `app/folha/tests/test_codigo_dominio_invalido.py:163–200`.

| Arquivo | Teste ou falha esperada | [nao verificado]
|---|---|
| `app/colaboradores/services/cadastro.py:53–55` | A retirada do helper quebra os imports. Os REDs de admissão, edição e juiz dão erro antes da asserção. `app/colaboradores/views.py:14`, `app/folha/export.py:791`, `app/colaboradores/tests/test_codigo_dominio_numerico.py:85–92,118–125`, `app/folha/tests/test_codigo_dominio_invalido.py:90–94`. |
| `app/colaboradores/views.py:1135–1139,1295–1298` | REDs da admissão e edição, que exigem recusa e integridade dos registros. `app/colaboradores/tests/test_codigo_dominio_numerico.py:85–92,118–125`. |
| `app/folha/export.py:882–891` | REDs de letras, TXT, Unicode, precedência e presença da chave na fonte por AST. `app/folha/tests/test_codigo_dominio_invalido.py:90–105,122–125,152–157,194–200`. |
| `app/folha/retrato_folha.py:89–93,221–227,1123–1125` | A reversão integral quebra o import em `app/folha/views_retrato.py:49`. A retirada isolada do destino também falha no RED de tipo e no selo de exaustividade. `app/folha/tests/test_codigo_dominio_invalido.py:168–171,194–200`, `app/folha/tests/test_retrato_folha_quadros.py:323–329`. |
| `app/folha/views.py:15–18` | RED de presença e texto do rótulo. `app/folha/tests/test_codigo_dominio_invalido.py:163–166`. |
| `app/folha/views_retrato.py:291–292,423–426,511–512` | REDs de lista e PDF. A projeção do Raio-X fica sem mordida própria. `app/folha/tests/test_codigo_dominio_invalido.py:246–278`, `app/folha/tests/test_retrato_raiox.py:134–141,162–207`. |
| `app/ponto/services/fechamento_lista.py:25–41` | RED da pílula falha na primeira asserção, ao receber `furo_espelho`. `app/folha/tests/test_codigo_dominio_invalido.py:177–183`. |
| `app/relatorios/views.py:1725–1739` | **Sem mordida encontrada.** O teste de Quem Cobrar usa código `77` e cobre rescisão, apto e sem fechamento. `app/relatorios/tests/test_quem_cobrar_folha_juiz.py:39–46,59–66`. |
| `app/relatorios/views_pend.py:48–51` | RED da presença da chave no mapa. `app/folha/tests/test_codigo_dominio_invalido.py:190–192`. |
| `app/templates/folha/_retrato_lista.html:65` | RED exige `VIGIA` lavrado no HTML depois da troca cadastral. `app/folha/tests/test_codigo_dominio_invalido.py:237–256`. |
| `app/templates/folha/_retrato_o_que_fazer.html:37` | RED do drawer exige a frase lavrada no HTML. `app/folha/tests/test_codigo_dominio_invalido.py:280–292`. |
| `app/templates/folha/_retrato_raiox.html:73` | **Sem mordida encontrada.** As fixtures da rota têm `codigo=''` e não exercitam a chave nova. `app/folha/tests/test_retrato_raiox.py:38,134–141,162–207`, `app/folha/tests/test_retrato_folha_quadros.py:42–50,86–97`. |
| `app/templates/ponto/_fech_linha.html:15–19` | RED da pílula falha na segunda asserção, que procura a chave por regex. `app/folha/tests/test_codigo_dominio_invalido.py:184–188`. |

O selo de relógio acusou uma chamada direta no controle sintético e retornou zero novos soltos na varredura isolada em memória. Ele aceita um marcador de congelamento no arquivo inteiro e não acompanha relógios acionados indiretamente. A fixture da lavratura congela o tempo. O RED do PDF verifica a *story* do ReportLab e o HTTP 200. Corte visual do PDF [não verificado]. `app/core/tests/test_selo_teste_sem_relogio.py:23–45,48–84`, `app/folha/tests/test_codigo_dominio_invalido.py:203–212,215–229,258–278`.

## 28/11 01:54 · sessao a4b647160404 · - · - · origem=Ronald (tmux)

- **Precedência confirmada.** Rescisão vence quando a demissão é até o fim da janela. Vazio após `strip()` e inválido preenchido são mutuamente exclusivos. O motivo novo prevalece sobre férias com batida, cadastro zero, furo de espelho e rubrica não homologada. A fixture `VIGIA` com batida em 03/06 durante férias de 01/06 a 10/06 muda de `ferias_com_batida` para `codigo_dominio_invalido`. O controle `00123` continua em férias com batida. [app/folha/export.py:54–60, 876–995; app/folha/tests/test_codigo_dominio_invalido.py:128–157]

- **Predicado compartilhado e comportamentos de vazio diferentes.** Os três chamadores aparam as bordas e usam `isascii() and isdigit()` para código preenchido. ` 00123 ` preserva `00123`, `1 23`, `１２３` e `²` são inválidos. Vazio, espaços e ausência deixam o padrão `''` na admissão, preservam o anterior na edição e produzem `sem_codigo_dominio` no juiz quando a rescisão não prevalece. `None` literal é normalizado no juiz e na edição. A admissão usa `.get(..., '').strip()`, cujo padrão trata a chave ausente, e falharia se um POST sintético entregasse `None` literal. [app/colaboradores/services/cadastro.py:53–55; app/colaboradores/models.py:311–314; app/colaboradores/views.py:1135–1139, 1239–1245, 1295–1298, 1364–1375; app/folha/export.py:850, 876–891]

- **Juiz único, validador de layout com predicado diferente.** `_K2` registra `classificar_export`. O layout verifica `.isdigit()` sobre a matrícula serializada, e o parser também. A execução isolada das funções extraídas em memória confirmou que o layout aceita matrículas de largura total e sobrescrito rejeitadas no cadastro. `[cpf]` passa no cadastro e pode produzir registro de 44 caracteres, que o layout rejeita. O Retrato mostra o código aparado que foi lavrado e o TXT preenche à esquerda a partir do campo de cadastro. [app/docs/JUIZES.md:119–121; app/core/juizes.py:181, 336–337; app/folha/export.py:34–45, 247–254, 272–279, 737, 850; app/folha/retrato_folha.py:89–93, 1243–1247]

- **Há outros escritores físicos.** Vincular e transferir posto e desligar efetivamente passam por `salvar_inteiro`, que usa `save()` sem `update_fields`. Reativação também usa `save()` completo. A porta guarda `situacao` após travar, sem atualizar `codigo_dominio` da instância. Uma transferência que carregue `VIGIA` antes de uma edição que corrige e grava o código pode restaurar `VIGIA` ao gravar a instância antiga. [app/colaboradores/views.py:331–346, 1410–1428; app/colaboradores/services/colaborador.py:84–91, 152–160, 184–200; app/colaboradores/services/desligamento.py:128–133, 235–247]

- **Prévia da proposta tem uma lacuna funcional.** Exibe a chave crua no CSV e no HTML. Sua contagem `mudariam_de_lado` pode incluir pessoa com código inválido se todos os dias de retenção da célula estiverem cobertos pela proposta, pois só testa os dias. O código permaneceria inválido no juiz do TXT. [app/folha/previa.py:97–103, 117–134, 150–174; app/folha/views.py:763–771, 815–817, 835–839; app/folha/export.py:886–891]

Os 15 blobs finais da cópia coincidem com os prefixos do anexo. Commit, ancestrais, suíte e ocorrência em produção [não verificado]. [nao verificado]

## 28/11 01:54 · sessao a3d93132ff87 · - · - · origem=Ronald (tmux)

**Três leitores laterais merecem destaque.**

- O copiloto recebe `codigo_dominio_invalido` cru. `EM_PORTUGUES` omite a chave e o fallback devolve a própria chave. A contagem inclui o caso entre os bloqueios e na soma da frota. A API preserva motivos dinamicamente nas respostas por empresa, por frota e na leitura ao vivo. `mensageria/nucleo/ferramentas.py:1196-1203,1246-1253,1264-1267,1297-1304`; `app/api/views_mensageria.py:223-229,261-276,283-302`.
- A **Prévia da proposta** também mostra a chave crua em HTML e CSV. Um colaborador com `VIGIA`, batida e propostas cobrindo todos os dias que o espelho acusa pode aparecer como `passaria a entrar` e somar em `mudariam_de_lado`. A projeção testa o espelho para qualquer item `fora`, sem considerar o motivo principal. Corrigir esses dias ainda deixa a retenção de cadastro ativa. `app/folha/views.py:758-771,812-820,835-839`; `app/folha/previa.py:97-122,133,170-174`; `app/folha/export.py:886-891`.
- Dois leitores de cadastro medem ausência. O contador `travam_a_folha` marca só `codigo_folha` faltante e pode deixar de contar um ativo com `VIGIA`. `_cadastro_incompleto` usa SQL `NULL` ou `''`, portanto deixa de fora até um código composto só de espaços. Esses leitores partem do cadastro ativo, com recortes próprios. `app/colaboradores/services/cadastro.py:26-30,79-92,104-134`; `app/api/views_mensageria.py:1275-1288,1703-1724`.

**Campo, autoridade e apresentação**

- Admissão e edição são as atribuições explícitas de `Colaborador.codigo_dominio` encontradas. `salvar_campos` persiste a lista de campos na admissão e `salvar_inteiro` persiste o objeto completo na edição. O Django Admin lê o campo pelo `SoLeitura`. Operações de posto, desligamento e reativação também regravam o objeto inteiro pela mesma porta. A trava relê `situacao`, então uma cópia carregada antes de uma edição concorrente pode regravar o código anterior. `app/colaboradores/views.py:346,1239-1245,1365-1375,1428`; `app/colaboradores/services/colaborador.py:55-69,84-91,140-160,184-200`; `app/colaboradores/services/desligamento.py:133,247`; `app/colaboradores/admin.py:47-48`; `app/core/admin.py:53-75`.
- A frase de recusa vem de `_ERRO_CODIGO_DOMINIO`. O envelope a envia ao Django messages ou ao evento HX e à trilha `ponto.recusa_critica`. Os retornos acontecem antes de criar ou editar o cadastro. `app/colaboradores/views.py:1104,1135-1139,1205-1212,1295-1298,1365-1375`; `app/core/respostas.py:74-85`; `app/core/recusa_critica.py:34,70-86`.
- `classificar_export` é `_K2`, a autoridade do TXT, e `_K1` para a pergunta distinta das travas de aprovação. Ele emite `status`, a chave `motivo` e o texto `detalhe`, além do código aparado. O lavrador copia código aparado, status e chave para `RetratoFolha.linhas` e grava dono, tipo e saldo. `TIPO_POR_MOTIVO` mapeia a chave para `cadastro`, também lido por `MOTIVOS_DE_CADASTRO` e `_saldo_cru`. `_fio_cru` resolve Admin para essa retenção. `app/docs/JUIZES.md:119-122`; `app/core/juizes.py:336-337`; `app/folha/export.py:847-859,882-891`; `app/folha/retrato_folha.py:221-235,380-397,430-433,1218-1257`; `app/folha/management/commands/lavrar_retrato_folha.py:65,72-79`.
- A prévia de exportação e o PDF de retidos agrupam por chave presente e usam `MOTIVO_RH`. O Retrato traduz com `rotulo_do_motivo` e forma o complemento a partir de `linha['codigo']` da lavra, usado em lista, PDF, Raio-X do Retrato e drawer individual. `app/folha/views.py:15-24,356-374,415-430,885-886`; `app/folha/retidos_pdf.py:29-38,83-95`; `app/folha/retrato_folha.py:76-93,1121-1125`; `app/folha/views_retrato.py:194-201,286-293,421-433,508-515`; `app/colaboradores/views.py:997-1017`; `app/templates/folha/_retrato_lista.html:65`; `app/templates/folha/_retrato_raiox.html:73`; `app/templates/folha/_retrato_o_que_fazer.html:37`.
- A lista do fechamento leva a chave e o detalhe à pílula própria. O Quem Cobrar usa o rótulo na coluna e no PDF, e a contagem e filtro “Sem código” comparam literalmente o rótulo antigo, delimitando os ausentes. Pendências de Cadastro usa a nova natureza `CODIGO_DOMINIO_INVALIDO` para linhas `apurado`, e renderiza natureza, descrição e recomendação. O Raio-X ao vivo usa o complemento de `_COM_PILULA`, contando o caso em “Outras pendências” e exibindo o detalhe. `app/ponto/services/fechamento_lista.py:25-41`; `app/templates/ponto/_fech_linha.html:15-19`; `app/relatorios/views.py:1724-1746,1763-1767`; `app/templates/relatorios/ranking_furos.html:48,68,78,104`; `app/relatorios/views_pend.py:48-71,79-85`; `app/templates/relatorios/pendencias_cadastro.html:25-29,47-49`; `app/folha/views.py:616-628,663-668,718-720`.
- Agrupam ou registram a chave dinamicamente `fora_da_folha`, as duas métricas de prontidão, simulador, censo e vigia TXT. Diagnósticos de ponto consomem o par status/motivo em mapas dinâmicos. `app/folha/fora_da_folha.py:147-155,173,249,273,303-304`; `app/inteligencia/calculadores.py:210-234,271-296`; `app/folha/management/commands/simular_folha.py:89-90,133-146,208-217,260`; `bin/censo_folha.py:14-23,40-47`; `app/folha/services/vigia_txt.py:98-116,168-176,231-232`; `app/ponto/management/commands/celula_veredito_velho.py:81-82,151-152`; `app/ponto/management/commands/diff_reclassificar_partido.py:152-153,208-209`; `app/ponto/management/commands/aplicar_09_corte_b.py:189-190,272-273`; `app/ponto/management/commands/desvio_o68b.py:190-191,216-220`.
- Leitores laterais do código incluem o PDF do TXT histórico, a comparação histórica, o PDF de validação e o match configurável de holerite. O parser Domínio padrão usa nome. `app/folha/pdf_export.py:144-150`; `app/folha/haiku_export.py:109-117`; `app/folha/validacao_pdf.py:65-72,91-103`; `app/holerite/views.py:1741`; `app/holerite/motor.py:158-164,216-225`.

## 28/11 01:53 · sessao 5302d656a3d6 · - · - · origem=Ronald (tmux)

Dos 13 arquivos de produção do diff anexo, 11 têm uma falha identificável sob reversão isolada. As duas lacunas são Quem Cobrar e o template do Raio-X do Retrato. As falhas abaixo vêm da leitura do diff e dos testes. A execução integral e o job informado ficam [não verificado]. [nao verificado]

| Arquivo revertido | Teste que acusa a reversão | [nao verificado]
|---|---|
| `app/colaboradores/services/cadastro.py:53` | A retirada do símbolo quebra o import da view e do juiz. Os REDs de admissão, edição e classificação dão erro, antes da asserção. `app/colaboradores/views.py:14`, `app/folha/export.py:791`, `app/colaboradores/tests/test_codigo_dominio_numerico.py:85-92,118-125`, `app/folha/tests/test_codigo_dominio_invalido.py:90-94`. |
| `app/colaboradores/views.py:1135-1139,1295-1298` | REDs da admissão e da edição verificam recusa e integridade dos registros. `app/colaboradores/tests/test_codigo_dominio_numerico.py:85-92,118-125`. |
| `app/folha/export.py:882-891` | REDs de letras, TXT, Unicode, precedência e enumeração AST do motivo. `app/folha/tests/test_codigo_dominio_invalido.py:90-105,122-125,152-157,194-200`. |
| `app/folha/retrato_folha.py:89-93,221-227,1123-1125` | RED do tipo e selo do destino, além do erro na importação do complemento pela view. `app/folha/tests/test_codigo_dominio_invalido.py:168-171,194-200`, `app/folha/tests/test_retrato_folha_quadros.py:323-328`, `app/folha/views_retrato.py:49`. |
| `app/folha/views.py:15-18` | RED da presença e do texto do rótulo. `app/folha/tests/test_codigo_dominio_invalido.py:163-166`. |
| `app/folha/views_retrato.py:291-292,423-426,511-512` | REDs de lista e PDF cobrem as duas primeiras projeções. O trecho do Raio-X fica sem mordida própria. `app/folha/tests/test_codigo_dominio_invalido.py:246-278`, `app/folha/tests/test_retrato_raiox.py:134-141,162-207`. |
| `app/ponto/services/fechamento_lista.py:25-41` | RED da pílula falha na primeira asserção, ao receber `furo_espelho`. `app/folha/tests/test_codigo_dominio_invalido.py:177-183`. |
| `app/relatorios/views.py:1725-1739` | Sem teste identificado para a entrada nova. O teste da coluna Folha usa código `77` em todos os colaboradores e confere rescisão, apto e sem fechamento. `app/relatorios/tests/test_quem_cobrar_folha_juiz.py:39-46,59-66`. |
| `app/relatorios/views_pend.py:48-51` | RED da presença da chave no mapa de cadastro. `app/folha/tests/test_codigo_dominio_invalido.py:190-192`. |
| `app/templates/folha/_retrato_lista.html:65` | RED da lista exige `VIGIA` lavrado após trocar o cadastro. `app/folha/tests/test_codigo_dominio_invalido.py:237-256`. |
| `app/templates/folha/_retrato_o_que_fazer.html:37` | RED do painel exige a frase lavrada no HTML. `app/folha/tests/test_codigo_dominio_invalido.py:280-292`. |
| `app/templates/folha/_retrato_raiox.html:73` | Sem teste identificado para o complemento. A rota carrega `LINHAS`, com `codigo=''` e sem a chave nova. `app/folha/tests/test_retrato_raiox.py:38,134-141,162-207`, `app/folha/tests/test_retrato_folha_quadros.py:42-50,86-97`. |
| `app/templates/ponto/_fech_linha.html:15-19` | O mesmo RED da pílula falha na segunda asserção, que busca a chave declarada por regex. `app/folha/tests/test_codigo_dominio_invalido.py:184-188`. |

Confirmei em memória as cinco mutações simples dos mapas e do template de Ponto nas asserções indicadas. O RED do PDF inspeciona a *story* do ReportLab e a resposta 200. Corte visual do PDF [não verificado]. O RED do drawer confirma `Admin` na saída do helper e a frase no HTML. A pílula de Ponto tem selo estrutural por regex. `app/folha/tests/test_codigo_dominio_invalido.py:163-200,203-212,265-292`.

A lógica do selo do relógio, executada isoladamente em memória, encontrou os mesmos 452 arquivos da lista, com zero novos e zero curados. O controle com `datetime.date.today()` acusou uma leitura. O detector conta chamadas diretas sem argumento, aceita `freeze_time` por texto no arquivo inteiro e não acompanha comandos ou requisições chamados pelo teste. O RED da lavratura congela o relógio explicitamente. `app/core/tests/test_selo_teste_sem_relogio.py:23-45,48-84`, `app/folha/tests/test_codigo_dominio_invalido.py:215-229,249-250`.

## 28/11 01:43 · sessao db69fbcafc44 · FERIADO-UM-JUIZ · leitura · origem=Code

**O DIFF deve rodar duas vezes sobre a mesma sombra**. Monte o `app/` da árvore viva para o **ANTES** e o `app/` de `d765154a3` para o **DEPOIS**, via `SONDA_ARVORE`. O gravado da sombra é a terceira coluna, usada para estimar o que o apply escreveria. Deltas reais, lista do TXT e hashes de hoje seguem [não verificado]. `bin/sonda_leitura.sh:226-240` `bin/diff_o124_frota.py:4-14` `app/ponto/services/diff_frota.py:7-12`

### 1. Instrumento e universo

- **`bin/diff_o124_frota.py` é o precedente para comparar duas árvores.** Ele roda o escritor real em modo leitura em cada montagem e compara as duas fotos por colaborador e campo. O script original fixa uma competência por rodada e chama o recalculador sem filtro de empresa. **`diff_frota.medir`** é o serviço da casa para gravado, hoje e novo quando a mudança é uma mutação dentro de uma única árvore. Ele parte de quem já tem mensal gravado. `bin/diff_o124_frota.py:10-14` `bin/diff_o124_frota.py:20-42` `bin/diff_o124_frota.py:47-69` `app/ponto/services/diff_frota.py:45-89`
- **`diff_calculador` compara motor e calculador por dia e rubrica dentro do código carregado.** Recebe um par `--mes/--ano` por chamada, percorre todas as empresas ativas e parte dos IDs com mensal gravado. `--contra-gravado` acrescenta a comparação do calculador com o mensal. `--periodos-do-fechamento` captura os períodos da mesma árvore e intercepta a lavratura. `app/ponto/management/commands/diff_calculador.py:25-56` `app/ponto/management/commands/diff_calculador.py:98-135` `app/ponto/management/commands/diff_calculador.py:541-588`
- **`sombra.sh --cobre INI FIM` consulta o carimbo de cobertura.** Use a janela de cada empresa, incluindo o dia adicional em cada ponta que o fechamento consulta. A vigência começa em 11/2026 e alcança 12/2026. Com corte 21, 20/11 é a véspera da janela da 12 e entra na busca de batidas. O corte efetivo das três empresas e a cobertura de hoje seguem [não verificado]. `bin/sombra.sh:219-233` `app/core/sombra_cobertura.py:32-49` `app/ponto/janelas.py:10-21` `app/ponto/janelas.py:54-57` `app/ponto/services/fechamento.py:155-173` `app/ponto/services/fechamento.py:601-604` `app/ponto/services/fechamento.py:685-699` `app/folha/export.py:383-391`

### 2. Sonda de impacto

`--sombra` seleciona `sombra_db/sombra`. Por padrão, a sonda monta `$RAIZ/app:/app:ro`, onde `$RAIZ` é a raiz do **wrapper chamado**. A opção de árvore é `SONDA_ARVORE=<cópia>/app`. O `--dir` é da suíte. O HEAD vivo `93e305860` e a disponibilidade dessa opção no wrapper instalado seguem [não verificado]. `bin/sonda_leitura.sh:31` `bin/sonda_leitura.sh:38-45` `bin/sonda_leitura.sh:66-71` `bin/sonda_leitura.sh:93-97` `bin/sonda_leitura.sh:226-240` `bin/suite.sh:170`

A sonda usa o universo integral do recalculador para as empresas 2, 3 e 4 nas competências 11 e 12, inclusive quem ainda está sem mensal. Os totais incluem diferenças de **0,01 h** e mostram quantos valores preenchidos entraram em cada soma. No TXT 11, segue a chamada do gerador a `classificar_export` sobre os mensais em memória. A marca `ITEM_8` mostra quando zerar só a HE100 feriado numa cópia em memória devolve o motivo `cadastro_zero`. `SEM_CELULA` distingue esse caso de cadastro sem previsto com célula. A coluna `TXT` preserva o veredito real, inclusive quando uma retenção posterior ainda barra o colaborador. `app/ponto/services/fechamento.py:490-507` `app/ponto/services/fechamento.py:619-622` `app/ponto/services/fechamento.py:962-973` `app/ponto/services/fechamento.py:1016-1021` `app/ponto/management/commands/recalcular_fechamento.py:54-55` `app/folha/views.py:214-222` `app/folha/export.py:924-926` `app/folha/export.py:948-1005`

Conteúdo de `impacto_feriado.py` para a sessão. Sintaxe, comparador e projeção do item 8 conferidos com dados sintéticos. Execução com Django e a sombra real [não verificado].

```python
import json
import sys
from copy import copy
from decimal import Decimal

ANO, MESES, EMPRESAS = 2026, (11, 12), (2, 3, 4)


def projetar(registros, campos):
    saida = {}
    for f in registros:
        valores = (getattr(f, campo) for campo in campos)
        saida[str(f.colaborador_id)] = [None if v is None else str(v) for v in valores]
    return saida


def vereditos(empresa, fechamentos):
    from folha.export import classificar_export

    itens = classificar_export(empresa, 11, ANO, fechamentos=fechamentos)
    candidatos = []
    for f in fechamentos:
        if f.horas_trabalhadas == 0 and f.horas_extras_100_feriado > 0:
            copia = copy(f)
            copia.horas_extras_100_feriado = 0
            candidatos.append(copia)
    retidos_sem_he = {
        i["colaborador_id"]: (
            "SEM_CELULA" if i["detalhe"].startswith("Sem NENHUMA celula") else "SEM_PREVISTO"
        )
        for i in classificar_export(empresa, 11, ANO, fechamentos=candidatos)
        if i["motivo"] == "cadastro_zero"
    }
    return {
        str(i["colaborador_id"]): [
            i["status"], i["motivo"],
            retidos_sem_he.get(i["colaborador_id"]) if i["motivo"] != "cadastro_zero" else None,
        ]
        for i in itens
    }


def coletar():
    from django.db import connection, transaction

    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cur.execute("SELECT current_database(), current_schema(), current_user, "
                        "current_setting('transaction_read_only')")
            if cur.fetchone() != ("sombra", "juliani", "leitor", "on"):
                raise RuntimeError("Sonda exige sombra/juliani com leitor em READ ONLY")

        from colaboradores.models import Empresa
        from ponto.janelas import janela_fechamento
        from ponto.management.commands.recalcular_fechamento import RUBRICAS
        from ponto.models import FechamentoMensal
        from ponto.services.fechamento import recalcular_fechamento_mes, universo_da_competencia

        dados = {}
        for mes in MESES:
            for eid in EMPRESAS:
                empresa = Empresa.objects.get(pk=eid)
                janela = janela_fechamento(mes, ANO, empresa)
                gravado = FechamentoMensal.objects.filter(
                    mes=mes, ano=ANO, colaborador__empresa_id=eid
                )
                lidos = recalcular_fechamento_mes(
                    mes, ANO, empresa_id=eid, somente_leitura=True,
                    retornar_objeto=True, lavrar_dias_pagos=False, propagar_falha=True,
                )
                esperados = set(universo_da_competencia(mes, ANO, eid).values_list("pk", flat=True))
                if lidos.keys() != esperados:
                    raise RuntimeError(f"Leitura incompleta emp{eid} {mes:02d}/{ANO}")
                dados[f"{mes:02d}/{ANO} emp{eid}"] = {
                    "janela": [d.isoformat() for d in janela],
                    "gravado": projetar(gravado, RUBRICAS),
                    "lido": projetar(lidos.values(), RUBRICAS),
                    "txt": vereditos(empresa, list(lidos.values())) if mes == 11 else {},
                }
        transaction.set_rollback(True)

    print("IMPACTO_JSON", json.dumps({"campos": RUBRICAS, "dados": dados}, sort_keys=True))


def delta(antes, depois):
    return None if antes is None or depois is None else Decimal(depois) - Decimal(antes)


def linha(*valores):
    print(*(v if v is not None else "-" for v in valores), sep="\t")


def comparar(antes, depois):
    campos = antes["campos"]
    a, b = antes["dados"], depois["dados"]
    if campos != depois["campos"] or a.keys() != b.keys():
        raise SystemExit("Escopos ou rubricas de comparação diferentes")
    if any(a[k]["gravado"] != b[k]["gravado"] or a[k]["janela"] != b[k]["janela"] for k in a):
        raise SystemExit("O gravado ou a janela mudou entre as leituras")

    linha("FOLHA", "comp empresa", "colaborador", "rubrica",
          "gravado", "antes", "depois", "efeito", "delta_apply")
    for grupo in sorted(a):
        linha("JANELA", grupo, *a[grupo]["janela"])
        gravado, velho, novo = a[grupo]["gravado"], a[grupo]["lido"], b[grupo]["lido"]
        fotos = (gravado, velho, novo)
        vazio = [None] * len(campos)
        for cid in sorted(gravado.keys() | velho.keys() | novo.keys(), key=int):
            col = f"col{int(cid):03d}"
            if not all(cid in foto for foto in fotos):
                linha("UNIVERSO", grupo, col, *(cid in foto for foto in fotos))
            for pos, campo in enumerate(campos):
                g, v, n = (foto.get(cid, vazio)[pos] for foto in fotos)
                linha("FOLHA", grupo, col, campo, g, v, n, delta(v, n), delta(g, n))

        for pos, campo in enumerate(campos):
            colunas = [[Decimal(v[pos]) for v in foto.values() if v[pos] is not None] for foto in fotos]
            g, v, n = (sum(valores, Decimal(0)) for valores in colunas)
            linha("TOTAL", grupo, "frota", campo, g, v, n, n - v, n - g,
                  *(len(valores) for valores in colunas))

        ta, tb = a[grupo]["txt"], b[grupo]["txt"]
        for cid in sorted(ta.keys() | tb.keys(), key=int):
            va = ta.get(cid, ["sem_fechamento", None, None])
            vb = tb.get(cid, ["sem_fechamento", None, None])
            movimento = "ENTROU" if vb[0] == "entra" and va[0] != "entra" else (
                "SAIU" if va[0] == "entra" and vb[0] != "entra" else "-"
            )
            linha("TXT", grupo, f"col{int(cid):03d}", movimento, *va[:2], *vb[:2])
            if va[2] or vb[2]:
                linha("ITEM_8", grupo, f"col{int(cid):03d}", movimento, va[2], vb[2])


if sys.argv[1:] == ["--comparar"]:
    comparar(json.loads(next(sys.stdin)), json.loads(next(sys.stdin)))
else:
    coletar()
```

As duas rodadas devem usar a mesma foto da sombra, sem refresh ou outro escritor no intervalo. A igualdade do gravado e das janelas detecta parte das mudanças de entrada. Cada rodada tem seu próprio snapshot e o fechamento usa o relógio para o teto de furos, relevante na 12 em curso. `app/ponto/services/fechamento.py:679-680` `app/ponto/services/fechamento.py:1147-1154` `bin/diff_o124_frota.py:10-14`

**Há uma diferença operacional importante.** O Python acima abre `READ ONLY` e termina com rollback. O wrapper provisiona role e GRANTs, reescreve `logs/.env_leitor`, copia a sonda para um temporário e grava trilha. Se a proibição de escrita inclui esses atos administrativos, use a montagem direta abaixo com leitor e rede **já provisionados**. Ela também evita depender de `app/logs` e `app/media` na cópia, usados como pontos de montagem pelo wrapper. A disponibilidade desses pontos e do leitor provisionado na sessão segue [não verificado]. `bin/sonda_leitura.sh:115-181` `bin/sonda_leitura.sh:184-203` `bin/sonda_leitura.sh:216-219` `bin/sonda_leitura.sh:245-250` `bin/sombra.sh:180-191`

```bash
(
  set -euo pipefail
  viva=/home/ronald/saas-hasner
  obra=/home/ronald/codex_ws/d765154a302318df66e6fd7c1a8ab01dab660004
  sonda=/caminho/da/sessao/impacto_feriado.py

  test "$(git -C "$viva" rev-parse --short=9 HEAD)" = 93e305860

  rodar() {
    SONDA_ARVORE="$1" bash "$viva/bin/sonda_leitura.sh" --sombra "$sonda"
  }

  medir() {
    rodar "$1" 2>&1 | awk '
      /^IMPACTO_JSON / { sub(/^IMPACTO_JSON /, ""); print; n++ }
      END { if (n != 1) exit 1 }
    '
  }

  antes=$(medir "$viva/app") || { echo "Falhou a leitura ANTES" >&2; exit 1; }
  depois=$(medir "$obra/app") || { echo "Falhou a leitura DEPOIS" >&2; exit 1; }
  printf '%s\n%s\n' "$antes" "$depois" | python3 -B "$sonda" --comparar
)
```

Para a montagem direta, substitua `rodar` no bloco acima pela função a seguir. Ela usa a imagem, a rede e a credencial de leitor da sonda, com a árvore em `/codigo:ro` e logs em tmpfs. Execução desse comando na sessão [não verificado]. `bin/sonda_leitura.sh:42-51` `bin/sonda_leitura.sh:232-240` `bin/recursos.sh:81` `app/config/settings/sombra.py:47-51` `app/config/settings/saas.py:118-143`

```bash
rodar() {
  (
    source "$viva/bin/recursos.sh"
    docker run --rm --read-only $TESTE_DOCKER \
      --network sonda_leitura --env-file "$viva/logs/.env_leitor" \
      -e POSTGRES_DB=sombra -e TZ=America/Sao_Paulo -e PYTHONDONTWRITEBYTECODE=1 \
      --mount "type=bind,src=$1,dst=/codigo,readonly" \
      --mount "type=bind,src=$sonda,dst=/sonda.py,readonly" \
      --tmpfs /app/logs --tmpfs /app/media --workdir /codigo \
      saas-hasner-core:latest \
      python /codigo/manage.py tenant_command shell --schema=juliani \
      --settings=config.settings.sombra \
      --command "exec(open('/sonda.py').read())"
  )
}
```

### 3. Reversão e apply da 11

- **Escritor genérico da foto pré-apply.** `carimbo_gravado --empresa ID --mes 11 --ano 2026` usa `foto()` e grava valores por colaborador e campo em `logs/carimbo_gravado/`. A obra da janela de HE é um precedente explícito. Seu apply grava as fotos `antes` em `logs/janela_he/` antes de alterar cadastro e chamar o recalculador. `recalcular_fechamento --apply` só grava seu próprio JSON **depois** do recálculo. `app/ponto/management/commands/carimbo_gravado.py:29-56` `app/ponto/management/commands/carimbo_gravado.py:101-113` `app/ponto/management/commands/carimbo_gravado.py:145-155` `app/ponto/management/commands/aplicar_janela_he_total.py:75-90` `app/ponto/management/commands/aplicar_janela_he_total.py:116-146` `app/ponto/management/commands/recalcular_fechamento.py:130-151`
- **Precedentes de reversão em `logs/`.** `bin/snapshot_relavratura_10_2026.py` escreve `/app/logs/reversao_relavratura_10_2026.json` e `bin/snapshot_o263_comp10.py` escreve `/app/logs/reversao_o263_comp10.json`. Ambos guardam mensal, `DiaPago.motor` e IDs sem mensal anterior. Para esta obra, a foto precisa cobrir também `DiaPago.oraculo`, pois a porta relavra as duas versões. `restaurar_fechamento` repõe campos de linhas mensais existentes. A existência de um arquivo de reversão específico da 11 em `logs/*/reversao*` segue [não verificado]. `bin/snapshot_relavratura_10_2026.py:25-64` `bin/snapshot_o263_comp10.py:53-84` `app/ponto/services/fechamento.py:1235-1239` `app/ponto/services/fechamento.py:1262-1266` `app/ponto/services/fechamento.py:1300-1319` `app/ponto/services/dia_pago.py:368-373` `app/ponto/services/fechamento.py:1398-1437`
- **Porta do apply.** É `tenant_command recalcular_fechamento --schema=juliani --mes 11 --ano 2026 --empresa ID --apply`, uma chamada por empresa do escopo. A chamada encaminha apenas `(11, 2026, ID)` e grava mensal e lavra nessa chave. A 12 pode mudar por uma chamada independente após o deploy, por exemplo, pelo recálculo por evento que resolve a competência da data do fato. Os contadores `recusas_oraculo` e `falhas_tecnicas_oraculo` saem no resumo da lavra. Seus valores e a prova pós-apply seguem [não verificado]. `app/ponto/management/commands/recalcular_fechamento.py:77-81` `app/ponto/management/commands/recalcular_fechamento.py:110-133` `app/ponto/services/fechamento.py:962-973` `app/ponto/services/fechamento.py:1235-1239` `app/ponto/services/fechamento.py:1320-1351` `app/ponto/services/fechamento.py:1836-1853`

### 4. Hash do FechamentoMensal de 06, 07, 08 e 09

A função da casa é **`ponto.portas.regen_exportada.hash_do_gravado(colaborador, mes, ano)`**. Ela consulta o mensal gravado e retorna 12 caracteres do MD5. O conjunto exclui chaves e carimbos como `previsto_em`, `derivado_em` e `atualizado_em`. Para um digest por empresa e competência, use SHA-256 do JSON ordenado de `carimbo_gravado.foto(eid, mes, 2026)` e publique também a contagem de linhas. Repita nos quatro meses, no mesmo banco antes e depois do apply, incluindo todas as empresas para a prova histórica. `app/ponto/portas/regen_exportada.py:35-38` `app/ponto/portas/regen_exportada.py:56-74` `app/ponto/portas/regen_exportada.py:121-122` `app/ponto/portas/regen_exportada.py:194-195` `app/ponto/management/commands/carimbo_gravado.py:29-56`

```python
from hashlib import sha256
import json

from colaboradores.models import Empresa
from ponto.management.commands.carimbo_gravado import foto

for mes in (6, 7, 8, 9):
    for eid in Empresa.objects.order_by("pk").values_list("pk", flat=True):
        mensal = foto(eid, mes, 2026)
        dados = json.dumps(mensal, sort_keys=True, separators=(",", ":")).encode()
        print(f"{mes:02d}/2026 emp{eid} linhas={len(mensal)} sha256={sha256(dados).hexdigest()}")
```

`ExportacaoDominio.hash_sha256` é o campo do **TXT**. Os hashes do mensal antes e depois do apply seguem [não verificado]. `app/folha/models.py:75-84` `app/ponto/portas/regen_exportada.py:62-74`

## 28/11 01:43 · sessao a0c0bb04d7f1 · RETRATO-PDF-CORTA-POR-QUE · censo · origem=Code

# Dossiê antes do construtor · RETRATO-PDF-CORTA-POR-QUE, item 2

O vínculo **dia → motivo** já existe na fonte. Ele se perde em `fora_da_folha`, que entrega à lavra os motivos agregados e as datas planas. O aval fornecido autoriza preservar esse vínculo em chave opcional, usar um mapa de rótulos junto ao produtor e mostrar no **Por quê** apenas as datas da janela. `app/folha/export.py:1268` · `app/folha/fora_da_folha.py:214` · `app/folha/fora_da_folha.py:234` · `app/folha/fora_da_folha.py:246` · `aval.txt:2`

Usei o aval anexado. A entrada indicada em `PROMPTS.md` está [nao verificado] na cópia, cujo arquivo termina na linha 3194. O SHA Git da cópia, as linhas de produção e a execução da suíte estão [nao verificado]. `app/docs/PROMPTS.md:3194`

## 1. Lavrador e contrato da fonte

- **Escritor único em produção.** `lavrar_retrato_folha --apply` chama `montar_linhas` e persiste `linhas` e `lavrado_em` por `empresa`, `mes` e `ano` com `update_or_create`. O cron usa o mesmo comando. O selo AST cobra um único importador de produção de `montar_linhas`. A linha nasce em `montar_linhas`; `o_que_fazer` é leitor. `app/folha/management/commands/lavrar_retrato_folha.py:65` · `app/folha/management/commands/lavrar_retrato_folha.py:72` · `app/folha/management/commands/lavrar_retrato_folha.py:77` · `app/config/crons.py:1659` · `app/folha/tests/test_retrato_folha_um_derivador.py:68` · `app/folha/retrato_folha.py:1237` · `app/folha/retrato_folha.py:1079`
- **`tipo`.** A lavra grava um valor por colaborador. Para `furo_espelho`, `_tipo_cru` consulta `TIPO_POR_CAUSA` com a `causa` já gravada em `FechamentoMensal.causa_espelho`. Para os demais bloqueios, usa `TIPO_POR_MOTIVO`. Há `sem_causa` para bloqueio sem tipo conhecido e `''` para quem entra ou sai da conta. `app/folha/retrato_folha.py:379` · `app/folha/retrato_folha.py:1167` · `app/folha/retrato_folha.py:1250`
- **`datas`.** Para `furo_espelho`, `fora_da_folha` já tem `_dias` vindos de `com_dias=True` e projeta `[str(d['data']) for d in _dias]`. Quando `_c.get('dias')` é `None`, a lavra tenta `dias_ferias_com_batida` do item do export. Grava uma lista plana `AAAA-MM-DD` ou `None` quando falta forma de dia. Retratos anteriores à E2 podem vir sem `datas`. O `informativo` de rescisão tem chave própria. `app/folha/fora_da_folha.py:115` · `app/folha/fora_da_folha.py:246` · `app/folha/export.py:885` · `app/folha/retrato_folha.py:1227` · `app/folha/retrato_folha.py:1243` · `app/folha/retrato_folha.py:1255` · `app/folha/models.py:334`
- **Retorno exato de `motivos_retencao_celula(com_dias=True)`.** Tupla `(motivos, dias)`. `motivos` é `list[str]`, sem duplicatas e ordenada lexicalmente. `dias` é `list[dict]`, em ordem cronológica, com um registro por **dia de retenção do colaborador**. As chaves são `data: datetime.date`, `motivo: str`, `veredito: str | None` e `celula_id: int | None`. `data` é o dia civil da célula, sem hora, ainda em Python. Na falta da célula, os dois últimos campos são `None`; célula existente sem veredito conserva o ID. Isento, desligado, intervalo colapsado ou nenhum dia retido resultam em `([], [])`. O modo padrão devolve só a lista de motivos. `app/folha/export.py:1214` · `app/folha/export.py:1247` · `app/folha/export.py:1265` · `app/folha/export.py:1268` · `app/folha/export.py:1273` · `app/folha/export.py:1304` · `app/escala/models.py:1696` · `app/escala/models.py:1783` · `app/escala/apps.py:5`

## 2. Leitores do JSON e tolerância a chave nova

`RetratoFolha.linhas` é `JSONField` sem validador de esquema por linha na declaração. `linhas_do_retrato` devolve os dicionários pelo recorte e valida o argumento `filtro`. `recortar` valida grupos e valores do filtro. A chave extra atravessa esses leitores. `app/folha/models.py:350` · `app/folha/retrato_folha.py:98` · `app/folha/retrato_folha.py:647`

| Leitor | O que faz com a chave desconhecida |
|---|---|
| **Tela inteira, parcial, barra e contadores** | `_recorte_da_barra` abre as linhas e `_cadastro` copia o dicionário. A barra lê os eixos conhecidos; `_tabela` projeta campos explícitos e hoje passa `motivo` a `rotulo_do_motivo`. A chave extra passa pelo recorte e fica fora da saída. `app/folha/views_retrato.py:116` · `app/folha/views_retrato.py:194` · `app/folha/views_retrato.py:278` · `app/folha/views_retrato.py:324` · `app/folha/views_retrato.py:345` · `app/folha/retrato_folha.py:114` |
| **PDF do retrato e Raio-X** | Usam `_recorte_da_barra`. O PDF extrai as células explicitamente. O Raio-X agrupa por `tipo` e projeta `motivo` e `dias`. Ambos toleram a chave e hoje a ignoram. `app/folha/views_retrato.py:388` · `app/folha/views_retrato.py:414` · `app/folha/views_retrato.py:489` · `app/folha/views_retrato.py:500` |
| **Drawer do colaborador** | `linha_do_colab` devolve o dicionário inteiro. `o_que_fazer`, `portas_da_linha` e o template leem campos específicos. A chave extra chega à linha e hoje fica sem consumo. O drawer mostra o rótulo agregado, o tipo único e as datas planas em ISO. `app/colaboradores/views.py:994` · `app/folha/retrato_folha.py:950` · `app/folha/retrato_folha.py:1047` · `app/folha/retrato_folha.py:1112` · `app/templates/folha/_retrato_o_que_fazer.html:42` |
| **Comandos e frescor** | `vigiar_retido_calado` lê os bloqueados e campos específicos, com tratamento próprio para ausência de `dias_sem_leitura`. Tolera a chave extra. `lavrar_retrato_folha` conta a linha recém-montada em memória, inclusive no dry-run. O frescor consulta o carimbo `lavrado_em`. `app/folha/management/commands/vigiar_retido_calado.py:30` · `app/folha/vigia_retido_calado.py:77` · `app/folha/management/commands/lavrar_retrato_folha.py:65` · `app/folha/retrato_folha.py:1413` |
| **TXT** | A geração segue `classificar_export` → `montar_linhas_export` e valida o layout do TXT. A descarga do histórico devolve `ExportacaoDominio.conteudo`. A chave do JSON do retrato está fora das duas rotas. `app/folha/views.py:219` · `app/folha/views.py:275` · `app/folha/views.py:282` · `app/folha/export.py:719` · `app/folha/views.py:574` |
| **Testes** | O selo da lavra compara o **conjunto exato de chaves** de uma linha e acusa a adição nessa fixture. O teste de idempotência compara duas lavras completas. O teste de lookup do drawer exige que a linha volte inteira. Os testes de período e de literal de mapa cobram a saída exibida. Os demais consumidores acima leem campos selecionados. `app/folha/tests/test_retrato_folha_quadros.py:1425` · `app/folha/tests/test_retrato_folha_contador.py:270` · `app/folha/tests/test_retrato_folha_drawer.py:140` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/folha/tests/test_retrato_folha_periodo.py:371` |

## 3. Universo diário e molde AST

**Oito códigos possíveis na cópia.** `sem_celula` sai de dois caminhos do laço. Os outros sete são `espelho_furo`, `espelho_discordante`, `espelho_indefinida`, `espelho_cobranca_indevida`, `espelho_cobrado`, `espelho_nunca_bateu` e `espelho_fato_em_ausencia`, formados por `'espelho_' + c.veredito` para a tupla fechada `CelulaDia.VEREDITOS_ACUSAM`. Os protestos não cobráveis e a isenção de pausa podem suprimir o dia. `furo_espelho` é o motivo agregado do item do export. `ferias_com_batida` tem sua própria fonte de datas. `app/folha/export.py:1281` · `app/folha/export.py:1294` · `app/folha/export.py:1302` · `app/escala/models.py:1780` · `app/folha/export.py:977` · `app/folha/export.py:885`

`MotivoExaustivoTest` fica em `app/folha/tests/test_retrato_folha_quadros.py:314`. `_motivos_da_fonte` faz `ast.parse` de `export.py`, localiza `classificar_export` e colhe strings não vazias de atribuições a `['motivo']` e de pares literais em dicionários. O teste antívacuidade exige pelo menos seis códigos e dois exemplos concretos. O teste de cobertura subtrai os destinos declarados e exige conjunto vazio. Para o selo diário, o mesmo molde precisa colher as chamadas a `_retem` em `_dias_sem_decisao` e resolver a concatenação pelo AST de `VEREDITOS_ACUSAM`. Assim um emissor novo sem rótulo deixa o selo vermelho. Os textos dos oito rótulos estão [nao verificado]. `app/folha/tests/test_retrato_folha_quadros.py:255` · `app/folha/tests/test_retrato_folha_quadros.py:317` · `app/folha/tests/test_retrato_folha_quadros.py:323` · `app/folha/export.py:1268` · `app/folha/export.py:1302` · `app/escala/models.py:1780` · `aval.txt:2`

## 4. Janela do Q2

É o intervalo inclusivo `de_data` / `ate_data` do calendário do filtro universal. `cfiltro.ler(request.GET)` valida as datas ISO e troca pontas invertidas. Uma ponta só já recorta. `_recorte_da_barra` passa os limites a `no_periodo`, que usa as `datas` planas para ajustar `dias` e `saldo` em cópia e conserva a lista integral. A janela sozinha mantém as linhas no universo. A janela da **competência**, usada pela fonte na lavra, vem de `janela_fechamento` e do corte da empresa. `app/templates/core/_filtro_campos.html:43` · `app/core/filtro.py:68` · `app/core/filtro.py:98` · `app/folha/views_retrato.py:216` · `app/folha/retrato_folha.py:1334` · `app/folha/fora_da_folha.py:146` · `app/ponto/janelas.py:54`

Tela, PDF e Raio-X recebem esse recorte. Os links da lista e do Raio-X abrem o drawer com `mes` e `ano`. O drawer busca a linha integral e hoje mostra as datas ISO de toda a competência. `app/folha/views_retrato.py:230` · `app/folha/views_retrato.py:337` · `app/templates/folha/_retrato_lista.html:60` · `app/templates/folha/_retrato_raiox.html:68` · `app/colaboradores/views.py:994` · `app/templates/folha/_retrato_o_que_fazer.html:42`

## 5. Matriz de RED

| Caso | Asserção |
|---|---|
| **Linha nova** | Dar à fonte `03/11` e `05/11` com `espelho_furo` e `12/11` com `espelho_cobrado`. Conferir que a lavra preserva os três pares da chamada já feita e que o **Por quê** usa os rótulos do mapa do produtor, com cada data sob seu motivo. Cobrir também férias, cuja lista plana vem de outra fonte. `app/folha/fora_da_folha.py:115` · `app/folha/fora_da_folha.py:214` · `app/folha/retrato_folha.py:1231` · `aval.txt:2` |
| **Linha antiga sem a chave** | Abrir a mesma linha legada. Conferir o rótulo agregado na tela e no PDF, o tipo único e as datas ISO planas no drawer. Incluir legado anterior à E2, com `datas` ausente e Dias vazio sob janela. `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/folha/retrato_folha.py:1118` · `app/templates/folha/_retrato_o_que_fazer.html:42` · `app/folha/tests/test_retrato_folha_periodo.py:172` |
| **Janela que corta** | Com `05/11` a `12/11`, exigir `05/11` e `12/11` em **Por quê**, dois dias contados e a linha gravada intacta. Cobrir uma ponta só e janela que corta todos os dias. O selo atual de período compara todas as colunas textuais salvo Dias e precisa contemplar a mudança autorizada em **Por quê**. A saída textual quando sobram zero pares está [nao verificado]. `app/folha/retrato_folha.py:1334` · `app/folha/tests/test_retrato_folha_periodo.py:155` · `app/folha/tests/test_retrato_folha_periodo.py:371` · `aval.txt:2` |
| **Motivo sem rótulo** | Retirar uma entrada do mapa ou acrescentar um emissor no produtor. O selo AST deve falhar informando o código faltante. Uma varredura vazia também deve falhar. `app/folha/tests/test_retrato_folha_quadros.py:317` · `app/folha/tests/test_retrato_folha_quadros.py:323` · `aval.txt:2` |
| **Tela, PDF e drawer leem o mesmo** | Alimentar as três portas com a mesma linha persistida. Cobrar igualdade de rótulos e datas em tela e PDF sob a mesma query, e leitura dos mesmos pares no drawer. Cobrar zero chamadas ao retentor nesses leitores. A igualdade de apresentação e a janela no drawer dependem da pergunta 1. `app/folha/views_retrato.py:260` · `app/folha/views_retrato.py:388` · `app/colaboradores/views.py:1001` · `app/folha/tests/test_retrato_pdf.py:138` · `app/folha/tests/test_retrato_folha_drawer.py:213` |

## 6. Arquivos × obras em voo

| Área | Toque previsto e cruzamento |
|---|---|
| **Fonte e lavra** | `app/folha/export.py` para o mapa, `app/folha/fora_da_folha.py` para transportar os pares já disponíveis e `app/folha/retrato_folha.py` para persistir e ler a chave. Cruzamento informado com **CODIGO-DOMINIO-VALIDO** em `export.py` e `retrato_folha.py`, que hoje também tratam o bloqueio de cadastro e sua projeção. `app/folha/export.py:881` · `app/folha/export.py:1185` · `app/folha/fora_da_folha.py:234` · `app/folha/retrato_folha.py:379` · `app/folha/retrato_folha.py:1237` |
| **Tela, PDF e Raio-X** | `app/folha/views_retrato.py` contém as três projeções e é o cruzamento informado com **RETRATO c1**. Os outros sítios informados da c1 são `app/relatorios/utils_audit.py` e `app/config/settings/base.py`. A ligação atual com o PDF auditável passa por `gerar_pdf_auditavel` e `get_commit_git`. `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/folha/views_retrato.py:443` · `app/folha/views_retrato.py:503` · `app/relatorios/pdf_base.py:135` · `app/relatorios/utils_audit.py:7` · `app/config/settings/base.py:4` |
| **Drawer e cadastro** | Se o drawer herdar a janela, os pontos são os links da lista e do Raio-X, `app/colaboradores/views.py` e o parcial `_retrato_o_que_fazer.html`. `colaboradores/views.py` também contém a edição do código de folha. `folha/views.py` contém o fluxo do TXT. O diff efetivo das duas obras em voo e a sobreposição exata nos trechos de views e cadastro estão [nao verificado]. `app/templates/folha/_retrato_lista.html:60` · `app/templates/folha/_retrato_raiox.html:68` · `app/colaboradores/views.py:994` · `app/colaboradores/views.py:1356` · `app/templates/folha/_retrato_o_que_fazer.html:42` · `app/folha/views.py:275` |

Os pontos de teste a tocar são o contrato de chaves da lavra, o molde AST, a janela, a paridade do PDF e o contrato do drawer conforme o alcance aprovado. A regra herdada é a testemunha ler a mesma autoridade, com um escritor canônico por estado. `app/folha/tests/test_retrato_folha_quadros.py:314` · `app/folha/tests/test_retrato_folha_quadros.py:1425` · `app/folha/tests/test_retrato_folha_periodo.py:371` · `app/folha/tests/test_retrato_pdf.py:138` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/docs/LEIS.md:60` · `app/docs/LEIS.md:65`

## 7. Perguntas de lei

**0 para abrir a lavra e o recorte de tela/PDF.** Q1 e Q2 estão decididas no anexo. Restam dois detalhes de saída para fechar a matriz inteira. `aval.txt:2` · `CLAUDE.md:443`

1. **Drawer.** A paridade pedida inclui exibir os pares sob a mesma janela, inclusive ao abrir pelo Raio-X? Se inclui, o **Por quê** pode exibir rótulo com datas no próprio valor ou os pares devem aparecer em linhas de detalhe? O corte atual e o selo exigem valor literal de mapa no bloco. `app/docs/CORTES.md:405` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/templates/folha/_retrato_raiox.html:68`
2. **Janela sem pares.** Para uma linha nova cujos pares ficam todos fora da janela, o **Por quê** fica vazio ou conserva o rótulo agregado, sem datas? O código atual define o vazio da coluna Dias; a saída de **Por quê** nesse caso está [nao verificado]. `app/folha/retrato_folha.py:1343` · `app/folha/views_retrato.py:291` · `aval.txt:2`

## 28/11 01:39 · sessao fb68df3fdb61 · RETRATO-PDF-CORTA-POR-QUE · verificador · origem=Code

**Parecer de verificação — RETRATO-PDF-CORTA-POR-QUE. O item 5 volta.** No bind-mount documentado, o leitor novo procura em `app/logs/` no host e o deploy grava em `logs/` na raiz do repositório. O RED do item 5 substitui o caminho padrão e deixa essa ligação sem prova. No item 4, os contadores vêm da barra, mas a precedência das linhas é definida na view do PDF e o RED deixa a posição de `None` sem cobertura. `app/docs/INFRA.md:22–30`, `app/config/settings/base.py:4–5`, `bin/deploy.sh:432–436`, `app/folha/views_retrato.py:399–420`, `app/folha/tests/test_retrato_pdf_pacote.py:60–84,228–267,274–315`.

Conferi a cópia e o diff anexo em leitura. A identidade Git da cópia, a execução do RED em `31ef2f199`, o GREEN informado de 358 testes e a configuração efetiva dos containers estão [nao verificado].

## 1. Quem escreve e quem lê

`JUIZES.md` é a tabela de consulta. O registro vivo mantém `classificar_export` como autoridade do fechamento e `tarefa_do_dia` como autoridade da porta diária. A permissão da tela é `tem_acao`, chamada tanto pela tela quanto pelo PDF. O PDF lê o retrato lavrado e delega a classificação visual a `led_da_linha`. Não encontrei uma segunda implementação desses juízes no diff. `app/docs/JUIZES.md:10–15,51,81–90,119–122`, `app/core/juizes.py:311,326–340`, `app/folha/views_retrato.py:257,377,391–405`, `app/folha/retrato_folha.py:516–532`.

| Campo ou valor | Quem produz ou grava | Quem lê |
|---|---|---|
| `RetratoFolha.linhas`, `colab`, `status`, `motivo`, `dias`, `datas`, `tipo`, `saldo`, `dono` | `classificar_export` entrega ID, estado e motivo. `fora_da_folha` entrega dias, datas e grupo de fio para o furo. `montar_linhas` compõe o JSON e `lavrar_retrato_folha --apply` grava por empresa, mês e ano. `app/folha/export.py:846–858,875–893,968–990`, `app/folha/fora_da_folha.py:146–165,210–253`, `app/folha/retrato_folha.py:1222–1255`, `app/folha/management/commands/lavrar_retrato_folha.py:65–79` | `linhas_do_retrato` abre o JSON. `_recorte_da_barra` monta a lista que tela, PDF e Raio-X leem. O drawer do colaborador e o vigia também passam pelo leitor do retrato. `app/folha/retrato_folha.py:98–111,930–953`, `app/folha/views_retrato.py:194–232,260–268,391–397,507–524`, `app/colaboradores/views.py:998–1004`, `app/folha/vigia_retido_calado.py:76–79` |
| `dono` gravado, dono de UI, cor e rótulo | `montar_linhas` grava o **fio** em `dono`. `fio_da_linha` o lê, com fallback para retrato legado. `dono_da_linha` projeta o balde da UI, incluindo `chamado → Admin` e `ninguem → Hoje` quando a primeira data é hoje ou posterior. `led_da_linha` acrescenta Entram e Rescisão como estados de linhas sem dono. `app/folha/retrato_folha.py:191–201,436–493,516–532,1246–1248` | Filtro, barra e drawer leem o dono de UI. Tela e PDF leem cor e rótulo via `desenho_do_led`. O PDF também lê a chave do LED para ordenar. O texto de `motivo` passa por `rotulo_do_motivo`, que lê `MOTIVO_RH`, também usado pela prévia e pelo PDF de retidos. `app/folha/retrato_folha.py:559–566,587–597,908–915,1112–1126,76–86`, `app/folha/views_retrato.py:286–292,399–405,435–446`, `app/folha/views.py:15–24,373,415,884–887` |
| Empresa, competência, filtros, `vis`, `universo`, `hoje` | `_recorte_da_barra` lê GET e preferência, usa `janela_atual` para o default, lê um instante e monta o recorte com `core.filtro`, `no_periodo`, `ler_grupos` e `recortar`. Quando vem `empresa=`, a própria porta chama `PreferenciaUI.set_pref`, inclusive no acesso pelo PDF. `app/folha/views_retrato.py:149–184,216–247`, `app/colaboradores/models.py:204–216` | Tela, PDF e Raio-X leem o recorte. `qs_do_recorte` produz o endereço do botão e a querystring guardada na auditoria. `core.filtro.chips` produz os rótulos e textos dos filtros ativos usados pela tela e pelo PDF. `app/folha/views_retrato.py:260–269,295–300,337,391–417,461–467,507–509`, `app/folha/retrato_folha.py:775–782`, `app/core/filtro.py:232–275` |
| `titulo`, `itens`, `ativo`, `rotulo`, `numero` da barra | `quadros_barra_retrato` monta o quadro. `_pilula` produz `numero` em **pessoas**. Para a pilula acesa, conta o recorte atual. Para a apagada, conta o recorte que sua ativação abriria. `app/folha/retrato_folha.py:785–813,838–845,879–915` | A barra e o PDF leem o mesmo `numero`. O PDF usa `ativo` para descrever o recorte e copia os pares rótulo/número de “Depende de”. O `n/total` do cabeçalho é outra medida, `len(vis)/len(linhas)`, tal como no filtro da tela. `app/templates/core/_barra_gestao.html:65–67`, `app/folha/views_retrato.py:315,324–325,411–430` |
| Razão social, nome, posto e descrição da escala | As portas de cadastro gravam empresa e posto, colaborador, vínculo de escala e `TipoEscala.descricao`. `_cadastro` projeta nome, posto e tipo de escala em memória, escolhendo o vínculo ativo mais recente por `data_inicio` e usando o posto do colaborador como fallback. `app/colaboradores/services/estrutura.py:13–22,43–45,67–79`, `app/colaboradores/views.py:1331–1333,1362–1365`, `app/colaboradores/services/colaborador.py:152–160,204–206`, `app/colaboradores/services/vinculo.py:274–292`, `app/escala/services/cadastro_tipo.py:184–188,307,382–401,431–446`, `app/folha/views_retrato.py:99–123` | O PDF usa a razão social no título e lê nome, posto e descrição nas células. A tela lê a mesma projeção cadastral. O item 3 passa a descrição por `escape` e `Paragraph` na coluna de 38 mm. Seu RED exige `Paragraph` e texto longo integral. A quebra visual na página e o comportamento com `<`, `>` e `&` estão [nao verificado]. `app/folha/views_retrato.py:395,424–425,435–450`, `app/templates/folha/_retrato_lista.html:60–66`, `app/folha/tests/test_retrato_pdf_pacote.py:47–48,210–222` |
| PDF, metadados e registro auditável | `retrato_pdf` fornece título, usuário, período, `tipo='outro'`, recorte e totais. `gerar_pdf_auditavel` escreve `RegistroPDFAuditavel`, incluindo `filtros_json`, `total_registros`, `commit_git` e o SHA-256 do PDF canônico. A view escreve `Content-Disposition` na resposta. `app/folha/views_retrato.py:461–475`, `app/relatorios/pdf_base.py:133–178` | O rodapé lê o commit e o SHA-256. A verificação pública busca pelo SHA-256 do PDF e exibe o commit armazenado, os filtros e o total. `app/relatorios/pdf_base.py:72–87`, `app/chamados/views.py:1202–1222`, `app/templates/chamados/verificar_pdf.html:37–50` |

## 2. Item 4 — `dias=None`, ordem e RED

- **O leitor que produz `None` é `no_periodo`.** Com pelo menos um limite, ele copia a linha e põe `dias=None` se `datas` estiver ausente, nula, vazia ou sem interseção com a janela inclusiva. Isso vale para qualquer motivo. Sem janela, conserva o valor lavrado. A cópia preserva `datas`, então uma linha pode continuar no dono Hoje com a coluna de dias vazia. `app/folha/retrato_folha.py:486–493,1334–1347`.
- **A lavra também produz `None`.** Entram, Rescisão, `sem_codigo_dominio`, `cadastro_zero` e `nao_homologado` chegam sem lista de dias. Férias com batida entrega datas. Para `furo_espelho`, `fora_da_folha` pode entregar o número `0` se a lista estiver vazia, e a lavra preserva esse zero. Tela e PDF exibem `None` em branco e `0` como número. Quantas linhas caem em cada caso em produção [nao verificado]. `app/folha/export.py:846–858,875–893,968–990`, `app/folha/fora_da_folha.py:146–165,234–246`, `app/folha/retrato_folha.py:1227–1243`, `app/templates/folha/_retrato_lista.html:66`, `app/folha/views_retrato.py:443`.
- **O RED de ordem deixa `None` sem cobertura.** A bancada tem `None` só em Entram e Rescisão, sem números nesses mesmos estados. Rodei em memória a chave extraída do código com a bancada, retirando apenas `dias is None`. A ordem esperada continuou igual. No controle sintético de mesmo dono, com ID 1 em `None` e ID 2 em `0`, a chave atual ordenou `[2, 1]` e a mutação ordenou `[1, 2]`. Os selos anteriores cobrem a produção de `None` na janela e na lavra. A decisão de posição no papel fica [nao verificado]. `app/folha/views_retrato.py:399–406`, `app/folha/tests/test_retrato_pdf_pacote.py:52–99,228–243`, `app/folha/tests/test_retrato_folha_periodo.py:126–153`, `app/folha/tests/test_retrato_folha_quadros.py:1409–1418`.
- **A ordem da tela ficou intacta.** `_recorte_da_barra` continua a fazer `ordenar(recortar(...))`, por balde, faixa crescente de saldo e ID. A view projeta `_vis` na mesma sequência e o template percorre `tabela` na página cheia e no parcial. Só o PDF cria outra lista com `sorted`. A guarda do pacote exige as mesmas pessoas e uma ordem diferente da do papel. O teste anterior de `ordenar` fixa a sequência esperada. `app/folha/views_retrato.py:225–232,260–268,278–293,318–346,391–406,435`, `app/folha/retrato_folha.py:754–772`, `app/templates/folha/_retrato_lista.html:35`, `app/templates/folha/retrato_folha.html:53`, `app/templates/folha/_retrato_parcial.html:7`, `app/folha/tests/test_retrato_pdf_pacote.py:245–249`, `app/folha/tests/test_retrato_folha_quadros.py:566–591`.
- **Fonte única.** O PDF lê o recorte existente e os números publicados pelo quadro. O RED compara esses números em dois recortes. O selo AST guarda a porta do universo, e a origem dos contadores é demonstrada pelo código. Para a ordem das linhas, a view lê `vis`, `dias`, `colab` e a chave do LED, mas escreve localmente a precedência `Sem ação → Sem leitura → Admin → Colab → Hoje → Entram → Rescisão`. O quadro publica os donos em outra sequência. Assim, a ordem reaproveita o julgamento de dono e tem regra de apresentação própria. Se o checklist exige que **a precedência** também venha publicada por `quadros_barra_retrato` ou `_recorte_da_barra`, essa exigência fica aberta. `app/folha/views_retrato.py:399–420`, `app/folha/retrato_folha.py:191–201,590–591,908–915`, `app/folha/tests/test_retrato_pdf_pacote.py:251–267`, `app/folha/tests/test_retrato_pdf.py:301–350`.

## 3. Item 5 — carimbo, alcance e efeitos

- **Caminho em produção.** A configuração SaaS herda `BASE_DIR / 'logs' / 'deploy.stamp'`, que no container vira `/app/logs/deploy.stamp`. O bind documentado o resolve para `/home/ronald/saas-hasner/app/logs/deploy.stamp`. `bin/deploy.sh` grava `/home/ronald/saas-hasner/logs/deploy.stamp`, via arquivo temporário e `mv`. Esse bind não expõe o arquivo escrito. Compose efetivo, montagem adicional, cópia manual e ambiente `COMMIT_GIT` do worker [nao verificado]. `app/config/settings/base.py:4–5`, `app/config/settings/saas.py:10,118`, `app/docs/INFRA.md:22–30`, `bin/deploy.sh:432–436`, `.gitignore:40–41`.
- **Comparação com o ramo antigo.** No cenário dado, sem `COMMIT_GIT`, sem `/app/.git` e sem arquivo no novo caminho, ambos retornam `''`. O rodapé segue `n/d` e o registro segue vazio. Um `COMMIT_GIT` não vazio continua prioritário, com `strip()` e limite de 40 caracteres. Onde `/app/.git` existia e funcionava, perde-se o fallback antigo. Se o carimbo ficar acessível, falha de leitura ou UTF-8 e `QUANDO` não numérico podem agora interromper a geração. O parser devolve `{}` quando falta `QUANDO`. Falhas reais em produção [nao verificado]. `obra.diff` anexo, hunk de `app/relatorios/utils_audit.py:1`, `app/relatorios/utils_audit.py:11–20`, `app/core/esteira_vigia.py:1032–1042`, `app/relatorios/pdf_base.py:72–77,135,164`.
- **Quem escreve e lê `COMMIT`.** O deploy escreve epoch em `QUANDO`, `git rev-parse HEAD` inteiro em `COMMIT` e o assunto em `TITULO`. O PDF reaproveita o parser do vigia e usa só `COMMIT`, limitado a 40 caracteres e sem validação de formato. O registro conserva esse valor e o rodapé exibe oito caracteres. O vigia lê no host e usa o SHA inteiro em `git revert`. A guarda de auth e o verificador de imports também leem o SHA do host, e o handoff o abrevia para exibição. Quem injeta `COMMIT_GIT` em produção [nao verificado]. `bin/deploy.sh:432–436`, `app/core/esteira_vigia.py:64,1032–1042,1202,1297–1309,1355`, `app/relatorios/utils_audit.py:13–20`, `app/relatorios/models.py:56`, `app/relatorios/pdf_base.py:72,164`, `bin/janela_auth.sh:39–45`, `bin/import_tardio_contra_o_ar.py:45–55`, `bin/handoff_sessao.sh:274–277`.
- **Dois chamadores diretos.** `gerar_pdf_auditavel` usa o mesmo commit nas duas passagens e no registro. Alcança retrato, espelho mensal, histórico de chamados, gestão de férias e gestão de HE. Se o commit impresso mudar, muda também a entrada do SHA-256 canônico. O outro chamador é o espelho informacional, usado no avulso por datas e no lote. Ele usa o commit só no rodapé. Uma falha comum aos dois caminhos pode fazer o lote pular a pessoa. `app/relatorios/pdf_base.py:133–178`, `app/folha/views_retrato.py:461–467`, `app/relatorios/pdf_espelho.py:1066–1074,1106–1116`, `app/relatorios/pdf_chamados.py:158–176`, `app/relatorios/services.py:235–255,338–347`, `app/relatorios/views.py:1135–1164`, `app/ponto/views.py:886–894`.
- **RED e ordem de publicação.** O RED prova leitor, persistência e passagem do SHA aos dois objetos de rodapé, com arquivo temporário e `override_settings`. O controle prova precedência da variável. O caminho padrão e os caracteres efetivamente impressos ficam fora das asserções. O deploy recarrega e prova as rotas **antes** de trocar o carimbo. Mesmo se o arquivo ficar visível, essa ordem permite código novo com carimbo anterior ou ausente. No fluxo `--reload-copia`, o reload ocorre antes do commit da cópia, e o carimbo lê o `HEAD` naquele momento. Ocorrência no ar [nao verificado]. `app/folha/tests/test_retrato_pdf_pacote.py:274–322`, `bin/deploy.sh:357–373,416–436`, `bin/molde_fatia/cadeia.sh:35–48,62–69`.

## 4. Import, relógio e travas

O import novo é `utils_audit → core.esteira_vigia`. O topo do vigia só importa biblioteca padrão, registra um logger, inicializa constantes e compila sete regex. A cadeia estática não volta a `relatorios`. O corpo do módulo não lê os arquivos operacionais nem resolve settings, e o CLI está protegido por `__main__`. Na chamada sem `COMMIT_GIT`, o helper resolve o setting, consulta o caminho e lê o arquivo se presente. Num processo local novo, `-X importtime` mediu cerca de **26 ms cumulativos** para importar o vigia e suas dependências, sem carregar Django. Custo incremental no worker e memória [nao verificado]. `app/relatorios/utils_audit.py:2–20`, `app/core/esteira_vigia.py:27–36,38–109,351–357,605,1202,1297,1355,1653–1654`.

O `hoje` novo na ordenação vem do instante já capturado na borda de `_recorte_da_barra`. Esse mesmo dia chega à barra e ao LED. O timestamp da auditoria segue uma leitura própria, explicitamente fora do contrato do recorte. Os trechos novos não adquirem trava. O pipeline mantém bloqueios fornecidos pelo chamador antes da geração, com a gravação entre as duas passagens. O retrato não fornece callbacks de bloqueio. O selo de relógio verifica o congelamento em testes, e a bancada está sob `freeze_time`. `app/folha/views_retrato.py:146–147,177–184,244–247,399–446,461–467`, `app/folha/retrato_folha.py:452–465`, `app/relatorios/utils_audit.py:13–20`, `app/relatorios/pdf_base.py:127–178`, `app/core/tests/test_selo_teste_sem_relogio.py:67–84`, `app/folha/tests/test_retrato_pdf_pacote.py:121–123`.

## 5. Item 1 — alcance do `attachment`

**Nenhum outro consumidor do partial encontrado aponta para essa rota.** `retrato_pdf` escreve o novo header apenas na própria resposta. A barra já fornece `pdf_href`, que o partial renderiza como âncora comum, sem `target`. Os outros consumidores continuam usando seus próprios destinos e headers, como Gestão HE (`inline`) e Furos (`attachment`). O RED exige `attachment` com o nome canônico e verifica a âncora na página cheia e no parcial OOB. O download efetivo no navegador [nao verificado]. `app/folha/urls.py:25`, `app/folha/views_retrato.py:337,468–475`, `app/templates/folha/_retrato_barra.html:50`, `app/templates/core/_btn_pdf.html:34–46`, `app/ponto/views.py:899`, `app/relatorios/views.py:1632`, `app/folha/tests/test_retrato_pdf_pacote.py:187–204`.

## 28/11 01:38 · sessao 39f9bf608e62 · CODIGO-DOMINIO-VALIDO · construtor · origem=Code

A cura está no diff da cópia. A revisão estática encontrou **13 arquivos de produção alterados e 0 `except` adicionados**. Os dois módulos do contrato declaram **22 casos, dos quais 15 têm prefixo `test_RED_`**. Resultado da suíte e GREEN [nao verificado]. [REDs da Folha](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:90) [REDs do Cadastro](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/tests/test_codigo_dominio_numerico.py:76) [nao verificado]

### Diff por item

- **Juiz e TXT.** Um predicado compartilhado exige dígitos ASCII após `strip()`, sem impor comprimento. `classificar_export` preserva o valor no item e atribui o motivo literal `codigo_dominio_invalido` entre código ausente e férias com batida. O montador já seleciona os itens `entra`. [colaboradores/services/cadastro.py:53](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/services/cadastro.py:53) [folha/export.py:850](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:850) [folha/export.py:882](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:882) [folha/export.py:723](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:723)
- **Cadastro.** Admissão e edição reexibem o formulário com a mensagem «Código Domínio deve conter apenas números.» antes dos respectivos pontos de gravação. O campo vazio segue aceito na admissão e preserva o anterior na edição. A validação fica nas duas portas de tela. [colaboradores/views.py:1135](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1135) [colaboradores/views.py:1239](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1239) [colaboradores/views.py:1296](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1296) [colaboradores/views.py:1365](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1365) [core/respostas.py:74](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/core/respostas.py:74)
- **Retrato.** `MOTIVO_RH` dá o rótulo, `TIPO_POR_MOTIVO` dá `cadastro` e o fio existente dá `admin`. Um único leitor de `linha['codigo']` monta `codigo Dominio = <valor>` para lista, PDF, drawer e Raio-X. No drawer, `valor` continua sendo o rótulo literal do mapa. [folha/views.py:18](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/views.py:18) [folha/retrato_folha.py:223](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:223) [folha/retrato_folha.py:382](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:382) [folha/retrato_folha.py:89](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:89) [templates/folha/_retrato_lista.html:65](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_lista.html:65) [folha/views_retrato.py:423](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/views_retrato.py:423) [templates/folha/_retrato_o_que_fazer.html:37](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_o_que_fazer.html:37) [templates/folha/_retrato_raiox.html:73](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_raiox.html:73)
- **Listas fechadas.** `_MOTIVO_NA_TELA` ganhou pílula própria, `_PORTAS_CADASTRO` ganhou a natureza correspondente e Quem Cobrar lê o rótulo de `MOTIVO_RH`. `MotivoExaustivo` já colhe a atribuição literal por AST e verifica seu destino no mapa. O Raio-X do export já conta a chave em **Outras pendências**. [ponto/services/fechamento_lista.py:28](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/ponto/services/fechamento_lista.py:28) [templates/ponto/_fech_linha.html:19](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/ponto/_fech_linha.html:19) [relatorios/views_pend.py:50](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views_pend.py:50) [relatorios/views.py:1729](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views.py:1729) [folha/tests/test_retrato_folha_quadros.py:325](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_retrato_folha_quadros.py:325) [folha/views.py:625](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/views.py:625)

### REDs e mutantes para o Opus

| Caso RED | Mutante a derrubar |
|---|---|
| No lote misto, `VIGIA` e `１２３` ficam fora, `00123` gera `0000000123` e o layout retorna `[]`. Em colisão com férias com batida, vence o código inválido. Vazio e brancos mantêm `sem_codigo_dominio`. | Usar só `isdigit()`, remover a retenção, mover a checagem para depois de férias ou eliminar também o controle numérico. [TXT](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:90) [Precedência](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:152) |
| Conferir rótulo, tipo, pílula, natureza e presença literal na fonte. Em Quem Cobrar, conferir o rótulo na linha retida. | Apagar uma entrada de mapa, apontar a pílula para espelho ou trocar a atribuição literal do motivo por uma referência indireta. [Mapas e AST](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:163) [Quem Cobrar](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views.py:1729) |
| Lavrar `VIGIA` com tipo `cadastro` e dono `admin`, corrigir o cadastro para `00123` e conferir o valor histórico na lista, PDF e drawer. | Buscar o código atual, omitir o complemento em um leitor ou interpolá-lo em `valor` no drawer. [Retrato](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:241) |
| Enviar `VIGIA` nas duas portas e exigir formulário HTTP 200 e mensagem visível. Na admissão, conferir as contagens de usuários e colaboradores. Na edição, conferir código e telefone anteriores. | Validar depois da gravação, redirecionar a recusa ou levar a regra ao modelo e impedir a criação do legado usado na fixture. [Admissão](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/tests/test_codigo_dominio_numerico.py:85) [Edição](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/tests/test_codigo_dominio_numerico.py:118) [Fixture legada](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:64) |

### Comando do GREEN

```bash
bash bin/suite.sh --dir /home/ronald/wt-codex-codigo-dominio-valido-013223 --only "folha.tests.test_codigo_dominio_invalido colaboradores.tests.test_codigo_dominio_numerico folha.tests.test_export ponto relatorios core.tests.test_selo_teste_sem_relogio"
```

## 28/11 01:37 · sessao 8ac04350367a · - · - · origem=Ronald (tmux)

**Item 4. O RED deixa a posição de `dias=None` em aberto.** O teste de ordem exige a sequência por dono, dias decrescentes e desempate por ID. Na bancada, `None` só aparece em Entram e Rescisão, sem dias numéricos nesses estados. Retirar a chave `dias is None` preservaria a ordem esperada da bancada. Para distinguir a chave de `-(dias or 0)` em um teste, o mesmo estado precisa de `0` e `None` com o ID menor no `None`. O zero é possível em `furo_espelho` cujo relatório devolve `len([])` e chega à lavra como zero. `app/folha/tests/test_retrato_pdf_pacote.py:52–98`, `:228–243`, `app/folha/views_retrato.py:399–406`, `app/folha/fora_da_folha.py:210–246`, `app/folha/retrato_folha.py:1227–1236`.

**Origens de `None`.** Na lavra, `montar_linhas` recebe a contagem de `furo_espelho` de `fora_da_folha` e usa a lista de férias quando essa contagem vem ausente. Sem as duas fontes, grava `None`. Isso abrange Entram, Rescisão, `sem_codigo_dominio`, `cadastro_zero` e `nao_homologado`. O leitor `no_periodo`, com pelo menos um limite temporal, cria uma cópia com `dias=None` quando `datas` está ausente/nula, vazia ou fora da janela. Isso se aplica a qualquer motivo. Sem janela, mantém os valores da lavra. Há testes para esses vazios, inclusive retrato legado, e para a faixa Cadastro permanecer. `app/folha/export.py:846–858`, `:875–893`, `:968–989`, `app/folha/fora_da_folha.py:146–165`, `:234–246`, `app/folha/retrato_folha.py:1222–1243`, `:1334–1347`, `app/folha/tests/test_retrato_folha_periodo.py:126–153`, `:172–183`, `app/folha/tests/test_retrato_folha_quadros.py:1394–1417`.

**Ordem da tela.** `_recorte_da_barra` segue produzindo `ordenar(recortar(...))`, com ordem por balde, faixa de saldo e ID. A tela projeta `_vis` na mesma ordem e o template percorre `tabela`. O PDF cria outra lista com `sorted(_vis, ...)`. O teste da tela no pacote confere mesmas pessoas e ordem diferente do PDF. A ordem exata da função e seu comportamento com o recorte de datas estão em testes anteriores. `app/folha/views_retrato.py:225–232`, `:260–268`, `:278–293`, `:318–346`, `:391–406`, `:435`, `app/folha/retrato_folha.py:754–772`, `app/templates/folha/_retrato_lista.html:35`, `app/folha/tests/test_retrato_pdf_pacote.py:245–249`, `app/folha/tests/test_retrato_folha_quadros.py:566–590`, `app/folha/tests/test_retrato_folha_periodo.py:354–368`.

**Juízes, campos e contadores.** O gravado `dono` é o fio escrito por `montar_linhas` e lido por `fio_da_linha`. O dono de UI é produzido por `dono_da_linha` e usado pela barra e pelo LED, com dois estados adicionais do LED para Entram e Rescisão. `hoje` vem da borda e é passado a todos os leitores. `_pilula` publica `numero` como quantidade de pessoas do recorte atual se acesa ou do recorte após ativação se apagada. Tela e PDF leem o número de `quadros_barra_retrato`. A ordem do PDF usa a chave e as linhas de suas fontes, com uma precedência local de apresentação distinta da sequência publicada no quadro. O RED de contadores compara resultados em dois recortes. O selo AST anterior guarda a fonte do universo, enquanto a dependência de `quadros_barra_retrato` é comprovada pelo código. `app/folha/management/commands/lavrar_retrato_folha.py:65–79`, `app/folha/retrato_folha.py:191–201`, `:436–493`, `:516–532`, `:588–597`, `:816–842`, `:904–915`, `:1246–1248`, `app/folha/views_retrato.py:177–182`, `:246–247`, `:324–325`, `:399–420`, `app/templates/core/_barra_gestao.html:67`, `app/folha/tests/test_retrato_pdf_pacote.py:251–267`, `app/folha/tests/test_retrato_pdf.py:301–350`.

**Item 3.** O RED verifica `Paragraph` e preservação do texto, com um exemplo longo sem metacaracteres. A troca para `Paragraph(escape(descricao))` acontece no leitor PDF. A geometria da quebra nas páginas e um exemplo com `<`, `>` ou `&` não recebem asserção direta. `app/folha/tests/test_retrato_pdf_pacote.py:46–48`, `:164–173`, `:210–222`, `app/folha/views_retrato.py:424`, `:441`, `:449–450`. Execução da suíte e distribuição em produção [nao verificado].

## 28/11 01:37 · sessao ca3696a25ee3 · - · - · origem=Ronald (tmux)

- **Import.** `utils_audit` importa o vigia no nível do módulo. O vigia importa apenas biblioteca padrão na carga, inicializa logger, constantes e sete regexes. A leitura do carimbo e o acesso ao valor em `settings` acontecem na chamada de `get_commit_git()` sem `COMMIT_GIT`. A rotina CLI fica sob `__main__`. A cadeia estática desse import não tem volta a `relatorios`. Tempo e memória adicionais [nao verificado]. `app/relatorios/utils_audit.py:2-20`, `app/core/esteira_vigia.py:27-36,38-81,83,90,101,104-109,351-357,605,1202,1297,1355,1653-1654`.
- **Attachment.** Só a resposta de `folha:retrato_pdf` recebe o novo `Content-Disposition`. A barra passa o URL dessa rota como `pdf_href`, que já era âncora direta sem `target`. Os modos submit e `hxAbrir` continuam definidos pelo mesmo partial e nenhum outro consumidor encontrado usa essa rota. Há exemplos independentes de inline em Cadastro × Realidade, HE e follow-up, e de attachment em Furos, Inteligência e histórico de exportações. `app/folha/urls.py:25`, `app/folha/views_retrato.py:337,461-475`, `app/templates/folha/_retrato_barra.html:50`, `app/templates/core/_btn_pdf.html:34-46`, `app/escala/views.py:528`, `app/ponto/views.py:899`, `app/relatorios/views.py:1110,1632`, `app/inteligencia/views.py:643`, `app/folha/views.py:861`.
- **Mordida.** O RED do item 1 exige attachment com nome canônico. A verificação da âncora é guarda de markup da página cheia e do parcial OOB. Download efetivo no navegador [nao verificado]. O RED de Escala exige `Paragraph` e texto integral com um caso longo. Quebra visual e caracteres especiais [nao verificado]. `app/folha/tests/test_retrato_pdf_pacote.py:47-49,187-204,210-222`.
- **Relógio e ordem do deploy.** O instante do recorte vem da borda, vira `hoje` e alimenta as novas chamadas. O timestamp da auditoria tem relógio próprio preexistente e exclusão explícita do contrato do recorte. Os trechos novos não adquirem locks. O reload e as sondas de rota vêm antes do `mv` do carimbo no host. Se esse carimbo passar a ser visível no container, há uma janela possível de código novo com stamp velho ou ausente. `app/folha/views_retrato.py:146-147,177-184,244-247,399-446`, `app/relatorios/pdf_base.py:129-135`, `app/relatorios/utils_audit.py:13-20`, `bin/deploy.sh:357-373,416-419,432-436`.

## 28/11 01:36 · sessao 5e85d18b9370 · - · - · origem=Ronald (tmux)

**Item 5 deve voltar.** A montagem documentada leva `/home/ronald/saas-hasner/app` a `/app`. O novo default aponta para `/app/logs/deploy.stamp`, equivalente a `app/logs/deploy.stamp` no host. `bin/deploy.sh` grava `logs/deploy.stamp` na raiz do repositório, por arquivo temporário e `mv`. Assim, o bind-mount documentado não expõe o carimbo ao PDF. O Compose efetivo, montagens extras e `COMMIT_GIT` do processo ficam [nao verificado]. `app/docs/INFRA.md:22–30`, `app/ponto/management/commands/apurar_furo_da_exportada.py:25–28`, `app/config/settings/base.py:4–5`, `app/config/settings/saas.py:10`, `bin/deploy.sh:432–436`, `.gitignore:40–41`.

- **Efeito na produção descrita.** Com `COMMIT_GIT`, a precedência e o limite de 40 caracteres seguem iguais. Sem a variável, sem `/app/.git` e sem carimbo no caminho novo, os dois ramos devolvem `''`. O rodapé fica `n/d` e o registro guarda vazio. Ambientes com checkout em `/app/.git` perdem aquele fallback. Se o carimbo passar a ser acessível, `read_text` sem tratamento pode propagar falha de leitura ou UTF‑8. `ler_deploy_stamp` devolve `{}` sem `QUANDO` e pode levantar `ValueError` com `QUANDO` não numérico. [nao verificado] o estado real desses arquivos. `obra.diff` anexo, `app/relatorios/utils_audit.py:11–20`, `app/core/esteira_vigia.py:1032–1042`, `app/relatorios/pdf_base.py:72–77,135,164`.
- **Consumidores.** `gerar_pdf_auditavel` lê uma vez e passa o valor às duas fases e a `RegistroPDFAuditavel.commit_git`. Isso alcança retrato, espelho mensal, histórico de chamados, gestão de férias e gestão de HE. O espelho informacional é o outro chamador direto e usa o valor só no rodapé. O lote usa os dois caminhos e pode pular o colaborador se ambos levantarem exceção. `app/relatorios/pdf_base.py:133–170`, `app/folha/views_retrato.py:461–467`, `app/relatorios/pdf_espelho.py:1041–1074,1106–1116`, `app/relatorios/pdf_chamados.py:158–176`, `app/relatorios/services.py:235–255,338–347`, `app/ponto/views.py:886–894`.
- **Semântica.** O shell escreve `git rev-parse HEAD` no `COMMIT`. O helper retorna até 40 caracteres sem validar formato. O rodapé mostra oito, a verificação pública mostra o valor persistido inteiro e a consulta usa o SHA‑256 do PDF canônico. O vigia passa o `COMMIT` inteiro ao `git revert` e usa oito só nas mensagens. `bin/deploy.sh:432–436`, `app/relatorios/utils_audit.py:13–20`, `app/relatorios/models.py:56`, `app/relatorios/pdf_base.py:72,147–164`, `app/chamados/views.py:1202–1222`, `app/templates/chamados/verificar_pdf.html:37,49`, `app/core/esteira_vigia.py:1297–1309`.
- **Alcance do RED.** Os testes criam carimbo temporário e substituem o setting. Checam o valor devolvido, o registro e o argumento recebido pelos dois objetos de rodapé. O controle cobre a precedência de `COMMIT_GIT`. O caminho padrão, a montagem e o texto efetivamente renderizado estão fora dessas asserções. `app/folha/tests/test_retrato_pdf_pacote.py:274–322`.
- **Import.** O topo de `core.esteira_vigia` carrega biblioteca padrão, cria logger, constantes e compila sete regex. As leituras ficam dentro de funções e o `main` é protegido por `__name__`. A inspeção estática não mostra ciclo nem leitura de disco ou settings pelo corpo do módulo. `utils_audit` resolve o setting na chamada. Tempo de import [nao verificado]. `app/core/esteira_vigia.py:27–36,64,83–109,351–357,605,1653–1654`, `app/relatorios/utils_audit.py:6–20`.

_8843 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
