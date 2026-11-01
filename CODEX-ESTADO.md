# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 02:02** · publicado em **01/11/2026 02:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 02:02 · sessao 99fd5c598699 · GRADE-FOLGA-JUIZ · verificador · origem=Code

**Os dois PROCEDENTES estão curados.** O teste novo morde as exclusões de `precedencia.py` pelo percurso, e a descrição de horizonte/`--forcar` corresponde ao código. **Não libero pouso incondicional:** o ensaio exigido antes dele continua **[não verificado]**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:245`, `app/ponto/services/cartorio.py:178`, `app/ponto/services/cartorio.py:174`)

Referências abaixo são da cópia fornecida como **16298e59**. Exercitei a catraca em memória, substituindo Django por `unittest` e o diretório temporário por arquivos simulados; não executei a integração Django nem consultei banco ou logs: **[não verificado]**.

**1. Cura e mordida**

A execução em memória dos **12 testes da catraca passou**, com **22 achados e 22 entradas na ALLOWLIST**. O teste novo falhou, por asserção, em cada mutante abaixo. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:139`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:176`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:260`)

| Mutante aplicado somente em memória | Teste novo | [nao verificado]
|---|---|
| Excluir arquivos chamados `precedencia.py` no filtro de extensão | **RED**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:128`) |
| Pular especificamente `ponto/precedencia.py` pelo caminho relativo | **RED**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:129`) |
| Podar o diretório `ponto` em `dirs[:]` | **RED**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:125`) |
| Fazer `andar` ignorar todas as funções de `ponto/precedencia.py` | **RED**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:113`) |

Portanto, a lacuna da rodada anterior foi fechada para essas formas de regressão. Isso não transforma a AST em detector universal: `Q`, kwargs dinâmicos, reflexão e código no nível de módulo continuam limites declarados. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:17`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:245`)

**2. Quem escreve e quem lê**

| Valor | Escrita → leitura |
|---|---|
| Fonte usada pelo teste novo | O próprio teste escreve uma **fonte sintética literal** em `ponto/precedencia.py` temporário; `achados` percorre e abre esse arquivo, e `achados_da_fonte` analisa a AST. A asserção exige somente `outro/filtro`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:251`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:130`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:260`) |
| Fonte real do juiz | **O teste novo não a lê nem executa o juiz.** Os testes anteriores de cópia literal é que usam `inspect.getsource` do predicado e do carregador reais. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:212`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:220`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:251`) |
| Versão e impressão | `REGRA_VERSOES` declara; `versao_para` escolhe a maior versão aplicável à competência; `_regra_do_dia` fornece o valor aos ramos; `impressao_insumos` incorpora `regra:N`. (`app/ponto/services/cartorio.py:111`, `app/ponto/services/cartorio.py:214`, `app/ponto/services/cartorio.py:963`, `app/ponto/services/cartorio.py:371`) |
| Ata e carimbos | O cartório monta a ata, chama a lavra existente e entrega os carimbos à porta; a porta salva célula e evento no `atomic` existente. (`app/ponto/services/cartorio.py:1580`, `app/ponto/services/cartorio.py:1602`, `app/ponto/portas/celula.py:639`) |

O texto novo está correto no alcance principal: a v3 altera a impressão da competência **10 em diante**; a 09 mantém versão 1. Sem `--forcar`, uma célula sem carimbo válido ou com impressão divergente continua elegível. (`app/ponto/services/cartorio.py:214`, `app/ponto/tests/test_o219_versao_da_regra.py:85`, `app/ponto/services/cartorio.py:1176`)

“Todas as células passadas” descreve o universo padrão do comando, **com `--apply` para persistir e sem `--limite`**. `forcar` remove o pulo por impressão atual; não elimina outros `continue`, como ausência do dia na grade. É uma ressalva de precisão, sem nova regressão demonstrada. (`app/ponto/management/commands/processar_cartorio.py:27`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:56`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1183`)

**3. Triagem: algum descarte errado?**

**Sim: o “NÃO PROCEDE” do pré-requisito precisa ser qualificado.** Não há prova de que faltou ensaio; também não há prova disponível de que foi cumprido. Atribuí-lo à sessão principal não dispensa a exigência explícita anterior ao pouso. Os resultados citados em logs, inclusive suficiência do ensaio da v9 para esta entrega, permanecem **[não verificado]**. (`app/ponto/services/cartorio.py:174`, `app/docs/BACKLOG.md:796`)

Os demais encaminhamentos são aceitáveis com estes limites:

- **Documentação:** BACKLOG ainda enumera fase como migrada; a linha seguinte registra o desmembramento. Pode ser corrigido no marco do pouso; não deve virar dívida posterior. A edição alegada na principal é **[não verificado]**. (`app/docs/BACKLOG.md:796`, `app/docs/BACKLOG.md:797`, `app/docs/LEIS.md:132`)
- **Juízes novos:** o corpo dos commits fornecido resolve a dúvida anterior: **um na obra, zero adicionais no delta**, compatível com registro e corte. (`app/core/juizes.py:192`, `app/docs/CORTES.md:416`)
- **C5, gerador e fase:** continuam filas declaradas; fase permanece fora desta obra pelo aval informado. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`, `app/docs/BACKLOG.md:797`)

**4. Classes de reprovação**

| Classe | Situação nesta verificação |
|---|---|
| Segundo juiz | Os sítios curados delegam; **C5 e gerador continuam abertos**, não curados pela obra. (`app/ponto/precedencia.py:505`, `app/ponto/services/declarar_dia.py:74`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`, `app/escala/models.py:1280`) |
| Segundo escritor fora da porta | Não localizado no trecho alterado: overlay escreve dict; persistência usa a porta existente. (`app/escala/utils.py:1126`, `app/ponto/services/cartorio.py:1582`, `app/ponto/portas/celula.py:807`) |
| Relógio fora da borda | Não localizado nas funções novas; o comando captura a data na borda. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:52`, `app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:222`) |
| Derivação paralela | Grade pergunta ao juiz; calendário compartilha a função entre classificação e publicação. (`app/escala/utils.py:1124`, `app/colaboradores/services/calendario.py:839`, `app/colaboradores/services/calendario.py:840`) |
| Selo sem mordida | **Curado para os mutantes exercitados acima.** (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:260`) |
| Ordem de trava | Nenhuma aquisição nova no delta; ausência de corrida operacional: **[não verificado]**. (`app/ponto/portas/celula.py:639`) |
| Pouso antes do pré-requisito | Condição permanece expressa; cumprimento: **[não verificado]**. (`app/ponto/services/cartorio.py:174`) |

**5. LEI-AKITA dos commits** [nao verificado]

A linha da **obra/303970f1** corresponde ao desenho: juiz, carregador, predicado, três sítios migrados e leitores indicados existem; `juiz unico` é padrão cadastrado. Resultados dos RED citados: **[não verificado]**. (`app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:212`, `app/ponto/precedencia.py:505`, `app/ponto/services/declarar_dia.py:74`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65`, `app/docs/GLOSSARIO.md:12`)

A linha do **delta/16298e59** é **parcialmente literal**: `achados`, `test_so_encolhe`, zero juízes adicionais e o padrão conferem. Porém, `testemunha=folga_lancada_do_dia` não descreve o que o teste novo lê: ele lê **o resultado de `achados` sobre fonte sintética**, sem chamar essa função do juiz. Cabe corrigir essa descrição na entrega; isso não desfaz a mordida demonstrada. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:177`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:251`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:260`)

**Bloqueia liberação incondicional:** comprovação do pré-requisito operacional ainda **[não verificado]**. **Fila:** C5, gerador e fase; documentação deve acompanhar o marco. **Não procede:** reabrir fase nesta obra, afirmar ausência do juiz em JUIZES.md, ou manter a acusação de catraca sem mordida para os quatro mutantes examinados. (`app/ponto/services/cartorio.py:174`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`, `app/docs/BACKLOG.md:797`, `app/docs/LEIS.md:132`, `app/docs/JUIZES.md:84`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:260`)

## 01/11 02:00 · sessao fcd1a1f10e20 · - · - · origem=Ronald (tmux)

Não localizei novo bloqueio material na cadeia de produção alterada; permanecem duas derivações conhecidas, declaradas como filas: C5 e gerador. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`)

- **Juiz e universo:** o carregador lê todas as FolgaDia da pessoa na janela, preserva todas por data e ordena por pk; o juiz percorre candidatas e aplica um único predicado de origem/vínculo. (`app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:208`, `app/ponto/precedencia.py:216`, `app/ponto/precedencia.py:237`)
- **Sítios migrados:** veto, declaração e comando perguntam ao juiz; `escala_vigente` entrega o vínculo por `vinculo_do_dia`. (`app/ponto/precedencia.py:505`, `app/ponto/services/declarar_dia.py:74`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65`, `app/escala/servico_jornada.py:57`)
- **Grade:** por turno passa o mapa julgado do vínculo do dia; legacy segmenta pelo mesmo juiz e conserva somente os dias do segmento. O overlay escreve apenas o dict em memória. (`app/escala/utils.py:1262`, `app/escala/utils.py:1794`, `app/escala/utils.py:972`, `app/escala/utils.py:995`, `app/escala/utils.py:999`, `app/escala/utils.py:1124`)
- **Calendário:** classificação e `eh_trabalho` publicado leem a mesma função sobre o mesmo dict; não há nova duplicação da regra nessa extração. (`app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:126`, `app/colaboradores/services/calendario.py:839`)
- **Segundo escritor:** não apareceu no trecho alterado. FolgaDia mantém escritor registrado em `escala/folgas.py`; cartório passa a ata pela lavra existente e carimba pela porta, cujo save/evento compartilham `atomic`. (`app/core/portas.py:466`, `app/ponto/services/cartorio.py:1582`, `app/ponto/services/cartorio.py:1602`, `app/ponto/portas/celula.py:639`, `app/ponto/portas/celula.py:807`)
- **Relógio:** o comando captura `hoje` na borda e passa a janela; carregador, juiz, predicado e overlay não fazem nova leitura do relógio. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:52`, `app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:212`, `app/ponto/precedencia.py:222`, `app/escala/utils.py:1103`)
- **Ordem de trava:** o delta fornecido não introduz aquisição de trava; a persistência examinada conserva o `atomic` existente. Ausência de corrida operacional: **[não verificado]**. (`app/ponto/portas/celula.py:639`)
- **C5 continua errado para calendário alheio:** monta pares por cobertura de vínculo, sem consultar origem/juiz, e acusa qualquer célula `trabalha=True` fora de competência exportada. A triagem reconhece corretamente essa classe aberta como GRADE-FOLGA-C5. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1532`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1542`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`)
- **Gerador continua por vínculo:** consome `_datas_folga` do próprio vínculo ou consulta FolgaDia por `escala_colaborador=self`; não vê manual presa a vínculo anterior. Essa dívida está explicitamente marcada GRADE-FOLGA-GERADOR. (`app/escala/models.py:1251`, `app/escala/models.py:1280`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`)

Há resíduo documental pequeno: o predicado ainda diz “o calendário passa `True`”, embora o calendário já tenha retirado essa alimentação. Não é escritor/juiz novo nem bloqueio funcional localizado. (`app/ponto/precedencia.py:229`, `app/colaboradores/services/calendario.py:617`)

## 01/11 02:00 · sessao 320c4aa0025c · - · - · origem=Ronald (tmux)

- **A triagem não pode tratar o pré-requisito de ensaio como dispensado.** O cartório exige `--apply`, dias rejulgados, dias que movem minuto e `FORA-DO-HORIZONTE 0` **antes do pouso**; BACKLOG ainda exige `main + reversão + frota antes do opus`. Cumprimento operacional permanece **[não verificado]**; isso não demonstra que o ensaio faltou, mas impede aprovação incondicional. (`app/ponto/services/cartorio.py:174`, `app/docs/BACKLOG.md:796`)

- **FILA de fase é correta pelo aval explícito.** A cópia continua lendo a foto do vínculo ativo e comparando foto×âncora; o desmembramento está registrado e sua imprecisão declarada. (`app/escala/services/fase_conflitante.py:71`, `app/escala/services/fase_conflitante.py:74`, `app/escala/services/fase_conflitante.py:80`, `app/docs/BACKLOG.md:797`)

- **C5 e gerador permanecem abertos na obra inteira, declaradamente.** C5 considera FolgaDia por cobertura e acusa célula de trabalho sem distinguir origem; gerador lê foto do vínculo. São limites expressos da catraca, não curas concluídas. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1532`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1548`, `app/escala/models.py:1251`, `app/escala/models.py:1280`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:17`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`)

- **FILA dos documentos é defensável como atualização no marco, mas ainda não concluída.** BACKLOG ainda afirma que fase pergunta ao juiz; a linha seguinte supersede isso. L-106 manda docs no commit do marco. A edição alegadamente existente na principal é **[não verificado]**. CORTES registra o corte original, agora limitado pelo aval fornecido pelo usuário. (`app/docs/BACKLOG.md:796`, `app/docs/BACKLOG.md:797`, `app/docs/LEIS.md:132`, `app/docs/CORTES.md:416`)

- **“Juízes novos” está resolvido pelo corpo dos commits fornecido:** uma autoridade na obra, zero adicionais no delta, compatível com registro e corte. O padrão `juiz unico` existe no vocabulário autorizado. (`app/core/juizes.py:188`, `app/core/juizes.py:192`, `app/docs/CORTES.md:416`, `app/docs/GLOSSARIO.md:12`, `app/docs/PLANO-PADROES.md:49`)

- **Não encontrei nova ordem de travas ou segundo escritor no trecho examinado.** O veto novo apenas carrega e julga; a lavra continua pela porta, com save/evento no `atomic` existente. Ausência de corrida em execução é **[não verificado]**. (`app/ponto/precedencia.py:501`, `app/ponto/precedencia.py:505`, `app/ponto/services/cartorio.py:1582`, `app/ponto/services/cartorio.py:1602`, `app/ponto/portas/celula.py:639`, `app/ponto/portas/celula.py:807`)

- **Limite probatório das linhas LEI-AKITA:** os endereços e leitores batem com o desenho, mas nomear logs de RED não demonstra seus resultados nesta verificação. O RED de allowlist referido no primeiro commit não prova sozinho reconhecimento de `getattr` ou percurso de arquivos; essas mordidas exigem a execução dos mutantes apropriados. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:176`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:204`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:245`; resultados dos logs: **[não verificado]**)

## 01/11 02:00 · sessao 58e0cd444abd · - · - · origem=Ronald (tmux)

A descrição nova está **curada quanto ao alcance do horizonte e de `--forcar`**: a versão é a maior cujo horizonte alcança a competência; o cache alimenta a impressão, e `forcar` desativa o pulo por impressão atual. (`app/ponto/services/cartorio.py:196`, `app/ponto/services/cartorio.py:214`, `app/ponto/services/cartorio.py:963`, `app/ponto/services/cartorio.py:1170`, `app/ponto/services/cartorio.py:1180`)

O comando seleciona células passadas da empresa e repassa a flag. “Toda célula” descreve esse universo padrão: `--limite` pode recortar colaboradores, `--apply` controla persistência, e uma célula sem dia correspondente na grade ainda é pulada. Isso é uma ressalva de precisão, sem regressão concreta demonstrada no dado. (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:56`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1183`)

O teste sem `--forcar` **não estabelece explicitamente** carimbo/impressão/veredito antes da chamada. Entretanto, seria errado concluir que a fixture está necessariamente sem carimbo: as batidas criadas acionam o signal, que chama o cartório antes da criação da FolgaDia e da substituição da ata. A premissa depende implicitamente desse caminho. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:62`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:65`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:74`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:121`, `app/ponto/signals.py:85`, `app/ponto/signals.py:131`, `app/ponto/services/cartorio.py:1738`)

Execução Django, efetivo carimbo da fixture e resultado dos testes: **[não verificado]**. [nao verificado]

## 01/11 01:57 · sessao 296ef6213d0c · GRADE-FOLGA-JUIZ · verificador · origem=Code

**Não libero o pouso nesta pré-verificação: a cura da varredura está implementada, mas sua regressão ainda passa com todos os testes da catraca verdes.** Reintroduzir, somente em memória, o descarte de `precedencia.py` inteiro em `achados()` manteve os 11 casos verdes: o `MORDE_segundo_juiz_dentro_de_precedencia` exercita diretamente `achados_da_fonte`, sem testar o percurso dos arquivos. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:122`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:236`)

As referências abaixo são relativas a `app/`. Os testes AST foram exercitados em memória, sem banco, logs ou gravação de arquivos; execução dos testes de integração Django: **[não verificado]**. [nao verificado]

**1. Bloqueios anteriores e mordida dos testes** [nao verificado]

| Item | Situação nesta cópia |
|---|---|
| **(a) `getattr` e cópia literal** | **Curado.** A catraca reconhece `getattr` literal e a cópia do predicado real é exigida pelo teste. Desativar esse reconhecimento deixou vermelhos os testes de cópia literal e leitura sem comparação. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:54`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:204`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:230`) |
| **(a) varrer `precedencia.py` fora do juiz** | **Parcialmente curado.** O código atual exclui somente as três funções autorizadas; porém excluir novamente o arquivo inteiro no percurso mantém os testes verdes. Falta a mordida nesse percurso. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:113`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:129`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:241`) |
| **(c) JUIZES.md** | **Curado.** Registra juiz, carregador, predicado e as duas filas; coincide com o registro vivo. (`docs/JUIZES.md:84`, `core/juizes.py:192`) |
| **Campo “juízes novos”** | O número correto desta obra é **1**, conforme a nova entrada e o corte. A alteração do campo na mensagem de entrega/commit não está disponível para conferência: **[não verificado]**. (`core/juizes.py:192`, `docs/CORTES.md:416`) |
| **Pauta DP** | **Corrigida no texto do cartório:** declara A0×C=0 e retira a conclusão de Pauta DP. O BACKLOG também cancela “comp09=8”, mas conserva textos anteriores sobre Pauta DP. Valores efetivos da comparação: **[não verificado]**. (`ponto/services/cartorio.py:180`, `docs/BACKLOG.md:796`, `docs/BACKLOG.md:795`, `docs/BACKLOG.md:860`) |
| **Integridade FM/TXT** | O teste agora compara a linha inteira de FM e todas as linhas de exportação, além do hash/conteúdo do TXT. Isso corrige a insuficiência da asserção; execução e hashes reais: **[não verificado]**. (`ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:106`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:118`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:119`) |

A mordida da catraca ficou assim:

| `MORDE_` | Resultado da conferência |
|---|---|
| `filtro_pessoa_e_data` | Vermelho ao desativar reconhecimento do filtro. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:189`) |
| `apelido_e_manager_relacionado` | Vermelho removendo apelidos de import ou reconhecimento dos managers relacionados. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:194`) |
| `predicado_da_origem` | Exige detectar o atributo direto; mutação específica executada: **[não verificado]**. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:200`) |
| `copia_literal_do_predicado_do_juiz` | Vermelho sem reconhecimento de `getattr`. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:204`) |
| `copia_literal_do_carregador_do_juiz` | Vermelho desativando reconhecimento do filtro. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:215`) |
| `apelido_de_modulo` | Vermelho retirando reconhecimento de `FolgaDia` no meio da cadeia. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:223`) |
| `leitura_da_origem_sem_comparar` | Vermelho sem reconhecimento de `getattr`. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:230`) |
| `segundo_juiz_dentro_de_precedencia` | Morde exclusão no analisador da fonte; **não morde exclusão no percurso real dos arquivos**. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:236`) |

Não cabe afirmar que cada `MORDE_` de integração ficará vermelho para qualquer reversão: os dois builders já erravam em sentidos diferentes. O próprio teste documenta que calendário alheio mordia o builder por turno, enquanto a manual no vínculo velho mordia o legacy; cada caso precisa ser associado à regressão específica. (`escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:20`)

Os testes adicionais cobram a retirada do portão na grade, inclusive `simular_dia`, e a relavra da ata da 09 com `--forcar`; suas execuções e mutações nesta cópia permanecem **[não verificado]**. (`escala/tests/test_grade_folga_vale_em_toda_competencia.py:74`, `escala/tests/test_grade_folga_vale_em_toda_competencia.py:79`, `escala/tests/test_grade_folga_vale_em_toda_competencia.py:84`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:101`)

**2. Quem escreve e quem lê**

**FolgaDia e seus campos**

| Valor | Escritores e leitores localizados |
|---|---|
| Estrutura | `escala_colaborador` tem CASCADE; `data` identifica o dia; `calendario_origem` é nullable/SET_NULL. Unicidade é vínculo+data, não pessoa+data. (`escala/models.py:1553`, `escala/models.py:1556`, `escala/models.py:1557`, `escala/models.py:1571`) |
| Foto de calendário | `gerar_foto_folgas` lê manuais, apaga a foto de calendário selecionada e cria vínculo/data/origem. (`escala/folgas.py:165`, `escala/folgas.py:169`, `escala/folgas.py:173`) |
| Foto manual | `lancar_folga` consulta origem, passa pelo veto e cria por vínculo/data; `_foto_do_dia` consulta, apaga/cria ou altera apenas o tipo informativo. (`escala/folgas.py:254`, `escala/folgas.py:258`, `escala/folgas.py:259`, `escala/folgas.py:274`, `escala/folgas.py:280`, `escala/folgas.py:284`, `escala/folgas.py:289`) |
| Reparentamento | Lê origem, vínculo, data e ocupação; altera `escala_colaborador` pela porta existente. O comando delega à porta. (`escala/folgas.py:405`, `escala/folgas.py:418`, `escala/folgas.py:435`, `escala/folgas.py:453`, `escala/management/commands/reparentar_folgas.py:28`) |
| Universo da pessoa | O carregador consulta todos os vínculos da pessoa na janela e preserva todas as candidatas por data; o predicado lê origem e identidade do vínculo. (`ponto/precedencia.py:206`, `ponto/precedencia.py:208`, `ponto/precedencia.py:237`, `ponto/precedencia.py:239`) |
| Foto por vínculo/gerador | `folgas_do_periodo` carrega vínculo/data; `eh_dia_trabalho_calculado` consome essa foto. (`escala/alimentacao.py:60`, `escala/alimentacao.py:66`, `escala/models.py:1251`, `escala/models.py:1280`, `escala/models.py:1327`) |
| Cadastro e diagnóstico | Defeito mensal, divergência calendário×foto, Plano de Folgas, toggle, diagnóstico do vínculo e limbo leem a foto por vínculo. (`escala/models.py:1220`, `escala/folgas.py:101`, `escala/views_plano_folgas.py:60`, `escala/views_plano_folgas.py:112`, `ponto/services/diagnostico_escala.py:102`, `relatorios/furos.py:112`) |
| Vigias | Fase lê a foto do vínculo ativo; C5 itera toda FolgaDia e confronta cobertura/célula. (`escala/services/fase_conflitante.py:74`, `chamados/management/commands/auditar_invariantes_chamados.py:1532`) |

Não localizei novo escritor direto de FolgaDia fora da porta; o registro autoriza `escala/folgas.py`. CASCADE/SET_NULL continuam sendo efeitos dos relacionamentos existentes. (`core/portas.py:466`, `escala/models.py:1554`, `escala/models.py:1558`)

**Vínculo entregue ao juiz em cada sítio**

| Sítio | Feed |
|---|---|
| Precedência | `vinculo_do_dia`, com células e vínculos; completa geradoras faltantes antes da escolha. (`ponto/precedencia.py:326`, `ponto/precedencia.py:350`, `ponto/precedencia.py:357`) |
| Veto de lançamento | `escala_vigente(colab,d)`. (`ponto/precedencia.py:505`) |
| Declaração | `_ec_do_dia` → `escala_vigente`. (`ponto/services/declarar_dia.py:56`, `ponto/services/declarar_dia.py:74`) |
| Rejulgar ausência em folga | `escala_vigente(cid,d)`. (`ponto/management/commands/rejulgar_ausencia_em_folga.py:65`) |
| Grade por turno | Mapa produzido por `vinculo_do_dia`; overlay recebe `esc_por_dia.get`. (`escala/utils.py:1262`, `escala/utils.py:1268`, `escala/utils.py:1794`) |
| Grade legacy | Segmenta pelo juiz do vínculo; passa o vínculo do segmento ao builder e retém apenas seus dias. O `lambda _: esc` responde nesse recorte. (`escala/utils.py:972`, `escala/utils.py:995`, `escala/utils.py:999`, `escala/utils.py:770`) |
| Calendário | Chama `fatos_do_periodo` sem a antiga substituição `{d: True}`; usa o carregador da pessoa. (`colaboradores/services/calendario.py:617`, `ponto/precedencia.py:281`) |

`escala_vigente` carrega a célula e delega a `vinculo_do_dia`; a validade da geradora fica na autoridade existente. (`escala/servico_jornada.py:46`, `escala/servico_jornada.py:57`, `escala/alimentacao.py:237`)

**Tipo da grade, ata e célula**

O `tipo_dia` nasce nos builders; o overlay curado só escreve no dict em memória, depois da resposta do juiz. Ausências ainda podem substituir o tipo por `falta`/`ausencia`. (`escala/utils.py:727`, `escala/utils.py:1751`, `escala/utils.py:1124`, `escala/utils.py:1126`, `escala/utils.py:1182`, `escala/utils.py:1188`)

O cartório lê a grade, monta `ata.tipo_dia`, entrega a ata à lavra existente e carimba. A porta grava ata/veredito/status e salva junto com os carimbos; save e evento compartilham `atomic`. A precedência entra como chave da mesma ata pela sua operação existente. (`ponto/services/cartorio.py:947`, `ponto/services/cartorio.py:583`, `ponto/services/cartorio.py:1580`, `ponto/services/cartorio.py:1600`, `ponto/portas/celula.py:768`, `ponto/portas/celula.py:807`, `ponto/portas/celula.py:639`, `ponto/portas/celula.py:819`)

Leitores externos ao diff localizados:

| Origem | Leitores |
|---|---|
| Ata/célula | Juiz de cobrança, leitor da célula e tripwire. (`chamados/juizes.py:408`, `escala/services/leitor_celula.py:345`, `ponto/management/commands/tripwire_celulas.py:342`) |
| Grade — cobrança/ação | Materialização, declaração textual, emissão de disputa, furos diários, supra-juiz, worklist, propositor e veredito humano. (`chamados/services/materializacao.py:225`, `chamados/services/declaracao_texto.py:90`, `chamados/services/disputa_emissao.py:68`, `escala/services/furos_diarios.py:39`, `ponto/supra_juiz.py:222`, `ponto/worklist.py:19`, `ponto/propositor.py:210`, `ponto/services/veredito_celula.py:221`) |
| Grade — apuração/tela | Status, espelho, simulação, bordas, órfãs, furos e calculadores. (`ponto/status_dia.py:146`, `ponto/services/espelho.py:690`, `ponto/services/simular_dia.py:12`, `ponto/services/bordas_realizado.py:74`, `escala/orfa_classe.py:65`, `relatorios/furos.py:147`, `inteligencia/calculadores.py:141`) |
| Financeiros | Dia pago, fechamento, causas e exportação. (`ponto/services/dia_pago.py:386`, `ponto/services/fechamento.py:1011`, `folha/causas.py:100`, `folha/export.py:1071`) |
| Instrumentos | Autópsia e mapa de divergência. (`ponto/management/commands/e6_piso_autopsia.py:13`, `ponto/management/commands/mapa_divergencia.py:131`) |

A completude contra acessos dinâmicos não identificáveis pelo censo textual é **[não verificado]**.

**REGRA_VERSOES:** a declaração escreve a tupla; `versao_para` lê seus horizontes pela competência; `_regra_do_dia` alimenta os dois ramos do cartório; `impressao_insumos` incorpora a versão ao hash. (`ponto/services/cartorio.py:111`, `ponto/services/cartorio.py:194`, `ponto/services/cartorio.py:210`, `ponto/services/cartorio.py:963`, `ponto/services/cartorio.py:1015`, `ponto/services/cartorio.py:1168`, `ponto/services/cartorio.py:368`)

**3. ALLOWLIST e sítios fora dela**

Não encontrei entrada que oculte outro juiz pessoa-dia sob motivo falso: são escritores, fotos por vínculo, perguntas mensais ou escolha de frase sobre resultado já julgado. O gerador é uma exceção relevante, mas está explicitamente marcado como **INCOMPLETO / GRADE-FOLGA-GERADOR**. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:137`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:158`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:165`)

A entrada de fase é coerente: o arquivo lê a foto do vínculo ativo e compara foto×âncora; a entrada declara sua fila própria. `test_os_sitios_curados_nao_voltam` cobra comando, declaração e ausência de `_folga_avulsa`, sem exigir cura de fase. (`escala/services/fase_conflitante.py:74`, `escala/services/fase_conflitante.py:80`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:142`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:182`)

**Sim, há leitor fora do juiz e fora da ALLOWLIST:** C5 percorre FolgaDia, usa cobertura por vínculo e acusa célula de trabalho sem distinguir origem. É a fila nominal GRADE-FOLGA-C5, expressamente declarada como limite da catraca. (`chamados/management/commands/auditar_invariantes_chamados.py:1532`, `chamados/management/commands/auditar_invariantes_chamados.py:1535`, `chamados/management/commands/auditar_invariantes_chamados.py:1548`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`)

**4. Resíduos da retirada de fase**

Nos documentos nominalmente pedidos, não achei afirmação residual de migração: JUIZES declara foto/fila; MAPA-JUIZES, MAPA e `core/juizes.py` registram o juiz; ARQUITETURA mantém o cron de fase. O docstring dos testes da obra declara a retirada, e os testes próprios de fase continuam verificando foto×âncora. (`docs/JUIZES.md:89`, `docs/MAPA-JUIZES.md:52`, `docs/MAPA.md:107`, `core/juizes.py:192`, `docs/ARQUITETURA.mmd:62`, `ponto/tests/test_grade_folga_juiz_sitios.py:13`, `escala/tests/test_fase_conflitante.py:60`)

**Sobrou texto fora desse conjunto:** BACKLOG ainda afirma que `fase_conflitante:74` pergunta ao juiz; CORTES conserva a enumeração original. A linha seguinte do BACKLOG registra o desmembramento. Cabe atualizar o registro vivo e explicitar a supersessão, sem trazer a cura de fase de volta à obra. (`docs/BACKLOG.md:796`, `docs/BACKLOG.md:797`, `docs/CORTES.md:416`)

**5. Classes de reprovação**

| Classe | Veredito |
|---|---|
| Segundo juiz | Os sítios curados delegam ao juiz; C5 e gerador permanecem filas declaradas. (`ponto/precedencia.py:505`, `ponto/services/declarar_dia.py:74`, `ponto/management/commands/rejulgar_ausencia_em_folga.py:65`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`) |
| Segundo escritor | Não aparece novo escritor persistente fora da porta; overlay altera memória e cartório usa a lavra existente. (`escala/utils.py:1126`, `ponto/services/cartorio.py:1580`, `ponto/portas/celula.py:807`) |
| Relógio fora da borda | O trecho novo do comando lê a data na borda; carregador, predicado e overlay novos não consultam relógio. (`ponto/management/commands/rejulgar_ausencia_em_folga.py:52`, `ponto/precedencia.py:194`, `ponto/precedencia.py:222`, `escala/utils.py:1103`) |
| Derivação paralela | A grade passa a chamar o juiz; calendário compartilha `trabalho_do_dia` entre classificação e publicação. (`escala/utils.py:1124`, `colaboradores/services/calendario.py:126`, `colaboradores/services/calendario.py:840`) |
| Selo sem mordida | **Procedente e bloqueante:** exclusão integral de `precedencia.py` no percurso passa verde. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:129`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:236`) |
| Ordem de trava | Não há nova aquisição de trava no trecho alterado; a persistência continua pelo `atomic` existente. Ausência de corrida em execução: **[não verificado]**. (`ponto/services/cartorio.py:111`, `ponto/portas/celula.py:639`) |
| Pouso antes do pré-requisito | O próprio cartório exige ensaio `--apply` antes do pouso. Ensaio, reversão e resultado específico de 303970f1: **[não verificado]**. (`ponto/services/cartorio.py:174`) |

**6. Exportada, escala/vínculo, motor e alcance do rejulgamento**

O diff fornecido não introduz escritor de escala/vínculo nem alteração em `motor_calculo_v2.py`; a cura da grade consulta FolgaDia e modifica o resultado em memória. A cadeia examinada do cartório grava célula/ata, carimbos, veredito/status e evento pela porta existente. (`escala/utils.py:1115`, `escala/utils.py:1124`, `escala/utils.py:1126`, `ponto/services/cartorio.py:1580`, `ponto/services/cartorio.py:1600`, `ponto/portas/celula.py:640`)

**Ata/célula da exportada pode mudar, pelo desenho autorizado.** Não há nova escrita direta de FechamentoMensal/TXT nesse caminho; os testes cobram sua integridade completa. A confirmação operacional dessa integridade é **[não verificado]**. (`docs/ADR/0003-celula-tres-naturezas.md:25`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:118`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:119`)

A v3 muda a versão da competência **10 em diante**; 09 permanece na v1 e 08 sem versão. Isso altera a impressão e provoca nova análise das células elegíveis; não constitui portão de escrita. (`ponto/services/cartorio.py:111`, `ponto/services/cartorio.py:212`, `ponto/services/cartorio.py:1168`, `ponto/tests/test_o219_versao_da_regra.py:85`)

Com `--forcar`, o comando ignora o pulo por carimbo atual e seleciona **todas as células passadas da empresa**, inclusive abaixo do horizonte, sem limitar à 09. Sem `--forcar`, a 09 não muda de impressão **por esta v3**, mas células sem carimbo ou com outros insumos alterados continuam elegíveis. A frase “a 09 só se rejulga com --forcar” é ampla demais e deve ser corrigida. (`ponto/management/commands/processar_cartorio.py:49`, `ponto/management/commands/processar_cartorio.py:61`, `ponto/services/cartorio.py:1174`, `ponto/services/cartorio.py:1178`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:122`)

**Bloqueia:** prender com teste a varredura real de `precedencia.py`. **Corrigir junto:** resíduos do BACKLOG e a descrição absoluta de `--forcar`. **Fila:** fase, C5 e gerador. **Não procede:** exigir cura de fase nesta obra, tratar relavra autorizada da ata da 09 como escrita proibida do gravado, ou afirmar que falta o juiz em JUIZES.md. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:129`, `docs/BACKLOG.md:796`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:122`, `docs/BACKLOG.md:797`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`, `docs/ADR/0003-celula-tres-naturezas.md:25`, `docs/JUIZES.md:84`)

## 01/11 01:55 · sessao da6e67fc3dfd · - · - · origem=Ronald (tmux)

**Cadeia de escrita/leitura**

- `tipo_dia` nasce nos builders: single vínculo em `app/escala/utils.py:727` e `:752`; por turno em `:1751` e `:1757`; legacy recorta segmentos pelo `vinculo_do_dia` em `:972`, `:995` e `:999`, transportando o campo em `:1018`. O overlay curado escreve apenas o dict em memória (`:1126`), após consultar o juiz (`:1124`); ausências podem sobrescrever para `falta`/`ausencia` em `:1182`/`:1188`.
- O cartório lê a grade em `app/ponto/services/cartorio.py:947`, transporta `tipo_dia` para a ata em `:583`, entrega a ata a `CelulaDia.lavrar_veredito` em `:1059` e `:1580`, e carimba pelo escritor existente em `:1600`.
- O método do model só delega: `app/escala/models.py:1787`. A porta escreve `cel.ata` em `app/ponto/portas/celula.py:741`/`:768`; `carimbar` salva campos e lavra juntos em `:787`/`:807`; save e outbox compartilham atomic em `:639`. A precedência tem chave própria na mesma ata: preservação em `:732`; escritor `carimbar_precedencia` em `:811`/`:819`, chamado por `app/ponto/services/cartorio.py:1695`.
- Nenhuma nova ordem de locks foi introduzida pelo diff fornecido; o trecho novo do cartório é declaração de versão e documentação (`app/ponto/services/cartorio.py:111`, `:168`). Prova operacional de ausência de corrida: **[não verificado]**.

**Leitores externos ao diff**

- Ata diretamente: juiz de cobrança `app/chamados/juizes.py:408`; leitor da célula `app/escala/services/leitor_celula.py:332`/`:345`, que publica o campo em `:360`; vigia `app/ponto/management/commands/tripwire_celulas.py:342`.
- Grade/dict de dia: `app/escala/orfa_classe.py:65`; `app/relatorios/furos.py:147`/`:151`/`:218`/`:237`; `app/chamados/services/materializacao.py:225`; `app/chamados/services/declaracao_texto.py:90`; `app/chamados/services/disputa_emissao.py:68`; `app/escala/services/furos_diarios.py:39`; `app/ponto/status_dia.py:146`; `app/ponto/worklist.py:19`; `app/ponto/propositor.py:36`/`:210`; `app/inteligencia/calculadores.py:46`/`:141`; `app/ponto/services/veredito_celula.py:221`; `app/ponto/services/bordas_realizado.py:74`; `app/ponto/services/simular_dia.py:12`; `app/ponto/services/espelho.py:690`; `app/ponto/supra_juiz.py:222`; `app/folha/causas.py:40`/`:41`/`:100`.
- Leitores financeiros continuam existindo: `app/ponto/services/dia_pago.py:386`/`:393`/`:398`; `app/ponto/services/fechamento.py:1011`/`:1015`; `app/folha/export.py:1071`/`:1075`/`:1080`. Logo, alterar `tipo_dia` pode alterar resultados de uma futura projeção financeira; não equivale a escrever o gravado pelo cartório (`app/ponto/services/fechamento.py:1112`).
- Instrumentos adicionais leem o campo em `app/ponto/management/commands/e6_piso_autopsia.py:13` e `app/ponto/management/commands/mapa_divergencia.py:131`/`:143`/`:155`/`:229`. Ocorrências em templates não apareceram no censo textual; completude contra acesso dinâmico: **[não verificado]**.

**REGRA_VERSOES e horizonte**

- Escritor único é a declaração `app/ponto/services/cartorio.py:111`; validação em `:183`; leitor `versao_para` em `:194`/`:212`, usando a competência de `janela_atual` (`:210`).
- A versão integra a impressão em `:368`; `_regra_do_dia` fornece-a aos dois ramos (`:963`, `:1015`, `:1168`). A v3 alcança competência 10 em diante; 09 conserva v1 e 08 fica sem versão, conforme asserções de `app/ponto/tests/test_o219_versao_da_regra.py:85`.
- `--forcar` ignora o pulo por impressão igual (`app/ponto/services/cartorio.py:1020`, `:1178`). O comando seleciona **todas as células passadas da empresa**, sem recorte de competência (`app/ponto/management/commands/processar_cartorio.py:49`, `:61`, `:65`). Portanto, com `--forcar`, abaixo do horizonte não significa só 09: qualquer célula passada selecionada pode ser rejulgada.
- Sem `--forcar`, a 09 não recebe mudança de impressão **por esta v3**, mas células sem carimbo ou com outros insumos alterados continuam elegíveis (`app/ponto/services/cartorio.py:1174`). A frase absoluta “a 09 só se rejulga com --forcar” do teste é mais ampla que o código (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:122`).
- Não confirmei falha desse teste: as batidas da fixture (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:62`) disparam julgamento por signal (`app/ponto/signals.py:131`/`:135`, `:85`/`:93`), podendo inicializar o carimbo antes do update da ata (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:74`). Execução do teste e mutante: **[não verificado]**.

**Exportada, prerequisitos e “corrigir junto”**

- O caminho examinado reescreve célula/ata, carimbos, status, protestos e emite outbox; não chama o escritor de `FechamentoMensal` ou a porta REGEN (`app/ponto/services/cartorio.py:1578`, `:1600`; `app/ponto/portas/celula.py:787`). A fixture cobra linha inteira de FM e todas as exportações intactas (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:106`, `:118`, `:119`). Verde real e hashes de produção: **[não verificado]**.
- O diff fornecido não altera motor nem escritores de vínculo; a chamada nova da grade consulta o juiz e muda memória (`app/escala/utils.py:1115`, `:1124`, `:1126`). Identidade binária com main, sem repositório Git nesta cópia: **[não verificado]**.
- Ensaio `--apply` na sombra, reversão e impacto **antes do pouso** são prerequisito explícito da própria declaração (`app/ponto/services/cartorio.py:174`/`:175`). Os anexos narram evidência antiga; ensaio da cópia 303970f1, números e reversão: **[não verificado]**.
- Pauta DP foi corrigida no texto vivo para A0×C=0 (`app/ponto/services/cartorio.py:180`/`:181`); o valor real e hashes TXT não são demonstráveis pelos arquivos examinados: **[não verificado]**.
- Registro efetivamente conta juiz novo (`app/core/juizes.py:192`) e há corte (`app/docs/CORTES.md:416`). O campo LEI-AKITA da mensagem de entrega/commit não está acessível; sua correção é **[não verificado]**.
- Resíduos vivos fora da lista nominal pedida: `app/docs/BACKLOG.md:796` e o corte histórico `app/docs/CORTES.md:416` ainda dizem que `fase_conflitante` pergunta ao juiz. A retirada decidida pelo usuário prevalece, mas o BACKLOG precisa acompanhar o novo escopo (`app/ponto/tests/test_grade_folga_juiz_sitios.py:13`).

## 01/11 01:55 · sessao 201524adb215 · - · - · origem=Ronald (tmux)

Catraca implementada, porém **a exclusão do arquivo inteiro `precedencia.py` ainda pode voltar sem teste vermelho**. `test_MORDE_segundo_juiz_dentro_de_precedencia` chama diretamente `achados_da_fonte`; não exerce o percurso `achados`. Em mutação somente em memória, inserir `if rel == 'ponto/precedencia.py': continue` nesse percurso deixou os **11 testes verdes**. Referências: `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:122`, `:130`, `:236`, `:241`. Esse é um bloqueio de mordida, embora o código atual efetivamente varra o arquivo (`:113`, `:115`, `:131`).

Verificação em memória, com AST e `unittest.TestCase` substituindo somente `SimpleTestCase`, sem importar Django, banco ou logs:

| MORDE | Referência | Mutação que o deixou vermelho |
|---|---|---|
| filtro pessoa/data | catraca:189 | Desativar reconhecimento de filtro pessoa/data |
| apelido e manager | catraca:194 | Remover reconhecimento de import com apelido; ou esvaziar `REL` |
| predicado da origem | catraca:200 | A asserção exige detectar `fd.calendario_origem_id`; regressão específica não executada [não verificado] |
| cópia literal do predicado | catraca:204 | Desativar `_getattr_literal` |
| cópia literal do carregador | catraca:215 | Desativar filtro pessoa/data |
| apelido de módulo | catraca:223 | Remover reconhecimento do modelo no meio da cadeia |
| origem sem comparação | catraca:230 | Desativar `_getattr_literal` |
| segundo juiz dentro de precedência | catraca:236 | Excluir precedência de `achados_da_fonte`; **não** excluir do percurso `achados` |

As funções e asserções estão em `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:54`, `:62`, `:95`, `:105`, `:189`, `:194`, `:200`, `:204`, `:215`, `:223`, `:230`, `:236`. Os testes atuais passaram e `achados == ALLOWLIST == 22`; resultado da execução em memória [não verificado por artefato persistido].

A ALLOWLIST está coerente com a retirada de fase: `medir` lê a foto do vínculo ativo, compara foto e âncora e conta divergências; nenhuma chamada ao juiz aparece nesse fluxo. A entrada declara expressamente sua fila própria. `test_os_sitios_curados_nao_voltam` prende apenas comando, declaração e lápide `_folga_avulsa`, sem exigir cura de fase. Referências: `app/escala/services/fase_conflitante.py:71`, `:74`, `:80`, `:83`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:142`, `:182`.

Nas entradas inspecionadas, não achei outro juiz pessoa-dia oculto: há fotos por vínculo, contadores mensais e escritores. O gerador continua decidindo o dia pela foto do próprio vínculo, mas isso está explicitamente marcado como incompleto/fila. Referências: `app/escala/alimentacao.py:49`, `:66`; `app/escala/models.py:1212`, `:1220`, `:1280`; `app/escala/views_plano_folgas.py:60`, `:63`, `:70`; `app/ponto/services/diagnostico_escala.py:81`, `:102`; `app/relatorios/furos.py:112`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`.

C5 permanece fora do juiz e fora da ALLOWLIST: itera toda FolgaDia, verifica cobertura por algum vínculo e acusa célula de trabalho sem distinguir origem. O limite está declarado na catraca como GRADE-FOLGA-C5. Referências: `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`, `:1534`, `:1535`, `:1540`, `:1542`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:19`.

Os documentos expressamente pedidos estão coerentes: JUIZES registra fase lendo foto em fila; MAPA-JUIZES/MAPA/core registram o juiz sem atribuir migração a fase; ARQUITETURA só mantém os nós do cron. Referências: `app/docs/JUIZES.md:84`, `:88`, `:90`; `app/docs/MAPA-JUIZES.md:52`; `app/docs/MAPA.md:107`; `app/core/juizes.py:188`, `:192`; `app/docs/ARQUITETURA.mmd:62`, `:112`.

**Sobrou resíduo fora desse conjunto:** BACKLOG ainda afirma `fase_conflitante:74` perguntando ao juiz; CORTES também enumera fase entre os migrados. A linha seguinte do BACKLOG registra o desmembramento, tornando necessário esclarecer o texto anterior. Referências: `app/docs/BACKLOG.md:796`, `:797`; `app/docs/CORTES.md:416`. O docstring dos testes da obra já declara corretamente a retirada; os testes antigos de fase verificam foto/âncora, sem depender dessa cura. Referências: `app/ponto/tests/test_grade_folga_juiz_sitios.py:13`; `app/escala/tests/test_fase_conflitante.py:60`, `:72`, `:93`.

## 01/11 01:54 · sessao 479951fb7264 · - · - · origem=Ronald (tmux)

Censo de `FolgaDia` e feeds do juiz:

| Valor / função | Quem escreve / quem lê |
|---|---|
| Modelo | `escala_colaborador` FK CASCADE, `data`, `calendario_origem` FK SET_NULL/nulo: `app/escala/models.py:1553-1559`; unicidade vínculo+data: `:1571`. |
| Foto do calendário | `gerar_foto_folgas`: lê manuais `app/escala/folgas.py:165-168`, apaga calendário `:169-172`, cria vínculo/data/origem `:173-175`. |
| Foto manual | `lancar_folga`: consulta/origem `app/escala/folgas.py:254-255`, veto `:258`, get_or_create vínculo/data `:259-260`; `_foto_do_dia`: consulta/origem `:274-275`, delete `:280`, create `:284`, mudança só de tipo `:288-289`. |
| Reparentamento | `app/escala/folgas.py:405-418` lê origem/vínculo/data/ocupação; único update de vínculo `:435-436`. Seleção em lote `:453-457`; command `app/escala/management/commands/reparentar_folgas.py:28-34` delega à porta. |
| Carregador da pessoa | `app/ponto/precedencia.py:206-208` lê vínculo pela pessoa e janela, preserva todas por data; juiz `:212-218`; único predicado origem/vínculo `:235-239`. |
| Foto por vínculo | `app/escala/alimentacao.py:66-70` lê vínculo/data, arredondando ao mês `:60-64`; consumidor gerador `app/escala/models.py:1251-1259`, `:1280-1282`, `:1327-1334`. |
| Defeito/cadastro | `app/escala/models.py:1220-1221`; divergência calendário/foto/origem `app/escala/folgas.py:101-104`; Plano de Folgas `app/escala/views_plano_folgas.py:60-76`; toggle lê origem para recusa e delega escrita `:112-137`; diagnóstico `app/ponto/services/diagnostico_escala.py:102-104`; limbo `app/relatorios/furos.py:112-115`. |
| Vigia fase | `app/escala/services/fase_conflitante.py:71-80` lê foto do vínculo ativo, compara `_foto_diz` e `_ancora_diz(ec,d)`; não chama o juiz. |
| Leitor fora juiz/ALLOWLIST | C5 em `app/chamados/management/commands/auditar_invariantes_chamados.py:1532-1536` itera toda FolgaDia, aceita cobertura por outro vínculo sem distinguir origem; acusa célula de trabalho `:1540-1550`. É segundo derivador já declarado como fila na catraca `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:21-24`. |

Não apareceu escritor direto novo fora de `escala/folgas.py` na busca dos símbolos FolgaDia/managers; o registro da porta permite só esse arquivo: `app/core/portas.py:466`. Exclusão de vínculo/calendário também pode alterar FolgaDia por CASCADE/SET_NULL do modelo, sem mudança desta obra: `app/escala/models.py:1553-1559`.

Feeds entregues ao juiz:

- Precedência: `vinculo_do_dia` em `app/ponto/precedencia.py:350`, aplicado `:357`; completa geradora ausente da lista `:326-330`.
- Veto: `escala_vigente(colab,d)` em `app/ponto/precedencia.py:505`.
- Declaração: `_ec_do_dia` → `escala_vigente` em `app/ponto/services/declarar_dia.py:56-58`; juiz `:74-75`; origem restante escolhe frase sobre folga já aceita `:112-118`.
- Command: `escala_vigente(cid,d)` em `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:64-65`.
- `escala_vigente` carrega célula e consulta o juiz do vínculo em `app/escala/servico_jornada.py:46-58`; validade da geradora e fallback ficam em `app/escala/alimentacao.py:237-254`.
- Grade por turno: `vinculo_do_dia` → mapa em `app/escala/utils.py:1262-1269`, overlay `:1794`.
- Grade legacy: segmenta pelo juiz `app/escala/utils.py:972-982`; chama builder com `e` `:995`; builder passa `lambda _: esc` `:770`; só retém dias do segmento escolhido `:997-1001`. Busca de chamadores de produção de `montar_grade_prevista` achou só esse legacy `:995`.
- Calendário usa `fatos_do_periodo` sem a antiga foto `{d: True}` em `app/colaboradores/services/calendario.py:617-619`; portanto recebe o mesmo juiz e carregador da pessoa `app/ponto/precedencia.py:281`.

As entradas da ALLOWLIST para foto, cadastro e escritor correspondem ao que os sítios fazem nas linhas acima; gerador é a dívida nominal `GRADE-FOLGA-GERADOR`, enquanto C5 permanece fora da cobertura e nominalmente declarado na própria catraca: `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:174-175`, `:21-24`.

## 01/11 01:42 · sessao 0c6764fddfce · CONTENCAO-QUE-MORDE-V2 · verificador · origem=Code

**Parecer: bloqueia o P1.** Há autorizações sem registro persistido, corridas entre consulta e reserva, semeadura incompleta e certificado que pode liberar sem comprovar a suite (`bin/disjuntor.py:318`, `bin/disjuntor.py:1142`, `bin/disjuntor.py:1372`, `bin/disjuntor.py:1264`). Execução integral dos selos, instalação efetiva dos hooks e ocorrência desses caminhos em produção: **[nao verificado]**.

1. **IDENTIDADE/RESERVA — bloqueia: autoriza mesmo se não conseguir gravar.** `_com_estado` executa a decisão, captura a falha de persistência e retorna o resultado original. As duas portas novas não conferem `gravou`: Agent pode abrir sem reservar vaga/volta; wrapper pode abrir sem consumir volta. A proteção já existente no B3 confere a gravação, mas não foi aplicada às novas autorizações (`bin/disjuntor.py:305`, `bin/disjuntor.py:318`, `bin/disjuntor.py:323`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:1397`, `bin/disjuntor.py:1048`).

2. **FALHA RESTRITIVA — bloqueia: exceção anterior à porta libera Agent não final.** Um estado ilegível pode levantar na leitura de `pretool`, antes de `porta_agent`. O tratamento externo nega somente o ramo B3; para um construtor não final, termina liberando sem verificar modelo, papel, vivos ou voltas (`bin/disjuntor.py:1965`, `bin/disjuntor.py:1992`, `bin/disjuntor.py:1995`, `bin/disjuntor.py:2005`, `bin/disjuntor.py:2015`).

3. **IDENTIDADE/ORDEM DE TRAVA — bloqueia: B3 e autorização não são uma transição.** Agent consulta B3 numa transação e reserva noutra; o wrapper faz o mesmo. Entre ambas, outro processo pode parar a obra, e a reserva posterior não revalida essa parada. Usar o mesmo arquivo de flock em chamadas separadas não fecha essa corrida (`bin/disjuntor.py:1961`, `bin/disjuntor.py:1465`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:1132`, `bin/disjuntor.py:1142`, `bin/disjuntor.py:1387`).

4. **(11) VIVOS — bloqueia: fotografia fora do flock permite subcontagem.** `txt` e `vivos_tx` são lidos antes da trava. Um processo pode fotografar cinco vivos; outro autorizar o sexto; uma transação seguinte podar a reserva desse sexto após seu lançamento. Quando o primeiro processo entra, sua fotografia ainda tem cinco e a reserva já desapareceu: pode autorizar o sétimo (`bin/disjuntor.py:1457`, `bin/disjuntor.py:1461`, `bin/disjuntor.py:1473`, `bin/disjuntor.py:1475`, `bin/disjuntor.py:1476`).

5. **(11) RESERVAS — bloqueia: poda por substring, sem reconhecer o resultado.** Qualquer objeto que contenha a sequência compacta `"tool_use_id":"<id>"` satisfaz a poda, sem exigir `type=tool_result`. Um resultado JSON válido com espaço após `:` não satisfaz a mesma busca. O leitor de lançamentos recupera apenas agentes assíncronos; síncronos anteriores, sem reserva registrada, não entram nessa recuperação (`bin/disjuntor.py:1473`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:403`, `bin/disjuntor.py:413`).

6. **IDENTIDADE — bloqueia: idempotência acaba quando a reserva é podada.** Agent consulta somente `reservas`; acrescenta `tool_use_id` em `voltas.chaves`, mas não consulta essas chaves. Após a poda, repetir a mesma autorização incrementa novamente. A sequência isolada `a, b, a`, com resultado de `a` antes de `b`, termina com três voltas e chaves `[a,b,a]` (`bin/disjuntor.py:1467`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:1484`, `bin/disjuntor.py:1485`).

7. **(1) SEMENTE — bloqueia: inicialização pelo wrapper congela Agent em zero.** Wrapper e `reabre` chamam `_voltas` sem transcript. A semente retorna `agent=0`, parcial; quando um Agent chega depois, o dicionário existente nunca é completado. Assim, seis voltas anteriores da sessão podem ser ignoradas (`bin/disjuntor.py:1388`, `bin/disjuntor.py:1494`, `bin/disjuntor.py:1346`, `bin/disjuntor.py:1357`, `bin/disjuntor.py:1372`).

   **Também bloqueia:** sidecar ilegível é simplesmente pulado, sem marcar semente parcial. A busca cobre somente `logs/`, embora o wrapper aceite resposta em caminho fornecido pelo chamador. Estado ausente vira `{}`, perdendo reservas e identidades que a semeadura não reconstrói (`bin/disjuntor.py:831`, `bin/disjuntor.py:1348`, `bin/disjuntor.py:1350`, `bin/codex_pergunta.sh:40`, `bin/disjuntor.py:299`).

8. **CHAVE / “só Ronald” — bloqueia: guarda apenas lexical.** A condição normal arquivo **e** linha está implementada, RELATO ilegível mantém ligado, e o wrapper consulta a chave. Porém, a exclusividade depende de encontrar nomes/literais no comando: importar `disjuntor` e chamar `reabre(...)` ou `redesenha(...)` passa sem os argumentos `--reabre`/`--redesenha`. Write direto no estado também não é guardado. As funções não verificam autoridade (`bin/disjuntor.py:176`, `bin/disjuntor.py:169`, `bin/disjuntor.py:1130`, `bin/disjuntor.py:190`, `bin/disjuntor.py:194`, `bin/disjuntor.py:201`, `bin/disjuntor.py:1491`, `bin/disjuntor.py:1502`).

9. **(2) SEGUNDO ERRO-BOBO — bloqueia: `--rearma-b3` contorna a exigência de `--redesenha`.** O rearme limpa `disparado` e arquiva as reprovações, preservando `pede_desenho`. Na decisão seguinte, a obra não disparada é ignorada **antes** de consultar esse pedido; volta a LIVRE sem `--redesenha` (`bin/disjuntor.py:1106`, `bin/disjuntor.py:1108`, `bin/disjuntor.py:1002`, `bin/disjuntor.py:1010`).

   **Fila — formato diferente do contratado:** `consumidas` continua sendo lista de SHA, com `consumidas_tipo` em mapa separado. A regra normal lê ambos, mas não grava `{sha,tipo}` como escrito no REVISADO (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:12`, `bin/disjuntor.py:1023`, `bin/disjuntor.py:1027`, `bin/disjuntor.py:1162`).

10. **(3) CURA — bloqueia: HEAD ausente não nega incondicionalmente.** Falha ao obter HEAD vira `None`, mas sua ausência só é verificada dentro do laço de PROCEDENTES. Com triagem vazia, passa, apesar da ressalva contratual “cópia sem hash = nega” (`bin/disjuntor.py:1448`, `bin/disjuntor.py:1451`, `bin/disjuntor.py:1276`, `bin/disjuntor.py:1280`, `bin/disjuntor.py:1288`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:13`).

   **Fila — alcance mais restritivo:** o gate de cura também atinge verificadores finais sonnet/haiku; não verifica modelo, enquanto o item (3) diz “opus final” (`bin/disjuntor.py:1427`, `bin/disjuntor.py:1469`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:13`).

11. **(4) UNIVERSO — bloqueia: porta e coletor reconhecem universos diferentes.** A porta aceita `general-purpose` com papel verificador na descrição. O coletor de reprovações continua exigindo `meta.agentType == verificador`; a entrega REPROVADO daquele agente não alimenta B3 (`bin/disjuntor.py:1191`, `bin/disjuntor.py:1427`, `bin/disjuntor.py:815`, `bin/disjuntor.py:821`).

12. **(5) B1 — bloqueia: permanecem caminhos liberais.**

   - Um agente em `SO_LEITURA` passa mesmo quando já foi encontrado job próprio antigo. O REVISADO não concede essa isenção (`bin/disjuntor.py:460`, `bin/disjuntor.py:466`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:15`).
   - Transcript ilegível retorna `disparo=False`; lançamento sem hora legível é ignorado (`bin/disjuntor.py:458`, `bin/disjuntor.py:530`).
   - Cópias são filtradas por `isdir`: uma cópia inacessível pode desaparecer, deixando outra com arquivo novo liberar a avaliação (`bin/disjuntor.py:468`, `bin/disjuntor.py:505`).
   - Job e término são reconhecidos por texto livre. Não se exige chamada Bash `run_in_background`, nem `task-notification`, nem estado terminal; qualquer `<status>[a-z]+</status>` correspondente elimina o job (`bin/disjuntor.py:514`, `bin/disjuntor.py:521`, `bin/disjuntor.py:525`).

13. **(9) CERTIFICADO — bloqueia: emissão e leitura não comprovam a suite.** A CLI pública aceita hashes, rc, recorte e log declarados pelo chamador; um log sintético com `Ran` e `OK` emite certificado. A guarda não protege essa emissão nem Write direto no `.ok`. O leitor exige somente hash e `rc=0`, aceitando arquivo sem `Ran` e sem `OK` (`bin/disjuntor.py:2229`, `bin/disjuntor.py:1226`, `bin/disjuntor.py:1234`, `bin/disjuntor.py:1242`, `bin/disjuntor.py:182`, `bin/disjuntor.py:1935`, `bin/disjuntor.py:1264`).

14. **(9) HASH — bloqueia: leitura parcial pode produzir identidade válida.** `git add --ignore-errors` tem o resultado ignorado; o comando seguinte pode emitir árvore apenas com a parte legível. O gate aceita o sucesso final e o hash, permitindo casar certificado de árvore parcial em vez de negar NAO_SEI (`bin/arvore_hash.sh:31`, `bin/arvore_hash.sh:32`, `bin/disjuntor.py:1220`).

   **Fila — corrida durante a suite:** H0 é calculado antes da trava; H1 depois da execução. Sem snapshot da cópia, A→B→A pode certificar A após testar B; a igualdade das duas pontas não demonstra a árvore executada (`bin/suite.sh:100`, `bin/suite.sh:102`, `bin/suite.sh:111`).

15. **(10) PUSH — bloqueia: três exclusões liberam sem julgamento.** [nao verificado]

   - `--no-merges` exclui instrumento introduzido na resolução de merge, sem essa exceção no REVISADO (`bin/contrato_do_push.py:37`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:20`).
   - Um commit novo que remova o próprio portão é tratado como anterior ao portão. Erro de `cat-file` também vira “não existe”, liberando nesse ramo (`bin/contrato_do_push.py:24`, `bin/contrato_do_push.py:41`).
   - Linha malformada é ignorada; entrada vazia devolve sucesso. O pre-push não verifica falha do `cat`; com `SKIP_TESTS`, pode sair verde após julgamento vazio (`bin/contrato_do_push.py:31`, `bin/contrato_do_push.py:33`, `bin/contrato_do_push.py:55`, `bin/pre-push.sh:7`, `bin/pre-push.sh:11`).

16. **(8) CUSTO — fila: medição incompleta apresentada sem marcar parcial.** Modelo/papel têm os gates previstos, mas `fable`, aceito pela porta, não possui preço e é descartado da soma. Arquivos ilegíveis também são ignorados sem sinalizar custo parcial (`bin/disjuntor.py:1412`, `bin/disjuntor.py:1417`, `bin/disjuntor.py:118`, `bin/disjuntor.py:126`, `bin/disjuntor.py:1529`, `bin/disjuntor.py:1533`).

17. **(4) RELÓGIO — fila: janela lida fora da borda.** `ler` recebe `agora`, mas `_b3_sem_obra` consulta outro `time.time()`, podendo usar janela diferente da medição recebida (`bin/disjuntor.py:2130`, `bin/disjuntor.py:2137`).

**Escritores e leitores solicitados.** A escrita física dos objetos do estado converge para `_com_estado`; há várias funções que os alteram logicamente (`bin/disjuntor.py:305`, `bin/disjuntor.py:310`, `bin/disjuntor.py:311`).

| Campo/arquivo | Quem escreve | Quem lê / multiplicidade |
|---|---|---|
| `voltas` | Criação em `bin/disjuntor.py:1373`; semente em `bin/disjuntor.py:1346` | Retorno em `bin/disjuntor.py:1374`; painel em `bin/disjuntor.py:1549` |
| `voltas.agent` | Semente `bin/disjuntor.py:1365`; autorização `bin/disjuntor.py:1484` | Teto `bin/disjuntor.py:1482`; mensagem `bin/disjuntor.py:1380`; painel `bin/disjuntor.py:1550`. **Dois escritores lógicos.** |
| `voltas.codex` | Semente `bin/disjuntor.py:1353`; reserva `bin/disjuntor.py:1393` | Teto `bin/disjuntor.py:1391`; mensagem `bin/disjuntor.py:1380`; painel `bin/disjuntor.py:1550`. **Dois escritores lógicos.** |
| `voltas.extra` | Inicialização `bin/disjuntor.py:1346`; `reabre` em `bin/disjuntor.py:1495` | Tetos `bin/disjuntor.py:1391`, `bin/disjuntor.py:1482`; painel `bin/disjuntor.py:1551` |
| `voltas.chaves` | Inicialização `bin/disjuntor.py:1346`; Codex `bin/disjuntor.py:1394`; Agent `bin/disjuntor.py:1485` | Codex consulta em `bin/disjuntor.py:1389`; Agent consulta apenas reservas em `bin/disjuntor.py:1467`. **Mais de um escritor.** |
| `reservas` | Criação `bin/disjuntor.py:1466`; poda `bin/disjuntor.py:1474`; inclusão `bin/disjuntor.py:1486` | Identidade `bin/disjuntor.py:1467`; poda/contagem `bin/disjuntor.py:1473`, `bin/disjuntor.py:1475`. Uma porta escritora. |
| `consumidas` / tipo | SHA `bin/disjuntor.py:1027`; mapa de tipo `bin/disjuntor.py:1023` | Deduplicação `bin/disjuntor.py:1014`; classificação antiga/nova `bin/disjuntor.py:1162`, `bin/disjuntor.py:1174`. Uma transição escritora. |
| `pede_desenho` | Liga `bin/disjuntor.py:1019`; remove `bin/disjuntor.py:1505` | Gate `bin/disjuntor.py:1010`. **Dois escritores.** |
| Triagem `cura=` | `triagem_cura`, em `bin/disjuntor.py:1307`; a triagem comum também escreve no mesmo arquivo em `bin/disjuntor.py:885` | Parser `bin/disjuntor.py:921`; cura `bin/disjuntor.py:1273`; idempotência `bin/disjuntor.py:1299`. B3/rearme também leem a trilha em `bin/disjuntor.py:963`, `bin/disjuntor.py:1095`. **Dois escritores do arquivo.** |
| `b3_sem_obra` com `t` | Migração `bin/disjuntor.py:1154`; coleta `bin/disjuntor.py:975`; negativa `bin/disjuntor.py:1434` | Deduplicação `bin/disjuntor.py:974`; janela `bin/disjuntor.py:2136`; painel `bin/disjuntor.py:2207`. **Mais de um escritor.** |
| `logs/suite_cert/<h>.ok` | Emissor `bin/disjuntor.py:1247`, chamado por `bin/suite.sh:112`; selo escreve diretamente em `bin/tests/test_disjuntor.sh:73`, `bin/tests/test_disjuntor.sh:1634` | Leitor operacional `bin/disjuntor.py:1260`, chamado em `bin/disjuntor.py:1444`. **Há escritor adicional explícito no selo; escritor adicional efetivo em prod [nao verificado].** |
| `DJ_VOLTA_CODEX` | Wrapper `bin/codex_pergunta.sh:166` | Porta `bin/disjuntor.py:1139`, `bin/disjuntor.py:1142` |
| `app/docs/contratos/<obra>.txt` | Artefato apresentado em `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:1`; selo cria contratos sintéticos em `bin/tests/test_contrato_do_push.sh:20`, `bin/tests/test_contrato_do_push.sh:32` | Gate consulta existência no PUSH_SHA em `bin/contrato_do_push.py:51`, via `bin/contrato_do_push.py:24`. Escritor operacional/autoria externa **[nao verificado]**. |

O ESTADO consome essas medições por `disjuntor.ler/linha`; não é outro escritor desses campos (`bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`).

**Selos e mutantes — achados.**

| Severidade | Mutante/caso | O que realmente acontece |
|---|---|---|
| **bloqueia** | `i4-sem-uma-obra`, `bin/tests/test_disjuntor.sh:2025` | O primeiro cenário tem zero obras; remover a guarda leva a `obras[0]`, causa `IndexError` e continua negando. O teste cai pela mensagem diferente antes de chegar ao cenário de duas obras (`bin/tests/test_disjuntor.sh:1621`, `bin/tests/test_disjuntor.sh:1622`, `bin/disjuntor.py:1470`, `bin/disjuntor.py:1996`). |
| **bloqueia** | `i9-uma-copia`, `bin/tests/test_disjuntor.sh:2033` | Sem cópia, remover a guarda leva a `copias[0]`; novamente há exceção e negativa, sem demonstrar autorização indevida (`bin/tests/test_disjuntor.sh:1640`, `bin/disjuntor.py:1443`, `bin/disjuntor.py:1996`). |
| **bloqueia** | `b3-codex-porta-sempre-livre`, `bin/tests/test_disjuntor.sh:1790` | Retira rc 3, mas a obra PARADA cai no ramo seguinte e recebe rc 4. Wrapper continua recusando; o caso morre porque exige exatamente 3 (`bin/disjuntor.py:1136`, `bin/tests/test_disjuntor.sh:1250`). |
| **bloqueia** | `cx8-ausente-sem-falha`, `bin/tests/test_disjuntor.sh:1818` | Permite alcançar `tri.get` com `tri=None`; a exceção vira NAO_SEI e mantém a recusa. O caso exige outro prefixo de negativa (`bin/disjuntor.py:1025`, `bin/disjuntor.py:1069`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1360`). |
| **fila** | `B3-duas-travas`, `bin/tests/test_disjuntor.sh:1902` | Acrescenta uma transação vazia; não divide a decisão existente. O teste o mata contando chamadas, sem reproduzir decisão partida (`bin/tests/test_disjuntor.sh:1904`, `bin/tests/test_disjuntor.sh:926`, `bin/tests/test_disjuntor.sh:935`). |
| **bloqueia** | `i9_cert` | O próprio fixture fabrica certificado sem `OK`, e o caso exige que passe. Testa emissão pela CLI com log sintético; retirar a chamada de certificação de `suite.sh` não é mordido (`bin/tests/test_disjuntor.sh:73`, `bin/tests/test_disjuntor.sh:1631`, `bin/tests/test_disjuntor.sh:1643`, `bin/suite.sh:112`). |
| **bloqueia** | `i11_vivos` / `i1_voltas` | Repetição de identidade ocorre somente antes da poda; vivos são simulados como reservas. `_ag` já escreve resultado antes da autorização. Não exercitam replay após poda, lançamento real ou disputa concorrente (`bin/tests/test_disjuntor.sh:1552`, `bin/tests/test_disjuntor.sh:1556`, `bin/tests/test_disjuntor.sh:339`, `bin/tests/test_disjuntor.sh:1566`). |
| **fila** | `test_contrato_do_push.sh::ordem` | Apenas compara primeiras ocorrências por grep; comentário contendo o nome do gate pode satisfazer a procura. Não executa pre-push com SKIP_TESTS (`bin/tests/test_contrato_do_push.sh:48`, `bin/tests/test_contrato_do_push.sh:49`, `bin/tests/test_contrato_do_push.sh:70`). |

Os demais mutantes P1 retiram os ramos declarados: CHAVE, modelo/papéis, tetos, reserva do wrapper, segundo ERRO-BOBO, cura/ancestralidade e condições do emissor de certificado (`bin/tests/test_disjuntor.sh:2003`, `bin/tests/test_disjuntor.sh:2006`, `bin/tests/test_disjuntor.sh:2011`, `bin/tests/test_disjuntor.sh:2014`, `bin/tests/test_disjuntor.sh:2018`, `bin/tests/test_disjuntor.sh:2021`, `bin/tests/test_disjuntor.sh:2027`, `bin/tests/test_disjuntor.sh:2034`). Os cinco mutantes do push alteram as costuras declaradas; o da ordem continua sendo prova textual (`bin/tests/test_contrato_do_push.sh:66`, `bin/tests/test_contrato_do_push.sh:70`).

Há mutantes mortos por ficarem **mais restritivos**, mediante casos positivos: sem poda, sem idempotência, reabre sem adicionar e redesenha sem remover pedido. Isso é verificável, mas não demonstra “liberou quando devia negar” (`bin/tests/test_disjuntor.sh:2012`, `bin/tests/test_disjuntor.sh:2013`, `bin/tests/test_disjuntor.sh:2016`, `bin/tests/test_disjuntor.sh:2020`). O harness aceita qualquer falha do caso como morte do mutante (`bin/tests/test_disjuntor.sh:1705`).

**Bloqueia — faltam mordidas específicas** para persistência da autorização, semeadura retroativa/parcial, idempotência após poda, migração de ERRO-BOBO antigo/ilegível, timestamps/janela, custo 24 h e tratamento restritivo de meta/cópia ilegível no B1. Os casos respectivos cobrem contagem nova, erros novos, universo, modelos/papéis e jobs legíveis; o bloco de mutantes P1 não retira essas outras costuras (`bin/tests/test_disjuntor.sh:1566`, `bin/tests/test_disjuntor.sh:1586`, `bin/tests/test_disjuntor.sh:1621`, `bin/tests/test_disjuntor.sh:1533`, `bin/tests/test_disjuntor.sh:1676`, `bin/tests/test_disjuntor.sh:2003`).

**Checklist da casa:** aplicam-se autorização antes da persistência, separação de decisão/reserva entre travas, universo derivado de duas formas, leitor de certificado com critério mais fraco que o emissor, relógio fora da borda e selos sem a mordida anunciada (`bin/disjuntor.py:318`, `bin/disjuntor.py:1142`, `bin/disjuntor.py:815`, `bin/disjuntor.py:1191`, `bin/disjuntor.py:1242`, `bin/disjuntor.py:1264`, `bin/disjuntor.py:2137`, `bin/tests/test_disjuntor.sh:1902`). B3 continua delegado ao juiz registrado; o defeito está na autorização posterior sem revalidação (`bin/disjuntor.py:1068`, `bin/disjuntor.py:1077`, `bin/disjuntor.py:1465`). Pouso efetivo antes de pré-requisito: **[nao verificado]**.

**Resposta direta à pergunta 5:** em `bin/pre-push.sh`, **sim**, o portão é chamado na linha 10, antes de `SKIP_TESTS` na linha 11; **não há saída normal anterior à chamada nesse arquivo** (`bin/pre-push.sh:7`, `bin/pre-push.sh:10`, `bin/pre-push.sh:11`). A instalação prevista delega para ele (`bin/hooks.sh:56`). Existe cópia divergente com SKIP no início, cuja utilização efetiva é **[nao verificado]** — **fila** (`app/core/hooks/pre-push.sh:5`). Mesmo no arquivo correto, permanecem os caminhos de julgamento vazio e exclusões descritos no achado 15 (`bin/contrato_do_push.py:33`, `bin/contrato_do_push.py:37`, `bin/contrato_do_push.py:41`).

## 01/11 01:40 · sessao 8e68217c962e · - · - · origem=Ronald (tmux)

Complemento do bloco antigo:

- **bloqueia — mutante não abre a porta.** `bin/tests/test_disjuntor.sh:1790`, `b3-codex-porta-sempre-livre`, remove só o retorno 3. A obra PARADA cai depois em `bin/disjuntor.py:1136` e retorna 4; o wrapper continua recusando. O caso mata o mutante porque exige exatamente 3 (`test_disjuntor.sh:1250`). Confirmado em memória.

- **bloqueia — mutante troca negativa por exceção.** `bin/tests/test_disjuntor.sh:1818`, `cx8-ausente-sem-falha`, deixa a execução alcançar `tri.get` com `tri=None` (`bin/disjuntor.py:1025`), causando `AttributeError`. `julga_b3` converte em NAO_SEI (`:1069`), mantendo a recusa. O teste falha em `test_disjuntor.sh:1360` porque `_xp` exige a mensagem “DISJUNTOR B3” (`:1261`), não “B3-NAO-SEI”. Confirmado em memória.

- **fila — mutante não parte a decisão entre travas.** `B3-duas-travas`, `bin/tests/test_disjuntor.sh:1902`, acrescenta uma transação vazia antes da decisão completa. A decisão continua inteira sob uma trava. O selo o mata contando chamadas (`:926`, `:935`), sem simular uma decisão repartida com corrida.

- Os **182 mutantes antigos** de `bin/tests/test_disjuntor.sh:1724` a `:2000` têm âncoras únicas; todos os Python resultantes passam AST e todos os Bash resultantes passam `bash -n`, verificados em memória.
- Os mutantes antigos da CHAVE retiram efetivamente o que declaram: `:1753`, `:1754`, `:1755`, `:1756`, `:1757`, `:1760`, `:1855`; os casos distinguem essas mudanças em `:616`, `:624`, `:630`, `:649`.
- Execução integral dos casos antigos: **[nao verificado]**.

## 01/11 01:38 · sessao 4e4238c0d392 · - · - · origem=Ronald (tmux)

Achados de `bin/disjuntor.py`, só leitura; reproduções mencionadas abaixo ocorreram em memória com I/O substituído, sem criar arquivos.

- **bloqueia — autorização sem persistência:** `bin/disjuntor.py:305`, `:313`, `:318`, `:323` deixam a decisão valer quando a gravação falha. `porta_agent` retorna essa autorização em `:1488`; `reserva_volta_codex` em `:1397`, sem conferir `gravou`. A chamada pode abrir sem registrar volta/reserva; chamadas seguintes reutilizam a vaga. B3 antigo confere persistência em `:1046`, mas as portas novas não repetem essa proteção.

- **bloqueia — exceção antes da porta libera Agent não final:** `pretool` lê estado em `bin/disjuntor.py:1965`; se ilegível, a execução pula `porta_agent` (`:1992`–`:1995`). A exceção fecha somente o ramo B3 (`:2005`–`:2014`); outro Agent termina liberado em `:2015`. Basta Agent construtor não final com estado ilegível: não passa pelos gates novos de modelo, papel, vivos ou volta.

- **bloqueia — B3 e autorização separados em duas travas:** Agent consulta B3 em `bin/disjuntor.py:1958`–`:1963`; reserva depois em `:1465`–`:1488`, sem revalidar B3. Wrapper consulta em `:1132`, reserva em `:1142`/`:1397`. Entre as duas transições, outro processo pode parar a obra; o primeiro ainda abre. O mesmo flock existe, mas não cobre a decisão completa.

- **bloqueia — corrida da contagem de vivos:** `txt`/`vivos_tx` são fotografados fora da trava (`bin/disjuntor.py:1455`–`:1463`); outro processo pode podar a reserva de um agente já lançado em `:1473`–`:1474`. Quando o processo com fotografia antiga entra na trava, o agente já não está em reservas e ainda não está na sua fotografia: `:1475` subconta e pode abrir o sétimo. A fotografia precisa corresponder ao estado sob o flock.

- **bloqueia — idempotência some depois da poda:** só `reservas` é consultado em `bin/disjuntor.py:1467`; `voltas.chaves` recebe a identidade em `:1485`, mas não é consultado pelo caminho Agent. Depois de `:1474` apagar a reserva, a mesma chamada incrementa de novo em `:1484`. Reproduzido em memória: `a,b,a`, com resultado de `a` antes de `b`, termina com `agent=3` e `chaves=['a','b','a']`. O teste só repete enquanto a reserva existe (`bin/tests/test_disjuntor.sh:1555`).

- **bloqueia — poda sem reconhecer resultado:** `bin/disjuntor.py:1473` procura uma substring compacta `"tool_use_id":"..."` em todo o transcript, sem verificar objeto `tool_result`. Texto citado pode eliminar reserva; JSON com espaço após `:` não a elimina. O primeiro caso permite subcontagem; o segundo conserva agentes já entregues.

- **bloqueia — semente Agent congelada em zero pelo wrapper/reabre:** `reserva_volta_codex` chama `_voltas(..., None)` em `bin/disjuntor.py:1388`; `reabre`, em `:1494`. `_semente` retorna `agent=0` e parcial quando não recebe transcript (`:1346`, `:1356`–`:1359`). `_voltas` nunca completa a semente existente (`:1372`–`:1374`). Assim, seis Agents anteriores podem ser ignorados se wrapper/reabre inicializar primeiro.

- **bloqueia — sidecar Codex ilegível não declara semente parcial:** `_le_id` devolve `None` em `bin/disjuntor.py:831`–`:833`; `_semente` simplesmente pula em `:1349`–`:1351`. Subconta voltas e não marca parcial, contrariando contrato `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:11`.

- **bloqueia — universo novo e coletor antigo divergem:** `_eh_verificador` admite papel na descrição (`bin/disjuntor.py:1191`); porta e PreToolUse o usam (`:1427`, `:1958`). Mas `_reprovacoes_finais` só coleta `meta.agentType == 'verificador'` (`:815`). Um `general-purpose` rotulado verificador pode entregar REPROVADO sem alimentar o B3. É derivação paralela do universo.

- **bloqueia — “só Ronald” é filtro lexical contornável:** `guarda_chave` bloqueia os literais CLI em `bin/disjuntor.py:192`–`:195`, mas não protege as funções `reabre`/`redesenha` (`:1491`–`:1507`) nem o estado contra Write (`:189`–`:212`). Verificado em memória: `guarda_chave` e `pretool` retornam `None` para Bash `python3 -c 'import sys; sys.path.insert(0,"bin"); import disjuntor; disjuntor.reabre("OBRA-X",3)'`; idem `redesenha`. Write direto em `logs/sessao/disjuntor_estado.json` também é admitido pela guarda. Não localizei segundo escritor operacional já existente fora dessas funções; a porta permite criar esse caminho.

- **bloqueia — HEAD ausente passa sem PROCEDENTE:** `head=None` é tolerado em `bin/disjuntor.py:1447`–`:1451`; só é negado dentro do loop dos PROCEDENTES em `:1276`–`:1281`. Com trilha vazia, retorna livre em `:1288`. A ressalva “cópia sem hash = nega” do contrato `:13` não é incondicional no código. Reproduzido em memória para `_cura_falta` com triagem vazia.

- **fila — formato `consumidas` diverge literalmente:** contrato pede `{sha,tipo}` (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:12`); implementação mantém lista de sha (`bin/disjuntor.py:1027`) mais mapa paralelo `consumidas_tipo` (`:1023`). A regra normal é implementada pelo leitor `:1162`–`:1174`, mas o formato escrito não é o contratado.

- **fila — relógio fora da borda:** `ler(..., agora)` chama `_b3_sem_obra` em `bin/disjuntor.py:2129`, que usa novo `time.time()` em `:2137`. A janela pode divergir do instante da medição fornecido.

- **fila — custo aceito e omitido:** `fable` é modelo aceito (`bin/disjuntor.py:118`), mas não possui preço em `:126`; `_custo_24h` o descarta em `:1533`–`:1535`. Arquivos ilegíveis são silenciosamente ignorados em `:1529`–`:1530`, podendo produzir custo parcial apresentado como total.

Rastreio dos campos, com escritores múltiplos explicitados:

| Campo | Escritores | Leitores |
|---|---|---|
| `voltas`/`agent`/`codex`/`extra`/`chaves` | criação/semente `bin/disjuntor.py:1346`, `:1353`, `:1365`, `:1373`; Codex incrementa `:1393`–`:1394`; Agent `:1484`–`:1485`; reabre `:1495` | `_voltas` `:1372`–`:1374`; Codex `:1389`–`:1392`; Agent `:1482`; mensagem `:1380`; painel `:1549`–`:1553` |
| `reservas` | cria `bin/disjuntor.py:1466`; poda `:1474`; grava `:1486` | `:1467`, `:1473`, `:1475` |
| `consumidas` / `consumidas_tipo` | `bin/disjuntor.py:1023`, `:1027` | `:1014`, `_erros_bobos` `:1162`–`:1174` |
| `pede_desenho` | liga `bin/disjuntor.py:1019`; remove `:1505` | `:1010` |
| triagem `cura=` | novo escritor `triagem_cura`, `bin/disjuntor.py:1307`–`:1309`; triagem sem cura continua escrevendo no mesmo arquivo em `:885`–`:887` | `_triagens` `:921`–`:926`; `_cura_falta` `:1273`–`:1287`; `triagem_cura` `:1299`–`:1305`; demais consumidores `_triagens` recebem o veredito sem o campo |
| `b3_sem_obra` com `t` | migração `bin/disjuntor.py:1154`–`:1155`; coletor `:975`; negativa da porta `:1434`–`:1435` | deduplicação `:974`; medição `:2136`–`:2138`; painel `:2207` |
| `DJ_VOLTA_CODEX` | `bin/codex_pergunta.sh:166` | `bin/disjuntor.py:1139`–`:1142` |

Todos esses objetos do estado são serializados pela mesma escrita física em `bin/disjuntor.py:308`–`:311`, embora várias funções façam mutações lógicas. `consumidas`/tipo têm uma transição escritora; `reservas` uma porta escritora; `pede_desenho`, triagem, `b3_sem_obra` e `voltas` têm mais de uma função escritora nos pontos acima.

CHAVE normal confere arquivo e linha (`bin/disjuntor.py:176`); RELATO ilegível mantém ligado (`:169`–`:171`); wrapper efetivamente consulta (`:1130`). O parâmetro `raiz` da chave vale só para o arquivo: `_chave_no_relato` não recebe raiz (`:164`), e `_topo_consistente` usa o `H.RELATO` global (`:749`–`:759`), podendo medir raiz diferente em chamadas programáticas de `ler(raiz)`; ocorrência em produção [nao verificado].

## 01/11 01:38 · sessao a855cc84a62c · - · - · origem=Ronald (tmux)

Achados para consolidar, todos por leitura; os dois checks de certificado usaram funções em memória com I/O substituído.

- **bloqueia — item 9, certificado forjável:** `bin/disjuntor.py:2229` expõe `--cert-suite`; `:1226–1242` confia no rc, recorte, hashes e log informados pelo chamador. Um log sintético `Ran 1 tests...\nOK` emite certificado sem executar suite. `guarda_chave` não guarda certificados (`:182`); Write/Bash da sessão principal passa pelo retorno de `pretool` em `:1935`. O leitor aceita somente `arvore=<h>` e `rc=0`, sem exigir Ran/OK (`:1264`). Confirmei ambas as funções em memória: certificado de duas linhas passa; log sintético emite certificado.

- **bloqueia — segundo escritor e selo que normaliza certificado inválido:** `bin/tests/test_disjuntor.sh:73` escreve `.ok` diretamente com `Ran=1`, sem OK; `i9_cert` considera esse arquivo verde em `:1631`. O escritor normal é `bin/disjuntor.py:1247`, acionado por `bin/suite.sh:112`; o leitor é `_cert_ok` em `bin/disjuntor.py:1260`, chamado pela porta em `:1468`. Os testes também sobrescrevem certificado em `bin/tests/test_disjuntor.sh:1634` e chamam a CLI usando log inventado em `:1642–1649`. Não há integração de `suite.sh` real nesse caso.

- **bloqueia — item 9, hash parcial pode virar identidade válida:** `bin/arvore_hash.sh:31` ignora erro de `git add --ignore-errors`; `:32` pode terminar com sucesso e emitir hash da parte legível. O gate só testa rc final e sintaxe do hash (`bin/disjuntor.py:1220`). Portanto árvore não integralmente lida pode casar com certificado da árvore parcial, em vez de NAO_SEI. Erros também são ocultados em `bin/arvore_hash.sh:31` e `bin/suite.sh:100`.

- **bloqueia — item 10 exclui merges sem amparo no REVISADO:** `bin/contrato_do_push.py:37` usa `--no-merges`. Uma resolução de merge que acrescente/mude instrumento em `bin/` não é julgada. O contrato `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:20` não exclui merge. O selo só cria commits lineares (`bin/tests/test_contrato_do_push.sh:29–43`).

- **bloqueia — item 10 usa presença do próprio juiz como marcador de era:** `bin/contrato_do_push.py:41` pula qualquer commit cuja árvore não contenha o portão. Um commit novo que apague o arquivo do portão, inclusive junto de outro instrumento, passa pelo ramo de “não retroage”. Além disso, `_existe` colapsa qualquer erro de `cat-file` para ausência (`:24–25`), que nesse ponto libera. O selo cobre commit anterior legítimo, não remoção posterior (`bin/tests/test_contrato_do_push.sh:30`, `:43`).

- **bloqueia — entrada ilegível/malformada pode virar push sem julgamento:** `bin/pre-push.sh:7` não verifica erro do `cat`; `set -e` só aparece em `:79`. Entrada vazia devolve rc 0 pelo loop vazio (`bin/contrato_do_push.py:31`, `:55`); linhas malformadas são silenciosamente ignoradas (`:33–34`). Com SKIP_TESTS, o hook sai 0 em `bin/pre-push.sh:11`.

- **Item 5, bloqueia — allowlist evita B1 novo:** `bin/disjuntor.py:466` libera tipos SO_LEITURA antes de avaliar cópias e job antigo, embora `:460` já tenha encontrado job. Verificador/Explore/Plan com job próprio e cópia parada escapa. O contrato `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:15` não prevê essa exceção.

- **Item 5, bloqueia — cópia ilegível desaparece:** `bin/disjuntor.py:468` filtra caminhos citados por `os.path.isdir`. Se há uma cópia ilegível/inacessível e outra legível com arquivo novo, a primeira some e a segunda libera em `:505–506`. Transcript ilegível também devolve `disparo=False` em `:458–459`; hora ilegível do lançamento é ignorada em `:530–531`.

- **Item 5, bloqueia — fim/job derivados de texto livre:** `bin/disjuntor.py:521` aceita `<task-id>…<status>[a-z]+</status>` em qualquer texto, sem exigir task-notification nem estado terminal; uma citação ou `running` cancela o job. `:525` reconhece job pela frase de saída, sem conferir chamada Bash com `run_in_background`. O selo só fornece frase de saída (`bin/tests/test_disjuntor.sh:1666`) e fim completed; não exercita autenticidade.

- **Resposta direta à pergunta 5:** em `bin/pre-push.sh` a chamada é `:10`, SKIP_TESTS é `:11`, e não há exit anterior. A instalação declarada delega a esse arquivo em `bin/hooks.sh:56`; instalação efetiva [nao verificado]. Existe cópia divergente `app/core/hooks/pre-push.sh:5`, com SKIP_TESTS no início e sem portão; uso efetivo [nao verificado], portanto **fila** para remover ambiguidade.

- **fila — corrida ABA na suite:** H0 é calculado antes de adquirir trava (`bin/suite.sh:100`, trava em `:102`); H1 é calculado em `:111`. A cópia pode ser A em H0, B durante testes, A em H1 e receber certificado A. Não há snapshot ou imobilização da árvore. O escritor satisfaz a igualdade literal antes/depois, mas ela não prova a árvore efetivamente testada.

Escritores/leitores de contratos: contrato versionado presente em `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:1`; o leitor operacional é `bin/contrato_do_push.py:51` via `cat-file` em `:24`. Nenhum escritor operacional de contratos foi encontrado; o selo cria contratos sintéticos por `_commit` (`bin/tests/test_contrato_do_push.sh:20–21`, chamadas `:32`, `:37`). autoria/materialização externa [nao verificado].

## 01/11 01:38 · sessao 8e68217c962e · - · - · origem=Ronald (tmux)

Achados dos selos, por leitura e sondas em memória, sem escrever arquivos:

- **bloqueia — mutante morto por exceção, item 4.** `bin/tests/test_disjuntor.sh:2025` remove a guarda de obra única, mas `i4_universo` começa pelo prompt sem obra (`:1621`): o mutante chega a `obras[0]` em `bin/disjuntor.py:1470`, causa `IndexError` e continua negando via `:1996`. O teste cai porque mudou a mensagem, não porque liberou. O caso de duas obras (`test_disjuntor.sh:1622`), que mostraria liberação indevida, nem é alcançado. `IndexError` confirmado em execução isolada em memória.

- **bloqueia — mutante morto por exceção, item 9.** `bin/tests/test_disjuntor.sh:2033` remove a guarda de cópia. O caso sem cópia (`:1640`) passa a acessar `copias[0]` em `bin/disjuntor.py:1443`; recebe `IndexError` e continua negando em `:1996`. O harness aceita qualquer retorno não zero do caso como mutante morto (`test_disjuntor.sh:1705`). `IndexError` confirmado em memória.

- **bloqueia — certificado artificial aceito pelo próprio selo.** `_arvore` escreve diretamente `arvore=…`, `rc=0`, `Ran=1`, sem `OK`, em `bin/tests/test_disjuntor.sh:73`; `i9_cert` exige autorização com esse arquivo em `:1631`. Isso demonstra aceitação de arquivo que não tem o formato produzido pelo escritor (`bin/disjuntor.py:1243`). O teste da emissão chama a CLI diretamente (`test_disjuntor.sh:1643`), sem exercitar `suite.sh → --cert-suite` (`bin/suite.sh:112`); retirar essa chamada da suite preserva o caso i9.

- **bloqueia — identidade exercitada só enquanto a reserva existe.** `i11_vivos` repete o mesmo ID antes do resultado (`bin/tests/test_disjuntor.sh:1556`), sem obra (`:1552`). Não repete uma autorização da mesma obra após poda. `_ag` gera ID novo e já acrescenta seu resultado antes da autorização (`:335`, `:339`), portanto os testes de voltas não mordem idempotência após poda nem o lançamento real.

- **bloqueia — sincronismo e concorrência não são medidos por i11.** As seis primeiras vagas são apenas reservas sem resultado (`bin/tests/test_disjuntor.sh:1552`); o teste não cria agentes síncronos rodando nem disputa simultânea entre autorizações. Assim, o caso permanece verde se o leitor de agentes lançados retornar sempre vazio, desde que a contagem de reservas permaneça (`bin/disjuntor.py:1475`).

- **bloqueia — cláusulas sem mutante correspondente.** O bloco P1 (`bin/tests/test_disjuntor.sh:2003`) não retira persistência da autorização, semeadura retroativa/parcial, idempotência por `chaves`, migração de ERRO-BOBO antigo/ilegível, timestamps/janela de `b3_sem_obra`, custo 24h nem tratamento restritivo de meta/cópia ilegível no B1. Os respectivos casos limitam-se a contagem nova (`:1566`), ERRO-BOBO novo (`:1586`), universo (`:1621`), modelos/papéis (`:1533`) e três cenários legíveis de job (`:1676`). Retirar essas costuras não é mordido por seus casos P1.

- **fila — ordem do pre-push aferida apenas por texto.** `bin/tests/test_contrato_do_push.sh:48` procura a primeira ocorrência do nome do arquivo, inclusive comentário; `:49` procura o primeiro teste de `SKIP_TESTS`. Não executa o hook. Uma menção anterior em comentário e uma chamada efetiva depois do skip podem passar. O mutante específico `skip-antes` (`:70`) é detectado por esse grep.

Verificação dos mutantes:

- Todos os **32 mutantes P1** de `bin/tests/test_disjuntor.sh:2003` têm âncora única; todas as mutações Python resultantes compilam, conferidas em memória. Os dois problemas de mordida são os `IndexError` descritos acima, não erros de sintaxe.
- Os demais removem os ramos declarados. `i11-sem-poda`, `i11-nao-idempotente`, `i1-reabre-nada` e `i2-redesenha-nao-tira` são mortos por negar uma autorização esperada, não por liberar algo proibido (`:2012`, `:2013`, `:2016`, `:2020`).
- Os cinco mutantes do push têm âncoras únicas e removem as costuras declaradas: contrato obrigatório, não retroatividade, exclusão de testes, existência do contrato e posição do skip (`bin/tests/test_contrato_do_push.sh:66`).
- Execução integral dos selos: **[nao verificado]**, porque criam e apagam arquivos (`test_disjuntor.sh:39`, `test_contrato_do_push.sh:14`).

## 01/11 00:58 · sessao 45f7099e218f · COBERTURA-PARCIAL-UM-JUIZ · verificador · origem=Code

**Parecer: bloqueia.** Há cobrança de marcos cobertos no noturno, âncora com data errada e leitores que ainda calam o resto da parcial. (`app/escala/utils.py:1335`, `app/ponto/services/cartorio.py:1719`, `app/chamados/services/materializacao.py:74`, `app/chamados/reconciliador.py:825`)

Os casos abaixo são contraexemplos do código; ocorrência e quantidade em produção, execução da suíte Django e cumprimento dos atos de pouso ficam **[nao verificado]**. [nao verificado]

**1. Bloqueia — noturno com intervalo dispensa os marcos errados.**

Caso: turno **D19 → D+1 02 → D+1 03 → D+1 07**, parcial **23–07**. A régua escolhe a menor entrada, **03**, e entrega `[E03,S07,E19,S02]`; essa régua chega diretamente à marcação das ausências. (`app/escala/utils.py:1335`, `app/escala/utils.py:1343`, `app/escala/utils.py:1362`, `app/escala/utils.py:1805`)

`deslocamentos_dos_marcos` encadeia essa ordem como se fosse a sequência do turno: produz **180, 420, 1140, 1560** minutos. Portanto, dispensa S02, mas deixa E03 e S07 faltantes, embora ambos pertençam à madrugada coberta. Pela regra do período noturno, os deslocamentos corretos seriam **1140, 1560, 1620, 1860**. (`app/ponto/turnos.py:2652`, `app/ponto/turnos.py:2661`, `app/escala/utils.py:345`, `app/ponto/turnos.py:2636`, `app/escala/utils.py:1209`)

A falha está na alimentação do juiz com a régua de apresentação. O teste noturno fornece diretamente apenas E19/S07 e não exercita essa costura. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:53`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:62`)

**2. Bloqueia — a nova âncora parcial perde a data da madrugada.**

Caso: turno **D19–D+1 07**, parcial **D19–23**, sem batidas. E19 é dispensada; sobra S07. O ramo novo adota a âncora de `_marcos_do_furo_parcial`, mas esse auxiliar ordena `HH:MM` e combina a primeira hora com **D**: resulta **D07**, 24 horas antes do marco devido. (`app/ponto/services/cartorio.py:1578`, `app/ponto/services/cartorio.py:1717`, `app/ponto/services/cartorio.py:1719`)

Isso acrescenta **1.440 minutos** ao atraso e grava/apresenta `horario_previsto` errado no contexto e no push. O SLA dessa porta usa `agora` e janela; não atribuo à âncora alteração direta desse prazo. (`app/ponto/management/commands/detectar_ausencias.py:581`, `app/ponto/management/commands/detectar_ausencias.py:702`, `app/ponto/management/commands/detectar_ausencias.py:776`, `app/ponto/management/commands/detectar_ausencias.py:593`)

**3. Bloqueia — duas ausências fazem materialização e grade escolherem coberturas diferentes.**

Caso: ausência integral antiga e parcial com período mais recente, ambas aprovadas, com marco faltante fora da parcial. O juiz de cobertura escolhe a maior `(data, pk)`; a grade segue essa vencedora e cobra o resto. A materialização usa `any()` sobre todas as ausências não-falta e cala a pergunta pela integral perdedora. (`app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2557`, `app/ponto/turnos.py:2784`, `app/escala/utils.py:1149`, `app/chamados/services/materializacao.py:74`)

`pergunta_no_app` e `disputa_emissao` propagam exatamente esse retorno. Assim, `_celula_furada` pode responder “furada” e a fábrica impedir a pergunta para o mesmo dia. (`app/chamados/services/pergunta_no_app.py:74`, `app/chamados/services/disputa_emissao.py:763`, `app/chamados/juizes.py:446`, `app/chamados/juizes.py:478`)

O argumento vazio **não é o defeito**: com os dois horários preenchidos, `periodo_da_parcial_no_dia(a, ())` retorna `fora`; faltando um horário, retorna `sem_periodo`. Logo, a comparação exclusiva com `sem_periodo` continua detectando ausência de cadastro. (`app/ponto/turnos.py:2632`, `app/ponto/turnos.py:2638`, `app/ponto/turnos.py:2641`)

**4. Bloqueia — ainda há leitores não migrados.**

| Leitor | Caso concreto e resposta divergente |
|---|---|
| Reconciliação c2 | Parcial com período e marco externo apagado: qualquer ausência vencedora veta criação, reabertura e até costura no chamado vivo. (`app/chamados/reconciliador.py:811`, `app/chamados/reconciliador.py:825`) |
| Calendário | Parcial sem batida, com resto cobrado: retorna `ausencia` antes de consultar o veredito; o status lavrado pode ser `irregular`. (`app/colaboradores/services/calendario.py:109`, `app/ponto/status_dia.py:98`, `app/ponto/status_dia.py:112`) |
| Relatório de furos | O mesmo dia parcial é classificado como `folga` antes de `estado_do_dia`; também desaparece das funções usadas pelo Raio-X. (`app/relatorios/furos.py:151`, `app/relatorios/furos.py:218`, `app/relatorios/furos.py:237`) |

No calendário, isso também exclui o dia do universo de “Resolver dia”, que exige `falta`, `irregular`, `aberto` ou `sem_certidao`. (`app/colaboradores/services/calendario.py:181`)

**5. Bloqueia a equivalência universal declarada — `falta` é uma exceção explícita.**

Pela definição fornecida, toda falta é parcial: `ausencia_explica_o_dia_inteiro` devolve `False` para `tipo='falta'`. Entretanto, a grade apaga suas células e órfãs, não grava o carimbo parcial e o supra exclui falta de `dia_de_cobranca`, declaradamente para evitar cobrança dobrada sobre dia já descontado. (`app/ponto/turnos.py:2690`, `app/escala/utils.py:1174`, `app/escala/utils.py:1178`, `app/ponto/supra_juiz.py:302`)

Portanto, o código não satisfaz literalmente “todo retorno `False` cobra o resto”. Isso tampouco é uma combinação real alterada por `_resto_cobrado`, pois a grade não produz o carimbo nessa falta. O catálogo padrão não aceita falta por minutos; existência de registro excepcional em produção: **[nao verificado]**. (`app/escala/utils.py:1191`, `app/ponto/catalogo/ausencias.py:88`)

**6. Matriz da alteração em `COBRANCA_EM_DIA_COBERTO`.**

Excluído o retorno inicial por isenção, o código anteriormente emitido é retirado exatamente nestas combinações; nenhuma das demais combinações muda por essa condição. (`app/ponto/supra_juiz.py:220`, `app/ponto/supra_juiz.py:530`)

| Insumo | Combinações que retiram o código |
|---|---|
| `cobertura_status` | `aprovada` |
| `tipo_dia` | Qualquer valor formal: o ramo não verifica o tipo |
| `cobertura_parcial` | Verdadeiro |
| `periodo_parcial` | `dentro`, `fora`, `nao_sei`; também `None`/ausente ou qualquer valor diferente de `sem_periodo` |
| `_n_missing` | Maior que zero |
| `chamados_vivos` | Maior que zero |
| `cobrancas_vivas` | Tanto zero quanto maior que zero |

Fonte da matriz: `app/ponto/supra_juiz.py:255`, `app/ponto/supra_juiz.py:268`, `app/ponto/supra_juiz.py:306`, `app/ponto/supra_juiz.py:530`.

**Retirar o código não equivale sempre a mudar `coberta` para `cobrado`:**

- Se já havia código de `furo` ou `cobranca_indevida`, ele já vencia `coberta`; o veredito permanece. (`app/escala/models.py:1580`)
- Se `coberta` estava vencendo códigos de cadastro, discordância ou fato, esses códigos passam a determinar o veredito. (`app/escala/models.py:1582`)
- Sem código restante, a derivação devolve `concorde`. Somente o cartório substitui esse resultado por `cobrado`, exigindo fio vivo **e protestante**; decisão administrativa continua prevalecendo. (`app/escala/models.py:1595`, `app/ponto/services/cartorio.py:1294`, `app/ponto/portas/celula.py:655`)

**Existe vazio sem `cobrado`.** Contraexemplo puro: ausência parcial, período `fora`, um missing, realizado 240, previsto 480, `sem_ancora=True`, turno encerrado, chamado vivo e nenhuma cobrança com fio. O ramo do fato não emite furo por `sem_ancora`, o novo ramo retira cobertura e o cartório não tem fio para escrever `cobrado`: sobra `concorde`. Ocorrência produtiva desses insumos: **[nao verificado]**. (`app/ponto/supra_juiz.py:392`, `app/ponto/supra_juiz.py:530`, `app/ponto/services/cartorio.py:1294`)

No caso ordinário **encerrado, previsto positivo, sem fato e com fio morto**, não há esse vazio: sai `FURO_COM_COBRANCA_MORTA`. (`app/ponto/supra_juiz.py:313`, `app/ponto/supra_juiz.py:360`)

A retirada não cria, sozinha, acusação de marco coberto quando a grade está correta; porém o contraexemplo noturno do achado 1 produz missing dentro da cobertura e derruba essa premissa. (`app/escala/utils.py:1209`, `app/ponto/supra_juiz.py:530`)

**7. Consumidores de `coberta` e efeito da mudança.**

| Consumidor | Efeito |
|---|---|
| Pergunta viva | `coberta` não acusa; `cobrado`, `furo` e demais acusadores mantêm a pergunta, ressalvada sua resolução própria. (`app/escala/models.py:1735`, `app/chamados/juizes.py:1102`) |
| Retratação | Veredito não acusador permite absolvição; marcos pendentes impedem essa conclusão antecipada. (`app/chamados/reconciliador.py:973`, `app/chamados/reconciliador.py:982`) |
| Emissão/reabertura | `coberta` integra os vereditos que vetam cobrança; `cobrado` não integra essa lista. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:154`) |
| Status lavrado | Sem batida, `coberta` dá ausência; parcial acusadora dá irregular. Com batida, `coberta` dá ok e acusação dá irregular. (`app/ponto/status_dia.py:98`, `app/ponto/status_dia.py:110`, `app/ponto/status_dia.py:117`) |
| Calendário | O mapa distingue `coberta→ok` e `cobrado→irregular`, mas o retorno antecipado continua escondendo a mudança na ausência sem batida. (`app/colaboradores/services/calendario.py:54`, `app/colaboradores/services/calendario.py:109`) |
| Folha/retenção | Acusadores passam pelo retentor, com exceções de protestos não cobráveis e furo exclusivamente de pausa; `coberta` fica fora desse ramo. (`app/folha/export.py:1215`, `app/folha/export.py:1235`) |
| Relatórios | `estado_do_dia` mapeia `coberta→folga` e `cobrado→furo`, mas o dataset pula antes os dias de ausência. Dia da frota distingue os rótulos e seleciona acusadores. (`app/relatorios/furos.py:24`, `app/relatorios/furos.py:151`, `app/relatorios/dia_da_frota.py:10`, `app/relatorios/dia_da_frota.py:54`) |

O executor do comando supra retrata diretamente por `COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO`; a consequência de `COBRANCA_EM_DIA_COBERTO` passa pela derivação e pelos consumidores do veredito. (`app/ponto/management/commands/supra_juiz.py:218`, `app/escala/models.py:1582`)

**8. Censo de escritores e leitores, inclusive fora do diff.**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `cobertura_parcial` | Builder consulta integralidade e carimba; leitor da ata também carimba pela cobertura vigente; cartório propaga para a ata. (`app/escala/utils.py:1151`, `app/escala/utils.py:1191`, `app/escala/services/leitor_celula.py:409`, `app/ponto/services/cartorio.py:628`) | Supra; impressão; dispensa integral; contagem de emissão/espelho; status; predicado usado por `dia_sem_trabalho` e declaração textual. (`app/ponto/supra_juiz.py:268`, `app/ponto/services/cartorio.py:345`, `app/ponto/services/cartorio.py:653`, `app/ponto/services/cartorio.py:1547`, `app/ponto/services/cartorio.py:1631`, `app/ponto/status_dia.py:150`, `app/chamados/juizes.py:408`, `app/chamados/services/declaracao_texto.py:98`) |
| `periodo_parcial` | Builder e leitor da ata chamam o juiz de período. Não é persistido por `ata_do_dia`; é recalculado pelo leitor. (`app/escala/utils.py:1195`, `app/escala/services/leitor_celula.py:412`, `app/ponto/services/cartorio.py:600`) | Dispensa por marco, impressão e ramos do supra. (`app/escala/utils.py:1204`, `app/ponto/services/cartorio.py:345`, `app/ponto/supra_juiz.py:381`, `app/ponto/supra_juiz.py:439`, `app/ponto/supra_juiz.py:530`) |
| Célula `dispensada` | Grade dispensa missing dentro do período; leitor reconstrói o marcador da lâmpada. (`app/escala/utils.py:1206`, `app/escala/services/leitor_celula.py:127`) | Impressão, propagação à lâmpada, exclusão de batida no cartório/status e ausência de resto no supra. (`app/ponto/services/cartorio.py:346`, `app/ponto/services/cartorio.py:457`, `app/ponto/services/cartorio.py:1217`, `app/ponto/status_dia.py:145`, `app/ponto/supra_juiz.py:311`) |
| Lâmpada `dispensada` | Propagação da célula; dispensa integral/justificativa no cartório. (`app/ponto/services/cartorio.py:458`, `app/ponto/services/cartorio.py:662`) | Reconstrução da célula e `_n_dispensadas`; juízes de lastro/furo leem o efeito `acesa`, sem exigir luz. (`app/escala/services/leitor_celula.py:68`, `app/escala/services/leitor_celula.py:127`, `app/ponto/services/lastro.py:179`, `app/chamados/juizes.py:478`) |
| `n_ok_lavrado` / `n_casadas` | Derivados de contagens da ata menos dispensadas; transportados no dia. (`app/escala/services/leitor_celula.py:91`, `app/escala/services/leitor_celula.py:97`, `app/escala/services/leitor_celula.py:369`) | Supra prefere os valores lavrados; contador de bordas usa `n_ok` como prova de batida. (`app/ponto/supra_juiz.py:229`, `app/ponto/supra_juiz.py:464`, `app/ponto/services/bordas_realizado.py:51`) |
| `_teve_fato` | Variável local derivada de realizado ou contagem positiva. (`app/escala/services/leitor_celula.py:343`) | `_fora`, que decide exclusão antes da admissão e no dia da demissão. (`app/escala/services/leitor_celula.py:347`) |
| `parcial_na_lista_sem_push` | `_contar_na_lista`, chamado nos dois portões de emissão. (`app/ponto/services/cartorio.py:1673`, `app/ponto/services/cartorio.py:1564`, `app/ponto/services/cartorio.py:1616`) | Incremento e testes; o resumo do comando não imprime essa chave. (`app/ponto/services/cartorio.py:1673`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:451`, `app/ponto/management/commands/processar_cartorio.py:66`) |
| `_parcial_na_lista_vistos` | `setdefault` e `add`; chave `(colab,data)`. (`app/ponto/services/cartorio.py:1551`, `app/ponto/services/cartorio.py:1670`) | Teste de pertinência; `finally` remove o conjunto da saída. (`app/ponto/services/cartorio.py:1671`, `app/ponto/services/cartorio.py:922`) |
| Âncora parcial | `_marcos_do_furo_parcial`. (`app/ponto/services/cartorio.py:1719`) | Ambos os ramos de emissão; emissor calcula atraso, contexto e push. (`app/ponto/services/cartorio.py:1580`, `app/ponto/services/cartorio.py:1619`, `app/ponto/management/commands/detectar_ausencias.py:581`, `app/ponto/management/commands/detectar_ausencias.py:702`, `app/ponto/management/commands/detectar_ausencias.py:776`) |
| `REGRA_VERSOES` | Declaração no cartório. (`app/ponto/services/cartorio.py:111`) | Validação, seleção da maior versão aplicável, impressão e testes de alcance. (`app/ponto/services/cartorio.py:179`, `app/ponto/services/cartorio.py:208`, `app/ponto/services/cartorio.py:380`, `app/ponto/tests/test_o219_versao_da_regra.py:77`) |
| Código / `coberta` / `cobrado` | Código nasce no supra; mapa deriva `coberta`; cartório propõe `cobrado`; porta decide e persiste. (`app/ponto/supra_juiz.py:532`, `app/escala/models.py:1582`, `app/ponto/services/cartorio.py:1295`, `app/ponto/portas/celula.py:658`, `app/ponto/portas/celula.py:745`) | Consumidores discriminados no achado 7. (`app/chamados/juizes.py:1102`, `app/folha/export.py:1215`, `app/relatorios/furos.py:24`) |
| Retorno `_coberto_por_ausencia` | Predicado de materialização; reexportado por models e importado por disputa. (`app/chamados/services/materializacao.py:55`, `app/chamados/models.py:3141`, `app/chamados/services/disputa_emissao.py:8`) | Retenção da pergunta no app e guarda antes da abertura da pergunta. (`app/chamados/services/pergunta_no_app.py:74`, `app/chamados/services/disputa_emissao.py:763`) |

**9. Contraexemplos restantes e limites dos testes.** [nao verificado]

- **Todos os marcos dispensados, chamado vivo:** não há furo; o supra puro mantém `COBRANCA_EM_DIA_COBERTO`. No cartório, chamado com marcos nomeados todos acesos deixa de protestar e pode ser fechado pelo fluxo por marco. Isso respeita “cobra o resto”: não sobrou marco. (`app/ponto/supra_juiz.py:311`, `app/ponto/supra_juiz.py:530`, `app/ponto/services/lastro.py:170`, `app/ponto/services/cartorio.py:1250`)
- **Hoje/VIGIA:** missing futuro sai do dia julgado e vira lâmpada `acesa=None, pendente=True`. Se nenhum missing vencido sobra, o código de cobertura pode aparecer; as guardas de pergunta e retratação impedem tratar a ata provisória como absolvição definitiva. (`app/ponto/services/cartorio.py:737`, `app/ponto/services/cartorio.py:755`, `app/chamados/juizes.py:1115`, `app/chamados/reconciliador.py:984`)
- **Fila — contagem ainda diverge com VIGIA:** duas dispensadas e duas pendentes, todas sem luz, dão `n_ok_lavrado=2`, mas a recontagem por hora dá zero. Isso também faz `_teve_fato=True`; subtrair apenas dispensadas não garante a igualdade anunciada pelos testes. (`app/escala/services/leitor_celula.py:91`, `app/ponto/supra_juiz.py:83`, `app/escala/services/leitor_celula.py:344`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:370`)

Os testes atravessam grade→ata→supra, julgamento→pergunta viva, edição→impressão→rejulgamento, declaração textual, espelho e duas passadas do contador. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:175`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:304`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:354`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:397`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:426`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:441`)

**Sim, testes passam com cura revertida:** os três controles finais preservam exatamente dia inteiro, parcial sem período e parcial sem resto. O RED de retirada do código distingue a cura; os dois testes de contagem distinguem a subtração das dispensadas. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:476`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:480`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:484`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:473`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:380`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:385`)

A lacuna bloqueante é concreta: teste noturno pula a construção da régua; teste de âncora é diurno e verifica somente `.time()`, não a data. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:43`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:423`)

**10. Checklist estrutural e pouso.** [nao verificado]

| Classe | Conclusão |
|---|---|
| Segundo juiz | O novo helper apenas lê o carimbo; não recalcula integralidade. Porém o `any()` da materialização conserva seleção paralela de cobertura, demonstrada no achado 3. (`app/chamados/juizes.py:408`, `app/docs/JUIZES.md:60`, `app/chamados/services/materializacao.py:74`) |
| Segundo escritor | Os ramos novos reutilizam `_espelhar_marcos`→`renovar_contexto`; ata e veredito continuam pelas portas. Segundo escritor persistente nesses ramos: não demonstrado. (`app/ponto/services/cartorio.py:1639`, `app/ponto/management/commands/detectar_ausencias.py:553`, `app/chamados/services/contexto.py:96`, `app/ponto/services/cartorio.py:1643`) |
| Relógio fora da borda | **Fila, preexistente:** `julgar_celula` aceita `agora`, mas consulta data e instante reais antes de repassá-lo. Caso: instante injetado diferente do host altera o portão temporal. (`app/ponto/services/cartorio.py:1778`, `app/ponto/services/cartorio.py:1783`, `app/ponto/services/cartorio.py:1804`) |
| Fonte única | **Bloqueia:** cobertura por `any()` diverge do desempate canônico; cadeia noturna recebe régua inadequada. (`app/chamados/services/materializacao.py:74`, `app/ponto/turnos.py:2784`, `app/escala/utils.py:1335`) |
| Ordem de trava | Abertura mantém trava do colaborador; atualização de contexto usa sua porta existente. Inversão/deadlock causado pela obra: **[nao verificado]**. (`app/chamados/models.py:861`, `app/chamados/services/contexto.py:96`) |
| Versão 3 / competência 09 | O horizonte alcança **toda competência ≥09**, não apenas cinco dias, e invalida impressões inclusive sem mudança de marcos dispensados. Não seleciona uma implementação histórica isolada. (`app/ponto/services/cartorio.py:208`, `app/ponto/services/cartorio.py:380`, `app/ponto/services/cartorio.py:1192`) |

A 09 exportada, por si só, não impede rejulgar ata: o aval escrito autoriza isso e exige fechamento intacto com hashes antes/depois e destino da diferença financeira. A guarda existente exclui do recálculo os colaboradores das empresas exportadas da 09. (`a

[truncado em 20000 de 20455 caracteres]

## 01/11 00:54 · sessao 8e53981aa4d4 · - · - · origem=Ronald (tmux)

Achados e matriz, somente código:

1. **Bloqueia — âncora noturna errada, agora usada também no ramo de parcial sem batida.** `_marcos_do_furo_parcial` ordena `HH:MM` lexicograficamente e combina a hora com a data inicial, sem encadear madrugada (`app/ponto/services/cartorio.py:1717-1720`). O ramo novo adota essa âncora (`app/ponto/services/cartorio.py:1578-1582`). Caso: turno D 19h–D+1 07h, parcial 19h–23h dispensa E19; S07 segue apagada. A cobrança fica ancorada **D 07h**, 24h antes de seu marco. Com mais marcos apagados, a ordenação pode escolher a madrugada antes dos marcos anteriores à meia-noite. A âncora alimenta `atraso_min`, contexto e texto do push (`app/ponto/management/commands/detectar_ausencias.py:581`, `:702-703`, `:776`). **Corrigindo minha mensagem anterior:** o SLA usa `agora`, portanto não demonstrei mudança direta do prazo por essa âncora (`app/ponto/management/commands/detectar_ausencias.py:593-597`).

2. **Bloqueia — calendário ainda absolve visualmente o dia parcial sem batida.** A classificação retorna `ausencia` antes de ler o veredito quando há faixa de ausência e nenhuma batida (`app/colaboradores/services/calendario.py:109-110`). `declaracao` cai na faixa padrão `atestado` (`app/ponto/precedencia.py:76-79`). O chamador não fornece cobertura parcial (`app/colaboradores/services/calendario.py:799-819`). Mesmo caso parcial com resto cobrado: status da lavra fica `irregular` (`app/ponto/status_dia.py:98-113`), folha retém `cobrado` (`app/escala/models.py:1735-1736`; `app/folha/export.py:1215`, `:1235-1236`), mas calendário continua `ausencia`. Isso também mantém o dia fora de “Resolver dia”, cujo universo exige `falta|irregular|aberto|sem_certidao` (`app/colaboradores/services/calendario.py:181`).

3. **Matriz exata da retirada do código.** A nova condição retira `COBRANCA_EM_DIA_COBERTO` se, e somente se, cobertura aprovada, `chamados_vivos` verdadeiro, `cobertura_parcial` verdadeira, `_n_missing>0`, `periodo_parcial != 'sem_periodo'` (`app/ponto/supra_juiz.py:255`, `:268`, `:306`, `:530-534`). Assim:

   | Campo | Combinações alteradas |
   |---|---|
   | `tipo_dia` | Qualquer valor formal; não há guarda de tipo nesse ramo |
   | `cobertura_parcial` | Verdadeiro |
   | `periodo_parcial` | `dentro`, `fora`, `nao_sei`, `None`/ausente e qualquer valor diferente de `sem_periodo` |
   | `_n_missing` | >0 |
   | `chamados_vivos` | >0 |
   | `cobrancas_vivas` | Tanto 0 como >0 |

   Fonte da tabela: `app/ponto/supra_juiz.py:530-534`. Falta real não produz essa combinação: a grade apaga suas células e não carimba parcial (`app/escala/utils.py:1174-1179`); o outro produtor só carimba `tipo_dia='ausencia'`, excluindo falta (`app/escala/services/leitor_celula.py:399-412`).

4. **Não é verdadeiro que retirar esse código sempre produz `cobrado`.** Se sobraram códigos de furo ou cobrança indevida, a precedência já fazia esses vereditos vencerem: nesses casos não há mudança de veredito (`app/escala/models.py:1580-1582`). Se antes `coberta` vencia códigos de cadastro, discordância ou fato em ausência, esses códigos passam a determinar o veredito (`app/escala/models.py:1582-1599`). Se não sobra código, `derivar_veredito([])` é `concorde` (`app/escala/models.py:1593-1596`); somente o cartório injeta `cobrado` quando existe fio vivo **e protestante** (`app/ponto/services/cartorio.py:1271-1295`). Decisão administrativa continua prevalecendo (`app/ponto/portas/celula.py:655-658`).

   No caso padrão de ausência parcial, passado encerrado, previsto>0, sem fato e fio morto, **não** encontrei vazio: sai `FURO_COM_COBRANCA_MORTA` (`app/ponto/supra_juiz.py:304-316`, `:360-362`). Vazio sem `cobrado` existe formalmente com flag espúrio em tipo não cobravel, ou com turno não encerrado/previsto zero; ocorrência real desses insumos na frota `[nao verificado]`. Não vender esse caso sintético como regressão produtiva.

5. **Todos dispensados com chamado vivo:** o supra puro continua emitindo `COBRANCA_EM_DIA_COBERTO`, porque `_n_missing=0` (`app/ponto/supra_juiz.py:530-534`), e não emite furo porque `_parcial_sem_resto` o bloqueia (`:311-316`). No cartório, chamado com marcos nomeados todos dispensados não protesta: `marcos_acesos` aceita `acesa=True`, sem exigir luz (`app/ponto/services/lastro.py:170-181`), e `_protesta` o exclui (`app/ponto/services/cartorio.py:1250-1258`). Assim pode sair `concorde` diretamente e fechar pelo fluxo de marcos; chamado sem marcos nomeados permanece indeterminado nesse juiz (`app/ponto/services/lastro.py:170-172`), permitindo `coberta` e fechamento por absolvição. Não há resto a cobrar nesse contraexemplo (`app/ponto/supra_juiz.py:307-316`).

6. **Hoje/VIGIA:** o corte remove somente células `missing` pendentes (`app/ponto/services/cartorio.py:737-745`) e deixa a lâmpada correspondente `acesa=None, pendente=True` (`:748-758`). O supra recebe o dia cortado e encerrado até esse teto (`:1221-1222`, `:1264-1266`). Se todo missing remanescente estava no futuro, `_n_missing=0` permite novamente `COBRANCA_EM_DIA_COBERTO`; isso não deve matar o fio dos marcos pendentes: a guarda existe tanto na pergunta (`app/chamados/juizes.py:1115-1117`) quanto na retratação do chamado (`app/chamados/reconciliador.py:982-985`). Depois do vencimento, o missing volta ao julgamento pela alteração de teto na impressão (`app/ponto/services/cartorio.py:1192-1197`). Não demonstrei falha nova nessa costura.

Consumidores de `coberta` e efeito da mudança:

| Consumidor | Leitura e efeito |
|---|---|
| Derivação/escrita | Código nasce no supra `:532`; vira `coberta` em `app/escala/models.py:1582`; porta decide em `app/ponto/portas/celula.py:647-661` e lavra em `:745-746`. |
| Pergunta/retratação | `coberta` está fora de `VEREDITOS_ACUSAM` (`app/escala/models.py:1735-1736`): pergunta sem resposta morre pelo degrau da célula, salvo teto (`app/chamados/juizes.py:1102-1117`); chamado pode ser retratado por absolvição (`app/chamados/reconciliador.py:973-986`). `cobrado/furo/...` mantêm acusação. |
| Veto de emissão/reabertura | `coberta` veta em `app/ponto/marcos.py:119`, `:154-162`; `cobrado` não. Chamadores incluem detectar ausências `:625-626`, reconciliar grade `:502`, reconciliador `:768`. |
| Status lavrado | Sem batida: `coberta` dá ausência; parcial com `cobrado/furo` dá irregular (`app/ponto/status_dia.py:98-113`). Com batida: `coberta` dá ok, `cobrado` dá irregular (`:117-120`). |
| Calendário | Mapa `coberta→ok`, `cobrado→irregular` (`app/colaboradores/services/calendario.py:54-58`), mas o retorno antecipado citado acima continua ocultando a mudança para ausência sem batida (`:109-110`). |
| Folha/retenção | `coberta` não entra em `VEREDITOS_ACUSAM`; novo `cobrado/furo/...` retém, ressalvadas exceções de protestos não cobraveis e pausa (`app/folha/export.py:1215-1236`). Não demonstrado recálculo do FechamentoMensal por essa mudança isolada `[nao verificado]`. |
| Relatório de furos | `coberta→folga`, elimina faltantes; `cobrado→furo` mantém faltantes (`app/relatorios/furos.py:24-52`). |
| Dia da frota | `coberta` aparece como “coberto por ausência”; `cobrado` como “cobrado, aguardando resposta” (`app/relatorios/dia_da_frota.py:10-15`), e só os acusadores entram na lista de cobrança (`:54-61`). |

Executei em memória `classificar_dia` com `python3 -B`, sem Django/banco/arquivos de saída; os resultados confirmaram os ramos descritos. Execução da suíte Django e ocorrências produtivas `[nao verificado]`. [nao verificado]

## 01/11 00:54 · sessao dc02185ca894 · - · - · origem=Ronald (tmux)

Achado **bloqueia — âncora noturna ainda usa data civil errada**. O ramo novo passa a âncora de `_marcos_do_furo_parcial` ao emissor (`app/ponto/services/cartorio.py:1578-1583`), mas o auxiliar ordena por `HH:MM` e combina com a data inicial, sem encadear virada (`app/ponto/services/cartorio.py:1717-1720`). Caso concreto: turno D 19:00→D+1 07:00, parcial D 19:00→23:00, nenhuma batida, E19 dispensada e S07 missing; a cobrança ancora em **D 07:00**, 24 horas antes do marco real. Isso altera `atraso_min`, `horario_previsto` e texto do push (`app/ponto/management/commands/detectar_ausencias.py:581`, `:702`, `:776`); SLA usa `agora`/janela, portanto não atribuir à âncora mudança do SLA (`:591-597`). A autoridade correta da virada já existe em `deslocamentos_dos_marcos`, que chama `encadear_marcos` (`app/ponto/turnos.py:2644-2661`).

**Testes / mordida.** [nao verificado]

- A costura principal tem testes reais de grade→ata→supra (`app/ponto/tests/test_cobparc_marco_no_periodo.py:175-202`), julgamento→emissão→pergunta viva (`:304-318`), edição de período→impressão→rejulgamento (`:347-362`), declaração textual (`:397-404`), espelho de chamado vivo (`:426-439`) e contador em duas passadas (`:441-452`). Isso é cobertura estática observada; execução RED/GREEN e suite permanecem **[nao verificado]**.
- A lacuna que deixa passar o achado noturno é concreta: testes noturnos só chamam `_grade_pura` (`:64-80`); o teste de âncora é diurno e confere apenas `.time()`, sem data (`:416-424`). **bloqueia**, junto ao bug da âncora. [nao verificado]
- `_grade_pura` força `cobre_parte_do_dia=True` (`:49-51`), portanto esses testes não verificam a ligação ausência real→juiz de integralidade→carimbo. **fila** para cobertura dessa costura. [nao verificado]
- Sim, alguns testes passam com a cura revertida: os controles `test_MORDE_dia_inteiro_segue_coberta`, `...parcial_sem_periodo...` e `...parcial_sem_resto...` afirmam exatamente o comportamento anterior do código (`:476-487`; comparação com diff dado). Isso é adequado como controles, mas não prova a cura. O RED que a distingue exige ausência do código para parcial com resto (`:473-474`). [nao verificado]
- `LavradoNaoContaDispensadaTest` morde a subtração das dispensadas: antigo `4−2=2` viola esperado zero (`:374-383`); o controle com uma luz exige 1, também falhando com cálculo antigo (`:385-389`). Nenhuma reversão foi executada **[nao verificado]**.
- As combinações tipo `falta`, duas ausências, hoje cortado pelo vigia e âncora noturna não aparecem como integração no arquivo de teste inspecionado (`:40-487`); confirmação de execução adversarial dessas classes é **[nao verificado]**. [nao verificado]

**Escritores/leitores.**

| Valor | Escrita | Leitura |
|---|---|---|
| `REGRA_VERSOES` | declaração única em `app/ponto/services/cartorio.py:111` | validação de formato `:179-187`; `versao_para` escolhe maior versão aplicável `:203-209`; cache do julgamento `:988-991`; impressão inclui `regra:` `:380-381`, usada no julgamento `:1192-1197`; testes de declaração/alcance `app/ponto/tests/test_o219_versao_da_regra.py:77`, `:87-93` |
| âncora parcial | `_marcos_do_furo_parcial` `app/ponto/services/cartorio.py:1717-1721` | novo ramo de furo `:1578-1583`; parcial com fato `:1619-1621`; emissor escreve contexto/atraso `app/ponto/management/commands/detectar_ausencias.py:581`, `:702-703`; push `:776` |
| `out['parcial_na_lista_sem_push']` | `_contar_na_lista` `app/ponto/services/cartorio.py:1673` | incremento lê valor anterior na mesma linha; testes `app/ponto/tests/test_cobparc_marco_no_periodo.py:326`, `:451`; resumo do command não o imprime (`app/ponto/management/commands/processar_cartorio.py:66-68`) |
| `_parcial_na_lista_vistos` | `setdefault` + `.add` `app/ponto/services/cartorio.py:1670-1672` | pertinência `:1671`; limpo pelo `finally` `:922`; teste garante ausência na saída `app/ponto/tests/test_cobparc_marco_no_periodo.py:452` |
| invocação do contador | gates de emissão `app/ponto/services/cartorio.py:1564-1565`, `:1616-1617` | segunda passada reutiliza `out` em `:843`; chave `(c.pk,data)` em `:1551` |

**fila — contador sem consumidor explícito de produção encontrado**. A chave nova não nasce em `contadores` (`app/ponto/services/cartorio.py:1724-1737`), só é criada quando conta (`:1673`), e o resumo de produção omite-a (`app/ponto/management/commands/processar_cartorio.py:66-68`). A lista administrativa alegada deve ser demonstrada pelo veredito/consumidor, não pelo contador; visibilidade operacional do número é **[nao verificado]**.

**Checklist estrutural.**

- **Segundo escritor:** espelho novo reutiliza `_espelhar_marcos` (`app/ponto/services/cartorio.py:1635-1639`), que chama a porta `renovar_contexto` (`app/ponto/management/commands/detectar_ausencias.py:553-559`), cujo save é canônico (`app/chamados/services/contexto.py:96-102`). Lavra/ata continuam por `lavrar_veredito` e `carimbar` (`app/ponto/services/cartorio.py:1643`, `:1663`); não há segundo escritor persistente demonstrado nesses novos ramos.
- **Relógio:** novas operações da âncora usam `make_aware/combine`, sem consultar relógio (`app/ponto/services/cartorio.py:1568`, `:1719-1720`). `julgar_colab` recebe `hoje/agora` (`:781-782`); command lê relógio na borda (`app/ponto/management/commands/processar_cartorio.py:38-39`). Há relógios preexistentes em `julgar_celula` apesar de `agora` injetado (`app/ponto/services/cartorio.py:1783`, `:1804`, `:1807`): **fila**, caso teste injete instante diferente do host.
- **Ordem de trava:** novos ramos não introduzem aquisição explícita; contexto usa `atomic` apenas na escrita e lê instância sem `select_for_update` (`app/chamados/services/contexto.py:96-102`, documentação `:25-30`). Abertura trava Colaborador antes do dedup (`app/chamados/models.py:861-865`); fechamento existente trava chamado dentro do atomic (`:666-673`). Deadlock concreto causado pela obra: **[nao verificado]**.
- **Pré-requisito 09:** versão 3 alcança toda competência ≥09, não só cinco dias (`app/ponto/services/cartorio.py:111`, `:205-209`); hash muda para todos esses julgamentos (`:380-381`). Comentário reconhece a chegada conjunta da regra 2 à 09 (`:174-176`). O dado fornecido não contém medição nominal, hashes antes/depois, diferença financeira ou Pauta DP: **[nao verificado]**; não tratar a docstring `:169-176` como execução comprovada.
- O código protege escrita do fechamento da 09 exportada por guarda existente (`app/ponto/services/fechamento.py:36-42`, `:544-548`). Rejulgar ata da 09 não equivale automaticamente a regravar FechamentoMensal (`app/ponto/services/cartorio.py:1641-1664`). **Não há bloqueio técnico demonstrado só por a 09 estar EXPORTADA**; confirmação do pré-requisito documental do aval fica **[nao verificado]**.
- O teste da versão morde declaração e janela (`app/ponto/tests/test_o219_versao_da_regra.py:77`, `:87-93`), mas não confirma hashes de FechamentoMensal/Pauta DP; essas garantias permanecem **[nao verificado]**.

## 01/11 00:54 · sessao e754a970d942 · - · - · origem=Ronald (tmux)

**Bloqueia — a fábrica escolhe cobertura diferente da grade quando há duas ausências.** `materializacao._coberto_por_ausencia` usa `any()` sobre **todas** as ausências não rejeitadas, excluindo falta; a grade e `_celula_furada` usam a vencedora por maior `(data, pk)`. Caso: atestado integral antigo + declaração parcial com período mais recente, aprovada, com marco faltante fora do período. A grade conserva o resto cobrável; `_celula_furada` responde `True`; a fábrica retorna cobertura por causa do atestado antigo, impedindo perguntas. Fontes: `app/chamados/services/materializacao.py:74`, `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2772`, `app/ponto/turnos.py:2784`, `app/escala/utils.py:1149`, `app/escala/utils.py:1191`, `app/chamados/juizes.py:446`, `app/chamados/juizes.py:478`. A propagação ocorre em `app/chamados/services/disputa_emissao.py:763` e `app/chamados/services/pergunta_no_app.py:74`.

**Bloqueia — leitor de cobrança não migrado em c2.** `reconciliador.py` continua vetando qualquer ausência retornada por `ausencia_cobre`, inclusive a parcial com período que deixa marco apagado. Caso: pergunta órfã de um marco externo à parcial, cuja religação precisaria criar, reabrir ou costurar o chamado do dia: c2 retorna `False` pela mera existência da ausência. Fontes: `app/chamados/reconciliador.py:825`, `app/chamados/reconciliador.py:826`; a costura no chamado já vivo também passa por esse veto (`app/chamados/reconciliador.py:811`, `app/chamados/reconciliador.py:809`).

**Bloqueia — noturno com intervalo usa ordem da régua de apresentação como cadeia do turno.** Para turno E19/S02/E03/S07, a régua união escolhe a menor entrada E03 como âncora e produz `[E03,S07,E19,S02]`. `deslocamentos_dos_marcos` encadeia essa ordem e obtém 180/420/1140/1560 minutos; parcial 23–07 abrange [1380,1860], dispensando S02, mas deixando E03 e S07 como faltantes, embora ambos pertençam à madrugada D+1 dentro do período. Fontes: `app/escala/utils.py:1335`, `app/escala/utils.py:1343`, `app/escala/utils.py:1362`, `app/escala/utils.py:1805`, `app/ponto/turnos.py:2652`, `app/ponto/turnos.py:2661`, `app/escala/utils.py:345`, `app/ponto/turnos.py:2636`, `app/escala/utils.py:1209`. O teste noturno usa somente E19/S07, portanto não atravessa essa combinação da régua união com a cadeia (`app/ponto/tests/test_cobparc_marco_no_periodo.py:53`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:62`).

**Bloqueia — âncora do resto noturno perde a data da madrugada.** Mesmo no noturno simples corretamente dispensado, `_marcos_do_furo_parcial` ordena `HH:MM` e combina o primeiro horário com **D**, sem a cadeia temporal. Caso: turno D19–D+1 07, parcial D19–23 dispensa E19, sobra S07; a nova emissão ancora em D07, 24 horas cedo. Fontes: `app/ponto/services/cartorio.py:1717`, `app/ponto/services/cartorio.py:1719`, `app/ponto/services/cartorio.py:1580`, `app/ponto/services/cartorio.py:1584`. Isso soma 1.440 minutos ao atraso, grava `horario_previsto` errado e aparece no push (`app/ponto/management/commands/detectar_ausencias.py:581`, `app/ponto/management/commands/detectar_ausencias.py:702`, `app/ponto/management/commands/detectar_ausencias.py:776`). **O SLA nessa porta usa `agora`, não a âncora** (`app/ponto/management/commands/detectar_ausencias.py:593`, `app/ponto/management/commands/detectar_ausencias.py:597`).

**Fila — falta expõe limite explícito da definição declarada de parcial.** `ausencia_explica_o_dia_inteiro` retorna `False` para qualquer falta, independentemente de minutos; contudo a grade trata falta em ramo próprio, apaga células e órfãs, não grava cobertura parcial, e o supra exclui falta de `dia_de_cobranca`. Logo “juiz retorna False” não equivale universalmente a “grade grava parcial e cobra resto”. Fontes: `app/ponto/turnos.py:2690`, `app/escala/utils.py:1174`, `app/escala/utils.py:1178`, `app/ponto/supra_juiz.py:302`, `app/ponto/supra_juiz.py:304`. O teste preserva deliberadamente `dia_sem_trabalho=True` para falta mesmo com carimbo parcial (`app/chamados/tests/test_emis_ausencia_parcial_cobra.py:50`). O catálogo padrão declara falta não lançável por minutos e a porta valida pelo catálogo vivo; existência de falta parcial concreta em produção: `[nao verificado]` (`app/ponto/catalogo/ausencias.py:88`, `app/ponto/services/ausencia.py:206`).

**Sem defeito no argumento vazio de período.** Com ambos os horários declarados, `periodo_da_parcial_no_dia(a, ())` retorna `fora`; sem um deles, retorna `sem_periodo`, independentemente das batidas. Portanto a comparação exclusiva com `'sem_periodo'` detecta ausência de cadastro e não muda por esvaziar o segundo argumento (`app/ponto/turnos.py:2632`, `app/ponto/turnos.py:2638`, `app/ponto/turnos.py:2641`). Ela não resolve o defeito do `any()` sobre ausências perdedoras (`app/chamados/services/materializacao.py:74`).

| Sítio | Predicado efetivamente lido |
|---|---|
| `_coberto_por_ausencia` | Qualquer ausência não falta, não rejeitada, que explique dia inteiro **ou** esteja sem período (`app/chamados/services/materializacao.py:74`). |
| `disputa_emissao` | Mesmo retorno, por importação e guarda antes de `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:8`, `app/chamados/services/disputa_emissao.py:763`, `app/chamados/services/disputa_emissao.py:770`). |
| `pergunta_no_app.retencao` | Mesmo retorno, para causa `coberto_por_ausencia` (`app/chamados/services/pergunta_no_app.py:60`, `app/chamados/services/pergunta_no_app.py:74`). |
| `dia_sem_trabalho` | Ata parcial de tipo ausência só cala se `trabalha=False`; outros tipos preenchidos diferentes de trabalho calam (`app/chamados/juizes.py:408`, `app/chamados/juizes.py:424`, `app/chamados/juizes.py:426`). |
| `_celula_furada` | Primeiro: ausência **vencedora** integral cala; depois lê ata, dia sem trabalho, lâmpadas apagadas e órfãs (`app/chamados/juizes.py:446`, `app/chamados/juizes.py:464`, `app/chamados/juizes.py:478`, `app/chamados/juizes.py:480`). |
| c2 | Qualquer ausência vencedora cala, sem ler integralidade ou período (`app/chamados/reconciliador.py:825`). |

**Diferença intencional a preservar:** parcial sem período e com lâmpada apagada pode dar `dia_sem_trabalho=False`, `_celula_furada=True`, mas `_coberto_por_ausencia=True`, porque a fábrica bloqueia a pergunta ao colaborador e remete o problema ao cadastro; não são respostas à mesma pergunta operacional (`app/chamados/juizes.py:424`, `app/chamados/juizes.py:478`, `app/chamados/services/materializacao.py:69`, `app/chamados/services/materializacao.py:74`).

**Outro leitor residual, de metadado:** `via_pelo_fato` ainda chama qualquer ausência aprovada de `VIA_AUSENCIA_DIA`, inclusive parcial com resto; esse valor significa “dia resolvido pela ausência”. Isso pode rotular erroneamente a testemunha de uma resolução; morte automática da pergunta por esse caminho, isoladamente, não demonstrada, porque o juiz de pergunta prioriza o veredito acusador da célula (`app/chamados/juizes.py:1381`, `app/chamados/juizes.py:1383`, `app/chamados/models.py:2261`, `app/chamados/juizes.py:1102`, `app/chamados/juizes.py:1104`). Gravidade: **fila**.

## 01/11 00:47 · sessao 8ff112be9bdd · FASE-CONFLITANTE-VINCULO-DO-DIA · censo · origem=Code

**O contador e a autoridade usam universos diferentes:** `medir` seleciona vínculos por `ativa=True` e os percorre pela janela inteira; a autoridade considera a geradora da célula e a vigência. Isso demonstra diferença de seleção, mas ocorrência e impacto no contador de produção são **[nao verificado]**. Fontes: [fase_conflitante.py:71](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:71), [alimentacao.py:237](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:237).

1. **Sítios em `fase_conflitante.py`**

   Há **uma seleção de universo de vínculos**, sem uma escolha individual de “vínculo do dia”:

   | Sítio | Decisão e fonte |
   |---|---|
   | [fase_conflitante.py:71](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:71) | Query direta de `EscalaColaborador`: inclui somente `ativa=True`. Não filtra colaborador, início ou fim. |
   | [fase_conflitante.py:73](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:73), [fase_conflitante.py:78](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:78) | Propagação dessa seleção: cada `ec` é usado para **todos** os dias da janela, sem testar se cobre aquele dia e sem consultar célula ou autoridade. |

   Os demais pontos não escolhem vínculo: a foto vem de query de **todas as `FolgaDia` daquele `ec`**, sem recorte de datas; `_ancora_diz` lê os campos `data_ancora_colaborador` e `tipo_escala.tipo_ciclo`; `_foto_diz` recebe o conjunto de datas. A ausência de foto elimina o vínculo da medição, não declara ausência de vínculo no dia. Fontes: [fase_conflitante.py:74](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:74), [fase_conflitante.py:40](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:40), [fase_conflitante.py:51](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:51).

2. **Autoridade registrada e predicado**

   É `escala/alimentacao.py::vinculo_do_dia`, definida em [alimentacao.py:196](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:196), registrada em [juizes.py:1270](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/core/juizes.py:1270) e indicada em [JUIZES.md:85](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/docs/JUIZES.md:85).

   - **Com geradora:** lê `escala_geradora_id` da célula e procura esse vínculo na lista recebida. Se faltar ou estiver fora da vigência, retorna `None`, sem escolher outro. As bordas são inclusivas; nesse ramo não exige vínculo ativo. Fonte: [alimentacao.py:237](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:237).
   - **Sem geradora:** usa `vinculo_cobre_a_data`: início não nulo, `início <= dia`, fim nulo ou `fim >= dia`, e `ativa or data_fim is not None`. Entre os elegíveis, escolhe o máximo por `(ativa, data_inicio)`. Fontes: [alimentacao.py:164](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:164), [alimentacao.py:182](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:182), [alimentacao.py:253](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:253).

   Portanto, encerrado pode responder dentro da vigência; futuro não responde antes do início; inativo sem fim é recusado no fallback, embora possa responder como geradora dentro dos limites declarados. Fontes: [alimentacao.py:182](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:182), [alimentacao.py:247](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:247).

3. **Comparação e casos de divergência**

   **Não são iguais.** O único seletor do contador exige atividade atual; sua aplicação por dia não verifica vigência nem geradora. A autoridade verifica esses elementos e retorna um vínculo ou `None`. Fontes: [fase_conflitante.py:71](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:71), [fase_conflitante.py:78](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:78), [alimentacao.py:237](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:237).

   Há uma contraprova **já escrita como fixture**, do colaborador `E4 C`: vínculo encerrado inativo de **01/09/2026 a 30/09/2026**, ativo desde **01/10/2026**, futuro ativo desde **01/11/2026**, além de inativo sem fim desde agosto. Fonte: [test_e4_vinculo_cobre_a_data.py:24](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_e4_vinculo_cobre_a_data.py:24).

   | Dia da fixture | Autoridade | Universo da query do contador |
   |---|---|---|
   | 15/09/2026 | Escolhe o encerrado de setembro. | Exclui o encerrado e inclui os dois ativos, ainda futuros nesse dia. |
   | 10/10/2026 | Escolhe o ativo iniciado em outubro. | Inclui também o futuro iniciado em novembro. |

   As respostas da autoridade estão afirmadas em [test_e4_vinculo_cobre_a_data.py:54](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_e4_vinculo_cobre_a_data.py:54); a seleção diferente decorre de [fase_conflitante.py:71](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:71).

   Isso comprova **diferença de universo**, não contagem indevida: essa fixture usa 5x2, que `_ancora_diz` não responde. Fontes: [test_e4_vinculo_cobre_a_data.py:22](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_e4_vinculo_cobre_a_data.py:22), [fase_conflitante.py:43](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:43). Caso concreto de produção com colaborador, datas e dia: **[nao verificado]**.

4. **Chamadores, leitores e efeitos**

   | Caminho | Efeito confirmado |
   |---|---|
   | Command → `medir` | Chama em [lavrar_fase_conflitante.py:31](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/management/commands/lavrar_fase_conflitante.py:31). Sem `--apply`, imprime; com a flag, escreve `MetricaSnapshot` com total, quantidade de vínculos e detalhe ([linha 32](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/management/commands/lavrar_fase_conflitante.py:32)). |
   | Cron → command | Declarado às 07:14, com `--apply`, dependente de `reconciliar_vinculo` ([crons.py:479](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/config/crons.py:479)). Instalação/execução em produção: **[nao verificado]**. |
   | `ultimo_lavrado` → snapshot | Lê o último snapshot; sem medida retorna `total=None` e `sem_medida=True`; com medida acrescenta `medido_em` ([fase_conflitante.py:105](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:105)). |
   | API → `ultimo_lavrado` | Entrega o resultado em `contadores.fase_conflitante`, como JSON ([views_mensageria.py:632](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/api/views_mensageria.py:632), [linha 643](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/api/views_mensageria.py:643)). |
   | Mensageria → API | O cliente recebe o JSON inteiro; `saude_da_operacao` o cacheia, mas processa somente `metricas` e devolve apenas essas medidas. **Esse caminho não repassa o contador ao contexto do chat.** Fontes: [core_client.py:108](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/mensageria/nucleo/core_client.py:108), [ferramentas.py:259](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/mensageria/nucleo/ferramentas.py:259), [linha 294](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/mensageria/nucleo/ferramentas.py:294), [linha 380](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/mensageria/nucleo/ferramentas.py:380). |

   Nesses caminhos, as escritas identificadas são **snapshot e cache**. Emissão de chamado, alteração de estado operacional ou retenção de pouso causada por esse contador: **[nao verificado]**. [nao verificado]

5. **Selos e testes existentes** [nao verificado]

   | Referência | O que afirma |
   |---|---|
   | [test_fase_conflitante.py:60](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:60) | Foto nos pares + âncora 20/07: um vínculo acusado, total **≥25**, esperado zero e vínculo na amostra. |
   | [test_fase_conflitante.py:72](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:72) | Mesma foto, âncora 21/07: total zero. |
   | [test_fase_conflitante.py:80](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:80) | Sem foto: zero. |
   | [test_fase_conflitante.py:84](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:84) | Âncora `None`: zero. |
   | [test_fase_conflitante.py:88](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:88) | Ciclo 6x1: zero. |
   | [test_fase_conflitante.py:93](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:93) | Foto diz folga no dia fotografado e trabalho no seguinte. |
   | [test_semselo5_ativo_nao_responde_por_dia.py:128](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:128) | Classifica `medir` em `HOJE`, como cadastro; a catraca cobra ausência de sítios novos/listas velhas, não equivalência com a autoridade ([linha 365](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:365)). |
   | [test_api_mensageria_saude.py:106](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/api/tests/test_api_mensageria_saude.py:106) | Cobra nominalmente `fase_conflitante`: sem lavra, `sem_medida=True` e `total=None`. |
   | [test_api_mensageria_saude.py:79](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/api/tests/test_api_mensageria_saude.py:79) | GET não altera a quantidade de snapshots. |

   Os seis testes do contador usam vínculo ativo iniciado em 01/01/2026 e janela de setembro; não testam troca, encerramento, vínculo futuro ou geradora. Fontes: [test_fase_conflitante.py:30](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:30), [linha 49](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:49), [linha 60](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:60). Resultado de execução dos testes nesta auditoria: **[nao verificado]**.

## 01/11 00:47 · sessao 709103adf3b6 · - · - · origem=Ronald (tmux)

A autoridade registrada é `escala/alimentacao.py::vinculo_do_dia`, definida em `app/escala/alimentacao.py:196`, registrada em `app/core/juizes.py:1262-1271` e documentada em `app/docs/JUIZES.md:84-89`.

- Com geradora: lê `CelulaDia.escala_geradora_id`, procura esse vínculo na lista recebida e devolve `None` se faltar ou se o dia estiver fora da vigência; limites inclusivos. Nesse ramo não exige `ativa=True`, nem exige `data_fim` para vínculo inativo — `app/escala/alimentacao.py:237-252`.
- Sem geradora: aceita início não nulo e `início <= dia <= fim`, sendo fim nulo ilimitado, e exige `ativa or data_fim is not None`; desempata por `(ativa, data_inicio)` — `app/escala/alimentacao.py:164`, `app/escala/alimentacao.py:182-184`, `app/escala/alimentacao.py:253-254`.
- Vínculo futuro não responde antes do início; encerrado responde dentro da vigência; inativo sem fim não responde no fallback — `app/escala/alimentacao.py:182-184`, `app/escala/alimentacao.py:250-254`.

Em `fase_conflitante`, há um sítio de seleção do universo: query direta `EscalaColaborador.objects.filter(ativa=True)`; cada vínculo retornado é aplicado a todos os dias da janela, sem verificar início, fim, célula ou juiz — `app/escala/services/fase_conflitante.py:71-80`. `_ancora_diz` lê âncora/ciclo; `_foto_diz` lê datas de folga; nenhum seleciona vínculo — `app/escala/services/fase_conflitante.py:40-58`.

Casos já presentes na cópia, como **fixtures**, não produção:

| Fixture | Datas/dia | Diferença de seleção |
|---|---|---|
| `E4 C` | Encerrada inativa 01/09..30/09; ativa desde 01/10; futura ativa desde 01/11; dia 15/09 | Juiz escolhe encerrada; filtro de fase inclui as duas ativas e exclui encerrada — `app/escala/tests/test_e4_vinculo_cobre_a_data.py:24-29`, `app/escala/tests/test_e4_vinculo_cobre_a_data.py:54`, `app/escala/services/fase_conflitante.py:71`. |
| `E4 C` | Mesmos vínculos; dia 10/10 | Juiz escolhe ativa desde 01/10; filtro inclui também futura desde 01/11 — `app/escala/tests/test_e4_vinculo_cobre_a_data.py:14`, `app/escala/tests/test_e4_vinculo_cobre_a_data.py:26-29`, `app/escala/tests/test_e4_vinculo_cobre_a_data.py:55`, `app/escala/services/fase_conflitante.py:71`. |
| `Colab EV` | Dia 30/09; velho inativo desde 31/08 até 05/10; novo ativo desde 30/09; célula nomeia velho | Juiz devolve velho; filtro de fase exclui velho e inclui novo — `app/escala/tests/test_escala_vigente_pergunta_a_celula.py:45-55`, `app/escala/tests/test_escala_vigente_pergunta_a_celula.py:88-96`, `app/escala/services/fase_conflitante.py:71`. |

Essas fixtures usam ciclos 5x2 e 6x1, respectivamente; provam diferença de universo pela leitura do código, não um total indevido do contador — `app/escala/tests/test_e4_vinculo_cobre_a_data.py:22-23`, `app/escala/tests/test_escala_vigente_pergunta_a_celula.py:40-42`, `app/escala/services/fase_conflitante.py:43-48`.

Caso de produção com colaborador, datas de vínculo e dia: **[nao verificado]**; o anexo recebido contém somente `JUIZES.md`, sem registros de colaboradores **[nao verificado]**.

## 01/11 00:46 · sessao 2e1e842e5343 · - · - · origem=Ronald (tmux)

**Chamadores e leitores**

- `medir` é importada e chamada pelo command em `app/escala/management/commands/lavrar_fase_conflitante.py:14` e `:31`. Sem `--apply`, só imprime e retorna (`:32-34`); com `--apply`, grava/atualiza exclusivamente o snapshot mostrado no trecho, usando `total` como numerador, `vinculos` como denominador e o dict como detalhe (`:35-38`); depois imprime (`:39-41`).
- O cron está **declarado** para 07:14, `--apply`, dependente de `reconciliar_vinculo`: `app/config/crons.py:479-480`. Instalação/execução em produção: **[nao verificado]**.
- `ultimo_lavrado` busca o último `MetricaSnapshot` de `chave=CHAVE`, `escopo='frota'`, por `-data_ref,-calculado_em`; sem snapshot, retorna `total=None, esperado=0, sem_medida=True`; com snapshot, copia detalhe e acrescenta `medido_em`: `app/escala/services/fase_conflitante.py:105-114`.
- A API importa essa leitora (`app/api/views_mensageria.py:632`), a põe em `saida['contadores']['fase_conflitante']` (`:643`) e devolve JSON (`:647`); rota em `app/api/urls.py:16`.
- `mensageria/nucleo/core_client.py:108-120` faz GET de `/api/mensageria/saude/` e devolve o JSON inteiro. Entretanto `saude_da_operacao` cacheia esse retorno (`mensageria/nucleo/ferramentas.py:259-262`), percorre **somente `dados['metricas']`** (`:265`) e retorna só `{'saude_da_operacao': medidas}` (`:294`). Portanto, nesse caminho, `contadores.fase_conflitante` **não chega ao contexto do chat**, que recebe `saude_da_operacao` em `:380`.
- Nos caminhos acima, a escrita ligada ao contador é snapshot (`app/escala/management/commands/lavrar_fase_conflitante.py:35-38`) e cache do payload (`mensageria/nucleo/ferramentas.py:262`); a API só devolve o contador (`app/api/views_mensageria.py:643-647`). Chamado emitido, mudança de estado operacional ou pouso retido **por esse contador**: **[nao verificado]**.

**Testes/selos** [nao verificado]

| Teste | O que efetivamente afirma | [nao verificado]
|---|---|
| `app/escala/tests/test_fase_conflitante.py:60` | Foto pares + âncora 20/07 gera 1 vínculo acusado, total ≥25, `esperado=0` e amostra com o vínculo (`:64-70`). |
| `app/escala/tests/test_fase_conflitante.py:72` | Mesma foto, âncora 21/07: total zero (`:74-78`). |
| `app/escala/tests/test_fase_conflitante.py:80` | Sem foto: total zero (`:81-82`). |
| `app/escala/tests/test_fase_conflitante.py:84` | Âncora `None`: total zero (`:85-86`). |
| `app/escala/tests/test_fase_conflitante.py:88` | Ciclo 6x1: total zero (`:90-91`). |
| `app/escala/tests/test_fase_conflitante.py:93` | `_foto_diz` devolve `False` no dia fotografado e `True` no seguinte (`:95-97`). |
| `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:128` | Declara `medir` em **HOJE**, motivo “cadastro: vinculos ativos em fase conflitante” (`:128-129`); o teste cobra ausência de sítios novos/listas velhas (`:365-371`) e motivos/listas disjuntas (`:373-377`). |
| `app/api/tests/test_api_mensageria_saude.py:106` | Inclui nominalmente `fase_conflitante`: sem lavra, `sem_medida=True` e `total=None` (`:108-112`). |
| `app/api/tests/test_api_mensageria_saude.py:79` | GET da API não altera a quantidade de snapshots (`:80-83`). |

Os seis testes funcionais usam fixture com `ativa=True`, início fixo em 01/01/2026 (`app/escala/tests/test_fase_conflitante.py:49-51`) e janela de setembro (`:30-31`); os corpos acima não verificam troca de vínculo, vínculo encerrado, futuro ou geradora de célula (`:60-97`). O teste da API que injeta snapshot e confere conteúdo é de **`previsto_cego`**, não de `fase_conflitante` (`app/api/tests/test_api_mensageria_saude.py:114-124`). Resultado de execução destes testes: **[nao verificado]**.

_1334 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
