# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 13:43** · publicado em **16/11/2026 13:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 13:43 · sessao 8c89640a4b0e · BUG-PAR-O-PAR · construtor · origem=Code

**Entrega incompleta: faltam os arrays U60/U103 do anexo `u163.py`.** O JSON não foi inventado; seu teste termina com `FileNotFoundError`. [RELATO.md:5](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:5) [nao verificado]

A cura escrita limita DXG aos PKs congelados, consulta K6 antes das escritas e usa a célula dona na guarda. [materializacao.py:703](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/materializacao.py:703), [validacao.py:289](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/validacao.py:289), [materializacao.py:402](/home/ronald/wt-codex-bug-par-o-par-131207/app/chamados/services/materializacao.py:402)

Arquivos tocados, relativos a `app/`:

```text
chamados/services/universo_bug_par.py
chamados/services/materializacao.py
chamados/services/validacao.py
chamados/services/fio.py
chamados/management/commands/bug_par_103.py
chamados/templatetags/chamados_tags.py
templates/chamados/partials/_pergunta_card.html
templates/chamados/partials/modal_fio.html
chamados/tests/test_universo_bug_par.py
chamados/tests/test_bug_par_volta.py
chamados/tests/test_bug_par_103.py
chamados/tests/test_bug_par_sem_banco.py
chamados/tests/test_bug_par_projetado.py
chamados/tests/test_s139.py
chamados/tests/bug_par_mutantes.py
core/tests/test_medida_turno_do_par.py
docs/JUIZES.md
docs/RELATO.md
```

A entrega detalhada está registrada com referências:

- **Cada RED, `arquivo::teste`, afirmação e mutante:** [RELATO.md:21](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:21). [nao verificado]
- **Comando de cada mutante:** [RELATO.md:46](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:46).
- **Quem escreve e quem lê cada campo/função:** [RELATO.md:64](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:64).
- **Lookup reconciliado:** [JUIZES.md:38](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/JUIZES.md:38).

Medição local, sem certificação: **8 testes OK, 15 mutantes mortos por asserção e 35 testes da catraca OK**. Integração Django, vizinhos O261/O317, dez mutantes de integração e reprodução de 84/19+75 permanecem **[nao verificado]**. [RELATO.md:48](/home/ronald/wt-codex-bug-par-o-par-131207/app/docs/RELATO.md:48) [nao verificado]

Comandos para a sessão, após fornecer o JSON exato:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_universo_bug_par chamados.tests.test_bug_par_sem_banco chamados.tests.test_bug_par_volta chamados.tests.test_bug_par_103 chamados.tests.test_bug_par_projetado chamados.tests.test_s139 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_o261_vespera_nao_absolve chamados.tests.test_o317_passivo_pela_porta chamados.tests.test_o261_paridade_respeita_a_lampada core.tests.test_medida_turno_do_par chamados.tests.test_e2_tarefa_do_dia chamados.tests.test_e3_portas_tarefa --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 -B -m chamados.tests.bug_par_mutantes --django --mutante todos
```

LEI-AKITA: origem=universo congelado pendente do anexo + portão da materialização, testemunha=estado_aplicacao_pergunta e competencia_entregue, RED=blocos (a)-(c) + fora163 + 5644, quem-mais-le=docs/RELATO.md:64, juizes novos=0, padrao=Single Writer Principle.

## 16/11 13:25 · sessao 77f8120193e8 · UI-VINCULO-MOSTRA-O-CALENDARIO · leitura · origem=Code

**(a) As duas leituras existem no código:** a foto deve chegar à célula pela geração/regeneração; grade e precedência também leem `FolgaDia` por cima do previsto, sem alterar `CelulaDia.trabalha`. Já `EC.eh_dia_trabalho` devolve o valor da célula existente. Portanto, gravar a foto não garante, por si só, que todos esses leitores responderão igual. (`app/escala/folgas.py:215-218`; `app/escala/utils.py:1147-1153`; `app/ponto/precedencia.py:356-387`; `app/escala/models.py:1087-1096`.)

**Quem escreve `FolgaDia`.** O módulo autorizado é `app/escala/folgas.py`. (`app/core/portas.py:470-473`.)

| Porta | Escrita e entradas |
|---|---|
| `gerar_foto_folgas` | Apaga a foto de calendário do mês, preserva manuais, recria as datas e chama a relavratura na mesma transação. Entradas: vínculo individual, aplicação ao tipo, herança do padrão e comando `lavrar_foto_folgas --apply`. (`app/escala/folgas.py:150-218`; `app/colaboradores/views.py:239-246`; `app/escala/views.py:408-412`; `app/colaboradores/services/vinculo.py:151-166`; `app/escala/management/commands/lavrar_foto_folgas.py:159-170`.) |
| `sincronizar_foto_futura_do_calendario` | Usa o mesmo escritor com recorte de datas futuras e depois regenera futuras. É chamada ao salvar o calendário ou mudar o calendário do vínculo. (`app/escala/folgas.py:597-642`; `app/colaboradores/services/vinculo.py:459-465`.) |
| `lancar_folga` | `get_or_create` manual seguido de relavratura. Entradas: toggle do Plano e declaração pela supervisão. (`app/escala/folgas.py:329-339`; `app/escala/views_plano_folgas.py:124-132`; `app/ponto/services/declarar_dia.py:179-193`.) |
| `alterar_folga` | `_foto_do_dia` apaga, cria ou altera `tipo`; depois relavra. Entrada: toggle do Plano. (`app/escala/folgas.py:343-382`; `app/escala/views_plano_folgas.py:134-137`.) |
| `aplicar_folgas_em_lote` | Usa `_foto_do_dia`, recusa competência anterior à corrente e relavra o intervalo gravado. Entrada: Plano em lote. (`app/escala/folgas.py:401-421`; `app/escala/views_plano_folgas.py:163-185`.) |
| `reparentar_folga` | Muda o vínculo da folga manual e chama relavratura; calendário é recusado. Na competência passada usa `so_rejulgar`, preservando o DNA. Entradas: signal de criação/vigência e comando de reparentamento. (`app/escala/folgas.py:484-525`; `app/escala/signals.py:185-207`; `app/escala/management/commands/reparentar_folgas.py:22-34`.) |

Há ainda efeitos referenciais declarados: excluir o vínculo apaga suas folgas por `CASCADE`; excluir o calendário deixa `calendario_origem` nulo por `SET_NULL`. (`app/escala/models.py:1585-1591`.)

**Quem escreve `CelulaDia.trabalha`.**

- **Geração:** `gerar_celulas_periodo` calcula `valor = ec.eh_dia_trabalho_calculado(d)` e cria a célula com esse valor e DNA. A data presente na foto do próprio vínculo resulta em `False`. Célula existente editada é preservada; passado existente só pode receber DNA ausente; hoje/futuro existente é pulado nessa função. (`app/ponto/portas/celula.py:911-935`; `app/escala/models.py:1280-1288`; `app/escala/models.py:1309-1317`.)
- **Command `gerar_celulas`:** antes da geração, possui suplemento que regenera células existentes de **hoje**. Isso não constitui uma atualização geral das futuras existentes. (`app/ponto/management/commands/gerar_celulas.py:303-325`; `app/ponto/management/commands/gerar_celulas.py:345-350`.)
- **Regeneração:** recalcula pela foto/cadastro; cria ausentes e reescreve existentes divergentes somente com `origem='gerada'`. Guarda DNA anterior, instante e incrementa `regeneracoes`. Também pode desligar células fora da vigência. (`app/ponto/portas/celula.py:229-311`.)
- **Edição humana:** `editar_dia` escreve `trabalha`, DNA, geradora, `origem='editada'`, autor e instante; não incrementa os campos de regeneração. Troca de plantão delega a essa edição. (`app/ponto/portas/celula.py:575-580`; `app/ponto/portas/celula.py:637-647`; `app/ponto/portas/celula.py:670-715`.)
- **Reversão:** `reverter_regeneracao_dia` restaura `trabalha` pelo DNA anterior e incrementa os carimbos de regeneração. (`app/ponto/portas/celula.py:817-837`.)

**Quem lê cada fonte.**

| Consumidor | Fonte efetivamente lida |
|---|---|
| `EC.eh_dia_trabalho` | Existindo célula, retorna `cel.trabalha`; somente sem célula chama o cálculo que consulta a foto. (`app/escala/models.py:1087-1096`.) |
| `EC.marcos_do_dia` | Lê `dna.marcos`; sem marcos utilizáveis, retorna os marcos do template. Não aplica a foto nessa leitura. (`app/escala/models.py:1117-1133`.) |
| Grade/espelho | O overlay consulta `FolgaDia` pelo juiz, põe `tipo_dia='folga'`, limpa as colunas e órfãs **do dicionário da grade**. O espelho obtém seu flag `folga` desse `tipo_dia`. (`app/escala/utils.py:1140-1153`; `app/escala/utils.py:1844`; `app/ponto/services/espelho.py:673-679`; `app/ponto/services/espelho.py:764-771`.) |
| Precedência | Usa `trabalha` como previsto, mas inclui folga lançada válida mesmo com previsto `True`; folga vence trabalho na ordenação. Calendário vale apenas para o vínculo do dia; manual vale para a pessoa. (`app/ponto/precedencia.py:46`; `app/ponto/precedencia.py:235-239`; `app/ponto/precedencia.py:347-397`.) |
| Motor | No caminho com vínculo não intermitente, classifica folga trabalhada pela resposta `False` de `EC.eh_dia_trabalho`; os marcos vêm de `EC.marcos_do_dia`, com fallback ao template. (`app/ponto/motor_calculo_v2.py:2445-2469`; `app/ponto/motor_calculo_v2.py:2290-2291`; `app/ponto/motor_calculo_v2.py:1532-1540`.) |
| Folha | O fechamento monta datas previstas por `EC.eh_dia_trabalho`, alimenta o motor com células e grava os resultados, incluindo FT. O TXT lê os eventos do fechamento. (`app/ponto/services/fechamento.py:724-755`; `app/ponto/services/fechamento.py:918-924`; `app/ponto/services/fechamento.py:965-966`; `app/folha/export.py:439-446`; `app/folha/export.py:725-739`.) |

O dinheiro mostrado no espelho segue outro percurso que seu flag visual de folga: pago/rubricas do dia leem `DiaPago`; totais da competência passam por `folha_manda` e `fechamento_lido`. (`app/ponto/services/espelho.py:805-813`; `app/ponto/services/espelho.py:1283-1284`; `app/relatorios/cartao_pela_celula.py:69-76`; `app/ponto/services/fechamento.py:1759-1761`.)

**Caso das cinco folgas de dezembro.** Para célula já existente, o percurso previsto é:

`gerar_foto_folgas → relavrar_celulas_da_foto → regenerar_futuras_do_cadastro → regenerar_celulas_vinculo → trabalha=False`.

As chamadas estão em `app/escala/folgas.py:215-218`, `app/escala/folgas.py:297-299` e `app/ponto/services/cadastro_futuro.py:43-45`; o cálculo da foto e a escrita estão em `app/escala/models.py:1309-1317` e `app/ponto/portas/celula.py:289-311`.

**O limite concreto encontrado:** a foto solicitada é mensal, mas a relavratura futura seleciona somente células geradas já existentes dentro de `_janela_default(hoje)`, a partir de amanhã, respeitando o vínculo do dia. Essa janela é a união das competências **correntes** das empresas ativas. Não é o mês inteiro pedido a `gerar_foto_folgas`. (`app/escala/folgas.py:180-197`; `app/ponto/services/cadastro_futuro.py:26-45`; `app/ponto/management/commands/gerar_celulas.py:290-300`.)

Logo, datas fora dessa janela não chegam à regeneração por esse percurso. **Que esse filtro tenha excluído precisamente as cinco datas relatadas: `[nao verificado]`.** Data do ato, cortes, origem/geradora das células e bloqueios do caso: `[nao verificado]`. `regeneracoes=0` também não prova ausência de chamada: o incremento ocorre na reescrita divergente, após seleção e guardas. (`app/ponto/portas/celula.py:284-311`.)

**(b) Sim: `gerar_foto_folgas` no mês corrente grava/apaga `FolgaDia` anterior a hoje.** A seleção é pelo mês inteiro; `delete` e `bulk_create` não têm corte por hoje. As manuais são preservadas. (`app/escala/folgas.py:180-197`.)

**A célula passada também pode ser tocada:** a função chama relavratura desde o primeiro dia do mês; o ramo passado encaminha `desde..hoje` à regeneração formal. Essa é precisamente a exceção à irretroatividade prevista na seção 4, sujeita às guardas de vigência, lavra e origem gerada. (`app/escala/folgas.py:212-218`; `app/escala/folgas.py:290-296`; `CLAUDE.md:149-156`; `app/ponto/portas/celula.py:147-162`; `app/ponto/portas/celula.py:284-311`.)

A sincronização **futura** preserva passado, mas as views de vincular/aplicar calendário também chamam depois a geração mensal sem esse recorte. Portanto, a proteção da primeira chamada não protege a segunda. (`app/escala/folgas.py:621-639`; `app/colaboradores/views.py:243-246`; `app/escala/views.py:411-412`.)

**DONO: NAO SEI** — o juiz exige insumos além dos três valores relatados; esses insumos do caso estão `[nao verificado]`. Decisão: `app/ponto/management/commands/e6_oraculo.py:98-110`.

## 16/11 13:24 · sessao 0e5369e6a2cb · UI-VINCULO-MOSTRA-O-CALENDARIO · sonda · origem=Code

# Calendarios de folga em uso com grade vazia (modo dias_mes)

UNIVERSO calendarios dias_mes em uso 72 | com grade vazia 68 | vinculos afetados 77. [nao verificado]

| Calendario (pk) | Vinculos ativos | Grades que faltam | Dias que ja tem (por grade) | Quem usa |
|---|---:|---|---|---|
| 69 | 5 | meses de 30; fevereiro | 31: [4, 11, 18, 25] | col390/ec929, col392/ec923, col468/ec402, col575/ec925, col825/ec926 [nao verificado] |
| 72 | 2 | fevereiro | 31: [1, 8, 15, 22, 25, 29]; 30: [5, 12, 15, 19, 26] | col309/ec1355, col491/ec1271 [nao verificado] |
| 78 | 2 | meses de 31; fevereiro | 30: [1, 7, 14, 21, 28] | col921/ec1141, col1026/ec1581 [nao verificado] |
| 87 | 2 | fevereiro | 31: [4, 11, 18, 25]; 30: [1, 8, 15, 22, 29] | col235/ec202, col600/ec944 [nao verificado] |
| 122 | 2 | meses de 30; fevereiro | 31: [1, 2, 5, 6, 7, 8, 9, 12, 13, 14, 15, 16, 19, 20, 21, 22, 23, 26, 27, 28, 29, 30] | col346/ec1383, col610/ec1420 [nao verificado] |
| 138 | 2 | meses de 31; fevereiro | 30: [1, 8, 15, 22, 29] | col476/ec1451, col864/ec1494 [nao verificado] |
| 39 | 1 | fevereiro | 31: [5, 6, 7, 12, 13, 19, 20, 26, 27]; 30: [5, 6, 7, 12, 13, 19, 20, 26, 27] | col216/ec184 [nao verificado] |
| 50 | 1 | meses de 31; fevereiro | 30: [30] | col128/ec910 [nao verificado] |
| 53 | 1 | meses de 30; fevereiro | 31: [3, 11, 17, 25, 31] | col384/ec1347 [nao verificado] |
| 54 | 1 | meses de 30; fevereiro | 31: [4, 9, 18, 23] | col310/ec1167 [nao verificado] |
| 55 | 1 | meses de 30; fevereiro | 31: [5, 11, 19, 25] | col204/ec1045 [nao verificado] |
| 56 | 1 | meses de 30; fevereiro | 31: [4, 13, 18, 27] | col962/ec1303 [nao verificado] |
| 57 | 1 | meses de 30; fevereiro | 31: [6, 11, 20, 25] | col577/ec491 [nao verificado] |
| 59 | 1 | meses de 30; fevereiro | 31: [4, 10, 18, 24] | col349/ec307 [nao verificado] |
| 60 | 1 | meses de 30; fevereiro | 31: [3, 11, 17, 25, 31] | col587/ec500 [nao verificado] |
| 61 | 1 | meses de 30; fevereiro | 31: [4, 12, 18, 26] | col205/ec1044 [nao verificado] |
| 62 | 1 | meses de 30; fevereiro | 31: [5, 11, 19, 25] | col214/ec182 [nao verificado] |
| 64 | 1 | meses de 30; fevereiro | 31: [8, 11, 15, 22, 29] | col919/ec1136 [nao verificado] |
| 66 | 1 | meses de 31; fevereiro | 30: [2, 9, 16, 22, 23, 30] | col243/ec953 [nao verificado] |
| 68 | 1 | fevereiro | 31: [6, 13, 18, 20, 27]; 30: [3, 10, 17, 22, 24] | col278/ec1350 [nao verificado] |
| 70 | 1 | fevereiro | 31: [6, 11, 13, 20, 27]; 30: [3, 8, 10, 17, 24] | col203/ec1409 [nao verificado] |
| 71 | 1 | meses de 31; fevereiro | 30: [4, 11, 15, 18, 25] | col617/ec1354 [nao verificado] |
| 73 | 1 | fevereiro | 31: [2, 9, 15, 16, 23, 30]; 30: [6, 13, 15, 20, 27] | col263/ec1526 [nao verificado] |
| 74 | 1 | fevereiro | 31: [3, 4, 10, 17, 24, 31]; 30: [6, 13, 15, 20, 27] | col922/ec1142 [nao verificado] |
| 76 | 1 | fevereiro | 31: [6, 8, 13, 20, 27]; 30: [3, 8, 10, 17, 24] | col968/ec1358 [nao verificado] |
| 77 | 1 | fevereiro | 31: [5, 12, 18, 19, 26]; 30: [2, 8, 9, 16, 23, 30] | col879/ec1359 [nao verificado] |
| 79 | 1 | fevereiro | 31: [4, 5, 12, 19, 26]; 30: [1, 2, 9, 16, 23, 30] | col327/ec285 [nao verificado] |
| 81 | 1 | fevereiro | 31: [2, 9, 16, 18, 23, 30]; 30: [6, 13, 15, 20, 27] | col522/ec446 [nao verificado] |
| 82 | 1 | meses de 30; fevereiro | 31: [3, 10, 17, 24, 25, 31] | col292/ec1267 [nao verificado] |
| 83 | 1 | fevereiro | 31: [5, 12, 18, 19, 26]; 30: [2, 9, 15, 16, 23, 30] | col917/ec1134 [nao verificado] |
| 84 | 1 | fevereiro | 31: [6, 13, 20, 25, 27]; 30: [3, 10, 17, 22, 24] | col301/ec265 [nao verificado] |
| 85 | 1 | fevereiro | 31: [4, 7, 14, 21, 28]; 30: [1, 4, 11, 18, 25] | col849/ec1001 [nao verificado] |
| 86 | 1 | fevereiro | 31: [1, 4, 8, 15, 22, 29]; 30: [1, 5, 12, 19, 26] | col382/ec1269 [nao verificado] |
| 89 | 1 | fevereiro | 31: [4, 11, 18, 25]; 30: [1, 8, 15, 22, 29] | col814/ec979 [nao verificado] |
| 90 | 1 | meses de 30; fevereiro | 31: [4, 7, 14, 20, 26] | col89/ec1039 [nao verificado] |
| 92 | 1 | meses de 30; fevereiro | 31: [2, 6, 12, 18, 24, 31] | col638/ec1306 [nao verificado] |
| 94 | 1 | meses de 30; fevereiro | 31: [5, 12, 18, 19, 26] | col81/ec812 [nao verificado] |
| 95 | 1 | meses de 30; fevereiro | 31: [1, 6, 11, 15, 21, 25, 28] | col900/ec1341 [nao verificado] |
| 96 | 1 | meses de 31; fevereiro | 30: [4, 8, 14, 20, 26] | col107/ec1203 [nao verificado] |
| 98 | 1 | meses de 31; fevereiro | 30: [5, 11, 18, 24, 29] | col114/ec1537 [nao verificado] |
| 101 | 1 | meses de 30; fevereiro | 31: [3, 10, 18, 27] | col829/ec947 [nao verificado] |
| 106 | 1 | meses de 30; fevereiro | 31: [1, 8, 16, 22, 29] | col749/ec1374 [nao verificado] |
| 107 | 1 | meses de 30; fevereiro | 31: [8, 15, 18, 23, 29] | col87/ec802 [nao verificado] |
| 109 | 1 | meses de 30; fevereiro | 31: [6, 13, 20, 27] | col245/ec1381 [nao verificado] |
| 115 | 1 | fevereiro | 31: [4, 11, 18, 20, 25]; 30: [5, 8, 12, 19, 26] | col822/ec1360 [nao verificado] |
| 118 | 1 | meses de 30; fevereiro | 31: [4, 10, 18, 24] | col43/ec1411 [nao verificado] |
| 123 | 1 | meses de 31; fevereiro | 30: [4, 11, 18, 22, 25] | col865/ec1357 [nao verificado] |
| 124 | 1 | meses de 30; fevereiro | 31: [4, 11, 18, 25] | col826/ec967 [nao verificado] |
| 127 | 1 | meses de 30; fevereiro | 31: [2, 10, 25, 28, 29] | col950/ec1321 [nao verificado] |
| 128 | 1 | meses de 30; fevereiro | 31: [2, 9, 11, 16, 23, 30] | col369/ec1313 [nao verificado] |
| 132 | 1 | meses de 30; fevereiro | 31: [5, 12, 19, 25] | col903/ec1460 [nao verificado] |
| 133 | 1 | meses de 30; fevereiro | 31: [4, 6, 13, 20, 27] | col129/ec853 [nao verificado] |
| 134 | 1 | meses de 30; fevereiro | 31: [1, 7, 13, 19, 25] | col789/ec1472 [nao verificado] |
| 135 | 1 | meses de 30; fevereiro | 31: [4, 10, 16, 22, 28] | col967/ec1316 [nao verificado] |
| 136 | 1 | meses de 30; fevereiro | 31: [6, 12, 18, 24, 30] | col446/ec384 [nao verificado] |
| 137 | 1 | meses de 30; fevereiro | 31: [2, 9, 16, 20, 25] | col502/ec1150 [nao verificado] |
| 140 | 1 | fevereiro | 31: [2, 8, 16, 22, 30]; 30: [2, 8, 16, 22, 30] | col899/ec1512 [nao verificado] |
| 141 | 1 | meses de 31; fevereiro | 30: [1, 7, 15, 21, 29] | col444/ec1513 [nao verificado] |
| 143 | 1 | meses de 31; fevereiro | 30: [11] | col1022/ec1521 [nao verificado] |
| 144 | 1 | meses de 31; fevereiro | 30: [8, 14, 22, 28] | col941/ec1556 [nao verificado] |
| 145 | 1 | meses de 31; fevereiro | 30: [7, 15, 21, 29] | col945/ec1255 [nao verificado] |
| 146 | 1 | meses de 31; fevereiro | 30: [1, 7, 15, 21, 29] | col985/ec1418 [nao verificado] |
| 149 | 1 | fevereiro | 31: [6, 13, 15, 20, 27]; 30: [7, 14, 15, 21, 28] | col191/ec161 [nao verificado] |
| 150 | 1 | meses de 31; fevereiro | 30: [1, 7, 14, 21, 28] | col933/ec1184 [nao verificado] |
| 151 | 1 | meses de 31; fevereiro | 30: [1, 5, 12, 19, 26] | col991/ec1425 [nao verificado] |
| 152 | 1 | meses de 31; fevereiro | 30: [2, 9, 16, 23, 30] | col992/ec1584 [nao verificado] |
| 153 | 1 | meses de 31; fevereiro | 30: [1, 7, 14, 21, 28] | col474/ec1270 [nao verificado] |
| 154 | 1 | meses de 30; fevereiro | 31: [1, 10, 15, 24, 29] | col529/ec1038 [nao verificado] |

Leitura em [nao verificado]; nada foi escrito no banco. [nao verificado] Cadastro e do admin (L-099): corrigir pela tela do calendario. [nao verificado]

## 16/11 13:24 · sessao 43aa1fa989b8 · - · - · origem=Ronald (tmux)

- **Célula versus foto:** `EC.eh_dia_trabalho` retorna diretamente `cel.trabalha` se existir célula; consulta cálculo/foto somente sem célula (`app/escala/models.py:1087-1096`). O cálculo puro lê `FolgaDia` do próprio vínculo/mês e responde `False` se a data estiver na foto (`app/escala/models.py:1280-1288`, `app/escala/models.py:1309-1317`). `EC.marcos_do_dia` lê `dna.marcos`; sem marcos, usa template (`app/escala/models.py:1117-1133`).

- **Grade/espelho leem folga por cima:** `_aplicar_folgas_foto` consulta juiz da folga e altera apenas o dicionário da grade: `tipo_dia='folga'`, colunas `None`, órfãs vazias (`app/escala/utils.py:1140-1153`); a grade composta chama esse overlay (`app/escala/utils.py:1844`). O espelho deriva seu flag `folga` desse `tipo_dia` e o publica no dia (`app/ponto/services/espelho.py:673-679`, `app/ponto/services/espelho.py:764-771`).

- **Precedência também sobrepõe:** lê o `trabalha` da célula como `previsto`; mesmo `previsto=True`, uma folga lançada válida entra na lista e vence trabalho pela ordem de precedência (`app/ponto/precedencia.py:347-387`, `app/ponto/precedencia.py:46`). Folga de calendário só vale para o vínculo do dia; manual vale para pessoa (`app/ponto/precedencia.py:235-239`). Espelho carrega esses fatos (`app/ponto/services/espelho.py:646-649`); o compositor responde `FOLGA` quando `tipo_do_dia='folga'` (`app/ponto/services/dia_decidido.py:370-372`).

- **Motor segue célula:** classificação de folga trabalhada pergunta a `EC.eh_dia_trabalho`; só sua resposta `False` aciona FT (`app/ponto/motor_calculo_v2.py:2445-2469`), separado em `periodos_ft` (`app/ponto/motor_calculo_v2.py:2492-2494`). Marcos vêm de EC e DNA, com fallback (`app/ponto/motor_calculo_v2.py:2290-2291`, `app/ponto/motor_calculo_v2.py:1532-1540`; outros consumidores `app/ponto/motor_calculo_v2.py:2686-2691`, `app/ponto/motor_calculo_v2.py:3014-3017`, `app/ponto/motor_calculo_v2.py:3391-3396`). Portanto, célula `True` e folga válida na foto permitem grade/precedência `folga` enquanto esta classificação do motor segue trabalho, pelas condições citadas.

- **Folha:** fechamento monta datas previstas por `esc.eh_dia_trabalho` (`app/ponto/services/fechamento.py:724-733`), alimenta motor com células e chama `apurar_por_fatia` (`app/ponto/services/fechamento.py:748-756`). Horas normais/extras leem resultado (`app/ponto/services/fechamento.py:918-924`); FT lê períodos reconhecidos e publica `horas_folga_trabalhada` (`app/ponto/services/fechamento.py:925-928`, `app/ponto/services/fechamento.py:965-966`). TXT lê fechamentos recebidos ou persistidos (`app/folha/export.py:717-722`) e seus eventos (`app/folha/export.py:725-739`), incluindo `horas_folga_trabalhada` (`app/folha/export.py:439-446`).

- **Totais/pago do espelho:** totais chamam `folha_manda` (`app/ponto/services/espelho.py:1283-1284`), que chama `totais_da_folha` (`app/relatorios/cartao_pela_celula.py:320-323`); esta chama `fechamento_lido` (`app/relatorios/cartao_pela_celula.py:69-76`), a mesma apuração em modo leitura (`app/ponto/services/fechamento.py:1759-1761`). Pago/rubricas do dia leem `DiaPago` (`app/ponto/services/espelho.py:805-813`).

- **Dono individual:** os insumos necessários para executar `dono_da_divergencia` não estão no dado recebido `[nao verificado]`. O juiz distingue cadastro por flags específicas, depois batida, fase e `em_cadastro_x_realidade`; só o resíduo é estrutura (`app/ponto/management/commands/e6_oraculo.py:98-110`). Causa executada e dono individual do relato não se deduzem de `FolgaDia`, `trabalha=True` e `regeneracoes=0` isoladamente `[nao verificado]`.

## 16/11 13:24 · sessao 325eca3c6fbc · - · - · origem=Ronald (tmux)

O módulo escritor de `CelulaDia` é `app/ponto/portas/celula.py`; o registro de portas aponta somente para ele e o selo tem allowlist vazia (`app/core/portas.py:107-113`; `app/ponto/tests/test_chokepoint_celula_gate.py:19-23,48-54`).

- **Geração normal:** `gerar_celulas_periodo` chama `ec.eh_dia_trabalho_calculado(d)` e cria a célula com `trabalha=valor`, geradora e DNA (`app/ponto/portas/celula.py:911-918`). Célula editada fica; passado existente só recebe DNA ausente, sem mudar `trabalha`; hoje/futuro existente é pulado pela função (`app/ponto/portas/celula.py:919-935`). O command possui suplemento próprio que regenera **hoje**, pela porta, antes da geração (`app/ponto/management/commands/gerar_celulas.py:303-325,345-350`).
- **Regeneração:** lê `eh_dia_trabalho_calculado`, monta DNA e cria ausentes; para existente `origem='gerada'` divergente, altera `trabalha`, geradora e DNA e incrementa `regeneracoes`, guardando DNA anterior e instante (`app/ponto/portas/celula.py:289-311`). Não depende de a data ser futura; é a exceção formal ao passado imutável (`app/ponto/portas/celula.py:112-135`; `CLAUDE.md:149-157`). Recusa vínculo sem vigência declarada e pula competência exportada, salvo ato explícito `apesar_da_lavra` (`app/ponto/portas/celula.py:147-162,218-221,284-288`).
- **Outras escritas de trabalha:** fora da vigência, a regeneração pode zerar `trabalha`, com ramos distintos para futuro e passado (`app/ponto/portas/celula.py:229-282`); `reverter_regeneracao_dia` restaura pelo DNA anterior e incrementa carimbos (`app/ponto/portas/celula.py:817-837`); `editar_dia` grava True/False, DNA, geradora, `origem='editada'`, autor/data, exigindo autor e motivo (`app/ponto/portas/celula.py:597-602,622-647`). Edição não incrementa os campos de regeneração (`app/ponto/portas/celula.py:575-580`). Troca de plantão delega duas edições ao mesmo escritor, cedente False e substituto True (`app/ponto/portas/celula.py:670-679,706-715`).

**Caminho foto → célula:** foto mensal é gravada e relavrada na mesma transação (`app/escala/folgas.py:150-163,193-218`). `eh_dia_trabalho_calculado` consulta `FolgaDia` do vínculo/mês; data presente na foto dá False (`app/escala/models.py:1280-1288,1296-1317`). A relavra chama a regeneração para o passado e `regenerar_futuras_do_cadastro` para futuro (`app/escala/folgas.py:280-300`). O suplemento futuro seleciona apenas células **geradas já existentes**, a partir de amanhã e dentro de `_janela_default`, respeitando o dono do dia (`app/ponto/services/cadastro_futuro.py:26-45`). Essa janela é a união das competências **correntes** das empresas ativas (`app/ponto/management/commands/gerar_celulas.py:290-300`).

Assim, uma foto de dezembro pode ser gravada sem alcançar células de dezembro que estejam fora daquela janela; o dado fornecido não informa dia do ato, cortes das empresas, origem/geradora das células nem se alguma guarda barrou — o motivo concreto é `[nao verificado]`. `regeneracoes=0` não prova que a função nunca foi chamada: o incremento só ocorre no ramo de reescrita divergente (`app/ponto/portas/celula.py:297-311`).

**Portas de cadastro relevantes:** vínculo criado/atualizado copia calendário padrão e gera foto do mês corrente e seguinte (`app/colaboradores/services/vinculo.py:151-166,283-298`); depois regenera passado e futuras existentes (`app/colaboradores/services/vinculo.py:315-340`). Troca do calendário do vínculo sincroniza foto futura quando o FK muda (`app/colaboradores/services/vinculo.py:450-465`). Salvar calendário sincroniza seus vínculos (`app/escala/folgas.py:571-601`). A sincronização específica poupa passado e seleciona células futuras existentes, reconcilia foto futura mensal e regenera pela porta (`app/escala/folgas.py:605-642`). Mudança de vigência e âncora tem signals separados para passado e futuro (`app/escala/signals.py:54-83,111-134`); edição de template também propaga pela regeneração (`app/escala/services/cadastro_tipo.py:332-366`).

**(b): sim, a chamada mensal `gerar_foto_folgas` alcança passado.** Sem `datas_foto`, seleciona foto de calendário pelo mês inteiro, apaga e recria todas as datas do calendário; não há filtro `>= hoje` (`app/escala/folgas.py:180-197`). Chama relavra desde o primeiro dia do mês (`app/escala/folgas.py:212-218`), e o ramo passado regenera de `desde` até **hoje**, sem `min(ate, hoje)` (`app/escala/folgas.py:290-296`). Portanto células passadas geradas podem ser tocadas pela exceção formal, respeitadas as guardas; não são genericamente protegidas dessa chamada pela irretroatividade (`app/ponto/portas/celula.py:284-311`; `CLAUDE.md:149-157`).

## 16/11 13:24 · sessao 02f6ef787e65 · - · - · origem=Ronald (tmux)

`FolgaDia` tem módulo escritor declarado único em `app/core/portas.py:471`: `app/escala/folgas.py`.

| Porta | Escrita e chamada de célula | Entradas |
|---|---|---|
| `gerar_foto_folgas` | Abre transação e chama `_gerar_foto_folgas_no_ato` (`app/escala/folgas.py:150-163`); apaga foto de calendário do mês e recria datas, preservando manual (`app/escala/folgas.py:180-197`); chama relavra (`app/escala/folgas.py:215-218`). | Aplicar calendário ao tipo (`app/escala/views.py:376-412`); vincular folga no colaborador (`app/colaboradores/views.py:212-246`); herdar padrão do template (`app/colaboradores/services/vinculo.py:151-166`, chamadas em `:284` e `:298`); comando `lavrar_foto_folgas --apply` (`app/escala/management/commands/lavrar_foto_folgas.py:159-170`), cron em `app/config/crons.py:425`. |
| `sincronizar_foto_futura_do_calendario` | Seleciona células após hoje (`app/escala/folgas.py:621-624`), reconcilia somente datas futuras dos meses encontrados (`:634-639`) e regenera futuras (`:640-642`). | Salvar calendário (`app/escala/folgas.py:597-601`); trocar calendário do vínculo quando ID muda (`app/colaboradores/services/vinculo.py:459-465`). |
| `lancar_folga` | `get_or_create` e relavra no mesmo atomic (`app/escala/folgas.py:329-339`). | Toggle do Plano (`app/escala/views_plano_folgas.py:124-132`); declaração pela supervisão (`app/ponto/services/declarar_dia.py:179-193`). |
| `alterar_folga` | Chama `_foto_do_dia`, depois relavra (`app/escala/folgas.py:377-382`); o helper apaga, cria ou troca `tipo` (`:343-366`). | Toggle do Plano (`app/escala/views_plano_folgas.py:134-137`). |
| `aplicar_folgas_em_lote` | Chama mesmo helper e relavra os dias gravados, recusando competência anterior à corrente (`app/escala/folgas.py:401-421`). | Plano em lote (`app/escala/views_plano_folgas.py:163-185`). |
| `reparentar_folga` | Recusa calendário (`app/escala/folgas.py:484-485`); salva vínculo novo (`:515-516`) e relavra (`:523-525`). Competência passada usa `so_rejulgar`, sem relavrar DNA (`:521-525`). | Signal de nascimento/mudança de vigência (`app/escala/signals.py:185-207`), via `reparentar_folgas_do_colaborador` (`app/escala/folgas.py:529-541`); comando `reparentar_folgas --apply` (`app/escala/management/commands/reparentar_folgas.py:22-34`). |

**Resposta (b): sim, `gerar_foto_folgas` para mês corrente grava/apaga antes de hoje.** Não há corte por hoje na seleção mensal, no `delete` ou no `bulk_create` (`app/escala/folgas.py:180-197`). A célula passada também é encaminhada à regeneração formal: `relavrar_celulas_da_foto` chama `regenerar_celulas_vinculo(ec, desde, hoje, estender_horizonte=False)` (`app/escala/folgas.py:290-296`). Portanto, a irretroatividade não impede toda alteração: a porta é a exceção formal para passado errado por cadastro (`app/ponto/portas/celula.py:112-119`; `CLAUDE.md:149-156`).

As guardas concretas da regeneração são vínculo sem vigência declarada (`app/ponto/portas/celula.py:153-162`), competência exportada (`:218-221`, `:284-288`), e origem humana: atualização normal exige `cel.origem == 'gerada'` (`:297-311`). Futuro alcançado pela relavra passa pelo suplemento que seleciona células geradas já existentes dentro da janela e do dono correto (`app/ponto/services/cadastro_futuro.py:26-45`).

**Atenção à diferença entre portas:** salvar/trocar calendário usa primeiro a sincronização futura, que preserva passado (`app/escala/folgas.py:621-639`; `app/colaboradores/services/vinculo.py:463-465`), mas as views `vincular_folga` e `aplicar_folga_calendario` também chamam depois `gerar_foto_folgas` para mês corrente e próximo, sem esse corte (`app/colaboradores/views.py:243-246`; `app/escala/views.py:411-412`).

No caso dezembro/FolgaDia existente/célula `True`/regenerações zero, o caminho esperado quando a foto chega depois da célula é `gerar_foto_folgas → relavrar_celulas_da_foto → regenerar_futuras_do_cadastro → regenerar_celulas_vinculo` (`app/escala/folgas.py:215-218`, `:297-299`; `app/ponto/services/cadastro_futuro.py:26-45`). O fato de esse caminho ter sido executado, a data da execução, a janela naquele instante, origem da célula, vigência/dono e eventual bloqueio não são demonstrados pelo dado fornecido: `[nao verificado]`. A divergência normal do ramo principal, quando atualizada, incrementa `regeneracoes` (`app/ponto/portas/celula.py:297-311`), portanto zero sozinho não prova que a função nunca foi chamada.

## 16/11 13:22 · sessao 7e002ba200c0 · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

**AVAL-DEVOLVIDO — item 1: corrigir a exclusividade da função escritora e fechar o aviso/RED literal; item 2: falta a relação individual raspada.** (`app/escala/folgas.py:659`; `app/templates/core/_dialogo.html:130`; `app/escala/tests/test_folga_cadastro.py:30`; relação individual: [nao verificado]).

**Item 1 — editor**

1. **Premissas: PROCEDE, exceto “única porta = função salvar”.**

   - **PROCEDE:** no modo `dias_mes`, aparece uma grade por vez, começando pela de 31: a view ordena 31/30/28, o JS começa com `idx = 0` e substitui o conteúdo de um único `fc-grade`. (`app/escala/views.py:318`; `app/templates/escala/folga_calendario_editar.html:45`; `app/templates/escala/folga_calendario_editar.html:74`; `app/templates/escala/folga_calendario_editar.html:86`).
   - **PROCEDE:** salva com outras grades vazias sem confirmação específica: o formulário submete normalmente; campos ausentes viram listas vazias, entregues à gravação. (`app/templates/escala/folga_calendario_editar.html:9`; `app/templates/escala/folga_calendario_editar.html:59`; `app/escala/views.py:342`; `app/escala/views.py:349`; `app/escala/views.py:356`).
   - **PROCEDE com precisão:** cada `FolgaCalendarioTamanho` guarda **uma** grade; o conjunto comporta 28/30/31, com unicidade por calendário/tamanho. O modelo não exige três registros preenchidos. (`app/escala/models.py:1563`; `app/escala/models.py:1565`; `app/escala/models.py:1570`; `app/escala/models.py:1575`).
   - **NAO PROCEDE a exclusividade da função:** `salvar_calendario_de_folga` atualiza/cria grades; `criar_calendario_de_folga` também as cria. A porta canônica registrada é o **módulo** `escala/folgas.py`. A função salvar também exclui os tamanhos quando muda para `semana_dia`. (`app/escala/folgas.py:594`; `app/escala/folgas.py:659`; `app/core/portas.py:473`; `app/escala/folgas.py:588`).
   - **PROCEDE o criador rápido:** o wizard chama `criar_calendario_de_folga`; o descarte `d <= tam` acontece **no servidor**. (`app/escala/views_wizard.py:192`; `app/escala/folgas.py:658`; `app/escala/folgas.py:661`).
   - **PROCEDE contexto e POST:** `editar_folga_calendario` monta o contexto; o JS serializa os dias selecionados de todos os tamanhos, e a view monta explicitamente as três listas antes de chamar salvar. Grades vazias chegam por ausência de campos, convertida em `[]`. (`app/escala/views.py:362`; `app/escala/views.py:369`; `app/templates/escala/folga_calendario_editar.html:108`; `app/escala/views.py:340`; `app/escala/views.py:356`).

2. **Colisão: NAO PROCEDE bloqueio demonstrado por O148; PROCEDE sobreposição do editor.**

   O O148-EDITAR contempla essa edição, mas consta como **só registrar**, com versionamento aguardando corte; aparece em LEIS entre atos, não como lei vigente. Isso não prova reserva de arquivo em obra ativa. Concorrência efetiva: [nao verificado]. (`app/docs/CORTES.md:101`; `app/docs/CORTES.md:357`; `app/docs/LEIS.md:206`; `app/docs/LEIS.md:313`).

   As leis aplicáveis exigem mesma autoridade para tela/ação, ajuda pelo componente e nenhum diálogo nativo; não demonstram proibição da projeção proposta. (`app/docs/LEIS-UI.md:23`; `app/docs/LEIS-UI.md:77`; `app/docs/LEIS-UI.md:87`).

3. **Condições literais: PROCEDE ressalva.**

   - **Repetir sem `static/js` novo: PROCEDE a viabilidade.** O template já contém JS inline, seleções por tamanho e sincronização dos campos. É possível copiar seleções na tela, filtrando pelo tamanho, e continuar submetendo pela mesma view; implementação concreta: [nao verificado]. (`app/templates/escala/folga_calendario_editar.html:70`; `app/templates/escala/folga_calendario_editar.html:73`; `app/templates/escala/folga_calendario_editar.html:108`).
   - **Componente da casa: PROCEDE sua existência, mas não o par literal pronto.** É `core/_dialogo.html`, incluído pela base, com `hxConfirmar`. Aceita “Salvar assim mesmo” como verbo; o negativo está fixo em **“Cancelar”**, sem parâmetro para “Voltar”. Fechar esse literal apenas pela API atual não procede; adaptação dentro do escopo ainda é [nao verificado]. (`app/templates/base.html:388`; `app/templates/core/_dialogo.html:172`; `app/templates/core/_dialogo.html:191`; `app/templates/core/_dialogo.html:130`).
   - **RED do POST: NAO PROCEDE como vermelho da cura visual.** Já existe teste que envia valores distintos nas três grades e confere a persistência; criar e editar usam a mesma view. O teste não observa explicitamente a chamada à porta. Para provar “pela porta”, cabe observar a função real e conferir persistência; isso reforça o contrato existente. O RED da cura deve morder na marca/aviso ausentes, com completo sem aviso. Execução dos testes: [nao verificado]. (`app/escala/tests/test_folga_cadastro.py:18`; `app/escala/tests/test_folga_cadastro.py:30`; `app/escala/urls.py:35`; `app/escala/urls.py:36`; `app/escala/views.py:356`).

4. **Alcance: PROCEDE escrita ao salvar; extrapolação financeira/exportada não demonstrada.**

   Salvar já atualiza as grades, sincroniza fotos futuras dos vínculos e regenera células futuras com `recalculo=False`. Portanto, o ato do admin produz escrita além da definição das grades, pelo caminho existente. (`app/escala/folgas.py:594`; `app/escala/folgas.py:600`; `app/escala/folgas.py:639`; `app/escala/folgas.py:640`).

   O caminho seleciona datas posteriores a hoje. Impacto real em dinheiro/exportada, migração nova ou necessidade de `!` fora do declarado: [nao verificado]. A lei exige `!` para gravado exportado e alterações de escala/vínculo fora da autorização literal. (`app/escala/folgas.py:623`; `app/ponto/services/cadastro_futuro.py:27`; `CLAUDE.md:265`).

**Item 2 — lista de trabalho**

1. **Premissas/dado suficiente: NAO PROCEDE.** O raspado fornecido contém totais e menciona calendários 51/154, mas não entrega a relação calendário → grades → colaboradores → quantidade de usos. A lista completa e sua ordenação não podem ser conferidas ou reconstruídas desse material. [nao verificado].

2. **Colisão com lei/O148: NAO PROCEDE colisão demonstrada.** Listar pendências de cadastro para o admin é compatível com L-099; O148-EDITAR trata edição/versionamento, exclusão e busca. (`CLAUDE.md:179`; `app/docs/CORTES.md:357`).

3. **Condição que não fecha: PROCEDE falta de evidência individual.** A lista existente no código ordena por nome e coleta todos os vínculos, sem filtro de atividade nesses acessos; não comprova a lista pedida “em uso, ordenada por uso”. Falta o resultado raspado individual da sonda. (`app/escala/views.py:286`; `app/escala/views.py:288`; `app/escala/views.py:300`; resultado individual: [nao verificado]).

4. **Alcance: NAO PROCEDE escrita de negócio demonstrada; há escrita operacional no envelope.** O envelope prevê role/grants e arquivo de ambiente, além de acrescentar trilha em log. O wrapper executa a sonda antes do Codex e raspa a saída. Execução específica, grants reais e efeitos desta sonda: [nao verificado]. (`bin/sonda_leitura.sh:28`; `bin/sonda_leitura.sh:248`; `bin/codex_pergunta.sh:328`; `bin/codex_pergunta.sh:334`).

VEREDITO: DEVOLVE <1: perguntas 1 e 3; 2: perguntas 1 e 3>

## 16/11 13:20 · sessao db8b5d6bc7f1 · - · - · origem=Ronald (tmux)

- **“Salvar é a única função escritora” — NÃO PROCEDE.** `salvar_calendario_de_folga` faz `update_or_create` das grades e as apaga ao trocar para `semana_dia`; `criar_calendario_de_folga` também cria as grades. A porta canônica declarada é o **módulo** `escala/folgas.py`, não exclusivamente a função salvar (`app/escala/folgas.py:588`, `app/escala/folgas.py:594`, `app/escala/folgas.py:645`, `app/escala/folgas.py:659`, `app/core/portas.py:473`).

- **Criador rápido — PROCEDE; corte no servidor.** `wizard_calendario_criar` recebe dias, valida `1..31` e chama `criar_calendario_de_folga`; esta percorre `(31,30,28)` e grava somente `d <= tam` (`app/escala/views_wizard.py:140`, `app/escala/views_wizard.py:179`, `app/escala/views_wizard.py:188`, `app/escala/views_wizard.py:192`, `app/escala/folgas.py:658`, `app/escala/folgas.py:661`).

- **Fonte — PROCEDE com precisão:** cada registro de `FolgaCalendarioTamanho` guarda uma grade; o calendário pode ter registros de 28,30,31, exclusivos por calendário/tamanho; não existe exigência de que os três estejam presentes ou preenchidos (`app/escala/models.py:1563`, `app/escala/models.py:1565`, `app/escala/models.py:1570`, `app/escala/models.py:1575`). `dias_por_tamanho()` lê as linhas existentes, sem completar grades ausentes (`app/escala/models.py:1558`).

- **Contexto e POST — PROCEDE.** A view define 31,30,28, lê os marcados, monta os três elementos e entrega o contexto ao editor (`app/escala/views.py:318`, `app/escala/views.py:362`, `app/escala/views.py:369`). No POST `dias_mes`, percorre os três tamanhos, transforma campos ausentes em listas vazias, valida o intervalo e passa o dicionário completo à porta (`app/escala/views.py:340`, `app/escala/views.py:342`, `app/escala/views.py:349`, `app/escala/views.py:356`).

- **Censo de leitores diretos:** `dias_por_tamanho()` é consumido em produção por `datas_que_o_calendario_manda`, que delega a `folgas_do_mes`; este escolhe o tamanho real, usando 28 para fevereiro bissexto, e devolve vazio quando a grade falta/está vazia (`app/escala/folgas.py:56`, `app/escala/folgas.py:12`, `app/escala/folgas.py:14`). A função de datas alimenta divergência, vigia, geração de foto e sincronização futura (`app/escala/folgas.py:99`, `app/escala/folgas.py:127`, `app/escala/folgas.py:158`, `app/escala/folgas.py:629`). Lista e editor também leem diretamente `cal.tamanhos` (`app/escala/views.py:292`, `app/escala/views.py:362`).

- **RED do POST: a obrigação já existe no código e em teste.** `test_post_cria_calendario_e_3_tamanhos` envia valores distintos nos três campos e confere o dicionário persistido; criar e editar usam a mesma view (`app/escala/tests/test_folga_cadastro.py:18`, `app/escala/tests/test_folga_cadastro.py:30`, `app/escala/urls.py:35`, `app/escala/urls.py:36`). O teste de porta também confere as três grades e idempotência (`app/escala/tests/test_porta_escala.py:95`). Logo um teste apenas dessa condição **não evidencia RED da mudança visual**, por inspeção desses caminhos; resultado de execução é [nao verificado]. Para testemunhar “pela porta”, uma verificação adicional precisaria observar a chamada da função real sem substituir a persistência; seria reforço de contrato, não o RED visual (`app/escala/views.py:354`, `app/escala/views.py:356`, `app/escala/folgas.py:594`).

- **Outros escritores em testes, separados de produção:** preparações com `objects.create` em `app/colaboradores/tests/test_vincular_folga_view.py:31`, `app/colaboradores/tests/test_vincular_folga_view.py:35`, `app/colaboradores/tests/test_vinculo_folga_padrao.py:25`, `app/escala/tests/test_folga_semana_dia.py:65`, `app/escala/tests/test_torneira_folgas.py:99`, `app/escala/tests/test_folga_models.py:11`, `app/escala/tests/test_gerador_folgas.py:32`, `app/escala/tests/test_gerador_folgas.py:34`, `app/escala/tests/test_smoke_clique_folga_calendario.py:112`, `app/escala/tests/test_editor_folga_modo.py:29`, `app/escala/tests/test_folga_aplicar.py:37`, `app/escala/tests/test_foto_folga_continua.py:64`, `app/escala/tests/test_selo_foto_folgas_relavra.py:46`; alteração por instância em `app/escala/tests/test_foto_folga_continua.py:145` e `app/escala/tests/test_foto_folga_continua.py:146`. A migração encontrada cria o esquema, sem carga de dados (`app/escala/migrations/0015_folgacalendario_folgacalendariotamanho.py:29`). A guarda de arquitetura proíbe escritores de produção fora da porta, com allowlist vazia (`app/escala/tests/test_chokepoint_escala_gate.py:27`, `app/escala/tests/test_chokepoint_escala_gate.py:55`).

## 16/11 13:20 · sessao 099ae93ca4b9 · - · - · origem=Ronald (tmux)

- **PROCEDE — item 1 alcança o editor previsto na O148.** O complemento histórico nomeia `editar_folga_calendario` como “EDITAR-NAO-PUBLICA”; O148-EDITAR contempla edição com uso por versão. Referências: `app/docs/O148-COMPLEMENTO.md:15`, `app/docs/CORTES.md:357`.

- **NAO PROCEDE — afirmar que versionamento já seja lei vigente ou que exista reserva de arquivo em obra ativa.** Os registros dizem “só registrar”, “não constrói agora” e “edição por versão espera corte de lei”. O148-EDITAR aparece na seção “CORTES que são ATO, não lei”. Referências: `app/docs/CORTES.md:101`, `app/docs/CORTES.md:357`, `app/docs/BACKLOG.md:819`, `app/docs/LEIS.md:206`, `app/docs/LEIS.md:313`. Concorrência efetiva no mesmo arquivo: **[nao verificado]**.

- **NAO PROCEDE — conflito necessário com leis de UI.** O aviso pode seguir componente da casa, sem diálogo nativo, e consumir o mesmo estado usado pela ação; essas são as exigências aplicáveis. Referências: `app/docs/LEIS-UI.md:23`, `app/docs/LEIS-UI.md:77`, `app/docs/LEIS-UI.md:87`. A raia normalmente permite templates/static/testes; a exceção nominal da view consta apenas no anexo fornecido **[nao verificado na cópia]**; regra geral: `CLAUDE.md:312`.

- **Achado material — a porta declarada não é o único escritor.** `criar_calendario_de_folga` também cria `FolgaCalendarioTamanho`; o descarte acima do tamanho ocorre no servidor. Referências: `app/escala/folgas.py:645`, `app/escala/folgas.py:658`, `app/escala/folgas.py:661`.

- **Achado material — salvar já produz efeitos além da definição das grades.** A porta sincroniza fotos futuras dos vínculos e chama regeneração futura pela porta da célula. A descrição histórica “grava e redireciona sem rematerializar” está superada nesse ponto. Referências: `app/escala/folgas.py:600`, `app/escala/folgas.py:639`, `app/escala/folgas.py:640`; descrição histórica: `app/docs/O148-COMPLEMENTO.md:15`.

## 16/11 12:57 · sessao 24f76b5f4000 · UI-VINCULO-MOSTRA-O-CALENDARIO · verificador · origem=Code

1) **NAO PROCEDE a objeção às premissas.**

- A fonte existe, recebe `(cal, ano, mes)` e concentra a escolha entre os dois modos; os resolvedores ficam abaixo dela (`app/escala/folgas.py:43`, `app/escala/folgas.py:54`). Há selo contra chamadas dos resolvedores fora desse módulo (`app/escala/tests/test_foto_folga_continua.py:281`).
- Leitores diretos encontrados: `divergencia_foto_x_calendario` (`app/escala/folgas.py:99`), `calendario_com_template_vazio_no_mes` (`app/escala/folgas.py:127`), `gerar_foto_folgas` (`app/escala/folgas.py:158`) e `sincronizar_foto_futura_do_calendario` (`app/escala/folgas.py:629`).
- A rota recebe `cid` inteiro e a view busca `FolgaCalendario.pk=cid` (`app/escala/urls.py:36`, `app/escala/views.py:319`).
- O bloco e a caixa existem. O texto vazio é **“— usar as folgas da escala (padrão do template) —”**, conforme a descrição do aval; não é literalmente “sem calendario” (`app/templates/colaboradores/partials/painel_vinculo.html:186`, `app/templates/colaboradores/partials/painel_vinculo.html:197`).
- `painel_vinculo` escolhe o primeiro vínculo ativo, fornece os calendários e renderiza o partial. A opção selecionada compara o calendário com **`escala_ativa.folga_calendario_id`**, não com o padrão do TipoEscala (`app/colaboradores/views.py:268`, `app/colaboradores/views.py:295`, `app/colaboradores/views.py:297`, `app/templates/colaboradores/partials/painel_vinculo.html:199`).

2) **NAO PROCEDE colisão no escopo declarado.**

- Ler essa fonte atende à testemunha que não recalcula e ao juiz único da tela (`app/docs/LEIS.md:60`, `app/docs/LEIS-UI.md:23`).
- O grep encontrou referências a outro sítio de `colaboradores/views.py`, a `reparentar_folga` e à porta de troca do calendário; nenhuma exige alteração nesta projeção (`app/docs/LEIS.md:64`, `app/docs/CORTES.md:349`, `app/docs/LEIS.md:189`). Outra referência específica impeditiva: `[nao verificado]`.
- O148-EDITAR permanece **“SO REGISTRAR”**, com edição por versão aguardando corte; abrir a edição existente não implementa esse complemento (`app/docs/CORTES.md:356`, `app/escala/views.py:369`).
- A exceção nominal do anexo cobre o contexto `.py` diante da restrição geral da raia UI (`CLAUDE.md:312`); registro dessa exceção em arquivo: `[nao verificado]`.
- Comparação com a lista BUG-PAR fornecida: nenhum arquivo em comum com os dois alvos declarados; confirmação externa dos arquivos efetivamente em voo: `[nao verificado]`.

3) **NAO PROCEDE impedimento literal; há cuidados obrigatórios de implementação.**

- A fonte aceita cada mês separadamente, sem restringir corrente/próximo; a decisão dos dias permanece integralmente nela (`app/escala/folgas.py:43`, `app/escala/folgas.py:55`).
- **Somente contexto inicial deixaria a projeção velha ao trocar a caixa:** não há atualização no select, o POST usa `hx-swap="none"` e retorna apenas toast (`app/templates/colaboradores/partials/painel_vinculo.html:193`, `app/templates/colaboradores/partials/painel_vinculo.html:195`, `app/colaboradores/views.py:251`). Isso cabe resolver nos alvos autorizados, projetando pela fonte os calendários já fornecidos e alternando sua apresentação no partial (`app/colaboradores/views.py:295`, `app/escala/folgas.py:43`).
- O RED pode comparar HTML com a fonte real; há precedente de GET e inspeção do painel (`app/colaboradores/tests/test_agenda_14dias.py:39`, `app/colaboradores/tests/test_agenda_14dias.py:44`). Para morder derivação paralela, deve também substituir o retorno da fonte por datas sentinela e exigir sua exibição. Execução desse RED/mutante: `[nao verificado]`.
- Ajuda por componente e ausência de diálogo nativo continuam exigíveis (`app/docs/LEIS-UI.md:77`, `app/docs/LEIS-UI.md:87`).

4) **NAO PROCEDE alcance adicional necessário.**

A projeção chama um leitor que devolve datas; abrir a edição por GET não entra no ramo de escrita (`app/escala/folgas.py:52`, `app/escala/views.py:320`, `app/escala/views.py:362`). Não exige alterar as portas que trocam vínculo ou materializam folgas (`app/colaboradores/views.py:239`, `app/colaboradores/views.py:245`). O `!` protege mudança do **dado** de escala/vínculo e do gravado de exportada (`CLAUDE.md:265`). Patch, migração, efeitos financeiros e cumprimento posterior desses limites: `[nao verificado]`.

VEREDITO: EXECUTA

## 16/11 12:55 · sessao 01ed894fced5 · - · - · origem=Ronald (tmux)

Não identifiquei impedimento intrínseco no literal; há uma armadilha de atualização que deve entrar no RED.

- A fonte aceita `(cal, ano, mes)` e retorna `set[date]`; resolve os modos internamente e aceita calendário ausente. Pode ser chamada duas vezes, inclusive na virada de ano, sem derivar folgas fora dela (`app/escala/folgas.py:43`, `app/escala/folgas.py:52`, `app/escala/folgas.py:54`).
- A seleção inicial vem de `escala_ativa.folga_calendario_id`; o vazio já tem o texto “usar as folgas da escala (padrão do template)”. A expressão literal “sem calendario” está na resposta do escritor, não nessa opção (`app/templates/colaboradores/partials/painel_vinculo.html:197`, `app/templates/colaboradores/partials/painel_vinculo.html:199`, `app/colaboradores/views.py:250`).
- Uma projeção estática ficaria velha ao mudar/salvar: o select não tem atualização e o formulário usa `hx-swap="none"`; o POST HTMX devolve corpo vazio e toast. O carregamento existente só se repete automaticamente após swap da linha (`app/templates/colaboradores/partials/painel_vinculo.html:193`, `app/templates/colaboradores/partials/painel_vinculo.html:195`, `app/colaboradores/views.py:251`, `app/templates/colaboradores/vincular.html:165`).
- Isso pode ser resolvido nos locais autorizados: a view já disponibiliza todos os calendários; o contexto pode acrescentar os dois retornos da fonte por calendário, e o partial alternar a projeção e o link conforme a seleção, sem calcular folgas. Viabilidade por leitura: `app/colaboradores/views.py:295`, `app/colaboradores/views.py:298`, `app/escala/folgas.py:43`; implementação e desempenho `[nao verificado]`.
- Para o mutante, apenas comparar datas reais não prova dependência da fonte. Proposta: substituir temporariamente o retorno da fonte por datas sentinela durante o GET e exigir essas datas no HTML e chamadas para os dois meses; execução desse RED `[nao verificado]`. O GET do painel já possui precedente de teste com `Client` e inspeção do HTML/contexto (`app/colaboradores/tests/test_agenda_14dias.py:39`, `app/colaboradores/tests/test_agenda_14dias.py:44`).

## 16/11 12:55 · sessao a627924a613d · - · - · origem=Ronald (tmux)

- **Pergunta 2: NAO PROCEDE, no escopo literal do item 1.** Ler a fonte única segue L-002 (`app/docs/LEIS.md:60`) e L2 de UI (`app/docs/LEIS-UI.md:23`). A regra geral de `wt-ui` limita a templates/static/teste (`CLAUDE.md:309`–313); a exceção nominal para contexto na view consta somente do anexo fornecido `[nao verificado em arquivo]`.

- Busca pelos arquivos/funções em `LEIS.md`/`CORTES.md`: `colaboradores/views.py` aparece por **form_empresa**, outro sítio (`app/docs/LEIS.md:64`); `escala/folgas.py` por **reparentar_folga**, outro predicado (`app/docs/CORTES.md:349`). Não encontrei referências exatas a `painel_vinculo`, `editar_folga_calendario`, `escala/urls.py` ou `vincular_folga` nesses dois documentos `[nao verificado: ausência não possui linha positiva]`.

- **O148-EDITAR não exige implementar versões nesta obra.** Está expressamente em “SO REGISTRAR”, com edição por versão aguardando corte (`app/docs/CORTES.md:356`). Item 1 pode apenas apontar à edição existente: GET busca o calendário e renderiza (`app/escala/views.py:319`, `app/escala/views.py:362`, `app/escala/views.py:369`); a escrita fica no ramo POST (`app/escala/views.py:320`, `app/escala/views.py:356`).

- **Pergunta 4: NAO PROCEDE se restrita a exibição/contexto/link.** `!` protege alteração de **dado** de escala/vínculo e gravado de exportada (`CLAUDE.md:265`–268); L-007 protege escritores e trilha (`app/docs/LEIS.md:65`, `app/docs/LEIS.md:189`). O resolver devolve datas sem persistir (`app/escala/folgas.py:43`–56).

- **Ponto para pergunta 3:** a caixa atual não dispara requisição ao mudar; `selected` compara o calendário **salvo** do vínculo (`app/templates/colaboradores/partials/painel_vinculo.html:195`–201). Apenas contexto inicial acompanha esse calendário salvo; acompanhar seleção ainda não salva exigiria mecanismo adicional `[nao verificado: intenção exata do anexo]`.

- O texto existente para ausência é exatamente “— usar as folgas da escala (padrão do template) —”, não a expressão “sem calendário” (`app/templates/colaboradores/partials/painel_vinculo.html:197`).

## 16/11 12:48 · sessao 680469d3a22e · O10-D-PERMUTA-RECIPROCA · censo · origem=Code

A conferência preserva o contrato do construtor: provocar rollback ao receber `'recusada'`, ordenar as quatro células e permitir `d1=d2`. A correção relevante é retirar a suposição do censo de que as datas precisam ser distintas. (`app/docs/RELATO.md:1640`; `app/docs/BACKLOG.md:2177`)

Os anexos abaixo são referenciados pela numeração do **texto colado**, contando linhas vazias e começando após o delimitador de cada anexo. Distingo as matrizes homônimas como `matriz_F2.md` e `matriz_F4.md`. Identidade textual com o commit histórico e vínculo Git da cópia ao hash informado: [nao verificado]. [nao verificado]

**1. Conferência do censo**

Para encurtar a tabela, `P` significa `app/ponto/portas/celula.py` e `G` significa `app/ponto/management/commands/gerar_celulas.py`. “MOVEU” significa que localizei o comportamento descrito em outra linha; comparação textual com a versão histórica: [nao verificado].

| Afirmação | Linha no censo e referências antigas | Arquivo:linha hoje | Resultado |
|---|---|---|---|
| A porta captura a recusa; duas chamadas dentro de um `atomic` externo não garantem rollback integral | `censo_v2.r:1`; P:652,694 | P:690,732,737 | **MOVEU**; comportamento preservado. |
| Cartas travadas por `pk`, exclusivamente em `Ausencia`; células ordenadas por colaborador/data e travadas dentro de `editar_dia` | `censo_v2.r:5`; P:657,673,593 | P:695,711,631 | **MOVEU**. |
| Ordenação é por chamada, não pelas quatro células; composição pode produzir espera circular | `censo_v2.r:7`; P:657,673 | P:695,711 | **MOVEU**; permanece dedução estática. Deadlock ocorrido: [nao verificado]. |
| Teste citado inspeciona AST, sem executar concorrência | `censo_v2.r:9`; teste:314 | `app/ponto/tests/test_o10b_editar_dia_troca.py:314`, :319, :322 | **IGUAL**. Ensaio concorrente da composição: [nao verificado]. |
| Cada edição consulta empresa/lavra antes de decidir `'igual'`; existe bypass `apesar_da_lavra` | `censo_v2.r:11`; P:513,568,594,676 | P:551,606,632,714 | **MOVEU**. |
| Não existe pré-validação das quatro; consulta seguinte acontece depois da edição anterior | `censo_v2.r:13`; P:675 | P:713 | **MOVEU**. |
| Consulta da exportação não trava seus registros; TOCTOU documentado | `censo_v2.r:13`; G:161; BACKLOG:1876 | G:170; `app/docs/BACKLOG.md:2176` | **MOVEU**. |
| Carimbo individual por edição; quatro trilhas individuais e duas do par, quando quatro alterações acontecem | `censo_v2.r:15`; P:598,611,686 | P:636,649,724 | **MOVEU**. |
| Deduplicação do recálculo é local; quatro alterações podem agendar quatro recálculos | `censo_v2.r:19`; P:450,458,627; fechamento:1824 | P:488,496,665; `app/ponto/services/fechamento.py:1824` | **MOVEU** na porta; **IGUAL** em fechamento. |
| Callback por alteração contém rejunção e reavaliação; rejunção exige FK nula; reavaliação recorta vivos/dias | `censo_v2.r:20`; P:490,502; demais:60,17,76 | P:528,540; `app/chamados/rejuncao.py:60`, :62; `app/chamados/services/feriado_reavaliar.py:17`, :76 | **MOVEU** na porta; demais sítios **IGUAIS**. |
| Rejulgamento síncrono pode observar composição incompleta; pula futuro/humano e captura falhas | `censo_v2.r:21`; P:427,434,437,627 | P:465,472,475,665 | **MOVEU**. |
| Marca órfã ocorre na transação, consulta carimbos e atualiza contexto | `censo_v2.r:22`; P:625; G:241,255 | P:663,664; G:250,264 | **MOVEU**. |
| Segunda chamada relê cartas/colaboradores; edição relê célula e vínculo | `censo_v2.r:26`; P:656,657,593; alimentação:355 | P:694,695,631; `app/escala/alimentacao.py:355`, :358 | **MOVEU** na porta; alimentação **IGUAL**. |
| Cartório relê célula; identity map é limitado às instâncias recebidas e restaurado ao sair | `censo_v2.r:28`; cartório:1976; alimentação:43,47 | `app/ponto/services/cartorio.py:2027`; `app/escala/alimentacao.py:43`, :47 | Cartório **MOVEU**; mapa **IGUAL**. |
| Um lado `'igual'` não impede o outro; só retorna `'igual'` quando ambos iguais; lado igual não escreve seus efeitos | `censo_v2.r:30`; P:594,596,673,679,690 | P:632,634,711,717,728 | **MOVEU**. |
| Igualdade é do estado da célula; não considera novo pedido, carta, motivo ou autor | `censo_v2.r:32`; P:594,675 | P:632,713 | **MOVEU**. |
| Segunda recusa, seguida de saída normal do externo, deixa a primeira elegível ao commit | `censo_v2.r:36`; P:652,694 | P:690,732,737 | **MOVEU**; dedução da captura fora do `atomic` interno. | [nao verificado]
| Rollback externo alcança escritas transacionais e auditoria; callbacks dependem do commit externo | `censo_v2.r:37`; observ:133; P:625,434,502; fechamento:1802 | `app/core/observ.py:133`; P:663,472,540; `app/ponto/services/fechamento.py:1802`, :1824 | Porta **MOVEU**; demais sítios **IGUAIS**. |
| Rejulgamento pode alcançar abertura de cobrança e push síncrono, que rollback SQL não recolhe | `censo_v2.r:39`; cartório:1740; detector:778; push:40 | `app/ponto/services/cartorio.py:1791`; `app/ponto/management/commands/detectar_ausencias.py:778`, :779; `app/chamados/push_utils.py:40` | Cartório **MOVEU**; demais **IGUAIS**. Envio concreto: [nao verificado]. |
| Helper de disponibilidade declara não validar 11h | `censo_v2.r:45`; utils:152 | `app/ponto/utils.py:152`, :153 | **IGUAL**. Existência de outro validador completo pronto: [nao verificado]. |
| `marcos_turno` e `fim_turno` fornecem insumos temporais, incluindo meia-noite | `censo_v2.r:46`; jornada:61,89 | `app/escala/servico_jornada.py:69`, :88, :97 | **MOVEU**. Juiz completo da colisão: [nao verificado]. |
| Permissão por ação e por departamento do chamado existe separadamente | `censo_v2.r:47`; mixins:322; utils:68 | `app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68` | **IGUAL**. Ação específica da permuta e composição contextual das quatro: [nao verificado]. |
| Lavra já possui juiz separado K9; helpers enumeram competências e produzem motivo | `censo_v2.r:48`; export:589; juízes:322; G:126,208 | `app/folha/export.py:597`; `app/core/juizes.py:353`; G:135,217 | **MOVEU**. |
| Portas não chamam permissão, 11h ou colisão; lavra já está separada | `censo_v2.r:50`; P:559,568,646,675 | P:597,606,684,713 | **MOVEU**. |
| `ausencias_sobrepostas` responde sobre períodos de `Ausencia` | `censo_v2.r:52`; turnos:2795 | `app/ponto/turnos.py:2817`, :2839 | **MOVEU**; não é veredito de colisão horária entre plantões. |
| `evento` aceita antes/depois; auditoria tem objeto inteiro e JSONs, sem coluna própria de correlação | `censo_v2.r:56`; observ:94,133; models:298,300 | `app/core/observ.py:94`, :133; `app/core/models.py:298`, :300, :301 | **IGUAL**. |
| `evento_kw` encaminha somente os campos conhecidos | `censo_v2.r:58`; observ:142 | `app/core/observ.py:142`, :146 | **IGUAL**; argumento adicional isolado não vira metadado persistido. |
| Precedente `lote='arquivar_lote'` é rótulo fixo, incorporado ao JSON | `censo_v2.r:60`; views:146; arquivo:133 | `app/chamados/views_arquivar.py:146`; `app/chamados/services/arquivo.py:133`, :134 | **IGUAL**. Identificador exclusivo de permuta já existente: [nao verificado]. |
| `trocar_plantao` não recebe ID de permuta; audita carta e dois lados; FK de origem não é copiada | `censo_v2.r:62`; P:632,682,686; models:786 | P:670,720,724,728; `app/ponto/models.py:786` | Porta **MOVEU**; FK **IGUAL**. |
| Para quatro coordenadas distintas, pessoas/datas vêm das quatro cartas relidas | `censo_v2.r:66`; P:659,673 | P:697,711,713 | **MOVEU**; afirmação permanece condicional a coordenadas distintas. |
| Exige cartas presentes/encontradas, colaboradores distintos e documento preenchido; não compara nem abre arquivos | `censo_v2.r:73`; P:653,660,662,664,684 | P:691,698,700,702,722 | **MOVEU**. |
| Não existe consumo/exclusividade global da carta, nem exigência de datas iguais dentro de uma chamada | `censo_v2.r:75`; P:656,673 | P:694,711,713 | **MOVEU**. |
| Tipo/status/período/pedido não participam da elegibilidade documental atual; lavra/vínculo são separados | `censo_v2.r:77`; P:664,675,568,584 | P:702,713,606,622 | **MOVEU**. |
| “BACKLOG:1879 trata BO-0511-G2-TRIAGEM” | `censo_v2.r:79` | `app/docs/BACKLOG.md:1879`, :2181; obra em :2177 | **MUDOU** o endereço físico: :1879 trata H12; BO está em :2181; O10-D em :2177. |
| Quatro células “exigem duas datas distintas” | `censo_v2.r:81` | `app/docs/RELATO.md:1640`; `app/docs/PROMPTS.md:2015` | **MUDOU**: lei permite `d1=d2`. |
| Universo das 11h permaneceu pergunta aberta | `censo_v2.r:85` | `aval_lei_permuta.txt:1`; `app/docs/RELATO.md:1640` | **MUDOU**: previsto anterior → assumido → seguinte, para ambos, após permuta. |
| Empresa/vínculo/tipos de escala permaneceram pergunta aberta | `censo_v2.r:86`; P:584,673 | P:622,711; `aval_lei_permuta.txt:1` | Código **MOVEU**; pergunta **RESPONDIDA**: mesma empresa, ativos/vigentes nas duas datas; escalas diferentes elegíveis sob as guardas. |
| Elegibilidade das cartas permaneceu pergunta aberta | `censo_v2.r:87`; P:664 | P:702; `aval_lei_permuta.txt:1` | Código **MOVEU**; pergunta **RESPONDIDA**: ato permuta aprovado, dia exato, ligado ao ID. |
| `folga_trabalhada` permaneceu pergunta aberta para a permuta | `censo_v2.r:88`; RELATO:449; BACKLOG:1876 | `app/docs/RELATO.md:2120`, :1640; `app/docs/BACKLOG.md:2176`; `aval_lei_permuta.txt:1` | Registro histórico **MOVEU**; conclusão **MUDOU**: permuta formal é compensação, com guarda de DSR. |

Nenhum dos comportamentos de código citados foi classificado como **SUMIU**; as mudanças substantivas encontradas são documentais e de interpretação da lei. (`app/ponto/portas/celula.py:670`; `app/docs/RELATO.md:1640`)

Sobre o contrato:

- **Rollback permanece obrigatório no compositor.** A recusa de negócio é capturada e devolvida como `'recusada'`; erros de autor/motivo e falha da trilha ainda podem propagar exceções. (`app/ponto/portas/celula.py:684`; :686; :730; :732; :737)
- **Ordenar as quatro permanece necessário.** A porta ordena somente os dois lados da chamada e trava cartas antes das células; o desenho da composição precisa considerar ambas as aquisições. (`app/ponto/portas/celula.py:695`; :711; `app/docs/RELATO.md:1640`)
- **`d1=d2` precisa ser aceito.** Nesse caso, duas chamadas inversas revisitam as mesmas duas coordenadas; `editar_dia` recebe um booleano e monta DNA pelo vínculo individual. Representar a troca dos turnos nesse caso é pendência técnica, não autorização para proibir datas iguais. (`app/ponto/portas/celula.py:622`; :629; :711; :713; `app/docs/RELATO.md:1640`)

**2. Arquivos da obra**

O censo fornecido **não contém uma lista fechada de arquivos a alterar**: contém referências de comportamento, dependências, testes existentes e documentação. A lista efetiva de alterações de produção/teste é [nao verificado]. O registro da obra também descreve o contrato sem enumerar arquivos de implementação ou novos testes. (`censo_v2.r:1`; :19; :45; :56; :66; `app/docs/BACKLOG.md:2177`)

O inventário rastreável do censo reúne **21 arquivos de produção, um teste e dois documentos**; ele aparece integralmente na tabela seguinte. Isso não transforma dependências citadas em alterações obrigatórias. (`censo_v2.r:5`; :9; :13; :19; :20; :26; :28; :37; :39; :45; :46; :47; :48; :52; :56; :60; :62; :79; :88`) [nao verificado]

**3. Cruzamento**

A tabela cruza **o inventário citado**, pois o conjunto efetivo de arquivos que O10-D mudará permanece [nao verificado].

Fontes das células:

- **A:** lista integral de arquivos BUG-PAR fornecida no item 3(a), tratada como `BUG-PAR:1`.
- **B:** inventário de migração do predesenho e avaliação da c9, iniciados em `predesenho.r:23` e :610. `n` significa ausência de indicação de mudança nesse inventário.
- **C:** diff próprio declarado da F2: somente `chamados/models.py` e `test_e4c_f2_models_le_o_ato.py`. (`matriz_F2.md:11`; :12)
- **D:** arquivos da F4 enumerados na introdução e costura com `chamados/juizes.py`; o contrato preserva também sítios por allowlist. (`matriz_F4.md:11`; :12; seção “CONTRATO DE SAIDA”, numeração física desta última: [nao verificado])

| Arquivo citado | Fonte no censo | (a) BUG-PAR | (b) F1 B2 pelo predesenho | (c) F2 | (d) F4 |
|---|---:|---|---|---|---|
| `app/ponto/portas/celula.py` | :1 | n · A | n · B | n · C | n · D |
| `app/ponto/management/commands/gerar_celulas.py` | :13 | n · A | n · B | n · C | n · D |
| `app/ponto/services/fechamento.py` | :19 | n · A | n · B | n · C | n · D |
| `app/chamados/rejuncao.py` | :20 | n · A | n · B | n · C | n · D |
| `app/chamados/services/feriado_reavaliar.py` | :20 | n · A | n · B | n · C | n · D |
| `app/escala/alimentacao.py` | :26 | n · A | n · B | n · C | n · D |
| `app/ponto/services/cartorio.py` | :28 | n · A | **s · `predesenho.r:131`**, R138 | n · C | n · D |
| `app/core/observ.py` | :37 | n · A | n · B | n · C | n · D |
| `app/ponto/management/commands/detectar_ausencias.py` | :39 | n · A | n · B | n · C | n · D |
| `app/chamados/push_utils.py` | :39 | n · A | n · B | n · C | n · D |
| `app/ponto/utils.py` | :45 | n · A | n · B | n · C | n · D |
| `app/escala/servico_jornada.py` | :46 | n · A | n · B | n · C | n · D |
| `app/colaboradores/mixins.py` | :47 | n · A | n · B | n · C | n · D |
| `app/chamados/utils.py` | :47 | n · A | n · B | n · C | n · D |
| `app/folha/export.py` | :48 | n · A | n · B; citado como contexto em :249 | n · C | n · D |
| `app/core/juizes.py` | :48 | n · A | n · B | n · C | n · D |
| `app/ponto/turnos.py` | :52 | n · A | n · B | n · C | n · D |
| `app/core/models.py` | :56 | n · A | n · B | n · C | n · D |
| `app/chamados/views_arquivar.py` | :60 | n · A | **s\* · `predesenho.r:617`** | n · C | n · D |
| `app/chamados/services/arquivo.py` | :60 | n · A | n · B | n · C | n · D |
| `app/ponto/models.py` | :62 | n · A | n · B | n · C | n · D |
| `app/ponto/tests/test_o10b_editar_dia_troca.py` | :9 | n · A | n · B | n · C | n · D |
| `app/docs/BACKLOG.md` | :13 | n · A | n · B | n · C | n · D |
| `app/docs/RELATO.md` | :88 | n · A | n · B | n · C | n · D |

`\*` O predesenho recomenda aproveitar a passagem de `agora/ator/causa` da c9, citando `views_arquivar.py:83`; isso demonstra presença no aproveitamento proposto, mas sua atribuição definitiva à **B2**, depois da B1, é [nao verificado]. (`predesenho.r:617`)

Assim, há referência de migração da F1 em `cartorio.py` e referência condicionada em `views_arquivar.py`. **Não há base para declarar o cruzamento definitivo da O10-D fechado**, porque o censo não fornece seu conjunto de alterações. (`predesenho.r:131`; :617; lista efetiva de mudanças da O10-D: [nao verificado])

**4. Perguntas de lei**

As quatro perguntas finais do censo foram respondidas pelo aval fornecido; o registro atual confirma expressamente as seis originais e as quatro residuais como respondidas. (`aval_lei_permuta.txt:1`; `app/docs/RELATO.md:1640`)

| Pergunta residual do censo | Resposta fornecida |
|---|---|
| Quais turnos entram nas 11h? | Fim previsto do anterior → início do assumido; fim do assumido → início do seguinte, para ambos, sobre a célula após a permuta; sem exceção de CCT. (`censo_v2.r:85`; `aval_lei_permuta.txt:1`) |
| Restrições entre empresas, vínculos e escalas? | Mesma empresa/CNPJ; ambos ativos e vigentes nas duas datas; escalas diferentes permitidas se passarem nas guardas. (`censo_v2.r:86`; `aval_lei_permuta.txt:1`) |
| Quais cartas são elegíveis? | Ato/Ausencia tipo permuta, aprovado, período do dia exato, vinculado ao ID; ausência preexistente de outro tipo não serve. (`censo_v2.r:87`; `aval_lei_permuta.txt:1`) |
| Permuta produz `folga_trabalhada` e decisão do admin? | Permuta formal é compensação, sem HE nem dobra de DSR; falta de DSR na semana barra. (`censo_v2.r:88`; `aval_lei_permuta.txt:1`) |

Não identifiquei outra decisão de negócio exigida pelos insumos. A confirmação pelo admin já consta do corte da troca; a ação RBAC específica, o desenho de `d1=d2` e os validadores são lacunas de implementação cuja solução não foi demonstrada. (`app/docs/CORTES.md:252`; `censo_v2.r:45`; :46; :47; `app/ponto/portas/celula.py:629`; :713; implementação: [nao verificado])

A homologação dos itens 1, 3 e 6 com o DP continua prevista; sua realização é [nao verificado]. (`aval_lei_permuta.txt:1`)

perguntas de lei: 0

## 16/11 12:42 · sessao 174dfb4261cd · - · - · origem=Ronald (tmux)

Todas as referências de `censo_v2.r` a `app/ponto/portas/celula.py` permanecem semanticamente compatíveis com a afirmação associada e **MOVEU +38 linhas**; nenhuma delas SUMIU. Conferência abaixo refere-se à semântica descrita no anexo, pois o arquivo histórico integral não está disponível nesta cópia. [nao verificado: identidade textual com 3803e6623]

| Afirmação do censo | Referências antigas → atuais | Resultado |
|---|---|---|
| Recusa capturada pela porta; `atomic` termina antes do `except` | `celula.py:652→690`, `694→732` (retorno em `737`) | MOVEU; a porta continua devolvendo `'recusada'`; `ValueError` de autor/motivo e `RuntimeError` de trilha continuam propagando (`celula.py:684`, `686`, `730`). |
| Cartas travadas por pk, somente `Ausencia` | `657→695` | MOVEU; `.select_for_update(of=('self',))`, `.order_by('pk')` (`celula.py:695`). |
| Ordem das células por colaborador/data, por chamada | `673→711` | MOVEU; o `sorted` recebe somente cedente/substituto dessa chamada (`celula.py:711`). |
| Trava individual dentro de editar | `593→631` | MOVEU; `CelulaDia.select_for_update()` (`celula.py:631`). |
| Risco de ordem composta A/d1 B/d1 A/d2 B/d2 | `657→695`, `673→711` | MOVEU; dedução estática ainda cabível; ocorrência concreta de deadlock [nao verificado]. |
| Teste concorrente citado é inspeção AST | `test_o10b_editar_dia_troca.py:314→314` | IGUAL; inspeciona chamadas `select_for_update` e `atomic`, sem executar concorrência (`test_o10b_editar_dia_troca.py:319`, `322`). | [nao verificado]
| Empresa atual e competências consultadas em cada edição, antes de igual | `513→551`, `568→606`, `594→632`, `676→714` | MOVEU; empresa consultada em `605`, competências em `607`, igualdade em `632`; bypass em `606`. |
| Não há pré-validação das quatro; chamada seguinte depois da anterior | `675→713` | MOVEU; edição ocorre dentro do laço, antes de avaliar o próximo lado (`celula.py:711`, `713`, `714`). |
| Carimbo/autor individual e quatro trilhas editar + duas trocar, se quatro mudanças | `598→636`, `611→649`, `686→724` | MOVEU; `timezone.now` por edição e evento individual/par (`celula.py:636`, `649`, `724`). |
| Deduplicação por competência local à execução | `450→488`, `458→496`, `627→665` | MOVEU; dicionário é local ao helper e `editar_dia` passa conjunto unitário (`celula.py:488`, `494`, `496`, `665`). |
| Rejunção e reavaliação em callback por alteração | `490→528`, `502→540` | MOVEU; callback reúne as duas, `on_commit` por chamada ao helper (`celula.py:531`, `536`, `539`, `540`). |
| Rejulgamento síncrono; pula futuro/humano, captura falhas | `427→465`, `434→472`, `437→475`, `627→665` | MOVEU; filtro humanos, teste futuro, chamada síncrona, tratamento (`celula.py:465`, `469`, `472`, `475`, `665`). | [nao verificado]
| Marca cobrança órfã na transação | `625→663` | MOVEU; import em `663`, chamada em `664`, dentro do `atomic` iniciado em `630` (`celula.py:630`, `663`). |
| Reler cartas/colaboradores; argumentos só pk | `656→694`, `657→695`, `593→631` | MOVEU; leitura nova da carta com colaborador e célula com trava (`celula.py:694`, `695`, `631`). |
| Um lado igual não impede outro; igual somente ambos | `594→632`, `596→634`, `673→711`, `679→717`, `690→728` | MOVEU; retorno individual antes da escrita, visita ambos e trilha com resultados (`celula.py:632`, `634`, `711`, `717`, `728`). |
| Igualdade não inclui pedido/cartas/motivo/autor; inversão pode reescrever | `594→632`, `675→713` | MOVEU; igualdade contém origem/trabalha/dna/escala e cada papel passa booleano oposto (`celula.py:632`, `711`, `713`). |
| Rollback exterior precisa ser provocado pelo compositor para evitar primeira confirmada | `652→690`, `694→732` | MOVEU; recusa é consumida depois do `atomic` interno (`celula.py:690`, `732`, `737`). |
| Rollback exterior inclui marca/síncrono; callbacks dependem de commit | `625→663`, `434→472`, `502→540` | MOVEU para chamadas da porta (`celula.py:630`, `663`, `472`, `540`); propriedades de Django/transações e outras fontes devem ser consolidadas pelo leitor dessas fontes. | [nao verificado]
| Permissão, 11h e colisão não são guardas da porta | `559→597`, `568→606`, `646→684`, `675→713` | MOVEU; valida autor/motivo/booleano e lavra/vínculo em editar; em trocar, autor/motivo/cartas/colaboradores/documento (`celula.py:597`, `599`, `601`, `606`, `622`, `684`, `691`, `698`, `700`, `702`). |
| Trocar não recebe ID próprio; trilha objeto carta e JSON lados | `632→670`, `682→720`, `686→724` | MOVEU; assinatura e JSON não possuem ID de permuta (`celula.py:670`, `720`, `724`, `728`). |
| Coordenadas vêm das cartas relidas | `659→697`, `673→711` | MOVEU; papéis/colaborador/data lidos da nova instância (`celula.py:697`, `711`, `713`). |
| Quatro registros para quatro coordenadas distintas | `659→697`, `673→711` | MOVEU, condicional a coordenadas distintas (`celula.py:697`, `711`). |
| Ausências presentes/encontradas, colaboradores distintos, documentos preenchidos | `653→691`, `660→698`, `662→700`, `664→702` | MOVEU; nenhuma comparação/abertura dos arquivos no caminho (`celula.py:691`, `698`, `700`, `702`, `722`). |
| Nome do documento apenas na trilha | `684→722` | MOVEU (`celula.py:722`). |
| Sem consumo/exclusividade global; sem exigir datas iguais dentro da chamada | `656→694`, `673→711` | MOVEU; seleção pelos IDs e edição pelas datas próprias, sem marca de consumo (`celula.py:694`, `695`, `711`, `713`). |
| Tipo/status/período/pedido não integram elegibilidade documental | `664→702`, `675→713`, `568→606`, `584→622` | MOVEU; documento é único campo documental testado; lavra/vínculo separados (`celula.py:702`, `713`, `606`, `622`). |

Mapa integral de linhas de `celula.py` citadas no censo, para completar referências de linhas/afirmações agrupadas:

```text
427→465 434→472 437→475 450→488 458→496
490→528 502→540 513→551 559→597 568→606
584→622 593→631 594→632 596→634 598→636
611→649 625→663 627→665 632→670 646→684
652→690 653→691 656→694 657→695 659→697
660→698 662→700 664→702 673→711 675→713
676→714 679→717 682→720 684→722 686→724
690→728 694→732
```

Sobre o contrato do construtor:

- **Rollback:** a obrigação de o compositor abortar o `atomic` externo quando receber `'recusada'` permanece; a porta captura `_TrocaRecusada`, embora outras exceções possam propagar (`app/ponto/portas/celula.py:690`, `app/ponto/portas/celula.py:732`, `app/ponto/portas/celula.py:737`, `app/ponto/portas/celula.py:731`).
- **Ordem das quatro:** ordenar quatro células continua requisito do compositor/desenho; a porta ordena somente dois lados e também adquire cartas antes das células (`app/ponto/portas/celula.py:695`, `app/ponto/portas/celula.py:711`). A ordem global deve considerar também as cartas para não adquirir cartas de pares opostos antes de tentar as restantes; ocorrência concreta de deadlock [nao verificado].
- **d1=d2:** a porta não proíbe datas iguais e não conhece `d1/d2` de uma composição; a lei fornecida permite expressamente `d1=d2` (`aval_lei_permuta.txt:1`, anexo; `app/ponto/portas/celula.py:711`). Portanto, a frase final do censo “quatro células — que exige duas datas distintas” **MUDOU frente à lei**, e não deve virar validação impeditiva (`aval_lei_permuta.txt:1`, anexo).
- **Limite material da composição no mesmo dia:** `editar_dia` monta DNA a partir do vínculo do próprio colaborador e de um booleano; não recebe turno do outro (`app/ponto/portas/celula.py:622`, `app/ponto/portas/celula.py:629`, `app/ponto/portas/celula.py:572`). Duas trocas inversas no mesmo dia reescrevem as mesmas duas coordenadas com booleanos opostos; a segunda não representa quatro células distintas (`app/ponto/portas/celula.py:711`, `app/ponto/portas/celula.py:713`). A representação da troca de turnos distintos no mesmo dia no esquema atual exige investigação/desenho técnico; não é uma nova regra de negócio dada a permissão explícita do aval (`aval_lei_permuta.txt:1`, anexo).

## 16/11 12:41 · sessao 7cdb8c89aa6e · - · - · origem=Ronald (tmux)

| Afirmação/referência do censo | Sítio hoje | Veredito |
|---|---|---|
| Recálculo posterior por `on_commit`: `fechamento.py:1824`; rollback externo condiciona callback: `:1802` | `app/ponto/services/fechamento.py:1824` registra `_rodar`; `:1802` declara dependência do commit; `_rodar` recalcula uma competência em `:1809`–`:1810`. | **IGUAL** nos sítios citados. |
| Rejunção só alcança FK nula: `rejuncao.py:60` | `app/chamados/rejuncao.py:60` filtra vivos; `:62` exige `celula__isnull=True`; escritor em `:98`, contador condicionado à escrita em `:107`. | **IGUAL**. |
| Reavaliação recorta dias/chamados vivos: `feriado_reavaliar.py:17/:76` | `app/chamados/services/feriado_reavaliar.py:18` filtra vivos; `:77`–`:80` recortam datas e colaborador; `:84` confirma dia e executa encerramento. | **IGUAL**; referências antigas `:17/:76` eram linhas adjacentes às expressões relevantes e continuam existentes. |
| Consulta das competências exportadas sem lock: `gerar_celulas.py:161` | `app/ponto/management/commands/gerar_celulas.py:170` consulta `ExportacaoDominio.objects.filter(...).order_by().values_list(...).distinct()`; juiz chamado em `:182`; a função está em `:135`. | **MOVEU**: consulta `161 → 170` (+9). Não há `select_for_update` nesse corpo (`:135`–`:189`). |
| Marca órfã consulta carimbos recentes: `gerar_celulas.py:241` | `app/ponto/management/commands/gerar_celulas.py:250` chama `carimbos_de_reescrita`; filtro colaborador/vivos em `:252`–`:254`; juiz em `:257`. | **MOVEU**: `241 → 250` (+9). |
| Marcador atualiza contexto: `gerar_celulas.py:255` | `app/ponto/management/commands/gerar_celulas.py:264` chama `renovar_contexto(..., orfa_regeneracao=...)`; docstring “só marca” em `:235`. | **MOVEU**: `255 → 264` (+9). |
| Cartório relê célula: `cartorio.py:1976` | `app/ponto/services/cartorio.py:2027` consulta `CelulaDia.objects.filter(colaborador=..., data=...).first()`. | **MOVEU**: `1976 → 2027` (+51). |
| Rejulgamento pode alcançar abertura de cobrança: `cartorio.py:1740` | `app/ponto/services/cartorio.py:1791` chama `emissor._abrir_chamado`, condicionado a `apply_`, à regra de lista e à âncora (`:1776`, `:1790`). | **MOVEU**: `1740 → 1791` (+51). |
| Abertura pode enviar push imediato: `detectar_ausencias.py:778` | `app/ponto/management/commands/detectar_ausencias.py:778` guarda `not _flexivel`; chamada síncrona `push_supervisao` em `:779`; falha capturada em `:785`. | **IGUAL**. |
| Push alcança envio externo: `push_utils.py:40` | `app/chamados/push_utils.py:40` chama `webpush(...)` diretamente. | **IGUAL**. Envio concreto durante permuta permanece **[nao verificado]**. |
| Recarregamento de vínculos: `alimentacao.py:355` | `app/escala/alimentacao.py:355` carrega células; `:357` carrega escalas; `:358` chama juiz do vínculo por par. | **IGUAL**. |
| Identity map limitado às instâncias recebidas e restaurado: `alimentacao.py:43/:47` | `app/escala/alimentacao.py:43` salva atributo anterior, `:44` instala mapa; `:47`–`:51` restauram/removem atributo. | **IGUAL**. |
| Savepoint da auditoria não é commit independente: `observ.py:133` | `app/core/observ.py:133` abre `transaction.atomic()` interno; `:134`–`:137` criam auditoria. | **IGUAL** no código; rollback externo alcança essa escrita transacional, como dedução estática do atomic aninhado (`app/core/observ.py:133`). |
| TOCTOU documentado e pergunta sobre `folga_trabalhada`: `BACKLOG.md:1876` | Fila O10-B em `app/docs/BACKLOG.md:2176`, com TOCTOU e pergunta histórica de FT. | **MOVEU**: conteúdo da fila hoje em `:2176`. A linha física `:1876` agora trata CODEX-CONSTRUTOR-NEGOCIO (`app/docs/BACKLOG.md:1876`). |
| “BACKLOG.md:1879 trata BO-0511-G2-TRIAGEM” | BO está em `app/docs/BACKLOG.md:2181`; `:1879` agora trata H12-DEMISSAO-NA-JANELA. A obra O10-D está em `:2177`. | **MUDOU** a afirmação sobre o sítio físico; BO **MOVEU** para `:2181`. A referência correta da obra passou a existir em `:2177`. |
| Pergunta histórica FT aberta em `RELATO.md:449` | Pergunta histórica em `app/docs/RELATO.md:2120`; `:449` agora registra estado E3. Resposta específica da permuta formal registrada em `app/docs/RELATO.md:1640` e lei fornecida pelo usuário. | **MOVEU** o registro histórico para `:2120`; **MUDOU** a conclusão “continua não respondida” para O10-D, porque `:1640` registra as 6+4 respondidas. |

O contrato documental atual reafirma expressamente: porta retorna `'recusada'` sem raise, compositor deve levantar para rollback; ordenar as quatro células; `d1=d2` é permitido e o censo supunha datas distintas (`app/docs/RELATO.md:1640`). A obra também consta no BACKLOG com composição de duas trocas, pré-validação das quatro, rollback integral e identificação própria na trilha (`app/docs/BACKLOG.md:2177`).

Nos efeitos transitivos examinados, o limite material do rollback permanece: a abertura chama push sincronicamente e este alcança `webpush`, portanto rollback SQL não desfaz envio já realizado; ocorrência concreta nessa composição **[nao verificado]** (`app/ponto/services/cartorio.py:1791`; `app/ponto/management/commands/detectar_ausencias.py:779`; `app/chamados/push_utils.py:40`).

## 16/11 12:41 · sessao 262d1a369f1a · - · - · origem=Ronald (tmux)

Seções 2 e 3 conferidas somente por leitura; nenhuma execução Django/teste ou escrita. [nao verificado]

| Afirmação do censo | Referência antiga → atual | Situação |
|---|---|---|
| Helper de cobertura declara que não valida 11h | `app/ponto/utils.py:152` → `:152` (item explícito em `:153`) | **IGUAL**. |
| `marcos_turno` dá datetimes crescentes e trata meia-noite | `app/escala/servico_jornada.py:61` → `:69` (laço `:88-93`) | **MOVEU**, sentido preservado. |
| `fim_turno` fornece término | `app/escala/servico_jornada.py:89` → `:97` (retorno `:101`) | **MOVEU**, sentido preservado. |
| `tem_acao` verifica ações sem gravar | `app/colaboradores/mixins.py:322` → `:322` (corpo `:326-332`) | **IGUAL**. |
| `pode_agir_chamado` verifica departamento dono | `app/chamados/utils.py:68` → `:68` (delegação `:72`) | **IGUAL**. |
| Lavra é juiz separado, bool/fontes | `app/folha/export.py:589` → `:597` (fontes/retorno `:622-634`) | **MOVEU**, sentido preservado. |
| Registro K9 aponta para esse juiz | `app/core/juizes.py:322` → `:353` | **MOVEU**, sentido preservado. |
| `_competencias_exportadas` enumera janelas candidatas e consulta juiz | `app/ponto/management/commands/gerar_celulas.py:126` → `:135` (consulta `:184`, janela `:187`) | **MOVEU**, sentido preservado. |
| `autoridade_da_lavra` compõe frase sem gravação | `app/ponto/management/commands/gerar_celulas.py:208` → `:217` (corpo `:225-229`) | **MOVEU**, sentido preservado. |
| `ausencias_sobrepostas` julga períodos de Ausencia, não colisão horária de plantões | `app/ponto/turnos.py:2795` → `:2817` (consulta `:2839-2848`) | **MOVEU**, sentido preservado. |
| `evento` não tem argumento correlação/lote/grupo; aceita antes/depois | `app/core/observ.py:94` → `:94` | **IGUAL**. |
| JSONs de auditoria persistem antes/depois | `app/core/observ.py:133` → `:133` (campos em `:136`) | **IGUAL**. |
| `LogAuditoria` tem objeto_id inteiro e JSONs | `app/core/models.py:298`, `:300` → mesmas linhas (`valor_depois :301`) | **IGUAL**. |
| `evento_kw` só encaminha campos conhecidos | `app/core/observ.py:142` → `:142` (encaminhamento `:146-148`) | **IGUAL**. |
| Precedente `lote='arquivar_lote'` é rótulo fixo | `app/chamados/views_arquivar.py:146` → `:146` | **IGUAL**. |
| Metadado extra é incorporado no JSON | `app/chamados/services/arquivo.py:133` → `:133` (incorporação `:134`, evento `:137-138`) | **IGUAL**. |
| `Ausencia.chamado_origem` pode referir pedido/chamado | `app/ponto/models.py:786` → `:786` | **IGUAL**; FK é especificamente para `ChamadoColaborador` (`:787-789`). |

**Limite dos controles prontos:** ausência de juiz completo de 11h/colisão pronto continua **[nao verificado]**; o helper declara explicitamente não validar 11h (`app/ponto/utils.py:152-153`), e os helpers geométricos só produzem marcos/término (`app/escala/servico_jornada.py:69`, `:97`). Busca textual por `11h|interjornada|permuta|permutar_plantoes` em `app/ponto`, `app/escala` e registro não localizou compositor/validador específico; isso não prova inexistência sob outro nome **[nao verificado]**.

**Escopo:** o censo fornecido enumera sítios existentes e insumos; não contém uma lista explícita “arquivos a modificar”. Lista fechada de mudanças de produção/teste **[nao verificado]**. Não converter todos os arquivos referenciados em arquivos que a obra muda. [nao verificado]

**Documentos atuais úteis para consolidação:**

- `app/docs/BACKLOG.md:2177`: contrato `permutar_plantoes(A,d1,B,d2)`, duas portas no atomic, pré-validação das quatro, rollback integral, recusa falante, ID/trilha e escala-base intocada; registra compositor levantar e ordenação das quatro.
- `app/docs/RELATO.md:1640`: declara respondidas as seis perguntas v1 e quatro residuais v2, mantém rollback por raise do compositor, ordenação das quatro e corrige a suposição do censo: **d1=d2 permitido**.
- `app/docs/PROMPTS.md:2015`: resumo das seis respostas da lei, inclusive mesma empresa, carta ato aprovado, compensação e guarda DSR.
- `app/docs/RELATO.md:1602`: “sem copia de construcao”; faltam construtor, selos RED 1–6 e etapas seguintes.
- `app/docs/RELATO.md:1645`: referência histórica ao BACKLOG:1879 reescrito; a linha atual da obra é `app/docs/BACKLOG.md:2177`.
- `app/docs/RELATO.md:5`: censo físico vive em logs não disponíveis na cópia e papel PREPARA foi objeto de revisão; portanto linhas físicas dos anexos apenas colados devem ter convenção explícita ou **[nao verificado]**.

As quatro perguntas finais do censo são expressamente tratadas como respondidas pela documentação atual (`app/docs/RELATO.md:1640`); correspondência semântica com a lei fornecida: turnos previstos anterior/seguinte (§1), empresa/vínculo/tipo (§3), cartas (§5), compensação sem folga_trabalhada (§6). Fonte física da lei anexa **[nao verificado]**.

## 16/11 12:36 · sessao 929ffb0f1659 · CORTES-F2-F9-1611 · construtor · origem=Code

Entregues quatro **TESTE-RED** e uma **CARACTERIZACAO**, com fixtures salvas copiadas do [teste vizinho](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_f1nova_b1_juiz_respondida.py:49), reunidas no [apoio novo](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/cortes_f2_f9_apoio.py:37). [nao verificado]

| Degrau | Decisão e resposta-alvo após F2 | Arquivo escrito e mutante |
|---|---|---|
| **M4** | **TESTE-RED**. Sem viva/validação/efeito, RESPOSTA no ato → `RESPONDIDA`, mesmo com projeção vazia. Hoje a queda lê `respondida_em` (`app/chamados/models.py:3158`); RESPOSTA já significa aguardar validação (`app/chamados/services/tarefa_do_dia.py:159`). | [M4](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_cortes_f2_f9_m4.py:51). Mutante `m4_pendente`: força a queda para `PENDENTE`. |
| **M13** | **TESTE-RED**. `_ancoradas` exclui a pergunta validada pelo ato e conserva a pendente. Hoje filtra pelo campo (`app/chamados/models.py:344`); VALIDACAO é decisão existente (`app/chamados/services/tarefa_do_dia.py:167`). | [M13](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_cortes_f2_f9_m13.py:38). Mutante `m13_inclui_validadas`: retira o filtro de validação. |
| **M14** | **TESTE-RED**. Relavra a pendente e preserva `julgado_em` das validadas, inclusive só no ato. Hoje a seleção usa `validada_em` (`app/chamados/models.py:310`); a lavra real altera o instante (`app/chamados/veredito_lavrado.py:272`). | [M14](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_cortes_f2_f9_m14.py:41). Mutante `m14_relavra_validadas`: inclui validadas na relavra. |
| **M15** | **TESTE-RED**. Após REABERTURA, SILENCIO fotografa resposta vigente `''`, preservando a anterior no histórico. A porta zera a resposta (`app/chamados/services/disputa_emissao.py:1609`); hoje SILENCIO fotografa os brancos residuais da projeção (`app/chamados/models.py:2007`). | [M15](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_cortes_f2_f9_m15.py:40). Mutante `m15_resposta_anterior`: fotografa `'07:13'`. |
| **M18** | **CARACTERIZACAO da cópia**. O C6 real recebe cópia com resposta/carimbos zerados; original e banco ficam intactos. Mesmo alvo antes/depois, conforme o trecho não migrado (`app/chamados/models.py:1541`, `app/chamados/models.py:1553`). | [M18](/home/ronald/wt-codex-cortes-f2-f9-1611-122527/app/chamados/tests/test_cortes_f2_f9_m18.py:41). Mutante `m18_texto_residual`: injeta texto na cópia. |

**M19 — ALVO-INDETERMINADO.** Qual API existente deve responder pelo contrato dos ramos prefetch, `pk=None` e duplo, sem pressupor `_leitura_dos_atos` da c3? Correspondência desse helper no main: **[nao verificado]**.

**M20 — ALVO-INDETERMINADO.** Qual lista de leituras residuais permitidas, identificada por `Classe.metodo`, define o oráculo do selo nesta base? Aplicabilidade de `F2_RESIDUO` da c3: **[nao verificado]**.

Comandos individuais, a partir de `app/`, também registrados nas docstrings dos arquivos:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m4 --settings=config.settings.ci
CORTES_F2_MUTANTE=m4_pendente PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m4 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m13 --settings=config.settings.ci
CORTES_F2_MUTANTE=m13_inclui_validadas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m13 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m14 --settings=config.settings.ci
CORTES_F2_MUTANTE=m14_relavra_validadas PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m14 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m15 --settings=config.settings.ci
CORTES_F2_MUTANTE=m15_resposta_anterior PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m15 --settings=config.settings.ci

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m18 --settings=config.settings.ci
CORTES_F2_MUTANTE=m18_texto_residual PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_cortes_f2_f9_m18 --settings=config.settings.ci
```

Em **cada degrau**, falha/verde efetivos, ausência de erro de setup e mordida dos mutantes: **[nao verificado]**. Testes e mutantes não foram executados. Conservação integral da materialização e invariância do `supra_juiz` em competência exportada: **[nao verificado]**. [nao verificado]

## 16/11 12:21 · sessao 7b8a48377983 · CORTES-F2-F9-1611 · leitura · origem=Code

Para A, **NÃO para o contrato completo “instante do ato de materialização, valor conservado”**. Hoje existe um instante persistido, mas o efeito lido é o **campo atual `materializada_em`**: ele pode ser substituído ou apagado. A projeção não o reconstrói de um ato conservado. (`app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1400`, `app/chamados/models.py:3295`, `app/chamados/models.py:3318`, `app/chamados/models.py:3393`.)

As provas abaixo são por **leitura das asserções**; resultados de execução, diferenças históricas por Git e efeitos sobre dados de produção: **[nao verificado]**.

A projeção atual recebe diretamente `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em` e `via_resolucao`. Seu índice `[3]` é `materializada_em`; quando recebe `agora`, apenas oculta carimbos posteriores ao instante consultado. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1396`, `app/chamados/juizes.py:1400`.)

**Os dois leitores pedidos não consomem diretamente essa tupla:**

- `sem_veredito_entre` exige `materializada_em is None`, validação ausente e resposta por instante **ou** texto; aplica também a tranca, salvo dispensa explícita. Portanto qualquer carimbo exclui pela materialização, independentemente da via. (`app/chamados/models.py:1923`, `app/chamados/models.py:1924`, `app/chamados/models.py:1926`.)
- `estado` retorna `RESOLVIDA` imediatamente para validada com carimbo. Sem validação, consulta primeiro `pergunta_viva`; somente se não estiver viva aceita o carimbo como `RESOLVIDA`. **Sair da lista não garante, sozinho, `RESOLVIDA`.** (`app/chamados/models.py:3151`, `app/chamados/models.py:3153`, `app/chamados/models.py:3154`, `app/chamados/models.py:3156`.)

| Ramo | Quem escreve o valor | Quem lê e o que significa hoje |
|---|---|---|
| **Carimbo de identidade** | `carimbar_identidade` escolhe `quando or _tz.now()` e salva. A atribuição comum escreve `materializada_em`, `materializada_por_id` e `via_resolucao`. Com `manter_carimbo=True`, conserva instante e autor anteriores. (`app/chamados/models.py:3292`, `app/chamados/models.py:3295`, `app/chamados/models.py:3300`, `app/chamados/models.py:3209`, `app/chamados/models.py:3210`, `app/chamados/models.py:3211`.) | A projeção lê o instante atual; o C6 combina via e carimbo; lista e estado leem o campo conforme a precedência acima. Não há seleção especial de identidade para formar `[3]`. (`app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1416`, `app/chamados/juizes.py:1444`, `app/chamados/models.py:1923`, `app/chamados/models.py:3153`.) |
| **`materializada_em` solto, via vazia** | `carimbar` recebe a via, escolhe o instante e repassa ao escritor comum; não há rejeição explícita de `''` nesse trecho. Escritor histórico dos registros soltos: **[nao verificado]**. (`app/chamados/models.py:3228`, `app/chamados/models.py:3234`, `app/chamados/models.py:3239`, `app/chamados/models.py:3209`.) | `[3]` recebe esse instante e a lista exclui a pergunta. Para `estado`, a via vazia não elimina a precedência de `pergunta_viva`; validada com carimbo resolve diretamente. (`app/chamados/juizes.py:1390`, `app/chamados/models.py:1923`, `app/chamados/models.py:3153`, `app/chamados/models.py:3154`.) |
| **Materialização de fato, sem carimbo** | `_pergunta_materializada_de_fato` fornece uma decisão booleana; o materializador usa essa decisão como guarda, captura o instante depois e chama `carimbar`. A presença do fato e a escrita do carimbo são passos distintos. (`app/chamados/juizes.py:666`, `app/chamados/services/materializacao.py:1239`, `app/chamados/services/materializacao.py:1250`, `app/chamados/services/materializacao.py:1283`.) | Sem carimbo, `[3]` permanece `None`. E sozinho não exclui por materialização nem produz `RESOLVIDA`; ambos os retornos `RESOLVIDA` exigem `materializada_em`. No C6, o degrau sem carimbo retorna viva antes da chamada final a E. (`app/chamados/juizes.py:1390`, `app/chamados/models.py:1923`, `app/chamados/models.py:3153`, `app/chamados/models.py:3156`, `app/chamados/juizes.py:1482`, `app/chamados/juizes.py:1489`.) |
| **`materializada_conservada_em`** | Não foi localizado esse campo/helper na cópia: **[nao verificado]**. O escritor efetivamente encontrado atribui `materializada_em`. (`app/chamados/models.py:3209`.) | A projeção atual não contém esse ramo: devolve o campo atual, eventualmente recortado por `agora`. (`app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1396`, `app/chamados/juizes.py:1400`.) |

Quanto ao relógio, **a projeção e o filtro de materialização de `sem_veredito_entre` não consultam `today/now`**. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1400`, `app/chamados/models.py:1923`.) Entretanto, **`estado` pode consultar relógio fora da borda**, transitivamente: chama `pergunta_viva(self)` sem instante; o cálculo pode alcançar `date.today()` na janela e `_tz.now()` na classificação da falta. (`app/chamados/models.py:3154`, `app/chamados/juizes.py:898`, `app/chamados/juizes.py:1471`, `app/chamados/juizes.py:596`, `app/chamados/juizes.py:1475`, `app/escala/servico_jornada.py:387`.)

Em `supra_juiz`, os leitores indiretos são as chamadas de `sem_veredito_entre` para **pai** e **destino**, ambas com `ignorar_tranca=True`; `pode_executar_csf` decide pela ausência dessas perguntas. (`app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:574`, `app/ponto/supra_juiz.py:588`.) Mudança efetiva em competência exportada: **[nao verificado]**.

A parte “instante do ato” tem correspondência operacional: o materializador captura `agora` e o persiste no carimbo. (`app/chamados/services/materializacao.py:1250`, `app/chamados/services/materializacao.py:1283`, `app/chamados/models.py:3262`.) A parte “valor conservado” não está fixada como contrato geral: conservar é opção na identidade; descarimbar apaga o valor; e o catálogo atual de `DecisaoPergunta` não lista ato de materialização. (`app/chamados/models.py:3292`, `app/chamados/models.py:3295`, `app/chamados/models.py:3318`, `app/chamados/models.py:3393`, `app/chamados/models.py:4024`, `app/chamados/models.py:4026`.)

Para B, “BASE mudou?” compara o comportamento atual com a **BASE transcrita no anexo**; confirmação por diff histórico: **[nao verificado]**. As hipóteses sobre leitura conservada da F1 antiga não podem ser transportadas para a projeção atual, que lê os campos diretamente. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1400`.)

| Degrau | Sítio hoje | BASE mudou? | Prova hoje | Nota |
|---|---|---|---|---|
| **[DEPENDE-F1] 1 — solto deixa a lista** | `app/chamados/models.py:1923`; projeção em `app/chamados/juizes.py:1390`. | **Não**: qualquer `materializada_em` continua excluindo, como a BASE. | Forma exigida em `app/chamados/tests/test_contract_dedup_status.py:143`; valor materializado via CSF em `app/ponto/tests/test_supra_juiz_executor.py:85`. | O teste T:189 citado no anexo, aplicado à árvore atual: **[nao verificado]**. |
| **[DEPENDE-F1] 2 — assinatura passiva × face SQL** | A validação atual é lida em `app/chamados/models.py:3151` e `app/chamados/juizes.py:1405`. | **Não nesses leitores**: continuam por campo. `_q_validada_pelo_ato` não localizado: **[nao verificado]**. | Asserção do confronto específico passivo/validação/face citada: **[nao verificado]**. | A paridade existente em `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:81` compara **resposta**, não validação. |
| **[DEPENDE-F1] 3 — desaparecimento do relógio** | Efeito em `app/chamados/juizes.py:1390`; `estado` chama o C6 em `app/chamados/models.py:3154`. | **Não no efeito por campo**. A conclusão ampla “leitores sem relógio” não vale para `estado`: caminho até `app/chamados/juizes.py:596`. | Asserção específica variando o relógio nesses leitores: **[nao verificado]**. | Não há E formando `[3]` no corpo atual; ainda há relógio transitivo no C6. (`app/chamados/juizes.py:1400`, `app/chamados/juizes.py:1471`.) |
| **[DEPENDE-F1] 4 — continência de `q_vivas`** | SQL por campo em `app/chamados/models.py:3496`; C6 por campo em `app/chamados/juizes.py:1405`. | O degrau de validação continua por campo; a quebra hipotética por migração para atos não está nesses corpos. | `app/chamados/tests/test_contract_pergunta_viva.py:60`; por degrau em `app/chamados/tests/test_viva_delega_ao_juiz.py:45`. | Cobertura específica com ato divergindo da projeção: **[nao verificado]**. |
| **[DEPENDE-F1] 5 — guarda `pk None`, N1/M7** | Leitura direta em `app/chamados/juizes.py:1389`; opções em `app/chamados/models.py:3706`. | **Não nesses caminhos**: não passam pela guarda conservada descrita no anexo. | Instâncias não salvas: `app/chamados/tests/test_pergunta_estado.py:47`, `app/chamados/tests/test_valor_rotulo.py:12`, `app/chamados/tests/test_valor_rotulo.py:16`. | São provas da leitura atual por campos, não de `_leitura_conservada`. |
| **[DEPENDE-F1] 6 — NameError no teste de memória antigo** | Identificação do teste/sítio correspondente hoje: **[nao verificado]**. | **[nao verificado]**. | **[nao verificado]**. | O anexo não fornece arquivo e símbolo suficientes para vincular aquele erro a uma asserção atual: **[nao verificado]**. | [nao verificado]

| Degrau | Sítio hoje | BASE mudou? | Prova hoje — asserção | Nota |
|---|---|---|---|---|
| **M1b** | `app/chamados/models.py:3151`, `app/chamados/models.py:3153`. | **Não**: validada com materialização → `RESOLVIDA`. | **Sim:** `app/chamados/tests/test_pergunta_estado.py:47`. | Fixture combina ambos os carimbos em `app/chamados/tests/test_pergunta_estado.py:45`. |
| **M4** | Queda final em `app/chamados/models.py:3158`. | **Não**: decide por `respondida_em`. | Prova que isole especificamente essa queda: **[nao verificado]**. | Asserções de rótulo existem em `app/chamados/tests/test_pergunta_estado.py:30` e `app/chamados/tests/test_pergunta_estado.py:36`, mas não isolam C6 falso sem validação/materialização. |
| **M7** | `app/chamados/models.py:3693`, `app/chamados/models.py:3701`, `app/chamados/models.py:3706`. | **Não**: fonte continua `resposta_colab`. | **Sim, opções:** `app/chamados/tests/test_valor_rotulo.py:12` e `app/chamados/tests/test_valor_rotulo.py:16`. | Dropdown posto/escala e texto vindo de ato: **[nao verificado]**. |
| **M11** | `app/chamados/models.py:1926`. | **Não**: tranca exclui, salvo `ignorar_tranca`. | **Sim:** `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:304`; dispensa da tranca em `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:305`. | A tranca é substituída por `True` em `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:303`. |
| **M13** | `_ancoradas`, filtro em `app/chamados/models.py:344`. | **Não**: usa `validada_em__isnull=True`. | **Parcial, AST:** `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:284`. | Exige presença dos campos; não distingue `isnull=True` de `False`, pois a extração retém o nome do campo. (`app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:283`.) |
| **M14** | Filtro e relavra em `app/chamados/models.py:310`, `app/chamados/models.py:312`. | **Não**: relavra não validadas por campo. | **Parcial:** relavra e ordem após salvar em `app/chamados/tests/test_o261e_f1_codex_v5.py:90`, `app/chamados/tests/test_o261e_f1_codex_v5.py:91`. | Asserção que distinga “só não validadas” de incluir também validadas: **[nao verificado]**. |
| **M15** | SILENCIO e foto em `app/chamados/models.py:2006`, `app/chamados/models.py:2007`. | **Não**: `ato_texto` vem de `resposta_colab`. | **Parcial:** existência/autoria do SILENCIO em `app/chamados/tests/test_e1_entradas.py:125`. | Essa asserção não consulta `ato_texto`; prova da fonte da fotografia: **[nao verificado]**. |
| **M17** | Conserva R/V em `app/chamados/models.py:1613`; limpa em `app/chamados/models.py:1629`. | **Sim**: agora também zera `resposta_colab=''`. (`app/chamados/models.py:1630`.) | **Sim:** par/carimbo em `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:276`, `:277`, `:278`; ausência de devolução em `:281`; trilha exata em `:285`; fila em `:329`, todos no mesmo arquivo. | Continua sem REABERTURA/RECUSA; conserva atos R/V, não “nenhum ato”. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:281`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:285`.) |
| **M18** | Cópia e julgamento em `app/chamados/models.py:1541`, `app/chamados/models.py:1557`. | **Sim**: simulação agora zera também texto. (`app/chamados/models.py:1556`.) | **Parcial:** seleção pelo C6 em `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:139`, `:142`, `:149`, `:150`, todos no mesmo arquivo. | Asserção específica da limpeza do texto **na cópia**: **[nao verificado]**. |
| **M19** | `_leitura_dos_atos` não localizado: **[nao verificado]**. Leitores atuais em `app/chamados/models.py:3665`, `app/chamados/models.py:3688`, `app/chamados/models.py:1923`. | Os leitores encontrados continuam por campo; não mostram a migração c3. | Prova dos ramos prefetch/`pk None`/duplo daquele helper: **[nao verificado]**. | `respostas_anteriores` lê decisões, mas é outro método. (`app/chamados/models.py:3667`, `app/chamados/models.py:3675`.) |
| **M3** | `app/chamados/models.py:3154`, `app/chamados/models.py:3156`. | **Não**: viva precede carimbo; não viva com carimbo resolve. | **Sim, solto:** `app/chamados/tests/test_pergunta_estado.py:53`, com fixture em `app/chamados/tests/test_pergunta_estado.py:52`. | Prova E-sem-carimbo: **[nao verificado]**; E não fornece o efeito atual. (`app/chamados/juizes.py:1390`.) |
| **M9** | `app/chamados/models.py:1924`, `app/chamados/models.py:1925`. | **Não**: instante **ou** texto continuam suficientes. | **Sim:** resposta em `app/ponto/tests/test_supra_juiz_executor.py:72`; texto sem instante em `:81`; sem resposta em `:69`, todos no mesmo arquivo. | Alcançam o filtro real pela delegação em `app/ponto/tests/test_supra_juiz_executor.py:46`. |
| **M10** | `app/chamados/models.py:1923`. | **Não**: qualquer `materializada_em` exclui. | **Sim:** valor via CSF em `app/ponto/tests/test_supra_juiz_executor.py:85`; forma textual em `app/chamados/tests/test_contract_dedup_status.py:143`. | Prova E-sem-carimbo: **[nao verificado]**; esse ramo não compõe `[3]` hoje. (`app/chamados/juizes.py:1400`.) |
| **M20** | `F2_RESIDUO`/`SeloF2Test` da c3 não localizado: **[nao verificado]**. | Comparação daquele selo: **[nao verificado]**. | Prova equivalente fora de `estado`: **[nao verificado]**. | O selo em `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:233` fiscaliza **escritores do par**; não comprova o selo de leitores pedido. |

## 16/11 12:19 · sessao 371319897e7d · - · - · origem=Ronald (tmux)

| Degrau | Sítio hoje | BASE mudou? | Prova hoje, por leitura | Nota |
|---|---|---|---|---|
| M17 | `app/chamados/models.py:1560`, `:1613`, `:1629` | **Sim**: descarimbo agora também zera `resposta_colab=''` (`:1630`). Continua conservando R/V antes de limpar, sem REABERTURA (`:1613`, `:1629`). | `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:276`, `:277`, `:278`, `:281`, `:285`, `:329`. | `:281` proíbe REABERTURA/RECUSA; `:285` exige trilha exata R/V. Não significa “sem ato nenhum”. |
| M18 | `app/chamados/models.py:1515`, `:1541`, `:1553` | **Sim**: simulação agora zera texto junto dos instantes (`:1554`, `:1555`, `:1556`). Continua cópia em memória consultando `pergunta_viva` (`:1557`). | `app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:139`, `:142`, `:149`, `:150`, `:181`, `:184`. | Mordem seleção pelo C6 e célula do julgamento. Asserção específica da nova limpeza de texto **na cópia**: [nao verificado]. |
| M19 | Helper `_leitura_dos_atos` não localizado [nao verificado]. Leitores atuais: `app/chamados/models.py:3665`, `:3688`, `:1923`. | Leitores encontrados seguem campos; a migração descrita para c3 não é o corpo atual (`:3665`, `:3688`, `:1923`). | Provas dos ramos prefetch/pk None/duplo daquele helper: [nao verificado]. | `respostas_anteriores` é outro leitor, de decisões, em `app/chamados/models.py:3667`, `:3675`. |
| M3 | `app/chamados/models.py:3154`, `:3156`, `:3158` | **Não**, ante BASE fornecida: viva precede o teste de materialização e o efeito lido é `materializada_em`. | `app/chamados/tests/test_pergunta_estado.py:53`; fixture sem resposta/validação, apenas materialização, em `:52`. | Morde solto via vazia. Caso E-sem-carimbo: [nao verificado]; esse efeito não é critério de RESOLVIDA no corpo atual (`models.py:3156`). |
| M9 | `app/chamados/models.py:1923`, `:1924`, `:1925` | **Não**: instante **ou** texto continuam suficientes. | `app/ponto/tests/test_supra_juiz_executor.py:72` (resposta), `:81` (texto sem instante), `:69` (sem resposta). Também `app/chamados/tests/test_e3_10_cura_no_escritor.py:132`, `:133`, `:134`. | O teste do CSF delega ao corpo real em `test_supra_juiz_executor.py:46`; só a tranca é substituída em `:58`. |
| M10 | `app/chamados/models.py:1923` | **Não**: qualquer `materializada_em` exclui. | Valor via CSF: `app/ponto/tests/test_supra_juiz_executor.py:85`. Forma textual: `app/chamados/tests/test_contract_dedup_status.py:143`. | `:85` combina origem validada e pergunta direta materializada; cada universo deve liberar o CSF. E-sem-carimbo: [nao verificado]. |
| M20 | F2_RESIDUO/SeloF2Test da c3 não localizado [nao verificado]. | Comparação daquele selo com BASE: [nao verificado]. | Prova equivalente cobrindo leitores fora de `estado`: [nao verificado]. | Existe selo de **escrita do par**, outro escopo: `app/chamados/tests/test_e3_porta_A_escritor_do_par.py:233`; usa qualname (`:189`, `:192`) e morde método novo (`:265`, `:266`). Não demonstra o F2_RESIDUO descrito. |

Asserções citadas foram lidas; resultado de execução: [nao verificado].

## 16/11 12:19 · sessao 0c12813eb48b · - · - · origem=Ronald (tmux)

A conclusão é **NÃO** para “já fixa o efeito como instante do ato de materialização, valor conservado”: hoje `[3]` é o campo atual `p.materializada_em`, sem leitura de ato de materialização; as portas podem substituí-lo e apagá-lo. Há conservação pontual opcional do carimbo, mas não o contrato descrito para `be2b8d733`. Fontes: `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1400`, `app/chamados/models.py:3292`, `app/chamados/models.py:3295`, `app/chamados/models.py:3318`, `app/chamados/models.py:3393`.

**Fonte campo a campo e efeito real**

- A projeção recebe `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em`, `via_resolucao` diretamente da pergunta; se houver `agora`, apenas recorta datas posteriores. Não há leitura de `DecisaoPergunta` nesse corpo. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1400`.
- `materializada_em` é `DateTimeField` anulável, com descrição “Quando pergunta foi processada (batida criada + chamado fechado)”. `materializada_por` é o autor; `via_resolucao` declara via, admitindo vazio por legado. `app/chamados/models.py:3530`, `app/chamados/models.py:3534`, `app/chamados/models.py:2666`.
- O escritor comum em memória atribui os três: `self.materializada_em = quando`, `self.materializada_por_id = autor_id`, `self.via_resolucao = via`. `app/chamados/models.py:3207`, `app/chamados/models.py:3209`, `app/chamados/models.py:3210`, `app/chamados/models.py:3211`.
- `sem_veredito_entre` **não chama a projeção temporal**: exclui qualquer `materializada_em is not None`, independentemente da via ou de E; também exige validação ausente e instante de resposta ou texto, com tranca opcional. `app/chamados/models.py:1922`, `app/chamados/models.py:1923`, `app/chamados/models.py:1924`, `app/chamados/models.py:1926`.
- `estado` **não chama a projeção diretamente**: validada com carimbo → `RESOLVIDA`; sem validação, pergunta viva → `PENDENTE`/`RESPONDIDA`; só depois, não viva com carimbo → `RESOLVIDA`. Portanto ter carimbo exclui da lista, mas não garante `RESOLVIDA` em todos os ramos. `app/chamados/models.py:3151`, `app/chamados/models.py:3153`, `app/chamados/models.py:3154`, `app/chamados/models.py:3156`, `app/chamados/models.py:3158`.

| Ramo | Quem escreve | Quem lê e consequência |
|---|---|---|
| Carimbo de identidade | `carimbar_identidade` aceita as vias de identidade, escolhe `quando or _tz.now()` e salva; com `manter_carimbo=True`, reutiliza instante/autor anteriores. A atribuição efetiva é `_escrever_carimbo`. Exemplos: `fechar_por_admin` e recusa por colisão. `app/chamados/models.py:3278`, `app/chamados/models.py:3288`, `app/chamados/models.py:3292`, `app/chamados/models.py:3295`, `app/chamados/models.py:3300`, `app/chamados/models.py:3209`, `app/chamados/models.py:3174`, `app/chamados/services/materializacao.py:1105`. | Projeção lê o campo atual; C6 lê via+carimbo nos degraus anterior à resposta e de identidade; lista lê apenas presença do campo; `estado` segue a precedência acima. `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1416`, `app/chamados/juizes.py:1443`, `app/chamados/juizes.py:1444`, `app/chamados/models.py:1923`, `app/chamados/models.py:3151`. |
| `materializada_em` solto, via vazia | O campo admite nulo e a via admite `''`; `_escrever_carimbo` é o sítio de atribuição. A porta `carimbar` não rejeita explicitamente `''`; recebe `via` e repassa ao escritor. Origem histórica ou escritor concreto dos registros soltos: **[nao verificado]**. `app/chamados/models.py:3530`, `app/chamados/models.py:2666`, `app/chamados/models.py:3228`, `app/chamados/models.py:3234`, `app/chamados/models.py:3239`, `app/chamados/models.py:3209`. | `[3]` recebe esse instante; `sem_veredito_entre` exclui a pergunta mesmo com via vazia. Em `estado`, validada+carimbo resolve; sem validação, o C6 ainda pode considerá-la viva. `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1400`, `app/chamados/models.py:1923`, `app/chamados/models.py:3153`, `app/chamados/models.py:3154`. |
| Materialização de fato E sem carimbo | E devolve booleano; não atribui `[3]` nem grava o carimbo. O materializador usa E como guarda e depois chama `carimbar` com instante capturado. A porta de pendentes também testa E e carimba depois. `app/chamados/juizes.py:666`, `app/chamados/juizes.py:763`, `app/chamados/juizes.py:765`, `app/chamados/services/materializacao.py:1239`, `app/chamados/services/materializacao.py:1250`, `app/chamados/services/materializacao.py:1283`, `app/chamados/services/materializacao.py:1422`, `app/chamados/services/materializacao.py:1433`, `app/chamados/services/materializacao.py:1437`. | Sem carimbo, `[3]` continua `None`. E sozinho não exclui da lista nem produz `RESOLVIDA`: a lista exige o carimbo para exclusão por materialização; `estado` exige carimbo para ambos retornos `RESOLVIDA`; C6 retorna viva antes de E quando chega ao degrau `materializada is None`. `app/chamados/juizes.py:1390`, `app/chamados/models.py:1923`, `app/chamados/models.py:3153`, `app/chamados/models.py:3156`, `app/chamados/juizes.py:1482`, `app/chamados/juizes.py:1483`, `app/chamados/juizes.py:1489`. |
| `materializada_conservada_em` | Não há equivalente desse valor na projeção atual: o valor efetivamente escrito/lido é `materializada_em`. Existência histórica ou fora da cópia: **[nao verificado]**. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1390`, `app/chamados/juizes.py:1400`, `app/chamados/models.py:3209`. | Não há ramo conservado nesse corpo; o retorno é o campo atual, eventualmente recortado por `agora`. `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1396`, `app/chamados/juizes.py:1400`. |

**Relógio fora da borda**

- A projeção e `sem_veredito_entre` não consultam relógio nesses corpos; usam os valores da pergunta e, na projeção, o `agora` recebido. `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1391`, `app/chamados/models.py:1922`.
- `estado` pode consultar relógio transitivamente: chama `pergunta_viva(self)` sem `hoje/agora`; o cálculo consulta `_dentro_da_janela_viva`, cujo fallback é `date.today()`. A classificação da falta também recebe `agora=None` e pode usar `_tz.now()`. `app/chamados/models.py:3154`, `app/chamados/juizes.py:898`, `app/chamados/juizes.py:1471`, `app/chamados/juizes.py:596`, `app/chamados/juizes.py:1475`, `app/escala/servico_jornada.py:387`.
- E também pode alcançar `date.today()` pela janela, mas **não é a fonte de `[3]` hoje**. `app/chamados/juizes.py:697`, `app/chamados/juizes.py:790`, `app/chamados/juizes.py:596`, `app/chamados/juizes.py:1390`.
- `supra_juiz` lê o efeito indiretamente ao chamar `sem_veredito_entre` nos universos pai e destino, ambos com `ignorar_tranca=True`; a decisão do CSF é a ausência dessas perguntas. `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:574`, `app/ponto/supra_juiz.py:588`. Mudança efetiva em competência exportada, sem banco/log/diff histórico: **[nao verificado]**.

**Por que NÃO, embora exista instante persistido**

O materializador captura um instante e o persiste pelo carimbo; a leitura atual reutiliza esse campo, sem gerar um novo instante. Isso atende parcialmente à descrição operacional de instante persistido. `app/chamados/services/materializacao.py:1250`, `app/chamados/services/materializacao.py:1283`, `app/chamados/models.py:3209`, `app/chamados/models.py:3262`, `app/chamados/juizes.py:1390`.

Mas o contrato de valor conservado não está fixado: `carimbar_identidade` só conserva mediante opção; caso contrário escreve outro instante. `descarimbar` e a forma em lote apagam o valor. O catálogo de `DecisaoPergunta` contém resposta, validação, devolução, recusa, reabertura, silêncio, retratação e absorção, sem ato de materialização listado. `app/chamados/models.py:3292`, `app/chamados/models.py:3295`, `app/chamados/models.py:3318`, `app/chamados/models.py:3393`, `app/chamados/models.py:4024`, `app/chamados/models.py:4026`.

_6027 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
