# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 10:41** · publicado em **27/11/2026 10:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 27/11 10:21 · sessao e678aeb86a9e · RC-SEM-PROXIMA · verificador · origem=Code

**REPROVADO.** O candidato preserva a lista `RC SEM COLHER` quando a árvore, o RELATO e o instante são fixos. A interface mudou: a mesma obra pode aparecer nas duas listas numa única chamada, e o `~/vercodex.py` conta `RC SEM PROXIMA` como `RC SEM COLHER`. O prazo também pode disparar antes de completados 15 minutos reais. `bin/rc_sem_colher.py:89–113,116–163` · `/home/ronald/vercodex.py:163–171`

### 1. A lista `SEM COLHER` é idêntica?

**Para a mesma árvore, o mesmo RELATO e o mesmo `agora`, sim.** A regex aceita as mesmas linhas e `marcas()` devolve as mesmas chaves `(obra, caminho, HH:MM do rc)`. A seleção do `.rc`, a janela de 24 h, `--min-idade`, a folga, a leitura do código e a ordenação foram preservadas. A comparação com o anterior está no `diff.txt` anexo, hunks `-22`, `-47`, `-73` e `-101`. `bin/rc_sem_colher.py:26–56,63–102`

**Alta — o resultado observado do comando muda.** Com uma única obra colhida e parada, antes eram zero linhas e rc `0`. Agora sai `RC SEM PROXIMA` e rc `3`. Além disso, se a primeira passagem imprime um órfão e um diretório fica ilegível antes da segunda, a nova chamada termina com rc `1` depois de emitir a linha. A anterior terminaria com rc `3`. Reproduzi os dois resultados em memória. `bin/rc_sem_colher.py:59–73,153–163` · `diff.txt`, hunk `-101`

**Alta — as duas listas podem descrever estados incompatíveis.** Com `a.rc` colhido às 10:01 e `b.rc` mais novo e sem marca, a primeira passagem lista `b.rc`. Se `b.rc` some entre as passagens, a segunda lista `a.rc` como `SEM PROXIMA`. Reproduzi as duas linhas para a mesma obra na mesma chamada. O RELATO é lido uma vez, mas o índice de marcas, a varredura e o relógio são refeitos. `bin/rc_sem_colher.py:36–49,89–102,116–135,153–163`

### 2. Contraexemplos de `SEM PROXIMA`

| Gravidade | Critério | Contraexemplo |
|---|---|---|
| Média | Forma da marca | Uma colheita registrada como `colhido OBRA-X r.rc (10:00)` sem o prefixo `- HH:MM `, ou citada no meio da linha, deixa a obra parada fora de `SEM PROXIMA`. Ela pode aparecer como `SEM COLHER`. É o efeito previsto pela gramática preservada. `bin/rc_sem_colher.py:30–49,95–101,125–130` |
| Alta | Marca do rc mais novo | `a.rc` foi colhido e chegou um `b.rc` sem marca. Só `b.rc` é avaliado e, se elegível, sai em `SEM COLHER`. Já se `a.rc` for refeito no **mesmo caminho e minuto**, o novo resultado herda a marca antiga e pode sair em `SEM PROXIMA` sem ter sido colhido. Uma marca de outro dia com a mesma chave também colide. Em empate exato de mtime, fica o primeiro `.rc` visitado. `bin/rc_sem_colher.py:36–49,72–85,95–101,125–132` |
| Alta | `superado` e marcas múltiplas | Qualquer `superado` da mesma chave veta a saída, inclusive um antigo que colidiu com um rc refeito. O inverso também ocorre: `superado a.rc` deixa passar `b.rc` com `colhido`, mesmo que `b.rc` seja só um artefato posterior de uma obra já encerrada. Entre vários `colhido`, vence o maior horário inferido. Para rc às 10:00, uma marca válida às 10:01 e outra linha às 09:59 para a mesma chave fazem a segunda virar “amanhã” e esconder a obra parada. `bin/rc_sem_colher.py:36–43,105–113,125–134` |
| Alta | Escrita depois do rc | Um brief escrito **exatamente 5 s** depois do `.rc` ainda deixa a obra na lista. Um brief criado com mtime preservado ou um link simbólico também pode passar despercebido. No sentido inverso, um simples `touch` num arquivo regular da obra mais de 5 s depois do rc tira uma obra que continua parada. Um brief real escrito mais de 5 s depois do rc, ainda antes da marca, tira a obra conforme o caso do selo. `bin/rc_sem_colher.py:72–85,122–124` · `bin/tests/test_rc_sem_colher.sh:60–72` |
| Média | Escrita do juiz ou da espera | O juiz lê o RELATO, consulta a árvore e escreve em stdout. A espera cria a marca com `mktemp` e imprime os caminhos achados. Se `TMPDIR` estiver dentro da obra, ou a sessão redirecionar a saída para `logs/OBRA-X/espera.log`, o próprio arquivo de controle pode tirar a obra da lista. A ocorrência desses arranjos em produção é [nao verificado]. `bin/rc_sem_colher.py:52–56,72–85,153–163` · `bin/disjuntor.py:1967–1969` |
| Alta | 15 minutos, meia-noite e fração | Com rc às `10:00:30.7`, colheita às `10:00:59` e consulta às `10:15:01`, o juiz lista com **14 min 02 s** reais de espera. Ele usa `10:00:00` como instante da marca. A fração do mtime foi tratada corretamente para manter o mesmo minuto, e `23:50 → 00:10` funciona num dia de 24 h. Ainda assim, um rc de ontem às `23:50:59.9` e uma marca de hoje às `23:50:10` podem ser associados ao minuto de ontem enquanto o rc ainda está na janela de 24 h. Num dia com mudança de offset, somar `86400` também pode cair em outra hora local. `bin/rc_sem_colher.py:65–85,105–113,131–134` · `bin/tests/test_rc_sem_colher.sh:88–102` |

### 3. Quem escreve e quem lê

| Componente | Contrato observado |
|---|---|
| Produtores | O wrapper do Codex escreve o `.rc` da chamada do construtor em temporário e publica por `mv`. A sessão colhe e registra a marca no RELATO. O juiz usa o mtime do arquivo e o texto da marca, não o `fim=` gravado no `.rc`. `bin/codex_pergunta.sh:565–574` · `CLAUDE.md:332–337` · `app/docs/RELATO.md:13639–13646` · `bin/rc_sem_colher.py:78–85,125–132` |
| `ESPERA_UNICA` | Lê `$?`, repassa stdout e pula o laço com rc `3` de qualquer lista. A espera termina com rc `0` para rc `0` ou `3` do juiz e ainda pode imprimir caminhos do `find` final. Já dentro do laço, só observa `.rc` por `-cnewer`. Uma marca que completa 15 minutos durante a espera não a acorda por si só, e o juiz não é chamado de novo ao sair. `bin/disjuntor.py:1963–1969` |
| `espera_nega` e PreToolUse | Leem o **texto do comando Bash**. Verificam forma do laço, `find` e teto. A forma com o juiz retirado também passa, como confirmei em memória. O caso `caso_forma` exige a presença do juiz por uma asserção adicional. `.claude/settings.json:62–69` · `bin/disjuntor.py:2157–2174,2217–2228` · `bin/tests/test_rc_sem_colher.sh:160–162` |
| Stop e handoff | O Stop usa a prova do RELATO, as decisões do disjuntor e a fila. O handoff importa o Stop para ler fila, travas e `PAREI`, e grava `HANDOFF-SESSAO.md`. Esses caminhos não consomem stdout ou rc de `rc_sem_colher.py`. `.claude/settings.json:3–29` · `bin/hook_stop_fila1.py:1305–1329,1458–1501` · `bin/handoff_sessao.sh:17–21,44–78,124–145,292–338` |
| `~/vercodex.py` | **Alta — contagem errada no código local explicitamente nomeado.** Captura stdout e rc, conta toda linha não vazia sob `RC SEM COLHER` e exibe até 15 linhas. Três `SEM PROXIMA` viram `RC SEM COLHER: 3`, mesmo com zero órfãos. A chamada observada também não passa `--min-idade 900`, embora o contrato da cópia o documente. A versão efetivamente usada no pouso é [nao verificado]. `/home/ronald/vercodex.py:5,163–171` · `app/docs/RELATO.md:13639–13642` |
| `logs/espera_unica.sh` e outros | O script é citado no RELATO e `logs/` é ignorado. Seu conteúdo e seu tratamento do stdout e do rc são [nao verificado]. O selo da cópia lê o juiz diretamente e usa a forma de `ESPERA_UNICA`. Outros consumidores externos são [nao verificado]. `app/docs/RELATO.md:13598` · `.gitignore:53–54` · `bin/tests/test_rc_sem_colher.sh:55,75–108,143–144` |

### 4. Selo e RED

- **Os seis mutantes novos mordem nas verificações em memória.** `proxima-desligada` perde rc `3` mesmo imprimindo as três linhas. Os mutantes de etapa, idade e `superado` incluem respectivamente `OBRA-LANCADA`, `OBRA-RECENTE` e `OBRA-ENCERRADA`. Os de virada e fração erram a diferença de segundos esperada. `bin/tests/test_rc_sem_colher.sh:57–102,197–210`
- **O RED funcional das três obras cai pelo motivo certo no comportamento anterior mostrado no anexo.** O juiz antigo dá rc `0` e stdout vazio para as três colhidas. Os três casos de exclusão também ficam vermelhos, mas porque falta o controle positivo exigido por `_proxima_fora`. Os casos de meia-noite e fração cairiam por ausência de `_instante_da_marca`. A espera antiga entra no laço e não imprime `SEM PROXIMA`. `diff.txt`, hunks `-83` e `-101` · `bin/tests/test_rc_sem_colher.sh:75–108` · `bin/disjuntor.py:1967–1969`
- **Média — cobertura vazia e relógio.** `caso_proxima_vazia` fica verde com a funcionalidade ausente, inclusive no comportamento antigo. Tem três obras de exclusão e serve como controle negativo. `caso_proxima_espera` exige uma linha real, rc `0` e menos de 3 s, mas não tem mutante novo próprio. A fixture usa horas relativas com margens amplas e a meia-noite comum está coberta pelo cálculo. O selo não exercita `900`/`901` s nem os segundos desconhecidos da marca. A medição de `<3 s` usa `date +%s` e pode falhar por atraso de execução ou ajuste do relógio. `bin/tests/test_rc_sem_colher.sh:57–108,197–210`
- Fiz as sondas em memória porque o selo integral cria, altera e apaga arquivos. **Verde integral, RED executado no commit `59600163c` e pouso pela pista** ficam [nao verificado]. As três obras do selo são fixtures. Seu estado em produção fica [nao verificado]. `bin/tests/test_rc_sem_colher.sh:26–28,57–73,165–182` · `aval.txt:1`

### 5. Classes já reprovadas

O diff mantém um executável, uma regex de marca e um seletor de rc. Não acrescenta `except` ao juiz. A derivação em paralelo aparece **na execução**: duas passagens sobre a mesma fonte, com o predicado da folga repetido e dois `time.time()` internos em vez de um instante comum capturado na borda. O campo `HH:MM` também tem dois usos: identifica o minuto exibido e vira um instante com segundos `00` para decidir o prazo. A dupla observação e a antecipação do prazo têm contraexemplos reproduzidos acima. `bin/rc_sem_colher.py:30–49,59–102,105–135,138–167`

## 27/11 10:17 · sessao 60ded10607a8 · CASO-LITERAL-14 · sonda · origem=Code

# CASO-LITERAL-14 — Fase 1

A segunda volta cobre **17 sobras, em 12 dias de 5 colaboradores**. `154900` já estava nas 83 da primeira volta. O bloco completo de 14/11 muda sua classe de **(d)** para **(c)**. O consolidado fica em **99 batidas únicas, 43 dias e 14 colaboradores**. `compara.out:59–171` · `r_f1.md:44` · `r_f1.md:62` · `saída da sonda:3–23`

As referências à `saída da sonda` contam as linhas do texto colado a partir de `sonda_leitura`, incluindo a linha em branco. O ensaio local usou `parear_turnos` com as batidas e os marcos do envelope. Os papéis da ata e o ramo exato que gerou as sobras em produção estão [nao verificado]. `app/ponto/turnos.py:1192` · `app/ponto/turnos.py:1948` · `app/ponto/turnos.py:2005`

## 1. Classificação por dia

Cada ID entra no dia de seu `DIA.sobras_ids`. **Não** em `validada_por_admin` significa `aprovada_por_id=null`. O modelo distingue esse campo de `aprovada`, que tem valor padrão `True`. Nas linhas **(c)**, cabe ao admin conferir a escala na tela. O código também prevê folga trabalhada. `saída da sonda:3–445` · `app/ponto/models.py:62` · `app/escala/models.py:1080` · `app/ponto/motor_calculo_v2.py:2576`

| empresa | colab_id | data | batidas SOBRA (id hora tipo) | célula (estado, marcos) | classe | prova | validada_por_admin | dúvida |
|---:|---:|---|---|---|:---:|---|---|---|
| 2 | 986 | 2026-11-14 | `154900 11:59:38 S` | `127362` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `154840 10:01:39 E` → sobra → `154953 13:02:58 E` → `155336 22:00:59 S`. As bordas conferem. O ensaio reconheceu a pausa de **1 h 03 min 20 s**, cerca de 4 h antes dos marcos internos. A pergunta `58126` está pendente na célula, com chamados `31901` e `39082` abertos. `saída da sonda:3–23` · `app/ponto/turnos.py:1466` · `app/escala/utils.py:477` | Não | Horário de pausa autorizado e marcos anteriores à regeneração [nao verificado]. |
| 2 | 986 | 2026-11-20 | `160434 12:08:00 S` | `127368` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `160310 10:02:41 E` → sobra → `160436 13:10:07 E` → `160805 22:01:34 S`. As bordas conferem e o ensaio reconheceu pausa de **1 h 02 min 07 s** perto de 12h–13h. A pergunta `60335` aponta para `160434`. `60335` e `60540` estão pendentes, com chamados `31901` e `40124` abertos. `saída da sonda:26–41` · `app/ponto/turnos.py:1466` · `app/escala/utils.py:477` | Não | Horário autorizado, marcos anteriores à regeneração e resolução da pergunta de liveness [nao verificado]. |
| 2 | 989 | 2026-10-22 | `134543 17:08:58 E` | `127432` folga, `null` | **c** | No ensaio, `134543` fechou com `135077 08:32:05 S` de 23/10, **15 h 23 min 07 s** depois. A jornada começa em 22/10, em folga na célula, com vínculo `1423` de `18:00–06:00`. O chamado `33904` está aberto e inclui o par. `saída da sonda:50–64` · `app/docs/LEIS.md:117` · `app/ponto/turnos.py:312` · `app/ponto/turnos.py:1430` | Não | Escala autorizada e teor do chamado `33904` [nao verificado]. |
| 2 | 989 | 2026-10-23 | `135077 08:32:05 S` | `127433` trabalho, `E18:00/S06:00 (+1)` | **c** | No ensaio, a sobra fechou a jornada de **22/10** iniciada por `134543 17:08:58 E`, cuja célula `127432` está em folga. A jornada de 23/10 tem outro par, `135589 17:59:28 E` → `135908 06:00:14 S` de 24/10. A conferência de escala recai sobre 22/10. `saída da sonda:50–57` · `saída da sonda:84–100` · `app/docs/LEIS.md:117` · `app/ponto/turnos.py:312` | Não | Escala autorizada em 22/10 e teor do chamado aberto `33904` [nao verificado]. |
| 2 | 989 | 2026-10-25 | `137193 22:11:08 S` | `127435` trabalho, `E18:00/S06:00 (+1)`, pausa `null` | **a** | O ensaio fechou `136992 15:57:34 E` → `137291 05:58:14 S` de 26/10 no dia **25/10**, com pausa `137193` → `137210 23:02:41 E` de **51 min 33 s**. As duas bordas externas ficam a menos de 3 h dos marcos. O chamado `33904` está aberto e inclui a sobra. Sem ação na batida. `saída da sonda:122–136` · `app/ponto/turnos.py:429` · `app/ponto/turnos.py:453` · `app/docs/LEIS.md:116–117` | Não | Teor do chamado, papéis da ata e causa exata da sobra [nao verificado]. |
| 2 | 1010 | 2026-11-04 | `146083 17:56:08 E` | `143296` folga, `null` | **d** | `145446 06:00:53 S` fecha a jornada iniciada em 03/11 no ensaio. A sobra da tarde fica aberta com as batidas apuráveis. O ensaio só a liga a `146380 05:59:33 S` de 05/11 quando inclui `146307 00:05:03 S`, já retratada por `657`. O chamado `36783` está aberto e inclui essas batidas. `saída da sonda:157–180` · `app/ponto/turnos.py:1417` · `app/ponto/turnos.py:2961` | Não | Faltam a decisão sobre `146307` e a confirmação do papel de `146083` na jornada de 04/11 [nao verificado]. |
| 2 | 1010 | 2026-11-05 | `146321 01:04:44 E` · `146380 05:59:33 S` | `143297` trabalho, `E18:00/S06:00 (+1)`, pausa `null` | **d** | Com as batidas apuráveis, o ensaio fecha as duas sobras como um par na madrugada de 05/11. Ao incluir `146307 00:05:03 S` só no ensaio, elas passam a compor a jornada iniciada por `146083` em 04/11. O plantão cadastrado de 05/11 tem `147009 17:58:49 E` e `147338 06:26:13 S` de 06/11. `saída da sonda:199–224` · `app/ponto/turnos.py:285` · `app/ponto/turnos.py:1491` · `app/ponto/turnos.py:2961` | Não, ambas | Faltam a confirmação da jornada efetiva de 04/11, a decisão sobre `146307` e o teor do chamado aberto `36783` [nao verificado]. |
| 2 | 1010 | 2026-11-07 | `151808 05:54:00 S` | `143299` trabalho, `E18:00/S06:00 (+1)`, pausa `null` | **d** | A pergunta validada `54816` materializou `151808` para `data_evento=06/11`, célula `143298`. A célula de 07/11 tem a própria saída `149056 05:53:56 S` em **08/11**, após `148746 17:59:35 E` de 07/11. `151808.retratada_em=null`. Chamado-pai `36652` aberto e destino `37414` resolvido. `saída da sonda:243–267` · `app/chamados/models.py:3095` · `app/docs/LEIS.md:117` | **Sim, `657`** | Faltam os marcos de `143298` e o teor da declaração validada para identificar a jornada que `151808` deveria fechar [nao verificado]. |
| 2 | 1036 | 2026-11-14 | `154907 12:00:48 E` | `143957` trabalho, `E14:40/S18:00/E19:00/S23:00` | **a** | O ensaio fechou `154907` → `155066 15:50:47 S` e `155091 16:50:34 E` → `155363 23:00:24 S` no próprio dia, com pausa de **59 min 47 s**. A entrada está a **2 h 39 min 12 s** de `hi` e a saída a **24 s** de `hf`. Sem ação na batida. `saída da sonda:317–336` · `app/ponto/turnos.py:1491` · `app/ponto/motor_calculo_v2.py:2044` | Não | Papéis da ata e causa exata da sobra [nao verificado]. |
| 2 | 1036 | 2026-11-19 | `159844 18:04:22 S` | `143962` trabalho, `E14:40/S18:00/E19:00/S23:00` | **b** | A sobra veio **1 s** depois de `159843 18:04:21 E`. O par `159785 17:04:57 S` → `159843` já fecha a pausa de **59 min 24 s**, entre `159655 14:40:28 E` e `160048 23:00:07 S`. No ensaio, omitir apenas `159844` preservou as bordas e a pausa. Omitir `159843` desfez o par da pausa. A `S` de 1 s é o toque excedente. `saída da sonda:337–364` · `app/docs/RELATO.md:259` · `app/docs/LEIS.md:80` · `app/ponto/turnos.py:880` · `app/ponto/turnos.py:1487` | **Não**, `aprovada_por_id=null` | `retratada_em=null`. Sem pergunta ou chamado aberto sobre `159844` no recorte. As quatro perguntas e os chamados resolvidos exibidos são de 20/11 e apontam para outros IDs. `saída da sonda:352–374` |
| 2 | 1036 | 2026-11-20 | `160483 14:43:43 E` · `160622 18:18:19 S` · `160742 19:18:33 E` · `160829 23:01:03 S` | `143963` folga, `null` | **c** | Dois períodos de trabalho no próprio dia, com pausa de **1 h 00 min 14 s**, compatíveis com `14:40–23:00` do vínculo `1552`. As quatro perguntas apontam para a célula e para os quatro IDs. Os chamados foram resolvidos pelo sistema via julgamento da célula. `saída da sonda:380–409` · `app/escala/orfa_classe.py:62` · `app/chamados/models.py:2847` | Não, as quatro | Escala autorizada e causa da folga [nao verificado]. |
| 2 | 1039 | 2026-11-19 | `159385 12:02:11 S` · `159454 12:59:59 E` | `144160` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `159304 09:56:22 E` → duas sobras → `160033 22:05:39 S`. O ensaio reconheceu a pausa de **57 min 48 s**, cerca de 4 h antes dos marcos internos. `59844` aponta para `159454`. As três perguntas estão respondidas e sem validação, com chamados `38937` e `39967` abertos. `saída da sonda:415–431` · `app/ponto/turnos.py:1466` · `app/escala/utils.py:477` | Não, ambas | Horário de pausa autorizado e teor das respostas [nao verificado]. |

## 2. Contagem desta volta

Cada célula mostra **dias / batidas**. A lista de IDs confere com `compara.out:151–171`. As classes vêm da tabela acima, com base nos `DIA.sobras_ids` da saída. `saída da sonda:3–445`

| colab_id | (a) | (b) | (c) | (d) | total |
|---:|---:|---:|---:|---:|---:|
| 986 | 0 / 0 | 0 / 0 | 2 / 2 | 0 / 0 | 2 / 2 |
| 989 | 1 / 1 | 0 / 0 | 2 / 2 | 0 / 0 | 3 / 3 |
| 1010 | 0 / 0 | 0 / 0 | 0 / 0 | 3 / 4 | 3 / 4 |
| 1036 | 1 / 1 | 1 / 1 | 1 / 4 | 0 / 0 | 3 / 6 |
| 1039 | 0 / 0 | 0 / 0 | 1 / 2 | 0 / 0 | 1 / 2 |
| **Total** | **2 / 2** | **1 / 1** | **6 / 10** | **3 / 4** | **12 / 17** |

## 3. Batidas (b) sem dúvida e sem validação admin

`159844` é a única batida que atende aos critérios no envelope. `compara.out:164` · `saída da sonda:350–364` · `app/docs/RELATO.md:259`

```text
B batida_id=159844 colab_id=1036 data=2026-11-19 hora=18:04:22 tipo=S
```

## 4. Batidas (b) com validação admin ou dúvida

**Lista vazia.** A sobra com `aprovada_por_id` preenchido, `151808`, ficou em **(d)** pela falta de prova sobre a jornada a que pertence. `saída da sonda:243–267` · `app/ponto/models.py:62`

## 5. Porta do botão e desfazer

- **Retratar.** O botão **Não foi trabalho (retratar)** envia `POST` para `ponto:retratar_batida`. A view exige `regularizar_batida` e chama `retratar_humano.retratar`. O serviço reconfere a permissão, exige motivo ativo e chama `registro_batida.retratar_batida`. A porta preserva o registro, grava `aprovada=False` e `retratada_em`, registra a trilha e aciona a atualização do fechamento. A apuração seleciona `retratada_em__isnull=True`. `app/templates/colaboradores/partials/_calendario_grade.html:127` · `app/ponto/urls.py:62` · `app/ponto/views.py:3453` · `app/ponto/services/retratar_humano.py:158` · `app/ponto/services/retratar_humano.py:175` · `app/ponto/registro_batida.py:287` · `app/ponto/turnos.py:2961`
- **Desfazer.** O botão envia `POST` para `ponto:desfazer_retratacao`. A view exige a mesma permissão e chama `retratar_humano.desfazer`. A porta exige `retratada_em` preenchido, última trilha `retratar_batida_admin` e prazo de **7 dias** a partir da retratação. Usa `registro_batida.anotar` para gravar `aprovada=True`, limpar `retratada_em` e registrar `desfazer_retratacao_batida`. `app/templates/colaboradores/partials/_calendario_grade.html:127` · `app/ponto/urls.py:64` · `app/ponto/views.py:3465` · `app/ponto/services/retratar_humano.py:26` · `app/ponto/services/retratar_humano.py:38` · `app/ponto/services/retratar_humano.py:186` · `app/ponto/services/retratar_humano.py:203`
- A cadeia dos botões foi conferida nos arquivos da cópia. O topo de `sonda_f1.py`, seu hash e o `HEAD` Git da cópia estão [nao verificado].

## Consolidado das duas voltas

O ajuste retira a linha parcial de `154900` da classe **(d)** da primeira volta e conserva a linha completa em **(c)**. O colab `986` fica com **5 dias / 5 batidas**, todos em **(c)**. `r_f1.md:41–44` · `r_f1.md:61–62` · `saída da sonda:3–41` · `compara.out:147–152`

| classe | primeira volta | segunda volta | ajuste por sobreposição | total único |
|:---:|---:|---:|---:|---:|
| (a) | 1 / 1 | 2 / 2 | 0 / 0 | **3 / 3** |
| (b) | 0 / 0 | 1 / 1 | 0 / 0 | **1 / 1** |
| (c) | 19 / 65 | 6 / 10 | 0 / 0 | **25 / 75** |
| (d) | 12 / 17 | 3 / 4 | −1 / −1 | **14 / 20** |
| **Total** | **32 / 83** | **12 / 17** | **−1 / −1** | **43 / 99** |

**14 colaboradores e 99 batidas únicas.** `compara.out:59–171` · `r_f1.md:62` · `saída da sonda:3–445`

## 27/11 10:10 · sessao 88a168481d71 · CASO-LITERAL-14 · sonda · origem=Code

# CASO-LITERAL-14 — Fase 1

O trecho colado contém **83 batidas em `DIA.sobras_ids`, em 32 dias de 10 colaboradores**. A saída termina no bloco de 14/11 do colab 986, antes da célula. Em relação ao universo informado de 99 batidas e 14 colaboradores, faltam 16 batidas no recorte. A distribuição delas por dia, colaborador e classe está [nao verificado]. Usei os blocos `DIA`, `BAT`, `CEL`, `VINCULO`, `PERGUNTA` e `CHAMADO` colados como fonte dos fatos abaixo. A numeração original da saída, o topo de `sonda_f1.py`, `compara.out` item 4 e o hash Git da cópia estão [nao verificado].

## 1. Classificação por dia

A data e as sobras de cada linha vêm de `DIA.sobras_ids`. Uma `BAT` marcada `SOBRA` na janela de um dia vizinho entra na linha da sua própria lista `sobras_ids`. `Não` em `validada_por_admin` significa `aprovada_por_id=null` nas sobras da linha. O modelo distingue esse campo de `aprovada`, cujo valor padrão é `True`. [app/ponto/models.py:62](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/models.py:62)

Nas linhas **(c)**, a divergência observável vai para conferência de escala/cadastro pelo admin. Onde a célula marca folga, o código também prevê folga trabalhada. A necessidade de editar a célula, ajustar o vínculo ou manter a folga e tratar o trabalho depende da escala efetivamente autorizada [nao verificado]. [app/escala/models.py:1080](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/models.py:1080) · [app/ponto/motor_calculo_v2.py:2576](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:2576)

| empresa | colab_id | data | batidas SOBRA (id hora tipo) | célula (estado, marcos) | classe | prova | validada_por_admin | dúvida |
|---:|---:|---|---|---|:---:|---|---|---|
| 3 | 163 | 2026-11-18 | `158099 05:06:08 S` | `140479` folga, `null` | **d** | `157833 18:53:02 E` de 17/11 precede `158092 04:07:13 S`, a sobra e `158201 06:59:03 E`. A pergunta `59348` liga a sobra à célula `140478` de 17/11 e ao chamado `35571`, aberto. A L-085 permite atribuir a madrugada à véspera. `app/ponto/turnos.py:285` | Não | Faltam os marcos e os papéis julgados da véspera, além do teor da resposta `59348`. A sequência tem duas `S` consecutivas. [nao verificado] |
| 3 | 163 | 2026-11-20 | `160135 05:12:54 S` | `140481` folga, `null` | **d** | `159878 18:53:11 E` de 19/11 precede `160127 04:13:41 S`, a sobra e `160202 06:56:06 E`. A pergunta `60098` liga a sobra à célula `140480` de 19/11 e ao chamado aberto `35571`. `app/ponto/turnos.py:285` | Não | Faltam os marcos e os papéis julgados da véspera e o teor da resposta `60098`. [nao verificado] |
| 2 | 219 | 2026-11-14 | `154790 07:03:07 E` · `154793 07:04:17 S` | `138398` folga, `null` | **d** | O par dura **70 s**, acima do piso de 60 s que impede formar turno. A véspera tem `154453 19:07:37 E` e três `S` na madrugada, incluindo `159534 02:00:00 S` aprovada por `657`. As perguntas `57827` e `57828` ligam as duas sobras ao chamado-pai `16611`, aberto. `app/ponto/turnos.py:130`, `app/ponto/turnos.py:1487` | Não, ambas | Faltam o fechamento e a ata da jornada de 13/11 ou confirmação de que a dupla foi um toque indevido. [nao verificado] |
| 2 | 301 | 2026-10-22 | `134361 14:45:11 E` · `134720 20:00:23 S` · `134740 21:00:21 E` · `134778 22:50:11 S` | `123197` folga, `null` | **c** | Dois períodos completos no próprio dia, com pausa de cerca de 1 h. O padrão se repete em 21/10 e 23/10. O vínculo `265`, template `397`, omite quinta-feira. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 301 | 2026-10-29 | `140896 14:45:26 E` · `141196 20:00:05 S` · `141220 21:00:15 E` · `141257 22:50:02 S` | `123204` folga, `null` | **c** | Dois períodos no dia, com horários equivalentes aos de 28/10 e 30/10. O vínculo `265` omite quinta-feira. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 301 | 2026-11-05 | `146858 14:45:34 E` · `147164 20:00:12 S` · `147191 21:02:44 E` · `147218 22:50:03 S` | `123211` folga, `null` | **c** | Dois períodos no dia. A série aprovada `152818`–`152821` é de **04/11**, na célula `123210`. A sequência de 05/11 tem IDs e instantes próprios. `app/escala/orfa_classe.py:62`, `app/docs/LEIS.md:117` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 301 | 2026-11-12 | `153134 14:45:13 E` · `153464 20:00:05 S` · `153492 21:00:04 E` · `153529 22:50:04 S` | `123218` folga, `null` | **c** | Dois períodos no dia, repetidos em 11/11 e 13/11. As perguntas `57176`, `57302`, `57325` e `57342` apontam para a própria célula e para o chamado aberto `22355`. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 301 | 2026-11-19 | `159659 14:45:03 E` · `159996 20:04:10 S` · `160018 21:04:15 E` · `160039 22:50:05 S` | `123225` folga, `null` | **c** | Dois períodos no dia, repetidos em 18/11 e 20/11. As perguntas `59897`, `60018`, `60024` e `60038` apontam para a própria célula e para o chamado aberto `22355`. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 334 | 2026-11-02 | `143656 01:40:09 S` · `143671 02:34:17 E` | `132547` folga, `null` | **c** | `143583 20:21:17 E` de 01/11 → pausa das duas sobras → `143844 07:52:44 S` de 02/11. O ensaio com as quatro batidas cruas formou um turno de **01/11**, com as sobras internas. As duas pontas estão mais de 3 h distantes de `10:00–22:00` do vínculo `1499`. A divergência é com o horário da jornada iniciada em 01/11. `app/ponto/turnos.py:385`, `app/ponto/turnos.py:407`, `app/ponto/turnos.py:312`, `app/ponto/motor_calculo_v2.py:2044` | Não | Marcos efetivos da célula `132546` de 01/11 e horário autorizado [nao verificado]. |
| 2 | 334 | 2026-11-10 | `150563 01:00:52 S` | `132555` folga, `null` | **d** | `150460 19:51:14 E` de 09/11 → sobra → `150585 02:02:38 E`. A pergunta `55996` liga `150585` à célula `132554` de 09/11. O ramo sem marcos exige uma `S` posterior para demonstrar continuidade após a volta. `app/ponto/turnos.py:393` | Não | Faltam a saída após `150585`, os marcos da véspera e a confirmação do papel de `150563`. [nao verificado] |
| 2 | 334 | 2026-11-16 | `156062 01:38:42 S` · `157001 20:34:22 E` | `132561` folga, `null` | **d** | `156062` fica entre `155980 20:12:08 E` de 15/11 e `156083 02:31:57 E`. Já `157001` pode formar jornada própria com `157080 00:03:27 S` de 17/11. As perguntas `58903` e `58935` ligam esse segundo trecho a 16/11 e ao chamado aberto `17636`. `app/ponto/turnos.py:312`, `app/ponto/turnos.py:1491` | Não | Falta o fechamento da jornada de 15/11 e a confirmação de qual `S` encerrou a de 16/11. As duas sobras da linha podem pertencer a jornadas diferentes. [nao verificado] |
| 2 | 334 | 2026-11-17 | `157080 00:03:27 S` · `157102 01:34:42 S` · `157339 07:57:06 S` | `132562` trabalho, `E10:00/S15:00/E16:00/S22:00` | **d** | As três `S` vêm depois de `157001 20:34:22 E` de 16/11 e antes de `163470 10:00:00 E`, plantada e aprovada por `657`. A pergunta `58935` liga `157080` a 16/11. Chamados `17636` e `39479` abertos. `app/ponto/turnos.py:293`, `app/ponto/turnos.py:1491` | Não | Faltam a saída efetiva de 16/11 e o papel das outras `S`. [nao verificado] |
| 2 | 446 | 2026-10-25 | `136793 08:00:46 E` | `136270` trabalho, `E14:40/S18:00/E19:00/S23:00` | **d** | `136990 15:57:29 S` permite um período diurno. `144666 18:00:00 S` e `144667 23:00:00 S` foram plantadas e aprovadas por `657` para a mesma célula. A L-084 exige identificar as duas bordas reais antes de comparar com os marcos. `app/ponto/motor_calculo_v2.py:2046` | Não | Falta confirmar se `136990` encerrou o trabalho e como os registros diurnos se relacionam às declarações validadas. [nao verificado] |
| 2 | 446 | 2026-10-27 | `138687 12:30:18 E` | `136272` trabalho, `E14:40/S18:00/E19:00/S23:00` | **d** | `138784 13:31:02 S` é candidata a fechar um período de 1 h. Também há `144664 18:00:00 S` e `145038 23:00:00 S`, plantadas e aprovadas por `657`. O chamado `36462` está `em_analise`. `app/ponto/turnos.py:1192` | Não | Faltam os papéis julgados e o teor da declaração para distinguir o período diurno da jornada declarada. [nao verificado] |
| 2 | 446 | 2026-11-01 | `143320 13:04:48 E` | `136277` trabalho, `E14:40/S18:00/E19:00/S23:00` | **d** | A entrada é 95 min 12 s anterior ao `hi`. Seguem `143451 18:01:12 S` e `143543 19:01:11 E`. A primeira `S` e a `E` seguinte estão junto dos marcos da pausa. `app/ponto/turnos.py:1356` | Não | Falta a saída após `143543` ou a ata que confirme a borda de encerramento. [nao verificado] |
| 2 | 446 | 2026-11-20 | `160414 13:00:43 E` | `136296` trabalho, `E14:40/S18:00/E19:00/S23:00` | **a** | A sonda lista `160414` como sobra. O ensaio de `parear_turnos` com as quatro batidas do dia e os marcos da célula fechou `160414 → 160528 15:52:59 S` e `160559 16:54:13 E → 160790 21:20:55 S`, ambos em 20/11. As bordas externas estão a menos de 3 h dos respectivos marcos. Sem ação sobre a batida. `app/ponto/turnos.py:1402`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1491`, `app/ponto/motor_calculo_v2.py:2044` | Não | O ramo exato que deixou a sobra e a obra que o corrige estão [nao verificado]. O ensaio usou as batidas do dia e os marcos exibidos, sem os papéis da ata. `app/ponto/turnos.py:1201`, `app/ponto/turnos.py:2005` |
| 2 | 465 | 2026-11-12 | `152798 09:00:29 S` · `152993 12:38:07 S` | `130759` trabalho, `E07:30/S12:30/E13:30/S19:30` | **d** | `152993` fica a 8 min 07 s de `hii` e tem `153079 13:34:14 E` como volta compatível. No ensaio com os marcos, `152798` fechou o período de `152760 07:33:15 E` e `152988 12:31:42 E` abriu o segundo período. `app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1491` | Não | Falta confirmar o papel real de `152798` e `152988` para classificar o dia inteiro. [nao verificado] |
| 2 | 707 | 2026-10-27 | `152914 06:55:00 E` | `130929` trabalho, `E11:00/S16:00/E17:00/S23:00` | **d** | `152914` foi plantada pela pergunta validada `50832`. `152916 12:10:00 E` também foi plantada. `152915 16:00:00 S` já foi retratada, e `151794 19:00:00 S` tem trilha de retratação, desfazer e inversão. A célula foi regenerada. Chamados `30747` aberto e `35226` em análise. `app/ponto/models.py:62`, `app/ponto/turnos.py:2941` | **Sim, `657`** | Faltam os marcos anteriores à regeneração e a decisão sobre qual entrada correspondeu à jornada. `152914.retratada_em=null` no recorte. [nao verificado] para a jornada real. |
| 2 | 917 | 2026-10-24 | `136272 15:00:39 E` · `136465 19:00:26 S` · `136519 20:01:27 E` · `136570 22:58:12 S` | `123158` folga, `null` | **c** | Dois períodos no sábado, com horários equivalentes aos de 23/10. O vínculo `1134`, template `282`, omite sábado. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para o sábado [nao verificado]. |
| 2 | 917 | 2026-10-31 | `142679 15:00:20 E` · `142887 19:02:25 S` · `142930 20:00:34 E` · `142989 22:53:43 S` | `123165` folga, `null` | **c** | Dois períodos no sábado, com horários equivalentes aos de 30/10 e 01/11. O vínculo `1134` omite sábado. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para o sábado [nao verificado]. |
| 2 | 917 | 2026-11-14 | `155033 15:00:59 E` · `155258 19:00:45 S` · `155296 20:00:18 E` · `155357 22:57:03 S` | `123179` folga, `null` | **c** | Dois períodos no sábado, com horários equivalentes aos de 13/11. As perguntas `57974`, `58082`, `58106` e `58134` apontam para essa célula e para o chamado aberto `20366`. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para o sábado [nao verificado]. |
| 2 | 968 | 2026-10-22 | `134368 14:49:09 E` · `134573 18:02:02 S` · `134668 19:00:22 E` · `134777 22:50:06 S` | `122828` folga, `null` | **c** | Dois períodos na quinta, com horários equivalentes aos de 21/10. O vínculo `1358`, template `397`, omite quinta-feira. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 968 | 2026-10-29 | `140898 14:50:08 E` · `141045 18:00:11 S` · `141137 19:00:15 E` · `141258 22:50:07 S` | `122835` folga, `null` | **c** | Dois períodos na quinta, com horários equivalentes aos de 28/10. O vínculo `1358` omite quinta-feira. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 968 | 2026-11-02 | `144020 14:50:08 E` · `144120 18:00:05 S` · `144221 19:00:17 E` · `144323 22:50:05 S` | `122839` folga, `null` | **c** | Dois períodos no dia, com horários equivalentes aos de 01/11 e 03/11. A etiqueta do template `397` inclui segunda-feira, e a célula está em folga. `app/escala/models.py:1080`, `app/escala/models.py:1384` | Não | Causa da folga e escala autorizada [nao verificado]. |
| 2 | 968 | 2026-11-05 | `146860 14:49:19 E` · `147014 18:00:08 S` · `147112 19:00:08 E` · `147219 22:50:23 S` | `122842` folga, `null` | **c** | Dois períodos na quinta, com horários equivalentes aos de 04/11. O vínculo `1358` omite quinta-feira. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 968 | 2026-11-12 | `153136 14:50:08 E` · `153317 18:00:06 S` · `153416 19:00:25 E` · `153531 22:50:14 S` | `122849` folga, `null` | **c** | Dois períodos na quinta, com horários equivalentes aos de 11/11. As perguntas `57177`, `57256`, `57276` e `57343` apontam para essa célula. O chamado `37385` consta como resolvido pelo sistema. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 968 | 2026-11-15 | `155745 14:49:57 E` · `155836 18:00:14 S` · `155922 19:00:05 E` · `156007 22:50:16 S` | `122852` folga, `null` | **c** | Dois períodos no dia, com horários equivalentes aos de 16/11. A etiqueta do template `397` inclui domingo. A série aprovada `158537`–`158540` pertence a **14/11**, célula `122851`. `app/escala/models.py:1080`, `app/docs/LEIS.md:117` | Não | Causa da folga e escala autorizada [nao verificado]. |
| 2 | 968 | 2026-11-19 | `159662 14:49:14 E` · `159831 18:00:16 S` · `159931 19:00:15 E` · `160040 22:50:06 S` | `122856` folga, `null` | **c** | Dois períodos na quinta, com horários equivalentes aos de 18/11. As perguntas `59898`, `59966`, `59987` e `60039` apontam para a célula `122856`, com quatro chamados resolvidos pelo sistema. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 986 | 2026-10-29 | `140694 12:01:23 S` | `127346` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `140600 10:00:39 E` → sobra → `140772 13:00:41 E` → `141243 22:00:18 S`. As bordas conferem e a pausa de 59 min 18 s fica cerca de 4 h antes dos marcos da pausa. Célula regenerada em 13/11. Pergunta `57451` pendente e chamado `38767` aberto. `app/escala/utils.py:477`, `app/ponto/motor_calculo_v2.py:2046` | Não | Motivo do horário da pausa e marcos anteriores à regeneração [nao verificado]. |
| 2 | 986 | 2026-11-06 | `147648 12:03:33 S` | `127354` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `147551 09:53:34 E` → sobra → `147741 13:09:54 E` → `148171 22:03:20 S`. As bordas conferem e a pausa de 1 h 06 min 21 s volta a ocorrer perto de 12h–13h. Célula regenerada em 13/11. Pergunta `57452` pendente e chamado `38769` aberto. `app/escala/utils.py:477`, `app/ponto/motor_calculo_v2.py:2046` | Não | Motivo do horário da pausa e marcos anteriores à regeneração [nao verificado]. |
| 2 | 986 | 2026-11-10 | `150969 12:50:48 S` | `127358` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `150825 09:51:19 E` → sobra → `151040 13:50:21 E` → `151438 22:03:41 S`. As bordas conferem e a pausa de 59 min 33 s fica mais de 3 h antes de ambos os marcos internos. Pergunta `56242` aponta diretamente para a sobra e está pendente. Chamados `31901` e `38770` abertos. `app/escala/utils.py:477`, `app/ponto/motor_calculo_v2.py:2046` | Não | Motivo do horário da pausa e marcos anteriores à regeneração [nao verificado]. |
| 2 | 986 | 2026-11-14 | `154900 11:59:38 S` | [nao verificado] | **d** | `154840 10:01:39 E` → sobra → `154953 13:02:58 E`. A pausa candidata dura 1 h 03 min 20 s. A saída colada termina durante a auditoria de `154953`. `app/ponto/turnos.py:407` | Não | Faltam célula, marcos, saída final e eventual pergunta ou chamado desse dia. [nao verificado] |

## 2. Contagem por classe e por colab

Cada célula mostra **dias / batidas**. Fonte da contagem: os `DIA.sobras_ids` transcritos na tabela acima. A contagem da competência completa está [nao verificado].

| colab_id | (a) | (b) | (c) | (d) | total |
|---:|---:|---:|---:|---:|---:|
| 163 | 0 / 0 | 0 / 0 | 0 / 0 | 2 / 2 | 2 / 2 |
| 219 | 0 / 0 | 0 / 0 | 0 / 0 | 1 / 2 | 1 / 2 |
| 301 | 0 / 0 | 0 / 0 | 5 / 20 | 0 / 0 | 5 / 20 |
| 334 | 0 / 0 | 0 / 0 | 1 / 2 | 3 / 6 | 4 / 8 |
| 446 | 1 / 1 | 0 / 0 | 0 / 0 | 3 / 3 | 4 / 4 |
| 465 | 0 / 0 | 0 / 0 | 0 / 0 | 1 / 2 | 1 / 2 |
| 707 | 0 / 0 | 0 / 0 | 0 / 0 | 1 / 1 | 1 / 1 |
| 917 | 0 / 0 | 0 / 0 | 3 / 12 | 0 / 0 | 3 / 12 |
| 968 | 0 / 0 | 0 / 0 | 7 / 28 | 0 / 0 | 7 / 28 |
| 986 | 0 / 0 | 0 / 0 | 3 / 3 | 1 / 1 | 4 / 4 |
| **Total** | **1 / 1** | **0 / 0** | **19 / 65** | **12 / 17** | **32 / 83** |

## 3. Batidas (b) sem dúvida e sem validação admin

**Lista vazia no recorte.** Nenhuma linha `B batida_id=...` é sustentada pelos blocos da tabela. A classificação das sobras ausentes está [nao verificado].

## 4. Batidas (b) com validação admin ou dúvida

**Lista vazia no recorte.** Os casos duvidosos estão em **(d)**. Em particular, `154790` e `154793` têm `aprovada_por_id=null` e `retratada_em=null`, perguntas `57827` e `57828` materializadas via `celula_absolveu` e chamado-pai `16611` aberto. O chamado `39246` também está aberto na célula `138398`, com `batidas_contexto_ids=[]`. O código descreve `celula_absolveu` como resultado do julgamento da célula. A causa da dupla de 70 s está [nao verificado]. [app/chamados/models.py:2847](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:2847)

## 5. Botão de retratar e desfazer

- **Retratar.** O botão **Não foi trabalho (retratar)** envia `POST` à rota `ponto:retratar_batida`. A view exige `regularizar_batida` e chama `retratar_humano.retratar`. O serviço reconfere a permissão, exige motivo ativo e chama `registro_batida.retratar_batida`. A porta grava `aprovada=False` e `retratada_em`, registra a trilha e aciona a atualização do fechamento. O filtro de apuração usa `retratada_em__isnull=True`. [app/templates/colaboradores/partials/_calendario_grade.html:127](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/templates/colaboradores/partials/_calendario_grade.html:127) · [app/ponto/urls.py:62](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/urls.py:62) · [app/ponto/views.py:3453](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/views.py:3453) · [app/ponto/services/retratar_humano.py:158](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/retratar_humano.py:158) · [app/ponto/services/retratar_humano.py:175](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/retratar_humano.py:175) · [app/ponto/registro_batida.py:287](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/registro_batida.py:287) · [app/ponto/turnos.py:2961](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/turnos.py:2961)
- **Desfazer.** O botão envia `POST` à rota `ponto:desfazer_retratacao`. A view chama `retratar_humano.desfazer`. A porta exige a mesma permissão, `retratada_em` preenchido, última trilha `retratar_batida_admin` e prazo de **7 dias** a partir da retratação. Usa `registro_batida.anotar` para gravar `aprovada=True`, limpar `retratada_em` e registrar `desfazer_retratacao_batida`. [app/templates/colaboradores/partials/_calendario_grade.html:127](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/templates/colaboradores/partials/_calendario_grade.html:127) · [app/ponto/urls.py:64](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7

[truncado em 20000 de 20597 caracteres]

## 27/11 10:08 · sessao 5706d8bf958d · RAZAO-DA-SOBRA · construtor · origem=Code

**reds1 entregue na cópia.** São 37 testes de comportamento e controle, mais 15 mutantes em memória. A checagem estática confirmou sintaxe válida, 10 âncoras presentes uma vez e 5 ausentes. O diff consiste nos seis módulos de teste abaixo. [Fonte](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:1) · [Lavra](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:1) · [Leitores](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_leitores.py:1) · [Repasse](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_repasse.py:1) · [E2 e fábrica](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_e2_fabrica.py:1) · [Mutantes](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:1) [nao verificado]

**Veredito da suite e linhas de `red.log` [nao verificado].** A rota da Pista 2 usa SSH, cria um commit de foto para a cópia suja e grava `refs/pista`. Essas operações esbarram nas restrições da volta. A rota alternativa também escreve um env-file. O comando autorizado para quem puder executar está no fim. [suite.sh](/home/ronald/wt-codex-razao-da-sobra-094857/bin/suite.sh:324) · [SSH](/home/ronald/wt-codex-razao-da-sobra-094857/bin/suite.sh:378) · [Foto e ref](/home/ronald/wt-codex-razao-da-sobra-094857/bin/suite.sh:420) · [Rota alternativa](/home/ronald/wt-codex-razao-da-sobra-094857/bin/suite.sh:509) [nao verificado]

### REDs e controles

Os motivos abaixo vêm da leitura da base. O resultado de cada asserção em execução segue **[nao verificado]**.

| Resposta ou volta | Caso e motivo esperado na base |
|---|---|
| R1 | Oposta a 8 s, borda inclusiva de 10 s e residual a 11 s. Falha explícita `autoridade escala.orfa_classe.razao_da_sobra ausente na base`. A classe legada opera por minuto. [RED](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:74) · [Base](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/orfa_classe.py:53) |
| R2 | Duas sobras no mesmo minuto em foto antiga. Falhas independentes `autoridade escala.services.leitor_celula.sobras_da_ata ausente na base` e `autoridade ponto.services.cartorio.medir_razoes_sobras ausente na base`. O leitor atual entrega horas. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:140) · [Base](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/services/leitor_celula.py:180) |
| R3 | O callback autônomo de isenção segue executando com TXT vigente. O mutante estende `_K6` ao callback e cobra a perda do silêncio. [Controle](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_repasse.py:190) · [Mutante](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:174) |
| R4 | Um admitido e um vetado. O teste exige agregado com um item e trilha da recusa por item. A base grava os dois IDs antes de processar as perguntas. [RED](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_repasse.py:130) · [Base](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/services/disputa_emissao.py:1117) | [nao verificado]
| R5 / A emissor | Origem aberta e alvo efetivo entregue. O teste observa a resolução do dia dentro de `_nascer`, os `INSERT`s, os emitidos e a releitura do chamador. A base executa `save(force_insert=True)` e a guarda B2 do `save` incide em alterações. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_repasse.py:72) · [Nascimento](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/models.py:3333) · [Guarda](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/models.py:3847) | [nao verificado]
| R6 | O residual é provocado depois do `carimbar` real. O teste exige rollback ao `SAVEPOINT`, contagens iguais em todas as tabelas detectadas na escrita e vizinha intacta. A base chama o corpo e a precedência sem a cerca desse ato. [RED](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:271) · [Base](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/services/cartorio.py:2112) | [nao verificado]
| V1 | Identidades distintas no mesmo instante, segundos no mesmo minuto, duas linhas na meia-noite e CxR com par adicional. A fonte atual reduz o insumo a instante, tipo e marca de intervalo e entrega sobras em `HH:MM`. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:166) · [Entrada](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/utils.py:1367) · [Saída](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/utils.py:1883) |
| V2 | Primeira ata para o supra, persistência, retirada da ocorrência na reconciliação, duas relavras e contadores. A base lavra horas e contagem, retira por minuto e omite os contadores de razão. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:60) · [Ata](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/services/cartorio.py:749) · [Reconciliação](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/utils.py:2093) · [Contadores](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/services/cartorio.py:2021) |
| V3 | Mesma ocorrência e razão na tela, PDF, supra e insumos de chamados e folha. O selo cobra leituras pela porta. A base ainda leva a hora crua ao template e compara por hora no PDF. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_leitores.py:74) · [Template](/home/ronald/wt-codex-razao-da-sobra-094857/app/templates/ponto/espelho.html:242) · [PDF](/home/ronald/wt-codex-razao-da-sobra-094857/app/relatorios/pdf_espelho.py:607) |
| A cartório | Margem de meia-noite, 10:00 com ambiente 10:02, C6 no gate e no tripwire, primeira lavra C6 `0→1` sem subir emitidos e veto antes da escrita. A base consulta o relógio ambiente no avulso e chama C6 sem `agora` no caminho compartilhado. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:28) · [Avulso](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/services/cartorio.py:2083) · [C6](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/models.py:368) |
| A E2 e fábrica | Duas candidatas, TXT em um dia, carimbo do admitido, fecho adiado da vetada e veto antes do descarimbo. A base percorre as candidatas do E2 por colaborador e chama o descarimbo antes da guarda de limbo. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_e2_fabrica.py:65) · [E2](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/reconciliador.py:681) · [Fábrica](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/services/fabrica_da_celula.py:206) |

Os controles escritos cobrem saída residual a 11 s, pausa L-032, foto histórica, ordem C → D → Ch → P, callback autônomo, dia sem ata, lista vazia, leitor alimentado com zero consultas e efeito zero em dinheiro, causa e retenção. **Verde em suite [nao verificado].** [Fonte](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:106) · [Repasse](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_repasse.py:143) · [Fábrica](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_e2_fabrica.py:128) · [Lavra](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:135) · [Leitores](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_leitores.py:170) [nao verificado]

Os testes assumem `batida_id`, `instante` e `razao` no registro de ocorrência, além das chaves `minutos_ambiguos` e `sobras_por_razao` nos contadores. A forma final dessas interfaces, a cobertura da borda 36, o toque em viewport real, o teto total de consultas da lavra e a publicação no placar e no HAIKU seguem **[nao verificado]**. O brief transcrito foi a referência disponível. A leitura integral do dossiê §6/§8 na cópia segue **[nao verificado]**. [Ocorrência e conferência](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:153) · [Contadores](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:101) [nao verificado]

### Mutantes

Cada rótulo abaixo é argumento de `rodar_mutante`. **Presente** indica âncora única conferida estaticamente, com mordida **[nao verificado]**. **Skip** indica âncora ausente na base e mensagem `espera a cura RAZAO-DA-SOBRA`. [Executor](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:28)

| Rótulo | Âncora | RED | Estado |
|---|---|---|---|
| `CatracaMutantesRazaoSobraTest.test_leitor_cru` | `_marcos_faltantes`, chamada a `grade_da_celula` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:62) | V3 catraca | Presente | [nao verificado]
| `CatracaMutantesRazaoSobraTest.test_relogio_solto_no_gate_avulso` | `julgar_celula`, `agora < _fim` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:73) | A1 10:00/10:02 | Skip | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_falta_de_razao_aparece_no_contador` | `julgar_celula`, `return out` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:89) | V2 `sobras_sem_razao=0` | Presente | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_soma_por_razao_perde_uma_sobra` | `julgar_celula`, `return out` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:96) | V2 soma igual a `n_orfas` | Presente | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_correcao_so_no_cron` | `julgar_celula`, guarda `competencia_do_dia_entregue` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:106) | A1 veto no avulso | Skip | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_relogio_so_no_gate` | `_gravar_estado`, `_vivas_entre(..., agora=agora)` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:113) | A1 C6 no tripwire | Skip | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_relogio_perdido_na_escrita` | `_julgar_colab_corpo`, `carimbar(..., agora=agora)` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:122) | A1 instante persistido | Presente | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_parei_residual_libera_savepoint` | `executar_sob_cerca`, `raise` após veto anotado [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:129) | R6 escrita parcial | Presente | [nao verificado]
| `E2MutantesRazaoSobraTest.test_veta_colaborador_inteiro` | `retratar_perguntas_absolvidas_por_celula`, `if not frescas` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:146) | A2 carimbo da admitida | Presente | [nao verificado]
| `E2MutantesRazaoSobraTest.test_guarda_so_depois_do_descarimbo` | `perguntar_pela_celula`, guarda de entrega antes do descarimbo [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:156) | A2 veto antes da escrita | Skip | [nao verificado]
| `E2MutantesRazaoSobraTest.test_inventa_zero_de_lista_vazia` | `julgar_colab`, registro `[PERGUNTA]` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:164) | A2 preserva emissão confirmada | Presente | [nao verificado]
| `RepasseMutantesRazaoSobraTest.test_k6_estendido_a_signal_on_commit_autonomo` | `_isentos_post_save_chamado`, `silenciar_do_colab(colab_id)` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:174) | R3 alcance | Presente | [nao verificado]
| `RepasseMutantesRazaoSobraTest.test_filtra_o_dia_antes_de_nascer` | `criar_disputa_a_partir_de_chamados_orfaos`, import de `_dtc_i` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:185) | R5 duas candidatas chegam a `_nascer` | Presente | [nao verificado]
| `RepasseMutantesRazaoSobraTest.test_insere_antes_do_veto` | `_nascer`, `vigiar_escrita_de_perguntas([p])` antes do `save` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:195) | R5 `INSERT` só para admitido | Skip | [nao verificado]
| `RepasseMutantesRazaoSobraTest.test_veta_o_lote_inteiro` | `criar_disputa_a_partir_de_chamados_orfaos`, seleção `_intervalo` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:203) | R5 retorno de sucesso | Presente | [nao verificado]

### Comandos

A partir da raiz da cópia, com autorização para a rota da Pista 2 e com o diretório de log disponível. Destino `logs/RAZAO-DA-SOBRA/reds1/red.log`. [Contrato da suite](/home/ronald/wt-codex-razao-da-sobra-094857/bin/suite.sh:22)

```bash
copia=/home/ronald/wt-codex-razao-da-sobra-094857
cd "$copia"

PYTHONDONTWRITEBYTECODE=1 bash bin/suite.sh --dir "$copia" \
  --only "escala.tests.test_razao_sobra_fonte ponto.tests.test_razao_sobra_lavra ponto.tests.test_razao_sobra_leitores chamados.tests.test_razao_sobra_e2_fabrica chamados.tests.test_razao_sobra_repasse chamados.tests.test_razao_sobra_mutantes" \
  --parallel 1 > logs/RAZAO-DA-SOBRA/reds1/red.log

rodar_mutante() {
  PYTHONDONTWRITEBYTECODE=1 bash bin/suite.sh --dir "$copia" \
    --only "chamados.tests.test_razao_sobra_mutantes.$1" --parallel 1
}
```

A certificação continua com Code e opus final. **`red.log` e veredito [nao verificado].**

## 27/11 10:03 · sessao 123515df12cb · CASO-LITERAL-14 · leitura · origem=Code

```python
SOBRAS = [
    # Preencha com (empresa, colab_id, "AAAA-MM-DD", batida_id).
    # Exemplos da amostra_sobra.txt:1-2
    # (2, 43, "2026-11-02", 143768),
    # (2, 43, "2026-11-02", 143890),
]

# O envelope recebe um .py do chamador e o executa com
# tenant_command shell --schema=juliani e exec(open(...).read()).
# O código e a sonda são montados em leitura e o ORM usa a role leitor.
# bin/sonda_leitura.sh:63-91,153-181,235-243
#
# PORTA DA FASE 2
# O botão "Não foi trabalho (retratar)" faz POST com motivo_id e observacao.
# app/templates/colaboradores/partials/_calendario_grade.html:127
# ponto:retratar_batida -> retratar_batida_view -> retratar_humano.retratar
# -> registro_batida.retratar_batida -> registro_batida.anotar.
# O serviço humano exige a permissão regularizar_batida e motivo cadastrado
# ativo. O escritor preserva a linha, grava aprovada=False e retratada_em,
# registra o motivo na auditoria e dispara o evento de fechamento.
# app/ponto/urls.py:62; app/ponto/views.py:3453-3462
# app/ponto/services/retratar_humano.py:118-183
# app/ponto/registro_batida.py:255-302
#
# O botão "desfazer" usa desfazer_retratacao_view -> retratar_humano.desfazer
# -> anotar. A mesma permissão é exigida. A última linha de auditoria da
# batida deve ser retratar_batida_admin e o prazo é de sete dias a partir
# de retratada_em. O ato grava retratada_em=None e aprovada=True.
# app/templates/colaboradores/partials/_calendario_grade.html:127
# app/ponto/urls.py:64; app/ponto/views.py:3465-3473
# app/ponto/services/retratar_humano.py:26-49,186-206
#
# Campos de aprovação/admin em Batida: aprovada, aprovada_por (lido via
# aprovada_por_id), aprovada_em e retratada_em. O sinal de aprovação
# atribuída a um usuário é aprovada_por_id preenchido. aprovada tem
# default True. Edições de tipo e de horário têm autoria e data na trilha.
# app/ponto/models.py:22-95; app/ponto/views.py:490-494
# app/ponto/services/flip_auto.py:285-298; app/ponto/services/flip_batida.py:41-49
# app/chamados/services/regularizacao_ext.py:150-162; app/core/models.py:285-307
#
# Lista completa dos 14, dados de produção e execução com o ORM no
# envelope [nao verificado]. Sintaxe e funções puras conferidas em memória.

import json
from collections import defaultdict
from datetime import date, datetime, time, timedelta
from functools import lru_cache
from zoneinfo import ZoneInfo

from django.db import connection
from django.db.models import Q
from django.utils import timezone

from chamados.catalogo.modulos import data_do_chamado
from chamados.models import (
    ChamadoColaborador,
    DecisaoPergunta,
    FollowupChamado,
    PerguntaDisputa,
    RegularizacaoItemBatida,
)
from colaboradores.models import Colaborador
from core.models import LogAuditoria
from escala.models import CelulaDia
from escala.servico_jornada import escala_vigente
from ferias.models import AgendamentoFerias
from ponto.catalogo.ausencias import CATALOGO, INDETERMINADO
from ponto.models import Ausencia, Batida, Justificativa, TipoAusencia

TZ = ZoneInfo("America/Sao_Paulo")  # app/config/settings/base.py:93-95
UM_DIA = timedelta(days=1)
NV = "[nao verificado]"


def formatar(valor):
    if isinstance(valor, datetime):
        return timezone.localtime(valor, TZ).isoformat()
    if isinstance(valor, (date, time)):
        return valor.isoformat()
    if isinstance(valor, str) and valor and "W004" not in valor and all(
        c.isalnum() or c in "_-+.:/" for c in valor
    ):
        return valor
    # Escapes JSON preservam o valor ao passar pelo filtro do envelope.
    # bin/sonda_leitura.sh:239-243
    return json.dumps(valor, ensure_ascii=False).replace(
        "W004", r"\u0057004"
    ).replace("objects imported", r"objects\u0020imported")


def linha(rotulo, *, marca="", **campos):
    valores = " ".join(f"{k}={formatar(v)}" for k, v in campos.items())
    print(f"{rotulo} {valores}" + (f" {marca}" if marca else ""))


def meia_noite(dia):
    return timezone.make_aware(datetime.combine(dia, time.min), TZ)


def agrupar():
    dias = {}
    todas = set()
    for empresa, cid, data_iso, bid in SOBRAS:
        if any(type(pk) is not int or pk <= 0 for pk in (empresa, cid, bid)):
            raise ValueError("SOBRAS exige IDs inteiros positivos")
        dia = date.fromisoformat(data_iso)
        if dia.isoformat() != data_iso or not date(2026, 10, 21) <= dia <= date(2026, 11, 20):
            raise ValueError(f"Data fora do formato ou da comp 11 de 2026 (colab_id={cid})")
        empresa_dia, ids = dias.setdefault((cid, dia), (empresa, set()))
        if empresa != empresa_dia:
            raise ValueError(f"Empresas divergentes no mesmo dia (colab_id={cid} data={dia})")
        ids.add(bid)
        todas.add(bid)
    if not dias:
        raise ValueError("Preencha SOBRAS antes da execucao")
    return dias, todas


def batidas_do_recorte(cid, dia, sobras_dia):
    # Janela semiaberta entre as meias-noites locais de D-1 e D+2.
    # Leitura da linha bruta, incluindo retratadas e pendentes.
    # app/ponto/models.py:22-29,40-73; app/ponto/registro_batida.py:99-109
    batidas = list(Batida.objects.filter(
        colaborador_id=cid, timestamp__gte=meia_noite(dia - UM_DIA),
        timestamp__lt=meia_noite(dia + 2 * UM_DIA),
    ).order_by("timestamp", "pk").values(
        "id", "timestamp", "tipo", "origem", "canal", "tipo_registro",
        "is_intervalo", "escala_id", "pergunta_origem_id",
        "aprovada", "aprovada_por_id", "aprovada_em", "retratada_em",
    ))
    faltam = sobras_dia - {b["id"] for b in batidas}
    if faltam:
        raise ValueError(
            f"SOBRA fora da janela ou do colaborador "
            f"(colab_id={cid} data={dia} ids={sorted(faltam)})"
        )
    return batidas


def trilhas_de(batida_ids):
    # A ação csrf_batida usa o ID do colaborador no lugar do ID da batida.
    # app/core/views.py:1224-1242; app/core/models.py:285-307
    saida = defaultdict(list)
    for ato in (LogAuditoria.objects.filter(modelo="Batida", objeto_id__in=batida_ids)
                .exclude(acao="csrf_batida").order_by("criado_em", "pk")
                .values("id", "objeto_id", "acao", "usuario_id", "criado_em", "valor_depois")):
        saida[ato["objeto_id"]].append(ato)
    return saida


def retratacao_vigente(instante, atos):
    # Casa o carimbo vigente com a trilha. Motivo humano sai como ID.
    # Quando a justificativa só existe em texto livre, a trilha identifica
    # o ato e motivo_retratacao sai como [nao verificado].
    # app/ponto/registro_batida.py:231-239,270-296
    # app/ponto/services/retratar_humano.py:172-181
    # app/chamados/services/acoes_chamado.py:261-279
    # app/ponto/management/commands/passivo_disputa_retro.py:164-171
    if instante is None:
        return None, None
    for ato in reversed(atos):
        depois = ato["valor_depois"]
        if isinstance(depois, dict) and depois.get("retratada_em"):
            if datetime.fromisoformat(depois["retratada_em"]) == instante:
                motivo = depois.get("motivo_retratacao") or {}
                motivo_id = motivo.get("id")
                if type(motivo_id) is int:
                    return ato, motivo_id
                if depois.get("motivo_classe") == "passivo_s84_folga":
                    return ato, "passivo_s84_folga"
                return ato, NV
    return None, NV


def imprimir_batida(batida, atos, todas_as_sobras):
    bid = batida["id"]
    local = timezone.localtime(batida["timestamp"], TZ)
    retratacao, motivo = retratacao_vigente(batida["retratada_em"], atos)
    linha(
        "BAT", id=bid, data=local.date(), hora=local.strftime("%H:%M:%S"),
        marca="SOBRA" if bid in todas_as_sobras else "",
        **{k: v for k, v in batida.items() if k not in ("id", "timestamp")},
        motivo_retratacao=motivo,
        retratacao_trilha_id=retratacao["id"] if retratacao else None,
        retratada_por_id=retratacao["usuario_id"] if retratacao else None,
        retratacao_acao=retratacao["acao"] if retratacao else None,
    )
    for ato in atos:
        linha("BAT_AUD", batida_id=bid, trilha_id=ato["id"], acao=ato["acao"],
              usuario_id=ato["usuario_id"], quando=ato["criado_em"])


def imprimir_escala(cid, dia):
    # hi/hf são entrada/saída. hii/hfi e hii2/hfi2 são saída/volta das pausas.
    # Os instantes previstos já gravados permitem ler a virada de meia-noite.
    # app/escala/models.py:1656-1747,1843-1847
    # app/ponto/management/commands/gerar_celulas.py:36-70,88-113
    celula = (CelulaDia.objects.filter(colaborador_id=cid, data=dia)
              .select_related("escala_geradora__tipo_escala").first())
    celulas = {(cid, dia): celula} if celula is not None else {}
    if celula is None:
        linha("CEL", id=None, estado=NV)
    else:
        dna = celula.dna or {}
        linha(
            "CEL", id=celula.pk, origem=celula.origem, trabalha=celula.trabalha,
            estado={True: "trabalho", False: "folga", None: "indeterminado"}[celula.trabalha],
            marcos=dna.get("marcos"), instantes_previstos=dna.get("lampadas"),
            escala_geradora_id=celula.escala_geradora_id, tipo_escala_dna_id=dna.get("tipo_escala_id"),
            regime_dna=dna.get("tipo_ciclo"), gerada_em=celula.gerada_em,
            regenerada_em=celula.regenerada_em, regeneracoes=celula.regeneracoes,
            editada_em=celula.editada_em, editada_por_id=celula.editada_por_id,
        )
    # Leitor do vínculo do dia, com a precedência e a vigência da geradora.
    # app/escala/servico_jornada.py:15-66; app/escala/alimentacao.py:269-286
    # app/escala/models.py:117-121,801-804,871-877
    escala = escala_vigente(cid, dia, celulas=celulas)
    if escala is None:
        linha("VINCULO", id=None)
    else:
        template = escala.tipo_escala
        linha("VINCULO", id=escala.pk, posto_id=escala.posto_id, template_id=template.pk,
              template=str(template), regime=template.tipo_ciclo, inicio=escala.data_inicio,
              fim=escala.data_fim, ativa=escala.ativa)


def imprimir_ausencias(cid, dia, tipos):
    # Mostra cada lançamento e seu estado, inclusive rejeitados e cancelados.
    # efeito_tipo=abona é a regra do tipo. O estado do lançamento sai em status.
    # app/ponto/models.py:571-590,660-705,743-820
    # app/ponto/turnos.py:2548-2579; app/ferias/models.py:119-121,291-319
    em_aberto = [codigo for codigo, dados in tipos.items() if dados[1] == INDETERMINADO]
    ausencias = list(Ausencia.objects.filter(colaborador_id=cid, data__lte=dia).filter(
        Q(data=dia) | Q(data_fim__gte=dia) | Q(data_fim__isnull=True, tipo__in=em_aberto)
    ).order_by("data", "pk").values(
        "id", "tipo", "status", "data", "data_fim", "minutos", "hora_ini", "hora_fim",
        "aprovado_por_id", "aprovada_em", "agendamento_ferias_id", "chamado_origem_id",
    ))
    ferias = list(AgendamentoFerias.objects.filter(
        periodo_aquisitivo__colaborador_id=cid, data_inicio__lte=dia, data_fim__gte=dia,
    ).order_by("data_inicio", "pk").values("id", "status", "data_inicio", "data_fim"))
    linha("AUSENCIAS", ids=[a["id"] for a in ausencias], ferias_ids=[f["id"] for f in ferias])
    for ausencia in ausencias:
        efeito, _, medico = tipos.get(ausencia["tipo"], (NV, None, NV))
        linha("AUS", **ausencia, efeito_tipo=efeito, tipo_medico=medico)
    for agendamento in ferias:
        linha("FERIAS", **agendamento)
    return {a["chamado_origem_id"] for a in ausencias if a["chamado_origem_id"] is not None}


@lru_cache(maxsize=None)
def chamados_de(cid):
    return list(ChamadoColaborador.objects.filter(colaborador_id=cid).order_by("pk").values(
        "id", "status_local", "modulo_origem", "contexto_json", "celula_id", "celula__data",
        "justificativa_relacionada_id", "via_encerramento", "respondido_por_id", "resolvido_em",
    ))


def imprimir_chamados(cid, dia, batidas, chamados_ids):
    # Vínculos explícitos da pergunta: dia/célula, batida de origem e batida
    # plantada. Estado resume os atos lidos e os carimbos armazenados.
    # Visibilidade atual na fila [nao verificado].
    # app/chamados/models.py:2724-2760,3351-3370,3811-3838,4472-4515
    # app/chamados/juizes.py:1626-1628,1644-1653; app/ponto/models.py:40-44
    ids = {b["id"] for b in batidas}
    ids_texto = {str(pk) for pk in ids}
    origens = {b["pergunta_origem_id"] for b in batidas if b["pergunta_origem_id"] is not None}
    perguntas = (PerguntaDisputa.objects.filter(colaborador_id=cid).filter(
        Q(celula__data=dia) | Q(data_evento=dia, conversa=False)
        | Q(batida_origem_id__in=ids) | Q(pk__in=origens)
    ).select_related("disputa").prefetch_related("decisoes").order_by("pk"))
    for p in perguntas:
        lido = DecisaoPergunta.composta(p)
        respondida = lido.respondida_em is not None and bool((lido.resposta_colab or "").strip())
        estado = "respondida" if respondida else "pendente"
        if lido.materializada_em is not None:
            estado = "materializada"
        elif lido.validada_em is not None:
            estado = "validada"
        linha(
            "PERGUNTA", id=p.pk, estado=estado, motivo=p.motivo, data_evento=p.data_evento,
            celula_id=p.celula_id, batida_origem_id=p.batida_origem_id,
            batidas_plantadas_ids=[b["id"] for b in batidas if b["pergunta_origem_id"] == p.pk],
            disputa_id=p.disputa_id, disputa_fechada_em=p.disputa.fechada_em,
            chamado_pai_id=p.disputa.chamado_id, chamado_destino_id=p.chamado_destino_id,
            respondida=respondida, respondida_em=lido.respondida_em, validada_em=lido.validada_em,
            materializada_em=lido.materializada_em, via=lido.via_resolucao,
        )
        chamados_ids |= set(map(int, p.chamados_relacionados_ids or ()))
        chamados_ids |= {p.disputa.chamado_id, p.chamado_destino_id}

    # Ligações formais por justificativa, regularização ou ausência.
    # app/ponto/models.py:139-144,167-177,786-790
    # app/chamados/models.py:81-85,1923-1931,4658-4662,4687-4715
    justificativas = Justificativa.objects.filter(colaborador_id=cid).filter(
        Q(data_turno=dia) | Q(batida_entrada_id__in=ids) | Q(batida_gerada_id__in=ids)
    ).order_by("pk").values_list("pk", "chamado_origem_id")
    justificativas_ids = set()
    for jid, chamado_id in justificativas:
        justificativas_ids.add(jid)
        chamados_ids.add(chamado_id)
    itens = RegularizacaoItemBatida.objects.filter(regularizacao__colaborador_id=cid).filter(
        Q(data=dia) | Q(batida_entrada_id__in=ids)
        | Q(batida_saida_id__in=ids) | Q(batida_alvo_id__in=ids)
    )
    chamados_ids |= set(itens.order_by().values_list("regularizacao__chamado_id", flat=True))

    # Chamado por célula, data declarada ou batida no contexto. Seguimentos
    # mostram autoria e instante sem expor o conteúdo da conversa.
    # app/chamados/models.py:39-72,1835-1848
    # app/chamados/juizes.py:89-129; app/chamados/catalogo/modulos.py:219-225
    # app/chamados/services/acoes_chamado.py:244-248
    relacionados = []
    for ch in chamados_de(cid):
        ctx = ch["contexto_json"] or {}
        data_ctx = data_do_chamado(ch["modulo_origem"], ctx) or ctx.get("data_turno")
        batidas_ctx = {str(ctx.get("batida_id"))}
        batidas_ctx |= {str((o or {}).get("batida_id")) for o in ctx.get("ocorrencias") or []}
        if not (
            ch["id"] in chamados_ids or ch["justificativa_relacionada_id"] in justificativas_ids
            or ch["celula__data"] == dia or batidas_ctx & ids_texto
            or (ch["celula_id"] is None and str(data_ctx)[:10] == dia.isoformat())
        ):
            continue
        relacionados.append(ch["id"])
        linha("CHAMADO", id=ch["id"], estado=ch["status_local"], celula_id=ch["celula_id"],
              batidas_contexto_ids=sorted(map(int, batidas_ctx & ids_texto)),
              via_encerramento=ch["via_encerramento"], respondido_por_id=ch["respondido_por_id"],
              resolvido_em=ch["resolvido_em"])
    for resposta in (FollowupChamado.objects.filter(chamado_id__in=relacionados)
                     .order_by("criado_em", "pk")
                     .values("id", "chamado_id", "autor_tipo", "autor_user_id", "criado_em")):
        linha("CHAMADO_SEGUIMENTO", **resposta)


def main():
    if connection.schema_name != "juliani":
        raise RuntimeError("A sonda exige o schema juliani")
    dias, sobras = agrupar()

    # Leitura direta do catálogo para propagar falhas de consulta. O catálogo
    # da cópia é usado quando a consulta retorna zero tipos ativos.
    # app/ponto/catalogo/ausencias.py:86-105,240-281,430-435
    tipos = {codigo: (efeito, fim, medico) for codigo, efeito, fim, medico in
             TipoAusencia.objects.filter(ativo=True).order_by().values_list("codigo", "efeito", "fim", "medico")}
    if not tipos:
        tipos = {codigo: (dados[0], dados[5], dados[6]) for codigo, dados in CATALOGO.items()}
    empresas_cadastradas = dict(Colaborador.objects.filter(
        pk__in={cid for cid, _ in dias},
    ).order_by().values_list("pk", "empresa_id"))  # app/colaboradores/models.py:319-322

    total_batidas = 0
    for (cid, dia), (empresa, sobras_dia) in sorted(dias.items()):
        batidas = batidas_do_recorte(cid, dia, sobras_dia)
        trilha = trilhas_de([b["id"] for b in batidas])
        linha("DIA", colab_id=cid, data=dia, empresa=empresa,
              empresa_cadastro_id=empresas_cadastradas[cid], sobras_ids=sorted(sobras_dia))
        for batida in batidas:
            imprimir_batida(batida, trilha[batida["id"]], sobras)
        imprimir_escala(cid, dia)
        chamados_ids = imprimir_ausencias(cid, dia, tipos)
        imprimir_chamados(cid, dia, batidas, chamados_ids)
        total_batidas += len(batidas)

    # batidas conta linhas BAT emitidas, inclusive em janelas sobrepostas.
    print(f"FIM dias={len(dias)} batidas={total_batidas}")


main()
```

## 27/11 10:02 · sessao b89a8e9ccbee · PAR-PELO-TURNO · verificador · origem=Code

## Procedentes

1. **O mesmo PK ainda muda de dia.** Com DNA `21:30–05:42`, `E 23/10 01:30` e `S 24/10 00:00`, a casca e o lote publicam a entrada em **22/10** na consulta de 21/10 e em **23/10** na consulta de 22/10. A segunda consulta alcança a saída e reclassifica a entrada. Há divergência mesmo com todas as marcas carregadas. Com A noturno até 21/10, B diurno a partir de 22/10 e `E 21/10 21:30`, `S 22/10 08:00`, `S 22/10 20:00`, a última saída recebe **22/10** na casca e na consulta individual, mas **21/10** na mesma janela com `turnos=`. Dois segmentos reclamam o PK e a última atribuição vence. O RED de `r.txt` passou na geometria original de um vínculo, assim como o controle sintético de col297. A invariância exigida pela L-085 segue aberta. `app/docs/LEIS.md:117` · `app/ponto/turnos.py:287` · `app/ponto/turnos.py:1998` · `app/ponto/turnos.py:3037` · `app/ponto/turnos.py:3060` · `app/ponto/turnos.py:3082` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:57` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:73`

2. **A pré-passagem da pausa ainda perde o relógio da entrada.** O caso anterior da `E` interna passou com flag gravada e com `Xi`. O novo contraexemplo usa marcos de borda `19:00–07:00`, `E 20/10 19:00`, `S 19:00:30`, `S 21/10 01:00`, `E 02:00`, `S 07:00`. Sem marcos de borda, a sonda fecha `19:00→07:00` com 60 min de pausa. Com os marcos, ela produz `19:00→01:00` e `02:00→07:00`, sem pausa. A pré-passagem com bordas zera `limite` pela saída de 30 s, enquanto o laço principal conserva a entrada. O RED da saída degenerada só chama o ramo sem bordas. `app/ponto/turnos.py:435` · `app/ponto/turnos.py:447` · `app/ponto/turnos.py:508` · `app/ponto/turnos.py:1501` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:82` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:116`

3. **O item 2 do aval ainda falha por regra.** A sonda original com só `E 22/10 12:00` deu **0,00 h** nos dois braços. Em `21/09–20/10`, com previsão comercial e sem ausência, usei `E 21/10 01:30`, `S 22/10 00:00` com papel `Xi` e uma batida posterior em `23/10 08:00`. A casca dispõe do fato posterior da O95, fecha o par e data ambas as marcas em **21/10**. A projeção nova da folha passa `ultima_batida=None`, mantém o turno aberto e atribui ambas a **20/10**. Com zero períodos no motor, os trechos reais do fechamento deram **0,00 h** sem as marcas e **−193,60 h** com elas. Esse saldo alimenta o `FechamentoMensal` e o banco do `DiaPago`. `app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:204` · `app/ponto/turnos.py:1172` · `app/ponto/turnos.py:1186` · `app/ponto/turnos.py:1931` · `app/ponto/services/fechamento.py:165` · `app/ponto/services/fechamento.py:679` · `app/ponto/services/fechamento.py:1123` · `app/ponto/services/fechamento.py:1199` · `app/ponto/services/fechamento.py:1227`

4. **A segunda virada continua dependente do cadastro do dia da saída.** Para `E 20/10 23:30 → S 22/10 01:30`, o juiz devolve um par fechado de 26 h. A sonda de `_turno_noturno_completo` devolveu `True` com o vínculo noturno e `False` com vínculo diurno ou sem vínculo em 22/10. A guarda rejeita o caso antes de consultar o par. O teste usa o mesmo vínculo nas duas datas e simula o lote com a própria casca. A transferência das duas pontas para a fatia de A passou no caso anterior de 26 h, mas o teste usa dois vínculos sem `tipo_escala`, o mesmo DNA e ata vazia. A fatia continua consumindo o mapa sujeito à dupla atribuição do item 1. `app/escala/servico_jornada.py:230` · `app/escala/servico_jornada.py:245` · `app/escala/servico_jornada.py:354` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:125` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:149` · `app/ponto/services/fechamento.py:284`

5. **O item 1 do aval, 4b, continua reservado e produz diferença de dinheiro.** Com `E 19/10 22:00 → S 20/10 22:00` e `E 21/10 03:00 → S 21/10 11:00`, o juiz data o segundo par em **21/10**. No mesmo `Motor12x36`, variar só a carga da antiga borda do PDF para `ini−4` fez **8,00 h virarem 0,00 h** na janela iniciada em 21/10. A primeira jornada inteira passa a chegar ao motor, que aplica a herança pelo vão de 5 h e exclui o segundo par como dia 20. A folha já recebe a carga ampliada. O PDF livre passou a recebê-la nesta volta e soma a saída desse motor. A outra pendência reservada, 3c, também permanece. Para fim em 20/10, a testemunha da folha pede julgamento até 21/10, carrega até 23/10 inclusive e só recorta a comparação depois. Uma marca em `23/10 00:00` entra nessa apuração e fica fora da carga da competência. `app/docs/par_pelo_turno_c1v6_evidencia.txt:137` · `app/core/juizes.py:216` · `app/ponto/motor_calculo_v2.py:2516` · `app/ponto/motor_calculo_v2.py:2542` · `app/ponto/services/fechamento.py:655` · `app/relatorios/pdf_espelho.py:281` · `app/relatorios/pdf_espelho.py:495` · `app/folha/porta_export.py:77` · `app/folha/porta_export.py:106` · `app/folha/porta_export.py:114`

## Escritores e leitores

| Valor | Onde nasce e onde o contrato diverge |
|---|---|
| `janela_com_pad` e `batidas_apuraveis_na_janela` | `turnos.py` escreve `ini−4..fim+2`, por dias locais completos, do primeiro ao último microssegundo inclusive. Casca e lote usam filtros de data local. Grade, carga de perguntas, fechamento, autoridade do espelho e PDF leem a janela. O fechamento usa o helper também na passada de órfãos. A carga de perguntas escreve a borda equivalente como `< 00:00` do dia seguinte, mas recorta de um lote já carregado. A divergência demonstrada está no período pedido pela testemunha da folha e no provedor da O95. `app/ponto/turnos.py:1869` · `app/ponto/turnos.py:1895` · `app/ponto/turnos.py:2990` · `app/ponto/turnos.py:3001` · `app/ponto/turnos.py:3149` · `app/escala/utils.py:1352` · `app/chamados/services/carga_pergunta_viva.py:174` · `app/ponto/services/fechamento.py:831` · `app/ponto/services/espelho.py:1031` · `app/relatorios/pdf_espelho.py:281` |
| Dia por PK e dia do período | `_dia_das_batidas_puro` escreve o dia do turno reclamante e completa as demais marcas pelo dia civil. Espelho e app filtram por PK, painel, férias e vigia consultam PKs específicos, e disputa e fio chegam pela consulta individual. Na tela, a soma de horas passou a ler o dia publicado pelo motor, mas `turnos` e `turnos_abertos` ainda filtram pela entrada civil. No próprio caso 3d, `E 21/09 00:00 → S 07:50` atribuído a 20/09 soma **7 h 50 min** no recorte até dia 20 e acrescenta **zero** a `turnos`. `app/ponto/turnos.py:3070` · `app/ponto/services/espelho.py:435` · `app/ponto/services/espelho.py:451` · `app/ponto/services/espelho.py:588` · `app/api/views.py:1444` · `app/api/views.py:1461` · `app/core/services/painel_op.py:330` · `app/ferias/management/commands/listar_ferias_com_batida.py:54` · `app/ponto/management/commands/vigia_plantio_overnight.py:87` · `app/chamados/services/disputa_emissao.py:283` · `app/chamados/services/fio.py:1827` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:103` |
| `dias_inconsistentes` | A tela incrementa uma vez por dia inconsistente. O PDF reconta as linhas. `folha_manda` pode sobrescrever ambos com `FechamentoMensal.inconsistencias`, que conta anomalias e acrescenta 1 por `sem_movimento`. Com zero anomalias e nenhuma linha inconsistente, o contador de dias vale **0** e o fechamento sem movimento publica **1**. O badge, o app e a comparação da porta leem a mesma chave com esses dois significados. `app/ponto/services/espelho.py:670` · `app/ponto/services/espelho.py:685` · `app/relatorios/pdf_espelho.py:604` · `app/ponto/services/fechamento.py:1119` · `app/relatorios/cartao_pela_celula.py:113` · `app/relatorios/cartao_pela_celula.py:346` · `app/templates/ponto/espelho.html:67` · `app/api/views.py:1434` · `app/folha/porta_export.py:502` |
| `pdf_espelho` | O coletor lê as linhas de `espelho_do_colab`, mas monta insumos e roda `apurar_por_fatia` ou `calcular_mes` em paralelo. Em período livre, publica o próprio saldo do motor. O mesmo coletor alimenta a geração do PDF, a testemunha de exportação e a pauta de dias em aberto. Com `agora=25/11` e pedido `26–27/11`, o `max` faz a apuração começar e terminar em **26/11**, enquanto a tela consultada para as linhas termina em **24/11**. É uma divergência de teto usando o mesmo `agora`. O delta fornecido tem **+0** cláusulas `except`. `app/relatorios/pdf_espelho.py:277` · `app/relatorios/pdf_espelho.py:410` · `app/relatorios/pdf_espelho.py:426` · `app/relatorios/pdf_espelho.py:524` · `app/relatorios/pdf_espelho.py:589` · `app/relatorios/pdf_espelho.py:647` · `app/relatorios/pdf_espelho.py:1041` · `app/folha/porta_export.py:502` · `app/ponto/services/dias_em_aberto.py:112` · `app/docs/par_pelo_turno_c1v6_evidencia.txt:149` |

## O que os testes deixam passar [nao verificado]

- **c1v6.** Ata vazia derrubou o teste novo, fechando aquele mutante de `r.txt`. Encurtar a carga do PDF, restaurar o recorte civil da soma e usar `not batidas` também derrubaram as respectivas sondas. Em contrapartida, mudei só `dia <= _data_fim` para `dia < _data_fim` no `sem_movimento`. Os **95 testes e sondas em memória** passaram. Uma única marca apurável atribuída ao dia 20, sem ausência, passaria a ser tratada como mês sem movimento. Os dois braços do 4c exercitam apenas o caso sem fato na competência. `app/ponto/tests/test_par_pelo_turno_c1v6.py:182` · `app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:266` · `app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:319` · `app/ponto/services/fechamento.py:679` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:120`
- **c1v7.** O AST distingue os dois nomes por igualdade e contou uma chamada do embrulho na linha 281. Os cinco testes estruturais passaram em memória. Uma segunda chamada via `carregar = batidas_apuraveis_na_janela; carregar(...)` deixou o censo idêntico. A chamada real a `apurar_por_fatia` também está fora de `AUTORIDADES`. O verde do selo permite a derivação paralela do coletor. `app/relatorios/tests/test_pdf_nao_calcula.py:34` · `app/relatorios/tests/test_pdf_nao_calcula.py:63` · `app/relatorios/tests/test_pdf_nao_calcula.py:96` · `app/relatorios/pdf_espelho.py:281` · `app/relatorios/pdf_espelho.py:412`
- **c1v8.** Comparei os literais via AST. A fixture copia as nove chaves e valores de `espelho.py`, incluindo `dias_inconsistentes`. Ela corrige o `KeyError` da montagem manual e preserva a asserção de horas. Como injeta o próprio resumo e chama `montar_dias`, o 3d deixa passar a remoção de `dias_inconsistentes` do inicializador de produção e a remoção do incremento. A comparação da fixture com o contrato atual é exata, mas o teste não protege esse contrato contra mudança futura. `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:100` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:113` · `app/ponto/services/espelho.py:685` · `app/ponto/services/espelho.py:1236`

A execução Django parou no import de `django.core`. O `Ran 356 OK`, a identidade Git de HEAD e das bases, o efeito no gravado e no TXT, e o laudo dos 29 casos por rubrica ficam **[nao verificado]**. Para col155/08-11, a cópia só oferece o ensaio sintético de **782 min**, teto de **732 min** e uma C9. `app/manage.py:11` · `app/docs/BACKLOG.md:2027` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:195`

**REPROVADO — PK muda de dia e de dono (`app/ponto/turnos.py:3085`), pausa perde a entrada (`app/ponto/turnos.py:508`), 4c perde a prova da O95 e muda o saldo (`app/ponto/services/fechamento.py:165`), guarda noturna rejeita par fechado (`app/escala/servico_jornada.py:230`), 3c amplia a janela (`app/folha/porta_export.py:77`), 4b perde 8 h (`app/ponto/motor_calculo_v2.py:2521`), PDF recalcula e usa outro recorte (`app/relatorios/pdf_espelho.py:412`), resumo mistura dias e ocorrências (`app/relatorios/cartao_pela_celula.py:113`) e os testes deixam passar borda exclusiva, chamada indireta e perda da chave de produção (`app/ponto/services/fechamento.py:679` · `app/relatorios/tests/test_pdf_nao_calcula.py:77` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:113`).**

## 27/11 09:52 · sessao ca7d3c808b96 · - · - · origem=Ronald (tmux)

- **R4 e R5.** O cenário de intervalo permite exercitar origem aberta e marco efetivo entregue na virada do corte. Na base, o repasse grava os IDs de ambos os itens no agregado antes de tentar as perguntas. O nascimento distingue alteração de inserção e só aplica a guarda B2 à alteração. O teste pode observar `_nascer`, o dia efetivo e a tentativa de `INSERT`, além da releitura pública de `cobrar_dia`. `chamados/tests/test_fio_intervalo.py:45`, `ponto/janelas.py:60`, `folha/tests/test_export_vigente.py:33`, `chamados/services/disputa_emissao.py:1117`, `chamados/models.py:3192`, `chamados/models.py:3333`, `chamados/models.py:3847`, `chamados/services/cobrar_dia.py:45`.
- **R3.** O callback autônomo de silêncio de isentos já tem fixture com callback real. Adicionar TXT vigente e cobrar o registro do silêncio detecta uma extensão indevida de `_K6`. O beco tem cerca B2 própria e é outro cenário. `chamados/signals.py:285`, `chamados/tests/test_varredura_isentos_evento.py:43`, `chamados/services/silencio_isentos.py:34`, `chamados/services/becos.py:63`.
- **Volta 3.** O template imprime só a hora, o PDF consulta HH:MM e o supra só imprime a lista de horas. Os pontos observáveis de transporte são a ata em curso no emissor, `grade_da_celula` durante `_marcos_faltantes` e `grade_do_fechamento` na folha. `templates/ponto/espelho.html:242`, `relatorios/pdf_espelho.py:472`, `relatorios/pdf_espelho.py:607`, `ponto/supra_juiz.py:527`, `ponto/services/cartorio.py:1505`, `chamados/services/declaracao_texto.py:101`, `folha/export.py:329`.

Execução dos cenários [nao verificado].

## 27/11 09:51 · sessao 74ba2bb386d4 · - · - · origem=Ronald (tmux)

- R3 tem um caminho autônomo observável. O `on_commit` de `retoma_regularizacoes_ao_fechar_disputa` chama a retomada fora do cartório. Um mutante que condicione essa chamada à ausência de TXT vigente deve fazer falhar o teste que observa a retomada com TXT. O caminho de beco já tem cerca própria. `chamados/signals.py:122–146`, `chamados/services/becos.py:63–71`
- R4 tem um ponto de asserção concreto. O repasse grava os IDs de todos os candidatos no contexto da disputa e do pai antes de processar as perguntas. O cenário misto deve exigir `[admitido.pk]` nos dois contextos e a trilha individual da vetada. `chamados/services/disputa_emissao.py:1100–1142`, `chamados/tests/test_e4a2_nunca_bateu.py:81–114`
- R6 pode interpor um residual depois do primeiro `carimbar` real. `vigiar_escrita_de_perguntas` é inerte fora da cerca e sinaliza o veto dentro dela. `executar_sob_cerca` desfaz o savepoint mesmo quando a exceção interna foi capturada. O cartório atual chama `julgar_colab` e aplica a precedência sem usar essa cerca. `ponto/services/cartorio.py:1905–1932`, `ponto/services/cartorio.py:2078–2134`, `chamados/services/fio.py:1536–1592`
- Desenho A. `julgar_celula` usa a data e a hora ambientes na margem do turno. Gate e tripwire convergem em `_vivas_entre`, que chama o C6 sem o instante explícito já aceito pelo juiz. O RELATO descreve o cenário C6 `0→1` após a primeira lavra. O percurso Django integral desse cenário segue [nao verificado]. `ponto/services/cartorio.py:2083–2115`, `chamados/models.py:236`, `chamados/models.py:368–402`, `chamados/juizes.py:911–943`, `docs/RELATO.md:1123`
- E2 e fábrica. Há uma fixture de pergunta absolvida e uma de carimbo sem lastro para montar dois dias do mesmo colaborador com TXT vigente em apenas um. A asserção deve observar tentativas de escrita, além da foto final, para distinguir o mutante que guarda depois do carimbo. `chamados/tests/test_e4c_f5_guarda_reconciliador.py:141–174`, `chamados/tests/test_o261_julgamento_desfaz_carimbo.py:49–104`, `folha/tests/test_k6_fechada_para_celula.py:32–44`, `chamados/reconciliador.py:695–707`, `chamados/services/fabrica_da_celula.py:206–219`

Resultados de execução [nao verificado].

## 27/11 09:50 · sessao e5a6d7e4fdc0 · - · - · origem=Ronald (tmux)

- R1 pode pedir a autoridade `escala.orfa_classe.razao_da_sobra`. A base tem `classificar_orfas`, que opera por minuto. O sítio futuro consta em `docs/RELATO.md:361,2639` e `docs/BACKLOG.md:2087`. O controle de 11 s confere a sobra e o carimbo de recusa no pareador. A pausa de 8 s pode usar as bordas 08:00–17:00 e o intervalo cadastrado 12:00–13:00, com marcas 12:30:00 S e 12:30:08 E, e verificar um turno de 480 minutos com 60 minutos de janela descontada. `escala/orfa_classe.py:53`, `ponto/turnos.py:130–162,707–771,932–940,1356–1376,1482–1490`.
- R2 pode pedir a autoridade pública `sobras_da_ata` e usar `grade_da_celula` como controle independente da foto. A base usa `_orfas_da_ata`, que entrega duas entradas `12:00` quando a ata contém duas sobras no mesmo minuto. `escala/services/leitor_celula.py:180–190,350–428`.
- V1 pode alimentar a porta pública da grade com marcas sintéticas distintas no mesmo minuto, no mesmo instante e em duas datas civis. A tupla de entrada perde a identidade e a saída conserva apenas `HH:MM`. O RED de conservação pode exigir identidade, instante, linha dona e razão. `escala/utils.py:1367–1368,1483–1484,1863–1891,2138`.
- A reconciliação da grade remove uma entrada pelo minuto, o que fornece o sítio para cobrar a remoção da ocorrência exata. `escala/utils.py:2047–2060,2093`.

Execução dos casos e arquivos integrais dos anexos [nao verificado].

## 27/11 09:48 · sessao c472a9d1c2c0 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

Conferi os cinco arquivos alterados contra o diff anexo e a cópia da base `59600163c`. Os hashes dos cinco blobs conferem com o anexo. A cópia veio sem `.git`, como prevê `CLAUDE.md:375-379`. O objeto do commit, sua relação de parentesco e o estado da VM estão [nao verificado]. Fiz a conferência em modo somente leitura.

## 1. Pronto do aval

| Item | O que a cópia demonstra | O que falta |
|---|---|---|
| Régua | Consulta o juiz antes do atalho `JA VERDE`. Uma recusa grava `FALHOU` e encerra antes do Docker. A consulta ocorre antes das travas e a montagem posterior usa a árvore viva. [bin/regua.sh:57](/home/ronald/codex_ws/2c2f212fca4b8b2af80aa979ddeb0934f18e0201/bin/regua.sh:57), `bin/regua.sh:102-122,183-187`. | RED da régua com teste novo inventariado, antes do opus [nao verificado]. |
| Vigia | `_rodar_testes` ganhou a guarda. O vigia horário ainda faz a suíte do retrato, o reteste dos vermelhos e a confirmação na árvore viva sem consultar o juiz. O próprio vigia Python pode relançar esse shell. [bin/vigia_arvore.sh:77](/home/ronald/codex_ws/2c2f212fca4b8b2af80aa979ddeb0934f18e0201/bin/vigia_arvore.sh:77), `bin/vigia_arvore.sh:38-40,83-88,105-106,131-135`, `app/config/crons.py:296`, `app/core/esteira_vigia.py:750-756,1187-1195`. | A cobertura operacional do vigia falha. RED de ponta a ponta na VM [nao verificado]. |
| Vizinhos da volta | `vizinhos_da_volta.py` seleciona labels, inclusive do teste tocado no diff. `suite.sh --volta` consome essas labels e tenta a pista. Se cair na VM, consulta o juiz antes da fila e novamente na vez. [bin/vizinhos_da_volta.py:92](/home/ronald/codex_ws/2c2f212fca4b8b2af80aa979ddeb0934f18e0201/bin/vizinhos_da_volta.py:92), `bin/vizinhos_da_volta.py:25-30,128-132`, `bin/volta_intermediaria.sh:32-46`, `bin/suite.sh:139-160,214-229,514-544`. | RED do caminho `--volta` na VM [nao verificado]. |
| Mesmo teste na pista | A rota envia SHA e labels e confere recibo e log. O selo de rota usa SSH falso e imprime `Ran` simulado. `bin/suite.sh:433-459,474-509`, `bin/tests/test_suite_rota_pista.sh:27-44,61-64`. | Recibo e log identificando o mesmo teste dos três REDs, na árvore enviada à pista [nao verificado]. |
| Opus final e LIBERA-B3 | A regra da cópia exige a suíte inteira verde antes do opus final. `CLAUDE.md:421-431`. | Suíte inteira verde, opus final `APROVADO` para este diff e cumprimento do restante do LIBERA-B3 [nao verificado]. | [nao verificado]

A lista positiva preservada pelo aval também diverge da cópia. O wrapper disponível proíbe `motor_calculo_v2.py` e `models.py` e aplica uma lista de proibições aos arquivos tocados. `bin/codex_pergunta.sh:159-176,224-234,643-647`.

## 2. Quem escreve, quem lê e o que cada valor significa

| Valor | Escritor, leitores e semântica |
|---|---|
| `.inventario` | O wrapper compara a worktree com o hash-base e publica o inventário **depois** de o construtor sair. `TOCADO` cobre arquivo novo, modificação, remoção e mudança de modo ou tipo. O wrapper relê o inventário para montar `.recusa`. O juiz lê apenas `TOCADO` e caminho, e escolhe a ocorrência de maior `mtime`. Portanto, `TOCADO` prova diferença contra a base daquela chamada. A ocorrência mais recente por caminho, sozinha, não identifica quem produziu os bytes da árvore montada. `bin/codex_pergunta.sh:143-149,195-234,781-807,811-824`, `bin/codex_teste_fora.py:68-94,152-164`. |
| `.id` | O wrapper escreve `obra,papel,hash,pid,fim,sha` após `rc=0` com resposta nova. O SHA é **da resposta**. O disjuntor valida os campos e o SHA sob a trava de resposta. O juiz CCM lê `obra` e `papel` sem essa trava. `obra` vem de `CODEX_OBRA`, enquanto `id_chamada` identifica uma execução. No transcript, `obra` vem da primeira identificação reconhecida no prompt, por outra gramática. `bin/codex_pergunta.sh:117-121,421,549,725-744,797-798,854-860`, `bin/disjuntor.py:159-175,607-632,856-863,880-905`, `bin/codex_teste_fora.py:85-94`. |
| `refs/heads/main` e bytes | Git grava a ref local quando o pouso avança `main`. `git revert` também a avança se `HEAD` estiver nessa branch. O juiz lê a ponta e o histórico em dois comandos, sem fixar um SHA comum, e compara os blobs aos bytes do arquivo montado. A busca histórica pode aceitar um blob de commit lateral alcançável após um merge, mesmo que esse blob jamais tenha estado na ponta de `main`. A ref real na VM está [nao verificado]. `CLAUDE.md:347-354`, `app/core/esteira_vigia.py:1316,1332`, `bin/codex_teste_fora.py:98-129,152-166`. |
| Transcript final | O harness é identificado como escritor do JSONL. O agente entrega o texto via `SubagentHandback`. O B3 lê o diretório da sessão e exige `agentType=verificador`. O CCM reutiliza o parser, varre todas as sessões do caminho fixo, dispensa metadado de tipo e modelo e escolhe o maior timestamp **entre os transcripts com veredito**. Uma verificação posterior sem veredito deixa a anterior em vigor. Nenhum SHA do diff é conferido nessa decisão. `bin/disjuntor.py:438-442,649-671,839-853`, `bin/codex_teste_fora.py:44-45,132-149,167-173`. O escritor físico e o transcript desta volta estão [nao verificado]. |
| Instante, `mtime` e fuso | O juiz compara o timestamp da **primeira linha** do transcript com o `mtime` do inventário e o do arquivo montado. `Z` ou offset explícito vira epoch. Timestamp sem offset é interpretado no fuso local do processo. O `fim` do `.id` é hora civil sem offset e fica fora da comparação. Início do agente, entrega do veredito e modificação no filesystem são eventos distintos. O fuso e os valores reais estão [nao verificado]. `bin/codex_pergunta.sh:854-859`, `bin/codex_teste_fora.py:91-94,132-148,169-173`. |
| RC do juiz e `None` | `main` emite `0` sem pendências detectadas, `1` com pendência e `3` para argv inválido. `NaoSei` sobe como exceção sem tratamento, com traceback e rc não zero. Régua traduz qualquer rc não zero em `1`, suite local em `3` e `_rodar_testes` grava stdout/stderr e devolve `None`. `_isolar` pula o cache, mas `classificar` pode devolver `CAIU_FATIA` com “não isolado”. O congelado devolve recusa explícita, o auto-revert conserva o revert, e a autocura descarta `None` pelo mesmo filtro que descarta `False`. `bin/codex_teste_fora.py:39-49,177-188`, `bin/regua.sh:57-68`, `bin/suite.sh:139-149`, `app/core/esteira_vigia.py:140-144,740-756,1324-1334,1377-1382,1467-1475`. |
| Carimbo, estado e avisos | A régua escreve `.regua_stamp` com impressões da árvore, status, epoch, hora civil e resumo. Leem a própria régua, o pre-push, as cadeias e o rodapé. O vigia escreve `ultimo_revert`, lido pelo limitador de uma hora. O aviso passa pelas portas existentes de Pauta e RELATO. A Pauta deduplica pela chave `auto_revert`, então uma Pauta já viva pode conservar o texto anterior. `bin/regua.sh:38-45,60-67,102-109,214-221`, `bin/pre-push.sh:52-70`, `bin/molde_fatia/cadeia.sh:72,82`, `bin/tickets_rodape.sh:29-33`, `app/core/esteira_vigia.py:852-853,946-948,1072-1074,1324-1329`, `app/core/management/commands/alarme_esteira.py:34-41,54-57`. |

## 3. Classes de reprovação

| Classe | Resultado |
|---|---|
| Segundo juiz registrado | **Ausente como cópia do parser**. O CCM chama `_veredito_e_obra`. **Presente como decisão paralela de quem pode aprovar**. O B3 exige metadado de verificador, enquanto o CCM pode aceitar um agente de outro tipo pelo prompt e pelo `Handback`. A L-011 trata da entrada no registro vivo, que este diff não altera. `bin/codex_teste_fora.py:52-56,138-149`, `bin/disjuntor.py:649-671,839-853`, `app/core/juizes.py:190`, `app/docs/LEIS.md:69`. |
| Segundo escritor fora da porta | **Ausente para inventário, recibo e transcript no delta. Presente como novo ponto de gravação fora da trava da régua**, no próprio script, inclusive com `--only`. Há ainda uma sobrescrita preexistente no vigia. `passada` lê o estado antes do auto-revert e pode gravar essa cópia depois, apagando `ultimo_revert`. `bin/codex_teste_fora.py:68-174`, `bin/regua.sh:47-68,115-122,198-221`, `app/core/esteira_vigia.py:1324-1325,1523-1525,1613,1632-1634`. |
| Relógio fora da borda | **Presente.** O ramo novo da régua lê `date` duas vezes no mesmo carimbo. Epoch e hora impressa podem cair em minutos diferentes. O novo relato do auto-revert formata o `agora` recebido. `bin/regua.sh:64-65`, `app/core/esteira_vigia.py:1294-1301,1329`. |
| Derivação paralela | **Presente.** CCM repete a conversão ISO para epoch e introduz outro leitor de início de transcript. Repete também a fórmula do blob. As `LABELS` continuam vindo da régua, conforme L-075. `bin/codex_teste_fora.py:126-135`, `bin/disjuntor.py:434-435`, `bin/orcamento_contexto.py:161-174`, `bin/codex_pergunta.sh:180-182`, `bin/regua.sh:23`, `bin/suite.sh:209`, `app/docs/LEIS.md:102`. |
| Selo ou RED sem mordida | **Mordida unitária presente.** `test_vigia_ccm.py` simula rc `1`, `2` e `-15` e exige uma única chamada de subprocesso, o juiz. Tirar a guarda quebra essa asserção. Os demais casos simulam o retorno ou o Docker. O teste deixa de fora o juiz real, o cron do vigia e o percurso VM/pista. RED executado no HEAD pelo motivo certo [nao verificado]. `app/core/tests/test_vigia_ccm.py:22-57,61-137`, `.claude/agents/verificador.md:9-10`. |
| Ordem de trava | **Presente.** Régua julga e carimba antes da trava própria e julga antes da espera pela trava de testes. A suite local normal reconsulta sob a trava, mas entre a consulta e a montagem ainda usa uma árvore mutável. O vigia Python tem a trava da passada quando chamado pelo wrapper e seu reteste direto não passa pela trava compartilhada da suíte. `bin/regua.sh:57-68,115-122,183-187`, `bin/suite.sh:149-160,544,575-584`, `bin/trava_teste.sh:35-42`, `bin/vigia_esteira.sh:8-11`, `app/core/esteira_vigia.py:748-772`. |
| Pouso antes do pré-requisito | **Presente no auto-revert.** Reverte e chama deploy antes de consultar o juiz no reteste. A recusa conserva a mudança. Um pouso efetivo desta volta está [nao verificado]. `app/core/esteira_vigia.py:1316-1333`. |
| Campo com dois significados | **Presente.** `.regua_stamp` é descrito como testemunha de que a régua rodou. O ramo CCM escreve `FALHOU` e horário por recusa pré-execução. Na autocura, `None` também cai no filtro de vermelho e `passada` registra `autocura_de`, o que pode impedir nova tentativa após a liberação enquanto a lista de falhas continuar igual. `bin/artefatos_de_estado.txt:28`, `bin/regua.sh:57-68,193-221`, `app/core/esteira_vigia.py:1467-1475,1560-1563,1632-1634`. |
| `except` novo | **Ausente. Zero no diff inteiro**, inclusive `bin/`. A conferência por AST encontrou 25 → 25 handlers no vigia e zero nos dois Python novos. O parser herdado do disjuntor já descarta linhas JSON inválidas, portanto a promessa de traceback para toda leitura que falha é mais ampla que o comportamento. `app/core/esteira_vigia.py:748-772`, `app/core/tests/test_vigia_ccm.py:22-57`, `bin/codex_teste_fora.py:36-65,132-149`, `bin/disjuntor.py:445-453`. |

## 4. O executor recusa todo rc não zero?

**Sim, quando o juiz é chamado para a árvore efetivamente montada e retorna rc não zero**, inclusive traceback. A régua sai `1`, a suite local sai `3` e `_rodar_testes` volta antes de `docker exec` e `docker run`. Se a chamada ou a gravação do motivo levantar no próprio vigia, o teste também fica sem iniciar, mas o chamador recebe exceção em vez de `None`. `bin/regua.sh:57-68`, `bin/suite.sh:139-160`, `app/core/esteira_vigia.py:750-770`.

**Há caminhos locais que escapam.** O vigia horário e o relançamento via Python executam pelo shell sem juiz. Na régua e na suite normal, o bind-mount pode ver alterações posteriores à consulta. Além disso, `--na-vez` julga o app indicado em `_NV_DIR` e executa `"$@"` sem conferir se o comando monta aquele app. O chamador normal fornece o app correspondente e passa pela trava. `app/core/esteira_vigia.py:1187-1195`, `bin/vigia_arvore.sh:83-88,105-106,131-135`, `bin/regua.sh:57,183-187`, `bin/suite.sh:146-160,575-584`.

**Também há liberações com rc 0 indevido pela regra do aval.** Um teste já escrito durante uma chamada ativa ainda pode estar sem inventário, que só nasce no fim. Um teste novo na raiz de `ferias`, como `app/ferias/test_ccm.py`, pode entrar na seleção e ficar fora da guarda, que exige uma pasta `tests/`. O mesmo descompasso existe na cópia para testes na raiz, como `app/ferias/tests_disponibilidade.py`. `bin/codex_pergunta.sh:738-745,781-807`, `bin/codex_teste_fora.py:59-61,78-81,152-157`, `bin/vizinhos_da_volta.py:33-45,92-94`, `app/ferias/tests_disponibilidade.py:30-31`.

**Sim, o auto-revert faz `git revert` primeiro.** Depois invoca `deploy.sh --sem-sombra`, sem usar o rc do deploy, e só então pergunta ao juiz pelo reteste. A recusa grava o estado e chama Pauta/RELATO, deixando o revert aplicado. `app/core/esteira_vigia.py:1316-1333`.

**Há uma regressão adicional nos selos da régua.** As fixtures de `test_suite_rota_pista.sh` e `test_suite_motivo.sh` copiam `suite.sh` sem instalar o juiz. Nos casos locais que exigem chegada à trava, o novo código tenta abrir o script ausente e sai `3` antes dela. A régua roda esses selos antes do Django. É uma falha derivada do código, com execução [nao verificado]. `bin/tests/test_suite_rota_pista.sh:27-37,239-240,377-379`, `bin/tests/test_suite_motivo.sh:23-47,73-78`, `bin/suite.sh:139-145,544`, `bin/regua.sh:152-163`.

## 5. Recusa indefinida de árvore legítima

- **Inventário sem recibo.** Se o inventário mais recente lista `app/folha/tests/test_ccm.py`, o recibo está ausente e a cópia tem bytes inéditos em `main`, a obra vira `None`. Nem um transcript aprovado da obra real pode liberar esse registro. O juiz não aplica prazo de expiração. `bin/codex_teste_fora.py:82-94,158-174`.
- **Merge com bytes diferentes.** Se a cópia legítima tem versão A e o merge levou só a versão B, A continua pendente quando seus bytes nunca apareceram no histórico consultado e não há aprovação reconhecida para a cópia. Uma árvore montada com B passa pela igualdade com `main`. `bin/codex_teste_fora.py:98-129,158-173`.
- **Transcript ausente ou cópia recriada.** Com recibo válido e bytes inéditos em `main`, um transcript ausente do caminho pesquisado mantém a recusa por tempo indeterminado. Mesmo com transcript `APROVADO`, recriar a cópia com os mesmos bytes e `mtime` posterior ao início do agente bloqueia de novo. A condição compara datas e dispensa uma identidade dos bytes aprovados. `bin/codex_teste_fora.py:44-45,132-149,167-174`. Ocorrências desses casos na VM [nao verificado].

**REPROVADO** — Críticos: vigia executa sem juiz, cobertura incompleta do inventário e aprovação sem vínculo ao opus e ao diff (`bin/vigia_arvore.sh:83-88`, `bin/codex_teste_fora.py:59-61,138-173`). Altos: REDs, mesmo teste na pista e opus final [nao verificado], regressão dos selos locais, auto-revert antes do reteste e recusas indefinidas (`bin/tests/test_suite_motivo.sh:23-47,73-78`, `app/core/esteira_vigia.py:1316-1330`, `bin/codex_teste_fora.py:82-94,158-174`). Médios: carimbo fora da trava, relógio duplicado e recusa absorvida por consumidores (`bin/regua.sh:57-68,115-122`, `app/core/esteira_vigia.py:140-144,1467-1475,1560-1563`).

## 27/11 09:48 · sessao dda30177af91 · - · - · origem=Ronald (tmux)

**REPROVADO.** O caminho periódico de `vigia_arvore.sh` continua rodando na VM sem consultar o juiz. O cron está declarado em `app/config/crons.py:296`. O vigia fotografa a árvore viva por `rsync` em `bin/vigia_arvore.sh:38-40`, roda a suíte em `:83-88`, repete os vermelhos em `:105-106` e pode confirmar direto na árvore viva em `:131-135`. Atividade em produção [nao verificado].

- **Régua.** Todo rc não zero encerra antes do atalho e do Docker, com carimbo global `FALHOU`. A checagem antecede ambas as travas e o bind da árvore continua mutável. O novo ramo carimba inclusive no `--only`, cujo caminho normal promete não carimbar. `bin/regua.sh:47-68`, `:102-122`, `:183-199`. RED [nao verificado].
- **Vigia da esteira.** Todo rc não zero encerra `_rodar_testes` antes do Docker, grava o diagnóstico e devolve `None`. `_isolar` evita o cache, mas `classificar` ainda pode declarar `CAIU_FATIA` com motivo `GREEN vermelho (nao isolado)`. O congelado trata `None` separadamente e a autocura o descarta pela mesma verdade booleana de `False`. `app/core/esteira_vigia.py:140-144`, `:740-756`, `:1377-1381`, `:1467-1475`. O teste novo morde a integração local sob simulação de subprocesso e falharia contra a versão anterior, mas não executa o juiz real. `app/core/tests/test_vigia_ccm.py:22-57`, `:61-72`. RED executado [nao verificado].
- **Vizinhos.** Seleciona labels do diff `<base>..HEAD`, inclusive teste novo já presente no diff, sem lançar processo de teste. `bin/vizinhos_da_volta.py:25-30`, `:92-94`, `:116-130`. `bin/volta_intermediaria.sh:33` delega a `suite.sh`, que envia SHA e labels à pista antes do ramo local, e no fallback recusa qualquer não zero antes da fila e novamente sob a trava. `bin/suite.sh:139-160`, `:220-230`, `:454-459`, `:516-520`, `:544`, `:576-583`. RED e mesmo teste na pista [nao verificado].
- **Ordem.** O auto-revert executa `git revert` e invoca deploy antes do reteste que consulta o juiz. Recusa grava `ultimo_revert`, deixa o revert aplicado e avisa; o rc do deploy não é consultado. `app/core/esteira_vigia.py:1316-1333`. Na suíte, `--na-vez` aceita chamada direta e executa o argv recebido sem verificar que seu bind corresponda ao app julgado, embora a chamada normal passe a árvore correta pela trava. `bin/suite.sh:146-160`, `:576-583`.
- **Regressão dos selos de host.** A régua roda os scripts `test_*.sh`. Pelo código, os cenários locais de três selos agora recebem rc 3 antes do resultado esperado porque suas raízes falsas têm `suite.sh` e os dublês antigos, mas não `codex_teste_fora.py`. `bin/regua.sh:156-162`, `bin/suite.sh:139-145`, `:544`, `bin/tests/test_suite_rota_pista.sh:27-37`, `:223-240`, `bin/tests/test_suite_motivo.sh:23-45`, `:240-249`, `bin/tests/test_rm_com_guarda.sh:113-122`, `:346-347`. Execução dos selos [nao verificado].
- **Teste sem inventário.** O wrapper remove o inventário velho antes da chamada e produz o novo só ao fim. A ausência de entrada é interpretada como ausência de pendência. A forma reconhecida pelo juiz também exclui teste Django na raiz de app, que `vizinhos` reconhece. `bin/codex_pergunta.sh:738-745`, `:776-789`, `:800-807`, `bin/codex_teste_fora.py:59-61`, `:152-157`, `bin/vizinhos_da_volta.py:33-45`.

## 27/11 09:47 · sessao 21ebe8cdcd3c · - · - · origem=Ronald (tmux)

**REPROVADO.** O vigia tem caminho local sem consulta, a régua tem janela entre consulta e execução, os selos existentes de suite perdem os casos positivos de VM, e faltam as provas exigidas de RED, pista e opus. [bin/vigia_arvore.sh:65,83,131] [bin/regua.sh:57,115,156,183] [bin/tests/test_suite_rota_pista.sh:27,189] [bin/tests/test_suite_motivo.sh:25,73] [nao verificado] [nao verificado]

Os cinco blobs calculados na cópia conferem com o pós-diff anexo. A cópia veio sem `.git`, então a autenticação do commit inteiro, do pai e do histórico de execução continua [nao verificado]. [nao verificado]

- **Régua:** a guarda cobre rc não zero antes de `JA VERDE`, grava `FALHOU` e sai 1. Consulta e gravação precedem o flock e a espera da suite, sem nova consulta sob a trava. Ela também grava carimbo em `--only`, apesar do contrato do parcial. [bin/regua.sh:47,57,102,115,183,198] RED CCM executado [nao verificado]. [nao verificado]
- **Vigia:** `_rodar_testes` para antes de Docker. O cron da árvore e o próprio vigia Python também chamam `bin/vigia_arvore.sh`, que executa suite e confirmação local sem juiz. [app/core/esteira_vigia.py:748,1187] [app/config/crons.py:296] [bin/vigia_arvore.sh:83,105,131] `test_vigia_ccm.py` morde a regressão de ignorar o rc ou seguir para Docker, com rc simulados 1, 2 e -15. Cobre rc 0 e consumidores de `None` com mocks. RED do HEAD, inventário real, rc 3 e traceback real [nao verificado]. [app/core/tests/test_vigia_ccm.py:22,38,50,60] [.claude/agents/verificador.md:10]
- **Vizinhos:** o helper lê a árvore e emite labels, a suite decide a execução. O caminho VM é consultado antes da fila e sob a trava, o recorte com cópia pode sair pela pista. O critério Django do helper inclui `test*.py` na raiz do app, enquanto o CCM exige diretório `tests/`. [bin/vizinhos_da_volta.py:25,33,92,116] [bin/codex_teste_fora.py:59] [bin/suite.sh:139,149,214,514,544] RED CCM da volta e mesmo teste na pista [nao verificado]. O selo da pista usa SSH, trava e Docker simulados. [bin/tests/test_suite_rota_pista.sh:11,27,56] `opus final APROVADO`, suite inteira verde e restante do LIBERA-B3 [nao verificado]. [CLAUDE.md:421,427] [nao verificado]
- **Regressão dos selos de host:** as fixtures de `test_suite_motivo` e `test_suite_rota_pista` copiam `suite.sh` sem copiar ou simular `codex_teste_fora.py`. Os casos positivos que esperam a trava recebem 3 antes dela. A própria régua executa esses selos antes do Django. [bin/tests/test_suite_motivo.sh:25,73] [bin/tests/test_suite_rota_pista.sh:27,189,295] [bin/suite.sh:139,544] [bin/regua.sh:152] [nao verificado]
- **Classes:** o parser de veredito é compartilhado, mas a seleção da aprovação é nova, ampla e sem a validação `agentType=verificador` do B3. Projeções repetem o cálculo de blob e ISO→epoch. A régua também acrescenta duas leituras do relógio do host, e o carimbo documentado como prova de que rodou passa a registrar recusa sem execução. Há novo ponto de gravação no mesmo escritor antes do flock. Pauta/RELATO continuam pelas portas existentes. [bin/disjuntor.py:434,649,839] [bin/codex_teste_fora.py:126,132,138] [bin/codex_pergunta.sh:180] [bin/regua.sh:57,115,214] [bin/artefatos_de_estado.txt:28] [app/core/esteira_vigia.py:825,946,1328] Zero novos `except` sintáticos no diff anexo. O `auto_revert` aplica o revert e chama deploy antes da consulta; se ela recusa, mantém revert, grava o limitador e avisa. [app/core/esteira_vigia.py:1316,1320,1322,1324,1326] [nao verificado]

## 27/11 09:46 · sessao 855e05c10268 · RC-SEM-PROXIMA · censo · origem=Code

## CENSO
- leitores: A cópia fornecida contém `RC SEM COLHER`. `RC SEM PROXIMA` aparece no anexo, com presença no HEAD [nao verificado]. As linhas abaixo foram conferidas na cópia do hash informado. A identidade Git do HEAD é [nao verificado].
  - `bin/disjuntor.py:1967` executa o juiz pela string `ESPERA_UNICA` antes do laço. `bin/disjuntor.py:2166`, `bin/disjuntor.py:2170` e `bin/disjuntor.py:2173` oferecem essa string nas negativas de `espera_nega`. `bin/disjuntor.py:2192` faz o mesmo na negativa de polling. O PreToolUse chama ambos em `bin/disjuntor.py:2226`, ligado por `.claude/settings.json:68` e `bin/disjuntor_settings.json:11`. `espera_nega` verifica a forma do `find` e o teto do `timeout`. A presença da chamada ao juiz nessa validação é [nao verificado] em `bin/disjuntor.py:2163`, `bin/disjuntor.py:2169` e `bin/disjuntor.py:2172`.
  - `bin/tests/test_rc_sem_colher.sh:2` declara o selo, `bin/tests/test_rc_sem_colher.sh:18` verifica a presença do arquivo, `bin/tests/test_rc_sem_colher.sh:50` executa o CLI e `bin/tests/test_rc_sem_colher.sh:54` confere a saída. O selo usa `ESPERA_UNICA` para acordar com lista, aguardar publicação por rename e propagar falha em `bin/tests/test_rc_sem_colher.sh:75`, `bin/tests/test_rc_sem_colher.sh:84`, `bin/tests/test_rc_sem_colher.sh:86` e `bin/tests/test_rc_sem_colher.sh:95`. Confere a forma e muda cópias do juiz e do disjuntor nos mutantes de `bin/tests/test_rc_sem_colher.sh:103`, `bin/tests/test_rc_sem_colher.sh:111`, `bin/tests/test_rc_sem_colher.sh:124` e `bin/tests/test_rc_sem_colher.sh:128`. O selo de agente usa a constante para aceitação, polling e paridade com a documentação em `bin/tests/test_agente_no_modelo.sh:210`, `bin/tests/test_agente_no_modelo.sh:244`, `bin/tests/test_agente_no_modelo.sh:278`, `bin/tests/test_agente_no_modelo.sh:283`, `bin/tests/test_agente_no_modelo.sh:301`, `bin/tests/test_agente_no_modelo.sh:345` e `bin/tests/test_agente_no_modelo.sh:346`.
  - Documentação da forma e do contrato em `CLAUDE.md:335`, `CLAUDE.md:341`, `app/docs/RELATO.md:13639`, `app/docs/RELATO.md:13640`, `app/docs/RELATO.md:13648`, `app/docs/RELATO.md:13655`, `app/docs/PROMPTS.md:3091`, `app/docs/PROMPTS.md:3093` e `app/docs/BACKLOG.md:2183`. `~/vercodex.py` é citado em `app/docs/PROMPTS.md:3070` e a chamada com `--min-idade 900` em `app/docs/RELATO.md:13640` e `bin/rc_sem_colher.py:51`. Seu código, a forma de chamada, o filtro de saída e qualquer import direto do juiz são [nao verificado]. `logs/espera_unica.sh` é citado em `app/docs/RELATO.md:13598`, sob `logs/` ignorado em `.gitignore:53`. Seu conteúdo e versionamento são [nao verificado].
  - Das marcas, o parser verificado é `bin/rc_sem_colher.py:26`, `bin/rc_sem_colher.py:33` e `bin/rc_sem_colher.py:80`. O Stop lê o topo e chama o checador de prova de ato em `bin/hook_stop_fila1.py:462` e `bin/hook_stop_fila1.py:1441`. O handoff lê `PAREI`/`lei`/`!` no topo e consulta a ordem viva em `bin/handoff_sessao.sh:97`, `bin/handoff_sessao.sh:116` e `bin/handoff_sessao.sh:320`. `gerar_estado` lê raias e interrupções em `bin/gerar_estado.py:215` e `bin/gerar_estado.py:236`. `gerar_codex_estado` usa `chamadas.log` em `bin/gerar_codex_estado.py:41`. A publicação carrega o RELATO como documento em `bin/relato.sh:125` e `bin/relato.sh:130`. A leitura das marcas por esses consumidores ou pelo vercodex é [nao verificado]. O Stop como leitor de RC sem colher aparece no contrato anterior em `app/docs/RELATO.md:137` e `app/docs/PROMPTS.md:3070`.
- escritores:
  - Marcas do RELATO são atribuição da sessão pelo fluxo de `CLAUDE.md:334` e `CLAUDE.md:337`, com registros de `superado` em `app/docs/RELATO.md:13625` e `app/docs/RELATO.md:13644` e de `colhido` em `app/docs/RELATO.md:13645` e `app/docs/RELATO.md:13651`. A autoria de cada edição e outro emissor de marcas são [nao verificado]. Há escritores do documento para alarmes e topo de censo, tabela de cortes e deploys agendados, respectivamente `app/core/esteira_vigia.py:815`, `app/core/esteira_vigia.py:865`, `bin/disjuntor.py:329`, `bin/gerar_cortes.py:117`, `bin/gerar_cortes.py:136` e `bin/deploy_agendado.sh:77`. A fixture grava marcas e `.rc` em árvore temporária em `bin/tests/test_rc_sem_colher.sh:22`, `bin/tests/test_rc_sem_colher.sh:31` e `bin/tests/test_rc_sem_colher.sh:40`.
  - `bin/codex_pergunta.sh:446` confina a resposta sob `logs/`. O trap de saída publica `<resposta>.rc` por temporário e rename para o construtor com a trava, em `bin/codex_pergunta.sh:568`, `bin/codex_pergunta.sh:572`, `bin/codex_pergunta.sh:573`, `bin/codex_pergunta.sh:574` e `bin/codex_pergunta.sh:582`. O RELATO cita `lanca_c16.sh` e `F4e/lanca2.sh`, suite por `suite_c1g/roda.sh`, mutantes por `c1g/mut.sh`, RED em `c3/red.rc` e recibo de certificação em `pouso_1900/certifica.rc`. Os corpos dos lançadores citados e o inventário dos escritores avulsos são [nao verificado]. `bin/pista_certifica.sh:13` recebe o recibo do chamador. `bin/suite.sh:459` coleta o recibo e `bin/suite.sh:585` grava o rc interno em temporário. `app/docs/RELATO.md:13637`, `app/docs/RELATO.md:13638`, `app/docs/RELATO.md:13606`, `app/docs/RELATO.md:13612`, `app/docs/RELATO.md:13663`, `app/docs/RELATO.md:13635`.
- decisoes:
  - Preservar `sem_colher`, inclusive `--min-idade` e os códigos do CLI. A saída adicional cabe no mesmo juiz, com a mesma seleção de obra e rc. `ESPERA_UNICA` já acorda com rc `3` na abertura e entra no laço com rc `0`. Durante o laço, só procura publicação de `.rc` por `ctime`, até 300 s. O selo de forma precisa cobrir a chamada, além do `find` e do teto. `bin/rc_sem_colher.py:50`, `bin/rc_sem_colher.py:76`, `bin/rc_sem_colher.py:104`, `bin/rc_sem_colher.py:107`, `bin/disjuntor.py:1967`, `bin/disjuntor.py:1969`, `bin/disjuntor.py:2172`, `bin/tests/test_rc_sem_colher.sh:103`.
  - Tratar `colhido` e `superado` como marcas por rc. Hoje ambos tiram o rc de `sem_colher`. O BACKLOG tem outro predicado de fechamento, que o handoff e a lista de interrompidas reutilizam. A divergência já cabe nos registros da cópia. `VIGIA-DISCO` tem um rc `superado` e o BACKLOG ainda registra construção. `O30-LOTE-MINIMA` registra pouso e `NO AR` sem negrito no BACKLOG, enquanto a suite da c2 tem `colhido` e o `superado` da c1 pertence a outro rc. `bin/rc_sem_colher.py:26`, `bin/rc_sem_colher.py:80`, `bin/hook_stop_fila1.py:55`, `bin/hook_stop_fila1.py:164`, `bin/handoff_sessao.sh:181`, `bin/interrompidas_estado.py:125`, `app/docs/RELATO.md:13633`, `app/docs/BACKLOG.md:2150`, `app/docs/BACKLOG.md:2184`, `app/docs/RELATO.md:13674`, `app/docs/RELATO.md:13683`.
  - Manter a pergunta no juiz de rc. Os candidatos encontrados respondem a outras condições. O Stop lê item fechado, congelado ou em espera no BACKLOG. O B3 lista obras com merge retido por reprovação. O handoff mostra o próximo item da ordem viva e ramos `raia20-*` à frente do main. O ESTADO lista raias sem agente e obras interrompidas por bug. O alarme de sessão mede ociosidade da sessão. A esteira mede fatia pronta fora da fila de integração ou falta de atividade da fila. O placar de tickets lê uma parada declarada. Outro predicado com a mesma combinação de rc, marca e inatividade é [nao verificado], portanto ampliar o juiz existente não duplica os predicados encontrados. `bin/hook_stop_fila1.py:176`, `bin/disjuntor.py:965`, `bin/handoff_sessao.sh:237`, `bin/handoff_sessao.sh:320`, `bin/raias_estado.py:117`, `bin/interrompidas_estado.py:105`, `bin/alarme_sessao_ociosa.py:33`, `app/core/esteira_vigia.py:460`, `app/core/esteira_vigia.py:584`, `app/core/placar_tickets.py:114`.
  - ESMERIL-SEMANTICA. O juiz atual compara o `HH:MM` entre parênteses com o minuto local do `mtime`, sem data nem segundos na chave. O `HH:MM` do início só valida a forma. Refazer o mesmo caminho no mesmo minuto, ou em outro dia com o mesmo `HH:MM` e a marca ainda no RELATO, pode reaproveitar a marca. A varredura inclui exatamente 24 h, tolera outro arquivo até exatamente `rc + 5 s` e exige idade do rc estritamente maior que `--min-idade`. O selo usa `date` local para montar a marca, e a convenção documentada para a trilha é `America/Sao_Paulo`. A configuração efetiva dos processos e a paridade com o vercodex são [nao verificado]. `bin/rc_sem_colher.py:26`, `bin/rc_sem_colher.py:53`, `bin/rc_sem_colher.py:59`, `bin/rc_sem_colher.py:63`, `bin/rc_sem_colher.py:69`, `bin/rc_sem_colher.py:76`, `bin/rc_sem_colher.py:79`, `bin/tests/test_rc_sem_colher.sh:30`, `bin/tests/test_hora_da_secao_nao_e_futura.sh:6`.
  - No anexo, a maior hora de `colhido` é reconstruída a partir do dia local do rc, somando `86400` se a hora cair antes do piso calculado. Com `mtime` fracionário, esse piso conserva a fração e pode mandar uma marca do mesmo minuto para o dia seguinte. A idade usa `> 900 s` a partir da marca reconstruída. O anexo também exclui o rc se houver outro arquivo escrito mais de 5 s depois dele, mesmo que esse arquivo já tenha mais de 15 min. A existência dessas regras na cópia é [nao verificado]. O juiz atual descarta a hora da linha e compara a idade do próprio rc. `bin/rc_sem_colher.py:29`, `bin/rc_sem_colher.py:76`.

perguntas de lei: 2
- Os 15 min correm desde a marca `colhido`, com qualquer escrita posterior ao rc além de 5 s já valendo como etapa seguinte, como no anexo, ou desde o último sinal de atividade da obra, permitindo que ela volte à lista após nova inatividade (`bin/rc_sem_colher.py:76`, regra adicional no HEAD [nao verificado])?
- Para `RC SEM PROXIMA`, basta o `superado` do rc mais novo para decidir encerramento, como no anexo, ou o fechamento no BACKLOG também deve excluir a obra quando as fontes divergem (`CLAUDE.md:337`, `bin/hook_stop_fila1.py:164`, `app/docs/RELATO.md:13633`, `app/docs/BACKLOG.md:2150`)?

## 27/11 09:44 · sessao d5d9b0c69d43 · PAR-PELO-TURNO · construtor · origem=Code

**c1v8. Um teste tocado:** `test_3d_resumo_da_tela_inclui_o_periodo_que_o_motor_datou_na_vespera`. O teste agora usa as nove chaves do literal de `espelho_do_colab`, com os mesmos valores iniciais. A asserção original foi preservada. [Teste:100](/home/ronald/wt-codex-par-pelo-turno-010806/app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:100), [literal canônico:1236](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/services/espelho.py:1236) [nao verificado]

A busca nos outros três módulos da obra não encontrou montagem manual de resumo. [test_par_pelo_turno.py:1](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/tests/test_par_pelo_turno.py:1), [test_par_pelo_turno_bordas.py:1](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/tests/test_par_pelo_turno_bordas.py:1), [test_par_pelo_turno_c1v6.py:1](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/tests/test_par_pelo_turno_c1v6.py:1)

### Diff

```diff
--- a/app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py
+++ b/app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py
@@ -109,7 +109,10 @@
         periodo, = r.periodos + r.periodos_ft
         self.assertEqual(r.dia_da_jornada[id(periodo)], dia)
         self.assertGreater(periodo.horas_trabalhadas, 0)
-        resumo = {'turnos': 0, 'turnos_abertos': 0}
+        # Resumo inicial de espelho_do_colab em ponto/services/espelho.py:1236.
+        resumo = {'total_trabalhadas': 0.0, 'total_noturnas': 0.0, 'total_extras': 0.0,
+                  'total_intra_indenizada': 0.0, 'turnos': 0, 'turnos_abertos': 0, 'dias_inconsistentes': 0,
+                  'noturnas_relogio': 0.0, 'acrescimo_noturno': 0.0}
         montar_dias(self.colab, escala, r, autoridade.batidas, dia, amanha,
                     self.agora.date(), dia.year, dia, dia, resumo, agora=self.agora)
         self.assertAlmostEqual(resumo['total_trabalhadas'], periodo.horas_trabalhadas)
```

### RED e mutantes

No ensaio isolado em memória, `atual` passou. `base` e `sem-contador` reproduziram `KeyError: 'dias_inconsistentes'`. `total-adulterado` fez a asserção original falhar com `8.0 != 7.0`. [Soma:454](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/services/espelho.py:454), [contador:685](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/services/espelho.py:685), [asserção:118](/home/ronald/wt-codex-par-pelo-turno-010806/app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:118) [nao verificado]

```bash
ensaio() {
  PYTHONDONTWRITEBYTECODE=1 python3 -I -S - "$1" <<'PY'
import ast
import subprocess
import sys
from pathlib import Path
from types import SimpleNamespace
from unittest import TestCase

modo = sys.argv[1]
alvo = Path("relatorios/tests/test_par_pelo_turno_c1v6_pdf.py")
fonte = (subprocess.check_output(["git", "show", f"HEAD:app/{alvo}"], text=True)
         if modo == "base" else alvo.read_text())
teste = next(n for n in ast.walk(ast.parse(fonte)) [nao verificado]
             if isinstance(n, ast.FunctionDef) and n.name.startswith("test_3d_"))
literal = next(n.value for n in teste.body if isinstance(n, ast.Assign) [nao verificado]
               and ast.unparse(n.targets[0]) == "resumo")
resumo = ast.literal_eval(literal)
if modo == "sem-contador":
    del resumo["dias_inconsistentes"]
if modo == "total-adulterado":
    resumo["total_trabalhadas"] = 1.0
periodo = SimpleNamespace(horas_trabalhadas=7.0)
servico = Path("ponto/services/espelho.py")
linhas = servico.read_text().splitlines()
escopo = dict(resumo_mes=resumo, resumo=resumo, self=TestCase(), periodo=periodo,
              _k="total_trabalhadas", _v=periodo.horas_trabalhadas)
for linha in (454, 685):
    exec(compile("\n" * (linha - 1) + linhas[linha - 1].strip(), str(servico), "exec"), escopo)
exec(compile(ast.Module(body=teste.body[-1:], type_ignores=[]), str(alvo), "exec"), escopo) [nao verificado]
PY
}

ensaio atual
ensaio base
ensaio sem-contador
ensaio total-adulterado
```

### Recorte sugerido para o Code

Execução Django pendente no banco de ensaio. [não verificado], [configuração CI:6](/home/ronald/wt-codex-par-pelo-turno-010806/app/config/settings/ci.py:6)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
  ponto.tests.test_par_pelo_turno \
  ponto.tests.test_par_pelo_turno_bordas \
  ponto.tests.test_par_pelo_turno_c1v6 \
  relatorios.tests.test_par_pelo_turno_c1v6_pdf
```

## 27/11 09:32 · sessao eaf2b30bba07 · SONDA-ESPURIAS-RAMO2 · leitura · origem=Code

**1. Os três colaboradores faltaram pelo corte global `detalhes[:20]`.** A s3 conta toda diferença de registro no `TETO`, exige também diferença de minutos para gerar `COLAB` e só imprime as primeiras 20 linhas `COLAB` acumuladas entre as empresas. [logs/sombra/espurias_txt11_s3.py:333–349,406–408,435–436, anexo]

As 20 linhas exibidas são de 11 colaboradores da empresa 2. Nessa empresa, o total de `0025` é −78,0 min e as linhas exibidas somam −8,4 min, deixando −69,6 min. Em `0243`, o total é +693,0 min e as linhas exibidas somam +633,0 min, deixando +60,0 min. Na empresa 3, nenhuma linha `COLAB` saiu e os deltas são +113,4 min em `0025` e +120,0 min em `0243`. São quatro pares distintos com delta de minutos, exatamente os quatro que faltam para os 24 pares do `TETO`. Portanto, o filtro `ma != mb` não eliminou nenhum par nesse resultado. Faltaram dois colaboradores da empresa 2 e um da empresa 3. Os IDs dos três ficam **[nao verificado]**. [logs/espurias_txt11_s3.out:27–59, anexo]

**2. A0 existe como leitura do gravado pelo mesmo montador.** O botão de exportar recalcula o fechamento em memória e injeta esses objetos na classificação e na montagem. Para A0, a sonda passa `fechamentos=None`: `classificar_export` consulta `FechamentoMensal` gravado e `montar_linhas_export` consulta o mesmo gravado com `STATUS_NO_TXT`. O montador lê os campos do fechamento e as datas de falta, e `conteudo_do_txt` serializa as linhas. Esse caminho dispensa a chamada ao motor de fechamento. O `original` rotulado `A0` na s3 já chamava o motor. [app/folha/views.py:214–219,269–288] [app/folha/export.py:402–452,525–539,696–755,791–797] [logs/sombra/espurias_txt11_s3.py:171–177,400–410, anexo]

Na sonda, `reg` é o SHA‑1 dos bytes UTF‑8 dos registros 10 e seus filhos 11, com os CRLF do TXT e na ordem emitida para o par. `FIM colabs` conta todos os colaboradores das três empresas. No comparador, um lado ausente vale zero minutos e tem registro ausente. `no_teto` testa o par colaborador/rubrica, e `RAMO2 muda` conta colaboradores distintos do teto com alguma mudança em R. [espurias_ramo2_s2.py:198–242,343,377] [compara_ramo2_s2.py:9,53–61,89–108] [app/folha/export.py:525–539]

A1, B e R usam as mesmas portas de relavratura da s3, com escritas temporárias em savepoints revertidos. O próprio `--rodar` tem uma etapa anterior à sonda que lê a lavra de produção e a grava na sombra. A execução no container e os resultados de R ficam **[nao verificado]**. [logs/sombra/espurias_txt11_s3.py:152–168,310–325, anexo] [bin/sombra.sh:136–167,483–493]

```python
# espurias_ramo2_s2.py
import logging
import sys
from collections import defaultdict
from contextlib import redirect_stderr, redirect_stdout
from datetime import date, datetime, time, timedelta
from decimal import Decimal
from hashlib import sha1
from io import StringIO
from traceback import walk_tb
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from chamados import fcm_utils, push_utils [nao verificado]
from colaboradores.models import Colaborador, Empresa
from escala import utils as grade
from escala.models import CelulaDia
from escala.orfa_classe import classificar_orfas
from folha import export
from ponto import turnos
from ponto.management.commands.detectar_ausencias import Command as Emissor
from ponto.nucleo import recompute_turnos
from ponto.services import cartorio, fechamento

LADOS = "A0A1B"
MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
JANELA = (date(2026, 10, 21), date(2026, 11, 20))
REF_S1 = "app/escala/orfa_classe.py:53-78"


class FalhaSonda(RuntimeError):
    pass


class FalhaRegistrada(BaseException):
    pass


class HoraComId(str):
    pass


class InstanteComId(datetime):
    def strftime(self, formato):
        hora = super().strftime(formato)
        if formato == "%H:%M" and hasattr(self, "batida_id"):
            hora = HoraComId(hora)
            hora.batida_id = self.batida_id
        return hora


def log_sem_dados(_logger, registro):
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        falha = FalhaRegistrada(f"{registro.pathname}:{registro.lineno} {tipo}")
        if erro is not None:
            falha = falha.with_traceback(erro.__traceback__)
        raise falha from None


def alvos_da_s1(empresa, janela, agora):
    # logs/sombra/espurias_txt11_s3.py:79-149
    ini, fim = janela
    primeiro = timezone.make_aware(datetime.combine(ini - timedelta(days=1), time.min))
    ultimo = timezone.make_aware(datetime.combine(fim + timedelta(days=3), time.min))
    ultimo -= timedelta(microseconds=1)
    ref = fim + timedelta(days=1)
    dias = (ref - ini).days + 1
    alvos = {}

    localtime_real = timezone.localtime
    pessoas = Colaborador.objects.filter(empresa=empresa).order_by("pk")
    with (
        patch.object(grade, "Exception", (), create=True),
        patch.object(turnos, "Exception", (), create=True),
    ):
        for colab in pessoas.iterator(chunk_size=200):
            batidas = list(turnos.batidas_apuraveis(colab, primeiro, ultimo))
            if not batidas:
                continue

            kwargs = dict(dias=dias, ref=ref, agora=agora)
            linhas = grade.montar_grade_prevista_periodo(colab, batidas, **kwargs)["dias"]
            if len(linhas) != dias:
                raise FalhaSonda(f"Grade fora da janela colab_id={colab.pk} {REF_S1}")
            classes = [classificar_orfas(linha) for linha in linhas[1:]]
            if not any(classes):
                continue

            # Só transporta a identidade. Os datetime e HH:MM mantêm seus valores.
            # [app/escala/utils.py:314-327,699-705,1367-1385,1766-1782,1882-1891]
            por_objeto = {id(b.timestamp): b.pk for b in batidas}

            def localtime_com_id(value=None, timezone=None):
                instante = localtime_real(value, timezone)
                pk = por_objeto.get(id(value))
                if pk is None or sys._getframe(1).f_globals is not vars(grade):
                    return instante
                marcado = InstanteComId(
                    instante.year, instante.month, instante.day,
                    instante.hour, instante.minute, instante.second,
                    instante.microsecond, tzinfo=instante.tzinfo, fold=instante.fold,
                )
                marcado.batida_id = pk
                return marcado

            with patch.object(timezone, "localtime", localtime_com_id):
                marcadas = grade.montar_grade_prevista_periodo(
                    colab, batidas, **kwargs,
                )["dias"]

            ids = set()
            for linha, marcada, classificacao in zip(
                linhas[1:], marcadas[1:], classes, strict=True,
            ):
                horas = marcada.get("orfas") or []
                if horas != (linha.get("orfas") or []):
                    raise FalhaSonda(f"Instrumentacao mudou a grade colab_id={colab.pk} {REF_S1}")
                for hora in horas:
                    if hora in classificacao:
                        pk = getattr(hora, "batida_id", None)
                        if pk is None:
                            raise FalhaSonda(f"Sobra sem identidade colab_id={colab.pk} {REF_S1}")
                        ids.add(pk)
            alvos[colab.pk] = ids

    return alvos


def reler_celulas(empresa, janela, alvo_colabs, agora):
    ini, fim = turnos.janela_com_pad(*janela)
    hoje = timezone.localdate(agora)
    emissor = Emissor(stdout=StringIO(), stderr=StringIO())
    pessoas = Colaborador.objects.filter(
        empresa=empresa, pk__in=alvo_colabs,
    ).order_by("pk")
    for colab in pessoas:
        recompute_turnos(colab, ini, fim)
        celulas = list(CelulaDia.objects.filter(
            colaborador=colab, data__range=(ini, fim), data__lt=hoje,
        ).order_by("data"))
        if celulas:
            cartorio.julgar_colab(
                colab, celulas, hoje=hoje, agora=agora, apply_=True,
                emissor=emissor, out=cartorio.contadores(), forcar=True,
            )


def medir_txt(empresa, janela, *, recalcular=False):
    # None le o gravado nas duas portas. app/folha/export.py:720-731,791-797
    objetos = None
    if recalcular:
        # Preserva a guarda da s3 e deixa os demais erros subirem.
        # logs/sombra/espurias_txt11_s3.py:171-177
        with patch.object(fechamento, "Exception", fechamento._GradeSemCelula, create=True):
            objetos = list(fechamento.recalcular_fechamento_mes(
                MES, ANO, empresa_id=empresa.pk, somente_leitura=True,
                retornar_objeto=True, lavrar_dias_pagos=False,
            ).values())
    itens = export.classificar_export(empresa, MES, ANO, fechamentos=objetos)

    eventos = export.eventos_do_fechamento
    montar = export.montar_linha_10
    atual = None
    pais = []

    def observar_eventos(fech, *args, **kwargs):
        nonlocal atual
        atual = fech.colaborador
        return eventos(fech, *args, **kwargs)

    def observar_linha_10(matricula, comp, rubrica, tipo, valor, cod_sistema):
        linha = montar(matricula, comp, rubrica, tipo, valor, cod_sistema)
        mat = str(matricula).zfill(10)
        cod = str(rubrica).zfill(4)
        inicio = len(f"10{mat}{comp}{cod}11")
        fim = -len(str(cod_sistema).zfill(10))
        if atual is None or not (cod.isascii() and cod.isdigit()):
            raise FalhaSonda("Registro 10 sem autoria ou rubrica numerica")
        pais.append((atual, cod, tipo, inicio, fim, linha))
        return linha

    # Cada bloco do montador comeca chamando eventos para o seu fechamento.
    # app/folha/export.py:734-755
    with (
        patch.object(export, "eventos_do_fechamento", observar_eventos),
        patch.object(export, "montar_linha_10", observar_linha_10),
    ):
        linhas, _ = export.montar_linhas_export(
            empresa, MES, ANO, itens=itens, fechamentos=objetos,
        )

    grupos = defaultdict(list)
    pai_txt = None
    datas = None
    fichas = iter(pais)
    for linha in export.conteudo_do_txt(linhas).splitlines():
        parte = export.partes_da_linha(linha)
        if linha.startswith("10"):
            colab, cod, tipo, inicio, fim, linha_montada = next(fichas)
            if linha != linha_montada:
                raise FalhaSonda("Registro 10 fora da ordem de montagem")
            # Mantem o valor numerico mesmo quando o layout do pai e irregular.
            # logs/sombra/espurias_txt11_s3.py:183-191,207-216
            cru = parte["valor"] if parte is not None else int(linha[inicio:fim])
            pai_txt, datas = [linha], []
            grupos[colab.pk, cod].append((colab, tipo, cru, pai_txt, datas))
        elif pai_txt is not None and parte is not None and parte["reg"] == "11":
            pai_txt.append(linha)
            datas.append(parte["data"])
        else:
            raise FalhaSonda("Registro 11 invalido ou sem pai")
    if next(fichas, None) is not None:
        raise FalhaSonda("Montador chamou registro 10 que nao saiu no TXT")

    faltas = {}
    foto = {}
    for chave, grupo in sorted(grupos.items()):
        total = Decimal(0)
        linhas_rubrica = []
        for colab, tipo, cru, bloco, datas in grupo:
            linhas_rubrica.extend(bloco)
            if tipo != "dias":
                total += Decimal(cru) * Decimal("0.6")
            else:
                if not datas:
                    raise FalhaSonda(f"Rubrica em dias sem filhos colab_id={colab.pk}")
                if colab.pk not in faltas:
                    mapa = export.minutos_de_falta_por_dia(colab, *janela)
                    faltas[colab.pk] = {
                        dia.strftime("%Y%m%d"): Decimal(str(minutos))
                        for dia, minutos in mapa.items()
                    }
                total += sum(faltas[colab.pk][dia] for dia in sorted(set(datas)))
        registro = export.conteudo_do_txt(linhas_rubrica).encode("utf-8")
        foto[chave] = (total, sha1(registro).hexdigest())
    return foto


def sequencias():
    # bin/sombra_db.sh:45
    quote = connection.ops.quote_name
    with connection.cursor() as cursor:
        cursor.execute(
            """
            SELECT n.nspname, c.relname, s.seqincrement, s.seqmin, s.seqmax, s.seqcycle
            FROM pg_catalog.pg_sequence s
            JOIN pg_catalog.pg_class c ON c.oid = s.seqrelid
            JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
            WHERE n.nspname IN (%s, %s)
            ORDER BY n.nspname, c.relname
            """,
            ("juliani", "public"),
        )
        saida = {}
        for schema, nome, passo, minimo, maximo, ciclo in cursor.fetchall():
            tabela = f"{quote(schema)}.{quote(nome)}"
            cursor.execute(f"SELECT last_value, is_called FROM {tabela}")
            valor, chamado = cursor.fetchone()
            saida[tabela] = (valor, chamado, passo, minimo, maximo, ciclo)
    return saida


def reiniciar_no_savepoint(estado):
    with connection.cursor() as cursor:
        for nome, (valor, chamado, passo, minimo, maximo, ciclo) in estado.items():
            proximo = valor + passo if chamado else valor
            if ciclo and proximo > maximo:
                proximo = minimo
            if ciclo and proximo < minimo:
                proximo = maximo
            cursor.execute(f"ALTER SEQUENCE {nome} RESTART WITH {int(proximo)}")


def rodada(empresa, janela, alvos, pks, agora, seq_antes):
    # A1/B da s3 usam savepoints distintos. logs/sombra/espurias_txt11_s3.py:310-325
    with transaction.atomic():
        reiniciar_no_savepoint(seq_antes)
        if pks:
            marcas = turnos.batidas_apuraveis(None).filter(
                colaborador__empresa=empresa, pk__in=pks,
            )
            if marcas.update(retratada_em=agora) != len(pks) or marcas.exists():
                raise FalhaSonda(f"Retirada incompleta empresa={empresa.pk}")
        reler_celulas(empresa, janela, alvos, agora)
        foto = medir_txt(empresa, janela, recalcular=True)
        transaction.set_rollback(True)
    return foto


def fatos_de_sobra(empresa, alvos, pks):
    marcas = turnos.batidas_apuraveis(None).filter(
        colaborador__empresa=empresa, pk__in=pks,
    ).order_by("colaborador_id", "timestamp", "pk")
    encontrados = set()
    for pk, cid, timestamp, tipo in marcas.values_list(
        "pk", "colaborador_id", "timestamp", "tipo",
    ):
        if pk not in alvos.get(cid, ()) or tipo not in ("E", "S"):
            raise FalhaSonda(f"Batida invalida batida_id={pk} colab_id={cid}")
        encontrados.add(pk)
        local = timezone.localtime(timestamp)
        yield (
            f"SOBRA empresa={empresa.pk} colab_id={cid} data={local:%Y-%m-%d} "
            f"hora={local:%H:%M:%S} tipo={tipo} batida_id={pk}"
        )
    if encontrados != pks:
        raise FalhaSonda(f"Leitura de sobras incompleta empresa={empresa.pk}")


def medir():
    if LADOS not in ("A0A1B", "R"):
        raise FalhaSonda("LADOS invalido")
    if (
        not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict["HOST"] != "sombra_db"
        or connection.settings_dict["NAME"] != "sombra"
        or connection.schema_name != "juliani"
    ):
        raise FalhaSonda("Conexao fora da sombra")

    fatos = []
    with (
        patch.object(logging.Logger, "handle", log_sem_dados),
        redirect_stdout(StringIO()),
        redirect_stderr(StringIO()),
        transaction.atomic(durable=True),
    ):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ WRITE")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise FalhaSonda("Banco ou schema fora da sombra")

        empresas = list(Empresa.objects.filter(pk__in=EMPRESAS).order_by("pk"))
        if tuple(e.pk for e in empresas) != EMPRESAS:
            raise FalhaSonda("Escopo incompleto de empresas")
        n_colabs = Colaborador.objects.filter(empresa_id__in=EMPRESAS).count()
        seq_antes = sequencias()
        agora = timezone.now()
        with (
            patch.object(timezone, "now", new=lambda: agora),
            patch.object(push_utils, "_enviar_push_usuario", return_value={"ok": False}), [nao verificado]
            patch.object(fcm_utils, "push_fcm_colaborador", return_value=False), [nao verificado]
        ):
            for empresa in empresas:
                janela = export.janela_competencia(empresa, MES, ANO)
                if janela != JANELA:
                    raise FalhaSonda(f"Janela fora do aval empresa={empresa.pk}")
                alvos = alvos_da_s1(empresa, janela, agora)
                fotos = {}
                if LADOS == "A0A1B":
                    fotos["A0"] = medir_txt(empresa, janela)
                    fotos["A1"] = rodada(empresa, janela, alvos, set(), agora, seq_antes)
                    pks = {pk for ids in alvos.values() for pk in ids}
                    fatos.extend(fatos_de_sobra(empresa, alvos, pks))
                    fotos["B"] = rodada(empresa, janela, alvos, pks, agora, seq_antes)
                else:
                    fotos["R"] = rodada(empresa, janela, alvos, set(), agora, seq_antes)

                for lado, foto in fotos.items():
                    for (cid, cod), (minutos, reg) in foto.items():
                        fatos.append(
                            f"MIN lado={lado} empresa={empresa.pk} colab_id={cid} "
                            f"rubrica={cod} min={minutos:f} reg={reg}"
                        )
        if sequencias() != seq_antes:
            raise FalhaSonda("Estado das sequencias divergiu")
        transaction.set_rollback(True)

    ids = ",".join(map(str, EMPRESAS))
    fatos.append(f"FIM lados={LADOS} empresas={ids} colabs={n_colabs}")
    return fatos


try:
    for fato in medir():
        print(fato, flush=True)
except (Exception, FalhaRegistrada) as erro:
    # Pilha completa sem texto de excecao do aplicativo nem linhas de fonte.
    print("Traceback (most recent call last):", file=sys.stderr)
    for quadro, linha in walk_tb(erro.__traceback__):
        codigo = quadro.f_code
        print(f'  File "{codigo.co_filename}", line {linha}, in {codigo.co_name}', file=sys.stderr)
    print(type(erro).__name__, file=sys.stderr)
    if isinstance(erro, (FalhaSonda, FalhaRegistrada)):
        print(str(erro), file=sys.stderr)
    raise SystemExit(1) from None
```

```python
# compara_ramo2_s2.py
import re
import sys
from collections import defaultdict
from decimal import Decimal
from pathlib import Path

LADOS = ("A0", "A1", "B", "R")
AUSENTE = (Decimal(0), None)
MIN = re.compile(
    r"MIN lado=(A0|A1|B|R) empresa=([0-9]+) colab_id=([0-9]+) "
    r"rubrica=([0-9]+) min=(-?[0-9]+(?:\.[0-9]+)?) reg=([0-9a-f]{40})"
)
SOBRA = re.compile(
    r"SOBRA empresa=([0-9]+) colab_id=([0-9]+) data=([0-9]{4}-[0-9]{2}-[0-9]{2}) "
    r"hora=([0-9]{2}:[0-9]{2}:[0-9]{2}) tipo=[ES] batida_id=([0-9]+)"
)
FIM = re.compile(r"FIM lados=(A0A1B|R) empresas=([0-9]+(?:,[0-9]+)*) colabs=[0-9]+")


def ler_fatos(caminho):
    try:
        linhas = Path(caminho).read_text(encoding="utf-8").splitlines()
    except FileNotFoundError:
        return []
    return [linha for linha in linhas if linha.startswith(("MIN ", "SOBRA ", "FIM "))]


def carregar(fatos, lados_arquivo, dados, sobras):
    rodape = FIM.fullmatch(fatos[-1])
    if rodape is None or rodape[1] != "".join(lados_arquivo):
        raise ValueError("FIM invalido ou fora do lugar")
    empresas = set(map(int, rodape[2].split(",")))
    for linha in fatos[:-1]:
        medida = MIN.fullmatch(linha)
        if medida is not None:
            lado, empresa, cid, cod

[truncado em 20000 de 24297 caracteres]

_8617 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
