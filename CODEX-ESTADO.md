# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 08:42** · publicado em **19/11/2026 08:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 08:42 · sessao 952ae32577d4 · E4C-F5-CONSTROI · construtor · origem=Code

A implementação está na cópia, mas **a volta ainda não está comprovada**: não executei banco, suíte, REDs ou mutantes, conforme solicitado. A comparação de consultas 1 × N nos caminhos reais e o retrato do teto do modal permanecem [não verificados]. O teste de lote preparado exercita os dois leitores sobre um queryset pré-carregado, mas não mede a renderização real do fio e do modal ([teste:97](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:97)). [nao verificado]

Alterações:

- [fio.py:178](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/services/fio.py:178): lê o ato e entrega `LeituraDoAto` ao juiz existente `respondida_sem_texto`. O objeto fornece os dois campos que o juiz já aceita, preservando seu predicado ([juizes.py:1531](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/juizes.py:1531)).
- [chamados_tags.py:145](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/templatetags/chamados_tags.py:145): o atalho para resposta vazia usa o texto da leitura.
- [historico.py:96](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/services/historico.py:96): resposta, validação e texto vêm da leitura; materialização permanece na projeção. O teste existente de histórico verifica presença de eventos, mas não confronta ato e projeção ([test_historico.py:124](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/tests/test_historico.py:124)); acrescentei esse confronto com materialização projetada ([teste:83](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:83)). [nao verificado]
- [cobranca.py:236](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/services/cobranca.py:236) e [relatorios_disputa.py:11](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/relatorios_disputa.py:11): pré-carregam decisões nos outros caminhos em lista encontrados. O queryset do fio já as pré-carregava ([fio.py:260](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/services/fio.py:260)).
- [teste novo:1](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:1): contém confrontos nos dois sentidos, controle, passivo sem ato, histórico, comparação de consultas e selo AST com antivacuidade. Sua execução e capacidade de matar os mutantes estão [não verificadas]. [nao verificado]

No caminho de resposta, `responder_pergunta_disputa_core` escreve texto e instante ([disputa_emissao.py:1439](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/services/disputa_emissao.py:1439), [disputa_emissao.py:1481](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/services/disputa_emissao.py:1481)); a sequência é escrita por `DecisaoPergunta.registrar` ([models.py:4329](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/models.py:4329)). Os leitores alterados são fio, filtro e histórico nos links acima; a testemunha incorpora o passivo da projeção ([models.py:4295](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/models.py:4295)).

Comando GREEN para a cópia, a ser executado pelo certificador:

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir /home/ronald/wt-codex-e4c-f5-constroi-083923 --only "chamados.tests.test_e4c_f5_volta1_leem_o_ato chamados.tests.test_historico chamados.tests.test_selo_teto_modal"
```

Para o RED, o mesmo comando precisa ser executado com o teste novo presente e os leitores de produção na base `3308f759…`; esse retrato não foi executado [não verificado]. O teto declarado atual é **2837**, mas não há medida antes/depois nesta volta ([test_selo_teto_modal.py:184](/home/ronald/wt-codex-e4c-f5-constroi-083923/app/chamados/tests/test_selo_teto_modal.py:184)).

Os mutantes preparados substituem a leitura pela própria projeção durante cada caso. Os comandos abaixo são para um ambiente de teste já configurado; `suite.sh` não repassa automaticamente essa variável ao container ([suite.sh:319](/home/ronald/wt-codex-e4c-f5-constroi-083923/bin/suite.sh:319)). Morte de todos: [não verificada]. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 E4C_F5_MUTANTE=fio python3 manage.py test chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_fio_ato_responde_e_projecao_pendente
PYTHONDONTWRITEBYTECODE=1 E4C_F5_MUTANTE=tag python3 manage.py test chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_tag_ato_tem_texto_e_projecao_vazia
PYTHONDONTWRITEBYTECODE=1 E4C_F5_MUTANTE=historico python3 manage.py test chamados.tests.test_e4c_f5_volta1_leem_o_ato.LeitoresTest.test_red_5_historico_resposta_validacao_texto_e_materializacao
```

A sintaxe dos arquivos alterados foi verificada por AST e `git diff --check` não apontou erros de whitespace [não verificado por suíte]. As medidas de sombra e CHECK-DE-TELA permanecem [não verificadas]. [nao verificado]

LEI-AKITA: origem=S-fio e S-tag, testemunha=DecisaoPergunta.leitura, RED=chamados/tests/test_e4c_f5_volta1_leem_o_ato.py, quem-mais-le=classes_v5b.tsv (447) [não verificado], juizes novos=0, padrao=um juiz por pergunta.

## 19/11 08:39 · sessao 841e89919e0c · - · - · origem=Ronald (tmux)

**PAREI: a cópia de trabalho deixou de estar acessível durante a execução.** A última tentativa de acessar `/home/ronald/wt-codex-e4c-f5-083733/app` retornou “No such file or directory”; a causa está [não verificado]. Não consegui concluir nem verificar a entrega.

Antes da perda de acesso, apliquei alterações em `chamados/services/fio.py:188`, `chamados/templatetags/chamados_tags.py:166`, `chamados/relatorios_disputa.py:42`, `chamados/services/acoes_chamado.py:352` e `chamados/services/historico.py:96`. **A persistência dessas alterações está [não verificado].** A criação do arquivo de teste foi rejeitada quando o diretório já estava inacessível; sua existência está [não verificado].

O levantamento anterior à perda identificou:

- O fio carrega `decisoes` em lote no queryset das perguntas (`chamados/services/fio.py:258`), mas `aguarda_o_colaborador` também é chamado pelo relatório, cujo queryset não as carregava (`chamados/relatorios_disputa.py:23`, `chamados/relatorios_disputa.py:42`).
- O detalhe do chamado também renderiza o filtro por pergunta (`chamados/services/acoes_chamado.py:352`, `templates/chamados/detalhe_local.html:309`, `templates/chamados/partials/_pergunta_card.html:43`).
- O teste existente de histórico verifica eventos com carimbos projetados, sem confrontar ato e projeção para resposta, validação e texto (`chamados/tests/test_historico.py:125`). O caso adicional com materialização projetada ficou pendente.
- O predicado existente `respondida_sem_texto` aceita um objeto com `respondida_em` e `resposta_colab` (`chamados/juizes.py:1531`); a alteração pretendida lhe passava `LeituraDoAto`, preservando o mesmo predicado (`chamados/models.py:19`).

RED, GREEN, mutantes, selo AST, medida de consultas 1 × N e diff final: **[não verificado]**. Não rodei banco nem suíte. Não há comandos de mutantes entregáveis enquanto o teste não estiver disponível. [nao verificado]

LEI-AKITA: origem=S-fio e S-tag, testemunha=DecisaoPergunta.leitura, RED=chamados/tests/test_e4c_f5_volta1_leem_o_ato.py [não verificado], quem-mais-le=classes_v5b.tsv (447) [não verificado], juizes novos=0 [não verificado], padrao=um juiz por pergunta.

## 19/11 08:33 · sessao 2829f923765b · ESCALA-NAO-DESCREVE · verificador · origem=Code

**BLOQUEIA.** A catraca de linhas diverge nesta cópia: o registro aponta a linha 58 do detector, mas o censo encontra a chamada na linha 61. O teste compara os dicionários por igualdade e, portanto, fica vermelho. ([test_medida_turno_do_par.py:277](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/core/tests/test_medida_turno_do_par.py:277), [detector_proposta.py:61](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/escala/detector_proposta.py:61), [test_medida_turno_do_par.py:558](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/core/tests/test_medida_turno_do_par.py:558))

1. **A catraca não bate.** Executei `censo('app')`, em leitura, e comparei todos os itens de `LINHAS_REGISTRADAS`: os 9 itens de `pareadores` coincidem; dos 85 itens de `derivacoes_de_dia`, apenas `medir_padrao` diverge, com `[58]` registrado e `[61]` retornado. O censo publica as linhas AST e o teste exige igualdade exata. ([medida_turno_do_par.py:399](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/core/medida_turno_do_par.py:399), [test_medida_turno_do_par.py:247](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/core/tests/test_medida_turno_do_par.py:247), [test_medida_turno_do_par.py:277](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/core/tests/test_medida_turno_do_par.py:277), [detector_proposta.py:61](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/escala/detector_proposta.py:61))

   Conferi também os quatro registros deslocados no delta: `espelho.py` **1029 e 426**, e `turnos.py` **2967 e 2945**; os sítios correspondentes estão nessas linhas. ([test_medida_turno_do_par.py:324](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/core/tests/test_medida_turno_do_par.py:324), [espelho.py:1029](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/services/espelho.py:1029), [espelho.py:426](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/services/espelho.py:426), [test_medida_turno_do_par.py:336](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/core/tests/test_medida_turno_do_par.py:336), [turnos.py:2967](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/turnos.py:2967), [turnos.py:2945](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/turnos.py:2945))

2. **Dependência da ESPURIA-TELA: não encontrei referência executável aos dois arquivos nomeados no escopo pesquisado.** Há uma menção a `templates/ponto/espelho.html` em docstring de teste; o corpo mostrado verifica valores produzidos por `_cels`, sem carregar esse template. ([test_t4_lei_lampada_delta.py:54](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/escala/tests/test_t4_lei_lampada_delta.py:54)) A ausência de dependência de **qualquer outro arquivo criado pela ESPURIA** fica **[não verificado]**: o material fornecido não lista todos eles e a cópia não tem histórico Git.

3. **Os textos identificam corretamente o caminho do vínculo, mas omitem a escrita anterior da proposta.** A view é `aplicar_proposta_da_lista`: ela lê a proposta da lavra, confere o predicado e chama `realocar_vinculo_para_destino`; esta chama `executar_vinculo`. O botão envia um POST para essa view. ([views.py:476](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/escala/views.py:476), [views.py:493](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/escala/views.py:493), [views.py:502](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/escala/views.py:502), [regua_defesa.py:324](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/escala/regua_defesa.py:324), [cadastro_x_realidade.html:32](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/templates/escala/cadastro_x_realidade.html:32))

   Antes do clique, `anomalias_da_frota` inclui campos da proposta no retrato; `lavrar` grava esse retrato no banco e tenta gravá-lo também em arquivo. Assim, o clique é o caminho descrito para alterar o vínculo, mas não é a primeira persistência de dados derivados da proposta. ([esmeril_espelho.py:241](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/services/esmeril_espelho.py:241), [esmeril_espelho.py:321](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/services/esmeril_espelho.py:321), [esmeril_espelho.py:349](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/services/esmeril_espelho.py:349))

4. **Não.** O selo usa a entrada literal `"escala/detector_proposta.py"` no conjunto `ALLOWLIST`, compara conjuntos de caminhos e examina AST dos arquivos de produção; o comentário alterado não é um critério dessa comparação. Alterar somente seu texto não muda o resultado do selo. ([test_contract_no_batida_date.py:18](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/tests/test_contract_no_batida_date.py:18), [test_contract_no_batida_date.py:213](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/tests/test_contract_no_batida_date.py:213), [test_contract_no_batida_date.py:239](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/tests/test_contract_no_batida_date.py:239), [test_contract_no_batida_date.py:268](/home/ronald/codex_ws/8a4697c0ee8afe92bea5ad2ba24385ee2ee5f082/app/ponto/tests/test_contract_no_batida_date.py:268))

## 19/11 08:33 · sessao 08c3ea761f2c · E4C-F5 · censo · origem=Code

# BRIEF C3 — E4C-F5

**Situação: ETAPA A não fechada; ETAPA B — AVAL-DEVOLVIDO.** Este é o texto para revisão, entregue em modo somente leitura; o arquivo de destino não foi criado. A matriz identifica os 15 sítios e as provas exigidas, mas a aprovação do censo pelo wrapper, os resultados dos REDs, a cobertura das guardas em todas as portas e as medidas de sombra permanecem [não verificado].

Base declarada no pedido: `3308f759942527fa084b6137304860d24c6f414d`, main com F6F7 e F9. A identidade Git desta cópia com o hash está [não verificado]. As linhas de código abaixo foram abertas nesta cópia; caminhos relativos partem de `app/`.

O aval e o censo foram fornecidos como anexos raspados no pedido; sua existência e conteúdo em arquivos da cópia estão [não verificado]. Os resultados de produção, inclusive as classes de 40 e 213 perguntas, estão [não verificado].

## 1. Testemunha e fronteira

**Testemunha:** `DecisaoPergunta.leitura(p)` devolve `LeituraDoAto(validada_em, respondida_em, resposta_colab)`; a leitura combina a sequência de decisões com o passivo da projeção sem linha correspondente (`chamados/models.py:19`, `chamados/models.py:4295`, `chamados/models.py:4308`, `chamados/models.py:4326`).

`materializada_em`, `recusa_motivo` e `validada_por` continuam na projeção: não fazem parte dos três campos de `LeituraDoAto` (`chamados/models.py:19`). A instrução de mantê-los ali até a E4d consta do aval raspado; a existência ou inexistência de outro ato C em toda a base está [não verificado].

Os juízes existentes continuam sendo as autoridades; não nasce outro juiz. `JUIZES.md` é tabela de consulta e aponta o registro vivo, cabendo conferir o código quando houver divergência (`docs/JUIZES.md:7`, `docs/JUIZES.md:14`). O juiz de resposta exige instante e texto não vazio (`chamados/juizes.py:1544`, `chamados/juizes.py:1552`).

**LEI-AKITA:** origem = chamados, 15 sítios abaixo; testemunha = `DecisaoPergunta.leitura`; RED = matriz e provas do §4; quem mais lê = censo raspado `classes_v5b.tsv` com 447 entradas, cuja conferência independente integral nesta cópia está [não verificado]; juízes novos = 0; padrão = um juiz por pergunta (`docs/JUIZES.md:10`).

## 2. Contagem e matriz por sítio

A contagem é **15 sítios**. As linhas `materializacao.py:199` e `:201` contam como **dois**: são duas expressões de leitura em condições distintas, a primeira condicionada também por `irma_validada`, a segunda usada para testar implausibilidade. Uma mutação em uma não substitui a prova da outra (`chamados/services/materializacao.py:199`, `chamados/services/materializacao.py:201`). Assim, esta contagem coincide com as 15 posições do aval raspado; a correspondência integral entre as 19 entradas de classe i do anexo e os sítios está [não verificado].

Convenções da tabela:

- **V** = `validada_em`; **R** = `respondida_em`; **T** = `resposta_colab`; **M** = `materializada_em`; **RM** = `recusa_motivo`; **VP** = `validada_por`.
- “Ato X / projeção Y” indica a fixture de divergência a construir. A leitura efetiva inclui o passivo sem linha correspondente; a fixture deve controlar os instantes e a correspondência para que X seja de fato o resultado de `DecisaoPergunta.leitura` (`chamados/models.py:4238`, `chamados/models.py:4295`).
- **RED proposto** significa teste ainda não executado. Se o teste já passar na base, registrar esse resultado e exigir o mutante que morre; não chamar de vermelho um teste que já passa. Situação RED/GREEN e morte dos mutantes: [não verificado]. [nao verificado]
- Na coluna “não migra”, M, RM e VP ficam na projeção quando presentes; um travessão significa que nenhum desses campos é lido na expressão citada. Isso não dispensa preservar os demais usos da projeção na função.

| Arquivo:linha; função | Campo; hoje lê → passa a ler | Campo que não migra e por quê | Decide escrita? Porta | Laço/lista e chamador | RED que morde; mutante | Volta |
|---|---|---|---|---|---|---|
| `chamados/juizes.py:1389`; `projecao_temporal_da_pergunta` | V/R/T da pergunta → os três da leitura, preservando horizonte | M e via continuam na projeção: não estão na testemunha (`chamados/juizes.py:1390`) | Não diretamente | Chamado por `_pergunta_viva_calc` e outro leitor (`chamados/juizes.py:351`, `chamados/juizes.py:1404`); outros laços [não verificado] | Ato respondido/validado, projeção divergente; comparar tupla no horizonte. Mutante volta aos três campos crus; outro remove o corte | 2 |
| `chamados/juizes.py:1552`; `pergunta_respondida` | R/T do argumento → leitura do ato quando o argumento for pergunta | — | Não diretamente; é chamado por portas | Chamadores incluem listas em `fio.py:708` e `:1283`; também recebe objeto projetado, não apenas pergunta (`chamados/juizes.py:1423`) | Ato com resposta válida / projeção pendente e inverso; mutante lê pergunta crua. Preservar contrato para `SimpleNamespace` e `LeituraDoAto` [implementação não verificada] | 2 |
| `chamados/services/acoes_disputa.py:349`; `acao_corrigir_resposta` | V da projeção → V da leitura | — | Sim: correção por `responder_pergunta_disputa_core`, seguida de `validar_pergunta` (`chamados/services/acoes_disputa.py:374`, `:383`) | Requisição individual; outros chamadores em lote [não verificado] | Ato validado / projeção sem V: bloquear correção; inverso: não bloquear por V cru. Mutante restaura `perg.validada_em` | 3 |
| `chamados/services/declaracao_texto.py:228`; `_resposta_validada` | V da projeção → V da leitura | — | Sim indiretamente: guarda das declarações; resposta pela porta canônica e validação (`chamados/services/declaracao_texto.py:379`, `:420`, `:669`, `:697`) | Também chamado pela testemunha de tela (`chamados/services/declaracao_texto.py:283`); laço completo [não verificado] | Ato validado / projeção sem V: recusar declaração; inverso: não recusar por V cru. Mutante restaura V cru | 3 |
| `chamados/services/disputa_emissao.py:2083`; `_resolver_perguntas_por_chamado_resolvido` | M e V da projeção → M da projeção e V da leitura | M: não existe na testemunha | Sim: seleciona abertas para `q.carimbar` (`chamados/services/disputa_emissao.py:2082`, `:2103`) | Compreensão sobre `_todas`, dentro de `_cands` (`chamados/services/disputa_emissao.py:2072`, `:2078`) | Ato validado / projeção sem V e M nula: não selecionar; inverso: selecionar se demais guardas permitirem. Mutante restaura V cru | 3 |
| `chamados/services/fio.py:188`; `espera_o_texto_do_colaborador` | R e predicado sobre R/T da projeção → leitura de R/T, preservando o predicado | — | Não: aviso/cartão (`chamados/services/fio.py:164`, `:171`) | Lista em `aguarda_o_colaborador` e laço de cartões (`chamados/services/fio.py:175`, `:356`) | Ato respondido / projeção pendente: não esperar; inverso: esperar. Mutante lê R/T crus | 1 |
| `chamados/services/materializacao.py:199`; `par_do_intervalo` | V da irmã na projeção → V da leitura da irmã | — | Sim indiretamente: guarda da irmã já validada no plantio (`chamados/services/materializacao.py:181`, `:1732`) | Chamado também pela tela e validação (`chamados/services/materializacao.py:923`, `chamados/services/validacao.py:632`) | Com `irma_validada=True`, ato sem V / projeção com V: impedir puxar irmã; inverso: não impedir por V cru. Mutante restaura condição da linha 199 | 3 |
| `chamados/services/materializacao.py:201`; `par_do_intervalo` | V da irmã na projeção → V da leitura da irmã | — | Sim indiretamente: muda a guarda de implausibilidade usada por validação e plantio (`chamados/services/materializacao.py:181`, `:201`) | Mesmos três caminhos da linha anterior | Ato sem V / projeção com V e irmã implausível: executar guarda; inverso: não executá-la por V cru. Mutante restaura condição da linha 201 | 3 |
| `chamados/services/materializacao.py:2123`; `materializar_perguntas_validadas_da_disputa` | V relida da projeção → V da leitura após trava | M continua na projeção, inclusive na guarda anterior (`chamados/services/materializacao.py:2120`) | Sim: `_materializar_uma` (`chamados/services/materializacao.py:2125`) | Laço sobre `perguntas_prontas`, com releitura sob transação (`chamados/services/materializacao.py:2111`, `:2117`) | Após seleção e releitura, ato sem V / projeção com V: não materializar. Mutante restaura V cru. O filtro SQL anterior também usa V projetada; alcance da fixture para ato com V / projeção sem V [não verificado] (`chamados/services/materializacao.py:2104`) | 3 |
| `chamados/services/validacao.py:640`; `_validar_o_par` | V da irmã na projeção → V da leitura | M continua como prova posterior de materialização (`chamados/services/validacao.py:643`) | Sim: `_carimbar_validacao` da irmã e `_materializar_uma` (`chamados/services/validacao.py:640`) | Chamado por `validar_pergunta` (`chamados/services/validacao.py:771`); alcance em lote via porta de validação (`chamados/services/validacao.py:1078`) | Ato da irmã validado / projeção sem V: não carimbar segunda validação; inverso: não pular por V cru. Mutante restaura V cru | 3 |
| `chamados/services/validacao.py:685`; `validar_pergunta` | V da projeção → V da leitura | M continua na guarda de materialização (`chamados/services/validacao.py:769`) | Sim: `_carimbar_validacao` e materialização (`chamados/services/validacao.py:740`, `:743`, `:775`) | Individual e chamada pelo lote via ação (`chamados/services/validacao.py:1078`, `chamados/services/acoes_disputa.py:13`) | Ato validado / projeção sem V: retornar já validada sem escrever; inverso: não barrar por V cru. Mutante restaura V cru | 3 |
| `chamados/services/validacao.py:898`; `recusar_pergunta` | V da projeção → V da leitura | RM permanece na projeção; é gravado pela porta (`chamados/services/validacao.py:916`, `:922`) | Sim: grava motivo e chama `reabrir_para_correcao` (`chamados/services/validacao.py:920`) | Porta individual; outros usos em lote [não verificado] | Ato validado / projeção sem V: não recusar; inverso: não barrar por V cru, respeitadas as demais guardas. Mutante restaura V cru | 3 |
| `chamados/services/validacao.py:1064`; `validar_lote_classe_a` | V da projeção → V da leitura | — | Sim indiretamente: chama ação de validar (`chamados/services/validacao.py:1078`) | Laço sobre `refs` (`chamados/services/validacao.py:1056`) | Ato validado / projeção sem V: rejeitar item antes da ação; inverso: não rejeitar por V cru. Mutante restaura V cru | 3 |
| `chamados/templatetags/chamados_tags.py:166`; `leitura_da_resposta` | T da projeção → T da leitura | — | Não: filtro de tela (`chamados/templatetags/chamados_tags.py:145`) | Uso em cartões/templates e limite de consultas exigem levantamento completo [não verificado]; o próprio filtro documenta custo multiplicado por cartão (`chamados/templatetags/chamados_tags.py:148`) | Ato com texto / projeção vazia: não sair pelo atalho; inverso: sair. Mutante restaura `getattr` cru | 1 |
| `chamados/services/validacao.py:262`; `devolver_validacao_recusada` | `refresh_from_db` traz V/VP/M/RM; condição decisória V em `:269` → V da leitura após refresh | M, RM e VP continuam projetados: são carimbo, motivo e autor, ausentes da testemunha (`chamados/services/validacao.py:262`, `:264`, `:266`) | Sim: limpa V/VP e registra DEVOLUÇÃO (`chamados/services/validacao.py:269`, `:276`, `:279`) | Chamada dentro de `validar_pergunta` (`chamados/services/validacao.py:781`); chamada direta também é possível (`chamados/services/validacao.py:237`) | Ato sem V / projeção com V, com recusa e sem M: não devolver por V cru; inverso: provar a decisão e efeito conforme a porta. Mutante restaura condição V crua | 3 |

**Histórico / RED-5 permanece, fora da contagem dos 15.** `_eventos_dos_chamados` percorre perguntas e lê R, V e M por nomes em tupla, além de T (`chamados/services/historico.py:96`, `:99`, `:101`, `:103`, `:105`, `:109`). Na volta 1, o RED-5 deve confrontar ato e projeção para eventos de resposta/validação e texto, mantendo M na projeção; seu mutante deve restaurar as leituras cruas correspondentes. A semântica de eventos anteriores versus leitura vigente deve ser comprovada pelo RED existente, cujo conteúdo não foi fornecido nem localizado nesta conferência [não verificado]. Não somar o histórico aos 15 sítios de classe i.

**Lote:** não fazer uma consulta por pergunta. `tarefa_do_dia` recebe `carga`, enquanto `_tarefa_pura` recebe `leituras`; a carga monta o mapa e o repassa (`chamados/services/tarefa_do_dia.py:425`, `:673`, `:677`, `:825`). Esses kwargs não provam que cada chamador da matriz já tenha carga adequada. Para cada lista, demonstrar carregamento das decisões em lote e RED de teto de consultas com fixtures de tamanhos crescentes; desenho e teto de cada caminho [não verificado]. `leitura` aproveita decisões pré-carregadas (`chamados/models.py:4304`).

## 3. Volta 3: escritores, leitores e competência

O campo julgado nos sítios da volta 3 é V. Um escritor de V e do ato de validação é `_carimbar_validacao` (`chamados/services/validacao.py:161`, `:168`, `:173`); há outros escritores, portanto não atribuir exclusividade global do campo a ele (`docs/JUIZES.md:38`). Os leitores são as posições V das linhas da matriz; em `devolver_validacao_recusada`, a leitura decisória está em `validacao.py:269`, após o refresh de `:262`.

| Sítio(s) da volta 3 | Quem escreve no caminho; quem lê | Guarda de entregue/exportada identificada |
|---|---|---|
| `acoes_disputa.py:349` | A condição lê V; a correção chama `responder_pergunta_disputa_core` e depois `validar_pergunta` (`chamados/services/acoes_disputa.py:349`, `:374`, `:383`) | A validação tem guarda K6 no trecho normal (`chamados/services/validacao.py:740`); guarda antes da própria correção de resposta [não verificado] |
| `declaracao_texto.py:228` | A guarda lê V; `responder_pergunta_disputa_core` escreve resposta e a porta de admin chama `validar_pergunta` (`chamados/services/declaracao_texto.py:228`, `:420`, `:694`, `:697`) | K6 no trecho normal da validação (`chamados/services/validacao.py:740`); cobertura antes de cada escrita das duas declarações [não verificado] |
| `disputa_emissao.py:2083` | A compreensão lê V/M; `q.carimbar` escreve o carimbo pela via (`chamados/services/disputa_emissao.py:2083`, `:2103`) | Existe cerca no chamador (`chamados/services/disputa_emissao.py:1919`); sua cobertura específica de entregue/exportada para este ramo [não verificado] |
| `materializacao.py:199` e `:201` | Ambas leem V da irmã; a validação pode escrever V pela porta em `validacao.py:641`, e o plantio chama `_materializar_uma` para a irmã (`chamados/services/materializacao.py:1732`, `:1744`) | `_materializar_uma` chama `_competencia_que_barra(..., somente_entregue=True)` antes das escritas (`chamados/services/materializacao.py:1866`); K6 consulta especificamente metades de intervalo (`chamados/services/materializacao.py:804`, `:811`) |
| `materializacao.py:2123` | Lê V após releitura; `_materializar_uma` escreve efeitos da materialização (`chamados/services/materializacao.py:2117`, `:2123`, `:2125`) | A guarda de `:1866` existe, mas K6 nessa composição consulta metades de intervalo; proteção de todos os demais tipos e de toda competência exportada [não verificado] (`chamados/services/materializacao.py:804`, `:831`) |
| `validacao.py:640` | Lê V da irmã; `_carimbar_validacao` escreve V e ato; `_materializar_uma` pode materializar (`chamados/services/validacao.py:640`, `:642`) | No caminho normal de `validar_pergunta`, `entrega_que_barra_a_validacao` é consultada antes do carimbo e novamente após materialização (`chamados/services/validacao.py:740`, `:777`); cobertura de todos os chamadores [não verificado] |
| `validacao.py:685` e `:1064` | Leem V; o lote chama a ação e a validação escreve por `_carimbar_validacao` (`chamados/services/validacao.py:685`, `:743`, `:1064`, `:1078`) | Guarda K6 do trecho normal em `:740`; há desvio anterior para aceite de folga (`chamados/services/validacao.py:711`). Cobertura de todos os tipos [não verificado] |
| `validacao.py:898` | Lê V; a própria porta grava RM e chama `reabrir_para_correcao` (`chamados/services/validacao.py:898`, `:920`, `:925`) | Guarda de entregue/exportada nesse caminho [não verificado] |
| `validacao.py:262` e `:269` | Lê V após refresh; a própria função limpa V/VP e registra DEVOLUÇÃO (`chamados/services/validacao.py:269`, `:276`, `:279`) | No caminho de `validar_pergunta`, há nova verificação de entrega antes da chamada (`chamados/services/validacao.py:777`, `:781`); chamada direta e outros caminhos [não verificado] |

`competencia_do_fato` consulta `competencia_entregue` pelo dia do fato (`chamados/services/materializacao.py:788`, `:798`). A guarda `_k6_que_barra` pula perguntas que não sejam metades de intervalo (`chamados/services/materializacao.py:804`, `:818`). Portanto, a presença de `materializacao.py:1866` não demonstra proteção universal. Para cada porta, medir e listar as perguntas cuja decisão mudaria e identificar entregue/exportada; qualquer movimento proibido pelo aval impede a volta. Resultado da medida [não verificado].

## 4. REDs, mutantes e conservação

1. **Passivo sem ato, juiz temporal:** criar perguntas sem decisões com R/T/V legais na projeção e comparar a tupla antes/depois, nos dois lados do horizonte. `linhas_da_projecao_sem_linha` gera as linhas virtuais para carimbos sem correspondência, e `leitura` as incorpora (`chamados/models.py:4238`, `:4251`, `:4260`, `:4265`, `:4312`). Mutante: omitir o passivo. RED e morte do mutante [não verificado].
2. **Passivo sem ato, juiz de respondida:** com instante e texto não vazio, conservar `True`; com ambos ausentes, conservar `False`. Mutante: omitir passivo. A leitura precisa respeitar que o juiz também aceita objetos já projetados (`chamados/juizes.py:1423`, `:1544`). RED e mutante [não verificado].
3. **Conservação não é identidade de dados:** texto sem R não produz linha virtual de resposta; o par é estado ilegal. Com R presente e T `None`, a linha virtual usa `''` (`chamados/models.py:4251`, `:4260`, `:4263`, `chamados/juizes.py:1537`). Provar separadamente texto sem carimbo, instante sem texto e `None → ''`; não afirmar igualdade literal da tupla nesses casos. REDs [não verificado].
4. **Corte temporal:** o primeiro juiz zera V futura, zera R e T quando R é futura e corta M futura, com regra própria para a via (`chamados/juizes.py:1391`). A leitura comum não recebe relógio (`chamados/models.py:4304`). Provar antes, no instante e depois de cada carimbo, inclusive sequência de atos em lados diferentes do horizonte; mutante remove o corte. A adequação do mecanismo existente de vista temporal ao juiz após a mudança está [não verificado]; há uma vista usada pela carga da tarefa (`chamados/services/tarefa_do_dia.py:390`, `:402`).
5. **RED-5 histórico:** manter o teste descrito no §2 e seu mutante; conteúdo e resultado do RED original [não verificado] (`chamados/services/historico.py:96`).
6. **Por sítio:** executar os confrontos e mutantes da matriz, incluindo ambos os ramos de `par_do_intervalo`. Para teste que já passa, registrar passagem na base e provar que o mutante morre; para teste que falha, registrar falha pertinente antes da mudança. Resultados [não verificado]. [nao verificado]

A função de passivo também pode acrescentar projeção sem correspondência quando **já existem outros atos**; “sem ato” é um caso de conservação, não a definição completa da função (`chamados/models.py:4244`, `:4249`).

## 5. SILÊNCIO

O aval raspado estabelece que, em competência aberta, o dono passe a ADMIN. A prova de produção dessa classe e sua contagem estão [não verificado]. A expectativa existente compara a tarefa antes e depois da ressuscitação e espera a razão `silencio_decidido`; as linhas 330 e 334 são, respectivamente, criação do chamado e mock de relógio, não um `assert` explícito do dono (`chamados/tests/test_f1nova_b1_juiz_respondida.py:328`, `:330`, `:334`, `:338`, `:343`). Reescrever a expectativa pertinente e testar explicitamente o dono no caminho real da tarefa; resultado [não verificado].

## 6. Medidas obrigatórias por volta

Em cada volta, 

[truncado em 20000 de 23617 caracteres]

## 19/11 08:24 · sessao 5acfc6496c4a · DST-L2-JZ1 · verificador · origem=Code

**NÃO BLOQUEIA**

1. **Os rótulos são aritmeticamente coerentes com as medidas c3 e c5 citadas na mesma linha:** 227 → 228 → 230 e 291 → 292 → 294. As linhas também citam medições anteriores de outras obras, em marcos distintos; elas, por si só, não demonstram contradição com os valores atribuídos ao main. A medição direta de 227/291 no main e os logs citados permanecem **[não verificado]**. [test_selo_chokepoint_escrita.py:196](/home/ronald/codex_ws/1a42ef455f7c18bd130b388d89f43b43404c8cf6/app/ponto/tests/test_selo_chokepoint_escrita.py:196), [test_selo_chokepoint_escrita.py:210](/home/ronald/codex_ws/1a42ef455f7c18bd130b388d89f43b43404c8cf6/app/ponto/tests/test_selo_chokepoint_escrita.py:210).

2. **Sim.** Ambos falham se a função injetada não for chamada: `_chamou` começa vazio, é preenchido pela injeção e é exigido pelo `assertTrue`. Ainda existe uma limitação: eles não conferem os argumentos da chamada nem os estados imediatamente antes da exceção; uma chamada indevida da função injetada, sem ter fechado e rejeitado os objetos esperados, poderia satisfazer a marca e deixar os estados finais aparentemente corretos. O código atual chama a ocorrência após o fecho e a rejeição nos dois caminhos. [test_dst_l2_jz1_c3_lote_portao.py:125](/home/ronald/codex_ws/1a42ef455f7c18bd130b388d89f43b43404c8cf6/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:125), [test_dst_l2_jz1_c3_lote_portao.py:285](/home/ronald/codex_ws/1a42ef455f7c18bd130b388d89f43b43404c8cf6/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:285), [cartorio.py:1033](/home/ronald/codex_ws/1a42ef455f7c18bd130b388d89f43b43404c8cf6/app/ponto/services/cartorio.py:1033), [cartorio.py:1580](/home/ronald/codex_ws/1a42ef455f7c18bd130b388d89f43b43404c8cf6/app/ponto/services/cartorio.py:1580).

3. **Pelo `diff --stat` fornecido, nenhum dos dois arquivos foi tocado pela obra.** Na cópia, a rejeição em lote do DP faz `get` sem `select_for_update`, enquanto a aprovação em lote usa trava; a regularização faz o `save` dentro do `atomic`. Assim, esses trechos já estavam na base comparada, desde que o `diff --stat` anexo esteja correto; a comparação Git independente é **[não verificado]**. [evidencia.txt, diff --stat fornecido, sem numeração de linhas] [views.py:1875](/home/ronald/codex_ws/1a42ef455f7c18bd130b388d89f43b43404c8cf6/app/ponto/views.py:1875), [views.py:1795](/home/ronald/codex_ws/1a42ef455f7c18bd130b388d89f43b43404c8cf6/app/ponto/views.py:1795), [regularizacao.py:89](/home/ronald/codex_ws/1a42ef455f7c18bd130b388d89f43b43404c8cf6/app/ponto/services/regularizacao.py:89).

4. **Sim.** O aval fornecido aceita expressamente não gravar `atualizado_em` e o custo de o contador “hoje” do DP não contar a auto-rejeição; também manda medir o delta de SAVEPOINT antes e depois e subir o teto exatamente por esse delta, com a causa ao lado. [evidencia.txt, aval literal MANHA-1911-0810, item 1 fornecido, sem numeração de linhas].

## 19/11 08:22 · sessao fd85da5b6faf · DST-L2-JZ1 · verificador · origem=Code

**BLOQUEIA** — o novo rótulo não corresponde às medidas citadas na própria causa: ele soma novamente o `+1` de c3, embora o ponto de partida declarado para c5 já o inclua ([teste:196](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/tests/test_selo_chokepoint_escrita.py:196), [teste:210](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/tests/test_selo_chokepoint_escrita.py:210)).

1. No `delta.diff` fornecido, a única mudança são os comentários das duas linhas de teto; os números permanecem **230 e 294**, respectivamente ([teste:196](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/tests/test_selo_chokepoint_escrita.py:196), [teste:210](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/tests/test_selo_chokepoint_escrita.py:210)).

2. **O rótulo é contraditório.** Em cada linha, ele diz `+3 = +1 c3 +2 c5`, mas a mesma causa diz que c3 já está no medido de partida: **228**, seguido de **228→230**, e **292**, seguido de **292→294**. Assim, sobre os pontos de partida citados, o acréscimo até o teto é `+2`, não `+3` ([teste:196](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/tests/test_selo_chokepoint_escrita.py:196), [teste:210](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/tests/test_selo_chokepoint_escrita.py:210)). A existência de uma base anterior com os valores 227 e 291 e a perda no merge **[não verificado]**.

3. Quanto aos itens descartados, **NÃO PROCEDE** abaixo significa que o motivo apresentado não fica comprovado integralmente pelo material disponível, e não necessariamente que sua conclusão seja falsa:

   - **Lote do DP — NÃO PROCEDE integralmente.** A leitura sem trava e o `save()` completo conferem; a rejeição automática grava `status` e `descricao`, sustentando a possibilidade de sobrescrita da descrição por uma instância lida antes ([views.py:1875](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/views.py:1875), [justificativa.py:332](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/services/justificativa.py:332)). Que seja anterior e esteja fora do diff histórico citado **[não verificado]**; o delta fornecido cobre somente a volta 2.

   - **`atualizado_em` — NÃO PROCEDE integralmente.** O código confirma que a auto-rejeição salva apenas `status` e `descricao`; o comentário relata o custo e o aval ([justificativa.py:270](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/services/justificativa.py:270), [justificativa.py:332](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/services/justificativa.py:332)). O texto literal do aval e a aceitação independente do custo **[não verificado]**.

   - **Ordem de trava na regularização — NÃO PROCEDE integralmente.** A cópia mostra o `save` da justificativa dentro do `atomic`, o limite documentado e o `select_for_update` do lote ([regularizacao.py:89](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/services/regularizacao.py:89), [justificativa.py:101](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/services/justificativa.py:101), [justificativa.py:314](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/services/justificativa.py:314)). A afirmação histórica de que a inversão já existia antes de c5 **[não verificado]**.

   - **Testes `C5_UM_ATO` — NÃO PROCEDE como resposta ao achado.** Os dois testes instalam a função que lança exceção, capturam `RuntimeError` e verificam o estado, mas não afirmam que a função injetada foi chamada ([teste:125](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:125), [teste:281](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:281)). O resultado alegado dos mutantes **[não verificado]**; mesmo confirmado, não acrescentaria essa afirmação explícita aos testes.

   - **Teto versus proibição de editar o selo — NÃO PROCEDE integralmente.** O delta fornecido altera somente os comentários dessas duas linhas, e o comentário da cópia atribui ao aval a instrução de subir o teto com a causa ao lado ([teste:196](/home/ronald/codex_ws/4f5a390a4cbc1c29e7ee24a3ef0532798e434ec0/app/ponto/tests/test_selo_chokepoint_escrita.py:196)). O texto completo do aval, sua relação com a proibição e a autorização para a edição **[não verificado]**.

## 19/11 08:19 · sessao 0599a373aa54 · DST-L2-JZ1 · verificador · origem=Code

**Parecer: BLOQUEIA.** Há duas divergências concretas: os tetos sobem 3 quando a causa registra delta medido de 2, e existe caminho que grava/trava `Justificativa` antes de alcançar travas de `Chamado`. Também há um segundo escritor de decisão em lote fora da porta travada, capaz de concorrer com a auto-rejeição. [app/ponto/tests/test_selo_chokepoint_escrita.py:196] [app/ponto/tests/test_selo_chokepoint_escrita.py:210] [app/ponto/services/regularizacao.py:89] [app/ponto/signals.py:161] [app/ponto/views.py:1875]

A identidade Git da cópia **[não verificado]**: o diretório disponibilizado não é um repositório Git; a comparação do delta usa o anexo fornecido e o código disponível. Execução de testes, mutantes, medições SQL e dados de produção **[não verificado]**. [nao verificado]

1. **Literal do item 1 — BLOQUEIA.** Os dois blocos envolvem fecho, rejeição e ocorrência em `atomic`; o lote usa `select_for_update`; a rejeição salva somente `status` e `descricao`; e a exclusão de `_APROVANDO` antecede o ramo do portão. O R8 contém a exigência de `False`. [app/ponto/services/cartorio.py:1033] [app/ponto/services/cartorio.py:1580] [app/ponto/services/justificativa.py:313] [app/ponto/services/justificativa.py:332] [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:182]

   O delta fornecido altera os tetos **227→230 e 291→294**, isto é, **+3 em cada**, embora as causas ao lado afirmem medição **228→230 e 292→294**, isto é, **+2**. Portanto o aumento em relação ao teto anterior não é exatamente o delta declarado. A medição independente e o SQL das chaves sem fecho são **[não verificado]**. [app/ponto/tests/test_selo_chokepoint_escrita.py:196] [app/ponto/tests/test_selo_chokepoint_escrita.py:210]

2. **Escritores, leitores e significado — BLOQUEIA.**

   - `Justificativa.status` é escrito pela auto-rejeição, pelas decisões individuais do DP e pelas decisões em lote; o fechamento consulta justificativas aprovadas, enquanto a fila e o lote distinguem pendentes de decididas. A rejeição humana grava também autor, instante e motivo próprio; a automática acrescenta o motivo à `descricao`. Logo `rejeitada` abrange decisões de naturezas diferentes. [app/ponto/services/justificativa.py:332] [app/ponto/services/justificativa.py:205] [app/ponto/services/justificativa.py:383] [app/ponto/views.py:1797] [app/ponto/views.py:1877] [app/ponto/services/justificativa.py:209]
   - `descricao` é alterada pela auto-rejeição e pela regularização; a própria auto-rejeição lê o texto anterior e lhe acrescenta a causa. Leitores completos da descrição **[não verificado]**. [app/ponto/services/justificativa.py:332] [app/ponto/services/regularizacao.py:102]
   - `atualizado_em` é `auto_now`, mas a auto-rejeição o exclui do `update_fields`; regularização o inclui explicitamente e os saves completos do DP também o atualizam. `houve_fato_novo` o lê como indício de fato do dia; o card do DP o lê junto com `status` como data de decisão; o admin expõe os campos do modelo. Assim, o campo não significa mais necessariamente “última alteração da linha”. [app/ponto/models.py:179] [app/ponto/services/justificativa.py:334] [app/ponto/services/regularizacao.py:106] [app/ponto/services/justificativa.py:383] [app/ponto/services/cartorio.py:406] [app/ponto/views.py:1741] [app/ponto/admin.py:18]
   - O card **não conta a auto-rejeição pela data em que ela ocorreu**, salvo se o carimbo anterior já cair em hoje: nesse caso pode contar uma rejeitada cujo carimbo representa outra alteração. Essa consequência mais precisa decorre do filtro, e o custo de não avançar o carimbo foi aceito no aval fornecido. [app/ponto/views.py:1741] [app/ponto/services/justificativa.py:334]
   - O fecho chama `reconciliar`, que passa pela escrita central de `status_local`; os gates e consumidores de chamados usam o estado do chamado. A ocorrência D5 é escrita pela porta de nota, com tipo e referência às justificativas; o reconciliador usa ocorrência `retratado`. Censo completo de leitores de ocorrência **[não verificado]**. [app/chamados/models.py:654] [app/chamados/models.py:207] [app/ponto/services/cartorio.py:48] [app/chamados/services/nota.py:75] [app/chamados/reconciliador.py:577]
   - Há concorrência fora da porta individual: `rejeitar_lote_justificativas` lê a pendente sem trava e depois executa `save()` completo; uma instância lida antes da auto-rejeição pode sobrescrever campos depois dela. A existência desse caminho é anterior ao delta, mas a trava nova não o torna seguro. [app/ponto/views.py:1875] [app/ponto/services/justificativa.py:314]

3. **Ordem de trava — BLOQUEIA.** No reconciliador, a transação chama `trava_na_ordem` antes da cerca e da rejeição; no DP individual, `travar_origem_antes` precede a trava da justificativa. A ordem global interna inclui **Colaborador → Disputa → Chamado → Pergunta**, não apenas as três entidades resumidas na pergunta. [app/chamados/reconciliador.py:542] [app/chamados/reconciliador.py:563] [app/ponto/services/justificativa.py:138] [app/chamados/models.py:2816]

   Há caminho inverso: a regularização atualiza a justificativa dentro de `atomic`; seu `post_save` chama o cartório, que pode chegar ao fecho e às travas de chamado enquanto a transação ainda mantém a trava da justificativa. O próprio código documenta limite de ordem para regularização e outros caminhos. A ocorrência efetiva de deadlock em produção **[não verificado]**. [app/ponto/services/regularizacao.py:88] [app/ponto/signals.py:161] [app/ponto/signals.py:85] [app/ponto/services/cartorio.py:1580] [app/ponto/services/justificativa.py:101]

4. **Falha e efeitos da transação — NÃO BLOQUEIA no escopo demonstrado.** `julgar_celula` não captura a falha de `julgar_colab`; seu `try` posterior é apenas para precedência. Quando acionado pelo signal, porém, `_julgar_dias` envolve o julgamento em `engolir`, que registra e suprime a exceção; isso já existia. [app/ponto/services/cartorio.py:2087] [app/ponto/signals.py:85] [app/core/observ.py:85]

   Antes, o fecho podia concluir sua transação própria antes da falha posterior na rejeição ou ocorrência; agora, a exceção desfaz o bloco inteiro, inclusive fechos e efeitos de banco executados dentro dele. No `_fech`, a primeira lavra e o E2 ocorrem antes do novo bloco; a batida que acionou o signal também não foi colocada dentro desse novo `atomic`. Uma transação externa pode ter escopo maior, portanto não se pode generalizar a persistência de toda batida ou efeito para todos os chamadores. [app/chamados/services/fecho_runtime.py:161] [app/ponto/services/cartorio.py:1016] [app/ponto/services/cartorio.py:1026] [app/ponto/services/cartorio.py:1033] [app/ponto/registro_batida.py:176] [app/ponto/services/justificativa.py:138]

5. **Classes de reprovação e testes — BLOQUEIA.** O segundo escritor concorrente e a inversão de ordem descritos nos itens 2 e 3 são achados concretos. `atualizado_em` também tem dois usos que deixam de acompanhar toda alteração da linha; a mudança específica foi aceita no aval fornecido, mas seus leitores não compartilham uma interpretação de “última alteração”. [app/ponto/views.py:1875] [app/ponto/services/regularizacao.py:102] [app/ponto/views.py:1741] [app/ponto/services/cartorio.py:406] [nao verificado]

   Não identifiquei no delta um novo juiz ou derivação paralela; o relógio de `houve_fato_novo`, quando `ate` não é fornecido, continua lendo `now()` dentro da função. Ausência de outras violações dessas classes **[não verificado]**. [app/ponto/services/cartorio.py:376]

   O teste `LEI_APROVANDO` sem portão exerce diretamente a exclusão e, pela lógica, falharia se ela fosse movida exclusivamente para o ramo com portão; a morte executada do mutante é **[não verificado]**. Os dois testes `C5_UM_ATO` verificam estado após injeção de falha, mas não afirmam que a função injetada foi chamada; no caminho da batida, o signal ainda pode engolir exceções. Assim, o código dos testes sozinho não prova que os mutantes de remoção dos `atomic` foram mortos em disco. [app/ponto/tests/test_dst_l2_jz1_rejeita_justificativa.py:321] [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:125] [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:281] [app/ponto/signals.py:85] [nao verificado]

6. **Edição do selo — NÃO BLOQUEIA quanto ao conteúdo adicional perguntado; conflito literal no aval.** No delta fornecido, as alterações de `test_selo_chokepoint_escrita.py` são as duas linhas de teto e suas justificativas; não há outra edição desse arquivo no anexo. Entretanto, o aval fornecido simultaneamente manda subir os tetos e proíbe editar o selo além do fixture já cortado: tomada isoladamente, essa proibição também alcança as linhas de teto. A autorização específica para subir teto e a proibição geral precisam ser lidas juntas; a resolução formal dessa tensão é **[não verificado]**. [app/ponto/tests/test_selo_chokepoint_escrita.py:196] [app/ponto/tests/test_selo_chokepoint_escrita.py:210]

## 19/11 08:18 · sessao 4ac148fa63de · TREM-DE-POUSO-1911 · verificador · origem=Code

**AVAL-DEVOLVIDO:** há defeitos demonstrados nas perguntas 1, 2 e 3. O principal é que o aval manda pousar os vagões 1 e 2 separadamente, mas as árvores intermediárias não são a árvore final certificada; além disso, não há veredito de opus final aprovado da ESPURIA-TELA entre os anexos fornecidos. A pergunta 4 permanece parcialmente [não verificado]. (`aval_literal.txt`, trechos “VAGAO 1” e “PROIBIDO”; `comboio_c_merges.txt`; `relato_trechos.txt:249`.)

A cópia disponibilizada contém os arquivos, mas não contém objetos Git acessíveis: `git` informou “not a git repository”. Assim, o diff de cada vagão contra `3f666ad84`, o `HEAD` do main atual e a comparação byte a byte com as cópias estão **[não verificado]**. O `diffstat_producao.txt` é uma lista conjunta de 22 arquivos, com caminhos abreviados em algumas entradas; ele não atribui os arquivos a cada vagão. (`diffstat_producao.txt`; [não verificado].)

### Vagão 1 — F6F7

1. **PROCEDE.** A suite citada é de `84786d946`: `Ran 16932`, `rc=0`. Existe opus final 4 aprovado, mas sobre `a555d027b`; as linhas completas da LEI-AKITA “já aprovadas” não estão demonstradas no veredito fornecido. O primeiro merge do comboio tem árvore `232923…`, diferente da árvore final `9dbdb9…`: o deploy individual do primeiro vagão não pousaria a árvore final certificada. (`comboio_c_espera.out`; `veredito_f6f7_opus4.md`, cabeçalho e “Não verificado”; `comboio_c_merges.txt`.)

2. **NÃO PROCEDE**, quanto à cláusula de esperar a simulação: o aval literal diz expressamente que a substitui nos avais nomeados. Outra colisão vigente para este vagão está [não verificado]. (`aval_literal.txt`, primeiro parágrafo; `relato_trechos.txt:89`.)

3. **PROCEDE.** O literal exige deploy individual e proíbe pousar árvore diferente da certificada, embora a árvore após o primeiro merge seja diferente da final do comboio. Também exige `cmp` arquivo a arquivo contra “a cópia” sem identificar, para esse passo, quais arquivos e qual cópia intermediária servem de referência; o resultado concreto de qualquer `cmp` está [não verificado]. (`aval_literal.txt`, trechos “VAGAO 1” e “PROIBIDO”; `comboio_c_merges.txt`.) [nao verificado]

4. **NÃO PROCEDE quanto a um toque fora do declarado demonstrado; alcance completo [não verificado].** O relato registra acesso **pelo leitor** à retenção, ainda que não por mudança de arquivo em `folha/`; isso não prova alteração de gravado exportado. A medição anterior cita quatro ocorrências do argumento já existente `exportada=`. A lista exata de produção deste vagão contra `3f666ad84` está [não verificado]. (`relato_trechos.txt:63`, `:166`, `:201`, `:206`; [tarefa_do_dia.py:35](/home/ronald/codex_ws/84786d946bace55e7f61e044239ac0fa199b8a86/app/chamados/services/tarefa_do_dia.py:35).)

### Vagão 2 — ESPURIA-TELA c10

1. **PROCEDE.** Não existe, entre os anexos fornecidos, veredito de opus final **APROVADO** para este vagão. O relato diz que a suite e o opus final ainda seriam feitos sobre a árvore de pouso; a suite verde do comboio não substitui, por si, esse veredito. A árvore intermediária após este merge também difere da árvore final certificada. (`relato_trechos.txt:249`; `comboio_c_espera.out`; `comboio_c_merges.txt`.) [nao verificado]

2. **NÃO PROCEDE quanto à colisão demonstrada.** A substituição da espera da simulação é expressa no novo aval; os anexos fornecidos não demonstram outra colisão específica para este vagão. Eventual conteúdo não transcrito dos avais anteriores está [não verificado]. (`aval_literal.txt`, primeiro parágrafo; `relato_trechos.txt:249`.)

3. **PROCEDE.** O segundo deploy individual tem o mesmo conflito literal: a árvore `caf200…` não é a árvore final certificada `9dbdb9…`, apesar da proibição de pousar árvore diferente da certificada. A referência e o resultado do `cmp` por arquivo estão [não verificado]. (`aval_literal.txt`, trechos “VAGAO 2” e “PROIBIDO”; `comboio_c_merges.txt`.) [nao verificado]

4. **NÃO PROCEDE quanto a um toque fora do declarado demonstrado; alcance completo [não verificado].** A medição da ponta anterior registra mudanças em `ponto/turnos.py`, `ponto/services/espelho.py` e `escala/folgas.py`, e nenhuma ocorrência nas categorias pesquisadas de `folha/`, export, retenção ou `DiaPago`. Ela não é o diff exato de c10 contra a base. A lista completa de produção deste vagão está [não verificado]. (`relato_trechos.txt:39`, `:63`.)

### Vagão 3 — F9

1. **NÃO PROCEDE quanto à suite e à existência de opus aprovado.** A suite do comboio corresponde a `84786d946`, e há opus final 2 aprovado para F9 em `ac955a260`. As linhas completas da LEI-AKITA por obra, ditas já aprovadas, estão [não verificado] no material fornecido. (`comboio_c_espera.out`; `veredito_f9_opus2.md`, cabeçalho; `aval_literal.txt`, trecho “LEI-AKITA”.)

2. **NÃO PROCEDE**, quanto à espera da simulação e à ordem: o novo aval substitui expressamente a espera e preserva F6F7 antes de F9. Outra colisão vigente específica está [não verificado]. (`aval_literal.txt`, primeiro parágrafo e trecho “VAGAO 1”; `relato_trechos.txt:97`.)

3. **NÃO PROCEDE quanto a um defeito adicional demonstrado.** O terceiro merge produz a árvore final certificada; a conformidade efetiva dos arquivos, do `cmp` e da linha LEI-AKITA do commit de pouso permanece [não verificado]. (`comboio_c_merges.txt`; `aval_literal.txt`, trecho “VAGAO 1”.) [nao verificado]

4. **NÃO PROCEDE quanto a um toque fora do declarado demonstrado; alcance completo [não verificado].** O relato da medição anterior não encontrou alterações nas categorias pesquisadas para F9; o próprio veredito limita a afirmação de “intocados” ao delta `d3e9544fd..ac955a260`, não ao diff inteiro contra o main. A lista exata de produção do vagão contra `3f666ad84` está [não verificado]. (`relato_trechos.txt:63`; `veredito_f9_opus2.md`, primeiro parágrafo.)

### Vagão 4 — ESCALA c7

1. **NÃO PROCEDE quanto à existência de opus.** Há opus final 3 aprovado para a obra e opus final **do delta** aprovado para c7; o segundo não é licença de pouso e traz condições. Sua suite própria é `Ran 16952`, `rc=0`, distinta da suite do comboio C. (`relato_trechos.txt:100`; `veredito_escala_c7_delta.md`, cabeçalho e itens 3, 4 e 6.) [nao verificado]

2. **PROCEDE.** O complemento citado no relato exige smoke de clique **antes** do pouso da ESCALA, enquanto o novo aval coloca os smokes de clique dele ao fim do vagão 4. A substituição expressa no novo aval trata da cláusula de simulação; não menciona revogação desse requisito de smoke anterior. (`relato_trechos.txt:274`; `aval_literal.txt`, primeiro parágrafo e trecho “SMOKE”.) [nao verificado]

3. **PROCEDE.** O veredito do delta exige a linha LEI-AKITA específica do instrumento e citar **L-042 no commit**; o aval resume as condições a linha por obra, correção da premissa e anotação de `col898`, sem mencionar a citação de L-042. A premissa “nada é escrito” consta de fato na cópia; o veredito afirma que ficou falsa na árvore c7. A medida de `col898` foi anotada no próprio veredito, mas a correção do texto e o conteúdo do futuro commit estão [não verificado]. (`veredito_escala_c7_delta.md`, itens 3, 5 e 6 e “MEDIDA PRE-POUSO”; [detector_proposta.py:25](/home/ronald/codex_ws/84786d946bace55e7f61e044239ac0fa199b8a86/app/escala/detector_proposta.py:25); [test_contract_no_batida_date.py:25](/home/ronald/codex_ws/84786d946bace55e7f61e044239ac0fa199b8a86/app/ponto/tests/test_contract_no_batida_date.py:25).)

4. **NÃO PROCEDE quanto ao delta c7 demonstrado; obra inteira [não verificado].** O veredito diz que o diff `68d17aa7d..2fd98211a` toca só teste. Isso não identifica todos os arquivos de produção da ESCALA contra `3f666ad84`, nem prova o alcance integral da obra em dinheiro, exportados ou `!`. (`veredito_escala_c7_delta.md`, item 1; [não verificado].)

### Vagão 5 — E4C-F3

1. **NÃO PROCEDE quanto à existência de opus aprovado.** O relato registra opus final aprovado para `88f67e29f`, com a LEI-AKITA do pouso entre os itens a refazer. A certificação do comboio D é uma condição futura no aval; sua existência e resultado estão [não verificado]. (`relato_trechos.txt:25`; `aval_literal.txt`, trecho “VAGAO 5”.)

2. **NÃO PROCEDE quanto à colisão demonstrada.** O novo aval substitui expressamente a espera da simulação; outra colisão vigente específica de F3 está [não verificado]. (`aval_literal.txt`, primeiro parágrafo; `relato_trechos.txt:25`.)

3. **PROCEDE.** A formulação geral de LEI-AKITA por obra remete a “linhas já aprovadas nos opus finais”, mas o relato do opus de F3 registra especificamente **“LEI-AKITA do pouso a refazer”**. O texto e a aprovação de uma linha refeita estão [não verificado]. (`aval_literal.txt`, trecho “LEI-AKITA”; `relato_trechos.txt:25`.) [nao verificado]

4. **NÃO PROCEDE quanto a um toque fora do declarado demonstrado; alcance completo [não verificado].** O despacho de construção de F3 autorizava escrever só o teste, e o corte deixa `carimbar_resposta_pela_trilha` fora da fatia; isso não substitui o diff exato solicitado contra `3f666ad84`. A lista de arquivos de produção desse diff e eventuais efeitos em gravado exportado, dinheiro ou `!` estão [não verificado]. (`relato_trechos.txt:43`, `:227`.)

## 19/11 08:11 · sessao d110a5090543 · - · - · origem=Ronald (tmux)

Investigação dos leitores e seus chamadores enviada ao agente principal, com referências à base e ressalvas sobre as guardas e a carga em lote.

## 19/11 08:11 · sessao de53552e4dba · - · - · origem=Ronald (tmux)

Evidências da volta 3 e das guardas foram enviadas ao agente principal, incluindo a limitação do K6 às metades de intervalo e o filtro SQL anterior à releitura de `validada_em` ([materializacao.py:804](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:804), [materializacao.py:2104](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2104)).

## 19/11 08:11 · sessao 96fb0bcc2955 · - · - · origem=Ronald (tmux)

Enviado ao agente principal o levantamento da testemunha, da conservação do passivo, dos dois juízes e do corte temporal, com referências à base e casos discriminantes. Também sinalizei que `pergunta_respondida` recebe tanto `LeituraDoAto` quanto objetos temporais sintéticos, e que o corte da projeção atual não prova, por si só, o resultado de um replay histórico.

## 19/11 08:09 · sessao af39d0380fdf · E4C-F5 · verificador · origem=Code

**ETAPA A — AVAL-DEVOLVIDO: não**, para a revisão e correção do brief. As divergências abaixo precisam constar do C3. A etapa não foi executada nem o C3 foi criado, pois esta revisão foi autorizada somente para leitura. A identidade da cópia com o hash informado não pôde ser comprovada por Git: a cópia não contém `.git` [não verificado].

**ETAPA B — AVAL-DEVOLVIDO: sim**, neste momento. O literal exige antes C3 fechado, matriz completa, zero perguntas de lei e aprovação do censo pelo wrapper; esses resultados não estão no dado fornecido [não verificado]. Além disso, a cobertura das três voltas e o alcance das portas de escrita precisam ser demonstrados antes da construção.

## 1. Premissas na cópia

**a) Testemunha: confere.** `DecisaoPergunta.leitura` retorna `LeituraDoAto`, com os três campos `validada_em`, `respondida_em` e `resposta_colab` ([models.py:19](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:19), [models.py:4295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4295), [models.py:4326](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4326)).

**b) As 15 entradas listadas no §2 existem nas posições citadas.** Todas contêm os campos indicados; a última é um `refresh_from_db` que nomeia os campos, e as condições que os usam aparecem nas linhas seguintes ([validacao.py:262](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:262), [validacao.py:264](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:264)).

- Juízes: [juizes.py:1389](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1389) e [juizes.py:1552](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1552).
- Serviços: [acoes_disputa.py:349](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:349), [declaracao_texto.py:228](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:228), [disputa_emissao.py:2083](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:2083) e [fio.py:188](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:188).
- Materialização: [materializacao.py:199](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:199), [materializacao.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:201) e [materializacao.py:2123](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2123).
- Validação: [validacao.py:640](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:640), [validacao.py:685](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:685), [validacao.py:898](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:898), [validacao.py:1064](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:1064) e [validacao.py:262](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:262).
- Template: [chamados_tags.py:166](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/templatetags/chamados_tags.py:166).

**c) `leituras`: a premissa literal da assinatura não confere.** A função pública `tarefa_do_dia` aceita `carga`, não `leituras` ([tarefa_do_dia.py:825](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:825)). O kwarg `leituras` existe na função interna `_tarefa_pura`; a carga monta um mapa por pergunta e o repassa ([tarefa_do_dia.py:196](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:196), [tarefa_do_dia.py:425](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:425), [tarefa_do_dia.py:677](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/tarefa_do_dia.py:677)). Isso não demonstra, por si só, o teto de consultas de cada sítio a migrar [não verificado].

**d) Juízes e passivo: existem, com ressalva.** Os dois juízes existem e hoje leem a projeção ([juizes.py:1382](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1382), [juizes.py:1544](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1544)). `DecisaoPergunta.leitura` incorpora o passivo por `linhas_da_projecao_sem_linha` ([models.py:4238](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4238), [models.py:4308](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4308)). A conservação não é identidade literal para qualquer projeção: texto sem `respondida_em` não entra como resposta e `None` é normalizado para texto vazio; texto sem carimbo é classificado como estado ilegal ([models.py:4251](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4251), [models.py:4260](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4260), [juizes.py:1537](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1537)). O corte temporal do primeiro juiz também precisa ser preservado ([juizes.py:1391](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1391)).

**e) Teste: arquivo existe, referência à expectativa não confere literalmente.** As linhas 330 e 334 criam o chamado e aplicam um *mock* de relógio; não são a expectativa do dono. O caso SILÊNCIO espera `silencio_decidido` e compara a tarefa antes e depois, sem um `assert` explícito de dono admin nessas linhas ([test_f1nova_b1_juiz_respondida.py:328](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:328), [test_f1nova_b1_juiz_respondida.py:338](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:338), [test_f1nova_b1_juiz_respondida.py:343](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:343)).

## 2. Colisão com aval e vínculos

Pelos textos raspados, o novo literal conserva as proibições centrais do item 2 da ESPINHA: escrita nova, alteração de motor, folha, exportação, DiaPago, escala ou vínculo, e pouso antes das condições indicadas. A alteração da testemunha corrige uma premissa do texto anterior; sua equivalência com todos os cortes anteriores é **[não verificado]**. [nao verificado]

A proibição de tocar vínculo é compatível, em texto, com as regras existentes de `vinculos.py`: primeiro apontamento prevalece e a porta não apaga vínculo com `None` ([vinculos.py:11](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/vinculos.py:11), [vinculos.py:24](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/vinculos.py:24)). Efeitos indiretos de todas as portas sobre vínculos são **[não verificado]**.

**MANHA-1911-0810: colisão [não verificado].** Seu conteúdo não consta do dado raspado apresentado nem foi localizado na cópia examinada.

## 3. Condições que não fecham ou exigem prova

- **19 linhas versus 15 sítios:** o §2 fornecido lista 15 posições, mas duas são linhas distintas da mesma função `par_do_intervalo` ([materializacao.py:199](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:199), [materializacao.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:201)). A correspondência das 19 linhas do censo com esses 15 sítios é **[não verificado]**. Além disso, o RED-5 do histórico é mantido pelo literal, embora o histórico esteja fora da lista de 15 posições; a matriz precisa explicitar seu tratamento ([historico.py:96](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/historico.py:96)).
- **Voltas 1/2/3:** a partição completa dos 15 sítios, sem sobra, é **[não verificado]**. `par_do_intervalo`, por exemplo, é compartilhada entre leitor de tela, validação e guarda de plantio; sua classificação não decorre apenas do nome da função ([materializacao.py:181](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:181)).
- **Conta:** 40 + 213 = **253**, não 353. O literal raspado estabelece 253; a existência, composição, eventual sobreposição e abrangência real dessas classes em produção são **[não verificado]**.
- **`models.py`:** a testemunha e o passivo já existem nesse arquivo, mas que algum dos 15 sítios *precise* alterá-lo é **[não verificado]** ([models.py:4238](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4238), [models.py:4295](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:4295)). O wrapper da cópia contém restrição para escrita em `models.py` ([codex_pergunta.sh:346](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/bin/codex_pergunta.sh:346)).

## 4. Exportada, dinheiro e efeitos de escrita

**Há alcance sobre escrita existente, expressamente relevante para a volta 3.** A condição em `materializacao.py:2123` determina se chama `_materializar_uma`; essa porta pode criar batida ou ausência conforme o tipo e chama a porta de registro de batida ([materializacao.py:2123](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2123), [materializacao.py:2069](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2069), [materializacao.py:1487](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1487)). A guarda de devolução de validação também pode limpar campos e registrar ato ([validacao.py:262](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:262), [validacao.py:269](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:269)).

A materialização contém uma guarda para competência entregue antes da escrita ([materializacao.py:1866](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1866)). Que **todas** as portas da volta 3 preservem os casos entregues ou exportados, que não haja efeito sobre célula ou dinheiro fora do declarado e que nenhuma classe adicional mude são **[não verificado]**. A medição exigida pelo literal continua necessária; o dado raspado não traz seus resultados.

**perguntas de lei: 0 identificadas nesta revisão.** As pendências acima são de premissa, escopo e evidência; não há, no material disponível, base suficiente para afirmar que exigem uma nova decisão de lei [não verificado].

## 19/11 08:08 · sessao 7e4d5d4d8b08 · - · - · origem=Ronald (tmux)

As 15 entradas listadas no §2 existem nas linhas indicadas e contêm os campos citados: [juizes.py:1389](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1389), [juizes.py:1552](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/juizes.py:1552), [acoes_disputa.py:349](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:349), [declaracao_texto.py:228](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:228), [disputa_emissao.py:2083](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:2083), [fio.py:188](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:188), [materializacao.py:199](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:199), [materializacao.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:201), [materializacao.py:2123](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2123), [validacao.py:640](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:640), [validacao.py:685](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:685), [validacao.py:898](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:898), [validacao.py:1064](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:1064), [chamados_tags.py:166](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/templatetags/chamados_tags.py:166) e [validacao.py:262](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:262). As entradas de `par_do_intervalo` são duas linhas da mesma função; em `validacao.py:262`, a operação é um `refresh_from_db`, seguido de guardas nas linhas 264–269. [materializacao.py:175](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:175), [validacao.py:262](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:262)

A porta de materialização pode criar batida: [materializacao.py:1487](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1487), [registro_batida.py:176](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/registro_batida.py:176). O `post_save` pode acionar julgamento e escrita da ata da célula, sujeito aos guardas da função: [signals.py:85](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/signals.py:85), [signals.py:131](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/signals.py:131), [cartorio.py:2012](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/services/cartorio.py:2012), [cartorio.py:1987](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/ponto/services/cartorio.py:1987).

## 19/11 08:08 · sessao da3a0e5d0a29 · - · - · origem=Ronald (tmux)

Análise enviada ao agente principal, com referências às assinaturas, à carga em lote, às regras de vínculos e à restrição do wrapper. A colisão com MANHA-1911-0810 permanece **[não verificado]**, pois seu conteúdo não foi localizado.

## 19/11 08:08 · sessao 29e5d408b193 · - · - · origem=Ronald (tmux)

Verificação enviada ao agente principal, com referências às linhas da testemunha, dos juízes, da conservação do passivo, do teste e da leitura em lote. [nao verificado]

## 19/11 08:06 · sessao 62c6e0723961 · DST-L2-JZ1 · verificador · origem=Code

**1. Premissas — NÃO CONFERE integralmente.**

- **(a)** Na cópia c4, `_ato_da_retratacao` já chama o escritor extraído. Esse escritor grava `status`, `descricao` **e `atualizado_em`**. O comentário registra que o antigo código inline do Fix A salvava sem `atualizado_em`, mas o Fix A executado pela cópia hoje não faz isso. [reconciliador.py:563](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:563), [justificativa.py:319](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:319).
- **(b) CONFERE.** Aprovar e rejeitar entram em `atomic`, travam a origem antes e fazem `select_for_update` da justificativa pendente. [justificativa.py:140](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:140), [justificativa.py:364](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:364).
- **(c) CONFERE.** Nos dois sítios, o lote recebe os chamados cujo fecho retornou sucesso; a ocorrência é chamada para os que tiveram rejeição. [cartorio.py:1030](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1030), [cartorio.py:1570](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1570).
- **(d) CONFERE com limite.** Um `save(update_fields=['status', 'descricao'])` não atualizaria o `auto_now` de `atualizado_em`; o modelo também tem `criado_em` e `aprovada_em`, mas esse save não os altera. O predicado lê os três: portanto a auto-rejeição, por esse save isolado, não acrescentaria um novo carimbo visível a ele. Isso não garante `False` se algum carimbo já estiver na janela ou houver outro fato novo. [models.py:164](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/models.py:164), [models.py:179](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/models.py:179), [cartorio.py:393](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:393).
- **(e) CONFERE**, com uma precisão: o contador filtra status decidido e a **data já armazenada** em `atualizado_em`. Sem atualizar o campo, a rejeição ainda pode entrar no contador se esse carimbo anterior já for de hoje; não se pode afirmar que ela nunca será contada. [views.py:1738](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/views.py:1738).

**2. Colisão com o aval vigente — PROCEDE.** O literal fornecido manda subir o teto medido e escrever a causa ao lado, mas também proíbe editar o selo JZ1 além do fixture já cortado. Como o teto e sua justificativa estão no arquivo do selo, cumprir ambas as ordens literalmente exige esclarecer a exceção. [não verificado: os anexos transcritos não têm linhas numeradas], [test_selo_chokepoint_escrita.py:196](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_selo_chokepoint_escrita.py:196).

A nova lei fornecida resolve a pergunta anterior de `_APROVANDO` para os dois caminhos; a exclusão já ocorre antes do portão na cópia. A ocorrência continua prevista apenas para quem rejeitou, e o reconciliador já evita repetir a consulta de retenção. [não verificado: anexo sem linhas numeradas], [justificativa.py:299](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:299), [reconciliador.py:538](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:538).

O `atomic` externo proposto **muda a fronteira transacional** para chamadores que hoje chegam sem transação: o `atomic` da transição termina antes da rejeição e ocorrência; com o envoltório, os três efeitos passam a compartilhar o resultado da transação. O efeito concreto sobre tratamento de erros, locks e callbacks de cada chamador está [não verificado]. [fecho_runtime.py:161](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/fecho_runtime.py:161), [cartorio.py:1030](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1030).

**3. Condições do literal — PROCEDE a ressalva.** O selo conta comandos SQL capturados durante cenários completos, inclusive `SAVEPOINT`/`RELEASE`, compara o total com um **teto**, e verifica separadamente a quantidade exata de registros `on_commit`. Não mede apenas consultas de dados nem garante, por si, que o teto iguale o valor medido. [test_selo_chokepoint_escrita.py:287](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_selo_chokepoint_escrita.py:287), [test_selo_chokepoint_escrita.py:428](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_selo_chokepoint_escrita.py:428).

Um `atomic` aninhado **pode** acrescentar um par mensurável; o valor e as chaves afetadas dependem do posicionamento e do caminho efetivamente percorrido. Em particular, envolver a construção de `_porm` pode abrir o bloco mesmo sem fecho; o selo tem cenários em que chamados ficam vivos. Delta real, SQL e eventual delta sem fecho: **[não verificado]**. [cartorio.py:1564](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1564), [test_selo_chokepoint_escrita.py:445](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_selo_chokepoint_escrita.py:445).

O `False` pedido para R8 é **alcançável em princípio** com a opção (b), se nenhum outro carimbo ou fato cair na janela; não é consequência universal de omitir `atualizado_em`. O R8 atual apenas imprime o resultado e afirma a estabilidade após o segundo julgamento; não afirma `False`. Sua execução alterada está [não verificado]. [cartorio.py:393](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:393), [test_dst_l2_jz1_c3_lote_portao.py:129](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:129).

**4. Exportada, dinheiro ou `!` fora do declarado — NÃO COMPROVADO.** Os sítios examinados escrevem justificativa e ocorrência; o fechamento financeiro lê justificativas aprovadas, de modo que uma mudança no conjunto aprovado pode ter efeito indireto. Escrita direta nova em exportada, motor, folha, espelho ou `FechamentoMensal`, e impacto efetivo nesses destinos: **[não verificado]**. A equivalência dos sítios na main indicada também está **[não verificado]**. [justificativa.py:319](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:319), [fechamento.py:660](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/fechamento.py:660).

**AVAL-DEVOLVIDO: sim**  
**perguntas de lei: 1** — esclarecer a autorização para editar os tetos e suas justificativas diante da proibição literal de editar o selo. [não verificado: anexo sem linhas numeradas]

## 19/11 03:29 · sessao f04606e63662 · ESCALA-NAO-DESCREVE · sonda · origem=Code

A escrita em `/app/logs/errors.log` fica no `tmpfs` do container e morre com ele: a árvore é montada em `/app` como somente leitura, `/app/logs` recebe um `tmpfs` próprio e o container usa `--rm`. Portanto, essa escrita não chega ao arquivo do host pelo caminho mostrado e não impede a sonda pelo critério corrigido. [bin/sombra.sh:182](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/bin/sombra.sh:182), [bin/sombra.sh:189](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/bin/sombra.sh:189), [app/config/settings/saas.py:136](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/saas.py:136).

A sonda abaixo usa `proposta` da função real como universo, consulta o ciclo atual pela função da tela e mede turnos pela porta solicitada. Uma proposta cujo tipo não aparece no mapa de ciclos é contada como erro, sem presumir que seja não cíclica. [app/ponto/services/esmeril_espelho.py:215](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:215), [app/escala/services/cadastro_realidade.py:819](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:819), [app/ponto/turnos.py:1850](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:1850).

```python
import collections
import datetime
import os
import sys

HOJE = datetime.date(2026, 11, 19)

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import TipoEscala
from escala.services.cadastro_realidade import ciclos_dos_destinos
from ponto.services.esmeril_espelho import anomalias_da_frota
from ponto.turnos import turnos_do_colab


INI = HOJE - datetime.timedelta(days=60)
FIM = HOJE - datetime.timedelta(days=1)


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def main():
    universo = None
    erros = 0
    contador = 0
    ciclicas = 0
    nao_ciclicas = 0
    por_confianca = collections.Counter()
    por_empresa = collections.Counter()
    linhas_casos = []
    erros_casos = []
    etapa = "GUARDA_SOMBRA"

    try:
        with transaction.atomic():
            guarda_sombra()

            etapa = "UNIVERSO"
            proposta = anomalias_da_frota(HOJE)["proposta"]
            universo = len(proposta)

            etapa = "CICLOS"
            ciclos = ciclos_dos_destinos(proposta)

            etapa = "CASOS"
            for cid, p in sorted(
                proposta.items(), key=lambda item: int(item[0])
            ):
                try:
                    # O savepoint permite continuar após erro de banco no caso.
                    with transaction.atomic():
                        colab = Colaborador.objects.get(pk=int(cid))
                        empresa = colab.empresa_id
                        tipo_id = p["tipo_escala_id"]
                        codigo = p["codigo"]
                        confianca = p["confianca"]

                        if tipo_id not in ciclos:
                            raise RuntimeError("ciclo_ausente")

                        ciclica = ciclos[tipo_id] in TipoEscala.CICLICOS
                        n_turnos = n_cruzam = n_abertos = 0

                        if not ciclica:
                            turnos = list(turnos_do_colab(colab, INI, FIM))
                            n_turnos = len(turnos)
                            for turno in turnos:
                                if turno.saida is None:
                                    n_abertos += 1
                                if turno.entrada is not None and turno.saida is not None:
                                    entrada = timezone.localtime(
                                        turno.entrada.timestamp
                                    )
                                    saida = timezone.localtime(
                                        turno.saida.timestamp
                                    )
                                    if entrada.date() != saida.date():
                                        n_cruzam += 1

                    por_confianca[str(confianca)] += 1
                    por_empresa[str(empresa)] += 1
                    if ciclica:
                        ciclicas += 1
                    else:
                        nao_ciclicas += 1
                        if n_cruzam > 0:
                            contador += 1
                            linhas_casos.append(
                                "col%d empresa=%s codigo=%s confianca=%s "
                                "n_turnos=%d n_cruzam=%d n_abertos=%d"
                                % (
                                    int(cid), empresa, codigo, confianca,
                                    n_turnos, n_cruzam, n_abertos,
                                )
                            )
                except Exception:
                    erros += 1
                    erros_casos.append("ERRO_CASO col%d" % int(cid))

        print("TOTAL=%d" % universo)
        print("CICLICAS=%d NAO_CICLICAS=%d" % (ciclicas, nao_ciclicas))
        for confianca, total in sorted(por_confianca.items()):
            print("CONFIANCA=%s TOTAL=%d" % (confianca, total))
        for empresa, total in sorted(por_empresa.items()):
            print("EMPRESA=%s TOTAL=%d" % (empresa, total))
        for linha in linhas_casos:
            print(linha)
        for linha in erros_casos:
            print(linha)
        print("CONTADOR=%d UNIVERSO=%d ERROS=%d" % (contador, universo, erros))
        print("FIM_SONDA_NOTURNO")
        return 0 if erros == 0 else 1

    except Exception:
        erros += 1
        print("ERRO_FATAL etapa=%s" % etapa)
        print(
            "CONTADOR=%d UNIVERSO=%s ERROS=%d"
            % (contador, universo if universo is not None else "NAO_VERIFICADO", erros)
        )
        print("FIM_SONDA_NOTURNO")
        return 1


sys.exit(main())
```

As contagens por ciclo, confiança e empresa incluem os casos processados com sucesso; um erro de caso aparece separado e pode tornar essas contagens parciais. Os resultados e a execução da sonda são [não verificado].

perguntas de lei: 0

## 19/11 03:28 · sessao d379f5ba58d0 · ESCALA-NAO-DESCREVE · sonda · origem=Code

Não posso entregar a sonda que chama `anomalias_da_frota`: há um caminho que pode escrever em arquivo de log, e a instrução foi não usar a função se ela escrever qualquer coisa.

A função chama `assinaturas_do_colab`, que pode chamar `get_motor_cct`; se a leitura do parâmetro falhar, `get_motor_cct` registra uma exceção. Na configuração herdada pela sombra, o logger raiz envia erros ao handler de arquivo `/app/logs/errors.log`. A transação `READ ONLY` do molde protege o banco, mas não impede essa escrita em arquivo. [app/ponto/services/esmeril_espelho.py:235](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:235), [app/ponto/services/esmeril_espelho.py:170](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:170), [app/core/regua_cct.py:442](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/regua_cct.py:442), [app/config/settings/sombra.py:27](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/sombra.py:27), [app/config/settings/saas.py:136](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/saas.py:136), [app/config/settings/saas.py:183](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/config/settings/saas.py:183).

Também há escrita de warning no caso de empate entre aplicações de CCT vigentes; se esse caso ocorre no dado da sombra é [não verificado]. [app/core/regua_cct.py:265](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/regua_cct.py:265), [app/core/regua_cct.py:366](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/core/regua_cct.py:366).

A janela solicitada confere: o detector recebe `hoje - 60 dias` até `hoje - 1 dia`. A porta `turnos_do_colab` retorna turnos por `data_turno` na janela, mas seu código consulta `Batida` diretamente com `retratada_em__isnull=True`; ela não chama `batidas_apuraveis` nessa carga. [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241), [app/ponto/turnos.py:1850](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:1850), [app/ponto/turnos.py:1867](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/turnos.py:1867).

Universo, contagens e ocorrência efetiva dos caminhos de log: [não verificado].

perguntas de lei: 0

## 19/11 03:17 · sessao c55e102a4eb1 · DST-L2-JZ1 · verificador · origem=Code

**Veredito: BLOQUEIA.** Há uma corrida que pode sobrescrever uma decisão do DP, uma janela em que o fecho fica sem seus efeitos associados e uma mudança de significado de “fato novo” que o R8 não cobre. A exclusão de `_APROVANDO` também altera o universo literal de D3 e precisa de decisão de lei. [app/ponto/services/justificativa.py:304](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:304), [app/ponto/services/cartorio.py:1030](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1030), [app/chamados/models.py:1389](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/models.py:1389).

**Achados**

1. **BLOQUEIA — decisão concorrente pode ser sobrescrita.** Trecho: `filter(..., status='pendente')`, seguido de `list(_qs...)` e depois `j.save(update_fields=['status', ...])`, sem trava da justificativa nem condição de status no update. Entre a leitura e o save automático, o DP pode aprovar a mesma linha; o save automático pode então gravar `rejeitada` sobre a aprovação. A porta do DP usa `atomic`, trava os chamados na ordem e faz `select_for_update()` da justificativa; o escritor novo não compartilha essa proteção. A execução efetiva dessa corrida em banco está [não verificado]. [app/ponto/services/justificativa.py:304](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:304), [app/ponto/services/justificativa.py:319](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:319), [app/ponto/services/justificativa.py:140](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:140), [app/ponto/services/justificativa.py:364](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:364).

2. **BLOQUEIA — fecho, rejeição e ocorrência não formam necessariamente uma transação.** Trecho: `_fech = [... transicionar_com_pauta(...)]` e, só depois, `rejeitar_justificativas_dos_chamados(...)` e `_ocorrencia_da_rejeicao_no_fecho(...)`; o mesmo ocorre em `_porm`. O `atomic` da porta de transição termina ao retornar, antes dos dois novos efeitos. Quando não houver transação externa, uma falha posterior pode deixar chamado fechado com justificativa pendente, ou justificativa rejeitada sem ocorrência D5. O reconciliador, em contraste, envolve seu ato na transação de S128. Quais entradas reais fornecem uma transação externa em todos os casos estão [não verificado]. [app/ponto/services/cartorio.py:1030](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1030), [app/ponto/services/cartorio.py:1570](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:1570), [app/chamados/services/fecho_runtime.py:161](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/fecho_runtime.py:161), [app/chamados/reconciliador.py:543](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:543).

3. **BLOQUEIA — a rejeição automática passa a ser fato novo para outro chamado do mesmo dia.** Trecho: o novo save inclui `atualizado_em`, e `houve_fato_novo` aceita qualquer justificativa do colaborador e dia com esse carimbo em `(desde, ate]`. Se um irmão foi encerrado via admin antes dessa rejeição e os demais fatos não satisfaziam o predicado, sua `recusa_da_reabertura` deixa de retornar `admin_sem_fato_novo`; outras guardas ainda podem recusar a reabertura. O teste R8 verifica o chamado encerrado pelo sistema e só imprime `fato`, sem afirmar seu valor nem criar o irmão administrativo. Portanto não prova a ausência desse efeito. [app/ponto/services/justificativa.py:322](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:322), [app/ponto/services/cartorio.py:393](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:393), [app/ponto/services/cartorio.py:405](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:405), [app/chamados/models.py:1389](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/models.py:1389), [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:146](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:146).

4. **BLOQUEIA — pergunta de lei: `_APROVANDO` foi excluída de D3 mesmo com portão aberto.** Trecho: `_qs.exclude(pk=_APROVANDO.get())` ocorre **antes** do `if portao_de_retencao`, de modo que vale também no reconciliador, que chama com `False`. Se o ponteiro é a própria J em aprovação, o predicado de retenção não responde `justificativa_alheia`; ainda assim a J pendente apontada é excluída. Isso contraria o universo literal de D3 fornecido no enunciado; não é consequência da condição R7, que fala de retenção respondendo reter. O teste C4 demonstra a razão prática pretendida — evitar rejeição transitória e ocorrência falsa —, mas a autorização dessa exceção ao universo de D3 está **[não verificado]**. Pergunta: a exclusão deve valer nos dois caminhos, inclusive no reconciliador? [app/ponto/services/justificativa.py:303](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:303), [app/chamados/reconciliador.py:452](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:452), [app/chamados/reconciliador.py:574](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/reconciliador.py:574), [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:343](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:343).

5. **BLOQUEIA — R9 não sela o leitor até o ESTADO.** Trecho: o teste verifica a chave no carimbo e a saída do comando, mas não executa `fio_estado.linha` nem o gerador do ESTADO. Apagar ou quebrar o leitor final pode deixar esse teste verde. O leitor existe no código: compara genericamente as chaves do carimbo e mostra apenas as fora do esperado; o gerador o chama. Os logs RED/verde, mutantes por sítio e medições antes/depois exigidos no dado fornecido estão **[não verificado]**; a árvore examinada não é um repositório Git, portanto o hash e o diff também não foram confirmados independentemente. [app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:273](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:273), [bin/fio_estado.py:121](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/fio_estado.py:121), [bin/fio_estado.py:191](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/fio_estado.py:191), [bin/gerar_estado.py:260](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/gerar_estado.py:260).

**Campos e efeitos comprovados**

| Campo ou dado | Quem escreve e quem lê, nos caminhos examinados |
|---|---|
| `Justificativa.status` | A função nova grava `rejeitada`; aprovação e rejeição do DP também escrevem status. O próprio lote e D4 filtram `pendente`; API e painel leem o estado. Há outros escritores de rejeição existentes: sua conformidade com a porta aplicável está [não verificado]. [justificativa.py:304](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:304), [justificativa.py:205](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:205), [justificativa.py:374](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:374), [api/views.py:2105](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:2105), [ponto/views.py:1739](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/views.py:1739). |
| `descricao` | O colaborador a fornece pela API; a rejeição automática acrescenta motivo ao mesmo texto; a API devolve o campo ao colaborador. Assim, descrição de entrada e anotação do sistema compartilham o campo. [api/views.py:1994](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:1994), [justificativa.py:323](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:323), [api/views.py:2107](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:2107). |
| `atualizado_em` | `auto_now`, incluído explicitamente no save novo; o painel usa a **data** para decisões “hoje” e `houve_fato_novo` usa o **instante** como mudança de insumo. A configuração usa `USE_TZ=True` e fuso `America/Sao_Paulo`. [ponto/models.py:179](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/models.py:179), [justificativa.py:324](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:324), [ponto/views.py:1741](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/views.py:1741), [cartorio.py:405](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:405), [base.py:93](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/config/settings/base.py:93). |
| Ponteiro e `chamado_origem` | `apontar_justificativa` escreve o ponteiro; a API pode escrever a origem ao criar ou completar a justificativa. O lote lê a união das duas relações; D4 também lê ambas. Outros escritores e leitores completos estão [não verificado]. [vinculos.py:81](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/vinculos.py:81), [api/views.py:1989](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/api/views.py:1989), [justificativa.py:294](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:294), [justificativa.py:342](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:342). |
| `criado_em` e `resolvido_em` | O primeiro nasce com `auto_now_add`; a transição terminal carimba o segundo e a reabertura o limpa. D4 compara **instantes**, com `criado_em > resolvido_em` estrito: igualdade não conta. Sua unidade é **pares justificativa × chamado**, deduplicados entre ponteiro e origem, não necessariamente justificativas distintas. [ponto/models.py:179](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/models.py:179), [chamados/models.py:243](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/models.py:243), [justificativa.py:330](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:330). |
| Ocorrência D5 | O cartório chama `anexar_ocorrencia` após rejeição, passando conteúdo, tipo e IDs; a porta grava `FollowupChamado` e deriva chave por chamado, tipo, referência e autor. O teste lê ocorrências por conteúdo. Outros consumidores do follow-up estão [não verificado]. [cartorio.py:47](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:47), [nota.py:62](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/nota.py:62), [nota.py:88](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/services/nota.py:88), [test_dst_l2_jz1_c3_lote_portao.py:43](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:43). |
| Chave nova do vigia | O vigia chama D4, grava o valor no `censo` e zero em `esperado`; `fio_estado` compara os dois e só exibe a chave quando divergem. O cálculo é de frota, inclusive quando o comando imprime em recorte de empresa; o carimbo é escrito apenas para frota. [vigiar_fio_morto.py:103](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/management/commands/vigiar_fio_morto.py:103), [vigiar_fio_morto.py:190](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/management/commands/vigiar_fio_morto.py:190), [vigiar_fio_morto.py:243](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/chamados/management/commands/vigiar_fio_morto.py:243), [bin/fio_estado.py:124](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/bin/fio_estado.py:124). |

**Dinheiro e restante do checklist.** A alteração nova grava status, descrição, carimbo e ocorrência; não encontrei, nesses trechos, escrita direta em motor, folha, espelho, `DiaPago` ou export. Isso não prova ausência de efeito indireto: fechamento lê justificativas **aprovadas**, e a corrida do achado 1 pode alterar esse conjunto. Impacto efetivo em dinheiro ou export está [não verificado]. [justificativa.py:319](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/justificativa.py:319), [fechamento.py:660](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/fechamento.py:660).

O `post_save` sai por `em_julgamento` dentro do cartório; fora dele chama `julgar_celula`. Os testes declaram os dois cenários, mas sua execução e a morte dos mutantes estão [não verificado]. Não demonstrei segundo juiz registrado, relógio novo fora da borda ou pouso realizado; esses pontos permanecem [não verificado]. [ponto/signals.py:85](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/signals.py:85), [ponto/signals.py:161](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/signals.py:161), [cartorio.py:992](/home/ronald/codex_ws/5e79a6e25205ed30e339cff3168ccec3b4c82946/app/ponto/services/cartorio.py:992).

BLOQUEIA: 5 | FILA: 0 | perguntas de lei: 1

## 19/11 03:12 · sessao da34ab3038c3 · ESCALA-NAO-DESCREVE · sonda · origem=Code

A medição pedida **não pode ser concluída com a janela exata a partir da lavra descrita pelo código**: a geração aceita um `hoje` fornecido pelo chamador, mas a proposta guardada não registra esse valor nem os limites da janela. `lavrado_em` registra o relógio ao fim da geração; usá-lo como `hoje` exigiria uma hipótese. [app/ponto/services/esmeril_espelho.py:215](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:215), [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241), [app/ponto/services/esmeril_espelho.py:246](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:246), [app/ponto/services/esmeril_espelho.py:253](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:253).

**A. Código**

1. **Gravação e leitura.** `lavrar` chama `gravar_lavra(d)` e também escreve um espelho JSON; o banco é a autoridade. O caminho padrão do arquivo é `logs/esmeril_espelho.json`. [app/ponto/services/esmeril_espelho.py:28](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:28), [app/ponto/services/esmeril_espelho.py:321](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:321).

   Trechos literais da gravação:

   ```python
   CHAVE_LAVRA = 'esmeril_espelho'
   ```

   ```python
   MetricaSnapshot.objects.update_or_create(
       chave=CHAVE_LAVRA, escopo='frota', escopo_id=None, data_ref=hoje,
       ...
       'valor': None, 'detalhe': d or {}})
   ```

   O campo é `MetricaSnapshot.detalhe`, um `JSONField`; `ler_lavra()` busca o snapshot mais recente por `data_ref` e, na sombra, verifica o marcador antes de devolvê-lo. [app/inteligencia/models.py:81](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/inteligencia/models.py:81), [app/inteligencia/models.py:95](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/inteligencia/models.py:95), [app/ponto/services/esmeril_espelho.py:291](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:291), [app/ponto/services/esmeril_espelho.py:359](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:359).

   Trecho literal do mapa, com chave igual ao PK do colaborador convertido em string:

   ```python
   proposta[str(c.pk)] = {'tipo_escala_id': p['tipo_escala_id'], 'codigo': p['codigo'],
                          'confianca': r['confianca']}
   ```

   O mapa é incluído no retrato sob a chave `proposta`; só recebe resultados com proposta, para colaboradores com assinatura. [app/ponto/services/esmeril_espelho.py:239](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:239), [app/ponto/services/esmeril_espelho.py:253](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:253).

2. **Dia usado pelo detector.** Ele consulta `batidas_apuraveis` e agrupa cada batida pela data civil local do seu próprio instante:

   ```python
   for b in batidas_apuraveis(colab, de, ate):
       lt = _tz.localtime(b.timestamp)
       por_dia[lt.date()].append(lt)
   ```

   Assim, uma batida após meia-noite cai no dia civil seguinte, não necessariamente no dia do turno; o próprio módulo declara a possibilidade de inflar `dias` e deslocar a entrada modal. [app/escala/detector_proposta.py:18](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/detector_proposta.py:18), [app/escala/detector_proposta.py:49](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/detector_proposta.py:49).

3. **Cíclica e não cíclica.** A lavra não guarda o ciclo. A tela consulta o `tipo_ciclo` **atual** do `TipoEscala` pelo ID da proposta. Sua constante é literalmente:

   ```python
   CICLICOS = ('12x36', '24x48')
   ```

   O predicado retorna `'ciclico'` para esses dois valores e `'aplicar'` para os demais que passem pelas outras condições; um template apagado não entra no mapa. A classificação histórica no momento da lavra é [não verificado]. [app/escala/models.py:386](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/models.py:386), [app/escala/services/cadastro_realidade.py:819](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:819), [app/escala/services/cadastro_realidade.py:830](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:830).

4. **Confiança e botão.** O campo é `confianca`. Para proposta existente, o detector atribui `alta` ou `media`; `insuficiente` acompanha `proposta: None`. [app/escala/detector_proposta.py:118](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/detector_proposta.py:118).

   Trechos literais:

   ```python
   if n >= MIN_DIAS_ALTA and coerente and estavel:
       conf, pq = 'alta', ...
   ...
       conf, pq = 'media', '; '.join(_faltas)
   ```

   O predicado do botão não consulta `confianca`; a view a lê para compor o motivo passado à porta de gravação:

   ```python
   if botao_da_proposta(v, proposta, sig, ciclos_dos_destinos({'p': proposta})) != 'aplicar':
   ```

   ```python
   motivo='proposta do esmeril (lavra de %s): %s, confianca %s' % (
       lavra.get('lavrado_em') or '?', proposta.get('codigo') or '?', proposta.get('confianca') or '?'),
   ```

   [app/escala/services/cadastro_realidade.py:830](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/services/cadastro_realidade.py:830), [app/escala/views.py:493](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/escala/views.py:493).

**B. Sonda parcial, não executada**

A sonda abaixo lê o universo da lavra pela função real, classifica pelo ciclo atual e conta confiança e empresa. Para cada proposta não cíclica, registra erro: sem os limites comprovados não chama o pareador para produzir uma contagem que poderia pertencer a outra janela. Portanto, **não entrega a lista de expostos nem um resultado noturno utilizável quando houver propostas não cíclicas**. O dado raspado fornecido não informa o conteúdo da lavra nem os limites efetivos: [não verificado]. A ausência de lavra no banco resulta em `UNIVERSO=0` com motivo, conforme `ler_lavra()`; falha da guarda ou do marcador resulta em erro. [app/ponto/services/esmeril_espelho.py:359](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:359).

```python
import collections
import os
import sys

from django.conf import settings
from django.db import connection, transaction

from colaboradores.models import Colaborador
from escala.models import TipoEscala
from escala.services.cadastro_realidade import ciclos_dos_destinos
from ponto.services.esmeril_espelho import ler_lavra


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def main():
    universo = 0
    expostos = 0
    erros = 0
    etapa = "GUARDA_OU_LAVRA"
    classes = collections.Counter()
    confiancas = collections.Counter()
    empresas = collections.Counter()
    linhas = []

    try:
        with transaction.atomic():
            guarda_sombra()
            lavra = ler_lavra()

            if lavra is None:
                linhas.append("MOTIVO=SEM_LAVRA_VALIDA_NO_BANCO")
            else:
                etapa = "MAPA_PROPOSTA"
                if "proposta" not in lavra:
                    linhas.append("MOTIVO=CAMPO_PROPOSTA_AUSENTE_NA_LAVRA")
                else:
                    propostas = lavra["proposta"]
                    if not isinstance(propostas, dict):
                        raise ValueError("mapa_proposta")
                    universo = len(propostas)

                    etapa = "CICLOS_ATUAIS"
                    # Uma proposta malformada não deve impedir os demais casos.
                    validas = {
                        chave: p
                        for chave, p in propostas.items()
                        if isinstance(p, dict)
                    }
                    ciclos = ciclos_dos_destinos(validas)

                    for chave, proposta in propostas.items():
                        cid = None
                        try:
                            # Savepoint permite seguir após erro de consulta por caso.
                            with transaction.atomic():
                                if not isinstance(chave, str):
                                    raise ValueError("chave")
                                cid = int(chave)
                                if cid <= 0 or str(cid) != chave:
                                    raise ValueError("chave")
                                if not isinstance(proposta, dict):
                                    raise ValueError("proposta")

                                tipo_id = proposta.get("tipo_escala_id")
                                if tipo_id not in ciclos:
                                    raise ValueError("tipo_ausente")
                                ciclo = ciclos[tipo_id]
                                if not isinstance(ciclo, str):
                                    raise ValueError("ciclo")

                                colab = Colaborador.objects.only(
                                    "id", "empresa_id"
                                ).get(pk=cid)
                                empresa = colab.empresa_id
                                if empresa is None:
                                    raise ValueError("empresa_ausente")

                                conf = proposta.get("confianca")
                                if conf not in ("alta", "media", "baixa",
                                               "insuficiente"):
                                    raise ValueError("confianca")

                                classe = (
                                    "ciclica"
                                    if ciclo in TipoEscala.CICLICOS
                                    else "nao_ciclica"
                                )

                            classes[classe] += 1
                            confiancas[conf] += 1
                            empresas[empresa] += 1

                            if classe == "nao_ciclica":
                                erros += 1
                                linhas.append(
                                    "ERRO col%d empresa=%s "
                                    "motivo=JANELA_DO_DETECTOR_NAO_COMPROVADA"
                                    % (cid, empresa)
                                )
                        except Exception:
                            erros += 1
                            if cid is not None and cid > 0:
                                linhas.append(
                                    "ERRO col%d motivo=CASO_NAO_MEDIDO" % cid
                                )
                            else:
                                linhas.append("ERRO motivo=CHAVE_INVALIDA")

                    linhas.append(
                        "CLASSIFICADAS ciclicas=%d nao_ciclicas=%d"
                        % (classes["ciclica"], classes["nao_ciclica"])
                    )
                    for conf, total in sorted(confiancas.items()):
                        linhas.append(
                            "CONFIANCA %s=%d" % (conf, total)
                        )
                    for empresa, total in sorted(
                        empresas.items(), key=lambda item: str(item[0])
                    ):
                        linhas.append(
                            "EMPRESA %s propostas=%d" % (empresa, total)
                        )
                    linhas.append(
                        "EXPOSTOS=NAO_MEDIDO "
                        "motivo=JANELA_DO_DETECTOR_NAO_COMPROVADA"
                    )
    except Exception:
        erros += 1
        linhas.append("ERRO etapa=" + etapa)

    for linha in linhas:
        print(linha)
    print("CONTADOR=%d UNIVERSO=%d ERROS=%d" % (expostos, universo, erros))
    print("FIM_SONDA_NOTURNO")
    return 0 if erros == 0 else 1


sys.exit(main())
```

Para obter o número solicitado é preciso comprovar, por evidência externa à proposta guardada, qual foi o `hoje` passado à geração daquela lavra; a identificação desse valor é [não verificado]. [app/ponto/services/esmeril_espelho.py:215](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:215), [app/ponto/services/esmeril_espelho.py:241](/home/ronald/codex_ws/2fd98211a55bdcd21e22331f6257b4e2979a3372/app/ponto/services/esmeril_espelho.py:241).

perguntas de lei: 1

_7283 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
