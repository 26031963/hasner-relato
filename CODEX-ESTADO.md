# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 10:51** · publicado em **09/11/2026 10:51** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 10:51 · sessao f5bd0a6923b7 · - · - · origem=Ronald (tmux)

- **PROCEDENTE — T4 residual em bloco:** partindo de carimbo hoje/OK, uma nova execução pode calcular `FALHOU`, falhar ao gravar e terminar com o `echo` bem-sucedido, preservando o OK anterior. `conferir` aceita esse OK se ainda houver migrations. Cura: invalidar antes da execução e exigir sucesso da publicação; testar carimbo inicialmente OK, erro no bloco e falha de gravação. (`bin/sombra.sh:356,407,417,428,429,430,433,455,459`)

- **PROCEDENTE — incremental conserva cobertura mentirosa:** altera a janela e confirma a transação antes de atualizar o carimbo. Falha da prova ou máscara deixa o carimbo antigo; falha de `carimbar` também é ignorada pelo `echo` seguinte. Um carimbo anteriormente `completa/OK` continua descrevendo cobertura completa. Cura: invalidar antes da alteração, preservar explicitamente os insumos necessários e exigir a publicação final; selo cobrando invalidação e recusa após cada falha. (`bin/sombra.sh:210,214,216,218,224,225,228`; `bin/sombra_incremental.sh:61,66,68,77,81,85`; `app/core/sombra_cobertura.py:39,40,41`)

- **NÃO PROCEDE — fim do refazer como preservação de verde antigo:** invalidação é exigida antes do drop; a publicação final é o último comando da função, portanto seu rc se propaga. Falha mantém dia vazio/REFAZENDO. `--dump-agora` converge para a mesma invalidação antes do drop; `conferir` não grava o carimbo. (`bin/sombra.sh:110,259,264,265,275,276,328,331,332,436,459`)

- **NÃO PROCEDE — predicados iguais:** o placar exige dia hoje, status diferente de REFAZENDO e número presente; conferir exige dia hoje, status exatamente OK e migrations positivas, ignorando o número. Status vazio com número: placar mostra número, conferir recusa. Hoje/OK sem INCOMPLETA: placar mostra `?`, conferir pode aprovar. Dia vazio ou ontem são rejeitados pelos dois; REFEITA/DIVERGE/FALHOU são admitidos como medida pelo placar, mas não como ensaio verde por conferir. São perguntas diferentes; centralizar parser e definições explícitas de validade/prontidão evita apresentar duas regras como idênticas. (`bin/placar_code.sh:34,35,36,37`; `bin/sombra.sh:439,455,456,459`)

- **PROCEDENTE — terceiro predicado e parsing próprio:** cobertura aceita hoje com OK **ou REFEITA**, depois aplica tipo/base/janela, sem consultar existência do banco. Seu parser usa última ocorrência e remove espaços/aspas; placar usa última ocorrência literal, enquanto conferir concatena todas as ocorrências. Cura: parser comum e predicados nomeados para perguntas distintas. (`app/core/sombra_cobertura.py:18,19,20,21,34,35,37,39,42,45,47`; `bin/placar_code.sh:34,35,36`; `bin/sombra.sh:439`)

- **Leitores diretos operacionais:** incremental lê DIA/BASE/TIPO/STATUS; bloco lê DIA/DIVERGE; conferir lê DIA/STATUS/TIPO/JANELAS/BASE/INCREMENTAL_EM/DIVERGE/ERROS; placar lê DIA/STATUS/INCOMPLETA; cobertura lê DIA/STATUS/TIPO/JANELAS/BASE. (`bin/sombra.sh:210,211,224,356,427,439,441,443,444`; `bin/placar_code.sh:34,35,36`; `app/core/sombra_cobertura.py:35,37,39,42,45`)

- **Leitores indiretos, sem regra própria do carimbo:** deploy; verde do placar; portão da esteira em `trava.sh`; simulação de folha e diff de janela. Estes dois últimos também exigem cobertura. (`bin/deploy.sh:162`; `bin/placar_code.sh:169`; `bin/trava.sh:40,43,52`; `bin/simular_folha.sh:68,70`; `bin/diff_janela_he_total.sh:19,22`)

- **FILA — relógio:** HOJE é capturado antes da trava; incremental e publicação completa consultam novamente a data; cobertura calcula hoje por conta própria. O placar adiciona captura anterior à chamada posterior de conferir. Cura: data de referência única na borda adequada, passada aos leitores; travessia real da meia-noite **[nao verificado]**. (`bin/sombra.sh:83,211,328,464,466,468,470,477`; `app/core/sombra_cobertura.py:34`; `bin/placar_code.sh:39,169`)

- **NÃO PROCEDE — segundo escritor operacional localizado:** os quatro caminhos escrevem por `carimbar`; invalidação escreve DIA/STATUS/INCOMPLETA, completo escreve os três, incremental/bloco escrevem STATUS. Escritor externo/manual **[nao verificado]**. (`bin/sombra.sh:92,101,110,225,227,328,329,331,430,432`)

## 09/11 10:51 · sessao 86ce7ad8eff9 · - · - · origem=Ronald (tmux)

- **2a/2b: fechados no código e selos mordem a causa indicada.** Estragos `SUPERUSER` e default de schema grantable entram no mesmo ciclo: chave positiva, rc 5, recusa nomeada, ausência de execução e cura pela porta. O log fornecido registra falhas correspondentes; alterações exatas dos mutantes e execução independente **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:181`, `:183`, `:185`, `:186`, `:187`, `:189`, `:207`, `:208`)

- **4: guarda fechada; mordida pertinente.** `carimbar` propaga falhas e `refazer` aborta antes do drop; 9c cobra ausência de `dropdb`, rc, mensagem e temporários. **Ressalva do selo:** caso `mv` aponta `STAMP` para caminho ausente, mas compara o outro arquivo `RF/stamp`; essa asserção não prova preservação de um carimbo existente após falha final de `mv`. (`bin/sombra.sh:94`, `:98`, `:99`, `:101`, `:275`; `bin/tests/test_sonda_sem_grant.sh:396`, `:399`, `:408`, `:409`, `:410`, `:411`, `:413`)

- **6: fechado no pedido literal; mordida pertinente.** Os casos dia vazio, `REFAZENDO`, ontem e número vazio cobram `?`; o mutante registrado cai nessa comparação. Status vazio não foi incluído no selo. (`bin/placar_code.sh:37`; `bin/tests/test_sonda_sem_grant.sh:434`, `:436`, `:437`, `:438`, `:439`, `:440`, `:441`, `:442`)

- **10: falha silenciosa original fechada, identidade ainda PROCEDENTE.** Cleanup preserva rc e acusa container ainda encontrado; o selo mata o comportamento antigo por rc e argv. Porém o trap antecede a criação: se `run --name "$PGT"` falhar por colisão, `EXIT` chama `rm -f "$PGT"` sobre o container alheio. Cura: registrar o ID da criação bem-sucedida, remover somente esse ID e não remover após criação recusada. O selo não cobre colisão. (`bin/tests/test_sonda_sem_grant_pg.sh:39`, `:41`, `:42`, `:44`, `:48`, `:50`, `:76`, `:80`, `:82`, `:93`, `:95`)

- **Nome vazio: NÃO PROCEDE no fluxo atual.** A atribuição sempre contém prefixo literal e PID; a alteração de `PGT` no teste fica no subshell. **“Nome exato criado pelo selo”: não provado**, pois exatidão do nome não prova autoria. (`bin/tests/test_sonda_sem_grant_pg.sh:35`, `:72`, `:73`, `:74`)

- **Limite da verificação de limpeza:** `rm` falha sem exame direto e qualquer erro de `inspect` equivale a “ausente”; o fake não distingue ausência de falha da consulta. Falha efetiva do Docker **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:41`, `:42`, `:85`, `:86`, `:87`, `:88`)

## 09/11 10:50 · sessao 72a0c7209703 · - · - · origem=Ronald (tmux)

- **PROCEDENTE quanto à diferença de cobertura; cura automática ampliada pode ir à FILA.** `REVOKE ... ON SCHEMAS` cura o caso `$PGU → leitor`, mas a conferência conta defaults de **qualquer** concedente e também de `PUBLIC`; a porta só revoga defaults `FOR ROLE $PGU ... FROM $ROLE`. Isso vale para schemas, tabelas e sequências. (bin/sonda_provisionar.sh:125-132; bin/sonda_leitor_comum.sh:119-122)
- **Não há falso pronto nesse caso:** `default_escrita != 0` reprova o parser; leitura sai rc 5 e porta sai rc 1 antes de promover `.env_leitor`. Há bloqueio persistente sem autocura pela porta. (bin/sonda_leitor_comum.sh:132,145; bin/sonda_leitura.sh:187,129-135; bin/sonda_provisionar.sh:147-151,171)
- **Quem cura:** o código manda `REASSIGN/REVOKE a mao`; para defaults, a correção sugerida é um operador autorizado revogar no concedente/objeto/escopo original, para `leitor` ou `PUBLIC`. A realização disso em produção é **[nao verificado]**. (bin/sonda_provisionar.sh:150; bin/sonda_leitor_comum.sh:119-122)
- **Item 2b fechado no domínio testado:** novo estrago é `saas_user → leitor`, `USAGE ON SCHEMAS WITH GRANT OPTION`; a estrutura exige chave positiva, rc 5, chave na recusa, sonda não executada e recuperação pela porta. Outros concedentes e defaults para `PUBLIC` não têm casos nesse conjunto. (bin/tests/test_sonda_sem_grant_pg.sh:179-191,192-209)
- **Item 2a fechado estruturalmente:** estrago SUPERUSER chega à consulta real e às mesmas quatro cobranças; a porta escreve NOSUPERUSER. (bin/tests/test_sonda_sem_grant_pg.sh:156-159,182-189,207; bin/sonda_leitor_comum.sh:68; bin/sonda_provisionar.sh:105)

Mapa dos insumos SQL alterados:

| Estado/campo | Quem escreve | Quem lê |
|---|---|---|
| Default schema de `$PGU` para `leitor` | Porta: REVOKE. (bin/sonda_provisionar.sh:132,144) | Conferência `defaclobjtype='n'`, ACL/grantee/privilégio/grantable. (bin/sonda_leitor_comum.sh:119-122) |
| `rolsuper` | Porta NOSUPERUSER; fixture SUPERUSER. (bin/sonda_provisionar.sh:105; bin/tests/test_sonda_sem_grant_pg.sh:207) | Consulta `super`; parser e consumidores. (bin/sonda_leitor_comum.sh:68,132,145; bin/sonda_leitura.sh:187; bin/sonda_provisionar.sh:148) |
| `default_escrita` | Derivado da consulta, sem escritor materializado de catálogo com esse nome. (bin/sonda_leitor_comum.sh:119-122) | Parser; porta antes/depois; leitura; selo PG. (bin/sonda_leitor_comum.sh:132,145; bin/sonda_provisionar.sh:72,147-148; bin/sonda_leitura.sh:184-187; bin/tests/test_sonda_sem_grant_pg.sh:182-186) |
| `PGU/ROLE/SCHEMAS/PG/BANCO` | Constantes e `_destino`. (bin/sonda_leitor_comum.sh:8-15,22-25) | Geração SQL e execução psql da porta/conferência. (bin/sonda_provisionar.sh:51,125-132; bin/sonda_leitor_comum.sh:64,96,120) |
| `.env_leitor` / senha | Porta, temporário e promoção. (bin/sonda_provisionar.sh:168-171) | Porta lê senha; leitura valida/lê e passa ao container. (bin/sonda_provisionar.sh:77; bin/sonda_leitura.sh:173-178,234) |

## 09/11 10:39 · sessao c1c80cb6a02e · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · verificador · origem=Code

**Há achados PROCEDENTES; GREEN, mortes M1–M38 e resultado em produção permanecem [nao verificado].**

1. **ATO ÚNICO — NAO PROCEDE para par identificado; garantia delimitada.** Validação, varredura e O330 passam pelo envelope comum. As duas aplicações, julgamento posterior, carimbos e reconciliação ficam no mesmo savepoint. Sem par identificado, continua possível aplicar uma pergunta individualmente, inclusive com irmã já aplicada. `app/chamados/services/validacao.py:474`; `app/chamados/services/materializacao.py:1596`; `app/chamados/services/passivo_validado.py:532`; `app/chamados/services/materializacao.py:1344`; `app/chamados/services/materializacao.py:1361`; `app/chamados/services/materializacao.py:1431`.

   **PROCEDENTE — paridade sobre universo errado.** O julgamento posterior usa vizinhas sem excluir batidas retratadas. Uma S retratada às 01:30 pode reprovar S01/E02, embora o conjunto vivo alterne. **Cura:** usar o universo canônico de apuração e testar esse contracaso. `app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:184`; `app/chamados/services/materializacao.py:1367`; `app/ponto/turnos.py:2866`.

   O censo AST com apelidos encontrou os plantadores de Pergunta nos ramos hora/data-hora. Regularização externa e veredito do dia têm unidades próprias e permitem resultados parciais: **FILA**, auditoria separada. `app/chamados/services/materializacao.py:1017`; `app/chamados/services/materializacao.py:1098`; `app/chamados/services/regularizacao_ext.py:166`; `app/ponto/services/veredito_celula.py:320`.

2. **ROLLBACK — limpeza funciona, mas descarimbo vazio tem efeitos.** Após restaurar o banco, o código decide pelo lastro, descarimba quando reprovado, grava recusa nas duas e chama a devolução. `app/chamados/services/materializacao.py:1386`.

   | Campo | Resultado após o tratamento da falha |
   |---|---|
   | `materializada_em/por`, `via_resolucao` | Lastro reprovado: `None/None/''`; lastro aprovado: conserva valores restaurados. `app/chamados/services/materializacao.py:1391`; `app/chamados/models.py:3211`. |
   | `validada_em/por` | Limpos quando é hora declarada, sem materialização, com recusa; não há limpeza incondicional para qualquer tipo. `app/chamados/services/validacao.py:190`; `app/chamados/services/validacao.py:198`. |
   | `recusa_motivo` | Recebe a recusa conjunta, truncada em 240 caracteres. `app/chamados/services/materializacao.py:1393`; `app/chamados/services/materializacao.py:1181`. |

   **PROCEDENTE — `save` sem mudança não é inócuo.** Mesmo com os três campos vazios, `descarimbar` salva com `update_fields=['via_resolucao','materializada_em','materializada_por']`; isso agenda o beco e insere EventoCelula quando há célula, sem deduplicação nessa emissão. **Cura:** tornar o descarimbo vazio um no-op na porta dona, comparando os três campos. `app/chamados/models.py:3211`; `app/chamados/signals.py:225`; `app/chamados/signals.py:242`; `app/chamados/models.py:3248`; `app/ponto/portas/celula.py:891`.

   Esse `save` não chama diretamente LogAuditoria nem re-lavra o veredito da pergunta. A devolução acrescenta E1 quando remove validação e audita quando sua foto muda; a nota de recusa depende de mudança do motivo. O censo estático conta sítios, não quantas vezes esse `save` executa. `app/chamados/models.py:3206`; `app/chamados/services/validacao.py:208`; `app/chamados/services/validacao.py:224`; `app/chamados/services/materializacao.py:1195`; `app/core/censo_escritas.py:451`.

   **“2x = mesmo estado e mesma trilha” não está demonstrado.** Repetir diretamente o descarimbo pode acrescentar eventos; repetir o envelope depois da devolução encontra ambas sem validação e não reaplica o par. `app/chamados/models.py:3215`; `app/ponto/portas/celula.py:891`; `app/chamados/services/materializacao.py:1415`; `app/chamados/services/materializacao.py:1433`.

   **NAO PROCEDE quanto a perda comprovada de carimbo legítimo fora do fato:** essas linhas são excluídas da identificação e a própria porta recusa descarimbá-las; vias de identidade carimbadas são absolvidas pelo lastro. Outro caso legítimo perdido nesta população: **[nao verificado]**. `app/chamados/services/materializacao.py:72`; `app/chamados/models.py:3209`; `app/chamados/juizes.py:670`.

3. **TRAVAS — NAO PROCEDE a inversão entre os dois validadores.** Ambos começam pelo mesmo Colaborador, depois Disputas, Chamados e Perguntas, ordenados por PK; a pergunta é relida após travar. O330 antecipa o universo da competência. Prova concorrente com duas conexões: **FILA**, não executada. `app/chamados/services/materializacao.py:125`; `app/chamados/models.py:2677`; `app/chamados/models.py:2686`; `app/chamados/models.py:2699`; `app/chamados/models.py:2703`; `app/chamados/services/validacao.py:435`; `app/chamados/services/passivo_validado.py:435`.

4. **ESTADO/FECHAMENTO — dois achados PROCEDENTES.**

   - **C7 tem um segundo julgamento dentro de `fechar`.** `pode_fechar` retém espera, mas `fechar` verifica respostas/C6 sem consultá-lo. Com metade aceita e irmã retratada, a porta pode fechar apesar de C7 negar. **Cura:** consultar C7 sob trava; testar a chamada real a `fechar`. P2 só testa `pode_fechar`. `app/chamados/models.py:1863`; `app/chamados/models.py:1897`; `app/chamados/models.py:1907`; `app/chamados/models.py:1943`; `app/chamados/tests/test_bug_par_intervalo.py:427`.
   - **Espera confunde resposta com aceite.** Com ambas respondidas e nenhuma validada, o predicado já retorna espera. O explicador pula “aguarda validação”; “Aplicar declaração” retorna antes de validar a própria pergunta. **Cura:** exigir validação da própria metade para esse estado. `app/chamados/services/materializacao.py:109`; `app/chamados/services/materializacao.py:117`; `app/chamados/services/explicador.py:74`; `app/chamados/services/declaracao_texto.py:678`; `app/chamados/services/declaracao_texto.py:709`.

   | Leitor | O que efetivamente lê/diz |
   |---|---|
   | C6, painel e badge | Validação mata a pergunta; o crédito continua por perguntas vivas/verbos. `app/chamados/juizes.py:1153`; `app/chamados/services/raiox.py:72`; `app/chamados/services/verbos.py:34`. |
   | Fio | Aberta carrega perguntas; fechada só admite C6 vivas, ocultando a espera se o fechamento escapar. `app/chamados/services/fio.py:221`; `app/chamados/services/fio.py:238`. |
   | Explicador, declaração, API | Leem/publicam o predicado de espera. `app/chamados/services/explicador.py:74`; `app/chamados/services/declaracao_texto.py:484`; `app/api/views.py:1126`. |
   | Vigia de carimbo/O330 | Separam espera da dívida e do alvo reparável. `app/chamados/services/vigia_carimbo.py:109`; `app/chamados/services/passivo_validado.py:324`. |
   | `vigia_fio`, `o_que_segura` | Continuam por C6 e autoridades de alcance/dono; não tratam espera como fio vivo. `app/chamados/services/vigia_fio.py:164`; `app/folha/fora_da_folha.py:221`; `app/chamados/juizes.py:1322`. |
   | Crons | Beco consulta C7; alerta diário continua avisando disputas abertas. Não há reabertura automática por espera nesses caminhos. `app/chamados/services/becos.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:59`. |

5. **IDENTIDADE — NAO PROCEDE a derivação paralela na tag.** O identificador consulta pausas efetivas/DNA e se abstém por multiplicidade; a tag chama o mesmo predicado. `par_ambiguo` preserva o despacho avulso, não constitui prova de equivalência integral de estado/trilha com HEAD. `app/chamados/services/materializacao.py:77`; `app/chamados/services/materializacao.py:88`; `app/escala/models.py:1189`; `app/chamados/templatetags/chamados_tags.py:18`; `app/chamados/services/materializacao.py:1431`.

6. **TEXTO/TELA — catálogo e proximidade corrigidos; percurso incompleto.** “Colada” exige testemunha viva nos ±60s inclusivos compartilhados com a porta; espera tem entrada própria. **NAO PROCEDE** a duplicação desses predicados. `app/chamados/services/materializacao.py:1357`; `app/ponto/registro_batida.py:94`; `app/ponto/registro_batida.py:115`; `app/chamados/catalogo/recusa.py:25`.

   **PROCEDENTE — aviso some do detalhe após recarga.** Os dois includes existem, mas o contexto do detalhe exclui a metade validada por C6. P5 injeta a pergunta diretamente. **Cura:** incluir espera no filtro de apresentação e testar GET real. `app/templates/chamados/partials/_pergunta_card.html:29`; `app/templates/chamados/partials/modal_fio.html:160`; `app/chamados/services/acoes_chamado.py:348`; `app/chamados/tests/test_bug_par_intervalo.py:486`.

7. **SELOS/SEMÂNTICA.** M7 e M35 agora têm asserções discriminantes estáticas; mortes executadas continuam **[nao verificado]**. **PROCEDENTE:** P6/M38 não provam a alteração específica v3: começam com carimbo preenchido, portanto passariam com a guarda anterior que só descarimbava nesse caso. **Cura:** mutante restaurando exatamente aquela guarda, cenário já vazio e contagem de saves/eventos/trilha em duas chamadas. `app/chamados/tests/test_bug_par_intervalo.py:280`; `app/chamados/tests/test_bug_par_intervalo.py:498`; `app/chamados/tests/test_bug_par_intervalo.py:508`; `app/chamados/tests/mutantes_bug_par_intervalo.py:131`.

   **FILA — relógio e dia civil/turno.** Há `now()` por metade no carimbo/aprovação; passar um instante do ato eliminaria essa dispersão. As derivações continuam pelos helpers existentes; contraprova de fuso ou competência neste dado: **[nao verificado]**. `app/chamados/services/materializacao.py:1253`; `app/chamados/services/materializacao.py:1373`; `app/chamados/services/materializacao.py:1023`; `app/chamados/services/materializacao.py:1351`.

8. **ESCRITORES/LEITORES adicionais dos campos centrais**, além dos consumidores acima:

   | Campo | Escritores / leitores fora do diff |
   |---|---|
   | Resposta e `respondida_em` | Resposta humana/automática/trilha; carimbo preenche ausente. Leitura pela derivação do slot. `app/chamados/services/disputa_emissao.py:1464`; `app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1671`; `app/chamados/models.py:3134`; `app/chamados/juizes.py:339`. |
   | `validada_em/por` | Validação, devolução, nova resposta, preenchimento pelo carimbo e reabertura. `app/chamados/services/validacao.py:134`; `app/chamados/services/validacao.py:202`; `app/chamados/services/disputa_emissao.py:1477`; `app/chamados/models.py:3137`; `app/chamados/models.py:1565`. |
   | Materialização/via | Portas de carimbo/descarimbo, lote, encerramento por fato e relavra; leitores C6/lastro/lavra persistida. `app/chamados/models.py:3100`; `app/chamados/models.py:3267`; `app/chamados/models.py:3278`; `app/chamados/services/relavra_via.py:125`; `app/chamados/juizes.py:645`; `app/chamados/veredito_lavrado.py:187`. |
   | Recusa | Identidade, pendência, recusa canônica/humana e escape; leitores catálogo e devolução. `app/chamados/models.py:3191`; `app/chamados/services/materializacao.py:940`; `app/chamados/services/validacao.py:605`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/catalogo/recusa.py:92`; `app/chamados/services/validacao.py:195`. |
   | `contexto_json` | Portas anotar/renovar/retirar/fundir; marco faltante lê seu conteúdo. `app/chamados/services/contexto.py:76`; `app/chamados/services/contexto.py:102`; `app/chamados/services/contexto.py:125`; `app/chamados/services/contexto.py:161`; `app/chamados/juizes.py:508`. |
   | Batida: instante/tipo/origem/vínculo | `registrar_batida` e `anotar`; leitores tolerância, vizinhança e lastro. `app/ponto/registro_batida.py:104`; `app/chamados/services/materializacao.py:1038`; `app/ponto/registro_batida.py:94`; `app/chamados/services/materializacao.py:169`; `app/chamados/juizes.py:732`. |

   Exaustividade de **todos** os leitores transitivos de cada campo/função na árvore: **[nao verificado]**.

## 09/11 10:37 · sessao 54312d58dc6d · - · - · origem=Ronald (tmux)

- **PROCEDENTE — teste da alteração específica v3 insuficiente.** P6 começa com `materializada_em` preenchido; restaurar a guarda anterior `materializada_em is not None and not lastro` continuaria satisfazendo esse teste. M38 inverte o lastro, sem testar o descarimbo já vazio. Acrescentar cenário vazio, captura dos efeitos de `save` e repetição 2x com contagem de trilha. `app/chamados/tests/test_bug_par_intervalo.py:508`; `app/chamados/tests/test_bug_par_intervalo.py:669`; `app/chamados/tests/test_bug_par_intervalo.py:529`; `app/chamados/tests/mutantes_bug_par_intervalo.py:131`.

- **PROCEDENTE — universo da paridade inclui retratadas.** O novo ato consulta `_vizinha_mesmo_tipo`, que busca `Batida` sem excluir retratadas, e recusa incondicionalmente a testemunha encontrada. Contraexemplo estático: sequência viva E18:32/S01:00/E02:00/S06:30 mais S retratada01:30 faz a saída encontrar S01:30 e recusar o conjunto. Cura no helper, usando `batidas_apuraveis`, com RED desse cenário. Ocorrência em produção [nao verificado]. `app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:184`; `app/chamados/services/materializacao.py:1367`; `app/ponto/turnos.py:2866`.

- **NAO PROCEDE — M7/M35 continuam sem discriminação estática.** M7 agora encontra DNA com uma pausa e quatro perguntas, exigindo `par_ambiguo`; M35 precisa produzir o aviso HTML exato uma vez no modal, e o runner troca também o filtro registrado. Mortes efetivamente executadas [nao verificado]. `app/chamados/tests/test_bug_par_intervalo.py:280`; `app/chamados/tests/test_bug_par_intervalo.py:288`; `app/chamados/tests/test_bug_par_intervalo.py:493`; `app/chamados/tests/test_bug_par_intervalo.py:498`; `app/chamados/tests/mutantes_bug_par_intervalo.py:230`.

- **NAO PROCEDE — nova cópia literal da proximidade/identidade.** Porta e ato leem `batida_mesmo_tipo_na_tolerancia`; identidade consulta `escala_vigente → vinculo_do_dia`, `pausas_do_dia` e mapa canônico existente. A guarda pós-par reutiliza o helper antigo; isso não elimina o defeito de universo acima. `app/ponto/registro_batida.py:115`; `app/chamados/services/materializacao.py:1357`; `app/chamados/services/materializacao.py:87`; `app/escala/servico_jornada.py:57`; `app/escala/models.py:1197`; `app/chamados/juizes.py:882`.

- **FILA — relógio por metade.** `_carimbar_aplicacao` lê `now()` internamente e é chamado para cada metade; aprovação também lê por escrita. A expressão foi extraída do escritor anterior segundo o diff fornecido; contraprova temporal neste dado [nao verificado]. Passar um instante do ato seria a cura estrutural. `app/chamados/services/materializacao.py:1253`; `app/chamados/services/materializacao.py:1373`; `app/chamados/services/materializacao.py:1023`.

Inventário de escritores fora e dentro do diff:

| Campo | Escritores |
|---|---|
| `materializada_em/por` | `_escrever_carimbo`, chamado pelas portas; descarimbo em lote. `app/chamados/models.py:3100`; `app/chamados/models.py:3267` |
| `via_resolucao` | Mesmas portas, mais encerramento por fato e relavra. `app/chamados/models.py:3278`; `app/chamados/services/relavra_via.py:125` |
| `validada_em/por` | Validar, devolver, responder novamente, preencher ausente no carimbo. `app/chamados/services/validacao.py:134`; `app/chamados/services/validacao.py:202`; `app/chamados/services/disputa_emissao.py:1477`; `app/chamados/models.py:3137` |
| `resposta_colab/respondida_em` | Resposta humana, automática, recuperação pela trilha; carimbo preenche instante ausente. `app/chamados/services/disputa_emissao.py:1464`; `app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1671`; `app/chamados/models.py:3134` |
| `recusa_motivo` | Identidade, pendência do plantio, recusa canônica, limpeza do par, recusa humana, escape da resposta. `app/chamados/models.py:3191`; `app/chamados/services/materializacao.py:940`; `app/chamados/services/materializacao.py:1193`; `app/chamados/services/materializacao.py:1378`; `app/chamados/services/validacao.py:605`; `app/chamados/services/disputa_emissao.py:1494` |
| `contexto_json` | Portas anotar/renovar/retirar/fundir. `app/chamados/services/contexto.py:76`; `app/chamados/services/contexto.py:102`; `app/chamados/services/contexto.py:125`; `app/chamados/services/contexto.py:161` |

A existência das asserções não certifica GREEN, RED anterior ou morte dos mutantes: [nao verificado].

## 09/11 10:37 · sessao 30e4a759213b · - · - · origem=Ronald (tmux)

**PROCEDENTE — C7 duplicado na porta de fechamento.** `pode_fechar()` agora retém espera, mas `fechar()` continua julgando apenas respostas sem veredito e perguntas vivas; não consulta C7. Com primeira metade validada e irmã retratada, C7 recusa e `fechar(user)` pode gravar fechamento. Cura: porta consultar C7 sob trava; RED chamar `fechar`, além de `pode_fechar`. `app/chamados/models.py:1863`, `app/chamados/models.py:1897`, `app/chamados/models.py:1907`, `app/chamados/models.py:1943`. O novo P2 só verifica `pode_fechar`. `app/chamados/tests/test_bug_par_intervalo.py:427`.

**PROCEDENTE — espera confunde resposta com aceite.** O predicado não exige validação da própria pergunta quando identifica par. Ambas respondidas, nenhuma validada: ambas viram `aguardando_par`, e o explicador deixa de oferecer validar a própria resposta. Cura: exigir `p.validada_em` para espera de metade aceita. `app/chamados/services/materializacao.py:109`, `app/chamados/services/materializacao.py:117`, `app/chamados/services/explicador.py:74`, `app/chamados/services/explicador.py:85`.

Isso também bloqueia portas: declaração do colaborador retorna antes de permitir alterar resposta; “Aplicar declaração” retorna antes de validar a própria pergunta já respondida. `app/chamados/services/declaracao_texto.py:382`, `app/chamados/services/declaracao_texto.py:424`, `app/chamados/services/declaracao_texto.py:678`, `app/chamados/services/declaracao_texto.py:709`.

**PROCEDENTE — aviso desaparece do detalhe após recarga.** O include existe, mas o contexto do detalhe seleciona apenas C6 vivas; a metade aceita é C6 morta. O retorno HTMX mostra aviso porque renderiza a pergunta diretamente, enquanto recarregar exclui aquela pergunta. Cura: filtro de apresentação incluir o predicado de espera. `app/chamados/services/acoes_chamado.py:348`, `app/chamados/juizes.py:1153`, `app/templates/chamados/detalhe_local.html:309`, `app/chamados/views.py:399`, `app/chamados/views.py:404`. P5 injeta pergunta diretamente e não testa essa montagem. `app/chamados/tests/test_bug_par_intervalo.py:487`.

**NÃO PROCEDE — templatetag derivando identidade paralela.** A tag chama o mesmo predicado; cartão e modal incluem o mesmo fragmento. `app/chamados/templatetags/chamados_tags.py:18`, `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/modal_fio.html:160`. A identidade lê pausas efetivas da escala; essa autoridade lê DNA da célula antes do template. `app/chamados/services/materializacao.py:86`, `app/escala/models.py:1189`, `app/escala/models.py:1197`, `app/escala/models.py:1207`.

Inventário compacto dos demais leitores:

| Leitor | Efeito |
|---|---|
| C6 / consulta viva | Aceite mata pergunta; SQL também a exclui. `app/chamados/juizes.py:1153`, `app/chamados/models.py:3362`. |
| Painel/badge | Crédito usa consulta viva; irmã não validada determina Cobrar/Validar. `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133`, `app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`. |
| Fio | Disputa aberta carrega perguntas; fechada só admite C6 vivas, ocultando aceite pendente se fechamento escapar. `app/chamados/services/fio.py:221`, `app/chamados/services/fio.py:238`. |
| Explicador | Espera antes de estado; fechamento por C7. `app/chamados/services/explicador.py:74`, `app/chamados/services/explicador.py:144`. |
| Projeção/API | Publicam espera e retiram aceitação. `app/chamados/services/declaracao_texto.py:484`, `app/api/views.py:1126`. |
| Vigia de carimbo/O330 | Espera sai da dívida e do alvo reparável. `app/chamados/services/vigia_carimbo.py:109`, `app/chamados/services/vigia_carimbo.py:133`, `app/chamados/services/passivo_validado.py:324`. |
| Vigia do fio | Continua julgando fio por C6; só importa espera na abertura de classes da porta. `app/chamados/services/vigia_fio.py:164`, `app/chamados/services/vigia_fio.py:370`, `app/chamados/services/vigia_fio.py:462`. |
| `o_que_segura` | Lê `porta_alcancavel_do_dia`, alimentada por C6 e verbos do painel. `app/folha/fora_da_folha.py:217`, `app/folha/fora_da_folha.py:221`, `app/chamados/juizes.py:1322`, `app/chamados/juizes.py:1396`. |
| Alerta diário | Toda disputa aberta segue recebendo aviso; não distingue espera. `app/ponto/management/commands/alertar_disputas_pendentes.py:33`, `app/ponto/management/commands/alertar_disputas_pendentes.py:59`. |
| Auditor I18 | Só acusa recusa vazia; espera normal possui código e não entra. `app/chamados/management/commands/auditar_invariantes_chamados.py:1075`, `app/chamados/services/materializacao.py:1436`. |

Se irmã sair do fato, o vigia poderá classificar o dia como sem fio, passando pelos outros donos; alarme efetivo depende da célula e desses donos **[nao verificado]**. `app/chamados/services/vigia_fio.py:416`, `app/chamados/services/vigia_fio.py:430`.

## 09/11 10:36 · sessao 463d4de49e19 · - · - · origem=Ronald (tmux)

**PROCEDENTE — paridade inclui batidas retratadas.** O pós-par consulta `_vizinha_mesmo_tipo` após ambas as escritas, mas esse helper lê `Batida.objects` sem excluir `retratada_em`; uma S retratada às 01:30 reprova S01/E02 mesmo quando o conjunto vivo alterna. Cura: consumir `batidas_apuraveis`, acrescentando RED com retratada entre metades. `app/chamados/services/materializacao.py:1366`; `app/chamados/services/materializacao.py:178`; `app/chamados/services/materializacao.py:184`; autoridade efetiva: `app/ponto/turnos.py:2866`. Ocorrência em produção: [nao verificado].

**NAO PROCEDE — divisão transacional para par identificado.** Validação direta e varredura chegam a `_materializar_uma`; O330 também, com inclusão explícita dos carimbos sem lastro. O envelope identifica o par, espera ambas respondidas/validadas e chama `_materializar_par`; as duas escritas, julgamento posterior, carimbos e reconciliação ficam no mesmo savepoint. `app/chamados/services/validacao.py:469`; `app/chamados/services/validacao.py:474`; `app/chamados/services/materializacao.py:1596`; `app/chamados/services/passivo_validado.py:532`; `app/chamados/services/materializacao.py:1414`; `app/chamados/services/materializacao.py:1433`; `app/chamados/services/materializacao.py:1344`; `app/chamados/services/materializacao.py:1361`; `app/chamados/services/materializacao.py:1373`; `app/chamados/services/materializacao.py:1380`.

A garantia tem universo delimitado: quando identidade retorna `None`, aplica-se uma pergunta avulsa. Portanto, o código admite plantio individual para intervalo sem par elegível; isso inclui irmã já materializada e casos ambíguos, preservando o caminho avulso. `app/chamados/services/materializacao.py:73`; `app/chamados/services/materializacao.py:77`; `app/chamados/services/materializacao.py:1431`; teste explícito da irmã aplicada: `app/chamados/tests/test_bug_par_intervalo.py:725`.

**NAO PROCEDE — inversão entre dois validadores dessas metades.** Ambos adquirem primeiro a mesma linha do Colaborador, depois disputas, chamados e perguntas; os grupos são ordenados por PK. A validação relê a pergunta após travar. O330 trava o conjunto da competência antes dos dias. `app/chamados/services/materializacao.py:125`; `app/chamados/services/materializacao.py:134`; `app/chamados/models.py:2677`; `app/chamados/models.py:2686`; `app/chamados/models.py:2699`; `app/chamados/models.py:2703`; `app/chamados/services/validacao.py:434`; `app/chamados/services/validacao.py:435`; `app/chamados/services/passivo_validado.py:435`.

**FILA — prova concorrente.** O teste confere argumentos das travas, sem duas conexões concorrentes; ausência operacional de deadlock é [nao verificado]. `app/chamados/tests/test_bug_par_intervalo.py:605`.

**Censo de plantadores:** os únicos consumidores de `registrar_batida` cujo plantio recebe `PerguntaDisputa` são os ramos hora e data/hora; ambos chegam pelo envelope comum. `app/chamados/services/materializacao.py:1017`; `app/chamados/services/materializacao.py:1098`; `app/chamados/services/materializacao.py:1477`.

Demais consumidores encontrados por AST, considerando nomes importados/apelidos e chamadas por atributo: APIs de chão (`app/api/views.py:684`, `app/api/views_core.py:681`), exceção (`app/chamados/services/acoes_chamado.py:44`), regularização externa (`app/chamados/services/regularizacao_ext.py:77`), justificativas (`app/ponto/services/justificativa.py:188`, `app/ponto/services/justificativa.py:361`, `app/ponto/views.py:1815`), triagem (`app/ponto/services/triagem_batida.py:701`) e veredito do dia (`app/ponto/services/veredito_celula.py:322`). Instrumento PWA: `app/ponto/management/commands/testar_pwa.py:184`, `:198`, `:219`, `:228`, `:229`.

**FILA — unidades externas parcialmente aplicáveis.** Regularização externa continua após falha de metade e pode marcar lote materializado; veredito do dia acumula erro por marco e só reverte se nenhuma ação ocorreu. Não são o aceite das duas PerguntaDisputa. `app/chamados/services/regularizacao_ext.py:166`; `app/chamados/services/regularizacao_ext.py:191`; `app/chamados/services/regularizacao_ext.py:197`; `app/ponto/services/veredito_celula.py:320`; `app/ponto/services/veredito_celula.py:342`.

## 09/11 10:29 · sessao cbbbf78661b6 · SONDA-SEM-GRANT · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 10:26 · sessao 415d564f876f · - · - · origem=Ronald (tmux)

T2 está implementado: o selo executa a conferência real, exige a chave certa positiva e recusa rc 5; o primeiro estrago é INSERT, suficiente para detectar `AND false` nas subconsultas de `esc_tab` (`bin/tests/test_sonda_sem_grant_pg.sh:108`, `:133`, `:136`, `:137`, `:144`). A execução verde e os “16/16 mortos” permanecem **[nao verificado]**: o usuário não forneceu `sonda_v3_mut.log`, RED ou GREEN, e os próprios selos só apontam para esses arquivos (`bin/tests/test_sonda_sem_grant.sh:32`; `bin/tests/test_sonda_sem_grant_pg.sh:21`).

**PROCEDENTE — cobertura semântica ainda incompleta.** Há contraprovas estáticas adicionais aos mutantes alegados:

- Trocar a medida de `super` por constante zero (`bin/sonda_leitor_comum.sh:68`) não encontra caso de leitor superuser: o falso devolve `super=0`, e os estragos PG não incluem esse atributo (`bin/tests/test_sonda_sem_grant.sh:62`, `:129`; `bin/tests/test_sonda_sem_grant_pg.sh:144`). Sobrevivência executada **[nao verificado]**. Conserto mínimo: conceder SUPERUSER na fixture e exigir `super>0`, rc 5 e ausência de execução da sonda.
- Remover `is_grantable` apenas do ramo de defaults de schema (`bin/sonda_leitor_comum.sh:122`) escapa da cobrança textual, que verifica somente o ramo SELECT, e dos estragos PG, que não incluem defaults de schema (`bin/tests/test_sonda_sem_grant.sh:214`; `bin/tests/test_sonda_sem_grant_pg.sh:144`). Sobrevivência executada **[nao verificado]**. Conserto mínimo: caso `ALTER DEFAULT PRIVILEGES ... GRANT USAGE ON SCHEMAS ... WITH GRANT OPTION`, exigindo `default_escrita>0`.

Sete estragos PG já exercitam comportamento previsto na volta 2: INSERT, UPDATE de coluna, USAGE de sequência, CREATE de schema, TEMP de PUBLIC, membership e default INSERT (`bin/tests/test_sonda_sem_grant_pg.sh:144`, `:145`, `:149`, `:151`, `:153`, `:154`, `:155`; predicados atuais anteriores ao acréscimo de `is_grantable`: `bin/sonda_leitor_comum.sh:103`, `:108`, `:112`, `:115`, `:118`, `:121`, `:123`). Resultado executado com código v2 **[nao verificado]**. Os seis GRANT OPTION e o override por banco são os casos dirigidos às novidades T1/T3 (`bin/tests/test_sonda_sem_grant_pg.sh:146`, `:147`, `:148`, `:150`, `:152`, `:156`, `:157`).

**NÃO — trap ausente no erro normal.** O trap é instalado antes do nascimento do container e cobre as saídas explícitas por falha de criação, readiness, pg_hba e âncora (`bin/tests/test_sonda_sem_grant_pg.sh:37`, `:38`, `:44`, `:46`, `:53`, `:57`, `:82`). **PROCEDENTE, restrito:** a limpeza ignora falha de `docker stop` e não verifica remoção; vazamento nessa condição **[nao verificado]** (`bin/tests/test_sonda_sem_grant_pg.sh:37`). Conserto mínimo: fallback por nome exato com `docker rm -f`, conservando o rc original e tornando falha da limpeza visível.

Não identifiquei SQL inequivocamente inválida somente pela leitura; execução em PG17 **[nao verificado]**. O alvo declarado sai de `IMAGEM=postgres:17-alpine` (`bin/db_teste.sh:23`; `bin/tests/test_sonda_sem_grant_pg.sh:31`). Bash `-n` passou; execução dos testes não realizada **[nao verificado]**.

Os selos não estão órfãos: a régua descobre `tests/test_*.sh` e bloqueia na falha (`bin/regua.sh:138`, `:140`, `:143`). A conferência é produzida pelo comum e consumida pelos dois caminhos e pelo selo PG (`bin/sonda_leitor_comum.sh:63`; `bin/sonda_leitura.sh:184`, `:187`; `bin/sonda_provisionar.sh:72`, `:144`, `:145`; `bin/tests/test_sonda_sem_grant_pg.sh:108`, `:110`).

## 09/11 10:26 · sessao eb32a108d276 · - · - · origem=Ronald (tmux)

T1 — **NAO permanece como falha de medição**: `is_grantable` entra nas duas parcelas de `esc_tab` e em `esc_seq`, `esc_schema`, `esc_banco`, `default_escrita`; todas passam pelo mesmo parser. Referências: `bin/sonda_leitor_comum.sh:103`, `:108`, `:112`, `:115`, `:118`, `:121`, `:122`, `:131-149`. Resultado efetivo em PG17: [nao verificado].

T3 — **NAO permanece estaticamente**: `ro_banco` conta configuração do leitor no banco destino diferente de `on`; a porta remove esse ajuste e exige o mesmo `leitor|on` usado pela leitura antes de publicar a credencial. Referências: `bin/sonda_leitor_comum.sh:74-78`, `:132`, `:167-171`; `bin/sonda_provisionar.sh:108`, `:154-168`; `bin/sonda_leitura.sh:193-196`. Resultado efetivo: [nao verificado].

**ESMERIL da conferência — sem divergência encontrada entre os consumidores operacionais.** Todas as chaves são produzidas por `_q_conferencia`; leitura e porta usam `_conferencia_pronta`, que exige quatro presenças iguais a `1`, demais valores iguais a `0`, recusando chave ausente. Referências: `bin/sonda_leitor_comum.sh:66-149`; `bin/sonda_leitura.sh:184-187`; `bin/sonda_provisionar.sh:72`, `:144-145`.

| Campos | Produtor da medida |
|---|---|
| `role`, `super`, `poderes`, `login` | `bin/sonda_leitor_comum.sh:67-70` |
| `ro`, `ro_banco` | `bin/sonda_leitor_comum.sh:71-78` |
| `connect`, `usage_falta` | `bin/sonda_leitor_comum.sh:79-83` |
| `tab_falta`, `seq_falta`, `default_falta` | `bin/sonda_leitor_comum.sh:84-98` |
| `esc_tab`, `esc_seq`, `esc_schema`, `esc_banco` | `bin/sonda_leitor_comum.sh:99-118` |
| `default_escrita`, `membro`, `dono` | `bin/sonda_leitor_comum.sh:119-125` |

O selo falso replica os mesmos valores esperados e verifica trechos por chave; o selo PG usa a consulta e o parser comuns, e exige chave positiva nos estragos. Referências: `bin/tests/test_sonda_sem_grant.sh:62`, `:129`, `:184-219`; `bin/tests/test_sonda_sem_grant_pg.sh:107-111`, `:130-157`.

**Escritores/leitores:**

- Role, senha e ajustes de sessão: porta escreve em `bin/sonda_provisionar.sh:104-109`; conferência lê em `bin/sonda_leitor_comum.sh:67-78`, `:123-125`; login verifica em `:167-171`.
- ACL/defaults: porta escreve em `bin/sonda_provisionar.sh:119-140`; conferência lê em `bin/sonda_leitor_comum.sh:79-125`.
- `.env_leitor`: porta lê senha em `bin/sonda_provisionar.sh:75-78`, grava temporário/promove em `:165-168`; leitura lê/verifica em `bin/sonda_leitura.sh:173-179`, Docker recebe em `:234`.
- `POSTGRES_DB/USER/PASSWORD/HOST/PORT`: Docker recebe em `bin/sonda_leitura.sh:234-236`; Django lê em `app/config/settings/saas.py:86-90`; sombra sobrescreve banco/host/porta em `app/config/settings/sombra.py:49-51`.
- Segundo escritor operacional desses estados fora da porta: não encontrado na pesquisa; inexistência absoluta [nao verificado]. Escritores dos selos usam raiz temporária: `bin/tests/test_sonda_sem_grant.sh:86-94`; `bin/tests/test_sonda_sem_grant_pg.sh:77-83`, `:175-179`.

**Limitação verificável do selo PG:** `docker run` da sonda vira no-op; assim ele não exercita conexão Django nem origem IP do container da sonda. Referência: `bin/tests/test_sonda_sem_grant_pg.sh:95`. Impacto em produção: [nao verificado].

**Não sustentar achado pela ausência de REVOKE explícito de coluna:** a semântica real de propagação de `REVOKE ALL ON TABLES` não foi executada nesta auditoria; portanto alegar que os casos de coluna necessariamente ficam vermelhos seria [nao verificado]. A porta usa esse comando em `bin/sonda_provisionar.sh:121`; o selo exige cura em `bin/tests/test_sonda_sem_grant_pg.sh:139-147`.

## 09/11 10:25 · sessao 305d1a7821ea · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

1. **Cura na origem:** [materializacao.py:1388](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/services/materializacao.py:1388). A antiga guarda lia o carimbo como **fato de escrita a desfazer**, não como vitalidade. Agora o juiz do lastro decide a limpeza, executada por `descarimbar`. O contrato continua estrito: detecta comparações cruas com `None`; a comparação saiu do chamador, sem nova allowlist (`chamados/tests/test_contract_leitores_vitalidade.py:315`).

   **ESCREVE:** `PerguntaDisputa.descarimbar`, pelo dono `_escrever_carimbo`, limpa `materializada_em`, `materializada_por` e `via_resolucao` juntos (`chamados/models.py:3206`, `:3100`). **LÊ:** juiz do lastro (`chamados/juizes.py:645`); devolução da validação (`chamados/services/validacao.py:163`). `_materializar_uma` chama o ato alterado (`chamados/services/materializacao.py:1439`).

2. **M7:** novo caso em [test_bug_par_intervalo.py:273](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/tests/test_bug_par_intervalo.py:273). Quatro perguntas representam duas pausas declaradas; o DNA tem uma pausa para impedir que outra guarda esconda M7. Cobra exatamente `(None, 'par_ambiguo')` e o efeito avulso do HEAD: recusa por paridade, validação devolvida, nenhuma batida nova, irmãs e chão preservados (`chamados/tests/test_bug_par_intervalo.py:288`, `:294`).

   **ESCREVE a fixture:** `pergunta` e `responder` (`chamados/tests/test_bug_par_intervalo.py:81`, `:88`). **LÊ a identidade:** `identificar_par_intervalo`, consumido pela validação e pelo materializador (`chamados/services/materializacao.py:55`; `chamados/services/validacao.py:467`).

3. **M35:** o helper agora fornece `disputa_obj` e `pode_agir_map`, abre o ramo real do modal e cobra o `<div>` completo do aviso, sem projeção injetada (`chamados/tests/test_bug_par_intervalo.py:481`, `:498`).

   **ESCREVE o contexto:** helper do teste (`chamados/tests/test_bug_par_intervalo.py:487`); em produção, `fio.py:253`, `:258`, `:467`. **LÊ:** template real (`templates/chamados/partials/modal_fio.html:122`, `:160`), fragmento compartilhado (`templates/chamados/partials/_espera_par_intervalo.html:2`) e filtro (`chamados/templatetags/chamados_tags.py:16`).

4. **Mutantes:** ajuste da âncora M6 e inclusão de M38 em [mutantes_bug_par_intervalo.py:34](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-075842/app/chamados/tests/mutantes_bug_par_intervalo.py:34), `:131`. O runner lê essas definições e recompila em memória (`chamados/tests/mutantes_bug_par_intervalo.py:212`).

| Mutante | Teste discriminante | Asserção esperada | [nao verificado]
|---|---|---|
| M7 | `test_R7_multiplas_perguntas_nao_escolhem_primeiro_par` | Igualdade com `(None, 'par_ambiguo')`, linha 288. |
| M25 / M35 | respectivos testes P5 de cartão/modal | Aviso HTML presente exatamente uma vez, linha 498. | [nao verificado]
| M26 / M38 novo | `test_P6_O330_falha_com_carimbo_previo_descarimba_antes_de_devolver` | Ambas descarimbadas, linha 529; autor e via limpos, linhas 532–533. |
| M6 | `test_R4_lastro_da_segunda_metade_reprova_as_duas` | Retorno recusado, linha 211. |

Essas mortes por asserção nesta volta são **[nao verificado]**; não executei testes ou mutantes. [nao verificado]

**DECISAO-ABERTA:** C7 já está ligado ao predicado nesta cópia (`chamados/models.py:1861`). Nenhuma alteração de model foi necessária nesta volta. GREEN e certificação pelo Code/opus permanecem **[nao verificado]**.

Comandos individuais reservados ao Code:

```bash
DJANGO_SETTINGS_MODULE=config.settings.saas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_contract_leitores_vitalidade.LeitoresVitalidadeContract.test_ninguem_julga_vitalidade_por_carimbo_cru --noinput
DJANGO_SETTINGS_MODULE=config.settings.saas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_bug_par_intervalo.BugParIntervaloCol960.test_R7_multiplas_perguntas_nao_escolhem_primeiro_par --noinput
DJANGO_SETTINGS_MODULE=config.settings.saas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_bug_par_intervalo.BugParIntervaloCol960.test_P5_modal_le_espera_sem_projecao_injetada --noinput
DJANGO_SETTINGS_MODULE=config.settings.saas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_bug_par_intervalo.BugParIntervaloCol960.test_P6_O330_falha_com_carimbo_previo_descarimba_antes_de_devolver --noinput

PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M6
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M7
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M25
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M26
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M35
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo M38
```

Executei somente a verificação estática abaixo: **rc 0**, `py_compile` dos três Python tocados sem persistir bytecode e conferência de sintaxe/âncoras M1–M38 (`chamados/tests/mutantes_bug_par_intervalo.py:173`, `:188`).

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo --sintaxe
```

## 09/11 10:25 · sessao 936ff5f03d79 · - · - · origem=Ronald (tmux)

T4 — **PROCEDENTE residual; fecha apenas o caminho sem erro de arquivo.** A invalidação grava `SOMBRA_DIA=` e `SOMBRA_STATUS=REFAZENDO` antes do `dropdb`, sob a trava (`bin/sombra.sh:104`, `bin/sombra.sh:269`, `bin/sombra.sh:270`, `bin/sombra.sh:458`). Porém o script usa `set -uo pipefail`, sem `-e`, e não testa o retorno da invalidação; falha no `mv` do carimbo permite continuar para o `dropdb` com carimbo antigo verde (`bin/sombra.sh:45`, `bin/sombra.sh:97`, `bin/sombra.sh:269`). Conserto mínimo: `invalidar_carimbo || falhou ...`; fazer `carimbar` propagar falhas intermediárias e criar temporário na pasta do carimbo (`bin/sombra.sh:89`).

A invalidação normal é recusada por incremental, bloco, conferir e cobertura; placar e deploy delegam sua decisão a conferir (`bin/sombra.sh:204`, `bin/sombra.sh:350`, `bin/sombra.sh:453`, `app/core/sombra_cobertura.py:35`, `app/core/sombra_cobertura.py:37`, `bin/placar_code.sh:155`, `bin/deploy.sh:162`). Entretanto **PROCEDENTE semântica residual no placar**: ele continua apresentando o antigo `SOMBRA_INCOMPLETA=0` como medida do “carimbo de hoje”, mesmo após dia vazio/REFAZENDO (`bin/placar_code.sh:160`, `bin/placar_code.sh:161`). Conserto mínimo: publicar `?` quando o carimbo não for válido para essa medida (`bin/placar_code.sh:160`).

T5 — **NÃO, no caminho da cópia a corrida relatada está fechada.** O `flock` vem antes da leitura/geração da senha e permanece aberto até o `mv` (`bin/sonda_provisionar.sh:68`, `bin/sonda_provisionar.sh:69`, `bin/sonda_provisionar.sh:75`, `bin/sonda_provisionar.sh:79`, `bin/sonda_provisionar.sh:168`). A chamada do refazer segue sombra → porta (`bin/sombra.sh:458`, `bin/sombra.sh:321`, `bin/sombra.sh:246`). Nenhuma aquisição da trava da sombra consta no provisionador; inversão/autoe­spera nesse caminho: **NÃO** (`bin/sonda_provisionar.sh:41`, `bin/sonda_provisionar.sh:68`, `bin/sonda_provisionar.sh:153`). A trava da sombra evita reacquisição nos filhos pela marca herdada (`bin/sombra_trava.sh:22`, `bin/sombra_trava.sh:38`).

Relógio — **PROCEDENTE preexistente D3.** `HOJE` é capturado antes de aguardar a trava; a data da base é consultada novamente ao publicar, permitindo datas discordantes se a espera/execução cruzar meia-noite (`bin/sombra.sh:83`, `bin/sombra.sh:458`, `bin/sombra.sh:322`). A cobertura também consulta relógio quando o chamador omite `hoje` (`app/core/sombra_cobertura.py:34`, `bin/sombra.sh:236`). Conserto mínimo: capturar o dia depois da trava e passar a data explicitamente; cruzamento real em produção [nao verificado].

Inventário do carimbo:

| Campos | Quem escreve | Quem lê |
|---|---|---|
| `SOMBRA_DIA` | invalidação/completo (`bin/sombra.sh:104`, `bin/sombra.sh:322`) | incremental/bloco/conferir/cobertura (`bin/sombra.sh:204`, `bin/sombra.sh:350`, `bin/sombra.sh:433`, `app/core/sombra_cobertura.py:35`) |
| `SOMBRA_STATUS` | invalidação/incremental/completo/bloco (`bin/sombra.sh:104`, `bin/sombra.sh:221`, `bin/sombra.sh:325`, `bin/sombra.sh:426`) | incremental/conferir/cobertura (`bin/sombra.sh:218`, `bin/sombra.sh:433`, `app/core/sombra_cobertura.py:37`) |
| `SOMBRA_TIPO` | incremental/completo (`bin/sombra.sh:219`, `bin/sombra.sh:322`) | incremental/conferir/cobertura (`bin/sombra.sh:205`, `bin/sombra.sh:435`, `app/core/sombra_cobertura.py:39`) |
| `SOMBRA_BASE_DIA` | completo (`bin/sombra.sh:322`) | incremental/conferir/cobertura (`bin/sombra.sh:205`, `bin/sombra.sh:437`, `app/core/sombra_cobertura.py:45`) |
| `SOMBRA_JANELA_INI/FIM` | incremental/completo (`bin/sombra.sh:219`, `bin/sombra.sh:323`) | conferir/cobertura (`bin/sombra.sh:437`, `app/core/sombra_cobertura.py:42`) |
| `SOMBRA_DIVERGE` | completo (`bin/sombra.sh:323`) | bloco/conferir (`bin/sombra.sh:421`, `bin/sombra.sh:438`) |
| `SOMBRA_INCOMPLETA` | completo (`bin/sombra.sh:323`) | placar (`bin/placar_code.sh:160`) |
| `SOMBRA_INCREMENTAL_EM` | incremental (`bin/sombra.sh:220`) | conferir (`bin/sombra.sh:437`) |
| `SOMBRA_ERROS` | bloco (`bin/sombra.sh:425`) | conferir (`bin/sombra.sh:438`) |
| `DUMP`, `REFEITA`, `BLOCO` | completo/bloco (`bin/sombra.sh:322`, `bin/sombra.sh:324`, `bin/sombra.sh:424`) | leitor operacional [nao verificado] |
| `INCREMENTAL_S`, `BLOCO_NA_JANELA` | incremental (`bin/sombra.sh:220`, `bin/sombra.sh:221`) | leitor operacional [nao verificado] |
| `COMANDOS`, `ALARMES`, `FORA_DO_ENSAIO` | bloco (`bin/sombra.sh:424`, `bin/sombra.sh:425`) | leitor operacional [nao verificado] |

A gravação material dos campos passa por `carimbar`, que preserva os campos não substituídos (`bin/sombra.sh:89`, `bin/sombra.sh:91`, `bin/sombra.sh:94`, `bin/sombra.sh:97`). O selo de host testa a invalidação bem-sucedida contra carimbo verde, mas não cobre falha da escrita do carimbo (`bin/tests/test_sonda_sem_grant.sh:361`, `bin/tests/test_sonda_sem_grant.sh:362`, `bin/tests/test_sonda_sem_grant.sh:364`).

## 09/11 10:10 · sessao 86b79a2759f4 · FIO-E0-V4-E4-ESCRITORES · censo · origem=Code

**O número 16 confere para as portas de alteração do estado. Incluindo os nascimentos por `create`/`get_or_create`/inserção, como o pedido exige, são 22 portas produtivas: 16 + 2 criadores de Chamado + 3 de Disputa + 1 de Pergunta.** Há ainda o executor genérico `PerguntaDisputa.save` e uma exclusão restrita a ensaio, discriminados abaixo. A diferença não é uma porta nova da E4: é a inclusão dos nascimentos que o recorte histórico separava como identidade. (`app/docs/RELATO.md:242`; `app/chamados/models.py:833`; `app/chamados/models.py:1054`; `app/chamados/models.py:2822`; `app/chamados/services/disputa_emissao.py:474`; `app/chamados/services/disputa_emissao.py:1024`; `app/chamados/services/disputa_emissao.py:1299`; `app/chamados/models.py:3402`; `app/chamados/models.py:563`)

A classificação usa a E1: **decisão** é o ato preservado na sequência append-only; **efeito** é sua consequência, conferida contra o fato/lavra; **fica** é identidade, data, cache ou metadado do continente. Uma porta pode ter ramos diferentes, explicitados sem contá-la duas vezes. A E1 declara que os campos atuais são projeção até a E4; a tarefa exige conferir decisões e efeitos, sem escritor próprio. (`app/chamados/models.py:3828`; `app/chamados/models.py:3833`; `app/chamados/models.py:3840`; `app/chamados/services/tarefa_do_dia.py:118`; `app/chamados/services/tarefa_do_dia.py:140`; `app/docs/CORTES.md:426`)

A análise foi estática por AST, incluindo imports com apelido, imports relativos, módulos `__init__` e imports locais. Não houve importação da aplicação, consulta a banco/log nem escrita de arquivo. Completude perante despacho inteiramente dinâmico, SQL construído dinamicamente, templates e execução real: **[nao verificado]**. Nos chamadores abaixo, **P** significa código produtivo/operacional; **T** identifica testemunhas de teste, sem afirmar execução delas nem esgotar todas as fixtures de nascimento. **[nao verificado]** [nao verificado]

Para compactar os campos repetidos:

- **C** = `materializada_em`, `materializada_por`, `via_resolucao`. (`app/chamados/models.py:3117`)
- **R** = `resposta_colab`, `respondida_em`; **V** = `validada_em`, `validada_por`. (`app/chamados/models.py:2444`; `app/chamados/models.py:3366`)
- **L** = `veredito_lavrado`, `julgado_em`, `janela_expira_em`. (`app/chamados/veredito_lavrado.py:250`)
- **F** = `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`. (`app/chamados/models.py:1933`)
- **DP** = `DecisaoPergunta`; “sem DP próprio” não significa ausência de trilha nem ausência de decisão registrada pelo chamador. (`app/chamados/models.py:3836`; `app/chamados/services/disputa_emissao.py:1612`)

Leis/cortes conferidos para as decisões abaixo: L-002, L-007, L-051, L-061, L-073, L-080, L-085, L-088, L-110, L-111, L-113, L-117 e L-124; FIO-E0-V4 e LEI-TRANCA-E2-JUIZ. (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:92`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:100`; `app/docs/LEIS.md:107`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:137`; `app/docs/LEIS.md:151`; `app/docs/LEIS.md:138`; `app/docs/LEIS.md:145`; `app/docs/CORTES.md:426`; `app/docs/CORTES.md:427`)

## CENSO

escritores:

- `app/chamados/models.py:201` `ChamadoColaborador._gravar_estado` -- `status_local=novo`; ao terminar, `resolvido_em` recebe instante, `via_encerramento=via`, urgência é rebaixada; ao reviver, limpa resolução/via e restaura urgência; pode gravar `respondido_por`; grava `atualizado_em` -- **efeito** nas transições derivadas; **decisao** no ramo de transição humana. Persiste em `app/chamados/models.py:296`; sem DP próprio, com trilha `estado_chamado` e relavra L das perguntas atingidas. P: `_aplicar` (`app/chamados/models.py:450`), `analisar` (`app/chamados/models.py:806`), `reconciliar` (`app/chamados/models.py:654`), `cancelar` (`app/chamados/models.py:788`), `reabrir` (`app/chamados/models.py:1442`), `encerrar` (`app/chamados/models.py:1616`), `encerrar_por_competencia_trancada` (`app/chamados/models.py:1636`). T: `app/chamados/tests/test_hx_b7_trilha.py:32`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:206`; `app/chamados/tests/test_o261e_f1_codex_v5.py:89`.

- `app/chamados/models.py:1852` `DisputaSupervisao.fechar` -- F recebe instante, autor, texto e motivo; excepcionalmente carimba perguntas vivas -- **efeito** no fechamento normal, após nenhuma viva/resposta sem veredito; **decisao** nas exceções `admin_encerra`/`desligamento`, que já registram DP `SILENCIO` no mesmo atomic (`app/chamados/models.py:1915`; `app/chamados/models.py:1921`; `app/chamados/models.py:1933`). P: `admin_encerra` e `remover_motivo` (`app/chamados/models.py:1947`; `app/chamados/models.py:2069`); reconciliador (`app/chamados/reconciliador.py:412`; `app/chamados/reconciliador.py:612`; `app/chamados/reconciliador.py:1171`); foto (`app/api/foto_recebida.py:80`); desligamento (`app/colaboradores/services/desligamento.py:142`); entrada sincronizada (`app/ponto/services/entrada_sincronizada.py:59`); materialização (`app/chamados/services/materializacao.py:586`; `app/chamados/services/materializacao.py:1333`); emissão (`app/chamados/services/disputa_emissao.py:1281`; `app/chamados/services/disputa_emissao.py:1961`); tranca (`app/chamados/services/competencia_trancada.py:204`); beco (`app/chamados/services/becos.py:57`); comandos (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`). T: `app/chamados/tests/test_porta_disputa_ciclo.py:285`; o contrato e as exceções estão na própria porta (`app/chamados/models.py:1855`).

- `app/chamados/models.py:2123` `DisputaSupervisao.reabrir_admin` -- limpa F; incrementa `reaberturas_admin`; pode agregar `motivos` -- **decisao** no ato administrativo; **fica** o contador/metadado. Persiste em `app/chamados/models.py:2158`; sem DP próprio. P: `app/chamados/services/acoes_disputa.py:259`. T: `app/chamados/tests/test_disputa_transicoes.py:134`; `app/chamados/tests/test_disputa_transicoes.py:148`; `app/chamados/tests/test_disputa_transicoes.py:156`; `app/chamados/tests/test_disputa_transicoes.py:163`; `app/chamados/tests/test_disputa_transicoes.py:172`; `app/chamados/tests/test_disputa_transicoes.py:181`.

- `app/chamados/models.py:2160` `DisputaSupervisao.reabrir_sistema` -- limpa F -- **efeito**, religando o continente após a pergunta voltar a exigir ação. Persiste em `app/chamados/models.py:2174`; sem DP próprio. P: `app/chamados/models.py:1572`; `app/chamados/services/materializacao.py:686`; `app/chamados/services/validacao.py:218`; `app/chamados/services/relavra_via.py:261`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:165`. Chamador direto de teste não localizado: **[nao verificado]**.

- `app/chamados/models.py:3092` `PerguntaDisputa.carimbar` -- C recebe instante/autor/via; opcionalmente completa `respondida_em`, V e L -- **efeito** para o carimbo; **decisao** quando completa resposta/validação. Persiste em `app/chamados/models.py:3141`; sem DP próprio, inclusive nos ramos que completam assinaturas (`app/chamados/models.py:3119`). P: `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:595`; `app/chamados/models.py:3276`; `app/chamados/services/materializacao.py:561`; `app/chamados/services/materializacao.py:1277`; `app/chamados/services/materializacao.py:1431`; `app/chamados/services/disputa_emissao.py:1860`; `app/chamados/services/disputa_emissao.py:1934`. T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:725`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:738`; `app/chamados/tests/test_o261e_f2_fila.py:194`; `app/chamados/tests/test_o261e_f2_fila.py:205`; `app/chamados/tests/test_o261e_f2_fila.py:221`; `app/chamados/tests/test_o261e_f2_fila.py:228`.

- `app/chamados/models.py:3157` `PerguntaDisputa.carimbar_identidade` -- C recebe via terminal/autor/instante, ou preserva autor/instante anteriores; pode gravar `recusa_motivo` -- **efeito** da identidade/decisão aplicável; o ato humano que o determina é **decisao**. Persiste em `app/chamados/models.py:3179`; sem DP próprio. P: `fechar_por_admin` (`app/chamados/models.py:3053`), `remover_motivo` (`app/chamados/models.py:2058`), recusa/paridade (`app/chamados/services/materializacao.py:924`; `app/chamados/services/materializacao.py:1099`), duplicata (`app/chamados/services/fio.py:1350`). O chamador `remover_motivo` já registra DP `RETRATACAO` (`app/chamados/models.py:2053`); `Disputa.fechar` registra `SILENCIO` (`app/chamados/models.py:1921`). T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:744`; `app/chamados/tests/test_e1_v5_portas_backfill.py:166`; `app/chamados/tests/test_e1_volta2.py:131`.

- `app/chamados/models.py:3192` `PerguntaDisputa.descarimbar` -- C passa a `None/None/''`; recusa linha fora do fato; `salvar=False` só prepara memória -- **efeito**. Persiste em `app/chamados/models.py:3201`; sem DP próprio. P: `app/chamados/models.py:3287`; `app/chamados/models.py:3304`; `app/chamados/models.py:3311`; simulação sem persistência em `app/chamados/models.py:1506`; `app/chamados/services/relavra_via.py:246`. T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:758`; `app/chamados/tests/test_e1_v5_portas_backfill.py:168`; `app/chamados/tests/test_e1_v5_portas_backfill.py:176`.

- `app/chamados/models.py:3241` `PerguntaDisputa.descarimbar_em_lote` -- C passa a `None/None/''`, mais campos recebidos em `outros`; exclui vias fora do fato -- **efeito**. `QuerySet.update` em `app/chamados/models.py:3253`; sem DP próprio. P: `app/chamados/models.py:1565`; `app/chamados/services/disputa_emissao.py:1627`, cujo chamador registra DP antes do update (`app/chamados/services/disputa_emissao.py:1612`). T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:752`; `app/chamados/tests/test_e1_v13_ordem.py:90`; `app/chamados/tests/test_e1_v13_ordem.py:101`; `app/chamados/tests/test_e1_v13_ordem.py:106`.

- `app/chamados/models.py:3257` `PerguntaDisputa.encerrar_por_fato_no_espelho` -- somente `via_resolucao=fato_materializado` -- **efeito**. Persiste em `app/chamados/models.py:3265`; sem DP próprio. P: `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`. T: `app/chamados/tests/test_porta_chamado_e3.py:72`; `app/chamados/tests/test_porta_chamado_e3.py:74`; `app/chamados/tests/test_e1_v5_portas_backfill.py:170`.

- `app/chamados/services/validacao.py:127` `_carimbar_validacao` -- V recebe instante/autor -- **decisao**. Save em `app/chamados/services/validacao.py:137`; DP `VALIDACAO`, mesmo instante/atomic, em `app/chamados/services/validacao.py:139`. P: `_aceitar_a_folga` (`app/chamados/services/validacao.py:288`; `app/chamados/services/validacao.py:320`) e `validar_pergunta` (`app/chamados/services/validacao.py:435`). T: `app/chamados/tests/test_e1_entradas.py:98`.

- `app/chamados/services/validacao.py:163` `devolver_validacao_recusada` -- V passa a nulo, preservando a resposta; pode reabrir o continente pela porta -- **decisao** `DEVOLUCAO`. Save em `app/chamados/services/validacao.py:202`; DP no mesmo atomic em `app/chamados/services/validacao.py:205`. P: `app/chamados/services/validacao.py:464`. T: `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:144`; `app/chamados/tests/test_o261_recusa_fala.py:178`; `app/chamados/tests/test_o261_recusa_fala.py:186`; `app/chamados/tests/test_o261_recusa_fala.py:199`; `app/chamados/tests/test_o261_recusa_fala.py:210`; `app/chamados/tests/test_e1_volta2.py:108`.

- `app/chamados/services/disputa_emissao.py:1422` `responder_pergunta_disputa_core` -- grava R; limpa V anterior; pode descarimbar e gravar recusa explicativa de escape -- **decisao** `RESPOSTA`; limpeza dos carimbos é consequência da resposta nova. Save em `app/chamados/services/disputa_emissao.py:1505`; DP em `app/chamados/services/disputa_emissao.py:1508`, preservando projeção legada sem linha. P: `app/api/views.py:2651`; `app/chamados/views.py:1283`; `app/chamados/services/declaracao_texto.py:419`; `app/chamados/services/declaracao_texto.py:693`; `app/chamados/services/acoes_disputa.py:371`. T: `app/chamados/tests/test_porta_resposta_pergunta.py:177`; `app/chamados/tests/test_porta_resposta_pergunta.py:209`; `app/chamados/tests/test_porta_resposta_pergunta.py:229`; `app/chamados/tests/test_e1_volta2.py:62`; `app/chamados/tests/test_e1_volta2.py:75`.

- `app/chamados/services/disputa_emissao.py:1559` `reabrir_para_correcao` -- limpa R e V; opcionalmente desfaz C da absorção -- **decisao** `REABERTURA` ou `RECUSA`. DP com foto anterior em `app/chamados/services/disputa_emissao.py:1612`; update em `app/chamados/services/disputa_emissao.py:1629`, ou porta de lote em `app/chamados/services/disputa_emissao.py:1627`; emite evento da célula no mesmo atomic. P, incluindo apelidos locais: `app/chamados/models.py:2234`; `app/chamados/views.py:1436`; `app/chamados/services/materializacao.py:682`; `app/chamados/services/validacao.py:593`; `app/chamados/services/acoes_disputa.py:103`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`. T: `app/chamados/tests/test_reabrir_nao_destroi.py:42`; `app/chamados/tests/test_reabrir_nao_destroi.py:102`; `app/chamados/tests/test_e1_v13_ordem.py:64`; `app/chamados/tests/test_e1_volta2.py:210`; `app/chamados/tests/test_e1_volta2.py:226`.

- `app/chamados/services/disputa_emissao.py:1635` `responder_automaticamente` -- R recebe texto/instante do fato entregue por outro caminho; não sobrescreve texto existente -- **decisao** de sistema `RESPOSTA`. Save em `app/chamados/services/disputa_emissao.py:1652`; DP em `app/chamados/services/disputa_emissao.py:1653`. P: `app/api/foto_recebida.py:48`. T: `app/chamados/tests/test_porta_chamado_e3.py:46`; `app/chamados/tests/test_porta_chamado_e3.py:48`; `app/chamados/tests/test_porta_chamado_e3.py:53`.

- `app/chamados/services/disputa_emissao.py:1658` `carimbar_resposta_pela_trilha` -- `respondida_em` recebe o instante original testemunhado, sem inventar instante da resposta -- **decisao** reconstruída `RESPOSTA`. Save em `app/chamados/services/disputa_emissao.py:1673`; DP em `app/chamados/services/disputa_emissao.py:1674`. P: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`. T: `app/chamados/tests/test_curar_perguntas_em_limbo.py:37`; `app/chamados/tests/test_curar_perguntas_em_limbo.py:50`; `app/chamados/tests/test_curar_perguntas_em_limbo.py:51`; `app/chamados/tests/test_porta_resposta_pergunta.py:247`; `app/chamados/tests/test_porta_resposta_pergunta.py:251`; `app/chamados/tests/test_porta_resposta_pergunta.py:262`.

- `app/chamados/services/relavra_via.py:106` `relavrar` -- via derivada atual e L, no mesmo save -- **efeito** para a via; **fica** L como cache. Save em `app/chamados/services/relavra_via.py:127`; sem DP próprio; exclui vias que não pertencem a `VIAS_DERIVADAS` (`app/chamados/services/relavra_via.py:118`). P: `app/chamados/services/relavra_via.py:157`; `app/chamados/management/commands/relavrar_via_i6.py:102`. T: `app/chamados/tests/test_porta_veredito_pergunta.py:363`; `app/chamados/tests/test_porta_veredito_pergunta.py:383`; `app/chamados/tests/test_porta_veredito_pergunta.py:386`; `app/chamados/tests/test_porta_veredito_pergunta.py:404`.

- `app/chamados/models.py:833` `ChamadoColaborador.abrir` -- cria identidade/contexto e `status_local` inicial decidido pelo motor; `get_or_create` em `app/chamados/models.py:971`, defaults em `app/chamados/models.py:938` -- **fica** o nascimento/identidade; não é escritor da tarefa. Sem DP próprio. P: `app/ponto/views.py:3012`; `app/escala/regua_defesa.py:51`; `app/api/views.py:1997`; `app/chamados/reconciliador.py:910`; `app/chamados/views.py:973`; `app/ponto/services/cluster_espurio.py:130`; `app/ponto/services/ausencia.py:659`; `app/ponto/management/commands/detectar_par_relampago.py:368`; `app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/detectar_ausencias.py:190`; `app/ponto/management/commands/detectar_ausencias.py:271`; `app/ponto/management/commands/detectar_ausencias.py:510`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`; `app/ponto/management/commands/processar_alertas_turno.py:335`; `app/chamados/services/disputa_emissao.py:568`; `app/chamados/services/disputa_emissao.py:1101`; `app/chamados/services/disputa_emissao.py:1336`; `app/chamados/services/adesao.py:74`. T: `app/ponto/tests/test_o_outbox_celula.py:287`; `app/ponto/tests/test_o_outbox_celula.py:555`.

- `app/chamados/models.py:1054` `ChamadoColaborador.criar` -- cria identidade/contexto/status inicial; auditoria aberta pode nascer `registrado` pela decisão do motor (`app/chamados/models.py:1064`); create em `app/chamados/models.py:1095` -- **fica**. Sem DP próprio. P, incluindo `_Chamado`, `_Ch9` e `_ChG`: `app/colaboradores/views.py:1978`; `app/colaboradores/views.py:2363`; `app/ponto/utils.py:40`; `app/ponto/utils.py:103`; `app/ponto/views.py:291`; `app/api/views_core.py:747`; `app/api/views.py:747`; `app/api/views.py:2201`; `app/colaboradores/services/vinculo.py:393`; `app/colaboradores/services/desligamento.py:163`; `app/ponto/services/justificativa.py:291`; `app/ponto/services/geofence.py:339`; `app/ponto/services/triagem_batida.py:107`; `app/ponto/services/triagem_batida.py:336`; `app/ponto/services/triagem_batida.py:459`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/chamados/services/regularizacao_ext.py:246`; `app/chamados/services/abertura.py:47`; `app/chamados/services/abertura.py:172`. Ensaio: `app/chamados/management/commands/smoke_chamados.py:123`. T: `app/colaboradores/tests/test_vincular_escala_selos.py:26`; `app/ponto/tests/test_cobertura_followup.py:45`.

- `app/chamados/services/disputa_emissao.py:474` `_abrir_ou_agregar_disputa` -- cria continente, motivos/contexto e estado inicial aberto, `fechada_em=NULL` pelo campo; create em `app/chamados/services/disputa_emissao.py:587` -- **fica**. Default em `app/chamados/models.py:1754`; sem DP próprio. P: `app/chamados/services/disputa_emissao.py:627`; `app/chamados/services/disputa_emissao.py:662`; `app/ponto/management/commands/reconciliar_grade.py:424`. Ensaio: `app/chamados/management/commands/smoke_chamados.py:99`. T: `app/chamados/tests/test_e1_fila_v2_2_integracao.py:132`.

- `app/chamados/services/disputa_emissao.py:1024` `criar_disputa_a_partir_de_chamados_orfaos` -- cria continente aberto, identidade, motivos/contexto; create em `app/chamados/services/disputa_emissao.py:1120` -- **fica**. `fechada_em=NULL` pelo default (`app/chamados/models.py:1754`); sem DP próprio. P: `app/ponto/management/commands/processar_alertas_avancados.py:159`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/ponto/management/commands/processar_alertas_turno.py:385`; `app/chamados/services/cobrar_dia.py:49`; `app/chamados/services/pergunta_no_app.py:208`; `app/chamados/services/acoes_disputa.py:222`. T: `app/chamados/tests/test_chamados_models.py:135`; `app/chamados/tests/test_chamados_models.py:155`; `app/chamados/tests/test_chamados_models.py:157`; `app/tenants/tests.py:74`; `app/tenants/tests.py:85`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:78`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:244`.

- `app/chamados/services/disputa_emissao.py:1299` `criar_disputa_ancora_de_chamado` -- cria continente aberto e vínculo com chamado, motivos/contexto; create em `app/chamados/services/disputa_emissao.py:1355` -- **fica**. `fechada_em=NULL` pelo default (`app/chamados/models.py:1754`); sem DP próprio. P: `app/chamados/services/acoes_disputa.py:189`. T: `app/chamados/tests/test_fio_ancora.py:74`; `app/chamados/tests/test_fio_ancora.py:92`; `app/chamados/tests/test_fio_ancora.py:93`; `app/chamados/tests/test_fio_ancora.py:104`; `app/chamados/tests/test_fio_ancora.py:151`; `app/chamados/tests/test_3b_reuso_1061.py:30`.

- `app/chamados/models.py:2822` `PerguntaDisputa._nascer` -- insere identidade, dia, instância, célula,

[truncado em 20000 de 75733 caracteres]

## 09/11 10:02 · sessao 0516585baa5b · - · - · origem=Ronald (tmux)

Inventário enviado em mapas 1–4, com extensões de autoria, silêncio, identidade e datas, sempre com referências de código.

Divergências principais:

- `materializada_em` significa processamento, inclusive recusa sem batida; não prova materialização de fato. `app/chamados/models.py:3382`; `app/chamados/services/materializacao.py:435`; `app/chamados/services/raiox.py:175`.
- `estado`, `q_vivas`, vivacidade e pendência no app respondem perguntas diferentes. `app/chamados/models.py:3030`; `app/chamados/models.py:3325`; `app/chamados/services/pergunta_no_app.py:67`.
- `data_evento` guarda dia do turno ou dia civil da conversa; leitores remanescentes sem discriminador já estão declarados no censo. `app/chamados/services/fio.py:1380`; `app/chamados/services/fio.py:1608`.
- Respostas incluem hora, data/hora, escapes, escolhas e identificadores; cada consumidor precisa preservar o discriminador. `app/chamados/services/materializacao.py:356`; `app/chamados/services/materializacao.py:995`; `app/chamados/models.py:3512`.
- Janela viva inclui ambas as extremidades; silêncio termina no instante limite; expiração usa dia local. `app/chamados/juizes.py:601`; `app/chamados/models.py:511`; `app/chamados/veredito_lavrado.py:116`.

O ajuste noturno atual já lê dia do turno e cadeia de marcos; o antigo recuo civil está removido. `app/chamados/juizes.py:296`; `app/chamados/services/materializacao.py:418`.

O inventário sintático não certifica leitores dinâmicos, templates, SQL cru ou execução em produção. [não verificado]

## 09/11 10:00 · sessao 502871ba1ad1 · - · - · origem=Ronald (tmux)

Chamadores por AST dos seis nascimentos:

- `Chamado.abrir`: `app/ponto/views.py:3012`; `app/escala/regua_defesa.py:51`; `app/api/views.py:1997`; `app/chamados/reconciliador.py:910`; `app/chamados/views.py:973`; `app/ponto/services/cluster_espurio.py:130`; `app/ponto/services/ausencia.py:659`; comandos de ponto `detectar_par_relampago.py:368`, `reconciliar_grade.py:507`, `detectar_ausencias.py:190`, `:271`, `:510`, `:693`, `emitir_turno_aberto_retroativo.py:151`, `processar_alertas_turno.py:335`; `app/chamados/services/disputa_emissao.py:568`, `:1101`, `:1336`; `app/chamados/services/adesao.py:74`. Testes exemplares: `app/ponto/tests/test_o_outbox_celula.py:287`, `:555`.
- `Chamado.criar`: `app/colaboradores/views.py:1978`, `:2363`; `app/ponto/utils.py:40`, `:103`; `app/ponto/views.py:291` (`_Chamado`); `app/api/views_core.py:747` e `app/api/views.py:747` (`_Ch9`); `app/api/views.py:2201`; `app/colaboradores/services/vinculo.py:393`; `app/colaboradores/services/desligamento.py:163`; `app/ponto/services/justificativa.py:291` (`_Chamado`); `app/ponto/services/geofence.py:339` (`_ChG`); `app/ponto/services/triagem_batida.py:107`, `:336`, `:459`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/chamados/services/regularizacao_ext.py:246`; `app/chamados/services/abertura.py:47`, `:172`. Ensaio: `app/chamados/management/commands/smoke_chamados.py:123`. Testes exemplares: `app/colaboradores/tests/test_vincular_escala_selos.py:26`, `app/ponto/tests/test_cobertura_followup.py:45`.
- `_abrir_ou_agregar_disputa`: `app/chamados/services/disputa_emissao.py:627` (`disputa_para_declaracao`), `:662` (`criar_ou_atualizar_disputa`); `app/ponto/management/commands/reconciliar_grade.py:424`. Ensaio: `app/chamados/management/commands/smoke_chamados.py:99`; teste com alias `de`: `app/chamados/tests/test_e1_fila_v2_2_integracao.py:132`.
- `criar_disputa_a_partir_de_chamados_orfaos`: comandos de ponto `processar_alertas_avancados.py:159`, `detectar_ausencias.py:802`, `processar_alertas_turno.py:385`; `app/chamados/services/cobrar_dia.py:49`, `pergunta_no_app.py:208`, `acoes_disputa.py:222`. Testes: `app/chamados/tests/test_chamados_models.py:135`, `:155`, `:157`; teste fora diretório `tests/`: `app/tenants/tests.py:74`, `:85`; alias `de`: `app/chamados/tests/test_e1_fila_v2_2_integracao.py:78`, `:244`.
- `criar_disputa_ancora_de_chamado`: produção `app/chamados/services/acoes_disputa.py:189`; testes `app/chamados/tests/test_fio_ancora.py:74`, `:92`, `:93`, `:104`, `:151`; `app/chamados/tests/test_3b_reuso_1061.py:30`.
- `Pergunta._nascer`: único chamador direto `app/chamados/models.py:2817`, `_sob_contrato`; este recebe `abrir:2786` e `abrir_conversa:2798`. Pergunta datada nasce por `abrir`; conversa usa dia civil `localdate`, explicitamente (`app/chamados/models.py:2791`).

Correlatos pedidos:

- `app/chamados/models.py:2179` `reabrir_por_colab`: `save:2237` grava somente `reaberturas_colab=usadas+1`; delega reabertura da resposta em `2234`, mesmo atomic de `2232`, gerando DP pela porta delegada. Não reabre disputa. Produção: `app/api/views.py:2710`, `app/chamados/views.py:1334`; testes: `app/chamados/tests/test_porta_disputa_ciclo.py:285`, `:302`, `:303`, `:315`, `:335`, `:343`, `:355`; `test_e1_v13_cadeia.py:125`.
- `app/chamados/models.py:2247` `limpar_resolucao_residual`: `save:2263` grava apenas `resolucao=''`; exige disputa aberta com texto (`2259`); trilha em `2266`. Nenhum chamador AST localizado, nem produção nem teste; chamada dinâmica: [não verificado].
- `app/chamados/models.py:1968` `remover_motivo`: `save:2062` grava `motivos/contexto`; DP `RETRATACAO:2053` e `carimbar_identidade:2058`, mesmo atomic. Não apaga perguntas na cópia. Produção: `app/colaboradores/signals.py:77`, `app/escala/signals.py:161`; testes exemplares `test_e1_entradas.py:138`, `:149`, `:163`; `test_e1_volta2.py:257`, `:281`; `test_porta_remocao_motivo.py:255`.
- `app/chamados/models.py:1949` `aparar_motivo_respondido`: `save:1965` grava somente `motivos/contexto`, sem fechar disputa; produção `app/api/foto_recebida.py:72`. O comentário que diz que `remover_motivo` apaga está desatualizado (`1952`), contradito pelo código de retração (`2053`).
- `Chamado.apagar_em_ensaio`: chamador em arquivo de comando, **não teste Django**: `app/ponto/management/commands/zerar_dados_operacionais.py:84`; execução é ensaio condicionado pela própria porta `app/chamados/models.py:563`.

Estado inicial de Chamado: `abrir` monta `status_local=_d.novo_estado` em `app/chamados/models.py:938`; `criar` converte aberto para estado do motor em `1064`. Disputa inicia `fechada_em=NULL` pelo campo `app/chamados/models.py:1754`.

## 09/11 09:58 · sessao 502871ba1ad1 · - · - · origem=Ronald (tmux)

AST confirma o núcleo histórico de **16 portas = 1 Chamado + 3 Disputa + 12 Pergunta**, desde que se conte alteração dos campos principais e se explicitem separadamente nascimentos, executores genéricos e complementos. O próprio RELATO distingue “16”, “24” e “mais 3 de identidade/data”. `app/docs/RELATO.md:242`

| Porta (definição) | Persistência / campos | DP junto |
|---|---|---|
| `app/chamados/models.py:201` `_gravar_estado` | `save:296`: `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`; autor e urgência condicionais (`228–253`) | Não direto |
| `app/chamados/models.py:1852` `Disputa.fechar` | `save:1933`: `fechada_em=now`, `fechada_por`, `resolucao`, `motivo_fechamento` | Só exceção que carimba perguntas: `SILENCIO:1921` |
| `app/chamados/models.py:2123` `reabrir_admin` | `save:2158`: zera os quatro campos do fechamento; incrementa `reaberturas_admin`; pode agregar `motivos` | Não direto |
| `app/chamados/models.py:2160` `reabrir_sistema` | `save:2174`: zera os quatro campos do fechamento | Não direto |
| `app/chamados/models.py:3092` `carimbar` | `save:3141`: `materializada_em/por`, `via_resolucao`; pode completar resposta/validação e lavrar cache | Não direto |
| `app/chamados/models.py:3157` `carimbar_identidade` | `save:3179`: trio do carimbo + `recusa_motivo` opcional | Não direto |
| `app/chamados/models.py:3192` `descarimbar` | `save:3201`: trio `None/None/''`; `salvar=False` só prepara campos | Não direto |
| `app/chamados/models.py:3241` `descarimbar_em_lote` | `update:3253`: trio `None/None/''` + `outros`; exclui vias fora do fato | Não próprio; chamador pode registrar |
| `app/chamados/models.py:3257` `encerrar_por_fato_no_espelho` | `save:3265`: somente `via_resolucao=fato_materializado` | Não direto |
| `app/chamados/services/validacao.py:127` `_carimbar_validacao` | `save:137`: `validada_em=now`, `validada_por=autor` | `VALIDACAO:139`, mesmo atomic |
| `app/chamados/services/validacao.py:163` `devolver_validacao_recusada` | `save:202`: `validada_em/por=None` | `DEVOLUCAO:205`, mesmo atomic |
| `app/chamados/services/disputa_emissao.py:1422` `responder_pergunta_disputa_core` | `save:1505`: resposta/texto+instante; zera validação se mudou; descarimbo em memória e recusa especial condicionais | `RESPOSTA:1508` |
| `app/chamados/services/disputa_emissao.py:1559` `reabrir_para_correcao` | `update:1629`: resposta vazia e resposta/validação nulas; com absorção delega descarimbo `1627` | `registrar:1612`, ato `reabertura` ou `recusa` |
| `app/chamados/services/disputa_emissao.py:1635` `responder_automaticamente` | `save:1652`: texto+instante somente sem texto anterior | `RESPOSTA:1653` |
| `app/chamados/services/disputa_emissao.py:1658` `carimbar_resposta_pela_trilha` | `save:1673`: `respondida_em=instante` testemunhado pela trilha | `RESPOSTA:1674` |
| `app/chamados/services/relavra_via.py:106` `relavrar` | `save:127`: via calculada hoje + trio cache | Não direto |

**Oito complementos**, fechando o recorte de 24 do RELATO:

- `app/chamados/services/materializacao.py:784` `_materializar_resposta_hora`: `save:851` grava **somente** `pendencia_admin/recusa_motivo`; nesse ponto não grava carimbo.
- `app/chamados/services/materializacao.py:1083` `_carimbar_recusa`: `save:1103` grava **somente** `recusa_motivo`; colisão delega carimbo de identidade em `1098`.
- `app/chamados/services/validacao.py:533` `recusar_pergunta`: `save:590` grava `recusa_motivo`; alteração da resposta delegada à reabertura, com ato recusa.
- `app/chamados/services/validacao.py:625` `conferir_data_do_fato`: `save:666` grava `data_conferida_em/por`; humano aceita `data_evento` quando diverge do chamado, com `LogAuditoria:667`; sem DP direto.
- `app/chamados/veredito_lavrado.py:240` `gravar_lavras_em_lote`: `bulk_update:250`, trio cache.
- `app/chamados/veredito_lavrado.py:254` `lavrar`: `save:275`, `veredito_lavrado/julgado_em/janela_expira_em`.
- `app/chamados/services/arquivo.py:141` `silenciar`: `save:171`, prazo/autor/motivo do silêncio+atualização.
- `app/chamados/services/arquivo.py:177` `levantar`: `save:210`, limpa silêncio; `resgatado_em` opcional. O módulo declara silêncio como véu, sem alterar estado (`154`).

**Três identidade/data**, fechando o recorte de 27:

- `app/chamados/models.py:2822` `_nascer`: `save:2998` atualiza célula/destino+cache no reuso; `save(force_insert=True):3008` insere pergunta.
- `app/chamados/services/fio.py:804` `reapontar_resolvedora`: `save:924`, destino/costura e cache.
- `app/chamados/services/fio.py:970` `rechavear_data_evento`: `save:1034`, dia/célula e cache.

**O pedido literal inclui `create/delete`; não esconder estes por trás do número 16:**

- `app/chamados/models.py:833` `Chamado.abrir`: `get_or_create:971`, estado inicial pelo `defaults`.
- `app/chamados/models.py:1054` `Chamado.criar`: `create:1095`, `status_local=status`; motor converte aberto de auditoria para registrado (`1059–1064`).
- `app/chamados/services/disputa_emissao.py:474` `_abrir_ou_agregar_disputa`: `create:587`.
- `app/chamados/services/disputa_emissao.py:1024` `criar_disputa_a_partir_de_chamados_orfaos`: `create:1120`.
- `app/chamados/services/disputa_emissao.py:1299` `criar_disputa_ancora_de_chamado`: `create:1355`.
- Esses três criadores de disputa iniciam `fechada_em=NULL` pelo default do campo (`app/chamados/models.py:1754`).
- `app/chamados/models.py:3402` `Pergunta.save`: `super().save:3412` é executor genérico e garante atomicidade com outbox, além de derivar identidade na inserção (`3410`); não representa novo ato semântico.
- `app/chamados/models.py:563` `Chamado.apagar_em_ensaio`: `delete:571`, explicitamente porta de ensaio.

**Exclusões por ausência de persistência:**

- `_vivas_entre`: troca `status_local` em memória e restaura (`app/chamados/models.py:356`, `:371`).
- `_universo_da_ressuscitacao`: simula validação/resposta nulas (`app/chamados/models.py:1507`).
- `_escrever_carimbo`: só atribui em memória (`app/chamados/models.py:3086`).
- `_desfazer_em_memoria`: só rollback da instância (`app/chamados/models.py:3148`).

Varredura AST global dos campos principais não localizou persistência principal de Pergunta/Disputa/Chamado fora de `chamados/`; `regularizacao_ext.reg.materializada_em` pertence a outro modelo (`app/chamados/services/regularizacao_ext.py:198`). Completude frente a despacho inteiramente dinâmico que não exponha modelo nem campo no AST: [não verificado].

## 09/11 09:56 · sessao 8573dd838240 · - · - · origem=Ronald (tmux)

Autoridade lida e critério:

- **DECISÃO** = fato próprio do ato humano/sistema; sequência única imutável, com instante, autor, texto e motivo; a projeção atual será encolhida pela E4. Atos existentes: resposta, validação, devolução, recusa, reabertura, silêncio, retratação e absorção. `app/chamados/models.py:3828`, `app/chamados/models.py:3850`.
- **EFEITO** = consequência cuja ocorrência precisa ser conferida externamente; resposta validada não equivale a materialização. E2 distingue sem lavra vigente, validada sem efeito, efeito ainda não visto pela célula e encerrada pelo juiz. `app/chamados/services/tarefa_do_dia.py:118`, `app/chamados/services/tarefa_do_dia.py:140`.
- **FICA** = identidade/data ou cache reconstruível: identidade da pergunta em `app/chamados/models.py:2424`; dia do turno atribuído exclusivamente por `gravar_dia_do_fato`, sem persistência própria, em `app/chamados/services/fio.py:790`; cache explícito da lavra em `app/chamados/veredito_lavrado.py:58`, `app/chamados/veredito_lavrado.py:74`.
- `DecisaoPergunta.registrar` é a porta única da sequência; update/delete/bulk_update e edição de linha existente são recusados. `app/chamados/models.py:3814`, `app/chamados/models.py:3892`, `app/chamados/models.py:3931`.

Ramos que impedem classificação ingênua por função:

- `DisputaSupervisao.fechar`: default é **efeito** da inexistência de pergunta viva/resposta sem veredito; exceções `desligamento`/`admin_encerra` são **decisão** e já registram `SILENCIO` E1. `app/chamados/models.py:1855`, `app/chamados/models.py:1892`, `app/chamados/models.py:1915`.
- `DisputaSupervisao.remover_motivo`: **decisão** `RETRATACAO` já registrada; carimbo terminal da pergunta e atualização da lista de motivos são consequências do ato. `app/chamados/models.py:2043`, `app/chamados/models.py:2053`.
- `_materializar_ausencia_de_resposta`: vínculo de absorção já é fato `ABSORCAO` E1, com escape e ausência; carimbo da pergunta vem no mesmo atomic. `app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:557`.
- `responder_automaticamente`: **decisão** `RESPOSTA` de sistema sobre fato entregue por outro caminho, não cache descartável. `app/chamados/services/disputa_emissao.py:1635`, `app/chamados/services/disputa_emissao.py:1653`.
- `carimbar_resposta_pela_trilha`: **decisão** reconstruída com o instante original testemunhado pela nota, não instante atual da reparação. `app/chamados/services/disputa_emissao.py:1658`, `app/chamados/services/disputa_emissao.py:1674`.

Divergências semânticas para DECISÕES numeradas:

1. **Carimbo de processamento ≠ prova de batida/fechamento.** Help text de `materializada_em` diz “batida criada + chamado fechado”; `carimbar_identidade` também escreve esse campo para identidade sem plantio e pode manter instante/autor anteriores. A E2 exige testemunha/lavra vigente. `app/chamados/models.py:3382`, `app/chamados/models.py:3157`, `app/chamados/models.py:3171`, `app/chamados/services/tarefa_do_dia.py:118`.
2. **Estado visual por timestamps ≠ exigibilidade do ato.** `estado` chama validada+materializada de resolvida; E2 distingue validação sem efeito ou sem lavra vigente. `app/chamados/models.py:3030`, `app/chamados/services/tarefa_do_dia.py:140`.
3. **`data_evento` = dia do turno, não dia civil da batida.** Help text antigo fala data do evento; identidade e E2 explicitam dia do turno. `app/chamados/models.py:2420`, `app/chamados/models.py:2424`, `app/chamados/services/tarefa_do_dia.py:213`, `app/chamados/services/fio.py:790`.
4. **Expiração = dia civil local; julgamento = instante.** `relogio_virou` usa `janela_expira_em <= hoje_local`, mas a lavra só vence pelo relógio quando `localdate(julgado_em) < janela_expira_em`. Não se compara a data UTC. `app/chamados/veredito_lavrado.py:95`, `app/chamados/veredito_lavrado.py:107`.
5. **Janela inclusiva e expiração exclusiva.** Janela viva inclui início da anterior menos um dia e fim da corrente; primeiro dia fora é a expiração. Não corresponde a duração fixa de 30/60 dias. `app/chamados/veredito_lavrado.py:119`, `app/chamados/veredito_lavrado.py:145`.
6. **Resposta/validação legadas sem linha ≠ decisão E1 demonstrada.** E1 preserva a projeção anterior como linhas antes de sobrescrevê-la; E2 devolve `passivo_sem_linha`, sem inventar ação. `app/chamados/models.py:3903`, `app/chamados/services/tarefa_do_dia.py:127`.
7. **`via_resolucao` não significa sempre declaração de hora.** Escape ATESTADO/FALTA_DIA e irmã muda usam ausência; o escritor pergunta `declarou_hora`, e a sequência liga a absorção à ausência concreta. `app/chamados/services/materializacao.py:534`, `app/chamados/services/materializacao.py:549`.
8. **Fechamento de disputa ≠ julgamento de resposta.** Default só fecha após inexistência de viva e resposta sem veredito; fechamento excepcional decide silêncio e registra E1. `app/chamados/models.py:1823`, `app/chamados/models.py:1852`, `app/chamados/models.py:1915`.

Leis/cortes conferidos: L-002 (`app/docs/LEIS.md:60`), L-007 (`:65`), L-051 (`:92`), L-061 (`:95`), L-073 (`:100`), L-080 (`:107`), L-085 (`:117`), L-088 (`:120`), L-110 (`:136`), L-111 (`:137`), L-117 (`:138`), L-124 (`:145`), L-113 (`:151`); FIO-MORTO-E-ESTRUTURAL (`app/docs/CORTES.md:253`), FIO-E0-V4 (`:426`), LEI-TRANCA-E2-JUIZ (`:427`). Essas autoridades respondem os pontos examinados; **perguntas de lei: 0** neste recorte. Não concluí censo integral de escritores/leitores, responsabilidade dos outros levantamentos. [não verificado]

## 09/11 09:55 · sessao 3fe3faa2e17b · FIO-E0-V4-E3 · construtor · origem=Code

Entrega: [F1–F8, causas, hunks, escritores/leitores e comandos de RED/mutantes](/home/ronald/wt-codex-fio-e0-v4-e3-084151/app/docs/ENTREGA_FIO_E0_V4_E3_V3.md:5).

**F2: código. F1/F3/F4/F5/F6/F7/F8: selo.**

`py_compile`: **10 arquivos, rc 0**, sem bytecode; `git diff --check`: rc 0. [Verificação e comandos](/home/ronald/wt-codex-fio-e0-v4-e3-084151/app/docs/ENTREGA_FIO_E0_V4_E3_V3.md:70). Suíte não executada; mordida dos RED/mutantes **[nao verificado]**. Ruff indisponível. [nao verificado]

**DECISAO-ABERTA:** DIFF/L-099 antes da fachada; crons e diagramas com a sessão. [Referência](/home/ronald/wt-codex-fio-e0-v4-e3-084151/app/docs/ENTREGA_FIO_E0_V4_E3_V3.md:90).

## 09/11 09:17 · sessao bbec4829eb42 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

5. Entrega no [relatório por hunk, escritores/leitores e comandos individuais](docs/BUG-PAR-INTERVALO-V2.txt:9).

- **DECISÃO-ABERTA — P2:** ligação de C7 ao predicado reservada à sessão; import e linha exata em [entrega:205](docs/BUG-PAR-INTERVALO-V2.txt:205). Teste: [test_P2_C7_nao_fecha_se_irma_sai_do_fato](chamados/tests/test_bug_par_intervalo.py:388).
- **Verificação:** `py_compile` sem persistência retornou rc 0; âncoras e sintaxe de M1–M37 conferidas. [entrega:301](docs/BUG-PAR-INTERVALO-V2.txt:301)
- **Sem execução da suíte ou dos mutantes:** RED/GREEN e mortes por asserção **[nao verificado]**. F1–F4 e produção permanecem fora da cobertura. [entrega:218](docs/BUG-PAR-INTERVALO-V2.txt:218)

## 09/11 09:10 · sessao 34f994372353 · SONDA-SEM-GRANT · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

_3106 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
