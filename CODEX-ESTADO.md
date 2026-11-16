# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 08:55** · publicado em **16/11/2026 08:55** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 08:55 · sessao 30eec1e06ad0 · LOTE3-C7 · verificador · origem=Code

**O delta fecha o mutante da volta 2. Não identifiquei sobrevivente plausível na família pedida para a falha injetada; a implementação atual mantém porta e trilha no mesmo `atomic`, com o `except` fora.** (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:246-264`; `app/colaboradores/views.py:333-343`). Execução dos mutantes: **[nao verificado]**.

1. **Poder de distinção do teste** [nao verificado]

| Mutante | Por que o teste o rejeita, por análise estática | [nao verificado]
|---|---|
| Salvar → trilha, sem atomic comum; atomic só na porta ou só na trilha | A porta deixa o posto novo gravado; a consulta final exige o antigo. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-257`). |
| Trilha → porta, inclusive o m3 com atomic somente na trilha | A leitura durante a trilha encontra o posto antigo; a nova asserção exige o novo. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:247-253`). |
| Atomic envolvendo o `try/except`, capturando a falha internamente sem provocar rollback | O `RuntimeError` ocorre depois do INSERT real; capturá-lo sem desfazer as escritas deixa FK/contagem incompatíveis com as verificações finais. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:248-259`). |
| Atomic somente no tratamento do erro | Não desfaz as escritas anteriores; falham FK ou contagem finais. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/services/historico_vinculo.py:13-17`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-259`). |

Isso não constitui prova universal da estrutura: o snapshot exige o **efeito da gravação**, sem verificar diretamente a chamada de `salvar_inteiro`. Substituí-la por `colaborador.save()` dentro do atomic comum preservaria os efeitos deste teste, removendo a guarda; a recusa é exercitada pelo teste separado. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:211-228`, `:246-264`; `app/colaboradores/services/colaborador.py:158-160`).

2. **Leitura dentro do substituto**

No caminho mostrado, ela acrescenta uma consulta e guarda um inteiro numa lista; não altera o objeto usado pela view nem executa gravação ou comando de rollback. A escrita real e o `RuntimeError` continuam depois dessa consulta. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:246-249`).

**Não há verde por uma asserção engolida:** as asserções ficam depois do POST. Se o `get` falhar, a chamada real não chega ao `append`, e `len(escritas) == 1` falha; se a trilha retornar `None`, outra asserção falha; o toast também exige exatamente a exceção planejada. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:247-264`).

A leitura observa a gravação visível nessa conexão, sem provar commit. O `TestCase` envolvente também não oculta escritas próprias que permaneceram após o POST, pois FK e contagem são consultadas antes do término do teste. (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:253-259`). Outra conexão, concorrência e efeitos de timing: **[nao verificado]**.

3. **Triagem e pouso** [nao verificado]

A correção sobre a releitura procede: o gancho altera somente situação/data/motivo, e o delta observa o posto durante a trilha. Esses testes continuam compatíveis com restaurar `_posto_antes` em memória no tratamento do erro; não distinguem essa restauração da releitura atual. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:40-43`, `:216-228`, `:247-253`; `app/colaboradores/views.py:343`).

Os demais itens têm fundamento no código:

| Item mantido em fila | Evidência |
|---|---|
| Homônimos podem trocar FK sem trilha | Comparação textual e representação por nome/empresa. (`app/colaboradores/services/historico_vinculo.py:10-11`; `app/colaboradores/models.py:276-277`). |
| Anterior potencialmente obsoleto | Capturado antes da trava; a guarda relê situação. (`app/colaboradores/views.py:326-334`; `app/colaboradores/services/colaborador.py:65-66`, `:89-91`). |
| `sem_posto` repetido | Dois ramos, painel e pendência registrada. (`app/colaboradores/views.py:346`, `:365`; `app/templates/colaboradores/partials/painel_vinculo.html:34`; `app/core/juizes.py:931-932`). |
| Datas com sentidos distintos | Dia atual, início do vínculo e demissão agendada. (`app/colaboradores/views.py:335`; `app/colaboradores/services/vinculo.py:354`; `app/colaboradores/services/desligamento.py:113-116`). |
| Trava adicional por matrícula ausente | Ramo condicional do `save`. (`app/colaboradores/models.py:427-438`). |

**Para o contrato “falha excepcional da trilha desfaz posto e trilha”, nenhum desses itens demonstra bloqueio funcional do pouso atual.** (`app/colaboradores/views.py:333-343`). O aval integral está apenas referenciado no código; conformidade com exigências além do escopo descrito no anexo, preexistência dos passivos e resultados alegados dos logs: **[nao verificado]**. (`app/colaboradores/views.py:331`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:231`).

## 16/11 08:55 · sessao 7519f15333fc · - · - · origem=Ronald (tmux)

Contando somente a marca `[DEPENDE-F1]` explícita na linha de cada degrau:

| Matriz | PROVA=NENHUM | Com marca | Sem marca |
|---|---:|---:|---:|
| F2, integrais na coluna c3 | 10 | 0 | **10** |
| F2, incluindo parciais | 14 | 0 | **14** |
| F4 | 21 | 3 | **18** |
| F6 | 11 | 8 | **3** |

Fontes: anexo F2/matriz.md:100-102; F4/matriz.md:126 e :130; F6/matriz.md:94 e respectivas linhas dos degraus abaixo. Numeração começa no primeiro `#` de cada anexo e inclui linhas vazias.

- **F2 integrais:** M1b (:37), M4 (:40), M7 (:43), M11 (:47), M13 (:49), M14 (:50), M15 (:51), M17 (:53), M18 (:54), M19 (:55). Parciais adicionais: M3 (:39), M9 (:45), M10 (:46), M20 (:56). A seção final distingue explicitamente “nenhuma mordida” de “algum ramo sem mordida” (:101); a coluna BASE é outro universo (:102).
- **F4 sem marca:** M1, M2, M13, M16, M17, M19, M23, M24; E4, E5, E7, E17, E18, E19, E20; X1, X2; J2. Os três NENHUM marcados são M9b (:42), E9 (:71) e J1 (:90). A lista completa dos21 aparece em :126.
- **F6 sem marca:** U9b (:44), S1 (:48), S2 (:49). S1/S2 trazem “SOME do escopo F6”; excluí-los daria **1**, mas o item9 não determina essa exclusão. A lista declarada dos11 inclui ambos (:94).

Ausência de marca na linha **não demonstra independência**: F2 tem dependências numa seção separada (:69-77), abrangendo M1-M4, M8-M10 e N1/M7; F4 marca dependência no contrato (:104-109), inclusive para M1/M2/M13/M16/M17 e M23, que não receberam a marca na própria linha.

Portanto, a seleção literal do item9 precisa distinguir coluna BASE/c3, integrais/parciais, dependência da linha/dependência do contrato e os degraus F6 excluídos do escopo. As matrizes fornecem as categorias, mas não uma seleção única inequívoca para “antes do pouso da F1”. (Anexo F2/matriz.md:69, :99-102; F4/matriz.md:104-109; F6/matriz.md:48-49, :94.)

## 16/11 08:54 · sessao 4d65f7eb3223 · - · - · origem=Ronald (tmux)

Não encontrei sobrevivente funcional plausível na família restrita solicitada, por análise estática: o teste exige posto novo durante a trilha, escrita real e, após a exceção, posto antigo, contagem original, HTML antigo e toast exato. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:246-264`).

| Mutante | Por que falha |
|---|---|
| Salvar → trilha, sem atomic comum; atomic só na porta; atomic só na trilha após porta | A porta encerra seu bloco próprio e deixa o posto novo; a consulta final exige o antigo. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-257`). |
| Trilha → porta, com ou sem atomic somente na trilha | A leitura durante a trilha encontra o antigo; o teste exige o novo. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:247`, `:253`). |
| Capturar o `RuntimeError` dentro do único atomic sem marcar rollback | As duas escritas permanecem; as consultas finais exigem desfazimento de ambas. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:248-249`, `:256-259`). |
| Atomic envolvendo somente o tratamento do erro | As escritas anteriores ficam preservadas e violam FK/contagem finais. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/services/historico_vinculo.py:13-17`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-259`). |

A implementação atual confirma atomic comum, porta antes da trilha e `except` fora do bloco. (`app/colaboradores/views.py:333-343`). Execução desses mutantes: **[nao verificado]**.

Limite: este teste sozinho não comprova passagem por `salvar_inteiro`; não há spy nessa função. Substituí-la por `colaborador.save()` dentro do mesmo atomic preservaria os efeitos examinados, mas removeria a guarda. A guarda é exercitada pelo teste separado de desligamento durante a carga. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:211-228`, `:246-264`; `app/colaboradores/services/colaborador.py:158-160`). Esse bypass é um mutante fora da família estrita perguntada. (`app/colaboradores/views.py:334`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:250-264`).

## 16/11 08:54 · sessao eb897fb79a41 · - · - · origem=Ronald (tmux)

Parecer sobre a pergunta 3: **não há colisão substantiva demonstrada pelo material disponível**; a lacuna operacional do wrapper deve ser tratada na pergunta 1. A lei permite expressamente testes RED em cópia própria fora do papel construtor, sob contrato e revisão (`CLAUDE.md:349-350`; `app/docs/PISTA.md:143-145`).

- O critério de colisão é **arquivo**, inclusive arquivo de teste; o item 9 já manda esperar quando cruzar F1/R3/BUG-PAR (`CLAUDE.md:309-310`; anexo `cortes_f2_f9_1611.txt`, item 9). [nao verificado]
- O registro disponível confirma cruzamentos da BUG-PAR com F1 em `fio.py`, `test_s139.py`, `test_medida_turno_do_par.py` e `JUIZES.md`; a construção da volta aguardava o pouso da B1 (`app/docs/RELATO.md:14`, `:25`, `:33`). O anexo COMPLEMENTO-2 preserva essa espera e autoriza apenas ensaio de leitura.
- **Não certificar ausência de cruzamento com LOTE7 c7/LOTE3 c7**: o registro da cópia descreve c6 e seus arquivos, enquanto o usuário informa c7; os deltas atuais das cópias não foram fornecidos (`app/docs/RELATO.md:4`, `:7`, `:12-13`; estado c7 **[nao verificado]**).
- A lei vigente **L-130 substituiu JUIZ-RESPONDIDA e F5/F6(2)**: “respondida” exige instante **e** texto; pares parciais são ilegais (`app/docs/LEIS.md:151`). Portanto, matrizes históricas com P5/texto sem instante precisam ser lidas à luz dessa lei. Não é prova de colisão de um teste ainda não escrito: um caso ilegal pode ser entrada válida para testar a recusa.
- Há uma precondição normativa ausente do item 9: RED em cópia própria exige contrato **antes do primeiro uso**, com cinco linhas e revisão do instrumento (`app/docs/PISTA.md:144-145`, `:170-175`; `CLAUDE.md:394`). Existência de contrato anterior: **[nao verificado]**.

Não atribuir ao veto antigo das matrizes uma colisão automática: o novo aval autoriza especificamente construir **só testes**, preservando a espera por dependências e cruzamentos (anexo `cortes_f2_f9_1611.txt`, item 9). [nao verificado]

## 16/11 08:54 · sessao 8cece979ddda · LOTE7-C7 · verificador · origem=Code

**O c7b fecha a preservação dos campos acrescentados, mas não demonstra rollback após tentativa de escrita nem frescor temporal “neste ato”.** O descarte do mutante que levanta antes de escrever precisa ser restringido; o teste positivo não elimina sua variante condicional. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:252`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`)

Resultados executados de testes/mutantes, logs, igualdade com árvores anteriores e estado de produção: `[nao verificado]`. As conclusões abaixo são estáticas, sobre a cópia e o delta fornecido. [nao verificado]

Nos achados marcados **PROCEDENTES**, o fechamento é o seguinte:

| Achado da v1 | O delta fecha? |
|---|---|
| Item 1: apagar ausências ou alterar outros campos e levantar a exceção esperada | **Sim, para o conjunto e os campos enumerados.** O teste fotografa PK, tipo, status, data, fim, minutos e descrição antes da chamada e exige igualdade depois. “Linha INTEIRA” continua excessivo: ficam fora, por exemplo, `dias_corridos`, `hora_ini` e `hora_fim`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:238`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:252`; `app/ponto/models.py:761`; `app/ponto/models.py:768`; `app/ponto/models.py:770`) |
| Item 4: campos omitidos em `_retrato` | **Sim, para os campos acrescentados.** Agendamento passa a incluir início/fim/dias; ausências incluem tipo/fim/minutos/descrição; trilhas antigas incluem descrição. A comparação continua limitada às PKs antigas da auditoria. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:523`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:546`) |
| Item 4: timestamp incorreto, porém não nulo | **Parcialmente.** O filtro agora rejeita valores anteriores a `instante`, mas o relógio congelado permite usar o instante da criação da célula e ainda passar; detalhes abaixo. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:60`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:454`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:463`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`) |
| Item 7: comentário sobre competência futura | **Fecha o trecho alterado.** O comentário passa a concordar com o teto que considera a última competência carimbada. Entretanto, permanece a afirmação contrária no comentário da edição. (`app/ponto/services/ausencia.py:1119`; `app/ponto/services/ausencia.py:147`; `app/ponto/services/ausencia.py:151`; `app/ponto/services/ausencia.py:665`) |
| Item 3: “regularização, só leitura” | A formulação corrigida no anexo é compatível se qualificar somente o acréscimo dos ganchos. O serviço grava Justificativa e pode criar ausências; não é integralmente de leitura. (`app/ponto/services/regularizacao.py:106`; `app/ponto/services/regularizacao.py:118`; `app/ponto/services/ausencia.py:597`) |

**Mutante sobrevivente principal:** mover `_ev_rej(...)` para antes de `a.save(...)`, mantendo a exceção quando a auditoria retorna `None`. No sucesso, continua salvando as cinco rejeições e criando as trilhas; na falha, levanta sem tentar salvar a ausência. Os negativos fornecidos fazem a primeira auditoria de rejeição falhar e não exigem tentativa nem ordem de escrita. Portanto, o positivo e os testes alterados não distinguem esse mutante do código atual. Sobrevivência inferida; execução `[nao verificado]`. (`app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1499`; `app/ponto/services/ausencia.py:1506`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:216`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:531`)

Uma variante mais grave também fica sem testemunha nesses testes: inverter auditoria/save **e retirar o `atomic` local**. A falha na segunda auditoria poderia deixar a primeira rejeição persistida numa chamada direta, enquanto os negativos atuais falham na primeira. O cancelamento ainda teria sua transação externa. Sobrevivência inferida; execução `[nao verificado]`. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1494`; `app/ferias/services.py:674`; `app/ferias/services.py:683`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:532`)

Para os **DESCARTADOS**, os motivos se sustentam nestes limites:

| Descarte | Verificação |
|---|---|
| “Levantar antes de escrever é morto pelo positivo” | **Somente para `raise` incondicional.** O mutante condicional acima sobrevive por leitura. Além disso, o positivo citado exige retorno `5` e cinco trilhas, mas não conta diretamente cinco ausências rejeitadas; essa contagem está no teste da célula. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:217`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:219`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:468`) |
| Trilha nova indevida permitida deliberadamente | **Coerente com o contrato escrito**, que preserva apenas as antigas. Contudo, o teste permite qualquer inserção nova, sem verificar que represente recusa legítima. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:517`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:546`) |
| Ramo com adesão fora do escopo | É uma delimitação de escopo; a lacuna permanece. A fixture percorre `nunca_bateu`, cujo ramo retorna antes do processamento normal. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:460`; `app/ponto/services/cartorio.py:1317`; `app/ponto/services/cartorio.py:1318`) |
| Veredito/ata fora do escopo | Também delimita escopo, sem demonstrar esses conteúdos. Os asserts verificam atividade, status, carimbo e FK, não veredito ou ata. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:466`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:472`) |
| Relógio, D e falha de invalidação sem retentativa estão em fila | Os mecanismos continuam presentes: leitura interna de relógio, julgamento sob `engolir` sem savepoint nesse wrapper, invalidação que retorna zero após erro e repetição que ignora insumos iguais. Antiguidade histórica e gestão efetiva da fila: `[nao verificado]`. (`app/ponto/services/ausencia.py:184`; `app/ponto/services/ausencia.py:191`; `app/ponto/signals.py:88`; `app/core/observ.py:86`; `app/ponto/services/fechamento.py:1661`) |
| Selo é apenas piso; testes comportamentais cobrem mapas vazios | **Sustenta-se para silenciar globalmente a invalidação.** O positivo exige fechamento invalidado. Isso não prova cobertura de todos os ramos; o selo verifica presença de chamada viva, sem validar seus argumentos. Os “18 FAIL” executados: `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:224`; `app/ponto/tests/test_lote7_selo_escritor_acorda.py:222`) |
| `obs` interpretado como metadado é dívida anterior | A interpretação por substrings existe. Sua antiguidade e tratamento em fila: `[nao verificado]`. (`app/ferias/management/commands/listar_ferias_dp.py:27`; `app/ferias/management/commands/listar_ferias_dp.py:28`; `app/ferias/management/commands/listar_ferias_dp.py:30`) |
| Histórico/base/logs foram conferidos por outro verificador | Isso não substitui verificação nesta cópia. Igualdade byte a byte com a base e execuções relatadas: `[nao verificado]`. A ausência do bloco J1 é compatível com a passagem direta da atualização da Justificativa para o passo 5b. (`app/ponto/services/regularizacao.py:102`; `app/ponto/services/regularizacao.py:108`) |

Quanto às armadilhas novas do delta:

- **Não identifiquei asserção universalmente incapaz de falhar.** Em particular, o filtro por timestamp falha se `julgada_em` continuar nulo: a fixture não o preenche e o modelo permite `NULL`. Entretanto, o acréscimo temporal não prova avanço entre atos. Sob o congelamento, um mutante que grave `cel.julgada_em = cel.gerada_em`, conservando os demais efeitos, satisfaz `>= instante`. Sobrevivência inferida; execução `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:60`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:454`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`; `app/escala/models.py:1702`; `app/escala/models.py:1745`; `app/ponto/portas/celula.py:1194`)
- **Não há dependência de ordem indeterminada nas comparações acrescentadas.** Ausências são ordenadas por tuplas começando pela PK; auditorias são dicionários por PK; células usam contagem ou comparação com vazio. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:239`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:472`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:523`)
- **Todos os campos acrescentados existem.** (`app/ponto/models.py:749`; `app/ponto/models.py:751`; `app/ponto/models.py:755`; `app/ponto/models.py:776`; `app/ferias/models.py:312`; `app/ferias/models.py:314`; `app/core/models.py:299`; `app/escala/models.py:1745`)
- **Há imprecisão documental:** o novo comentário “linha INTEIRA” excede o retrato implementado; a docstring de `_retrato` continua enumerando os campos antigos. O comentário corrigido sobre futuro está correto, mas permanece outro contraditório em `editar_ausencia_registro`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:250`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:515`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`; `app/ponto/services/ausencia.py:1119`; `app/ponto/services/ausencia.py:665`; `app/ponto/services/ausencia.py:669`)

O mapa de escritores/leitores abaixo cobre os caminhos conferidos; censo exaustivo do repositório: `[nao verificado]`.

| Campo | Quem escreve | Quem lê e concordância com o teste | [nao verificado]
|---|---|---|
| `Ausencia.tipo` | Criação, edição e espelho de férias. (`app/ponto/services/ausencia.py:484`; `app/ponto/services/ausencia.py:648`; `app/ponto/services/ausencia.py:1390`) | O insumo da folha inclui tipo; o juiz o usa para interpretar ausência sem fim. A igualdade protege a classificação anterior, sem validar sua correção. (`app/ponto/services/ausencia.py:127`; `app/ponto/turnos.py:2805`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `Ausencia.data_fim` | Criação recebe; edição deriva/atribui; `save` pode derivar de `dias_corridos`. (`app/ponto/services/ausencia.py:485`; `app/ponto/services/ausencia.py:627`; `app/ponto/services/ausencia.py:650`; `app/ponto/models.py:917`) | O juiz interpreta fim inclusivo; `NULL` prolonga somente tipos em aberto, senão representa dia único. O snapshot preserva o valor sem impor interpretação divergente. (`app/ponto/turnos.py:2798`; `app/ponto/turnos.py:2803`; `app/ponto/turnos.py:2808`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `Ausencia.minutos` | Criação/edição; `save` deriva falta aprovada sem minutos; férias fornece zero. (`app/ponto/services/ausencia.py:485`; `app/ponto/services/ausencia.py:658`; `app/ponto/models.py:887`; `app/ponto/models.py:897`; `app/ponto/services/ausencia.py:1391`) | Insumo da folha e propriedade `horas` leem duração. O teste exige preservação; zero em férias não significa inexistência da cobertura, que é criada por dia. (`app/ponto/services/ausencia.py:127`; `app/ponto/models.py:876`; `app/ponto/services/ausencia.py:1390`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `Ausencia.descricao` | Criação, edição, reabertura e espelho; este grava `Ferias agendamento #N`. (`app/ponto/services/ausencia.py:486`; `app/ponto/services/ausencia.py:653`; `app/ponto/services/ausencia.py:1316`; `app/ponto/services/ausencia.py:1393`) | Painel exibe; backfill interpreta a marca `#N`. Preservar a descrição protege também esse metadado. (`app/templates/ponto/partials/painel_ausencia.html:56`; `app/ponto/services/ausencia.py:1552`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `AgendamentoFerias.data_inicio` | Criação e troca de janela. (`app/ferias/services.py:582`; `app/ferias/services.py:716`; `app/ferias/services.py:729`) | O espelho inicia a enumeração nessa data; snapshot exige a mesma janela após recusa. Significado concordante. (`app/ponto/services/ausencia.py:1380`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`) |
| `AgendamentoFerias.data_fim` | Criação calcula fim; troca de janela atribui/persiste. (`app/ferias/services.py:565`; `app/ferias/services.py:584`; `app/ferias/services.py:716`; `app/ferias/services.py:729`) | O espelho inclui o último dia. A comparação preserva esse limite inclusivo. (`app/ponto/services/ausencia.py:1381`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`) |
| `AgendamentoFerias.dias` | Criação e troca de janela. (`app/ferias/services.py:584`; `app/ferias/services.py:716`) | Modelo confere quantidade inclusiva; cancelamento subtrai `vivo.dias` dos concedidos. O teste preserva quantidade e janela, sem recalculá-las. (`app/ferias/models.py:382`; `app/ferias/services.py:680`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`) |
| `LogAuditoria.descricao` | `evento` persiste o argumento; rejeição fornece dia/agendamento. (`app/core/observ.py:136`; `app/ponto/services/ausencia.py:1500`) | Busca e tela leem texto. O teste preserva descrições antigas; não valida a descrição das trilhas novas. (`app/core/views.py:714`; `app/templates/core/log_auditoria.html:73`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:523`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:546`) |
| `CelulaDia.julgada_em` | `carimbar` grava `agora`, salvo retorno por idempotência; desfazer julgamento futuro pode limpar. (`app/ponto/portas/celula.py:1190`; `app/ponto/portas/celula.py:1194`; `app/ponto/portas/celula.py:1165`) | Cartório combina carimbo, impressão e veredito para frescor. O delta usa apenas o limite temporal para afirmar “neste ato”; sob congelamento, essa afirmação é mais forte que a evidência. (`app/ponto/services/cartorio.py:1260`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:463`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`) |

## 16/11 08:54 · sessao 6c1c5237105b · - · - · origem=Ronald (tmux)

A triagem atualizada procede no limite do contrato mostrado no código: `salvar_inteiro` e `registrar_mudanca` estão no mesmo `atomic`, e o `except` fica depois da saída desse bloco. Nenhum dos passivos abaixo demonstra quebra desse rollback excepcional na implementação atual. (`app/colaboradores/views.py:331-343`)

| Item | Conferência |
|---|---|
| Releitura sem teste que a distinga da restauração em memória | A correção da entrega é adequada: o gancho de desligamento só altera situação/data/motivo, preservando posto; suas asserções continuam compatíveis com restaurar `_posto_antes`. A leitura nova ocorre durante a trilha e não testa a origem do objeto usado no `except`. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:40-43`, `:216-228`, `:247-253`; `app/colaboradores/views.py:343`) |
| Homônimos | `de == para` retorna sem histórico; a view passa representações textuais, e `Posto.__str__` contém nome e empresa. Dois postos com essa mesma representação podem trocar FK sem trilha, por inferência estática. (`app/colaboradores/services/historico_vinculo.py:10-17`; `app/colaboradores/views.py:326-336`; `app/colaboradores/models.py:276-277`) |
| Anterior potencialmente obsoleto | `_posto_antes` é obtido antes da trava da porta; a guarda trava e relê situação, sem atualizar esse anterior. Corrida real: **[nao verificado]**. (`app/colaboradores/views.py:326-334`; `app/colaboradores/services/colaborador.py:65-66`, `:89-91`, `:158-160`) |
| `sem_posto` repetido | Está nos dois ramos e no painel, com pendência explícita registrada. (`app/colaboradores/views.py:346`, `:365`; `app/templates/colaboradores/partials/painel_vinculo.html:34`; `app/core/juizes.py:931-932`) |
| Datas distintas | A view usa dia atual; vínculo usa `data_inicio`; desligamento agendado usa `data_demissao`. (`app/colaboradores/views.py:335`; `app/colaboradores/services/vinculo.py:354`; `app/colaboradores/services/desligamento.py:107-116`) |
| Trava adicional na matrícula | O `save` pode travar a linha da maior matrícula quando a matrícula estiver ausente. (`app/colaboradores/models.py:427-438`) |

O aval integral `RESPOSTAS-DA-MANHA-1611` não foi encontrado na cópia; o código apenas o referencia. Portanto, dizer que esses itens estão “fora do aval literal” depende do resumo da entrega; conformidade com o aval integral e autorização final de pouso: **[nao verificado]**. (`app/colaboradores/views.py:331`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:231`)

Preexistência/ancestralidade dos passivos e resultados alegados de testes, mutantes, ruff e logs: **[nao verificado]**; a cópia não oferece metadados Git utilizáveis, e nenhum teste foi executado. **[nao verificado]** [nao verificado]

## 16/11 08:54 · sessao 452fd522f259 · - · - · origem=Ronald (tmux)

**1) PROCEDE.** A escrita excepcional de RED em cópia própria existe na regra, sob CONTRATO-DE-INSTRUMENTO e revisão pelo Code (`CLAUDE.md:349-350`; `app/docs/PISTA.md:143-145`). Entretanto, **não existe modo/flag do wrapper que a execute fora do papel construtor**: parser aceita apenas `--sonda`, `--anexo`, `--construtor` (`bin/codex_pergunta.sh:58-66`), e o próprio cabeçalho declara que o RED excepcional “não passa por aqui” (`bin/codex_pergunta.sh:30-33`).

O único caminho com escrita é `CODEX_PAPEL=construtor` combinado com `--construtor <wt>`; combinações divergentes são recusadas (`bin/codex_pergunta.sh:197-200`). Esse caminho exige `logs/construtor.vaga`, com `flock -n`, e recusa vaga ocupada (`bin/codex_pergunta.sh:248-253`). Os demais papéis recebem ordem explícita de não criar/editar/apagar e executam em sandbox `read-only` (`bin/codex_pergunta.sh:348-350`; `bin/codex_pergunta.sh:424-426`).

Assim, “se o wrapper exigir a vaga, entra atrás da espinha” resolve a espera, mas não torna executável a escrita **fora do papel construtor** prescrita no item 9 (`cortes_f2_f9_1611.txt:10`, numerando o anexo a partir de “aval Ronald”).

Se houver criação/adaptação de instrumento para suprir esse caminho, o contrato de cinco linhas e sua revisão precisam anteceder o código; o contrato específico desta obra não foi fornecido **[nao verificado]** (`CLAUDE.md:394`; `app/docs/PISTA.md:170-175`).

## 16/11 08:52 · sessao 0bf595a634d6 · - · - · origem=Ronald (tmux)

Mapa limitado aos caminhos de criação/edição, espelho/cancelamento e consumidores conferidos; censo exaustivo do repositório `[nao verificado]`.

| Campo | Escritores conferidos | Leitores e significado; concordância do teste | [nao verificado]
|---|---|---|
| `Ausencia.tipo` | `criar_ausencia` prepara tipo; edição troca; espelho escreve `'ferias'`. (`app/ponto/services/ausencia.py:484`; `app/ponto/services/ausencia.py:648`; `app/ponto/services/ausencia.py:1390`) | `_insumo_da_folha` fotografa tipo; juiz usa tipo para decidir extensão sem fim; folha usa tipos pagos. Comparar igualdade antes/depois protege a classificação existente, sem provar que ela era correta. (`app/ponto/services/ausencia.py:127`; `app/ponto/services/ausencia.py:139`; `app/ponto/turnos.py:2805`; `app/ponto/services/fechamento.py:694`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `Ausencia.data_fim` | Criação recebe; edição deriva e atribui; `Ausencia.save` deriva de `dias_corridos` quando aplicável. (`app/ponto/services/ausencia.py:485`; `app/ponto/services/ausencia.py:627`; `app/ponto/services/ausencia.py:650`; `app/ponto/models.py:912`; `app/ponto/models.py:917`) | Cobertura usa range fechado, ou tipo em aberto até horizonte, ou dia único. Snapshot concorda ao exigir preservação, sem interpretar `NULL` como universalmente aberto. O help text isolado é mais amplo que o juiz real. (`app/ponto/turnos.py:2798`; `app/ponto/turnos.py:2803`; `app/ponto/models.py:753`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `Ausencia.minutos` | Criação recebe; edição zera/recebe minutos; `save` deriva falta aprovada sem minutos pelo previsto; espelho de férias fornece zero. (`app/ponto/services/ausencia.py:485`; `app/ponto/services/ausencia.py:658`; `app/ponto/models.py:887`; `app/ponto/models.py:895`; `app/ponto/models.py:897`; `app/ponto/services/ausencia.py:1391`) | `_insumo_da_folha` inclui minutos; `horas` divide por 60; export distribui `a.minutos` entre dias. Igualdade no teste significa “não alterar a duração gravada”; zero em férias não significa ausência de cobertura. (`app/ponto/services/ausencia.py:127`; `app/ponto/models.py:876`; `app/folha/export.py:117`; `app/folha/export.py:121`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`; `app/ponto/services/ausencia.py:1391`) |
| `Ausencia.descricao` | Criação recebe; edição substitui se não vazia; reabertura concatena; espelho grava `Ferias agendamento #N`. (`app/ponto/services/ausencia.py:486`; `app/ponto/services/ausencia.py:652`; `app/ponto/services/ausencia.py:1316`; `app/ponto/services/ausencia.py:1393`) | Painel exibe texto; backfill também interpreta a marca `#N`. Portanto não é somente texto ornamental; snapshot protege inclusive esse metadado legado. (`app/templates/ponto/partials/painel_ausencia.html:56`; `app/templates/ponto/partials/painel_ausencia.html:57`; `app/ponto/services/ausencia.py:1552`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:522`) |
| `AgendamentoFerias.data_inicio/data_fim/dias` | Agendamento calcula fim inclusivo e grava dias; gozo histórico calcula dias inclusivos; `_trocar_janela` atribui e persiste os três. (`app/ferias/services.py:565`; `app/ferias/services.py:582`; `app/ferias/services.py:584`; `app/ferias/services.py:523`; `app/ferias/services.py:524`; `app/ferias/services.py:716`; `app/ferias/services.py:729`) | Modelo valida `dias == fim-início+1`; espelho enumera datas inclusivas; cancelamento subtrai `vivo.dias` do concedido. Snapshot tem o mesmo significado: preservar janela e quantidade em recusa, sem recalculá-las. (`app/ferias/models.py:382`; `app/ferias/models.py:383`; `app/ponto/services/ausencia.py:1380`; `app/ponto/services/ausencia.py:1381`; `app/ferias/services.py:680`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`) |
| `LogAuditoria.descricao` | `core.observ.evento` recebe e cria campo; rejeição fornece texto com dia e agendamento. (`app/core/observ.py:94`; `app/core/observ.py:134`; `app/core/observ.py:136`; `app/ponto/services/ausencia.py:1500`) | Busca filtra descrição e tela exibe; snapshot exige texto antigo idêntico. Não valida conteúdo/descrição das novas trilhas: `_nada_mudou` restringe comparação às PKs anteriores. (`app/core/views.py:714`; `app/templates/core/log_auditoria.html:73`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:523`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:546`) |
| `CelulaDia.julgada_em` | Porta `carimbar` atribui `agora`, salvo quando idempotência evita save; `desfazer_julgamento_da_futura` também pode limpar os campos de julgamento. (`app/ponto/portas/celula.py:1190`; `app/ponto/portas/celula.py:1194`; `app/ponto/portas/celula.py:1201`; `app/ponto/portas/celula.py:1165`) | Cartório exige carimbo + impressão + veredito para frescor; consumidores de parcial usam julgamento anterior. Delta interpreta como carimbo recente, mas `>= instante` não demonstra avanço entre dois atos sob relógio congelado. (`app/ponto/services/cartorio.py:1260`; `app/ponto/services/cartorio.py:1409`; `app/ponto/services/cartorio.py:417`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:60`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:463`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`) |

Campos novos existem, sem ocorrência de campo inexistente nestas comparações. (`app/ponto/models.py:749`; `app/ponto/models.py:751`; `app/ponto/models.py:755`; `app/ponto/models.py:776`; `app/ferias/models.py:312`; `app/ferias/models.py:313`; `app/ferias/models.py:314`; `app/core/models.py:299`; `app/escala/models.py:1745`)

Não há comparação nova dependente de ordem indefinida: ausências são `sorted` por tuplas cuja primeira coluna é PK única; trilhas são dicionário por PK; a consulta residual de células é comparada ao vazio. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:239`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:240`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:521`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:523`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:472`)

O comentário corrigido em `:1119` passa a concordar com `_teto_da_linha_aberta`, mas ficou comentário contraditório em `editar_ausencia_registro`: ainda diz que futuras não são alcançadas, apesar de chamar o mesmo acordador que calcula teto das competências carimbadas. Isso é remanescente, não nova mudança de código demonstrada pelo patch. (`app/ponto/services/ausencia.py:1119`; `app/ponto/services/ausencia.py:147`; `app/ponto/services/ausencia.py:151`; `app/ponto/services/ausencia.py:665`; `app/ponto/services/ausencia.py:666`; `app/ponto/services/ausencia.py:669`; `app/ponto/services/ausencia.py:190`; `app/ponto/services/ausencia.py:191`)

A docstring de `_retrato` ficou desatualizada: enumera somente status/obs e pk/status/data, enquanto código compara também os campos acrescentados. “INTEIRO” continua sendo somente o retrato enumerado, não linha completa. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:515`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:516`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:524`)

## 16/11 08:52 · sessao f5d22736a7d3 · - · - · origem=Ronald (tmux)

Auditoria estática, sem execução de testes ou mutação; resultados executados `[nao verificado]`. [nao verificado]

- **`julgada_em >= instante` tem mordida, mas não prova frescor temporal adicional.** A fixture cria células sem `julgada_em`, cujo campo permite NULL, então omitir o carimbo faz a contagem falhar. Entretanto, o relógio congelado faz `gerada_em` e `instante` coincidirem: um mutante de `carimbar` que grave `cel.julgada_em = cel.gerada_em`, mantendo os demais efeitos, passa. A asserção não distingue julgamento atual de um carimbo anterior no mesmo instante congelado. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:56`, `:60`, `:191`, `:439`, `:454`, `:463`, `:470`; `app/escala/models.py:1702`, `:1745`; `app/ponto/portas/celula.py:1194`)

- **Não identifiquei dependência de ordem indeterminada nas comparações novas.** Ausências usam `sorted` com PK como primeiro componente; trilhas são dicionários indexados por PK; carimbo usa contagem. A comparação com lista vazia independe de ordenação. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:239`, `:470`, `:472`, `:521`, `:523`)

- **Os campos novos existem.** Ausência: `tipo`, `data_fim`, `minutos`, `descricao`; agendamento: início, fim, dias; auditoria: descrição; célula: julgamento. (`app/ponto/models.py:749`, `:751`, `:755`, `:776`; `app/ferias/models.py:312`, `:313`, `:314`; `app/core/models.py:299`; `app/escala/models.py:1745`)

- **O comentário de produção corrigido concorda com o código.** O teto é o máximo entre hoje e o fim da última competência carimbada, e o botão aceita mês/ano do POST. (`app/ponto/services/ausencia.py:1119`, `:147`, `:151`, `:190`; `app/ponto/views.py:1240`, `:1241`, `:1255`)

- **“Linha INTEIRA” continua exagerado.** O retrato ampliado omite `dias_corridos`, `hora_ini`, `hora_fim` e outros campos existentes. O delta fecha os campos enumerados, sem provar preservação integral da linha. A docstring de `_retrato` também continua enumerando o retrato antigo. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:239`, `:250`, `:515`, `:520`; `app/ponto/models.py:761`, `:768`, `:770`)

- **Descartes periféricos:** “trilha nova pode nascer” corresponde expressamente ao contrato implementado, mas o teste permite qualquer inserção nova, sem validar que seja trilha legítima de recusa. Ramo sem adesão retorna antes do normal, portanto a lacuna do ramo com adesão é real; classificá-la fora do escopo não equivale a demonstrar cobertura. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:517`, `:546`, `:460`; `app/ponto/services/cartorio.py:1317`, `:1318`)

- **D/E e relógio permanecem na árvore.** `engolir` não abre savepoint; o signal envolve o julgamento nesse wrapper; o acordador lê `localdate()` para linha mudada sem fim; a invalidação captura exceção e retorna zero; o contador só encontra carimbo NULL. Origem histórica em c6, fila efetiva e providências externas `[nao verificado]`. (`app/core/observ.py:86`, `:88`, `:90`; `app/ponto/signals.py:88`, `:93`; `app/ponto/services/ausencia.py:184`, `:190`; `app/ponto/services/fechamento.py:1661`, `:1665`, `:1696`)

- **Selo como piso é descrição correta; “18 FAIL” não prova suficiência.** O selo verifica presença de chamada viva e não o conteúdo dos mapas; o comportamento com mapa vazio global é observado por testes que exigem invalidação, mas sobrevivência de mutantes condicionais não fica eliminada por essa contagem relatada. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:35`, `:222`, `:224`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:224`; execução dos 18 FAIL `[nao verificado]`)

- **`obs` é efetivamente lido como metadado por substring.** A existência da dívida está demonstrada; sua idade e gestão em fila `[nao verificado]`. (`app/ferias/management/commands/listar_ferias_dp.py:27`, `:28`, `:30`)

## 16/11 08:52 · sessao 02b77040be5f · - · - · origem=Ronald (tmux)

- O descarte de “levantar antes de tentar escrever” **só se sustenta para `raise` incondicional**. O positivo chama o serviço sem mock e exige retorno 5, cinco trilhas, invalidação e idempotência; uma exceção incondicional escaparia. Ele não exige a ordem escrita→auditoria nem tentativa de escrita quando a auditoria falha. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:211`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:216`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:227`)

- **Mutante sobrevivente plausível:** mover `_ev_rej(...)` para antes de `a.save`; se retornar `None`, levantar `TrilhaDaRejeicaoNaoGravada`; caso contrário, salvar. O positivo continua observando cinco rejeições/trilhas; o negativo encontra exatamente o retrato anterior e a exceção esperada, sem nenhuma tentativa de salvar ausência. Os mocks negativos falham já na primeira auditoria de rejeição. Mutação executada `[nao verificado]`. (`app/ponto/services/ausencia.py:1494`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1499`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:252`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:531`)

- Variante mais forte: além dessa inversão, retirar o `atomic` local da rejeição. Falha na segunda trilha de uma chamada direta poderia deixar a primeira rejeição persistida, mas os negativos falham na primeira; o positivo não injeta falha. No cancelamento, o `atomic` externo ainda protegeria o ato. A sobrevivência é inferência estática; execução `[nao verificado]`. (`app/ponto/services/ausencia.py:1484`; `app/ponto/services/ausencia.py:1494`; `app/ferias/services.py:674`; `app/ferias/services.py:683`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:531`)

- Os retratos novos fecham alterações/exclusões nos **campos enumerados**, não na linha inteira. Permanecem fora, por exemplo, `Ausencia.hora_ini/hora_fim/aprovado_por/aprovada_em/atualizado_em`, `AgendamentoFerias.confirmacao/confirmado_em/aviso_emitido` e `LogAuditoria.usuario/ip/ator/criado_em`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:238`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:520`; `app/ponto/models.py:768`; `app/ponto/models.py:815`; `app/ponto/models.py:824`; `app/ferias/models.py:315`; `app/ferias/models.py:324`; `app/core/models.py:295`; `app/core/models.py:302`)

- O descarte de trilha nova é coerente com o contrato explicitamente escolhido: comparar somente PKs antigas. Porém o teste permite **qualquer** trilha nova, sem conferir que seja recusa. Os handlers de falha consultados apenas retornam mensagem; não criam explicitamente auditoria de recusa. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:517`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:546`; `app/ferias/views.py:398`; `app/ferias/views.py:452`; `app/ferias/views.py:621`)

- Os descartes de adesão e conteúdo do julgamento delimitam escopo, mas não eliminam as lacunas: a fixture percorre o ramo `nunca_bateu`, que carimba e retorna antes do ramo normal; os asserts conferem contador, carimbo e FK, sem veredito/ata. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:454`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:460`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:470`; `app/ponto/services/cartorio.py:1314`; `app/ponto/services/cartorio.py:1318`)

- D/E continuam presentes: `engolir` não abre savepoint; a invalidação captura falha e retorna zero; repetição com insumos iguais não invalida; o contador olha apenas `derivado_em=NULL`. Classificá-los como fora desta volta é decisão de escopo; antiguidade independente dos anexos `[nao verificado]`. (`app/ponto/signals.py:88`; `app/core/observ.py:85`; `app/ponto/services/fechamento.py:1661`; `app/ponto/services/ausencia.py:184`; `app/ponto/services/fechamento.py:1696`)

- O selo continua apenas estrutural quanto à chamada: detecta uma chamada viva ao acordador, não valida seus argumentos. Os testes comportamentais positivos matam silenciar **todos** os acordamentos, mas não demonstram cobertura de todo ramo/contexto. Os “18 FAIL” executados permanecem `[nao verificado]`. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:222`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:224`)

- O leitor de `obs` realmente infere origem por substrings. A árvore também confirma que regularização grava Justificativa; a formulação atual da entrega, restringindo “só SELECT” aos acréscimos dos ganchos, corrige a ambiguidade anterior. Igualdade byte a byte com a base e logs de execução `[nao verificado]`. (`app/ferias/management/commands/listar_ferias_dp.py:26`; `app/ponto/services/regularizacao.py:102`; `app/ponto/services/regularizacao.py:106`)

## 16/11 08:51 · sessao 46d141e5339f · COMPLEMENTO-LAUDO-[nome]-1611 · sonda · origem=Code

Os números abaixo são transcritos do anexo. Uso **[nao verificado]** porque a saída foi fornecida na mensagem, sem arquivo:linha para citação ou confirmação independente.

**1. Frota e empresas**

Em cada coluna, a ordem é **colaboradores no recorte / colaboradores divergentes / vínculos divergentes**. A unidade principal declarada é colaborador distinto; divergência significa algum vínculo do recorte com posto diferente do cadastro. [nao verificado]

| Empresa | `flags_ativos` | `flags_ativos_e_vigencia_hoje` | Evidência |
|---|---:|---:|---|
| 1 | 2 / 0 / 0 | 2 / 0 / 0 | [nao verificado] |
| 2 | 419 / 44 / 44 | 419 / 44 / 44 | [nao verificado] |
| 3 | 100 / 6 / 6 | 100 / 6 / 6 | [nao verificado] |
| 4 | 19 / 3 / 3 | 19 / 3 / 3 | [nao verificado] |
| 20 | 0 / 0 / 0 | 0 / 0 / 0 | [nao verificado] |
| 21 | 0 / 0 / 0 | 0 / 0 / 0 | [nao verificado] |
| 29 | 0 / 0 / 0 | 0 / 0 / 0 | [nao verificado] |
| **Total da frota** | **540 / 53 / 53** | **540 / 53 / 53** | [nao verificado] |

A saída declara schema `juliani`, consulta em `2026-11-16 11:50:25.864495+00:00` e `hoje=2026-11-16`. [nao verificado]

**2. Estado de `col237`**

| Campo | Valor exibido | Evidência |
|---|---|---|
| Cadastro | `pk=237`, empresa `2`, situação `ativo` | [nao verificado] |
| Posto do cadastro | `444` | [nao verificado] |
| Vínculos exibidos | Um, com `ec_pk=204` | [nao verificado] |
| Posto do vínculo | `487` | [nao verificado] |
| Tipo de escala | `tipo_escala_id=177` | [nao verificado] |
| Flag do vínculo | `ativa=true` | [nao verificado] |
| Datas do vínculo | `data_inicio=2026-07-21`; `data_fim=null` | [nao verificado] |
| Divergência | `diverge_do_cadastro=true` | [nao verificado] |
| Inclusão nos recortes | `entra_recorte_flags=true`; `entra_recorte_vigencia_hoje=true` | [nao verificado] |

**3. Erros e linhas ausentes**

- O trecho termina com `rc_sombra=0` e não exibe mensagem de erro ou traceback. [nao verificado]
- Há sete linhas por empresa e uma de total em cada recorte, além do cabeçalho e do detalhe de `col237`; as somas por empresa coincidem com os totais apresentados. [nao verificado]
- Não há lacuna aparente nessa estrutura; a existência de outras linhas obrigatórias segundo a seção 4 ou o código exato da sonda permanece [nao verificado].

**4. O que a saída não responde**

- Qual posto a geofence ou a apuração de feriado efetivamente usaram em dias passados, inclusive para `col237`. [nao verificado]
- Quando surgiu a divergência, quem a causou ou qual posto deveria prevalecer; `data_inicio` é a data exibida do vínculo, sem histórico de alterações anexado. [nao verificado]
- Quais são os demais colaboradores divergentes, seus vínculos, ou os nomes e características dos postos `444` e `487`. [nao verificado]
- Se houve impacto em batidas, feriados, pagamentos ou alertas, e qual é o estado posterior à consulta. [nao verificado]

## 16/11 08:50 · sessao 848fdbb6ac2d · LOTE3-C7 · verificador · origem=Code

**O delta distingue as três versões pedidas, por análise estática. Ainda há mutante plausível sobrevivente e uma justificativa incorreta na triagem: o teste c6 citado também não prova a releitura do banco.** (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:216-260`; `app/colaboradores/views.py:333-343`).

**1. Poder de distinção do teste** [nao verificado]

| Versão | Resultado esperado pelo código |
|---|---|
| **(a) Sem atomic comum, ordem salvar → trilha** | Falha na FK: a porta encerra seu atomic próprio; o substituto escreve histórico e levanta depois. O posto novo permanece visível, contrariando a exigência do antigo. (`app/colaboradores/services/colaborador.py:158-161`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-253`). |
| **(b) Trilha → salvar, sem atomic comum** | Falha na contagem: a trilha real insere, depois o substituto levanta; a porta não é alcançada, mas o histórico permanece. (`app/colaboradores/services/historico_vinculo.py:13-17`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-255`). |
| **(c) Atomic comum atual** | As duas escritas são desfeitas antes do `except`; posto antigo, contagem anterior, HTML antigo e toast esperado são compatíveis com as asserções. (`app/colaboradores/views.py:333-358`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:249-260`). |

**Mutante sobrevivente:** colocar a trilha primeiro, envolver **somente ela** em atomic e deixar `salvar_inteiro` depois, fora desse bloco. A exceção injetada desfaz o histórico e impede alcançar a porta; todas as verificações desse teste ficam satisfeitas. Em um caminho sem essa exceção, as duas operações continuariam sem atomic comum. O teste não exige passagem pela porta nem sua ordem relativa à trilha. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-260`; chamadas atuais em `app/colaboradores/views.py:334-336`). Execução desse mutante: **[nao verificado]**.

Logo, o teste fortaleceu a prova dos **efeitos dessa falha**, mas a existência do atomic comum continua demonstrada também pela inspeção da view. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:249-260`; `app/colaboradores/views.py:333-336`).

**2. Possível verde por motivo errado** [nao verificado]

- **Asserção engolida pela view: não ocorre neste substituto.** Ele apenas chama a função real, guarda o retorno e levanta `RuntimeError`. Todas as asserções executam depois do POST, fora do alcance do `except Exception` da view. Se a função real levantar antes de retornar, o `append` não termina e falha `len(escritas) == 1`; se retornar `None`, falha a asserção seguinte. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-260`; `app/colaboradores/views.py:337-359`).
- **Contagem na mesma transação: não mascara os mutantes simples.** O teste herda de `TestCase` e consulta novamente FK e contagem depois do POST; escritas próprias não desfeitas permanecem visíveis nessa conexão. Portanto, a transação envolvente do teste não torna automaticamente verde a ausência do atomic da view. A evidência é de rollback observado localmente; commit real, outra conexão e concorrência ficam **[nao verificado]**. (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:248-255`).
- **Toast:** agora exige o objeto `showToast` completo, com mensagem e tipo exatos. Não exige que esse seja o único evento no cabeçalho. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:259-260`).

**3. Triagem dos descartes**

**Há uma correção necessária na entrega:** o teste c6 citado também aceita substituir a releitura por `colaborador.posto = _posto_antes`. Seu gancho altera somente situação, data e motivo de desligamento; o posto permanece antigo. As verificações de FK, nomes no HTML e histórico vazio continuam satisfeitas pela restauração em memória. A implementação atual efetivamente relê o banco, mas esses testes não distinguem as duas soluções. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:40-43`, `:216-228`, `:252-258`; `app/colaboradores/views.py:343`).

Os demais itens descartados têm fundamento no código:

| Descarte | Evidência |
|---|---|
| Homônimos podem trocar FK sem histórico | A comparação é textual; `Posto.__str__` contém nome e empresa. (`app/colaboradores/services/historico_vinculo.py:10-11`; `app/colaboradores/models.py:276-277`). |
| Anterior potencialmente obsoleto | `_posto_antes` é capturado antes da trava; a guarda relê situação, sem renovar esse anterior. Possibilidade por inferência estática. (`app/colaboradores/views.py:326`, `:334-335`; `app/colaboradores/services/colaborador.py:89-91`, `:65-66`). |
| Derivação repetida de `sem_posto` | Existe nos dois ramos e no painel; há pendência registrada. (`app/colaboradores/views.py:346`, `:365`; `app/templates/colaboradores/partials/painel_vinculo.html:34`; `app/core/juizes.py:931-932`). |
| Datas com significados distintos | Recebem dia da execução, início da escala e data de demissão agendada. (`app/colaboradores/views.py:335`; `app/colaboradores/services/vinculo.py:354`; `app/colaboradores/services/desligamento.py:113-116`). |
| Trava adicional por matrícula ausente | O override trava também a linha da maior matrícula. (`app/colaboradores/models.py:427-438`). |

**Para o contrato restrito “falha excepcional da trilha desfaz posto e trilha”, nenhum desses descartes demonstra bloqueio funcional da cura atual:** as duas chamadas estão no mesmo atomic e o `except` está fora. A alegação de cobertura da releitura precisa ser corrigida. (`app/colaboradores/views.py:333-343`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:216-228`).

O teor integral do aval e do laudo original não foi apresentado; há referências a eles no código. Conformidade com exigências adicionais desses documentos: **[nao verificado]**. (`app/colaboradores/views.py:331`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:231`).

**4. Escritores e leitores dos valores examinados**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `HX-Trigger` | A view serializa mensagem da exceção e `tipo='erro'`. (`app/colaboradores/views.py:358`). | Teste decodifica e compara `showToast`; navegador decodifica e chama o toast, usando `textContent`. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:259-260`; `app/templates/core/_dialogo.html:79-85`, `:37`). |
| `HistoricoVinculo` | A view fornece os campos; `registrar_mudanca` executa `create`. O substituto chama essa função real. Existe também o escritor `registrar_evento_de_vinculo`. (`app/colaboradores/views.py:335-336`; `app/colaboradores/services/historico_vinculo.py:13-17`, `:20-27`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:245`). | Teste conta antes/depois e verifica retorno. Painel consulta e mostra histórico; cadastro e fio também exibem. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:240`, `:249-255`; `app/colaboradores/views.py:306-307`; `app/templates/colaboradores/partials/painel_vinculo.html:60-64`; `app/templates/colaboradores/editar.html:187`; `app/templates/chamados/fio_colaborador.html:89-91`). |
| `Colaborador.posto_id` | Fixture grava o antigo; POST informa o novo; view atribui a relação e a porta salva. (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:54-55`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:207-209`; `app/colaboradores/views.py:322-327`; `app/colaboradores/services/colaborador.py:160`). | Teste consulta a FK diretamente. View relê o banco e fornece o posto ao template, que mostra nome/praça; teste verifica os nomes. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:252-258`; `app/colaboradores/views.py:343`, `:350`; `app/templates/colaboradores/partials/vinculo_row.html:16-21`). |

Resultados RED/verde/mutantes/ruff alegados nos anexos, identidade Git e ocorrência dos passivos em PROD: **[nao verificado]**. [nao verificado]

## 16/11 08:50 · sessao de75e71aae41 · - · - · origem=Ronald (tmux)

1. Por análise estática, o delta distingue as três versões citadas: sem atomic externo e ordem save→trilha, a FK nova permanece visível e falha a asserção de posto antigo; com trilha→save sem atomic comum, o INSERT real permanece visível e falha a contagem; com o atomic atual, ambas as escritas são desfeitas. A porta tem atomic próprio, e o substituto levanta somente depois de retornar do create real. (`app/colaboradores/views.py:333-337`; `app/colaboradores/services/colaborador.py:152-161`; `app/colaboradores/services/historico_vinculo.py:13-17`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-255`). Execução independente dessas variantes: [nao verificado].

2. **Mutante plausível sobrevivente:** mover a trilha para antes da porta, envolver **somente a trilha** em atomic e deixar `salvar_inteiro` depois, fora dele. O substituto insere e levanta dentro desse atomic; ele desfaz o histórico; a porta jamais é alcançada; posto antigo, histórico inalterado, HTML antigo e toast esperado satisfazem o teste. Portanto, o delta demonstra os efeitos exigidos nesse erro, mas não exige que a porta tenha sido alcançada nem prova estruturalmente atomic comum. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-260`; `app/colaboradores/views.py:334-336`). Execução do mutante: [nao verificado]. Remover a chamada à porta também sobrevive a esse teste isolado pelo mesmo motivo. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:249-260`).

3. **Não há asserção do teste executada dentro do substituto.** Ele só chama a função real, registra retorno e levanta `RuntimeError`; todas as asserções, inclusive JSON, executam depois do POST. Logo, o `except Exception` da view não engole essas asserções. Se a função real levantar antes de retornar, `append` não termina e `len(escritas)==1` falha; se retornar `None`, falha `assertIsNotNone`. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:244-260`; `app/colaboradores/views.py:337-359`).

4. A leitura na mesma transação do `TestCase` não mascara os dois mutantes simples: as queries posteriores consultam o banco novamente, e escritas próprias não desfeitas continuam visíveis nessa conexão. O encerramento do teste ocorre depois dessas asserções. Isso verifica rollback local, sem demonstrar commit real, outra conexão, concorrência ou callbacks de commit. (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:43`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:248-255`). Comportamento em execução independente/PROD: [nao verificado].

5. **A justificativa de um descarte da entrega é incorreta:** `test_MORDE_desligado_no_meio_linha_mostra_o_posto_gravado` também não distingue substituir releitura por `colaborador.posto = _posto_antes`. Seu gancho muda situação/data/motivo, sem mudar posto; suas asserções exigem somente FK antiga, nomes antigo/novo no HTML e histórico vazio. A restauração em memória satisfaz isso. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:40-43`, `:216-228`). O template não mostra situação/data/motivo de desligamento, portanto não expõe o objeto desatualizado nesse cenário. (`app/templates/colaboradores/partials/vinculo_row.html:1-48`). Isso pede correção da alegação de cobertura da entrega; a implementação atual efetivamente relê o banco. (`app/colaboradores/views.py:343`). Execução do mutante: [nao verificado].

## 16/11 08:49 · sessao b1facf76be2c · - · - · origem=Ronald (tmux)

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `HX-Trigger` | Recusa da view serializa `{'showToast': {'msg': f'Erro: {e}', 'tipo': 'erro'}}`. `app/colaboradores/views.py:337`, `:358`. | Teste faz `json.loads` e compara o objeto integral. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:259-260`. Produção lê cabeçalho/JSON e passa mensagem/tipo ao toast; mensagem vai para `textContent`. `app/templates/core/_dialogo.html:79-85`, `:37`. |
| `HistoricoVinculo` | View fornece os campos do ato; serviço faz `objects.create`. `app/colaboradores/views.py:335-336`; `app/colaboradores/services/historico_vinculo.py:13-17`. Substituto chama a função real e depois levanta. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:241-246`. | Teste conta antes/depois e exige um retorno não nulo do escritor. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:240`, `:249-255`. Painel consulta linhas/contagem e exibe os campos. `app/colaboradores/views.py:306-307`; `app/templates/colaboradores/partials/painel_vinculo.html:60-64`. Cadastro e fio também exibem histórico. `app/templates/colaboradores/editar.html:179`, `:187`; `app/inteligencia/resumo.py:51-52`; `app/templates/chamados/fio_colaborador.html:89-91`. |
| `Colaborador.posto_id` | Fixture grava o posto antigo; POST fornece o novo. `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:54-55`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:201`, `:207-209`. View atribui a relação e porta executa `save()`. `app/colaboradores/views.py:322-327`; `app/colaboradores/services/colaborador.py:158-160`; `app/colaboradores/models.py:439`. | Teste consulta diretamente o FK no banco. `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:252-253`. View relê banco e fornece relação ao template; HTML mostra nome/praça, conferidos pelo teste. `app/colaboradores/views.py:343`, `:350`; `app/templates/colaboradores/partials/vinculo_row.html:16-21`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:256-258`. |

Há outro escritor de histórico, `registrar_evento_de_vinculo`, também com `objects.create`; portanto “escritor único = registrar_mudanca” só é adequado se restrito ao caminho examinado. `app/colaboradores/services/historico_vinculo.py:20-27`.

O formulário HTMX recebe a resposta substituindo a linha inteira. `app/templates/colaboradores/partials/painel_vinculo.html:112-115`.

## 16/11 08:49 · sessao fa11404d8245 · - · - · origem=Ronald (tmux)

A triagem tem uma justificativa incorreta: o teste c6 **também não distingue** a releitura do banco de `colaborador.posto = _posto_antes`. Seu gancho muda somente `situacao`, `data_demissao` e `motivo_desligamento`; o posto continua `self.po`, igual ao capturado anteriormente. Portanto restaurar o anterior em memória satisfaz suas asserções de posto, HTML e contagem. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:40-43`, `:216-228`; `app/colaboradores/views.py:326`, `:343`).

Os demais descartes correspondem a limitações presentes no código, fora da falha excepcional que o atomic resolve:

- Igualdade textual descarta histórico; `Posto.__str__` usa nome e empresa. (`app/colaboradores/services/historico_vinculo.py:10-11`; `app/colaboradores/models.py:276-277`).
- Anterior é capturado antes da trava, portanto pode ficar obsoleto sob concorrência, por inferência estática. (`app/colaboradores/views.py:319`, `:326`, `:333-335`; `app/colaboradores/services/colaborador.py:89-91`).
- `sem_posto` aparece nos dois ramos e no painel, com pendência explícita no registro. (`app/colaboradores/views.py:346`, `:365`; `app/templates/colaboradores/partials/painel_vinculo.html:34`; `app/core/juizes.py:931-932`).
- `HistoricoVinculo.data` recebe dia da execução aqui, início de escala no vínculo e demissão futura no agendamento. (`app/colaboradores/views.py:335`; `app/colaboradores/services/vinculo.py:354-357`; `app/colaboradores/services/desligamento.py:113-116`).
- Matrícula ausente provoca trava adicional sobre o colaborador da maior matrícula. (`app/colaboradores/models.py:427-438`).

Para o contrato restrito **“exceção na trilha desfaz posto e trilha”**, nenhum desses itens demonstra bloqueio funcional: as duas portas estão no atomic comum e o `except` está fora. (`app/colaboradores/views.py:333-343`). A afirmação de cobertura da releitura exige correção na entrega; o teste c6 citado não fornece essa prova. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:216-228`).

Não localizei na cópia o conteúdo literal de `respostas_da_manha_1611.txt` nem o laudo opus c6; há referências em comentários, mas a conformidade com seu teor integral fica **[nao verificado]**. (`app/colaboradores/views.py:331`; `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:231`). Datas/execuções de logs anexados e condições reais de PROD: **[nao verificado]**.

## 16/11 08:48 · sessao 596161895ebe · BUG-PAR-ENSAIO-MERGE · leitura · origem=Code

**Pelo anexo, o merge é textualmente limpo; isso não demonstra compatibilidade de sentido.** A saída contém somente a árvore `135d7d842d90a3ce2a8e0ed907382919c4cafea8`, sem arquivo em conflito; o pedido informa `rc 0`. (`ensaio_mergetree_full.txt:1`; execução independente do merge: [não verificado].)

A comparação abaixo usa a árvore disponível e os hunks B1 fornecidos. O histórico e o diff próprio completo da raia não puderam ser reconstruídos nesta cópia: [não verificado]. Resultado de suíte pós-merge e estado de produção: [não verificado]. [nao verificado]

**1) Os três arquivos tocados pelos dois lados têm alterações separadas.**

| Arquivo | Trecho visível na raia | Hunk B1 fornecido | Relação |
|---|---|---|---|
| `app/chamados/services/fio.py` | O tratamento BUG-PAR transforma falha da testemunha em `TESTEMUNHA_INDISPONIVEL`, retirando a autorização para validar. (`app/chamados/services/fio.py:372`, `app/chamados/services/fio.py:376`.) | `@@ -1592,17 +1592,25 @@`: substitui `recusa_respondida_sem_texto` por `recusa_par_ilegal`, acrescentando a recusa de texto sem instante. A definição antiga está na raia em `app/chamados/services/fio.py:1598`. | **Não se sobrepõem; estão distantes.** O bloco de apresentação e a porta de integridade são funções distintas. (`app/chamados/services/fio.py:372`; `app/chamados/services/fio.py:1598`; hunk B1 citado.) |
| `app/chamados/tests/test_s139.py` | O teste espera ausência de batida e erro `DECLARADO-X-GRAVADO`; a chamada ao materializador está em `:48`, os asserts em `:53` e `:55`. (`app/chamados/tests/test_s139.py:26`, `app/chamados/tests/test_s139.py:48`, `app/chamados/tests/test_s139.py:53`.) | `@@ -42,7 +42,8 @@`: acrescenta `respondida_em=agora` ao `create` que já contém `'02:00'`. A fixture atual está em `app/chamados/tests/test_s139.py:43`. | **Próximos, sem sobreposição das alterações:** B1 corrige a fixture; a raia conserva a expectativa de espera depois da materialização. (`app/chamados/tests/test_s139.py:45`, `app/chamados/tests/test_s139.py:53`; hunk B1 citado.) |
| `app/core/tests/test_medida_turno_do_par.py` | O registro de materialização aponta `_plantio_preenche_furo_na_data` para `[417]`. (`app/core/tests/test_medida_turno_do_par.py:271`.) | `@@ -255,10 +255,10 @@`: atualiza quatro referências de `juizes.py`, de `53/169/274/203` para `54/170/275/204`. Os valores antigos estão em `app/core/tests/test_medida_turno_do_par.py:258`. | **Próximos, sem sobreposição:** são entradas diferentes do mesmo dicionário. Devem permanecer tanto os quatro valores B1 quanto a entrada da materialização correspondente à árvore final. (`app/core/tests/test_medida_turno_do_par.py:258`, `app/core/tests/test_medida_turno_do_par.py:271`; hunk B1 citado.) |

Os trechos acima são os lados visíveis na cópia; os hunks próprios originais da raia, contra seu ancestral, são [não verificado].

**2) Os cruzamentos de sentido exigem distinguir nomeação do juiz, integridade da escrita e destino da tarefa.**

Na B1, “respondida” significa **instante presente E texto não vazio após trim**. A recusa de gravação passa a abranger as duas metades ilegais; a migração dos leitores das classes B/C/D fica expressamente para B2. (B1 `app/chamados/juizes.py`, hunk `@@ -1531,6 +1534,24 @@`.)

| Ponto da raia | Suposição atual e mudança B1 | Coerência necessária na volta |
|---|---|---|
| **C6 e par projetado** | A projeção corta conjuntamente instante e texto posteriores a `agora`; o C6 testa os valores projetados com `I E T`. B1 nomeia essa mesma conjunção por `pergunta_respondida(SimpleNamespace(...))`. (`app/chamados/juizes.py:1388`, `app/chamados/juizes.py:1393`, `app/chamados/juizes.py:1420`; B1 hunk `@@ -1417,7 +1418,9 @@`.) | Preservar a chamada B1 **sobre o par projetado**. Chamar diretamente `pergunta_respondida(p)` nesse degrau perderia o corte temporal. A tabela-verdade desse degrau não precisa ser invertida. (`app/chamados/juizes.py:1393`, `app/chamados/juizes.py:1403`; hunk B1 citado.) |
| **Irmã do intervalo** | A irmã “respondida” é selecionada por instante no SQL e texto no Python, além de materialização nula, identidade e exclusão de pergunta em voo. (`app/chamados/services/materializacao.py:143`, `app/chamados/services/materializacao.py:146`.) B1 passa a nomear essa conjunção pelo juiz. (B1 `app/chamados/juizes.py`, hunk `@@ -1531,6 +1534,24 @@`.) | Para acompanhar a autoridade B1, nomear `pergunta_respondida` ou sua face SQL, conservando identidade, materialização, ambiguidade e voo. A troca equivalente não implica novo conjunto de irmãs. (`app/chamados/services/materializacao.py:141`, `app/chamados/services/materializacao.py:145`, `app/chamados/services/materializacao.py:147`; hunk B1 citado.) |
| **BUG-PAR-103 e situação da irmã** | Ambos repetem `I E T`; o primeiro também exige via, materialização anterior ao corte e lâmpada não comprovadamente acesa. (`app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:276`, `app/chamados/services/materializacao.py:307`.) | Nomear o juiz nesses dois leitores, preservando os demais filtros e rótulos. Uma pergunta ressuscitada pela B1, com par vazio, deve cair em “não respondida”. (`app/chamados/services/materializacao.py:278`, `app/chamados/services/materializacao.py:305`; B1 `app/chamados/models.py`, hunk `@@ -1609,10 +1611,16 @@`.) |
| **Contador de recusadas** | Já usa `q_respondida_com_texto`; a B1 muda a documentação dessa função para face SQL do juiz, sem alterar seu corpo nesse hunk. (`app/chamados/services/materializacao.py:247`, `app/chamados/services/materializacao.py:249`; B1 `app/chamados/juizes.py`, hunk `@@ -1552,7 +1573,7 @@`.) | Conservar a face SQL e as expectativas de conjunto; não substituir por instante isolado nem por `exclude(resposta_colab='')`. (`app/chamados/services/materializacao.py:249`; hunk B1 citado.) |
| **Leitores de uma só metade** | A metade principal do par testa somente texto; a validação testa somente instante; a partição do app também usa instante. (`app/chamados/services/materializacao.py:188`; `app/chamados/services/validacao.py:521`; `app/chamados/services/pergunta_no_app.py:76`.) | Registrar a dependência, sem afirmar que B1 já migrou esses leitores. Trocar seus predicados pode alterar o tratamento do legado; B1 reserva essas classes para B2. (B1 `app/chamados/juizes.py`, hunk `@@ -1531,6 +1534,24 @@`.) |
| **Porta de integridade em `fio.py`** | A porta atual recusa instante sem texto. B1 renomeia a função e acrescenta `TextoSemCarimbo`. (`app/chamados/services/fio.py:1598`, `app/chamados/services/fio.py:1605`; B1 hunk `@@ -1592,17 +1592,25 @@`.) | Preservar a renomeação e os chamadores B1 em nascimento, save, lote e resposta automática. O tratamento BUG-PAR da testemunha não chama essa porta antiga. (`app/chamados/services/fio.py:372`; B1 `app/chamados/models.py`, hunks `@@ -2918,8 +2936,8 @@`, `@@ -3339,20 +3357,21 @@`, `@@ -3526,44 +3545,40 @@`; B1 `disputa_emissao.py`, hunk `@@ -1664,11 +1664,12 @@`.) |
| **Save e meia escrita do par** | O save atual compara o par carregado em memória; B1 passa a julgar o par efetivamente persistido, lendo do banco a metade omitida por `update_fields` e comparando com o banco atual. (`app/chamados/models.py:3550`; B1 hunk `@@ -3526,44 +3545,40 @@`.) | Fixtures e chamadas que mudam a resposta devem persistir o par completo, ou completar legalmente a metade existente. Ter o par legal apenas na memória deixa de absolver um `update_fields` incompleto. (B1 hunk citado; B1 `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:121`, no anexo.) |
| **Lote e ressuscitação** | A ressuscitação atual zera somente o instante no lote. B1 zera também o texto da simulação e delega a escrita a `reabrir_para_correcao(... REABERTURA)`. (`app/chamados/models.py:1555`, `app/chamados/models.py:1613`; B1 hunks `@@ -1553,13 +1553,15 @@`, `@@ -1609,10 +1611,16 @@`.) | Conservar o par inteiro vazio, a foto anterior na REABERTURA e o evento da célula. Expectativas de ressuscitação sem esse ato precisam acompanhar a nova sequência. (`app/chamados/services/disputa_emissao.py:1609`, `app/chamados/services/disputa_emissao.py:1631`; hunks B1 citados.) |
| **Devolução BUG-PAR** | A devolução103 limpa materialização e validação, preserva a resposta e registra `DEVOLUCAO`. A B1 muda a ressuscitação para uma porta que apaga a resposta e registra `REABERTURA`. (`app/chamados/services/validacao.py:293`, `app/chamados/services/validacao.py:299`, `app/chamados/services/validacao.py:303`; B1 `models.py`, hunk `@@ -1609,10 +1611,16 @@`.) | **Preservar a DEVOLUCAO BUG-PAR.** Substituí-la pela reabertura B1 apagaria a resposta que será validada e mudaria o próximo responsável. (`app/chamados/services/validacao.py:304`; `app/chamados/services/tarefa_do_dia.py:152`, `app/chamados/services/tarefa_do_dia.py:154`.) |
| **Tarefa: decisão e dono** | RESPOSTA com texto deixa validar/admin; RECUSA ou REABERTURA deixa responder/colaborador; DEVOLUCAO deixa validar/admin. (`app/chamados/services/tarefa_do_dia.py:148`, `app/chamados/services/tarefa_do_dia.py:152`, `app/chamados/services/tarefa_do_dia.py:154`.) B1 acrescenta a REABERTURA da ressuscitação. (B1 `models.py`, hunk `@@ -1609,10 +1611,16 @@`.) | Testar a ressuscitação como REABERTURA/colaborador e a devolução BUG-PAR como DEVOLUCAO/admin. Não manter a validação anterior como último ato depois de uma REABERTURA posterior. (`app/chamados/services/tarefa_do_dia.py:147`, `app/chamados/services/tarefa_do_dia.py:152`.) |
| **Tarefa: fila e tempo** | A carga ordena decisões por pergunta, instante e PK, corta por `agora` e consulta a fila no mesmo instante; a porta administrativa só existe se a âncora estiver na fila. (`app/chamados/services/tarefa_do_dia.py:142`, `app/chamados/services/tarefa_do_dia.py:410`, `app/chamados/services/tarefa_do_dia.py:436`.) | Manter esse corte temporal e verificar fila/porta ao testar o dono após o ato novo B1. DEVOLUCAO, isoladamente, não prova exposição no painel. (`app/chamados/services/tarefa_do_dia.py:142`, `app/chamados/services/tarefa_do_dia.py:613`; B1 hunk de ressuscitação citado.) |

Há ainda estes cruzamentos que não devem ser confundidos com mudança de regra:

- **Fio e créditos:** `espera_o_texto_do_colaborador` já equivale à negação de `I E T`; a montagem publica seu inverso em `resposta_enviada`. A B1 não troca esse leitor no diff fornecido. Já `raiox.creditos_perguntas` passa da conjunção sobre tuplas para o juiz sobre `SimpleNamespace`, preservando a mesma separação. (`app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:187`, `app/chamados/services/fio.py:357`; `app/chamados/services/raiox.py:143`; B1 `raiox.py`, hunk `@@ -138,10 +138,13 @@`.)
- **Retenção e property:** a retenção sem veredito continua usando `I OU T`, e `PerguntaDisputa.respondida` continua usando somente instante. Não cabe atribuir à B1 a migração desses dois predicados para o juiz novo. (`app/chamados/models.py:1905`, `app/chamados/models.py:1907`, `app/chamados/models.py:3640`; B1 `juizes.py`, hunk `@@ -1531,6 +1534,24 @@`.)
- **S128:** B1 troca a conjunção inline por `pergunta_respondida(p)` ao preservar resposta ainda não materializada de fato. A proteção da pergunta em voo continua relevante durante o plantio BUG-PAR; a substituição do predicado não autoriza outro escritor a carimbá-la. (`app/chamados/services/disputa_emissao.py:1997`; `app/chamados/services/materializacao.py:1623`, `app/chamados/services/materializacao.py:1755`; B1 `disputa_emissao.py`, hunk `@@ -1997,8 +1998,8 @@`.)
- **I21:** B1 passa a usar a face SQL com trim, excluindo carimbo com somente brancos. `materializada_em__isnull=True` já era lido antes; essa leitura não nasce na B1. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1572`; B1 hunk `@@ -1569,10 +1569,12 @@`.)

Os escritores e consumidores dos campos cruzados ficam assim:

| Campo/ato | Sítios relevantes na raia | Efeito do pouso B1 | [nao verificado]
|---|---|---|
| **`materializada_em`** | `_materializar_uma` chama `carimbar`; o escritor central atribui o carimbo. `_validar_o_par` exige as duas materializadas e o laço relê a pergunta para não repetir a irmã já gravada. (`app/chamados/services/materializacao.py:1812`; `app/chamados/models.py:3182`; `app/chamados/services/validacao.py:490`; `app/chamados/services/materializacao.py:1944`.) | Preservar esses caminhos. A B1 passa a desfazer o carimbo da ressuscitação pela reabertura existente; os leitores de irmã, corte103 e aplicação precisam observar esse estado limpo. (`app/chamados/services/materializacao.py:145`, `app/chamados/services/materializacao.py:276`, `app/chamados/services/materializacao.py:748`; B1 `models.py`, hunk de ressuscitação.) |
| **Instante e texto da resposta** | A resposta real grava ambos; `carimbar` só completa instante quando existe texto; a devolução103 conserva ambos. (`app/chamados/services/disputa_emissao.py:1481`; `app/chamados/models.py:3213`; `app/chamados/services/validacao.py:294`.) | Esses caminhos completos ou guardados são compatíveis com a recusa B1. Escrita nova só de texto, ou retirada só do instante de quem tem texto, passa a ser recusada. (B1 `models.py`, hunks de save e lote citados; execução: [não verificado].) |
| **`DecisaoPergunta`** | A validação grava VALIDACAO com texto; a devolução grava DEVOLUCAO; a autoridade de passivo preserva RESPOSTA/VALIDACAO anteriores. (`app/chamados/services/validacao.py:167`, `app/chamados/services/validacao.py:302`; `app/chamados/models.py:4074`, `app/chamados/models.py:4078`.) | B1 acrescenta REABERTURA com foto na ressuscitação. A tarefa lê a sequência; o rótulo BUG-PAR lê especificamente DEVOLUCAO com motivo BUG-PAR-103 e não deve aparecer para a nova REABERTURA. (`app/chamados/services/tarefa_do_dia.py:410`; `app/chamados/services/materializacao.py:291`; B1 `models.py`, hunk de ressuscitação.) |

`app/docs/JUIZES.md` diz que o registro vivo manda e que divergências documentais devem ser corrigidas; documenta tarefa por última decisão/fila e tranca C20, mas não contém a entrada C21 nesse trecho. A B1 acrescenta C21 ao registro vivo; sua alteração efetiva de documentação é [não verificado], pois o anexo exclui docs. A volta precisa reconciliar a tabela com `pergunta_respondida`, a recusa simétrica e a ressuscitação por REABERTURA, preservando a diferença da DEVOLUCAO ao admin. (`app/docs/JUIZES.md:12`, `app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:78`, `app/docs/JUIZES.md:94`; B1 `app/core/juizes.py`, hunks `@@ -163,6 +163,11 @@` e `@@ -298,6 +303,7 @@`.)

**3) Não há quebra necessária demonstrada dos testes próprios BUG-PAR quando todos os hunks B1 são preservados.** Há falhas previsíveis se a volta perder correções B1 ou confundir os atos; aprovação da suíte completa permanece [não verificado]. (`app/chamados/tests/test_bug_par_projetado.py:78`, `app/chamados/tests/test_bug_par_projetado.py:253`, `app/chamados/tests/test_bug_par_projetado.py:308`.)

| Teste/assert da raia | Quebra previsível e condição | [nao verificado]
|---|---|
| `test_s139.py`, asserts de ausência de batida e espera em `:53` e `:55` | **Sem a correção de fixture B1**, o `create` de texto sem instante em `:43` será recusado antes dos asserts. **Com a correção B1**, manter os asserts da raia; não há quebra demonstrada dessa expectativa. (`app/chamados/tests/test_s139.py:43`, `app/chamados/tests/test_s139.py:53`; B1 hunk `@@ -42,7 +42,8 @@`.) |
| `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit`, assert `:550` | Falha se ficarem os quatro números antigos de `juizes.py`, ou se a consolidação perder a referência correta da materialização. B1 já fornece os quatro deslocamentos; novas alterações da volta exigem conferir novamente as linhas finais. (`app/core/tests/test_medida_turno_do_par.py:258`, `app/core/tests/test_medida_turno_do_par.py:271`, `app/core/tests/test_medida_turno_do_par.py:550`; B1 hunk `@@ -255,10 +255,10 @@`.) |
| BUG-PAR-103: texto preservado e exatamente uma DEVOLUCAO, asserts `:526` e `:528` | Falhariam se a devolução fosse substituída pela reabertura B1. Essa substituição apagaria o texto e produziria outro ato; não é consequência necessária do merge fornecido. (`app/chamados/tests/test_bug_par_projetado.py:526`, `app/chamados/tests/test_bug_par_projetado.py:528`; B1 `models.py`, hunk de ressuscitação.) |
| Tarefa por DEVOLUCAO: assert `:229`; matriz de atos `:733` | A expectativa DEVOLUCAO→validar/admin continua coerente. Para ressuscitação real B1, a expectativa deve ser REABERTURA→responder/colaborador; misturar os dois fluxos causaria falha de razão/dono. (`app/chamados/tests/test_e2_tarefa_do_dia.py:229`; `app/chamados/tests/test_e3_portas_tarefa.py:733`; `app/chamados/services/tarefa_do_dia.py:152`; B1 hunk de ressuscitação.) |
| Ambientes que extraem o corpo do C6 | Aplicar somente a mudança de `juizes.py` deixaria dependências ausentes nos extratores. **A B1 completa já acrescenta o juiz e `SimpleNamespace` aos apoios correspondentes**; não é correto classificar esse erro parcial como quebra do merge completo. (`app/chamados/tests/e3_c8b_apoio.py:109`; `app/chamados/tests/e3_v8_apoio.py:182`; `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:158`; `app/chamados/tests/test_e4b_classe_e_memoria.py:304`; hunks B1 desses arquivos.) |

As fixtures próprias centrais respondem pela porta real ou escrevem instante e texto juntos; os testes de travas verificam ordem, sem congelar um teto de consultas. Portanto, nem a recusa simétrica nem o SELECT adicional do save demonstram, por si, quebra desses asserts BUG-PAR. (`app/chamados/tests/test_bug_par_projetado.py:78`, `app/chamados/tests/test_bug_par_projetado.py:253`, `app/chamados/tests/test_bug_par_projetado.py:308`, `app/chamados/tests/test_bug_par_projetado.py:417`, `app/chamados/tests/test_bug_par_projetado.py:588`; B1 `models.py`, hunk `@@ -3526,44 +3545,40 @@`; execução: [não verificado].)

## 16/11 08:48 · sessao 38a79e603691 · COMPLEMENTO-LAUDO-[nome]-1611 · censo · origem=Code

**Geofence e feriado podem usar postos diferentes:** a geofence da batida normal usa `Colaborador.posto`; o feriado usa `EscalaColaborador.posto` do vínculo que responde pelo dia. O previsto vem do vínculo/tipo/célula, com participação do posto do vínculo no feriado. (`app/ponto/models.py:100`; `app/ponto/services/geofence.py:258`; `app/core/feriados.py:131`; `app/core/feriados.py:161`; `app/escala/models.py:1095`; `app/escala/models.py:1395`)

O estado relatado do `col237` — cadastro 444, vínculo 204/posto 487, troca em 22/10 — não foi consultado em banco nesta cópia: **[nao verificado]**. A quantidade atual de divergências também é **[nao verificado]**. O código contém comentário sobre a medição histórica de 05/11: 45 colaboradores com postos divergentes, dos quais 11 com município diferente. (`app/core/feriados.py:120`)

A correspondência da árvore ao HASH informado é **[nao verificado]**.

**1. Censo de escritores**

Nas tabelas, **C** significa `Colaborador.posto` e **E** significa `EscalaColaborador.posto`. São FKs independentes; C permite nulo, E exige posto. (`app/colaboradores/models.py:322`; `app/escala/models.py:873`)

| Campo | Função e escrita | Trilha | Grava o outro posto no mesmo ato? |
|---|---|---|---|
| C | `vincular_posto`: atribuição e `salvar_inteiro`. `app/colaboradores/views.py:327`, `app/colaboradores/views.py:329` | `registrar_mudanca`, tipo posto. `app/colaboradores/views.py:331` | Não no corpo desse caminho. `app/colaboradores/views.py:325–332` |
| C | `posto_add_colab`: `gravar({'posto': posto})`. `app/colaboradores/views.py:645` | `HistoricoVinculo`, motivo vínculo, no mesmo `atomic`. `app/colaboradores/views.py:643–651` | Não. `app/colaboradores/views.py:643–651` |
| C | `posto_remove_colab`: `gravar({'posto': None})`. `app/colaboradores/views.py:670` | `HistoricoVinculo`, motivo desvínculo, no mesmo `atomic`. `app/colaboradores/views.py:668–675` | Não. `app/colaboradores/views.py:668–675` |
| C | `admitir_colaborador`: criação com `posto_id`. `app/colaboradores/views.py:1185`, `app/colaboradores/views.py:1190` | Evento de admissão quando há `posto_id`. `app/colaboradores/views.py:1219–1227` | Não cria E nesse caminho. `app/colaboradores/views.py:1185–1229` |
| C | `transferir_posto`: atribui posto e empresa, salva inteiro. `app/colaboradores/views.py:1388–1391` | Evento de transferência, no mesmo `atomic`. `app/colaboradores/views.py:1387`, `app/colaboradores/views.py:1396–1402` | Não. `app/colaboradores/views.py:1387–1403` |
| C, regravação | `editar_colaborador`: `salvar_inteiro`, sem atribuição nova de posto no trecho. `app/colaboradores/views.py:1314–1345` | Sem chamada de trilha no trecho de persistência/retorno. `app/colaboradores/views.py:1343–1355` | Não nesse trecho. `app/colaboradores/views.py:1343–1355` |
| C, regravação | `executar_desligamento`, ramo efetivo: salva objeto inteiro após mudar situação/demissão/motivo. `app/colaboradores/services/desligamento.py:129–133` | `HistoricoVinculo` e log de desligamento. `app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:229` | Fecha vínculos; não altera E.posto. `app/colaboradores/services/desligamento.py:147–149` |
| C, regravação | `executar_reativacao`: salva inteiro após mudar situação/demissão/motivo. `app/colaboradores/services/desligamento.py:242–246` | Evento de reativação. `app/colaboradores/services/desligamento.py:251–257` | Não escreve E.posto nem reativa escala nessa função. `app/colaboradores/services/desligamento.py:235–258` |
| C, persistência delegada | `onboarding_salvar_selfie`: `colab.foto.save(..., save=True)`. `app/core/views.py:1061` | Sem chamada de trilha nesse bloco. `app/core/views.py:1048–1064` | Sem escrita explícita de E. A extensão interna da regravação por `FieldFile.save` é **[nao verificado]**. |
| E | `executar_vinculo`, existente: copia C.posto e faz `save()` completo. `app/colaboradores/services/vinculo.py:275`, `app/colaboradores/services/vinculo.py:283` | Histórico condicionado a `acao != 'no_op'`; log de diferenças inclui posto. `app/colaboradores/services/vinculo.py:36`, `app/colaboradores/services/vinculo.py:349–358`, `app/colaboradores/services/vinculo.py:371–379` | Lê C, não o escreve. `app/colaboradores/services/vinculo.py:274–283` |
| E | `executar_vinculo`, novo: cria copiando C.posto. `app/colaboradores/services/vinculo.py:287–296` | Histórico acima e log de criação. `app/colaboradores/services/vinculo.py:354`, `app/colaboradores/services/vinculo.py:381–383` | Não escreve C. `app/colaboradores/services/vinculo.py:287–297` |
| E | `criar_trecho_retroativo`: cria vínculo inativo copiando `ec.posto`. `app/colaboradores/services/vinculo.py:967–974` | O comando chamador registra log de criação; a foto desse log não inclui posto. `app/colaboradores/management/commands/corrigir_escala_retroativa.py:196–201` | Não escreve C. `app/colaboradores/services/vinculo.py:970–974` |
| E | `ajustar_vinculo_pelo_sistema`: cria copiando `ec.posto`. `app/colaboradores/services/vinculo.py:1084–1092` | Chamador `aplicar_no_vinculo` registra `HistoricoVinculo`, com IDs antigo/novo. `app/escala/services/escala_auto_executor.py:101–110` | Não escreve C. `app/colaboradores/services/vinculo.py:1087–1092` |
| E | `desfazer_ajuste_do_sistema`, recriação: copia `antigo.posto`. `app/colaboradores/services/vinculo.py:1110–1113` | Chamador `desfazer` registra `HistoricoVinculo`. `app/escala/services/escala_auto_executor.py:144–149` | Não escreve C. O outro ramo apenas remove o novo e reabre o antigo. `app/colaboradores/services/vinculo.py:1101–1113` |

Os pontos de persistência abaixo completam o censo das portas:

| Sítio | Alcance comprovado |
|---|---|
| `colaborador.gravar` | Atribui campos e chama `save(update_fields=...)`; operação posto permite `posto` e `empresa`. Não contém histórico nem sincronização de E. `app/colaboradores/services/colaborador.py:33`, `app/colaboradores/services/colaborador.py:55–62` |
| `colaborador.salvar_campos` | Salva os campos informados; pode aceitar posto pela operação correspondente. Não contém histórico nem sincronização de E. `app/colaboradores/services/colaborador.py:65–70` |
| `colaborador.salvar_inteiro` | Faz `colab.save()` completo; confere a operação, não restringe os campos do objeto. `app/colaboradores/services/colaborador.py:73–77` |
| `colaborador.criar_colaborador` | Encaminha os campos para `Colaborador.objects.create`. `app/colaboradores/services/colaborador.py:91–93` |
| `Colaborador.save` | Termina em `super().save(*args, **kwargs)`; não atribui posto nessa implementação. `app/colaboradores/models.py:415–439` |
| `colaborador.atualizar_em_lote` | Rejeita expressamente `posto` e `posto_id`. `app/colaboradores/services/colaborador.py:80–88` |
| `vinculo.gravar_regua` | Porta genérica com `qs.update(**campos)`; os quatro chamadores consultados passam campos da régua, sem posto. `app/colaboradores/services/vinculo.py:1048–1070`; `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:719`, `app/escala/regua_defesa.py:742`, `app/escala/regua_defesa.py:765` |

Admin, importação, comandos e signals:

- Os admins de C e E herdam `SoLeitura`, que bloqueia inclusão/alteração/exclusão e torna os campos somente leitura. (`app/colaboradores/admin.py:48`; `app/escala/admin.py:19`; `app/core/admin.py:59–75`)
- O importador Nexti consultado salva telefone/email, sem posto. (`app/colaboradores/management/commands/importar_telefones_nexti.py:113`)
- `smoke_chamados._fixture` cria C com posto, mas o comando exige banco com `test` no nome e desfaz a fixture por rollback. (`app/chamados/management/commands/smoke_chamados.py:33–45`; `app/chamados/management/commands/smoke_chamados.py:78`)
- Os receivers consultados não sincronizam C.posto e E.posto: tratam onboarding/grupo, eventos de posto, regeneração, previsto, disputas e folgas. (`app/colaboradores/signals.py:9`, `app/colaboradores/signals.py:49`, `app/colaboradores/signals.py:102`, `app/colaboradores/signals.py:119`; `app/escala/signals.py:19`, `app/escala/signals.py:35`, `app/escala/signals.py:87`, `app/escala/signals.py:138`, `app/escala/signals.py:156`, `app/escala/signals.py:186`)

Outros escritores por mecanismo dinâmico não resolvido na leitura estática: **[nao verificado]**.

**2. Censo de leitores**

As referências incluem acesso ao atributo, filtros/projeções ORM, relações reversas e carregamento explícito por `select_related`. Nas referências com várias linhas, todas pertencem ao arquivo indicado.

**Cadastro e serviços de colaboradores**

| Campo | Função | Arquivo:linha |
|---|---|---|
| C | `vincular_colaboradores` | `app/colaboradores/views.py:41,42,46,72,80,105,139` |
| C | `passe_livre` | `app/colaboradores/views.py:205` |
| C | `painel_vinculo` | `app/colaboradores/views.py:267` |
| C | `vincular_posto` | `app/colaboradores/views.py:319,326,338,342,357,361` |
| C | `vincular_escala` / `_render_row` | `app/colaboradores/views.py:386,396,401` |
| C | `detalhe_colaborador` | `app/colaboradores/views.py:462` |
| C | `painel_posto` | `app/colaboradores/views.py:602` |
| C | `buscar_colabs_posto` | `app/colaboradores/views.py:619,625` |
| C | `posto_add_colab` | `app/colaboradores/views.py:642` |
| C | `posto_remove_colab` | `app/colaboradores/views.py:667` |
| C | `toggle_posto`, relação reversa | `app/colaboradores/views.py:884` |
| C | `painel_colaborador` | `app/colaboradores/views.py:924` |
| C | `lista_colaboradores` | `app/colaboradores/views.py:1021,1026,1052` |
| C | `transferir_posto` | `app/colaboradores/views.py:1381` |
| C | `gestao_usuarios` | `app/colaboradores/views.py:1450` |
| C | `lista_beneficios` | `app/colaboradores/views.py:1683,1687,1692,1696,1752,1753` |
| C | `calendario_ponto`, carregamento | `app/colaboradores/views.py:1947` |
| C | `agenda_modal_view`, carregamento | `app/colaboradores/views.py:2206` |
| C | `painel_situacional` | `app/colaboradores/views.py:2599,2625` |
| C | `painel_situacional_linhas` | `app/colaboradores/views.py:2785,2789` |
| C | `disponibilidade` | `app/colaboradores/views.py:2852,2858,2862,2891,2902,2908,2918,2928` |
| C | `Posto.colaboradores_ativos`, reversa | `app/colaboradores/models.py:280` |
| C | `ColaboradorAdmin`, exibição | `app/colaboradores/admin.py:49` |
| C | `get_postos_visiveis` | `app/colaboradores/mixins.py:231,232` |
| C | `get_colaboradores_visiveis` | `app/colaboradores/mixins.py:260` |
| C | `posto_post_save_revalida_disputas` | `app/colaboradores/signals.py:72` |
| C | `montar_linha_colab` / `_cell_colab_status` | `app/colaboradores/painel_cell.py:40,56,57,58,77` |
| C | `pendencias_do_colaborador` / `contador` | `app/colaboradores/services/cadastro.py:80,105,106,111` |
| C | `executar_desligamento` / `executar_reativacao` | `app/colaboradores/services/desligamento.py:114,141,253` |
| C | `contexto_detalhe` | `app/colaboradores/services/detalhe.py:92,93,94,133,134,137` |
| C | `montar_situacional` | `app/colaboradores/services/situacional.py:179,201,202,203` |
| C | `registrar_mudanca`, posto padrão da trilha | `app/colaboradores/services/historico_vinculo.py:16` |
| C | `ficha_do_colab` | `app/colaboradores/services/ficha.py:211,212,218` |
| E | `linha_do_vinculo` / carga em `ficha_do_colab` | `app/colaboradores/services/ficha.py:95,123` |
| E e C | `contexto_drawer`: prefere E, fallback C | `app/colaboradores/services/drawer.py:27,35` |
| C | `julgar_vinculo` | `app/colaboradores/services/vinculo.py:74` |
| C | `executar_vinculo` | `app/colaboradores/services/vinculo.py:275,289,296` |
| E | `_foto_da_trilha` | `app/colaboradores/services/vinculo.py:36` |
| E | `reavaliar_pela_troca_de_municipio` | `app/colaboradores/services/vinculo.py:953,954` |
| E | `criar_trecho_retroativo` | `app/colaboradores/services/vinculo.py:971` |
| E | `ajustar_vinculo_pelo_sistema` | `app/colaboradores/services/vinculo.py:1090` |
| E | `desfazer_ajuste_do_sistema` | `app/colaboradores/services/vinculo.py:1111` |
| E | `presenca_em_feriado` | `app/colaboradores/services/feriado_do_posto.py:96,105` |
| E | `raios_fora_do_padrao` | `app/colaboradores/services/geofence_raios.py:63,64` |
| E | `marcar_feriado_do_posto.Command._marcar` | `app/colaboradores/management/commands/marcar_feriado_do_posto.py:104,110` |
| C | `pauta_aparelhos_compartilhados.Command.handle` | `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:48,58,63,78,109` |

**Escala, célula e previsto**

| Campo | Função | Arquivo:linha |
|---|---|---|
| C | `escala_buscar_colabs` | `app/escala/views.py:248,249` |
| C | `propostas` | `app/escala/services/propositor.py:443,542` |
| C | `EIXOS` / `ranking` | `app/escala/services/furos_diarios.py:118,119,135,137,145,148,156,159` |
| C | `ranking_colabs` | `app/escala/services/furos_diarios.py:180,182,183,237` |
| C | `universo_empresa` | `app/escala/services/furos_diarios.py:259,279,280,325,326,327` |
| E | `minha_escala` | `app/escala/views.py:17,20,25` |
| E | `EscalaColaboradorAdmin` | `app/escala/admin.py:27,31` |
| E | `plano_folgas`, reversa `Posto.escalas` | `app/escala/views_plano_folgas.py:43,44`; FK em `app/escala/models.py:873` |
| E | `_grid_ctx` | `app/escala/views_plano_folgas.py:58,59` |
| E | `escalas_do_periodo_por_colab`, carga posto/município | `app/escala/alimentacao.py:148,152,157,158` |
| E | `vinculos_5x2_6x1_trabalha_feriado` | `app/escala/models.py:928,932` |
| E, indireto | `feriado_vira_folga` → `lugar_do_vinculo` | `app/escala/models.py:1394,1395`; `app/core/feriados.py:131,132` |
| E | `observado_meio_periodo` | `app/escala/services/meio_periodo.py:153,187,188` |
| E | `beco_em_lote` | `app/escala/services/cadastro_realidade.py:513,517` |
| E | `_vinculos` | `app/escala/services/cadastro_realidade.py:761,762,765,768` |
| E | `_colabs_do_posto` | `app/escala/services/semana_do_posto.py:76,77,79` |

`vinculo_do_dia` não compara C.posto com E.posto: lê a geradora da célula, procura aquele vínculo e verifica suas datas; sem geradora, escolhe entre os vínculos que cobrem a data por `(ativa, data_inicio)`. (`app/escala/alimentacao.py:269–286`)

Os caminhos afins que alimentam essa seleção são:

| Função | Campo/caminho | Arquivo:linha |
|---|---|---|
| `vinculo_do_dia_carregado` | Compõe carregadores; chega a E pelo vínculo selecionado | `app/escala/alimentacao.py:316,317` |
| `vinculos_do_dia_carregado` | Mesma seleção para vários colaboradores | `app/escala/alimentacao.py:329,330` |
| `vinculos_dos_dias` | Carrega células/vínculos e chama `vinculo_do_dia` | `app/escala/alimentacao.py:355,357,358` |
| `escala_vigente` | Célula/geradora e mesma seleção | `app/escala/servico_jornada.py:46,48,61,63,64,66` |
| `_esc_vigente_do_dia` | Mesma seleção para o previsto | `app/escala/utils.py:2180,2181,2191,2194` |
| `minutos_previstos_do_dia` | Vínculo escolhido, trabalho e marcos; E participa pelo feriado | `app/escala/utils.py:2260,2266,2269`; `app/escala/models.py:1395` |
| `previstas_do_periodo` | Percorre fatias de vínculo e consulta `eh_dia_trabalho` | `app/escala/alimentacao.py:389–397` |

Há consumidores de valores de posto já derivados em `cadastro_realidade.acao`, `lista` e `pdf_bytes`, além do template e da busca do Plano de Folgas. (`app/escala/services/cadastro_realidade.py:713,725,905,931,962,1083,1084`; `app/templates/escala/cadastro_x_realidade.html:21,23`; `app/templates/escala/plano_folgas.html:245,247,251,294,296`)

**API, core, chamados e comunicados**

| Campo | Função | Arquivo:linha |
|---|---|---|
| C | `_validar_assinatura_keystore` | `app/api/views.py:191`; `app/api/views_core.py:114` |
| C | `api_me`, nome do posto | `app/api/views.py:344`; `app/api/views_core.py:332` |
| C, indireto | `api_me` → `posto_geo_do_colab` | `app/api/views.py:351`; `app/api/views_core.py:339`; `app/ponto/services/geofence.py:223,241` |
| E | `api_escala_v2` | `app/api/views.py:957` |
| C | `api_mensageria_sinais`, reversa | `app/api/views_mensageria.py:449` |
| C | `api_mensageria_situacao_ferias` | `app/api/views_mensageria.py:807,833` |
| E | `_onde_trabalha` | `app/api/views_mensageria.py:979–982` |
| C | `api_mensageria_pessoa` | `app/api/views_mensageria.py:1063` |
| C | `_cadastro_incompleto` | `app/api/views_mensageria.py:1287,1288` |
| C | `detectar_anomalias` | `app/chamados/detector_anomalias.py:89,101,104,105,109` |
| C | `abrir_chamado_local` / `abrir_chamado_para_colaborador` | `app/chamados/services/abertura.py:62,187` |
| C | `_texto` / `sincronizar_adesao` | `app/chamados/services/adesao.py:15,63` |
| C | `montar_qs_filtrado` / `montar_contexto_painel` | `app/chamados/services/painel.py:99,130,266` |
| C | `fio_colaborador` | `app/chamados/views.py:695` |
| C | `auditar_invariantes_chamados.Command.handle`, carga | `app/chamados/management/commands/auditar_invariantes_chamados.py:1218` |
| C | `para_colaborador` / `audiencia_de` | `app/comunicados/services.py:23,63` |
| C | `candidato` / `parecidos` | `app/core/busca.py:89,104` |
| C | `qs_colaborador` / `_colab` | `app/core/busca.py:127,154` |
| C | `cell_ultimas_batidas` | `app/core/cell.py:11` |
| C | `montar_painel_operacional` | `app/core/services/painel_op.py:69,260,261` |
| C | `dashboard` | `app/core/views.py:176,195,213` |
| C | `dashboard_supervisor` | `app/core/views.py:352,353,362,412` |
| C | `painel_tv` | `app/core/views.py:797,801,831` |
| C | `batida_detalhe_json` | `app/core/views.py:979,1012` |
| C | `dashboard_dados` | `app/core/views.py:1164,1180,1194` |
| E | `lugar_do_vinculo` | `app/core/feriados.py:131,132` |
| E, indireto | `feriados_do_colab` | `app/core/feriados.py:161` |
| E | `regua_para`, praça do vínculo do dia | `app/core/regua_cct.py:329,330` |
| C | `lavrar_placar_situacional.Command.handle`, carga | `app/core/management/commands/lavrar_placar_situacional.py:55` |

**Ponto: modelos, serviços, telas e comandos**

| Campo | Função | Arquivo:linha |
|---|---|---|
| C | `Batida.posto_para_calculo`, após `posto_efetivo` | `app/ponto/models.py:100` |
| C | `AlertaGeofence.ainda_vale` | `app/ponto/models.py:265` |
| C | `publicar_batida` | `app/ponto/realtime.py:28` |
| C | `bloquear_se_sem_posto` | `app/ponto/utils.py:25` |
| C | `abriu_nao_bateu.com_nomes` | `app/ponto/services/abriu_nao_bateu.py:113,115,143` |
| C | `deslogados.com_nomes` | `app/ponto/services/deslogados.py:202,206` |
| C | `dia_do_colab` | `app/ponto/services/dia_do_colab.py:78` |
| C | `diagnostico_escala` | `app/ponto/services/diagnostico_escala.py:196` |
| C | `passivo_da_competencia` | `app/ponto/services/dias_em_aberto.py:109,127` |
| C | `recalcular_fechamento_mes`, carga | `app/ponto/services/fechamento.py:619` |
| E, indireto | `recalcular_fechamento_mes`, feriados | `app/ponto/services/fechamento.py:648` |
| C | `montar_resumos` | `app/ponto/services/fechamento_lista.py:79` |
| E e C | `resumo_escala`, cabeçalho: E ou C | `app/ponto/services/espelho.py:203,207` |
| E, indireto | `autoridade_do_periodo`, feriados | `app/ponto/services/espelho.py:1029` |
| C | `vinculo_contagem` | `app/ponto/services/geofence.py:48` |
| C | `posto_do_colab_agora`, sem turno aberto | `app/ponto/services/geofence.py:223` |
| C, indireto | `posto_do_colab_agora`, com turno aberto | `app/ponto/services/geofence.py:222`; `app/ponto/models.py:100` |
| C, indireto | `verificar_geofence` | `app/ponto/services/geofence.py:258`; `app/ponto/models.py:100` |
| C | `reavaliar_chamados_geofence` | `app/ponto/services/geofence.py:422` |
| C | `gestao_he._cadastro` / `enriquecer` | `app/ponto/services/gestao_he.py:60,264,288,289,324` |
| C | `alvo_da_lavra` | `app/ponto/services/pautas_do_esmeril.py:31` |
| C | `triagem_bater_ponto` | `app/ponto/services/triagem_batida.py:80,120` |
| C | `vigia_entrada.do_dia` | `app/ponto/services/vigia_entrada.py:121,136` |
| C | `vinculo_na_lista.achar` | `app/ponto/services/vinculo_na_lista.py:65,66,76,79,80,83,88` |
| C | `vinculo_na_lista.item` / `colabs_em_revisao` | `app/ponto/services/vinculo_na_lista.py:106,153,154` |
| E | `regenerar_futuras_por_feriado`, carga da geradora | `app/ponto

[truncado em 20000 de 36765 caracteres]

## 16/11 08:47 · sessao baa774361d2c · LOTE7-C7 · verificador · origem=Code

A correção de A está demonstrada estaticamente; o bloco J1 saiu da regularização, e os asserts do item 6 ganharam mordida. Isso não certifica o diff inteiro da obra nem o verde executado: a cópia disponível não tem histórico Git, árvores base/c6 ou os logs citados. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/services/regularizacao.py:102`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:457`; `[nao verificado]`)

Nas referências abaixo, todos os caminhos são relativos à cópia.

1. **Achado A — motivo da falha, caminho verde e mutantes.** [nao verificado]

   Considerando o comportamento da base informado no enunciado — `update()` que rejeita sem trilha —, o teste c7 falharia por **asserção na linha 243**, com a mensagem da linha 244: existem ausências rejeitadas persistidas. Não depende mais de importar a classe nova; importa somente a função. A execução contra a base permanece `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:235`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`)

   Na c7, o caminho é: abre `atomic` → salva a primeira rejeição → `evento`, substituído pelo mock, retorna `None` → levanta `TrilhaDaRejeicaoNaoGravada` dentro do bloco → desfaz a escrita. O teste captura sua classe-base `RuntimeError`, exige ausência de rejeição persistida, exige exceção e confere seu nome. Esse é o caminho estático esperado de aprovação; execução verde `[nao verificado]`. (`app/ponto/services/ausencia.py:122`; `app/ponto/services/ausencia.py:1483`; `app/ponto/services/ausencia.py:1497`; `app/ponto/services/ausencia.py:1505`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:241`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`)

   | Mutante | Resultado inferido pela leitura |
   |---|---|
   | Levantar outra `RuntimeError` antes de gravar | Falha na conferência do nome concreto. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:246`) |
   | Levantar depois de confirmar as rejeições | Falha porque ficaram linhas rejeitadas. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`) |
   | Não levantar e não rejeitar | Falha porque `levantou` continua `None`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:245`) |
   | Levantar exceção que não herda de `RuntimeError` | Escapa do handler e produz erro. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:241`) |
   | Levantar a própria exceção antes de tentar escrever | **Passa neste teste**: ele não exige tentativa de escrita. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:245`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:246`) |
   | Alterar outro campo, ou remover linhas, confirmar e levantar a exceção esperada | **Pode passar neste teste**: ele verifica inexistência de rejeitadas, sem comparar o conjunto original nem outros campos. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:243`) |

   A sobrevivência desses mutantes é inferência estática; mutação executada `[nao verificado]`.

2. **Achados B/C — retirada do J1 e caracterização.**

   O bloco retirado não permanece na atualização da Justificativa: ela grava `pendente` e segue diretamente para o passo 5b. Há outras invalidações por **aprovação** de Justificativa, que respondem a outra transição; não são a reaparição do bloco aprovada→pendente. (`app/ponto/services/regularizacao.py:102`; `app/ponto/services/regularizacao.py:108`; `app/ponto/services/justificativa.py:205`; `app/ponto/services/justificativa.py:215`; `app/ponto/services/justificativa.py:383`; `app/ponto/services/justificativa.py:390`)

   Igualdade byte a byte de `regularizacao.py` com `f8ee770a5`: **`[nao verificado]`**. O patch fornecido mostra a retirada, mas a árvore base não está disponível para comparação independente. `[nao verificado]`

   Permanecem referências ao J1 na docstring do módulo, na classe, no nome do teste e nas mensagens. São referências à **caracterização da ausência de invalidação**, sem exigir o comportamento removido. O selo monitora a porta de Ausência. Ausência de qualquer outra dependência no **diff inteiro base..c7**: `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:20`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:266`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:272`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:286`; `app/ponto/tests/test_lote7_selo_escritor_acorda.py:35`)

   O teste J1 afirma: retorno `ok`, Justificativa agora `pendente`, zero chamadas de `invalidar_fechamento` e carimbo preservado. Ele **não cobre Ausência intermediária**: hoje é 05/11, a entrada é de 04/11 e o laço interrompe antes de criar ausência no próprio dia 05. Passar na base e na c7 é compatível com o código e com o relato anexado; execução nas duas árvores `[nao verificado]`. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:56`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:275`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:283`; `app/ponto/services/regularizacao.py:111`)

3. **Kernel — alcance, banco e invalidação.**

   A entrada e a chamada ao serviço conferem. A enumeração **exaustiva dos sítios pertencentes ao diff base..c7** permanece `[nao verificado]`; o mapa abaixo cobre os ganchos LOTE7 identificáveis na árvore e os caminhos descritos na entrega. (`app/api/views_core.py:1134`; `app/api/views_core.py:1151`; `app/api/views_core.py:1179`)

   | Sítio/caminho | Banco | Pode chamar `invalidar_fechamento`? |
   |---|---|---|
   | `regularizar_turno_orfao` | Lê batida; lê/grava Justificativa, ausências, chamados e auditoria. | O bloco J1 não chama mais. (`app/ponto/services/regularizacao.py:52`; `app/ponto/services/regularizacao.py:91`; `app/ponto/services/regularizacao.py:118`; `app/ponto/services/regularizacao.py:130`; `app/ponto/services/regularizacao.py:150`) |
   | `garantir_ausencia` | Delega; não tem consulta própria. | Indiretamente, pela criação. (`app/ponto/services/ausencia.py:602`; `app/ponto/services/ausencia.py:610`) |
   | Criação nova → gancho em `:597` | A função grava ausência e trilha; o gancho acrescenta um SELECT do insumo. | Neste fluxo, não: falta nasce aguardando, fora da folha; os mapas são `{},{}`. (`app/ponto/services/ausencia.py:475`; `app/ponto/services/ausencia.py:562`; `app/ponto/services/ausencia.py:585`; `app/ponto/services/ausencia.py:597`; `app/ponto/turnos.py:2528`) |
   | Rejeitada existente → `_reabrir` → `mudar_estado` | Pode gravar descrição; grava estado e trilha. O gancho acrescenta dois SELECTs de insumo. | Neste fluxo, não: rejeitada→aguardando fica fora da folha nos dois lados. (`app/ponto/services/ausencia.py:433`; `app/ponto/services/ausencia.py:1297`; `app/ponto/services/ausencia.py:1315`; `app/ponto/services/ausencia.py:1319`; `app/ponto/services/ausencia.py:987`; `app/ponto/services/ausencia.py:993`; `app/ponto/turnos.py:2528`) |
   | Linha existente devolvida | Já houve consultas para localizar conflito; retorna antes do gancho novo. | Não por esse gancho. (`app/ponto/services/ausencia.py:427`; `app/ponto/services/ausencia.py:456`) |
   | `_insumo_da_folha` → `_pelo_efeito` | SELECT; devolve apenas linhas contadas pela folha. | Não. (`app/ponto/services/ausencia.py:138`; `app/ponto/services/ausencia.py:139`; `app/ponto/turnos.py:2529`) |
   | `_acordar_projecao` | Com `{},{}`, não consulta fechamento nem lê relógio; genericamente pode fazê-lo. | **Sim no grafo genérico**, em `:195`; zero chamadas nos estados acima. (`app/ponto/services/ausencia.py:183`; `app/ponto/services/ausencia.py:191`; `app/ponto/services/ausencia.py:195`) |
   | `_teto_da_linha_aberta` | SELECT de fechamento; eventual leitura da empresa. | Não diretamente; não é executado pelos mapas vazios acima. (`app/ponto/services/ausencia.py:147`; `app/ponto/services/ausencia.py:151`; `app/ponto/services/ausencia.py:183`) |
   | `invalidar_fechamento` → `competencias_do_fato` | Resolve competências e escreve `derivado_em=NULL`. | É o próprio invalid validador; não é executado pelo gancho nesses fluxos. (`app/ponto/services/fechamento.py:1599`; `app/ponto/services/fechamento.py:1649`; `app/ponto/services/fechamento.py:1656`) |

   Há efeitos indiretos de escrita: os `save()` de Justificativa/Ausência acionam cartório; o cartório pode gravar célula e evento. `carimbar`→`salvar_e_emitir` não chama recálculo imediatamente. Portanto “só leitura” descreve apenas o **acréscimo dos ganchos LOTE7 nesses estados**, não o serviço nem toda a execução do endpoint. (`app/ponto/signals.py:143`; `app/ponto/signals.py:161`; `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:1314`; `app/ponto/portas/celula.py:1201`; `app/ponto/portas/celula.py:962`)

   A seção Kernel confere nos caminhos citados e nos custos **adicionais** de um/dois SELECTs. “Zero sítios de invalidação real” é correto como resultado desses fluxos; não significa ausência de alcance sintático da função invalidante. Já “regularização, só leitura”, na abertura da entrega, é impreciso se qualificar o serviço inteiro. (`app/ponto/services/ausencia.py:597`; `app/ponto/services/ausencia.py:987`; `app/ponto/services/ausencia.py:993`; `app/ponto/services/ausencia.py:195`; `app/ponto/services/regularizacao.py:106`)

4. **Item 6 — mordida atual e mutantes sobreviventes.**

   O teste da célula agora mata: `save` substituído por `update` sem signal; juiz que retorna vazio/contadores zero; ausência de carimbo persistido; carimbo que conserva a FK das férias rejeitadas. A fixture começa com essa FK, o espião chama o juiz real, exige contador positivo para cada dia e confere os resultados persistidos. (`app/ponto/services/ausencia.py:1497`; `app/ponto/signals.py:143`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:446`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:451`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:457`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:461`)

   Ainda pode passar neste teste: [nao verificado]

   - Quebrar somente o ramo com adesão/batidas: a fixture percorre `nunca_bateu`, que incrementa `carimbadas` e retorna antes do ramo normal de `julgadas`. (`app/ponto/services/cartorio.py:1317`; `app/ponto/services/cartorio.py:1318`; `app/ponto/services/cartorio.py:1476`)
   - Gravar veredito/ata/impressão errados, ou timestamp incorreto porém não nulo, conservando contador positivo e FK nula: esses conteúdos não são comparados. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:457`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:461`; `app/ponto/portas/celula.py:1194`)

   `_nada_mudou` agora mata alteração de status/obs, exclusão ou substituição de ausência, mudança de data/status, alteração/remoção das trilhas antigas nos campos fotografados e alteração do saldo. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:510`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:531`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:533`)

   Ainda passam alterações em campos omitidos — por exemplo, `Ausencia.minutos/data_fim/descricao`, datas/dias do agendamento e descrição/IP da auditoria — e inserção de trilha nova indevida. A comparação das trilhas considera somente as PKs anteriores; “INTEIRO” significa o retrato enumerado, não a linha inteira. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:505`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:510`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:535`)

5. **Quem escreve, quem lê e significado.**

   Este é o mapa dos campos e valores comportamentais conferidos no patch e nos caminhos tocados. Censo exaustivo de **todos** os leitores/escritores do repositório: `[nao verificado]`.

   | Campo/valor | Quem escreve | Quem lê / significado |
   |---|---|---|
   | `Ausencia.pk`, vínculo ao agendamento, `tipo`, `data`, `status` | Espelho cria férias aprovadas; rejeição grava `rejeitada`. (`app/ponto/services/ausencia.py:1388`; `app/ponto/services/ausencia.py:1488`; `app/ponto/services/ausencia.py:1495`) | Snapshot e asserts identificam conjunto/status/data; folha conta aprovadas, cobrança exclui rejeitadas. São efeitos explicitamente diferentes. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:511`; `app/ponto/turnos.py:2528`; `app/ponto/turnos.py:2530`) |
   | `Ausencia.atualizado_em` | `save` da rejeição inclui o campo. (`app/ponto/services/ausencia.py:1496`) | Serve como toque/fato novo; não é carimbo da projeção. (`app/ponto/services/ausencia.py:1496`; `app/ponto/models.py:457`) |
   | `CelulaDia.colaborador/data/trabalha/origem/dna` | Fixture cria pessoa/dia, `trabalha=True`, origem gerada e marcos 08–17. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:446`) | Cartório lê dia, trabalho e DNA no julgamento/impressão. (`app/ponto/services/cartorio.py:1252`; `app/ponto/services/cartorio.py:1257`; `app/ponto/services/cartorio.py:1354`) |
   | `CelulaDia.ausencia` | Fixture semeia FK; porta `carimbar` grava cobertura. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:446`; `app/ponto/portas/celula.py:1196`) | Teste exige FK nula após rejeição; produção deriva cobertura para **cobrança**, não exclusivamente folha/aprovadas. Há concordância nesse caso de rejeição. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:462`; `app/ponto/services/cartorio.py:1228`; `app/ponto/services/cartorio.py:1356`) |
   | `CelulaDia.julgada_em` | `carimbar` grava `agora`. (`app/ponto/portas/celula.py:1194`) | Cartório usa no frescor; teste exige apenas preenchimento, não valor correto. (`app/ponto/services/cartorio.py:1260`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:461`) |
   | `julgadas` / `carimbadas` | Cartório inicializa ambos; incrementa após classificação / carimbo. (`app/ponto/services/cartorio.py:1955`; `app/ponto/services/cartorio.py:1476`; `app/ponto/services/cartorio.py:1317`; `app/ponto/services/cartorio.py:1875`) | Espião soma ambos como evidência de atividade. **Não são sinônimos**; o ramo normal pode incrementar ambos. A soma não prova dois julgamentos nem distingue os ramos. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:453`; `app/ponto/services/cartorio.py:1476`; `app/ponto/services/cartorio.py:1875`) |
   | `AgendamentoFerias.status`, inclusive `concluido/cancelado` | Cancelamento grava `cancelado`; teste de correção prepara `concluido`. (`app/ferias/services.py:688`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:562`) | Guardas recusam estados terminais; snapshot exige preservação. (`app/ferias/services.py:637`; `app/ferias/models.py:307`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:533`) |
   | `AgendamentoFerias.obs` | Cancelamento concatena assinatura/data/lote; criação aceita observação. (`app/ferias/services.py:584`; `app/ferias/services.py:685`; `app/ferias/services.py:687`) | Snapshot exige igualdade. Outro leitor infere origem por substrings: observação também carrega metadado interpretado, dívida preexistente à alteração apresentada. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:510`; `app/ferias/management/commands/listar_ferias_dp.py:27`; autoria temporal `[nao verificado]`) |
   | `PeriodoAquisitivo.dias_concedidos` | Cancelamento subtrai dias e salva. (`app/ferias/services.py:680`; `app/ferias/services.py:682`) | Helper compara com o saldo fotografado; representa dias concedidos, não diretamente saldo restante. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:501`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:531`) |
   | `LogAuditoria.pk/acao/modelo/objeto_id/valor_antes/valor_depois` | `evento` cria registro; rejeição fornece status anterior e rejeitado+agendamento posterior. (`app/core/observ.py:134`; `app/ponto/services/ausencia.py:1498`; `app/ponto/services/ausencia.py:1501`) | Teste positivo confere de/para por ausência; snapshot preserva registros anteriores. Os JSONs são envelopes genéricos contextualizados por ação/modelo, não esquema universal de status. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:218`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:512`; `app/core/models.py:296`) |
   | `Justificativa.status` | Fixture prepara aprovada; regularização grava pendente; portas de decisão gravam aprovada. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:278`; `app/ponto/services/regularizacao.py:105`; `app/ponto/services/justificativa.py:205`) | Folha lê aprovada; caracterização exige pendente sem invalidar. O significado do status concorda; permanece a lacuna de atualização da projeção. (`app/ponto/services/fechamento.py:662`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:285`) |
   | `Justificativa.saida_informada/descricao/atualizado_em` | Regularização preenche saída/descrição e salva os três junto do status. (`app/ponto/services/regularizacao.py:102`; `app/ponto/services/regularizacao.py:106`) | `saida_informada` condiciona a reabertura; o teste J1 confere somente status e efeito no fechamento. (`app/ponto/services/regularizacao.py:102`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:285`) |
   | `FechamentoMensal.derivado_em`, pessoa/mês/ano | Fixture carimba; derivação preenche; invalidação/restauração limpam. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:83`; `app/ponto/services/fechamento.py:1143`; `app/ponto/services/fechamento.py:1189`; `app/ponto/services/fechamento.py:1391`; `app/ponto/services/fechamento.py:1656`) | `_viva` testa não nulo; teto lê competências carimbadas; contador encontra nulos. Significado consistente: carimbo da derivação, NULL para nunca derivada/invalidada. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:97`; `app/ponto/services/ausencia.py:147`; `app/ponto/services/fechamento.py:1679`) |
   | Retorno `ok`, quantidade `5`, zero chamadas, datas fixas | Serviço retorna sucesso; rejeição retorna quantidade; fixtures fixam datas. (`app/ponto/services/regularizacao.py:157`; `app/ponto/services/ausencia.py:1510`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:56`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:192`) | Asserts exigem sucesso, cinco rejeições e ausência de invalidação J1; não significam validação de produção. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:283`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:286`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:456`; produção `[nao verificado]`) |

6. **Classes de reprovação.**

   - **Segundo juiz:** não identifiquei no acordador inspecionado. Ele pergunta ao predicado existente da folha e ao juiz dos dias; competências delegam à derivação existente. Ausência de duplicação no diff inteiro `[nao verificado]`. (`app/ponto/services/ausencia.py:138`; `app/ponto/services/ausencia.py:192`; `app/ponto/services/fechamento.py:1599`; `app/core/juizes.py:194`)
   - **Segundo escritor fora da porta:** a rejeição especial continua dentro de `ausencia.py`, embora seja exceção textual à afirmação “estado só muda por mudar_estado”. Escritores externos no diff inteiro `[nao verificado]`. (`app/ponto/services/ausencia.py:16`; `app/ponto/services/ausencia.py:1495`)
   - **Relógio fora da borda:** permanece. O acordador lê `localdate()` internamente e consulta fechamento para qualquer linha mudada com fim nulo, inclusive tipo de dia único; o juiz só prolonga tipos em aberto. (`app/ponto/services/ausencia.py:190`; `app/ponto/services/ausencia.py:147`; `app/ponto/turnos.py

[truncado em 20000 de 22884 caracteres]

## 16/11 08:46 · sessao 3fbea08596d5 · - · - · origem=Ronald (tmux)

**Testes próprios BUG-PAR: não encontrei quebra necessária causada pelo diff B1 fornecido; execução pós-merge [não verificado].** As fixtures principais respondem pela porta real, que grava o par (`test_bug_par_projetado.py:78-81`); as fixtures diretas já levam texto e instante juntos (`app/chamados/tests/test_bug_par_projetado.py:251-254`, `app/chamados/tests/test_bug_par_projetado.py:306-312`).

1. **`test_s139.py`: fixture B1 e expectativa RAIA devem coexistir.** A raia cria `resposta_colab='02:00'` sem instante no `create`, depois carimba por `update` (`app/chamados/tests/test_s139.py:43-47`). Com a recusa simétrica B1, o `create` antigo seria recusado antes dos asserts `app/chamados/tests/test_s139.py:53-56`; **porém o próprio B1 corrige exatamente essa fixture**, adicionando `respondida_em=agora` (anexo B1, hunk `app/chamados/tests/test_s139.py @@ -42,7 +42,8`). Portanto não relatar como quebra do merge completo. A raia mudou o teste para esperar zero batida e erro `DECLARADO-X-GRAVADO`; manter esses asserts, que são posteriores/próximos, mas distintos do argumento adicionado ao `create` (`app/chamados/tests/test_s139.py:48-56`; hunk B1 citado).

2. **`test_medida_turno_do_par.py`: conservar os dois registros independentes.** A raia congela os quatro sítios de `juizes.py` em 53/169/274/203 (`app/core/tests/test_medida_turno_do_par.py:258-261`); o B1 muda para 54/170/275/204 por seu novo import (anexo B1, `app/chamados/juizes.py @@ -2,6 +2,7`; `app/core/tests/test_medida_turno_do_par.py @@ -255,10 +255,10`). A raia também congela `_plantio_preenche_furo_na_data` em 417 (`app/core/tests/test_medida_turno_do_par.py:270-271`). São regiões distintas: hunk B1 no início do dicionário, entrada da materialização logo depois. Sem os quatro offsets B1, falharia o assert de igualdade das linhas `app/core/tests/test_medida_turno_do_par.py:550-553`; **com ambos os registros preservados, não há quebra demonstrada**. O hunk próprio original da raia não pôde ser reconstruído [não verificado].

3. **Nascer `pergunta_respondida` não acrescenta, por si, item ao censo do turno.** O instrumento seleciona de `JUIZES` somente o destino terminado em `::turnos_do_colab` (`app/core/medida_turno_do_par.py:246-258`), inicia o fechamento apenas nele e em `ponto.turnos.parear_turnos` (`app/core/medida_turno_do_par.py:345`). Seu scanner conta formas de pareamento e extração de data (`app/core/medida_turno_do_par.py:197-228`), ausentes do corpo B1 de `pergunta_respondida` (anexo B1, `app/chamados/juizes.py @@ -1531,6 +1534,24`). Não há razão estática para aumentar os contadores por esse nascimento.

4. **Os asserts de travas BUG-PAR não congelam número de queries.** Verificam travas antes da primeira batida/escrita e ordem das quatro entidades (`app/chamados/tests/test_bug_par_projetado.py:417-428`, `app/chamados/tests/test_bug_par_projetado.py:579-589`, `app/chamados/tests/test_bug_par_projetado.py:740-743`, `app/chamados/tests/test_bug_par_projetado.py:757-759`). O SELECT simples introduzido no save B1 não muda essas relações, porque não usa `FOR UPDATE` (anexo B1, `app/chamados/models.py @@ -3526,44 +3545,40`). Quebra por teto de queries nesses testes não foi identificada.

5. **Devolução BUG-PAR-103 deve continuar preservando resposta e produzindo `DEVOLUCAO`.** A porta só limpa materialização/validação, conserva o par e registra `DEVOLUCAO` (`app/chamados/services/validacao.py:293-306`). O teste exige materialização e validação nulas, texto `'01:00'` preservado e um ato `DEVOLUCAO` (`app/chamados/tests/test_bug_par_projetado.py:523-529`). Não substituir essa chamada pela reabertura B1, que zera o par e usa `REABERTURA`: é outro ato (anexo B1, `app/chamados/models.py @@ -1609,10 +1611,16`). Seria necessário reforçar o teste para verificar também o instante preservado; hoje ele verifica somente o texto (`app/chamados/tests/test_bug_par_projetado.py:523-529`).

6. **Helpers C6 herdados: B1 já traz as dependências novas.** A raia extrai C6 sem `pergunta_respondida`/`SimpleNamespace` em `app/chamados/tests/e3_c8b_apoio.py:109-116`, `app/chamados/tests/e3_v8_apoio.py:182-187`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:158-161`, `app/chamados/tests/test_e4b_classe_e_memoria.py:304-309`; os hunks B1 desses quatro arquivos adicionam essas dependências. Não apresentar o `NameError` que ocorreria ao aplicar somente `juizes.py` como quebra do B1 completo (anexo B1, hunks desses arquivos). Os testes próprios BUG-PAR usam imports normais, não extração AST (`app/chamados/tests/test_bug_par_projetado.py:19-23`, `app/chamados/tests/test_bug_par_projetado.py:78-81`).

7. **Outro contrato próprio da raia continua pertinente.** As entradas de `par_do_intervalo`, irmã, contador, classe 103 e `_validar_o_par` declaram leituras de `materializada_em`/`validada_em` como portas ou identidade do ato, não vitalidade (`app/chamados/tests/test_contract_leitores_vitalidade.py:104-124`). B1 não altera esses corpos no diff fornecido; não há incompatibilidade necessária demonstrada entre essas declarações e a recusa simétrica do par [não verificado quanto à execução].

A cópia disponível não contém repositório Git utilizável; ancestral e diff próprio exato da raia não puderam ser reconstruídos [não verificado]. Nenhum teste foi executado; nenhum arquivo foi escrito [não verificado]. [nao verificado]

## 16/11 08:46 · sessao 4f345ea7519a · - · - · origem=Ronald (tmux)

A extração não contém repositório Git acessível; a autoria/base exata das mudanças da raia é [não verificado]. A comparação abaixo usa somente a árvore lida e os hunks B1 fornecidos.

| Cruzamento | O que a raia supõe e o que B1 muda | Coerência da volta |
|---|---|---|
| C6 e par projetado | `projecao_temporal_da_pergunta` corta conjuntamente instante e texto futuros, além dos carimbos de validação/materialização; `_pergunta_viva_calc` testa o par retornado, com `respondida is not None and strip(resposta)`. B1 apenas nomeia esse mesmo teste por `pergunta_respondida(SimpleNamespace(...))`. **Não muda a tabela-verdade nem deve julgar o objeto atual no lugar do par projetado.** (`app/chamados/juizes.py:1388`, `:1393`, `:1403`, `:1420`; B1 `juizes.py @@ -1417,7 +1418,9 @@`, `@@ -1531,6 +1534,24 @@`.) | Preservar chamada nova B1, `SimpleNamespace`, `texto_vazio` e o juiz nos ambientes que extraem corpos reais; manter expectativa de corte temporal do par. (`app/chamados/juizes.py:1393`, `:1420`; B1 `e3_c8b_apoio.py @@ -107,7 +107,7 @@`, `e3_v8_apoio.py @@ -182,8 +182,9 @@`, `test_e3_tarefa_admin_sem_banco.py @@ -157,8 +157,9 @@`.) |
| Vida lida pela tarefa | A tarefa recebe `viva_c6`; a exposição administrativa não inventa vitalidade nem converte `RESPONDER/colaborador` em `VALIDAR/admin`. A carga chama C6 com `agora=self.agora`. B1 conserva a semântica de C6, portanto não há inversão demonstrada de dono por nascimento do juiz. (`app/chamados/services/tarefa_do_dia.py:256`, `:260`, `:268`, `:521`, `:525`; B1 `juizes.py @@ -1417,7 +1418,9 @@`.) | Conservar `viva_c6` e o mesmo relógio; os testes que carregam C6 por AST precisam carregar também o novo juiz e `SimpleNamespace`. (`app/chamados/services/tarefa_do_dia.py:521`, `:525`; B1 hunks dos apoios citados.) |
| Decisão, devolução e ressuscitação | `_pela_decisao` já diferencia: `RESPOSTA` vazia → responder/colaborador; resposta com texto → validar/admin; `RECUSA` ou `REABERTURA` → responder/colaborador; `DEVOLUCAO` → validar/admin. B1 faz ressuscitação passar por `reabrir_para_correcao(... ato=REABERTURA)`, zerando par inteiro e guardando foto. (`app/chamados/services/tarefa_do_dia.py:148`, `:152`, `:154`; B1 `models.py @@ -1609,10 +1611,16 @@`.) | Preservar a nova REABERTURA de ressuscitação; não reutilizar essa porta para a devolução BUG-PAR ao admin. BUG-PAR conserva resposta e usa DEVOLUCAO. (`app/chamados/services/validacao.py:269`, `:272`, `:293`, `:299`, `:303`; `app/chamados/services/disputa_emissao.py:1609`, `:1631`; B1 hunk de ressuscitação.) |
| Consumidor dos atos novos | A carga lê toda sequência em ordem `(pergunta_id, ato_em, pk)`, corta atos por `agora` e passa ao corpo puro; a consulta de passivo é pela autoridade `linhas_da_projecao_sem_linha`. B1 acrescentará a REABERTURA na ressuscitação, que pode se tornar a última decisão e trocar uma antiga validação/estado sem ato por `devolvida_ao_colaborador`. (`app/chamados/services/tarefa_do_dia.py:410`, `:412`, `:419`, `:557`; `app/chamados/services/tarefa_do_dia.py:152`; B1 `models.py @@ -1609,10 +1611,16 @@`.) | Testar a sequência resultante da ressuscitação real, a foto da REABERTURA e o novo último ato; não esperar a ausência de ato ou continuar tratando a validação anterior como última decisão. (`app/chamados/services/disputa_emissao.py:1631`, `:1635`, `:1645`; B1 `test_f1nova_b1_juiz_respondida.py:260–289` no anexo.) |
| Fila e dono | A fila vem de `qs_na_fila(agora=...)`; ação admin só recebe porta se âncora está na fila. O dono colab exige pergunta pendente no app e ação RESPONDER; dono admin exige porta de ação na fila; prioridade é colab, admin, chamado, sistema. B1 não contém alteração direta nessas regras. (`app/chamados/services/tarefa_do_dia.py:142`, `:436`, `:610`, `:613`, `:622`; diff B1 fornecido.) | Não inferir `fio='admin'` apenas de uma pergunta viva ou DEVOLUCAO: precisa da fila/porta. Preservar distinção entre nova REABERTURA B1 e DEVOLUCAO BUG-PAR no teste integrado de fila. (`app/chamados/services/tarefa_do_dia.py:154`, `:610`, `:613`, `:629`.) |
| Pendente no app | A partição ainda pergunta só `respondida_em is None` e disputa aberta; a tarefa a consome antes do C6. B1 não migra esse leitor para `pergunta_respondida`, de modo que legado “instante sem texto” continua podendo ficar fora dos pendentes do app embora o juiz novo diga não respondida. (`app/chamados/services/pergunta_no_app.py:76`, `:81`; `app/chamados/services/tarefa_do_dia.py:513`, `:523`; B1 docstring `test_f1nova_b1_juiz_respondida.py:8–9` e `pergunta_respondida` no hunk `juizes.py @@ -1531,6 +1534,24 @@`.) | Não apresentar migração da partição como efeito já entregue por B1; é leitor de classe posterior e o anexo expressamente reserva B/C/D para B2. (Mesmas referências.) |

Os leitores específicos BUG-PAR têm estes cruzamentos adicionais:

- `metade_irma_do_intervalo` define a irmã “respondida” por filtro SQL de instante e teste Python de texto, com `materializada_em` nulo. É a mesma conjunção do juiz novo: a volta deve nomear `pergunta_respondida` no teste Python preservando o filtro de materialização, identidade, ambiguidade e pergunta em voo; não há mudança de universo demonstrada por essa troca. (`app/chamados/services/materializacao.py:137`, `:143`, `:145`, `:146`, `:147`; B1 `juizes.py @@ -1531,6 +1534,24 @@`.)
- `par_do_intervalo` testa somente texto da metade principal antes de procurar irmã; isso é leitor só-texto que B1 declara não migrar ainda. Alterá-lo para o juiz pode ser exigência futura de B2, mas não é quebra demonstrada por B1; fixtures que tentem nascer com texto sem instante passarão a ser recusadas. (`app/chamados/services/materializacao.py:186`, `:188`, `:190`; B1 `models.py @@ -3526,44 +3545,40 @@`, docstring de `pergunta_respondida`.)
- `_qs_recusadas_paridade_de_intervalo` já usa a face SQL `q_respondida_com_texto`, cujo corpo B1 não altera; B1 muda a autoridade documentada para o juiz novo. A expectativa de conjunto deve permanecer. (`app/chamados/services/materializacao.py:246`, `:249`; `app/chamados/juizes.py:1554`, `:1559`; B1 `juizes.py @@ -1552,7 +1573,7 @@`.)
- `paridade_absolvida_bug_par_103` e `_situacao_da_irma` repetem I∧T em Python; são pontos da volta a nomear pelo juiz, sem trocar materialização/corte/via/lâmpada ou os rótulos da partição. (`app/chamados/services/materializacao.py:274`, `:275`, `:276`, `:278`, `:305`, `:307`; B1 `juizes.py @@ -1531,6 +1534,24 @@`.)
- A DEVOLUCAO de BUG-PAR chama `linhas_da_projecao_sem_linha` antes de limpar materialização/validação, preserva resposta e grava DEVOLUCAO com texto e motivo BUG-PAR-103; `rotulo_bug_par_103` lê especificamente esse ato. Trocar por REABERTURA apagaria o par e também perderia o gatilho do rótulo, além de mudar o dono na tarefa. (`app/chamados/services/validacao.py:293`, `:294`, `:299`, `:303`, `:304`; `app/chamados/services/materializacao.py:281`, `:291`; `app/chamados/services/tarefa_do_dia.py:152`, `:154`; B1 `models.py @@ -1609,10 +1611,16 @@`.)
- A autoridade de passivo materializa atos RESPOSTA e VALIDACAO conforme os carimbos existentes; B1 passa a usá-la antes da ressuscitação e depois registra a foto na REABERTURA. A tarefa lê tanto o resultado dessa autoridade quanto a sequência e, portanto, é consumidora direta dos atos que B1 passa a produzir. (`app/chamados/models.py:4067`, `:4074`, `:4076`, `:4078`, `:4080`; `app/chamados/services/tarefa_do_dia.py:410`, `:419`; B1 `models.py @@ -1609,10 +1611,16 @@`.)

`app/docs/JUIZES.md` descreve a tarefa como decisão pela última `DecisaoPergunta`, efeito pela lâmpada e chamado pela fila, mas não registra C21 no trecho de CHAMADO; o registro atual vai de tarefa C19 a tranca C20. B1 inclui `_C21` e seu registro em `core/juizes.py`, enquanto o diff recebido exclui docs: a atualização concreta de JUIZES.md pela B1 é [não verificado]. A volta precisa reconciliar a documentação com C21, par I∧T, recusa simétrica e ressuscitação pela REABERTURA, preservando a distinção da DEVOLUCAO ao admin. (`app/docs/JUIZES.md:78`, `:80`, `:82`, `:94`, `:100`; `app/core/juizes.py:165`, `:299`, `:300`; B1 `core/juizes.py @@ -163,6 +163,11 @@`, `@@ -298,6 +303,7 @@`.)

Os testes de tarefa já distinguem os dois sentidos e não devem ser classificados como quebra automática: `Caso3MaterializacaoRecusada` espera `('materializacao_recusada', [('validar', 'admin')])` para DEVOLUCAO; a matriz E3 separa `recusa`, `reabertura` e `devolucao`, esperando razões distintas. O risco está nas fixtures/ambientes de C6 e nas expectativas de ressuscitação sem REABERTURA, não nesses asserts por si sós. (`app/chamados/tests/test_e2_tarefa_do_dia.py:225`, `:227`, `:229`; `app/chamados/tests/test_e3_portas_tarefa.py:725`, `:733`, `:734`; B1 hunks de apoios e ressuscitação citados.)

_5969 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
