# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 13:21** · publicado em **31/10/2026 13:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 13:21 · sessao a3feea6f45d5 · - · - · origem=Ronald (tmux)

Mapeamento da cadeia tocada, sem execução de testes: resultados de runtime e de frota são [não verificado]. [nao verificado]

| Campo/entidade | Escritor/porta | Leitor na cadeia |
|---|---|---|
| `CelulaDia.trabalha` | Regeneração calcula o valor pelo vínculo e cria/escreve na porta; fora da vigência escreve `False`. Reversão restaura pelo DNA; geração em período cria e atualiza futuro/presente. — `app/ponto/portas/celula.py:231`, `:243`, `:247`, `:260`, `:532`, `:590`, `:593`, `:612` | Juiz lê diretamente; fábrica consulta também para distinguir limbo no gate de exportada; projetor da grade lê como fallback de tipo e previsto. — `app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:153`; `app/escala/services/leitor_celula.py:344`, `:353` |
| `ata['tipo_dia']` | Grade produz folga/trabalho e overlays alteram para folga/falta/ausência; cartório copia à ata; porta persiste a ata. — `app/escala/utils.py:727`, `:752`, `:1118`, `:1174`, `:1180`, `:1743`, `:1749`; `app/ponto/services/cartorio.py:567`; `app/ponto/portas/celula.py:730`, `:741`, `:768`, `:775` | Juiz; projetor da grade; `_marcos_faltantes` repete veto de tipo diferente de trabalho. — `app/chamados/juizes.py:408`; `app/escala/services/leitor_celula.py:344`; `app/chamados/services/declaracao_texto.py:90`, `:95` |
| `ata['sem_ancora']` | Fonte 1: `work is None`; fonte 2: `_sem_anc_i` no intermitente; fonte 3: `eh_dia_trabalho(...) is None` no não intermitente. Cartório copia o booleano e a mesma porta persiste a ata. — `app/escala/utils.py:729`, `:1514`, `:1701`, `:1752`; `app/ponto/services/cartorio.py:570`; `app/ponto/portas/celula.py:768` | Projetor copia para grade; folha usa como causa e fechamento conta dias incertos. Cluster e intervalo deixam de ler a chave; juiz só lê `trabalha`/`tipo_dia`. — `app/escala/services/leitor_celula.py:362`; `app/folha/causas.py:96`; `app/ponto/services/fechamento.py:1018`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; `app/chamados/juizes.py:407` |
| `PerguntaDisputa` | Fábrica chama núcleo de disputa e geração de perguntas; geração chama `PerguntaDisputa.abrir`; gate reaponta existente ou cria. — `app/chamados/services/fabrica_da_celula.py:124`; `app/chamados/services/disputa_emissao.py:598`, `:606`, `:618`, `:389`; `app/chamados/models.py:2408`, `:2451`, `:2453` | Fábrica lê identidade do fato e carimbo; estado consulta juiz `pergunta_viva`; resposta grava pela função central; materialização lê e pode carimbar paridade. — `app/chamados/services/fabrica_da_celula.py:103`; `app/chamados/models.py:2476`; `app/chamados/services/disputa_emissao.py:1212`, `:1266`; `app/chamados/services/materializacao.py:818`, `:843`, `:860`, `:870` |
| `ChamadoColaborador` | Núcleo de disputa chama `abrir` para chamado pai; pergunta recém-nascida órfã chama reconciliador; cluster abre pela mesma porta e fecha por `reconciliar`. Gate trava colaborador antes do dedup e cria com `get_or_create`. — `app/chamados/services/disputa_emissao.py:504`, `:404`; `app/ponto/services/cluster_espurio.py:83`, `:88`; `app/chamados/models.py:864`, `:865`, `:930` | Cluster lê vivos para fechar e todos estados para dedup; intervalo lê vivos e reconcilia legado. — `app/ponto/services/cluster_espurio.py:45`, `:58`; `app/ponto/management/commands/detectar_intervalo_ausente.py:123`, `:138` |

A operação de editar célula não consta entre as operações registradas da porta; o registro relaciona gerar/regenerar/reverter, lavrar e carimbar. O delta de produção dos quatro emissores troca decisões/leitores e acrescenta escrita de Pauta pela porta `escrever`, sem acrescentar escrita direta de célula/pergunta/chamado nesses emissores. — `app/ponto/portas/celula.py:66`; `app/chamados/services/fabrica_da_celula.py:80`, `:153`, `:185`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`.

`_plantio_preenche_furo_na_data` é leitor, com decisão utilizada tanto no preview quanto no ato de materializar: lê célula do dia, chama juiz, monta slots e marcas, e devolve o resultado do propositor; o preview usa na guarda de paridade e o ato usa antes de recusar ou plantar. — `app/chamados/services/materializacao.py:156`, `:160`, `:164`, `:167`, `:438`, `:818`.

Defeitos adicionais:

- **Bypass da Pauta no fio mudo:** o segundo universo de `disparar_perguntas_competencia` inclui chamados vivos com célula sem filtrar `trabalha=True`; calcula motivos, exclui pergunta já existente e chama `fabrica.perguntar` diretamente, sem passar pelo gate de competência/Pauta. O encadeamento de produção está configurado com `--comp-corrente --apply`. Portanto, sob a condição de competência corrente entregue + limbo com chamado vivo + fato ainda sem pergunta, a rota pode criar a pergunta que o julgamento desviou para Pauta; ocorrência real na frota [não verificado]. — `app/chamados/management/commands/disparar_perguntas_competencia.py:155`, `:167`, `:220`, `:276`, `:288`, `:295`; `app/config/crons.py:1464`; `app/chamados/services/fabrica_da_celula.py:153`.
- **O gate não cumpre o literal amplo “pergunta nova só em aberta”:** trabalha `True` exportado continua perguntando, pois apenas `None` consulta a entrega; há teste que exige esse comportamento. — `app/chamados/services/fabrica_da_celula.py:153`, `:158`; `app/chamados/tests/test_f1c4_3_emissores.py:207`.
- **Intervalo não é emissor de nascimento nesta cópia:** detector alterado alimenta reconciliação de chamados legados; o nascimento foi aposentado e o comando incrementa suprimidos e faz `continue`. Os testes dessa função provam detecção de furos, não criação de chamado. — `app/ponto/management/commands/detectar_intervalo_ausente.py:119`, `:123`, `:138`, `:149`, `:155`; `app/chamados/tests/test_f1c4_3_emissores.py:139`.
- **Sem conclusão sobre COL717:** o código genérico conserva/reabre o chamado encontrado pelo lookup, mas não há, no dado fornecido, estado concreto do primeiro chamado ou seus lookups para provar preservação nominal. — `app/chamados/models.py:865`, `:880`, `:883`; [não verificado].

## 31/10 13:20 · sessao 8ee4ed027e0c · - · - · origem=Ronald (tmux)

São 23 testes: 18 de emissores e 5 do desvio para Pauta; a análise abaixo prevê a morte dos mutantes por inspeção, sem execução [não verificado]. As asserções estão em `app/chamados/tests/test_f1c4_3_emissores.py:80-153` e `:199-239`.

Abreviações: `T` = `app/chamados/tests/test_f1c4_3_emissores.py`; `F` = `app/chamados/services/fabrica_da_celula.py`; `M` = `app/chamados/services/materializacao.py`; `C` = `app/ponto/services/cluster_espurio.py`; `I` = `app/ponto/management/commands/detectar_intervalo_ausente.py`; `J` = `app/chamados/juizes.py`.

| Teste — nome após `test_` | Mutação mínima que prevê vermelho | [nao verificado]
|---|---|
| `RED_fabrica_limbo_pergunta` | Restaurar `cel.trabalha is not True` no veto de `F:80`; desaparece a lista exigida em `T:80-81`. |
| `fabrica_folga_nao_pergunta` | Trocar o retorno do veto por `['orfao_14h']`, em `F:81`; viola `T:83-84`. **Remover apenas o juiz não mata este teste**, pois há outro veto em `app/chamados/services/declaracao_texto.py:95-96`. |
| `MORDE_fabrica_dia_de_trabalho_pergunta` | Veto incondicional em `F:80`, retornando `[]`; viola `T:86-87`. |
| `fabrica_ausencia_lavrada_com_trabalha_true_nao_pergunta` | Trocar o retorno do veto por `['orfao_14h']`, em `F:81`; viola `T:89-91`. Remover apenas a cláusula `tipo_dia` do juiz **sobrevive**, pelo segundo veto de `declaracao_texto.py:95-96`. |
| `RED_materializacao_limbo_furo_absolve_o_plantio` | Restaurar `cel.trabalha is not True` em `M:160`; devolve `False` em `M:169`, violando `T:100-101`. |
| `materializacao_folga_nao_absolve` | Remover a chamada ao juiz do veto de `M:160`, deixando `if not lamp`; a fixture alcança o plantio de `M:167-168`, contrariando `T:103-104`. |
| `MORDE_materializacao_dia_de_trabalho_absolve` | Veto incondicional em `M:160`; viola `T:106-107`. |
| `RED_materializacao_ausencia_lavrada_com_trabalha_true_nao_absolve` | Em `M:160`, substituir juiz por `cel.trabalha is False`; a ausência passa ao plantio e viola `T:109-110`. |
| `RED_cluster_limbo_conta` | Restaurar `cel.trabalha is not True` em `C:28`; viola `T:118-119`. |
| `cluster_folga_nao_conta` | Em `C:28`, usar só `cel.trabalha is False`; a fixture `None/folga` chega ao `append` de `C:35`, violando `T:121-122`. |
| `MORDE_cluster_dia_de_trabalho_conta` | Veto incondicional em `C:28`; viola `T:124-125`. |
| `RED_cluster_ausencia_lavrada_com_trabalha_true_nao_conta` | Em `C:28`, usar só `cel.trabalha is False`; viola `T:127-128`. |
| `cluster_sem_ancora_com_trabalha_true_conta` | Acrescentar `or ata.get('sem_ancora')` em `C:28`; viola `T:130-131`. |
| `RED_intervalo_limbo_conta` | Restaurar `cel.trabalha is not True` em `I:51`; viola `T:140-141`. |
| `intervalo_folga_nao_conta` | Em `I:51`, usar só `cel.trabalha is False`; viola `T:143-144`. |
| `MORDE_intervalo_dia_de_trabalho_conta` | Veto incondicional em `I:51`; viola `T:146-147`. |
| `RED_intervalo_ausencia_lavrada_com_trabalha_true_nao_conta` | Em `I:51`, usar só `cel.trabalha is False`; viola `T:149-150`. |
| `intervalo_sem_ancora_com_trabalha_true_conta` | Acrescentar `or ata.get('sem_ancora')` em `I:51`; viola `T:152-153`. |
| `RED_limbo_em_exportada_nao_pergunta_e_vai_a_pauta_dp` | Desativar o bloco de `F:153`, deixando seguir para `perguntar` em `F:160`; viola retorno/perguntas/Pauta de `T:199-205`. |
| `limbo_em_aberta_pergunta_sem_pauta` | Negar o resultado de `competencia_do_dia_entregue` em `F:155`; aberta vai para Pauta e retorna `[]`, violando `T:207-210`. |
| `MORDE_trabalho_em_exportada_segue_perguntando_sem_pauta` | Retirar `and cel.trabalha is None` de `F:153`; passa a desviar também `True`, violando `T:212-216`. |
| `RED_limbo_em_exportada_duas_vezes_uma_pauta` | Remover `ja.update(...)` de `F:182`; segunda passagem escreve outra Pauta, violando `T:218-225`. |
| `RED_limbo_em_exportada_pauta_feita_nao_reabre` | Acrescentar `feito_em__isnull=True` ao filtro de `F:180`; a feita some da dedup e nasce outra, violando `T:227-239`. |

As mutações dos dois negativos da fábrica que trocam `[]` por lista mostram apenas que os testes observam o resultado; uma mutação relevante de arquitetura — retirar o juiz — sobrevive porque `_marcos_faltantes` também cala `tipo_dia != 'trabalho'`. São necessários dois desvios para liberar esse caso pelo fluxo normal: retirar o veto em `F:80` e o veto em `app/chamados/services/declaracao_texto.py:95-96`; o encadeamento ocorre em `F:49` e `app/chamados/services/disputa_emissao.py:826-827`.

Não há indicação de fixture vazia nos positivos: a fábrica e o plantio recebem entrada acesa e saída apagada; cluster recebe duas órfãs a dois minutos de distância; intervalo recebe entrada/saída final acesas e duas pausas apagadas (`T:45-49`, `:77`, `:96`, `:115`, `:136-137`). Os positivos exigem lista, `True`, dia ou marcos concretos, e os testes aberta/trabalho exportada contam efetivamente `PerguntaDisputa` (`T:81`, `:101`, `:119`, `:141`, `:209`, `:215`). [nao verificado]

Há ramos e contratos sem prova:

- Os testes dos quatro emissores chamam helpers; só a segunda classe chama `perguntar_pela_celula`. Não demonstram o ato de materializar, abrir chamado de cluster ou executar o command de intervalo (`T:75-78`, `:94-98`, `:113-116`, `:134-138`, `:184-187`; atos de cluster em `C:87-105` e command em `I:102-119`). [nao verificado]
- A fixture escreve `CelulaDia` por `create`/`update`, sem chamar a lavra; portanto o teste não prova o formato produzido pelo cartório nem a interação com a ata em curso (`T:65-72`; projeção do julgamento em `J:392-395`). [nao verificado]
- Nenhum caso fornece `trabalha=False`, `tipo_dia` ausente/vazio ou pergunta anterior carimbada; os valores construídos e casos são os de `T:65-153`, e o ramo de desfazer carimbo fica em `F:145-148`.
- Nenhum teste cria Pauta cancelada, resposta filha ou contexto longo; os únicos históricos exercitados são viva repetida e feita (`T:218-239`). A consulta nova exige cabeça `pai__isnull=True` em `F:180`. [nao verificado]
- O caso feita chama a porta, mas não verifica `feito_em`; tornar `marcar_feita_pelo_sistema` um no-op mantém seus asserts e o cenário fica equivalente ao teste de viva. Atualmente a porta grava `feito_em` em `app/pautas/services.py:261-266`; o teste só confere retorno, quantidade e perguntas (`T:233-239`).
- O caso de exportada verifica apenas que existe algum `[motivo:`, sem exigir o motivo exato nem o dia no texto; a busca só usa âncora/DP/prefixo (`T:189-205`).
- O teste `MORDE_trabalho_em_exportada...` exige uma pergunta nova em exportada, apesar do aval literal do usuário “Pergunta NOVA ... só em competência ABERTA” [não verificado quanto ao aval em arquivo]; a exigência está em `T:212-216` e a produção a implementa em `F:153-160`. [nao verificado]

Outro defeito concreto por construção: mover as tags ao início não garante idempotência contra truncamento. `escrever` reserva `len(_ctx)` sem teto e corta o corpo em `500-len(_ctx)-4`; o contexto inclui nome e todas as batidas, sem limite nessa montagem (`app/pautas/services.py:168-177`, `:413`, `:419-420`). Se o espaço restante cortar o marcador ou uma tag, o próximo julgamento não encontra `startswith` ou `[motivo:...]` e escreve outra Pauta (`F:180-190`). Ocorrência desse caso em produção: [não verificado].

## 31/10 13:20 · sessao 6818fb16157f · - · - · origem=Ronald (tmux)

Achados para consolidação (referências do snapshot atual):

- **Disparo sem persistência pode desaparecer na troca de sessão.** `_com_estado` mantém o retorno da decisão quando `os.replace` falha; não reconstitui o estado pela trilha. A próxima sessão só varre seus próprios subagentes. Portanto: Stop dispara → gravação falha → sessão muda → novo final da obra passa, sem linha nova. (`bin/disjuntor.py:263`, `bin/disjuntor.py:269`, `bin/disjuntor.py:319`, `bin/disjuntor.py:670`, `bin/disjuntor.py:1096`)
- **Disparo persistido também pode ser contornado por exceção.** Um transcript de verificador ilegível, inclusive de outra obra, aborta `varrer_b3` antes da consulta decisória ao estado; `main_pretool` libera a ferramenta. Trava ocupada e estado ilegível chegam ao mesmo caminho. É comportamento herdado do mecanismo de falha aberta, mas impede a garantia absoluta do aval. (`bin/disjuntor.py:326`, `bin/disjuntor.py:534`, `bin/disjuntor.py:673`, `bin/disjuntor.py:247`, `bin/disjuntor.py:254`, `bin/disjuntor.py:1132`)
- **Fotografia falha permite promover linha antiga.** `_topo_shas` troca leitura falha por `[]`; edição posterior de qualquer parte do RELATO atualiza o mtime. A linha antiga, ausente da fotografia, passa nas duas verificações sem ter sido escrita depois do disparo. (`bin/disjuntor.py:638`, `bin/disjuntor.py:643`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`)
- **Readback não fecha a corrida com redisparo.** A confere consumo persistido e `disparado=False`; B dispara novamente depois dessa conferência; A escreve LIBERA e retorna sucesso. O novo final passa com a obra novamente disparada e sem linha posterior ao segundo disparo. Confirmei a transição em memória, injetando o segundo disparo durante a chamada de trilha LIBERA. (`bin/disjuntor.py:622`, `bin/disjuntor.py:624`, `bin/disjuntor.py:631`, `bin/disjuntor.py:634`, `bin/disjuntor.py:699`, `bin/disjuntor.py:1096`)
- **Concorrência também causa negativa indevida.** A recebe obra disparada; B consome a linha e persiste a liberação; A entra em `libera_b3`, encontra `disparado=False`, recebe `None` e nega mesmo assim. (`bin/disjuntor.py:604`, `bin/disjuntor.py:612`, `bin/disjuntor.py:1096`, `bin/hook_stop_fila1.py:1045`)
- **Stop decide com candidato anterior à fotografia.** O Stop lê topo+mtime antes de varrer; `varrer_b3` fotografa novamente sob a trava. Escrita/remoção entre as leituras permite que `libera_b3` receba texto obsoleto, distinto do texto fotografado. O mtime foi lido separadamente e antes do conteúdo. (`bin/hook_stop_fila1.py:1044`, `bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:700`, `bin/disjuntor.py:738`, `bin/disjuntor.py:741`)
- **Linha válida posterior pode não liberar por identidade textual.** Reescrever depois do disparo a mesma linha já consumida continua proibido; o hash representa conteúdo, não ocorrência de escrita. O selo exige expressamente essa recusa. (`bin/disjuntor.py:555`, `bin/disjuntor.py:607`, `bin/tests/test_disjuntor.sh:376`, `bin/tests/test_disjuntor.sh:379`)
- **Migração muda a referência temporal de disparo antigo.** Estado antigo já disparado recebe `disparado_em=agora` e fotografia atual; uma linha válida escrita depois do disparo original, mas antes da migração, não libera. Confirmei esse caminho em memória. (`bin/disjuntor.py:655`, `bin/disjuntor.py:657`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`)

Todos os campos são persistidos pela porta `_com_estado`, com flock e substituição do JSON; a leitura genérica é `ler_estado`. (`bin/disjuntor.py:229`, `bin/disjuntor.py:242`, `bin/disjuntor.py:263`, `bin/disjuntor.py:275`)

| Campo | Escritores | Leitores |
|---|---|---|
| `reprovados` | inicializa/append: `bin/disjuntor.py:690`, `:694`; limpa: `:612`, `:662`, `:719` | `bin/disjuntor.py:611`, `:658`, `:661`, `:693`, `:697`, `:717`, `:718` |
| `rearmado` | migração `bin/disjuntor.py:661`; rearme `:718` | `bin/disjuntor.py:661`, `:691`, `:718` |
| `liberado` | consumo `bin/disjuntor.py:611` | `bin/disjuntor.py:611`, `:691` |
| `disparado` | inicialização `bin/disjuntor.py:690`; dispara `:699`; limpa `:612`, `:719` | `bin/disjuntor.py:604`, `:624`, `:655`, `:698`, `:705`, `:745` |
| `disparado_em` | `bin/disjuntor.py:657`, `:699` | `bin/disjuntor.py:609`, `:656` |
| `consumidas` | `bin/disjuntor.py:613` | `bin/disjuntor.py:607`, `:613`, `:623` |
| `topo_no_disparo` | `bin/disjuntor.py:657`, `:700` | `bin/disjuntor.py:607` |
| `b3_limiar` | `bin/disjuntor.py:663` | `bin/disjuntor.py:651`, `:660` |

| Trilha | Escritor | Leitor de produção |
|---|---|---|
| `DISJUNTOR B3` | varredura `bin/disjuntor.py:701` → `dispara`, `:133` → `trilha`, `:120` | `ler`, `bin/disjuntor.py:1180`, `:1202`; apresentação `:1295` |
| `LIBERA` | `bin/disjuntor.py:634` | `bin/disjuntor.py:1180`, `:1198`; devolve `b3_libera`, `:1238`; apresenta `:1308` |
| `MIGRA` | `bin/disjuntor.py:659` | Não entra na lista lida por `ler`; o selo procura a linha. (`bin/disjuntor.py:1180`, `bin/tests/test_disjuntor.sh:330`) |
| `FALHA` | RELATO `bin/disjuntor.py:224`; persistência `:269`; consumo `:626`, `:629`, `:632`; B4 `:1110`; PreToolUse `:1133`; Stop `bin/hook_stop_fila1.py:1056` | `ler` lê todas, mas somente `relato_negacao` incrementa o painel; as demais não reconstroem estado. (`bin/disjuntor.py:1180`, `:1196`, `:1197`) |

## 31/10 13:20 · sessao 232c2fc7e8f8 · - · - · origem=Ronald (tmux)

Conclusões da integração, sem modificar arquivos:

- **Corrida que aceita linha anterior ao disparo real.** Stop captura `agora`, depois captura topo/mtime, só então dispara; a fotografia usada no disparo pode conter outro topo. Uma candidata capturada pelo Stop, retirada antes da fotografia, passa porque seu hash não está em `topo_no_disparo` e seu mtime supera o relógio capturado antes. Fontes: `bin/hook_stop_fila1.py:1038`, `bin/hook_stop_fila1.py:1044`, `bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`, `bin/disjuntor.py:699`, `bin/disjuntor.py:700`. Sonda em memória com funções reais confirmou `disparado=False` e consumo da candidata; não há artefato gravado. [não verificado — registro persistido da execução]

- **Corrida que nega apesar da linha válida.** PreToolUse varre e observa disparo; Stop consome a linha entre essa varredura e `libera_b3` do PreToolUse; esse `libera_b3` encontra `disparado=False`, retorna `None` e o chamador nega a ferramenta. Fontes: `bin/disjuntor.py:1096`, `bin/disjuntor.py:604`, `bin/disjuntor.py:605`, `bin/disjuntor.py:1097`, `bin/hook_stop_fila1.py:1046`.

- **Outra falsa negativa temporal.** `_topo_e_mtime` lê mtime antes do conteúdo; escrita válida concorrente pode produzir conteúdo novo associado ao mtime antigo, que `libera_b3` rejeita. Fontes: `bin/disjuntor.py:738`, `bin/disjuntor.py:741`, `bin/disjuntor.py:609`.

- **Trava parcial.** Cada leitura/mutação/persistência de estado usa flock, mas `varrer_b3 → libera_b3 → resposta` não forma operação única. A conferência de consumo ocorre fora da trava, mediante `ler_estado`. Fontes: `bin/disjuntor.py:229`, `bin/disjuntor.py:242`, `bin/disjuntor.py:257`, `bin/disjuntor.py:272`, `bin/disjuntor.py:616`, `bin/disjuntor.py:622`, `bin/disjuntor.py:1096`.

Checklist:

| Classe | Conclusão |
|---|---|
| Segundo juiz registrado | Não demonstrado: trata-se de instrumento host; o reconhecedor duplicado anterior foi removido, com lápide. `bin/disjuntor.py:4`; `bin/hook_stop_fila1.py:411`. |
| Segundo escritor fora da porta | Não demonstrado: mutações B3 passam por `_com_estado`; linha automática no RELATO usa `no_relato`. `bin/disjuntor.py:616`; `bin/disjuntor.py:708`; `bin/disjuntor.py:721`; `bin/disjuntor.py:221`. |
| Relógio fora da borda | Callers de produção passam `agora`; auxiliares mantêm fallback `time.time()`. A corrida principal é instante capturado antes da fotografia. `bin/hook_stop_fila1.py:1038`; `bin/hook_stop_fila1.py:1045`; `bin/disjuntor.py:1096`; `bin/disjuntor.py:653`; `bin/disjuntor.py:669`. |
| Derivação paralela | Reconhecedor e leitura de topo foram centralizados; não encontrei duplicação nessa integração. `bin/disjuntor.py:560`; `bin/disjuntor.py:598`; `bin/disjuntor.py:733`; `bin/hook_stop_fila1.py:1044`; `bin/hook_stop_fila1.py:1046`. |
| Ordem de trava | Cai nas corridas acima; chamadas separadas não ficam serializadas como decisão completa. `bin/disjuntor.py:616`; `bin/disjuntor.py:622`; `bin/disjuntor.py:1096`. |
| Pouso antes de pré-requisito | CLAUDE exige regra/ponteiro e tem uma linha; o RELATO distingue construção de pouso depois do produto. O commit isolado não prova deploy/pouso da obra. `CLAUDE.md:502`; `app/docs/RELATO.md:61`; `app/docs/BACKLOG.md:779`. |

A frase “para a obra / antes de nova volta” é mais ampla que a trava implementada: ferramentas comuns, construtor e chamadas Codex seguem fora do B3; só novo `Agent` verificador reconhecido como final recebe a trava. Entretanto, isso já está explicitamente limitado no contrato técnico do RELATO. Fontes: `bin/disjuntor.py:1075`, `bin/disjuntor.py:1084`, `bin/disjuntor.py:1093`, `bin/disjuntor.py:1096`, `app/docs/RELATO.md:11`, `app/docs/RELATO.md:13`.

O Stop pode sair antes de varrer B3 por orçamento, PAREI, espera comprovada, fila vazia ou teto; isso é limitação expressamente documentada, e PreToolUse varre novamente antes da decisão. Fontes: `bin/hook_stop_fila1.py:1213`, `bin/hook_stop_fila1.py:1221`, `bin/hook_stop_fila1.py:1227`, `bin/hook_stop_fila1.py:1241`, `bin/hook_stop_fila1.py:1288`, `bin/hook_stop_fila1.py:1295`, `app/docs/RELATO.md:13`, `bin/disjuntor.py:1096`.

## 31/10 13:19 · sessao 4a3e8aa54332 · - · - · origem=Ronald (tmux)

O `_C18` tem uma autoridade registrada: `chamados/juizes.py::dia_sem_trabalho`; `core/juizes.py` declara a pergunta e o endereço, sem outra implementação concorrente nessa pergunta. O juiz de vínculo responde previsão, e o de status responde status lavrado, perguntas distintas. (`app/core/juizes.py:142`, `app/core/juizes.py:266`, `app/core/juizes.py:165`, `app/core/juizes.py:180`)

O predicado real é `trabalha is False or bool(tipo_dia and tipo_dia != 'trabalho')`: ausência de `tipo_dia` não cala, `None` não cala e `sem_ancora` não participa. `_celula_furada` chama esse juiz, sem duplicar o predicado. (`app/chamados/juizes.py:407`, `app/chamados/juizes.py:408`, `app/chamados/juizes.py:443`)

Há respostas próprias de produção que permanecem:

| Sítio | Resposta própria / diferença |
|---|---|
| `_marcos_faltantes` | `tipo_dia != 'trabalho'` elimina marcos de cobrança; não incorpora `trabalha=False` com ata contraditória e elimina tipo ausente/indefinido. (`app/chamados/services/declaracao_texto.py:90`, `app/chamados/services/declaracao_texto.py:95`) |
| `classificar_dia` | Declara `COBRANCA_EM_DIA_SEM_TRABALHO` usando tipo diferente de trabalho, fora de `SEM_JURISDICAO`, previsto zero, chamados vivos e ausência de cobertura; predicado diferente e mais restrito que C18. (`app/ponto/supra_juiz.py:443`, `app/ponto/supra_juiz.py:445`) |
| `celula_veta_cobranca` | `trabalha is False` vira `FOLGA`; ignora cláusula da ata, além de vetos próprios de veredito. (`app/ponto/marcos.py:155`, `app/ponto/marcos.py:159`, `app/ponto/marcos.py:161`) |
| Emissão de ausências | Usa o juiz paralelo `celula_veta_cobranca`. (`app/ponto/management/commands/detectar_ausencias.py:625`) |
| Emissão de reconciliação da grade | Usa o mesmo juiz paralelo. (`app/ponto/management/commands/reconciliar_grade.py:502`) |
| Emissão/reabertura c2 | Importa o mesmo juiz paralelo. (`app/chamados/reconciliador.py:768`) |
| Leitor de furos sem cobrança | Usa esse veto para rotular a porta. (`app/ponto/services/furos_sem_cobranca.py:98`) |
| Lote de perguntas, primeiro universo | Seleciona exclusivamente `trabalha=True`, eliminando limbo antes de qualquer chamada ao juiz. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`) |
| Vigia de hora | Seleciona exclusivamente `trabalha=True` antes de chamar o cartório; limbo fica fora desse caminho de emissão. (`app/ponto/services/vigia_de_hora.py:21`, `app/ponto/services/vigia_de_hora.py:130`) |

Também existem testemunhas que apresentam o limbo como não cobrável: worklist elimina todo tipo diferente de trabalho; relatório de furos elimina `sem_ancora`; frase para admin/IA usa `not cel.trabalha`; relatório diário conta `None` como folga. São leitores/relatórios, não portas diretas de nascimento de chamado. (`app/ponto/worklist.py:19`, `app/relatorios/furos.py:151`, `app/relatorios/furos.py:157`, `app/relatorios/furos.py:218`, `app/relatorios/furos.py:237`, `app/ponto/services/dia_do_colab.py:121`, `app/relatorios/dia_da_frota.py:47`)

Dois defeitos importantes para a consolidação:

1. O gate novo só protege `trabalha=None`; `True` em exportada chega a `perguntar`, contrariando o literal atual “pergunta NOVA só competência ABERTA”. O comentário da obra mantém a exceção antiga O44 como se prevalecesse. (`app/chamados/services/fabrica_da_celula.py:151`, `app/chamados/services/fabrica_da_celula.py:153`, `app/chamados/services/fabrica_da_celula.py:158`)
2. O lote chama `fabrica.perguntar` diretamente, pulando o gate novo de `perguntar_pela_celula`. Seu universo de fio mudo consulta chamados com célula sem restringir `trabalha`, busca motivo pela ata e aceita fato sem pergunta existente; portanto há caminho para pergunta nova de limbo exportado sem Pauta. (`app/chamados/management/commands/disparar_perguntas_competencia.py:155`, `app/chamados/management/commands/disparar_perguntas_competencia.py:167`, `app/chamados/management/commands/disparar_perguntas_competencia.py:220`, `app/chamados/management/commands/disparar_perguntas_competencia.py:226`, `app/chamados/management/commands/disparar_perguntas_competencia.py:277`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`)

O registro C18 não ganhou padrão específico em `PROIBIDOS['chamado']`, que cobre apenas SLA e dia cru do chamado; registrar o endereço não impede essas derivações. (`app/core/juizes.py:1368`, `app/core/juizes.py:1375`)

## 31/10 13:19 · sessao 54380a25fb11 · - · - · origem=Ronald (tmux)

Achados do selo e documentação:

- **O mutante limiar=3 fica vermelho antes de medir a primeira reprovação.** O caso `b3` exige literalmente `MIGRA ... limiar 3 -> 1` em `bin/tests/test_disjuntor.sh:330`; a constante mutada produz `-> 3` por `bin/disjuntor.py:659`. Assim, o mutante declarado em `bin/tests/test_disjuntor.sh:845` pode morrer nessa comparação antes de chegar à prova da primeira reprovação em `bin/tests/test_disjuntor.sh:339`. Há mordida textual, sem isolamento causal do limiar.

- **Mutante relevante que sobreviveria aos casos B3, por inspeção:** acrescentar `and lei != 'escritor unico'` ao predicado em `bin/disjuntor.py:576`. As aceitações de `b3_topo` usam juiz único, célula soberana e lâmpada em `bin/tests/test_disjuntor.sh:675`, `bin/tests/test_disjuntor.sh:694`, `bin/tests/test_disjuntor.sh:695`. A única linha de escritor único do caso integrado está numa sequência que espera continuar bloqueada, em `bin/tests/test_disjuntor.sh:385` e `bin/tests/test_disjuntor.sh:387`. Não executei a suíte porque cria arquivos — `bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:51`.

- **Outro sobrevivente, por inspeção:** trocar `>= B3_BOBO_MIN` por `> B3_BOBO_MIN` em `bin/disjuntor.py:581`: não há motivo com exatamente 20 caracteres nos casos explícitos; eles cobrem `typo` e motivo longo em `bin/tests/test_disjuntor.sh:696` e `bin/tests/test_disjuntor.sh:697`.

- **Divergência literal do aval:** mínimo de 20 caracteres é requisito acrescentado em `bin/disjuntor.py:75`, aplicado em `bin/disjuntor.py:581`; o texto fornecido pelo usuário pede somente uma linha de motivo [não verificado no arquivo]. Execução isolada do parser em AST, só em memória, confirmou que `ERRO-BOBO OBRA-X: faltou import` é recusado; o comportamento vem de `bin/disjuntor.py:581`. O contrato local já explicita essa restrição em `app/docs/RELATO.md:12`; portanto distinguir divergência contra aval literal de implementação contra contrato local.

- **A forma aceita não demonstra verdade semântica:** `DESENHO ... pergunta=x app/inexistente.py:0` passa pelo parser; regex aceita zero em `bin/disjuntor.py:76`, e a validação só exige uma palavra adicional em `bin/disjuntor.py:576` e `bin/disjuntor.py:577`. Não verifica arquivo existente, linha existente, lei efetivamente quebrada ou ausência de juiz.

- **CLAUDE.md atende ao recorte pedido:** uma linha com nome e ponteiros em `CLAUDE.md:502`; a referência preserva o nome em `bin/tests/claude_md_regras.ref:93`. O diff real avô→atual acrescenta apenas essa linha no CLAUDE e o nome no `.ref` — posições atuais citadas acima.

- **O próprio contrato reconhece limites relevantes:** falha aberto, saídas do Stop antes de varrer B3, e PreToolUse como trava principal estão expressos em `app/docs/RELATO.md:13`; não apresentar essas propriedades como se fossem garantias universais de bloqueio.

## 31/10 13:08 · sessao 0b3b9a70c232 · GRADE-FOLGA · verificador · origem=Code

**Há um bloqueador: o calendário adaptou o formato, mas continua fornecendo um universo de folgas incompatível com a fonte compartilhada.** Isso preserva divergências entre calendário, grade e precedência (`app/colaboradores/services/calendario.py:593`, `app/colaboradores/services/calendario.py:607`; `app/ponto/precedencia.py:207`).

1. **Quem escreve e quem lê o novo formato**

   O carregador produz listas com todas as `FolgaDia` da pessoa, ordenadas por pk; o calendário produz tuplas `(True,)` somente com datas do vínculo ativo (`app/ponto/precedencia.py:207`; `app/colaboradores/services/calendario.py:593`, `app/colaboradores/services/calendario.py:607`).

   | API/formato | Todos os chamadores diretos encontrados em produção |
   |---|---|
   | `folgas_lancadas_do_periodo` | `app/ponto/precedencia.py:282`; `app/escala/utils.py:1119` |
   | `folga_lancada_do_dia` | `app/ponto/precedencia.py:358`; `app/escala/utils.py:1124` |
   | `folga_lancada_vale_no_dia` | `app/ponto/precedencia.py:218` |
   | Argumento explícito `folgas_lancadas=` | `app/colaboradores/services/calendario.py:607` |

   Não encontrei chamadas diretas dessas APIs nem outro fornecimento explícito do parâmetro em testes [nao verificado]. Os testes novos exercitam os helpers indiretamente pela grade e por `fatos_do_dia` (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:57`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:62`).

   **Os leitores das listas estão corretos:** ambos passam as candidatas ao seletor; este percorre até encontrar uma válida. O `bool(folga_lancada)` da saída ocorre depois da seleção (`app/ponto/precedencia.py:217`, `app/ponto/precedencia.py:358`, `app/ponto/precedencia.py:408`; `app/escala/utils.py:1124`).

   **O erro é a alimentação alternativa do calendário:**

   - `folgas_do_periodo` conserva vínculo e data, mas descarta `calendario_origem_id`; o calendário carrega apenas o vínculo ativo (`app/escala/alimentacao.py:66`; `app/colaboradores/services/calendario.py:593`).
   - Quando `_cobre`, fornece `(True,)` e impede a carga completa da pessoa; `True` é aceito como folga manual incondicionalmente (`app/colaboradores/services/calendario.py:602`, `app/colaboradores/services/calendario.py:607`; `app/ponto/precedencia.py:281`, `app/ponto/precedencia.py:238`).
   - **Falso positivo:** o ativo cobre a janela, mas a célula declara outro vínculo como geradora. Uma folga de calendário do ativo passa como manual, embora devesse ser rejeitada contra o vínculo escolhido pela juíza (`app/ponto/precedencia.py:327`, `app/ponto/precedencia.py:351`, `app/ponto/precedencia.py:240`; `app/colaboradores/services/calendario.py:607`).
   - **Falso negativo:** uma folga manual presa ao vínculo antigo desaparece dessa alimentação, embora o carregador completo a encontre e o predicado a aceite (`app/colaboradores/services/calendario.py:593`; `app/ponto/precedencia.py:207`, `app/ponto/precedencia.py:238`).

   Isso muda a apresentação: `_fatos` alimenta `folga_humana`, que força `trabalho=False` e pode devolver status `folga` (`app/colaboradores/services/calendario.py:792`, `app/colaboradores/services/calendario.py:801`, `app/colaboradores/services/calendario.py:116`, `app/colaboradores/services/calendario.py:134`). A correção deve fornecer as candidatas reais pelo carregador compartilhado, ou deixar `fatos_do_periodo` carregá-las (`app/ponto/precedencia.py:281`).

2. **Duas ou mais FolgaDia: a seleção foi corrigida nos dois builders**

   Não há leitor que selecione simplesmente a primeira candidata bruta nesses caminhos: grade e precedência usam o mesmo seletor, que testa cada candidata até a primeira válida (`app/escala/utils.py:1124`; `app/ponto/precedencia.py:217`, `app/ponto/precedencia.py:358`).

   No builder por turno, o vínculo passado vem do mapa calculado pela juíza; no legacy, o vínculo do segmento também vem dela, e o recorte elimina o dia extra `d0-1` (`app/escala/utils.py:1262`, `app/escala/utils.py:1268`, `app/escala/utils.py:1794`, `app/escala/utils.py:977`, `app/escala/utils.py:999`). Portanto, o descarte do item 3 da triagem se sustenta para esse dia extra; o ramo legacy sem vínculo continua retornando antes do overlay e não foi alterado pelo diff (`app/escala/utils.py:987`).

   A ordem agora é todas as folgas, depois as ausências; o tratamento conserva folga diante de falta aprovada e permite que ausência não falta a sobreponha (`app/escala/utils.py:1794`, `app/escala/utils.py:1797`, `app/escala/utils.py:1178`). A medição desse impacto mencionada na triagem não foi apresentada [nao verificado].

3. **Versão 3 e horizonte**

   `REGRA_VERSOES` é lida pela validação no import e por `versao_para`; este escolhe a maior versão aplicável à competência, sem teto superior (`app/ponto/services/cartorio.py:177`, `app/ponto/services/cartorio.py:201`, `app/ponto/services/cartorio.py:206`).

   | Competência | Versão resultante |
   |---|---|
   | Até 2026-08 | `None` |
   | 2026-09 | `1` |
   | Desde 2026-10, inclusive futuras | `3` |

   Essas respostas seguem a comparação dos horizontes com a competência derivada da janela da empresa, não do mês civil (`app/ponto/services/cartorio.py:203`, `app/ponto/services/cartorio.py:206`; `app/ponto/janelas.py:60`).

   O chamador de produção de `versao_para` é `_regra_do_dia`, usado nos dois ramos que montam a impressão. A mudança 2→3 altera o hash e permite sair do pulo por impressão igual na próxima execução do cartório (`app/ponto/services/cartorio.py:957`, `app/ponto/services/cartorio.py:1009`, `app/ponto/services/cartorio.py:1161`, `app/ponto/services/cartorio.py:362`, `app/ponto/services/cartorio.py:1167`).

   Os selos da declaração viva foram atualizados. As expectativas restantes de versões 1/2 usam declarações substituídas por `patch` ou valores passados diretamente ao hash; o teste de `nunca_bateu` consulta a versão viva (`app/ponto/tests/test_o219_versao_da_regra.py:76`, `app/ponto/tests/test_o219_versao_da_regra.py:89`, `app/ponto/tests/test_o219_versao_da_regra.py:112`, `app/ponto/tests/test_o219_versao_da_regra.py:136`, `app/ponto/tests/test_o219_versao_da_regra.py:151`, `app/ponto/tests/test_o219_versao_da_regra.py:159`, `app/ponto/tests/test_o219_versao_da_regra.py:166`; `app/ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py:128`).

   **Exportação não é filtro da versão:** qualquer competência exportada desde 2026-10 seria alcançada (`app/ponto/services/cartorio.py:206`). O comentário afirma exportadas somente até 09, mas existência efetiva de exportada alcançada não está demonstrada pelos dados fornecidos [nao verificado] (`app/ponto/services/cartorio.py:172`).

4. **O item 6 da triagem precisa voltar como bloqueador**

   A parte do calendário não responde “pode lançar?”: alimenta a classificação exibida. Logo, sua adaptação não podia parar em `True → (True,)` (`app/colaboradores/services/calendario.py:607`, `app/colaboradores/services/calendario.py:792`, `app/colaboradores/services/calendario.py:799`).

   Os outros dois sítios enfileirados continuam sendo caminhos de porta e não consomem o novo formato: `o_dia_ja_e_folga` consulta o vínculo do dia; `_folga_avulsa` alimenta o veto de lançamento (`app/ponto/services/declarar_dia.py:90`; `app/ponto/precedencia.py:165`, `app/ponto/precedencia.py:501`). Isso não absolve suas regras, mas não demonstra uma regressão de formato causada por este diff nesses dois sítios (`app/ponto/services/declarar_dia.py:70`; `app/ponto/precedencia.py:166`).

5. **Classes de reprovação**

   - **Derivação paralela de fonte única: ocorre no calendário**, pela reconstrução incompleta das candidatas já descrita (`app/colaboradores/services/calendario.py:607`; `app/ponto/precedencia.py:207`).
   - **Segundo juiz novo:** o predicado foi extraído da precedência e compartilhado; não identifiquei duplicação nova da autoridade registrada nesses trechos (`app/core/juizes.py:160`; `app/ponto/precedencia.py:223`, `app/ponto/precedencia.py:358`; `app/escala/utils.py:1124`).
   - **Segundo escritor persistente, relógio fora da borda ou ordem nova de travas:** os trechos acrescentados carregam dados e alteram dicionários locais; não acrescentam escrita ORM, leitura de relógio ou aquisição de trava (`app/ponto/precedencia.py:207`; `app/escala/utils.py:1118`, `app/escala/utils.py:1125`).
   - **Cada teste novo fica vermelho sem a cura? Não.** Por inspeção, quatro das dez execuções herdadas são controles que já passavam: calendário velho no legacy, manual velha por turno e calendário novo nos dois builders. O próprio teste reconhece os dois primeiros; coexistência e manual fora da janela fornecem contraexemplos para ambos (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:21`, `:80`, `:85`, `:91`, `:97`, `:102`). Execução efetiva RED/GREEN [nao verificado].
   - **Pouso antes do pré-requisito:** o ensaio `--apply` é exigido expressamente, mas seus resultados e eventual pouso não foram fornecidos [nao verificado] (`app/ponto/services/cartorio.py:173`, `app/ponto/services/cartorio.py:175`).

## 31/10 13:06 · sessao 5d7fa2e39603 · - · - · origem=Ronald (tmux)

Não encontrei defeito novo nos dois builders quanto à seleção de múltiplas candidatas: o seletor percorre cada candidata até achar a primeira válida; grade e precedência usam esse mesmo seletor (`app/ponto/precedencia.py:217`, `app/ponto/precedencia.py:358`, `app/escala/utils.py:1124`).

- **Legacy / descarte da triagem 3:** o vínculo de cada segmento vem da juíza; `lambda _d: esc` corresponde ao vínculo dos dias preservados. O dia extra `d0-1` é removido pelo recorte (`app/escala/utils.py:972`, `app/escala/utils.py:977`, `app/escala/utils.py:770`, `app/escala/utils.py:995`, `app/escala/utils.py:999`).
- **Legacy sem vínculo:** retorna diretamente dias como folga e pula o overlay; não chama `montar_grade_prevista(None)`. Isso já existia e não é alterado pelo diff fornecido (`app/escala/utils.py:987`, `app/escala/utils.py:989`, `app/escala/utils.py:991`).
- **Por turno:** o mapa passado ao overlay é calculado pela mesma juíza; não há escolha paralela do vínculo acrescentada nesta obra (`app/escala/utils.py:1262`, `app/escala/utils.py:1268`, `app/escala/utils.py:1794`).
- **Ordem dos overlays:** agora todas as folgas precedem todas as ausências, como triagem 2 declara. Ausência não falta vence folga; falta aprovada conserva folga. A ordem está expressa no código; a medição na sombra citada pela triagem não veio como resultados auditáveis (`app/escala/utils.py:1794`, `app/escala/utils.py:1797`, `app/escala/utils.py:1178`) — medição [nao verificado].
- **RED não vale para cada teste individual:** o próprio arquivo reconhece que calendário velho já passava no legacy e mão velha já passava por turno. O caso calendário do vínculo novo também é controle que já passava pelos caminhos anteriores do diff fornecido (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:21`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:80`). Assim, por inspeção, **4/10 execuções herdadas são controles verdes antes da cura**; isso não torna o selo inteiro vazio: coexistência e mão fora da janela têm contraexemplos nos dois builders (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:91`). Execução RED/GREEN/suite [nao verificado].
- **Juiz duplicado:** a seleção compartilhada extrai o predicado anterior da precedência e ambas as testemunhas o leem; não identifiquei segundo juiz novo nesses trechos (`app/ponto/precedencia.py:223`, `app/ponto/precedencia.py:358`, `app/escala/utils.py:1115`). A autoridade registrada para precedência permanece `fatos_do_dia` (`app/core/juizes.py:160`).
- **Escrita/relógio/travas:** os novos caminhos carregam FolgaDia e alteram somente o dicionário local da grade; não acrescentam escrita ORM, leitura de relógio ou aquisição de trava (`app/ponto/precedencia.py:207`, `app/escala/utils.py:1118`, `app/escala/utils.py:1125`).

## 31/10 13:05 · sessao aa849a92299f · CODEX-CUSTO-POR-OBRA · verificador · origem=Code

**1) As fontes existem e são legíveis como descrito?**

- **“JSONL do Codex” é ambíguo.** O wrapper grava eventos em `logs/codex/<obra>_<data>_<pid>.jsonl`; o leitor de tokens/cota busca sessões em `~/.codex/sessions`. São fontes distintas, que o contrato precisa identificar. (`bin/codex_pergunta.sh:156–158`; `bin/codex_limite_estado.py:127–128,198–208`)
- **`.q/.r` não são um registro padronizado pelo wrapper:** pergunta e resposta são caminhos recebidos como argumentos; o log não guarda esses caminhos, a volta ou os anexos. (`bin/codex_pergunta.sh:39–40,53–60,88–89`)
- **Falta nomear `logs/codex/chamadas.log`:** é a fonte atual de obra, papel, início e identificação da sessão, necessária ao casamento com os tokens. (`bin/codex_limite_estado.py:49–68,185–194`)
- A cópia inspecionada não contém `logs/`; existência, conteúdo e legibilidade dos arquivos reais e dos “35 verificadores”: **[nao verificado]**. O diretório está ignorado pelo versionamento. (`.gitignore:53`)

**O universo proposto está errado:** chamadas não equivalem a linhas JSONL; há linhas de resposta, raciocínio e ferramentas. Hoje contam-se os `ini` do wrapper, inclusive tentativas que falham antes de executar o Codex. Além disso, tokens usam dia civil e sessões identificadas por `fim`, enquanto chamadas usam janela de 24 horas. (`bin/gerar_codex_estado.py:7–11`; `bin/codex_limite_estado.py:71–78,188–194`; `bin/codex_pergunta.sh:88–105`)

**2) Que medida não tem fonte na casa?**

A principal lacuna do escopo registrado é **achados procedentes/descartados**, portanto também **tokens por achado procedente por papel**. Essas medidas estão no aval, mas o contrato não define uma fonte de triagem ligada à chamada; há, por exemplo, triagem registrada em prosa e remetida a scratchpad. A resposta do Codex não basta para estabelecer a decisão posterior sobre seus achados. (`app/docs/PROMPTS.md:1725`; `app/docs/RELATO.md:65,137`)

Também faltam duas definições de alimentação:

- **Razão medida tokens→pp:** o leitor calcula delta de cota diária e tokens separadamente; não fornece uma calibração ou atribuição por obra. (`bin/codex_limite_estado.py:165–182,185–213`)
- **ANEXO-ENXUTO por volta:** volta e anexos são reconhecidos durante a inspeção do comando, mas não são preservados no registro de chamadas. O contador publicado agrega disparos por regra; não demonstra quantas voltas elegíveis foram verificadas. (`bin/disjuntor.py:975–988,1182–1183`; `bin/codex_pergunta.sh:53–60`)

**3) Que falha pode imprimir ZERO em vez de “sem medida”?**

Há três caminhos concretos:

- `chamadas.log` ausente → listas vazias → todos os papéis com **0 chamadas**. (`bin/codex_limite_estado.py:53–54,74–79`)
- Exceção na leitura das chamadas → `None` → `_codex_txt` converte em `{}` e imprime **“nenhuma”**, perdendo o motivo. (`bin/disjuntor.py:1208–1211,1259–1263`)
- Trilha de disparos ausente/ilegível → `OSError` ignorado → `disparos` vazio → `linha` imprime **`ANEXO-ENXUTO=0`**. (`bin/disjuntor.py:1156,1188–1189,1275–1277`)

Analogamente, sessões de tokens ausentes podem deixar `{}`, exibido como **“nenhuma hoje”**. O contrato precisa preservar a diferença entre leitura completa sem ocorrências e fonte indisponível/incompleta. (`bin/codex_limite_estado.py:195–213`; `bin/disjuntor.py:1252–1255`)

**4) Quem mais lê os campos/fontes envolvidos?**

- Os campos `codex_tokens`, `codex_orc` e `codex_chamadas` são retornados por `ler`, consumidos por `_codex_txt` e incorporados por `linha`; `gerar_estado.py` publica o resultado. São dados transitórios do painel, distintos do `disjuntor_estado.json` de controle. (`bin/disjuntor.py:59,229–263,1213,1251–1264,1288`; `bin/gerar_estado.py:280–281,365`)
- Outra seção do **mesmo ESTADO** publica chamadas, últimas chamadas e uso semanal pelo módulo `codex_limite_estado`. (`bin/gerar_estado.py:269–270`; `bin/codex_limite_estado.py:46,235–236`)
- **CODEX-ESTADO** usa o casamento sessão→obra/papel para declarar procedência das respostas. (`bin/codex_limite_estado.py:92–96`; `bin/gerar_codex_estado.py:153–156,175–176`)
- **B4-CODEX** também consome o orçamento para negar chamadas; alterar esse cálculo afeta comportamento além do painel. (`bin/disjuntor.py:997–1000,1019–1038`)

## 31/10 13:05 · sessao c528c5728007 · - · - · origem=Ronald (tmux)

Auditoria da versão, somente leitura:

- `REGRA_VERSOES` é consumida em produção pela validação no import e por `versao_para`; esta escolhe a maior versão cujo horizonte seja `<=` à competência. Não existe teto: **09/2026 → 1; <=08/2026 → None; >=10/2026 → 3**, inclusive competências futuras. A competência vem do fim da janela da empresa, não do mês civil. `app/ponto/services/cartorio.py:177`, `app/ponto/services/cartorio.py:201`, `app/ponto/services/cartorio.py:204`, `app/ponto/services/cartorio.py:206`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`.

- O único chamador de produção de `versao_para` é `_regra_do_dia`; ambos os ramos do cartório passam seu resultado à impressão. A versão integra o hash; mudar 2→3 tira a célula do pulo por impressão igual. `app/ponto/services/cartorio.py:955`, `app/ponto/services/cartorio.py:1009`, `app/ponto/services/cartorio.py:1161`, `app/ponto/services/cartorio.py:362`, `app/ponto/services/cartorio.py:1010`, `app/ponto/services/cartorio.py:1167`.

- A ativação passa pelo cron normal: `processar_cartorio --apply` visita todas as células passadas da empresa; não exige `--forcar`. A redação “o deploy rejulga” descreve o efeito na execução seguinte, não um ato executado pela declaração. `app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:61`, `app/ponto/services/cartorio.py:173`.

- Os selos vivos da declaração foram atualizados para v3. Os testes que ainda esperam 1/2 substituem explicitamente a declaração por tuplas de ensaio; o teste de hashes recebe `regra=1/2` diretamente. Não são expectativas esquecidas da declaração viva. `app/ponto/tests/test_o219_versao_da_regra.py:76`, `app/ponto/tests/test_o219_versao_da_regra.py:85`, `app/ponto/tests/test_o219_versao_da_regra.py:89`, `app/ponto/tests/test_o219_versao_da_regra.py:112`, `app/ponto/tests/test_o219_versao_da_regra.py:136`, `app/ponto/tests/test_o219_versao_da_regra.py:151`, `app/ponto/tests/test_o219_versao_da_regra.py:159`, `app/ponto/tests/test_o219_versao_da_regra.py:166`.

- O outro teste consumidor pede a versão viva, portanto acompanha v3 automaticamente. Há também selos por AST verificando os dois chamadores e a forma da declaração. `app/ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py:128`, `app/ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py:130`, `app/ponto/tests/test_o219_versao_da_regra.py:221`, `app/ponto/tests/test_o219_versao_da_regra.py:229`, `app/ponto/tests/test_o219_versao_da_regra.py:243`.

- O código **não exclui exportadas**: se alguma competência >=10/2026 estiver exportada, a v3 a alcança. O comentário declara exportadas somente até 09, mas o anexo não fornece linhas da sonda que permitam confirmar esse estado de produção. `app/ponto/services/cartorio.py:191`, `app/ponto/services/cartorio.py:206`, `app/ponto/services/cartorio.py:172`; existência efetiva de exportada alcançada: **[nao verificado]**.

- O ensaio `--apply` com números de rejulgamento/minutos/fora do horizonte é pré-requisito explícito; o diff somente aponta `logs/grade_folga/`, sem apresentar os resultados. Cumprimento do ensaio e eventual pouso antes dele: **[nao verificado]**. `app/ponto/services/cartorio.py:173`, `app/ponto/services/cartorio.py:174`, `app/ponto/services/cartorio.py:175`.

Não encontrei erro bloqueador específico na seleção da versão ou selo vivo de versão esquecido nas ocorrências acima. `app/ponto/services/cartorio.py:206`, `app/ponto/tests/test_o219_versao_da_regra.py:76`, `app/ponto/tests/test_o219_versao_da_regra.py:89`.

## 31/10 13:05 · sessao 94c6d3dac7ba · - · - · origem=Ronald (tmux)

Há um bloqueador no calendário: trocar `True` por `(True,)` corrige o formato, mas preserva uma carga semanticamente diferente da nova fonte única (`app/colaboradores/services/calendario.py:593-608`).

- `_folgas_mes` contém somente datas do vínculo ativo; perde `calendario_origem_id` e ignora folgas de outros vínculos (`app/colaboradores/services/calendario.py:336`, `app/colaboradores/services/calendario.py:593`; `app/escala/alimentacao.py:66-70`).
- Quando `_cobre`, o calendário fornece essas datas como `(True,)`, suprimindo o carregador real (`app/colaboradores/services/calendario.py:602-608`; `app/ponto/precedencia.py:281-282`).
- O predicado aceita `True` como folga manual incondicionalmente, inclusive se a geradora da célula indicar outro vínculo; o próprio `fatos_do_periodo` completa esse vínculo e consulta a juíza (`app/ponto/precedencia.py:238-240`, `app/ponto/precedencia.py:327-351`).
- Assim, uma folga de calendário do ativo pode valer indevidamente quando o vínculo do dia é outro; reciprocamente, uma folga manual de vínculo antigo fica omitida, embora a fonte real a aceite (`app/colaboradores/services/calendario.py:607`; `app/ponto/precedencia.py:207-209`, `app/ponto/precedencia.py:238-240`).
- Esse caminho classifica o dia na tela: lê `_fatos`, define `folga_humana`, força `trabalho=False` e pode devolver `folga`; não responde apenas “pode lançar?” (`app/colaboradores/services/calendario.py:792-804`, `app/colaboradores/services/calendario.py:116-135`). Portanto o item 6 da triagem deve bloquear no trecho do calendário (`app/colaboradores/services/calendario.py:607`).

Censo completo dos chamadores diretos encontrados:

| API/formato | Produção | Testes diretos | [nao verificado]
|---|---|---|
| `folgas_lancadas_do_periodo` | `app/ponto/precedencia.py:282`; `app/escala/utils.py:1119` | Nenhum encontrado [nao verificado] |
| `folga_lancada_do_dia` | `app/ponto/precedencia.py:358`; `app/escala/utils.py:1124` | Nenhum encontrado [nao verificado] |
| `folga_lancada_vale_no_dia` | `app/ponto/precedencia.py:218` | Nenhum encontrado [nao verificado] |
| parâmetro externo `folgas_lancadas=` | `app/colaboradores/services/calendario.py:607` | Nenhum encontrado [nao verificado] |

Os leitores reais de listas estão corretos: ambos entregam as candidatas ao mesmo seletor, que percorre até a primeira **válida**, e não simplesmente a primeira da lista (`app/ponto/precedencia.py:217-220`, `app/ponto/precedencia.py:342-358`; `app/escala/utils.py:1124`). O booleano final de `fatos['folga_lancada']` é aplicado depois da seleção e não perde candidatas (`app/ponto/precedencia.py:408`).

Executei apenas os dois helpers extraídos via AST, em memória: duas candidatas com a inválida primeiro selecionaram a segunda; o objeto de calendário alheio foi rejeitado e `(True,)` foi aceito, conforme seus ramos (`app/ponto/precedencia.py:217-240`). Ocorrência e quantidade em produção: [nao verificado].

## 31/10 13:04 · sessao fe841d63bff0 · - · - · origem=Ronald (tmux)

Leitores e destinos encontrados:

- `codex_tokens`, `codex_orc` e `codex_chamadas` são campos do dicionário retornado por `ler`, consumidos por `_codex_txt`; `linha` incorpora esse texto. Não correspondem ao estado de controle persistido em `logs/sessao/disjuntor_estado.json`. Referências: `bin/disjuntor.py:1213`, `bin/disjuntor.py:1252`, `bin/disjuntor.py:1256`, `bin/disjuntor.py:1259`, `bin/disjuntor.py:1288`, `bin/disjuntor.py:59`, `bin/disjuntor.py:229`.
- O consumidor externo direto é `gerar_estado.py`, que chama `disjuntor.ler/linha` e insere o resultado na seção “Disjuntores” do ESTADO; a CLI `--linha` também chama as duas funções. Referências: `bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`, `bin/gerar_estado.py:336`, `bin/gerar_estado.py:365`, `bin/disjuntor.py:1295`.
- O ESTADO possui outra seção que lê as mesmas fontes pelo módulo `codex_limite_estado`: chamadas por papel, últimas chamadas e uso semanal. Portanto alterações no esquema compartilhado de `chamadas.log` ou no leitor têm outro consumidor no mesmo documento. Referências: `bin/gerar_estado.py:269`, `bin/gerar_estado.py:270`, `bin/codex_limite_estado.py:46`, `bin/codex_limite_estado.py:235`.
- `CODEX-ESTADO.md` também usa `chamadas.log`: `sessoes_do_code` devolve `(obra, papel)` por thread, e `gerar_codex_estado` usa isso para rotular a procedência das respostas. Referências: `bin/codex_limite_estado.py:92`, `bin/codex_limite_estado.py:96`, `bin/gerar_codex_estado.py:175`, `bin/gerar_codex_estado.py:176`, `bin/gerar_codex_estado.py:153`, `bin/gerar_codex_estado.py:155`.
- O orçamento derivado do JSONL também alimenta decisões de bloqueio B4-CODEX, além do painel. Referências: `bin/disjuntor.py:997`, `bin/disjuntor.py:1000`, `bin/disjuntor.py:1019`, `bin/disjuntor.py:1023`, `bin/disjuntor.py:1028`, `bin/disjuntor.py:1033`.
- A publicação existente gera ESTADO, raspa/copia e publica por commit/push; o timer declarado é de cinco minutos. Referências: `bin/relato.sh:38`, `bin/relato.sh:100`, `bin/relato.sh:102`, `bin/relato.sh:115`, `bin/relato.sh:124`, `bin/relato.sh:126`, `bin/hasner-relato.timer:6`. Timer realmente ativo: `[nao verificado]`.

O escopo registrado inclui medidas ausentes do contrato literal fornecido: achados procedentes/descartados, tokens por achado procedente por papel e série pp/dia desde 29/10. Referências: `app/docs/BACKLOG.md:780`, `app/docs/PROMPTS.md:1725`; contrato literal em `app/docs/RELATO.md:65`.

## 31/10 13:04 · sessao 722e69e39635 · - · - · origem=Ronald (tmux)

- Há **dois JSONL diferentes**: o wrapper grava eventos de `codex exec --json` em `logs/codex/<obra>_<data>_<pid>.jsonl`; os leitores de tokens e cota percorrem `~/.codex/sessions`. O contrato precisa nomear qual deles usa. (`bin/codex_pergunta.sh:156`; `bin/codex_limite_estado.py:127`; `bin/codex_limite_estado.py:198`)

- **Chamadas não equivalem a linhas JSONL**: o schema contém respostas, raciocínio e eventos de ferramentas; o leitor filtra especificamente respostas finais. A contagem existente usa somente linhas `ini` de `chamadas.log`, incluindo tentativas que terminam antes de chamar o Codex, como rc 3/4. (`bin/gerar_codex_estado.py:7`; `bin/gerar_codex_estado.py:96`; `bin/codex_limite_estado.py:71`; `bin/codex_pergunta.sh:88`; `bin/codex_pergunta.sh:99`; `bin/codex_pergunta.sh:105`)

- O log registra **obra, papel, hash, pid, início, sessão, rc e duração**. Não registra `CODEX_VOLTA`, lista de anexos nem caminho original da pergunta/resposta; a pergunta registrada é raspada e truncada em 240 caracteres. Portanto `.q/.r` não têm ligação contratada suficiente com a chamada. (`bin/codex_pergunta.sh:53`; `bin/codex_pergunta.sh:78`; `bin/codex_pergunta.sh:88`)

- O wrapper exige obra e papel válido; sem isso, sai antes do `ini`. O leitor histórico transforma papel desconhecido em `outro`, preservando a obra original; não implementa `obra=?`. (`bin/codex_pergunta.sh:43`; `bin/codex_pergunta.sh:48`; `bin/codex_limite_estado.py:65`; `bin/codex_limite_estado.py:78`)

- **Procedência/descarte dos achados exige outra fonte**: existem registros em prosa e triagem fora da resposta original — por exemplo, 18 procedentes e 7 descartados remetidos a scratchpad. O contrato não define sua leitura nem vínculo com cada chamada; isso afeta também “tokens por achado procedente”. (`app/docs/RELATO.md:137`; `app/docs/PROMPTS.md:1725`; `app/docs/RELATO.md:65`)

- **Razão medida tokens→pp não está implementada** no leitor: cota diária é diferença entre observações semanais; tokens são coletados separadamente pelo último `total_token_usage.total_tokens`, associados por thread. (`bin/codex_limite_estado.py:165`; `bin/codex_limite_estado.py:181`; `bin/codex_limite_estado.py:185`; `bin/codex_limite_estado.py:208`)

- A contagem por tokens usa **dia civil**, apenas chamadas com linha `fim` e sessão identificada; não cobre automaticamente todas as chamadas das últimas 24 horas. (`bin/codex_limite_estado.py:188`; `bin/codex_limite_estado.py:193`)

- **Falha que vira zero/vazio**: `_linhas_chamadas` retorna listas vazias quando o arquivo inexiste; `ler_chamadas` então devolve todos os papéis zerados. `tokens_por_papel` também retorna `{}` quando não encontra alvos e mantém `{}` quando nenhuma sessão correspondente é encontrada. (`bin/codex_limite_estado.py:53`; `bin/codex_limite_estado.py:74`; `bin/codex_limite_estado.py:196`; `bin/codex_limite_estado.py:213`)

- `ANEXO-ENXUTO` conhece volta e anexos **durante a inspeção do comando**, mas o wrapper não preserva esses campos no log de chamadas. Seus anexos cheios são `JUIZES.md`, `LEIS.md`, `CORTES.md`, `CLAUDE.md`; são admitidos em `1`, `contrato`, `final`. (`bin/disjuntor.py:896`; `bin/disjuntor.py:898`; `bin/disjuntor.py:985`; `bin/disjuntor.py:988`; `bin/codex_pergunta.sh:53`)

- Nesta cópia não foi encontrado diretório `logs/`; existência e legibilidade dos dados reais, `.q/.r`, série desde 29/10 e “35 verificadores”: **[nao verificado]**. O diretório de logs está excluído do versionamento. (`.gitignore:53`)

## 31/10 13:03 · sessao 771afd200dcd · REPROVA-E-DESENHO-B3 · verificador · origem=Code

**Parecer: reprovar.** Há caminhos que liberam sem declaração posterior ao disparo, e caminhos que recusam uma declaração nova válida. Os principais problemas estão na migração, na identificação da obra e na separação entre disparo, leitura do RELATO e consumo. (`bin/disjuntor.py:625`, `bin/disjuntor.py:571`, `bin/disjuntor.py:657`, `bin/disjuntor.py:614`)

A análise usou leitura e simulações em memória. A execução integral do selo ficou **[não verificado]**: ele cria árvores temporárias e arquivos, contrariando a restrição desta revisão. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:746`)

**1. Falhas contra o código e o aval**

- **A migração pode perdoar a primeira reprovação nova.** Sem `b3_limiar`, `_migra_limiar` trata todos os `achados` como antigos, inclusive num estado virgem. Move a primeira reprovação para `rearmado`; a varredura seguinte a ignora. Não existe comparação com instante de implantação que distinga reprovação antiga de nova. O selo começa com uma migração preparada antes de testar a primeira reprovação, portanto não cobre essa inicialização. (`bin/disjuntor.py:628`, `bin/disjuntor.py:630`, `bin/disjuntor.py:638`, `bin/disjuntor.py:671`, `bin/tests/test_disjuntor.sh:323`, `bin/tests/test_disjuntor.sh:334`)

- **Estado antigo já disparado migra incompleto.** A migração preserva `disparado=True`, mas não preenche `disparado_em` nem `topo_no_disparo`. A varredura também não os preenche, pois só inicializa esses campos num novo disparo. A liberação então compara o `mtime` com zero e aceita uma declaração anterior ao disparo antigo. Esse comportamento foi reproduzido em memória. (`bin/disjuntor.py:635`, `bin/disjuntor.py:640`, `bin/disjuntor.py:678`, `bin/disjuntor.py:604`)

- **Uma declaração de outra obra libera a obra consultada.** Por exemplo, para OBRA-X, o reconhecedor aceita:
  ```text
  DESENHO OBRA-Y: lei=juiz unico pergunta=quem julga OBRA-X? app/a.py:1
  ```
  Basta OBRA-X aparecer em qualquer posição da linha; o código não vincula a obra ao cabeçalho depois de `DESENHO` ou `ERRO-BOBO`. O selo testa obras em linhas diferentes, sem testar esse cruzamento dentro da mesma linha. (`bin/disjuntor.py:571`, `bin/disjuntor.py:573`, `bin/disjuntor.py:579`, `bin/tests/test_disjuntor.sh:357`)

- **A prova temporal não prova a escrita da linha.** `topo_no_disparo` é fotografado antes da trava; `disparado_em` nasce depois. A liberação usa o `mtime` do arquivo inteiro, que também muda quando o alarme regrava o RELATO preservando seu topo. Isso permite aceitar uma linha anterior ao disparo. (`bin/disjuntor.py:657`, `bin/disjuntor.py:679`, `bin/disjuntor.py:604`, `app/core/esteira_vigia.py:829`, `app/core/esteira_vigia.py:884`)

- **O consumo não está vinculado à geração do disparo.** Depois de soltar a trava, `libera_b3` confere apenas a presença do SHA em `consumidas`. Outro processo pode ter disparado novamente a obra nesse intervalo; a chamada original ainda retorna sucesso. Esse intercalamento foi reproduzido em memória. (`bin/disjuntor.py:272`, `bin/disjuntor.py:614`, `bin/disjuntor.py:679`, `bin/disjuntor.py:622`)

- **A primeira declaração válida na sintaxe pode esconder a segunda, válida para liberar.** O reconhecedor retorna o primeiro resultado; se esse SHA estiver consumido ou no topo antigo, a liberação termina sem tentar a segunda linha. (`bin/disjuntor.py:578`, `bin/disjuntor.py:582`, `bin/disjuntor.py:590`, `bin/disjuntor.py:600`, `bin/disjuntor.py:602`)

- **O CLAUDE.md excede “nome + ponteiro”.** A linha é fisicamente única, mas contém a regra de parada, as duas sintaxes de liberação e o ponteiro do selo. O aval registrado pede somente nome e ponteiro para o disjuntor. (`CLAUDE.md:502`, `app/docs/PROMPTS.md:1733`)

Há também diferenças entre o **aval literal** e o **contrato v2 escrito**: quatro leis permitidas, motivo mínimo de 20 caracteres e consumo por conteúdo são restrições presentes no contrato; sua aprovação específica além do aval citado é **[não verificado]**. Uma justificativa curta como `import errado` não libera, embora seja uma linha de motivo. (`app/docs/RELATO.md:12`, `bin/disjuntor.py:74`, `bin/disjuntor.py:75`, `bin/disjuntor.py:581`)

O reconhecedor verifica apenas forma: `pergunta=app/a.py:1` já satisfaz seu predicado, sem texto de pergunta. Portanto, a presença efetiva da “pergunta sem juiz” não é garantida pelo mecanismo. (`bin/disjuntor.py:576`, `bin/disjuntor.py:577`)

O alcance implementado também é mais estreito que suspender o construtor: B3 bloqueia o próximo `Agent` do tipo `verificador` reconhecido como final; ferramentas comuns da sessão podem retornar antes dessa avaliação. Esse recorte está declarado no contrato v2. (`bin/disjuntor.py:1055`, `bin/disjuntor.py:1073`, `app/docs/RELATO.md:13`)

**2. Escritores e leitores**

Na tabela abaixo, todos os números pertencem a **`bin/disjuntor.py`**; E significa escritor semântico e L, leitor. A persistência física de todos esses campos passa por `_com_estado`, sob `flock`. (`bin/disjuntor.py:229`, `bin/disjuntor.py:242`, `bin/disjuntor.py:257`, `bin/disjuntor.py:263`)

| Campo | Quem escreve — arquivo:linha | Quem lê — arquivo:linha |
|---|---|---|
| `reprovados` | `_migra_limiar`: `bin/disjuntor.py:631`, `:633`, `:639`; `varrer_b3`: `:670`, `:674`; `libera_b3`: `:607`; `rearma_b3`: `:699` | Migração: `bin/disjuntor.py:632`, `:635`, `:637`, `:638`; varredura: `:673`, `:677`; liberação: `:606`; rearme: `:697`, `:698` |
| `rearmado` | Migração: `bin/disjuntor.py:638`; rearme: `:698` | Migração: `bin/disjuntor.py:632`, `:638`; varredura: `:671`; rearme: `:698` |
| `liberado` | Liberação: `bin/disjuntor.py:606` | Liberação: `bin/disjuntor.py:606`; varredura: `:671` |
| `disparado` | Inicializações: `bin/disjuntor.py:631`, `:670`; disparo: `:679`; liberação: `:607`; rearme: `:699` | Liberação: `bin/disjuntor.py:598`; migração: `:635`; varredura: `:678`, `:685`; `b3_disparados`: `:725` |
| `disparado_em` | Varredura: `bin/disjuntor.py:679` | Liberação: `bin/disjuntor.py:604` |
| `consumidas` | Liberação: `bin/disjuntor.py:608` | Liberação: `bin/disjuntor.py:600`, `:608`, `:614` |
| `topo_no_disparo` | Varredura: `bin/disjuntor.py:680` | Liberação: `bin/disjuntor.py:602` |
| `b3_limiar` | Migração: `bin/disjuntor.py:640` | Migração: `bin/disjuntor.py:628`, `:637` |

Não identifiquei outro escritor semântico de produção desses campos. Há o escritor físico `_com_estado` e uma escrita direta de fixture no selo. Stop e PreToolUse usam as mesmas portas de varredura e liberação. (`bin/disjuntor.py:263`, `bin/tests/test_disjuntor.sh:326`, `bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:1076`)

| Linha de trilha | Quem escreve | Quem lê |
|---|---|---|
| `DISJUNTOR B3` | `varrer_b3` → `dispara` → `trilha`. (`bin/disjuntor.py:681`, `bin/disjuntor.py:133`, `bin/disjuntor.py:124`) | `ler` conta disparos B3; o selo exige a linha. (`bin/disjuntor.py:1183`, `bin/tests/test_disjuntor.sh:338`) |
| `LIBERA` | `libera_b3` → `trilha`. (`bin/disjuntor.py:621`) | `ler` conta por tipo; o selo verifica desenho e erro-bobo. (`bin/disjuntor.py:1179`, `bin/tests/test_disjuntor.sh:369`, `bin/tests/test_disjuntor.sh:378`) |
| `MIGRA` | `_migra_limiar` → `trilha`. (`bin/disjuntor.py:636`) | O selo verifica a linha; `ler` não inclui esse evento. (`bin/tests/test_disjuntor.sh:329`, `bin/disjuntor.py:1160`) |
| `FALHA` | Estado, consumo, registro do RELATO, wrapper PreToolUse e wrapper Stop; também tratamento B4. (`bin/disjuntor.py:269`, `:616`, `:619`, `:224`, `:1113`, `:1090`; `bin/hook_stop_fila1.py:1056`) | `ler` só incrementa `sem_relato` quando o detalhe começa por `relato_negacao`; o selo conta todas as linhas FALHA. (`bin/disjuntor.py:1177`, `bin/tests/test_disjuntor.sh:92`) |

**Sim, há escritores de `FALHA` fora das quatro funções citadas**; isso é tratamento de infraestrutura, não um segundo escritor semântico de estado B3. Todas essas linhas chegam ao apêndice comum de `trilha`. (`bin/disjuntor.py:224`, `bin/disjuntor.py:269`, `bin/disjuntor.py:1113`, `bin/hook_stop_fila1.py:1056`, `bin/disjuntor.py:124`)

Quanto aos leitores, a varredura foi atualizada para ignorar `liberado`, e o gerador do ESTADO continua delegando a composição a `disjuntor.ler/linha`. O problema remanescente é que `ler` não expõe falhas de persistência/consumo no contador apresentado como “sem linha no RELATO”, nem lê `MIGRA`; isso limita a observabilidade, sem provar um leitor ainda usando limiar 3. (`bin/disjuntor.py:671`, `bin/gerar_estado.py:280`, `bin/disjuntor.py:1160`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:1284`)

**3. Checklist das classes da casa**

As classes solicitadas estão enumeradas na regra de revisão da casa. (`app/docs/PISTA.md:103`)

| Classe | Este diff cai nela? |
|---|---|
| **Segundo juiz duplicando juiz registrado** | **Não demonstrado.** O reconhecedor antigo do Stop foi removido, e ambos os caminhos chamam `libera_b3`. Não há fundamento nesta revisão para chamar o parser de segundo juiz de negócio registrado. (`bin/hook_stop_fila1.py:411`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:1076`) |
| **Segundo escritor fora da porta** | **Não identificado para o estado B3.** As mutações passam pelas quatro funções e pela persistência comum. O alarme do RELATO continua usando `no_relato`. (`bin/disjuntor.py:606`, `:638`, `:679`, `:698`, `:263`, `:221`) |
| **Relógio lido fora da borda** | **Sim.** PreToolUse e Stop obtêm `agora`, mas `varrer_b3` chama outro `time.time()` dentro da transição. (`bin/disjuntor.py:1101`, `bin/hook_stop_fila1.py:1038`, `bin/disjuntor.py:679`) |
| **Derivação paralela de fonte única** | **Parcial.** A identidade da obra e o reconhecedor são compartilhados; porém, a novidade temporal da linha é inferida de duas medidas separadas — fotografia do topo e `mtime` global — que podem divergir. Há ainda duas definições de `_sem_acento`, com a segunda sobrescrevendo a primeira. (`bin/disjuntor.py:502`, `:517`, `:564`, `:657`, `:604`, `:479`, `:555`) |
| **Selo/RED sem mordida** | **Lacuna de concorrência; limiar bem coberto por inspeção.** O caso exige parada após uma reprovação e declara o mutante de limiar 3. Entretanto, substituir o `flock` do estado por `pass` sobreviveria ao caso B3 sequencial — inferência estática, execução **[não verificado]**. (`bin/tests/test_disjuntor.sh:334`, `:336`, `:822`, `:320`; `bin/disjuntor.py:242`) |
| **Ordem de trava; Stop × PreToolUse** | **Sim.** Stop lê o topo antes de varrer; PreToolUse varre antes de ler. Fotografia, disparo, consumo e confirmação não formam uma única operação serializada. O defeito demonstrado é de corrida; deadlock **[não verificado]**. (`bin/hook_stop_fila1.py:1044`, `bin/disjuntor.py:1076`, `:657`, `:610`, `:614`) |
| **Pouso antes do pré-requisito** | **Não demonstrado.** Existe ordem expressa para pousar depois do produto em curso; o BACKLOG ainda registra construção em cópia. Violação cronológica de merge/deploy é **[não verificado]**. (`app/docs/PROMPTS.md:1733`, `app/docs/BACKLOG.md:779`) |

**4. Sequências de eventos**

**Sim: uma obra disparada pode receber novo final sem linha válida escrita depois daquele disparo.** Os caminhos concretos são:

1. **Corrida após o consumo:** PreToolUse A consome a linha do disparo 1 e solta a trava → Stop B encontra outra reprovação e grava o disparo 2 → A confere somente o SHA antigo em `consumidas` → A retorna sucesso e admite o final para uma obra novamente disparada. (`bin/disjuntor.py:607`, `:272`, `:679`, `:614`, `:622`, `:1076`)

2. **Linha anterior rejuvenescida pelo alarme:** a varredura fotografa o topo sem L → L é escrita → ocorre o disparo, cuja fotografia não contém L → a negativa escreve um alarme, preservando L e renovando o `mtime` → o próximo PreToolUse aceita L, embora ela seja anterior ao disparo. (`bin/disjuntor.py:657`, `:679`, `:1111`, `:602`, `:604`; `app/core/esteira_vigia.py:829`, `:884`)

3. **Disparo não persistido e troca de sessão:** a reprovação dispara e a gravação do estado falha → a chamada corrente continua negada → troca-se a sessão → a próxima varredura só encontra os subagentes da sessão nova → a reprovação perdida não restaura o disparo. (`bin/disjuntor.py:269`, `:270`, `:319`, `:645`, `:655`, `:688`)

4. **Estado antigo já disparado:** migração preserva o disparo sem preencher sua referência temporal → uma declaração antiga com `mtime` positivo passa pela comparação com zero. (`bin/disjuntor.py:635`, `:640`, `:678`, `:604`)

5. **Trava ocupada ou estado ilegível:** uma obra já disparada chega ao PreToolUse → `_com_estado` levanta → o wrapper registra FALHA e deixa a ferramenta passar. Esse caminho é a política de falha aberta declarada no contrato, não uma novidade acidental desta obra. (`bin/disjuntor.py:247`, `:254`, `:1112`; `app/docs/RELATO.md:13`)

**Também sim: uma linha válida escrita depois do disparo pode não liberar.**

- Primeira linha consumida + segunda linha nova válida: o reconhecedor escolhe a primeira e a liberação não tenta a segunda. (`bin/disjuntor.py:569`, `:578`, `:590`, `:600`)
- Reescrita posterior do mesmo conteúdo fotografado no disparo: continua barrada por `topo_no_disparo`. Isso exige conteúdo diferente, além de escrita posterior. (`bin/disjuntor.py:602`)
- Reescrita de conteúdo consumido anteriormente: continua barrada por `consumidas`; aqui o comportamento corresponde ao contrato v2 de consumo único. (`bin/disjuntor.py:600`, `app/docs/RELATO.md:12`)
- Falha ao persistir o consumo: a negativa fica. Essa proteção existe e é exercitada pelo selo. (`bin/disjuntor.py:614`, `:619`, `bin/tests/test_disjuntor.sh:365`, `:366`)
- Linha entra entre a leitura do Stop e sua decisão: aquela execução usa a fotografia anterior e não libera; uma chamada posterior pode reconhecer a linha. (`bin/hook_stop_fila1.py:1044`, `:1045`, `:1046`)

## 31/10 13:01 · sessao 642bf1fa0dfc · - · - · origem=Ronald (tmux)

Achados confirmados por leitura e chamada pura em memória (`python3 -B`, sem gravar arquivos):

1. **Linha de outra obra libera a obra disparada.** `linha_libera_b3('OBRA-X', ['DESENHO OBRA-Y: lei=juiz unico pergunta=quem julga OBRA-X? app/a.py:1'])` retorna `('desenho', linha)`. Também aceita para OBRA-X `ERRO-BOBO OBRA-Y: consertei um import da OBRA-X`. O reconhecedor procura o ID em toda a linha e só exige prefixo DESENHO/ERRO-BOBO, sem vincular o ID ao cabeçalho. `bin/disjuntor.py:571`, `bin/disjuntor.py:573`, `bin/disjuntor.py:579`. O teste cobre IDs em linhas diferentes, sem cobrir este cruzamento na mesma linha. `bin/tests/test_disjuntor.sh:357`.

2. **CLAUDE.md excede o “nome + ponteiro” literal.** É uma linha física, porém repete a condição de parada, toda a sintaxe de liberação e o ponteiro do selo. `CLAUDE.md:502`. `.ref` passa a preservar também ERRO-BOBO, além de REPROVA-E-DESENHO. `bin/tests/claude_md_regras.ref:29`, `bin/tests/claude_md_regras.ref:94`. O aval registrado pede apenas nome e ponteiro. `app/docs/PROMPTS.md:1733`.

3. **Mordida específica do limiar existe.** Mutante `B3_REPROVACOES = 3` está declarado e o caso exige parada após uma reprovação. `bin/tests/test_disjuntor.sh:822`, `bin/tests/test_disjuntor.sh:334`, `bin/tests/test_disjuntor.sh:336`, `bin/tests/test_disjuntor.sh:338`. Não executei o selo porque cria arquivos. `bin/tests/test_disjuntor.sh:39`.

4. **Os mutantes sem-mtime e sem-consumo têm casos que os matam.** A linha com mtime antigo entra depois do primeiro disparo com topo neutro; removida a comparação, a liberação quebra `_segue`. `bin/tests/test_disjuntor.sh:359`, `bin/tests/test_disjuntor.sh:360`, `bin/tests/test_disjuntor.sh:348`, `bin/disjuntor.py:604`. A repetição consumida ocorre após novo disparo com topo neutro, evitando que `topo_no_disparo` mascare o teste. `bin/tests/test_disjuntor.sh:373`, `bin/tests/test_disjuntor.sh:374`.

5. **Falta mordida da exclusão do estado e concorrência Stop × PreToolUse.** Mutante que substitua o flock do estado por `pass` sobreviveria ao caso B3 sequencial — inferência estática, sem execução. `bin/disjuntor.py:242`, `bin/tests/test_disjuntor.sh:320`, `bin/tests/test_disjuntor.sh:420`. As disputas de lock existentes cobrem sombra.lock e diretório do RELATO. `bin/tests/test_disjuntor.sh:198`, `bin/tests/test_disjuntor.sh:256`, `bin/tests/test_disjuntor.sh:458`, `bin/tests/test_disjuntor.sh:642`.

6. **Não tratar automaticamente as restrições adicionais como invenção do código:** o contrato v2 já documenta duas linhas úteis, quatro leis, motivo mínimo de 20 caracteres, consumo por SHA, migração e falha aberta. `app/docs/RELATO.md:12`, `app/docs/RELATO.md:13`. Isso comprova contrato escrito, sem demonstrar por si só que cada ampliação foi aprovada. [não verificado]

7. **Pouso antes do pré-requisito:** existe ordem expressa “depois do produto em curso”, mas o snapshot e o diff sozinhos não provam violação cronológica do pouso B3. `app/docs/PROMPTS.md:1733`. A obra ainda consta como “construção em cópia”. `app/docs/BACKLOG.md:779`.

## 31/10 13:01 · sessao 3dd2487ad585 · - · - · origem=Ronald (tmux)

Achados da integração, todos por leitura:

- **Relógio fora da borda:** `pretool(payload, agora)` recebe o instante da borda, mas `varrer_b3` ignora esse valor e chama `time.time()` dentro da mutação; o Stop também obtém `agora` e não o passa ao B3. (`bin/disjuntor.py:1050`, `bin/disjuntor.py:1101`, `bin/disjuntor.py:679`, `bin/hook_stop_fila1.py:1038`)

- **Snapshot sem serialização com o disparo:** o topo é lido antes de `_com_estado`; o `flock` só protege o estado. Portanto, uma linha pode entrar depois do snapshot e antes de `disparado_em`. Um alarme posterior regrava o RELATO, preservando o topo, e renova o mtime; a próxima chamada aceita a linha que já existia antes do disparo porque seu hash não ficou no snapshot. (`bin/disjuntor.py:657`, `bin/disjuntor.py:679`, `bin/disjuntor.py:600`, `bin/disjuntor.py:604`, `app/core/esteira_vigia.py:829`, `app/core/esteira_vigia.py:859`, `app/core/esteira_vigia.py:884`)

- **Linha posterior válida pode não liberar:** uma linha reescrita após o disparo continua recusada se seu conteúdo já estiver em `topo_no_disparo` ou `consumidas`. O predicado prova conteúdo inédito, não escrita posterior. (`bin/disjuntor.py:600`, `bin/disjuntor.py:602`)

- **Primeira linha sombreia a segunda:** com duas linhas sintaticamente válidas para a mesma obra, a primeira é devolvida imediatamente. Se ela estiver consumida ou no snapshot, `libera_b3` rejeita sem examinar a segunda, mesmo que esta seja nova e posterior ao disparo. (`bin/disjuntor.py:569`, `bin/disjuntor.py:578`, `bin/disjuntor.py:582`, `bin/disjuntor.py:590`, `bin/disjuntor.py:600`)

- **Stop e PreToolUse usam fotografias diferentes:** o Stop lê topo e mtime antes de varrer; o PreToolUse varre antes de ler. Isso amplia as janelas de corrida, embora ambos usem a mesma porta de liberação. (`bin/hook_stop_fila1.py:1044`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:1076`)

- **Falha de trava/leitura deixa passar:** `_com_estado` levanta após aproximadamente um segundo de disputa do `flock`, ou diante de estado ilegível; a exceção chega ao `main_pretool`, que registra FALHA e libera a ferramenta. Isso também vale para novo final de obra já disparada. (`bin/disjuntor.py:240`, `bin/disjuntor.py:247`, `bin/disjuntor.py:254`, `bin/disjuntor.py:1112`)

- **Não encontrei fundamento para acusar segundo juiz de negócio:** o reconhecedor frouxo do Stop foi removido, ambos os caminhos chamam `libera_b3`, e o reconhecedor novo mantém a leitura compartilhada da identidade da obra. (`bin/hook_stop_fila1.py:411`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:1076`, `bin/disjuntor.py:564`, `bin/disjuntor.py:571`, `bin/disjuntor.py:502`, `bin/disjuntor.py:517`) Vínculo com pergunta de negócio registrada em `core/juizes.py`: [não verificado].

- **Não há prova de pouso real antes do pré-requisito nesta análise.** A quebra demonstrada é permitir nova volta sem evidência temporal correta; realização de merge/deploy pela obra: [não verificado]. [nao verificado]

- **A liberação normal pelo Stop não provoca redisparo da mesma reprovação:** ela move os agentes para `liberado`, e a varredura seguinte os ignora. (`bin/disjuntor.py:606`, `bin/disjuntor.py:671`)

## 31/10 13:01 · sessao bd7b292cc2b4 · - · - · origem=Ronald (tmux)

Auditoria somente por leitura; os achados abaixo decorrem do fluxo do código, sem execução da suíte. [não verificado] [nao verificado]

| Campo | Escritores em `bin/disjuntor.py` | Leitores em `bin/disjuntor.py` |
|---|---|---|
| `reprovados` | `_migra_limiar`:631,633,639; `varrer_b3`:670,674; `libera_b3`:607; `rearma_b3`:699 | migração:632,635,637,638; varredura:673,677; liberação:606; rearme:697,698 |
| `rearmado` | migração:638; rearme:698 | migração:632,638; varredura:671; rearme:698 |
| `liberado` | liberação:606 | liberação:606; varredura:671 |
| `disparado` | inicialização:631,670; varredura:679; liberação:607; rearme:699 | liberação:598; migração:635; varredura:678,685; `b3_disparados`:725 |
| `disparado_em` | varredura:679 | liberação:604 |
| `consumidas` | liberação:608 | liberação:600,608,614 |
| `topo_no_disparo` | varredura:680 | liberação:602 |
| `b3_limiar` | migração:640 | migração:628,637 |

A persistência física passa por `_com_estado`, que lê, executa a mutação e substitui o JSON sob `flock`; não identifiquei outro escritor semântico de produção desses campos. O teste escreve diretamente uma fixture de estado. (`bin/disjuntor.py:229`, `bin/disjuntor.py:257`, `bin/disjuntor.py:263`, `bin/tests/test_disjuntor.sh:326`)

As duas entradas de produção usam as mesmas portas: PreToolUse chama `varrer_b3` e `libera_b3`; Stop também. A função `b3_disparados` permanece definida, mas sua utilização encontrada é apenas no mutante que substitui a varredura por uma leitura do estado. (`bin/disjuntor.py:1076`, `bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:724`, `bin/tests/test_disjuntor.sh:825`)

| Trilha | Escritor | Leitor |
|---|---|---|
| `DISJUNTOR B3` | `varrer_b3` chama `dispara`, que chama `trilha` | `ler` conta `p[3]` como B3 |
| `LIBERA` | `libera_b3` chama `trilha` | `ler` reconhece desenho/erro-bobo |
| `MIGRA` | `_migra_limiar` chama `trilha` | Não há evento MIGRA na lista lida por `ler` |
| `FALHA` | `_com_estado`, `libera_b3`, `main_pretool`, Stop e registro do RELATO | `ler` só soma `sem_relato` para detalhe `relato_negacao` |

Referências: `bin/disjuntor.py:120`, `bin/disjuntor.py:133`, `bin/disjuntor.py:681`, `bin/disjuntor.py:621`, `bin/disjuntor.py:636`, `bin/disjuntor.py:269`, `bin/disjuntor.py:616`, `bin/disjuntor.py:619`, `bin/disjuntor.py:1113`, `bin/disjuntor.py:224`, `bin/hook_stop_fila1.py:1056`, `bin/disjuntor.py:1160`, `bin/disjuntor.py:1177`, `bin/disjuntor.py:1179`, `bin/disjuntor.py:1183`.

Os seguintes cenários precisam entrar na consolidação:

- **Novo verificador com um novo disparo já ativo:** A consome uma linha para o disparo 1; `_com_estado` solta a trava; B observa outra reprovação e grava o disparo 2; A confere somente se o SHA antigo continua em `consumidas`, retorna sucesso e deixa o verificador entrar. A conferência não verifica `disparado` nem a identidade do disparo consumido. (`bin/disjuntor.py:272`, `bin/disjuntor.py:614`, `bin/disjuntor.py:679`, `bin/disjuntor.py:622`, `bin/disjuntor.py:1076`)
- **Disparo perdido após falha de gravação e troca de sessão:** a decisão em memória ainda nega a primeira chamada, mas o JSON conserva o estado anterior; a sessão seguinte varre somente seu próprio diretório `subagents`, perdendo a reprovação que não foi persistida. (`bin/disjuntor.py:269`, `bin/disjuntor.py:270`, `bin/disjuntor.py:319`, `bin/disjuntor.py:645`, `bin/disjuntor.py:655`, `bin/disjuntor.py:688`)
- **Linha válida nova não libera:** a primeira linha sintaticamente válida já foi consumida; a segunda linha contém uma declaração nova e válida. O reconhecedor retorna a primeira, e `libera_b3` a rejeita sem procurar a segunda. (`bin/disjuntor.py:569`, `bin/disjuntor.py:578`, `bin/disjuntor.py:582`, `bin/disjuntor.py:590`, `bin/disjuntor.py:600`)
- **Declaração de outra obra libera a obra disparada:** `DESENHO OBRA-Y: lei=juiz unico pergunta=por que OBRA-X falha? app/a.py:1` pode liberar OBRA-X, pois basta o ID aparecer em qualquer lugar da linha; o alvo após `DESENHO` não é vinculado à obra consultada. O mesmo ocorre com `ERRO-BOBO`. (`bin/disjuntor.py:571`, `bin/disjuntor.py:573`, `bin/disjuntor.py:577`, `bin/disjuntor.py:579`)
- **Cobertura insuficiente para esse vínculo:** o selo testa OBRA-Y numa linha e OBRA-X em outra, mas esse caso não cobre OBRA-X no corpo da própria declaração de OBRA-Y. (`bin/tests/test_disjuntor.sh:357`)
- **Falha de consumo persistido conserva a negativa:** aqui há proteção explícita: depois da tentativa de gravação, a ausência do SHA no estado provoca `FALHA` e retorno sem liberação. O selo exercita essa situação. (`bin/disjuntor.py:614`, `bin/disjuntor.py:619`, `bin/tests/test_disjuntor.sh:365`, `bin/tests/test_disjuntor.sh:366`)

## 31/10 12:57 · sessao a4ce2daa69ca · V3-RETOMA · verificador · origem=Code

A correção está incompleta: **o template passou a ser o do dia, mas a CCT continua consultada em `ini`, e o calendário continua vindo do chamador**. (`app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:507`, `app/ponto/calculador/alimentacao.py:524`)

As conclusões abaixo são de inspeção estática. Execução dos testes, mutantes, RED/GREEN/suite e efeitos concretos em produção: **[nao verificado]**. [nao verificado]

**1) Quem escreve e quem lê os cinco insumos**

A origem comum dos quatro primeiros é `_te_dia`: começa com o template do chamador e é substituído pelo template do vínculo que `vinculo_do_dia` escolheu para `k`. Sem vínculo/template conhecido, ou havendo exceção, permanece o declarado pelo chamador. (`app/ponto/calculador/alimentacao.py:487`, `app/ponto/calculador/alimentacao.py:490`, `app/ponto/calculador/alimentacao.py:494`, `app/ponto/calculador/alimentacao.py:498`)

`_cct_de` monta outro motor, consulta os leitores únicos e guarda o resultado por `template.pk`. **Passa `ini` à fábrica, não `k`.** (`app/ponto/calculador/alimentacao.py:389`, `app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:403`)

| Insumo | Quem resolve/escreve o valor entregue | Leitores no caminho v3 de produção |
|---|---|---|
| `prorrogacao_pos5h` | `get_motor_cct` injeta a regra legal ou a régua cadastrada; `MotorBase` grava `prorrogacao_noturna_pos5h`; `cct_do_motor` transcreve; alimentação entrega. (`app/core/regua_cct.py:454`, `app/core/regua_cct.py:466`, `app/ponto/motor_calculo_v2.py:712`, `app/ponto/calculador/nucleo.py:348`, `app/ponto/calculador/alimentacao.py:508`) | Guarda das noturnas, guarda do split de HE, janela noturna e desconto noturno dos intervalos. (`app/ponto/calculador/regras.py:609`, `app/ponto/calculador/regras.py:627`, `app/ponto/calculador/regras.py:80`, `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:824`) |
| `hora_reduzida_afastada` | Fábrica aplica a cláusula cadastrada somente ao 12x36, respeitando a trava; motor grava; fronteira transcreve; alimentação entrega. (`app/core/regua_cct.py:470`, `app/ponto/motor_calculo_v2.py:719`, `app/ponto/calculador/nucleo.py:349`, `app/ponto/calculador/alimentacao.py:509`) | Guarda e fator das noturnas, tanto no ramo de folga quanto no normal. (`app/ponto/calculador/regras.py:609`, `app/ponto/calculador/regras.py:88`, `app/ponto/calculador/regras.py:715`, `app/ponto/calculador/regras.py:839`) |
| `feriado_em_dobra` | `get_motor` resolve pelo ciclo e `CICLOS_FERIADO_SIMPLES`; a fábrica CCT pode conceder dobra ao 12x36; motor grava; fronteira transcreve; alimentação entrega. (`app/ponto/motor_calculo_v2.py:3607`, `app/core/regua_cct.py:477`, `app/ponto/motor_calculo_v2.py:737`, `app/ponto/calculador/nucleo.py:351`, `app/ponto/calculador/alimentacao.py:510`) | Guarda de cadastro/calendário e `dobra_de_feriado_do_dia`, cujo contexto empresta o juiz do motor. (`app/ponto/calculador/regras.py:633`, `app/ponto/calculador/regras.py:776`, `app/ponto/calculador/regras.py:290`, `app/ponto/calculador/regras.py:347`) |
| `pontualidade_fora_por_lei` | A tabela `NAO_JULGA_PONTUALIDADE` declara as classes; `motivo_nao_julga_pontualidade` faz lookup pela identidade da classe do novo motor. Alimentação entrega a causa. (`app/ponto/calculador/regras.py:487`, `app/ponto/calculador/regras.py:518`, `app/ponto/calculador/alimentacao.py:403`, `app/ponto/calculador/alimentacao.py:531`) | `do_dia` declara atraso e antecipada e impede o julgamento da pontualidade quando existe causa. (`app/ponto/calculador/regras.py:618`) |
| `feriados` | **O chamador.** Alimentação converte o argumento em conjunto e o entrega; não usa `_cct.feriados`. Fechamento e espelho montam o calendário pelo município do posto atual do colaborador. (`app/ponto/calculador/alimentacao.py:407`, `app/ponto/calculador/alimentacao.py:524`, `app/ponto/services/fechamento.py:622`, `app/ponto/services/espelho.py:147`, `app/core/feriados.py:113`) | Alimentação usa na classificação `folga_e_feriado`; regra usa na guarda e na dobra; contexto copia o conjunto. (`app/ponto/calculador/alimentacao.py:523`, `app/ponto/calculador/regras.py:633`, `app/ponto/calculador/regras.py:776`, `app/ponto/calculador/regras.py:294`) |

O cadastro das três cláusulas está em `Sindicato`; a tela monta os valores a partir do POST e os atribui ao sindicato. A escolha da convenção depende da empresa, da praça do vínculo na data recebida e da vigência. (`app/core/models.py:47`, `app/core/models.py:56`, `app/core/models.py:72`, `app/core/views_config.py:278`, `app/core/views_config.py:297`, `app/core/views_config.py:309`, `app/core/regua_cct.py:329`, `app/core/regua_cct.py:366`)

Os demais leitores relevantes são:

- **V3 de ensaio, `periodo.py`:** `noturno_no_par` lê os dois flags noturnos; `reler` usa `cct.feriados` e `cct.feriado_em_dobra`; `pontualidade_no_dia` recebe a causa `fora_por_lei`. Essa API recebe uma CCT e uma causa do chamador, sem resolução por dia. O próprio módulo a declara caminho de ensaio. (`app/ponto/calculador/periodo.py:508`, `app/ponto/calculador/periodo.py:509`, `app/ponto/calculador/periodo.py:830`, `app/ponto/calculador/periodo.py:463`, `app/ponto/calculador/periodo.py:540`, `app/ponto/calculador/periodo.py:543`)
- **Outro helper v3:** `feriado_em_dobra_do_ciclo` deriva o predicado diretamente da lista do motor; não considera a concessão por CCT. Os chamadores localizados são testes. (`app/ponto/calculador/regras.py:298`, `app/ponto/calculador/regras.py:306`, `app/ponto/tests/test_s5b_regra_pontualidade.py:289`, `app/core/regua_cct.py:477`)
- **V2, flags noturnos:** prorrogação é lida no cálculo noturno, split de HE e desconto dos intervalos; hora reduzida é lida em `fator_hora_noturna`. (`app/ponto/motor_calculo_v2.py:922`, `app/ponto/motor_calculo_v2.py:955`, `app/ponto/motor_calculo_v2.py:1866`, `app/ponto/motor_calculo_v2.py:930`)
- **V2, dobra:** `_feriado_dobra_no_dia` lê o flag; os quatro sítios de pagamento chamam esse juiz. Há também leitura direta no ramo de alerta. (`app/ponto/motor_calculo_v2.py:879`, `app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:3030`, `app/ponto/motor_calculo_v2.py:3193`, `app/ponto/motor_calculo_v2.py:3275`, `app/ponto/motor_calculo_v2.py:2756`)
- **V2, calendário:** `eh_feriado` lê o conjunto e consulta outro calendário quando a data não está nele: `is_feriado(data)`, sem município. Essa resposta alimenta dobra, bloqueio de HE, ramos diários e exclusão da soma semanal. (`app/ponto/motor_calculo_v2.py:903`, `app/ponto/motor_calculo_v2.py:905`, `app/ponto/motor_calculo_v2.py:41`, `app/ponto/motor_calculo_v2.py:1387`, `app/ponto/motor_calculo_v2.py:2734`, `app/ponto/motor_calculo_v2.py:3020`, `app/ponto/motor_calculo_v2.py:3095`, `app/ponto/motor_calculo_v2.py:3189`, `app/ponto/motor_calculo_v2.py:3268`)
- **Carimbo:** `impressao_da_regua` lê os três flags do motor efetivamente montado, utilizado pelo fechamento, espelho e PDF. (`app/core/regua_cct.py:423`, `app/core/regua_cct.py:424`, `app/core/regua_cct.py:425`, `app/ponto/services/fechamento.py:265`, `app/ponto/services/espelho.py:1186`, `app/relatorios/pdf_espelho.py:435`)
- **Exportação:** relê a cláusula correlata `feriado_12x36_em_dobra` em `fim`, e decide a supressão para o fechamento inteiro pelo ciclo/cláusula recebidos. (`app/folha/export.py:359`, `app/folha/export.py:369`, `app/folha/export.py:411`)
- **Frase de pontualidade da tela:** `folha_julga_pontualidade` usa o mesmo leitor da causa, mas aceita template do chamador; sem template resolve o vínculo da data e, sem data, usa hoje. Não fornece as células à consulta do vínculo. (`app/chamados/juizes.py:1530`, `app/chamados/juizes.py:1539`, `app/chamados/juizes.py:1543`, `app/chamados/juizes.py:1545`, `app/chamados/juizes.py:1551`)
- **Escala:** `feriado_vira_folga`, chamado pela alimentação, consulta novamente o juiz do calendário, usando o posto atual do colaborador. (`app/ponto/calculador/alimentacao.py:523`, `app/escala/models.py:1362`)

Portanto, **sim, continuam existindo fontes diferentes**: template residual do chamador; CCT de `ini` na alimentação contra CCT de `fatia_ini` no motor; cláusula de `fim` na exportação; calendário do posto atual; calendário injetado contra fallback nacional; e calendário zerado pelo intermitente. (`app/ponto/calculador/alimentacao.py:487`, `app/ponto/calculador/alimentacao.py:392`, `app/ponto/services/fechamento.py:711`, `app/folha/export.py:369`, `app/core/feriados.py:113`, `app/ponto/motor_calculo_v2.py:905`, `app/ponto/motor_calculo_v2.py:3495`)

**2) Mutações concretas que sobreviveriam às duas classes**

São previsões por inspeção dos casos e asserções; execução dos mutantes: **[nao verificado]**.

| Mutação no trecho de produção | Por que os testes não a distinguem | [nao verificado]
|---|---|
| Em `508–509`, trocar os campos: `_cct_pro = …_cct.hora_reduzida_afastada` e `_cct_hr = …_cct.prorrogacao_pos5h`. (`app/ponto/calculador/alimentacao.py:508`) | A fábrica falsa fornece exclusivamente `(False, False)` e `(True, True)`. Os testes herdados verificam somente dobra e usam trabalho diurno. (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:156`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:185`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:191`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:87`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:98`) |
| Substituir a leitura de `_cct.feriado_em_dobra` por `regras.feriado_em_dobra_do_ciclo(getattr(_te_dia, 'tipo_ciclo', None))`. (`app/ponto/calculador/alimentacao.py:510`, `app/ponto/calculador/regras.py:298`) | Os casos exigem a política básica 12x36/24x48; não cadastram uma CCT concedendo dobra ao 12x36. A exceção que seria perdida existe na fábrica real. (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:59`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:104`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:116`, `app/core/regua_cct.py:477`) |
| Trocar `ini` por `fim` em `get_motor_cct`. (`app/ponto/calculador/alimentacao.py:392`) | O mock ignora `competencia`; os casos reais não montam convenções distintas por data/praça. (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:158`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:59`) |
| Trocar a chave do cache de `pk` para `tipo_ciclo`. (`app/ponto/calculador/alimentacao.py:389`) | Existe somente um template por ciclo nas fixtures. Dois templates do mesmo ciclo poderiam selecionar classes diferentes por base/horário e, consequentemente, ter causas de pontualidade diferentes. (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:66`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:70`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:143`, `app/ponto/motor_calculo_v2.py:3561`, `app/ponto/motor_calculo_v2.py:3571`, `app/ponto/calculador/regras.py:487`) |
| Quando `_cct is None`, substituir os três `None` por `False`, `False`, `True`. (`app/ponto/calculador/alimentacao.py:508`) | Nenhum caso provoca fábrica indisponível ou cadastro incompleto; a fábrica falsa sempre devolve um motor. Isso deixaria passar novamente a afirmação de valores por ausência de cadastro. (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:158`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:174`, `app/ponto/calculador/nucleo.py:344`) |

**3) Intermitente: nacional versus municipal**

Considere trabalho efetivo de **H horas**, iniciado no feriado, cadastro reconhecendo o feriado nacional e conjunto municipal aplicável fornecido à v3. O municipal abaixo não coincide com um nacional. Os resultados são deduções dos caminhos citados, não medições de produção. (`app/ponto/calculador/regras.py:347`, `app/ponto/motor_calculo_v2.py:2747`, `app/core/feriados.py:60`, `app/core/feriados.py:64`)

| Feriado | `horas_extras_100_feriado` v3 | V2 intermitente |
|---|---:|---:|
| Nacional | **H** — `app/ponto/calculador/regras.py:785` | **H** — `app/ponto/motor_calculo_v2.py:2750` |
| Municipal aplicável | **H** — `app/ponto/calculador/regras.py:785` | **0** — `app/ponto/motor_calculo_v2.py:3495`, `app/ponto/motor_calculo_v2.py:905`, `app/core/feriados.py:91` |

O caminho é:

- A fábrica escolhe `MotorIntermitente`. Como `CICLOS_FERIADO_SIMPLES` contém somente 12x36, o flag de dobra do intermitente fica verdadeiro. (`app/ponto/motor_calculo_v2.py:3563`, `app/ponto/motor_calculo_v2.py:3537`, `app/ponto/motor_calculo_v2.py:3607`)
- O construtor intermitente esvazia `self.feriados`, mas não desliga o flag nem substitui o método de consulta. (`app/ponto/motor_calculo_v2.py:3483`, `app/ponto/motor_calculo_v2.py:3491`)
- No v2, o conjunto vazio leva ao fallback sem município: nacional é reconhecido; municipal não. O cálculo herdado lança toda a duração em HE100 de feriado quando a dobra é confirmada. (`app/ponto/motor_calculo_v2.py:905`, `app/ponto/motor_calculo_v2.py:41`, `app/core/feriados.py:91`, `app/ponto/motor_calculo_v2.py:2747`)
- Na v3, o conjunto do chamador permanece disponível. O intermitente atravessa o ramo de dia sem previsão e chega à dobra normal, que lança todo o trabalhado. (`app/ponto/calculador/alimentacao.py:407`, `app/ponto/calculador/alimentacao.py:524`, `app/ponto/calculador/regras.py:736`, `app/ponto/calculador/regras.py:775`)

**Não cabe concluir “intermitente paga zero no nacional” pela docstring.** O registro vivo já denuncia expressamente o nacional que retorna pelo fallback e dobra. (`app/ponto/motor_calculo_v2.py:3487`, `app/core/juizes.py:437`)

Não foram fornecidas horas concretas de um intermitente: quantidade absoluta paga em produção **[nao verificado]**.

**4) Checklist das classes de reprovação**

| Classe | Aplicação ao diff |
|---|---|
| Segundo juiz duplicando registrado | **Não identifiquei juiz novo no trecho alterado:** ele delega à fábrica, aos leitores CCT/família e ao juiz do vínculo. Dobra e vínculo já têm autoridades registradas. (`app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:403`, `app/ponto/calculador/alimentacao.py:490`, `app/core/juizes.py:262`, `app/core/juizes.py:1255`) |
| Segundo escritor fora da porta | **Não no trecho de produção alterado:** escreve cache local e argumentos. A persistência continua pela mesma `lavrar`, com `rubricas=`. (`app/ponto/calculador/alimentacao.py:385`, `app/ponto/calculador/alimentacao.py:516`, `app/ponto/services/fechamento.py:1245`, `app/ponto/services/fechamento.py:1256`) |
| Relógio fora da borda | **Não introduzido nesse trecho.** Existe leitor externo da causa usando `localdate()` quando não recebe data; é preexistente. (`app/ponto/calculador/alimentacao.py:392`, `app/chamados/juizes.py:1530`) |
| Derivação paralela de fonte única | **Aplica-se à reconstrução dos insumos:** alimentação monta outro motor com `ini`, enquanto o motor dos períodos foi montado com `fatia_ini`; o calendário também é entregue separadamente do calendário efetivo desse motor. São divergências de alimentação, embora deleguem à mesma fábrica. (`app/ponto/calculador/alimentacao.py:392`, `app/ponto/services/fechamento.py:711`, `app/ponto/calculador/alimentacao.py:524`, `app/ponto/motor_calculo_v2.py:3495`) |
| Selo/RED sem mordida | **Mordida insuficiente**, demonstrada pelos mutantes da pergunta 2. O mock acopla os flags e ignora a competência; os casos adicionais usam períodos diurnos e calendário vazio. (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:156`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:158`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:166`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:177`) |
| Ordem de trava | Ordem efetivamente executada **[nao verificado]**. |
| Pouso antes do pré-requisito | A exigência documental é RED → fix → GREEN → suite; sua execução e ordem neste pouso **[nao verificado]**. (`CLAUDE.md:348`) | [nao verificado]

**5) Outros defeitos de correção**

**O principal defeito remanescente no helper novo é temporal e territorial.** Com a régua ativa, `get_motor_cct(c, ini, tipo_escala=_te_dia)` combina o ciclo de `k` com a praça e vigência resolvidas para `ini`. O cache por template perpetua essa combinação. (`app/ponto/calculador/alimentacao.py:389`, `app/ponto/calculador/alimentacao.py:392`, `app/core/regua_cct.py:458`, `app/core/regua_cct.py:329`, `app/core/regua_cct.py:261`)

Contraexemplo: dois vínculos na janela usam o mesmo template 12x36, mas praças com convenções diferentes; a segunda concede dobra. A alimentação reutiliza a CCT consultada para `ini`, enquanto o fechamento monta o motor da segunda fatia com `fatia_ini`. **Trocar apenas `ini` por `k` também não bastaria mantendo o cache somente por template.** (`app/core/regua_cct.py:366`, `app/core/regua_cct.py:477`, `app/ponto/calculador/alimentacao.py:389`, `app/ponto/services/fechamento.py:711`) Existência desse cadastro em produção: **[nao verificado]**.

Há ainda **um defeito preexistente no consumidor, alcançado pela mudança da dobra**: quando `_dobra > 0`, `do_dia` retorna antes do split noturno, das horas noturnas e da pontualidade. Com entradas completas, esses valores foram inicialmente preenchidos com zero; portanto um feriado noturno pode sair com adicional noturno zero afirmado. O v2 calcula as noturnas antes da dobra. (`app/ponto/calculador/regras.py:641`, `app/ponto/calculador/regras.py:779`, `app/ponto/calculador/regras.py:789`, `app/ponto/calculador/regras.py:815`, `app/ponto/calculador/regras.py:835`, `app/ponto/calculador/regras.py:846`, `app/ponto/motor_calculo_v2.py:2730`, `app/ponto/motor_calculo_v2.py:2747`)

Os testes deste diff não detectam esse retorno antecipado: os casos de dobra são diurnos e só conferem HE100 de feriado; os outros casos conferem argumentos da chamada. (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:87`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:98`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:179`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:185`)

## 31/10 12:54 · sessao 491ee7a9f492 · - · - · origem=Ronald (tmux)

Q1 — rastreamento consolidado, somente leitura.

- Os quatro valores passam agora por `_te_dia`, obtido do `tipo_escala` do vínculo julgado para o dia; permanecem o template declarado pelo chamador quando não há vínculo/template conhecido ou a consulta falha (`app/ponto/calculador/alimentacao.py:487`, `:490`, `:494`, `:498`). `_cct_de` fabrica motor pelo template, lê a causa de pontualidade e a CCT e guarda por `template.pk` (`app/ponto/calculador/alimentacao.py:389`, `:392`, `:403`). A fábrica recebe **`ini`, não o dia** (`app/ponto/calculador/alimentacao.py:392`).

| Insumo | Quem escreve/resolve | Leitores v3 |
|---|---|---|
| `prorrogacao_pos5h` | `get_motor_cct`: regime CLT com switch desligado usa regra legal por ciclo; ligado usa `regua_para`; `MotorBase` grava atributo, com resolução legal se não recebeu (`app/core/regua_cct.py:454`, `:466`; `app/ponto/motor_calculo_v2.py:708`, `:712`). `cct_do_motor` transcreve (`app/ponto/calculador/nucleo.py:348`); alimentação entrega (`app/ponto/calculador/alimentacao.py:508`, `:530`). | Guardas noturna/HE noturna, janela noturna e split HE (`app/ponto/calculador/regras.py:609`, `:627`, `:714`, `:824`, `:838`); `noturnas_do_dia` usa janela e contexto de intervalos (`:80`, `:85`). API de ensaio `periodo.noturno_no_par` lê a CCT fornecida pelo chamador (`app/ponto/calculador/periodo.py:508`). |
| `hora_reduzida_afastada` | Fábrica usa cláusula cadastrada, restrita a ciclo 12x36 e trava; desligada prevalece default da fábrica/motor (`app/core/regua_cct.py:470`; `app/ponto/motor_calculo_v2.py:3540`, `:719`). Fronteira transcreve (`app/ponto/calculador/nucleo.py:349`); alimentação entrega (`app/ponto/calculador/alimentacao.py:509`, `:530`). | Guarda e fator das noturnas (`app/ponto/calculador/regras.py:609`, `:715`, `:839`, `:88`); ensaio `periodo.noturno_no_par` (`app/ponto/calculador/periodo.py:509`). |
| `feriado_em_dobra` | `get_motor` resolve por ciclo fora de `CICLOS_FERIADO_SIMPLES`; `get_motor_cct` pode conceder dobra ao 12x36 por cláusula (`app/ponto/motor_calculo_v2.py:3607`; `app/core/regua_cct.py:477`). Motor grava, fronteira transcreve e alimentação entrega (`app/ponto/motor_calculo_v2.py:737`; `app/ponto/calculador/nucleo.py:351`; `app/ponto/calculador/alimentacao.py:510`, `:524`). | Guarda de calendário/cadastro, dobra e contexto emprestando juiz do motor (`app/ponto/calculador/regras.py:633`, `:775`, `:777`, `:295`, `:347`); ensaio usa `cct` fornecida (`app/ponto/calculador/periodo.py:830`). Existe ainda helper que deriva novamente o mesmo predicado da lista do motor (`app/ponto/calculador/regras.py:298`, `:306`); não encontrei chamador de produção [nao verificado]. |
| `pontualidade_fora_por_lei` | Tabela `NAO_JULGA_PONTUALIDADE`, chave por identidade da classe; leitor `motivo_nao_julga_pontualidade` (`app/ponto/calculador/regras.py:487`, `:502`, `:518`). Alimentação pergunta sobre o novo motor e entrega a causa (`app/ponto/calculador/alimentacao.py:403`, `:531`). | `do_dia` declara atraso/antecipada e remove pontualidade quando existe causa (`app/ponto/calculador/regras.py:618`). Ensaio recebe `fora_por_lei` do chamador e aplica ao dia (`app/ponto/calculador/periodo.py:540`, `:787`). |
| `feriados` | **Continua vindo do chamador**: convertido em set e entregue diretamente; `_cct.feriados` não é usado (`app/ponto/calculador/alimentacao.py:407`, `:524`). Fechamento monta pelo município do posto atual do colaborador; espelho faz o mesmo (`app/ponto/services/fechamento.py:621`; `app/ponto/services/espelho.py:146`; `app/core/feriados.py:113`). | Alimentação usa também para `folga_e_feriado` (`app/ponto/calculador/alimentacao.py:523`); regra guarda e calcula dobra (`app/ponto/calculador/regras.py:633`, `:776`), contexto copia (`:294`). Ensaio usa o conjunto da CCT fornecida (`app/ponto/calculador/periodo.py:830`). |

Leitores fora v3:

- Prorrogação: `calcular_noturnas`, split HE e desconto noturno de intervalos (`app/ponto/motor_calculo_v2.py:922`, `:955`, `:1866`). Hora reduzida: propriedade `fator_hora_noturna` (`app/ponto/motor_calculo_v2.py:930`).
- Dobra: juiz `_feriado_dobra_no_dia` e ramo de alerta (`app/ponto/motor_calculo_v2.py:879`, `:2756`); os quatro ramos de pagamento perguntam ao mesmo juiz (`:2747`, `:3030`, `:3193`, `:3275`).
- Calendário: `eh_feriado` usa o conjunto injetado e, se a data não consta, cai em `is_feriado(data)` **sem município**, que consulta o juiz nacional (`app/ponto/motor_calculo_v2.py:903`, `:905`, `:41`; `app/core/feriados.py:91`). Outros consumidores são bloqueio de HE, ramos diários e exclusão da soma semanal (`app/ponto/motor_calculo_v2.py:1387`, `:2734`, `:3020`, `:3095`, `:3189`, `:3268`).
- Carimbo lê os três flags **do motor que calculou**, via `impressao_da_regua`; usado por fechamento, espelho e PDF (`app/core/regua_cct.py:423`; `app/ponto/services/fechamento.py:264`; `app/ponto/services/espelho.py:1185`; `app/relatorios/pdf_espelho.py:435`).
- Exportação relê a cláusula correlata `feriado_12x36_em_dobra` na **data final da janela**, e usa ciclo/cláusula para suprimir feriado no fechamento inteiro (`app/folha/export.py:359`, `:369`, `:411`; consumidores `:713`, `:978`).
- Frase de pontualidade da tela pergunta ao mesmo leitor, mas pode usar template passado pelo chamador; sem template resolve vínculo da data; sem data lê relógio e resolve vínculo de hoje; não fornece mapa de células (`app/chamados/juizes.py:1530`, `:1539`, `:1543`, `:1545`, `:1551`).

Divergências concretas de fonte:

1. **Ciclo do dia + CCT do início da janela**: nova fábrica recebe `_te_dia`, mas `ini`; `regua_para` resolve vínculo/praça e aplicação vigente pela data recebida (`app/ponto/calculador/alimentacao.py:392`; `app/core/regua_cct.py:329`, `:330`, `:366`). V2 fechamento e espelho usam `fatia_ini` (`app/ponto/services/fechamento.py:711`; `app/ponto/services/espelho.py:1115`). Cache somente por template não distingue vigência/posto (`app/ponto/calculador/alimentacao.py:389`).
2. **Calendário do chamador versus calendário efetivo do motor**: alimentação ignora `_cct.feriados` e usa argumento; motor intermitente limpa seu conjunto (`app/ponto/calculador/alimentacao.py:407`, `:524`; `app/ponto/motor_calculo_v2.py:3495`). Isso diverge especialmente no feriado municipal, pois fallback sem município só reconhece nacional (`app/ponto/motor_calculo_v2.py:905`; `app/core/feriados.py:91`).
3. **Calendário pelo posto atual**, não posto do vínculo do dia: `lugar_do_colab` lê `colaborador.posto` (`app/core/feriados.py:113`); fechamento/espelho usam esse lugar para a janela inteira (`app/ponto/services/fechamento.py:622`; `app/ponto/services/espelho.py:147`).
4. `periodo.reler` é API de ensaio, recebe CCT e causa únicas do chamador; não resolve dia a dia (`app/ponto/calculador/periodo.py:540`, `:563`, `:581`). Não há evidência de chamador de produção [nao verificado]. `Autoridade.motor` é o motor da última fatia, explicitamente documentado e retornado (`app/ponto/services/espelho.py:1192`, `:1195`); não afirmar que `periodo.reler` o usa em produção [nao verificado].

## 31/10 12:53 · sessao 78d2b4c76dcd · - · - · origem=Ronald (tmux)

Q3: Seja `H` o trabalho efetivo do dia, em horas, com jornada iniciada nesse dia e calendário municipal correto fornecido à v3. O código resulta em:

| Feriado | v3 (`rubricas_por_dia` → `regras.do_dia`) | v2 `MotorIntermitente` |
|---|---:|---:|
| Nacional reconhecido no cadastro | `H` | `H` |
| Municipal aplicável, sem nacional coincidente | `H` | `0` |

Fundamentação:

- A fábrica escolhe `MotorIntermitente` pelo ciclo/base (`app/ponto/motor_calculo_v2.py:3563`); intermitente está fora de `CICLOS_FERIADO_SIMPLES={'12x36'}`, logo `feriado_em_dobra=True` (`app/ponto/motor_calculo_v2.py:3537`, `app/ponto/motor_calculo_v2.py:3607`).
- O intermitente **só esvazia o conjunto** `self.feriados`; não desativa `feriado_em_dobra` nem sobrescreve `eh_feriado` (`app/ponto/motor_calculo_v2.py:3483`, `app/ponto/motor_calculo_v2.py:3491`).
- Após o esvaziamento, `MotorBase.eh_feriado` cai em `is_feriado(data)` (`app/ponto/motor_calculo_v2.py:901`), que chama o juiz sem município (`app/ponto/motor_calculo_v2.py:40`); nesse caso, somente nacionais alcançam a pessoa (`app/core/feriados.py:91`). Portanto nacional continua dobrando e municipal desaparece no v2.
- A implementação herdada de `Motor12x36.calcular_periodo` computa duração efetiva e desconta intervalo (`app/ponto/motor_calculo_v2.py:2697`, `app/ponto/motor_calculo_v2.py:2712`), e escreve toda a duração em `minutos_extra_100_feriado` quando o juiz confirma dobra (`app/ponto/motor_calculo_v2.py:2747`).
- Na v3, o calendário do chamador não é substituído pelo conjunto zerado do motor intermitente (`app/ponto/calculador/alimentacao.py:407`, `app/ponto/calculador/alimentacao.py:524`). Intermitente segue para trabalho normal mesmo com `dia_de_trabalho=False` (`app/ponto/calculador/regras.py:736`), e recebe a dobra inteira (`app/ponto/calculador/regras.py:775`, `app/ponto/calculador/regras.py:785`). O contexto empresta o mesmo juiz do motor (`app/ponto/calculador/regras.py:290`), mas mantém o calendário injetado (`app/ponto/calculador/regras.py:294`).
- O registro vivo já denuncia explicitamente a armadilha do nacional intermitente (`app/core/juizes.py:437`). A docstring “sem feriado-100” não descreve o comportamento executável (`app/ponto/motor_calculo_v2.py:3487`).

Sem horas concretas dadas para intermitente, número absoluto pago em produção: **[nao verificado]**.

Q4:

- **Segundo juiz:** o trecho novo delega às autoridades existentes: fábrica (`app/ponto/calculador/alimentacao.py:392`), leitor CCT e lei da família (`app/ponto/calculador/alimentacao.py:403`), vínculo-do-dia (`app/ponto/calculador/alimentacao.py:490`). O juiz do vínculo está registrado (`app/core/juizes.py:1255`); feriado e dobra estão registrados (`app/core/juizes.py:162`, `app/core/juizes.py:262`). Não localizei duplicação nova da decisão nessas linhas.
- **Segundo escritor persistente, relógio, ordem de travas:** o trecho alterado mantém cache local e argumentos, sem gravação persistente, `now` ou locks (`app/ponto/calculador/alimentacao.py:385`, `app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:507`). Não há evidência dessas classes no trecho novo.
- **Selo insuficiente:** a nova classe espiona argumentos e fabrica flags por template (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:156`, `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:174`), com períodos apenas diurnos (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:166`) e `feriados=set()` (`app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:177`). Não cobre os efeitos noturnos nem dobra de intermitente. Execução RED/GREEN/suite: **[nao verificado]**.
- **Pouso antes de pré-requisito:** normas pedem RED → fix → GREEN → suite (`CLAUDE.md:348`) e bateria pela regra/propriedades (`CLAUDE.md:52`); sua execução/ordem efetiva é **[nao verificado]**. [nao verificado]

Q5 / fonte temporal incompleta:

O helper novo recebe somente template, chaveia por `pk`, e consulta sempre `ini`, não `k` (`app/ponto/calculador/alimentacao.py:387`, `app/ponto/calculador/alimentacao.py:389`, `app/ponto/calculador/alimentacao.py:392`). Porém a régua determina praça pelo vínculo **na data recebida** (`app/core/regua_cct.py:329`), e CCT vigente por essa data (`app/core/regua_cct.py:261`, `app/core/regua_cct.py:366`).

Contraexemplo estático: dois vínculos na janela com mesmo template 12x36, postos/praças diferentes e aplicações de convenção diferentes; uma concede dobra e outra não. A v3 utiliza a praça e vigência de `ini` para ambos, enquanto o fechamento monta motor com `fatia_ini` (`app/ponto/services/fechamento.py:699`, `app/ponto/services/fechamento.py:711`). A dobra concedida por CCT é aplicada pela fábrica (`app/core/regua_cct.py:477`). A alteração acerta o ciclo, mas não torna toda a CCT “do dia”. Existência desse cadastro em produção: **[nao verificado]**.

_1039 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
