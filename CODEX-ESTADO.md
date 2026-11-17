# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 01:12** · publicado em **17/11/2026 01:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 01:12 · sessao ec551772e0be · REVISAO-AVAL-RD1711 · verificador · origem=Code

1. **Premissas: conferem, exceto a garantia de criação discutida no item 3.** K6 hoje exclui U163; a validação exige resposta, ausência de validação anterior, passagem pela espera declarado × gravado e pelo implausível. (`app/chamados/services/materializacao.py:795`; `app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:597`; `app/chamados/services/validacao.py:609`; `app/chamados/services/validacao.py:615`)

   Entregue com `FechamentoMensal` aberto é compatível: K6 consulta exportação vigente; F6 barra fechamento aprovado/exportado. B1 também confere: o juiz preserva L-123-MEDIDA-EM-PY, e o hook já chama esse juiz. A causa concreta de `4633d2c17` permanece **[nao verificado]**. (`app/folha/export.py:564`; `app/ponto/janelas.py:86`; `bin/tests/test_lei_akita.sh:348`; `bin/commit_msg.sh:29`)

2. **Colisões: nenhuma nova identificada nos registros conferidos.** F4 sobre B2 já é o encaixe declarado; a inclusão de U163 em L1/L5 sucede expressamente o tratamento anterior, preservando F6. Continua obrigatória a condição J1: F4 logo atrás de F1/F2 ou contador esperado zero no intervalo. (`app/docs/RELATO.md:26`; `app/docs/RELATO.md:28`; `app/docs/BACKLOG.md:823`; `app/docs/CORTES.md:488`)

   BACKLOG e resumo do brief CLASSE-D registram R1 antes de F2; o registro posterior dispensa F2 de esperar CLASSE-D. LU2-07/LU2-08 conservam seus ramos abertos, sem decisão nova por este aval. Conferência do brief integral: **[nao verificado]**; seu resumo consta no RELATO. (`app/docs/BACKLOG.md:1990`; `app/docs/RELATO.md:421`; `app/docs/RELATO.md:4`; `app/docs/RELATO.md:352`; `app/docs/RELATO.md:354`)

   B preserva a permissão cuja supressão motivou a devolução e mantém a exceção nominal fechada, com catraca 13. (`app/docs/RELATO.md:7`; `bin/tests/test_lei_akita.sh:348`; `bin/tests/test_lei_akita.sh:411`; `bin/tests/test_lei_akita.sh:413`)

3. **Literal: ainda não fecha a contagem `+1` de A4.** A busca de batida existente não filtra aprovação; o escritor reutiliza qualquer encontrada e só cria quando não encontra nenhuma. Portanto, “sem batida aprovada” ainda permite reuso sem criação. (`app/chamados/juizes.py:247`; `app/chamados/services/materializacao.py:1347`; `app/chamados/services/materializacao.py:1353`)

   Contraexemplo de código: sem irmã, batida não aprovada do mesmo tipo exatamente em `ts`. Ela não produz `ja_satisfeita` e não entra na vizinhança anterior/posterior, cujos limites são estritos; ainda assim, o escritor a reutiliza. Nenhuma guarda precisa ser afrouxada. Ocorrência em produção: **[nao verificado]**. (`app/chamados/services/materializacao.py:899`; `app/chamados/services/materializacao.py:113`; `app/chamados/services/materializacao.py:116`; `app/chamados/services/materializacao.py:1347`)

   Para fechar esse ponto, A4 precisa exigir ausência de qualquer batida retornada pela busca de reuso. (`app/chamados/juizes.py:239`; `app/chamados/services/materializacao.py:1353`)

4. **Exportada, dinheiro e `!`: não identifiquei ampliação além do bloqueio declarado.** A guarda está antes do carimbo e do recálculo na validação e antes das escritas do materializador. Alteração do `FechamentoMensal` exportado continua sob `!`; impacto monetário efetivo: **[nao verificado]**. (`app/chamados/services/validacao.py:652`; `app/chamados/services/validacao.py:655`; `app/chamados/services/validacao.py:666`; `app/chamados/services/materializacao.py:1829`; `CLAUDE.md:411`)

**VEREDITO: PROCEDE (item 3/A4: “sem batida aprovada” permite reuso sem criação e não garante `+1`; app/chamados/juizes.py:247; app/chamados/services/materializacao.py:1353).**

## 17/11 01:12 · sessao 00e5b02f1deb · - · - · origem=Ronald (tmux)

Brief contado em memória: **123 linhas**; título = linha 1; vazias incluídas. Correspondência com arquivo original externo à cópia: [nao verificado].

| Elemento | Linhas |
|---|---|
| Todas as marcas `[DEPENDE-F1]` | **18, 31, 56, 61, 68** |
| Seções 1–7 | **6, 35, 66, 86, 97, 106, 117** |
| Premissa F1/E3 da base | 3 |
| Forma E3 e comparação dos oito arquivos | 9–14 |
| U1–U9 | 20–28 |
| Exceções/sobra real | 31 |
| Caller da tarefa | 33 |
| Censo e chamadores | 37–59 |
| Faces F1 privada/SQL/custo | 61–64 |
| Frota/matriz histórica | 68–84 |
| RED 1–8 | 88–95 |
| Cruzamentos E3/F1/F2/BUG-PAR | 100–104 |
| Impacto dinheiro e medidas | 108 |
| Lei 1 aberta | 110 |
| Assuntos considerados decididos | 113–115 |
| Condições de pouso | 119–123 | [nao verificado]

Marcas: :18 governa o contrato U1–U9; :31 governa as exceções C/via/recusa; :56 trata recusa técnica; :61 governa as três faces descritas em :62–64; :68 governa a comparação histórica por leitura conservada. (anexo `brief.md:18`; `brief.md:31`; `brief.md:56`; `brief.md:61`; `brief.md:68`)

## 17/11 01:12 · sessao 22e9da8fe3fe · - · - · origem=Ronald (tmux)

A1–A3 corrigem as lacunas anteriores: K6 pula U163; validação exige resposta e ausência de validação prévia, consulta a espera e o implausível. (`app/chamados/services/materializacao.py:795`; `app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:597`; `app/chamados/services/validacao.py:609`; `app/chamados/services/validacao.py:615`)

Entregue com FechamentoMensal aberto é compatível estruturalmente: K6 pergunta por ExportacaoDominio vigente; F6 bloqueia FechamentoMensal aprovado/exportado. (`app/folha/export.py:556`; `app/folha/export.py:564`; `app/ponto/janelas.py:86`)

**Lacuna literal remanescente:** A4 diz “sem batida aprovada”, mas o helper de reuso não filtra aprovação. A testemunha só retorna `ja_satisfeita` se a encontrada estiver aprovada; o escritor, porém, reusa qualquer encontrada e só cria quando o retorno é `None`. Portanto, o literal admite ausência de batida aprovada com reuso de não aprovada: não garante contagem `+1`. (`app/chamados/juizes.py:247`; `app/chamados/services/materializacao.py:898`; `app/chamados/services/materializacao.py:1341`; `app/chamados/services/materializacao.py:1353`)

Para fechar o RED, exigir ausência de **qualquer** batida retornada pelo helper, preservando as guardas; deduplicação e mesma chave também retornam batida existente sem criação. (`app/chamados/juizes.py:247`; `app/ponto/registro_batida.py:137`; `app/ponto/registro_batida.py:144`)

O `+1` também pede fixture isolada: o caminho do par grava duas metades, e depois a validação varre a disputa inteira. A alternativa autorizada “sem irmã” permite escolher um caso unitário; execução desse RED: [nao verificado]. (`app/chamados/services/validacao.py:545`; `app/chamados/services/validacao.py:555`; `app/chamados/services/validacao.py:698`; `app/chamados/tests/test_bug_par_projetado.py:109`)

**Recomendação: PROCEDE (item 3: “sem batida aprovada” não exclui reuso sem criação; `app/chamados/juizes.py:247`; `app/chamados/services/materializacao.py:1353`).**

## 17/11 01:11 · sessao 4c909f496e91 · - · - · origem=Ronald (tmux)

Complemento: consumidores diretos das funções alteráveis, separados dos registros de contratos e testes. [nao verificado]

| Função | Chamadores de produção | Arquivos de teste/referências | [nao verificado]
|---|---|---|
| `nota_da_resposta` | `app/chamados/management/commands/curar_perguntas_em_limbo.py:56` | Mock: `app/chamados/tests/test_curar_perguntas_em_limbo.py:65`. |
| `linhas_a_criar` | `app/chamados/management/commands/backfill_decisoes_pergunta.py:455` | Chamada: `app/chamados/tests/test_e1_v6_horizonte.py:125`; import `:122`. Registros estruturais: `app/chamados/tests/test_e3_catraca_respondedores.py:68`, `:81`; `app/chamados/tests/test_contract_leitores_vitalidade.py:43`. |
| `admin_decidiu_sem_ato` | `app/chamados/management/commands/backfill_decisoes_pergunta.py:487`, `:488` | `app/chamados/tests/test_e1_v13d_exportada_fora.py:64`, `:73`, `:88`, `:156`, `:157`; `app/chamados/tests/test_e1_v6_horizonte.py:130`, `:302`, `:303`, `:312`, `:335`; allowlist `app/chamados/tests/test_contract_leitores_vitalidade.py:282`. |
| `universo_i6` | `app/chamados/management/commands/auditar_invariantes_chamados.py:496`, `:737`; `app/chamados/management/commands/relavrar_via_i6.py:49` | `app/chamados/tests/test_selo_lapide_legado.py:89`; `app/chamados/tests/test_selo_i6_ouve_a_lei.py:166`, `:178`, `:188`, `:199`; registro AST `app/core/tests/test_medida_turno_do_par.py:367`. |
| `_i6_pares` | `app/chamados/management/commands/auditar_invariantes_chamados.py:461` | Substituído/restaurado: `app/chamados/tests/test_selo_lapide_legado.py:85`, `:87`, `:91`; allowlist `app/chamados/tests/test_contract_leitores_vitalidade.py:89`. |
| `_contraprova_i21a` | `app/chamados/management/commands/auditar_invariantes_chamados.py:1587` | Nenhum chamador direto de teste localizado: [nao verificado quanto a chamadas dinâmicas]. |
| `_contraprova_f1t` | `app/chamados/management/commands/auditar_invariantes_chamados.py:1723` | Allowlist `app/chamados/tests/test_contract_leitores_vitalidade.py:277`; nenhum chamador direto de teste localizado: [nao verificado quanto a chamadas dinâmicas]. |
| `_relavrar_orfa` | `app/ponto/management/commands/reconciliar_grade.py:228` | `app/escala/tests/test_cobranca_orfa_regeneracao.py:301`, `:314`, `:320`, `:328`, `:345`; `app/chamados/tests/test_porta_chamado_e2.py:65`, `:72`. |
| `classificar` do comando migrar | `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:212` | Testado pelo comando: `app/chamados/tests/test_e1_v15f.py::MatrizClassificarTest` `:351`, com casos `:357`, `:362`, `:366`, `:372`, `:376`, `:381`. Registros AST: `app/chamados/tests/test_contract_leitores_vitalidade.py:45`; `app/core/tests/test_medida_turno_do_par.py:169`, `:265`. Nenhum import/chamada direta externa de `classificar` foi localizado. [nao verificado quanto a chamadas dinâmicas] |

O walker vigente é `ofensores()` (`app/chamados/tests/test_contract_leitores_vitalidade.py:310`). Seu consumidor é `LeitoresVitalidadeContract.test_ninguem_julga_vitalidade_por_carimbo_cru` (`:340`, `:341`). O walker usa `_raiz` (`:312`), `_e_campo` (`:328`, `:332`), `_e_none` (`:328`) e `_funcao_de` (`:335`). Suas limitações são explícitas: só C/V (`:14`); exclui `models.py`/`juizes.py` (`:320`); encontra comparações com `None`, keywords `__isnull`, `if/ifexp` simples (`:328`, `:330`, `:332`). Não é o `leituras_rvc` da F1 histórica. (`app/chamados/tests/test_contract_leitores_vitalidade.py:310`, `:328`.)

O contrato é listado pelos harnesses `app/chamados/tests/e3_pos_suite_sem_banco.py:21` e `app/chamados/tests/e4b_c4_verificar.py:247`. Resultado executado: [nao verificado].

## 17/11 01:11 · sessao 035b1cb5991a · - · - · origem=Ronald (tmux)

- **A não colide com o encaixe vigente:** F4 sobre B2 já consta em `app/docs/RELATO.md:26` e `app/docs/BACKLOG.md:823`. A alteração da exceção anterior “U163 entregue fica sob F6” (`app/docs/RELATO.md:49`) já foi decidida expressamente como inclusão em L1/L5, mantendo F6 intacta (`app/docs/RELATO.md:28`).

- **A condição J1 continua vigente:** F4 deve pousar logo após F1/F2 **ou** haver contador esperado zero para a costura enquanto F4 não pousa (`app/docs/CORTES.md:488`). “Depois da B2” não elimina essa obrigação (`app/docs/RELATO.md:26`).

- **CLASSE-D/LU2 não bloqueiam este registro:** o resumo do brief CLASSE-D determina R1 isolada entre F1/F2 e dispensa esperar LU2-07/08 (`app/docs/RELATO.md:421`); o complemento PREPARA posteriormente registra abertura de F2 após B2 sem esperar CLASSE-D (`app/docs/RELATO.md:4`). LU2-08 conserva o ramo FATO_EM_AUSENCIA aberto (`app/docs/RELATO.md:352`), e LU2-07 conserva a pergunta emp1 e a distinção dos 155 casos (`app/docs/RELATO.md:354`). A não decide esses ramos. A conferência do **arquivo integral** `logs/CLASSE-D/brief/brief.md` é [nao verificado]: ele não foi localizado na cópia; o resumo está em `app/docs/RELATO.md:421`.

- **B corrige a devolução registrada:** a objeção era apagar a exceção L-123 e curar sem medir a causa (`app/docs/RELATO.md:7`). O juiz já preserva n/a medida em módulo autorizado (`bin/tests/test_lei_akita.sh:348`) e já recusa o restante (`bin/tests/test_lei_akita.sh:350`); o hook chama esse juiz (`bin/commit_msg.sh:29`). Amend e cherry-pick/rebase são limites declarados (`bin/commit_msg.sh:10`). “Cópia sem hook” como causa concreta permanece [nao verificado].

- **B não amplia exceções nem regrava história:** a exceção nominal vigente já é fechada em 4633d2c17, catraca 13, e recusa regravar (`app/docs/CORTES.md:497`). Medição com cura adiada não muda isso; causa efetiva e recuperabilidade: [nao verificado].

- **Exportada/dinheiro:** o histórico já reconhece o recálculo herdado fora do delta (`app/docs/RELATO.md:57`) e não o transforma em condição de pouso (`app/docs/RELATO.md:49`). Alterar `FechamentoMensal` exportado continua exigindo `!` (`CLAUDE.md:411`); paga também conserva `!` fora da competência corrente (`CLAUDE.md:404`). Não há autorização adicional desses atos no escopo descrito; impacto monetário efetivo: [nao verificado].

**Conclusão da frente de cortes: CONFERE, preservada a condição J1 de `app/docs/CORTES.md:488`; nenhuma colisão nova encontrada nos registros examinados.**

## 17/11 01:11 · sessao 4056af62b199 · - · - · origem=Ronald (tmux)

Item B **CONFERE como medição, sem cura agora**:

- **B1:** `negocio()` classifica arquivos de `app/` fora de testes/docs; o juiz recusa `n/a`, preservando exclusivamente o aceite de `n/a (medida)` quando `medida(a)` permite. O registro é fechado e a comparação exige AST mascarada idêntica. (`bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:172`, `bin/tests/test_lei_akita.sh:317`, `bin/tests/test_lei_akita.sh:346`)
- **Premissa do hook:** ele já chama o mesmo juiz; `julgar_commit` calcula os arquivos do commit e encaminha à mesma função `julgar`. A proposta anterior de criar recusa por arquivo estava devolvida por apagar a permissão de medida; B1 resolve essa colisão ao preservá-la explicitamente. (`bin/commit_msg.sh:29`, `bin/tests/test_lei_akita.sh:441`, `bin/tests/test_lei_akita.sh:463`, `app/docs/RELATO.md:7`)
- **B2:** `--amend` julga somente o delta, e cherry-pick/rebase sem `--edit` constam dos limites declarados. Há também limite conhecido de `revert` sem chamar o hook, registrado no BACKLOG; isso recomenda não tratar a enumeração como prova da causa. A causa concreta de `4633d2c17` permanece **[nao verificado]**. (`bin/commit_msg.sh:10`, `app/docs/BACKLOG.md:780`)
- **B3 / cortes:** medir e devolver pré-desenho não altera a exceção nominal, a catraca 13 nem história; a exceção de `4633d2c17` já está presente e fixada pela catraca. Não há fundamento no código para exigir nova regra de recusa antes dessa medição. (`app/docs/CORTES.md:497`, `bin/tests/test_lei_akita.sh:411`, `bin/tests/test_lei_akita.sh:413`, `bin/tests/test_lei_akita.sh:423`)
- **Exportada/dinheiro/`!`:** o item B permanece instrumento de julgamento de commits; o `!` anteriormente documentado era exceção de selo ou regravação, já respondido pela exceção nominal. Usar `--no-verify` seria outro `!`, mas não está pedido no novo aval. (`bin/tests/test_lei_akita.sh:441`, `app/docs/RELATO.md:9`, `app/docs/BACKLOG.md:780`)

Ressalva sem procedência: se B2 pretendesse uma taxonomia exaustiva das causas possíveis, omitiria o `revert` conhecido; o texto fornecido permite encerrar como “não recuperável”, sem inventar causa. (`app/docs/BACKLOG.md:780`; previsão de encerramento no anexo fornecido **[nao verificado]** quanto a execução.)

## 17/11 01:11 · sessao 6f0a502a94ee · - · - · origem=Ronald (tmux)

Achado central: a F1 pressuposta pelo brief/matriz **CAIU**. A BASE tem juiz booleano sobre o par projetado, `pergunta_respondida`, e declara expressamente que B/C/D migram na B2; não disponibiliza o antigo contrato D01 descrito. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1550`; `app/chamados/juizes.py:1552`.)

| Premissa F5 | Veredito e prova BASE |
|---|---|
| `atos_pergunta.leitura`/`LeituraDosAtos` e conservação pela F1 antiga | **CAIU como interface disponível.** O C6 recebe campos atuais em `projecao_temporal_da_pergunta`; `respondida_sem_texto` continua lendo o par projetado. (`app/chamados/juizes.py:1382`; `app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1534`.) Ausência absoluta de interface dinâmica: [nao verificado]. |
| Mistura “metade projeção/metade ato” em `espera_o_texto` | **CAIU.** Os dois lados continuam projetivos. (`app/chamados/services/fio.py:187`; `app/chamados/juizes.py:1534`.) |
| SQL V herdado da F2 antiga | **CAIU como suporte pousado.** Estado e pré-filtro continuam lendo V projetada. (`app/chamados/models.py:3151`; `app/chamados/models.py:3496`.) |
| Ordenação DP `(ato_em,pk)` | **CONFERE.** (`app/chamados/models.py:4056`.) |
| Não há ato MATERIALIZACAO | **CONFERE na enumeração do modelo.** (`app/chamados/models.py:4024`; `app/chamados/models.py:4035`.) |
| `_pela_decisao` já lê sequência | **CONFERE**, mas é decisão de exigibilidade, não antigo fold F1. (`app/chamados/services/tarefa_do_dia.py:142`; `app/chamados/services/tarefa_do_dia.py:158`.) |
| RESPOSTA vazia pertence ao admin pelo corte | **CONFERE como corte; implementação ainda contradiz.** Corte mantém fora do app/admin; tarefa devolve RESPONDER/COLABORADOR. (`app/docs/CORTES.md:488`; `app/chamados/services/tarefa_do_dia.py:161`.) |
| C conserva o campo atual | **CONFERE como implementação.** A projeção temporal lê `materializada_em`; não recupera instante apagado. (`app/chamados/juizes.py:1390`; `app/chamados/juizes.py:1400`.) |
| Vigia pode medir projeção como integridade | **CONFERE a natureza documentada.** Seu corpo declara “não julga a pergunta” e conta campos que a porta escreve. (`app/chamados/services/vigia_fio.py:737`; `app/chamados/services/vigia_fio.py:740`; `app/chamados/services/vigia_fio.py:746`.) |
| BUG-PAR ainda fora da BASE | **CAIU segundo registro local do pouso c4+c4c.** (`app/docs/RELATO.md:32`.) |
| BUG-PAR toca explicador/fio | **CONFERE**, com contrato maior que o antigo: testemunha, fallback indisponível e espera declarado×gravado. (`app/chamados/services/explicador.py:124`; `app/chamados/services/explicador.py:132`; `app/chamados/services/fio.py:376`; `app/chamados/services/fio.py:384`.) |
| R3/F1 continuam pré-requisitos não pousados | **CAIU segundo registros locais.** R3 registra pouso; B1 registra NO AR. Ancestralidade Git independente: [nao verificado]. (`app/docs/RELATO.md:235`; `app/docs/RELATO.md:174`.) |
| B2 já pode ser presumida na BASE | **CAIU.** Registro a descreve aberta/em construção. (`app/docs/RELATO.md:30`; `app/docs/BACKLOG.md:824`.) |
| Números históricos demonstram estoque atual | **CAIU como medição atual.** Banco/sondas atuais: [nao verificado]. O helper real de correspondência permanece em `app/chamados/models.py:4081`. |

Referências atualizadas de lei: corte F5 está em **`app/docs/CORTES.md:488`**, juiz-nasce em **`:486`**, L-130 em **`app/docs/LEIS.md:151`**, D04/D05 em **`app/docs/BACKLOG.md:2091`**. O corte U163 já tem registro local em **`app/docs/BACKLOG.md:823`** e **`app/docs/RELATO.md:28`**.

**NOVO** consumidor omitido do brief: `projecao_do_app` chama a consulta e monta o badge. (`app/chamados/services/pergunta_no_app.py:88`; `app/chamados/services/pergunta_no_app.py:100`; `app/chamados/services/pergunta_no_app.py:108`.)

Indiretos confirmados: `CargaDaTarefa` chama partição em `app/chamados/services/tarefa_do_dia.py:530`; cobrança em `app/chamados/services/cobranca.py:48`; aviso em `app/chamados/services/aviso_pendencia.py:9`; API em `app/api/views.py:998` e `app/api/views.py:2538`; core em `app/core/views.py:253`.

Briefs F3/F6/F8/F9 não vieram no dado e não foram localizados como arquivos na cópia: cruzamentos próprios dessas fatias **[nao verificado]**. Não substituir seus briefs por inferência de nomes.

**Numeração textual dos anexos fornecidos**, título=1 e vazias incluídas; equivalência com arquivos externos originais [nao verificado]:

- Brief: marcas em `:20`, `:77`, `:87`, `:110`; contrato `:76-78`; SQL `:92-95`; cruzamentos/ordem `:126-132`; lei/números `:136-146`; pré-requisitos `:150-157`.
- Matriz: contrato `:16-29`; linhas #1..#20 em `:35-54`; indiretos em `:55-58`; transições `:67-78`; saída `:80-95`; pergunta `:98-102`; resumo `:107-108`; corte final **`:110-111`**.
- Atenção: o bullet inteiro do corte final ocupa **uma linha `matriz:111`**. As referências anteriores `:112-114` que enviei foram quebra visual e devem ser descartadas.

## 17/11 01:10 · sessao 4ee3b6d6dd05 · - · - · origem=Ronald (tmux)

Censo realizado com AST em memória de todos os Python de produção sob `app/`, complementado por `rg` em templates, `bin/`, `scripts/`, `eval/`, `mensageria/` e `ops/`. Não importei a aplicação nem executei testes. Acessos inteiramente dinâmicos: [nao verificado]. As referências abaixo são da cópia atual. [nao verificado]

**BASE/documentos**

- `git rev-parse HEAD` falha por ausência de `.git`; SHA independente: [nao verificado].
- Não localizei `AGENTS.md`, os briefs/matrizes F5/F6/F8/F9 nem `logs/ESPINHA-BRIEFS/` nesta cópia; cruzamento pelos briefs dessas quatro fatias: [nao verificado].
- Os offsets documentais mudaram: corte integral `CORTES-F2-F9-1611` em `app/docs/CORTES.md:488`; D04/D05 em `app/docs/BACKLOG.md:2091`.
- D04 está literalmente registrado: atos humanos ocorridos, duas validações com devolução no meio contam duas. (`app/docs/BACKLOG.md:2091`)
- O corte F2–F9 não declara substituir o contrato F3 inteiro pela antiga API; registra decisões específicas F2/F4/F5/F6/F8. (`app/docs/CORTES.md:488`)

**Escritores completos identificados dos campos da pergunta**

| Campo | Escritor/persistência na BASE |
|---|---|
| `resposta_colab` | Manual `app/chamados/services/disputa_emissao.py:1481`→`:1483`; automática `:1674`→`:1677`; reabertura via lote `:1609`→`:1648`; ressuscitação **NOVO** `app/chamados/models.py:1629`→`:1630`; nascimento via `**campos` **NOVO** `:3124`→`:3127`. Simulação, sem persistência: `:1556`. |
| `respondida_em` | Manual `disputa_emissao.py:1482`; automática `:1675`; reparo da trilha **NOVO** `:1696`; lote `:1609`→`:1648`; ressuscitação `models.py:1629`; `carimbar` `:3242`; nascimento `:3124`. Memória: `:1555`,`:3273`. |
| `validada_em/por` | Validação `validacao.py:168`,`:169`→`:171`; devolução `:273`,`:274`→`:276`; devolução BUG-PAR **NOVO** `:349`,`:350`→`:351`; resposta humana limpa `disputa_emissao.py:1494`,`:1495`; lote `:1609`; ressuscitação `models.py:1629`; carimbo `:3245`,`:3246`; nascimento `:3124`. Memória: `:1554`,`:3274`,`:3275`. |
| `materializada_em` | Atribuição individual `_escrever_carimbo`, `models.py:3209`; portas persistem em `:3262`,`:3299`,`:3322`; lote direto **NOVO** `:3391`→`:3393`; nascimento `:3124`. |
| `recusa_motivo` | Escape humano **NOVO** `disputa_emissao.py:1511`→`:1513`; motivo técnico **NOVO** `materializacao.py:1332`→`:1333`; recusa terminal **NOVO** `:1665`→`:1666`; identidade **NOVO** `models.py:3298`→`:3299`; recusa administrativa `validacao.py:827`→`:833`; nascimento `models.py:3124`. |
| `data_conferida_em/por` | `validacao.py:907`,`:908`→`:909`. |
| Disputa `fechada_em/resolucao` | Fechar `models.py:2013`,`:2015`; reabrir admin `:2230`,`:2232`; reabrir sistema `:2254`,`:2256`; complemento de resolução `:2346`; nascimento do modelo/manager: conferir `:1870`,`:1888`. |
| DP | Porta persistente `DecisaoPergunta.registrar`, `models.py:4110`→`bulk_create` em `:4120`; `save` existente recusa alteração em `:4070`. |

`regularizacao_ext.py:199` escreve `RegularizacaoExterna.materializada_em`, outro modelo (`models.py:4152`), portanto não é escritor da pergunta.

**O furo carimbar permanece:** o corpo preenche R/V em `models.py:3242`,`:3245` e salva em `:3262` sem registrar RESPOSTA/VALIDACAO. O comentário de “todo escritor chama registrar” não basta para provar universalidade. (`models.py:4015`)

**Todos os produtores de DP encontrados**

- Já nomeados no brief: `models.py:1613,2005,2137,3129`; `services/disputa_emissao.py:1525,1631,1678,1699`; `services/validacao.py:173,279`; `services/materializacao.py:1045`.
- **NOVO:** `services/validacao.py:354`; `management/commands/backfill_fio_morto_e4b.py:141`; `management/commands/backfill_decisoes_pergunta.py:324`; `app/ponto/services/veredito_celula.py:195`.
- As linhas não são todas “ato novo”: os chamadores de `linhas_da_projecao_sem_linha` podem registrar conservação anterior. (`models.py:4081,4094,4098,4102`)

**Todos os leitores produtivos das funções da F3 encontrados**

| Função | Chamadores atuais |
|---|---|
| `validar_pergunta` | `acoes_disputa.py:19,381`; **NOVO** `api/foto_recebida.py:51`, `declaracao_texto.py:696`, `management/commands/validar_classe_a.py:100`. |
| `recusar_pergunta` | `acoes_disputa.py:82`. |
| `conferir_data_do_fato` | **NOVO** `chamados/views.py:1312`. |
| `_carimbar_validacao` | `validacao.py:432,464,554,655`. |
| `devolver_validacao_recusada` | `validacao.py:693`. |
| `_e_aceite_de_folga` | `validacao.py:623`; **NOVO** `vigia_fio.py:259`, importado de `validacao` em `:247`. |
| `validar_lote_classe_a` | **NOVO** `views_cobrar.py:261,381`; `app/core/views_ia.py:169`. |
| `classe_do_lote` | `validacao.py:978,1165`; **NOVO** `app/api/views_mensageria.py:545`. |
| `fila_validar_classificada` | **NOVO** `views_cobrar.py:288,349,361`. |
| `placar_equipe` | `chamados/views.py:1486`. |
| `placar_aceite` | `painel.py:298`; `chamados/views.py:1488`; `app/folha/views_retrato.py:515` — offset antigo 508 caiu. |
| `marcador_do_placar` | `painel.py:91`. |
| `etag_do_placar` | `chamados/views.py:820`. |
| `montar_contexto_painel` | `chamados/views.py:727,852`; `app/core/management/commands/lavrar_badge_navbar.py:39`; `management/commands/backfill_fio_morto_e4b.py:72`. |
| `marcos_do_lote` | `raiox.py:38`; `fio.py:282` — não 279; `tarefa_do_dia.py:434` — não 423; auditor `management/commands/auditar_invariantes_chamados.py:1163`. |
| `acao_validar_pergunta` | `chamados/views.py:501`; `fio.py:655`; `views_cobrar.py:208`; `validacao.py:989`. |
| `acao_recusar_pergunta` | `chamados/views.py:518`; `fio.py:684`. |
| `acao_reabrir_disputa` | `fio.py:690`. |
| `acao_regularizar_disputa` | `chamados/views.py:475`. |
| `acao_corrigir_resposta` | `chamados/views.py:536`; `fio.py:685`. |

D9 da matriz pressupõe dois leitores em `vigia_fio.py:297,311`; **CAIU** essa localização/quantidade. Hoje existe um sítio importador real em `:247` e um caller em `:259`; `:297` é comentário histórico e `:311` é contador. (`app/chamados/services/vigia_fio.py:247,259,297,311`)

**Leitores globais dos campos fora dos corpos centrais — todos NOVO no censo do brief**

R=`respondida_em`, T=`resposta_colab`, V=`validada_em`, VP=`validada_por[/_id]`, C=`materializada_em`, M=`recusa_motivo`, DC=`data_conferida_*`. Listas incluem `getattr`, atributos, filtros e nomes de campos para transporte; não significam que cada referência decida estado.

| Arquivo | Sítios |
|---|---|
| `app/api/views.py` | T `1163,2561`; R `1165,2563,2577,2620,2637`. |
| `app/api/views_mensageria.py` | T `567`; R `569,570`. |
| `app/chamados/canal.py` | T `65`. |
| `app/chamados/juizes.py` | C `292,683,711,858,1390,1502`; R `352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753`; T `364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052`; V `856,1389`. |
| `app/chamados/models.py` | R `1614,1924,2949,3155,3158,3237,3241,3243,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100`; T `1533,1614,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105`; V `310,344,1614,1923,3151,3237,3244,3247,3331,3496,4091,4102,4103,4104`; VP `1614,3237,3247,4105`; C `1531,1923,2006,3153,3156,3166,3236,3238,3292,3293,3296,3320,3331,3365,3483,3485,3411,3423,3440`; M `3299`. |
| `app/chamados/reconciliador.py` | C `367,630,1050,1145,1155,1505`; T `365,1083`; V `631,1050`. |
| `services/becos.py` | R `30`. |
| `services/declaracao_texto.py` | T/R `417`; R `419,687`; V `227`. |
| `services/disputa_emissao.py` | C `228,776,1084,1104,1340,1637,1930,2062,2073,2090`; R `1483,1635,1677,1692,1698,1703,1735,1843,1861,2074,2091`; T `1483,1635,1663,1677,1692,1700,1861,2079`; V `1484,1485,1496,1537,1538,1636,2062`; VP `1484,1496,1537,1538,1636`; M `1513`. |
| `services/explicador.py` | T `55,102,103`; M `113`. |
| `services/fabrica_da_celula.py` | C `171`. |
| `services/fio.py` | R `187,706,1613`; T `377,1279,1291,1613`; M `564,1407`. |
| `services/foto_e4b.py` | C/R/V `180`. |
| `services/historico.py` | R `99`; V `101`; C `103`; T `109`. |
| `services/materializacao.py` | C `148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127`; T `546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619`; R `1151,2064`; V `199,201,1152,2065,2082`; VP `1152`; M `1333,1652,1655,1663,1666`; DC `169,1851`. |
| `services/passivo_validado.py` | C `64,534,535,536`; R/V/VP/M `66`. |
| `services/pergunta_no_app.py` | R `81`. |
| `services/raiox.py` | C `135,182`; R/T `138,147`. |
| `services/relavra_via.py` | C `181,244,252`. |
| `services/vigia_carimbo.py` | V `98`. |
| `services/vigia_fio.py` | R `636,746`; T `749`. |
| `templatetags/chamados_tags.py` | T `139,166`. |
| `chamados/views.py` | C `1257`; R `1280,1416`. |
| `chamados/views_cobrar.py` | R `169,176`; T `291`. |
| `app/colaboradores/services/calendario.py` | T `504`. |
| `app/colaboradores/services/ficha.py` | T `171`. |
| `app/folha/previa.py` | T `142`. |
| `app/ponto/services/cartorio.py` | T `1641`. |
| `app/ponto/services/dia_do_colab.py` | T `70`. |
| `app/ponto/services/flip_auto.py` | V `130`; M `132`. |
| `app/ponto/services/procedencia_batida.py` | VP `71,72`; C/V `74`. |
| `app/ponto/services/veredito_celula.py` | C `196`; T `197`. |

Nos cinco arquivos centrais, os sítios da BASE foram cobertos pelo mesmo AST; os outros exploradores estão lendo seus corpos e provas. Em particular, a matriz antiga não enumerava os leitores novos BUG-PAR `validacao.py:181,262,269,343,344,380,489,553` nem as novas devoluções. (`app/chamados/services/validacao.py:181,262,269,343,344,380,489,553`)

**Comandos globais — NOVO**

Prefixo `app/chamados/management/commands/`, salvo indicado:

| Comando | Sítios |
|---|---|
| `auditar_disputas.py` | C `71,150`. |
| `auditar_invariantes_chamados.py` | C `470,538,588,905,955,1075,1156,1577,1604,1769`; R `177,592,1105,1605`; T `1106,1657`; V `205,1075,1576,1604,1768,1780`; VP `1076`; M `536,1075,1112`. |
| `backfill_decisoes_pergunta.py` | C `101,325`; R `56,60`; T `61,326,504`; V `56,60,102`; VP `60`. |
| `bug_par_103.py` | C `66`; V/VP `68`; M `69`. |
| `curar_perguntas_em_limbo.py` | T `32,46`; R `46,69`. |
| `dedup_perguntas_fato.py` | T `75`. |
| `encerrar_perguntas_com_fato.py` | C/V `32`. |
| `migrar_data_evento_dia_do_turno.py` | C `147`. |
| `reabrir_orfas_enterradas.py` | C `67,88,91,129,142,160`; V `68`. |
| `validar_classe_a.py` | T `80`. |
| `app/ponto/management/commands/reconciliar_fantasmas.py` | C `213`. |
| `app/ponto/management/commands/reconciliar_grade.py` | R `461`. |
| `bin/censo_folha.py` | R/F `28`; R/V `29`. |
| `bin/sombra_regras_pessoais.py` | T/M `85` — lista de campos para raspagem, não juiz. |
| `bin/esmeril_batida_censo.py` | `marcos_do_lote` `109` — nome em catálogo, não chamada produtiva. |

**Leitores de DP além dos escritores**

- Histórico `models.py:3675`.
- Correspondência de legado `models.py:4094`.
- **NOVO** rótulo BUG-PAR `materializacao.py:275`.
- **NOVO** prefetch `fio.py:263`.
- **NOVO** tarefa `tarefa_do_dia.py:158,159,178,421,430`.
- **NOVO** fotografia `foto_e4b.py:280`.
- **NOVO** medidor/backfill `backfill_decisoes_pergunta.py:99,315`.
- **NOVO** template histórico `app/templates/chamados/partials/modal_fio.html:205,210,212`.

**Templates com leituras reais dos campos**

Prefixo `app/templates/chamados/partials/`:

- `_card_atendimento.html`: T/R `106,143,391`; T `114,141,148,154,182,183`; M `192`. **NOVO no brief**.
- `_pergunta_card.html`: V `20,23,42`; R `24,29,32`; filtro que lê T em `43`; `resposta_display` em `31`. **NOVO no brief**.
- `modal_fio.html`: DC `170,171`; R `185,217,220`; T `219`; M `226`; V `228`; Disputa F `406`. Inputs POST M `61,322` e resolução `384,534,545,557` não são leitura do campo da pergunta. **NOVO no brief**.
- `_lote_preview.html`: T `24`. **NOVO no brief**.
- `detalhe_local.html:284,321` são entradas POST `resolucao`, não campo da pergunta.

**Fechada/resolução: separar alcance**

Disputa `fechada_em` é lida por `acoes_disputa.py:257,349`; POST `resolucao` é lido em `acoes_disputa.py:125`, não uma projeção da resposta. Os leitores globais de F/resolução não se tornam automaticamente escopo F3: API `foto_recebida.py:66,78,84`, `views.py:1018,1034,2678`; juízes `2466`; auditor `592,806`; modelos `1888,1960,1961,1964,2114,2219,2248,2305,2343,2345`; reconciliador `412,413,600,611,612,613,1161,1162,1172`; beco `27,56,57,59`; competência `176,221,229`; emissão `41,541,992,1111,1298,1347,1536,2026,2050,2052,2111,2113,2114`; materialização `1067,1068,1989,2001,2006`; sinais `29,46,131,155`; consumidores de outros módulos podem importar esse fechamento. (`app/chamados/services/acoes_disputa.py:125,257,349`)

**Cruzamentos verificáveis com o dado fornecido**

- Os cinco arquivos F3 centrais permanecem identificáveis: `validacao.py:574`, `acoes_disputa.py:92`, `verbos.py:242`, `painel.py:50`, `veredito_lavrado.py:151`.
- B2 anexa cruza diretamente `validacao.py`, `acoes_disputa.py`, `verbos.py`; não lista `painel.py` nem `veredito_lavrado.py`. Origem dessa lista é o anexo; diff da B2 em voo: [nao verificado].
- F4 fornecida cruza `validacao.py` e consumidores `acoes_disputa.py`, além das portas/escritores compartilhados; não presumir que todo consumidor será editado pela F4. (`validacao.py:209,574`; `acoes_disputa.py:19,381`)
- F7 fornecida tem dependências em `validacao.py`, mas seu núcleo nominal é de templates/fio/modelos; afirmar edição direta de F3 por F7: [nao verificado].
- A remedida F2 enumera leitura de vários campos F3, mas seu núcleo nominal é modelo/juiz; afirmar edição de todos esses consumidores por F2: [nao verificado].
- F5/F6/F8/F9: apenas cortes documentais foram vistos, sem os respectivos briefs; ordem sugerida entre as cinco pelo cruzamento integral: [nao verificado]. (`app/docs/CORTES.md:488`)

## 17/11 01:10 · sessao 7cb1df21275c · - · - · origem=Ronald (tmux)

Referências centrais da BASE:

- A F1 vigente fornece `pergunta_respondida(p)` sobre **instante e texto projetados**, com B/C/D destinados à B2; o antigo contrato D01 não descreve esses corpos. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1552`)
- SQL-R existe, mas consulta projeção, não sequência DP. (`app/chamados/juizes.py:1575`, `app/chamados/juizes.py:1580`)
- `q_vivas` ainda filtra V projetada. O `_q_validada_pelo_ato` pressuposto não foi localizado na cópia; implementação histórica: [nao verificado]. (`app/chamados/models.py:3496`)
- O selo vigente cobre apenas C/V e comparações/`__isnull`/`if` direto, excluindo models/juízes. (`app/chamados/tests/test_contract_leitores_vitalidade.py:14`, `app/chamados/tests/test_contract_leitores_vitalidade.py:320`, `app/chamados/tests/test_contract_leitores_vitalidade.py:328`)

**Correção documental relevante:** `CORTES-F2-F9-1611` está em **`app/docs/CORTES.md:488`**; a linha 487 agora trata de HE. A20 LU-08 está em **`:466`**. (`app/docs/CORTES.md:487`, `app/docs/CORTES.md:488`, `app/docs/CORTES.md:466`)

Classificação das perguntas antigas:

| Pergunta | Classificação |
|---|---|
| C nos comandos até E4d | **DECIDIDO PELO CORTE L1-L2-E4C-F1 \| contestavel**: ato de conclusão na E4d, escrita dupla morre no Contract. (`app/docs/BACKLOG.md:1981`) |
| L1/conservação global | **DECIDIDO PELO CORTE CORTES-F2-F9-1611 \| contestavel**. (`app/docs/CORTES.md:488`) |
| A20 | **DECIDIDO PELO CORTE LU-08 \| contestavel**: conferência vale para o par conferido. (`app/docs/CORTES.md:466`) |
| SQL-V pública ou privada | **DECIDIDO PELO CORTE CORTES-F2-F9-1611, item 2 \| contestavel**, quanto à autoridade: face SQL do juiz, com selo por pergunta. A visibilidade da API é decisão técnica, não lei nova. (`app/docs/CORTES.md:488`) |
| Autor em I18 | **DECIDIDO PELA conservação do corte CORTES-F2-F9-1611 \| contestavel**: conservar exclusão por autor humano; mera existência de VALIDACAO não reproduz o predicado. (`app/docs/CORTES.md:488`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1074`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1076`) |
| Limbo pela projeção ou pelo ato | **DECIDIDO PELO CORTE 13/09 \| contestavel**: reparo fechado, duas carimbadas pela trilha e demais reabertas com texto limpo; mudança do conjunto recusa execução. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:3`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:13`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:17`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`) |
| Selo restrito versus global | **DECIDIDO PELA L-127/Expand-Contract \| contestavel**: proteção dos 12 comandos não demonstra ausência global de leitores antigos; Contract exige censo fechado. (`app/docs/PLANO-PADROES.md:90`, `app/docs/PLANO-PADROES.md:92`, `app/docs/PLANO-PADROES.md:94`, `app/docs/BACKLOG.md:2035`) |

Não extrapolar o item 4 do corte F2-F9 para autorizar genericamente o limbo F9: ele nomeia os reparos **F4 E11/E14/E17**; a prova própria do comando F9 é o corte 13/09 acima. (`app/docs/CORTES.md:488`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:3`)

Numeração dos anexos contada pelas quebras do enunciado:

- **Brief:** `[DEPENDE-F1]` em **28,29,73,77,84,110**; `[DEPENDE-F2]` em **30,75,86,137**. Premissas F1/F2/R3 **119–122**, BUG-PAR **123**, ordem **125–127**, C/E4d **131–133**, L1/A20 **134**, SQL-V privada **135–137**, limbo **138–140**, pré-requisitos **160**. Correspondência com arquivos originais externos: [nao verificado].
- **Matriz:** marcas introdutórias **12,13,22,23**; D01 **33**; D05 **43**; D06 **44**; D07 **47**; D08 **48**; D11 **54**; D14 **59**; D15 **60**; D22 **71**; D23 **72**; D26 **75**; D29 **78**; D32 **81**; D33 **82**; D34 **83**; D35 **84**; D36 **85**; observação semântica **87–90**. P4 **100–102**, P7 **108–110**, P11 **118–119**, P12 **120–121**; perguntas **124–128**. Correspondência externa: [nao verificado].

Não há briefs F3/F5/F6/F8 disponíveis na cópia inspecionada; seu cruzamento literal permanece **[nao verificado]**. A BUG-PAR atual está registrada como c4+c4c pousada, e a implementação inclui a composição K6 e o pulo U163 atual. (`app/docs/BACKLOG.md:2084`, `app/chamados/services/materializacao.py:792`, `app/chamados/services/materializacao.py:795`)

## 17/11 01:10 · sessao 3bd57ddf8a40 · PREPARA-F2-F4-F7 · sonda · origem=Code

**Nenhum número desta rodada justifica mudar a contagem de lei ou de RED que morde: Δlei=0; ΔRED=0.** Permanecem as três pendências da §4 e as ressalvas dos três casos marcados “sim” na §5 do `f4.r`; a sonda não demonstra execução desses RED. A linha final com os N/n originalmente registrados não consta do trecho fornecido. [nao verificado]

Os valores abaixo são transcritos do dado fornecido da **sombra**, sem verificação independente do banco ou do código efetivamente importado naquela execução. Por isso recebem `[nao verificado]`. [nao verificado]

Referências abreviadas: `MD` = `app/chamados/models.py`; `B` = `app/chamados/management/commands/backfill_decisoes_pergunta.py`; `J` = `app/chamados/juizes.py`; `M` = `app/chamados/services/materializacao.py`.

O SCRIPT 1 dimensiona o **pré-backfill** mencionado na §4. O helper conta atos faltantes por **pergunta + ato + instante**; suas saídas são linhas propostas, sem gravá-las. (`MD:4094`, `MD:4098`, `MD:4102`, `MD:4107`)

| Rótulo | Valor informado | O que responde nas §§4–5 |
|---|---:|---|
| `integral_linhas_resposta` | 16941 [nao verificado] | **Dimensiona** a lacuna integral de RESPOSTA. **Derruba** eventual hipótese de pré-requisito integral já satisfeito; não demonstra bug de plantio nem determina trocar o leitor por atos. (`MD:4098`; `J:1552`) |
| `integral_linhas_resposta_em_validadas_nao_materializadas` | 96 [nao verificado] | **Dimensiona** a lacuna de RESPOSTA dentro desse subconjunto operacional. Não significa 96 materializações possíveis: a fila ainda passa pelas guardas da porta. (`MD:4098`; `M:2063`, `M:2084`) |
| `integral_linhas_validacao` | 15115 [nao verificado] | **Dimensiona** a lacuna integral de VALIDACAO; também impede declarar completo o pré-requisito integral. (`MD:4102`) |
| `integral_linhas_validacao_em_validadas_nao_materializadas` | 96 [nao verificado] | **Dimensiona** a lacuna de VALIDACAO nesse subconjunto. A igualdade com os 96 anteriores não prova identidade dos conjuntos de perguntas. (`MD:4098`, `MD:4102`; interseção: [nao verificado]) |
| `integral_perguntas_examinadas` | 50447 [nao verificado] | **Dimensiona** o universo percorrido pelo SCRIPT 1; não é quantidade de perguntas com lacuna, de pessoas ou de RED. [nao verificado] |
| `recorte_oficial_linhas_resposta` | 2992 [nao verificado] | **Dimensiona** RESPOSTAS faltantes no recorte oficial, distinto da frota integral: o medidor filtra horizonte/sem dia/conversa. (`B:55`, `B:59`, `B:67`) |
| `recorte_oficial_linhas_validacao` | 2248 [nao verificado] | **Dimensiona** VALIDACOES faltantes nesse mesmo recorte; não deve ser somado ao integral como estoque adicional. (`B:59`, `B:67`; inclusão efetiva da execução: [nao verificado]) |
| `recorte_oficial_nao_entregue_resposta` | 2992 [nao verificado] | **Confirma a coincidência numérica** entre o subtotal rotulado “não entregue” e o total oficial de RESPOSTA. Não responde quantas U163 estão entregues na fila nem qual RED atravessaria todas as guardas. (`M:795`, `M:808`; classificação executada: [nao verificado]) |
| `recorte_oficial_nao_entregue_validacao` | 2248 [nao verificado] | **Confirma a mesma coincidência** para VALIDACAO. Não demonstra ausência de lacunas em competências entregues na frota integral; o recorte oficial é restrito. (`B:55`, `B:59`; distribuição integral: [nao verificado]) |

Esses números substituem o “sem medição atual” **para essas lacunas**, mas não transformam o backfill em aplicado: o comando declara expressamente que `--aplicar` não escreve as linhas R/V da projeção. (`B:3`; resultado informado: [nao verificado]) [nao verificado]

O SCRIPT 4 responde ao item **paridade do slot** da §5, limitado ao recorte `hora` informado. (`J:361`, `J:376`; seleção exata da sonda: [nao verificado])

| Rótulo | Valor informado | O que responde nas §§4–5 |
|---|---:|---|
| `j1_hora_examinadas` | 7240 [nao verificado] | **Dimensiona** a comparação J1/hora; não mede `data_hora`, cujo parse tem ramo próprio, nem prova todos os chamadores do par. (`J:363`, `M:214`, `M:222`) |
| `j1_hora_divergentes` | 0 [nao verificado] | **Confirma ausência de divergência encontrada nessa comparação**; mantém “não há divergência demonstrada”. Não certifica a correção do resultado pela regra: J1 chama o próprio derivador compartilhado. (`J:376`, `M:599`) |
| `j1_hora_indeterminadas` | 0 [nao verificado] | **Confirma zero casos classificados como indeterminados pela sonda**. O critério exato dessa classificação não está no trecho fornecido; não equivale a zero impedimentos na porta completa. (`M:885`, `M:891`, `M:903`; critério da sonda: [nao verificado]) |

Os avisos do SCRIPT 4 **não são divergências J1×materializador**: registram desacordo entre o marco do chamado-destino e o marco encontrado na ata; o código conserva a prioridade do chamado e retorna o tipo. Portanto, coexistem com `j1_hora_divergentes=0` sem contradição. (`M:527`, `M:568`, `M:576`; ocorrência informada: [nao verificado])

`NoneType: None` é compatível com esse registro usando `exc_info=True` fora de uma exceção ativa; não demonstra, sozinho, queda do script. As mensagens repetidas tampouco devem ser contadas como perguntas ou RED distintos. (`M:571`; `app/core/observ.py:79`; atribuição de cada repetição: [nao verificado])

Os scripts com erro ficaram assim:

| Script | Causa em uma linha | O que ficou sem número |
|---|---|---|
| 2 | O import executado não localizou `chamados.services.universo_bug_par`; o módulo **existe nesta cópia**, portanto a causa da diferença de ambiente/árvore é [nao verificado]. (`app/chamados/services/universo_bug_par.py:12`) | Nenhum indicador desse script foi impresso; seus rótulos exatos não podem ser recuperados porque a seção SONDAS não consta do trecho anexado. [nao verificado] |
| 3 | O `chamados.juizes` executado não ofereceu `texto_sem_carimbo`; a função **existe nesta cópia**, portanto não é ausência comprovada na BASE. (`J:1537`; causa operacional: [nao verificado]) | Nenhum indicador desse script foi impresso; seus rótulos exatos também ficaram [nao verificado]. |

**Continuam sem número**, no conjunto fornecido: meios pares ilegais, absorções retornadas pela função real, U163 entregues/sem dia/aplicáveis, abrangência da lista textual e distribuição nos baldes do contador. Erro de import não autoriza registrar zero nesses itens. [nao verificado]

Também continua aberta a **prova OPUS-C4-B com autoridade real**: os resultados fornecidos não mostram a transição pela validação do admin, e o teste existente ainda instala `competencia_entregue=False`. (`app/chamados/tests/test_bug_par_volta.py:463`, `app/chamados/tests/test_bug_par_volta.py:468`; `app/chamados/tests/test_bug_par_projetado.py:51`)

O `rc_sombra=0` informado não elimina os erros explícitos dos scripts 2 e 3 nem fecha essas medições ausentes. [nao verificado]

## 17/11 01:10 · sessao 93116828942d · - · - · origem=Ronald (tmux)

Auditoria somente leitura; execução dos testes e números de frota: **[nao verificado]**. [nao verificado]

**Corpos e diferenças atuais**

| Unidade | BASE |
|---|---|
| U3/U4 | Permanecem `strip(resposta_colab)` em `app/ponto/services/cartorio.py:1641`, isenção do laço em `:1648`, dedup em `:1665`, via DUPLICATA em `:1650` e transição em `:1651`. |
| U6 | Forma R3 presente: assinatura com `agora` em `app/colaboradores/services/calendario.py:188`, relógio único em `:192`, filtro projetivo em `:504`, C6 com `agora` em `:506`, falha desconhecida em `:509` e `:624`. |
| U7 | Perguntas do app em `app/colaboradores/services/ficha.py:166`; retenção em `:169`; conjunto `falou` em `:170`; strip cru em `:171`; ações admin/colab em `:190` e `:192`. |
| U7b | `respostas_sem_veredito` delega em `app/chamados/models.py:1906`; corpo em `:1910`, exclui V/C em `:1923`, aceita instante OU texto em `:1924` e `:1925`, controla tranca em `:1926`. |
| U8 | `app/folha/previa.py:142` retorna resposta por texto isolado; cai para `fio_vivo` em `:144`. |
| U9 | `app/ponto/services/dia_do_colab.py:70` serializa texto por truthiness; vida é independente em `:71`. |

A F1 atual fornece `pergunta_respondida` por instante E texto em `app/chamados/juizes.py:1544` e `:1552`; face SQL em `:1575` e `:1580`. A docstring destina os leitores B/C/D à B2 em `:1550` e `:1551`. O C6 usa a projeção temporal em `:1404`, recorta resposta futura em `:1394` e `:1395`, e chama o juiz único em `:1423`. O corte F8 L1 está atualmente em **`app/docs/CORTES.md:488`**, não mais `:487`; a L-130 está em `app/docs/LEIS.md:151`.

**Chamadores produtivos completos localizados**

| Função | Chamadores |
|---|---|
| `_julgar_colab_corpo` | `app/ponto/services/cartorio.py:996`, `:1021`, `:1086`. |
| `contexto_calendario` | `app/colaboradores/views.py:1990`; `app/colaboradores/services/drawer.py:123`; `app/relatorios/furos.py:164`; `app/colaboradores/services/calendario.py:157`. |
| `resolver_dia_oferecido_nas_datas` | `app/chamados/services/tarefa_do_dia.py:613`, import em `:607`. |
| `ficha_do_colab` | `app/colaboradores/services/ficha.py:269`, `:275`. |
| `_material` da prévia | `app/folha/previa.py:95`. |
| `dia_do_colab` | `app/colaboradores/services/ficha.py:280`; `app/api/views_mensageria.py:1977`. |
| `respostas_sem_veredito` | `app/chamados/models.py:1934`, `:1967`; `app/chamados/reconciliador.py:1164`; `app/chamados/management/commands/validar_classe_a.py:54`; `app/chamados/services/competencia_trancada.py:126`; `app/chamados/services/fila_por_causa.py:226`; `app/colaboradores/services/desligamento.py:165`; `app/colaboradores/services/ficha.py:169`. |
| `sem_veredito_entre` | `app/chamados/models.py:1906`; `app/chamados/juizes.py:2105`; `app/ponto/supra_juiz.py:573`, `:574`. |

`app/core/juizes.py:886` contém um registro textual da chamada de calendário, não chamada executável. Completude de acesso inteiramente dinâmico: **[nao verificado]**.

**Provas existentes**

| Degrau | Prova lida |
|---|---|
| U3y resposta preservada no dedup | `app/ponto/tests/test_fechar3_troca_escala_gate.py::test_MORDE_fio_dup_c8_nao_fecha_o_velho_com_resposta_do_colab`, definição `:342`, fixture projetiva com instante E texto `:351`–`:352`, asserções `:355`–`:358`. |
| U4 DUPLICATA | Mesmo arquivo `::test_MORDE_fio_dup_dois_so_duplicata_sobrevive_o_mais_novo`, `:303`; `::test_MORDE_fio_dup_o_novo_so_duplicata_fica_e_o_velho_entrega_a_pergunta_viva`, `:325`. |
| U3x sozinho, U4 remoção com via mista | **NENHUM localizado**; os ramos são `app/ponto/services/cartorio.py:1648` e `:1650`. |
| U6 viva com resposta bloqueia | `app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py::test_MORDE_resposta_VIVA_pelo_C6_segue_bloqueando`, `:61`. |
| U6 tranca libera | Mesmo arquivo `::test_RED_resposta_que_o_C6_da_por_MORTA_nao_bloqueia_o_ultimo_recurso`, `:51`. |
| U6 não ler q_vivas | Mesmo arquivo `::test_RED_o_calendario_nao_le_mais_q_vivas`, `:65`. |
| **NOVO** custo calendário | `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py::test_Q_queries_do_perfil_nao_sobem`, `:230`, captura `:232`, limite `:237`. Não demonstra custo constante em N perguntas carimbadas: a propriedade cobrada é limite do perfil fixo, conforme `:235` e `:237`. |
| U7 sem_resposta / U7b integração da ficha | **NENHUM específico localizado**. Os testes de pendências existentes afirmam pilulas/chamados, `app/colaboradores/tests/test_ficha_do_colab.py:72` e `:79`. |
| U8 material por resposta | **NENHUM localizado**. O selo registra ausência de chamadas comportamentais em `app/chamados/tests/test_o261_fio_tem_uma_porta.py:32`. |
| U9 resposta serializada | **NENHUM específico localizado**; os testes do endpoint cobrem marcos/veredito em `app/api/tests/test_dia_do_colab.py:90` e `:100`. |

Os pins permanecem em `app/core/tests/test_medida_turno_do_par.py:275` para calendário `[273]` e `:319` para cartório `[1205,1358]`.

**RED pelo juiz atual**

| Caso | MORDE-HOJE |
|---|---|
| U3 legado texto sem instante, fio morto e demais gates de FIO-DUP satisfeitos | **sim**: juiz L-130 diz não respondida, mas isenção usa texto isolado. (`app/chamados/juizes.py:1552`; `app/ponto/services/cartorio.py:1641`, `:1648`, `:1665`.) |
| U6 texto só espaços, pergunta que o C6 ainda considera viva | **sim**: filtro admite espaços, bloqueia pela vida; juiz de resposta diz falso. (`app/colaboradores/services/calendario.py:504`, `:506`; `app/chamados/juizes.py:1552`.) |
| U6 texto sem instante, C6 ainda viva pelas demais premissas | **sim** pela mesma separação texto/juiz. (`app/colaboradores/services/calendario.py:504`; `app/chamados/juizes.py:1552`.) |
| U7 sem V/C, dia destrancado, qualquer meio par ilegal | **sim**, mas a mordida principal é herdada da retenção: ambos entram em `sem_veredito`, tornam-se `falou` e deixam de ser pendência do colaborador. (`app/chamados/models.py:1923`–`:1926`; `app/colaboradores/services/ficha.py:169`–`:171`.) |
| U8 texto sem instante | **sim**: anuncia “resposta do colaborador”, embora juiz diga falso. (`app/folha/previa.py:142`–`:143`; `app/chamados/juizes.py:1552`.) |
| U9 texto sem instante ou somente espaços | **sim** para apresentação como resposta corrente: truthiness expõe texto sem consultar o juiz. (`app/ponto/services/dia_do_colab.py:70`; `app/chamados/juizes.py:1552`.) |
| Texto do último ato diferente da projeção, mantendo par legal | **não é RED fechado pelo contrato atual**: L-130 define o par projetado e não fornece o redutor D01 antigo. (`app/chamados/juizes.py:1544`–`:1552`; `app/docs/LEIS.md:151`.) |
| V legada e R sintética posterior | **não**: V encerra antes da resposta, alinhada ao corte que conserva V. (`app/chamados/juizes.py:1405`; `app/docs/CORTES.md:488`.) |

Montagens/execução e ocorrência em frota: **[nao verificado]**.

**Numeração literal de matriz.md do enunciado**

Título como linha 1, vazias incluídas; correspondência com arquivo externo original: **[nao verificado]**.

- Marcas `[DEPENDE-F1]`: **8; 24 duas vezes; 25; 26; 27; 34; 35; 42; 50; 56; 75; 89; 106**. O contador textual está em **116**. Portanto, 14 marcas com colchetes, 15 ocorrências contando o contador.
- Marcas `[DEPENDE-R3]`: **10; 56; 57 três vezes; 103**. Contador em **116**: seis marcas com colchetes, sete ocorrências contando o contador.
- Premissas de base/F1/R3/censo: **6–14**.
- U1: **22–28**; U2: **33–36**; U3/U4: **41–43**; U5: **48–51**; U6: **56–57**; U7: **62–63**; U8: **68**; U9: **73**.
- F1 detalhada: **76–84**.
- P1: **87–89**; P2: **90–91**; P3: **92–93**; P4: **94–95**; P5: **96–97**; P6: **98**; P7: **99**; P8: **100–101**; P9: **102**; P10: **103**.
- L1 antiga: **106–109**; degraus sem prova: **111–114**; corte posterior: **118–119**.

## 17/11 01:10 · sessao 4c909f496e91 · - · - · origem=Ronald (tmux)

Censo somente leitura, por AST de atributos (`Load/Store`), keywords ORM, strings de campos e grep dos templates. Campos: **T**=`resposta_colab`, **R**=`respondida_em`, **V**=`validada_em`, **VP**=`validada_por[/_id]`, **C**=`materializada_em`, **CP**=`materializada_por[/_id]`. `NOVO` = ausente do inventário do brief; não indica nascimento recente. Nomes inteiramente dinâmicos e identidade semântica de todos os homônimos: [nao verificado].

### Escritores físicos/infraestrutura

| Campo | Escritor |
|---|---|
| T/R | `responder_pergunta_disputa_core`: `app/chamados/services/disputa_emissao.py:1481`, `:1482`, persistência `:1483`, ato `:1525`. **NOVO no censo de escritores do brief.** |
| T/R | `responder_automaticamente`: `app/chamados/services/disputa_emissao.py:1674`, `:1675`, persistência `:1677`, ato `:1678`. **NOVO.** |
| R | `carimbar_resposta_pela_trilha`: `app/chamados/services/disputa_emissao.py:1696`, `:1698`, ato `:1699`. Já citado como porta. |
| V/VP | Nova resposta invalida V: `app/chamados/services/disputa_emissao.py:1494`, `:1495`, `:1496`. **NOVO.** |
| V/VP | `_carimbar_validacao`: `app/chamados/services/validacao.py:168`, `:169`, `:171`, ato `:173`. **NOVO.** |
| V/VP | `devolver_validacao_recusada`: `app/chamados/services/validacao.py:273`, `:274`, `:276`, ato `:279`. **NOVO.** |
| V/VP | `devolver_paridade_bug_par_103`: `app/chamados/services/validacao.py:349`, `:350`, `:351`, ato `:354`. **NOVO BUG-PAR.** |
| C/CP | `_escrever_carimbo`: `app/chamados/models.py:3209`, `:3210`; persistem `carimbar` `:3262`, `carimbar_identidade` `:3300`, `descarimbar` `:3322`. **NOVO.** |
| R/V/VP | `carimbar(resposta=)` também escreve: `app/chamados/models.py:3242`, `:3245`, `:3246`, persistência `:3262`. **NOVO; esse corpo não registra RESPOSTA/VALIDACAO.** |
| C/CP + campos recebidos | `descarimbar_em_lote`: `app/chamados/models.py:3391`, `:3393`. Chamadores que zeram R/T/V/VP: ressuscitação `:1629`, `:1630`; reabertura `app/chamados/services/disputa_emissao.py:1609`, `:1648`. **NOVO.** |
| R/T por infraestrutura | `_nascer` recebe campos e instancia/salva: `app/chamados/models.py:2949`, `:2950`, `:3124`, `:3127`, conservação `:3129`. **NOVO.** |
| Apenas memória | Simulação de ressuscitação `app/chamados/models.py:1554`, `:1555`, `:1556`; rollback `:3273`, `:3274`, `:3275`. **NOVO; não confundir com persistência.** |
| DP | Escritor único `DecisaoPergunta.registrar`, `app/chamados/models.py:4110`, `:4120`. |

O escritor homônimo de `app/chamados/services/regularizacao_ext.py:199`, `:200`, `:201`, `:220` pertence a outro modelo (`app/chamados/models.py:4152`).

### Os 12 comandos: censo RVC atual

Prefixo **cmd/**=`app/chamados/management/commands/`; **pcmd/**=`app/ponto/management/commands/`.

| Comando | Todos os sítios de campo encontrados por AST |
|---|---|
| `cmd/auditar_invariantes_chamados.py` | R `:177,592,1105,1605`; T `:1106,1657`; V `:205,1075,1576,1604,1768,1780`; VP `:1076`; C `:470,538,588,905,955,1075,1156,1577,1604,1769`. |
| `cmd/backfill_decisoes_pergunta.py` | R `:56,60`; T `:61,326,504`; V `:56,60,102`; VP `:60`; C `:101,325`; CP `:326`. |
| `cmd/reabrir_orfas_enterradas.py` | C `:67,88,91,129,142,160`; CP `:130,143,160`; V `:68`. |
| `cmd/curar_perguntas_em_limbo.py` | T `:32,46`; R `:46,69`. |
| `cmd/auditar_disputas.py` | C `:71,150`. |
| `cmd/encerrar_perguntas_com_fato.py` | C/V `:32`. |
| `cmd/dedup_perguntas_fato.py` | T `:75`. |
| `cmd/migrar_data_evento_dia_do_turno.py` | C `:147`. |
| `cmd/validar_classe_a.py` | T `:80`. |
| `cmd/relavrar_via_i6.py` | Zero acesso direto RVC; importa `universo_i6` em `:46`. |
| `pcmd/reconciliar_fantasmas.py` | C `:213`. |
| `pcmd/reconciliar_grade.py` | R `:461`. |

**Delta importante:** I21 já chama `q_respondida_com_texto()` em vez do antigo pré-filtro R/texto cru; o comentário explicita trim. Portanto D32/P2 histórico deve ser reconferido, não repetido literalmente. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572`, `:1573`, `:1575`, `:1576`, `:1577`.)

Nenhuma atribuição de atributo RVC nem `update/create` RVC foi encontrada pelo AST nesses 12; os dicts de reversão/evento são fotografia, e as persistências passam pelas portas (`app/chamados/management/commands/reabrir_orfas_enterradas.py:129`, `:142`, `:157`, `:160`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`, `:69`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`).

### Leitores/transporte/seleção fora dos 12 — todos NOVO em relação ao inventário do brief

Os sítios seguintes são candidatos sintáticos desambiguados por arquivo, não todos juízes de estado. Referências de escritores também estão aqui quando o mesmo corpo lê/fotografa campos.

```text
app/api/views.py T:1163,2561; R:1165,2563,2577,2620,2637
app/api/views_mensageria.py T:567; R:569,570
app/chamados/canal.py T:65

app/chamados/juizes.py
 C:292,683,711,858,1390,1502
 R:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753
 T:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052
 V:856,1389

app/chamados/models.py
 C:1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485
 CP:3210,3236,3238,3293,3296,3320,3365,3393
 R:1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100
 T:1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105
 V:310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104
 VP:1614,1629,3237,3246,3247,3275,4105

app/chamados/reconciliador.py
 C:367,630,1050,1145,1155,1505; CP:1146,1156; T:365,1083; V:631,1050

app/chamados/services/acoes_disputa.py R:97,132; T:355; V:347
app/chamados/services/becos.py R:30
app/chamados/services/declaracao_texto.py R:417,419,687; T:417; V:227
app/chamados/services/disputa_emissao.py
 C:228,776,1084,1104,1340,1637,1930,2062,2073,2090
 R:1482,1483,1609,1635,1675,1677,1692,1696,1698,1703,1735,1843,1861,2074,2091
 T:1481,1483,1609,1635,1663,1674,1677,1692,1700,1861,2079
 V:1484,1485,1494,1496,1537,1538,1609,1636,2062
 VP:1484,1495,1496,1537,1538,1609,1636
app/chamados/services/explicador.py T:55,102,103
app/chamados/services/fabrica_da_celula.py C:171
app/chamados/services/fio.py R:187,706,1613; T:377,1279,1291,1613
app/chamados/services/foto_e4b.py C/R/V:180
app/chamados/services/historico.py R:99; V:101; C:103; T:109
app/chamados/services/materializacao.py
 C:148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127
 CP:1154; R:1151,2064
 T:546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619
 V:199,201,1152,2065,2082; VP:1152
app/chamados/services/painel.py C/R/V:67; V:270,271,272,273; VP:262
app/chamados/services/passivo_validado.py C:64,534,535,536; CP:64,535; R/V/VP:66
app/chamados/services/pergunta_no_app.py R:81
app/chamados/services/raiox.py C:135,182; R/T:138,147
app/chamados/services/relavra_via.py C:181,244,252
app/chamados/services/validacao.py
 C:262,264,341,363,556,681,701,717; CP:342,363
 R:594,813,824,848,1167
 T:175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109
 V:168,171,173,181,262,269,273,276,300,302,343,349,351,364,489,553,597,701,810,975,996
 VP:169,171,181,262,274,276,300,302,343,350,351,364,489
app/chamados/services/verbos.py R:335,336,342,345; V:264,270; VP:264,265,266,268
app/chamados/services/vigia_carimbo.py V:98
app/chamados/services/vigia_fio.py R:636,746; T:749
app/chamados/templatetags/chamados_tags.py T:139,166
app/chamados/veredito_lavrado.py C:203,204; R:207,208
app/chamados/views.py C:1257; R:1280,1416
app/chamados/views_cobrar.py R:169,176; T:291
app/chamados/management/commands/bug_par_103.py C:66; CP:67; V/VP:68

app/colaboradores/services/calendario.py T:504
app/colaboradores/services/ficha.py T:171
app/folha/previa.py T:142
app/ponto/services/cartorio.py T:1641
app/ponto/services/dia_do_colab.py T:70
app/ponto/services/flip_auto.py V:130
app/ponto/services/procedencia_batida.py C/V:74; CP/VP:71,72
app/ponto/services/veredito_celula.py C:196; T:197

bin/censo_folha.py R:28,29; V:29
bin/sombra_regras_pessoais.py T:85
```

Templates, também NOVO no censo repo-wide do brief:

- `app/templates/chamados/partials/_card_atendimento.html`: R/T/display `:106`, T `:114,141,143,148,154,182,183`, R/T/display `:391`.
- `app/templates/chamados/partials/_lote_preview.html`: T `:24`.
- `app/templates/chamados/partials/_pergunta_card.html`: V `:20,23,42`; R `:24,29,32`.
- `app/templates/chamados/partials/modal_fio.html`: R `:185,217,220`; T `:219`; V `:228`.

### Funções dos comandos e seus consumidores

Os três importadores produtivos diretos conferem:

| Exportação | Leitor |
|---|---|
| `universo_i6` (`app/chamados/management/commands/auditar_invariantes_chamados.py:455`) | `app/chamados/management/commands/relavrar_via_i6.py:46` |
| `aplicar` / `CONTADORES_DO_APPLY` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:163`) | `app/chamados/management/commands/dedup_perguntas_fato.py:104` |
| `_parse_pendentes` / `MODULOS_ALVO` (`app/ponto/management/commands/reconciliar_fantasmas.py:58`) | `app/ponto/worklist.py:37` |

**NOVO consumo nominal**, sem chamar o comando: proposta de ação `validar_classe_a` em `app/core/views_ia.py:134`, `:138`; `mensageria/nucleo/views.py:156`. O bin já citado conserva o nome em `bin/gerar_haiku_dentes.py:203`.

Cron/configuração tem mais sítios nominais que os três despachos: `app/config/crons.py:126,392,462,542,598,605,616,747,799,803,829,1357,1363,1405,1415,1470,1476,2192,2219`. Não são todos execuções de cron; há catálogos/declarações de não-cron.

### Testes consumidores — cuidado com os números antigos [nao verificado]

Grep literal dos nomes dos comandos em arquivos de teste encontrou **arquivos**, não testes/assertivas: auditor 29, backfill 15, reabrir 4, curar 3, auditar_disputas 7, encerrar 4, dedup 10, migrar 14, validar 3, relavrar 0, fantasmas 17, grade 45. Isso não reproduz as contagens antigas 31/17/11/15/20/49: alcance do levantamento histórico [nao verificado]. Não usar esses números como frota nem como prova funcional. [nao verificado]

Âncoras para o conjunto de provas adicional do censo:

- Auditor: `app/chamados/tests/test_contract_texto_com_carimbo.py:18` **NOVO**, `test_contract_leitores_vitalidade.py:89,271,276`, `test_o261e_f2_ramo_e_i20.py:271,298`, `test_c5_auditor.py:37`, `test_selo_i6_ouve_a_lei.py:29,163,175,185,196`.
- Backfill: `app/chamados/tests/test_e1_v9.py:212,235,250,261`, `test_e1_v12.py:283,292,293`, `test_e1_v13d_exportada_fora.py:118,168,182,324`, `test_e3_catraca_respondedores.py:68,81`, `test_porta_reapontamento_pergunta.py:86,205,209,213`.
- Limbo: `app/chamados/tests/test_curar_perguntas_em_limbo.py:58,66,70,76,84`, `test_e3_catraca_respondedores.py:591,594`, `test_e3_porta_A_reabrir_nao_planta_segunda.py:152`.
- Grade: `app/ponto/tests/test_f1c4_3_reconciliar_grade_pergunta_ao_juiz.py:51` **NOVO**, `test_cq_c6.py:16`, `app/escala/tests/test_cobranca_orfa_regeneracao.py:279`.
- Fantasmas: `app/ponto/tests/test_varredura_fantasmas_evento.py:43,49,55,61,62`, `test_selo_s141_pergunta_ao_nucleo.py:58,93`.
- Validar classe A: `app/chamados/tests/test_validar_classe_a.py:69`, `test_validacao_respeita_tranca.py:123`; proposta nominal **NOVO** `app/core/tests/test_ia_escreve_com_sim.py:91,103,150`.

Não executei testes. Resultado de suíte/mutantes: [nao verificado]. [nao verificado]

## 17/11 01:10 · sessao 4b9273ea74d8 · - · - · origem=Ronald (tmux)

Somente leitura; nenhum teste/banco executado. Provas abaixo são leitura estática dos testes; resultado de execução: [nao verificado]. [nao verificado]

**Mudanças relevantes da BASE**

- L-130 exige instante **e** texto não branco; a F1 pousada nomeia esse juiz sobre a projeção e destina leitores B/C/D à B2. Não é o D01 antigo. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`; `app/chamados/juizes.py:1552`)
- O corte CORTES-F2-F9 está em **`app/docs/CORTES.md:488`**, não :487; :487 trata HE. O item 4 mantém guardas de reparo na projeção; item 2 exige paridade integral da futura face SQL-V. (`app/docs/CORTES.md:488`)
- I21 já migrou a seleção R para a face SQL real; o filtro V/C ainda é projetivo. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1574`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1576`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1577`)
- **P8 contém erro de contrato:** `universo_i6()` retorna `(set de pks em violação, dict via -> lista de pks de piso)`, e não pares `(pk, materializada_em)`. A leitura desses pares é interna. O consumidor usa `viol,_` e `pk__in=viol`. (`app/chamados/management/commands/auditar_invariantes_chamados.py:455`; `app/chamados/management/commands/auditar_invariantes_chamados.py:457`; `app/chamados/management/commands/auditar_invariantes_chamados.py:469`; `app/chamados/management/commands/relavrar_via_i6.py:49`; `app/chamados/management/commands/relavrar_via_i6.py:55`)

**Matriz de estados / provas**

| Estado | Juiz/operação | Persistido | Retorno | Prova existente |
|---|---|---|---|---|
| Grade órfã com marco igual | Comparação de horas antes de olhar perguntas | No apply, remove contexto | `ORFA_JA_CONFERE` | `app/escala/tests/test_cobranca_orfa_regeneracao.py::test_marco_igual_apenas_sai_da_fila` (:326); corpo `app/ponto/management/commands/reconciliar_grade.py:449` |
| Grade órfã com marco diferente; resposta legal e pergunta viva | Prefiltro R projetado → `fio_vivo` | Nada | `ORFA_FIO_PRESO` | `app/escala/tests/test_cobranca_orfa_regeneracao.py::test_ADVERSARIAL_pergunta_respondida_barra_a_reescrita` (:332; asserção :345); corpo `app/ponto/management/commands/reconciliar_grade.py:460` |
| Grade órfã, sem fio preso, dry | Mesmo fluxo | Nada | `ORFA_DRY` | `app/escala/tests/test_cobranca_orfa_regeneracao.py::test_shadow_nao_escreve` (:318), parcial; corpo `app/ponto/management/commands/reconciliar_grade.py:463` |
| R sem texto, pergunta ainda viva | Juiz respondida=False; prefiltro atual aceita R | Nada no dry | Hoje `ORFA_FIO_PRESO`; regra pede não tratar como respondida | **NENHUM** específico; `app/chamados/juizes.py:1552`; `app/ponto/management/commands/reconciliar_grade.py:461` |
| Fantasma, marco apagado, C6 viva, dentro da janela | Espelho → C6 → janela → demais guardas | Dry nada; apply porta `reabrir_fantasma`, disputa/pai e trilha | Contadores/reabertura | `app/chamados/tests/test_reabrir_orfas_enterradas.py::test_RED_lampada_apagada_na_aberta_volta_para_a_fila_pela_porta` (:99; asserções :106); corpo `app/chamados/management/commands/reabrir_orfas_enterradas.py:83`, :88, :157 |
| Mesmo fantasma, fora da janela | Carimbo C | Nada | `FORA_DA_JANELA` | `app/chamados/tests/test_reabrir_orfas_enterradas.py::test_RED_fora_da_janela_do_aval_e_listado_e_nao_tocado` (:202); corpo `app/chamados/management/commands/reabrir_orfas_enterradas.py:88` |
| Reabertura apply | Foto projetada + portas | Reversão antes, trilha, alteração pela porta | Contador | `app/chamados/tests/test_reabrir_orfas_enterradas.py::test_RED_trilha_simetrica_e_reversao_ANTES` (:209; chaves :214); corpo `app/chamados/management/commands/reabrir_orfas_enterradas.py:129`, :142, :157 |
| C presente/V ausente/via sem história/marco aceso | Seleção projetada → `fato_do_marco_no_espelho` | Dry nada; apply via pela porta e possível fecho | Candidatas, carimbadas, disputas fechadas | **NENHUM** confirmado para divergência V/ato; corpo `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`, :41, :53, :68, :83, :88 |
| Limbo mudou frente a MEDIDAS | Universo projetado + igualdade nominal | Nada | `RECUSADO` | `app/chamados/tests/test_curar_perguntas_em_limbo.py::test_MORDE_recusa_se_o_limbo_mudou` (:69); predicado ORM é mockado (:59–65), portanto sua prova específica é **NENHUM**; corpo `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, :48 |
| Limbo nominal intacto, dry | Mesmo universo | Nada | Plano e `DRY-RUN` | `app/chamados/tests/test_curar_perguntas_em_limbo.py::test_dry_run_diz_o_que_faria_e_nao_escreve` (:75); corpo `app/chamados/management/commands/curar_perguntas_em_limbo.py:64` |
| Limbo carimbável com nota real | `nota_da_resposta` → porta retroativa | R + RESPOSTA retroativa; demais pela reabertura | Contadores | `app/chamados/tests/test_curar_perguntas_em_limbo.py::test_MORDE_carimba_com_o_instante_da_trilha` (:29; ato :41), prova da porta, não do predicado/nota ORM; corpo `app/chamados/management/commands/curar_perguntas_em_limbo.py:32`, :67, :69 |
| Lacuna R/V no acervo | Helper real de correspondência pergunta+ato+instante | `linhas_a_criar` não escreve | Dicionário agrupado por competência | **NENHUM** específico confirmado da seleção; `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, :48, :56, :67 |
| Admin decidiu sem ato | C/V/via projetados + ausência SILENCIO/VALIDACAO | Medição nada; apply do acervo tem porta separada | Pks | **NENHUM** específico confirmado; `app/chamados/management/commands/backfill_decisoes_pergunta.py:94`, :99, :101` |
| Apply do backfill | Porta de acervo, não apply das linhas R/V | Entre outros atos, SILENCIO com foto C/autor/texto | Contadores | `app/chamados/tests/test_e1_v9.py::test_backfill_veta_pela_mesma_porta` (:210), parcial de mérito; corpo `app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, :163, :325` |
| I18 | V humana, C nula, sem recusa; exclui autor sistema | Nada | Linha I18 | **NENHUM** específico V/ato/autor confirmado; `app/chamados/management/commands/auditar_invariantes_chamados.py:1074`, :1075, :1076 |
| I20 novo / cicatriz | Integridade da projeção; R nula e T != vazio, exceto lista nominal | Nada | Violação / silêncio | `app/chamados/tests/test_o261e_f2_ramo_e_i20.py::test_caso2_dia_regularizado_o_vigia_nao_conta_e_o_i20_conta` (:258, controles :248, asserção :244); `::test_MORDE_caso2_na_cicatriz_o_i20_cala_e_o_selo_acusa` (:274); corpo `app/chamados/management/commands/auditar_invariantes_chamados.py:1101`, :1105 |
| I21 | SQL real respondida + V/C projetados + destino/pai fora de vivos | Nada | Violação ou piso por ausência aprovada | **NENHUM** específico V/ato confirmado; `app/chamados/management/commands/auditar_invariantes_chamados.py:1575`, :1581 |
| C2 | R/V/C todos nulos, célula absolvendo | Nada | Linha C2 | **NENHUM** confirmado; `app/chamados/management/commands/auditar_invariantes_chamados.py:1603`, :1604 |
| F1 sem pergunta viva | Depois de C6, presença de texto cru isenta | Nada | Linha F1 ou silêncio | `app/chamados/tests/test_selo_f1_cego.py::test_MORDE_chamado_vivo_SEM_pergunta_nenhuma_entra_no_F1` (:86), parcial; ramo T isolado **NENHUM**; corpo `app/chamados/management/commands/auditar_invariantes_chamados.py:1655`, :1657 |
| F2/F3 | Pré-filtro V projetado; depois C6 e lei/célula | Nada | Linhas F2/F3 | F2 V: **NENHUM**; F3 parcial `app/chamados/tests/test_hx_f3_lei_cala.py::test_f3_viola_quando_lei_cala` (:47); corpo `app/chamados/management/commands/auditar_invariantes_chamados.py:1767`, :1780, :1784 |
| I6 sem lastro | C ou PMF, C6, porta de alcance; piso nominal | Nada | Set violação, dict piso | `app/chamados/tests/test_selo_i6_ouve_a_lei.py::test_MORDE_carimbo_SEM_fato_e_acusado` (:78), `::test_MORDE_fila_viva_nao_e_acusada` (:105), `::test_MORDE_adesao_e_piso_nao_violacao` (:159); corpo `app/chamados/management/commands/auditar_invariantes_chamados.py:455`, :538, :540 |
| Auditor disputas cegas/sem destino | C nula/disputa aberta | Nada | Contadores | `app/chamados/tests/test_auditar_disputas_padroes.py::test_auditor_detecta_cada_padrao_e_lista_sem_quebrar` (:94) cobre existência; `::test_pergunta_cega_de_disputa_FECHADA_nao_conta` (:80) cobre fecho. Ramo C diferencial **NENHUM**; corpo `app/chamados/management/commands/auditar_disputas.py:71`, :150 |
| Dedup | Texto cru apenas escolhe rótulo; plano/porta reais escolhem ações | Dry nada; apply `aplicar` | `(com resposta)` / `(menor id)` | Rótulo diferencial **NENHUM**; corpo `app/chamados/management/commands/dedup_perguntas_fato.py:75`, :98, :104 |
| Classe A | Particionar → retenção → classificação; texto só na impressão | Dry nada; apply valida e assina | Listagem/contadores | `app/chamados/tests/test_validar_classe_a.py::test_MORDE_dry_run_nao_valida` (:91), `::test_MORDE_apply_sem_autor_e_recusado` (:97), `::test_apply_com_autor_valida_e_assina` (:105); impressão R/ato **NENHUM**; corpo `app/chamados/management/commands/validar_classe_a.py:54`, :80, :85, :100 |
| Migrar data | C escolhe fonte materializada/viva | Classificar nada; apply posterior altera dia pela porta | Classificação | `app/chamados/tests/test_migrar_data_evento_dia_do_turno.py::test_MORDE_as_vivas_vao_ao_dia_do_turno_e_o_diurno_fica` (:126), parcial; corpo `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, :151 |
| Relavrar I6 | API real I6 → via de hoje → porta | Dry nada; apply relavra vias/vereditos | Plano/contadores | **NENHUM** confirmado; corpo `app/chamados/management/commands/relavrar_via_i6.py:49`, :64, :96, :102 |
| Fantasmas `--alarme` | Desliga apply; C apenas monta string de ligação | Nada | Alarme/relatório | Ramo C específico **NENHUM**; corpo `app/ponto/management/commands/reconciliar_fantasmas.py:153`, :210, :213, :217 |

**RED pela regra atual**

| Caso | Exigido | MORDE-HOJE |
|---|---|---|
| Grade órfã com horas diferentes; R presente/T branco; C6 viva por célula acusadora ou sem C | Não prender a relavra **como resposta dada**; no dry `ORFA_DRY` | **sim**. Juiz False, mas R-only inclui e retorna preso. (`app/chamados/juizes.py:1552`; `app/chamados/juizes.py:1454`; `app/chamados/juizes.py:1482`; `app/ponto/management/commands/reconciliar_grade.py:437`; :461; :462) |
| I2: chamado terminal, C ausente, disputa aberta, R presente/T branco | Não entrar no piso “respondidas” | **sim**. Piso testa apenas R. (`app/docs/LEIS.md:151`; `app/chamados/management/commands/auditar_invariantes_chamados.py:592`; :596) |
| Contraprova I21a recebendo ausência aprovada + R presente/T branco | Acusar falta de resposta válida | **sim** em entrada direta do helper; I21 normal já não produz essa entrada, pois usa SQL real. (`app/chamados/management/commands/auditar_invariantes_chamados.py:177`; :1575; `app/chamados/juizes.py:1552`) |
| F1 chamado vivo sem pergunta viva; única fala T presente/R ausente, demais condições de F1 satisfeitas | Texto isolado não basta para “respondida” pela L-130 | **sim** para o ramo cru, se essa leitura for a finalidade “resposta” da L-130; sentido “fala registrada” distinto: [nao verificado]. (`app/docs/LEIS.md:151`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1633`; :1657) |
| Antigo RED DP RESPOSTA + projeção R zerada → grade presa | Não é gabarito dedutível da F1 vigente | **não**, premissa D01 caiu; não classificar diferença como bug atual. (`app/chamados/juizes.py:1544`; :1552) |
| Antigo RED V só no ato → auditor/comandos excluem | Medir divergência física; sobrepor V pela API antiga não está disponível | **não** como RED fechado pela premissa antiga. O corte permite face SQL do juiz registrado, não fornece implementação nem revoga toda conservação. (`app/docs/CORTES.md:488`; `app/chamados/juizes.py:1389`; :1405) |
| Limbo selecionado por ato sem projeção, fora de MEDIDAS | Manter reparo da própria projeção e trava nominal | **não**; mudar universo contrariaria corte/escopo de reparo existente. (`app/docs/CORTES.md:488`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:3`; :13; :46; :48) |
| I20 novo/T sem R e cicatriz | Continuar acusando novo, calando lista nominal | **não**, código/provas existentes já cobrem. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1101`; :1105; `app/chamados/tests/test_o261e_f2_ramo_e_i20.py:258`; :274) |
| Selo atual diante `p.respondida_em`/`getattr(...validada_em)` | Selo final requerido deveria alcançar essas leituras | **sim como lacuna de alcance**; atual só C/V e comparações específicas. (`app/chamados/tests/test_contract_leitores_vitalidade.py:14`; :293; :330; :340) |

O RED da contraprova I21a é unitário de função, mas não prova incidência pela chamada normal; não somá-lo como bug alcançável de produção sem marcar essa diferença. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1575`; :1581; :1587)

## 17/11 01:10 · sessao 19a066405f0e · - · - · origem=Ronald (tmux)

Censo executado por AST em memória de atributos (`Load`/`Store`), keywords ORM e strings de campos em Python, mais grep de templates; não importei a aplicação nem executei testes/banco. Exaustividade para nomes construídos dinamicamente: [nao verificado]. [nao verificado]

`NOVO` abaixo significa ausente da enumeração do brief, não necessariamente código recém-criado.

**Escritores físicos / memória**

| Campo | Escritores efetivos |
|---|---|
| R/T | Resposta humana `app/chamados/services/disputa_emissao.py:1481`, `:1482`, `:1483`; automática `:1674`, `:1675`, `:1677`; trilha R `:1696`, `:1698`. |
| V | Resposta nova limpa V `app/chamados/services/disputa_emissao.py:1494`, `:1496`; validação `app/chamados/services/validacao.py:168`, `:171`; devolução `:273`, `:276`; **NOVO** devolução BUG-PAR `:349`, `:351`. |
| R/T/V por lote | Reabertura prepara zeragem em `app/chamados/services/disputa_emissao.py:1609`, preserva foto `:1631` e passa à porta `:1648`; **NOVO** ressuscitação conserva DP `app/chamados/models.py:1613` e zera R/V/T `:1629`, `:1630`; persistidor do lote `:3391`, `:3393`. |
| R/V via carimbo | `app/chamados/models.py:3240`, `:3242`, `:3245`, persistência `:3262`. **NOVO contra a premissa universal do brief:** esse corpo pode escrever R/V sem RESPOSTA/VALIDACAO respectivos. |
| C | `_escrever_carimbo` é atribuidor em memória `app/chamados/models.py:3207`, `:3209`; persistem portas `:3262`, `:3299`, `:3322`; lote `:3393`. |
| **NOVO nascimento por `**campos`** | `app/chamados/models.py:3124`, `:3127`; conservação DP `:3129`. |
| Só memória | Simulação ressuscitação `app/chamados/models.py:1554`, `:1555`, `:1556`; rollback carimbo `:3273`, `:3274`. |
| Homônimo excluído | `app/chamados/services/regularizacao_ext.py:199` grava outro modelo, definido em `app/chamados/models.py:4152`. Não é escritor C de PerguntaDisputa. |

**Sequência DP**

Persistidor único: `app/chamados/models.py:4110`, `:4120`. Produtores: `app/chamados/models.py:1613`, `:2005`, `:2137`, `:3129`; `app/chamados/services/disputa_emissao.py:1525`, `:1631`, `:1678`, `:1699`; `app/chamados/services/validacao.py:173`, `:279`, **NOVO** `:354`; `app/chamados/services/materializacao.py:1045`; `app/ponto/services/veredito_celula.py:195`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.

Leitores de DP/autoridade de conservação: `app/chamados/models.py:3675`, `:4094`; **NOVO rótulo BUG-PAR** `app/chamados/services/materializacao.py:275`; prefetch `app/chamados/services/fio.py:263`; tarefa `app/chamados/services/tarefa_do_dia.py:159`, `:178`, `:421`, `:430`; foto `app/chamados/services/foto_e4b.py:280`; backfill `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`, `:99`, `:315`.

**Grafo de funções F5 — chamadas produtivas encontradas**

| Função | Chamadores |
|---|---|
| `espera_o_texto_do_colaborador` | `app/chamados/services/fio.py:174`, `:359`; `app/chamados/services/cobranca.py:241`. |
| `processar_post_fio` | `app/chamados/views.py:674`, `:954`. |
| `canonica_do_grupo` | `app/chamados/services/fio.py:1003`, `:1900`, `:2223`; **NOVO** `app/chamados/models.py:3031`; `app/chamados/services/declaracao_texto.py:253`, `:275`; `app/chamados/management/commands/dedup_perguntas_fato.py:76`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:276`, `:393`, `:506`. |
| `merito_perdedor` | `app/chamados/services/fio.py:1335`. |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:1620`. |
| `qs_perguntas_absolviveis` | `app/chamados/reconciliador.py:652`; **NOVO** `app/chamados/management/commands/reconciliar_chamados.py:92`. |
| `_orfas_abertas` | `app/chamados/reconciliador.py:1135`, `:1192`, `:1303`. |
| `veredito_orfa` | `app/chamados/reconciliador.py:1110`, `:1136`; wrapper `fantasma_pela_lampada` chamado em `:1193`, `:1304`. |
| `encerrar_orfas_pela_lampada` | `app/chamados/reconciliador.py:1376`. |
| `perguntas_orfas_reatavies` | `app/chamados/reconciliador.py:1522`; **NOVO** `app/chamados/management/commands/reconciliar_chamados.py:97`, `:98`. |
| `_explicar_validada` / `explicar_pergunta` | `app/chamados/services/explicador.py:137`, `:171`; wrapper `por_que_nao_fecha` em `app/chamados/services/fio.py:348`. |
| `_resposta_validada` | `app/chamados/services/declaracao_texto.py:285`, `:378`, `:668`. |
| `declarar_hora_colab` / `aplicar_declaracao` | `app/chamados/views.py:1090`, `:480`. |
| `censo_alcance` | **NOVO** `app/chamados/management/commands/backfill_fio_morto_e4b.py:78`; `app/chamados/management/commands/vigiar_fio_morto.py:181`. |
| `respondida_sem_resposta` | `app/chamados/management/commands/vigiar_fio_morto.py:183`. |
| `creditos_perguntas` | `app/chamados/services/fio.py:436`; **NOVO** `app/chamados/services/verbos.py:34`. |
| `pergunta_sem_acao_possivel` | `app/chamados/services/fio.py:317`; função `pks_stale_por_varredura` consumida em **NOVO** `app/chamados/management/commands/lavrar_perguntas_stale.py:60`. |
| `particao_do_app` | `app/chamados/services/pergunta_no_app.py:73`; `app/chamados/services/tarefa_do_dia.py:530`. |
| `consulta_do_app` | `app/chamados/services/pergunta_no_app.py:100`; `app/chamados/services/cobranca.py:48`; `app/chamados/services/aviso_pendencia.py:9`; `app/core/views.py:253`; `app/api/views.py:998`, `:2538`. |
| `becos.classificar` | `app/chamados/services/becos.py:74`; **NOVO** `app/chamados/management/commands/fechar_becos_disputa.py:39`. |
| `_eventos_dos_chamados` | `app/chamados/services/historico.py:128`, `:151`; `historico_do_chamado` também em `:175`. |
| `vigia_carimbo.classificar` | `app/chamados/services/vigia_carimbo.py:124`; **NOVO** `app/chamados/services/passivo_validado.py:327`; `app/chamados/services/vigia_fio.py:415`. |

**Censo global Python dos quatro campos (leitura, escrita, filtro, transporte), produção**

Legenda R=`respondida_em`, T=`resposta_colab`, V=`validada_em`, C=`materializada_em`. Linhas já classificadas como escritores acima não constituem leitores novos. Arquivos fora dos 14 do brief são ampliação **NOVO**; dentro dos 14, os sítios omitidos do brief também são **NOVO** (ex.: `fio.py:377`).

```text
app/api/views.py R:1165,2563,2577,2620,2637 T:1163,2561
app/api/views_mensageria.py R:569,570 T:567
app/chamados/canal.py T:65
app/chamados/juizes.py C:292,683,711,858,1390,1502
 R:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753
 T:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052
 V:856,1389
app/chamados/models.py C:1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485
 R:1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100
 T:1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105
 V:310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104
app/chamados/reconciliador.py C:367,630,1050,1145,1155,1505 T:365,1083 V:631,1050
app/chamados/services/acoes_disputa.py R:97,132 T:355 V:347
app/chamados/services/becos.py R:30
app/chamados/services/declaracao_texto.py R:417,419,687 T:417 V:227
app/chamados/services/disputa_emissao.py C:228,776,1084,1104,1340,1637,1930,2062,2073,2090
 R:1482,1483,1609,1635,1675,1677,1692,1696,1698,1703,1735,1843,1861,2074,2091
 T:1481,1483,1609,1635,1663,1674,1677,1692,1700,1861,2079
 V:1484,1485,1494,1496,1537,1538,1609,1636,2062
app/chamados/services/explicador.py T:55,102,103
app/chamados/services/fabrica_da_celula.py C:171
app/chamados/services/fio.py R:187,706,1613 T:377,1279,1291,1613
app/chamados/services/foto_e4b.py C/R/V:180
app/chamados/services/historico.py C:103 R:99 T:109 V:101
app/chamados/services/materializacao.py C:148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127
 R:1151,2064
 T:546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619
 V:199,201,1152,2065,2082
app/chamados/services/painel.py C/R:67 V:67,270,271,272,273
app/chamados/services/passivo_validado.py C:64,534,535,536 R/V:66
app/chamados/services/pergunta_no_app.py R:81
app/chamados/services/raiox.py C:135,182 R/T:138,147
app/chamados/services/regularizacao_ext.py C:199,201,220 [outro modelo]
app/chamados/services/relavra_via.py C:181,244,252
app/chamados/services/validacao.py C:262,264,341,363,556,681,701,717
 R:594,813,824,848,1167
 T:175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109
 V:168,171,173,181,262,269,273,276,300,302,343,349,351,364,489,553,597,701,810,975,996
app/chamados/services/verbos.py R:335,336,342,345 V:264,270
app/chamados/services/vigia_carimbo.py V:98
app/chamados/services/vigia_fio.py R:636,746 T:749
app/chamados/templatetags/chamados_tags.py T:139,166
app/chamados/veredito_lavrado.py C:203,204 R:207,208
app/chamados/views.py C:1257 R:1280,1416
app/chamados/views_cobrar.py R:169,176 T:291
app/colaboradores/services/calendario.py T:504
app/colaboradores/services/ficha.py T:171
app/folha/previa.py T:142
app/ponto/management/commands/reconciliar_fantasmas.py C:213
app/ponto/management/commands/reconciliar_grade.py R:461
app/ponto/services/cartorio.py T:1641
app/ponto/services/dia_do_colab.py T:70
app/ponto/services/flip_auto.py V:130
app/ponto/services/procedencia_batida.py C/V:74
app/ponto/services/veredito_celula.py C:196 T:197
bin/censo_folha.py R:28,29 V:29
bin/sombra_regras_pessoais.py T:85
```

Comandos de `app/chamados/management/commands/`, **NOVO como ampliação além do brief**:

```text
auditar_disputas.py C:71,150
auditar_invariantes_chamados.py C:470,538,588,905,955,1075,1156,1577,1604,1769
 R:177,592,1105,1605 T:1106,1657 V:205,1075,1576,1604,1768,1780
backfill_decisoes_pergunta.py C:101,325 R:56,60 T:61,326,504 V:56,60,102
bug_par_103.py C:66 V:68
curar_perguntas_em_limbo.py R:46,69 T:32,46
dedup_perguntas_fato.py T:75
encerrar_perguntas_com_fato.py C/V:32
migrar_data_evento_dia_do_turno.py C:147
reabrir_orfas_enterradas.py C:67,88,91,129,142,160 V:68
validar_classe_a.py T:80
```

Templates, **NOVO no censo global F5**:

| Arquivo | Referências |
|---|---|
| `app/templates/chamados/partials/modal_fio.html` | R `:185`, `:217`, `:220`; T `:219`; V `:228`. |
| `app/templates/chamados/partials/_card_atendimento.html` | R `:106`, `:143`, `:391`; T `:106`, `:114`, `:141`, `:143`, `:148`, `:154`, `:182`, `:183`, `:391`. |
| `app/templates/chamados/partials/_pergunta_card.html` | R `:24`, `:29`, `:32`; V `:20`, `:23`, `:42`. |
| `app/templates/chamados/partials/_lote_preview.html` | T `:24`. |

O AST não encontrou leitura literal R/T/V/C nos outros arquivos F5 `tarefa_do_dia.py`, `cobranca.py`, `furo_sem_canal.py`, `competencia_trancada.py`; isso não elimina dependência indireta: tarefa lê partição em `app/chamados/services/tarefa_do_dia.py:530`, cobrança lê consulta/espera em `app/chamados/services/cobranca.py:48`, `:241`, furo lê carga em `app/chamados/services/furo_sem_canal.py:94`.

## 17/11 01:10 · sessao 4d642465cda1 · - · - · origem=Ronald (tmux)

A BASE atual usa **L-130**, `pergunta_respondida(p)`, e a face SQL projetiva; a antiga API de atos não está disponível como premissa da F8. (`app/chamados/juizes.py:1544`, `:1552`, `:1575`, `:1580`; `app/docs/LEIS.md:151`.)

- `atos_pergunta.py` não foi localizado; `_leitura_conservada`, `_leitura_da_pergunta`, `LeituraDosAtos` e `LeiturasDaVida` não foram localizados em produção. A ausência global não tem linha positiva própria; **[nao verificado]** quanto a interface escondida/dinâmica. As alternativas concretas são `pergunta_respondida` e `q_respondida_com_texto`. (`app/chamados/juizes.py:1544`, `:1575`.)
- `carga_pergunta_viva.py:69` é `carga_da_vida`, com `CargaDaVida` interna e `_slot_esperado` em `:119`; não a classe histórica do brief. (`app/chamados/services/carga_pergunta_viva.py:69`, `:83`, `:119`.)
- A V legada continua encerrando vida antes de R; não existe aqui fold que zere a V pelo instante posterior da R. (`app/chamados/juizes.py:1389`, `:1405`.)
- A pergunta F8 L1 está decidida pelo **item 8 do corte em `app/docs/CORTES.md:488`**, cujo offset mudou de `:487` dos anexos. “SIM, segue validada; ato sintético do backfill não revoga decisão humana”. A medição contra a árvore pousada continua requerida ali; números atuais **[nao verificado]**. (`app/docs/CORTES.md:488`.)
- Não há briefs/matrizes locais encontrados para F3/F5/F6/F9; os caminhos externos são mencionados, por exemplo, em `app/docs/RELATO.md:830`, mas isso não prova seus arquivos atuais. Cruzamento pelo brief dessas quatro: **[nao verificado]**.

O censo AST global de produção foi executado em memória, com atributos `Load/Store`, keywords ORM/lookup e strings exatas, mais grep de templates/bin. Os resultados confirmam os offsets do grande censo do `f4.r` dado, salvo que a relação de escritores de `recusa_motivo` do brief F8 é incompleta. Completude para nomes construídos dinamicamente **[nao verificado]**.

**Escritores físicos, portas e simulações**

| Campo | Escritores conferidos; NOVO = ausente do brief F8 |
|---|---|
| R/T | **NOVO** resposta humana `app/chamados/services/disputa_emissao.py:1481`, `:1482`, salva `:1522`, RESPOSTA `:1525`; automática `:1674`, `:1675`, salva `:1677`, RESPOSTA `:1678`; trilha apenas I `:1696`, salva `:1698`, RESPOSTA `:1699`. |
| R/T/V/autor | **NOVO** ressuscitação por lote `app/chamados/models.py:1629`, `:1630`; reabertura `app/chamados/services/disputa_emissao.py:1609`→`:1648`; nascimento genérico `app/chamados/models.py:3124`, `:3127`, conservação `:3129`. |
| R/V/autor | **NOVO** `carimbar(resposta=)` em `app/chamados/models.py:3242`, `:3245`, `:3246`, salva `:3262`; não registra RESPOSTA/VALIDACAO nesse corpo. |
| V/autor | `app/chamados/services/validacao.py:168`, `:169`, salva `:171`, VALIDACAO `:173`; devolução `:273`, `:274`, salva `:276`, DEVOLUCAO `:279`; **NOVO BUG-PAR** `:349`, `:350`, salva `:351`, DEVOLUCAO `:354`; nova resposta limpa V/autor em `app/chamados/services/disputa_emissao.py:1494`, `:1495`. |
| C/autor/via | Atribuição em memória `app/chamados/models.py:3209`, `:3210`, `:3211`; persistência nas portas `:3262`, `:3300`, `:3322`; **NOVO** lote físico `:3391`, `:3393`; via somente `:3402`, `:3403`; via relavrada em `app/chamados/services/relavra_via.py:125`, `:127`. |
| `recusa_motivo` | **NOVO contra a alegação “dois escritores”**: escape da resposta humana `app/chamados/services/disputa_emissao.py:1511`, salva `:1522`; pendência técnica `app/chamados/services/materializacao.py:1332`, salva `:1333`; `_carimbar_recusa` `:1665`, salva `:1666`; ramo de colisão delega `:1662`, `:1663`→`app/chamados/models.py:3298`, salva `:3300`; recusa administrativa `app/chamados/services/validacao.py:827`, salva `:833`, reabertura/RECUSA `:836`. Nascimento genérico também recebe `**campos` em `app/chamados/models.py:3124`. |
| Apenas memória | Simulação de ressuscitação `app/chamados/models.py:1554`, `:1555`, `:1556`; restauração de carimbo `:3273`, `:3274`, `:3275`. |
| Homônimo excluído | `app/chamados/services/regularizacao_ext.py:199`, `:200` pertence a `RegularizacaoExterna`, definido em `app/chamados/models.py:4152`; não é PerguntaDisputa. |

**Leitores/cargas/filtros globais** — NOVO é todo sítio externo aos oito arquivos F8 e aos escritores/chamadores expressamente listados no brief; uma ocorrência como string/keyword ainda precisa ser distinguida de julgamento. As portas escritoras acima também leem os mesmos campos.

Legenda: `T=resposta_colab`, `I=respondida_em`, `V=validada_em`, `VP=validada_por/_id`, `C=materializada_em`, `CP=materializada_por/_id`, `X=via_resolucao`, `M=recusa_motivo`. Caminhos dos serviços abreviados abaixo estão em `app/chamados/services/`.

```text
NOVO app/api/views.py:
 T1163,2561; I1165,2563,2577,2620,2637.
NOVO app/api/views_mensageria.py:
 T567; I569,570.
NOVO app/chamados/canal.py:
 T65.
app/chamados/juizes.py:
 C292,683,711,858,1390,1502;
 I352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753;
 T364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052;
 V856,1389; X683,695,857,1390.
app/chamados/models.py:
 C1531,1923,2006,3153,3156,3166,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485;
 CP3236,3238,3293,3296,3320,3365,3393;
 I1614,1924,2949,3155,3158,3237,3241,3243,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100;
 T1533,1614,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105;
 V310,344,1614,1923,3151,3237,3244,3247,3331,3496,4091,4102,4103,4104;
 VP1614,1629,3237,3247,4105;
 X1535,2001,2010,2011,2136,2141,2829,2860,3165,3236,3238,3296,3311,3320,3331,3365,3392,3393,3400,3403,3411,3423,3440,3447,3483,3485,3652,3658;
 M3299.
NOVO app/chamados/reconciliador.py:
 C367,630,1050,1145,1155,1505; CP1146,1156;
 T365,1083; V631,1050; X1144,1155.
NOVO acoes_disputa.py:
 M83; I97,132; T355; V347.
NOVO becos.py:
 I30.
NOVO competencia_trancada.py:
 X276,282.
NOVO declaracao_texto.py:
 I417,419,687; T417; V227.
disputa_emissao.py:
 C228,776,1084,1104,1340,1637,1930,2062,2073,2090;
 I1483,1609,1635,1677,1692,1698,1703,1735,1843,1861,2074,2091;
 T1483,1609,1635,1663,1677,1692,1700,1861,2079;
 V1484,1485,1496,1537,1538,1609,1636,2062;
 VP1484,1496,1537,1538,1609,1636;
 X337,409,428,446,1637,2072,2073,2087,2090; M1513.
NOVO explicador.py:
 M113; T55,102,103; X118.
NOVO fabrica_da_celula.py:
 C171; X149.
NOVO fio.py:
 M564,1407; I187,706,1613; T377,1279,1291,1613;
 X1147,1214,1290,1397,1480,1921,1957,1979,2024.
NOVO foto_e4b.py:
 C/I/V180.
NOVO historico.py:
 C103; I99; T109; V101.
materializacao.py:
 C148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127;
 CP1154;
 M1333,1652,1655,1663,1666;
 I1151,2064;
 T546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619;
 V199,201,1152,2065,2082; VP1152;
 X261,292,862,868,1108,1150,1652.
NOVO painel.py:
 C/I67; V67,270,271,272,273; VP262.
NOVO passivo_validado.py:
 C64,534,535,536; CP64,535; M/I/V/VP66; X64,140,141,142,530,535.
NOVO pergunta_no_app.py:
 I81.
NOVO raiox.py:
 C135,182; I/T138,147.
NOVO relavra_via.py:
 C181,244,252; X118,121,123,127,134,136,155,181,245,252.
NOVO tarefa_do_dia.py:
 X256,257.
validacao.py:
 C262,264,341,363,556,681,701,717; CP342,363;
 M181,262,266,282,299,301,302,344,364,825,833,847;
 I594,813,824,848,1167;
 T175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109;
 V171,173,181,262,269,276,300,302,343,351,364,489,553,597,701,810,975,996;
 VP171,181,262,276,300,302,343,351,364,489;
 X341,363.
NOVO verbos.py:
 I335,336,342,345; V264,270; VP264,265,266,268.
NOVO vigia_carimbo.py:
 V98.
NOVO vigia_fio.py:
 I636,746; T749.
NOVO app/chamados/templatetags/chamados_tags.py:
 T139,166.
NOVO app/chamados/veredito_lavrado.py:
 C203,204; I207,208.
NOVO app/chamados/views.py:
 C1257; I1280,1416.
NOVO app/chamados/views_cobrar.py:
 I169,176; T291.
app/colaboradores/services/calendario.py: T504.
app/colaboradores/services/ficha.py: T171.
app/folha/previa.py: T142.
app/ponto/services/cartorio.py: T1641; X1650.
app/ponto/services/dia_do_colab.py: T70.
app/ponto/services/flip_auto.py: M132; V130; X129.
app/ponto/services/procedencia_batida.py: C/V74; CP/VP71,72.
app/ponto/services/veredito_celula.py: C196; T197.
```

**Comandos e scripts**, todos ampliação **NOVO** do inventário nominal do brief F8; prefixo dos dez primeiros `app/chamados/management/commands/`:

```text
auditar_disputas.py: C71,150.
auditar_invariantes_chamados.py:
 C470,538,588,905,955,1075,1156,1577,1604,1769;
 M536,1075,1112; I177,592,1105,1605; T1106,1657;
 V205,1075,1576,1604,1768,1780; VP1076; X202,206,433,542,955,1712.
backfill_decisoes_pergunta.py:
 C101,325; CP326; I56,60; T61,326,504; V56,60,102; VP60;
 X101,314,330,335,336,398,504.
bug_par_103.py: C/X66; CP67; M69; V/VP68.
curar_perguntas_em_limbo.py: I46,69; T32,46.
dedup_perguntas_fato.py: T75.
encerrar_perguntas_com_fato.py: C/V32; X33,42,49,66,75,76.
migrar_data_evento_dia_do_turno.py: C147; X70,71,72.
reabrir_orfas_enterradas.py:
 C67,88,91,129,142,160; CP130,143,160; V68; X67,129,142,160.
relavrar_via_i6.py: X58,59,62,66,68,88.
validar_classe_a.py: T80.
app/ponto/management/commands/reconciliar_fantasmas.py: C213.
app/ponto/management/commands/reconciliar_grade.py: I461.
bin/censo_folha.py: I28,29; V29.
bin/sombra_regras_pessoais.py: M/T85.
```

**Templates**, todos **NOVO** no censo nominal F8; abaixo há também ocorrências em atributos HTML e comentários, que não devem ser confundidas automaticamente com juiz:

```text
app/templates/chamados/partials/_card_atendimento.html:
 I95,106,143,391; T106,114,134,141,143,148,154,182,183,391;
 M188,190,192; X188,192.
app/templates/chamados/partials/_lote_preview.html: T24.
app/templates/chamados/partials/_pergunta_card.html:
 V20,23,42; I24,29,32; M122.
app/templates/chamados/partials/modal_fio.html:
 M61,226,322; I185,217,220; T219; V228.
```

Os chamadores que deslocaram contra os anexos incluem `contexto_calendario` em `app/colaboradores/views.py:1990`, `app/colaboradores/services/drawer.py:123`, `app/relatorios/furos.py:164`; `fechar_o_fio_do_dia` em `app/chamados/services/validacao.py:440`. Os dois primeiros já passam `agora`. (`app/colaboradores/views.py:1990`; `app/colaboradores/services/drawer.py:123`.)

## 17/11 01:10 · sessao 7b5eb31efcc4 · - · - · origem=Ronald (tmux)

Leitura estática concluída; sondas e testes não executados. [nao verificado quanto à execução] [nao verificado]

**Contratos atuais e RED:**

| Sítio | Código vigente e consequência |
|---|---|
| `placar_equipe()` | Retorna lista `{'nome': username, 'hoje': n, 'd7': n}`; soma chamados resolvidos e perguntas atualmente validadas. Validação exige staff, autor não nulo e username diferente de `sistema`. Continua lendo projeção; T1/T2 mordem por leitura. (`app/chamados/services/verbos.py:242`; `:253`; `:263`; `:272`) |
| Painel | `_valid_base` exclui somente autor nulo/sistema, sem staff; janelas pela projeção. T7 morde por leitura. (`app/chamados/services/painel.py:261`; `:268`) |
| `placar_aceite(empresa_id=None)` | Retorna `enviados`, `abertos`, `respondidos`, cada qual com `hoje/d7`; resposta após última cobrança, colaboradores distintos; não consulta autor da resposta. O requisito por ato ainda não está implementado: T8 morde contra o anexo, mas aceitar `ato_por=None` exige classificar a pergunta antiga pela definição escrita, não pelo número histórico. (`app/chamados/services/verbos.py:277`; `:325`; `:337`; `:348`) |
| Marcador | Duas queries: agregados CH e PD, sem DP nem Batida. T3/T3b mordem; T4 morde quando a Batida muda sem atualizar os campos agregados. (`app/chamados/services/painel.py:64`; `:66`) |
| ETag | Hash de marcador + filtros + quantum + extra; quantum 120 s. (`app/chamados/services/painel.py:47`; `:79`; `:91`) |
| Marcos | Retorna `{colaborador_id: datetime}`, agrega Batida, CelulaDia, PD M/R; respeita `ate`. Já percebe Batida nova, portanto T4 não morde neste leitor. DP não entra: T3 morde contra o requisito anexado. (`app/chamados/veredito_lavrado.py:151`; `:163`; `:177`; `:191`; `:202`) |

**NOVO: compatibilidade de D19.** O brief não pode ser traduzido literalmente em “retornar Max(pk)/Count” nesse dicionário: `esta_stale` compara o marco a `julgado_em`, e `raiox` faz a comparação diretamente. A mudança precisa conservar esse contrato ou alcançar esses consumidores; não proponho construção. (`app/chamados/veredito_lavrado.py:230`; `app/chamados/services/raiox.py:45`)

**Lei:** D04 está registrado literalmente como atos humanos, inclusive duas validações com devolução no meio. (`app/docs/BACKLOG.md:2091`) Literal D06 em LEIS/CORTES/BACKLOG: [nao verificado]; o requisito é fornecido pelo brief/matriz.

**Provas existentes lidas:**

| Função | Prova |
|---|---|
| Equipe | `app/chamados/tests/test_placar_equipe_staff.py::test_colab_nao_staff_fora_staff_dentro`, `:28`, cobre chamados, pois fixture cria somente CH em `:21`; `::test_marca_no_codigo`, `:33`, conta etiqueta textual, não prova o predicado de validação. |
| Aceite | `app/chamados/tests/test_placar_aceite.py::test_conta_3_camadas`, `:43`; `::test_filtro_empresa`, `:51`; `::test_resposta_antes_da_cobranca_nao_conta`, `:59`. A fixture cria PD com par preenchido em `:35-41`; não afirmar “sem DP” sem observar o nascimento atual, que conserva projeção em `app/chamados/models.py:3129`. |
| Marcador | `app/chamados/tests/test_selo_cadeia_placar.py::test_304_nao_paga_o_placar_inteiro`, `:68`, exige duas queries em `:72`; `::test_escrita_no_balde_invalida_o_etag`, `:91`, cria CH, não DP. |
| **NOVO: horizonte dos marcos** | `app/escala/tests/test_signal_ancora.py::test_RED_marcos_cortam_cada_insumo`, `:218`; `::test_MORDE_marcos_sem_corte`, `:222`; `::test_MORDE_celula_sem_corte_por_campo`, `:231`. O teste também exige os filtros projetivos atuais de PD em `:202-206`: será afetado pela troca, não deve sumir do cruzamento. |
| Payload lavrado | **Caminho corrigido:** `app/core/tests/test_selo_placar_lavrado.py::test_MORDE_lavrado_bate_com_recomputado`, `:178`; compara `equipe_janelas` em `:187-189`; não demonstra D04 pois fixture específica de DP: [nao verificado]. |

**Censo de chamadores produtivos das funções auditadas:**

- `marcos_do_lote`: `app/chamados/services/raiox.py:38`; alias `_mdl_s2` em **`app/chamados/services/fio.py:281-282`**; **`app/chamados/services/tarefa_do_dia.py:434`**; `app/chamados/management/commands/auditar_invariantes_chamados.py:1163`.
- `marcador_do_placar`: `app/chamados/services/painel.py:91`.
- `etag_do_placar`: `app/chamados/views.py:820`.
- `placar_equipe`: `app/chamados/views.py:1486`.
- `placar_aceite`: `app/chamados/services/painel.py:298`; `app/chamados/views.py:1488`; **`app/folha/views_retrato.py:515`**.
- `montar_contexto_painel`: `app/chamados/views.py:727,852`; `app/core/management/commands/lavrar_badge_navbar.py:39`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:72`.
- **NOVO:** modo resumo transporta `validacoes_janelas/equipe_janelas` de `base` em `app/chamados/services/painel.py:530-535`.
- Persistência indireta do número: `PlacarNavbar.payload_placar` em **`app/core/management/commands/lavrar_badge_navbar.py:92`**, além da chamada em `:39`.

**Premissas adicionais a julgar:**

- “Nenhum leitor F3 sem janela” **CAIU como afirmação universal**: marcador agrega PD inteira; marcos com `ate=None` não limita o acervo. Isso não transforma o marcador em contador D04, mas invalida a justificativa genérica do brief. (`app/chamados/services/painel.py:66`; `app/chamados/veredito_lavrado.py:163`)
- Furo de `carimbar('validar')` continua: atribui R/V e salva sem DP nesse corpo; absorção registra ABSORCAO, não VALIDACAO. Quantidade atual: [nao verificado]. (`app/chamados/models.py:3242`; `:3245`; `:3262`; `app/chamados/services/materializacao.py:1043`; `:1045`)
- O chamador antigo de chamado encerrado agora usa `resposta='responder'`, portanto não cabe transportar a lista histórica como descrição de todos os escritores atuais de V. (`app/chamados/services/disputa_emissao.py:2082`)
- “Devolvida no mesmo request” não pode ser comprovado só pela proximidade entre `ato_em`; DP possui os campos em `app/chamados/models.py:4038-4051`, sem identificador de request. A porta atual registra DEVOLUCAO com motivo em `app/chamados/services/validacao.py:279-282`; ocorrência/mesmo request em produção: [nao verificado].

**Sondas necessárias — implementação sugerida ao consolidar, todas em transação READ ONLY:**

1. Chamar funções reais `placar_equipe`, `montar_contexto_painel(...,so_placar=True)`, `placar_aceite`, `marcador_do_placar`, `marcos_do_lote(ids,ate=agora)`, `D.linhas_da_projecao_sem_linha`. Extrair os números de saída sem imprimir usernames, pois podem ser CPF. Equipe por autor deve usar `u<pk>`; colaborador somente `colNNN`. (`verbos.py:242,277`; `painel.py:50,198`; `veredito_lavrado.py:151`; `models.py:4081`)
2. Para as figuras históricas 323/380/57/1444/391/1121, medir separadamente: V atual staff; V atual não staff; linhas DP VALIDACAO staff; perguntas distintas; distribuição de linhas por pergunta; maior repetição; autores por PK; adjacência VALIDACAO→DEVOLUCAO e intervalo em milissegundos, explicitando que isso não prova request. Não antecipar nenhum valor. (`verbos.py:263`; `painel.py:261`; `models.py:4038-4051`)
3. Para 2601/2189/zero em 7d: chamar `linhas_da_projecao_sem_linha`, filtrar linhas VALIDACAO, e separar desde a primeira DP, staff e janela; distribuir via atual sem atribuí-la automaticamente à via histórica do ato. (`models.py:4094`; `:4102`)
4. Para absorcao/devolucao/reabertura/recusa/silencio históricos: agrupar DP por `ato` na janela; não dizer que todos seriam invisíveis ao marcador se houver alteração simultânea de CH/PD. O teste isolado é a prova da invisibilidade do evento sem projeção. (`painel.py:64-67`; `models.py:4024-4025`) [nao verificado]
5. Para 49/860 RESPOSTA com autor nulo: total DP RESPOSTA e nulos; cruzar, pelo colaborador da pergunta, com última cobrança real e contar colaboradores distintos após a cobrança. Autor não participa da definição atual. (`verbos.py:328-346`; `models.py:4041`)

Todos os caminhos abreviados `verbos.py`, `painel.py`, `models.py`, `veredito_lavrado.py` acima correspondem aos caminhos completos já citados neste relatório.

## 17/11 01:09 · sessao 68705488624d · - · - · origem=Ronald (tmux)

Leitura concluída, sem alterações nem testes executados. Todas as referências abaixo são da BASE. [nao verificado]

- **F1 pressuposta CAIU:** `pergunta_respondida` exige instante e texto da projeção; B2 está encarregada das classes B/C/D. O código não demonstra o antigo `LeituraDosAtos`. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`; `app/chamados/juizes.py:1552`)
- **BUG-PAR já está incorporada:** estados próprios K6, composição pergunta/irmã, validação do par e devolução estão implementados. A premissa histórica “esperar pouso BUG-PAR” caiu como descrição da BASE. (`app/chamados/services/validacao.py:57`; `app/chamados/services/validacao.py:224`; `app/chamados/services/validacao.py:308`; `app/chamados/services/validacao.py:526`)
- **Offsets das leis mudaram:** L-130 em `app/docs/LEIS.md:151`; LU-08 em `app/docs/CORTES.md:466`; CORTES-F2-F9-1611 em `app/docs/CORTES.md:488`; D04 “duas validações com devolução = duas” em `app/docs/BACKLOG.md:2091`.

Censo dos dois arquivos por AST em memória:

| Função | Leituras/escritas |
|---|---|
| `_carimbar_validacao` | V/autor escritos; VALIDACAO e snapshot T. (`app/chamados/services/validacao.py:168`; `:169`; `:173`; `:175`) |
| **NOVO** `_foto_da_validacao` | V, autor, motivo, fechamento da disputa. (`app/chamados/services/validacao.py:181`; `:182`) |
| `devolver_validacao_recusada` | C, motivo, V, T; limpa V/autor e registra DEVOLUCAO; pode reabrir disputa. (`app/chamados/services/validacao.py:264`; `:266`; `:269`; `:273`; `:279`; `:291`) |
| **NOVO** `devolver_paridade_bug_par_103` | C, V/autor, motivo, T; guarda K6; descarimba; limpa V/autor; registra DEVOLUCAO. (`app/chamados/services/validacao.py:338`; `:341`; `:343`; `:346`; `:349`; `:354`; `:356`) |
| `_e_aceite_de_folga` | T projetado. (`app/chamados/services/validacao.py:380`) |
| **NOVO** `_validar_o_par` | V da irmã decide nova VALIDACAO; count de duas C confirma gravação inteira. (`app/chamados/services/validacao.py:553`; `:554`; `:556`) |
| `validar_pergunta` | R, V e C projetados. (`app/chamados/services/validacao.py:594`; `:597`; `:681`; `:717`) |
| `recusar_pergunta` | V, R, T e motivo; escreve motivo e chama reabertura com RECUSA. (`app/chamados/services/validacao.py:810`; `:813`; `:823`; `:824`; `:825`; `:827`; `:836`) |
| `conferir_data_do_fato` | D próprio; escreve data/autoria. (`app/chamados/services/validacao.py:905`; `:907`; `:908`) |
| `validar_lote_classe_a` | V em duas decisões. (`app/chamados/services/validacao.py:975`; `:996`) |
| `classe_do_lote` | T em todos os ramos. (`app/chamados/services/validacao.py:1094`; `:1096`; `:1098`; `:1103`; `:1104`; `:1105`; `:1106`; `:1109`) |
| `fila_validar_classificada` | R para idade. (`app/chamados/services/validacao.py:1167`) |
| `acao_reabrir_disputa` | Conta R nula sobre **todas as perguntas da disputa**, antes de obter corrigíveis: caiu a premissa da matriz de que a contagem era sobre corrigíveis. (`app/chamados/services/acoes_disputa.py:97`; `:99`) |
| `acao_regularizar_disputa` | R nula nas perguntas vivas bloqueia fechamento. (`app/chamados/services/acoes_disputa.py:132`) |
| `acao_corrigir_resposta` | V, fechamento da disputa, T anterior. (`app/chamados/services/acoes_disputa.py:347`; `:349`; `:355`) |

Homônimos fora: `recusa_motivo` é POST em `acoes_disputa.py:83`; `resolucao` é POST em `acoes_disputa.py:125`. Fechamento em `:257`/`:349` pertence à disputa. (`app/chamados/services/acoes_disputa.py:83`; `:125`; `:257`; `:349`)

Chamadores ampliados:

| Função | Chamadores |
|---|---|
| `validar_pergunta` | Ações `:19`, `:381`; **NOVO** foto recebida `:51`, declaração `:696`, comando `:100`. (`app/chamados/services/acoes_disputa.py:19`; `:381`; `app/api/foto_recebida.py:51`; `app/chamados/services/declaracao_texto.py:696`; `app/chamados/management/commands/validar_classe_a.py:100`) |
| `validar_lote_classe_a` | `views_cobrar.py:261,381`; `core/views_ia.py:169`. |
| `fila_validar_classificada` | `views_cobrar.py:288,349,361`. |
| `classe_do_lote` | **NOVO** `api/views_mensageria.py:545`; `validacao.py:978,1165`. |
| `_e_aceite_de_folga` | `vigia_fio.py:259`, importado em `:247`; os sítios históricos 297/311 não são os atuais. |
| `conferir_data_do_fato` | `chamados/views.py:1312`. |
| Ações | Reabrir: `fio.py:690`; regularizar: `views.py:475`; corrigir: `views.py:536`, `fio.py:685`. |
| Devolução BUG-PAR | `management/commands/bug_par_103.py:106`. |

Provas existentes que ampliam os antigos NENHUM:

| Degrau | Prova lida |
|---|---|
| D5 fora do fato | `app/chamados/tests/test_e3_porta_A_reabrir.py::test_RED_recusa_de_linha_fora_do_fato_e_falante` (`:66`); casca `::test_RED_casca_traduz_fora_do_fato_como_info` (`:85`) |
| D7 lote | `app/chamados/tests/test_validar_lote.py::test_07_ja_validada_nao_recarimba` (`:131`), parcial: guarda |
| D8 classe | `app/chamados/tests/test_fila_validar_lote.py::test_MORDE_as_quatro_classes` (`:42`); `::test_MORDE_confirma_sem_marco_nao_e_lote` (`:54`); `::test_MORDE_batida_perto_tira_do_lote` (`:67`) |
| D10 fila | Mesmo arquivo `::test_MORDE_o_lote_so_valida_a_classe_do_lote` (`:76`); **não prova idade** |
| D17 S116 | `app/chamados/tests/test_guard_s116_detalhe_local.py::test_pergunta_sem_resposta_BLOQUEIA_fechamento` (`:52`) |
| D18 correção | `app/chamados/tests/test_corrigir_resposta.py::test_MORDE_pergunta_ja_validada_nao_se_corrige` (`:112`); `::test_MORDE_a_voz_do_colaborador_vai_para_a_trilha` (`:80`) |
| D21 devolução | `app/chamados/tests/test_o261_recusa_fala.py::test_acervo_devolve_pela_MESMA_funcao_e_reabre_a_disputa` (`:164`); `::test_MORDE_acervo_nao_devolve_o_que_a_porta_encerrou` (`:190`) |

Estados centrais e provas:

| Estado | Operação/persistido/retorno | Prova |
|---|---|---|
| R ausente | Validar não escreve; `sem_resposta`. (`validacao.py:594`) | `test_service_validar_puro.py::test_05_sem_resposta_nao_valida` (`:95`) |
| V presente | Validar não escreve; `ja_validada`. (`validacao.py:597`) | Mesmo `::test_06_ja_validada_e_idempotente` (`:105`) |
| V presente | Recusar não escreve; `ja_validada`. (`validacao.py:810`) | `test_porta_validacao_pergunta.py::test_MORDE_pergunta_JA_VALIDADA_nao_e_recusavel` (`:297`) |
| R ausente | Recusar não escreve; `ja_pendente`. (`validacao.py:813`) | Mesmo `::test_MORDE_recusar_2x_e_IDEMPOTENTE` (`:281`) |
| Fora do fato | Recusar não escreve; `fora_do_fato`. (`validacao.py:817`) | `test_e3_porta_A_reabrir.py::test_RED_recusa_de_linha_fora_do_fato_e_falante` (`:66`) |
| Resposta recusável | Motivo + RECUSA + zeragem pela reabertura; `recusada`. (`validacao.py:827`; `:833`; `:836`; `:862`) | `test_porta_validacao_pergunta.py::test_MORDE_recusar_SEM_request_carimba_reabre_e_deixa_trilha` (`:247`) |
| Hora, C ausente, recusa escrita | VALIDACAO seguida DEVOLUCAO; V/autor limpos; `nao_gravou`. (`validacao.py:655`; `:693`; `:727`) | `test_o261_recusa_fala.py::test_acervo_devolve_pela_MESMA_funcao_e_reabre_a_disputa` (`:164`), e BUG-PAR L2 abaixo |
| F6 aprovada sem K6 | Conserva toque; motivo de competência; VALIDACAO+DEVOLUCAO. (`validacao.py:536`; `:562`; `:683`; `:693`) | `test_bug_par_volta.py::test_matriz_linha2_fechada_nao_entregue_conserva_o_toque_e_o_motivo` (`:237`) |
| K6 entregue | Recusa antes de carimbo; estado próprio. (`validacao.py:652`; `:654`) | Mesmo `::test_matriz_linha1_entregue_fora163_recusa_estado_proprio_esconde_validar` (`:222`) |
| Sem dia do fato | Estado próprio, sem escrita. (`validacao.py:232`; `:233`) | Mesmo `::test_matriz_linha5_fora163_sem_dia_do_fato_estado_proprio` (`:304`) |
| Declarado divergente U163 | Antes de implausibilidade/escrita; espera. (`validacao.py:609`; `:613`) | Mesmo `::test_matriz_linha3_163_declarado_x_gravado_mostra_a_espera` (`:275`) |
| Par aplicável | Valida irmã e grava ambas; `validada` com `par`. (`validacao.py:554`; `:555`; `:745`) | Mesmo `::test_matriz_linha4_nada_barra_aplicavel_validar_grava` (`:289`) |
| A20 já conferida | Não escreve; `ja_conferida`. (`validacao.py:905`) | `test_porta_validacao_pergunta.py::test_MORDE_conferir_2x_nao_recarimba` (`:340`) |

RED atual admissível pela lei: legado com instante preenchido e texto branco **não pode ser considerado respondido**; `validar_pergunta` e `recusar_pergunta` ainda só testam instante em seus ramos. Isso morde a guarda projetiva por leitura, sujeito aos demais pré-requisitos do caso; a prova do juiz está em `app/chamados/juizes.py:1552` e a lei em `app/docs/LEIS.md:151`, os leitores em `app/chamados/services/validacao.py:594` e `:813`. O antigo RED “DP RESPOSTA presente + projeção vazia deve validar” **não tem esse gabarito dedutível da F1 pousada**: seu oráculo D01 caiu. (`app/chamados/juizes.py:1544`; `:1552`)

## 17/11 01:09 · sessao 00e5b02f1deb · - · - · origem=Ronald (tmux)

U1/U2/U5 continuam na forma projetiva descrita pelos anexos, mas a F1 vigente é `pergunta_respondida(p)`, que exige instante e texto após trim; o comentário destina os leitores só-texto à B2. Não há fundamento nesses corpos para usar o antigo fold como oráculo da F8. (`app/chamados/juizes.py:1544`; `app/chamados/juizes.py:1551`; `app/chamados/juizes.py:1552`)

| Unidade | Corpo atual e persistência | Prova existente |
|---|---|---|
| U1 | `procedencia` carrega perguntas em lote com autores V/C, escolhe `validada_por or materializada_por` e `validada_em or materializada_em`; só retorna dicionário. (`app/ponto/services/procedencia_batida.py:70`; `app/ponto/services/procedencia_batida.py:72`; `app/ponto/services/procedencia_batida.py:74`; `app/ponto/services/procedencia_batida.py:97`) | `app/ponto/tests/test_procedencia_batida.py::test_RED_a_plantada_por_disputa_diz_QUANDO_e_POR_QUEM`, :61; `::test_UMA_query_para_as_perguntas_de_todas`, :117. O segundo usa cinco perguntas sem R/V carimbados; não prova custo de uma futura carga conservada. (`app/ponto/tests/test_procedencia_batida.py:123`; `app/ponto/tests/test_procedencia_batida.py:126`) |
| U2 | `fila_flip_recusas(dias)` lê relógio internamente, filtra via/V e extrai batida do motivo técnico; não escreve. (`app/ponto/services/flip_auto.py:126`; `app/ponto/services/flip_auto.py:129`; `app/ponto/services/flip_auto.py:132`; `app/ponto/services/flip_auto.py:140`) | `app/chamados/tests/test_a27_recusa_com_dono.py::test_recusa_de_paridade_vira_item_da_fila`, :73; fixture só-projeção :81–85; assert da tupla :87. Não cobre outra via, janela vencida ou dedup. |
| U5 | Fecha perguntas selecionadas por `q_vivas`, chama `fechar_por_admin` e registra SILENCIO usando o carimbo do mesmo ato e texto projetado. (`app/ponto/services/veredito_celula.py:181`; `app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:195`; `app/ponto/services/veredito_celula.py:197`) | `app/chamados/tests/test_e1_v5_portas_backfill.py::test_fechar_o_fio_do_dia_grava_o_silencio`, :85: verifica via, ato/carimbo e idempotência; não verifica `ato_texto`. (`app/chamados/tests/test_e1_v5_portas_backfill.py:90`; `app/chamados/tests/test_e1_v5_portas_backfill.py:92`; `app/chamados/tests/test_e1_v5_portas_backfill.py:94`) |

A pergunta antiga V legada + R sintética posterior já está **DECIDIDA PELA LEI CORTES-F2-F9-1611, item 8 | contestavel**: segue validada; medir contra a árvore pousada, esperando nenhuma batida validada mudar. A linha agora é **488**, não 487 dos anexos anteriores. (`app/docs/CORTES.md:488`)

O RED antigo “última RESPOSTA diferente da projeção → usar texto do ato” não está pronto pela lei atual: L-130 decide a condição de respondida, não instala aquele fold antigo. A escrita SILENCIO hoje fotografa o campo atual. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1552`; `app/ponto/services/veredito_celula.py:197`)

Censo de chamadores e consumidores:

| Função | Produção |
|---|---|
| `procedencia` | `app/colaboradores/services/calendario.py:650`; `app/ponto/services/espelho.py:756`. Os resultados viajam em `b.procedencia`. (`app/colaboradores/services/calendario.py:671`; `app/colaboradores/services/calendario.py:677`; `app/ponto/services/espelho.py:758`) |
| `frase_para_papel` | PDF usa o adaptador e recebe a procedência montada no espelho. (`app/relatorios/pdf_espelho.py:950`; `app/relatorios/pdf_espelho.py:952`) |
| `fila_flip_recusas` | Só `fila_flip`, em `app/ponto/services/flip_auto.py:148`. |
| `fila_flip` | `avaliar_fila`, em `app/ponto/services/flip_auto.py:268`. **Correção do brief:** :112 é `fila_flip.append` numa lista local de `classificar_pares`, não chamada de `fila_flip(dias)`. (`app/ponto/services/flip_auto.py:61`; `app/ponto/services/flip_auto.py:112`; `app/ponto/services/flip_auto.py:146`) |
| `avaliar_fila` | Command `app/ponto/management/commands/flip_automatico.py:32`; **NOVO** tela humana `app/ponto/views.py:3459`. |
| `fechar_o_fio_do_dia` | `app/ponto/services/veredito_celula.py:347`; `app/ponto/services/declarar_dia.py:206`; `app/chamados/services/validacao.py:440`. |

U2 alcança escrita de dinheiro indiretamente: cron roda `--apply`, command chama `flip_tipo`, que escreve o tipo pela porta `anotar`. Isso não autoriza executar a cadeia nesta leitura. (`app/config/crons.py:707`; `app/ponto/management/commands/flip_automatico.py:68`; `app/ponto/services/flip_batida.py:30`)

A BUG-PAR vigente afeta o universo de U2: antes de devolver, consulta a competência real; sem dia ou entregue, não devolve. Quando devolve, descarimba, zera V/autor, conserva resposta/motivo e registra DEVOLUCAO. A premissa antiga “sempre tira linhas de U2” precisa desse recorte. (`app/chamados/services/validacao.py:338`; `app/chamados/services/validacao.py:339`; `app/chamados/services/validacao.py:345`; `app/chamados/services/validacao.py:349`; `app/chamados/services/validacao.py:354`; `app/chamados/services/validacao.py:364`)

O motivo técnico continua distinto do motivo administrativo: `_carimbar_recusa` persiste projeção sem registrar DP no corpo; recusa administrativa chama a reabertura com ato RECUSA. Há ainda escritores que o brief não enumerou: escape do colaborador e pendência técnica de derivação. (`app/chamados/services/materializacao.py:1646`; `app/chamados/services/materializacao.py:1665`; `app/chamados/services/materializacao.py:1666`; `app/chamados/services/validacao.py:827`; `app/chamados/services/validacao.py:836`; `app/chamados/services/disputa_emissao.py:1511`; `app/chamados/services/materializacao.py:1332`)

Para a tabela de RED:

| Caso | Exigido | MORDE-HOJE |
|---|---|---|
| V legada com R sintética posterior | Conservar V e procedência atual | **não**: U1 já lê V projetada. (`app/docs/CORTES.md:488`; `app/ponto/services/procedencia_batida.py:72`; `app/ponto/services/procedencia_batida.py:74`) |
| U1 somente C, sem V | Conservar fallback autor/instante | **não**: os dois fallbacks existem; prova específica só-C: NENHUM no teste inspecionado. (`app/ponto/services/procedencia_batida.py:72`; `app/ponto/services/procedencia_batida.py:74`; `app/ponto/tests/test_procedencia_batida.py:61`; `app/ponto/tests/test_procedencia_batida.py:75`) |
| Recusa técnica sem ato RECUSA | Continuar entrando na fila pelo motivo técnico | **não**: já passa pelo leitor projetivo; mutante que troca pelo motivo do ato perde a fonte, mas não é falha atual. (`app/ponto/services/flip_auto.py:132`; `app/chamados/tests/test_a27_recusa_com_dono.py:81`; `app/chamados/tests/test_a27_recusa_com_dono.py:87`) |
| U2 relógio fornecido pelo chamador | Julgar a janela no mesmo instante da rodada | **sim contra P7 da matriz**, mas não atribuir a L-130: `avaliar_fila` lê um instante e U2 lê outro. (`app/ponto/services/flip_auto.py:126`; `app/ponto/services/flip_auto.py:263`; `app/ponto/services/flip_auto.py:268`) |
| U5 texto só no ato, projeção diferente | Oráculo antigo de P6 precisa ser adaptado ao contrato pousado | **não como RED fechado pela L-130**: ela não determina trocar o texto vigente pelo fold. (`app/docs/LEIS.md:151`; `app/ponto/services/veredito_celula.py:197`) |
| U5 carimbo e idempotência | SILENCIO com carimbo da própria conclusão; segunda chamada neutra | **não**: corpo e teste cobrem. (`app/chamados/models.py:3165`; `app/chamados/models.py:3174`; `app/ponto/services/veredito_celula.py:196`; `app/chamados/tests/test_e1_v5_portas_backfill.py:92`; `app/chamados/tests/test_e1_v5_portas_backfill.py:94`) |

Pins continuam calendário [273], cartório [1205,1358], flip [140]. (`app/core/tests/test_medida_turno_do_par.py:275`; `app/core/tests/test_medida_turno_do_par.py:319`; `app/core/tests/test_medida_turno_do_par.py:330`)

Sonda possível para os recortes U1/U2, sem reconstruir o fold ausente. Ela conta a forma física e a fonte efetiva; equivalência com futura F8 requer repetir na mesma fotografia de dados. A máscara inclui `recusa_motivo`, portanto fila raspada não certifica tamanho da fila em produção. (`app/chamados/models.py:4081`; `app/ponto/services/procedencia_batida.py:54`; `app/ponto/services/flip_auto.py:116`; `bin/sombra_regras_pessoais.py:85`)

```python
from collections import Counter
from django.db import connection, transaction
from django_tenants.utils import schema_context
from chamados.models import PerguntaDisputa as P, DecisaoPergunta as D
from chamados.juizes import pergunta_respondida
from ponto.models import Batida
from ponto.services.procedencia_batida import procedencia
from ponto.services.flip_auto import fila_flip_recusas

with schema_context('juliani'), transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")

    n = Counter()
    ps = list(P.objects.select_related(
        'validada_por', 'materializada_por', 'disputa'
    ).order_by('pk'))
    faltantes = D.linhas_da_projecao_sem_linha(ps)
    v_sem_linha = {
        x['pergunta_id'] for x in faltantes
        if x['ato'] == D.VALIDACAO
    }
    alvos = {
        p.pk: p for p in ps
        if p.pk in v_sem_linha
        and p.validada_em is not None
        and p.respondida_em is not None
        and p.respondida_em > p.validada_em
    }
    n['v_sem_linha_com_resposta_posterior'] = len(alvos)
    n['v_sem_linha_resposta_posterior_respondida_pelo_juiz'] = sum(
        pergunta_respondida(p) for p in alvos.values()
    )
    n['v_sem_linha_resposta_posterior_via_paridade'] = sum(
        p.via_resolucao == P.VIA_PARIDADE_RECUSADA
        for p in alvos.values()
    )

    bs = list(Batida.objects.filter(
        pergunta_origem_id__in=alvos,
    ).exclude(origem='app').order_by('pk'))
    proc = procedencia(bs)
    n['batidas_nao_app_do_recorte_v_legada'] = len(bs)
    n['batidas_recorte_com_procedencia'] = len(proc)

    for b in bs:
        p = alvos[b.pergunta_origem_id]
        d = proc.get(b.pk)
        if d and d['quando'] == p.validada_em:
            n['batidas_recorte_instante_validacao_conservado'] += 1
        print(
            f"ITEM procedencia col{b.colaborador_id:03d} "
            f"pergunta={p.pk} batida={b.pk}"
        )

    fila = fila_flip_recusas(30)
    n['fila_flip_recusas_30d_na_sombra'] = len(fila)
    n['fila_flip_recusas_30d_batidas_distintas'] = len({
        x[2] for x in fila
    })
    for cid, dia, bid, loc, atual, alvo in fila:
        print(
            f"ITEM fila_flip col{cid:03d} batida={bid} "
            f"dia={dia} tipo_atual={atual} tipo_alvo={alvo}"
        )

    for rotulo in (
        'v_sem_linha_com_resposta_posterior',
        'v_sem_linha_resposta_posterior_respondida_pelo_juiz',
        'v_sem_linha_resposta_posterior_via_paridade',
        'batidas_nao_app_do_recorte_v_legada',
        'batidas_recorte_com_procedencia',
        'batidas_recorte_instante_validacao_conservado',
        'fila_flip_recusas_30d_na_sombra',
        'fila_flip_recusas_30d_batidas_distintas',
    ):
        print(f"NUMERO {rotulo}: {n[rotulo]}")
```

Execução de testes, frota atual e resultados dessas sondas: [nao verificado]. [nao verificado]

## 17/11 01:09 · sessao 33770149ac2d · - · - · origem=Ronald (tmux)

Censo global feito com AST de atributos, keywords ORM e strings exatas, mais grep de templates/chamadas. Nenhum arquivo foi criado, alterado ou apagado; nenhuma aplicação/teste foi importada ou executada. Exaustividade para nomes construídos dinamicamente: [nao verificado]. [nao verificado]

Convenção abaixo: prefixo `app/`; `R=respondida_em`, `T=resposta_colab`, `V=validada_em`, `VP=validada_por[/_id]`, `M=materializada_em`, `MP=materializada_por[/_id]`. Os sítios incluem leituras, seleções, transporte e escritores; escritores foram classificados na mensagem anterior.

**Todos os candidatos produtivos dos campos encontrados:**

```text
api/views.py T:1163,2561; R:1165,2563,2577,2620,2637
api/views_mensageria.py T:567; R:569,570
chamados/canal.py T:65
chamados/juizes.py
 M:292,683,711,858,1390,1502
 R:352,856,1389,1423,1534,1541,1552,1572,1580,1744,1753
 T:364,857,1360,1389,1423,1534,1541,1552,1572,1580,1779,1926,2030,2035,2052
 V:856,1389
chamados/models.py
 M:1531,1923,2006,3153,3156,3166,3209,3236,3238,3292,3293,3296,3320,3331,3365,3393,3411,3423,3440,3483,3485
 MP:3210,3236,3238,3293,3296,3320,3365,3393
 R:1555,1614,1629,1924,2949,3155,3158,3237,3241,3242,3243,3273,3331,3370,3375,3376,3380,3382,3568,3584,3585,3665,4091,4098,4099,4100
 T:1533,1556,1614,1630,1925,2007,2139,2950,3240,3331,3370,3375,3379,3383,3568,3584,3586,3688,3693,3697,3701,3705,3708,3709,3710,4101,4105
 V:310,344,1554,1614,1629,1923,3151,3237,3244,3245,3247,3274,3331,3496,4091,4102,4103,4104
 VP:1614,1629,3237,3246,3247,3275,4105
chamados/reconciliador.py M:367,630,1050,1145,1155,1505; MP:1146,1156; T:365,1083; V:631,1050
chamados/services/acoes_disputa.py R:97,132; T:355; V:347
chamados/services/becos.py R:30
chamados/services/declaracao_texto.py R:417,419,687; T:417; V:227
chamados/services/disputa_emissao.py
 M:228,776,1084,1104,1340,1637,1930,2062,2073,2090
 R:1482,1483,1609,1635,1675,1677,1692,1696,1698,1703,1735,1843,1861,2074,2091
 T:1481,1483,1609,1635,1663,1674,1677,1692,1700,1861,2079
 V:1484,1485,1494,1496,1537,1538,1609,1636,2062
 VP:1484,1495,1496,1537,1538,1609,1636
chamados/services/explicador.py T:55,102,103
chamados/services/fabrica_da_celula.py M:171
chamados/services/fio.py R:187,706,1613; T:377,1279,1291,1613
chamados/services/foto_e4b.py M/R/V:180
chamados/services/historico.py R:99; V:101; M:103; T:109
chamados/services/materializacao.py
 M:148,261,262,289,326,862,871,1015,1107,1109,1114,1153,1652,1656,2066,2079,2127
 MP:1154
 R:1151,2064
 T:546,574,625,671,678,718,733,770,869,950,1047,1109,1150,1311,1352,1534,1558,1567,1619
 V:199,201,1152,2065,2082; VP:1152
chamados/services/painel.py M/R/V:67; VP:262; V:270,271,272,273
chamados/services/passivo_validado.py M:64,534,535,536; MP:64,535; R/V/VP:66
chamados/services/pergunta_no_app.py R:81
chamados/services/raiox.py M:135,182; R/T:138,147
chamados/services/relavra_via.py M:181,244,252
chamados/services/validacao.py
 M:262,264,341,363,556,681,701,717; MP:342,363
 R:594,813,824,848,1167
 T:175,263,281,356,380,823,843,848,1094,1096,1098,1103,1104,1105,1106,1109
 V:168,171,173,181,262,269,273,276,300,302,343,349,351,364,489,553,597,701,810,975,996
 VP:169,171,181,262,274,276,300,302,343,350,351,364,489
chamados/services/verbos.py R:335,336,342,345; V:264,270; VP:264,265,266,268
chamados/services/vigia_carimbo.py V:98
chamados/services/vigia_fio.py R:636,746; T:749
chamados/templatetags/chamados_tags.py T:139,166
chamados/veredito_lavrado.py M:203,204; R:207,208
chamados/views.py M:1257; R:1280,1416
chamados/views_cobrar.py R:169,176; T:291
colaboradores/services/calendario.py T:504
colaboradores/services/ficha.py T:171
folha/previa.py T:142
ponto/management/commands/reconciliar_fantasmas.py M:213
ponto/management/commands/reconciliar_grade.py R:461
ponto/services/cartorio.py T:1641
ponto/services/dia_do_colab.py T:70
ponto/services/flip_auto.py V:130
ponto/services/procedencia_batida.py MP/VP:71,72; M/V:74
ponto/services/veredito_celula.py M:196; T:197
```

**Comandos também leitores** — prefixo `app/chamados/management/commands/`:

```text
auditar_disputas.py M:71,150
auditar_invariantes_chamados.py
 M:470,538,588,905,955,1075,1156,1577,1604,1769
 R:177,592,1105,1605; T:1106,1657; V:205,1075,1576,1604,1768,1780; VP:1076
backfill_decisoes_pergunta.py M:101,325; MP:326; R:56,60; T:61,326,504; V:56,60,102; VP:60
bug_par_103.py M:66; MP:67; V/VP:68
curar_perguntas_em_limbo.py R:46,69; T:32,46
dedup_perguntas_fato.py T:75
encerrar_perguntas_com_fato.py M/V:32
migrar_data_evento_dia_do_turno.py M:147
reabrir_orfas_enterradas.py M:67,88,91,129,142,160; MP:130,143,160; V:68
validar_classe_a.py T:80
```

**NOVO fora `app/`:** `bin/censo_folha.py:28,29` lê R/V; `bin/sombra_regras_pessoais.py:85` lê T. Os homônimos `RegularizacaoExterna` devem ficar separados: `app/chamados/services/regularizacao_ext.py:199,200,201,220`; declaração em `app/chamados/models.py:4152,4153`.

**Templates dos campos** — prefixo `app/templates/chamados/partials/`:

| Template | Sítios |
|---|---|
| `_pergunta_card.html` | V:20,23,42; R:24,29,32; display:31; filtros:43 |
| `modal_fio.html` | R:185,217,220; T:219; V:228; filtro:274 |
| `_card_atendimento.html` | R:106,143,391; T:106,114,141,143,148,154,182,183,391 |
| `_lote_preview.html` | T:24 |

Todos fora dos sete arquivos e dois templates explicitamente enumerados pelo brief são **NOVO no censo**, sem alegar que nasceram agora.

**Grafo completo das funções diretamente tocadas:**

| Função | Consumidores produtivos |
|---|---|
| `api_chamados_v2` | `app/api/urls.py:73` |
| `api_disputa_perguntas` | `app/api/urls.py:92` |
| `api_disputa_responder` | `app/api/urls.py:93` |
| `_item_validar` | `app/api/views_mensageria.py:546` |
| `validar_inline` | `app/chamados/urls.py:33`; **NOVO** `app/templates/chamados/partials/_lista_chamados.html:82` |
| `proposta_lote` | `app/chamados/urls.py:38`; **NOVO** `app/templates/core/_copiloto.html:16` |
| `responder_pergunta_disputa` | `app/chamados/urls.py:17`; **NOVO** `app/templates/chamados/partials/_card_atendimento.html:78,169` |
| `reabrir_disputa_supervisao` | `app/chamados/urls.py:20`; **NOVO** `app/templates/chamados/partials/modal_fio.html:395`; **NOVO** `app/templates/chamados/detalhe_local.html:9` |
| `perguntas_mudas_com_resposta` | `app/chamados/views_cobrar.py:180,182` |
| `leitura_da_resposta` | `app/templates/chamados/partials/_pergunta_card.html:43`; `app/templates/chamados/partials/modal_fio.html:274` |

`core/juizes.py` também registra as portas de UI como strings, sem constituir chamada delas: `app/core/juizes.py:1060,1071,1076`.

**NOVO decisivo no próprio módulo U12:** `aplicacao_da_resposta` contém outro leitor de T em `app/chamados/templatetags/chamados_tags.py:139`; não basta migrar o curto-circuito de `leitura_da_resposta:166`. Consome testemunha e faz fallback `indisponivel` para hora com texto; `fio` possui fallback equivalente em `app/chamados/services/fio.py:377`.

**Provas adicionais ao inventário do brief, relacionadas por função/rota:** `app/chamados/tests/test_c2_endpoints_janela.py:61`; `test_e3_v4_orm.py:43,64`; `test_e3_leitores_fio_disputa.py:149`; `test_ui_fatia0.py:9`; `test_ui5_painel_colapsado.py:55`; `test_selo_painel_oferece_cobrar.py:210`; `test_b8_smoke_reabertura.py:74`; `test_smoke.py:163,170`. Asserções e suficiência dessas provas para o novo oráculo: [nao verificado]; são referências de consumo, não evidência de teste executado.

## 17/11 01:09 · sessao 32a97e3ce5a1 · - · - · origem=Ronald (tmux)

A F1 vigente define resposta pelo **par projetado** (`respondida_em` + texto após trim), e reserva migração de classes B/C/D à B2. Não fornece, nesse contrato, o redutor antigo pressuposto pela F5. (`app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1550`, `app/chamados/juizes.py:1552`.)

**Correção de referência:** o corte vigente F2–F9 está agora em **`app/docs/CORTES.md:488`**. O item 5 conserva resposta vazia **fora do app**, atribui a ação ao **admin** e proíbe painel dizendo “colab deve texto”. (`app/docs/CORTES.md:488`.)

Tabela de estados/provas inspecionadas; testes não executados: [nao verificado]. [nao verificado]

| estado | juiz | operação | persistido | retorno atual / exigido | prova |
|---|---|---|---|---|---|
| Sem instante, texto vazio, disputa aberta | Partição | Classificar app | Nada | Pendente | `app/chamados/tests/test_e3_degrau_dono_e_porta.py::test_consulta_reusa_predicado_e_encaminha_relogio`, definição `:261`, asserção `:271`; corpo `app/chamados/services/pergunta_no_app.py:81` |
| Instante + texto, disputa aberta | Partição | Classificar app | Nada | Histórico | Mesmo teste, `:272`; corpo `app/chamados/services/pergunta_no_app.py:81` |
| Disputa fechada, sem instante | Partição | Classificar app | Nada | Histórico | Mesmo teste, `:264`, `:272` | [nao verificado]
| Instante + texto vazio | Partição | Classificar app | Nada | Histórico, conforme corte | `app/chamados/tests/test_o261e_f5_alcance.py::test_b_respondida_com_resposta_VAZIA_sob_ancora_na_fila_e_SEM_PORTA_causa_b`, `:184`, asserção `:194`; corte `app/docs/CORTES.md:488` |
| Instante + texto vazio | Tarefa | Exigibilidade | Nada | **Hoje COLABORADOR/RESPONDER; corte exige ADMIN** | `app/chamados/services/tarefa_do_dia.py:159`, `:161`; corte `app/docs/CORTES.md:488`. Teste atual exige razão antiga e ausência de porta admin em `app/chamados/tests/test_o261e_f5_alcance.py:199`, `:201`; **NENHUM** para resultado do corte |
| Instante + texto vazio | Fio/cobrança | Deve texto / calados | Nada | Hoje `True`, conta calado | `app/chamados/tests/test_e3_item1b.py::test_RED_carimbo_sem_texto_conta_como_calado`, `:119`, `:124`; corpo `app/chamados/services/fio.py:187` |
| Instante + texto | Fio/cobrança | Deve texto / calados | Nada | Não calado | `app/chamados/tests/test_e3_item1b.py::test_MORDE_com_texto_nao_e_calado`, `:126`, `:130` |
| Instante + texto vazio | Card colaborador | Mostrar resposta enviada | Nada | Não mostra “Resposta enviada” | `app/chamados/tests/test_e3_item1b.py::test_RED_carimbo_sem_texto_nao_diz_resposta_enviada`, `:150`, `:155`, `:156` |
| Instante + texto | Card colaborador | Mostrar resposta enviada | Nada | Mostra | `app/chamados/tests/test_e3_item1b.py::test_MORDE_com_texto_diz_resposta_enviada`, `:158`, `:162`, `:163` |
| Última RESPOSTA com texto | Tarefa | Julgar decisão | Nada | `aguarda_validacao`, admin | `app/chamados/services/tarefa_do_dia.py:159`, `:162`; **NENHUM inspecionado para o ramo isolado** |
| RECUSA/REABERTURA | Tarefa | Julgar decisão | Nada | `devolvida_ao_colaborador` | `app/chamados/services/tarefa_do_dia.py:163`, `:164`; **NENHUM inspecionado para divergência física ato/projeção** |
| Reabertura pela porta real | Porta existente | Reabrir | Limpa texto/instante/V/autor | Pendente | `app/chamados/tests/test_reabrir_nao_destroi.py::test_MORDE_a_pergunta_volta_a_pendente_como_antes`, `:54`, `:58` |
| DEVOLUCAO | Tarefa | Julgar decisão | Nada | `materializacao_recusada`, admin | `app/chamados/services/tarefa_do_dia.py:165`, `:166`; **NENHUM inspecionado para divergência física** |
| VALIDACAO | Tarefa | Julgar decisão | Nada | Depende de lavra/efeito: indeterminada, materializar, rejulgar ou encerrada | `app/chamados/services/tarefa_do_dia.py:167`, `:170`, `:172`, `:174`, `:175`; **NENHUM isolado inspecionado** |
| SILENCIO | Tarefa | Julgar decisão | Nada | `silencio_decidido`, nenhuma ação | `app/chamados/services/tarefa_do_dia.py:176`, `:177`; controle ressuscitação `app/chamados/tests/test_f1nova_b1_juiz_respondida.py::test_RED_ressuscitar_com_SILENCIO_nao_devolve_ao_colaborador`, `:334` |
| ABSORCAO | Tarefa | Julgar decisão | Nada | Aprovada: nenhuma; rejeitada: responder; aguarda documento: entregar; demais: admin | `app/chamados/services/tarefa_do_dia.py:178`, `:181`, `:183`, `:185`, `:186`; **NENHUM para decidir partição app por esses atos** |
| V projetada presente | Declarador | `_resposta_validada` | Nada | `True`, independentemente da sequência | `app/chamados/services/declaracao_texto.py:227`; **NENHUM para antiga troca por fold** |
| V projetada presente | Vigia carimbo | Selecionar universo | Nada | Inclui hora/data_hora | `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/tests/test_o306_vigia_carimbo.py::test_validada_que_nunca_plantou_e_o_beco_do_fio_morto`, `:141`, `:145`; teste das partes `:148`, `:159`, `:160` |
| Nenhuma validada | Vigia carimbo | Censo | Nada | `universo=0`, `cego=True` | `app/chamados/tests/test_o306_vigia_carimbo.py::test_CEGO_quando_nao_ha_pergunta_validada_nenhuma`, `:163`, `:166`, `:167` |
| Duas RESPOSTA + reabertura + validação | Histórico | Gerar eventos | Nada | Hoje um evento por campo; brief exige sequência | Corpo `app/chamados/services/historico.py:98`, `:105`, `:109`, `:112`; **NENHUM** para multiato. `app/chamados/tests/test_historico.py::test_MORDE_carimbo_de_pergunta_entra_como_evento`, `:121`, `:135`, `:136` cobra somente tipos presentes |

Perguntas antigas:

| Pergunta | Classificação |
|---|---|
| Resposta vazia volta a pendente do app na F5? | **DECIDIDO PELA LEI corte CORTES-F2-F9-1611 item 5 com a linha \| contestavel**: permanece fora, ação admin. (`app/docs/CORTES.md:488`.) |
| “Resposta vazia deveria voltar ao colaborador?” | **LEI NOVA: falta decidir a devolução ao colaborador na lei RESSUSCITAR-DEVOLVE-AO-COLABORADOR**; o corte expressamente registra sem decidir. (`app/docs/CORTES.md:488`; `app/docs/LEIS.md:151`.) |
| F5 pode mudar fases públicas/badge por um fold novo? | **DECIDIDO PELA LEI corte E4C-D04-D05 com a linha \| contestavel**: fases públicas conservadas; exigibilidade aditiva. (`app/docs/BACKLOG.md:2091`.) |
| Qual é o juiz de “respondida”? | **DECIDIDO PELA LEI L-130 com a linha \| contestavel**: instante **e** texto, dois meios pares ilegais, um juiz para todas as finalidades. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1552`.) |
| Vigia da projeção #15/#16 migra ou morre E4d? | Questão técnica, não lei nova; o corpo identifica explicitamente leitura do par escrito pela porta. Decisão técnica final da F5: [nao verificado]. (`app/chamados/services/vigia_fio.py:738`, `:740`, `:741`.) |
| C em SQL deve virar ato? | O modelo de atos não tem MATERIALIZACAO; inferir uma face do fold antigo não está autorizado pelo contrato atual. (`app/chamados/models.py:4024`; `app/chamados/juizes.py:1544`.) |
| A20/data conferida | **DECIDIDO PELA LEI corte LU-08 \| contestavel**; não é pergunta própria F5. (`app/docs/CORTES.md:466` — referência precisa ainda deve ser reconferida; **[nao verificado]**.) |

RED julgados estaticamente, sem execução:

| Caso | Exigência pela regra | MORDE-HOJE |
|---|---|---|
| RESPOSTA vazia, instante legado, painel/tarefa | Fora do app; ação admin; nunca “colab deve texto” | **sim**: tarefa declara COLABORADOR/RESPONDER. (`app/docs/CORTES.md:488`; `app/chamados/services/tarefa_do_dia.py:161`.) |
| Antigo RED-2: mesma entrada deve ir a pendentes app | Corte conserva histórico | **não**: resultado pendente do brief foi cortado. (`app/docs/CORTES.md:488`; `app/chamados/services/pergunta_no_app.py:81`.) |
| Instante legado + texto vazio, POST regularizar | Não tratar como resposta do colaborador | **sim no degrau**, não prova fechamento final indevido: guarda só busca instante nulo e a materialização ainda roda depois. (`app/docs/LEIS.md:151`; `app/chamados/services/fio.py:706`; `:716`.) |
| Instante legado + texto vazio, aplicar declaração com HH:MM válido e demais guardas liberadas | Resposta tem de entrar pela porta antes de validar | **sim no degrau**: instante existente faz pular resposta canônica. (`app/docs/LEIS.md:151`; `app/chamados/services/declaracao_texto.py:687`; `:693`; `:696`.) |
| Texto legado sem instante, escolha canônica do grupo | Não equiparar meio par ilegal a resposta válida | **sim na leitura crua**, montagem final deve isolar grupo/comparador | `app/chamados/services/fio.py:1279`; juiz `app/chamados/juizes.py:1552` |
| Duas respostas e reabertura no histórico | Brief RED-5 exige todos os atos | **sim contra requisito do brief**: histórico só percorre campos. Lei D03 literal na BASE: **[nao verificado]**; não confundir D04 de `app/docs/BACKLOG.md:2091`. (`app/chamados/services/historico.py:98`, `:112`.) |
| Antigos RED-1/3/4: ato difere da projeção, aplicar `leitura` antiga | Oráculo antigo não corresponde à F1 pousada | **não como RED fechado pela premissa antiga**. (`app/chamados/juizes.py:1544`, `:1550`, `:1552`.) |
| Reabertura real | Zerar par e V/autor, preservando anterior | **não**: teste existente cobra os quatro campos. (`app/chamados/tests/test_reabrir_nao_destroi.py:54`, `:58`.) |

O beco atual exclui da lista impeditiva pelo instante sozinho; para instante legado com texto vazio ele pode chegar a `BARRADA` onde o filtro novo deveria manter `None`, dependendo do pai/destino e C7. Não contabilizo esse cenário sem explicitar essas condições. (`app/chamados/services/becos.py:27`, `:30`, `:31`, `:33`; `app/docs/LEIS.md:151`.)

_6279 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
