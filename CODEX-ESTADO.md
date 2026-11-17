# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 15:29** · publicado em **17/11/2026 15:30** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 15:29 · sessao 0365105362ad · E4C-F2 · verificador · origem=Code

**BLOQUEIA o P1 pelo inventário pessoal:** a coluna `resposta_mantem_validacao` aciona um selo existente e não foi inventariada (`app/chamados/models.py:4064`; `bin/sombra_regras_pessoais.py:163`; `bin/inventario_pessoal_dos_modelos.py:78`). Os demais achados estão discriminados abaixo.

1. **BLOQUEIA — coluna nova derruba o selo de inventário.**

   O padrão casa qualquer nome contendo `resposta`; a cobertura de `DecisaoPergunta` contém `ato_texto`/`ato_motivo`, e a isenção existente cobre `pergunta_id`, sem a coluna nova (`bin/sombra_regras_pessoais.py:142`; `:163`; `:233`). `chamados` pertence ao universo inspecionado (`app/config/settings/saas.py:38`).

   O inventário percorre `concrete_fields`, acusa campos descobertos e sai com erro; o selo de host exige seu resultado OK (`bin/inventario_pessoal_dos_modelos.py:73`; `:78`; `:87`; `bin/tests/test_inventario_pessoal_no_commit.sh:31`). O selo da sombra também recusará a coluna após ela aparecer no catálogo (`bin/sombra_selo_pessoal.py:30`; `:34`; `:94`; `bin/sombra.sh:275`).

   **Cura necessária:** declarar a coluna em `ISENTOS`, com o motivo de ser booleano de estado; essa é a correção indicada pelo próprio inventário (`bin/inventario_pessoal_dos_modelos.py:91`). Execução integral desses selos: **[não verificado]**.

2. **NAO PROCEDE — escritor de RESPOSTA omitido ou valor declarado divergente do padrão de motivo.**

   Os quatro montadores encontrados declaram o campo:

   | Montador | Valor | Correspondência com `ato_motivo` |
   |---|---:|---|
   | Porta humana | `False` | Motivo omitido, cujo default é vazio (`app/chamados/services/disputa_emissao.py:1525`; `:1527`; `app/chamados/models.py:4043`). |
   | Resposta automática | `True` | Motivo constante preenchido (`app/chamados/services/disputa_emissao.py:1679`; `:1680`; `:1681`). |
   | Carimbo pela trilha | `True` | Prefixo preenchido, mesmo quando o argumento `motivo` for vazio (`app/chamados/services/disputa_emissao.py:1701`; `:1702`; `:1703`). |
   | Foto da projeção sem linha | `True` | Motivo constante preenchido (`app/chamados/models.py:4106`; `:4128`; `:4129`; `:4130`). |

   Todos desembocam em `registrar`, cujo `bulk_create` materializa os dicionários (`app/chamados/models.py:4139`; `:4149`). A montagem com `ato` variável está limitada a REABERTURA/RECUSA pela guarda de `ATOS_COM_FOTO`; não constitui quinto escritor de RESPOSTA (`app/chamados/services/disputa_emissao.py:1620`; `:1633`; `app/chamados/models.py:4036`).

   Nas migrações anteriores, a transformação do acervo atribui REABERTURA; o `RunPython` correspondente atualiza chaves de `PerguntaDisputa`, sem criar RESPOSTA (`app/chamados/migrations/0051_decisao_pergunta.py:22`; `:49`; `:61`; `:101`). `backfill_decisoes_pergunta` usa o montador para medir RESPOSTA, enquanto sua escrita própria identificada é SILENCIO (`app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; `:324`).

3. **FILA — o tripwire cobre montadores explícitos, sem fechar fluxo indireto.**

   Ele reconhece literalmente `'resposta'` ou `.RESPOSTA`, percorre apenas `app/` e exclui migrações; não acompanha `ato` variável, expansão `**kwargs`, SQL ou alterações posteriores de dicionários (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:20`; `:32`; `:48`; `:51`; `:65`).

   Estas vias existentes ficam fora da inspeção **do local chamador**, embora deleguem aos montadores cobertos:

   - Ressuscitação e nascimento com assinaturas: `app/chamados/models.py:1613`; `:3129`.
   - Preservação anterior às respostas humana/automática: `app/chamados/services/disputa_emissao.py:1480`; `:1674`.
   - Validação e devoluções: `app/chamados/services/validacao.py:167`; `:272`; `:345`.
   - Backfill efetivamente escritor: `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.
   - Command pela porta da trilha: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.
   - Entradas humanas: `app/chamados/views.py:1283`; `app/api/views.py:2627`; `app/chamados/services/acoes_disputa.py:373`; `app/chamados/services/declaracao_texto.py:419`; `:693`.
   - Entrada automática: `app/api/foto_recebida.py:48`.

   Portanto, essas vias não são escritores adicionais omitindo o campo; são pontos cegos de acompanhamento do selo, cujo reconhecimento termina na montagem explícita (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:51`; `app/chamados/models.py:4149`).

4. **NAO PROCEDE para leitor de negócio antecipado; FILA para a afirmação absoluta “ninguém lê”.**

   `da_ressuscitacao` nasce `False`; seu leitor de negócio é `es_da_ressuscitacao`, chamado pela carga. Não há escritor de produção atribuindo `True` nos montadores encontrados; o teste fabrica essa situação (`app/chamados/models.py:4058`; `:4096`; `app/chamados/services/tarefa_do_dia.py:427`; `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:79`).

   `resposta_mantem_validacao` é escrito pelos quatro montadores acima. Não encontrei julgamento de negócio usando seu valor; `_pela_decisao` continua decidindo RESPOSTA pelo ato e texto (`app/chamados/services/tarefa_do_dia.py:158`; `:159`; `:160`; `:162`). **Existem leitores genéricos**, além dos checks do banco, de modo que “nenhum leitor” precisa significar “nenhum leitor de negócio” (`app/chamados/models.py:4082`; `:4085`; `app/chamados/services/foto_e4b.py:251`).

5. **FILA — leitores genéricos mudam a representação das linhas.**

   | Caminho | Efeito |
   |---|---|
   | Foto E4B | Ambas as colunas entram em `campos`, pois enumera todos os campos concretos e inclui `DecisaoPergunta` (`app/chamados/services/foto_e4b.py:251`; `:280`). |
   | Reversão E4B | O arquivo passa a carregar essas colunas nas fotos (`app/chamados/management/commands/backfill_fio_morto_e4b.py:284`; `:287`). |
   | Comparação E4B | Compara dicionários inteiros; foto base versus foto P1 diferirá mesmo com ambos os valores `False` (`app/chamados/services/foto_e4b.py:118`; `:121`). |
   | Inventário de modelos/catalogação | A coluna de resposta muda o resultado para reprovação, conforme achado 1 (`bin/inventario_pessoal_dos_modelos.py:73`; `bin/sombra_selo_pessoal.py:30`). |
   | `describe model` | Passa a listar os dois campos e tipos (`app/core/management/commands/describe.py:55`; `:65`). |
   | `values()` no teste existente | A foto ganha as colunas, mas compara dentro da mesma execução (`app/chamados/tests/test_e1_entradas.py:279`; `:295`). |

   No command E4B real, antes/depois são produzidos no mesmo processo e transação; não foi identificado caminho que compare uma foto antiga recuperada contra uma nova após deploy (`app/chamados/management/commands/backfill_fio_morto_e4b.py:280`; `:303`; `:319`). **Golden, hash persistido ou comparação efetivamente atravessando deploy: [não verificado].**

   O helper de admin enumera `_meta.fields`, mas registro de `DecisaoPergunta` nele: **[não verificado]** (`app/core/admin.py:50`). Uso desse modelo em `model_to_dict`, serializers ou exportação adicional: **[não verificado]**. O helper de reexibição também enumera campos, mas uso com `DecisaoPergunta`: **[não verificado]** (`app/core/reexibir.py:30`).

6. **NAO PROCEDE — leitor de último ato escapando ao filtro da carga.**

   Os dois sítios encontrados que decidem pelo último ato recebem o mapa filtrado:

   | Sítio | Decisão | Caminho |
   |---|---|---|
   | `_pela_decisao` | Última REABERTURA/RECUSA devolve ao colaborador (`app/chamados/services/tarefa_do_dia.py:158`; `:163`). | Recebe `_dec` da carga (`:427`; `:430`; `:588`; `:589`). |
   | `_tarefa_pura` | Última RETRATAÇÃO exclui a pergunta (`app/chamados/services/tarefa_do_dia.py:259`; `:260`). | Mesmo mapa filtrado (`:588`; `:589`). |

   A chamada pública sem carga cria essa mesma carga; portanto não abre desvio pelo caminho avulso (`app/chamados/services/tarefa_do_dia.py:745`; `:746`).

   Os outros usos decisórios de REABERTURA/`ATOS_COM_FOTO` são o predicado da marca, a guarda do escritor e as constraints, sem escolher último ato por outra sequência (`app/chamados/models.py:4096`; `app/chamados/services/disputa_emissao.py:1620`; `app/chamados/models.py:4077`; `:4082`).

7. **FILA/P2 — o histórico lê REABERTURA por fora da carga.**

   `respostas_anteriores` percorre `self.decisoes.all()`; qualquer `ATOS_COM_FOTO` entra no histórico e limpa `vigente` (`app/chamados/models.py:3675`; `:3680`; `:3682`). Usa prefetch próprio, sem filtro da marca, ou query própria quando não há cache (`app/chamados/services/fio.py:263`; `app/chamados/models.py:3675`). O wrapper delega a esse leitor (`app/chamados/services/disputa_emissao.py:2126`).

   Uma REABERTURA marcada do P2 aparecerá como “Reaberta para correcao”, inclusive com motivo, porque o template usa o choice geral (`app/chamados/models.py:4031`; `app/templates/chamados/partials/modal_fio.html:205`; `:212`). **Mudança de saída demonstrável; incorreção semântica do histórico: [não verificado].** Ressuscitar efetivamente limpa a resposta, e o histórico declara mostrar respostas que deixaram de ser vigentes (`app/chamados/models.py:3668`; `:1629`).

   O outro leitor desse prefetch, `rotulo_bug_par_103`, reage apenas a DEVOLUCAO com prefixo próprio; a REABERTURA marcada não satisfaz essa condição (`app/chamados/services/materializacao.py:275`).

8. **NAO PROCEDE para falha das constraints no P1; FILA para valor transitório e pré-requisito do P2.**

   As colunas nascem com `default=False` e `db_default=False`. Como cada check aceita incondicionalmente o respectivo campo `False`, nenhuma linha existente será recusada **por esses dois checks** (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:19`; `:24`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:20`; `:25`).

   INSERT antigo omitindo as colunas recebe `False`; UPDATE antigo que as preserve também não viola esses checks. Reescritas normais de decisões já são recusadas pelo modelo/queryset (`app/chamados/models.py:3996`; `:4002`; `:4098`).

   **Há valor semanticamente incompleto:** resposta automática/trilha/projeção gravada por processo anterior ao P1 recebe `resposta_mantem_validacao=False`, embora pertença ao grupo que o P1 marca `True`. Isso é aceito nesta expansão e exige o backfill posterior, já previsto com a condição de não sobreviver processo anterior ao P1 (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:3`; `:4`; `:5`). Drenagem real desses processos: **[não verificado]**.

9. **FILA/P2 — preservar validação ainda não orienta a tarefa deste hash.**

   Para sequência `VALIDACAO → RESPOSTA(texto, resposta_mantem_validacao=True)`, este código escolhe a RESPOSTA e retorna `aguarda_validacao`; o filtro só remove REABERTURA marcada (`app/chamados/services/tarefa_do_dia.py:158`; `:162`; `:427`).

   Esse comportamento já existe com os escritores automáticos/sintéticos do próprio P1; não é regressão introduzida exclusivamente pelo futuro P2 (`app/chamados/services/disputa_emissao.py:1681`; `:1703`; `app/chamados/models.py:4130`). A conservação desse resultado entre leitores P1/P2 precisa ser conferida no diff P2; seu resultado final é **[não verificado]**.

   Também não confere como implementação a frase “a regra de hoje deriva de `ato_motivo`”: os valores dos quatro escritores coincidem com esse padrão, mas a tarefa não consulta motivo; a projeção temporal lê os campos da pergunta (`app/chamados/models.py:4062`; `app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/juizes.py:1389`). O leitor decisório de motivo localizado trata DEVOLUCAO BUG-PAR (`app/chamados/services/materializacao.py:275`).

10. **NAO PROCEDE para testes decorativos; FILA para limites da prova.** [nao verificado]

   - **Default:** o teste omite ambas as colunas no INSERT, exige `(False, False)` e confere o catálogo. Retirar `db_default` da migração atinge diretamente essa prova (`app/chamados/tests/test_f2_db_default_das_colunas_novas.py:25`; `:32`; `:35`).
   - **Tripwire:** exige os quatro locais/valores e contém fonte sintética com omissão, valor não constante e formas distintas de montagem. Morde sintaxe e constantes, sem provar o comportamento completo dos escritores (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:76`; `:80`; `:82`).
   - **Filtro:** recria carga, exige uma instância da pergunta e compara a razão antes/depois; a REABERTURA comum deve continuar devolvendo ao colaborador (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:73`; `:75`; `:88`; `:95`). Sem o filtro, o ramo alcançado retorna `devolvida_ao_colaborador` e reprova a asserção (`app/chamados/services/tarefa_do_dia.py:163`). **O teste compara a razão, não a tarefa inteira** (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:76`; `:89`).

   A análise em memória confirmou o censo AST e a diferença do ramo com/sem filtragem. Execução integral Django, fixture ORM e mutante integral sem filtro: **[não verificado]**.

11. **NAO PROCEDE — duplicação de juiz, novo escritor, relógio ou inversão de trava no trecho alterado.**

   O predicado novo identifica origem da linha; não responde às perguntas de C19 ou C21, cujas autoridades permanecem registradas (`app/chamados/models.py:4096`; `app/core/juizes.py:159`; `:170`; `:304`; `:306`). A escrita continua em `registrar`; a carga usa `agora` recebido, exigido pela API (`app/chamados/models.py:4149`; `app/chamados/services/tarefa_do_dia.py:429`; `:743`). Os acréscimos não introduzem aquisição de trava antes/depois da escrita existente (`app/chamados/services/disputa_emissao.py:1520`; `:1525`; `:1677`).

   Na semântica, `da_ressuscitacao` significa origem do compensador, não dono da tarefa; `resposta_mantem_validacao=True` significa que aquela RESPOSTA não revoga validação, não que existe validação vigente (`app/chamados/models.py:4056`; `:4059`). `ato`, texto, motivo e instante conservam seus papéis nos montadores; a foto sintética usa o instante original da resposta (`app/chamados/models.py:4020`; `:4128`; `app/chamados/services/disputa_emissao.py:1525`; `:1680`). A ressalva semântica transitória do estoque está no achado 8.

12. **NAO PROCEDE — contrariedade demonstrada às leis anexadas neste P1.**

   O filtro preserva a interpretação anterior da tarefa diante do compensador, sem criar outra autoridade de dono/porta; isso é compatível com L-129 e o contrato do C19 (`app/docs/LEIS.md:149`; `app/docs/CORTES.md:430`; `app/chamados/services/tarefa_do_dia.py:427`). A limpeza integral do par exigida pela L-130 continua pela porta existente (`app/docs/LEIS.md:151`; `app/chamados/models.py:1629`). As novas migrações expandem o esquema sem antecipar backfill/leitores, conforme a conservação descrita no corte F2 (`app/docs/CORTES.md:488`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:1`).

   Cumprimento operacional dos pré-requisitos de pouso, execução de `migrate`, defaults reais e estado dos processos: **[não verificado]**. [nao verificado]

## 17/11 15:29 · sessao 5e3d30c858e3 · FIDELIDADE-BATIDA-X-ESPELHO · sonda · origem=Code

Os números abaixo são transcritos da saída fornecida; a confirmação deles em arquivo local está **[não verificado]**. Nas tabelas, **C/P = contagem de batidas / pessoas distintas**.

| Superfície | Classe | Total C/P | Empresa 2 C/P | Empresa 3 C/P | Empresa 4 C/P | Até 3 exemplos: pessoa + data | Referência |
|---|---|---:|---:|---:|---:|---|---|
| ESPELHO_ATE_HOJE | **a — não aparece em nenhum dia** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| ESPELHO_ATE_HOJE | **b — mais de um dia** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| ESPELHO_ATE_HOJE | **c — dia errado** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| ESPELHO_ATE_HOJE | **d — impressa não apurável sem procedência** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| ESPELHO_ATE_HOJE | **e — impressa como órfã** | NA | NA | NA | NA | — | [não verificado] |
| PDF_REAL | **a — não aparece em nenhum dia** | **135/75** | 100/56 | 31/17 | 4/2 | col44 + 2026-10-21; col59 + 2026-10-21; col59 + 2026-10-21¹ | [não verificado] |
| PDF_REAL | **b — mais de um dia** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| PDF_REAL | **c — dia errado** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| PDF_REAL | **d — impressa não apurável sem procedência** | 0/0 | 0/0 | 0/0 | 0/0 | — | [não verificado] |
| PDF_REAL | **e — impressa como órfã** | **1.136/204** | 896/164 | 238/39 | 2/1 | col43 + 2026-11-02; col43 + 2026-11-02; col44 + 2026-11-09² | [não verificado] |

¹ São três batidas distintas: PKs **132891, 132730, 132746**, respectivamente. [não verificado]  
² São três batidas distintas: PKs **143768, 143890, 150459**, respectivamente. [não verificado]

A classe **e** usa o atributo `orfa` atribuído pelo coletor do PDF; por isso, NA no espelho não significa zero. [`app/relatorios/pdf_espelho.py:609`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:609)

A borda pedida — saída cross-meia-noite do último dia de cada recorte — ficou assim na saída fornecida. [não verificado]

| Superfície | borda_a C/P | borda_elegiveis C/P | borda_a / borda_elegiveis | Empresa 2 | Empresa 3 | Empresa 4 | Referência |
|---|---:|---:|---|---|---|---|---|
| ESPELHO_ATE_HOJE | 0/0 | 0/0 | **0/0 — sem elegíveis** | 0/0 | 0/0 | 0/0 | [não verificado] |
| PDF_REAL | 0/0 | 85/85 | **0/85** | 0/64 | 0/17 | 0/4 | [não verificado] |

Assim, **nenhuma das 135 batidas de (a) no PDF foi classificada como essa borda**. [não verificado]

As janelas declaradas são iguais para as empresas **2, 3 e 4**, na competência **11/2026**. [não verificado]

| Recorte | Início | Fim usado | Fim da competência | Referência |
|---|---|---|---|---|
| ESPELHO_ATE_HOJE | 2026-10-21 | 2026-11-17 | 2026-11-20 | [não verificado] |
| PDF_REAL | 2026-10-21 | 2026-11-16 | 2026-11-20 | [não verificado] |

O instante declarado foi **2026-11-17T15:18:26.061216-03:00**. [não verificado] O teto do PDF em ontem está implementado no coletor. [`app/relatorios/pdf_espelho.py:287`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:287)

A cobertura e os controles declarados foram:

| Indicador | Resultado da saída | Referência |
|---|---|---|
| Frota / medidos | **545/545**; empresa 2: **424**, empresa 3: **101**, empresa 4: **20** | [não verificado] |
| Falhas | **falhas=0**; `FALHAS_POR_EMPRESA {}` | [não verificado] |
| COBERTURA | **COMPLETA** | [não verificado] |
| Tabelas antes/depois | **103/103** | [não verificado] |
| CONTAGENS_IGUAIS | **True** | [não verificado] |
| ROLLBACK_FINAL | **OK** | [não verificado] |

Para as classes positivas, os limites são:

- **(a), PDF_REAL:** a saída acusa 135 PKs esperados ausentes de todos os dias retornados por essa superfície. Não permite afirmar apagamento do banco, causa da ausência, ausência em outros documentos/períodos ou enquadramento dos 135 em `sem_turno`; isso permanece **[não verificado]**. Pelo limite declarado no item1.r, a medição observa estruturas consumidas pelo desenho, sem inspecionar bytes renderizados. **[não verificado]**
- **(e), PDF_REAL:** os 1.136 casos representam o atributo `orfa` observado pela sonda. **[não verificado]** Não certificam, por identidade de PK, que cada batida seja efetivamente órfã: o coletor atribui esse atributo comparando **HH:MM**, enquanto a identidade do mapa de batidas é o **PK**. [`app/relatorios/pdf_espelho.py:609`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:609); [`app/ponto/turnos.py:2916`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/ponto/turnos.py:2916). A presença efetiva do asterisco nos bytes de cada PDF permanece **[não verificado]**; o desenho prevê a impressão das batidas sinalizadas com `*HH:MM`. [`app/relatorios/pdf_espelho.py:885`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:885)

Os zeros também respeitam o alcance dos predicados: **c=0** não certifica dia de turno para batidas sem turno, diagnóstico que a saída separa; **d=0** verifica o campo de procedência, sem certificar sua legibilidade integral no documento. **[não verificado]** O PDF corta as observações agregadas em 120 caracteres. [`app/relatorios/pdf_espelho.py:974`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:974)

Por fim, `falhas=0` não elimina a limitação de exceções internas silenciosas declarada no item1.r: a leitura de `DiaPago` pode capturar uma exceção e seguir com um dicionário vazio. [`app/ponto/services/espelho.py:409`](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/ponto/services/espelho.py:409) `CONTAGENS_IGUAIS=True` informa igualdade das contagens, sem demonstrar igualdade integral do conteúdo. **[não verificado]**

## 17/11 15:28 · sessao 6a420b646c9b · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — novo campo quebra inventário pessoal.** `resposta_mantem_validacao` nasce em `app/chamados/models.py:4064`; o padrão considera qualquer nome contendo `resposta` pessoal (`bin/sombra_regras_pessoais.py:163`), mas sua cobertura para DecisaoPergunta declara somente `ato_texto`/`ato_motivo` (`bin/sombra_regras_pessoais.py:142`) e a isenção existente é apenas a FK (`bin/sombra_regras_pessoais.py:233`). O inventário percorre todos os campos concretos, acusa coluna descoberta e sai 1 (`bin/inventario_pessoal_dos_modelos.py:73`, `bin/inventario_pessoal_dos_modelos.py:78`, `bin/inventario_pessoal_dos_modelos.py:87`). O selo host transforma a falta de OK em reprovação (`bin/tests/test_inventario_pessoal_no_commit.sh:31`). O selo da sombra também encontra a coluna pelo catálogo e recusa (`bin/sombra_selo_pessoal.py:30`, `bin/sombra_selo_pessoal.py:34`, `bin/sombra_selo_pessoal.py:94`), interrompendo o refazer (`bin/sombra.sh:275`). `chamados` está no universo tenant (`app/config/settings/saas.py:38`). Confirmei por AST/regex em memória que o nome casa e não consta em ISENTOS; execução do selo host/DB [não verificado].

2. **FILA — a frase “nenhum leitor lê resposta_mantem_validacao” precisa delimitar leitores de negócio.** A fotografia E4B enumera todos os campos concretos (`app/chamados/services/foto_e4b.py:251`) e inclui DecisaoPergunta (`app/chamados/services/foto_e4b.py:280`): ambas as colunas entram na saída e no arquivo de reversão (`app/chamados/management/commands/backfill_fio_morto_e4b.py:284`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:287`). A comparação de fotos considera igualdade integral dos dicionários (`app/chamados/services/foto_e4b.py:116`). Uma foto anterior ao deploy comparada a outra posterior produziria diferenças de esquema; caminho que efetivamente recupere foto antiga para essa comparação [não verificado]. No command real, antes/depois são fotografados pelo mesmo processo, dentro do atomic (`app/chamados/management/commands/backfill_fio_morto_e4b.py:280`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:303`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:319`); a releitura do arquivo é imediata e compara contra o conteúdo recém-gerado (`app/chamados/management/commands/backfill_fio_morto_e4b.py:56`). Hash ou golden persistido cruzando deploy nesse caminho [não verificado].

3. **NAO PROCEDE — outros genéricos, com escopo.** `describe model DecisaoPergunta` passa a listar os dois nomes/tipos (`app/core/management/commands/describe.py:43`, `app/core/management/commands/describe.py:55`, `app/core/management/commands/describe.py:65`). A foto de teste que chama `values()` sem projeção também ganha ambos os campos, mas compara antes/depois na mesma execução (`app/chamados/tests/test_e1_entradas.py:279`, `app/chamados/tests/test_e1_entradas.py:295`). O genérico de formulário `core/reexibir.py` enumera campos (`app/core/reexibir.py:30`), porém os chamadores encontrados são de Colaborador/Posto (`app/colaboradores/views.py:1114`, `app/colaboradores/views.py:1279`); uso com DecisaoPergunta [não verificado]. O mixin admin é genérico (`app/core/admin.py:50`), mas registro de DecisaoPergunta no admin [não verificado] — `app/chamados/admin.py` está vazio. Uso de `model_to_dict`, `ModelSerializer` ou `serializers.serialize` com DecisaoPergunta [não verificado]; não encontrei esses mecanismos na busca pelos `.py` da cópia.

4. **NAO PROCEDE para selo vazio; execução integral [não verificado].** Executei sem Django/DB, via AST em memória e `python3 -B`, os dois métodos do tripwire substituindo somente a base por `unittest.TestCase`: passaram o censo real de quatro escritores e a fonte sintética (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:76`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:82`). O selo exclui migrations/tests e só reconhece ato sintaticamente literal/atributo RESPOSTA (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:32`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:51`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:65`): indiretos e SQL continuam fora.

5. **NAO PROCEDE quanto ao ramo do RED do filtro; fixture ORM [não verificado].** A fixture declara RESPOSTA antes da REABERTURA (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:67`, `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:79`), recria carga a cada leitura e exige uma instância da pergunta (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:73`). O ramo real retorna `aguarda_validacao` para RESPOSTA textual, e `devolvida_ao_colaborador` para última REABERTURA (`app/chamados/services/tarefa_do_dia.py:158`, `app/chamados/services/tarefa_do_dia.py:163`). Extraí e executei esse corpo por AST com insumos mínimos: antes = `aguarda_validacao`; marcada sem filtro = `devolvida_ao_colaborador`; marcada filtrada = `aguarda_validacao`. Assim, o mutante alcançado pelo ramo reprova as asserções (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:88`). A execução da fixture inteira e do mutante sem filtro no ORM [não verificado]. O teste compara apenas a razão, não a tarefa inteira (`app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:76`, `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:89`).

6. **NAO PROCEDE para RED de default decorativo; execução DB [não verificado].** O teste insere omitindo ambas as colunas e exige retorno `(False, False)` (`app/chamados/tests/test_f2_db_default_das_colunas_novas.py:25`, `app/chamados/tests/test_f2_db_default_das_colunas_novas.py:32`), além de conferir os defaults no catálogo (`app/chamados/tests/test_f2_db_default_das_colunas_novas.py:35`). Remover somente o default Python não faria esse teste vermelho; remover `db_default` da migração retiraria justamente o mecanismo que o INSERT omitido exige (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:18`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:19`). Sem DB, resultado efetivo [não verificado].

## 17/11 15:27 · sessao 6b961a0c2650 · - · - · origem=Ronald (tmux)

**NAO PROCEDE — escritor RESPOSTA omitido ou com valor divergente do padrão de motivo.** Os quatro montadores efetivos declaram:

| Montador | Campo | Motivo |
|---|---:|---|
| `app/chamados/services/disputa_emissao.py:1525` | False, linha 1527 | Omitido; default `''` em `app/chamados/models.py:4043` |
| `app/chamados/services/disputa_emissao.py:1679` | True, linha 1681 | Texto constante, linha 1680 |
| `app/chamados/services/disputa_emissao.py:1701` | True, linha 1703 | Prefixo constante `"pela trilha: "`, linha 1702 |
| `app/chamados/models.py:4128` | True, linha 4130 | Constante definida na linha 4106, usada na linha 4129 |

Todos desembocam no único `bulk_create`, em `app/chamados/models.py:4149`; as atualizações em lote são recusadas em `app/chamados/models.py:3996`, `app/chamados/models.py:4002`, e o `save` de linha existente em `app/chamados/models.py:4098`.

**FILA — premissa documental sobre “regra de hoje deriva de ato_motivo” não confere como implementação.** O único leitor decisório de `ato_motivo` localizado usa **DEVOLUCAO**, não RESPOSTA, em `app/chamados/services/materializacao.py:275`; a outra leitura é exibição em `app/templates/chamados/partials/modal_fio.html:212`. A projeção temporal lê os campos da própria pergunta, sem decisões ou motivo, em `app/chamados/juizes.py:1389`. Logo, os quatro valores novos reproduzem o **padrão de preenchimento do motivo** dos escritores; não localizei juiz atual que derive esse booleano de motivo. A promessa contrária está em `app/chamados/models.py:4062` e `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:3`.

**NAO PROCEDE — montagem indireta com `ato` variável produzindo RESPOSTA.** A montagem variável existente em `app/chamados/services/disputa_emissao.py:1633` aceita apenas `ATOS_COM_FOTO`, mediante guarda na linha 1620; o conjunto é `(REABERTURA, RECUSA)` em `app/chamados/models.py:4036`.

**FILA — alcance real do tripwire.** O scanner só reconhece o literal `'resposta'` ou atributo `.RESPOSTA` (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:32`, linha 51), ignora migrações e só percorre `app/` (linhas 20, 64–66). Não acompanha fluxo por variáveis, `**kwargs`, mutação posterior de dicionário nem o conteúdo recebido em `registrar` (`app/chamados/models.py:4139`). Os chamadores indiretos existentes, portanto, ficam fora da análise do próprio local de chamada, embora usem o montador coberto:

- Ressuscitação e nascimento com assinaturas: `app/chamados/models.py:1613`, `app/chamados/models.py:3129`.
- Preservação antes de resposta humana/automática: `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1674`.
- Validação e devoluções: `app/chamados/services/validacao.py:167`, `app/chamados/services/validacao.py:272`, `app/chamados/services/validacao.py:345`.
- Backfill efetivamente escritor: `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.
- Entrada de command pela porta da trilha: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`.
- Entradas humanas: `app/chamados/views.py:1283`, `app/api/views.py:2627`, `app/chamados/services/acoes_disputa.py:373`, `app/chamados/services/declaracao_texto.py:419`, `app/chamados/services/declaracao_texto.py:693`.
- Entrada automática: `app/api/foto_recebida.py:48`.

O `backfill_decisoes_pergunta` **mede** RESPOSTA com o montador em `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`; a escrita própria localizada é SILENCIO, linhas 324–327. Não acrescentar um quinto montador RESPOSTA por causa dessa medição. A migração anterior cria `RespostaAnterior` em `app/chamados/migrations/0050_resposta_anterior.py:17`; a 0051 transforma o acervo em REABERTURA (`app/chamados/migrations/0051_decisao_pergunta.py:49`, linha 61), e seu `RunPython` apenas preenche chaves de PerguntaDisputa (linhas 22–27 e 101).

**NAO PROCEDE — rejeição de linhas anteriores pelas duas constraints novas.** Ambas as colunas nascem False (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:19`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:20`), satisfazendo incondicionalmente o segundo braço dos respectivos checks (linhas 24 e 25). INSERT velho omitindo as colunas recebe False pelo `db_default` desses mesmos locais; UPDATE velho que preserve essas colunas também não viola tais checks.

**FILA — valor semântico transitório e pré-requisito do P2.** Processo anterior ao P1 ainda escreve False para RESPOSTA automática/trilha/projeção, mesmo com motivo preenchido, porque omite o campo; isso é aceito pelo check da 0055 e previsto pelo adiamento do backfill (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:3`). O P2 precisa garantir que nenhum desses processos sobreviva ao backfill, exigência já escrita nas linhas 4–5; drenagem efetiva é **[não verificado]**.

**Validação:** executei somente o extrato puro das funções AST do tripwire, em memória e com `python3 -B`; ele retornou exatamente os quatro locais e valores da tabela. Testes Django, defaults reais do catálogo, execução de migrate e inspeção da base `fbc5c76ce` são **[não verificado]**; a cópia não contém `.git`, e não usei banco nem logs.

## 17/11 15:27 · sessao e7821b5c428f · - · - · origem=Ronald (tmux)

Censo de leitores concluído, somente por código.

1. **NÃO PROCEDE — o filtro cobre os dois sítios que decidem pelo último ato de `DecisaoPergunta`.** A carga faz query própria ordenada por pergunta/instante/pk e exclui `es_da_ressuscitacao(d)` antes de alimentar `_dec` (`app/chamados/services/tarefa_do_dia.py:421`, `:427`, `:430`). Esse mapa chega a `_tarefa_pura` em `:588`, `:589`; os dois julgamentos do último ato são:
   - `_pela_decisao`: `ult = decisoes[-1]`, e REABERTURA/RECUSA viram `devolvida_ao_colaborador` (`app/chamados/services/tarefa_do_dia.py:158`, `:163`, `:164`). Chamado em `:273` e `:334`.
   - Exclusão de pergunta cujo último ato é RETRATAÇÃO (`app/chamados/services/tarefa_do_dia.py:259`, `:260`).
   
   A entrada pública sem carga também cria essa carga (`app/chamados/services/tarefa_do_dia.py:745`, `:746`). Portanto ambos recebem a sequência filtrada, inclusive na janela P2 (`app/chamados/services/tarefa_do_dia.py:427`, `:588`).

2. **FILA/P2 — há leitor de REABERTURA fora da carga, mas não decide fila nem último ato: histórico da resposta.** `PerguntaDisputa.respostas_anteriores` percorre `self.decisoes.all()` e, para qualquer `ATOS_COM_FOTO`, acrescenta a decisão ao histórico e limpa `vigente`; `ATOS_COM_FOTO` continua incluindo REABERTURA (`app/chamados/models.py:3675`, `:3680`, `:3681`, `:3682`, `:4036`). Esse caminho usa `Prefetch('decisoes')` próprio, sem filtro de marca (`app/chamados/services/fio.py:263`, `:264`), ou query própria quando não há cache (`app/chamados/models.py:3675`). O wrapper delega ao mesmo leitor (`app/chamados/services/disputa_emissao.py:2122`, `:2126`). O modal publica o histórico (`app/templates/chamados/partials/modal_fio.html:205`, `:208`, `:212`).
   
   Quando P2 gravar uma REABERTURA marcada, ela **entrará nesse histórico**, e o rótulo será “Reaberta para correcao”, pois o template usa `get_ato_display` e o choice permanece esse (`app/templates/chamados/partials/modal_fio.html:212`; `app/chamados/models.py:4031`). Isso é mudança de saída verificável pelo código. Ser semanticamente errado é **[não verificado]**: o próprio contrato do leitor diz que expõe respostas que deixaram de ser vigentes (`app/chamados/models.py:3668`), e ressuscitar zera a resposta (`app/chamados/models.py:1629`, `:1630`). Não confundir esse histórico com desvio do dono da fila.

3. **NÃO PROCEDE — demais usos de REABERTURA/ATOS_COM_FOTO não são leitores de sequência que decidam fila.**
   - `es_da_ressuscitacao`: testa ato REABERTURA e a coluna (`app/chamados/models.py:4096`).
   - A porta `reabrir_para_correcao` valida o parâmetro `ato` contra `ATOS_COM_FOTO`, antes de escrever; não lê uma linha P2 (`app/chamados/services/disputa_emissao.py:1620`, `:1621`, `:1633`).
   - Constraints de fotos e marca restringem atos admissíveis; não julgam sequência (`app/chamados/models.py:4077`, `:4082`).
   - `linhas_da_projecao_sem_linha` consulta apenas RESPOSTA/VALIDAÇÃO por correspondência de ato e instante; a REABERTURA marcada não entra nessa consulta (`app/chamados/models.py:4122`, `:4123`, `:4126`, `:4131`).
   - `rotulo_bug_par_103` percorre o mesmo prefetch, mas só reage a DEVOLUÇÃO com prefixo de motivo; REABERTURA marcada não satisfaz (`app/chamados/services/materializacao.py:275`, `:276`).
   - Backfill consulta existência de SILÊNCIO/VALIDAÇÃO, não último ato nem REABERTURA (`app/chamados/management/commands/backfill_decisoes_pergunta.py:315`).

4. **FILA/P2, sem regressão nova demonstrada — último ato RESPOSTA ignora `resposta_mantem_validacao`.** Toda RESPOSTA com texto é `aguarda_validacao`, independentemente do novo booleano (`app/chamados/services/tarefa_do_dia.py:159`, `:160`, `:162`). A carga só remove REABERTURA marcada (`app/chamados/services/tarefa_do_dia.py:427`, `:428`). Logo uma sequência VALIDACAO seguida de RESPOSTA com `resposta_mantem_validacao=True` continua caindo em `aguarda_validacao` neste hash (`app/chamados/services/tarefa_do_dia.py:158`, `:162`). Isso já existe para os escritores automáticos e sintéticos, que neste P1 gravam True (`app/chamados/services/disputa_emissao.py:1681`, `:1703`; `app/chamados/models.py:4130`); atribuir o erro especificamente à futura janela P2 exige o contrato final dos leitores P2, **[não verificado]**. Não achei leitor de sequência derivando esse significado de `ato_motivo` neste hash; a tarefa sequer consulta motivo em `_pela_decisao` (`app/chamados/services/tarefa_do_dia.py:142`, `:187`).

5. **NÃO PROCEDE — novo predicado não duplica os juízes registrados C19/C21.** C19 responde ações/dono/porta e é registrado em `tarefa_do_dia`; C21 responde par instante+texto e é registrado em `pergunta_respondida` (`app/core/juizes.py:159`, `:170`, `:304`, `:306`). O novo predicado só identifica a marca da linha (`app/chamados/models.py:4096`), e o filtro atua dentro da carga do próprio C19 (`app/chamados/services/tarefa_do_dia.py:427`). Não há novo relógio no trecho alterado: compara `ato_em` ao `agora` recebido (`app/chamados/services/tarefa_do_dia.py:429`); a API exige `agora` e valida igualdade com a carga (`app/chamados/services/tarefa_do_dia.py:743`, `:747`).

6. **Leitor genérico confirmado, para consolidar com o outro explorador:** a foto E4B incorpora todos os campos concretos (`app/chamados/services/foto_e4b.py:251`) e fotografa explicitamente `DecisaoPergunta` (`:280`); ambas as colunas passam a compor `campos` por esse caminho. A comparação entre fotos considera desigualdade de dicionários inteiros (`app/chamados/services/foto_e4b.py:118`, `:121`), portanto foto antes com código base versus foto depois com P1 muda mesmo com False nos dois campos. Execução concreta atravessando deploy/golden existente é **[não verificado]**.

Não usei banco nem executei testes com banco; resultados de execução e existência real de processos antigos são **[não verificado]**. [nao verificado]

## 17/11 15:18 · sessao b5989c47cce4 · E4C-F2 · censo · origem=Code

Nos anexos colados, conto a primeira linha de cada anexo como 1, incluindo linhas vazias. As referências à cópia usam seus próprios números de linha.

**1 — Escritor antigo `responder_automaticamente`: (b), com ressalva sobre “descartou”.** O Codex viu exatamente o defeito: “RESPOSTA automática ou pela trilha [...] inserida pelo código antigo após o backfill, omitindo a coluna. Ela recebe `False`, e a leitura nova desfaz a validação”. A triagem respondeu “PROCEDENTE, FORA DA CONDICAO 1, sobe ao coordenador” e “Nao aplicado (escopo do aval literal: condicao 1)”. Portanto, houve exclusão da cura por escopo, não rejeição técnica do achado. (`v5.r:25`; `triagem_v5.txt:6–10`.) [nao verificado]

O escritor antigo está confirmado: grava RESPOSTA com motivo sem declarar a coluna nova. ([disputa_emissao.py:1678](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/services/disputa_emissao.py:1678).)

**1 — Escritor antigo `carimbar_resposta_pela_trilha`: (b), na mesma acepção.** O trecho “RESPOSTA automática ou pela trilha” identifica também este escritor; a triagem aceita o defeito e deixa a correção fora do escopo autorizado. O código antigo confirma a RESPOSTA com motivo e sem a coluna. (`v5.r:25`; `triagem_v5.txt:6–10`; [disputa_emissao.py:1699](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/services/disputa_emissao.py:1699).)

Há uma ressalva factual ao laudo: a cópia descreve `curar_perguntas_em_limbo` como “cura de acervo que roda UMA VEZ”, enquanto a linha 1482 nomeia outro comando. Seu agendamento como cron e sua execução durante a janela são **[nao verificado]**. ([crons.py:1476](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/config/crons.py:1476); `app/config/crons.py:1477`; `app/config/crons.py:1482`.)

**1 — Escritor antigo da foto da projeção: (a).** O tema estava presente: “automática, carimbo pela trilha e foto da projeção escrevem `True`”. A fábrica e seus backfills também estavam no censo anterior. Entretanto, o contraexemplo temporal do v5 nomeia apenas automática e trilha. Assim, “TERCEIRO nao citado pelo Codex” deve ser restringido a **não identificado como terceiro escritor antigo afetado pela janela**; ele foi citado como montador. (`v5.r:22,25`; `v2.r:25,29`; `opus_final_c8f5.txt:4–5`.)

A cópia confirma a fábrica antiga produzindo RESPOSTA com `ato_motivo`, sem a coluna nova. ([models.py:4100](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/models.py:4100); `app/chamados/models.py:4101`.)

Como complemento comum aos três escritores, a correção sugerida pela triagem era executar novamente o backfill “DEPOIS do HUP”; porém o deploy documenta que comandos por `docker exec`, incluindo cron em curso, não recebem esse sinal. Esse prolongamento da janela não foi explicitado no contraexemplo do v5. (`triagem_v5.txt:9`; `bin/deploy.sh:329–330`; `v5.r:25`.)

**2 — Janela inversa merge→migrate: (a) apenas quanto ao mecanismo concreto do cron.** A incompatibilidade geral **já foi vista**: “Código novo executado contra banco sem 0055 é incompatível, pois já exige a coluna”. Portanto, seria incorreto classificá-la integralmente como despercebida. O trecho não identifica o cron que nasce após o merge e antes da migration; é essa especificação que o opus acrescenta. (`v2.r:90`; `opus_final_c8f5.txt:13`.)

Descarte dessa janela por alguma triagem fornecida: **[nao verificado]**. A expressão “classe da casa, nao bloqueia” pertence ao próprio opus, não a uma triagem do Codex. (`opus_final_c8f5.txt:13`.)

**3 — Condição documental aberta: (e), provisória.** O suporte literal da pendência é “SEM ato de devolucao” e “lei NAO decidida”; o laudo registra ainda zero ocorrências dos dois cortes em `CORTES.md/json`. ([LEIS.md:151](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/docs/LEIS.md:151); `opus_final_c8f5.txt:14–15`.)

Não identifiquei nas respostas fornecidas uma análise dessa pendência documental; que ela tenha sido perguntada é **[nao verificado]**. **Sem as `.q`, é impossível distinguir com segurança (a) de (e).** A triagem mencionar o corte como justificativa funcional não comprova uma pergunta sobre sua documentação. (`triagem_v1.txt:18–20`; conteúdo das perguntas: **[nao verificado]**.)

**4 — Decisão por prefixo de `ato_motivo` em materialização: (a).** O Codex descreveu o comportamento, mas não o denunciou como contradição: “O leitor decisório de motivo [...] restringe-se a **DEVOLUCAO BUG-PAR**” e “permanece o reconhecimento textual de DEVOLUCAO BUG-PAR, fora dessa cura”. (`v2.r:41,91`.)

A cópia confirma literalmente `.startswith(MOTIVO_BUG_PAR_103)`. A triagem “8-11: conferencia, sem defeito apontado” não documenta descarte de uma acusação desse defeito. ([materializacao.py:275](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/chamados/services/materializacao.py:275); `triagem_v1.txt:22`.)

- Resposta automática antiga → **(b), excluída da cura por escopo**. (`triagem_v5.txt:6–10`)
- Carimbo antigo pela trilha → **(b), excluído da cura por escopo**. (`triagem_v5.txt:6–10`)
- Foto antiga da projeção → **(a)**. (`v5.r:22,25`)
- Janela inversa → **(a) parcial: mecanismo do cron; incompatibilidade já vista**. (`v2.r:90`; `opus_final_c8f5.txt:13`)
- Pendência documental → **(e) provisória; (a)/(e) indistinguíveis sem `.q`**. [nao verificado]
- Prefixo de motivo na materialização → **(a)**. (`v2.r:41,91`)

## 17/11 15:17 · sessao 46f631bd2ab0 · FIDELIDADE-BATIDA-X-ESPELHO · leitura · origem=Code

**A) Não localizei um contador de frota que verifique o contrato completo por identidade de batida.** A inexistência absoluta fora dos caminhos inspecionados permanece [não verificado]. Os instrumentos encontrados verificam fixtures, minutos ou resumos, como detalhado abaixo. `app/relatorios/tests/test_cartao_nao_perde_batida.py:57`; `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:78`; `app/folha/porta_export.py:496`.

A cadeia real preserva objetos `Batida` em `dia['batidas']`: o espelho agrupa por `pk → dia`, o coletor do PDF consome esses dias e acrescenta `b.orfa`, e o desenho imprime tanto as normais quanto as órfãs. A identidade correta é **PK**, pois timestamps podem colidir. `app/ponto/services/espelho.py:450`; `app/ponto/services/espelho.py:574`; `app/ponto/services/espelho.py:764`; `app/relatorios/pdf_espelho.py:596`; `app/relatorios/pdf_espelho.py:604`; `app/relatorios/pdf_espelho.py:876`; `app/ponto/turnos.py:2916`.

| Instrumento | Universo, exclusões e esperado | Quem chama; placar; o que não cobre |
|---|---|---|
| `CartaoNaoPerdeBatidaTest` | Uma pessoa, três turnos noturnos e uma espúria; somente os PKs plantados na fixture. Espera faltantes vazios e nenhuma repetição. `app/relatorios/tests/test_cartao_nao_perde_batida.py:24`; `:39`; `:57`; `:67`. | Os testes chamam `_coletar_dados_espelho`. Não consultam `batidas_apuraveis`, não varrem frota nem comparam o dia de cada PK com o juiz. `app/relatorios/tests/test_cartao_nao_perde_batida.py:58`; `:61`; `:68`; `:71`. Vínculo nominal desse teste com item do placar: [não verificado]. |
| `pdf_x_espelho_divergentes` | Empresas em geral, mas somente pessoas classificadas como `entra` no TXT; classificação que falha e pessoa não encontrada são puladas. Compara **minutos e aberto por dia**, esperando zero divergentes. `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:56`; `:78`; `:97`; `:114`. | Comando manual, declarado como não cron; chama coletor do PDF e `espelho_do_colab`. Está em **E4-pdf-x-espelho**. Não compara PKs, completude, duplicação ou procedência. `app/config/crons.py:1139`; `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:73`; `:92`; `app/core/espelho_verdade.py:124`. |
| `selo_leitores_no_mesmo_numero` → `folha.porta_export.medir` | Empresas ativas; a comparação tela/PDF considera `classificar_export(... status='entra')`. Compara cinco escalares do resumo, tolerância `0.01`; o selo espera zero divergências. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`; `app/folha/porta_export.py:469`; `:496`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:178`. | O comando chama `medir` **e lavra**. É selo de porta/manual, relacionado ao **E4** e fonte do **R4 estrutural**. Uma perda compartilhada pelos leitores pode conservar os escalares: o predicado não contém identidade de batida. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88`; `app/config/crons.py:887`; `app/core/espelho_verdade.py:120`; `app/core/placar_estrutural.py:414`; `app/folha/porta_export.py:497`. |
| `e6_oraculo` | Empresas ativas, pessoas com `FechamentoMensal`; opcionalmente só TXT. Exclui isenção/ausência, falta de lavratura, zero dos dois lados e diferença explicada pela janela. Compara minutos, aceitando diferença até dez minutos. `app/ponto/management/commands/e6_oraculo.py:303`; `:339`; `:365`. | Lê apuráveis, mas usa pareamento independente; não certifica cada PK impresso contra `turnos_do_colab`. Relaciona-se a **E6-sistema-bate** e **E6-batida-impar**. `app/ponto/management/commands/e6_oraculo.py:8`; `app/core/espelho_verdade.py:180`; `:188`. |
| `esmeril_espelho` | Pessoas em operação, excluindo isentos; janela da competência mais antiga não entregue até ontem. Conta assinaturas recorrentes; A5 usa órfãs da ata com repetição mínima. `app/ponto/services/esmeril_espelho.py:38`; `:125`; `:139`; `:206`. | Há cron declarado com `--apply`. Seus insumos são células, turnos e ata; não compara `dia['batidas']` com o universo apurável. `app/config/crons.py:640`; `app/ponto/services/esmeril_espelho.py:95`. Item E1–E6/R específico dessa fidelidade: [não verificado]. |

Portanto, **E4/R4 são os mais próximos na frota; o teste `CartaoNaoPerdeBatidaTest` é o mais próximo por PK**. O R4 enumera comparações entre leitores e valores, sem declarar o par “apuráveis × marcações impressas”. `app/core/placar_estrutural.py:211`; `app/relatorios/tests/test_cartao_nao_perde_batida.py:61`.

**B) Script proposto, sem execução no banco. [não verificado]**

Há dois recortes necessários: `ESPELHO_ATE_HOJE` mede o espelho solicitado até hoje; `PDF_REAL` respeita o teto em ontem do coletor. A classe **e** pertence a `PDF_REAL`, pois esse coletor atribui `orfa`. Assim, batidas de hoje não são acusadas como perdas do PDF por causa do teto previsto. `app/ponto/services/espelho.py:1194`; `app/relatorios/pdf_espelho.py:285`; `:598`; `:604`.

A janela vem de `janela_competencia(empresa, 11, 2026)`; “ativa” significa `situacao='ativo'`. O universo apurável inclui pendentes de DP e exclui retratadas; o script não exclui isentos nem exige entrada no TXT. `app/folha/export.py:47`; `app/colaboradores/models.py:296`; `:323`; `app/ponto/turnos.py:2871`.

| Classe | Identificação usada |
|---|---|
| a | `b.pk` esperado ausente de **todos os dias retornados** da pessoa. Universo por `batidas_apuraveis` e dia por `dia_das_batidas`, incluindo as não reclamadas por turno. `app/ponto/turnos.py:2868`; `:2898`; `:2957`. |
| b | Mesmo `b.pk` em mais de um `dia['data']`. Há diagnóstico adicional para repetição no mesmo dia. `app/ponto/services/espelho.py:764`; `app/ponto/turnos.py:2916`. |
| c | `b.pk` em `t.batidas`, comparando `dia['data']` com `t.data_turno`. Batida sem turno é publicada separadamente, sem inventar dia de turno. `app/ponto/turnos.py:43`; `:2961`. |
| d | `b.pk` fora de `batidas_apuraveis(colab)` **sem recorte temporal**, e `b.procedencia` vazio. `app/ponto/turnos.py:2879`; `app/ponto/services/espelho.py:755`; `app/relatorios/pdf_espelho.py:951`. |
| e | `b.pk` cujo `b.orfa` foi atribuído pelo coletor real. **A atribuição atual usa HH:MM**, embora a contagem abaixo preserve PK. `app/relatorios/pdf_espelho.py:604`. |
| borda | Subconjunto de a: `t.saida.pk`, com `t.cross_meianoite` e `t.data_turno == fim` daquele recorte. Publica também o denominador elegível. `app/ponto/turnos.py:43`; `app/relatorios/pdf_espelho.py:271`. |

O script mede as estruturas consumidas pelo desenho; não inspeciona bytes renderizados. Em particular, d verifica o campo de procedência, enquanto o PDF limita o texto agregado de observações a 120 caracteres. As células sintéticas de isenção são impressas por outro campo e não entram como `Batida`. `app/relatorios/pdf_espelho.py:887`; `:951`; `:974`.

O código abaixo propõe transação PostgreSQL somente leitura, savepoint por pessoa, contagens antes/depois no mesmo snapshot e rollback deliberado. Captura exceções propagadas, erros SQL e avisos registrados; exceções internas sem log, como a leitura degradada de `DiaPago`, continuam sendo uma limitação observável do código da casa. **Validação em execução: [não verificado].** `app/ponto/services/espelho.py:409`.

```python
# Proposta para colar como um bloco no manage.py shell.
# Execucao e resultados no tenant: [nao verificado].
# Nenhuma funcao geradora de PDF, lavra ou comando --apply e chamada.

import logging
from collections import Counter, defaultdict
from datetime import date

_incidentes = set()
_handle_original = logging.Logger.handle


def _log_sem_arquivo(self, registro):
    # Somente memoria; nao encaminha mensagens/PII aos handlers.
    if registro.levelno >= logging.WARNING:
        exc = registro.exc_info
        classe = exc[0].__name__ if exc and exc[0] else (
            "Log" + registro.levelname
        )
        _incidentes.add(classe)


class _RollbackFinal(Exception):
    pass


class _LeituraDegradada(Exception):
    pass


def executar_fidelidade():
    from django.db import connection, transaction
    from django.utils import timezone
    from django_tenants.utils import schema_context
    from colaboradores.models import Colaborador, Empresa
    from folha.export import janela_competencia
    from ponto.turnos import (
        batidas_apuraveis, turnos_do_colab, dia_das_batidas,
    )
    from ponto.services.espelho import espelho_do_colab
    from relatorios.pdf_espelho import _coletar_dados_espelho
    from relatorios.cartao_pela_celula import ate_ontem

    empresas_ids = (2, 3, 4)
    agora = timezone.now()
    hoje = timezone.localtime(agora).date()
    falhas = set()
    falhas_empresa = Counter()
    medidos = Counter()
    totais = defaultdict(lambda: {
        "contagem": 0, "pessoas": set(),
        "empresa": Counter(), "pessoas_empresa": defaultdict(set),
        "exemplos": [],
    })
    classes = (
        "a", "b", "c", "d", "e", "borda_a",
        "universo_apuravel", "impressas",
        "sem_turno", "juiz_multidia",
        "repetida_mesmo_dia", "borda_elegiveis",
    )

    def vigiar_sql(execute, sql, params, many, context):
        try:
            return execute(sql, params, many, context)
        except Exception as erro:
            _incidentes.add(type(erro).__name__)
            raise

    def contar_tabelas(rotulo):
        with connection.cursor() as cursor:
            cursor.execute("""
                SELECT table_name
                  FROM information_schema.tables
                 WHERE table_schema = %s
                   AND table_type = 'BASE TABLE'
                 ORDER BY table_name
            """, ["juliani"])
            tabelas = [r[0] for r in cursor.fetchall()]
            if not tabelas:
                raise RuntimeError("SchemaSemTabelas")
            q = connection.ops.quote_name
            contagens = {}
            for tabela in tabelas:
                cursor.execute(
                    "SELECT COUNT(*) FROM %s.%s"
                    % (q("juliani"), q(tabela))
                )
                contagens[tabela] = cursor.fetchone()[0]
        print("TABELAS_%s quantidade=%d" % (rotulo, len(contagens)))
        for tabela, quantidade in contagens.items():
            print("%s juliani.%s=%d" % (rotulo, tabela, quantidade))
        return contagens

    def medir_pessoa(colab, ini, fim, dias, apuraveis, pdf=False):
        # Universo de apresentacao: autoridade real, sem copiar seu
        # tratamento de orfas/espurias/borda.
        # app/ponto/turnos.py:2898,2957
        turnos = turnos_do_colab(colab, ini, fim)
        mapa = dia_das_batidas(colab, ini, fim, turnos=turnos)

        esperadas = {
            pk: b for pk, b in apuraveis.items()
            if b.timestamp <= agora
            and ini <= mapa.get(pk, date.min) <= fim
        }

        juiz = defaultdict(set)
        bordas = set()
        for turno in turnos:
            for b in turno.batidas:
                juiz[b.pk].add(turno.data_turno)
            if (
                turno.data_turno == fim
                and turno.cross_meianoite
                and turno.saida is not None
                and turno.saida.pk in esperadas
            ):
                bordas.add(turno.saida.pk)

        # Nao recorta antes de indexar: batida presente em outro dia
        # retornado deve entrar em c, em vez de virar falsa ausencia.
        impressas = defaultdict(list)
        objetos = {}
        for dia in dias:
            for b in dia["batidas"]:
                if b.pk is None:
                    raise ValueError("MarcacaoSemPK")
                impressas[b.pk].append(dia["data"])
                objetos.setdefault(b.pk, b)

        alvo = set(esperadas) | {
            pk for pk, datas in impressas.items()
            if any(ini <= d <= fim for d in datas)
        }
        achados = {classe: {} for classe in classes}

        def incluir(classe, pk, dia, b):
            achados[classe][pk] = (
                dia, timezone.localtime(b.timestamp).isoformat()
            )

        faltantes = set(esperadas) - set(impressas)
        for pk, b in esperadas.items():
            incluir("universo_apuravel", pk, mapa[pk], b)
            if pk in faltantes:
                incluir("a", pk, mapa[pk], b)

        for pk in bordas:
            incluir("borda_elegiveis", pk, fim, esperadas[pk])
            if pk in faltantes:
                incluir("borda_a", pk, fim, esperadas[pk])

        for pk in sorted(alvo & set(impressas)):
            b = objetos[pk]
            datas = impressas[pk]
            dias_distintos = set(datas)
            primeiro_dia = min(datas)
            incluir("impressas", pk, primeiro_dia, b)

            if len(dias_distintos) > 1:
                incluir("b", pk, primeiro_dia, b)
            if any(n > 1 for n in Counter(datas).values()):
                incluir("repetida_mesmo_dia", pk, primeiro_dia, b)

            dias_juiz = juiz.get(pk, set())
            if not dias_juiz:
                incluir("sem_turno", pk, primeiro_dia, b)
            else:
                if len(dias_juiz) > 1:
                    incluir("juiz_multidia", pk, primeiro_dia, b)
                errados = [
                    d for d in datas
                    if any(d != esperado for esperado in dias_juiz)
                ]
                if errados:
                    incluir("c", pk, min(errados), b)

            if pk not in apuraveis and not getattr(b, "procedencia", None):
                incluir("d", pk, primeiro_dia, b)

            if pdf:
                # Usa o atributo posto pelo coletor; nao refaz HH:MM.
                # app/relatorios/pdf_espelho.py:604-609
                if not hasattr(b, "orfa"):
                    raise AttributeError("ColetorSemFlagOrfa")
                if b.orfa:
                    incluir("e", pk, primeiro_dia, b)

        return achados

    def acumular(superficie, colab, achados):
        for classe, casos in achados.items():
            if superficie == "ESPELHO_ATE_HOJE" and classe == "e":
                continue
            total = totais[(superficie, classe)]
            total["contagem"] += len(casos)
            if casos:
                total["pessoas"].add(colab.pk)
                total["empresa"][colab.empresa_id] += len(casos)
                total["pessoas_empresa"][colab.empresa_id].add(colab.pk)
            for pk, (dia, instante) in sorted(casos.items()):
                if len(total["exemplos"]) >= 3:
                    break
                total["exemplos"].append(
                    "col%s %s batida=%s instante=%s"
                    % (colab.pk, dia.isoformat(), pk, instante)
                )

    if connection.in_atomic_block:
        raise RuntimeError("ExigeShellForaDeTransacao")

    with schema_context("juliani"):
        try:
            with transaction.atomic():
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL "
                        "REPEATABLE READ, READ ONLY"
                    )
                    cursor.execute("SELECT current_schema()")
                    if cursor.fetchone()[0] != "juliani":
                        raise RuntimeError("SchemaIncorreto")

                antes = contar_tabelas("ANTES")
                try:
                    empresas = Empresa.objects.in_bulk(empresas_ids)
                    if set(empresas) != set(empresas_ids):
                        raise LookupError("EmpresaSolicitadaAusente")

                    janelas = {}
                    for eid in empresas_ids:
                        ini, fim_competencia = janela_competencia(
                            empresas[eid], 11, 2026
                        )
                        fim = min(fim_competencia, hoje)
                        if fim < ini:
                            raise ValueError("CompetenciaAindaNaoIniciada")
                        fim_pdf = max(ini, ate_ontem(fim, hoje=hoje))
                        janelas[eid] = (ini, fim, fim_pdf)
                        print(
                            "JANELA emp%s competencia=11/2026 "
                            "inicio=%s fim_competencia=%s "
                            "espelho_ate=%s pdf_ate=%s"
                            % (eid, ini, fim_competencia, fim, fim_pdf)
                        )

                    frota = list(
                        Colaborador.objects.filter(
                            empresa_id__in=empresas_ids,
                            situacao="ativo",
                        ).select_related("empresa").order_by("pk")
                    )
                    frota_empresa = Counter(c.empresa_id for c in frota)
                    print("INSTANTE", timezone.localtime(agora).isoformat())
                    print("FROTA", len(frota), dict(frota_empresa))
                    if not frota:
                        raise ValueError("UniversoVazio")

                    for colab in frota:
                        _incidentes.clear()
                        try:
                            # Savepoint: erro SQL de uma pessoa nao
                            # inutiliza a transacao das demais.
                            with transaction.atomic():
                                with connection.execute_wrapper(vigiar_sql):
                                    ini, fim, fim_pdf = janelas[colab.empresa_id]

                                    # Vida inteira para d nao confundir
                                    # fora da janela com nao apuravel.
                                    # app/ponto/turnos.py:2879
                                    apuraveis = {
                                        b.pk: b
                                        for b in batidas_apuraveis(colab)
                                    }

                                    espelho = espelho_do_colab(
                                        colab, hoje, ini, fim,
                                        ano=2026, agora=agora,
                                    )
                                    resultado_espelho = medir_pessoa(
                                        colab, ini, fim,
                                        espelho["dias"], apuraveis,
                                    )

                                    # O coletor chama o montador real e
                                    # atribui o flag exato usado no papel.
                                    # app/relatorios/pdf_espelho.py:596,604
                                    pdf = _coletar_dados_espelho(
                                        colab, ini, fim, agora=agora
                                    )
                                    resultado_pdf = medir_pessoa(
                                        colab, ini, fim_pdf,
                                        pdf["dias"], apuraveis, pdf=True,
                                    )

                                    if _incidentes:
                                        raise _LeituraDegradada()

                            # So publica a pessoa depois das duas leituras.
                            acumular(
                                "ESPELHO_ATE_HOJE", colab, resultado_espelho
                            )
                            acumular("PDF_REAL", colab, resultado_pdf)
                            medidos[colab.empresa_id] += 1

                        except Exception as erro:
                            falhas.add(colab.pk)
                            falhas_empresa[colab.empresa_id] += 1
                            print(
                                "ERRO col%s emp%s classe=%s internas=%s"
                                % (
                                    colab.pk, colab.empresa_id,
                                    type(erro).__name__,
                                    ",".join(sorted(_incidentes)) or "-",
                                )
                            )
                            

[truncado em 20000 de 22503 caracteres]

## 17/11 15:15 · sessao 51bc0855b4b9 · E4C-F1-B2-CERCA · verificador · origem=Code

**REPROVA.** Há falhas na seleção do dia consultado, no alcance da guarda e na certificação por mutantes/AST. Os caminhos síncronos dos antigos achados 1 e 2 passaram a abrir cerca. (`app/chamados/services/fio.py:1409`; `app/chamados/models.py:3576`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:565`; `selo_diff.txt:+102`; `app/chamados/services/disputa_emissao.py:1909`)

Nas referências ao selo, `+N` indica a linha do arquivo novo apresentado no anexo. Execução integral da suíte, resultados dos logs citados na triagem e ocorrência dos contraexemplos em produção: **[nao verificado]**. [nao verificado]

**1. Contorno por retenção — NAO PROCEDE como falha remanescente nesse caminho.**

O fallback continua sendo chamado quando a retratação retorna falsa por `retencao` ou `transicao`, mas agora entra na porta pública do resolver; essa porta trava e executa o corpo sob cerca. A recusa volta no dicionário e o comando a conta antes do `continue`. (`app/ponto/management/commands/reconciliar_fantasmas.py:288`; `app/chamados/services/disputa_emissao.py:1903`; `app/chamados/services/disputa_emissao.py:1909`; `app/chamados/services/disputa_emissao.py:1914`; `app/ponto/management/commands/reconciliar_fantasmas.py:291`)

As escritas ordinárias de perguntas existentes desse resolver passam pelo `save` vigiado; o descarimbo em lote também tem vigilância. Não identifiquei, nesse fluxo síncrono, escrita do resolver pelo bulk de lavras. (`app/chamados/models.py:3572`; `app/chamados/models.py:3401`; `app/chamados/services/disputa_emissao.py:2019`; `app/chamados/services/disputa_emissao.py:2093`; `app/chamados/management/commands/backfill_veredito_lavrado.py:143`)

**2. Resolver sem cerca — NAO PROCEDE para as entradas nomeadas.**

| Entrada | Caminho efetivo |
|---|---|
| Beco | O callback inclui resolvers e `d.fechar`, dentro da cerca do beco. (`app/chamados/services/becos.py:55`; `app/chamados/services/becos.py:66`) |
| HIST | Costura e resolver estão no mesmo callback cercado. (`app/chamados/reconciliador.py:1690`; `app/chamados/reconciliador.py:1701`) |
| S143 | Chama a porta pública, que abre a cerca do resolver. (`app/chamados/signals.py:117`; `app/chamados/services/disputa_emissao.py:1909`) |
| Retratação | Chama o resolver dentro da cerca externa da unidade. (`app/chamados/reconciliador.py:553`; `app/chamados/reconciliador.py:580`) |
| Fantasmas | Chama a mesma porta pública. (`app/ponto/management/commands/reconciliar_fantasmas.py:288`) |

As chamadas de produção encontradas ao corpo privado estão na própria porta: a chamada cercada e a exceção da via trancada. Nessa exceção, o corpo retorna antes das consultas e escritas de perguntas. Portanto, **chamar o corpo sem cerca nessa via não demonstra bypass de escrita**. (`app/chamados/services/disputa_emissao.py:1897`; `app/chamados/services/disputa_emissao.py:1909`; `app/chamados/services/disputa_emissao.py:1934`)

**3. Sobrealcance — PROCEDE na porta; a cura não seleciona somente aquilo que será persistido.**

O `save` chama a vigilância antes de encaminhar `update_fields`. A vigilância examina o objeto inteiro em memória e a data gravada, sem receber os campos efetivamente enviados ao banco. (`app/chamados/models.py:3572`; `app/chamados/models.py:3576`; `app/chamados/models.py:3581`; `app/chamados/services/fio.py:1403`; `app/chamados/services/fio.py:1409`)

Isso permite dois contraexemplos estáticos:

- Pergunta ENTREGUE com `save(update_fields=[])`: a guarda pode recusar antes de chegar à operação que não persistiria campos. (`app/chamados/models.py:3576`; `app/chamados/models.py:3581`)
- Banco com dia ABERTO, objeto com dia ENTREGUE somente em memória e `update_fields=['pergunta']`: a guarda consulta também o dia ENTREGUE, embora `data_evento` não fosse gravada. (`app/chamados/services/fio.py:1409`; `app/chamados/services/fio.py:1411`; `app/chamados/models.py:3581`)

O descarimbo individual e o lote também não eliminam previamente linhas cujos campos já estejam nos valores finais. Assim, podem consultar o veto para um descarimbo sem mudança de valores. (`app/chamados/models.py:3327`; `app/chamados/models.py:3333`; `app/chamados/models.py:3401`; `app/chamados/models.py:3406`)

Os controles HX evitam o sobrealcance anteriormente provocado pela previsão; isso não prova ausência dos contraexemplos acima em toda retratação real. Ocorrência integrada desses padrões em produção: **[nao verificado]**. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:331`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:345`)

**4. Mordida dos testes — PROCEDE parcialmente: alvo corrigido, certificação ainda defeituosa.** [nao verificado]

A asserção específica foi implementada: compara a linha completa pelo PK da pergunta-alvo antes das outras fotografias; a classificação `ALVO` também exige esse PK na mensagem. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:249`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:258`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:535`)

| Ponto | Conclusão |
|---|---|
| `sem_escopo:fechar_beco = {}` | **Coerente com o caso, mas sem mordida.** Retirar apenas a cerca do beco preserva a cerca interna do resolver. Falta caso que exija a proteção adicional do `d.fechar`. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:435`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:563`; `app/chamados/services/becos.py:60`; `app/chamados/services/disputa_emissao.py:1909`) |
| `engolido_sobrevive → e=ALVO` | **Incoerente.** No caso `e`, o veto direto sobe ao `except` do próprio mutante, que devolve as vetadas. O wrapper registra a recusa. A guarda continua anterior à persistência; o fluxo desse caso deve permanecer verde, não escrever o alvo. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:94`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:418`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:565`; `app/chamados/services/disputa_emissao.py:1913`; `app/chamados/models.py:3576`) |
| Caso `s` | **C21 completo:** texto não vazio, `respondida_em` e validação. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:382`) |
| Geofence | **Rótulo coerente:** o relatório particiona pelos mesmos três motivos impressos pelo comando. (`app/ponto/services/geofence.py:444`; `app/ponto/services/geofence.py:448`; `app/ponto/management/commands/reconciliar_geofence.py:46`) |

A fotografia também tem limites: decisões, pautas, ocorrências e auditoria são comparadas por contagem; a auditoria exclui todas as ações `retratacao_retida_*`. Ela não demonstra identidade integral do conteúdo dessas tabelas. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:215`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:219`)

**5. Quem escreve e quem lê — PROCEDE a existência de escritor sem vigilância, declarado como backfill.**

O mapa abaixo reúne os campos explícitos do diff e dos caminhos examinados. Enumeração global de todos os leitores e escritores indiretos da aplicação: **[nao verificado]**.

| Modelo / campos | Escritores | Leitores relevantes |
|---|---|---|
| Pergunta: `disputa`, `motivo`, `pergunta`, `tipo_resposta`, `conversa` | Nascimento em `app/chamados/models.py:3134`; fixtures em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:146` | Guarda em `app/chamados/services/fio.py:1408`; HIST em `app/chamados/reconciliador.py:1602`; leitura integral da pergunta em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:258` |
| Pergunta: `celula`, `chamado_destino` | Nascimento/reuso em `app/chamados/models.py:3086`, `app/chamados/models.py:3102`, `app/chamados/models.py:3134`; costura em `app/chamados/services/fio.py:974` | Seleção direta em `app/chamados/services/disputa_emissao.py:1939`; HX em `app/chamados/services/disputa_emissao.py:2069`; relavra em `app/chamados/models.py:309` |
| Pergunta: `data_evento` | Atribuição na porta em `app/chamados/services/fio.py:849`; costura em `app/chamados/services/fio.py:928`; nascimento em `app/chamados/models.py:3136` | Guarda em `app/chamados/services/fio.py:1403`, `app/chamados/services/fio.py:1409`; HIST em `app/chamados/reconciliador.py:1612`; resolver em `app/chamados/services/disputa_emissao.py:2019` |
| Pergunta: `materializada_em`, `materializada_por`, `via_resolucao` | Porta do carimbo em `app/chamados/models.py:3220`; persistências em `app/chamados/models.py:3273`, `app/chamados/models.py:3311`, `app/chamados/models.py:3333`; lote em `app/chamados/models.py:3406` | Resolver em `app/chamados/services/disputa_emissao.py:1941`, `app/chamados/services/disputa_emissao.py:2073`, `app/chamados/services/disputa_emissao.py:2083`; exclusão do lote em `app/chamados/models.py:3402` |
| Pergunta: `resposta_colab`, `respondida_em` | Resposta em `app/chamados/services/disputa_emissao.py:1481`; automática em `app/chamados/services/disputa_emissao.py:1674`; reparo em `app/chamados/services/disputa_emissao.py:1696`; carimbo em `app/chamados/models.py:3253`; limpeza em `app/chamados/models.py:3408` | C21 em `app/chamados/juizes.py:1552`; resolver em `app/chamados/services/disputa_emissao.py:2013`; HX em `app/chamados/services/disputa_emissao.py:2090`; porta do par em `app/chamados/models.py:3603` |
| Pergunta: `validada_em`, `validada_por` | Validação em `app/chamados/services/validacao.py:168`; devolução em `app/chamados/services/validacao.py:273`; nova resposta em `app/chamados/services/disputa_emissao.py:1494`; carimbo em `app/chamados/models.py:3256`; limpeza em `app/chamados/models.py:3408` | Relavra em `app/chamados/models.py:310`; HX em `app/chamados/services/disputa_emissao.py:2073`; nova resposta em `app/chamados/services/disputa_emissao.py:1484` |
| Pergunta: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar` em `app/chamados/veredito_lavrado.py:270`; backfill em `app/chamados/management/commands/backfill_veredito_lavrado.py:138`; bulk em `app/chamados/veredito_lavrado.py:250` | `esta_stale` em `app/chamados/veredito_lavrado.py:224`; fotografia em `app/chamados/veredito_lavrado.py:281`; teste em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:258` |
| Chamado: `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por`, `atualizado_em` | `_gravar_estado` em `app/chamados/models.py:228`, `app/chamados/models.py:231`, `app/chamados/models.py:244`; persistência em `app/chamados/models.py:296` | Resolver em `app/chamados/services/disputa_emissao.py:1896`, `app/chamados/services/disputa_emissao.py:2066`; S143 em `app/chamados/signals.py:97`; fotografia em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:211` |
| Chamado: `urgencia`, `urgencia_original` | Ritual em `app/chamados/models.py:460`; restauração em `app/chamados/models.py:251`; update do receiver em `app/chamados/models.py:637` | Rebaixamento em `app/chamados/models.py:469`; fotografia em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:211` |
| Chamado: `colaborador`, `justificativa_relacionada`, `categoria`, `subcategoria`, `modulo_origem`, `contexto_json`, `assunto`, `titulo`, `area` | Nascimento parametrizado em `app/chamados/models.py:1142`; fixtures em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:139` | Retratação em `app/chamados/reconciliador.py:550`, `app/chamados/reconciliador.py:573`; HIST em `app/chamados/reconciliador.py:1633`, `app/chamados/reconciliador.py:1642`; fotografia em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:211` |
| Disputa: `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | Fecho em `app/chamados/models.py:2023`; reaberturas em `app/chamados/models.py:2240`, `app/chamados/models.py:2264` | Beco em `app/chamados/services/becos.py:59`; resolver em `app/chamados/services/disputa_emissao.py:2063`; receivers em `app/chamados/signals.py:29`, `app/chamados/signals.py:165` |
| Disputa: `colaborador`, `chamado`, `motivos`, `contexto` | Emissão em `app/chamados/services/disputa_emissao.py:550`; alterações em `app/chamados/models.py:2057`, `app/chamados/models.py:2154`, `app/chamados/models.py:2195`; fixtures em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:201` | Guarda em `app/chamados/services/fio.py:1414`; beco em `app/chamados/services/becos.py:44`; HIST em `app/chamados/reconciliador.py:1605`; resolver em `app/chamados/services/disputa_emissao.py:2061` |
| Auditoria: `acao`, `modelo`, `objeto_id`, `descricao`, `valor_antes`, `valor_depois`, `usuario`, `ip`, `ator` | `evento` em `app/core/observ.py:134`; adaptador em `app/core/observ.py:146`; retenção prepara e delega em `app/chamados/reconciliador.py:478`, `app/chamados/reconciliador.py:493` | Deduplicação lê ação/modelo/objeto, `id` e payload em `app/chamados/reconciliador.py:489`; testes filtram ações em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:219`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:224` |

**Bypass concreto:** `gravar_lavras_em_lote` usa `bulk_update` sem `vigiar`. Os chamadores encontrados são os dois pontos do backfill; não encontrei ligação dos atos cercados até esse escritor. (`app/chamados/veredito_lavrado.py:250`; `app/chamados/management/commands/backfill_veredito_lavrado.py:143`; `app/chamados/management/commands/backfill_veredito_lavrado.py:146`)

O `update` de descarimbo passa pela guarda; os `save` de nascimento não passam, por decisão explícita da implementação. Ausência absoluta de escrita dinâmica/raw: **[nao verificado]**. (`app/chamados/models.py:3401`; `app/chamados/models.py:3406`; `app/chamados/models.py:3572`)

**6. Classes recorrentes — PROCEDE a falha semântica; ordem dos abridores não é uniforme.**

**ESMERIL-SEMANTICA:** o helper utilizado pela guarda simplesmente devolve `data_evento`. Entretanto, o próprio módulo distingue essa chave gravada da identidade do fato, obtida pelo juiz do turno para linhas com episódio. A migração reconhece dia da saída diferente do dia do turno e preserva grupos que toquem competência exportada. Logo, o código não garante que a chave consultada tenha sempre o significado exigido pelo veto. (`app/chamados/juizes.py:342`; `app/chamados/services/fio.py:1409`; `app/chamados/services/fio.py:1605`; `app/chamados/services/fio.py:1620`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:17`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:30`)

Contraexemplo estático: turno de 23/09 ENTREGUE, saída em 24/09 ABERTA e pergunta legada com `data_evento=24/09`. Uma pergunta materializada, não validada, pode ser relavrada pela transição do destino; a guarda consulta somente 24/09 e libera. Existência desse caso em produção: **[nao verificado]**. (`app/chamados/models.py:305`; `app/chamados/models.py:310`; `app/chamados/veredito_lavrado.py:275`; `app/chamados/services/fio.py:1409`; `app/chamados/services/vigia_fio.py:802`)

| Abridor | Savepoint da cerca em relação à trava |
|---|---|
| Retratação | Depois da trava. (`app/chamados/reconciliador.py:552`; `app/chamados/reconciliador.py:553`) |
| Beco | Depois da trava. (`app/chamados/services/becos.py:52`; `app/chamados/services/becos.py:66`) |
| Resolver | Depois da trava. (`app/chamados/services/disputa_emissao.py:1903`; `app/chamados/services/disputa_emissao.py:1909`) |
| HIST | Antes da trava interna da costura. (`app/chamados/reconciliador.py:1701`; `app/chamados/reconciliador.py:1691`; `app/chamados/services/fio.py:938`) |

Essa diferença do HIST, isoladamente, não prova deadlock. Deadlock observado: **[nao verificado]**.

Não nasceu outro juiz de competência: a guarda chama o veto existente, que compõe janela e K6. As funções novas da cerca não leem relógio; os caminhos continuam tendo `now()` internos no resolver e na lavra. Portanto, “nenhum relógio fora da borda” não foi demonstrado. (`app/chamados/services/fio.py:1419`; `app/chamados/services/fio.py:1365`; `app/chamados/services/vigia_fio.py:800`; `app/chamados/services/disputa_emissao.py:2019`; `app/chamados/services/disputa_emissao.py:2076`; `app/chamados/veredito_lavrado.py:271`)

A `_CERCA` recebe estado por abertura e restaura o token em `finally`; aninhamentos reutilizam o estado externo. Não encontrei fundamento nesse código para afirmar vazamento normal entre requests, threads independentes ou testes. Certificação no ambiente real: **[nao verificado]**. (`app/chamados/services/fio.py:1433`; `app/chamados/services/fio.py:1435`; `app/chamados/services/fio.py:1448`)

Cumprimento dos pré-requisitos de pouso: **[nao verificado]**. [nao verificado]

**7. Selo AST — PROCEDE a insuficiência da adaptação como certificação.**

A implementação consulta na porta, depois de a transição já ter iniciado; o rollback substitui a consulta anterior à transição. Isso não implementa o instante literal descrito no aval reproduzido na triagem. (`app/chamados/reconciliador.py:553`; `app/chamados/reconciliador.py:567`; `app/chamados/models.py:3576`)

Quanto aos requisitos do próprio selo:

- **(a) Insuficientemente provado.** O selo aceita chamada sob `if False` antes do save; procura qualquer `If/Raise` para a guarda e qualquer `With/If/Raise` para a cerca, sem demonstrar a condição de veto ou o `atomic` correto. O abridor só precisa mencionar as vetadas em algum `if`. (`selo_diff.txt:+277`; `selo_diff.txt:+290`; `selo_diff.txt:+296`; `selo_diff.txt:+239`)
- **(b) Falha concreta de subaproximação.** `_indexa` e `_liga` não atravessam `if`/`with` para indexar definições. Perdem `_ato` do beco e `_costura_e_fecho` do HIST; o corpo desses callbacks também é excluído do chamador. Callbacks sem definição são depois ignorados. Esse limite não é o limite declarado de atributos homônimos. (`selo_diff.txt:+102`; `selo_diff.txt:+139`; `selo_diff.txt:+148`; `selo_diff.txt:+308`; `app/chamados/services/becos.py:42`; `app/chamados/services/becos.py:55`; `app/chamados/reconciliador.py:1603`; `app/chamados/reconciliador.py:1690`)
- **(c) Coerente para `lavrar(salvar=False)`:** a persistência fica no save do chamador, mas isso depende de o grafo encontrar esse chamador. (`selo_diff.txt:+163`; `app/chamados/services/fio.py:975`; `app/chamados/services/fio.py:976`)
- **(d) Coerente tratar `registrar_retencao` como trilha:** ela delega a escrita de auditoria, não escreve pergunta. (`selo_diff.txt:+47`; `app/chamados/reconciliador.py:493`)

Os 14 nomes declarados têm explicações compatíveis com entradas avulsas sem cobertura integral. Os dois nomes do resolver incluem falsos positivos conservadores da via trancada, cujo corpo não escreve. A quantidade efetivamente emitida pelo selo completo: **[nao verificado]**. Essa lista não certifica completude, pois os callbacks acima desaparecem do grafo. (`app/chamados/services/disputa_emissao.py:1897`; `app/chamados/services/disputa_emissao.py:1934`; `selo_diff.txt:+102`; `selo_diff.txt:+308`)

**8. Matriz dos 15 sítios — PROCEDE uma lacuna de cobertura da unidade.**

| Sítio | Situação |
|---|---|
| 1. `retratar_com_motivo` | Abre cerca antes do corpo, depois da trava. (`app/chamados/reconciliador.py:552`; `app/chamados/reconciliador.py:553`) |
| 2. `fechar_beco` | Cerca resolver e fecho. (`app/chamados/services/becos.py:55`; `app/chamados/services/becos.py:66`) |
| 3. Resolver | Cerca o corpo; via trancada retorna sem escrita. (`app/chamados/services/disputa_emissao.py:1909`; `app/chamados/services/disputa_emissao.py:1934`) |
| 4. `reconciliar` | Herda cerca quando aninhado; avulso escreve estado/relavra. (`app/chamados/models.py:655`; `app/chamados/models.py:296`; `app/chamados/models.py:312`) |
| 5. `transicionar` | Cobertura herdada; converge no escritor de estado. (`app/chamados/models.py:450`; `app/chamados/models.py:296`) |
| 6. `encerrar` | Cobertura herdada. (`app/chamados/models.py:1686`; `app/chamados/models.py:296`) |
| 7. Encerramento por tranca | Via especial; resolver não escreve. (`app/chamados/models.py:1702`; `app/chamados/services/disputa_emissao.py:1934`) |
| 8. `reabrir` | Descarimbo vigiado quando há cerca externa. (`app/chamados/models.py:1639`; `app/chamados/models.py:3401`) |
| 9. `Disputa.fechar` | Herda cerca; avulso pode escrever perguntas e disputa. (`app/chamados/models.py:2012`; `app/chamados/models.py

[truncado em 20000 de 22820 caracteres]

## 17/11 15:14 · sessao 65345d776547 · - · - · origem=Ronald (tmux)

Item 4: o comportamento já estava descrito, mas não foi tratado como defeito. Literais: v2 §3, “O leitor decisório de motivo encontrado em `materializacao.py` restringe-se a DEVOLUCAO BUG-PAR, não RESPOSTA”; v2 §6, “permanece o reconhecimento textual de DEVOLUCAO BUG-PAR, fora dessa cura”. A cópia confirma `(d.ato_motivo or '').startswith(MOTIVO_BUG_PAR_103)` em `app/chamados/services/materializacao.py:275`. Recomendo **(a)**: tema presente; contradição não apontada. “8-11: conferencia, sem defeito apontado” da triagem v1 não documenta descarte desse defeito.

Item 2: v2 §6 já afirma literalmente “Código novo executado contra banco sem 0055 é incompatível, pois já exige a coluna”. Assim, **não cabe dizer que a incompatibilidade geral passou despercebida**. O mecanismo específico merge → nascimento de cron → migrate não é explicitado; **(a)** só cabe delimitando essa omissão. Nenhuma triagem fornecida documenta descarte dessa janela inversa; forçar **(b)** seria inadequado. [nao verificado]

A cópia confirma árvore como bind mount (`bin/deploy.sh:95`), migration (`bin/deploy.sh:317`) antes do HUP (`bin/deploy.sh:360`) e que comandos via exec, incluindo cron, não recebem esse HUP (`bin/deploy.sh:329`).

Ressalvas para consolidação:

- `models.py:4175/4182` do laudo são linhas da raia, não desta cópia: localmente pertencem a `RegularizacaoExterna`. Não usar como verificação local da alegada contradição.
- O laudo chama `curar_perguntas_em_limbo` de cron; nesta cópia ele aparece como cura de execução única, “--apply e ato do Ronald” (`app/config/crons.py:1476`). Sua chamada à porta existe (`app/chamados/management/commands/curar_perguntas_em_limbo.py:67`); agendamento rotineiro e ocorrência efetiva são **[nao verificado]**.

## 17/11 15:14 · sessao e7054ee1dc3a · - · - · origem=Ronald (tmux)

Item 1: automática e trilha cabem em **(b)**: v5 identifica literalmente “RESPOSTA automática ou pela trilha [...] inserida pelo código antigo após o backfill”; a triagem registra “PROCEDENTE, FORA DA CONDICAO 1” e “Nao aplicado (escopo do aval literal: condicao 1)”. (`v5.r`, item 5; `triagem_v5.txt:5–9`, numeração a partir da primeira linha do anexo.) [nao verificado]

A foto da projeção cabe em **(a)**: o tema aparece no censo — “automática, carimbo pela trilha e foto da projeção escrevem `True`” —, mas o contraexemplo da janela nomeia apenas automática e trilha. Portanto, “TERCEIRO nao citado pelo Codex” precisa significar não citado **como escritor antigo afetado pela janela**, pois o montador foi citado nas respostas. (`v5.r`, item 5; `v2.r`, seção 2; `v3.r`, seção 5.)

Código antigo confirmado na cópia:

- Automática: RESPOSTA com motivo e sem campo novo em `app/chamados/services/disputa_emissao.py:1678–1679`; entrada em `app/api/foto_recebida.py:48`.
- Trilha: RESPOSTA com motivo e sem campo em `app/chamados/services/disputa_emissao.py:1699–1700`; chamada em `app/chamados/management/commands/curar_perguntas_em_limbo.py:67–68`.
- Projeção: RESPOSTA com motivo em `app/chamados/models.py:4100–4101`; usos em `app/chamados/services/validacao.py:167/272/345`, `app/chamados/services/disputa_emissao.py:1480/1673`, `app/chamados/models.py:1613/3129`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`.

O deploy migra em `bin/deploy.sh:317` e envia HUP em `bin/deploy.sh:360`; seus comentários explicam que o comando por `docker exec` não recebe o sinal em `bin/deploy.sh:329–330`. A imutabilidade antiga consta em `app/chamados/models.py:4070–4072`.

Ressalva relevante ao laudo: `curar_perguntas_em_limbo` está listado como cura que roda “UMA VEZ” e exige ato do Ronald em `app/config/crons.py:1476–1477`; `:1482` identifica outro comando. Seu agendamento efetivo como cron é **[nao verificado]**. A ancestralidade dos hashes também é **[nao verificado]**, pois a cópia não contém repositório Git acessível.

## 17/11 15:13 · sessao 515c143223d7 · - · - · origem=Ronald (tmux)

- **L-130 desatualizada na cópia:** texto literal `SEM ato de devolucao` e ``RESSUSCITAR-DEVOLVE-AO-COLABORADOR` e lei NAO decidida` em `app/docs/LEIS.md:151`.
- A busca literal por `RESSUSCITAR-GRAVA-ATO` e `RESPOSTAS-1711-1050` retornou zero ocorrências em `app/docs/CORTES.md` e `app/docs/CORTES.json`; os arquivos terminam em `:497` e `:1888`, respectivamente. A afirmação sobre a árvore **c8f58df30** continua [nao verificado]; esta é a cópia main fornecida.
- Nas respostas v1/v2/v3/v5 fornecidas, não há análise da pendência documental da L-130, ausência desses cortes ou condição 2 do opus anterior. A existência de perguntas sobre isso é [nao verificado].
- `triagem_v1.txt`, item 6, menciona `CORTE-F2-RESSUSCITAR-GRAVA-ATO` como justificativa funcional; isso não demonstra pergunta ao Codex sobre o registro documental. Linha física do anexo [nao verificado].
- **Classe sugerida: (e), provisória — ausência do tema nas respostas; sem as .q, impossível distinguir (a) de (e).** Não há fundamento para afirmar categoricamente “não perguntado”. [nao verificado]

## 17/11 15:12 · sessao 7aa33ea3be7b · E4C-F2 · censo · origem=Code

**Os quatro montadores de RESPOSTA encontrados coincidem com a 0055 nos valores que produzem; essa equivalência não é uma obrigação do modelo.** A porta aceita as duas discordâncias, demonstradas nos testes. (`app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/disputa_emissao.py:1708`, `app/chamados/models.py:4251`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`)

O censo histórico exato do **pai da obra** permanece `[nao verificado]`: o material disponível não fornece sua fonte integral. O teste que menciona a main `fbc5c76ce` contém uma comparação com resultado salvo, não a árvore daquele commit. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:2`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:10`)

1. **Quem escreve o campo nesta árvore**

   | Montador de RESPOSTA | Valor e regra | Evidência |
   |---|---|---|
   | `responder_pergunta_disputa_core` | `False`; resposta humana nova desfaz a validação anterior. Omite `ato_motivo`, cujo default é `''`. | `app/chamados/services/disputa_emissao.py:1494`, `app/chamados/services/disputa_emissao.py:1532`, `app/chamados/models.py:4162` |
   | `responder_automaticamente` | `True`; grava texto/instante sem zerar validação. Motivo fixo: `resposta automatica (fato entregue por outro caminho)`. | `app/chamados/services/disputa_emissao.py:1685`, `app/chamados/services/disputa_emissao.py:1686` |
   | `carimbar_resposta_pela_trilha` | `True`; carimba uma resposta cujo texto já existe, sem zerar validação. Motivo sempre prefixado por `pela trilha: `. | `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/services/disputa_emissao.py:1707`, `app/chamados/services/disputa_emissao.py:1708` |
   | `linhas_da_projecao_sem_linha` | `True` na foto RESPOSTA: existe `respondida_em`, mas falta linha correspondente ao mesmo instante. Usa `MOTIVO_PROJECAO_SEM_LINHA`. | `app/chamados/models.py:4224`, `app/chamados/models.py:4249`, `app/chamados/models.py:4251` |

   A escrita física comum é `registrar`: constrói `cls(**ln)` e executa `bulk_create`; não calcula o booleano pelo motivo. Campo omitido recebe `default=False`; INSERT que omite a coluna recebe `db_default=False`. (`app/chamados/models.py:4328`, `app/chamados/models.py:4185`)

   Os sítios que persistem as fotos produzidas pelo helper são: nascimento; ressuscitação; resposta humana e automática; validação; devolução; devolução BUG-PAR; tratamento do estoque L130; backfill E4b. (`app/chamados/models.py:3207`, `app/chamados/models.py:1650`, `app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:354`, `app/chamados/services/validacao.py:439`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`)

   Os demais atos omitem o campo e recebem `False`: REABERTURA compensadora; SILENCIO; RETRATACAO; REABERTURA/RECUSA; VALIDACAO; DEVOLUCAO; ABSORCAO; SILENCIO do backfill e do veredito da célula. (`app/chamados/models.py:1691`, `app/chamados/models.py:2083`, `app/chamados/models.py:2215`, `app/chamados/services/disputa_emissao.py:1639`, `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:279`, `app/chamados/services/validacao.py:354`, `app/chamados/services/materializacao.py:1048`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `app/ponto/services/veredito_celula.py:195`, `app/chamados/models.py:4185`)

   `save()` permite inserir uma instância nova, mas recusa regravar instância com PK; `update()` e `bulk_update()` do manager vivo também recusam. Não há nesses métodos uma regra adicional baseada em `ato_motivo`. (`app/chamados/models.py:4216`, `app/chamados/models.py:4115`, `app/chamados/models.py:4121`)

   A **0055** adiciona a coluna com `False` e faz o único UPDATE específico encontrado: `ato='resposta' AND ato_motivo!='' → True`, pelo modelo histórico de migrations. A **0054** adiciona apenas `da_ressuscitacao`; a **0051** atribui `ato='reabertura'` ao acervo renomeado. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:11`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:26`, `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:17`, `app/chamados/migrations/0051_decisao_pergunta.py:49`)

2. **Equivalência com o backfill**

   **Não são regras idênticas, linha a linha:** os escritores declaram o efeito da sua operação; a 0055 classifica pelo conteúdo de `ato_motivo`. Entretanto, os quatro montadores atuais produzem valores coincidentes: humana = motivo vazio/False; automática, trilha e projeção = motivo não vazio/True. Isso inclui expressamente `MOTIVO_PROJECAO_SEM_LINHA` e “resposta automatica”. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/disputa_emissao.py:1708`, `app/chamados/models.py:4251`)

   **Nos quatro montadores de produção encontrados, nenhum grava o contrário do predicado. Pela porta genérica, ambas as discordâncias são permitidas:** a constraint só proíbe `True` fora de RESPOSTA. Os testes gravam motivo preenchido/False e motivo vazio/True, e exigem que o leitor siga o campo. (`app/chamados/models.py:4205`, `app/chamados/models.py:4328`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:143`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:150`)

   Logo, a 0055 mudaria o primeiro caso para `True`; deixaria o segundo `True`, pois ela não zera as linhas fora do filtro. A ocorrência desses casos em produção é `[nao verificado]`. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`)

3. **Quem lê; derivação versus persistência; momento**

   Os dois leitores diretos de decisão são `q_validada_pelo_ato`, que devolve um filtro SQL, e `_resposta_desfaz_validacao`, consumido por `leitura`. O `validada = None` dentro de `leitura` altera uma variável local; o resultado é `LeituraDoAto`, sem salvar a pergunta. (`app/chamados/models.py:3547`, `app/chamados/models.py:3557`, `app/chamados/models.py:4281`, `app/chamados/models.py:4299`, `app/chamados/models.py:4312`, `app/chamados/models.py:4315`)

   | Consumidor | Resultado |
   |---|---|
   | `estado`, `respondida`, `resposta_display` | Derivam rótulo, booleano e texto. (`app/chamados/models.py:3231`, `app/chamados/models.py:3782`, `app/chamados/models.py:3806`) |
   | `sem_veredito_entre` / `pode_fechar` | Derivam lista e autorização para fechar. (`app/chamados/models.py:2000`, `app/chamados/models.py:2011`) |
   | `_ancoradas`, `q_vivas` | Selecionam perguntas usando a face SQL. (`app/chamados/models.py:357`, `app/chamados/models.py:3609`) |
   | `_gravar_estado` | Usa a face para selecionar perguntas e chamar a lavra persistida. (`app/chamados/models.py:317`, `app/chamados/models.py:319`, `app/chamados/veredito_lavrado.py:275`) |
   | `ressuscitar_perguntas` | Lê a foto, grava REABERTURA, limpa resposta/validação/carimbo, emite evento e reabre disputa. (`app/chamados/models.py:1684`, `app/chamados/models.py:1696`, `app/chamados/models.py:1698`, `app/chamados/models.py:1701`, `app/chamados/models.py:1708`) |
   | `fechar` | Usa a leitura na foto do SILENCIO, grava fechamento e evento. (`app/chamados/models.py:2085`, `app/chamados/models.py:2095`, `app/chamados/models.py:2096`) |
   | `remover_motivo` | Usa o texto lido na RETRATACAO, carimba identidade e salva a disputa. (`app/chamados/models.py:2217`, `app/chamados/models.py:2220`, `app/chamados/models.py:2224`) |

   Há também leitura genérica para fotografia de todos os campos das decisões, sem decidir validação pelo booleano. (`app/chamados/services/foto_e4b.py:247`, `app/chamados/services/foto_e4b.py:280`)

   A cadeia roda em **request** — upload consulta `pode_fechar` e fecha; desarquivar chama ressuscitação —, em **signal** — `post_save` da pergunta agenda fechamento de beco no commit — e em **cron configurado** — consumidor de eventos, por minuto, alcança consultas e fechamentos de disputas. Execução efetiva desses caminhos em produção: `[nao verificado]`. (`app/api/foto_recebida.py:83`, `app/chamados/views_arquivar.py:86`, `app/chamados/signals.py:229`, `app/chamados/signals.py:240`, `app/chamados/services/becos.py:34`, `app/chamados/services/becos.py:58`, `app/config/crons.py:394`, `app/chamados/reconciliador.py:1368`, `app/chamados/reconciliador.py:613`)

   **C6 Python não lê esse booleano:** lê a projeção da pergunta. Seu filtro SQL `q_vivas`, nesta árvore, usa a validação pela sequência. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1404`, `app/chamados/models.py:3609`)

4. **Escritores no main anterior**

   **Censo certificado do pai: `[nao verificado]`.** Na árvore disponível, os produtores de RESPOSTA com motivo não vazio são os três da tabela: automática, trilha e foto da projeção. Atribuí-los integralmente ao pai seria extrapolar a evidência. (`app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/disputa_emissao.py:1708`, `app/chamados/models.py:4251`)

   Os caminhos atuais comprovados são:

   - **Automática:** request de upload → kernel da foto → `responder_automaticamente`. (`app/api/views.py:860`, `app/api/views_core.py:844`, `app/api/foto_recebida.py:74`, `app/api/foto_recebida.py:48`)
   - **Trilha:** comando `curar_perguntas_em_limbo --apply`; o cadastro o classifica como cura de execução única, não rotina. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:64`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`, `app/config/crons.py:1482`)
   - **Projeção sem linha:** os sítios enumerados na pergunta 1; entre as entradas comprovadas estão request de desarquivamento e ressuscitação pelo cartório. A cadeia histórica completa request/signal/cron no pai permanece `[nao verificado]`. (`app/chamados/views_arquivar.py:86`, `app/ponto/services/cartorio.py:1674`)

5. **Divisão P1/P2**

   **Para o booleano, os quatro montadores atuais não apresentam incompatibilidade com o predicado do backfill.** A 0055 não tem corte por data, PK ou origem: respostas escritas entre P1 e P2, presentes na execução e com motivo não vazio, entram no UPDATE; respostas humanas ficam corretamente em `False`. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/services/disputa_emissao.py:1532`, `app/chamados/services/disputa_emissao.py:1686`, `app/chamados/services/disputa_emissao.py:1708`, `app/chamados/models.py:4251`)

   A omissão das colunas por código antigo tem `db_default=False`; existe teste que simula esse INSERT. Isso não prova execução em produção nem todas as compatibilidades do pai `[nao verificado]`. (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:19`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:26`, `app/chamados/tests/test_f2_db_default_das_colunas_novas.py:25`)

   **Para a obra inteira, há uma dependência que impede declarar “leitor velho” genericamente seguro:** o escritor novo grava REABERTURA compensadora; a carga nova precisa ignorá-la antes de escolher o último ato. Se P1 conservar um leitor que entregue essa linha a `_pela_decisao`, ela vira `devolvida_ao_colaborador`. Essa consequência é inferida dos ramos presentes; a implementação exata do leitor no pai é `[nao verificado]`. O filtro da tarefa precisa acompanhar esse escritor no P1 para conservar esse comportamento. (`app/chamados/models.py:1691`, `app/chamados/services/tarefa_do_dia.py:426`, `app/chamados/services/tarefa_do_dia.py:158`, `app/chamados/services/tarefa_do_dia.py:163`)

   Hoje AddField e backfill estão na mesma 0055; a divisão proposta exige separar essas operações. A cobertura acima vale para linhas presentes na execução do UPDATE, não demonstra o comportamento de um pouso concorrente efetivamente realizado `[nao verificado]`. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:22`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:28`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`)

1 — Quatro montadores de RESPOSTA; persistência por `registrar/bulk_create`; defaults False e UPDATE da 0055. (`app/chamados/models.py:4328`, `app/chamados/models.py:4185`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`)  
2 — Coincidem nos valores emitidos atualmente; equivalência universal não existe no contrato. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`)  
3 — Leitores centrais derivam; consumidores também persistem, em requests, signals e consumidor cronificado. (`app/chamados/models.py:4315`, `app/chamados/models.py:1698`, `app/chamados/signals.py:240`, `app/config/crons.py:394`)  
4 — Censo exato dos escritores e caminhos do pai da obra: `[nao verificado]`.  
5 — O intervalo não é excluído pelo backfill; o escritor da REABERTURA exige o filtro correspondente da tarefa no P1. (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `app/chamados/services/tarefa_do_dia.py:426`)

## 17/11 15:12 · sessao 402d3089e3f6 · - · - · origem=Ronald (tmux)

Interfaces e limites confirmados, sem executar banco ou escrever arquivo:

- Tenant: `schema_context('juliani')`, padrão usado por `app/tenants/cron.py:7` e `app/tenants/cron.py:25`; os dados de tenant ficam em schema próprio, segundo `app/tenants/models.py:4`.
- Frota ativa: `Colaborador.objects.filter(empresa_id__in=(2, 3, 4), situacao='ativo').select_related('empresa').order_by('pk')`; FK empresa e campo situação em `app/colaboradores/models.py:321` e `app/colaboradores/models.py:323`, escolha ativo em `app/colaboradores/models.py:296`.
- Janela: `janela_competencia(empresa, 11, 2026)`, assinatura e delegação em `app/folha/export.py:47`; corte por empresa real em `app/ponto/janelas.py:36` e `app/ponto/janelas.py:54`.
- `batidas_apuraveis(colab, ini_datetime, fim_datetime)` aceita timestamps inclusivos, sempre exclui retratadas e inclui pendentes de DP: `app/ponto/turnos.py:2868`, `app/ponto/turnos.py:2871`, `app/ponto/turnos.py:2888`. Não passar datas diretamente; o filtro é `timestamp__gte`/`timestamp__lte`, em `app/ponto/turnos.py:2890` e `app/ponto/turnos.py:2892`.
- `turnos_do_colab(colab, ini_date, fim_date)` recebe datas e devolve turnos cujo `data_turno` pertence à janela; carrega pad real em `app/ponto/turnos.py:1850` e `app/ponto/turnos.py:1867`. Entrada e saída são objetos Batida, evidenciado por acesso a `.timestamp` em `app/ponto/turnos.py:209` e `app/ponto/turnos.py:740`.
- Montador exato que deixa as batidas prontas para impressão: `_coletar_dados_espelho(colab, data_ini, data_fim, _turnos_juiz=turnos, agora=agora)`, assinatura em `app/relatorios/pdf_espelho.py:187`; usa `espelho_do_colab` em `app/relatorios/pdf_espelho.py:596` e devolve os dias recortados em `app/relatorios/pdf_espelho.py:602`.
- Atenção à fronteira temporal: PDF real clipa o fim em ontem, em `app/relatorios/pdf_espelho.py:285`–`288`; `ate_ontem` é `min(data_fim, hoje-1)` em `app/relatorios/cartao_pela_celula.py:37`. Deve-se declarar janela pedida até hoje e janela efetivamente impressa até ontem; medir batidas de hoje contra PDF acusa perdas previstas pelo próprio teto.
- O PDF reescreve `b.orfa` por HH:MM da grade, em `app/relatorios/pdf_espelho.py:604`–`609`; só chamar `espelho_do_colab` não assegura o flag exato que o PDF imprimirá.
- A saída de turno noturno do último dia pode estar na tarde do dia seguinte; a classe é explicitamente documentada em `app/relatorios/pdf_espelho.py:271`–`281`. Não restringir o universo esperado exclusivamente ao timestamp dentro da janela: unir batidas dos turnos reclamados pelo juiz; `dia_das_batidas` já documenta fim+1 em `app/ponto/turnos.py:2919`.
- Borda pode ser identificada por `t.cross_meianoite`, `t.data_turno == fim`, `t.saida is not None`, `t.saida.pk in ausentes`; campos oficiais do Turno em `app/ponto/turnos.py:43` e construção em `app/ponto/turnos.py:323`.
- Identidade correta é PK; timestamp não deve ser chave, conforme contrato explícito em `app/ponto/turnos.py:2916`.
- `espelho_do_colab` pode montar dias anteriores à competência porque usa `min(piso_visual, apur_ini)`, em `app/ponto/services/espelho.py:1225`; o PDF filtra os dias em `app/relatorios/pdf_espelho.py:602`.
- Existem exceções engolidas internamente por funções reais: leitura de DiaPago em `app/ponto/services/espelho.py:409`–`412`, grade em `app/ponto/services/espelho.py:569`–`571`, palavra do dia em `app/ponto/services/espelho.py:645`–`651`. O script deve ao menos contabilizar exceções propagadas por pessoa; afirmar que captura todas as exceções internas seria incorreto.

Sugestão de defesa do script: transação externa com `SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY`, savepoint por pessoa para sobreviver a erro SQL, exceção sentinela ao final para rollback, e contagens de linhas de todas as tabelas do schema antes/depois. Essa montagem é proposta, não validada executando o ambiente. [não verificado]

## 17/11 15:11 · sessao 88bde2b706ce · - · - · origem=Ronald (tmux)

Não encontrei medidor que responda a conjunção inteira por identidade de batida na frota; os instrumentos próximos abaixo comparam fixtures, minutos ou resumos. A inexistência absoluta fora dos caminhos pesquisados permanece [não verificado].

| Instrumento | Universo e exclusões | Esperado / quem chama | Lacuna |
|---|---|---|---|
| `CartaoNaoPerdeBatidaTest` | Fixture de **um colaborador**, três turnos noturnos de junho/2026 e uma batida espúria, construídos em `app/relatorios/tests/test_cartao_nao_perde_batida.py:24` e `:39`. | `test_toda_batida_do_periodo_aparece_no_documento` chama `_coletar_dados_espelho`, compara `self.ids` com `b.pk`, espera conjunto faltante vazio (`:57`). `test_nao_imprime_batida_duplicada` espera comprimento da lista igual ao conjunto de PKs (`:67`). | Não consulta `batidas_apuraveis`, não mede frota e não compara cada dia com `turnos_do_colab`; os predicados efetivos estão em `:61` e `:71`. |
| `test_pdf_le_o_juiz_do_dia` | Um cenário 12x36 19h–07h, um colaborador construído no teste (`app/relatorios/tests/test_pdf_le_o_juiz_do_dia.py:40`). | Chama `turnos_do_colab` e espera `data_turno=11/09`, `cross_meianoite=True` (`:56`); outro teste inspeciona o código para exigir referência ao juiz e ausência da heurística antiga (`:74`). | O teste comportamental mostrado verifica o turno; o teste do PDF é estrutural, sem censo por identidade em produção (`:61`, `:84`). |
| `pdf_x_espelho_divergentes` | Todas as empresas, mas só pessoas com `classificar_export(status='entra')`; empresa cuja classificação falha é excluída e colaborador ausente é pulado (`app/relatorios/management/commands/pdf_x_espelho_divergentes.py:56`). | Chama `_coletar_dados_espelho` (`:73`) e `espelho_do_colab` (`:92`); compara por dia **soma de minutos e flag de aberto**, esperado zero divergentes (`:78`, `:97`, `:114`). É comando manual, não cron (`app/config/crons.py:1139`). | Não lê `dia['batidas']`, não compara PKs, completude, duplicidade ou proveniência; os campos comparados são somente os de `:83` e `:101`. Registrado em **E4-pdf-x-espelho** (`app/core/espelho_verdade.py:124`). |
| `selo_leitores_no_mesmo_numero` / `folha.porta_export.medir` | Empresas ativas no comando (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`); comparação tela/PDF só entre `classificar_export(status='entra')` (`app/folha/porta_export.py:469`). | O comando chama `medir` e **lavra** resultado (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88`). `medir` chama os leitores reais e compara cinco escalares do resumo, com tolerância 0,01 (`app/folha/porta_export.py:476`, `:496`). É selo de porta/manual, não pipeline agendado (`app/config/crons.py:887`). | Pode haver a mesma perda nos dois leitores com resumo coincidente; identidade das marcações não participa do predicado (`app/folha/porta_export.py:497`). Está relacionado ao **E4** (`app/core/espelho_verdade.py:120`) e é fonte do **R4** (`app/core/placar_estrutural.py:211`, `:414`). |
| `e6_oraculo` | Empresas ativas e colaboradores com `FechamentoMensal` na competência, opcionalmente restritos ao TXT (`app/ponto/management/commands/e6_oraculo.py:303`). Exclui dias de isenção/ausência, sem lavratura, zero nos dois e diferença de janela declarada (`:339`). | Compara minutos do espelho com calculadora independente, tolerância até 10 minutos (`:362`). Lê `batidas_apuraveis`, mas forma turnos com regra própria (`:8`). | Não confere marcação impressa por identidade nem usa `turnos_do_colab` como autoridade do esperado para cada PK. Itens **E6-sistema-bate**, **E6-batida-impar** (`app/core/espelho_verdade.py:180`, `:188`); valor é termômetro **R1** no estrutural (`app/core/placar_estrutural.py:58`). |
| `esmeril_espelho` | `colaboradores_em_operacao`, exclui `isento_ponto`; janela mais antiga não entregue até ontem (`app/ponto/services/esmeril_espelho.py:38`, `:206`). | Conta assinaturas recorrentes A1–A11 (`:14`); A5 vem de órfãs da ata e requer repetição (`:125`, `:139`). Cron declarado às 08:30 com `--apply` (`app/config/crons.py:640`); comando lê ou lavra (`app/ponto/management/commands/esmeril_espelho.py:15`). | Não monta `dia['batidas']`, portanto não verifica fidelidade do impresso; seus insumos são células, turnos e ata (`app/ponto/services/esmeril_espelho.py:95`). |

O **R4** declara pares entre leitores e meta zero em cada par; a lista é tela/PDF, cartão/TXT, espelho/DiaPago, fechamento/soma, topo/linhas, app/tela e linhas/badge, sem um par “apuráveis × marcações impressas” (`app/core/placar_estrutural.py:211`). O item de “dia com batida apurável e sem DiaPago” aparece como **cura ainda necessária**, e diz respeito à lavratura, não ao espelho impresso (`app/core/placar_estrutural.py:406`).

## 17/11 15:10 · sessao ff67f8be2721 · - · - · origem=Ronald (tmux)

Leitores diretos de produção:

- `PerguntaDisputa.q_validada_pelo_ato`: lê a flag no predicado SQL que identifica RESPOSTA que limpa validação (`app/chamados/models.py:3547`); devolve `Q`, sem persistência (`app/chamados/models.py:3557`). Sua negação está em `app/chamados/models.py:3562`.
- `DecisaoPergunta._resposta_desfaz_validacao`: `ato == RESPOSTA and not resposta_mantem_validacao` (`app/chamados/models.py:4281`); usado para montar a sequência em `leitura` (`app/chamados/models.py:4299`). `validada = None` é variável local (`app/chamados/models.py:4312`), e a função devolve namedtuple (`app/chamados/models.py:4315`; definição em `app/chamados/models.py:19`).

Todos os consumidores diretos de `leitura` e da face SQL encontrados em produção ficam em `app/chamados/models.py`:

| Consumidor | Referência | Efeito |
|---|---|---|
| `_gravar_estado` consulta face SQL | `:317` | Seleciona perguntas para persistir lavra por `_lavrar_s2` (`:319`; grava campos em `app/chamados/veredito_lavrado.py:270` e `:275`). |
| `_ancoradas` consulta face SQL | `:357` | Retorna lista usada pelo gate de estado; o método apenas seleciona (`:343`). |
| `q_vivas` consulta face SQL | `:3609` | Retorna filtro SQL; não grava nesse método (`:3609`). |
| `ressuscitar_perguntas` | `:1684` | Usa leitura na foto da REABERTURA (`:1691`), registra ato (`:1696`), limpa projeção (`:1698`), acorda células (`:1701`) e reabre disputas (`:1708`). |
| `sem_veredito_entre` | `:2000` | Lista derivada (`:1999`), consumida por `pode_fechar` (`:2011`) e `fechar` (`:2044`). |
| `fechar` | `:2085` | Foto persistida no SILENCIO (`:2083`); evento da pergunta (`:2086`), grava fechamento de disputa (`:2095`) e evento (`:2096`). |
| `remover_motivo` | `:2217` | Texto persistido em RETRATACAO (`:2215`), carimba identidade (`:2220`) e salva disputa (`:2224`). |
| `estado` | `:3231` | Deriva rótulo (`:3232`). |
| `respondida` | `:3782` | Deriva booleano pelo juiz. |
| `resposta_display` | `:3806` | Deriva texto exibido. |

Portanto, o leitor central não desfaz `PerguntaDisputa.validada_em` no banco; seus consumidores incluem escritores reais, especialmente ressuscitação e fechamento (`app/chamados/models.py:4312`, `app/chamados/models.py:1698`, `app/chamados/models.py:2095`).

Caminhos concretos:

- **Request:** upload de foto chama kernel em `app/api/views.py:861` e `app/api/views_core.py:845`; kernel consulta `pode_fechar` e fecha (`app/api/foto_recebida.py:83`). Desarquivar é POST (`app/chamados/views_arquivar.py:56`), chama reabrir (`:70`) e ressuscitar (`:86`).
- **Signal:** `post_save` de PerguntaDisputa registra `on_commit` (`app/chamados/signals.py:229`, `:240`), que chama fechamento de beco (`:239`); classifica pelo juiz (`app/chamados/services/becos.py:34`) e fecha (`:57`). Salvar fechamento da disputa dispara receiver que tenta fechar pai (`app/chamados/signals.py:19`, `:43`).
- **Cron configurado:** consumidor a cada minuto (`app/config/crons.py:394`) chama reconciliação (`app/chamados/reconciliador.py:1368`), que consulta juiz e fecha (`:613`). Também chama encerramento de órfãs (`:1378`), cujo fechamento consulta leitores (`:1166`, `:1173`). `fechar_becos_disputa` está configurado com `--alarme` (`app/config/crons.py:663`), portanto só lê nessa configuração; a escrita exige `--apply` sem `--alarme` (`app/chamados/management/commands/fechar_becos_disputa.py:30`, `:54`, `:57`). Cron instalado e execução efetiva em produção: `[nao verificado]`.

**Nuance C6:** não lê esta flag: `projecao_temporal_da_pergunta` lê as três colunas da projeção (`app/chamados/juizes.py:1389`), e `_pergunta_viva_calc` decide com essa projeção (`:1404`). Diferenciar do filtro SQL novo `q_vivas`, que usa a sequência (`app/chamados/models.py:3609`).

**P1/P2:** para a flag isolada, backfill não tem corte de data/pk: alcança toda RESPOSTA de motivo não vazio existente na execução (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`). Coluna e backfill estão juntos atualmente (`:22`, `:28`); divisão exige separar operações, não reaplicar `AddField` (`:22`). `db_default=False` permite omissão da coluna no INSERT (`:26`), com teste que simula INSERT da main (`app/chamados/tests/test_f2_db_default_das_colunas_novas.py:25`), sem execução nesta auditoria.

**Risco P1 da obra inteira:** escritor novo grava REABERTURA compensadora (`app/chamados/models.py:1691`). A tarefa atual precisa ignorá-la antes de escolher último ato (`app/chamados/services/tarefa_do_dia.py:423`, `:426`), pois qualquer REABERTURA entregue ao corpo vira `devolvida_ao_colaborador` (`:163`). Assim, **se “leitor velho” significa carregar esses atos sem o filtro novo, P1 muda a fila do colaborador**, mesmo sem trocar o leitor da flag; é inferência demonstrável desses ramos, não verificação do pai indisponível. O teste declara main `fbc5c76ce` e comparação com dump (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:2`, `:10`), mas não fornece a fonte integral do pai.

## 17/11 15:10 · sessao 1c17ecf2b98a · - · - · origem=Ronald (tmux)

Censo dos escritores de produção:

| Escritor | Regra gravada | Comparação com 0055 |
|---|---|---|
| `responder_pergunta_disputa_core` | `False`; omite motivo, cujo default é `''`; zera `validada_em/por` quando havia validação. | Coincide: RESPOSTA sem motivo não entra no UPDATE. `app/chamados/services/disputa_emissao.py:1484`, `:1494`, `:1532`; `app/chamados/models.py:4162`. |
| `responder_automaticamente` | `True`, motivo fixo não vazio; salva somente resposta/texto, preservando validação. | Coincide, inclusive “resposta automatica”. `app/chamados/services/disputa_emissao.py:1682`, `:1685`, `:1686`. |
| `carimbar_resposta_pela_trilha` | `True`, motivo prefixado por `"pela trilha: "`; salva somente `respondida_em`. | Coincide mesmo com argumento `motivo=''`, pois o prefixo permanece. `app/chamados/services/disputa_emissao.py:1705`, `:1707`, `:1708`. |
| `linhas_da_projecao_sem_linha` | `True` para RESPOSTA faltante, motivo `MOTIVO_PROJECAO_SEM_LINHA`; VALIDACAO omitindo campo recebe `False`. | Coincide, inclusive o motivo especial. `app/chamados/models.py:4224`, `:4249`, `:4251`, `:4254`. |

Porta física: `registrar` instancia `cls(**ln)` e chama `objects.bulk_create`; não deduz booleano a partir do motivo (`app/chamados/models.py:4318`, `:4328`). Campo omitido recebe `default=False`, e INSERT antigo sem coluna recebe `db_default=False` (`app/chamados/models.py:4185`; `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:26`). Demais atos emitidos por essa porta recebem `False`; constraint proíbe `True` fora de RESPOSTA (`app/chamados/models.py:4205`).

`save` aceita nascimento e bloqueia regravação com PK (`app/chamados/models.py:4216`). `QuerySet.update` e `bulk_update` são bloqueados (`app/chamados/models.py:4115`, `:4121`). Não localizei outro create/save/update/bulk de produção do booleano; AST do tripwire, executado isoladamente com stdlib e `python3 -B`, retornou exatamente os quatro montadores acima; o próprio teste declara limite para montagem indireta (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:4`, `:7`, `:18`, `:59`).

Migration: 0055 cria com False e atualiza **todas** RESPOSTAS cujo motivo difere de `''`, sem recorte por PK/data/origem e sem checar valor anterior (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`, `:22`, `:28`). O model histórico via `apps.get_model` permite esse UPDATE, diferentemente do manager vivo (`:11`; `app/chamados/models.py:4115`).

0051 renomeia modelo/campos e atribui `ato='reabertura'` ao acervo; não cria RESPOSTA e não conhece o booleano (`app/chamados/migrations/0051_decisao_pergunta.py:41`, `:49`, `:61`). Seu RunPython só preenche chave de `PerguntaDisputa` (`:22`, `:25`, `:27`). Máscara da sombra pode usar `_base_manager.bulk_update`, mas inventário restringe DecisaoPergunta a texto/motivo, não ao booleano (`bin/sombra_mascara_pessoal.py:47`, `:62`; `bin/sombra_regras_pessoais.py:142`).

Ressalva essencial para Q2/P1: os quatro escritores vivos coincidem **por valor emitido**, mas o contrato do modelo não impõe equivalência universal com o predicado 0055. Há fixtures deliberadas gravando motivo preenchido+False e motivo vazio+True pela mesma porta; leitura segue campo (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`, `:143`, `:150`). Não são escritores de produção. A constraint permite ambas (`app/chamados/models.py:4205`). Em P2, UPDATE alteraria o primeiro caso para True; deixaria o segundo True, pois não faz reset dos não selecionados (`app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`).

## 17/11 15:09 · sessao 3072bb3d73be · - · - · origem=Ronald (tmux)

1. **NAO PROCEDE como contorno remanescente.** O fallback chama a porta pública (`app/ponto/management/commands/reconciliar_fantasmas.py:288`), que trava e abre a cerca (`app/chamados/services/disputa_emissao.py:1903`, `:1909`). A recusa é devolvida no dict e contada antes de continuar (`:1914`; `app/ponto/management/commands/reconciliar_fantasmas.py:291`).

2. **NAO PROCEDE para os caminhos nomeados.** Beco abre a cerca sobre resolver e fecho (`app/chamados/services/becos.py:55`, `:66`); HIST cerca costura e resolver (`app/chamados/reconciliador.py:1690`, `:1701`); S143 chama a porta pública (`app/chamados/signals.py:117`). As referências de produção ao corpo privado são as duas chamadas da porta (`app/chamados/services/disputa_emissao.py:1897`, `:1909`). A exceção `VIA_COMPETENCIA_TRANCADA` devolve sem escrita (`:1934`).

3. **PROCEDE como sobrealcance da porta.** `save` vigia antes de considerar `update_fields`, que só é encaminhado depois (`app/chamados/models.py:3572`, `:3578`). Consequentemente, consulta até data alterada exclusivamente em memória, embora o campo não seja persistido; também vigia `save(update_fields=[])` (`app/chamados/services/fio.py:1409`; `app/chamados/models.py:3576`). `descarimbar` não testa se os três campos já estão vazios (`app/chamados/models.py:3327`), e o lote não exclui linhas cujos valores já coincidem com os valores finais (`:3401`, `:3406`).

   Contraprova em memória, AST real dessas funções, imports/ORM/atomic/super simulados: `update_fields=[]` com pergunta ENTREGUE foi vetado antes do super; banco ABERTA + data ENTREGUE apenas em memória + `update_fields=['pergunta']` também foi vetado. Integração Django e ocorrência desse padrão dentro de retratação real: **[nao verificado]**.

6. **Ordem dos abridores:** retratação, beco e resolver travam antes do savepoint da cerca (`app/chamados/reconciliador.py:552`; `app/chamados/services/becos.py:52`; `app/chamados/services/disputa_emissao.py:1903`; abertura em `app/chamados/services/fio.py:1439`). HIST abre antes da trava feita internamente pela costura (`app/chamados/reconciliador.py:1701`, `:1691`; `app/chamados/services/fio.py:938`).

   **ESMERIL-SEMANTICA — contradição estática relevante:** a guarda chama `_dia_de_turno_da_pergunta`, que apenas retorna `data_evento` (`app/chamados/services/fio.py:1409`; `app/chamados/juizes.py:342`). O mesmo módulo declara que, para episódios, `data_evento` pode ser chave divergente do dia real; a identidade vem do juiz da batida (`app/chamados/services/fio.py:1605`, `:1620`). A migração reconhece explicitamente dia de saída diferente do dia do turno e deixa fora grupos com qualquer dia exportado (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:17`, `:30`). Portanto, normalização universal não está garantida pelo código: turno D23 ENTREGUE com chave D24 ABERTA permite consulta apenas da competência ABERTA; a relavra S2 de pergunta materializada continua alcançável (`app/chamados/models.py:305`; `app/chamados/veredito_lavrado.py:275`). Existência dessa linha em produção: **[nao verificado]**.

   **ContextVar:** há estado novo por abertura e reset em `finally`, inclusive com erro (`app/chamados/services/fio.py:1435`, `:1448`); aninhamentos reutilizam o contexto externo (`:1433`). AST real com atomic simulado confirmou reset após sucesso, exceção e veto engolido; thread nova observou `None`. Vazamento real entre requests/testes: **[nao verificado]**.

   **Relógio/juiz:** funções novas não introduzem leitura de relógio; a competência continua composta pelo veto existente e pelos juízes da janela/K6 (`app/chamados/services/fio.py:1358`, `:1389`; `app/chamados/services/vigia_fio.py:800`). Os `now()` do resolver e da lavra permanecem em `app/chamados/services/disputa_emissao.py:2019`, `:2076` e `app/chamados/veredito_lavrado.py:271`.

   **Limite adicional a investigar:** HIST avulso pode fazer da cerca a transação externa; `_CERCA` só é resetada após sair do atomic (`app/chamados/reconciliador.py:1701`; `app/chamados/services/fio.py:1439`, `:1449`). Há callbacks `on_commit` que chamam beco/regularizações (`app/chamados/signals.py:240`, `:146`). Uma recusa tardia nesses callbacks não teve cenário completo demonstrado: **[nao verificado]**.

## 17/11 15:09 · sessao f14db117992d · - · - · origem=Ronald (tmux)

Q4 histórico exato: `[nao verificado]`. A cópia não tem `.git` nem fonte identificável do pai; comentários/testes documentam MAIN sem colunas, mas não provam seu censo completo (`app/chamados/tests/test_f2_db_default_das_colunas_novas.py:1`).

Censo comprovável na árvore atual:

- Automática: grava RESPOSTA, motivo constante não vazio `'resposta automatica (fato entregue por outro caminho)'`, flag True (`app/chamados/services/disputa_emissao.py:1686`). Request POST upload: `app/api/views.py:837`, chamada `:860`; equivalente `app/api/views_core.py:821`, chamada `:844`; kernel chama `_validar_pelo_dono` (`app/api/foto_recebida.py:74`), que chama escritor (`:48`) e validador (`:51`).
- Trilha: RESPOSTA, motivo `'pela trilha: %s'`, flag True (`app/chamados/services/disputa_emissao.py:1708`). Comando `curar_perguntas_em_limbo --apply` (`app/chamados/management/commands/curar_perguntas_em_limbo.py:64`, `:67`). Classificado como ato único, não rotina (`app/config/crons.py:1482`).
- Foto da projeção sem linha: RESPOSTA com `MOTIVO_PROJECAO_SEM_LINHA` (`app/chamados/models.py:4224`), flag True, se existe `respondida_em` sem correspondência `(pergunta, RESPOSTA, instante)` (`:4249`, escrita `:4251`). Helper apenas monta dicionários; persistências atuais:
  - nascimento `_nascer`: `app/chamados/models.py:3202`, `:3207`;
  - ressuscitação: `app/chamados/models.py:1650`;
  - resposta humana: `app/chamados/services/disputa_emissao.py:1480`, `:1532`;
  - resposta automática: `app/chamados/services/disputa_emissao.py:1681`, `:1686`;
  - validação: `app/chamados/services/validacao.py:167`, `:173`;
  - devolução: `app/chamados/services/validacao.py:272`, `:279`;
  - BUG-PAR: `app/chamados/services/validacao.py:345`, `:354`;
  - estoque L130: `app/chamados/services/validacao.py:436`, `:439`;
  - backfill E4b: `app/chamados/management/commands/backfill_fio_morto_e4b.py:138`, `:141`, classificado sob ordem, sem rotina (`app/config/crons.py:797`).

Ressuscitação tem request explícito em `app/chamados/views_arquivar.py:52`, `:86`, também via `reabrir` (`app/chamados/models.py:1510`), e cartório (`app/ponto/services/cartorio.py:1674`). A cadeia histórica exata por signal/cron no pai permanece `[nao verificado]`.

`backfill_decisoes_pergunta` **só mede** as linhas RESPOSTA do helper: `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `:67`; seu apply explicitamente não escreve essas linhas (`:3`). Documentação histórica de regras/fluxos pode apoiar inferência, mas não substitui o pai ausente: `[nao verificado]`.

## 17/11 15:08 · sessao a24eba6cf524 · - · - · origem=Ronald (tmux)

Item 5 — censo e mapa dos campos

O censo estático executado em memória (`python3 -B`, sem Django) encontrou um `update` atribuível a PerguntaDisputa em `app/chamados/models.py:3406` e um `bulk_update` em `app/chamados/veredito_lavrado.py:250`; o primeiro chama a vigilância em `app/chamados/models.py:3401`, o segundo não chama. Os únicos chamadores encontrados de `gravar_lavras_em_lote` são o backfill em `app/chamados/management/commands/backfill_veredito_lavrado.py:143` e `:146`.

| Modelo / campos relevantes | Escritores concretos | Leitores relevantes do diff/caminho |
|---|---|---|
| PerguntaDisputa `disputa`, `motivo`, `celula`, `conversa`, `chamado_destino` no nascimento | `_nascer`, `app/chamados/models.py:3134`, persistência `:3137` | Guarda lê `conversa`/`disputa` em `app/chamados/services/fio.py:1403`, `:1408`, `:1414`; resolver seleciona destino/disputa/célula em `app/chamados/services/disputa_emissao.py:1939`; HIST lê motivo/disputa em `app/chamados/reconciliador.py:1602`, `:1605`, `:1649` |
| PerguntaDisputa `chamado_destino`, `celula` no reuso | `app/chamados/models.py:3086`, `:3102`, save `:3126` | `_ancoradas`, `app/chamados/models.py:344`; HX, `app/chamados/services/disputa_emissao.py:2069` |
| PerguntaDisputa `chamado_destino` na costura | `app/chamados/services/fio.py:974`, save `:976` | Resolver direto `app/chamados/services/disputa_emissao.py:1940`; HX `:2069`; HIST `app/chamados/reconciliador.py:1691` |
| PerguntaDisputa `data_evento` | Atribuição única `app/chamados/services/fio.py:849`; nascimento chama em `app/chamados/models.py:3136`; costura em `app/chamados/services/fio.py:928` | Vigilância lê memória e banco em `app/chamados/services/fio.py:1403`, `:1409`, `:1411`; juiz do dia em `app/chamados/juizes.py:342`; resolver em `app/chamados/services/disputa_emissao.py:2019`, `:2091`; HIST em `app/chamados/reconciliador.py:1612` |
| PerguntaDisputa `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição do trio `app/chamados/models.py:3220`; `carimbar` salva em `:3273`, identidade em `:3311`, descarimbar em `:3333`; lote `:3406` | Seleção direta `app/chamados/services/disputa_emissao.py:1941`; HX `:2073`, fotografia/trilha `:2083`; filtro do lote `app/chamados/models.py:3402`; guarda executa antes do save em `:3576` |
| PerguntaDisputa `respondida_em`, `resposta_colab` | Resposta humana `app/chamados/services/disputa_emissao.py:1481`, save `:1522`; automática `:1674`, save `:1677`; reparo pela trilha `:1696`, save `:1698`; preenchimento de instante pela porta `app/chamados/models.py:3253`; limpeza por lote `:3408` com `**outros` | C21 `app/chamados/juizes.py:1552`; resolver direto `app/chamados/services/disputa_emissao.py:2013`; HX `:2085`, `:2090`; porta de par `app/chamados/models.py:3603` |
| PerguntaDisputa `validada_em`, `validada_por` | Validação `app/chamados/services/validacao.py:168`, save `:171`; devolução `:273`, save `:276`; resposta nova limpa em `app/chamados/services/disputa_emissao.py:1494`; auto-validação `app/chamados/models.py:3256`; descarimbo/lote pode limpar via `**outros`, `:3408` | `_ancoradas` `app/chamados/models.py:344`; relavra S2 `:310`; HX `app/chamados/services/disputa_emissao.py:2073`; resposta nova `:1484` |
| PerguntaDisputa `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar`, `app/chamados/veredito_lavrado.py:270`, save `:275`; backfill atribui em `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, persiste pelo bulk `app/chamados/veredito_lavrado.py:250` | `esta_stale`, `app/chamados/veredito_lavrado.py:224`, `:228`, `:232`; foto `:281`; testes leem `.values()` em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:258` |
| ChamadoColaborador `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por`, `urgencia`, `urgencia_original`, `atualizado_em` | `_gravar_estado`, `app/chamados/models.py:228`, `:231`, `:244`, `:245`, `:246`, `:251`, persistência `:296`; urgência via update `:637` | Resolver tranca `app/chamados/services/disputa_emissao.py:1896`, `:1934`; HX lê status `:2040`, `:2066`, `:2069`; S143 `app/chamados/signals.py:97`; recarregar estado `app/chamados/models.py:759`, `:761` |
| ChamadoColaborador `colaborador`, `justificativa_relacionada`, `categoria`, `subcategoria`, `contexto_json` | Nascimento/reuso pelo modelo; todos os escritores desses campos na árvore: **[nao verificado]** | Ato lê colaborador/justificativa em `app/chamados/reconciliador.py:550`, `:573`; HIST lê contexto/categoria/subcategoria `:1633`, `:1634`, `:1642`; callback do filho lê contexto `app/chamados/signals.py:361` |
| DisputaSupervisao `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | `fechar`, `app/chamados/models.py:2023`, save `:2027`; reaberturas também escrevem — enumeração completa **[nao verificado]** | Beco `app/chamados/services/becos.py:59`; resolver `app/chamados/services/disputa_emissao.py:2063`, `:2124`; Defesa 1 `app/chamados/signals.py:29`, `:46` |
| DisputaSupervisao `colaborador`, `chamado`, `contexto`, `motivos` | Emissão/reuso tem múltiplos ramos: `app/chamados/services/disputa_emissao.py:550`, `:576`, `:1097`, `:1145`, `:1269`, `:1380`; modelo modifica motivos/contexto em `app/chamados/models.py:2057`, `:2154`, `:2195`, `:2250` | Resolver `app/chamados/services/disputa_emissao.py:2061`, `:2065`; HIST contexto `app/chamados/reconciliador.py:1605`; beco pai `app/chamados/services/becos.py:44`; guarda colaborador `app/chamados/services/fio.py:1414` |
| LogAuditoria `acao`, `modelo`, `objeto_id`, `descricao`, `valor_antes`, `valor_depois`, `usuario`, `ip`, `ator` | `core.observ.evento`, criação em `app/core/observ.py:134`; adaptação `evento_kw` em `:146`; retenção prepara payload e chama em `app/chamados/reconciliador.py:478`, `:493` | Dedup da retenção filtra ação/modelo/objeto e lê `valor_depois`, ordenando id, em `app/chamados/reconciliador.py:489`; fotografia de teste em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:219` |

PKs/IDs são lidos na guarda (`app/chamados/services/fio.py:1404`) e nos filtros; o nascimento usa PK gerada pelo modelo, sem atribuição explícita no trecho (`app/chamados/models.py:3134`). Enumeração de todos os leitores da árvore, inclusive apresentação e ferramentas, **[nao verificado]**; a tabela nomeia os leitores relevantes à alteração.

**Bypass real:** `gravar_lavras_em_lote` não passa por `vigiar`; a ausência é concreta em `app/chamados/veredito_lavrado.py:247`. Não encontrei caminho dos atos cercados até esse bulk; seus chamadores encontrados são exclusivamente `backfill_veredito_lavrado.py:143` e `:146`. Não foi demonstrada escrita SQL/raw de PerguntaDisputa; ausência absoluta de escrita dinâmica **[nao verificado]**.

Item 8 — matriz

| Sítio | Cobertura e evidência |
|---|---|
| 1 retratar_com_motivo | Abre cerca `app/chamados/reconciliador.py:553`, após trava `:552`, antes do corpo/transição `:567` |
| 2 fechar_beco | Abre cerca `app/chamados/services/becos.py:66`, após trava `:52`; callback inclui resolver e disputa `:57`, `:60` |
| 3 resolver público | Cerca `app/chamados/services/disputa_emissao.py:1909`, após trava `:1903` |
| 4 reconciliar | Herda quando aninhado; avulso chama `_gravar_estado` em `app/chamados/models.py:655` e relavra em `:312`, sem abrir cerca própria |
| 5 transicionar / 6 encerrar | Convergem no escritor de estado; save e relavra `app/chamados/models.py:296`, `:312`; cobertura apenas herdada |
| 7 encerrar_por_competencia_trancada | Via especial do resolver retorna sem escrita em `app/chamados/services/disputa_emissao.py:1934` |
| 8 reabrir | Escritas por descarimbo ficam guardadas quando há contexto; lote vigia em `app/chamados/models.py:3401`, persiste `:3406` |
| 9 Disputa.fechar | Escreve perguntas mudas em `app/chamados/models.py:2012`, disputa em `:2027`; cerca herdada nos atos, avulso permanece fora |
| 10 absolvidas pela célula | Carimba em `app/chamados/reconciliador.py:639`, fecha disputa em `:656`; avulso não abre cerca |
| 11 reatar órfã | HIST cercado em `app/chamados/reconciliador.py:1701`; c1 chama `lavrar_sem_turno_aberto` em `:1664`; derivável costura fora em `:1712` |
| 12 órfãs pela lâmpada | Carimba `app/chamados/reconciliador.py:1192`, fecha disputa `:1216`; sem abridor próprio |
| 13 reapontar | Save guardado quando aninhado em `app/chamados/services/fio.py:976` |
| 14 lavrar | Save guardado quando aninhado em `app/chamados/veredito_lavrado.py:275` |
| 15 gravar_lavras_em_lote | Bulk sem vigilância, `app/chamados/veredito_lavrado.py:250`; backfill avulso `app/chamados/management/commands/backfill_veredito_lavrado.py:143` |

**Caminho temporal que merece achado nos itens 7–8:** a transição cercada pode registrar o callback `_filho_morto_acorda_o_pai`, por `transaction.on_commit(_reconciliar)` em `app/chamados/signals.py:371`; o callback chama `reconciliar_chamados_colab` em `:370`, que chama **antes** de qualquer retratação própria `retratar_perguntas_absolvidas_por_celula` e `reconciliar_perguntas_orfas_colab` em `app/chamados/reconciliador.py:1477`. A primeira grava pergunta em `:639`; a segunda pode seguir c1 em `:1664` ou derivável em `:1712`. No fluxo com atomic externo de retratar (`:543`), o savepoint da cerca termina antes desse commit e `_CERCA` é restaurado em `app/chamados/services/fio.py:1449`. Portanto, essas escritas diferidas não herdam a cerca que originou o callback, e a guarda retorna cedo em `:1397`.

O selo anexo liga callbacks como arestas estáticas sem modelar a execução após commit; esse limite temporal não está declarado entre os limites descritos no anexo **selo_diff.txt: `_liga_corpo` / `_via`**. A ocorrência com dados reais e uma pergunta ENTREGUE elegível nesse callback é **[nao verificado]**; o caminho de código está presente nos pontos acima. [nao verificado]

## 17/11 15:08 · sessao 3923d3e3219f · - · - · origem=Ronald (tmux)

Item 4 — **PROCEDE parcialmente: mordida específica foi corrigida, mas há expectativa incoerente e mutante sem mordida.**

- As recusas conferem a linha completa da pergunta pelo PK antes das fotografias restantes; a classificação ALVO também confere o PK na mensagem. Portanto a mordida específica está implementada. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:249-267,523-540`.
- `sem_escopo:fechar_beco = {}` é coerente com o caso construído: ele tenta resolver a pergunta direta ENTREGUE; retirado apenas o escopo do beco, o resolver interno conserva sua própria cerca. Entretanto o teste aceita explicitamente que retirar a cerca do beco não derrube caso nenhum; falta prova da proteção adicional do `d.fechar`. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:435-448,550-563`; `app/chamados/services/becos.py:55-66`; `app/chamados/services/disputa_emissao.py:1909-1914`.
- **`engolido_sobrevive -> e=ALVO` está errado pelo fluxo.** O mutante conserva a guarda anterior ao save e captura a exceção propagada do callback, retornando vetadas. No caso e, a pergunta validada não entra na relavra do chamado; o carimbo direto do resolver encontra a guarda, que levanta antes da persistência. O wrapper recebe vetadas e registra a trilha. O desenho deste caso conduz a **verde**, não ALVO: não existe aqui uma exceção engolida dentro do callback do resolver. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:84-99,418-433,565`; `app/chamados/services/disputa_emissao.py:1909-1914,2019-2020`; `app/chamados/models.py:3270-3277,3572-3581`. Execução integral dos testes: **[nao verificado]**.
- O caso s tem C21 completo, com texto não vazio, instante e validação. `app/chamados/tests/test_e4c_b2_cerca_retratar.py:382-384`.
- O relatório do geofence usa os mesmos três motivos que a saída imprime; o caso g exige a saída particionada. `app/ponto/services/geofence.py:444-448`; `app/ponto/management/commands/reconciliar_geofence.py:46-49`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:477-478`.

Item 7 — **PROCEDE: o selo tem subaproximação concreta não declarada e suas verificações estruturais não demonstram todos os requisitos.**

1. `_indexa` e `_liga` do anexo só entram em statements diretamente FunctionDef/AsyncFunctionDef/ClassDef; não atravessam If/With/Try para descobrir definições aninhadas. `_liga_corpo`, por sua vez, cria referências para essas definições via `ast.walk`, mas exclui seus corpos do chamador. O resultado são callbacks apontando para IDs sem definição; `vermelhos` ignora callbacks ausentes que não sejam SITIOS. Evidência: `selo_diff.txt` anexo, métodos `_indexa`, `_liga`, `_liga_corpo` e trecho `if s not in g.defs` **[linhas do anexo nao verificadas]**.
2. Dois exemplos reais:
   - `_ato` de fechar_beco está dentro do With; desaparecem suas chamadas aos resolvers e ao `d.fechar`. `app/chamados/services/becos.py:42,55-60,66`.
   - `_costura_e_fecho` está dentro de `if ch is None`; desaparecem `reapontar_resolvedora` e resolver. `app/chamados/reconciliador.py:1603,1690-1696,1702`.
   - Fiz reprodução somente AST, em memória: o algoritmo de indexação do selo encontra zero definições `_costura_e_fecho`, embora o AST real a encontre na linha 1690. Não houve banco, teste Django nem arquivo criado. **[nao verificado: resultado de ferramenta sem artefato persistido]** [nao verificado]
3. Outros limites não descritos:
   - a1 conta chamada dentro de `if False` antes da persistência; não prova passagem efetiva pela guarda.
   - a2 procura qualquer If contendo Raise; não confere que o teste seja `vetadas`. [nao verificado]
   - a3 procura qualquer With contendo If/Raise; não confere `transaction.atomic` nem a condição de veto.
   - a4 exige que o nome das vetadas apareça em algum If; não demonstra que isso devolva/publique a recusa.
   Essas propriedades decorrem literalmente dos predicados de `vermelhos` e `_abridor_ok` do anexo. **[linhas do anexo nao verificadas]**
4. Os 14 nomes enumerados são compatíveis com a própria classificação declarada; a confirmação da quantidade calculada executando o selo completo permanece **[nao verificado]**. A via trancada explica dois vermelhos conservadores: wrapper chama o corpo sem cerca, e o corpo retorna antes de qualquer escrita. `app/chamados/services/disputa_emissao.py:1896-1897,1931-1935`.
5. A adaptação substitui consulta prévia por rollback; ela não demonstra o instante literal do aval. A implementação chama transição dentro do callback e só consulta na porta do save. `app/chamados/reconciliador.py:553,569-574`; `app/chamados/models.py:3572-3581`.

_6619 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
