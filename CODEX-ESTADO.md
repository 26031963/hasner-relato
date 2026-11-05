# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 15:23** · publicado em **05/11/2026 15:23** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 15:23 · sessao 99174368c0c4 · REVISAO-JUIZ-4A · verificador · origem=Code

**ACHADO: o lote pode declarar zero para um limbo de ontem que o vigia de hora sequer seleciona.** O lote delega pela janela temporal; o vigia exige também `trabalha=True`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:110`; `app/ponto/services/vigia_de_hora.py:21`)

Ocorrências reais, quantidade de atingidos, resultados de testes e impacto monetário em produção: **[não verificado]**. Os cadastros abaixo são contraexemplos construídos, não registros produtivos. [nao verificado]

**Item 1 — quem escreve e quem lê**

| Campo/valor relevante ao diff | Quem escreve | Quem lê e qual efeito |
|---|---|---|
| `CelulaDia.data`, `trabalha`, `dna`, `escala_geradora` | Geração e regeneração da célula. (`app/ponto/portas/celula.py:247`; `app/ponto/portas/celula.py:260`) | O lote seleciona datas; a fábrica interpreta trabalho/limbo; o teto resolve os marcos pelo vínculo do dia. (`app/chamados/management/commands/disparar_perguntas_competencia.py:102`; `app/chamados/services/fabrica_da_celula.py:91`; `app/ponto/services/cartorio.py:841`) |
| `ata`, `veredito`, `julgada_em` | Porta de lavra e carimbo. (`app/ponto/portas/celula.py:768`; `app/ponto/portas/celula.py:770`; `app/ponto/portas/celula.py:800`) | A fábrica verifica dia, veredito acusador e parcial; o fio mudo passa a consultar a parcial. (`app/chamados/services/fabrica_da_celula.py:91`; `app/chamados/services/fabrica_da_celula.py:99`; `app/chamados/services/fabrica_da_celula.py:102`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`) |
| `cobertura_parcial`, `periodo_parcial` | Grade escreve; cartório transporta para a ata. (`app/escala/utils.py:1251`; `app/escala/utils.py:1255`; `app/ponto/services/cartorio.py:732`) | `parcial_so_lista`: sem período sempre cala; com período considera julgamento anterior e fato novo. (`app/ponto/services/cartorio.py:396`) |
| Motivos, disputa, pergunta e identidade `(colaborador, dia, motivo)` | `perguntar` chama criação/agregação da disputa e geração das perguntas; `PerguntaDisputa.abrir` deduplica e persiste. (`app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/services/disputa_emissao.py:389`; `app/chamados/models.py:2420`; `app/chamados/models.py:2465`) | Fábrica deduplica perguntas existentes; fio mudo consulta pergunta viva e existência do fato. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/management/commands/disparar_perguntas_competencia.py:148`; `app/chamados/management/commands/disparar_perguntas_competencia.py:211`) |
| Perguntas do caminho da célula | Julgamento chama `perguntar_pela_celula`; o lote passa a apenas contar. (`app/ponto/services/cartorio.py:997`; `app/chamados/management/commands/disparar_perguntas_competencia.py:112`) | `motivos_que_o_evento_deixou` consulta a regra da fábrica e o destino do limbo. (`app/chamados/services/fabrica_da_celula.py:199`) |
| Perguntas do fio mudo | O lote continua chamando `perguntar` dentro da transação de aplicação. (`app/chamados/management/commands/disparar_perguntas_competencia.py:280`; `app/chamados/management/commands/disparar_perguntas_competencia.py:287`) | Seleção lê chamados vivos, célula, perguntas resolvedoras, situação e dedup. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/management/commands/disparar_perguntas_competencia.py:148`; `app/chamados/management/commands/disparar_perguntas_competencia.py:169`) |
| Pauta: âncora, departamento, marcador e `[motivo:…]` | `pauta_dp_do_limbo_exportado` chama a porta `escrever`, que cria `Pauta`. (`app/chamados/services/fabrica_da_celula.py:236`; `app/pautas/services.py:180`) | Escritor e novo vigia compartilham a busca das cabeças de pauta; feita ou cancelada também satisfaz a guarda. A interface lista por destinatário e estado. (`app/chamados/services/fabrica_da_celula.py:208`; `app/chamados/services/fabrica_da_celula.py:219`; `app/pautas/services.py:322`) |
| Contador `FABRICA-POR-EVENTO`, alvos e retorno 2 | O comando imprime os contadores e levanta `SystemExit(2)`; na aplicação, depois do commit do fio mudo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:238`; `app/chamados/management/commands/disparar_perguntas_competencia.py:249`; `app/chamados/management/commands/disparar_perguntas_competencia.py:300`) | O placar raspa o prefixo do contador; a configuração executa `--comp-corrente --apply` e registra em `fio_mudo.log`. (`bin/placar_code.sh:381`; `app/config/crons.py:1464`) |

**Item 2 — contraexemplo concreto pela regra**

Cadastro construído **col01**: ativo, não isento, admitido antes de outubro, sem demissão; vínculo cobrindo **20/10/2026**, turno **22h–06h**, célula desse dia com `trabalha=None`, DNA desse turno, ata `tipo_dia='trabalho'`, entrada acesa, saída apagada, veredito acusador, sem parcial, pergunta ou chamado. Existência desse cadastro em produção: **[não verificado]**.

Executar `--comp-corrente --alarme` em **21/10/2026 às 06h35 −03:00**:

- A célula entra no intervalo consultado, mas `_ontem_em_janela` retorna verdadeiro: o fim foi às 06h e ainda não passou uma hora. O lote pula a célula. (`app/chamados/management/commands/disparar_perguntas_competencia.py:101`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110`; `app/ponto/services/vigia_de_hora.py:55`)
- O vigia de hora também a exclui, porque sua consulta exige `trabalha=True`. (`app/ponto/services/vigia_de_hora.py:21`)
- Para esse universo isolado, o lote imprime déficit zero e retorna sem alarme. (`app/chamados/management/commands/disparar_perguntas_competencia.py:240`; `app/chamados/management/commands/disparar_perguntas_competencia.py:251`)
- Pela regra, limbo com ata de trabalho cobra; o marco das 06h venceu às 06h30. A regra compartilhada da fábrica produziria o motivo faltante para contagem. (`app/docs/JUIZES.md:55`; `app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:846`; `app/chamados/services/fabrica_da_celula.py:126`)

A divergência é **falso zero nessa rodada**, não perda definitiva demonstrada: o piso de anteontem permite alcançar a célula na rodada seguinte. (`app/chamados/management/commands/disparar_perguntas_competencia.py:101`)

**Item 3 — regra alterada sem lastro identificado nos anexos**

O diff acrescenta três recortes: excluir hoje, excluir ontem durante `_ontem_em_janela` e ampliar o piso para anteontem em `--comp-corrente`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:101`; `app/chamados/management/commands/disparar_perguntas_competencia.py:110`)

L-060 e L-101 fundamentam papel e autoridade do cron; a exceção do fio mudo fundamenta sua permanência como escritor. Essas disposições não estabelecem aqueles recortes temporais. Lastro específico nos anexos: **[não verificado]**. (`app/docs/LEIS.md:94`; `app/docs/LEIS.md:127`; `app/docs/CORTES.md:305`)

A ampliação problemática demonstrada é delegar também o **limbo**, embora o serviço destinatário selecione apenas trabalho explícito. (`app/chamados/management/commands/disparar_perguntas_competencia.py:110`; `app/ponto/services/vigia_de_hora.py:21`)

**Item 4 — selo que não morde**

`test_01_MORDE_ata_acusa_e_lei_cala_a_pergunta_nasce` passou a conferir somente a mensagem `o julgamento deixou 1 pergunta(s)`; seu helper executa sem `--apply` e sem `--alarme`. (`app/chamados/tests/test_fabrica_pela_ata.py:32`; `app/chamados/tests/test_fabrica_pela_ata.py:39`)

O primeiro pai já imprimia essa mensagem antes do dry-run com rollback. Portanto, esse teste passaria com a mudança escritor→vigia revertida; ele não distingue os dois comportamentos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:266`, **1º pai conforme o patch anexo**; `app/chamados/tests/test_fabrica_pela_ata.py:42`) Execução dessa reversão: **[não verificado]**.

Isso não torna a suíte inteira inócua: o teste novo de aplicação exige zero perguntas gravadas, contador um e retorno 2. (`app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:98`)

**Item 5 — cadastro de hoje decidindo passado**

| Campo | Escritor | Leitura do passado e limite da conclusão |
|---|---|---|
| `situacao` | Desligamento e reativação. (`app/colaboradores/services/desligamento.py:104`; `app/colaboradores/services/desligamento.py:210`) | Situação atual cala perguntas de qualquer data na fábrica e no fio mudo. O próprio caminho declara desligado como domínio do RH; não é, isoladamente, erro histórico demonstrado. (`app/chamados/services/fabrica_da_celula.py:91`; `app/chamados/management/commands/disparar_perguntas_competencia.py:127`; `app/chamados/management/commands/disparar_perguntas_competencia.py:169`) |
| `data_demissao` | Desligamento grava; reativação limpa. (`app/colaboradores/services/desligamento.py:105`; `app/colaboradores/services/desligamento.py:211`) | O leitor compara o dia antigo com a fronteira atualmente cadastrada e pode classificá-lo `fora_vinculo`. A fábrica chega a esse leitor por `_ata_do_dia` e `_marcos_faltantes`. (`app/escala/services/leitor_celula.py:351`; `app/escala/services/leitor_celula.py:366`; `app/chamados/services/fabrica_da_celula.py:49`; `app/chamados/services/disputa_emissao.py:827`) |
| `posto` | Transferência altera o posto e, junto, a empresa. (`app/colaboradores/views.py:1385`) | Leitura direta do posto pelas funções alteradas: **[não verificado]**. O efeito comprovado nessa cadeia é a troca simultânea de empresa. (`app/colaboradores/views.py:1386`) |
| `empresa` | Transferência grava a empresa do novo posto. (`app/colaboradores/views.py:1386`) | `limbo_vai_a_pauta` passa a empresa **atual** ao juiz de entrega; ele calcula janela e consulta exportação por essa empresa, mesmo para data antiga. (`app/chamados/services/fabrica_da_celula.py:188`; `app/chamados/services/vigia_fio.py:807`) |
| `regime_trabalhista` | Formulário da empresa, pela porta de atualização. (`app/colaboradores/views.py:512`; `app/colaboradores/views.py:523`; `app/colaboradores/services/estrutura.py:17`) | A régua financeira lê o regime atual da empresa. Participação dessa leitura na seleção dos motivos alterada pelo diff: **[não verificado]**. (`app/core/regua_cct.py:352`) |
| `isento_ponto` | Tela Passe Livre grava a flag. (`app/colaboradores/views.py:176`) | Flag atual exclui ontem do vigia e cala marcos antigos no leitor usado pela fábrica. A autoridade de isenção declara expressamente essa flag como fonte; exigência de vigência histórica nos anexos: **[não verificado]**. (`app/ponto/services/vigia_de_hora.py:117`; `app/escala/services/leitor_celula.py:349`; `app/chamados/services/declaracao_texto.py:83`; `app/core/constants.py:13`) |
| Vínculo | Porta atualiza ou cria `EscalaColaborador`. (`app/colaboradores/services/vinculo.py:265`; `app/colaboradores/services/vinculo.py:278`) | O teto consulta a geradora e a vigência **do dia**. Vínculo inativo hoje pode responder pelo passado; sem geradora, `ativa` participa do desempate entre vínculos que cobrem a data. Não há escolha automática do vínculo atual. (`app/ponto/services/cartorio.py:841`; `app/escala/alimentacao.py:269`; `app/escala/alimentacao.py:282`; `app/escala/alimentacao.py:285`; `app/docs/JUIZES.md:98`) |

Exemplo construído da dependência de empresa: célula limbo de **15/10**, empresa A entregue; transferência em **01/11** para B não entregue. Preservada a célula, a consulta passa a usar B e pode mudar **Pauta DP → pergunta**. A mudança decorre da transferência e da consulta atual. (`app/colaboradores/views.py:1386`; `app/chamados/services/fabrica_da_celula.py:188`; `app/chamados/services/fabrica_da_celula.py:171`) Ocorrência produtiva e regra específica exigindo a empresa histórica nesse caso: **[não verificado]**.

VEREDITO: **ACHADO**

1. `app/chamados/management/commands/disparar_perguntas_competencia.py:110` — itens **2–3**: falso zero para limbo noturno delegado a vigia que o exclui; **toca dinheiro: não**, efeito demonstrado no contador/alarme.
2. `app/chamados/tests/test_fabrica_pela_ata.py:42` — item **4**: selo passaria com a mudança escritor→vigia revertida; **toca dinheiro: não**.
3. `app/chamados/services/fabrica_da_celula.py:188` — itens **1 e 5**: empresa atual decide destino de dia passado; infração temporal específica **[não verificado]**; **toca dinheiro: não**, efeito demonstrado no destino pauta/pergunta.

## 05/11 15:23 · sessao 0f4eec568928 · - · - · origem=Ronald (tmux)

Item 5 — leituras encontradas:

| Campo | Escritor | Leitor sobre dia passado | Avaliação |
|---|---|---|---|
| `situacao` | `app/colaboradores/services/desligamento.py:104`; reativação `:210` | `app/chamados/services/fabrica_da_celula.py:91`; `app/chamados/management/commands/disparar_perguntas_competencia.py:169` | Usa situação atual; desligado fora da cobrança tem lastro I5a em `app/chamados/tests/test_selo_disparo_nao_cobra_desligado.py:26`. Não tratar a temporalidade isolada como violação. |
| `data_demissao` | `app/colaboradores/services/desligamento.py:105`; reativação apaga em `:211` | `app/escala/services/leitor_celula.py:351` e `:366` | Cadastro atual pode tornar dia passado `fora_vinculo`, com exceção para fato lavrado descrita em `:364`. |
| `isento_ponto` | `app/colaboradores/views.py:176` pela porta `app/colaboradores/services/colaborador.py:47` | `app/core/constants.py:18` → `app/escala/services/leitor_celula.py:349` → `app/chamados/services/declaracao_texto.py:83` | Flag atual faz leitura passada devolver ausência de marcos cobrados; regra atual da isenção é explícita em `app/escala/services/leitor_celula.py:188`. Proibição de retroatividade dessa flag nos anexos: [nao verificado]. |
| `empresa` | edição `app/colaboradores/views.py:1313`; transferência `:1386` | `app/chamados/services/fabrica_da_celula.py:188` → `app/chamados/services/vigia_fio.py:807` | Empresa atual decide se competência passada foi entregue e, portanto, pergunta versus Pauta DP (`app/chamados/services/fabrica_da_celula.py:171`). Exigência explícita de empresa histórica nos anexos: [nao verificado]. |
| `posto` | transferência `app/colaboradores/views.py:1385` | Dropdown genérico usa empresa atual em `app/chamados/services/disputa_emissao.py:304`; execução desse ramo para motivos desta fábrica: [nao verificado]. | Motivos de marco produzidos nesta rota são os quatro mapeados em `app/chamados/services/declaracao_texto.py:42`. |
| `regime` | cadastro de empresa tratado em `app/colaboradores/views.py:513` | Leitura de regime alcançada pelo decisor desta fábrica: [nao verificado]. | Não há demonstração suficiente para achado desse campo. [nao verificado] |
| vínculo | escolha pelo dia em `app/escala/alimentacao.py:269` | `app/ponto/services/cartorio.py:841` → `app/escala/servico_jornada.py:55` | Lê geradora da célula e vigência; marcos vêm do DNA do dia em `app/escala/models.py:1124`. Não demonstrado uso exclusivo do vínculo atual. |

Contraexemplo sintético de temporalidade: célula de 28/10/2026 com ata acusando entrada ausente, trabalhador ativo e sem pergunta; em 05/11, alterar somente `isento_ponto` para `True` faz `_marcos_faltantes` devolver `[]`, conservando a ata anterior (`app/chamados/services/declaracao_texto.py:82`; `:83`). Existência desse cadastro em produção e violação de regra temporal específica: [nao verificado].

Não há cadastro de produção raspado no material recebido; as datas e colaboradores citados em comentários/testes da cópia não provam o estado atual de produção. [nao verificado] [nao verificado]

## 05/11 15:22 · sessao bb9e73f73598 · F3-BECO-1 · verificador · origem=Code

**Parecer: bloqueia**, pelas falhas de atomicidade, concorrência e revalidação abaixo (`app/escala/folgas.py:169`; `app/escala/folgas.py:301`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`). Execução dos testes, RED anterior à cura, efeitos em produção e identidade Git da cópia: **[não verificado]**.

1. **Bloqueia — a foto do calendário não garante snapshot, escrita e re-julgamento na mesma transação.**

   `gerar_foto_folgas` tira o snapshot, apaga a foto anterior, faz `bulk_create` e chama a re-lavra sem abrir `atomic` próprio (`app/escala/folgas.py:169`; `app/escala/folgas.py:170`; `app/escala/folgas.py:174`; `app/escala/folgas.py:193`). Existem chamadores sem transação envolvente nesses trechos: a view e o comando de lavra (`app/colaboradores/views.py:238`; `app/colaboradores/views.py:245`; `app/escala/management/commands/lavrar_foto_folgas.py:162`).

   Contraexemplo inferido: gravar a foto e falhar antes da re-lavra deixa o mês corrigido com julgamento antigo; a próxima tentativa encontra defeito falso e perde a transição True→False. A transação interna da regeneração começa depois da escrita da foto (`app/ponto/portas/celula.py:212`; `app/escala/folgas.py:210`; `app/escala/folgas.py:219`).

2. **Bloqueia — os demais `atomic` não serializam a transição mensal.**

   Lançamento, alteração, lote e reparentamento tiram snapshot antes da escrita e comparam depois dentro de `atomic`; entretanto, não travam uma linha comum do vínculo/mês antes do snapshot (`app/escala/folgas.py:299`; `app/escala/folgas.py:344`; `app/escala/folgas.py:372`; `app/escala/folgas.py:480`).

   Contraexemplo inferido: dois lançamentos em datas diferentes do mesmo mês podem ambos observar defeito verdadeiro e ambos solicitar o re-julgamento mensal. A unicidade de FolgaDia é por vínculo/data, não por transição mensal (`app/escala/models.py:1600`; `app/escala/folgas.py:301`; `app/escala/folgas.py:302`). Portanto, “re-julga uma vez” não está garantido; duplicação efetiva de pergunta/chamado sob essa concorrência: **[não verificado]**.

   No fluxo sequencial, a escrita antecede a comparação e o julgamento; não encontrei chamada deliberada com a FolgaDia ainda não salva nesses quatro atos (`app/escala/folgas.py:302`; `app/escala/folgas.py:304`; `app/escala/folgas.py:346`; `app/escala/folgas.py:379`; `app/escala/folgas.py:483`; `app/escala/folgas.py:490`).

3. **Bloqueia — falha no julgamento pode consumir definitivamente o gatilho da cura.**

   A porta captura exceção de `julgar_celula` por dia e continua; assim, uma falha não relacionada à integridade da transação pode permitir a confirmação da FolgaDia sem concluir o re-julgamento (`app/ponto/portas/celula.py:402`; `app/ponto/portas/celula.py:405`). Na tentativa seguinte, o mês já não entra em `meses_no_defeito`, portanto esse mecanismo não tenta novamente os dias que falharam (`app/escala/folgas.py:210`; `app/escala/folgas.py:217`).

   Isso é relevante para o caso curado: a impressão normal usa DNA e outros insumos, mas não codifica explicitamente esse defeito mensal; impressão igual pode ser pulada sem `forcar` (`app/ponto/services/cartorio.py:1348`; `app/ponto/services/cartorio.py:1358`; `app/ponto/services/cartorio.py:1362`). Recuperação por outro evento em produção: **[não verificado]**.

4. **Bloqueia — “pai na fila” é conferido antes da trava, não no momento da costura.**

   O comando materializa os IDs em `qs_na_fila`, grava a reversão e somente depois chama a porta (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:91`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:95`).

   A porta trava/recarrega o destino, mas aceita imediatamente qualquer status não terminal; não revalida a fila sob essa trava (`app/chamados/services/fio.py:759`; `app/chamados/services/fio.py:764`; `app/chamados/services/fio.py:837`). Contraexemplo inferido: pai selecionado, depois silenciado ou convertido em `registrado`, ainda recebe a pergunta. A fila exclui ambos; a costura só verifica terminalidade nesse ramo (`app/chamados/services/painel.py:186`; `app/chamados/services/painel.py:188`; `app/chamados/services/fio.py:765`). Ocorrência em produção: **[não verificado]**.

5. **Fila — a trilha diz “re-julgados” antes de saber quem foi julgado.**

   `_rejulgados` é registrado antes da exclusão dos vereditos humanos e antes da chamada ao juiz (`app/ponto/portas/celula.py:298`; `app/ponto/portas/celula.py:303`; `app/ponto/portas/celula.py:395`; `app/ponto/portas/celula.py:402`). Além das exceções, o juiz pode retornar `None` para hoje com turno ainda não terminado (`app/ponto/services/cartorio.py:1982`; `app/ponto/services/cartorio.py:1986`).

   Logo, a contagem atual comprova seleção para tentativa, não quantidade efetivamente re-julgada; o teste que exige uma trilha não distingue essas situações (`app/ponto/portas/celula.py:303`; `app/escala/tests/test_f3_beco1_cadastro_entrou.py:132`).

6. **Fila — censo de FolgaDia: não encontrei escritor explícito de entrada sobrevivente fora da porta.**

   Os escritores explícitos de produção encontrados são:

   | Escritor | Operação e encaminhamento |
   |---|---|
   | `gerar_foto_folgas` | Apaga/recria e entrega snapshot à re-lavra (`app/escala/folgas.py:170`; `app/escala/folgas.py:174`; `app/escala/folgas.py:193`). |
   | `lancar_folga` | `get_or_create`, seguido da re-lavra mensal (`app/escala/folgas.py:302`; `app/escala/folgas.py:304`). |
   | `_foto_do_dia` | Apaga, cria ou altera tipo; os chamadores completos são alteração e lote (`app/escala/folgas.py:323`; `app/escala/folgas.py:327`; `app/escala/folgas.py:332`; `app/escala/folgas.py:346`; `app/escala/folgas.py:379`). |
   | `reparentar_folga` | Altera FK e entrega snapshot do receptor (`app/escala/folgas.py:481`; `app/escala/folgas.py:483`; `app/escala/folgas.py:490`). |

   `core/portas.py` **registra** o escritor autorizado; não escreve FolgaDia: declara a família e aponta para `escala/folgas.py` (`app/core/portas.py:51`; `app/core/portas.py:470`). `_foto_do_dia` isoladamente não re-julga; seu contrato exige chamada pelos atos completos (`app/escala/folgas.py:310`).

   Há exclusão indireta fora desse módulo: apagar EscalaColaborador apaga suas FolgaDia por CASCADE, e existem deletes de vínculo nas reversões. Isso não representa entrada de folga que cure um vínculo sobrevivente (`app/escala/models.py:1582`; `app/colaboradores/services/vinculo.py:864`; `app/colaboradores/services/vinculo.py:940`). A FK do calendário também prevê `SET_NULL`; exclusões externas de calendário: **[não verificado]** (`app/escala/models.py:1586`).

   Leitores diretos encontrados: divergência/foto/guardas/reparentamento na própria porta (`app/escala/folgas.py:101`; `app/escala/folgas.py:165`; `app/escala/folgas.py:296`; `app/escala/folgas.py:317`; `app/escala/folgas.py:464`; `app/escala/folgas.py:502`); defeito e cálculo do vínculo (`app/escala/models.py:1249`; `app/escala/models.py:1286`; `app/escala/models.py:1310`; `app/escala/models.py:1356`); carregadores (`app/escala/alimentacao.py:98`; `app/ponto/precedencia.py:206`); Plano (`app/escala/views_plano_folgas.py:60`; `app/escala/views_plano_folgas.py:112`); diagnósticos e relatórios (`app/escala/services/fase_conflitante.py:74`; `app/ponto/services/diagnostico_escala.py:102`; `app/relatorios/furos.py:86`); comandos de reparentamento e auditoria (`app/escala/management/commands/reparentar_folgas.py:28`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1533`).

7. **Fila — censo de CelulaDia e `rejulgar`: sem segundo escritor introduzido.**

   | Campo | Escritores de produção encontrados |
   |---|---|
   | `trabalha`, `dna` | Regeneração: fora de vigência, criação e reescrita (`app/ponto/portas/celula.py:237`; `app/ponto/portas/celula.py:253`; `app/ponto/portas/celula.py:266`); reversão (`app/ponto/portas/celula.py:560`); geração/enriquecimento/futuro (`app/ponto/portas/celula.py:623`; `app/ponto/portas/celula.py:634`; `app/ponto/portas/celula.py:642`). |
   | `regenerada_em`, `regeneracoes`, `dna_anterior` | Reescrita e reversão (`app/ponto/portas/celula.py:267`; `app/ponto/portas/celula.py:268`; `app/ponto/portas/celula.py:269`; `app/ponto/portas/celula.py:561`; `app/ponto/portas/celula.py:566`; `app/ponto/portas/celula.py:567`). |

   Leitores de `trabalha` encontrados incluem juiz/fábrica (`app/chamados/juizes.py:425`; `app/chamados/services/fabrica_da_celula.py:185`); vínculo/jornada/grade (`app/escala/models.py:1095`; `app/escala/servico_jornada.py:270`; `app/escala/servico_jornada.py:303`; `app/escala/services/leitor_celula.py:375`; `app/escala/services/furos_diarios.py:313`; `app/ponto/marcos.py:155`; `app/ponto/precedencia.py:350`); cartório e projeções (`app/ponto/services/cartorio.py:1227`; `app/ponto/services/cartorio.py:1408`; `app/ponto/services/cartorio.py:1982`; `app/ponto/services/dia_do_colab.py:90`; `app/ponto/services/esmeril_espelho.py:71`; `app/ponto/services/flip_auto.py:208`; `app/ponto/services/passivo_s84.py:96`; `app/ponto/supra_juiz.py:196`); folha/tela/Pauta/relatório (`app/folha/export.py:1237`; `app/core/views.py:396`; `app/pautas/services.py:417`; `app/relatorios/dia_da_frota.py:47`).

   Também há leitores por consulta nos diagnósticos e seleções (`app/chamados/services/fila_por_causa.py:192`; `app/escala/services/feriado_do_posto.py:94`; `app/escala/services/cadastro_realidade.py:217`; `app/escala/services/propositor.py:474`; `app/escala/services/semana_do_posto.py:86`; `app/ponto/services/plano_b_no_dinheiro.py:144`; `app/ponto/services/vigia_entrada.py:79`; `app/ponto/management/commands/vigia_de_hora.py:21`). Auditorias/comandos leem o campo em `app/chamados/management/commands/auditar_invariantes_chamados.py:377`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:118`, `app/ponto/management/commands/censo_fase_12x36.py:392` e `app/ponto/management/commands/tripwire_celulas.py:30`.

   Leitores de `dna`: modelo/jornada/utilitários (`app/escala/models.py:1122`; `app/escala/models.py:1187`; `app/escala/models.py:1232`; `app/escala/servico_jornada.py:270`; `app/escala/utils.py:46`; `app/escala/utils.py:97`; `app/escala/utils.py:130`; `app/escala/utils.py:164`; `app/escala/utils.py:1914`); juízes/adesão/Pauta (`app/chamados/juizes.py:891`; `app/chamados/juizes.py:1047`; `app/chamados/services/adesao.py:32`; `app/pautas/services.py:413`; `app/ponto/juiz_batida.py:158`); serviços (`app/ponto/services/cartorio.py:1196`; `app/ponto/services/cartorio.py:1348`; `app/ponto/services/cartorio.py:1800`; `app/ponto/services/dia_do_colab.py:45`; `app/ponto/services/diagnostico_escala.py:153`; `app/ponto/services/esmeril_espelho.py:116`; `app/ponto/services/faltas_de_hoje.py:43`; `app/ponto/services/veredito_celula.py:31`; `app/ponto/services/vigia_entrada.py:126`); comandos (`app/ponto/management/commands/aplicar_col900.py:67`; `app/ponto/management/commands/backfill_b5_insumos.py:73`; `app/ponto/management/commands/censo_col900.py:119`; `app/ponto/management/commands/censo_fase_12x36.py:392`; `app/ponto/management/commands/detectar_intervalo_ausente.py:53`; `app/ponto/management/commands/tripwire_celulas.py:99`; `app/ponto/management/commands/vigia_b5_ausencia.py:79`; `app/escala/management/commands/celulas_contra_o_template.py:49`).

   `regenerada_em` alimenta invalidação, fase e diagnóstico (`app/chamados/veredito_lavrado.py:153`; `app/escala/services/fase_12x36.py:189`; `app/escala/services/regeneracao.py:80`; `app/ponto/management/commands/gerar_celulas.py:242`; `app/ponto/services/cartorio.py:365`; `app/ponto/management/commands/supra_juiz.py:169`; `app/chamados/management/commands/materializar_passivo_validado.py:220`; `app/ponto/services/dia_do_colab.py:91`). `dna_anterior` é lido na reversão e nos comandos de correção/materialização (`app/ponto/portas/celula.py:548`; `app/ponto/portas/celula.py:557`; `app/ponto/management/commands/aplicar_col900.py:68`; `app/chamados/management/commands/materializar_passivo_validado.py:222`). Para `regeneracoes`, encontrei leitura operacional apenas nos incrementos da porta (`app/ponto/portas/celula.py:269`; `app/ponto/portas/celula.py:567`).

   O único fornecedor explícito de `rejulgar` é a re-lavra de folgas; a porta consome o parâmetro no bloco novo (`app/escala/folgas.py:267`; `app/ponto/portas/celula.py:289`). Os outros chamadores mantêm `rejulgar=()` e não entram nesse bloco: signals, cadastro de tipo, executor automático, correção retroativa, exceção exportada, regeneração diária e `_regen` do vínculo (`app/ponto/portas/celula.py:102`; `app/escala/signals.py:71`; `app/escala/signals.py:117`; `app/escala/services/cadastro_tipo.py:348`; `app/escala/services/escala_auto_executor.py:104`; `app/escala/services/escala_auto_executor.py:143`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:230`; `app/ponto/portas/regen_exportada.py:174`; `app/ponto/portas/celula.py:504`; `app/colaboradores/services/vinculo.py:320`). O import antigo é reexportação da mesma função (`app/ponto/management/commands/gerar_celulas.py:13`).

8. **Fila — pai/fio: a alteração usa o escritor existente, mas sua seleção precisa da correção do achado 4.**

   “Pai/fio” não é o nome do campo persistido alterado: a âncora relevante é `PerguntaDisputa.chamado_destino`, com fallback para o chamado da disputa (`app/chamados/models.py:2772`; `app/chamados/services/fio.py:116`). Seus escritores encontrados são nascimento/reapontamento por identidade em `abrir` e reapontamento explícito pela porta do fio (`app/chamados/models.py:2439`; `app/chamados/models.py:2465`; `app/chamados/services/fio.py:845`).

   O pai LIMBO é selecionado entre vivos, por maior PK; C11 publica pai/perguntas; comando, fio e gates leem essa âncora (`app/chamados/services/limbo_folgas.py:217`; `app/escala/services/cadastro_realidade.py:457`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:85`; `app/chamados/services/fio.py:230`; `app/chamados/models.py:328`). Repetir a mesma costura retorna `ja_reatada` sem salvar (`app/chamados/services/limbo_folgas.py:239`).

9. **Fila — não encontrei segundo juiz, derivação paralela ou escape sequencial de exportada nos caminhos examinados.**

   O defeito mensal é perguntado duas vezes à mesma EC; o código novo apenas deduplica meses e enumera dias (`app/escala/folgas.py:207`; `app/escala/folgas.py:210`; `app/escala/folgas.py:219`). A fonte decide ciclo, folga fixa e existência de FolgaDia no mês calendário (`app/escala/models.py:1246`).

   Exportada usa `_competencias_exportadas`, que pergunta ao juiz `competencia_fechada_para_celula`; cobertura usa `vinculo_cobre_a_data`. Esses juízes estão registrados, junto com `vinculo_do_dia` (`app/ponto/portas/celula.py:205`; `app/ponto/management/commands/gerar_celulas.py:173`; `app/ponto/portas/celula.py:295`; `app/core/juizes.py:312`; `app/core/juizes.py:322`; `app/core/juizes.py:1285`). A reata consulta a definição existente de fila, sem copiar o predicado (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`; `app/chamados/services/painel.py:186`).

   O caminho normal é `julgar_celula` → `julgar_colab` → `perguntar_pela_celula`; portanto não foi construído um modo deliberado “re-julga sem emitir” (`app/ponto/portas/celula.py:402`; `app/ponto/services/cartorio.py:1989`; `app/ponto/services/cartorio.py:997`). A fábrica preserva limbo entregue → Pauta e demais motivos → pergunta com push silenciado (`app/chamados/services/fabrica_da_celula.py:171`; `app/chamados/services/fabrica_da_celula.py:175`).

   Exportadas são puladas na regeneração e na inclusão dos dias adicionais; folgas não passa `apesar_da_lavra` (`app/ponto/portas/celula.py:244`; `app/ponto/portas/celula.py:295`; `app/escala/folgas.py:262`). Nos callbacks examinados, feriado recebe somente dias tocados; rejunção global apenas aponta célula existente e o signal retorna sem mudança de status (`app/ponto/portas/celula.py:462`; `app/chamados/rejuncao.py:73`; `app/chamados/signals.py:168`). Não encontrei escape sequencial por esses caminhos; exportação concorrente à leitura da guarda: **[não verificado]**.

10. **Fila — relógio permanece na borda nova, mas não há instante único.**

    A leitura nova é `timezone.localdate()` dentro da porta; os helpers mensais recebem datas e não consultam relógio (`app/ponto/portas/celula.py:290`; `app/escala/folgas.py:200`; `app/escala/folgas.py:214`). Contudo, esse teto ignora `hoje` recebido: horizonte usa `hoje or date.today()`, inclusão mensal usa `localdate()` e julgamento consulta outro `localdate()` (`app/ponto/portas/celula.py:177`; `app/ponto/portas/celula.py:290`; `app/ponto/portas/celula.py:394`).

    O mês re-julgado é o das datas escritas, não automaticamente o mês atual (`app/escala/folgas.py:208`; `app/escala/folgas.py:373`). No lote, “competência corrente” vem de `janela_atual(hoje, empresa)` (`app/escala/folgas.py:368`). Na reata, hoje vem da conversão local do instante do comando, e as competências vêm de `competencias_a_lavrar`; a consulta à fila lê outro instante (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:52`; `app/escala/services/cadastro_realidade.py:489`; `app/escala/services/cadastro_realidade.py:495`; `app/chamados/services/painel.py:188`). O fuso configurado é São Paulo; fuso efetivo do processo: **[não verificado]** (`app/config/settings/base.py:93`).

11. **Fila — os testes têm mordida local, mas não certificam as propriedades completas.** [nao verificado]

    Esta é análise das asserções; execução dos mutantes e resultado RED/GREEN: **[não verificado]**.

    | Teste | Mutação plausível que suas asserções não pegariam | [nao verificado]
    |---|---|
    | Cadastro: caso 107 | Proteger TXT e esquecer holerite publicado: a fixture cria somente `ExportacaoDominio` (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:52`; `app/escala/tests/test_f3_beco1_cadastro_entrou.py:123`). |
    | Cadastro: caso 155 | Retirar guarda humana/futuro: as células observadas são geradas e passadas (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:63`; `app/escala/tests/test_f3_beco1_cadastro_entrou.py:137`). |
    | Cadastro: segunda folga | Duplicar pergunta/chamado na primeira execução: observa veredito e trilha, sem contar essas entidades (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:148`). |
    | Cadastro: lote | Tirar snapshot apenas do primeiro mês: usa uma única data de outubro (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:161`). |
    | Cadastro: calendário | Transformar geração em no-op retornando zero: vira `skipTest` (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:172`). |
    | Cadastro: reparentar | Remover o novo ramo de competência passada: usa 31/10, hoje 05/11 e corte dia 21 (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:40`; `app/escala/tests/test_f3_beco1_cadastro_entrou.py:186`). |
    | Reata: apply/idempotência | Gravar reversão depois da costura: verifica conteúdo final, não ordem (`app/chamados/tests/test_f3_beco1_reata_limbo.py:56`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:59`). |
    | Reata: dry | Escrever trilha ou relavrar mantendo destino: compara somente destino (`app/chamados/tests/test_f3_beco1_reata_limbo.py:74`). |
    | Reata: sem pai | Escrever auditoria indevida: verifica chamados, destino e C11, não auditoria (`app/chamados/tests/test_f3_beco1_reata_limbo.py:79`). |
    | Reata: reversão ausente/existente | Alterar outro campo antes de levantar erro: compara somente destino (`app/chamados/tests/test_f3_beco1_reata_limbo.py:92`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:100`). |
    | Reata: pai silenciado | Trocar fila por “silêncio não futuro”, admitindo `registrado`; também não pega silêncio posterior à seleção (`app/chamados/tests/test_f3_beco1_reata_limbo.py:113`; `app/chamados/tests/test_f3_beco1_reata_limbo.py:116`). |
    | Reata: silêncio vencido | Retirar relavra do reapontamen

[truncado em 20000 de 22883 caracteres]

## 05/11 15:22 · sessao ac5a1f66b61d · REVISAO-JUIZ-2 · verificador · origem=Code

O patch fornecido muda somente docstrings; não altera decisões, campos persistidos ou testes (`app/ponto/services/lastro.py:219`, `app/ponto/services/lastro.py:370`). O aval MANHA-0511 e os casos de produção narrados nessas docstrings não estão comprovados pelos anexos: `JUIZ-2.leis` veio vazio e não foram fornecidas linhas de cadastro/sonda. [não verificado]

1. **Item 1 — escritores e leitores. Toca dinheiro: não, neste diff.** Os valores documentados continuam seguindo este fluxo (`app/ponto/services/lastro.py:314`, `app/ponto/services/lastro.py:334`, `app/ponto/services/lastro.py:378`):

   | Valor | Quem escreve | Quem lê |
   |---|---|---|
   | `com_lastro` / `retido_pelo_fechamento` | `julgar()` inicializa os baldes e classifica pela resposta de `reconciliaria()` (`app/ponto/services/lastro.py:249`, `app/ponto/services/lastro.py:316`). | `fechar()` percorre ambos; `contador()` conta separadamente; o comando publica `(a)` e `(a2)` (`app/ponto/services/lastro.py:334`, `app/ponto/services/lastro.py:378`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:56`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:62`). |
   | `contador()['n']` | Recebe somente `len(com_lastro)`; o retido permanece em chave própria (`app/ponto/services/lastro.py:378`). | Testes conferem os dois números; o placar consome a saída do comando (`app/ponto/tests/test_bug114_fechador_de_dois_dias.py:208`, `bin/placar_code.sh:116`). |
   | `_n_lastro`, `_n_retido`, `TOTAL` | O shell extrai as duas linhas e soma ambas ao `TOTAL` (`bin/placar_code.sh:118`, `bin/placar_code.sh:121`, `bin/placar_code.sh:549`). | O próprio placar publica os valores e decide a saída pelo total (`bin/placar_code.sh:176`, `bin/placar_code.sh:177`, `bin/placar_code.sh:551`). |
   | Estado e trilha da tentativa | `reconciliar()` consulta a decisão comum e delega a escrita a `_gravar_estado()`; as recusas passam por `_registrar_recusa()` (`app/chamados/models.py:617`, `app/chamados/models.py:620`, `app/chamados/models.py:380`). | `reconciliaria()` usa a mesma decisão sem registrar; `fechar()` relê o estado e só anexa nota quando houve fechamento (`app/chamados/models.py:627`, `app/ponto/services/lastro.py:338`, `app/ponto/services/lastro.py:345`). |

   Nas entradas dessa cadeia, o detector escreve/renova `contexto_json.marcos_faltantes`, e o lastro lê hora e tipo (`app/ponto/management/commands/detectar_ausencias.py:558`, `app/ponto/management/commands/detectar_ausencias.py:700`, `app/ponto/services/lastro.py:75`). O cartório produz as lâmpadas e entrega a ata à porta de lavra; o lastro consulta essa ata (`app/ponto/services/cartorio.py:638`, `app/ponto/services/cartorio.py:702`, `app/ponto/services/cartorio.py:1802`, `app/ponto/portas/celula.py:768`, `app/ponto/services/lastro.py:89`).

   A resposta do colaborador é gravada pelo serviço de resposta; a validação é carimbada pelo serviço de validação; `_pergunta_viva_calc()` lê esses campos e compara a resposta com a luz (`app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/validacao.py:131`, `app/chamados/juizes.py:1078`, `app/chamados/juizes.py:1092`, `app/chamados/juizes.py:1104`).

2. **Item 2 — contraexemplo concreto. Toca dinheiro: não demonstrado.** Cadastro, data, resultado observado e resultado exigido por LEIS/CORTES: **[não verificado]**. Os identificadores e horários citados no comentário não substituem as linhas de produção ausentes (`app/ponto/services/lastro.py:210`, `app/ponto/services/lastro.py:214`). Não há base fornecida para apresentar um caso nominal divergente. [não verificado]

3. **Item 3 — regra de negócio alterada sem lastro. Toca dinheiro: não, neste diff.** Não há alteração executável de regra nos dois hunks; há uma nova afirmação documental de que o retido permanece no TOTAL por aval confirmado (`app/ponto/services/lastro.py:224`, `app/ponto/services/lastro.py:375`). A soma está implementada no shell (`bin/placar_code.sh:549`); a confirmação normativa específica permanece **[não verificado]**. O texto publicado pelo placar ainda diz “lei aberta no topo do RELATO” (`bin/placar_code.sh:177`).

4. **Item 4 — selo que não morde. Toca dinheiro: não, neste diff.** Não existe teste alterado no patch fornecido; as alterações estão nas duas docstrings (`app/ponto/services/lastro.py:219`, `app/ponto/services/lastro.py:370`). Os testes existentes verificam retenção, separação de `n`, manutenção do chamado aberto e trilha única da recusa (`app/ponto/tests/test_bug114_fechador_de_dois_dias.py:208`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:216`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:244`). O teste do shell exige que três retidos produzam TOTAL 3 (`bin/tests/test_placar_conta_o_retido_do_lastro.sh:645`). Reverter apenas os comentários preserva esses comportamentos; isso não demonstra um selo ineficaz de cura executável (`app/ponto/services/lastro.py:316`, `app/ponto/services/lastro.py:334`, `bin/placar_code.sh:549`). Execução e passagem dos testes: **[não verificado]**.

5. **Item 5 — cadastro atual decidindo passado. Toca dinheiro: sim no caminho de grade/folha; alteração monetária por este patch: não demonstrada.** Há estas leituras no fluxo e nos produtores das atas que ele consome (`app/ponto/services/lastro.py:289`, `app/ponto/services/cartorio.py:1802`; destino da folha documentado em `app/folha/export.py:4`):

   | Campo | Escritor identificado | Leitura temporal |
   |---|---|---|
   | `situacao` | Desligamento e reativação (`app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210`). | O produtor da ata usa a situação atual para impedir emissão no dia julgado; a fila direta do lastro filtra estado do chamado e silêncio (`app/ponto/services/cartorio.py:1697`, `app/ponto/services/cartorio.py:1762`, `app/chamados/services/painel.py:186`). |
   | `data_demissao` | Desligamento grava; reativação apaga (`app/colaboradores/services/desligamento.py:105`, `app/colaboradores/services/desligamento.py:211`). | O cartório passa a data atual do cadastro ao julgamento; sem vínculo, ela participa da comparação com o dia examinado (`app/ponto/services/cartorio.py:1096`, `app/ponto/supra_juiz.py:194`). |
   | `posto` | Transferência (`app/colaboradores/views.py:1385`). | Quando a célula/ata não conclui a premissa, a reavaliação consulta o município do posto atual, sem argumento temporal (`app/chamados/models.py:1249`, `app/chamados/models.py:1255`, `app/chamados/services/feriado_reavaliar.py:33`, `app/ponto/precedencia.py:287`, `app/core/feriados.py:113`). |
   | `empresa` | Transferência muda empresa junto com posto (`app/colaboradores/views.py:1386`). | A guarda decide se a competência do `data_evento` foi entregue usando a empresa atual; o juiz da pergunta também consulta a tranca pela empresa atual (`app/chamados/models.py:347`, `app/chamados/services/vigia_fio.py:807`, `app/chamados/juizes.py:936`, `app/chamados/juizes.py:942`). |
   | `regime` | Edição do cadastro da empresa (`app/colaboradores/views.py:512`, `app/colaboradores/views.py:521`). | Uso do regime atual nessa cadeia para decidir passado: **[não verificado]**. |
   | `isento_ponto` | Tela Passe livre (`app/colaboradores/views.py:173`). | O produtor da ata aplica a flag atual aos dias montados; o cartório também a passa ao classificador, que retorna sem acusações quando isento (`app/escala/utils.py:903`, `app/escala/utils.py:909`, `app/ponto/services/cartorio.py:1419`, `app/ponto/supra_juiz.py:220`). |
   | Vínculo atual | Porta de vínculo cria/atualiza e encerra vigência (`app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:797`). | A seleção consulta a geradora da célula e seus limites; sem geradora, escolhe entre vínculos que cobrem a data, com desempate por ativa/início (`app/escala/servico_jornada.py:55`, `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:282`, `app/escala/alimentacao.py:285`). |

   Essas leituras são demonstráveis no código; cadastro real afetado, valor divergente e violação de regra anexada permanecem **[não verificado]**. Elas não são introduzidas pelos dois hunks documentais (`app/ponto/services/lastro.py:219`, `app/ponto/services/lastro.py:370`).

**VEREDITO: SEM-ACHADO** de desvio demonstrado pelo patch; impacto em produção **[não verificado]**.

## 05/11 15:22 · sessao 0aad819fc0a7 · - · - · origem=Ronald (tmux)

Achado demonstrado no contrato da porta: `foto_da_lavra(self)` executa fora do novo `try`; se a leitura de um campo da lavra falha, o carimbo já aplicado permanece na instância. Isso ocorre inclusive com `lavrar_veredito=False`. (`app/chamados/models.py:2575`, `app/chamados/models.py:2593`, `app/chamados/models.py:2594`, `app/chamados/veredito_lavrado.py:237`)

Executei, somente em memória, os corpos extraídos por AST: C6 falso, autor 17, instante 15/09/2026 15:00 UTC, getter de `veredito_lavrado` levantando erro; resultado: `(None,None,'')` virou `(15/09/2026 15UTC,17,'juiz_aprovou')` após a exceção. Reprodução com Django, campo diferido real e ocorrência em produção: [não verificado]. Não localizei chamador direto da porta que carregue `PerguntaDisputa` com esses campos diferidos; incidência no fluxo corrente: [não verificado]. [nao verificado]

Selos:

- `test_sem_erro_grava_como_sempre` continua passando ao reverter a cura de rollback, porque somente exerce o caminho sem exceção; é um controle, não prova da cura. (`app/chamados/tests/test_o261e_f2_fila.py:216`)
- `test_os_leitores_leem_a_fonte` não examina C6, lastro, SQL ou auditor. Reverter apenas o leitor C6 para a lista anterior mantém esse teste verde; o selo AST separado detectaria a duplicação. (`app/chamados/tests/test_o261e_f2_fila.py:129`, `app/chamados/tests/test_o261e_f2_fila.py:106`, `app/chamados/juizes.py:1112`)
- Os três testes de erro realmente verificam restauração após falha de `save`, falha de `lavrar` e falha de `save` depois de lavrar, mas não exercitam erro ao tirar a fotografia. (`app/chamados/tests/test_o261e_f2_fila.py:179`, `app/chamados/tests/test_o261e_f2_fila.py:190`, `app/chamados/tests/test_o261e_f2_fila.py:200`)

Mapa de escritores/leitores enviado nas mensagens anteriores. Mudança de regra de negócio no conjunto das vias: não identifiquei; os quatro valores de escrita e o acréscimo da tranca correspondem ao conjunto anterior fornecido no patch. (`app/chamados/models.py:2276`, `app/chamados/models.py:2543`; patch fornecido)

Duplicidade da nova trilha em duas chamadas diretas permanece uma ressalva, não um achado forte: cada recusa insere outro evento, mas a distinção entre reenvio e evento novo já está documentada como pergunta de lei. (`app/chamados/services/materializacao.py:548`, `app/core/observ.py:134`, `app/core/contratos_estruturais.py:560`)

## 05/11 15:21 · sessao ffe0cbacee28 · - · - · origem=Ronald (tmux)

Achado demonstrável do item 4: os três testes novos só exercitam `corrente`, o comando Django e `caminho_do_log`; nenhum executa o consumidor shell (`app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19-47`). Reverter somente o redirecionamento de `bin/encadeado.sh:101-103` preserva todas as condições verificadas pelos três testes, embora a saída deixe de ir ao log declarado. Execução dessa mutação: [nao verificado], pois mantive somente leitura.

Fluxo dos campos/valores alterados:

- `ENC` escreve `log`, com default `<cmd>.log` (`app/config/crons.py:53-54`); `disparar_perguntas_competencia` declara `fio_mudo.log` (`app/config/crons.py:1464-1466`).
- `corrente` lê os campos do dict e escreve o novo sexto elemento, resolvido por `caminho_do_log` (`app/config/crons.py:1782-1791`).
- `crons corrente` lê a tupla e escreve `log<TAB>argv` (`app/core/management/commands/crons.py:30-31`).
- O shell lê a linha, separa log/argv e escreve `ENC_LOG` para o processo filho (`bin/encadeado.sh:56-64`); o filho lê `ENC_LOG` e acrescenta stdout/stderr do runner nesse arquivo (`bin/encadeado.sh:101-103`).
- O comando escreve o contador `FABRICA-POR-EVENTO` (`app/chamados/management/commands/disparar_perguntas_competencia.py:238-240`); o placar lê o último contador e verifica apenas a data de modificação do arquivo (`bin/placar_code.sh:381-383`).
- `RAW.log` é escrito pelo construtor e agora lido pelo resolvedor comum antes de renderizar o redirecionamento (`app/config/crons.py:108`, `app/config/crons.py:1799-1800`).
- Apesar da documentação dizer “UM resolvedor” para C/RAW/ENC, `C` ainda concatena `LOGS/e['log']` diretamente (`app/config/crons.py:1790`, `app/config/crons.py:1810-1811`). Isso é inconsistência documental; regressão concreta nas declarações atuais: [nao verificado].

Fragilidade adicional do leitor: um log contendo contador antigo `0`, seguido hoje por um traceback antes da emissão do novo contador, mantém esse `0` como último resultado e passa a guarda de frescor pelo mtime atualizado (`bin/encadeado.sh:102`; `app/chamados/management/commands/disparar_perguntas_competencia.py:238`; `bin/placar_code.sh:381-383`). Ocorrência em produção: [nao verificado]. Não é contraexemplo de cadastro.

Regra de negócio alterada por este diff sem lastro: [nao verificado]; a mudança examinada transporta e persiste logs, enquanto os argumentos de execução continuam saindo dos mesmos campos (`app/config/crons.py:1782-1783`; `app/core/management/commands/crons.py:30-31`).

## 05/11 15:21 · sessao 32a24f646a56 · - · - · origem=Ronald (tmux)

- **Item 4 — selo não cobre a cura shell:** os três testes novos exercitam apenas `corrente`, o comando Django e `caminho_do_log`; nenhum lê ou executa `encadeado.sh` (`app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19`, `:30`, `:43`). Reverter apenas o consumo de `log<TAB>argv` e o redirecionamento shell mantém esses asserts inalterados (`bin/encadeado.sh:58`, `:64`, `:101`). Passagem efetivamente executada: [nao verificado]. Toca dinheiro: **não diretamente** (`app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19`).

- **Limitação do contador — último número pode ser de outra rodada:** o comando escreve `len(deixou)` no prefixo raspado (`app/chamados/management/commands/disparar_perguntas_competencia.py:238`); o shell acrescenta stdout/stderr ao arquivo (`bin/encadeado.sh:102`); o placar pega o último prefixo e verifica somente a modificação do arquivo desde meia-noite (`bin/placar_code.sh:381`, `:382`). Contraexemplo construído: arquivo com número 0 de 04/11, execução de 05/11 falha antes do contador e acrescenta traceback; o placar aceita o número antigo como atual, pelas condições citadas (`bin/placar_code.sh:381`, `:382`; ponto de emissão: `app/chamados/management/commands/disparar_perguntas_competencia.py:238`). Ocorrência em produção: [nao verificado]. Essa fragilidade está no leitor preexistente, fora da linha efetivamente alterada do placar (`bin/placar_code.sh:381`, `:383`).

- **Item 3:** não identifiquei alteração de regra de negócio sem lastro no patch fornecido; o código alterado transporta o caminho de log e redireciona a saída (`app/config/crons.py:1782`, `app/core/management/commands/crons.py:30`, `bin/encadeado.sh:102`). O fio mudo continua autorizado a escrever pelo caminho existente (`app/chamados/management/commands/disparar_perguntas_competencia.py:280`, `:287`); o cron continua na lista de juízes por causa desse universo (`app/config/crons.py:1976`, `:1980`).

## 05/11 15:21 · sessao 1c16db342e2e · PWA-OFFLINE-APP-SHELL · censo · origem=Code

**O SW atual não grava o HTML das navegações; a fila não guarda o colaborador; existem caminhos para reenvio da mesma batida.** O servidor deduplica por colaborador e horário, com limites descritos abaixo. (`app/static/sw.js:45-50`; `app/static/pwa.js:113-119,126-165`; `app/api/views.py:557-575`.)

As contraprovas abaixo são **cenários condicionais inferidos do código**, não ocorrências de produção. Cache existente, sessões reais, duplicidades reais e comportamento em aparelhos: **[nao verificado]**.

1. **Navegação: o que entra no cache e o que volta offline — certeza alta.**

   O fluxo efetivo do SW é:

   | Ramo | Escrita no cache | Resposta |
   |---|---|---|
   | Método diferente de GET | Nenhuma | SW não intercepta. `app/static/sw.js:35-36` |
   | GET em `/login/`, `/logout/`, `/admin/`, `/onboarding/` | Nenhuma | Só `fetch`, sem fallback. `app/static/sw.js:39-43` |
   | GET com `mode === 'navigate'`, incluindo `/` e `/ponto/bater/` | **Nenhuma** | Rede; se rejeitar, procura cópia; sem cópia, `Response.error()`. **Retorna antes dos demais ramos.** `app/static/sw.js:29-30,45-50` |
   | GET não navegação em `/static/` | Grava resposta da rede em caso de miss | Cache primeiro; depois rede. `app/static/sw.js:53-64` |
   | GET não navegação em `/` ou prefixo `/ponto/bater/` | Grava clone da resposta | Rede primeiro; em rejeição, cache ou `Response.error()`. `app/static/sw.js:67-79` |
   | Demais GET | Nenhuma | Rede; em rejeição, cache ou `Response.error()`. `app/static/sw.js:82-85` |

   A instalação pré-carrega somente `pwa.js`, manifest e dois ícones; não inclui HTML. Os ramos que gravam respostas não verificam `response.ok`. HTTP 403/404 recebido da rede também não aciona o fallback, que está no `.catch()`. (`app/static/sw.js:3-12,48,57-60,71-77`.)

   **Contraexemplo:** instalação limpa → navegar online em `/ponto/bater/` → ficar offline → recarregar. A primeira navegação não guardou HTML; sem outra escrita correspondente, o fallback devolve `Response.error()`. Visitar online essa tela não basta para torná-la disponível offline. (`app/static/sw.js:3-12,29-30,45-50`.)

   Já um `fetch('/ponto/bater/')` que chegue como GET **não navegação** pode alimentar o cache pelo ramo posterior. A existência dessa chamada em execução: **[nao verificado]**. (`app/static/sw.js:46,68-77`.)

2. **Censo de escritores e leitores — certeza alta sobre o código.**

   **Cache do SW.** `CACHE_NAME` é `juliani-__BUILD_ID__`. O servidor calcula oito caracteres do SHA-1 do conteúdo dos cinco arquivos governados, memoriza por 30 segundos e substitui o marcador ao servir `/sw.js`; templates não integram essa lista. (`app/static/sw.js:1`; `app/core/views.py:520-555,560-572`.)

   Os escritores são `install → addAll` e os dois `cache.put(event.request, clone)`. As chaves são as URLs do pré-cache ou o `event.request`, sem acrescentar identidade do colaborador. Os leitores usam `caches.match(...)` **sem restringir a consulta ao `CACHE_NAME`**; a ativação apaga todos os caches com nome diferente do atual. (`app/static/sw.js:3-20,29-30,56-59,73-74`.)

   **Fila e configuração.** O armazenamento implementado é IndexedDB `juliani-offline`, versão 2:

   - `batidas_pendentes`: chave `id` autoincremental; campos `tipo`, `timestamp` corrigido, `timestamp_dispositivo` bruto, `timestamp_gap_ms`, `latitude`, `longitude`, `gps_accuracy`, `gps_captured_at`, `foto`. Não há campo de colaborador, sessão ou CSRF. (`app/static/pwa.js:48-56,107-119`.)
   - `config`: chave `chave`, registros `{chave, valor}`. `calcularESalvarGap()` escreve `ntp_gap` e `ntp_calculado_em` a partir de `/hora/`; `obterGap()` lê e renova após 30 minutos; a gravação offline e `JulianiPWA.agora()` consomem o gap. (`app/static/pwa.js:63-104,108-109,305-308`.)
   - A fila não possui alternativa em localStorage nesses caminhos. O `sessionStorage['avisos_convite']` do módulo serve ao convite de notificações. (`app/static/pwa.js:48-60,107-122,126-183,353,362,366`.)

   A escrita real da batida é `registrarBatidaOffline()`. Há chamadas no v2, no template antigo e na triagem apta; isso não prova que todos esses templates sejam usados em produção. (`app/templates/ponto/bater_ponto_v2.html:356,374`; `app/templates/ponto/bater_ponto.html:543,597`; `app/templates/ponto/triagem/apto.html:289`.)

   **Leitura, envio e exclusão.** `sincronizarBatidas()` exige `navigator.onLine` e a meta de perfil colaborador, lê **toda** a fila e ordena pelo `timestamp` corrigido. Envia multipart para `/api/ponto/bater/` com tipo, coordenadas, timestamp **bruto**, liveness indisponível e foto. Não envia `id`, timestamp corrigido, gap, precisão ou instante GPS. (`app/static/pwa.js:126-157`.)

   O CSRF vem do cookie atual, em cada envio. Não há `Authorization` nem identidade gravada no item. A configuração admite autenticação JWT e sessão; a API escolhe `request.user.colaborador`. A validação efetiva do framework instalado em produção é **[nao verificado]**. (`app/static/pwa.js:140-157`; `app/config/settings/base.py:32-43`; `app/api/views.py:398-404`.)

   Após resposta OK, o sincronizador exige JSON e depois apaga pelo `id` local. HTTP 400 também apaga; outros status e exceções conservam o item. O indicador apenas conta pendências. Os disparadores são inicialização online, evento `online` e mensagem `SYNC_BATIDAS`. (`app/static/pwa.js:15-16,158-192,320-328`.)

   **Leitores no servidor.** A rota normal chega a `api.views.api_bater_ponto`; a configuração kernel tem outra implementação, em `views_core`. Qual configuração atende produção é **[nao verificado]**. (`app/config/urls.py:37`; `app/api/urls.py:67`; `app/config/urls_core.py:6`; `app/api/urls_core.py:5,14`.)

   Nas duas APIs, `timestamp_dispositivo` é lido para auditoria de GPS ausente, composição do payload de assinatura e decisão da hora efetiva; depois é passado a `registrar_batida`. (`app/api/views.py:459,497-504,536-539,682-695`; `app/api/views_core.py:451,489-496,528-531,679-692`.)

   `hora_do_aparelho()` aceita o horário legível entre agora e 48 horas atrás; fora disso usa agora, preservando o horário bruto legível. `registrar_batida()` recebe esse valor em `**campos` e o encaminha ao `create`; o campo persistido é nullable. (`app/api/hora_do_aparelho.py:98-121,135-153`; `app/ponto/registro_batida.py:94,157-159`; `app/ponto/models.py:75`.)

   Há dois leitores persistidos relevantes:

   - `descartadas()` compara `timestamp` e `timestamp_dispositivo`; seu uso efetivo em produção é **[nao verificado]**. (`app/api/hora_do_aparelho.py:156-185`.)
   - A tela DP mostra “dispositivo”, mas sua view seleciona somente `origem='offline_pendente', aprovada=False`; a API atual grava `origem='app'`. Portanto esse leitor não demonstra visibilidade das novas batidas da fila nessa tela. (`app/templates/ponto/batidas_offline_pendentes.html:71-72`; `app/ponto/views.py:441-446`; `app/api/views.py:683-695`.)

   `ponto.views.bater_ponto` é outra porta: recebe o formulário web e delega E/S a `ramo_registrar_es`. A fila não a chama. A antiga `batida_offline`, embora decorada com `csrf_exempt`, responde 410. (`app/ponto/views.py:268-273,346-354,417-428`; `app/static/pwa.js:153`.)

   **Recarga.** A URL `/ponto/bater/` seleciona triagem; `/ponto/bater-v2/` seleciona o template citado. No v2, a recarga acontece **ao voltar visível após mais de cinco minutos oculta**, sem consultar `_enviando` ou a foto; não é um timer disparando enquanto permanece oculta. Há ainda recarga por `pageshow.persisted` e por troca de controlador, esta com guarda de formulário alterado. (`app/ponto/urls.py:11-12`; `app/ponto/views.py:356-358`; `app/templates/ponto/bater_ponto_v2.html:305-310`; `app/static/pwa.js:36-45,430-431`.)

   **Contraprovas:** um gap correto não garante horário corrigido no servidor, pois o multipart transmite o bruto; e uma selfie apenas em `_fotoBlob` não está protegida da recarga após cinco minutos. Se a gravação IndexedDB já completou, a pendência está fora dessa variável. (`app/static/pwa.js:109-121,147`; `app/templates/ponto/bater_ponto_v2.html:146,252,305-310`.)

3. **Troca de identidade no mesmo aparelho — certeza alta, cenário condicional.**

   A fila não guarda o colaborador. A meta usada como guarda indica somente perfil e, na base, nasce de `not request.user.is_staff`; não identifica o dono das pendências. (`app/static/pwa.js:113-119,128`; `app/templates/base.html:16`.)

   **Contraexemplo passo a passo:**

   1. Colaborador A registra offline: ficam tipo, horário, GPS e foto, sem A. (`app/static/pwa.js:107-119`.)
   2. A sai; B entra no mesmo armazenamento. O logout mostrado encerra autenticação e redireciona, sem limpar a fila IndexedDB. (`app/core/views.py:79-84`; `app/static/pwa.js:48-54`.)
   3. Uma página elegível de B sincroniza, lendo todos os itens e o CSRF atual. (`app/static/pwa.js:126-157,328`.)
   4. A API resolve `request.user.colaborador` como B. Passando autenticação e demais validações, os dados capturados por A são processados em nome de B. (`app/api/views.py:402,415-423,448-466,683-695`; `app/api/views_core.py:389`.)

   Não há garantia de aceitação: situação do colaborador e GPS podem impedir o registro. Ocorrência dessa troca em produção: **[nao verificado]**. (`app/api/views.py:415-423,448-466`.)

4. **Duplicidade: dois envios são possíveis; duas linhas dependem da deduplicação — certeza alta.**

   **Online + fila:** o v2 envia online; se o servidor gravar e a resposta se perder, `fetch` pode rejeitar e o catch grava uma representação offline. A sincronização posterior envia novamente. (`app/templates/ponto/bater_ponto_v2.html:328-356`; `app/static/pwa.js:138-157`.)

   **Chamadas concorrentes:** boot, `online` e mensagem do SW podem chamar a mesma função. Ela não tem trava: faz `getAll()`, envia e só depois exclui. Duas chamadas podem ler o mesmo item antes da exclusão; o SW também transmite a mensagem a todos os clientes encontrados. (`app/static/pwa.js:15-16,126-165,320-328`; `app/static/sw.js:116-119`.)

   **Retry:** commit seguido de resposta perdida, JSON inválido ou falha na exclusão conserva o item. O `id` local não é enviado como chave de idempotência. (`app/static/pwa.js:143-179`.)

   O servidor possui estas proteções:

   - API: mesmo colaborador + tipo, em janela inclusiva de **±5 minutos do horário efetivo**, devolve `ja_registrada`. (`app/api/views.py:557-575`; `app/api/views_core.py:549-567`.)
   - Escritor: mesmo colaborador + tipo, não retratada, em **±60 segundos**; depois verifica a chave exata colaborador + timestamp, inclusive outro tipo ou retratada. (`app/ponto/registro_batida.py:106-122`.)
   - Tipo oposto: envelope de 300 segundos com escala/marcos, senão 60 segundos, com exceção quando casa com marco próprio; a API pode responder sucesso `ignorada_intervalo`. (`app/ponto/registro_batida.py:21-49,138-151`; `app/api/views.py:698-721`.)
   - Modelo: unicidade declarada por **colaborador + timestamp**, sem UUID do evento. Aplicação dessa constraint no banco de produção: **[nao verificado]**. (`app/ponto/models.py:102-106`.)

   **Contraexemplo à idempotência universal:** item com horário bruto de mais de 48 horas é enviado às 10:00; servidor usa agora e grava; resposta se perde. Retry às 10:06 usa outro agora. A primeira linha fica fora de ±5 minutos, ±60 segundos e da chave exata. Sem outras colisões ou impedimentos, pode nascer outra linha para o mesmo evento físico. (`app/api/hora_do_aparelho.py:141-153`; `app/static/pwa.js:158-179`; `app/api/views.py:557-562`; `app/ponto/registro_batida.py:106-122,138-159`.)

5. **Conteúdo pessoal do HTML cacheável — certeza alta sobre os bytes; vazamento ocorrido [nao verificado].**

   O v2 e sua base contêm:

   | Conteúdo | Presença na resposta HTML |
   |---|---|
   | CSRF | Sim, no formulário. `app/templates/ponto/bater_ponto_v2.html:102-103` |
   | Identificação | `user.username` na base e `user.pk` no painel incluído. Nome completo não aparece diretamente no v2; equivalência entre username e nome real: **[nao verificado]**. `app/templates/base.html:316,432`; `app/templates/core/_copiloto.html:17` |
   | Posto | Latitude, longitude e raio interpolados em JavaScript. `app/templates/ponto/bater_ponto_v2.html:139-140` |
   | Estado da tela | Tipo sugerido e, no sucesso, hora e aviso. `app/templates/ponto/bater_ponto_v2.html:53-56,104` |
   | Selfie capturada | Obtida em execução por câmera/arquivo/canvas. `app/templates/ponto/bater_ponto_v2.html:235-267` |
   | GPS atual da pessoa | Campos inicialmente vazios, preenchidos por geolocalização em execução. `app/templates/ponto/bater_ponto_v2.html:105-106,175-195` |

   O v2 esconde os demais elementos com `display:none`; isso conserva os dados no DOM. (`app/templates/ponto/bater_ponto_v2.html:148`.)

   **Contraexemplo condicional:** uma resposta HTML de A é efetivamente guardada e corresponde à requisição offline de B no mesmo armazenamento. B recebe identificação, token renderizado, posto e estado de A. Isso não inclui automaticamente a selfie ou o GPS inseridos depois pelo JavaScript: o SW grava o clone da resposta HTTP. (`app/static/sw.js:73-74`; `app/templates/base.html:316`; `app/templates/core/_copiloto.html:17`; `app/templates/ponto/bater_ponto_v2.html:103-104,139-140,175-180,235-267`.)

   A condição “HTML efetivamente guardado” precisa ser demonstrada: navegação normal não faz essa escrita. Exposição do token também não demonstra autenticação como A. (`app/static/sw.js:45-50`; `app/api/views.py:398-402`.)

6. **iOS Safari e APIs opcionais — certeza alta sobre o código; suporte real [nao verificado].**

   Existe listener de Background Sync para `sync-batidas`, mas não há chamada que registre `sync.register(...)` nos caminhos auditados. O listener delega a páginas abertas; sem cliente, pede ao usuário que abra o app por notificação, sem transmitir a fila. (`app/static/sw.js:112-132`; `app/static/pwa.js:11-31,107-124,320-346`.)

   Os disparadores efetivos de envio estão na página: boot online, `online` e mensagem do SW. Não há implementação de Periodic Sync nesses caminhos. (`app/static/pwa.js:15-16,320-328`; `app/static/sw.js:112-132`.)

   `storage.persisted()` e `persist()` são opcionais, protegidos por teste de existência e `try/catch`; ausência, negativa ou exceção não bloqueia a fila nem cria uma garantia alternativa de retenção. Sem Service Worker, o registro retorna `null`, mas a inicialização da página continua. (`app/static/pwa.js:11-12,323-328,436-443`.)

   **Contraexemplo:** sem Background Sync, com todas as páginas fechadas, os disparadores implementados não enviam a fila. Ao abrir uma página elegível online, o boot tenta sincronizar. Retenção da fila até essa abertura e comportamento de suspensão/expurgo em uma versão concreta do Safari: **[nao verificado]**. (`app/static/pwa.js:126-128,323-328`; `app/static/sw.js:116-127`.)

7. **Selo `test_sw_offline_fallback.py` — certeza alta sobre o alcance estático.**

   O selo executa o `sw.js` fonte em Node/VM, com rede, cache e eventos simulados. Testa três entradas: navegação em `/escala/?app=1`, `/ponto/bater/` com modo `cors`, e outro GET de API. (`app/core/tests/test_sw_offline_fallback.py:23-45,62-74`.)

   Suas nove combinações cobram:

   - Sem rede e sem cache: `respondWith` recebe `Response` do tipo `error`. (`app/core/tests/test_sw_offline_fallback.py:79-87`.)
   - Sem rede e com cache: corpo `DO-CACHE`. (`app/core/tests/test_sw_offline_fallback.py:89-94`.)
   - Com rede e cache: corpo `DA-REDE`. (`app/core/tests/test_sw_offline_fallback.py:96-101`.)

   **Não prova alimentação do cache:** `caches.match` ignora a chave e devolve uma resposta predeterminada; `put` e `addAll` são operações vazias; instalação e ativação não são disparadas. (`app/core/tests/test_sw_offline_fallback.py:33-45`.)

   **Mutantes que passariam, por inferência estática:**

   | Mutante | Por que escapa |
   |---|---|
   | Remover os `cache.put` | O mock não observa escrita. `app/core/tests/test_sw_offline_fallback.py:37` |
   | Trocar a chave de `caches.match` por uma errada | O mock não examina argumentos. `app/core/tests/test_sw_offline_fallback.py:37` |
   | Esvaziar o pré-cache ou alterar sua instalação | O harness só dispara `fetch`. `app/core/tests/test_sw_offline_fallback.py:36-45` |
   | Alterar `CACHE_NAME` ou quebrar limpeza de versões | Nenhuma asserção sobre nomes/ativação. `app/core/tests/test_sw_offline_fallback.py:37-45,79-101` |
   | Manter navegação sem gravar HTML | É justamente o comportamento atual; “cache presente” é fornecido artificialmente. `app/static/sw.js:45-50`; `app/core/tests/test_sw_offline_fallback.py:33,91` |

   Também não cobre POST, exclusões de login/logout, ramo estático, respostas HTTP de erro, fila, identidade, concorrência ou Safari: as entradas são GET e os casos se limitam aos três sítios e respostas simuladas. (`app/core/tests/test_sw_offline_fallback.py:39,44,62-65,79-101`.)

   **Resultado executado do selo e dos mutantes: [nao verificado].** A tentativa de execução encontrou ausência de `node` neste ambiente; o próprio selo exige esse executável. (`app/core/tests/test_sw_offline_fallback.py:69-72`.)

| Item | Sítio | Escritor | Leitor | Contraexemplo | Certeza |
|---|---|---|---|---|---|
| 1 | `app/static/sw.js:45-50` | Navegação não escreve | Fallback global | Visita online seguida de recarga offline não encontra HTML | Alta, condicional |
| 2 — versão/cache | `app/core/views.py:524-572`; `app/static/sw.js:12-30,59,74` | Servidor gera build; SW grava | `caches.match`; ativação | Busca não é limitada ao cache atual | Alta |
| 2 — fila/config | `app/static/pwa.js:48-183` | Gravador offline; cálculo do gap | Sincronizador; relógio; indicador | Horário corrigido fica local; bruto é enviado | Alta |
| 2 — timestamp | `app/api/views.py:536-539,682-695`; `app/ponto/registro_batida.py:157-159` | Cliente e escritor de Batida | APIs, `hora_do_aparelho`, contador e template DP | Tela DP filtra origem diferente da API atual (`app/ponto/views.py:441-446`) | Alta |
| 2 — recarga | `app/templates/ponto/bater_ponto_v2.html:305-310` | Eventos atualizam `_vistoEm` | Callback de visibilidade | Voltar após 5min1s recarrega foto ainda volátil | Alta, condicional |
| 3 | `app/static/pwa.js:113-157`; `app/api/views.py:402` | A grava sem identidade | Sessão atual de B | Dados de A processados como B | Alta, condicional |
| 4 — reenvio | `app/templates/ponto/bater_ponto_v2.html:335-356`; `app/static/pwa.js:126-179` | Online e fila | API | Resposta perdida ou duas leituras concorrentes | Alta, condicional |
| 4 — persistência | `app/api/views.py:557-562`; `app/ponto/models.py:106` | `registrar_batida` | Filtros por horário e chave exata | Bruto >48h e retry após seis minutos muda a chave efetiva | Alta, condicional |
| 5 | `app/templates/base.html:316`; `app/templates/ponto/bater_ponto_v2.html:103,140` | Renderização autenticada; eventual `cache.put` | Outro cliente com cache correspondente | HTML de A expõe identificação, token e posto a B | Alta sobre conteúdo; ocorrência **[nao verificado]** |
| 6 | `app/static/sw.js:112-132`; `app/static/pwa.js:320-328,436-443` | Página registra pendências | Página sincroniza; persistência opcional | Sem página aberta, não há envio implementado da fila | Alta sobre código; Safari **[nao verificado]** |
| 7 | `app/core/tests/test_sw_offline_fallback.py:33-45,79-101` | Mocks fornecem respostas | Asserções do selo | Cache nunca alimentado ainda satisfaz o selo | Alta estática; execução **[nao verificado]** |

## 05/11 15:20 · sessao 2081d9dfcf94 · - · - · origem=Ronald (tmux)

A hipótese pedida com irmã respondida `HH:MM` não funciona: `carimbar(..., resposta='validar')` preenche `validada_em` quando há texto, e o C6 devolve morta antes de examinar a divergência da luz (`app/chamados/models.py:2576`, `app/chamados/models.py:2580`, `app/chamados/juizes.py:1078`).

Para uma recusa real, use irmã muda, sem resposta, cuja célula continua acusando: ela não recebe validação e o C6 retorna viva pelo veredito (`app/chamados/models.py:2576`, `app/chamados/juizes.py:1123`, `app/chamados/juizes.py:1125`).

O helper chamado diretamente duas vezes com essa irmã produz duas linhas: a irmã recusada continua satisfazendo `materializada_em IS NULL`, a ausência existente é devolvida sem nova criação, e cada passagem chama `evento` sem deduplicação (`app/chamados/services/materializacao.py:528`, `app/chamados/services/materializacao.py:546`, `app/chamados/services/materializacao.py:548`, `app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:299`). Isso contrasta com o “Idempotente” da função e com “2x = mesmo estado, 1 linha de trilha, 0 duplicata” (`app/chamados/services/materializacao.py:507`, `CLAUDE.md:175`).

Contudo, **a ponta real não permite reenvio sequencial do mesmo escape**: o próprio escape `ATESTADO` recebe validação na primeira passagem e morre; API e web consultam o C6 e recusam a pergunta morta antes de entrar no core (`app/chamados/models.py:2576`, `app/chamados/models.py:2580`, `app/chamados/juizes.py:1078`, `app/api/views.py:2631`, `app/chamados/views.py:1242`). O core isoladamente aceita a repetição, mas o teste existente declara expressamente que decidir se reenvio é novo evento ou repetição ainda é regra de negócio pendente (`app/chamados/tests/test_porta_resposta_pergunta.py:27`, `app/chamados/tests/test_porta_resposta_pergunta.py:30`).

Conclusão para consolidação: duplicação por chamada direta do helper é demonstrável estaticamente; um reenvio sequencial do mesmo escape pela API/web não é contraexemplo válido. Corrida entre duas requisições é possibilidade sem execução observada [não verificado]. Toca dinheiro: não diretamente; a divergência demonstrada está na quantidade de registros de auditoria (`app/chamados/services/materializacao.py:548`).

## 05/11 15:20 · sessao cdb3324b57b6 · - · - · origem=Ronald (tmux)

Mapa do item 5, separando leitura demonstrada de violação:

| Campo | Quem escreve | Quem lê no caminho revisado | Efeito |
|---|---|---|---|
| `situacao` | `app/colaboradores/services/desligamento.py:104`, reativação em `:210` | `app/chamados/services/fabrica_da_celula.py:91`; `app/chamados/management/commands/disparar_perguntas_competencia.py:169` | O estado atual cala perguntas inclusive de dias antigos. Há lastro explícito de domínio do RH em `disparar_perguntas_competencia.py:123`. |
| `data_demissao` | `app/colaboradores/services/desligamento.py:105`; reativação limpa em `:211` | `app/escala/services/leitor_celula.py:351`, chamado por `app/chamados/services/declaracao_texto.py:82`, via `app/chamados/services/disputa_emissao.py:827` e `app/chamados/services/fabrica_da_celula.py:49` | A fronteira atual pode transformar o dia passado em `fora_vinculo` (`leitor_celula.py:366`). A reativação elimina essa fronteira. Proibição temporal específica nos anexos: [não verificado]. |
| `posto` | `app/colaboradores/views.py:1385`; vínculo recebe posto em `app/colaboradores/services/vinculo.py:266` | Leitura direta pelas funções alteradas: [não verificado]. | A transferência também troca `empresa`, efeito diretamente relevante em `app/colaboradores/views.py:1386`. |
| `empresa` | `app/colaboradores/views.py:1386` | `app/chamados/services/fabrica_da_celula.py:188`; `app/chamados/services/vigia_fio.py:807`; universo do vigia em `app/ponto/services/vigia_de_hora.py:21` | Empresa atual decide janela/entrega de competência passada e, portanto, pauta versus pergunta. A regra exigindo empresa histórica não está explícita nos anexos: [não verificado]. |
| `regime_trabalhista` | `app/colaboradores/views.py:512`, `:523`, pela porta `app/colaboradores/services/estrutura.py:17` | Leitor financeiro em `app/core/regua_cct.py:353` | Efeito específico na seleção de motivos do diff: [não verificado]. |
| `isento_ponto` | `app/colaboradores/views.py:176` | `app/ponto/services/vigia_de_hora.py:117`; `app/escala/services/leitor_celula.py:349`; `app/chamados/services/declaracao_texto.py:83` | Flag atual exclui ontem do vigia e cala marcos antigos no caminho dos motivos. O juiz declara a flag como fonte em `app/core/constants.py:13`; exigência de vigência histórica nos anexos: [não verificado]. |
| vínculo | `app/colaboradores/services/vinculo.py:265`, `:278` | `app/ponto/services/cartorio.py:841` → `app/escala/servico_jornada.py:55` → `app/escala/alimentacao.py:269` | Consulta geradora e vigência do dia; não escolhe automaticamente vínculo atual. Inativo hoje continua respondendo pelo passado se cobrir a data (`alimentacao.py:282`). Sem geradora, usa candidatos que cobrem a data e desempate por `ativa` (`:285`). |

Contraexemplo sintético de dependência do cadastro atual: célula limbo de 15/10 na empresa A, cuja competência foi entregue; transferência em 01/11 para B, sem entrega. A transferência troca a empresa atual (`app/colaboradores/views.py:1386`); `limbo_vai_a_pauta` passa B ao juiz (`app/chamados/services/fabrica_da_celula.py:188`), que consulta a entrega de B (`app/chamados/services/vigia_fio.py:807`). O resultado muda de pauta para pergunta, preservada a célula. Ocorrência real e obrigação de consultar A: [não verificado].

Achado adicional: o lote pula qualquer célula de ontem aceita por `_ontem_em_janela`, inclusive `trabalha=None` (`app/chamados/management/commands/disparar_perguntas_competencia.py:110`); o vigia efetivo só candidata `trabalha=True` (`app/ponto/services/vigia_de_hora.py:21`). Assim, limbo noturno com vínculo 22–06, ontem, visto às 06:35, fica fora dos dois caminhos nessa rodada, embora a regra diga que limbo cobra (`app/docs/JUIZES.md:55`). A contagem só reaparece depois da janela ou no dia seguinte (`disparar_perguntas_competencia.py:101`).

## 05/11 15:20 · sessao 435e1a587531 · - · - · origem=Ronald (tmux)

Matriz temporal: o diff altera apenas docstrings (`app/ponto/services/lastro.py:219-228`, `:370-375`); os caminhos abaixo já existem no código executável (`app/ponto/services/lastro.py:239-317`, `app/chamados/models.py:623-659`).

| Campo | Caminho de leitura e decisão sobre passado | Escritor circunscrito |
|---|---|---|
| `situacao` | Não participa diretamente do filtro do lastro: fila cobra status do **chamado** e silêncio, sem filtrar situação da pessoa (`app/chamados/services/painel.py:153-155`, `:186-188`; `app/ponto/services/lastro.py:239-244`). No escritor da ata/veredito, a situação **atual** impede emissão para dia julgado: `c.situacao != 'desligado'` (`app/ponto/services/cartorio.py:1696-1697`, `:1762`). | Desligamento atribui `desligado` (`app/colaboradores/services/desligamento.py:104-108`); reativação atribui `ativo` (`:210-214`), persistidos pela porta (`app/colaboradores/services/colaborador.py:65-68`). |
| `data_demissao` | Não é filtro direto de `lastro.julgar` (`app/ponto/services/lastro.py:239-264`). No escritor da ata/veredito, `c.data_demissao` atual alimenta o cadastro do dia (`app/ponto/services/cartorio.py:1094-1097`); sem vínculo, compara `data > data_demissao` para decidir `sem_vinculo_em_dia_de_trabalho` (`app/ponto/supra_juiz.py:193-197`). | Desligamento grava a data (`app/colaboradores/services/desligamento.py:105-108`); reativação apaga-a (`:211-214`). |
| `posto` | Caminho alcançável pelo gate **no fallback sem célula/ata conclusiva**: `premissa_regularizada` → `espurios` → `fatos_do_dia` → `lugar_do_colab` (`app/chamados/models.py:1249-1255`; `app/chamados/services/feriado_reavaliar.py:30-38`; `app/ponto/precedencia.py:256`, `:286-289`). O município vem do `colaborador.posto` atual, sem data (`app/core/feriados.py:113-114`). | Vincular posto (`app/colaboradores/views.py:327-329`); transferência muda posto e empresa (`:1385-1388`); porta persiste (`app/colaboradores/services/colaborador.py:65-68`). |
| `empresa` | Caminho direto alcançável: `lastro.julgar` → `reconciliaria` → `_gate_pergunta_viva` → `_vivas_entre` (`app/ponto/services/lastro.py:316`; `app/chamados/models.py:627`, `:651`, `:402`, `:366`). `_vivas_entre` decide se a competência do `data_evento` foi entregue usando `p.disputa.colaborador.empresa` atual (`app/chamados/models.py:347`; `app/chamados/services/vigia_fio.py:807-813`). C6 decide tranca pela empresa atual e data antiga (`app/chamados/juizes.py:929-942`, `:1082-1083`). O corte também é o cadastro atual da empresa (`app/ponto/janelas.py:50-51`, `:60-63`). | Transferência cross empresa (`app/colaboradores/views.py:1385-1388`); corte e regime da empresa são persistidos por `atualizar_empresa` (`app/colaboradores/views.py:521-523`; `app/colaboradores/services/estrutura.py:17-20`). |
| `regime` | Não identifiquei leitura de `regime_trabalhista` na cadeia direta lastro → gates → C6 nem no cartório/grade auditados; generalização para todo o sistema [não verificado]. `juizes.py::folha_desconta_atraso` é outra função, fora dessa cadeia (`app/chamados/juizes.py:1558-1571`). | Formulário aceita o campo (`app/colaboradores/views.py:512-515`) e o envia à porta (`:521-523`; `app/colaboradores/services/estrutura.py:17-20`). |
| `isento_ponto` | Não é condição direta de `lastro.julgar` (`app/ponto/services/lastro.py:239-317`). No **escritor upstream**: grade consulta o flag atual, sem data, e aplica isenção a cada dia montado (`app/escala/utils.py:903-910`); cartório chama a grade (`app/ponto/services/cartorio.py:1126`). Também passa flag atual ao classificador (`:1419`), que retorna sem códigos quando isento (`app/ponto/supra_juiz.py:218-221`), e ao status gravado (`app/ponto/services/cartorio.py:1802-1807`). | Passe livre grava pelo cadastro (`app/colaboradores/views.py:173-176`; `app/colaboradores/services/colaborador.py:47-53`). |
| vínculo atual | O vínculo do **dia** segue geradora e vigência: carga da célula/geradora (`app/escala/servico_jornada.py:46-58`); geradora escolhida por PK e limites (`app/escala/alimentacao.py:269-284`); sem geradora, candidatos que cobrem a data, desempate por ativa/início (`:285-286`, `:214-216`). O gate usa esse caminho para órfãs (`app/chamados/juizes.py:480-487`); não escolhe simplesmente `ativa=True` atual. | Porta atualiza/cria vínculo (`app/colaboradores/services/vinculo.py:265-288`) e encerra vigência (`:797-799`). |

Origens adicionais das entradas: o cartório monta `ata.lampadas` a partir da grade (`app/ponto/services/cartorio.py:701-704`) e grava ata/veredito (`:1800-1807`); o emissor renova `contexto_json.marcos_faltantes` (`app/ponto/management/commands/detectar_ausencias.py:543-559`); lastro lê essa lista e `ata.lampadas` (`app/ponto/services/lastro.py:75-78`, `:89-94`, `:273`, `:289-292`).

O código prova leituras do cadastro atual; não prova violação das LEIS/CORTES anexos nem cadastro real afetado: o anexo `JUIZ-2.leis` e o dado de produção fornecido não contêm as linhas necessárias [não verificado]. Não há contraexemplo de produção sustentado, regra executável alterada pelo patch ou base para atribuir a ele esses comportamentos pré-existentes (`app/ponto/services/lastro.py:219-228`, `:370-375`).

## 05/11 15:20 · sessao 511539d4e84a · - · - · origem=Ronald (tmux)

1. **Contraexemplo sintético: limbo exportado com chamado vivo recebe pergunta pelo fio mudo.** Cadastro ativo, célula de 15/10/2026 com `trabalha=None`, ata de trabalho E08 acesa/S17 apagada, chamado vivo, nenhuma pergunta e competência entregue; execução em 20/10. O evento desviaria os motivos para Pauta DP e devolveria `[]` (`app/chamados/services/fabrica_da_celula.py:164`, `:171`, `:188`). O vigia também reconhece esse destino (`:199`). Entretanto, o fio mudo só verifica parcial, motivo, situação e dedup; depois chama `perguntar` (`app/chamados/management/commands/disparar_perguntas_competencia.py:154`, `:158`, `:169`, `:211`, `:280`). Não há guarda de exportação nos escritores seguintes: `app/chamados/services/disputa_emissao.py:389`; `app/chamados/models.py:2417`, `:2465`. **É inconsistência entre leitores, preexistente ao diff**; a autorização específica “limbo exportado → Pauta” está em `app/docs/PROMPTS.md:1719`, mas sua presença nos LEIS/CORTES anexados é **[não verificado]**. Os anexos asseguram apenas que limbo cobra: `app/docs/JUIZES.md:55`.

2. **Exclusão de ontem usa predicado menos restritivo que a seleção do vigia de hora.** O lote pula toda célula de ontem para a qual `_ontem_em_janela` seja verdadeira (`app/chamados/management/commands/disparar_perguntas_competencia.py:110`), embora prometa pular enquanto o vigia de hora a candidata (`:94`). O vigia só seleciona `trabalha=True` (`app/ponto/services/vigia_de_hora.py:21`) e ainda exclui desligados/isentos (`:117`). Cadastro sintético ativo, não isento, célula de ontem limbo, vínculo noturno 22–06: hoje às 06:35, `_ontem_em_janela` retorna verdadeiro (`:47`, `:55`); o lote omite o déficit, mas o vigia de hora nem seleciona a célula. O alarme fica adiado até outra rodada que a alcance (`app/chamados/management/commands/disparar_perguntas_competencia.py:101`). Nenhum caso real correspondente foi fornecido **[não verificado]**.

3. **Regra nova sem lastro específico identificado nos anexos:** `--alarme` deixa de contar hoje por `data__lt=_hoje`, e `--comp-corrente` ganha piso de anteontem (`app/chamados/management/commands/disparar_perguntas_competencia.py:101`). L-060 e L-101 tratam do papel do cron, sem estabelecer esses recortes (`app/docs/LEIS.md:94`, `:127`); o fundamento específico está nos comentários do código (`app/chamados/management/commands/disparar_perguntas_competencia.py:91`). Autoridade específica nos anexos para esses recortes: **[não verificado]**. Evitaria chamar isso automaticamente de violação: outro vigia já documenta exclusão de hoje por corte próprio (`app/chamados/services/vigia_fio.py:61`).

## 05/11 15:20 · sessao 76efbac6ac4f · REVISAO-N1-ALIMENTACAO · verificador · origem=Code

Há achados nas dependências lidas e um selo que não detecta a reversão da cura. Alteração de regra de negócio introduzida pelo diff: `[nao verificado]`. Ocorrência em cadastro real de produção e valor financeiro afetado: `[nao verificado]`.

**1. Quem escreve e quem lê os valores envolvidos**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `itens`, `entram`, `fora_por_motivo`, `furo` | `dias_que_seguram` recebe a classificação e monta os agregados: `app/folha/fora_da_folha.py:119`, `app/folha/fora_da_folha.py:122` | `fora_da_folha`: `app/folha/fora_da_folha.py:162` |
| `colabs` e `dias` do universo extraído | Carga dos colaboradores e `_dias_que_retem`: `app/folha/fora_da_folha.py:131`, `app/folha/fora_da_folha.py:137` | Folha: `app/folha/fora_da_folha.py:163`; vigia: `app/chamados/services/vigia_fio.py:559` |
| Datas retidas, motivos, `celula_id`, veredito | `motivos_retencao_celula` monta cada dia: `app/folha/export.py:1200` | `_dias_que_retem`: `app/folha/fora_da_folha.py:98`; apresentação da folha: `app/folha/fora_da_folha.py:175` |
| `alim['instancias']` | Sementes em `app/chamados/juizes.py:1244`; captura posterior em `app/chamados/juizes.py:992`; busca subsidiária em `app/chamados/juizes.py:1262` | Injeção em `disputa.colaborador`: `app/chamados/juizes.py:990`; `_no_app`: `app/chamados/juizes.py:1263` |
| Sementes fornecidas ao mapa | Retidos da folha: `app/folha/fora_da_folha.py:166`; colaborador recebido: `app/folha/fora_da_folha.py:218`; carga do vigia: `app/chamados/services/vigia_fio.py:577` | `alimentar_portas`: `app/chamados/juizes.py:1244` |
| `verbo`, `fila_ids`, `fila_cel`, `vivas` | Partição, fila e juiz C6: `app/chamados/juizes.py:1232`, `app/chamados/juizes.py:1235`, `app/chamados/juizes.py:1247` | Porta: `app/chamados/juizes.py:1321`; vigia: `app/chamados/services/vigia_fio.py:579`, `app/chamados/services/vigia_fio.py:584` |
| `no_app`, `_tranca_memo`, `_pv_memo` | `app/chamados/juizes.py:1264`, `app/chamados/juizes.py:969`, `app/chamados/juizes.py:845` | Porta: `app/chamados/juizes.py:1325`; tranca: `app/chamados/juizes.py:935`; vitalidade: `app/chamados/juizes.py:834` |
| Universo e competência do vigia | Chave `(colaborador, data)` com `setdefault`: `app/chamados/services/vigia_fio.py:561` | Classificação por dia: `app/chamados/services/vigia_fio.py:608`; contador: `app/chamados/services/vigia_fio.py:562` |

Fora do diff, o retrato lê `itens`, `colabs`, dias, datas e grupo, transformando-os em suas linhas; o comando persiste essas linhas em `RetratoFolha` (`app/folha/retrato_folha.py:1157`, `app/folha/retrato_folha.py:1177`, `app/folha/management/commands/lavrar_retrato_folha.py:74`). Outro consumidor de `_alimentar` é a pauta de discordantes (`app/ponto/services/pautas_dos_discordantes.py:75`).

**2. Contraexemplo concreto contra a regra**

Cenário **sintético**: colaborador de empresa com `dia_inicio_competencia=10`; consulta em **05/11/2026** sobre **15/09/2026**. A competência deve respeitar o corte da empresa (`app/docs/LEIS.md:77`, `app/docs/JUIZES.md:71`).

Preservando o intervalo que o próprio helper declara — anterior mais corrente, incluindo a véspera —, as janelas dessa empresa abrangem **09/09 a 09/11**. Entretanto, `_dentro_da_janela_viva` chama ambas sem empresa e recebe o padrão 21: intervalo **20/09 a 20/11**. Assim, para **15/09**, o helper retorna **False**, enquanto o corte cadastrado exige **True** (`app/chamados/juizes.py:561`, `app/chamados/juizes.py:580`, `app/chamados/juizes.py:586`, `app/ponto/janelas.py:50`).

Essa diferença foi reproduzida em memória executando as funções da cópia. É dependência **fora do diff**, alcançada por `pergunta_viva` (`app/chamados/juizes.py:1142`). Resultado final de uma pergunta concreta ou ocorrência em produção: `[nao verificado]`; os degraus seguintes ainda podem determinar sua vitalidade (`app/chamados/juizes.py:1152`, `app/chamados/juizes.py:1159`).

**3. Regra de negócio alterada sem lastro**

Não identifiquei predicado novo de negócio na extração: permanecem `classificar_export`, `_dias_que_retem`, `pergunta_viva` e a porta compartilhada (`app/folha/fora_da_folha.py:119`, `app/folha/fora_da_folha.py:137`, `app/chamados/juizes.py:1247`, `app/folha/fora_da_folha.py:77`). A porta única corresponde ao corte anexado (`app/docs/CORTES.md:401`, `app/docs/CORTES.md:409`).

Equivalência de comportamento para todos os cadastros, inclusive instâncias com valores ou memos previamente carregados: `[nao verificado]`.

**4. Selo que não morde**

`test_MORDE_o_universo_e_o_mesmo_do_fora_da_folha` monta o esperado com `fora_da_folha` e compara somente universo/datas, descartando as contagens `_n` e `_p` (`app/chamados/tests/test_n1_alimentacao_unica.py:115`, `app/chamados/tests/test_n1_alimentacao_unica.py:117`).

Com o caminho anterior restaurado, o vigia volta a consumir justamente esses `colabs/datas`; portanto, a igualdade permanece por construção. Esse teste verifica invariância da saída, mas não detecta alimentação duplicada (`app/chamados/tests/test_n1_alimentacao_unica.py:118`). Execução com a cura revertida: `[nao verificado]`.

Isso não significa ausência de cobertura: outros testes afirmam uma alimentação, uma consulta ao app e identidade da instância (`app/chamados/tests/test_n1_alimentacao_unica.py:92`, `app/chamados/tests/test_n1_alimentacao_unica.py:97`, `app/chamados/tests/test_n1_alimentacao_unica.py:137`).

**5. Cadastro de hoje decidindo passado**

| Campo | Escritor localizado | Leitura temporal comprovada |
|---|---|---|
| `situacao` | Desligamento: `app/colaboradores/services/desligamento.py:104` | Situação atual e vínculo `ativa=True` determinam `sem_ancora_12x36` nas travas da competência solicitada: `app/folha/export.py:787`, `app/folha/export.py:813`. A pauta histórica também filtra atualmente ativos: `app/ponto/services/pautas_dos_discordantes.py:70`. |
| `data_demissao` | Agendamento: `app/colaboradores/services/desligamento.py:85`; desligamento: `app/colaboradores/services/desligamento.py:105` | Qualquer data preenchida produz `rescisao_modulo_proprio`, sem comparar com a competência: `app/folha/export.py:840`. A retenção também recorta o período pela data atual: `app/folha/export.py:1182`. |
| `posto` | Transferência: `app/colaboradores/views.py:1385` | O calendário alcançado pelo vigia fornece o município do posto atual ao carregamento dos fatos do período: `app/core/feriados.py:113`, `app/colaboradores/services/calendario.py:240`, `app/colaboradores/services/calendario.py:485`. |
| `empresa` | Transferência: `app/colaboradores/views.py:1386` | Seleciona fechamentos antigos pela empresa atual: `app/folha/export.py:767`; define janela de `o_que_segura`: `app/folha/fora_da_folha.py:212`; define a tranca da pergunta antiga: `app/chamados/juizes.py:936`. |
| `regime_trabalhista` | Formulário e porta: `app/colaboradores/views.py:512`, `app/colaboradores/services/estrutura.py:17` | `regua_para` recebe data passada, mas lê empresa e regime atuais; essa régua alcança a classificação do export: `app/core/regua_cct.py:352`, `app/folha/export.py:978`. |
| `isento_ponto` | Passe livre: `app/colaboradores/views.py:176` | A flag atual esvazia retenção de células passadas e eventos do fechamento: `app/core/constants.py:18`, `app/folha/export.py:1174`, `app/folha/export.py:407`. |
| vínculo atual | Criação/alteração: `app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:278` | A trava de aprovação usa `ativa=True`: `app/folha/export.py:788`. Já ciclo e praça consultam vínculo **do dia**, portanto não cabe estender a acusação a esses leitores: `app/folha/export.py:348`, `app/core/regua_cct.py:329`. |

Exemplo temporal **construído**: cadastro admitido em janeiro, fechamento de **09/2026**, célula retentora em **01/09** e demissão em **25/10**. Consultar setembro passa a retornar `rescisao_modulo_proprio`; o colaborador deixa de entrar no universo que seleciona exclusivamente `furo_espelho` (`app/folha/export.py:840`, `app/folha/fora_da_folha.py:128`). O comentário limita a exclusão à demissão **na janela** (`app/folha/export.py:835`); regra expressa nos LEIS/CORTES anexados que resolva essa demissão posterior: `[nao verificado]`.

Essas leituras atuais estão comprovadas; sua presença, isoladamente, não prova violação de negócio. A isenção, por exemplo, declara a flag como fonte (`app/core/constants.py:15`). Cadastro real afetado, histórico necessário e montantes: `[nao verificado]`.

**VEREDITO: ACHADO**

1. `app/chamados/juizes.py:561` — **item 2**: corte da empresa omitido na janela de vitalidade; herdado, fora do diff. **Toca dinheiro: nao**, no retorno demonstrado (`app/chamados/juizes.py:586`).
2. `app/chamados/tests/test_n1_alimentacao_unica.py:117` — **item 4**: selo de universo preserva igualdade com a cura revertida; execução da reversão `[nao verificado]`. **Toca dinheiro: nao**.
3. `app/folha/export.py:840`, `app/folha/export.py:407` — **item 5**: cadastro atual decide exclusão e emissão de eventos de competência passada; alteração introduzida pelo diff `[nao verificado]`. **Toca dinheiro: sim** (`app/folha/export.py:428`).

## 05/11 15:20 · sessao 24780c2bf7da · - · - · origem=Ronald (tmux)

Leituras de cadastro atual confirmadas, sem prova de ocorrência em produção:

| Campo | Escritor | Leitor alcançado pelo fluxo |
|---|---|---|
| `empresa`/`posto` | Transferência atribui ambos e salva: `app/colaboradores/views.py:1385-1388`. | C6 consulta tranca da **empresa atual** para `data_evento`: `app/chamados/juizes.py:929-942`; chamada anterior às respostas: `:1080-1083`. |
| `data_demissao` | Desligamento atribui/salva: `app/colaboradores/services/desligamento.py:104-108`; reativação apaga: `:210-214`. | `grade_da_celula` lê cadastro atual e transforma dia em `fora_vinculo`: `app/escala/services/leitor_celula.py:351-372`. Materializador lê essa grade para régua e tipo: `app/chamados/services/materializacao.py:227-241`, `:281-297`. |
| `isento_ponto` | Porta aceita campo na operação cadastro: `app/colaboradores/services/colaborador.py:34-35`, `:47-53`. | Flag atual, sem data: `app/core/constants.py:13-18`; grade aplica a cada dia passado: `app/escala/services/leitor_celula.py:347-349`, `:412-413`. |
| `situacao` | Desligamento/reativação nos escritores acima. | Renascimento de chamado histórico depende de situação atual: `app/chamados/models.py:944-967`; reconciliador também barra não ativo: `app/chamados/reconciliador.py:783-790`. |
| `regime` | Atualização de empresa: `app/colaboradores/services/estrutura.py:17-20`. | Fábrica lê empresa/regime atuais: `app/core/regua_cct.py:352-357`; alcance desse cálculo pelo miolo alterado F2-FILA [não verificado]. |
| vínculo | Porta de vínculo em `app/colaboradores/services/vinculo.py`. | Caminho examinado usa geradora e vigência do **dia**, não simplesmente vínculo atual: `app/escala/servico_jornada.py:38-58`, `app/escala/alimentacao.py:269-286`; conforme lookup `app/docs/JUIZES.md:98-101`. |

Contraexemplos sintéticos úteis:

1. **Troca A→B:** pergunta de 15/09/2026, competência trancada em A e aberta em B, colaborador hoje em B. `dia_da_pergunta_trancado` consulta B e perde a tranca de A (`app/chamados/juizes.py:936-945`). Ocorrência produtiva e regra anexada exigindo empresa histórica [não verificado]. Dinheiro direto: não demonstrado; alteração de vitalidade de pergunta: sim (`:1082-1083`).

2. **Demissão atual sobre dia histórico:** pergunta de 15/09/2026 com ata contendo marcos 08:00/17:00, resposta default `07:00`; cadastro hoje com demissão 14/09. Grade retorna `fora_vinculo` e régua vazia (`app/escala/services/leitor_celula.py:366-372`, `app/chamados/services/materializacao.py:239-240`); o crivo que recusaria `07:00` deixa de recusar (`:370-377`). Isso alcança também juiz do lastro: `_pergunta_materializada_de_fato` → `_slot_esperado` → `_ts_tipo_do_plantio` (`app/chamados/juizes.py:716`, `:341-345`). Dados produtivos e expectativa jurídica específica para esse cadastro [não verificado].

Não trataria essas leituras como regressões introduzidas por F2-FILA: as linhas correspondentes são externas às alterações fornecidas; existência de infração temporal às LEIS/CORTES anexadas exige distinguir leitura atual intencional de regra histórica, ainda [não verificado].

## 05/11 15:19 · sessao 1677a706abf6 · - · - · origem=Ronald (tmux)

Mapa das entradas do lastro, limitado às portas e consumidores pertinentes:

| Entrada | Escritores | Leitores pertinentes |
|---|---|---|
| `contexto_json.marcos_faltantes` | Emissão por `detectar_ausencias` → `ChamadoColaborador.abrir` (`app/ponto/management/commands/detectar_ausencias.py:688`, payload em `:700`); atualização por `_espelhar_marcos` (`app/ponto/management/commands/detectar_ausencias.py:558`) → `renovar_contexto`, que atribui e salva (`app/chamados/services/contexto.py:81`, `:101`). Cartório fornece apagados ou marcos parciais à emissão (`app/ponto/services/cartorio.py:1742`, `:1780`) e atualiza chamados existentes pelo mesmo espelhador (`app/ponto/services/cartorio.py:1798`). Reconstruções: `app/chamados/management/commands/nomear_marco_por_lampada.py:109`, `app/ponto/management/commands/reconciliar_grade.py:473`; reconciliador monta o contexto lendo apagados (`app/chamados/reconciliador.py:673`). | Lastro normaliza hora/tipo (`app/ponto/services/lastro.py:75`); suprajuiz (`app/ponto/supra_juiz.py:129`), emissão de perguntas (`app/chamados/services/disputa_emissao.py:982`) e autoridade do marco cobrado (`app/chamados/juizes.py:519`). |
| `CelulaDia.ata.lampadas.{hora,tipo,acesa,luz}` | `lampadas_do_dia` monta payloads (`app/ponto/services/cartorio.py:565`, `:638`, `:677`, `:688`); `ata_do_dia` agrega (`app/ponto/services/cartorio.py:702`). Dispensa por ausência/justificativa escreve `acesa=True`, `luz=None` (`app/ponto/services/cartorio.py:774`). Teto temporal escreve `acesa=None`, `pendente=True` (`app/ponto/services/cartorio.py:862`). Cartório entrega ata à lavra (`app/ponto/services/cartorio.py:1799`); método do modelo delega (`app/escala/models.py:1816`); porta atribui e persiste (`app/ponto/portas/celula.py:741`, `:743`, `:768`, `:775`). | `_lampada`, `marcos_acesos` e `julgar` (`app/ponto/services/lastro.py:89`, `:170`, `:289`); juiz da resposta compara resposta com `luz` (`app/chamados/juizes.py:1053`, `:1067`). |
| `ChamadoColaborador.status_local` | `_gravar_estado` atribui e salva (`app/chamados/models.py:195`, `:222`, `:279`); fechamento do lastro chega por `reconciliar` (`app/chamados/models.py:617`, `:620`). | Lastro relê antes de fechar (`app/ponto/services/lastro.py:338`); decisão pelo motor (`app/chamados/models.py:639`); guarda da pergunta viva (`app/chamados/models.py:400`); juiz da pergunta considera destino encerrado conforme ramo do cálculo (`app/chamados/models.py:313`). |
| `PerguntaDisputa.resposta_colab`, `respondida_em` | `responder_pergunta_disputa_core` escreve resposta/data e salva (`app/chamados/services/disputa_emissao.py:1247`, `:1282`, `:1301`); `carimbar` pode preencher `respondida_em` (`app/chamados/models.py:2576`). | `_pergunta_viva_calc` testa resposta com texto e compara-a à luz (`app/chamados/juizes.py:1092`, `:1104`); comparação lê texto (`app/chamados/juizes.py:1055`). |
| `PerguntaDisputa.validada_em` | `validar_pergunta` (`app/chamados/services/validacao.py:343`) usa `_carimbar_validacao` (`:127`, atribuição/salvamento `:131`). Também existe preenchimento por `PerguntaDisputa.carimbar(resposta='validar')` (`app/chamados/models.py:2580`, `:2598`). Nova resposta limpa validação anterior (`app/chamados/services/disputa_emissao.py:1295`); devolução por recusa limpa e salva (`app/chamados/services/validacao.py:188`). | Consulta das perguntas ancoradas (`app/chamados/models.py:327`); juiz responde morta quando validada (`app/chamados/juizes.py:1078`); guarda usa o juiz para reconhecer vivas (`app/chamados/models.py:351`). |

“Lâmpada acesa” pode significar dispensa, sem marca real, pelo escritor que grava `acesa=True` e `luz=None` (`app/ponto/services/cartorio.py:774`). Essa distinção já existe no código executável e não é alterada pelo patch de docstrings (`app/ponto/services/lastro.py:219`, `:370`).

O teste shell não foi executado; ele cria e remove arquivos temporários (`bin/tests/test_placar_conta_o_retido_do_lastro.sh:595`).

## 05/11 15:19 · sessao 55586364e333 · - · - · origem=Ronald (tmux)

Item 4 confirmado por inspeção: `test_01_MORDE_ata_acusa_e_lei_cala_a_pergunta_nasce` passou a exigir apenas a mensagem `o julgamento deixou 1 pergunta(s)`; o helper roda sem `--apply` e sem `--alarme`. O código anterior do patch já imprimia essa mensagem quando havia uma pergunta planejada, antes do dry-run com rollback. Portanto esse teste passaria com a cura escritor→vigia revertida; não comprova ausência de escrita nem retorno 2. (`app/chamados/tests/test_fabrica_pela_ata.py:32`, `app/chamados/tests/test_fabrica_pela_ata.py:39`; versão anterior: anexo `JUIZ-4A.patch`, sem arquivo local/linha [não verificado]).

Outro exemplo novo: `test_folga_lavrada_nao_e_contada` cria `trabalha=None`, sem chamado, e exige contador zero/rc zero. O filtro anterior `trabalha=True` já excluiria essa célula e produziria a mesma resposta, sem consultar `dia_sem_trabalho`. (`app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:112`; filtro anterior no anexo [não verificado]).

Existe lacuna na combinação **limbo + competência entregue + chamado vivo**: os testes de exportada e os de fio mudo cobrem essas condições separadamente. A fábrica encaminha o limbo entregue para Pauta DP, enquanto o ramo fio mudo verifica parcial, consulta motivos e chama `perguntar`, sem consultar `limbo_vai_a_pauta`. Isso já existia no ramo anterior; ocorrência concreta em produção [não verificado]. (`app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:135`, `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:152`, `app/chamados/services/fabrica_da_celula.py:167`, `app/chamados/management/commands/disparar_perguntas_competencia.py:154`, `app/chamados/management/commands/disparar_perguntas_competencia.py:280`).

Não executei testes: a conclusão sobre reversão é análise do teste e do patch, não resultado medido. [não verificado] [nao verificado]

## 05/11 15:19 · sessao 2081d9dfcf94 · - · - · origem=Ronald (tmux)

A centralização preserva a regra: `VIAS_DE_IDENTIDADE` contém as mesmas quatro vias, e o helper acrescenta apenas `competencia_trancada`; o teste também exige esse conteúdo exato (`app/chamados/models.py:2275`, `app/chamados/models.py:2543`, `app/chamados/tests/test_o261e_f2_fila.py:133`). Não identifiquei regra de negócio nova nessa parte; contraexemplo de produção depende de cadastro ausente no dado fornecido [não verificado].

Escritores e leitores relevantes:

- `admin_decidiu`: escritor `fechar_por_admin`, em `app/chamados/models.py:2511`.
- `duplicata`: escritor `retratar_duplicata`, em `app/chamados/services/fio.py:891`.
- `paridade_recusada`: escritor `_materializar_resposta_hora`, em `app/chamados/services/materializacao.py:891`.
- `fato_ocupado`: escritor `_carimbar_recusa`, em `app/chamados/services/materializacao.py:1066`.
- Os quatro usam `carimbar_identidade`, que valida a fonte única e grava os três campos; a atribuição efetiva de `materializada_em`, `materializada_por_id` e `via_resolucao` ocorre em `_escrever_carimbo` (`app/chamados/models.py:2623`, `app/chamados/models.py:2628`, `app/chamados/models.py:2633`, `app/chamados/models.py:2547`).
- `competencia_trancada` continua sem escrita pelas duas portas: ambas recusam essa via; o legado continua lido pelo C6 e pela reabertura de competência (`app/chamados/models.py:2566`, `app/chamados/models.py:2623`, `app/chamados/juizes.py:1087`, `app/chamados/models.py:2707`).
- Leitores da fonte única: lastro (`app/chamados/juizes.py:655`), C6 (`app/chamados/juizes.py:1112`), SQL `q_vivas` (`app/chamados/models.py:2734`), elegibilidade de desfazer carimbo (`app/chamados/services/relavra_via.py:179`) e auditor de invariantes (`app/chamados/management/commands/auditar_invariantes_chamados.py:195`).

A ausência escreve a nova ação `irma_viva_na_ausencia` quando `carimbar` devolve `False`, incluindo escape, ausência, via e data no JSON (`app/chamados/services/materializacao.py:546`, `app/chamados/services/materializacao.py:548`, `app/chamados/services/materializacao.py:551`). `core.observ.evento` persiste esses valores em `LogAuditoria`; a tela genérica lê os logs e permite filtrar pela ação (`app/core/observ.py:133`, `app/core/observ.py:135`, `app/core/views.py:705`, `app/core/views.py:709`). A escrita é explicitamente best effort, podendo retornar `None`; o materializador não verifica seu resultado (`app/core/observ.py:98`, `app/core/observ.py:139`, `app/chamados/services/materializacao.py:548`).

A nova lista `recusadas` é devolvida, mas o único chamador de produção descarta seu retorno; a trilha permanece sendo o efeito consumível em produção (`app/chamados/services/materializacao.py:566`, `app/chamados/services/disputa_emissao.py:1330`). Isso, isoladamente, não demonstra mudança de resultado de negócio [não verificado].

Sobre selo que não morde: `test_os_leitores_leem_a_fonte` não executa nem inspeciona C6, lastro, SQL ou auditor; verifica somente o modelo e a remoção do nome no módulo `relavra_via`. Reverter apenas os leitores de `juizes.py` para suas listas anteriores preservaria esse teste individual; o selo AST completo, entretanto, encontraria as listas restauradas (`app/chamados/tests/test_o261e_f2_fila.py:129`, `app/chamados/tests/test_o261e_f2_fila.py:133`, `app/chamados/tests/test_o261e_f2_fila.py:135`, `app/chamados/tests/test_o261e_f2_fila.py:106`, `app/chamados/tests/test_o261e_f2_fila.py:114`). Portanto, não apresentaria esse teste individual como falha da proteção conjunta [não verificado].

Outro limite objetivo: o selo de retorno considera suficiente atribuir `ok = q.carimbar(...)`, mesmo sem ler `ok`; sua própria fixture aprova isso. Ele detecta chamada solta, não consumo efetivo do resultado (`app/chamados/tests/test_o261e_f2_fila.py:44`, `app/chamados/tests/test_o261e_f2_fila.py:65`, `app/chamados/tests/test_o261e_f2_fila.py:66`). O teste comportamental da irmã exige a lista e a linha de auditoria, cobrindo a reversão literal da cura nesse materializador (`app/chamados/tests/test_o261e_f2_fila.py:240`, `app/chamados/tests/test_o261e_f2_fila.py:243`, `app/chamados/tests/test_o261e_f2_fila.py:245`).

## 05/11 15:19 · sessao 8c4082aa3aea · - · - · origem=Ronald (tmux)

- **Fila — transição consumida mesmo se o julgamento falhar.** A trilha afirma “re-julgados” antes da execução (`app/ponto/portas/celula.py:299`); exceção por dia é engolida e a porta segue (`app/ponto/portas/celula.py:405`). A próxima escrita já encontra o mês fora do defeito e não repete esse re-julgamento (`app/escala/folgas.py:211`, `app/escala/folgas.py:224`). A impressão normal não inclui o defeito e pode pular a célula cuja impressão permaneceu igual (`app/ponto/services/cartorio.py:1348`, `app/ponto/services/cartorio.py:1358`, `app/ponto/services/cartorio.py:1362`). Recuperação efetiva em produção: [não verificado].

- **Fila — trilha superestima dias julgados.** `_rejulgados` inclui células que depois são excluídas pelo veredito humano (`app/ponto/portas/celula.py:298`, `app/ponto/portas/celula.py:395`), e dias atuais cujo turno ainda não terminou podem receber `None` do juiz (`app/ponto/services/cartorio.py:1982`). Mesmo assim já foram registrados como re-julgados (`app/ponto/portas/celula.py:303`).

- **Sem segundo juiz identificado neste trecho.** Exportada usa `_competencias_exportadas` (`app/ponto/portas/celula.py:205`), que consulta o juiz existente (`app/ponto/management/commands/gerar_celulas.py:152`); cobertura usa `vinculo_cobre_a_data` (`app/ponto/portas/celula.py:295`); julgamento e emissão seguem `julgar_celula` → fábrica (`app/ponto/portas/celula.py:402`, `app/ponto/services/cartorio.py:997`). “Humana” canônica é o conjunto de vereditos `_HUMANOS`, preservado pelo fluxo (`app/colaboradores/services/pos_vinculo.py:14`, `app/ponto/portas/celula.py:395`).

- **Sem escape de exportada identificado pelos callbacks examinados.** Reavaliação de feriado recebe somente `_tocadas` (`app/ponto/portas/celula.py:462`, `app/chamados/services/feriado_reavaliar.py:76`). Rejunção global apenas aponta FK de célula existente (`app/chamados/rejuncao.py:73`, `app/chamados/services/vinculos.py:98`); o signal retorna quando o status não mudou (`app/chamados/signals.py:168`).

- **Chamadores sem pedido explícito preservam comportamento quanto ao parâmetro novo.** O default é `rejulgar=()` e o bloco novo exige valor verdadeiro (`app/ponto/portas/celula.py:102`, `app/ponto/portas/celula.py:289`). Único chamador que passa `rejulgar`: `app/escala/folgas.py:267`. Demais: `app/escala/signals.py:71`, `app/escala/signals.py:117`, `app/escala/services/cadastro_tipo.py:348`, `app/escala/services/escala_auto_executor.py:104`, `app/escala/services/escala_auto_executor.py:143`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:230`, `app/ponto/portas/regen_exportada.py:174`, `app/ponto/portas/celula.py:504`; reexport em `app/ponto/management/commands/gerar_celulas.py:13`.

## 05/11 15:18 · sessao d02a48d17352 · - · - · origem=Ronald (tmux)

Censo servidor, somente leitura:

- **Porta normal:** `/api/` inclui `api.urls`, que aponta `ponto/bater/` para `api.views.api_bater_ponto` (`app/config/urls.py:37`; `app/api/urls.py:2,67`). **Porta kernel:** `config.urls_core` inclui `api.urls_core`, que importa `views_core` e aponta a mesma rota para sua função (`app/config/urls_core.py:6`; `app/api/urls_core.py:5,14`). Qual configuração atende produção: **[nao verificado]**.
- **Identidade:** ambos exigem autenticação e obtêm `colab = request.user.colaborador`; não escolhem colaborador a partir do corpo (`app/api/views.py:398-404`; `app/api/views_core.py:385-391`). A configuração admite JWT e sessão (`app/config/settings/base.py:32-43`). Portanto, sob sessão válida de B, batida guardada quando A estava logado será atribuída a B, se passar os gates — inferência estática, certeza alta (`app/api/views.py:402,683-695`).
- **CSRF:** sessão e middleware CSRF estão configurados (`app/config/settings/base.py:39,56-59`); a fila manda `X-CSRFToken` obtido do cookie atual (`app/static/pwa.js:140,153-157`). A função não tem `csrf_exempt`, apenas decorators DRF (`app/api/views.py:398-400`). Validação efetiva pelo framework instalado: **[nao verificado]**; o contrato de SessionAuthentication é inferido da classe configurada (`app/config/settings/base.py:39`).
- **Gates relevantes:** tipo precisa ser E/S; latitude/longitude são obrigatórios; falta de GPS dá 403 (`app/api/views.py:432-439,453-466`; kernel: `app/api/views_core.py:419-426,440-458`). Situação fora de ativo/afastado dá 403 (`app/api/views.py:415-423`). Assim, atribuição a B é um caminho possível, não garantia de aceitação (`app/api/views.py:402,415-423,453-466`).

**`timestamp_dispositivo`: escritores e leitores**

1. Cliente grava ISO bruto na fila (`app/static/pwa.js:113-119`) e o envia como campo multipart (`app/static/pwa.js:147`). O `timestamp` corrigido pelo gap é usado para ordenar a fila, mas o multipart não o envia; também não envia `timestamp_gap_ms`, `gps_accuracy` ou `gps_captured_at` (`app/static/pwa.js:137,143-157`).
2. APIs leem o bruto para auditoria de GPS ausente e payload de assinatura (`app/api/views.py:459,497-504`; `app/api/views_core.py:451,489-496`), e para decidir a hora efetiva por `hora_do_aparelho` (`app/api/views.py:536-539`; `app/api/views_core.py:528-531`).
3. `hora_lida` aceita epoch/ISO, normaliza timezone ou devolve None (`app/api/hora_do_aparelho.py:98-121`). `hora_do_aparelho` usa o timestamp do aparelho se estiver entre agora e 48h atrás; fora disso usa agora, preservando o timestamp legível bruto (`app/api/hora_do_aparelho.py:62-67,135-153`).
4. As APIs passam `timestamp_dispositivo=ts_aparelho` ao escritor `registrar_batida` (`app/api/views.py:682-695`; `app/api/views_core.py:679-692`). Ele encaminha `**campos` a `Batida.objects.create`; campo do modelo é nullable (`app/ponto/registro_batida.py:94,152-159`; `app/ponto/models.py:75`).
5. Leitor persistido: template DP mostra a coluna “dispositivo” (`app/templates/ponto/batidas_offline_pendentes.html:70-75`), porém a view alimentadora seleciona **somente** `origem='offline_pendente', aprovada=False` (`app/ponto/views.py:441-446`), enquanto o endpoint atual grava `origem='app'` (`app/api/views.py:683-684`). Logo, existência desse leitor não prova que novas batidas da fila apareçam nessa tela — certeza alta (`app/ponto/views.py:441-446`; `app/api/views.py:684`).
6. Outro leitor persistido: `descartadas` compara `timestamp` e `timestamp_dispositivo` acima da tolerância de 0,05h (`app/api/hora_do_aparelho.py:67,156-185`). Chamador dessa função fora de testes não foi localizado; execução em produção **[nao verificado]**.
7. `ponto.views.bater_ponto` recebe o formulário web legado, usa colaborador da sessão e delega POST E/S para `ramo_registrar_es`; não é a URL usada pela fila (`app/ponto/views.py:241-249,268-272,346-354`; `app/static/pwa.js:153`). `ponto.views.batida_offline` está aposentada e responde 410, apesar do decorator `csrf_exempt` (`app/ponto/views.py:417-428`). Não confundir esse `csrf_exempt` com a API atual (`app/api/views.py:398-400`).
8. Há leituras semelhantes em **ping geográfico**, não no recebimento da batida da fila: `app/api/views.py:2343-2361`; `app/api/views_core.py:881-899`; `app/ponto/views.py:2929,2987`.

**Deduplicação: transmissão repetida não implica duas linhas**

- API devolve `ja_registrada` quando encontra mesmo colaborador + mesmo tipo em janela inclusiva ±5min do **timestamp efetivo**, retornando sucesso (`app/api/views.py:557-575`; kernel: `app/api/views_core.py:549-567`). Esse filtro não exclui batidas retratadas (`app/api/views.py:559-562`).
- Chokepoint retorna batida existente do mesmo tipo, não retratada, em ±60s (`app/ponto/registro_batida.py:106-115`); depois verifica a chave exata colaborador+timestamp, inclusive outro tipo/retratada (`app/ponto/registro_batida.py:116-122`).
- Tipo oposto tem envelope de 300s com escala/marcos, senão 60s; pode ser permitido quando casa com marco próprio; sem autoridade humana, colisão é recusada (`app/ponto/registro_batida.py:21-49,138-151`). A API transforma essa recusa em resposta de sucesso `ignorada_intervalo` (`app/api/views.py:698-721`).
- Modelo declara unicidade por **colaborador+timestamp**, sem tipo e sem ID do evento/fila (`app/ponto/models.py:102-106`). Verificação prévia e create são separados; concorrência com mesmo timestamp pode chegar à constraint, sem tratamento local nesse helper (`app/ponto/registro_batida.py:120-122,157-161`). Constraint realmente aplicada ao banco de produção: **[nao verificado]**.

**Contraexemplo concreto à idempotência universal — inferência estática, certeza alta no caminho, ocorrência [nao verificado]:** item tem timestamp bruto com mais de 48h. Primeiro envio às 10:00 usa `agora=10:00`, grava; resposta/JSON/delete falha e item permanece. Retry às 10:06 usa `agora=10:06`, pois o bruto continua antigo demais. Não encontra a primeira no filtro ±5min, no ±60s ou pela chave exata; sem outras colisões/gates, pode criar segunda batida (`app/api/hora_do_aparelho.py:141-153`; `app/static/pwa.js:158-179`; `app/api/views.py:557-562,683-695`; `app/ponto/registro_batida.py:106-122,157-161`; `app/ponto/models.py:106`).

**Contraexemplo ainda na fronteira:** primeira tentativa com bruto de 47h59min é aceita na hora antiga; resposta perdida; retry após cruzar 48h muda a hora efetiva para agora e pode escapar da deduplicação, mesmo sendo o mesmo item (`app/api/hora_do_aparelho.py:141-153`; `app/api/views.py:557-562`). Condições restantes de aceitação e ocorrência em produção: **[nao verificado]**.

## 05/11 15:18 · sessao abe6c0def59b · - · - · origem=Ronald (tmux)

**Certeza alta: conclusões estáticas abaixo; ocorrência em aparelho/produção [não verificado].**

- **Rota efetiva:** `/ponto/bater/` chama `bater_ponto` com `ui_v2=False`; `/ponto/bater-v2/` passa `ui_v2=True` (`app/ponto/urls.py:11-12`). A view seleciona `bater_ponto_v2.html` somente com `ui_v2`; caso contrário usa `_template_triagem`, cujo ramo apto escolhe `triagem/apto.html` (`app/ponto/views.py:235,356-358`). Não atribuir automaticamente a recarga do template v2 à URL `/ponto/bater/`.

- **Escritores/chamadores da fila:** `bater_ponto_v2.html` grava após exceção de `fetch` online (`app/templates/ponto/bater_ponto_v2.html:349-356`) ou se `navigator.onLine` falso (`:367-374`). Outros chamadores existentes: template antigo `app/templates/ponto/bater_ponto.html:543,597`; triagem apta `app/templates/ponto/triagem/apto.html:289`. Os chamadores fornecem tipo, coordenadas, precisão, instante GPS e foto; a gravação real é `pwa.js` (`app/static/pwa.js:107-119`). Uso em produção do template antigo [não verificado].

- **Online v2:** o formulário tem CSRF, tipo sugerido, latitude, longitude e foto (`app/templates/ponto/bater_ponto_v2.html:102-107`); o JS adiciona precisão e selfie e envia multipart para `/api/ponto/bater/`, lendo o CSRF atual do cookie (`:328-338`). **Não acrescenta `timestamp_dispositivo` no envio online v2**; esse campo nasce na gravação offline (`app/static/pwa.js:115`), e o sincronizador o envia (`:147`). `timestamp`, `timestamp_gap_ms`, `gps_accuracy`, `gps_captured_at` guardados na fila não aparecem no FormData do sincronizador (`app/static/pwa.js:143-156`).

- **Contraexemplo online+sync:** servidor aceita o POST online de `v2Confirmar`, mas a conexão cai antes de a resposta chegar; `fetch` rejeita, o catch salva uma segunda representação da mesma batida; a próxima sincronização a envia novamente (`app/templates/ponto/bater_ponto_v2.html:335-356`; `app/static/pwa.js:138-157`). Isto prova caminho para **dois envios**, não duas linhas persistidas: dedup do servidor precisa de análise separada. Cenário condicional inferido do código; ocorrência [não verificado].

- **Recarga automática v2:** `_vistoEm` começa em `Date.now()`, é sobrescrito quando a página deixa de estar visível; ao voltar visível, recarrega somente se diferença **maior** que 300000ms (`app/templates/ponto/bater_ponto_v2.html:305-310`). Não existe timer que recarregue enquanto a página continua oculta; não existe guarda de `_enviando`, foto ou operação de banco nesse callback (`:305-310`). Outra recarga ocorre em `pageshow.persisted` (`:306`), repetida globalmente no PWA (`app/static/pwa.js:430-431`).
  - **Contraexemplo:** A tira selfie, minimiza por 5min1s e volta offline: `location.reload()` é chamado; a selfie que só está em `_fotoBlob` se perde com a execução anterior; qualquer sucesso do novo carregamento depende do SW/rede (`app/templates/ponto/bater_ponto_v2.html:146,252,305-310`). Se a fila já confirmou gravação, ela está em IndexedDB e não apenas nessa variável (`app/static/pwa.js:110-122`). Resultado real [não verificado].

- **Dados nos bytes do HTML v2:** CSRF renderizado (`app/templates/ponto/bater_ponto_v2.html:103`); tipo sugerido (`:104`); posto com latitude, longitude e raio (`:139-140`); no sucesso, hora e aviso do dia (`:53-56`). A base renderiza `user.username` para autenticado (`app/templates/base.html:112,316`), e inclui painel com `data-dono="{{ user.pk }}"` (`app/templates/base.html:432`; `app/templates/core/_copiloto.html:17`). O v2 só esconde outros elementos com `display:none`, não remove seus bytes/DOM (`app/templates/ponto/bater_ponto_v2.html:148`).
  - **Nome:** nome completo não é interpolado diretamente no v2; a base contém **username**, que pode ou não coincidir com nome real [não verificado] (`app/templates/base.html:316`).
  - **Foto:** selfie é obtida em runtime por câmera/arquivo e canvas; vídeo e inputs nascem sem imagem pessoal no HTML (`app/templates/ponto/bater_ponto_v2.html:88-91,235-267`).
  - **Geolocalização:** coordenadas do **posto** são do HTML (`:139-140`); coordenadas atuais da pessoa são de `getCurrentPosition`, colocadas em variáveis e campos inicialmente vazios (`:105-106,175-180,183-195`).
  - **Contraexemplo de casca cacheada, condicional:** guardar resposta HTML de A e servi-la offline a B na mesma URL expõe username/ID, token renderizado, posto e estado de tela de A; o cache armazena resposta HTTP, não a selfie ou o GPS que o JS inseriu posteriormente. A existência desse HTML no cache depende dos ramos do SW e não está demonstrada por mera navegação (`app/static/sw.js:45-50,67-77`). Certeza alta sobre conteúdo; vazamento ocorrido [não verificado].

- **Storage adicional:** a fila do PWA usa IndexedDB `juliani-offline` v2, não localStorage (`app/static/pwa.js:48-60,107-119`). Na base há `sessionStorage` de scroll, chave `scrollpos:`+pathname, com gravação no submit e leitura/remoção no load (`app/templates/base.html:405-417`). Convite de notificações usa `sessionStorage['avisos_convite']` (`app/static/pwa.js:353,362,366`); triagem apta usa `sessionStorage['gps_aviso_visto']` (`app/templates/ponto/triagem/apto.html:278-286`). LocalStorage encontrado no script global de tabela guarda preferências sob namespace `hx.table.v2`, não batidas (`app/static/js/hasner-table.js:11,45-50`).

- **Persistência:** `navigator.storage.persisted()`/`persist()` são condicionais; ausência da API não interrompe a execução e exceções são engolidas (`app/static/pwa.js:436-443`). Ausência de API não produz garantia alternativa de durabilidade; retenção real no Safari [não verificado]. No SW existe listener `sync-batidas` (`app/static/sw.js:112-132`), mas não encontrei registro `sync.register` ou `periodicSync` nos arquivos auditados `sw.js`, `pwa.js`, `bater_ponto_v2.html`; a sincronização de página roda pelo evento online e no boot online (`app/static/pwa.js:320,323-328`). Não é necessário alegar compatibilidade de versão específica do Safari para essa conclusão.

- **Disparadores concorrentes:** `online` e boot podem chamar `sincronizarBatidas`; o listener de mensagem do SW também chama (`app/static/pwa.js:15-17,320,328`). A função não tem mutex: faz `getAll`, envia e só depois apaga (`app/static/pwa.js:126-165`). **Contraexemplo:** duas chamadas obtêm a mesma pendência antes da primeira exclusão e ambas fazem POST; igualmente duas abas têm a mesma fila e podem receber a mensagem porque o SW faz broadcast a todos (`app/static/sw.js:116-119`). Certeza alta do caminho; ocorrência e persistência duplicada [não verificado].

_1595 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
