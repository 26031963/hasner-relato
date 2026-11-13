# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 18:41** · publicado em **13/11/2026 18:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 18:41 · sessao 0edada80a8c4 · JUSTIFICATIVA-PAGA-O-PERIODO · sonda · origem=Code

Não fiz nova execução: o envelope reescreve credenciais, cria arquivo temporário e acrescenta uma trilha, contrariando a regra de não escrever arquivos (`bin/sonda_leitura.sh:126–134`, `bin/sonda_leitura.sh:216–218`, `bin/sonda_leitura.sh:248–250`).

Segue a transcrição literal inteira da saída fornecida, sem validação independente da execução. [nao verificado]

```text
sonda_leitura: sonda.py [3f1bd3f83f2f34bc] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO 45
CHAVES_LAMPADA ['acesa', 'dispensada', 'hora', 'luz', 'tipo', 'tipo_real']
MULT just_aprovadas_mesmo_tipo>=2 0 ancoras_distintas_mesmo_tipo>=2 0 lampadas_dispensadas_mesmo_marco>=2 28 sem_justificativa_aprovada_no_dia 0
HIST (n_just_aprovadas, n_lampadas_dispensadas) -> [((1, 1), 17), ((1, 2), 1), ((1, 3), 27)]
CASO (118689, 56, '2026-10-07', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1}, 2)
CASO (140889, 56, '2026-10-25', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1}, 2)
CASO (127808, 976, '2026-10-26', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (112074, 567, '2026-10-12', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (135337, 496, '2026-10-22', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (3283, 408, '2026-08-17', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (114134, 206, '2026-09-22', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (2261, 855, '2026-08-18', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (110059, 341, '2026-10-07', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (108830, 340, '2026-10-08', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (24651, 697, '2026-09-01', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1, 'S': 2}, 4)
CASO (24660, 697, '2026-09-10', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (140580, 750, '2026-10-26', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (113955, 438, '2026-09-23', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (112352, 347, '2026-09-30', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (112358, 347, '2026-10-06', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (106510, 922, '2026-09-28', {'outro': 1}, {'outro': 1}, {'S': 1}, 2)
CASO (106526, 922, '2026-10-14', {'outro': 1}, {'outro': 1}, {'S': 1}, 2)
CASO (110352, 389, '2026-09-30', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (105323, 789, '2026-10-11', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (8127, 518, '2026-07-25', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (8131, 518, '2026-07-29', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1, 'S': 2}, 4)
CASO (8148, 518, '2026-08-15', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1, 'S': 2}, 4)
CASO (8150, 518, '2026-08-17', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (141802, 996, '2026-10-26', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (131421, 912, '2026-10-23', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (105365, 938, '2026-09-23', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (33812, 564, '2026-09-24', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (116944, 358, '2026-10-02', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (121745, 962, '2026-10-25', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2}, 4)
CASO (105667, 931, '2026-09-25', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1}, 2)
CASO (33879, 310, '2026-09-21', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 2, 'S': 1}, 4)
CASO (21683, 228, '2026-09-14', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (104743, 228, '2026-10-01', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1}, 2)
CASO (113727, 468, '2026-10-05', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (116515, 829, '2026-09-23', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 2, 'S': 1}, 4)
CASO (106125, 331, '2026-10-03', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (118347, 643, '2026-09-25', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (16234, 510, '2026-08-10', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (129875, 932, '2026-10-27', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (143004, 1004, '2026-10-27', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 2, 'S': 1}, 4)
CASO (111910, 396, '2026-09-28', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (1013, 863, '2026-08-10', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (1021, 863, '2026-08-18', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (1023, 863, '2026-08-20', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
FIM
```

1. **Contagens impressas para 2+ no mesmo colab-dia:**

   | Medida | Casos |
   |---|---:|
   | Justificativas aprovadas do mesmo tipo | 0 [nao verificado] |
   | Âncoras `batida_entrada` distintas do mesmo tipo | 0 [nao verificado] |
   | Lâmpadas dispensadas agrupadas pelo tipo do marco | 28 [nao verificado] |

   **A medida de âncoras distintas é a que corresponde a episódios/turnos distintos entre as três:** `batida_entrada` nomeia a “Batida de entrada do turno com problema”, e a identidade declarada da justificativa combina colaborador, data e essa âncora (`app/ponto/models.py:139–144`, `app/ponto/models.py:186–187`).

   A lâmpada nomeia o **marco** por `tipo` e `hora`, mantendo separadamente `tipo_real` da batida (`app/ponto/services/cartorio.py:648–650`, `app/ponto/services/cartorio.py:706–713`). Portanto, os agrupamentos impressos `E`/`S` mostram repetição de **tipo de marco**, sem demonstrar repetição do mesmo marco completo `(tipo, hora)` nem de episódios. [nao verificado] Uma justificativa pode acompanhar várias lâmpadas dispensadas porque o código percorre todas as lâmpadas ainda não acesas quando o dia está justificado (`app/ponto/services/cartorio.py:802–810`).

2. **Sim, bate numericamente: 45 = 9+23+3+10**, distribuição registrada em `app/docs/BACKLOG.md:1872`; o dado fornecido declara 45, lista 45 casos e seu histograma soma 17+1+27=45. [nao verificado] A identidade dos IDs com `logs/HJ-093/sonda2/r.r` e a equivalência dos filtros não foram verificadas. [nao verificado]

3. **O dado não mostra erro nem a falha de chave exemplificada:** `CHAVES_LAMPADA` contém `tipo` e `hora`, e os agrupamentos aparecem como `E`/`S`, sem `?`; nenhuma das três medidas impressas está vazia. [nao verificado] A ausência de uma chave literalmente chamada `marco` não caracteriza falha: o código representa o marco usando `tipo` e `hora` (`app/ponto/services/cartorio.py:648–650`, `app/ponto/services/cartorio.py:711`). O corpo da sonda, o tratamento de âncoras nulas e o código de saída da execução permanecem [nao verificado].

## 13/11 18:37 · sessao 85aae53bb678 · RAIA-PREPARACAO · censo · origem=Code

**O universo global não fechou: total de costuras e total global com desencontro = [nao verificado].** A revisão individualizada abaixo resulta em **30 itens: 23 reais, 6 escritas mortas, 0 ramos mortos demonstrados e 1 falso**. Dos 23 originais, são **16 reais, 6 escritas mortas e 1 falso**; os sete adicionais repetem a omissão da data catalogada no leitor em lote (`app/chamados/juizes.py:114`, `:187`; provas individuais no quadro).

“Real” abaixo significa incompatibilidade demonstrada em caminho alcançável pelo código. Ocorrência e quantidade efetiva em produção continuam **[nao verificado]**.

**Contagem e limite do universo.** Usei AST em memória sobre `Path('app/chamados').rglob('*.py')`, excluindo componentes `tests`, `migrations`, `__pycache__` e arquivos `test_*`. Os padrões foram `ast.Dict`, `ast.DictComp`, chamadas `dict(...)`, chamadas com `keyword.arg is None`, funções com `args.kwarg`, subscritos e chamadas `.get/.setdefault/.update/.items/.values/.keys`. A busca textual complementar usou:

```text
contexto_json|merge_contexto|contexto_novo
JsonResponse|resposta_render|render\(
\*\*(kwargs|campos|chaves|extras|ln)
\.(get|pop|setdefault|update|items|values|keys)\(
```

Esse procedimento levanta **sítios candidatos**, não resolve sozinho os pares emissor→consumidor. Não concluí a expansão de todos os chamadores, apelidos, variantes, encaminhamentos integrais e leitores Python/templates/JS; portanto, não apresento sua contagem sintática como universo **[nao verificado]**.

Os sítios concretos ainda sem expansão global fechada são:

| Fronteira | Por que contar literais ou chaves nominais não fecha |
|---|---|
| Contexto por kwargs | Recebe chaves variáveis, compara e persiste o grupo: `app/chamados/services/contexto.py:62`, `:71`, `:76`, `:82`, `:100`, `:102`. |
| Contexto fundido | Percorre qualquer chave recebida e a incorpora: `app/chamados/services/contexto.py:146`, `:154`, `:156`, `:161`. |
| Contexto da disputa | Mescla chaves dinâmicas; o nascimento recebe o contexto inteiro: `app/chamados/services/disputa_emissao.py:548`, `:556`, `:561`, `:590`. |
| Kwargs de decisão | Cada linha recebida vira kwargs do modelo: `app/chamados/models.py:3952`, `:3954`. |
| Leitura catalogada | O nome da chave lida vem do catálogo: `app/chamados/catalogo/modulos.py:225`. |
| Leitura integral | O painel mostra todas as chaves/valores; ausência de leitura nominal não prova escrita morta de `contexto_json`: `app/templates/chamados/partials/painel_chamado_turno.html:35`. |
| Auditoria | Persiste os dicionários integrais recebidos: `app/core/observ.py:94`, `:136`. |

No quadro, caminhos sem prefixo são relativos a **`app/chamados/`**; `T/` significa **`app/templates/chamados/`**.

- **N** = selo específico que prenda a diferença não demonstrado; inexistência global **[nao verificado]**.
- **S1** = existe teste do par, explicado após o quadro; execução verde **[nao verificado]**. [nao verificado]
- **E** = ESTRUTURA: diferença no protocolo do código, pela separação da L-099 (`CLAUDE.md:179`). Em D07, **E\*** indica somente o domínio auditado; não há defeito ao qual atribuir culpa.

| ID | Emissor arquivo:linha | Chave(s) | Consumidor arquivo:linha | Classe | Prova que decide | Selo | Dono |
|---|---|---|---|---|---|---|---|
| D01 | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` | Grava `foto_ausente.batida_id,batida_em`; leitor procura `[motivo].data,ultima_batida.id` | `services/disputa_emissao.py:372`, `:387` | **real** | Contexto do nascimento é preservado em `services/disputa_emissao.py:590`; sem dia, segue para conversa em `:431`, `:439`. Retro agendado com `--apply`: `app/config/crons.py:686`. | N | E |
| D02 | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` | `foto_ausente.batida_id` → `ultima_batida.id` | `models.py:3464` | **real** | Retorna origem `None` em `models.py:3465`; esse resultado decide o dedup em `services/disputa_emissao.py:325`, `:326`. | N | E |
| D03 | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` | Mesma troca de identidade | `app/api/foto_recebida.py:67` | **real** | Sem identidade correspondente, pula a disputa em `app/api/foto_recebida.py:68`. Upload chama o consumidor em `app/api/views.py:858`. | N | E |
| D04 | `services/abertura.py:183`, `:191`; catálogo `catalogo/modulos.py:120` | `ausencia_colab.data`; lote procura `data_turno` | `juizes.py:187`, `:197`, `:226` | **real** | Singular lê catálogo em `juizes.py:114`; lote omite esse degrau. Sem célula, pode contar chamado datado como sem dia: `services/juizes_do_dia.py:94`, `:95`. | N | E |
| D05 | `services/adesao.py:79`, `:86` | `data_inicio,periodo`; lote procura `data_turno` | `juizes.py:187`, `:226` | **real** | Catálogo exige `data_inicio` para esse módulo em `catalogo/modulos.py:126`; leitor do lote não a consulta em `juizes.py:187`. | N | E |
| D06 | `services/abertura.py:183`, `:191` | Não fornece `ausencia_id` | `views.py:1512` | **real** | O problema provado é a nota: quando `analisar()` avança, grava “registro da ausencia #None” em `views.py:1513`, `:1516`. O redirecionamento geral tem alternativa explícita em `:1517`. | N | E |
| D07 | `services/abertura.py:183`, `:191` | Não fornece `ausencia_id` | `juizes.py:2331` | **falso** | Ausência é fonte opcional. A competência já recebe `dia_do_fato`, que lê a data catalogada: `juizes.py:114`, `:2359`, `:2362`. Não depende desse ID para obter a data humana. | N | E* |
| D08 | `services/abertura.py:183`, `:191` | Não fornece `tem_documento,ausencia_id` | `T/partials/_card_atendimento.html:65`, `:70` | **real** | Ausência de `tem_documento` oferece o anexo; ID vazio monta `/api/ausencias//documento/`: `T/partials/_card_atendimento.html:65`, `:70`. | N | E |
| D09 | `reconciliador.py:708`; formato `app/ponto/services/lastro.py:134` | `marcos_faltantes[].motivo='lampada_apagada'` | `juizes.py:551`, `:576` | **real** | Leitor exige motivo da pergunta; qualquer motivo preenchido bloqueia alternativa por tipo. Resultado influencia tipo e hora do plantio: `services/materializacao.py:275`, `:414`. | N | E |
| D10 | `management/commands/nomear_marco_por_lampada.py:61`, `:109` | Mesmo formato de D09 | `juizes.py:551`, `:576` | **real** | Comando entrega a saída de `marcos_apagados`; o leitor trata a etiqueta da lâmpada como motivo da pergunta: `app/ponto/services/lastro.py:134`, `juizes.py:551`. Caminho de escrita condicionado a `--apply`: comando `:97`, `:109`. | N | E |
| D11 | `push_utils.py:28` | `requireInteraction,url`; leitor usa `urgente` e destino fixo | `app/static/sw.js:97`, `:107` | **real** | `requireInteraction=True` não alimenta a opção; clique abre `/` ou apenas focaliza janela: `app/static/sw.js:97`, `:106`, `:107`. | N | E |
| D12 | `glpi_client.py:138` | Ticket sem `urgencia` | `T/partials/tabela_chamados.html:30` | **real** | Formato chega diretamente ao template por `views.py:323`, `:340`; ausência de urgência termina em “Normal”: `T/partials/tabela_chamados.html:37`. | N | E |
| D13 | `glpi_client.py:209` | `data`; não fornece `criado_em,autor_tipo,autor_user` | `T/detalhe.html:83`, `:98`, `:101` | **real** | Transporte direto em `views.py:246`, `:262`; data fica vazia e autor cai em “Colaborador”: `T/detalhe.html:98`, `:101`. | N | E |
| D14 | `views_arquivar.py:62` | `erro` singular → `erros` plural | `T/partials/_lista_chamados.html:134` | **real** | Botão chama desarquivar em `T/partials/_lista_chamados.html:85`; recusa específica vira “Falha ao arquivar” em `:134`. Visualização não exige permissão de agir: `utils.py:71`. | N | E |
| D15 | `services/fio.py:461` | Omite `contador_ativos,contador_msgs_mes,contador_disputas_abertas,cor_disputas_map` | `T/partials/modal_fio.html:14`, `:76` | **real** | Callback renderiza esse contexto em `services/fio.py:494`; contadores somem e cor ausente cai no cinza em `templatetags/chamados_tags.py:107`. Completo fornece os campos em `views.py:945`. | N | E |
| D16 | `views.py:927` | Omite `pode_conferir_data` | `T/partials/modal_fio.html:172` | **real** | GET usa esse contexto em `views.py:956`; usuário autorizado recebe o ramo de bloqueio em `T/partials/modal_fio.html:180`. Parcial calcula a permissão em `services/fio.py:474`. | N | E |
| D17 | `views_cobrar.py:263`, `:265` | `ok,erros,nao_gravados`; JS lê apenas contagem/recusados | `app/templates/core/_copiloto.html:251`, `:254` | **real** | Serviço distingue validação sem gravação em `services/validacao.py:753`, `:756`; outro template mostra esse aviso em `T/partials/_lote_preview.html:9`, mas copiloto o omite. Também não lê `ok/erros`: `app/templates/core/_copiloto.html:252`. | N | E |
| D18 | `services/fio.py:1114` | `fonte` excedente | `management/commands/backfill_decisoes_pergunta.py:553` | **escrita morta** | Consumidor imprime os demais campos com cabeçalho fixo em `management/commands/backfill_decisoes_pergunta.py:554`, `:555`; não usa `fonte`. | N | E |
| D19 | `services/fio.py:1181` | `fonte` excedente | `management/commands/backfill_decisoes_pergunta.py:559` | **escrita morta** | Mesmo descarte no retorno: `management/commands/backfill_decisoes_pergunta.py:560`, `:561`. | N | E |
| D20 | `services/fio.py:1206` | `fonte` excedente | `management/commands/backfill_decisoes_pergunta.py:565` | **escrita morta** | Produção imprime cabeçalho fixo e demais campos em `management/commands/backfill_decisoes_pergunta.py:566`, `:567`. Há leitura de `fonte` em teste: `tests/test_e1_v15h.py:186`. | **Sim, S1** | E |
| D21 | `services/fio.py:1902` | `fonte` excedente | `management/commands/backfill_decisoes_pergunta.py:545` | **escrita morta** | Impressão usa `n,esperado,motivo,pks`, sem `fonte`: `management/commands/backfill_decisoes_pergunta.py:546`, `:547`. | N | E |
| D22 | `services/fio.py:1343` | `merito_recusado,episodios_unidos,sem_episodio_intocada,canonica_sem_episodio` excedentes | `management/commands/backfill_decisoes_pergunta.py:331` | **escrita morta** | Esse chamador e `services/fio.py:2181` extraem só `retratadas`; `services/fio.py:922` ignora o retorno inteiro. Classificação vale para a saída; o plano anterior tem leitores em `services/fio.py:2162`, `:2165`. | N | E |
| D23 | `reconciliador.py:1340` | `rejuntados,perguntas,lastro` excedentes | `management/commands/consumir_eventos_celula.py:31` | **escrita morta** | Interpolação nomeia oito campos, sem esses três: `management/commands/consumir_eventos_celula.py:32`, `:35`. Os efeitos continuam executados: `reconciliador.py:1365`, `:1366`, `:1384`. | N | E |
| D24 | `app/colaboradores/services/desligamento.py:203` | `desligamento_revisao.data_demissao` | `juizes.py:187` | **real** | Chave declarada em `catalogo/modulos.py:141`, lida pelo singular em `juizes.py:114`, ignorada pelo lote que alimenta `services/juizes_do_dia.py:94`. | N | E |
| D25 | `app/colaboradores/services/vinculo.py:419`, `:424` | `escala_colaborador.data_inicio` | `juizes.py:187` | **real** | Declaração em `catalogo/modulos.py:137`; mesma perda no ramo sem célula do lote: `juizes.py:184`, `:187`, `:226`. | N | E |
| D26 | `app/escala/regua_defesa.py:72`, `:76` | `regua_degenerada.data_exemplo` | `juizes.py:187` | **real** | Declaração em `catalogo/modulos.py:133`; emissão chamada pelo detector em `app/ponto/management/commands/detectar_ausencias.py:601`; lote ignora a chave. | N | E |
| D27 | `app/ponto/utils.py:120`, `:122` | `colaborador_em_ferias.data_inicio` | `juizes.py:187` | **real** | Declaração em `catalogo/modulos.py:123`; emissor chamado por `detector_anomalias.py:116`, acionado pela API em `app/api/views.py:434`. | N | E |
| D28 | `app/ponto/management/commands/detectar_ausencias.py:224`, `:231` | `sem_previsao_escala/folga_nao_definida.data_inicio` | `juizes.py:187` | **real** | Chave catalogada em `catalogo/modulos.py:126`; lote sem célula passa por `juizes.py:187`, `:226` sem consultá-la. | N | E |
| D29 | `app/ponto/management/commands/detectar_ausencias.py:294`, `:297` | `ancora_limbo.data_inicio` | `juizes.py:187` | **real** | Chave declarada em `catalogo/modulos.py:131`; mesma omissão do lote, observada pelo contador em `services/juizes_do_dia.py:94`, `:95`. | N | E |
| D30 | `app/ponto/management/commands/detectar_ausencias.py:532` | `sem_previsao_escala/dia_sem_grade.data_inicio` | `juizes.py:187` | **real** | O próprio chamador exige célula inexistente antes de emitir: `app/ponto/management/commands/detectar_ausencias.py:660`, `:668`. Assim, o primeiro degrau do lote não recupera a data emitida. | N | E |

A condição dos nove itens de data é relevante: **célula ligada pode esconder a incompatibilidade**, pois o lote a consulta primeiro; a porta permite nascimento sem essa ligação (`app/chamados/juizes.py:184`, `app/chamados/models.py:1241`, `:1243`). O consumidor que conta “sem dia” seleciona todos os estados vivos, incluindo `registrado` (`app/chamados/services/juizes_do_dia.py:92`, `app/chamados/catalogo/motor.py:24`).

As omissões dos contextos do modal não são preenchidas pelo helper de resposta: ele encaminha o contexto diretamente ao render (`app/core/respostas.py:74`, `:83`). Os processadores configurados também não fornecem esses campos (`app/config/settings/base.py:76`; retornos em `app/core/context_processors.py:154`, `:189`, `:210`, `app/core/context_processors_ia.py:31`, `app/pautas/context_processors.py:35`).

**O anexo errou em D07 e exagerou a conclusão possível de D18–D23.** D07 não demonstra incompatibilidade, porque a data humana já chega pela outra fonte. D18–D23 descrevem campos excedentes de retorno, sem decisão errada demonstrada nos consumidores de produção rastreados; não são funções inteiras mortas (`app/chamados/juizes.py:2359`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:547`, `:555`, `:561`, `:567`, `:331`; `app/chamados/management/commands/consumir_eventos_celula.py:32`).

Também é incorreto generalizar “zero selo de par demonstrado”: **S1 existe em D20**, chama o backfill e verifica sua saída. Ele não exige imprimir `fonte`, portanto não transforma esse metadado excedente em defeito real (`app/chamados/tests/test_e1_v15h.py:194`, `:197`, `:200`, `:201`). Nos demais itens, os testes citados pelo anexo cobrem aspectos separados: contexto canônico em `app/chamados/tests/test_a2_data_evento_batida.py:21`; emissão humana em `app/chamados/tests/test_form_catalogo.py:76`; escritor/leitor de marcos separadamente em `app/chamados/tests/test_emissor_nomeia_o_marco.py:54` e `app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:45`; modal completo em `app/chamados/tests/test_paridade_fio_modal_f8.py:65`. Cobertura global dos pares permanece **[nao verificado]**.

Os totais são **do quadro individualizado**, não do módulo inteiro:

| Classe | D01–D23 | D24–D30 | Total |
|---|---:|---:|---:|
| real | 16 | 7 | **23** |
| escrita morta | 6 | 0 | **6** |
| ramo morto demonstrado | 0 | 0 | **0** |
| falso | 1 | 0 | **1** |
| Itens examinados | **23** | **7** | **30** |

Entre os reais, estas são repetições da mesma causa:

| Itens | Mesma classe de defeito | Prova |
|---|---|---|
| **D01–D03** | Identidade da batida emitida em um caminho e procurada em outro: **CONTRATO-DE-ENTRADA-DO-FIO**, já registrado. | `app/docs/BACKLOG.md:2100`; `app/chamados/models.py:3464`; `app/api/foto_recebida.py:67`. |
| **D04, D05, D24–D30** | Lote omite o degrau da chave de data catalogada que o singular consulta. | `app/chamados/juizes.py:114`, `:187`. |
| **D06, D08** | Emissão humana de ausência consumida como se trouxesse referência a uma entidade `Ausencia`. | `app/chamados/services/abertura.py:183`; `app/chamados/views.py:1516`; `app/templates/chamados/partials/_card_atendimento.html:70`. |
| **D09, D10** | `motivo` significa etiqueta da lâmpada no emissor e motivo da pergunta no leitor. | `app/ponto/services/lastro.py:134`; `app/chamados/juizes.py:551`, `:576`. |
| **D15, D16** | Dois construtores do mesmo contexto entregam conjuntos diferentes de campos ao mesmo template. | `app/chamados/services/fio.py:461`; `app/chamados/views.py:927`; `app/templates/chamados/partials/modal_fio.html:14`, `:172`. |

D11, D12, D13, D14 e D17 continuam como incompatibilidades próprias dos respectivos protocolos; agrupá-las numa única causa além de “contrato divergente” seria **[nao verificado]**. O total global e a afirmação de que não há outros desencontros continuam **[nao verificado]**.

## 13/11 18:36 · sessao 7ecd4aa57676 · RAIA-PREPARACAO · censo · origem=Code

**O universo de `ponto/` continua não fechado.** A revisão dos 15 apontamentos resulta em **7 reais, 1 escrita morta, 2 ramos mortos e 5 falsos**. Acrescento o **ID 16**, escrita morta de `anuencia_erro`; portanto, o recorte abaixo soma **16 itens: 7 reais, 2 escritas mortas, 2 ramos mortos e 5 falsos**, conforme as provas individualizadas. (`app/ponto/services/triagem_batida.py:384`; `app/templates/ponto/triagem/fora_horario.html:3`; demais referências na tabela)

A classificação considera os caminhos de produção presentes na cópia. Ocorrência, frequência e estado efetivamente implantado em produção permanecem **[não verificado]**. A correspondência independente da cópia com o HEAD solicitado também permanece **[não verificado]**, pois não há metadados Git disponíveis nesta árvore.

**1. Universo e método**

| Medida | Resultado |
|---|---|
| Total de costuras do módulo pela regra TODO | **[não verificado]** |
| Total de costuras com desencontro no módulo inteiro | **[não verificado]** |
| Apontamentos classificados neste recorte | **16**, individualizados abaixo |
| Desencontros do recorte, excluídos os falsos | **11**: 7 reais, 2 escritas mortas e 2 ramos mortos; provas abaixo |

Mantive a unidade do anexo: **estrutura × módulo emissor × módulo consumidor**, reunindo os sítios do mesmo módulo; transporte integral conta como consumo opaco, sem provar interpretação das chaves. Essa distinção é necessária porque há cópia integral e também apresentação genérica por iteração. (`app/chamados/services/contexto.py:70`; `app/templates/chamados/partials/painel_chamado_turno.html:35`)

A varredura AST percorreu `app/ponto/**/*.py`, excluindo componentes `tests`, `migrations`, `__pycache__` e nomes `test_*`. Os padrões foram `ast.Dict`, `ast.DictComp`, chamadas `dict(...)`, subscritos em contexto `Store`, chamadas `.update()`/`.setdefault()` e expansões de kwargs (`ast.keyword.arg is None`). O casamento inicial procurou `ImportFrom`, atribuição de retorno de chamada e leituras `.get(<literal>)`/`[<literal>]`; depois conferi manualmente os percursos e os templates. Execução somente em memória, sem artefato persistido: **[não verificado por arquivo:linha]**.

As buscas textuais complementares usaram estes padrões, seguidos de busca pelos nomes das funções e chaves encontradas. **[não verificado por arquivo:linha]**

```bash
rg -n --glob '*.py' --glob '!**/tests/**' --glob '!**/migrations/**' \
  'JSONField|contexto_json|payload|carimbo|\*\*' app/ponto

rg -n \
  'foto_ausente|entrada_timestamp|sem_previsao_escala|entrada_bloqueada_turno_aberto|requireInteraction|urgente|anuencia_erro|volta_url' \
  app
```

**Não demonstrei contador == universo.** Os sítios concretos ainda sem matriz completa de origem→destino são:

| Pendência de fechamento | Sítio que exige rastreamento adicional |
|---|---|
| Contextos preservados e modificados por chaves recebidas dinamicamente | `app/chamados/services/contexto.py:70`, `:97`, `:100`; merge por motivo em `app/chamados/services/disputa_emissao.py:548` |
| Retornos cujo esquema vem de outra função ou de uma exceção transportadora | `app/ponto/services/autorizacao_he_periodo.py:322`, `:325`; composição em `:286` |
| Kwargs que atravessam a porta até o ORM | `app/ponto/registro_batida.py:94`, `:105`, `:153`, `:158` |
| Blobs históricos restaurados sem reconstrução do esquema | `app/ponto/portas/celula.py:824`, `:827`; conteúdo histórico **[não verificado]** |
| Consumo genérico que impede declarar chave órfã apenas por ausência de lookup nominal | `app/templates/chamados/partials/painel_chamado_turno.html:35` |

Esses pontos não tornam o levantamento impossível; mostram onde **este censo ainda não fechou**. Somar construções AST nesses sítios não produziria uma contagem de contratos. (`app/chamados/services/contexto.py:71`; `app/ponto/services/autorizacao_he_periodo.py:325`; `app/ponto/registro_batida.py:153`)

**2. Classificação por item**

Abreviações: `P=app/ponto/`, `C=app/chamados/`, `T=app/templates/`, `A=app/api/`, `S=app/static/`.

Na coluna selo, **“não demonstrado” significa que não comprovei um selo que prenda o par**; inexistência global de selo permanece **[não verificado]**. Os testes citados mostram os limites encontrados. [nao verificado]

O dono **ESTRUTURA** refere-se ao contrato entre componentes, não ao cadastro ou à batida de alguma pessoa. Nos falsos, não há divergência de dado atribuída. ([L-099, `CLAUDE.md:179`](/home/ronald/codex_ws/e439fac78949a3cd5256574999a3a58831141d47/CLAUDE.md:179))

| ID | Emissor arquivo:linha | Chave(s) | Consumidor arquivo:linha | Classe | Prova que decide | Selo que prende | Dono L-099 |
|---|---|---|---|---|---|---|---|
| **1** | `P/management/commands/marcar_foto_ausente_retro.py:98` | Grava `foto_ausente.batida_id/batida_em`; leitor busca `[motivo].data` e `ultima_batida.id` | `C/services/disputa_emissao.py:372`, `:387` | **real** | O contexto entra sem tradução em `:662`. Sem dia, o gerador pode abrir conversa em `:431`, `:439`; não lê a referência específica gravada. O comando está registrado com `--apply`: `app/config/crons.py:686`. | **Não demonstrado.** `P/tests/test_bug126_selfie_invisivel.py:70` verifica a chave produzida, não a interpretação pelo gerador. | ESTRUTURA |
| **2** | Mesmo emissor, `:98` | `foto_ausente.batida_id` versus `ultima_batida.id` | `C/models.py:3464` | **real** | O derivador retorna `None` sem `ultima_batida.id`: `:3465`. Participa do nascimento em `:3450`; a identidade por episódio exige origem preenchida em `:3490`. | **Não demonstrado.** O teste localizado termina verificando o contexto: `P/tests/test_bug126_selfie_invisivel.py:71`. | ESTRUTURA |
| **3** | Mesmo emissor, `:98` | `foto_ausente.batida_id` versus `ultima_batida.id` | `A/foto_recebida.py:67` | **real** | A comparação que decide tratar a disputa só aceita `ultima_batida.id == batida.pk`: `:67`. Os uploads chamam esse helper: `A/views.py:856`, `A/views_core.py:840`. | **Não demonstrado [não verificado].** | ESTRUTURA |
| **4** | `P/management/commands/processar_alertas_turno.py:348` | `data_turno,entrada_timestamp,posto` | Helper apontado no anexo: `C/services/disputa_emissao.py:54` | **falso** | O emissor chama a fábrica em `processar_alertas_turno.py:385`. Esse percurso lê `data_turno` por `C/data_evento.py:27`, chamado em `disputa_emissao.py:1232`, e entrega a data à pergunta em `:1244`. O helper citado no anexo não é a entrada desse percurso. | **Não demonstrado para o emissor.** Parcial da fábrica: `C/tests/test_cenarios_disputa.py:89`, `:96`. | ESTRUTURA |
| **5** | `P/management/commands/processar_alertas_avancados.py:126` | `data_turno,entrada_timestamp` | Mesmo helper apontado: `C/services/disputa_emissao.py:54` | **falso** | Também chama a fábrica: `processar_alertas_avancados.py:159`. A leitura correta está em `C/services/disputa_emissao.py:1232`, com reconhecimento de `data_turno` em `C/data_evento.py:27`. | **Não demonstrado para o emissor.** Parcial da fábrica: `C/tests/test_cenarios_disputa.py:89`. | ESTRUTURA |
| **6** | `P/management/commands/emitir_turno_aberto_retroativo.py:128`, `:156` | `data_turno,entrada_timestamp` | Mesmo helper apontado: `C/services/disputa_emissao.py:54` | **falso** | O comando abre chamado em `:151`. O processamento do passivo encaminha chamados à fábrica em `C/services/pergunta_no_app.py:238`, `:246`; ela lê a data em `C/services/disputa_emissao.py:1232`. | **Não demonstrado para o percurso completo.** `P/tests/test_passe_turno_aberto_retroativo.py:92` cobre o nascimento. | ESTRUTURA |
| **7** | `P/management/commands/detectar_ausencias.py:231`, `:532` | `sem_previsao_escala.data_inicio` | `C/catalogo/modulos.py:126`, `:225` | **falso** | O catálogo declara uma chave de data, não “início do vínculo”: `modulos.py:10`. O segundo nascimento agrega dias sem grade por colaborador e começa pela lacuna: `detectar_ausencias.py:501`, `:506`, `:532`, `:538`. O significado incompatível afirmado pelo anexo não está demonstrado nesse consumidor. | **Não demonstrado.** O teste genérico do catálogo não distingue esses nascimentos: `C/tests/test_f4_catalogo.py:51`. | ESTRUTURA |
| **8** | `P/views.py:280`, `:288`, `:301` | Grava `pendentes,total`; falta `data_turno` | `C/catalogo/modulos.py:182`, `:225` | **real** | O módulo pertence a `FURO_MODULES`: `C/catalogo/furo_modulos.py:15`. O cartório pergunta a data e só inclui o chamado quando ela existe: `P/services/cartorio.py:135`, `:137`. A criação não acrescenta essa chave: `C/models.py:1068`, `:1071`. | **Não demonstrado.** O selo do cartório fabrica `data_turno` no fixture: `P/tests/test_cartorio_so_chamado_de_batida.py:43`, `:45`. | ESTRUTURA |
| **9** | `P/webpush.py:44` | `url`; ausência de `urgente` | `S/sw.js:96`, `:102` | **escrita morta** | `url` segue opacamente em `options.data`, mas o clique só foca uma janela ou abre `/`: `sw.js:105`, `:107`. Este produtor não pede interação obrigatória; `urgente=false` é o default explícito do leitor: `:97`. | **Não demonstrado.** O teste localizado substitui o emissor e não executa o SW: `app/holerite/tests/test_selo_sem_notificacao.py:375`. | ESTRUTURA |
| **10** | `P/management/commands/processar_alertas_avancados.py:207`, `:240` | `requireInteraction=True`; leitor pede `urgente`; `url` sem efeito | `S/sw.js:97`, `:102` | **real** | Os dois payloads pedem interação obrigatória em `:211`, `:244`; o SW decide `false` pela chave diferente em `sw.js:97`. Há chamadores em `processar_alertas_avancados.py:82`, `:144`, `:190` e cron em `app/config/crons.py:363`. | **Não demonstrado.** Teste próximo substitui o envio: `P/tests/test_avancados_grade.py:18`. | ESTRUTURA |
| **11** | `P/management/commands/processar_alertas_turno.py:279` | `requireInteraction`; leitor pede `urgente`; `url` sem efeito | `S/sw.js:97`, `:102` | **real** | Para `pos_10/pos_20`, o produtor envia `True`: `processar_alertas_turno.py:283`. O SW decide `false`: `sw.js:97`. Chamador em `processar_alertas_turno.py:244`; cron em `app/config/crons.py:362`. | **Não demonstrado.** Teste próximo substitui o envio: `P/tests/test_alertas_catalogo_premissa.py:99`. | ESTRUTURA |
| **12** | `P/views.py:1087` | Faltam `empresa_id,periodo_fechado` | `T/ponto/_fech_linha.html:13`, `:42`, `:47`, via `_fech_lote.html:8` | **real** | O lote fornece apenas quatro raízes: `views.py:1088`. O include usa as ausentes para apresentação e POST; a página principal as fornece em `views.py:1181`. Rota do lote: `P/urls.py:29`. O POST vazio afeta o retorno em `views.py:1443`, `:1457`. | **Não demonstrado.** `P/tests/test_selo_lotes_fechamento.py:57`, `:66`, `:81` verificam quantidade, ordem e continuação. | ESTRUTURA |
| **13** | `P/services/triagem_batida.py:206` | `tem_bloqueio_turno` versus `entrada_bloqueada_turno_aberto` | `P/views.py:229` | **ramo morto** | O contexto nasce local em `views.py:249` e recebe a triagem em `:260`; não recebe a flag do seletor. O ramo `turno_regularizar` fica sem ativação. A flag efetivamente produzida já tem consumidor vivo em `views.py:275`, que prossegue o registro em `:312`. | **Não demonstrado.** `P/tests/test_f7_triagem_caracterizacao.py:78` verifica a flag produzida, não a seleção do template. | ESTRUTURA |
| **14** | `P/services/triagem_batida.py:758` | Contexto de sucesso; `saida_sem_entrada` é escrita em outro dict | `T/ponto/triagem/apto.html:39` | **ramo morto** | O alerta está no `else` de `sucesso`: `apto.html:4`, `:37`. O registro define `sucesso=True`: `triagem_batida.py:758`. A flag aninhada de `:738` vai à disputa, sem entrar no contexto da tela. | **Não demonstrado.** O teste localizado verifica disputa, não o alerta: `P/tests/test_saida_cross_midnight_view.py:49`. | ESTRUTURA |
| **15** | `P/views.py:2079`; complementos `:2032`, `:2046` | Ausência de `volta_url` | `T/ponto/partials/painel_ausencia.html:282` | **falso** | O endpoint aceita `volta` vazio e escolhe expressamente a lista: `views.py:2581`. A própria lista abre o painel: `T/ponto/ausencias.html:217`, `:228`. Exigência de preservar filtros ou outro destino **[não verificado]**. | **Não demonstrado.** `P/tests/test_drawer_ausencia_no_molde.py:191` verifica rodapé, não esse retorno. | ESTRUTURA |
| **16 — adicional** | `P/services/triagem_batida.py:384`, `:387` | `anuencia_erro` | Contexto transportado por `P/views.py:343` para `T/ponto/triagem/fora_horario.html:1` | **escrita morta** | O template decide por `anuencia_enviada` e apresenta `bloqueio_horario`, sem exibir o erro: `fora_horario.html:3`, `:15`, `:38`. A herança começa em `base_triagem.html:1`; os includes da base estão em `T/base.html:92`, `:388`, `:432`. Não foi encontrado leitor dessa chave nesse percurso. | **Não demonstrado.** `P/tests/test_f7_ramos_frios.py:125` afirma a chave no contexto; não afirma mensagem renderizada. | ESTRUTURA |

Nos IDs **10 e 11**, a classe única atribuída é **real** pela decisão de interação incorreta; a URL também fica sem efeito, mas não constitui uma segunda classificação desses mesmos IDs. (`app/static/sw.js:97`, `:105`, `:107`)

**3. Totais e famílias dos reais**

| Classe | IDs | Total |
|---|---|---:|
| **real** | 1, 2, 3, 8, 10, 11, 12 | **7** — referências dos respectivos pares acima |
| **escrita morta** | 9, 16 | **2** — `app/static/sw.js:105`; `app/templates/ponto/triagem/fora_horario.html:3` |
| **ramo morto** | 13, 14 | **2** — `app/ponto/views.py:229`; `app/templates/ponto/triagem/apto.html:37` |
| **falso** | 4, 5, 6, 7, 15 | **5** — `app/chamados/services/disputa_emissao.py:1232`; `app/chamados/catalogo/modulos.py:225`; `app/ponto/views.py:2581` |

Os sete reais se agrupam em **quatro classes específicas de defeito**:

| Família | IDs | Prova |
|---|---|---|
| **Referência do episódio não entregue aos leitores canônicos — CONTRATO-DE-ENTRADA-DO-FIO** | **1, 2, 3** | Registro existente em `app/docs/BACKLOG.md:2100`; escritor em `P/management/commands/marcar_foto_ausente_retro.py:98`; leitores em `C/services/disputa_emissao.py:387`, `C/models.py:3464`, `A/foto_recebida.py:67`. |
| **Data canônica do chamado ausente no nascimento** | **8** | `P/views.py:280`; declaração e leitura em `C/catalogo/modulos.py:182`, `:225`; efeito em `P/services/cartorio.py:137`. |
| **Nome divergente da opção de interação WebPush** | **10, 11** | Escrita de `requireInteraction` em `P/management/commands/processar_alertas_avancados.py:211` e `processar_alertas_turno.py:283`; leitura de `urgente` em `S/sw.js:97`. |
| **Render parcial omite raízes exigidas pelo include** | **12** | `P/views.py:1087`; consumo em `T/ponto/_fech_linha.html:13`, `:42`, `:47`. |

O anexo errou ao apresentar **4–7 e 15** como desencontros confirmados e ao não separar os ramos mortos **13–14**. Permanece sem comprovação a quantidade de costuras e de desencontros **fora deste recorte**. (`app/chamados/services/disputa_emissao.py:1232`; `app/chamados/catalogo/modulos.py:225`; `app/ponto/views.py:2581`, `:229`; `app/templates/ponto/triagem/apto.html:37`; completude global **[não verificado]**)

## 13/11 18:34 · sessao 721067d32d25 · RAIA-PREPARACAO · censo · origem=Code

**O universo não ficou fechado.** Total de costuras no módulo e total global com desencontro: **[nao verificado]**. Nos **20 apontamentos examinados**, a classificação é: **1 real, 14 escritas mortas, 0 ramos mortos e 5 falsos**; são subtotais deste levantamento, fundamentados nos pares abaixo.

A identidade da cópia com o commit `e439fac78` permanece **[nao verificado]**: o diretório fornecido não contém metadados Git. Execução dos selos e ocorrência dos caminhos em produção: **[nao verificado]**.

**Contagem e limite do universo.** Adotei como unidade o par **emissor de uma estrutura → consumidor em outro arquivo**, agrupando suas chaves; dois consumidores geram dois pares, como nas linhas de nome canônico e rótulo da régua (`app/escala/models.py:486-522`; `app/core/templatetags/hasner_filters.py:72`; `app/colaboradores/services/ficha.py:82`; `app/escala/regua_defesa.py:632`; `app/chamados/juizes.py:516`).

A varredura sintática em memória encontrou **61 arquivos, 435 `ast.Dict`, 45 `ast.DictComp`, 32 chamadas `dict(...)` e 18 chamadas com expansão `**kwargs`**. Esses números **não são contagens de costuras**; equivalência com o universo: **[nao verificado]**. O recorte e os padrões foram:

```python
ps = [
    p for p in Path("app/escala").rglob("*.py")
    if not set(p.parts) & {"tests", "migrations", "__pycache__"}
]
# ast.parse(p.read_text()); ast.walk(tree)
# isinstance(n, ast.Dict)
# isinstance(n, ast.DictComp)
# Call com func=Name(id="dict")
# Call com algum keyword.arg is None
```

O rastreamento complementar usou `rg -n` pelos nomes dos produtores, campos, templates e chamadas, incluindo Python, HTML e JS; isso encontra referências nominais, mas o fechamento dos repasses dinâmicos permanece **[nao verificado]**.

Os sítios concretos que impedem apresentar esse contador como universo fechado são:

| Sítio | Pendência de fechamento |
|---|---|
| `app/escala/services/cadastro_tipo.py:265-267` | Persiste HPD recebido integralmente; contar apenas chaves literais do escritor perde esse conteúdo. |
| `app/escala/services/fase_12x36.py:85-94,211-214` | Persiste `detalhe` recebido e depois devolve o snapshot integral com metadados; exige rastrear a origem e os repasses. |
| `app/escala/services/cadastro_realidade.py:898-900` | Funde assinatura, campos comuns e `_extra`; um construtor sintático não representa sozinho o contrato final. |
| `app/escala/services/leitor_celula.py:418-440` | Incorpora chave importada e acrescenta campos condicionais ao dia. |
| `app/escala/signals.py:178-182`; `app/chamados/models.py:2042-2046` | O contexto de disputa é incorporado integralmente à anotação; ausência de leitura nominal de uma chave não prova escrita morta. |

O par de carimbos também pertence à busca: `regenerada_em/editada_em` são emitidos e expandidos pelo consumidor, com assinatura correspondente; **nenhum desencontro foi demonstrado nesse par** (`app/escala/services/regeneracao.py:104-105,141`; `app/ponto/management/commands/supra_juiz.py:275-276`). O outbox declara escritor externo a `escala/` e ausência de payload; a localização do model não o torna emissor desse módulo (`app/escala/models.py:1848-1855`).

**Classificação dos apontamentos.** Como o anexo não traz IDs explícitos, usei **01–17 na ordem das linhas marcadas como desencontro**, excluindo “Mesmo resultado”, que já declarava ausência de desencontro; correspondência com IDs de algum arquivo original: **[nao verificado]**.

Na coluna de selo, **N** significa “selo específico não localizado **[nao verificado]**”, sem afirmar sua inexistência. **ESTRUTURA\*** identifica o problema de contrato do código; o dono L-099 de uma ocorrência dia-colaborador permanece **[nao verificado]**, pois depende dos insumos do juiz (`CLAUDE.md:179`; `app/ponto/management/commands/e6_oraculo.py:98-110`). Para os falsos, não atribuo dono de defeito.

| ID | Emissor arquivo:linha | Chave(s) | Consumidor arquivo:linha | Classe | Prova que decide | Selo que prende | Dono |
|---|---|---|---|---|---|---|---|
| 01 | `app/escala/services/propositor.py:788` | Ausentes `ok/msg`; emite `decisao_id/saida` | `app/chamados/services/proposta_no_fio.py:151-155` | **real**, quanto a `ok` | O escritor é resolvido diretamente; `ok` ausente participa da condição viva que devolve sucesso (`app/core/intake.py:49-52,481-487`; `app/chamados/services/proposta_no_fio.py:152-155`). | **N**. Teste substitui o escritor por `{'ok': True}` (`app/chamados/tests/test_proposta_no_fio.py:106-120`). | ESTRUTURA* |
| 02 | `app/escala/services/propositor.py:765-770` | `schema` | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:53-63` | **escrita morta** | Só `handle` lê `schema`; o emissor entra deliberadamente em `_rodar` para conservar o tenant (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:48-51`; `app/escala/services/propositor.py:760-764`). | **N**. Teste adjacente de aplicação: `app/escala/tests/test_porta_da_proposta.py:148-166`. | ESTRUTURA* |
| 03 | `app/escala/views_wizard.py:20-59` | `descricao`; HPD condicional | `app/escala/services/cadastro_tipo.py:185-188,217,265-267` | **falso** | Descrição deriva do nome; HPD é emitido quando enviado e só é atualizado quando presente (`app/escala/services/cadastro_tipo.py:185-188,265-267`; `app/escala/views_wizard.py:53-59`). | **Sim, parcial:** POST sem/com HPD (`app/escala/tests/test_wizard_views.py:28-49,80-90`). Preservação exata na omissão: **[nao verificado]**. | — |
| 04 | `app/escala/services/escala_auto_executor.py:65-84` | `_delta_blocos` | `app/escala/services/cadastro_tipo.py:443-454` | **falso** | Zero conserva blocos; somente A7 pede deslocamento. A4 altera pausa e FOLGA altera dia da semana (`app/escala/services/escala_auto_executor.py:68-83`; `app/escala/services/cadastro_tipo.py:443,451-454`). | **Sim, parcial:** A4 real sem delta (`app/escala/tests/test_escala_auto_executor.py:66-84`). Assert específico dos blocos: **[nao verificado]**. | — |
| 05 | `app/escala/services/escala_auto.py:111-115` | `valores.hfi` | `app/escala/services/escala_auto_executor.py:73-81` | **escrita morta** | Executor lê `hii` e calcula o fim pela duração declarada; a razão está documentada (`app/escala/services/escala_auto_executor.py:74-80`). Usar a mediana em `para/evidencia` não lê essa chave (`app/escala/services/escala_auto.py:112-115`). | **N**. O caso existente faz duração e `hfi` coincidirem (`app/escala/tests/test_escala_auto_executor.py:49-53,78-79`). | ESTRUTURA* |
| 06 | `app/escala/services/escala_auto_executor.py:114-115` | `ec_antigo` | `app/escala/management/commands/ajustar_escalas_auto.py:95-99` | **escrita morta** | O chamador já dispõe de `ec_id` e lê os outros cinco campos do retorno (`app/escala/management/commands/ajustar_escalas_auto.py:93-99`). | **N**. Teste lê `ec_novo/historico` (`app/escala/tests/test_escala_auto_executor.py:72-84`). | ESTRUTURA* |
| 07 | `app/escala/services/escala_auto_executor.py:152` | `historico` de desfazer | `app/escala/management/commands/ajustar_escalas_auto.py:39-42` | **escrita morta** | Chamador lê `volta/celulas`; imprime o histórico original `h.pk`, sem ler o histórico retornado (`app/escala/management/commands/ajustar_escalas_auto.py:35-42`). | **N**. Teste de desfazer também ignora o retorno (`app/escala/tests/test_escala_auto_executor.py:86-101`). | ESTRUTURA* |
| 08 | `app/escala/services/propositor.py:644-650` | Ausência de `alavanca` | `app/pautas/semanal.py:139-146` | **falso** | Contrato explícito: usar alavanca quando declarada; senão, `n`, que o emissor fornece (`app/pautas/semanal.py:142-146`; `app/escala/services/propositor.py:645`). | **Sim, parcial:** pauta com/sem alavanca (`app/pautas/tests/test_pauta_texto.py:113-136`). Travessia deste emissor: **[nao verificado]**. | — |
| 09 | `app/escala/services/fase_12x36.py:229-234` | Ausência de `alavanca` | `app/pautas/semanal.py:139-146` | **falso** | Mesmo contrato de fallback para `n`; o produtor fornece `n` (`app/pautas/semanal.py:142-146`; `app/escala/services/fase_12x36.py:230`). | **Sim, parcial:** alavanca na pauta e fase pela lavra (`app/pautas/tests/test_pauta_texto.py:129-136`; `app/escala/tests/test_o261_v_fase_na_lista.py:254-260`). Travessia específica: **[nao verificado]**. | — |
| 10 | `app/escala/models.py:486-522` | `ciclico` | `app/core/templatetags/hasner_filters.py:72-123,207-217` | **escrita morta** | Filtros, dropdown e ficha usam as outras quatro partes (`app/core/templatetags/hasner_filters.py:72-123`; `app/escala/models.py:528-536`; `app/colaboradores/services/ficha.py:82-90`). | **N**. Há assert da emissão, não de consumo (`app/escala/tests/test_nome_canonico.py:85,95`). | ESTRUTURA* |
| 11 | `app/escala/models.py:486-522` | Mesmo `ciclico` de 10 | `app/colaboradores/services/ficha.py:70-90` | **escrita morta** | A ficha extrai apenas `tipo/apelido/dias/horario`; é a mesma escrita do ID10 (`app/colaboradores/services/ficha.py:82-90`). | **N**. Cabeçalho testado sem prender essa chave (`app/colaboradores/tests/test_ficha_cabecalho_compacto.py:28-40`). | ESTRUTURA* |
| 12 | `app/escala/regua_defesa.py:632-633` | `faixa/escore/texto` | `app/escala/services/furos_diarios.py:36,305-309` | **escrita morta** | Leitores usam existência e `classe`; os três campos restantes não participam dessas decisões (`app/escala/services/furos_diarios.py:36,306-309`; `app/chamados/juizes.py:516`). | **N**. Teste do produtor afirma campos do rótulo (`app/escala/tests/test_termometro_regua.py:175-188`). | ESTRUTURA* |
| 13 | `app/escala/regua_defesa.py:632-633` | Mesmos `faixa/escore/texto` de 12 | `app/chamados/juizes.py:514-516` | **escrita morta** | Este leitor só distingue `None`; entretanto, **`classe` tem outro leitor**, portanto não integra as chaves mortas (`app/escala/services/furos_diarios.py:306-309`). | **N**; teste do produtor em `app/escala/tests/test_termometro_regua.py:175-188`. | ESTRUTURA* |
| 14 | `app/escala/regua_defesa.py:313` | Ausência de `data_ancora/confirm_txt` | `app/colaboradores/services/vinculo.py:57-59,146` | **falso** | Destino cíclico é recusado antes do POST; exigência de âncora é para criação cíclica. `confirm_txt` é observação opcional (`app/escala/regua_defesa.py:310-313`; `app/colaboradores/services/vinculo.py:126-130,354-356`). | **Sim:** sucesso não cíclico e recusa cíclica pela porta (`app/escala/tests/test_termometro_camada3.py:81-106`). | — |
| 15 | `app/escala/views.py:294-299` | `tamanhos.tamanho/dias` | `app/templates/escala/folga_calendarios_lista.html:21-25` | **escrita morta** | Contexto segue ao template, que só lê `t.canonico` desse item (`app/escala/views.py:311`; `app/templates/escala/folga_calendarios_lista.html:22`). | **N**. Teste inspeciona outro campo do contexto (`app/escala/tests/test_editor_folga_modo.py:66-71`). | ESTRUTURA* |
| 16 | `app/escala/views.py:363-371` | `tipos`, `tamanhos.dias_range` | `app/templates/escala/folga_calendario_editar.html:67-80` | **escrita morta** | Seed usa `tamanho/rotulo/marcados`; JS constrói o intervalo por `d <= t.n` (`app/templates/escala/folga_calendario_editar.html:68,80`). | **N** para essas chaves. Existe smoke de JS e POST omitido pelo anexo (`app/escala/tests/test_smoke_clique_folga_calendario.py:7-20,163-170`). | ESTRUTURA* |
| 17 | `app/escala/views_plano_folgas.py:158,190-191` | `gravadas/celulas_relavradas/barrados`; erro sem `ok` | `app/templates/escala/plano_folgas.html:174-189` | **escrita morta** | Os três campos de sucesso não são usados. A ausência de `ok` no erro é **falso parcial**: `!j.ok` entra no tratamento de `j.erro` (`app/templates/escala/plano_folgas.html:176-189`). | **N** para os três campos; teste prende erros de item (`app/escala/tests/test_bug136_plano_em_lote_relavra.py:75-82`). | ESTRUTURA* |
| 18 | `app/escala/services/impacto_troca.py:47-48` | `de/ate/furos_antes/furos_depois/dias_furo_antes/dias_furo_depois` | `app/colaboradores/services/vinculo.py:141-147,355-360` | **escrita morta** | Consumidores finais usam somente `frase`, sem serializar o dict integral (`app/colaboradores/services/vinculo.py:356,360`). | **N** para os seis campos; integração prende frase no aviso/trilha (`app/colaboradores/tests/test_troca_escala_porta.py:63-75`). | ESTRUTURA* |
| 19 | `app/escala/services/fase_na_tela.py:206-212` | `dias_furo_antes/dias_furo_depois` | `app/colaboradores/views_fase.py:98-115` → `app/templates/colaboradores/partials/_fase_12x36.html:6-39` | **escrita morta** | Templates usam contagens, semanas e células; o include também não lê as listas (`app/templates/colaboradores/partials/_calendario_fase.html:5-15`). | **N** para consumo; teste lê as listas diretamente no produtor (`app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:177-188`). | ESTRUTURA* |
| 20 | `app/escala/services/semana_do_posto.py:201-214` | `titular_id/substituto_id/candidatos` | `app/api/views_mensageria.py:2120-2133` | **escrita morta** | API reconstrói a proposta sem esses campos; outro consumidor apenas conta propostas (`app/api/views_mensageria.py:2129-2133`; `app/escala/services/semana_do_posto.py:237-241`). | **N** para consumo; testes afirmam emissão dos IDs/candidatos (`app/escala/tests/test_semana_do_posto.py:149-168`). | ESTRUTURA* |

As classificações de escrita morta valem para os consumidores estáticos rastreados na cópia; consumidores adicionais por resolução dinâmica permanecem **[nao verificado]**.

No **ID01**, a classificação real segue literalmente o critério pedido: `ok` não chega e participa de uma decisão viva. **Não demonstra falso sucesso ocorrido em produção [nao verificado]**. Já `msg` está subordinado ao ramo de recusa, inalcançável com o retorno atual, sempre não vazio; isso não transforma a leitura viva de `ok` em ramo morto (`app/escala/services/propositor.py:788`; `app/chamados/services/proposta_no_fio.py:152-155`).

**Totais dos quadros, sem extrapolação ao módulo:**

| Recorte | real | escrita morta | ramo morto | falso |
|---|---:|---:|---:|---:|
| IDs 01–17 | 1 | 11 | 0 | 5 |
| Adicionais 18–20 | 0 | 3 | 0 | 0 |
| **20 apontamentos examinados** | **1** | **14** | **0** | **5** |

Esses totais contam linhas; **10=11** repetem a mesma escrita de `ciclico`, e **12=13** repetem a mesma escrita de `faixa/escore/texto` (`app/escala/models.py:486-522`; `app/escala/regua_defesa.py:632-633`).

O anexo errou ao tratar as omissões opcionais dos IDs **03, 04, 08, 09 e 14** como desencontros, e ao misturar no ID17 a ausência correta de `ok` com campos efetivamente sem consumo; as provas estão nas respectivas linhas. Também exagerou o “restante” do rótulo: `classe` possui leitor (`app/escala/services/furos_diarios.py:306-309`).

Entre os **reais desta auditoria**, há somente o **ID01**. Ele e o exemplo `foto_ausente.batida_id` pertencem ao mesmo agrupamento geral de contrato de entrada do fio: nomes emitidos e procurados divergem. São contratos concretos distintos: resultado de aplicação no ID01; identidade da batida no exemplo, cujo emissor está fora de `escala/` (`app/escala/services/propositor.py:788`; `app/chamados/services/proposta_no_fio.py:152`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:98-103`; `app/chamados/models.py:3464`). Não há segundo real comprovado neste levantamento para consolidar como duplicata do ID01; completude dessa conclusão no módulo: **[nao verificado]**.

## 13/11 18:34 · sessao 289e7353a8e6 · - · - · origem=Ronald (tmux)

D01–D10: **9 reais, 1 falso (D07)**, segundo os caminhos estáticos abaixo; ocorrência efetiva em produção [nao verificado].

| ID | Emissor / chaves | Consumidor | Classe / prova |
|---|---|---|---|
| D01 | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`: `foto_ausente.batida_id,batida_em,tipo_batida,_por` | `app/chamados/services/disputa_emissao.py:372`, `:387` | **real**. O nascimento persiste esse contexto integral em `disputa_emissao.py:590`; a ausência de `[motivo].data` e `ultima_batida.id` deixa `_dia_p` sem data e abre conversa em `:431–441`. O retro está agendado com `--apply` em `app/config/crons.py:686`. |
| D02 | Mesmo emissor | `app/chamados/models.py:3464` | **real**. `batida_origem_do_nascimento` lê exclusivamente `ultima_batida.id`, retorna `None` em `models.py:3465–3466`; o valor é usado no dedup em `disputa_emissao.py:325–329` e na origem da pergunta em `models.py:3450–3451`. |
| D03 | Mesmo emissor | `app/api/foto_recebida.py:67` | **real**. O consumidor compara `ultima_batida.id` com a batida e pula a disputa em `:68–69`; o upload chama esse consumidor em `app/api/views.py:856–858` e `app/api/views_core.py:840–842`. |
| D04 | `app/chamados/services/abertura.py:183–191`: `data` para `ausencia_colab`, pela declaração `app/chamados/catalogo/modulos.py:120` | `app/chamados/juizes.py:187`, `:197`, `:226` | **real**, quando não há célula ligada. O singular consulta catálogo em `juizes.py:114`; o lote só consulta `data_turno` em `:187`. A ligação de célula pode devolver `None` (`models.py:1241–1243`). Há consumidor vivo do lote que conta chamados sem dia em `services/juizes_do_dia.py:92–99`. |
| D05 | `app/chamados/services/adesao.py:79–87`: `data_inicio,periodo,adesao` | Mesmo lote de D04 | **real**, sob a mesma condição de célula ausente. Catálogo declara `data_inicio` em `catalogo/modulos.py:126`; o lote ignora essa chave em `juizes.py:187`. |
| D06 | Emissão humana de D04, sem `ausencia_id` | `app/chamados/views.py:1512` | **real pelo texto produzido**, embora o redirecionamento tenha alternativa explícita correta. Antes da alternativa, `views.py:1516` grava “abriu o registro da ausencia #None para validar”; sem ID segue à lista geral em `:1517–1519`. |
| D07 | Mesma emissão humana | `app/chamados/juizes.py:2331` | **falso**. `_dia_da_ausencia` é uma fonte opcional: `competencia_do_chamado` já chama `dia_do_fato` em `:2359–2361` e só acrescenta a ausência se existir em `:2362–2364`. O singular lê a chave catalogada `data` em `:114`. Ausência de ID não demonstra perda de competência. |
| D08 | Mesma emissão humana, sem `tem_documento,ausencia_id` | `app/templates/chamados/partials/_card_atendimento.html:65`, `:70` | **real**. O cartão oferece comprovante quando `tem_documento` falta e monta `/api/ausencias//documento/` com o ID vazio em `:70`. A página inclui o cartão em ambas as abas (`app/templates/chamados/meu_atendimento.html:127`, `:144`). |
| D09 | `app/chamados/reconciliador.py:708`, recebendo `motivo='lampada_apagada'` de `app/ponto/services/lastro.py:134` | `app/chamados/juizes.py:551`, `:576` | **real**, colisão semântica. O leitor compara `motivo` ao motivo da pergunta e qualquer motivo preenchido bloqueia alternativa por tipo. A saída alimenta o tipo do plantio em `services/materializacao.py:275–277` e a escolha da hora de intervalo em `:414–416`. Há alternativas pela grade em alguns consumidores (`juizes.py:1861–1862`, `:1988–1990`), mas elas não tornam os dois significados equivalentes. |
| D10 | `app/chamados/management/commands/nomear_marco_por_lampada.py:109`: mesmo formato de D09, obtido em `:61` | Mesmo leitor de D09 | **real**, mesma colisão. O comando tem caminho `--apply` em `:97–109`; é ferramenta de acervo, explicitamente não rotina, em `app/config/crons.py:1353–1358`. Execução passada [nao verificado]. |

A emissão humana `ausencia_colab` é alcançável: rota `app/chamados/urls.py:10`; POST para colaborador em `views.py:164–165`; tipo vindo do catálogo em `views.py:99–101`; chamada da abertura em `views.py:135–140`; declaração humana de ausência em `catalogo/modulos.py:120–122`. Portanto D06/D08 não dependem de inventar um emissor ou supor uma rota ausente.

Outros emissores canônicos de `ultima_batida` existem (`app/api/views.py:758–765`, `app/api/views_core.py:752`), mas **não corrigem o retro**: o nascimento do retro recebe só sua estrutura; no reuso, o merge conserva campos anteriores e pode deixar a identidade de outra batida (`app/chamados/services/disputa_emissao.py:548–561`). D01–D03 pertencem à classe já registrada **CONTRATO-DE-ENTRADA-DO-FIO**, com esse nome em `app/docs/BACKLOG.md:2100`; D03 também tem registro específico em `BACKLOG.md:1720`.

**Sete pares adicionais concretos da classe D04/D05**, para numeração após os itens já reservados pelo consolidador:

| Emissor | Chave ignorada pelo lote | Alcance estático |
|---|---|---|
| `app/colaboradores/services/desligamento.py:202–207` | `data_demissao`; módulo `desligamento_revisao` em `:201`; catálogo `modulos.py:141` | Rota `app/colaboradores/urls.py:52` → `views.py:1437`. |
| `app/colaboradores/services/vinculo.py:419–424` | `data_inicio`; módulo `escala_colaborador` em `:418`; catálogo `modulos.py:137` | `views.py:423` → `executar_vinculo`; modo aceite chama `_executar_aceite` em `services/vinculo.py:385–386`. |
| `app/escala/regua_defesa.py:72–76` | `data_exemplo`; módulo `regua_degenerada` em `:53`; catálogo `modulos.py:133` | Chamadores em `app/ponto/management/commands/detectar_ausencias.py:600–601` e `reconciliar_grade.py:494–495`. |
| `app/ponto/utils.py:120–123` | `data_inicio`; módulo `colaborador_em_ferias` em `:118`; catálogo `modulos.py:123` | `app/chamados/detector_anomalias.py:115–116` ← `app/api/views.py:433–434` / `views_core.py:419–420`. |
| `app/ponto/management/commands/detectar_ausencias.py:224–231` | `data_inicio`; `sem_previsao_escala/folga_nao_definida` em `:193`, `:222` | Cron `app/config/crons.py:364`. |
| Mesmo arquivo `:294–297` | `data_inicio`; `ancora_limbo` em `:274`; catálogo `modulos.py:131` | Mesmo cron. |
| Mesmo arquivo `:532–534` | `data_inicio`; `sem_previsao_escala/dia_sem_grade` em `:513–514` | **Caso explicitamente sem célula:** `detectar_ausencias.py:660–668` chama o emissor quando a célula não existe. |

Todos os sete: **real**, consumidor `app/chamados/juizes.py:187` e contagem de “sem dia” em `app/chamados/services/juizes_do_dia.py:92–99`. A célula, quando existente e ligada, antecede o contexto em `juizes.py:184–186`; isso limita a manifestação, não elimina o ramo sem célula permitido em `models.py:1241–1243`. `VIVOS` inclui `registrado`, então o chamado de auditoria do desligamento também é selecionado (`catalogo/motor.py:24`).

Não acrescentei `regua_suspeita`: o emissor `avisar_cadastro_regua_suspeita` foi removido, conforme lápide `app/escala/regua_defesa.py:547–551`; outro emissor atual [nao verificado].

**Selos:** nenhum selo específico que prenda essas combinações emissor→leitor foi localizado [nao verificado quanto à ausência global]. Os selos próximos **não prendem os desencontros**:

- D01–D03: `test_a2_data_evento_batida.py:21–28` fabrica o contexto canônico `ultima_batida`; `app/ponto/tests/test_selfie_ausente_juiz.py:82–91` verifica prazo/emissão do retro, sem verificar episódio, dia ou fechamento por upload.
- D04/D05 e sete adicionais: `app/chamados/tests/test_contract_dia_do_fato.py:92` exclui **todo `juizes.py`** da varredura; não confere a paridade singular/lote. A busca literal `dias_do_fato` nos testes Python encontrou apenas `test_e1_v9.py:358–368`, cobrindo conversa/fato de disputa, sem os formatos catalogados.
- D06–D08: `test_form_catalogo.py:76–83` prende emissão humana de `ausencia_colab` e `data`, sem referência/anexo/nota.
- D09/D10: escritor preso em `test_emissor_nomeia_o_marco.py:54–63` e `:152–163`; leitor preso separadamente em `test_o306_marco_do_motivo_nao_e_do_vizinho.py:45–71`. Não há nesse par um teste que entregue `lampada_apagada` ao leitor semântico [nao verificado quanto à ausência global]. [nao verificado]

Dono dos desencontros reais: **ESTRUTURA**, pois a divergência está no protocolo do código, sem exigir cadastro ou batida incorretos; separação L-099 em `CLAUDE.md:179`. D07: sem defeito demonstrado, dono de defeito não aplicável; se a coluna exigir um dos três rótulos, marcar ESTRUTURA apenas como domínio auditado, não como culpa provada.

Agrupamento: **D01–D03** = identidade do episódio; **D04/D05 + sete adicionais** = lote ignora a chave de data catalogada; **D06/D08** = emissão humana de ausência sem entidade referenciada, consumida como se houvesse entidade; **D09/D10** = `motivo` da lâmpada confundido com motivo da pergunta, pelos sítios citados.

## 13/11 18:33 · sessao 18779249eade · - · - · origem=Ronald (tmux)

D11–D17: **7 reais no código; ocorrência em produção [nao verificado]**. Todos pertencem a ESTRUTURA pelo critério L-099 (`CLAUDE.md:179`).

| ID | Emissor/chaves | Consumidor | Classe e prova decisiva | Selo específico |
|---|---|---|---|---|
| D11 | `app/chamados/push_utils.py:28`: `title,body,url,requireInteraction` | `app/static/sw.js:97`, `:107`: `urgente`, clique `/` | **real**: `requireInteraction=True` vira `false`; URL é ignorada. Caminho PWA registrado em `app/static/pwa.js:14`; servidor serve SW em `app/core/views.py:570`; chamada viva em `app/chamados/services/abertura.py:82`. | Não demonstrado [nao verificado]. `app/chamados/tests/test_push_fcm.py:106` só exige chamada ao transporte (`:110`). |
| D12 | `app/chamados/glpi_client.py:138`: ticket sem `urgencia` | `app/templates/chamados/partials/tabela_chamados.html:30` | **real**: termina em “Normal” (`:37`). Rota viva `app/chamados/urls.py:13`; dados formatados chegam sem enriquecimento por `app/chamados/views.py:323`, `:340`. | Não localizado [nao verificado]. |
| D13 | `app/chamados/glpi_client.py:209`: `id,data,conteudo` | `app/templates/chamados/detalhe.html:83`, `:98`, `:101`: `autor_tipo,autor_user,criado_em` | **real**: data some e autor é apresentado como “Colaborador” pelo `else` (`:98`). Transporte direto em `app/chamados/views.py:246`, `:262`; rota `app/chamados/urls.py:11`. | Não demonstrado [nao verificado]. `app/chamados/tests/test_glpi_offline.py:95` cobre indisponibilidade, não formato. |
| D14 | `app/chamados/views_arquivar.py:62`: `ok,erro` | `app/templates/chamados/partials/_lista_chamados.html:134`: `erros` | **real**: recusa de departamento vira mensagem genérica. Botão vivo em `:85`, rota `app/chamados/urls.py:27`; recusa calculada por `app/chamados/utils.py:75`. | Não demonstrado [nao verificado]. `app/chamados/tests/test_f3_arquivar_porta.py:64` cobre sucesso/trilha (`:70–75`). |
| D15 | `app/chamados/services/fio.py:461`: omite contadores e mapa | `app/templates/chamados/partials/modal_fio.html:14`, `:76` | **real**: contadores desaparecem; cores passam ao cinza (`app/chamados/templatetags/chamados_tags.py:107`). Callback vivo em `app/chamados/services/fio.py:494`, `:609`, chamado pelo POST em `app/chamados/views.py:954`; também `app/chamados/views.py:1399`. | Não demonstrado [nao verificado]. `app/chamados/tests/test_paridade_fio_modal_f8.py:62` verifica contadores somente no GET completo (`:65`, `:69`). |
| D16 | `app/chamados/views.py:927`: omite `pode_conferir_data` | `app/templates/chamados/partials/modal_fio.html:172` | **real**: usuário autorizado perde formulário e recebe bloqueio (`:180`). GET entrega esse contexto (`app/chamados/views.py:956`); variante parcial calcula a permissão em `app/chamados/services/fio.py:474`. | Não localizado para este par [nao verificado]. O teste da porta (`app/chamados/tests/test_data_do_fato_divergente.py:185`) não verifica a presença do formulário no GET. |
| D17 | `app/chamados/views_cobrar.py:265`: `ok,validadas,recusados,nao_gravados`; erro em `:263` inclui `erros` | `app/templates/core/_copiloto.html:252` | **real**: ignora `nao_gravados`, `ok` e `erros`. Prova sem ambiguidade: referências vazias produzem erro (`app/chamados/services/validacao.py:721`), view responde HTTP 200 com `ok=False,validadas=0,recusados=[]` (`app/chamados/views_cobrar.py:263`), JS só verifica HTTP (`_copiloto.html:251`) e escreve “Todos entraram” (`:254–255`). Rota viva `app/chamados/urls.py:34`; URLs ligadas em `_copiloto.html:16`. | Não demonstrado [nao verificado]. `app/chamados/tests/test_validar_lote.py:146` testa `ok=False`, sem executar consumidor JS. |

Os processadores globais **não completam D15/D16**: lista configurada em `app/config/settings/base.py:76`; saídas em `app/core/context_processors.py:154`, `:189`, `:210`; `app/core/context_processors_ia.py:31`; `app/pautas/context_processors.py:35`.

**Novo candidato descartado:** `app/chamados/views_arquivar.py:34` também emite `erro` singular, porém não demonstrei ligação desse endpoint ao JS acima. O único chamador nominal do helper passa `resgatar=true` e URL de desarquivar (`app/templates/chamados/partials/_lista_chamados.html:85`). A alternativa `resgatar=false` existe (`:114`, `:125`), mas chamador vivo dela é [nao verificado]. Portanto, **não acrescentar um novo real apenas pela semelhança do envelope**.

**Nuance D17:** `nao_gravados` inclui perguntas efetivamente validadas cujo callback não retornou sucesso (`app/chamados/services/validacao.py:753–758`). A frase “Todos entraram” isoladamente pode referir-se à validação; a prova independente e inequívoca é a perda de `ok/erros` descrita acima. O outro consumidor mostra explicitamente “Validadas sem gravar o ponto” (`app/templates/chamados/partials/_lote_preview.html:9`).

## 13/11 18:32 · sessao b273dbc92701 · - · - · origem=Ronald (tmux)

D18–D23 são **escrita morta na saída**, considerando leitores de produção. Os serviços continuam tendo efeitos; a classificação vale apenas para os campos excedentes retornados. (`app/chamados/services/fio.py:922`, `app/chamados/services/fio.py:2181`, `app/chamados/reconciliador.py:1365`, `app/chamados/reconciliador.py:1384`.)

| ID | Emissor / chave excedente | Consumidor | Classe e prova | Selo |
|---|---|---|---|---|
| D18 | `app/chamados/services/fio.py:1114`: `fonte` | `app/chamados/management/commands/backfill_decisoes_pergunta.py:553` | **escrita morta**: formatação lê `n,esperado,motivo,universo,universo_rotulo,pks`; não lê `fonte` (`:554`–`:556`). | Não localizado para a omissão [nao verificado]. Teste vizinho de contador: `app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:320`. |
| D19 | `app/chamados/services/fio.py:1181`: `fonte` | `app/chamados/management/commands/backfill_decisoes_pergunta.py:559` | **escrita morta**: mesmas seis leituras de D18 (`:560`–`:562`). | Não localizado para a omissão [nao verificado]. Teste vizinho: `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:293`. |
| D20 | `app/chamados/services/fio.py:1206`: `fonte` | `app/chamados/management/commands/backfill_decisoes_pergunta.py:565` | **escrita morta**: mesmas seis leituras; cabeçalho de fonte é texto fixo (`:566`–`:568`). **Há leitura em teste**, portanto “ninguém lê” sem restringir a produção seria falso (`app/chamados/tests/test_e1_v15h.py:186`). | **Existe teste do par**, `app/chamados/tests/test_e1_v15h.py:194`: chama comando, confere linha, número/motivo e PK (`:197`–`:201`). Ele não exige imprimir `fonte`. |
| D21 | `app/chamados/services/fio.py:1902`: `fonte` | `app/chamados/management/commands/backfill_decisoes_pergunta.py:545` | **escrita morta**: lê `n,esperado,motivo,pks` (`:546`–`:547`). | Não localizado para a omissão [nao verificado]. Teste de contador: `app/chamados/tests/test_e1_v15b.py:165`. |
| D22 | `app/chamados/services/fio.py:1342`: `merito_recusado,episodios_unidos,sem_episodio_intocada,canonica_sem_episodio` | `app/chamados/management/commands/backfill_decisoes_pergunta.py:331` | **escrita morta no retorno**: esse chamador e `app/chamados/services/fio.py:2181` extraem somente `retratadas`; `app/chamados/services/fio.py:922` ignora retorno inteiro. **O plano anterior é vivo**, inclusive suas categorias (`app/chamados/services/fio.py:2162`–`:2165`); não confundir plano com resultado da porta. | Não localizado para as quatro omissões [nao verificado]. Teste da porta confere `retratadas` e ordem de trava: `app/chamados/tests/test_e1_v15g_portas.py:237`. |
| D23 | `app/chamados/reconciliador.py:1340`: `rejuntados,perguntas,lastro` | `app/chamados/management/commands/consumir_eventos_celula.py:31` | **escrita morta no retorno**: interpolação mapeada lê oito campos e ignora esses três (`:32`–`:35`). Os efeitos chamados são vivos (`app/chamados/reconciliador.py:1365`, `:1366`, `:1384`). Cron chama o comando (`app/config/crons.py:394`). | Não localizado para omissões na impressão [nao verificado]. |

Dono das seis diferenças de contrato: **ESTRUTURA**, segundo separação de código e dado em `CLAUDE.md:179`. Cobertura/executabilidade dos testes nesta cópia não foi executada [nao verificado]. [nao verificado]

Sobre universo: a contagem AST que executei **não é contagem de costuras** [nao verificado]. O algoritmo exato foi `Path('app/chamados').rglob('*.py')`, excluindo componentes `tests,migrations,__pycache__`, `ast.parse` em memória, sem importar aplicação. Resultado operacional: 140 arquivos, zero erros; `ast.Dict` 1108, dos quais 46 com chave dinâmica/expansão; `ast.DictComp` 107; chamadas `dict(...)` 101; chamadas com `keyword.arg is None` 53; funções com `args.kwarg` 76; chamadas `.get/.pop/.setdefault` 925, das quais 381 sem primeiro argumento constante [nao verificado — evidência operacional não persistida por regra de leitura]. [nao verificado]

Esses números contam sintaxe, não relações emissor→consumidor, e não incluem fluxos de JS/templates [nao verificado]. Os sítios concretos que precisam ser resolvidos antes de afirmar contador==universo são:

- Contextos abertos por kwargs: `app/chamados/services/contexto.py:62`, `:71`, `:74`, `:76`; renovação `:82`, `:90`, `:100`, `:102`.
- Fusão de qualquer chave recebida: `app/chamados/services/contexto.py:131`, `:146`, `:154`, `:156`, `:161`.
- Contexto de disputa recebido e mesclado com chaves dinâmicas: `app/chamados/services/disputa_emissao.py:548`, `:555`, `:556`, `:559`, `:561`, nascimento integral `:590`.
- Dicts de decisões transformados em kwargs do modelo: `app/chamados/models.py:3944`, `:3952`, `:3954`.
- Leitor cuja chave depende do catálogo: `app/chamados/catalogo/modulos.py:225`.
- Leitor integral de contexto, que impede chamar campos dele globalmente mortos por ausência de `.get('nome')`: `app/templates/chamados/partials/painel_chamado_turno.html:35`, `:36`.
- Auditoria persiste os dicts integrais recebidos: `app/core/observ.py:94`, `:136`.

Não encontrei novo desencontro comprovado além de D18–D23 nas famílias que examinei [nao verificado]. Não concluí expansão global de costuras, portanto total do módulo e total global de desencontros permanecem [nao verificado].

## 13/11 18:32 · sessao a1a6ce57542f · - · - · origem=Ronald (tmux)

Universo **não fechado**. A varredura AST em memória contou **61 arquivos Python de produção em escala**, **435 `ast.Dict`**, **45 `ast.DictComp`**, **32 chamadas `dict(...)`**, **18 expansões `**kwargs`** e **2 dicts com expansão `**`**. Esses números contam construções sintáticas, não pares emissor→consumidor: incluem estruturas locais e não resolvem transporte integral, aliases e chaves recebidas.

Padrão reprodutível:

```python
ps = [
    p for p in Path("app/escala").rglob("*.py")
    if not set(p.parts) & {"tests", "migrations", "__pycache__"}
]
# ast.parse(p.read_text()); ast.walk(tree)
# Dict; DictComp; Call(func=Name(id="dict"));
# Call.keywords com arg is None; Dict.keys contendo None.
```

Sítios concretos que impedem equiparar esse contador ao universo:

- `app/escala/services/cadastro_tipo.py:265-267`: persiste integralmente o mapa recebido; as chaves não se restringem a literais definidos nesse escritor.
- `app/escala/services/fase_12x36.py:85-94`: escreve `detalhe` recebido; o próprio código identifica o produtor material em outro módulo (`:88`). `:211-214` devolve o snapshot integral, acrescido de metadados.
- `app/escala/services/cadastro_realidade.py:898-900`: funde dicionário de assinatura, campos comuns e `_extra`.
- `app/escala/services/leitor_celula.py:418-428`: monta um dia e incorpora chave importada (`INSUMO_DIA_SEM_TRABALHO`); as chaves adicionais condicionais aparecem em `:433-440`.
- `app/escala/services/regeneracao.py:104-105`: monta mapa de carimbos por data; o consumidor externo o expande como kwargs em `app/ponto/management/commands/supra_juiz.py:275-276`, e os nomes correspondem à assinatura de `cobranca_orfa` (`app/escala/services/regeneracao.py:141`). Não há desencontro demonstrado nesse par.
- `app/escala/signals.py:178-182`: emite contexto de disputa; o consumidor incorpora o dicionário inteiro à anotação (`app/chamados/models.py:2042-2046`). Campos sem leitura nominal posterior não podem ser chamados mortos apenas por essa falta.
- Outbox: não há payload e o escritor é externo a escala (`app/escala/models.py:1848-1855`).

**Três desencontros nominais adicionais comprovados nos consumidores de produção localizados:**

| ID proposto | Emissor | Chaves sem consumo de produção | Consumidor | Classe | Prova decisiva | Selo específico |
|---|---|---|---|---|---|---|
| 18 | `app/escala/services/impacto_troca.py:47-48` | `de, ate, furos_antes, furos_depois, dias_furo_antes, dias_furo_depois` | `app/colaboradores/services/vinculo.py:141-147` → `:355-360` | escrita morta | Único chamador nominal importa e recebe o dict; os dois consumidores finais usam somente `frase` (`:356`, `:360`), sem serializar o dict integral | **Não** para descarte. Parcial: `app/colaboradores/tests/test_troca_escala_porta.py:50-56` afirma contagens no produtor; `:63-75` prende frase no toast/trilha |
| 19 | `app/escala/services/fase_na_tela.py:206-212` | `dias_furo_antes, dias_furo_depois` | `app/colaboradores/views_fase.py:98-115` → `app/templates/colaboradores/partials/_fase_12x36.html:6-39` | escrita morta | View passa `cal` e três prévias; template lê contagens e calendário; include lê `cal.semanas`, `mes`, `ano` e células (`app/templates/colaboradores/partials/_calendario_fase.html:5-15`), sem usar as duas listas. Rota viva: `app/colaboradores/urls.py:15` | **Não** para descarte. Produtor preso em `app/colaboradores/tests/test_o207_vinculo_inicio_declarado.py:177-188`, inclusive leitura direta das listas |
| 20 | `app/escala/services/semana_do_posto.py:201-214` | `titular_id, substituto_id, candidatos` | `app/api/views_mensageria.py:2120-2133` | escrita morta | API reconstrói cada proposta com `data, frase, confianca, titular, substituto` (`:2129-2133`); consumidor interno só conta propostas (`app/escala/services/semana_do_posto.py:237-241`) | **Não** para descarte. Produtor preso em `app/escala/tests/test_semana_do_posto.py:149-152` e `:165-168` |

O rastreamento nominal global por `impacto_da_troca`, `calendario_de_fase` e `propostas_cruzadas` encontrou apenas os chamadores descritos fora dos testes; resolução dinâmica adicional permanece **[nao verificado]**. [nao verificado]

Dono desses contratos: **ESTRUTURA como atribuição técnica**, pois são campos de interface sem consumo, não divergências de cadastro ou batidas (`CLAUDE.md:179`). Dono de qualquer ocorrência dia-colaborador de produção: **[nao verificado]**; o juiz depende de insumos do dia, paridade e cadastro (`app/ponto/management/commands/e6_oraculo.py:98-110`). Nenhum desses três extras é `real`, nem integra a classe do fio `foto_ausente.batida_id`.

## 13/11 18:32 · sessao 1b942ee66ddc · HJ-090 · verificador · origem=Code

**BLOQUEIA a pré-verificação:** há reversão interrompida por PA apagado, validação incompleta do registro e lacuna de calendário em D. Há também um caminho de edição que passa a lançar `ValidationError` sem tratamento. Referências: `app/ferias/management/commands/realinhar_concedidos_ferias.py:102`, `:71`; `app/ferias/services.py:754`; `app/ferias/views.py:838`.

Execução da suíte, RED contra mutantes, acervo real de registros e confirmação do hash por Git: **[nao verificado]**. Os números de calendário abaixo foram conferidos com as funções da cópia, extraídas em memória. [nao verificado]

**1. PROCEDENTES A–D**

| Item | Veredito | Conferência |
|---|---|---|
| **A** | **OK** | A identidade inclui `pk`, **dias**, início e fim. Mudar somente dias mantendo datas também invalida a identidade; a porta de remarcação atualiza concedidos pelo delta dos dias. `app/ferias/services.py:139`, `:192`, `:652`. |
| **B** | **OK para a guarda do ato** | A consulta filtra **modelo e PA antes de selecionar o último PK**. Atos intercalados de dois períodos da mesma pessoa não se contaminam. Intenção sem commit não deixa o ato exigido pela reversão. `app/ferias/services.py:164`, `:177`, `:193`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:142`. |
| **C** | **BLOQUEIA a garantia de validação integral** | `{}` é recusado antes do laço de reversão. Entretanto, `vivos` só precisa ser uma lista: `[null]`, `[[1]]` ou datas inválidas passam. Essas linhas viram “linha mudou”, possivelmente **depois de reversões anteriores**, em vez de corrupção recusada previamente. O formato produzido exige quatro componentes por identidade. `app/ferias/management/commands/realinhar_concedidos_ferias.py:69`, `:100`; `app/ferias/services.py:139`, `:192`. |
| **D** | **BLOQUEIA o fechamento completo** | Criação e edição agora chamam a mesma conta, mas a linha de ciclos ainda pode deixar um dia descoberto. `app/ferias/views.py:164`, `:167`; `app/ferias/services.py:770`. |

**PA apagado — BLOQUEIA a reversão operacional.** Um registro pode continuar contendo ato válido depois de cancelar os agendamentos e excluir o PA pelas portas regulares; `LogAuditoria.objeto_id` não é FK. O command faz `.get()` sem tratar o PA ausente: aborta o restante do arquivo, enquanto reversões anteriores já podem ter terminado suas transações. Isso não é corrupção JSON nem uma linha simplesmente pulada. `app/ferias/services.py:781`, `:190`; `app/core/models.py:298`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:100`.

**D, contraexemplo de 29/02 — BLOQUEIA.** Para admissão em `29/02/2024`, o código gera:

- ciclo iniciado em `28/02/2027`, terminado em `27/02/2028`;
- ciclo seguinte iniciado em `29/02/2028`;
- portanto, **`28/02/2028` não pertence a PA algum**.

O início vem da admissão original; o fim vem do início já truncado para 28/02. Isso contraria a própria “linha completa” anunciada pela criação. `app/ferias/views.py:157`, `:164`; `app/ferias/services.py:742`, `:754`.

Há efeito concreto nos leitores: uma falta nesse dia fica fora dos intervalos inclusivos dos PAs. Um gozo histórico iniciado nesse dia não encontra o ciclo por contenção e cai no PA mais recente de todo o acervo. `app/ferias/models.py:147`; `app/ferias/views.py:199`, `:203`. Existência desse cadastro em produção: **[nao verificado]**.

**Último dia do mês — OK para a fórmula documentada.** `31/01/2024` resulta em fim `30/01/2025`, limite `30/01/2026`; `31/03/2025` resulta em fim `30/03/2026`, limite `30/03/2027`. A função aplica aniversário menos um dia, sem preservar “último dia do mês” como propriedade adicional. `app/ferias/services.py:742`, `:750`.

**2. DESCARTADOS da triagem3**

- **r.r:48 — BLOQUEIA o descarte incondicional.** A guarda de sobreposição existe no modelo, mas D muda as datas entregues a ela. Contraexemplo: PA1 `01/01/2024..30/12/2024`, compatível com a antiga edição `+364`, seguido de PA2 iniciado em `31/12/2024`. Reeditar PA1 mantendo início e direito agora calcula fim `31/12/2024`, colide e lança `ValidationError`; a view trata `RecusaNaPorta` e `IntegrityError`, mas não essa exceção. A guarda pode ser antiga e o caminho afetado pelo delta. `app/ferias/services.py:754`, `:770`; `app/ferias/models.py:193`, `:200`; `app/ferias/views.py:838`. Preexistência histórica da reprodução original de r.r:48: **[nao verificado]**.

- **r.r:79 — NAO-BLOQUEIA como alteração deste delta; a fragilidade permanece.** A correção de datas termina a porta antes de chamar `registrar_log`; essa trilha é best-effort. Falhar a auditoria não desfaz a correção, embora a mensagem anuncie motivo registrado. Esse caminho não recebeu a proteção `_trilha` usada pelo realinhamento. `app/ferias/views.py:519`, `:530`, `:537`; `app/core/auditoria.py:17`, `:25`; `app/ferias/services.py:421`. A alegação histórica “preexistente”: **[nao verificado]**.

- **r.r:110 — OK, motivo confirmado no contrato local.** A lei fornecida permite correção provada sem trava de tela por exportada/trancada/paga, e o materializador declara expressamente que período fechado não recusa esse caminho. Isso não comprova qualquer efeito real sobre fechamento ou TXT. `CLAUDE.md:418`, `:421`; `app/ponto/services/ausencia.py:1164`, `:1169`.

- **50, 54, 55, 56, 61, 65, 71, 73 e 77 — NAO-BLOQUEIA apenas como encaminhamento declarado; mérito [nao verificado].** O anexo contém os números e a justificativa coletiva, mas não o conteúdo desses achados nem das FILAS 6–8 da preverif2. Não é possível conferir individualmente o motivo de descarte: **[nao verificado]**.

**3. Quem escreve, quem lê e significado dos campos**

| Campo | Escritores | Leitores e significado |
|---|---|---|
| `data_inicio` | Criação por `garantir_periodo_aquisitivo`; edição na porta travada. `app/ferias/services.py:386`, `:769`. | Identifica o ciclo na criação; limita faltas e seleção do PA do histórico; ordena ciclos anteriores. `app/ferias/views.py:176`, `:199`; `app/ferias/models.py:149`; `app/ferias/management/commands/listar_ferias_dp.py:66`. |
| `data_fim` | Criação entrega o resultado comum pelos defaults; edição chama a mesma função. `app/ferias/views.py:167`, `:179`; `app/ferias/services.py:770`. | **Inclusivo** em faltas, sobreposição e contenção do histórico. O status permanece `em_curso` durante o próprio último dia. `app/ferias/models.py:149`, `:196`, `:101`; `app/ferias/views.py:200`. |
| `data_limite_concessao` | Mesmas portas das datas acima. `app/ferias/views.py:179`; `app/ferias/services.py:770`. | O juiz considera vencido apenas quando `limite < hoje`; situação e listas usam a data para avisos/ordenação. `app/ferias/models.py:103`; `app/ferias/situacao.py:193`, `:209`; `app/ferias/services.py:703`. |
| `dias_concedidos` | Inicialização, realinhamento/reversão, histórico, agendamento, cancelamentos, remarcação e aprovação. `app/ferias/views.py:180`; `app/ferias/services.py:159`, `:196`, `:491`, `:526`, `:594`, `:621`, `:652`, `:723`. | É consumo **agendado ou gozado**, não somente gozo concluído. Saldo desconta esse valor; contador confronta-o com todos os agendamentos não cancelados; tela explicita ambos os usos. `app/ferias/models.py:220`; `app/ferias/services.py:105`, `:125`; `app/templates/ferias/_secao_colab.html:49`. |
| `status` do PA | Nasce na criação e é atualizado pelas portas de saldo/datas e sincronização. `app/ferias/views.py:171`, `:181`; `app/ferias/services.py:160`, `:197`, `:329`, `:392`, `:772`. | Autoridade é `status_de_periodo`; saldo zerado/negativo prevalece sobre datas. Há leitores do campo armazenado, como `pode_agendar`, seleção da aprovação e listas. `app/ferias/models.py:98`, `:234`; `app/ferias/services.py:702`; `app/ferias/management/commands/listar_ferias_dp.py:67`. |

**NAO-BLOQUEIA — leitor com limite diferente já declarado.** O relatório escreve “Vencido!” quando `limite <= hoje`; o juiz exige `< hoje`. A divergência está registrada entre os leitores pendentes. `app/templates/relatorios/ferias_vencendo.html:50`; `app/ferias/models.py:103`; `app/core/juizes.py:846`.

| Registro/chave | Escritor e leitor |
|---|---|
| `realinhar_*.json` | Command escreve snapshot informativo com `motivo='HJ-090'`, `por` e períodos. A reversão exige o arquivo `.aplicado.jsonl`, recusando esse snapshot. `app/ferias/management/commands/realinhar_concedidos_ferias.py:97`, `:122`, `:126`. |
| `.aplicado.jsonl` | Recebe o retorno da porta sob transação externa; write/flush/fsync antecedem o commit daquele PA. Leitura completa precede o laço de reversões. `app/ferias/management/commands/realinhar_concedidos_ferias.py:100`, `:142`, `:145`. |
| `depois.ato` | Porta grava o PK da auditoria do apply **depois** de produzir a trilha; validador exige inteiro e reversão compara com o último ato HJ-090 **daquele PA**. A própria auditoria não recebe esse ponteiro. `app/ferias/services.py:164`, `:168`, `:177`, `:193`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:71`. |
| Motivos HJ-090 | Constantes alimentam `LogAuditoria.descricao`; `_ultimo_ato_hj090` lê exatamente os motivos de apply e reversão. Não lê `motivo` do snapshot. `app/ferias/services.py:131`, `:164`, `:178`, `:199`. |
| `ferias_soma_maior` / `ferias_concedidos_maior` | Autoridade retorna listas de PAs; vigia grava seus comprimentos. Tripwire e lista DP consultam a mesma autoridade. `app/ferias/services.py:116`; `app/chamados/management/commands/vigiar_fio_morto.py:234`, `:250`; `app/ferias/management/commands/tripwire_status_ferias.py:37`; `app/ferias/management/commands/listar_ferias_dp.py:55`. |
| `desligados_retidos` | Origem seleciona retidos por **`furo_espelho`** julgados desligados; agregação soma ocorrências **colaborador × competência**, não pessoas únicas. Vigia publica o agregado. `app/folha/fora_da_folha.py:128`, `:144`; `app/chamados/services/vigia_fio.py:583`, `:589`; `app/chamados/management/commands/vigiar_fio_morto.py:252`. |

**4. Classes de reprovação**

- **OK — duplicação de juiz registrado não encontrada para `datas_do_ciclo`.** As autoridades registradas de férias respondem saldo, status/vencimento, abono, fracionamento e aviso; duração de ausência responde outra pergunta. `app/core/juizes.py:268`.

- **OK no censo de produtores encontrados — sem outro derivador produtivo de fim/limite do PA.** Criação e edição usam a função comum; admin registra Afastamento, migration declara o esquema e API apresenta as datas. `app/ferias/views.py:167`; `app/ferias/services.py:770`; `app/ferias/admin.py:4`; `app/ferias/migrations/0001_initial.py:40`; `app/api/views_mensageria.py:837`. Escritores dinâmicos externos à cópia: **[nao verificado]**.

- **BLOQUEIA certificar “relógio só na borda”.** `executar_agendamento` e `_trocar_janela` ainda leem `date.today()` internamente; suas chamadas de status também podem cair no relógio implícito do modelo. O histórico, por sua vez, recebe `hoje` explicitamente. `app/ferias/services.py:508`, `:654`, `:658`, `:493`; `app/ferias/models.py:94`, `:283`.

- **OK — realinhamento não engole falha de trilha.** `_trilha` transforma o `None` best-effort em exceção dentro do atomic. Isso não cobre a trilha posterior da view mencionada em r.r:79. `app/ferias/services.py:421`, `:427`, `:153`; `app/core/observ.py:139`.

- **OK — command normal sem `--apply` retorna antes de criar arquivos.** `--reverter --por` é uma porta explícita de escrita documentada. `app/ferias/management/commands/realinhar_concedidos_ferias.py:19`, `:95`, `:114`. A simulação da lista DP escreve na sombra e força rollback; efeitos externos de signals: **[nao verificado]**. `app/ferias/management/commands/listar_ferias_dp.py:52`, `:98`, `:109`.

- **NAO-BLOQUEIA — datas fixas identificadas são fixtures com relógio congelado.** A conta produtiva do ciclo recebe a data como entrada. `app/ferias/test_hj090_um_juiz.py:41`, `:62`; `app/ferias/test_hj090_volta3.py:80`; `app/ferias/services.py:750`.

**Mordida dos novos testes, por leitura estática:** [nao verificado]

| Cura | Veredito |
|---|---|
| A | **OK contra voltar a `[pk,dias]`**: troca apenas datas e exige recusa. Não exercita mudança só de dias. `app/ferias/test_hj090_volta3.py:261`. |
| B | **OK contra retirar a guarda ou aceitar qualquer ato existente**. **Cobertura incompleta** contra retirar o filtro por PA: os casos novos não intercalam períodos. `app/ferias/test_hj090_volta3.py:270`, `:285`. |
| C | **OK contra retirar a validação prévia**; não cobre identidade interna inválida nem PA apagado. `app/ferias/test_hj090_volta3.py:298`. |
| D | **BLOQUEIA certificar fonte única e fronteiras**: morde a antiga edição `+364`, mas não chama a criação. Uma conta independente restaurada na criação sobreviveria; 29/02 também fica sem caso. `app/ferias/test_hj090_volta3.py:316`. |

**5. Merge do carimbo**

**OK quanto à coerência das três chaves.** `ferias_soma_maior`, `ferias_concedidos_maior` e `desligados_retidos` aparecem tanto no esperado quanto no censo. O ESTADO compara genericamente todas as entradas do esperado e publica divergências. `app/chamados/management/commands/vigiar_fio_morto.py:100`, `:250`; `bin/fio_estado.py:125`, `:191`.

**NAO-BLOQUEIA — cobertura dos canais é diferente.** Essas chaves não integram os conjuntos de alarme do vigia; o placar consulta explicitamente outros dois contadores. Portanto, carimbo coerente não implica alarme de férias/desligados nesses canais. `app/chamados/management/commands/vigiar_fio_morto.py:84`; `app/core/management/commands/pipeline_placar.py:135`. Hook externo/instalado: **[nao verificado]**.

**NAO-BLOQUEIA condicionado ao acervo — registros antigos.** O leitor agora exige `depois.ato`; registros anteriores sem essa chave são recusados integralmente. Existência e necessidade de reversão desses arquivos em produção: **[nao verificado]**. `app/ferias/management/commands/realinhar_concedidos_ferias.py:69`.

## 13/11 18:31 · sessao 0d33144acba3 · - · - · origem=Ronald (tmux)

Auditoria dos IDs **7–11**, preservando a ordem do anexo. A classificação considera caminhos vivos na cópia; ocorrência efetiva em produção permanece **[não verificado]**.

| ID | Emissor / chave | Consumidor | Classe | Prova decisiva |
|---|---|---|---|---|
| **7** | `app/ponto/management/commands/detectar_ausencias.py:231` e `:532`; ambos escrevem `data_inicio` | `app/chamados/catalogo/modulos.py:126`, `:225` | **falso** | O catálogo declara uma **chave de data genérica**, não “início do vínculo”: `app/chamados/catalogo/modulos.py:10`. Seu leitor simplesmente devolve `contexto_json[chave_data]`: `:225`. O segundo produtor abre uma janela agregada por colaborador, iniciada pela lacuna e acrescida de ocorrências: `app/ponto/management/commands/detectar_ausencias.py:501`, `:506`, `:532`, `:538`. O anexo inferiu indevidamente que todo `data_inicio` precisaria significar início do vínculo; essa exigência não está no consumidor citado. |
| **8** | `app/ponto/views.py:280`, renovação `:288`, nascimento `:301`; `pendentes,total`, sem `data_turno` | `app/chamados/catalogo/modulos.py:182`, `:225` | **real** | Esse emissor não fornece a chave declarada. O leitor tem efeito: o cartório seleciona os módulos de batida e só inclui chamado quando a data retorna preenchida (`app/ponto/services/cartorio.py:127`, `:135`, `:137`); `entrada_bloqueada_turno_aberto` pertence ao universo (`app/chamados/catalogo/furo_modulos.py:15`). A porta de criação resolve célula, mas não acrescenta `data_turno` ao contexto recebido (`app/chamados/models.py:1068`, `:1071`). |
| **9** | `app/ponto/webpush.py:44`; `title,body,url` | `app/static/sw.js:88`, `:102` | **escrita morta** | `url` é transportada opacamente dentro de `options.data` (`app/static/sw.js:96`), mas o clique só foca a primeira janela ou abre `/`, sem consultar a URL (`:105`, `:106`, `:107`). A ausência de `urgente` sozinha não demonstra defeito neste produtor: ele não transmite intenção de interação obrigatória (`app/ponto/webpush.py:44`); o leitor declara `false` (`app/static/sw.js:97`). |
| **10** | `app/ponto/management/commands/processar_alertas_avancados.py:207`, `:240`; `requireInteraction=True,url` | `app/static/sw.js:97`, `:102` | **real** | Ambos os payloads pedem interação obrigatória (`app/ponto/management/commands/processar_alertas_avancados.py:211`, `:244`), enquanto a decisão de apresentação lê `urgente`, ausente nesses payloads (`app/static/sw.js:97`). Portanto decide `false`. Os métodos têm chamadores no corpo do comando (`app/ponto/management/commands/processar_alertas_avancados.py:82`, `:144`, `:190`) e o comando está no cron (`app/config/crons.py:363`). A URL também fica sem efeito no clique (`app/static/sw.js:107`). |
| **11** | `app/ponto/management/commands/processar_alertas_turno.py:279`; `requireInteraction,url` | `app/static/sw.js:97`, `:102` | **real** | Para `pos_10`/`pos_20`, o produtor envia `requireInteraction=True` (`app/ponto/management/commands/processar_alertas_turno.py:283`); o consumidor lê `urgente` e decide `false` (`app/static/sw.js:97`). Há chamada de `_enviar_push` pelo executor (`app/ponto/management/commands/processar_alertas_turno.py:244`), execução pelo corpo principal (`:125`, `:143`) e cron (`app/config/crons.py:362`). URL igualmente sem efeito (`app/static/sw.js:107`). |

**Selos:** nenhum selo localizado demonstra integralmente as cinco costuras; inexistência global de selo é **[não verificado]**. Os limites concretos são:

- **7:** o teste genérico do catálogo testa `batida_ausente` e `disputa_supervisao`, sem distinguir esses nascimentos de `sem_previsao_escala` (`app/chamados/tests/test_f4_catalogo.py:51`).
- **8:** o selo do cartório fabrica `data_turno` no fixture, evitando o produtor real (`app/ponto/tests/test_cartorio_so_chamado_de_batida.py:43`, `:45`, `:63`). Os testes da view verificam nascimento e continuidade da batida (`app/ponto/tests/test_f7_ramo_quente.py:113`), ou deduplicação (`app/ponto/tests/test_eb_dedup.py:39`).
- **9:** o teste do holerite substitui `push_web_colaborador` por mock e não executa o SW (`app/holerite/tests/test_selo_sem_notificacao.py:375`).
- **10–11:** testes próximos substituem os métodos de envio, não prendendo payload ao SW (`app/ponto/tests/test_avancados_grade.py:18`; `app/ponto/tests/test_alertas_catalogo_premissa.py:99`).

**Totais deste recorte:** real **3**, escrita morta **1**, ramo morto **0**, falso **1**; fundamentação nas cinco linhas acima. **10 e 11 são a mesma classe de defeito**, `requireInteraction → urgente`, compartilhando o consumidor `app/static/sw.js:97`. O **8** é ausência da chave canônica de data; os contratos examinados são questão de **ESTRUTURA**, pela separação entre coerência do sistema e dado cadastral/batida (`CLAUDE.md:179`).

A rota e o registro do SW existem (`app/core/urls.py:24`; `app/static/pwa.js:14`), mas a aplicação pode servir o arquivo coletado em `/app/staticfiles`, selecionado em execução (`app/core/views.py:528`, `:570`). Identidade desse artefato implantado com a cópia auditada: **[não verificado]**.

## 13/11 18:31 · sessao 47b436f171e5 · - · - · origem=Ronald (tmux)

Auditoria dos IDs 1–6: **3 real; 3 falso**. Os três reais pertencem à classe já registrada **CONTRATO-DE-ENTRADA-DO-FIO**; o registro cita precisamente emissor e leitores, sem precisar aproveitar seus números históricos. (`app/docs/BACKLOG.md:2100`)

| ID | Emissor / chave | Consumidor | Classe | Prova decisiva | Selo / dono |
|---|---|---|---|---|---|
| 1 | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`: `foto_ausente.{batida_id,batida_em,tipo_batida,_por}` | `app/chamados/services/disputa_emissao.py:372`, `:387` | **real** | O cron está registrado com `--apply` (`app/config/crons.py:686`); o contexto é passado sem tradução à emissão (`app/chamados/services/disputa_emissao.py:662`). Sem `motivo.data` e `ultima_batida.id`, a emissão pode seguir como conversa (`app/chamados/services/disputa_emissao.py:431`, `:439`). A pergunta de selfie existe e não exige `{data}` no texto (`app/chamados/catalogo/perguntas.py:94`). | Selo do par **[não verificado]**; teste localizado verifica só a escrita de `foto_ausente.batida_id` (`app/ponto/tests/test_bug126_selfie_invisivel.py:65`). **ESTRUTURA**, por desencontro do contrato, segundo L-099 (`CLAUDE.md:179`). |
| 2 | Mesmo emissor (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`) | `app/chamados/models.py:3464`: `ultima_batida.id` | **real** | `foto_ausente` é família BATIDA (`app/chamados/catalogo/motivos.py:43`); o derivador retorna `None` sem `ultima_batida.id` (`app/chamados/models.py:3464`). Esse derivador participa do dedup inicial (`app/chamados/services/disputa_emissao.py:325`) e do nascimento da identidade (`app/chamados/models.py:3449`); o índice de episódio requer origem preenchida (`app/chamados/models.py:3490`). | Selo do par **[não verificado]**; limite do teste em `app/ponto/tests/test_bug126_selfie_invisivel.py:70`. **ESTRUTURA** (`CLAUDE.md:179`). |
| 3 | Mesmo emissor (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`) | `app/api/foto_recebida.py:67`: `ultima_batida.id` | **real** | A comparação que decide tratar a disputa só aceita `ultima_batida.id == batida.pk`; sem isso, pula (`app/api/foto_recebida.py:67`). Os endpoints de upload chamam o helper (`app/api/views.py:856`, `app/api/views_core.py:840`) e têm rotas (`app/api/urls.py:59`, `app/api/urls_core.py:8`). Registro anterior do mesmo defeito: `app/docs/BACKLOG.md:1720`. | Selo do par **[não verificado]**. **ESTRUTURA** (`CLAUDE.md:179`). |
| 4 | `app/ponto/management/commands/processar_alertas_turno.py:348`: `data_turno,entrada_timestamp,posto` | Anexo apontou `_data_furo_da_grade` (`app/chamados/services/disputa_emissao.py:54`) | **falso** | O produtor chama **outra entrada do módulo**: a fábrica de chamados órfãos (`app/ponto/management/commands/processar_alertas_turno.py:385`). Ela deriva origem (`app/chamados/services/disputa_emissao.py:1187`) e dia (`:1205`), ou lê o contexto (`:1232`) pelo helper que reconhece `data_turno` (`app/chamados/data_evento.py:27`), e cria pergunta datada (`app/chamados/services/disputa_emissao.py:1244`). | Selo integral emissor→leitor **[não verificado]**. Parcial do caminho correto: `app/chamados/tests/test_cenarios_disputa.py:84`. Dono do apontamento rejeitado: **ESTRUTURA** (`CLAUDE.md:179`). |
| 5 | `app/ponto/management/commands/processar_alertas_avancados.py:126`: `data_turno,entrada_timestamp` | Mesmo helper apontado pelo anexo (`app/chamados/services/disputa_emissao.py:54`) | **falso** | Também chama diretamente a fábrica (`app/ponto/management/commands/processar_alertas_avancados.py:159`), cujo caminho lê a data e abre pergunta datada (`app/chamados/services/disputa_emissao.py:1232`, `:1244`; `app/chamados/data_evento.py:27`). | Selo integral **[não verificado]**; parcial do caminho correto em `app/chamados/tests/test_cenarios_disputa.py:84`. **ESTRUTURA** (`CLAUDE.md:179`). |
| 6 | `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:128`, persistência `:156` | Mesmo helper apontado pelo anexo (`app/chamados/services/disputa_emissao.py:54`) | **falso** | O comando abre chamado e trata o resultado, sem chamar o gerador ao qual pertence o helper (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`, `:170`). O passivo encaminha chamados à mesma fábrica (`app/chamados/services/pergunta_no_app.py:238`, `:246`), que lê `data_turno` (`app/chamados/services/disputa_emissao.py:1232`; `app/chamados/data_evento.py:27`). | Teste de nascimento, não do par, em `app/ponto/tests/test_passe_turno_aberto_retroativo.py:92`. Selo integral **[não verificado]**. **ESTRUTURA** (`CLAUDE.md:179`). |

**Correção importante do anexo:** `_data_furo_da_grade` realmente ignora `entrada_timestamp`, mas sua única chamada executável encontrada está no gerador de perguntas da outra porta (`app/chamados/services/disputa_emissao.py:350`); os emissores 4–6 usam os caminhos descritos acima. Portanto não são três defeitos reais demonstrados pelo mero cruzamento nominal de produtor e helper. (`app/ponto/management/commands/processar_alertas_turno.py:385`, `app/ponto/management/commands/processar_alertas_avancados.py:159`, `app/chamados/services/pergunta_no_app.py:246`)

**Emissor alternativo não absolve IDs 1–3:** os kernels API gravam `ultima_batida`, mas referente à sua própria batida (`app/api/views.py:758`, `app/api/views_core.py:752`). O merge do retro preserva outras raízes sem recalcular essa referência; assim, uma raiz antiga pode apontar para outro episódio, em vez de suprir a batida que está em `foto_ausente.batida_id`. (`app/chamados/services/disputa_emissao.py:548`, `:561`)

Ocorrência e frequência em produção destes resultados permanecem **[não verificado]**; o anexo fornecido não contém banco nem logs. **[não verificado]**

## 13/11 18:31 · sessao ba2eb57da603 · - · - · origem=Ronald (tmux)

| ID | Emissor | Chaves | Consumidor | Classe | Prova decisiva | Selo específico |
|---|---|---|---|---|---|---|
| 01 | `app/escala/services/propositor.py:788` | retorna `decisao_id,saida`; não retorna `ok,msg` | `app/chamados/services/proposta_no_fio.py:151-155` | **real**, quanto a `ok`, pelo critério literal pedido | A condição viva consulta `ok` ausente e decide sucesso; a rota resolve diretamente este escritor (`app/core/intake.py:49-52,481-487`). O resultado vira mensagem de sucesso/erro no fio (`app/chamados/services/fio.py:509-519`), alcançado por POST de views roteadas (`app/chamados/views.py:667-674,951-954`; `app/chamados/urls.py:21,39`). **Ressalva:** `msg` está no ramo de recusa; com retorno único não vazio de `aplicar`, esse ramo não é atingido. Falso sucesso concreto observado em produção: **[nao verificado]**. | **Não localizado [nao verificado]**. O teste do fio substitui o emissor por `{'ok': True}` (`app/chamados/tests/test_proposta_no_fio.py:106-120`); testes reais de `aplicar` verificam trilha/ACEITA sem atravessar o fio (`app/escala/tests/test_porta_da_proposta.py:148-166`). |
| 02 | `app/escala/services/propositor.py:765-770` | `schema=None` | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:53-63,162` | **escrita morta** | `schema` só é usado por `handle`, desviado por este emissor (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:48-51`). A escolha de `_rodar` é deliberada para conservar o tenant corrente (`app/escala/services/propositor.py:760-764`). Não é ramo morto do comando: é campo supérfluo desse dict. | **Não localizado [nao verificado]** para ausência de efeito de `schema`; testes adjacentes em `app/escala/tests/test_porta_da_proposta.py:148-166`. |
| 03 | `app/escala/views_wizard.py:20-59` | omissão de `descricao`; HPD condicional | `app/escala/services/cadastro_tipo.py:185-188,217,265-267` | **falso** | `descricao` deriva de nome; não é entrada obrigatória (`app/escala/services/cadastro_tipo.py:184-188`). HPD tem emissor real quando enviado (`app/escala/views_wizard.py:53-59`), default histórico documentado e preservação quando ausente (`app/escala/services/cadastro_tipo.py:208-219,265-267`). | **Sim, parcial**: POST real sem HPD (`app/escala/tests/test_wizard_views.py:28-49`); POST real com HPD (`app/escala/tests/test_wizard_views.py:80-90`). Preservação exata de descrição/HPD ausente: **[nao verificado]**. |
| 04 | `app/escala/services/escala_auto_executor.py:65-84` | `_delta_blocos` só para A7 | `app/escala/services/cadastro_tipo.py:443-454` | **falso** | Default zero significa não deslocar blocos; A4 muda pausa e FOLGA muda dia da semana, enquanto A7 desloca horários (`app/escala/services/escala_auto_executor.py:68-83`; `app/escala/services/cadastro_tipo.py:443,451-454`). | **Sim, parcial**: execução real de A4 sem `_delta_blocos`, derivação correta (`app/escala/tests/test_escala_auto_executor.py:66-84`). Assert de blocos preservados especificamente: **[nao verificado]**. |
| 05 | `app/escala/services/escala_auto.py:111-115` | `valores.hfi` | `app/escala/services/escala_auto_executor.py:73-81` | **escrita morta** | Executor usa apenas `hii` e calcula fim pela duração declarada; razão documentada: pausa gozada a menos é intrajornada, não cadastro (`app/escala/services/escala_auto_executor.py:74-80`). A mediana de volta também compõe `para/evidencia` no emissor, mas isso não lê a chave `valores.hfi` (`app/escala/services/escala_auto.py:112-115`). | **Não localizado [nao verificado]** para descarte de `hfi`: teste usa pausa real 14–15 e duração cadastrada 60, que coincidem; portanto não distingue conservar duração de copiar `hfi` (`app/escala/tests/test_escala_auto_executor.py:49-53,78-79`). |
| 06 | `app/escala/services/escala_auto_executor.py:114-115` | `ec_antigo` | `app/escala/management/commands/ajustar_escalas_auto.py:95-99` | **escrita morta** | Chamador usa `ec_id` já conhecido para vínculo antigo e lê os outros cinco campos de resultado (`app/escala/management/commands/ajustar_escalas_auto.py:93-99`). Busca global por `aplicar_no_vinculo`, import do executor e `ec_antigo` encontrou esse único chamador de produção; demais são testes. | **Não localizado [nao verificado]** para descarte da chave; teste de comportamento lê `ec_novo,historico` (`app/escala/tests/test_escala_auto_executor.py:72-84`). |
| 07 | `app/escala/services/escala_auto_executor.py:152` | `historico` de desfazer | `app/escala/management/commands/ajustar_escalas_auto.py:39-42` | **escrita morta** | Único chamador de produção lê `volta,celulas`; imprime `h.pk` do ajuste original, não o histórico de desfazimento (`app/escala/management/commands/ajustar_escalas_auto.py:35-42`). Os testes de desfazer também ignoram seu retorno (`app/escala/tests/test_escala_auto_executor.py:86-101`). | **Não localizado [nao verificado]** para descarte; teste adjacente verifica estado e existência de trilha (`app/escala/tests/test_escala_auto_executor.py:86-95`). |
| 08 | `app/escala/services/propositor.py:644-650` | omissão de `alavanca` | `app/pautas/semanal.py:139-146` | **falso** | Contrato explícito: alavanca quando declarada; senão o contador `n` (`app/pautas/semanal.py:142-146`). O emissor fornece `n` (`app/escala/services/propositor.py:645`). | **Sim, parcial**: entradas sem alavanca na montagem real da pauta e caso com prioridade explícita (`app/pautas/tests/test_pauta_texto.py:113-136`). Travessia do emissor específico: **[nao verificado]**. |

**Subtotal 01–08:** real 1; escrita morta 4; ramo morto 0; falso 3, conforme provas da tabela. O `msg` do item 01 é um subcampo em ramo inalcançável com o retorno atual, mas o item foi contado uma vez pelo consumo vivo de `ok` (`app/escala/services/propositor.py:788`; `app/chamados/services/proposta_no_fio.py:152-155`).

**Dono:** todos são questões de contrato do código, portanto ESTRUTURA como atribuição de manutenção, não CADASTRO/BATIDA (`CLAUDE.md:179`). Aplicação factual do juiz L-099 a dias de produção: **[nao verificado]**, pois exige os insumos descritos em `app/ponto/management/commands/e6_oraculo.py:53-70`.

**Limite sobre item 01:** a recusa silenciosa por âncora futura não demonstra falso sucesso nessa via. O retro retorna `0` na recusa, `_rodar` descarta esse status e `aplicar` registra ACEITA (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:185-189,162`; `app/escala/services/propositor.py:765-776`). Entretanto, a proposta do fio é recalculada (`app/chamados/services/proposta_no_fio.py:105-114`); usa janela encerrada ontem (`app/escala/services/propositor.py:415-417,498-500`); substitui âncora futura prioritariamente por `max(dias_bat)` (`app/escala/services/propositor.py:274-276,313-323`); as outras sugestões também escolhem dias batidos (`app/escala/services/fase_12x36.py:57-64`; `app/escala/services/propositor.py:176-185,349-365`). Não transportar esse cenário para alegação de ocorrência produtiva.

## 13/11 18:31 · sessao 53c794f30147 · - · - · origem=Ronald (tmux)

| id | emissor | chave(s) | consumidor | classe | prova decisiva | selo que prende | dono |
|---|---|---|---|---|---|---|---|
| 12 | `app/ponto/views.py:1087` | faltam `empresa_id,periodo_fechado` | `app/templates/ponto/_fech_linha.html:13`, `:42`, `:47`, via `_fech_lote.html:8` | **real** | Rota viva em `app/ponto/urls.py:29`; lote envia apenas quatro raízes em `views.py:1088`. O include toma decisões de apresentação e preenche o POST com as ausentes. `empresa_id` vazio chega a `views.py:1443` e altera o destino de retorno em `:1457`. A página principal fornece ambas em `views.py:1181`. | **Não demonstrado [não verificado]**. O selo encontrado confere quantidade/ordem/continuação (`app/ponto/tests/test_selo_lotes_fechamento.py:57`, `:66`, `:81`), não essas raízes. | **ESTRUTURA**, atribuição de contrato conforme `CLAUDE.md:179`; não classificação de dia pelo juiz [não verificado]. |
| 13 | `app/ponto/services/triagem_batida.py:206` | grava `tem_bloqueio_turno`; seletor pede `entrada_bloqueada_turno_aberto` | `app/ponto/views.py:229` | **ramo morto** | O contexto nasce local em `views.py:249`, recebe a triagem em `:260` e vai ao seletor em `:358`. O produtor escreve a flag em `triagem_batida.py:206`; a outra chave só identifica módulo de chamado em `views.py:283`, `:299`. A flag real já é consumida em caminho vivo em `views.py:275`; esse caminho expressamente prossegue o registro em `:312`. O ramo do seletor para `turno_regularizar.html` nunca recebe sua flag nesse fluxo. | **Não demonstrado [não verificado]**. `test_f7_triagem_caracterizacao.py:78` afirma a chave produzida, sem afirmar seleção do template. `test_f7_ramos_frios.py:62` confere contexto após POST direto. | **ESTRUTURA**, responsabilidade de ligação do código (`CLAUDE.md:179`); sem defeito de dado demonstrado [não verificado]. |
| 14 | `app/ponto/services/triagem_batida.py:758` | sucesso da tela; `saida_sem_entrada` pertence a outro dict | `app/templates/ponto/triagem/apto.html:39` | **ramo morto** | O alerta fica dentro do **else de `sucesso`** (`apto.html:4`, `:37`). A gravação bem-sucedida define `sucesso=True` (`triagem_batida.py:758`). `saida_sem_entrada` é enviada ao criador de disputa em `:735`, dentro do dict de `:738`, sem mutar o contexto da tela. GET tampouco recebe essa flag (`views.py:249`, `:260`). | **Não demonstrado [não verificado]**. O teste de saída encontrado verifica disputa, não alerta renderizado (`app/ponto/tests/test_saida_cross_midnight_view.py:49`). | **ESTRUTURA**, responsabilidade do ramo de apresentação (`CLAUDE.md:179`); não BATIDA. |
| 15 | `app/ponto/views.py:2079`; complementos `:2032`, `:2046` | ausência de `volta_url` | `app/templates/ponto/partials/painel_ausencia.html:282` | **falso**, por default explícito correto para o chamador localizado | O hidden vazio é enviado ao endpoint que define `POST.volta or reverse('ponto:lista_ausencias')` (`views.py:2581`). A própria lista abre esse drawer (`app/templates/ponto/ausencias.html:217`, `:228`). As ações irmãs igualmente retornam à lista (`views.py:2657`, `:2715`). Exigência de preservar filtros/query ou voltar a outra página **[não verificado]**; não transformar esse requisito ausente em defeito. | **Não demonstrado [não verificado]**. `test_reverter_rejeicao.py:27` importa/testa o serviço; `test_drawer_ausencia_no_molde.py:191` verifica rodapé de aberta/aprovada, não `volta`. | **ESTRUTURA** como dono do contrato inspecionado (`CLAUDE.md:179`); não há divergência real demonstrada. |

Totais deste recorte: **1 real, 2 ramo morto, 1 falso, 0 escrita morta**, conforme os quatro pares e provas acima.

Correção do anexo: #13 e #14 não são efeitos demonstrados de produção; são ramos sem flag de tela. #15 não comprova defeito de navegação, pois há destino explícito no endpoint; #12 continua real no caminho roteado. (`app/ponto/views.py:229`, `:249`, `:2581`; `app/templates/ponto/triagem/apto.html:37`; `app/ponto/urls.py:29`)

Não acrescentei novos desencontros confirmados neste recorte; completude das costuras próximas **[não verificado]**. Não executei testes nem consultei banco/log; resultados de execução **[não verificado]**. [nao verificado]

## 13/11 18:31 · sessao c9ed8d5d77b4 · - · - · origem=Ronald (tmux)

Numeração considerada: 09–17 são os nove últimos desencontros do anexo, excluída a linha “Mesmo resultado” que já dizia não haver desencontro `[nao verificado: numeração não existe explicitamente no anexo]`.

| ID | Emissor | Chaves | Consumidor | Classe | Prova decisiva | Selo que prende o apontamento |
|---|---|---|---|---|---|---|
| 09 | `app/escala/services/fase_12x36.py:229-234` | Ausência de `alavanca` | `app/pautas/semanal.py:139-146` | **falso** | O contrato documenta expressamente “quando [alavanca] existe”; senão usa o próprio contador `n` (`app/pautas/semanal.py:142-146`). | **Sim, parcial:** ordenação por alavanca em `app/pautas/tests/test_pauta_texto.py:129-136`; produtor de fase lendo lavra em `app/escala/tests/test_o261_v_fase_na_lista.py:254-260`. Teste específico fase→default de alavanca `[nao verificado]`. |
| 10 | `app/escala/models.py:486-487` | `ciclico` | `app/core/templatetags/hasner_filters.py:72-123,207-217` | **escrita morta** | Os filtros usam `tipo/apelido/dias/horario`; não usam `ciclico`. O outro consumidor interno, dropdown, também só usa essas quatro chaves (`app/escala/models.py:528-536`); ficha idem (`app/colaboradores/services/ficha.py:82-90`). | **Não**, para ausência de consumo. Há selo só de emissão da chave, `app/escala/tests/test_nome_canonico.py:85,95`. |
| 11 | `app/escala/models.py:486-487` | Mesmo `ciclico` do ID10 | `app/colaboradores/services/ficha.py:82-90` | **escrita morta** | Mesmo campo e mesmo conjunto de consumidores do ID10; a ficha só extrai quatro outras chaves (`app/colaboradores/services/ficha.py:70-71,85-90`). | **Não**, para ausência de consumo; selo do cabeçalho usa dias/horário e não `ciclico` (`app/colaboradores/tests/test_ficha_cabecalho_compacto.py:28-40`). |
| 12 | `app/escala/regua_defesa.py:632-633` | `faixa/escore/texto` | `app/escala/services/furos_diarios.py:36,305-309` | **escrita morta** | Consumidores usam existência e `classe` (`app/escala/services/furos_diarios.py:36,306-309`; `app/chamados/juizes.py:516`). O docstring pede leitura inteira, mas os leitores não a fazem (`app/escala/regua_defesa.py:621-625`). | **Não**, para o não consumo. Selo apenas do produtor para `classe/texto/faixa` em `app/escala/tests/test_termometro_regua.py:175-188`. |
| 13 | `app/escala/regua_defesa.py:632-633` | Mesmos `faixa/escore/texto` do ID12 | `app/chamados/juizes.py:514-516` | **escrita morta** | Aqui se lê só `None`; porém o anexo exagera se incluir `classe` entre os campos sem leitor: `classe` é lida por outro consumidor (`app/escala/services/furos_diarios.py:306-309`). | **Não**, para o não consumo; teste do produtor citado no ID12 (`app/escala/tests/test_termometro_regua.py:175-188`). |
| 14 | `app/escala/regua_defesa.py:313` | Ausência de `data_ancora/confirm_txt` | `app/colaboradores/services/vinculo.py:57-59,146` | **falso** | O emissor recusa destino cíclico **antes** de formar POST (`app/escala/regua_defesa.py:310-313`); a exigência de âncora é para criação cíclica (`app/colaboradores/services/vinculo.py:126-130`). `confirm_txt` é texto opcional de observação, concatenado apenas quando existe (`app/colaboradores/services/vinculo.py:354-356`). | **Sim:** integração pela mesma porta, sucesso não cíclico e recusa cíclica em `app/escala/tests/test_termometro_camada3.py:81-106`. |
| 15 | `app/escala/views.py:294-299` | `tamanhos.tamanho/dias` | `app/templates/escala/folga_calendarios_lista.html:21-25` | **escrita morta** | O contexto tem destino direto e único no render (`app/escala/views.py:311`); o template só lê `t.canonico` (`app/templates/escala/folga_calendarios_lista.html:22`). | **Não**, para chaves mortas; teste existente só inspeciona `semana_dia_rotulo` (`app/escala/tests/test_editor_folga_modo.py:66-71`). |
| 16 | `app/escala/views.py:363-371` | `tipos`, `tamanhos.dias_range` | `app/templates/escala/folga_calendario_editar.html:67-80` | **escrita morta** | O JS recebe `tamanho/rotulo/n/marcados`; o intervalo de dias é construído por `d <= t.n` (`app/templates/escala/folga_calendario_editar.html:68,80`). `tipos` não participa desse template `[nao verificado para consumidores externos à cópia]`. | **Não**, para órfãs. **Há smoke de JS omitido pelo anexo:** explica execução real e reenvio do POST em `app/escala/tests/test_smoke_clique_folga_calendario.py:7-20`, com prova de modo semana×dia em `:163-170`. |
| 17 | `app/escala/views_plano_folgas.py:158,190-191` | `gravadas/celulas_relavradas/barrados`; ausência de `ok` no erro | `app/templates/escala/plano_folgas.html:174-189` | **escrita morta** | Para a classe única, prevalecem os três campos emitidos sem leitura pelo JS. A parte “erro sem `ok`” é **falsa**: `if(!j.ok)` entra exatamente no erro e lê `j.erro` (`app/templates/escala/plano_folgas.html:176`). O JS só usa `ok/erro/erros` (`:176-189`). | **Não**, para descarte desses três campos; teste do endpoint prende `erros[].erro`, não os três, em `app/escala/tests/test_bug136_plano_em_lote_relavra.py:75-82`. |

**Totais dos nove itens:** 0 real; 7 escrita morta; 0 ramo morto; 2 falso. Os sete registros mortos correspondem a cinco conjuntos de emissão: ID10=ID11 e ID12=ID13 repetem os mesmos campos (`app/escala/models.py:486-487`; `app/escala/regua_defesa.py:632-633`; demais emissores nas linhas da tabela).

**Dono:** para efeitos de protocolo, os campos mortos dizem respeito à **ESTRUTURA**, não demonstram escala cadastrada errada nem batida faltante; para os falsos, não há defeito a atribuir. A atribuição L-099 de uma ocorrência real de produção permanece `[nao verificado]`: o juiz exige insumos do dia e do cadastro, ausentes no dado fornecido (`app/ponto/management/commands/e6_oraculo.py:55-110`; `CLAUDE.md:179`).

A busca de leitores nomeados foi global em `app/`, com os padrões exatos `rotulo_regua_para_espelho`, `nome_canonico_partes`, `plano_folgas_aplicar`, `folga_calendarios_lista.html` e `folga_calendario_editar.html`, incluindo Python/HTML/JS. Para `ciclico`, a busca global da chave encontrou leituras de produção de um **outro contexto** de fase (`app/colaboradores/views_fase.py:51,104`; `app/templates/colaboradores/partials/_fase_12x36.html:6-7`); isso não é leitura do dict de `nome_canonico_partes` (`app/escala/models.py:486-487`). Consumidores dinâmicos não nomeados e consumidores externos à cópia `[nao verificado]`.

## 13/11 18:29 · sessao 9eac57b99568 · - · - · origem=Ronald (tmux)

- **BLOQUEIA — período apagado aborta a reversão restante.** O command faz `.get(pk=item['periodo'])` sem tratar ausência; períodos anteriores podem já ter sido revertidos e posteriores deixam de ser processados (`app/ferias/management/commands/realinhar_concedidos_ferias.py:100-104`; `app/ferias/services.py:190-200`). É alcançável pela porta regular: cancelar agendamentos e excluir o PA (`app/ferias/services.py:781-794`); a trilha permanece possível porque `objeto_id` não é FK (`app/core/models.py:298`).

- **A — OK nos estados examinados.** Identidade inclui `pk`, `dias`, início e fim, cobrindo remarcação que mantém datas e muda dias (`app/ferias/services.py:139-140`). A porta também atualiza concedidos pelo delta dos dias (`app/ferias/services.py:652-653`). O RED novo morde a identidade antiga `[pk,dias]`, pois troca somente datas (`app/ferias/test_hj090_volta3.py:261-268`). Execução do RED: **[nao verificado]**.

- **B — OK na seleção do último ato por período.** A consulta filtra modelo e PA antes de ordenar por PK; atos intercalados de PAs distintos não se contaminam (`app/ferias/services.py:177-179`). Os testes mordem replay de A depois de B e intenção sem commit (`app/ferias/test_hj090_volta3.py:270-296`). Não cobrem retirar o filtro por PA dessa consulta (`app/ferias/test_hj090_volta3.py:270-296`). Execução: **[nao verificado]**.

- **C — cura externa fecha `{}`, mas validação interna permanece incompleta.** Só exige que `vivos` seja lista; aceita `[null]`, `[[1]]` ou datas malformadas (`app/ferias/management/commands/realinhar_concedidos_ferias.py:69-71`). Esses formatos serão tratados como “linha mudou”, eventualmente depois de reverter linhas anteriores (`app/ferias/services.py:192-194`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:101-104`). Confirmação com funções extraídas por AST e `BytesIO`, sem arquivo: leitor aceitou duas linhas, sendo a segunda com `vivos=[None]` **[nao verificado por arquivo de prova]**. O RED só cobre formatos externos mínimos (`app/ferias/test_hj090_volta3.py:298-309`).

- **NAO-BLOQUEIA condicionado ao acervo — incompatibilidade anterior.** Registros sem `depois.ato` agora são recusados pelo validador (`app/ferias/management/commands/realinhar_concedidos_ferias.py:69-71`). Existência desses registros em produção: **[nao verificado]**.

- **OK — escrita explícita e falha de trilha.** `--reverter --por` é uma porta de escrita expressamente documentada; execução normal sem `--apply` retorna antes de criar arquivos (`app/ferias/management/commands/realinhar_concedidos_ferias.py:19-24`; `app/ferias/management/commands/realinhar_concedidos_ferias.py:95-119`). `_trilha` transforma retorno `None` em erro dentro da transação (`app/ferias/services.py:421-429`; `app/ferias/services.py:153-165`).

- **NAO-BLOQUEIA — deadlock específico não demonstrado.** Realinhar/reverter travam PA, mas consultam agendamentos sem `select_for_update`; a ordem agendamento→PA da remarcação não estabelece ciclo de espera com essas portas (`app/ferias/services.py:105-113`; `app/ferias/services.py:139-140`; `app/ferias/services.py:154`; `app/ferias/services.py:577-580`). Concorrência executada: **[nao verificado]**.

## 13/11 18:29 · sessao a3cfceb0b3b9 · - · - · origem=Ronald (tmux)

D: **BLOQUEIA pela cobertura incompleta do calendário**, embora criação e edição agora concordem: ambas usam `datas_do_ciclo` (`app/ferias/views.py:167`; `app/ferias/services.py:770`). A criação promete linha completa de ciclos (`app/ferias/views.py:157`).

Contraexemplo confirmado executando somente as duas funções reais extraídas via AST, em memória: admissão em 29/02/2024 produz ciclo 28/02/2027–27/02/2028 e próximo ciclo começando em 29/02/2028; **28/02/2028 não pertence a ciclo algum**. O início é calculado pela admissão original e o fim pelo início já truncado (`app/ferias/views.py:164`; `app/ferias/services.py:742`; `app/ferias/services.py:754`). O algoritmo já existia na criação apresentada no delta; a extração conservou esse defeito. Existência desse caso em produção: [nao verificado].

Esse buraco tem leitores concretos: faltas usam intervalo inclusivo e a falta desse dia fica fora de todos os PAs (`app/ferias/models.py:147`); o gozo histórico busca PA contendo `gi`, mas, sem resultado, escolhe o PA mais recente de todo o acervo, possivelmente posterior ao gozo (`app/ferias/views.py:199`; `app/ferias/views.py:203`). A porta valida futuro, intervalo e saldo, mas não a pertinência de `gi` ao PA escolhido (`app/ferias/services.py:460`; `app/ferias/services.py:475`; `app/ferias/services.py:487`).

Último dia do mês: a função entrega 31/01/2024→30/01/2025, limite 30/01/2026; 31/03/2025→30/03/2026, limite 30/03/2027. Isso corresponde à fórmula documentada, aniversário menos um dia (`app/ferias/services.py:750`). Para início em 29/02/2024 entrega 27/02/2025 e limite 27/02/2026, porque primeiro trunca o aniversário para 28/02 e depois subtrai um dia (`app/ferias/services.py:743`; `app/ferias/services.py:754`).

**RED D: mordida parcial.** Restaurar a antiga aritmética da edição reprovaria o teste de 01/01/2024; porém restaurar a conta independente na criação sobreviveria, pois o novo teste não chama a view. Também não cobre 29/02 ou último dia do mês (`app/ferias/test_hj090_volta3.py:316`).

**Juiz: OK quanto a duplicação registrada.** As autoridades registradas são saldo, status/vencimento, abono, fracionamento, aviso e duração da ausência; nenhuma responde às datas do ciclo aquisitivo. `fim_de` trata duração da ausência, outra pergunta (`app/core/juizes.py:268`). A exigência de corte refere-se ao registro de autoridade nova em `core/juizes.py` (`CLAUDE.md:74`).

Mapa dos campos de datas:

- **Escritores:** criação pela porta `garantir_periodo_aquisitivo`, recebendo início e defaults (`app/ferias/services.py:378`); edição pela porta travada (`app/ferias/services.py:758`). Criação entrega as datas calculadas pela função comum (`app/ferias/views.py:164`).
- **Leitores inclusivos:** faltas (`app/ferias/models.py:147`), sobreposição (`app/ferias/models.py:193`), PA do gozo histórico (`app/ferias/views.py:199`), PA vigente no painel (`app/ponto/views.py:2022`).
- **Leitores de vencimento:** status considera ciclo aberto enquanto `data_fim >= hoje`, vencimento apenas quando `limite < hoje` (`app/ferias/models.py:101`); situação usa limite inclusivo para aviso e exclusivo para vencido (`app/ferias/situacao.py:193`; `app/ferias/situacao.py:209`).
- **Leitores de apresentação/ordenação:** API (`app/api/views_mensageria.py:837`), CSV (`app/relatorios/views.py:365`), seção do colaborador (`app/templates/ferias/_secao_colab.html:41`), lista DP (`app/ferias/management/commands/listar_ferias_dp.py:66`; `app/ferias/management/commands/listar_ferias_dp.py:101`).
- **Admin/migration/command:** admin registra somente `Afastamento` (`app/ferias/admin.py:2`); migration inicial declara os campos, sem derivação (`app/ferias/migrations/0001_initial.py:40`); cron de vencimento lê limite e delega sincronização (`app/ferias/management/commands/alertar_ferias_vencendo.py:26`). `FeriasConfig` não registra signals (`app/ferias/apps.py:3`). Não encontrei outra derivação produtiva das datas na busca integral; execução dinâmica fora dos arquivos examinados: [nao verificado].

## 13/11 18:29 · sessao 81ac17b751e1 · - · - · origem=Ronald (tmux)

Resultado da fatia vigia/listas, somente leitura:

- **OK — merge do carimbo.** `ferias_soma_maior`, `ferias_concedidos_maior` e `desligados_retidos` aparecem em `ESPERADO_DO_CARIMBO` com zero e no `censo` com as mesmas grafias. O contador de férias é chamado diretamente, sem outra regra de saldo. `app/chamados/management/commands/vigiar_fio_morto.py:100`, `:234`, `:250`, `:252`.

- **OK — leitor ESTADO.** O leitor compara genericamente todas as entradas de `esperado` com `censo`; assim lê também as três chaves do merge e publica suas divergências. A integração do ESTADO chama esse leitor. `bin/fio_estado.py:121`, `:125`, `:191`; `bin/gerar_estado.py:260`.

- **NAO-BLOQUEIA — placar não alarma por essas três chaves.** `pipeline_placar` chama seu leitor somente para `fio_mudo_alvos` e `nunca_bateu_sem_adesao`; o próprio vigia também não inclui férias/desligados em seus conjuntos de alarme. Portanto coerência do carimbo não significa alarme dessas chaves no placar. `app/core/management/commands/pipeline_placar.py:135`; `app/chamados/management/commands/vigiar_fio_morto.py:84`. Leitor dessas chaves em hook externo: **[nao verificado]**; nenhum encontrado nos hooks da cópia.

- **OK — escritores/leitores das chaves férias.** A autoridade produz listas de PAs em `services.py`, o vigia grava seus comprimentos; tripwire e lista DP consultam a mesma autoridade. `app/ferias/services.py:116`, `:123`; `app/chamados/management/commands/vigiar_fio_morto.py:234`; `app/ferias/management/commands/tripwire_status_ferias.py:37`; `app/ferias/management/commands/listar_ferias_dp.py:55`.

- **NAO-BLOQUEIA — significado de `desligados_retidos`.** Origem conta somente colaboradores retidos por `furo_espelho` e julgados desligados; agregação soma ocorrências colaborador×competência, não pessoas únicas da frota. Há fila explícita para a amplitude do rótulo. `app/folha/fora_da_folha.py:122`, `:128`, `:143`; `app/chamados/services/vigia_fio.py:583`, `:587`; `app/docs/BACKLOG.md:736`. O merge conserva a mesma conta em censo e esperado. `app/chamados/management/commands/vigiar_fio_morto.py:102`, `:252`.

- **OK — teste do carimbo tem mordida estática observável.** Cria sentidos não vazios com comprimentos diferentes, exige uma consulta à autoridade e verifica valores e esperados: retiradas, zeros fixos ou inversão dos sentidos seriam detectados. O teste completo do vigia exige também `desligados_retidos` na igualdade dos dois blocos. `app/ferias/test_hj090_volta3.py:206`, `:214`, `:221`, `:224`; `app/chamados/tests/test_o261_vigia_fio.py:819`, `:833`. Execução/RED real **[nao verificado]**.

- **OK — tripwire diferencia alarme e DP.** Ambos os sentidos retiram a mensagem OK; somente divergência de status ou soma maior provoca saída 2. O caso concedidos maior é publicado como LISTA-DP. `app/ferias/management/commands/tripwire_status_ferias.py:41`, `:52`, `:56`; testes em `app/ferias/test_hj090_um_juiz.py:461`, `:470`.

- **NAO-BLOQUEIA — duas leituras de relógio na lista nova.** `listar_ferias_com_batida` obtém `hoje` e `agora` separadamente. Na passagem da meia-noite pode entregar o dia anterior com um instante do dia seguinte a `InsumosDoDono`; ambos afetam classificação e ordenação da lista. É inconsistência da borda, não leitura fora dela. `app/ferias/management/commands/listar_ferias_com_batida.py:36`, `:52`, `:61`. Não existe teste da virada nessa lista; o teste apresentado cobre uma ausência sem agendamento. `app/ferias/test_hj090_um_juiz.py:478`.

- **OK com limite — comandos de listas.** Caminho normal da lista DP consulta e imprime; `--simular` escreve deliberadamente na sombra dentro de transação revertida por `_Volta`, com erros impressos. Não demonstrado commit persistente sem `--apply`. `app/ferias/management/commands/listar_ferias_dp.py:52`, `:55`, `:98`, `:109`, `:112`. Efeitos externos ao banco durante signals dessa simulação: **[nao verificado]**.

- **OK — universo da lista com batida declarado.** Ausência de férias sem filtro de status × batida apurável do dia civil; a informação do dia do turno serve para marcar plantão da véspera. Isso não é a pergunta de retenção do TXT. `app/ferias/management/commands/listar_ferias_com_batida.py:4`, `:40`, `:44`, `:46`, `:53`.

- **OK — descarte da trava de competência fechada na materialização corresponde ao contrato local.** O próprio materializador declara que período fechado não recusa esse caminho e materializa por `criar_ausencia`. `app/ponto/services/ausencia.py:1169`, `:1203`. Não afirmo situação de produção **[nao verificado]**.

Limite do exame: não há repositório `.git` na cópia; confirmação histórica de “preexistente/fora do diff”, hook instalado, execução e dados reais **[nao verificado]**.

## 13/11 18:25 · sessao 6adac308e008 · RAIA-PREPARACAO · censo · origem=Code

**LEU** = sítio aberto na cópia fornecida; **INFERIU** = interpretação ou proposta apoiada nos sítios citados. A identidade Git do HEAD, o histórico dos commits e a aplicação das refs são **[nao verificado]**: esta cópia não disponibiliza repositório Git. [nao verificado]

Os cruzamentos e os indicadores `main_moveu` abaixo vêm dos anexos fornecidos; confirmação independente desses metadados: **[nao verificado]**. Os estados do BACKLOG são tratados como registros, sem substituir a leitura do código.

## COBPARC-FILA-RELOGIO

1. **O que cura — LEU.** Fora do julgamento, `agora_deste_julgamento()` pode devolver `None`; recebendo teto ausente, `houve_fato_novo` consulta novamente o relógio. Assim, chamadas do mesmo lote podem avaliar instantes diferentes. Sítios: `app/ponto/services/cartorio.py:82`, `app/ponto/services/cartorio.py:373`.

2. **Existe no HEAD? SIM — LEU.** O c2 ainda passa o instante potencialmente ausente; o cluster faz o mesmo, e a retenção chega ao predicado sem instante fornecido pela borda. A guarda de reabertura também consulta fato novo sem teto explícito. Fontes: `app/chamados/reconciliador.py:877`, `app/chamados/reconciliador.py:884`, `app/ponto/services/cluster_espurio.py:164`, `app/ponto/services/cartorio.py:428`, `app/chamados/services/pergunta_no_app.py:144`, `app/chamados/services/disputa_emissao.py:940`, `app/chamados/models.py:1347`.

3. **Falta / reaplicação.** O BACKLOG registra mutantes_v4, suíte, opus e resposta do padrão pendentes; o censo da raia registrado ali não prova o HEAD. Fonte: `app/docs/BACKLOG.md:860`.

   **INFERIU:** exige adaptação ao caminho atual. O comando `reconciliar_chamados` agora é vigia; a retratação foi extraída para `retratar_premissa_morta_colab`, alcançada pelo consumidor. Reaplicar o antigo laço escritor do cron recuperaria um desenho ultrapassado. Fontes: `app/chamados/management/commands/reconciliar_chamados.py:1`, `app/chamados/management/commands/reconciliar_chamados.py:75`, `app/chamados/reconciliador.py:1446`, `app/chamados/reconciliador.py:1457`.

   O anexo marca `main_moveu=True` nestes arquivos — **[nao verificado]** por Git:

   - `app/chamados/management/commands/{disparar_perguntas_competencia,reabrir_orfas_enterradas,reconciliar_chamados,smoke_chamados}.py`;
   - `app/chamados/{models,reconciliador}.py`;
   - `app/chamados/services/{adesao,becos,competencia_trancada,disputa_emissao,fabrica_da_celula,pergunta_no_app}.py`;
   - `app/chamados/tests/test_selo_universo_reconciliador.py`;
   - `app/ponto/management/commands/{detectar_ausencias,detectar_cluster_espurio,processar_alertas_avancados,processar_alertas_turno,reconciliar_fantasmas,supra_juiz}.py`;
   - `app/ponto/services/{cartorio,cluster_espurio,geofence,justificativa}.py`;
   - `app/ponto/tests/test_o_outbox_celula.py` e `app/ponto/views.py`.

   Aplicação limpa e conflitos textuais: **[nao verificado]**.

4. **Arquivos em comum.** Comparação dos anexos: `app/chamados/models.py` com **BUG-PAR/O168/E3**; `app/chamados/services/disputa_emissao.py` com **BUG-PAR/E3**; nenhum com HJ-090. Confirmação fora dos anexos: **[nao verificado]**.

5. **Depende de.** Resposta do padrão e estacionamento por aval continuam registrados; a própria linha distingue estacionamento de trava L1/contrato 20. Não registra `!` do motor como bloqueio desta obra. Fonte: `app/docs/BACKLOG.md:860`.

6. **PROPOSTA — INFERIU: CONCLUIR**, após estabilizar as sobreposições com BUG-PAR/O168/E3 e resolver o estacionamento registrado, mantendo a prioridade inferior às obras bloqueantes L1/contrato 20. O defeito persiste, mas a retomada deve seguir os caminhos por evento do HEAD. Fontes: `app/docs/BACKLOG.md:860`, `app/ponto/services/cartorio.py:373`, `app/chamados/reconciliador.py:1446`.

## V3-RETOMA-PERIODOS-DA-CELULA

1. **O que cura — LEU.** A alimentação entrega segmentos como períodos e passa pausa agregada do núcleo; a intra perde a unidade de cada período. Além disso, a CCT é resolvida em `ini`, e o split noturno usa as extras do motor da fatia. Fontes: `app/ponto/calculador/alimentacao.py:158`, `app/ponto/calculador/alimentacao.py:515`, `app/ponto/calculador/regras.py:764`, `app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/regras.py:827`.

2. **Existe no HEAD? PARCIAL — LEU.**

   - **Permanece:** segmentos em `pares_por_dia`, intra diária, CCT em `ini` e split pelos minutos do motor. Fontes: `app/ponto/calculador/alimentacao.py:158`, `app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/regras.py:764`, `app/ponto/calculador/regras.py:827`.
   - **P4 permanece:** período sem dia/carimbo incrementa contador e segue por `continue`; a porta inicia o cálculo sem a recusa proposta. Fontes: `app/ponto/calculador/alimentacao.py:79`, `app/ponto/calculador/alimentacao.py:91`, `app/ponto/calculador/alimentacao.py:449`.
   - **P5 já está presente:** união dos intervalos, cortes por horas e teto no total. O RELATO atribui o pouso a **16c27de8**. Fontes: `app/ponto/motor_calculo_v2.py:162`, `app/ponto/motor_calculo_v2.py:186`, `app/ponto/motor_calculo_v2.py:196`, `app/docs/RELATO.md:7673`.
   - **Outra cura já presente:** a dobra deixou de retornar antes da noturna/pontualidade, e os intervalos já atravessam a alimentação. O RELATO registra **424632fd**, merge de **f9a65d66**. Fontes: `app/ponto/calculador/regras.py:793`, `app/ponto/calculador/alimentacao.py:107`, `app/ponto/calculador/alimentacao.py:527`, `app/docs/RELATO.md:7720`.

   Os hashes acima são atribuições documentais; ancestralidade Git: **[nao verificado]**.

3. **Falta / reaplicação.** Falta concluir períodos e pausas por período, CCT do dia, distribuição reversa da HE e recusa P4, com os REDs e a catraca pedidos. Fonte: `app/docs/BACKLOG.md:964`.

   **INFERIU:** P4 precisa chegar como resultado declarado ao consumidor, pois o fechamento captura genericamente falhas do oráculo. Fontes: `app/ponto/services/fechamento.py:1266`, `app/ponto/services/fechamento.py:1297`.

   O anexo marca `main_moveu=True` em `app/ponto/calculador/{alimentacao,periodo,regras}.py`, `app/ponto/motor_calculo_v2.py` e `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py` — **[nao verificado]** por Git.

   **INFERIU:** reaproveitar somente os deltas restantes, preservando P5, a retirada do retorno da dobra e o transporte atual dos intervalos. Esses trechos da raia já se sobrepõem a curas presentes. Fontes: `app/ponto/motor_calculo_v2.py:162`, `app/ponto/calculador/regras.py:793`, `app/ponto/calculador/alimentacao.py:107`. Conflitos textuais: **[nao verificado]**.

4. **Arquivos em comum.** Nenhum dos arquivos da raia aparece na lista fornecida das quatro obras em voo. Confirmação fora dos anexos: **[nao verificado]**.

5. **Depende de.** O desenho de períodos, ancoramento e transbordo reverso já está registrado; “espera P5” não descreve mais integralmente o código atual. Fontes: `app/docs/BACKLOG.md:964`, `app/ponto/motor_calculo_v2.py:162`.

   Nova alteração do motor exige aval explícito; a troca V3 ainda tem cobertura integral do impacto e lista de `!` por exportada registradas como pendentes. RECORTE-LONGO e HE-NOTURNA-POR-PEÇA são obras distintas. Fontes: `CLAUDE.md:142`, `app/docs/BACKLOG.md:1797`, `app/docs/BACKLOG.md:966`, `app/docs/BACKLOG.md:972`.

6. **PROPOSTA — INFERIU: CONCLUIR**, como restante da retomada V3, preservando as partes já recebidas. Manter a posição documentada da V3 — após O10-B, antes de OTIMIZA-3 — condicionada ao impacto completo; situação atual dessas obras antecedentes: **[nao verificado]**. Fontes: `app/docs/BACKLOG.md:1797`, `app/ponto/calculador/alimentacao.py:158`, `app/ponto/calculador/regras.py:764`.

## V3-RETOMA-FILA

1. **O que cura — LEU.** O instrumento descarta a lavra real e deriva sozinho o lado v2 do DIFF, sem ler HE100 de feriado nem os splits noturnos; também reúne toda folga trabalhada numa rubrica. Fontes: `app/ponto/management/commands/diff_calculador.py:123`, `app/ponto/management/commands/diff_calculador.py:137`, `app/ponto/management/commands/diff_calculador.py:245`, `app/ponto/management/commands/diff_calculador.py:263`.

2. **Existe no HEAD? SIM para o instrumento — LEU.** A lavra continua substituída por `_nada`, e os laços próprios continuam formando o lado v2. A lavra verdadeira contém as rubricas e a separação de folgas omitidas. Fontes: `app/ponto/management/commands/diff_calculador.py:137`, `app/ponto/management/commands/diff_calculador.py:247`, `app/ponto/services/dia_pago.py:181`, `app/ponto/services/dia_pago.py:234`, `app/ponto/services/dia_pago.py:244`.

   O item CCT em `ini` também persiste; os números de divergência e o caso nominal de cadastro dos demais itens são registros do BACKLOG, sem confirmação atual da frota. Fontes: `app/ponto/calculador/alimentacao.py:390`, `app/docs/BACKLOG.md:976`; ocorrência atual: **[nao verificado]**.

3. **Falta / reaplicação.** A raia fornecida resolve o instrumento; não encerra, por si, o re-DIFF, a classificação das divergências ou o cadastro. A linha ainda registra dependência do pouso da V3. Fonte: `app/docs/BACKLOG.md:976`.

   **INFERIU:** a captura precisa distinguir lavra completa, ausente e falha, porque o fechamento absorve exceções da lavratura. Fonte: `app/ponto/services/fechamento.py:1238`.

   Segundo o anexo, **nenhum** dos dois arquivos tem `main_moveu=True`. Isso não certifica aplicação limpa; aplicação e conflitos textuais: **[nao verificado]**.

4. **Arquivos em comum.** Nenhum dos dois arquivos da raia consta na lista das obras em voo. Confirmação fora dos anexos: **[nao verificado]**.

5. **Depende de.** Pouso próprio de instrumento L-105 e dependência POUSO-DA-V3 registrada. A correção da CCT deve ser consolidada com a obra de períodos, pois ambas atingem o mesmo sítio — **INFERIU**. Fontes: `app/docs/BACKLOG.md:976`, `app/docs/BACKLOG.md:964`, `app/ponto/calculador/alimentacao.py:390`.

6. **PROPOSTA — INFERIU: CONCLUIR**, em pouso de instrumento, antes do próximo DIFF usado para apresentar o impacto da V3. Essa posição sugerida precisa ser conciliada pelo chat com o bloqueio POUSO-DA-V3 registrado; não considero essa dependência revogada. Fontes: `app/docs/BACKLOG.md:976`, `app/ponto/management/commands/diff_calculador.py:247`.

## OTIMIZA-1

1. **O que cura — LEU.** Falta registrar as suítes inteiras e publicar inteiras por pouso no ESTADO. A trava atual mantém o dono do lock e executa o comando, sem produzir essa medição. Fontes: `app/docs/RELATO.md:1648`, `bin/trava_teste.sh:35`, `bin/trava_teste.sh:42`.

2. **Existe no HEAD? SIM — LEU.** A execução termina em `"$@"`, e a composição atual do painel não inclui o quadro contratado de suítes/pouso. Fontes: `bin/trava_teste.sh:42`, `bin/gerar_estado.py:370`. Commit de eventual cura por outro caminho: **[nao verificado]**.

3. **Falta / reaplicação.** Suíte, replay, verificador e pouso próprio permanecem registrados; PRONTO 5 não tem mapa histórico promovido no relato consultado. Fontes: `app/docs/BACKLOG.md:2083`, `app/docs/RELATO.md:1637`.

   O anexo marca `main_moveu=True` em `bin/gerar_estado.py`, `bin/suite.sh` e `bin/tests/test_suite_sh.sh` — **[nao verificado]** por Git.

   **INFERIU:** o porte deve preservar as novas seções do painel e as guardas de motivo da suíte inteira, junto do selo correspondente. Fontes: `bin/gerar_estado.py:277`, `bin/suite.sh:133`, `bin/tests/test_suite_sh.sh:31`. Conflitos textuais: **[nao verificado]**.

4. **Arquivos em comum.** A raia original, composta por `bin/*`, não cruza a lista das obras em voo. O complemento de carga alcança `app/chamados/management/commands/vigiar_fio_morto.py`, comum a **BUG-PAR/HJ-090/E3** na lista raspada. Cruzamentos fora dos anexos: **[nao verificado]**. O complemento está expressamente registrado para depois da E3: `app/docs/BACKLOG.md:738`.

5. **Depende de.** SUITE-POR-IMPACTO/PRONTO 5 para fechar a medição do comboio e pouso separado de instrumento pela L-105. O complemento de carga exige contrato anterior e depende da E3; isso não é evidência de implementação do complemento. Fontes: `app/docs/RELATO.md:1645`, `app/docs/LEIS.md:131`, `app/docs/BACKLOG.md:738`.

6. **PROPOSTA — INFERIU: CONCLUIR**, como fechamento do mesmo comboio de SUITE-POR-IMPACTO, depois da prova PRONTO 5. Manter o complemento de carga separado como pendência pós-E3, sem declará-lo construído. Fontes: `app/docs/BACKLOG.md:2083`, `app/docs/RELATO.md:1637`, `app/docs/BACKLOG.md:738`.

## SUITE-POR-IMPACTO [nao verificado]

1. **O que cura — LEU.** Selecionar testes por cobertura teste→arquivo, mantendo os casos de suíte inteira e certificando a árvore do pouso. Hoje `--volta` seleciona por `vizinhos_da_volta.py`; não entrega o contrato de coverage. Fontes: `app/docs/PROMPTS.md:1682`, `bin/suite.sh:82`, `bin/suite.sh:88`.

2. **Existe no HEAD? SIM no núcleo; PARCIAL nas guardas adjacentes — LEU.** A seleção por vizinhos permanece. A régua ainda possui saída “JÁ VERDE” por impressão de `.py/.html` antes dos selos de host; porém o pre-push já exige também hash da árvore. Portanto a descrição ampla do BACKLOG sobre essas duas portas envelheceu parcialmente. Fontes: `bin/suite.sh:88`, `bin/regua.sh:33`, `bin/regua.sh:84`, `bin/regua.sh:138`, `bin/pre-push.sh:65`, `app/docs/BACKLOG.md:1037`. Commit responsável: **[nao verificado]**.

3. **Falta / reaplicação.** Mapa, replay PRONTO 5 e rebase continuam registrados. O diagnóstico histórico relata falhas na geração e mapa expirado; não certifica o replay. Fontes: `app/docs/BACKLOG.md:1036`, `app/docs/RELATO.md:1635`, `app/docs/RELATO.md:1637`.

   O contrato exige testar alterações de testes e reproduzir a regressão com o mapa apropriado; o complemento distingue alterações de `.py` de produção que forçam inteira. Fontes: `app/docs/PROMPTS.md:1683`, `app/docs/PROMPTS.md:1686`.

   Mesmos `main_moveu=True` de OTIMIZA-1: `bin/gerar_estado.py`, `bin/suite.sh`, `bin/tests/test_suite_sh.sh` — **[nao verificado]** por Git. **INFERIU:** preservar a detecção de seleção efetivamente inteira e os motivos `pre-opus|pouso`. Fontes: `bin/suite.sh:140`, `bin/suite.sh:173`, `bin/tests/test_suite_sh.sh:42`. Aplicação limpa: **[nao verificado]**.

4. **Arquivos em comum.** Nenhum da raia original cruza as quatro obras em voo. O complemento DIFF-POR-IMPACTO não fornece lista de arquivos de implementação suficiente para outro cruzamento: **[nao verificado]** além dos anexos.

5. **Depende de.** PRONTO 5, certificação e pouso próprio L-105; o aval original coloca o instrumento depois do CALENDÁRIO. O BACKLOG registra CALENDARIO-HOJE-MINIMO fechado, sem que isso prove ancestralidade Git nesta cópia. Fontes: `app/docs/BACKLOG.md:1036`, `app/docs/LEIS.md:131`, `app/docs/PROMPTS.md:1682`, `app/docs/BACKLOG.md:819`.

   **Separação necessária:** o complemento de 13/11 entra após E3; GATE-PELA-CLASSE é descrito como lei nova que ainda exige corte. Essa pendência não equivale a falta de autorização do núcleo antigo. Fonte: `app/docs/PROMPTS.md:2243`.

6. **PROPOSTA — INFERIU: CONCLUIR**, primeiro no comboio com OTIMIZA-1, começando pela recuperação de PRONTO 5 e adaptação às guardas atuais. O complemento segue na posição pós-E3 registrada, com o corte de lei pendente. Fontes: `app/docs/BACKLOG.md:1036`, `app/docs/RELATO.md:1637`, `app/docs/PROMPTS.md:2243`.

## JUSTIFICATIVA-PAGA-O-PERIODO

1. **O que cura — LEU.** A justificativa aprovada ainda limpa todos os marcos faltantes do dia, sem ler seu tipo; o efeito financeiro recebe a data inteira como justificada e não calcula o período da justificativa. Fontes: `app/escala/utils.py:1194`, `app/escala/utils.py:1254`, `app/ponto/services/efeito_ausencia.py:40`, `app/ponto/services/fechamento.py:660`.

2. **Existe no HEAD? SIM — LEU, com correção da premissa do BACKLOG.** O código não corresponde à frase “já cobre só o marco”: a grade consulta apenas datas e limpa todos os `missing`. O acumulador parcial de `efeito_ausencia` soma Ausência, enquanto o motor distingue trabalho/abono pelos conjuntos de datas. Fontes: `app/escala/utils.py:1199`, `app/escala/utils.py:1256`, `app/ponto/services/efeito_ausencia.py:46`, `app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2606`.

   O caso HJ-093 e os totais de triagem são registros fornecidos; a própria linha admite outras causas possíveis. Não são contagem comprovada de ocorrências deste defeito. Fonte: `app/docs/BACKLOG.md:1872`; confirmação nominal atual: **[nao verificado]**.

3. **Falta / reaplicação.** Falta ligar cobertura do marco, período pago e consumidores financeiros, produzir DIFF novo e demonstrar as condições do aval. Fontes: `app/docs/BACKLOG.md:1872`, `app/docs/CORTES.md:336`, `app/docs/CORTES.md:353`.

   O anexo marca `main_moveu=True` em `app/core/juizes.py`, `app/escala/utils.py`, `app/escala/tests/test_grade_segue_a_precedencia.py`, `app/ponto/services/{cartorio,efeito_ausencia,fechamento}.py` e `app/ponto/turnos.py` — **[nao verificado]** por Git.

   **INFERIU:** o porte precisa adaptar três mudanças do HEAD:

   - A grade agora recebe deslocamentos da cadeia do turno, em vez de reconstruí-los pelas colunas antigas. Fonte: `app/escala/utils.py:1156`, `app/escala/utils.py:1178`.
   - O cartório já propaga `dispensada` por marco, mecanismo a preservar na integração. Fonte: `app/ponto/services/cartorio.py:592`.
   - O fechamento eliminou o acumulador mensal antigo e usa `creditos_por_dia`, cujo universo atual continua sendo Ausência. Somar apenas em `efeito_ausencia.min_parcial_pagos` não demonstra consumo pelo fechamento. Fontes: `app/ponto/services/fechamento.py:672`, `app/ponto/services/fechamento.py:994`, `app/ponto/services/credito_parcial.py:15`, `app/ponto/services/credito_parcial.py:26`.

   Aplicação limpa e conflitos textuais: **[nao verificado]**.

4. **Arquivos em comum.** `app/core/juizes.py` cruza **BUG-PAR/O168/E3**; nenhum outro arquivo da raia original cruza a lista fornecida. Confirmação fora dos anexos: **[nao verificado]**.

5. **Depende de.** O desenho da cobertura e do período está registrado; o `!` do motor **já foi concedido**, com escopo específico e condições, portanto não deve ser apresentado como autorização ainda inexistente. Fontes: `app/docs/CORTES.md:319`, `app/docs/CORTES.md:336`, `app/docs/CORTES.md:353`.

   Permanecem as condições: DIFF prévio das competências indicadas, nenhuma semana adicional de DSR perdida fora das exceções nomeadas e hash do TXT igual nas exportadas. CADASTRO/BATIDA seguem os destinos definidos no corte; atendimento dessas condições hoje: **[nao verificado]**. Fonte: `app/docs/CORTES.md:353`.

6. **PROPOSTA — INFERIU: CONCLUIR**, imediatamente após HJ-090 na fila de negócio, como já registrado, coordenando `core/juizes.py` com BUG-PAR/O168/E3. Concluir a obra completa evita pousar isoladamente o estreitamento da cobertura, expressamente condicionado a existir quem pague o período. Fontes: `app/docs/BACKLOG.md:1872`, `app/docs/CORTES.md:330`.

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| COBPARC-FILA-RELOGIO | Instante único por ato (`app/ponto/services/cartorio.py:373`) | **SIM** (`app/chamados/reconciliador.py:884`) | Porte aos eventos, mutantes/suíte/opus (`app/docs/BACKLOG.md:860`) | Padrão e estacionamento; sobreposições conforme anexos **[nao verificado]** | **CONCLUIR**, após estabilizar sobreposições — INFERIU |
| V3-RETOMA-PERIODOS-DA-CELULA | Período/pausa/CCT diária (`app/ponto/calculador/alimentacao.py:158`) | **PARCIAL**; P5/dobra presentes (`app/ponto/motor_calculo_v2.py:162`; `app/ponto/calculador/regras.py:793`) | Períodos, intra, split e P4 (`app/docs/BACKLOG.md:964`) | Impacto integral e `!` da troca/exportadas (`app/docs/BACKLOG.md:1797`) | **CONCLUIR**, restante da retomada V3 — INFERIU |
| V3-RETOMA-FILA | DIFF lê a lavra (`app/ponto/management/commands/diff_calculador.py:137`) | **SIM** (`app/ponto/management/commands/diff_calculador.py:247`) | Instrumento completo e re-DIFF (`app/docs/BACKLOG.md:976`) | L-105; conciliar POUSO-DA-V3 (`app/docs/BACKLOG.md:976`) | **CONCLUIR**, a

[truncado em 20000 de 20942 caracteres]

## 13/11 18:24 · sessao 026a019ad69b · RAIA-PREPARACAO · censo · origem=Code

**LEU** indica sítio aberto na cópia; **INFERIU** indica conclusão ou proposta baseada nesses sítios. Aplicação das refs, conflitos textuais de merge, testes executados agora e situação atual de produção: **[nao verificado]**. [nao verificado]

## SILENCIO-VENCIDO-VOLTA

1. **O que cura — LEU.** O contador aceita silêncio vencido, mas a partição o transforma em HISTORICO e a lista o esconde (`app/chamados/services/painel.py:190`; `app/chamados/services/verbos.py:51`; `app/chamados/services/painel.py:328`).  
   A lista do colaborador também exige prazo nulo, excluindo quem teve silêncio já vencido (`app/chamados/services/janela_colab.py:73`).

2. **Existe no HEAD? SIM — LEU/INFERIU.** A contradição permanece nesses caminhos. Parte da infraestrutura já existe: `qs_na_fila` recebe `agora`, e a instância tem `silenciado_em`; isso não corrige HISTORICO nem a seleção do colaborador (`app/chamados/services/painel.py:158`; `app/chamados/models.py:508`; `app/chamados/services/verbos.py:51`). Commit de introdução dessas partes: **[nao verificado]**.

3. **Falta/aplicabilidade.** A SPEC atual é o transplante b4 da E3, mais `verbos.py` e a lista do colaborador, com pouso junto da QUARENTENA (`app/docs/BACKLOG.md:782`). O registro posterior informa que b4/b6a foram retiradas da raia E3; portanto sua existência naquela raia não demonstra cura entregue (`app/docs/RELATO.md:243`). Os arquivos com `main_moveu=True` e os conflitos semânticos estão no quadro compartilhado abaixo; aplicação integral da raia: **[nao verificado]**.

4. **Arquivos comuns.** São os do quadro compartilhado SILENCIO/QUARENTENA abaixo. O anexo contém uma raia cumulativa; atribuir todos esses arquivos exclusivamente à fatia SILENCIO: **[nao verificado]**.

5. **Depende de.** QUARENTENA e integração da SPEC retirada da E3 (`app/docs/BACKLOG.md:782`). A regra de silêncio com prazo está na L-062 (`app/docs/LEIS.md:96`). Necessidade direta de alterar motor, gravado exportado ou dados de escala/vínculo nesta fatia: **[nao verificado]**.

6. **Proposta — INFERIU: CONCLUIR junto da QUARENTENA**, após o pouso E3, conforme a condição mais recente de reabertura. O defeito continua legível; “pronta” no cadastro não basta para encerrá-lo (`app/docs/BACKLOG.md:2469`; `app/chamados/services/verbos.py:51`; `app/chamados/services/janela_colab.py:73`).

## QUARENTENA-DO-COLAB

1. **O que cura — LEU.** O contrato troca o silêncio escrito em cada chamado por isolamento do colaborador por período, com juiz e porta únicos (`app/docs/BACKLOG.md:783`).  
   O sítio atual ainda aplica silêncio chamado a chamado, acionado também pelos sinais (`app/chamados/services/silencio_isentos.py:34`; `app/chamados/signals.py:270`).

2. **Existe no HEAD? SIM — LEU/INFERIU.** A fila considera status e prazo; a emissão datada segue para `PerguntaDisputa.abrir`; o comando de isentos ainda escreve silêncio (`app/chamados/services/painel.py:188`; `app/chamados/services/disputa_emissao.py:443`; `app/chamados/management/commands/silenciar_chamados_isentos.py:49`). Uma cura equivalente em outro caminho não foi demonstrada: **[nao verificado]**.

3. **Falta/aplicabilidade.** O HEAD manda reaplicar a versão antiga como SPEC numa cópia nova e produzir DIFF de frota (`app/docs/BACKLOG.md:783`; `app/docs/BACKLOG.md:2469`). O relato detalha oito achados: fechar a janela ao desligar isenção; vigia chegar a zero; eliminar segundo escritor; migration reversível sem importar serviço vivo; LEI-AKITA; RED da retenção; DIFF pela consulta real; início pela ligação correta (`app/docs/RELATO.md:251`). Esse mesmo registro não estabelece suite aprovada; a retomada posterior parou com apenas um RED não rodado (`app/docs/RELATO.md:251`; `app/docs/RELATO.md:190`). Aplicação limpa da raia antiga: **[nao verificado]**; divergências abaixo.

4. **Arquivos comuns.** Quadro compartilhado abaixo, calculado dos anexos; comparação com as refs atuais: **[nao verificado]**.

5. **Depende de.** Pouso E3 e decisão sobre início da isenção: o desenho registra última ligação versus uma janela por período da trilha, com proposta de 16 janelas em vez de 15 (`app/docs/BACKLOG.md:2469`; `app/docs/RELATO.md:191`). O contrato exige espelho, folha e TXT intactos; comprovação atual dessa invariância: **[nao verificado]** (`app/docs/BACKLOG.md:783`). Qualquer execução que alcance motor, gravado exportado ou dados de escala/vínculo entra no âmbito de `!` (`CLAUDE.md:265`).

6. **Proposta — INFERIU: CONCLUIR com SILENCIO**, depois da E3 e coordenada com SAIDA-NOTURNA. Usar a raia antiga como especificação e resolver a reprovação antes de certificar; essa é a retomada registrada, já interrompida novamente (`app/docs/BACKLOG.md:2469`; `app/docs/RELATO.md:190`; `app/docs/RELATO.md:245`).

**Raia compartilhada SILENCIO/QUARENTENA — ponto 3 das duas obras.**

O anexo marca `main_moveu=True` nestes 24 arquivos; conflitos textuais e aplicação efetiva: **[nao verificado]**.

- `app/chamados/management/commands/auditar_invariantes_chamados.py`;
  `app/chamados/{models,signals,views_arquivar}.py`. **[nao verificado]**
- `app/chamados/services/{aviso_pendencia,cobrar_dia,disputa_emissao,materializacao,painel,pergunta_no_app,silencio_ausencia,verbos,vigia_fio}.py`. **[nao verificado]**
- `app/chamados/tests/test_o261_vigia_fio.py`. **[nao verificado]**
- `app/colaboradores/services/{colaborador,ficha}.py`;
  `app/colaboradores/views.py`. **[nao verificado]**
- `app/core/{juizes,portas}.py`;
  `app/core/tests/{test_selo_performance,test_tripwire_contagem_de_pendencia}.py`. **[nao verificado]**
- `app/docs/{ARQUITETURA.mmd,MAPA.md}`;
  `app/ponto/tests/test_selo_chokepoint_escrita.py`. **[nao verificado]**

**Conflitos semânticos — INFERIU:** reaplicar corpos antigos pode desfazer a delegação do aviso a `consulta_do_app`, a trilha anterior ao cartório e a distinção entre conversa e fato datado. São pontos concretos para reconciliar, além do relógio `agora` já incorporado (`app/chamados/services/aviso_pendencia.py:7`; `app/chamados/services/cobrar_dia.py:31`; `app/chamados/services/disputa_emissao.py:424`; `app/chamados/services/painel.py:158`).

**Interseção com as obras em voo — ponto 4 das duas obras, segundo os anexos:**

| Em voo | Arquivos comuns à raia compartilhada |
|---|---|
| BUG-PAR e E3 | `app/chamados/models.py`; `app/chamados/services/{disputa_emissao,materializacao,vigia_fio}.py`; `app/chamados/tests/test_o261_vigia_fio.py`; `app/core/juizes.py`; `app/core/tests/test_tripwire_contagem_de_pendencia.py`. **[nao verificado]** |
| O168 | `app/chamados/models.py`; `app/core/juizes.py`; `app/ponto/tests/test_selo_chokepoint_escrita.py`. **[nao verificado]** |
| HJ-090 | `app/chamados/tests/test_o261_vigia_fio.py`. **[nao verificado]** |

## V3-E1

1. **O que cura — LEU.** Fecha a certificação da v3 por casos validados e propriedades fixas; o sítio de ensaio é `reler`, explicitamente destinado à bateria e ao DIFF (`app/docs/BACKLOG.md:821`; `app/ponto/calculador/periodo.py:543`).  
   O registro específico da interrupção informa VALOR verde nos casos validados e falhas de STATUS, esclarecendo o “motivo: não sei” do cadastro (`app/docs/RELATO.md:2055`; `app/docs/BACKLOG.md:2081`).

2. **Existe no HEAD? PARCIAL — LEU/INFERIU.** A bateria E1 continua descrita pelos testes atuais como pertencente a outra raia (`app/ponto/tests/test_e3_status_lavrado.py:7`). Parte dos defeitos históricos já tem resposta no HEAD: previsto sem batida com veredito acusador resulta em `irregular`; isento resulta em `ok`, com testes correspondentes (`app/ponto/status_dia.py:150`; `app/ponto/status_dia.py:154`; `app/ponto/tests/test_e3_status_lavrado.py:55`; `app/ponto/tests/test_e3_status_lavrado.py:62`). Cura dos demais casos históricos e commit responsável por cada um: **[nao verificado]**.

3. **Falta/aplicabilidade.** Reconstituir a bateria sobre o HEAD, reavaliar STATUS e preservar as pendências do DP. O relato da validação inclui C05/C06/C16/C17 pendentes; a decisão C17 — HE 0/intra 60 — já está registrada (`app/docs/RELATO.md:2055`; `app/docs/RELATO.md:2064`). Conferir também o gate CCT e o impacto por rubrica, lacunas registradas (`app/docs/RELATO.md:2065`; `app/docs/BACKLOG.md:821`). No anexo, nenhum dos cinco arquivos tem `main_moveu=True`; compatibilidade atual dos testes e aplicação limpa: **[nao verificado]**. A API `reler` permanece no HEAD (`app/ponto/calculador/periodo.py:543`).

4. **Arquivos comuns.** Interseção literal com BUG-PAR/O168/HJ-090/E3 vazia segundo os anexos; confirmação das raias: **[nao verificado]**.

5. **Depende de.** DP/contador para os casos pendentes e certificação pela regra (`app/docs/RELATO.md:2055`; `app/docs/BACKLOG.md:821`). Alterar v2 ou dados protegidos exige `!`; a troca para v3 também está explicitamente condicionada a `!` (`CLAUDE.md:265`; `app/docs/BACKLOG.md:963`).

6. **Proposta — INFERIU: CONCLUIR na pista de certificação, antes da troca v3.** A cura posterior de alguns STATUS reduz o trabalho residual, mas não substitui a bateria nem resolve as pendências do DP (`app/ponto/tests/test_e3_status_lavrado.py:7`; `app/docs/RELATO.md:2055`; `app/docs/BACKLOG.md:821`).

## CODEX-CONTEXTO-DA-CASA

1. **O que cura — LEU.** O contrato pede contexto gerado com hashes e brief de verificador restrito a caminhos, hash e pergunta (`app/docs/BACKLOG.md:1038`).  
   O sítio atual monta o cabeçalho e transcreve integralmente o arquivo de pergunta (`bin/codex_pergunta.sh:335`; `bin/codex_pergunta.sh:355`).

2. **Existe no HEAD? SIM — LEU/INFERIU.** A transcrição crua permanece, e a guarda do verificador nesse caminho consulta B3 (`bin/codex_pergunta.sh:355`; `bin/codex_pergunta.sh:390`). A publicação de GLOSSARIO já existe, mas não demonstra a implementação do contrato de contexto e neutralidade do brief (`bin/relato.sh:111`; `app/docs/BACKLOG.md:1038`). Commit de cura completa: **[nao verificado]**.

3. **Falta/aplicabilidade.** Reconciliar briefs com a exigência `PERGUNTA:` e resolver teto/fontes: o relato documenta aproximadamente 42,7 KB para teto de 30.000 B, além da classificação das leis das seções 6b/7b (`app/docs/BACKLOG.md:1038`; `app/docs/RELATO.md:1943`). O bloqueio histórico por F1/V-FASE não comprova que esses construtores continuem em voo (`app/docs/BACKLOG.md:1038`); estado atual deles: **[nao verificado]**. O anexo marca `main_moveu=True` em `bin/{codex_limite_estado.py,codex_pergunta.sh,relato.sh}`; conflitos textuais: **[nao verificado]**. **INFERIU:** preservar o papel construtor, inventário, relógio por papel e publicação atuais ao incorporar a cura (`bin/codex_pergunta.sh:85`; `bin/codex_pergunta.sh:195`; `bin/relato.sh:111`).

4. **Arquivos comuns.** Interseção literal com as quatro obras em voo vazia segundo os anexos; confirmação das raias: **[nao verificado]**.

5. **Depende de.** Decisão sobre teto/fontes e pouso próprio de instrumento (`app/docs/RELATO.md:1943`; `app/docs/LEIS.md:131`). SEM-CONDUTA é complemento sucessor: o registro manda construí-lo depois de ENXUTO e DA-CASA (`app/docs/RELATO.md:1993`). Necessidade direta de `!` de motor/exportada/escala: **[nao verificado]**.

6. **Proposta — INFERIU: CONCLUIR em vaga de instrumento após o produto**, reconciliando o contrato sucessor SEM-CONDUTA. A lacuna do brief permanece; o desenho de fontes/teto precisa de resolução antes da certificação (`bin/codex_pergunta.sh:355`; `app/docs/RELATO.md:1943`; `app/docs/LEIS.md:131`).

## PADRAO-DE-CIMA

1. **O que cura — LEU.** O contrato pede reconhecer a segunda pergunta de desenho da mesma entidade/programa em 24 horas e bloquear construtor/verificador até a decisão consumida uma vez (`app/docs/BACKLOG.md:836`).  
   O mecanismo atual acumula e decide reprovações por obra (`bin/disjuntor.py:997`; `bin/disjuntor.py:1004`).

2. **Existe no HEAD? SIM — LEU/INFERIU.** O PreToolUse aplica B3 por obra e o censo do construtor, sem realizar ali o agrupamento contratado por entidade/24 h (`bin/disjuntor.py:1826`; `bin/disjuntor.py:1842`; `bin/disjuntor.py:1850`). Também permanece o problema de prova discutido no desenho: `_mutante` aceita qualquer retorno não zero, sem identificar a causa da morte (`bin/tests/test_disjuntor.sh:1522`; `app/docs/RELATO.md:1845`). Commit de cura completa: **[nao verificado]**.

3. **Falta/aplicabilidade.** Decidir como provar a causa da morte do mutante e certificar a versão escolhida. O relato distingue a versão estacionada da volta 10, que ficou sem verificador e sem opus final (`app/docs/RELATO.md:1845`). O anexo marca `main_moveu=True` em `bin/{codex_pergunta.sh,disjuntor.py,hook_stop_fila1.py}` e `bin/tests/{test_disjuntor.sh,test_hook_esperando_com_prova.sh,test_lei_akita.sh}`; aplicação limpa: **[nao verificado]**. **INFERIU:** reconciliar com CENSO-ANTES-DA-VAGA, papel construtor e recorte atual do topo (`bin/disjuntor.py:1850`; `bin/codex_pergunta.sh:195`; `bin/hook_stop_fila1.py:467`).

4. **Arquivos comuns.** Interseção com BUG-PAR/O168/HJ-090/E3 vazia segundo os anexos. Entre estas interrompidas, compartilha `bin/codex_pergunta.sh` com CODEX-CONTEXTO-DA-CASA. Confirmação das refs: **[nao verificado]**.

5. **Depende de.** PDC-DESENHO, contrato de instrumento e pouso separado de produto (`app/docs/RELATO.md:1845`; `CLAUDE.md:394`; `app/docs/LEIS.md:131`). B3 já possui implementação no HEAD (`bin/disjuntor.py:1004`). Necessidade direta de `!` de motor/exportada/escala: **[nao verificado]**.

6. **Proposta — INFERIU: CONCLUIR após a decisão de desenho, mantendo posição antes de B1-ESPERA-OCIOSA.** O contrato ainda tem lacuna no código, e a posição relativa está registrada (`bin/disjuntor.py:1826`; `app/docs/BACKLOG.md:837`). A reprovação repetida exige resolver o desenho; não demonstra cura ou superação da necessidade (`app/docs/RELATO.md:1845`).

## BUG-ENC-CELULAS-FORMA

1. **O que cura — LEU.** `dias_encerrados` repassa células com chave `(colaborador_id,data)` ao pareador, cujo contrato recebe chave `data`; a conversão posterior produz chave incompatível para consultar a geradora (`app/ponto/turnos.py:2132`; `app/ponto/turnos.py:2150`; `app/ponto/turnos.py:1854`; `app/ponto/turnos.py:1956`).  
   O cartório fornece exatamente esse mapa composto, sem `turnos`, e apenas para seu intervalo de julgamento (`app/ponto/services/cartorio.py:1131`; `app/ponto/services/cartorio.py:1154`).

2. **Existe no HEAD? SIM — LEU/INFERIU.** O repasse defeituoso permanece em `app/ponto/turnos.py:2150`. O pareador só carrega sua janela com pad quando recebe `celulas=None`; o mapa fornecido impede essa carga (`app/ponto/turnos.py:1884`). A via singular do vigia também passa chave composta, que chega diretamente ao pareador (`app/ponto/services/vigia_de_hora.py:58`; `app/ponto/turnos.py:2098`). O lote já converte corretamente para chave `data`, mas isso não cura a casca (`app/ponto/turnos.py:3038`). Commit dessa implementação do lote: **[nao verificado]**.

3. **Falta/aplicabilidade.** Completar a via singular; selos de booleano e pad; validação; suite; verificação final; impacto. São pendências explícitas do registro da interrupção (`app/docs/RELATO.md:7799`). E4-CARGA-PARIDADE deve preceder a cura, com fixture mista e mutante sem `mais=` (`app/docs/BACKLOG.md:932`). No anexo, somente `app/ponto/turnos.py` tem `main_moveu=True`; aplicação integral: **[nao verificado]**. **INFERIU:** o hunk fornecido ainda tem alvo reconhecível em `:2150`, mas resolve apenas a casca; a singular continua exigindo tratamento próprio (`app/ponto/turnos.py:2150`; `app/ponto/turnos.py:2098`).

4. **Arquivos comuns.** Interseção literal dos três caminhos da raia com as quatro obras em voo vazia segundo os anexos; confirmação das refs: **[nao verificado]**.

5. **Depende de.** E4-CARGA-PARIDADE e prova do impacto da mudança de veredito (`app/docs/BACKLOG.md:931`; `app/docs/BACKLOG.md:932`). O lote E4-7 já está implementado no HEAD (`app/ponto/turnos.py:2959`). O patch descrito toca `turnos.py`; eventual alteração de v2, gravado exportado ou dados de escala/vínculo exige `!` (`CLAUDE.md:265`). Alcance atual sobre produção: **[nao verificado]**.

6. **Proposta — INFERIU: CONCLUIR na próxima vaga de bug liberada, com E4-CARGA-PARIDADE primeiro no mesmo pacote.** Há defeito estático atual em chamadores concretos, e o cadastro lhe atribui fila 1; a espera histórica por F2 era ordem de vaga, sem dependência de código provada naquele registro (`app/ponto/turnos.py:2150`; `app/ponto/services/vigia_de_hora.py:58`; `app/docs/BACKLOG.md:931`; `app/docs/RELATO.md:1824`).

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| SILENCIO-VENCIDO-VOLTA | Retorno coerente do vencido (`app/chamados/services/verbos.py:51`) | **SIM** (`app/chamados/services/janela_colab.py:73`) | SPEC + verbos/lista (`app/docs/BACKLOG.md:782`) | QUARENTENA/E3 (`app/docs/BACKLOG.md:2469`) | **CONCLUIR junto, após E3** — INFERIU (`app/docs/BACKLOG.md:2469`) |
| QUARENTENA-DO-COLAB | Isolamento por período (`app/docs/BACKLOG.md:783`) | **SIM** (`app/chamados/services/silencio_isentos.py:43`) | Oito achados, validação e DIFF (`app/docs/RELATO.md:251`) | E3; desenho 16×15 (`app/docs/BACKLOG.md:2469`; `app/docs/RELATO.md:191`) | **CONCLUIR com SILENCIO** — INFERIU (`app/docs/BACKLOG.md:2469`) |
| V3-E1 | Certificação v3 (`app/docs/BACKLOG.md:821`) | **PARCIAL** (`app/ponto/tests/test_e3_status_lavrado.py:7`; `app/ponto/status_dia.py:150`) | Revalidar bateria/DP/CCT (`app/docs/RELATO.md:2055`; `app/docs/RELATO.md:2065`) | DP; troca com `!` (`app/docs/BACKLOG.md:963`) | **CONCLUIR antes da troca** — INFERIU (`app/docs/BACKLOG.md:821`) |
| CODEX-CONTEXTO-DA-CASA | Contexto verificável e brief neutro (`app/docs/BACKLOG.md:1038`) | **SIM** (`bin/codex_pergunta.sh:355`) | Wrapper, fontes/teto (`app/docs/RELATO.md:1943`) | Decisão; SEM-CONDUTA sucessor (`app/docs/RELATO.md:1993`) | **CONCLUIR em vaga de instrumento** — INFERIU (`app/docs/LEIS.md:131`) |
| PADRAO-DE-CIMA | Desenho por entidade/24 h (`app/docs/BACKLOG.md:836`) | **SIM** (`bin/disjuntor.py:1004`) | Causa do mutante e certificação (`app/docs/RELATO.md:1845`) | PDC-DESENHO (`app/docs/BACKLOG.md:836`) | **CONCLUIR após decisão, antes de B1** — INFERIU (`app/docs/BACKLOG.md:837`) |
| BUG-ENC-CELULAS-FORMA | Contratos de células e pad (`app/ponto/turnos.py:2150`) | **SIM** (`app/ponto/services/vigia_de_hora.py:58`) | Singular, selos, suite/impacto (`app/docs/RELATO.md:7799`) | E4-CARGA-PARIDADE (`app/docs/BACKLOG.md:932`) | **CONCLUIR na vaga de bug** — INFERIU (`app/docs/BACKLOG.md:931`) |

## 13/11 18:24 · sessao 27b58837747d · RAIA-PREPARACAO · censo · origem=Code

**LEU** identifica código/documento aberto na cópia; **INFERIU** identifica conclusão da comparação. SIM/PARCIAL abaixo qualificam o código: ocorrência atual em produção, passivo e impacto financeiro são **[não verificado]**. Histórico Git, ancestralidade e aplicação mecânica das raias também são **[não verificado]**; os commits citados têm atribuição documental. [nao verificado]

**O261-PAI-TERMINAL**

1. **O que cura — LEU.** Disputa aberta pode conservar pai terminal: o emissor retorna a disputa existente sem verificar o estado do pai. (`app/chamados/services/disputa_emissao.py:531`, `app/chamados/services/disputa_emissao.py:566`)  
   Pergunta sem destino continua ancorada nesse pai; a fila cobra apenas chamados vivos. (`app/chamados/services/fio.py:117`, `app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:189`)

2. **Ainda existe? PARCIAL — LEU.** O retorno sem conferir o pai permanece; `reabrir_sistema` reabre a disputa sem religar sua âncora, e o nascimento admite `chamado_destino=None`. (`app/chamados/services/disputa_emissao.py:566`, `app/chamados/models.py:2164`, `app/chamados/models.py:3015`) Entretanto, a O261-E já introduziu guarda no encerramento: considera perguntas da competência aberta, reata por pergunta ou recusa o fechamento. Esse sítio **proíbe expressamente `religar_pai_vivo` por não separar competências**. (`app/chamados/models.py:364`, `app/chamados/models.py:374`, `app/chamados/models.py:410`) A absorção do objetivo pela O261-E consta do BACKLOG; isso não comprova cura completa do nascimento/passivo. (`app/docs/BACKLOG.md:892`)

3. **Falta / aplicação — LEU + INFERIU.** Falta resolver a âncora por pergunta/dia, preservando a separação entre aberta e exportada. A condição das disputas mistas está escrita no corte; o desenho atual já escolheu a reata por pergunta. (`app/docs/CORTES.md:397`, `app/chamados/models.py:374`)  
   `main_moveu=True`, segundo o anexo: `app/chamados/models.py`, `app/chamados/services/acoes_disputa.py`, `app/chamados/services/vigia_fio.py`, `app/chamados/management/commands/vigiar_fio_morto.py` e `app/chamados/tests/test_o261_vigia_fio.py`. **[não verificado: comparação histórica fornecida]**  
   **INFERIU:** há conflitos de desenho e API: a raia religa a disputa inteira, usa ordem Disputa→Colaborador e chama a antiga porta de pergunta na reabertura; o HEAD exige ordem Colaborador→Disputa→Chamado→Pergunta e usa `abrir_conversa` nesse ato. (`app/chamados/models.py:380`, `app/chamados/models.py:2834`, `app/chamados/services/acoes_disputa.py:263`; raia: anexo, **[não verificado na cópia]**)

4. **Arquivos em comum.** BUG-PAR/E3: os quatro arquivos de produção acima e `test_o261_vigia_fio.py`; O168: `models.py`; HJ-090: o comando `vigiar_fio_morto.py` e `test_o261_vigia_fio.py`. **[não verificado na cópia; cruzamento dos anexos]**

5. **Depende de — LEU.** Separação por competência, juízes da vida/âncora e conclusão pelo desenho O261-E/FIO; exportada deve seguir a condição específica do corte. (`app/docs/CORTES.md:397`, `app/chamados/models.py:324`, `app/chamados/models.py:377`) Tocar o gravado exportado exige `!`; religar indiscriminadamente a disputa não atende à condição escrita. (`CLAUDE.md:265`, `app/chamados/models.py:380`)

6. **Proposta — INFERIU: DEMOLIR a solução antiga por disputa.** O desenho foi superado pela reata por pergunta e está explicitamente vedado no HEAD. Encaminhar o residual de nascimento/reabertura à O261-E/FIO, junto das obras em voo que tocam esses arquivos, aproveitando os casos de teste como insumo. (`app/chamados/models.py:380`, `app/docs/BACKLOG.md:892`, `app/docs/PROMPTS.md:1685`) A proposta não declara o defeito encerrado. (`app/chamados/services/disputa_emissao.py:566`)

**O10-C**

1. **O que cura — LEU.** Entregar ao admin os lançamentos SOLO e permitir declarar o par pela tela, usando a porta existente. Esse é o desenho da volta 2 registrado. (`app/docs/RELATO.md:1338`, `app/docs/BACKLOG.md:2082`)  
   A operação de troca já existe, mas exige dois lados e duas cartas; SOLO é pedido, não troca confirmada. (`app/ponto/portas/celula.py:670`, `app/ponto/portas/celula.py:691`, `app/ponto/portas/celula.py:702`)

2. **Ainda existe? SIM, no objetivo lista+UI — LEU + INFERIU.** A base O10-B está presente: trava as cartas e edita os dois lados no mesmo atomic. (`app/ponto/portas/celula.py:695`, `app/ponto/portas/celula.py:711`) Não foi encontrado chamador de UI na busca da cópia; a ausência global fica **[não verificado]**, apoiada pela definição, rotas abertas e registro explícito da lacuna. (`app/ponto/portas/celula.py:670`, `app/ponto/urls.py:53`, `app/docs/RELATO.md:1338`) O BACKLOG atribui a base ao merge `c3ba27d7`; não atribui a ele a entrega C. (`app/docs/BACKLOG.md:2033`)

3. **Falta / aplicação — LEU + INFERIU.** Falta Pauta DP por dia com link para view/form que receba substituto, duas cartas e motivo; o caso de aceite é SOLO produzir zero edição do par e uma linha com porta alcançável. (`app/docs/RELATO.md:1338`, `app/docs/RELATO.md:1341`)  
   `main_moveu=True`: somente `app/config/crons.py`, segundo o anexo. **[não verificado: comparação histórica fornecida]**  
   **INFERIU:** o lote antigo pode fornecer classificação e casos, mas imprimir `so_um_lado` não entrega a lista persistida nem o formulário. A raia precisa incorporar a volta 2; a região de comandos avulsos também evoluiu. (`app/docs/RELATO.md:1338`, `app/config/crons.py:1370`; lote: anexo, **[não verificado na cópia]**)

4. **Arquivos em comum.** `app/config/crons.py` com BUG-PAR e HJ-090; nenhum dos arquivos fornecidos da raia com O168/E3. **[não verificado na cópia; cruzamento dos anexos]**

5. **Depende de — LEU.** O desenho SOLO já está decidido: o admin declara o par. A porta barra competência lavrada e dia sem vínculo. (`app/docs/RELATO.md:1338`, `app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:622`) O apply em exportada tem condição própria de DRY e `!`; alteração de dado de escala/vínculo também exige `!`. (`app/docs/RELATO.md:1342`, `CLAUDE.md:265`) **INFERIU:** a UI pode chamar a porta existente sem alterar o motor. (`app/docs/RELATO.md:1338`, `app/ponto/portas/celula.py:670`)

6. **Proposta — INFERIU: CONCLUIR pela volta 2.** Sugerida para a primeira vaga de negócio depois das prioridades estruturais, entregando lista e ação alcançável. O lote sozinho não satisfaz o desenho registrado. (`CLAUDE.md:291`, `app/docs/RELATO.md:1338`, `app/docs/BACKLOG.md:2082`)

**O275**

1. **O que cura — LEU.** Permitir que cadastro do tipo faça um feriado criar dia previsto onde o padrão semanal não previa trabalho. (`app/docs/BACKLOG.md:1557`)  
   Hoje o semanal consulta os dias cadastrados, e o vínculo apenas pode suprimir trabalho por feriado; falta o caminho feriado `False→True`. (`app/escala/models.py:830`, `app/escala/models.py:1365`)

2. **Ainda existe? SIM — LEU.** `TipoEscala` não apresenta o novo campo entre `apelido` e os campos de ciclo; o cálculo mantém a base do template e a supressão de feriado. (`app/escala/models.py:114`, `app/escala/models.py:117`, `app/escala/models.py:1365`) Payload e formulário abertos também não oferecem esse cadastro. (`app/escala/views_wizard.py:34`, `app/templates/escala/wizard_tipo_escala.html:173`) Cura equivalente ou commit que a introduziu: **[não verificado]**.

3. **Falta / aplicação — LEU + INFERIU.** Falta decidir **FORMA A** — todos os ciclos menos intermitente — ou **FORMA B** — somente 6x1/5x2/semanal. O bloqueio é escolha de lei com códigos diferentes; “default desligado” não escolhe entre elas. (`app/docs/BACKLOG.md:1557`, `app/docs/RELATO-ARQUIVO.md:750`, `app/docs/RELATO-ARQUIVO.md:753`) Os 12 h × 36 h são relato histórico; reprodução atual **[não verificado]**. (`app/docs/RELATO-ARQUIVO.md:748`)  
   `main_moveu=True`: `app/config/crons.py`, `app/escala/models.py`, `app/escala/services/cadastro_tipo.py`, `app/escala/views_wizard.py` e `app/templates/escala/tipos_lista.html`. **[não verificado: comparação histórica fornecida]**  
   **INFERIU:** precisa conciliar o leitor/contador antigos com as autoridades atuais de lugar e cobertura do vínculo. (`app/escala/models.py:929`, `app/escala/models.py:1385`; implementação antiga: anexo, **[não verificado na cópia]**) Há também conflito de grafo: a raia acrescenta migração após `0042`, enquanto o HEAD já tem `0043_eventocelula` e uma `0044` dependente dela; será necessário reencadear a migração. (`app/escala/migrations/0043_eventocelula.py:8`, `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:9`; migração da raia: **[não verificado na cópia]**)

4. **Arquivos em comum.** `app/config/crons.py` com BUG-PAR e HJ-090; nenhum dos arquivos fornecidos com O168/E3. **[não verificado na cópia; cruzamento dos anexos]**

5. **Depende de — LEU.** Corte FORMA A/B, cadastro pela UI, derivador único e migration conciliada. O277 vem depois da O275. (`app/docs/BACKLOG.md:1557`, `app/docs/BACKLOG.md:1559`) Alterar dados de escala/vínculo exige `!`; eventual toque no motor tem aval próprio. (`CLAUDE.md:265`, `CLAUDE.md:142`)

6. **Proposta — INFERIU: CONCLUIR após o corte A/B**, em vaga de negócio e antes da O277. A capacidade pedida permanece ausente; a decisão de alcance impede tratar a raia como pronta para pouso. (`app/escala/models.py:1365`, `app/docs/RELATO-ARQUIVO.md:753`, `app/docs/BACKLOG.md:1559`)

**LASTRO-DEGRAU-2**

1. **O que cura — LEU.** Elimina o casamento próprio de batida com marco quando falta lâmpada, fazendo o recuo ler o juiz da atribuição. (`app/docs/BACKLOG.md:1756`, `app/core/juizes.py:212`)  
   O sítio ainda combina a hora cobrada com a data civil e busca batida do mesmo tipo pela distância linear. (`app/ponto/services/lastro.py:304`)

2. **Ainda existe? SIM — LEU.** `TOLERANCIA_MIN=90` permanece e o degrau 2 ainda usa `abs(timestamp-alvo) <= tol`. (`app/ponto/services/lastro.py:59`, `app/ponto/services/lastro.py:307`) Outra parte evoluiu: LASTRO-PARTE-2 levou o fecho ao evento e tornou o comando vigia; o BACKLOG atribui isso a `a7372499`, e o código confirma o comportamento. Isso não removeu a conta linear. (`app/docs/BACKLOG.md:753`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:44`, `app/ponto/services/lastro.py:364`)

3. **Falta / aplicação — LEU + INFERIU.** Falta migrar o recuo para o juiz e verificar/corrigir a invariância da grade entre janela unitária e lote. A proposta antiga de consultar somente o raio já foi explicitamente superada. (`app/docs/BACKLOG.md:1756`, `app/docs/BACKLOG.md:1066`) Parte da causa mudou: folga/ausência agora são aplicadas por pessoa e vínculo do dia; a régua ainda reúne escalas da janela. O caso histórico continua ocorrendo hoje: **[não verificado]**. (`app/escala/utils.py:1843`, `app/escala/utils.py:1313`, `app/escala/utils.py:1350`)  
   `main_moveu=True`: `app/ponto/management/commands/fechar_cobranca_com_lastro.py`, `app/ponto/services/cartorio.py`, `app/ponto/services/lastro.py`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py`. **[não verificado: comparação histórica fornecida]**  
   **INFERIU:** exige recomposição: a raia usa o antigo `fechar()`, enquanto o HEAD preserva fecho por evento e o balde `retido_pelo_fechamento`; seu compositor também omite `agora`, hoje obrigatório no builder. (`app/ponto/services/lastro.py:317`, `app/ponto/services/lastro.py:364`, `app/escala/utils.py:2084`; chamadas antigas: anexo, **[não verificado na cópia]**)

4. **Arquivos em comum.** Nenhum dos seis arquivos fornecidos da raia aparece nas quatro listas de obras em voo. **[não verificado na cópia; cruzamento dos anexos]**

5. **Depende de — LEU.** Juiz único do marco, equivalência da grade e fechamento do grupo (1) de leitores. (`app/core/juizes.py:212`, `app/docs/BACKLOG.md:811`, `app/docs/BACKLOG.md:1756`) Eventual alteração de motor, dados escala/vínculo ou gravado exportado exige o `!` correspondente. (`CLAUDE.md:265`)

6. **Proposta — INFERIU: CONCLUIR o residual**, após verificar/corrigir o construtor e antes de declarar zerado o grupo (1). Preservar fecho por evento, guardas e contagem de retidos já presentes. (`app/docs/BACKLOG.md:811`, `app/ponto/services/lastro.py:317`, `app/ponto/services/lastro.py:364`)

**ATA-X-TURNO-PAUSA**

1. **O que cura — LEU + INFERIU.** A pausa reconhecida pelo juiz pode perder atribuição no cluster; a reconciliação posterior trata somente as bordas do turno. (`app/escala/utils.py:561`, `app/escala/utils.py:572`, `app/escala/utils.py:1975`)  
   Também falta publicar a acusação e os detalhes `l084_pausa` na CxR, como pede a obra. (`app/docs/BACKLOG.md:1725`, `app/ponto/services/cxr_na_lista.py:29`)

2. **Ainda existe? PARCIAL — LEU.** O juiz já alinha por papel, mas o fluxo posterior termina a reconciliação das bordas sem o bloco da pausa; acusador e campos específicos também faltam. (`app/escala/utils.py:502`, `app/escala/utils.py:2039`, `app/ponto/motor_calculo_v2.py:1673`, `app/ponto/motor_calculo_v2.py:2717`, `app/ponto/services/cxr_na_lista.py:30`)  
   A subobra JUIZ-DO-PAR-DUAS-ENTRADAS foi absorvida pela cura na fonte, documentalmente `b3bf529e`; o sítio atual confirma a atribuição dentro do juiz. Isso não absorveu a ATA-PAUSA inteira. (`app/docs/RELATO.md:212`, `app/docs/RELATO.md:2736`, `app/escala/utils.py:590`, `app/docs/RELATO.md:210`) Caso col841 e dinheiro atual: **[não verificado]**.

3. **Falta / aplicação — LEU + INFERIU.** Faltam 10/10 explicados e dinheiro igual na frota; são condições, não resultados comprovados aqui. O despacho manda refazer o bloco consultando a autoridade e atualizar a fixture. (`app/docs/CORTES.md:304`, `app/docs/RELATO.md:1082`)  
   `main_moveu=True`: `app/core/tests/test_medida_turno_do_par.py`, `app/escala/services/cadastro_realidade.py`, `app/escala/utils.py`, `app/ponto/turnos.py`. **[não verificado: comparação histórica fornecida]**  
   **INFERIU:** recompor conservação de órfãs, reconciliação e CxR sobre os sítios atuais; reavaliar o hunk antigo que passa papel pelo motor, considerando a cura na fonte. A catraca de linhas também evoluiu. (`app/escala/utils.py:1816`, `app/escala/utils.py:1975`, `app/escala/services/cadastro_realidade.py:360`, `app/core/tests/test_medida_turno_do_par.py:253`, `app/docs/RELATO.md:212`) `main_moveu=False` no motor, informado pelo anexo, não comprova compatibilidade do desenho. **[não verificado: comparação histórica fornecida]**

4. **Arquivos em comum.** `app/escala/services/cadastro_realidade.py` com BUG-PAR/E3; `app/core/tests/test_medida_turno_do_par.py` com BUG-PAR/HJ-090; nenhum com O168. **[não verificado na cópia; cruzamento dos anexos]**

5. **Depende de — LEU.** O `! ATA-PAUSA-NO-MOTOR` tem escopo condicionado a 10/10 e igualdade financeira. A pergunta de desenho consta respondida, com instrução de reconstrução. (`app/docs/CORTES.md:304`, `app/docs/RELATO.md:1082`, `app/docs/RELATO.md:1090`) Ocupação e pausas devem consultar as autoridades registradas. (`app/core/juizes.py:212`, `app/core/juizes.py:214`)

6. **Proposta — INFERIU: CONCLUIR**, em fila 1, depois das precedências C7/COBPARC registradas e da liberação dos arquivos comuns com E3/BUG-PAR. A conclusão destrava CXR-PRONTO1-COL841. (`app/docs/RELATO.md:1003`, `app/docs/BACKLOG.md:798`; cruzamento em voo: **[não verificado na cópia]**)

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| O261-PAI-TERMINAL | Âncora viva para pergunta (`app/chamados/services/fio.py:117`) | **PARCIAL** (`app/chamados/services/disputa_emissao.py:566`) | Residual por pergunta/dia (`app/chamados/models.py:374`) | O261-E + separação de competência (`app/docs/CORTES.md:397`) | **DEMOLIR raia antiga**, desenho vedado; residual na O261-E (`app/chamados/models.py:380`) |
| O10-C | SOLO na lista + troca pela UI (`app/docs/RELATO.md:1338`) | **SIM**; porta pronta (`app/ponto/portas/celula.py:670`) | Volta 2 UI/lista (`app/docs/BACKLOG.md:2082`) | Par declarado; vínculo/exportada (`app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:691`) | **CONCLUIR**, vaga de negócio após estruturais — INFERIU (`CLAUDE.md:291`) |
| O275 | Feriado cria previsto por cadastro (`app/docs/BACKLOG.md:1557`) | **SIM** (`app/escala/models.py:1365`) | A/B e migration conciliada (`app/docs/BACKLOG.md:1557`, `app/escala/migrations/0043_eventocelula.py:8`) | Corte A/B (`app/docs/RELATO-ARQUIVO.md:750`) | **CONCLUIR** após corte, antes O277 — INFERIU (`app/docs/BACKLOG.md:1559`) |
| LASTRO-DEGRAU-2 | Recuo lê juiz do marco (`app/core/juizes.py:212`) | **SIM** (`app/ponto/services/lastro.py:304`) | Janela, evento e relógio (`app/docs/BACKLOG.md:1756`, `app/escala/utils.py:2084`) | Construtor + grupo (1) (`app/docs/BACKLOG.md:811`) | **CONCLUIR** após construtor — INFERIU (`app/docs/BACKLOG.md:1756`) |
| ATA-X-TURNO-PAUSA | Pausa na ata e CxR (`app/docs/BACKLOG.md:1725`) | **PARCIAL** (`app/escala/utils.py:2039`, `app/ponto/services/cxr_na_lista.py:30`) | Reconstrução, 10/10, dinheiro igual (`app/docs/RELATO.md:1082`, `app/docs/CORTES.md:304`) | `!` motor condicionado; arquivos em voo **[não verificado]** | **CONCLUIR**, após precedências registradas — INFERIU (`app/docs/RELATO.md:1003`) |

_4781 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
