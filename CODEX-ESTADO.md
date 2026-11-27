# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 11:15** · publicado em **27/11/2026 11:17** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 11:15 · sessao 50281f6e60fe · RC-SEM-PROXIMA · verificador · origem=Code

## REPROVADO

### 1. MÉDIA — R1 curado na origem para remoção ou troca após a visita, com nova regressão de leitura

`_obras_vivas` captura mtime com `fstat` e lê o texto pelo mesmo descritor. `sem_colher` extrai o `rc=` do texto já capturado. Nas sondas em memória, a c2 perdeu a saída após remoção e misturou `10:00` com `rc=7` após troca. A c3 preservou `10:00 rc=1` nos dois casos. `caso_leitura_removido` e `caso_leitura_trocado` exigem essa linha e rc `3`. Os mutantes `le-tarde-removido` e `le-tarde-trocado` reintroduzem a abertura tardia e caíram nas sondas. [bin/rc_sem_colher.py:90–100](/home/ronald/codex_ws/1e824c3f77f507fdc1a0ee18f7ac268e18d1b3d9/bin/rc_sem_colher.py:90), `bin/rc_sem_colher.py:103–116`, `bin/tests/test_rc_sem_colher.sh:152–156,179–183,212–218,349–350`.

A c3 abre **todo** `.rc` regular antes de filtrar idade e escolher o mais novo. Na sonda, um `.rc` vencido com `PermissionError` derrubou a chamada inteira, enquanto a c2 listou o órfão válido. O mesmo ocorreu com um `.rc` colhido e ilegível que a c2 listava em `SEM PROXIMA`, classe que dispensa seu conteúdo. O selo cobre remoção e troca **após** a leitura e erro de diretório. Esses dois casos de erro de arquivo ficam descobertos. `c2:bin/rc_sem_colher.py:77–91,97–106,124–138`, `bin/rc_sem_colher.py:84–100,137–152,176–187`, `bin/tests/test_rc_sem_colher.sh:179–183,212–218,257–260,297`.

### 2. MÉDIA — R2 curado na origem, com selo no juiz e lacuna no leitor

O juiz imprime uma linha final opcional a partir de `len(colher)`, `len(proxima)` e `min_idade`, usando as próprias listas que acabou de emitir. `caso_resumo` exige uma linha de órfão, três de parada, um único resumo final, `min_idade` correto e ausência de resumo na chamada sem a flag. `caso_resumo_vazio` exige os dois zeros com rc `0`. Os mutantes `resumo-trocado`, `resumo-soma`, `resumo-sem-min-idade` e `resumo-mudo-no-zero` caíram nas sondas em memória. `bin/rc_sem_colher.py:156–187`, `bin/tests/test_rc_sem_colher.sh:228–240,353–356`.

O `vercodex.diff` chama `--min-idade 900 --resumo` e incorpora ao cabeçalho a última linha `RESUMO ` tal como veio. O limite de 15 linhas e 120 caracteres vale para os detalhes. Confirmei com uma resposta simulada contendo uma linha de detalhe e resumo `7/21`. O leitor imprimiu `7/21`. Para rc `0` e `3`, imprime cabeçalho e detalhes. Para outro rc, imprime também a última linha do stderr, ou `(stderr vazio)`. Se faltar resumo, imprime `SEM RESUMO do juiz`, inclusive com rc `0` ou `3`. Esse bloco não propaga o rc do juiz. O selo do juiz continuaria verde se o vercodex perdesse as flags ou voltasse a contar linhas. `vercodex.py:165–176` no anexo `vercodex.diff`, `bin/tests/test_rc_sem_colher.sh:64,228–240,353–356`.

### 3. ALTA — R3 corrige a colisão original, mas ainda lista com 10 segundos reais em DST

A função gera um instante por dia civil, no dia do `.rc` e no seguinte, com `mktime(..., tm_isdst=-1)`. Aplica o piso inclusivo do início do minuto do `.rc` e o teto inclusivo de `agora`, e escolhe o maior instante restante. A listagem exige **mais de 900 s desde o fim** do minuto escolhido. `caso_dia_da_marca` passou de rc `3` na c2 para rc `0` vazio na c3. `caso_dia_da_marca_ontem` manteve rc `3` às `00:06:01`. Os mutantes `marca-mais-antiga` e `marca-sem-teto` caíram, respectivamente, nesses casos. `bin/rc_sem_colher.py:119–129,143–152`, `bin/tests/test_rc_sem_colher.sh:157–163,220–226,351–352`.

- **Dois dias civis com menos de 24 h reais.** Em `America/New_York`, `.rc` em `07/03/2026 23:50`, marca real em `09/03 00:10:10` e `agora` às `00:10:20`. O `.rc` tinha `84.020 s` e passou pelo corte. O juiz inferiu `08/03 00:10` e o `main` retornou rc `3` após **10 s reais**. Um `.rc` com mais de `86.400 s` reais fica fora já na passagem pela árvore. [bin/rc_sem_colher.py:77–95](/home/ronald/codex_ws/1e824c3f77f507fdc1a0ee18f7ac268e18d1b3d9/bin/rc_sem_colher.py:77), `bin/rc_sem_colher.py:119–129,146–152`.
- **Hora repetida e todas as marcas `None`.** Em processos novos com `America/New_York`, em `01/11/2026`, um `.rc` às `00:50` e uma marca às `01:10:10 -0500` deram listagem após **10 s**, porque a candidata foi `01:10 -0400`. Com `.rc` às `01:00 -0500` e a mesma marca, a primeira ocorrência caiu abaixo do piso e o dia seguinte passou do teto. A parada real de **20 min** sumiu das duas listas. É coerente deixar o prazo indeterminado quando faltam candidatas. Aqui faltou representar uma ocorrência válida, e `sem_colher` já excluiu a obra pela chave da marca. `bin/rc_sem_colher.py:106–115,119–129,141–151`.
- **Virada comum e mtime futuro.** `.rc` às `23:50` e marca às `00:10` do dia seguinte funcionam. Se a marca entra perto do fim da janela de 24 h, o `.rc` pode vencer antes de o prazo amadurecer. A colisão das `23:50` também pode ocultar uma marca realmente antiga quando a candidata de hoje acaba de entrar no teto. Já um `rc_m` futuro pode ganhar a seleção: com piso futuro recebe `None`, e no mesmo minuto ainda fica aquém do prazo. `bin/rc_sem_colher.py:77–95,108–115,119–129,146–152`, `bin/tests/test_rc_sem_colher.sh:97–104,157–163`.

### 4. BAIXA — O selo novo do resumo usa relógio de parede

O instrumento lê `time.time()` uma vez no `main` e passa o mesmo `agora` à varredura e às duas classificações. `_cenario` injeta instantes fixos. Já `caso_resumo` e `caso_resumo_vazio` usam `_arvore_proxima`, que lê `date +%s`, e chamam o juiz, que consulta o relógio novamente. O caso misto faz três chamadas em instantes distintos. `bin/rc_sem_colher.py:173–179`, `bin/tests/test_rc_sem_colher.sh:64–82,122–131,167–170,228–240`.

## Escritores, leitores e compatibilidade

| Valor | Caminho |
|---|---|
| Tupla `(ob, d, rc_m, rc_p, rc_txt, outro)` | `_obras_vivas` monta e emite. `main` materializa uma vez. `sem_colher` usa os seis campos. `sem_proxima` usa os outros cinco e descarta `_txt`. Esses são os consumidores encontrados na cópia. `bin/rc_sem_colher.py:74–115,132–142,176–179`. |
| `rc=` do `.rc` | `codex_pergunta.sh::_saida` grava o valor e publica por `mv`. `_obras_vivas` lê até 200 caracteres. `_rc_de` procura `rc=(\d+)`, depois o primeiro grupo de dígitos, ou devolve `?`. `sem_colher` entrega o valor a `main`, que imprime `rc=…`. `bin/codex_pergunta.sh:565–574`, `bin/rc_sem_colher.py:64–67,90–95,108–115,180–181`. |
| `RESUMO` e rc do processo | `main` escreve o resumo quando solicitado. `--min-idade` filtra só `SEM COLHER`. O vercodex proposto lê a linha pronta. `ESPERA_UNICA` chama sem flags, deixa passar stdout, entra no laço com rc `0`, pula com `3` e termina com sucesso para ambos. O `find` final também pode imprimir caminhos. `espera_nega` examina o comando Bash. `bin/rc_sem_colher.py:108–115,166–187`, `bin/disjuntor.py:1963–1969,2157–2174`, `vercodex.py:165–174` no anexo. |

**Saída sem `--resumo`, byte a byte:** a constante `ESPERA_UNICA` e os formatos das duas linhas ficaram iguais. A sonda mista comum deu stdout idêntico entre c2 e c3. A igualdade geral é falsa: no caso das `23:50`, a c2 emitiu `SEM PROXIMA` e pulou o laço, enquanto a c3 teve stdout vazio no juiz e entrou no laço. Confirmei esse desvio com o Bash da espera e dependências simuladas. `bin/disjuntor.py:1967–1969`, `c2:bin/rc_sem_colher.py:164–169`, `bin/rc_sem_colher.py:180–187`, `bin/tests/test_rc_sem_colher.sh:157–163,220–226`.

**Esmeril final.** Contagem de `^\+.*except`: **0** no `diff.patch` e **0** no `vercodex.diff`. O `except Exception` é contexto. A classificação e as contas ficam no juiz. Dentro dele, o parser puro roda duas vezes sobre o mesmo texto e o predicado de folga aparece nas duas listas. As sondas foram em memória e `bash -n` passou. RED/GREEN integrais, `test_disjuntor.sh`, Ruff, instalação do vercodex e ocorrência desses cenários em produção ficam [nao verificado]. `bin/rc_sem_colher.py:48–61,106–115,135–150,173–187`, `bin/tests/test_rc_sem_colher.sh:35–37,297–314`, `vercodex.py:167–176` no anexo.

## 27/11 11:11 · sessao c71da07a488e · E4C-F5-INSTANTE · verificador · origem=Code

## APROVADO — ver3 do E4C-F5-INSTANTE

A volta cura P3 na origem e fecha a lacuna de trava na auditoria pós-rollback. O teste multiempresa confirma a regra de estoque já presente na cura2. O item 6 continua fora desta volta. [materializacao.py:846](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:846) · [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [test_e4c_f5_c4_red.py:932](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:932) · [test_e4c_f5_c4_red.py:634](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:634)

### Procedentes do ver2

1. **MÉDIA · P3 curado na origem.** Os dois caminhos de veto passam `unica=True` pelo ponto comum `trilha_do_veto_k6`. O escritor procura uma linha com a mesma identidade e só insere se ela faltar. O RED original exige estado preservado e uma linha após duas validações. O RED fora de request repete o mesmo usuário e comando e exige uma linha, além de exigir uma linha distinta para outro comando. **M44** desliga a opção no chamador e **M46** a ignora no escritor. Ambos atingem a contagem do segundo RED. A cura evita novas duplicatas e reaproveita uma linha se já houver histórico igual. [materializacao.py:846](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:846) · [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138) · [test_e4c_f5_c4_red.py:1055](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1055) · [e4c_f5_c4red2_mutantes.py:757](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:757)
2. **MÉDIA · Pós-rollback curado na origem.** O novo `atomic` toma a mesma trava do dono pela porta existente e relê a pergunta antes de auditar. O RED força o veto tardio em duas tentativas, exige uma linha, confere a foto após o rollback e exige o dono registrado nas duas recusas. **M45** remove a aquisição. A mordida vem da asserção do sentinela, mesmo que a contagem ainda dê um em execução serial. O teste retira previamente os sentinelas da fixture e simula o momento da leitura do K6. Uma disputa real entre conexões nesse ramo fica **[nao verificado]**. [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [test_e4c_f5_c4_red.py:1087](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1087) · [test_f5_v3_guarda_sob_trava.py:28](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_f5_v3_guarda_sob_trava.py:28) · [e4c_f5_c4red2_mutantes.py:762](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:762)
3. **BAIXA · Multiempresa é controle.** A ordem do teste é Empresa A → dono A → Empresa B → dono B. A regra herdada já recusa B por vir depois do **primeiro** dono do lote. O teste exige a mensagem específica e as duas fotos intactas. **M40** continua ligado ao RED de uma Empresa e um dono. O controle multiempresa veio sem mutante próprio. [test_e4c_f5_c4_red.py:932](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:932) · [export.py:602](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/folha/export.py:602) · [e4c_f5_c4red2_mutantes.py:724](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:724)
4. **BAIXA · M16 conserva a mordida.** Cada passagem de materialização e passivo ganha seu savepoint revertido. As asserções ainda exigem uma trilha nova com fotos e motivo para cada metade. M16 apaga fotos e descrição e derruba a asserção de conteúdo nos quatro subcasos. O teste sem banco já substitui a função inteira de trilha e continua provando o encaminhamento de uma tentativa. [test_e4c_f5_c4_red.py:1017](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1017) · [test_e4c_f5_c4_red.py:1170](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1170) · [e4c_f5_c4red2_mutantes.py:254](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:254) · [e4c_f5_c4red2_mutantes.py:475](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:475) · [test_bug_par_sem_banco.py:153](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_bug_par_sem_banco.py:153)

### Campos de `evento(unica=True)`

`evento` escreve todos os campos pelo mesmo `create`. A consulta de duplicata lê os oito campos de uma cópia de `registro`. A tabela aponta quem fornece cada valor e onde a trilha é lida no recorte. [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138)

| Campo | Quem fornece | Leitor da trilha |
|---|---|---|
| `acao` | `trilha_do_veto_k6` fornece `veto_competencia_do_fato`. [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | RED seleciona a ação. [test_e4c_f5_c4_red.py:1065](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1065) |
| `modelo` | A trilha fornece `PerguntaDisputa`. [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | `_trilha` seleciona o modelo. [test_e4c_f5_c4_red.py:1014](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1014) |
| `objeto_id` | A trilha passa `p.pk`. [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | `_trilha` seleciona o PK. [test_e4c_f5_c4_red.py:1014](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1014) |
| `descricao` | A guarda produz o motivo e a trilha passa `texto[:500]`. [materializacao.py:826](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:826) · [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | `_ver_trilha` verifica o motivo e a tela o exibe. [test_e4c_f5_c4_red.py:1025](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1025) · [log_auditoria.html:73](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/templates/core/log_auditoria.html:73) |
| `valor_antes` | A trilha monta a foto do texto e dos três instantes. [materializacao.py:854](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:854) | RED pós-rollback compara a foto. [test_e4c_f5_c4_red.py:1119](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1119) |
| `valor_depois` | A trilha passa `dict(foto)`. [materializacao.py:859](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:859) | O mesmo RED exige igualdade com a foto anterior. [test_e4c_f5_c4_red.py:1119](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1119) |
| `usuario` | `evento` usa o autor explícito com PK ou, se o argumento `ator` for `None`, o usuário autenticado da request. Sem autor resolvido com PK, grava nulo. [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) | `_trilha` seleciona `self.admin` e a tela filtra pela FK. [test_e4c_f5_c4_red.py:1014](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1014) · [views.py:705](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/views.py:705) |
| `ator` | `evento` grava `sistema:<comando>` quando reconhece o processo fora de request, inclusive com usuário explícito. Em request ou sem comando reconhecido, o campo fica nulo. [observ.py:22](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:22) · [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) | O RED distingue os dois comandos pelo campo `ator`. [test_e4c_f5_c4_red.py:1081](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1081) |

**Mesma autoria no filtro e no `create`, sim.** `usuario` e `ator` são resolvidos uma vez. Deixar `ip` fora da identidade é coerente com o veto por pergunta, motivo, fotos e autoria. Uma repetição por outro IP reutiliza a linha encontrada e conserva o IP dela. `unica` é opt-in e a trilha K6 é o único chamador que o ativa na cópia. Os demais, inclusive `evento_kw` e `registrar_log`, seguem direto ao `create` com os valores resolvidos. [observ.py:94](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:94) · [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) · [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138) · [observ.py:157](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:157) · [auditoria.py:22](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/auditoria.py:22)

**JSONField.** No Django fixado pela cópia, `create` com `None` simples numa `JSONField(null=True)` grava **SQL NULL**. `campo__isnull=True` casa esse valor. Um `null` dentro do dicionário continua sendo parte do JSON comparado por igualdade. A trilha K6 sempre passa dois dicionários, então seus REDs deixam o ramo de `None` no campo inteiro sem exercício. SQL efetivamente emitido **[nao verificado]**. [requirements.txt:13](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/requirements.txt:13) · [models.py:300](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/models.py:300) · [observ.py:143](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:143) · [materializacao.py:854](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:854)

### Travas em todos os caminhos da trilha

- **`validacao.py:242`, antes do carimbo.** A recusa de `:797` roda dentro do `atomic` que já chamou `travar_o_ato_do_plantio`. Essa função toma o dono pela `trava_na_ordem`. [validacao.py:242](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:242) · [validacao.py:755](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:755) · [validacao.py:795](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:795) · [materializacao.py:2223](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:2223)
- **`validacao.py:242`, depois do rollback.** O rollback elimina a aquisição e o sentinela feitos naquele bloco. Uma trava já detida pelo `atomic` externo sobrevive, como pode ocorrer na validação em lote. O novo bloco usa a porta reentrante e relê a pergunta. Há precedente de `trava_na_ordem(colab_id)` só para proteger consulta de identidade e escrita em `lavrar`. [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [trava_do_colaborador.py:16](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/trava_do_colaborador.py:16) · [validacao.py:1172](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:1172) · [par_relampago_na_lista.py:116](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/ponto/services/par_relampago_na_lista.py:116)
- **`materializacao.py:2004`.** O laço da disputa toma a trava antes da unidade. O passivo `reparar` toma a trava antes de `_trabalhar_o_par` → `_plantar_o_dia` → `_materializar_uma`. A validação chega à mesma unidade pela tentativa do par ou pela metade avulsa sob a trava de `:758`. A chamada recursiva para a irmã ocorre dentro do ato da principal, sob o mesmo dono. O RED do passivo chama `_plantar_o_dia` diretamente e prova conteúdo da trilha, enquanto a aquisição da trava no caminho de produção se verifica em `reparar`. [materializacao.py:2004](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:2004) · [materializacao.py:2301](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:2301) · [passivo_validado.py:393](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/passivo_validado.py:393) · [passivo_validado.py:491](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/passivo_validado.py:491) · [passivo_validado.py:585](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/passivo_validado.py:585) · [validacao.py:687](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:687) · [validacao.py:826](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:826) · [materializacao.py:1841](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:1841) · [test_e4c_f5_c4_red.py:1182](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1182)

**Empresa antes do dono.** Nenhuma das duas entradas de `_recusa_da_entrega` exige a trava da Empresa. A primeira já tem o dono e chega com a decisão da guarda. A segunda toma só o dono e reutiliza o veto obtido antes do rollback. A exigência explícita de Empresa antes do dono pertence ao registro de **estoque**, que confere a ordem antes do K6. Assim, nos caminhos da recusa, a ausência da trava da Empresa é compatível com o contrato da trilha. A trava do dono serializa consulta e inserção da auditoria. O motivo do veto tardio continua sendo o da tentativa anterior ao rollback, enquanto a foto vem da releitura posterior. [validacao.py:237](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:237) · [validacao.py:795](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:795) · [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [models.py:4873](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4873) · [models.py:4892](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4892)

### Esmeril

5. **BAIXA · Semântica e autoria.** O parâmetro `ator` recebe o autor humano, enquanto a coluna `ator` identifica o processo. Essa diferença é herdada e o filtro usa as duas colunas já resolvidas. Também seguem herdados `data_evento` como dia do turno ou dia civil da conversa e `entrada_em` como instante de inserção do ato ou marco convencional do estoque. [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) · [models.py:3086](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3086) · [models.py:3097](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3097) · [models.py:4402](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4402)
6. **BAIXA · Alcance dos selos.** M44 e M46 provam que a dedupe é usada. O RED fora de request varia o comando e mantém sempre o mesmo usuário, portanto a retirada isolada de `usuario` do filtro escaparia desse teste. M45 prova a presença da trava pelo registro transacional. M20 continua apontando para o RED já vermelho do item 6, onde o `post_save` e `DecisaoPergunta.registrar` chegam ao mesmo emissor. [test_e4c_f5_c4_red.py:1068](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1068) · [test_e4c_f5_c4_red.py:1106](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1106) · [e4c_f5_c4red2_mutantes.py:489](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:489) · [signals.py:253](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/signals.py:253) · [models.py:4791](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4791)
7. **BAIXA · Relógio, derivação, escritor e `except`.** O delta usa os instantes da pergunta na foto e o `criado_em` já definido no modelo. Os testes novos usam o congelamento da classe. Segue herdado `carimbar` chamar `pergunta_viva(self)` sem passar o relógio que resolveu, com possibilidade de `localdate()` no degrau da janela. K6 segue no juiz registrado. A identidade da trilha deriva do mesmo `registro` que alimenta o único `create`. No diff fornecido, **`^\+.*except` = 0**. A captura existente continua em `engolir`. [materializacao.py:854](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:854) · [models.py:307](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/models.py:307) · [test_e4c_f5_c4_red.py:68](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:68) · [models.py:3460](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3460) · [models.py:3496](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3496) · [juizes.py:642](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/juizes.py:642) · [juizes.py:350](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/juizes.py:350) · [observ.py:85](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:85) · [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138)

O veredito é técnico, sobre o delta fornecido. A alteração em `core/observ.py` extrapola a lista dos briefs 3 e 3b anexados. O aval específico da 3c, a reprodução independente dos números da pista e a ocorrência em produção ficam **[nao verificado]**. [observ.py:94](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:94)

## 27/11 11:09 · sessao ccb1176605bd · ORFAS-VOLTOU · sonda · origem=Code

## B1 — Universo

A sonda terminou com `FIM 1895 1895`, sem erro de execução na saída fornecida. O `confere False` é a divergência com o aval. `sonda_leitura:3,137`

| Medida | Sonda | Aval | Diferença |
|---|---:|---:|---:|
| Perguntas | 1.895 | 1.890 | +5 |
| Colabs | 184 | 183 | +1 |
| Células | 658 | 657 | +1 |
| Células com chamado | 481 | 480 | +1 |
| Células sem chamado | 177 | 177 | 0 |
| `indefinida` | 347 | 347 | 0 |
| `furo` | 175 | 174 | +1 |
| `nunca_bateu` | 51 | 50 | +1 |
| Outros | 85 | 86 | −1 |

Os totais com e sem chamado coincidem pelos dois critérios da sonda, FK da célula e `dia_do_fato`. Há 12 perguntas sem FK de célula. Os 85 outros vereditos são `concorde` 61, nulo 3, `cobrado` 8, `cobranca_indevida` 4, `discordante` 8 e `fato_em_ausencia` 1. `sonda_leitura:3–6` `aval_v2.txt:1` `app/chamados/juizes.py:89–130`

O corte declarado é 21/10 nos dois anexos. A sonda usa `data_evento >= 2026-10-21`, destino, validação e materialização nulos e via vazia, sem exigir disputa aberta. A consulta original, o instante da leitura do aval e os PKs da diferença estão [nao verificado]. A saída permite medir a diferença, mas sua causa — mudança de estoque, diferença de filtro ou de data — está [nao verificado]. `aval_v2.txt:1` `sonda_leitura:3–6`

## B2 — Classes e portas

As **103 classes somam 1.895**. Cada ID da saída aparece uma vez abaixo. Uso **(a)** para histórico e, se vazio, c1/c2, e **(b)** para destino vivo, conforme a nomenclatura da sonda. `sonda_leitura:7–109,137` `app/chamados/reconciliador.py:1711–1779,1800–1826`

| Classes da sonda | Perguntas | Porta existente e condição de fecho |
|---|---:|---|
| 1–2 | 5 | Disputa já fechada, pergunta ainda viva. **Nenhuma porta automática para esse resíduo no estado medido.** O reate e o S128 exigem disputa aberta. A decisão explícita do admin sobre o fato do dia tem porta própria, cuja aplicação a essas linhas está [nao verificado]. `app/chamados/reconciliador.py:1654–1661` `app/chamados/services/disputa_emissao.py:2038–2042,2155–2170` `app/ponto/services/veredito_celula.py:178–202` |
| 26–27 | 2 | C6 dá a pergunta por morta e a célula está `concorde`. **Lavra da célula → `retratar_perguntas_absolvidas_por_celula`**, que carimba `celula_absolveu` se a tranca permitir. O **S128 sem credor** também pode alcançar as duas numa execução para o colab, se passar pelas guardas da disputa. `app/chamados/reconciliador.py:695–707,747–789,1513–1515` `app/chamados/services/disputa_emissao.py:2155–2193` |
| 13, 28 | 11 | Fora do reate, vivas e sem FK de célula. A leitura da lâmpada fica sem prova. O `sem_chamado` dessas linhas vem da falta da FK na sonda. **Nenhuma porta automática de fecho demonstrada**. O ato humano por colab e data existe para pergunta datada e fora de conversa, se houver decisão sobre o fato. `sonda_leitura:19,34` `app/chamados/reconciliador.py:1654–1661` `app/chamados/juizes.py:979–995` `app/ponto/services/veredito_celula.py:178–202` |
| 3–12, 14–25, 29–34 | 212 | Vivas e fora do reate. **Cartório → retratação pela célula** quando a lavra absolver e o C6 confirmar. **Lista do admin** quando o juiz apontar CADASTRO ou BATIDA. Com o C6 vivo, o S128 sem credor pula a pergunta. A porta que se aplica a cada caso deste grupo está [nao verificado]. `app/chamados/reconciliador.py:747–789,1513–1515,1654–1661` `app/chamados/services/disputa_emissao.py:2171–2181` `app/escala/services/cadastro_realidade.py:821–835` |
| 51, 67, 85, 103 | 8 | **Reate (b)**. A derivação encontrou destino vivo e `resolvedora_valida` aceitou. A porta é `reapontar_resolvedora`. A costura sob trava e o fecho posterior estão [nao verificado]. `app/chamados/reconciliador.py:1711–1714,1821–1826` `app/chamados/services/fio.py:1040–1073` |
| 35, 41–42, 52, 58–59, 69, 75–76, 87, 94–95 | 149 | **Reate (a)**, histórico vazio ou descartado pelo C6, com veto `NUNCA_BATEU` à reemissão. A lista do admin tem a entrada **C5** para `nunca_bateu`, sujeita à janela e à situação do colab. O C5 lê o dono do dia. Presença na lista e dono por linha estão [nao verificado]. `app/chamados/reconciliador.py:1748–1779,1008–1012` `app/ponto/marcos.py:119–163` `app/escala/services/cadastro_realidade.py:165–175,205–244,757–759` |
| 68, 86 | 2 | **Reate (a)** com histórico descartado e veto `FOLGA` à reemissão. A porta de fecho pelo cartório é o evento da lavra, se o marco da própria pergunta estiver comprovado, o dia encerrado e as demais guardas passarem. Esses insumos estão [nao verificado]. `app/chamados/reconciliador.py:1008–1012,1182–1228,1275–1291,1523–1525` `app/ponto/marcos.py:155–163` |
| 40, 48, 57, 64, 73–74, 82, 89, 92–93, 101 | 1.176 | **Reate (a)** com célula `indefinida`. A porta para o veredito é a **lavra do cartório**. Quando o problema é de dado, a rota é a **lista do admin**, com a ação do dono pela L-099. O C11 tem predicado próprio para folga indefinida. Fora das quatro células do BO, o protesto e o dono por linha estão [nao verificado]. `app/chamados/reconciliador.py:1748–1779` `app/ponto/services/cartorio.py:1905–1913` `app/escala/services/cadastro_realidade.py:535–559,579–599,793–798,821–835` |
| 36–39, 53–56, 70–72, 88, 90–91 | 140 | **Reate (a)**. O C6 esvazia o histórico. c1 recusa a viva e c2 pode costurar no chamado da sua chave se estiver vivo, ou reemitir se passar pelas guardas. Para `orfao_14h` e `saida_sem_entrada`, também existe o fecho pela lâmpada do cartório. A sonda deixou essas guardas [nao verificado]. `app/chamados/reconciliador.py:362–402,956–1032,1156–1173,1204–1216,1748–1779` |
| 43–47, 49–50, 60–63, 65–66, 77–78, 80–81, 83–84, 96–100, 102 | 189 | **Reate (a)** com histórico vazio. c1 recusa pelo motivo ou pelo C6. A porta é c2, sujeito à chave, aos protestos e aos demais vetos. O fecho pela lâmpada continua disponível para os dois motivos que o seletor aceita. Resultado dessas portas [nao verificado]. `app/chamados/reconciliador.py:333,362–402,956–1032,1156–1173,1204–1216,1766–1779` |
| 79 | 1 | **Reate (a)** com histórico vazio e sem FK de célula. c1 recusa a viva. O fecho pela lâmpada fica sem prova. c2 consulta a célula por colab e data, então a FK nula da pergunta, sozinha, deixa a elegibilidade ao c2 [nao verificado]. `sonda_leitura:85` `app/chamados/juizes.py:979–995` `app/chamados/reconciliador.py:400–402,956–992,1766–1779` |
| **Total** | **1.895** | **`FIM 1895 1895`**. `sonda_leitura:137` |

Na L-099, o juiz `dono_da_divergencia` decide. **CADASTRO** recebe a ação da assinatura, **BATIDA** recebe a regularização da batida do dia e, para **ESTRUTURA**, a lista devolve **nenhuma porta**. A sonda deixou o resultado desse juiz por linha [nao verificado]. A falta de fecho automático observada, por si só, deixa o dono [nao verificado]. `app/ponto/management/commands/e6_oraculo.py:53–70,98–110` `app/escala/services/cadastro_realidade.py:821–835`

## B3 — As quatro células do colab PK 502

Em cada linha, as quatro auditorias `pergunta_emitida` são **posteriores** ao `resolvido_em` do chamado correspondente. O horário exato do `INSERT` e sua ordem em relação à resolução estão [nao verificado] para cada pergunta, porque a auditoria é gravada depois do `INSERT`. `sonda_leitura:110–136` `app/chamados/models.py:254–261,3330–3347`

| Célula e data | Chamado e `resolvido_em` | `saida_sem_entrada` | `intervalo_saida` | `intervalo_volta` | `orfao_14h` |
|---|---|---|---|---|---|
| 123535 · 01/11 | 36329 · 10:30:07.660433 | 52488 · 10:30:08.348888 | 52574 · 14:30:11.426806 | 52600 · 15:30:12.604666 | 52684 · 18:50:06.563843 |
| 123541 · 07/11 | 37532 · 10:30:13.380408 | 54887 · 10:30:13.624269 | 54965 · 14:30:10.807721 | 54984 · 15:30:10.999976 | 55065 · 18:50:07.243566 |
| 123547 · 13/11 | 38788 · 10:30:11.853324 | 57477 · 10:30:12.101662 | 57580 · 14:30:27.963646 | 57599 · 15:30:19.322599 | 57681 · 18:50:09.607853 |
| 123553 · 19/11 | 39854 · 10:30:14.733435 | 59786 · 10:30:15.144317 | 59891 · 14:30:23.025397 | 59917 · 15:30:18.774954 | 59984 · 18:50:08.329437 |

Horários locais `−03:00`. As colunas de pergunta mostram PK e horário da auditoria. `sonda_leitura:110–133`

- **Reate.** Para as 16, a seleção medida dá **(a), histórico esvaziado pelo C6**. A derivação ficou sem destino vivo e o histórico de encerrados foi descartado porque a pergunta estava viva no alcance do O261(C). No c1, as oito de intervalo são recusadas pelo motivo. As oito de `saida_sem_entrada` e `orfao_14h` são recusadas enquanto o C6 as der por vivas. No c2, o único protesto impresso nas quatro células é `PREVISTO_COM_DIA_INDEFINIDO`, que fica fora de `PROTESTOS_QUE_COBRAM`. Sem um chamado vivo na chave `batida_ausente` do c2, a reemissão cai nessa guarda, se tiver passado pelas anteriores. Com c1 e c2 recusados, o reate devolve `False` na linha 1779. O resultado da consulta exata à chave do c2 e a primeira guarda que barrou cada execução em produção estão [nao verificado]. `sonda_leitura:46,63,79,98,110–133` `app/chamados/reconciliador.py:333,362–402,956–992,1711–1779` `app/ponto/services/furos_sem_cobranca.py:35`
- **S128.** Na fotografia da sonda, as 16 estão sem `chamado_destino`, então ficam fora do carimbo por destino. O fecho sem credor admite FK nula, mas pula pergunta viva pelo C6. A passagem por `_ancoradas()` poderia tentar reabrir o credor se o chamado fosse o pai da disputa e as demais guardas passassem. A sonda não mostrou essa âncora. O `post_save` do encerramento é síncrono e só lê as perguntas que já consegue consultar naquele ato. Quais linhas estavam visíveis naquele instante, se houve outra chamada do S128 e o resultado dessas chamadas estão [nao verificado]. `sonda_leitura:110–133` `app/chamados/signals.py:91–119` `app/chamados/models.py:349–366` `app/chamados/services/disputa_emissao.py:1924–1948,2038–2042,2071–2084,2155–2193`
- **Célula.** As quatro têm lavra do cartório com `PREVISTO_COM_DIA_INDEFINIDO`, que deriva `indefinida`. O suprajuiz pode emitir esse protesto por falta de vínculo que sustente o dia ou por falta de âncora, como folga sem cadastro. Encerrar o chamado não resolve essa premissa de **CADASTRO**. O S128, quando carimba com `lavrar_veredito=True`, grava o veredito da pergunta. O cartório grava o da célula, e `indefinida` ainda conta como veredito que acusa no C6. A causa cadastral exata e a presença no C11 estão [nao verificado]. `sonda_leitura:110,116,122,128` `app/ponto/supra_juiz.py:346–372,464–469` `app/escala/models.py:1622–1642,1780–1782` `app/chamados/models.py:3502–3514` `app/chamados/juizes.py:1551–1555` `app/ponto/services/cartorio.py:1905–1913`

## B4 — Vigia

**38 órfãs reatáveis para o colab PK 502**, pela função real `perguntas_orfas_reatavies(502).count()`. O total da frota e os totais I8/I8o estão [nao verificado]. `sonda_leitura:134–137` `app/chamados/reconciliador.py:1640–1661` `app/chamados/management/commands/reconciliar_chamados.py:57–58,97–98`

O cron publica essa contagem como medida em `logs/reconciliador.log` a cada 15 minutos. A soma que dispara o `ALARME` com RC 2 usa outros contadores. I8/I8o saem em `logs/invariantes.log` e em `/app/media/invariantes_chamados.html`. O silêncio dos logs desde 29/09 está [nao verificado]. `app/config/crons.py:395,542–543,1913–1917` `app/chamados/management/commands/reconciliar_chamados.py:115–124` `app/chamados/management/commands/auditar_invariantes_chamados.py:1866–1889`

## 27/11 11:04 · sessao 8f2663b90aab · ORFAS-VOLTOU · leitura · origem=Code

## A1. Ramo e recusas do reate

**Um chamado `resolvido` na célula fica fora da derivação de destino vivo.** Se `_derivar_chamado_destino` encontrar outro destino vivo elegível, a pergunta segue pelo **(b)**. Se devolver `None`, segue pelo **(a)**. A derivação procura chamados vivos do colaborador da disputa, exclui `disputa_supervisao`, exige que o chamado atenda o motivo, descarta chamado já ocupado por pergunta do mesmo motivo com `materializada_em IS NULL` e compara os dias quando ambos são conhecidos. Havendo vários candidatos, tenta desempatar pela entrada do turno aberto e pelo estágio do chamado. Com zero ou mais de um candidato ao final, devolve `None`. `app/chamados/services/disputa_emissao.py:97–163,191–245` `app/chamados/reconciliador.py:1711–1714,1821–1826`

| Motivo | Histórico no alcance O261(C), com C6 morta | Histórico fora do alcance O261(C) | Histórico vazio |
|---|---|---|---|
| `orfao_14h` | Aceita categoria ou subcategoria reconhecida por `motivos_atendidos`. | O mapa legado precisa devolver `orfao_14h`. | Tenta c1 e, se recusar, c2. |
| `saida_sem_entrada` | Aceita categoria ou subcategoria reconhecida por `motivos_atendidos`. | A categoria precisa mapear para `saida_sem_entrada`, sem uma subcategoria mapeada que prevaleça. | Tenta c1 e, se recusar, c2. |
| `intervalo_saida` | Aceita categoria ou subcategoria reconhecida por `motivos_atendidos`. | Os mapas legados não têm esse valor. | c1 recusa pelo motivo. c2 exige alcance O261(C) e C6 viva. |
| `intervalo_volta` | Aceita categoria ou subcategoria reconhecida por `motivos_atendidos`. | Os mapas legados não têm esse valor. | c1 recusa pelo motivo. c2 exige alcance O261(C) e C6 viva. |

Fontes da tabela. `app/chamados/juizes.py:2331–2342` `app/chamados/models.py:4133–4170` `app/chamados/reconciliador.py:333,362–375,838–880,918–932,1028–1031,1748–1779`

- **(a), seleção do histórico.** Sem data na pergunta e sem data no contexto do motivo, sai `False` na linha 1725. O histórico exige o mesmo colaborador, status `resolvido` ou `fechado`, `contexto_json.data_turno` igual ao dia e categoria diferente de `disputa_supervisao`. Uma FK para a célula, sozinha, não satisfaz essa consulta. No alcance O261(C), C6 viva esvazia o histórico. Dia inválido ou ausente, competência entregue ou dia trancado levam à seleção legada. Com mais de um candidato, sai `False` na linha 1779. Com zero, sai pela mesma linha se c1 e c2 recusarem. `app/chamados/reconciliador.py:857–871,1711–1779`
- **(a), c1.** `lavrar_sem_turno_aberto` recusa motivo fora dos dois permitidos, resposta presente pelo juiz, materialização já preenchida, turno aberto no núcleo ou C6 viva. Também recusa se `carimbar` falhar pelo estado fora do fato, C6 viva após aplicar o carimbo em memória, par ato/coluna inválido `(False, True)` ou retenção ativa. O sucesso do c1 pode deixar a FK nula. `app/chamados/reconciliador.py:333,362–417,1774–1775` `app/chamados/models.py:3460–3519` `app/chamados/services/fio.py:1620–1632`
- **(a), c2.** `reemitir_chamado_do_dia` recusa motivo fora dos quatro, dia ausente ou inválido, colaborador fora de `ativo`, isenção aplicável, ausência que cobre, veto de `celula_veta_cobranca` ou veto de `EMITIR` pelo motor. Sem um `batida_ausente` vivo na chave do c2, também pode recusar por horizonte, ausência de protesto que cobra, `parcial_so_lista` ou veto da régua. Os dois intervalos ainda exigem `_viva_em_competencia_aberta`. Depois disso, sai `False` se `abrir` devolver `None`, se devolver chamado fora de `VIVOS` ou se a costura recusar. `app/chamados/reconciliador.py:874–880,930–1080`
- **(a), recusas dentro de `abrir`.** Um nascimento pode devolver `None` por absolvição pela célula ou veto da régua. Na reutilização de encerrado, `abrir` devolve o objeto mesmo se o renascimento recusar. Esse renascimento verifica situação ativa, via `cascade`, tranca e régua, depois passa por `reabrir`, que também verifica fechamento administrativo sem fato novo, premissa regularizada, agrupamento, irmão vivo e transição permitida. Se o objeto continuar encerrado, o c2 recusa na linha 1062. `app/chamados/models.py:972–998,1025–1037,1068–1090,1403–1426` `app/chamados/reconciliador.py:1055–1062`
- **Costura, nos dois ramos.** `reapontar_resolvedora` recusa destino de outro colaborador. `resolvedora_valida` pode recusar quando o juiz não reconhece o episódio como fato do turno, a ponte A2 alcança mais de um dia ou há divergência comprovada entre o dia do fato e o do chamado. Para dias adjacentes, aceita a travessia de meia-noite confirmada ou indeterminada. A guarda seguinte relê o destino e recusa se ele estiver terminal, a competência estiver comprovadamente aberta e a pergunta permanecer viva com o destino aplicado em memória. Se a costura também estiver datando uma pergunta, há recusas adicionais para conversa, dia do fato inválido e colisão que a porta não pode resolver. Esse último grupo fica fora do universo datado do aval. `app/chamados/services/fio.py:916–947,1017–1073,1098–1133,1911–1924,1946–2005` `app/chamados/juizes.py:2440–2498`
- **Saídas da costura.** Com histórico único, a recusa da costura sai `False` na linha 1804. Se a cerca vetar alguma escrita atingida pela costura ou pelo S128, desfaz o ato e sai `False` na linha 1815. Se a costura passar e o S128 retornar normalmente, o reate pode devolver `True` com zero perguntas carimbadas. No **(b)**, a recusa da mesma costura sai `False` na linha 1825. `app/chamados/reconciliador.py:1800–1826` `app/chamados/services/fio.py:1470–1480,1536–1561,1564–1592`

**Por que pode nascer solta.** A emissão pode gravar a pergunta sem destino quando a derivação devolve `None`. A tentativa imediata de reate exige pergunta recém-criada, FK ainda nula e motivo do grupo do dia. A fábrica deduplica pelo fato já representado, inclusive se a linha estiver solta. O estoque elegível volta ao reate pelo laço do colaborador, acionado por batida ou pela saída de `VIVOS` de um filho listado por pai vivo. Para as 16 perguntas do aval, a condição de nascimento, o ramo efetivamente executado e a recusa concreta estão [nao verificado]. `app/chamados/services/disputa_emissao.py:402–478` `app/chamados/services/fabrica_da_celula.py:138–157` `app/chamados/reconciliador.py:1674–1685` `app/ponto/signals.py:211–223` `app/chamados/signals.py:351–371`

## A2. O que o S128 alcança

**O S128 pode alcançar uma pergunta com FK nula pelo fecho sem credor.** O filtro por FK vale para o primeiro laço. Há três leituras relevantes em `resolver_perguntas_por_chamado_resolvido`. `app/chamados/services/disputa_emissao.py:2038–2042,2071–2084,2155–2193`

1. **Carimbo pelo destino.** O ORM exige exatamente `chamado_destino=chamado`, `materializada_em__isnull=True` e `disputa__fechada_em__isnull=True`. O laço pula pergunta em voo, viva pelo C6, de dia trancado ou respondida pelo juiz sem materialização de fato. As restantes só contam se `carimbar(...)` aceitar. Não há filtro inicial por data de criação nem por `validada_em`. Se o chamado tiver via `competencia_trancada`, o S128 retorna zero antes dos laços. `app/chamados/services/disputa_emissao.py:2030–2042,2087–2122`
2. **Âncora para reabertura.** `_ancoradas()` pode incluir FK nula se `disputa.chamado_id == chamado.pk` e a pergunta estiver sem validação pelo ato. Esse laço tenta devolver um credor à pergunta viva sem resposta. `app/chamados/models.py:349–366` `app/chamados/services/disputa_emissao.py:1924–1948,2071–2084`
3. **Fecho sem credor.** A varredura inclui todas as disputas abertas do colaborador do chamado. Pula a disputa se o pai estiver em `VIVOS` ou se qualquer pergunta tiver destino em `VIVOS`. Entre as restantes, examina perguntas com `materializada_em is None` e `DecisaoPergunta.composta(q).validada_em is None`, inclusive com FK nula. Pula as em voo e as vivas pelo C6, depois tenta `carimbar`. A disputa só fecha se não houver pergunta em voo e `pode_fechar()` aceitar. `app/chamados/services/disputa_emissao.py:2155–2193,2213–2226` `app/chamados/models.py:2047–2053`

**Gatilho principal.** `resolve_perguntas_ao_resolver` é `post_save` síncrono de `ChamadoColaborador`, com savepoint próprio. Exige status resultante em `ENCERRADOS` e save que escreveu status. Isso inclui criação, `update_fields` contendo `status_local` mesmo com valor igual, ou save integral em que o valor difere do `pre_save`. Um `QuerySet.update()` não emite esse sinal. Também há chamadas diretas na retratação e no reate histórico, no fecho de beco acionado após commit de `PerguntaDisputa`, e nos caminhos de aplicação manual de `fechar_becos_disputa` e `reconciliar_fantasmas`. Os agendamentos desses dois comandos usam `--alarme`. `app/chamados/signals.py:76–119,180–186,229–240` `app/chamados/reconciliador.py:602–616,1800–1807` `app/chamados/services/becos.py:38–61,80–94` `app/chamados/management/commands/fechar_becos_disputa.py:22–30,52–57` `app/ponto/management/commands/reconciliar_fantasmas.py:144–153,264–288` `app/config/crons.py:392,663`

Uma pergunta criada depois do save de encerramento estava ausente daquela execução. Uma pergunta com FK nula fica fora do laço por destino enquanto permanecer assim, mesmo se já existia no fechamento. O fecho sem credor ainda pode alcançá-la em outra chamada, sujeito aos predicados acima. Para as quatro células, a ordem dos eventos e o motivo específico de o S128 não ter carimbado as perguntas estão [nao verificado]. `app/chamados/signals.py:91–119` `app/chamados/services/disputa_emissao.py:2038–2042,2155–2193`

## A3. Por que a célula pode continuar `indefinida`

O escritor operacional do par é `ponto.portas.celula.lavrar_veredito`. `CelulaDia.lavrar_veredito` delega a essa porta. O cartório e a decisão do admin chegam por ela, com precedência para o admin. A mesma porta de célula também contém a operação que apaga a lavra do cartório ao regenerar uma célula futura. O `lavrar_veredito=True` passado por S128 a `PerguntaDisputa.carimbar` grava o veredito **da pergunta**. `app/escala/models.py:1832–1841` `app/ponto/portas/celula.py:1007–1024,1069–1142,1145–1150,1161–1204` `app/ponto/services/veredito_celula.py:357` `app/chamados/models.py:3502–3514`

A lavra da célula pode ocorrer com perguntas abertas ou sem lavra própria. O cartório classifica os insumos do dia, entre eles a quantidade de cobranças vivas com fio, e passa os códigos à lavra. `indefinida` vem de `PREVISTO_COM_DIA_INDEFINIDO`, `FASE_NAO_DECLARADA_COM_PREVISTO` ou `PARCIAL_SEM_PERIODO` quando nenhum código de maior precedência vence. Resolver o chamado pode mudar o insumo de cobrança e ainda deixar presente o motivo de cadastro ou de cobertura que produz `indefinida`. A própria `indefinida` integra `VEREDITOS_ACUSAM` e pode manter a pergunta viva quando o C6 chega à leitura da célula. `app/ponto/services/cartorio.py:1418–1423,1498–1516,1905–1913` `app/escala/models.py:1622–1643,1780–1782` `app/ponto/supra_juiz.py:346–372,392–402,450–469` `app/chamados/juizes.py:1499–1518,1551–1567`

**O gatilho de rejulgamento existe.** Na criação ou mudança de status, `_cartorio_post_save_chamado` tenta `julgar_celula` quando consegue o dia por `dia_do_fato(..., fortes=True)` e o cartório não está em julgamento. O lote `processar_cartorio` também chama o juiz para células passadas. Uma impressão igual pode pular a lavra. A impressão inclui PK e status dos chamados que o cartório lê, recortados pelos módulos de furo e pela chave de data do catálogo. Qual gatilho faltou nas quatro células, ou se houve rejulgamento que manteve o mesmo veredito, está [nao verificado]. `app/chamados/signals.py:180–218` `app/ponto/services/cartorio.py:131–157,490–503,1432–1448,2078–2115` `app/ponto/management/commands/processar_cartorio.py:45–65` `app/config/crons.py:616`

## A4. O que os vigias publicam

| Vigia | Universo e destino |
|---|---|
| `reconciliar_chamados --alarme` | A linha **`orfas reataveis`** soma `perguntas_orfas_reatavies(c).count()` pelos colaboradores do tenant. O seletor exige destino e materialização nulos, disputa aberta e um dos quatro motivos. Inclui colaboradores com evento pendente e dispensa C6, corte de data, via vazia e validação nula. Sai em stdout a cada 15 minutos em `logs/reconciliador.log`. O estoque fica fora da soma de alarme. Quando os outros contadores da soma disparam, o comando escreve em stderr e sai com RC 2. `app/chamados/reconciliador.py:1637–1661` `app/chamados/management/commands/reconciliar_chamados.py:57–62,89–124` `app/config/crons.py:395,1913–1917` |
| `auditar_invariantes_chamados --html` | **I8/I8o** partem de FK nula, colaborador ativo, motivo na união de `MOTIVOS_QUE_PRECISAM_CHAMADO_DESTINO_SET` com os quatro do dia, e C6 viva. A consulta base não exige disputa aberta, data mínima, via vazia, validação ou materialização nulas. `_piso_i8_sem_entrada_no_contexto` põe em I8o quem tem dois ou mais candidatos e nenhum com `entrada_timestamp`. Os candidatos casam por `contexto_json.data_turno` ou `celula.data`, mesmo colaborador e motivo atendido, com exclusão de `disputa_supervisao` e sem filtro de status. O restante é I8. O cron das 07:18 publica em stdout para `logs/invariantes.log` e escreve `/app/media/invariantes_chamados.html`. I8o tem o tamanho do piso na descrição e `0` na coluna de violações. A contraprova pode acrescentar texto à descrição. RC 2 depende de F1/F2, sem contribuição de I8 ou I8o. `app/chamados/models.py:4126,4169–4170` `app/chamados/management/commands/auditar_invariantes_chamados.py:51–110,818–831,860–872,1866–1889` `app/config/crons.py:542–543,1913–1917` |

`cron_run.sh` redireciona a execução pelo cron e grava duração e RC em `app/logs/placar.jsonl`. `pipeline_placar.medidos` lê esse RC e contabiliza o `2` como alarme de negócio. A pauta para TI do próprio runner trata RC diferente de `0` e `2`. Leitor automático das linhas de estoque ou de I8/I8o, leitor humano efetivo, silêncio dos logs desde 29/09 e números atuais dos vigias estão [nao verificado]. `app/config/crons.py:747,1515–1518,1913–1917` `bin/cron_run.sh:2–13,19–38` `app/core/management/commands/pipeline_placar.py:61–82,110–137`

## B. Sonda de leitura

O aval informa **1.890 perguntas, 183 colaboradores e 657 células**, com quatro perguntas soltas por célula nos quatro casos. As contagens por classe e os dados individuais ainda estão [nao verificado]. `aval_v2.txt:1`

A sonda cruza os dois seletores reais, o C6, o ramo que a seleção atual indica, o estado dos chamados do dia e o veredito da célula. Para mostrar chamados do dia, usa `dia_do_fato(..., fortes=True)`. Para diagnosticar o histórico do reate, usa a chave `contexto_json.data_turno` exigida por aquele ramo. `juiz_aceitou` mede apenas `resolvedora_valida`. A execução da costura, do c1, do c2 e da cerca fica [nao verificado]. `app/chamados/juizes.py:89–130,2414–2498` `app/chamados/reconciliador.py:1166–1176,1640–1661,1711–1826`

`PerguntaDisputa` não tem `criada_em`. A sonda lê apenas PK e instante da auditoria `pergunta_emitida`, gravada depois da inserção e em regime best-effort. `emissao_antes` e `emissao_depois` comparam esse registro com `resolvido_em`. Um registro posterior deixa o instante exato da inserção [nao verificado]. `ChamadoColaborador` usa `resolvido_em`, preservado na passagem de `resolvido` para `fechado`. Quando disponível, a sonda mostra também o carimbo da transição auditada para `fechado`. `app/chamados/migrations/0014_perguntadisputa.py:16–29` `app/chamados/models.py:62–80,254–261,304–308,3330–3348` `app/core/models.py:295–307` `app/core/observ.py:94–105`

Em B4, os totais de frota ficam dentro dos respectivos `handle`. A sonda imprime essa indisponibilidade e chama o seletor real para o número no recorte `--colab` das células-alvo. O Python usa transação `REPEATABLE READ, READ ONLY`. O envelope já seleciona `juliani` e tem escritas administrativas próprias, incluindo credencial, permissões quando aplicáveis, arquivo temporário e trilha. A sintaxe Python foi conferida em memória. A execução com o ORM da aplicação e em produção está [nao verificado]. `app/chamados/management/commands/reconciliar_chamados.py:57–58,97–98` `app/chamados/management/commands/auditar_invariantes_chamados.py:860–872` `app/chamados/tests/cef_c3_sonda.py:16–22` `bin/sonda_leitura.sh:115–143,153–181,216–240,245–251`

```python
from collections import Counter, defaultdict
from datetime import date
import logging

from django.db import connection, transaction
from django.db.models import BooleanField, Case, Exists, OuterRef, Q, Value, When
from django.utils import timezone

from chamados.catalogo.motor import ENCERRADOS, VIVOS
from chamados.juizes import dia_do_fato, motivos_atendidos, pergunta_viva, resolvedora_valida
from chamados.models import (
    MAPA_CATEGORIA_PARA_MOTIVO_DISPUTA,
    MAPA_SUBCATEGORIA_PARA_MOTIVO_DISPUTA,
    ChamadoColaborador,
    PerguntaDisputa,
)
from chamados.reconciliador import (
    _dia_fora_do_alcance_c,
    _orfas_abertas,
    perguntas_orfas_reatavies,
)
from chamados.services.disputa_emissao import _derivar_chamado_destino
from core.models import LogAuditoria
from core.observ import engolidos
from escala.models import CelulaDia
from ponto.marcos import celula_veta_cobranca

INICIO = date(2026, 10, 21)
CASOS = (123535, 123541, 123547, 123553)
AVAL = (1890, 183, 657)
NV = "[nao verificado]"


def local(instante):
    return timezone.localtime(instante).isoformat() if instante is not None else None


def cronologia(emissao, chamado):
    if chamado.resolvido_em is None:
        return "sem_resolvido_em"
    if emissao is None:
        return "sem_trilha_de_emissao"
    if emissao < chamado.resolvido_em:
        return "emissao_antes"
    if emissao > chamado.resolvido_em:
        return "emissao_depois"
    return "mesmo_instante"


def ramo_de_hoje(p, viva, historicos):
    # Seleção inline de reatar_pergunta_orfa para data_evento preenchida.
    # app/chamados/reconciliador.py:1711–1779,1821–1826
    if not p.no_reate:
        return "fora_do_laco", None
    colab = p.disputa.colaborador
    vivo, _ = _derivar_chamado_destino(p.disputa, p.motivo, data_pergunta=p.data_evento)
    if vivo is not None:
        ok, _ = resolvedora_valida(vivo, p)
        return "b_derivou_vivo", "juiz_aceitou" if ok else "juiz_recusou"

    encerrados = historicos.get((colab.pk, p.data_evento.isoformat()), ())
    if encerrados and not _dia_fora_do_alcance_c(p, colab, p.data_evento):
        if viva:
            veto = celula_veta_cobranca(colab.pk, p.data_evento)
            return "a_historico_esvaziado_pelo_C6", veto
        candidatos = [
            c for c in encerrados
            if p.motivo in motivos_atendidos(c.categoria, c.subcategoria)
        ]
    else:
        candidatos = []
        for c in encerrados:
            sub = (c.subcategoria or "").lower().strip()
            cat = (c.categoria or "").lower().strip()
            motivo = (
                MAPA_SUBCATEGORIA_PARA_MOTIVO_DISPUTA.get(sub)
                or MAPA_CATEGORIA_PARA_MOTIVO_DISPUTA.get(cat)
            )
            if motivo == p.motivo:
                candidatos.append(c)

    if len(candidatos) > 1:
        return "a_historico_multiplo", None
    if len(candidatos) == 1:
        ok, _ = resolvedora_valida(candidatos[0], p)
        return "a_historico_unico", "juiz_aceitou" if ok else "juiz_recusou"
    return "a_historico_vazio", celula_veta_cobranca(colab.pk, p.data_evento)


def estado_do_dia(chamados):
    estados = set()
    for chamado in chamados:
        if chamado.status_local in VIVOS:
            estados.add("vivo")
        elif chamado.status_local in ENCERRADOS:
            estados.add("resolvido_fechado")
        else:
            estados.add("outro_status")
    return "+".join(sorted(estados)) or "sem_chamado"


def sondar():
    filtro = Q(
        data_evento__gte=INICIO,
        chamado_destino__isnull=True,
        via_resolucao="",
        validada_em__isnull=True,
        materializada_em__isnull=True,
    )
    perguntas = list(
        PerguntaDisputa.objects.filter(filtro 

[truncado em 20000 de 28111 caracteres]

## 27/11 10:55 · sessao 77187f8251ee · RC-SEM-PROXIMA · verificador · origem=Code

## 1. P1–P4 na origem

| Achado | Cura e mordida |
|---|---|
| **P1 — parcial.** | `main` lê `agora` uma vez, materializa `vivas` uma vez e calcula as duas listas antes de imprimir. Na mesma passagem, a chave que exclui um `.rc` de `SEM COLHER` é a que permite sua entrada em `SEM PROXIMA`. Em memória, a c1 falhou nos três cenários e a c2 passou. `dois-walks` voltou a listar a mesma obra duas vezes, `dois-walks-ilegivel` voltou a dar rc `1`, e `dois-relogios` voltou a listar cedo. `caso_misto` confirma uma linha de cada classe para obras distintas. `bin/rc_sem_colher.py:94–106,121–138,157–169` · `bin/tests/test_rc_sem_colher.sh:165–180,287–289` |
| **P2 — corrigido na proposta, sem selo do leitor.** | O diff chama o juiz com `--min-idade 900`, conta cada prefixo separadamente e mostra diagnóstico para rc fora de `0`/`3`. Em memória, uma saída só de `SEM PROXIMA` passou de uma falsa contagem de órfãos para `0` órfãos e `1` parada. O teto global de 15 linhas pode ocultar todos os detalhes de `SEM PROXIMA` e de `OUTRAS LINHAS`, embora as contagens apareçam. `caso_min_idade` e `sem-min-idade` exercitam a CLI do juiz. Restaurar a contagem antiga ou remover o argumento **só do vercodex** deixa o selo intacto. `vercodex.py:165–176` (anexo `vercodex.diff`) · `bin/tests/test_rc_sem_colher.sh:58,202,243–248,271` |
| **P3 — corrigido para o minuto inferido.** | O juiz soma `60` s ao início do minuto e exige **mais de** `900` s desse limite. Em memória, `899/900/901` deram rc `0/0/3` na c2 e `3/3/3` na c1. `prazo-do-inicio-do-minuto` voltou a listar em `899` e `900`. A data inferida ainda pode estar errada. `bin/rc_sem_colher.py:37–38,110–118,135–138` · `bin/tests/test_rc_sem_colher.sh:182–186,286` |
| **P4 — corrigido no caso da próxima etapa.** | `caso_proxima_espera` exige rc `0` do Bash, linha `SEM PROXIMA` e ausência da testemunha de entrada no corpo do laço. Na sonda do Bash com árvore simulada, `proxima-desligada-espera` preservou a linha, mas entrou no laço. `proxima-vazia-espera` entrou no laço e perdeu a linha. A medição deixou de usar `date +%s`, embora a montagem da fixture ainda use horas relativas. O comportamento positivo já passa com o juiz da c1. A mordida específica vem dos mutantes. `bin/tests/test_rc_sem_colher.sh:60–76,107–113,221–222,290–291` · `c1:bin/tests/test_rc_sem_colher.sh:104–108` |

**Regressão de P1 reproduzida em memória.** Ao começar a varrer `OBRA-B`, removi o `r.rc` já visitado de `OBRA-A`. A c1 já tinha lido seu conteúdo e retornou rc `3`. A c2 guardou apenas caminho e mtime, abriu o caminho depois de varrer `OBRA-B` e terminou com `FileNotFoundError`, rc `1` e stdout vazio. Na variante com substituição do arquivo, a c2 imprimiu `10:00 rc=7`, juntando o mtime do arquivo antigo ao conteúdo do novo. A publicação por `mv` existe no produtor. Os dois casos de walk do selo disparam somente numa **segunda visita** à mesma obra, por isso deixam passar a troca durante a primeira passagem. `c1:bin/rc_sem_colher.py:89–102,153–163` · `bin/rc_sem_colher.py:57–61,77–91,99–106,160–169` · `bin/codex_pergunta.sh:572–574` · `bin/tests/test_rc_sem_colher.sh:151–171`

Nenhum dos seis mutantes novos passou nas sondas em memória. `caso_proxima_vazia` sozinho passa com a funcionalidade desligada, como controle negativo. `caso_misto` também passa na c1 com árvore estável. RED anterior à cura, GREEN integral, `test_disjuntor.sh` e instalação do `vercodex.diff` ficam [nao verificado]. `bin/tests/test_rc_sem_colher.sh:87–90,176–180,243–260,286–291`

## 2. Quem escreve e quem lê

| Valor | Escritor e leitores |
|---|---|
| **`agora`** | `main` captura `time.time()`. `_obras_vivas` usa o valor para o corte de 24 h, `sem_colher` para a idade mínima e `sem_proxima` para o prazo da marca. `bin/rc_sem_colher.py:68–85,94–100,121–136,160–163` |
| **`vivas`, mtime e conteúdo do `.rc`** | O wrapper publica o `.rc` com `rc=` e `fim=`. `_obras_vivas` lê o mtime, escolhe o caminho e registra o maior mtime de outro arquivo. `main` materializa as tuplas. Ambas as listas leem as tuplas e `sem_colher` abre o `.rc` para extrair `rc=`. `bin/codex_pergunta.sh:565–574` · `bin/rc_sem_colher.py:57–61,68–91,99–106,126–131,160–163` |
| **Marcas do RELATO** | A sessão colhe e registra `colhido` ou `superado` no formato documentado. O comando exato de escrita fica [nao verificado]. `main` lê o texto uma vez. `marcas_datadas` extrai os campos, `sem_colher` consulta a existência da chave e `sem_proxima` consulta tipo e hora da linha. `CLAUDE.md:332–337` · `app/docs/RELATO.md:13639–13646` · `bin/rc_sem_colher.py:34–54,97–105,124–136,157–159` |
| **rc `3` e as duas linhas** | `main` escreve os dois formatos em stdout e retorna `3` se houver alguma linha. `ESPERA_UNICA` deixa passar stdout, lê `$?`, pula o laço com `3` e fecha o Bash com `0` para rc `0` ou `3` do juiz. O `find` final ainda pode imprimir caminhos. O rc da linha `rc=<n>` vem do `.rc`. O vercodex captura stdout e código de saída. `bin/rc_sem_colher.py:32,57–61,164–173` · `bin/disjuntor.py:1963–1969` · `vercodex.py:165–176` (anexo) |

- **`disjuntor.py` e PreToolUse.** A espera chama a CLI, cujas opções continuam aceitas. `espera_nega` lê o texto do Bash e confere `find` e teto. Durante o laço, a espera observa ctime de `.rc`; uma marca atingir 15 minutos enquanto o laço roda não acorda a espera por si só. `bin/rc_sem_colher.py:142–156` · `bin/disjuntor.py:1967–1969,2157–2174,2217–2228`
- **`logs/espera_unica.sh`.** O RELATO cita o script e `logs/` é ignorado. Conteúdo, leitores e compatibilidade do script ficam [nao verificado]. `app/docs/RELATO.md:13598` · `.gitignore:53–54`
- **`vercodex` e `hook_stop`.** O vercodex antigo soma as duas classes sob `SEM COLHER`; o diff proposto corrige a contagem. O Stop lê o topo do RELATO para `PAREI`/`ESPERANDO`, o rc do checador de prova e as decisões B1/B3/B4 do disjuntor. Esses caminhos não consomem as listas nem o rc deste instrumento. Os chamadores Python encontrados das duas assinaturas alteradas estão no próprio `main`. `/home/ronald/vercodex.py:163–171` (anexo `ver1_r.md`) · `vercodex.py:165–176` (anexo `vercodex.diff`) · `bin/hook_stop_fila1.py:462–481,783–831,1305–1329,1434–1446,1489–1501` · `bin/rc_sem_colher.py:94,121,162–163`

## 3. Esmeril de semântica e classes

- **Relógio e unidades.** `agora` é um instante em segundos Unix lido na borda da chamada. A janela admite mtime exatamente no corte de 24 h. `--min-idade 900` só admite órfãos com idade **maior que** `900` s. A docstring sustenta o argumento; a frase “menos de 15 min” do RELATO contradiz o predicado. A espera usa ctime contra o arquivo de referência para detectar publicação por `mv`. `bin/rc_sem_colher.py:3–4,68–85,94–103,160–163` · `app/docs/RELATO.md:13640–13643` · `bin/disjuntor.py:1963–1969`
- **Dois `HH:MM`.** O de dentro dos parênteses identifica o minuto local do mtime do `.rc`. O prefixo informa o minuto local da colheita. A gramática distingue os campos, mas o prefixo não traz dia, offset ou segundos. `_instante_da_marca` infere o dia a partir do minuto do `.rc`, arredonda o piso sem perder o mesmo minuto por fração e soma `86400` para a virada. Meia-noite comum funciona. Em sondas com mudança de offset, a inferência deslocou a hora válida em uma hora. `bin/rc_sem_colher.py:34–48,99–104,110–118,130–136` · `bin/tests/test_rc_sem_colher.sh:91–105`
- **Juiz e derivação paralela.** O vercodex lê a classificação emitida e conta linhas. A regra de elegibilidade continua no instrumento. A dupla observação de relógio e árvore da c1 saiu. Ainda há duas chamadas ao mesmo parser puro sobre o mesmo texto e duas cópias do predicado de folga. Isso deixa duplicação interna, com o risco de divergência demonstrado acima concentrado na leitura tardia do conteúdo do `.rc`. `CLAUDE.md:17–22,35–36` · `app/core/juizes.py:3–14` · `bin/rc_sem_colher.py:2–4,41–54,97–106,124–136,157–163` · `vercodex.py:165–174` (anexo)
- **`except` adicionado, contando `^\+.*except`.** `diff.patch`: **0**. `vercodex.diff`: **0**. O `except Exception` do leitor é contexto do hunk. `bin/rc_sem_colher.py:94–169` · `bin/tests/test_rc_sem_colher.sh:107–186,243–296` · `vercodex.py:175–178` (anexo)

## 4. Descartados da c1

- **Mesmo caminho e minuto, chave sem dia, `touch` e marca sem prefixo** já existiam na c1. A chave pode reaproveitar a marca de outro `.rc`, um `touch` elegível mais de 5 s depois conta como escrita e uma marca sem prefixo fica fora da gramática. Eu manteria esses critérios fora do bloqueio desta volta, salvo a garantia de prazo abaixo. `c1:bin/rc_sem_colher.py:30–49,72–85,94–101,122–132` · `CLAUDE.md:334–337`
- **Hora anterior ao `.rc` e DST** já eram assim na c1. Eu bloquearia a garantia incondicional de 15 minutos reais. Em ambas as versões, um `.rc` de ontem às `23:50:59,9` e uma marca de hoje às `23:50:10` podem listar às `23:50:20`, com **10 s reais** de espera. O `.rc` ainda cabe na janela de 24 h e a data inferida para a marca é ontem. A c2 mudou a margem em segundos, preservando a ambiguidade de data. `c1:bin/rc_sem_colher.py:105–113,125–134` · `bin/rc_sem_colher.py:13–17,68–85,110–118,130–138`
- **`TMPDIR` e `caso_espera` com `date +%s`** também preexistem. `ESPERA_UNICA` chama `mktemp` sem fixar `/tmp`. Se a referência for parar dentro de uma obra elegível, pode entrar na conta de outro arquivo. A configuração efetiva fica [nao verificado]. A medição antiga permanece em `caso_espera` de órfão, fora do caso P4 corrigido. Eu não bloquearia a c2 por esses dois itens. `bin/disjuntor.py:1967–1969` · `bin/rc_sem_colher.py:77–91,99–101,126–128` · `c1:bin/tests/test_rc_sem_colher.sh:134–142` · `bin/tests/test_rc_sem_colher.sh:107–113,212–222`

## Veredito: REPROVADO

1. **P1 introduz uma regressão de leitura.** A c2 pode combinar mtime antigo com conteúdo novo ou perder toda a resposta por remoção do `.rc` durante a primeira passagem. O selo atual não exercita essa janela. `bin/rc_sem_colher.py:57–61,77–91,99–106,160–169` · `bin/tests/test_rc_sem_colher.sh:151–171`
2. **P2 tem correção proposta sem RED no leitor.** O selo fica verde se a contagem antiga ou a chamada sem `--min-idade 900` voltar no vercodex. `CLAUDE.md:28–29` · `bin/tests/test_rc_sem_colher.sh:202,243–248,271` · `vercodex.py:165–174` (anexo)
3. **A promessa de 15 minutos reais excede o que a marca prova.** A borda `899/900/901` morde para a data inferida. A colisão de dia ainda permite listar após 10 segundos reais. `bin/rc_sem_colher.py:13–17,110–118,130–138` · `bin/tests/test_rc_sem_colher.sh:182–186,286`

## 27/11 10:55 · sessao 6d8bb13e8944 · E4C-F5-INSTANTE · construtor · origem=Code

Implementei a cura3c na cópia. A deduplicação do veto K6 agora usa a autoria resolvida pelo próprio escritor da trilha. [observ.py:113](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:113) · [materializacao.py:858](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:858)

- `core/observ.py::evento` ganhou `unica=False`. Quando ativado, retorna a linha existente com a mesma ação, modelo, objeto, descrição, fotos, usuário e ator, quando o campo está disponível. A consulta e a criação usam o mesmo `engolir` e o mesmo savepoint. A docstring atribui a serialização ao chamador, pela trava do dono. [observ.py:94](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:94) · [observ.py:127](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:127) · [observ.py:138](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:138)
- `chamados/services/materializacao.py::trilha_do_veto_k6` monta a foto e chama `evento(..., unica=True)`. A busca na cópia confirmou que esse é o único chamador de `evento` que passa `unica`. [materializacao.py:846](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:846)
- `chamados/tests/test_e4c_f5_c4_red.py::Item13PorPKTest.test_RED_item13_veto_repetido_fora_de_request_deduplica_por_usuario_e_ator` usa relógio congelado e autor explícito. Exige uma linha para o comando que já havia vetado e uma única linha para o comando que veta duas vezes. Pela leitura do código, a cura3b produziria zero linhas para o segundo comando, e cada mutante produziria duas. A execução está **[nao verificado]**. [test_e4c_f5_c4_red.py:983](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:983) · [test_e4c_f5_c4_red.py:1068](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1068)
- `chamados/tests/e4c_f5_c4red2_mutantes.py::veto_sem_deduplicacao` faz o M44 trocar `unica=True` por `unica=False` na trilha. `evento_ignora_unica` faz o M46 pular a consulta no escritor. Ambos apontam para a mesma asserção do RED novo. [e4c_f5_c4red2_mutantes.py:757](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:757) · [e4c_f5_c4red2_mutantes.py:768](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:768)

O diff inteiro da cópia contra `0ba8b22b7b486543caaced318666c264a03b1e81` teve **0** linhas em `^\+.*except`. A sintaxe dos quatro arquivos e o ponto único de aplicação de cada mutante foram verificados estaticamente. A suite e as mordidas ficam **[nao verificado]**.

**Comandos para o Code**

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_veto_repetido_fora_de_request_deduplica_por_usuario_e_ator
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M44VetoSemDeduplicacao.test_mordida
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M46EventoIgnoraUnica.test_mordida
```

## 27/11 10:51 · sessao 2f6e479709bf · E4C-F5-INSTANTE · construtor · origem=Code

Implementei a cura3b na cópia. Após o rollback da validação, `validar_pergunta` abre um `atomic` próprio, toma a trava existente do dono, relê a pergunta e registra o veto com a foto restaurada. A execução dos testes e a mordida dos mutantes ficam **[nao verificado]**. [validacao.py:834](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:834) [nao verificado]

- **Trava.** O rollback remove o registro adquirido no savepoint desfeito. O novo bloco usa `PerguntaDisputa.trava_na_ordem` só com o dono, como já faz `validar_lote_classe_a` antes da releitura. A trilha consulta e grava `LogAuditoria`, sem adquirir trava de Empresa, preservando a ordem Empresa → dono. [trava_do_colaborador.py:16](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/trava_do_colaborador.py:16) · [validacao.py:1171](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:1171) · [materializacao.py:871](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:871) · [observ.py:133](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:133)
- **Deduplicação.** `trilha_do_veto_k6` procura uma linha com a mesma pergunta, autoria, ação, motivo e fotos completas. A autoria usa o usuário efetivo quando presente e a identificação de sistema registrada nos demais casos. A criação segue por `core.observ.evento`. A validação inicial, o laço de materialização e a entrada do passivo já tomam a trava do dono antes de chegar à trilha. [materializacao.py:846](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:846) · [observ.py:108](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:108) · [validacao.py:755](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:755) · [validacao.py:795](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:795) · [materializacao.py:2317](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2317) · [passivo_validado.py:393](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/passivo_validado.py:393) · [passivo_validado.py:585](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/passivo_validado.py:585)
- **RED e mutantes.** M44 desliga a deduplicação, de modo que o RED existente deve voltar à contagem de duas linhas. O RED pós-rollback exige uma única linha com as fotos restauradas e observa o registro real da trava nas duas recusas finais. M45 retira a aquisição do bloco final e deve falhar nessa última asserção em uma thread. A fixture usa o auxiliar existente para remover os registros das travas da preparação. Contenção entre transações **[nao verificado]**. [test_e4c_f5_c4_red.py:1055](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1055) · [test_e4c_f5_c4_red.py:1068](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1068) · [test_f5_v3_guarda_sob_trava.py:28](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_f5_v3_guarda_sob_trava.py:28) · [e4c_f5_c4red2_mutantes.py:756](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:756)
- **Isolamento e M16.** Cada porta do teste do item 13 agora roda em um savepoint revertido, com as asserções originais. M16 continua apontado para a asserção que exige fotos completas nas duas metades de cada porta. O dublê sem banco já substitui `trilha_do_veto_k6` inteira e dispensa ajuste. [test_e4c_f5_c4_red.py:1151](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1151) · [test_e4c_f5_c4_red.py:1024](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1024) · [e4c_f5_c4red2_mutantes.py:253](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:253) · [e4c_f5_c4red2_mutantes.py:474](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:474) · [test_bug_par_sem_banco.py:153](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_bug_par_sem_banco.py:153) [nao verificado]
- **Multiempresa — CONTROLE por leitura estática.** A Empresa B é travada depois do primeiro dono e antes do próprio dono. A porta já passa todas as Empresas do lote à guarda, que deve levantar `EntregaNaoTravada` para B. [test_e4c_f5_c4_red.py:932](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:932) · [models.py:4867](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/models.py:4867) · [export.py:613](/home/ronald/wt-codex-e4c-f5-instante-012038/app/folha/export.py:613)

A checagem estática confirmou que os quatro arquivos compilam, que M44 e M45 têm um ponto de aplicação cada e que o patch adiciona **0** linhas em `^\+.*except`. As asserções existentes do item 13 e o RED do item 6 foram preservados. [validacao.py:841](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:841) · [materializacao.py:846](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:846) · [test_e4c_f5_c4_red.py:634](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:634) · [e4c_f5_c4red2_mutantes.py:756](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:756)

**Comandos para o Code**

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_veto_repetido_deixa_uma_linha_de_trilha
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_veto_pos_rollback_repetido_audita_uma_vez_sob_trava
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_materializacao_e_passivo_preservam_e_auditam_por_pk
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_c4_red.EstoqueConcorrenteComEntregaTest.test_porta_do_estoque_exige_todas_as_empresas_antes_do_primeiro_dono
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_bug_par_sem_banco.BugParSemBancoTest.test_unidade_entregue_sai_antes_de_recusa_e_carimbo
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M16TrilhaVazia.test_mordida
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M44VetoSemDeduplicacao.test_mordida
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M45TrilhaPosRollbackSemTrava.test_mordida
```

## 27/11 10:50 · sessao d0ab90c9cfc0 · E4C-F5-INSTANTE · sonda · origem=Code

**1. Casos (i): zero no recorte da sonda.** A saída é consistente com o script. Os subtotais por mês fecham com os totais por empresa, e `FIM 701 0` indica 701 candidatas, todas da classe (ii). A incidência fora do recorte ou após a coleta fica **[nao verificado]**. (`r1.md:90–96`, saída de `sonda.py:3–14`)

| Empresa | Total (i) | Casos (i) por mês de `data_evento` | Fonte |
|---|---:|---|---|
| 2 | 0 | 2026-08: 0 · 2026-09: 0 · 2026-10: 0 · 2026-11: 0 | Saída de `sonda.py:3,6–9` |
| 3 | 0 | 2026-10: 0 · 2026-11: 0 | Saída de `sonda.py:4,10–11` |
| 4 | 0 | 2026-10: 0 · 2026-11: 0 | Saída de `sonda.py:5,12–13` |

**2. Competência entregue: 0 de 0.** O script coleta até dez casos da classe (i). Com a amostra vazia, nenhuma consulta de entrega K6 foi feita pelo ramo da amostra. (`r1.md:73–88`, saída de `sonda.py:13–14`)

**3. Guarda da cura2: nenhum caso da amostra para barrar.** Pelo anexo, a recusa ocorreria se, após C6 e a checagem de presença, a redução real ainda desse T1 e a coluna trouxesse T2 posterior sem linha correspondente. A última `RESPOSTA` persistida, sozinha, não estabelece que T1 continua vigente, pois `RECUSA` ou `REABERTURA` pode limpá-la. (`ver2_r.md:27`, `app/chamados/models.py:4455`, `app/chamados/models.py:4495`)

A origem de um T2 em produção, seja conservação legítima de legado ou escrita de `carimbar`, fica **[nao verificado]**. A cópia mostra que `carimbar` pode gravar `respondida_em` sem criar a `RESPOSTA` correspondente quando recebe a coluna nula e há texto. Se a instância já chega com T2 preenchido, a chamada preserva esse instante. (`app/chamados/models.py:3474–3482`, `app/chamados/models.py:3514`, `app/chamados/models.py:4419`)

## 27/11 10:49 · sessao 86646eb56732 · E4C-F5-INSTANTE · leitura · origem=Code

**E4C-F5-INSTANTE.** A cópia tem `DecisaoPergunta.linhas_da_projecao_sem_linha`, que verifica a correspondência por pergunta, tipo de ato e instante, e `DecisaoPergunta.leitura`, que combina as linhas reais com a projeção sem linha. `composta` usa essa leitura, e `par_ato_coluna` compara a presença da validação. O redutor `_reduzir`, a leitura `_reais_reduzidos` e a guarda de instante da cura2 estão ausentes da cópia. A confirmação do SHA por Git, a versão implantada e a incidência atual em produção ficam **[nao verificado]**. [app/chamados/models.py:4416](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4416) · [app/chamados/models.py:4473](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4473) · [app/chamados/models.py:4507](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4507) · [app/chamados/models.py:4518](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4518) · [app/chamados/models.py:3492](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3492)

### Sonda

No ambiente de produção, abra `python -B manage.py tenant_command shell --schema=SEU_SCHEMA --interface=python` e cole o bloco. A sonda usa uma transação `REPEATABLE READ, READ ONLY` e imprime o estado atual do universo, com os dez menores PKs de (i). A sintaxe foi conferida em memória. A execução com banco fica **[nao verificado]**. [app/docs/APP-ATESTADO.md:71](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/APP-ATESTADO.md:71) · [app/chamados/tests/cef_c3_sonda.py:21](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/cef_c3_sonda.py:21)

A classe (i) usa a última `RESPOSTA` persistida na ordem `(ato_em, pk)` e a ausência de uma `RESPOSTA` correspondente na coluna. `leitura_antiga_em_T2` indica se a leitura antiga termina em T2. Uma recusa ou reabertura posterior pode limpar a resposta nessa leitura. As contagens mensais usam o mês civil de `data_evento`, como pedido. A entrega usa a competência da janela da empresa para esse dia, por `competencia_do_fato`, que chama `folha.export.competencia_entregue`. Sem `data_evento`, a pergunta entra na contagem e a entrega sai como `NA`. [app/chamados/models.py:4378](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4378) · [app/chamados/models.py:4438](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4438) · [app/chamados/models.py:4495](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4495) · [app/chamados/services/materializacao.py:803](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/materializacao.py:803) · [app/chamados/services/passivo_validado.py:91](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/passivo_validado.py:91)

```python
exec(r"""
from collections import Counter, defaultdict

from django.conf import settings
from django.db import connection, transaction
from django.db.models import Exists, OuterRef, Prefetch
from django.utils import timezone

from chamados.models import DecisaoPergunta as D, PerguntaDisputa
from chamados.services.materializacao import competencia_do_fato

schema = connection.schema_name
if not schema or schema == getattr(settings, "PUBLIC_SCHEMA_NAME", "public"):
    raise RuntimeError("Abra o shell no schema do tenant.")

respostas = D.objects.filter(pergunta_id=OuterRef("pk"), ato=D.RESPOSTA)
decisoes = D.objects.only(
    "pk", "pergunta_id", "ato", "ato_em", "ato_texto", "resposta_mantem_validacao",
)
candidatas = (
    PerguntaDisputa.objects.filter(
        Exists(respostas),
        respondida_em__isnull=False,
        validada_em__isnull=True,
        materializada_em__isnull=True,
    )
    .select_related("disputa__colaborador__empresa")
    .only("data_evento", "respondida_em", "validada_em", "resposta_colab",
          "disputa__colaborador__empresa__dia_inicio_competencia")
    .prefetch_related(Prefetch("decisoes", queryset=decisoes))
    .order_by("pk")
)

totais = Counter()
por_empresa = defaultdict(Counter)
por_mes = defaultdict(Counter)
amostra = []

with transaction.atomic(durable=True):
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        cursor.execute("SELECT current_schema(), pg_is_in_recovery()")
        schema_atual, em_recuperacao = cursor.fetchone()
        if schema_atual != schema or em_recuperacao:
            raise RuntimeError("A sonda exige o schema do tenant no banco primário.")

    for p in candidatas.iterator(chunk_size=500):
        reais = list(p.decisoes.all())
        ultima = next(d for d in reversed(reais) if d.ato == D.RESPOSTA)
        ja = {(p.pk, d.ato, d.ato_em) for d in reais}
        falta_resposta = any(
            linha["ato"] == D.RESPOSTA
            for linha in D.linhas_da_projecao_sem_linha([p], ja=ja)
        )
        leitura = D.leitura(p)
        classe = "i" if falta_resposta and ultima.ato_em < p.respondida_em else "ii"

        empresa = p.disputa.colaborador.empresa
        mes = p.data_evento.strftime("%Y-%m") if p.data_evento else "SEM_DATA_EVENTO"
        totais[classe] += 1
        por_empresa[empresa.pk][classe] += 1
        por_mes[empresa.pk, mes][classe] += 1

        if classe == "i" and len(amostra) < 10:
            k6 = competencia_do_fato(p, empresa)
            if k6 is None:
                competencia, entregue = "SEM_DATA_EVENTO", "NA"
            else:
                mes_k6, ano_k6, foi_entregue = k6
                competencia = f"{ano_k6:04d}-{mes_k6:02d}"
                entregue = "SIM" if foi_entregue else "NAO"
            ato_em = timezone.localtime(ultima.ato_em).isoformat()
            respondida_em = timezone.localtime(p.respondida_em).isoformat()
            amostra.append(
                f"PK {p.pk} empresa={empresa.pk} mes_data_evento={mes} "
                f"ato_em={ato_em} respondida_em={respondida_em} "
                f"competencia_k6={competencia} entregue={entregue} "
                f"leitura_antiga_em_T2={int(leitura.respondida_em == p.respondida_em)}"
            )

for empresa_id, n in sorted(por_empresa.items()):
    print(f"EMPRESA {empresa_id} i={n['i']} ii={n['ii']}")
for (empresa_id, mes), n in sorted(por_mes.items()):
    print(f"MES_DATA_EVENTO {empresa_id} {mes} i={n['i']} ii={n['ii']}")
for linha in amostra:
    print(linha)
print(f"FIM {totais['i'] + totais['ii']} {totais['i']}")
""")
```

### De onde pode vir T2

- **`PerguntaDisputa.carimbar` é o escritor de aplicação encontrado que preenche `respondida_em` sem registrar a `RESPOSTA` correspondente.** Com `resposta='responder'` ou `'validar'`, texto preenchido e `self.respondida_em` nulo, atribui `quando`. Se as guardas passam, grava a projeção por `save`. Nessa base, o próprio `carimbar(resposta='validar')` termina nesse `save`, sem registrar a foto `_sem_linha`. O fecho sem credor chama com `'responder'`. A absorção chama com `'validar'` e registra `ABSORCAO`. [app/chamados/models.py:3474](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3474) · [app/chamados/models.py:3504](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3504) · [app/chamados/services/disputa_emissao.py:2192](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:2192) · [app/chamados/services/materializacao.py:1108](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/materializacao.py:1108) · [app/chamados/services/materializacao.py:1129](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/materializacao.py:1129)
- **O acervo anterior à E1 também pode trazer o instante só na coluna.** A migração 0051 converteu as linhas antigas em `REABERTURA`. Na base, `_nascer` registra as assinaturas recebidas, e os escritores de resposta humana, automática e pela trilha registram a `RESPOSTA` no mesmo instante que gravam na coluna. [app/chamados/models.py:4419](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4419) · [app/chamados/migrations/0051_decisao_pergunta.py:41](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/migrations/0051_decisao_pergunta.py:41) · [app/chamados/models.py:3333](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3333) · [app/chamados/services/disputa_emissao.py:1562](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1562) · [app/chamados/services/disputa_emissao.py:1757](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1757) · [app/chamados/services/disputa_emissao.py:1793](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1793)
- **`save` e `descarimbar_em_lote` são mecanismos de persistência sem `registrar` próprio.** Nos chamadores do lote que passam `respondida_em`, o valor passado é `None`. O `descarimbar` unitário pode tirar a materialização preservando T2, como faz `desfazer_carimbo_sem_lastro` quando C6 volta a dar a pergunta por viva. [app/chamados/models.py:3840](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3840) · [app/chamados/models.py:3657](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3657) · [app/chamados/models.py:1739](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:1739) · [app/chamados/services/disputa_emissao.py:1661](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1661) · [app/chamados/services/validacao.py:476](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/validacao.py:476) · [app/chamados/models.py:3572](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3572) · [app/chamados/services/relavra_via.py:242](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/relavra_via.py:242)

**T2 só na coluna é possível por construção.** Para o caso misto, `carimbar` exige que o instante esteja nulo na instância que recebe, então preserva um T1 já preenchido nessa instância. A sonda mede a divergência persistida. A origem e a legitimidade de cada T2, bem como a chegada de cada candidata ao ponto de recusa da cura2, ficam **[nao verificado]**. [app/chamados/models.py:3471](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3471) · [app/chamados/models.py:3483](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3483)

## 27/11 10:47 · sessao 4c37eb8792c5 · ORFAS-VOLTOU · leitura · origem=Code

O `sem_evento=0` informado às 06:36 é a leitura do vigia do **carimbo por lâmpada**. O estoque de perguntas candidatas a reate é publicado em `logs/reconciliador.log`, na linha `orfas reataveis (UNIVERSO, MEDIDA ...)`. O total atual da frota, a empresa das quatro células e o ramo das 16 perguntas estão [nao verificado]. `app/ponto/management/commands/reconciliar_perguntas_orfas.py:29–36` `app/chamados/management/commands/reconciliar_chamados.py:97–115` `app/config/crons.py:395`

## 1. Porta de reate, chamadores e trilha

`reatar_pergunta_orfa(p, colab)` primeiro deriva um destino vivo, usando `p.data_evento` antes da data do contexto. Sem destino derivado, monta o histórico de chamados `resolvido` ou `fechado` do colaborador para `contexto_json.data_turno`, excluindo `disputa_supervisao`. Havendo históricos, para pergunta datada em dia que o juiz põe no alcance do O261(C), C6 viva esvazia o histórico e C6 morta casa pelo juiz `motivos_atendidos`. Fora desse alcance, inclusive dia entregue ou trancado, o histórico usa os mapas de categoria e subcategoria. `app/chamados/reconciliador.py:838–871,1711–1765` `app/chamados/catalogo/motor.py:30`

| Retorno | Predicado da saída | Fonte |
|---|---|---|
| `False` | Sem destino vivo derivado e sem data da pergunta nem data no contexto do motivo. | `app/chamados/reconciliador.py:1711–1725` |
| `True` | Histórico vazio e `lavrar_sem_turno_aberto` consegue carimbar. É o c1, que pode deixar a FK nula. | `app/chamados/reconciliador.py:403–417,1766–1775` |
| `True` | Histórico vazio, c1 devolve falso e `reemitir_chamado_do_dia` consegue costurar a pergunta ao chamado vivo. | `app/chamados/reconciliador.py:1032–1080,1774–1777` |
| `False` | Histórico vazio e c1 e c2 devolvem falso, ou o histórico tem mais de um candidato. | `app/chamados/reconciliador.py:1766–1779` |
| `False` | Histórico único e a costura recusa, ou a cerca veta a escrita, desfaz o ato e registra a retenção no chamado histórico. | `app/chamados/reconciliador.py:1800–1816` |
| `True` | Histórico único, costura aceita, S128 retorna normalmente e a cerca libera. A porta descarta o retorno do S128, que pode ter carimbado zero perguntas. | `app/chamados/reconciliador.py:1800–1816` `app/chamados/services/disputa_emissao.py:2033–2042,2087–2122` |
| `False` | Há destino vivo derivado, mas `reapontar_resolvedora` devolve recusa. | `app/chamados/reconciliador.py:1821–1825` |
| `True` | Há destino vivo derivado e `reapontar_resolvedora` aceita. | `app/chamados/reconciliador.py:1821–1826` |
| Exceção | O corpo não tem `raise` explícito. Falhas não tratadas das chamadas internas sobem. Os dois parses locais tratam `ValueError` convertendo a data a `None`. A cerca própria converte veto anotado em recusa. Com cerca externa, a exceção do ato pode subir para quem abriu essa cerca. | `app/chamados/reconciliador.py:1697–1826` `app/chamados/services/fio.py:1564–1592` |

O **c1** exige motivo `orfao_14h` ou `saida_sem_entrada`, ausência de resposta pelo juiz, ausência de carimbo, ausência de turno aberto pelo núcleo, C6 morta e carimbo aceito. O **c2** passa pelos gates de motivo, dia, colaborador ativo, isenção, ausência, veto da célula e motor. Para nascer ou renascer, aplica também horizonte, protesto que cobra, regra da parcial só lista e régua. Para os dois motivos de intervalo, exige C6 viva e dia dentro do alcance O261(C). Por fim precisa obter chamado vivo e passar pela costura. Como `abrir` vem antes da costura, `False` no c2 pode vir depois de uma escrita no chamado. `app/chamados/reconciliador.py:333–417,838–880,930–1080`

**Chamadores diretos na cópia** são `gerar_perguntas_disputa` na emissão e `reconciliar_perguntas_orfas_colab` no laço por colaborador. A batida chega ao laço pelo `post_save` de `Batida`. Também chega o `on_commit` de um chamado filho que sai de `VIVOS`, quando há pai vivo do mesmo colaborador que lista o PK do filho. Execuções concretas para as quatro células [nao verificado]. `app/chamados/services/disputa_emissao.py:468–478` `app/chamados/reconciliador.py:1583–1588,1674–1685` `app/ponto/signals.py:211–223` `app/chamados/signals.py:351–371`

**Trilha.** A costura grava destino, veredito lavrado e evento de célula. A porta escreve log `INFO` em recusas de costura e, no veto do histórico, usa `registrar_retencao` para gravar `retratacao_retida_competencia_entregue` no chamado. Ramos delegados podem gravar trilhas de nascimento de chamado, fechamento de disputa e `fecho_sem_credor` em pergunta. A autoria humana da tentativa precisa ser registrada pelo chamador. `app/chamados/services/fio.py:1082–1095` `app/chamados/reconciliador.py:493–509,1800–1826` `app/chamados/models.py:1037–1046,2199–2207` `app/chamados/services/disputa_emissao.py:2190–2202`

A porta existente para a sessão é **`core.observ.evento`**, com `modelo="PerguntaDisputa"`, `objeto_id=p.pk`, `ator` igual ao objeto `User` do executor e o motivo em `descricao`. `LogAuditoria` guarda `usuario` e `criado_em`. O helper de request `core.auditoria.registrar_log` delega a ela. A sonda usa o precedente de levantar erro dentro do `atomic` se `evento` devolver `None`. `app/core/observ.py:94–139` `app/core/models.py:295–307` `app/core/auditoria.py:8–30` `app/chamados/services/fio.py:1202–1208` `app/chamados/services/validacao.py:395–405,497–498`

## 2. Elegibilidade e campos

| Seletor | O que exige |
|---|---|
| `perguntas_orfas_reatavies(colab)` | `chamado_destino` e `materializada_em` nulos, disputa aberta do colaborador e motivo em `orfao_14h`, `saida_sem_entrada`, `intervalo_saida`, `intervalo_volta`. `app/chamados/reconciliador.py:1637–1661` `app/chamados/models.py:4169–4170` |
| `_orfas_abertas(colaborador_ids=None)` | `materializada_em` e `validada_em` nulos, disputa aberta, `data_evento` presente e motivo `orfao_14h` ou `saida_sem_entrada`. Aceita qualquer valor de `chamado_destino`, com recorte de colaboradores opcional. `app/chamados/reconciliador.py:1162–1176` |

Com destino nulo, **disputa fechada, `materializada_em` preenchido ou motivo fora dos quatro** já bastam, cada um, para ficar fora dos dois universos. `validada_em` preenchido, `data_evento` nulo ou motivo de intervalo podem tirar só do universo do carimbo. Os dois seletores leem as condições acima, sem filtrar `via_resolucao` ou `veredito_lavrado`. `app/chamados/reconciliador.py:1166–1176,1654–1661` `app/chamados/models.py:4169–4170`

Na **pergunta**, “sem via” corresponde a `via_resolucao == ""` e veredito nunca lavrado a `veredito_lavrado == ""`. `julgado_em` é o instante da lavra. Na **célula**, os campos são `veredito` e `veredito_via`, ambos anuláveis. Há ainda o sentido de **resposta sem veredito**, que usa a validação da leitura composta, a materialização, o juiz de resposta e a tranca. A sonda mostra os campos da pergunta e da célula. Qual leitura produziu a descrição raspada [nao verificado]. `app/chamados/models.py:2040–2045,2872–2906` `app/escala/models.py:1782–1785`

## 3. Vigia da frota e o zero histórico

- **Estoque para reate.** `reconciliar_chamados.Command.handle` soma `perguntas_orfas_reatavies(c).count()` para todos os colaboradores do tenant quando roda sem `--colab`. A configuração agenda `reconciliar_chamados --alarme` a cada 15 minutos e grava a saída em `logs/reconciliador.log`. É a linha para ler o número já medido. Ela conta o universo elegível do item 2, inclui colaboradores com evento pendente, dispensa o C6 e a tentativa de reate, e fica fora da soma do alarme. O placar de corridas grava metadados da execução e o código de saída. `app/chamados/management/commands/reconciliar_chamados.py:57–62,89–124` `app/config/crons.py:17–21,29,395,1908–1917` `bin/cron_run.sh:2–13`
- **Fio solto.** O auditor diário das 07:18 também publica `I8` em `logs/invariantes.log` e no HTML. Ele parte de perguntas com destino nulo, motivo atendível, colaborador ativo e C6 viva. A faixa `I8o` registra à parte os casos com ao menos dois candidatos e nenhum `entrada_timestamp`. `app/config/crons.py:542–543` `app/chamados/management/commands/auditar_invariantes_chamados.py:51–95,818–831,860–872,1866–1884`
- **`332 → 0` de 29/10.** O BACKLOG resume o pouso `ac721607`. O RELATO documenta `sem_evento=332`, semeadura de 379 eventos, 332 perguntas encerradas e `sem_evento=0` depois. A função continua sendo `orfas_pela_lampada_sem_evento`. Hoje ela mede o mesmo resíduo de carimbo por lâmpada. Em `_orfas_abertas`, exige pergunta sem resposta pelo juiz, lâmpada do próprio marco acesa, C6 viva, dia passado e encerrado, e colaborador sem evento vivo pendente. A comparação sustentada pelo dado fornecido é **`sem_evento` 0 em 29/10 e 0 em 27/11**. O estoque `orfas reataveis` de hoje está [nao verificado]. `app/docs/BACKLOG.md:17,810` `app/docs/RELATO.md:7010–7015` `app/chamados/reconciliador.py:1182–1233,1325–1341`

## 4. Por que o cron da manhã não reata

O **06:40** era o horário antigo. Desde 29/09, a configuração encadeia `reconciliar_perguntas_orfas` ao fim de `disparar_perguntas_competencia`, com `--alarme` e saída em `perguntas_orfas.log`. O comando sempre conta e imprime `[VIGIA] sem_evento=N (esperado 0)`. Com `--alarme` e `N > 0`, também imprime até 20 PKs em stderr e levanta `SystemExit(2)`. Sem a flag, termina depois da contagem. O corpo tem apenas leitura e saída. O BACKLOG registra que a falta de redirecionamento do log foi sanada e que o arquivo já voltara a ser gravado em 08/11. `app/config/crons.py:479–485,1528–1531,1548–1554,1646–1651` `app/ponto/management/commands/reconciliar_perguntas_orfas.py:24–36` `app/docs/BACKLOG.md:811`

Em **23/10**, VARREDURA 1b-iii parte 3 extraiu a porta do laço antigo e ligou o reate ao nascimento na emissão. O RELATO cita a cópia `2263122b`. Em **29/10**, `ac721607` moveu o **carimbo fantasma** ao consumidor de `EventoCelula`. Em **05/11**, JUIZ-VARREDURA-3, documentado no deploy `de9fd69b`, converteu o `reconciliar_chamados` de `*/15`, que ainda reatava, em vigia. `app/chamados/reconciliador.py:1688–1695` `app/docs/RELATO.md:8882–8888,7010–7015,13032–13041` `app/config/crons.py:2094–2104` `app/chamados/management/commands/reconciliar_chamados.py:1–16`

A emissão pode criar com FK nula quando o derivador termina com zero ou múltiplos candidatos após o desempate. A tentativa imediata só ocorre se a pergunta foi **criada nessa chamada**, segue com FK nula e tem motivo do grupo do dia. Pode ficar sem vínculo se a tentativa recusa, falha no savepoint ou só carimba pelo c1. Uma emissão posterior de outra pergunta não percorre o estoque nesse bloco. O consumidor de `EventoCelula` faz o carimbo e chama a fábrica para os dias do lote, e a fábrica deduplica o fato já representado mesmo quando sua pergunta segue sem destino. O estoque elegível volta à porta no laço do colaborador, acionado pela batida ou pelo evento de filho descrito acima. Se a ambiguidade some sem um desses gatilhos, ou a pergunta já está fora do seletor do laço, não há nova tentativa por esses caminhos. Qual condição ocorreu nas 16 [nao verificado]. `app/chamados/services/disputa_emissao.py:191–244,417–478` `app/chamados/reconciliador.py:1523–1537,1583–1588,1654–1685,1774–1775` `app/chamados/services/fabrica_da_celula.py:138–174,207–219,288–302` `app/chamados/management/commands/reconciliar_chamados.py:24–25`

## 5. Sonda para o shell do tenant

Abra `python -B manage.py tenant_command shell --schema=SEU_SCHEMA` e cole o `exec` abaixo. O modo padrão é `ensaio`. Em `aplicar`, preencha `ATOR_PK` com o PK do usuário executor. `FIM` conta como reatada a pergunta que saiu de FK nula para FK preenchida com retorno `True`. A foto do ensaio é tirada antes do rollback de cada pergunta. A porta expõe um booleano e alguns logs de ramo, sem diagnóstico estruturado que cubra todas as recusas. Por isso a saída por tentativa traz retorno e estado. Sintaxe conferida. Execução com Django e banco [nao verificado]. `bin/sonda_frota.sh:45–46` `app/core/observ.py:108–137` `app/chamados/reconciliador.py:1774–1826`

A sonda valida os quatro pares de PK e data, as quatro perguntas por célula e o vínculo ainda nulo sob a trava existente. Em `aplicar`, registra também as tentativas que devolvem `False`, pois o c2 pode escrever antes da recusa. Ela interrompe o `aplicar` se encontrar regularização externa confirmada e pendente do colaborador. O fechamento de uma disputa pode retomar essa regularização depois do commit e escrever batidas. A checagem lê a pendência existente naquele instante. Uma confirmação concorrente pode gravar seu estado por outra transação, e a ausência dessa concorrência está [nao verificado]. `app/chamados/models.py:2930–2949` `app/chamados/reconciliador.py:410–417,1032–1080,1800–1807` `app/chamados/signals.py:122–146` `app/chamados/services/regularizacao_ext.py:71–82,122–156,211–225,348–357`

```python
exec(r"""
from collections import Counter
from datetime import date

from django.conf import settings
from django.contrib.auth import get_user_model
from django.db import connection, transaction
from django.utils import timezone

from chamados.juizes import pergunta_viva
from chamados.models import PerguntaDisputa, RegularizacaoExterna
from chamados.reconciliador import (
    _orfas_abertas,
    perguntas_orfas_reatavies,
    reatar_pergunta_orfa,
)
from chamados.services.validacao import TrilhaNaoGravada
from core.observ import evento
from escala.models import CelulaDia

MODO = "ensaio"
ATOR_PK = None  # Em aplicar, PK do auth.User de quem executa.
AVAL = "ORFAS-VOLTOU-2711-1045"
DIAS = {
    123535: date(2026, 11, 1),
    123541: date(2026, 11, 7),
    123547: date(2026, 11, 13),
    123553: date(2026, 11, 19),
}


class DesfazerEnsaio(Exception):
    pass


def local(instante):
    return timezone.localtime(instante).isoformat() if instante is not None else None


def estado(p):
    return {
        "chamado_destino_id": p.chamado_destino_id,
        "materializada_em": local(p.materializada_em),
    }


def executar():
    if MODO not in ("ensaio", "aplicar"):
        raise ValueError("MODO deve ser ensaio ou aplicar")
    schema = getattr(connection, "schema_name", None)
    if not schema or schema == getattr(settings, "PUBLIC_SCHEMA_NAME", "public"):
        raise RuntimeError("Abra o shell no schema do tenant")
    if not transaction.get_autocommit():
        raise RuntimeError("A sonda precisa de uma transacao independente por pergunta")

    celulas = CelulaDia.objects.select_related("colaborador").in_bulk(DIAS)
    if {pk: c.data for pk, c in celulas.items()} != DIAS:
        raise RuntimeError("PKs e datas das celulas divergem do aval")
    donos = {c.colaborador_id for c in celulas.values()}
    if len(donos) != 1:
        raise RuntimeError("As quatro celulas precisam ter o mesmo colaborador")
    colab = celulas[next(iter(DIAS))].colaborador
    print("colaborador_id", colab.pk, "empresa_id", colab.empresa_id)
    for c in celulas.values():
        print("celula_id", c.pk, "data", c.data, "celula.veredito", c.veredito,
              "celula.veredito_via", c.veredito_via)

    qs = PerguntaDisputa.objects.select_related("celula", "disputa__colaborador")
    perguntas = list(qs.filter(celula_id__in=DIAS).order_by("celula_id", "pk"))
    pks = [p.pk for p in perguntas]
    reataveis = set(perguntas_orfas_reatavies(colab).filter(pk__in=pks)
                    .values_list("pk", flat=True))
    abertas = set(_orfas_abertas([colab.pk]).filter(pk__in=pks)
                  .values_list("pk", flat=True))

    for p in perguntas:
        print({
            "pk": p.pk, "celula_id": p.celula_id, "motivo": p.motivo,
            "data_evento": p.data_evento.isoformat() if p.data_evento else None,
            **estado(p), "validada_em": local(p.validada_em),
            "via_resolucao": p.via_resolucao, "veredito_lavrado": p.veredito_lavrado,
            "disputa_id": p.disputa_id, "disputa.fechada_em": local(p.disputa.fechada_em),
            "perguntas_orfas_reatavies": p.pk in reataveis,
            "_orfas_abertas": p.pk in abertas, "pergunta_viva": pergunta_viva(p),
        })

    if Counter(p.celula_id for p in perguntas) != {pk: 4 for pk in DIAS}:
        raise RuntimeError("A lista atual diverge das quatro perguntas por celula do aval")

    ator = None
    if MODO == "aplicar":
        if ATOR_PK is None:
            raise ValueError("Preencha ATOR_PK com o PK do usuario executor")
        ator = get_user_model().objects.get(pk=ATOR_PK)

    n_reatadas = 0
    for original in perguntas:
        try:
            with transaction.atomic():
                PerguntaDisputa.trava_do_colaborador(colab.pk)
                p = qs.get(pk=original.pk)
                if (p.celula_id != original.celula_id or
                    p.colaborador_id != colab.pk or
                    p.disputa.colaborador_id != colab.pk or
                    p.chamado_destino_id is not None):
                    raise RuntimeError(f"Pergunta {p.pk} diverge do alvo orfao do aval")

                if MODO == "aplicar" and RegularizacaoExterna.objects.filter(
                    colaborador_id=colab.pk, status="confirmada", materializada_em__isnull=True,
                ).exists():
                    raise RuntimeError(f"Colaborador {colab.pk} tem regularizacao confirmada pendente")

                antes = estado(p)
                retorno = reatar_pergunta_orfa(p, p.disputa.colaborador)
                p.refresh_from_db()
                depois = estado(p)
                resultado = {"pk": p.pk, "retorno": retorno, **depois}
                if MODO == "ensaio":
                    raise DesfazerEnsaio(resultado)

                log = evento(
                    "reatar_pergunta_orfa", "PerguntaDisputa", p.pk,
                    descricao=f"{AVAL} | Tentativa de reatar a pergunta pela porta existente",
                    antes=antes, depois={**depois, "retorno": retorno}, ator=ator,
                )
                if log is None:
                    raise TrilhaNaoGravada(f"{AVAL} | Pergunta {p.pk}")
        except DesfazerEnsaio as ensaio:
            resultado, = ensaio.args
        print(resultado)
        if resultado["retorno"] is True and resultado["chamado_destino_id"] is not None:
            n_reatadas += 1
    print(f"FIM {len(perguntas)} {n_reatadas}")


executar()
""")
```

## 27/11 10:44 · sessao 52b726a95a4a · E4C-F5-INSTANTE · construtor · origem=Code

**Parei conforme a condição do P3.** Em `validar_pergunta`, o veto detectado após a tentativa de plantio desfaz o `atomic` que adquiriu a trava do dono. Só depois do rollback a função relê a pergunta e chama `_recusa_da_entrega`, que chama `trilha_do_veto_k6`. A trava adquirida naquele bloco já caiu. [validacao.py:755](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:755) · [validacao.py:834](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:834) · [validacao.py:237](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:237) · [trava_do_colaborador.py:16](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/trava_do_colaborador.py:16)

- **Caminhos com trava.** O veto inicial da validação chama a trilha dentro do `atomic` que tomou a trava. O laço da materialização e o fluxo do passivo também tomam a trava antes de chegar a `_materializar_uma`. [validacao.py:755](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:755) · [validacao.py:795](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:795) · [materializacao.py:2228](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2228) · [materializacao.py:2300](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2300) · [materializacao.py:2318](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2318) · [passivo_validado.py:393](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/passivo_validado.py:393)
- **M16.** Pela leitura do código, o mutante apaga as fotos e a descrição. A asserção de fotos completas reprova as duas metades em cada porta. Execução **[nao verificado]**. [e4c_f5_c4red2_mutantes.py:253](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:253) · [e4c_f5_c4red2_mutantes.py:474](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:474) · [test_e4c_f5_c4_red.py:996](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:996)
- **Multiempresa.** Controle esperado por leitura estática. A porta passa todas as Empresas do lote à guarda, que encontra o primeiro dono e lança `EntregaNaoTravada` pela Empresa registrada depois dele. O teste novo ficou pendente com a parada. Execução **[nao verificado]**. [models.py:4868](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/models.py:4868) · [export.py:613](/home/ronald/wt-codex-e4c-f5-instante-012038/app/folha/export.py:613) [nao verificado]

Nenhum arquivo foi alterado, nenhum `except` foi adicionado e nenhum teste foi executado. A deduplicação, o isolamento e o M44 ficaram pendentes. [nao verificado]

Comandos dos casos existentes para o Code. [test_e4c_f5_c4_red.py:1034](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1034) · [e4c_f5_c4red2_mutantes.py:474](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:474)

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_veto_repetido_deixa_uma_linha_de_trilha
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M16TrilhaVazia.test_mordida
```

## 27/11 10:44 · sessao fd1effd0b35b · - · - · origem=Ronald (tmux)

- A porta retorna `True` quando a costura no destino derivado funciona, quando a costura no único histórico funciona e S128 termina, quando c1 lavra ou quando c2 reemite e costura. Retorna `False` sem data no caminho histórico, quando há vários históricos, quando não há histórico e c1/c2 recusam, quando a costura recusa ou quando a cerca desfaz o histórico. O corpo não tem `raise` explícito. Os dois parses tratam `ValueError`, e falhas não tratadas propagam. A cerca própria converte veto em estado de recusa. `app/chamados/reconciliador.py:1711–1826`, `app/chamados/services/fio.py:1564–1592`.
- O histórico considera chamados encerrados do colaborador com o `data_turno` em questão, exceto `disputa_supervisao`. Dentro do alcance O261(C), uma pergunta viva esvazia essa lista e o encaixe de motivo usa `motivos_atendidos`. Fora do alcance, usa os mapas legados. `app/chamados/reconciliador.py:838–871`, `app/chamados/reconciliador.py:1740–1765`.
- A costura não lavra auditoria de autoria da sessão. Grava destino, campos do veredito e desperta outbox. A recusa de costura tem log `INFO` e o veto histórico pode registrar `retratacao_retida_competencia_entregue` no chamado. S128 pode fechar outras perguntas do colaborador e gravar `fecho_sem_credor`. Para autoria humana em shell, a porta existente é `core.observ.evento`, com `ator` como objeto `User` e motivo em `descricao`. `LogAuditoria.criado_em` registra o instante. O precedente `BUG-PAR-103` verifica o retorno da trilha dentro da mesma transação e lança erro se for `None`. `app/chamados/services/fio.py:1082–1095`, `app/chamados/reconciliador.py:493–509`, `app/chamados/reconciliador.py:1800–1825`, `app/chamados/services/disputa_emissao.py:2158–2202`, `app/core/observ.py:94–139`, `app/core/models.py:295–307`, `app/chamados/services/validacao.py:395–405`.
- `perguntas_orfas_reatavies` seleciona destino e materialização nulos, disputa aberta do colaborador e um dos quatro motivos do dia. `_orfas_abertas` seleciona materialização e validação nulas, disputa aberta, dia presente e os dois motivos `orfao_14h` e `saida_sem_entrada`. Qualquer destino cabe neste segundo filtro. Disputa fechada, materialização presente ou motivo fora dos quatro podem deixar destino nulo e excluir dos dois. `via_resolucao == ''` e `veredito_lavrado == ''` representam os campos relatados. `app/chamados/reconciliador.py:1162–1176`, `app/chamados/reconciliador.py:1654–1661`, `app/chamados/models.py:4169–4170`, `app/chamados/models.py:2872–2906`.
- Chamadores diretos são emissão e o laço de conciliação por colaborador. Este laço é acionado por `post_save` de batida e `on_commit` quando um filho sai de `VIVOS` e tem pai vivo referenciando-o. O outbox chega à emissão via fábrica, que exclui da nova emissão um fato que já tem pergunta canônica, mesmo com destino nulo. Na emissão, a tentativa é somente para criada agora, destino nulo e motivo do grupo dos quatro. Nenhuma evidência de que esses gatilhos tenham ou não ocorrido para as 16 foi fornecida. `app/chamados/services/disputa_emissao.py:462–478`, `app/chamados/reconciliador.py:1534–1537`, `app/chamados/reconciliador.py:1587–1588`, `app/chamados/reconciliador.py:1675–1684`, `app/ponto/signals.py:211–223`, `app/chamados/signals.py:351–371`, `app/chamados/services/fabrica_da_celula.py:138–174`, `app/chamados/services/fabrica_da_celula.py:207–219`, `app/chamados/services/fabrica_da_celula.py:288–302`. Última frase [nao verificado].
- O `reconciliar_chamados --alarme` de `*/15` publica `orfas reataveis (UNIVERSO, MEDIDA ...)` no `reconciliador.log`: soma o seletor por colaborador e a soma de alarme fica separada dessa medida. O auditor diário 07:18 traz ainda I8, que aplica C6, motivo atendível, colaborador ativo, destino nulo e separa I8o. O `332 → 0` documentado em 29/10 era `orfas_pela_lampada_sem_evento`, função que permanece consultada por `reconciliar_perguntas_orfas`, com ou sem `--alarme`. A flag só muda a saída quando há residual, com código 2. Conceitos, recortes e épocas estão documentados. `app/config/crons.py:395`, `app/config/crons.py:542`, `app/chamados/management/commands/reconciliar_chamados.py:97–124`, `app/chamados/management/commands/auditar_invariantes_chamados.py:818–831`, `app/chamados/management/commands/auditar_invariantes_chamados.py:860–872`, `app/docs/RELATO.md:7010–7015`, `app/chamados/reconciliador.py:1325–1341`, `app/ponto/management/commands/reconciliar_perguntas_orfas.py:24–36`.
- A porta desceu à emissão em 23/10 (`2263122b`) e a varredura que reatava foi rebaixada em 05/11 (`de9fd69b`). O ENC do carimbo foi rebaixado em 29/10 (`ac721607`) e roda por fim do gatilho, sem minuto próprio. `app/chamados/reconciliador.py:1691–1695`, `app/docs/RELATO.md:8882–8888`, `app/docs/RELATO.md:13032–13041`, `app/docs/RELATO.md:7010–7015`, `app/config/crons.py:1528–1531`, `app/config/crons.py:1548–1554`.
- A sonda deve tirar a foto com uma instância nova por chamada e usar `refresh_from_db()` dentro do atomic antes do rollback, pois S128 trabalha com outras instâncias. A saída booleana não carrega diagnóstico completo. `True` pode ser c1 sem novo destino, e `False` no c2 pode vir depois de `abrir`. A autoria pode registrar a tentativa e seu retorno. `app/chamados/juizes.py:911–952`, `app/chamados/services/disputa_emissao.py:2038–2042`, `app/chamados/reconciliador.py:403–417`, `app/chamados/reconciliador.py:1032–1044`, `app/chamados/reconciliador.py:1076–1080`, `app/chamados/reconciliador.py:1774–1826`.

## 27/11 10:43 · sessao 9df9f5737cd0 · - · - · origem=Ronald (tmux)

O cron `reconciliar_chamados --alarme` publica `orfas reataveis (UNIVERSO, MEDIDA -- o reatar nao tem predicado seco)` a cada 15 minutos em `logs/reconciliador.log`, no schema `juliani`. Soma `perguntas_orfas_reatavies(colab).count()` para todos os colaboradores quando executado sem `--colab`. O conjunto tem destino nulo, disputa aberta, `materializada_em` nulo e um dos quatro motivos do dia. A contagem dispensa `pergunta_viva`, tentativa de reatamento e ausência de evento pendente. Seu valor fica fora da soma do alarme. O placar de corridas armazena início, fim, duração e código de saída. [app/config/crons.py:17-21,395,1908-1917; app/chamados/management/commands/reconciliar_chamados.py:57-62,89-124; app/chamados/reconciliador.py:1637-1661; app/chamados/models.py:4169-4170; bin/cron_run.sh:2-13]

O auditor diário também publica `I8`, que mede perguntas vivas pelo C6, de colaboradores ativos, com destino nulo e motivo atendível. `I8o` destaca à parte o piso de casos com pelo menos dois candidatos daquele dia e nenhum `entrada_timestamp`. A execução das 07:18 gera `logs/invariantes.log` e `/app/media/invariantes_chamados.html`. [app/config/crons.py:542-543; app/chamados/management/commands/auditar_invariantes_chamados.py:51-95,818-831,860-872,1866-1884]

O `332 → 0` do BACKLOG foi `sem_evento` antes e depois da semeadura e do consumo de eventos no pouso `ac721607`, documentado no RELATO. A função `orfas_pela_lampada_sem_evento` ainda existe. Ela conta as perguntas de `_orfas_abertas` que `veredito_orfa` manda encerrar e cujos colaboradores estão sem evento pendente. O universo `_orfas_abertas` inclui perguntas com destino preenchido. O dado raspado permite comparar `sem_evento=0` de hoje com `sem_evento=0` de 29/10. O total de hoje de candidatas a reatar e o I8 são [nao verificado]. [app/docs/BACKLOG.md:17,810; app/docs/RELATO.md:7010-7015; app/config/crons.py:2102-2104; app/chamados/reconciliador.py:1166-1176,1182-1233,1325-1341; app/ponto/management/commands/reconciliar_perguntas_orfas.py:29-36]

A cópia confirma o gatilho indireto adicional `_filho_morto_acorda_o_pai`. Quando um filho sai de `VIVOS` e existe pai vivo do mesmo colaborador que lista seu PK, o receiver registra callback `on_commit` que chama `reconciliar_chamados_colab`, que percorre as órfãs elegíveis. [app/chamados/signals.py:351-371; app/chamados/reconciliador.py:1583-1588,1676-1684]

## 27/11 10:43 · sessao 57695d33e151 · - · - · origem=Ronald (tmux)

- O antigo minuto 06:40 saiu em 29/09. O comando atual roda encadeado após `disparar_perguntas_competencia`, com `--alarme` e log `perguntas_orfas.log`. Com ou sem o argumento, mede `orfas_pela_lampada_sem_evento` e imprime o tamanho. Com o argumento e resultado positivo, também escreve até 20 PKs em stderr e levanta `SystemExit(2)`. [crons.py:479–485](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:479) · [crons.py:1551–1554](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:1551) · [reconciliar_perguntas_orfas.py:24–36](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/reconciliar_perguntas_orfas.py:24)
- Em 23/10, VARREDURA classe B 1b-iii parte 3, a emissão passou a chamar a porta extraída do antigo laço de `reconciliar_chamados`. O relato cita a cópia `2263122b`. Em 29/10, `ac721607` moveu o carimbo `fantasma` ao consumidor de `EventoCelula` e converteu `reconciliar_perguntas_orfas` em vigia. Em 05/11, JUIZ-VARREDURA-3, `reconciliar_chamados` também virou vigia. [reconciliador.py:1688–1695](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/reconciliador.py:1688) · [RELATO.md:8882–8888](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/RELATO.md:8882) · [LAPIDES.md:785](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LAPIDES.md:785) · [reconciliar_chamados.py:1–16](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/reconciliar_chamados.py:1)
- A emissão chama diretamente para pergunta recém-criada com destino nulo e motivo do dia, em savepoint próprio. O receiver `post_save` da batida e o receiver de chamado filho que sai de `VIVOS` também chegam ao reatar pelo laço do colaborador. O segundo exige pai vivo do mesmo colaborador que liste o filho no contexto e chama após commit. [disputa_emissao.py:462–478](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:462) · [ponto/signals.py:211–223](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/signals.py:211) · [chamados/signals.py:351–371](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/signals.py:351) · [reconciliador.py:1587–1588, 1676–1684](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/reconciliador.py:1587)
- O derivador retorna destino nulo quando termina com zero ou múltiplos candidatos. A pergunta pode ficar sem vínculo quando o reatar da emissão recusa, falha ou apenas carimba pelo c1. Emissão posterior de outra linha não revisita a existente pelo bloco de nascimento. Batida e filho só percorrem as elegíveis do colaborador. O outbox carimba e chama a fábrica para datas do lote, cuja emissão deduplica fatos já representados. Assim, mudança de elegibilidade sem batida, sem evento elegível do filho e sem novo nascimento que passe pela porta pode deixar o estoque sem nova tentativa de reatar. Causa concreta das 16 e eventos posteriores [nao verificado]. [disputa_emissao.py:191–244, 468–478](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:191) · [reconciliador.py:341–417, 1523–1537, 1640–1684](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/reconciliador.py:1523) · [fabrica_da_celula.py:139–157, 288–302](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/fabrica_da_celula.py:139)
- O histórico literal do `332 → 0` é o `sem_evento` do carimbo. O relato registra 332 antes, 379 eventos semeados, 332 perguntas encerradas e 0 depois. O BACKLOG registra o retorno do log em 08/11. [RELATO.md:7010–7015](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/RELATO.md:7010) · [BACKLOG.md:811](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/BACKLOG.md:811)

## 27/11 10:42 · sessao 8bc3e85087ce · E4C-F5-INSTANTE · verificador · origem=Code

## E4C-F5-INSTANTE · verificação do delta

Conferi `ad10296c4` sobre `d5b417695` na cópia indicada, em somente leitura. As conclusões sobre a mordida dos REDs vêm do código e da mutação correspondente. Execução dos testes, `makemigrations --check`, implantação e ocorrência em produção ficam **[nao verificado]**. O material fornecido contém o pedido, os recortes de leis, o registro de juízes e o diff. As perguntas com dados montados nos testes são fixtures. `app/chamados/tests/test_e4c_f5_c4_red.py:121`, `app/chamados/tests/test_e4c_f5_c4_red.py:962`.

### 1. P1 a P5

| Achado | Resultado do delta e mordida |
|---|---|
| **P1 · alta** | **Cura na origem o falso acordo de instantes.** Guarda e comparador reduzem as mesmas linhas reais. A guarda acrescenta os atos que o carimbo está prestes a gravar. M36 repõe `composta` e o RED do `carimbar` pega o defeito original em resposta e validação quando a coluna está **depois** do ato. O inverso é controle: a linha real posterior já vencia na leitura antiga. M37 pega o mesmo defeito nas duas colunas de uma instância relida do ORM. Ele chama a guarda diretamente. O RED anterior de absorção verifica a reconferência e o rollback pela porta em outro cenário. `app/chamados/models.py:4620`, `app/chamados/models.py:4637`, `app/chamados/models.py:4688`, `app/chamados/tests/test_e4c_f5_c4_red.py:121`, `app/chamados/tests/test_e4c_f5_c4_red.py:226`, `app/chamados/tests/test_e4c_f5_c4_red.py:484`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:685`. |
| **P2 · média** | **Cura na origem os dois desvios da lista.** O DRY e a porta consultam `veto_do_estoque` por campo. M38 tira essa consulta do DRY e a conversa volta à lista. M39 restaura a precedência por PK e o `b` novo volta a esconder o `b` indeterminado do outro campo. As duas asserções mordem o comportamento original. O quadro agregado continua tendo precedência por PK, coberta separadamente. `app/chamados/models.py:4828`, `app/chamados/models.py:4888`, `app/chamados/management/commands/estoque_ato_x_coluna.py:46`, `app/chamados/management/commands/estoque_ato_x_coluna.py:55`, `app/chamados/tests/test_e4c_f5_c4_red.py:498`, `app/chamados/tests/test_e4c_f5_c4_red.py:705`, `app/chamados/tests/test_e4c_f5_c4_red.py:783`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:701`. |
| **P3 · média** | **Sem cura.** Duas validações idênticas vetadas chamam o escritor duas vezes. O RED novo exige uma única linha e pega a duplicata original. O teste sem banco prova uma chamada com pergunta, autor e motivo em **uma tentativa**. M16 corrompe as fotos e a descrição. A repetição segue sem mutante próprio. `app/chamados/services/validacao.py:237`, `app/chamados/services/validacao.py:795`, `app/chamados/services/materializacao.py:846`, `app/core/observ.py:133`, `app/chamados/tests/test_e4c_f5_c4_red.py:1034`, `app/chamados/tests/test_bug_par_sem_banco.py:153`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:253`. |
| **P4 · média** | **Cura a lacuna de prova para dados persistidos. A cobertura dos pendentes em memória continua parcial.** O teste novo exercita os dois carregadores reais: `exigencia_das_perguntas` pela decisão retornada e `carga_da_tarefa` pelos dados que ela carregou em `_leituras` e `_passivo`. M41, M42 e M43 retiram, respectivamente, os cortes da linha, da coluna e do passivo. O teste novo já tem esses cortes na base. A injeção antiga continua sendo a prova específica de `_atos_do_carimbo` pendentes, que as duas faces públicas não recebem ao recarregar do ORM. `app/chamados/services/tarefa_do_dia.py:393`, `app/chamados/services/tarefa_do_dia.py:419`, `app/chamados/services/tarefa_do_dia.py:437`, `app/chamados/services/tarefa_do_dia.py:475`, `app/chamados/services/tarefa_do_dia.py:537`, `app/chamados/tests/test_e4c_f5_c4_red.py:260`, `app/chamados/tests/test_e4c_f5_c4_red.py:336`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:733`. |
| **P5 · média** | **Cura na origem a aceitação da ordem inversa pela porta.** O RED registra dono → Empresa no mesmo `atomic` e exige `EntregaNaoTravada` pelo motivo específico. M40 desliga só a conferência da ordem e repõe a aceitação antiga. A conferência ocorre antes da leitura do K6 e do `INSERT`. A aquisição das duas travas já aconteceu quando a porta confere. `app/chamados/models.py:4773`, `app/chamados/models.py:4874`, `app/chamados/models.py:4892`, `app/folha/export.py:602`, `app/chamados/tests/test_e4c_f5_c4_red.py:919`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:723`. |

### 2. Quem escreve e quem lê

| Campo ou valor | Escrita e leitura |
|---|---|
| **`respondida_em`, `resposta_colab`, `validada_em`, `validada_por_id`** | A resposta humana escreve instante e texto e limpa a validação anterior. A automática escreve instante e texto. O carimbo pela trilha escreve o instante de resposta. `_carimbar_validacao` escreve instante e autor da validação. `carimbar` preenche os instantes faltantes conforme a opção e o texto, e pode restaurar a foto em memória. O nascimento aceita assinaturas e registra os atos correspondentes. Reabertura/recusa, devolução e ressuscitação limpam os campos aplicáveis. A guarda lê os dois instantes crus, `linhas_da_projecao_sem_linha` lê também texto e autor, o comparador lê os instantes, e a trilha K6 fotografa texto e instantes. C6 e a validação leem a composta. `app/chamados/services/disputa_emissao.py:1530`, `app/chamados/services/disputa_emissao.py:1661`, `app/chamados/services/disputa_emissao.py:1757`, `app/chamados/services/disputa_emissao.py:1793`, `app/chamados/services/validacao.py:170`, `app/chamados/services/validacao.py:308`, `app/chamados/services/validacao.py:387`, `app/chamados/services/validacao.py:487`, `app/chamados/models.py:1730`, `app/chamados/models.py:3322`, `app/chamados/models.py:3478`, `app/chamados/models.py:3531`, `app/chamados/models.py:4509`, `app/chamados/models.py:4622`, `app/chamados/models.py:4688`, `app/chamados/juizes.py:1499`, `app/chamados/services/validacao.py:768`, `app/chamados/services/materializacao.py:855`. |
| **Linhas reais, pendentes e passivo** | As portas montam os atos e `DecisaoPergunta.registrar` insere as linhas. Resposta humana deixa a marca de manutenção da validação falsa. Resposta automática, carimbo pela trilha e foto do passivo declaram a marca verdadeira. `carimbar` cria e remove `_atos_do_carimbo` na instância. `leitura`, a vista de horizonte e a guarda de instante leem os pendentes. `_reduzir` consome `ato_em`, ordem, PK, `ato`, `ato_texto` e o efeito de limpar a validação. Comparador e reprojeção leem a redução real. A presença de validação continua lendo `composta`, que também incorpora o passivo sintetizado. `app/chamados/models.py:3477`, `app/chamados/models.py:3520`, `app/chamados/models.py:3539`, `app/chamados/models.py:4511`, `app/chamados/models.py:4530`, `app/chamados/models.py:4557`, `app/chamados/models.py:4572`, `app/chamados/models.py:4642`, `app/chamados/models.py:4648`, `app/chamados/models.py:4675`, `app/chamados/models.py:4688`, `app/chamados/models.py:4778`, `app/chamados/services/tarefa_do_dia.py:419`. |
| **`marco`, `entrada_origem`, `entrada_em`, `div`** | `marco_do_estoque` lê o `applied` da 0057. No ato normal, a origem tem default `ato` e a entrada vem do relógio do banco. A 0058 marca o histórico com `estoque` e o marco. O payload de estoque declara os dois valores e `_lote_de_estoque` confere. O comparador produz `(classe, campo, novo)` a partir da redução, da coluna, do marco e de `fonte.entrada_origem`. `entrada_em` serve à marcação histórica e à admissão do payload. DRY, vigia e `_conferir_estoque` consomem `div`. O DRY e a porta passam `div`, `ato`, `p` e `marco` ao veto, que devolve texto de recusa ou `''`. `app/chamados/migrations/0057_decisao_entrada_do_ato.py:15`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:9`, `app/chamados/models.py:4594`, `app/chamados/models.py:4620`, `app/chamados/models.py:4797`, `app/chamados/models.py:4828`, `app/chamados/models.py:4889`, `app/chamados/services/vigia_ato_x_coluna.py:18`, `app/chamados/management/commands/estoque_ato_x_coluna.py:34`. |
| **Identidade, projeção elegível e K6** | `_nascer` grava a FK da disputa, `conversa` e o dia pelo ponto único `gravar_dia_do_fato`. O dono vem de `DisputaSupervisao.colaborador` e a empresa do cadastro do colaborador. `_conferir_estoque` relê essas relações e compara pergunta/PK, tipo de ato, instante, texto, autor, marca de manutenção e motivo do payload com a foto de `linhas_da_projecao_sem_linha`. Depois aplica o veto e lê K6 por `data_evento` e empresa. A janela fornece mês/ano. `ExportacaoDominio.registrar` e `invalidar` escrevem o estado que `competencia_entregue` lê. O DRY lê a mesma composição. `app/chamados/models.py:1901`, `app/chamados/models.py:3092`, `app/chamados/models.py:3104`, `app/chamados/models.py:3322`, `app/chamados/models.py:4821`, `app/chamados/models.py:4864`, `app/chamados/models.py:4875`, `app/chamados/models.py:4892`, `app/chamados/services/fio.py:950`, `app/colaboradores/views.py:1335`, `app/chamados/services/materializacao.py:803`, `app/chamados/services/passivo_validado.py:91`, `app/folha/models.py:143`, `app/folha/models.py:174`, `app/folha/export.py:647`. |
| **Registros de trava e `antes_de`** | `travar_entrega` adquire a linha da Empresa e registra `_TravaDaEntrega.empresa_id`. `trava_na_ordem` adquire o dono e `_registrar_travados` registra `_TravaDoColaborador.cid` e `aquisicao`. Os leitores do dono são `colaboradores_travados`, `estampa_das_travas` e o predicado local `do_dono`. `registrar` exige a presença dos donos antes de `_conferir_estoque`. `exigir_entrega_travada` exige todas as Empresas do lote antes do **primeiro** sentinela de um dono do lote. Há um único chamador direto na cópia. `travar_entrega` também é chamado pelo fluxo de exportação, pela regeração do TXT e pela revisão de par relâmpago. Esses caminhos continuam adquirindo a trava. O argumento opcional só ativa a ordem para quem o fornece. `app/chamados/models.py:2968`, `app/chamados/models.py:4773`, `app/chamados/models.py:4867`, `app/chamados/services/trava_do_colaborador.py:26`, `app/chamados/services/trava_do_colaborador.py:39`, `app/chamados/services/trava_do_colaborador.py:58`, `app/folha/export.py:573`, `app/folha/export.py:588`, `app/folha/export.py:602`, `app/folha/views.py:134`, `app/folha/management/commands/regerar_txt_dominio.py:67`, `app/ponto/services/par_relampago_na_lista.py:183`. |

### 3. Recusas novas de P1

- **Condição.** A recusa nova alcança o campo em que a redução real ainda dá T1 e a coluna traz T2 posterior sem linha correspondente, quando a síntese antiga fazia a leitura terminar em T2. O carimbo precisa chegar à guarda depois do C6 e da checagem de presença. Legado sem instante vigente no ato e pares coincidentes continuam passando pela face de instante. Classe `a` por ausência ou limpeza pode continuar aparecendo só no comparador. `app/chamados/models.py:4509`, `app/chamados/models.py:4563`, `app/chamados/models.py:4628`, `app/chamados/models.py:4688`, `app/chamados/models.py:3495`, `app/chamados/tests/test_e4c_f5_c4_red.py:147`.
- **Produção.** A irmã na absorção pode ser recusada pelo `carimbar` ou pela reconferência do registro, que relê e levanta `_Reprovada` antes de gravar a `ABSORCAO`. O plantio unitário recebe `RECUSA_PAR_ATO_COLUNA` e desfaz a batida tentada. `materializar_pendentes_aprovadas` deixa de carimbar e pode emitir a trilha de recusa do fecho. Também herdam a guarda a resolução de perguntas por chamado, o fecho sem credor, o reconciliador de turno/célula e o encerramento de fantasma, quando seus filtros deixam a pergunta chegar ao `carimbar`. `carimbar_identidade` tem porta própria. `app/chamados/services/materializacao.py:1123`, `app/chamados/services/materializacao.py:1132`, `app/chamados/services/materializacao.py:1155`, `app/chamados/services/materializacao.py:2113`, `app/chamados/services/materializacao.py:2375`, `app/chamados/services/disputa_emissao.py:2118`, `app/chamados/services/disputa_emissao.py:2192`, `app/chamados/reconciliador.py:408`, `app/chamados/reconciliador.py:699`, `app/chamados/reconciliador.py:1290`, `app/chamados/models.py:3692`, `app/chamados/models.py:3541`.
- **Alta, risco de bloquear conservação legítima de legado.** Existe um caso estático em que a aceitação anterior podia terminar com o par correto: resposta real em T1, resposta legítima do acervo na coluna em T2 > T1, validação vazia e `carimbar(resposta='validar')` depois de T2. A escrita antiga podia registrar a resposta de T2 em `_sem_linha` junto da nova validação. A guarda nova vê T1 e barra antes desse `registrar`. O teste de legado puro cobre zero linhas reais, e o comparador classifica esse caso misto como `a`. Se T2 era legítimo e quantos casos assim existem em produção, **[nao verificado]**. `app/chamados/models.py:3477`, `app/chamados/models.py:3502`, `app/chamados/models.py:3522`, `app/chamados/models.py:4487`, `app/chamados/models.py:4628`, `app/chamados/tests/test_e4c_f5_c4_red.py:147`.

### 4. Fixtures e efeitos colaterais

- **Conversa · média.** O RED conserva a mordida de conteúdo. Ainda exige `ValueError` com PK, ato e motivo `conversa`, e exige pergunta e sequência intactas. M09 ainda simula a aceitação da conversa com dia civil. A Empresa antecipada retira desse teste a cronologia conversa → lote no mesmo `atomic`: a abertura registra o dono, e um `atomic` interno bem-sucedido mantém o sentinela até o fim do externo. Se a Empresa só for tomada depois, a porta de estoque recusa pela ordem antes de conferir a conversa. O RED de P5 cobre essa inversão diretamente. A separação em duas transações é possível pelo código. A frequência ou existência da composição no mesmo `atomic` em produção é **[nao verificado]**. `app/chamados/tests/test_e4c_f5_c4_red.py:396`, `app/chamados/tests/test_e4c_f5_c4_red.py:464`, `app/chamados/tests/test_e4c_f5_c4_red.py:919`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:177`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:448`, `app/chamados/models.py:3122`, `app/chamados/models.py:3153`, `app/chamados/services/trava_do_colaborador.py:16`.
- **`pk_misto` · média.** Mudaram duas expectativas da lista. Elas agora reprovam a omissão do `b` indeterminado e continuam exigindo a saída exata. Comparador, vigia e quadro conservam suas asserções de precedência. M13 ainda ataca o quadro e M39 ataca a lista. A troca corrige uma expectativa que reproduzia o defeito e altera uma asserção existente, vedação literal do pedido. `app/chamados/tests/test_e4c_f5_c4_red.py:705`, `app/chamados/tests/test_e4c_f5_c4_red.py:756`, `app/chamados/tests/test_e4c_f5_c4_red.py:782`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:468`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:718`.
- **Import privado · baixa.** O import é local ao caminho de estoque, e o módulo da trava declara que não importa `chamados.models`. Há acoplamento novo ao tipo privado, a `.cid` e à representação da fila `run_on_commit`. A leitura da ordem continua baseada no registro existente. Para lote multiempresa, a implementação requer **todas** as Empresas antes do primeiro dono do lote. O RED novo cobre uma Empresa e um dono. `app/chamados/models.py:4867`, `app/chamados/services/trava_do_colaborador.py:1`, `app/chamados/services/trava_do_colaborador.py:26`, `app/folha/export.py:613`, `app/chamados/tests/test_e4c_f5_c4_red.py:919`.
- **Comando fora da lista · baixa.** A mudança no DRY é necessária para P2 e altera a lista impressa. Ela pode ter mais ou menos PKs que o balde homônimo do quadro. Na fixture mista, o quadro espera um e a lista espera dois. O quadro ainda pode classificar conversa pelo dia civil, pois `competencia_do_fato` lê `data_evento`. A exclusão ocorre na lista. Reprojeção, lista de entregues e caráter de somente leitura são preservados pelo fluxo do comando. Consumidores externos do texto **[nao verificado]**. `app/chamados/management/commands/estoque_ato_x_coluna.py:32`, `app/chamados/management/commands/estoque_ato_x_coluna.py:55`, `app/chamados/management/commands/estoque_ato_x_coluna.py:60`, `app/chamados/services/materializacao.py:811`, `app/chamados/tests/test_e4c_f5_c4_red.py:720`, `app/config/crons.py:802`.

### 5. Classes que já reprovaram obra

| Classe | Presença no delta ou no código herdado |
|---|---|
| Segundo juiz registrado | **Ausente no delta.** `veto_do_estoque` extrai a regra de elegibilidade por campo. DRY e porta delegam o juízo de entrega a K6. A guarda de instante compara integridade do par. C21 e K6 continuam nas autoridades registradas. `app/chamados/models.py:4688`, `app/chamados/models.py:4828`, `app/chamados/services/materializacao.py:803`, `app/core/juizes.py:313`, `app/core/juizes.py:350`. |
| Segundo escritor fora da porta | **Ausente para decisões e travas. Presente a emissão dupla herdada do item 6.** Resposta salva a pergunta, o `post_save` acorda a célula e `registrar` também a acorda. Ambos chegam ao mesmo escritor de `EventoCelula`. `app/chamados/services/disputa_emissao.py:1562`, `app/chamados/signals.py:253`, `app/chamados/models.py:3628`, `app/chamados/models.py:4778`, `app/chamados/models.py:4791`, `app/chamados/tests/test_e4c_f5_c4_red.py:634`. |
| Relógio fora da borda | **Presente, herdado.** `carimbar` resolve `quando` e chama `pergunta_viva(self)` sem repassá-lo. O degrau da janela pode cair em `timezone.localdate()`. `app/chamados/models.py:3460`, `app/chamados/models.py:3496`, `app/chamados/juizes.py:642`, `app/chamados/juizes.py:1568`. |
| Derivação paralela | **Corrigida no defeito P1 e no veto P2. Resta duplicação de montagem, baixa.** `leitura` e `_reais_reduzidos` codificam separadamente as tuplas das linhas reais e dos pendentes. Ambas usam `_reduzir`. A presença ainda usa deliberadamente a composta. `app/chamados/models.py:4557`, `app/chamados/models.py:4572`, `app/chamados/models.py:4642`, `app/chamados/models.py:4675`. |
| Selo/RED sem mordida | **Há limites de alcance.** M37 prova a guarda relida, P4 novo prova o persistido, e o Mock de P3 prova encaminhamento. Cada asserção tem alvo próprio. Nenhuma dessas três provas, isoladamente, demonstra rollback da absorção no cenário novo, chegada de pendente real em memória ou idempotência da trilha. `app/chamados/tests/test_e4c_f5_c4_red.py:226`, `app/chamados/tests/test_e4c_f5_c4_red.py:260`, `app/chamados/tests/test_e4c_f5_c4_red.py:336`, `app/chamados/tests/test_e4c_f5_c4_red.py:484`, `app/chamados/tests/test_bug_par_sem_banco.py:173`. |
| Ordem de trava | **Corrigida na admissão P5.** A inversão pelo chamador ainda pode ocorrer antes de chegar à conferência. `app/chamados/models.py:4773`, `app/chamados/models.py:4874`, `app/folha/export.py:613`, `app/chamados/tests/test_e4c_f5_c4_red.py:924`. |
| Pouso antes do pré-requisito | **Ausente na ancestralidade conferida.** O pouso de PORTA-CONFERE-TRAVA `eb76e7be7` é ancestral da base `59600163c`. A 0058 declara dependência da 0057. Ordem efetiva em produção **[nao verificado]**. `app/docs/BACKLOG.md:2179`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:25`. |
| Campo com dois significados | **Presente, herdado.** `data_evento` é dia do turno no fato e dia civil na conversa. `entrada_em` é instante real de inserção para `ato` e marco convencional para `estoque`. `app/chamados/models.py:3086`, `app/chamados/models.py:3097`, `app/chamados/models.py:4402`. |
| `except` novo | **Ausente.** A contagem de linhas adicionadas que casam com `^\+.*except` no delta é **0**. Os `raise` adicionados na conferência e no uso do veto estão em `app/folha/export.py:619` e `app/chamados/models.py:4891`. |

### 6. P3 e o conflito de desenho

**Média. O conflito existe com a montagem atual do teste antigo.** Ele usa as mesmas duas metades, o mesmo ator e a mesma entrega. Antes de **cada** porta, tira a foto dos PKs de trilha existentes. Depois exige, para cada metade, um evento com PK novo, fotos completas e motivo de entrega. Assim, uma deduplicação global do mesmo veto faria a segunda passagem

[truncado em 20000 de 21755 caracteres]

## 27/11 10:41 · sessao ea730b0547a5 · PAR-PELO-TURNO · leitura · origem=Code

**18 dos 19 blocos são da obra.** O smoke de pauta é independente na trilha de código que produz `SEM-LINHA []`. A causa específica no Chromium e a repetição desse resultado na base ficam [nao verificado]. As três combinações de turno textual contam como três blocos do mesmo método. [pautas/tests/test_smoke_clique_pauta_lida.py:78](/home/ronald/wt-codex-par-pelo-turno-010806/app/pautas/tests/test_smoke_clique_pauta_lida.py:78) · [chamados/tests/test_e3_c8b_sem_banco.py:438](/home/ronald/wt-codex-par-pelo-turno-010806/app/chamados/tests/test_e3_c8b_sem_banco.py:438)

### Catracas e Ruff

Comparei os censos AST de `59600163c` e `4ba908393` em memória. Os sete testes de `test_medida_turno_do_par` passaram com as fontes da base, reproduziram as falhas com as fontes da cópia e passaram com os registros atualizados em memória. A reexecução da suíte integrada na base fica [nao verificado]. [core/medida_turno_do_par.py:336](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/medida_turno_do_par.py:336) · [core/tests/test_medida_turno_do_par.py:504](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:504)

- **Números e identidades.** Pareadores `9 → 9`, derivações de dia `81 → 79`, A1 `6 → 6`, B1 `78 → 76`. Saem de B1 `autoridade_do_periodo`, `somar_periodos` e `_coletar_dados_espelho`. Entra `_turno_noturno_completo`, agora com `data_local(t.saida.timestamp)` no limite do recorte. O teste cobra igualdade dos números e das identidades. A cura é registrar `79`, `B1=76` e as quatro mudanças de identidade no mesmo commit. `core/tests/test_medida_turno_do_par.py:108`, `:109`, `:159`, `:508`, `:548`, `:556` · `ponto/services/espelho.py:84`, `:1035` · `relatorios/pdf_espelho.py:291` · `escala/servico_jornada.py:250`.
- **Mutantes e C8c.** O candidato amplo e a chamada morta medem `79` contra `81` e param em `DESCEU`. O mutante de dia acrescenta `1`, chega a `80` e também recebe `DESCEU`, quando o teste espera `SUBIU`. Com o registro `79`, o mutante volta a testar a subida. C8c chama a mesma `_catraca` e para em `79` contra `81`. A asserção seguinte, sobre a ausência de derivação no construtor de `CargaDaVida`, é satisfeita pelos dois censos. `core/tests/test_medida_turno_do_par.py:513`, `:580`, `:718`, `:845`, `:850` · `chamados/tests/test_e3_c8c_sem_banco.py:155`, `:160`, `:161`.
- **Composição.** A lista congelada tem `47` funções e a cópia mede `55`. Sai `_declara_separado_pelo_teto`. Entram `_chave_batida`, `_c9_do_par`, `_marca_intra_duracao.tipo_de_borda`, `_carregar_turnos_do_colab`, `marco_no_dia`, `marco_de_saida`, `minutos_ate_o_marco`, `localizar` e `MotorBase._cadastro_nao_descreve`. O censo segue as chamadas alcançadas a partir do juiz, inclusive as novas dependências da C9. A cura é registrar a composição no mesmo commit. `core/tests/test_medida_turno_do_par.py:110`, `:517` · `ponto/turnos.py:166`, `:171`, `:399`, `:1887` · `ponto/janela_he.py:20`, `:41`, `:64` · `ponto/motor_calculo_v2.py:73`, `:2046`.
- **Publicação `CLASSIFICAR`.** A2−A1 fica em `9`. B2−B1 sobe de `113` para `114` com `_coletar_dados_espelho`, que perde a ocorrência B1 no carregamento dos feriados e conserva o `.date()` do teto em ontem. A cura é registrar essa entrada no mesmo commit. `core/tests/test_medida_turno_do_par.py:351`, `:569` · `relatorios/pdf_espelho.py:278`, `:291`.
- **Ruff.** O teste exige código de saída `0`. A pista devolveu `1` e apontou `B023` em `ponto/tests/test_par_pelo_turno.py:212:63`. O hunk adiciona esse arquivo, incluindo `lambda: dna` que captura a variável reatribuída no laço. A cura é de código no teste, com `lambda dna=dna: dna`. Esse B023 está ausente da base porque o arquivo é novo. O resultado completo do Ruff na base fica [nao verificado]. [core/tests/test_contract_esmeril.py:22](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_contract_esmeril.py:22) · [ponto/tests/test_par_pelo_turno.py:212](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/tests/test_par_pelo_turno.py:212)

O delta completo de `LINHAS_REGISTRADAS` abaixo mostra **esperado → medido**. São linhas da ocorrência que o censo conta, inclusive quando a mudança só desloca o código. A cura é registrar esses vetores no mesmo commit. [core/tests/test_medida_turno_do_par.py:251](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:251) · [core/tests/test_medida_turno_do_par.py:563](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:563) [nao verificado]

| Ocorrência | Esperado → medido |
|---|---|
| `ponto/turnos.py::_pares_marcados` | `[812] → [829]` |
| `ponto/turnos.py::_dia_das_batidas_puro` | `[3044] → [3087]` |
| `ponto/turnos.py::dia_do_turno_da_batida` | `[3021] → [3058]` |
| `ponto/turnos.py::par_completo` | `[2237] → [2267]` |
| `ponto/services/fechamento.py::_so_a_data` | `[1593] → [1588]` |
| `ponto/services/fechamento.py::apurar_por_fatia` | `[224] → [246]` |
| `ponto/services/fechamento.py::recalcular_fechamento_mes` | `[675, 677, 683, 703, 774, 813, 859, 871] → [673, 675, 685, 705, 776, 815, 854, 866]` |
| `ponto/services/espelho.py::autoridade_do_periodo` | `[1065] → ∅` no hunk `:1035` |
| `ponto/services/espelho.py::somar_periodos` | `[84] → ∅` no hunk `:84` |
| `relatorios/pdf_espelho.py::_coletar_dados_espelho` | `[298] → ∅` no hunk `:291` |
| `escala/servico_jornada.py::_turno_noturno_completo` | `∅ → [250]` |

### Comportamento e smoke

- **C8b / CargaBasicaLeitura.** O traceback termina em `AttributeError: 'types.SimpleNamespace' object has no attribute '_turnos_com_contexto'`. O teste extrai o método real e espera que duas chamadas leiam as batidas uma vez e usem o cache sem parear. Na base, `turnos` fazia a carga e devolvia a tupla do dublê. Na cópia, o hunk extrai `_turnos_com_contexto` e `turnos` filtra a resposta por `t.data_turno`. O replay passou na base, falhou na cópia e passou com o helper real ligado ao dublê e turnos datados. `chamados/tests/test_e3_c8b_sem_banco.py:537`, `:544`, `:546` · `ponto/turnos.py:3112` (base), `:3155`, `:3175` (cópia).
- **C8b / AlimentacaoC6, ausência e memo.** Ambos dão `False is not true` depois de remover a ausência da carga. O contrato é reabrir a pergunta nesse braço, preservando o memo avulso. O teste lê `janela_com_pad` por AST e injeta só `datetime`. O hunk passa a ler `_PAD_FECHO`. A carga estoura no dublê, e `classificar_falta` registra `nucleo_indisponivel_na_lei` e volta sem acusação. O replay passou na base e passou na cópia ao alimentar a constante real no ambiente AST. `chamados/tests/test_e3_c8b_sem_banco.py:312`, `:325`, `:326` · `chamados/tests/e3_c8b_apoio.py:139` · `chamados/services/carga_pergunta_viva.py:174` · `escala/servico_jornada.py:396`, `:400` · `ponto/turnos.py:54`, `:1869`.
- **C8b / AlimentacaoC6, três braços textuais.** O contrato espera `False` com turno fechado ou trilha de retratação e `True` com turno aberto sem trilha, sempre com `env.erros == []`. O mesmo hunk de `_PAD_FECHO` quebra a consulta de turno. O juiz registra `juiz_indisponivel` e devolve `False`. Nos dois primeiros braços o traceback mostra `NameError("name '_PAD_FECHO' is not defined")` em `env.erros`. No terceiro mostra `False != True`. Os três passaram no replay da base e com a alimentação da constante na cópia. `chamados/tests/test_e3_c8b_sem_banco.py:438`, `:447`, `:449` · `chamados/juizes.py:448`, `:865`, `:1586`, `:1593` · `chamados/services/carga_pergunta_viva.py:174` · `ponto/turnos.py:1869`.
- **Espelho do app.** O traceback dá `False is not true` na premissa «a fixture nao reproduz o desacordo», antes de testar o card. Reproduzi em memória o resultado do motor, o mapa por PK e o helper do teste com as três batidas. O motor conserva o período aberto de `06:57` em `31/08` e o fechado de `12:50–21:20` em `01/09` nas duas versões. Na base, o mapa consultado para setembro fica sem o turno de `31/08` e usa a data civil de `06:57`, jogando o aberto em `01/09` no helper. Na cópia, o julgamento com contexto recupera `31/08` para esse PK, e o helper retorna `False` em `01/09`. O contrato a preservar é o card ler a geometria. A cura é construir o desacordo do teste de forma explícita, com um resultado de motor divergente e uma geometria fechada para o mesmo dia. A execução integrada do endpoint na base fica [nao verificado]. `api/tests/test_espelho_app_fonte_e_geometria.py:295`, `:301`, `:305`, `:318`, `:325` · `ponto/turnos.py:2996`, `:3044` (base), `:2000`, `:3035`, `:3082`, `:3087` (cópia) · `api/views.py:1447`, `:1512`.
- **Espelho da tela.** O traceback diz `False is not true: somar_periodos deixou de recortar por data`. O teste inspeciona o AST e espera uma chamada `.date()`. A base tem essa chamada na linha `84`. A cópia tem zero e conserva o recorte, comparando `dentro` com o dia publicado em `dia_da_jornada[id(p)]`. A cura é atualizar a asserção estrutural para reconhecer o recorte pelo dia publicado e preservar o teste que proíbe derivação em `montar_dias`. `ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:193`, `:245`, `:248` · `ponto/services/espelho.py:84`, `:451`.
- **Ponta pequena.** O traceback dá `0 != 1` com `{}` às `03:10`. A fixture grava as três batidas sob o mesmo `freeze_time` de `20:05`. Isso faz a entrada de `20:04` parecer um eco de flush da entrada de `14:57`. No replay do pareador, a base ainda usa a entrada das `20:04` para inferir a pausa após a saída das `19:01` e deixa o turno aberto. A cópia exclui o eco da inferência e fecha `14:57–19:01`. Na consulta retroativa das `02:10`, o aberto da base ainda manteria a célula de ontem no vigia e expiraria às `03:00`. Com o par fechado, a cópia já pode descartar a célula antes de contar ou emitir. Gravar cada batida da fixture no próprio instante conservou o aberto nos dois pareadores. A ata efetivamente gravada e a execução integrada com ORM na base ficam [nao verificado]. `ponto/tests/test_ponta_pequena_prazo_evento.py:84`, `:388`, `:392`, `:400` · `ponto/models.py:79` · `ponto/turnos.py:369`, `:399`, `:404`, `:469`, `:1234`, `:1420`, `:2073` · `ponto/services/vigia_de_hora.py:59`, `:66`, `:169` · `ponto/services/cartorio.py:881`.
- **Smoke de pauta.** O traceback para em `'SEM-LINHA []' is not an instance of <class 'dict'>`. A sonda chegou à raiz e à subaba e encontrou zero rótulos aos `700 ms`, antes de clicar no texto e verificar ausência de POST. O caminho usa a casca do holerite, os GETs de pauta e o `fetch` injetado no Chromium. A âncora `2026-09` é de competência e a listagem filtra destinatário e estado, sem recorte pela data da competência. A trilha da linha de pauta e do clique está fora dos hunks. O JSON efetivo da supervisão, o motivo da ausência dos rótulos e a recorrência na base ficam [nao verificado]. `pautas/tests/test_smoke_clique_pauta_lida.py:54`, `:78`, `:90`, `:94`, `:144`, `:153`, `:169`, `:174`, `:211` · `pautas/views.py:29`, `:61` · `pautas/services.py:168`, `:304`, `:322` · `templates/core/_copiloto.html:359`, `:400`, `:475`.

### Tabela final

| Teste | Da obra? | Classe | Sítio | Cura proposta | [nao verificado]
|---|---|---|---|---|
| `test_carregar_batidas_uma_vez_sem_executar_pareador` · `chamados/tests/test_e3_c8b_sem_banco.py:544` | sim | comportamento | `ponto/turnos.py:3155`, `:3175` | Ligar o helper real ao dublê com janela e turnos datados, preservando uma leitura e zero pareamentos. |
| `test_MORDE_o_card_diz_o_que_a_GEOMETRIA_diz_e_nao_o_que_o_motor_diz` · `api/tests/test_espelho_app_fonte_e_geometria.py:318` | sim | comportamento | `ponto/turnos.py:2000`, `:3035`, `:3082` | Tornar explícito o desacordo entre o motor e a geometria no mesmo dia da fixture. |
| `test_MORDE_clicar_o_texto_e_nao_o_rotulo_nao_posta` · `pautas/tests/test_smoke_clique_pauta_lida.py:210` | não pela trilha estática. Repetição na base [nao verificado] | relógio/ambiente | `pautas/tests/test_smoke_clique_pauta_lida.py:90`, `:94`, `:174` | Afirmar A no JSON de supervisão e sincronizar a sonda com a renderização da subaba. |
| `test_RED_a_celula_de_ONTEM_fica_no_vigia_ate_o_aberto_expirar_e_o_evento_sai` · `ponto/tests/test_ponta_pequena_prazo_evento.py:400` | sim | comportamento | `ponto/turnos.py:399`, `:404`, `:469`, `:1234` | Gravar cada batida no próprio instante e manter os horários das passadas do vigia. |
| `test_ruff_zero` · `core/tests/test_contract_esmeril.py:24` | sim | ruff | `ponto/tests/test_par_pelo_turno.py:212` | Vincular `dna` na definição com `lambda dna=dna: dna`. |
| `test_CATRACA_composicao_congelada_exige_registro_no_mesmo_commit` · `core/tests/test_medida_turno_do_par.py:517` | sim | catraca-registro | `ponto/turnos.py:166`, `:171`, `:399`, `:1887` | Registrar a composição de `55` funções no mesmo commit. |
| `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` · `core/tests/test_medida_turno_do_par.py:563` | sim | catraca-registro | `ponto/turnos.py:829` e demais ocorrências da tabela de linhas | Atualizar `LINHAS_REGISTRADAS` no mesmo commit. |
| `test_CATRACA_numeros_exatos_e_registro_no_mesmo_commit` · `core/tests/test_medida_turno_do_par.py:547` | sim | catraca-registro | `ponto/services/espelho.py:84`, `:1035` · `relatorios/pdf_espelho.py:291` · `escala/servico_jornada.py:250` | Registrar `79`, B1 `76` e as identidades revistas no mesmo commit. |
| `test_MORDE_candidato_amplo_novo_e_publicado_sem_travar_a_catraca` · `core/tests/test_medida_turno_do_par.py:718` | sim | catraca-registro | `core/tests/test_medida_turno_do_par.py:108`, `:513` | Registrar `79` no mesmo commit para o mutante voltar a provar a publicação. |
| `test_MORDE_chamada_morta_no_juiz_exige_registro_da_composicao` · `core/tests/test_medida_turno_do_par.py:580` | sim | catraca-registro | `core/tests/test_medida_turno_do_par.py:108`, `:513`, `:583` | Atualizar números e composição no mesmo commit para o mutante chegar à asserção de composição. |
| `test_MORDE_dia_novo_sobe_contador_2_e_catraca_fica_RED` · `core/tests/test_medida_turno_do_par.py:850` | sim | catraca-registro | `core/tests/test_medida_turno_do_par.py:108`, `:847`, `:850` | Registrar `79` no mesmo commit para `80` produzir `SUBIU`. |
| `test_a_classificar_publicado_sem_entrar_como_criterio_amplo` · `core/tests/test_medida_turno_do_par.py:569` | sim | catraca-registro | `relatorios/pdf_espelho.py:278`, `:291` | Incluir `_coletar_dados_espelho` em `B2_menos_B1` no mesmo commit. |
| `test_MORDE_o_selo_distingue_JANELA_de_DIA` · `ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:248` | sim | comportamento | `ponto/services/espelho.py:84`, `:451` | Atualizar o selo para verificar recorte pelo dia publicado. |
| `test_ausencia_alimentada_nao_pode_sumir_ou_ser_de_outro_colab` · `chamados/tests/test_e3_c8b_sem_banco.py:312` | sim | comportamento | `ponto/turnos.py:1869` · `chamados/tests/e3_c8b_apoio.py:139` | Alimentar `_PAD_FECHO` real no dublê AST. |
| `test_memo_avulso_nao_contamina_nem_e_contaminado_pela_carga` · `chamados/tests/test_e3_c8b_sem_banco.py:325` | sim | comportamento | `ponto/turnos.py:1869` · `chamados/tests/e3_c8b_apoio.py:139` | Alimentar `_PAD_FECHO` real e preservar a asserção sobre o memo avulso. |
| `test_turno_textual_fechado_e_trilha_com_e_sem_carga` · `fechado=True, auditada=False` · `chamados/tests/test_e3_c8b_sem_banco.py:449` | sim | comportamento | `ponto/turnos.py:1869` · `chamados/juizes.py:1593` | Alimentar `_PAD_FECHO` real para o turno fechado resolver sem erro. |
| `test_turno_textual_fechado_e_trilha_com_e_sem_carga` · `fechado=False, auditada=True` · `chamados/tests/test_e3_c8b_sem_banco.py:449` | sim | comportamento | `ponto/turnos.py:1869` · `chamados/juizes.py:865` | Alimentar `_PAD_FECHO` real para o juiz chegar à leitura da trilha. |
| `test_turno_textual_fechado_e_trilha_com_e_sem_carga` · `fechado=False, auditada=False` · `chamados/tests/test_e3_c8b_sem_banco.py:447` | sim | comportamento | `ponto/turnos.py:1869` · `chamados/juizes.py:1593` | Alimentar `_PAD_FECHO` real para a pergunta aberta sem trilha voltar a `True`. |
| `test_selo_turno_sem_derivacao_nova` · `chamados/tests/test_e3_c8c_sem_banco.py:160` | sim | catraca-registro | `core/tests/test_medida_turno_do_par.py:108`, `:513` | Atualizar o registro compartilhado para `79` no mesmo commit. |

## 27/11 10:38 · sessao 93cc64114c4e · - · - · origem=Ronald (tmux)

Os seis blocos de C8b são da obra. A reprodução em memória repetiu os cinco `FAIL` e um `ERROR` na cópia. Sob o mesmo dublê da estampa de trava, trocar os trechos extraídos pelos da base `59600163c` fez os quatro métodos passarem. `chamados/tests/e4_apoio.py:8`, `chamados/tests/e4_catracas.py:8`, `chamados/juizes.py:1168`

- **Ausência** (`chamados/tests/test_e3_c8b_sem_banco.py:312`) e **memo** (`chamados/tests/test_e3_c8b_sem_banco.py:325`) esperam que a pergunta fique viva ao remover a ausência da carga. Recebem `False is not true`. A lei chama `carga.turnos`, que avalia o `janela_com_pad` real extraído sem sua nova constante. Ela registra `nucleo_indisponivel_na_lei` e deixa de acusar a falta. `escala/servico_jornada.py:393`, `escala/servico_jornada.py:400`, `chamados/services/carga_pergunta_viva.py:174`, `ponto/turnos.py:1869`
- **Texto, fechado sem trilha** e **texto, aberto com trilha** chegam ao `False` esperado, mas registram `juiz_indisponivel`, com `NameError("name '_PAD_FECHO' is not defined")`, violando `env.erros == []`. **Texto, aberto sem trilha** recebe `False != True` pelo mesmo retorno conservador. O caminho da exceção passa pelo leitor da completude textual e pela carga até a mesma constante. `chamados/tests/test_e3_c8b_sem_banco.py:447`, `chamados/tests/test_e3_c8b_sem_banco.py:449`, `chamados/juizes.py:448`, `chamados/juizes.py:865`, `chamados/juizes.py:1587`, `chamados/juizes.py:1593`
- **Cura dos cinco blocos de AlimentacaoC6.** Levar a constante real ao ambiente que compila `janela_com_pad` por AST. O dublê injeta apenas `datetime` e o código novo lê `_PAD_FECHO`. A injeção da constante derivada da fonte fez os cinco blocos passarem em memória. `chamados/tests/e3_c8b_apoio.py:139`, `ponto/turnos.py:54`, `ponto/turnos.py:56`, `ponto/turnos.py:1869`
- **CargaBasicaLeitura.** O dublê tinha uma tupla com marcador textual e extraía `turnos` isoladamente. Na base, o método carregava as batidas e devolvia o cache. A cópia passou a chamar `_turnos_com_contexto` e a filtrar por `data_turno`, gerando `AttributeError` no dublê. Ligar o helper real e fornecer `ini`, `fim` e turnos datados preservou no replay a asserção de uma única leitura e zero pareamento. `chamados/tests/test_e3_c8b_sem_banco.py:537`, `chamados/tests/test_e3_c8b_sem_banco.py:544`, `chamados/tests/test_e3_c8b_sem_banco.py:546`, `ponto/turnos.py:3155`, `ponto/turnos.py:3175`

## 27/11 10:38 · sessao 19f2fc3b4622 · - · - · origem=Ronald (tmux)

Os sete testes de `CatracaTurnoDoParTest` passaram no replay em memória da base `59600163c` e reproduziram as falhas da pista na cópia. O selo C8c mede os mesmos números e mantém ausente o construtor de `CargaDaVida` nos dois censos. [app/core/tests/test_medida_turno_do_par.py:504](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:504) [app/chamados/tests/test_e3_c8c_sem_banco.py:151](/home/ronald/wt-codex-par-pelo-turno-010806/app/chamados/tests/test_e3_c8c_sem_banco.py:151)

- **Números**. Pareadores `9→9`, derivações `81→79`, A1 `6→6`, B1 `78→76`. B1 perde duas funções do espelho e a coleta do PDF, e ganha `_turno_noturno_completo`. Registrar os números e as identidades no mesmo commit. O mutante amplo e a chamada morta medem `79`, e o mutante de dia mede `80`, abaixo do `81` congelado. [app/core/tests/test_medida_turno_do_par.py:108](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:108) [app/core/tests/test_medida_turno_do_par.py:546](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:546) [app/core/tests/test_medida_turno_do_par.py:577](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:577) [app/core/tests/test_medida_turno_do_par.py:714](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:714) [app/core/tests/test_medida_turno_do_par.py:845](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:845) [nao verificado]
- **Composição**. `47→55`, com nove inclusões alcançadas pelo fechamento estático e remoção de `_declara_separado_pelo_teto`. Registrar a composição no mesmo commit. [app/core/tests/test_medida_turno_do_par.py:110](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:110) [app/core/tests/test_medida_turno_do_par.py:517](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:517) [app/ponto/turnos.py:171](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/turnos.py:171) [nao verificado]
- **Linhas**. O primeiro delta é `_pares_marcados`, `[812]→[829]`. A lista completa foi entregue ao agente principal. Registrar ocorrências no mesmo commit. [app/core/tests/test_medida_turno_do_par.py:251](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:251) [app/core/tests/test_medida_turno_do_par.py:563](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:563) [app/ponto/turnos.py:829](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/turnos.py:829) [nao verificado]
- **CLASSIFICAR**. A2−A1 `9→9`, B2−B1 `113→114`. Só entra `_coletar_dados_espelho`, que perde a ocorrência B1 e conserva a data ampla. Registrar `CLASSIFICAR` no mesmo commit. [app/core/tests/test_medida_turno_do_par.py:351](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:351) [app/core/tests/test_medida_turno_do_par.py:569](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:569) [app/relatorios/pdf_espelho.py:278](/home/ronald/wt-codex-par-pelo-turno-010806/app/relatorios/pdf_espelho.py:278) [nao verificado]
- **Ruff**. A pista obteve retorno `1` frente ao `0` exigido. O único B023 apontado vem de `lambda: dna` no arquivo introduzido pela obra. Vincular `dna` na definição com `lambda dna=dna: dna`. Execução do Ruff na base [nao verificado]. [app/core/tests/test_contract_esmeril.py:24](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_contract_esmeril.py:24) [app/ponto/tests/test_par_pelo_turno.py:212](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/tests/test_par_pelo_turno.py:212)

## 27/11 10:37 · sessao 64cf12af4b89 · - · - · origem=Ronald (tmux)

- **Ponta pequena — sim, comportamento.** A fixture grava as três batidas às 20:05, por isso a entrada de 20:04 vira eco de flush. A base ainda usava essa entrada para inferir uma pausa das 19:01 às 20:04, deixando o turno aberto. A cópia exclui o eco dessa inferência e fecha o par às 19:01. Confirmei os dois resultados em memória. No vigia das 03:10, a consulta retroativa às 02:10 mantém a célula na base e a elimina na cópia, compatível com `0 != 1 ... {}`. A cura proposta é gravar cada batida da fixture no próprio instante e preservar o relógio das passadas. O teste completo com ORM segue [nao verificado]. `app/ponto/tests/test_ponta_pequena_prazo_evento.py:82,388,392,400` · `app/ponto/models.py:79` · `app/ponto/turnos.py:360,369,399,404,469,1234,1420,2053,2073` · `app/ponto/services/vigia_de_hora.py:59,66,169`.

- **Smoke pauta — independente pelo caminho lido, classe relógio/ambiente.** `SEM-LINHA []` indica que a sonda achou raiz e subaba e viu zero rótulos no prazo fixo de 700 ms. A falha ocorreu antes do clique e das asserções de POST e de leitura. O teste usa a casca do holerite e injeta no Chromium o JSON dos GETs. A lista de pautas vivas filtra por destinatário e estado. A data da âncora é um identificador nessa leitura. A cura proposta é afirmar a presença de A no JSON e aguardar a renderização da subaba antes do clique. O motivo preciso dos zero rótulos e a repetição da falha na base seguem [nao verificado]. `app/pautas/tests/test_smoke_clique_pauta_lida.py:54,78,90,144,153,169,174,210` · `app/pautas/services.py:304,322` · `app/pautas/models.py:57` · `app/templates/core/_copiloto.html:344,359,400,475`.

## 27/11 10:24 · sessao 1bca4d244636 · CASO-LITERAL-14 · leitura · origem=Code

```python
import json
import logging
import re
from datetime import datetime, timedelta
from difflib import unified_diff
from inspect import getclosurevars

from django.contrib.auth import get_user_model
from django.db import connection, transaction
from django.db.models import Q
from django.test import TestCase
from django.utils import timezone

from chamados.catalogo.motor import COBRANCA
from chamados.models import ChamadoColaborador, PerguntaDisputa
from colaboradores.mixins import tem_acao
from colaboradores.models import Empresa
from folha import export
from ponto.models import Batida, FechamentoMensal
from ponto.services import fechamento, retratar_humano
from ponto.services.par_relampago_na_lista import travar_batidas_do_sinal

# Executar no shell de `tenant_command shell --schema=juliani`.
# Escopo autorizado em aval.txt:1. Alvo em r_f1b.md:22 e r_f1b.md:41.
# HEAD Git, versão instalada, dados atuais de produção e execução [nao verificado].
MODO = "ensaio"
USUARIO_ID = None
MOTIVO_ID = None

ALVO = 159844
ANTERIOR = 159843
COLAB_ID = 1036
EMPRESA_ID = 2
COMPETENCIA = (11, 2026)
HORA_LOCAL = datetime(2026, 11, 19, 18, 4, 22)
OBSERVACAO = (
    "CASO-LITERAL-14-v2-2711-0955: toque excedente 1 s apos 159843 "
    "(classe b, fase 1)"
)

# Campos consumidos na montagem e classificação do TXT.
# app/folha/export.py:402, :699, :774, :977 e :1310
CAMPOS_TXT = (
    "colaborador_id", "mes", "ano", "status", "horas_trabalhadas",
    "turnos_abertos", "inconsistencias", "horas_extras_50",
    "horas_extras_100", "horas_extras_100_feriado", "horas_folga_trabalhada",
    "horas_noturnas", "horas_intra_indenizada", "horas_atraso",
    "horas_saida_antecipada",
)


def exigir(condicao, mensagem):
    if not condicao:
        raise ValueError(mensagem)


def imprimir_json(rotulo, dado):
    print(rotulo, json.dumps(dado, default=str, ensure_ascii=False, sort_keys=True))


def rotulo_publicavel(texto):
    # Os rótulos solicitados vêm de cadastros com texto livre.
    # app/ponto/models.py:641. Conteúdo desses cadastros [nao verificado].
    if re.search(r"@|\b(?:\d{11}|\d{3}\.\d{3}\.\d{3}-\d{2})\b", texto):
        return "[oculto]"
    return texto


def referencias_no_contexto(campo):
    # IDs em contexto, incluindo a forma textual do acervo.
    # app/chamados/models.py:1919, :3914
    # app/chamados/juizes.py:131
    # app/chamados/services/disputa_emissao.py:59
    # app/chamados/services/acoes_chamado.py:244
    ids = (ALVO, str(ALVO))
    return (
        Q(**{f"{campo}__batida_id__in": ids})
        | Q(**{f"{campo}__ultima_batida__id__in": ids})
        | Q(**{f"{campo}__entrada_sem_saida__entrada_id__in": ids})
        | Q(**{f"{campo}__ocorrencias__contains": [{"batida_id": ALVO}]})
        | Q(**{f"{campo}__ocorrencias__contains": [{"batida_id": str(ALVO)}]})
    )


def guardas(travar):
    # A ordem Empresa -> Colaborador -> Batidas vem da porta existente.
    # app/ponto/services/par_relampago_na_lista.py:172
    # app/folha/export.py:556 e app/folha/views.py:134
    if travar:
        exigir(Empresa.objects.filter(pk=EMPRESA_ID).exists(),
               "Empresa 2 ausente. app/colaboradores/models.py:40")
        marcas = travar_batidas_do_sinal(COLAB_ID, (ALVO, ANTERIOR), {EMPRESA_ID})
    else:
        marcas = Batida.objects.filter(pk__in=(ALVO, ANTERIOR))

    por_id = {marca.pk: marca for marca in marcas}
    batida = por_id.get(ALVO)
    exigir(batida is not None, "Batida 159844 ausente do escopo. app/ponto/models.py:8")
    exigir(batida.colaborador_id == COLAB_ID,
           "Colaborador divergente. app/ponto/models.py:22")
    exigir(batida.colaborador.empresa_id == EMPRESA_ID,
           "Empresa divergente. app/colaboradores/models.py:321")
    exigir(timezone.localtime(batida.timestamp).replace(tzinfo=None) == HORA_LOCAL,
           "Timestamp local divergente. app/ponto/models.py:23 e app/config/settings/base.py:93")
    exigir(batida.tipo == "S", "Tipo divergente. app/ponto/models.py:24")
    exigir(batida.retratada_em is None, "Batida já retratada. app/ponto/models.py:67")
    exigir(batida.aprovada_por_id is None, "Batida aprovada por admin. app/ponto/models.py:68")

    anterior = por_id.get(ANTERIOR)
    exigir(
        anterior is not None
        and anterior.colaborador_id == COLAB_ID
        and anterior.tipo == "E"
        and anterior.timestamp == batida.timestamp - timedelta(seconds=1)
        and anterior.retratada_em is None,
        "Batida anterior divergente. r_f1b.md:22 e app/ponto/models.py:22–24,67",
    )

    # .viva delega ao juiz da pergunta. A abertura da disputa usa fechada_em.
    # app/chamados/models.py:1997, :2751, :3792 e :3812
    # app/chamados/juizes.py:911 e app/ponto/models.py:40
    perguntas = PerguntaDisputa.objects.filter(disputa__fechada_em__isnull=True).filter(
        Q(batida_origem_id=ALVO)
        | Q(batidas_plantadas__pk=ALVO)
        | referencias_no_contexto("disputa__contexto")
        | referencias_no_contexto("disputa__chamado__contexto_json")
        | referencias_no_contexto("chamado_destino__contexto_json")
    ).distinct()
    exigir(not any(pergunta.viva for pergunta in perguntas),
           "Pergunta aberta ligada à batida. app/chamados/models.py:3792")

    # COBRANCA contém os estados aberto e em_analise.
    # app/chamados/catalogo/motor.py:29 e app/chamados/models.py:59, :1923, :3812
    chamados = ChamadoColaborador.objects.filter(status_local__in=COBRANCA).filter(
        referencias_no_contexto("contexto_json")
        | referencias_no_contexto("disputa_origem__contexto")
        | Q(disputa_origem__perguntas__batida_origem_id=ALVO)
        | Q(perguntas_que_resolvem__batida_origem_id=ALVO)
        | Q(disputa_origem__perguntas__batidas_plantadas__pk=ALVO)
        | Q(perguntas_que_resolvem__batidas_plantadas__pk=ALVO)
    )
    exigir(not chamados.exists(), "Chamado aberto ligado à batida. app/chamados/catalogo/motor.py:29")

    # competencia_do_fato usa a janela da empresa. O juiz de “exportada”
    # é competencia_entregue, também chamado por marcos_da_competencia.
    # app/ponto/services/fechamento.py:1626, :1561 e :36
    # app/ponto/services/dia_pago.py:702 e app/folha/export.py:565
    comp = fechamento.competencia_do_fato(batida.colaborador, batida.timestamp)
    exigir(comp == COMPETENCIA, "Competência divergente. app/ponto/services/fechamento.py:1626")
    exigir(not export.competencia_entregue(EMPRESA_ID, *comp),
           "Competência exportada. app/folha/export.py:565")
    exigir(comp not in fechamento.GUARDA_COMPETENCIA,
           "Competência consta na guarda. app/ponto/services/fechamento.py:36")
    return batida


def listar():
    # As duas consultas usam os critérios da porta.
    # app/ponto/services/retratar_humano.py:108, :118, :127 e :162
    # app/colaboradores/mixins.py:322 e app/config/settings/saas.py:34
    motivos = {motivo.pk: motivo for motivo in retratar_humano.motivos_ativos()}
    usuarios = {
        usuario.pk: usuario
        for usuario in get_user_model().objects.only(
            "id", "username", "is_active", "is_superuser"
        ).order_by("pk")
        if tem_acao(usuario, retratar_humano.ACAO_EXIGIDA)
    }
    imprimir_json("MOTIVOS_ATIVOS", [(pk, rotulo_publicavel(m.nome)) for pk, m in motivos.items()])
    imprimir_json("USUARIOS_REGULARIZAR_BATIDA",
                  [(pk, rotulo_publicavel(u.username)) for pk, u in usuarios.items()])
    return motivos, usuarios


def fotografia(rotulo, batida, apos_retratacao=None):
    # values() fotografa as colunas, com IDs nas FKs. Preserva as chaves dos
    # campos de texto livre e do caminho da foto, ocultando valores preenchidos.
    # app/ponto/models.py:22–95
    dados_batida = Batida.objects.filter(pk=batida.pk).values().first()
    exigir(dados_batida is not None, "Batida ausente na fotografia. app/ponto/models.py:8")
    for campo in ("terminal", "tipo_registro", "justificativa", "foto"):
        if dados_batida[campo]:
            dados_batida[campo] = "[oculto]"

    colab = batida.colaborador
    empresa = colab.empresa
    mes, ano = COMPETENCIA
    gravado = FechamentoMensal.objects.filter(
        colaborador_id=colab.pk, mes=mes, ano=ano,
    ).values("id", "derivado_em", *CAMPOS_TXT).first()
    if apos_retratacao is not None:
        exigir(dados_batida["retratada_em"] == apos_retratacao and not dados_batida["aprovada"],
               "Estado após a retratação divergente. app/ponto/registro_batida.py:294")
        # O callback contém falhas. O carimbo confirma a lavra do fechamento.
        # app/ponto/services/fechamento.py:1167, :1213 e :1842
        exigir(gravado and gravado["derivado_em"] and gravado["derivado_em"] >= apos_retratacao,
               "Lavra do fechamento sem confirmação. app/ponto/services/fechamento.py:1167,1213")

    # A view entrega o fechamento lido ao classificador e ao montador. O
    # recorte na entrada mantém os registros 11 junto às rubricas do colab.
    # app/folha/views.py:214, :219 e :275
    # app/ponto/services/fechamento.py:468 e app/folha/export.py:215, :699
    lido = fechamento.recalcular_fechamento_mes(
        mes, ano, empresa_id=empresa.pk, colaborador_ids=[colab.pk],
        somente_leitura=True, retornar_objeto=True,
    ).get(colab.pk)
    exigir(lido is not None, "Fechamento lido indisponível. app/ponto/services/fechamento.py:1333")
    itens = export.classificar_export(empresa, mes, ano, fechamentos=[lido])
    linhas, _ = export.montar_linhas_export(
        empresa, mes, ano, itens=itens, fechamentos=[lido],
    )
    # Validação e conteúdo da view. Hash sobre os bytes UTF-8 com CRLF do TXT.
    # app/folha/views.py:282 e app/folha/export.py:266, :525 e :542
    exigir(not export.validar_layout_dominio(linhas), "Layout inválido. app/folha/export.py:266")
    sha256 = export.hash_do_txt(export.conteudo_do_txt(linhas))

    # As rubricas e seus insumos vêm dos mesmos leitores da montagem.
    # app/folha/export.py:184, :341, :363, :402, :739 e :993
    ciclo = export._ciclo_do_colab(colab, mes, ano)
    dobra = export.feriado_12x36_em_dobra(colab, mes, ano)
    eventos = dict(export.eventos_do_fechamento(lido, tipo_ciclo=ciclo, feriado_12x36_em_dobra=dobra))
    rubricas = export._rubricas_map(empresa)
    insumos_rubricas = {}
    for evento in (*eventos, "falta_diurna"):
        rubrica = rubricas.get(evento)
        insumos_rubricas[evento] = (
            None if rubrica is None else {
                "id": rubrica.pk,
                "empresa_id": rubrica.empresa_id,
                "codigo_fopag": rotulo_publicavel(rubrica.codigo_fopag),
                "tipo": rotulo_publicavel(rubrica.tipo),
            }
        )

    imprimir_json(f"{rotulo} BATIDA", dados_batida)
    imprimir_json(f"{rotulo} FECHAMENTO_GRAVADO", gravado)
    imprimir_json(f"{rotulo} FECHAMENTO_LIDO", {campo: getattr(lido, campo) for campo in CAMPOS_TXT})
    imprimir_json(f"{rotulo} CLASSIFICACAO", [
        {campo: item[campo] for campo in ("colaborador_id", "status", "motivo", "travas_aprovacao")}
        for item in itens
    ])
    imprimir_json(f"{rotulo} RUBRICAS", {
        "tipo_ciclo": ciclo,
        "feriado_12x36_em_dobra": dobra,
        "eventos": eventos,
        "datas_falta": export.datas_falta(colab, empresa, mes, ano),
        "mapeamento": insumos_rubricas,
    })
    print(f"{rotulo} TXT linhas={len(linhas)} sha256={sha256}")
    for linha in linhas:
        print(linha)
    return linhas


def retratar_e_capturar_evento(batida, usuario, motivo):
    # Botão -> rota -> view -> retratar -> retratar_batida -> fechamento por evento.
    # app/templates/colaboradores/partials/_calendario_grade.html:127
    # app/ponto/urls.py:62, app/ponto/views.py:3453
    # app/ponto/services/retratar_humano.py:146
    # app/ponto/registro_batida.py:195, :287 e :301
    # captureOnCommitCallbacks coleta os callbacks reais sem executá-los.
    # app/chamados/tests/test_e2_celula_regenerada_reavalia.py:227
    with TestCase.captureOnCommitCallbacks(execute=False) as callbacks:
        ok, _ = retratar_humano.retratar(
            batida, motivo=motivo, observacao=OBSERVACAO, ator=usuario,
        )
        exigir(ok, "Porta recusou a retratação. app/ponto/services/retratar_humano.py:146")

    eventos = []
    for callback in callbacks:
        if (
            getattr(callback, "__module__", None) == fechamento.__name__
            and getattr(callback, "__qualname__", None) == "recalcular_por_evento.<locals>._rodar"
        ):
            contexto = getclosurevars(callback).nonlocals
            if (
                contexto["motivo"] == "retratacao"
                and contexto["colaborador"].pk == COLAB_ID
                and contexto["quando"] == batida.timestamp
            ):
                eventos.append(callback)
    exigir(len(eventos) == 1, "Evento da retratação sem confirmação. app/ponto/services/fechamento.py:1822")

    batida.refresh_from_db()
    exigir(batida.retratada_em is not None and not batida.aprovada,
           "Retratação sem confirmação. app/ponto/registro_batida.py:294")
    prazo = retratar_humano.prazo_de_desfazer(batida)
    exigir(prazo is not None, "Trilha de reversão ausente. app/ponto/services/retratar_humano.py:38")
    return eventos[0], prazo


def comparar(batida, antes):
    depois = fotografia("DEPOIS", batida, apos_retratacao=batida.retratada_em)
    diff = list(unified_diff(antes, depois, fromfile="antes", tofile="depois", lineterm=""))
    print("DIFF TXT")
    for linha in diff:
        print(linha)
    # diff_linhas soma as linhas removidas e adicionadas, sem os cabeçalhos.
    return sum(linha.startswith(("-", "+")) for linha in diff[2:])


def imprimir_reversao(usuario_id, prazo):
    # O comando é Python para o mesmo shell juliani. Janela de 7 dias da trilha.
    # app/ponto/views.py:3465 e app/ponto/services/retratar_humano.py:28, :38 e :186
    print(f"REVERSAO prazo_dias={retrar_dias()} prazo_local={timezone.localtime(prazo).isoformat()}")
    print(
        "from django.contrib.auth import get_user_model; "
        "from ponto.models import Batida; "
        "from ponto.services import retratar_humano; "
        f"print(retratar_humano.desfazer(Batida.objects.get(pk={ALVO}), "
        f"ator=get_user_model().objects.get(pk={usuario_id})))"
    )


def retrar_dias():
    # app/ponto/services/retratar_humano.py:28
    return retratar_humano.DIAS_PARA_DESFAZER


def fim(retratada, diff_linhas):
    print(f"FIM modo={MODO} retratada={retratada} diff_linhas={diff_linhas}", flush=True)


def executar():
    exigir(MODO in ("ensaio", "aplicar"), "MODO inválido. aval.txt:1")
    exigir(getattr(connection, "schema_name", None) == "juliani",
           "Schema divergente. app/config/settings/saas.py:22")
    exigir(transaction.get_autocommit() and not connection.in_atomic_block,
           "Use o shell fora de outra transação. app/ponto/services/fechamento.py:1848")

    ids_definidos = USUARIO_ID is not None and MOTIVO_ID is not None
    if MODO == "ensaio" and not ids_definidos:
        guardas(travar=False)
        listar()
        fim("nao", 0)
        return

    diff_ensaio = None
    try:
        with transaction.atomic(durable=True):
            batida = guardas(travar=True)
            motivos, usuarios = listar()
            exigir(type(USUARIO_ID) is int and USUARIO_ID in usuarios,
                   "USUARIO_ID sem permissão. app/ponto/services/retratar_humano.py:127")
            exigir(type(MOTIVO_ID) is int and MOTIVO_ID in motivos,
                   "MOTIVO_ID ausente ou inativo. app/ponto/services/retratar_humano.py:162")

            antes = fotografia("ANTES", batida)
            evento, prazo = retratar_e_capturar_evento(
                batida, usuarios[USUARIO_ID], motivos[MOTIVO_ID],
            )
            # O botão agenda _rodar em on_commit. A execução é síncrona após
            # o commit externo, na mesma conexão. O ensaio chama esse callback [nao verificado]
            # dentro do atomic, cuja reversão descarta os callbacks pendentes.
            # app/ponto/services/fechamento.py:1822–1851
            if MODO == "ensaio":
                evento()
                diff = comparar(batida, antes)
                imprimir_reversao(USUARIO_ID, prazo)
                diff_ensaio = diff
                raise ValueError("Rollback intencional do ensaio. app/ponto/services/fechamento.py:1848")
    finally:
        if diff_ensaio is not None:
            fim("nao", diff_ensaio)

    # O commit já executou o callback antes da fotografia em modo aplicar. [nao verificado]
    # Em caso de falha na fotografia após o commit, a reversão ainda é impressa. [nao verificado]
    # app/ponto/services/fechamento.py:1848 e app/ponto/services/retratar_humano.py:186
    try:
        diff = comparar(batida, antes)
    finally:
        imprimir_reversao(USUARIO_ID, prazo)
    fim("sim", diff)


# A configuração SaaS envia mensagens a streams e arquivos. O motor inclui
# nome em mensagens de erro. Silencia o logging Python durante esta execução,
# com restauração ao sair. A trilha ORM passa pela porta de auditoria.
# app/config/settings/saas.py:119–183, app/ponto/services/fechamento.py:1327
# app/ponto/registro_batida.py:245 e app/core/observ.py:133
nivel_anterior = logging.root.manager.disable
logging.disable(logging.CRITICAL)
try:
    executar()
finally:
    logging.disable(nivel_anterior)
```

_8632 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
