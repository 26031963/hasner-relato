# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 00:12** · publicado em **31/10/2026 00:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 00:12 · sessao 308ea00e1d2e · - · - · origem=Ronald (tmux)

A constante inclui explicitamente `'cobranca_indevida'` entre os vereditos acusadores; portanto os consumidores de `VEREDITOS_ACUSAM` tratam esse valor como acusação. (`app/escala/models.py:1735`)

| Leitor | Decisão |
|---|---|
| `folha/export.py::motivos_retencao_celula` | Produz `espelho_cobranca_indevida`, salvo protestos integralmente não cobráveis ou isenção por pausa. (`app/folha/export.py:1215`, `app/folha/export.py:1235`) |
| `folha/export.py::classificar_export` | Motivos do retentor colocam colaborador `fora`, por `furo_espelho`. (`app/folha/export.py:942`) |
| `folha/fora_da_folha.py::_dias_que_retem` | Delega ao retentor com `com_dias=True`. (`app/folha/fora_da_folha.py:93`) |
| `folha/fora_da_folha.py::fora_da_folha` | Usa dias retidos para agrupar colaboradores pelo fio que falta. (`app/folha/fora_da_folha.py:139`, `app/folha/fora_da_folha.py:154`) |
| `folha/fora_da_folha.py::o_que_segura` | Expõe dias retidos, veredito, fio e ação correspondente. (`app/folha/fora_da_folha.py:193`, `app/folha/fora_da_folha.py:209`) |
| `folha/previa.py::_cobre_a_retencao` | A proposta cobre a retenção somente se os dias retidos forem não vazios e estiverem todos nos propostos. (`app/folha/previa.py:169`) |
| `PerguntaDisputa.q_vivas` | Inclui célula acusadora na condição SQL de pergunta viva, respeitando validação, tranca e identidade. (`app/chamados/models.py:2578`, `app/chamados/models.py:2581`) |
| `chamados/juizes.py::_pergunta_viva_calc` | Retorna `True` ao chegar ao degrau de célula com veredito acusador. (`app/chamados/juizes.py:1054`) |
| `chamados/juizes.py::_resposta_concorda_com_a_luz` | Mesmo havendo concordância pela régua, só retorna concordância resolutiva se o dia não acusa. (`app/chamados/juizes.py:1000`) |
| `chamados/reconciliador.py::retratar_perguntas_absolvidas_por_celula` | Exclui perguntas de células acusadoras da seleção para carimbar `celula_absolveu`. (`app/chamados/reconciliador.py:598`, `app/chamados/reconciliador.py:619`) |
| `chamados/reconciliador.py::_absolvido_pela_celula` | Não considera absolvido chamado cujo dia tem veredito acusador; também exige ausência de marcos pendentes. (`app/chamados/reconciliador.py:978`) |
| `chamados/services/disputa_emissao.py::motivo_fora_da_fabrica` | Pergunta anteriormente materializada só exclui o chamado da fábrica se a célula não acusa ou está ausente. (`app/chamados/services/disputa_emissao.py:695`, `app/chamados/services/disputa_emissao.py:705`) |
| `chamados/services/limbo_folgas.py::filhos_no_defeito` | Filho com célula absolvente não segura o pai; célula acusadora ainda passa pelos testes de defeito e fio vivo. (`app/chamados/services/limbo_folgas.py:194`, `app/chamados/services/limbo_folgas.py:199`) |
| `chamados/services/resposta_tardia.py::receber` | Tenta renascer chamado encerrado se a célula acusa; conseguindo, devolve `reaberto`. (`app/chamados/services/resposta_tardia.py:45`) |
| `chamados/services/silencio_ausencia.py::_premissa_morre` | Para os módulos examinados, a cobrança só morre por ausência se o veredito não acusa. (`app/chamados/services/silencio_ausencia.py:34`, `app/chamados/services/silencio_ausencia.py:41`) |
| `chamados/services/fabrica_da_celula.py::motivos_a_perguntar` | Recusa veredito não acusador; antes disso já recusa folga, colaborador inativo e dia futuro. (`app/chamados/services/fabrica_da_celula.py:78`, `app/chamados/services/fabrica_da_celula.py:86`) |
| `chamados/services/vigia_fio.py::censo` | Seleciona células acusadoras apontadas por chamados vivos para medir fio morto. (`app/chamados/services/vigia_fio.py:182`, `app/chamados/services/vigia_fio.py:228`) |
| `ponto/services/vigia_ausencia_veredito.py::censo` | Conta discordância de célula acusadora, exceto `fato_em_ausencia`, com ausência aprovada integral. (`app/ponto/services/vigia_ausencia_veredito.py:41`, `app/ponto/services/vigia_ausencia_veredito.py:51`) |
| `folha/services/vigia_pausa.py::_dias_isentos` | Conta células acusadoras isentas pela regra de pausa. (`app/folha/services/vigia_pausa.py:155`) |
| `relatorios/dia_da_frota.py::dia_da_frota` | Pula dias sem trabalho; nos demais inclui o valor em `acusam`, rotulando “cobranca indevida”. (`app/relatorios/dia_da_frota.py:11`, `app/relatorios/dia_da_frota.py:47`, `app/relatorios/dia_da_frota.py:55`) |
| `relatorios/furos.py::estado_do_dia` | Traduz o valor para `parcial`, preservando os índices furados. (`app/relatorios/furos.py:26`, `app/relatorios/furos.py:45`) |
| `colaboradores/services/calendario.py::_classificar` | Mapeia o valor para `ok`; não aplica esse `ok` sem batida, salvo isento, nem substitui `previsto`, `aberto`, `folga` ou `folga_trabalhada`. (`app/colaboradores/services/calendario.py:72`, `app/colaboradores/services/calendario.py:171`) |
| `ponto/marcos.py::celula_veta_cobranca` | Veta por `FOLGA` quando `trabalha=False`; nos demais casos esse veredito retorna `COBRANCA_INDEVIDA` como motivo de veto. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:155`) |
| `disparar_perguntas_competencia.Command.handle` | Permite célula acusadora e pula veredito não acusador, além dos filtros de trabalho, atividade e motivos vencidos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/chamados/management/commands/disparar_perguntas_competencia.py:120`) |
| `ausencias_sem_efeito_na_celula.classificar` | Conta ausência integral abonada cuja célula acusa, exceto `fato_em_ausencia`, ou cuja ata tem lâmpada explicitamente apagada. (`app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:48`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:62`) |
| `auditar_invariantes_chamados._contraprova_f1t` | Alarma quando o piso F1t contém célula que não acusa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:197`) |
| `auditar_invariantes_chamados.contraprova_piso` | Exclui célula acusadora, folga ou dia com movimento da denúncia de absolvição falsa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:342`, `app/chamados/management/commands/auditar_invariantes_chamados.py:375`) |
| `auditar_invariantes_chamados.Command.handle`, A4 | Só mantém candidato à denúncia de encerrado com premissa viva quando o dia trabalha e acusa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1387`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1433`) |
| Mesmo auditor, C2 | Denuncia pergunta sem materialização, validação ou resposta cuja célula tem veredito não acusador. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1597`) |
| Mesmo auditor, F1t | Classifica como piso “cobrança com admin” perguntas mortas por via nomeada com célula ainda acusadora. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1703`) |

Em `ponto/services/espelho.py`, `_status_dia` decide por ausência, inconsistência, turnos/batidas, isenção, folga e previsto; esse corpo não lê o veredito da célula. (`app/ponto/services/espelho.py:152`) `aplicar_palavra_do_dia` delega para `dia_decidido.do_dia`, usando fatos, decisão, cobertura e pendências. (`app/ponto/services/espelho.py:339`, `app/ponto/services/espelho.py:351`)

## 31/10 00:12 · sessao 2667dfc67a82 · - · - · origem=Ronald (tmux)

Censo complementar concluído:

- Chamadas diretas de produção ao método: `_julgar_colab_corpo`, nas linhas `1000` e `1455`, e `veredito_celula`, na linha `330`; o método do modelo delega para a porta. A atribuição material aparece em `ponto.portas.celula.lavrar_veredito`. (`app/ponto/services/cartorio.py:1000`, `:1455`; `app/ponto/services/veredito_celula.py:330`; `app/escala/models.py:1782`; `app/ponto/portas/celula.py:692`).
- Só a chamada principal do cartório fornece códigos que podem gerar `cobranca_indevida`; a chamada do ramo sem adesão fornece `chamados_vivos=0`, e o admin aceita somente `trabalhou/faltou/justificado`. (`app/ponto/services/cartorio.py:985`, `:1149`, `:1455`; `app/ponto/services/veredito_celula.py:18`, `:201`; `app/ponto/supra_juiz.py:419`, `:442`).
- `celula_veredito_velho.Command.handle` é uma entrada de rejulgamento, sem escritor/derivador alternativo: `_espia_lavra` chama a porta original; o comando chama `julgar_colab(...apply_=True,forcar=True)`, mantendo ou revertendo a transação conforme `--aplicar`. (`app/ponto/management/commands/celula_veredito_velho.py:121`, `:122`, `:144`, `:145`, `:156`, `:157`, `:161`).
- `supra_juiz.Command.handle` identifica os dois códigos, mas sua ação explícita é `_retr(_ch, conteudo=motivo_retratacao(...))`, dirigida ao chamado; esse bloco não lavra `CelulaDia.veredito`. (`app/ponto/management/commands/supra_juiz.py:218`, `:243`, `:247`, `:248`).
- O selo estrutural existente percorre o app excluindo testes/migrações e verifica escritas por `update`, `bulk_update`, raw/SQL e saves fora da porta; sua asserção exige lista vazia. Não executei esse selo. (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:1`, `:3`, `:98`, `:155`, `:156`, `:164`, `:177`).

Na busca estática realizada em Python de produção de `app/`, não apareceu atribuição alternativa ao campo; a própria declaração do modelo nomeia `lavrar_veredito()` como escritor único. (`app/escala/models.py:1717`; `app/ponto/portas/celula.py:692`).

## 31/10 00:12 · sessao 72af1ce290cc · - · - · origem=Ronald (tmux)

Para o cenário que efetivamente contém perguntas **vivas pelo C6**, em competência aberta e sem outro chamado apto a recebê-las, o ciclo **não converge por repetição do julgamento**: `cobranca_indevida` pertence a `VEREDITOS_ACUSAM`, portanto sustenta a vitalidade da pergunta e fica excluído de E2 (`app/escala/models.py:1735`; `app/chamados/juizes.py:1054-1057`; `app/chamados/reconciliador.py:595-599`).

Ordem observada para FOLGA, contexto somente `data_turno`:

1. `julgar_celula` permite a FOLGA de hoje sem esperar fim de turno e chama `julgar_colab`; isso pressupõe data não futura e célula existente (`app/ponto/services/cartorio.py:1591-1607`).
2. Primeira passagem recebe `adiar_pela_lampada=True` (`app/ponto/services/cartorio.py:744`).
3. O corpo lê chamados vivos e fio vivo, calcula a impressão e pode pular julgamento se impressão igual, veredito presente e sem `forcar` (`app/ponto/services/cartorio.py:1088-1113`).
4. Monta a ata deste julgamento e a disponibiliza em `_tl.atas` **antes** de classificar e tentar reconciliar (`app/ponto/services/cartorio.py:1132-1136`).
5. Sem `marcos_faltantes`, `_marcos_cobrados` devolve lista vazia e `marcos_acesos` devolve `None`; consequentemente o chamado fica em `_protestam`, porque a condição exclui apenas `is True` (`app/ponto/services/lastro.py:74-79`, `app/ponto/services/lastro.py:170-172`; `app/ponto/services/cartorio.py:1147`).
6. `classificar_dia` recebe `chamados_vivos=len(_protestam)` e, se tipo não trabalho e não sem jurisdição, previsto zero e cobertura `None`, emite `COBRANCA_EM_DIA_SEM_TRABALHO` (`app/ponto/services/cartorio.py:1148-1158`; `app/ponto/supra_juiz.py:441-445`).
7. O chamado sem marcos **não** entra em `_adiados_pela_lampada`, nem na reconciliação por marcos acesos, porque ambas exigem `marcos_acesos is True`; também não entra em `marcos_inexistentes`, que recusa contexto sem marcos (`app/ponto/services/cartorio.py:1204-1208`, `app/ponto/services/cartorio.py:1219-1221`; `app/ponto/services/lastro.py:110-114`).
8. O ramo `COBRANCA_*` tenta `reconciliar()` **antes de lavrar** o veredito (`app/ponto/services/cartorio.py:1234-1245`, `app/ponto/services/cartorio.py:1452-1471`).
9. `reconciliar` pede `_decisao_do_fechamento`; primeiro roda `_gate_premissa_do_fechamento`, depois `_gate_pergunta_viva` (`app/chamados/models.py:605-619`, `app/chamados/models.py:645-653`).
10. Para `batida_ausente` em FOLGA, a premissa deixa fechar: célula do dia sem trabalho e módulo que não começa com `turno_aberto` resulta `True` (`app/chamados/models.py:1220-1226`; `app/chamados/juizes.py:386-389`).
11. Quem recusa, nas condições do primeiro parágrafo, é **`_gate_pergunta_viva`**, motivo `pergunta_viva_na_competencia_aberta` (`app/chamados/models.py:391-412`). Ela lê perguntas não validadas ancoradas pelo `chamado_destino` ou, sem destino, pelo chamado da disputa; consulta se competência foi entregue e o C6, simulando o estado terminal em memória; tenta achar sobrevivente do mesmo colaborador-dia excluindo o próprio chamado e validar a costura (`app/chamados/models.py:327-350`, `app/chamados/models.py:355-375`). Sem sobrevivente válido, retorna plano de recusa e o fechamento não ocorre (`app/chamados/models.py:373-374`, `app/chamados/models.py:403-412`).
12. Depois da recusa, a lavra recebe `codigos=cods`, deriva `cobranca_indevida` pelo mapa e carimba célula/impressão (`app/ponto/services/cartorio.py:1452-1471`; `app/escala/models.py:1581`, `app/escala/models.py:1592-1599`).
13. Voltando a `julgar_colab`, E2 só é chamado se houver `_adiados_pela_lampada`; esse chamado sem marcos não foi adiado. Mesmo que outro chamado faça E2 rodar, o filtro exclui células com qualquer `VEREDITOS_ACUSAM`, incluindo `cobranca_indevida` (`app/ponto/services/cartorio.py:745-755`; `app/chamados/reconciliador.py:595-599`; `app/escala/models.py:1735`).
14. A recusa não coloca a célula em `rejulgar`: nesse ramo, o enfileiramento depende de algum `reconciliar()` devolver `True`. Segunda passagem só roda se `rejulgar` estiver preenchido; sobra da segunda é descartada com contador, sem terceira passagem por esse mecanismo (`app/ponto/services/cartorio.py:1242-1245`, `app/ponto/services/cartorio.py:761-772`).
15. Próxima chamada com mesmos insumos pode pular pela impressão; com `forcar`, repete classificação, recusa e a mesma lavra. Nenhum passo desse circuito remove a pergunta viva, pois o novo veredito continua acusando (`app/ponto/services/cartorio.py:1111-1113`, `app/ponto/services/cartorio.py:1147-1149`, `app/ponto/services/cartorio.py:1242-1245`; `app/chamados/juizes.py:1054-1057`).

**Ressalva indispensável:** “não respondida” não basta para garantir recusa. Pergunta validada, competência trancada e certas vias de identidade já podem produzir `False`; veredito anterior definitivo fora de `VEREDITOS_ACUSAM` também pode matá-la antes da tentativa de reconciliar (`app/chamados/juizes.py:1006-1016`, `app/chamados/juizes.py:1039-1045`, `app/chamados/juizes.py:1054-1065`). O gate também ignora competência entregue/indecidível e permite reata válida; logo o desfecho absoluto depende desses dados não especificados (`app/chamados/models.py:345-346`, `app/chamados/models.py:370-375`, `app/chamados/models.py:413-420`).

Para **COBRANCA_SEM_FURO com o mesmo contexto sem marcos**, o circuito é equivalente: exige trabalho, previsto positivo, realizado `>=` previsto, células não vazias e todas casadas, além de `chamados_vivos` (`app/ponto/supra_juiz.py:414-422`). Sem marcos nomeados o chamado continua em `_protestam` e não entra nos adiados; o ramo compartilhado tenta reconciliar antes da lavra e pode sofrer a mesma recusa F1; depois grava o mesmo `cobranca_indevida`, que E2 exclui (`app/ponto/services/lastro.py:170-172`; `app/ponto/services/cartorio.py:1147-1149`, `app/ponto/services/cartorio.py:1204-1208`, `app/ponto/services/cartorio.py:1234-1245`, `app/ponto/services/cartorio.py:1455`; `app/escala/models.py:1581`, `app/chamados/reconciliador.py:598`).

Nesse caso de trabalho, a premissa lê a ata deste julgamento; sem lâmpada apagada e sem órfãs que a prendam, `_celula_furada` retorna `False`, liberando a guarda da premissa, mas sem eliminar a guarda da pergunta viva (`app/chamados/juizes.py:373-383`, `app/chamados/juizes.py:433-446`; `app/chamados/models.py:1240-1242`, `app/chamados/models.py:646-653`).

Há um caminho distinto quando o chamado **nomeia marcos e todos estão acesos**: ele sai de `_protestam`; se tem fio vivo vai a `_adiados_pela_lampada`; a primeira passagem pode lavrar `concorde`, depois E2 carimba, então o chamado reconcilia e a célula segue à segunda passagem forçada (`app/ponto/services/cartorio.py:1147-1149`, `app/ponto/services/cartorio.py:1185-1186`, `app/ponto/services/cartorio.py:1204-1207`, `app/ponto/services/cartorio.py:745-765`; `app/escala/models.py:1594-1596`). Esse mecanismo exige marcos comprovadamente acesos; o contexto somente `data_turno` devolve `None` e não o percorre (`app/ponto/services/lastro.py:170-181`).

## 31/10 00:12 · sessao 2667dfc67a82 · - · - · origem=Ronald (tmux)

Q1 — referências e conclusões:

- **Escritor material único:** `ponto.portas.celula.lavrar_veredito` atribui `cel.veredito = v` e os metadados; com `save=True`, chama `salvar_e_emitir`, que salva os campos. O método `CelulaDia.lavrar_veredito` apenas delega. (`app/ponto/portas/celula.py:692`, `:693`, `:694`, `:696`, `:697`, `:634`, `:640`; `app/escala/models.py:1775`, `:1781`, `:1782`).
- **Orquestrador que pode produzir `cobranca_indevida`:** `_julgar_colab_corpo` calcula `cods`, deixa `_vd=None` quando há código e chama `cel.lavrar_veredito(via='cartorio', veredito=_vd, codigos=cods, save=False)`; `carimbar` persiste junto os campos devolvidos. (`app/ponto/services/cartorio.py:1148`, `:1159`, `:1161`, `:1162`, `:1455`, `:1470`; `app/ponto/portas/celula.py:726`, `:728`).
- **Entrada pública:** `julgar_celula` busca uma célula e chama `julgar_colab`; este chama `_julgar_colab_corpo`. (`app/ponto/services/cartorio.py:1593`, `:1606`, `:744`).
- **Admin não grava esse valor pela porta:** admin aceita somente `('trabalhou','faltou','justificado')`; a porta valida esse vocabulário. (`app/escala/models.py:1727`; `app/ponto/portas/celula.py:683`).
- **Ramo nunca bateu não emite os dois códigos:** chama `classificar_dia` com `chamados_vivos=0`, e ambos os códigos exigem valor verdadeiro nesse insumo; usa `_vd_nb` ou renomeia como `nunca_bateu`. (`app/ponto/services/cartorio.py:984`, `:985`, `:989`, `:999`, `:1000`; `app/ponto/supra_juiz.py:419`, `:442`).

**Derivação:**

- `classificar_dia` devolve tuplas `(codigo, detalhe)`; o cartório extrai somente os códigos. A porta usa `v = veredito or derivar_veredito(codigos or [])`. (`app/ponto/supra_juiz.py:185`; `app/ponto/services/cartorio.py:1148`; `app/ponto/portas/celula.py:678`).
- `derivar_veredito` transforma códigos em conjunto e retorna o primeiro grupo correspondente: `furo` precede `cobranca_indevida`, que precede `coberta`, `indefinida`, `discordante`, etc.; ambos `COBRANCA_EM_DIA_SEM_TRABALHO` e `COBRANCA_SEM_FURO` estão no grupo `cobranca_indevida`. Portanto, se também houver algum código do primeiro grupo, o resultado será `furo`. (`app/escala/models.py:1579`, `:1580`, `:1581`, `:1594`, `:1597`, `:1598`, `:1599`).
- Códigos vazios resultam em `concorde`; código sem correspondência resulta em `ValueError`. (`app/escala/models.py:1595`, `:1596`, `:1600`).

**Insumos efetivamente necessários:**

- A guarda global `isento_ponto` retorna lista vazia antes dos ramos. (`app/ponto/supra_juiz.py:195`).
- `COBRANCA_EM_DIA_SEM_TRABALHO`: `tipo_dia != 'trabalho'`, tipo fora de `SEM_JURISDICAO=('fora_vinculo','sem_celula')`, `minutos_previstos` falso/zero, `chamados_vivos` verdadeiro, `cobertura_status is None`; não exige encerramento do turno, quantidade realizada, fio vivo ou contagem de marcos. (`app/ponto/supra_juiz.py:46`, `:197`, `:198`, `:441`, `:442`, `:443`).
- `COBRANCA_SEM_FURO`: `tipo_dia=='trabalho'`, previsto verdadeiro, realizado `>=` previsto, `chamados_vivos` verdadeiro e `todas_casadas`; `todas_casadas` exige lista `celulas` não vazia e `n_casadas == len(celulas)`. Usa `dia['n_casadas']` se diferente de `None`, senão reconta `_celulas_com_hora`, que exige dict, não missing, não isenção e hora verdadeira. Não exige fio vivo ou turno encerrado. (`app/ponto/supra_juiz.py:58`, `:63`, `:64`, `:65`, `:197`, `:198`, `:199`, `:414`, `:415`, `:416`, `:417`, `:418`, `:419`, `:420`).

**Origem no cartório:**

- `dia`/`marcos`: batidas apuráveis da janela `[d0-1, d1+3)` alimentam `eu.montar_grade_prevista_periodo`; `marcos=g['marcos']`; cada dia é indexado por `ref-i`; no julgamento busca `dias_por_data[data]`. (`app/ponto/services/cartorio.py:849`, `:874`, `:875`, `:890`, `:891`, `:892`, `:893`, `:894`, `:896`, `:897`, `:1114`).
- `tipo_dia`, previsto, realizado e `celulas` são as chaves do dicionário do dia que `classificar_dia` lê. No modo por turno, a grade monta folga com previsto 0 e células `None`, ou trabalho com células pareadas e previsto do mapa; realizado vem de `realizado_do_dia`, sendo zero explícito quando `sem_turno`. (`app/ponto/supra_juiz.py:197`, `:198`, `:199`, `:414`; `app/escala/utils.py:1743`, `:1744`, `:1749`, `:1750`, `:1751`, `:1985`, `:1998`).
- O dispatcher usa builder por turno se `settings.GRADE_POR_TURNO` for verdadeiro, senão legacy; não assumir em relatório que a configuração em execução é verdadeira sem verificar. (`app/escala/utils.py:2024`, `:2026`, `:2027`, `:2029`).
- `cobertura_status`: `ausencia_cobre(c,data,para='cobranca')`, depois `getattr(cob,'status',None)`. (`app/ponto/services/cartorio.py:1060`, `:1151`).
- `isento_ponto`: `bool(c.isento_ponto)`. (`app/ponto/services/cartorio.py:1153`).
- No teto de hoje, o classificador recebe `_dia_ate_o_teto(...)`; fora dele recebe o dia original. (`app/ponto/services/cartorio.py:1125`, `:1149`).

**`chamados_vivos` — detalhe fundamental deste HASH:**

1. `chamados_que_falam_de_batida(c)` consulta **todos os chamados do colaborador** cujo módulo está em `FURO_MODULES`, com prefetch das perguntas; a query não restringe status ou intervalo de datas. (`app/ponto/services/cartorio.py:80`, `:85`, `:86`, `:1023`).
2. `FURO_MODULES = ('batida_ausente','turno_aberto','intervalo_pendente','entrada_bloqueada_turno_aberto')`. (`app/chamados/catalogo/furo_modulos.py:15`, `:16`).
3. Agrupa pelo dia retornado por `data_do_chamado(modulo, contexto_json)`; só inclui se esse dia for verdadeiro; para os quatro módulos a chave declarada é `data_turno`, lida diretamente do contexto. Portanto o chamado entra no julgamento do **mesmo dia de `contexto_json.data_turno`**, inclusive se a batida for noturna; não usa `criado_em` nem dia civil da batida. (`app/ponto/services/cartorio.py:93`, `:94`, `:95`, `:96`, `:1059`; `app/chamados/catalogo/modulos.py:105`, `:107`, `:111`, `:182`, `:219`, `:222`, `:225`).
4. Para a célula, `lista=chs.get(data.isoformat(),[])`; `vivos` filtra status em `VIVOS=('aberto','em_analise','registrado')`. (`app/ponto/services/cartorio.py:1059`, `:1088`; `app/chamados/catalogo/motor.py:24`).
5. O HASH **não passa simplesmente `len(vivos)`**: monta a ata deste julgamento, e `_protestam = [x for x in vivos if marcos_acesos(x,_ata_agora) is not True]`; passa `chamados_vivos=len(_protestam)`, `chamados_total=len(lista)` e, separadamente, `cobrancas_vivas=len(_com_fio)`. (`app/ponto/services/cartorio.py:1132`, `:1133`, `:1147`, `:1149`, `:1150`).
6. `_com_fio` usa o juiz `fio_vivo(x.perguntas_que_resolvem.all())`; não é o mesmo insumo que `chamados_vivos`. (`app/ponto/services/cartorio.py:1027`, `:1093`, `:1150`).
7. Chamado sem marcos nomeados retorna `None` em `marcos_acesos`, logo continua em `_protestam` por `is not True`. (`app/ponto/services/lastro.py:170`, `:171`, `:172`; `app/ponto/services/cartorio.py:1147`).

## 31/10 00:06 · sessao 67dcc465444f · V-FASE-DONO-AS-OF · verificador · origem=Code

**Reprovaria a obra:** a segunda lavra no mesmo dia pode desfazer a re-lavra histórica; os carimbos não cobrem todas as alterações consumidas pelo censo; e há leitores de fatos passados ainda presos à última lavra. (`app/escala/services/fase_12x36.py:91`, `app/escala/services/fase_12x36.py:174`, `app/escala/signals.py:108`, `app/chamados/services/fila_por_causa.py:174`)

Análise estática da cópia fornecida. Execução dos testes/mutantes, RED/GREEN anunciados e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

**1. Onde o diff falha**

- **Repetir no mesmo dia pode ressuscitar a lavra velha.** A primeira execução reabre outubro e grava sua cobertura e `itens_reabertos`. Sem fato novo, a segunda execução não reabre outubro; entretanto, `update_or_create` substitui o **mesmo snapshot diário**, agora sem aquela cobertura histórica. O leitor volta à lavra anterior que ainda cobre outubro, recuperando o resultado desatualizado. O teste repete somente no dia seguinte, quando nasce outro snapshot. (`app/escala/services/fase_12x36.py:91`, `app/escala/services/fase_12x36.py:171`, `app/escala/services/fase_12x36.py:186`, `app/ponto/management/commands/censo_fase_12x36.py:298`, `app/ponto/management/commands/censo_fase_12x36.py:303`, `app/escala/tests/test_vfase_dono_as_of.py:73`)

- **O gatilho não observa integralmente os insumos do censo.** Ele agrega três carimbos de células existentes; o censo também lê vínculo, âncora e template vivos, além de aceitar dias sem célula. Portanto, pode mudar o resultado sem aparecer carimbo naquela janela — caminhos detalhados na resposta 3. (`app/escala/services/fase_12x36.py:182`, `app/ponto/management/commands/censo_fase_12x36.py:156`, `app/ponto/management/commands/censo_fase_12x36.py:171`, `app/escala/alimentacao.py:253`)

- **“Última cobertura” tem duas implementações diferentes.** O leitor escolhe por **dia contido em intervalo**; a reabertura guarda a última ocorrência do **triplo exato empresa/início/fim**. Se mudar o corte da empresa, intervalos antigos e correntes podem sobrepor. O censo mede ambos separadamente, com paridades próprias; depois o leitor une `itens` e `itens_reabertos`, sem resolver resultados conflitantes do mesmo dia na mesma lavra. Isso permite que a janela volte a influenciar a classificação. (`app/escala/services/fase_12x36.py:114`, `app/escala/services/fase_12x36.py:137`, `app/escala/services/fase_12x36.py:173`, `app/ponto/janelas.py:36`, `app/ponto/management/commands/censo_fase_12x36.py:124`, `app/ponto/management/commands/censo_fase_12x36.py:175`, `app/escala/services/fase_12x36.py:144`)

- **Empresa atual e empresa do item não são reconciliadas.** `InsumosDoDono` consulta pela empresa **atual** do colaborador; o censo também agrupa colaboradores pela empresa atual. Entretanto, depois de selecionar uma lavra pela cobertura da empresa solicitada, o leitor aceita o item apenas por colaborador/data, ignorando `x['empresa']`. Com transferência ou cortes diferentes, a cobertura usada e a empresa do item podem divergir. Transferências concretas no dado fornecido: **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:164`, `app/ponto/management/commands/censo_fase_12x36.py:110`, `app/escala/services/fase_12x36.py:137`, `app/escala/services/fase_12x36.py:146`)

- **O universo pode desaparecer justamente quando o cadastro muda.** Só entram colaboradores que ainda possuem algum vínculo cujo template é 12x36; só suas empresas e células participam da reabertura. Alterar o ciclo do último template 12x36 pode excluir o colaborador do detector, deixando sua lavra histórica sem reconstrução. (`app/ponto/management/commands/censo_fase_12x36.py:104`, `app/ponto/management/commands/censo_fase_12x36.py:110`, `app/escala/services/fase_12x36.py:174`, `app/escala/services/fase_12x36.py:182`)

- **A leitura pode combinar versões diferentes.** Primeiro são lidos PK/cobertura; depois, o detalhe dessas PKs. Uma sobrescrita diária entre as consultas pode entregar cobertura de uma versão e itens de outra. Na escrita, a escolha das reaberturas, a medição e a publicação também são etapas separadas; o timestamp de início não prova quais commits ficaram visíveis durante cada leitura. (`app/escala/services/fase_12x36.py:134`, `app/escala/services/fase_12x36.py:142`, `app/escala/services/fase_12x36.py:91`, `app/ponto/management/commands/censo_fase_12x36.py:62`, `app/ponto/management/commands/censo_fase_12x36.py:116`, `app/ponto/management/commands/censo_fase_12x36.py:282`)

- **O as-of implementado é diário e sobrescrevível.** O filtro temporal usa `data_ref <= hoje`; o `agora` recebido por `InsumosDoDono` não limita a seleção da fase. A única linha daquele dia pode ser substituída, sem preservar a versão intradiária anterior. (`app/escala/services/fase_12x36.py:108`, `app/escala/services/fase_12x36.py:91`, `app/ponto/management/commands/e6_oraculo.py:130`, `app/ponto/management/commands/e6_oraculo.py:164`)

- **O pouso proposto não recompõe o passado inteiro.** Lavras antigas sem `janelas` são excluídas tanto da leitura quanto da descoberta de reaberturas. A primeira execução nova mede somente anterior+atual; dias anteriores a essas duas janelas continuam sem cobertura e sem candidato a reabertura, mesmo que seus fatos mudem. (`app/escala/services/fase_12x36.py:105`, `app/escala/services/fase_12x36.py:171`, `app/ponto/management/commands/censo_fase_12x36.py:81`)

**2. Quem escreve e quem lê**

A porta efetiva grava o JSON inteiro em `MetricaSnapshot`, chave `desvio_de_cadastro_12x36`, escopo `tenant/0`, identificado pela data. O censo monta o conteúdo e chama essa porta. (`app/escala/services/fase_12x36.py:76`, `app/escala/services/fase_12x36.py:91`, `app/ponto/management/commands/censo_fase_12x36.py:271`, `app/ponto/management/commands/censo_fase_12x36.py:282`)

| Campo | Escritor/origem | Leitores efetivos |
|---|---|---|
| `detalhe.janelas` | Censo monta correntes e reabertas. (`app/ponto/management/commands/censo_fase_12x36.py:298`) | Índice; leitor por dia; descoberta de reaberturas. (`app/escala/services/fase_12x36.py:110`, `app/escala/services/fase_12x36.py:137`, `app/escala/services/fase_12x36.py:172`) |
| `detalhe.agora` | `_agora = timezone.now()` na entrada; serializado no detalhe. (`app/ponto/management/commands/censo_fase_12x36.py:62`, `app/ponto/management/commands/censo_fase_12x36.py:302`) | Índice transporta; reabertura compara com carimbos. O leitor por dia descarta `_agora`. (`app/escala/services/fase_12x36.py:110`, `app/escala/services/fase_12x36.py:179`, `app/escala/services/fase_12x36.py:134`) |
| `detalhe.itens` | Censo, incluindo `dono_do_dia` perguntado ao juiz. (`app/ponto/management/commands/censo_fase_12x36.py:274`, `app/ponto/management/commands/censo_fase_12x36.py:291`) | As-of; C10; propositor. (`app/escala/services/fase_12x36.py:144`, `app/escala/services/cadastro_realidade.py:345`, `app/escala/services/propositor.py:492`) |
| `detalhe.itens_reabertos` | Censo, somente linhas reabertas em desvio. (`app/ponto/management/commands/censo_fase_12x36.py:246`, `app/ponto/management/commands/censo_fase_12x36.py:303`) | Classificação por dia no as-of; `ultimo_lavrado` também transporta o JSON inteiro. (`app/escala/services/fase_12x36.py:144`, `app/escala/services/fase_12x36.py:205`) |
| `calculado_em` | ORM, `auto_now=True`; não é o instante de início da medição. (`app/inteligencia/models.py:96`, `app/ponto/management/commands/censo_fase_12x36.py:62`) | Ordenação dos dois leitores; exposição como `medido_em`. (`app/escala/services/fase_12x36.py:109`, `app/escala/services/fase_12x36.py:200`, `app/escala/services/fase_12x36.py:207`) |
| `data_ref` | Porta recebe `_hoje`, derivado de `_agora`. (`app/ponto/management/commands/censo_fase_12x36.py:63`, `app/ponto/management/commands/censo_fase_12x36.py:307`, `app/escala/services/fase_12x36.py:92`) | Filtro/ordenação e metadados dos leitores; vigia e API. (`app/escala/services/fase_12x36.py:108`, `app/escala/services/fase_12x36.py:199`, `app/escala/services/fase_12x36.py:206`, `app/ponto/management/commands/vigiar_familia_escala.py:26`, `app/api/views_mensageria.py:1676`) |

| Carimbo | Escritores reais | Leitores |
|---|---|---|
| `julgada_em` | `carimbar`, usando o `agora` recebido pelo cartório; impressão igual permite não escrever. (`app/ponto/portas/celula.py:708`, `app/ponto/portas/celula.py:717`, `app/ponto/portas/celula.py:721`) | Novo gatilho; salto de julgamento do cartório; invalidação da lavra de perguntas. (`app/escala/services/fase_12x36.py:183`, `app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:1092`, `app/chamados/veredito_lavrado.py:153`) |
| `regenerada_em` | Regeneração do vínculo **e reversão de regeneração do dia**. (`app/ponto/portas/celula.py:262`, `app/ponto/portas/celula.py:536`) | Novo gatilho; detecção de fato novo; invalidação das perguntas; cobrança órfã. (`app/escala/services/fase_12x36.py:183`, `app/ponto/services/cartorio.py:285`, `app/chamados/veredito_lavrado.py:153`, `app/escala/services/regeneracao.py:80`) |
| `editada_em` | **Sem escritor de produção declarado na cópia**; o contrato registra expressamente essa ausência. A docstring nova atribui-lhe um produtor humano inexistente nesse catálogo. (`app/escala/tests/test_d3_guarda_da_linha_celula.py:50`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:69`, `app/escala/services/fase_12x36.py:160`) | Novo gatilho; detecção de fato novo; invalidação das perguntas. (`app/escala/services/fase_12x36.py:184`, `app/ponto/services/cartorio.py:285`, `app/chamados/veredito_lavrado.py:154`) |

Outros usos dos carimbos incluem cobrança órfã no gerador/supra-juiz/materialização, exibição e diagnósticos, backfill e vigias de integridade. (`app/ponto/management/commands/gerar_celulas.py:242`, `app/ponto/management/commands/supra_juiz.py:152`, `app/chamados/management/commands/materializar_passivo_validado.py:220`, `app/ponto/services/dia_do_colab.py:91`, `app/ponto/management/commands/celula_veredito_velho.py:109`, `app/ponto/management/commands/backfill_b5_insumos.py:61`, `app/ponto/management/commands/vigia_b5_ausencia.py:55`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1593`)

**Leitores de passado que ficaram fora:**

- **Fila por causa:** conhece a data do fato, mas lê `colabs_de_desvio` da última lavra e decide `grade_errada` apenas pelo colaborador. Um chamado antigo pode mudar de gaveta quando a janela anda; a classificação também pode alcançar dias que nunca foram itens de fase. (`app/chamados/services/fila_por_causa.py:161`, `app/chamados/services/fila_por_causa.py:174`, `app/chamados/services/fila_por_causa.py:199`, `app/chamados/services/fila_por_causa.py:220`)
- **Propositor com janela ampliada:** a CLI aceita `--janela` arbitrária, enquanto o gatilho de fase continua lendo somente `ultimo_lavrado().itens`. A justificativa de “28 dias dentro de anterior+atual” não cobre essa porta existente. (`app/escala/management/commands/propostas_de_escala.py:12`, `app/escala/management/commands/propostas_de_escala.py:29`, `app/escala/services/propositor.py:416`, `app/escala/services/propositor.py:492`)

O novo leitor chega a e6, teto/C9 e pautas discordantes por `InsumosDoDono`; ele fornece **dias de fase**, não o `dono_do_dia` armazenado: o dono é novamente composto com outros insumos. (`app/ponto/management/commands/e6_oraculo.py:164`, `app/ponto/management/commands/e6_oraculo.py:167`, `app/ponto/management/commands/e6_oraculo.py:310`, `app/ponto/services/teto_na_lista.py:72`, `app/ponto/management/commands/pautas_discordantes.py:82`)

**Leitores do “hoje”:** C10/propositor continuam em `itens`; fila/pauta continuam nos IDs; vigia/API continuam nos metadados e linhas correntes. As reabertas são retiradas dessas listas. Porém, na transição, `ultimo_lavrado` aceita legado com `itens` sem `janelas`, enquanto o as-of o exclui: C10 e dono podem discordar até sobre dias das janelas correntes. (`app/escala/services/cadastro_realidade.py:345`, `app/escala/services/propositor.py:492`, `app/chamados/services/fila_por_causa.py:174`, `app/escala/services/fase_12x36.py:219`, `app/ponto/management/commands/censo_fase_12x36.py:247`, `app/escala/services/fase_12x36.py:201`, `app/escala/services/fase_12x36.py:105`)

**3. Mudanças que podem alterar o censo sem mover os três carimbos**

**Não, os carimbos não cobrem tudo.** Estes são caminhos concretos da cópia; sua ocorrência em produção permanece **[nao verificado]**.

| Mudança | Caminho e buraco |
|---|---|
| **Âncora de vínculo antigo** | O censo usa a âncora viva. O signal regenera somente desde a janela atual; mudança para `None` retorna sem regenerar. Assim, o passado antigo pode mudar de previsto sem carimbo nele. (`app/ponto/management/commands/censo_fase_12x36.py:171`, `app/escala/services/propositor.py:218`, `app/escala/signals.py:108`, `app/escala/signals.py:114`) |
| **Invalidação de âncoras por `update`** | `invalidar_ancoras_incoerentes` escreve `None` por `QuerySet.update`, sem atravessar o signal de âncora; pode alcançar outros vínculos ativos além daquele regenerado pelo ato principal. (`app/colaboradores/services/vinculo.py:849`, `app/colaboradores/services/vinculo.py:852`, `app/colaboradores/services/vinculo.py:857`, `app/escala/utils.py:2049`) |
| **Reversão de vínculo por recusa** | A recusa chama `reverter_para_snapshot`: apaga vínculos e restaura tipo/vigência/ativa por `update`, sem regeneração nessa função. A exclusão pode zerar a geradora via `SET_NULL`; isso muda a escolha do vínculo sem carimbar a célula. (`app/chamados/services/acoes_chamado.py:141`, `app/colaboradores/services/vinculo.py:864`, `app/colaboradores/services/vinculo.py:874`, `app/escala/models.py:1660`, `app/escala/alimentacao.py:237`) |
| **Ciclo/geometria de template histórico** | A propagação alcança só vínculos ativos e desde a competência anterior. O censo consulta ciclo vivo; o pareador também recebe geometria viva. Vínculos encerrados e passado mais antigo podem mudar sem carimbo. (`app/escala/services/cadastro_tipo.py:334`, `app/escala/services/cadastro_tipo.py:337`, `app/ponto/management/commands/censo_fase_12x36.py:157`, `app/ponto/turnos.py:1889`) |
| **Correção retroativa com regeneração parcial ou barrada** | A correção muda o vínculo; a regeneração pode ter piso posterior ao trecho alterado, pular competência lavrada ou preservar célula humana. O censo continua lendo o vínculo alterado. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:225`, `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:251`, `app/ponto/management/commands/censo_fase_12x36.py:171`) |
| **Batida em janela sem células** | Inserção/exclusão tenta julgar, mas o cartório retorna se não houver célula. O censo mede pelo vínculo e pelas batidas; o gatilho não tem linha para agregar. O mesmo problema alcança retratação salva pelo receiver. (`app/ponto/signals.py:131`, `app/ponto/signals.py:138`, `app/ponto/services/cartorio.py:1547`, `app/escala/alimentacao.py:253`, `app/ponto/management/commands/censo_fase_12x36.py:172`, `app/escala/services/fase_12x36.py:182`) |
| **Batida movida para outra data distante** | A regularização externa altera `timestamp`; o receiver julga apenas a **nova** data e sua véspera. A janela de origem não recebe carimbo no evento, embora perca a batida; depende da varredura posterior. (`app/chamados/services/regularizacao_ext.py:149`, `app/chamados/services/regularizacao_ext.py:155`, `app/ponto/signals.py:108`, `app/ponto/signals.py:135`, `app/ponto/management/commands/processar_cartorio.py:49`) |
| **Nascimento de célula ou enriquecimento de DNA antigo** | O gerador cria célula ou preenche DNA nulo sem escrever os três carimbos. Isso pode mudar vínculo escolhido/geometria dos turnos. `gerada_em`, que registra o nascimento, ficou fora do gatilho. (`app/ponto/portas/celula.py:592`, `app/ponto/portas/celula.py:603`, `app/escala/alimentacao.py:237`, `app/ponto/turnos.py:1946`, `app/ponto/turnos.py:1973`, `app/escala/services/fase_12x36.py:183`) |
| **DNA apagado fora da vigência** | Esse ramo salva somente `trabalha`/`dna`. O julgamento posterior depende de `recalculo` e exclui vereditos humanos; portanto não há garantia de atualização de nenhum dos três carimbos pelo ato. (`app/ponto/portas/celula.py:231`, `app/ponto/portas/celula.py:233`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:365`) |

A impressão do cartório contém batidas e partes do DNA, mas não a âncora/vigência viva que o censo usa. Logo, mesmo executar o cartório não transforma automaticamente esses carimbos num rastreador completo dos insumos do censo. (`app/ponto/services/cartorio.py:323`, `app/ponto/services/cartorio.py:327`, `app/ponto/services/cartorio.py:1092`, `app/ponto/management/commands/censo_fase_12x36.py:156`, `app/ponto/management/commands/censo_fase_12x36.py:171`)

**4. Checklist das classes de reprovação**

| Classe | Aparece neste diff? |
|---|---|
| **Segundo juiz duplicando juiz registrado** | **Não identifiquei essa duplicação introduzida.** Fase continua julgada pelo censo; vínculo e janelas continuam delegados às autoridades existentes. `_cobre` pergunta cobertura da lavra, não cobertura de vínculo. (`app/ponto/management/commands/censo_fase_12x36.py:246`, `app/ponto/management/commands/censo_fase_12x36.py:156`, `app/ponto/management/commands/censo_fase_12x36.py:84`, `app/escala/services/fase_12x36.py:113`) |
| **Segundo escritor fora da porta** | **Não para essa chave.** O diff continua publicando por `lavrar`; o defeito é substituir a fotografia diária e perder cobertura histórica. (`app/ponto/management/commands/censo_fase_12x36.py:271`, `app/ponto/management/commands/censo_fase_12x36.py:282`, `app/escala/services/fase_12x36.py:91`) |
| **Relógio lido fora da borda** | **Nenhuma leitura nova de relógio no miolo adicionado.** O censo captura `_agora` na entrada. O problema temporal é tratar carimbo/início como prova de leitura e permitir sobrescrita, não um novo `now()` escondido. (`app/ponto/management/commands/censo_fase_12x36.py:62`, `app/escala/services/fase_12x36.py:179`, `app/escala/services/fase_12x36.py:186`, `app/escala/services/fase_12x36.py:91`) |
| **Derivação paralela de fonte única** | **Sim, na escolha da lavra cobridora:** por dia no leitor, por identidade exata da janela no gatilho. Elas divergem quando as coberturas sobrepõem. (`app/escala/services/fase_12x36.py:137`, `app/escala/services/fase_12x36.py:173`) |
| **Selo/RED sem mordida suficiente** | **Sim, cobertura insuficiente:** os testes deixam sobreviver retirada dos carimbos de cadastro e do filtro de empresa; também não repetem a re-lavra no mesmo dia. Mutantes abaixo; execução deles **[nao verificado]**. (`app/escala/tests/test_vfase_dono_as_of.py:50`, `app/escala/tests/test_vfase_dono_as_of.py:73`, `app/escala/tests/test_o261_v_fase_na_lista.py:52`) |
| **Ordem de trava** | **Inversão de aquisição de travas: [nao verificado].** Há uma falha comprovável de consistência entre seleção e leitura/publicação, que não deve ser chamada de deadlock. (`app/escala/services/fase_12x36.py:134`, `app/escala/services/fase_12x36.py:142`, `app/escala/services/fase_12x36.py:91`) |
| **Pouso antes do pré-requisito** | **Sim, no desenho:** o leitor novo exige cobertura que o legado não possui. Além disso, o gatilho passou a depender de julgamento, mas o cron continua declarando somente `reconciliar_vinculo`, com comentário dispensando o cartório. Horário nominal não garante conclusão. Pouso efetivamente realizado: **[nao verificado]**. (`app/escala/services/fase_12x36.py:105`, `app/escala/services/fase_12x36.py:183`, `app/config/crons.py:617`, `app/config/crons.py:620`) |

**5. Os testes mordem? Qual mutante cada um deixa verde?** [nao verificado]

**Mordem partes do defeito original; não sustentam a condição completa do aval.** Há seis testes novos, com uma empresa na fixture; a

[truncado em 20000 de 23568 caracteres]

## 31/10 00:06 · sessao f3688f19738e · DISJUNTOR-E-CLEAR-RAPIDO · verificador · origem=Code

Há motivos para reprovar esta FINAL: o quarto verificador pode escapar, o orçamento diário tem desvios, a chave pode ser contornada e há caminhos que negam ou travam com medida desconhecida. (`bin/disjuntor.py:749`, `bin/disjuntor.py:696`, `bin/disjuntor.py:706`, `bin/disjuntor.py:117`, `bin/disjuntor.py:173`)

**a) Falsos positivos e permissões indevidas**

| Ponto | Comportamento encontrado | Evidência |
|---|---|---|
| Guarda da chave | Nega `Read` da chave e `Bash` como `cat DISJUNTOR_DESLIGADO`. A condição verifica nome, sem distinguir consulta de alteração; também alcança outros arquivos com o mesmo prefixo. | `bin/disjuntor.py:117`, `bin/disjuntor.py:119` |
| Proteção incompleta | `Bash` pode sobrescrever `.claude/settings.json` sem citar a chave. `Edit` pode trocar apenas `pretool` por outro argumento: a guarda exige que `old_string` contenha `disjuntor.py`. Nome da chave montado por concatenação também escapa. | `bin/disjuntor.py:116`, `bin/disjuntor.py:127` |
| ESCADA | Nega `Agent`, mas `Write/Edit/Bash` da sessão principal normalmente saem antes da medição. Portanto, “sem obra nova” fica como orientação, sem impedir sua abertura pela própria sessão. | `bin/disjuntor.py:728`, `bin/disjuntor.py:744` |
| Palavra “escada” | Só reconhece o começo da última mensagem humana elegível; outra mensagem encerra a escada. “Agora use a escada”, por exemplo, não ativa. Essa restrição de posição/duração excede a definição “a palavra dele” fornecida. | `bin/disjuntor.py:64`, `bin/disjuntor.py:516`, `bin/disjuntor.py:529` |
| Quarto verificador | PreToolUse consulta apenas o estado acumulado. As três reprovações são descobertas no fim do Stop, depois de várias saídas antecipadas. Outro verificador lançado antes dessa varredura passa. | `bin/disjuntor.py:749`, `bin/hook_stop_fila1.py:1004`, `bin/hook_stop_fila1.py:1256` |
| Identificação de reprovação | Exige meta `verificador`, prompt com `LEITURA FINAL`, `obra <ID>` conforme regex e veredito no `SubagentHandback`. Outra grafia, canal ou verificação via Codex não recebe a mesma guarda. | `bin/disjuntor.py:71`, `bin/disjuntor.py:412`, `bin/disjuntor.py:419`, `bin/disjuntor.py:435`, `bin/disjuntor.py:747` |
| Rearme B3 | Guarda os IDs antigos em `rearmado`, mas a varredura seguinte ignora esse campo e recoloca as mesmas reprovações. Pode voltar a negar imediatamente depois do rearme. | `bin/disjuntor.py:443`, `bin/disjuntor.py:468` |
| Orçamento diário | Fora de `contrato`, só corta papel literal `verificador`. `censo`, `leitura`, `sonda` e papel não reconhecido passam, contrariando a formulação “só a volta FINAL usa Codex”. | `bin/disjuntor.py:702`, `bin/disjuntor.py:706` |
| ESCADA × orçamento | Com Codex >85% e escada ativa, retorna autorização **antes** de verificar contrato e orçamento diário. Assim, contrato e volta intermediária acima do orçamento também passam. | `bin/disjuntor.py:696`, `bin/disjuntor.py:702` |
| Interpretação de Bash | Trata texto de `echo`/comentário como chamada. `… CODEX_VOLTA=final bash bin/codex_pergunta.sh …` perde papel/volta no parser: FINAL com anexo pode ser negada; chamada intermediária sem anexo pode escapar. `codex exec` direto nem entra nessa fiscalização. | `bin/disjuntor.py:649`, `bin/disjuntor.py:659`, `bin/disjuntor.py:687`, `bin/disjuntor.py:728` |
| Trava de sombra | Ocupada ou ilegível, suspende inclusive B3, B4 e controle Codex; portanto deixa passar chamadas que os limiares mandariam negar. | `bin/disjuntor.py:237`, `bin/disjuntor.py:244`, `bin/disjuntor.py:730` |

A negativa de contrato apenas **manda** escrever `sem Codex: orcamento`; o registro automático no RELATO contém regra, ferramenta e alvo, sem garantir essa anotação na linha da obra. (`bin/disjuntor.py:703`, `bin/disjuntor.py:165`)

**b) Medidas ilegíveis, ausentes ou inesperadas**

- **Cota ausente, vencida ou JSON inválido:** vira `None`; sem palavra escada legível, deixa passar. Contudo, `NaN` é aceito como número e acaba negando agente com motivo `nan% >=75%`, pois tanto `NaN >=85` quanto `NaN <75` são falsos. (`bin/disjuntor.py:485`, `bin/disjuntor.py:526`, `bin/disjuntor.py:541`)
- **Statusline com estrutura inesperada:** `rate_limits="x"` ou `seven_day="x"` levanta fora do bloco protetor; `context_window="x"` também pode levantar. Sem atualização, a cota anterior continua válida até o reset; o leitor não verifica idade nem sessão. (`bin/statusline_cota.py:28`, `bin/statusline_cota.py:30`, `bin/statusline_cota.py:42`, `bin/disjuntor.py:485`)
- **Estado do disjuntor ilegível:** vira `{}`, perdendo memória de B1/B3. Estruturas internas erradas podem provocar exceção; no PreToolUse ela normalmente libera. Inclusive cota conhecida que mandaria negar pode passar se a gravação do estado falhar antes do retorno da negativa. (`bin/disjuntor.py:206`, `bin/disjuntor.py:224`, `bin/disjuntor.py:763`, `bin/disjuntor.py:780`)
- **Transcript B1 ilegível:** não dispara. Porém, **cópia ilegível é diferente**: erros de `scandir/stat` são ignorados, retornando “nenhum arquivo novo”; isso pode disparar B1 indevidamente. Meta ilegível também perde a isenção de agente somente leitura. (`bin/disjuntor.py:275`, `bin/disjuntor.py:305`, `bin/disjuntor.py:324`, `bin/disjuntor.py:329`, `bin/disjuntor.py:366`)
- **Transcript humano inválido:** linhas inválidas são puladas; uma mensagem anterior pode continuar determinando escada. Falha global de leitura retorna `None`. (`bin/disjuntor.py:501`, `bin/disjuntor.py:516`, `bin/disjuntor.py:518`)
- **Piso:** procura o primeiro assistente com tokens positivos, podendo pular o verdadeiro primeiro turno sem `usage`. Se não medir no primeiro Stop, consome `piso_pendente` e não tenta novamente naquele ciclo. (`bin/orcamento_contexto.py:265`, `bin/hook_stop_fila1.py:866`)

**Sim, há exceção que resulta em bloqueio:** falha do orçamento retorna ao caminho normal do Stop; como CLEAR está excluído do PAREI genérico, um par válido que normalmente liberaria pode terminar bloqueado até o TETO. Além disso, erro de `stat` em qualquer novo arquivo protegido invalida até uma prova ESPERANDO apresentada por outro arquivo legítimo. (`bin/hook_stop_fila1.py:977`, `bin/hook_stop_fila1.py:1080`, `bin/hook_stop_fila1.py:1260`, `bin/hook_stop_fila1.py:563`)

**Há travamento sem prazo no código:** o lock do RELATO usa `flock(LOCK_EX)` bloqueante; a coleta Codex usa `open` comum em `.jsonl`, podendo esperar em FIFO. A abertura protegida contra FIFO existe em `orcamento_contexto.abre`, mas não nessa coleta Codex. (`bin/disjuntor.py:173`, `bin/codex_limite_estado.py:141`, `bin/orcamento_contexto.py:74`)

**c) Escritores e leitores**

Os caminhos operacionais são `logs/sessao/*`. `logs/disjuntor/contrato.txt` aparece como referência documental; autor, conteúdo e consumidores desse contrato ficam **[nao verificado]**. (`bin/disjuntor.py:2`, `bin/disjuntor.py:47`)

| Arquivo | Quem escreve | Quem lê |
|---|---|---|
| `logs/sessao/disjuntor.log` | `disjuntor.trilha`, chamado pelos disparos/falhas/rearme. | `disjuntor.ler`, publicado pelo gerador de ESTADO. (`bin/disjuntor.py:85`, `bin/disjuntor.py:826`, `bin/gerar_estado.py:280`) |
| `disjuntor_estado.json` | `_com_estado`, usado por PreToolUse, Stop e CLI de rearme; temporário `.disjuntor_estado.*` seguido de replace. | `_com_estado`, `ler_estado` e decisões B1/B3/B4. (`bin/disjuntor.py:185`, `bin/disjuntor.py:209`, `bin/disjuntor.py:218`) |
| `disjuntor_estado.json.lock` | Criado/aberto por `_com_estado`. | Os mesmos participantes usam flock, com tentativas limitadas. (`bin/disjuntor.py:191`, `bin/disjuntor.py:193`) |
| `disjuntor_estado.json.relato.lock` | Criado/aberto por `relato_negacao`. | As negativas do disjuntor usam esse lock; o escritor antigo não o adquire. (`bin/disjuntor.py:171`, `app/core/esteira_vigia.py:830`) |
| `cota.json` | `statusline_cota`, por `.cota.*` e replace. | `disjuntor.ler_cota`, usado pelas decisões e ESTADO. (`bin/statusline_cota.py:34`, `bin/statusline_cota.py:39`, `bin/disjuntor.py:480`) |
| `pisos.log` | `registrar_piso`, acionado pelo Stop **e** por `--semear-pisos`. | `ultimo_piso`/`gatilho_tokens` e painel do disjuntor. (`bin/orcamento_contexto.py:198`, `bin/orcamento_contexto.py:309`, `bin/hook_stop_fila1.py:870`, `bin/disjuntor.py:869`) |
| `orcamento_estado.json` | Continua sendo `_orc_salva` do Stop, com temporário e replace. | Stop, `orcamento_contexto.ler` e `disjuntor.ler`. (`bin/hook_stop_fila1.py:722`, `bin/hook_stop_fila1.py:817`, `bin/orcamento_contexto.py:353`, `bin/disjuntor.py:850`) |
| `orcamento.log` | `_orc_trilha` do Stop. | Avaliador/painel de orçamento e painel do disjuntor. (`bin/hook_stop_fila1.py:699`, `bin/orcamento_contexto.py:359`, `bin/disjuntor.py:826`) |
| `ultimo_clear.json` e lock | `marco_de_clear.py`, preservando clears anteriores e usando replace. | Próprio escritor e `orcamento_contexto.ler_marco`. (`bin/marco_de_clear.py:38`, `bin/marco_de_clear.py:47`, `bin/marco_de_clear.py:61`, `bin/orcamento_contexto.py:152`) |
| `RELATO.md` e `.novo` | Novo chamador `relato_negacao` reutiliza `esteira_vigia.no_relato`. | Stop, handoff e publicação. (`bin/disjuntor.py:176`, `app/core/esteira_vigia.py:845`, `bin/hook_stop_fila1.py:408`, `bin/handoff_sessao.sh:88`, `bin/relato.sh:104`) |
| `DISJUNTOR_DESLIGADO` | O código operacional descrito apenas consulta; o manifesto atribui `touch/rm` ao dono. Exclusividade efetiva: **[nao verificado]**, pelas brechas da guarda. | `desligado`, no PreToolUse, Stop e painel. (`bin/disjuntor_settings.json:3`, `bin/disjuntor.py:103`, `bin/disjuntor.py:775`, `bin/hook_stop_fila1.py:994`) |

**Existe concorrência de escritores do RELATO.** Reutilizar a função não cria escritor único por processo: o vigia antigo continua chamando `no_relato` sem o lock novo; ambos usam o temporário fixo `RELATO.md.novo`. Há ainda `gerar_cortes.py`, que reescreve o RELATO diretamente. Isso permite perda de atualização; ocorrência em produção **[nao verificado]**. (`app/core/esteira_vigia.py:845`, `app/core/esteira_vigia.py:1129`, `bin/disjuntor.py:171`, `bin/gerar_cortes.py:136`)

A negativa é emitida **antes** de registrar no RELATO. RELATO ausente causa retorno silencioso; erro na gravação mantém a negativa e apenas tenta registrar FALHA. Portanto, “cada negação no RELATO” não está garantido. (`bin/disjuntor.py:169`, `bin/disjuntor.py:179`, `bin/disjuntor.py:778`)

Não aparece novo escritor direto do estado antigo de orçamento: muda o conteúdo que o mesmo Stop salva. O orçamento **Codex** é calculado das sessões, sem novo estado persistente próprio. (`bin/hook_stop_fila1.py:875`, `bin/codex_limite_estado.py:158`)

A configuração entregue aponta ao projeto; preservação da configuração de usuário efetivamente instalada fica **[nao verificado]**. (`.claude/settings.json:68`, `bin/disjuntor_settings.json:2`)

**d) Mudanças nas decisões antigas de Stop**

Sim; não se limita a acrescentar recados:

- CLEAR passa a exigir handoff novo, pode ser adiado por agente/trava e ganha um bloqueio adicional quando o handoff aparece. (`bin/hook_stop_fila1.py:882`, `bin/hook_stop_fila1.py:895`, `bin/hook_stop_fila1.py:935`)
- A validade cai para dez minutos; tempo e quantidade de deploys deixam de disparar, substituídos pelo limiar adaptativo de tokens. Esses comportamentos não constam dos requisitos transcritos; aval adicional **[nao verificado]**. (`bin/orcamento_contexto.py:56`, `bin/orcamento_contexto.py:225`, `bin/orcamento_contexto.py:335`)
- `PAREI ! COTA` perde a liberação genérica mesmo com disjuntor desligado ou sem escada ativa. (`bin/hook_stop_fila1.py:994`, `bin/hook_stop_fila1.py:1080`)
- B2 acrescenta liberação antecipada e conta **todo `BLOCK`**, incluindo orçamento, B4 e RELATO sem prova, em vez de identificar somente `siga` da fila. (`bin/disjuntor.py:591`, `bin/hook_stop_fila1.py:1156`, `bin/hook_stop_fila1.py:1179`, `bin/hook_stop_fila1.py:1251`)
- Sem o par COTA+handoff, B4 bloqueia uma vez e depois vira lembrete. O fluxo antigo pode então liberar por PAREI comum, ESPERANDO, fila vazia, TETO ou B2, sem comprovar fechamento do marco. (`bin/disjuntor.py:633`, `bin/hook_stop_fila1.py:1182`, `bin/hook_stop_fila1.py:1187`, `bin/hook_stop_fila1.py:1201`, `bin/hook_stop_fila1.py:1240`)

**e) Selos que não mordem**

| Mutação | Por que as asserções funcionais não a distinguem |
|---|---|
| `< B4_NEGA` → `<= B4_NEGA` | Muda exatamente 75%, mas as entradas são 74,9%, 80% e 90%. (`bin/disjuntor.py:541`, `bin/tests/test_disjuntor.sh:305`, `bin/tests/test_disjuntor.sh:310`, `bin/tests/test_disjuntor.sh:312`) |
| `>= B4_PARA` → `> B4_PARA` | Muda exatamente 85% Claude; o teste de escada por cota usa 90%. (`bin/disjuntor.py:526`, `bin/tests/test_disjuntor.sh:312`) |
| `agora - desde <= janela` → `< janela` | Muda o instante exato de dez minutos; os testes usam imediato, nove, onze e vinte minutos. (`bin/hook_stop_fila1.py:918`, `bin/tests/test_disjuntor.sh:159`, `bin/tests/test_disjuntor.sh:161`, `bin/tests/test_orcamento_contexto.sh:188`, `bin/tests/test_orcamento_contexto.sh:192`, `bin/tests/test_orcamento_contexto.sh:408`) |

As duas primeiras alterações também mexem em âncoras textuais do executor de mutantes: eventual falha por “âncora sumiu” não demonstra que o caso funcional detectou o erro. (`bin/tests/test_disjuntor.sh:463`, `bin/tests/test_disjuntor.sh:487`, `bin/tests/test_disjuntor.sh:492`)

Além disso, várias autorizações são verificadas apenas por stdout vazio, exatamente a saída de uma exceção capturada antes do deny. Esses casos não distinguem autorização correta de falha aberta. (`bin/tests/test_disjuntor.sh:365`, `bin/tests/test_disjuntor.sh:375`, `bin/disjuntor.py:780`)

Execução integral dos selos e resultado dos mutantes declarados: **[nao verificado]**; os scripts criam e removem arquivos, incompatíveis com a regra desta revisão. (`bin/tests/test_disjuntor.sh:35`, `bin/tests/test_disjuntor.sh:37`, `bin/tests/test_orcamento_contexto.sh:101`)

## 31/10 00:04 · sessao f3649a18e4f8 · - · - · origem=Ronald (tmux)

Achados para **(e)**, sem executar os selos que criam arquivos:

- **Mutante sobrevivente em 75%:** trocar `pct < B4_NEGA` por `pct <= B4_NEGA` deixa passar agente com exatamente 75%; o caso `b4` injeta 80%, 74,9%, 80% vencido e 90%, sem testar 75%. Confirmei em memória que os resultados dessas entradas permanecem iguais e somente 75% diverge. `bin/disjuntor.py:541`; `bin/tests/test_disjuntor.sh:305`; `bin/tests/test_disjuntor.sh:310`; `bin/tests/test_disjuntor.sh:312`.

- **Mutante sobrevivente em 85% Claude:** trocar `pct >= B4_PARA` por `pct > B4_PARA` impede escada em exatamente 85%; os testes usam 90% para escada por cota e 50% para escada pela palavra. Confirmei em memória igualdade nas entradas usadas e divergência em 85%. `bin/disjuntor.py:526`; `bin/tests/test_disjuntor.sh:312`; `bin/tests/test_disjuntor.sh:331`.

- **Fronteira de dez minutos sem mordida:** trocar `agora - desde <= janela` por `< janela` conserva os resultados testados em nove/onze minutos e muda exatamente dez minutos. O segundo selo testa liberação imediata, onze minutos e clear tardio em vinte minutos. `bin/hook_stop_fila1.py:918`; `bin/tests/test_disjuntor.sh:159`; `bin/tests/test_disjuntor.sh:161`; `bin/tests/test_orcamento_contexto.sh:188`; `bin/tests/test_orcamento_contexto.sh:192`; `bin/tests/test_orcamento_contexto.sh:408`.

- **“Passou” pode ser exceção:** diversas verificações de permissão exigem apenas stdout vazio; `main_pretool` também retorna zero e stdout vazio quando há exceção antes da negativa. Assim esses casos não distinguem autorização normal de falha aberta; para a FINAL, por exemplo, um mutante que levantasse exceção exclusivamente nessa volta continuaria satisfazendo a asserção de stdout. `bin/tests/test_disjuntor.sh:365`; `bin/tests/test_disjuntor.sh:375`; `bin/disjuntor.py:780`.

Os dois primeiros mutantes **não estão entre os declarados**, que testam outras costuras; execução integral e sobrevivência observada no shell ficam **[nao verificado]**. A lista declarada está em `bin/tests/test_disjuntor.sh:471`.

O caso de proteção de `pisos.log` parece morder a retirada **somente dessa proteção**: a primeira avaliação estabelece o mtime, o append o avança e a seguinte passaria a liberar ESPERANDO, contrariando a exigência de stdout não vazio. `bin/tests/test_disjuntor.sh:392`; `bin/tests/test_disjuntor.sh:393`; `bin/hook_stop_fila1.py:590`; `bin/hook_stop_fila1.py:610`; `bin/hook_stop_fila1.py:1187`.

## 31/10 00:03 · sessao c8c99e8e4f0c · - · - · origem=Ronald (tmux)

Achados prioritários, somente por código:

1. **Guarda da chave tem falsos positivos:** `Read` do arquivo da chave é negado como se gravasse; qualquer basename começando por `DISJUNTOR_DESLIG` também; `Bash` de leitura/documentação que apenas cite a substring é negado. A condição não distingue leitura de escrita. `bin/disjuntor.py:117-120`.

2. **A proteção pode ser contornada:** alterações do settings via `Bash` sem citar a chave não são inspecionadas; o teste de remoção do hook só considera `file_path` e `Write/Edit/MultiEdit`. Montar o nome da chave por concatenação também evita a substring literal. `bin/disjuntor.py:116-130`.

3. **ESCADA não impede obra nova pela sessão principal:** ferramentas sem `agent_id`, diferentes de `Agent` e sem `codex_pergunta` saem antes das medições; portanto um `Write/Edit/Bash` da sessão principal pode iniciar nova obra mesmo com cota ≥85%. A restrição fica no recado do Stop. `bin/disjuntor.py:727-731`, `bin/disjuntor.py:744-764`, `bin/disjuntor.py:633-634`.

4. **“Escada” tem semântica mais estreita que palavra em qualquer posição:** só a última mensagem humana elegível começando por `escada` ativa; outra mensagem a encerra. Texto ilegível na cauda é ignorado, podendo fazer uma mensagem anterior continuar valendo. `bin/disjuntor.py:64`, `bin/disjuntor.py:501-529`.

5. **B3 deixa passar o quarto verificador antes da varredura:** PreToolUse só lê estado acumulado; a descoberta dos três REPROVADO ocorre no ramo final de recados do Stop, depois de várias saídas que liberam. `bin/disjuntor.py:429-458`, `bin/disjuntor.py:747-749`, `bin/hook_stop_fila1.py:1004`, `bin/hook_stop_fila1.py:1256`.

6. **B3 depende de grafia e canal exatos:** só meta `agentType=verificador`, primeiro prompt contendo `LEITURA FINAL`, `obra <ID>` conforme regex e `SubagentHandback` com primeira linha REPROVADO. Uma quarta consulta via Codex não passa por essa guarda de `Agent`. `bin/disjuntor.py:71`, `bin/disjuntor.py:408-425`, `bin/disjuntor.py:435-439`, `bin/disjuntor.py:747-749`.

7. **Rearme B3 não dura:** `rearmado` recebe os IDs anteriores, mas a próxima varredura não consulta esse campo e adiciona novamente todos os REPROVADO antigos. `bin/disjuntor.py:443-446`, `bin/disjuntor.py:468-469`.

8. **Acima do orçamento diário, papéis diferentes de `verificador` escapam**, salvo a volta explicitamente marcada `contrato`; papel herdado do ambiente vira `?` no parser. Além disso, Codex >85% com ESCADA retorna `None` antes das restrições diárias, permitindo contrato/meio acima do orçamento. `bin/disjuntor.py:655-662`, `bin/disjuntor.py:696-712`.

9. **Regex de shell produz falsos positivos e negativos:** comentários, `echo` e heredocs contendo o wrapper são tratados como invocações; `CODEX_PAPEL=verificador CODEX_VOLTA=final bash bin/codex_pergunta.sh ...` é interpretado como papel `?`, volta `meio`. Assim FINAL com JUIZES pode ser negada por ANEXO, e volta do meio sem anexo pode escapar ao orçamento. Probes em memória confirmaram as interpretações; a causa está em `bin/disjuntor.py:649-662`, e os efeitos em `bin/disjuntor.py:687-706`.

10. **Codex direto e wrappers diferentes escapam:** só `Bash` contendo `codex_pergunta` é encaminhado ao controle, e `_codex_cmds` reconhece `codex_pergunta.sh`. `bin/disjuntor.py:727-739`, `bin/disjuntor.py:649-662`.

11. **A alegação de falha aberta tem exceções:**
    - `_mais_novo` engole erros de leitura/stat e devolve `False`; cópia ilegível vira “sem arquivo novo”, podendo disparar B1. `bin/disjuntor.py:305-324`, `bin/disjuntor.py:366-370`.
    - `.meta.json` ilegível vira `{}`, perdendo a isenção de agente de leitura; se houver cópia citada, B1 continua avaliando. `bin/disjuntor.py:275-281`, `bin/disjuntor.py:329-339`.
    - Cota JSON `NaN`, com reset futuro, passa na validação numérica e B4 nega com motivo `nan% >=75%`; probe em memória confirmou. `bin/disjuntor.py:485-488`, `bin/disjuntor.py:541-544`.
    - A negação é emitida **antes** da tentativa de registro no RELATO; falha no registro não desfaz o deny. `bin/disjuntor.py:777-781`, `bin/disjuntor.py:179-180`.

12. **Há travas sem timeout:** o lock do RELATO usa `flock(LOCK_EX)` bloqueante depois de emitir deny; coleta Codex usa `open` comum em `.jsonl`, permitindo FIFO bloquear. O lock de estado, por contraste, tenta por aproximadamente um segundo e então lança exceção capturada pelo topo. `bin/disjuntor.py:171-176`, `bin/disjuntor.py:193-200`, `bin/disjuntor.py:778-781`, `bin/codex_limite_estado.py:139-142`.

13. **Medidas ausentes/ilegíveis normalmente passam:** cota retorna `None`, estado retorna `{}`, transcript humano retorna `None`, transcript B1 ilegível não dispara; exceções anteriores à emissão do deny são capturadas pelo `main_pretool`. Porém falha de gravação de estado pode liberar até um `Agent` cuja cota conhecida manda negar, porque `_com_estado` precede o retorno do motivo. `bin/disjuntor.py:218-224`, `bin/disjuntor.py:333-336`, `bin/disjuntor.py:480-490`, `bin/disjuntor.py:518-519`, `bin/disjuntor.py:757-781`.

14. **Statusline não contém todo formato inesperado:** `rate_limits="x"` ou `seven_day="x"` pode levantar fora do `try`; `context_window="x"` também, em `cw.get`. Ausência de medidas ou falha de atualização preserva a cota anterior, que o leitor aceita até reset sem usar idade/session_id. `bin/statusline_cota.py:28-42`, `bin/disjuntor.py:480-490`.

15. **Exceção/ilegibilidade da sombra libera PreToolUse:** falha ao abrir/consultar a trava equivale a DIFF em curso e todas as regras normais são puladas; a guarda da chave continua anterior. `bin/disjuntor.py:229-246`, `bin/disjuntor.py:730-731`, `bin/disjuntor.py:774-776`.

Nenhuma ocorrência desses cenários em produção foi fornecida: [nao verificado].

## 31/10 00:03 · sessao 16c8e5e66646 · - · - · origem=Ronald (tmux)

Achados consolidados, somente leitura:

- **Piso pode ser de um turno posterior.** `_primeiro_turno` ignora assistente sem `usage` positivo e segue procurando; portanto o primeiro turno ilegível/zerado pode virar piso de outro turno, em vez de “não sei”. Probe em memória confirmou `{usage:{}}` seguido de `input_tokens=123` → piso `123`. `bin/orcamento_contexto.py:265-270`.
- **Piso ausente no primeiro Stop não é tentado novamente:** `piso_pendente` é removido antes de medir; sem `_tk`, não registra e já consumiu a oportunidade. `bin/hook_stop_fila1.py:866-871`.
- **Piso não é vinculado à hora/sessão do marco.** O Stop passa apenas `marco['origem']`; o avaliador escolhe o último boundary dessa origem no transcript recebido, sem conferir hora nem `session_id` do marco. Isso permite promover um boundary antigo ao piso mais recente, se o marco global e o transcript não corresponderem. `bin/hook_stop_fila1.py:832-870`; `bin/orcamento_contexto.py:274-286`; o estado global do marco contém `session_id`, mas ele não participa dessa escolha: `bin/orcamento_contexto.py:158-162`.

- **Falha de medida não significa ALLOW do Stop.** `_orcamento_contido` devolve `None` em exceção; o fluxo anterior continua, excluindo `PAREI ! CLEAR` da porta genérica e podendo terminar em `BLOCK`. Assim, estado/escrita quebrados podem trocar um CLEAR que liberaria pelo par válido por bloqueio até o TETO. `bin/hook_stop_fila1.py:977-986`, `bin/hook_stop_fila1.py:1080-1081`, `bin/hook_stop_fila1.py:912-920`, `bin/hook_stop_fila1.py:1205-1260`.
- **Medida atual ilegível não cancela disparo ou pendência anteriores.** `disparo` ativo continua conduzindo o CLEAR; `pendente` pode reativar o gatilho mesmo quando `avaliar` não dispara agora. `bin/hook_stop_fila1.py:878-948`.
- **Exceção pode bloquear ESPERANDO:** os cinco novos caminhos protegidos entram numa lista cujo `stat` com `OSError` rejeita a prova inteira, mesmo que o arquivo de produção apresentado seja outro e válido. A espera também captura exceção e devolve “sem prova”. `bin/hook_stop_fila1.py:558-567`, `bin/hook_stop_fila1.py:648-652`, `bin/hook_stop_fila1.py:1187-1191`.

- **B4 não exige o par em todas as saídas.** Sem handoff+COTA, primeiro retorna `siga`, depois `lembrete`; esse lembrete volta ao fluxo normal, que pode liberar por PAREI comum, ESPERANDO, fila vazia, TETO ou B2. Probe em memória confirmou `siga` → `lembrete` sem par. `bin/disjuntor.py:626-635`; `bin/hook_stop_fila1.py:1163-1189`, `bin/hook_stop_fila1.py:1201-1202`, `bin/hook_stop_fila1.py:1240-1253`.
- **B3 pode não ser atualizado antes do quarto verificador.** A varredura ocorre apenas nos `recados`, depois de todas aquelas saídas antecipadas; o PreToolUse consulta o estado acumulado sem varrer. Três reprovações em transcript ainda não varrido deixam passar outro verificador. `bin/hook_stop_fila1.py:1004`, `bin/hook_stop_fila1.py:1256`; `bin/disjuntor.py:748-749`.

Quanto à pergunta **d**, existem mudanças reais de decisão:

- CLEAR que antes liberava passa a exigir handoff novo e pode ser adiado por lock/agente pendente; um handoff novo também cria outro bloqueio para pedir a linha. `bin/hook_stop_fila1.py:882-911`, `bin/hook_stop_fila1.py:935-941`.
- `PAREI ! COTA` perde a porta genérica mesmo sem escada ativa ou com disjuntor desligado. `bin/hook_stop_fila1.py:994-995`, `bin/hook_stop_fila1.py:1080-1081`.
- Novos arquivos deixam de provar ESPERANDO, com a consequência adicional do `stat` acima. `bin/hook_stop_fila1.py:558-569`.
- B2 acrescenta ALLOW antes do TETO normal alcançar sua liberação. Conta todos os `BLOCK` recentes, inclusive os do orçamento/B4/RELATO, sem conferir se eram realmente `siga` de fila. `bin/disjuntor.py:588-599`; `bin/hook_stop_fila1.py:1156`, `bin/hook_stop_fila1.py:1168`, `bin/hook_stop_fila1.py:1179`, `bin/hook_stop_fila1.py:1250-1253`.
- O avaliador remove os gatilhos antigos de tempo/deploy e usa apenas tokens adaptativos; a janela de CLEAR fica em dez minutos. O aval desses comportamentos além dos requisitos expressos fornecidos é **[nao verificado]**. `bin/orcamento_contexto.py:56`, `bin/orcamento_contexto.py:225-232`, `bin/orcamento_contexto.py:332-341`.

Mapa para **c**:

| Arquivo | Escritor | Leitores |
|---|---|---|
| `orcamento_estado.json` | Mesmo Stop, `_orc_salva`, sem novo escritor direto identificado | Stop; `orcamento_contexto.ler`; `disjuntor.ler` — `bin/hook_stop_fila1.py:722-739`, `bin/hook_stop_fila1.py:816-822`, `bin/orcamento_contexto.py:348-375`, `bin/disjuntor.py:849-852` |
| `orcamento.log` | `_orc_trilha` do Stop | OC e DJ — `bin/hook_stop_fila1.py:699-709`, `bin/orcamento_contexto.py:359-367`, `bin/disjuntor.py:826-844` |
| `pisos.log` | Função `registrar_piso`, acionada pelo Stop **e** CLI `--semear-pisos` | `ultimo_piso`/`gatilho_tokens`, DJ, painel — `bin/orcamento_contexto.py:198-232`, `bin/orcamento_contexto.py:291-310`, `bin/orcamento_contexto.py:402-406`, `bin/hook_stop_fila1.py:866-871`, `bin/disjuntor.py:869-871` |
| `ultimo_clear.json` | `marco_de_clear.py`, sob flock e replace | Próprio escritor conserva clears; OC; Stop por OC — `bin/marco_de_clear.py:37-61`, `bin/orcamento_contexto.py:152-164`, `bin/hook_stop_fila1.py:832` |
| `HANDOFF-SESSAO.md` | `handoff_sessao.sh` | Novo Stop usa mtime — `bin/handoff_sessao.sh:18`, `bin/hook_stop_fila1.py:748-752` |

`gerar_estado.py` continua leitor do orçamento e acrescenta a leitura do disjuntor; a leitura do orçamento permanece fora do `try` novo e pode derrubar a geração se o JSON tiver formato estrutural inesperado, pois `OC.ler` faz `est.get` depois do bloco de leitura sem validar que seja dicionário. `bin/gerar_estado.py:274-283`; `bin/orcamento_contexto.py:351-368`.

O RELATO tem uma função reutilizada, mas não um único processo escritor: `no_relato` faz read-modify-write com temporário fixo `RELATO.md.novo`, sem lock próprio. O lock novo do disjuntor só serializa quem o respeitar; chamadas preexistentes do vigia e geradores continuam concorrentes. `app/core/esteira_vigia.py:830-847`, `app/core/esteira_vigia.py:1129`, `bin/disjuntor.py:171-176`, `bin/gerar_cortes.py:117-136`.

## 31/10 00:03 · sessao 8a469ce71757 · - · - · origem=Ronald (tmux)

Achados concretos para Q2/Q3, só por código:

1. **Os três carimbos não cobrem toda alteração do cadastro.**

- O censo usa a âncora viva do vínculo para recalcular `prev`, não a âncora congelada no DNA: `app/ponto/management/commands/censo_fase_12x36.py:170` e `app/escala/services/propositor.py:218`. Alteração da âncora pelo signal só regenera desde a janela **atual**, não os dias antigos: `app/escala/signals.py:104`, `app/escala/signals.py:114`, `app/escala/signals.py:118`. Além disso, mudança para `None` sai sem regenerar: `app/escala/signals.py:108`. Portanto uma âncora diferente pode alterar o censo de uma competência antiga sem alterar nenhum dos três carimbos nessa competência; o vínculo não tem carimbo próprio, como reconhece `app/escala/signals.py:125`.
- Há escrita explícita para apagar âncoras por `QuerySet.update`, sem signal: `invalidar_ancoras_incoerentes`, `app/colaboradores/services/vinculo.py:849`, chamado por `app/escala/utils.py:2049`. O chamador maior `executar_vinculo` regenera o vínculo alvo depois, mas a invalidação pode alcançar outros vínculos ativos do colaborador: `app/colaboradores/services/vinculo.py:852`, `app/colaboradores/services/vinculo.py:312`.
- **Recusa de escala** chama `reverter_para_snapshot`: `app/chamados/services/acoes_chamado.py:141`. A reversão apaga os vínculos novos e restaura tipo, início, fim e ativa por `update`, sem regeneração: `app/colaboradores/services/vinculo.py:864`, `app/colaboradores/services/vinculo.py:874`. Apagar vínculo pode zerar a geradora da célula por `SET_NULL`, sem mover os carimbos: `app/escala/models.py:1660`. Isso altera a autoridade do dia e o universo/segmentos do censo: `app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:253`, `app/ponto/management/commands/censo_fase_12x36.py:156`.
- **Alteração de template** propaga regeneração apenas para vínculos ativos e desde a competência anterior: `app/escala/services/cadastro_tipo.py:334`, `app/escala/services/cadastro_tipo.py:337`, `app/escala/services/cadastro_tipo.py:348`. Alteração do ciclo de template histórico pode tirar dias/colaborador do universo 12x36, sem carimbo antigo: `app/ponto/management/commands/censo_fase_12x36.py:104`, `app/ponto/management/commands/censo_fase_12x36.py:157`. A geometria viva também alimenta o pareador: `app/ponto/turnos.py:1889`, `app/ponto/turnos.py:1965`.
- Mesmo a correção pela porta pode deixar o passado sem carimbo: a regeneração pula competências lavradas e células humanas, enquanto o censo lê vínculo e âncora vivos: `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:251`, `app/ponto/management/commands/censo_fase_12x36.py:171`. O comando retroativo ainda admite piso de regeneração posterior ao início corrigido: `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:225`.

2. **Batida sem célula não gera nenhum dos três carimbos.**

O evento chama o cartório, mas ele retorna se a célula não existe: `app/ponto/signals.py:131`, `app/ponto/services/cartorio.py:1547`. O censo consegue medir esses dias pelo fallback de vigência: `app/escala/alimentacao.py:253`, e lê suas batidas: `app/ponto/management/commands/censo_fase_12x36.py:172`. O novo gatilho consulta exclusivamente células existentes: `app/escala/services/fase_12x36.py:182`. Logo inserir/retratar/apagar batida de uma janela histórica sem células pode mudar a paridade sem reabertura. O exemplo de teste cria a célula antes da batida e não cobre esse caso: `app/escala/tests/test_vfase_dono_as_of.py:54`.

3. **Correção que move batida de data não carimba a origem no evento.**

A regularização externa modifica `timestamp` e salva a instância: `app/chamados/services/regularizacao_ext.py:149`. O receiver calcula somente a nova data e a nova véspera, sem reter a data antiga: `app/ponto/signals.py:108`, `app/ponto/signals.py:131`. Se a batida sair de uma janela antiga para outra distante, o evento não marca a janela de origem; depende da varredura posterior do cartório: `app/ponto/management/commands/processar_cartorio.py:49`. Já `delete` comum chama o cartório com a data original: `app/ponto/signals.py:138`. Não confirmei caminho produtivo que mude `colaborador` da batida; essa generalização seria [nao verificado].

4. **Nascimento/enriquecimento de DNA sem carimbo existe.**

O gerador cria células antigas ou preenche DNA antes nulo sem escrever `julgada_em`, `regenerada_em` ou `editada_em`: `app/ponto/portas/celula.py:592`, `app/ponto/portas/celula.py:603`. O DNA alimenta a geometria real do turno: `app/ponto/turnos.py:1946`, `app/ponto/turnos.py:1973`. O novo gatilho ignora `gerada_em`: `app/escala/services/fase_12x36.py:183`. O leitor de invalidação já existente considera `gerada_em` e `veredito_em` além desses três campos: `app/chamados/veredito_lavrado.py:151`.

5. **Escritores reais dos carimbos.**

| Campo | Escritor |
|---|---|
| `julgada_em` | `carimbar`, atribui o `agora` recebido e grava: `app/ponto/portas/celula.py:721`, `app/ponto/portas/celula.py:728`. O cartório chama após julgamento: `app/ponto/services/cartorio.py:999`, `app/ponto/services/cartorio.py:1424`. |
| `regenerada_em` | Regeneração de vínculo: `app/ponto/portas/celula.py:262`; reversão de regeneração de um dia: `app/ponto/portas/celula.py:536`. Os dois leem relógio diretamente. |
| `editada_em` | Campo existe: `app/escala/models.py:1700`, mas **não há escritor produtivo** no censo da cópia; o contrato declara explicitamente `SEM_ESCRITOR`: `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:69`. Portanto a docstring nova promete um gatilho humano que atualmente não tem produtor: `app/escala/services/fase_12x36.py:160`. |

A regeneração tem mais exceções que a docstring nova reconhece: fora da vigência há um ramo que salva apenas `trabalha`/`dna` (`app/ponto/portas/celula.py:231`); criação salva só `gerada_em` automático (`app/ponto/portas/celula.py:247`); regeneração normal só marca quando DNA/trabalha/geradora diferem (`app/ponto/portas/celula.py:251`). Com `recalculo=True`, os dias tocados geralmente são julgados depois, mas células com veredito humano são excluídas dessa etapa: `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:365`.

6. **Leitores produtivos dos carimbos encontrados por busca.**

- Novo gatilho: todos os três, `app/escala/services/fase_12x36.py:183`.
- Invalidação da lavra de perguntas: todos os três, mais `gerada_em`/`veredito_em`, `app/chamados/veredito_lavrado.py:153`.
- Cartório: regenerada/editada para detectar fato no intervalo, `app/ponto/services/cartorio.py:285`; julgada para pular impressão igual, `app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:1092`.
- Regeneração/cobrança órfã: `app/escala/services/regeneracao.py:80`, `app/escala/services/regeneracao.py:145`; `app/ponto/management/commands/gerar_celulas.py:242`; `app/ponto/management/commands/supra_juiz.py:152`; `app/chamados/management/commands/materializar_passivo_validado.py:220`.
- Exibição/diagnóstico: `app/ponto/services/dia_do_colab.py:91`; `app/ponto/management/commands/celula_veredito_velho.py:109`.
- Universo de backfill/vigia: `app/ponto/management/commands/backfill_b5_insumos.py:61`; `app/ponto/management/commands/vigia_b5_ausencia.py:55`.
- Auditoria de julgada sem veredito: `app/chamados/management/commands/auditar_invariantes_chamados.py:1593`.

7. **Pré-requisito do cartório ficou de fora da declaração do cron.**

O cron declara explicitamente que não depende do cartório, e só depende de `reconciliar_vinculo`: `app/config/crons.py:617`, `app/config/crons.py:620`; este só declara `detectar_par_relampago`: `app/config/crons.py:470`. O novo gatilho depende de `julgada_em`: `app/escala/services/fase_12x36.py:183`; o batch que atualiza esse campo julga todas as células passadas: `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:65`. O horário nominal anterior do cartório (`app/config/crons.py:597`) não prova execução/conclusão nem substitui a dependência.

Signals de batida normais tentam julgar sincronicamente, inclusive exclusão: `app/ponto/signals.py:131`, `app/ponto/signals.py:138`; porém o julgamento é best effort e falha não barra a escrita: `app/ponto/signals.py:85`. Não encontrei bulk produtivo de batidas que justificasse alegar esse caminho como fato; o `bulk_create` identificado está restrito a ensaio: `app/ponto/registro_batida.py:331`.

## 31/10 00:03 · sessao c7368d7ebe57 · - · - · origem=Ronald (tmux)

Achados prioritários já enviados:

- Repetir censo no mesmo dia após reabertura apaga `janelas`/`itens_reabertos` daquela reabertura: segunda corrida não reabre, mas sobrescreve a mesma linha diária; leitura recai na lavra velha. Porta: `app/escala/services/fase_12x36.py:91-93`; seleção: `:171-186`; conteúdo substituído: `app/ponto/management/commands/censo_fase_12x36.py:298-305`.
- `fila_por_causa` julga chamado de dia passado, mas continua lendo IDs da última lavra e decide `grade_errada` sem usar o dia: `app/chamados/services/fila_por_causa.py:161`, `:174`, `:199-221`.
- Propositor aceita janela arbitrária pela CLI e continua só com `ultimo_lavrado().itens`: `app/escala/management/commands/propostas_de_escala.py:12`, `:29`; `app/escala/services/propositor.py:415-417`, `:492-493`. A justificativa do commit sobre janela invariavelmente de 28 dias é insuficiente.
- Legado com `itens` mas sem `janelas`: `ultimo_lavrado` aceita (`fase_12x36.py:201`), as-of exclui (`:105-106`). Primeira lavra só mede anterior+atual (`censo_fase_12x36.py:81-84`), portanto executar uma vez não reconstrói passado anterior a esse recorte; reabertura também só percorre lavras com cobertura (`fase_12x36.py:171`).
- Pré-condição de cron mudou: gatilho agora depende de `julgada_em` (`fase_12x36.py:182-186`), mas dependências continuam somente `reconciliar_vinculo`, com comentário dispensando cartório (`app/config/crons.py:617-620`).
- Seleção de cobertura e carga do detalhe são consultas separadas sem proteção abrangente: sobrescrita entre ambas pode misturar versões (`fase_12x36.py:134`, `:142`); ocorrência concreta em produção é [nao verificado].

Mapa compacto de campos:

| Campo | Escritor | Leitores |
|---|---|---|
| `detalhe.itens` | Censo `:291-293` → porta fase `:91-93` | as-of fase `:144-146`; último fase `:201-205`; C10 cadastro_realidade `:345-352`; propositor `:492-493` |
| `detalhe.janelas` | Censo `:298-301` → mesma porta | índice fase `:105-110`; as-of `:134-137`; reabertura `:171-176` |
| `detalhe.agora` | `_agora` censo `:62`, serialização `:302` → mesma porta | índice fase `:110`; reabertura `:179-186` |
| `detalhe.itens_reabertos` | Censo `:303-305` → mesma porta | as-of fase `:144-146`; último transporta JSON completo `:205` |
| `data_ref` | Porta fase `:92`, recebe `_hoje` censo `:63`, `:307` | índice fase `:108-109`; último `:199-206`; API `app/api/views_mensageria.py:1676`; vigia `app/ponto/management/commands/vigiar_familia_escala.py:26` |
| `calculado_em` | ORM `auto_now`, `app/inteligencia/models.py:96` | índice fase `:109`; último `:200`, `:207` |

Consumidores de outros valores da última lavra: fila `colabs_de_desvio` (`fila_por_causa.py:174`); pauta IDs (`fase_12x36.py:219-224`); vigia metadados/linhas (`vigiar_familia_escala.py:24-32`); API metadados/linhas (`views_mensageria.py:1672-1677`).

Checklist sem falsos positivos: não identifiquei segundo juiz de fase introduzido; paridade e piso continuam no censo (`:246-248`), vínculo passa por `vinculo_do_dia` (`:155-160`), janela pelos juízes existentes (`:81-84`). `_cobre` é cobertura da lavra (`fase_12x36.py:113-114`), distinta de cobertura de vínculo. Não identifiquei segundo escritor efetivo desta chave: caminho continua censo → `lavrar` (`censo:271`, `:282-307`). `inteligencia.metricas.calcular` exige registro (`app/inteligencia/metricas.py:39-42`) e não há registro desta chave encontrado; genericamente escreve em `:47-56`. Nenhum `timezone.now()` novo no miolo adicionado; relógio do censo permanece na borda (`censo:62`).

## 31/10 00:03 · sessao 229f7b6e6704 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos por leitura estática; execução dos mutantes e dos RED/GREEN: [nao verificado].

**Falha concreta de repetição no mesmo dia.** `lavrar` sobrescreve o snapshot da mesma `data_ref` (`app/escala/services/fase_12x36.py:91`; unicidade em `app/inteligencia/models.py:103`). Após uma corrida reabrir outubro, a segunda corrida do mesmo dia considera a janela assentada porque seu `agora` já ultrapassa o carimbo (`app/escala/services/fase_12x36.py:171`, `:186`). Ela grava apenas as correntes, retirando a cobertura e os `itens_reabertos` da primeira corrida (`app/ponto/management/commands/censo_fase_12x36.py:298`, `:303`). O leitor volta à lavra anterior que cobre outubro, podendo ressuscitar o resultado antigo (`app/escala/services/fase_12x36.py:134`, `:144`). O teste ii repete somente **no dia seguinte**, preservando outro snapshot (`app/escala/tests/test_vfase_dono_as_of.py:73`).

**Leitura em duas versões.** O índice escolhe PKs e cobertura numa consulta e busca o detalhe posteriormente; uma sobrescrita da mesma PK entre elas permite combinar cobertura antiga com itens novos (`app/escala/services/fase_12x36.py:134`, `:142`). A porta diária permite essa sobrescrita (`app/escala/services/fase_12x36.py:91`). Ocorrência em produção: [nao verificado].

**Janelas sobrepostas.** Reabertura identifica a janela pelo triplo exato empresa/início/fim (`app/escala/services/fase_12x36.py:173`), enquanto o leitor identifica cobertura por dia (`app/escala/services/fase_12x36.py:114`). O corte da empresa pode mudar os intervalos correntes (`app/ponto/janelas.py:36`, `:60`). Uma janela antiga reaberta pode sobrepor uma corrente; ambas são medidas separadamente, com denominadores distintos (`app/ponto/management/commands/censo_fase_12x36.py:124`, `:172`). Depois, o leitor une itens correntes e reabertos sem distinguir a janela que julgou cada item (`app/escala/services/fase_12x36.py:144`). Cenário real dessa mudança: [nao verificado].

**Pergunta 5 — os seis testes e mutantes que sobrevivem individualmente:** [nao verificado]

| Teste | O que verifica | Mutante que o deixa verde | [nao verificado]
|---|---|---|
| `test_MORDE_i...` (`app/escala/tests/test_vfase_dono_as_of.py:39`) | Preserva outubro quando a janela anda. | Ordenar lavras da **mais antiga** para a mais recente no índice: outubro tem apenas a primeira cobertura nesse caso (`app/escala/services/fase_12x36.py:109`; teste `:40`, `:45`). |
| `test_MORDE_ii...` (`:50`) | Batida nova, leitura de reabertos, separação das listas e assentamento no dia seguinte. | Remover `Max(regenerada_em)` e `Max(editada_em)`, usando só `julgada_em`: a única alteração exercitada é inserção de batidas (`app/escala/services/fase_12x36.py:182`; teste `:59`). Também sobrevive trocar `>` por `>=`, pois não há igualdade exercitada (`fase_12x36.py:186`; teste `:59`, `:64`). |
| `test_MORDE_iii...` (`:82`) | Demonstra o defeito do leitor da última lavra e restaura o leitor real. | Fazer `janelas_reabertas` devolver sempre `{}`: o caso não altera fatos depois da primeira lavra (`app/escala/services/fase_12x36.py:151`; teste `:86`, `:88`). A ausência dentro do `mock` é a resposta programada do próprio substituto (`teste :90`, `:93`). |
| `test_MORDE_dia_que_lavra_nenhuma_cobriu...` (`:97`) | Outubro ausente numa lavra que só tem janelas posteriores. | Restaurar o leitor antigo baseado apenas em `ultimo_lavrado(...).itens`: a última lavra também não contém outubro nessa fixture (`teste :99`; substituto equivalente em `:90`). | [nao verificado]
| `test_MORDE_lavra_mais_recente...` (`:103`) | Cobertura mais recente sem item vence; respeita `ate` entre datas. | Remover o filtro de empresa de `_cobre`: só existe uma empresa no cenário (`app/escala/services/fase_12x36.py:114`; fixture em `app/escala/tests/test_o261_v_fase_na_lista.py:52`). |
| `test_MORDE_a_lavra_declara...` (`:120`) | Dois intervalos correntes e `agora` verdadeiro. | Gravar `_hoje.isoformat()` em `agora`, perdendo a hora: o teste verifica somente truthiness (`app/ponto/management/commands/censo_fase_12x36.py:302`; teste `:125`). |

Os seis testes inteiros deixam sobreviver, em conjunto, a retirada dos carimbos de cadastro e a retirada do filtro de empresa: a fixture contém uma empresa/um colaborador e nenhum teste novo modifica cadastro (`app/escala/tests/test_o261_v_fase_na_lista.py:52`, `:57`; `app/escala/tests/test_vfase_dono_as_of.py:39`, `:50`, `:82`, `:97`, `:103`, `:120`).

`_Caso` não acrescenta testes herdados: contém `setUpTestData` e helpers (`app/escala/tests/test_o261_v_fase_na_lista.py:45`, `:75`). O arquivo novo possui seis métodos de teste nas linhas 39, 50, 82, 97, 103 e 120. Os logs mencionados no commit não foram encontrados na busca pelos arquivos da cópia; comando utilizado e resultados de “Ran 9”/“Ran 79”: [nao verificado].

## 31/10 00:02 · sessao 3c8dcbe5be29 · V3-E1 · verificador · origem=Code

**(1) O fluxo normal está protegido, mas `SkipTest` ainda tem precedência sobre falhas já coletadas.** (`app/ponto/tests/test_v3_e1_valor.py:148`, `app/ponto/tests/test_v3_e1_valor.py:159`)

- Sem censo, o modo valor falha como `ARNES` antes de imprimir VERDE. Nos adaptadores reais, o censo é registrado depois de `_releitura`. Isso comprova passagem pela chamada; **não exige `censo.relidos > 0`**. (`app/ponto/tests/test_v3_e1_valor.py:170`, `app/ponto/tests/test_v3_e1_valor.py:229`, `app/ponto/tests/test_v3_e1_valor.py:254`, `app/ponto/tests/test_v3_e1_valor.py:187`)
- Falhas de asserção dentro e fora de `subTest` são coletadas no modo valor; com censo, validado falha e pendente vira skip rotulado. Erros como `ValueError`, `TypeError` e `KeyError` propagam: os coletores só capturam `SkipTest` e `failureException`. (`app/ponto/tests/test_v3_e1_valor.py:148`, `app/ponto/tests/test_v3_e1_valor.py:161`, `app/ponto/tests/test_v3_e1_valor.py:182`)
- **Brecha residual:** uma asserção falha em `subTest`, é coletada, e depois o caso chama `skipTest`: o skip escapa imediatamente, inclusive para caso validado, sem consultar a coleta. É caminho permitido pelo marcador; os casos explicitamente excluídos de dúvida/status ficam fora do universo de valor. (`app/ponto/tests/test_v3_e1_valor.py:150`, `app/ponto/tests/test_v3_e1_valor.py:159`, `app/ponto/tests/test_v3_e1_valor.py:65`, `app/ponto/tests/test_v3_e1_valor.py:210`)
- VERDE com **zero períodos relidos** continua possível; não significa ausência da chamada. L16, por exemplo, afirma zeros sem batidas. (`app/ponto/tests/test_v3_e1_valor.py:170`, `app/ponto/tests/test_v3_e1_valor.py:176`, `app/ponto/tests/test_e1_lote3.py:614`, `app/ponto/tests/test_e1_lote3.py:623`)

**(2) A classificação dos nomes recebidos por `subTest(rubrica=...)` está correta; a cesta é incompleta como inventário de tudo que o caso afirma.** Os helpers encaminham as chaves, e a divisão usa pertinência ao conjunto. (`app/ponto/tests/test_e1_lote1_valor_do_dia.py:128`, `app/ponto/tests/test_e1_lote3.py:235`, `app/ponto/tests/test_v3_e1_valor.py:173`)

| Casos | Classificação conferida | Evidência |
|---|---|---|
| C01–C20, exceto C06/C16/C19 | Todos os nomes encaminhados estão em v3 | `app/ponto/tests/test_v3_e1_valor.py:58`; helpers em `app/ponto/tests/test_e1_lote1_valor_do_dia.py:125` |
| C19 | v3: `ft_min,he100_fer,extra`; v2: `ft` | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:539` |
| L01 | v3: `trab,atraso,antecipada` | `app/ponto/tests/test_e1_lote3.py:364` |
| L13/L14 | v3: trabalho/HE e pontualidade afirmados; v2: `ft` | `app/ponto/tests/test_e1_lote3.py:570`, `app/ponto/tests/test_e1_lote3.py:588` |
| L15 | v3: `atraso,antecipada`; v2: `faltas` | `app/ponto/tests/test_e1_lote3.py:609` |
| L16 | v3: `trab,ft_min,atraso,antecipada`; v2: `faltas,horas_falta` | `app/ponto/tests/test_e1_lote3.py:623` |
| L17/L18 | v3: trabalho/HE afirmados; v2: `ft` | `app/ponto/tests/test_e1_lote3.py:650`, `app/ponto/tests/test_e1_lote3.py:668` |
| L19/L20 | v3: `trab`; v2: `semanas_dsr_ok,semanas_dsr_perdido` | `app/ponto/tests/test_e1_lote3.py:692`, `app/ponto/tests/test_e1_lote3.py:714` |

`ft_min` pertence corretamente à v3: o leitor soma `minutos_trabalhados` dos FT, e `reler` percorre normais e FT e reescreve esse campo. (`app/ponto/tests/test_o280_bateria_da_lei.py:174`, `app/ponto/calculador/periodo.py:606`, `app/ponto/calculador/periodo.py:742`)

Ficam **fora da cesta**:

- `fonte_da_regua` em C01/C02/C03/C04/C05/C07/C08/C09 e `regua_excedente` em C05. (`app/ponto/tests/test_e1_lote1_valor_do_dia.py:160`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:181`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:205`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:231`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:256`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:257`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:298`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:317`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:345`)
- A CxR de v2 em L01 e, especialmente, **`trab + ft_min == 180` em L15**. Se essa soma falhar, aborta antes dos subTests seguintes: pode produzir `v3=[] v2=[]`, embora a falha seja preservada como `(fora de subTest)`. (`app/ponto/tests/test_e1_lote3.py:365`, `app/ponto/tests/test_e1_lote3.py:608`, `app/ponto/tests/test_e1_lote3.py:609`, `app/ponto/tests/test_v3_e1_valor.py:163`)
- Também ficam externas as precondições de previsto em L13, calendário em L17/L18 e sucesso das batidas. (`app/ponto/tests/test_e1_lote3.py:567`, `app/ponto/tests/test_e1_lote3.py:649`, `app/ponto/tests/test_e1_lote3.py:667`, `app/ponto/tests/test_e1_lote3.py:174`)

A cesta indica responsabilidade pelo **nome**, não medição efetiva: não consulta os flags do censo. C19 ainda afirma `extra/he100_fer` sobre somas apenas dos períodos normais, podendo afirmar zero de soma vazia. (`app/ponto/tests/test_v3_e1_valor.py:173`, `app/ponto/calculador/periodo.py:745`, `app/ponto/tests/test_o280_bateria_da_lei.py:166`, `app/ponto/tests/test_o280_bateria_da_lei.py:179`, `app/ponto/tests/test_o280_bateria_da_lei.py:182`)

**(3) O selo morde parcialmente; não comprova todos os caminhos pedidos.** (`app/ponto/tests/test_v3_e1_valor.py:294`, `app/ponto/tests/test_v3_e1_valor.py:306`)

| Caminho | Alcance do selo | Evidência |
|---|---|---|
| Validado vermelho | Exige falha, com censo artificial; não verifica mensagem | `app/ponto/tests/test_v3_e1_valor.py:284`, `app/ponto/tests/test_v3_e1_valor.py:296` |
| Pendente vermelho | Exige skip e verifica prefixo | `app/ponto/tests/test_v3_e1_valor.py:299` |
| Verde | Exige somente ausência de exceção; não verifica impressão/cesta | `app/ponto/tests/test_v3_e1_valor.py:302` |
| Sem releitura | Exige falha para caso sem censo | `app/ponto/tests/test_v3_e1_valor.py:303` |
| `VALIDADOS` sem caso | Guarda implementada, mas sem injeção negativa; com `{}`, o laço é vazio | `app/ponto/tests/test_v3_e1_valor.py:55`, `app/ponto/tests/test_v3_e1_valor.py:310` |

Execução completa da suíte Django e comparação Git com `2258f914`: [não verificado].

## 31/10 00:00 · sessao 2d1b429ee43c · - · - · origem=Ronald (tmux)

A cesta classifica corretamente os nomes que chegam por `subTest(rubrica=...)`: os helpers encaminham cada chave e o marcador divide por pertinência a `RUBRICAS_DA_V3`. (`app/ponto/tests/test_e1_lote1_valor_do_dia.py:125`, `app/ponto/tests/test_e1_lote3.py:233`, `app/ponto/tests/test_v3_e1_valor.py:143`, `app/ponto/tests/test_v3_e1_valor.py:173`)

- Todos os subTests de C01–C05, C07–C15, C17–C18 e C20 são v3; C19 separa `ft_min/he100_fer/extra` em v3 e `ft` em v2. (`app/ponto/tests/test_e1_lote1_valor_do_dia.py:157`, `:179`, `:203`, `:229`, `:254`, `:296`, `:315`, `:342`, `:363`, `:382`, `:402`, `:419`, `:438`, `:457`, `:499`, `:519`, `:539`, `:557`)
- L01: v3 `trab/atraso/antecipada`; L13: v3 `trab/extra/he50/he100/atraso/antecipada`, v2 `ft`; L14: v3 `trab/extra/he50/he100`, v2 `ft`. (`app/ponto/tests/test_e1_lote3.py:364`, `:570`, `:588`)
- L15: v3 `atraso/antecipada`, v2 `faltas`; L16: v3 `trab/ft_min/atraso/antecipada`, v2 `faltas/horas_falta`. (`app/ponto/tests/test_e1_lote3.py:609`, `:623`)
- L17: v3 `trab/he100_fer/he50`, v2 `ft`; L18: v3 `trab/he100_fer/he50/he100/extra`, v2 `ft`; L19/L20: v3 `trab`, v2 `semanas_dsr_ok/semanas_dsr_perdido`. (`app/ponto/tests/test_e1_lote3.py:650`, `:668`, `:692`, `:693`, `:695`, `:714`, `:715`, `:717`)
- C06/C16 ficam fora do universo de valor, conforme filtro explícito. (`app/ponto/tests/test_v3_e1_valor.py:65`, `:82`)

`ft_min` foi corrigido justificadamente: o leitor soma `minutos_trabalhados` dos FT, e `reler` percorre normais e FT e reescreve esse campo. (`app/ponto/tests/test_o280_bateria_da_lei.py:174`, `app/ponto/calculador/periodo.py:606`, `:742`)

Há afirmações que a cesta **não vê**, pois `_vistas` só recebe nomes na entrada de `subTest`:

- `fonte_da_regua` em C01, C02, C03, C04, C05, C07, C08 e C09; e `regua_excedente` em C05. (`app/ponto/tests/test_e1_lote1_valor_do_dia.py:160`, `:181`, `:205`, `:231`, `:256`, `:257`, `:298`, `:317`, `:345`; coleta em `app/ponto/tests/test_v3_e1_valor.py:143`)
- CxR de **v2** (`r.dias_cadastro_x_realidade`) em L01; a CxR da v3 é escrita em `censo.cxr`. (`app/ponto/tests/test_e1_lote3.py:365`, `app/ponto/calculador/periodo.py:790`)
- A afirmação central de L15, `trab + ft_min == 180`, também fica invisível. Se ela falhar, o caso aborta antes dos subTests seguintes, e a cesta pode ficar inteiramente vazia; a falha continua capturada como `(fora de subTest)`. (`app/ponto/tests/test_e1_lote3.py:608`, `:609`, `app/ponto/tests/test_v3_e1_valor.py:161`)
- As precondições sobre previsto em L13, feriados em L17/L18 e sucesso do registro de cada batida também são externas a subTest. (`app/ponto/tests/test_e1_lote3.py:567`, `:649`, `:667`, `:174`)

A classificação é por **campo de responsabilidade**, não prova de medição efetiva: `not_rel/not_leg` dependem de CCT; atraso/antecipada dependem de marcos, e os períodos sem dia permanecem v2 dentro das somas. O resumo imprime os flags e ressalvas, mas a divisão da cesta não consulta esses flags. (`app/ponto/calculador/periodo.py:745`, `:595`, `:608`, `app/ponto/tests/test_v3_e1_valor.py:121`, `:173`)

C19 tem a limitação declarada corretamente: `extra/he100_fer` somam apenas normais; no caso só FT, os zeros podem ser soma vazia. (`app/ponto/tests/test_o280_bateria_da_lei.py:164`, `:166`, `:179`, `:182`, `app/ponto/tests/test_v3_e1_valor.py:25`)

## 31/10 00:00 · sessao 353e609ef4f8 · - · - · origem=Ronald (tmux)

O selo cobre parcialmente os caminhos pedidos:

- **Validado vermelho:** injeta `Z99` em `VALIDADOS`, cria censo artificial e exige `failureException`; portanto exercita o ramo de falha, embora não confira a mensagem `VALIDADO VERMELHO`. `app/ponto/tests/test_v3_e1_valor.py:284`, `app/ponto/tests/test_v3_e1_valor.py:296`, `app/ponto/tests/test_v3_e1_valor.py:182`.
- **Pendente vermelho:** exige `SkipTest` e confere o prefixo correspondente; esse ramo tem verificação explícita do resultado. `app/ponto/tests/test_v3_e1_valor.py:299`.
- **Verde:** chama um caso com censo artificial e asserção verdadeira; verifica somente que não há exceção. Não captura `stderr` nem exige `V3-E1 VERDE`, cesta ou censo; remover a impressão verde não faria esse selo falhar. `app/ponto/tests/test_v3_e1_valor.py:289`, `app/ponto/tests/test_v3_e1_valor.py:302`, `app/ponto/tests/test_v3_e1_valor.py:176`.
- **Sem releitura:** caso vazio precisa levantar `failureException`; cobre a guarda de ausência de censo, mas não exige a mensagem específica `ARNES`. `app/ponto/tests/test_v3_e1_valor.py:303`, `app/ponto/tests/test_v3_e1_valor.py:170`.
- **VALIDADOS sem caso:** há guarda funcional com `assertIn(cid, ids)`, mas **não há teste que injete um ID inexistente e prove que essa guarda falha**. Como `VALIDADOS = {}`, o laço atual é vazio; esse caminho não tem selo anti-vacuidade. O mesmo vale para citação vazia. `app/ponto/tests/test_v3_e1_valor.py:55`, `app/ponto/tests/test_v3_e1_valor.py:306`, `app/ponto/tests/test_v3_e1_valor.py:310`.

A suíte do selo também não injeta erro comum nem falha fora de `subTest`, embora esses caminhos existam no marcador; tampouco cobre os ramos de propriedade sem censo/com `_props`. `app/ponto/tests/test_v3_e1_valor.py:294`, `app/ponto/tests/test_v3_e1_valor.py:161`, `app/ponto/tests/test_v3_e1_valor.py:164`, `app/ponto/tests/test_v3_e1_valor.py:315`.

## 30/10 23:58 · sessao 58f8cb44399b · V3-E1 · verificador · origem=Code

**O arnês admite sucesso sem comprovar que a v3 julgou a rubrica.** No modo de valor, basta `_coleta` vazia; nem sequer é obrigatória uma releitura. No modo de propriedades, as asserções dos casos importados podem ser descartadas. ([test_v3_e1_valor.py:146](/home/ronald/codex_ws/2258f914afd6597484901ae7cde3cf91cf6a0ace/app/ponto/tests/test_v3_e1_valor.py:146), [test_v3_e1_valor.py:150](/home/ronald/codex_ws/2258f914afd6597484901ae7cde3cf91cf6a0ace/app/ponto/tests/test_v3_e1_valor.py:150), [test_v3_e1_valor.py:155](/home/ronald/codex_ws/2258f914afd6597484901ae7cde3cf91cf6a0ace/app/ponto/tests/test_v3_e1_valor.py:155))

Abaixo está a análise do código disponível. Identidade/diff do commit e resultado integral da suíte Django: **[nao verificado]**. [nao verificado]

**1) Rubricas, caso a caso**

A coluna “v3 escreve” indica o escritor do campo nos períodos efetivamente relidos. Não comprova cobertura: `reler` deixa passar períodos sem dia, abertos, sem carimbo ou invertidos, e o arnês não exige censo sem ressalvas antes do VERDE. Noturno depende de `cct`; pontualidade depende de `marcos`. (`app/ponto/calculador/periodo.py:609`, `:634`, `:641`, `:645`, `:696`, `:745`; `app/ponto/tests/test_v3_e1_valor.py:155`)

| Caso | Rubricas afirmadas cujo campo a v3 escreve | Herdado do v2 / outra autoridade | Asserção |
|---|---|---|---|
| C01 | `trab`, `not_rel`, `not_leg`, `extra`, `he100_fer` | `fonte_da_regua(m)`: motor/porta CCT | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:157`, `:160` |
| C02 | `trab`, `not_rel`, `not_leg`, `extra` | `fonte_da_regua(m)` | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:179`, `:181` |
| C03 | `trab`, `not_rel`, `not_leg`, `extra` | `fonte_da_regua(m)` | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:203`, `:205` |
| C04 | `trab`, `not_rel`, `not_leg`, `extra` | `fonte_da_regua(m)` | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:229`, `:231` |
| C05 | `trab`, `not_rel`, `not_leg`, `extra` | `fonte_da_regua(m)` e `regua_para(...)[regua_excedente]`; não são saída de `reler` | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:254`, `:256`, `:257` |
| C06 | Nenhuma: excluído do arnês | Caso original dá skip por pergunta de lei | `app/ponto/tests/test_v3_e1_valor.py:57`; `app/ponto/tests/test_e1_lote1_valor_do_dia.py:274` |
| C07 | `trab`, `he100_fer` | `fonte_da_regua(m)` | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:296`, `:298` |
| C08 | `trab`, `he100_fer`, `extra` | `fonte_da_regua(m)` | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:315`, `:317` |
| C09 | `trab`, `not_rel`, `not_leg`, `intra_descontada`, `extra` | `fonte_da_regua(m)` | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:342`, `:345` |
| C10 | `trab`, `not_rel`, `not_leg`, `intra_descontada`, `extra`, `he100_fer` | Nenhuma adicional | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:363` |
| C11 | `trab`, `not_rel`, `not_leg`, `extra` | Nenhuma adicional | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:382` |
| C12 | `trab`, `he50`, `he100_fer` | Nenhuma adicional | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:402` |
| C13 | `trab`, `extra`, `he50`, `he100` | Nenhuma adicional | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:419` |
| C14 | `trab`, `atraso`, `antecipada`, `extra` | Nenhuma adicional | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:438` |
| C15 | `atraso`, `antecipada`, `extra` | Nenhuma adicional | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:457` |
| C16 | Nenhuma: excluído do arnês | Caso original dá skip por pergunta de lei | `app/ponto/tests/test_v3_e1_valor.py:57`; `app/ponto/tests/test_e1_lote1_valor_do_dia.py:475` |
| C17 | `trab`, `intra_descontada`, `intra_indenizada`, `extra`, `he50`, `he100` | Nenhuma adicional | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:499` |
| C18 | `trab`, `extra`, `he50`, `he100` | Nenhuma adicional | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:519` |
| C19 | `ft_min`; `extra`/`he100_fer` têm a ressalva abaixo | `ft`: quantidade/classificação dos períodos FT | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:539` |
| C20 | `trab`, `antecipada`, `atraso`, `extra` | Nenhuma adicional | `app/ponto/tests/test_e1_lote1_valor_do_dia.py:557` |
| L01 | `trab`, `atraso`, `antecipada` | `r.dias_cadastro_x_realidade`; não consulta `censo.cxr` | `app/ponto/tests/test_e1_lote3.py:364`, `:365` |
| L13 | `trab`, `extra`, `he50`, `he100`, `atraso`, `antecipada` | `ft`; pertencimento a `previstas_do_periodo` é outra autoridade | `app/ponto/tests/test_e1_lote3.py:567`, `:570` |
| L14 | `trab`, `extra`, `he50`, `he100` | `ft` | `app/ponto/tests/test_e1_lote3.py:588` |
| L15 | `trab + ft_min`; `atraso`/`antecipada` recebem zero por exclusão legal | `faltas` | `app/ponto/tests/test_e1_lote3.py:608`, `:609`; `app/ponto/calculador/regras.py:494`; `app/ponto/calculador/periodo.py:464`, `:639`, `:809` |
| L16 | Sem batidas/períodos, as somas `trab`, `ft_min`, `atraso`, `antecipada` podem ser zero sem escrita alguma | `faltas`, `horas_falta` | `app/ponto/tests/test_e1_lote3.py:614`, `:621`; `app/ponto/tests/test_o280_bateria_da_lei.py:166` |
| L17 | `trab`, `he100_fer`, `he50` | `ft`; conjunto de feriados é verificado separadamente | `app/ponto/tests/test_e1_lote3.py:649`, `:650` |
| L18 | `trab`, `he100_fer`, `he50`, `he100`, `extra` | `ft`; conjunto de feriados é verificado separadamente | `app/ponto/tests/test_e1_lote3.py:667`, `:668` |
| L19 | `trab` | `semanas_dsr_ok`, `semanas_dsr_perdido` | `app/ponto/tests/test_e1_lote3.py:692`, `:693`, `:695` |
| L20 | `trab` | `semanas_dsr_ok`, `semanas_dsr_perdido` | `app/ponto/tests/test_e1_lote3.py:714`, `:715`, `:717` |

Duas correções importantes à apresentação do próprio arnês:

- **`ft_min` não passa intocado:** `leitura` soma `minutos_trabalhados` dos FT; `reler` percorre também FT e reescreve esse campo. A classificação na docstring do arnês está errada. (`app/ponto/tests/test_o280_bateria_da_lei.py:174`; `app/ponto/calculador/periodo.py:606`, `:742`; `app/ponto/tests/test_v3_e1_valor.py:18`)
- **C19 não verifica a HE do objeto FT:** `extra` e `he100_fer` de `leitura` somam somente períodos normais. Quando só existe FT, esses zeros vêm de soma vazia, embora `reler` também zere HE nos FT. (`app/ponto/tests/test_o280_bateria_da_lei.py:164`, `:167`, `:179`, `:182`; `app/ponto/calculador/periodo.py:758`, `:767`)

Além disso, `RUBRICAS_DA_V3` é declarada, mas o veredito/log não a consulta; não implementa a separação prometida das duas origens. (`app/ponto/tests/test_v3_e1_valor.py:20`, `:50`, `:155`, `:159`)

**2) Falha perdida e erro convertido em skip**

**Sim, há asserção perdida no modo `propriedade`.** Tanto `subTest` quanto `_caso` capturam `self.failureException`, mas só registram a falha quando `MODO == 'valor'`. Havendo `_censos` e estando `_props` vazio, o caso termina com sucesso, sem fail nem skip. Isso inclui asserções fora de `subTest`. ([test_v3_e1_valor.py:135](/home/ronald/codex_ws/2258f914afd6597484901ae7cde3cf91cf6a0ace/app/ponto/tests/test_v3_e1_valor.py:135), [test_v3_e1_valor.py:146](/home/ronald/codex_ws/2258f914afd6597484901ae7cde3cf91cf6a0ace/app/ponto/tests/test_v3_e1_valor.py:146), [test_v3_e1_valor.py:149](/home/ronald/codex_ws/2258f914afd6597484901ae7cde3cf91cf6a0ace/app/ponto/tests/test_v3_e1_valor.py:149))

No modo `valor`, a asserção capturada termina em fail se VALIDADO ou skip se PENDENTE. A lista atual está vazia, portanto nenhuma divergência de valor desses casos bloqueia por validação. (`app/ponto/tests/test_v3_e1_valor.py:47`, `:137`, `:148`, `:162`)

**Não há, nesses dois métodos, conversão de exceção comum em skip.** Exceções diferentes de `self.failureException` e `SkipTest` propagam; `SkipTest` é relançado. (`app/ponto/tests/test_v3_e1_valor.py:131`, `:143`)

Há ainda VERDE de valor sem releitura: `_caso` aceita coleta vazia, imprime `sem releitura` e retorna. O MORDE exercita explicitamente um caso vazio bem-sucedido. (`app/ponto/tests/test_v3_e1_valor.py:155`, `:276`)

**3) As propriedades conseguem ficar vermelhas, mas têm pontos cegos distintos**

As mutações abaixo são exemplos derivados do fluxo, não alterações aplicadas aos arquivos. (`app/ponto/tests/test_v3_e1_valor.py:171`)

| Propriedade | Como pode ficar vermelha | Mutação em `reler` que ela não detecta |
|---|---|---|
| **P-SOMA** | Escrever `he50 = x.extra_50_min + 1` quebra `extra == he50 + he100`; a falha entra em `_props` e bloqueia. (`app/ponto/calculador/periodo.py:846`; `app/ponto/tests/test_v3_e1_valor.py:102`, `:153`) | Zerar conjuntamente `extra`, `he50`, `he100` e `he100_fer`: os valores podem estar errados, mas preservam as duas relações verificadas. (`app/ponto/tests/test_v3_e1_valor.py:98`, `:105`) |
| **P-IDEM** | Trocar a atribuição de `minutos_trabalhados` por `+=` muda o segundo estado observado. (`app/ponto/calculador/periodo.py:742`; `app/ponto/tests/test_v3_e1_valor.py:172`) | Fixar `minutos_trabalhados = 123.0`: erro estável passa. Também passaria incrementar `resultado.dias_falta`, pois esse atributo não integra o snapshot nem o Censo. (`app/ponto/tests/test_v3_e1_valor.py:172`; `app/ponto/tests/test_o280_bateria_da_lei.py:4325`, `:4350`) |
| **P-PREV** | Pode falhar se a régua externa observada mudar entre os snapshots, por exemplo por uma mutação que altere o cadastro consultado. (`app/ponto/tests/test_v3_e1_valor.py:176`; `app/ponto/calculador/periodo.py:518`) | Usar internamente `dataclasses.replace(regua_de(dia), previsto_min=0)` no lugar da régua recebida: o cálculo usa outro previsto, mas a comparação externa continua consultando o mesmo cadastro. (`app/ponto/calculador/periodo.py:614`, `:819`; `app/ponto/tests/test_v3_e1_valor.py:177`) |

P-IDEM observa rubricas arredondadas a duas casas e compara primeira v3 contra segunda v3; não valida correção contra um oráculo. P-PREV não compara a régua efetivamente utilizada dentro de `reler`. (`app/ponto/tests/test_o280_bateria_da_lei.py:4354`; `app/ponto/tests/test_v3_e1_valor.py:172`, `:177`)

No lote 3, P-PREV só compara dias que receberam snapshot antes de `_bater`; em L16 não há `_bater`, logo nenhuma igualdade dessa propriedade é cobrada. Só P-SOMA tem MORDE dedicado neste arquivo, usando objetos artificiais. (`app/ponto/tests/test_v3_e1_valor.py:215`, `:236`, `:287`; `app/ponto/tests/test_e1_lote3.py:621`)

**4) Escritores e leitores**

| Atributo observado | Escritores | Leitores |
|---|---|---|
| `minutos_extra`, `_50`, `_100`, `_100_feriado` | V2 escreve excesso/dobra; v3 zera os pares julgados e escreve o portador do dia. (`app/ponto/motor_calculo_v2.py:3031`, `:3078`; `app/ponto/calculador/periodo.py:758`, `:838`, `:845`) | Arnês: P-SOMA, `leitura`, snapshot. Produção: propriedades mensais, `dia_pago`, espelho e fechamento. (`app/ponto/tests/test_v3_e1_valor.py:98`; `app/ponto/tests/test_o280_bateria_da_lei.py:179`, `:4328`; `app/ponto/motor_calculo_v2.py:274`; `app/ponto/services/dia_pago.py:180`; `app/ponto/services/espelho.py:112`; `app/ponto/services/fechamento.py:894`) |
| `minutos_extra_50_noturna`, `_100_noturna` | V2, em `_aplicar_he_noturna`; v3 não os reescreve. (`app/ponto/motor_calculo_v2.py:952`, `:958`, `:960`; `app/ponto/calculador/periodo.py:101`) | **Não entram no arnês**, inclusive no snapshot P-IDEM. Produção os lê nas propriedades mensais, `dia_pago` e fechamento. (`app/ponto/tests/test_v3_e1_valor.py:98`; `app/ponto/tests/test_o280_bateria_da_lei.py:4325`; `app/ponto/motor_calculo_v2.py:288`; `app/ponto/services/dia_pago.py:181`; `app/ponto/services/fechamento.py:896`) |
| `minutos_atraso`, `minutos_saida_antecipada` | V2 escreve; v3 zera quando recebe `marcos` e escreve o portador. (`app/ponto/motor_calculo_v2.py:2157`, `:2164`; `app/ponto/calculador/periodo.py:639`, `:809`) | `leitura`, snapshot; produção: propriedades mensais, espelho e fechamento. (`app/ponto/tests/test_o280_bateria_da_lei.py:183`, `:4329`; `app/ponto/motor_calculo_v2.py:297`, `:301`; `app/ponto/services/espelho.py:115`; `app/ponto/services/fechamento.py:969`) |
| `minutos_trabalhados`, `intra_descontada`, `minutos_intrajornada_indenizada` | V2 origina; v3 substitui nos pares relidos, inclusive FT. (`app/ponto/motor_calculo_v2.py:2713`, `:2722`, `:2723`; `app/ponto/calculador/periodo.py:742`) | `leitura`, snapshot, propriedades mensais e `dia_pago`. (`app/ponto/tests/test_o280_bateria_da_lei.py:173`, `:4326`; `app/ponto/motor_calculo_v2.py:245`, `:249`; `app/ponto/services/dia_pago.py:178`, `:186`) |
| `minutos_noturnos_relogio`, `minutos_noturnos_legais` | V2 origina; v3 substitui com `cct` nos pares relidos. (`app/ponto/motor_calculo_v2.py:941`; `app/ponto/calculador/periodo.py:745`, `:752`) | `leitura` e snapshot; produção soma o legal nas propriedades mensais e em `dia_pago`. (`app/ponto/tests/test_o280_bateria_da_lei.py:177`, `:4327`; `app/ponto/motor_calculo_v2.py:271`; `app/ponto/services/dia_pago.py:179`) |
| `minutos_fora_janela` | V2 escreve; v3 lê para o teto e só zera na cópia duplicada. (`app/ponto/motor_calculo_v2.py:2725`; `app/ponto/calculador/periodo.py:184`, `:736`, `:786`) | `leitura`, snapshot e alimentação do calculador. (`app/ponto/tests/test_o280_bateria_da_lei.py:185`, `:4327`; `app/ponto/calculador/alimentacao.py:83`) |
| Listas normal/FT, pontas e `turno_aberto` | V2 constrói/classifica a geometria; v3 a percorre. (`app/ponto/motor_calculo_v2.py:160`, `:1778`, `:2430`; `app/ponto/calculador/periodo.py:606`, `:644`) | `leitura` conta períodos/abertos; snapshot registra pontas e cesta FT. (`app/ponto/tests/test_o280_bateria_da_lei.py:170`, `:4345`, `:4348`) |
| `hora_extra_100` | V2 escreve; v3 redefine como falso e liga na dobra. (`app/ponto/motor_calculo_v2.py:3034`, `:3084`; `app/ponto/calculador/periodo.py:772`, `:842`) | Snapshot P-IDEM. (`app/ponto/tests/test_o280_bateria_da_lei.py:4353`) |
| Faltas, abonos, horas de falta, CxR do resultado e semanas DSR | V2 escreve; não são reescritos por `reler`. (`app/ponto/motor_calculo_v2.py:2549`, `:2556`, `:2558`, `:2657`, `:3375`, `:3400`, `:3417`; `app/ponto/calculador/periodo.py:540`) | `leitura` e asserções diretas dos lotes. (`app/ponto/tests/test_o280_bateria_da_lei.py:186`; `app/ponto/tests/test_e1_lote3.py:365`, `:693`, `:715`) |

`estado_dos_periodos` **é uma função de teste**, não um atributo de produção. Ela constrói um multiset ordenado com pontas, cesta FT, aberto, booleano HE100 e 12 rubricas; não observa todo o `ResultadoMes`. (`app/ponto/tests/test_o280_bateria_da_lei.py:4325`, `:4333`, `:4344`)

Para **todos os campos do Censo**, o escritor funcional é `reler`, com uma instância nova por chamada:

| Campos | Escrita em `app/ponto/calculador/periodo.py` |
|---|---|
| `cct_medida`, `pontualidade_medida` | `:592`, `:604` |
| `sem_dia`, `abertos`, `sem_carimbo` | `:610`, `:642`, `:646` |
| `efetivo_invertido`, `invertidos` | `:697`, `:698` |
| `par_duplicado`, `duplicados` | `:734`, `:735` |
| `periodos_noturno`, `relidos`, `dias_ft` | `:754`, `:774`, `:776` |
| `dias_sem_pontualidade_por_lei`, `cxr` | `:789`, `:790` |
| `dias_sem_pontualidade_por_par_nao_lido`, `atraso_em_dia_so_ft`, `dias_pontualidade` | `:804`, `:807`, `:812` |
| `dias`, `dias_sem_previsto_com_trabalho`, `dias_feriado_dobra` | `:815`, `:821`, `:843` |

O arnês imprime seis campos do Censo e compara **todos** via `dataclasses.asdict` em P-IDEM. Não exige flags verdadeiras nem contadores de ressalva zerados para aceitar o valor. (`app/ponto/tests/test_v3_e1_valor.py:110`, `:155`, `:172`)

**Não há leitor de produção identificado de `reler`.** O contrato o declara ensaio; os chamadores encontrados são o arnês e a bateria de testes. Os leitores produtivos dos atributos acima não demonstram integração da v3. ([periodo.py:543](/home/ronald/codex_ws/2258f914afd6597484901ae7cde3cf91cf6a0ace/app/ponto/calculador/periodo.py:543), `app/ponto/tests/test_v3_e1_valor.py:83`, `:88`; `app/ponto/tests/test_o280_bateria_da_lei.py:3307`, `:3310`, `:4375`, `:4382`)

**5) C17 versus limite acrescido do intervalo**

C17 afirma `trab=540`, `intra_indenizada=60`, `extra=he50=60` para COM 08–17 sem pausa. Com os mesmos números, a fórmula atual dá `limite=480+min(60,60)=540`, portanto `extra=0`. É uma incompatibilidade de expectativa, inferida diretamente da fórmula. ([test_e1_lote1_valor_do_dia.py:499](/home/ronald/codex_ws/2258f914afd6597484901ae7cde3cf91cf6a0ace/app/ponto/tests/test_e1_lote1_valor_do_dia.py:499), [periodo.py:410](/home/ronald/codex_ws/2258f914afd6597484901ae7cde3cf91cf6a0ace/app/ponto/calculador/periodo.py:410), [periodo.py:414](/home/ronald/codex_ws/2258f914afd6597484901ae7cde3cf91cf6a0ace/app/ponto/calculador/periodo.py:414))

| Comportamento afirmado | Testes | [nao verificado]
|---|---|
| Os 60 minutos sem pausa também geram HE no COM 08–17 | **C17**, exigindo intra60 e extra/he50=60. `app/ponto/tests/test_e1_lote1_valor_do_dia.py:499` |
| Acrescentar o suprimido ao limite do COM | **A6b**, COM 08–22: trab840, intra60, extra300, correspondente a limite540. `app/ponto/tests/test_o280_bateria_da_lei.py:3611`, `:3624` |
| Fórmula explicitamente testada | **`test_v3_excesso_do_dia_e_PURO_e_a_reparticao_SOMA`**: previsto480/declarado60; trab630/suprimido30 exige limite510/extra120. `app/ponto/tests/test_o280_bateria_da_lei.py:3660`, `:3672` |
| Mesma regra integrada com pausa parcial | **F7d**: trab630, intra30, extra120. `app/ponto/tests/test_o280_bateria_da_lei.py:3518`, `:3521` |
| Sem pausa, intra não gera HE correspondente no D12 | **`test_RED_doze_horas_sem_intervalo_NAO_tem_hora_extra`**: 07–19, intra60, extra0. `app/ponto/tests/test_he_intra_dobrada.py:77`, `:83` |
| Excedente além desse limite continua pago | **`test_MORDE_treze_horas_sem_intervalo_TEM_uma_hora_de_extra`**: 07–20, intra60, extra60. `app/ponto/tests/test_he_intra_dobrada.py:88` |
| Pausa gozada não aumenta o limite | **`test_MORDE_intervalo_gozado_nao_ganha_folga_no_limite`**: intra0, extra60. `app/ponto/tests/test_he_intra_dobrada.py:95`, `:102` |

O antigo `test_comercial_sem_intra_registrada` também usa 08–17, mas só exige intra positiva: não decide HE60 versus HE0. (`app/ponto/tests/test_motor.py:1041`, `:1045`, `:1049`)

No arnês atual, a divergência de valor de C17 vira **PENDENTE/skip**; já suas identidades e idempotência podem passar com HE zero. (`app/ponto/tests/test_v3_e1_valor.py:47`, `:102`, `:162`, `:172`)

## 30/10 23:56 · sessao 1b93bacd51ca · - · - · origem=Ronald (tmux)

Q4 — mapa consolidado, somente por leitura:

| Atributo observado | Quem escreve | Quem lê no arnês / produção |
|---|---|---|
| `minutos_extra`, `_50`, `_100`, `_100_feriado` | Defaults do `PeriodoCalculo`; motor v2 escreve, por exemplo, dobra e excesso; `reler` zera pares julgados e escreve o último portador. (`app/ponto/motor_calculo_v2.py:166`, `:3031`, `:3078`; `app/ponto/calculador/periodo.py:758`, `:767`, `:838`, `:845`) | P-SOMA lê os quatro; `leitura` soma os quatro; snapshot P-IDEM inclui os quatro. Produção: propriedades do `ResultadoMes`, `dia_pago`, `espelho`, fechamento. (`app/ponto/tests/test_v3_e1_valor.py:98`; `app/ponto/tests/test_o280_bateria_da_lei.py:179`, `:4328`; `app/ponto/motor_calculo_v2.py:274`; `app/ponto/services/dia_pago.py:180`; `app/ponto/services/espelho.py:112`; `app/ponto/services/fechamento.py:894`) |
| `minutos_extra_50_noturna`, `_100_noturna` | Motor v2 em `_aplicar_he_noturna`, com resets mensais; `reler` não escreve esses campos. (`app/ponto/motor_calculo_v2.py:952`, `:958`, `:960`, `:2875`; `app/ponto/calculador/periodo.py:101`) | **Não são observados pelo arnês**, nem P-SOMA, nem `leitura`, nem snapshot P-IDEM. Produção os lê nas propriedades mensais e em `dia_pago`/fechamento. (`app/ponto/tests/test_v3_e1_valor.py:98`; `app/ponto/tests/test_o280_bateria_da_lei.py:179`, `:4325`; `app/ponto/motor_calculo_v2.py:288`; `app/ponto/services/dia_pago.py:181`; `app/ponto/services/fechamento.py:896`) |
| `minutos_atraso`, `minutos_saida_antecipada` | Motor v2 e seu teto; `reler` zera se recebeu `marcos`, depois escreve o portador. (`app/ponto/motor_calculo_v2.py:2157`, `:2164`, `:1033`, `:1132`; `app/ponto/calculador/periodo.py:634`, `:639`, `:809`) | `leitura` e snapshot; produção: propriedades mensais, espelho, crédito parcial e fechamento. (`app/ponto/tests/test_o280_bateria_da_lei.py:183`, `:4329`; `app/ponto/motor_calculo_v2.py:297`, `:301`; `app/ponto/services/espelho.py:115`; `app/ponto/services/credito_parcial.py:36`; `app/ponto/services/fechamento.py:969`) |
| Trabalhado, intra descontada/indenizada | Motor v2 origina; `reler` substitui nos pares julgados, inclusive FT. (`app/ponto/motor_calculo_v2.py:2713`, `:2722`, `:2723`; `app/ponto/calculador/periodo.py:606`, `:742`) | `leitura`, snapshot; propriedades mensais e `dia_pago`. **`ft_min` é soma do trabalhado FT reescrito pela v3**, embora o docstring do arnês diga que passa intocado. (`app/ponto/tests/test_o280_bateria_da_lei.py:173`, `:174`, `:4326`; `app/ponto/motor_calculo_v2.py:245`, `:249`; `app/ponto/services/dia_pago.py:178`, `:186`; `app/ponto/tests/test_v3_e1_valor.py:18`) |
| Noturno relógio/legal | V2 escreve; v3 substitui apenas com `cct` e par relido. (`app/ponto/motor_calculo_v2.py:941`; `app/ponto/calculador/periodo.py:745`, `:752`) | `leitura`, snapshot; produção lê legal para total mensal e dia pago. (`app/ponto/tests/test_o280_bateria_da_lei.py:177`, `:4327`; `app/ponto/motor_calculo_v2.py:271`; `app/ponto/services/dia_pago.py:179`) |
| `minutos_fora_janela` | Motor v2 escreve; v3 só zera na cópia duplicada e lê para teto de pontualidade. (`app/ponto/motor_calculo_v2.py:1410`, `:2725`; `app/ponto/calculador/periodo.py:184`, `:736`, `:786`) | `leitura`, snapshot; alimentação do calculador. (`app/ponto/tests/test_o280_bateria_da_lei.py:185`, `:4327`; `app/ponto/calculador/alimentacao.py:83`) |
| Pontas, `turno_aberto`, `hora_extra_100` | Motor v2 origina pontas/aberto; v3 redefine o booleano HE100, ligado somente na dobra. (`app/ponto/motor_calculo_v2.py:160`, `:1778`, `:2691`; `app/ponto/calculador/periodo.py:772`, `:842`) | Snapshot lê pontas efetivas com fallback cru, FT, aberto e HE100. Snapshot é helper de teste, não atributo produtivo. (`app/ponto/tests/test_o280_bateria_da_lei.py:4333`, `:4345`, `:4348`, `:4353`) |

`estado_dos_periodos` constrói um multiset ordenado: duas pontas em epoch, identificação FT, aberto, HE100 e 12 rubricas arredondadas para duas casas. Ele não cobre os campos HE noturna nem alertas, intervalos ou outros dados do resultado. (`app/ponto/tests/test_o280_bateria_da_lei.py:4325`, `:4344`, `:4350`, `:4354`)

`Censo` tem defaults declarados em `app/ponto/calculador/periodo.py:225` e cada chamada cria nova instância em `:591`. Seus escritores funcionais estão em `reler`:

- `cct_medida`: `:592`; `pontualidade_medida`: `:604`; `sem_dia`: `:610`; `abertos`: `:642`; `sem_carimbo`: `:646`.
- `efetivo_invertido` / `invertidos`: `:697`; `par_duplicado` / `duplicados`: `:734`; `periodos_noturno`: `:754`; `relidos`: `:774`; `dias_ft`: `:776`.
- `dias_sem_pontualidade_por_lei`: `:789`; `cxr`: `:790`; `dias_sem_pontualidade_por_par_nao_lido`: `:804`; `atraso_em_dia_so_ft`: `:807`; `dias_pontualidade`: `:812`.
- `dias`: `:815`; `dias_sem_previsto_com_trabalho`: `:821`; `dias_feriado_dobra`: `:843`.

O arnês imprime seis campos (`cct_medida`, `pontualidade_medida`, `relidos`, `abertos`, `sem_dia`, `sem_carimbo`) e compara **todos** via `dataclasses.asdict` no P-IDEM; não exige qualquer contador igual a zero ou flag verdadeira antes de anunciar VERDE. (`app/ponto/tests/test_v3_e1_valor.py:110`, `:155`, `:172`)

Não foi encontrado leitor produtivo de `reler` nas buscas em `.py` do repositório; os importadores/chamadores encontrados são os dois arquivos de teste. O próprio contrato o declara ensaio sem caminho de produção. Os leitores produtivos dos atributos acima recebem os resultados do motor v2. (`app/ponto/tests/test_v3_e1_valor.py:83`, `:88`, `:170`, `:173`; `app/ponto/tests/test_o280_bateria_da_lei.py:4375`, `:4382`; `app/ponto/calculador/periodo.py:543`)

Observação adicional: `RUBRICAS_DA_V3` é apenas definida; `_caso` não a consulta ao imprimir log ou falhas. A prometida separação das “duas cestas no texto” não está implementada. (`app/ponto/tests/test_v3_e1_valor.py:20`, `:50`, `:155`, `:159`)

## 30/10 23:56 · sessao 3dcaa8a24202 · - · - · origem=Ronald (tmux)

**2. Há perda de asserção no modo propriedade.** Com `_coleta` ativa, `subTest` captura `self.failureException`, mas só registra em `MODO == 'valor'`; em propriedade, descarta. O mesmo ocorre para asserção fora de `subTest`. Se já houve releitura e `_props` está vazio, `_caso` retorna sem fail nem skip (`app/ponto/tests/test_v3_e1_valor.py:125`, `app/ponto/tests/test_v3_e1_valor.py:135`, `app/ponto/tests/test_v3_e1_valor.py:146`, `app/ponto/tests/test_v3_e1_valor.py:149`).

Em modo valor, falhas capturadas entram na coleta e terminam em fail para VALIDADO ou skip para PENDENTE; atualmente `VALIDADOS = {}` (`app/ponto/tests/test_v3_e1_valor.py:47`, `app/ponto/tests/test_v3_e1_valor.py:137`, `app/ponto/tests/test_v3_e1_valor.py:148`, `app/ponto/tests/test_v3_e1_valor.py:162`). Exceções que não sejam `self.failureException` ou `SkipTest` não são capturadas: propagam como erro, sem conversão em skip (`app/ponto/tests/test_v3_e1_valor.py:131`, `app/ponto/tests/test_v3_e1_valor.py:143`).

O marcador de valor também admite VERDE sem releitura, desde que não tenha asserções falhadas: imprime `sem releitura` e retorna; o próprio MORDE exercita caso vazio verde (`app/ponto/tests/test_v3_e1_valor.py:155`, `app/ponto/tests/test_v3_e1_valor.py:276`).

**3. As propriedades conseguem falhar, com limites:**

| Propriedade | Pode ficar vermelha | Mutação despercebida |
|---|---|---|
| P-SOMA | Sim: divergência extra versus he50+he100, ou feriado acima de he100, entra em `_props` e causa fail. Trocar a atribuição de he50 por `x.extra_50_min + 1` seria detectável (`app/ponto/tests/test_v3_e1_valor.py:98`, `app/ponto/tests/test_v3_e1_valor.py:152`, `app/ponto/calculador/periodo.py:846`). | Zerar conjuntamente extra/he50/he100/he100_fer: os valores permanecem coerentes, embora errados. Só identidades são verificadas (`app/ponto/tests/test_v3_e1_valor.py:102`, `app/ponto/tests/test_v3_e1_valor.py:105`). |
| P-IDEM | Sim: trocar `p.minutos_trabalhados = d.minutos_trabalhados` por `+=` altera o segundo estado e seria detectado (`app/ponto/calculador/periodo.py:742`, `app/ponto/tests/test_v3_e1_valor.py:172`). | Fixar `p.minutos_trabalhados = 123.0` dá erro determinístico e idempotente. Compara primeira v3 com segunda v3, sem oráculo de correção (`app/ponto/tests/test_v3_e1_valor.py:172`). Alterações inferiores à precisão observada podem desaparecer: o estado arredonda rubricas a duas casas (`app/ponto/tests/test_o280_bateria_da_lei.py:4354`). |
| P-PREV | Sim, se a régua externa observada mudar, por exemplo se `reler` passar a alterar cadastro. Não verifica a régua realmente usada dentro de `reler` (`app/ponto/tests/test_v3_e1_valor.py:176`, `app/ponto/calculador/periodo.py:513`). | Em `reler`, substituir a régua recebida por `dataclasses.replace(regua_de(dia), previsto_min=0)` deixa o cadastro externo intacto e passa na comparação, embora o cálculo use outro previsto (`app/ponto/calculador/periodo.py:614`, `app/ponto/tests/test_v3_e1_valor.py:177`). |

No lote 3, P-PREV só compara dias com snapshot anterior; sem `_bater`, como L16, `_reguas_antes` fica vazio e nenhuma igualdade é verificada (`app/ponto/tests/test_v3_e1_valor.py:215`, `app/ponto/tests/test_v3_e1_valor.py:236`, `app/ponto/tests/test_v3_e1_valor.py:177`, `app/ponto/tests/test_e1_lote3.py:621`).

Só P-SOMA tem MORDE dedicado neste arquivo; ele testa o helper com objetos artificiais (`app/ponto/tests/test_v3_e1_valor.py:287`).

## 30/10 23:55 · sessao 2331bd004654 · - · - · origem=Ronald (tmux)

P5 — há conflito de expectativas, não apenas diferença de nomes: C17 fixa COM `08–17`, pausa declarada `12–13`, previsto 480, sem pausa gozada, e exige `trab=540`, `intra_indenizada=60`, `extra=he50=60` (`app/ponto/tests/test_e1_lote1_valor_do_dia.py:72`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:482`, `app/ponto/tests/test_e1_lote1_valor_do_dia.py:499`).

Para esses números, `excesso_do_dia` produz limite `480 + min(60,60)=540` e `extra=0`: conclusão por leitura estática do corpo (`app/ponto/calculador/periodo.py:410`, `app/ponto/calculador/periodo.py:414`, `app/ponto/calculador/periodo.py:416`).

Testes que afirmam os dois comportamentos: [nao verificado]

| Comportamento | Teste e evidência | [nao verificado]
|---|---|
| HE paga também os 60 minutos sem pausa de COM `08–17` | `test_E1_C17_comercial_sem_intervalo_indeniza_sessenta`: exige extra/he50=60 além da intra=60 (`app/ponto/tests/test_e1_lote1_valor_do_dia.py:499`). |
| Acrescentar suprimido ao limite de COM | `test_v3_A6_e_A6b_a_hora_acima_do_limite_legal_segue_DEVIDA`: COM `08–22`, trab840/intra60/extra300, isto é, limite540 (`app/ponto/tests/test_o280_bateria_da_lei.py:3611`, `app/ponto/tests/test_o280_bateria_da_lei.py:3624`). |
| Fórmula `previsto + min(suprimido,declarado)` explicitamente | `test_v3_excesso_do_dia_e_PURO_e_a_reparticao_SOMA`: com previsto480/declarado60 e trab630/suprimido30, exige limite510/extra120 (`app/ponto/tests/test_o280_bateria_da_lei.py:3660`, `app/ponto/tests/test_o280_bateria_da_lei.py:3672`). |
| Mesma fórmula integrada, pausa parcial | F7d: COM trab630, intra30, extra120 (`app/ponto/tests/test_o280_bateria_da_lei.py:3511`, `app/ponto/tests/test_o280_bateria_da_lei.py:3518`). |
| Sem pausa não gera HE correspondente à intra, em D12 | `test_RED_doze_horas_sem_intervalo_NAO_tem_hora_extra`: `07–19`, intra60 e extra0 (`app/ponto/tests/test_he_intra_dobrada.py:77`, `app/ponto/tests/test_he_intra_dobrada.py:83`). |
| Excedente além do limite continua pago, em D12 | `test_MORDE_treze_horas_sem_intervalo_TEM_uma_hora_de_extra`: `07–20`, intra60/extra60 (`app/ponto/tests/test_he_intra_dobrada.py:88`). |
| Pausa gozada não aumenta limite | `test_MORDE_intervalo_gozado_nao_ganha_folga_no_limite`: intra0/extra60 (`app/ponto/tests/test_he_intra_dobrada.py:95`). |

O teste antigo `test_comercial_sem_intra_registrada` usa exatamente batidas `08–17`, porém só afirma `intra > 0`; não decide HE60 versus HE0 (`app/ponto/tests/test_motor.py:1041`, `app/ponto/tests/test_motor.py:1045`, `app/ponto/tests/test_motor.py:1049`).

C17 também é herdado pelas classes de valor e propriedade do arnês; com `VALIDADOS={}`, eventual divergência de valor torna-se skip, enquanto o modo propriedade não cobra o valor esperado da rubrica (`app/ponto/tests/test_v3_e1_valor.py:47`, `app/ponto/tests/test_v3_e1_valor.py:146`, `app/ponto/tests/test_v3_e1_valor.py:149`, `app/ponto/tests/test_v3_e1_valor.py:164`, `app/ponto/tests/test_v3_e1_valor.py:249`).

## 30/10 23:34 · sessao 6f377c5d4b5c · DISJUNTOR · verificador · origem=Code

O desenho precisa corrigir **a fonte do B4, a identificação da volta e a integração do piso com o Stop**. O leitor atual não publica os campos necessários ao orçamento diário, e o avaliador de contexto ainda aplica os três gatilhos do v2 (`bin/codex_limite_estado.py:118`; `bin/orcamento_contexto.py:209`).

Abaixo, distingo defeitos presentes de cenários condicionais de implementação. Para `contrato.txt`, numerei o cabeçalho como linha 1, as cláusulas como linhas 2–6 e o MEDIDO como linha 7.

1. **Item 1 — o texto Bash não basta para determinar papel e volta.**  
   O wrapper recebe `CODEX_PAPEL` do **ambiente efetivo**; suas linhas de registro não contêm `CODEX_VOLTA` (`bin/codex_pergunta.sh:44`; `bin/codex_pergunta.sh:53`; `bin/codex_pergunta.sh:88`). Cenários:

   - Papel herdado ou `CODEX_PAPEL="$P"` não pode ser resolvido apenas procurando atribuições literais.
   - FINAL declarada em ambiente herdado, sem atribuição visível no comando, vira `meio` pela regra proposta: **nega uma FINAL legítima** quando o orçamento diário estiver estourado.
   - Uma busca textual pode aproveitar `CODEX_VOLTA=final` de comentário, string ou outra invocação e **liberar uma chamada do meio**.
   - Um Bash contendo duas chamadas, uma permitida e outra proibida, exige decisões por invocação; uma única classificação do texto inteiro não resolve essa diferença. O parser e essa integração ainda são **[não verificado]**; a autoridade atual do papel é o ambiente recebido pelo wrapper (`bin/codex_pergunta.sh:44`; `contrato.txt:2`).

2. **Item 3 — a fonte “que o ESTADO já publica” não fornece o orçamento diário.**  
   `ler_semanal()` devolve somente `plan_type`, `used_percent` e `window_minutes`: descarta `resets_at`, timestamp e histórico. Portanto, consumir sua saída atual resulta sempre em “sem resets_at/sem registro de hoje”; pelo contrato, **libera o meio mesmo quando deveria negar**. É preciso ampliar esse leitor para obter primeiro/último registro do dia e reset correspondente (`bin/codex_limite_estado.py:99`; `bin/codex_limite_estado.py:118`; `contrato.txt:4`).

3. **Item 3 — “último” atualmente significa arquivo com maior mtime.**  
   O leitor ordena arquivos por mtime, examina apenas quarenta e escolhe a última linha contendo `rate_limits` no primeiro arquivo aproveitável; não compara timestamps das medições. Um arquivo com uso antigo de 10% tocado depois de outro com 86% **abre indevidamente**; o inverso pode **negar inclusive FINAL**, ao aplicar a restrição de 85% sobre dado velho. Um registro relevante no 41º arquivo também fica invisível (`bin/codex_limite_estado.py:105`; `bin/codex_limite_estado.py:110`; `contrato.txt:4`).

4. **Item 3 — faltam regras para reset e precedência.**  
   No dado anexado, 54−12 = 42 pp, acima dos 13,3 pp/dia e abaixo de 85%: esse caso exige negar meio e preservar FINAL de verificador (`contrato.txt:4`; `contrato.txt:7`). Mas o contrato não fecha estes casos:

   - Reset durante o dia: primeiro registro 90%, último 5% produz −85 pp; a subtração atravessou duas janelas.
   - Orçamento estourado e uso ≥85%: `escada` de papel `leitura` satisfaz “todo papel”, mas não “papel verificador”, se ambas as restrições forem acumuladas.
   - Uso ≥85% sem `resets_at`: conflitam “só escada” e “não sei, não nega”.

   Esses resultados precisam de precedência explícita; o leitor atual nem conserva o reset para separar as janelas (`contrato.txt:4`; `bin/codex_limite_estado.py:118`).

5. **Item 3 — ANEXO-ENXUTO por nome textual permite escape e falso bloqueio.**  
   O wrapper aceita caminhos arbitrários e concatena o conteúdo real. `--anexo "$A"` apontando para `LEIS.md`, ou uma cópia chamada `/tmp/a.txt`, escapa de reconhecimento literal; procurar nomes indiscriminadamente pode negar um comando que apenas os menciona. Também fica indefinido se `escada` pode levar esses anexos: a negativa especifica somente `meio`, mas a explicação diz “só na 1 e na final” (`bin/codex_pergunta.sh:32`; `bin/codex_pergunta.sh:50`; `bin/codex_pergunta.sh:111`; `contrato.txt:4`).

6. **Itens 1/3 — autorização antes da fila não garante limite na execução.**  
   Se seis chamadas passam a 84%, podem continuar depois de a primeira levar o uso a 85%: o wrapper espera gate/slots e chama o CLI sem revalidar orçamento. O timeout cobre somente o `codex exec`; a espera pelo gate e o laço dos slots não têm prazo global. Assim, o desenho pode **liberar execução já proibida** e deixar chamada aguardando indefinidamente (`bin/codex_pergunta.sh:137`; `bin/codex_pergunta.sh:140`; `bin/codex_pergunta.sh:142`; `bin/codex_pergunta.sh:157`).

7. **Itens 1/2 — “primeiro Stop” mede contexto depois do trabalho, não o piso imediatamente após compactar.**  
   A medida atual pega o último evento `assistant` e soma entrada mais caches. Um ciclo pode executar muitos turnos de ferramentas antes do primeiro Stop; seu “piso” fica inflado e pode ultrapassar 200 mil, desligando C1 justamente nesse ciclo. O anexo distingue primeiro turno pós-compact ≤107.621 de valores de primeiro Stop maiores; não são a mesma medida (`bin/orcamento_contexto.py:89`; `bin/orcamento_contexto.py:101`; `bin/orcamento_contexto.py:110`; `contrato.txt:7`).

8. **Item 2 — 200 mil não é a janela declarada para todos os modelos.**  
   O código declara 1 milhão para opus/sonnet/fable e 200 mil para haiku-4-5. Com piso 135.592, a fórmula proposta dispara em 167.796 — aproximadamente 16,8% da janela de 1 milhão. Com piso 865.942, desliga C1, embora o contexto ainda esteja dentro daquela janela. Isso pode ser um teto operacional deliberado, mas não pode ser rotulado como limite da janela do modelo (`bin/orcamento_contexto.py:35`; `bin/orcamento_contexto.py:44`; `contrato.txt:3`; `contrato.txt:7`).

9. **Itens 1/2 — o ciclo atual mistura sessões.**  
   `ultimo_clear.json` é global por raiz. Guarda `session_id`, mas o Stop extrai apenas `transcript_path` e usa o marco sem casar a sessão. Cenário: A compacta; B faz o próximo Stop; um registrador encaixado nesse fluxo pode gravar tokens de B como piso pós-compact de A. Além disso, um clear seguido de compact antes de qualquer Stop sobrescreve o marco anterior: não haverá medida para “cada ciclo” intermediário (`bin/marco_de_clear.py:20`; `bin/marco_de_clear.py:44`; `bin/marco_de_clear.py:61`; `bin/hook_stop_fila1.py:794`; `bin/hook_stop_fila1.py:1018`).

10. **Itens 1/2 — posição do registro e ausência de piso mudam o comportamento.**  
    Se `registrar_piso` entrar apenas dentro do orçamento, não verá necessariamente o primeiro Stop: o checador do RELATO pode bloquear e sair antes. E um `return` antecipado por “sem piso” ou “piso ≥ teto” dentro de `avaliar()` desligaria também os gatilhos de seis horas e três deploys, que hoje são independentes. O desligamento precisa atingir somente C1 (`bin/hook_stop_fila1.py:1029`; `bin/hook_stop_fila1.py:1034`; `bin/hook_stop_fila1.py:1037`; `bin/orcamento_contexto.py:209`; `bin/orcamento_contexto.py:212`; `bin/orcamento_contexto.py:214`).

11. **Itens 1/2/3 — uma nova negativa no Stop pode contornar sua válvula de saída.**  
    Um `block` por B4≥85 ou erro de piso, colocado antes de PAREI/ESPERANDO/TETO e repetido incondicionalmente, pode impedir inclusive a saída para clear e **prender a sessão**. Hoje o orçamento contém exceções e devolve ao caminho comum; o contador só é atualizado depois. Já existe uma exceção à promessa geral de falha aberta: falha persistente do checador do RELATO bloqueia antes do teto (`bin/hook_stop_fila1.py:886`; `bin/hook_stop_fila1.py:1007`; `bin/hook_stop_fila1.py:1029`; `bin/hook_stop_fila1.py:1092`).

    A linha `DISJUNTOR B4-CODEX` também não é uma transição reconhecida pelo Stop atual. Se for colocada acima de um PAREI ou ESPERANDO legítimo, esconde essa declaração, porque o leitor considera somente a primeira linha útil (`bin/hook_stop_fila1.py:400`; `bin/hook_stop_fila1.py:413`; `bin/hook_stop_fila1.py:938`; `contrato.txt:3`).

12. **Itens 1/4 — escritor único não garante piso único nem prova externa.**  
    Dois Stops concorrentes, ou append seguido de falha antes de persistir “ciclo registrado”, podem duplicar o piso; a operação precisa de identidade do ciclo e serialização. O orçamento atual já serializa leitura/decisão/gravação (`bin/hook_stop_fila1.py:733`; `bin/hook_stop_fila1.py:755`).

    Há ainda um encaixe perigoso: `pisos.log` não está na lista de arquivos proibidos como prova de ESPERANDO. Se seu mtime já foi observado e o próprio Stop apenda um piso antes de conferir a espera, `ESPERANDO: agente-x | arquivo=logs/sessao/pisos.log` pode obter **prova produzida pelo próprio hook** e liberar indevidamente (`bin/hook_stop_fila1.py:556`; `bin/hook_stop_fila1.py:605`; `bin/hook_stop_fila1.py:1051`).

13. **Item 4 — duas linhas não são duas chamadas; “ini” também não prova execução do Codex.**  
    O contador atual acerta ao contar somente `ini`; contar `ini` e `fim` no placar novo duplica chamadas concluídas. Porém `ini` é escrito antes de archive e raspagem: falhas rc3/rc4 entram como “chamadas ao Codex” sem alcançar o CLI. O rótulo atual mede tentativas iniciadas pelo wrapper, não exclusivamente execuções do modelo (`bin/codex_limite_estado.py:64`; `bin/codex_limite_estado.py:76`; `bin/codex_pergunta.sh:88`; `bin/codex_pergunta.sh:99`; `bin/codex_pergunta.sh:105`; `bin/codex_pergunta.sh:157`).

14. **Item 4 — a junção existente pode perder ou duplicar atribuição de tokens.**  
    A chave `(obra, hash12, pid)` não inclui identificador único da chamada. Se o PID for reciclado, o último `fim` sobrescreve o anterior; somar tokens por cada `ini` pode atribuir a mesma sessão a duas chamadas. No caminho LIMITE, o wrapper sai antes de extrair `thread_id`, registra `-`, e o mapa exclui essa sessão: eventual consumo anterior ao erro desaparece da atribuição por papel (`bin/codex_limite_estado.py:63`; `bin/codex_limite_estado.py:67`; `bin/codex_limite_estado.py:96`; `bin/codex_pergunta.sh:59`; `bin/codex_pergunta.sh:171`; `bin/codex_pergunta.sh:175`).

    Também há divergência configurável de destino: o wrapper escreve em `CODEX_LOGD`, enquanto o leitor procura `logs/codex/chamadas.log` sob a raiz (`bin/codex_pergunta.sh:51`; `bin/codex_limite_estado.py:22`; `bin/codex_limite_estado.py:52`).

15. **Item 4 — alguns rótulos prometem medidas ainda não demonstradas.**  
    O leitor chama a apresentação de “uso semanal”, mas aceita qualquer `window_minutes`; uma principal de 300 minutos seria apresentada sob esse título (`bin/codex_limite_estado.py:133`; `bin/codex_limite_estado.py:142`). Para “tokens do dia”, usar o total final de uma sessão que atravessa meia-noite exige definir em qual dia contabilizá-lo; presença e semântica de `total_token_usage` nos registros reais fornecidos são **[não verificado]** (`contrato.txt:5`).

    “Não há fonte estruturada” também não está demonstrado: já existe leitura estruturada de `tool_use/input` dos transcripts Claude. O leitor disponível escolhe somente o transcript mais novo e descarta dimensões necessárias ao placar por modelo/tipo; ele não basta para esse placar, mas sua insuficiência não prova inexistência da fonte (`bin/gerar_sessao.py:33`; `bin/gerar_sessao.py:65`; `bin/gerar_sessao.py:71`).

16. **Item 5 — desligamento precisa existir no processo que decide.**  
    `ORCAMENTO_CONTEXTO=0` já é lido pelo hook, antes de sua trava. Colocar a variável somente no ambiente de outro comando não altera automaticamente esse ambiente. A implementação de `DISJUNTOR=0`, sua abrangência e a leitura de `cota.json` são **[não verificado]**; a cota conhecida pelo handoff atual vem do último aval textual `COTA-*` em PROMPTS (`bin/hook_stop_fila1.py:741`; `bin/handoff_sessao.sh:207`; `contrato.txt:6`).

Quanto à autoria e leitura dos arquivos:

| Arquivo | Quem escreve | Quem lê |
|---|---|---|
| `logs/sessao/pisos.log` | O contrato propõe `orcamento_contexto.registrar_piso`, chamado pelo Stop; implementação **[não verificado]** (`contrato.txt:2`). | Avaliador e ESTADO são consumidores previstos; implementação **[não verificado]** (`contrato.txt:3`; `contrato.txt:5`). |
| `logs/codex/chamadas.log` | Wrapper: `ini` e `fim` (`bin/codex_pergunta.sh:59`; `bin/codex_pergunta.sh:88`). | `codex_limite_estado`; ESTADO consome esse leitor, e CODEX-ESTADO usa seu mapa de sessões (`bin/codex_limite_estado.py:55`; `bin/gerar_estado.py:269`; `bin/gerar_codex_estado.py:176`). |
| `~/.codex/sessions/**/*.jsonl` | O wrapper inicia o CLI; o código que efetivamente grava esses arquivos não está demonstrado: **[não verificado]** (`bin/codex_pergunta.sh:157`). | Leitor de limites e gerador de respostas finais (`bin/codex_limite_estado.py:101`; `bin/gerar_codex_estado.py:80`). |
| `cota.json` | **[não verificado]**. | **[não verificado]**; não confundir com a leitura textual de cota do handoff (`bin/handoff_sessao.sh:207`). |
| `logs/sessao/ultimo_clear.json` | `marco_de_clear.py`, por substituição atômica (`bin/marco_de_clear.py:61`). | `orcamento_contexto.ler_marco`, também chamado pelo Stop (`bin/orcamento_contexto.py:145`; `bin/hook_stop_fila1.py:794`). |
| `orcamento_estado.json` e `orcamento.log` | Hook de Stop (`bin/hook_stop_fila1.py:693`; `bin/hook_stop_fila1.py:717`). | `orcamento_contexto.ler`, consumido pelo ESTADO (`bin/orcamento_contexto.py:228`; `bin/orcamento_contexto.py:235`; `bin/gerar_estado.py:274`). |

A instalação real de PreToolUse/statusLine, o conteúdo de `cota.json` e os selos v3 com mutantes permanecem **[não verificado]**. A ausência desses artefatos nesta cópia não prova ausência no host: `logs/` e `.claude/` são ignorados pelo Git (`.gitignore:53`; `.gitignore:66`).

_826 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
