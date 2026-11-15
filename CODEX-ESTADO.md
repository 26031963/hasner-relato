# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 02:44** · publicado em **15/11/2026 02:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 02:44 · sessao 2c5a4a7cf2df · - · - · origem=Ronald (tmux)

Achado de gravidade média: selo antigo contradiz a cura c7. `app/escala/tests/test_f3_beco1_folga_na_lista.py:67-74` cria âncora `resolvido` e exige `('admin', 'porta_acao', 'validar')`, enquanto o filtro novo elimina esse dono (`app/chamados/services/tarefa_do_dia.py:655-657`) e M2b exige ausência de porta (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:544-550`).

Executei contraponto sem banco, sem gravar arquivos: corpo real de `_lavra` (`app/escala/tests/test_f3_beco1_folga_na_lista.py:105-111`), leitor real da ata, instâncias/tarefa/publicador reais, teto 08h/17h vencido, pergunta `orfao_14h` com resposta E1, fila vazia, oferta False. c7 produziu `('nada', None, None)` e o corpo antigo falhou na linha 72; removendo apenas o filtro c7 em memória, produziu `('admin', 'porta_acao', 'validar')` e passou. Execução ORM original [nao verificado]. A fixture controla oferta False (`:58-65`) e cria resposta E1 (`:26-36`, `:95-102`).

O mesmo contrato antigo aparece em `app/chamados/tests/test_f3_beco1_reata_limbo.py:28-30`, `:55-67`, `:87`, e em `app/chamados/tests/test_e4_passivo.py:65-69`, `:122-127`; consequências completas desses testes ORM [nao verificado].

Item 3: a fonte lógica é a mesma — campo `status_local` e constante `TERMINAIS` (`app/chamados/services/tarefa_do_dia.py:447-449`; `app/chamados/services/fio.py:794`, `:809-810`). O estado observado pode diferir: carga faz SELECT e congela o conjunto (`tarefa_do_dia.py:404-408`, `:449`), enquanto costura relê banco sob trava (`fio.py:801-808`). Interleaving concreto possível: SELECT vê vivo; fechamento ocorre; fila posterior já exclui o PK (`tarefa_do_dia.py:450-451`); `_pela_decisao` fornece `porta=None` (`:150-160`), mas conjunto antigo permite admin (`:655-657`). Considero limite admissível de leitura sem atomicidade, pois contrato declara estado atual, sem reconstrução histórica (`:383-388`), e a carga não escreve esse estado (`:448-449`). Ocorrência em produção [nao verificado].

Item 4: nenhum KeyError demonstrado sob FKs válidas. `chamado_que_lista` só retorna destino ou chamado da disputa (`app/chamados/services/fio.py:116-117`); ambos entram no dicionário e `None` sai das âncoras (`app/chamados/services/tarefa_do_dia.py:444`, `:448-449`). Pergunta exige disputa (`app/chamados/models.py:2494-2498`); os dois chamados são nullable com SET_NULL (`:1796-1802`, `:3494-3500`). Integridade real do banco [nao verificado].

Item 6: os nove mutantes morreram na execução atual, cada qual com controle verde e falha de asserção; o runner rejeita erros como morte (`app/chamados/tests/e3_r3_c7_mutantes.py:46-63`). Nenhuma lista terminal literal nova em produção: a cura importa a constante única (`app/chamados/services/tarefa_do_dia.py:447-449`; definição `app/chamados/catalogo/motor.py:30-31`). Nenhuma trava ou leitura de relógio acrescentada nesses blocos (`tarefa_do_dia.py:404-449`, `:586-589`, `:652-657`).

## 15/11 02:44 · sessao 3d95f3e4c5cb · - · - · origem=Ronald (tmux)

Não achei falha estática nova nos chamadores ou mudança de significado incompatível nos leitores do mapa publicado; os dois caminhos produtivos passam a mesma alimentação enriquecida com terminais (`app/chamados/services/tarefa_do_dia.py:589`, `app/chamados/services/tarefa_do_dia.py:591`, `app/chamados/services/tarefa_do_dia.py:616`).

- `alimentar_fila`: chamado pela carga com células da janela, `agora` e `colab_ids`; chamado por `alimentar_portas` com células e `agora`, sem `colab_ids` ou terminais. Esse segundo caminho segue para C17, sem passar por `_portas_da_tarefa` (`app/chamados/services/tarefa_do_dia.py:586`, `app/chamados/juizes.py:1616`, `app/chamados/juizes.py:1627`).
- `_portas_da_tarefa`: os únicos chamadores produtivos são a primeira publicação e a segunda passagem após consultar Resolver; ambos recebem `self._portas`, âncoras por pergunta e pendentes do app, e a segunda acrescenta a oferta (`app/chamados/services/tarefa_do_dia.py:591`, `app/chamados/services/tarefa_do_dia.py:616`).
- Chamadas de teste podem omitir a chave; `.get(..., ())` então considera qualquer âncora não terminal. M2 usa essa entrada sintética; M2b fornece explicitamente a âncora terminal (`app/chamados/services/tarefa_do_dia.py:656`, `app/chamados/tests/e3_portas_apoio.py:34`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:541`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:547`).
- O ramo de porta inteira antecede o filtro de terminal. A porta inteira nasce da pertinência à foto da fila; portanto, avaliar esse desvio exige avaliar as fotos tomadas em consultas distintas, e não constitui sozinho contraexemplo estático (`app/chamados/services/tarefa_do_dia.py:151`, `app/chamados/services/tarefa_do_dia.py:654`).

Censo da saída:

| Valor/caminho | Escritor → leitores |
|---|---|
| `admin_alcancavel`, `com_dono` | Locais em `app/chamados/services/tarefa_do_dia.py:652` e `app/chamados/services/tarefa_do_dia.py:658`; alimentam donos, residuais e portas vivas em `app/chamados/services/tarefa_do_dia.py:660`, `app/chamados/services/tarefa_do_dia.py:663`, `app/chamados/services/tarefa_do_dia.py:670`. |
| `fio/rota/acao` | Publicados em `app/chamados/services/tarefa_do_dia.py:696`; projetados sem rejulgar em `app/chamados/services/tarefa_do_dia.py:708`. |
| Folha | Lê envelope e fio em `app/folha/fora_da_folha.py:84`; agrega grupos, ações e contadores em `app/folha/fora_da_folha.py:203`; ferramenta monta links em `app/folha/fora_da_folha.py:267`. |
| Retrato | Grava fio como `dono` e copia ações em `app/folha/retrato_folha.py:1228`; persistência em `app/folha/management/commands/lavrar_retrato_folha.py:78`; lê fio e traduz balde em `app/folha/retrato_folha.py:448`, `app/folha/retrato_folha.py:485`; ações viram links em `app/folha/retrato_folha.py:1036`. |
| Views/templates | Foto→frase/links em `app/colaboradores/views.py:982`; quadros em `app/folha/views_retrato.py:318`; template mostra valores e links prontos em `app/templates/folha/_retrato_o_que_fazer.html:33` e `app/templates/folha/_retrato_o_que_fazer.html:72`; LED pronto em `app/templates/folha/_retrato_lista.html:47`. |
| Contadores Folha | Partição em `app/folha/retrato_folha.py:695`; pílulas em `app/folha/retrato_folha.py:835`; sem ação e sem leitura em `app/folha/vigia_retido_calado.py:53` e `app/folha/vigia_retido_calado.py:57`. |
| Pauta | Admite apenas fio `nada`; desconhecido é contado à parte em `app/ponto/services/pautas_dos_discordantes.py:87`, `app/ponto/services/pautas_dos_discordantes.py:90`, `app/ponto/services/pautas_dos_discordantes.py:95`. |
| Cadastro/C11 | Exige envelope `None`, depois relê status das âncoras e aplica `TERMINAIS` em `app/escala/services/cadastro_realidade.py:472`, `app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:499`. |
| Vigia/Furo | Alcance por rota e admin por fio em `app/chamados/services/vigia_fio.py:596` e `app/chamados/services/vigia_fio.py:601`; sem canal por ausência de rota em `app/chamados/services/furo_sem_canal.py:98` e `app/chamados/services/furo_sem_canal.py:104`. |

Para validar sob âncora terminal, sem outra ação ou oferta, a mudança publicada é `admin/porta_acao/validar` → `nada/None/None`; isso conduz Folha para grupo D/sem ação, pode admitir pauta discordante e permite continuar a avaliação C11. São efeitos coerentes com os predicados existentes desses leitores (`app/chamados/services/tarefa_do_dia.py:652`, `app/chamados/services/tarefa_do_dia.py:667`, `app/folha/fora_da_folha.py:109`, `app/folha/retrato_folha.py:193`, `app/ponto/services/pautas_dos_discordantes.py:95`, `app/escala/services/cadastro_realidade.py:472`).

## 15/11 02:43 · sessao 71f85e0dcf2f · - · - · origem=Ronald (tmux)

Itens 5/7 e mutantes:

- **Nenhum positivo enfraquecido encontrado.** Permanecem `nada`/D, golden `sem_acao_disponivel=1`, transição `nada → chamado → admin`, `[D1]` e `[(D1,'BATIDA')]` com motor não chamado: `app/folha/tests/test_haiku_fora_da_folha.py:92`, `:537`, `:584`, `:595`, `:617`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:79`, `:258`, `:260`.
- **Irmãos desconhecidos têm asserções concretas.** Folha exige `nao_sei`/E e golden exige `sem_leitura_sistema=1`, conferindo ferramenta/tela; pauta exige `nao_sei=True`, rota `None`, exclusão e contagem em `sem_leitura`: `app/folha/tests/test_haiku_fora_da_folha.py:110`, `:545`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:87`.
- **Ata montada manualmente; leitor real exercitado.** Helpers devolvem dict, sem chamar produtor/lavra: `app/folha/tests/test_haiku_fora_da_folha.py:32`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:27`. O teste extrai `_celulas_da_ata`, instâncias, tarefa e porta reais: `app/chamados/tests/test_e3_r3_c7_sem_banco.py:91`. O produtor real pode produzir esses valores de lâmpada, embora acrescente outros campos: `app/ponto/services/cartorio.py:711`, `:722`, `:738`. Não há prova de estrutura semanticamente impossível; a combinação completa `furo` + lâmpadas acesas sem batidas reais não foi produzida pela lavra nesses testes [nao verificado].
- **Mutantes mordem as alterações escolhidas.** Reexecutei os oito testes novos e nove mutantes com `PYTHONDONTWRITEBYTECODE=1`: oito verdes e 9/9 mortos por asserção, sem erros [nao verificado como arquivo de minha execução]. Evidência preservada equivalente: `app/chamados/tests/e3_r3_c7_green_carga.txt:11`; `app/chamados/tests/e3_r3_c7_mutantes.txt:206`. O mutante sem join verifica presença do argumento, sem medir SQL/N+1 real: `app/chamados/tests/test_e3_r3_c7_sem_banco.py:183`, `:215`.
- **Lacuna declarada de F2:** identificação exata dos dois fios originais continua pendente; entrega não a prova e reconhece isso: `app/chamados/tests/e3_r3_c7_entrega.txt:56`.
- **Risco posterior ao estado inicial da golden, ainda não confirmado:** criar o chamado em `app/folha/tests/test_haiku_fora_da_folha.py:590` aciona relavra pelo signal genérico em `app/chamados/signals.py:190`, `:210`, `:218`. A fixture não tem batidas; a ata manual pode ser substituída antes da asserção `:595` [nao verificado]. O teste sem banco chamado “preserva … transição” só confere o estado inicial: `app/chamados/tests/test_e3_r3_c7_sem_banco.py:111`, `:119`.
- **Comentários/docstrings:** nenhum erro concreto adicional encontrado; o alcance limitado sem ORM/paridade completa é explicitado em `app/chamados/tests/test_e3_r3_c7_sem_banco.py:1`, `:83`.

## 15/11 02:37 · sessao a9d477f71569 · E3-RESTO-R3 · construtor · origem=Code

Entrega em [e3_r3_c7_entrega.txt](chamados/tests/e3_r3_c7_entrega.txt:1), com curas, REDs, censo e comandos.

[145 testes sem banco passaram](chamados/tests/e3_r3_c7_green_final.txt:174), [9/9 mutantes morreram](chamados/tests/e3_r3_c7_mutantes.txt:206) e [ruff passou](chamados/tests/e3_r3_c7_ruff.txt:1).

Pendente: fio final das duas execuções originais de F2 `[nao verificado]`; certificação pela suite do Code também pendente ([limites](chamados/tests/e3_r3_c7_entrega.txt:40)).

## 15/11 02:37 · sessao f36d7ab685c1 · JUSTIFICATIVA-PAGA-O-PERIODO · leitura · origem=Code

**F-A é fixture incompleta; F-B é expectativa antiga de apresentação.** O código sustenta esses diagnósticos, mas o **primeiro commit responsável por cada valor permanece [não verificado]**: esta cópia não contém histórico Git acessível. Não atribuo um SHA por aproximação. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:213`; `app/ponto/services/efeito_ausencia.py:96`)

**F-A — 635 versus 550**

**(1) Origem atual e commit.** A linha que devolve o saldo é `periodo['minutos'] = max(0, previsto - trabalhado)`. Ela recebe **previsto 635 e realizado zero da ata**; o previsto é lido da ata antes de qualquer fallback. Primeiro commit da série que passou a produzir esse resultado: **[não verificado]**. (`app/ponto/services/efeito_ausencia.py:159`; `app/ponto/services/efeito_ausencia.py:165`; `app/ponto/services/efeito_ausencia.py:170`; `app/escala/services/leitor_celula.py:372`)

**(2) Quem escreve, quem lê e de onde sai 635.**

| Etapa | Escritor → leitor; significado |
|---|---|
| Cadastro | `_cadastrar_pausas` grava **10:00–11:25 e 14:00–15:25** no template. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:280`) |
| Previsto preliminar | `_justificar` consulta o previsto **antes de criar a célula**. Sem célula, o vínculo lê as duas pausas do template: **720−85−85=550 minutos**. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:211`; `app/escala/models.py:1208`; `app/escala/utils.py:2338`; `app/escala/utils.py:2352`) |
| DNA da fixture | Logo depois, o helper cria a célula copiando somente `hi,hf,hii,hfi`. **Não escreve `hii2/hfi2`**. Grava ainda o previsto preliminar na ata. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:213`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:217`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:220`) |
| Nova lavra | Criar a justificativa aprovada aciona o cartório. A grade lê os pares presentes no DNA: agora existe **uma pausa**. Soma **07–10 = 180** e **11:25–19 = 455**, totalizando **635 minutos**. (`app/ponto/signals.py:161`; `app/ponto/signals.py:165`; `app/ponto/services/cartorio.py:1175`; `app/escala/utils.py:57`; `app/escala/utils.py:1658`; `app/escala/utils.py:1678`) |
| Persistência e saldo | A grade publica 635; `ata_do_dia` copia o previsto, a porta grava a ata e a fonte lê esse valor. A única entrada não contém par fechado: realizado zero; saldo **635−0=635**. (`app/escala/utils.py:1852`; `app/ponto/services/cartorio.py:739`; `app/ponto/portas/celula.py:1091`; `app/ponto/turnos.py:733`; `app/ponto/services/efeito_ausencia.py:170`) |

A chave é o **dia da jornada/turno**, inclusive quando atravessa a meia-noite; previsto, realizado e saldo são **minutos**. Ata sem realizado responde `None`, desconhecido; realizado lavrado zero responde zero. Separadamente, `cobre=None` significa cobertura inteira, não desconhecimento. (`app/ponto/turnos.py:708`; `app/ponto/turnos.py:719`; `app/escala/utils.py:2278`; `app/escala/services/leitor_celula.py:354`; `app/escala/services/leitor_celula.py:360`; `app/ponto/services/efeito_ausencia.py:106`)

**(3) Resposta pela regra.** A célula nasce **depois** do cadastro das pausas, durante `_justificar`; a pista de “célula gerada antes” não explica este caso. Porém ela nasce **sem a segunda pausa**. A soberania é da célula congelada, e a grade é fonte única do previsto; uma chave ausente da segunda pausa significa uma pausa só. Portanto **635 corresponde à célula efetivamente montada; 550 corresponde ao caso de duas pausas que o teste pretendia montar**. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:291`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:293`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:213`; `app/docs/LEIS.md:71`; `app/docs/LEIS.md:75`; `app/escala/models.py:1173`; `app/escala/models.py:1177`)

**(4) Julgamento e montagem positiva.** O vermelho não prova perda da segunda pausa pela produção: **a fixture não a colocou no DNA**. Para conservar a prova positiva de 550, cadastrar ambas e criar a célula com **`montar_dna(self.ec, self.d, True)` antes da aprovação**, substituindo a cópia manual da quatro-tupla. Esse produtor lê todas as pausas e escreve o segundo par. O comentário sobre a v2 não comprova a montagem atual. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:217`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:289`; `app/ponto/management/commands/gerar_celulas.py:73`; `app/ponto/management/commands/gerar_celulas.py:91`; `app/ponto/management/commands/gerar_celulas.py:101`)

**F-B — `None` versus 1**

**(1) Origem atual e commit.** O escritor direto é: [nao verificado]

`resumo.update(dias_abono=None, horas_abono=None, motivo_dias_abono='sem_ata')`.

O PDF o chama **depois de copiar o número do motor**. A entrega identifica essa alteração como **c10**; o SHA introdutor exato no `git log` permanece **[não verificado]**. (`app/ponto/services/efeito_ausencia.py:96`; `app/relatorios/pdf_espelho.py:479`; `app/relatorios/pdf_espelho.py:483`; `app/ponto/tests/c10_entrega.txt:16`; `app/ponto/tests/c10_entrega.txt:21`)

**(2) Caminho dos valores.**

| Etapa | Escritor → leitor; significado |
|---|---|
| Fixture | Deixa `dia_abono` sem batidas e cria somente a justificativa aprovada, sem montar sua ata. (`app/relatorios/test_espelho_falta.py:45`; `app/relatorios/test_espelho_falta.py:50`; `app/relatorios/test_espelho_falta.py:54`) |
| Fonte | Seleciona a aprovação por `data_turno`; lê previsto da célula/grade e realizado da ata. Sem realizado lavrado, escreve `minutos=None`, `trabalhado=None`, motivo `sem_ata`. (`app/ponto/services/efeito_ausencia.py:121`; `app/ponto/services/efeito_ausencia.py:139`; `app/ponto/services/efeito_ausencia.py:159`; `app/ponto/services/efeito_ausencia.py:168`) |
| Alimentação | Mantém o dia em `justificadas`, coloca-o em `abono_sem_apuracao` e o exclui do mapa numérico `min_justificados`. (`app/ponto/services/efeito_ausencia.py:55`; `app/ponto/services/efeito_ausencia.py:56`; `app/ponto/services/efeito_ausencia.py:57`) |
| Motor | Sem trabalho e com justificativa, entra em `_abono`; fora do mapa parcial, entra em `_abono_inteiro`: soma **1 dia** e **previsto/60 horas**. (`app/ponto/motor_calculo_v2.py:2611`; `app/ponto/motor_calculo_v2.py:2629`; `app/ponto/motor_calculo_v2.py:2630`; `app/ponto/motor_calculo_v2.py:2631`) |
| Resumo e PDF | O coletor copia os números; o rotulador substitui-os por `None` e escreve `motivo_dias_abono='sem_ata'`. O renderizador lê ambos e imprime **“Abonos — sem apuracao ainda (sem ata)”**. (`app/relatorios/pdf_espelho.py:479`; `app/relatorios/pdf_espelho.py:483`; `app/ponto/services/efeito_ausencia.py:96`; `app/relatorios/pdf_espelho.py:790`) |

Na rota posterior de lavratura, `dias_com_abono` conta dias com crédito e realizado conhecido; realizado ausente também devolve `None`. O cartão escreve `motivo_dias_abono='sem_ata'` e preserva esse `None` ao aplicar os totais. Portanto **dias são contagem; horas/minutos são duração; `None` não é zero**. (`app/ponto/services/dia_pago.py:702`; `app/ponto/services/dia_pago.py:723`; `app/ponto/services/dia_pago.py:737`; `app/relatorios/cartao_pela_celula.py:192`; `app/relatorios/cartao_pela_celula.py:208`)

**(3) Resposta pela regra.** O item 2 do aval fornecido foi incorporado ao contrato: **sem ata e sem batida, entrando em `_abono_inteiro`, vale o previsto por P−0; esse pagamento não é inventado**. Simultaneamente, **tela e PDF permanecem rotulados “sem apuração ainda”**. Assim, neste cadastro, o motor conta o dia inteiro, mas o resumo do PDF deve conter **`dias_abono=None`, `horas_abono=None`, `motivo_dias_abono='sem_ata'`**; o papel mostra o rótulo, não “1”, “0” ou o literal “None”. (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:3`; `app/ponto/tests/test_catraca_abono_do_motor_c11.py:4`; `app/ponto/services/efeito_ausencia.py:96`; `app/relatorios/pdf_espelho.py:790`)

**(4) Julgamento e montagem positiva.** A expectativa `dias_abono=1` está velha **para a apresentação sem ata**. Para conservar o positivo “uma falta e um abono numérico”, manter as batidas e a justificativa, mas fornecer **célula/ata lavrada do `dia_abono`, com previsto correspondente e `minutos_realizados=0`**, deixando `dia_falta` como está. A prova da c10 já declara essa transição: sem ata → ata W=0 → PDF com **1 dia**, sem motivo `sem_ata`. A porta real exige célula existente e é `julgar_celula`. (`app/relatorios/test_espelho_falta.py:58`; `app/ponto/tests/test_justificativa_c10_sem_banco.py:172`; `app/ponto/tests/test_justificativa_c10_sem_banco.py:180`; `app/ponto/tests/test_justificativa_c10_sem_banco.py:181`; `app/ponto/services/cartorio.py:2027`; `app/ponto/services/cartorio.py:2040`)

Execução dessas montagens ajustadas e identificação dos commits introdutores: **[não verificado]**. [nao verificado]

## 15/11 02:36 · sessao 031bbd28fb38 · - · - · origem=Ronald (tmux)

F-A: a célula da fixture é criada **depois** do cadastro das duas pausas, mas nasce com apenas a primeira; a causa é o `zip(('hi','hf','hii','hfi'), marcos)`, que não escreve `hii2/hfi2`. Portanto não é uma célula antiga que antecede o cadastro. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:291`, `:292`, `:293`, `:213`, `:215`, `:217`)

**Caminho dos escritores/leitores**

1. A fixture cria o template 07–19 sem intervalos; `_cadastrar_pausas(duas=True)` declara 10–11:25 e 14–15:25 e salva o template. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:185`, `:187`, `:280`, `:286`)

2. `_bater(07:00,E)` grava a única batida; seu signal chama o cartório, mas o cartório retorna `None` quando não existe célula, sem criá-la. O núcleo também só emite evento para células existentes. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:197`, `:201`; `app/ponto/signals.py:131`, `:135`; `app/ponto/services/cartorio.py:2027`, `:2029`; `app/ponto/nucleo.py:88`, `:93`)

3. `_justificar` mede o previsto **antes** de criar a célula; sem célula, `EscalaColaborador.pausas_do_dia` cai no template, que vê ambas as pausas. A autoridade de previsto desconta todas elas: `720−85−85=550`. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:211`, `:213`; `app/escala/models.py:1208`, `:304`, `:307`; `app/escala/utils.py:2319`, `:2338`, `:2352`, `:2360`)

4. Em seguida o helper cria a célula com somente quatro chaves do DNA: 07,19,10,11:25. A segunda pausa não entra. A ata preliminar recebe o previsto calculado anteriormente (`setdefault`). (`app/ponto/tests/test_justificativa_paga_o_periodo.py:213`, `:217`, `:220`, `:222`)

5. A aprovação aciona novo julgamento, agora com célula existente. O cartório monta a grade; a grade lê as pausas do **DNA**, não completa o DNA com pausas atuais do template. O mapa percorre `PARES_PAUSA_DNA`, mas só acrescenta pares presentes; aqui devolve apenas 10–11:25. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:223`; `app/ponto/signals.py:161`, `:165`; `app/ponto/services/cartorio.py:1175`; `app/escala/utils.py:27`, `:53`, `:61`, `:1658`, `:1661`)

6. A grade soma os segmentos E→S: 07–10 (180) + 11:25–19 (455) = **635 minutos**, publica esse previsto na linha do dia; `ata_do_dia` transporta-o para a ata; a porta `lavrar_veredito` escreve a ata, substituindo o 550 preliminar da fixture. (`app/escala/utils.py:1678`, `:1682`, `:1850`, `:1852`; `app/ponto/services/cartorio.py:739`, `:1851`, `:1853`; `app/ponto/portas/celula.py:1091`)

7. `periodos_justificados` lê primeiro `ata.minutos_previstos`, inclusive zero, e o realizado da mesma ata. A única E não fornece par fechado; o realizado soma zero, conservando `aberto=True`. O saldo final é `max(0,635−0)=635`. (`app/escala/services/leitor_celula.py:372`, `:377`, `:360`, `:362`; `app/ponto/turnos.py:733`, `:739`, `:771`; `app/ponto/services/efeito_ausencia.py:159`, `:170`)

**Regra e diagnóstico**

A lei da casa declara célula soberana, com DNA congelado/irretroativo, e grade como fonte única do previsto; chave ausente significa que aquele dia não declara o marco, não desconhecimento. O leitor de pausas explicita que DNA de uma pausa responde uma pausa, não “não sei”. Logo **635 é coerente com a célula efetivamente montada**, enquanto **550 é coerente com o caso positivo de célula com duas pausas que o teste pretendia montar**. A fixture está incompleta; completar pela leitura do template violaria a autoridade congelada do dia. (`app/docs/LEIS.md:71`, `:75`; `app/escala/models.py:1173`, `:1177`, `:1675`, `:1676`)

Para conservar o caso positivo, cadastrar as duas pausas e depois construir a célula com `montar_dna(self.ec,self.d,True)` antes da aprovação, em vez de copiar a quatro-tupla manualmente. O produtor oficial lê todas as pausas e grava o segundo par `hii2/hfi2`. A célula e a ata desse cenário responderiam 550 pela mesma grade. (`app/ponto/management/commands/gerar_celulas.py:73`, `:89`, `:103`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:213`, `:217`)

**Unidades/datas**

O previsto e o saldo são minutos; o realizado agrega turnos cujo `data_turno` é o dia da jornada, não `timestamp.date()` civil. `None` em realizado significa ata ausente/incompleta; `cobre=None` é outro contrato: cobertura do dia inteiro. (`app/escala/utils.py:2278`; `app/ponto/turnos.py:708`, `:719`; `app/escala/services/leitor_celula.py:354`, `:362`; `app/ponto/services/efeito_ausencia.py:105`, `:112`)

**Histórico**

Não foi possível atribuir o primeiro commit da série: a cópia não contém `.git`; a busca literal por `29460c1d1` nos Markdown da cópia também não retornou registro. Portanto qualquer atribuição de F-A à v2/c8/c9 sem o histórico seria **[não verificado]**.

## 15/11 02:36 · sessao b7d93176c112 · - · - · origem=Ronald (tmux)

F-B: expectativa de `dias_abono=1` no resumo está velha para uma fixture **sem ata**; o motor continua contando o abono inteiro, mas o contrato de apresentação exige `None` com rótulo. [app/ponto/motor_calculo_v2.py:2611](app/ponto/motor_calculo_v2.py:2611), [app/ponto/motor_calculo_v2.py:2628](app/ponto/motor_calculo_v2.py:2628), [app/ponto/services/efeito_ausencia.py:95](app/ponto/services/efeito_ausencia.py:95)

1. **Origem e commit.** A produção escreve os valores finais em `resumo.update(dias_abono=None, horas_abono=None, motivo_dias_abono='sem_ata')`; o PDF chama esse escritor depois de copiar o resultado do motor. O próprio código atribui esse comportamento à **c10**. SHA exato e identificação pelo `git log`: **[não verificado]**, pois a cópia não contém `.git`. [app/ponto/services/efeito_ausencia.py:86](app/ponto/services/efeito_ausencia.py:86), [app/ponto/services/efeito_ausencia.py:95](app/ponto/services/efeito_ausencia.py:95), [app/relatorios/pdf_espelho.py:479](app/relatorios/pdf_espelho.py:479)

2. **Caminho dos valores.**
   - A fixture deixa os dois últimos dias previstos sem batidas; cria somente a justificativa aprovada para o penúltimo e coleta o PDF, sem chamada de lavra. [app/relatorios/test_espelho_falta.py:43](app/relatorios/test_espelho_falta.py:43), [app/relatorios/test_espelho_falta.py:50](app/relatorios/test_espelho_falta.py:50)
   - `periodos_justificados` seleciona aprovação por **`data_turno`**, usa essa data como chave e lê previsto da célula/grade; o trabalhado vem da ata. Sem ata ou sem `minutos_realizados`, o leitor devolve `None`, significando **desconhecido**, distinto de W=0. [app/ponto/services/efeito_ausencia.py:121](app/ponto/services/efeito_ausencia.py:121), [app/ponto/services/efeito_ausencia.py:139](app/ponto/services/efeito_ausencia.py:139), [app/ponto/services/efeito_ausencia.py:158](app/ponto/services/efeito_ausencia.py:158), [app/escala/services/leitor_celula.py:354](app/escala/services/leitor_celula.py:354)
   - O previsto é lido primeiro de `ata.minutos_previstos`, inclusive zero; ausente, consulta S133 `minutos_previstos_do_dia`. São **minutos**, enquanto `dias_abono` é contagem de dias. [app/escala/services/leitor_celula.py:365](app/escala/services/leitor_celula.py:365), [app/ponto/motor_calculo_v2.py:2630](app/ponto/motor_calculo_v2.py:2630)
   - `efeitos_da_ausencia` mantém o dia em `justificadas`, põe-o em `abono_sem_apuracao` e o exclui de `min_justificados`. O PDF fornece esses mapas ao motor. [app/ponto/services/efeito_ausencia.py:54](app/ponto/services/efeito_ausencia.py:54), [app/relatorios/pdf_espelho.py:333](app/relatorios/pdf_espelho.py:333), [app/relatorios/pdf_espelho.py:394](app/relatorios/pdf_espelho.py:394), [app/relatorios/pdf_espelho.py:407](app/relatorios/pdf_espelho.py:407)
   - Sem trabalho e com aprovação, o motor inclui o dia em `_abono`; ausente de `_mj`, inclui em `_abono_inteiro`, soma **1 dia** e o previsto dividido por 60 às horas. O PDF copia esse número e depois o rotulador escreve `None` e `sem_ata`. [app/ponto/motor_calculo_v2.py:2611](app/ponto/motor_calculo_v2.py:2611), [app/ponto/motor_calculo_v2.py:2628](app/ponto/motor_calculo_v2.py:2628), [app/relatorios/pdf_espelho.py:479](app/relatorios/pdf_espelho.py:479), [app/ponto/services/efeito_ausencia.py:95](app/ponto/services/efeito_ausencia.py:95)
   - A renderização lê os dois campos e imprime **“Abonos — sem apuracao ainda (sem ata)”**, sem imprimir 1 ou zero. [app/relatorios/pdf_espelho.py:790](app/relatorios/pdf_espelho.py:790)

3. **Regra.** O item 2 do aval anexado diz expressamente que o dia **sem ata e sem batida**, entrando em `_abono_inteiro`, vale o previsto por **COMPLETA-ATÉ-O-PREVISTO, P−0**; portanto o valor do motor é correto, não “inventado”. O mesmo aval mantém tela/PDF rotulados “sem apuração ainda”. O anexo não existe como arquivo numerado na cópia: **[não verificado: localização arquivo:linha do aval]**. A implementação da apresentação coincide com essa orientação. [app/ponto/motor_calculo_v2.py:2628](app/ponto/motor_calculo_v2.py:2628), [app/ponto/services/efeito_ausencia.py:95](app/ponto/services/efeito_ausencia.py:95), [app/relatorios/pdf_espelho.py:790](app/relatorios/pdf_espelho.py:790)

4. **Montagem que conserva o positivo.** Para continuar provando **falta=1 e abono=1 no PDF**, a fixture deve fornecer **ata lavrada para `dia_abono`, com `minutos_realizados=0` e previsto correspondente**, conservando o dia sem batidas e deixando `dia_falta` como está. O teste da própria c10 demonstra a transição sem ata → ata W=0 → PDF/tela com 1 dia, sem motivo `sem_ata`; há também prova ORM positiva com essa ata. [app/ponto/tests/test_justificativa_c10_sem_banco.py:168](app/ponto/tests/test_justificativa_c10_sem_banco.py:168), [app/ponto/tests/test_justificativa_paga_o_periodo.py:622](app/ponto/tests/test_justificativa_paga_o_periodo.py:622), [app/ponto/tests/test_justificativa_paga_o_periodo.py:644](app/ponto/tests/test_justificativa_paga_o_periodo.py:644)

A porta real de lavra é `julgar_celula`; o teste ORM usa essa porta e verifica o W persistido. Execução da montagem modificada de F-B: **[não verificado]**, não rodei banco nem testes. [app/ponto/services/cartorio.py:2011](app/ponto/services/cartorio.py:2011), [app/ponto/tests/test_justificativa_paga_o_periodo.py:703](app/ponto/tests/test_justificativa_paga_o_periodo.py:703)

## 15/11 02:20 · sessao 6e535e4da7d0 · BATIDA-DEDUP-RETRATADA · verificador · origem=Code

**Não demonstrei regressão nova de produção no diff. A corrida de classificação do v2 está fechada, e o horário da triagem foi restaurado. Encontrei três lacunas concretas de testes e a corrida SQL preexistente.** (`app/ponto/registro_batida.py:155`; `app/ponto/registro_batida.py:161`; `app/ponto/services/triagem_batida.py:657`; `app/ponto/tests/dedup_retratada_c3_checks.txt:39`)

Referências abaixo relativas a `app/`, salvo indicação contrária.

| Arquivo:linha | O que está errado | Caso concreto | Gravidade |
|---|---|---|---|
| `ponto/services/triagem_batida.py:666`; `ponto/tests/test_dedup_retratada_c1.py:150` | **Cobertura insuficiente de sequência no dedup.** Mutante acrescentando `sequencia=1` ao `.filter(tipo=tipo)` passou nos **31 testes**, inclusive tripwire e paridade. As fixtures anteriores têm sequência 1; o selo verifica a origem da consulta, sem conferir esse filtro. (`ponto/tests/test_dedup_retratada_c1.py:194`) | Viva S18:00, sequência 2; pedido S18:03. Original responde `ja_registrada`; mutante chama a porta, cria segunda linha e preenche sucesso. Reproduzido em memória pelo executor existente. (`ponto/tests/test_dedup_retratada_c2.py:47`) | **fila** — lacuna da bateria; esse filtro não existe no código atual. |
| `ponto/registro_batida.py:161`; `ponto/tests/test_dedup_retratada_c1.py:26` | **Cobertura temporal concentrada no segundo zero.** Mutante `batida.criada_neste_toque = timestamp.second == 0` passou nos **31 testes**. Variar microssegundos conserva o segundo zero. (`ponto/tests/test_dedup_retratada_c3.py:79`) | Nova batida às 18:03:01 nas três cascas: mutante grava uma linha, publica SSE, responde `ja_registrada` e pula foto/ordem. Reproduzido em memória; o código atual atribui `True` incondicionalmente. (`ponto/tests/test_dedup_retratada_c2.py:64`; `ponto/services/resposta_batida.py:14`) | **fila** — defeito do mutante, não do original. |
| `ponto/services/triagem_batida.py:716`; `ponto/tests/test_dedup_retratada_c2.py:252` | **Falta controle com duas opostas vivas elegíveis.** Remover `.order_by('-timestamp')` passou nos **31 testes**. Sem essa ordenação, a autoridade entrega ordem crescente. (`ponto/turnos.py:2883`) | E18:00/id1 e E18:02/id2; pedido S18:03, janela 300 s. Original apresenta id2; mutante apresenta id1. Reproduzido em memória. (`ponto/tests/test_dedup_retratada_c2.py:47`) | **fila** — cobertura insuficiente da testemunha apresentada. |
| `ponto/registro_batida.py:152`; `ponto/registro_batida.py:158`; `ponto/models.py:106` | **`IntegrityError` concorrente continua sem captura.** É preexistente; o diff somente acrescenta as marcas após os creates. (`ponto/tests/dedup_retratada_c3_checks.txt:38`) | A e B consultam ausência; B grava antes do create de A. A colide na unicidade colaborador/timestamp e não alcança marca/helper. Resposta HTTP e ocorrência em produção: **[nao verificado]**. | **fila** — distinta da corrida do v2. |

Os três mutantes foram aplicados somente em memória, nos dois mapas `SOBRESCRITAS`; também adaptei a leitura de `open()` para conferir a paridade sobre fontes mutadas. Cada execução terminou com **31 testes, zero falhas e zero erros**. Os pontos de injeção e execução são os usados pela própria entrega. (`ponto/tests/test_dedup_retratada_c3.py:228`; `ponto/tests/test_dedup_retratada_c3.py:234`; `ponto/tests/test_dedup_retratada_c2.py:288`)

A marca e os valores consumidos pelo diff ficaram assim:

| Valor | Quem escreve e quem lê |
|---|---|
| `criada_neste_toque` | **Dois escritores de produção**, ambos dentro de `registrar_batida`, imediatamente após cada create. **Um leitor direto**, o helper; as três cascas consomem seu resultado. Não é campo do modelo nem argumento persistido pelo create. (`ponto/registro_batida.py:155`; `ponto/registro_batida.py:161`; `ponto/services/resposta_batida.py:14`; `ponto/models.py:8`; `api/views.py:693`; `api/views_core.py:684`; `ponto/services/triagem_batida.py:723`) |
| Colaborador, instante e tipo pedidos | Cascas fornecem os argumentos; dedup/porta os usam nas consultas; create persiste os argumentos. As APIs usam `ts_efetivo`; triagem usa `agora`. (`api/views.py:507`; `api/views.py:531`; `api/views.py:654`; `api/views_core.py:645`; `ponto/services/triagem_batida.py:626`; `ponto/services/triagem_batida.py:700`; `ponto/registro_batida.py:153`) |
| `retratada_em` | Dedup lê o filtro da autoridade; porta mantém seus filtros vivos e o lookup de chave exata. Helper não usa retratação para inferir criação. Nenhuma retratação é desfeita pelo delta. (`ponto/turnos.py:2876`; `ponto/registro_batida.py:109`; `ponto/registro_batida.py:120`; `ponto/services/resposta_batida.py:14`) |
| PK, tipo, timestamp e sequência devolvidos | Helper lê exclusivamente o objeto retornado. Apresenta seu timestamp local, preservando microssegundos, e sua sequência antiga. A sequência de uma nova continua vindo de `sequencia_na_jornada`. (`ponto/services/resposta_batida.py:17`; `ponto/services/resposta_batida.py:21`; `api/views.py:554`; `api/views_core.py:540`; `ponto/services/triagem_batida.py:672`) |
| `status`, mensagem, sucesso e disputa | Helper escreve `ja_registrada`, mensagem derivada do objeto, `sucesso=True`, `em_disputa=False`, motivos vazios. As duas últimas constantes não certificam a disputa atual da batida. (`ponto/services/resposta_batida.py:19`) |
| `_resposta_existente` | Helper produz; cada casca retorna antes dos efeitos posteriores de criação. (`api/views.py:693`; `api/views_core.py:684`; `ponto/services/triagem_batida.py:723`) |
| `g_op`, `op`, id/tipo da recusa | `_janela_oposto` fornece a janela; triagem consulta a autoridade viva e apresenta a candidata. Essa consulta não decide a recusa. (`ponto/registro_batida.py:21`; `ponto/registro_batida.py:145`; `ponto/services/triagem_batida.py:714`) |
| `fora_horario_info`, justificativa e aprovação | Emissor escreve informação/contexto/sessão; triagem transforma a informação em justificativa e `aprovada=False` enviados à porta. (`ponto/services/triagem_batida.py:478`; `ponto/services/triagem_batida.py:486`; `ponto/services/triagem_batida.py:685`; `ponto/services/triagem_batida.py:709`) |
| `ALLOW`, contagem e dono | Tripwire declara os totais e compara igualdade exata: APIs 2/2, triagem 1, helper 0. (`ponto/tests/test_tripwire_bocais_apuracao.py:21`; `ponto/tests/test_tripwire_bocais_apuracao.py:27`; `ponto/tests/test_tripwire_bocais_apuracao.py:37`) |

**Não encontrei caminho atual de criação chegando sem marca, nem reutilização chegando marcada.** Entre create e retorno não há cópia, refresh ou reconsulta; entre retorno e helper também não. Fechamento recebe colaborador/instante, sem receber a batida. Os receivers inspecionados não substituem a instância retornada. (`ponto/registro_batida.py:152`; `ponto/registro_batida.py:177`; `api/views.py:669`; `api/views_core.py:660`; `ponto/services/triagem_batida.py:713`; `ponto/signals.py:35`; `ponto/signals.py:132`; `ponto/signals.py:207`; `colaboradores/signals.py:87`)

O censo direto fora dos testes encontrou **16 sítios**, incluindo o comando de ensaio: [nao verificado]

| Chamador | Arquivo:linha; consumo |
|---|---|
| Três cascas | `api/views.py:654`; `api/views_core.py:645`; `ponto/services/triagem_batida.py:700`: objeto passa diretamente ao helper. |
| Justificativas e aprovação em lote | `ponto/services/justificativa.py:188`; `ponto/services/justificativa.py:361`; `ponto/views.py:1818`: leem `None`, PK, justificativa e vinculam FK. (`ponto/services/justificativa.py:195`; `ponto/services/justificativa.py:370`; `ponto/views.py:1825`) |
| Veredito | `ponto/services/veredito_celula.py:323`: verifica recusa por `None`. (`ponto/services/veredito_celula.py:325`) |
| Materialização | `chamados/services/materializacao.py:933`; `chamados/services/materializacao.py:1013`: leem vínculo/PK e anotam campos explícitos. (`chamados/services/materializacao.py:952`; `chamados/services/materializacao.py:1041`) |
| Regularização externa | `chamados/services/regularizacao_ext.py:78`: lê PK para auditoria e retorna o objeto. (`chamados/services/regularizacao_ext.py:95`; `chamados/services/regularizacao_ext.py:106`) |
| Exceção autorizada | `chamados/services/acoes_chamado.py:44`: lê justificativa e passa objeto a anotação/foto. (`chamados/services/acoes_chamado.py:54`; `chamados/services/acoes_chamado.py:67`) |
| Comando de ensaio | `ponto/management/commands/testar_pwa.py:184`, `:198`, `:219`, `:228`, `:229`: três retornos descartados; depois compara `None`/PK. (`ponto/management/commands/testar_pwa.py:230`) |

Nesses consumidores não encontrei enumeração dos atributos da batida que incorporasse a marca nova. `anotar` lê somente campos explicitamente fornecidos, sob allowlist; SSE também monta payload explícito. Chamadas dinâmicas e execução desses fluxos com ORM real: **[nao verificado]**. (`ponto/registro_batida.py:243`; `ponto/registro_batida.py:246`; `ponto/realtime.py:32`)

Para os ramos da porta, o comportamento é:

| Ramo | APIs UI/core | Triagem |
|---|---|---|
| Viva do mesmo tipo ≤60 s | Se alcançar a porta, retorna existente sem marca; helper responde `ja_registrada` com seus campos e interrompe efeitos posteriores. Normalmente o dedup de cinco minutos captura antes; correção de tipo pode permitir alcançar a porta. (`ponto/registro_batida.py:113`; `api/views.py:577`; `api/views.py:693`; `api/views_core.py:684`) | Mesmo tratamento pelo helper; pode alcançar por intercalação concorrente ou batida futura dentro dos 60 s. (`ponto/registro_batida.py:113`; `ponto/services/triagem_batida.py:666`; `ponto/services/triagem_batida.py:723`) |
| Mesma chave viva/retratada/outro tipo | Consulta temporal tem precedência; depois chave exata. Retorna objeto antigo, sem desfazer retratação, com `ja_registrada` e sem efeitos posteriores. (`ponto/registro_batida.py:113`; `ponto/registro_batida.py:120`; `ponto/services/resposta_batida.py:19`) | Igual. (`ponto/services/triagem_batida.py:723`) |
| Oposta → `None` | `ignorada_intervalo` antes do helper; sem efeitos posteriores. Reconsulta oposta para apresentar payload. (`api/views.py:669`; `api/views_core.py:660`) | `ignorada_intervalo`, com id/tipo consultados no universo vivo e janela da porta; sem efeitos posteriores. (`ponto/services/triagem_batida.py:713`) |
| Create com conflito revisado | **Inalcançável pelas três chamadas**, que passam autoridade humana falsa. Na porta, esse ramo marca True e agenda fechamento. (`api/views.py:656`; `api/views_core.py:647`; `ponto/services/triagem_batida.py:702`; `ponto/registro_batida.py:150`) | Mesma restrição. (`ponto/services/triagem_batida.py:702`) |
| Create normal, inclusive oposta admitida pelo marco próprio | Marca True; helper libera ordem, detector/chamado leve, disputa, foto, geofence, autoresolve de entrada e aviso; responde `ok`. Efeitos são condicionais aos respectivos ramos. (`ponto/registro_batida.py:145`; `ponto/registro_batida.py:158`; `api/views.py:696`; `api/views.py:704`; `api/views.py:758`; `api/views.py:772`; `api/views.py:792`; `api/views.py:812`) | Libera disputa pendente, ordem, foto, geofence, autoresolve, contexto de sucesso e aviso. (`ponto/services/triagem_batida.py:726`; `ponto/services/triagem_batida.py:745`; `ponto/services/triagem_batida.py:757`) |
| `IntegrityError` concorrente | Exceção escapa antes da marca/helper; HTTP efetivo **[nao verificado]**. (`ponto/registro_batida.py:158`) | Igual. (`ponto/services/triagem_batida.py:700`) |

Criação também dispara os receivers do ORM e fechamento da porta; reutilização não executa novo create. SSE consulta `created` do ORM, independentemente da marca transiente. (`ponto/signals.py:40`; `ponto/registro_batida.py:156`; `ponto/registro_batida.py:162`)

**A corrida específica do v2 está fechada:** se B confirma antes das consultas de A, A reutiliza objeto sem marca; somente o create executado por A recebe a marca. A colisão posterior às consultas permanece sendo o problema SQL separado da tabela. (`ponto/registro_batida.py:113`; `ponto/registro_batida.py:120`; `ponto/registro_batida.py:161`)

Quanto à ordem e ao escopo completo:

| Conferência | Resultado |
|---|---|
| `_bloqueio_horario_clt` | Está incondicionalmente antes da recuperação da confirmação e do dedup. Pode criar chamado e atualizar contexto/sessão mesmo quando depois houver reutilização ou recusa. Essa posição corresponde ao diff fornecido e à comparação histórica registrada; não reexecutei o objeto git nesta cópia. (`ponto/services/triagem_batida.py:657`; `ponto/services/triagem_batida.py:459`; `ponto/services/triagem_batida.py:486`; `ponto/tests/dedup_retratada_c3_checks.txt:39`) |
| Outros efeitos anteriores à porta | Não identifiquei outro reposicionamento no diff integral fornecido. O novo predicado permite que tentativas antes barradas pela retratada alcancem o preparo já existente. (`api/views.py:531`; `api/views.py:554`; `ponto/services/triagem_batida.py:666`; `ponto/services/triagem_batida.py:672`) |
| Janelas, dupla viva e sequência | APIs conservam ±5 min e escolha mais recente; triagem conserva `[agora−5min, agora]` e escolha crescente. Porta conserva ±60 s para mesmo tipo. Mensagens da dupla viva e autoridade da sequência permanecem. (`api/views.py:529`; `api/views.py:531`; `api/views.py:542`; `ponto/services/triagem_batida.py:665`; `ponto/turnos.py:2883`; `ponto/models.py:105`; `ponto/registro_batida.py:106`) |
| Mudanças além do filtro | O helper muda resposta/efeitos de reutilização. Na triagem, `ignorada_intervalo` passa a apresentar testemunha viva usando 60/300 s da porta, em lugar da reconsulta crua fixa em 60 s. Não muda quem a porta recusa. (`ponto/services/resposta_batida.py:14`; `ponto/services/triagem_batida.py:714`; `ponto/registro_batida.py:145`) |
| Escritor, universo e juiz | Não identifiquei escritor novo fora da porta nem derivação paralela do universo vivo. Helper apresenta o resultado causal declarado pela porta; não decide admissibilidade. (`ponto/registro_batida.py:155`; `ponto/services/resposta_batida.py:14`; `ponto/turnos.py:2876`; `core/portas.py:103`) |

Os selos **mordem os casos declarados**, mas não certificam toda a semântica: tripwire conta regex exata; C1 acompanha origem da consulta; paridade compara AST das funções de mesmo nome nas duas APIs, com whitelist vazia. O harness original da paridade lê disco, sem `SOBRESCRITAS`; essa limitação já está declarada na entrega. (`ponto/tests/test_tripwire_bocais_apuracao.py:30`; `ponto/tests/test_dedup_retratada_c1.py:175`; `api/tests/test_contract_paridade_kernels.py:12`; `api/tests/test_contract_paridade_kernels.py:19`; `api/tests/test_contract_paridade_kernels.py:31`; `ponto/tests/dedup_retratada_c3_entrega.txt:162`)

Reexecutei C1–C3: **31 testes OK**, incluindo os contratos existentes. Suite inteira, opus final, smoke real col677 nas duas cascas e medições de produção dos 60 dias: **[nao verificado]**; os testes locais não encerram esses requisitos. (`ponto/tests/dedup_retratada_c3_green.txt:38`; `ponto/tests/dedup_retratada_c3_entrega.txt:169`; `ponto/tests/dedup_retratada_c3_entrega.txt:176`)

## 15/11 02:18 · sessao 085ea1e4b857 · - · - · origem=Ronald (tmux)

Achado reproduzido, referências relativas a `app/`:

| Arquivo:linha | O que está errado | Caso concreto | Gravidade |
|---|---|---|---|
| `ponto/services/triagem_batida.py:666`; `ponto/tests/test_dedup_retratada_c1.py:261` | Falta teste com **duas duplas vivas elegíveis**. Mutante de uma linha adicionando `.order_by('-timestamp')` antes de `.first()` passa nos 31 testes C1–C3, com zero falhas/erros. A paridade API fica legitimamente intacta; o tripwire mantém a mesma contagem. | Duas S vivas: 17:59, id=1; 18:02, id=2. Tentativa às 18:03: original retorna id=1; mutante retorna id=2. O teste existente põe apenas uma viva e uma retratada. (`ponto/tests/test_dedup_retratada_c1.py:261–268`; `ponto/tests/test_tripwire_bocais_apuracao.py:30`; `api/tests/test_contract_paridade_kernels.py:28`) | **fila**: lacuna real de cobertura; a implementação atual conserva a escolha anterior. (`ponto/models.py:105`; `ponto/turnos.py:2883`) |
| `ponto/registro_batida.py:152`; `ponto/registro_batida.py:158`; `ponto/models.py:106` | `IntegrityError` concorrente continua sem captura nesses creates. É preexistente conforme o diff fornecido; não confundir com a corrida de classificação do v2. | A e B passam pelas duas consultas sem linha; B grava a chave antes do create de A. A encontra a restrição `(colaborador,timestamp)`. Resposta HTTP/ocorrência em produção: **[nao verificado]**. (`ponto/registro_batida.py:107`; `ponto/registro_batida.py:120`; `ponto/models.py:106`) | **fila** |

Ramos conferidos:

| Ramo da porta | Resposta e efeitos nas cascas |
|---|---|
| Viva mesmo tipo ≤60 s | Retorna existente sem marca, sem create/fechamento da porta; helper produz `ja_registrada`, interrompendo efeitos posteriores. Normalmente dedup de cinco minutos captura antes; APIs podem chegar após correção de tipo, triagem pode chegar por concorrência/futuro ≤60 s. (`ponto/registro_batida.py:113`; `ponto/services/resposta_batida.py:14`; `api/views.py:577`; `api/views.py:693`; `api/views_core.py:684`; `ponto/services/triagem_batida.py:723`) |
| Mesma chave viva/retratada/outro tipo | Temporal ganha primeiro; caso contrário retorna a chave exata sem marca. Payload usa os campos antigos; não cria, não desfaz retratação e não roda efeitos posteriores. (`ponto/registro_batida.py:113`; `ponto/registro_batida.py:120`; `ponto/services/resposta_batida.py:17`) |
| Oposta → `None` | As três chamadas usam `autoridade_humana=False`; retornam `ignorada_intervalo` antes do helper/efeitos posteriores. Triagem consulta agora a janela da porta e o universo vivo para apresentar id/tipo. (`api/views.py:656`; `api/views.py:669`; `api/views_core.py:647`; `api/views_core.py:660`; `ponto/services/triagem_batida.py:702`; `ponto/services/triagem_batida.py:713`) |
| Create com conflito revisado | Marca True e fechamento; inacessível pelas três chamadas, que passam autoridade False. (`ponto/registro_batida.py:150`; `ponto/registro_batida.py:152`; `ponto/registro_batida.py:155`; `api/views.py:656`; `api/views_core.py:647`; `ponto/services/triagem_batida.py:702`) |
| Create normal, inclusive oposta admitida pelo marco | Marca True, fechamento; helper libera efeitos. APIs: ordem, detector/chamado leve, disputa, foto, geofence, autoresolve de E, aviso, `ok`. Triagem: disputa quando pendente, ordem, foto, geofence, autoresolve, contexto de sucesso e aviso. (`ponto/registro_batida.py:145`; `ponto/registro_batida.py:158`; `api/views.py:696`; `api/views.py:702`; `api/views.py:758`; `api/views.py:772`; `api/views.py:780`; `api/views.py:792`; `api/views.py:810`; `ponto/services/triagem_batida.py:726`; `ponto/services/triagem_batida.py:745`; `ponto/services/triagem_batida.py:748`; `ponto/services/triagem_batida.py:757`) |
| `IntegrityError` | Exceção escapa antes da marca/helper/efeitos posteriores; HTTP efetivo **[nao verificado]**. (`ponto/registro_batida.py:152`; `ponto/registro_batida.py:155`; `ponto/registro_batida.py:158`; `ponto/registro_batida.py:161`) |

Em todos os ramos de triagem, `_bloqueio_horario_clt` roda antes da confirmação/dedup/porta e pode criar chamado e atualizar contexto/sessão. Essa posição/condição coincide com o diff integral fornecido e com a conferência histórica registrada; o objeto git `880331a8c` não está disponível nesta cópia para repetir a comparação. (`ponto/services/triagem_batida.py:657`; `ponto/services/triagem_batida.py:459`; `ponto/services/triagem_batida.py:478`; `ponto/services/triagem_batida.py:486`; `ponto/tests/dedup_retratada_c3_checks.txt:28`; `ponto/tests/dedup_retratada_c3_checks.txt:39`)

Não encontrei alteração adicional de janela, sequência ou mensagem da dupla viva no código atual: APIs conservam ±5 min e mais recente; triagem conserva `[agora−5min,agora]` e mais antiga para o colaborador fixo. A sequência continua calculada pela mesma autoridade. (`api/views.py:529`; `api/views.py:531`; `api/views.py:541`; `api/views.py:554`; `api/views_core.py:515`; `api/views_core.py:517`; `api/views_core.py:540`; `ponto/services/triagem_batida.py:665`; `ponto/services/triagem_batida.py:666`; `ponto/services/triagem_batida.py:672`; `ponto/models.py:105`; `ponto/turnos.py:2883`)

O helper acrescenta classificação do retorno para suprimir efeitos posteriores em reutilização; não consulta universo nem escreve `Batida`. A mudança de `ignorada_intervalo` na triagem é de identificação da testemunha após a recusa, com a mesma janela e universo da porta; a decisão de recusar continua exclusivamente na porta. (`ponto/services/resposta_batida.py:7`; `ponto/services/resposta_batida.py:14`; `ponto/registro_batida.py:138`; `ponto/registro_batida.py:145`; `ponto/services/triagem_batida.py:713`)

## 15/11 02:18 · sessao 4b4dee27661a · - · - · origem=Ronald (tmux)

Encontrei três mutantes de uma linha que sobrevivem aos **31 testes C1+C2+C3**, inclusive com `builtins.open` adaptado para a paridade ler fontes mutadas. São lacunas da bateria; os defeitos abaixo pertencem aos mutantes, não ao código atual. (`app/ponto/tests/test_dedup_retratada_c2.py:288`; `app/ponto/tests/test_dedup_retratada_c3.py:234`)

| Sítio | Mutante sobrevivente | Caso concreto fora da bateria | Gravidade sugerida |
|---|---|---|---|
| `app/ponto/registro_batida.py:161` | `batida.criada_neste_toque = timestamp.second == 0` | Nova S às **18:03:01**, nas três cascas: grava uma linha/SSE, responde `ja_registrada` e pula foto/ordem. Todos os creates da bateria partem de `FATO` com segundo zero; variar microssegundos conserva isso. (`app/ponto/tests/test_dedup_retratada_c1.py:26`; `app/ponto/tests/test_dedup_retratada_c3.py:79`) | **fila**: cobertura insuficiente da marca; não demonstra bug atual. |
| `app/ponto/services/triagem_batida.py:666` | Acrescentar `sequencia=1` ao `.filter(tipo=tipo)` | Viva S18:00 com sequência 2 + pedido S18:03: original retorna `ja_registrada`; mutante cria segunda batida. As fixtures `anterior()` fixam sequência 1; o selo verifica origem da consulta, sem proibir esse filtro. (`app/ponto/tests/test_dedup_retratada_c1.py:150`; `app/ponto/tests/test_dedup_retratada_c1.py:194`) | **fila**. |
| `app/ponto/services/triagem_batida.py:716` | Remover `.order_by('-timestamp')` | E18:00 id1 e E18:02 id2, pedido S18:03, janela 300 s: original responde `ignorada_intervalo` id2; mutante id1. O ORM também muda, pois a ordenação padrão é crescente. (`app/ponto/models.py:105`; `app/ponto/tests/test_dedup_retratada_c2.py:252`) | **fila**. |

Reprodução compacta do sobrevivente principal, sem escrita:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import sys, io, unittest
sys.path.insert(0, 'app')
from ponto.tests import test_dedup_retratada_c1 as c1
from ponto.tests import test_dedup_retratada_c2 as c2

p = 'ponto/registro_batida.py'
antes = '\n    batida.criada_neste_toque = True\n'
depois = '\n    batida.criada_neste_toque = timestamp.second == 0\n'
fonte = c2.fonte(p)
assert fonte.count(antes) == 1
c1.SOBRESCRITAS[p] = c2.SOBRESCRITAS[p] = fonte.replace(antes, depois)

nomes = ['ponto.tests.test_dedup_retratada_c1',
         'ponto.tests.test_dedup_retratada_c2',
         'ponto.tests.test_dedup_retratada_c3']
unittest.TextTestRunner().run(
    unittest.defaultTestLoader.loadTestsFromNames(nomes))
for sitio in c1.SITIOS:
    resposta, store, _, efeitos, _ = c2.executar(
        sitio, [], instante=c1.FATO.replace(second=1))
    print(sitio, resposta['status'], len(store.linhas),
          efeitos['foto'].call_count, efeitos['ordem'].call_count)
PY
```

Resultado observado: `31 testes OK`; nas três cascas, `ja_registrada 1 0 0`. O mecanismo de injeção utilizado é o mesmo `SOBRESCRITAS` da entrega. (`app/ponto/tests/test_dedup_retratada_c3.py:228`)

O tripwire tem mordida real contra queries diretas novas/restauradas e compara contagem **exata**, mas continua uma regex de `Batida|_Bat.objects.filter`; não certifica semântica de filtros, ordenação ou causalidade. (`app/ponto/tests/test_tripwire_bocais_apuracao.py:30`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:37`; `app/ponto/tests/test_dedup_retratada_c2.py:266`; `app/ponto/tests/test_dedup_retratada_c3.py:172`)

A paridade compara AST de funções com nomes presentes nos dois módulos, whitelist vazia; o harness entregue normalmente lê disco via `open()`, sem aplicar `SOBRESCRITAS`. Os sobreviventes acima não alteram APIs, e permaneceram verdes mesmo com essa leitura corrigida em memória. (`app/api/tests/test_contract_paridade_kernels.py:12`; `app/api/tests/test_contract_paridade_kernels.py:19`; `app/api/tests/test_contract_paridade_kernels.py:31`; `app/ponto/tests/test_dedup_retratada_c2.py:288`)

## 15/11 02:17 · sessao 1964f311ddf6 · E3-RESTO-R3 · leitura · origem=Code

A triagem separa **três falhas da Folha por desconhecimento antes ocultado**, **três C11 por uma nova porta administrativa com regras escritas conflitantes** e **duas da pauta cujo pré-filtro de fio impede chegar à prova pretendida**. Nos dois casos da pauta, o traceback não informa qual fio final excluiu D1; esse detalhe permanece **[nao verificado]** (`app/chamados/services/tarefa_do_dia.py:700`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:385`; `app/ponto/management/commands/pautas_discordantes.py:71`).

**Cadeia compartilhada pela Folha.** As fixtures gravam DNA e veredito, mas não ata com lâmpadas; `ata` admite `NULL`. O leitor transforma essa ausência em `{}`, e a tarefa responde `nao_sei=True`, razão `ata_sem_lampadas`. Esse julgamento já existia; o trecho determinante do diff é publicar o envelope desconhecido mesmo sem rota, em vez de devolver `None`. A Folha passa a lê-lo como `nao_sei`, grupo E (`app/folha/tests/test_haiku_fora_da_folha.py:53`; `app/folha/tests/test_haiku_fora_da_folha.py:425`; `app/escala/models.py:1778`; `app/escala/services/leitor_celula.py:379`; `app/chamados/services/tarefa_do_dia.py:238`; `app/chamados/services/tarefa_do_dia.py:700`; `app/folha/fora_da_folha.py:87`; `app/folha/fora_da_folha.py:109`).

1. **`test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA` — Folha.**

   **Cadeia até a asserção:** célula sem ata → tarefa desconhecida → novo envelope sem rota → `_fio_da_porta()` devolve `nao_sei` → `o_que_segura()` publica esse fio → linha 83 exige `nada`. O patch de oferta `False` impede Resolver dia, mas não fornece as lâmpadas ausentes nem limpa o desconhecimento (`app/folha/tests/test_haiku_fora_da_folha.py:53`; `app/folha/tests/test_haiku_fora_da_folha.py:75`; `app/chamados/services/tarefa_do_dia.py:238`; `app/chamados/services/tarefa_do_dia.py:609`; `app/folha/fora_da_folha.py:260`; `app/folha/fora_da_folha.py:272`; `app/folha/tests/test_haiku_fora_da_folha.py:83`).

   **Regra versus expectativa:** o teste escreve “LU-03/M4: sem pergunta, chamado E oferta, o fio é nada”; a implementação escreve “None = ausência conhecida de porta” e preserva desconhecimento conforme 2A. Essas regras podem coexistir: falta à fixture estabelecer uma **negativa conhecida**, além de ausência de oferta (`app/folha/tests/test_haiku_fora_da_folha.py:83`; `app/chamados/services/tarefa_do_dia.py:697`; `app/chamados/tests/test_e3_r3_c6_sem_banco.py:6`).

   **Fixture e cura:** ainda monta um dia acusador, mas seu início é desconhecido, não `nada`. **Cura no teste/fixture:** fornecer leitura lavrada suficiente para provar ausência conhecida de porta; conservar separadamente um caso sem lâmpadas que espere `nao_sei` (`app/folha/tests/test_haiku_fora_da_folha.py:55`; `app/chamados/services/tarefa_do_dia.py:238`; `app/chamados/services/tarefa_do_dia.py:697`).

2. **`test_GOLDEN_quantos_bloqueados_dependem_SO_do_admin_na_competencia` — Folha.**

   **Cadeia até a asserção:** mesma ausência de ata → fio `nao_sei` → grupo E → `_esperado_pela_ferramenta()` preserva E → `_esperado_por_dono()` aplica `DONO_DO_FIO` → `sem_leitura_sistema`, deixando `sem_acao_disponivel=0`, contra o `1` da linha 517 (`app/folha/tests/test_haiku_fora_da_folha.py:425`; `app/folha/fora_da_folha.py:203`; `app/folha/fora_da_folha.py:215`; `app/folha/tests/test_haiku_fora_da_folha.py:483`; `app/folha/tests/test_haiku_fora_da_folha.py:497`; `app/folha/retrato_folha.py:193`; `app/folha/tests/test_haiku_fora_da_folha.py:517`).

   **Regra versus expectativa:** a golden já incorpora LU-05 e já traduz o grupo E. A igualdade ferramenta/tela é verificada **antes** da constante que falha; portanto, este traceback aponta a premissa “furo sem fio = um balde próprio”, não uma divergência demonstrada entre ferramenta e tela. LU-05 separa `ninguem`; 2A separa desconhecimento (`app/folha/tests/test_haiku_fora_da_folha.py:485`; `app/folha/tests/test_haiku_fora_da_folha.py:511`; `app/folha/tests/test_haiku_fora_da_folha.py:515`; `app/docs/CORTES.md:437`; `app/chamados/tests/test_e3_r3_c6_sem_banco.py:6`).

   **Fixture e cura:** preencher a competência inteira resolveu a ausência de células, mas não a ausência de leitura das lâmpadas. **Cura no teste/fixture**, preservando a golden positiva de `ninguem → sem_acao_disponivel`; simplesmente mudar o esperado para E substituiria o caso que ela pretende provar (`app/folha/tests/test_haiku_fora_da_folha.py:415`; `app/folha/tests/test_haiku_fora_da_folha.py:425`; `app/folha/tests/test_haiku_fora_da_folha.py:521`).

3. **`test_MORDE_a_GOLDEN_o_dono_ANDA_quando_o_fio_do_dia_muda` — Folha.**

   **Cadeia até a asserção:** mesma fixture → grupo E → `montar_linhas()` grava o fio correspondente em `dono` → `_fio_na_tela()` conta `nao_sei` → linha 546 exige um `ninguem`. A falha acontece antes da criação do chamado que exercitaria a transição (`app/folha/retrato_folha.py:179`; `app/folha/retrato_folha.py:1228`; `app/folha/tests/test_haiku_fora_da_folha.py:452`; `app/folha/tests/test_haiku_fora_da_folha.py:546`; `app/folha/tests/test_haiku_fora_da_folha.py:551`).

   **Regra versus expectativa:** o teste descreve `nada → chamado → admin`, mas inicia atualmente em `nao_sei`. Também conserva a frase histórica “vivo na fila TEM porta”, enquanto o código novo exclui residuais `cobrar`, histórico/arquivado e `None`; a transição posterior precisa respeitar esse recorte, mas não foi alcançada no dado fornecido **[nao verificado]** (`app/folha/tests/test_haiku_fora_da_folha.py:528`; `app/folha/tests/test_haiku_fora_da_folha.py:536`; `app/chamados/services/tarefa_do_dia.py:655`).

   **Fixture e cura:** **cura no teste/fixture**, reconstruindo o início conhecido sem porta e conservando a prova de movimento conjunto; aceitar o início desconhecido mudaria a transição descrita (`app/folha/tests/test_haiku_fora_da_folha.py:528`; `app/folha/tests/test_haiku_fora_da_folha.py:546`).

A publicação do envelope e os mapas E estão atribuídos textualmente à **c5**; a c6 altera o agrupamento misto, que não explica estes três casos com um único dia acusador. O commit exato de introdução é **[nao verificado]** (`app/chamados/tests/e3_r3_c5_entrega.txt:54`; `app/chamados/tests/e3_r3_c5_entrega.txt:56`; `app/chamados/tests/e3_r3_c6_entrega.txt:16`).

**Cadeia compartilhada pelos três C11.** `_beco()` cria âncora `resolvido`, pergunta respondida e registra a resposta na sequência E1. `_pela_decisao()` transforma a resposta textual em `validar/admin`; a ação tem `porta=None` porque a âncora está fora da fila. O hunk novo aceita a existência dessa âncora para `admin_alcancavel`, publica `admin/porta_acao/validar`, e a projeção entrega o envelope. Os três testes param na exigência `(False, None, None)` da linha 385 (`app/ponto/tests/test_cxr_tela_le_o_motor.py:359`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:366`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:372`; `app/chamados/services/tarefa_do_dia.py:151`; `app/chamados/services/tarefa_do_dia.py:160`; `app/chamados/services/tarefa_do_dia.py:648`; `app/chamados/services/tarefa_do_dia.py:673`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:385`).

4. **`test_MORDE_C11_le_a_lista_lavrada_e_nao_chama_o_motor`.**

   A cadeia acima interrompe o teste **antes de lavrar** `[BECO]`. Sua prova pretendida continua sendo C11 com dono `CADASTRO`, nenhuma chamada ao motor e juiz consultado com lista positiva; o comportamento dessas etapas nesta execução é **[nao verificado]** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:409`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:410`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:413`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:415`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:417`).

5. **`test_MORDE_C11_lavra_que_cobre_sem_dia_e_lista_vazia_sem_motor`.**

   A mesma guarda interrompe o teste **antes da lavra vazia**. A regra escrita distingue cobertura sem dias de ausência de resposta, esperando `ESTRUTURA` sem motor; essa prova não foi alcançada **[nao verificado]** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:420`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:422`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:423`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:425`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:426`).

6. **`test_MORDE_C11_fora_da_lavra_fica_sem_resposta_e_nao_chama_o_motor`.**

   A mesma guarda interrompe o teste antes da consulta sem lavra. A expectativa posterior é dono `None`, sem motor; o traceback não verifica nem refuta esse comportamento **[nao verificado]** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:429`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:430`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:431`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:432`).

**Regras, fixture e cura dos três C11: PERGUNTA.** A fixture segue a leitura antiga de que resposta sob âncora terminal fica sem porta, inclusive verificando o C17. Seu comentário “espera o SISTEMA” também diverge diretamente do corpo atual, que manda validar pelo ADMIN (`app/ponto/tests/test_cxr_tela_le_o_motor.py:365`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:376`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:382`; `app/chamados/services/tarefa_do_dia.py:160`).

A leitura nova segue LU-03, “ação direta do admin **com tela** = porta”, interpretando âncora existente fora da fila como suficiente; o teste M2 fixa essa interpretação. Entretanto, LU-04 escreve que pergunta com “porta None” não dá dono, e a costura conserva a regra “âncora terminal é fio morto — o admin não a vê”. Falta reconciliar o significado de “porta None” e o alcance de validar sob terminal (`app/docs/CORTES.md:435`; `app/docs/CORTES.md:436`; `app/chamados/services/tarefa_do_dia.py:622`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:540`; `app/chamados/services/fio.py:789`).

Há ainda um limite concreto da suposição “com tela”: esta fixture cria disputa **sem chamado**, enquanto a montagem do modal seleciona disputas com `chamado__isnull=False` e busca perguntas dessas disputas. A existência de `chamado_destino` não comprova, sozinha, o botão de validar dessa pergunta; a renderização efetiva é **[nao verificado]** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:363`; `app/chamados/services/fio.py:211`; `app/chamados/services/fio.py:247`; `app/chamados/services/fio.py:255`; `app/chamados/views.py:889`).

Assim, a fixture **não monta SEM PORTA segundo o juiz atual**, e o beco real a excluiria. Se essa interpretação de LU-03 prevalecer, cura na fixture para conservar um C11 positivo; se prevalecer a restrição terminal/sem tela, cura no publicador de alcance administrativo. Trocar somente a asserção para aceitar a porta deixaria de exercitar o beco (`app/chamados/services/tarefa_do_dia.py:648`; `app/escala/services/cadastro_realidade.py:472`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:412`).

Essa interpretação já aparece na entrega LU2 anterior à LU3; atribuir sua introdução especificamente a c3, c4, c5 ou c6 é **[nao verificado]** (`app/chamados/tests/e3_lu2_entrega.txt:20`; `app/chamados/tests/e3_lu3_entrega.txt:53`).

7. **`test_MORDE_o_universo_le_a_celula_e_o_FIO` — pauta.**

   **Cadeia:** a fixture cria D1 discordante sem ata. Isso oferece o caminho trabalho sem lâmpadas → `nao_sei` → envelope novo → `_fio_da_porta()` → exclusão em `dias_sem_fio()`. O leitor só acrescenta ao resultado dias cujo fio é `nada`; a asserção recebe `[]`, esperando `[D1]`. Porém a consulta real ao Resolver também participa: o fio final dessa execução não está no traceback **[nao verificado]** (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:39`; `app/chamados/services/tarefa_do_dia.py:238`; `app/chamados/services/tarefa_do_dia.py:597`; `app/chamados/services/tarefa_do_dia.py:700`; `app/folha/fora_da_folha.py:87`; `app/ponto/services/pautas_dos_discordantes.py:90`; `app/ponto/services/pautas_dos_discordantes.py:95`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:68`).

   **Regra versus expectativa:** O320 exige discordante **sem fio**, e o próprio teste define isso como `fio == 'nada'`. K3 interpreta 2A + L-099 como exclusão de desconhecimento da pauta de DP, com registro separado em `sem_leitura`. Portanto, a expectativa `[D1]` continua válida para um dia conhecido sem fio; a fixture deixou essa premissa sem estabelecer (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:1`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:96`; `app/ponto/services/pautas_dos_discordantes.py:60`).

   **Cura:** **teste/fixture**, fornecendo leitura conhecida e controlando a oferta. A publicação desconhecida é c5; o ramo explícito da pauta é **c6 K3**. Atribuir o `[]` exclusivamente a K3, sem o fio final, permanece **[nao verificado]** (`app/chamados/tests/e3_r3_c5_entrega.txt:54`; `app/chamados/tests/e3_r3_c6_entrega.txt:20`).

8. **`test_MORDE_contagem_IMPAR_e_BATIDA_e_o_motor_NAO_roda` — pauta.**

   **Cadeia comprovável:** uma batida é criada; seu signal chama o cartório; a fixture restaura **somente o veredito**. `apurar()` consulta primeiro `dias_sem_fio()` e pode retornar `[]` antes de montar os insumos ou perguntar a paridade. O traceback não mostra se D1 foi excluído por `nao_sei`, por porta administrativa ou por outro fio: origem interna exata **[nao verificado]** (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:202`; `app/ponto/signals.py:132`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:206`; `app/ponto/management/commands/pautas_discordantes.py:71`; `app/ponto/management/commands/pautas_discordantes.py:85`; `app/ponto/management/commands/pautas_discordantes.py:100`).

   **Fixture:** monta uma contagem ímpar, mas não fixa a premissa adicional de dia conhecido sem fio. Deliberadamente não cria vínculo real; o mock de cadastro usado na classificação do dono não fornece vínculo à carga da tarefa. Sem vínculo, o teto dos marcos pode ficar vazio; lâmpada apagada sem teto produz desconhecimento. A oferta real de Resolver também não está controlada (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:212`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:216`; `app/ponto/turnos.py:3065`; `app/ponto/services/cartorio.py:890`; `app/chamados/services/tarefa_do_dia.py:300`; `app/chamados/services/tarefa_do_dia.py:605`).

   **Regra e cura:** BUG-144 continua exigindo classificação da contagem ímpar pelo juiz, e o comando conserva essa consulta depois do filtro de universo. Este failure não demonstra regressão na paridade. **Cura no teste/fixture:** estabelecer explicitamente entrada no universo sem fio, preservando a prova positiva `BATIDA` e motor não chamado; não trocar o esperado para lista vazia (`app/ponto/management/commands/e6_oraculo.py:63`; `app/ponto/management/commands/e6_oraculo.py:106`; `app/ponto/management/commands/pautas_discordantes.py:87`; `app/ponto/tests/test_o320_pauta_dos_discordantes.py:229`).

O agrupamento para encaminhamento fica:

| Causa comum | Casos | Encaminhamento |
|---|---|---|
| Fixture presume ausência conhecida; c5 passa a expor desconhecimento já julgado | Folha ×3 | **Teste/fixture**, conservando também um irmão desconhecido (`app/chamados/services/tarefa_do_dia.py:238`; `app/chamados/tests/e3_r3_c5_entrega.txt:54`). |
| Âncora fora da fila passa a bastar para porta administrativa, incluindo a terminal da fixture | C11 ×3 | **PERGUNTA** entre LU-03/com tela e restrições escritas de LU-04/âncora terminal; depois, código ou fixture conforme a resposta (`app/docs/CORTES.md:435`; `app/docs/CORTES.md:436`; `app/chamados/services/fio.py:789`). |
| Fixture não estabelece o novo pré-requisito de fio conhecido `nada` | Pauta ×2 | **Teste/fixture**; o ramo final que excluiu cada D1 é **[nao verificado]** no dado fornecido (`app/ponto/services/pautas_dos_discordantes.py:87`; `app/ponto/services/pautas_dos_discordantes.py:95`; `app/ponto/management/commands/pautas_discordantes.py:71`). |

## 15/11 02:17 · sessao a4f7fab07c05 · - · - · origem=Ronald (tmux)

Não achei defeito atual na propagação da marca pelos caminhos inspecionados: os dois `create` recebem `True` antes do retorno; os dois ramos de reutilização retornam objetos consultados sem atribuição; as três cascas passam diretamente o objeto ao helper. (`app/ponto/registro_batida.py:113`; `app/ponto/registro_batida.py:120`; `app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:155`; `app/ponto/registro_batida.py:158`; `app/ponto/registro_batida.py:161`; `app/api/views.py:654`; `app/api/views.py:693`; `app/api/views_core.py:645`; `app/api/views_core.py:684`; `app/ponto/services/triagem_batida.py:700`; `app/ponto/services/triagem_batida.py:723`)

| Item | Conferência |
|---|---|
| Escritores/leitores | Escritores de produção: `registrar_batida`, dois sítios; leitor direto único: `resposta_se_reutilizada`. As cascas leem o dicionário retornado. (`app/ponto/registro_batida.py:155`; `app/ponto/registro_batida.py:161`; `app/ponto/services/resposta_batida.py:14`; `app/api/views.py:693`; `app/api/views_core.py:684`; `app/ponto/services/triagem_batida.py:723`) |
| Substituição/perda entre criação e casca | Não existe cópia, reconsulta ou `refresh_from_db` nesses segmentos; fechamento recebe colaborador/instante, sem receber a batida. (`app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:177`; `app/api/views.py:669`; `app/api/views_core.py:660`; `app/ponto/services/triagem_batida.py:713`) |
| Modelo/receivers | `Batida` herda diretamente `models.Model`, sem `save`, manager ou `from_db` próprios nessa classe. Receivers chamam serviços usando `instance`, sem substituir o retorno da porta. SSE usa `created`, não a marca; seu payload enumera campos explicitamente. (`app/ponto/models.py:8`; `app/ponto/models.py:111`; `app/ponto/signals.py:34`; `app/ponto/signals.py:40`; `app/ponto/signals.py:132`; `app/ponto/signals.py:207`; `app/ponto/realtime.py:32`) |
| Segundo escritor pela porta | Os únicos `Batida.objects.create` de produção encontrados são os dois ramos marcados. Reutilização não executa esses ramos. (`app/ponto/registro_batida.py:115`; `app/ponto/registro_batida.py:122`; `app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:158`) |
| Outros consumidores | Leem campos explícitos, identidade, `None`, justificativa ou vinculam FK; não enumeram `vars`/`__dict__` da batida. `anotar` lê apenas os campos fornecidos, sob allowlist. (`app/ponto/services/justificativa.py:195`; `app/ponto/services/justificativa.py:197`; `app/ponto/services/justificativa.py:202`; `app/ponto/services/justificativa.py:204`; `app/ponto/services/justificativa.py:370`; `app/ponto/services/justificativa.py:381`; `app/ponto/views.py:1825`; `app/ponto/views.py:1829`; `app/ponto/views.py:1834`; `app/ponto/services/veredito_celula.py:325`; `app/chamados/services/materializacao.py:952`; `app/chamados/services/materializacao.py:959`; `app/chamados/services/materializacao.py:1041`; `app/chamados/services/regularizacao_ext.py:95`; `app/chamados/services/regularizacao_ext.py:106`; `app/chamados/services/acoes_chamado.py:54`; `app/chamados/services/acoes_chamado.py:67`; `app/ponto/registro_batida.py:243`; `app/ponto/registro_batida.py:246`) |

Censo AST dos **16 sítios diretos fora dos testes**: [nao verificado]

| Chamador | Sítios |
|---|---|
| Três cascas | `app/api/views.py:654`; `app/api/views_core.py:645`; `app/ponto/services/triagem_batida.py:700` |
| Justificativas | `app/ponto/services/justificativa.py:188`; `app/ponto/services/justificativa.py:361`; `app/ponto/views.py:1818` |
| Veredito | `app/ponto/services/veredito_celula.py:323` |
| Materialização | `app/chamados/services/materializacao.py:933`; `app/chamados/services/materializacao.py:1013` |
| Regularização externa | `app/chamados/services/regularizacao_ext.py:78` |
| Exceção autorizada | `app/chamados/services/acoes_chamado.py:44` |
| Comando de ensaio | `app/ponto/management/commands/testar_pwa.py:184`; `app/ponto/management/commands/testar_pwa.py:198`; `app/ponto/management/commands/testar_pwa.py:219`; `app/ponto/management/commands/testar_pwa.py:228`; `app/ponto/management/commands/testar_pwa.py:229` |

O comando descarta os três primeiros retornos e compara `None`/PK nos dois últimos; a entrega cita `:230` como segunda chamada, mas ali está a comparação: a chamada real é `:229`. (`app/ponto/management/commands/testar_pwa.py:184`; `app/ponto/management/commands/testar_pwa.py:198`; `app/ponto/management/commands/testar_pwa.py:219`; `app/ponto/management/commands/testar_pwa.py:229`; `app/ponto/management/commands/testar_pwa.py:230`)

A corrida específica de V2 fica causalmente resolvida pelo código: concorrente confirmado antes da consulta gera retorno por reutilização sem marca; criação local recebe marca. A colisão posterior às consultas ainda escapa como exceção, sem alcançar helper, porque não há captura de `IntegrityError` no corpo da porta. (`app/ponto/registro_batida.py:113`; `app/ponto/registro_batida.py:120`; `app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:158`; `app/ponto/models.py:106`)

Execução SQL/Django, chamadas dinâmicas, comportamento de bibliotecas externas e frequência em produção: **[nao verificado]**.

## 15/11 02:16 · sessao 5ce9eaee8b60 · - · - · origem=Ronald (tmux)

**Pauta 1 — `test_MORDE_o_universo_le_a_celula_e_o_FIO`**

- A fixture cria D1 `discordante`, `trabalha=True`, DNA de 07h–19h, mas não fornece `ata`; o campo aceita `NULL` (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:39`, `app/escala/models.py:1778`). O leitor transforma ata ausente em `{}`, mantém tipo `trabalho` e obtém zero lâmpadas (`app/escala/services/leitor_celula.py:379`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:413`).
- Sem instâncias de lâmpadas, a tarefa escreve `nao_sei=True`, razão `ata_sem_lampadas`; essa ignorância não nasceu no diff fornecido, mas o diff passa a publicá-la mesmo sem rota (`app/chamados/services/tarefa_do_dia.py:232`, `app/chamados/services/tarefa_do_dia.py:238`, `app/chamados/services/tarefa_do_dia.py:700`). A Folha lê esse envelope como fio `nao_sei`; a pauta exclui esse fio, resultando em `[]` na asserção que exige `[D1]` (`app/folha/fora_da_folha.py:87`, `app/ponto/services/pautas_dos_discordantes.py:90`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:68`). **O valor final interno da tarefa após a consulta real ao Resolver dia não aparece no traceback; atribuir toda a execução exclusivamente a esse caminho é [não verificado].**
- O corte O320 manda enviar os dias discordantes **sem fio** à pauta de DP; o próprio teste explica que SEM FIO é `fio == 'nada'`, e não ausência de qualquer pergunta/chamado (`app/ponto/services/pautas_dos_discordantes.py:4`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:96`). A regra nova separa desconhecimento de ausência conhecida de porta e impede pauta de DP para `nao_sei`, interpretado como ESTRUTURA (`app/chamados/services/tarefa_do_dia.py:697`, `app/ponto/services/pautas_dos_discordantes.py:60`).
- Portanto, o nome continua medindo duas autoridades, mas a fixture não estabelece a premissa **SEM FIO conhecido**: estabelece trabalho sem ata de lâmpadas (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:39`, `app/chamados/services/tarefa_do_dia.py:238`). Cura proposta: **teste/fixture**, lavrando um caso conhecido sem porta e controlando a oferta de Resolver; se a intenção for desconhecimento, exigir exclusão e testemunho em `sem_leitura` (`app/ponto/services/pautas_dos_discordantes.py:90`).
- Volta identificável: publicação do envelope em **c5**, documentada na entrega; ramo explícito da pauta em **c6 K3** (`app/chamados/tests/e3_r3_c5_entrega.txt:54`, `app/chamados/tests/e3_r3_c6_entrega.txt:20`).

**Pauta 2 — `test_MORDE_contagem_IMPAR_e_BATIDA_e_o_motor_NAO_roda`**

- A fixture cria uma célula discordante e uma única batida; depois restaura **somente** `veredito='discordante'` por `update()` (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:182`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:202`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:206`). O save da batida chama o cartório e pode alterar ata/status antes desse update (`app/ponto/signals.py:132`, `app/ponto/signals.py:85`).
- A fixture deliberadamente não cria vínculo real e simula cadastro apenas por mock de `cadastro_do_dia_carregado`; esse mock não fornece vínculo à carga da tarefa (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:212`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:216`, `app/ponto/turnos.py:3053`). Sem vínculo, o teto dos marcos retorna `[]`; uma lâmpada apagada fica sem teto e a tarefa marca desconhecimento (`app/ponto/turnos.py:3065`, `app/ponto/services/cartorio.py:890`, `app/chamados/services/tarefa_do_dia.py:123`, `app/chamados/services/tarefa_do_dia.py:300`).
- Outra mudança do diff permite que a oferta real de Resolver dia publique `admin/porta_acao`; a fixture não controla essa oferta (`app/chamados/services/tarefa_do_dia.py:597`, `app/chamados/services/tarefa_do_dia.py:605`, `app/chamados/services/tarefa_do_dia.py:674`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:217`). **O traceback mostra somente `[]`; não mostra se D1 saiu por `nao_sei`, por `admin` ou por outro fio: origem interna exata [não verificado].**
- A cadeia comprovável termina antes do juiz da paridade: `apurar()` primeiro chama `dias_sem_fio()`, que só inclui fio `nada`; com lista vazia, retorna `[]` imediatamente. A montagem de `InsumosDoDono`, o pré-teste do motor e `_ins.dono()` ficam depois desse retorno (`app/ponto/services/pautas_dos_discordantes.py:87`, `app/ponto/services/pautas_dos_discordantes.py:95`, `app/ponto/management/commands/pautas_discordantes.py:71`, `app/ponto/management/commands/pautas_discordantes.py:85`, `app/ponto/management/commands/pautas_discordantes.py:100`, `app/ponto/management/commands/pautas_discordantes.py:112`).
- A regra BUG-144 continua escrita e implementada no juiz; a falha fornecida não demonstra violação dela, porque a asserção recebe um universo vazio antes dessa pergunta (`app/ponto/management/commands/e6_oraculo.py:62`, `app/ponto/tests/test_o320_pauta_dos_discordantes.py:229`, `app/ponto/management/commands/pautas_discordantes.py:72`). A regra nova LU-03 considera ação direta administrativa com tela como porta; K3 exclui desconhecimento do universo de pauta (`app/docs/CORTES.md:435`, `app/ponto/services/pautas_dos_discordantes.py:60`).
- Cura proposta: **teste/fixture**, separar a montagem da paridade do pré-filtro de alcance — ou fornecer explicitamente um dia conhecido sem fio. A identificação do ramo concreto que excluiu D1 permanece **[não verificado]**, portanto não agrupar automaticamente esta falha com “ata vazia” (`app/ponto/tests/test_o320_pauta_dos_discordantes.py:208`, `app/ponto/management/commands/pautas_discordantes.py:71`).

**Agrupamento:** a primeira integra a causa “ausência de lâmpadas antes mascarada como ausência de porta”; a segunda integra “fixture do teste de paridade não fixa mais o universo sem fio”. Ambas indicam cura de fixture, preservando testes próprios para `nao_sei` e para porta administrativa; não há, nestes dois tracebacks, prova de regressão no juiz BUG-144 (`app/chamados/services/tarefa_do_dia.py:238`, `app/chamados/services/tarefa_do_dia.py:700`, `app/ponto/management/commands/pautas_discordantes.py:71`, `app/ponto/management/commands/e6_oraculo.py:62`).

## 15/11 02:15 · sessao 8be6effeb4cc · - · - · origem=Ronald (tmux)

Os três C11 têm a **mesma falha de preparação**: `_beco()` exige `(False, None, None)` na linha 385; nenhuma das três execuções chega às asserções sobre lavra ou motor (`app/ponto/tests/test_cxr_tela_le_o_motor.py:385`, `:409`, `:422`, `:429`).

**Cadeia comum escritor → leitor → asserção**

- A fixture cria chamado `resolvido`, pergunta com `resposta_colab='17:00'` e `respondida_em` anterior ao instante da consulta; depois registra a conservação dessa resposta na E1 (`app/ponto/tests/test_cxr_tela_le_o_motor.py:359`, `:366`, `:372`).
- `linhas_da_projecao_sem_linha` produz ato `RESPOSTA` com o texto da projeção; `registrar` grava a sequência (`app/chamados/models.py:4074`, `:4086`, `:4096`).
- A carga lê decisões até `agora` e as ordena por ato; `_pela_decisao` transforma a última resposta textual em `aguarda_validacao`, ação `validar/admin` (`app/chamados/services/tarefa_do_dia.py:419`, `:421`, `:157`, `:160`).
- A âncora sai de `chamado_que_lista`: prevalece o `chamado_destino_id`; a ação recebe `porta=None` quando essa âncora está fora da fila (`app/chamados/services/fio.py:117`; `app/chamados/services/tarefa_do_dia.py:150`, `:151`).
- **Trecho causal do diff:** `admin_alcancavel` agora aceita `VALIDAR` com âncora existente mesmo fora da fila; escolhe `fio='admin'`, publica `rota='porta_acao'`, `acao='validar'`, e `porta_da_tarefa` devolve o envelope (`app/chamados/services/tarefa_do_dia.py:646`, `:648`, `:659`, `:673`, `:684`, `:700`).
- Isso explica o valor raspado `(False, 'porta_acao', {...fio:'admin', acao:'validar'...})` diante da expectativa de `_beco()` (`app/ponto/tests/test_cxr_tela_le_o_motor.py:385`; dado fornecido no traceback dessa linha).

**Cada teste, separadamente** [nao verificado]

| Teste | O que pretendia provar depois de `_beco()` | O que a falha prova | [nao verificado]
|---|---|---|
| `test_MORDE_C11_le_a_lista_lavrada_e_nao_chama_o_motor` | Dia coberto e presente na lavra → C11 com `CADASTRO`, zero chamadas ao motor e juiz consultado com lista positiva (`app/ponto/tests/test_cxr_tela_le_o_motor.py:408`, `:410`, `:413`, `:415`, `:417`). | Falha antes de produzir a lavra ou consultar a lista; comportamento posterior nesta execução **[nao verificado]** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:409`, `:385`). |
| `test_MORDE_C11_lavra_que_cobre_sem_dia_e_lista_vazia_sem_motor` | Cobertura com conjunto vazio → `ESTRUTURA`, zero chamadas ao motor (`app/ponto/tests/test_cxr_tela_le_o_motor.py:419`, `:420`, `:423`, `:425`, `:426`). | Falha antes da lavra vazia; comportamento posterior nesta execução **[nao verificado]** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:422`, `:385`). |
| `test_MORDE_C11_fora_da_lavra_fica_sem_resposta_e_nao_chama_o_motor` | Sem lavra → dono `None`, zero chamadas ao motor (`app/ponto/tests/test_cxr_tela_le_o_motor.py:428`, `:430`, `:431`, `:432`). | Falha antes da consulta à lista; comportamento posterior nesta execução **[nao verificado]** (`app/ponto/tests/test_cxr_tela_le_o_motor.py:429`, `:385`). |

O contrato de lavra continua escrito no código: `cxr_lavrado` distingue ausência de cobertura (`None`) de cobertura sem dias (`set()`); `InsumosDoDono` com `sem_motor=True` conserva a ausência de resposta e impede fallback para motor (`app/ponto/services/cxr_na_lista.py:165`, `:171`, `:182`; `app/ponto/management/commands/e6_oraculo.py:142`, `:166`, `:189`, `:193`). Nada no dado fornecido demonstra regressão nesse contrato; os tracebacks param antes dele (`app/ponto/tests/test_cxr_tela_le_o_motor.py:385`).

**Regra velha, regra nova e conflito escrito**

1. A fixture segue a leitura antiga de porta: pergunta respondida sob âncora terminal fica sem porta; ainda verifica o C17 antigo como `None` (`app/ponto/tests/test_cxr_tela_le_o_motor.py:364`, `:376`). O C17 continua documentado como “chamado da célula em `qs_na_fila` OU pergunta viva no app com âncora na fila” (`app/chamados/juizes.py:1667`).
2. O comentário da fixture ainda diz que resposta com linha espera **SISTEMA**; o corpo atual diz que última `RESPOSTA` textual espera **ADMIN validar**. Essa discordância é direta, independentemente da discussão sobre alcance (`app/ponto/tests/test_cxr_tela_le_o_motor.py:382`; `app/chamados/services/tarefa_do_dia.py:157`, `:160`).
3. A implementação nova segue LU-03: “ação direta do admin com tela = porta”; seu docstring interpreta âncora existente, inclusive fora da fila, como tela de validar (`app/docs/CORTES.md:435`; `app/chamados/services/tarefa_do_dia.py:622`). O teste M2 exige exatamente ação `validar` com `porta=None`, mapa de âncora e resultado `admin/porta_acao` (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:540`).
4. Há texto contemporâneo contrário: LU-04 diz que pergunta em marco dispensado, fora de marco **ou porta None** não dá dono (`app/docs/CORTES.md:436`). Também permanece a regra escrita “âncora terminal é fio morto — o admin não a vê” (`app/chamados/services/fio.py:785`, `:789`). Não achei nesses sítios declaração que revogue essas restrições para a interpretação nova de LU-03 **[nao verificado]**.
5. O novo teste “caminho terminal de antes segue” não resolve essa composição: força tarefa `fio='nada'` e `porta_da_tarefa=lambda t:None`; testa o beco depois de supor ausência de porta (`app/chamados/tests/test_e3_r3_c6_sem_banco.py:283`, `:285`, `:323`).

**A fixture ainda monta o caso do nome?**

Pelo juiz publicado no hash, **não monta o beco SEM PORTA**: sua resposta textual conhecida e âncora existente passam a produzir `validar/admin/porta_acao`; o beco real rejeita qualquer envelope não `None` (`app/ponto/tests/test_cxr_tela_le_o_motor.py:366`, `:372`, `:385`; `app/chamados/services/tarefa_do_dia.py:648`, `:673`; `app/escala/services/cadastro_realidade.py:472`).

Contudo, existência de âncora **não comprova a tela desta fixture**: ela cria `DisputaSupervisao` sem `chamado`; esse campo admite `NULL`; a montagem do modal busca disputas com `chamado__isnull=False` e carrega perguntas somente das disputas selecionadas (`app/ponto/tests/test_cxr_tela_le_o_motor.py:363`; `app/chamados/models.py:1796`; `app/chamados/services/fio.py:211`, `:214`, `:247`, `:255`). A rota do modal usa essa montagem (`app/chamados/views.py:889`). A tela efetivamente renderizada não foi executada **[nao verificado]**.

**Agrupamento e cura**

Os três pertencem à causa comum **“a fixture antes sem porta ganhou porta administrativa pela interpretação nova de LU-03”** (`app/chamados/services/tarefa_do_dia.py:646`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:385`). Classificação principal: **PERGUNTA**, pois LU-03/comentário da implementação e LU-04/regra de âncora terminal sustentam leituras diferentes (`app/docs/CORTES.md:435`, `:436`; `app/chamados/services/tarefa_do_dia.py:622`; `app/chamados/services/fio.py:789`).

- Se âncora terminal existente garante tela de validar, a cura dos três selos é **na fixture do teste**, preservando a anti-vacuidade do C11 e suas três situações de lavra; trocar apenas a expectativa para aceitar porta faria o caso deixar de percorrer o beco (`app/escala/services/cadastro_realidade.py:472`; `app/ponto/tests/test_cxr_tela_le_o_motor.py:412`).
- Se prevalece “porta None/âncora terminal sem tela não dá dono”, a cura é **no código que publica alcance administrativo**, com prova da composição real, pois `admin_alcancavel` hoje aceita apenas a existência da âncora (`app/docs/CORTES.md:436`; `app/chamados/services/fio.py:789`; `app/chamados/services/tarefa_do_dia.py:648`).
- A volta exata c3/c4/c5/c6 que introduziu esse hunk é **[nao verificado]**; o comentário o vincula a LU-03/LU-07 (`app/chamados/services/tarefa_do_dia.py:620`).

## 15/11 02:14 · sessao 2418af21e5c7 · - · - · origem=Ronald (tmux)

Os três failures da Folha têm uma causa comum: a fixture cria `veredito='furo'` e DNA de marcos, mas não fornece `ata` com lâmpadas; o juiz distingue agora ausência **conhecida** de porta de ausência de leitura. A regra E2 já classifica trabalho com marcos vencidos e nenhuma instância como `nao_sei=True, razao='ata_sem_lampadas'`; o diff passa a publicar esse testemunho e a Folha passa a preservá-lo (`app/folha/tests/test_haiku_fora_da_folha.py:53`, `app/folha/tests/test_haiku_fora_da_folha.py:425`, `app/chamados/services/tarefa_do_dia.py:232`, `app/chamados/services/tarefa_do_dia.py:692`, `app/folha/fora_da_folha.py:84`).

**Cadeia comum, até o primeiro valor alterado.** `CelulaDia.ata` é nullable; o leitor usa `{}` quando falta ata, mantém o tipo trabalho de `trabalha=True`, e deriva as células exclusivamente da ata. Sem lâmpadas, nenhum marco com tipo/hora emerge para `instancias_do_turno`; `_tarefa_pura` responde desconhecimento. O patch de oferta `False` impede Resolver dia, mas não apaga esse desconhecimento (`app/escala/models.py:1778`, `app/escala/services/leitor_celula.py:379`, `app/escala/services/leitor_celula.py:397`, `app/escala/services/leitor_celula.py:413`, `app/escala/services/leitor_celula.py:114`, `app/chamados/services/tarefa_do_dia.py:106`, `app/chamados/services/tarefa_do_dia.py:232`, `app/chamados/services/tarefa_do_dia.py:609`).

O trecho novo determinante é `porta_da_tarefa`: antes o resultado sem rota desaparecia como `None`; agora `nao_sei=True` também publica envelope. `_fio_da_porta` interpreta envelope sem rota como `nao_sei`, e `_grupo` dá E quando não há dia com ação (`app/chamados/services/tarefa_do_dia.py:692`, `app/folha/fora_da_folha.py:84`, `app/folha/fora_da_folha.py:92`).

1. **`HaikuForaDaFolhaTest.test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA`.** O patch retorna oferta `False`; a célula tem DNA/veredito, sem ata. `o_que_segura` carrega tarefa, projeta porta, lê fio e grava o resultado em `_d['fio']`; a asserção exige `nada`, embora a cadeia entregue `nao_sei` (`app/folha/tests/test_haiku_fora_da_folha.py:53`, `app/folha/tests/test_haiku_fora_da_folha.py:75`, `app/folha/fora_da_folha.py:257`, `app/folha/fora_da_folha.py:260`, `app/folha/fora_da_folha.py:272`, `app/folha/tests/test_haiku_fora_da_folha.py:83`).

   A regra do teste é explícita: “LU-03/M4: sem pergunta, chamado E oferta, o fio e nada”. A regra da implementação também: “None = ausência conhecida de porta”; desconhecimento publica testemunho, conforme DESENHOS-DA-NOITE 2A. As duas regras não precisam conflitar: o teste deixou de montar uma negativa conhecida, porque suprimir oferta não fornece as lâmpadas que faltam (`app/folha/tests/test_haiku_fora_da_folha.py:83`, `app/chamados/services/tarefa_do_dia.py:697`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:5`, `app/chamados/services/tarefa_do_dia.py:238`).

2. **`GoldenDonoDoBloqueadoTest.test_GOLDEN_quantos_bloqueados_dependem_SO_do_admin_na_competencia`.** A fixture cobre toda a competência com células e deixa apenas um dia acusador, mas todas continuam sem ata/lâmpadas; seu patch de oferta também é `False`. `fora_da_folha` lê o fio `nao_sei` e conta grupo E; `_esperado_pela_ferramenta` preserva E como `nao_sei`; `_esperado_por_dono` aplica `DONO_DO_FIO`, que manda esse fio para `sem_leitura_sistema`. Por isso `sem_acao_disponivel` dá zero na asserção que pede um (`app/folha/tests/test_haiku_fora_da_folha.py:374`, `app/folha/tests/test_haiku_fora_da_folha.py:425`, `app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:215`, `app/folha/tests/test_haiku_fora_da_folha.py:483`, `app/folha/tests/test_haiku_fora_da_folha.py:497`, `app/folha/retrato_folha.py:193`, `app/folha/tests/test_haiku_fora_da_folha.py:517`).

   A comparação ferramenta/tela vem **antes** da asserção fixa que falha; o traceback fornecido não demonstra divergência entre esses dois caminhos. O teste já incorporou LU-05, que exige balde próprio para `ninguem`, e já incorporou E na tradução; o erro restante é presumir que sua fixture produz grupo D. A regra 2A exige separar desconhecimento, e a fixture produz justamente esse caso (`app/folha/tests/test_haiku_fora_da_folha.py:511`, `app/folha/tests/test_haiku_fora_da_folha.py:515`, `app/folha/tests/test_haiku_fora_da_folha.py:485`, `app/docs/CORTES.md:437`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:6`).

3. **`GoldenDonoDoBloqueadoTest.test_MORDE_a_GOLDEN_o_dono_ANDA_quando_o_fio_do_dia_muda`.** A mesma fixture vira E. Na cadeia da tela, `montar_linhas` chama `fora_da_folha`, `_fio_cru` traduz a letra E por `FIO_DA_LETRA`, e grava `dono='nao_sei'`; `_fio_na_tela` conta o fio gravado. A asserção inicial exige um `ninguem`, antes de criar o chamado que exercitaria a mudança (`app/folha/retrato_folha.py:1192`, `app/folha/retrato_folha.py:1228`, `app/folha/retrato_folha.py:378`, `app/folha/retrato_folha.py:179`, `app/folha/tests/test_haiku_fora_da_folha.py:452`, `app/folha/tests/test_haiku_fora_da_folha.py:546`, `app/folha/tests/test_haiku_fora_da_folha.py:551`).

   O nome e docstring descrevem uma transição `nada → chamado → admin`; a fixture atual inicia em `nao_sei`. O teste conserva escrita histórica “vivo na fila TEM porta”, enquanto LU-02 exclui residuais cobrar/histórico/arquivado/None; isso merece revisar o verbo efetivo do chamado depois de reparar o início, mas esse trecho posterior **não foi alcançado** no traceback fornecido (`app/folha/tests/test_haiku_fora_da_folha.py:528`, `app/folha/tests/test_haiku_fora_da_folha.py:536`, `app/chamados/services/tarefa_do_dia.py:655`, `app/folha/tests/test_haiku_fora_da_folha.py:546`). O comportamento posterior desse chamado smoke é [nao verificado].

**Volta e cura.** A entrega c5 declara expressamente a publicação do envelope desconhecido e o grupo E/mapas da Folha. A entrega c6 declara ajuste do grupo misto; isso não explica estes casos de um único dia acusador. Identificar commits exatos sem `.git` é [nao verificado] (`app/chamados/tests/e3_r3_c5_entrega.txt:1`, `app/chamados/tests/e3_r3_c5_entrega.txt:54`, `app/chamados/tests/e3_r3_c5_entrega.txt:56`, `app/chamados/tests/e3_r3_c6_entrega.txt:16`).

**Classificação: cura no teste/fixture**, preservando a prova de ausência conhecida de porta e seu caso irmão com oferta. A fixture precisa fornecer os insumos lavrados suficientes para `nao_sei=False`; trocar simplesmente `nada` por `nao_sei` eliminaria a prova nominal do dia sem ação conhecida. Acrescentar um caso distinto de ata sem lâmpadas preservaria a regra 2A e separaria as duas situações (`app/chamados/services/tarefa_do_dia.py:238`, `app/chamados/services/tarefa_do_dia.py:697`, `app/folha/tests/test_haiku_fora_da_folha.py:83`, `app/folha/tests/test_haiku_fora_da_folha.py:88`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:6`).

## 15/11 02:14 · sessao 0d23cacef646 · BATIDA-DEDUP-RETRATADA · construtor · origem=Code

C3 entregue em [dedup_retratada_c3_entrega.txt](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c3_entrega.txt:7), com cura, diff, RED/GREEN, censo e comandos.

Validação local: **31 testes verdes e 40 mutantes mortos**, com controles verdes ([evidências](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c3_entrega.txt:68)). `py_compile` e Ruff passaram ([checks](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c3_checks.txt:18), [Ruff](/home/ronald/wt-codex-batida-dedup-retratada-011759/app/ponto/tests/dedup_retratada_c3_checks.txt:43)). [nao verificado]

Suite, smoke real e certificação Code/opus: [nao verificado]. [nao verificado]

## 15/11 02:07 · sessao ab1e79bc6658 · JUSTIFICATIVA-PAGA-O-PERIODO · verificador · origem=Code

**Há bloqueios para liberar esta volta**, principalmente na preservação de exportadas, no contrato `None` entre fechamento e lavratura e na catraca K1. As referências estão na tabela; ocorrência e quantidade desses casos em produção: **[nao verificado]**.

| Arquivo:linha | O que está errado | Caso concreto | Gravidade |
|---|---|---|---|
| `app/ponto/services/fechamento.py:36`, `:539`, `:560`, `:1157`, `:1194` | **Exportada não está genericamente protegida.** A guarda contém somente 09/2026. Nas demais competências, detectar exportação gera aviso e permite continuar até `save/update`. A c11 muda previsto, dias, carimbo e motivos no caminho antes interrompido pelo `sum(None)`. | Competência 10 exportada, com dia justificado `sem_ata`: o recálculo pode persistir o estado novo. A afirmação da entrega “exportada intocada” não é garantida por esse escritor. | **bloqueia** |
| `app/ponto/services/fechamento.py:1073`, `:1225`; `app/ponto/services/dia_pago.py:292`; `app/folha/porta_export.py:455` | **O mesmo desconhecido tem dois significados.** Fechamento conserva abono/realizado `None`; `lavrar` transforma esses mapas em `0.0`. O comparador pula o campo cujo fechamento é `None`, ocultando a perda de informação. A conversão é herdada e continua alcançável pela c11. | Grade com P=480 e W desconhecido: fechamento tem abono/realizado desconhecidos; `DiaPago` tem `0/0`; a comparação não registra divergência nesses campos. | **bloqueia** |
| `app/ponto/services/espelho.py:1264`; `app/templates/ponto/espelho.html:35`, `:270`; `app/ponto/services/dia_pago.py:956` | **A tela não renderiza o rótulo c10.** O resumo recebe `motivo_dias_abono`, mas o template não o consulta. O texto existente depende de `lavrada=False`; qualquer `DiaPago` existente recebe `lavrada=True`. O PDF consulta o motivo em `app/relatorios/pdf_espelho.py:790`. | Dia justificado sem ata, com linha `DiaPago` existente: a tela não mostra o rótulo exigido, embora o resumo esteja marcado. | **bloqueia** |
| `app/ponto/services/espelho.py:1234`, `:1236`, `:1264`; `app/relatorios/pdf_espelho.py:333` | **O rótulo da tela usa a janela visual inteira.** O conjunto `abono_sem_apuracao` não é recortado para a competência, diferentemente dos furos em `espelho.py:1258`. O PDF alimenta seu conjunto pela janela própria. | Justificativa sem ata numa competência anterior pode tornar desconhecido o abono do resumo atual, mesmo com os dias atuais apurados. O fechamento não fornece `dias_abono` para desfazer isso (`app/relatorios/cartao_pela_celula.py:93`). | **bloqueia** |
| `app/ponto/tests/test_catraca_abono_do_motor_c11.py:68` | **K1 tem escape com nome literal estático.** Toda chave de dicionário é descartada como se fosse chave de resumo, sem examinar seu uso. | Novo leitor: `nomes={'dias_abono':'Abonos'}` seguido de `return {k: getattr(resultado,k) for k in nomes}`. Reproduzido: nenhum achado e nenhuma diferença da allowlist. Não exige nome construído em execução. | **bloqueia** |
| `app/ponto/services/fechamento.py:1073`; `app/ponto/tests/test_justificativa_c11_sem_banco.py:80`, `:90` | **Sobrevive um mutante que reabre o defeito K2.** Trocar somente `any` por `all` no abono passa os 102 testes. A fixture desconhecida só produz mapa de abono `{D: None}`, sem mistura com número. | D sem ata + D2 com abono=180: o mutante tenta somar `None` com 180, cai no `except` e apaga previsto/dias/carimbo; motivos permanecem velhos (`fechamento.py:1100`). | **bloqueia** |
| `app/ponto/services/efeito_ausencia.py:75`; `app/ponto/tests/test_justificativa_c10_sem_banco.py:145` | **Outro mutante de uma linha passa os 102 testes:** `sem_apuracao.discard(cd)` → `sem_apuracao.clear()`. A ausência inteira só é testada isoladamente. | D com ausência inteira + VIZ justificado sem ata: o mutante remove também VIZ do conjunto. Na reprodução, PDF passa de desconhecido para **2 dias/16 horas**; tela passa para 2 dias. | **bloqueia** |
| `app/ponto/services/efeito_ausencia.py:75`; `app/folha/export.py:339`; `app/escala/services/leitor_celula.py:479` | **A precedência da ausência inteira diverge entre os caminhos.** O feed remove o desconhecido, mas a grade financeira lê `periodos_justificados` cru e recoloca `sem_ata`. | Ausência inteira + justificativa aprovada + ata ausente: motor recebe dia sem rótulo; grade entrega abono/realizado `None`. O teste c10 verifica somente o feed (`test_justificativa_c10_sem_banco.py:154`). Resíduo herdado. | **fila** |
| `app/relatorios/cartao_pela_celula.py:146`, `:187`, `:208`, `:214` | **A aplicação final dos totais pode desfazer parcialmente o rótulo.** Lavratura fornece horas numéricas enquanto dias permanecem desconhecidos. Além disso, motivo posterior `None` não limpa o motivo anterior. | Resumo marcado pode terminar com `dias_abono=None` e `horas_abono=0`; ou receber dias=1/horas=8 e conservar motivo `'sem_ata'`. O badge PDF exige dias `None`, portanto motivo velho sozinho não prova badge errado (`pdf_espelho.py:790`). | **fila** |
| `app/folha/causas.py:85`; `app/ponto/services/fechamento.py:1093`; `app/ponto/tests/test_justificativa_c11_sem_banco.py:61` | **O caminho liberado pela c11 grava diagnóstico falso de zero.** O classificador usa `None or 0`; o teste substitui esse classificador por resposta constante. | Trabalho previsto=480, realizado desconhecido, sem missing/órfãs: função real devolve `realizado_zero`, com evidência “zero minuto realizado”. Reproduzido em memória. | **fila** |
| `app/ponto/services/fechamento.py:616`, `:1080`; `app/folha/export.py:1248`, `:1317` | **Há leitura de relógio fora do instante da passada.** O escritor já possui `_agora_da_passada`, mas chama retenção sem `hoje`; o leitor consulta `localdate()`. É caminho herdado, agora executado também para `sem_ata`. | Passada atravessando meia-noite pode julgar retenção com outra data. Efeito ocorrido em produção: **[nao verificado]**. | **fila** |

O rastreio dos contratos fica assim:

| Valor | Escritores e leitores conferidos | Contrato / resultado |
|---|---|---|
| Abono, previsto e realizado por dia | Grade recebe os minutos da justificativa em `app/folha/export.py:339`; transporta desconhecido em `app/escala/services/leitor_celula.py:479`; `por_dia_da_grade` produz os mapas em `app/ponto/services/dia_pago.py:387`. Fechamento lê em `fechamento.py:1066`; prontidão em `export.py:1110`; lavratura em `dia_pago.py:288`. | Minutos por data da grade. `sem_ata` produz abono/realizado `None`, preservando previsto (`dia_pago.py:395`). A lavratura rompe esse contrato ao converter para zero (`:292`). |
| Totais de `FechamentoMensal` | Atribuição em `app/ponto/services/fechamento.py:1073`, `:1076`, `:1096`; abstinência no `except :1107`; persistência por `save :1157` ou cópia no `update :1194`. Campos permitem `NULL` em `app/ponto/models.py:432`, `:440`, `:445`. | Com qualquer dia desconhecido, abono/realizado mensal ficam `None`; previsto/dias/carimbo podem permanecer conhecidos. Não grava o previsto como abono. |
| `DiaPago.minutos_abonados` e demais mapas | `lavrar` escreve em `app/ponto/services/dia_pago.py:292`; `retratar_exportada` cria/atualiza em `:490`, `:495` e limpa linhas fora do mapa em `:509`. Campos não são nuláveis (`app/ponto/models.py:349`). | Ambos os caminhos convertem desconhecido em zero. O problema também existe no retrato de exportada; a c11 não altera esse escritor. |
| Abono do resultado do motor | Motor acumula dias/horas em `app/ponto/motor_calculo_v2.py:2630`, `:2631`, `:2633`; composição de fatias declara os campos em `app/ponto/services/fechamento.py:99`. Tela lê em `espelho.py:1261`; PDF em `pdf_espelho.py:479`; instrumento em `ponto/management/commands/diff_espelho_alimentacao.py:28`. | Dias e horas, nunca `None` nesse ramo. Sem ata/sem batida, a ausência da chave permite pagar o previsto por `_abono_inteiro` (`motor_calculo_v2.py:2629`), conforme o aval fornecido. |
| Conjunto e motivo do rótulo | Feed escreve conjunto em `app/ponto/services/efeito_ausencia.py:56`, `:82`; `Autoridade` transporta em `espelho.py:1200`; helper escreve dias/horas `None` e motivo em `efeito_ausencia.py:96`. Tela/PDF chamam em `espelho.py:1264` e `pdf_espelho.py:483`. | O helper não calcula dinheiro. Há problemas posteriores de janela, renderização e sobrescrita descritos acima. |
| Lista de fechamento e comparador | Lista chama prontidão por linha em `app/ponto/services/fechamento_lista.py:85`; prontidão rejeita snapshot sem abono em `app/folha/export.py:1051` e devolve cego/`sem_ata` em `:1112`. Comparador pula total `None` em `app/folha/porta_export.py:455`. | Prontidão preserva desconhecido; comparador deixa de medir o campo. Custo adicional da leitura da grade em produção: **[nao verificado]**. |
| TXT Domínio e extrato | TXT monta eventos em `app/folha/export.py:733`, usando rubricas `horas_*` de `:423`; seleção consulta motivos em `:971`. Extrato lê somas de `DiaPago` em `app/relatorios/views.py:520` e decide ausência de número em `:535`. | Não encontrei conversão direta desses totais `None` para zero no TXT. Entretanto, c11 atualiza os motivos usados na seleção (`fechamento.py:1080`). O extrato não transporta o desconhecimento desses campos de minutos. |

K1 captura atributo, apelido de variável, `getattr` literal e apelido de import; sua allowlist declara contagem, papel e motivo. O escape demonstrado impede considerá-la fechada (`app/ponto/tests/test_catraca_abono_do_motor_c11.py:34`, `:74`, `:103`, `:145`).

Os testes c10 param nas atribuições e na chamada do helper; não exercitam `folha_manda` nem renderização. Os c11 extraem o bloco do escritor e substituem os classificadores vizinhos, portanto o verde não cobre a integração inteira (`app/ponto/tests/test_justificativa_c10_sem_banco.py:11`, `:47`; `app/ponto/tests/test_justificativa_c11_sem_banco.py:25`, `:59`).

Não identifiquei no delta nova fórmula para dia com ata, falta ou DSR: o filtro muda a entrega dos desconhecidos, enquanto cobertura e DSR continuam dependentes de `justificadas`. Isso não comprova os números exigidos de produção nem preservação de exportadas (`app/ponto/services/efeito_ausencia.py:55`, `:58`, `:78`; `app/ponto/motor_calculo_v2.py:2605`, `:2641`). Suite inteira, banco e medidas de produção nesta volta: **[nao verificado]**.

## 15/11 02:05 · sessao 7b78b1e3d767 · - · - · origem=Ronald (tmux)

| Arquivo:linha | Achado | Caso reproduzido em memória | Gravidade |
|---|---|---|---|
| `app/ponto/tests/test_catraca_abono_do_motor_c11.py:67` | K1 exclui toda chave de dicionário supondo que seja resumo, sem verificar o objeto ou seu uso posterior. | Novo módulo com `nomes={'dias_abono':'Abonos'}; return {k:getattr(resultado,k) for k in nomes}`: `achados=[]` e `fora_da_allowlist={}`. Leitor do motor escapa com nome literal estático; não depende dos limites de nomes construídos em runtime. | bloqueia |
| `app/ponto/services/fechamento.py:1073`; `app/ponto/tests/test_justificativa_c11_sem_banco.py:81` | Mutante de uma linha `any(...) → all(...)` sobre `_abono_dia` sobrevive aos 102 testes. A fixture sem ata só possui `_abono_dia={D:None}`, conforme a própria asserção em `:90`, deixando sem cobertura o mapa misto. | D sem ata + D2 com W=300/P=480/abono=180. Original: `(previsto,dias,incertos,abono,realizado,carimbo)=(960,2,0,None,None,AGORA)`. Mutante cai no `except` de `fechamento.py:1100`: os seis campos viram `None`, motivos/causa permanecem `VELHO`. | bloqueia |
| `app/ponto/services/efeito_ausencia.py:75`; `app/ponto/tests/test_justificativa_c10_sem_banco.py:145` | Mutante de uma linha `sem_apuracao.discard(cd) → sem_apuracao.clear()` também sobrevive aos mesmos 102 testes. A ausência inteira só é testada isoladamente em D..D (`:149`). | D com ausência inteira + VIZ justificado sem ata: original conserva `{VIZ}`, PDF/tela saem `None` com `sem_ata`; mutante esvazia o conjunto e retorna PDF `dias_abono=2, horas_abono=16.0`, tela `dias_abono=2`. | bloqueia |

Os sete módulos executados foram `test_justificativa_c11_sem_banco`, `test_catraca_abono_do_motor_c11`, `test_justificativa_c10_sem_banco`, `test_justificativa_c9_sem_banco`, `test_justificativa_c8_sem_banco`, `test_justificativa_soma_sem_banco` e `test_justificativa_periodo_v2_sem_banco`: 102 testes, zero failures/errors para cada sobrevivente. Execução somente em memória com `python3 -B`; catálogos de mutantes não foram executados nesta auditoria [nao verificado quanto ao catálogo]. [nao verificado]

K1 captura as sete formas anunciadas em `test_catraca_abono_do_motor_c11.py:145`, incluindo apelido de variável/import; `resultado.__getattribute__('dias_abono')` também foi captado como `str_arg` pela regra de `:74`. A allowlist declara contagem, papel e motivo em `:34`. `any→all` sobre realizado é morto pelo teste sem ata de `test_justificativa_c11_sem_banco.py:80` (uma falha, zero erros). [nao verificado]

## 15/11 02:05 · sessao 870959463a02 · - · - · origem=Ronald (tmux)

| arquivo:linha | O que está errado | Caso concreto | Gravidade |
|---|---|---|---|
| `app/templates/ponto/espelho.html:270`; `app/ponto/services/dia_pago.py:956`; `app/ponto/services/espelho.py:1264` | O rótulo c10 existe no resumo, mas a tela não o renderiza. Seu texto “sem apuração ainda” depende exclusivamente da inexistência de `DiaPago`; qualquer linha existente recebe `lavrada=True`. O PDF consulta o motivo correto em `app/relatorios/pdf_espelho.py:790`. | Dia justificado sem ata, com `DiaPago` existente: resumo recebe `motivo_dias_abono='sem_ata'`, mas a tela não mostra o rótulo exigido. O topo mostra outros campos em `app/templates/ponto/espelho.html:35`. | **bloqueia** |
| `app/ponto/services/espelho.py:1234`; `app/ponto/services/espelho.py:1264` | O conjunto que decide o rótulo cobre a janela visual, potencialmente anterior à competência, e não é filtrado pela competência. Apenas os furos recebem esse filtro em `app/ponto/services/espelho.py:1258`. | Dia sem ata na competência anterior faz o resumo atual receber abono desconhecido; o fechamento não repõe `dias_abono` nem limpa seu motivo em `app/relatorios/cartao_pela_celula.py:93`. | **bloqueia** |
| `app/ponto/services/dia_pago.py:395`; `app/ponto/services/dia_pago.py:292`; `app/ponto/services/fechamento.py:1225` | `None` do mesmo mapa significa desconhecido no fechamento e vira zero na lavratura, sem marca no detalhe. Reproduzido executando as funções reais por AST, sem persistência. | Grade `P=480`, abono/realizado `None` → `DiaPago` com abono `0.0`, previsto `480.0`, realizado `0.0`, detalhe `{}`. O fechamento c11 conserva abono/realizado `None` em `app/ponto/services/fechamento.py:1073`. | **bloqueia** |
| `app/relatorios/cartao_pela_celula.py:214`; `app/ponto/services/efeito_ausencia.py:96` | Uma fonte posterior conhecida não limpa o motivo c10: `None` é descartado também para o campo textual. | Reproduzido: resumo desconhecido + totais `{dias_abono:1, horas_abono:8, motivo_dias_abono:None}` termina com `1`, `8` e motivo `'sem_ata'`. O defeito está no leitor herdado, alimentado pelo novo escritor do motivo. | **fila** |
| `app/ponto/services/efeito_ausencia.py:75`; `app/folha/export.py:339`; `app/escala/services/leitor_celula.py:479` | A precedência de ausência inteira é aplicada somente no feed do motor. O caminho financeiro da grade lê os períodos crus e recoloca o desconhecido eliminado pelo feed. | Reproduzido com a fixture de `app/ponto/tests/test_justificativa_c10_sem_banco.py:148`: ausência inteira + justificativa + ata ausente → feed sem rótulo; grade e mapas de abono/realizado com `None`. O teste verifica só o primeiro lado em `app/ponto/tests/test_justificativa_c10_sem_banco.py:154`. | **fila** |

Os testes c10 executam somente atribuições e a chamada do rotulador, antes da aplicação final de totais e da renderização; portanto não cobrem os dois primeiros achados nem a retenção do motivo (`app/ponto/tests/test_justificativa_c10_sem_banco.py:47`, `app/ponto/tests/test_justificativa_c10_sem_banco.py:90`). Alcance desses casos em produção: **[nao verificado]**.

## 15/11 02:05 · sessao 8d658e89513a · - · - · origem=Ronald (tmux)

| Arquivo:linha | Erro / caso concreto | Gravidade |
|---|---|---|
| `app/ponto/services/fechamento.py:35`, `:539`, `:560`, `:1073`, `:1099`, `:1157`, `:1173` | **Exportada não está genericamente protegida.** A guarda contém somente 09/2026; outras competências exportadas recebem apenas log e seguem para `save/update`. Com dia `sem_ata`, c11 passa a persistir previsto/dias/carimbo onde antes o `except` os anulava, além de atualizar motivos/causa. Caso alcançável: competência 10 exportada, célula de trabalho justificada sem ata. Existência desse caso em produção: **[nao verificado]**. | **bloqueia** |
| `app/ponto/services/dia_pago.py:395`, `:292`; `app/folha/porta_export.py:453` | **Mesmo desconhecido vira zero e desaparece da conferência.** `por_dia_da_grade` entrega abono/realizado `None`; FM conserva `None`, mas `lavrar` grava zero. Comparador pula integralmente o campo cujo FM é `None`, sem registrar desconhecimento. Caso: P=480 e W sem ata → FM desconhecido, DiaPago=0, contador sem divergência nesse campo. | **bloqueia** |
| `app/folha/causas.py:85`, `:86`; `app/ponto/services/fechamento.py:1096`; `app/ponto/tests/test_justificativa_c11_sem_banco.py:61` | **Novo caminho ativo grava diagnóstico falso de realizado zero.** Com trabalho, P=480, W=`None`, sem missing/orfas, classificador transforma `None` em zero e devolve `realizado_zero`, evidência “zero minuto realizado”. Reproduzido executando o módulo real em memória, sem escrita. O teste substitui o classificador por resposta constante. | **fila** |
| `app/relatorios/cartao_pela_celula.py:146`, `:187`, `:208`, `:214`; `app/ponto/services/dia_pago.py:723` | **Lavratura pode desfazer parcialmente o rótulo.** Dia sem ata produz `dias_abono=None`, mas horas vêm da soma numérica de DiaPago, que já perdeu o desconhecido. `aplicar_nos_totais` pode entregar dias desconhecidos ao lado de horas numéricas. Os testes c10 excluem expressamente `folha_manda`: `app/ponto/tests/test_justificativa_c10_sem_banco.py:12`. | **fila** |

O TXT Domínio **não exporta esses três campos de minutos diretamente**: monta eventos em `app/folha/export.py:733`, definidos pelos campos `horas_*` em `:423` e `:442`. Portanto, “exporta zero silenciosamente por causa de `minutos_abonados=None`” não foi encontrado nesse emissor. Entretanto, a participação no TXT consulta motivos por `app/folha/export.py:971`; c11 passa a atualizar esses motivos em `app/ponto/services/fechamento.py:1080`.

A lista lê `prontidao` por colaborador em `app/ponto/services/fechamento_lista.py:85`; abono desconhecido cai na grade e retorna cego/`sem_ata` em `app/folha/export.py:1051` e `:1112`. O custo adicional em produção é **[nao verificado]**.

Há relógio herdado no caminho agora executado para `sem_ata`: `app/ponto/services/fechamento.py:1080` não fornece `hoje`; `app/folha/export.py:1317` encaminha `None` e `:1248` lê `localdate()`, apesar do instante único disponível em `app/ponto/services/fechamento.py:616`. Impacto concreto na virada de dia: **[nao verificado]**.

_5494 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
