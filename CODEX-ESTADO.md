# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 06:25** · publicado em **15/11/2026 06:27** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 06:25 · sessao 7c8db30e6497 · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura, diff, RED/GREEN e censo registrados na [entrega c17](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/justificativa_c17_entrega.txt:1).

- [129 testes sem banco passaram](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/justificativa_c17_entrega.txt:1032). [nao verificado]
- [2/2 mutantes mortos por asserção](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/justificativa_c17_entrega.txt:327).
- [Ruff e py_compile passaram, sem gravar bytecode](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/justificativa_c17_entrega.txt:1038).

[Suite com banco reservada ao Code; sem commit](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/justificativa_c17_entrega.txt:128). [nao verificado]

## 15/11 06:13 · sessao 09489d17e7f7 · JUSTIFICATIVA-PAGA-O-PERIODO · verificador · origem=Code

1. **Existe, mas não concentra toda a decisão de cobertura.** O registro aponta para `marcos_cobertos_por_justificativa`, definida na árvore; a fonte intermediária também decide cobertura sem passar por ela. (`app/core/juizes.py:265`; `app/ponto/turnos.py:2703`; `app/ponto/services/efeito_ausencia.py:143`; `app/ponto/services/efeito_ausencia.py:187`.)

| Sítio de produção | Decisão / relação com a função registrada |
|---|---|
| `app/ponto/turnos.py:2668` | Define quais tipos nomeiam marco: `esqueci_saida→hf`, `esqueci_entrada→hi`. |
| `app/ponto/turnos.py:2686` | `ancora_nao_cobre` decide ausência de âncora, papel errado e ponta já fechada. É chamada pela função registrada em `app/ponto/turnos.py:2723` e separadamente pela fonte em `app/ponto/services/efeito_ausencia.py:204`. |
| `app/ponto/turnos.py:2720` | Função registrada: devolve dia inteiro (`None`), nenhum marco (`frozenset()`) ou o marco nomeado (`frozenset({alvo})`). |
| `app/ponto/services/efeito_ausencia.py:121` | Define o universo: justificativas aprovadas do colaborador, `data_turno` não nula e dentro do intervalo. |
| `app/ponto/services/efeito_ausencia.py:141` | Cobertura inteira anterior prevalece; tipo sem marco recebe `cobre=None` diretamente, **sem chamar a registrada**. |
| `app/ponto/services/efeito_ausencia.py:148` | Motivo conserva o inteiro; demais aprovações do dia têm os conjuntos unidos em `app/ponto/services/efeito_ausencia.py:153`. |
| `app/ponto/services/efeito_ausencia.py:185` | `_cobertura_de_uma`: sem vínculo/marcos ou sem entrada/saída previstas, conserva inteiro com `celula_sem_marco`, **sem chamar a registrada**. |
| `app/ponto/services/efeito_ausencia.py:195` | Seleciona o turno da âncora e exige o mesmo dia; resolve papéis, consulta `ancora_nao_cobre` e retorna inteiro quando há motivo. Só o restante chama a registrada, por parâmetro, em `app/ponto/services/efeito_ausencia.py:207`. |
| `app/escala/utils.py:1208` | A grade aplica precedência da ausência; aplica justificativa somente em dia de trabalho, em `app/escala/utils.py:1252` e `app/escala/utils.py:1261`. Lê a fonte intermediária em `app/escala/utils.py:1197`. |
| `app/escala/utils.py:1270` | Converte `hi/hf` em extremos da cadeia e dispensa somente células faltantes; decide `justificado` pela ausência de `missing` em `app/escala/utils.py:1283`. Usa o resultado da fonte, sem repetir o predicado de âncora. |
| `app/ponto/services/cartorio.py:802` | Lê `dia['justificado']` e dispensa lâmpadas restantes; não chama a registrada. A escrita ocorre em `app/ponto/services/cartorio.py:808`. |
| `app/escala/services/leitor_celula.py:85` | Reconstrói `justificado` pelas lâmpadas dispensadas e ausência de apagadas; transporta para o dia em `app/escala/services/leitor_celula.py:472`. Não chama a registrada. |

Também há decisões financeiras próprias:

| Sítio | Predicado efetivo |
|---|---|
| `app/ponto/services/efeito_ausencia.py:170` | Pagamento = `max(0, previsto − trabalhado)`, depois de montar a cobertura. |
| `app/ponto/services/efeito_ausencia.py:55` | Todos os dias presentes no mapa entram em `justificadas`; o mapa financeiro depende de `trabalhado != 0` e de haver apuração, em `app/ponto/services/efeito_ausencia.py:57`. |
| `app/ponto/motor_calculo_v2.py:2581` | O motor usa existência de período para `datas_trabalho`; separa abono inteiro pela ausência da chave em `minutos_justificados`, em `app/ponto/motor_calculo_v2.py:2629`. Não consulta o juiz de marcos. |
| `app/ponto/services/dia_pago.py:395` | A projeção financeira trata `sem_ata` como desconhecido, ausência pelo saldo previsto e justificativa pelo crédito recebido, em `app/ponto/services/dia_pago.py:401`. |
| `app/ponto/services/dia_pago.py:717` | Competência entregue conserva a contagem gravada; fora dela, conta dia inteiro por `tipo_dia='ausencia'` ou realizado zero, em `app/ponto/services/dia_pago.py:737`. |

**Predicado e universo:** a função pura recebe tipo, papel da âncora, papéis e marcos; a fonte acrescenta aprovação, colaborador, datas, existência da célula e composição entre justificativas. Portanto, função registrada e fonte completa não têm o mesmo universo. (`app/ponto/turnos.py:2703`; `app/ponto/services/efeito_ausencia.py:121`; `app/ponto/services/efeito_ausencia.py:141`; `app/ponto/services/efeito_ausencia.py:185`.)

Pelo delta fornecido, esses corpos não foram alterados: houve registro e troca de docstring. A comparação histórica independente com a autoridade anterior a esse registro está **[nao verificado]**.

2. **Sim, a frase existe com o nome exato `marcos_cobertos_por_justificativa`.** Consta no corte geral e no registro específico, tanto em Markdown quanto em JSON. (`app/docs/CORTES.md:341`; `app/docs/CORTES.md:342`; `app/docs/CORTES.json:1097`; `app/docs/CORTES.json:1104`.)

3. **Os dois arquivos citados leem a cadeia, mas de formas diferentes.** `efeito_ausencia.py` importa a registrada, passa-a para `_cobertura_de_uma` e a executa apenas no ramo restante; `escala/utils.py` chama `periodos_justificados`, portanto a leitura é indireta e sujeita aos desvios descritos acima. (`app/ponto/services/efeito_ausencia.py:118`; `app/ponto/services/efeito_ausencia.py:146`; `app/ponto/services/efeito_ausencia.py:207`; `app/escala/utils.py:1197`.)

Outros leitores não enumerados no comentário do registro:

| Leitor | Caminho |
|---|---|
| `efeitos_da_ausencia` | Chama a fonte em `app/ponto/services/efeito_ausencia.py:54`. |
| Fase financeira da grade | Chama a fonte com o realizado do julgamento em `app/escala/utils.py:1297`. |
| Grade da folha | Chama a fonte em `app/folha/export.py:339`. |
| Grade do espelho | Chama a fonte em `app/ponto/services/espelho.py:520`. |
| Contador de dias abonados | Chama a fonte em `app/ponto/services/dia_pago.py:722`. |
| Fechamento, PDF e esmeril | Leem por `efeitos_da_ausencia`: `app/ponto/services/fechamento.py:665`; `app/relatorios/pdf_espelho.py:332`; `app/ponto/services/esmeril_espelho.py:161`. |
| Comandos de comparação | Leem por `efeitos_da_ausencia`: `app/relatorios/management/commands/diff_extrato_parcial.py:41`; `app/ponto/management/commands/diff_espelho_alimentacao.py:117`. |

4. **Restou “fallback” em comentário identificado nominalmente com a obra.** O bloco começa com `JUSTIFICATIVA-PAGA-O-PERIODO (verificador v2 A3, 14/11)` e contém “o fallback dizia 0%% onde o gravado tem 630”. Descreve o caminho anterior, mas a palavra permanece. (`app/folha/export.py:1105`; `app/folha/export.py:1108`.)

A docstring alvo do delta já usa `de minutos_previstos_do_dia`. (`app/escala/services/leitor_celula.py:370`.)

Persistem ainda docstrings com “a cura e do motor e espera `!`; ao curar, esta assercao TROCA”, nos quatro pontos abaixo; a entrega c9 registra que os casos foram deixados como caracterização com lei/pergunta na docstring. (`app/ponto/tests/test_justificativa_c8_sem_banco.py:126`; `app/ponto/tests/test_justificativa_c8_sem_banco.py:269`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:325`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:653`; `app/ponto/tests/c9_entrega.txt:98`.)

A introdução histórica dessas ocorrências pelo **diff integral**, excluindo tudo que já existia antes da obra, está **[nao verificado]**.

5. **Escritores e leitores dos valores envolvidos no delta e no contrato que ele documenta:**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| Pergunta e endereço em `JUIZES['ausencia/ferias']` | Declaração em `app/core/juizes.py:265`. | Selo de existência em `app/ponto/tests/test_contract_juiz_ausencia.py:39`; gerador em `app/core/management/commands/gerar_diagrama.py:729` e `app/core/management/commands/gerar_diagrama.py:781`. |
| Retornos `None` / `frozenset` | Registrada em `app/ponto/turnos.py:2722`, `app/ponto/turnos.py:2724`, `app/ponto/turnos.py:2726` e `app/ponto/turnos.py:2727`; a fonte também produz cobertura inteira em `app/ponto/services/efeito_ausencia.py:144` e `app/ponto/services/efeito_ausencia.py:149`. | Fonte recebe e compõe em `app/ponto/services/efeito_ausencia.py:151` e `app/ponto/services/efeito_ausencia.py:153`; grade lê em `app/escala/utils.py:1252` e `app/escala/utils.py:1270`. |
| `celulas={(colaborador_id,data):celula}` | Carregador em `app/escala/alimentacao.py:73`; conversão do mapa por data em `app/escala/services/leitor_celula.py:407` e `app/escala/services/leitor_celula.py:442`. | Fonte passa ao helper em `app/ponto/services/efeito_ausencia.py:159`; grade passa em `app/escala/services/leitor_celula.py:443`; helper encaminha ao juiz do previsto em `app/escala/services/leitor_celula.py:375`. |
| `ata['minutos_previstos']` | Cartório monta em `app/ponto/services/cartorio.py:739` e entrega à lavra em `app/ponto/services/cartorio.py:1853`; porta atribui a ata em `app/ponto/portas/celula.py:1064` e `app/ponto/portas/celula.py:1091`. | Helper lê em `app/escala/services/leitor_celula.py:372`. |
| Previsto devolvido pelo helper | Usa ata ou `minutos_previstos_do_dia`, com zero para célula sem trabalho no segundo caminho, em `app/escala/services/leitor_celula.py:372` e `app/escala/services/leitor_celula.py:375`. | Saldo pago em `app/ponto/services/efeito_ausencia.py:170`; saída da grade em `app/escala/services/leitor_celula.py:453`. |

O registro é consumido como catálogo; o contador `fora_de_autoridade` apenas conta `PENDENTES`, e seu zero não demonstra unicidade de chamadas. (`app/core/management/commands/gerar_diagrama.py:729`; `app/core/juizes.py:1486`.)

## 15/11 06:12 · sessao 8734c0e0a169 · - · - · origem=Ronald (tmux)

Q5 — leitura estrita do delta fornecido:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `JUIZES['ausencia/ferias']['que marcos do turno a justificativa aprovada cobre?']` e endereço da função | Declaração em `app/core/juizes.py:265`; família em `app/core/juizes.py:251`. | Selo confere existência por AST em `app/ponto/tests/test_contract_juiz_ausencia.py:39`; gerador lê referências em `app/core/management/commands/gerar_diagrama.py:729`, `:770`, `:781`; diagrama agrega arquivos e contagens em `:344`. |
| `None` / `frozenset` descritos no comentário | Função produz `None` em `app/ponto/turnos.py:2722`, `:2724`, conjunto vazio em `:2726`, conjunto do marco em `:2727`. | Fonte recebe retorno por função injetada em `app/ponto/services/efeito_ausencia.py:146`; combina cobertura em `:151`, `:153`. |
| Contrato `celulas={(colaborador_id,data):celula}` nomeado pela docstring | Carregador produz mapa em `app/escala/alimentacao.py:73`; efeito o carrega em `app/ponto/services/efeito_ausencia.py:131`. Grade converte seu mapa `{data:celula}` ao contrato de pares em `app/escala/services/leitor_celula.py:407`, `:442`. | Efeito passa mapa ao helper em `app/ponto/services/efeito_ausencia.py:159`; grade passa em `app/escala/services/leitor_celula.py:443`; helper encaminha à autoridade em `:375`; autoridade usa vínculo, dia de trabalho, marcos e pausas em `app/escala/utils.py:2307`, `:2313`, `:2316`, `:2338`. |
| `ata['minutos_previstos']`, lido pelo helper cuja docstring mudou | `ata_do_dia` monta valor em `app/ponto/services/cartorio.py:739`; entrega ata à lavra em `:1851`, `:1853`; método delega em `app/escala/models.py:1826`; porta atribui em `app/ponto/portas/celula.py:1064`, `:1091` e salva em `:1066`, `:1098`. | Helper lê em `app/escala/services/leitor_celula.py:372`; resultado vai à saída da grade em `:453` e ao saldo pago em `app/ponto/services/efeito_ausencia.py:170`. |

O comentário do registro não cria despacho: API usa apenas nomes de famílias em `app/api/views_mensageria.py:1448`; contador `fora_de_autoridade` conta `PENDENTES`, não chamadas nem unicidade, em `app/core/juizes.py:1486`.

O delta fornecido altera registro e documentação; os campos de dados acima são o contexto executável referido pela docstring, sem alteração executável no helper mostrado em `app/escala/services/leitor_celula.py:372`.

## 15/11 06:11 · sessao e30ac8989913 · - · - · origem=Ronald (tmux)

Q4 tem ocorrência remanescente identificada como parte da obra: o comentário começa com `JUSTIFICATIVA-PAGA-O-PERIODO (verificador v2 A3, 14/11)` em `app/folha/export.py:1105` e contém **“o fallback dizia 0%% onde o gravado tem 630”** em `app/folha/export.py:1108`. Descreve o caminho anterior; a palavra continua no comentário. A introdução histórica pelo diff integral é [nao verificado].

O alvo do delta foi corrigido: a docstring agora diz `de minutos_previstos_do_dia` em `app/escala/services/leitor_celula.py:370`. A docstring A3 atual descreve “previsto injetado”, sem `fallback`, em `app/ponto/tests/test_justificativa_soma_sem_banco.py:425-426`.

Persistem docstrings da obra com **“a cura e do motor e espera !; ao curar, esta assercao TROCA”** em:

- `app/ponto/tests/test_justificativa_c8_sem_banco.py:126-127`;
- `app/ponto/tests/test_justificativa_c8_sem_banco.py:269-270`;
- `app/ponto/tests/test_justificativa_paga_o_periodo.py:325-326`;
- `app/ponto/tests/test_justificativa_paga_o_periodo.py:653-654`.

A entrega c9 declara a alteração desses casos para caracterização e registra que deixaram “lei/pergunta na docstring”, em `app/ponto/tests/c9_entrega.txt:98-100`. O dado fornecido já assinala que o `!` fora concedido; não inferi uma aprovação nova.

Existem outras palavras `fallback`/`band-aid` nos arquivos tocados, mas atribuí-las à obra inteira, sem comparação com a base, é [nao verificado]. Não há base suficiente para afirmar **“todos os comentários introduzidos pela obra foram saneados”** [nao verificado].

## 15/11 06:11 · sessao 5ca348ddc426 · - · - · origem=Ronald (tmux)

A função registrada existe em `app/ponto/turnos.py:2703`; o retorno efetivo é `None`, `frozenset()` ou `frozenset({alvo})` em `app/ponto/turnos.py:2720-2727`. Ela chama `ancora_nao_cobre` (`app/ponto/turnos.py:2723`), cujo predicado fica em `app/ponto/turnos.py:2686-2700`.

Não é o único sítio que determina cobertura na composição de produção:

| Sítio | Decisão e relação com juiz |
|---|---|
| `app/ponto/services/efeito_ausencia.py:121-124` | Define universo ORM: justificativas aprovadas do colaborador com `data_turno` no intervalo e não nula. |
| `app/ponto/services/efeito_ausencia.py:141-145` | Dia previamente coberto inteiro prevalece; tipo que não nomeia marco recebe `cobre=None` diretamente, sem chamada do juiz. |
| `app/ponto/services/efeito_ausencia.py:148-153` | Motivo conserva dia inteiro diretamente; demais coberturas do mesmo dia são unidas. |
| `app/ponto/services/efeito_ausencia.py:185-188` | Sem vínculo/marcos, ou sem entrada/saída prevista, retorna cobertura inteira e `celula_sem_marco`, sem chamada do juiz. Este caso não existe na assinatura do juiz registrado. |
| `app/ponto/services/efeito_ausencia.py:195-207` | Define turno da âncora, exige mesmo dia, resolve papéis; chama `ancora_nao_cobre` e conserva inteiro antes do juiz quando há motivo. Só o ramo restante chama o juiz, por parâmetro `juiz`, na linha 207. |
| `app/escala/utils.py:1197-1199` | Lê `periodos_justificados`, portanto juiz indiretamente e apenas nos ramos anteriores que chegam nele. |
| `app/escala/utils.py:1252-1260` | Aplica cobertura inteira apenas a `tipo_dia='trabalho'`; apaga marcos faltantes e órfãs. |
| `app/escala/utils.py:1261-1284` | Aplica cobertura parcial aos marcos extremos da cadeia e determina `justificado` se não restar `missing`; lê `cobre`, sem refazer tipo/âncora do juiz. |
| `app/ponto/services/cartorio.py:800-810` | Lê `dia['justificado']` e dispensa todas as lâmpadas ainda não acesas; não consulta o juiz. |
| `app/escala/services/leitor_celula.py:83-86` | Reconstrói `justificado` de lâmpada dispensada por justificativa e ausência de lâmpada apagada; lê ata, não o juiz. |

O universo do juiz puro é tipo/papel da âncora/papéis/marcos, sem status, colaborador, data ou precedência (`app/ponto/turnos.py:2703`). A fonte acrescenta essas restrições e compõe aprovações (`app/ponto/services/efeito_ausencia.py:121-153`). A aplicação da grade restringe novamente a dias de trabalho e dá precedência à ausência (`app/escala/utils.py:1208`, `app/escala/utils.py:1252`, `app/escala/utils.py:1261`). Portanto, juiz puro, fonte por dia e efeito da grade não têm o mesmo universo (`app/ponto/turnos.py:2703`; `app/ponto/services/efeito_ausencia.py:121-153`; `app/escala/utils.py:1208-1261`). Comparação histórica com implementação anterior ao registro: [nao verificado].

O comentário do registro cita dois arquivos, mas o leitor direto do juiz é somente `efeito_ausencia.py`, com importação em `app/ponto/services/efeito_ausencia.py:118-119`, passagem em `app/ponto/services/efeito_ausencia.py:146-147` e chamada em `app/ponto/services/efeito_ausencia.py:207`. `escala/utils.py` lê a fonte intermediária em `app/escala/utils.py:1197-1199`.

Outros leitores da fonte intermediária, ausentes do comentário do registro:

- `efeitos_da_ausencia`: `app/ponto/services/efeito_ausencia.py:54`.
- Fase financeira do julgamento: `app/escala/utils.py:1295-1302`.
- Grade do fechamento: `app/folha/export.py:336-341`.
- Grade do espelho: `app/ponto/services/espelho.py:519-522`.
- Contagem de dias com abono: `app/ponto/services/dia_pago.py:720-723`.

Leitores financeiros por `efeitos_da_ausencia`: fechamento em `app/ponto/services/fechamento.py:664-667`, PDF em `app/relatorios/pdf_espelho.py:332-336`; crédito chega aos motores em `app/ponto/services/fechamento.py:755`, `app/relatorios/pdf_espelho.py:407` e `app/ponto/services/espelho.py:1141`.

Separação necessária: cobertura de marcos e pagamento não usam o mesmo predicado. A fonte paga `max(0, previsto - trabalhado)` após montar cobertura (`app/ponto/services/efeito_ausencia.py:158-170`), inclui todo dia do mapa em `justificadas` (`app/ponto/services/efeito_ausencia.py:54-58`), o motor distingue dias sem período e presença da chave do crédito (`app/ponto/motor_calculo_v2.py:2581-2586`, `app/ponto/motor_calculo_v2.py:2611-2634`), e `dias_com_abono` conta por `tipo_dia='ausencia'` ou realizado zero (`app/ponto/services/dia_pago.py:729-738`).

## 15/11 06:02 · sessao 004e56fcc326 · CORTES-RECEBIDO-FORA-DA-PAUSA · censo · origem=Code

```text
O148-EDITAR | B | app/docs/BACKLOG.md:817; app/docs/BACKLOG.md:1459 | Registrada, não construir agora; complemento O148/O152. Edição por versão ainda aguarda corte de lei; frase não entregue.
IA-RBAC-INCLUDE-DO-COPILOTO | C | app/docs/RELATO.md:3603; app/docs/RELATO.md:3606; app/docs/RELATO.md:3608; app/docs/CORTES.md:351 / 329e5eca7, 4e877efa1, 44ae9d5f | Pouso integral relatado, instrumento e smoke OK; falta linha NO AR/FECHADA no BACKLOG/TICKETS. Casa documental: IA-PERGUNTAR-SEM-RBAC.
REJULGAR-COMP10-RESTO-DA-FROTA | B | app/docs/BACKLOG.md:819; app/docs/BACKLOG.md:820; app/docs/RELATO.md:3188; app/docs/RELATO.md:3476 | Atos aplicados e lista HE regenerada, 184→249. SOMBRA-X-PROD-COL988-124639 segue registrada; fechamento integral [nao verificado].
CONTRATO-20-TERMOMETRO-E-TOL | D | app/docs/CORTES.md:350; app/docs/RELATO.md:3548 / 9269eae07, 47ce5996d | Superado em parte por CONTRATO-20-TOL-DUAS-PONTAS: “(2.3) ... RETIRADO”; “TOL segue só pela saída”. (2.2) pousou; frase original inteira não.
BO-ADMIN-2710 | B | app/docs/BACKLOG.md:830; app/docs/RELATO.md:3537; app/docs/RELATO.md:160; app/docs/RELATO.md:165 / 57877acf5 | Linha ainda “medindo”; revisão mantém 8.2/8.3 não fechados. 57877acf5 instrumenta a GRADE, não comprova fechamento dos oito achados.
SEM-SELO-CAUSA-ESPELHO-09 | D | app/docs/CORTES.md:363; app/docs/RELATO.md:3516; app/docs/BACKLOG.md:1134 / 2359b756d | RESPOSTAS-TOPO-2710-1720 substitui a condição: só causa/hash nos 5 da 10; 33 da 09 intactos, col417/931→O210. Produto pousou; original não integral.
BO-[nome]-FECHA | B | app/docs/BACKLOG.md:833; app/docs/BACKLOG.md:832; app/docs/BACKLOG.md:1928; app/docs/RELATO.md:160 / 475318fa9 | Pousos parciais; 475318fa9 entrega D. TENTATIVA-DO-ADMIN-VIRA-COBRANCA registrada; justificativa/abono em obra e 8.3 não fechado. Pacote não encerrado.
BO-[nome]-NOMES | B | app/docs/BACKLOG.md:839; app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:847; app/docs/BACKLOG.md:1928 | Nomes incorporados às obras existentes, sem novo ID. Encaminhamento aplicado; justificativa/abono permanece em obra. Não equivale a fechamento das obras.
BO-ADMIN-2710-LEIS | B | app/docs/TICKETS.md:156; app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:1928; app/docs/BACKLOG.md:770 / 97bcb01c, b3bf529e | Ausência parcial e juiz do par pousaram; JUSTIFICATIVA-PAGA-O-PERIODO voltou à ordem viva, com complemento EM CURSO. Entrega apenas parcial.
JUIZ-DO-PAR-DUAS-ENTRADAS-DECISOES | D | app/docs/RELATO.md:3066; app/docs/RELATO.md:3063; app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:1932 / b3bf529e, 77c6193c | JUIZ-DO-PAR-RUBRICA-E-RESIDUO substitui “não pousa”: “obra fecha com o resíduo declarado”, corrigido para 573. Cura pousou; resto em MOTOR-LE-O-TURNO.
COBERTURA-PARCIAL-PERIODO | D | app/docs/CORTES.md:381; app/docs/TICKETS.md:156; app/docs/BACKLOG.md:1928 / 97bcb01c; anexo 399565438 WIP, 973d645ce RED | COBERTURA-ABONO-DO-PERIODO substitui o pouso conjunto: “a (A) se SEPARA”. Ausência NO AR; justificativa/abono segue em construção. Original não integral.
FIO-EXPORTADA-X-O44 | A | app/docs/BACKLOG.md:847; app/docs/BACKLOG.md:1133; app/chamados/tests/test_fio_le_o_juiz.py:277; app/chamados/tests/test_fio_le_o_juiz.py:291 / 7c9e4c27 | NO AR documental, frase inteira: exportada igual, Validar sem reabrir/push; dono-do-dia registrado. Descrição “só leitura” do BACKLOG está desatualizada; prod atual [nao verificado].
REGISTRADO-NAO-E-VIVO-DESVIO | B | app/docs/BACKLOG.md:834; app/docs/BACKLOG.md:835; app/docs/BACKLOG.md:836; app/core/tests/test_tripwire_contagem_de_pendencia.py:137; app/docs/CORTES.md:380 / 475318fa9, 5302ac95d, 667c94081 | Pouso parcial: falta um leitor em SILENCIO-VENCIDO-VOLTA, INTERROMPIDA; “TODA pendência” não entregue. REGISTRADO-CARD-3524 atualizou o card aceito para 3524.
```

## 15/11 06:01 · sessao 8fb299e5c2f7 · - · - · origem=Ronald (tmux)

**C**, pela definição estrita: A exige linha NO AR/FECHADA no BACKLOG/TICKETS. Registre o pouso integral relatado e a falta dessa casa documental, sem negar a implementação. Evidência: `app/docs/RELATO.md:3603`, `app/docs/RELATO.md:3609`, `app/docs/CORTES.md:351`.

Estado sugerido: “Pouso integral relatado, com instrumento e smoke; sem casa de encerramento [nao verificado]. Deveria constar como IA-PERGUNTAR-SEM-RBAC no BACKLOG/TICKETS.” [nao verificado]

## 15/11 06:00 · sessao 3e6ba30ae74a · CONTRATO-DE-ENTRADA-DO-FIO · construtor · origem=Code

Entrega em [cef_c5_entrega.txt](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c5_entrega.txt:1), com diff, comandos e saídas.

- Teste novo verde; controles rc=0 e seis mutantes rc=1, incluindo `sem_fechada`. [registro](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c5_entrega.txt:48) [nao verificado]
- `py_compile` rc=0, com gravação de bytecode interceptada. [registro](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c5_entrega.txt:113)
- Ruff indisponível; lint **[nao verificado]**. [tentativas](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c5_entrega.txt:100)

## 15/11 06:00 · sessao 3599e782e15d · - · - · origem=Ronald (tmux)

Classificações sugeridas, com os pontos que impedem confundir pouso parcial com A: [nao verificado]

- **JUIZ-DO-PAR-DUAS-ENTRADAS-DECISOES — D.** A exigência “NAO pousa com o residuo” de `app/docs/CORTES.md:369` foi substituída por **JUIZ-DO-PAR-RUBRICA-E-RESIDUO**, que autoriza “a obra fecha com o residuo declarado” (`app/docs/RELATO.md:3066`), depois corrigido para 573 (`app/docs/RELATO.md:3063`). A cura na fonte pousou em **b3bf529e**, com prova **77c6193c**, mas há resíduo em **MOTOR-PASSA-O-TURNO-A-VOLTA** (`app/docs/BACKLOG.md:829`, `app/docs/BACKLOG.md:866`), incorporado ao **MOTOR-LE-O-TURNO** (`app/docs/BACKLOG.md:1932`). Código: `_match_marcos` lê papel em `app/escala/utils.py:585` e `app/escala/utils.py:613`. Estado ≤200: `Superado por RUBRICA-E-RESIDUO: aceitou resíduo; cura na fonte NO AR b3bf529e, 573 seguem MOTOR-LE-O-TURNO.`

- **COBERTURA-PARCIAL-PERIODO — D.** **COBERTURA-ABONO-DO-PERIODO** substitui o pouso conjunto: “a (A) se SEPARA”, ausência pousa, justificativa espera quem pague o período (`app/docs/CORTES.md:381`). Ausência parcial **NO AR 97bcb01c** (`app/docs/TICKETS.md:156`, `app/docs/RELATO.md:8272`); **JUSTIFICATIVA-PAGA-O-PERIODO** permanece viva, retomada após HJ-090 (`app/docs/BACKLOG.md:1928`, estado mais recente em `app/docs/RELATO.md:51`). Os commits do anexo **399565438** e **973d645ce** dizem WIP/RED, não encerramento [nao verificado quanto ao conteúdo integral dos commits]. Estado ≤200: `Superado por COBERTURA-ABONO-DO-PERIODO: ausência NO AR 97bcb01c; justificativa separada, ainda em construção.`

- **FIO-EXPORTADA-X-O44 — A**, com **7c9e4c27** documentado em `app/docs/BACKLOG.md:847` e prova de deploy em `app/docs/RELATO.md:3227`. O **1fcc61e0** do anexo era WIP: o relato registra “NAO POUSA” por falta de prova (`app/docs/RELATO.md:3480`). A regra pedida existe no código: exportada ou não, validar sem reabrir (`app/chamados/services/fio.py:638`, `app/chamados/services/fio.py:647`), selo de modal e POST em competência exportada (`app/chamados/tests/test_fio_le_o_juiz.py:276`, `app/chamados/tests/test_fio_le_o_juiz.py:291`). A obra própria solicitada foi registrada (`app/docs/BACKLOG.md:1133`). A descrição antiga de BACKLOG:847 ainda diz “exportada só leitura”, contradizendo o corte e código citados. Estado ≤200: `NO AR integral pela árvore: fio igual nas exportadas, Validar sem reabrir/push; JUIZ-DE-DONO-DO-DIA registrado. Estado atual de prod [nao verificado].`

- **REGISTRADO-NAO-E-VIVO-DESVIO — B**, se julgar literalmente “TODA pendencia pelo juiz”. Produto **NO AR 475318fa** (`app/docs/BACKLOG.md:833`, `app/docs/RELATO.md:3488`); instrumento **a8730557** pousado (`app/docs/BACKLOG.md:834`); dois leitores curados pelos commits do anexo **5302ac95d/667c94081**, mas um leitor ainda não migrou e permanece em **SILENCIO-VENCIDO-VOLTA**, interrompida (`app/docs/BACKLOG.md:835`, `app/docs/BACKLOG.md:836`). O selo confirma **SEM_JUIZ** e **TETO_SEM_JUIZ=1** (`app/core/tests/test_tripwire_contagem_de_pendencia.py:137`, `app/core/tests/test_tripwire_contagem_de_pendencia.py:146`). `registrado` segue VIVOS (`app/chamados/catalogo/motor.py:24`); fila usa COBRANCA (`app/chamados/services/painel.py:189`). Também houve substituição explícita do número por **REGISTRADO-CARD-3524**: “aceito 4165 -> 3524 [...] o 3687 era do commit anterior” (`app/docs/CORTES.md:380`); D é defensável se priorizar esse corte posterior, mas A integral não é. Estado ≤200: `Pouso parcial 475318fa; AST pousado a8730557; falta 1 leitor em SILENCIO-VENCIDO-VOLTA, INTERROMPIDA. Card aceito passou a 3524.`

## 15/11 06:00 · sessao 16f693b37daf · - · - · origem=Ronald (tmux)

REJULGAR-COMP10-RESTO-DA-FROTA | B | app/docs/BACKLOG.md:819; app/docs/BACKLOG.md:820; app/docs/RELATO.md:3476; app/docs/RELATO.md:3188; app/docs/RELATO.md:3189 / gitlog anexo sem commits | G1/G2/G4 aplicados; lista HE 184→249. Divergência col988 segue registrada em SOMBRA-X-PROD-COL988-124639; fechamento integral [nao verificado].

CONTRATO-20-TERMOMETRO-E-TOL | D | app/docs/CORTES.md:350; app/docs/RELATO.md:3548; app/config/crons.py:2178 / 9269eae07, 47ce5996d | Superado parcialmente por CONTRATO-20-TOL-DUAS-PONTAS: “(2.3) ... RETIRADO”, “TOL segue só pela saída”. (2.2) pousou; frase inteira não.

SEM-SELO-CAUSA-ESPELHO-09 | D | app/docs/CORTES.md:363; app/docs/RELATO.md:3252; app/docs/RELATO.md:3516; app/ponto/services/fechamento.py:1079 / 2359b756d | Condição substituída por RESPOSTAS-TOPO-2710-1720: só causa/hash nos 5 da 10; col417/931→O210. Produto pousou; condição original dos 38 não fechou.

Detalhes para consolidar:

- REJULGAR não tem linha específica NO AR/FECHADA em BACKLOG/TICKETS; a linha `REJULGAR-COMP10-4-REVERTIDOS` diz APLICADO, “revise”, e conserva divergência na fila (`BACKLOG.md:819`); `GRADE-MESTRA.md:133` também diz “REGISTRADA (aplicação feita; divergência restante)”. A própria operação tem evidência documental forte: teste dos 3 colabs excluídos (`RELATO.md:3464`), DIFF/reversão prévios (`RELATO.md:3465`, `:3468`), G1/G2 e reversões (`:3470`, `:3471`), prova externa (`:3472`), G4 (`:3188`, `:3189`) e lista HE regravada (`:3476`). Não há commits no anexo para esse ID. [nao verificado]
- CONTRATO-20: retirada posterior explícita em `CORTES.json:955` e `:957`; condição original 13→13 falhou, pois só 1 de 12 na CxR e regressão #1224 (`RELATO.md:3264`). Cron diário às 06:00 em `app/config/crons.py:428`, autoridade declarada `:2178`, justificativa do papel prazo `:2105`. Pouso 2.2 documentado `RELATO.md:3546`–`:3549`. `BACKLOG.md:821` NO AR é outra cura, TERMOMETRO-GRAVA-SEM-TRILHA, não deve ser usado para fingir fechamento integral deste corte.
- SEM-SELO: alteração de condição está em corte posterior `RESPOSTAS-TOPO-2710-1720` (`CORTES.json:1046`, `:1048` / `CORTES.md:363`). `RELATO.md:3252` diz literalmente que condição original não fecha: 33 da 09 não mudam nada e 2 FM da 10 têm outras mudanças nas duas árvores; o aval restringe o ! ao patch e manda os 2 para O210. Complemento vivo O210 em `BACKLOG.md:1134`. Pouso do patch comprovado `RELATO.md:3479`, `:3514`–`:3516`, código `app/ponto/services/fechamento.py:1079`, teste nominal comportamental `app/ponto/tests/test_semselo5_causa_espelho_pelo_vinculo_da_janela.py:63`.

## 15/11 05:59 · sessao 8fb299e5c2f7 · - · - · origem=Ronald (tmux)

- **BO-ADMIN-2710 → B**: linha viva própria, `medindo`, em `app/docs/BACKLOG.md:830`. O relatório apresenta número/dono/destino dos oito em `app/docs/RELATO.md:3530` a `:3537`; revisão posterior registra 8.2 e 8.3 não fechados, com destino na fila, em `app/docs/RELATO.md:160`. O commit anexo `57877acf5` é instrumento da GRADE, ainda encaminhado ao verificador em `app/docs/RELATO.md:165`; não comprova cura integral.
- **BO-[nome]-FECHA → B**: pousos parciais A/B/C/D/E comprovados em `app/docs/TICKETS.md:156`, `app/docs/BACKLOG.md:829`, `:847`, `:833`, `:831`. Permanecem `TENTATIVA-DO-ADMIN-VIRA-COBRANCA`, **registrada**, em `app/docs/BACKLOG.md:832`, e abono da justificativa em obra viva, `app/docs/BACKLOG.md:1928`; complemento **EM CURSO** em `app/docs/BACKLOG.md:770`. O commit anexo `475318fa9` fecha somente D; `app/docs/RELATO.md:3488` e `:3491` publicam caso nominal. A ordem original foi substituída parcialmente por PAZ-DO-ADMIN, cuja frase declara “substitui a ordem de hoje so na fila 1”, em `app/docs/CORTES.md:375`; isso não supera o pacote inteiro.
- **BO-[nome]-NOMES → B, como encaminhamento aplicado às obras existentes**: a identificação A=COBERTURA-PARCIAL-UM-JUIZ está explícita em `app/docs/BACKLOG.md:839`; B e C permanecem com os nomes indicados em `app/docs/BACKLOG.md:829` e `:847`. O escopo A de justificativa/abono ainda tem obra viva em `app/docs/BACKLOG.md:1928`. **Não encontrei fecho independente do corte de nomes [nao verificado]**; não classificaria A apenas porque o encaminhamento já consta.
- **BO-ADMIN-2710-LEIS → B**: cobertura da ausência parcial fechada em `app/docs/TICKETS.md:156` (`97bcb01c`); juiz do par NO AR em `app/docs/BACKLOG.md:829` (`b3bf529e`, prova `77c6193c`). Porém a frase também exige justificativa limitada ao período: sua cobertura/abono foi separada da A, conforme `app/docs/RELATO.md:3157`, e segue em `JUSTIFICATIVA-PAGA-O-PERIODO`, `app/docs/BACKLOG.md:1928`, com complemento EM CURSO em `app/docs/BACKLOG.md:770`. Portanto pouso parcial, não A integral.

Linhas finais possíveis, com campo de estado curto:

```text
BO-ADMIN-2710 | B | app/docs/BACKLOG.md:830; app/docs/RELATO.md:160 / 57877acf5 | Medição/encaminhamento publicados; 8.2 e 8.3 seguem não fechados. O commit instrumenta a GRADE, não fecha os oito achados.
BO-[nome]-FECHA | B | app/docs/BACKLOG.md:832; app/docs/BACKLOG.md:1928; app/docs/BACKLOG.md:833 / 475318fa9 | Pouso parcial: D implementada; 8.2 registrada e justificativa/abono ainda em obra. Não fechar o pacote.
BO-[nome]-NOMES | B | app/docs/BACKLOG.md:839; app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:847; app/docs/BACKLOG.md:1928 | Encaminhamento aplicado às obras existentes; escopo de justificativa/abono permanece vivo. Fecho independente [nao verificado].
BO-ADMIN-2710-LEIS | B | app/docs/TICKETS.md:156; app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:1928 | Ausência parcial e juiz do par pousaram; justificativa/abono segue em obra própria. Cobertura da frase apenas parcial.
```

## 15/11 05:55 · sessao 16cf76174a73 · CONTRATO-DE-ENTRADA-DO-FIO · verificador · origem=Code

**APROVA o delta c4:** o upload deixou de presumir a origem das perguntas NULL pelo contexto corrente. A seleção agora exige motivo e batida de origem exatos. Isso corrige A2; não garante que toda legada permaneça viva ou tenha porta publicada no dia. (`app/api/foto_recebida.py:71`; `app/chamados/juizes.py:1465`; `app/chamados/services/tarefa_do_dia.py:629`)

1. **Quitação da NULL pelo upload: o casamento indevido saiu; existem efeitos indiretos distintos.**

   No resolver, nenhuma NULL chega às portas de resposta/validação. Sem pergunta da batida exata, ele retorna antes de validar, aparar motivo ou fechar. (`app/api/foto_recebida.py:71`; `app/api/foto_recebida.py:72`; `app/api/foto_recebida.py:74`)

   **O percurso completo do upload ainda pode provocar fechamento de disputa com acervo NULL:** anexar a foto salva a batida, dispara o reconciliador e alcança perguntas de qualquer motivo/origem cuja célula absolveu. Esse caminho exige que o C6 já considere a pergunta morta; grava materialização/via e pode fechar a disputa. Não responde nem valida a muda por associação com a foto. (`app/ponto/registro_batida.py:309`; `app/ponto/registro_batida.py:256`; `app/ponto/signals.py:215`; `app/chamados/reconciliador.py:628`; `app/chamados/reconciliador.py:662`; `app/chamados/reconciliador.py:596`; `app/chamados/reconciliador.py:612`)

   Validar uma pergunta exata também pode materializar uma irmã NULL **previamente respondida e validada**, pois o materializador percorre a disputa inteira. Não preenche esses dois requisitos numa NULL pendente. (`app/chamados/services/validacao.py:494`; `app/chamados/services/materializacao.py:1375`)

   Contraexemplo de legada viva que seja quitada apenas porque a foto chegou, reproduzindo A2 por outro caminho: **[nao verificado]**.

2. **A legada pode continuar visível e acionável; “fica com o admin” precisa de qualificação.**

   O modal/fio administrativo carrega os chamados sem janela e as perguntas das disputas abertas sem excluir origem NULL. A âncora é o chamado-destino ou o chamado-pai da disputa. Portanto, faltar origem não a torna invisível por si só. (`app/chamados/services/fio.py:190`; `app/chamados/services/fio.py:204`; `app/chamados/services/fio.py:251`; `app/chamados/services/fio.py:117`; `app/chamados/services/fio.py:271`)

   **Muda:** o admin vê “Aguardando resposta” e pode cobrar. **Respondida:** pode validar; a ação verifica pergunta/âncora/colaborador, e o validador exige resposta, sem exigir origem. O colaborador também tem porta de resposta sem filtro de origem. (`app/templates/chamados/partials/modal_fio.html:319`; `app/templates/chamados/partials/modal_fio.html:349`; `app/templates/chamados/partials/modal_fio.html:281`; `app/chamados/services/fio.py:636`; `app/chamados/services/validacao.py:410`; `app/chamados/services/validacao.py:418`; `app/chamados/views.py:1239`)

   `fio_vivo` apenas reduz o C6. Já `porta_alcancavel_do_dia` exige célula e chamado na fila, ou pergunta viva no app com âncora na fila; sem célula retorna `None`. O universo do app não exclui NULL. Logo, **vitalidade não prova alcance administrativo do dia**. (`app/chamados/juizes.py:1516`; `app/chamados/juizes.py:1699`; `app/chamados/juizes.py:1706`; `app/chamados/services/pergunta_no_app.py:52`)

   Há ainda uma ressalva preexistente no juiz publicado E3: foto fica fora do mapa de marcos e passa por `expor_admin`, que conserva ações administrativas. A muda sem decisões produz ação do colaborador; se restar somente chamado com verbo `cobrar`, a projeção pode publicar `rota=None`. Isso permite **modal/app visíveis e porta publicada ausente**, sem demonstrar fio invisível. Ocorrência nas perguntas do dado fornecido: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:145`; `app/chamados/services/tarefa_do_dia.py:268`; `app/chamados/services/tarefa_do_dia.py:334`; `app/chamados/services/tarefa_do_dia.py:618`; `app/chamados/services/tarefa_do_dia.py:629`)

   **Só a legada ainda viva mantém a disputa aberta:** o upload não a seleciona, `pode_fechar` exige ausência de fio vivo, e a regularização manual recusa viva sem resposta. Depois de resposta e validação, o ramo `escolha` resolve sem precisar da origem. (`app/api/foto_recebida.py:72`; `app/chamados/models.py:1917`; `app/chamados/services/fio.py:698`; `app/chamados/services/materializacao.py:1231`; `app/chamados/services/materializacao.py:1082`)

   `admin_encerra` existe como método excepcional com motivo, mas sua existência não prova botão de tela; o modal da muda oferece cobrança. Chamador produtivo dinâmico desse método: **[nao verificado]**. (`app/chamados/models.py:2007`; `app/templates/chamados/partials/modal_fio.html:334`)

3. **Retorno 0: muda o número da resposta, sem transformar o upload em erro.**

   As duas views anexam a foto antes do resolver e devolvem `status='ok'` com o resultado em `disputas_fechadas`; não ramificam por 0/1. (`app/api/views.py:851`; `app/api/views.py:861`; `app/api/views.py:863`; `app/api/views_core.py:835`; `app/api/views_core.py:845`; `app/api/views_core.py:847`)

   O contador já significa **disputas tratadas**, apesar do nome externo: incrementa mesmo sem conferir quantas validações deram certo e antes de conferir fechamento. Pode retornar 1 com a disputa aberta e a legada viva. (`app/api/foto_recebida.py:74`; `app/api/foto_recebida.py:77`; `app/api/foto_recebida.py:83`; `app/chamados/tests/test_cef_c3_sem_banco.py:175`)

   A trilha de anexação continua independente do contador. O publisher de batida retorna em atualização, pois exige `created=True`. Saem, naquele ramo NULL eliminado, os efeitos das escritas de pergunta — incluindo beco e outbox — porque essas escritas deixam de ocorrer. Não há gatilho por “retorno zero” nesses pontos. (`app/ponto/registro_batida.py:265`; `app/ponto/signals.py:42`; `app/chamados/signals.py:229`; `app/chamados/signals.py:253`; `app/api/foto_recebida.py:72`)

   Dependência desse número em APK ou consumidor externo: **[nao verificado]**.

4. **Escritores e leitores dos valores envolvidos.**

   | Valor | Quem escreve | Quem lê neste percurso |
   |---|---|---|
   | Batida/foto e colaborador | Anexação grava a foto; view recupera a batida pertencente ao usuário. (`app/ponto/registro_batida.py:309`; `app/api/views.py:846`) | Resolver recebe ambos e restringe disputas pelo colaborador. (`app/api/foto_recebida.py:65`; `app/api/foto_recebida.py:71`) |
   | Pergunta: motivo e origem | Nascimento recebe motivo, deriva origem e persiste. (`app/chamados/models.py:2949`; `app/chamados/models.py:3097`) | Filtro exato do upload. (`app/api/foto_recebida.py:71`) |
   | `ultima_batida`/contexto | APIs e retro produzem; emissão persiste/funde. (`app/api/views.py:761`; `app/api/views_core.py:755`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/services/disputa_emissao.py:576`) | Origem no nascimento continua lendo; upload C4 seleciona pela coluna, sem esse fallback. (`app/chamados/models.py:3606`; `app/api/foto_recebida.py:71`) |
   | Resposta/texto/carimbo | Porta automática grava o par. (`app/chamados/services/disputa_emissao.py:1673`) | Validador exige resposta; C6 lê sua projeção. (`app/chamados/services/validacao.py:418`; `app/chamados/juizes.py:1403`) |
   | Validação/autoria | Validador grava ambos. (`app/chamados/services/validacao.py:159`) | Materializador e C6. (`app/chamados/services/materializacao.py:1375`; `app/chamados/juizes.py:1404`) |
   | Motivos/contexto aparado | Emissão agrega; `aparar_motivo_respondido` remove. (`app/chamados/services/disputa_emissao.py:550`; `app/chamados/models.py:2030`) | Resolver restringe disputas e consulta o fio antes de aparar. (`app/api/foto_recebida.py:67`; `app/api/foto_recebida.py:81`) |
   | Fechamento da disputa | `fechar` grava carimbo, autor e resolução. (`app/chamados/models.py:1996`) | Resolver seleciona abertas e relê após validação. (`app/api/foto_recebida.py:66`; `app/api/foto_recebida.py:76`) |
   | Contador retornado | Resolver incrementa e retorna. (`app/api/foto_recebida.py:77`; `app/api/foto_recebida.py:88`) | As duas respostas HTTP. (`app/api/views.py:863`; `app/api/views_core.py:847`) |

   **Outros escritores de resposta/validação existem:** resposta humana escreve `respondida_em` e desfaz validação anterior; recuperação pela trilha completa resposta faltante; `carimbar(..., resposta='validar')` pode completar ambos quando há texto; nascimento aceita assinaturas em `campos`; reaberturas zeram carimbos. Portanto, “único escritor literal de `validada_em`” seria descrição incompleta da cópia. (`app/chamados/services/disputa_emissao.py:1482`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1695`; `app/chamados/models.py:3213`; `app/chamados/models.py:3218`; `app/chamados/models.py:3102`; `app/chamados/services/disputa_emissao.py:1609`; `app/chamados/models.py:3363`)

5. **Os testes novos mordem a seleção; não certificam toda a promessa administrativa.** [nao verificado]

   Há asserções específicas para NULL junto da origem exata, contexto A→B com duas NULL e outros motivos com/sem origem. Os cinco operadores têm alvo único; o runner exige a falha esperada e ausência de `ERROR`. Os registros mostram 29 testes verdes e cinco mutantes mortos. (`app/chamados/tests/test_cef_c3_sem_banco.py:175`; `app/chamados/tests/test_cef_c3_sem_banco.py:185`; `app/chamados/tests/test_cef_c3_sem_banco.py:199`; `app/chamados/tests/test_cef_c3_sem_banco.py:209`; `app/chamados/tests/test_cef_c3_sem_banco.py:35`; `app/chamados/tests/cef_c3_verificar.py:33`; `app/chamados/tests/cef_c4_green.txt:32`; `app/chamados/tests/cef_c4_mutantes.txt:63`; `app/chamados/tests/cef_c4_mutantes.txt:525`)

   **Mutante plausível vivo no ensaio em memória:** retirar `fechada_em__isnull=True` manteve os 29 testes verdes. As fixtures do upload oferecem somente uma disputa aberta, deixando a exclusão das fechadas sem prova. Registro persistido novo desse ensaio: **[nao verificado]**. (`app/api/foto_recebida.py:66`; `app/chamados/tests/test_cef_c1_sem_banco.py:210`; `app/chamados/tests/test_cef_c1_sem_banco.py:226`)

   O helper real de validação é substituído por double que apenas marca `viva=False`; vitalidade e fechamento também são doubles. A integração preparada usa `.update(foto=...)` e chama diretamente o resolver: não percorre anexação/sinais nem o admin. Execução ORM, alcance administrativo e persistência real nesta revisão: **[nao verificado]**. (`app/chamados/tests/test_cef_c1_sem_banco.py:215`; `app/chamados/tests/test_cef_c1_sem_banco.py:219`; `app/chamados/tests/test_cef_c1_sem_banco.py:227`; `app/chamados/tests/test_cef_c3_integracao.py:46`)

6. **Classes de reprovação e itens já enviados à fila.**

   - **Segundo juiz ou escritor novo fora da porta:** nenhum demonstrado no delta; continuam as portas de resposta/validação, `fio_vivo` e `pode_fechar`. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/api/foto_recebida.py:81`; `app/api/foto_recebida.py:83`)
   - **Derivação paralela/origem com dois significados:** o defeito A2 foi retirado; a origem individual substitui a presunção pelo agregado. (`app/api/foto_recebida.py:71`; `app/chamados/models.py:3599`)
   - **Relógio fora da borda:** o relógio interno do helper permanece preexistente; a c4 não acrescenta relógio. (`app/api/foto_recebida.py:42`; `app/api/foto_recebida.py:71`)
   - **Selo sem mordida:** há mordida sobre seleção; falta prova de upload completo e porta administrativa. (`app/chamados/tests/test_cef_c3_sem_banco.py:192`; `app/chamados/tests/test_cef_c3_integracao.py:46`)
   - **Campo com dois significados:** persiste a nomenclatura enganosa do contador “fechadas”, embora conte tratadas; não nasce nesta cura. (`app/api/foto_recebida.py:62`; `app/api/foto_recebida.py:77`; `app/api/views.py:863`)
   - **Pouso antes do pré-requisito:** nenhum novo demonstrado; a foto é anexada antes do resolver, e a NULL não recebe resposta/validação por esse filtro. (`app/api/views.py:851`; `app/api/views.py:861`; `app/api/foto_recebida.py:71`)

   **Concorrência em `renovar_contexto` e reuso em `nota.py`: nenhuma piora demonstrada pela c4.** A retirada do leitor legado não acrescenta escrita nesses caminhos; suas implementações permanecem as da fila. (`app/api/foto_recebida.py:71`; `app/chamados/models.py:2182`; `app/chamados/services/nota.py:123`)

## 15/11 05:52 · sessao 59fa89197d9f · - · - · origem=Ronald (tmux)

1. **NULL não exclui a legada da tela do admin.** Fio e modal chamam `montar_chamados_fio`; no modo admin ele carrega todos os chamados do colaborador, sem janela, e todas as perguntas das disputas abertas, sem filtro de motivo/origem. A pergunta aparece sob `chamado_destino` ou, na falta dele, `disputa.chamado`. (`app/chamados/views.py:677`; `app/chamados/views.py:889`; `app/chamados/services/fio.py:190`; `app/chamados/services/fio.py:204`; `app/chamados/services/fio.py:251`; `app/chamados/services/fio.py:255`; `app/chamados/services/fio.py:117`; `app/chamados/services/fio.py:271`)

2. **Admin vê e cobra a muda; valida depois de resposta.** A legada `foto_ausente` é pergunta de escolha; o colaborador possui formulário de opções e porta de resposta que testa propriedade/vitalidade, sem exigir `batida_origem`. No modal admin, muda mostra “Aguardando resposta” e botão Cobrar; resposta não validada ganha Validar. A seleção da ação usa pergunta e a mesma âncora da listagem; a validação exige resposta, sem exigir origem. (`app/chamados/catalogo/perguntas.py:94`; `app/templates/chamados/partials/_card_atendimento.html:77`; `app/templates/chamados/partials/_card_atendimento.html:110`; `app/chamados/views.py:1239`; `app/chamados/views.py:1242`; `app/templates/chamados/partials/modal_fio.html:319`; `app/templates/chamados/partials/modal_fio.html:349`; `app/templates/chamados/partials/modal_fio.html:281`; `app/chamados/services/fio.py:633`; `app/chamados/services/fio.py:636`; `app/chamados/services/validacao.py:410`; `app/chamados/services/validacao.py:418`)

3. **Não fecha automaticamente enquanto só a legada continuar viva.** `pode_fechar` exige ausência de vivas e de resposta sem veredito; `fechar` recusa viva sem porta de exceção. Regularizar pelo modal também recusa viva sem resposta. Depois de resposta e validação, o ramo escolha resolve sem precisar de batida/origem; pode então permitir o fechamento normal. (`app/chamados/models.py:1917`; `app/chamados/models.py:1961`; `app/chamados/services/fio.py:697`; `app/chamados/services/fio.py:698`; `app/chamados/services/materializacao.py:1077`; `app/chamados/services/materializacao.py:1082`; `app/chamados/services/materializacao.py:1086`)

4. **`admin_encerra` é exceção existente, mas não demonstrada como botão/caminho humano de produção.** O método exige motivo e delega ao fechamento excepcional; o fechamento carimba as vivas antes de salvar a disputa. Busca textual encontrou definição e testes, sem chamador produtivo literal; chamada dinâmica: [nao verificado]. Não convém chamar essa existência isolada de porta de tela. (`app/chamados/models.py:2007`; `app/chamados/models.py:2011`; `app/chamados/models.py:2014`; `app/chamados/models.py:1982`; `app/chamados/models.py:1985`)

5. **C17 tem alcance condicional, não garantia universal.** `fio_vivo` reduz o mesmo C6, sem opinião sobre origem. C17 só aceita `celula_id`; carrega perguntas vivas dessa célula e reconhece chamado da célula na fila, ou pergunta viva no app com âncora na fila. O universo do app não filtra origem NULL. Sem célula, C17 retorna `None`. Silenciamento/status/âncora/célula das perguntas 40112 e 56615: [nao verificado]. (`app/chamados/juizes.py:1516`; `app/chamados/juizes.py:1624`; `app/chamados/juizes.py:1628`; `app/chamados/juizes.py:1699`; `app/chamados/juizes.py:1702`; `app/chamados/juizes.py:1706`; `app/chamados/services/pergunta_no_app.py:52`; `app/chamados/services/pergunta_no_app.py:61`)

6. **Nuance relevante do juiz publicado E3:** ele conserva chamados sem FK de célula pelo dia forte e âncoras na fila; porém pergunta de motivo fora dos marcos, como foto, entra apenas por `expor_admin`, que conserva ações de ADMIN. A muda sem decisões produz RESPONDER/COLAB e não vira instância administrativa. Se a única porta residual tiver verbo `cobrar`, não dá dono e a rota publicada pode ser `None`. Isso permite diferença entre **modal/app visíveis**, **C17 alcançável** e **`porta_da_tarefa` sem porta**, em dia cujo único trabalho seja foto muda. É mecanismo preexistente; ocorrência concreta no acervo fornecido: [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:145`; `app/chamados/services/tarefa_do_dia.py:256`; `app/chamados/services/tarefa_do_dia.py:268`; `app/chamados/services/tarefa_do_dia.py:331`; `app/chamados/services/tarefa_do_dia.py:338`; `app/chamados/services/tarefa_do_dia.py:599`; `app/chamados/services/tarefa_do_dia.py:601`; `app/chamados/services/tarefa_do_dia.py:618`; `app/chamados/services/tarefa_do_dia.py:624`; `app/chamados/services/tarefa_do_dia.py:629`)

7. **Nenhuma prova de fio morto invisível por ser NULL.** A condição NULL não participa dos caminhos de listagem, cobrança ou validação acima. A c4 deixa de quitar a muda pelo upload e mantém o fluxo normal de resposta e decisão; “fica com admin” precisa ser lido como **visível/cobrável e validável após resposta**, sem prometer encerramento unilateral da muda pela tela. (`app/api/foto_recebida.py:71`; `app/chamados/services/fio.py:255`; `app/templates/chamados/partials/modal_fio.html:319`; `app/templates/chamados/partials/modal_fio.html:349`; `app/chamados/services/validacao.py:418`)

## 15/11 05:51 · sessao f18502481a68 · - · - · origem=Ronald (tmux)

1. **Retorno 0:** ambas views continuam respondendo `status='ok'`; muda apenas `disputas_fechadas`, após a foto já ter sido anexada. Não há ramificação por esse contador. (`app/api/views.py:851`; `app/api/views.py:861`; `app/api/views.py:863`; `app/api/views_core.py:835`; `app/api/views_core.py:845`; `app/api/views_core.py:847`)

2. **Contador tem significado preexistente de “disputas tratadas”:** incrementa depois da seleção/validação, ignora quantas perguntas validaram e só depois examina fechamento; pode ser 1 com disputa aberta e legada viva. (`app/api/foto_recebida.py:74`; `app/api/foto_recebida.py:77`; `app/api/foto_recebida.py:78`; `app/api/foto_recebida.py:83`; `app/chamados/tests/test_cef_c3_sem_banco.py:175`)

3. **Log/SSE:** anexar foto continua registrando trilha independentemente do retorno; o publisher de batida retorna quando `created=False`. As escritas de pergunta anteriormente feitas no ramo NULL deixavam eventos de beco/outbox; retirando essa seleção, esses efeitos deixam de nascer desse ramo. (`app/ponto/registro_batida.py:309`; `app/ponto/registro_batida.py:256`; `app/ponto/registro_batida.py:265`; `app/ponto/signals.py:39`; `app/ponto/signals.py:42`; `app/chamados/signals.py:229`; `app/chamados/signals.py:253`)

   Consumidor JS/TS/HTML/Java/Kotlin/Dart desse campo não foi encontrado por busca textual; dependência no APK externo: **[nao verificado]**.

4. **Testes mordem a seleção:** origem exata misturada com NULL, contexto trocado A→B e motivos diferentes têm asserções específicas. Os cinco operadores têm alvo único e o runner exige falha de asserção prevista, sem `ERROR`. (`app/chamados/tests/test_cef_c3_sem_banco.py:175`; `app/chamados/tests/test_cef_c3_sem_banco.py:185`; `app/chamados/tests/test_cef_c3_sem_banco.py:199`; `app/chamados/tests/test_cef_c3_sem_banco.py:209`; `app/chamados/tests/test_cef_c3_sem_banco.py:35`; `app/chamados/tests/cef_c3_verificar.py:33`)

   Executei diretamente os módulos com `PYTHONDONTWRITEBYTECODE=1`: controle 29 testes verde; mutantes morreram com 2/4/8/9/1 falhas, sem erros. Corresponde aos registros fornecidos. (`app/chamados/tests/cef_c4_green.txt:32`; `app/chamados/tests/cef_c4_mutantes.txt:63`; `app/chamados/tests/cef_c4_mutantes.txt:140`; `app/chamados/tests/cef_c4_mutantes.txt:302`; `app/chamados/tests/cef_c4_mutantes.txt:484`; `app/chamados/tests/cef_c4_mutantes.txt:525`)

5. **Mutante plausível vivo:** retirei somente `fechada_em__isnull=True` da seleção, em `c1.[nome]`, mantendo tudo em memória: os 29 testes continuaram verdes. As fixtures só oferecem uma disputa aberta, portanto não demonstram exclusão de disputa fechada. Registro persistido desse ensaio: **[nao verificado]**. (`app/api/foto_recebida.py:66`; `app/chamados/tests/test_cef_c1_sem_banco.py:210`; `app/chamados/tests/test_cef_c1_sem_banco.py:226`; `app/chamados/tests/test_cef_c1_sem_banco.py:27`)

6. **Limite relevante:** `_validar_pelo_dono` é substituído por double que apenas marca `viva=False`; `fio_vivo` e `pode_fechar` também são doubles. A integração C3 prepara foto com `.update()`, chama diretamente o resolver e não exercita upload completo nem porta admin. Execução ORM: **[nao verificado]**. (`app/chamados/tests/test_cef_c1_sem_banco.py:215`; `app/chamados/tests/test_cef_c1_sem_banco.py:219`; `app/chamados/tests/test_cef_c1_sem_banco.py:227`; `app/chamados/tests/test_cef_c1_sem_banco.py:230`; `app/chamados/tests/test_cef_c3_integracao.py:46`; `app/chamados/tests/test_cef_c3_integracao.py:48`)

7. **Classes do delta:** ele conserva os donos de resposta/validação e os juízes de vitalidade/fechamento, removendo a derivação contexto→origem NULL. O relógio interno é preexistente. Não há piora demonstrada dos caminhos A1/`nota.py`/`renovar_contexto` pelo trecho produtivo alterado. (`app/api/foto_recebida.py:42`; `app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/api/foto_recebida.py:71`; `app/api/foto_recebida.py:81`; `app/api/foto_recebida.py:83`)

## 15/11 05:51 · sessao 82681902e4d0 · - · - · origem=Ronald (tmux)

1. **O resolver C4 não entrega NULL ao escritor de resposta/validação.** Seleciona exclusivamente `motivo='foto_ausente', batida_origem=batida`; sem candidata, sai antes das portas e do fechamento. (`app/api/foto_recebida.py:71`; `app/api/foto_recebida.py:72`; `app/api/foto_recebida.py:74`)

2. **Há efeitos genéricos do upload capazes de carimbar/fechar acervo NULL, mas não demonstram o erro A2.** `anexar_foto` chama `anotar`, cujo `save` dispara a reconciliação do colaborador; esta inclui perguntas de qualquer motivo/origem cuja célula absolve e cujo C6 já declara mortas. Carimba `materializada_em/via`, preservando resposta/validação, e fecha a disputa se `pode_fechar`. É alcance genérico pelo C6, não casamento foto↔NULL pelo contexto. (`app/ponto/registro_batida.py:309`; `app/ponto/registro_batida.py:256`; `app/ponto/signals.py:206`; `app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1433`; `app/chamados/reconciliador.py:628`; `app/chamados/reconciliador.py:662`; `app/chamados/reconciliador.py:596`; `app/chamados/reconciliador.py:612`)

   **Contraexemplo de legada viva antes do upload que morra apenas pela presença da foto: [nao verificado].** O C6 pode absolver pela célula; no tipo `escolha`, o lastro lê `materializada_em`, e sem carimbo o ramo final mantém viva. (`app/chamados/juizes.py:1450`; `app/chamados/juizes.py:1465`; `app/chamados/juizes.py:709`; `app/chamados/juizes.py:1479`)

3. **Outra cauda pode materializar irmã NULL previamente respondida e validada.** Validar uma pergunta exata chama o materializador da disputa inteira, que busca todas com ambos os carimbos e sem materialização; não responde/valida uma NULL pendente nesse caminho. (`app/chamados/services/validacao.py:494`; `app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1381`)

4. **Escritores persistidos de `respondida_em`/`validada_em`:**
   
   - Resposta humana: escreve resposta/carimbo e desfaz validação anterior. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`)
   - Resposta automática: escreve resposta/carimbo; único chamador produtivo encontrado é o helper da foto. (`app/chamados/services/disputa_emissao.py:1673`; `app/api/foto_recebida.py:48`)
   - Recuperação pela trilha: preenche carimbo faltante, chamada pelo comando de cura de limbo. (`app/chamados/services/disputa_emissao.py:1691`; `app/chamados/services/disputa_emissao.py:1695`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`)
   - Validador: grava validação/autoria; devolução recusada pode zerá-las. (`app/chamados/services/validacao.py:159`; `app/chamados/services/validacao.py:224`)
   - Porta `carimbar`, com texto e opção `resposta`: preenche os carimbos ausentes; chamada pelo materializador e S128. (`app/chamados/models.py:3213`; `app/chamados/models.py:3218`; `app/chamados/services/materializacao.py:1283`; `app/chamados/services/disputa_emissao.py:2007`)
   - Reabertura e ressuscitação: zeram carimbos pela porta de lote. (`app/chamados/services/disputa_emissao.py:1609`; `app/chamados/services/disputa_emissao.py:1648`; `app/chamados/models.py:1613`; `app/chamados/models.py:3363`)
   - Nascimento: recebe assinaturas em `**campos`, persiste e registra os atos. (`app/chamados/models.py:3097`; `app/chamados/models.py:3100`; `app/chamados/models.py:3102`)

   As atribuições em cópia de simulação e restauração da memória não constituem escritores persistidos independentes. (`app/chamados/models.py:1541`; `app/chamados/models.py:1554`; `app/chamados/models.py:3242`)

5. **Legada ainda viva impede fechamento normal.** `pode_fechar` exige ausência de resposta sem veredito e de fio vivo; `fechar` recusa vivas sem exceção nomeada. Admin pode encerrar com motivo, carimbando identidade `admin_decidiu`; esse ato não exige origem preenchida e não escreve resposta/validação. (`app/chamados/models.py:1917`; `app/chamados/models.py:1961`; `app/chamados/models.py:1982`; `app/chamados/models.py:2007`; `app/chamados/models.py:3147`)

**Nenhum novo caminho de quitação NULL por contexto da foto foi demonstrado; o caminho incorreto A2 foi retirado.** (`app/api/foto_recebida.py:71`)

## 15/11 05:48 · sessao aa3076601e72 · CONTRATO-DE-ENTRADA-DO-FIO · construtor · origem=Code

Cura aplicada: upload seleciona somente a origem exata; legadas ficam com o admin ([foto_recebida.py:60](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/api/foto_recebida.py:60)). [nao verificado]

[Entrega completa](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c4_entrega.txt:53): RED com três falhas, GREEN com 29 testes, cinco mutantes mortos, censo e comandos. [nao verificado]

Ruff indisponível; integração e certificação pelo Code/opus pendentes ([registro](/home/ronald/wt-codex-contrato-de-entrada-do-fio-005238/app/chamados/tests/cef_c4_entrega.txt:164)).

## 15/11 05:40 · sessao a8ee662a6825 · CONTRATO-DE-ENTRADA-DO-FIO · verificador · origem=Code

**REPROVA.** A1 corrige o apagamento sequencial apontado na v1; A2 pode validar pergunta legada de **outra batida** usando a última identidade do contexto agregado. (`app/chamados/services/disputa_emissao.py:569`; `app/api/foto_recebida.py:70`; `app/chamados/models.py:3599`)

1. **A1 — cura correta, com alcance limitado.**

   Mesmo `id` conhecido: `{**anterior, **novo}` preserva `recebida_em` omitida e deixa valor explicitamente enviado vencer. Outro `id`, identidade ausente ou bloco incompatível: substituição integral. Isso cura API→retro consecutivos da mesma batida; não transforma o contexto em histórico de episódios. (`app/chamados/services/disputa_emissao.py:569`; `app/chamados/services/disputa_emissao.py:573`; `app/chamados/services/disputa_emissao.py:575`)

   **Reuso por `nota.py` não repete esse apagamento:** escreve `Chamado.contexto_json`, delegando a `fundir_contexto`; dicionário já presente não é substituído. Entretanto, também não incorpora uma recepção nova se `ultima_batida` já existir sem ela, podendo conservar inclusive um episódio antigo. É comportamento distinto da fusão da disputa. (`app/chamados/services/disputa_emissao.py:600`; `app/chamados/services/nota.py:110`; `app/chamados/services/nota.py:123`; `app/chamados/services/contexto.py:155`)

   Os outros escritores examinados preservam o bloco sequencialmente: preenchimento de motivo/data, remoção da chave de um motivo e acréscimo de `revalidacoes`. O envelope atribuído no gerador pertence a uma cópia transitória. (`app/chamados/services/disputa_emissao.py:291`; `app/chamados/services/disputa_emissao.py:312`; `app/chamados/models.py:2028`; `app/chamados/models.py:2104`; `app/chamados/models.py:2128`)

   **Há possibilidade preexistente de perda por concorrência:** o cron carrega contexto antigo; a API salva a recepção; depois `renovar_contexto('ultimo_alerta_diario', ...)` salva o JSON inteiro da instância antiga. Essa porta não relê o contexto antes de gravar. Ocorrência concreta na sombra: **[nao verificado]**. (`app/ponto/management/commands/alertar_disputas_pendentes.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48`; `app/chamados/models.py:2182`; `app/chamados/models.py:2187`)

2. **A2 — bloqueio: contexto de B pode quitar pergunta NULL de A.**

   Cenário: pergunta de foto da batida A permanece viva, com origem NULL; outra emissão B reutiliza a disputa e substitui `ultima_batida` por B; chega foto de B. O ramo novo inclui **todas** as perguntas `foto_ausente` NULL daquela disputa, inclusive a de A. Não compara dia nem vínculo individual. O próprio modelo documenta que esse contexto, depois do nascimento, pode apontar outra anomalia. (`app/chamados/services/disputa_emissao.py:540`; `app/chamados/services/disputa_emissao.py:575`; `app/api/foto_recebida.py:70`; `app/api/foto_recebida.py:72`; `app/chamados/models.py:3599`)

   As portas seguintes não corrigem a seleção: recebem pergunta/disputa, sem receber a batida do upload. O ramo `escolha` de `foto_ausente` pode materializar sem conferir foto ou episódio; a validação já retira a pergunta da vitalidade. Portanto, **usar o dono correto da escrita não garante que ele recebeu a pergunta correta**. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/chamados/catalogo/perguntas.py:94`; `app/chamados/services/materializacao.py:1082`; `app/chamados/services/materializacao.py:1238`; `app/chamados/juizes.py:1404`)

3. **Sombra — duas candidatas, quantidade com associação errada desconhecida.**

   O dado fornecido registra 15 perguntas vivas NULL e duas com disputa aberta/contexto com ID: pergunta **40112**, disputa **5158**, batida contextual **118315**; pergunta **56615**, disputa **8065**, batida contextual **155158**. Isso identifica candidatas, não comprova a origem histórica delas. (`sonda.out:3`, anexo fornecido; `app/chamados/tests/cef_c3_sonda.py:23`; `app/chamados/tests/cef_c3_sonda.py:31`)

   **Quantas dessas duas caem no cenário: [nao verificado].** A sonda não mede correspondência pergunta↔episódio, nem exige o motivo na lista atual da disputa, exigência existente no resolver. (`app/chamados/tests/cef_c3_sonda.py:23`; `app/chamados/tests/cef_c3_sonda.py:33`; `app/api/foto_recebida.py:64`)

   Para contar, faltam elegibilidade completa do resolver, existência/posse da batida contextual e lastro independente da batida que originou cada pergunta. Comparar dia do turno pode revelar incompatibilidade, mas **dia igual não prova episódio igual**: o acervo NULL tem somente chave de dia. Se o lastro disponível não distinguir episódios, o resultado deve continuar indeterminado. (`app/chamados/models.py:3610`; `app/chamados/models.py:3599`; `app/chamados/services/fio.py:1286`)

   Não reabro o bloqueio antigo da medição 140/0, descartado pela triagem fornecida. (`triagem_v1.txt:6`, anexo fornecido)

4. **O `|` não causa o erro de associação.**

   Os conjuntos são disjuntos: `batida_origem=batida` versus `batida_origem IS NULL`, ambos no mesmo relacionamento da disputa. Não há sobreposição entre os ramos que explique duplicação; a união inclui o conjunto legado inteiro quando o contexto casa. Quando não casa, as NULL ficam excluídas deliberadamente. O problema é esse critério de inclusão. (`app/api/foto_recebida.py:68`; `app/api/foto_recebida.py:70`; `app/api/foto_recebida.py:72`)

   Execução SQL real: **[nao verificado]**. O teste sem banco usa uma implementação própria de união, que não demonstra comportamento do ORM. (`app/chamados/tests/test_cef_c1_sem_banco.py:49`; `app/chamados/tests/test_cef_c3_sem_banco.py:3`)

5. **Escritores e leitores dos valores envolvidos.**

   | Valor | Quem escreve | Quem lê |
   |---|---|---|
   | `ultima_batida.id` | APIs e retro; fusão persiste na disputa. (`app/api/views.py:762`; `app/api/views_core.py:756`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:99`; `app/chamados/services/disputa_emissao.py:576`) | Comparação A1; associação legada A2; busca do dia/episódio no nascimento. (`app/chamados/services/disputa_emissao.py:571`; `app/api/foto_recebida.py:70`; `app/chamados/services/disputa_emissao.py:396`; `app/chamados/models.py:3606`) |
   | `tipo` | APIs e retro. (`app/api/views.py:763`; `app/api/views_core.py:757`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:100`) | Transporte genérico pelo merge; leitor semântico específico: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:573`) |
   | `timestamp` | APIs: instante local do fato; retro: timestamp local da batida. (`app/api/views.py:764`; `app/api/views_core.py:758`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`) | Juiz fraco extrai a data da string. (`app/chamados/juizes.py:162`; `app/chamados/juizes.py:169`) |
   | `recebida_em` | APIs; retro omite; A1 conserva a omissão no mesmo episódio. (`app/api/views.py:765`; `app/api/views_core.py:759`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/services/disputa_emissao.py:573`) | Merge genérico; consumidor semântico específico: **[nao verificado]**. O juiz fraco lê `timestamp`. (`app/chamados/juizes.py:163`) |
   | `Disputa.contexto` / `Chamado.contexto_json` | Emissão escreve ambos no nascimento; agrega contexto da disputa; reuso do chamado passa por nota/contexto. (`app/chamados/services/disputa_emissao.py:594`; `app/chamados/services/disputa_emissao.py:604`; `app/chamados/services/disputa_emissao.py:576`; `app/chamados/services/nota.py:123`) | Gerador usa envelope do ato; upload legado usa agregado; template exibe contexto do chamado. (`app/chamados/services/disputa_emissao.py:315`; `app/api/foto_recebida.py:70`; `app/templates/chamados/partials/painel_chamado_turno.html:35`) |
   | Pergunta: `motivo`, `batida_origem`, `data_evento` | Porta de nascimento recebe motivo, deriva origem e grava dia pela porta do fio. (`app/chamados/models.py:2903`; `app/chamados/models.py:2949`; `app/chamados/models.py:3097`; `app/chamados/models.py:3099`) | Dedup lê episódio/acervo; upload lê motivo e origem, sem usar dia no ramo legado. (`app/chamados/models.py:2798`; `app/api/foto_recebida.py:68`; `app/api/foto_recebida.py:72`) |
   | Resposta e validação | `responder_automaticamente` grava texto/carimbo; validação grava carimbo/autor. (`app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/validacao.py:159`) | Validador, materializador e juiz de vitalidade. (`app/chamados/services/validacao.py:418`; `app/chamados/services/materializacao.py:1375`; `app/chamados/juizes.py:1403`) |
   | Motivos e fechamento da disputa | Emissão agrega motivos; portas existentes aparam motivo e fecham disputa. (`app/chamados/services/disputa_emissao.py:550`; `app/chamados/models.py:2030`; `app/chamados/models.py:1996`) | Resolver seleciona abertas com `foto_ausente`; consulta vitalidade e possibilidade de fechar. (`app/api/foto_recebida.py:62`; `app/api/foto_recebida.py:82`; `app/api/foto_recebida.py:84`) |

6. **Testes mordem os cortes escolhidos, mas não o bloqueio encontrado.** [nao verificado]

   Os quatro operadores registrados têm asserções específicas: preservar sempre, substituir sempre, retirar legado e aceitar legado sem conferir ID. O runner distingue falha esperada de erro de execução. (`app/chamados/tests/cef_c3_verificar.py:12`; `app/chamados/tests/cef_c3_verificar.py:31`)

   A fixture sem banco assume uma NULL associada ao contexto atual; a integração apaga a origem justamente da última batida emitida. Nenhuma exercita **NULL de A → contexto atualizado para B → upload de B**, nem múltiplas NULL de episódios diferentes. (`app/chamados/tests/test_cef_c3_sem_banco.py:140`; `app/chamados/tests/test_cef_c3_integracao.py:33`; `app/chamados/tests/test_cef_c3_integracao.py:38`)

   **Mutante plausível vivo na C3 sem banco:** retirar `motivo='foto_ausente'` apenas do filtro legado, passando a selecionar NULL de outros motivos. O ensaio em memória manteve os dez testes verdes; as fixtures desse caminho só contêm `foto_ausente`. Registro persistido desse ensaio: **[nao verificado]**. (`app/api/foto_recebida.py:72`; `app/chamados/tests/test_cef_c3_sem_banco.py:140`; `app/chamados/tests/test_cef_c1_sem_banco.py:208`)

   ORM, persistência, concorrência e validação real continuam fora da prova sem banco; execução da integração nesta revisão: **[nao verificado]**. (`app/chamados/tests/test_cef_c3_sem_banco.py:3`; `app/chamados/tests/test_cef_c1_sem_banco.py:219`; `app/chamados/tests/test_cef_c3_integracao.py:1`)

7. **Classes de reprovação.**

   **Derivação paralela / campo com dois significados: presente em A2.** O último episódio do agregado passa a funcionar como origem presumida de cada pergunta NULL, contrariando a distinção documentada entre contexto corrente e episódio da pergunta. (`app/api/foto_recebida.py:70`; `app/chamados/models.py:3599`; `app/chamados/services/fio.py:1286`)

   **Segundo juiz de vitalidade ou segundo escritor de resposta/validação: nenhum novo demonstrado nesses trechos.** As portas e `fio_vivo` são reutilizados; o defeito está no universo entregue a elas. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/api/foto_recebida.py:82`)

   **Relógios internos são preexistentes**, para `ultima_em` e resposta automática; a convivência de timestamp legado de recepção com timestamp novo do fato também já está documentada. (`app/chamados/services/disputa_emissao.py:567`; `app/api/foto_recebida.py:42`; `app/chamados/juizes.py:152`; `app/chamados/juizes.py:155`)

   **O selo tem mordida parcial:** verifica estrutura do envelope, mas não comprova correspondência histórica entre pergunta NULL e contexto. Os quatro mutantes mortos não cobrem essa propriedade. (`app/chamados/tests/test_e1_v5_familia.py:133`; `app/chamados/tests/test_e1_v5_familia.py:170`; `app/chamados/tests/cef_c3_verificar.py:12`)

## 15/11 05:38 · sessao 862111608ebb · - · - · origem=Ronald (tmux)

**A2 continua inseguro: o ramo legado confunde contexto agregado com identidade da pergunta.** (`app/api/foto_recebida.py:70`; `app/chamados/models.py:3599`)

1. **Cenário demonstrável:** pergunta A `foto_ausente`, origem NULL, permanece viva; uma nova emissão B reutiliza a disputa e substitui `ultima_batida` por B; chega upload de B. O ramo legado seleciona A porque filtra apenas motivo/NULL na disputa, sem conferir episódio ou dia da pergunta. Isso pode ocorrer mesmo se B nasceu por outra anomalia. (`app/chamados/services/disputa_emissao.py:540`; `app/chamados/services/disputa_emissao.py:575`; `app/api/foto_recebida.py:70`; `app/api/foto_recebida.py:72`)

2. **As portas existentes não corrigem essa seleção.** O helper passa somente pergunta/disputa ao respondedor e validador; nenhuma batida é passada para verificar correspondência. O respondedor grava texto/carimbo; o validador busca por PK/disputa e carimba a validação. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/chamados/services/disputa_emissao.py:1673`; `app/chamados/services/validacao.py:410`; `app/chamados/services/validacao.py:460`; `app/chamados/services/validacao.py:159`)

   `foto_ausente` tem resposta `escolha`; esse ramo reconcilia destino e retorna verdadeiro sem consultar foto/episódio. A exigência posterior de testemunha aplica-se a `hora`/`data_hora`; pergunta validada morre imediatamente pelo juiz. (`app/chamados/catalogo/perguntas.py:94`; `app/chamados/services/materializacao.py:1069`; `app/chamados/services/materializacao.py:1085`; `app/chamados/services/materializacao.py:1238`; `app/chamados/juizes.py:1404`)

3. **Das duas da sombra, quantidade afetada: [nao verificado].** A sonda só observa pergunta viva NULL, disputa aberta e ID presente; não verifica se o ID representa a pergunta, sequer exige `foto_ausente` nos motivos atuais da disputa como o resolver exige. (`app/chamados/tests/cef_c3_sonda.py:23`; `app/chamados/tests/cef_c3_sonda.py:27`; `app/chamados/tests/cef_c3_sonda.py:31`; `app/api/foto_recebida.py:64`)

   Medir para `(40112,5158,118315)` e `(56615,8065,155158)`: elegibilidade completa da disputa/upload, existência e dono da batida do contexto, dia do turno versus dia da pergunta e, principalmente, evidência independente da batida que originou a pergunta. Os pares vêm somente do anexo fornecido: [nao verificado]. Dias diferentes podem revelar incompatibilidade; dias iguais não demonstram igualdade de episódio, pois o acervo tem apenas chave de dia. (`app/chamados/services/fio.py:1286`; `app/chamados/models.py:3599`)

4. **O `|` não explica o defeito lógico.** Os predicados `origem=batida` e `origem IS NULL` são disjuntos e usam o mesmo manager da disputa; o problema é o universo indevido incluído pelo segundo predicado. (`app/api/foto_recebida.py:68`; `app/api/foto_recebida.py:71`)

   SQL real, plano e comportamento efetivo do Django: **[nao verificado]**. Django não está disponível neste ambiente; o double implementa sua própria união sem repetição e os testes declaram não demonstrar SQL. (`app/chamados/tests/test_cef_c1_sem_banco.py:49`; `app/chamados/tests/test_cef_c3_sem_banco.py:3`)

5. **Testes não cobrem deslocamento posterior do contexto.** A fixture sem banco só chama a NULL de “legada de 101”, sem identidade independente; a integração também deixa por último exatamente o episódio convertido em NULL. Não exercita legada A seguida por atualização do contexto para B e upload B. (`app/chamados/tests/test_cef_c3_sem_banco.py:140`; `app/chamados/tests/test_cef_c3_sem_banco.py:153`; `app/chamados/tests/test_cef_c3_integracao.py:33`; `app/chamados/tests/test_cef_c3_integracao.py:38`)

6. **Mutante plausível vivo confirmado em memória:** remover `motivo='foto_ausente'` somente do filtro legado, permitindo validar NULL de outros motivos. Os dez testes C3 passaram; registro persistido da execução: **[nao verificado]**. As fixtures de upload contêm somente `foto_ausente`, portanto não detectam a ampliação. (`app/api/foto_recebida.py:72`; `app/chamados/tests/test_cef_c3_sem_banco.py:140`; `app/chamados/tests/test_cef_c3_sem_banco.py:141`; `app/chamados/tests/test_cef_c1_sem_banco.py:208`)

## 15/11 05:37 · sessao ba44442ab92c · - · - · origem=Ronald (tmux)

1. **Os testes C3 mordem os quatro mutantes escolhidos**, exigindo falha de asserção identificada e recusando erro de execução. Há controle verde e falhas registradas para os operadores; isso prova esses cortes, não completude. (`app/chamados/tests/cef_c3_verificar.py:12`; `app/chamados/tests/cef_c3_verificar.py:31`; `app/chamados/tests/cef_c3_mutantes.txt:17`; `app/chamados/tests/cef_c3_mutantes.txt:33`)

2. **Mutante plausível vivo novo:** fazer a recepção antiga vencer quando a nova emissão trouxer `recebida_em` explícita. Substituí somente a fonte em memória de `ctx[k] = {**ctx[k], **v}` por:

   ```python
   ctx[k] = {
       **ctx[k], **v,
       **({'recebida_em': ctx[k]['recebida_em']}
          if 'recebida_em' in ctx[k] else {}),
   }
   ```

   O controle executado passou 10/10 e esse mutante também passou 10/10, com `PYTHONDONTWRITEBYTECODE=1`, sem arquivo de saída. O código correto permite valor novo vencer; o caso chamado “atualiza valor presente” só altera `timestamp`, deixando essa recepção explícita sem testemunha. A integração também apenas omite a recepção ou troca o episódio. (`app/chamados/services/disputa_emissao.py:573`; `app/chamados/tests/test_cef_c3_sem_banco.py:113`; `app/chamados/tests/test_cef_c3_integracao.py:18`)

3. **A2 tem uma lacuna mais grave que os mutantes cobertos:** cada teste sem banco cria uma única NULL e assume sua associação ao `ctx.id=101`. Na integração, a NULL é construída apagando a origem da própria batida que ficou por último no contexto. O caso negativo envia uma terceira batida cujo id diverge do contexto. Nenhum desses casos representa NULL antiga de A, contexto atualizado para B e upload de B; tampouco duas NULL de episódios distintos no mesmo agrupador. (`app/chamados/tests/test_cef_c3_sem_banco.py:140`; `app/chamados/tests/test_cef_c3_sem_banco.py:153`; `app/chamados/tests/test_cef_c3_integracao.py:32`; `app/chamados/tests/test_cef_c3_integracao.py:60`)

4. **Limites efetivos:** ORM, transação, `save`, validação e vitalidade são doubles no executor sem banco. `_validar_pelo_dono` é substituído por função que simplesmente marca as perguntas selecionadas como mortas. Portanto, o mutante antigo `_validar_pelo_dono -> return 0` permanece invisível nesse executor; a integração tem asserções de `validada_em` e vitalidade que o denunciariam, mas sua execução nesta revisão é **[nao verificado]**. (`app/chamados/tests/test_cef_c3_sem_banco.py:3`; `app/chamados/tests/test_cef_c1_sem_banco.py:219`; `app/chamados/tests/test_cef_c1_sem_banco.py:230`; `app/chamados/tests/test_cef_c3_integracao.py:53`)

5. **Classes de reprovação:**
   
   - **Derivação paralela / campo com dois significados: demonstrada em A2.** O leitor toma o último id do agregado como origem de todas as NULL selecionadas. O contrato da coluna documenta precisamente que, depois do nascimento, esse contexto pode apontar outra anomalia e que o acervo não recebe episódio por essa derivação; o acervo só tem chave de dia. (`app/api/foto_recebida.py:70`; `app/api/foto_recebida.py:72`; `app/chamados/models.py:3599`; `app/chamados/services/fio.py:1286`)
   - **Segundo juiz de vitalidade / segundo escritor de resposta ou validação:** nenhum novo demonstrado no delta; o resolver usa `fio_vivo` e a rotina delega às portas de resposta e validação existentes. O defeito está no universo entregue a essas portas. (`app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`; `app/api/foto_recebida.py:82`)
   - **Relógio fora da borda:** há relógios internos preexistentes para `ultima_em` e resposta automática; C3 não acrescenta outro nesses trechos. (`app/chamados/services/disputa_emissao.py:567`; `app/api/foto_recebida.py:42`)
   - **Selo sem mordida:** a catraca AST verifica presença estrutural de envelope/id, sem provar valor em execução, domínio do ramo ou correspondência histórica entre pergunta e contexto. Sua mordida não cobre A2. (`app/chamados/tests/test_e1_v5_familia.py:133`; `app/chamados/tests/test_e1_v5_familia.py:170`; `app/chamados/tests/test_e1_v5_familia.py:180`)
   - **Timestamp com significados recepção antiga/fato novo:** convivência explicitamente preservada, anterior a C3. (`app/chamados/juizes.py:152`; `app/chamados/juizes.py:155`)

6. **Não executei `cef_c3_verificar`: ele escreve `cef_c3_mutantes.txt`**, apesar de não alterar fontes. O ensaio acima carregou e alterou apenas `c1.[nome]` em memória, mecanismo previsto no próprio executor. (`app/chamados/tests/cef_c3_verificar.py:36`; `app/chamados/tests/test_cef_c1_sem_banco.py:23`; `app/chamados/tests/test_cef_c1_sem_banco.py:26`)

## 15/11 05:37 · sessao b8538b8203fa · - · - · origem=Ronald (tmux)

A1 está correta para o defeito sequencial: somente dicionários com `id` conhecido igual fundem; chaves omitidas, inclusive `recebida_em`, sobrevivem; valor explícito novo vence e outro episódio substitui integralmente. (`app/chamados/services/disputa_emissao.py:569`; `app/chamados/services/disputa_emissao.py:573`; `app/chamados/services/disputa_emissao.py:575`)

O censo dos outros escritores dá estes limites:

- **Reuso via nota não apaga recepção já presente.** Escreve `ChamadoColaborador.contexto_json`, enquanto A1 escreve `DisputaSupervisao.contexto`; a nota delega à porta `fundir_contexto`, que trata dicionário como escalar e só o insere se a chave estiver ausente. Também **não acrescenta** recepção nova a `ultima_batida` já presente sem ela: permanece o snapshot antigo. Esse comportamento é preexistente e pode alimentar o fallback de dia pelo timestamp antigo. (`app/chamados/services/disputa_emissao.py:594`; `app/chamados/services/disputa_emissao.py:600`; `app/chamados/services/nota.py:110`; `app/chamados/services/nota.py:123`; `app/chamados/services/contexto.py:144`; `app/chamados/services/contexto.py:155`; `app/chamados/juizes.py:162`)
- **`contexto.py` não é outro escritor atual de `ultima_batida` da disputa.** Atua em `contexto_json` do chamado; `renovar_contexto` pode substituir qualquer chave recebida, mas os chamadores localizados enviam outras chaves. Exemplos: marcos, batida/data do turno, agrupamento, escape e sugestão. Acesso dinâmico fora do censo textual: **[nao verificado]**. (`app/chamados/services/contexto.py:97`; `app/chamados/services/contexto.py:100`; `app/chamados/services/contexto.py:102`; `app/chamados/management/commands/nomear_marco_por_lampada.py:109`; `app/chamados/management/commands/carimbar_por_batida_coincidente.py:98`; `app/chamados/services/adesao.py:113`; `app/chamados/services/declaracao_texto.py:208`; `app/chamados/services/limbo_folgas.py:68`)
- **Outras atribuições de `.contexto` preservam `ultima_batida` sequencialmente:** preenchimento de motivo/data parte de cópia; aparar remove a chave do motivo; remover acrescenta `revalidacoes`. A atribuição do envelope no gerador ocorre numa cópia transitória e não grava o agregado. (`app/chamados/services/disputa_emissao.py:291`; `app/chamados/services/disputa_emissao.py:296`; `app/chamados/services/disputa_emissao.py:312`; `app/chamados/models.py:2028`; `app/chamados/models.py:2032`; `app/chamados/models.py:2104`; `app/chamados/models.py:2109`; `app/chamados/models.py:2128`)
- **Portas genéricas da disputa substituem uma chave, mas os chamadores encontrados escrevem outras:** `supervisao_omissa_escalado`, `incidente_operacional`, `ultimo_alerta_diario`. Não demonstram outro caso sequencial do mesmo apagamento. (`app/chamados/models.py:2164`; `app/chamados/models.py:2182`; `app/chamados/views.py:1343`; `app/ponto/management/commands/escalonar_chamados_supervisao.py:159`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48`)

**Não cabe prometer “nunca perde recepção” para concorrência:** o cron pode carregar a disputa antes da recepção; a emissão salva `recebida_em`; depois `renovar_contexto` grava o JSON inteiro da instância antiga, com outra chave, apagando a recepção recém-conhecida. A porta de renovação não relê nem trava nessa implementação. É risco preexistente, distinto do defeito sequencial curado; ocorrência na sombra **[nao verificado]**. (`app/ponto/management/commands/alertar_disputas_pendentes.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:42`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48`; `app/chamados/models.py:2182`; `app/chamados/models.py:2187`; `app/chamados/services/disputa_emissao.py:539`; `app/chamados/services/disputa_emissao.py:579`)

Escritores dos envelopes: ambas APIs fornecem `id`, `tipo`, timestamp local do fato e recepção; retro fornece os três primeiros e omite recepção. O significado local de recepção não decide o dia no juiz fraco. (`app/api/views.py:761`; `app/api/views_core.py:755`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`; `app/chamados/juizes.py:151`; `app/chamados/juizes.py:169`)

_5532 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
