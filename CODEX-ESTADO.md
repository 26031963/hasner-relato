# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 18:18** · publicado em **12/11/2026 18:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 18:18 · sessao 0d22f717f4c0 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · verificador · origem=Code

**P1 foi corrigido; P2 continua PROCEDENTE.** A trilha tornou-se obrigatória nos dois ramos, mas o relógio ainda é lido no interior e em vários instantes do mesmo Salvar. (`app/ponto/portas/celula.py:1145`, `app/ponto/portas/celula.py:1163`; `app/colaboradores/services/vinculo.py:646`, `app/colaboradores/services/vinculo.py:314`; `app/docs/PLANO-PADROES.md:92`)

1. **PROCEDENTE 1 — trilha e atomicidade**

- **Sim, ambos levantam se `evento()` devolver `None`:** conservado em `app/ponto/portas/celula.py:1151`; desfeito em `app/ponto/portas/celula.py:1163`. O retorno `None` após falha engolida está em `app/core/observ.py:105` e `app/core/observ.py:139`.
- **DNA, limpeza e evento desfazem juntos no banco nos três pontos de chamada direta:** chamadas `:262` e `:309` sob o `atomic` iniciado em `:220`; chamada `:834` sob o `atomic` iniciado em `:818`. A limpeza possui ainda seu próprio `atomic`, incluindo `salvar_e_emitir` e a trilha obrigatória. (`app/ponto/portas/celula.py:220`, `app/ponto/portas/celula.py:262`, `app/ponto/portas/celula.py:309`, `app/ponto/portas/celula.py:818`, `app/ponto/portas/celula.py:834`, `app/ponto/portas/celula.py:1145`; `app/ponto/portas/celula.py:953`)
- **Ressalva:** o objeto Python já recebeu os `None`; o código não restaura seus atributos quando o banco faz rollback. (`app/ponto/portas/celula.py:1156`)
- **Existe ancestral que captura e segue:** `censo_fase_12x36` captura `Exception`, imprime `RECUSADO` e continua o lote. A captura ocorre depois da saída de `executar_vinculo`, que é `@transaction.atomic`; portanto não confirma aquele ato como sucesso. (`app/ponto/management/commands/censo_fase_12x36.py:421`, `app/ponto/management/commands/censo_fase_12x36.py:427`; `app/colaboradores/services/vinculo.py:169`)
- Os catches históricos de `salvar_tipo` e dos receivers de vínculo cobrem a parte até hoje; o suplemento futuro está fora deles. A view encaminha a exceção para código que relança `RuntimeError`; o chamador do snapshot captura apenas `ValueError`. (`app/escala/services/cadastro_tipo.py:323`, `app/escala/services/cadastro_tipo.py:328`; `app/escala/signals.py:73`, `app/escala/signals.py:81`, `app/escala/signals.py:125`, `app/escala/signals.py:132`; `app/colaboradores/views.py:438`; `app/core/erros.py:75`; `app/chamados/services/acoes_chamado.py:148`)

**O teste de banco tem mordida por construção:** exige `RuntimeError`, faz `refresh_from_db()` e confere ata, julgamento e veredito anteriores. Um mutante que ignore novamente o retorno falha já no `assertRaises`; o ramo conservado também exige a exceção. **Execução efetiva desse teste no banco: [nao verificado].** (`app/ponto/tests/test_cadastro_futuro_c5d_banco.py:38`, `app/ponto/tests/test_cadastro_futuro_c5d_banco.py:43`, `app/ponto/tests/test_cadastro_futuro_c5d_banco.py:56`)

2. **PROCEDENTE 2 — censo do relógio e chamadores**

O serviço `cadastro_futuro` ficou sem relógio, com `hoje` obrigatório e recusa explícita de `None`; isso corrige aquele arquivo, mas não o caminho inteiro. (`app/ponto/services/cadastro_futuro.py:9`, `app/ponto/services/cadastro_futuro.py:24`, `app/ponto/services/cadastro_futuro.py:49`, `app/ponto/services/cadastro_futuro.py:55`)

Censo do caminho de cadastro/materialização, incluindo geração, reversão e caudas imediatas; cada posição abaixo identifica a leitura na cópia:

| Arquivo:função | Leitura(s) | Classe |
|---|---|---|
| `app/core/signals.py::_feriado_regenerar_futuras` | `localdate`, `:36` | BORDA, receiver `:28` |
| `app/core/signals.py::_feriado_excluido_regenerar_futuras` | `localdate`, `:43` | BORDA, receiver `:39` |
| `app/escala/signals.py::_ec_vigencia_regenera_celulas` | `localdate`, `:67`, `:68` | BORDA, receiver `:34`; duas leituras |
| `app/escala/signals.py::_ec_ancora_regenera_celulas` | `localdate`, `:119` | BORDA, receiver `:86` |
| `app/escala/admin.py::TipoEscalaAdmin.save_model` | `localdate`, `:15` | BORDA admin |
| `app/colaboradores/views.py::vincular_folga` | `date.today`, `:242`, `:243` | BORDA view |
| `app/colaboradores/views.py::painel_vinculo` | `date.today`, `:280` | BORDA view |
| `app/colaboradores/views.py::vincular_posto` | `localdate`, `:331` | BORDA view |
| `app/escala/views.py::escala_remove_colab` | `date.today`, `:269` | BORDA view |
| `app/escala/views.py::aplicar_folga_calendario` | `date.today`, `:393` | BORDA view |
| `app/escala/views_plano_folgas.py::plano_folgas` | `date.today`, `:31` | BORDA view |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py::Command._rodar / _aplicar` | `localdate`, `:67`, `:227` | BORDA command |
| `app/ponto/management/commands/gerar_celulas.py::Command.handle` | `date.today`, `:339` | BORDA command |
| `app/colaboradores/services/vinculo.py::julgar_vinculo` | `date.today`, `:71` | INTERIOR |
| `app/escala/models.py::EscalaColaborador.recusa_ancora_futura` | `localdate`, `:965` | INTERIOR |
| `app/colaboradores/services/vinculo.py::_copiar_folga_padrao` | `date.today`, `:163` | INTERIOR |
| `app/colaboradores/services/vinculo.py::executar_vinculo` | `localdate`, `:314` | INTERIOR |
| `app/colaboradores/services/vinculo.py::trocar_calendario_de_folga` | `localdate`, `:460` | INTERIOR |
| `app/colaboradores/services/vinculo.py::fechar_vigencia` | `localdate`, `:646` | INTERIOR |
| `app/colaboradores/services/vinculo.py::reabrir_vigencia_impossivel` | `date.today`, `:728`; `localdate`, `:740` | INTERIOR |
| `app/colaboradores/services/vinculo.py::marcar_trabalha_em_feriado` | `localdate`, `:885` | INTERIOR |
| `app/colaboradores/services/vinculo.py::reverter_para_snapshot` | `localdate`, `:1021` | INTERIOR |
| `app/colaboradores/services/estrutura.py::_evento_do_municipio` | `localdate`, `:62` | INTERIOR |
| `app/escala/services/cadastro_tipo.py::salvar_tipo` | `localdate`, `:310` | INTERIOR |
| `app/escala/folgas.py::salvar_calendario_de_folga` | `localdate`, `:599` | INTERIOR |
| `app/escala/folgas.py::relavrar_celulas_da_foto` | `localdate`, `:280` | INTERIOR |
| `app/escala/folgas.py::aplicar_folgas_em_lote` | `_tz.localdate`, `:401` | INTERIOR |
| `app/escala/folgas.py::reparentar_folga` | `_tz.localdate`, `:512` | INTERIOR |
| `app/escala/services/escala_auto_executor.py::inicio_do_efeito / aplicar_no_vinculo / desfazer` | `localdate`, `:29`, `:98`, `:127` | INTERIOR |
| `app/ponto/portas/celula.py::regenerar_celulas_vinculo` | `date.today`, `:188`; `now`, `:258`, `:303`; `localdate`, `:329` | INTERIOR |
| `app/ponto/management/commands/gerar_celulas.py::_marcar_cobrancas_orfas` | `now`, `:264` | INTERIOR, apesar do arquivo de command |
| `app/ponto/portas/celula.py::_o_chao_do_dia_mudou` | `localdate`, `:459` | INTERIOR |
| `app/colaboradores/services/pos_vinculo.py::reconciliar_apos_vinculo` | `_tz.localdate`, `:56` | INTERIOR |
| `app/ponto/portas/celula.py::reverter_regeneracao_dia` | `now`, `:826`; `localdate`, `:831` | INTERIOR |
| `app/ponto/portas/celula.py::gerar_celulas_periodo` | `date.today`, `:860` | INTERIOR |

Na cauda de julgamento/apuração também permanecem `cartorio.julgar_celula`: `localdate` em `:2016`, `now` em `:2037` e `:2040`; `lavrar_veredito`: `now` em `:1086`; e `gravar_furos`: `now` em `:1252`, todos INTERIOR. Os `now` de regeneração/furo são carimbos; os `localdate` citados decidem alcance ou elegibilidade temporal. (`app/ponto/services/cartorio.py:2016`, `app/ponto/services/cartorio.py:2037`, `app/ponto/services/cartorio.py:2040`; `app/ponto/portas/celula.py:1086`, `app/ponto/portas/celula.py:1252`, `app/ponto/portas/celula.py:333`, `app/ponto/portas/celula.py:464`)

**Restaram defaults/fallbacks no caminho:**

| Função | Prova |
|---|---|
| `regenerar_celulas_vinculo` | `hoje=None`: `app/ponto/portas/celula.py:113`; `hoje or date.today()`: `:188` |
| `regenerar_celulas_dia` | `hoje=None`: `app/ponto/portas/celula.py:736` |
| `reverter_regeneracao_dia` | `hoje=None`: `app/ponto/portas/celula.py:773`; `hoje or`: `:831` |
| `gerar_celulas_periodo` | `hoje=None`: `app/ponto/portas/celula.py:855`; `hoje or`: `:860` |
| `aplicar_folgas_em_lote` | `hoje=None`: `app/escala/folgas.py:385`; `hoje or`: `:401` |
| `reparentar_folga` / wrapper do colaborador | `app/escala/folgas.py:471`, `app/escala/folgas.py:512`, `app/escala/folgas.py:529` |
| `recusa_ancora_futura` | `app/escala/models.py:938`, `app/escala/models.py:965` |
| executor automático: início/aplicar/desfazer | `app/escala/services/escala_auto_executor.py:24`, `:29`, `:87`, `:98`, `:118`, `:127` |
| `apurar_colab`, na cauda pós-vínculo | `app/escala/services/furos_diarios.py:25`, `app/escala/services/furos_diarios.py:30` |

**Não identifiquei chamada da cópia que passe a quebrar por omitir `hoje` nas sete assinaturas alteradas.** Os receivers passam explicitamente; os demais chamadores encontrados também. Não apareceu importação com apelido nem `getattr` literal dessas sete funções; resolução dinâmica externa permanece [nao verificado]. (`app/core/signals.py:35`, `app/core/signals.py:43`; `app/escala/signals.py:81`, `app/escala/signals.py:132`; `app/escala/admin.py:15`; `app/escala/folgas.py:298`, `:601`, `:640`; `app/colaboradores/services/estrutura.py:62`; `app/colaboradores/services/vinculo.py:333`, `:460`, `:650`, `:738`, `:898`, `:949`, `:1023`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:243`; `app/escala/services/cadastro_tipo.py:323`, `:328`, `:366`, `:376`, `:388`; `app/ponto/services/cadastro_futuro.py:77`)

Há chamadas sem `hoje` a funções **que continuam opcionais**, portanto ainda não quebram: `julgar_vinculo → recusa_ancora_futura` e o receiver que chama `reparentar_folgas_do_colaborador`. São pendências da classe, não novos `TypeError` deste delta. (`app/colaboradores/services/vinculo.py:133`; `app/escala/signals.py:204`)

3. **As sete “portas do ato” são INTERIOR; meia-noite continua relevante**

| Porta | Chamador que demonstra composição interna |
|---|---|
| `fechar_vigencia` | `executar_vinculo`: `app/colaboradores/services/vinculo.py:208`, `:235` |
| `reabrir_vigencia_impossivel` | command: `app/escala/management/commands/corrigir_vinculo_vigencia.py:84` |
| `reverter_para_snapshot` | serviço: `app/chamados/services/acoes_chamado.py:149` |
| `trocar_calendario_de_folga` | view: `app/colaboradores/views.py:239` |
| `salvar_calendario_de_folga` | view: `app/escala/views.py:356` |
| `_evento_do_municipio` | outras portas: `app/colaboradores/services/estrutura.py:72`, `:97` |
| `salvar_tipo` | inclusive outro serviço: `app/chamados/services/limbo_folgas.py:92` |

**Sim, podem discordar.** O executor chama `fechar_vigencia` duas vezes antes de capturar seu próprio `_hoje`; cada fechamento lê seu relógio. O comentário “UM relógio no ato” não descreve esse caminho. (`app/colaboradores/services/vinculo.py:208`, `:235`, `:314`, `:646`)

Consequência possível: fechamento antes da meia-noite de D trata D+1 como futura e limpa seu julgamento; executor depois da meia-noite captura D+1, inclui o mesmo dia na regeneração até hoje e pode executar o pós-vínculo. Outra passagem permite selecionar uma célula como futura pelo `_hoje` recebido e depois elegê-la para julgamento pela nova leitura da cauda. São decisões temporais diferentes no mesmo Salvar; ocorrência ou dano em produção: [nao verificado]. (`app/ponto/services/cadastro_futuro.py:27`; `app/ponto/portas/celula.py:246`, `:262`, `:459`, `:464`; `app/colaboradores/services/vinculo.py:319`, `:340`)

4. **QUEM ESCREVE / QUEM LÊ**

**`hoje` é dia civil de referência**, usado para competência e futuro estrito. O dia da célula/turno permanece separado em `dia` ou `cel.data`; não identifiquei dois significados nesse parâmetro alterado. `date.today()` usa a data do processo; equivalência efetiva com a data local do Django em produção: [nao verificado]. (`app/ponto/services/cadastro_futuro.py:26`, `:27`, `:43`, `:45`; `app/ponto/portas/celula.py:1137`; `app/ponto/management/commands/gerar_celulas.py:339`; `app/config/settings/base.py:93`)

| Parâmetro/estado | Escritor ou produtor | Leitor/efeito |
|---|---|---|
| `hoje` | bordas e serviços enumerados no item 2 | janela, filtro `> hoje`, classificação da limpeza: `app/ponto/services/cadastro_futuro.py:26`, `:27`, `:60`; `app/escala/folgas.py:623`; `app/ponto/portas/celula.py:1137` |
| `desde`, `datas`, `substitui_dono` | chamador do evento | seleção e proteção do dono: `app/ponto/services/cadastro_futuro.py:27`, `:33`, `:37` |
| `autor/ator`, `motivo` | chamador; request/processo complementa autoria | descrição, usuário e antes/depois da trilha: `app/ponto/portas/celula.py:1147`, `:1159`; `app/core/observ.py:108`, `:118`, `:134` |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata`, `status_dia`, `status_rotulo` | `lavrar_veredito`; D1 limpa pela mesma porta: `app/ponto/portas/celula.py:1082`, `:1103`, `:1156` | grade, precedência admin, stale e status: `app/escala/services/leitor_celula.py:379`, `:424`; `app/ponto/portas/celula.py:970`; `app/chamados/veredito_lavrado.py:191`; `app/ponto/services/cartorio.py:1084`; `app/ponto/status_dia.py:287` |
| `julgada_em`, `impressao`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` | `carimbar` e operação de insumos; D1 limpa: `app/ponto/portas/celula.py:1185`, `:1223`, `:1156` | cache/idempotência, stale e vigia B5: `app/ponto/services/cartorio.py:1260`, `:1409`; `app/chamados/veredito_lavrado.py:191`; `app/ponto/portas/celula.py:1181`; `app/ponto/management/commands/vigia_b5_ausencia.py:71` |
| ações `julgamento_da_futura_conservado/desfeito` | D1 solicita; `evento` grava `LogAuditoria`: `app/ponto/portas/celula.py:1147`, `:1159`; `app/core/observ.py:134` | leitores nominais encontrados nos testes; leitura genérica na auditoria: `app/ponto/tests/test_cadastro_futuro_c5.py:157`, `:204`; `app/ponto/tests/test_cadastro_futuro_c5d.py:78`, `:87`; `app/core/views.py:705` |

O vigia futuro **lê os campos da célula**, não a trilha nominal: `julgada_em`, `ata`, `veredito` e a distinção administrativa por `veredito_via`. (`app/ponto/services/vigia_cadastro_futuro.py:21`, `:45`, `:47`)

A autoridade permanece a indicada em `JUIZES.md`: `vinculo_do_dia` e `vinculo_cobre_a_data`; o suplemento usa o carregador que chama essa juíza, e a janela vem de `janela_atual`. (`app/docs/JUIZES.md:120`; `app/ponto/services/cadastro_futuro.py:38`; `app/escala/alimentacao.py:358`; `app/ponto/management/commands/gerar_celulas.py:299`)

5. **Classes que já reprovaram obra**

| Classe | Resultado nesta volta |
|---|---|
| Segundo juiz | **Ausente no delta examinado:** permanece chamada à autoridade existente. `app/ponto/services/cadastro_futuro.py:38`; `app/escala/alimentacao.py:358` |
| Segundo escritor fora da porta | **Ausente no delta examinado:** limpeza é operação declarada da porta; auditoria continua em `evento`. `app/ponto/portas/celula.py:80`, `:1156`; `app/core/observ.py:134` |
| Relógio fora da borda | **Presente:** exemplos novos `app/colaboradores/services/vinculo.py:646`, `:1021`; exemplo remanescente `app/ponto/portas/celula.py:188` |
| Derivação paralela de fonte única | **Não introduzida pelo delta:** dono, janela e calendário continuam nas derivações existentes. `app/ponto/services/cadastro_futuro.py:38`; `app/ponto/management/commands/gerar_celulas.py:299`; `app/escala/folgas.py:629` |
| Selo/RED sem mordida | **Problema presente:** censo do relógio cobre somente sete funções e compara nomes finais; não resolve apelidos/`getattr`. `app/ponto/tests/test_cadastro_futuro_c5d.py:25`, `:185`, `:195` |
| Ordem de trava | **Nenhuma inversão nova demonstrada:** snapshot mantém vínculo→célula; fechamento escreve vínculo antes do suplemento. Ausência global de deadlock: [nao verificado]. `app/colaboradores/services/vinculo.py:1008`, `:1013`, `:629`, `:650`; `app/ponto/services/cadastro_futuro.py:31` |
| Pouso antes do pré-requisito | **[nao verificado]**: cópia/delta não demonstram a ordem efetiva dos pousos. | [nao verificado]
| Campo com dois significados | **Não identificado em `hoje` no delta:** referência civil separada do dia-alvo. `app/ponto/services/cadastro_futuro.py:27`, `:43`; `app/ponto/portas/celula.py:1137` |

**Há regressão concreta no instrumento anterior:** `desfazer_apaga_admin` perdeu a âncora. Ele procura o corpo antigo, enquanto o novo possui `atomic` e `_t = evento`; falha na preparação com “mutante sem alvo unico”, antes de testar a preservação administrativa. Esse vermelho não prova mais aquela propriedade. (`app/ponto/tests/test_cadastro_futuro_c5.py:37`, `:78`, `:103`, `:199`; `app/ponto/portas/celula.py:1145`, `:1147`)

6. **LEIS dos sítios**

- **L-007:** o ponto específico da triagem foi atendido: recusa de trilha ausente e rollback conjunto. (`app/docs/LEIS.md:65`; `app/ponto/portas/celula.py:1145`, `:1151`, `:1163`)
- **Clock Injection permanece descumprido no caminho:** a exigência é leitura uma vez na borda; serviços compostos continuam lendo seu próprio relógio. (`app/docs/PLANO-PADROES.md:91`, `:92`, `:94`; `app/colaboradores/services/vinculo.py:646`, `:314`)
- **L-112 preservada pelo delta:** o executor continua consumindo início e âncora declarados, gravando a âncora explícita e fechando a inativa sobreposta pela porta. O delta não converte a sugestão por batida em limite da declaração. (`app/docs/LEIS.md:154`; `app/colaboradores/services/vinculo.py:179`, `:235`, `:274`, `:285`)
- **L-021 e L-020:** o delta mantém o filtro de vigência, a escrita pela porta e a proteção de célula editada; não demonstra ampliação nova do passado. (`app/docs/LEIS.md:71`, `:72`; `app/colaboradores/services/vinculo.py:629`; `app/ponto/portas/celula.py:292`; `app/ponto/services/cadastro_futuro.py:27`, `:32`)

7. **DESCARTADO 3**

**Continua DESCARTADO.** O cron conserva o alcance de hoje consultando a juíza, passa `hoje` explicitamente e não usa `ativa=True` como resposta sobre o dono daquele dia. Vínculo inativo com fim que cobre o dia continua admitido pela autoridade; o sem vigência declarada continua recusado pela porta. O delta não altera esses critérios. (`app/ponto/management/commands/gerar_celulas.py:303`, `:321`, `:324`; `app/escala/alimentacao.py:272`, `:282`; `app/ponto/portas/celula.py:148`)

## 12/11 18:16 · sessao 452d55645847 · - · - · origem=Ronald (tmux)

Mapa de projeções, somente leitura:

- `calendario`: grava sets por `(dia,mês)` e compara `strftime('%H:%M')`; trocar por data completa e timestamps aware. Para legado, `b.orfa=None` preserva classificação desconhecida; o render usa timestamp da batida. (`colaboradores/services/calendario.py:380`, `colaboradores/services/calendario.py:622`, `templates/colaboradores/partials/_calendario_grade.html:121`)
- PDF: `_entrada_espuria`, associação de batidas e supressão de alertas devem comparar `periodo.entrada`/`b.timestamp` diretamente com instantes. As agregações `or []` e `or set()` apagam desconhecimento. (`relatorios/pdf_espelho.py:174`, `relatorios/pdf_espelho.py:477`, `relatorios/pdf_espelho.py:552`, `relatorios/pdf_espelho.py:607`, `relatorios/pdf_espelho.py:960`)
- **PERGUNTA DE DESENHO:** filtro de anomalias do PDF recebe mensagens textuais e as identifica por substring HH:MM. Não há identidade temporal disponível nesse caminho; preservar comportamento por instante exige mudança do contrato de anomalias. (`relatorios/pdf_espelho.py:716`, `relatorios/pdf_espelho.py:719`)
- `orfa_classe`: parser por fatia e diferença de minutos devem virar diferenças entre datetimes; `classificar_orfas` precisa propagar desconhecido em vez de `{}`. (`escala/orfa_classe.py:31`, `escala/orfa_classe.py:44`, `escala/orfa_classe.py:58`)
- `horas_reais_do_dia`: já possui saída `[int|None]`; pode calcular deslocamentos dos instantes reais desde a origem do dia e publicar `None` para legado. O consumidor já responde `'nao_sei'`. (`ponto/turnos.py:2616`, `ponto/turnos.py:2643`, `ponto/turnos.py:2646`, `ponto/turnos.py:2667`)
- `supra_juiz`: `join(_orfas)` quebra com datetime; formatar no texto final. O fallback por batidas deve ficar restrito à chave ausente, sem executar quando `orfas=None`. (`ponto/supra_juiz.py:514`, `ponto/supra_juiz.py:520`, `ponto/supra_juiz.py:521`)
- Espelho: transportar `None`; templates devem formatar `o|date:"H:i"` e renderizar órfãs independentemente de `realizado_grade`. (`ponto/services/espelho.py:689`, `ponto/services/espelho.py:844`, `templates/ponto/espelho.html:151`, `templates/ponto/espelho.html:240`)
- Contadores: `furos_diarios`, `status_dia`, bordas e causas devem usar `n_orfas` conhecido quando identidades forem desconhecidas. (`escala/services/furos_diarios.py:22`, `ponto/status_dia.py:188`, `ponto/services/bordas_realizado.py:49`, `folha/causas.py:43`)
- **PERGUNTA DE DESENHO:** sem lista e sem contagem, `_furos_do_dia` retorna tupla numérica e `_teve_batida_na_ata` retorna bool consumido por `int`; não possuem saída atual para desconhecido. (`escala/services/furos_diarios.py:12`, `escala/services/furos_diarios.py:23`, `ponto/services/bordas_realizado.py:21`, `ponto/services/bordas_realizado.py:80`)
- `classificar_causa` já tem resultado `indeterminado`; `dia_perfeito` pode negar prova de perfeição quando órfãs forem desconhecidas. (`folha/causas.py:34`, `folha/causas.py:80`, `ponto/services/simular_dia.py:12`)
- Render do detector de cluster precisa formatar datetime antes de `join`; detector de par relâmpago também apaga desconhecimento na contagem. (`ponto/management/commands/detectar_cluster_espurio.py:87`, `ponto/management/commands/detectar_par_relampago.py:140`)

## 12/11 18:16 · sessao 4960b88c2174 · - · - · origem=Ronald (tmux)

Leitura concluída; nenhum arquivo alterado.

| Leitor | Caminho existente para “não sei” / adaptação |
|---|---|
| `_celula_furada` | Já retorna `None` para célula/ata ausente. Presença pode continuar provada por `n_orfas>0`; identidade desconhecida sem contagem positiva precisa retornar `None`, evitando o `False` final. `chamados/juizes.py:488`, `:505`, `:510`, `:518`. |
| `_plantio_preenche_furo_na_data` | Contrato explícito: leitura ausente/ambígua → `False`, segunda opinião não absolve plantio. Migrar sem HH:MM exige adaptar DP para coordenadas temporais: slots, luzes e plantio continuam strings hoje. `chamados/services/materializacao.py:143`, `:177`, `:182`; `ponto/propositor.py:112`, `:122`. |
| `periodos_do_dia` | Pode transportar `orfas=None` no resultado; já dispõe de `nao_sei`, `perguntas` e `desalinhado`. Atenção: `nao_sei` é sobrescrito ao final. `ponto/juiz_batida.py:152`, `:161`, `:164`, `:252`. |
| `_leitura_da_celula` | Migração direta: `None` do leitor único → `None`; rajada por diferença entre datetimes. Consumidor já conserva `None` no catálogo. Descrição formata HH:MM; contexto persiste ISO. `ponto/services/cluster_espurio.py:15`, `:29`, `:34`, `:74`, `:210`, `:211`. |
| `assinaturas_do_colab` — A5 | Não dispõe de estado de medição: saída é `{codigo: [ocorrencias]}`, posteriormente contada por `len`. Categoria recorrente pode derivar de hora/minuto do instante localizado; legado não pode simplesmente desaparecer. **Pergunta de desenho necessária para publicação de A5 desconhecida.** `ponto/services/esmeril_espelho.py:79`, `:125`, `:139`, `:198`. |
| `medir_sobra_com_tipo_trocado` | Saída somente `(achados, universo)`; não há contador de não medidos. Peneira atual é circular por HH:MM e confirmação ainda identifica órfã por HH:MM. **Pergunta de desenho necessária para legado não medido.** `ponto/management/commands/tripwire_celulas.py:135`, `:165`, `:183`, `:195`, `:237`, `:264`, `:297`. |
| `vigia_pareador_celula.censo` | Já publica `cego`, porém global por universo vazio; não há representação explícita por célula desconhecida. Comparação migra para `b.timestamp in orfas_civil`. **Pergunta de desenho: legado marca todo censo cego ou recebe contador próprio?** `ponto/services/vigia_pareador_celula.py:43`, `:60`, `:74`, `:109`, `:113`. |
| `orfas_e_isencao_da_ata` | `rg` nos Python permitidos encontrou somente definição e teste; nenhum chamador produtivo. L-111 permite apagar função e teste específico. Chamadas dinâmicas são [não verificado]. `relatorios/cartao_pela_celula.py:277`; `relatorios/tests/test_cartao_pela_celula.py:159`, `:166`. |

Propositor possui saída explícita `MOTIVO_NAO_AVALIAVEL`, utilizável para órfãs desconhecidas; a guarda precisa ocorrer antes de `or []`. A DP atual inteira trabalha com minutos extraídos de strings, portanto converter somente a órfã para HH:MM perderia novamente sua identidade. `ponto/propositor.py:18`, `:39`, `:44`, `:55`, `:112`, `:122`.

## 12/11 18:16 · sessao c305d899aace · - · - · origem=Ronald (tmux)

P2 continua PROCEDENTE no caminho completo: o serviço `cadastro_futuro` deixou de ler relógio e exige `hoje`, mas o delta deslocou leituras para serviços internos, enquanto a regra exige uma leitura na borda e passagem até o julgamento. (`app/ponto/services/cadastro_futuro.py:17`; `app/docs/PLANO-PADROES.md:92`)

Censo do caminho direto de cadastro/materialização, incluindo as caudas imediatas:

| Arquivo::função | Leitura | Classe |
|---|---|---|
| `app/core/signals.py::_feriado_regenerar_futuras` | `localdate`, :36 | BORDA receiver (:28) |
| `app/core/signals.py::_feriado_excluido_regenerar_futuras` | `localdate`, :43 | BORDA receiver (:39) |
| `app/escala/signals.py::_ec_vigencia_regenera_celulas` | `localdate`, :67 e :68 | BORDA receiver (:34); já são duas leituras |
| `app/escala/signals.py::_ec_ancora_regenera_celulas` | `localdate`, :119 | BORDA receiver (:86) |
| `app/escala/admin.py::TipoEscalaAdmin.save_model` | `localdate`, :15 | BORDA admin (:11) |
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py::Command._rodar` | `localdate`, :67 e :227 | BORDA command, chamado por `handle` (:48-51) |
| `app/ponto/management/commands/gerar_celulas.py::Command.handle` | `date.today`, :339 | BORDA command (:337) |
| `app/colaboradores/services/vinculo.py::julgar_vinculo` | `date.today`, :71 | INTERIOR (:52) |
| `app/escala/models.py::EscalaColaborador.recusa_ancora_futura` | `hoje or timezone.localdate`, :965 | INTERIOR; chamado sem `hoje` pelo juiz (:133) |
| `app/colaboradores/services/vinculo.py::_copiar_folga_padrao` | `date.today`, :163 | INTERIOR; executor chama :278/:292; foto chega à re-lavra em `app/escala/folgas.py:215` |
| `app/colaboradores/services/vinculo.py::executar_vinculo` | `localdate`, :314 | INTERIOR executor (:170) |
| `app/colaboradores/services/vinculo.py::trocar_calendario_de_folga` | `localdate`, :460 | INTERIOR; view chama `app/colaboradores/views.py:239` |
| `app/colaboradores/services/vinculo.py::fechar_vigencia` | `localdate`, :646 | INTERIOR; executor chama :208/:235 |
| `app/colaboradores/services/vinculo.py::reabrir_vigencia_impossivel` | `date.today`, :728; `localdate`, :740 | INTERIOR; command chama `app/escala/management/commands/corrigir_vinculo_vigencia.py:84` |
| `app/colaboradores/services/vinculo.py::marcar_trabalha_em_feriado` | `localdate`, :885 | INTERIOR (:857) |
| `app/colaboradores/services/vinculo.py::reverter_para_snapshot` | `localdate`, :1021 | INTERIOR; serviço chama `app/chamados/services/acoes_chamado.py:149` |
| `app/colaboradores/services/estrutura.py::_evento_do_municipio` | `localdate`, :62 | INTERIOR; outras portas chamam :72/:97 |
| `app/escala/services/cadastro_tipo.py::salvar_tipo` | `localdate`, :310 | INTERIOR; chamado inclusive pelo serviço `app/chamados/services/limbo_folgas.py:92` |
| `app/escala/folgas.py::salvar_calendario_de_folga` | `localdate`, :599 | INTERIOR; view chama `app/escala/views.py:356` |
| `app/escala/folgas.py::relavrar_celulas_da_foto` | `localdate`, :280 | INTERIOR; portas chamam :337/:380/:418/:523 |
| `app/escala/folgas.py::aplicar_folgas_em_lote` | `hoje or _tz.localdate`, :401 | INTERIOR; view chama `app/escala/views_plano_folgas.py:185` |
| `app/escala/folgas.py::reparentar_folga` | `hoje or _tz.localdate`, :512 | INTERIOR; command chama `app/escala/management/commands/reparentar_folgas.py:34` |
| `app/ponto/portas/celula.py::regenerar_celulas_vinculo` | fallback `date.today`, :188; `timezone.now`, :258/:303; `localdate`, :329 | INTERIOR; serviço futuro chama `app/ponto/services/cadastro_futuro.py:44` |
| `app/ponto/management/commands/gerar_celulas.py::_marcar_cobrancas_orfas` | `timezone.now`, :264 | INTERIOR; porta chama `app/ponto/portas/celula.py:322` |
| `app/ponto/portas/celula.py::_o_chao_do_dia_mudou` | `localdate`, :459 | INTERIOR; porta chama :387 |
| `app/colaboradores/services/pos_vinculo.py::reconciliar_apos_vinculo` | `_tz.localdate`, :56 | INTERIOR; executor chama `app/colaboradores/services/vinculo.py:342` |
| `app/ponto/portas/celula.py::gerar_celulas_periodo` | fallback `date.today`, :860 | INTERIOR; command chama `app/ponto/management/commands/gerar_celulas.py:349` |

As leituras `now` de `regenerada_em` e da trilha de cobrança são carimbos de escrita, enquanto :188/:329/:459 decidem classificação temporal; todos continuam dentro da porta/serviço. (`app/ponto/portas/celula.py:258`; `app/ponto/portas/celula.py:303`; `app/ponto/management/commands/gerar_celulas.py:264`; `app/ponto/portas/celula.py:333`; `app/ponto/portas/celula.py:464`)

Também resta o caminho de reversão, fora do suplemento principal: `reverter_regeneracao_dia` tem `hoje=None`, `timezone.now` :826 e `hoje or timezone.localdate` :831; chama a mesma limpeza D1 :834. (`app/ponto/portas/celula.py:773`)

Defaults restantes comprovados:

- `regenerar_celulas_vinculo(hoje=None)` e fallback :188. (`app/ponto/portas/celula.py:113`)
- `regenerar_celulas_dia(hoje=None)`, repassa à porta :768. (`app/ponto/portas/celula.py:736`)
- `gerar_celulas_periodo(hoje=None)` e fallback :860. (`app/ponto/portas/celula.py:855`)
- `aplicar_folgas_em_lote(hoje=None)` e fallback :401. (`app/escala/folgas.py:385`)
- `reparentar_folga(hoje=None)` e fallback :512; wrapper `reparentar_folgas_do_colaborador(hoje=None)` :529. O signal chama wrapper sem `hoje`. (`app/escala/signals.py:204`)
- `recusa_ancora_futura(hoje=None)` e fallback :965, efetivamente chamado sem `hoje`. (`app/escala/models.py:938`; `app/colaboradores/services/vinculo.py:133`)

Nas sete funções que o delta tornou obrigatórias, o censo AST de produção encontrou 23 chamadas, todas com `hoje=`; nenhuma ocorrência de alias de import nem literal exato para `getattr` foi encontrada. Os receivers efetivos também passam o parâmetro. (`app/core/signals.py:35`; `app/core/signals.py:43`; `app/escala/signals.py:81`; `app/escala/signals.py:132`; demais chamadas em `app/escala/folgas.py:298/:601/:640`, `app/escala/admin.py:15`, `app/colaboradores/services/estrutura.py:62`, `app/colaboradores/services/vinculo.py:333/:460/:650/:738/:898/:949/:1023`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:243`, `app/ponto/services/cadastro_futuro.py:77`, `app/escala/services/cadastro_tipo.py:323/:328/:366/:376/:388`). Chamada dinâmica construída externamente ou ocorrência real em produção: [não verificado].

O selo cobre apenas as sete funções de `SEM_DEFAULT`; deixa fora as portas internas que ganharam relógio e a própria porta da célula. Seu detector de chamadas compara o nome final, sem resolver aliases/getattr; o anti-vacuidade usa só chamada direta. (`app/ponto/tests/test_cadastro_futuro_c5d.py:25`; `app/ponto/tests/test_cadastro_futuro_c5d.py:185`; `app/ponto/tests/test_cadastro_futuro_c5d.py:195`)

Sobre meia-noite: `executar_vinculo` chama `fechar_vigencia` duas vezes antes de capturar `_hoje`; cada chamada captura seu próprio dia, e signals/fotos também podem ler relógio antes disso. Portanto o comentário “UM relógio no ato” não corresponde ao caminho. (`app/colaboradores/services/vinculo.py:208`; `app/colaboradores/services/vinculo.py:235`; `app/colaboradores/services/vinculo.py:646`; `app/colaboradores/services/vinculo.py:314`)

Consequência concreta de execução possível: encerramento às 23:59 de D classifica D+1 como futura e apaga seu julgamento; executor após 00:00 captura D+1, inclui esse mesmo dia no ramo até hoje e aciona pós-vínculo para um início em D+1. Além disso a cauda :459 pode julgar uma célula selecionada como futura pelo `_hoje` anterior. Isso demonstra decisões temporais diferentes no mesmo Salvar; ocorrência ou dano em produção não está no dado. (`app/ponto/services/cadastro_futuro.py:27`; `app/ponto/portas/celula.py:246`; `app/ponto/portas/celula.py:262`; `app/colaboradores/services/vinculo.py:319`; `app/colaboradores/services/vinculo.py:340`; `app/ponto/portas/celula.py:459`; `app/ponto/portas/celula.py:464`)

## 12/11 18:16 · sessao 55c1b880a856 · - · - · origem=Ronald (tmux)

P1 está curado no corpo da porta: ambos os ramos exigem retorno diferente de `None`, levantam `RuntimeError` dentro do `atomic` e propagam a falha. `evento` é best-effort e retorna `None` depois de engolir a falha. (app/ponto/portas/celula.py:1145; app/ponto/portas/celula.py:1151; app/ponto/portas/celula.py:1163; app/core/observ.py:105; app/core/observ.py:139)

As três chamadas diretas estão protegidas pelo `atomic` que também escreveu o DNA: as duas da regeneração ficam sob o bloco iniciado em `:220`; a reversão fica sob `:818`. Nenhuma dessas chamadas está dentro de `try/except` local. Portanto a falha desfaz no banco DNA e limpeza conjuntamente; o objeto Python já alterado não é explicitamente restaurado. (app/ponto/portas/celula.py:220; app/ponto/portas/celula.py:260; app/ponto/portas/celula.py:262; app/ponto/portas/celula.py:305; app/ponto/portas/celula.py:309; app/ponto/portas/celula.py:818; app/ponto/portas/celula.py:828; app/ponto/portas/celula.py:834; app/ponto/portas/celula.py:1156)

Há um ancestral que captura e segue: `censo_fase_12x36._curar` captura `Exception`, imprime `RECUSADO` e continua o laço. A captura acontece depois que `executar_vinculo`, decorado com `@transaction.atomic`, já saiu com erro e desfez seu ato. (app/ponto/management/commands/censo_fase_12x36.py:400; app/ponto/management/commands/censo_fase_12x36.py:421; app/ponto/management/commands/censo_fase_12x36.py:427; app/colaboradores/services/vinculo.py:169)

A view também captura `Exception`, mas encaminha para `so_recusa_de_negocio`, que relança esse `RuntimeError`; o chamador do snapshot captura somente `ValueError`. Os catches históricos dos signals e de `salvar_tipo` cobrem a parte até hoje; o suplemento futuro está fora deles. (app/colaboradores/views.py:431; app/colaboradores/views.py:438; app/core/erros.py:42; app/core/erros.py:75; app/chamados/services/acoes_chamado.py:148; app/escala/signals.py:73; app/escala/signals.py:80; app/escala/signals.py:125; app/escala/signals.py:131; app/escala/services/cadastro_tipo.py:323; app/escala/services/cadastro_tipo.py:328)

O teste de banco tem mordida estrutural: com `evento=None`, exige `RuntimeError`, recarrega a célula e confere ata, julgamento e veredito anteriores. Ignorar novamente o retorno faria o `assertRaises` falhar; executar esse teste no banco permanece [não verificado]. Há também teste do ramo conservado. (app/ponto/tests/test_cadastro_futuro_c5d_banco.py:38; app/ponto/tests/test_cadastro_futuro_c5d_banco.py:43; app/ponto/tests/test_cadastro_futuro_c5d_banco.py:56)

Executei os 13 testes c5d somente em memória: `OK`, rc=0. Executei ainda mutante somente em memória, trocando os dois guards `_t is None` por `False`: os três testes de recusa falharam com `RuntimeError not raised`, confirmando a mordida desses testes. (app/ponto/tests/test_cadastro_futuro_c5d.py:73; app/ponto/tests/test_cadastro_futuro_c5d.py:82; app/ponto/tests/test_cadastro_futuro_c5d.py:91)

Novo problema de instrumento: o mutante antigo `desfazer_apaga_admin` perdeu sua âncora porque c5d introduziu `atomic` e `_t = evento`. Confirmei executando o teste: ele falha em `setUp` com `mutante sem alvo unico`, antes de exercer o comportamento. Esse RED não comprova mais a preservação do julgamento administrativo. (app/ponto/tests/test_cadastro_futuro_c5.py:38; app/ponto/tests/test_cadastro_futuro_c5.py:78; app/ponto/tests/test_cadastro_futuro_c5.py:103; app/ponto/tests/test_cadastro_futuro_c5.py:199; app/ponto/portas/celula.py:1145; app/ponto/portas/celula.py:1147)

## 12/11 18:15 · sessao 6b14b64296ac · - · - · origem=Ronald (tmux)

- **Quem escreve os 13 campos:** `lavrar_veredito` escreve `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata`, `status_dia` e `status_rotulo`; `carimbar` escreve `julgada_em`, `impressao`, `ausencia`, `tem_entrada`, `tem_saida` e `insumos_em`; D1 limpa esses mesmos campos pela operação `lavrar_veredito` da mesma porta, sem escritor externo novo. `app/ponto/portas/celula.py:74`, `:1082`, `:1103`, `:1156`, `:1185`.

- **Leitores concretos:** `ata` e `veredito` alimentam o leitor da grade (`app/escala/services/leitor_celula.py:379`, `:424`); `veredito_via` protege decisão admin (`app/ponto/portas/celula.py:970`, `:1052`); `protestos` entra na contagem do cartório (`app/ponto/services/cartorio.py:1084`); `veredito_em` e `julgada_em` compõem o marco de invalidação das perguntas (`app/chamados/veredito_lavrado.py:191`); `status_dia/status_rotulo` alimentam o leitor do status (`app/ponto/status_dia.py:287`); `julgada_em/impressao` permitem pular julgamento já atualizado (`app/ponto/services/cartorio.py:1260`, `:1409`); `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` são comparados pelo vigia B5 e pela idempotência da própria porta (`app/ponto/management/commands/vigia_b5_ausencia.py:71`, `:75`, `:80`; `app/ponto/portas/celula.py:1181`, `:1220`).

- **Trilha:** a função D1 produz `acao`, `modelo='CelulaDia'`, `objeto_id=cel.pk`, descrição com motivo, antes/depois e ator; `core.observ.evento` escreve `LogAuditoria`, associa usuário/IP da request ou ator de processo. `app/ponto/portas/celula.py:1147`, `:1159`; `app/core/observ.py:108`, `:118`, `:134`. Leitura nominal de `julgamento_da_futura_*` encontrada somente nos testes; a tela faz leitura genérica dos logs. `app/ponto/tests/test_cadastro_futuro_c5.py:157`, `:204`; `app/ponto/tests/test_cadastro_futuro_c5d.py:78`, `:87`; `app/core/views.py:705`, `:709`. O vigia futuro **não lê essa trilha**: lê `julgada_em/ata/veredito` e `veredito_via`. `app/ponto/services/vigia_cadastro_futuro.py:21`, `:45`.

- **Semântica de `hoje`:** no suplemento é o dia civil usado para escolher competência e futuro estrito; o dia da célula/turno continua separado em `dia`/`cel.data`. Não identifiquei significado duplo no parâmetro alterado. `app/ponto/services/cadastro_futuro.py:26`, `:27`, `:43`, `:45`; `app/ponto/portas/celula.py:1137`. `timezone.localdate()` produz o valor nas portas, enquanto o cron usa `datetime.date.today()`; equivalência de fuso efetiva em produção [não verificado]. `app/colaboradores/services/vinculo.py:314`; `app/ponto/management/commands/gerar_celulas.py:339`.

- **Juiz novo/derivação paralela: ausentes no delta examinado.** O suplemento usa o carregador `vinculos_dos_dias`; este chama `vinculo_do_dia`, autoridades registradas no anexo vivo. A sincronização da foto usa a derivação existente do calendário. `app/ponto/services/cadastro_futuro.py:38`; `app/escala/alimentacao.py:334`, `:358`; `app/docs/JUIZES.md:120`; `app/escala/folgas.py:629`.

- **Ordem de travas:** o snapshot mantém E→C: vínculos por PK, depois células; `fechar_vigencia` escreve E antes de chamar suplemento que trava C. O delta acrescenta `atomic` na limpeza, sem nova trava explícita. Nenhuma inversão nova demonstrada; ausência global de deadlock [não verificado]. `app/colaboradores/services/vinculo.py:1008`, `:1013`, `:629`, `:650`; `app/ponto/services/cadastro_futuro.py:31`; `app/ponto/portas/celula.py:1145`.

- **Selo com problema novo:** o mutante `desfazer_apaga_admin` ainda procura o corpo anterior, sem o novo `atomic` e sem `_t =`; seu alvo desapareceu. Isso exige reconferir a execução do mutante, sem declarar selo completo por inspeção. `app/ponto/tests/test_cadastro_futuro_c5.py:37`; `app/ponto/portas/celula.py:1146`.

- **L-112 preservada no delta:** executor continua lendo `v['data_inicio']` e `v['data_ancora']`, gravando a declaração explícita e fechando a vigência inativa sobreposta pela porta. O delta só unifica parte das leituras posteriores de relógio; não transforma batida em limite da declaração. `app/colaboradores/services/vinculo.py:179`, `:235`, `:274`, `:285`; `app/docs/LEIS.md:154`.

- **Pré-requisito/pouso:** atendimento à condição de pousar após E4B/E3 não demonstrável pela cópia e pelo delta anexado [não verificado]. [nao verificado]

- **DESCARTADO 3 continua descartado:** o cron ainda pergunta ao carregador do juiz, passando `hoje` explicitamente; vínculo inativo com fim que cobre o dia continua aceito pela autoridade. O delta não altera esse caminho. `app/ponto/management/commands/gerar_celulas.py:303`, `:321`, `:324`; `app/escala/alimentacao.py:272`, `:282`.

## 12/11 18:10 · sessao 2f0114aa1e5a · TIPO-NA-FONTE · verificador · origem=Code

**Parcial; mantenho a não aprovação como instrumento completo de “todo leitor e escritor”.** O delta cura defeitos concretos, mas introduz perdas silenciosas por sementes de laço e atribuições indevidas de proveniência. A medida também continua sem excluir acessor ou aplicar teto. (`bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`, `bin/leitores_de_chave.py:695`)

Execução integral do selo, publicação do censo, identidade Git do pouso e comportamento em produção: **[não verificado]**. O selo contém gravações e remoção de temporários; seus casos foram examinados sem executá-lo. (`bin/tests/test_leitores_de_chave.sh:27`, `bin/tests/test_leitores_de_chave.sh:29`, `bin/tests/test_leitores_de_chave.sh:82`)

**1. Destino dos achados da volta 1.**

| Achado anterior | Veredito nesta volta e lugar no delta |
|---|---|
| Briefs chamarem obrigatoriamente a ferramenta; entrega ao construtor/preverificador | **Não curado demonstravelmente.** O selo chama a ferramenta; o disjuntor continua exigindo referências textuais, sem conferir sua saída. Entrega efetiva: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:35`; `bin/disjuntor.py:1565`) |
| Universo Python, separação produção/testes, parse inválido, chave ausente, uso errado | **Garantias anteriores mantidas.** Parse e declaração falham antes da saída; consulta sem resultado continua rc 1 nomeado. Isso não prova completude da proveniência. (`bin/leitores_de_chave.py:623`, `bin/leitores_de_chave.py:635`, `bin/leitores_de_chave.py:683`, `bin/leitores_de_chave.py:736`) |
| Apelido de dois ou mais saltos | **Curado para cadeias acíclicas simples; parcial no geral.** `alias()` substitui o corte de um salto, mas ciclos de atribuições válidas podem perder proveniência. (`bin/leitores_de_chave.py:257`, `bin/leitores_de_chave.py:263`, `bin/leitores_de_chave.py:270`, `bin/leitores_de_chave.py:288`) |
| Laço perdia `lamp` e as escritas de subchaves | **Curado no sítio apontado.** Agora o laço transmite `lampadas.[]`, recuperando as escritas de `acesa`, `luz` e `dispensada`. A solução geral é **parcial**, pelos defeitos de escopo abaixo. (`bin/leitores_de_chave.py:168`; `app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`) |
| Renomear somente o parâmetro declarado encolhia o censo silenciosamente | **Curado para o mutante exato; parcial como guarda de declaração.** Agora confere o nome, mas aceita também qualquer variável local atribuída com aquele nome. (`bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:608`; `bin/tests/test_leitores_de_chave.sh:82`) |
| Reatribuição mantinha proveniência indevida | **Parcial.** Apelidos comuns ambíguos viram lacuna e deixam de contar; isso também apaga acessos válidos anteriores. Nomes semeados continuam ignorando a reatribuição. (`bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:286`) |
| Dependência manual de `PARAMS/RETURNS` | **Não curada como fonte paralela de proveniência.** Os mapas continuam alimentando sementes e retornos presumidos; a guarda ficou mais forte, mas não verifica que o valor seja o JSON declarado. (`bin/leitores_de_chave.py:48`, `bin/leitores_de_chave.py:75`, `bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:330`, `bin/leitores_de_chave.py:608`) |
| `.ata/.dna`, `getattr` e argumentos nomeados aceitos sem provar classe | **Não curado; agora mais claramente declarado.** Permanecem candidatos sintáticos. (`bin/leitores_de_chave.py:44`, `bin/leitores_de_chave.py:282`, `bin/leitores_de_chave.py:309`, `bin/leitores_de_chave.py:554`) |
| Comprehension sobre lista literal/chamada, chamadas indiretas e consultas ORM | **Não curado.** Lista literal/chamada e ORM continuam explicitamente fora; a resolução de chamadas segue reconhecendo formas específicas. (`bin/leitores_de_chave.py:43`, `bin/leitores_de_chave.py:44`, `bin/leitores_de_chave.py:307`) |
| Contador confundido com persistência/catraca; sem exclusão do acessor ou teto | **Parcial na comunicação; não curado na medida contratada.** O delta explica a unidade e a escrita em memória, mas a seleção continua sendo eventos reconhecidos de topo. (`bin/leitores_de_chave.py:26`, `bin/leitores_de_chave.py:38`, `bin/leitores_de_chave.py:695`) |
| Meta zero retornava erro | **Curado.** Quando consegue medir, imprime inclusive zero e retorna rc 0. Parse, raiz e declaração ainda podem produzir rc 2 antes disso. (`bin/leitores_de_chave.py:675`, `bin/leitores_de_chave.py:683`, `bin/leitores_de_chave.py:687`, `bin/leitores_de_chave.py:697`) |
| Selo ignorava papéis, DNA e rc do contador sintético | **Curados nos casos exercitados.** Agora compara papéis, confere rc e exercita DNA com controle, mutante e vizinha. (`bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:131`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:164`) |
| Falha de `mktemp` permitia caminhos fora do temporário | **Curado para a falha apontada.** Aborta por falha, vazio ou ausência do diretório antes dos redirecionamentos. (`bin/tests/test_leitores_de_chave.sh:27`, `bin/tests/test_leitores_de_chave.sh:28`, `bin/tests/test_leitores_de_chave.sh:35`) |
| Árvore real só exigia presença mínima; raiz só provava presença | **Não curados.** Continuam mínimo de leitores/escritores, número positivo e existência de três arquivos; não há conferência de completude ou identidade do hash. (`bin/tests/test_leitores_de_chave.sh:23`, `bin/tests/test_leitores_de_chave.sh:46`, `bin/tests/test_leitores_de_chave.sh:54`) |

As constatações anteriores sobre ausência de escrita nos dados da aplicação e relógio apenas de duração permanecem: o instrumento lê fontes; o selo escreve cópias e usa `date` para duração. Acessor, migração e lei dos instantes não são implementados por esses caminhos. (`bin/leitores_de_chave.py:629`, `bin/leitores_de_chave.py:710`; `bin/tests/test_leitores_de_chave.sh:58`, `bin/tests/test_leitores_de_chave.sh:183`; `aval.txt:5`, anexo)

**2. Novos riscos do delta.**

| Mudança | Falso verde / falso vermelho | [nao verificado]
|---|---|
| Guarda `PARAMS` | **Pode aceitar declaração semanticamente falsa:** renomear o parâmetro e deixar um local `ata={}` satisfaz `nomes`; esse local recebe a semente declarada. Presença do nome não prova sua origem. (`bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:608`) |
| Guarda `RETURNS` | **Pode aceitar retorno sem JSON:** `return None` e `return 42` satisfazem `retorna`, porque ambos têm expressão AST. Chamadas continuam recebendo raiz presumida pelo mapa. A guarda detecta ausência de função ou de expressão de retorno. (`bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:330`, `bin/leitores_de_chave.py:613`) |
| Semeadura de laço | **Produz falsos positivos e negativos.** A semente vale para todo o escopo; até uma semente vazia tem prioridade sobre aliases. Também contorna a verificação de reatribuição. (`bin/leitores_de_chave.py:165`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`) |
| Alias memoizado | **Resolve a cadeia simples, mas pode perder ciclos válidos:** `a=c.ata; b=a; a=b; a["k"]; b["j"]` perde ambos na reprodução em memória. O vazio devolvido por `busy` participa do resultado memoizado e da classificação como reatribuído. (`bin/leitores_de_chave.py:263`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:270`) |
| Reatribuição excluída | **Pode dar falso verde na medida:** `a=c.ata; a["k"]; a={}` apaga o acesso válido anterior. A consulta de `k` pode dar falso negativo; o contador pode diminuir com rc 0. A lacuna não acompanha a saída do contador. (`bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:274`, `bin/leitores_de_chave.py:697`, `bin/leitores_de_chave.py:736`) |
| rc 0 quando mede | **Correto para permitir a meta zero; insuficiente como certificado de completude.** Lacunas não impedem rc 0 e o modo não informa sua quantidade. (`bin/leitores_de_chave.py:697`, `bin/leitores_de_chave.py:721`; `aval.txt:5`, anexo) |

O sítio real pedido é **`app/chamados/services/materializacao.py:177` e `:178`**. Os três cortes de string da lambda `_h_ok` passam a aparecer como `orfas.[].<dinamica>` por causa do `for h` **posterior**, em `:181`. O indexador não separa o escopo de lambda/comprehension, e a semeadura aplica o nome à função inteira. Isso é atribuição indevida de proveniência, não mudança física da linha do arquivo. (`app/chamados/services/materializacao.py:177`, `app/chamados/services/materializacao.py:178`, `app/chamados/services/materializacao.py:181`; `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:199`, `bin/leitores_de_chave.py:286`)

Há outro exemplo real: o `x["entrada"]` que percorre `turnos` passa a ser atribuído a `lampadas.[].entrada`, contaminado pelo `x` da comprehension anterior de lâmpadas. (`app/ponto/services/diagnostico_escala.py:154`, `app/ponto/services/diagnostico_escala.py:156`, `app/ponto/services/diagnostico_escala.py:171`; `bin/leitores_de_chave.py:286`)

Dois contraexemplos adicionais reproduzidos em memória delimitam a falha:

- `a=c.ata; a["k"]; for a in []: pass` perde `k` **sem lacuna**: o laço instala semente vazia e impede a resolução do alias. (`bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`)
- `for a in c.ata["xs"]: pass; a={}; a["k"]` emite indevidamente `xs.[].k`, **sem lacuna**: a semente prevalece sobre os bindings. (`bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:286`)

Esses falsos positivos podem tornar vermelha uma checagem por presença de acesso; as perdas podem tornar verde uma checagem por diminuição/zero. Neste contador específico, eventos aninhados falsos ficam fora de `top`; portanto não se deve atribuir automaticamente aumento numérico aos exemplos aninhados. (`bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:699`, `bin/leitores_de_chave.py:736`)

**3. Mordida e vacuidade do selo.**

| Caso | Avaliação |
|---|---|
| **f2** | **Morde o defeito exato e não é vazio:** exige âncora antes da mutação, renomeia o parâmetro e cobra rc 2, nome declarado e stdout vazio. Não demonstra verdade da proveniência nem resistência a um local homônimo. (`bin/tests/test_leitores_de_chave.sh:80`, `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:84`; `bin/leitores_de_chave.py:188`) |
| **h** | **Morde a regressão zero→rc 1.** Universo sem acesso é o objeto do teste, não vacuidade: exige exatamente zero, rc 0 e uma ressalva do rótulo. Os outros casos exigem contagens positivas/exatas. (`bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:54`, `bin/tests/test_leitores_de_chave.sh:145`) |
| **i** | **Morde DNA ausente e desaparecimento da chave:** exige medida positiva, controle conhecido, mutação, rc 1 nomeado e vizinha preservada. Não prova cobertura completa de DNA. (`bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:168`, `bin/tests/test_leitores_de_chave.sh:171`, `bin/tests/test_leitores_de_chave.sh:175`) |
| **g ampliado** | **Morde dois saltos simples, laço simples e papéis.** A reatribuição testada contém somente acesso posterior ao valor não JSON; não testa acesso válido anterior, ciclos ou sementes vazias. Lacunas são comparadas por conjunto caminho/forma, sem localização ou multiplicidade. (`bin/tests/test_leitores_de_chave.sh:113`, `bin/tests/test_leitores_de_chave.sh:119`, `bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:134`) |

**Falta mutante de `RETURNS`.** f/f2 alteram uma função de `PARAMS`; os sintéticos desligam declarações. Retirar somente a verificação de `RETURNS` não contraria nenhuma dessas asserções. Logo, “todos os casos mordem” não demonstra mordida desse ramo. (`bin/leitores_de_chave.py:61`, `bin/leitores_de_chave.py:610`; `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:91`, `bin/tests/test_leitores_de_chave.sh:129`, `bin/tests/test_leitores_de_chave.sh:184`)

**É furo de cobertura da garantia adicionada**, mas o contrato fornecido exige explicitamente o mutante de chave conhecida sem resultado, não um mutante para cada ramo de declaração. O furo semântico de aceitar qualquer expressão retornada existe independentemente dessa ausência de teste. (`contrato.txt:6`, anexo; `bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:613`)

**4. ESMERIL-SEMÂNTICA: as saídas compartilham eventos, mas seus números não são intercambiáveis.**

| Saída | Quem escreve; quem lê; significado |
|---|---|
| **Texto** | `run()` imprime eventos selecionados por seção, incluindo descendentes e `*`. O selo consulta principalmente rc/stderr dessas chamadas; não valida o conteúdo da listagem textual. (`bin/leitores_de_chave.py:702`, `bin/leitores_de_chave.py:727`, `bin/leitores_de_chave.py:730`; `bin/tests/test_leitores_de_chave.sh:62`, `bin/tests/test_leitores_de_chave.sh:67`) |
| **`--json`** | `run()` publica listas filtradas, lacunas globais e resumo global. O selo lê sítios, papéis, formas, separação e lacunas. `operacoes_topo` é o mesmo total usado pelo contador; tamanho da lista `producao` não é esse total. (`bin/leitores_de_chave.py:713`, `bin/leitores_de_chave.py:719`, `bin/leitores_de_chave.py:721`; `bin/tests/test_leitores_de_chave.sh:46`, `bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:134`) |
| **`--contar-crus`** | Imprime `len(top)`: eventos reconhecidos de produção cujo caminho é exatamente uma chave literal de topo. O selo lê o número e rc; h também lê parte do rótulo. Descendentes, `*`, caminhos dinâmicos e aliases descartados não compõem esse total. (`bin/leitores_de_chave.py:694`, `bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:699`; `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`) |

A unidade continua sendo evento deduplicado por posição/caminho/papéis/forma. Uma operação com LEITOR e ESCRITOR vale um evento no contador, mas participa das duas contagens por papel no JSON. (`bin/leitores_de_chave.py:361`, `bin/leitores_de_chave.py:715`, `bin/leitores_de_chave.py:716`)

**O rótulo stderr melhorou e descreve o recorte principal**, inclusive escrita em memória e ausência de exclusão do acessor. Entretanto, não explicita “reconhecidos”, exclusão de reatribuídos ou lacunas. O selo ainda chama a medida de “catraca”, enquanto o instrumento declara que teto/catraca não mora nele; o contrato exige acessos fora do acessor. Portanto a concordância de significado continua parcial. (`bin/leitores_de_chave.py:29`, `bin/leitores_de_chave.py:38`, `bin/leitores_de_chave.py:274`; `bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:145`; `contrato.txt:5`, anexo)

Há ainda rótulo textual desatualizado: “lacunas ... chave dinamica/copia inteira” agora também totaliza `apelido`. (`bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:732`)

Consumidores efetivos em briefs, construtor ou HAIKU, além do selo demonstrado: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:38`)

**5. QUEM ESCREVE e QUEM LÊ os valores alterados pelo diff.**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `ROTULO_CRUS` | Literal. (`bin/leitores_de_chave.py:38`) | `run()`→stderr; h confere substring. (`bin/leitores_de_chave.py:698`; `bin/tests/test_leitores_de_chave.sh:157`) |
| `CONTA_REATRIBUIDO` | Constante `False`. (`bin/leitores_de_chave.py:40`) | Retorno de `alias()`. (`bin/leitores_de_chave.py:274`) |
| `NAO_COBERTOS` | Enumeração de limites. (`bin/leitores_de_chave.py:41`) | JSON e texto. (`bin/leitores_de_chave.py:722`, `bin/leitores_de_chave.py:734`) |
| Bindings em `env` | Indexação de atribuições→`bind()`. (`bin/leitores_de_chave.py:142`, `bin/leitores_de_chave.py:212`) | Novo resolvedor de aliases. (`bin/leitores_de_chave.py:267`) |
| `busy` | Inicialização; inclusão/remoção durante resolução. (`bin/leitores_de_chave.py:117`, `bin/leitores_de_chave.py:266`, `bin/leitores_de_chave.py:268`) | Guarda de ciclo. (`bin/leitores_de_chave.py:263`) |
| `memo[(escopo,nome)] = (out, reatribuido)` | Resolução dos bindings; limpeza por rodada e ao terminar semeadura. (`bin/leitores_de_chave.py:163`, `bin/leitores_de_chave.py:175`, `bin/leitores_de_chave.py:270`) | Consulta/cache de `alias()`. (`bin/leitores_de_chave.py:265`, `bin/leitores_de_chave.py:271`) |
| `ps`, `atual`, `novo`; sementes `<chave>.[]` | Laço de até quatro rodadas; união em `seeds`. (`bin/leitores_de_chave.py:162`, `bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:171`) | Controle de nova rodada e `paths(Name)`; depois emissão de eventos. (`bin/leitores_de_chave.py:173`, `bin/leitores_de_chave.py:286`, `bin/leitores_de_chave.py:447`, `bin/leitores_de_chave.py:482`) |
| Lacuna `reatribuido:<nome>` / `apelido` | `alias()`; lacunas preliminares são apagadas após semeadura. (`bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:273`) | `censo()`→JSON/total textual; g lê caminho/forma. (`bin/leitores_de_chave.py:639`, `bin/leitores_de_chave.py:721`, `bin/leitores_de_chave.py:732`; `bin/tests/test_leitores_de_chave.sh:134`) |
| Assinatura `nomes` | Parâmetros e nomes locais `Store`. (`bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`) | Guarda `PARAMS`. (`bin/leitores_de_chave.py:608`) |
| Assinatura `retorna` | Qualquer `Return` com expressão. (`bin/leitores_de_chave.py:190`) | Guarda `RETURNS`. (`bin/leitores_de_chave.py:613`) |
| `defs[f.rel]`, `faltam` | `censo()` guarda assinaturas; guard acrescenta faltantes. (`bin/leitores_de_chave.py:634`, `bin/leitores_de_chave.py:605`, `bin/leitores_de_chave.py:609`, `bin/leitores_de_chave.py:614`) | `run()` imprime diagnóstico e retorna rc 2; f/f2 conferem. (`bin/leitores_de_chave.py:687`, `bin/leitores_de_chave.py:690`; `bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:93`) |
| `len(top)` / rc 0 | Seleção produção→topo; impressão/retorno. (`bin/leitores_de_chave.py:693`, `bin/leitores_de_chave.py:699`) | JSON e selo real/e/h/i. (`bin/leitores_de_chave.py:713`; `bin/tests/test_leitores_de_chave.sh:52`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:165`) |
| `_tmp` | `mktemp`, agora validado. (`bin/tests/test_leitores_de_chave.sh:27`, `bin/tests/test_leitores_de_chave.sh:28`) | Redirecionamentos, cópias, fixtures e limpeza. (`bin/tests/test_leitores_de_chave.sh:29`, `bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:57`, `bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:161`) |
| `LC` / parâmetro f2 | Caminho e `sed` de renomeação/restauração. (`bin/tests/test_leitores_de_chave.sh:79`, `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:87`) | Âncora, ferramenta e asserção. (`bin/tests/test_leitores_de_chave.sh:80`, `bin/tests/test_leitores_de_chave.sh:83`, `bin/tests/test_leitores_de_chave.sh:84`) |
| Fixture g; `G`, `ESP`, `A` | Heredoc; extração JSON e expectativa literal. (`bin/tests/test_leitores_de_chave.sh:113`, `bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:131`, `bin/tests/test_leitores_de_chave.sh:134`) | Comparações de eventos/lacunas e contador esperado 10. (`bin/tests/test_leitores_de_chave.sh:132`, `bin/tests/test_leitores_de_chave.sh:135`, `bin/tests/test_leitores_de_chave.sh:145`) |
| `Z` / fonte sem acesso | Fixture zero. (`bin/tests/test_leitores_de_chave.sh:155`) | Ferramenta e asserção h. (`bin/tests/test_leitores_de_chave.sh:156`, `bin/tests/test_leitores_de_chave.sh:157`) |
| `D`, `N_DNA`, chave mutada | Cópia, captura do contador e substituição de literal. (`bin/tests/test_leitores_de_chave.sh:161`, `bin/test

[truncado em 20000 de 21407 caracteres]

## 12/11 18:08 · sessao f61fb656431c · - · - · origem=Ronald (tmux)

Achados de proveniência, com reprodução somente em memória:

- **Apelido de 2+ saltos: curado para cadeia acíclica simples.** `a=c.ata; b=a; b["k"]` agora aparece; resolução passa por `alias()` com memo e guarda de ciclo. (`bin/leitores_de_chave.py:257`, `bin/leitores_de_chave.py:288`)
- **Perda de `lamp` em `dispensar_por_ausencia`: curada no sítio real.** O laço transmite `lampadas.[]` e agora aparecem leitura em `:807`, escritas em `:808–810` e leitura em `:812`. (`bin/leitores_de_chave.py:167`, `app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:807`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`, `app/ponto/services/cartorio.py:812`)
- **Guarda de nome declarado: parcial.** Renomear exclusivamente o parâmetro de `_orfas_da_ata` agora falha; porém um local `ata={}` mantém a assinatura válida e recebe proveniência declarada, mesmo com parâmetro renomeado. O guard aceita parâmetros **ou qualquer `Name(Store)`**. (`bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:608`, `app/escala/services/leitor_celula.py:180`)
- **Guarda de `RETURNS`: parcial.** Detecta função ausente e `return` sem expressão; aceita `return None` e `return 42`, porque apenas testa `n.value is not None`. Logo prova presença sintática de retorno, sem provar que retorna JSON. (`bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:613`, `bin/leitores_de_chave.py:330`)

Falsos resultados novos confirmados:

1. **Semeadura retroativa real:** `h[2]`, `h[:2]`, `h[3:]` da lambda `_h_ok`, definidos em `app/chamados/services/materializacao.py:177` e `app/chamados/services/materializacao.py:178`, passam a sair como `orfas.[].<dinamica>`, devido ao `for h in ...orfas` **posterior**, em `app/chamados/services/materializacao.py:181`. Lambda e comprehension não criam escopo no indexador; sementes valem para a função inteira. (`bin/leitores_de_chave.py:167`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:199`, `bin/leitores_de_chave.py:285`)

2. **Outro falso positivo real:** `x['entrada']`, cujo `x` percorre `turnos`, vira `lampadas.[].entrada`; o `x` usado antes na comprehension de lâmpadas contamina o restante da função. O mesmo atinge `x` percorrendo `dias` em `:209–235`. (`app/ponto/services/diagnostico_escala.py:154`, `app/ponto/services/diagnostico_escala.py:156`, `app/ponto/services/diagnostico_escala.py:171`, `app/ponto/services/diagnostico_escala.py:208`, `app/ponto/services/diagnostico_escala.py:209`)

3. **Seed vazio apaga alias silenciosamente:** sintético `a=c.ata; a["k"]; for a in []: pass; a["j"]` perde os dois acessos, **sem lacuna**. O bloco sempre cria entrada em `seeds`, mesmo com `ps=set()`, e `paths(Name)` retorna essa entrada antes de tentar o alias. (`bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`)

4. **Seed também burla reatribuição:** sintético `for a in c.ata["xs"]: pass; a={}; a["k"]` emite `xs.[].k` sem lacuna. A proveniência semeada ignora os bindings e não consulta `CONTA_REATRIBUIDO`. (`bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:274`, `bin/leitores_de_chave.py:286`)

5. **Reatribuição reduz medida apesar de acesso válido anterior:** sintético `a=c.ata; a["k"]; a={}` remove `k`, deixando lacunas. A lacuna é visível no JSON e contada no texto, porém `--contar-crus` retorna número menor com rc 0 sem informar a lacuna. Isso permite falso verde se o número for usado como catraca/completude. (`bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:697`, `bin/leitores_de_chave.py:721`, `bin/leitores_de_chave.py:732`)

6. **Ciclo válido de atribuições perde proveniência:** sintético `a=c.ata; b=a; a=b; a["k"]; b["j"]` termina, mas perde ambos. `busy` devolve conjunto vazio; esse resultado parcial participa do memo e da detecção de reatribuição. (`bin/leitores_de_chave.py:263`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:270`)

Comparação v1 reconstruída em memória versus atual: eventos **de topo** permanecem 22→22 em `cartorio.py`, 20→20 em `leitor_celula.py`, 10→10 em `juizes.py`; nenhum topo entra/sai nesses três arquivos. Unidade/filtro: `bin/leitores_de_chave.py:695`; emissão e deduplicação: `bin/leitores_de_chave.py:347`, `bin/leitores_de_chave.py:361`.

No trecho `_ata_ate_o_teto`, entram eventos aninhados nas linhas `914`, `915` e `917`: leitura de `acesa/tipo/hora`, cópia de `lampadas.[].*`, escritas `lampadas.[].acesa`/`pendente` e leitura final de `acesa`. (`app/ponto/services/cartorio.py:913`, `app/ponto/services/cartorio.py:914`, `app/ponto/services/cartorio.py:915`, `app/ponto/services/cartorio.py:917`)

## 12/11 18:07 · sessao 0f18aeb186a9 · - · - · origem=Ronald (tmux)

| Valor do delta | Quem escreve | Quem lê / efeito |
|---|---|---|
| `ROTULO_CRUS` | Literal em `bin/leitores_de_chave.py:38` | `run` imprime no stderr (`bin/leitores_de_chave.py:698`); h confere apenas substring (`bin/tests/test_leitores_de_chave.sh:157`). |
| `CONTA_REATRIBUIDO=False` | `bin/leitores_de_chave.py:40` | `alias` descarta proveniência ambígua (`bin/leitores_de_chave.py:274`). |
| `NAO_COBERTOS` | `bin/leitores_de_chave.py:41` | JSON (`bin/leitores_de_chave.py:722`) e texto (`bin/leitores_de_chave.py:734`); g confere aviso de `--sem-declaracoes`, não essa enumeração (`bin/tests/test_leitores_de_chave.sh:132`). |
| `env` / bindings usados pelo novo alias | Atribuições indexadas e `bind` (`bin/leitores_de_chave.py:142`, `bin/leitores_de_chave.py:212`) | `alias` resolve cada binding (`bin/leitores_de_chave.py:267`). |
| `memo` | Inicialização (`bin/leitores_de_chave.py:117`), limpeza por rodada/final (`bin/leitores_de_chave.py:163`, `bin/leitores_de_chave.py:175`), preenchimento `(out, reatribuido)` (`bin/leitores_de_chave.py:270`) | Teste de presença e recuperação (`bin/leitores_de_chave.py:265`, `bin/leitores_de_chave.py:271`). |
| `busy` | Inicialização (`bin/leitores_de_chave.py:117`), inclusão/remoção (`bin/leitores_de_chave.py:266`, `bin/leitores_de_chave.py:268`) | Guarda de ciclo retorna conjunto vazio (`bin/leitores_de_chave.py:263`). |
| `seeds` / caminho `<chave>.[]` | PARAMS (`bin/leitores_de_chave.py:138`), atribuição a atributo (`bin/leitores_de_chave.py:155`), `values_list` (`bin/leitores_de_chave.py:246`), novo laço (`bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:171`) | `paths(Name)` dá precedência à seed (`bin/leitores_de_chave.py:286`), alimentando novos eventos de `scan` (`bin/leitores_de_chave.py:447`, `bin/leitores_de_chave.py:482`). |
| `ps`, `atual`, `novo` | `bin/leitores_de_chave.py:164`, `bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:172` | União de caminhos e encerramento das até quatro rodadas (`bin/leitores_de_chave.py:170`, `bin/leitores_de_chave.py:173`, `bin/leitores_de_chave.py:162`). |
| Lacuna `reatribuido:<nome>`, forma `apelido` | `alias` (`bin/leitores_de_chave.py:273`); lacunas preliminares apagadas ao fim da semeadura (`bin/leitores_de_chave.py:176`) | `censo` serializa (`bin/leitores_de_chave.py:639`); JSON publica (`bin/leitores_de_chave.py:721`); texto só totaliza (`bin/leitores_de_chave.py:732`); g lê conjunto caminho/forma (`bin/tests/test_leitores_de_chave.sh:134`). |
| Assinatura `nomes` | Parâmetros e nomes `Store` (`bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`) | Guard PARAMS compara existência do nome (`bin/leitores_de_chave.py:608`). |
| Assinatura `retorna` | Existência de qualquer `Return` com expressão AST (`bin/leitores_de_chave.py:190`) | Guard RETURNS (`bin/leitores_de_chave.py:613`). |
| `defs[f.rel]` | `censo` recebe `f.assinaturas()` (`bin/leitores_de_chave.py:634`) | `run` entrega ao guard (`bin/leitores_de_chave.py:687`), que consulta arquivo/função (`bin/leitores_de_chave.py:603`, `bin/leitores_de_chave.py:612`). |
| `faltam` | Guard acrescenta função/nome/retorno ausente (`bin/leitores_de_chave.py:605`, `bin/leitores_de_chave.py:609`, `bin/leitores_de_chave.py:614`) | `run` emite diagnóstico e rc 2 (`bin/leitores_de_chave.py:688`); f/f2 conferem rc, diagnóstico e stdout vazio (`bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:93`). |
| Número `len(top)` / rc 0 | Seleção produção→chaves literais de topo (`bin/leitores_de_chave.py:693`); impressão e retorno (`bin/leitores_de_chave.py:699`) | Selo real (`bin/tests/test_leitores_de_chave.sh:52`), e (`bin/tests/test_leitores_de_chave.sh:145`), h (`bin/tests/test_leitores_de_chave.sh:157`), DNA (`bin/tests/test_leitores_de_chave.sh:165`). JSON publica o mesmo total (`bin/leitores_de_chave.py:713`). |
| `_tmp` | `mktemp`, agora conferido (`bin/tests/test_leitores_de_chave.sh:27`) | Guarda de diretório (`bin/tests/test_leitores_de_chave.sh:28`), redirecionamentos (`bin/tests/test_leitores_de_chave.sh:35`), cópias/fixtures (`bin/tests/test_leitores_de_chave.sh:57`, `bin/tests/test_leitores_de_chave.sh:97`, `bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:161`), limpeza (`bin/tests/test_leitores_de_chave.sh:29`). |
| `LC` / parâmetro mutado f2 | Caminho (`bin/tests/test_leitores_de_chave.sh:79`), `sed` de renomeação (`bin/tests/test_leitores_de_chave.sh:82`) | Âncora (`bin/tests/test_leitores_de_chave.sh:80`), ferramenta (`bin/tests/test_leitores_de_chave.sh:83`), asserção (`bin/tests/test_leitores_de_chave.sh:84`); restauração (`bin/tests/test_leitores_de_chave.sh:87`). |
| Fixture g: laço, dois saltos, reatribuição | Heredoc (`bin/tests/test_leitores_de_chave.sh:113`) | AST da ferramenta (`bin/tests/test_leitores_de_chave.sh:129`); `G/ESP` com papéis (`bin/tests/test_leitores_de_chave.sh:130`); `A` para lacunas (`bin/tests/test_leitores_de_chave.sh:134`); contador esperado 10 (`bin/tests/test_leitores_de_chave.sh:145`). |
| `Z` / universo zero | Caminho e fonte sem acesso (`bin/tests/test_leitores_de_chave.sh:155`) | Ferramenta (`bin/tests/test_leitores_de_chave.sh:156`); h exige zero, rc 0 e substring (`bin/tests/test_leitores_de_chave.sh:157`). |
| `D`, `N_DNA`, chave mutada | Cópia (`bin/tests/test_leitores_de_chave.sh:161`), captura (`bin/tests/test_leitores_de_chave.sh:165`), mutação literal (`bin/tests/test_leitores_de_chave.sh:171`) | Contagem positiva (`bin/tests/test_leitores_de_chave.sh:167`), controle (`bin/tests/test_leitores_de_chave.sh:168`), mutante (`bin/tests/test_leitores_de_chave.sh:172`), vizinha (`bin/tests/test_leitores_de_chave.sh:175`), mensagem final (`bin/tests/test_leitores_de_chave.sh:184`). |
| stdout, stderr, `rc` entre chamadas | `_lc` sobrescreve os três (`bin/tests/test_leitores_de_chave.sh:35`) | `_jq` lê stdout JSON (`bin/tests/test_leitores_de_chave.sh:38`); asserções leem número, diagnóstico e status (`bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:173`). |

ESMERIL: o número do contador e `resumo_producao.operacoes_topo` compartilham `len(top)`, mas o texto e as listas JSON incluem eventos selecionados com descendentes e `*`; portanto seus tamanhos não são intercambiáveis. (`bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:703`, `bin/leitores_de_chave.py:713`, `bin/leitores_de_chave.py:719`, `bin/leitores_de_chave.py:727`)

A linguagem do selo ainda chama essa medida de “catraca”, enquanto o instrumento declara que teto/catraca não mora ali. O texto também continua descrevendo lacunas apenas como dinâmica/cópia inteira, embora agora inclua `apelido`. (`bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:145`; `bin/leitores_de_chave.py:29`, `bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:732`)

Consumidor executável demonstrado dessas saídas: o selo; entrega efetiva a briefs, construtor, preverificador ou HAIKU: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:38`)

## 12/11 18:06 · sessao 9e5e15709a4a · - · - · origem=Ronald (tmux)

- **f2 morde e não é vazio:** exige âncora real antes de renomear; cobra rc 2, nome declarado e stdout vazio. O guard anterior passaria indevidamente esse caso. (`bin/tests/test_leitores_de_chave.sh:80`, `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:84`; `app/escala/services/leitor_celula.py:180`)
- **h morde a regressão zero→rc 1:** cobra exatamente `0`, rc 0 e a ressalva sobre acessor. Seu universo é vazio por construção; isso não torna o conjunto vacuamente verde, porque a/e/i exigem medidas positivas/exatas. (`bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:54`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:167`)
- **i morde DNA quebrado e desaparecimento:** medida positiva, controle conhecido rc 0, mutação, rc 1 nomeado e vizinha preservada. Não demonstra completude de DNA. (`bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:168`, `bin/tests/test_leitores_de_chave.sh:171`, `bin/tests/test_leitores_de_chave.sh:173`, `bin/tests/test_leitores_de_chave.sh:175`)
- **RETURNS continua sem mutante próprio:** f/f2 mexem exclusivamente em `_orfas_da_ata`, declarada em PARAMS; os sintéticos seguintes desligam declarações. Retirar só a verificação RETURNS pode preservar todas as asserções do selo atual. É lacuna de cobertura da promessa “todos os casos mordem”, sem estabelecer obrigação contratual explícita de um mutante para cada ramo. (`bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:91`, `bin/tests/test_leitores_de_chave.sh:129`, `bin/leitores_de_chave.py:61`, `bin/leitores_de_chave.py:610`, `bin/tests/test_leitores_de_chave.sh:184`)
- **Guarda curada parcialmente:** agora verifica nome declarado e existência de `return <expressão>`, mas nomes locais `Store` satisfazem PARAMS, e `return None`/`return 0` satisfazem RETURNS. Reprodução em memória confirmou essas três situações; a guarda prova presença sintática, sem provar JSON retornado/recebido. (`bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:608`, `bin/leitores_de_chave.py:613`)
- **Achados v1 curados no selo:** papéis de setdefault/pop agora conferidos; rc do contador sintético conferido; DNA exercitado; falha de mktemp aborta antes das gravações. (`bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:131`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:27`)
- **Achados v1 persistentes:** árvore real exige somente presenças mínimas e total positivo; raiz comprova presença, não identidade do hash; não há asserção de teto anterior ou exclusão de acessor. (`bin/tests/test_leitores_de_chave.sh:23`, `bin/tests/test_leitores_de_chave.sh:46`, `bin/tests/test_leitores_de_chave.sh:54`; `bin/leitores_de_chave.py:695`)
- **Semântica ainda divergente:** instrumento declara que teto/catraca não mora nele, mas selo continua nomeando medida “catraca”; texto rotula lacunas apenas como “chave dinamica/copia inteira”, omitindo novo `apelido`. h verifica apenas uma substring do rótulo. (`bin/leitores_de_chave.py:29`; `bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`; `bin/leitores_de_chave.py:732`)
- **Limite novo de g:** lacunas são verificadas por conjunto de caminho/forma, sem posição, função ou multiplicidade. (`bin/tests/test_leitores_de_chave.sh:134`, `bin/tests/test_leitores_de_chave.sh:135`)

## 12/11 17:46 · sessao d118260d8f2a · TRANCA-VIVAS-08 · sonda · origem=Code

O envelope executa a sonda antes do Codex e entrega a saída raspada no prompt ([bin/codex_pergunta.sh:326](bin/codex_pergunta.sh:326), [bin/codex_pergunta.sh:357](bin/codex_pergunta.sh:357)). Segue a transcrição integral recebida; reexecução independente: [nao verificado].

```text
sonda_leitura: sonda_v4.py [b406e34784086e43] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

log#747607 acao=veredito_celula modelo=CelulaDia obj=10153 usuario=657 ator=None em=2026-10-22T15:35:25.946330-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=cobrado chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> cobrado via cartorio'
log#747612 acao=veredito_celula modelo=CelulaDia obj=10153 usuario=657 ator=None em=2026-10-22T15:35:27.552330-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=discordante chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> discordante via cartorio'
log#747617 acao=veredito_celula modelo=CelulaDia obj=10152 usuario=657 ator=None em=2026-10-22T15:35:29.134502-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=cobrado chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> cobrado via cartorio'
log#747622 acao=veredito_celula modelo=CelulaDia obj=10152 usuario=657 ator=None em=2026-10-22T15:35:30.735412-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=discordante chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> discordante via cartorio'
log#747627 acao=veredito_celula modelo=CelulaDia obj=10151 usuario=657 ator=None em=2026-10-22T15:35:32.359774-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=cobrado chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> cobrado via cartorio'
log#769000 acao=veredito_celula modelo=CelulaDia obj=4538 usuario=653 ator=None em=2026-10-26T11:05:49.557493-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=concorde veredito_depois=furo chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito concorde -> furo via cartorio'
log#933557 acao=veredito_celula modelo=CelulaDia obj=10151 usuario=657 ator=None em=2026-11-11T11:48:54.050593-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=furo chaves_depois=['protestos', 'status_dia', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> furo via cartorio'
cel#4538 colab367 data=2026-08-01 veredito=furo via=cartorio veredito_em=2026-10-26T11:05:49.511359-03:00
   logs veredito_celula com usuario: 1; com acoes (servico Resolver dia): 0
cel#10151 colab444 data=2026-08-03 veredito=furo via=cartorio veredito_em=2026-11-11T11:48:53.970330-03:00
   logs veredito_celula com usuario: 2; com acoes (servico Resolver dia): 0
cel#10152 colab444 data=2026-08-04 veredito=furo via=cartorio veredito_em=2026-11-12T06:27:03.004867-03:00
   logs veredito_celula com usuario: 2; com acoes (servico Resolver dia): 0
cel#10153 colab444 data=2026-08-05 veredito=discordante via=cartorio veredito_em=2026-10-22T15:35:27.512146-03:00
   logs veredito_celula com usuario: 2; com acoes (servico Resolver dia): 0
total_erros=0
== anexo: retira_acervo_tranca_2.txt ==
aval Ronald: RETIRA ACERVO-TRANCA-2 (publicado 16:3x, contraria CLAUDE.md 7b DINHEIRO item 3: nenhuma trava por exportada/trancada/paga; correcao provada entra em qualquer competencia). Nao executar os itens 1, 2, 3 e 5; nenhuma lei nova. Fica SO o item 4, que e lei existente (TRANCA-DE-VERDADE): as 4 perguntas vivas na comp 08 emp2 trancada -> listar pk e o degrau de chamados/juizes.py::pergunta_viva que as deixou vivas; curar na origem se for defeito; contador vivas_apesar_da_tranca na frota, esperado 0. Segue a fila; PAREI so em lei ou !
```

1. Pela assinatura recebida, **747607, 747612, 747617, 747622 e 747627** correspondem à trilha da lavra em **`app/ponto/portas/celula.py:1042`**, todos via **`cartorio`**; a descrição coincide com `app/ponto/portas/celula.py:1043`.
2. **769000** corresponde à mesma lavra, via **`cartorio`** (`app/ponto/portas/celula.py:1042`, `app/ponto/portas/celula.py:1043`).
3. **933557** corresponde à mesma lavra, via **`cartorio`**, incluindo `status_dia` no payload (`app/ponto/portas/celula.py:1044`).
4. Nenhum tem a assinatura de “Resolver dia”, que grava `acoes` (`app/ponto/services/veredito_celula.py:356`, `app/ponto/services/veredito_celula.py:358`), nem a descrição exclusiva de alteração de status do ramo `app/ponto/portas/celula.py:1028`.
5. **`_toque_humano_nos_dias` confunde as assinaturas:** usa a ação definida em `app/chamados/juizes.py:1088`, exige usuário não nulo/diferente do sistema (`app/chamados/juizes.py:1253`) e aceita ação+célula+horário, sem conferir `via`, `acoes` ou `ator` (`app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1292`).
6. Esse toque passa por `_admin_tocou_os_dias_depois` (`app/chamados/juizes.py:1225`) e afasta a tranca em `dias_trancados` (`app/chamados/juizes.py:1079`), influenciando o degrau de `pergunta_viva` em `app/chamados/juizes.py:1395`.
7. Outro leitor direto é **`decisao_do_dia`**: restringe previamente a célula à via humana `admin` (`app/ponto/services/veredito_celula.py:76`, `app/ponto/services/veredito_celula.py:110`) e consulta a ação para completar autoria (`app/ponto/services/veredito_celula.py:115`, `app/ponto/services/veredito_celula.py:120`).
8. A lavra pode herdar o usuário autenticado da requisição (`app/core/observ.py:113`); portanto, usuário preenchido não distingue “Resolver dia” de cartório. Identidade do usuário de sistema, horário da tranca, PKs das perguntas e efeito efetivo desses logs na produção: [nao verificado].

## 12/11 17:45 · sessao a73b5ea5ea07 · - · - · origem=Ronald (tmux)

Encontrei dois leitores diretos de `LogAuditoria.acao='veredito_celula'`:

- `_toque_humano_nos_dias`: constante em `app/chamados/juizes.py:1088`; exige usuário não nulo/diferente do sistema em `app/chamados/juizes.py:1253`; consulta ação, modelo e célula em `app/chamados/juizes.py:1287`; marca toque pelo horário em `app/chamados/juizes.py:1292`, sem conferir `acoes`, `via` ou `ator`. Esse toque afasta a tranca em `app/chamados/juizes.py:1079`.
- `decisao_do_dia`: primeiro restringe as células a `veredito_via='admin'` em `app/ponto/services/veredito_celula.py:110`; depois consulta essa ação em `app/ponto/services/veredito_celula.py:115` para obter o autor da última trilha com nome em `app/ponto/services/veredito_celula.py:118`. Consumidores: calendário em `app/colaboradores/services/calendario.py:416` e API em `app/api/views.py:1411`.

Não encontrei outro leitor direto na busca da cópia; exaustividade fora dos arquivos pesquisados: [não verificado].

## 12/11 17:43 · sessao 3d62cbc2dfb9 · TRANCA-VIVAS-08 · sonda · origem=Code

Não houve nova execução: foi fornecida a saída raspada, sem o código de `sonda_v3.py`; execução independente [nao verificado]. Transcrição integral da saída fornecida [nao verificado]:

```text
sonda_leitura: sonda_v3.py [a35d1b44e7abf2fc] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora 2026-11-12T17:42:11.101502-03:00
PF#3 emp2 08/2026 fechado_em=2026-09-16T09:11:43.807307-03:00 reaberto_em=None janela_fechamento=(datetime.date(2026, 7, 21), datetime.date(2026, 8, 20))
#20798 col367 emp2 data_evento=2026-08-01 dia_turno=2026-08-01 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=False resposta_nao_vazia=False materializada=True via=sem_turno_aberto | celula=4538 veredito=furo via_cel=cartorio admin_decidiu=False concorda=n/a | viva=True
   toque cobrar: n=0 []
   toque resolver: n=1 [(769000, 653, '2026-10-26T11:05:49.557493-03:00')]
   toque desarquivar: n=0 []
#47879 col444 emp2 data_evento=2026-08-03 dia_turno=2026-08-03 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=True resposta_nao_vazia=True materializada=False via= | celula=10151 veredito=furo via_cel=cartorio admin_decidiu=False concorda=None | viva=True
   toque cobrar: n=0 []
   toque resolver: n=2 [(747627, 657, '2026-10-22T15:35:32.359774-03:00'), (933557, 657, '2026-11-11T11:48:54.050593-03:00')]
   toque desarquivar: n=0 []
#47881 col444 emp2 data_evento=2026-08-04 dia_turno=2026-08-04 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=True resposta_nao_vazia=True materializada=False via= | celula=10152 veredito=furo via_cel=cartorio admin_decidiu=False concorda=None | viva=True
   toque cobrar: n=0 []
   toque resolver: n=2 [(747617, 657, '2026-10-22T15:35:29.134502-03:00'), (747622, 657, '2026-10-22T15:35:30.735412-03:00')]
   toque desarquivar: n=0 []
#47884 col444 emp2 data_evento=2026-08-05 dia_turno=2026-08-05 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=True resposta_nao_vazia=True materializada=False via= | celula=10153 veredito=discordante via_cel=cartorio admin_decidiu=False concorda=None | viva=True
   toque cobrar: n=0 []
   toque resolver: n=2 [(747607, 657, '2026-10-22T15:35:25.946330-03:00'), (747612, 657, '2026-10-22T15:35:27.552330-03:00')]
   toque desarquivar: n=0 []
total_erros=0
```

- Pelo dado fornecido, nas quatro perguntas a chave `(2,8,2026)`, derivada do fim da janela em `20/08/2026`, encontrou PF#3; portanto a causa não é período ausente. O juiz deriva e consulta essa chave em `app/chamados/juizes.py:1062`–`1068`. Dados de produção [nao verificado].
- **#20798:** chega ao degrau **célula acusadora**, `furo`, e retorna `True` em `app/chamados/juizes.py:1440`–`1441`, apesar do carimbo `sem_turno_aberto`. C20=False por **Resolver dia**, log 769000, usuário 653, em 26/10, depois de `fechado_em` de 16/09; comparação em `app/chamados/juizes.py:1292`, inversão em `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **#47879:** retorna no degrau **resposta preenchida, sem decisão administrativa atual, concordância não comprovada**: `concorda=None` faz `is not True` retornar `True` em `app/chamados/juizes.py:1407`–`1419`. C20=False por **Resolver dia**, logs 747627 e 933557, usuário 657, em 22/10 e 11/11, posteriores ao fechamento; `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **#47881:** mesmo degrau da resposta: `admin_decidiu=False`, `concorda=None`, retorno `True` em `app/chamados/juizes.py:1415`–`1419`. C20=False por **Resolver dia**, logs 747617 e 747622, usuário 657, em 22/10, posteriores ao fechamento; `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **#47884:** mesmo degrau da resposta, antes de examinar o veredito `discordante`: `admin_decidiu=False`, `concorda=None`, retorno `True` em `app/chamados/juizes.py:1415`–`1419`. C20=False por **Resolver dia**, logs 747607 e 747612, usuário 657, em 22/10, posteriores ao fechamento; `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **Resolver dia** corresponde a `veredito_celula` em `app/chamados/juizes.py:1088`; o filtro exige usuário preenchido e diferente do sistema em `app/chamados/juizes.py:1253`–`1254`. O dado fornecido mostra zero ocorrências de cobrar e desarquivar nas quatro perguntas [nao verificado].

## 12/11 17:37 · sessao d45010bd76f3 · E3-LEITORES · censo · origem=Code

**PERGUNTA DE DESENHO — o código tem várias operações que persistem a resposta.** O core humano, a resposta automática, o carimbo pela trilha e `PerguntaDisputa.carimbar` escrevem independentemente. Portanto, o censo não certifica **UMA porta de escrita**. (`app/chamados/services/disputa_emissao.py:1467`, `:1657`, `:1679`; `app/chamados/models.py:3157`)

A função denominada “porta do carimbo de resposta”, `recusa_respondida_sem_texto`, é uma **guarda**, sem persistência: constrói um `SimpleNamespace`, consulta o predicado e pode levantar exceção. (`app/chamados/services/fio.py:1578`, `:1585`, `:1587`)

**HEAD**

Critério: sítio de acesso ao campo, incluindo payloads encaminhados, projeções ORM e acessos dinâmicos; chamadas transitivas não são contadas novamente como acesso direto. A identificação Git do snapshot, execução dos testes e estado efetivo de produção ficam **[nao verificado]**. [nao verificado]

Nas tabelas, **E = `respondida_em`**, **T = `resposta_colab`**. Caminhos sem prefixo são relativos a **`app/chamados/`**. Linhas agrupadas pertencem à mesma função.

**Escritores e enquadramento nas portas**

A porta declarada para resposta web/API é `responder_pergunta_disputa_core`; ela recusa texto vazio e persiste o par. A guarda comum é `recusa_respondida_sem_texto`. São funções diferentes. (`services/disputa_emissao.py:1425`, `:1432`, `:1508`; `services/fio.py:1578`)

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo; porta |
|---|---|---|---|---|
| `models.py:1509:ChamadoColaborador._universo_da_ressuscitacao` | ESCRITOR — memória | `_q.respondida_em = None` | E | Cópia de `p` em `:1495`, vindo de `self.perguntas_que_resolvem` em `:1484`; relação de PD em `:3415`. **Fora do core**; simulação sem salvar, usada pelo juiz em `:1510`. |
| `models.py:1567:ChamadoColaborador.ressuscitar_perguntas` | ESCRITOR — delegação | `descarimbar_em_lote(..., respondida_em=None)` | E | `alvo` da mesma relação, `:1551`. **Fora do core**; passa pelo descarimbo em lote. |
| `models.py:2809:PerguntaDisputa.abrir` | ESCRITOR — encaminhamento possível | `**campos` para `_sob_contrato` | E/T recebidos | Método da classe PD. Caminho de nascimento; **fora do core**, chega à guarda em `:2864`. |
| `models.py:2821:PerguntaDisputa.abrir_conversa` | ESCRITOR — encaminhamento possível | `**campos` para `_sob_contrato` | E/T recebidos | Classe PD; mesmo enquadramento. |
| `models.py:2840:PerguntaDisputa._sob_contrato` | ESCRITOR — encaminhamento possível | `_nascer(**kw)` | E/T recebidos | `cls` da classe PD; chamadas em `:2809`, `:2821`. |
| `models.py:3039,3042:PerguntaDisputa._nascer` | ESCRITOR | `cls(..., **campos)`; `save(force_insert=True)` | E/T recebidos | `cls` de PD. **Fora do core; chama a guarda** em `:2864`, `:2865`. |
| `models.py:3157,3158,3177:PerguntaDisputa.carimbar` | ESCRITOR | atribuição; acrescenta E a `campos`; `save(update_fields=campos)` | E | `self` de PD. **Fora do core e da guarda comum**; exige texto em `:3155`. |
| `models.py:3188:PerguntaDisputa._desfazer_em_memoria` | ESCRITOR — memória | restaura `self.respondida_em` | E | `self` de PD; desfaz `carimbar`, chamadas em `:3166`, `:3169`, `:3180`. **Fora do core**, sem persistência própria. |
| `models.py:3289,3291:PerguntaDisputa.descarimbar_em_lote` | ESCRITOR | `PerguntaDisputa.objects...update(..., **outros)` | E/T recebidos | Modelo explícito. **Fora do core; HEAD sem guarda do par**. Payloads comprovados em `models.py:1567` e `services/disputa_emissao.py:1630`. |
| `models.py:3448:PerguntaDisputa.save` | ESCRITOR — persistência genérica | `super().save(*args, **kwargs)` | E/T conforme save | `self` de PD. Infraestrutura comum dos saves; **não concentra os `QuerySet.update`**, existentes em `:3291` e `services/disputa_emissao.py:1632`. |
| `services/disputa_emissao.py:1467,1468,1469,1508:responder_pergunta_disputa_core` | ESCRITOR | atribuições T/E; `save(update_fields=_campos)` | E/T | PD consultada em `views.py:1239`, passada ao core em `:1283`. **É o core**; exige texto em `:1432`, `:1434`; não chama a guarda comum. |
| `services/disputa_emissao.py:1593,1630,1632:reabrir_para_correcao` | ESCRITOR | `dict(E=None,T='')`; `descarimbar_em_lote(**campos)` ou `qs.update(**campos)` | E/T | Querysets PD fornecidos em `services/validacao.py:593`, `services/materializacao.py:688`. **Fora do core**; porta de reabertura. |
| `services/disputa_emissao.py:1657,1658,1660:responder_automaticamente` | ESCRITOR | atribuições T/E; `save(update_fields=[T,E])` | E/T | Perguntas do upload, chamada em `app/api/foto_recebida.py:48`. **Fora do core; chama a guarda** em `:1655`. |
| `services/disputa_emissao.py:1679,1681:carimbar_resposta_pela_trilha` | ESCRITOR | atribuição E; `save(update_fields=[E])` | E | PD consultadas pelo comando em `management/commands/curar_perguntas_em_limbo.py:55`, chamada em `:67`, `:68`. **Fora do core e da guarda comum**; guarda própria em `:1675`. |
| `bin/sombra_mascara_pessoal.py:57:<módulo>` | ESCRITOR — sombra | `setattr(o,c,novo)` | T | `apps.get_model(app,mod)` em `:46`; configuração identifica PD/T em `bin/sombra_regras_pessoais.py:85`. **Fora das portas**. |
| `bin/sombra_mascara_pessoal.py:62,64:<módulo>` | ESCRITOR — sombra | `bulk_update(lote,list(campos))` | T | Mesma resolução de modelo/campo. **Fora das portas**; o script exige banco `sombra` em `:10`. |

O nascimento aceita o par por `**campos`, independentemente de haver chamada atual passando essas chaves; sua aceitação e persistência estão no construtor e no save. (`models.py:2864`, `:3039`, `:3042`) Chamador de produção passando explicitamente o par ao nascimento: **[nao verificado]**.

**Leitores no modelo e nos juízes**

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `models.py:1487:ChamadoColaborador._universo_da_ressuscitacao` | LEITOR | `Q(T__isnull=True)`, `Q(T='')` | T | Relação `perguntas_que_resolvem`, `:1484`; FK PD em `:3415`. |
| `models.py:1565:ChamadoColaborador.ressuscitar_perguntas` | LEITOR | `only(E,T)` | E/T | Mesmo queryset relacionado. |
| `models.py:1849,1850:DisputaSupervisao.respostas_sem_veredito` | LEITOR | atributos | E/T | `self.perguntas` em `:1847`; relação PD em `:2436`, `:2439`. |
| `models.py:1933:DisputaSupervisao.fechar` | LEITOR | atributo T para trilha | T | `self.perguntas` → `vivas`, `:1902`, `:1903`. |
| `models.py:2065:DisputaSupervisao.remover_motivo` | LEITOR | atributo T para trilha | T | Iteração de `self.perguntas.filter`, `:2061`. |
| `models.py:2864,2865:PerguntaDisputa._nascer` | LEITOR — payload | `campos.get(E/T)` | E/T | Método PD; valores destinados ao construtor em `:3039`. |
| `models.py:3070,3073:PerguntaDisputa.estado` | LEITOR | comparação de atributo | E | `self` da classe PD. |
| `models.py:3152,3155,3156:PerguntaDisputa.carimbar` | LEITOR | foto anterior; teste de texto; comparação | E/T | `self` da classe PD. | [nao verificado]
| `models.py:3373,3374:PerguntaDisputa.q_vivas` | LEITOR | `Q(E__isnull=False)`, exclusão de T vazio/nulo | E/T | Método da classe PD. |
| `models.py:3523:PerguntaDisputa.respondida` | LEITOR | atributo `is not None` | E | `self` da classe PD. |
| `models.py:3546,3551,3555,3559,3563,3566,3567,3568:PerguntaDisputa.resposta_display` | LEITOR | atributos; conversão; comparação; retorno | T | `self` da classe PD. |
| `models.py:3949,3956,3957,3958,3959,3963:DecisaoPergunta.linhas_da_projecao_sem_linha` | LEITOR | atributos para projeção da trilha | E/T | Recebe PD em chamadas como `models.py:1564` e `services/disputa_emissao.py:1466`. |
| `juizes.py:348,360:_slot_esperado` | LEITOR | atributos | E/T | Pergunta da cadeia do C6/materialização; chamada em `:752`. |
| `juizes.py:852,853:_chave_memo_pv` | LEITOR | `getattr` literal | E/T | Chamado por `pergunta_viva`, `:890`, `:902`; admite também sondadores, conforme `:844`. |
| `juizes.py:1346:_resposta_concorda_com_a_luz` | LEITOR | atributo | T | Recebe a pergunta do C6 em `:1419`. |
| `juizes.py:1375:projecao_temporal_da_pergunta` | LEITOR | atributos | E/T | Recebe a pergunta do C6 em `:1390`. |
| `juizes.py:1518:respondida_sem_texto` | LEITOR | atributos; `texto_vazio(T)` | E/T | Predicado aplicado às perguntas, inclusive em `models.py:1851`; também recebe sonda em `services/fio.py:1585`. |
| `juizes.py:1665:perguntas_corrigiveis` | LEITOR | filtro ORM | E | `PerguntaDisputa.objects` explícito. |
| `juizes.py:1672,1673,1675:perguntas_respondidas_a_validar` | LEITOR | filtro, exclusão, `order_by(E)` | E/T | `PerguntaDisputa.objects` explícito. |
| `juizes.py:1701:declarou_hora` | LEITOR | atributo | T | Perguntas da materialização, chamada em `services/materializacao.py:555`. |
| `juizes.py:1848:delta_da_resposta` | LEITOR | atributo | T | Pergunta da validação, chamada em `services/validacao.py:864`. |
| `juizes.py:1952,1957:classificar_resposta_validacao` | LEITOR | atributo | T | Fila PD em `app/api/views_mensageria.py:540`, `:544`. |
| `juizes.py:1974:motivo_implausivel_validacao` | LEITOR | atributo | T | Pergunta carregada em `services/validacao.py:389`, chamada em `:404`. |
| `juizes.py:2022,2026:irmas_respondidas_sem_consumo` | LEITOR | filtro ORM; atributo | E/T | `PerguntaDisputa.objects`, `:2020`. |

**Demais leitores de `app/chamados/`**

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `relatorios_disputa.py:21:classificar` | LEITOR | atributo `is None` | E | `disputa.perguntas.all()` em `:12`. |
| `views.py:1280:responder_pergunta_disputa` | LEITOR | atributo `is not None` | E | Consulta PD em `:1239`. |
| `views.py:1416:reabrir_disputa_supervisao` | LEITOR | filtro ORM | E | `PerguntaDisputa.objects` explícito. |
| `views_cobrar.py:169,176:validar_inline` | LEITOR | atributo; filtro ORM | E | `_vivas_de(d)` e consulta PD em `:175`. |
| `views_cobrar.py:291:proposta_lote` | LEITOR | `x['pergunta'].T` | T | Itens classificados pela fila, `services/validacao.py:921`, `:924`. |
| `canal.py:65:perguntas_mudas_com_resposta` | LEITOR | filtro ORM | T | Consulta PD em `:58`, `:64`. |
| `reconciliador.py:365:lavrar_sem_turno_aberto` | LEITOR | atributo | T | Pergunta recebida pela cadeia de reatar, chamada em `:1620`. |
| `reconciliador.py:1083:veredito_orfa` | LEITOR | atributo | T | Universo PD em `:1048`, `:1049`; chamada em `:1136`. |
| `veredito_lavrado.py:207,208:marcos_do_lote` | LEITOR | filtro E; `**_ate(E)`; `Max(E)` | E | Alias `_PD` em `:201`; `_ate` gera limite em `:163`. |
| `services/acoes_disputa.py:95:acao_reabrir_disputa` | LEITOR | filtro ORM | E | Consulta PD explícita. |
| `services/acoes_disputa.py:130:acao_regularizar_disputa` | LEITOR | atributo | E | Perguntas de `_vivas_de(d)`. |
| `services/acoes_disputa.py:353:acao_corrigir_resposta` | LEITOR | atributo | T | Consulta PD em `:341`. |
| `services/becos.py:30:classificar` | LEITOR | atributo | E | `d.perguntas.all()` em `:29`. |
| `services/cobranca.py:229:_calados` | LEITOR | filtro ORM | E | `PerguntaDisputa.objects` em `:228`. |
| `services/declaracao_texto.py:417,419:declarar_hora_colab` | LEITOR | atributos; `bool(E)` | E/T | Retorno de `PerguntaDisputa.abrir`, `:376`. |
| `services/declaracao_texto.py:687:aplicar_declaracao` | LEITOR | atributo | E | Retorno de `PerguntaDisputa.abrir`, `:666`. |
| `services/disputa_emissao.py:1619,1627:reabrir_para_correcao` | LEITOR | `values_list(T,E)`; nomes `_txt`, `_resp` | E/T | Queryset de perguntas; usa portas PD em `:1609`, `:1630`. |
| `services/disputa_emissao.py:1647:responder_automaticamente` | LEITOR | atributo | T | Perguntas do upload, `app/api/foto_recebida.py:48`. |
| `services/disputa_emissao.py:1675,1683:carimbar_resposta_pela_trilha` | LEITOR | atributos | E/T | Queryset PD do comando, `management/commands/curar_perguntas_em_limbo.py:55`, `:68`. |
| `services/disputa_emissao.py:1718:recobrar_questionario` | LEITOR | filtro ORM | E | Consulta PD em `:1717`. |
| `services/disputa_emissao.py:1826,1844:reabrir_credor_de_pergunta_viva` | LEITOR | atributos | E/T | Relação `perguntas_que_resolvem`, `:1803`, `:1805`. |
| `services/disputa_emissao.py:1984,2057,2062,2074:_resolver_perguntas_por_chamado_resolvido` | LEITOR | atributos; serialização | E/T | Perguntas do chamado tratadas pela função, `:1984`. |
| `services/explicador.py:32:_explicar_validada` | LEITOR | atributo | T | Chamado por `explicar_pergunta`, `:97`; consulta PD em `:122`. |
| `services/explicador.py:79,80:explicar_pergunta` | LEITOR | atributos em textos | T | Consulta PD em `:122`, aplicação em `:133`. |
| `services/fio.py:174:aguarda_o_colaborador` | LEITOR | atributo; predicado do par | E | Coleção PD do fio, carregada em `:242`, encaminhada em `:361`. |
| `services/fio.py:681:processar_post_fio` | LEITOR | atributo | E | `perguntas_vivas_de`, importada em `:680`. |
| `services/fio.py:1254:canonica_do_grupo` | LEITOR | atributo | T | Grupo PD fornecido, por exemplo, por `management/commands/dedup_perguntas_fato.py:74`, `:76`. |
| `services/fio.py:1266:merito_perdedor` | LEITOR | atributo | T | Perguntas do mesmo grupo; constantes PD em `:1265`. |
| `services/foto_e4b.py:180:nascimentos` | LEITOR | `a[c]`, `d[c]`, com `c=E` | E | Ramo `perguntas:` em `:179`; foto PD em `:272`. |
| `services/foto_e4b.py:251:capturar.guardar` | LEITOR | `f.value_from_object(obj)` para todos os campos concretos | E/T | `guardar('perguntas', PerguntaDisputa.objects...)` em `:272`. |
| `services/historico.py:99,105,109:_eventos_dos_chamados` | LEITOR | `getattr(p,campo)` variável; atributo T | E/T | Queryset PD em `:96`; E integra a tupla em `:99`. |
| `services/materializacao.py:280,308:_tipo_pelo_marco` | LEITOR | atributos | T | Cadeia da materialização PD; queryset em `:1375` e carregamento em `services/validacao.py:389`. |
| `services/materializacao.py:359,405,412:_ts_tipo_do_plantio` | LEITOR | atributos | T | Mesma cadeia da materialização. |
| `services/materializacao.py:438:estado_aplicacao_pergunta` | LEITOR | atributo | T | Perguntas carregadas pelo fio em `services/fio.py:242`, chamada em `:357`. |
| `services/materializacao.py:474:_declarar_ausencia_de_pergunta` | LEITOR | atributo | T | Cadeia da materialização PD. |
| `services/materializacao.py:571:_materializar_ausencia_de_resposta` | LEITOR | atributo de `q` | T | `disputa.perguntas.filter`, `:538`. |
| `services/materializacao.py:633:perguntas_absorvidas_por` | LEITOR | filtro `T__in` | T | Base PD em `:629`. |
| `services/materializacao.py:674,675:devolver_absorvidas_da_ausencia_rejeitada` | LEITOR | atributos; foto em dict | E/T | `perguntas_absorvidas_por`, `:671`. |
| `services/materializacao.py:835,876,977:_materializar_resposta_hora` | LEITOR | atributos | T | Cadeia da materialização PD. |
| `services/materializacao.py:1001,1010,1062:_materializar_resposta_data_hora` | LEITOR | atributos | T | Cadeia da materialização PD. |
| `services/materializacao.py:1376:materializar_perguntas_validadas_da_disputa` | LEITOR | filtro ORM | E | `disputa.perguntas.filter`, `:1375`. |
| `services/painel.py:67:marcador_do_placar` | LEITOR | `Max(E)` | E | Agregação PD em `:66`. |
| `services/passivo_validado.py:83:_reversao_de` | LEITOR | `getattr(p,c,None)` | E | `CAMPOS_REVERSAO` em `:64`, `:66`; pergunta recebida em `:367`. |
| `services/pergunta_no_app.py:71:consulta_do_app` | LEITOR | atributo | E | `perguntas_do_app`, `:70`; consulta PD em `:49`. |
| `services/raiox.py:138,143,144:creditos_perguntas` | LEITOR | `values_list(E,T)`; índices `p[3]`, `p[4]` | E/T | `_pq=PerguntaDisputa.objects`, `:56`; seleção em `:135`, `:138`. |
| `services/validacao.py:141:_carimbar_validacao` | LEITOR | atributo | T | Pergunta carregada em `:389`. |
| `services/validacao.py:189,207:devolver_validacao_recusada` | LEITOR | `refresh_from_db(fields=[T,...])`; atributo | T | Instância carregada em `:389`, encaminhada em `:464`. |
| `services/validacao.py:236:_e_aceite_de_folga` | LEITOR | atributo | T | Helper da validação PD. |
| `services/validacao.py:393:validar_pergunta` | LEITOR | atributo | E | Consulta PD em `:389`. |
| `services/validacao.py:576,580,581,600:recusar_pergunta` | LEITOR | atributos; leitura `antes[T]` | E/T | Consulta PD em `:569`, `:570`. |
| `services/validacao.py:851,853,855,860,861,862,863,866:classe_do_lote` | LEITOR | atributos | T | Perguntas da fila, chamadas em `:921`, `:922`. |
| `services/validacao.py:924:fila_validar_classificada` | LEITOR | atributo; idade da resposta | E | Fila padrão em `:918`, `:921`; argumento alternativo `qs` sem validação de tipo. |
| `services/verbos.py:335,336,342,345:placar_aceite` | LEITOR | filtro; `values(E)`; `pg[E]` | E | Consulta PD em `:333`. |
| `services/vigia_fio.py:681:censo_alcance` | LEITOR | atributo | E | Universo PD em `:615`. |
| `services/vigia_fio.py:789,792:respondida_sem_resposta` | LEITOR | filtro E; `values_list(T)` | E/T | Consulta PD explícita em `:789`. |
| `templatetags/chamados_tags.py:142:leitura_da_resposta` | LEITOR | `getattr(perg,T,'')` | T | Recebe `perg` dos templates: `modal_fio.html:250`, `_pergunta_card.html:43`. |

**Leitores nos management commands**

Todos os caminhos desta tabela ficam em `app/chamados/management/commands/`.

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `curar_perguntas_em_limbo.py:32:nota_da_resposta` | LEITOR | atributo | T | Perguntas PD selecionadas em `:46`. |
| `curar_perguntas_em_limbo.py:46,69:Command.handle` | LEITOR | filtros ORM | E/T | `PerguntaDisputa.objects` explícito. |
| `validar_classe_a.py:80:Command.handle` | LEITOR | atributo | T | `d.respostas_sem_veredito()` em `:54`. |
| `auditar_invariantes_chamados.py:177:_contraprova_i21a` | LEITOR | atributo | E | Subconjunto PD de `:1572`, encaminhado em `:1585`. |
| `auditar_invariantes_chamados.py:592,1105,1106,1572,1575,1603,1655:Command.handle` | LEITOR | atributos; filtros ORM | E/T | Alias `PD.objects`; em `:1655`, relação de perguntas carregada em `:1652`. |
| `backfill_decisoes_pergunta.py:56,60,61:linhas_a_criar` | LEITOR | filtro; `only(E,T)` | E/T | Consulta PD explícita. |
| `backfill_decisoes_pergunta.py:326:aplicar` | LEITOR | atributo para trilha | T | Perguntas do backfill. |
| `backfill_decisoes_pergunta.py:504:Command.handle` | LEITOR | `only(T)` | T | Consulta PD explícita. |
| `dedup_perguntas_fato.py:75:Command.handle` | LEITOR | atributo | T | Queryset PD em `:74`. |

**Leitores fora de `app/chamados/`**

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `bin/censo_folha.py:28,29:<módulo>` | LEITOR | filtros ORM | E | `PerguntaDisputa.objects`; import em `:12`. |
| `app/folha/previa.py:142:_material` | LEITOR | atributo | T | Mapa PD em `:64`, `:67`, encaminhado em `:95`. |
| `app/ponto/supra_juiz.py:545,548:pode_executar_csf` | LEITOR | atributos | E | Relações `d.perguntas` e `ch.perguntas_que_resolvem`, `:543`, `:548`; FKs em `app/chamados/models.py:2439`, `:3415`. |
| `app/api/views.py:1159,1161:api_chamados_v2` | LEITOR | atributos; guarda e serialização E | E/T | Perguntas de `consulta`, mapa em `:1029`, `:1035`; fonte PD em `services/pergunta_no_app.py:49`. |
| `app/api/views.py:2557,2559:api_disputa_perguntas._item` | LEITOR | atributos; guarda e serialização E | E/T | `consulta_do_app` em `:2532`, `:2535`; chamadas em `:2562`, `:2572`. |
| `app/api/views.py:2573:api_disputa_perguntas` | LEITOR | atributo | E | `consulta['historico']`; mesma origem. |
| `app/api/views.py:2616,2633:api_disputa_responder` | LEITOR | comparação; serialização | E | Consulta PD em `:2607`, `:2609`. |
| `app/api/views_mensageria.py:567,569,570:_item_validar` | LEITOR | atributos; serialização; diferença de datas | E/T | Fila em `:540`, `:546`; consulta PD em `app/chamados/juizes.py:1672`. |
| `app/colaboradores/services/calendario.py:474:contexto_calendario` | LEITOR | `exclude(T='')` | T | Alias PD em `:469`; queryset em `:473`. |
| `app/colaboradores/services/ficha.

[truncado em 20000 de 31100 caracteres]

## 12/11 17:33 · sessao aa3d7d6986fe · TIPO-NA-FONTE · verificador · origem=Code

**Não aprovo como cumprimento integral.** O instrumento entrega listagens e falhas explícitas, mas não garante “todo leitor e escritor”: há perdas silenciosas de proveniência e a medida chamada `acessos_crus_ata` ainda não significa “fora do acessor”. O selo tem mordida parcial e admite ferramentas quebradas em aspectos contratados. (`bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:224`, `bin/leitores_de_chave.py:538`, `bin/leitores_de_chave.py:627`; `bin/tests/test_leitores_de_chave.sh:100`)

As referências a `aval.txt` e `contrato.txt` abaixo são aos anexos fornecidos, contando o título como linha 1. Execução histórica do selo, publicação prévia do contrato e pouso: **[não verificado]**. [nao verificado]

**1. As cinco linhas do contrato têm atendimento parcial.**

| Linha | Onde está e onde há furo |
|---|---|
| **Quem chama** | O contrato determina brief com saída anexa. O chamador executável demonstrado é o selo; o juiz existente do CENSO aceita referências textuais, sem conferir saída deste instrumento. Automatização para todo brief que toca ata/dna: **[não verificado]**. (`contrato.txt:2`, anexo; `bin/tests/test_leitores_de_chave.sh:26`; `bin/disjuntor.py:1565`) |
| **Quem espera** | Texto e JSON entregam arquivo, linha, função e papéis. Entrega efetiva ao construtor/preverificador e espera condicionada ao resultado: **[não verificado]**. (`contrato.txt:3`, anexo; `bin/leitores_de_chave.py:291`, `bin/leitores_de_chave.py:663`) |
| **Falha/limite** | Há universo Python, separação produção/testes, declaração de HTML/JS não cobertos, rc 1 nomeando chave sem resultado e rc 2 por erro de parse. O furo é tratar essas garantias como completude do censo: várias perdas de proveniência não geram erro nem lacuna. (`bin/leitores_de_chave.py:34`, `bin/leitores_de_chave.py:224`, `bin/leitores_de_chave.py:555`, `bin/leitores_de_chave.py:569`, `bin/leitores_de_chave.py:617`, `bin/leitores_de_chave.py:669`) |
| **Medida** | Existem resumo por chave e `--contar-crus`. Publicação do censo das 72 chaves no RELATO: **[não verificado]**. O contador não exclui acessor nem compara com teto anterior; portanto fornece uma medida, ainda não a catraca contratada. (`contrato.txt:5`, anexo; `bin/leitores_de_chave.py:627`, `bin/leitores_de_chave.py:631`, `bin/leitores_de_chave.py:648`) |
| **Quem desliga / selo** | Ronald está declarado como autoridade no contrato. O mutante de chave desaparecida tem controle antes e vizinha preservada depois; não é vazio nesse aspecto. A mordida não cobre toda a semântica prometida. (`contrato.txt:6`, anexo; `bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:60`, `bin/tests/test_leitores_de_chave.sh:62`, `bin/tests/test_leitores_de_chave.sh:100`) |

O conteúdo dos dois arquivos permanece no instrumento e no selo de host: não implementa acessor, migração de `orfas` ou persistência de instantes. Os modos adicionais servem à inspeção e à medida; chamá-los de catraca pronta antecipa uma garantia ausente. (`bin/leitores_de_chave.py:5`, `bin/leitores_de_chave.py:24`, `bin/leitores_de_chave.py:552`, `bin/leitores_de_chave.py:631`; `bin/tests/test_leitores_de_chave.sh:4`; `aval.txt:5`, anexo)

**2. O AST reconhece candidatos sintáticos; não prova `CelulaDia`.**

Não há regra global “variável chamada `ata` é ata”. Há, porém, dependência de **nomes declarados** por arquivo e função em `PARAMS`, e de nomes de funções em `RETURNS`. Além disso, qualquer atributo `.ata`/`.dna`, qualquer `getattr` com esse literal e qualquer argumento nomeado `ata=`/`dna=` com dicionário são aceitos sem prova da classe ou do destinatário. (`bin/leitores_de_chave.py:39`, `bin/leitores_de_chave.py:66`, `bin/leitores_de_chave.py:128`, `bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:249`, `bin/leitores_de_chave.py:493`)

| Forma | Resultado e risco |
|---|---|
| `a=c.ata; a["orfas"]` | Reconhecida por um salto. (`bin/leitores_de_chave.py:166`, `bin/leitores_de_chave.py:221`) |
| `a=c.ata; b=a; b["orfas"]` | Perde o acesso no caso comum: o limite de profundidade interrompe o segundo salto. Ocorrência real de raiz perdida exclusivamente por isso: **[não verificado]**. (`bin/leitores_de_chave.py:224`) |
| `a=b=c.ata` | Reconhece ambos: cada alvo recebe diretamente o mesmo valor. Não equivale ao caso anterior. (`bin/leitores_de_chave.py:134`, `bin/leitores_de_chave.py:138`) |
| Comprehension | Acesso direto à origem é visto; `values_list` recebe tratamento específico. Um caso sintético como `[a["orfas"] for a in [c.ata]]` perde a ligação do elemento. (`bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:203`, `bin/leitores_de_chave.py:218`) |
| `getattr(c,"ata")["orfas"]` | Reconhecido. Campo obtido de variável ou chamada indireta `getattr(c.ata,"get")("orfas")` não recebe a mesma resolução. (`bin/leitores_de_chave.py:249`, `bin/leitores_de_chave.py:256`) |
| Reatribuição / ordem | O ambiente agrega atribuições da função inteira, sem invalidar proveniência. Em exemplo sintético `a=c.ata; a={...}; a["orfas"]`, o acesso continua atribuído à ata; uma atribuição posterior também pode contaminar acesso anterior. (`bin/leitores_de_chave.py:127`, `bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:227`) |
| Outro objeto com `.ata` | Entra como candidato. Já outro objeto apenas com `.orfas` não entra por esse motivo. (`bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:377`) |

**Sítios reais incluídos:** `getattr` em `app/chamados/juizes.py:966`, `app/chamados/reconciliador.py:701` e `app/ponto/services/lastro.py:89`, `app/ponto/services/lastro.py:113`, `app/ponto/services/lastro.py:128`, `app/ponto/services/lastro.py:149`; comprehensions em `app/ponto/juiz_batida.py:161` e `app/relatorios/cartao_pela_celula.py:288`; desempacotamento de `values_list` em `app/ponto/services/vigia_pareador_celula.py:71`, com leituras em `app/ponto/services/vigia_pareador_celula.py:74` e `app/ponto/services/vigia_pareador_celula.py:78`.

**Sítios corretamente excluídos:** a variável chamada `ata` em `app/relatorios/furos.py:132` é mapa de dias, consumido em `app/relatorios/furos.py:134`. As leituras de `orfas` em dias da grade ficam fora em `app/escala/services/furos_diarios.py:22`, `app/folha/causas.py:43`, `app/folha/causas.py:100`, `app/ponto/services/bordas_realizado.py:49`, `app/relatorios/pdf_espelho.py:477` e `app/relatorios/pdf_espelho.py:552`. `FuroDiario.orfas` é um inteiro próprio, não a chave do JSON. (`app/escala/models.py:1883`)

**Perdas reais:** a leitura de `ata["lampadas"]` é vista, mas o laço não transmite proveniência a `lamp`; as escritas de `acesa`, `luz` e `dispensada` somem do censo dessas subchaves. Também ficam fora consultas ORM, conforme limite declarado. (`app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`; `app/escala/management/commands/celulas_contra_o_template.py:49`; `app/chamados/models.py:3356`; `bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:34`)

**O 95 depende das declarações.** Na reprodução em memória, o total foi 95; sem apenas `PARAMS`, 73; sem `PARAMS` e `RETURNS`, 62. A fórmula conta eventos de topo reconhecidos pelos mapas e pelo rastreamento. (`bin/leitores_de_chave.py:39`, `bin/leitores_de_chave.py:66`, `bin/leitores_de_chave.py:627`)

As 33 operações que desaparecem desligando ambos os mapas estão nestes sítios; multiplicidade indica mais de um evento na mesma linha:

- `app/chamados/juizes.py:438` ×2.
- `app/escala/services/leitor_celula.py:68`, `app/escala/services/leitor_celula.py:82`, `app/escala/services/leitor_celula.py:91`, `app/escala/services/leitor_celula.py:103`, `app/escala/services/leitor_celula.py:105` ×2, `app/escala/services/leitor_celula.py:115`, `app/escala/services/leitor_celula.py:175`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:190`.
- `app/ponto/services/cartorio.py:424`, `app/ponto/services/cartorio.py:426`, `app/ponto/services/cartorio.py:738` ×2, `app/ponto/services/cartorio.py:739`, `app/ponto/services/cartorio.py:740`, `app/ponto/services/cartorio.py:741`, `app/ponto/services/cartorio.py:742`, `app/ponto/services/cartorio.py:745`, `app/ponto/services/cartorio.py:746`, `app/ponto/services/cartorio.py:762`, `app/ponto/services/cartorio.py:767`, `app/ponto/services/cartorio.py:771`, `app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:812`, `app/ponto/services/cartorio.py:834`, `app/ponto/services/cartorio.py:839`, `app/ponto/services/cartorio.py:913`, `app/ponto/services/cartorio.py:917` ×2.
- `app/relatorios/dia_da_frota.py:21`.

Há uma perda silenciosa concreta por nome declarado: renomear somente o parâmetro `ata` e seus usos em `_orfas_da_ata`, mantendo a função, elimina duas operações e leva 95 a 93. O guard verifica a função, não a existência do parâmetro declarado. (`app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:190`; `bin/leitores_de_chave.py:128`, `bin/leitores_de_chave.py:541`)

**3. O selo morde alguns defeitos, mas não sustenta “todos os casos mordem”.**

| Caso | Mordida / vacuidade |
|---|---|
| **a — árvore real** | Exige presença mínima de leitor/escritor nos arquivos esperados e número positivo. Não verifica completude nem valor/delta da medida. (`bin/tests/test_leitores_de_chave.sh:39`, `bin/tests/test_leitores_de_chave.sh:44`) |
| **b — mutante** | Não vazio para desaparecimento: exige controle rc 0, mutante rc 1 nomeado e vizinha rc 0. Uma ferramenta sempre “sem resultado” reprova. (`bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:60`, `bin/tests/test_leitores_de_chave.sh:62`) |
| **c — inexistente** | Exige rc 1 nomeado; isoladamente seria vazio para ferramenta sempre negativa, mas b fornece controle positivo. (`bin/tests/test_leitores_de_chave.sh:67`, `bin/tests/test_leitores_de_chave.sh:55`) |
| **d — parse** | Introduz arquivo inválido depois do universo válido e exige rc 2, arquivo nomeado e stdout vazio. (`bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:116`, `bin/tests/test_leitores_de_chave.sh:118`) |
| **e — testes** | Confere separação e chave exclusiva de teste. A asserção do contador aceita stdout `8` sem verificar rc. (`bin/tests/test_leitores_de_chave.sh:106`, `bin/tests/test_leitores_de_chave.sh:110`, `bin/tests/test_leitores_de_chave.sh:113`) |
| **f — declaração** | Morde função removida/renomeada; não morde parâmetro renomeado com função preservada. (`bin/tests/test_leitores_de_chave.sh:72`; `bin/leitores_de_chave.py:541`) |
| **g — formas** | Confere exatamente caminhos e formas, mas ignora papéis. Retirar ESCRITOR de `setdefault` e ESCRITOR/EXCLUI de `pop` preserva a comparação. (`bin/tests/test_leitores_de_chave.sh:100`; `bin/leitores_de_chave.py:427`, `bin/leitores_de_chave.py:429`) |
| **uso** | Exige rc 64 por argumento faltando. (`bin/tests/test_leitores_de_chave.sh:122`) |

Todas as invocações do selo usam `CelulaDia.ata`; uma implementação de DNA quebrada pode passar. (`bin/tests/test_leitores_de_chave.sh:36`, `bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:122`)

A raiz é calculada por `BASH_SOURCE/../..` e três arquivos são conferidos **antes** de `mktemp`. Isso permite cwd externo; transportar o script para fora da posição `bin/tests` altera a raiz calculada. A prova é de presença, não de identidade do hash. (`bin/tests/test_leitores_de_chave.sh:16`, `bin/tests/test_leitores_de_chave.sh:18`, `bin/tests/test_leitores_de_chave.sh:22`)

No caminho normal, gravações ficam sob `$_tmp`. **Se `mktemp` falhar**, seu retorno não é conferido e `_tmp=''` produz tentativas em `/out`, `/err`, `/copia` e `/sint`. Portanto não há confinamento garantido em todos os caminhos. (`bin/tests/test_leitores_de_chave.sh:15`, `bin/tests/test_leitores_de_chave.sh:22`, `bin/tests/test_leitores_de_chave.sh:28`, `bin/tests/test_leitores_de_chave.sh:50`, `bin/tests/test_leitores_de_chave.sh:78`)

**4. As três saídas não oferecem números intercambiáveis.**

| Saída | Escritor, leitor demonstrado e significado |
|---|---|
| **Texto** | `run()` escreve eventos selecionados por seção; inclui `*` e descendentes compatíveis. O selo usa rc/stderr nas consultas textuais. (`bin/leitores_de_chave.py:635`, `bin/leitores_de_chave.py:659`; `bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:60`) |
| **JSON** | `run()` escreve listas filtradas, mas o resumo é global, mesmo consultando uma chave. O selo lê sítios, formas e separação produção/testes. (`bin/leitores_de_chave.py:645`, `bin/leitores_de_chave.py:652`; `bin/tests/test_leitores_de_chave.sh:30`, `bin/tests/test_leitores_de_chave.sh:39`, `bin/tests/test_leitores_de_chave.sh:100`) |
| **`--contar-crus`** | Imprime eventos de produção cujo caminho é exatamente uma chave literal de topo; exclui `*` e caminhos dinâmicos. O selo lê o número real e o sintético. (`bin/leitores_de_chave.py:627`, `bin/leitores_de_chave.py:631`; `bin/tests/test_leitores_de_chave.sh:44`, `bin/tests/test_leitores_de_chave.sh:112`) |

A unidade efetiva é **evento emitido e deduplicado**, com posição, caminho, papéis e forma; não linha nem arquivo. Uma operação com dois papéis continua um evento, portanto somar leitores e escritores pode contar duas vezes. (`bin/leitores_de_chave.py:288`, `bin/leitores_de_chave.py:302`, `bin/leitores_de_chave.py:385`, `bin/leitores_de_chave.py:424`)

Também entram escritas apenas em memória, como `_cj.ata`; o contador não mede exclusivamente persistência. (`app/escala/utils.py:1886`, `app/escala/utils.py:1887`, `app/escala/utils.py:1895`)

Os principais conflitos futuros são: nenhum filtro “fora do acessor”; nenhuma comparação com teto; e total zero retorna rc 1. Assim, a meta zero poderá produzir erro, enquanto acessos internos ao acessor continuarão contados. (`bin/leitores_de_chave.py:627`, `bin/leitores_de_chave.py:633`; `aval.txt:5`, anexo)

Não há evidência suficiente para atribuir a briefs futuros ou à linha HAIKU uma suposição de unidade “linha” ou “arquivo”: **[não verificado]**. Tampouco é legítimo interpretar 174→95 como redução: o aval descreve grep em outro HEAD, e o instrumento define outra seleção por AST. (`aval.txt:1`, anexo; `bin/leitores_de_chave.py:24`)

**5. Classes que já reprovaram obra.**

| Classe | Situação neste recorte |
|---|---|
| **Segundo juiz** | **Não demonstrado como juiz de negócio.** Há classificador sintático sem prova de classe, insuficiente como veredito completo sobre proveniência. (`bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:249`) |
| **Segundo escritor** | **Não no instrumento para dados da aplicação:** lê fonte e emite saída. O selo escreve fixtures/cópias, com o furo de `mktemp` descrito. (`bin/leitores_de_chave.py:563`, `bin/leitores_de_chave.py:643`; `bin/tests/test_leitores_de_chave.sh:22`, `bin/tests/test_leitores_de_chave.sh:58`) |
| **Relógio fora da borda** | **Não demonstrado no julgamento.** `date` mede apenas duração do selo. (`bin/tests/test_leitores_de_chave.sh:33`, `bin/tests/test_leitores_de_chave.sh:125`) |
| **Derivação paralela de fonte única** | **Presente na proveniência manual:** `PARAMS/RETURNS` precisam acompanhar o código; o guard incompleto permite divergência silenciosa. (`bin/leitores_de_chave.py:39`, `bin/leitores_de_chave.py:66`, `bin/leitores_de_chave.py:538`) |
| **Selo sem mordida** | **Presente parcialmente:** papéis não conferidos, DNA sem exercício e rc ignorado numa asserção do contador. (`bin/tests/test_leitores_de_chave.sh:100`, `bin/tests/test_leitores_de_chave.sh:113`, `bin/tests/test_leitores_de_chave.sh:122`) |
| **Ordem de trava** | **Não há operação de banco neste instrumento.** A ordem local raiz→temporário está correta, mas falta abortar na falha do temporário. (`bin/leitores_de_chave.py:552`; `bin/tests/test_leitores_de_chave.sh:18`, `bin/tests/test_leitores_de_chave.sh:22`) |
| **Pouso antes do pré-requisito** | **[não verificado]**: anexos e código não provam cronologia de verificação, publicação ou pouso. | [nao verificado]
| **Campo com dois significados** | **Presente no nome da medida:** `acessos_crus_ata` designa todos os eventos reconhecidos de topo, enquanto o contrato exige acessos fora do acessor. (`bin/leitores_de_chave.py:24`, `bin/leitores_de_chave.py:627`; `contrato.txt:5`, anexo) |

## 12/11 17:32 · sessao d7de2a47bc6a · - · - · origem=Ronald (tmux)

Censo de escritores do HEAD por AST, sem execução do app e sem gravações. A árvore não contém `.git`; a identidade do HEAD informado não pôde ser verificada independentemente `[não verificado]`.

`R = respondida_em`; `T = resposta_colab`.

| arquivo:linha:função | papel | forma/campo | prova do modelo e porta |
|---|---|---|---|
| `app/chamados/models.py:1509:ChamadoColaborador._universo_da_ressuscitacao` | ESCRITOR, memória | `_q.respondida_em = None` — R | `_q = copy.copy(p)` em `:1495`; `p` vem de `self.perguntas_que_resolvem` em `:1484`, reverse FK de PerguntaDisputa declarada em `:3415`. Não salva a cópia; simula estado para `pergunta_viva` em `:1510`. |
| `app/chamados/models.py:1567:ChamadoColaborador.ressuscitar_perguntas` | ESCRITOR, delegação | `PerguntaDisputa.descarimbar_em_lote(..., respondida_em=None)` — R | `alvo` vem da mesma reverse FK em `:1551`. Passa pela porta de descarimbo em lote, não pelo core de resposta. |
| `app/chamados/models.py:2809:PerguntaDisputa.abrir` | encaminhamento de payload | `**campos` → `_sob_contrato` — R/T se recebidos | Método da classe PerguntaDisputa, encaminha sem selecionar campos. |
| `app/chamados/models.py:2821:PerguntaDisputa.abrir_conversa` | encaminhamento de payload | `**campos` → `_sob_contrato` — R/T se recebidos | Mesmo modelo; nascimento de conversa. |
| `app/chamados/models.py:2840:PerguntaDisputa._sob_contrato` | encaminhamento de payload | `cls._nascer(**kw)` — R/T se recebidos | `cls` de PerguntaDisputa; os dois chamadores estão em `:2809/:2821`. |
| `app/chamados/models.py:3039:PerguntaDisputa._nascer` | ESCRITOR | `p = cls(..., **campos)`; `p.save(force_insert=True)` em `:3042` — R/T | `cls` de PerguntaDisputa. Guarda `recusa_respondida_sem_texto` em `:2864/:2865`. Fora do core de resposta. O código aceita o payload; chamador de produção passando explicitamente R/T no nascimento não foi encontrado. |
| `app/chamados/models.py:3157:PerguntaDisputa.carimbar` | ESCRITOR | atribuição R; `campos.append('respondida_em')` em `:3158`; `save(update_fields=campos)` em `:3177` | `self` de PerguntaDisputa. Guarda própria exige texto em `:3155`. Fora do core e da função `recusa_respondida_sem_texto`. |
| `app/chamados/models.py:3188:PerguntaDisputa._desfazer_em_memoria` | ESCRITOR, memória | restaura R a partir de `antes` | `self` de PerguntaDisputa; restauração na recusa/exceção de `carimbar`, chamadas em `:3166/:3169/:3180`. Sem persistência própria. |
| `app/chamados/models.py:3289:PerguntaDisputa.descarimbar_em_lote` | ESCRITOR | queryset `.update(..., **outros)` termina em `:3291` — R/T conforme payload | Queryset explicitamente `PerguntaDisputa.objects`; recebe R em `:1567` e R/T no caminho `disputa_emissao.py:1630`. Fora do core; HEAD não valida o par aqui. |
| `app/chamados/models.py:3448:PerguntaDisputa.save` | persistência genérica | `super().save(*args, **kwargs)` — pode persistir R/T | `self` de PerguntaDisputa. HEAD só envolve a persistência em atomic, sem guarda do par (`:3438/:3448`). É infraestrutura comum dos saves, não reúne o caminho `QuerySet.update`. |
| `app/chamados/services/disputa_emissao.py:1467:responder_pergunta_disputa_core` | ESCRITOR | T em `:1467`, R em `:1468`; `_campos` em `:1469`; `save(update_fields=_campos)` em `:1508` | `p` vem comprovadamente de `PerguntaDisputa.objects` em `chamados/views.py:1239`, passado ao core em `:1283`. É a porta de resposta web/API declarada em `disputa_emissao.py:1425/:1429`; recusa texto vazio em `:1432/:1434`. |
| `app/chamados/services/disputa_emissao.py:1593:reabrir_para_correcao` | ESCRITOR | `dict(respondida_em=None, ..., resposta_colab='')`; `descarimbar_em_lote(qs, **campos)` em `:1630` ou `qs.update(**campos)` em `:1632` | Chamadores dão queryset explícito de PerguntaDisputa em `validacao.py:593`, `materializacao.py:688`, `curar_perguntas_em_limbo.py:69`. Fora do core; porta de reabertura. |
| `app/chamados/services/disputa_emissao.py:1657:responder_automaticamente` | ESCRITOR | T em `:1657`, R em `:1658`; `save(update_fields=['resposta_colab','respondida_em'])` em `:1660` | Modelo determinado pelo chamador de foto e seu conjunto de perguntas, não só pelo nome `p`; chamada `api/foto_recebida.py:48`. Guarda comum em `disputa_emissao.py:1655`. Fora do core de resposta web/API. |
| `app/chamados/services/disputa_emissao.py:1679:carimbar_resposta_pela_trilha` | ESCRITOR | atribui R; `save(update_fields=['respondida_em'])` em `:1681` | O comando obtém os objetos com `PerguntaDisputa.objects.filter(...)` em `curar_perguntas_em_limbo.py:55` e chama em `:67/:68`. Guarda própria em `disputa_emissao.py:1675`. Fora do core e da guarda comum. |

Há um escritor operacional adicional, explicitamente restrito à sombra:

| arquivo:linha:função | papel | forma/campo | prova do modelo |
|---|---|---|---|
| `bin/sombra_mascara_pessoal.py:57:<módulo>` | ESCRITOR | `setattr(o, c, novo)` — T | `M=apps.get_model(app,mod)` em `:46`; `o` vem de `M._base_manager` em `:51`; `TEXTO[('chamados','PerguntaDisputa')]` inclui T em `bin/sombra_regras_pessoais.py:85`. |
| `bin/sombra_mascara_pessoal.py:62:<módulo>` | ESCRITOR | `M._base_manager.bulk_update(lote,list(campos))` — T; lote cheio | Mesma origem acima. |
| `bin/sombra_mascara_pessoal.py:64:<módulo>` | ESCRITOR | mesmo `bulk_update` — T; sobra | Mesma origem acima. |

O script exige `connection.settings_dict['NAME'] == 'sombra'` em `bin/sombra_mascara_pessoal.py:10`; portanto não prova uma segunda porta viva no banco de produção. A presença do escritor de T no código está comprovada pelas linhas acima.

A função `app/chamados/services/fio.py:1578:recusa_respondida_sem_texto` **não é um escritor de PerguntaDisputa**: instancia apenas `SimpleNamespace` em `:1585`, consulta o predicado e pode levantar em `:1587`. No HEAD, os escritores que a chamam são `_nascer` (`models.py:2864`) e `responder_automaticamente` (`disputa_emissao.py:1655`). O core humano, a trilha, o carimbo e a reabertura têm caminhos distintos nos pontos da tabela. Assim, não há UMA porta que concentre todas as escritas físicas enumeradas.

Casos excluídos após prova:

- `app/chamados/services/explicador.py:78`: `base.update(...)` atualiza dict, não queryset; utiliza T apenas na construção dos textos em `:79/:80`.
- `app/chamados/veredito_lavrado.py:250`: `PerguntaDisputa.objects.bulk_update(...)` usa `CAMPOS_DA_LAVRA`, definido em `:237` sem R/T.
- `app/core/reexibir.py:49`: `setattr(obj,campo,valor)` genérico, sem persistência; os chamadores encontrados são `colaboradores/views.py:1097/:1262` e trabalham com Colaborador, não PerguntaDisputa.
- `app/chamados/services/passivo_validado.py:83`: `getattr(p,c,None)` percorre `CAMPOS_REVERSAO` (`:64/:66`) para um snapshot colocado na saída em `:367`; não há restauração desse snapshot no arquivo.
- `app/chamados/services/foto_e4b.py:180`: leitura dos dicts antes/depois, não atribuição.

Não houve escritor selecionado somente pelo nome da variável: os escritores da tabela foram atribuídos por classe, queryset/relação, chamador ou registro `apps.get_model`.

Sobre escritores que mudam o desenho:

- `responder_automaticamente` é outra operação persistente de resposta comprovada, chamada por upload de foto em `api/foto_recebida.py:48`; não passa pelo core humano (`disputa_emissao.py:1657/:1660`).
- `carimbar_resposta_pela_trilha` é outra operação persistente comprovada, chamada por management command com `--apply` em `curar_perguntas_em_limbo.py:64/:68`; não passa pelo core humano.
- `PerguntaDisputa.carimbar` pode também persistir R (`models.py:3157/:3177`), além do carimbo de materialização.
- Migrações que obtêm o modelo histórico: `0051_decisao_pergunta.py:23` atualiza somente `colaborador_id` em `:25` e `instancia_marco` em `:27`; `0053_pergunta_unica_por_familia.py:37` somente consulta colisões em `:39/:46`. Os campos R/T em `0014_perguntadisputa.py:24/:25` são declaração de schema. Não foi encontrado escritor de dados R/T nas migrations examinadas.
- Receivers de PerguntaDisputa fecham beco (`signals.py:229/:240`) ou emitem eventos (`:252/:259`), sem escrita direta do par nesses corpos.

No WIP fornecido, as atribuições e updates listados permanecem; acrescentam-se validações no `save` e no lote. Isso acrescenta uma guarda compartilhada a mais caminhos, mas não transfere as escritas para uma única função — prova no anexo `wip_1b.diff`, hunks de `models.py` iniciados em `@@ -3279` e `@@ -3442`. A execução dos testes e o estado efetivo de produção são `[não verificado]`. [nao verificado]

## 12/11 17:31 · sessao 1cf1df545dd1 · - · - · origem=Ronald (tmux)

Censo AST somente leitura concluído para `app/chamados/**/*.py`, excluindo `tests/`, `migrations/`, `models.py` e `juizes.py` (estes ficaram com o agente principal). Atributos `Load`, kwargs ORM, literais de projeção, `getattr`, índices e nomes provenientes de tuplas foram conferidos no código.

Legenda: `E=respondida_em`, `T=resposta_colab`; todas as linhas abaixo são **LEITOR**. Caminhos relativos a `app/chamados/`.

| arquivo:linha:função | forma/campo | origem/modelo |
|---|---|---|
| `canal.py:65:perguntas_mudas_com_resposta` | filtro T | `PerguntaDisputa.objects`, 58,64 |
| `reconciliador.py:365:lavrar_sem_turno_aberto` | atributo T | recebe pergunta; chamada em 1620 a partir de `reatar_pergunta_orfa`; origem por encadeamento, não anotação |
| `reconciliador.py:1083:veredito_orfa` | atributo T | universo `_orfas_abertas`, `PerguntaDisputa.objects` em 1048-1049; chamada 1136 |
| `views_cobrar.py:169:validar_inline` | atributo E | `_vivas_de(d)`/perguntas vivas; origem `PerguntaDisputa` |
| `views_cobrar.py:176:validar_inline` | filtro E | `PerguntaDisputa.objects` 175 |
| `views_cobrar.py:291:proposta_lote` | `x['pergunta'].resposta_colab` T | itens da fila/classificação de perguntas |
| `veredito_lavrado.py:207-208:marcos_do_lote` | filtro E, `**_ate('respondida_em')`, `Max(E)` | alias `_PD=PerguntaDisputa`, 201; `_ate` 163 gera filtro de corte |
| `views.py:1280:responder_pergunta_disputa` | atributo E | `PerguntaDisputa.objects.filter` 1239 |
| `views.py:1416:reabrir_disputa_supervisao` | filtro E | `PerguntaDisputa.objects` na própria linha |
| `relatorios_disputa.py:21:classificar` | atributo E | `disputa.perguntas.all()` 12; `vivas` 18 |
| `services/cobranca.py:229:_calados` | filtro E | `PerguntaDisputa.objects` 228 |
| `services/vigia_fio.py:681:censo_alcance` | atributo E | universo `PerguntaDisputa.objects` 615 |
| `services/vigia_fio.py:789,792:respondida_sem_resposta` | filtro E / `values_list(T)` | `PerguntaDisputa.objects` 789 |
| `services/passivo_validado.py:83:_reversao_de` | `getattr(p,c)` E | `CAMPOS_REVERSAO` 64-66; chamada 367, universo `classificar(empresas)` 324 |
| `services/materializacao.py:280,308:_tipo_pelo_marco` | atributos T | parâmetro `p` da cadeia de materialização; chamadas originam do queryset de perguntas 1375-1382 e da validação `PerguntaDisputa.objects.get` em `validacao.py:389,463` |
| `services/materializacao.py:359,405,412:_ts_tipo_do_plantio` | atributos T | mesma cadeia |
| `services/materializacao.py:438:estado_aplicacao_pergunta` | atributo T | chamada por `fio.py:356`, sobre perguntas do chamado; usa também constantes `PerguntaDisputa` 444 |
| `services/materializacao.py:474:_declarar_ausencia_de_pergunta` | atributo T | mesma cadeia de materialização |
| `services/materializacao.py:571:_materializar_ausencia_de_resposta` | atributo T de `q` | `disputa.perguntas.filter` 538 |
| `services/materializacao.py:633:perguntas_absorvidas_por` | filtro T | `base=PerguntaDisputa.objects.filter` 629 |
| `services/materializacao.py:674-675:devolver_absorvidas_da_ausencia_rejeitada` | atributos T/E; serialização para dict | `ps=perguntas_absorvidas_por` 671; origem PD 629 |
| `services/materializacao.py:835,876,977:_materializar_resposta_hora` | atributos T; dict de saída | mesma cadeia de materialização |
| `services/materializacao.py:1001,1010,1062:_materializar_resposta_data_hora` | atributos T; dict de saída | mesma cadeia |
| `services/materializacao.py:1376:materializar_perguntas_validadas_da_disputa` | filtro E | `disputa.perguntas.filter` 1375 |
| `services/validacao.py:141:_carimbar_validacao` | atributo T | `perg` carregada por `validar_pergunta`, `PerguntaDisputa.objects.get` 389 |
| `services/validacao.py:189,207:devolver_validacao_recusada` | `refresh_from_db(fields=[T])`; atributo T | chamada 464 com a instância carregada em 389 |
| `services/validacao.py:236:_e_aceite_de_folga` | atributo T | helper da validação PD |
| `services/validacao.py:393:validar_pergunta` | atributo E | `PerguntaDisputa.objects.get` 389 |
| `services/validacao.py:576,580-581,600:recusar_pergunta` | atributos E/T; leitura `antes[T]` | `PerguntaDisputa.objects...get` 569-570 |
| `services/validacao.py:851,853,855,860-863,866:classe_do_lote` | atributos T | fila de `perguntas_respondidas_a_validar`, chamada 921-922 |
| `services/validacao.py:924:fila_validar_classificada` | atributo E | `perguntas_respondidas_a_validar()` 918,921; parâmetro `qs` alternativo não é validado por tipo |
| `services/pergunta_no_app.py:71:consulta_do_app` | atributo E | `perguntas_do_app` 70, cuja consulta é `PerguntaDisputa.objects` 49 |
| `services/declaracao_texto.py:417,419:declarar_hora_colab` | atributos E/T | retorno `PerguntaDisputa.abrir` 376 |
| `services/declaracao_texto.py:687:aplicar_declaracao` | atributo E | retorno `PerguntaDisputa.abrir` 666 |
| `services/verbos.py:335-336,342,345:placar_aceite` | filtro E; `values(E)`; `pg[E]` | `PerguntaDisputa.objects` 333 |
| `services/disputa_emissao.py:1619,1627:reabrir_para_correcao` | `values_list(T,E)`; nomes `_txt`,`_resp` na condição | queryset de PD; método chama portas PD em 1609,1630 |
| `services/disputa_emissao.py:1647:responder_automaticamente` | atributo T | parâmetro `p`, contrato e chamadas de resposta PD |
| `services/disputa_emissao.py:1675,1683:carimbar_resposta_pela_trilha` | atributos E/T | parâmetro `p`; chamada no command `curar_perguntas_em_limbo` sobre PD |
| `services/disputa_emissao.py:1718:recobrar_questionario` | filtro E | `PerguntaDisputa.objects` 1717 |
| `services/disputa_emissao.py:1826,1844:reabrir_credor_de_pergunta_viva` | atributos E/T | `p` pergunta; `carga` de relações `perguntas_que_resolvem`, 1803-1805 |
| `services/disputa_emissao.py:1984,2057,2062,2074:_resolver_perguntas_por_chamado_resolvido` | atributos E/T | universo perguntas do chamado, PD importada 17 |
| `services/explicador.py:32:_explicar_validada` | atributo T | chamada 97 por `explicar_pergunta`; origem queryset PD 122,133 |
| `services/explicador.py:79-80:explicar_pergunta` | atributo T | `PerguntaDisputa.objects.filter` 122; chamada 133 |
| `services/foto_e4b.py:180:nascimentos` | `a[c]`,`d[c]`, c=E | somente ramo `chave.startswith('perguntas:')` 179; foto associa perguntas a PD 254,272,311 |
| `services/raiox.py:138,143-144:creditos_perguntas` | `values_list(E,T)`, `p[3]`,`p[4]` | `_pq=PerguntaDisputa.objects` 56, `_vivas` 74,135-138 |
| `services/painel.py:67:marcador_do_placar` | `Max(E)` | `PerguntaDisputa.objects.aggregate` 66 |
| `services/becos.py:30:classificar` | atributo E | `d.perguntas.all()` 29 |
| `services/acoes_disputa.py:95:acao_reabrir_disputa` | filtro E | `PerguntaDisputa.objects` na linha |
| `services/acoes_disputa.py:130:acao_regularizar_disputa` | atributo E | `_vivas_de(d)` |
| `services/acoes_disputa.py:353:acao_corrigir_resposta` | atributo T | `PerguntaDisputa.objects...get` 341 |
| `services/fio.py:174:aguarda_o_colaborador` | atributo E | coleção de perguntas recebida; chamadores e contrato PD |
| `services/fio.py:681:processar_post_fio` | atributo E | `perguntas_vivas_de` importada 680 |
| `services/fio.py:1254:canonica_do_grupo` | atributo T | contrato retorno PD 1248; grupos carregados de PD, por exemplo command dedup 74,76 |
| `services/fio.py:1266:merito_perdedor` | atributo T | perguntas do mesmo grupo; constante PD 1265 |
| `services/historico.py:99,105,109:_eventos_dos_chamados` | campo variável `getattr(p,campo)` E; atributo T | `PerguntaDisputa.objects` 96, laço 97-105 |
| `templatetags/chamados_tags.py:142:leitura_da_resposta` | `getattr(perg,T,'')` | parâmetro de filtro de template; passagem por juiz de resposta 144-147; tipo não validado em runtime |
| `management/commands/curar_perguntas_em_limbo.py:32:nota_da_resposta` | atributo T | `p` de queryset PD em `handle`, 46 |
| `management/commands/curar_perguntas_em_limbo.py:46,69:Command.handle` | filtros E/T | `PerguntaDisputa.objects` |
| `management/commands/validar_classe_a.py:80:Command.handle` | atributo T | `d.respostas_sem_veredito()` 54, guardadas em `pendentes` e `classe_a` |
| `management/commands/auditar_invariantes_chamados.py:177:_contraprova_i21a` | atributo E | chamada 1585 sobre subconjunto de `_i21_cru`, queryset PD 1572 |
| `management/commands/auditar_invariantes_chamados.py:592,1105-1106,1572,1575,1603,1655:Command.handle` | atributos E/T e filtros E/T | alias `PD.objects`; 1655 relação `perguntas_que_resolvem` 1652 |
| `management/commands/backfill_decisoes_pergunta.py:56,60-61:linhas_a_criar` | filtro E; `only(E,T)` | `PerguntaDisputa.objects` 56 |
| `management/commands/backfill_decisoes_pergunta.py:326:aplicar` | atributo T | itens de perguntas do backfill |
| `management/commands/backfill_decisoes_pergunta.py:504:Command.handle` | `only(T)` | `PerguntaDisputa.objects` na linha |
| `management/commands/dedup_perguntas_fato.py:75:Command.handle` | atributo T | `ps=PerguntaDisputa.objects...` 74 |

Casos que **não** devem entrar como escritor/leitor do modelo diretamente:

- `services/fio.py:1585:recusa_respondida_sem_texto`: kwargs E/T são de **`types.SimpleNamespace`**, importado em 1583; o código cria um sondador para o predicado, não `PerguntaDisputa`. É guarda do payload, não porta que persiste. `fio.py:1578-1585`.
- `services/validacao.py:605`: dict `depois` enviado ao observador `_evento`, não escrita ORM. A limpeza real passa por `reabrir_para_correcao` em 593-594.
- `services/disputa_emissao.py:1686`: chave E em dict de observabilidade, não escrita de modelo.
- `services/materializacao.py:674-678`: dict `_foto` guarda valores lidos, não os persiste de volta.
- `services/passivo_validado.py:66`: declaração de nomes; a leitura real é 83.
- `services/disputa_emissao.py:1469,1660,1681`: literais dos campos em `save`, portanto pertencem à tabela de **escritores**, não projeções leitoras.
- `services/disputa_emissao.py:1593`: dict de limpeza E/T, portanto payload de **escritor**; persistência em 1630 ou ramo subsequente.

Pontos de cautela de tipo: as funções auxiliares acima recebem parâmetros sem anotação; a evidência é o encadeamento de chamadas e querysets indicado. As funções `aguarda_o_colaborador`, `fila_validar_classificada(qs=...)`, filtros de template e helpers públicos não verificam em runtime que todo argumento é PD; afirmar isso para qualquer chamador externo seria `[nao verificado]`. Nenhum homônimo de outro modelo apareceu neste recorte da varredura AST; a exceção concreta é o `SimpleNamespace` em `fio.py:1585`.

## 12/11 17:31 · sessao 144396724f9c · - · - · origem=Ronald (tmux)

Censo AST de `.py` fora de `app/chamados`, excluindo diretórios `tests/` e `migrations/`; templates por expressões ativas, excluindo comentários. `E = respondida_em`; `T = resposta_colab`.

| arquivo:linha:função | papel | forma/campo | prova do modelo |
|---|---|---|---|
| `bin/censo_folha.py:28,29:<módulo>` | LEITOR | filtros ORM `E__isnull` | `PerguntaDisputa.objects` explícito; import `:12` |
| `app/folha/previa.py:142:_material` | LEITOR | atributo T | Chamador `:95`; mapa alimentado de `PerguntaDisputa.objects` em `:64-67` |
| `app/ponto/supra_juiz.py:545,548:pode_executar_csf` | LEITOR | atributo E, `is not None` | Itera `d.perguntas` e `ch.perguntas_que_resolvem` em `:543-548`; relações declaradas por `PerguntaDisputa` em `app/chamados/models.py:2436-2439,3410-3415` |
| `app/api/views.py:1159,1161:api_chamados_v2` | LEITOR | T; E duas vezes, serialização e guarda | `perguntas_por_chamado` alimentado de `consulta['pendentes'] + consulta['historico']` em `:1029-1035`; fonte `app/chamados/services/pergunta_no_app.py:49-58,70-75` |
| `app/api/views.py:2557,2559:api_disputa_perguntas._item` | LEITOR | T; E duas vezes, serialização e guarda | `consulta_do_app` em `:2532-2535`; chamadas `_item(p)` `:2562,2572`; mesma fonte acima |
| `app/api/views.py:2573:api_disputa_perguntas` | LEITOR | atributo E, `is not None` | `consulta['historico']`; fonte acima |
| `app/api/views.py:2616,2633:api_disputa_responder` | LEITOR | E comparação e serialização | Objeto consultado explicitamente por `PerguntaDisputa.objects` em `:2607-2609` |
| `app/api/views_mensageria.py:567,569,570:_item_validar` | LEITOR | T; E serialização e diferença de datas | Chamador `:540-546` usa `perguntas_respondidas_a_validar()`; origem explícita `PerguntaDisputa.objects` em `app/chamados/juizes.py:1672-1676` |
| `app/colaboradores/services/calendario.py:474:contexto_calendario` | LEITOR | `.exclude(T='')` | `_PD = PerguntaDisputa`, import `:469`; queryset `:473` |
| `app/colaboradores/services/ficha.py:171:ficha_do_colab` | LEITOR | atributo T | `perguntas_do_app` em `:166`; fonte explícita em `app/chamados/services/pergunta_no_app.py:49-58` |
| `app/ponto/services/cartorio.py:1641:_julgar_colab_corpo._tem_resposta_do_colab` | LEITOR | atributo T | Chamador fornece `x.perguntas_que_resolvem.all()` em `:1645`; relação em `app/chamados/models.py:3410-3415` |
| `app/ponto/services/dia_do_colab.py:70:dia_do_colab` | LEITOR | atributo T | `for p in PerguntaDisputa.objects.filter(...)` em `:64-65` |
| `app/ponto/services/veredito_celula.py:197:fechar_o_fio_do_dia` | LEITOR | atributo T | `vivas` de `PerguntaDisputa.objects` em `:181-182`; iteração `:189` |
| `app/ponto/management/commands/reconciliar_grade.py:461:Command._relavrar_orfa` | LEITOR | filtro ORM E | `PerguntaDisputa.objects.filter` em `:460-461` |
| `bin/sombra_mascara_pessoal.py:51,54:<módulo>` | LEITOR | `.only('pk', *campos)`; `getattr(o,c)` variável T | `R.TEXTO` → `apps.get_model(app,mod)` em `:44-46`; `bin/sombra_regras_pessoais.py:85` contém `('chamados','PerguntaDisputa')` e `resposta_colab` |
| `bin/sombra_selo_pessoal.py:77:<módulo>` | LEITOR | SQL dinâmico lê T em `coalesce` e regex | Itera `R.TEXTO` `:74-76`, tabela resolvida por `apps.get_model` `:24`; configuração `bin/sombra_regras_pessoais.py:85` |

O mascarador também é **ESCRITOR fora da porta**: `setattr(o,c,novo)` em `bin/sombra_mascara_pessoal.py:57`, persistido por `bulk_update(lote,list(campos))` em `:62,64`. Ele exige banco chamado `sombra` por `assert` em `:10`; não atribuir esse escritor a produção operacional sem delimitar o universo. O código prova a escrita na cópia da sombra; execução efetiva [não verificado].

Metadados, sem leitura do valor do campo: `bin/sombra_regras_pessoais.py:85` declara T; `bin/inventario_pessoal_dos_modelos.py:53-60` e `bin/sombra_selo_pessoal.py:26-29` usam essa declaração para inventariar colunas.

Templates:

| arquivo:linha:contexto | papel | forma/campo | prova do modelo |
|---|---|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:94,155,384` | LEITOR | condições E | Loops sobre `chamado.perguntas_disputa` `:76,384`; fonte `app/chamados/services/fio.py:242,261,271` |
| mesmo `:100` | LEITOR | filtro `default:T`; `E|date` | mesma origem |
| mesmo `:108,135,148,176` | LEITOR | condições/comparações T | mesma origem |
| mesmo `:137` | LEITOR | `default:T`, condição E, `E|date` | mesma origem |
| mesmo `:142,177` | LEITOR | interpolação T com `default` | mesma origem |
| mesmo `:385` | LEITOR | `default:T`, `E|date` | mesma origem |
| `app/templates/chamados/partials/modal_fio.html:185,217` | LEITOR | condições E | Loop `:158` sobre mesma `chamado.perguntas_disputa`; fonte acima |
| mesmo `:219,220` | LEITOR | interpolação T; filtro `E|date` | mesma origem |
| `app/templates/chamados/partials/_pergunta_card.html:24,29,32` | LEITOR | condições E; `E|date` | Contexto explícito `{'perg': _p}` de `_PD.objects.filter` em `app/chamados/views.py:398-405` |
| `app/templates/chamados/partials/_lote_preview.html:24` | LEITOR | interpolação `x.pergunta.T` | `views_cobrar.py:361-363` fornece `f['por_classe'][LOTE_A]`; `services/validacao.py:918-924` põe `pergunta=p` da fila `perguntas_respondidas_a_validar()`; queryset em `juizes.py:1672-1676` |

Nenhum caso dessa tabela foi decidido **somente pelo nome**: todos têm origem rastreável em queryset, chamada ou relação declarada, conforme a última coluna.

Delta dos templates no anexo: o wip retira as condições E das linhas HEAD `94,137,155,384`, substituindo por `resposta_enviada`; preserva a exibição E e as leituras T. No resultado projetado, E continua nas linhas wip `106,143,391`; T nas linhas `106,114,141,143,148,154,182,183,391` — linhas calculadas pelos hunks fornecidos, não por arquivo wip local. A atribuição `resposta_enviada` é outro atributo, não escrita dos dois campos censitados (`wip_1b.diff`, hunk `fio.py`, nova linha `358`).

## 12/11 17:31 · sessao bc99985d1006 · - · - · origem=Ronald (tmux)

- **Não há classificação global pelo nome `ata`/`dna`.** `Name` só ganha proveniência por declaração exata arquivo+função+nome, atribuição, `values_list` ou alias; qualquer `.ata`/`.dna` e `getattr(x, 'ata'/'dna')` é aceito sem comprovar a classe. (`bin/leitores_de_chave.py:127`, `bin/leitores_de_chave.py:140`, `bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:249`)

- **O 95 depende das declarações.** Desligar somente `PARAMS` produz 73; desligar `PARAMS+RETURNS` produz 62 em reprodução em memória. A formação do número está em `bin/leitores_de_chave.py:627` e `bin/leitores_de_chave.py:631`; os mapas estão em `bin/leitores_de_chave.py:39` e `bin/leitores_de_chave.py:66`.

- **Os 33 acessos de topo dependentes desses mapas**, agrupados por sítio, são: `app/chamados/juizes.py:438` (2); `app/escala/services/leitor_celula.py:68`, `:82`, `:91`, `:103`, `:105` (2), `:115`, `:175`, `:187`, `:190`; `app/ponto/services/cartorio.py:424`, `:426`, `:738` (2), `:739`, `:740`, `:741`, `:742`, `:745`, `:746`, `:762`, `:767`, `:771`, `:805`, `:812`, `:834`, `:839`, `:913`, `:917` (2); `app/relatorios/dia_da_frota.py:21`. As declarações correspondentes estão em `bin/leitores_de_chave.py:41` e `bin/leitores_de_chave.py:68`.

- **Há furo na validação das declarações:** ela confere existência da função, sem conferir existência do nome declarado. Renomear o parâmetro `ata` e suas referências em `_orfas_da_ata`, mantendo a função, faz desaparecerem os acessos `orfas`/`n_orfas`; a contagem cai de 95 para 93 sem violar essa validação. (`bin/leitores_de_chave.py:128`, `bin/leitores_de_chave.py:541`, `app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:190`)

- **Sítios corretamente fora:** a variável chamada `ata` em `app/relatorios/furos.py:132` é mapa da grade indexado por data, consumido em `app/relatorios/furos.py:134`; `dia.get('orfas')` em `app/escala/services/furos_diarios.py:22` recebe dia da grade por `app/escala/services/furos_diarios.py:43` e `app/escala/services/furos_diarios.py:54`; outras leituras da grade ficam em `app/folha/causas.py:43`, `app/folha/causas.py:100`, `app/ponto/services/bordas_realizado.py:49`, `app/relatorios/pdf_espelho.py:477` e `app/relatorios/pdf_espelho.py:552`. `FuroDiario.orfas` é inteiro, separado do JSON da célula. (`app/escala/models.py:1883`)

- **Sítios reais incluídos:** `getattr` em `app/chamados/juizes.py:966`, `app/chamados/reconciliador.py:701`, `app/ponto/services/lastro.py:89`, `:113`, `:128`, `:149`; comprehension lendo a raiz em `app/ponto/juiz_batida.py:161` e `app/relatorios/cartao_pela_celula.py:288`; desempacotamento e alias `a` em `app/ponto/services/vigia_pareador_celula.py:69`, `:73`, `:74`, `:78`. A cópia apenas em memória `_cj.ata` também conta como escrita de código. (`app/escala/utils.py:1886`, `app/escala/utils.py:1887`, `app/escala/utils.py:1895`)

- **Faltas reais declaradas:** leitura ORM de `dna__marcos` em `app/escala/management/commands/celulas_contra_o_template.py:49`; filtro ORM de `ata.lampadas` em `app/chamados/models.py:3356`. A exclusão é declarada em `bin/leitores_de_chave.py:19` e `bin/leitores_de_chave.py:34`.

- **Subchaves em laço comum não propagam proveniência:** `ata.get('lampadas')` é visto, mas `lamp['acesa']`, `lamp['luz']` e `lamp['dispensada']` não são ligados à ata. Isso limita censo de subchaves, embora a operação de topo já seja contada. (`app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:806`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`, `bin/leitores_de_chave.py:176`)

- **Alias de dois saltos:** o corte é explícito em `bin/leitores_de_chave.py:224`. Exemplo real de acesso de topo perdido exclusivamente por esse corte: [não verificado]. Atribuição encadeada `a=b=c.ata` liga ambos os alvos ao mesmo valor; não constitui automaticamente dois saltos. (`bin/leitores_de_chave.py:134`, `bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:166`)

## 12/11 17:29 · sessao 56a1fc7d5d3d · - · - · origem=Ronald (tmux)

Achados sobre consumidores e semântica:

- O emissor é `run()`: `--contar-crus` imprime `len(top)`; JSON imprime resumo e listas; texto imprime seções e seus comprimentos. São projeções do mesmo censo, mas seus números têm unidades diferentes: operações de topo, arquivos distintos e eventos selecionados. `bin/leitores_de_chave.py:627`, `bin/leitores_de_chave.py:631`, `bin/leitores_de_chave.py:645`, `bin/leitores_de_chave.py:659`.
- No texto, `len(sel)` inclui eventos `*`, descendentes e dinâmicos que casem com a seleção; no JSON, `resumo_producao` é global mesmo numa consulta de uma chave; `producao`/`testes` são filtrados. Não são números intercambiáveis. `bin/leitores_de_chave.py:635`, `bin/leitores_de_chave.py:645`, `bin/leitores_de_chave.py:652`, `bin/leitores_de_chave.py:660`.
- Cada evento guarda posição, caminho, papéis e forma; a deduplicação usa também coluna, papéis e forma. Não é uma unidade “linha”, “arquivo” ou necessariamente “nó AST único”. Um `setdefault`, `pop` ou `+=` conta um evento com dois papéis; somar leitores e escritores superconta as operações. `bin/leitores_de_chave.py:288`, `bin/leitores_de_chave.py:302`, `bin/leitores_de_chave.py:385`, `bin/leitores_de_chave.py:424`.
- Execução somente leitura confirmou `--contar-crus CelulaDia.ata` = 95; JSON produziu 12 chaves, 95 operações de topo, 27 arquivos, 103 eventos de produção e 89 pares arquivo:linha. Os totais são resultado da execução, sem arquivo de resultado persistido: [não verificado]. O cálculo correspondente está em `bin/leitores_de_chave.py:627`.
- Os quatro eventos `*` vistos foram cópias de JSON em `app/ponto/portas/celula.py:945`, `app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:1083`, `app/ponto/services/cartorio.py:917`; entram na listagem por chave, mas não no contador. `bin/leitores_de_chave.py:629`, `bin/leitores_de_chave.py:635`.
- O consumidor executável encontrado é o selo: chama a ferramenta, captura stdout/stderr e lê JSON, número e códigos de retorno. O JSON é lido para conferir sítios, formas e separação produção/testes; o número é lido para os casos real e sintético. `bin/tests/test_leitores_de_chave.sh:26`, `bin/tests/test_leitores_de_chave.sh:30`, `bin/tests/test_leitores_de_chave.sh:36`, `bin/tests/test_leitores_de_chave.sh:44`, `bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:106`, `bin/tests/test_leitores_de_chave.sh:112`.
- A implementação chama esse número de catraca `acessos_crus_ata`, mas conta todos os eventos de topo de produção; não existe exclusão do futuro acessor nem comparação com teto anterior. Portanto ainda não implementa “acessos crus fora do acessor, só encolhe”. `bin/leitores_de_chave.py:24`, `bin/leitores_de_chave.py:627`, `bin/tests/test_leitores_de_chave.sh:44`.
- Não encontrei consumidor da ferramenta em `gerar_estado`, disjuntor ou briefs disponíveis: integração futura e unidade pressuposta por esses leitores são [não verificado]. O juiz existente do CENSO exige referências textuais de leitores/escritores e decisões, sem conferir saída desse instrumento. `bin/disjuntor.py:1522`, `bin/disjuntor.py:1565`.
- `PARAMS`/`RETURNS` são fonte manual adicional de proveniência. O guard verifica somente existência da função; não verifica nome do parâmetro, tipo nem fluxo. Renomear só o parâmetro pode reduzir o censo sem disparar rc 2. `bin/leitores_de_chave.py:39`, `bin/leitores_de_chave.py:66`, `bin/leitores_de_chave.py:538`.
- O censo publicado, o envio prévio do contrato, a execução histórica do selo, o pouso e o aval em arquivos de `logs/` não estão demonstrados pelos arquivos disponíveis: [não verificado]. A referência local ao contrato é apenas comentário/docstring. `bin/leitores_de_chave.py:10`, `bin/tests/test_leitores_de_chave.sh:2`.

_4382 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
