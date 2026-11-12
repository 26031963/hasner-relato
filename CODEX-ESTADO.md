# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 11:25** · publicado em **12/11/2026 11:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 11:25 · sessao dc6cc4c65e84 · - · - · origem=Ronald (tmux)

1. **Bloqueia a certificação literal de “pergunta preservada”**: o caso com ATA conserva marca positiva e zero turno aberto, mas o caso original sem célula foi enfraquecido: agora exige marca ausente e um turno aberto. Contraexemplo concreto: a mesma batida espúria de 10/06/2026 16:19, apagadas as células, satisfaz o novo selo com `turnos_abertos=1`, enquanto contrariaria o selo anterior com expectativa zero. A fonte única justifica ler a ausência de classificação; não demonstra preservação da garantia “nunca turno aberto” (`app/relatorios/tests/test_cartao_marca_espuria.py:53`, `:77`, `:97`, `:119`, `:124`, `:131`). A referência do calendário documenta perda da marca, não autorização para acusar turno aberto (`app/colaboradores/tests/test_calendario_orfa.py:18`).

2. **Fila — igualdade com a tela não provada pelo selo.** O comentário afirma “como é na tela”, mas o método só chama o coletor e exige contador 1; não chama a tela nem compara a data da lista (`app/relatorios/tests/test_cartao_marca_espuria.py:131`). O código explica por que 1 é plausível: tela incrementa cada `p.turno_aberto`, enquanto PDF exclui os horários classificados como órfãos; sem célula, a lista de órfãs é vazia (`app/ponto/services/espelho.py:424`, `:436`; `app/relatorios/pdf_espelho.py:512`; `app/escala/services/leitor_celula.py:394`; `app/escala/utils.py:1087`). O resultado integrado para essa fixture e o mesmo dia é **[nao verificado]**.

3. **Fila — mordida visual incompleta.** Mesmo o caso positivo verifica `batida.orfa`, ausência de “turno aberto” no texto e presença de “16:19”; não verifica asterisco, legenda ou separação da linha (`app/relatorios/tests/test_cartao_marca_espuria.py:81`, `:109`). Contraexemplo: renderizar a órfã junto das marcações comuns, mantendo o atributo e o horário no texto, não violaria esses asserts. O código atual efetivamente separa as marcações e aplica asterisco/cinza; portanto é lacuna de prova, não defeito visual demonstrado (`app/relatorios/pdf_espelho.py:832`, `:841`, `:851`).

4. **Fila — nova classe perde prova de emissão.** O override substitui a emissão/extração do PDF do método pai por uma checagem apenas do resumo. A presença da batida sem célula é verificada só no dict do coletor (`app/relatorios/tests/test_cartao_marca_espuria.py:103`, `:126`, `:131`). Contraexemplo: o renderer descartar a marcação sem célula preservaria esses asserts.

5. **Sem achado de segundo escritor produtivo neste retarget.** A criação direta de `CelulaDia` é fixture de teste expressamente informada pela sessão; não demonstra escritor produtivo fora da porta (`app/relatorios/tests/test_cartao_marca_espuria.py:66`). Porém “ATA que o cartório lavraria” permanece **[nao verificado]**: a fixture entrega lâmpadas e órfã prontas, sem executar builder/cartório para comparar a classificação (`app/relatorios/tests/test_cartao_marca_espuria.py:56`).

Leituras adicionais: o dia apresentado vem do juiz `dia_das_batidas`, que distingue batida reclamada por turno e batida sem turno; a tela agrupa por esse mapa, e não por dia civil próprio (`app/ponto/turnos.py:2886`; `app/ponto/services/espelho.py:453`, `:579`). Nenhuma execução integrada certificada nesta análise **[nao verificado]**.

## 12/11 11:25 · sessao c96f7f36ec7e · - · - · origem=Ronald (tmux)

(c) **Sem achado de RED puramente congelado no núcleo novo.** O carregador padrão lê os fontes desta cópia e compila AST real (`app/ponto/tests/test_lastro_c5_sem_banco.py:33-49`, `:108-118`). Os mutantes mudam AST produtiva: canal `_contrato`, reset do mapa, tipo no overlay e metadados da ata (`app/ponto/tests/test_lastro_c14_sem_banco.py:35-55`); overlay/canais/PDF da c13 também (`app/ponto/tests/test_lastro_c13_sem_banco.py:46-80`).

**Fila — alcance parcial e selos textuais.** Os dois selos F1c/cartão exercitados por c13 continuam sendo testes de presença/ausência de texto; executar suas funções não os transforma em prova comportamental (`app/ponto/tests/test_lastro_c13_sem_banco.py:127-149`; `app/relatorios/tests/test_cartao_pela_celula.py:170-181`). A prova dos canais extrai apenas chamadas reais dos builders e substitui o resto do corpo (`app/ponto/tests/test_lastro_c13_sem_banco.py:190-199`). Contraexemplo: quebrar a geometria antes da chamada `_marcar_ausencias` no builder não é percebido por esse recorte. Isso limita a prova, mas não elimina sua mordida sobre o repasse de `recusas` (`app/ponto/tests/test_lastro_c13_sem_banco.py:194-208`).

**RED histórico: reexecução nesta cópia [nao verificado].** Exige `git show` dos objetos c12/c13 (`app/ponto/tests/test_lastro_c13_sem_banco.py:26-45`; `app/ponto/tests/test_lastro_c14_sem_banco.py:29-34`). Os arquivos `docs/evidencia_c14_*.txt` citados no relato não constam nesta cópia; conteúdo/saídas históricas além do anexo [nao verificado].

(d) **Há transporte real em memória; não é só duplo.** O teste chama o builder single real e verifica folga, metadados, limpeza das órfãs e conservação da batida externa; a contraprova desliga apenas o overlay e reencontra `23:00` (`app/ponto/tests/test_lastro_c14_sem_banco.py:199-215`). Chama `ata_do_dia`, `lavrar_veredito(save=False)`, leitor, `grade_do_fechamento`, mapas financeiros e trecho real do fechamento; verifica mudança da ata antiga ausência → folga (`app/ponto/tests/test_lastro_c14_sem_banco.py:216-273`). Os mutantes removem classificação/metadados nos fontes reais (`:45-55`).

**Bloqueia o fechamento literal de (d), caso o requisito seja simular a cadeia integrada.** A passagem ao simulador substitui o recalculador real por `Mock(return_value={17: fech})`, classificador/TXT por duplos e roda `instantaneo(lido=True)` sobre esse fechamento já preparado (`app/ponto/tests/test_lastro_c14_sem_banco.py:280-306`). Contraexemplo: se o recalculador real deixar de ler a ata relavrada e retornar `{}`, esse teste continua verde, porque a implementação real foi substituída em `:295`. O simulador real solicita o recalculador em `app/folha/management/commands/simular_folha.py:129-134`; esse elo não é exercitado.

A antiga ata também é sintetizada a partir da nova, mudando só `tipo_dia`; não nasce de um primeiro julgamento ausência (`app/ponto/tests/test_lastro_c14_sem_banco.py:217-222`). Persistência é explicitamente proibida no teste (`:223-244`). Portanto: prova classificação/transporte/lavra/leitores em memória; execução `processar_cartorio` → persistência → recalcular → fotos/comparação [nao verificado].

O caminho shell capaz de atravessar a relavra existe: `--antes` roda antes do simulador; em `par`, aplica somente à foto depois (`bin/simular_folha.sh:65-74`, `:94-99`). Isso não é executado pelo teste c14 (`app/ponto/tests/test_lastro_c14_sem_banco.py:183-308`).

Execuções feitas agora, com `-B`/`PYTHONDONTWRITEBYTECODE`: c14 8/8, c13 10/10; mutante `relavra_tipo` falha na asserção `c14:207`; `ata_metadados` falha com `KeyError` em `c14:243`. Não há artefato gravado para esses resultados: [nao verificado].

## 12/11 11:25 · sessao c020ff845a76 · AUDITORIA-DE-TRAVAS-CORTE · censo · origem=Code

Regra: mantenho as regras 1–3 e o recorte da volta 1 (`medida_8_v1.r:3–5`). Aplico **A primeiro**: arquivo de algum teste vermelho alterado na mesma obra, com `run < commit ≤ pouso`. Depois **B**: nenhuma alteração elegível e **todos os IDs completos** repetidos em outras obras com distância absoluta ≤24 h. **C** é o restante. Associações de obra por pasta/assunto permanecem **[nao verificado]**. [nao verificado]

| Classe | Número | Linhas de `suites_14d.tsv` |
|---|---:|---|
| **A — MUDOU DECISAO** | **63** | `suites_14d.tsv:1,3,7,23–24,27,40,42,54,58,94,118,124,145,151–152,160,186–187,213,219,228,232,239,254,268–269,279,309,340,342,344,346,363–365,368,370,372,376,379,390,395–396,398,400–402,405,439,444–445,447,457–458,462,466,468,478,485,496–497,506` |
| **B — RUIDO** | **8** | `suites_14d.tsv:6,30,65,79,107,130,291,451` |
| **C — SEM SINAL** | **38** | `suites_14d.tsv:20,22,45,59,63,72,74–75,78,89–90,98,116,163,203,214,216–217,244,256,259,265,302,304,310,318,354,358,380,403–404,419,428,430,470,472,498–499` |

Correção da volta 1: **470 fica em C**. Seu vermelho de `test_selo_mapa_contrato` só reaparece fora da janela de 24 h nas outras obras identificadas (`suites_14d_falhas.tsv:30,79,114`; identidade das obras **[nao verificado]**).

Limite declarado: cura apenas em código de produção cai em **C**, não em A; a causa concreta dos casos C permanece **[nao verificado]**.

## 12/11 11:25 · sessao f2eb9b860cab · - · - · origem=Ronald (tmux)

(b) **Sem achado novo nos três sítios.** `_recusas_tela` nasce antes da seleção, entra no helper e sobrevive até o compositor; pré-apuração e `AtaAgregadaSemMarcos` usam esse helper (`app/ponto/services/espelho.py:486`, `:496`, `:498`, `:536`, `:565`, `:654`, `:655`, `:881`). A2 entrega a exceção no mapa e não conclui precedência nesse dia; overlay transporta o rótulo e `tipo_dia=None` (`app/ponto/precedencia.py:420`, `:423`, `:425`; `app/escala/utils.py:1218`).

**Sem except novo que engole.** Permanecem os genéricos antigos da seleção e da palavra; a captura explícita de `TipoSemFaixa` na composição relança (`app/ponto/services/espelho.py:574`, `:657`, `:660`; delta fornecido do arquivo). Falha HTTP integrada/ausência de outros 500: **[nao verificado]**; o handler chama o serviço diretamente (`app/ponto/views.py:157`).

**Fila — rótulo visível não garantido na UI.** Contraexemplo: ausência `fantasma_xyz` + batida em dia encerrado sem lavra dá título genérico; com `status_dia='ok'` previamente lavrado, mantém título antigo. D4 retorna antes de consultar recusas; HTML lê `status_rotulo` (`app/ponto/status_dia.py:290`, `:295`, `:317`; `app/templates/ponto/espelho.html:151`; caracterização `app/ponto/tests/test_lastro_c14_sem_banco.py:169`).

**Quem escreve/lê:**

- `recusas[data]`: `TipoSemFaixa` cria código/rótulo; A2 escreve mapa; overlay e espelho leem; D4 possui seu mapa e leitor (`app/ponto/precedencia.py:99`, `:423`; `app/escala/utils.py:1218`; `app/ponto/services/espelho.py:881`; `app/ponto/status_dia.py:301`, `:317`).
- `_recusa_do_dia`: espelho escreve; compositor lê e opcionalmente limpa; PDF chama esse mesmo compositor como último leitor (`app/ponto/services/espelho.py:881`, `:339`, `:349`; `app/relatorios/pdf_espelho.py:592`, `:594`).
- `palavra_dia`: inicializada vazia, preenchida pelo compositor comum; PDF lê para desenhar (`app/ponto/services/espelho.py:906`, `:340`, `:347`, `:1302`; `app/relatorios/pdf_espelho.py:879`). HTML da faixa usa outro campo (`app/templates/ponto/espelho.html:151`).

**Mordida real, com limites.** O ensaio executa seleção/handlers/helper reais e o valor real de `_recusa_do_dia`; builder e leitor/geometria são duplos; A2 posterior é propositalmente vazia para expor perda do primeiro transporte (`app/ponto/tests/test_lastro_c14_sem_banco.py:97`, `:103`, `:108`, `:113`, `:124`, `:136`, `:143`). Mutantes alteram AST produtivo do canal/reset, sem modificar arquivo (`app/ponto/tests/test_lastro_c14_sem_banco.py:24`, `:35`, `:39`). Executei quatro testes pertinentes: OK; canal removido deu dois erros e uma falha, reset deu três falhas. Registro persistido desta execução: **[nao verificado]**.

## 12/11 11:24 · sessao 89b671245bc2 · - · - · origem=Ronald (tmux)

Fatia `suites_14d.tsv:150–345`: **32 FAILED intermediárias**, com associação de obras por nome **[nao verificado]**.

| Classe | Total | Linhas de `suites_14d.tsv` |
|---|---:|---|
| A | 18 | 151, 152, 160, 186, 187, 213, 219, 228, 232, 239, 254, 268, 269, 279, 309, 340, 342, 344 |
| B | 1 | 291 |
| C | 13 | 163, 203, 214, 216, 217, 244, 256, 259, 265, 302, 304, 310, 318 |

A linha **279**, `jv3/suite_73a27d96_wip.log`, associa-se ao commit `790648d84`, assunto C20-JUIZ-3, pelo hash da suite seguinte; essa identidade de obra permanece **[nao verificado]**. O arquivo vermelho alterado é `app/ponto/tests/test_o_outbox_celula.py` **[nao verificado: número da linha no gitlog]**.

**309 é A; 310 é C**: a primeira run tem horário 05:37 e a cópia 05:55 (`suites_14d.tsv:309–310`); o commit `41e5dea51` que altera os três arquivos vermelhos tem horário 05:38 **[nao verificado: número da linha no gitlog; associação nominal]**.

Para **B291**, o ID completo `chamados.tests.test_contract_crons.ContratoPipelineB6Test.test_todo_command_tem_casa` também aparece na obra `f3_beco1`, 12h29 antes (`suites_14d.tsv:259`, `:291`; IDs em `suites_14d_falhas.tsv`, números das linhas **[nao verificado]**; distinção nominal das obras **[nao verificado]**).

## 12/11 11:24 · sessao d75940a3c4e3 · - · - · origem=Ronald (tmux)

Na fatia `suites_14d.tsv:346–526`, as 44 intermediárias FAILED dividem-se assim; associações entre obra e assunto **[nao verificado]**:

| Classe | Total | Linhas de `suites_14d.tsv` |
|---|---:|---|
| A | 31 | 346, 363–365, 368, 370, 372, 376, 379, 390, 395–396, 398, 400–402, 405, 439, 444–445, 447, 457–458, 462, 466, 468, 478, 485, 496–497, 506 |
| B | 1 | 451 |
| C | 12 | 354, 358, 380, 403–404, 419, 428, 430, 470, 472, 498–499 |

Correção da volta 1: **470 é C**. O ID vermelho de `test_selo_mapa_contrato` aparece somente em `suites_14d_falhas.tsv:30`, `:79` e `:114`; as outras duas ocorrências ficam fora de 24 horas da run 470 (`suites_14d.tsv:99`, `:358`, `:470`).

## 12/11 11:22 · sessao f5779ce1b0ab · - · - · origem=Ronald (tmux)

Nas linhas 1–149, são **33 INTERMEDIÁRIAS FAILED**, excluídas as FINAL e indeterminadas da volta 1; associações nominais **[nao verificado]**.

| Classe | Número | Linhas em `suites_14d.tsv` |
|---|---:|---|
| A | **14** | 1, 3, 7, 23, 24, 27, 40, 42, 54, 58, 94, 118, 124, 145 |
| B | **6** | 6, 30, 65, 79, 107, 130 |
| C | **13** | 20, 22, 45, 59, 63, 72, 74, 75, 78, 89, 90, 98, 116 |

Testemunhas A, todas posteriores à run e anteriores à FINAL candidata associada, conforme os anexos; números de linha do gitlog **[nao verificado]**:

| Linha da suite | Commit | Arquivo vermelho alterado | [nao verificado]
|---:|---|---|
| 1 | `caaf0e3ec`, 29/10 00:55 | `app/ponto/tests/test_passe_turno_aberto_retroativo.py` |
| 3 | `378481fd3`, 29/10 02:54 | `app/chamados/tests/test_contract_leitores_vitalidade.py` |
| 7 | `a1a19ce86`, 29/10 04:09 | `app/chamados/tests/test_contract_leitores_vitalidade.py` |
| 23 | `786a09283`, 29/10 17:33 | `app/escala/tests/test_ata_x_turno_entrada.py` |
| 24 | `786a09283`, 29/10 17:33 | `app/ponto/tests/test_a2_marco_morto.py` |
| 27 | `cbedeb91b`, 29/10 17:54 | `app/ponto/tests/test_selo_chokepoint_escrita.py` |
| 40 | `673ecaf79`, 30/10 12:17 | `app/api/tests/test_chamados_v2_destino.py` |
| 42 | `bfbc2aba7`, 30/10 13:04 | `app/chamados/tests/test_agrupado_em.py` |
| 54 | `8cb8a4c55`, 30/10 17:40 | `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py` |
| 58 | `4f0476aa5`, 30/10 19:51 | `app/chamados/tests/test_b2d_resposta_visivel.py` |
| 94 | `1d01bd905`, 31/10 07:39 | `app/ponto/tests/test_a2_marco_morto.py` |
| 118 | `973753a24`, 31/10 17:34 | `app/ponto/tests/test_contract_no_batida_date.py` |
| 124 | `f6f420ec3`, 31/10 23:54 | `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py` |
| 145 | `5ec1220ad`, 01/11 12:33 | `app/ponto/tests/test_cobparc_marco_no_periodo.py` |

Os vínculos `grupo1` + `grupo1p1` nas linhas 23–24 e `v3per` + `v3p5` na linha 124 seguem o agrupamento da volta 1 **[nao verificado]**.

## 12/11 11:21 · sessao 94df418df845 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Curas, REDs, mutantes e comando para a sombra estão no [LASTRO_C14.txt](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C14.txt:7).

**81 testes sem banco e ruff passaram**, com RC=0 ([saídas](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/evidencia_c14_validacao.txt:403)). [nao verificado]

A entrega permanece parcial: parei o ponto do selo de marca espúria porque sua premissa depende da grade própria, conforme sua condição literal ([pendência](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/LASTRO_C14.txt:36)). Os vizinhos Django não carregaram por falta da dependência ([saída](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/evidencia_c14_validacao.txt:109)).

Sem execução da sombra ou commit ([escopo](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/evidencia_c14_escopo.txt:35)). [nao verificado]

## 12/11 11:19 · sessao 49c101e1bfcf · AUDITORIA-DE-TRAVAS-CORTE · censo · origem=Code

Regra adotada, antes dos números:

1. Uma linha do TSV = uma run, incluindo cópias em `opus_final`. A numeração abaixo começa na primeira linha de cada anexo.
2. Associação pela pasta; separação por nome quando a pasta reúne obras diferentes, como E1/E2/E1-FILA em `fio_e0_v4`. Associações e agrupamentos por nome são **[nao verificado]**.
3. FINAL candidata = última linha com horário **estritamente anterior** ao pouso associado. Empate entre runs: última linha do TSV. Empate com o minuto do commit, ausência de pouso identificável ou execução posterior ao pouso: classificação temporal indeterminada **[nao verificado]**. [nao verificado]
4. FAILED intermediária: primeiro procurar alteração posterior do arquivo/módulo em commit da mesma obra; depois verificar se **todos os IDs completos** dos testes vermelhos aparecem em outras obras com distância absoluta ≤24 h; restante indeterminado. Coincidência não prova causalidade **[nao verificado]**. [nao verificado]

**O censo fecha por linha; o número de “mudaram decisão” não é verificável com o material fornecido.** O gitlog não informa arquivos alterados por commit, e o código atual não permite reconstruir essas alterações históricas **[nao verificado]**. [nao verificado]

| Classificação nominal | Total | FAILED | OK |
|---|---:|---:|---:|
| FINAL candidatas | 88 | 3 | 85 |
| INTERMEDIARIAS | 344 | 109 | 235 |
| Classificação temporal indeterminada | 94 | 28 | 66 |
| **Total** | **526** | **140** | **386** |

Fonte: `suites_14d.tsv:1–526`; classificação por associação de nomes **[nao verificado]**.

São **525 linhas com rótulo de pasta/obra e 1 SEM obra**, `integrador.log`. Rótulo de pasta não comprova identidade da obra **[nao verificado]**. Fontes: `suites_14d.tsv:1–526`, especialmente `suites_14d.tsv:526`.

As **94 temporalmente indeterminadas** são: 84 sem pouso identificável, oito empatadas no minuto do pouso, uma posterior ao pouso conhecido (`lastro2/construcao_green.log`) e uma cujo commit associado diz “cura”, sem pouso explícito (`o261/suite_bd0aef09.log`) **[nao verificado]**.

Para classificar cada linha sem reproduzir os 526 registros: estas são as **88 FINAL candidatas**; a lista seguinte contém as 94 temporalmente indeterminadas; **todas as demais linhas são as 344 INTERMEDIARIAS**, segundo a regra acima **[nao verificado]**.

| Data | Linhas FINAL candidatas em `suites_14d.tsv` |
|---|---|
| 29/10 | 2, 19, 21, 29, 32 |
| 30/10 | 39, 50, 56, 60, 71 |
| 31/10 | 83, 84, 88, 92, 93, 97, 99, 108, 112, 114, 119, 123, 138 |
| 01/11 | 141, 149, 159 |
| 02/11 | 170, 173, 181, 183, 185, 194 |
| 03/11 | 199 |
| 04/11 | 204, 207, 208, 210–212, 215, 218, 222, 224–225 |
| 05/11 | 235, 238, 248, 251–252, 260, 263–264, 273, 275, 277, 280, 283, 289 |
| 06/11 | 303, 312–313, 319–320, 322, 330–331, 335, 343 |
| 07/11 | 353, 357, 378, 389 |
| 08/11 | 407, 437 |
| 09/11 | 443, 446, 448, 456, 465, 469, 471 |
| 10/11 | 474, 477, 487 |
| 11/11 | 502–503, 509, 516 |

As três FINAL candidatas FAILED são ORFAS, O261-V e LASTRO-3 (`suites_14d.tsv:19`, `suites_14d.tsv:71`, `suites_14d.tsv:313`); associação aos pousos **[nao verificado]**.

As 94 linhas temporalmente indeterminadas, em `suites_14d.tsv`, são **[nao verificado: associação ao pouso]**: [nao verificado]

```text
33–34, 36–38, 57, 61–62, 73, 96, 100, 102, 150, 171,
174–180, 191, 193, 198, 200–201, 209, 226–227, 243, 253,
284, 287, 290, 292–296, 298–300, 305–308, 315, 317,
323–326, 332–334, 336, 339, 366, 409, 449–450, 453–454,
460, 463–464, 480, 488–495, 504–505, 508, 510–515, 517–526
```

**Das 109 INTERMEDIARIAS FAILED, nenhuma pode ser atribuída com comprovação a “mudou decisão” ou a “ruído exclusivo”.** Todas permanecem indeterminadas quanto a essa divisão **[nao verificado]**.

Dentro delas, **17 atendem ao critério observável de coincidência integral em outras obras em até 24 h**: linhas 6, 30, 65, 79, 107, 130, 291, 365, 368, 439, 447, 451, 466, 468, 470, 478 e 506 de `suites_14d.tsv`. Os IDs constam de `suites_14d_falhas.tsv:3`, `:11`, `:20`, `:26`, `:31`, `:35`, `:63`, `:82–83`, `:103`, `:106`, `:108`, `:112–114`, `:116` e `:132`; distinção entre obras **[nao verificado]**.

Essas 17 são **candidatas a ruído**, pois falta excluir alteração posterior na própria obra. As outras **92** não preencheram esse critério integral; isso também não demonstra cura **[nao verificado]**.

Das OK, são **235 intermediárias**, sem mudança de decisão pela definição solicitada. As **85 OK escolhidas como últimas antes do pouso** estão entre as FINAL candidatas; outras **66 OK** têm classificação temporal indeterminada (`suites_14d.tsv:1–526`; associação **[nao verificado]**). [nao verificado]

| Tabela final solicitada | Resultado |
|---|---:|
| Total | **526** |
| Intermediárias na reconstrução nominal | **344** **[nao verificado]** |
| Intermediárias que mudaram decisão | **[nao verificado] — 0 comprovadas** |
| Ruído exclusivo | **[nao verificado] — 0 comprovadas** |
| FAILED intermediárias indeterminadas | **109**, incluindo 17 candidatas a ruído |
| Runs sem classificação FINAL/INTERMEDIARIA resolvida | **94** **[nao verificado]** |

Fonte das contagens: `suites_14d.tsv:1–526` e `suites_14d_falhas.tsv:1–140`. “0 comprovadas” expressa ausência de comprovação, **não zero ocorrências reais [nao verificado]**.

As dez obras com mais intermediárias **na reconstrução nominal**, excluídas as 94 linhas temporalmente indeterminadas, são **[nao verificado: associações por nome]**:

| Obra/agrupamento adotado | Intermediárias | Fonte em `suites_14d.tsv` |
|---|---:|---|
| FIO-E0-V4-E1 | 61 | `:346–437` |
| FIO-E0-V4-E2 | 14 | `:358–448` |
| COBPARC | 13 | `:142–170` |
| GRADE-FOLGA-JUIZ (`grade_folga` + `grade_juiz`) | 13 | `:106–149` |
| O261-E-F5C (`F5` + `F5C`) | 13 | `:63–88` |
| JUIZ-VARREDURA-1 | 10 | `:228–264` |
| O261-VETO-A | 10 | `:40–50` |
| ORFAS | 10 | `:3–19` |
| V3-P5 (`v3per` + `v3p5`) | 10 | `:124–141` |
| V2-BUGS-DO-MOTOR, item 2 | 9 | `:338–357` |

O ranking definitivo e a quantidade causal pedida no item 8 permanecem **[nao verificado]**, pelas associações nominais, pelas 94 posições temporais não resolvidas e pela ausência dos arquivos alterados por commit. [nao verificado]

## 12/11 11:15 · sessao 78f2590fba58 · - · - · origem=Ronald (tmux)

Mapa consolidado: **88 FINAL estritas: 85 OK e 3 FAILED**; **94 com classificação temporal indeterminada**; sobre 526 linhas, restam **344 INTERMEDIÁRIAS**. Todas as associações obra↔pasta/pouso são por nome **[nao verificado]**. [nao verificado]

Os três FAILED finais são `orfas/suite_inteira_6.log` (10-29 08:53), `o261v/suite3_9c9e030e.log` (10-30 23:07) e `lastro3/suite_v3.log` (11-06 09:00). **[nao verificado: associação por nome]**

As 94 linhas temporalmente indeterminadas compreendem:

- 84 linhas em 25 grupos sem pouso identificável posterior. [nao verificado]
- 8 linhas empatadas no minuto do pouso: `o261v`, `medida_programa`, `o261e/d3`, `disjuntor`, `e3solo`, `mc`, `jv2p`, `h12h06b/pauta08`. [nao verificado]
- `lastro2/construcao_green.log`, posterior ao pouso conhecido.
- `o261/suite_bd0aef09.log`, cuja associação depende de tratar um commit “cura” como pouso. **[nao verificado]**

Para as duas linhas `lastro2/.../suite_v2.log` de 11-06 05:55, a FINAL única usa a última linha do TSV, `lastro2/suite_v2.log`; a ordem real entre ambas é **[nao verificado]**.

A lista completa das 88 FINAL e os hashes dos pousos foi enviada nas mensagens anteriores.

## 12/11 11:15 · sessao 628c0cf32473 · - · - · origem=Ronald (tmux)

Não há `.git` nem anexos locais nesta cópia; não foi possível verificar arquivos alterados por commit. O gitlog fornecido contém assunto, data e hash, sem diff. Portanto, “mudou decisão” e “ruído exclusivo” não são quantificáveis com comprovação. [nao verificado] [nao verificado]

Coincidências integrais candidatas a ruído, contando IDs completos de teste e distância absoluta ≤24 h: [nao verificado]

| Linhas em `suites_14d_falhas.tsv` | Contrapartes |
|---|---|
| 3, 5, 11 | Entre si: diagrama |
| 20 | 18: import resolve |
| 26 | 18: diagrama |
| 31 | 26 ou 35: diagrama |
| 35 | 31 ou 37: diagrama |
| 63 | 57 ou 73: cron |
| 69 | 77: chokepoint |
| 82 | 83: signal âncora |
| 83 | 80 ou 82: signal âncora |
| 103 | 105; alternativamente 112 para `classificar` e 104 para `linhas` |
| 106, 107, 108 | 103–105 e entre si: catraca linhas |
| 112, 113 | 103 ou 105: classificar |
| 114 | 109: diagrama e mapa |
| 116 | 121, 124, 127 ou 132: catraca linhas |
| 132 | 116, 121, 124, 127 ou 135: catraca linhas |

São **20 candidatas**, separando E1/E2/FILA dentro de `fio_e0_v4`; **18** se usar somente pasta raiz. Todas as separações de obra por nome são [nao verificado]. Referências: linhas indicadas de `suites_14d_falhas.tsv`.

Duas candidatas parecem FINAL FAILED, devendo sair das intermediárias: linha 5, `orfas/suite_inteira_6.log`, antes de `ac7216078` em 29/10 09:24; linha 69, `lastro3/suite_v3.log`, antes de `24946096d` em 06/11 09:43. Associação por nome [nao verificado]. Isso deixa **18 candidatas intermediárias** com divisão E1/E2/FILA, ou **16** com pasta raiz.

A precedência “cura antes de ruído” continua impossível de verificar: as candidatas podem também ter recebido alterações posteriores na própria obra. A classificação estrita deve manter todo FAILED intermediário como indeterminado e apresentar as coincidências como subtotal observável. [nao verificado]

O código atual permite resolver módulos, por exemplo `app/core/tests/test_medida_turno_do_par.py:544`, `app/core/tests/test_medida_turno_do_par.py:550` e `app/core/tests/test_selo_diagrama_do_codigo.py:42`; ele não fornece o histórico de alterações. [nao verificado]

Há inconsistência na linha 140 do anexo de falhas: veredito `failures=5`, mas lista extensa de FAIL e dois ERROR. Conta como uma linha/run; composição histórica dessa lista [nao verificado].

## 12/11 11:14 · sessao ae97398abee9 · - · - · origem=Ronald (tmux)

Censo por linha do anexo: **526 runs = 140 FAILED + 386 OK; 111 pastas/raízes distintas**, incluindo `integrador.log` como raiz sem pasta. Fontes: `suites_14d.tsv:1–526`, `suites_14d_falhas.tsv:1–140` (numeração do anexo textual).

Formato abaixo: `pasta total/FAILED`; OK = total − FAILED. Fonte de todas as contagens: `suites_14d.tsv:1–526`.

```text
fio_e0_v4 83/27
o261e 40/13
cobparc 14/3
o261 12/3
orfas 11/3
e4cal 11/2
jv1 11/2
c20_fiomudo 11/2
v2bugs 10/2
grade_juiz 9/1
BO-0611-05 9/2
col129 8/2
v3p5 8/0
f3_beco1 8/3
dme 7/0
otimiza3 7/0
FIO-E0-V4-E3 7/4
calendario_hoje 6/1
cobparc_relogio 6/1
jv3 6/2
jv4 6/0
h12h06 6/1
h12h06b 6/1
fio_e3 6/3
BO-DISPUTA-SEM-PERGUNTA 6/6
E4B-ALCANCE 6/4
grupo1 5/3
v3retoma 5/0
grade_folga 5/1
f1c4emis 5/1
lastro2 5/2
esemvivab 5/0
LASTRO-FAIXA 5/5
justif 4/2
o261n 4/1
o261v 4/2
vfase_asof 4/0
mc 4/1
n1im 4/1
f346 4/0
plantio_mais1 4/3
motor_turno 4/0
par_nao_adjacente 4/1
selos_canon 4/1
FIO-E0-V4-E4-ESCRITORES 4/2
FIO-E0-V4-E4A-2 4/1
disjuntor 3/1
e4_7iii 3/1
v3per 3/1
lenvivos 3/0
toast 3/0
e4app 3/2
juizvarredura2 3/0
n1alim 3/1
snc2 3/1
pep3 3/1
o10c 3/0
codexneg 3/0
cxr_tela 3/1
UI-PAINEL 3/1
DATA-EVENTO-CRU 3/0
TRIAGEM-OFFLINE-SEM-CAMERA 3/0
outbox 2/1
grupo1p1 2/0
suite_impacto 2/1
medida_programa 2/0
e3status 2/1
comboio 2/0
e3isento 2/0
cobparc_rotulo 2/0
isento_h10 2/0
noreverse 2/0
n1pna 2/2
he 2/0
cronsmarca 2/0
c20_o169 2/0
pep 2/2
akmed 2/0
ferias2x 2/1
e1fila_v22 2/1
CODEX-2-RAIAS 2/0
MODO-COMUM-PASSADO-SEM-TURNO 2/1
O169-FECHADA 2/0
COMBOIO-1556 2/1
fioadmin 1/0
e3solo 1/0
reprova_desenho 1/0
instr 1/0
comboio_instr 1/0
e4_comboio 1/0
contencao 1/0
cods_ato 1/0
e4_8 1/0
e4esp 1/0
cron15 1/0
sw 1/0
jv2p 1/0
sombra_c1 1/0
lastro3 1/1
fiomudob 1/0
pep5 1/0
o10b 1/0
r1pub 1/0
comboio_0611 1/0
esminst 1/0
ata_pausa_0811 1/1
auditoria_0911 1/0
REVERTER-DIA-SEM-RECALCULO 1/0
FIO-E0-V4-E4B 1/1
BO-ROTULO-ENCERRADO 1/0
integrador.log 1/1
```

Separação solicitada por nome de arquivo — identidade de obra [nao verificado]:

| Pasta | Stream inferido | Runs | FAILED | OK |
|---|---|---:|---:|---:|
| fio_e0_v4 | E1 | 62 | 20 | 42 |
| fio_e0_v4 | E2 | 15 | 6 | 9 |
| fio_e0_v4 | E1FILA | 6 | 1 | 5 |
| o261e | F1 | 4 | 4 | 0 |
| o261e | D3 | 1 | 0 | 1 |
| o261e | F5 | 8 | 2 | 6 |
| o261e | F5C | 6 | 1 | 5 |
| o261e | F1C4 | 5 | 2 | 3 |
| o261e | F1C4-3 | 3 | 1 | 2 |
| o261e | F2 | 9 | 2 | 7 |
| o261e | F2FILA | 3 | 1 | 2 |
| o261e | dj → DISJUNTOR | 1 | 0 | 1 |

E2 inclui 11 `green_e2*` em 07/11, `e2_grade_recorte` em 08/11 e três `e2_rebase` em 09/11. E1FILA inclui quatro `e1_fila*` em 08/11 e dois em 09/11. Fonte: `suites_14d.tsv:347–448`. Associações [nao verificado].

Há **10 linhas em `opus_final`** com Ran/tempo/veredito repetidos fora da subpasta: 3 FAILED e 7 OK; mantidas na contagem. Distribuição: jv3 1, c20_fiomudo 4, pep 1, otimiza3 2, lastro2 2. Fonte: `suites_14d.tsv:282–312`; identidade física da execução [nao verificado].

Faixas do anexo úteis para citações:

```text
29/10: 1–33
30/10: 34–74
31/10: 75–139
01/11: 140–168
02/11: 169–198
03/11: 199
04/11: 200–228
05/11: 229–289
06/11: 290–346
07/11: 347–397
08/11: 398–441
09/11: 442–473
10/11: 474–490
11/11: 491–520
12/11: 521–526
```

## 12/11 11:06 · sessao e7500f65d2e9 · AUDITORIA-DE-TRAVAS-CORTE · censo · origem=Code

Numerei cada anexo desde a primeira linha de dados. Usei **p50 = mediana** e **p95 = valor na posição `ceil(0,95 × n)`**, após ordenar.

**Item 6 — chamadas do wrapper**

“Total” abaixo significa últimas 20 chamadas globais; leitura/censo/sonda formam um grupo.

| Janela | n | p50 (s) | p95 (s) | Máximo (s) | rc=124 | >900 s | Fonte |
|---|---:|---:|---:|---:|---:|---:|---|
| Construtor | 20 | 715,5 | 1.630 | 1.928 | 0 | 7 | `chamadas_fim_vivo.tsv:316–395`, filtrando papel |
| Verificador | 20 | 325,5 | 594 | 656 | 0 | 0 | `chamadas_fim_vivo.tsv:335–400`, filtrando papel |
| Leitura/censo/sonda | 20 | 204,5 | 891 | 1.178 | 0 | 1 | `chamadas_fim_vivo.tsv:351–399`, filtrando grupo |
| Total global | 20 | 332 | 1.068 | 1.928 | 0 | 2 | `chamadas_fim_vivo.tsv:381–400` |
| União das três janelas | 60 | 386,5 | 1.206 | 1.928 | 0 | 8 | Mesmas seleções acima |

No anexo inteiro, os **três rc=124** são: construtor/904 s, construtor/905 s e censo/905 s; estão fora dessas janelas. (`chamadas_fim_vivo.tsv:66,130,317`)

| Suites | n | p50 (s) | p95 (s) | Máximo (s) | Fonte |
|---|---:|---:|---:|---:|---|
| Últimas 20 linhas `Ran` | 20 | 1.306,5525 | 1.638,001 | 1.788,073 | `suites_14d.tsv:507–526` |
| Últimas 20 aparentemente completas¹ | 20 | 1.316,237 | 1.638,001 | 1.788,073 | `suites_14d.tsv:500–525`, seleção por nome/tamanho |

¹ A classificação como completa usa o nome do log e o número de testes; a fase efetiva da execução não consta do anexo. [não verificado] [nao verificado]

O **timeout de 900 s do wrapper deve ser avaliado pelas chamadas Codex**. Ele envolve `codex exec`, tanto no construtor quanto nos demais papéis; o construtor recebe `-k 30`. Nesta cópia, os locais são `bin/codex_pergunta.sh:401` e `:408`, e o default permanece em `:83`. O próprio cabeçalho atribui a certificação por suite ao Code. (`bin/codex_pergunta.sh:331`)

Há uma ressalva na medida: `duracao_s` começa antes da preparação e da espera por vagas/locks. Portanto, **passar de 900 s não demonstra que o `codex exec` excedeu seu timeout**, nem revela o `CODEX_TIMEOUT` efetivamente usado naquela chamada. (`bin/codex_pergunta.sh:252,260,348–376`; valor efetivo por chamada: [não verificado])

Outro teto de **900 s** existe em **`bin/encadeado.sh:71`**: espera carimbos de conclusão de cron e sai com rc=2 quando vence; não é timeout de suite nem de Codex. (`bin/encadeado.sh:84–96`)

As contas das margens são:

| Cobertura pretendida | Conta `p95 + margem` | Teto empírico | Fonte |
|---|---|---:|---|
| Últimas 20 suites | `1.638,001 + (1.788,073 − 1.638,001)` | **1.788,073 s → 1.789 s inteiros** | `suites_14d.tsv:514,524` |
| Últimos 20 construtores | `1.630 + (1.928 − 1.630)` | **1.928 s** | `chamadas_fim_vivo.tsv:365,386` |
| Wrapper cobrindo também o maior rc=0 de todo o anexo | `1.630 + (2.806 − 1.630)` | **2.806 s** | `chamadas_fim_vivo.tsv:214,365` |

**Proposta empírica para o wrapper: 2.807 s**, isto é, `1.630 + 1.177`; a margem é `(2.806 − 1.630) + 1 s`, ficando estritamente acima do máximo registrado, com resolução inteira. Usar apenas a janela recente daria 1.929 s pelo mesmo critério. (`chamadas_fim_vivo.tsv:214,365,386`)

Isso cobre os máximos de **duração registrada**; a afirmação de que algum dos três rc=124 foi falso, ou de que esse teto evitaria falsos rc=124 futuros, permanece **[não verificado]**.

**Item 8 — “mudaram decisão”: não medido; não significa zero.**

Há **526 registros `Ran`, 140 deles FAILED**. O anexo informa totais de falhas, mas não identifica os testes; também não marca sistematicamente intermediária/final. Há possíveis cópias em diretórios `opus_final`, impedindo equiparar linhas a execuções independentes. (`suites_14d.tsv:1–526`; exemplos de cópias: `281–282,302–304,309–312`) [nao verificado]

O caso E3 mostra o limite: a suite tem sete falhas, um commit posterior diz “pós-suite: S1–S7 curados”, depois aparecem três falhas e finalmente verde. O título truncado não identifica os casos da execução nem fecha a prova pedida até a final. (`suites_14d.tsv:497–500`; `gitlog_14d.txt:59,30`) [nao verificado]

A tabela abaixo cobre **todos os agrupamentos com algum FAILED**. **NM = não medido** em ambas as colunas solicitadas. Os números entre parênteses são o censo verificável: **registros / FAILED**. Minutos são a soma bruta de `Ran ... in Xs ÷ 60`, **não minutos comprovados de intermediárias ou de economia**. Não tratei a última observação como final. (`suites_14d.tsv:1–526`)

| Obra/prefixo do anexo | Intermediárias (registros/FAILED) | Mudaram decisão | Minutos brutos | Linhas em `suites_14d.tsv` |
|---|---:|---|---:|---|
| outbox | NM (2/1) | NM | 18,640 | 1–2 |
| orfas | NM (11/3) | NM | 119,192 | 3–19, prefixo |
| col129 | NM (8/2) | NM | 93,913 | 6–21, prefixo |
| grupo1 | NM (5/3) | NM | 72,541 | 22–26 |
| justif | NM (4/2) | NM | 59,898 | 27,30–32 |
| o261n | NM (4/1) | NM | 33,198 | 34,36–38 |
| o261v | NM (4/2) | NM | 63,096 | 35,59,71,73 |
| o261/veto_a2 | NM (11/3) | NM | 166,253 | 40–50 |
| calendario_hoje | NM (6/1) | NM | 85,975 | 51–56 |
| suite_impacto — gerações distintas | NM (2/1) | NM | 49,138 | 57,226 |
| o261e, exceto f2/f2fila | NM (28/10) | NM | 327,878 | 58–97, prefixo |
| disjuntor | NM (3/1) | NM | 44,711 | 78,84,96 |
| e3status | NM (2/1) | NM | 30,745 | 98–99 |
| grade_folga | NM (5/1) | NM | 69,002 | 106,111,116,120,122 |
| f1c4emis | NM (5/1) | NM | 84,507 | 107,109,113,117,119 |
| e4_7iii | NM (3/1) | NM | 43,106 | 118,121,123 |
| v3per | NM (3/1) | NM | 39,316 | 124–126 |
| grade_juiz | NM (9/1) | NM | 150,402 | 127–149, prefixo |
| cobparc | NM (14/3) | NM | 208,564 | 142–170, prefixo |
| o261e/f2 | NM (9/2) | NM | 108,488 | 151–159 |
| cobparc_relogio | NM (6/1) | NM | 79,354 | 174–179 |
| e4cal | NM (11/2) | NM | 168,778 | 186–199, prefixo |
| mc | NM (4/1) | NM | 69,594 | 203,205,207,209 |
| e4app | NM (3/2) | NM | 36,395 | 213–215 |
| n1pna | NM (2/2) | NM | 41,197 | 216–217 |
| n1im | NM (4/1) | NM | 40,556 | 219–222 |
| jv1 | NM (11/2) | NM | 169,889 | 228–264, prefixo |
| n1alim | NM (3/1) | NM | 28,680 | 232–233,235 |
| jv3 | NM (6/2) | NM | 93,137 | 239,241,279,281–283 |
| o261e/f2fila | NM (3/1) | NM | 41,743 | 244,246,248 |
| f3_beco1 | NM (8/3) | NM | 152,549 | 256–275, prefixo |
| h12h06 | NM (6/1) | NM | 99,348 | 265–280, prefixo |
| snc2 | NM (3/1) | NM | 37,106 | 268,271,273 |
| c20_fiomudo | NM (11/2) | NM | 119,175 | 284–300, prefixo |
| h12h06b, incluindo pauta08 | NM (6/1) | NM | 103,853 | 291,297,301,303,331–332 |
| pep | NM (2/2) | NM | 34,120 | 302,304 |
| lastro2 | NM (5/2) | NM | 76,021 | 309–312,508 |
| lastro3 | NM (1/1) | NM | 17,170 | 313 |
| pep3 | NM (3/1) | NM | 26,640 | 314,316,318 |
| plantio_mais1 | NM (4/3) | NM | 63,081 | 333–334,336,339 |
| v2bugs/i2 | NM (10/2) | NM | 82,524 | 338–357, prefixo |
| ferias2x | NM (2/1) | NM | 36,728 | 342–343 |
| fio_e0_v4 — mistura E1/E2/fila/rebase | NM (83/27) | NM | 1.055,536 | 346–448, prefixo |
| cxr_tela | NM (3/1) | NM | 64,876 | 405–407 |
| ata_pausa_0811 | NM (1/1) | NM | 19,673 | 409 |
| e1fila_v22 | NM (2/1) | NM | 38,836 | 444,446 |
| par_nao_adjacente | NM (4/1) | NM | 87,109 | 449–450,453–454 |
| selos_canon | NM (4/1) | NM | 82,189 | 451–452,455–456 |
| fio_e3 — 09/11 | NM (6/3) | NM | 124,964 | 457–465, prefixo |
| FIO-E0-V4-E4-ESCRITORES | NM (4/2) | NM | 78,317 | 466–469 |
| MODO-COMUM-PASSADO-SEM-TURNO | NM (2/1) | NM | 39,387 | 470–471 |
| FIO-E0-V4-E4A-2 | NM (4/1) | NM | 79,135 | 472–473,476–477 |
| BO-0611-05 | NM (9/2) | NM | 132,060 | 478–487, prefixo |
| BO-DISPUTA-SEM-PERGUNTA | NM (6/6) | NM | 112,739 | 480,488,490–491,493,495 |
| FIO-E0-V4-E4B | NM (1/1) | NM | 3,754 | 489 |
| E4B-ALCANCE | NM (6/4) | NM | 128,479 | 492,494,504–505,515,519 |
| FIO-E0-V4-E3 — 11/11 | NM (7/4) | NM | 161,441 | 496–502 |
| UI-PAINEL | NM (3/1) | NM | 69,216 | 506–507,516 |
| COMBOIO-1556 | NM (2/1) | NM | 49,174 | 513–514 |
| LASTRO-FAIXA | NM (5/5) | NM | 98,717 | 521–525 |
| integrador | NM (1/1) | NM | 6,606 | 526 |

**Falta, para cada linha NM:** identificar a execução e sua fase, os testes que ficaram vermelhos, o commit que curou esses mesmos testes e a execução final que confirma a cura. A correspondência entre prefixos e obras também precisa fechar onde há agrupamentos mistos. [não verificado] [nao verificado]

Além disso:

- **BO-DISPUTA e LASTRO-FAIXA:** a última observação ainda é FAILED; falta a final. (`suites_14d.tsv:495,525`)
- **FIO-E4B ↔ E4B-ALCANCE:** falta comprovar a associação entre esses nomes. [não verificado]
- **jv3, c20_fiomudo, pep e lastro2:** falta distinguir cópia de log de execução independente. (`suites_14d.tsv:281–282,287,292–295,298–299,302,304,309–312`)
- **Demais prefixos:** seus registros não contêm FAILED; isso não identifica quantas execuções foram intermediárias nem comprova a final. (`suites_14d.tsv:1–526`, excluindo os agrupamentos da tabela)

Assim, **140 linhas FAILED não podem ser convertidas em 140 suites intermediárias que mudaram decisão**. O número solicitado no item 8 permanece **não medido** pelos anexos fornecidos. (`suites_14d.tsv:1–526`; vínculo causal: [não verificado])

## 12/11 11:05 · sessao f9aecd196290 · - · - · origem=Ronald (tmux)

Proposta concreta, sem escrita:

1. Use builder completo, sem reaproveitar `julgar_sem_adesao`: esse harness força ausência de batidas e testa outro caminho (`ponto/tests/test_lastro_c5_sem_banco.py:566-567`).

2. O builder completo mais curto é `montar_grade_prevista`: aceita escala/horários e batidas em memória; executa pareamento, cobertura e classificação para folga (`escala/utils.py:668`, `escala/utils.py:805-814`). Limite: esse builder não publica minutos previstos/realizados; não atribuir a ele uma prova financeira positiva (`escala/utils.py:791`). Para provar esses valores, usar o builder por turno (`escala/utils.py:1301`).

3. Crie célula em memória com ata antiga `tipo_dia='ausencia'`; rode builder com ausência `folga` integral e batida. A cobertura deve produzir `tipo_dia='folga'`, transportar ausência e limpar órfãs/células, preservando a batida de entrada externa (`escala/utils.py:1243-1254`, `escala/utils.py:1275-1277`).

4. Execute `ata_do_dia`, depois a porta real por AST: `lavrar_veredito`, `veredito_que_a_lavra_escreve`, `_status_alvo`, `_aplicar_status`; substitua somente emissão/persistência por duplos. Assert obrigatório: retorno contém `ata`, ata mudou de ausência para folga e metadados sobreviveram (`ponto/services/cartorio.py:721-768`, `ponto/portas/celula.py:912`, `ponto/portas/celula.py:951-1054`). Ler esse arquivo não exige editá-lo.

5. Para provar leitores: `grade_do_fechamento` real → `grade_da_celula` real → `por_dia_da_grade` real → bloco do fechamento que publica mapas e causa. Execute `_retidos` real de `simular_folha` com esse fechamento e `prontidao` real; isso demonstra o leitor do simulador, com integração completa/motor/TXT ainda `[nao verificado]` (`folha/export.py:329-338`, `escala/services/leitor_celula.py:397-438`, `ponto/services/fechamento.py:1014-1085`, `folha/management/commands/simular_folha.py:68-93`).

Dois pontos que precisam constar do relato:

- Batida isolada não implica `dias_trocados`: esse ramo exige órfã e condições anteriores; cobertura integral limpa órfãs (`folha/causas.py:92-106`, `escala/utils.py:1275-1277`).
- Célula `trabalha=True`, mesmo com ata relavrada para folga, fica fora do tripwire de ata fantasma (`ponto/management/commands/tripwire_celulas.py:338-342`).

## 12/11 11:05 · sessao 8f90fa80b83a · - · - · origem=Ronald (tmux)

Mapeamento somente leitura; nenhum arquivo escrito.

- Curas c13 mantidas: transporte `recusas` nas seis costuras dos builders e tratamento no overlay, preservando geometria e removendo conclusão de faixa (`escala/utils.py:807`, `:1009`, `:1018`, `:1205`, `:1212`, `:1218`, `:1872`, `:2112`, `:2364`). Harness de pacotes previamente carregados (`ponto/tests/test_lastro_c12_sem_banco.py:87`).
- RED histórico executado: `--red-c12` com os três testes `LastroC13.test_grade_recusa_nomeada_preserva_orfa_e_vizinhos_sem_inventar_faixa`, `test_recusa_nao_depende_de_cobertura_aprovada_nem_de_quem_a_venceu` e `test_canal_atravessa_dispatcher_e_costuras_dos_dois_builders`: rc 1, `FAILED (errors=6)`, por ausência do argumento `recusas` na c12 (`ponto/tests/test_lastro_c13_sem_banco.py:39`).
- Nove mutantes executados, todos rc 1: `recusa_apagada`, `recusa_gate`, `recusa_faixa`, `canal_grade`, `canal_dispatcher`, `canal_turno`, `canal_legacy`, `canal_single`, `canal_overlay` (`ponto/tests/test_lastro_c13_sem_banco.py:49`).
- `--mutante harness_c12` executa o `setUp` histórico exato. Dois testes API falharam: `busca real proibida` e `grade real proibida`, rc 1 (`ponto/tests/test_lastro_c13_sem_banco.py:26`, `:89`).

Comando comum:

```sh
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.test_lastro_c13_sem_banco --mutante NOME TESTE -v [nao verificado]
```

Selos vizinhos podem exigir delegação a `espelho_do_colab`, consumo de `orfas_grade` e proibição da grade própria no PDF, preservando as exigências financeiras (`escala/tests/test_f1c_contrato.py:42`; `relatorios/tests/test_cartao_pela_celula.py:170`).

Há incompatibilidade concreta na fixture de marca espúria: cria vínculo e batidas, sem `CelulaDia`; a tela entrega `sem_celula`, e o adaptador retorna órfãs vazias sem marcos (`relatorios/tests/test_cartao_marca_espuria.py:33`; `escala/services/leitor_celula.py:394`; `escala/utils.py:1087`). Comparar apenas vazios perderia a mordida positiva. Para preservá-la, a fixture precisa entregar tela com célula/ata/lâmpadas/órfã e manter tanto a marca quanto a supressão do turno aberto e a prova textual do PDF (`relatorios/tests/test_cartao_marca_espuria.py:55`, `:70`).

## 12/11 11:04 · sessao fdafdbeb8bfa · - · - · origem=Ronald (tmux)

Recorte recente: **298 registros, 89 FAILED, 4.691,280 minutos brutos** (`suites_14d.tsv:229–526`). As últimas 20 linhas são `507–526`.

Tabela completa enviada nas mensagens anteriores com n bruto, FAILED, soma dos tempos e referências. Pontos principais:

| Prefixo | n | FAILED | Minutos brutos | Linhas |
|---|---:|---:|---:|---|
| BO-0611-05 | 9 | 2 | 132,060 | 478–479, 481–487 |
| BO-DISPUTA-SEM-PERGUNTA | 6 | 6 | 112,739 | 480, 488, 490–491, 493, 495 |
| FIO-E0-V4-E3 | 7 | 4 | 161,441 | 496–502 |
| E4B-ALCANCE | 6 | 4 | 128,479 | 492, 494, 504–505, 515, 519 |
| LASTRO-FAIXA | 5 | 5 | 98,717 | 521–525 |
| DATA-EVENTO-CRU | 3 | 0 | 35,784 | 510, 512, 518 |
| TRIAGEM-OFFLINE-SEM-CAMERA | 3 | 0 | 74,051 | 511, 517, 520 |
| O169-FECHADA | 2 | 0 | 42,975 | 475, 503 |

**Intermediárias que mudaram decisão: não medido.** Não há fechamento demonstrado do vínculo entre execução vermelha específica, cura dos respectivos casos e final da obra. BO-DISPUTA e LASTRO terminam vermelhos no anexo (`suites_14d.tsv:495,525`).

Há registros potencialmente copiados para `opus_final`, portanto estes números contam linhas, sem demonstrar execuções independentes: `jv3:281–282`; `c20_fiomudo:287,292;290,293;294–295;298–299`; `pep:302,304`; `otimiza3:305,308;306–307`; `lastro2:309–312`.

## 12/11 11:04 · sessao 0c048614a4ba · - · - · origem=Ronald (tmux)

Recalculei e enviei a tabela dos 27 grupos com FAILED: **169 registros, 51 FAILED e 2.317,742 minutos brutos**. `suites_14d.tsv:1-228`

O trecho completo tem **228 registros e 3.181,774 minutos brutos**; os outros **59 registros** são OK e somam **864,032 minutos**. `suites_14d.tsv:1-228`

“Intermediárias” e “mudaram decisão” permanecem **não medido**; nenhuma contagem usa `n−1`. [não verificado]

## 12/11 11:03 · sessao 0c048614a4ba · - · - · origem=Ronald (tmux)

No trecho `suites_14d.tsv:1-228`:

- **228 registros**, **51 FAILED**, **56 prefixos**.
- Proxy “todas menos a última observada por prefixo”: **172 candidatas**, **2.278,789 minutos**.
- Nos **27 prefixos com FAILED**: **169 registros**, **142 candidatas**, **1.883,350 minutos**.
- **Mudaram decisão: não medido**. Não fechei um vínculo entre falha identificada na suite intermediária, commit que atribua a cura àquela falha e final comprovada; os títulos truncados não sustentam essa causalidade. [não verificado] [nao verificado]

Tabelas completas, referências de linhas e grupos sem FAILED foram enviadas nas mensagens anteriores. Os minutos são soma de `Ran ... in Xs / 60`, sem o último registro de cada agrupamento; não representam tempo causal de cura. `suites_14d.tsv:1-228`

Cuidados para consolidar:

- `comboio`, `suite_impacto`, `otimiza3` e `jv1` continuam no trecho recente; agregar antes de subtrair a última observação. `suites_14d.tsv:100,227-229`
- `suite_impacto:57,226` contém duas gerações distintas; a associação a uma mesma obra não está demonstrada. [não verificado]
- A última observação é vermelha em `orfas`, `n1pna` e `suite_impacto`; não a chamar de final comprovada. `suites_14d.tsv:19,217,226`
- Exemplos antigos de cura que pareciam promissores citam reprovação do **opus**, ou vermelhos anteriores à suite, e não fecham a prova solicitada. [não verificado] [nao verificado]

## 12/11 11:03 · sessao 6fd90b5b6a65 · - · - · origem=Ronald (tmux)

Método: linhas numeradas a partir da primeira linha de dados; p50 = mediana; p95 = ordem `ceil(0,95 × n)`.

| Recorte | n | p50 s | p95 s | max s | rc124 | >900 s | Fonte |
|---|---:|---:|---:|---:|---:|---:|---|
| Últimas20 construtor |20|715,5|1630|1928|0|7|chamadas_fim_vivo.tsv:316–395, filtrando construtor |
| Últimas20 verificador |20|325,5|594|656|0|0|chamadas_fim_vivo.tsv:335–400, filtrando verificador |
| Últimas20 leitura/censo/sonda |20|204,5|891|1178|0|1|chamadas_fim_vivo.tsv:351–399, filtrando o grupo |
| Últimas20 globais |20|332|1068|1928|0|2|chamadas_fim_vivo.tsv:381–400 |
| União das três janelas |60|386,5|1206|1928|0|8|mesmas seleções |

Linhas exatas selecionadas:

- Construtor:316,333,342,350,352,353,356,359,360,365,366,368,371,374,375,378,386,391,393,395.
- Verificador:335,336,337,338,339,341,354,361,367,369,373,376,379,380,388,389,390,392,394,400.
- Grupo leitura/censo/sonda:351,355,357,358,362,363,364,370,372,377,381,382,383,384,385,387,396,397,398,399.

O anexo inteiro tem400 chamadas; seus3 `rc=124` são construtor904s, construtor905s e censo905s, todos fora das janelas acima. (chamadas_fim_vivo.tsv:66,130,317)

Últimas20 linhas `Ran`: p50=1306,5525s; p95=1638,001s; max=1788,073s;15 passaram de900s. (suites_14d.tsv:507–526)

Se considerar apenas completas, últimas20 linhas são500–507,509,511,513–520,524–525: p50=1316,237s; p95/max permanecem1638,001/1788,073s. A classificação como completa usa nomes e tamanho observado dos testes. (suites_14d.tsv:500–525) [nao verificado]

Margens mínimas empíricas:

- Suite: `1788,073 − 1638,001 =150,072s`; teto `1638,001+150,072=1788,073s`, arredondável para1789s inteiros. (suites_14d.tsv:514,524) [nao verificado]
- Wrapper/construtor, janela20: `1928−1630=298s`; teto1928s. (chamadas_fim_vivo.tsv:365,386)
- Wrapper/construtor, cobrindo todo o histórico fornecido: max comrc0=2806s; `2806−1630=1176s`; teto2806s. (chamadas_fim_vivo.tsv:214,365)

O fato de um124 ter sido “falso” e qualquer garantia para chamadas futuras são [não verificado].

## 12/11 11:01 · sessao e15566ed2acf · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · construtor · origem=Code

Entrega parcial em [resposta_c3.txt](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/docs/resposta_c3.txt:1), com correções, comandos RED/GREEN, mutantes e inventário.

[66 testes passaram; dois REDs permanecem](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/docs/resposta_c3.txt:5), dependentes da porta proibida. Ruff indisponível. Sem commit, respeitando a proibição inicial; limitações registradas no arquivo. [nao verificado]

_4195 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
