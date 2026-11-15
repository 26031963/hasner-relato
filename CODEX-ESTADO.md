# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 14:05** · publicado em **15/11/2026 14:05** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 14:05 · sessao 55c4017566ce · - · - · origem=Ronald (tmux)

Complemento do censo:

| Classe | Locais efetivos |
|---|---|
| Produção, chamadas diretas | `app/chamados/reconciliador.py:406`, `:596`; `app/chamados/services/disputa_emissao.py:2007`, `:2081`; `app/chamados/services/materializacao.py:567`, `:1283`, `:1437`; `app/chamados/models.py:3386` |
| Testes, chamadas diretas reais | `app/chamados/tests/test_o261e_f2_fila.py:194`, `:205`, `:221`, `:228`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:725`, `:738` |
| Apelido efetivo em teste | `orig = PerguntaDisputa.carimbar`, seguido de `orig(self_p, via, ...)`: `app/chamados/tests/test_pep_reentrada.py:92`, `:96`; wrapper instalado em `:100`, exercitado em `:102` |
| Atribuições de Mock | `app/chamados/tests/test_e4b_c6_memoria.py:27`; `app/chamados/tests/test_e4b_classe_e_memoria.py:287`, `:1001` |
| Asserts sobre Mock | `app/chamados/tests/test_e4b_classe_e_memoria.py:399`, `:462`, `:490`, `:575`, `:1006`; `app/chamados/tests/test_e4b_c4v14_memoria.py:114` |
| Código sintético em strings, excluído das chamadas | `app/chamados/tests/test_o261e_f2_fila.py:63`, `:66`, `:67` |

Não encontrei alias de produção nem `getattr`/`methodcaller` efetivos para essa porta na varredura AST; ausência de construções dinâmicas não literais permanece [não verificado].

**Correção importante de W7:** escolha não chega hoje à chamada `materializar_pendentes_aprovadas → carimbar`: o queryset exige `materializada_em IS NULL` (`app/chamados/services/materializacao.py:1424`), o gate é `_pergunta_materializada_de_fato` (`:1433`), e o ramo escolha desse gate exige `materializada_em IS NOT NULL` (`app/chamados/juizes.py:709`). O ramo anterior de vias também exige materialização não nula (`app/chamados/juizes.py:682`). Isso descreve o fluxo sem mutação concorrente entre carga e gate; concorrência desse cenário: [não verificado].

As cinco choices de tipo não fecham o universo legado: teste existente cria `tipo_resposta='texto'` (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:635`), e `carimbar` não testa `tipo_resposta` (`app/chamados/models.py:3186`). Assim, nas linhas sem filtro de tipo, escrever “sem restrição de tipo no chamador”, não “somente os cinco tipos das choices”.

## 15/11 14:05 · sessao b51f314182fb · - · - · origem=Ronald (tmux)

C2 — conclusão: **perguntas 1 e 3 PROCEDEM; 2 e 4 NÃO PROCEDEM no alcance literal**.

1. **PROCEDE — premissas incompletas.** O universo 163 e o portão DECLARADO-X-GRAVADO têm registro documental em `app/docs/BACKLOG.md:1995` e `app/docs/RELATO.md:21`. A reconciliação **8 na sombra × 79 em prod** está registrada com a ressalva literal **“[diferenca nao verificada]”** em `app/docs/RELATO.md:21`; a contagem 8 reaparece em `app/docs/RELATO.md:66`. Os respectivos logs primários não foram fornecidos; comprovação deles [nao verificado]. **15 do laço** e **29 metades em exportada** aparecem como números afirmados pelo aval, sem resultado de sonda citável fornecido [nao verificado].

   Existe um contador próximo, **I18**, mas ele conta `validada_em != NULL`, `materializada_em == NULL`, `recusa_motivo == ''` e exclui os usuários `sistema`/`sistema_bot`; portanto não demonstra a pergunta “validada sem **batida** e sem recusa” pela função real (`app/chamados/management/commands/auditar_invariantes_chamados.py:1074`). O juiz de materialização real distingue testemunha/batida por tipo de resposta (`app/chamados/juizes.py:665`, `:711`).

2. **NÃO PROCEDE — colisão não demonstrada no literal completo.** C2 pede instrumento novo, mas sua última frase preserva a espera da volta de código até a redefinição; isso é compatível com COMPLEMENTO-3, item g. COMPLEMENTO-2 libera continuidade de espinha em voo, não todo instrumento; nenhuma revogação geral decorre dele. Referências: anexo `avais_todos.txt`, blocos `cortes_1358.txt`, item C2; `complemento3_queima.txt`, item g; `complemento2_queima.txt`, itens 1–2. Numeração física dos anexos [nao verificado]. Construção imediata seria interpretação adicional, não ordem expressa no C2 [nao verificado].

3. **PROCEDE — a medida exigida ainda não tem produtor comprovado.** “NOVA = 0 por EXECUÇÃO” não apresenta baseline, identificação das execuções nem função produtora fornecida; “esperado = legado medido (15)” depende de medição não fornecida [nao verificado]. I18 não fecha essa lacuna porque mede carimbo, universo humano e recusa, não ausência de batida pelo juiz real (`app/chamados/management/commands/auditar_invariantes_chamados.py:1074`). A exigência de medida pela função real está em `CLAUDE.md:35`; contrato de instrumento anterior ao uso, em `CLAUDE.md:394`. O “senão” geral existe: condição que não fecha = PAROU (`CLAUDE.md:37`); não devolver apenas pela ausência da palavra “senão”.

4. **NÃO PROCEDE — extrapolação não demonstrada pelo texto.** O C2 proíbe reabrir/retratar fechadas e tocar as 29 metades em exportada, e manda reconciliar produção só por leitura: anexo `avais_todos.txt`, bloco `cortes_1358.txt`, item C2. A identidade das 29 e o efeito real de execução permanecem [nao verificado]. Não converter ausência de prova em afirmação de que houve escrita/dinheiro tocado [nao verificado].

Deslocamentos próprios de C2: não há referência numérica a linha de código no seu texto; a fonte citada é `logs/BUG-PAR-POUSA/aval.txt`, item 2, entregue apenas no anexo. O registro local de C7 permanece em `app/core/juizes.py:302`; `pode_fechar` está em `app/chamados/models.py:1911`, e a exclusão de validadas de `sem_veredito_entre` está em `:1905`.

## 15/11 14:05 · sessao 76d3ec15c22a · - · - · origem=Ronald (tmux)

A — sugestão: `AVAL-DEVOLVIDO: CORTES-1358 A | perguntas 1, 3`, ressalvada a avaliação do pai sobre “sem senão”. A ausência das duas funções nesta árvore é verificável pela busca somente leitura; as linhas citadas hoje contêm outros métodos. (`app/chamados/juizes.py:285`; `app/chamados/models.py:3522`).

1. **PROCEDE: referências e premissas não conferem integralmente neste snapshot.**

   - `_leitura_conservada` e `_q_validada_pelo_ato` não existem nos arquivos Python desta cópia. `juizes.py:285` define `_retratada_com_trilha`; `models.py:3522` define `save`. Não se trata de simples deslocamento dos símbolos: a entrega fornecida atribui essas funções à raia F2 `8c6577245`, que o RELATO identifica como outra cópia. (`app/chamados/juizes.py:285`; `app/chamados/models.py:3522`; `app/docs/RELATO.md:37`).
   - A pergunta “são duas faces da mesma pergunta?” tem fonte documental: o RELATO chama `_q_validada_pelo_ato` de face SQL do degrau 0 do C6. A equivalência da implementação da F2 não pode ser conferida nesta árvore. (`app/docs/RELATO.md:14`; implementação F2: [nao verificado]).
   - O HEAD lê `validada_em` diretamente e considera a pergunta não viva se houver validação; a face SQL atual exige `validada_em IS NULL` para trazer pergunta viva. Não contém aqui o desempate `V >= R` citado da F2. (`app/chamados/juizes.py:1388`; `app/chamados/juizes.py:1402`; `app/chamados/juizes.py:1404`; `app/chamados/models.py:3468`).
   - **Deslocamento da porta:** `disputa_emissao.py:1480-1481` não zera V: conserva projeção sem linha e atribui resposta. O zeramento está em **1494-1495**; portanto +14 linhas para o par alegado no aval. (`app/chamados/services/disputa_emissao.py:1480`; `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1495`).
   - As referências de `entrega.md` à porta `1485-1496`, ao automático `1654-1680` e à projeção sem linha `models.py:4057` conferem no HEAD, sem deslocamento. (`app/chamados/services/disputa_emissao.py:1485`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1654`; `app/chamados/services/disputa_emissao.py:1679`; `app/chamados/models.py:4057`).
   - **187/186 têm fonte:** o RELATO registra 187 perguntas, 169 exportadas, 186 respostas reais e uma automática, com remissão à entrega LU2-13 e às sondas fornecidas. É prova documental herdada, sem nova aferição. (`app/docs/RELATO.md:13`; `app/docs/RELATO.md:48`; `app/docs/RELATO.md:56`).
   - A entrega fornecida deixa expressamente sem conferir se a resposta automática de pg32660 tem linha `RESPOSTA` real. Portanto “as 187 têm R e V ambos apenas projetados” e a possibilidade de conciliar o RED real com o selo universal 0/187 permanecem [nao verificado].

2. **NAO PROCEDE quanto à colisão construtor novo/COMPLEMENTO-2.**

   **F2 em voo: sim.** Há F2 construída em cópia própria, com c1/c2/c3, entrega e bloqueio herdado da F1; o registro posterior ainda manda ataque à F2. Isso é continuidade de obra existente, enquadrada no COMPLEMENTO-2 fornecido, não abertura de obra nova. (`app/docs/RELATO.md:37`; `app/docs/RELATO.md:95`; anexo `complemento2_queima.txt`, item 1).
   
   Não há autorização em A para enfileirar job novo na pista durante a interdição; a execução concreta da suíte permanece [nao verificado]. O código da F2 e sua identidade física “mesma cópia” não estão acessíveis nesta árvore; a existência documental da raia está em `app/docs/RELATO.md:37`.

3. **PROCEDE quanto à condição ainda não fechada/documentada; evitar declarar contradição já demonstrada.**

   O critério “0 das 187 mudam” exige classificar a pergunta automática, que o dado fornecido deixa sem verificar. Se tiver linha RESPOSTA real posterior a V, o próprio RED manda desassinar; se tiver apenas projeção, recebe a exceção. Qual dos ramos ocorre é [nao verificado]. Não há prova de que os 187 satisfaçam simultaneamente a preservação de estado e o caso que morde; não afirmar que o conflito já aconteceu. (Dados: anexo `entrega.md`, seção `[nao verificado]`; universo e risco de lavra: `app/docs/RELATO.md:48`).
   
   O literal de A dá critérios PRONTO e proibições, mas não traz um ramo explícito para falha do selo 0/187, da lista 186 ou de 0 FM/DiaPago; a frase geral “PAREI só em lei ou !” não nomeia essa falha. (Anexo `cortes_1358.txt`, item A).

4. **NAO PROCEDE quanto a alcance adicional comprovado; impacto zero ainda [nao verificado].**

   A preserva o estado em relação ao main, proíbe gravado e exige sombra/0 FM/0 DiaPago. O universo inclui 169 exportadas e leitores capazes de relavrar, portanto o zero depende do ensaio, que não foi fornecido. Não afirmar que A já tocou ou que comprovadamente deixará de tocar dinheiro. (`app/docs/RELATO.md:48`; anexo `cortes_1358.txt`, item A; ensaio: [nao verificado]).

**Referências importantes para consolidação:** `app/docs/RELATO.md:88` registra CLASSE-D entre pouso F1 e F2; `:40` explicita F1 → CLASSE-D → F2 → E4d. Isso não prova colisão com A (“depois da c6” não diz “imediatamente”), mas impede tratar a vaga da F2 como livre/agendada só pela c6 sem examinar a fila. (`app/docs/RELATO.md:40`; `app/docs/RELATO.md:88`).

## 15/11 14:04 · sessao 55c4017566ce · - · - · origem=Ronald (tmux)

Censo de produção: 8 referências diretas a `PerguntaDisputa.carimbar`; não apareceu alias de atributo, nome solto, `getattr` ou `methodcaller` nessa porta na varredura AST. Referências conferidas: `app/chamados/reconciliador.py:406`, `:596`; `app/chamados/services/disputa_emissao.py:2007`, `:2081`; `app/chamados/services/materializacao.py:567`, `:1283`, `:1437`; `app/chamados/models.py:3386`. O utilitário existente de busca por referência inclui atributo, nome solto e string exata: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:450`.

As duas chamadas de `disputa_emissao.py` deslocaram +14 linhas: 1993→2007 e 2067→2081. As demais posições citadas continuam iguais, inclusive a porta em `app/chamados/models.py:3186`.

| Chamada | Via × tipo possível pelo código | Atomic no chamador | Retorno tratado | Teste existente | [nao verificado]
|---|---|---|---|---|
| `reconciliador.py:406` | `sem_turno_aberto`; não filtra tipo, exige motivos C1 e texto vazio (`reconciliador.py:333`, `:363`, `:365`) | A função não abre atomic (`reconciliador.py:341`, `:415`); somente atomic do `save` (`models.py:3532`) | False→False; True segue fechamento e retorna True (`reconciliador.py:406`, `:412`, `:415`) | Positivo e mordida do carimbo atual: `test_o261_lavra_le_a_lampada.py:148`, `:129`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `reconciliador.py:596` | `celula_absolveu`; sem filtro de tipo no laço (`reconciliador.py:585`, `:596`) | Atomic por pergunta, com trava do colaborador (`reconciliador.py:593`, `:594`) | False→continue; True incrementa; erro é registrado (`reconciliador.py:597`, `:599`, `:603`) | Via pelo orquestrador: `test_o261_lavra_le_a_lampada.py:172`; chamada direta: `test_selo_celula_absolveu.py:70`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `disputa_emissao.py:2007` | `via_pelo_fato`; qualquer tipo permitido pelo filtro; resposta=`validar`, lavrar=True (`disputa_emissao.py:1927`, `:2007`) | Wrapper usa atomic atual ou abre um (`disputa_emissao.py:1900`) | False→continue; True incrementa resolvidas (`disputa_emissao.py:2009`, `:2010`) | Controle positivo real: `test_pep_reentrada.py:229`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `disputa_emissao.py:2081` | `via_pelo_fato(pai)` ou `chamado_encerrado`; não filtra tipo apesar da prosa “escolha”; resposta=`responder` (`disputa_emissao.py:2060`, `:2079`, `:2081`) | Mesmo wrapper (`disputa_emissao.py:1900`) | False→continue; True registra evento de auditoria (`disputa_emissao.py:2082`, `:2084`) | Guarda de voo com C6 mockado, limite declarado: `test_pep_reentrada.py:267`, `:301`, `:304`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `materializacao.py:567` | Somente `hora`; `declaracao` ou `ausencia_dia`; resposta=`validar` (`materializacao.py:539`, `:555`, `:567`) | Atomic por irmã (`materializacao.py:563`) | True→registra ABSORCAO; False→lista recusadas e evento (`materializacao.py:569`, `:574`, `:575`) | Uma ABSORCAO por irmã, mesmas datas, repetição sem duplicata: `test_e1_entradas.py:239`; recusa com trilha: `test_o261e_f2_fila.py:238` |
| `materializacao.py:1283` | `hora`/`data_hora`/`escolha`; `declaracao` se `declarou_hora`, senão `via_pelo_fato` ou `chamado_encerrado`; resposta=`validar`, lavrar=True (`materializacao.py:1227`, `:1268`, `:1271`, `:1274`, `:1283`) | Atomic de `_materializar_uma` e contexto de voo (`materializacao.py:1226`) | False levanta `_Reprovada`, desfaz transação; erro acumulado (`materializacao.py:1285`, `:1289`, `:1292`) | Um escritor durante reentrada e via do materializador: `test_pep_reentrada.py:109`, `:134`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `materializacao.py:1437` | `juiz_aprovou`; qualquer tipo aprovado pelo lastro; resposta=None (`materializacao.py:1424`, `:1433`, `:1437`) | Sem atomic próprio na função (`materializacao.py:1396`, `:1439`); via signal165 durante `Disputa.fechar`, há atomic externo (`signals.py:165`, `models.py:1969`) | True→inclui pk; False→omite (`materializacao.py:1437`, `:1438`) | Positivo, repetição e lastro recusado: `test_porta_desfecho_pergunta.py:222`, `:234`, `:256`; resposta por validar: `test_o261e_f2_porta_carimbo.py:687`; conclusão: sem teste (`models.py:4000`) | [nao verificado]
| `models.py:3386` | `fantasma`; não filtra tipo nem altera resposta (`models.py:3378`, `:3386`) | Sem atomic próprio; consumidor de produção abre um por pergunta (`reconciliador.py:1147`, `:1148`) | Propaga bool; já fantasma carimbada→False (`models.py:3383`, `:3386`) | Viva recusada e morta aceita: `test_o261e_f2_porta_carimbo.py:665`, `:676`; conclusão: sem teste (`models.py:4000`) | [nao verificado]

`via_pelo_fato` pode devolver **cinco vias**, não somente as duas mostradas na frota de escolhas: `cadastro_folga`, `adesao`, `ausencia_dia`, `completude_turno`, `chamado_encerrado` (`app/chamados/juizes.py:1793`, `:1794`, `:1802`, `:1810`, `:1811`). Os cinco tipos atuais são escolha, hora, posto_dropdown, escala_dropdown e data_hora (`app/chamados/catalogo/respostas.py:20`). Presença de cada combinação na produção: [não verificado].

A única chamada das oito que já registra DP no próprio chamador é a absorção: `app/chamados/services/materializacao.py:567`→`:569`. Os eventos dos outros chamadores são auditoria, não DP, por exemplo `disputa_emissao.py:2084` e `reconciliador.py:1150`.

A porta atual não contém atomic abrangendo seu corpo; chama `save` e retorna True (`app/chamados/models.py:3235`, `:3240`). O `save` abre atomic apenas para `super().save`, terminando antes de devolver à porta (`app/chamados/models.py:3532`, `:3534`). Portanto esse atomic não envolve um futuro `registrar` posterior dentro de `carimbar` (`app/chamados/models.py:3235`).

A nomenclatura do ato futuro e o significado de seu `ato_motivo` não estão implementados; o catálogo atual não possui conclusão e `ato_motivo` é texto livre sem choices (`app/chamados/models.py:4000`, `:4019`). A matriz prospectiva pode registrar “conclusão, nome pendente”, conservando a via dentro do ato conforme o aval fornecido; qualquer decisão adicional sobre codificar a via em `ato_motivo` é [não verificado].

## 15/11 14:04 · sessao 5aae7f45bbfd · - · - · origem=Ronald (tmux)

Censo feito por busca textual + AST em memória; nenhuma escrita nem teste executado. O snapshot não contém `atos_pergunta.py`, `_leitura_conservada` ou `instante_materializacao`; o leitor F1 descrito no anexo permanece **[nao verificado] nesta cópia**. [nao verificado]

**Escritores de `DecisaoPergunta`**

Existem 14 chamadas produtivas à porta `registrar`; nenhuma delas usa apelido de import. O escritor físico é `bulk_create`, sob atomic, seguido do evento da célula (`app/chamados/models.py:4095`, `:4096`, `:4097`).

| Função | Chamada / ato |
|---|---|
| `ChamadoColaborador.ressuscitar_perguntas` | Preserva RESPOSTA/VALIDACAO sem linha — `app/chamados/models.py:1610` |
| `DisputaSupervisao.fechar` | SILENCIO após `fechar_por_admin` — `app/chamados/models.py:1985`, `:1988` |
| `DisputaSupervisao.remover_motivo` | RETRATACAO, motivo textual “motivo … removido”, depois identidade — `app/chamados/models.py:2120`, `:2123`, `:2125` |
| `PerguntaDisputa._nascer` | Preserva RESPOSTA/VALIDACAO recebidas no nascimento — `app/chamados/models.py:3100`, `:3102` |
| `responder_pergunta_disputa_core` | RESPOSTA + legado — `app/chamados/services/disputa_emissao.py:1522`, `:1525` |
| `reabrir_para_correcao` | REABERTURA ou RECUSA, com foto — `app/chamados/services/disputa_emissao.py:1619`, `:1631`, `:1632` |
| `responder_automaticamente` | RESPOSTA + legado — `app/chamados/services/disputa_emissao.py:1676`, `:1677` |
| `carimbar_resposta_pela_trilha` | RESPOSTA — `app/chamados/services/disputa_emissao.py:1697`, `:1698` |
| `_carimbar_validacao` | VALIDACAO + legado — `app/chamados/services/validacao.py:162`, `:164` |
| `devolver_validacao_recusada` | DEVOLUCAO + legado — `app/chamados/services/validacao.py:227`, `:230` |
| `_materializar_ausencia_de_resposta` | ABSORCAO se `carimbar` retornou verdadeiro — `app/chamados/services/materializacao.py:567`, `:568`, `:569` |
| `fechar_o_fio_do_dia` | SILENCIO após `fechar_por_admin` — `app/ponto/services/veredito_celula.py:190`, `:195` |
| backfill `aplicar` | SILENCIO, relógio `materializada_em` — `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `:325` |
| backfill `_executar_dia` | RESPOSTA/VALIDACAO conservadas — `app/chamados/management/commands/backfill_fio_morto_e4b.py:141` |

Os dicionários de RESPOSTA/VALIDACAO conservadas nascem em `app/chamados/models.py:4076` e `:4080`; não são escrita física. A consulta de correspondência considera exclusivamente esses dois atos (`app/chamados/models.py:4070`).

Fora do fluxo de produção há uma exceção operacional explícita: a máscara da sombra resolve modelos dinamicamente e usa `_base_manager.bulk_update` (`bin/sombra_mascara_pessoal.py:46`, `:51`, `:62`, `:64`), incluindo os campos DP `ato_texto`/`ato_motivo` (`bin/sombra_regras_pessoais.py:142`). Não é chamada a `registrar` nem nascimento de ato.

**Choices**

`ATOS`: `resposta`, `validacao`, `devolucao`, `recusa`, `reabertura`, `silencio`, `retratacao`, `absorcao` (`app/chamados/models.py:4000`, `:4001`, `:4002`).

Não há `ATO_MOTIVO` com choices: `ato_motivo` é `CharField(max_length=240, blank=True, default='')` (`app/chamados/models.py:4019`; também migration histórica `app/chamados/migrations/0051_decisao_pergunta.py:47`). Existe apenas a constante textual `MOTIVO_PROJECAO_SEM_LINHA` (`app/chamados/models.py:4054`).

A conclusão nova falta em `ATOS` (`app/chamados/models.py:4002`). A via ainda não tem campo próprio em DP: campos atuais em `app/chamados/models.py:4014` a `:4026`. Nome do novo ato e convenção para codificar a via em `ato_motivo`: **[nao verificado]**, decisão de desenho ainda não presente no código. ABSORCAO exige ambas as referências, e nenhum outro ato pode recebê-las (`app/chamados/models.py:4035`).

**Leitores e impacto**

| Leitor | Comportamento atual / impacto |
|---|---|
| `tarefa_do_dia._pela_decisao` | **Risco concreto:** pega a última linha em `:147`; despacha os oito atos conhecidos em `:148`–`:175`; ato novo cairá em `ato_desconhecido:<valor>, None` em `:176`. Referências: `app/chamados/services/tarefa_do_dia.py:147`, `:176`. |
| Carga de `tarefa_do_dia` | Carrega todos os atos ordenados por pergunta/instante/PK; guarda os anteriores a `agora`; RETRATACAO posterior tem tratamento específico. Conclusão entrará na sequência normalmente e poderá tornar-se a última. `app/chamados/services/tarefa_do_dia.py:410`, `:412`, `:414`. |
| `PerguntaDisputa.respostas_anteriores` | Considera só RESPOSTA e `ATOS_COM_FOTO`; conclusão será ignorada, preservando histórico existente. `app/chamados/models.py:3651`, `:3652`, `:3656`. |
| `disputa_emissao.respostas_anteriores` | Encaminha ao método do modelo; mesmo impacto. `app/chamados/services/disputa_emissao.py:2122`. |
| `fio.montar_chamados_fio` | Prefetch de todas as decisões, com autores; passa a carregar também a nova linha, consumida pelo leitor anterior. `app/chamados/services/fio.py:261`. |
| `modal_fio` | Usa `perg.respostas_anteriores`; não passa a exibir conclusão diretamente. `app/templates/chamados/partials/modal_fio.html:205`, `:209`, `:212`. |
| `linhas_da_projecao_sem_linha` | Só busca RESPOSTA/VALIDACAO com correspondência de instante; conclusão não elimina esse passivo. `app/chamados/models.py:4070`, `:4074`, `:4078`. |
| `backfill_decisoes_pergunta.admin_decidiu_sem_ato` | Só SILENCIO/VALIDACAO fazem a pergunta sair do conjunto sem ato. Conclusão não será reconhecida por esse filtro. `app/chamados/management/commands/backfill_decisoes_pergunta.py:99`, `:105`. |
| Rechecagem do apply desse backfill | Repete filtro SILENCIO/VALIDACAO; mesmo limite. `app/chamados/management/commands/backfill_decisoes_pergunta.py:315`. |
| `foto_e4b` | Fotografa todas as DP do colaborador e todos os campos concretos: a nova conclusão aparecerá no diff como linha criada. `app/chamados/services/foto_e4b.py:250`, `:251`, `:280`. |
| `atos_pergunta.leitura` | Ausente da cópia; contrato/impacto da árvore F1 fornecido pelo anexo **[nao verificado]**. |
| Máscara da sombra | Lê e mascara `ato_texto`/`ato_motivo` de todas as linhas, inclusive novas; atenção se via for armazenada em motivo livre. `bin/sombra_regras_pessoais.py:142`, `bin/sombra_mascara_pessoal.py:51`, `:55`. |

Não achei outro leitor direto de DP, alias de DP, relação `decisoes` da pergunta ou SQL direto sobre sua tabela no código produtivo pesquisado; garantia além da análise estática: **[nao verificado]**.

**Provas existentes úteis**

- Escritor único e proibição de atualização/remoção: AST em `app/chamados/tests/test_e1_entradas.py:185`, teste em `:217`; tem mordida de update/delete em `:224`. Não segue aliases genéricos: comparação textual do nome em `:196`, `:201`.
- Imutabilidade funcional: `app/chamados/tests/test_e1_entradas.py:78`.
- ABSORCAO exatamente uma linha após duas materializações, referências e relógio iguais ao carimbo: `app/chamados/tests/test_e1_entradas.py:239`, `:242`, `:246`, `:248`, `:250`.
- DP + EventoCelula no mesmo atomic, inclusive falha do outbox: `app/chamados/tests/test_e4_expand.py:353`, `:375`, `:378`.
- Teste do ato novo de conclusão: **sem teste**; valor ainda ausente das choices (`app/chamados/models.py:4002`).

## 15/11 14:04 · sessao 6e36adaf9c8d · - · - · origem=Ronald (tmux)

Censo de chamadores conferido por busca textual e AST em memória, sem executar Django/banco nem escrever arquivos:

| Chamador | Depois do retorno |
|---|---|
| **Produção:** import local em `app/api/foto_recebida.py:39`, chamada `:48` | Ignora o `bool`; chama **sempre** `validar_pergunta(p.pk, user, disputa=...)` em `:51`; incrementa `n` somente se `r.get('ok')`, `:52-54`. |
| `app/chamados/tests/test_porta_chamado_e3.py:46` | Exige `True`; chama novamente em `:48`, exige `False` dentro de `assertNumQueries(0)`, `:47-48`. |
| Mesmo arquivo `:48` | Depois da asserção termina o teste, `:48-49`. | [nao verificado]
| Mesmo arquivo `:53` | Exige `False`, relê banco e exige texto original preservado, `:54`. |
| `app/chamados/tests/test_e3_10_cura_no_escritor.py:62` | Espera exceção; confere motivo `respondida_sem_texto`, ausência de carimbo/texto/DP e memória intacta, `:63-69`. |
| Mesmo arquivo `:75` | Exige `True`; relê pergunta, confere par e predicado `respondida_sem_texto=False`, `:76-78`. |
| Mesmo arquivo `:85` | Exige `False` e zero queries, `:84-85`; termina teste. | [nao verificado]
| `app/chamados/tests/test_e4_expand_orm.py:176` | Exige `True`; chama outra vez em `:177`, exige `False`; compara foto e DP, `:178-179`. |
| Mesmo arquivo `:177` | Compara foto preservando demais campos e ato resposta com instante anterior, `:178-179`. |
| **Teste dinâmico:** `app/chamados/tests/test_e4_expand.py:207` | `self.executar(EMISSAO, 'responder_automaticamente', ...)`, exige `True`; repete em `:208`, exige `False`; compara foto, ato e uma gravação, `:209-211`. |
| Mesmo arquivo `:208` | Compara foto, ato e uma gravação, `:209-211`. O executor extrai/compila função pelo nome, `:119-121`, `app/chamados/tests/e4_apoio.py:8-16`. |

Não achei alias da função: todos os imports dela encontrados são locais sem `as`, nos arquivos acima; `models.py` **não** a reexporta na lista de compatibilidade (`app/chamados/models.py:4235-4237`). A busca de `.py` achou 9 chamadas sintáticas diretas e as duas chamadas dinâmicas de teste acima; qualquer chamada gerada externamente à cópia: `[nao verificado]`.

A base arquivada contém **definição**, não outro chamador: `app/chamados/tests/e4b_c6_bases/0a65a5d64ae282a4ec55e468baf227b93609dcba/chamados/services/disputa_emissao.py.txt:1638-1658`. Menções em catracas e comentários não são chamadas (`app/chamados/tests/test_porta_resposta_pergunta.py:72`, `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:45`, `app/chamados/tests/test_porta_chamado_e3.py:102`).

Cascata produtiva:

- `resolver_disputas_de_foto` seleciona disputas **abertas** com motivo foto, `app/api/foto_recebida.py:65-68`; perguntas pelo episódio `motivo='foto_ausente', batida_origem=batida`, sem filtro de validação, `:71`.
- Ignora também o total de `_validar_pelo_dono`, `:74`; relê disputa, `:76`; incrementa `fechadas` em `:77` **antes** de verificar `d.fechada_em`, `:78-79`. Esse retorno conta disputas tratadas, inclusive as que permanecem abertas (`:62`, `:77-88`).
- Se ainda aberta, consulta `fio_vivo`, pode aparar motivo, `:81-82`; consulta `pode_fechar`, pode fechar, `:83-84`; se houver chamado, anexa ocorrência, `:85-87`; retorna contador, `:88`.
- APIs: anexam foto antes do kernel (`app/api/views.py:850-851`, `app/api/views_core.py:834-835`); chamam kernel (`views.py:860-862`, `views_core.py:844-846`); retornam o contador como `disputas_fechadas` (`views.py:863`, `views_core.py:847`).
- Para pergunta já validada, `validar_pergunta` relê banco, `app/chamados/services/validacao.py:410-417`; como a automática acabou de preencher `respondida_em`, passa a guarda `:418-420`; encontra `validada_em` e retorna exatamente `{'ok': False, 'estado': 'ja_validada', 'msg': 'Pergunta ja validada.', 'erros': []}`, `:421-423`. A materialização dessa função fica após esse retorno, `:450-477`.

Cuidado com “efeito exato”: o save direto em `app/chamados/services/disputa_emissao.py:1676` dispara receivers:

- `_becos_post_save_pergunta` agenda `fechar_beco_da_disputa` em `on_commit`, `app/chamados/signals.py:229-240`; o leitor classifica pelo `respondida_em`/estado dos chamados e `pode_fechar`, `app/chamados/services/becos.py:25-33`; pode resolver perguntas e fechar disputa, `:36-59`; evento reconsulta após commit, `:66-80`.
- `_outbox_post_save_pergunta` atua sincronamente quando `update_fields` intersecta `CAMPOS_QUE_ACORDAM`, `app/chamados/signals.py:252-259`. Portanto distinguir “dois campos escritos diretamente” de eventuais efeitos condicionais da rede. Sem caso concreto, quais cascatas realmente ocorreram: `[nao verificado]`.

Recusa de texto novo é mais precisa que “recusa sempre vazio”: `recusa_respondida_sem_texto` chama o predicado com o par solicitado, `app/chamados/services/fio.py:1595-1605`; só lança se o par for `respondida_sem_texto`. O teste cobre instante preenchido e `('', '   ', None)`, `app/chamados/tests/test_e3_10_cura_no_escritor.py:58-69`.

## 15/11 14:04 · sessao 1698f1bfad8f · - · - · origem=Ronald (tmux)

A composição já usada pela casa é `competencia_do_dia_entregue(p.data_evento, p.disputa.colaborador.empresa, memo=...)`, que usa `janela_atual` e `competencia_entregue`; não usa mês civil nem status de FechamentoMensal (`app/chamados/services/vigia_fio.py:843-865`). `janela_atual` respeita o corte da empresa (`app/ponto/janelas.py:36-63`). Exportada significa TXT vigente não invalidado (`app/folha/export.py:580-594`).

O modelo tem `FotoBatida.criado_em`, relacionado pelo episódio `p.batida_origem`, mas não há carimbo específico do último recebimento da foto (`app/chamados/models.py:2518-2519`; `app/ponto/models.py:1036-1041`). O upload posterior usa `update_or_create`, preservando `criado_em` quando já existe registro: portanto só é possível contar **registro de foto criado após V**, não todos os recebimentos posteriores (`app/ponto/registro_batida.py:298-313`). `Batida.sincronizado_em` pertence à batida, não à foto (`app/ponto/models.py:72-79`).

Texto vazio inclui whitespace pelo juiz real `texto_vazio` (`app/chamados/juizes.py:1523-1525`). O envelope já chama shell no tenant `juliani` (`bin/sonda_leitura.sh:239-240`).

Sugestão de script, somente SELECT; os únicos identificadores emitidos são PKs de perguntas:

```python
# Bases: chamados/juizes.py:1523-1525;
# chamados/services/vigia_fio.py:843-865;
# ponto/janelas.py:60-63; ponto/models.py:1036-1041.
import json
from collections import defaultdict
from django.db import connection
from chamados.models import PerguntaDisputa
from chamados.juizes import texto_vazio
from chamados.services.vigia_fio import competencia_do_dia_entregue
from ponto.janelas import janela_atual

assert connection.schema_name == "juliani", "tenant inesperado"

por_tipo = defaultdict(list)
por_competencia = defaultdict(list)
por_tipo_competencia = defaultdict(list)
foto_por_grupo = defaultdict(list)
todos = []
memo = {}

qs = (
    PerguntaDisputa.objects.filter(validada_em__isnull=False)
    .select_related(
        "disputa__colaborador__empresa",
        "batida_origem__foto_batida",
    )
    .order_by("pk")
)

for p in qs.iterator(chunk_size=500):
    if not texto_vazio(p.resposta_colab):
        continue
    todos.append(p.pk)
    empresa = p.disputa.colaborador.empresa
    if p.data_evento is None:
        competencia = "SEM_DATA"
        estado = "INDETERMINADA"
    else:
        fim = janela_atual(p.data_evento, empresa)[1]
        competencia = "%04d-%02d" % (fim.year, fim.month)
        estado = (
            "exportada"
            if competencia_do_dia_entregue(
                p.data_evento, empresa, memo=memo
            )
            else "aberta"
        )

    tipo = p.tipo_resposta
    chave = (tipo, competencia, estado)
    por_tipo[tipo].append(p.pk)
    por_competencia[(competencia, estado)].append(p.pk)
    por_tipo_competencia[chave].append(p.pk)

    b = p.batida_origem
    fb = getattr(b, "foto_batida", None) if b is not None else None
    if b is None:
        foto_estado = "SEM_EPISODIO"
    elif fb is None:
        foto_estado = "SEM_REGISTRO_FOTOBATIDA"
    elif not fb.foto.name:
        foto_estado = "REGISTRO_SEM_ARQUIVO"
    elif fb.criado_em > p.validada_em:
        foto_estado = "REGISTRO_FOTO_CRIADO_APOS_VALIDACAO"
    else:
        foto_estado = "REGISTRO_FOTO_CRIADO_ATE_VALIDACAO"
    foto_por_grupo[chave + (foto_estado,)].append(p.pk)

def emitir(rotulo, chave, ids):
    print(json.dumps(
        {"grupo": rotulo, "chave": chave, "n": len(ids),
         "pergunta_ids": ids},
        ensure_ascii=True,
    ))

emitir("total", [], todos)
for chave, ids in sorted(por_tipo.items()):
    emitir("tipo_resposta", [chave], ids)
for chave, ids in sorted(por_competencia.items()):
    emitir("competencia", chave, ids)
for chave, ids in sorted(por_tipo_competencia.items()):
    emitir("tipo_competencia", chave, ids)
for chave, ids in sorted(foto_por_grupo.items()):
    emitir("foto_registro", chave, ids)
print("FIM")
```

`SEM_DATA/INDETERMINADA` evita inventar competência; o vigia da casa opta por aberta para a sua própria medida, explicitamente, mas a composição devolve `None` sem dia (`app/chamados/services/vigia_fio.py:823`, `:852-856`). Se desejar seguir literalmente aquele rótulo de medida, pode usar `SEM_DATA/aberta` e explicar a convenção.

## 15/11 14:04 · sessao fef56d00aea9 · - · - · origem=Ronald (tmux)

Porta/transação/rollback, conferidos:

- Linhas do aval não se deslocaram nesta área: `app/chamados/models.py:3186` (`carimbar`); auxiliares em `:3180`, `:3242`, identidade em `:3251`; `save` em `:3522`; `DecisaoPergunta.registrar` em `:4086`.
- Hoje `carimbar` não abre `atomic`; aplica C/autor/via em memória (`models.py:3212`), aplica R/V só se solicitado e houver texto (`:3213`), consulta C6 (`:3222`), fotografa lavra (`:3230`), eventualmente lavra (`:3233`), salva (`:3235`) e retorna `True` (`:3240`). C6 vivo restaura e retorna `False` (`:3226`); erro restaura e sobe (`:3223`, `:3236`).
- `save` abre seu próprio `atomic` em `models.py:3532`; DP abre outro em `:4095`, escreve via `bulk_create` (`:4096`) e acorda células (`:4097`). Para ser o mesmo atomic, o bloco comum precisa estar dentro da porta, abrangendo `self.save(...)` e `DecisaoPergunta.registrar(...)`, com o `except` atual fora desse bloco, e `True` só depois do sucesso dos dois — inferência de desenho ancorada nesses pontos.
- Ordem concreta preferível para preservar a leitura atual: C6 em memória → lavra opcional → `save` → `registrar` → retorno. O C6 e a lavra hoje antecedem `save` (`models.py:3222`, `:3233`, `:3235`); o ato novo não pode nascer no caminho `False` (`:3226`). Contrato do futuro leitor F1 dentro do próprio C6 não verificável nesta cópia; cuidado para não criar dependência circular no ato que ainda vai nascer [não verificado].
- Atomic comum também mantém callbacks depois do DP: o `post_save` da pergunta agenda fecho de becos em `on_commit` (`app/chamados/signals.py:240`) e emite EventoCelula síncrono (`:259`). Só `save.atomic` permite que o callback rode antes de um `registrar` seguinte em autocommit (`models.py:3532`, `signals.py:240`) — inferência do fluxo.
- Tanto `save` como `registrar` acordam células (`signals.py:259`, `models.py:4097`). `acordar_celulas` deduplica apenas dentro da própria chamada pelo conjunto local `vistos` (`models.py:3321`, `:3330`); deduplicação global do evento não foi inspecionada [não verificado].

O que a restauração cobre:

- `_desfazer_em_memoria` restaura exatamente `materializada_em`, `materializada_por_id`, `via_resolucao`, `respondida_em`, `validada_em`, `validada_por_id`, e apaga `_pv_memo` (`models.py:3244`).
- A lavra é restaurada separadamente por `_vl.desfazer_lavra_em_memoria` (`models.py:3237`), que restaura `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/veredito_lavrado.py:284`).
- Se o DP falhar **depois de um save bem sucedido**, surge lacuna adicional: `save` já atualizou `_par_carregado` (`models.py:3534`); `_desfazer_em_memoria` não o restaura (`:3244`). A instância pode ficar com campos antigos e fotografia do par nova — inferência direta.
- Não restaura linhas DP nem EventoCelula: isso depende do rollback SQL comum; DP é criado por `bulk_create` e a porta não recebe/atualiza a própria instância da pergunta (`models.py:4094`).
- Também não invalida prefetch de decisões: `respostas_anteriores` explicitamente usa `self.decisoes.all()` e o cache existente (`models.py:3648`); o helper só apaga `_pv_memo` (`:3249`). Se o novo desenho reutilizar uma instância com decisões prefetched, isso exige consideração.
- Rollback posterior do atomic do chamador depois de `carimbar=True` também não chama esse helper: a porta já retornou (`models.py:3240`); preservação de memória nesse cenário não é coberta pelo contrato atual [não verificado quanto a testes]. [nao verificado]

Identidade:

- Aceita só `VIAS_DE_IDENTIDADE` (`models.py:3261`), fonte com `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`, `motivo_retratado` (`:2588`). `competencia_trancada` continua legível, mas não é aceita pelo escritor (`:3178`, `:3261`).
- Grava via + instante + autor; `manter_carimbo=True` preserva instante/autor anteriores se já houver carimbo; pode gravar `recusa_motivo` no mesmo save (`models.py:3265`). Retorna `True` após save (`:3273`), recusa retorno ao fato por identidade inadequada com `False` (`:3263`).
- Não pergunta ao C6 porque a decisão de identidade é do chamador, e C6 conhece essas vias pela própria identidade (`models.py:3252`). Portanto não é conclusão de materialização da porta `carimbar`; misturar os atos eliminaria a distinção que o código mantém (`:3160`).
- Chamadores de identidade reais observados: admin (`models.py:3147`), retratação de motivo (`:2125`), paridade (`materializacao.py:930`), fato ocupado (`:1105`), duplicata (`fio.py:1398`). `remover_motivo` já registra RETRATACAO antes da identidade (`models.py:2120`), com motivo descritivo que não é literal `obsoleta` (`:2123`).

Testes existentes, apenas lidos: [nao verificado]

| Cobertura | Prova |
|---|---|
| C6 recusa, memória e banco sem carimbo | `app/chamados/tests/test_o261e_f2_porta_carimbo.py:713` |
| identidade/tranca rejeitadas por `carimbar` | `test_o261e_f2_porta_carimbo.py:733` |
| via de efeito rejeitada por identidade | `test_o261e_f2_porta_carimbo.py:740` |
| falha de save restaura memória | `app/chamados/tests/test_o261e_f2_fila.py:187` |
| falha de lavra restaura memória | `test_o261e_f2_fila.py:198` |
| falha de save depois da lavra restaura lavra | `test_o261e_f2_fila.py:208` |
| sucesso grava carimbo/autor/via | `test_o261e_f2_fila.py:224` |
| admin identidade 2× não move carimbo | `app/chamados/tests/test_porta_pergunta_identidade.py:194` |
| admin grava autor/via | `test_porta_pergunta_identidade.py:209` |
| falha do futuro `registrar` após save, rollback conjunto DP/pergunta/outbox, restauração de `_par_carregado` | sem teste encontrado [não verificado] | [nao verificado]

Interseções verificáveis nesta cópia:

- `app/chamados/juizes.py:285` ainda lê trilha obsoleta via log; `app/chamados/services/carga_pergunta_viva.py:110` também carrega `LogAuditoria`. F1 `atos_pergunta.leitura` e diff real da outra worktree não estão verificados [não verificado].
- `tarefa_do_dia` escolhe a última decisão (`app/chamados/services/tarefa_do_dia.py:147`) e atualmente cai em `ato_desconhecido:<ato>` para conclusão nova (`:176`); portanto recebe impacto funcional direto do DP novo, ainda que E4d não edite o arquivo.
- `tarefa_do_dia` carrega C6 com `carga_da_vida` e consulta o juiz (`tarefa_do_dia.py:518`, `:521`), logo a migração do leitor precisa conferir também essa carga.
- `vigia_fio` consome a porta da tarefa (`app/chamados/services/vigia_fio.py:561`, `:649`) e ainda consulta C6 em outros censos (`:776`, `:834`); recebe impacto indireto do novo ato/leitor.
- Arquivos declarados pelo usuário para obras em voo coincidem com esses pontos; hashes/diffs reais das worktrees E4c F1 e E3-RESTO-R3 não conferidos [não verificado].

## 15/11 14:03 · sessao 05c6694e5160 · - · - · origem=Ronald (tmux)

Leitores e efeitos confirmados no snapshot, todos os caminhos abaixo relativos à raiz:

- `responder_automaticamente` grava diretamente somente `resposta_colab=texto` e `respondida_em=instante`, incluindo conservação das projeções legadas sem linha e registro da nova decisão RESPOSTA no mesmo `atomic`; não modifica diretamente `validada_em`, `validada_por`, `materializada_em`, `materializada_por`, `via_resolucao`, `tipo_resposta` ou disputa. `app/chamados/services/disputa_emissao.py:1671-1679`.
- A conservação consulta correspondência `(pergunta_id, ato, ato_em)` para RESPOSTA e VALIDACAO; pode adicionar a resposta antiga, mesmo vazia, se havia `respondida_em`, e a validação antiga com `validada_por_id` e texto anterior vazio, se faltavam linhas correspondentes. `app/chamados/models.py:4067-4083`.
- O registro da nova resposta não fornece `ato_por`; campo nullable. `app/chamados/services/disputa_emissao.py:1677-1678`; `app/chamados/models.py:4017`.
- `estado`: antes e depois continua VALIDADA se `materializada_em is None`; continua RESOLVIDA se há materialização, pois validação vence antes da leitura da resposta. `app/chamados/models.py:3124-3126`.
- `.respondida`: muda de False para True se o carimbo anterior era nulo; se já havia carimbo, permanece True, mas troca a data. `app/chamados/models.py:3640-3641`; `app/chamados/services/disputa_emissao.py:1674`.
- C6 atual lê os campos de projeção: `projecao_temporal_da_pergunta` retorna os campos atuais, com recorte por `agora`; `_pergunta_viva_calc` retorna False imediatamente quando `validada` não é nula, antes de considerar resposta. Assim a nova resposta posterior à validação não desassina nem reabre a pergunta neste snapshot. `app/chamados/juizes.py:1388-1405`.
- A chave do memo lê `validada_em`, `respondida_em`, `resposta_colab`, `via_resolucao`, `materializada_em`, etc.; a troca do par invalida naturalmente o memo por mudança da chave. `app/chamados/juizes.py:854-862,893-905`.
- Face SQL `q_vivas` exige `validada_em__isnull=True`, portanto a pergunta validada continua excluída. `app/chamados/models.py:3468-3471`.
- App: `perguntas_do_app` primeiro filtra por `q_vivas`, depois C6; a pergunta validada fica fora do universo, tanto antes quanto depois. A partição pendente/histórico só opera nas vivas e lê `respondida_em is None` + disputa aberta. `app/chamados/services/pergunta_no_app.py:52-61,76-85`. Portanto não dizer que passa para “histórico do app” neste caso.
- `respondida_sem_texto`: antes pode ser True se havia carimbo antigo; depois será False para texto automático não vazio. `app/chamados/juizes.py:1523-1531`.
- `respostas_sem_veredito`: exige validação nula e materialização nula, além de carimbo OU texto; esta pergunta validada não entra antes nem depois. `app/chamados/models.py:1905-1909`.
- `pode_fechar` exige lista sem veredito vazia E ausência de `fio_vivo`; esta pergunta não bloqueia nenhum desses dois juízos por continuar validada. Outras perguntas podem bloquear, dados não fornecidos. `app/chamados/models.py:1915-1917`; `app/chamados/juizes.py:1404-1405,1507-1516`.
- Fila a validar usa `q_vivas` e `q_respondida_com_texto`; permanece fora pela validação. `app/chamados/juizes.py:1723-1727`; `app/chamados/models.py:3468`.
- Validador relê do banco e, com novo `respondida_em` preenchido, retorna `{'ok': False, 'estado': 'ja_validada', ...}` antes de carimbar/materializar. `app/chamados/services/validacao.py:410-423`.
- Se o materializador geral da disputa for chamado por outra via, seu filtro pega perguntas com `respondida_em` não nulo + `validada_em` não nulo + `materializada_em` nulo. A troca pode inserir a pergunta nesse universo se antes `respondida_em` era nulo; a própria chamada automática seguida da validação recusada não executa esse materializador pelo validador. `app/chamados/services/materializacao.py:1375-1385`; `app/chamados/services/validacao.py:421-423`.
- `.resposta_display` passa a ler o texto novo, com tradução de dropdown/opções dependendo do tipo; fallback é a resposta literal. Histórico de respostas lê decisões ordenadas e considera anterior a resposta superada por outra. `app/chamados/models.py:3643-3659,3662-3686`.

Cascatas que impedem afirmar “globalmente só dois campos mudam”:

- `PerguntaDisputa.save` executa `super().save()` em atomic, portanto receptores participam do save. `app/chamados/models.py:3522-3534`.
- Save do par acorda outbox sincronamente: os campos estão em `CAMPOS_QUE_ACORDAM`; receptor chama `acordar_celulas`. `app/chamados/models.py:3303-3305`; `app/chamados/signals.py:252-259`.
- `DecisaoPergunta.registrar` grava decisões e chama `acordar_celulas` novamente. A função emite um evento por célula encontrada no lote, podendo usar FK ou par colaborador/data; sem célula não emite. `app/chamados/models.py:4094-4099,3314-3332`.
- Outro receptor agenda `fechar_beco_da_disputa` em `on_commit`. `app/chamados/signals.py:229-240`.
- Beco lê `respondida_em` para decidir se ainda há pergunta visível; exige chamado pai terminal e `pode_fechar`. Pode passar a ser beco após a gravação caso o carimbo anterior fosse nulo e os demais requisitos fechem. `app/chamados/services/becos.py:25-33`.
- `fechar_beco_da_disputa` relê do banco e, se FECHÁVEL, chama `fechar_beco`, que resolve perguntas associadas a chamados terminais e fecha disputa quando permitido. Campos adicionais efetivamente alterados dependem do restante do estado, não fornecido. `app/chamados/services/becos.py:66-80,41-59`.
- O chamador explícito da foto ainda relê disputa e, se aberta, lê `fio_vivo`, apara motivo e fecha quando `pode_fechar`; pode anexar ocorrência no chamado. `app/api/foto_recebida.py:74-87`.

Não há `app/chamados/services/atos_pergunta.py`, `_leitura_conservada` ou `_q_validada_pelo_ato` no código desta cópia (pesquisa por nome e arquivos); só menções documentais em `app/docs/RELATO.md:14,50,90,111,127`. Não usar como implementação vigente.

## 15/11 14:02 · sessao 30b1ff93b5e0 · - · - · origem=Ronald (tmux)

B — evidências no snapshot:

- A função está em `app/chamados/services/disputa_emissao.py:1654-1679`; a faixa citada `1640-1666` começa ainda em `reabrir_para_correcao` e não cobre a escrita automática (`app/chamados/services/disputa_emissao.py:1640-1649`, `:1673-1679`). `app/api/foto_recebida.py:48` permanece correto.
- A premissa operacional confere: havendo texto prévio retorna `False`; caso contrário grava texto e resposta, sem guarda nem limpeza de `validada_em` (`app/chamados/services/disputa_emissao.py:1663-1679`).
- O literal universal “só recusa quando já há texto” não confere integralmente: também chama a recusa de resposta sem texto (`app/chamados/services/disputa_emissao.py:1667-1671`), que pode levantar `RespondidaSemTexto` (`app/chamados/services/fio.py:1595-1605`). No chamador citado, o texto constante não vazio elimina esse contracaso (`app/api/foto_recebida.py:25`, `:48`).
- O chamador ignora o booleano da resposta automática e chama o validador incondicionalmente (`app/api/foto_recebida.py:48-53`). O validador relê o banco e devolve `ok=False, estado='ja_validada'` para validação anterior; portanto não desfaz a resposta já gravada (`app/chamados/services/validacao.py:414-423`; `app/chamados/services/disputa_emissao.py:1675-1678`).
- O fluxo externo continua: atualiza a disputa, consulta `fio_vivo`, pode aparar motivo e fechar (`app/api/foto_recebida.py:74-87`).
- Os campos permitem validação com texto vazio: texto tem default vazio e validação é nullable (`app/chamados/models.py:2525-2527`). O total de validadas sem texto na frota é `[nao verificado]`: o anexo fornecido relata um caso, não esse censo `[nao verificado]`.
- B autoriza registro e censo, não construção: não identifico colisão com QUEIMA/COMPLEMENTO-2 no alcance literal fornecido; referência disponível apenas no anexo virtual `avais_todos.txt`, bloco `cortes_1358.txt`, item B `[nao verificado]`.
- O registro/censo de B não autoriza executar o escritor automático nem o fechamento descrito acima; efeitos desses caminhos não são efeitos necessários do trabalho B (`app/chamados/services/disputa_emissao.py:1675-1678`; `app/api/foto_recebida.py:74-87`; alcance do anexo `[nao verificado]`).
- Não há repositório Git acessível nesta cópia; o delta histórico exato contra `4b75cba` é `[nao verificado]`. O deslocamento/inadequação da faixa citada no arquivo atual é verificável como indicado acima (`app/chamados/services/disputa_emissao.py:1640-1679`).

Sugestão ao consolidador: Q1 deve registrar a ressalva literal e o deslocamento; não converter ausência de censo atual em “zero”, nem confundir a guarda extra com correção do risco sobre validação (`app/chamados/services/disputa_emissao.py:1663-1679`; `app/api/foto_recebida.py:25`, `:48`).

## 15/11 13:50 · sessao 9051fd3ab9a9 · - · - · origem=Ronald (tmux)

Portões identificados: V1 lê `pergunta_viva` em `chamados/reconciliador.py:399` antes de `.carimbar` em `:406`; V5 lê em `chamados/services/disputa_emissao.py:2069` antes de `.carimbar` em `:2081`.

Para V1, extrair corpo integral com `e4_apoio.funcao`, fornecer `_tem_turno_aberto=False`, motivo permitido, texto vazio, C=None, `carimbar=Mock`, e importar o C6 real do harness. Esperado: retorno False e zero chamadas ao carimbo (`chamados/reconciliador.py:365`).

Para V5, reutilizar `test_e4b_classe_e_memoria.AlcanceTest` e seu `resolver`, que executa o corpo produtivo integral (`chamados/tests/test_e4b_classe_e_memoria.py:382`). Cenário executado sem banco:

```python
c = E4B.AlcanceTest()
c.setUp()
p = c.p
p.tipo_resposta = 'texto'
p.data_evento = None
p.celula = None
p.chamado_destino = None
p.chamado_destino_id = None
c.pai._ancoradas = Mock(return_value=[])
c.c6.__globals__['_pergunta_materializada_de_fato'] = lambda *a, **k: True
completar_f1(c.c6.__globals__, '_dia_de_turno_da_pergunta')
c.resolver(c.pai)
assert p.carimbar.call_count == 0
```

Antes da cura: C6=False, carimbar=1, disputas_fechadas=1; captura persistida em arquivo [nao verificado]. A premissa direta `c.c6(p)` exige `E4B.modulos_isolados(c.mods)` (`chamados/tests/test_e4b_classe_e_memoria.py:382`).

Censo AST local: oito chamadas, nos sítios `chamados/reconciliador.py:406`, `:596`; `chamados/models.py:3386`; `chamados/services/materializacao.py:567`, `:1283`, `:1437`; `chamados/services/disputa_emissao.py:2007`, `:2081`. Para P12, comparar `(arquivo, ast.unparse(chamada))`, preservando também argumentos. Nenhum arquivo editado.

## 15/11 13:50 · sessao 633dd4848df6 · - · - · origem=Ronald (tmux)

P9 tem três custos distintos:

- `_leitura_da_pergunta` reconsulta decisões em cada chamada e a conservação consulta novamente: `chamados/juizes.py:329`, `chamados/models.py:4070`.
- L2 faz `.exists()` por pergunta: `chamados/juizes.py:387`.
- `_marco_do_efeito` agrega batidas por chamada avulsa: `chamados/juizes.py:362`. O caminho com carga já evita esse memo: `chamados/juizes.py:983`.

Sugestão: carga fornecer decisões integrais agrupadas, linhas conservadas agrupadas e conjunto de IDs com trilha L2; `_leitura_da_pergunta` e `_retratada_com_trilha` recebem `carga` opcional, preservando o avulso e sua invalidação. A conservação canônica pode ser chamada uma vez sobre todas as perguntas, sem editar models: `chamados/models.py:4057`.

Não usar somente `tarefa._dec`: ele já exclui atos futuros, enquanto o slot compara leitura cortada e integral: `chamados/services/tarefa_do_dia.py:421`, `chamados/juizes.py:447`.

Propagar carga pelos leitores em `chamados/juizes.py:778`, `chamados/juizes.py:1451`, `chamados/juizes.py:1479` e pelas quatro chamadas de trilha em `chamados/juizes.py:902`, `chamados/juizes.py:920`, `chamados/juizes.py:923`.

Testes existentes: [nao verificado]
- N constante com carga e paridade avulsa: `chamados/tests/test_e3_c8b_sem_banco.py:268`.
- Memo avulso isolado da carga: `chamados/tests/test_e3_c8b_sem_banco.py:302`.
- Trilha L2 real: `chamados/tests/test_e3_c8b_sem_banco.py:421`.
- Invalidação por ato, reabertura, plantio e retratação: `chamados/tests/test_e4c_f1_juizes_le_o_ato.py:277`.
- A contagem ORM atual cobre apenas composição SQL; precisa extensão aos três sítios: `chamados/tests/test_e4c_f1v4_custo.py:107`.

Risco: alterar os testes antigos que exigem exatamente duas leituras requer declarar as leituras adicionais de lote; a expectativa atual está em `chamados/tests/test_e3_c8b_sem_banco.py:279`. O limite documental de nove também precisa refletir a implementação: `chamados/services/carga_pergunta_viva.py:70`.

Nenhum arquivo editado; nenhuma consulta a banco executada.

## 15/11 13:46 · sessao 8d34801ce890 · E3-RESTO-R3 · construtor · origem=Code

Base: `6fb74c6d285a3f280800a5965c41102ec7121244`. SHA256 do código: `6644768e0fea2a64ee4700a54bd0e01cf44b813decbd474b99c0294a13852a49`. [Manifesto](chamados/tests/e3_r3_c11_auditoria.txt:1)

[Entrega completa — 29 linhas](chamados/tests/e3_r3_c11_entrega.txt:1), com `arquivo::função`, comandos e logs por item:

| Item | RED antes da cura | GREEN |
|---|---|---|
| 1 — LU-01 | [FAILED](chamados/tests/e3_r3_c11_red1.txt:26) | [32 OK](chamados/tests/e3_r3_c11_green1.txt:41) |
| 2 — dia no href e marca | [FAILED](chamados/tests/e3_r3_c11_red2.txt:25) | [4 OK](chamados/tests/e3_r3_c11_green2.txt:10) |
| 3 — recorte das pautas | [FAILED](chamados/tests/e3_r3_c11_red3.txt:51) | [6 OK](chamados/tests/e3_r3_c11_green3.txt:15) |
| 4 — relógio | [FAILED](chamados/tests/e3_r3_c11_red4.txt:69) | [3 OK](chamados/tests/e3_r3_c11_green4.txt:9) |
| 5 — alarme passivo | [FAILED](chamados/tests/e3_r3_c11_red5.txt:16) | [2 OK](chamados/tests/e3_r3_c11_green5.txt:8) |
| 6 — cor existente | [FAILED](chamados/tests/e3_r3_c11_red6.txt:29) | [2 OK](chamados/tests/e3_r3_c11_green6.txt:9) |
| 7 — docstrings e drawer | [FAILED](chamados/tests/e3_r3_c11_red7.txt:36) | [3 OK](chamados/tests/e3_r3_c11_green7.txt:9) |

Regressão local: **137 OK**; mutantes c11: **23/23 mortos por asserção**, com controles verdes. [Regressão](chamados/tests/e3_r3_c11_regressao.txt:168), [mutantes](chamados/tests/e3_r3_c11_mutantes.txt:550).

Censo dos cinco leitores e escritores/leitores dos campos constam na [entrega](chamados/tests/e3_r3_c11_entrega.txt:14).

LEI-AKITA: `padrao=Single Source of Truth / Aggregate Root; Clock Injection; CQRS com projecoes reconstruiveis`. [Referências](chamados/tests/e3_r3_c11_entrega.txt:29)

Sem commit: o `git-dir` fica fora de `app`, único diretório autorizado para escrita. [Evidência](chamados/tests/e3_r3_c11_auditoria.txt:2). ORM, produção, renderização Django e certificação final: [nao verificado].

## 15/11 13:29 · sessao 63adf9493c6c · - · - · origem=Ronald (tmux)

Censo do residual `decidir → admin`:

- **Folha:** dia muda `chamado→admin`; pessoa isolada `C→A`, misto com `colab` pode mudar `C→B`; outro dia `chamado` conserva C (`folha/fora_da_folha.py:89`, `:103`, `:214`, `:231`). Balde visual Admin permanece Admin (`folha/retrato_folha.py:193`).
- **Vigia:** `alcance_por_rota_papel` muda `porta_fila|chamado→porta_fila|admin`; `alcance_porta_exige_o_admin` aumenta. Rota e `fio_vivo` permanecem (`chamados/services/vigia_fio.py:578`, `:601`, `:369`).
- **Furo:** nenhum balde muda; lê rota (`chamados/services/furo_sem_canal.py:98`, `:104`).
- **Cadastro:** nenhum balde muda; lê presença do envelope (`escala/services/cadastro_realidade.py:472`).
- **Sombra:** projeções de fio e `exige_admin`, além da assinatura, mudam; `sem_porta`/`sem_fio` permanecem (`chamados/management/commands/diff_e3_sombra.py:57`, `:62`, `:63`, `:67`, `:75`). C17 e candidata usam o mesmo juiz; diferença nova entre lados é [nao verificado] (`:144`, `:145`).

Atenção: fio admin residual atualmente publica `acao='validar'` pela expressão existente, antes `None` (`chamados/services/tarefa_do_dia.py:693`). Comentário contraditório em `:669` precisa acompanhar a cura.

`LEI-AKITA: padrao=Single Source of Truth / Aggregate Root; Clock Injection; CQRS com projecoes reconstruiveis` — vocabulário em `docs/GLOSSARIO.md:11`, `:19`, `:24`; exigência em `docs/PLANO-PADROES.md:53`.

## 15/11 13:28 · sessao 071afeef32a4 · E4C-F1V3 · construtor · origem=Code

Aplicada a tipagem explícita, sem alterar os predicados: [juizes.py:1652](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/juizes.py:1652), [juizes.py:1674](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/juizes.py:1674). [nao verificado]

- Censo completo: **2 Coalesce, 4 Subquery, 5 Value, 0 Case**, em 89 arquivos Python examinados; todas as 11 expressões receberam tipo. [resultados:30](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:30)
- Tipos: `ato_texto → TextField`; `ato → CharField(max_length=16)`; PK → `BigAutoField`. [models.py:4015](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/models.py:4015), [apps.py:5](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/apps.py:5)
- Doubles adaptados para aceitar `output_field`, sem mudar asserções. [teste:460](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e4c_f1_juizes_le_o_ato.py:460), [teste SQL:88](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e4c_f1v4_sql.py:88) [nao verificado]

O `test_o261` tem causa independente, por leitura: a fixture omite `respondida_em`; a conservação gera somente validação, que não fornece texto de resposta. [fixture:357](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_o261_vigia_fio.py:357), [models.py:4074](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/models.py:4074), [leitura:50](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/services/atos_pergunta.py:50)
O `except` existente engole o erro de parsing do texto vazio e devolve slot ausente: sem plantio resulta `slot_nao_derivavel`; com plantio ligado resulta `com_registro`. Esse defeito permanece nesta volta. [juizes.py:459](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/juizes.py:459), [juizes.py:862](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/juizes.py:862), [classificador:106](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/services/vigia_carimbo.py:106)

- RED estrutural: `PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f1v5_mutantes --red-base` → 11 falhas, rc=1. [resultados:46](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:46)
- GREEN/regressão: 49 testes, rc=0, **2 skips ORM**; comando completo registrado. [resultados:157](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:157) [nao verificado]
- Mutantes: `PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4c_f1v5_mutantes --todos` → 22 detectados; comandos individuais registrados. [resultados:166](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:166)
- Ruff com `ruff.toml` e `py_compile` em memória: rc=0; comandos registrados. [resultados:249](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:249)
- **FieldError real ainda não reproduzido aqui:** Django ORM indisponível. Os testes de `str(qs.query)`, com conexão/cursor proibidos, ficaram para Code/suite. [teste:110](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/test_e4c_f1v5_tipos_sql.py:110), [resultados:169](/home/ronald/wt-codex-e4c-f1v3-112729/app/chamados/tests/e4c_f1v5_resultados.txt:169) [nao verificado]

LEI-AKITA: origem=`chamados/juizes.py:1636` (face SQL da conservação); testemunha=`DecisaoPergunta` via `atos_pergunta.leitura:22`; RED=`test_e4c_f1v5_tipos_sql.py:42`; quem-mais-lê=`raiox.py:135`, `fio.py:428`, `juizes.py:1708`, `cadastro_realidade.py:460`; juízes novos=0; padrão=Parallel Change / Expand-Contract (`juizes.py:318`). Certificação permanece com Code/suite e opus final.

## 15/11 13:21 · sessao 22609382723a · E3-RESTO-R3 · verificador · origem=Code

Os casos abaixo são demonstráveis pelo código; sua ocorrência e quantidade em produção são **[nao verificado]**. Atribuição histórica além do diff fornecido também é **[nao verificado]**.

1. **Campos, escritores, leitores e significado**

   **Alta — a porta de Resolver dia perde a data que justificou sua publicação.** A carga consulta datas específicas, mas publica a ação sem transportar o dia até o link. A Folha conserva apenas `acao/ausencia/rota`, e `portas_da_linha` monta a URL somente com o colaborador. Caso concreto: pendência de setembro consultada em novembro publica `admin/porta_acao`, mas o clique abre novembro, porque o calendário sem parâmetros usa o mês atual. A porta publicada não entrega diretamente o dia que precisa ser resolvido. Referências: `app/chamados/services/tarefa_do_dia.py:609`, `app/folha/fora_da_folha.py:210`, `app/folha/retrato_folha.py:1042`, `app/colaboradores/services/calendario.py:194`, `app/colaboradores/services/calendario.py:239`.

   **Média — `dias_sem_porta` é gravado e fica sem leitor de produção.** Caso concreto: pessoa com um dia `admin` e outro `nada` passa ao grupo A; o dia sem porta é preservado no JSON, mas o drawer mostra todas as datas com a mesma apresentação, sem identificar aquela pendência. A mudança de agrupamento retira essa pessoa do balde sem ação, e a marca necessária para distinguir seus dias não chega à tela. Referências: `app/folha/fora_da_folha.py:102`, `app/folha/fora_da_folha.py:207`, `app/folha/retrato_folha.py:1225`, `app/folha/retrato_folha.py:1098`, `app/templates/folha/_retrato_o_que_fazer.html:42`.

   Censo dos contratos afetados:

   | Campos/valores | Quem escreve ou fornece | Quem lê / significado |
   |---|---|---|
   | `CelulaDia.ata`, `lampadas`, `veredito`, `veredito_em`, `veredito_via`, `protestos` | Porta da célula: `app/ponto/portas/celula.py:1032`, `app/ponto/portas/celula.py:1091` | Leitor da ata: `app/escala/services/leitor_celula.py:114`; tarefa: `app/chamados/services/tarefa_do_dia.py:282`; retenção: `app/folha/export.py:1176`. |
   | Pergunta: `data_evento`, `celula`, `motivo`, `instancia_marco`, `conversa`, `via_resolucao` | Nascimento: `app/chamados/models.py:3100`; rechaveamento: `app/chamados/services/fio.py:1018` | Carga: `app/chamados/services/tarefa_do_dia.py:404`; identidade: `app/chamados/services/tarefa_do_dia.py:242`. `data_evento` significa dia do turno, não dia civil. |
   | `chamado_destino`, `disputa.chamado`, `status_local` | Costura: `app/chamados/services/fio.py:964`; estado do chamado: `app/chamados/models.py:228` | Âncora: `app/chamados/services/fio.py:116`; terminais na carga: `app/chamados/services/tarefa_do_dia.py:448`; alcance administrativo: `app/chamados/services/tarefa_do_dia.py:652`. |
   | Decisão: `ato`, `ato_em`, `ato_texto`, `absorcao_ausencia` | Porta append-only: `app/chamados/models.py:4086` | Carga ordena e corta por `ato_em <= agora`: `app/chamados/services/tarefa_do_dia.py:419`; julgamento: `app/chamados/services/tarefa_do_dia.py:156`. |
   | `resposta_colab`, `respondida_em`, `validada_em`, projeção sem linha | Resposta: `app/chamados/services/disputa_emissao.py:1439`; validação: `app/chamados/services/validacao.py:398` | Passivo E1: `app/chamados/services/tarefa_do_dia.py:428`; partição do app: `app/chamados/services/tarefa_do_dia.py:528`. |
   | `veredito_lavrado`, `julgado_em`, `janela_expira_em`; efeito com `pergunta_origem` | Lavra da pergunta: `app/chamados/models.py:3233` | Carga lê vigência e efeito no horizonte: `app/chamados/services/tarefa_do_dia.py:430`, `app/chamados/services/tarefa_do_dia.py:440`. |
   | Ausência: `pk`, `status`, cobertura | Porta de decisão: `app/ponto/services/ausencia.py:963` | Tarefa: `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/services/tarefa_do_dia.py:307`; link por ID da ausência: `app/folha/retrato_folha.py:1042`. |
   | `agora`, `hoje`, `mes/ano`, `[ini,fim]` | Commands capturam o instante: `app/chamados/management/commands/vigiar_fio_morto.py:171`, `app/ponto/management/commands/pautas_discordantes.py:118` | Tarefa exige instante: `app/chamados/services/tarefa_do_dia.py:720`; janela inclusiva: `app/chamados/services/tarefa_do_dia.py:455`; competência pelo juiz: `app/ponto/services/pautas_dos_discordantes.py:77`. As exceções de relógio estão no item 4. |
   | `fila_ids`, `fila_cel`, `fila_dia`, `verbo`; `ancoras_terminais`, `_tarefas`, `_resolver_lidos` | Alimentação: `app/chamados/juizes.py:1562`; memos: `app/chamados/services/tarefa_do_dia.py:449`, `app/chamados/services/tarefa_do_dia.py:595` | Publicador da tarefa: `app/chamados/services/tarefa_do_dia.py:636`; os memos são por carga, sem persistência. |
   | `instancias`, `acoes`, `de`, `porta`, `razao`, `nao_sei` | Corpo da tarefa: `app/chamados/services/tarefa_do_dia.py:206`, `app/chamados/services/tarefa_do_dia.py:282` | Publicador: `app/chamados/services/tarefa_do_dia.py:635`; projeção: `app/chamados/services/tarefa_do_dia.py:708`. Desconhecimento pode coexistir com rota. |
   | `fio`, `rota`, `fila`, `verbos`, `acao`, `ausencia` | Publicador: `app/chamados/services/tarefa_do_dia.py:696` | Folha: `app/folha/fora_da_folha.py:203`; vigia: `app/chamados/services/vigia_fio.py:593`; furo: `app/chamados/services/furo_sem_canal.py:97`; C11: `app/escala/services/cadastro_realidade.py:472`; sombra: `app/chamados/management/commands/diff_e3_sombra.py:47`. |
   | `grupo`, `por_fio`, `datas`, `dias`, `acoes`, `dias_sem_leitura`, `dias_sem_porta` | Folha: `app/folha/fora_da_folha.py:215`, `app/folha/fora_da_folha.py:218` | Retrato copia: `app/folha/retrato_folha.py:1217`; persistência: `app/folha/management/commands/lavrar_retrato_folha.py:78`. `grupo` e baldes contam pessoas; listas identificam dias. |
   | `dono`, `tipo`, `saldo`, `informativo`; cores e rótulos | Montagem: `app/folha/retrato_folha.py:1228`; tabelas: `app/folha/retrato_folha.py:190`, `app/folha/retrato_folha.py:309` | Leitura da UI: `app/folha/retrato_folha.py:438`, `app/folha/retrato_folha.py:468`; drawer: `app/colaboradores/views.py:986`; template lê `it.numero`: `app/templates/core/_barra_gestao.html:67`. |
   | `sem_leitura`, `retidas_sem_leitura`, chaves/texto de Pauta | Coleta: `app/ponto/services/pautas_dos_discordantes.py:90`; sincronização: `app/ponto/services/pautas_dos_discordantes.py:191` | Command: `app/ponto/management/commands/pautas_discordantes.py:132`; retenção funciona por colab/competência, não por dia: `app/ponto/services/pautas_dos_discordantes.py:195`. |
   | `com_porta_*`, `nao_sei`, `total`, `universo`; `alcance_*`, linha de nove posições | Furo: `app/chamados/services/furo_sem_canal.py:107`; alcance: `app/chamados/services/vigia_fio.py:580` | Snapshot: `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`; impressão: `app/chamados/services/vigia_fio.py:664`; backfill lê índice 4: `app/chamados/management/commands/backfill_fio_morto_e4b.py:79`. |
   | `calados`, `roteados`, `dias_sem_leitura_sistema`, `cego` | `app/folha/vigia_retido_calado.py:50` | Impressão: `app/folha/vigia_retido_calado.py:78`; alarme lê só parte deles: `app/folha/management/commands/vigiar_retido_calado.py:37`. |

2. **Juiz duplicado / regra paralela**

   **Alta — LU-01 não foi implementada.** O corte determina residual `decidir` com dono `admin`, e o registro da R3 inclui LU-01..07. Contudo, o mapa compartilhado só reconhece `cobrar/validar`; `decidir` cai em `chamado`, resultado incorporado pela tarefa. Caso concreto: nenhuma ação de instância e fila `{101: 'decidir'}` produz `fio='chamado'`, `rota='porta_fila'`, `acao=None`. Referências: `app/docs/CORTES.md:433`, `app/docs/BACKLOG.md:768`, `app/chamados/juizes.py:1653`, `app/chamados/juizes.py:1664`, `app/chamados/services/tarefa_do_dia.py:665`.

   Nenhuma lista literal nova de terminais encontrada na cura c7: ela importa `TERMINAIS` e lê `status_local`, como a costura. Referências: `app/chamados/services/tarefa_do_dia.py:447`, `app/chamados/services/fio.py:794`, `app/chamados/services/fio.py:809`.

3. **Escritores, portas, idempotência e dry**

   **Alta — `pautas_discordantes --apply` fecha pautas fora do recorte apurado.** A apuração recebe competência/empresa, mas a sincronização carrega todas as pautas vivas `[DISCORDANTE]` e fecha `vivas - alvo`. Caso concreto: apurar competência 10/empresa 1 fecha pauta da competência 9/empresa 2 sem consultá-la. O novo `sem_leitura` protege somente pares conhecidos pela rodada. É falha preexistente preservada, inclusive reconhecida na entrega c6. Referências: `app/ponto/management/commands/pautas_discordantes.py:49`, `app/ponto/services/pautas_dos_discordantes.py:186`, `app/ponto/services/pautas_dos_discordantes.py:194`, `app/ponto/services/pautas_dos_discordantes.py:208`, `app/chamados/tests/e3_r3_c6_entrega.txt:103`.

   | Command | Escrita / porta / repetição |
   |---|---|
   | `lavrar_furo_sem_canal` | Dry por default; apply usa `MetricaSnapshot.update_or_create` por chave/escopo/dia, sem duplicar snapshot: `app/chamados/management/commands/lavrar_furo_sem_canal.py:42`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`. |
   | `reatar_beco_folga_ao_limbo` | Dry por default; apply exige reversão; costura por `reapontar_resolvedora`; já reatada não escreve novamente: `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:39`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:90`, `app/chamados/services/limbo_folgas.py:242`, `app/chamados/services/limbo_folgas.py:245`. |
   | `pautas_discordantes` | Dry por default; escreve/fecha pelas portas de Pauta. A comparação de chaves evita repetição sequencial, mas mantém o erro de recorte acima: `app/ponto/services/pautas_dos_discordantes.py:193`, `app/ponto/services/pautas_dos_discordantes.py:202`, `app/ponto/services/pautas_dos_discordantes.py:208`. |
   | `diff_e3_sombra` | Bloqueia SQL de escrita, inclusive dependências, e apenas lê o usuário-sistema: `app/chamados/management/commands/diff_e3_sombra.py:30`, `app/chamados/management/commands/diff_e3_sombra.py:113`. |
   | `vigiar_fio_morto` | Escreve carimbo em disco na rodada de frota, mesmo sem flag; não possui dry de filesystem: `app/chamados/management/commands/vigiar_fio_morto.py:234`, `app/chamados/management/commands/vigiar_fio_morto.py:240`, `app/chamados/management/commands/vigiar_fio_morto.py:140`. |
   | `vigiar_retido_calado` | O corpo imprime e retorna/alarma, sem escrita explícita: `app/folha/management/commands/vigiar_retido_calado.py:30`. |

   **Média — leitura pode criar `auth.User`.** Com usuário `sistema` ausente e competência trancada exigindo consulta de toque, a tarefa alcança `get_user_sistema()`, implementado com `get_or_create`. Isso contradiz “zero escrita no banco”, embora não constitua segundo escritor das cinco entidades nomeadas. Referências: `app/chamados/services/tarefa_do_dia.py:457`, `app/chamados/juizes.py:1228`, `app/core/utils.py:18`, `app/chamados/management/commands/vigiar_fio_morto.py:12`.

   Nenhum segundo escritor explícito de `Chamado`, `CelulaDia`, `DiaPago` ou `FechamentoMensal` foi identificado nos commands alterados; Pauta e costura usam as portas indicadas acima. Idempotência sob concorrência: **[nao verificado]**.

4. **Relógio fora da borda**

   **Média — ainda há relógio dentro de leitores.** `censo(hoje=<dia passado>)`, sem `agora`, mistura o dia informado com tarefas julgadas no instante atual. `contexto_calendario` também conserva `timezone.now()` como fallback. O command principal corrigiu sua chamada, mas os contratos dos leitores continuam permitindo essa mistura. Referências: `app/chamados/services/vigia_fio.py:279`, `app/chamados/services/vigia_fio.py:352`, `app/colaboradores/services/calendario.py:192`, `app/chamados/management/commands/vigiar_fio_morto.py:180`.

   **Média — reatador usa instantes diferentes para a mesma rodada.** Captura `agora`, mas omite-o nas duas consultas de fila. Caso concreto: `silenciado_ate` vence entre a seleção C11 e a consulta do pai; o alcance muda por outro relógio. Referências: `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:52`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`, `app/chamados/services/painel.py:190`.

   A Folha também continua lendo o relógio durante apresentação em `_hoje_iso`; `o_que_fazer(hoje=...)` não repassa esse parâmetro ao cálculo do LED. Referências: `app/folha/retrato_folha.py:465`, `app/folha/retrato_folha.py:1093`, `app/folha/retrato_folha.py:1097`.

5. **Crons e papel declarado**

   **Média — o contador novo de desconhecimento não participa de nenhum alarme.** Caso concreto: `cego=0`, `calados=0`, `dias_sem_leitura_sistema=3`, com `--alarme --alarme-passivo`, termina sem alarme. As listas contêm somente `cego` e `calados`; o comentário do cron promete ativação do próprio número pelo passivo, mas isso nunca alcança o contador novo. Referências: `app/folha/vigia_retido_calado.py:82`, `app/folha/management/commands/vigiar_retido_calado.py:16`, `app/folha/management/commands/vigiar_retido_calado.py:37`, `app/config/crons.py:1679`.

   Os papéis registrados são coerentes com a operação principal: vigias em `vigia`, snapshot em `lavra`. Nenhuma nova decisão própria de prazo identificada nessas entradas. Referências: `app/config/crons.py:2236`, `app/config/crons.py:2262`, `app/config/crons.py:2291`.

6. **Registro e frase no CORTES**

   **Alta — o anexo e o CORTES da cópia divergem.** As frases de nascimento de `tarefa_do_dia` e `dia_da_pergunta_trancado` existem no anexo fornecido, mas não foram encontradas no `app/docs/CORTES.md` local. Ambos estão registrados e ficam fora da base anterior à trava; a extração da trava exige a frase literal. Portanto, a árvore fornecida não satisfaz essa verificação documental. Referências: `app/core/juizes.py:299`, `app/core/juizes.py:300`, `bin/tests/test_juiz_novo_tem_corte.sh:28`, `bin/tests/test_juiz_novo_tem_corte.sh:37`, `bin/tests/test_juiz_novo_tem_corte.sh:44`.

   O delta fornecido de `core/juizes.py` não acrescenta autoridade: reancora uma impressão pendente. Atribuir a ausência das frases à c10 é **[nao verificado]**; não estou afirmando ausência de autorização, pois ela consta do anexo. Referência da alteração: `app/core/juizes.py:878`.

7. **Testes e mutantes c7–c10** [nao verificado]

   **Alta — há teste verde que fixa resultado contrário à LU-01.** `test_C6_sem_devida_residual_decidir` exige `chamado`, enquanto o corte exige `admin`. Esse selo conserva a cura omitida. Referências: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:129`, `app/docs/CORTES.md:433`.

   Não identifiquei enfraquecimento na correção textual c10 do E1: a primeira rodada continua exigindo `segue_sem_porta` e uma decisão; a segunda exige `sem_porta_de_sistema` e mantém exatamente uma linha. Isso corresponde ao prefixo emitido quando não há ato executável. Referências: `app/chamados/tests/test_e4_passivo.py:65`, `app/chamados/tests/test_e4_passivo.py:69`, `app/chamados/tests/test_e4_passivo.py:72`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:263`.

   Foram executados 81 testes sem banco e os nove mutantes c7; os testes passaram e os mutantes morreram por asserção **[nao verificado por artefato persistido]**. Os casos não terminal/terminal permanecem separados no teste. Referências: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:540`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:544`.

   **Prova incompleta:** morte do mutante ORM `item02_E1_terminal` após c10 e suite inteira do hash permanecem **[nao verificado]**. O mutante está definido em `app/chamados/tests/e3r3_c9_mutantes.py:27`.

8. **`_barra_gestao.html` e duas cascas**

   Nenhum derivador numérico no delta: o template exibe `it.numero`; a alteração acrescenta somente o ramo de cor `petroleo`. Referência: `app/templates/core/_barra_gestao.html:67`.

   **Média — exceção de cor nova sem corte específico demonstrado.** O código admite que `#0f766e` é cor nova e chama a exceção de decisão da triagem c6, apesar da proibição registrada no próprio bloco. A entrega reconhece a necessidade de corte se essa proibição prevalecer. Autorização da exceção: **[nao verificado]**. Referências: `app/folha/retrato_folha.py:300`, `app/folha/retrato_folha.py:303`, `app/chamados/tests/e3_r3_c6_entrega.txt:108`.

   Paridade visual das duas cascas por renderização/estilo calculado: **[nao verificado]**. O teste novo examina texto do template, não estilo calculado: `app/chamados/tests/test_e3_r3_c6_sem_banco.py:376`.

9. **Docstrings e comentários incorretos**

   - **Baixa:** `furo_sem_canal` promete porta para chamado vivo da fila mesmo sem ação; residual `cobrar` é excluído pela tarefa. Referências: `app/chamados/services/furo_sem_canal.py:19`, `app/chamados/services/tarefa_do_dia.py:663`.
   - **Baixa:** `lavrar_furo_sem_canal` se descreve como VIGIA com papel declarado, mas está registrado como `lavra` e grava snapshot. Referências: `app/chamados/management/commands/lavrar_furo_sem_canal.py:6`, `app/config/crons.py:2291`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`.
   - **Baixa:** a Folha ainda promete queries constantes em N, enquanto a tarefa agora declara consulta plural adicional por colaborador sem porta. Referências: `app/folha/fora_da_folha.py:32`, `app/chamados/services/tarefa_do_dia.py:22`.
   - **Média:** o novo caminho de ausência abre formulário cuja edição chama `salvarAusencia`, definida apenas na página Ausências. O drawer da Folha não carrega essa definição; aprovar/rejeitar continuam por POST nativo. Referências: `app/folha/retrato_folha.py:1014`, `app/templates/folha/_retrato_o_que_fazer.html:73`, `app/templates/ponto/partials/painel_ausencia.html:148`, `app/templates/ponto/ausencias.html:234`, `app/templates/ponto/partials/painel_ausencia.html:323`.

10. **Travas e pré-requisitos de pouso** [nao verificado]

   Nenhuma dependência técnica ausente de COMPETENCIA-PAGA, BUG-PAR ou E4c foi demonstrada pelo código examinado; comprovação de pouso dessas obras em produção é **[nao verificado]**. O registro ainda declara o apply do reatador em produção bloqueado até corte de desenho. Referência: `app/config/crons.py:820`.

   A entrega c6 exige medição anterior ao pouso para a ampliação M1 do C11. Essa medição no hash final não consta do dado fornecido: **[nao verificado]**. Referência: `app/chamados/tests/e3_r3_c6_entrega.txt:86`.

**VEREDITO: REPROVA.**

## 15/11 13:21 · sessao 157e344c5f6f · - · - · origem=Ronald (tmux)

Censo completo do diff, validado por AST em 89 arquivos Python alterados: 11 construtores, todos em `chamados/juizes.py`:

- `Subquery`: 4 — `:1652`, `:1657` (PK), `:1674` (`ato_texto`), `:1675` (`ato`).
- `Coalesce`: 2 — `:1674`, `:1675`.
- `Value`: 5 — `:1674`, `:1675` (vazios), `:1676` (`RESPOSTA`), `:1678`, `:1680` (regex).
- `Case`: 0 no diff examinado.

Tipos de origem: `ato_texto = TextField` em `chamados/models.py:4018`; `ato = CharField(max_length=16)` em `chamados/models.py:4015`; PK implícita `BigAutoField` por `chamados/apps.py:5` e `config/settings/base.py:104`.

Atenção aos doubles existentes: `Value`, `Subquery` e `Coalesce` não aceitam kwargs em `chamados/tests/test_e4c_f1v4_sql.py:88`; há equivalentes em `chamados/tests/test_e4c_f1_juizes_le_o_ato.py` [linha não verificada]. Adaptar assinaturas sem alterar asserções.

## 15/11 13:20 · sessao 03e711464610 · - · - · origem=Ronald (tmux)

Causa independente do `FieldError`, por leitura do código:

- `_porta` preenche texto e validação, mas omite `respondida_em`, cujo padrão é NULL (`chamados/tests/test_o261_vigia_fio.py:357`; `chamados/tests/test_o261_vigia_fio.py:364`; `chamados/models.py:2526`).
- A conservação só gera RESPOSTA com `respondida_em`; sem isso gera apenas VALIDAÇÃO (`chamados/models.py:4074`; `chamados/models.py:4078`).
- `leitura` inicia resposta vazia; VALIDAÇÃO não atribui texto (`chamados/services/atos_pergunta.py:33`; `chamados/services/atos_pergunta.py:50`).
- `_slot_esperado` passa a ler esse texto vazio; `date.fromisoformat('')` falha e o `except Exception` existente devolve `(None,None)` (`chamados/juizes.py:446`; `chamados/juizes.py:459`; `chamados/juizes.py:466`).
- Sem batida, a classe torna-se `slot_nao_derivavel` (`chamados/services/vigia_carimbo.py:110`).
- Com batida ligada, slot ausente produz absolvição por existência da batida (`chamados/juizes.py:862`), portanto `com_registro` (`chamados/services/vigia_carimbo.py:106`).

O caminho explica exatamente as duas classes informadas, sem depender da face SQL problemática. O `except` captura falha de parsing, não o `FieldError`, nesse percurso (`chamados/juizes.py:460`). Na base, o derivador lia diretamente `p.resposta_colab`; a troca para `atos.resposta` está no diff `77dfa94fa..97bbaa604`, hunk de `_slot_esperado` [linha histórica: `77dfa94fa:app/chamados/juizes.py:363`].

Nenhum arquivo ou asserção alterado. Reprodução executada dos subtestes com banco: [não verificado].

## 15/11 13:18 · sessao 3336f98fe395 · - · - · origem=Ronald (tmux)

Achados:

- **Alta — LU-01 ficou sem cura e o teste certifica comportamento contrário à lei.** O corte manda residual `decidir` ter dono `admin` (`app/docs/CORTES.md:433`), e R3 inclui LU-01..07 (`app/docs/BACKLOG.md:768`). Contudo, `fio_pelos_verbos` devolve `chamado` para `decidir` (`app/chamados/juizes.py:1653`, `:1664`); a tarefa conserva isso e escreve “LU-01 fora” (`app/chamados/services/tarefa_do_dia.py:665`, `:669`). Reproduzi com corpos reais por AST: fila `{101: 'decidir'}`, nenhuma ação → `fio='chamado'`, `rota='porta_fila'`, `acao=None`. O teste exige esse resultado errado (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:129`). Nenhum corte posterior de exclusão localizado nos documentos consultados [nao verificado como censo universal].

- **Alta — frases de nascimento presentes no anexo estão ausentes do CORTES da árvore.** `tarefa_do_dia` e `dia_da_pergunta_trancado` estão registrados (`app/core/juizes.py:299`, `:300`), mas suas frases não aparecem no arquivo local; o anexo fornecido afirma restauração. Reproduzi em memória a extração da trava: esses dois e outros oito registros ficam sem frase. Pela implementação, isso dá falha (`bin/tests/test_juiz_novo_tem_corte.sh:37`, `:44`). Não executei o script, que escreve temporário/base (`:52`, `:57`). Atribuição ao delta c10 [nao verificado].

- **Média — exceção de cor nova sem corte demonstrado.** A própria produção registra a proibição e admite ter criado exceção por “triagem c6” (`app/folha/retrato_folha.py:300`, `:303`, `:306`); a nova cor entra no partial (`app/templates/core/_barra_gestao.html:67`). A entrega admite que depende de corte se a proibição prevalecer (`app/chamados/tests/e3_r3_c6_entrega.txt:108`). Autorização específica [nao verificado].

- **Baixa — comentário promete teste inexistente.** A referência ao teste Django `folha/tests/test_e3_r3_c6_cor_do_balde.py` está em `app/chamados/tests/test_e3_r3_c6_sem_banco.py:377`; o arquivo não existe na cópia [nao verificado por arquivo:linha inexistente]. A entrega confirma que a renderização não foi feita (`app/chamados/tests/e3_r3_c6_entrega.txt:99`).

Verificação dos testes: [nao verificado]

- Executei **81 testes sem banco: OK** [nao verificado por artefato persistido; execução nesta sessão]. M2 positivo e M2b terminal permanecem distintos (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:540`, `:544`).
- Executei os **nove mutantes c7: todos mortos por asserção**, sem escrever fontes [nao verificado por artefato persistido; execução nesta sessão]. Os alvos e critérios estão em `app/chamados/tests/e3_r3_c7_mutantes.py:14`, `:60`.
- A troca c10 de texto da segunda rodada é coerente com o command: ato não executável publica o prefixo (`app/chamados/management/commands/backfill_fio_morto_e4b.py:263`), agora exigido pelo teste (`app/chamados/tests/test_e4_passivo.py:69`). Não identifiquei enfraquecimento desse caso.
- O mutante ORM c9 item02 está definido (`app/chamados/tests/e3r3_c9_mutantes.py:27`), mas sua morte após c10 permanece **[nao verificado]**, conforme o anexo.
- O partial lê `it.numero`, sem derivação (`app/templates/core/_barra_gestao.html:67`). Quebra das duas cascas por execução visual **[nao verificado]**; o ramo adicionado só seleciona `petroleo` nessa linha.

## 15/11 13:18 · sessao c0a4f2aee8c3 · - · - · origem=Ronald (tmux)

Achados da revisão de commands/crons, somente leitura:

1. **Média — contador novo não participa do alarme.** `dias_sem_leitura_sistema` é publicado com esperado 0 (`app/folha/vigia_retido_calado.py:82`), mas `ALARMA_SEMPRE` contém somente `cego` e `ALARMA_EM_0` somente `calados` (`app/folha/management/commands/vigiar_retido_calado.py:16`). Caso concreto: `cego=0`, `calados=0`, `dias_sem_leitura_sistema=3`, com ambas as flags, retorna normalmente porque só consulta aquelas listas (`app/folha/management/commands/vigiar_retido_calado.py:37`). Confirmei executando o `handle` extraído por AST, com dependências simuladas, sem banco/escrita. O comentário do cron promete ativação pelo `--alarme-passivo`, que nunca alcançará esse contador (`app/config/crons.py:1679`).

2. **Média — editar ausência quebra no novo caminho pela Folha.** A ação nova aponta ao partial de ausência (`app/folha/retrato_folha.py:1014`), aberto no drawer genérico (`app/templates/folha/_retrato_o_que_fazer.html:73`). O formulário chama `salvarAusencia` e não declara `method` (`app/templates/ponto/partials/painel_ausencia.html:148`); a única definição está na página Ausências (`app/templates/ponto/ausencias.html:234`). A edição só grava com POST (`app/ponto/views.py:2804`); portanto o novo caminho mostra um formulário cuja edição não funciona. Aprovar/rejeitar usam forms POST nativos e continuam alcançáveis (`app/templates/ponto/partials/painel_ausencia.html:323`, `app/templates/ponto/partials/painel_ausencia.html:344`).

3. **Alta, preexistente preservada — sincronização fecha pautas fora do recorte.** A apuração é limitada por competência/empresa (`app/ponto/management/commands/pautas_discordantes.py:49`, `app/ponto/services/pautas_dos_discordantes.py:77`), mas `sincronizar` lê todas as pautas vivas `[DISCORDANTE]` (`app/ponto/services/pautas_dos_discordantes.py:186`) e fecha as ausentes do alvo (`app/ponto/services/pautas_dos_discordantes.py:194`, `app/ponto/services/pautas_dos_discordantes.py:208`). Caso concreto: executar competência 10/empresa 1 fecha pauta viva da competência 9/empresa 2, sem apurá-la. O delta mantém a falha; não atribuir sua origem à c10.

4. **Baixa — papel documentado divergente.** O command se declara VIGIA com papel registrado (`app/chamados/management/commands/lavrar_furo_sem_canal.py:6`), mas o registro é `lavra` (`app/config/crons.py:2265`, `app/config/crons.py:2291`), coerente com sua escrita em `MetricaSnapshot` (`app/chamados/management/commands/lavrar_furo_sem_canal.py:50`).

5. **Baixa — promessa antiga contradiz a nova porta.** O serviço afirma que chamado vivo da fila conserva porta mesmo sem ação (`app/chamados/services/furo_sem_canal.py:19`), mas a tarefa exclui residual `cobrar` da atribuição de dono (`app/chamados/services/tarefa_do_dia.py:665`).

6. **Relógio interno permanece:** `censo` ainda aceita omissão de `agora` e lê `timezone.now()` dentro do leitor (`app/chamados/services/vigia_fio.py:279`). O command alterado passa o instante corretamente (`app/chamados/management/commands/vigiar_fio_morto.py:180`). O reatador captura um instante (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:52`), mas suas duas consultas de fila omitem esse parâmetro (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:113`); cada uma lê outro relógio (`app/chamados/services/painel.py:190`). Caso: `silenciado_ate` vence entre seleção C11 e consulta da fila.

Portas e efeitos conferidos:

- `lavrar_furo_sem_canal`: dry por default; com `--apply`, atualiza/cria snapshot por chave+escopo+dia (`app/chamados/management/commands/lavrar_furo_sem_canal.py:42`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`).
- `reatar_beco_folga_ao_limbo`: dry por default; exige arquivo de reversão no apply; escreve pela porta `reapontar_resolvedora` (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:39`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:90`, `app/chamados/services/limbo_folgas.py:245`). Já reatada, não escreve novamente (`app/chamados/services/limbo_folgas.py:242`).
- `pautas_discordantes`: dry por default; escreve/fecha pelas portas de Pauta (`app/ponto/management/commands/pautas_discordantes.py:54`, `app/ponto/services/pautas_dos_discordantes.py:198`, `app/ponto/services/pautas_dos_discordantes.py:202`, `app/ponto/services/pautas_dos_discordantes.py:208`).
- `diff_e3_sombra`: bloqueio SQL de escrita e usuário-sistema somente lido (`app/chamados/management/commands/diff_e3_sombra.py:30`, `app/chamados/management/commands/diff_e3_sombra.py:113`).
- `vigiar_fio_morto`: sem flag escreve carimbo em disco na rodada de frota; portanto não tem dry de filesystem (`app/chamados/management/commands/vigiar_fio_morto.py:234`, `app/chamados/management/commands/vigiar_fio_morto.py:240`, `app/chamados/management/commands/vigiar_fio_morto.py:140`).
- Há escrita indireta potencial de `auth.User` nos leitores: tarefa consulta `dias_trancados` (`app/chamados/services/tarefa_do_dia.py:457`), que pode chamar `get_user_sistema` (`app/chamados/juizes.py:1228`), implementado com `get_or_create` (`app/core/utils.py:18`). Caso concreto: sistema ausente e competência trancada com toque a verificar. Contradiz “zero escrita no banco” do vigia (`app/chamados/management/commands/vigiar_fio_morto.py:12`); não é segundo escritor das cinco entidades nomeadas pelo usuário.
- Nenhuma dependência concreta ausente de BUG-PAR/COMPETENCIA-PAGA/E4c demonstrada nesta fatia [nao verificado]. O próprio registro mantém apply em produção do reatador bloqueado por desenho (`app/config/crons.py:823`).

_5652 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
