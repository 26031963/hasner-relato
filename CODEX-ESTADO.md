# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 14:10** · publicado em **30/10/2026 14:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 14:10 · sessao efaf950742da · - · - · origem=Ronald (tmux)

Varredura completa do selo executada em memória, sem Django, banco ou escrita. Foram usados o AST real de `varrer` e o valor real de `VIVOS` (`app/chamados/tests/test_o261_veto_na_porta.py:225`; `app/chamados/catalogo/motor.py:24`).

Resultado:

- Escritas: `0038_a3_registrado_backfill.py::desfazer` e `::registrar`.
- Cópias: `reemitir_chamado_do_dia`, `detectar_ausencias.Command._abrir_chamado`, `reconciliar_grade.Command._emitir_furo`.
- Ambas as listas coincidem exatamente com suas allowlists; catraca `5 <= 5`; as duas portas foram encontradas (`app/chamados/tests/test_o261_veto_na_porta.py:40`, `:46`, `:54`, `:244`).

Isso confirma que o selo passa suas comparações atuais, embora não detecte a reabertura automática por `_pai.reabrir()` do HX: o corpo real do resolver e o mutante mínimo retornaram `([], [])`, coerentemente com o descarte do receptor não reconhecido (`app/chamados/tests/test_o261_veto_na_porta.py:207`, `:212`; `app/chamados/services/disputa_emissao.py:1606`).

Não foi executada a suíte Django; comportamento de integração e ocorrências em produção: [nao verificado]. [nao verificado]

## 30/10 14:09 · sessao efaf950742da · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **Reabertura automática foge do veto pelo HX.** `resolver_perguntas_por_chamado_resolvido` chama `_pai.reabrir()` diretamente quando há resposta aguardando validação, sem consultar `_renascer_por_premissa_viva` nem `motivo_do_veto` (`app/chamados/services/disputa_emissao.py:1598`, `:1604`, `:1606`). O caminho é automático: receiver de `post_save` chama o resolver (`app/chamados/signals.py:75`, `:89`, `:92`); o reconciliador também o chama (`app/chamados/reconciliador.py:538`).

   Contraexemplo concreto construído por leitura: pai histórico `entrada_bloqueada_turno_aberto`, `resolvido`, categoria/subcategoria sem classificação de furo, dia conhecido sem vínculo, sem agrupamento/irmão vivo/encerramento admin; disputa aberta com resposta não validada e não materializada creditada ao pai. O juiz das irmãs só exige carimbos/resposta/crédito (`app/chamados/juizes.py:1471`). Esse módulo está no universo do veto (`app/chamados/catalogo/furo_modulos.py:15`), mas fora de `CALADOS_POR_ISENCAO` (`app/chamados/catalogo/modulos.py:235`), permitindo evitar a guarda de premissa (`app/chamados/models.py:984`, `:1078`). `reabrir` grava estado vivo (`app/chamados/models.py:1158`) sem perguntar vínculo/veto. Ocorrência concreta em produção e execução de integração: [nao verificado].

2. **Grade pode desfazer a recusa da própria porta.** O vínculo/template é pré-carregado (`app/ponto/management/commands/reconciliar_grade.py:189`, `:196`), e o veto externo usa esse objeto (`:487`, `:491`). `abrir` chama `_renascer_por_premissa_viva`, mas devolve o existente mesmo quando renascer recusou (`app/chamados/models.py:686`, `:688`). A guarda nova só trata `ch is None`; se veio encerrado, a grade chama `ch.reabrir()` diretamente (`app/ponto/management/commands/reconciliar_grade.py:519`, `:521`, `:522`).

   Contraexemplo: chamado `batida_ausente` resolvido, célula ainda acusa marco faltante; template carregado 08–17 muda para 08–08 antes do pedido à porta. A cópia externa aprova o objeto antigo, a porta consulta vínculo/template atual e veta (`app/chamados/models.py:778`, `:896`), mas o fallback reabre sem veto. A trava de `abrir` é no colaborador (`app/chamados/models.py:669`), não no template. Ocorrência em produção/ensaio concorrente: [nao verificado].

3. **O selo AST não cobre a fuga real de reabertura por instância.** Só reconhece pedido de porta quando o receptor cita o modelo (`app/chamados/tests/test_o261_veto_na_porta.py:207`); descarta receptores como `_pai` (`:212`); as escritas reconhecidas são construtor/save ou operações create/update de gerente (`:209`, `:216`, `:218`). Executei extração das funções AST em memória com `python3 -B`, sem Django/imports locais/DB/escrita: tanto `def automatica(pai): pai.reabrir()` quanto o corpo real de `resolver_perguntas_por_chamado_resolvido` retornaram `([], [])`. Evidência de execução só nesta sessão, sem artefato: [nao verificado]. A limitação é verificável nas linhas acima.

4. **Ato humano tem trilha genérica, mas não mostra/registra o veto dispensado.** Desarquivar chama `reabrir` e grava resgate (`app/chamados/views_arquivar.py:68`, `:75`, `:79`); reabertura administrativa grava motivo/justificativa humana (`app/chamados/services/acoes_disputa.py:266`, `:270`); reabrir competência grava competência/status (`app/chamados/services/competencia_trancada.py:258`, `:261`). Nenhum desses caminhos pergunta o veto. A trilha nova só registra **recusas** (`app/chamados/models.py:915`, `:924`); o próprio contador admite que ato humano ainda não deixa trilha própria (`app/ponto/services/furos_vetados.py:89`). Contraexemplo: desarquivar chamado de competência trancada com dia sem vínculo; registra resgate, sem `VETO_DEGENERADA` ou dispensa humana desse veto.

Hipótese descartada:

- **Abertura manual via `abrir_chamado_para_colaborador` produzir `None.pk` para módulo do universo: sem achado no catálogo atual.** O serviço rejeita módulo fora de `tipos_para_colaborador` (`app/chamados/services/abertura.py:149`), que só inclui `abrivel_por_humano` (`app/chamados/catalogo/modulos.py:281`, `:306`). Os módulos do universo não declaram essa abertura (`:105`, `:107`, `:109`, `:111`, `:182`); não alcançarão `criar` por essa interface.

## 30/10 14:07 · sessao ab87115a333a · - · - · origem=Ronald (tmux)

Achado confirmado: o efeito “ativo sem vínculo segue abrindo `regua_degenerada`” não tem caminho alcançável para o caso sem vínculo em todo o período.

- O escritor existe: `vetar_cobranca_se_degenerada` transforma `ec=None` em template `None`, considera degenerada e chama `ChamadoColaborador.abrir(modulo_origem='regua_degenerada')`, com `tipo_escala_id=None` e código `SEM_ESCALA` (`app/escala/regua_defesa.py:23`, `:45`, `:51`, `:73`).
- Seus dois chamadores são `_abrir_chamado` do detector (`app/ponto/management/commands/detectar_ausencias.py:599`) e `_emitir_furo` da grade (`app/ponto/management/commands/reconciliar_grade.py:486`).
- O detector exclui `ec=None` antes de chegar ali (`app/ponto/management/commands/detectar_ausencias.py:139`).
- O cartório integral exige vínculo não nulo (`app/ponto/services/cartorio.py:1347`); seu ramo parcial retorna sem vínculo (`app/ponto/services/cartorio.py:1446`).
- O emissor retroativo de furo pula vínculo ausente (`app/ponto/management/commands/emitir_furo_retroativo.py:118`).
- A grade não coloca o dia na fila de emissão quando o template é ausente: `escala_ok(None)==False` (`app/ponto/management/commands/reconciliar_grade.py:157`, `app/ponto/marcos.py:107`).
- Os emissores de turno tampouco abrem cadastro: saem sem vínculo (`app/ponto/management/commands/processar_alertas_avancados.py:37`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`).
- O novo juiz é somente leitura e só retorna `VETO_DEGENERADA`; a porta também não chama o escritor de cadastro (`app/ponto/services/furos_vetados.py:40`, `app/chamados/models.py:896`).

Contraexemplo concreto por fluxo: colaborador ativo, em operação, sem qualquer `EscalaColaborador` nem geradora de célula, sem chamado prévio de cadastro; detector e retroativos saltam o dia, cartório não chama emissor, grade não enfileira emissão, porta veta o chamado de turno sem abrir `regua_degenerada`. Chamado de cadastro não nasce pelos caminhos acima; ocorrência real em produção [nao verificado]. A lacuna já pode anteceder o diff; o que fica reprovado é a garantia do aval nesta cópia.

Achado adicional de impressão: existe um ramo do cartório que não incorpora o novo veto.

- `adesao is None` entra no ramo “nunca bateu” (`app/ponto/services/cartorio.py:907`).
- Esse ramo chama `impressao_insumos` sem argumento `veto` (`app/ponto/services/cartorio.py:941`), compara o hash e pula a célula se igual (`:943`), encerrando antes do leitor de `motivo_do_veto` (`:1003`, `:1007`).
- O ramo normal consulta o juiz em `:1013`, passa seu resultado em `:1083`; `impressao_insumos` acrescenta `veto:VETO_DEGENERADA` ao hash em `:329`.

Contraexemplo: ativo sem adesão, célula já carimbada, `ec=None`, demais insumos iguais. O novo resultado do juiz não muda `imp0`; `:943` continua pulando. Se “ec=None também para impressão” abrange toda célula, o censo deixou esse leitor indireto de fora. Não há cobrança de furo nesse ramo por desenho (`app/ponto/services/cartorio.py:903`); portanto não alegar cobrança indevida como efeito.

Relavratura/dinheiro/exportada: há caminho de rejulgamento automático que percorre todas as células passadas sem recorte de competência (`app/ponto/management/commands/processar_cartorio.py:49`, `:61`, `:65`), e o hash novo do ramo normal pode provocar lavra (`app/ponto/services/cartorio.py:1083`, `:1092`, `:1409`, `:1424`). Escritores efetivos são `lavrar_veredito` (`app/ponto/portas/celula.py:689`) e `carimbar` (`:721`). Reversão prévia, aplicação financeira autorizada, hashes preservados de exportação e Pauta DP da mudança específica: [nao verificado]. Não confundir rejulgamento de célula com alteração comprovada do TXT exportado.

## 30/10 14:07 · sessao 95faa6137e78 · - · - · origem=Ronald (tmux)

**Migração “sem vínculo/sem escala”: sem achado.** O caso explicitamente “sem escala” preserva a condição: `_colab(..., com_escala=False)` só cria vínculo quando solicitado; `test_MORDE_procedencia_ignora_batida_retratada` não chama o helper `_chamado_agrupado` que ganhou `dar_vinculo` (`app/chamados/tests/test_selo_f4_fatia4.py:45`, `:50`, `:69`, `:131`, `:136`, `:139`). `TrancaSemCadastro` testa a exclusão de **avisos de cadastro** da tranca, e não ausência de vínculo (`app/chamados/tests/test_tranca_sem_cadastro.py:9`, `:39`, `:50`).

**Vacuidades encontradas em testes tocados, mas não demonstradas como introduzidas pela migração:** [nao verificado]

1. `test_MORDE_desligado_segue_o_gate_do_c2` ganha vínculo, mas continua sem célula para o desligado; a célula da fixture pertence ao outro colaborador (`app/chamados/tests/test_emissao_reata_orfa.py:30`, `:31`, `:67`, `:71`). As asserções exigem apenas destino ausente/zero chamados (`:73`, `:74`). **Contraexemplo:** remover o gate `situacao != 'ativo'` ainda deixa o caso barrado pela ausência de protesto da célula (`app/chamados/reconciliador.py:789`, `:834`, `:836`). O teste não prova o gate que nomeia.

2. `test_sem_celula_o_chamado_ainda_nasce` somente chama `_ctx_c2` e inspeciona o dicionário; não tenta abrir chamado (`app/chamados/tests/test_emissor_nomeia_o_marco.py:88`, `:92`, `:93`). **Contraexemplo:** impedir todo nascimento na porta não é observado por essas asserções; a porta é chamada separadamente pelo c2 (`app/chamados/reconciliador.py:862`).

3. A mesma limitação ocorre em `test_sem_ata_possivel_o_chamado_nasce_sem_marco_e_nao_deixa_de_nascer`: somente `_ctx_c2` e três campos do resultado são exercitados (`app/chamados/tests/test_bug125_cobranca_cega.py:176`, `:182`, `:183`). **Contraexemplo:** recusar o nascimento depois de construir contexto não altera o resultado que esse teste verifica (`app/chamados/reconciliador.py:862`, `:872`).

**Controles úteis preservados:** os testes de deduplicação exigem um chamado efetivo, evitando verde por veto total (`app/chamados/tests/test_3c_abrir_ausencia.py:52`, `app/chamados/tests/test_f3_dedup_atraso.py:57`, `:64`); o teste novo sem vínculo espera `(None, False)` e compara com nascimento com vínculo (`app/chamados/tests/test_o261_veto_na_porta.py:395`, `:404`, `:406`).

Execução da suíte e estado verde efetivo: **[nao verificado]**. [nao verificado]

## 30/10 14:04 · sessao 4573a6843b71 · CALENDARIO-HOJE · verificador · origem=Code

**Parecer: reprova pelo teto civil que dispensa o juiz para dias anteriores a ontem e pelo custo adicional por card no request.** (`app/colaboradores/services/calendario.py:702-704`; `app/relatorios/furos.py:184-188`). Execução dos testes, medição de queries e ocorrência dos contraexemplos em produção: **[nao verificado]**.

1. **Quem escreve e quem lê**

   `contexto_calendario` fornece `futuro`; `_classificar` o lê nos ramos de trabalho e de escala suspeita. O campo não é exportado: os consumidores recebem `status` e seus derivados. (`app/colaboradores/services/calendario.py:131,147,797,867-901`)

   | Leitor | Efeito da mudança |
   |---|---|
   | Perfil e gaveta | Mudam cor, ícone e apresentação pela mesma grade. (`app/colaboradores/views.py:1946`; `app/colaboradores/services/drawer.py:121`; `app/templates/colaboradores/partials/_calendario_grade.html:102-105`) |
   | Furos HTML/PDF | O calendário muda; seleção, `nfuro`, marcos faltantes e ordenação continuam na cadeia independente `grade_da_celula → estado_do_dia`. O PDF recebe o novo status para pintar. (`app/relatorios/furos.py:141,160-190`; `app/relatorios/views.py:1550-1551`; `app/relatorios/furos_pdf.py:56,66`) |
   | Resolver dia / DP | HOJE já era excluído. ONTEM em curso pode perder a ação, pois `previsto` não pede decisão, caso também satisfaça “último recurso”. (`app/colaboradores/services/calendario.py:188,196-197,886-887`) |
   | Vigia | Nesse cenário condicionado de ONTEM, deixa de contar `decisao_do_dia_pendente` e pode alcançar `fio_morto`, dependendo dos demais filtros. Ocorrência real **[nao verificado]**. (`app/chamados/services/vigia_fio.py:289-324`) |
   | Resumo/rodapé | Reduz `faltas` ou `ok` e aumenta `previstos`; previstos são contados, mas não exibidos no rodapé. (`app/colaboradores/services/calendario.py:921-937`; `app/templates/colaboradores/partials/_calendario_grade.html:141-173`) |
   | `status_do_dia` / `contar_por_status` | Herdam a mudança integralmente por montarem o calendário. Chamadas dinâmicas externas: **[nao verificado]**. (`app/colaboradores/services/calendario.py:217-231`) |

   O alcance excede `falta → previsto`: inclui `ok` do isento, `escala_suspeita`, possível `escala_suspeita → sem_dados` no limbo e bloqueio da sobreposição de veredito. O rótulo de escala suspeita também desaparece quando o status muda. (`app/colaboradores/services/calendario.py:131-150,175,885`)

   **Achado de apresentação:** HOJE/ONTEM em curso passam a receber o tooltip **“Dia futuro, ainda sem batida”**. (`app/colaboradores/services/calendario.py:55,873`; `app/templates/colaboradores/partials/_calendario_grade.html:105`)

2. **HOJE/ONTEM é teto por data: sim**

   Para datas anteriores a ontem, o calendário não pergunta ao juiz e fornece `futuro=False`. A justificativa de que todo turno anterior já encerrou não é garantida pelo juiz. (`app/colaboradores/services/calendario.py:698-704,797`; `app/ponto/turnos.py:2102-2105`)

   Há três contraexemplos derivados do código:

   - **24 horas com aberta:** início/fim às 23h representa jornada de 24h; sua aberta pode permanecer viva até D+2 às 03h, pelo fim+4h. Em D+2 às 02h, D é anterior a ontem e ainda não encerrou. Isso refuta a justificativa, embora, com batida, o status já siga `aberto` independentemente de `futuro`. (`app/escala/servico_jornada.py:95-102`; `app/ponto/turnos.py:1992,2007-2011`; `app/colaboradores/services/calendario.py:129,151`)
   - **Turno longo sem batida:** o encadeamento `E18 → S06 → E18 → S06` alcança D+2 às 06h. Consultado às 02h de D+2, o juiz ainda responde não encerrado; o corte exclui D e permite `falta`. É contraexemplo do algoritmo; presença desse DNA/template em produção **[nao verificado]**. (`app/escala/servico_jornada.py:74-85,89-92`; `app/ponto/turnos.py:2105`; `app/colaboradores/services/calendario.py:702,797,140`)
   - **Trabalho sem fim previsto:** o juiz devolve `sem_previsto`, sem prazo civil para virar encerrado. O calendário pode dar `previsto` hoje/ontem e `falta` antes disso apenas pela idade da data. Existência real **[nao verificado]**. (`app/ponto/turnos.py:2102-2105`; `app/escala/models.py:1099-1108`; `app/colaboradores/services/calendario.py:131-140,702,797`)

   Sem célula, o juiz pode recorrer ao template; ausência de célula não garante encerramento. Folga, feriado e ausência têm precedências próprias no classificador, portanto não viram necessariamente `previsto` por estarem no conjunto `_em_curso`. (`app/escala/models.py:1097-1108`; `app/colaboradores/services/calendario.py:120-128,141-146`)

3. **Relógios**

   **Sem achado na cadeia adicionada:** `_agora` produz `hoje` e é passado a `dias_encerrados`; este só lê outro relógio se receber `None`, e o julgamento usa o argumento recebido. Também não nasce outro juiz: o lote chama o predicado puro existente. (`app/colaboradores/services/calendario.py:250-251,704`; `app/ponto/turnos.py:2083-2105,2118-2132`)

   **Há relógios independentes no restante da montagem:** `folha_manda` consulta `turno_aberto_de` sem `agora` e `ate_ontem` sem `hoje`. Eles podem decidir a palavra/lista “Em aberto” com instante diferente daquele usado pelo status. Divergência efetivamente observada **[nao verificado]**. (`app/colaboradores/services/calendario.py:681`; `app/relatorios/cartao_pela_celula.py:398,412,40`; `app/ponto/turnos.py:2254-2255`)

   Há ainda relógio próprio de `pergunta_viva` para janela de competência e outro para `pode_desfazer`; são finalidades distintas do encerramento novo. (`app/colaboradores/services/calendario.py:335-339,580`; `app/chamados/juizes.py:795,1066,518`)

4. **Custo adicional e request**

   Sem interseção com hoje/ontem: **zero consultas adicionais**. Com interseção, o calendário chama o lote sem alimentar células ou turnos já carregados. (`app/colaboradores/services/calendario.py:308,402,702-704`)

   A conta estática é **`6 + E + M + U + K`**: seis consultas base; `E` para geradoras extras, `M` para mapa DNA, `U` para última batida e `K` para os dias que consultam DNA por `fim_turno`, até dois. Medição efetiva **[nao verificado]**. (`app/ponto/turnos.py:1858,1880-1886,1903-1911,1556,1949,2102,2123-2126`; `app/escala/alimentacao.py:123-127`; `app/escala/models.py:1097`)

   No cenário `E=U=0`, `M=1`, com todos os dias alcançando o fim previsto: **+8 queries para um dia; +9 para dois; +9N para N cards nessa condição**. É derivação estática, não medição. (`app/ponto/turnos.py:2123-2132`; `app/escala/utils.py:100-102`; `app/escala/models.py:1097`)

   A duplicação inclui duas cargas de células: o lote cria `cels`, mas passa o argumento original `celulas=None` a `turnos_do_colab`. HTML e PDF pagam o acréscimo durante o request, antes da bifurcação de renderização; o PDF reutiliza os cards. (`app/ponto/turnos.py:2123-2126`; `app/relatorios/furos.py:184-188`; `app/relatorios/views.py:1541,1560-1566`)

5. **Premissa do isento: sem achado funcional**

   Para HOJE às 10h, turno 08–17 sem batida, `ok → previsto` acompanha o aval: o turno não encerrou e `futuro` precede a isenção. O teste discriminante do Art. 62 continua cobrando dias encerrados separadamente. (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:94,298-302,272-286`; `app/colaboradores/services/calendario.py:131-134`; `app/ponto/turnos.py:2102-2105`)

   Ficaram obsoletos o nome, a descrição e a mensagem que ainda prometem verificar o caso literal `status=ok`. (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:288-304`)

6. **Mordida dos testes e adequação de `previsto`** [nao verificado]

   Os dois RED distinguem a expressão antiga; o MORDE impede manter ONTEM previsto depois do fim no cenário 18–06. **Execução Django: [nao verificado]**. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:23-47`)

   **A cobertura é insuficiente:** o mutante `data >= hoje or data in _em_curso` satisfaz as três expectativas e continua errado para HOJE após o fim. Faltam também turno longo, limite exato, aberta viva e fatos de ausência/cobertura/veredito. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:37-47`; `app/colaboradores/services/calendario.py:797`)

   `previsto` é adequado para trabalho sem batida e sem fato prioritário, mas não é resposta universal:

   | Fato presente | Resultado atual |
   |---|---|
   | Ausência na faixa humana | `falta` ou `ausencia`, antes de `futuro`. (`app/colaboradores/services/calendario.py:120-121`) |
   | Cobertura abonadora | Substitui `previsto` por `ausencia`. (`app/colaboradores/services/calendario.py:179-180`) |
   | Veredito isolado | `previsto` bloqueia sua sobreposição, inclusive `faltou`, `nunca_bateu` e `justificado`. (`app/colaboradores/services/calendario.py:65-69,175`) |

   Esse último é um **risco não testado**, sem regressão de produção demonstrada: a porta normal de `faltou/justificado` cria ausência aprovada antes de lavrar o veredito, permitindo a precedência da ausência. Existência de ata isolada no dado fornecido **[nao verificado]**. (`app/ponto/services/veredito_celula.py:310-330`; `app/ponto/precedencia.py:211-217,318`; `app/colaboradores/services/calendario.py:120-121`)

## 30/10 14:02 · sessao c6cb00e95f9a · - · - · origem=Ronald (tmux)

**Pergunta 2 — há teto por data e a justificativa não é válida em geral.**

- O calendário pergunta somente sobre `[max(início, hoje−1), min(fim, hoje)]`; dias anteriores não entram em `_em_curso`, e seu `futuro` será falso. Isso decide implicitamente encerramento por idade civil nesses dias. (`app/colaboradores/services/calendario.py:702`, `app/colaboradores/services/calendario.py:704`, `app/colaboradores/services/calendario.py:797`)
- Contraexemplo do juiz: turno de 24 horas, `hi=hf=23:00`, iniciado em D, termina em D+1 às 23:00 e aberta expira em D+2 às 03:00; consultado em D+2 às 02:00, D é anterior a ontem e permanece não encerrado. Igualdade das bordas representa 24 horas; expiração é fim+4h, com comparação estrita `agora > expiração`. (`app/escala/servico_jornada.py:95`, `app/escala/servico_jornada.py:102`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:2007`, `app/ponto/turnos.py:2011`, `app/ponto/turnos.py:2097`)
- Esse contraexemplo com aberta refuta o comentário do corte, mas sozinho não prova status incorreto: com batida, o classificador segue `aberto`, independentemente de `futuro`. (`app/colaboradores/services/calendario.py:129`, `app/colaboradores/services/calendario.py:151`)
- Há contraexemplo de status derivável: dia de trabalho sem batida e sem marcos/fim previsto pode continuar não encerrado indefinidamente; o juiz devolve `sem_previsto`, porém o calendário passa `futuro=False` quando anterior a ontem e pode produzir `falta`. Célula sem DNA de marcos recorre ao tipo, que pode fornecer horários ausentes. Existência desse caso nos dados de produção **[nao verificado]**. (`app/ponto/turnos.py:2102`, `app/ponto/turnos.py:2104`, `app/escala/models.py:1099`, `app/escala/models.py:1108`, `app/escala/servico_jornada.py:72`, `app/colaboradores/services/calendario.py:130`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:797`)
- O encadeamento dos quatro marcos admite avançar mais de uma virada; portanto o corpo de `fim_turno` não impõe duração máxima de um dia. A presença de DNA/template assim em produção **[nao verificado]**. (`app/escala/servico_jornada.py:74`, `app/escala/servico_jornada.py:82`, `app/escala/servico_jornada.py:89`)

**Pergunta 3 — juiz novo recebe o mesmo relógio; calendário inteiro ainda tem outros relógios.**

- **Sem achado** na cadeia adicionada: calendário captura `_agora` e deriva `hoje` dele; repassa `_agora`; `dias_encerrados` só captura `timezone.now()` quando argumento ausente; julgamento usa o argumento recebido. (`app/colaboradores/services/calendario.py:250`, `app/colaboradores/services/calendario.py:251`, `app/colaboradores/services/calendario.py:704`, `app/ponto/turnos.py:2118`, `app/ponto/turnos.py:2097`, `app/ponto/turnos.py:2105`)
- O calendário chama `folha_manda`, que consulta `turno_aberto_de` sem `agora` e `ate_ontem` sem `hoje`; ambos capturam relógio novamente. Isso afeta a palavra/lista “Em aberto”, enquanto `_em_curso` e status usam a borda anterior. (`app/colaboradores/services/calendario.py:681`, `app/relatorios/cartao_pela_celula.py:398`, `app/ponto/turnos.py:2254`, `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:40`)
- Existe outro `timezone.now()` para `pode_desfazer`, de finalidade distinta. (`app/colaboradores/services/calendario.py:335`, `app/colaboradores/services/calendario.py:339`)
- `_pv` também pode chegar a `date.today()`: calendário consulta pergunta viva; seu cálculo consulta janela viva, que usa data do sistema. Isso influencia `_perg_viva`, não o encerramento consultado no diff. (`app/colaboradores/services/calendario.py:580`, `app/chamados/juizes.py:795`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:518`)
- `grade_da_celula` não captura relógio no seu corpo principal. (`app/escala/services/leitor_celula.py:321`, `app/escala/services/leitor_celula.py:371`)

**Pergunta 4 — custo adicional em request, por card.**

- Sem interseção com hoje/ontem: **zero** consultas adicionadas. Havendo interseção: uma chamada de lote por montagem, sem alimentação das células/turnos já carregados. (`app/colaboradores/services/calendario.py:308`, `app/colaboradores/services/calendario.py:402`, `app/colaboradores/services/calendario.py:703`, `app/colaboradores/services/calendario.py:704`)
- Contagem estática adicional: **`6 + E + M + U + K`**. Seis fixas: duas cargas de células, dois carregamentos de vínculos, batidas e vínculo da ata; `E`=0/1 geradoras extras, `M`=0/1 mapa DNA, `U`=0/1 última batida, `K`=0..2 consultas unitárias de DNA quando os dias alcançam `fim_turno`. Medição efetiva **[nao verificado]**. (`app/ponto/turnos.py:2123`, `app/ponto/turnos.py:2124`, `app/ponto/turnos.py:2126`, `app/ponto/turnos.py:1880`, `app/ponto/turnos.py:1883`, `app/ponto/turnos.py:1858`, `app/ponto/turnos.py:1591`, `app/ponto/turnos.py:1556`, `app/escala/alimentacao.py:123`, `app/ponto/turnos.py:1949`, `app/ponto/turnos.py:1906`, `app/ponto/turnos.py:2102`, `app/escala/models.py:1097`)
- No caso comum `E=U=0`, `M=1`, todos os dias alcançando fim previsto: **+8 queries para um dia; +9 para dois dias**; para N cards com os dois dias, **+9N**. Derivação estática; execução **[nao verificado]**. (Mesmas consultas acima.)
- A duplicação de células existe porque o lote cria `cels`, mas passa o argumento original `celulas=None` a `turnos_do_colab`; além disso, não passa células ao `fim_turno → EC.marcos_do_dia`. (`app/ponto/turnos.py:2123`, `app/ponto/turnos.py:2126`, `app/ponto/turnos.py:2102`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1097`)
- HTML e PDF pagam isso no request: calendário é construído no laço de cards, dataset é chamado pela view antes da bifurcação HTML/PDF; o PDF reutiliza os cards, sem segunda montagem nessa bifurcação. (`app/relatorios/furos.py:139`, `app/relatorios/furos.py:184`, `app/relatorios/views.py:1541`, `app/relatorios/views.py:1560`, `app/relatorios/views.py:1566`)

## 30/10 14:02 · sessao d1a89dce4e93 · - · - · origem=Ronald (tmux)

5. **Sem achado na troca `ok → previsto` para essa premissa.** O fixture fixa HOJE às 10h, turno 08–17, isento sem batida; no classificador, `futuro` precede isenção e o juiz só encerra após o fim previsto. A mudança acompanha a lei do aval; a proteção específica do Art.62 continua discriminada pelos dias encerrados em `test_05`. (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:94`, `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:116`, `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:127`, `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:272`, `app/colaboradores/services/calendario.py:131`, `app/ponto/turnos.py:2102`)

O nome/docstring/mensagem do `test_05b` ficaram obsoletos: ainda descrevem status `ok` e o “CASO 5 literal”, mas a asserção agora verifica `previsto`. Isso não demonstra regressão funcional; enfraquece a descrição do que o teste garante. (`app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:288`, `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:301`, `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:304`)

6. **Os três casos especificados têm mordida por inspeção, mas execução Django [nao verificado].** O fixture define jornada diária 18–06; os dois RED verificam D às 13h e D+1 às 02h como `previsto`; o MORDE exige `falta` em D+1 às 11h. Com a expressão antiga `data > hoje`, D não seria futuro em nenhum dos três momentos: os RED distinguem a correção e o MORDE impede mantê-lo previsto indefinidamente. Os testes não cobrem instante exato do fim, pós-fim no próprio dia, jornadas longas, aberta viva, ausência/cobertura/veredito nem unicidade de relógio. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:23`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:32`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:37`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:41`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:45`; expressão antiga no diff fornecido, `calendario.py:785`)

**`previsto` não é resposta universal quando há outros fatos.** A precedência atual permanece:

| Fatos além de trabalho/sem batida/não encerrado | Resultado |
|---|---|
| Nenhum; ou isento | `previsto`. (`app/colaboradores/services/calendario.py:131`) |
| Faixa humana `ferias/afastamento/atestado` | `falta` se desconta; senão `ausencia`, antes de consultar `futuro`. (`app/colaboradores/services/calendario.py:120`) |
| Feriado; ou folga humana | `folga`. (`app/colaboradores/services/calendario.py:123`, `app/colaboradores/services/calendario.py:127`) |
| Cobertura que abona, sem faixa humana | `ausencia`: o ajuste final sobrescreve `previsto`. (`app/colaboradores/services/calendario.py:179`) |
| Cobertura que desconta, sem faixa humana | `previsto`, pois ela só determina `falta` no ramo passado. (`app/colaboradores/services/calendario.py:131`, `app/colaboradores/services/calendario.py:140`) |
| Veredito isolado `faltou`, `nunca_bateu`, `justificado` ou `concorde` | `previsto`: esse status bloqueia a sobreposição de qualquer veredito. (`app/colaboradores/services/calendario.py:65`, `app/colaboradores/services/calendario.py:175`) |

Exercitei essa tabela extraindo apenas `_classificar` e suas constantes por AST em memória, com `python3 -B`, sem importar Django nem acessar banco; os resultados correspondem aos ramos acima. Evidência persistida dessa execução [nao verificado].

**Ressalva para não produzir falso achado:** a porta normal de veredito humano cria ausência aprovada antes de lavrar `faltou/justificado`; essa ausência entra na faixa humana e retorna antes de `previsto`. Portanto, “o diff apaga toda falta declarada pelo admin” seria uma conclusão incorreta. A regressão é demonstrável para o classificador com **ata isolada**, mas existência desse estado real no dado fornecido [nao verificado]. (`app/ponto/services/veredito_celula.py:310`, `app/ponto/services/veredito_celula.py:329`, `app/ponto/precedencia.py:211`, `app/ponto/precedencia.py:318`, `app/colaboradores/services/calendario.py:120`)

Achado adicional para item 1: HOJE/ONTEM em curso passam a carregar `status_rotulo='Dia futuro, ainda sem batida'`, e o template imprime essa frase no tooltip do ícone `previsto`. O rótulo não acompanha a ampliação semântica do status. (`app/colaboradores/services/calendario.py:55`, `app/colaboradores/services/calendario.py:797`, `app/colaboradores/services/calendario.py:873`, `app/templates/colaboradores/partials/_calendario_grade.html:105`)

## 30/10 14:01 · sessao 24d67b3f80aa · - · - · origem=Ronald (tmux)

Censo de leitores confirmado; não encontrei outro chamador direto de produção além das cadeias abaixo. Chamadas dinâmicas ou externas permanecem [nao verificado].

| Escritor/leitor | Efeito do diff |
|---|---|
| Escritor de `futuro` | `contexto_calendario` escreve o fato em `calendario.py:797`; somente `_classificar` lê esse fato (`app/colaboradores/services/calendario.py:131`, `app/colaboradores/services/calendario.py:147`). O dicionário exportado contém `status` e derivados, sem `futuro` (`app/colaboradores/services/calendario.py:867`). |
| Escritor de `status` | `_classificar` decide e `contexto_calendario` publica `status`, `status_rotulo`, `regua_rotulo`, `resolver_dia_oferecido` e contagens (`app/colaboradores/services/calendario.py:104`, `app/colaboradores/services/calendario.py:872`, `app/colaboradores/services/calendario.py:885`, `app/colaboradores/services/calendario.py:917`, `app/colaboradores/services/calendario.py:920`). |
| Perfil | A view monta o calendário; o partial inclui a grade; falta muda de fundo/ícone para previsto (`app/colaboradores/views.py:1946`, `app/templates/colaboradores/calendario_partial.html:62`, `app/templates/colaboradores/partials/_calendario_grade.html:72`, `app/templates/colaboradores/partials/_calendario_grade.html:75`, `app/templates/colaboradores/partials/_calendario_grade.html:102`, `app/templates/colaboradores/partials/_calendario_grade.html:105`). |
| Gaveta | Monta o mesmo calendário e inclui a mesma grade e resumo (`app/colaboradores/services/drawer.py:121`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`). |
| Furos HTML | Monta o calendário por card e inclui a grade/resumo. Contagem, seleção, lista de marcos e ordenação continuam vindo da outra cadeia `grade_da_celula → estado_do_dia` (`app/relatorios/furos.py:141`, `app/relatorios/furos.py:160`, `app/relatorios/furos.py:164`, `app/relatorios/furos.py:184`, `app/relatorios/furos.py:190`, `app/templates/relatorios/furos.html:42`). |
| Furos PDF | A view extrai `data → status`; o PDF pinta `falta` e `previsto` com cores distintas, mas imprime `nfuro` e a lista de furos independentes (`app/relatorios/views.py:1550`, `app/relatorios/furos_pdf.py:12`, `app/relatorios/furos_pdf.py:13`, `app/relatorios/furos_pdf.py:56`, `app/relatorios/furos_pdf.py:66`). |
| Resolver dia / DP | `previsto` não pertence aos status que oferecem decisão. HOJE já era excluído por passado estrito; ONTEM em curso pode perder o botão se também satisfizer último recurso (`app/colaboradores/services/calendario.py:188`, `app/colaboradores/services/calendario.py:196`, `app/colaboradores/services/calendario.py:886`, `app/templates/colaboradores/partials/_calendario_grade.html:125`). |
| Vigia | Pula hoje/futuro e consulta a porta curta do calendário para passado. Se ONTEM perder Resolver dia, deixa de entrar em `decisao_do_dia_pendente` e segue pelos outros filtros; pode alcançar `fio_morto` (`app/chamados/services/vigia_fio.py:289`, `app/chamados/services/vigia_fio.py:294`, `app/chamados/services/vigia_fio.py:298`, `app/chamados/services/vigia_fio.py:301`, `app/chamados/services/vigia_fio.py:324`). Ocorrência real [nao verificado]. |
| Rodapé/resumo | Conta literalmente os status; diminui `faltas` ou `ok`, aumenta `previstos`. O rodapé mostra “Sem registro” e “Ok”, mas não exibe previstos (`app/colaboradores/services/calendario.py:921`, `app/colaboradores/services/calendario.py:922`, `app/colaboradores/services/calendario.py:937`, `app/templates/colaboradores/partials/_calendario_grade.html:142`, `app/templates/colaboradores/partials/_calendario_grade.html:146`, `app/templates/colaboradores/partials/_calendario_grade.html:173`). |
| Portas curtas | `status_do_dia` e `contar_por_status` montam calendário e devolvem o status/contagem; herdam integralmente a mudança (`app/colaboradores/services/calendario.py:217`, `app/colaboradores/services/calendario.py:220`, `app/colaboradores/services/calendario.py:228`, `app/colaboradores/services/calendario.py:231`). Chamadores diretos encontrados são testes; inexistência de chamadas dinâmicas [nao verificado]. |

**Alcance maior que “falta → previsto”.** O novo `futuro=True` também desloca isento de `ok` para `previsto`, escala suspeita de `escala_suspeita` para `previsto`, e pode deslocar limbo suspeito para `sem_dados`; remove o `regua_rotulo` quando deixa de ser escala suspeita (`app/colaboradores/services/calendario.py:131`, `app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:135`, `app/colaboradores/services/calendario.py:147`, `app/colaboradores/services/calendario.py:150`, `app/colaboradores/services/calendario.py:885`).

**Ata passa a ser ignorada nesse ramo.** Antes, uma falta/ok/escala suspeita podia receber o status do veredito; após virar `previsto`, a guarda impede a substituição. Cobertura abonadora ainda substitui `previsto` por `ausencia`, e ausência/feriado continuam precedendo a pergunta de futuro (`app/colaboradores/services/calendario.py:120`, `app/colaboradores/services/calendario.py:123`, `app/colaboradores/services/calendario.py:175`, `app/colaboradores/services/calendario.py:179`).

**Achado condicionado para o vigia:** na virada da competência, ONTEM em curso pode satisfazer último recurso e antes oferecer Resolver dia; com `previsto`, perde a ação e pode aparecer como fio morto, embora o turno ainda esteja vivo. A cadeia permite isso; caso real no dado fornecido [nao verificado] (`app/colaboradores/services/calendario.py:571`, `app/colaboradores/services/calendario.py:886`, `app/colaboradores/services/calendario.py:196`, `app/chamados/services/vigia_fio.py:294`, `app/chamados/services/vigia_fio.py:324`).

## 30/10 13:55 · sessao 3ded292a25d3 · O261-VETO-A2 · verificador · origem=Code

**O delta não comprova a cura dos achados 1 e 2; cura os alvos de 3, 4 e 5 no código, com lacunas de teste descritas abaixo.** Execução dos testes, RED contra a base e resultados em produção: **[nao verificado]**. [nao verificado]

**1. Achados 1..5 do opus**

| Achado | Veredito e mudança | Mutante/lacuna que permanece |
|---|---|---|
| **1 — RED do nascimento contra a base** | **Cura não demonstrada.** O diff fornecido não acrescenta essa prova **[nao verificado]**. Há testes de nascimento, `ec=None` e reabertura na cópia; existência de teste não comprova RED contra a base (`app/chamados/tests/test_o261_veto_na_porta.py:418`, `:476`, `:500`, `:522`). | Não há teste novo desse item no delta. Quais mutantes a execução contra a base eliminaria: **[nao verificado]**. |
| **2 — linha LEI-AKITA desatualizada** | **Cura não demonstrada.** A linha corrigida de commit não foi fornecida **[nao verificado]**. As origens que precisam constar existem: `recusa_da_reabertura` consulta o veto e `reabrir` consulta essa guarda (`app/chamados/models.py:1090`, `:1143`). O leitor omitido importa e consome `furos_vetados_por_regua` (`app/escala/management/commands/desfazer_corroboracao_regua.py:25`, `:38`). | Nenhum teste novo verifica esse metadado **[nao verificado]**. No consumidor citado, `vinculo=None` é excluído pelo filtro `== ec.pk`: **sem achado funcional nesse caminho** (`app/ponto/services/furos_vetados.py:69`; `app/escala/management/commands/desfazer_corroboracao_regua.py:39`). |
| **3 — impressão com `ec=None` sem teste próprio** | **Curado no alvo: sem achado.** O teste novo observa a impressão real, controla `{None}`, encerra a vigência e exige `{'VETO_DEGENERADA'}` (`app/ponto/tests/test_veto_que_cai_rejulga.py:124`, `:129`, `:130`, `:131`). O cartório consulta o juiz mesmo sem vínculo (`app/ponto/services/cartorio.py:1013`). | Mutante plausível: `impressao_insumos` receber `VETO_DEGENERADA`, mas ignorar especificamente esse valor no hash. O spy continuaria vendo o argumento; o teste direto do hash só compara `VETO_REGUA` (`app/ponto/services/cartorio.py:329`; `app/ponto/tests/test_veto_que_cai_rejulga.py:126`, `:138`). Sobrevivência executada: **[nao verificado]**. |
| **4 — helper 5x2 dependente do fim de semana** | **Curado no calendário declarado:** agora é semanal com os sete dias (`app/chamados/tests/_vinculo_do_dia.py:18`). Semanal usa a lista; 5x2 sem configuração folga sábado/domingo (`app/escala/models.py:830`, `:852`, `:861`). Efeitos nas fixtures estão discriminados abaixo. | Não foi acrescentado teste direto dos sete weekdays. Um mutante omitindo domingo não seria detectado pelos novos testes de cartório/comando, que criam seus próprios tipos 5x2 e 6x1 (`app/ponto/tests/test_veto_que_cai_rejulga.py:37`; `app/ponto/tests/test_passe_turno_aberto_retroativo.py:41`). Resultado na suíte inteira: **[nao verificado]**. |
| **5 — veto contado por eliminação** | **Curado no código: sem achado no alvo.** Agora pergunta célula, pergunta veto e, sem resposta dos dois, incrementa `recusado_sem_juiz` (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174`, `:179`, `:184`, `:193`). | Mutante plausível: imprimir `[RECUSADO veto]` no fallback, preservando o contador correto. O MORDE novo só exige os contadores, sem exigir `[RECUSADO sem juiz]` (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:188`; `app/ponto/tests/test_passe_turno_aberto_retroativo.py:142`). Sobrevivência executada: **[nao verificado]**. |

**2. Helper semanal: mudança de comportamento e módulos dependentes**

A mudança **pode alterar a previsão calculada dos fins de semana**, mas não sobrescreve uma célula existente: `EscalaColaborador.eh_dia_trabalho` devolve primeiro `cel.trabalha`; só sem célula consulta o cálculo (`app/escala/models.py:1066`, `:1071`, `:1073`).

Também não se pode deduzir “novo chamado por remoção do veto”: `regua_degenerada` consulta os marcos, e `TipoEscala.marcos_do_dia` fornece horários independentemente de trabalho/folga (`app/escala/regua_defesa.py:26`; `app/escala/models.py:326`). Portanto, folga → trabalho não implica automaticamente veto → nascimento (`app/ponto/services/furos_vetados.py:40`).

Casos fixos relevantes:

| Módulo | Efeito identificável |
|---|---|
| `app/chamados/tests/test_agrupado_em.py:17` | O delta também troca 03/08 por **01/08**. A fixture efetiva era segunda de trabalho e passa a sábado de trabalho; não é o mesmo dia passando de folga a trabalho. Exercita abrir/reabrir (`app/chamados/tests/test_agrupado_em.py:32`, `:42`; comparação anterior: diff fornecido **[nao verificado]**). |
| `app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:52` | O sábado **05/09** permanece fixo e passa a trabalho no template do helper; a premissa regularizada é mockada como falsa, enquanto reabrir continua real (`app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:42`, `:69`, `:126`). |
| `app/chamados/tests/test_trilha_x_banco.py:28` | O primeiro caso monta o domingo **11/10**; muda o calendário disponível à abertura (`app/chamados/tests/test_trilha_x_banco.py:39`). |
| `app/ponto/tests/test_justificativa_encerra_origem.py:101` | O segundo caso do lote cria chamado no domingo **25/10**; muda o calendário disponível nesse cenário (`app/ponto/tests/test_justificativa_encerra_origem.py:86`, `:102`). |

**As dependências explícitas de folga/trabalho continuam presentes:**

- **Folga:** `test_reconciliar_pergunta_a_premissa` força `trabalha=False` antes e depois da abertura, mantém folga com ata velha e compara folga/trabalho na reabertura. O helper não apagou esses cenários (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196`, `:199`, `:212`, `:239`, `:250`).
- **Indefinição:** `test_a2_marco_morto` força `trabalha=None`, com contraprova de grade furada (`app/ponto/tests/test_a2_marco_morto.py:38`, `:48`).
- **Trabalho fixado na própria célula:** os módulos abaixo preservam esse booleano pela precedência da célula (`app/escala/models.py:1071`):

| Módulo | Fixture |
|---|---|
| `app/chamados/tests/test_b2d_resposta_visivel.py` | `:86` |
| `app/chamados/tests/test_c2_le_o_protesto_que_cobra.py` | `:40` |
| `app/chamados/tests/test_emissao_reata_orfa.py` | `:37` |
| `app/chamados/tests/test_emissao_reemite_c2.py` | `:48`, `:61` |
| `app/chamados/tests/test_fechar3_retorno_do_gate.py` | `:62` |
| `app/chamados/tests/test_reabrir_decide_pelo_banco.py` | `:42` |
| `app/chamados/tests/test_reabrir_pergunta_a_premissa.py` | `:47` |
| `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py` | `:43` |
| `app/chamados/tests/test_tranca_de_verdade.py` | `:60` |

Os usuários com **dia do fato relativo**, que exigem atenção ao calendário de execução, são estes; essa lista identifica datas sensíveis, não comprova mudança de resultado:

| Módulo | Referência |
|---|---|
| `app/api/tests/test_chamados_v2_destino.py` | `:37` |
| `app/chamados/tests/test_3c_abrir_ausencia.py` | `:36` |
| `app/chamados/tests/test_b8_smoke_reabertura.py` | `:36` |
| `app/chamados/tests/test_bug125_cobranca_cega.py` | `:56` |
| `app/chamados/tests/test_e5b_cobrar_massa.py` | `:24` |
| `app/chamados/tests/test_emissor_nomeia_o_marco.py` | `:44`, `:126` |
| `app/chamados/tests/test_f3_dedup_atraso.py` | `:55`, `:62` |
| `app/chamados/tests/test_fila_por_causa.py` | `:111` |
| `app/chamados/tests/test_orfa_sem_candidato.py` | `:37` |
| `app/chamados/tests/test_reconciliar_pergunta_orfa.py` | `:27` |
| `app/chamados/tests/test_regressao_etapa7.py` | `:79`, `:107`, `:328`, `:475` |
| `app/chamados/tests/test_resposta_tardia.py` | `:20` |
| `app/chamados/tests/test_selo_b5h_followup_motivo.py` | `:68`, `:82` |
| `app/chamados/tests/test_selo_dedup_premissa_viva.py` | `:90` |
| `app/chamados/tests/test_selo_f4_fatia4.py` | `:65` |
| `app/chamados/tests/test_selo_i6_ouve_a_lei.py` | `:63` |
| `app/chamados/tests/test_selo_porta_recusa.py` | `:63`, `:186` |
| `app/chamados/tests/test_validar_classe_a.py` | `:45` |
| `app/ponto/tests/test_a2_marco_morto.py` | `:31`, `:59` |
| `app/ponto/tests/test_bug114_fechador_de_dois_dias.py` | `:73` |
| `app/ponto/tests/test_bug115_lampada_do_marco.py` | `:54` |

Não há demonstração de folga explícita escondida pelo helper: **sem achado nesses cenários** (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196`, `:212`). Novo furo, chamado ou total previsto efetivamente produzido pela troca, e resultado da suíte: **[nao verificado]**.

**3. O spy intercepta a chamada real?**

**Sim — sem achado.** `impressao_insumos` é definida no próprio módulo; `_julgar_colab_corpo` a chama pelo nome global desse módulo. O patch substitui justamente esse atributo. Não há alias local/importado nessa chamada (`app/ponto/services/cartorio.py:314`, `:829`, `:1083`; `app/ponto/tests/test_veto_que_cai_rejulga.py:124`).

O percurso observado é `_julgar()` → `julgar_colab()` → `_julgar_colab_corpo()` → `impressao_insumos(..., veto=_veto_do_dia(data))` (`app/ponto/tests/test_veto_que_cai_rejulga.py:85`; `app/ponto/services/cartorio.py:744`, `:1083`).

**4. Ordem e ramo exercitado pelo MORDE**

**A ordem dos dois juízes coincide — sem achado:** `abrir` pergunta primeiro à célula e depois ao veto. `sem_juiz` é fallback diagnóstico do comando, não uma terceira guarda de `abrir` (`app/chamados/models.py:725`, `:731`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:184`).

**O MORDE exercita o fallback pretendido por injeção:** a fixture lavra `furo`, que não absolve; mantém vínculo 6x1 válido; o patch entrega `(None, False)` ao comando (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:41`, `:50`, `:69`, `:140`; `app/chamados/reconciliador.py:977`).

Ele não exercita o corpo real de `abrir` nem reproduz mudança concorrente do dia (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:140`). Também falta cenário com **ambos** os juízes recusando: inverter a prioridade célula/veto não seria distinguido pelos novos casos de veto isolado e nenhum juiz (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:124`, `:135`). Mutação executada: **[nao verificado]**.

**5. Leitores de `fim:` e campo novo**

**Sem achado na cópia.** O comando está declarado fora do pipeline, para execução por ato (`app/config/crons.py:743`, `:875`, `:884`). Os testes capturam `stdout` e verificam substrings, aceitando o campo acrescentado (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:77`, `:118`, `:142`).

O envelope de cron registra tempo e código de saída em JSON próprio; o placar lê esse JSON, sem interpretar o `fim:` textual (`bin/cron_run.sh:11`, `:13`; `app/core/management/commands/pipeline_placar.py:69`, `:72`, `:76`, `:80`).

Regex ou consumidor externo à cópia, e eventual quebra em produção: **[nao verificado]**.

## 30/10 13:54 · sessao 152a623de8b1 · - · - · origem=Ronald (tmux)

Achado 4: o helper realmente passou a `semanal`, com `dias_trabalho_semana='0,1,2,3,4,5,6'` (`app/chamados/tests/_vinculo_do_dia.py:18`). O template anterior `5x2` folgava sábado/domingo pelo fallback; o atual trabalha nesses dias (`app/escala/models.py:830-834`, `app/escala/models.py:852-861`). Isso muda o calendário da fixture, mas não demonstra automaticamente novo furo/chamado: a célula existente prevalece sobre o template (`app/escala/models.py:1066-1073`).

Casos fixos cujo calendário muda:

- `test_agrupado_em`: sábado `2026-08-01`, exercitando abrir/reabrir (`app/chamados/tests/test_agrupado_em.py:17`, `:32-45`).
- `test_selo_i4b_um_chamado_por_turno`: sábado `2026-09-05`, helper condicional; a premissa é explicitamente mockada como viva (`app/chamados/tests/test_selo_i4b_um_chamado_por_turno.py:37-44`, `:52-54`, `:66-77`).
- `test_trilha_x_banco`: o primeiro caso monta domingo `2026-10-11` por formatação (`app/chamados/tests/test_trilha_x_banco.py:28-30`, `:39`).
- `test_justificativa_encerra_origem`: o segundo caso do lote cria chamado de domingo `2026-10-25` (`app/ponto/tests/test_justificativa_encerra_origem.py:86-94`, `:97-109`).

Folga e indefinição próprias continuam explícitas:

- `test_reconciliar_pergunta_a_premissa` força `trabalha=False` antes e depois de abrir, e também mantém cenários de folga/trabalho (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196-201`, `:212-219`, `:239-259`).
- `test_a2_marco_morto` monta `trabalha=None` e contraprova com grade definida e furada (`app/ponto/tests/test_a2_marco_morto.py:38-42`, `:48-68`).

Módulos que fixam trabalho na célula, além dos relativos: `test_b2d_resposta_visivel.py:86`, `test_c2_le_o_protesto_que_cobra.py:40`, `test_emissao_reata_orfa.py:37`, `test_emissao_reemite_c2.py:48,61`, `test_fechar3_retorno_do_gate.py:62`, `test_reabrir_decide_pelo_banco.py:42`, `test_reabrir_pergunta_a_premissa.py:47`, `test_reconciliar_pergunta_a_premissa.py:43`, `test_tranca_de_verdade.py:60`, todos sob `app/chamados/tests/`. O helper não sobrescreve esses booleanos (`app/escala/models.py:1071-1073`).

Os 21 módulos com dia do fato relativo são:

- `app/api/tests/test_chamados_v2_destino.py:37`.
- Sob `app/chamados/tests/`: `test_3c_abrir_ausencia.py:36`; `test_b8_smoke_reabertura.py:36`; `test_bug125_cobranca_cega.py:56`; `test_e5b_cobrar_massa.py:24`; `test_emissor_nomeia_o_marco.py:44,126`; `test_f3_dedup_atraso.py:55,62`; `test_fila_por_causa.py:111`; `test_orfa_sem_candidato.py:37`; `test_reconciliar_pergunta_orfa.py:27`; `test_regressao_etapa7.py:79,107,328,475`; `test_resposta_tardia.py:20`; `test_selo_b5h_followup_motivo.py:68,82`; `test_selo_dedup_premissa_viva.py:90`; `test_selo_f4_fatia4.py:65`; `test_selo_i6_ouve_a_lei.py:63`; `test_selo_porta_recusa.py:63,186`; `test_validar_classe_a.py:45`.
- Sob `app/ponto/tests/`: `test_a2_marco_morto.py:31,59`; `test_bug114_fechador_de_dois_dias.py:73`; `test_bug115_lampada_do_marco.py:54`.

Os outros seis módulos com relógio relativo têm fato fixo e usam `now()` para carimbos: `test_b2d_resposta_visivel.py:16,53`, `test_competencia_trancada.py:20-21,46`, `test_laco_hx_x_bug31.py:37,70-75`, `test_reabrir_pergunta_a_premissa.py:24,95-97`, `test_tranca_de_verdade.py:22,38`, `test_validacao_respeita_tranca.py:30,51`, sob `app/chamados/tests/`. Portanto “27 módulos relativos” não significa 27 dias do fato relativos nesses usuários.

Feriados: não há mudança demonstrada pelo helper; ele usa `objects.create` sem preencher `trabalha_em_feriado`, cujo default é `True` nos dois casos (`app/chamados/tests/_vinculo_do_dia.py:25-26`; `app/escala/models.py:896-898`). A política estática que diferencia `5x2` de `semanal` não é chamada pelo helper (`app/escala/models.py:900-917`).

Resultado da suíte ou novo furo/chamado/previsto efetivamente produzido: `[nao verificado]`. Sem achado demonstrado de asserção sobre folga apagada pela mudança; os cenários explícitos preservam a folga e a indefinição (`app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:196-215`; `app/ponto/tests/test_a2_marco_morto.py:38-42`).

## 30/10 13:53 · sessao e52407c8c32d · - · - · origem=Ronald (tmux)

Achado 5 curado no código: recusa por célula, depois veto consultado explicitamente, depois `recusado_sem_juiz`; o contador deixou de atribuir veto por eliminação. A sequência coincide com os dois gates de nascimento de `abrir`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174-188`; `app/chamados/models.py:725-734`.)

O MORDE exercita o fallback pretendido por injeção: `_entrada_sem_saida` usa `acusa=True` e lavra `furo`; célula acusadora não absolve, e o patch faz `abrir` devolver `(None, False)`. Ele verifica `recusado_veto=0` e `recusado_sem_juiz=1`. Não comprova uma corrida real nem executa o corpo de `abrir`. (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:57-74`, `:135-143`; `app/chamados/reconciliador.py:977-979`.)

Mutantes plausíveis ainda permitidos pelos novos testes: [nao verificado]

- Trocar apenas o texto do fallback para `[RECUSADO veto]`, mantendo o contador `recusado_outro`: o MORDE só confere contadores, sem exigir `[RECUSADO sem juiz]`. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:184-188`; `app/ponto/tests/test_passe_turno_aberto_retroativo.py:140-143`.)
- Inverter a prioridade célula/veto quando ambos recusam: os novos casos cobrem veto isolado e ausência dos dois juízes, sem forçar dupla recusa. (`app/ponto/tests/test_passe_turno_aberto_retroativo.py:124-143`.)

Pergunta 5: **sem achado na cópia**. O comando está declarado fora do pipeline, executado por ato; os consumidores diretos encontrados são os testes, que capturam `stdout` e usam `assertIn`, tolerando o campo acrescentado. (`app/config/crons.py:743`, `:875-885`; `app/ponto/tests/test_passe_turno_aberto_retroativo.py:77-84`, `:118-143`.)

O envelope de cron usa código de saída e tempos para produzir seu próprio JSON; o placar lê desse JSON `nome`, `emp`, `exit` e `dur`, sem interpretar a linha textual `fim:`. (`bin/cron_run.sh:11-13`; `app/core/management/commands/pipeline_placar.py:60-80`.) Leitores externos/regex fora da cópia e execução dos testes: `[nao verificado]`.

## 30/10 13:52 · sessao 3f59d5d879cc · - · - · origem=Ronald (tmux)

1. **RED contra a base: cura não demonstrada pelo delta.** Os testes de nascimento e reabertura existem na cópia, mas sua existência não prova execução vermelha contra a base: `app/chamados/tests/test_o261_veto_na_porta.py:418`, `:476`, `:500`, `:522`. Não encontrei o log `red_reabrir.log`, árvore Git ou mensagem de commit que permitam conferir aquisição e hash; execução contra a base é `[nao verificado]`. O delta fornecido não altera esses testes nem acrescenta essa prova `[nao verificado]`.

2. **Linha LEI-AKITA: cura não demonstrada pelo delta.** A origem atual realmente inclui `recusa_da_reabertura`, que pergunta o veto em `app/chamados/models.py:1090`, e `reabrir`, que consulta essa função e grava a recusa em `app/chamados/models.py:1143`. O leitor omitido também existe: `app/escala/management/commands/desfazer_corroboracao_regua.py:25` importa `furos_vetados_por_regua`; `:38` consome a lista e `:39` filtra por vínculo. A linha de commit corrigida não está disponível `[nao verificado]`.

   Nesse consumidor específico, `vinculo=None` não quebra a leitura: o produtor retorna essa chave com `None` em `app/ponto/services/furos_vetados.py:69`, e o filtro por `ec.pk` o exclui em `app/escala/management/commands/desfazer_corroboracao_regua.py:39`. **Sem achado funcional nesse caminho.**

Para 1/2, não há teste novo dedicado no delta fornecido; eventual “mutante sobrevivente” desses itens não foi executado `[nao verificado]`. [nao verificado]

## 30/10 13:50 · sessao 13957253312a · - · - · origem=Ronald (tmux)

OK

## 30/10 13:40 · sessao bdc58fb8ddda · O261-VETO-A2 · censo · origem=Code

As referências aos anexos contam a primeira linha após cada cabeçalho `== anexo: ... ==` como linha 1.

1. **RED contra a base — classe (c), fora do alcance.**  
   **Pergunta:** incluía “selo/RED sem mordida”. (`pergunta_final.q:8`)  
   **Resposta:** delimitou “Execução da bateria Django [...] [nao verificado]”. Não há trecho que identifique o RED executado na cópia intermediária. (`codex_final.txt:3`)  
   O achado depende da execução histórica e da base efetivamente usada, informadas somente no relato do Opus; sua reprodução independente é **[nao verificado]**. (`opus_achados.txt:2`)

2. **Linha LEI-AKITA desatualizada — classe (a), não apontado com o tema presente.**  
   **Pergunta:** exigia “censo quem-mais-le incompleto” e “QUEM ESCREVE e QUEM LE cada campo/valor”. (`pergunta_final.q:8`; `pergunta_final.q:11`)  
   **Resposta:** identificou expressamente o leitor: “desfazer corroboracão filtra pelo vínculo”, citando `desfazer_corroboracao_regua.py:38`; também examinou a reabertura direta. Portanto, viu esses caminhos, mas não apontou sua ausência na declaração LEI-AKITA. (`codex_final.txt:21`; `codex_final.txt:50`)  
   A omissão documental específica não consta da triagem fornecida; não há sustentação para classificá-la como descarte pela triagem. (`triagem_final.txt:2–16`)

3. **Impressão com `ec=None` sem teste próprio — classe (a), não apontado com o tema presente.**   [nao verificado]
   **Pergunta:** nomeava explicitamente “ec=None vale TAMBEM para a impressao”, os leitores de cartório/impressão e a classe “selo/RED sem mordida”. (`pergunta_final.q:6`; `pergunta_final.q:8`; `pergunta_final.q:11`)  
   **Resposta:** descreveu que o cartório “Acrescenta `veto:VETO_DEGENERADA` à impressão do ramo normal”, mas não denunciou a falta de teste desse contrato nem o mutante indicado pelo Opus. (`codex_final.txt:28`; `opus_achados.txt:4`)   [nao verificado]
   O achado colocado em FILA foi outro: impressão do ramo `nunca_bateu` sem veto. Isso não demonstra descarte do achado sobre cobertura de `ec=None` no ramo normal. (`codex_final.txt:58`; `triagem_final.txt:8`)

4. **Helper com folga no fim de semana — classe (a), não apontado com o tema presente.**  
   **Pergunta:** nomeava o helper e perguntava se algum teste “ficou verde pelo motivo errado” ou “ficou vacuo”. (`pergunta_final.q:12`)   [nao verificado]
   **Resposta:** declarou “sem achado de intenção ‘sem vínculo/sem escala’ mascarada pela migração” e apontou dois testes que não provavam nascimento; não apontou a dependência do calendário descrita pelo Opus. (`codex_final.txt:36`; `codex_final.txt:38`; `opus_achados.txt:5`)   [nao verificado]
   Classifico a configuração do helper como inspecionável pelo código. A contagem histórica de 27 módulos e o resultado efetivo da suíte permanecem **[nao verificado]**. A cópia atual já usa sete dias de trabalho e atribui essa alteração ao Opus final. (`app/chamados/tests/_vinculo_do_dia.py:5–6`; `app/chamados/tests/_vinculo_do_dia.py:18`)

5. **`recusado_veto` contado por eliminação — classe (a), não apontado com o tema presente.**  
   **Pergunta:** exigia examinar quem determina e lê cada campo/valor, incluindo os “emissores”, além de procurar derivações paralelas de fonte única. (`pergunta_final.q:8`; `pergunta_final.q:11`)  
   **Resposta:** examinou nominalmente o retroativo: “lê indiretamente o juiz pelo dry `veto_no_nascimento`”. Não apontou a classificação da recusa no caminho de aplicação. (`codex_final.txt:34`)  
   A cópia atual contém uma consulta explícita ao juiz antes de incrementar `recusado_veto`, com comentário atribuindo a correção ao Opus; o defeito anterior é o registrado no anexo. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:179–187`; `opus_achados.txt:6`)

6. **Condições operacionais do pouso e medidas — classe (c), fora do alcance, com risco já visto.**   [nao verificado]
   **Pergunta:** exigia “reversao antes, exportada intacta por hash” e procurar “pouso antes do pre-requisito”; também nomeava `furos_sem_cobranca`. (`pergunta_final.q:6`; `pergunta_final.q:8`; `pergunta_final.q:11`)   [nao verificado]
   **Resposta:** registrou “Reversão anterior, preservação por hash e diferença à Pauta DP: [nao verificado]”. Na DELTA, reiterou: “Item 9 continua pendente de pouso, embora esteja como MEDINDO”. (`codex_final.txt:64`; `codex_delta.txt:45`)   [nao verificado]
   A triagem manteve a medida “antes de qualquer pouso”; portanto, há evidência de reconhecimento da pendência, sem evidência de descarte. A execução das condições, a divergência “63 × 1.576” e os resultados antes/depois não são demonstrados pelo material fornecido: **[nao verificado]**. (`triagem_final.txt:15–16`; `opus_achados.txt:7`) [nao verificado]

7. **Ato humano sem mostrar/gravar o veto ultrapassado — classe (a), não apontado com o tema presente.**  
   **Pergunta:** trazia literalmente “ato humano passa com trilha”; a DELTA 2 voltou a pedir conferência das três cadeias `humano=True`. (`pergunta_final.q:5`; `pergunta_delta2.q:4`)  
   **Resposta:** declarou “sem achado nas três cadeias humanas atuais”, verificando permissões e origem humana, mas não apontou a ausência do veto ultrapassado na apresentação/auditoria. (`codex_delta2.txt:7–9`)  
   Na cópia, `humano=True` dispensa a consulta ao veto nessa guarda; as notas e auditorias examinadas registram o ato administrativo sem nomear esse veto. Isso sustenta a distinção entre registrar a ação humana e registrar qual veto ela ultrapassou. (`app/chamados/models.py:1090–1093`; `app/chamados/services/acoes_disputa.py:266–270`; `app/chamados/services/competencia_trancada.py:258–264`)

| Achado | Classe | Referência principal |
|---|---|---|
| 1. RED contra a base | **(c)** | `codex_final.txt:3`; `opus_achados.txt:2` |
| 2. LEI-AKITA desatualizada | **(a)** | `pergunta_final.q:8`; `codex_final.txt:21` |
| 3. Teste da impressão com `ec=None` | **(a)** | `pergunta_final.q:6`; `codex_final.txt:28` | [nao verificado]
| 4. Helper dependente do calendário | **(a)** | `pergunta_final.q:12`; `codex_final.txt:36–38` |
| 5. Contador por eliminação | **(a)** | `pergunta_final.q:11`; `codex_final.txt:34` |
| 6. Condições e medidas do pouso | **(c)**, com pendência reconhecida | `codex_delta.txt:45`; `triagem_final.txt:15–16` | [nao verificado]
| 7. Trilha do veto ultrapassado pelo humano | **(a)** | `pergunta_final.q:5`; `codex_delta2.txt:7–9` |

## 30/10 13:36 · sessao ff42a8bee5de · O261-VETO-A2 · sonda · origem=Code

`sonda_fsc.py` não foi localizado na cópia; não houve nova execução. Segue integralmente a saída fornecida. [nao verificado]

```text
sonda_leitura: sonda_fsc.py [fe36a5107d6c7951] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

TOTAL=61
POR_PORTA|pergunta_ao_colab=45
POR_PORTA|supervisao_sem_push=16
POR_PORTA|veto_celula=1
POR_EMPRESA|2=61
ITENS=62
FIM-DA-SONDA
```

## 30/10 13:30 · sessao 3a14b0861375 · CALENDARIO-HOJE · censo · origem=Code

Analisei a árvore disponível em modo de leitura. A correspondência exata dela com o commit `d2c37edd16a8b0ca3aa26c2eab9f60de661fc675` ficou **[nao verificado]**, pois o snapshot não oferece os metadados Git necessários à conferência.

**1. Classificador e resultado para HOJE**

A função é `_classificar(f)`: recebe os fatos já carregados e devolve uma string de status. O calendário passa `'futuro': data > hoje`; portanto, **HOJE recebe `False`**. (`app/colaboradores/services/calendario.py:104`, `app/colaboradores/services/calendario.py:779`, `app/colaboradores/services/calendario.py:785`)

Para trabalho sem batida, sem isenção, escala suspeita, cobertura ou veredito que altere o resultado:

| Valor de `futuro` | Resultado |
|---|---|
| `True` | `previsto`. (`app/colaboradores/services/calendario.py:130`, `app/colaboradores/services/calendario.py:131`) |
| `False` | `falta`. É o caminho comum de HOJE na implementação atual. (`app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:785`) |

Essas três premissas — hoje, trabalho e nenhuma batida — não bastam para garantir `falta`: com `futuro=False`, isento recebe `ok`, escala suspeita recebe `escala_suspeita`, cobertura sem desconto recebe `ausencia`; o veredito também pode substituir o status. Ausências e feriado têm precedência anterior; cobertura sem desconto pode transformar até `previsto` em `ausencia`. (`app/colaboradores/services/calendario.py:120`, `app/colaboradores/services/calendario.py:123`, `app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:135`, `app/colaboradores/services/calendario.py:164`, `app/colaboradores/services/calendario.py:175`, `app/colaboradores/services/calendario.py:179`)

Os status existentes são **`ok`, `irregular`, `folga_trabalhada`, `falta`, `escala_suspeita`, `aberto`, `previsto`, `folga`, `ausencia`, `sem_certidao`, `sem_dados`**. Hora extra é selo separado, não status. (`app/colaboradores/services/calendario.py:44`, `app/colaboradores/services/calendario.py:60`, `app/colaboradores/services/calendario.py:116`)

**2. Quem lê este calendário**

A chave **`futuro` é interna ao classificador**: não integra o dicionário exportado de cada dia. Os leitores recebem `status`, `status_rotulo`, `eh_trabalho`, `resolver_dia_oferecido` e os demais campos; outras grades têm suas próprias chaves chamadas `futuro`. (`app/colaboradores/services/calendario.py:785`, `app/colaboradores/services/calendario.py:855`, `app/colaboradores/services/calendario.py:889`, `app/colaboradores/services/calendario.py:904`)

| Leitor | O que mostra ou decide | Regra própria de hoje/futuro |
|---|---|---|
| **Perfil do colaborador no admin** | A view monta o calendário e renderiza o partial; a grade usa status para fundo/borda e ícone: falta vermelha, previsto azul, ok verde, irregular amarelo etc. Ausência e palavra do dia podem ocupar o lugar do ícone. (`app/colaboradores/views.py:1945`, `app/colaboradores/views.py:1962`, `app/templates/colaboradores/partials/_calendario_grade.html:69`, `app/templates/colaboradores/partials/_calendario_grade.html:97`) | A view acrescenta outro `hoje=localdate()` quando há permissão de regularização; a grade lê o carimbo `resolver_dia_oferecido`, sem refazer a comparação temporal para oferecer os botões. (`app/colaboradores/views.py:1957`, `app/colaboradores/views.py:1960`, `app/templates/colaboradores/partials/_calendario_grade.html:125`) |
| **Gaveta de diagnóstico/painel** | Monta calendário customizado e reutiliza a mesma grade em fita, com os mesmos status e totais. (`app/colaboradores/services/drawer.py:120`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:33`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`) | Usa `localdate()` para escolher competência atual e anterior e aplicar piso visual; não reclassifica o status recebido. (`app/colaboradores/services/drawer.py:111`, `app/colaboradores/services/drawer.py:113`, `app/colaboradores/services/drawer.py:118`) |
| **Mapa de furos HTML** | Cada card inclui este calendário. Entretanto, seleção dos cards, lista de marcos faltantes, `nfuro` e ordenação vêm de `grade_da_celula → estado_do_dia`, independentemente do status deste calendário. (`app/relatorios/furos.py:141`, `app/relatorios/furos.py:160`, `app/relatorios/furos.py:164`, `app/relatorios/furos.py:184`, `app/relatorios/furos.py:190`, `app/templates/relatorios/furos.html:42`) | A view usa `localdate()`; o período padrão termina hoje, enquanto o período explícito não recebe ali um teto em hoje. (`app/relatorios/views.py:1518`, `app/relatorios/views.py:1531`, `app/relatorios/views.py:1537`, `app/relatorios/views.py:1541`) |
| **PDF do mapa de furos** | Extrai `data → status` dos dias do calendário e pinta o minicalendário conforme esse status. Status sem entrada nos mapas de cores recebem fundo branco/texto cinza. O número de furos continua vindo da outra contagem. (`app/relatorios/views.py:1548`, `app/relatorios/views.py:1551`, `app/relatorios/furos_pdf.py:11`, `app/relatorios/furos_pdf.py:21`, `app/relatorios/furos_pdf.py:56`, `app/relatorios/furos_pdf.py:66`) | O gerador recebe o período da view e percorre as semanas prontas; não compara com o relógio nesse render. (`app/relatorios/views.py:1562`, `app/relatorios/furos_pdf.py:25`, `app/relatorios/furos_pdf.py:47`) |
| **Resolver dia / Pauta DP** | O serviço deriva `resolver_dia_oferecido` para status `falta`, `irregular`, `aberto`, `sem_certidao`, exigindo último recurso e trabalho. O template acrescenta a permissão e oferece seleção em lote, Resolver dia e DP. (`app/colaboradores/services/calendario.py:188`, `app/colaboradores/services/calendario.py:196`, `app/templates/colaboradores/partials/_calendario_grade.html:125`) | Exige **passado estrito: `data < hoje`**. Assim, falta de HOJE não oferece essa ação por este predicado. (`app/colaboradores/services/calendario.py:196`) |
| **Vigia do fio morto** | Chama a porta de um dia `resolver_dia_oferecido_na_data`; resultado verdadeiro entra em `decisao_do_dia_pendente` e deixa de seguir como fio morto. (`app/colaboradores/services/calendario.py:205`, `app/colaboradores/services/calendario.py:208`, `app/chamados/services/vigia_fio.py:294`, `app/chamados/services/vigia_fio.py:298`) | Tem `hoje` próprio: conta `data > hoje` no tripwire de futuro e separa `data >= hoje` antes de perguntar ao calendário. O calendário chamado captura outro relógio. (`app/chamados/services/vigia_fio.py:181`, `app/chamados/services/vigia_fio.py:278`, `app/chamados/services/vigia_fio.py:289`, `app/colaboradores/services/calendario.py:249`) |
| **Contadores internos / rodapé** | `resumo` conta literalmente os status; HE conta o selo. O rodapé mostra `faltas` como **“Sem registro”**, além de Ok, HE, trabalho em folga, irregulares, escala suspeita, folgas e abertos; `ausencias` e `previstos` são calculados, mas não têm bloco nesse rodapé. (`app/colaboradores/services/calendario.py:908`, `app/colaboradores/services/calendario.py:923`, `app/colaboradores/services/calendario.py:925`, `app/templates/colaboradores/partials/_calendario_grade.html:141`, `app/templates/colaboradores/partials/_calendario_grade.html:145`, `app/templates/colaboradores/partials/_calendario_grade.html:173`) | A contagem percorre os status já produzidos, sem corte temporal adicional. (`app/colaboradores/services/calendario.py:908`) |
| **Portas `status_do_dia` / `contar_por_status`** | Montam respectivamente um dia ou um período; devolvem o status ou `Counter(status)`. (`app/colaboradores/services/calendario.py:211`, `app/colaboradores/services/calendario.py:220`, `app/colaboradores/services/calendario.py:223`, `app/colaboradores/services/calendario.py:231`) | Herdam o relógio de `contexto_calendario`. Não localizei chamadores de produção dessas duas portas; ausência de chamadas dinâmicas **[nao verificado]**. (`app/colaboradores/services/calendario.py:217`, `app/colaboradores/services/calendario.py:228`, `app/colaboradores/services/calendario.py:249`) |

O comando `vigiar_fio_morto` está declarado na corrente automática, com `--alarme --alarme-passivo`; execução efetiva dessa configuração no ambiente de produção **[nao verificado]**. (`app/config/crons.py:1654`)

**App/API e cartão de ponto seguem outra cadeia:**

| Superfície | Fonte e comportamento |
|---|---|
| **API do app `/api/espelho/v2/`** | Lê `espelho_do_colab`, não este calendário. Para dia sem batidas, calcula `_futuro = data > hoje`; em HOJE de trabalho, mantém “futuro” se não houver fim previsto ou `agora < fim_turno`. Nesse caminho entrega `previsto`; depois do fim pode entregar `falta`, observadas isenção/cobertura. (`app/api/urls.py:81`, `app/api/views.py:1363`, `app/api/views.py:1369`, `app/api/views.py:1629`, `app/api/views.py:1632`, `app/api/views.py:1635`, `app/api/views.py:1650`, `app/api/views.py:1653`) |
| **API, dias com batidas** | Usa vocabulário próprio: `aberto`, `alerta` ou `ok`; publica `status` no JSON. A apresentação efetiva no cliente móvel externo **[nao verificado]**. (`app/api/views.py:1570`, `app/api/views.py:1591`, `app/api/views.py:1700`) |
| **Cartão/PDF de ponto** | Lê os dias de `espelho_do_colab`. Aplica `max(data_ini, ate_ontem(data_fim, hoje=localtime(agora).date()))`; `ate_ontem` retorna `min(data_fim, hoje-1)`. O `max` exige ressalva para janela integralmente futura: o corte isoladamente não garante excluir todo hoje/futuro nesse caso. (`app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:598`, `app/relatorios/cartao_pela_celula.py:37`) |
| **Palavra/contador “Em aberto” compartilhado** | Vem de `folha_manda → datas_em_aberto_de`, usando teto `ate_ontem` para furos e retirando o turno em curso; não deriva de `status == 'falta'` deste calendário. (`app/relatorios/cartao_pela_celula.py:395`, `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:418`, `app/ponto/services/dia_decidido.py:311`) |

Outra chave homônima é a de `clipar_grade_janela`, que neutraliza células futuras da grade do espelho; outra ainda pertence ao calendário de fase, cujo template lê `c.futuro` e `c.hoje`. Não são o fato interno passado a `_classificar`. (`app/escala/utils.py:934`, `app/escala/utils.py:942`, `app/ponto/services/espelho.py:688`, `app/templates/colaboradores/partials/_calendario_fase.html:12`, `app/templates/colaboradores/partials/_calendario_fase.html:14`)

**3. Outros sítios que usam DATA para decidir falta/furo ou se ainda cabe cobrança**

| Sítio | Comparação e efeito |
|---|---|
| **API do app** | `data > hoje`; HOJE recebe também teste por instante contra `fim_turno`, conforme descrito acima. (`app/api/views.py:1629`, `app/api/views.py:1632`) |
| **Espelho `_status_dia`** | Após as precedências de ausência, inconsistência, batidas, isenção e folga, previsto com `data < hoje` recebe `sem_registro`; HOJE sem esses fatos cai em `vazio`. (`app/ponto/services/espelho.py:152`, `app/ponto/services/espelho.py:169`) |
| **Grade `clipar_grade_janela`** | `dia > hoje` marca futuro, troca células por `None` e limpa órfãs, retirando indicação de marco faltante. HOJE passa por esse corte. (`app/escala/utils.py:942`) |
| **Cartório `julgar_celula`** | Recusa `data > hoje`. Para HOJE de trabalho, sem `teto_de_hoje`, espera fim do turno; folga pula essa espera. Essa guarda usa `localdate()`/`now()` próprios, inclusive quando há argumento `agora`. (`app/ponto/services/cartorio.py:1536`, `app/ponto/services/cartorio.py:1545`, `app/ponto/services/cartorio.py:1553`, `app/ponto/services/cartorio.py:1557`) |
| **Cartório `julgar_colab`** | Distingue hoje/ontem para marcos vencidos; passado cujo turno não encerrou é marcado em curso por `data < hoje and not _turno_encerrado(data)`. (`app/ponto/services/cartorio.py:1048`, `app/ponto/services/cartorio.py:1053`, `app/ponto/services/cartorio.py:1082`) |
| **Fábrica da célula** | `cel.data > hoje` não gera motivos. Sem marcos, passado recebe `None` para seguir o caminho da lei das batidas; hoje/futuro recebe conjunto vazio e espera. (`app/chamados/services/fabrica_da_celula.py:28`, `app/chamados/services/fabrica_da_celula.py:30`, `app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/fabrica_da_celula.py:85`) |
| **Cobrança manual `cobrar_dia`** | Recusa `data >= localdate()`, afirmando que o turno ainda não terminou. (`app/chamados/services/cobrar_dia.py:16`, `app/chamados/services/cobrar_dia.py:26`) |
| **Retenção de pergunta no app** | Chamado de furo sem pergunta com `dia >= hoje` não recebe causa de retenção: ainda não é hora. (`app/chamados/services/pergunta_no_app.py:56`, `app/chamados/services/pergunta_no_app.py:64`) |
| **Reconciliador de órfãs** | `data_evento >= localtime(agora).date()` retorna `TETO` antes de consultar `dia_encerrado_motivo`. (`app/chamados/reconciliador.py:1036`, `app/chamados/reconciliador.py:1038`) |
| **Suprajuiz** | Furos usam o resultado de encerramento; sobrevive comparação civil na acusação de cadastro de trabalho sem vínculo/previsto: `not (hoje and data and data >= hoje)`. (`app/ponto/supra_juiz.py:253`, `app/ponto/supra_juiz.py:254`, `app/ponto/supra_juiz.py:405`) |
| **Apuração `furos_diarios`** | Corta em `min(fim, hoje-1)`; HOJE inteiro fica fora da apuração/contagem. A borda reaparece na manutenção e no universo. (`app/escala/services/furos_diarios.py:24`, `app/escala/services/furos_diarios.py:26`, `app/escala/services/furos_diarios.py:99`, `app/escala/services/furos_diarios.py:282`) |
| **Cartão e “Em aberto”** | `ate_ontem` corta em hoje−1; `datas_em_aberto_de` limita furos apurados a `d <= ate`, enquanto turnos abertos e saídas órfãs entram sem esse teto civil. (`app/relatorios/cartao_pela_celula.py:37`, `app/ponto/services/dia_decidido.py:311`, `app/ponto/services/dia_decidido.py:316`) |
| **Validação de cartão/PDF** | Recusa fim posterior a ontem; tem mensagens distintas para HOJE e futuro. O default de hoje usa `date.today()`. (`app/relatorios/services.py:35`, `app/relatorios/services.py:38`, `app/relatorios/services.py:41`, `app/relatorios/services.py:45`) |
| **Prévia da folha** | Exclui `d >= hoje` antes de propor preenchimento dos marcos apagados. (`app/folha/previa.py:77`, `app/folha/previa.py:79`, `app/folha/previa.py:84`) |
| **Retenção no export da folha** | Célula ausente/sem veredito gera retenção `sem_celula` apenas com `d < hoje`, ressalvado o dia da demissão; célula já acusando segue outro ramo. (`app/folha/export.py:1211`, `app/folha/export.py:1215`, `app/folha/export.py:1237`) |
| **Regularização de retorno** | Interrompe a criação de faltas intermediárias em `dia >= hoje`. (`app/ponto/services/regularizacao.py:108`, `app/ponto/services/regularizacao.py:113`, `app/ponto/services/regularizacao.py:118`) |
| **Abandono provisório** | Cria faltas até ontem; para em `dia >= hoje`, pois ele ainda pode aparecer hoje. (`app/ponto/management/commands/marcar_abandono_provisorio.py:69`, `app/ponto/management/commands/marcar_abandono_provisorio.py:72`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76`) |
| **Declaração manual/chat** | Recusa falta apenas com `data > hoje`; essa guarda permite HOJE. (`app/ponto/services/declarar_dia.py:94`, `app/ponto/services/declarar_dia.py:100`) |

Também há cortes civis nos comandos e contadores:

| Sítio | Corte |
|---|---|
| `reconciliar_grade` | Pula `dt >= hoje`; depois verifica vencimento por instante dos marcos restantes. (`app/ponto/management/commands/reconciliar_grade.py:134`, `app/ponto/management/commands/reconciliar_grade.py:169`) |
| `processar_cartorio` | Lê `localdate()` e `now()` separadamente e varre somente `data__lt=hoje`. (`app/ponto/management/commands/processar_cartorio.py:38`, `app/ponto/management/commands/processar_cartorio.py:39`, `app/ponto/management/commands/processar_cartorio.py:50`) |
| `furos_sem_cobranca` | Considera furos até ontem. (`app/ponto/services/furos_sem_cobranca.py:53`, `app/ponto/services/furos_sem_cobranca.py:61`) |
| `vigia_entrada` | Considera células de trabalho anteriores a hoje. (`app/ponto/services/vigia_entrada.py:73`, `app/ponto/services/vigia_entrada.py:79`) |
| `folha.services.sem_furo` | Consulta até `min(fim, ontem)`. (`app/folha/services/sem_furo.py:39`, `app/folha/services/sem_furo.py:51`) |
| `emitir_turno_aberto_retroativo` | Se o fim pedido for `>= hoje`, recua para ontem. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:72`) |
| `retratar_chamados_lei_cala` | Usa `date.today()` e pula `D >= hoje` antes de classificar a falta. (`app/chamados/management/commands/retratar_chamados_lei_cala.py:32`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:43`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`) |
| `mapa_divergencia` | Separa `data > hoje` e limita a análise de divergências ao restante; HOJE entra no grupo passado. (`app/ponto/management/commands/mapa_divergencia.py:162`, `app/ponto/management/commands/mapa_divergencia.py:164`) |

Para “ainda pode bater”, há ainda decisões por **instante**: `faltas_de_hoje_sem_cobranca` só conta após vencer a entrada prevista mais sua tolerância; `vigia_de_hora` julga marcos de hoje/ontem e consulta encerramento com `agora - 2*teto`. O antigo corte civil de `flip_auto` foi substituído pela chamada a `dia_encerrado`. (`app/ponto/services/faltas_de_hoje.py:28`, `app/ponto/services/faltas_de_hoje.py:48`, `app/ponto/services/faltas_de_hoje.py:68`, `app/ponto/services/vigia_de_hora.py:46`, `app/ponto/services/vigia_de_hora.py:57`, `app/ponto/services/vigia_de_hora.py:130`, `app/ponto/services/flip_auto.py:285`)

**4. `dia_encerrado` e `dia_encerrado_motivo`**

Ambas recebem `(colaborador, data, agora=None, celulas=None)`. `agora` deve ser datetime aware e, omitido, nasce de `timezone.now()`. `dia_encerrado` devolve exatamente `dia_encerrado_motivo(...) is None`. O mapa de células é repassado a `turnos_do_colab`, cujo contrato é `{data: CelulaDia}`. (`app/ponto/turnos.py:2014`, `app/ponto/turnos.py:2052`, `app/ponto/turnos.py:2058`, `app/ponto/turnos.py:2073`, `app/ponto/turnos.py:2077`, `app/ponto/turnos.py:1852`)

A ordem efetiva é:

| Condição | `dia_encerrado_motivo` | `dia_encerrado` |
|---|---|---|
| Há aberta viva com expiração prevista | `turno_aberto_relogio` | `False`. (`app/ponto/turnos.py:2097`, `app/ponto/turnos.py:2099`) |
| Há aberta viva sem expiração prevista | `turno_aberto` | `False`. (`app/ponto/turnos.py:2101`) |
| Sem aberta viva, mas sem fim previsto | `sem_previsto` | `False`. (`app/ponto/turnos.py:2102`, `app/ponto/turnos.py:2103`) |
| Sem aberta viva, `agora < fim` | `relogio` | `False`. (`app/ponto/turnos.py:2105`) |
| Sem aberta viva, `agora >= fim` | `None` | `True`. (`app/ponto/turnos.py:2105`, `app/ponto/turnos.py:2058`) |

A aberta expira pelo **fim do template + 4 horas**, somente quando `agora > expiração`: no instante exato ainda está viva. Já o fim previsto usado no segundo teste lê o vínculo e seu DNA de célula. (`app/ponto/turnos.py:1981`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:2007`, `app/ponto/turnos.py:2011`, `app/ponto/turnos.py:2102`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1099`)

Na virada da meia-noite, os marcos são encadeados: cada horário que não for posterior ao anterior avança um dia. Portanto, turno de D pode continuar vivo em D+1; fechar o primeiro bloco de um turno partido também não basta antes do fim previsto do conjunto. (`app/escala/servico_jornada.py:74`, `app/escala/servico_jornada.py:81`, `app/escala/servico_jornada.py:89`, `app/ponto/turnos.py:2033`)

**Sem célula não implica `False`; folga também não tem retorno especial.** O julgamento não consulta `trabalha`: sem DNA de marcos, `EC.marcos_do_dia` recorre ao template. Se não houver vínculo/tipo/fim, retorna `sem_previsto`, salvo a precedência de uma aberta viva. (`app/ponto/turnos.py:2095`, `app/ponto/turnos.py:2102`, `app/escala/models.py:1097`

[truncado em 20000 de 26230 caracteres]

## 30/10 13:27 · sessao 62633b9a6473 · - · - · origem=Ronald (tmux)

**Entradas e retorno.** `dia_encerrado(colaborador, data, agora=None, celulas=None)` retorna booleano pela expressão `dia_encerrado_motivo(...) is None`; ambos compartilham assinatura e carregamento, com `agora` omitido resolvido por `timezone.now()`. O contrato documentado pede datetime aware; `celulas` segue para `turnos_do_colab`, cujo formato é `{date: CelulaDia}`. (`app/ponto/turnos.py:2014`, `app/ponto/turnos.py:2052`, `app/ponto/turnos.py:2058`, `app/ponto/turnos.py:2073`, `app/ponto/turnos.py:2077`, `app/ponto/turnos.py:1852`.)

**Motivos e precedência.** O julgamento primeiro pergunta por aberta viva: havendo aberta com expiração prevista, retorna `'turno_aberto_relogio'`; aberta que não expira retorna `'turno_aberto'`. Sem aberta viva, ausência de fim previsto retorna `'sem_previsto'`; fim previsto ainda não alcançado retorna `'relogio'`; `agora >= fim` retorna `None`, portanto `dia_encerrado=True`. (`app/ponto/turnos.py:2066`, `app/ponto/turnos.py:2070`, `app/ponto/turnos.py:2095`.)

**Bordas temporais.** A aberta expira somente quando `agora > fim_previsto + 4h`, portanto continua viva no instante exato da expiração; o outro teste usa `agora >= fim`. Expiração da aberta consulta o template; o fim do dia consulta `ec.marcos_do_dia`, isto é, DNA da célula quando disponível. (`app/ponto/turnos.py:1981`, `app/ponto/turnos.py:1991`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2007`, `app/ponto/turnos.py:2102`, `app/escala/servico_jornada.py:68`, `app/escala/models.py:1099`.)

**Meia-noite e partido.** `fim_turno` toma o último marco de `marcos_turno`; a sequência avança um dia sempre que o próximo horário não é posterior ao anterior. Assim, noite de D pode continuar aberta em D+1; um primeiro bloco fechado não basta para encerrar um dia partido. Há testes explícitos para noturno aberto às 06h de D+1 e partido ainda aberto às 23h. (`app/escala/servico_jornada.py:74`, `app/escala/servico_jornada.py:81`, `app/escala/servico_jornada.py:89`, `app/ponto/tests/test_juiz_dia_encerrado.py:103`, `app/ponto/tests/test_juiz_dia_encerrado.py:120`.)

**Sem célula, folga ou sem escala.** Não há consulta a `trabalha` ou `eh_dia_trabalho` no julgamento: ele decide por aberta e marcos. Sem célula, `EC.marcos_do_dia` recorre ao template; portanto “sem célula” não significa automaticamente “não encerrado”. O template explicitamente não decide trabalho/folga. Se não houver vínculo/tipo/fim, o motivo será `'sem_previsto'`, salvo aberta viva, que tem precedência. Célula de folga também é julgada pelos marcos efetivamente presentes no DNA/template, sem tratamento específico de folga nesta função. (`app/ponto/turnos.py:2095`, `app/escala/models.py:1097`, `app/escala/models.py:1108`, `app/escala/models.py:326`.)

**Chamadores atuais em código de produção.**

| Leitor | Uso |
|---|---|
| `detectar_ausencias::_hora_de_cobrar` | Só cobra entrada se houver marco vencido e o dia **não** estiver encerrado. (`app/ponto/management/commands/detectar_ausencias.py:460`, `app/ponto/management/commands/detectar_ausencias.py:470`.) |
| `flip_auto::avaliar_fila` | Pula a batida e conta `dia_em_curso` quando o juiz retorna falso. (`app/ponto/services/flip_auto.py:285`.) |
| Fechamento, filtro de faltas afirmáveis | Acrescenta a data à lista somente quando encerrada; passa células recortadas pelo pad. (`app/ponto/services/fechamento.py:349`, `app/ponto/services/fechamento.py:353`, `app/ponto/services/fechamento.py:361`.) |
| `vigia_de_hora::_ontem_em_janela` | Mantém ontem na janela enquanto não estava encerrado em `agora - 2*TETO_MARCO_MIN`; contém atalho anterior ao fim + dois tetos. (`app/ponto/services/vigia_de_hora.py:26`, `app/ponto/services/vigia_de_hora.py:50`.) |
| `reconciliador::veredito_orfa` | Único chamador externo direto do motivo encontrado: após teto próprio `data_evento >= localtime(agora).date()`, encerra com motivo `None`, adia com `'relogio'`/`'turno_aberto_relogio'`, e deixa sem encerramento nos demais motivos. (`app/chamados/reconciliador.py:1008`, `app/chamados/reconciliador.py:1036`, `app/chamados/reconciliador.py:1041`.) |

**Versão de período.** Existe `dias_encerrados(colaborador, ini, fim, agora=None, celulas=None) -> {date: bool}`: lê `agora` uma vez, carrega turnos uma vez e percorre o intervalo inclusivo usando o mesmo julgamento. É por colaborador, não um carregador de frota. (`app/ponto/turnos.py:2108`, `app/ponto/turnos.py:2118`, `app/ponto/turnos.py:2124`, `app/ponto/turnos.py:2126`, `app/ponto/turnos.py:2129`.)

Seus leitores são cartório, comando `supra_juiz`, emissor retroativo e `par_completo_da_janela`; os três primeiros fornecem o resultado como `turno_encerrado` a `classificar_dia`, e o último exclui dias não encerrados do mapa de pares completos. (`app/ponto/services/cartorio.py:850`, `app/ponto/services/cartorio.py:969`, `app/ponto/services/cartorio.py:1117`, `app/ponto/management/commands/supra_juiz.py:108`, `app/ponto/management/commands/supra_juiz.py:215`, `app/ponto/management/commands/emitir_furo_retroativo.py:62`, `app/ponto/management/commands/emitir_furo_retroativo.py:108`, `app/ponto/turnos.py:2143`.)

**Queries: contagem estática, não medição de execução [nao verificado].**

Defina `T = 3 + C + E + M + U` para `turnos_do_colab`, contando:

- Base 3: batidas, vínculos candidatos e vínculo usado pela ata. (`app/ponto/turnos.py:1858`, `app/ponto/turnos.py:1883`, `app/ponto/turnos.py:1892`, `app/ponto/turnos.py:1911`, `app/ponto/turnos.py:1591`, `app/ponto/turnos.py:1556`.)
- `C=1` sem células fornecidas; zero com alimentação. (`app/ponto/turnos.py:1880`.)
- `E=1` se geradoras faltantes exigirem a segunda busca dos vínculos. (`app/escala/alimentacao.py:123`.)
- `M=1` se algum segmento usar o mapa de DNA, buscado e memoizado para a janela; segmentos cujo tipo tem segundo intervalo completo pulam esse mapa. (`app/ponto/turnos.py:1944`, `app/ponto/turnos.py:1966`, `app/escala/utils.py:100`.)
- `U=1` se o pareador invocar o callback de última batida; ele é memoizado por chamada. (`app/ponto/turnos.py:1903`, `app/ponto/turnos.py:1973`.)

Para uma chamada unitária: `Q = T + V + F`. `V=1` para `escala_vigente` com célula e geradora, ou `2` quando precisa buscar vínculos; `F=1` se alcançar `fim_turno` com tipo, porque `EC.marcos_do_dia` volta a consultar a célula; caso contrário `F=0`. O mapa recebido por `dia_encerrado` **não** é repassado nem a `escala_vigente` nem a `fim_turno`. (`app/ponto/turnos.py:2079`, `app/escala/servico_jornada.py:46`, `app/escala/servico_jornada.py:55`, `app/ponto/turnos.py:2102`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1097`.)

Exemplos estáticos: com célula geradora, sem alimentação, sem segunda busca/callback, usando mapa DNA e alcançando fim previsto: **7 queries**; com aberta viva: **6**; sem célula, mas com vínculo/tipo e alcançando fim: **8**. Alimentar corretamente o mapa reduz somente `C`, uma query. (Derivação das buscas citadas acima; execução não medida [nao verificado].)

No período, `Q = T + L + 1 + K`: `L=1` se não recebeu células; `1` para todos os vínculos; `K` é o número de dias que chegam a `fim_turno` com tipo. O lote evita recarregar turnos/vínculos por dia, mas ainda pode pagar **uma consulta de célula por dia** no julgamento. Sem alimentação, o caso comum é `7 + K`; há duas cargas de célula porque a linha 2123 cria `cels`, enquanto 2126 passa o argumento original `celulas`, possivelmente `None`, ao carregador de turnos. (`app/ponto/turnos.py:2123`, `app/ponto/turnos.py:2124`, `app/ponto/turnos.py:2126`, `app/ponto/turnos.py:2131`, `app/ponto/turnos.py:2102`, `app/escala/models.py:1097`.)

**Ressalva estrutural de alimentação.** `turnos_do_colab` espera chaves `date` e monta `{(cid, d): cel}`; o lote e os chamadores cartório/vigia usam mapas com chave `(cid, date)`. Isso produz chaves aninhadas quando repassado e pode impedir o reconhecimento da geradora na geometria; efeito em dados reais não medido [nao verificado]. (`app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1928`, `app/ponto/turnos.py:2123`, `app/ponto/turnos.py:2126`, `app/ponto/services/cartorio.py:853`, `app/ponto/services/vigia_de_hora.py:57`.)

## 30/10 13:27 · sessao 168a5d3a8d7b · - · - · origem=Ronald (tmux)

Censo fora de `calendario.py`, somente leitura. As comparações abaixo são código executável; comentários históricos sobre regras já removidas foram separados. (`app/api/views.py:1629`; `app/ponto/supra_juiz.py:254`)

**Leitor do calendário em vigia/cron**

- `chamados.services.vigia_fio.censo`: importa `resolver_dia_oferecido_na_data` e, quando o calendário oferece “Resolver dia”, conta `decisao_do_dia_pendente` e pula a classificação como fio morto. Tem relógio próprio: `hoje = hoje or timezone.localdate()`; conta `data > hoje` como futuro no universo e retira `data >= hoje` do contador alarmado antes de perguntar ao calendário. (`app/chamados/services/vigia_fio.py:181`, `:238`, `:278`, `:289`, `:294`, `:298`)
- O comando desse vigia participa da corrente automática como `ENC('vigiar_fio_morto', ... args='--alarme --alarme-passivo')`. (`app/config/crons.py:1654`)
- Não encontrei outros consumidores executáveis de `status_do_dia` ou `contar_por_status` fora do próprio módulo: busca retornou definições, docstrings e registro em `core/juizes.py`. Exaustividade por análise estática: [nao verificado].

**Outros classificadores/decisores por DATA**

| Sítio | Regra e consequência |
|---|---|
| API do app, preenchimento de dias sem batidas | `_futuro = _d > _hoje`; HOJE em trabalho usa `fim_turno`: ausência de fim ou `agora < fim` mantém futuro. Trabalho passado/hoje após fim gera `falta`, exceto isento ou cobertura; futuro gera `previsto`. (`app/api/views.py:1627`, `:1629`, `:1632`, `:1635`, `:1636`, `:1643`, `:1649`, `:1651`) |
| `ponto.services.espelho._status_dia` | Depois de ausência, inconsistência, batidas, isenção e folga: previsto com `data < hoje` vira `sem_registro`; demais viram `vazio`. (`app/ponto/services/espelho.py:156`, `:169`, `:171`) |
| `escala.utils.clipar_grade_janela` | `_dia > hoje` marca `futuro`, zera órfãs e substitui células por `None`, apagando indicação de marco faltante; HOJE passa. (`app/escala/utils.py:934`, `:942`) |
| `cartorio.julgar_celula` | `hoje` próprio via `localdate()`, futuro retorna `None`; HOJE de trabalho, sem `teto_de_hoje`, espera `fim_turno`, comparando `_tz.now()` diretamente — **essa guarda não usa o argumento `agora`**. Folga (`trabalha is False`) pula a espera. (`app/ponto/services/cartorio.py:1531`, `:1536`, `:1545`, `:1553`, `:1557`, `:1560`) |
| `cartorio.julgar_colab` | Com `teto_de_hoje`, distingue HOJE e ONTEM para julgar só marcos vencidos; passado com turno vivo recebe `_em_curso = _pend is None and data < hoje and not _turno_encerrado(data)`. (`app/ponto/services/cartorio.py:1048`, `:1053`, `:1082`) |
| `chamados.services.fabrica_da_celula.motivos_a_perguntar` | `cel.data > hoje` não pergunta. Com marcos, usa vencimento por instante; sem marcos retorna `None` para passado e conjunto vazio para hoje/futuro. (`app/chamados/services/fabrica_da_celula.py:28`, `:30`, `:68`, `:80`, `:85`) |
| `chamados.services.cobrar_dia.cobrar_dia` | Recusa cobrança manual se `data >= timezone.localdate()`, dizendo que turno ainda não terminou; não consulta encerramento efetivo nessa guarda. (`app/chamados/services/cobrar_dia.py:16`, `:26`) |
| `chamados.services.pergunta_no_app.retencao` | Para chamado vivo de furo sem pergunta, `dia >= hoje` retorna `None`: “ainda não é hora”, em vez de causa de retenção. (`app/chamados/services/pergunta_no_app.py:56`, `:64`) |
| `chamados.reconciliador.veredito_orfa` | Depois de verificar fato e pergunta viva, `data_evento >= localtime(agora).date()` retorna `TETO`; só depois chama `dia_encerrado_motivo`. (`app/chamados/reconciliador.py:1028`, `:1032`, `:1036`, `:1038`) |
| `ponto.supra_juiz` | Ramo geral de furo usa `turno_encerrado`; regra por data sobrevive somente na acusação de cadastro para célula de trabalho sem vínculo e sem previsto: `not (hoje and data and data >= hoje)`. (`app/ponto/supra_juiz.py:253`, `:254`, `:280`, `:405`) |
| `escala.services.furos_diarios.apurar_colab` | Corta período em `min(fim, hoje - 1)` e lê ata/célula para contar marcos previstos e furados; exclui HOJE inteiro. Mesma borda reaparece na manutenção e no universo. (`app/escala/services/furos_diarios.py:24`, `:26`, `:35`, `:44`, `:99`, `:282`) |
| Cartão `ate_ontem` | `hoje or timezone.localdate()`, devolve `min(data_fim, hoje - 1)`; PDF aplica corte usando a data local do mesmo `agora` do ato. (`app/relatorios/cartao_pela_celula.py:37`, `:40`, `:41`; `app/relatorios/pdf_espelho.py:285`, `:287`) |
| `dia_decidido.datas_em_aberto_de` | Derivador puro limita **furo apurado** a `d <= ate`; turnos abertos e saídas órfãs entram sem esse teto. O escritor fornece `ate_ontem(data_fim_pedido)` e retira o turno vivo. (`app/ponto/services/dia_decidido.py:311`, `:314`, `:315`, `:318`; `app/relatorios/cartao_pela_celula.py:395`, `:397`, `:412`, `:418`) |
| Validação de cartão/PDF | `data_fim > ontem` recusa: mensagem específica para HOJE (“ainda está sendo batido”) e outra para futuro; `hoje` default usa `date.today()`. (`app/relatorios/services.py:35`, `:38`, `:41`, `:45`) |
| `folha.previa` | Exclui `d >= hoje` antes de propor preenchimento dos marcos apagados da ata. (`app/folha/previa.py:41`, `:77`, `:79`, `:84`) |
| `folha.export.motivos_retencao_celula` | Célula inexistente ou ainda sem veredito só causa retenção `sem_celula` com `d < hoje` (exceto dia da demissão). Célula já acusando pode reter independentemente desse teste. (`app/folha/export.py:1179`, `:1211`, `:1213`, `:1215`, `:1237`) |
| Regularização de retorno | Cria faltas intermediárias só antes de HOJE; `dia >= hoje` interrompe laço. (`app/ponto/services/regularizacao.py:74`, `:108`, `:113`, `:118`) |
| Abandono provisório | Cria falta de D+1 até ontem, com guarda `dia >= hoje: break`; comentário expressamente diz “não inclui hoje, ele pode aparecer”. (`app/ponto/management/commands/marcar_abandono_provisorio.py:69`, `:72`, `:76`) |
| Declaração manual/chat | Só recusa `tipo == 'falta' and data > hoje`; HOJE é permitido por essa guarda. (`app/ponto/services/declarar_dia.py:94`, `:100`) |

**Vigias, crons e comandos com janela civil própria**

- `reconciliar_grade`: pula `dt >= hoje`, depois verifica também vencimento de cada marco por instante, inclusive noturno. Está agendado às 06:20 sem `--apply`. (`app/ponto/management/commands/reconciliar_grade.py:134`, `:169`; `app/config/crons.py:443`)
- `processar_cartorio`: lê `localdate()` e `now()` separadamente, varre apenas células `data__lt=hoje`, entrega `hoje/agora` ao serviço; agendado às 06:27. Isso é janela de varredura além da regra do serviço. (`app/ponto/management/commands/processar_cartorio.py:38`, `:39`, `:50`, `:62`, `:65`; `app/config/crons.py:597`)
- `ponto.services.furos_sem_cobranca.medir`: só considera `veredito='furo'` até ontem (`data__lte=ontem`). (`app/ponto/services/furos_sem_cobranca.py:53`, `:54`, `:61`)
- `ponto.services.vigia_entrada.por_dia_da_semana`: só células de trabalho `data__lt=hoje`; contador termina ontem. (`app/ponto/services/vigia_entrada.py:73`, `:79`, `:86`)
- `folha.services.sem_furo`: consulta dias com furo até `min(fim, ontem)`. (`app/folha/services/sem_furo.py:39`, `:40`, `:51`)
- `emitir_turno_aberto_retroativo`: se fim pedido `>= hoje`, recua para ontem por considerar HOJE não encerrado. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:72`, `:73`)
- `retratar_chamados_lei_cala`: compara contra `date.today()`, pula `D >= hoje`, só então pergunta `classificar_falta` e pode retratar. (`app/chamados/management/commands/retratar_chamados_lei_cala.py:32`, `:43`, `:45`, `:59`)
- `mapa_divergencia`: separa `data > hoje` como futuro e só examina divergências de chaves/marcos se não futuro; HOJE fica no balde passado. (`app/ponto/management/commands/mapa_divergencia.py:162`, `:164`)

**Regras por INSTANTE que também importam para “ainda pode bater”**

- `faltas_de_hoje_sem_cobranca`: relógio é `localtime(agora or now())`; fixa HOJE e só conta ausência de batida após a entrada prevista vencer por `horas` (padrão constante do módulo), com exclusões de isento, adesão, batida, cobertura e fato vencedor. Não lê o status do calendário. (`app/ponto/services/faltas_de_hoje.py:13`, `:28`, `:29`, `:48`, `:49`, `:55`, `:57`, `:59`, `:61`, `:68`)
- `vigia_de_hora`: consulta células de hoje/ontem, e para ontem exige último marco + teto a partir da meia-noite atual; conserva candidato até dois tetos após encerramento respondido por `dia_encerrado(agora - 2*teto)`. Julga com `teto_de_hoje=True`. (`app/ponto/services/vigia_de_hora.py:23`, `:46`, `:51`, `:52`, `:54`, `:57`, `:94`, `:130`)
- **Não confundir comentário antigo com código atual:** `flip_auto` antes tinha `dia >= hoje`; agora chama `dia_encerrado`. (`app/ponto/services/flip_auto.py:279`, `:285`)
- **Regra adjacente, sem classificar falta/furo:** declaração retrospectiva de cobertura recusa `dia >= hoje` e manda usar designação de cobertura. (`app/ponto/services/cobertura.py:271`, `:272`)

## 30/10 13:27 · sessao c707ea85e5a1 · - · - · origem=Ronald (tmux)

Censo de leitores, somente leitura. A chave `'futuro'` de `calendario.py:785` é fato interno de `_classificar`; não é exportada nos dicionários de `dias` (estes exportam `status`, `status_rotulo`, `eh_trabalho` etc.). **Não confundir com outras chaves `futuro` de outras grades.** — `app/colaboradores/services/calendario.py:779`, `app/colaboradores/services/calendario.py:785`, `app/colaboradores/services/calendario.py:855`, `app/colaboradores/services/calendario.py:889`.

| Leitor | Caminho e decisão/exibição | Regra temporal própria |
|---|---|---|
| Perfil do colaborador na tela de gestão | `detalhe.html` carrega endpoint HTMX; view chama `contexto_calendario` e renderiza partial; partial inclui grade. Status define fundo/borda e ícone/title: `ok` verde/check, `irregular` amarelo/aviso, `folga_trabalhada` laranja, `falta` vermelho/ícone falta, `escala_suspeita` roxo pontilhado/`?`, `aberto` amarelo/cadeado aberto, `previsto` azul/ícone previsto, `ausencia` roxo, `folga` cinza. Ausência/palavra podem tomar precedência sobre ícone. — `app/templates/colaboradores/detalhe.html:207`, `app/templates/colaboradores/detalhe.html:215`, `app/colaboradores/views.py:1945`, `app/colaboradores/views.py:1962`, `app/templates/colaboradores/calendario_partial.html:62`, `app/templates/colaboradores/partials/_calendario_grade.html:69`, `app/templates/colaboradores/partials/_calendario_grade.html:97`, `app/templates/colaboradores/partials/_calendario_grade.html:107` | View põe outro `hoje=localdate()` no contexto quando usuário pode regularizar, mas grade atual só lê `dia.resolver_dia_oferecido` para botões, sem comparar data com hoje. — `app/colaboradores/views.py:1957`, `app/colaboradores/views.py:1960`, `app/templates/colaboradores/partials/_calendario_grade.html:125` |
| Gaveta de diagnóstico / painel | Chama `contexto_calendario` em período custom e inclui a mesma grade em fita, com mesmos status, totais e botões. — `app/colaboradores/services/drawer.py:120`, `app/colaboradores/services/drawer.py:121`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:33`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` | `localdate()` define janela atual + anterior, limitada pelo piso visual; isso escolhe período, sem reclassificar status. — `app/colaboradores/services/drawer.py:111`, `app/colaboradores/services/drawer.py:113`, `app/colaboradores/services/drawer.py:118` |
| Auditoria de espelhos / mapa de furos HTML | `dataset_furos` chama calendário por colaborador e template inclui a mesma grade. **Seleção de cards, `nfuro`, ordenação e lista de marcos faltantes não usam status calendário:** vêm de `grade_da_celula` → `estado_do_dia`. — `app/relatorios/furos.py:141`, `app/relatorios/furos.py:160`, `app/relatorios/furos.py:164`, `app/relatorios/furos.py:165`, `app/relatorios/furos.py:184`, `app/relatorios/furos.py:190`, `app/templates/relatorios/furos.html:42`, `app/templates/relatorios/furos.html:43` | View lê `localdate`; período padrão termina hoje, mas período explícito não recebe teto em hoje nessa view. — `app/relatorios/views.py:1518`, `app/relatorios/views.py:1531`, `app/relatorios/views.py:1537`, `app/relatorios/views.py:1541` |
| PDF do mapa de furos | View extrai **status do calendário** para `card['semanas']`; gerador pinta dia conforme esse estado. Mapas têm `ok`, `irregular`, `folga_trabalhada`, `falta`, `aberto`, `previsto`, `folga`, `sem_dados`, `sem_marcos`; outros caem no fundo branco/texto cinza. `nfuro` continua sendo outra contagem (acima). — `app/relatorios/views.py:1548`, `app/relatorios/views.py:1550`, `app/relatorios/views.py:1551`, `app/relatorios/views.py:1562`, `app/relatorios/furos_pdf.py:11`, `app/relatorios/furos_pdf.py:21`, `app/relatorios/furos_pdf.py:47`, `app/relatorios/furos_pdf.py:56`, `app/relatorios/furos_pdf.py:66` | Herda período da view; gerador não compara data com relógio. — `app/relatorios/views.py:1562`, `app/relatorios/furos_pdf.py:25`, `app/relatorios/furos_pdf.py:47` |
| Botão Resolver dia e Pauta DP | Calendário usa status para derivar `resolver_dia_oferecido`: `pode_ultimo_recurso && eh_trabalho && data < hoje && status in STATUS_QUE_PEDEM_DECISAO`. Template acrescenta permissão `pode_veredito`, oferecendo checkbox, Resolver dia e botão DP. — `app/colaboradores/services/calendario.py:191`, `app/colaboradores/services/calendario.py:196`, `app/colaboradores/services/calendario.py:197`, `app/colaboradores/services/calendario.py:904`, `app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/templates/colaboradores/partials/_calendario_grade.html:126` | Exige passado estrito `data < hoje` no serviço. — `app/colaboradores/services/calendario.py:196` |
| Vigia do fio morto | Censo chama `resolver_dia_oferecido_na_data`, que monta calendário de um dia e lê carimbo; true entra em `decisao_do_dia_pendente` e deixa de seguir para fio morto. — `app/chamados/services/vigia_fio.py:238`, `app/chamados/services/vigia_fio.py:294`, `app/chamados/services/vigia_fio.py:298`, `app/colaboradores/services/calendario.py:205`, `app/colaboradores/services/calendario.py:208` | Antes dessa chamada, dias `>= hoje` sem fio vivo vão ao contador `fio_morto_de_hoje_pra_frente`; `> hoje` também incrementa tripwire `dias_futuros_no_universo`. Command captura `agora=now`, deriva `hoje=localdate(agora)` e passa ao censo; calendário da porta curta captura seu próprio hoje (não recebe o do vigia). — `app/chamados/services/vigia_fio.py:278`, `app/chamados/services/vigia_fio.py:289`, `app/chamados/services/vigia_fio.py:292`, `app/chamados/management/commands/vigiar_fio_morto.py:158`, `app/chamados/management/commands/vigiar_fio_morto.py:160`, `app/colaboradores/services/calendario.py:205` |
| Contadores do calendário | `resumo` conta literalmente status para `ok`, `faltas`, `escala_suspeita`, `folgas_trabalhadas`, `irregulares`, `folgas`, `ausencias`, `abertos`, `previstos`; HE é selo separado. Template mostra `faltas` como “Sem registro”, além de Ok, HE, Trab. folga, Irreg., escala?, Folgas e Abertos; não há bloco exibindo `previstos` ou `ausencias` nesse rodapé. — `app/colaboradores/services/calendario.py:908`, `app/colaboradores/services/calendario.py:925`, `app/templates/colaboradores/partials/_calendario_grade.html:141`, `app/templates/colaboradores/partials/_calendario_grade.html:145`, `app/templates/colaboradores/partials/_calendario_grade.html:173` | Sem relógio próprio no rodapé. — `app/templates/colaboradores/partials/_calendario_grade.html:137` |
| Portas `status_do_dia` e `contar_por_status` | Primeira monta um dia e devolve status ou `sem_dados`; segunda monta período e `Counter(status)`, invertendo datas se necessário. — `app/colaboradores/services/calendario.py:211`, `app/colaboradores/services/calendario.py:217`, `app/colaboradores/services/calendario.py:220`, `app/colaboradores/services/calendario.py:223`, `app/colaboradores/services/calendario.py:226`, `app/colaboradores/services/calendario.py:231` | Nenhum chamador de produção localizado por busca de referências; ausência absoluta/dinâmica [nao verificado]. |

**App/API e cartão PDF não são leitores desse classificador.** A API `api_espelho_v2` usa `espelho_do_colab`, `grade_da_celula` e classificação própria; cartão PDF também lê `espelho_do_colab`. — `app/api/views.py:1363`, `app/api/views.py:1369`, `app/api/views.py:1500`, `app/api/views.py:1570`, `app/api/views.py:1627`, `app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:598`.

- API do app: captura `agora=localtime(now())`; monta mês civil; para dia com batidas escolhe `aberto` se turno aberto, senão `alerta` se inconsistente, senão `ok`. Para dia sem batidas usa `_futuro = data > hoje`; **hoje de trabalho é considerado futuro até `fim_turno`** (`fim is None or agora < fim`), depois pode virar falta. Trabalho futuro recebe `previsto`; ausência/isento/vínculo/folga modulam a resposta. Payload inclui `status` e `falta`; vocabulário é diferente do calendário. — `app/api/views.py:1350`, `app/api/views.py:1355`, `app/api/views.py:1356`, `app/api/views.py:1570`, `app/api/views.py:1597`, `app/api/views.py:1627`, `app/api/views.py:1629`, `app/api/views.py:1632`, `app/api/views.py:1635`, `app/api/views.py:1643`, `app/api/views.py:1656`, `app/api/views.py:1673`, `app/api/views.py:1674`.
- API retorna `dias` em JSON; rota `/api/espelho/v2/`. A exibição efetiva no cliente móvel externo não está verificada neste snapshot. — `app/api/views.py:1700`, `app/api/views.py:1729`, `app/api/urls.py:81`; cliente [nao verificado].
- Cartão PDF: gera `agora=now()` uma vez por ato, corta fim por `max(data_ini, ate_ontem(fim, hoje=localtime(agora).date()))`, lê dias do espelho e recorta janela. `ate_ontem` devolve `min(fim, hoje-1)`. Ressalva real: o `max(data_ini, ...)` impede afirmar genericamente “nenhum hoje/futuro pode entrar” para janela integralmente futura. — `app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:593`, `app/relatorios/pdf_espelho.py:594`, `app/relatorios/pdf_espelho.py:598`, `app/relatorios/pdf_espelho.py:1041`, `app/relatorios/pdf_espelho.py:1088`, `app/relatorios/cartao_pela_celula.py:37`, `app/relatorios/cartao_pela_celula.py:41`.
- Palavra “Em aberto” compartilhada por espelho/cartão/app vem de outra derivação: `folha_manda` usa seu próprio `ate_ontem(fim)` e passa ao `datas_em_aberto_de`; não deriva de `status=='falta'` do calendário. — `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:418`, `app/relatorios/cartao_pela_celula.py:421`.
- Outra chave `futuro`: `escala/utils.py::clipar_grade_janela` marca dias `> hoje` e neutraliza células/órfãs; `ponto/services/espelho.py:688` ignora grade marcada. Não é a chave interna do `_classificar`. — `app/escala/utils.py:934`, `app/escala/utils.py:942`, `app/escala/utils.py:943`, `app/escala/utils.py:945`, `app/ponto/services/espelho.py:688`.
- Calendário de fase lê `c.futuro` para cinzar números e `c.hoje` para borda; é outro componente/contrato (cal.semanas), não os `dias/status` de `contexto_calendario`. — `app/templates/colaboradores/partials/_calendario_fase.html:5`, `app/templates/colaboradores/partials/_calendario_fase.html:12`, `app/templates/colaboradores/partials/_calendario_fase.html:14`.

O command do vigia imprime linhas do censo, carimba todos os contadores e só alarma de acordo com constantes; `decisao_do_dia_pendente` é informativo, enquanto `dias_futuros_no_universo` alarma sempre e fio morto só com flag de passivo. Agendamento real em crontab não foi verificado neste snapshot. — `app/chamados/management/commands/vigiar_fio_morto.py:80`, `app/chamados/management/commands/vigiar_fio_morto.py:81`, `app/chamados/management/commands/vigiar_fio_morto.py:160`, `app/chamados/management/commands/vigiar_fio_morto.py:165`, `app/chamados/management/commands/vigiar_fio_morto.py:205`; crontab [nao verificado].

## 30/10 13:21 · sessao abaa92c92829 · COL309-SETEMBRO · sonda · origem=Code

Não houve nova execução: o lançador escreve arquivos ([bin/sonda_leitura.sh:132](bin/sonda_leitura.sh:132), [bin/sonda_leitura.sh:217](bin/sonda_leitura.sh:217)). Abaixo, a saída fornecida integralmente; execução e dados de produção [nao verificado].

```text
sonda_leitura: sonda_classe.py [1401c8b1fb95d59a] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

TRILHA_LINHAS_DESDE_28_09=610 COM_BARRADOS=29 DIAS_BARRADOS=563 VINCULOS=25
BARR log=595390 ec=1327 dias=1 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-20..2026-10-20 apesar=False
BARR log=595957 ec=1328 dias=31 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False
BARR log=596310 ec=1330 dias=4 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-17..2026-10-20 apesar=False
BARR log=597279 ec=1331 dias=19 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-02..2026-10-20 apesar=False
BARR log=597355 ec=1333 dias=30 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-22..2026-10-20 apesar=False
BARR log=597406 ec=1334 dias=30 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-22..2026-10-20 apesar=False
BARR log=597429 ec=1335 dias=8 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-13..2026-10-20 apesar=False
BARR log=597451 ec=1336 dias=14 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-07..2026-10-20 apesar=False
BARR log=597484 ec=1337 dias=10 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-11..2026-10-20 apesar=False
BARR log=597680 ec=1333 dias=30 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-22..2026-10-20 apesar=False
BARR log=597757 ec=1339 dias=22 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-30..2026-10-20 apesar=False
BARR log=601349 ec=1255 dias=10 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-11..2026-10-20 apesar=False
BARR log=602270 ec=1340 dias=5 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-16..2026-10-20 apesar=False
BARR log=602379 ec=1341 dias=20 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-01..2026-10-20 apesar=False
BARR log=603110 ec=1220 dias=19 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-02..2026-10-20 apesar=False
BARR log=613403 ec=1328 dias=31 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False
BARR log=640685 ec=1365 dias=1 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-20..2026-10-20 apesar=False
BARR log=653981 ec=1380 dias=31 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False
BARR log=660581 ec=1397 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=660739 ec=1263 dias=1 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-09-20..2026-10-20 apesar=False [nao verificado]
BARR log=662019 ec=1405 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=668800 ec=1407 dias=4 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-09-17..2026-10-20 apesar=False [nao verificado]
BARR log=668822 ec=1408 dias=5 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-09-16..2026-10-20 apesar=False [nao verificado]
BARR log=668849 ec=1409 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=692598 ec=1416 dias=20 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-09-01..2026-10-20 apesar=False [nao verificado]
BARR log=698178 ec=1420 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=699261 ec=1420 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=724164 ec=1437 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=724205 ec=1437 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
FORA_DA_VIGENCIA|emp=1|comp=2026-10|aberta celulas=21 colabs=1
FORA_DA_VIGENCIA|emp=2|comp=2026-08|EXPORTADA celulas=643 colabs=52
FORA_DA_VIGENCIA|emp=2|comp=2026-09|EXPORTADA celulas=1036 colabs=53
FORA_DA_VIGENCIA|emp=2|comp=2026-10|aberta celulas=1272 colabs=56
FORA_DA_VIGENCIA|emp=2|comp=2026-11|aberta celulas=894 colabs=31
FORA_DA_VIGENCIA|emp=2|comp=2026-12|aberta celulas=110 colabs=6
FORA_DA_VIGENCIA|emp=2|comp=2027-01|aberta celulas=33 colabs=3
FORA_DA_VIGENCIA|emp=3|comp=2026-08|aberta celulas=161 colabs=11
FORA_DA_VIGENCIA|emp=3|comp=2026-09|EXPORTADA celulas=216 colabs=12
FORA_DA_VIGENCIA|emp=3|comp=2026-10|aberta celulas=206 colabs=10
FORA_DA_VIGENCIA|emp=3|comp=2026-11|aberta celulas=475 colabs=18
FORA_DA_VIGENCIA|emp=3|comp=2026-12|aberta celulas=110 colabs=4
FORA_DA_VIGENCIA|emp=3|comp=2027-01|aberta celulas=44 colabs=4
FORA_DA_VIGENCIA|emp=4|comp=2026-08|aberta celulas=11 colabs=2
FORA_DA_VIGENCIA_TOTAL=5232 COLABS=173
COL309_FORA=27
ERROS=0
FIM-DA-SONDA
```

Trilha: 563 dias em 29 registros, abrangendo 25 vínculos, conforme dado fornecido [nao verificado].  
Fora da vigência da geradora: 5.232 células = 1.895 em competências exportadas + 3.337 em abertas, somadas do dado fornecido [nao verificado].  
COL309_FORA=27, conforme dado fornecido [nao verificado].  
ERROS=0, conforme dado fornecido [nao verificado].

_491 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
