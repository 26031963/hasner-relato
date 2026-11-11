# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 11:31** · publicado em **11/11/2026 11:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 11:31 · sessao 9c6b12fbf335 · MAPA-ATE-O-20 · censo · origem=Code

**E3-LEITORES: 1 de 10; troca do vigia pendente.** A linha 635 ainda chama `porta_alcancavel_do_dia`; isso corresponde ao **item 4** do PRONTO de E3-TROCA-PELO-DIFF. (`app/chamados/services/vigia_fio.py:635`; `avais_obras.txt:55`, bloco `=== ../FIO-E0-V4-E3/aval_e3_troca_pelo_diff_1111.txt`)

Nos anexos, contei linhas preservando os brancos: `avais_obras.txt:1` começa no primeiro `===`; `relato_topo.txt:1`, no título `# RELATO`. Abaixo, `BACKLOG.md`, `TICKETS.md` e `RELATO.md` significam os arquivos em `app/docs/`. A identidade Git da cópia com o HASH informado é **[não verificado]**.

A busca de PRONTO abrangeu os avais fornecidos, `app/docs/TICKETS.md` e `app/docs/BACKLOG.md`. Resultados únicos em prosa foram aceitos explicitamente; enumerações de decisões, fases ou ordem não viraram listas de PRONTO. (`avais_obras.txt:138`; fontes específicas abaixo)

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **E3 — LEITORES** | **1 de 10** | `avais_obras.txt:55`, bloco `=== ../FIO-E0-V4-E3/aval_e3_troca_pelo_diff_1111.txt` | **2. Classes V/T/X com número:** V=15, T=0, X=217, discriminado em X1–X4. (`RELATO.md:62`, `RELATO.md:63`, `RELATO.md:64`, `RELATO.md:65`, `RELATO.md:66`) | **1. Sonda com soma 230:** medida foi **232**; divergência explicada, sem contar cumprimento literal. (`RELATO.md:74`)<br>**3. V curada ou zero:** falta prova posterior à cura cobrindo os 15 V; a decomposição anterior é A3/B2/P8/S2. (`RELATO.md:53`; conclusão integral **[não verificado]**)<br>**4. Três chamadores lendo só tarefa_do_dia:** falta a troca; vigia ainda consulta a porta antiga. (`app/chamados/services/vigia_fio.py:635`)<br>**5. Cinco respondedores mortos com certidão:** seguem declarados vivos. (`RELATO.md:99`)<br>**6. Catraca e paridade verdes:** falta prova depois da troca. **[não verificado]**<br>**7. DIFF do alcance antes/depois, por classe:** falta essa medição depois da troca. **[não verificado]**<br>**8. Suíte inteira OK:** há prova da fase `999a7966c`, sem a troca concluída. (`relato_topo.txt:28`; `app/chamados/services/vigia_fio.py:635`)<br>**9. Opus final aprova:** aprovação da mesma fase, sem prova da obra LEITORES completa. (`relato_topo.txt:27`)<br>**10. Pouso:** pousou a fase; falta pouso com os resultados da troca comprovados. (`relato_topo.txt:7`; `avais_obras.txt:138`) |
| **E3 — sublista X1, separada** | **6 de 7 — não somados aos 10** | `avais_obras.txt:37`, bloco `=== ../FIO-E0-V4-E3/aval_e3_x1_folga_1111.txt` | **1. Censo dos 15:** sonda identificada, n=15, rc=0. (`RELATO.md:41`)<br>**2. Cura no ramo:** alteração do cartório descrita na aprovação. (`relato_topo.txt:27`)<br>**3. RED/GREEN + mutante:** base morta, GREEN 14 OK, mutantes X1 7/7. (`relato_topo.txt:31`)<br>**5. Suíte inteira OK:** Ran 15134, rc=0. (`relato_topo.txt:28`)<br>**6. Opus final:** aprovado. (`relato_topo.txt:27`)<br>**7. Pouso:** merge e deploy rc=0 registrados. (`relato_topo.txt:7`) | **4. DIFF da sombra por colaborador e zero trabalho mudando:** falta a discriminação completa. O topo apresenta agregados; o aval dos 35 dias posteriores ao desligamento resolve o corte, mas não apresenta o DIFF completo exigido. (`relato_topo.txt:18`, `relato_topo.txt:21`; `avais_obras.txt:44`) |

Há conflito documental na E3: ORDEM-ATE-O-ZERO referencia **“6 resultados”** de E3-TROCA-PELO-DIFF, mas o próprio aval enumera **10**. Não há seis itens transcritos naquela referência para construir outra contagem. A sublista X1 enumera sete resultados próprios. (`avais_obras.txt:28`, bloco `=== ../FIO-E0-V4-E3/aval_ordem_ate_o_zero_1111.txt`; `avais_obras.txt:55`; `avais_obras.txt:37`)

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **E4B** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1866`, `BACKLOG.md:1875`; `avais_obras.txt:92`, bloco `=== ../E4C/aval_expand_contract.txt`; busca também em `TICKETS.md` | Há GREEN e mutantes da c4, mas a suíte seguinte ficou vermelha; não fornecem N da obra. (`relato_topo.txt:9`, `relato_topo.txt:4`) | Formalizar a lista: o BACKLOG reúne A24, C3, três rodadas e `alarma_em_0` sem numerá-los como PRONTO. O aval ainda exige classe e, limbo, ramo sem porta e backfill da 11 com DIFF; não converti esse conjunto em N. (`BACKLOG.md:1866`; `avais_obras.txt:28`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **E4C** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1866`; `avais_obras.txt:92`, bloco `=== ../E4C/aval_expand_contract.txt`; busca também em `TICKETS.md` | Censo identificado, com amostra 10/10 conferida; prova de preparação, sem contar migração concluída. (`relato_topo.txt:36`) | A prosa reúne **migração dos DIVERGE para o ato** e **selo 2**, sem numeração de PRONTO. Falta lista e prova desses resultados; D04/D05 seguem registradas. O PRONTO do censo conjunto é outra entrega, também não numerada. (`BACKLOG.md:1866`; `relato_topo.txt:36`; `avais_obras.txt:87`, bloco `=== ../E4C/aval_censo_adiantado_1111.txt`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **E4D** | **0 de 1 — resultado único em prosa** | `BACKLOG.md:1866`; `avais_obras.txt:92`, bloco `=== ../E4C/aval_expand_contract.txt` | Nenhum resultado de CONTRACT comprovado. **[não verificado]** | **1. Parar de escrever a projeção:** falta registro no RELATO de commit/sonda que prove a interrupção da escrita. Retirada física e migration estão separadamente registradas, fora deste resultado. (`BACKLOG.md:1866`, `BACKLOG.md:1867`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F1** | **PRONTO AUSENTE** | `BACKLOG.md:1883`; `avais_obras.txt:98`, bloco `=== ../RAIA-2-BO-VISIVEL/aval_raiz_f1_um_eleitor_1111.txt`; busca também em `TICKETS.md` | Censo `f0f1.r`, rc=0, identifica três sítios persistentes e três eleitores divergentes. (`RELATO.md:19`) | Os números (1)–(4) do aval organizam decisões/instruções, sem lista de PRONTO. Falta essa lista e prova da implementação do escritor/eleitor único; o censo não a demonstra. (`avais_obras.txt:98`; `RELATO.md:19`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F2** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1883`, `BACKLOG.md:1877`; busca nos avais e em `TICKETS.md` | Há registro de c21 estacionada, c22 não lançada e suíte17 vermelha. (`RELATO.md:27`) | F2=BO-DISPUTA reúne cura, invariante, selo e contadores em prosa. Falta lista numerada e prova de cumprimento de cada resultado; não transformei as fases em itens de PRONTO. (`BACKLOG.md:1883`, `BACKLOG.md:1877`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F3** | **PRONTO AUSENTE** | `BACKLOG.md:1883`; `avais_obras.txt:87`, bloco `=== ../E4C/aval_censo_adiantado_1111.txt`; `avais_obras.txt:104`, bloco `=== ../RAIA-2-BO-VISIVEL/aval_f4_par_pela_celula_1111.txt`; busca também em `TICKETS.md` | Censo e sonda conjuntos estão registrados. (`relato_topo.txt:37`, `relato_topo.txt:38`) | Falta PRONTO específico da fase. O aval define autor do fecho automático; o topo registra dependência por arquivo comum, sem prova de cura pousada. (`avais_obras.txt:104`; `relato_topo.txt:11`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F4** | **PRONTO AUSENTE** | `BACKLOG.md:1883`; `avais_obras.txt:102`, bloco `=== ../RAIA-2-BO-VISIVEL/aval_f4_par_pela_celula_1111.txt`; busca também em `TICKETS.md` | A sonda identifica perda do par e a fonte `PARES_PAUSA_DNA`; isso prova o diagnóstico. (`relato_topo.txt:38`) | Falta lista de PRONTO e prova da leitura do par pela célula implementada. O aval define autoridade e proibições, sem enumerar resultados de conclusão. (`avais_obras.txt:102`, `avais_obras.txt:106`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F5** | **0 de 1 — herdado de E4D** | Equivalência E4d=F5: `BACKLOG.md:1756`; resultado: `BACKLOG.md:1866`, `avais_obras.txt:92`, bloco `=== ../E4C/aval_expand_contract.txt` | Nenhum resultado de CONTRACT comprovado. **[não verificado]** | **1. Parar de escrever a projeção:** falta prova escrita no RELATO. É o mesmo resultado da E4D, sem lista própria adicional nem soma entre obras. (`BACKLOG.md:1756`, `BACKLOG.md:1866`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F6** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1883`; `avais_obras.txt:10`, bloco `=== ../META-18/aval_meta18_duas_raias_1111.txt`; busca também em `TICKETS.md` | Conclusão dos resultados: **[não verificado]** | A prosa exige DRY, DIFF por classe, reversão, lista de exportadas e medição de idade/efeito das duplicadas. São vários resultados sem numeração; faltam lista e provas correspondentes. (`avais_obras.txt:10`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F7** | **PRONTO AUSENTE** | `BACKLOG.md:1883`; `avais_obras.txt:5`, bloco `=== ../META-18/aval_meta18_duas_raias_1111.txt`; busca também em `TICKETS.md` | Selo F7 verde: **[não verificado]** | “Selo esperado 0” nasce com cada fase, mas essa referência não identifica uma lista de resultados nem a condição concreta medida pelo selo. Falta definição de PRONTO e prova identificável. (`avais_obras.txt:5`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **O168** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1395`; busca nos avais e em `TICKETS.md` | Censo/sonda registrados; `CSF_RETRATADO_30d` consta **não mensurável, null**, sem prova de zero. (`RELATO.md:903`) | A prosa reúne guarda compartilhada, aplicação no evento, retirada posterior do `--executar` e `CSF_RETRATADO=0`. Falta lista numerada e prova desses resultados; “nada construído” está registrado. (`BACKLOG.md:1395`; `RELATO.md:173`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **LASTRO** | **PRONTO AUSENTE para a obra inteira** | `BACKLOG.md:1731`, `BACKLOG.md:1760`, `BACKLOG.md:1762`, `BACKLOG.md:1807`; busca nos avais e em `TICKETS.md` | Parte 2: deploy rc=0 e `sem_evento=0`. (`RELATO.md:863`)<br>Parte 3: pouso registrado. (`RELATO.md:799`) | Falta lista de PRONTO da LASTRO inteira. Partes 2/3 e FOLGA-FORA-DO-CATALOGO têm registros separados; não somei suas entregas para fabricar N. (`BACKLOG.md:1760`, `BACKLOG.md:1762`, `BACKLOG.md:1807`) | [nao verificado]

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **O169-FECHADA** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1921`; `avais_obras.txt:132`, bloco `=== ../O169-FECHADA/aval_tres_respostas_1110_0040.txt`; busca também em `TICKETS.md` | Suíte v5 verde; GREEN/mutantes registrados; merge/deploy, APPLY 3 e DRY seguinte vazio. (`relato_topo.txt:8`, `relato_topo.txt:12`, `relato_topo.txt:6`) | Falta lista numerada que permita N. O `(3)` do aval é a terceira decisão de um aval misto; não é uma lista de três resultados da O169. O nome vigente do contador é `cluster_encerrado_sem_lastro`. (`avais_obras.txt:132`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **UI-PAINEL** | **PRONTO AUSENTE — quatro resultados declarados, não numerados** | `avais_obras.txt:124`, bloco `=== ../UI-PAINEL/aval_ui_painel_raspado.txt`; busca também em `TICKETS.md`/`BACKLOG.md` | O GREEN falhou em quatro testes; mutantes não medidos porque caíram nas mesmas falhas. (`relato_topo.txt:5`) | O PRONTO lista, sem números: **faixa na aba Mensagens; contador do botão; função única com três leitores; selo API×painel verde com mutante morto**. Falta numeração e prova de cada resultado. A enumeração MUDA cobre somente 1–3, sem incluir o selo; não a completei por inferência. (`avais_obras.txt:117`, `avais_obras.txt:118`, `avais_obras.txt:119`, `avais_obras.txt:124`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **BO-ROTULO-ENCERRADO** | **0 de 1 — resultado único em prosa** | `BACKLOG.md:1884` | Há mapeamento da fonte do modal e dos leitores; não prova da troca concluída. (`RELATO.md:7699`) | **1. Rótulo lido da mesma fonte do modal, sem mudar estado:** falta prova escrita da alteração e do comportamento do seletor para resolvido/fechado. (`BACKLOG.md:1884`; conclusão **[não verificado]**) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **TRIAGEM-OFFLINE-SEM-CAMERA** | **PRONTO AUSENTE — não numerada** | `avais_obras.txt:95`, bloco `=== ../RAIA-2-BO-VISIVEL/aval_raia2_bo_visivel_1111.txt`; contexto em `BACKLOG.md:1889`; busca também em `TICKETS.md`/`BACKLOG.md` | Consta na fila depois de BO-ROTULO. (`relato_topo.txt:17`) | Falta lista de PRONTO. O aval exige juiz único de selfie impeditiva e smoke nas duas cascas; conclusão desses resultados e preservação da batida offline sem câmera: **[não verificado]**. (`avais_obras.txt:95`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **LOTE-VALIDAR-REGRESSAO-0311** | **PRONTO AUSENTE** | `BACKLOG.md:1892`; `avais_obras.txt:95`, bloco `=== ../RAIA-2-BO-VISIVEL/aval_raia2_bo_visivel_1111.txt`; busca também em `TICKETS.md` | Medição citada: custo por referência estável, crescimento pelo tamanho do lote e reoferta de recusados; nenhuma cura pontual, retorno a LOTE-VALIDAR-VIRA-JOB. Prova identificada: `sonda_lote0311.r`. (`relato_topo.txt:10`) | Falta lista de resultados de conclusão. “Medir a causa antes” é precondição; a medição realizada não autoriza inventar N nem declarar a obra encerrada. (`BACKLOG.md:1892`; `avais_obras.txt:95`, `avais_obras.txt:138`) |

**Obras PRONTO AUSENTE:**

- **E4B e E4C** — prosa com múltiplos resultados, sem lista numerada. (`BACKLOG.md:1866`)
- **RAIZ F1, F2, F3, F4, F6 e F7** — sem lista específica de PRONTO nas fontes examinadas. (`BACKLOG.md:1883`; `avais_obras.txt:98`, `avais_obras.txt:87`, `avais_obras.txt:102`, `avais_obras.txt:10`, `avais_obras.txt:5`)
- **O168**. (`BACKLOG.md:1395`)
- **LASTRO**. (`BACKLOG.md:1760`, `BACKLOG.md:1762`, `BACKLOG.md:1807`)
- **O169-FECHADA**. (`BACKLOG.md:1921`; `avais_obras.txt:132`)
- **UI-PAINEL** — quatro resultados declarados, não numerados. (`avais_obras.txt:124`)
- **TRIAGEM-OFFLINE-SEM-CAMERA**. (`avais_obras.txt:95`)
- **LOTE-VALIDAR-REGRESSAO-0311**. (`BACKLOG.md:1892`; `avais_obras.txt:95`)

## 11/11 11:28 · sessao b11c41e475b0 · - · - · origem=Ronald (tmux)

Censo concluído só por leitura dos três documentos do HEAD e anexos do prompt. As linhas do BACKLOG do anexo estão deslocadas em relação à cópia; usar abaixo as linhas reais do HEAD.

| Obra | Lista de PRONTO encontrada | Fonte HEAD | Prova escrita disponível / falta |
|---|---|---|---|
| O168 | Não há lista numerada. A prosa reúne vários resultados: guarda compartilhada, valer no evento, retirada posterior do `--executar`, `CSF_RETRATADO=0`; portanto não converter em N=1. | BACKLOG.md:1395; BACKLOG.md:1960 | RELATO.md:903 registra censo/sonda: guarda só no supra; `CSF_RETRATADO_30d` não mensurável, `null`, não zero. RELATO.md:173 diz nada construído. Falta lista formal de PRONTO e provas dos resultados. |
| LASTRO | Não há lista única numerada de PRONTO para a obra inteira. Há partes e complementos separados; não somar. | BACKLOG.md:1731; BACKLOG.md:1760; BACKLOG.md:1762; BACKLOG.md:1807 | Fase 2: construção, RED/GREEN, mutantes, sombra e suíte em RELATO.md:860; opus em :861; pouso em :862; deploy e `sem_evento=0` em :863. Parte 3 pousou, opus e suíte verdes em RELATO.md:799. Essas provas não dão N da LASTRO inteira. Folga fora do catálogo ainda foi encaminhada a censo em RELATO.md:7353. | [nao verificado]
| O169-FECHADA | Não há lista numerada de PRONTO. BACKLOG tem contador + mutante + reparo do legado; múltiplos resultados sem N. O `(3)` do aval TRES RESPOSTAS é a terceira decisão de um aval misto, não três itens PRONTO da obra. | BACKLOG.md:1921 | Anexo `relato_topo.txt`, linha “O169-FECHADA NO AR 10:57”: merge `ca978f65d`, deploy rc0, DRY 3, APPLY 3, DRY seguinte vazio. Suíte v5 e mutantes também no topo. Falta N autorizado; não escrever X de N com base no pouso. Nome antigo `fechados_por_idade` do BACKLOG foi substituído por `cluster_encerrado_sem_lastro` no aval anexado; mudança explicada em RELATO.md:194. |
| UI-PAINEL | Aval traz **“PRONTO (4 resultados)”**, mas os quatro estão separados por `;`, **sem numeração**: faixa; contador; função única com três leitores; selo API/painel verde com mutante morto. Pela exigência literal do usuário, informar **“não numerada, quatro resultados declarados”**, sem inventar N a partir dela. `MUDA` numera somente 1–3 e não cobre o selo. | Não há ocorrência específica em TICKETS/BACKLOG HEAD; fonte é bloco `=== ../UI-PAINEL/aval_ui_painel_raspado.txt` do anexo. | `relato_topo.txt`, linha “UI-PAINEL v1 ORM 10:55”: quatro falhas no GREEN; mutantes não medidos porque caem nas mesmas falhas. Não prova nenhum dos quatro resultados finalizados. Construção iniciada não basta. |
| BO-ROTULO-ENCERRADO | Nenhuma lista numerada. **Candidato à exceção N=1**: “cura = rótulo lido da mesma fonte do modal, sem mexer em estado” é um único resultado observável, embora não venha rotulado PRONTO. Se aceitar prosa de escopo como lista singular, usar **0 de 1**, sem somar outras exigências. | BACKLOG.md:1884 | RELATO.md:7699 só registra mapa, fonte modal e 13 leitores/escritores; não comprova troca. Topo põe a obra depois do pouso O169. Falta prova de que seletor lê a mesma fonte do modal e distingue resolvido/fechado. | [nao verificado]
| TRIAGEM-OFFLINE-SEM-CAMERA | Sem lista numerada. Aval exige juiz único online/offline/v2 e smoke nas duas cascas: vários resultados, não N=1. | Entrada específica só no anexo; BACKLOG.md:1889 contém “selfie impeditiva” dentro do resto da classe, sem PRONTO. | Topo a mantém na fila depois de BO-ROTULO. Não há prova escrita de cura/smoke. Falta lista e prova de admissibilidade única, batida preservada offline sem câmera e smoke das duas cascas. |
| LOTE-VALIDAR-REGRESSAO-0311 | Sem lista numerada de PRONTO. BACKLOG “medir a causa antes” é precondição; aval RAIA2 determina conduta condicional, não enumera resultados de conclusão. | BACKLOG.md:1892 | Medição comprovada no topo, linha “LOTE-VALIDAR-REGRESSAO-0311 MEDIDA 10:3x”, com `logs/RAIA-2-BO-VISIVEL/sonda_lote0311.r`: custo por referência estável; aumento pelo lote e reoferta de recusados; nenhuma cura pontual; volta à LOTE-VALIDAR-VIRA-JOB. RELATO.md:7695 era o estado anterior: causa por medir. Não transformar essa medição em X de N sem lista. |

Busquei todas as ocorrências dos nomes e dos termos `PRONTO`/`resultados` em `app/docs/TICKETS.md` (1323 linhas), `app/docs/BACKLOG.md` (2339 linhas) e `app/docs/RELATO.md` (7726 linhas); não localizei outra lista numerada para essas sete obras.

Recomendação estrita para a consolidação: PRONTO AUSENTE para O168, LASTRO, O169-FECHADA, UI-PAINEL, TRIAGEM e LOTE; BO-ROTULO pode ser **0 de 1** apenas pela exceção expressa de resultado único, com fonte BACKLOG.md:1884.

## 11/11 11:28 · sessao c7cd088f0fd1 · - · - · origem=Ronald (tmux)

E3-LEITORES: **1 de 10**, em leitura estrita da lista E3-TROCA-PELO-DIFF. Só o item 2 tem prova completa inequívoca: V=15, T=0, X=217, discriminados em `app/docs/RELATO.md:62–66`.

- Item 1: aval exige soma 230; medida foi 232. Divergência explicada em `app/docs/RELATO.md:74`, mas não conto cumprimento literal.
- Item 3: falta medida pós-cura demonstrando solução dos 15 V. A decomposição A3/B2/P8/S2 está em `app/docs/RELATO.md:53`; a entrada de A/S no c8b está em `app/docs/RELATO.md:35` e `:38`; entrega c8c genérica em `:9` não demonstra o resultado dos 15.
- Item 4: falta trocar os três chamadores. Confirmado no código: `app/chamados/services/vigia_fio.py:635` ainda chama `porta_alcancavel_do_dia`.
- Item 5: falta certidão dos cinco respondedores. Sua sobrevivência está declarada em `app/docs/RELATO.md:238`.
- Item 6: falta prova da catraca “respondedores=1” e paridade após troca.
- Item 7: falta DIFF de alcance antes/depois da troca por classe.
- Itens 8–10: suite, opus e pouso do topo vivo demonstram a fase c8+X1, não o fechamento da obra LEITORES com a troca concluída. [nao verificado]

E3-X1 tem **lista separada de 7 itens**, sem somar ao N=10. Recomendo **6 de 7**:

1. Censo dos 15: `app/docs/RELATO.md:41`.
2. Cura no ramo: relato_topo, linha do opus final 999a7966c.
3. RED/GREEN e mutantes: relato_topo, linha e3fix11 ORM 09:21.
4. **Falta** DIFF completo por colaborador, demonstrando zero dia de trabalho exigível mudando. O topo dá agregados e os 35 pós-desligamento; o aval libera esses 35, mas não substitui a discriminação exigida.
5. Suite inteira: relato_topo, linha suite pre-opus 09:46. [nao verificado]
6. Opus final: relato_topo, linha opus final 09:54.
7. Pouso: relato_topo, linha FIO-E0-V4-E3 NO AR 11:0x. [nao verificado]

E4B: **PRONTO AUSENTE**. `app/docs/BACKLOG.md:1866` traz prosa composta — A24, C3, três rodadas, alarma — sem lista numerada. `:1875` descreve backfill, sem fornecer lista de PRONTO. Não há lista pertinente em TICKETS.

E4C: **PRONTO AUSENTE**. `app/docs/BACKLOG.md:1866` traz dois resultados em prosa: migração dos DIVERGE e selo 2. O aval do censo adiantado é entrega conjunta de censo, não PRONTO da migração.

E4D: **0 de 1**, caso se aplique a exceção de resultado único em prosa: parar de escrever a projeção, `app/docs/BACKLOG.md:1866` e aval E4-EXPAND-CONTRACT. Não há prova escrita de conclusão do CONTRACT no RELATO fornecido.

A referência HEAD correta de E3-LEITORES é `app/docs/BACKLOG.md:1865`; as linhas do anexo backlog_obras não correspondem à numeração da cópia.

## 11/11 11:28 · sessao 0cccfd47af33 · - · - · origem=Ronald (tmux)

Censo RAIZ concluído, sem escrita. As linhas abaixo são do HEAD da cópia; diferem do anexo `backlog_obras.txt`. (`app/docs/BACKLOG.md:1883`)

| Fase | PRONTO encontrado | Prova / limite |
|---|---|---|
| RAIZ F1 | **PRONTO AUSENTE** específico. A sequência chama F1 de “matriz”, sem lista de resultados. O aval UM-ELEITOR enumera decisões de implementação, não PRONTO. (`app/docs/BACKLOG.md:1883`; anexo `aval_raiz_f1_um_eleitor_1111.txt`) | Existe censo `f0f1.r`, rc 0, com 3 sítios persistentes e 3 eleitores divergentes; isso prova o censo, não a implementação do escritor/eleitor único. (`app/docs/RELATO.md:19`) |
| RAIZ F2 | **PRONTO AUSENTE** específico. F2 é BO-DISPUTA; sua linha mistura cura, invariante, selo e contadores em prosa, sem lista PRONTO numerada. (`app/docs/BACKLOG.md:1883`; `app/docs/BACKLOG.md:1877`) | c21 estacionada, c22 não lançada; suite17 terminou vermelha. (`app/docs/RELATO.md:27`) |
| RAIZ F3 | **PRONTO AUSENTE** de produto. Há o censo conjunto F3/F4 no anexo; seu PRONTO é entrega de leitura, não encerramento da fase. (`app/docs/BACKLOG.md:1883`; anexo `aval_censo_adiantado_1111.txt`) | O topo fornecido relata censo, sonda e bloqueio por arquivo comum; não relata cura pousada. (anexo `relato_topo.txt`, linhas a numerar pelo pai) |
| RAIZ F4 | **PRONTO AUSENTE** de produto. O aval PAR-PELA-CELULA define autoridade e proibições; não enumera PRONTO. A linha BUG-PAR descreve ato do par, RED e DRY em prosa. (`app/docs/BACKLOG.md:1929`; anexo `aval_f4_par_pela_celula_1111.txt`) | Censo/sonda do topo provam descoberta da fonte `PARES_PAUSA_DNA`, não implementação. (anexo `relato_topo.txt`, linhas a numerar) |
| RAIZ F5 | **Sem lista própria.** Pode herdar **N=1** de E4d, se aplicado o critério de resultado único em prosa: “parar de escrever a projeção”. A equivalência E4d = F5 está explícita. (`app/docs/BACKLOG.md:1756`; `app/docs/BACKLOG.md:1866`) | **X=0**: nenhuma prova escrita de contract concluído encontrada no RELATO; o topo mantém E4c dependente da E4b. (anexo `relato_topo.txt`, linhas a numerar) |
| RAIZ F6 | **PRONTO AUSENTE** numerado. O aval META descreve vários resultados sem numeração: DRY, DIFF por classe, reversão, lista de exportadas e medição das duplicadas; não cabe transformar tudo em um item. (`app/docs/BACKLOG.md:1883`; anexo `aval_meta18_duas_raias_1111.txt`) | Não há prova de conclusão desses resultados no RELATO pesquisado. [não verificado] |
| RAIZ F7 | **PRONTO AUSENTE** específico. “Selo esperado 0 nasce com cada fase” não fornece lista nem define, nessa frase, a condição medida. (`app/docs/BACKLOG.md:1883`; anexo `aval_meta18_duas_raias_1111.txt`) | Não há prova citada de selo RAIZ F7 verde no material fornecido. [não verificado] |

Fontes locais pesquisadas integralmente: `app/docs/TICKETS.md` (1323 linhas), `app/docs/BACKLOG.md` (2339 linhas), `app/docs/RELATO.md` (7726 linhas). `TICKETS.md` não tem entradas RAIZ-DA-DISPUTA, BO-DISPUTA-SEM-PERGUNTA, BUG-PAR ou DISPUTA-ZUMBI. [não verificado — resultado negativo da busca]

Atenção para não converter a ordem F0→F7 em sete itens de PRONTO: ela descreve fases da obra. (`app/docs/BACKLOG.md:1883`)

## 11/11 10:57 · sessao 5a702a9c8f16 · MAPA-20 · verificador · origem=Code

REPROVADO para pouso [nao verificado]

Referências `diff:arquivo:linha` usam as linhas de destino do patch fornecido; `base:arquivo:linha`, a cópia disponível. A identidade Git dessa cópia com o HASH informado ficou [nao verificado].

1. **Triagem 1–5: três correções fecham o achado; duas permanecem parciais.**

   | Achado | Conferência |
   |---|---|
   | **1 — nome da obra** | **Fecha.** `UI-PAINEL` foi corrigido e as 18 obras correspondem nominalmente ao aval. (`diff:bin/mapa20_valida.py:19–24`; `aval_mapa_ate_o_20_1111.txt:1`, item 1.) |
   | **2 — tabela torta** | **Parcial.** Há checagem de largura das linhas, duplicatas e células vazias, mas continuam passando separador com largura errada e tabela ainda dentro de cerca; exemplos no item 3. (`diff:bin/mapa20_valida.py:47–52,64–67,82–94`.) |
   | **3 — retido não atômico/saída antecipada** | **Fecha os dois defeitos indicados.** A re-raspagem escreve ao lado, troca por `mv`, e a falha segue para os passos seguintes com `MAPA_RC=1`. O `rm` é tentado, mas seu sucesso não é conferido. (`diff:bin/relato.sh:99–112`.) |
   | **4 — falta de foto** | **Fecha no código.** O validador lê `$TMP/mapa20.md`, e esse mesmo arquivo é a origem entregue ao raspador; regravar a fonte depois não troca o documento deste ciclo. (`diff:bin/relato.sh:96–97,141`.) |
   | **5 — selo por texto/mutantes** | **Parcial.** Agora há execução de cenários, mas sobrevivem mutantes plausíveis; exemplos no item 4. A alegação de “12 mortos” não veio acompanhada dos mutantes/resultados: [nao verificado]. (`diff:bin/tests/test_mapa20_valida.sh:54–109`.) |

2. **Todos os `exit` de `relato.sh` após o diff: 12 comandos.**

   | Local | Valor e condição |
   |---|---|
   | `base:bin/relato.sh:27` | `1`: falha da trava. |
   | `base:bin/relato.sh:28` | `1`: destino sem `.git`. |
   | `base:bin/relato.sh:32` | `1`: falha da lista de nomes. |
   | `base:bin/relato.sh:35` | `1`: falha do BACKLOG-TOPO. |
   | `base:bin/relato.sh:38` | `1`: falha do ESTADO. |
   | `base:bin/relato.sh:42` | `1`: falha da SESSAO. |
   | `base:bin/relato.sh:86` | `1`: falha da raspagem do RELATO retido. |
   | `diff:bin/relato.sh:126` | `1`: falha de `gerar_avais.py`. |
   | `diff:bin/relato.sh:141` | `1`: falha da raspagem geral. |
   | `diff:bin/relato.sh:155` | `$MAPA_RC`, portanto `0` ou `1`: índice sem mudança. |
   | `diff:bin/relato.sh:161` | `1`: falha da expressão `git push && echo`. | [nao verificado]
   | `diff:bin/relato.sh:162` | `$MAPA_RC`, portanto `0` ou `1`: saída final. |

   **Validador executado vermelho → rc final 0:** não há esse caminho no fluxo mostrado. O ramo fixa `MAPA_RC=1`, nenhuma atribuição posterior o zera, e ambas as saídas normais o devolvem. **Validador verde → rc 1 sem outra falha:** também não há; as saídas posteriores com `1` dependem de outra falha. (`diff:bin/relato.sh:94–99,126,141,155,161–162`.) [nao verificado]

   **O público pode perder MAPA-20 sem o validador falhar:** sim, **sem ele executar**. Fonte ausente/ilegível → `cp` falha → o `&&` pula Python → re-raspagem do público falha → `rm` → remoção rastreada entra no commit. O ciclo sai `1`; isso não constitui validador verde. (`diff:bin/relato.sh:96–108,149–162`.) [nao verificado]

   Continua possível falha de `git commit` seguida de push bem-sucedido terminar `0` com mapa verde; existe outra falha nesse caso. A ausência de conferência de `cd`/commit já era anterior, conforme o descarte 6, e não foi curada pelo diff. (`base:bin/relato.sh:21,120–132`; `diff:bin/relato.sh:144–162`; `triagem_v1.txt:7`.) [nao verificado]

3. **Validador: universo correto, parser ainda insuficiente.**

   Nome a nome, a tupla contém: **E3, E4B, E4C, E4D; RAIZ F1, F2, F3, F4, F5, F6, F7; O168; LASTRO; O169-FECHADA; UI-PAINEL; BO-ROTULO-ENCERRADO; TRIAGEM-OFFLINE-SEM-CAMERA; LOTE-VALIDAR-REGRESSAO-0311**. São as 18 do aval; `SEM DONO` é exigido separadamente. (`diff:bin/mapa20_valida.py:19–25,92–94`; `aval_mapa_ate_o_20_1111.txt:1`, itens 1–2.)

   Nos exemplos seguintes, **G** significa uma tabela completa com cabeçalho de três colunas, separador de três colunas e as 19 linhas obrigatórias preenchidas. Essa forma passa no próprio selo. (`diff:bin/tests/test_mapa20_valida.sh:20–29`.)

   **Aceita indevidamente:**

   - **Separador estreito:** em G, substituir somente o separador por `|---|---|` continua verde. O código confere o conteúdo das células do separador, mas não sua quantidade contra o cabeçalho. (`diff:bin/mapa20_valida.py:64–67`.) [nao verificado]
   - **Tabela dentro de cerca:** quatro crases de abertura, depois três crases, depois G e finalmente quatro crases. Continua verde, embora as três crases intermediárias não encerrem a cerca de quatro. O mesmo ocorre com quatro/três tils. O parser guarda só o caractere da cerca. (`diff:bin/mapa20_valida.py:47–52`.) [nao verificado]
   - **Fechamento com texto:** uma linha de três crases seguida de `python` também é tratada como fechamento. (`diff:bin/mapa20_valida.py:47–52`.)
   - **Cabeçalho vazio:** `| | | |`, separador normal e as 19 linhas preenchidas passam; o cabeçalho só tem sua quantidade de colunas examinada. (`diff:bin/mapa20_valida.py:79–89`.)
   - **Pontuação sem dado:** `…`, `!!!` e `___` passam. O filtro reconhece apenas o conjunto restrito `-?.–— `, além da retirada de asteriscos/crases; `...` reprova, mas `…` passa. (`diff:bin/mapa20_valida.py:27,70–71`.)

   **Recusa indevidamente formas de apresentação equivalentes:**

   - G sem os pipes externos: retorna `mapa sem tabela`, porque só coleta linhas iniciadas por `|`. (`diff:bin/mapa20_valida.py:58–63`.)
   - Obra escrita `_E3_`: acusa E3 ausente, enquanto `**E3**` é normalizado. A limpeza trata asteriscos/crases, mas não underscores. (`diff:bin/mapa20_valida.py:30–31,92–94`.)

   **Escape:** `a\|b` funciona como uma célula. Entretanto, a substituição é cega: na linha `| E3 | caminho\\| nao medido |`, com **duas barras reais**, funde as duas últimas células e acusa largura 2 contra 3; não examina a paridade das barras. (`diff:bin/mapa20_valida.py:34–39,87–88`.)

   **Limite de conformidade:** uma tabela de apenas `obra | dado`, com todas as linhas dizendo `nao medido`, também passa. Isso satisfaz o mínimo implementado, mas não comprova as colunas específicas, a medição ou o tamanho de SEM DONO exigidos pelo aval. (`diff:bin/mapa20_valida.py:79–94`; `aval_mapa_ate_o_20_1111.txt:1`, itens 1–2.)

4. **O selo não fica verde com recorte literalmente vazio, mas ainda deixa regressões passarem.** [nao verificado]

   Os três `awk` dependem de prefixos textuais dos marcadores. Se um recorte ficar vazio, as guardas por `mapa20_valida.py`, `raspar_publico.py` e `git push` incrementam `FALHAS`; portanto, a hipótese específica “bloco vazio e selo verde” está barrada. As guardas, porém, não comprovam os limites exatos dos recortes. (`diff:bin/tests/test_mapa20_valida.sh:54–58,112`.) [nao verificado]

   **Mutante plausível que passa:** retirar `|~{3,}` da regex de cercas do validador. A parte (a) só testa cerca de crases; os seis cenários da parte (b) também não usam tils. O mutante preserva esses resultados e aceita G inteiramente cercado por `~~~`. (`diff:bin/mapa20_valida.py:47`; `diff:bin/tests/test_mapa20_valida.sh:29–49,83–109`.)

   **Outro mutante sobrevivente:** validar e publicar diretamente `logs/MAPA-20/mapa.md`, deixando a foto sem uso. Os seis cenários não alteram a fonte entre validação e raspagem; assim, não distinguem esse mutante da correção do achado 4. (`diff:bin/relato.sh:96–97`; `diff:bin/tests/test_mapa20_valida.sh:83–109`.)

   A reprodução em memória dos 48 textos da parte (a) não encontrou divergências; o caso ausente é tratado com rc 1. Execução integral do selo, incluindo seus repositórios temporários: [nao verificado]. (`diff:bin/tests/test_mapa20_valida.sh:29–49`; `diff:bin/mapa20_valida.py:99–104`.)

5. **Selos vizinhos: nenhuma quebra demonstrada nas verificações que leem `relato.sh`.**

   - **`test_codex_estado`:** o recorte começa em `CODEX_OK=""`, depois do novo 2f, e termina no passo 3. Os novos comandos não entram na região examinada por `exit`. (`base:bin/tests/test_codex_estado.sh:104–111`; `diff:bin/relato.sh:89–122`.)
   - **`test_relato_publica_claude_md`:** mantém o par CLAUDE e o pathspec. O novo `git add` de MAPA está indentado e não aumenta a contagem de linhas iniciadas na coluna 1 por `git add -A `. (`base:bin/tests/test_relato_publica_claude_md.sh:13–20`; `diff:bin/relato.sh:141,145,151,160`.)
   - **`test_codex_estado_evento`:** continuam as chamadas de `codex_estado.sh` e `relato_nomes.sh`, sem introduzir `gerar_codex_estado.py` inline. (`base:bin/tests/test_codex_estado_evento.sh:27–30`; `base:bin/relato.sh:32,97`.)
   - **`test_relato_afirma_com_prova`:** a chamada exigida permanece. (`base:bin/tests/test_relato_afirma_com_prova.sh:43–45`; `base:bin/relato.sh:55`.)
   - **`test_publicacao_sem_nome`:** o universo vem de `git ls-files` do público; MAPA rastreado entra automaticamente. Não depende de um recorte do script. (`base:bin/tests/test_publicacao_sem_nome.sh:58–72`.)

   Execução completa desses cinco selos e resultado de privacidade em produção: [nao verificado].

6. **Quem escreve e quem lê.**

   | Campo/arquivo | Escritor | Leitor |
   |---|---|---|
   | `MAPA_RC` | Shell: inicializa `0`; falha de cópia **ou** validação fixa `1`. | Saídas sem mudança e final. (`diff:bin/relato.sh:94–99,155,162`.) |
   | `MAPA_PAR` | Shell: vazio ou par foto→público. | Argumento opcional do raspador geral. (`diff:bin/relato.sh:94,97,141`.) |
   | `MAPA_PATH` | Shell: vazio ou nome após `git add` bem-sucedido. | Mensagem e pathspec do commit. (`diff:bin/relato.sh:149–160`.) | [nao verificado]
   | `$TMP/mapa20.md` | `cp` da fonte; limpeza pelo trap. | Validador e raspador geral. (`diff:bin/relato.sh:96–97,141`; `base:bin/relato.sh:25`.) |
   | `logs/MAPA-20/mapa.md` | “Sessão por evento” está declarado no comentário; escritor efetivo e gatilhos: [nao verificado]. | `cp`. (`diff:bin/relato.sh:89–96`.) |
   | Público `MAPA-20.md` | Raspador geral; no retido, raspador→arquivo ao lado→`mv`; falha tenta `rm`. | Re-raspagem do retido e Git para inclusão/remoção/publicação. (`base:bin/raspar_publico.py:23–40`; `diff:bin/relato.sh:102–108,141,150–161`.) |

7. **Classes históricas, uma a uma.**

   | Classe | Resultado |
   |---|---|
   | **2º juiz** | Não surge segundo juiz de nomes: raspagem reutiliza `nome_no_texto`. A lista independente no teste serve de fixture contra troca de obra. (`base:bin/raspar_publico.py:15–16,31,36`; `diff:bin/tests/test_mapa20_valida.sh:3–4,20–22`.) | [nao verificado]
   | **2º escritor fora da porta** | Os dois caminhos de escrita pública estão no mesmo publicador, sob a trava existente. Escritor efetivo da fonte: [nao verificado]. (`base:bin/relato.sh:27`; `diff:bin/relato.sh:96–112,141`.) | [nao verificado]
   | **Relógio fora da borda** | Nenhum relógio de decisão novo; o `date` permanece na mensagem de commit. O timer de publicação já existe. (`base:bin/relato.sh:130`; `diff:bin/relato.sh:159`; `base:bin/hasner-relato.timer:5–7`.) | [nao verificado]
   | **Derivação paralela de fonte única** | O publicador consome a foto validada e conserva o raspador existente. Reuso dos censos e medidas pela função real: [nao verificado]. (`diff:bin/relato.sh:96–97,141`; `aval_mapa_ate_o_20_1111.txt:1`, item 1.) | [nao verificado]
   | **Selo sem mordida** | **Presente:** cercas de tils e a propriedade da foto admitem os mutantes descritos no item 4. (`diff:bin/tests/test_mapa20_valida.sh:29–49,83–109`.) |
   | **Ordem de trava** | Não aparece nova aquisição. A chamada `codex_estado.sh --gerar` retorna antes da trava do ramo evento, evitando autoespera nessa costura. (`base:bin/relato.sh:27,97`; `base:bin/codex_estado.sh:54–56,65–66`.) |
   | **Pouso antes do pré-requisito** | O comentário referencia contrato, mas contrato efetivo, mapa completo/publicado, suíte e opus desta volta: [nao verificado]. Esses requisitos constam do aval e das regras da base. (`diff:bin/relato.sh:93`; `aval_mapa_ate_o_20_1111.txt:1`; `base:CLAUDE.md:377–381,394`.) | [nao verificado]
   | **Campo com dois significados** | `MAPA_RC` significa falha da etapa MAPA, não rc puro do Python; `MAPA_PATH` também representa remoção, não prova de mapa novo publicado. O fluxo é coerente com esses usos, mas nenhum deles comprova “validador executou” ou “mapa novo está no ar”. (`diff:bin/relato.sh:94–99,148–162`.) | [nao verificado]

## 11/11 10:55 · sessao f8e3618b4d7e · - · - · origem=Ronald (tmux)

Contraexemplos confirmados em memória, sem criar arquivos:

- **Aceita tabela torta**: cabeçalho de três colunas com separador de duas; retorna `P=[]`, pois só verifica o conteúdo das células do separador, sem comparar sua largura. Também aceita `|- - -|- - -|- - -|`. (`diff bin/mapa20_valida.py:64-67`)
- **Aceita tabela ainda cercada**: abertura de quatro crases, fechamento de três e mapa completo; idem com quatro/três tils. Também considera ` ```python ` fechamento. Guarda somente o caractere, sem comprimento nem condição sobre texto restante. (`diff bin/mapa20_valida.py:47-52`)
- **Aceita pontuação sem dado**: células `!!!`, `___`, `…`; o conjunto só contempla `-?.–— ` e a limpeza adicional de asteriscos/crases. (`diff bin/mapa20_valida.py:27`, `:70-71`)
- **Aceita cabeçalho vazio** `| | | |` e mapa de somente duas colunas com todas as 19 linhas. Isso satisfaz o mínimo implementado, mas não comprova as colunas exigidas pelo aval. (`diff bin/mapa20_valida.py:79-89`; `aval_mapa_ate_o_20_1111.txt:item 1`)
- **Recusa tabela sem pipes externos**, produzindo `mapa sem tabela`. (`diff bin/mapa20_valida.py:58-63`)
- **Recusa `_E3_`**, embora aceite `**E3**`; sua normalização não remove underscores. (`diff bin/mapa20_valida.py:30-31`, `:92-94`)
- **Recusa separador precedido por duas barras reais**: na linha `| E3 | caminho\\| nao medido |`, funde as duas últimas células e acusa `linha E3: 2 coluna(s), o cabecalho tem 3`. Não considera a paridade das barras anteriores ao pipe. (`diff bin/mapa20_valida.py:38-39`, `:87-88`)
- **Pipe simples escapado funciona**: `a\|b` permanece uma célula e o mapa completo fica verde. (`diff bin/mapa20_valida.py:34-39`)

A tupla corresponde nominalmente às 18 obras do aval, incluindo `UI-PAINEL`; `SEM DONO` é exigido separadamente. (`diff bin/mapa20_valida.py:19-25`, `:92`; `aval_mapa_ate_o_20_1111.txt:item 1-2`)

Reproduzi os 48 mapas da fixture: nenhuma divergência; o caso adicional de arquivo ausente gera a exceção capturada com retorno 1. Um mutante que remove apenas `|~{3,}` da regex de cercas continua acertando esses 49 casos e aceita um mapa inteiramente entre `~~~`. A fixture testa exclusivamente a cerca de três crases. (`diff bin/mapa20_valida.py:47`, `:99-104`; `diff bin/tests/test_mapa20_valida.sh:29-49`)

## 11/11 10:54 · sessao 1623f482cb44 · - · - · origem=Ronald (tmux)

Fluxo após diff, com linhas **novas** de `bin/relato.sh`:

- `MAPA_RC=0; MAPA_PAR=""` em **94**; `cp` e validador em **96**; sucesso escreve `MAPA_PAR` em **97**; falha de qualquer um escreve `MAPA_RC=1` em **99**. Nenhuma outra atribuição a `MAPA_RC` no diff. Portanto validador executado vermelho não termina com rc 0; verde termina com rc 0 salvo outra falha. (`diff bin/relato.sh:94–99,155,161–162`.)
- Todos os `exit`: base **27,28,32,35,38,42,86**, todos `1`; novos **126** (`gerar_avais`, `1`), **141** (raspagem geral, `1`), **155** (`"$MAPA_RC"`: `0` ou `1`), **161** (falha do push/echo: `1`), **162** (`"$MAPA_RC"`: `0` ou `1`). A inserção de 24 linhas deixa as primeiras sete posições intactas. (`bin/relato.sh` base citada; `diff bin/relato.sh` linhas novas citadas.)
- Pode remover público **sem executar o validador**: fonte ausente/ilegível faz `cp` falhar, o `&&` pula Python; re-raspagem da cópia existente falha, `rm` apaga `MAPA-20.md`, `MAPA_RC` permanece `1`; etapa 4 inclui remoção rastreada. Isso não é caminho com **validador verde**. (`diff bin/relato.sh:96,99,102–109,148–152,159–162`.)
- Não encontrei caminho interno com validador verde que entre nesse `rm`: está exclusivamente no `else` do `cp && validar`. A raspagem geral ainda pode falhar por motivos próprios e sai `1`. (`diff bin/relato.sh:96–112,127–141`.)

QUEM ESCREVE / QUEM LÊ:

| Valor | Escritor | Leitor |
|---|---|---|
| `MAPA_RC` | shell: inicializa `0`, falha fixa `1` | saídas de “nada mudou” e final |
| `MAPA_PAR` | shell: vazio / par da foto | argumento opcional da raspagem geral |
| `MAPA_PATH` | shell: vazio / nome após `git add` bem sucedido | mensagem e pathspec do commit | [nao verificado]
| `$TMP/mapa20.md` | `cp` da fonte | validador; raspador pela lista de pares |
| `logs/MAPA-20/mapa.md` | sessão, **apenas declarado em comentário**, produtor real não fornecido | `cp` |
| `$DEST/MAPA-20.md` | `raspar_publico.py:40`, ou troca do arquivo re-raspado com `mv`; remoção com `rm` | raspador no ramo retido, teste `-f`, Git | [nao verificado]

Refs: `diff bin/relato.sh:89–112,141,148–162`; `bin/raspar_publico.py:23–40`. Escritor efetivo/eventos da fonte: **[nao verificado]**. O diff não cria gerador nem gatilho para regravar a fonte em pouso/reprovação/lei: o comentário delega à sessão. (`diff bin/relato.sh:89–90`.)

Classes:

- **Segundo juiz:** raspagem reaproveita o juiz único de privacidade: `raspar_publico.py:15–16,31,36`, `nome_no_texto.py:59,87`; não introduz uma segunda regex de nomes.
- **Segundo escritor:** duas rotas de atualização do público estão no mesmo publicador sob a trava já existente: `bin/relato.sh` base **27**, diff **96–112,141**; produtor da fonte fora desse bloco não fornecido.
- **Relógio:** nenhum relógio novo de decisão no diff; `date` continua só na mensagem do commit (`base bin/relato.sh:130`, diff **159**). Timer já existente em `bin/hasner-relato.timer:5–7`; chama script por `bin/hasner-relato.service:15`.
- **Fonte única / derivação:** `MAPA_PAR` faz publicador consumir a foto efetivamente validada (`diff bin/relato.sh:96–97,141`); censo/fonte/medidas de negócio não são computados pelo diff **[nao verificado]**.
- **Trava:** nenhuma trava nova; `codex_estado.sh --gerar` sai antes da aquisição de lock do ramo evento, logo não autoespera na trava que `relato.sh` já segura. (`bin/relato.sh:27`; `bin/codex_estado.sh:54–56,65–66`.)
- **Pouso/pré-requisito:** contrato MAPA-20 é apenas referenciado pelo novo comentário; não consta no texto físico desta base. Comprovação do contrato, fonte pronta, mapa publicado, selos executados e opus: **[nao verificado]**. A regra de contrato está em `CLAUDE.md:394`, suíte/opus em **377–381**. [nao verificado]
- **Campo com dois significados:** `MAPA_RC` é resultado acumulado da etapa MAPA (cópia **ou** validação), não rc puro do Python; `MAPA_PATH` identifica artefato rastreado inclusive para remoção, não prova de publicação nova. Isso está explícito no uso (`diff bin/relato.sh:94–99,148–152`). Confundir os nomes com “validador verde”/“mapa publicado” produziria conclusões erradas.

Limite preexistente útil: `set -uo pipefail` sem `-e`, `cd`, `git add` geral e `git commit` sem checagem continuam na base (`bin/relato.sh:21,120–132`), agora linhas novas **144–161**. Não são cura completa pelo novo `push || exit 1`; falha de commit seguida de push bem sucedido pode ainda terminar `0` quando mapa verde, embora exista outra falha.

## 11/11 10:52 · sessao 1239d5bfa276 · - · - · origem=Ronald (tmux)

Nenhum dos cinco selos vizinhos tem quebra estática causada pelo diff; execução completa dos selos ficou [nao verificado], pois eles criam arquivos e/ou acessam recursos externos. A busca equivalente a `git grep 'relato.sh' bin/tests` encontra referências nos cinco arquivos abaixo. (`bin/tests/test_codex_estado.sh:100`; `bin/tests/test_codex_estado_evento.sh:28`; `bin/tests/test_relato_publica_claude_md.sh:8`; `bin/tests/test_relato_afirma_com_prova.sh:44`; `bin/tests/test_publicacao_sem_nome.sh:6`.)

1. **`test_codex_estado.sh`: não quebra.** O recorte começa na primeira ocorrência literal de `CODEX_OK=""` e termina no marcador do passo 3; só busca `exit` nesse intervalo, excluindo comentários. O bloco 2f novo fica antes do bloco 2e e de `CODEX_OK=""`, portanto seus novos comandos não entram nessa região. (`bin/tests/test_codex_estado.sh:104-111`; base `bin/relato.sh:89-98`; diff `bin/relato.sh:89-122`, linhas novas.)

2. **`test_relato_publica_claude_md.sh`: não quebra.** A raspagem geral mantém o par CLAUDE; o novo raspador de MAPA retido não casa com a regex que exige continuação de linha imediatamente depois de `"$DEST"`. O teste conta apenas linhas começando na coluna 1 por `git add -A `; o novo `git add -A -- MAPA-20.md` tem indentação e não aumenta essa contagem. O pathspec do commit continua contendo CLAUDE. (`bin/tests/test_relato_publica_claude_md.sh:13-20`; diff `bin/relato.sh:104-105`, `141`, `145`, `151`, `160`, linhas novas.)

3. **`test_codex_estado_evento.sh`: não quebra na costura.** Continua sem `gerar_codex_estado.py` em `relato.sh`, mantendo chamadas de `codex_estado.sh` e `relato_nomes.sh`. (`bin/tests/test_codex_estado_evento.sh:27-30`; base `bin/relato.sh:32`, `97`; diff não altera essas chamadas.)

4. **`test_relato_afirma_com_prova.sh`: não quebra na costura.** Seu predicado apenas exige a presença de `relato_afirma_com_prova.py`, preservada pelo diff. (`bin/tests/test_relato_afirma_com_prova.sh:43-45`; base `bin/relato.sh:55`.)

5. **`test_publicacao_sem_nome.sh`: não recorta `relato.sh`.** As referências são comentários; o universo efetivo vem de `git ls-files` do público. MAPA-20 rastreado entra automaticamente nesse universo. Resultado de privacidade em produção permanece [nao verificado]. (`bin/tests/test_publicacao_sem_nome.sh:58-72`.)

## 11/11 10:48 · sessao ff7bc3086497 · UI-PAINEL-MOSTRA-O-APP · construtor · origem=Code

Cura aplicada na cópia: faixa, perguntas em leitura e contador no [template](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/templates/colaboradores/partials/painel_colaborador.html:39); projeção única em [pergunta_no_app.py](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/chamados/services/pergunta_no_app.py:78). `consulta_do_app` preserva seu contrato; os dois `/me/` mantêm a construção do JSON (`api/views.py:316`, `api/views_core.py:304`).

Não fiz commit: prevaleceu a proibição inicial. Encaixe, certificação e smoke permanecem com a sessão/Code. [nao verificado] [nao verificado]

Arquivos alterados:

- `templates/colaboradores/partials/painel_colaborador.html:39`: contador; faixa e lista em `:190`.
- `colaboradores/views.py:940`: painel recebe a projeção.
- `api/views.py:315` e `api/views_core.py:303`: substituição das regras inline; remoção dos imports tornados inúteis.
- `chamados/services/pergunta_no_app.py:78`: projeção ao lado da autoridade existente.
- `chamados/tests/e3_mutantes.py:23`: alvos `badge_disputas`, `kernel_core_disputas` e `vermelho_autor_errado` atualizados após a extração.
- `core/tests/test_tripwire_contagem_de_pendencia.py:83`: dois sítios antigos substituídos pelo único; teto reduzido para 25 em `:147`.
- `colaboradores/tests/test_ui_painel_mostra_o_app.py:1`: selo em memória e registro do RED.
- `colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:54`: selo painel × duas APIs.
- `colaboradores/tests/ui_painel_fontes.py:19`: quatro mutantes, exclusivamente em memória.

O RED anterior à cura registrou **cinco falhas por assertion**, sem erros. Depois, passaram 13 testes em memória; os quatro mutantes novos morreram por assertion. Tripwire e paridade passaram em 19 testes com adaptação em memória de `SimpleTestCase`. `py_compile` compilou os nove Python com escrita de bytecode interceptada; `git diff --check` passou. O registro inicial está em `colaboradores/tests/test_ui_painel_mostra_o_app.py:7`; resultados posteriores não foram gravados em arquivo. [nao verificado]

**ORM, RED visual e smoke não foram executados; faltam Django e Ruff neste ambiente.** O selo ORM cobre os três casos, a precedência com mensagem simultânea, identidade por campos e rótulo variável; sua execução continua pendente (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:135`). As duas cascas têm prova pelo grafo completo até o carregamento do painel (`colaboradores/tests/test_ui_painel_mostra_o_app.py:83`).

Quem escreve e quem lê cada campo usado?

| Campo | Escritores | Leitores |
|---|---|---|
| `pendentes` | Lista derivada por `consulta_do_app`, usando `respondida_em` e `fechada_em` (`chamados/services/pergunta_no_app.py:69`). Respostas/reaberturas: `chamados/services/disputa_emissao.py:1468`, `:1593`, `:1653`, `:1674`; `chamados/models.py:1566`, `:3132`. Fechamento/reabertura da disputa: `chamados/models.py:1932`, `:2149`, `:2173`. | Projeção `chamados/services/pergunta_no_app.py:89`; listagens `api/views.py:993`, `:2531`; dashboard `core/views.py:253`; aviso `chamados/services/aviso_pendencia.py:9`; cobrança `chamados/services/cobranca.py:48`. |
| `lido_colaborador_em` | Nasce nulo (`chamados/models.py:1673`, `chamados/services/nota.py:88`); `marcar_lidas` grava o carimbo (`chamados/services/nota.py:126`). | Projeção `chamados/services/pergunta_no_app.py:95`; serializador `api/views.py:1116`; web `chamados/views.py:603`; cartão `templates/chamados/partials/_card_atendimento.html:51`. |
| `status_local` | Nascimento pela porta `criar` (`chamados/models.py:1095`); transições persistentes por `_gravar_estado` (`chamados/models.py:201`, `:296`). | A projeção lê `VIVOS` (`chamados/services/pergunta_no_app.py:93`), cujo conjunto vem de `chamados/catalogo/motor.py:24`. |

Há outro leitor da regra de rótulo? **Sim:** `templates/core/dashboard_colaborador_v2.html:80`, alimentado por `core/views.py:253`. Permaneceu fora do diff e deve entrar na fila. Outros resultados do grep são textos de navegação/relatórios e expectativas/mutantes de testes (`templates/chamados/form_abertura.html:7`, `relatorios/views.py:1672`, `inteligencia/views.py:255`, `chamados/tests/test_e3_app_orm.py:44`, `colaboradores/tests/ui_painel_fontes.py:26`). Não houve ocorrência em JS. [nao verificado]

O que fazem os antigos leitores `api/views.py:1004/:2542`? Agora em `:993/:2531`, continuam usando `consulta_do_app`: listam perguntas/histórico e publicam perguntas da disputa; não calculam badge. Não foram alterados. O GET de chamados marca notas como lidas em `api/views.py:1038`; o selo usa exclusivamente `/me/` (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:93`).

Comandos para a sessão, no banco de ensaio; esperado **PASS**, ainda [nao verificado]:

```bash
export PYTHONDONTWRITEBYTECODE=1
export DJANGO_SETTINGS_MODULE=config.settings.ci

python3 manage.py test \
  colaboradores.tests.test_ui_painel_mostra_o_app \
  colaboradores.tests.test_ui_painel_mostra_o_app_orm \
  chamados.tests.test_e3_app_orm \
  chamados.tests.test_e3_v4_orm \
  colaboradores.tests.test_retrato_no_drawer \
  api.tests.test_contract_paridade_kernels \
  core.tests.test_tripwire_contagem_de_pendencia
```

Cada comando abaixo deve terminar com **FAIL por assertion**, sem `ERROR`; instalação dos mutantes em `colaboradores/tests/ui_painel_fontes.py:56`. Resultado ORM esperado [nao verificado]:

```bash
morder() {
  UI_PAINEL_MUTANTE="$1" python3 manage.py test \
    colaboradores.tests.test_ui_painel_mostra_o_app_orm
}
morder painel_query
morder template_rotulo
morder api_ui_precedencia
morder api_core_precedencia

python3 -m chamados.tests.e3_mutantes --mutante badge_disputas
python3 -m chamados.tests.e3_mutantes --mutante kernel_core_disputas
python3 -m chamados.tests.e3_mutantes --mutante vermelho_autor_errado
```

Os três alvos E3 ajustados também têm mordida por assertion em memória (`colaboradores/tests/test_ui_painel_mostra_o_app.py:154`). Reprodução local e Ruff pendente:

```bash
python3 -m unittest colaboradores.tests.test_ui_painel_mostra_o_app \
  chamados.tests.test_e3_pendentes_app -v

UI_PAINEL_MUTANTE=painel_query python3 -m unittest colaboradores.tests.test_ui_painel_mostra_o_app
UI_PAINEL_MUTANTE=template_rotulo python3 -m unittest colaboradores.tests.test_ui_painel_mostra_o_app
UI_PAINEL_MUTANTE=api_ui_precedencia python3 -m unittest colaboradores.tests.test_ui_painel_mostra_o_app
UI_PAINEL_MUTANTE=api_core_precedencia python3 -m unittest colaboradores.tests.test_ui_painel_mostra_o_app

ruff check --no-cache --config ruff.toml \
  api/views.py api/views_core.py colaboradores/views.py \
  chamados/services/pergunta_no_app.py chamados/tests/e3_mutantes.py \
  core/tests/test_tripwire_contagem_de_pendencia.py \
  colaboradores/tests/*ui_painel*.py
```

LEI-AKITA: origem=chamados.services.pergunta_no_app.projecao_do_app, testemunha=consulta_do_app, RED=colaboradores.tests.test_ui_painel_mostra_o_app_orm.PainelMostraOAppORMTest, quem-mais-le=api/views.py:993/2531+core/views.py:253+dashboard_colaborador_v2.html:80+aviso_pendencia.py:9+cobranca.py:48, juizes novos=0, padrao=projecao

## 11/11 10:48 · sessao 33adf0aa3653 · MAPA-ATE-O-20 · censo · origem=Code

MAPA-ATE-O-20 — cópia informada `9ca563bb3e0470eae1586b080303b2b026c85455`; identidade Git da cópia: [nao verificado].

Referências: **B:n** = `backlog_obras.txt:n`; **A:n** = `arquivos_obras.txt:n`, contando desde o primeiro cabeçalho do anexo; **L:n** = linha **n do RELATO**, transcrita em `leis_abertas.txt`; **R:n** = `app/docs/RELATO.md:n`. Os identificadores entre parênteses na primeira coluna são abreviações usadas nos cruzamentos. As referências de cada linha sustentam suas células; cruzamento significa arquivo compartilhado, sem presumir alteração no mesmo trecho. (`arquivos_obras.txt:1`; `backlog_obras.txt:2367`.)

Prefixos: **C/** = `app/chamados/`; **S/** = `app/chamados/services/`; **Q/** = `app/chamados/management/commands/`; **P/** = `app/ponto/services/`; **QP/** = `app/ponto/management/commands/`; **H/** = `app/templates/chamados/partials/`. Arquivos Python abreviados conservam `.py`. Essa convenção apenas encurta os caminhos citados nos censos. (`app/chamados/models.py:1853`; `app/chamados/services/materializacao.py:425`.)

**K34** é o perímetro compartilhado reaproveitado de F3/F4: `C/{models,juizes,reconciliador,signals,relatorios_disputa,views_cobrar}.py`; `S/{tarefa_do_dia,materializacao,validacao,disputa_emissao,fio,painel,verbos,explicador,declaracao_texto,becos,acoes_disputa,raiox}.py`; `app/api/views.py`; `P/cartorio.py`; `app/ponto/supra_juiz.py`; `Q/{auditar_invariantes_chamados,migrar_data_evento_dia_do_turno}.py`; `QP/reconciliar_grade.py`; `H/{modal_fio,_card_atendimento}.html`. São sítios do censo comum, **sem divisão comprovada de alterações entre F3 e F4**. (`app/chamados/models.py:1853`; `app/chamados/services/materializacao.py:425`; `app/chamados/services/validacao.py:461`; `app/chamados/services/disputa_emissao.py:1508`; `app/chamados/reconciliador.py:411`; `f3f4_censo.r` [nao verificado: numeração do anexo].)

**K4** reaproveita o censo E4C: inclui os arquivos de K34 também inventariados ali, mais `app/api/views_core.py`, `S/vigia_fio.py`, `app/colaboradores/services/calendario.py`, `P/dia_do_colab.py`, `C/templatetags/chamados_tags.py`, `app/core/views_ia.py`, `H/_lote_preview.html`, `app/templates/core/_copiloto.html` e `app/templates/chamados/{detalhe,detalhe_local,fio_colaborador}.html`. (`app/chamados/services/vigia_fio.py:675`; `app/colaboradores/services/calendario.py:474`; `app/ponto/services/dia_do_colab.py:70`; `app/api/views_core.py:313`; `app/chamados/templatetags/chamados_tags.py:128`; `censo.r` [nao verificado: numeração do anexo].)

**†** identifica obra sem censo integral entregue nestes anexos; não afirma inexistência de censo fora deles. `nao medido` não significa zero. VISIVEL descreve o resultado pretendido documentado, sem afirmar que já foi entregue. [nao verificado: cobertura integral fora dos anexos.]

| obra | arquivos que toca | cruza com | leis em aberto | depende de | move fio_morto_alcance | move a Central | VISIVEL | voltas ja gastas | etapa |
|---|---|---|---|---|---|---|---|---|---|
| **E3†** — alias de FIO-E0-V4-E3-LEITORES (B:1866) | `C/juizes.py`; `S/{carga_pergunta_viva,materializacao,tarefa_do_dia}.py`; `app/escala/servico_jornada.py`; `app/folha/export.py`; `P/cartorio.py`; `app/ponto/turnos.py` (A:2; A:3; A:4; A:5; A:18; A:19; A:20; A:25) | E4B, E4C, RAIZ F3/F4, O168, LASTRO, CAL-LE, LP2, LP2F, E2, A17, A18, J760, D5 | L:4, corte do pouso; L:554, unidade do app, trecho incompleto | Corte para os 35 dias com célula trabalha=True de desligados; sem ele não pousa (L:4) | **nao medido**. Impacto de frota: cura muda 206 dias contra 177 da base; 117 exclusivos em 12 colabs fora de X1, incluindo os 35 barrados (L:4; dado da sessão [nao verificado por arquivo:linha para 206/177]) | nao medido | Fila e verbos passam a consumir tarefa_do_dia (B:1866) | volta 7 (B:1866); X1 volta 3 (R:10) | **PAREI** (L:4) |
| **E4B†** — alias de FIO-E0-V4-E4B (B:1887) | `C/juizes.py`; `Q/backfill_fio_morto_e4b.py`; `S/{disputa_emissao,foto_e4b}.py`; `app/config/crons.py`; `P/cartorio.py` (A:27; A:28; A:29; A:30; A:47; A:58) | E3, E4C, RAIZ F1–F4, O168, LASTRO, LP2, LP2F, LP3, PRESC, F0F1, E4A2, BOD, BOM, J760, D5 | nenhuma; sem_porta foi respondida (R:82) | E3 antes do apply; E4a no ar antes do backfill; exportadas exigem o corte próprio (R:99; B:1878; B:1887) | nao medido | nao medido | O passivo volta às portas canônicas; credor com pergunta viva recupera alcance (B:1887; R:82) | 5 voltas Codex; c4 em preparo no registro (R:49; R:30) | construtor (A:26; R:30) |
| **E4C** — sem linha própria; fase de B:1878 | K4; migração de leitores, preservando exceções (`app/chamados/models.py:3034`; `app/chamados/services/tarefa_do_dia.py:398`) | E3, E4B, RAIZ F1–F4, O168, LASTRO, ROTULO, LOTE, CAL-LE, LP2, LP2F, LP3, JOB, E1, E2, ANCORA, F0F1, RET, A03/A05/A06/A17/A18/A20, BOD, BOM, F4C, J760, D5, RESTO, O137; também E4CAL-P3-ULTIMO-RECURSO-ENGOLE, E4CAL-P3-CENSO-HOJE-PORTA-CURTA e E4CAL-L1-P3 | **L:28 — D04 e D05** | E4B medida antes de trocar a fonte; respostas D04/D05 para os respectivos contratos; coordenação das testemunhas com F3/F4 (`app/docs/PROMPTS.md:2034`; `app/chamados/services/tarefa_do_dia.py:398`; L:28) | nao medido | nao medido | Fases, respostas, atividade e filas passam a ler decisões e efeitos; apresentação depende de D04/D05 (L:28) | nao medido | censo |
| **E4D†** — sem linha própria; fase de B:1878 | nao medido | nao medido | nenhuma própria | E4C retirar leitores migráveis antes de parar projeção; seis exceções permanecem (`app/docs/PROMPTS.md:2030`; `app/docs/PROMPTS.md:2034`) | nao medido | nao medido | nada na tela, como mudança própria documentada (B:1878) | nao medido | fila |
| **RAIZ F1†** — em F0F1, sem linha individual (B:1875) | `C/models.py:2982/:3014`; `S/fio.py:925`; `S/disputa_emissao.py:238/:1197`; `S/declaracao_texto.py:377` (R:19) | E4B, E4C, RAIZ F2–F4, LASTRO, LP3, E1, ANCORA, F0F1, RET, A03/A05/A06, BOD, BOM, F4C, PLANTIO | L:186, h2; eleição do destino consta RESPONDIDA em B:1875 | F0; entrada do c22 depois de E3 e E4B c4 (B:1875) | nao medido | nao medido | Destino da pergunta passa a ter uma eleição comum; mudança visual específica nao medido (R:19; B:1875) | nao medido | censo |
| **RAIZ F2†** — alias da BO-DISPUTA (B:1895) | `Q/auditar_invariantes_chamados.py`; `S/{declaracao_texto,descarte_disputa,disputa_emissao}.py`; `app/core/observ.py`; `QP/reconciliar_grade.py` (A:68; A:69; A:70; A:71; A:94; A:98) | E4B, E4C, RAIZ F1/F3/F4, LASTRO, LP3, F0F1, BOD, BOM | L:186, conflito h2 | F1 e c22; nascimento e destino pela raiz (B:1875; B:1895) | nao medido | nao medido | Evita disputa nascer sem pergunta; legado não é removido por shell (B:1889) | c21 existente; c22 ainda previsto (B:1875) | fila |
| **RAIZ F3** — sem linha própria (B:1895) | K34, perímetro comum do censo; divisão F3/F4 nao medido (`app/chamados/models.py:1853`; `app/chamados/services/materializacao.py:425`) | E3, E4B, E4C, RAIZ F1/F2/F4, O168, LASTRO, ROTULO, LOTE, CAL-LE, LP2, LP2F, LP3, JOB, E1, E2, ANCORA, F0F1, RET, A03/A05/A06/A17/A18/A20, BOD, BOM, F4C, J760, D5, PLANTIO, RESTO, O137 | nenhuma impeditiva; D8 não trava F3/F4 (L:31) | F2; aplicação e reação da raiz coordenadas; autor do fecho segue o último carimbo (B:1895; B:1896) | nao medido | nao medido | Fechamento e explicação passam a respeitar C7 e autoria dos efeitos (`app/chamados/models.py:1853`; `app/chamados/services/explicador.py:137`) | nao medido | censo |
| **RAIZ F4** — sem linha própria (B:1895) | K34; identidade do par em `C/models.py:3441`, fonte PARES_PAUSA_DNA (B:1896) | E3, E4B, E4C, RAIZ F1/F2/F3, O168, LASTRO, ROTULO, LOTE, CAL-LE, LP2, LP2F, LP3, JOB, E1, E2, ANCORA, F0F1, RET, A03/A05/A06/A17/A18/A20, BOD, BOM, F4C, J760, D5, PLANTIO, RESTO, O137 | nenhuma impeditiva; D8 não trava F3/F4 (L:31) | F3; par lido dos marcos da célula, sem novo juiz (B:1895; B:1896) | nao medido | nao medido | As duas metades da pausa são tratadas como par; metade aguarda a outra (`app/chamados/services/validacao.py:461`; `app/chamados/services/materializacao.py:1375`; censo F3/F4) | nao medido | censo |
| **RAIZ F5†** — sem linha própria (B:1895) | nao medido | nao medido | nenhuma própria | F4; coordenada com E4D (B:1895) | nao medido | nao medido | nao medido | nao medido | fila |
| **RAIZ F6†** — sem linha própria (B:1895) | nao medido | nao medido | nenhuma identificada | F5, pela sequência registrada (B:1895) | nao medido | nao medido | nao medido | nao medido | fila |
| **RAIZ F7†** — sem linha própria (B:1895) | nao medido | nao medido | nenhuma identificada | F6, pela sequência registrada (B:1895) | nao medido | nao medido | nao medido | nao medido | fila |
| **O168†** (B:1396) | `app/ponto/supra_juiz.py:539`; `P/cartorio.py:1091` | E3, E4B, E4C, RAIZ F3/F4, LASTRO, LP2, LP2F, D5 | nenhuma: guarda já é lei escrita (B:1396) | Guarda valer no evento antes de retirar --executar; CSF_RETRATADO medido em zero (B:1396) | nao medido | nao medido | Cobrança com conversa deixa de ser fechada pelo caminho rápido cego (B:1396) | nao medido | fila |
| **LASTRO†** — agregado nomeado na META; sem linha própria (B:2367) | Sítios dos filhos: `P/lastro.py:274/:340`; `P/cartorio.py:1374`; `P/dia_do_colab.py:38`; `P/diagnostico_escala.py:155`; `S/disputa_emissao.py`; `C/signals.py:158` (B:1732; B:1762; B:1763) | E3, E4B, E4C, RAIZ F1–F4, O168, LD2, LP2, LP2F, LP3, F0F1, BOD, BOM, D5 | nenhuma identificada | JUIZ-DO-PAR para degrau 2; JUIZ-1 e CRONS-INSTALL-SOB-MARCA para parte 2 (B:1732; B:1761) | nao medido | nao medido | Chamado fecha por lastro do marco e recebe reação por evento; parte 3 já pousada (B:1761; B:1763) | nao medido | fila; parte 3 NO AR (B:1763) |
| **O169-FECHADA†** (B:1940) | `QP/detectar_cluster_espurio.py`; `P/cluster_espurio.py` (A:61; A:62) | O169, L15, VOC | nenhuma de negócio; B3 DESENHO registrado (B:1940) | Volta 5 após E4A2; tratamento do legado pela porta (B:1940) | nao medido | nao medido | Legado não trancado reabre pela porta; trancado relavra a via com trilha (B:1940) | v5 (B:1940; A:60) | PAREI, conforme B3 registrado; liberação nao medido (B:1940) |
| **UI-PAINEL-MOSTRA-O-APP†** (B:1131) | **nenhum no delta da cópia**; planejados `app/api/views_core.py:313` e `app/api/views.py:324` (A:66; B:1131) | nenhum no delta; cruzamento do futuro patch nao medido (A:66) | nenhuma identificada | Mesmo lote de UI-ANEXO-AUSENCIA; projeção reconstruível e contagem única (B:1131) | nao medido | nao medido | Painel passa a mostrar o rótulo e os não lidos pela função comum ao app (B:1131) | nao medido | fila |
| **BO-ROTULO-ENCERRADO (ROTULO)** (B:1899) | `S/verbos.py:114`; `C/templatetags/chamados_tags.py:53`; `app/templates/chamados/{detalhe.html:132,detalhe_local.html:395,fio_colaborador.html:268}`; `H/{modal_fio.html:667,painel_chamado_turno.html:76}` — reuso mapa_3.r | E4C, RAIZ F3/F4, A20, O137 | nenhuma | Pouso O169-FECHADA na raia 2; centralização do vocabulário, pois o modal ainda usa literais (B:1897; `app/templates/chamados/partials/modal_fio.html:667`) | nao medido | nao medido | Seletor distingue encerrar de encerrar e arquivar nos três seletores (`app/templates/chamados/partials/modal_fio.html:668`; `app/templates/chamados/detalhe_local.html:396`) | nao medido | censo |
| **TRIAGEM-OFFLINE-SEM-CAMERA†** (B:1905) | `app/templates/ponto/triagem/apto.html:252`; `app/templates/ponto/bater_ponto_v2.html:287` — cobertura parcial reaproveitada de mapa_1.r/mapa_4.r | RESTO | nenhuma própria identificada | ROTULO na raia 2; juiz comum de admissibilidade e smoke de clique nas duas cascas (B:1897; B:1905) | nao medido | nao medido | Falha de câmera deixa de impedir a entrada da batida offline na fila (B:1905) | nao medido | fila |
| **LOTE-VALIDAR-REGRESSAO-0311 (LOTE)** (B:1911) | `S/validacao.py:695`; `S/acoes_disputa.py:18`; `C/views_cobrar.py:261/:381`; `app/core/views_ia.py:169`; `H/_lote_preview.html:4`; `app/templates/core/_copiloto.html:247` — reuso mapa_2.r | E4C, RAIZ F3/F4, JOB, A20 | nenhuma | TRIAGEM na raia 2; medir causa antes da cura (B:1897; B:1911) | nao medido | nao medido | Mudança visual da cura nao medido; a linha ainda pede investigação (B:1911) | nao medido | censo |
| **E4CAL-P1-REVERTER-AMPLO†** — extra B:852 | nao medido | nao medido | nenhuma identificada | Snapshot por campo antes de permitir reversão ampla (B:852) | nao medido | nao medido | nada na tela | nao medido | fila |
| **E4CAL-P1-PAREI-FM-CORRENTE†** — extra B:853 | nenhum sítio de produção identificado; referência é `logs/e4cal/r8_pouso/comum.py:15` (B:853) | nao medido | nenhuma identificada | Hash apenas de competência exportada (B:853) | nao medido | nao medido | nada na tela | nao medido | fila |
| **E4CAL-P1-SELO-CAMPOS-ACORDAM†** — extra B:854 | nao medido | nao medido | nenhuma identificada | Foto incluir outbox e auditoria da célula (B:854) | nao medido | nao medido | nada na tela | Codex v2 citado; total nao medido (B:854) | fila |
| **E4CAL-P1-ROTULO-SEM-CERTIDAO†** — extra B:855 | nao medido | nao medido | nenhuma identificada | Medir dono L-099 antes da cura (B:855) | nao medido | nao medido | Rótulo congelado sem certidão é o defeito registrado; resultado da cura nao medido (B:855) | nao medido | fila |
| **E4CAL-P3-ULTIMO-RECURSO-ENGOLE†** — extra B:856 | `app/colaboradores/services/calendario.py:436`, sítio registrado, sem censo próprio anexo (B:856) | E4C, CAL-LE, E2, E4CAL-L1-P3 | nenhuma identificada | Exceção após início da competência não deixar último recurso aberto (B:856) | nao medido | nao medido | nao medido | Codex v2 citado; total nao medido (B:856) | fila |
| **E4CAL-P3-CENSO-HOJE-PORTA-CURTA†** — extra B:857 | `S/vigia_fio.py:289/:400` (B:857) | E4C | nenhuma identificada | Injeção do mesmo relógio alcançar a porta curta (B:857) | nao medido | nao medido | nada na tela | Codex v2 citado; total nao medido (B:857) | fila |
| **E4CAL-P3-LEITURA-ESCREVE† (CAL-LE)** — extra B:858 | `app/colaboradores/services/calendario.py`; `S/materializacao.py`, sítios da cadeia citada (B:858; `app/colaboradores/services/calendario.py:474`; `app/chamados/services/materializacao.py:279`) | E3, E4C, RAIZ F3/F4, E4CAL-P3-ULTIMO-RECURSO-ENGOLE, E2, A17, A18, E4CAL-L1-P3 | nenhuma identificada | Separar leitura do efeito registrado como best-effort (B:858) | nao medido | nao medido | nao medido | nao medido | fila |
| **E4CAL-L1-POUSO-0411†** — extra B:859 | nao medido | nao medido | nenhuma identificada | Rebase e árvore certificada do pouso, já registrado (B:859) | nao medido | nao medido | nao medido | v2 citada; total nao medido (B:859) | **NO AR** (B:859) | [nao verificado]
| **E4CAL-P3-DOCS-VELHAS†** — extra B:860 | `app/core/juizes.py:717`, comentário; demais arquivos citados são documentação (B:860) | nao medido | nenhuma identificada | Marco da L1 (B:860) | nao medido | nao medido | nada na tela | nao medido | fila |
| **E4CAL-CARD-LE-NUCLEO†** — extra B:862 | `app/templates/colaboradores/partials/_calendario_grade.html:169` (B:862) | nao medido | nenhuma identificada | Retomada da L1; ler rótulos do núcleo (B:862) | nao medido | nao medido | Cards do rodapé recebem o vocabulário do núcleo (B:862) | nao medido | fila |
| **O169†** — extra B:1397 | `QP/detectar_cluster_espurio.py:61`; `P/cluster_espurio.py:55/:95` (B:1397; B:1940) | O169-FECHADA, L15, VOC | nenhuma: morte só por lastro consta decidida (B:1940) | Delta adicional acompanhado em O169-FECHADA (B:1940) | nao medido | nao medido | Chamado de cluster deixa de morrer apenas por idade (B:1940) | nao medido | **NO AR**, C20-O169; delta separado (B:1940) |
| **LASTRO-DEGRAU-2† (LD2)** — extra B:1732 | `P/lastro.py:274` (B:1732) | LASTRO, LP2, LP2F | nenhuma identificada | Pouso JUIZ-DO-PAR; pergunta ao papel do marco, não ao raio (B:1732) | nao medido | nao medido | nao medido | nao medido | fila; proposta anterior reprovada/superada (B:1732) |
| **LASTRO-PARTE-2† (LP2)** — extra B:1761 | `P/{lastro.py:340,cartorio.py:1374,dia_do_colab.py:38,diagnostico_escala.py:155}` (B:1762) | E3, E4B, E4C, RAIZ F3/F4, O168, LASTRO, LD2, LP2F, D5 | nenhuma identificada | JUIZ-1 e CRONS-INSTALL-SOB-MARCA antes do pouso (B:1761) | nao medido | nao medido | Fecho por marco passa a reconciliar; varredor vira alarme (B:1761) | nao medido | fila | [nao verificado]
| **LASTRO-PARTE-2-FILA-OPUS† (LP2F)** — extra B:1762 | Mesmos sítios de LP2; `C/signals.py:158` também citado (B:1762) | E3, E4B, E4C, RAIZ F3/F4, O168, LASTRO, LD2, LP2, LP3, BOD, RAIZ F2 | nenhuma identificada | Tratar achados do opus da parte 2; escopo de cada item ainda requer a medida indicada (B:1762) | nao medido | nao medido | nao medido | opus final citado; total nao medido (B:1762) | fila |
| **LASTRO-PARTE-3† (LP3)** — extra B:1763 | `S/disputa_emissao.py`; `C/signals.py`, reação ao estado da pergunta (B:1763) | E4B, E4C, RAIZ F1–F4, LASTRO, LP2F, F0F1, BOD, BOM | nenhuma identificada | Parte 2 e reação por evento; pouso registrado (B:1763) | nao medido | nao medido | Pergunta que morre aciona a reação do chamado (B:1763) | nao medido | **NO AR** (B:1763) |
| **O169-ATA-VAZIA-FOLGA†** — extra B:1795 | nenhum arquivo de produção identificado para o selo (B:1795) | nao medido | nenhuma identificada | Caso trabalha=False e ata vazia morder a ordem dos insumos (B:1795) | nao medido | nao medido | nada na tela | nao medido | fila |
| **O169-CONTADOR-NAO-MEDIDO†** — extra B:1796 | nao medido | nao medido | nenhuma identificada | Contabilizar fato None e provar vigia/caso (B:1796) | nao medido | nao medido | nada na tela | Codex v2 citado; total nao medido (B:1796) | fila |
| **O169-LIMIAR-15-DUPLO† (L15)** — extra B:1797 | `app/escala/orfa_classe.py`; `P/cluster_espurio.py`; linhas dos sítios nao medido (B:1797) | O169-FECHADA, O169, VOC | nenhuma identificada | Assinatura por juiz único (B:1797) | nao medido | nao medido | nada na tela | Codex v2 citado; total nao medido (B:1797) | fila |
| **LASTRO-FOLGA-FORA-DO-CATALOGO†** — extra B:1808 | nao medido | nao medido | nenhuma identificada | Medir escritor e frota antes da cura (B:1808) | nao medido | nao medido | nao medido | nao medido | censo |
| **LOTE-VALIDAR-VIRA-JOB (JOB)** — extra B:1832 | Mesmos caminhos de LOTE; resultado atual em `S/validacao.py:726/:762`, `C/views_cobrar.py:383`, `app/core/views_ia.py:173`, `H/_lote_preview.html:4` e `app/templates/core/_copiloto.html:247` — reuso mapa_2.r | E4C, RAIZ F3/F4, LOTE, A20 | nenhuma | Mecanismo existente de job; conservar confirmação, autoria e reclassificação pela porta (`app/chamados/services/validacao.py:698/:735/:746`; B:1832) | nao medido | nao medido | Requisição deixa de esperar o lote; tela lê resultado posterior (B:1832) | nao medido | censo |
| **FIO-E0-V4-E1-ENTRADAS† (E1)** — extra B:1852 | Recorte PLANTIO reaproveitado: `C/models.py:2776`; `S/fio.py:800/:979`; `Q/migrar_data_evento_dia_do_turno.py:179`; restante da obra nao medido | E4C, RAIZ F1/F3/F4, ANCORA, F0F1, RET, A03/A05/A06, F4C, PLANTIO | nenhuma identificada | Portas do fato e censo de todas as portas; apply em exportada exige corte (B:1852) | nao medido | nao medido | nao medido | voltas 5, 8 e 15g citadas; total consolidado nao medido (B:1852) | construtor, conforme registro anexado (B:1852) |
| **FIO-E0-V4-E1-FILA†** — extra B:1853 | nao medido | nao medido | nenhuma identificada | E1; V2-2 permanece em linha própria (B:1853) | nao medido | nao medido | nao medido | opus final volta 5; demais voltas nao medido (B:1853) | **NO AR** (B:1853) |
| **FIO-E0-V4-E2-FUNCAO† (E2)** — extra B:1865 | `S/tarefa_do_dia.py:562`; `app/colaboradores/services/calendario.py:163/:250` (B:1865; L:611) | E3, E4C, RAIZ F3/F4, E4CAL-P3-ULTIMO-RECURSO-ENGOLE, CAL-LE, E4CAL-L1-P3 | nenhuma nova: L:611 é encaixe com decisão já escrita | E1 antes do rebase; cura residual do relógio continua na fila E2 (B:1865; L:611) | nao medido | nao medido | Ação do dia é atribuída ao admin conforme decisão registrada (L:611) | 4 voltas Codex; v6 de suíte (B:1865) | **NO AR**, com residual em fila (B:1865; L:611) |
| **E4B-D2-ANCORA-RAIOX† (ANCORA)** — e

[truncado em 20000 de 39149 caracteres]

## 11/11 10:45 · sessao 599f583cc054 · - · - · origem=Ronald (tmux)

Não encontrei falha concreta de runtime nos três arquivos novos, por inspeção. ORM continua [nao verificado].

- As duas perguntas reutilizam o cenário E3 existente, com marcos `fim`/`int_ida`; a fixture cria identidades distintas. (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:137`, `:138`; `chamados/tests/test_e3_app_orm.py:47`; `chamados/services/declaracao_texto.py:42`)
- O painel usa superusuário e passa o RBAC; o request não depende de sessão nos processadores examinados. (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:70`, `:89`; `colaboradores/mixins.py:328`)
- Os mutantes Python recompilam a função decorada usando o namespace real do módulo; os testes invocam exatamente o atributo substituído. Não vi caminho para morrer por nome ausente. (`colaboradores/tests/ui_painel_fontes.py:75`, `:78`, `:79`; `colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:90`, `:98`)
- O mutante do template alcança `render_to_string` pelo `loader.get_template`, preservando os includes normais. (`colaboradores/tests/ui_painel_fontes.py:66`, `:68`, `:73`)
- O selo de origem variável exige três chamadas e o mesmo colaborador; mata a regra manual mesmo quando os três casos usuais coincidiriam. (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:166`, `:175`, `:177`)
- As cascas são comprovadas pelo grafo, incluindo URL, view, partial e mecanismo de injeção; não há falsa prova por render duplicado do partial. (`colaboradores/tests/test_ui_painel_mostra_o_app.py:83`, `:95`, `:97`, `:101`, `:105`)

Há uma limitação concreta de cobertura: o selo novo de mensagem possui um chamado do colaborador e um do DP; sozinho, esse cenário não distingue o mutante E3 de autor trocado porque ambos contam `1`. A matriz E3 existente tem dois chamados do colaborador e mata esse mutante por diferença `2 × 1`; mantenha seu comando na entrega. (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:153`, `:157`; `chamados/tests/test_e3_app_orm.py:67`, `:69`, `:72`; `chamados/tests/e3_mutantes.py:87`)

O ajuste do tripwire é necessário para a extração: substitui duas declarações pelo sítio único e reduz o teto; não cria exceção nova em quantidade. A autorização depende da cláusula do usuário que permite adaptar testes existentes presos à regra inline. (`core/tests/test_tripwire_contagem_de_pendencia.py:83`, `:147`)

## 11/11 10:43 · sessao e683c1d88894 · MAPA-20 · verificador · origem=Code

REPROVADO para pouso. Há divergência do aval, falsos verdes no validador e mutantes do publicador que escapam ao selo. (Diff: `bin/mapa20_valida.py:18,33–59`; `bin/tests/test_mapa20_valida.sh:42–50`.)

Referências: **B** = base; **D** = arquivo resultante proposto pelo diff, com numeração pós-diff; **A** = anexo `aval_mapa_ate_o_20_1111.txt:1`.

1. **RC e todos os `exit`.** No fluxo implementado, validador vermelho **não termina em rc 0**: o `else` grava `MAPA_RC=1`, preservado até as duas saídas finais. Validador verde **não causa rc 1 sem outra falha**. (D `bin/relato.sh:94–103,142,148–149`.)

   | Linha de `relato.sh` após diff | Valor do `exit` | Gatilho |
   |---|---:|---|
   | 27 | 1 | Falha de lock |
   | 28 | 1 | Destino sem `.git` |
   | 32 | 1 | Falha na lista de nomes |
   | 35 | 1 | Falha em BACKLOG-TOPO |
   | 38 | 1 | Falha em ESTADO |
   | 42 | 1 | Falha em SESSAO |
   | 86 | 1 | Falha raspando RELATO retido |
   | 101 | 1 | Falha raspando MAPA retido |
   | 117 | 1 | Falha em gerar avais |
   | 132 | 1 | Falha na raspagem geral |
   | 142 | `$MAPA_RC` | Nada mudou |
   | 148 | 1 | Falha no `git push` ou no `echo` associado | [nao verificado]
   | 149 | `$MAPA_RC` | Final após publicação |

   Fontes da tabela: B `bin/relato.sh:27–42,86`; D `bin/relato.sh:101,117,132,140–149`.

   Ressalva: `cd`, `git add` e `git commit` continuam sem tratamento obrigatório de erro; não há `set -e`. Isso permite outros falsos sucessos, mas não zera o `MAPA_RC` vermelho. (B `bin/relato.sh:21`; D `bin/relato.sh:135–149`.)

2. **Ramo retido: escrita parcial possível e isolamento incompleto.** `DEST` recebe `DEST_RELATO` ou `/home/ronald/hasner-relato`. O ramo retido passa `$DEST/MAPA-20.md` como origem e, efetivamente, como destino do raspador. (B `bin/relato.sh:23`; D `bin/relato.sh:100–101`.)

   `raspar_publico.py` lê o conteúdo inteiro, mascara e confere; depois abre o destino com `'w'`. Falha antes dessa abertura preserva o arquivo; erro/interrupção durante a escrita pode deixá-lo truncado ou parcial no checkout público. Não há temporário nem substituição atômica. Ocorrência real: **[nao verificado]**. (B `bin/raspar_publico.py:28–40`.)

   O `|| exit 1` interrompe também a publicação do restante e não desfaz eventual truncamento. Portanto, “o resto segue” não vale quando essa raspagem falha. Num ciclo posterior, a cópia parcial pode ser re-raspada e incluída pelo teste de existência, sem validação estrutural da cópia pública. (D `bin/relato.sh:99–101,139,146–148`.)

   Comparação com 2e: CODEX-ESTADO gera e raspa em temporário antes de copiar, protegendo o destino de falhas dessas duas etapas. Entretanto, a cópia final é `cp` direto: também não garante atomicidade contra falha durante a cópia. O comentário “nunca arquivo pela metade” excede essa garantia. (B `bin/relato.sh:89–97`; B `bin/codex_estado.sh:25–38`.)

   Há ainda intervalo entre validar e publicar: o validador lê a origem, e a raspagem geral a lê novamente. Sem snapshot, alteração concorrente pode publicar bytes diferentes dos validados; adesão do escritor ao lock e ocorrência real: **[nao verificado]**. (D `bin/relato.sh:95–96,118–132`; D `bin/mapa20_valida.py:65`; B `bin/raspar_publico.py:28`; B `bin/relato.sh:27`.)

3. **Argumento `origem:destino`: o par novo está correto.** O raspador usa `par.rsplit(':', 1)`: o **último** `:` separa origem de destino. Assim, `:` na origem não quebra o argumento; `:` no destino muda a interpretação. (B `bin/raspar_publico.py:23–24`.)

   Os dois pares novos respeitam esse formato. `RAIZ` vem de `pwd`, sem restrição que proíba `:`; mesmo contendo esse caractere, ele permanece na parte da origem. Caminho real de produção: **[nao verificado]**. (B `bin/relato.sh:22`; D `bin/relato.sh:96,101`; B `bin/raspar_publico.py:24`.)

4. **Validador: falsos aceites, falsas recusas e universo divergente.** Para os exemplos expressamente perguntados:

   | Caso | Comportamento |
   |---|---|
   | Obra somente citada em prosa | Recusa: não satisfaz presença |
   | Obra somente na segunda tabela, após outra tabela | Recusa: examina apenas a primeira |
   | Célula de dados contendo somente espaços | Recusa |
   | Célula `-` | Aceita |
   | Linha inteira de separador | Ignora; não substitui uma obra |

   Fontes: D `bin/mapa20_valida.py:28–40,49–59`. Portanto, prosa, segunda tabela e espaços não são todos falsos aceites.

   **Contraexemplos aceitos indevidamente**, tomando uma tabela completa para `OBRAS + SEM DONO`:

   - Uma única coluna, contendo apenas as identidades: nenhuma coluna de conteúdo é exigida. (D `bin/mapa20_valida.py:48–59`; A.)
   - Cabeçalho com uma coluna e dados com três: a largura do cabeçalho é descartada antes da comparação. (D `bin/mapa20_valida.py:40,54–56`.)
   - Linhas `|E3|||`, e equivalentes para todas as identidades: `strip('|')` remove as colunas vazias finais. (D `bin/mapa20_valida.py:33–34,51–56`.)
   - Tabela inteira dentro de bloco cercado por crases; tabela sem separador; obras duplicadas. O parser não reconhece cercas, não exige separador e não acusa duplicação. (D `bin/mapa20_valida.py:30–40,49–50`.)
   - `-` em coluna que deveria trazer medição ou `nao medido`: só se testa vazio, não a declaração exigida pelo aval. (D `bin/mapa20_valida.py:51–59`; A.)

   **Recusas indevidas:** mapa completo na segunda tabela; tabela Markdown sem pipes externos; uma célula `x\|y.py:1`, cujo pipe escapado cria coluna artificial; mapa usando `UI-PAINEL`, literalmente autorizado. (D `bin/mapa20_valida.py:18,32–40,54–59`; A.)

   Comparação nome a nome:

   | Aval | `OBRAS` |
   |---|---|
   | E3 | E3 |
   | E4B | E4B |
   | E4C | E4C |
   | E4D | E4D |
   | RAIZ F1 | RAIZ F1 |
   | RAIZ F2 | RAIZ F2 |
   | RAIZ F3 | RAIZ F3 |
   | RAIZ F4 | RAIZ F4 |
   | RAIZ F5 | RAIZ F5 |
   | RAIZ F6 | RAIZ F6 |
   | RAIZ F7 | RAIZ F7 |
   | O168 | O168 |
   | LASTRO | LASTRO |
   | O169-FECHADA | O169-FECHADA |
   | **UI-PAINEL** | **UI-PAINEL-MOSTRA-O-APP — diverge** |
   | BO-ROTULO-ENCERRADO | BO-ROTULO-ENCERRADO |
   | TRIAGEM-OFFLINE-SEM-CAMERA | TRIAGEM-OFFLINE-SEM-CAMERA |
   | LOTE-VALIDAR-REGRESSAO-0311 | LOTE-VALIDAR-REGRESSAO-0311 |

   Fontes da comparação: A; D `bin/mapa20_valida.py:14–18`. Há 18 elementos, mas contar 18 não resolve a identidade errada.

5. **O selo tem mordida parcial; não certifica as garantias anunciadas.**

   | Caso MORDE | Mutante plausível que passa |
   |---|---|
   | `falta_obra` | Trocar `E3` por `INVENTADA`, mantendo 18: a fixture segue `M.OBRAS` |
   | `sem_dono` | Conferir apenas quantidade mínima de linhas, em lugar das identidades: retirar SEM DONO reduz a quantidade, mas uma duplicata poderia substituí-lo |
   | `celula_vazia` | Conferir vazios somente em E4C |
   | `sem_tabela` | Conferir presença somente de RAIZ F5 e SEM DONO, preservando a rejeição inicial de texto sem tabela; todas as outras obras deixam de ser protegidas |

   A fixture usa o próprio universo do validador; os únicos alvos pontuais são RAIZ F5 e E4C. Os mutantes de universo, presença restrita e vazio restrito passaram os casos em memória. (D `bin/tests/test_mapa20_valida.sh:19,22–28,36`; D `bin/mapa20_valida.py:45–59`.)

   Os checks de ligação também ficaram verdes, em memória, com estes mutantes de `relato.sh`:

   - `MAPA_RC=0` no `else`;
   - `if false; then # mapa20_valida.py`;
   - `MAPA_PAR` preenchido incondicionalmente;
   - retirada de MAPA do `git add` e do pathspec do commit; [nao verificado]
   - saída final `exit 0`, conservando `exit "$MAPA_RC"` dentro de `if false`.

   Esses checks apenas procuram texto/regex; não executam o publicador nem provam o fluxo das variáveis. (D `bin/tests/test_mapa20_valida.sh:40–50`.)

   **Comentário pode satisfazer a ligação:** só comentários de linha inteira são removidos; o nome em comentário inline satisfaz `find('mapa20_valida.py')`. A regex do ramo também ignora exatamente `|| exit 1`, incluindo a saída antecipada existente na raspagem retida. (D `bin/tests/test_mapa20_valida.sh:42–49`; D `bin/relato.sh:101`.)

6. **Selos vizinhos: nenhuma quebra textual identificada pelo diff.**

   - `test_codex_estado.sh`: o recorte começa em `CODEX_OK=""`, depois do bloco novo; permanece sem `exit`. (B `bin/tests/test_codex_estado.sh:104–113`; D `bin/relato.sh:111–113`.)
   - `test_relato_publica_claude_md.sh`: o par CLAUDE, o `git add` e o pathspec permanecem reconhecíveis. Seu mutante continua retirando CLAUDE e ficando vermelho. (B `bin/tests/test_relato_publica_claude_md.sh:15–20,29–30`; D `bin/relato.sh:132,136,147`.)
   - `test_codex_estado_evento.sh` e `test_relato_afirma_com_prova.sh`: os nomes procurados permanecem. Esses `grep` também aceitam comentários; `codex_estado.sh` já aparece em comentário da base. (B `bin/tests/test_codex_estado_evento.sh:28–30`; B `bin/tests/test_relato_afirma_com_prova.sh:44`; B `bin/relato.sh:93`.)
   - `test_publicacao_sem_nome.sh`: as menções ao publicador são comentários; o universo verificado vem de `git ls-files`, incluindo MAPA quando rastreado. Resultado sobre o repo público real: **[nao verificado]**. (B `bin/tests/test_publicacao_sem_nome.sh:58–72`.)

   Verde das suítes completas: **[nao verificado]**. [nao verificado]

7. **QUEM ESCREVE e QUEM LÊ.**

   | Valor | Quem escreve | Quem lê |
   |---|---|---|
   | `MAPA_RC` | Inicialização 0; `else` grava 1 | Saídas por nada alterado e final |
   | `MAPA_PAR` | Inicialização vazia; ramo verde grava o par | Expansão na raspagem geral | [nao verificado]
   | `MAPA_PATH` | Inicialização vazia; teste de existência grava o nome | Mensagem e pathspec do commit | [nao verificado]

   Fontes: D `bin/relato.sh:94,96,98,132,139,142,146–149`.

   `logs/MAPA-20/mapa.md`: o comentário atribui regravação à sessão por evento; não há produtor executável apresentado no diff. Escritor real: **[nao verificado]**. Leitores demonstrados: validador e, depois, raspador. (D `bin/relato.sh:89–96,132`; D `bin/mapa20_valida.py:63–65`; B `bin/raspar_publico.py:28`.)

   `$DEST/MAPA-20.md`: `raspar_publico.py` escreve tanto na publicação aprovada quanto na re-raspagem retida. Leem o arquivo o teste de existência, o próprio raspador no ramo retido e o Git para staging/commit. Leitura efetiva por usuários on-line: **[nao verificado]**. (D `bin/relato.sh:96,100–101,132,139,146–148`; B `bin/raspar_publico.py:28,40`.)

8. **Classes de reprovação, uma a uma.**

   | Classe | Conferência |
   |---|---|
   | **2º juiz duplicando registrado** | Duplicação de juiz de negócio não demonstrada. A privacidade continua consultando o módulo único `nome_no_texto`. (B `bin/raspar_publico.py:15–16,31–36`.) |
   | **2º escritor fora da porta** | As duas rotas MAPA usam o mesmo raspador; a falha demonstrada é escrita direta não atômica. Exclusividade e disciplina do produtor do mapa: **[nao verificado]**. (D `bin/relato.sh:96,101,132`; B `bin/raspar_publico.py:40`.) |
   | **Relógio fora da borda** | Nenhuma nova leitura temporal no validador. O `date` da mensagem de commit já existia na base. (D `bin/mapa20_valida.py:12–77`; B `bin/relato.sh:130`; D `bin/relato.sh:146`.) |
   | **Derivação paralela de fonte única** | O universo copiado diverge do aval em UI; o teste deriva sua expectativa da própria cópia. Não há derivação numérica de negócio nesse validador. (A; D `bin/mapa20_valida.py:14–18,43–60`; D `bin/tests/test_mapa20_valida.sh:22,36`.) |
   | **Selo sem mordida** | Procedente como cobertura insuficiente: mutantes de universo, validação e publicação sobrevivem, conforme item 5. (D `bin/tests/test_mapa20_valida.sh:19–36,42–50`.) |
   | **Ordem de trava** | O bloco MAPA executa sob o lock já adquirido. A chamada CODEX `--gerar` retorna antes do lock do caminho de evento; não nasce reaquisição nesse caminho. Trava do escritor do mapa: **[nao verificado]**. (B `bin/relato.sh:27`; D `bin/relato.sh:95–112`; B `bin/codex_estado.sh:54–66`.) |
   | **Pouso antes do pré-requisito** | Contrato efetivamente publicado, suite integral verde e opus final: **[nao verificado]**. São requisitos registrados, e a menção ao contrato no comentário não os demonstra. (B `CLAUDE.md:377–381,394`; D `bin/relato.sh:93`.) |
   | **Campo com dois significados** | `MAPA_RC` representa o resultado do validador; `MAPA_PAR`, a inclusão aprovada; `MAPA_PATH`, somente existência local. Este último não comprova validação nem atualização: inclui também a cópia retida. (D `bin/relato.sh:94–101,132,139,142–149`.) |

## 11/11 10:42 · sessao b9ef413725f4 · - · - · origem=Ronald (tmux)

1. **Divergência do universo:** o aval pede `UI-PAINEL`; o validador exige `UI-PAINEL-MOSTRA-O-APP`. Os demais nomes coincidem: `E3`, `E4B`, `E4C`, `E4D`; `RAIZ F1`, `RAIZ F2`, `RAIZ F3`, `RAIZ F4`, `RAIZ F5`, `RAIZ F6`, `RAIZ F7`; `O168`, `LASTRO`, `O169-FECHADA`; `BO-ROTULO-ENCERRADO`, `TRIAGEM-OFFLINE-SEM-CAMERA`, `LOTE-VALIDAR-REGRESSAO-0311`. Uma tabela com os nomes literais do aval recebe rc 1 pela divergência de UI. (`aval_mapa_ate_o_20_1111.txt:1`, anexo; diff `bin/mapa20_valida.py:15-18,57-59`.)

2. **Os exemplos sugeridos não são todos falsos aceites:** obra somente na prosa não satisfaz presença; obra somente na segunda tabela também não; célula contendo espaços é recusada; célula `-` passa; linha inteira de separador é ignorada e não substitui obra. Reproduzido em memória: respectivos rc `1,1,1,0`; inserir separador entre linhas válidas preserva rc 0. (`diff bin/mapa20_valida.py:30-40,49-59,73`.)

3. **Falsos aceites reproduzidos, rc 0:**
   - Tabela com todas as identidades e **uma coluna apenas**; nenhuma coluna exigida pelo aval é cobrada. (`diff bin/mapa20_valida.py:48-59`; aval anexo:1.)
   - Cabeçalho com uma coluna e dados com três: somente larguras das linhas de dados são comparadas. (`diff bin/mapa20_valida.py:40,54-56`.)
   - Linhas `|E3|||`, repetindo o padrão para todas as obras e `SEM DONO`: `strip('|')` elimina as colunas vazias finais. (`diff bin/mapa20_valida.py:34,51-56`.)
   - Conteúdo completo dentro de bloco de código cercado por crases; tabela sem separador; linhas duplicadas: o parser não distingue contexto de código, não exige separador e sobrescreve a identidade no dicionário. (`diff bin/mapa20_valida.py:30-40,49-50`.)
   - `-` nas colunas de medição também passa: o validador verifica presença textual, sem conferir medição ou declaração `nao medido`. (`diff bin/mapa20_valida.py:51-59`; aval anexo:1.)

4. **Falsas recusas reproduzidas, rc 1:** mapa completo na segunda tabela após tabela alheia; tabela Markdown sem pipes externos; uma célula com pipe escapado `x\|y.py:1`, por produzir largura adicional; mapa com `UI-PAINEL` literal do aval. (`diff bin/mapa20_valida.py:18,32-40,54-59`; aval anexo:1.)

5. **Mutantes executados em memória passam os cinco casos do selo novo**, com sequência `verde=0`, `MORDE_falta_obra=1`, `MORDE_sem_dono=1`, `MORDE_celula_vazia=1`, `MORDE_sem_tabela=1`: [nao verificado]
   - Trocar `E3` por `INVENTADA` em `OBRAS`, conservando tamanho 18: o teste fabrica sua expectativa a partir da própria lista mutada. (`diff bin/tests/test_mapa20_valida.sh:22-27,36`.)
   - Conservar `OBRAS`, mas conferir presença apenas de `RAIZ F5` e `SEM DONO`: a única obra retirada pelo teste é F5. (`diff bin/tests/test_mapa20_valida.sh:25-26`.)
   - Conferir células vazias somente em `E4C`: o único vazio testado está nessa obra. (`diff bin/tests/test_mapa20_valida.sh:19,27`.)

   A verificação de arquivo ausente continua verde nesses mutantes, porque a captura de `OSError` permanece intacta. (`diff bin/mapa20_valida.py:64-68`; diff `bin/tests/test_mapa20_valida.sh:35`.)

6. **Classes relativas ao validador:** selo sem mordida é demonstrado pelos mutantes acima; fonte única do universo não é certificada pelo teste, que deriva a fixture do objeto sob teste; o próprio validador declara usar o universo literal do aval e o desrespeita em UI. (`diff bin/tests/test_mapa20_valida.sh:22-27,36`; diff `bin/mapa20_valida.py:7-8,18`.) A regra de pré-verificação exige conferir essas classes e reserva o pouso à leitura final subsequente. (`CLAUDE.md:372-381`.)

## 11/11 10:41 · sessao 75140b8e444c · - · - · origem=Ronald (tmux)

5. O selo mata remoções completas de verificações, mas usa o próprio `M.OBRAS` para gerar quase todo o oráculo; trocar `E3` por `E3-ERRADA`, mantendo 18 elementos, passa nos casos e na contagem. Os únicos alvos específicos são ausência de `RAIZ F5`, ausência de `SEM DONO`, terceira célula vazia em `E4C` e texto sem tabela. [diff:bin/tests/test_mapa20_valida.sh:19,22,25-28,36]

Comprovação somente em memória: os seguintes mutantes do publicador passam os quatro checks textuais: [nao verificado]

- `MAPA_RC=0` no ramo vermelho.
- Trocar a chamada por `if false; then # mapa20_valida.py`.
- Inicializar `MAPA_PAR` já preenchido, incondicionalmente.
- Retirar MAPA do `git add` e do pathspec de commit. [nao verificado]
- Encerrar com `exit 0`, preservando `exit "$MAPA_RC"` dentro de `if false`.

Os predicados apenas buscam posição, substring, regex de ramo e alguma linha com `exit "$MAPA_RC"`; não executam o publicador nem exigem atribuição vermelha, caminho condicionado, staging ou saída final efetiva. [diff:bin/tests/test_mapa20_valida.sh:40-50] [nao verificado]

Comentários de linha inteira são removidos, mas comentários inline permanecem: o mutante `if false; then # mapa20_valida.py` fica verde. A regex `(?<!\|\| )\bexit\b` também ignora exatamente `|| exit 1`, portanto não acusa a saída antecipada já existente na raspagem retida. [diff:bin/tests/test_mapa20_valida.sh:42,44,48-49; diff:bin/relato.sh:100] [nao verificado]

6. Nenhum check textual vizinho quebra com esse diff:

- CODEX: o recorte começa em `CODEX_OK=""`, depois do bloco novo; continua sem `exit`. [base:bin/tests/test_codex_estado.sh:104-113]
- CLAUDE: regex da raspagem, `git add` e pathspec continuam satisfeitos; o mutante que remove CLAUDE continua vermelho. [base:bin/tests/test_relato_publica_claude_md.sh:15-20,29-30; diff:bin/relato.sh:132-149]
- Evento CODEX e afirmação com prova: os nomes buscados permanecem; os `grep` aceitam comentários também. [base:bin/tests/test_codex_estado_evento.sh:28-30; base:bin/tests/test_relato_afirma_com_prova.sh:44]
- Publicação sem nome não lê o publicador: as ocorrências de `relato.sh` são comentários; o universo vem de `git ls-files`, incluindo MAPA depois de rastreado. Verde real sem dados do repo público: [não verificado]. [base:bin/tests/test_publicacao_sem_nome.sh:58-72] [nao verificado]

## 11/11 10:40 · sessao 61a4a2c2a774 · - · - · origem=Ronald (tmux)

1. **Saídas/RC:** no fluxo normal de comandos do arquivo, não há caminho em que validador falhe e relato termine em 0: `MAPA_RC` começa em 0, recebe 1 no `else` e ambos os términos finais usam esse valor; os demais términos explícitos usam 1. Validador verde também não produz RC 1 sem outra falha. [diff `bin/relato.sh`:94–103,142,148–149]

   Todos os `exit` após diff:
   - RC **1**: 27 (lock), 28 (repo), 32 (nomes), 35 (backlog), 38 (estado), 42 (sessão), 86 (raspagem RELATO retido), 101 (raspagem MAPA retido), 117 (gerar avais), 132 (raspagem geral), 148 (push ou `echo` final falhando). [base `bin/relato.sh`:27–42,86; diff `bin/relato.sh`:101,117,132,148]
   - RC **`$MAPA_RC`**: 142 (nada mudou), 149 (após publicação): 0 se verde, 1 se vermelho. [diff `bin/relato.sh`:94–98,140–149]
   
   Há falhas não propagadas em `cd`, `git add` e `git commit`, pois não há `set -e`; isso é outro problema, mas não transforma MAPA vermelho em RC 0. [base `bin/relato.sh`:21; diff `bin/relato.sh`:135–149]

2. **Retido e parcial:** variável `DEST` recebe `DEST_RELATO` ou caminho padrão; o ramo retido passa o próprio arquivo público como origem e destino. [base `bin/relato.sh`:23; diff `bin/relato.sh`:100–101] O raspador primeiro lê tudo, mascara e confere; só então abre o destino em `'w'`, sem arquivo temporário ou rename. Erro anterior a essa abertura preserva arquivo; erro de escrita/interrupção após truncamento pode deixar arquivo vazio/parcial no checkout público; ocorrência real não demonstrada. [base `bin/raspar_publico.py`:28–40; ocorrência: não verificado] O `|| exit 1` impede o commit normal daquela execução, mas não desfaz a escrita. [diff `bin/relato.sh`:101]

   CODEX-ESTADO gera e raspa em temporário, protegendo o destino contra falhas do gerador/raspador; entretanto a cópia final usa `cp` direto, que também não é substituição atômica e pode falhar durante a escrita. Portanto o comentário “nunca arquivo pela metade” é mais forte que a implementação. [base `bin/codex_estado.sh`:25–38; base `bin/relato.sh`:89–95]

3. **Formato:** usa `src, dst = par.rsplit(':', 1)`: último `:` separa origem e destino. O par novo respeita o formato; `:` na origem **não quebra**. `:` no destino altera a separação. [base `bin/raspar_publico.py`:23–24; diff `bin/relato.sh`:96,101,132] `RAIZ` vem de `cd ... && pwd`, sem regra que proíba `:`; portanto o código não garante ausência desse caractere, e a construção nova continua funcionando com ele na origem. [base `bin/relato.sh`:22; base `bin/raspar_publico.py`:24]

7. **Produtores/consumidores:**
   
   | Valor | Escreve | Lê |
   |---|---|---|
   | `MAPA_RC` | inicialização 0 e ramo vermelho 1 | dois exits finais |
   | `MAPA_PAR` | inicialização vazia e ramo verde com par | expansão condicional na raspagem geral | [nao verificado]
   | `MAPA_PATH` | vazio; recebe nome se arquivo público existe | mensagem e pathspec do commit | [nao verificado]
   
   [diff `bin/relato.sh`:94,96,98,132,139,142,146–149]

   `logs/MAPA-20/mapa.md`: o comentário atribui regravação à sessão por evento, mas não há escritor executável no diff; a existência/produção real não é verificável nesta cópia, que não contém `logs`. O validador lê via `open(...).read()` e o raspador lê novamente na publicação. [diff `bin/relato.sh`:89–96; diff `bin/mapa20_valida.py`:63–65; base `bin/raspar_publico.py`:28; produção real: não verificado]

   `MAPA-20.md` público: escreve `raspar_publico.py` tanto pelo par aprovado na raspagem geral quanto pelo par do ramo retido; lê o teste `-f`, o próprio raspador no ramo retido e o Git ao adicionar/commitar/publicar. [diff `bin/relato.sh`:96,100–101,132,139,146–148; base `bin/raspar_publico.py`:28,40]

   **Janela entre validar e publicar:** lê o mapa para validar e depois torna a lê-lo para raspar; não há snapshot. O lock do publicador não demonstra que o escritor do mapa o obedece. Assim, mudança concorrente pode publicar bytes diferentes dos validados; ocorrência real não demonstrada. [base `bin/relato.sh`:27; diff `bin/relato.sh`:95–96,118–132; diff `bin/mapa20_valida.py`:65; base `bin/raspar_publico.py`:28; concorrência real: não verificado]

## 11/11 10:36 · sessao 1ca6effd2852 · - · - · origem=Ronald (tmux)

Há 48 IDs de primeira coluna adicionais aos 18 nomes pedidos; aliases E3/E4B/UI podem ser integrados às linhas obrigatórias. A lista completa foi enviada em duas mensagens, preservando os números de `backlog_obras.txt:852` a `backlog_obras.txt:2367`.

A instrução literal também alcança obras nomeadas dentro das descrições: UI-ANEXO-AUSENCIA (`backlog_obras.txt:1131`), JUIZ-DO-PAR (`backlog_obras.txt:1732`), JUIZ-1 e CRONS-INSTALL-SOB-MARCA (`backlog_obras.txt:1761`), PLANTIO (`backlog_obras.txt:1852`), E1-FILA-V2-2-TRAVA (`backlog_obras.txt:1853`), BO-0611-05 (`backlog_obras.txt:1889`), BUG-PAR (`backlog_obras.txt:1895`), O137/ESMERIL-NO-RASTRO (`backlog_obras.txt:1899`), BO-0611-05-RESTO-DA-CLASSE (`backlog_obras.txt:1905`), O284-A (`backlog_obras.txt:1911`) e ORDEM-ATE-O-ZERO (`backlog_obras.txt:2367`). A distinção entre obra própria, alias e referência histórica precisa ser declarada; omitir silenciosamente esses nomes contraria a leitura literal. [nao verificado: critério final de identidade]

E4C/E4D e RAIZ F2–F7 existem como etapas nomeadas, sem linha própria no BACKLOG anexado; não devem ser anunciadas como ausentes do BACKLOG inteiro (`backlog_obras.txt:1878`, `backlog_obras.txt:1895`). LASTRO genérico aparece na META, mas seus registros específicos são LASTRO-DEGRAU-2 e LASTRO-PARTE-2/3 (`backlog_obras.txt:1732`, `backlog_obras.txt:1761`, `backlog_obras.txt:1763`, `backlog_obras.txt:2367`).

Todos os impactos de extras em `fio_morto_alcance` e na Central devem permanecer `nao medido`; nenhuma medida desses efeitos foi dada para eles. [nao verificado]

## 11/11 10:36 · sessao 599f583cc054 · - · - · origem=Ronald (tmux)

Censo somente leitura, na BASE `9ca563bb3`; as linhas dos arquivos alterados precisarão ser atualizadas após a cura. [nao verificado]

**Quem escreve e quem lê**

- `pendentes` é uma lista derivada, não um campo persistido: `consulta_do_app` escreve a lista com perguntas vivas sem `respondida_em` e com disputa sem `fechada_em`; conserva `historico` separado. A autoridade da vitalidade é `perguntas_do_app`, que chama `pergunta_viva`. (`chamados/services/pergunta_no_app.py:45`, `:61`, `:69`, `:71`, `:75`)
- Escritores persistentes de `respondida_em`: resposta em `chamados/services/disputa_emissao.py:1468`; reabertura em `:1593`; resposta automática em `:1653`; reconstrução pela trilha em `:1674`; ressurreição em `chamados/models.py:1566`; `carimbar` em `:3132`. A atribuição `models.py:3163` desfaz estado apenas em memória. (`chamados/models.py:3159`)
- Escritores de `DisputaSupervisao.fechada_em`: fechamento em `chamados/models.py:1932`; reaberturas em `:2149` e `:2173`.
- Leitores diretos de `consulta_do_app` em produção: dois badges `/me/` (`api/views_core.py:313`, `api/views.py:324`), listagem de chamados (`api/views.py:1004`), perguntas da disputa (`api/views.py:2542`), dashboard (`core/views.py:252`), aviso de pendência (`chamados/services/aviso_pendencia.py:8`) e cobrança (`chamados/services/cobranca.py:47`).
- `FollowupChamado.lido_colaborador_em` nasce nulo: campo aceita nulo e a criação não fornece carimbo. (`chamados/models.py:1673`, `chamados/services/nota.py:88`) A escrita de leitura passa por `marcar_lidas`, que resolve o nome em `LADOS` e faz `update` com instante. (`chamados/services/nota.py:59`, `:126`, `:132`) Chamadores do lado colaborador: `api/views.py:1050`, `chamados/views.py:1135`, `:1150`.
- Leitores de `lido_colaborador_em`: badges dos kernels (`api/views_core.py:311`, `api/views.py:322`), serializador de followups (`api/views.py:1128`), contador web (`chamados/views.py:603`) e indicação “novo” do cartão (`templates/chamados/partials/_card_atendimento.html:51`, `:59`).
- `ChamadoColaborador.status_local`: escritor de transição persistente é `_gravar_estado`, atribui e salva. (`chamados/models.py:201`, `:228`, `:296`) Nascimento também fornece status no INSERT, pela porta `criar`. (`chamados/models.py:1095`) Atribuições `:356` e `:371` simulam estado em memória. A query do badge lê `status_local__in=VIVOS`; o conjunto inclui `aberto`, `em_analise` e `registrado`. (`api/views_core.py:309`, `api/views.py:320`, `chamados/catalogo/motor.py:24`)

**Outros leitores dos rótulos**

O grep no código de produção, incluindo templates e JS, encontrou a regra duplicada nos dois kernels e uma terceira regra manual no dashboard, além de usos textuais de “Chamados” sem regra de badge. (`api/views_core.py:316`, `:318`, `api/views.py:327`, `:329`, `templates/core/dashboard_colaborador_v2.html:80`)

A linha de fila deve apontar `templates/core/dashboard_colaborador_v2.html:80`: escolhe “Responder DP”/“Chamados” pelo número de perguntas, sem ramo vermelho; o número vem de `core/views.py:253`. Outros usos são rótulo de voltar (`templates/chamados/form_abertura.html:7`), cabeçalhos de relatório (`relatorios/views.py:1672`, `inteligencia/views.py:255`) e chave de comando (`ponto/management/commands/zerar_dados_operacionais.py:52`). O grep em `static` por ambos os literais não encontrou ocorrências. [nao verificado]

**Leitores `api/views.py:1004` e `:2542`**

- `api_chamados_v2`: consulta perguntas e histórico, indexa as perguntas por chamado vivo/destino e serializa a conversa. Não calcula badge. Seu GET marca followups do DP como lidos, portanto não serve para comparar o contador anterior. (`api/views.py:1003`, `:1040`, `:1044`, `:1050`)
- `api_disputa_perguntas`: publica pendentes, histórico e o subconjunto `respondidas`; mantém `em_disputa`, IDs e opções servidas. Não calcula badge. (`api/views.py:2531`, `:2543`, `:2571`, `:2573`, `:2581`, `:2582`) Ambos devem continuar usando `consulta_do_app`; nenhuma mudança nesses leitores é necessária. (`chamados/services/pergunta_no_app.py:61`)

**Contrato adicional a tratar**

O teste de contagem registra os dois sítios inline dos badges como exceções `MENSAGEM`. Ao mover a query, o contrato exige retirar os dois sítios antigos, declarar o sítio da projeção e reduzir o teto de acordo; sem isso haverá assertions por sítio novo e declarações obsoletas. (`core/tests/test_tripwire_contagem_de_pendencia.py:83`, `:85`, `:383`, `:390`, `:395`) Não editei arquivos nem executei banco. [nao verificado]

## 11/11 10:36 · sessao a41024c8adde · - · - · origem=Ronald (tmux)

- **Causas a–e:** E4B é a dona escrita de todas as cinco classes; não é necessário inventar dono por semelhança de código. O BACKLOG anexo diz literalmente “backfill do fio morto (matriz_e4 C3, classes a-e)”; a cópia confirma o escopo. Isso prova responsabilidade, não redução do baseline nem cobertura eficaz medida. (`backlog_obras.txt:1887`; `app/docs/BACKLOG.md:1875`.)
- **Significados:** a = chamado vivo fora da fila; b = resposta carimbada sem texto; c = respondida com destino terminal; d = colaborador na lista CXR e dia fora; e = restante discriminado. O subtipo de e `pergunta_viva_sob_chamado_terminal` tem ramo próprio. (`app/chamados/services/vigia_fio.py:519`; `app/chamados/services/vigia_fio.py:538`; `app/chamados/services/vigia_fio.py:667`; `app/chamados/services/vigia_fio.py:688`.)
- **SEM DONO:** os dados permitem dizer que nenhuma causa a–e ficou sem obra dona explícita, porque E4B nomeia todas. Não permitem dar como zero o resíduo depois das obras; redução efetiva não medida continua “nao medido”. A coluna de tamanho solicitada deve permanecer literalmente “a medir pela sessao”. (`backlog_obras.txt:1887`; impacto pós-obras: [nao verificado].)

Para as linhas RAIZ, a sequência escrita é F0→F1→F2=BO-DISPUTA→F3→F4=BUG-PAR→F5 com E4d→F6→F7. Essa ordem não define por si o inventário de arquivos de cada fase. (`backlog_obras.txt:1895`; `app/docs/BACKLOG.md:1883`.)

- **RAIZ F1:** sítios de destino em `app/chamados/models.py:2982`, `:3014`; `app/chamados/services/fio.py:925`; eleitores em `app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/services/declaracao_texto.py:377`. F1 aparece respondida no anexo recente, precedendo c22 depois de E3/E4B; a lei antiga ainda aberta no RELATO da cópia é anterior e não deve prevalecer. (`backlog_obras.txt:1875`; `app/docs/RELATO.md:19`.)
- **RAIZ F2:** equivale a BO-DISPUTA e utiliza a cópia c21/c22 preservada: `app/chamados/management/commands/auditar_invariantes_chamados.py`, `app/chamados/services/declaracao_texto.py`, `app/chamados/services/descarte_disputa.py`, `app/chamados/services/disputa_emissao.py`, `app/core/observ.py`, `app/ponto/management/commands/reconciliar_grade.py`. (`arquivos_obras.txt`, bloco c21/c22: [nao verificado quanto à numeração do anexo]; `backlog_obras.txt:1875`; `backlog_obras.txt:1895`.)
- **RAIZ F3/F4:** reutilizar o mesmo censo, sem alegar divisão exaustiva entre as duas fases. Sítios centrais: `app/chamados/models.py:1853`, `:3103`; `app/chamados/services/materializacao.py:425`, `:875`, `:1158`; `app/chamados/services/validacao.py:134`, `:199`, `:461`; `app/chamados/services/disputa_emissao.py:1508`, `:1632`; `app/chamados/reconciliador.py:411`, `:611`, `:1170`; `app/chamados/services/becos.py:56`; `app/chamados/signals.py:238`; `app/chamados/services/explicador.py:137`; `app/chamados/relatorios_disputa.py:20`. Essas referências são as do `f3f4_censo.r` fornecido, não novo levantamento. (`f3f4_censo.r`: [nao verificado quanto à numeração do anexo].)
- **RAIZ F4 especificamente:** par da pausa pela célula, perda em `app/chamados/models.py:3441`, fonte `PARES_PAUSA_DNA`; não atribuir D8 como bloqueio de F3/F4. (`backlog_obras.txt:1896`; `leis_abertas.txt`, RELATO:31.)
- **RAIZ F5:** coordenada com E4d; o inventário fechado próprio dessa fase não foi fornecido. Pode citar a dependência da parada da projeção e preservar as seis exceções, sem inventar lista exaustiva de alterações. (`backlog_obras.txt:1895`; `app/docs/PROMPTS.md:2030`; `app/docs/PROMPTS.md:2034`.)
- **RAIZ F6/F7:** há nome e posição na sequência; escopo, arquivos, voltas e efeito visível específicos = “nao medido”. Não converter os lotes sugeridos do censo em fases oficiais. (`backlog_obras.txt:1895`; `app/docs/BACKLOG.md:1883`.)
- **O168:** `app/ponto/supra_juiz.py:539` e cartório do evento; retirar `--executar` depende da guarda funcionar no evento e de `CSF_RETRATADO` medido em zero. Nada disso é medida atual de fio_morto_alcance. (`backlog_obras.txt:1396`; `app/docs/BACKLOG.md:1395`; `app/ponto/services/cartorio.py:1091`.)
- **LASTRO:** o nome abreviado agrega obras distintas; não declarar uma única etapa para todo o agregado sem ressalva. Degrau 2 toca `app/ponto/services/lastro.py:274` e depende do juiz do par; parte 2 depende de JUIZ-1 e CRONS-INSTALL; parte 3 já pousou e toca emissão/reação. (`backlog_obras.txt:1732`; `backlog_obras.txt:1761`; `backlog_obras.txt:1763`.)

**C — cautela:** compartilhamento de arquivo prova cruzamento e necessidade de integração, não desmente sozinho uma ordem. O censo F3/F4 pede coordenação das testemunhas com E4c e parada com E4d, mas não fornece número que prove uma inversão obrigatória. (`f3f4_censo.r`, “Ordem sugerida”, item 5: [nao verificado quanto à numeração do anexo]; `app/docs/BACKLOG.md:1883`.)

A declaração META de O169-FECHADA “4 arquivos, 0 comuns com E3/E4B” diverge da contagem bruta da cópia anexa, que lista cinco arquivos, dos quais dois de produção; a ausência de arquivos comuns com E3/E4B continua confirmada pelo inventário fornecido. Isso é divergência de contagem, não premissa de ordem desmentida. (`meta18_linha.txt:1`; `arquivos_obras.txt`, blocos E3/E4B/O169-FECHADA: [nao verificado quanto à numeração do anexo].)

## 11/11 10:35 · sessao 5b642dda3528 · - · - · origem=Ronald (tmux)

Furos concretos para D:

- **QUEM CHAMA:** o caminho `logs/MAPA-20/mapa.md → MAPA-20.md` ainda não entra na raspagem, no staging nem no commit; acrescentar apenas o par não publica o arquivo. (`bin/relato.sh:103`, `bin/relato.sh:117`, `bin/relato.sh:121`, `bin/relato.sh:130`, `bin/relato.sh:131`)
- **QUEM ESPERA / LIMITE:** o publicador espera o lock por até 600 s e depende da obtenção da lista de nomes e dos geradores; o contrato não declara essas dependências nem como a atualização por evento trata falha/timeout. (`bin/relato.sh:27`, `bin/relato.sh:32`, `bin/relato.sh:35`, `bin/relato.sh:38`, `bin/relato.sh:42`, `bin/relato.sh:102`)
- **FALHA:** origem ausente é pulada com sucesso; não existe nesses dois programas validação de obras META-18 ou de células vazias. O `rc 1` prometido exige validação adicional antes da cópia. (`bin/raspar_publico.py:25`, `bin/raspar_publico.py:27`, `bin/raspar_publico.py:40`, `bin/raspar_publico.py:46`)
- **MEDIDA:** os programas contam substituições de nomes/CPF, não obras nem cobertura de `fio_morto_alcance`/Central; o contrato não define identificação única, tratamento das obras extras ou deduplicação de impactos. Implementação dessa conferência: [nao verificado]. (`bin/raspar_publico.py:41`, `bin/raspar_publico.py:45`)
- **FALHA DE PUBLICAÇÃO:** a atualização da linha local não prova publicação; o push pode falhar e não há retry por evento declarado no contrato. (`bin/relato.sh:132`)
- **QUEM DESLIGA:** falta definir a operação de desligamento e o destino da página já publicada; o script não possui condição de 20/20 e o próprio precedente documenta remoção do par **e** remoção do arquivo público. (`bin/relato.sh:101`, `bin/relato.sh:103`, `bin/relato.sh:132`)

Não executei os publicadores; execução, chamadas externas, estado público e comportamento da esteira em produção: [nao verificado]. [nao verificado]

## 11/11 10:34 · sessao 8051d24e8124 · MAPA-ATE-O-20 · sonda · origem=Code

```text
sonda_leitura: sonda_central.py [3912ec00c599cf5c] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

central_qs_na_fila n 3634
por_status_local 'aberto' n 2026
por_status_local 'em_analise' n 1608
por_categoria 'Ausencia' n 2710
por_categoria 'disputa_supervisao' n 323
por_categoria 'turno_aberto' n 171
por_categoria 'auditoria_clt' n 128
por_categoria 'Ponto' n 97
por_categoria 'sem_previsao_escala' n 63
por_categoria 'Outros' n 50
por_categoria 'batida_espuria' n 25
por_categoria 'turno_aberto_24h' n 18
por_categoria 'ausencia' n 14
por_categoria 'cadastro' n 12
por_categoria 'Escala' n 5
por_categoria 'App' n 3
por_categoria 'Documentos' n 3
por_categoria 'Ferias' n 3
por_categoria 'acesso_bloqueado' n 3
por_categoria 'Holerite' n 2
por_categoria 'agenda_aceite' n 2
por_categoria 'batida_fora_horario' n 1
por_categoria 'geofence' n 1
por_urgencia 'normal' n 1861
por_urgencia 'urgente' n 1295
por_urgencia 'emergencia' n 322
por_urgencia 'critica' n 140
por_urgencia 'consulta' n 16
agora 2026-11-11 10:34:04
```

_3800 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
